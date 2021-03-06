<properties
    pageTitle="Ρύθμιση παραμέτρων παν αναπαραγωγής για βάση δεδομένων SQL Azure με Transact-SQL | Microsoft Azure"
    description="Ρύθμιση παραμέτρων παν αναπαραγωγής για βάση δεδομένων SQL Azure χρησιμοποιώντας την Transact-SQL"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/13/2016"
    ms.author="carlrab"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-transact-sql"></a>Ρύθμιση παραμέτρων παν αναπαραγωγής για βάση δεδομένων SQL Azure με Transact-SQL

> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-geo-replication-overview.md)
- [Πύλη του Azure](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Σε αυτό το άρθρο δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους του ενεργού παν-αναπαραγωγής για μια βάση δεδομένων SQL Azure με Transact-SQL.

Για να ξεκινήσετε ανακατεύθυνσης χρησιμοποιώντας την Transact-SQL, ανατρέξτε στο θέμα [Προετοιμασία προγραμματισμένες ή μη προγραμματισμένες ανακατεύθυνσης για βάση δεδομένων SQL Azure με Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Ενεργό παν αναπαραγωγής (αναγνώσιμα secondaries) είναι τώρα διαθέσιμο για όλες τις βάσεις δεδομένων σε όλα τα επίπεδα εξυπηρέτησης. Στο 2017 Απριλίου θα απόσυρση ο δευτερεύων τύπος μη αναγνώσιμο και υπάρχουσες βάσεις δεδομένων δεν είναι αναγνώσιμο θα αναβαθμιστούν αυτόματα σε ευανάγνωστη secondaries.

Για τη ρύθμιση παραμέτρων ενεργού παν-αναπαραγωγής χρησιμοποιώντας Transact-SQL, χρειάζεστε τα εξής:

- Μια συνδρομή του Azure.
- Λογική διακομιστή βάσης δεδομένων SQL Azure <MyLocalServer> και μια βάση δεδομένων SQL <MyDB> -την κύρια βάση δεδομένων που θέλετε να αναπαραγάγετε.
- Μία ή περισσότερες λογικές βάση δεδομένων SQL Azure διακομιστές < MySecondaryServer(n) > - η λογική διακομιστές που θα τους διακομιστές συνεργάτη με την οποία θα δημιουργήσετε δευτερεύοντα βάσεις δεδομένων.
- Μια σύνδεση που είναι DBManager στην κύρια, έχετε db_ownership της τοπικής βάσης δεδομένων που θα παν-αντιγραφή, και είναι DBManager οι διακομιστές συνεργάτη στο οποίο θα ρυθμίσετε παν αναπαραγωγής.
- SQL Server Management Studio (SSMS)

> [AZURE.IMPORTANT] Συνιστάται να χρησιμοποιείτε πάντα την πιο πρόσφατη έκδοση του Management Studio για να παραμείνετε συγχρονισμένοι με ενημερώσεις για το Microsoft Azure και βάση δεδομένων SQL. [Ενημέρωση SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="add-secondary-database"></a>Προσθήκη δευτερεύουσας βάσης δεδομένων

Μπορείτε να χρησιμοποιήσετε τη δήλωση **ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ** για να δημιουργήσετε αναπαραχθούν παν δευτερεύοντα βάσης δεδομένων σε ένα διακομιστή του συνεργάτη. Μπορείτε να εκτελέσετε αυτήν τη δήλωση στην κύρια βάση δεδομένων του διακομιστή που περιέχει τη βάση δεδομένων να αναπαραχθεί. Το παν-βάση δεδομένων από αναπαραγωγή (το "κύρια βάση δεδομένων") θα έχει το ίδιο όνομα με τη βάση δεδομένων γίνεται αναπαραγωγή και θα, από προεπιλογή, έχουν το ίδιο επίπεδο service ως την κύρια βάση δεδομένων. Δευτερεύουσα βάσης δεδομένων μπορεί να είναι αναγνώσιμο ή μη αναγνώσιμο και μπορεί να είναι μία βάση δεδομένων ή ένα ελαστικά databbase. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) και [Επίπεδα υπηρεσίας](sql-database-service-tiers.md).
Μετά τη δευτερεύουσα βάση δεδομένων δημιουργήθηκε και τοποθετούνται, δεδομένων θα ξεκινήσει η αναπαραγωγή ασύγχρονα από την κύρια βάση δεδομένων. Τα παρακάτω βήματα περιγράφουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους παν αναπαραγωγής χρησιμοποιώντας Management Studio. Βήματα για να δημιουργήσετε μη αναγνώσιμο και είναι ευανάγνωστο secondaries, είτε με μία βάση δεδομένων ή μια βάση δεδομένων της ελαστικότητας, παρέχονται.

> [AZURE.NOTE] Εάν υπάρχει μια βάση δεδομένων στο διακομιστή καθορισμένο συνεργάτη με το ίδιο όνομα με την κύρια βάση δεδομένων θα αποτύχει η εντολή.


### <a name="add-non-readable-secondary-single-database"></a>Προσθήκη μη αναγνώσιμο δευτερεύον (μία βάση δεδομένων)

Χρησιμοποιήστε τα ακόλουθα βήματα για να δημιουργήσετε μη αναγνώσιμο δευτερεύον ως μία βάση δεδομένων.

1. Χρησιμοποιώντας την έκδοση 13.0.600.65 ή νεότερη έκδοση του SQL Server Management Studio.

     > [AZURE.IMPORTANT] Κάντε λήψη του την [πιο πρόσφατη](https://msdn.microsoft.com/library/mt238290.aspx) έκδοση του SQL Server Management Studio. Συνιστάται να χρησιμοποιείτε πάντα την πιο πρόσφατη έκδοση του Management Studio για να παραμείνει σε συγχρονισμό με ενημερώσεις στην πύλη του Azure.


2. Ανοίξτε το φάκελο βάσεις δεδομένων, αναπτύξτε το φάκελο **Βάσεων δεδομένων συστήματος** , κάντε δεξί κλικ στην **κύρια**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέο ερώτημα**.

3. Χρησιμοποιήστε την ακόλουθη πρόταση **ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ** για να μετατρέψετε μια τοπική βάση δεδομένων σε μια αναπαραγωγή παν πρωτεύοντος με μη αναγνώσιμο δευτερεύοντα βάσης δεδομένων σε MySecondaryServer1 όπου MySecondaryServer1 είναι το όνομα του διακομιστή φιλικό.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Κάντε κλικ στο κουμπί **Εκτέλεση** για να εκτελέσετε το ερώτημα.


### <a name="add-readable-secondary-single-database"></a>Προσθήκη δευτερεύουσας αναγνώσιμο (μία βάση δεδομένων)
Χρησιμοποιήστε τα ακόλουθα βήματα για να δημιουργήσετε αναγνώσιμο δευτερεύον ως μία βάση δεδομένων.

1. Στο Management Studio, συνδεθείτε με το διακομιστή λογική βάση δεδομένων SQL Azure.

2. Ανοίξτε το φάκελο βάσεις δεδομένων, αναπτύξτε το φάκελο **Βάσεων δεδομένων συστήματος** , κάντε δεξί κλικ στην **κύρια**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέο ερώτημα**.

3. Χρησιμοποιήστε την ακόλουθη πρόταση **ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ** για να μετατρέψετε μια τοπική βάση δεδομένων σε μια αναπαραγωγή παν πρωτεύοντος με αναγνώσιμη δευτερεύοντα βάσης δεδομένων σε έναν δευτερεύοντα διακομιστή.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Κάντε κλικ στο κουμπί **Εκτέλεση** για να εκτελέσετε το ερώτημα.



### <a name="add-non-readable-secondary-elastic-database"></a>Προσθήκη μη αναγνώσιμο δευτερεύον (ελαστικότητας βάση δεδομένων)

Χρησιμοποιήστε τα ακόλουθα βήματα για να δημιουργήσετε μη αναγνώσιμο δευτερεύον ως βάση ελαστικότητας.

1. Στο Management Studio, συνδεθείτε με το διακομιστή λογική βάση δεδομένων SQL Azure.

2. Ανοίξτε το φάκελο βάσεις δεδομένων, αναπτύξτε το φάκελο **Βάσεων δεδομένων συστήματος** , κάντε δεξί κλικ στην **κύρια**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέο ερώτημα**.

3. Χρησιμοποιήστε την ακόλουθη πρόταση **ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ** για να μετατρέψετε μια τοπική βάση δεδομένων σε μια αναπαραγωγή παν πρωτεύοντος με μη αναγνώσιμο δευτερεύοντα βάσης δεδομένων σε έναν δευτερεύοντα διακομιστή σε ένα σύνολο ελαστικότητας.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Κάντε κλικ στο κουμπί **Εκτέλεση** για να εκτελέσετε το ερώτημα.



### <a name="add-readable-secondary-elastic-database"></a>Προσθήκη αναγνώσιμο δευτερεύον (ελαστικότητας βάση δεδομένων)
Χρησιμοποιήστε τα ακόλουθα βήματα για να δημιουργήσετε αναγνώσιμο δευτερεύον ως βάση ελαστικότητας.

1. Στο Management Studio, συνδεθείτε με το διακομιστή λογική βάση δεδομένων SQL Azure.

2. Ανοίξτε το φάκελο βάσεις δεδομένων, αναπτύξτε το φάκελο **Βάσεων δεδομένων συστήματος** , κάντε δεξί κλικ στην **κύρια**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέο ερώτημα**.

3. Χρησιμοποιήστε την ακόλουθη πρόταση **ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ** για να μετατρέψετε μια τοπική βάση δεδομένων σε μια αναπαραγωγή παν πρωτεύοντος με αναγνώσιμη δευτερεύοντα βάσης δεδομένων σε έναν δευτερεύοντα διακομιστή σε ένα σύνολο ελαστικότητας.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Κάντε κλικ στο κουμπί **Εκτέλεση** για να εκτελέσετε το ερώτημα.



## <a name="remove-secondary-database"></a>Κατάργηση δευτερεύοντα βάσης δεδομένων

Μπορείτε να χρησιμοποιήσετε τη δήλωση **ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ** για να τερματίσετε οριστικά τη συνεργασία αλληλεπίδραση μεταξύ μιας δευτερεύουσας βάσης δεδομένων και τα κύρια. Αυτή η πρόταση εκτελείται στην κύρια βάση δεδομένων στην οποία βρίσκεται η κύρια βάση δεδομένων. Μετά τη λύση τη σχέση, τη δευτερεύουσα βάση δεδομένων μετατρέπεται σε κανονική βάση δεδομένων ανάγνωση και εγγραφή. Εάν τη συνδεσιμότητα με δευτερεύοντα βάση δεδομένων είναι κατεστραμμένη την εντολή ολοκληρωθεί με επιτυχία, αλλά τη δευτερεύουσα θα μετατραπεί σε ανάγνωση και εγγραφή μετά την επαναφορά συνδεσιμότητας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) και [Επίπεδα υπηρεσίας](sql-database-service-tiers.md).

Χρησιμοποιήστε τα ακόλουθα βήματα για να καταργήσετε αναπαραχθούν παν δευτερεύον από μια συνεργασία παν αναπαραγωγής.

1. Στο Management Studio, συνδεθείτε με το διακομιστή λογική βάση δεδομένων SQL Azure.

2. Ανοίξτε το φάκελο βάσεις δεδομένων, αναπτύξτε το φάκελο **Βάσεων δεδομένων συστήματος** , κάντε δεξί κλικ στην **κύρια**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέο ερώτημα**.

3. Χρησιμοποιήστε την ακόλουθη πρόταση **ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ** για να καταργήσετε ένα δευτερεύον αναπαραχθούν παν.

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Κάντε κλικ στο κουμπί **Εκτέλεση** για να εκτελέσετε το ερώτημα.

## <a name="monitor-geo-replication-configuration-and-health"></a>Ρύθμιση παραμέτρων αναπαραγωγής παν οθόνη και εύρυθμης λειτουργίας

Παρακολούθηση εργασίες περιλαμβάνουν παρακολούθηση της ρύθμισης παραμέτρων παν αναπαραγωγής και παρακολούθηση εύρυθμης λειτουργίας αναπαραγωγής δεδομένων.  Μπορείτε να χρησιμοποιήσετε την προβολή δυναμική διαχείριση **sys.dm_geo_replication_links** στην κύρια βάση δεδομένων για να λάβετε πληροφορίες σχετικά με όλες τις συνδέσεις αναπαραγωγής Τερματισμός λειτουργίας για κάθε βάση δεδομένων στο διακομιστή βάσης δεδομένων SQL Azure λογική. Αυτή η προβολή περιλαμβάνει μια γραμμή για κάθε έναν από τη σύνδεση αλληλεπίδραση μεταξύ κύριας και δευτερεύουσας βάσεις δεδομένων. Μπορείτε να χρησιμοποιήσετε την προβολή δυναμική διαχείριση **sys.dm_replication_link_status** για να επιστρέψει μια γραμμή για κάθε βάση δεδομένων SQL Azure που είναι αυτήν τη στιγμή που εκτελούν μια σύνδεση αναπαραγωγής αναπαραγωγής. Περιλαμβάνονται και οι κύριες και δευτερεύουσες βάσεις δεδομένων. Εάν υπάρχει περισσότερες από μία σύνδεση Συνεχής αναπαραγωγή για μια δεδομένη κύρια βάση δεδομένων, αυτός ο πίνακας περιέχει μια γραμμή για κάθε μία από τις σχέσεις. Η προβολή δημιουργείται σε όλες τις βάσεις δεδομένων, όπως το υπόδειγμα λογική. Ωστόσο, κατά την υποβολή ερωτήματος αυτής της προβολής στο υπόδειγμα λογικές επιστρέφει ένα κενό σύνολο. Μπορείτε να χρησιμοποιήσετε την προβολή δυναμική διαχείριση **sys.dm_operation_status** για να εμφανίσετε την κατάσταση για όλες τις εργασίες βάσης δεδομένων συμπεριλαμβανομένης της κατάστασης των συνδέσεων αναπαραγωγής. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [sys.geo_replication_links (βάση δεδομένων SQL Azure)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (βάση δεδομένων SQL Azure)](https://msdn.microsoft.com/library/mt575504.aspx)και [sys.dm_operation_status (βάση δεδομένων SQL Azure)](https://msdn.microsoft.com/library/dn270022.aspx).

Χρησιμοποιήστε τα παρακάτω βήματα για να παρακολουθείτε μια συνεργασία παν αναπαραγωγής.

1. Στο Management Studio, συνδεθείτε με το διακομιστή λογική βάση δεδομένων SQL Azure.

2. Ανοίξτε το φάκελο βάσεις δεδομένων, αναπτύξτε το φάκελο **Βάσεων δεδομένων συστήματος** , κάντε δεξί κλικ στην **κύρια**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέο ερώτημα**.

3. Χρησιμοποιήστε την ακόλουθη πρόταση για να εμφανίσετε όλες τις βάσεις δεδομένων με συνδέσεις παν αναπαραγωγής.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Κάντε κλικ στο κουμπί **Εκτέλεση** για να εκτελέσετε το ερώτημα.
5. Ανοίξτε το φάκελο βάσεις δεδομένων, αναπτύξτε το φάκελο **Βάσεων δεδομένων συστήματος** , κάντε δεξί κλικ στην **περίπτωση**, και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέο ερώτημα**.
6. Χρησιμοποιήστε την ακόλουθη πρόταση για να εμφανίσετε την αναπαραγωγή υστέρηση και η τελευταία αναπαραγωγή χρόνου μου δευτερεύοντα βάσεων δεδομένων με την περίπτωση.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Κάντε κλικ στο κουμπί **Εκτέλεση** για να εκτελέσετε το ερώτημα.
8. Χρησιμοποιήστε την ακόλουθη πρόταση για να εμφανίσετε τις πιο πρόσφατες λειτουργίες παν αναπαραγωγής που σχετίζεται με βάση δεδομένων περίπτωση.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Κάντε κλικ στο κουμπί **Εκτέλεση** για να εκτελέσετε το ερώτημα.

## <a name="upgrade-a-non-readable-secondary-to-readable"></a>Αναβάθμιση μιας δευτερεύουσας μη αναγνώσιμη σε ευανάγνωστη

Στο 2017 Απριλίου θα απόσυρση ο δευτερεύων τύπος μη αναγνώσιμο και υπάρχουσες βάσεις δεδομένων δεν είναι αναγνώσιμο θα αναβαθμιστούν αυτόματα σε ευανάγνωστη secondaries. Εάν χρησιμοποιείτε μη αναγνώσιμο secondaries σήμερα και θέλετε να αναβαθμίσετε τους να είναι δυνατή η ανάγνωση, μπορείτε να χρησιμοποιήσετε την παρακάτω απλά βήματα για κάθε δευτερεύον.

> [AZURE.IMPORTANT] Δεν υπάρχει μέθοδος από το χρήστη από την αναβάθμιση στην ίδια θέση της μη αναγνώσιμο δευτερεύον σε ευανάγνωστη. Εάν απόρριψη σας μόνο δευτερεύοντα, στη συνέχεια, την κύρια βάση δεδομένων θα παραμείνει μη προστατευμένος μέχρι τη νέα δευτερεύουσα έχει συγχρονιστεί πλήρως. Εάν το SLA της εφαρμογής σας απαιτεί ότι η κύρια προστατεύεται πάντα, πρέπει να λάβετε υπόψη τη δημιουργία μιας δευτερεύουσας παράλληλες σε διαφορετικό διακομιστή πριν από την εφαρμογή τα παραπάνω βήματα αναβάθμισης. Σημείωση κάθε κύρια μπορεί να έχει έως και 4 δευτερεύοντα βάσεις δεδομένων.


1. Πρώτα, συνδεθείτε με το διακομιστή *δευτερεύοντα* και αποθέστε τη μη αναγνώσιμο δευτερεύοντα βάση δεδομένων:  
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Συνδεθείτε στον *πρωτεύοντα* διακομιστή τώρα και να προσθέσετε μια νέα δευτερεύουσα αναγνώσιμο

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε περισσότερα σχετικά με το ενεργό παν-αναπαραγωγής, ανατρέξτε στο θέμα - [Active παν-αναπαραγωγής](sql-database-geo-replication-overview.md)
- Για μια επισκόπηση επιχειρηματικές συνέχειας και σενάρια, ανατρέξτε στο θέμα [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)
