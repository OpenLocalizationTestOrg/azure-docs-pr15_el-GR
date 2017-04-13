<properties
    pageTitle="Διόρθωση ένα σφάλμα σύνδεσης SQL, μεταβατικές σφάλμα | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να αντιμετωπίσετε, διάγνωση και να αποτρέψετε ένα σφάλμα σύνδεσης SQL ή μεταβατικές σφάλμα στη βάση δεδομένων SQL Azure. "
    keywords="σύνδεση SQL, συμβολοσειρά σύνδεσης, ζητήματα συνδεσιμότητας, μεταβατικές σφάλματος, σφάλμα σύνδεσης"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="daleche"/>


# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Αντιμετώπιση προβλημάτων, διάγνωση και αποτροπή σφαλμάτων σύνδεσης SQL και μεταβατικές σφαλμάτων για βάση δεδομένων SQL

Σε αυτό το άρθρο περιγράφει τον τρόπο για να αποτρέψετε την αντιμετώπιση προβλημάτων, διάγνωση και συμβάλει στην αντιμετώπιση σφαλμάτων σύνδεσης και μεταβατικές σφαλμάτων που συναντά την εφαρμογή-πελάτη όταν το αλληλεπιδρά με βάση δεδομένων SQL Azure. Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους της λογικής "Επανάληψη", δημιουργείτε τη συμβολοσειρά σύνδεσης και να προσαρμόσετε άλλες ρυθμίσεις σύνδεσης.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Μεταβατικές σφάλματα (σφάλματα μεταβατικές)

Ένα σφάλμα μεταβατικές - επίσης, μεταβατικές σφαλμάτων - έχει ένα υποκείμενο αιτία που επιλύει σύντομα ίδια. Μια περιστασιακές αιτία μεταβατικές σφάλματα είναι όταν το σύστημα Azure γρήγορα μετατοπίζει πόροι υλικού για την καλύτερη εξισορρόπηση φόρτου διάφορες φόρτους εργασίας. Τα περισσότερα από αυτά τα συμβάντα αλλαγή των ρυθμίσεων παραμέτρων ολοκλήρωση συχνά σε λιγότερο από 60 δευτερόλεπτα. Κατά τη διάρκεια αυτό το αλλαγή των ρυθμίσεων παραμέτρων χρονικό διάστημα, μπορεί να έχετε προβλήματα συνδεσιμότητας με βάση δεδομένων SQL Azure. Σύνδεση με βάση δεδομένων SQL Azure εφαρμογές θα πρέπει να βασίζεται να αναμένετε μεταβατικές αυτά τα σφάλματα, τις χειριστεί με την εφαρμογή "Επανάληψη" λογική στον κώδικά τους, αντί για εμφάνιση τους σε χρήστες με σφάλματα εφαρμογών.

Εάν το πρόγραμμα-πελάτη είναι χρησιμοποιώντας το ADO.NET, το πρόγραμμά σας είναι σχετική οδηγία σχετικά με το σφάλμα μεταβατικές από το πετώ από μια **SqlException**. Η ιδιότητα **αριθμός** μπορούν να συγκριθούν με στη λίστα των σφαλμάτων μεταβατικές κοντά στο επάνω μέρος του θέματος: [τους κωδικούς σφάλματος SQL για εφαρμογές προγράμματος-πελάτη βάσης δεδομένων SQL](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Σύνδεση έναντι της εντολής

Θα επαναλάβετε τη διαδικασία σύνδεσης SQL ή δημιουργήστε ξανά, ανάλογα με τα εξής:

* **Παρουσιάζεται ένα σφάλμα μεταβατικές κατά τη δοκιμή σύνδεσης**: Η σύνδεση θα πρέπει να επαναληφθεί η προσπάθεια μετά να καθυστερήσει για μερικά δευτερόλεπτα.

* **Παρουσιάζεται ένα σφάλμα μεταβατικές κατά τη διάρκεια μια εντολή SQL**: Η εντολή δεν θα πρέπει να επαναληφθούν αμέσως. Αντί για αυτό, από μια καθυστέρηση, θα πρέπει η δημιουργία της σύνδεσης πρόσφατα. Στη συνέχεια, να επαναληφθεί η εντολή.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Λογικής για μεταβατικές σφάλματα επανάληψης


Τα προγράμματα-πελάτες που συναντήσετε μερικές φορές ένα σφάλμα μεταβατικές είναι πιο ισχυρό όταν περιέχουν λογικής "Επανάληψη".


Όταν το πρόγραμμα επικοινωνεί με βάση δεδομένων SQL Azure μέσω ενός 3η ενδιάμεσο πάρτι, ερώτημα με τον προμηθευτή αν το ενδιάμεσο περιέχει λογική "Επανάληψη" για μεταβατικές σφάλματα.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Αρχές για "Επανάληψη"


- Εάν το σφάλμα είναι μεταβατικές πρέπει να επαναληφθεί η προσπάθεια για να ανοίξετε μια σύνδεση.


- Μια πρόταση SQL SELECT που αποτυγχάνει με μεταβατικές σφάλμα δεν πρέπει να επαναληφθεί απευθείας.
 - Αντί για αυτό, δημιουργήστε μια νέα σύνδεση και, στη συνέχεια, επαναλάβετε την ΕΠΙΛΟΓΉ.


- Όταν αποτύχει μια πρόταση SQL UPDATE με μεταβατικές σφάλμα, Φρέσκο πρέπει να σύνδεση πριν από την επανάληψη της ενημερωμένης ΈΚΔΟΣΗΣ.
 - Η λογική "Επανάληψη" πρέπει να βεβαιωθείτε ότι η συναλλαγή ολόκληρη τη βάση δεδομένων ολοκληρωθεί ή που ολόκληρη η συναλλαγή επανέρχεται.


#### <a name="other-considerations-for-retry"></a>Άλλα ζητήματα σχετικά με τη "Επανάληψη"


- Ένα πρόγραμμα δέσμης που ξεκινά αυτόματα μετά τις ώρες εργασίας και που θα ολοκληρωθεί πριν από το πρωί, μπορεί να πρέπει να πολύ υπομονή με το χρόνο χρονικά διαστήματα μεταξύ των προσπαθειών "Επανάληψη".


- Ένα πρόγραμμα του περιβάλλοντος εργασίας χρήστη πρέπει να λογαριασμού για την τάση human να δώσετε μετά πολύ χρόνο αναμονής.
 - Ωστόσο, η λύση δεν πρέπει να για να επαναλάβετε την κάθε μερικά δευτερόλεπτα, επειδή η πολιτική είναι δυνατό να κατακλύσουν του συστήματος με αιτήσεις.


#### <a name="interval-increase-between-retries"></a>Αύξηση διάστημα μεταξύ των επαναλήψεων



Συνιστάται να ότι μπορείτε καθυστερήσετε για 5 δευτερόλεπτα πριν από την πρώτη "Επανάληψη". Επανάληψη μετά από μια καθυστέρηση μικρότερη από 5 δευτερόλεπτα τους κινδύνους overwhelming την υπηρεσία cloud. Για κάθε οι επόμενες επανάληψη προσπάθειας την καθυστέρηση θα πρέπει να αυξηθεί εκθετικά, έως και 60 δευτερόλεπτα.

Μια συζήτηση σχετικά με τον *αποκλεισμό περίοδο* για προγράμματα-πελάτες που χρησιμοποιούν το ADO.NET είναι διαθέσιμη στο [SQL Server σύνδεσης συγκέντρωση (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Μπορεί επίσης να θέλετε να ορίσετε ένα μέγιστο αριθμό των επαναλήψεων πριν από την αυτο-καταγγελία το πρόγραμμα.


#### <a name="code-samples-with-retry-logic"></a>Δείγματα κώδικα με λογική "Επανάληψη"


Δείγματα κώδικα με λογική "Επανάληψη", σε διάφορες γλώσσες προγραμματισμού, είναι διαθέσιμες στο:

- [Βιβλιοθήκες σύνδεσης για τη βάση δεδομένων SQL και του SQL Server](sql-database-libraries.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Δοκιμή λογικής "Επανάληψη"


Για να ελέγξετε τη λογική "Επανάληψη", πρέπει να προσομοιώσετε ή να προκαλέσουν σφάλμα από είναι δυνατό να διορθωθεί ενώ εκτελείται ακόμη το πρόγραμμά σας.


##### <a name="test-by-disconnecting-from-the-network"></a>Δοκιμή με αποσύνδεση από το δίκτυο


Έναν τρόπο με τον οποίο μπορείτε να ελέγξετε τη λογική "Επανάληψη" είναι να αποσυνδεθείτε υπολογιστή-πελάτη από το δίκτυο, ενώ εκτελείται το πρόγραμμα. Το σφάλμα θα είναι:

- **SqlException.Number** = 11001
- Μήνυμα: "δεν υπάρχει κεντρικού υπολογιστή είναι γνωστό"


Ως μέρος της την πρώτη προσπάθεια "Επανάληψη", το πρόγραμμα να διορθώσετε το ορθογραφικό λάθος και, στη συνέχεια, προσπαθήστε να συνδεθείτε.


Για να γίνει αυτή η πρακτική, αποσυνδέστε τον υπολογιστή σας από το δίκτυο πριν να ξεκινήσετε το πρόγραμμα. Στη συνέχεια, το πρόγραμμά σας αναγνωρίζει μια παράμετρο χρόνο εκτέλεσης που έχει ως αποτέλεσμα το πρόγραμμα να:

1. Προσθέστε προσωρινά 11001 στη λίστα των σφαλμάτων για να λάβετε υπόψη σας ως μεταβατική.
2. Η προσπάθεια τη σύνδεση στο πρώτο ως συνήθως.
3. Αφού εντοπίζεται το σφάλμα, καταργήστε 11001 από τη λίστα.
4. Εμφανίζεται ένα μήνυμα που σας ενημερώνει το χρήστη για να συνδέσετε τον υπολογιστή στο δίκτυο.
 - Καταδείξτε περαιτέρω εκτέλεσης, χρησιμοποιώντας τη μέθοδο **Console.ReadLine** ή ένα παράθυρο διαλόγου με ένα κουμπί "OK". Ο χρήστης πατήσει το πλήκτρο Enter μετά από τον υπολογιστή συνδεδεμένο σε δίκτυο.
5. Επανάληψη προσπάθειας για να συνδεθείτε, περιμένετε επιτυχίας.


##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Για να δοκιμάσετε, ορθογραφικό λάθος το όνομα της βάσης δεδομένων κατά τη σύνδεση


Το πρόγραμμά σας μπορεί να ανορθόγραφα σκόπιμα το όνομα χρήστη πριν από την πρώτη απόπειρα σύνδεσης. Το σφάλμα θα είναι:

- **SqlException.Number** = 18456
- Μήνυμα: "η σύνδεση απέτυχε για το χρήστη 'WRONG_MyUserName'."


Ως μέρος της την πρώτη προσπάθεια "Επανάληψη", το πρόγραμμα να διορθώσετε το ορθογραφικό λάθος και, στη συνέχεια, προσπαθήστε να συνδεθείτε.


Για να γίνει αυτή η πρακτική, το πρόγραμμα θα μπορούσε να αναγνωρίζουν μια παράμετρο χρόνο εκτέλεσης που έχει ως αποτέλεσμα το πρόγραμμα να:

1. Προσθέστε προσωρινά 18456 στη λίστα των σφαλμάτων για να λάβετε υπόψη σας ως μεταβατική.
2. Προσθήκη σκόπιμα 'WRONG_' για το όνομα χρήστη.
3. Αφού εντοπίζεται το σφάλμα, καταργήστε 18456 από τη λίστα.
4. Κατάργηση 'WRONG_' από το όνομα χρήστη.
5. Επανάληψη προσπάθειας για να συνδεθείτε, περιμένετε επιτυχίας.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>Παράμετροι .NET SqlConnection για επανάληψη προσπάθειας σύνδεσης


Εάν το πρόγραμμα-πελάτης συνδέεται με βάση δεδομένων SQL Azure, χρησιμοποιώντας το .NET Framework κλάση **System.Data.SqlClient.SqlConnection**, θα πρέπει να χρησιμοποιήσετε .NET 4.6.1 ή νεότερη έκδοση, ώστε να μπορείτε να αξιοποιήσετε τη δυνατότητα σύνδεσης "Επανάληψη". Λεπτομέρειες της δυνατότητας είναι [εδώ](http://go.microsoft.com/fwlink/?linkid=393996).


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Όταν δημιουργείτε τη [συμβολοσειρά σύνδεσης](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) για το αντικείμενο **SqlConnection** , θα πρέπει να συντονίσετε τις τιμές από τις ακόλουθες παραμέτρους:

- ConnectRetryCount &nbsp; &nbsp; *(η προεπιλογή είναι 1. Περιοχή είναι 0 έως 255.)*
- ConnectRetryInterval &nbsp; &nbsp; *(η προεπιλογή είναι 1 δευτερόλεπτο. Περιοχή είναι 1 έως 60.)*
- Χρονικό όριο σύνδεσης &nbsp; &nbsp; *(η προεπιλογή είναι 15 δευτερολέπτων. Περιοχή είναι 0 έως 2147483647)*


Συγκεκριμένα, τις τιμές του επιλεγμένου θα πρέπει να κάνετε τα εξής ισότητας true:

- Χρονικό όριο σύνδεσης = ConnectRetryCount * ConnectionRetryInterval

Για παράδειγμα, εάν το πλήθος = 3 και διάστημα = 10 δευτερόλεπτα, ένα χρονικό όριο μόνο 29 δευτερόλεπτα θα απολύτως σας παρέχει το σύστημα αρκετό χρόνο για την τρίτη και τελικών "Επανάληψη" κατά τη σύνδεση: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Σύνδεση έναντι της εντολής


Οι παράμετροι **ConnectRetryCount** και **ConnectRetryInterval** αφήσετε το αντικείμενο **SqlConnection** επανάληψη της λειτουργίας σύνδεση χωρίς που σας ενημερώνει ή bothering πρόγραμμα, όπως η επιστροφή του ελέγχου στο πρόγραμμα. Το επαναλήψεις μπορεί να προκύψουν στις ακόλουθες περιπτώσεις:

- κλήση της μεθόδου mySqlConnection.Open
- κλήση της μεθόδου mySqlConnection.Execute

Υπάρχει μια subtlety. Εάν παρουσιάζεται ένα σφάλμα μεταβατικές ενώ εκτελείται το *ερώτημα* , το αντικείμενο **SqlConnection** ξανά τη λειτουργία σύνδεση και το σίγουρα ξανά το ερώτημά σας. Ωστόσο, **SqlConnection** πολύ γρήγορα ελέγχει τη σύνδεση πριν από την αποστολή του ερωτήματος για εκτέλεση. Εάν η γρήγορη ελέγχου εντοπίσει ένα πρόβλημα σύνδεσης, **SqlConnection** επαναλήψεις τη λειτουργία σύνδεσης. Εάν το "Επανάληψη" είναι επιτυχής, ερωτήματος που αποστέλλεται για εκτέλεση.


#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>ConnectRetryCount πρέπει να συνδυάζεται με λογική εφαρμογών "Επανάληψη";

Ας υποθέσουμε ότι η εφαρμογή σας έχει λογικής ισχυρό προσαρμοσμένο "Επανάληψη". Αυτό μπορεί να επαναλάβετε τη λειτουργία σύνδεση 4 ώρες. Αν προσθέσετε **ConnectRetryInterval** και **ConnectRetryCount** = 3 για να σας συμβολοσειρά σύνδεσης, θα αυξήσετε το πλήθος των επαναλήψεων έως 4 * 3 = 12 επαναλήψεις. Δεν μπορεί να σκοπεύετε τόσο υψηλή αριθμός των επαναλήψεων.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Συνδέσεις με βάση δεδομένων SQL Azure

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Σύνδεσης: Συμβολοσειρά σύνδεσης


Η συμβολοσειρά σύνδεσης είναι απαραίτητο για τη σύνδεση με βάση δεδομένων SQL Azure είναι λίγο διαφορετική από τη συμβολοσειρά σύνδεσης σε Microsoft SQL Server. Μπορείτε να αντιγράψετε τη συμβολοσειρά σύνδεσης για τη βάση δεδομένων από την [Πύλη Azure](https://portal.azure.com/).


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Σύνδεσης: Διεύθυνση IP


Πρέπει να ρυθμίσετε το διακομιστή βάσης δεδομένων SQL ώστε να δέχονται επικοινωνία από τη διεύθυνση IP του υπολογιστή που φιλοξενεί το πρόγραμμα-πελάτη. Μπορείτε να το κάνετε αυτό με επεξεργασία των ρυθμίσεων του τείχους προστασίας μέσω της [Πύλης Azure](https://portal.azure.com/).


Εάν ξεχάσετε να ρυθμίσετε τις παραμέτρους της διεύθυνσης IP, το πρόγραμμα θα αποτύχει με ένα μήνυμα εύχρηστο σφάλματος που αναφέρει τα απαραίτητα διεύθυνση IP.


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα: [ΔΙΑΔΙΚΑΣΙΕΣ: ρύθμιση παραμέτρων τείχους προστασίας στη βάση δεδομένων SQL](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Σύνδεση: θύρες


Συνήθως μόνο πρέπει να βεβαιωθείτε ότι η θύρα 1433 είναι ανοιχτή για τις εξερχόμενες επικοινωνίες, στον υπολογιστή που φιλοξενεί που το πρόγραμμα-πελάτη.


Για παράδειγμα, όταν το πρόγραμμα-πελάτη σας φιλοξενείται σε υπολογιστή με Windows, το τείχος προστασίας των Windows στον κεντρικό υπολογιστή σάς επιτρέπει να ανοίξετε τη θύρα 1433:


1. Ανοίξτε τον πίνακα ελέγχου
2. &gt;Όλα τα στοιχεία Πίνακας ελέγχου
3. &gt;Τείχος προστασίας των Windows
4. &gt;Ρυθμίσεις για προχωρημένους
5. &gt;Κανόνες εξερχόμενων
6. &gt;Ενέργειες
7. &gt;Δημιουργία κανόνα


Εάν το πρόγραμμα-πελάτη φιλοξενείται σε μια εικονική μηχανή Azure (Εικονική), πρέπει να διαβάσετε:<br/>[Θύρες πέρα από 1433 για ADO.NET διαίρεσης 4,5 και V12 βάσης δεδομένων SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


Για πληροφορίες φόντου για cofiguration θύρες και τη διεύθυνση IP, ανατρέξτε στο θέμα: [βάση δεδομένων SQL Azure τείχος προστασίας](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Σύνδεση: ADO.NET 4.6.1


Εάν το πρόγραμμά σας χρησιμοποιεί κλάσεις ADO.NET όπως **System.Data.SqlClient.SqlConnection** για σύνδεση με βάση δεδομένων SQL Azure, συνιστάται να χρησιμοποιήσετε .NET Framework έκδοση 4.6.1 ή νεότερη έκδοση.


ADO.NET 4.6.1:

- Για βάση δεδομένων SQL Azure, υπάρχει βελτιωμένη αξιοπιστία κατά το άνοιγμα μιας σύνδεσης, χρησιμοποιώντας τη μέθοδο **SqlConnection.Open** . Η μέθοδος **Open** ενσωματώνει τώρα βέλτιστη μηχανισμούς προσπάθειας "Επανάληψη" απόκριση σε μεταβατικές σφάλματα, για συγκεκριμένα σφάλματα μέσα στο χρονικό όριο σύνδεσης.
- Υποστηρίζει συγκέντρωση συνδέσεων. Αυτό περιλαμβάνει μια αποτελεσματική επαλήθευσης που λειτουργεί το αντικείμενο σύνδεσης που σας δίνει το πρόγραμμά σας.



Όταν χρησιμοποιείτε ένα αντικείμενο σύνδεσης από ένα χώρο συγκέντρωσης σύνδεσης, συνιστάται να το πρόγραμμά σας να κλείσετε προσωρινά τη σύνδεση όταν χρησιμοποιείτε δεν αμέσως. Εκ νέου άνοιγμα σύνδεσης δεν είναι ακριβό είναι ο τρόπος δημιουργίας μιας νέας σύνδεσης.


Εάν χρησιμοποιείτε το ADO.NET 4.0 ή παλαιότερη έκδοση, συνιστάται να κάνετε αναβάθμιση σε την πιο πρόσφατη ADO.NET.

- Το Νοέμβριο 2015, μπορείτε να [κάνετε λήψη ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Εργαλεία διαγνωστικών

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Εργαλεία διαγνωστικών: Ελέγξτε αν βοηθητικά προγράμματα μπορούν να συνδεθούν


Εάν το πρόγραμμα αποτυγχάνει για σύνδεση με βάση δεδομένων SQL Azure, μία διαγνωστικών επιλογή είναι να επιχειρήσετε να συνδεθείτε με ένα βοηθητικό πρόγραμμα. Ιδανικά βοηθητικού προγράμματος θα συνδεθείτε με τη χρήση της ίδιας βιβλιοθήκης που χρησιμοποιεί το πρόγραμμά σας.


Σε οποιονδήποτε υπολογιστή με Windows, μπορείτε να δοκιμάσετε αυτά τα βοηθητικά προγράμματα:

- SQL Server Management Studio (ssms.exe), που συνδέεται με τη χρήση ADO.NET.
- SQLCMD.exe, που συνδέεται με τη χρήση [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).


Μετά τη σύνδεση, ελέγξτε αν λειτουργεί ένα σύντομο ερώτημα SQL SELECT.


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Εργαλεία διαγνωστικών: Έλεγχος ανοιχτές θύρες


Ας υποθέσουμε ότι υποπτεύεστε ότι προσπάθειες σύνδεσης αποτυγχάνει λόγω ζητημάτων θύρα. Στον υπολογιστή σας, μπορείτε να εκτελέσετε ένα βοηθητικό πρόγραμμα που εκθέσεις σχετικά με τη ρύθμιση παραμέτρων θύρας.


Σε Linux τα ακόλουθα βοηθητικά προγράμματα μπορεί να είναι χρήσιμες:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (Αλλάξτε την τιμή παράδειγμα να είναι η διεύθυνση IP).


Στα Windows το [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) βοηθητικού προγράμματος μπορεί να είναι χρήσιμες. Ακολουθεί ένα παράδειγμα εκτέλεσης που ερώτημα με την κατάσταση θύρα διακομιστή βάσης δεδομένων SQL Azure και που εκτελέστηκε σε φορητό υπολογιστή:


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Διαγνωστικών: Τα σφάλματα καταγραφής


Μερικές φορές καλύτερα γίνεται διάγνωση ένα περιστασιακό πρόβλημα με την ανίχνευση ενός μοτίβου γενικά επάνω σε ημέρες ή εβδομάδες.


Το πρόγραμμα-πελάτη μπορούν να σας βοηθήσουν στη διάγνωση από την καταγραφή όλων των σφαλμάτων συναντά. Θα μπορεί να για να συσχετίσετε τις εγγραφές του αρχείου με βάση δεδομένων SQL Azure καταγράφει ίδια εσωτερικά δεδομένα σφάλματος.


Για μεγάλες επιχειρήσεις βιβλιοθήκη 6 (EntLib60) προσφέρει .NET διαχειριζόμενες κλάσεις για να σας βοηθήσει με την καταγραφή:

- [5 - τόσο εύκολη όσο η που υπάγονται απενεργοποίηση ένα αρχείο καταγραφής: χρήση του μπλοκ καταγραφή από την εφαρμογή](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Εργαλεία διαγνωστικών: Εξετάστε αρχεία καταγραφής συστήματος για σφάλματα


Εδώ θα βρείτε ορισμένες προτάσεις SELECT Transact-SQL που αρχείων καταγραφής ερωτημάτων σφάλματος και άλλες πληροφορίες.


| Ερώτημα του αρχείου καταγραφής | Περιγραφή |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` | Η προβολή [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) προσφέρει πληροφορίες σχετικά με μεμονωμένες συμβάντα, συμπεριλαμβανομένων κάποιων που μπορούν να προκαλέσουν σφάλματα μεταβατικές ή αποτυχίες συνδεσιμότητας.<br/><br/>Ιδανικά, μπορείτε να συσχετίσετε το **start_time** ή **end_time** τιμών με τις πληροφορίες σχετικά με το πότε το πρόγραμμα-πελάτης αντιμετώπισε προβλήματα.<br/><br/>**ΣΥΜΒΟΥΛΉ:** Πρέπει να συνδεθείτε με την **κύρια** βάση δεδομένων για την εκτέλεση. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` | Η προβολή [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) προσφέρει συγκεντρωτική πλήθος τύπους συμβάντων, για τα Διαγνωστικά του επιπλέον.<br/><br/>**ΣΥΜΒΟΥΛΉ:** Πρέπει να συνδεθείτε με την **κύρια** βάση δεδομένων για την εκτέλεση. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Εργαλεία διαγνωστικών: Αναζήτηση για το πρόβλημα συμβάντα στο αρχείο καταγραφής της βάσης δεδομένων SQL


Μπορείτε να αναζητήσετε καταχωρήσεις σχετικά με το πρόβλημα συμβάντα στο αρχείο καταγραφής της βάσης δεδομένων SQL Azure. Δοκιμάστε την ακόλουθη Transact-SQL SELECT στην **κύρια** βάση δεδομένων:


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Μερικές γραμμές επιστρέφεται από sys.fn_xe_telemetry_blob_target_read_file


Επόμενο τι μια γραμμή που επιστρέφεται θα είναι κάπως έτσι. Οι τιμές null εμφανίζονται συχνά δεν είναι null σε άλλες γραμμές.


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Βιβλιοθήκη Enterprise 6


Για μεγάλες επιχειρήσεις βιβλιοθήκη 6 (EntLib60) είναι ένα πλαίσιο κλάσεων .NET που σας βοηθά να υλοποιήσετε ισχυρό προγράμματα-πελάτες των υπηρεσιών cloud, μία από τις οποίες είναι η υπηρεσία βάσης δεδομένων SQL Azure. Μπορείτε να εντοπίσετε αποκλειστικά για κάθε περιοχή στην οποία μπορούν να σας βοηθήσουν EntLib60 μεταβαίνοντας πρώτα τα θέματα:

- [Βιβλιοθήκη Enterprise 6-Απρίλιος 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


Λογική "Επανάληψη" για το χειρισμό μεταβατικές σφάλματα είναι μία περιοχή στην οποία μπορούν να σας βοηθήσουν EntLib60:

- [4 - perseverance, μυστικό από όλες τις επιτυχίες τους σχετικά: χρήση του μπλοκ χειρισμού σφαλμάτων μεταβατικές εφαρμογής](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


> [AZURE.NOTE] Ο κωδικός προέλευσης για EntLib60 είναι διαθέσιμο για δημόσια [λήψη](http://go.microsoft.com/fwlink/p/?LinkID=290898). Η Microsoft έχει χωρίς προγράμματα για να κάνετε περαιτέρω ενημερώσεις δυνατοτήτων ή ενημερώσεις συντήρησης EntLib.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>EntLib60 κλάσεις για μεταβατικές σφάλματα και προσπαθήστε ξανά


Οι παρακάτω κλάσεις EntLib60 είναι ιδιαίτερα χρήσιμα για λογική "Επανάληψη". Όλα αυτά είναι ή είναι περαιτέρω κάτω από το χώρο ονομάτων **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*Στο χώρο ονομάτων* *Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling* *:*

- **RetryPolicy** κλάσης
 - Μέθοδος **ExecuteAction**


- **ExponentialBackoff** κλάσης


- **SqlDatabaseTransientErrorDetectionStrategy** κλάσης


- **ReliableSqlConnection** κλάσης
 - Μέθοδος **ExecuteCommand**


Στο χώρο ονομάτων **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** κλάσης

- **NeverTransientErrorDetectionStrategy** κλάσης


Εδώ θα βρείτε συνδέσεις για πληροφορίες σχετικά με το EntLib60:

- Δωρεάν [βιβλίου λήψη: Οδηγός για προγραμματιστές στη βιβλιοθήκη Microsoft για μεγάλες επιχειρήσεις, 2η έκδοση](http://www.microsoft.com/download/details.aspx?id=41145)

- Βέλτιστες πρακτικές: [Επανάληψη γενικές οδηγίες για τη](../best-practices-retry-general.md) περιλαμβάνει μια εξαιρετική λεπτομερής ανάλυση της λογικής "Επανάληψη".

- Λήψη NuGet της [Βιβλιοθήκης για μεγάλες επιχειρήσεις - μεταβατικές χειρισμού σφαλμάτων μπλοκ εφαρμογή 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Το μπλοκ καταγραφής


- Το μπλοκ καταγραφής είναι μια λύση εξαιρετικά ευέλικτη και με δυνατότητα ρύθμισης παραμέτρων που σας επιτρέπει να:
 - Δημιουργία και αποθήκευση αρχείου καταγραφής μηνυμάτων σε μια μεγάλη ποικιλία θέσεις.
 - Κατηγοριοποίηση και φιλτράρετε τα μηνύματα.
 - Συλλογή με βάση τα συμφραζόμενα πληροφοριών που είναι χρήσιμο για τον εντοπισμό σφαλμάτων και ανίχνευση, καθώς και για τον έλεγχο και γενικά καταγραφής απαιτήσεις.


- Το μπλοκ καταγραφής συνοψίζει τη λειτουργικότητα καταγραφής από τον προορισμό της καταγραφής, έτσι ώστε να είναι συνεπή, ανεξάρτητα από τη θέση και τον τύπο του χώρου αποθήκευσης προορισμού καταγραφής τον κώδικα της εφαρμογής.


Για λεπτομέρειες, ανατρέξτε: [5 - ως εύκολο ως που υπάγονται απενεργοποίηση ένα αρχείο καταγραφής: χρησιμοποιώντας το μπλοκ εφαρμογή καταγραφή](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient μέθοδο πηγαίου κώδικα


Στη συνέχεια, από την κλάση **SqlDatabaseTransientErrorDetectionStrategy** , είναι ο κωδικός προέλευσης C# για τη μέθοδο **IsTransient** . Ο κωδικός προέλευσης διευκρινίζει των σφαλμάτων έχουν θεωρούνται μεταβατικές και worthy της "Επανάληψη", το Απριλίου 2013.

Πολλές γραμμές **//comment** έχουν καταργηθεί από αυτό το αντίγραφο για να δώσετε έμφαση αναγνωσιμότητας.


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Επόμενα βήματα

- Για την αντιμετώπιση προβλημάτων άλλων κοινών προβλημάτων σύνδεσης βάσης δεδομένων SQL Azure, επισκεφθείτε την [Αντιμετώπιση προβλημάτων σύνδεσης με βάση δεδομένων SQL Azure](sql-database-troubleshoot-common-connection-issues.md).

- [Συγκέντρωση (ADO.NET) του SQL Server συνδέσεων](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*Retrying* είναι μια 2.0 Apache άδεια γενικής χρήσης επανάληψη βιβλιοθήκη, γραμμένο σε **Python**, για να απλοποιήσετε την εργασία της προσθήκης συμπεριφορά "Επανάληψη" για να σχεδόν σε οτιδήποτε.](https://pypi.python.org/pypi/retrying)
