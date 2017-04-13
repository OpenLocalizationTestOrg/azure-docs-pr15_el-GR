<properties
   pageTitle="Τύποι δεδομένων για πίνακες στο αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Γρήγορα αποτελέσματα με τους τύπους δεδομένων για πίνακες αποθήκη δεδομένων του SQL Azure."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="data-types-for-tables-in-sql-data-warehouse"></a>Τύποι δεδομένων για πίνακες στο αποθήκη δεδομένων του SQL

> [AZURE.SELECTOR]
- [Επισκόπηση][]
- [Τύποι δεδομένων][]
- [Διανομή][]
- [Ευρετήριο][]
- [Partition][]
- [Στατιστικά στοιχεία][]
- [Προσωρινό][]

Αποθήκη δεδομένων του SQL υποστηρίζει τα πιο συχνά χρησιμοποιούμενα τύπους δεδομένων.  Ακολουθεί μια λίστα με τους τύπους δεδομένων που υποστηρίζονται από το αποθήκη δεδομένων του SQL.  Για πρόσθετες πληροφορίες σχετικά με τον τύπο δεδομένων υποστήριξης, ανατρέξτε στο θέμα [Δημιουργία πίνακα][].

|**Υποστηριζόμενοι τύποι δεδομένων**|||
|---|---|---|
[bigint][]|[δεκαδικών ψηφίων][]|[smallint][]|
[δυαδικό][]|[Αιώρηση][]|[smallmoney][]|
[bit][]|[Int][]|[όνομα συστήματος][]|
[CHAR][]|[χρήματα][]|[ώρα][]|
[ημερομηνία][]|[nchar][]|[tinyint][]|
[ημερομηνίας και ώρας][]|[nvarchar][]|[UniqueIdentifier][]|
[datetime2][]|[πραγματικό][]|[varbinary][]|
[datetimeoffset][]|[smalldatetime][]|[varchar][]|


## <a name="data-type-best-practices"></a>Βέλτιστες πρακτικές τύπου δεδομένων

 Κατά τον καθορισμό του τύπους στηλών, χρησιμοποιώντας τον μικρότερο τύπο δεδομένων που θα υποστηρίζει τα δεδομένα σας θα βελτιώσει τις επιδόσεις ερωτημάτων. Αυτό είναι ιδιαίτερα σημαντικό για στήλες CHAR και VARCHAR. Εάν η μεγαλύτερη τιμή σε μια στήλη είναι 25 χαρακτήρων, στη συνέχεια, ορίστε τη στήλη ως VARCHAR(25). Αποφύγετε τον ορισμό όλες τις στήλες χαρακτήρα σε μια μεγάλη προεπιλεγμένο μήκος. Επίσης, να ορίσετε στήλες ως VARCHAR όταν αυτό είναι το μόνο που είναι απαραίτητη, αντί να χρησιμοποιήσετε [NVARCHAR][].  Χρησιμοποιήστε NVARCHAR(4000) ή VARCHAR(8000) όταν είναι δυνατό αντί για NVARCHAR(MAX) ή VARCHAR(MAX).

## <a name="polybase-limitation"></a>Περιορισμός Polybase

Εάν χρησιμοποιείτε Polybase για να φορτώσετε τους πίνακές σας, ορίστε τους πίνακές σας, έτσι ώστε το μέγεθος μέγιστη δυνατή γραμμής, περιλαμβανομένου του πλήρους μήκους στήλες μεταβλητού μήκους, δεν υπερβαίνει τα 32.767 byte.  Ενώ μπορείτε να ορίσετε μια γραμμή με δεδομένα μεταβλητού μήκους που μπορεί να υπερβαίνει το πλάτος και φόρτωση γραμμών με BCP, δεν θα μπορείτε να χρησιμοποιήσετε Polybase για τη φόρτωση αυτών των δεδομένων.  Σύντομα θα προστεθούν Polybase υποστήριξης για μεγάλη γραμμές.

## <a name="unsupported-data-types"></a>Μη υποστηριζόμενους τύπους δεδομένων

Εάν κάνετε μετεγκατάσταση βάσης δεδομένων από μια άλλη πλατφόρμα SQL ως βάση δεδομένων SQL Azure, καθώς κάνετε μετεγκατάσταση, που μπορεί να αντιμετωπίσετε ορισμένοι τύποι δεδομένων που δεν υποστηρίζονται σε αποθήκη δεδομένων του SQL.  Παρακάτω υπάρχουν μη υποστηριζόμενους τύπους δεδομένων, καθώς και μερικές εναλλακτικές λύσεις που μπορείτε να χρησιμοποιήσετε στη θέση της μη υποστηριζόμενους τύπους δεδομένων.

|Τύπος δεδομένων|Λύση:|
|---|---|
|[Γεωμετρική][]|[varbinary][]|
|[Γεωγραφία][]|[varbinary][]|
|[hierarchyid][]|[nvarchar][] (4000)|
|[εικόνα][ntext,text,image]|[varbinary][]|
|[κείμενο][ntext,text,image]|[varchar][]|
|[ntext][ntext,text,image]|[nvarchar][]|
|[SQL_VARIANT][]|Διαίρεση στήλης σε πολλές στήλες ισχυρό τύπο.|
|[Πίνακας][]|Μετατροπή σε προσωρινό πίνακες.|
|[χρονική σήμανση][]|Τροποποιήστε κώδικα που θα χρησιμοποιήσετε [datetime2][] και `CURRENT_TIMESTAMP` συνάρτηση.  Υποστηρίζονται μόνο οι σταθερές ως προεπιλογές, επομένως current_timestamp δεν μπορούν να οριστούν ως έναν προεπιλεγμένο περιορισμό. Εάν χρειάζεστε για τη μετεγκατάσταση των τιμών της έκδοσης γραμμής από μια στήλη πληκτρολογήσατε χρονικής σήμανσης, στη συνέχεια, χρησιμοποιήστε [ΔΥΑΔΙΚΌ][](8) ή [VARBINARY][ΔΥΑΔΙΚΌ](8) για δεν τιμή NULL ή NULL γραμμής έκδοση τιμές.|
|[XML][]|[varchar][]|
|[τύποι που ορίζονται από το χρήστη][]|μετατρέψετε ξανά τους εγγενείς τύπους όπου είναι δυνατό|
|προεπιλεγμένες τιμές|προεπιλεγμένες τιμές υποστηρίζει λεκτικές σταθερές και μόνο σταθερές.  Μη ντετερμινιστική παραστάσεις ή συναρτήσεις, όπως `GETDATE()` ή `CURRENT_TIMESTAMP`, δεν υποστηρίζονται.|

Το παρακάτω SQL μπορεί να εκτελεστεί σε την τρέχουσα βάση δεδομένων SQL για τον προσδιορισμό των στηλών που δεν υποστηρίζονται από αποθήκη δεδομένων SQL Azure:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στα άρθρα σε [Πίνακα Επισκόπηση][Επισκόπηση], [διανομή πίνακα][κατανομή], [δημιουργίας ευρετηρίου για έναν πίνακα][ευρετήριο], [διαμερισμάτων πίνακα][διαμερισμάτων], [Λαμβάνοντας υπόψη στατιστικά στοιχεία πίνακα][Στατιστικά στοιχεία] και [Προσωρινό πίνακες][προσωρινό].  Για περισσότερες πληροφορίες σχετικά με τις βέλτιστες πρακτικές, ανατρέξτε στο θέμα [Βέλτιστες πρακτικές αποθήκη δεδομένων SQL][].

<!--Image references-->

<!--Article references-->
[Επισκόπηση]: ./sql-data-warehouse-tables-overview.md
[Τύποι δεδομένων]: ./sql-data-warehouse-tables-data-types.md
[Διανομή]: ./sql-data-warehouse-tables-distribute.md
[Ευρετήριο]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Στατιστικά στοιχεία]: ./sql-data-warehouse-tables-statistics.md
[Προσωρινό]: ./sql-data-warehouse-tables-temporary.md
[Βέλτιστες πρακτικές αποθήκη δεδομένων SQL]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[Δημιουργία πίνακα]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[δυαδικό]: https://msdn.microsoft.com/library/ms188362.aspx
[bit]: https://msdn.microsoft.com/library/ms177603.aspx
[CHAR]: https://msdn.microsoft.com/library/ms176089.aspx
[ημερομηνία]: https://msdn.microsoft.com/library/bb630352.aspx
[ημερομηνίας και ώρας]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[δεκαδικών ψηφίων]: https://msdn.microsoft.com/library/ms187746.aspx
[Αιώρηση]: https://msdn.microsoft.com/library/ms173773.aspx
[Γεωμετρική]: https://msdn.microsoft.com/library/cc280487.aspx
[Γεωγραφία]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[Int]: https://msdn.microsoft.com/library/ms187745.aspx
[χρήματα]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[πραγματικό]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[SQL_VARIANT]: https://msdn.microsoft.com/library/ms173829.aspx
[όνομα συστήματος]: https://msdn.microsoft.com/library/ms186939.aspx
[Πίνακας]: https://msdn.microsoft.com/library/ms175010.aspx
[ώρα]: https://msdn.microsoft.com/library/bb677243.aspx
[χρονική σήμανση]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[UniqueIdentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[XML]: https://msdn.microsoft.com/library/ms187339.aspx
[τύποι που ορίζονται από το χρήστη]: https://msdn.microsoft.com/library/ms131694.aspx
