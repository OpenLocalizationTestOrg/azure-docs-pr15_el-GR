<properties
   pageTitle="Ομαδοποίηση κατά επιλογές στο αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Συμβουλές για την υλοποίηση των ομάδα επιλογών σε αποθήκη δεδομένων του SQL Azure για την ανάπτυξη λύσεων."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="group-by-options-in-sql-data-warehouse"></a>Ομαδοποίηση κατά επιλογές στο αποθήκη δεδομένων του SQL

Ο όρος [GROUP BY][] χρησιμοποιείται για συγκέντρωση δεδομένων σε ένα σύνολο γραμμών σύνοψης. Έχει επίσης μερικές επιλογές που επέκταση του λειτουργικότητα που χρειάζεστε για να εργαστείτε γύρω από, καθώς δεν υποστηρίζεται απευθείας από αποθήκη δεδομένων SQL Azure.

Αυτές οι επιλογές είναι
- ΟΜΑΔΟΠΟΊΗΣΗ κατά με ROLLUP
- ΟΜΑΔΟΠΟΊΗΣΗ ΣΥΝΌΛΩΝ
- ΟΜΑΔΟΠΟΊΗΣΗ κατά με ΚΎΒΟΥ

## <a name="rollup-and-grouping-sets-options"></a>Επιλογές σύνολα συνάθροισης και ομαδοποίησης
Η επιλογή απλούστερος εδώ είναι να χρησιμοποιήσετε `UNION ALL` αντί για αυτό για την εκτέλεση της συνάθροισης αντί να βασίζεστε σχετικά με τη σύνταξη ρητή. Το αποτέλεσμα είναι ακριβώς ίδια

Ακολουθεί ένα παράδειγμα μιας ομάδας, πρόταση χρησιμοποιώντας το `ROLLUP` επιλογή:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Με τη χρήση ΣΥΝΆΘΡΟΙΣΗΣ θα σας έχουν ζητήσει το παρακάτω συγκεντρώσεις:
- Χώρας και περιοχής
- Χώρα
- Γενικό σύνολο

Για να αντικαταστήσετε αυτό θα πρέπει να χρησιμοποιήσετε `UNION ALL`; που καθορίζει την συναθροίσεις υποχρεωτικά ρητά για να λάβετε τα ίδια αποτελέσματα:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Για ΟΜΑΔΟΠΟΊΗΣΗ ΣΎΝΟΛΑ όλα πρέπει να κάνετε είναι υιοθετήσουν το ίδιο κεφάλαιο αλλά μόνο δημιουργία ΈΝΩΣΗΣ ΌΛΩΝ των ενοτήτων για τα επίπεδα συγκέντρωσης θέλουμε να δούμε

## <a name="cube-options"></a>Επιλογές κύβου
Είναι δυνατή η δημιουργία ΟΜΆΔΑΣ από με ΚΎΒΟ χρησιμοποιώντας την προσέγγιση UNION ALL. Το σφάλμα είναι ότι ο κώδικας μπορεί να γίνει γρήγορα αργή και δύσχρηστες. Για τον περιορισμό αυτό, μπορείτε να χρησιμοποιήσετε αυτήν την πιο εξελιγμένη προσέγγιση.

Ας χρησιμοποιήσουμε το παραπάνω παράδειγμα.

Το πρώτο βήμα είναι να ορίσετε 'κύβο' που καθορίζει όλα τα επίπεδα συγκέντρωσης που θέλετε να δημιουργήσετε. Είναι σημαντικό να λάβετε υπόψη το CROSS ΣΥΜΜΕΤΟΧΉ από τους δύο πίνακες που προέρχονται. Αυτό δημιουργεί όλα τα επίπεδα για μας. Το υπόλοιπο του κώδικα υπάρχει πραγματικά για τη μορφοποίηση.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Τα αποτελέσματα της το CTAS μπορούν να προβληθούν παρακάτω:

![][1]

Το δεύτερο βήμα είναι να καθορίσετε έναν πίνακα προορισμού για να αποθηκεύσετε προσωρινά αποτελέσματα:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Το τρίτο βήμα είναι να επανάληψη μέσω μας κύβου στηλών εκτέλεση της συνάθροισης. Το ερώτημα θα εκτελέσει μία φορά για κάθε γραμμή του πίνακα προσωρινό #Cube και να αποθηκεύσετε τα αποτελέσματα του πίνακα temp #Results

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Τέλος, θα σας μπορεί να επιστρέψει τα αποτελέσματα διαβάζοντας το απλώς από τον προσωρινό πίνακα #Results

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Από διαιρέσεις τον κώδικα σε ενότητες και δημιουργία ενός βρόχου δομή του κώδικα γίνεται πιο διαχειρίσιμα και συντηρούνται.


## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες συμβουλές ανάπτυξης, ανατρέξτε στο θέμα [Επισκόπηση ανάπτυξης][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[Επισκόπηση ανάπτυξης]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[ΟΜΑΔΟΠΟΊΗΣΗ ΚΑΤΆ]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
