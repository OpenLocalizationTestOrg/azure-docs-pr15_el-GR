<properties
    pageTitle="SQL στη μνήμη, ξεκινήστε | Microsoft Azure"
    description="Τεχνολογίες SQL στη μνήμη βελτιώσετε σημαντικά την απόδοση των συναλλαγών και ανάλυση φόρτους εργασίας. Μάθετε πώς μπορείτε να επωφεληθείτε από αυτές τις τεχνολογίες."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jodebrui"/>


# <a name="get-started-with-in-memory-preview-in-sql-database"></a>Γρήγορα αποτελέσματα με το στη μνήμη (έκδοση Preview) σε βάση δεδομένων SQL

Δυνατότητες στη μνήμη βελτιώσετε σημαντικά την απόδοση των συναλλαγών και ανάλυση φόρτους εργασίας στις περιπτώσεις δεξιά.

Αυτό το θέμα δίνει έμφαση δύο επιδείξεις, μία για Συναλλαγής στη μνήμη και μία για ανάλυσης που βρίσκεται στη μνήμη. Κάθε επίδειξη παρέχεται μαζί με τα βήματα και κώδικα που χρειάζεστε για να εκτελέσετε την επίδειξη. Μπορείτε να κάνετε:

- Χρήση του κωδικού για να ελέγξετε παραλλαγές για να δείτε τις διαφορές στις επιδόσεις αποτελέσματα. ή
- Διαβάστε τον κωδικό για να κατανοήσετε το σενάριο, καθώς και για να δείτε πώς μπορείτε να δημιουργήσετε και να χρησιμοποιούν τα αντικείμενα στη μνήμη.

> [AZURE.VIDEO azure-sql-database-in-memory-technologies]

- [1 γρήγορη έναρξη: τεχνολογίες Συναλλαγής στη μνήμη για ταχύτερη απόδοση T-SQL](http://msdn.microsoft.com/library/mt694156.aspx) -είναι ένα άλλο άρθρο για να σας βοηθήσει να ξεκινήσετε.

#### <a name="in-memory-oltp"></a>Συναλλαγής στη μνήμη

Οι δυνατότητες του στη μνήμη [Συναλλαγής](#install_oltp_manuallink) (επεξεργασία ηλεκτρονικών συναλλαγών) είναι:

- Βελτιστοποίηση μνήμης πίνακες.
- Η μεταγλώττιση εγγενώς αποθηκευμένες διαδικασίες.


Ένας πίνακας Βελτιστοποίηση μνήμης έχει μία αναπαράσταση του εαυτού της στο ενεργό μνήμη, εκτός από την τυπική αναπαράσταση σε μια μονάδα σκληρού δίσκου. Επιχειρηματικές συναλλαγές σε σχέση με τον πίνακα εκτελείται πιο γρήγορα, επειδή αλληλεπιδρούν απευθείας με μόνο η απεικόνιση που βρίσκεται στη μνήμη ενεργό.

Με Συναλλαγής στη μνήμη, μπορείτε να επιτύχετε προς τα επάνω για να αποκτήσετε 30 ώρες στο μετάδοσης κίνησης, ανάλογα με τις λεπτομέρειες του φόρτου εργασίας.


Εγγενώς μεταγλωττισμένο αποθηκευμένες διαδικασίες απαιτούν λιγότερες μηχανικής οδηγίες κατά το χρόνο εκτέλεσης από παραδοσιακά ερμηνεύεται αποθηκευμένες διαδικασίες. Θα σας δει αποτέλεσμα εγγενούς μεταγλώττισης διάρκειες που είναι 1/100 της ερμηνευθεί διάρκειας.


#### <a name="in-memory-analytics"></a>Ανάλυσης που βρίσκεται στη μνήμη 

Η δυνατότητα των στη μνήμη [ανάλυσης](#install_analytics_manuallink) είναι:

Ευρετήρια Columnstore βελτιώσετε την απόδοση της αναφοράς ερωτήματα και ανάλυση. 


#### <a name="real-time-analytics"></a>Ανάλυση σε πραγματικό χρόνο

Για [Ανάλυση σε πραγματικό χρόνο](http://msdn.microsoft.com/library/dn817827.aspx) μπορείτε να συνδυάσετε Συναλλαγής στη μνήμη και τις αναλύσεις για να:

- Βάση δεδομένων λειτουργίας σε πραγματικό χρόνο επιχειρησιακή διαφάνεια.


#### <a name="availability"></a>Διαθεσιμότητα


GA, γενικής διαθεσιμότητας:

- [Ευρετήρια Columnstore](http://msdn.microsoft.com/library/dn817827.aspx) που υπάρχουν *στο δίσκο*.


Προεπισκόπηση:

- Συναλλαγής στη μνήμη
- Σε πραγματικό χρόνο λειτουργικές ανάλυσης


Θέματα ενώ οι δυνατότητες στη μνήμη είναι στην προεπισκόπηση έχουν περιγράφεται [παρακάτω σε αυτό το θέμα](#preview_considerations_for_in_memory).


> [AZURE.NOTE] Αυτές οι δυνατότητες preview είναι διαθέσιμη μόνο για βάσεις δεδομένων [*Premium*](sql-database-service-tiers.md) δεν στην ελαστικότητας χώρους συγκέντρωσης και δεν είναι διαθέσιμη για όλες τις βάσεις δεδομένων Basic ή τυπική.  Υποστήριξη για βάσεις δεδομένων Premium στην ελαστικότητας χώρους συγκέντρωσης σύντομα διαθέσιμο. 


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="a-install-the-in-memory-oltp-sample"></a>ΑΠΑΝΤΉΣΕΙΣ. Εγκατάσταση του δείγματος Συναλλαγής στη μνήμη

Μπορείτε να δημιουργήσετε το δείγμα βάσης δεδομένων AdventureWorksLT [V12] με λίγα μόνο κλικ στην [πύλη του Azure](https://portal.azure.com/). Στη συνέχεια, τα βήματα αυτής της ενότητας εξηγούν πώς μπορείτε να εμπλουτίσετε τη βάση δεδομένων AdventureWorksLT με:

- Πίνακες στη μνήμη.
- Οι μεταγλωττισμένες εγγενώς αποθηκευμένη διαδικασία.


#### <a name="installation-steps"></a>Βήματα εγκατάστασης

1. Στην [πύλη του Azure](https://portal.azure.com/), δημιουργία Premium βάσης δεδομένων σε ένα διακομιστή V12. Ορισμός της **προέλευσης** το δείγμα βάσης δεδομένων AdventureWorksLT [V12].
 - Για λεπτομερείς οδηγίες, μπορείτε να δείτε [Δημιουργία του πρώτου βάση δεδομένων Azure SQL](sql-database-get-started.md).

2. Σύνδεση με τη βάση δεδομένων με SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Αντιγράψτε τη [δέσμη ενεργειών στη μνήμη Συναλλαγής Transact-SQL](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) στο Πρόχειρο.
 - Δέσμη ενεργειών T SQL που δημιουργεί τα αντικείμενα είναι απαραίτητο στη μνήμη AdventureWorksLT δείγμα βάσης δεδομένων που δημιουργήσατε στο βήμα 1.

4. Επικολλήστε τη δέσμη ενεργειών T-SQL σε SSMS και, στη συνέχεια, εκτελέστε τη δέσμη ενεργειών.
 - Είναι κρίσιμης σημασίας το `MEMORY_OPTIMIZED = ON` όρος καταστάσεις ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ, ως σε:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Σφάλμα 40536


Εάν λάβετε σφάλμα 40536 κατά την εκτέλεση της δέσμης ενεργειών T SQL, εκτελέστε την ακόλουθη δέσμη ενεργειών T-SQL για να επιβεβαιώσετε αν η βάση δεδομένων υποστηρίζει στη μνήμη:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Αποτέλεσμα **0** σημαίνει ότι δεν υποστηρίζεται στη μνήμη και 1 σημαίνει ότι υποστηρίζεται. Για να εντοπίσετε το πρόβλημα:

- Βεβαιωθείτε ότι η βάση δεδομένων δημιουργήθηκε μετά τις δυνατότητες Συναλλαγής στη μνήμη γίνεται ενεργό για προεπισκόπηση.
- Βεβαιωθείτε ότι η βάση δεδομένων βρίσκεται στο το επίπεδο υπηρεσιών Premium.


#### <a name="about-the-created-memory-optimized-items"></a>Σχετικά με τα στοιχεία που έχουν δημιουργηθεί Βελτιστοποίηση μνήμης

**Πίνακες**: το δείγμα περιέχει Βελτιστοποίηση μνήμης στους πίνακες που ακολουθούν:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Μπορείτε να ελέγξετε Βελτιστοποίηση μνήμης πίνακες μέσω της **Εξερεύνησης αντικειμένου** στο SSMS από:

- Κάντε δεξί κλικ σε **πίνακες** > **φίλτρο** > **Ρυθμίσεις φίλτρου** > **Βελτιστοποίηση μνήμης** είναι ίσο με 1.


Ή μπορείτε να υποβάλετε ερώτημα στις προβολές του καταλόγου όπως:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Οι μεταγλωττισμένες εγγενώς αποθηκευμένη διαδικασία**: SalesLT.usp_InsertSalesOrder_inmem μπορεί να ελεγχθεί μέσω ενός ερωτήματος στον κατάλογο προβολή:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

## <a name="run-the-sample-oltp-workload"></a>Εκτελέστε το φόρτο εργασίας Συναλλαγής δείγματος

Η μόνη διαφορά μεταξύ του παρακάτω δύο *αποθηκευμένες διαδικασίες* είναι ότι η πρώτη διαδικασία χρησιμοποιεί Βελτιστοποίηση μνήμης εκδόσεις των πινάκων, ενώ η δεύτερη διαδικασία χρησιμοποιεί την κανονική πίνακες στο δίσκο:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


Σε αυτήν την ενότητα, μπορείτε να δείτε πώς μπορείτε να χρησιμοποιήσετε το βοηθητικό πρόγραμμα εύχρηστο **ostress.exe** να εκτελέσει τις δύο αποθηκευμένες διαδικασίες σε stressful επίπεδα. Μπορείτε να συγκρίνετε το χρόνο που χρειάζεται η εκτελείται δύο φόρτο για να ολοκληρωθεί.


Όταν εκτελείτε ostress.exe, συνιστάται να περάσετε τιμές παραμέτρων που έχει σχεδιαστεί για και τα δύο:

- Εκτέλεση μεγάλου αριθμού ταυτόχρονες συνδέσεις, με τη χρήση - n100.
- Έχετε κάθε επανάληψη σύνδεσης εκατοντάδες φορές, με χρήση - r500.


Ωστόσο, ενδέχεται να θέλετε να ξεκινήσετε με πολύ μικρότερες τιμές όπως - n10 και - r50 για να βεβαιωθείτε ότι όλα εργασία.


### <a name="script-for-ostressexe"></a>Δέσμη ενεργειών για ostress.exe


Αυτή η ενότητα εμφανίζει τη δέσμη ενεργειών T-SQL που είναι ενσωματωμένο σε μας ostress.exe γραμμή εντολών. Η δέσμη ενεργειών χρησιμοποιεί τα στοιχεία που έχουν δημιουργηθεί με τη δέσμη ενεργειών T SQL που εγκαταστήσατε προηγουμένως.


Η ακόλουθη δέσμη ενεργειών εισάγει ένα δείγμα παραγγελίας πωλήσεων με πέντε στοιχεία γραμμής στο παρακάτω Βελτιστοποίηση μνήμης *πίνακες*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;
    
INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);
    
WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Για να κάνετε την έκδοση _ondisk από το προηγούμενο T SQL για ostress.exe, πρέπει να αντικαταστήσετε απλώς δύο εμφανίσεις της δευτερεύουσας συμβολοσειράς *_inmem* με *_ondisk*. Αυτές οι αντικαθιστά επηρεάζουν τα ονόματα των πινάκων και αποθηκευμένες διαδικασίες.


### <a name="install-rml-utilities-and-ostress"></a>Βοηθητικά προγράμματα RML εγκατάσταση και ostress


Ιδανικά θα σκοπεύετε να εκτελέσετε ostress.exe σε μια Εικονική Azure. Πρέπει να δημιουργήσετε μια [εικονική μηχανή Azure](https://azure.microsoft.com/documentation/services/virtual-machines/) στην ίδια Azure γεωγραφική περιοχή όπου βρίσκεται η βάση δεδομένων AdventureWorksLT. Αλλά μπορείτε να εκτελέσετε ostress.exe στο φορητό υπολογιστή σας αντί για αυτό.


Στο η Εικονική ή ό, τι κεντρικού υπολογιστή επιλέξτε, εγκαταστήσετε τα βοηθητικά προγράμματα αναπαραγωγής Markup Language (RML), που περιλαμβάνουν ostress.exe.

- Ανατρέξτε στο θέμα συζήτησης ostress.exe στο [Δείγμα βάσης δεδομένων για Συναλλαγής στη μνήμη](http://msdn.microsoft.com/library/mt465764.aspx).
 - Εναλλακτικά, ανατρέξτε στην ενότητα [Δείγμα βάσης δεδομένων για Συναλλαγής στη μνήμη](http://msdn.microsoft.com/library/mt465764.aspx).
 - Ή ανατρέξτε στο θέμα [ιστολόγιο για την εγκατάσταση του ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx)



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Εκτελέστε το φόρτο εργασίας φόρτο _inmem πρώτα


Μπορείτε να χρησιμοποιήσετε ένα παράθυρο *Γραμμής εντολών Cmd RML* για να εκτελέσετε τη γραμμή εντολών ostress.exe. Τις παραμέτρους γραμμής εντολών απευθείας ostress για να:

- Εκτέλεση 100 συνδέσεις ταυτόχρονα (-n100).
- Έχετε κάθε σύνδεση εκτελέστε τη δέσμη ενεργειών T-SQL 50 φορές (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


Για να εκτελέσετε την προηγούμενη γραμμή εντολών ostress.exe:


1. Επαναφορά του περιεχομένου δεδομένα της βάσης δεδομένων, εκτελώντας την παρακάτω εντολή στο SSMS, για να διαγράψετε όλα τα δεδομένα που έχει εισαχθεί από οποιαδήποτε προηγούμενη εκτελείται:
```
EXECUTE Demo.usp_DemoReset;
```

2. Αντιγράψτε το κείμενο της προηγούμενης γραμμής εντολών ostress.exe στο Πρόχειρο.

3. Αντικαταστήστε το `<placeholders>` για το παράμετροι -S - U -Π -d με τις σωστές τιμές πραγματικό.

4. Εκτελέστε το επεξεργασμένο γραμμής εντολών σε ένα παράθυρο RML Cmd.


#### <a name="result-is-a-duration"></a>Αποτέλεσμα είναι μια διάρκεια


Όταν ολοκληρωθεί η ostress.exe, εγγράφει η εκτέλεση διάρκεια ως του ολοκληρωμένου γραμμή εξόδου στο παράθυρο RML Cmd. Για παράδειγμα, μια μικρότερη εκτέλεση δοκιμής τελευταία περίπου 1,5 λεπτά:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Επαναφορά, επεξεργαστείτε για _ondisk και, στη συνέχεια, εκτελέστε ξανά το


Αφού έχετε το αποτέλεσμα από την εκτέλεση _inmem, εκτελέστε τα ακόλουθα βήματα για την εκτέλεση _ondisk:


1. Επαναφορά της βάσης δεδομένων, εκτελώντας την παρακάτω εντολή στο SSMS, για να διαγράψετε όλα τα δεδομένα που έχει εισαχθεί από την προηγούμενη εκτέλεση:
```
EXECUTE Demo.usp_DemoReset;
```

2. Επεξεργαστείτε τη γραμμή εντολών ostress.exe για να αντικαταστήσετε όλες *_inmem* με *_ondisk*.

3. Εκτελέστε ξανά το ostress.exe για δεύτερη φορά και να καταγράψετε το αποτέλεσμα διάρκεια.

4. Επαναφορά ξανά τη βάση δεδομένων, για υπεύθυνοι διαγραφή του τι μπορεί να είναι ένα μεγάλο όγκο δεδομένων δοκιμής.


#### <a name="expected-comparison-results"></a>Αποτελέσματα σύγκρισης αναμενόμενο

Δοκιμές μας στη μνήμη έχουν εμφανίζεται μια βελτίωση των επιδόσεων **9 ώρες** για αυτό simplistic φόρτο εργασίας, με ostress εκτελείται σε μια Εικονική Azure στην ίδια περιοχή Azure ως βάση δεδομένων.



<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;


## <a name="b-install-the-in-memory-analytics-sample"></a>B. Εγκατάσταση του δείγματος ανάλυσης στη μνήμη


Σε αυτήν την ενότητα, συγκρίνετε τα αποτελέσματα εισόδου/ΕΞΌΔΟΥ και στατιστικά στοιχεία όταν χρησιμοποιείτε ένα ευρετήριο columnstore έναντι ένα ευρετήριο παραδοσιακή b-δέντρου.


Για σε πραγματικό χρόνο ανάλυση σε ένα φόρτο εργασίας Συναλλαγής, συχνά είναι καλύτερα να χρησιμοποιήσετε ένα μη συγκεντρωτικό columnstore ευρετήριο. Για λεπτομέρειες, ανατρέξτε [Περιγράφεται ευρετήρια Columnstore](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Προετοιμασία της δοκιμής ανάλυση columnstore


1. Χρησιμοποιήστε την πύλη του Azure για να δημιουργήσετε μια νέα βάση δεδομένων AdventureWorksLT από το δείγμα.
 - Χρησιμοποιήστε αυτό το ακριβές όνομα.
 - Επιλέξτε οποιοδήποτε επίπεδο υπηρεσιών Premium.

2. Αντιγράψτε το [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) στο Πρόχειρο.
 - Δέσμη ενεργειών T SQL που δημιουργεί τα αντικείμενα είναι απαραίτητο στη μνήμη AdventureWorksLT δείγμα βάσης δεδομένων που δημιουργήσατε στο βήμα 1.
 - Η δέσμη ενεργειών δημιουργεί πίνακα Διάσταση και δύο πινάκων δεδομένων. Οι πίνακες fact συμπληρώνονται με 3,5 εκατομμύρια γραμμές.
 - Η δέσμη ενεργειών μπορεί να διαρκέσει 15 λεπτά για να ολοκληρωθεί.

3. Επικολλήστε τη δέσμη ενεργειών T-SQL σε SSMS και, στη συνέχεια, εκτελέστε τη δέσμη ενεργειών.
 - Κρίσιμης σημασίας είναι η λέξη-κλειδί **COLUMNSTORE** σε μια πρόταση **CREATE INDEX** , ως:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Ορισμός AdventureWorksLT επίπεδο συμβατότητας 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`
 - Επίπεδο 130 δεν σχετίζεται απευθείας στη μνήμη δυνατότητες. Αλλά επίπεδο 130 παρέχει γενικά ταχύτερη απόδοση ερωτήματος από ό, 120.


#### <a name="crucial-tables-and-columnstore-indexes"></a>Κρίσιμης σημασίας πίνακες και ευρετήρια columnstore


- dbo. FactResellerSalesXL_CCI είναι ένας πίνακας που έχει ένα ευρετήριο ομαδοποιημένων **columnstore** , το οποίο εξελιγμένα συμπίεσης στο επίπεδο *δεδομένων* .

- dbo. FactResellerSalesXL_PageCompressed είναι ένας πίνακας που έχει μια ισοδύναμη κανονική ομαδοποιημένων ευρετήριο, το οποίο είναι συμπιεσμένη μόνο στο επίπεδο της *σελίδας* .


#### <a name="crucial-queries-to-compare-the-columnstore-index"></a>Κρίσιμης σημασίας ερωτημάτων για να συγκρίνετε το ευρετήριο columnstore


[Εδώ](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) είναι πολλούς τύπους ερωτήματος T SQL μπορείτε να εκτελέσετε για να δείτε βελτιώσεις απόδοσης. Από το βήμα 2 στη δέσμη ενεργειών T-SQL, υπάρχει ένα ζεύγος ερωτήματα που σας ενδιαφέρουν απευθείας. Τα δύο ερωτήματα διαφέρουν μόνο σε μία γραμμή:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Είναι ένα ευρετήριο ομαδοποιημένων columnstore στον πίνακα**_CCI** FactResellerSalesXL.

Το παρακάτω απόσπασμα δέσμης ενεργειών T-SQL εκτυπώνει στατιστικά στοιχεία για εισόδου/ΕΞΌΔΟΥ και την ΏΡΑ για το ερώτημα κάθε πίνακα.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a Clustered Columnstore index CCI 
-- The comparison numbers are even more dramatic the larger the table is, this is a 11 million row table only.
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```



<a id="preview_considerations_for_in_memory" name="preview_considerations_for_in_memory"></a>


## <a name="preview-considerations-for-in-memory-oltp"></a>Προεπισκόπηση ζητήματα για Συναλλαγής στη μνήμη


Οι δυνατότητες στη μνήμη Συναλλαγής σε βάση δεδομένων SQL Azure γίνεται [ενεργό για προεπισκόπηση σε 28 Οκτωβρίου 2015](https://azure.microsoft.com/updates/public-preview-in-memory-oltp-and-real-time-operational-analytics-for-azure-sql-database/).


Στην τρέχουσα προεπισκόπηση, Συναλλαγής στη μνήμη υποστηρίζεται μόνο για:

- Βάσεις δεδομένων που υπάρχουν σε ένα επίπεδο υπηρεσιών *Premium* .

- Βάσεις δεδομένων που έχουν δημιουργηθεί μετά τις δυνατότητες Συναλλαγής στη μνήμη γίνεται ενεργή.
 - Μια νέα βάση δεδομένων δεν υποστηρίζει Συναλλαγής στη μνήμη, εάν αυτό είναι δυνατή η επαναφορά από μια βάση δεδομένων που δημιουργήθηκε πριν από τις δυνατότητες Συναλλαγής στη μνήμη γίνεται ενεργή.


Όταν βρίσκεστε σε έχετε αμφιβολίες, μπορείτε να εκτελείτε πάντα την παρακάτω ΕΠΙΛΈΞΤΕ T-SQL για να διαπιστώσετε εάν η βάση δεδομένων σας υποστηρίζει Συναλλαγής στη μνήμη. Ένα αποτέλεσμα **1** σημαίνει ότι η βάση δεδομένων υποστηρίζει Συναλλαγής στη μνήμη:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```


Εάν το ερώτημα επιστρέφει **1**, Συναλλαγής στη μνήμη υποστηρίζεται σε αυτήν τη βάση δεδομένων και το αντίγραφο της βάσης δεδομένων και τις Επαναφορά βάσης δεδομένων που δημιουργήθηκε με βάση αυτήν τη βάση δεδομένων.


#### <a name="objects-allowed-only-at-premium"></a>Αντικείμενα επιτρέπεται μόνο σε Premium


Εάν μια βάση δεδομένων περιέχει οποιαδήποτε από τα παρακάτω είδη αντικειμένων στη μνήμη Συναλλαγής ή τύπους, υποβάθμιση το επίπεδο υπηρεσιών της βάσης δεδομένων από Premium για βασικές ή τυπική δεν υποστηρίζεται. Για να υποβιβάσετε τη βάση δεδομένων, πρέπει πρώτα να αποθέσετε αυτά τα αντικείμενα:

- Βελτιστοποίηση μνήμης πίνακες
- Τύποι πίνακα Βελτιστοποίηση μνήμης
- Οι μεταγλωττισμένες εγγενώς λειτουργικές μονάδες


#### <a name="other-relationships"></a>Άλλες σχέσεις


- Χρήση Συναλλαγής στη μνήμη δυνατότητες με βάσεις δεδομένων στην ελαστικότητας χώρους συγκέντρωσης δεν υποστηρίζεται κατά τη διάρκεια της προεπισκόπησης.
 - Για να μετακινήσετε μια βάση δεδομένων που έχει ή είχε Συναλλαγής στη μνήμη αντικείμενα σε ένα σύνολο ελαστικά, ακολουθήστε τα παρακάτω βήματα:
  - 1. Απόθεση οποιαδήποτε Βελτιστοποίηση μνήμης πινάκων, οι τύποι πίνακα και εγγενώς μεταγλωττισμένο λειτουργικές μονάδες T-SQL στη βάση δεδομένων
  - 2. Αλλάξτε το επίπεδο υπηρεσιών της βάσης δεδομένων σε πρότυπο
  - 3. Μετακίνηση της βάσης δεδομένων σε χώρο συγκέντρωσης ελαστικότητας

- Χρήση Συναλλαγής στη μνήμη με αποθήκη δεδομένων του SQL δεν υποστηρίζεται.
 - Η δυνατότητα ευρετηρίου columnstore της ανάλυσης που βρίσκεται στη μνήμη υποστηρίζεται στο αποθήκη δεδομένων του SQL.

- Ο χώρος αποθήκευσης του ερωτήματος δεν καταγραφή ερωτημάτων εσωτερικό μεταγλωττιστεί εγγενώς λειτουργικές μονάδες.

- Ορισμένες δυνατότητες Transact-SQL δεν υποστηρίζονται με Συναλλαγής στη μνήμη. Αυτό ισχύει για το Microsoft SQL Server και βάση δεδομένων SQL Azure. Για λεπτομέρειες, ανατρέξτε στα θέματα:
 - [Υποστήριξη Transact-SQL για Συναλλαγής στη μνήμη](http://msdn.microsoft.com/library/dn133180.aspx)
 - [Δομές Transact-SQL που δεν υποστηρίζεται από τον Συναλλαγής στη μνήμη](http://msdn.microsoft.com/library/dn246937.aspx)


## <a name="next-steps"></a>Επόμενα βήματα


- Δοκιμάστε [Συναλλαγής στη μνήμη χρήση σε μια υπάρχουσα εφαρμογή SQL Azure.](sql-database-in-memory-oltp-migration.md)


## <a name="additional-resources"></a>Πρόσθετοι πόροι

#### <a name="deeper-information"></a>Περαιτέρω πληροφορίες

- [Μάθετε περισσότερα σχετικά με Συναλλαγής στη μνήμη, η οποία ισχύει για το Microsoft SQL Server και βάση δεδομένων SQL Azure](http://msdn.microsoft.com/library/dn133186.aspx)

- [Μάθετε περισσότερα σχετικά με σε πραγματικό χρόνο λειτουργικές ανάλυσης στο MSDN](http://msdn.microsoft.com/library/dn817827.aspx)

- Λευκή Βίβλος για [κοινά μοτίβων φόρτο εργασίας και ρυθμίσεις μετεγκατάστασης](http://msdn.microsoft.com/library/dn673538.aspx), που περιγράφει τα μοτίβα φόρτο εργασίας όπου Συναλλαγής στη μνήμη παρέχει συνήθως σημαντικά κέρδη στην απόδοση.

#### <a name="application-design"></a>Σχεδίαση εφαρμογής

- [Στη μνήμη Συναλλαγής (βελτιστοποίηση στη μνήμη)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Χρήση Συναλλαγής στη μνήμη σε μια υπάρχουσα εφαρμογή SQL Azure.](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Εργαλεία

- [Δεδομένων του SQL Server εργαλεία Προεπισκόπηση (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx), για την πιο πρόσφατη έκδοση μηνιαία.

- [Περιγραφή των βοηθητικών προγραμμάτων αναπαραγωγής Markup Language (RML) για τον SQL Server](http://support.microsoft.com/en-us/kb/944837)

- [Οθόνη αποθήκευσης στη μνήμη](sql-database-in-memory-oltp-monitoring.md) για Συναλλαγής στη μνήμη.

