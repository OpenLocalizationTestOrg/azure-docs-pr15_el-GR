<properties
    pageTitle="Πάντα κρυπτογραφημένα: Προστασία ευαίσθητα δεδομένα στη βάση δεδομένων SQL Azure με κρυπτογράφηση βάσης δεδομένων | Microsoft Azure"
    description="Προστασία ευαίσθητα δεδομένα στη βάση δεδομένων SQL σε λεπτά."
    keywords="κρυπτογράφηση δεδομένων, κρυπτογράφησης sql, κρυπτογράφηση βάσης δεδομένων, ευαίσθητα δεδομένα, πάντα κρυπτογραφημένα"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Πάντα κρυπτογράφηση: Προστασία ευαίσθητα δεδομένα στη βάση δεδομένων SQL και αποθήκευση των κλειδιών κρυπτογράφησης στο χώρο αποθήκευσης πιστοποιητικών των Windows

> [AZURE.SELECTOR]
- [Azure θάλαμο κλειδιού](sql-database-always-encrypted-azure-key-vault.md)
- [Πιστοποιητικό του Windows store](sql-database-always-encrypted.md)


Αυτό το άρθρο παρουσιάζει τον τρόπο για την ασφάλιση ευαίσθητα δεδομένα σε μια βάση δεδομένων SQL με κρυπτογράφηση βάσης δεδομένων με χρήση του [Πάντα οδηγού κρυπτογραφημένα](https://msdn.microsoft.com/library/mt459280.aspx) στο [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Σας δείχνει επίσης τον τρόπο αποθήκευσης των κλειδιών κρυπτογράφησης στο χώρο αποθήκευσης πιστοποιητικών των Windows.

Πάντα κρυπτογραφημένο είναι μια νέα τεχνολογία κρυπτογράφησης δεδομένων σε βάση δεδομένων SQL Azure και SQL Server που βοηθά στην προστασία ευαίσθητα δεδομένα στο υπόλοιπο στο διακομιστή, κατά την κίνησή μεταξύ του προγράμματος-πελάτη και διακομιστή και όταν τα δεδομένα είναι σε χρήση, εξασφάλιση ποτέ ευαίσθητα δεδομένα εμφανίζεται ως απλό κείμενο μέσα στο σύστημα βάσης δεδομένων. Μετά την κρυπτογράφηση δεδομένων, μόνο σε εφαρμογές προγράμματος-πελάτη ή εφαρμογή διακομιστές που έχουν πρόσβαση στα κλειδιά να αποκτήσετε πρόσβαση δεδομένων απλού κειμένου. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Πάντα κρυπτογραφημένα (του μηχανισμού βάσεων δεδομένων)](https://msdn.microsoft.com/library/mt163865.aspx).

Μετά τη ρύθμιση των παραμέτρων της βάσης δεδομένων για να χρησιμοποιήσετε κρυπτογράφηση πάντα, θα δημιουργήσετε μια εφαρμογή προγράμματος-πελάτη στο C# με το Visual Studio για να εργαστείτε με τα κρυπτογραφημένα δεδομένα.

Ακολουθήστε τα βήματα σε αυτό το άρθρο για να μάθετε πώς μπορείτε να ρυθμίσετε το πάντα κρυπτογράφηση μιας βάσης δεδομένων Azure SQL. Σε αυτό το άρθρο, θα μάθετε πώς να εκτελείτε τις ακόλουθες εργασίες:

- Χρησιμοποιήστε τον Οδηγό πάντα κρυπτογραφημένα στο SSMS για να δημιουργήσετε [Πάντα κρυπτογραφημένα κλειδιά](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
    - Δημιουργία [Στήλης πρωτεύοντος κλειδιού (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Δημιουργία [στήλης κλειδιού κρυπτογράφησης (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Δημιουργήστε έναν πίνακα βάσης δεδομένων και κρυπτογράφηση στήλες.
- Δημιουργήστε μια εφαρμογή που εισάγει, επιλέγει και εμφανίζει δεδομένα από το κρυπτογραφημένο στήλες.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για αυτό το πρόγραμμα εκμάθησης, θα πρέπει:

- Μια λογαριασμός Azure και τη συνδρομή. Εάν δεν έχετε ένα, εγγραφείτε για μια [δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) έκδοση 13.0.700.242 ή νεότερη έκδοση.
- [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) ή νεότερη έκδοση (του υπολογιστή-πελάτη).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).



## <a name="create-a-blank-sql-database"></a>Δημιουργήστε μια κενή βάση δεδομένων SQL
1. Είσοδος στην [πύλη του Azure](https://portal.azure.com/).
2. Κάντε κλικ στην επιλογή **νέα** > **δεδομένων + χώρος αποθήκευσης** > **βάση δεδομένων SQL**.
3. Δημιουργήστε μια **κενή** βάση δεδομένων με το όνομα **περιβάλλον** σε ένα νέο ή υπάρχον διακομιστή. Για λεπτομερείς οδηγίες σχετικά με τη δημιουργία μιας βάσης δεδομένων στην πύλη του Azure, ανατρέξτε στο θέμα [Δημιουργία μιας βάσης δεδομένων SQL σε λεπτά](sql-database-get-started.md).

    ![Δημιουργήστε μια κενή βάση δεδομένων](./media/sql-database-always-encrypted/create-database.png)

Θα χρειαστείτε τη συμβολοσειρά σύνδεσης αργότερα στην εκμάθηση. Μετά τη δημιουργία της βάσης δεδομένων, μεταβείτε στη νέα βάση δεδομένων περιβάλλον και αντιγράψτε τη συμβολοσειρά σύνδεσης. Μπορείτε να λάβετε τη συμβολοσειρά σύνδεσης ανά πάσα στιγμή, αλλά είναι εύκολο να το αντιγράψετε όταν βρίσκεστε στην πύλη του Azure.

1. Κάντε κλικ στην επιλογή **βάσεις δεδομένων SQL** > **περιβάλλον** > **Εμφάνιση συμβολοσειρές σύνδεσης βάσης δεδομένων**.
2. Αντιγράψτε τη συμβολοσειρά σύνδεσης για **ADO.NET**.

    ![Αντιγράψτε τη συμβολοσειρά σύνδεσης](./media/sql-database-always-encrypted/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Σύνδεση με τη βάση δεδομένων με SSMS

Ανοίξτε το SSMS και συνδεθείτε με το διακομιστή με τη βάση δεδομένων περιβάλλον.


1. Άνοιγμα SSMS. (Κάντε κλικ στην επιλογή **σύνδεση** > **Του μηχανισμού βάσεων δεδομένων** για να ανοίξετε το παράθυρο **σύνδεση με το διακομιστή** , εάν δεν είναι ανοιχτό).
2. Πληκτρολογήστε το όνομα του διακομιστή και τα διαπιστευτήρια. Μπορείτε να βρείτε το όνομα του διακομιστή του blade βάσης δεδομένων SQL και στη συμβολοσειρά σύνδεσης που αντιγράψατε προηγουμένως. Πληκτρολογήστε το πλήρες όνομα διακομιστή συμπεριλαμβανομένων *database.windows.net*.

    ![Αντιγράψτε τη συμβολοσειρά σύνδεσης](./media/sql-database-always-encrypted/ssms-connect.png)

Εάν ανοίξει το παράθυρο **Νέος κανόνας τείχους προστασίας** , πραγματοποιήστε είσοδο στο Azure και σας επιτρέπουν να δημιουργήσετε έναν νέο κανόνα τείχους προστασίας για εσάς SSMS.


## <a name="create-a-table"></a>Δημιουργία πίνακα

Σε αυτήν την ενότητα, θα δημιουργήσετε έναν πίνακα για να χωρέσει ασθενών δεδομένα. Αυτό θα είναι αρχικά--κανονική πίνακα που θα ρυθμίσετε τις παραμέτρους της κρυπτογράφησης στην επόμενη ενότητα.

1. Ανάπτυξη **βάσεις δεδομένων**.
1. Κάντε δεξί κλικ στη βάση δεδομένων **περιβάλλον** και κάντε κλικ στην επιλογή **Νέο ερώτημα**.
2. Επικολλήστε το εξής Transact-SQL (T-SQL) σε νέο παράθυρο του ερωτήματος και **Εκτέλεση** του.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Κρυπτογράφηση στήλες (ρύθμιση πάντα κρυπτογραφημένα)

SSMS παρέχει έναν οδηγό για να ρυθμίσετε εύκολα τις πάντα κρυπτογραφημένα κατά τη ρύθμιση του το CMK CEK και κρυπτογραφημένο στήλες για εσάς.

1. Ανάπτυξη **βάσεις δεδομένων** > **περιβάλλον** > **πίνακες**.
2. Κάντε δεξί κλικ στον πίνακα **ασθενών** και επιλέξτε **Κρυπτογράφηση στήλες** για να ανοίξετε τον Οδηγό πάντα κρυπτογραφημένα:

    ![Κρυπτογράφηση στηλών](./media/sql-database-always-encrypted/encrypt-columns.png)

Ο οδηγός πάντα κρυπτογραφημένα περιλαμβάνει τις ακόλουθες ενότητες: **Επιλογή στηλών**, **Ρύθμιση παραμέτρων πρωτεύον κλειδί** (CMK), **επικύρωσης**και **Σύνοψη**.

### <a name="column-selection"></a>Επιλογή στήλης ###

Κάντε κλικ στο κουμπί **Επόμενο** στη σελίδα " **Εισαγωγή** " για να ανοίξετε τη σελίδα **Επιλογή στηλών** . Σε αυτήν τη σελίδα, θα μπορείτε να επιλέξετε τις στήλες που θέλετε για την κρυπτογράφηση, [τον τύπο της κρυπτογράφησης, και τι στήλης κλειδιού κρυπτογράφησης (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) για να χρησιμοποιήσετε.

Κρυπτογράφηση **α.μ.κ.α** "και" **ημερομηνία γέννησης** πληροφοριών για κάθε υπομονή. Η στήλη **α.μ.κ.α** θα χρησιμοποιήσει ντετερμινιστική κρυπτογράφηση, η οποία υποστηρίζει ισότητας αναζητήσεις, οι σύνδεσμοι και ομαδοποίηση κατά. Η στήλη **"ημερομηνία γέννησης"** θα χρησιμοποιήσει τυχαία κρυπτογράφηση, η οποία δεν υποστηρίζει λειτουργίες.

Ορίστε τον **Τύπο κρυπτογράφησης** για τη στήλη **α.μ.κ.α** για να **Deterministic** και τη στήλη **"ημερομηνία γέννησης"** για να **Randomized**. Κάντε κλικ στο κουμπί **Επόμενο**.

![Κρυπτογράφηση στηλών](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Ρύθμιση παραμέτρων του πρωτεύοντος κλειδιού###

Η σελίδα **Ρύθμισης παραμέτρων πρωτεύον κλειδί** είναι όπου ρύθμιση CMK σας και επιλέξτε την υπηρεσία παροχής του χώρου αποθήκευσης κλειδιών όπου θα αποθηκευτεί το CMK. Προς το παρόν, μπορείτε να αποθηκεύσετε ένα CMK στο χώρο αποθήκευσης πιστοποιητικού των Windows, θάλαμο κλειδί Azure ή λειτουργικής μονάδας ασφαλείας υλικού (HSM). Αυτό το πρόγραμμα εκμάθησης δείχνει τον τρόπο για την αποθήκευση των αριθμών-κλειδιών στο χώρο αποθήκευσης πιστοποιητικών των Windows.

Βεβαιωθείτε ότι είναι επιλεγμένο το **Windows store πιστοποιητικό** και κάντε κλικ στο κουμπί **Επόμενο**.

![Ρύθμιση παραμέτρων του πρωτεύοντος κλειδιού](./media/sql-database-always-encrypted/master-key-configuration.png)


### <a name="validation"></a>Επικύρωση###

Μπορείτε να κρυπτογραφήσετε τώρα τις στήλες ή να αποθηκεύσετε μια δέσμη ενεργειών του PowerShell για να εκτελέσετε αργότερα. Για αυτό το πρόγραμμα εκμάθησης, επιλέξτε **συνέχεια για να ολοκληρώσετε τώρα** και κάντε κλικ στο κουμπί **Επόμενο**.

### <a name="summary"></a>Σύνοψη###

Βεβαιωθείτε ότι οι ρυθμίσεις είναι όλα διόρθωση και κάντε κλικ στο κουμπί **Τέλος** για να ολοκληρώσετε τη ρύθμιση για πάντα κρυπτογραφημένα.

![Σύνοψη](./media/sql-database-always-encrypted/summary.png)


### <a name="verify-the-wizards-actions"></a>Επαλήθευση του οδηγού ενέργειες

Μετά την ολοκλήρωση του οδηγού, βάση δεδομένων σας έχει ρυθμιστεί για πάντα κρυπτογραφημένα. Ο οδηγός έχει πραγματοποιηθεί τις ακόλουθες ενέργειες:

- Δημιουργία μιας CMK.
- Δημιουργία μιας CEK.
- Ρύθμιση παραμέτρων τις επιλεγμένες στήλες για την κρυπτογράφηση. Ο πίνακας **ασθενών** έχει αυτήν τη στιγμή δεν υπάρχουν δεδομένα, αλλά υπάρχουν δεδομένα στις επιλεγμένες στήλες είναι πλέον κρυπτογραφημένα.

Μπορείτε να επαληθεύσετε τη δημιουργία των κλειδιών στο SSMS μεταβαίνοντας στο **περιβάλλον** > **ασφαλείας** > **Πάντα πλήκτρα κρυπτογραφημένα**. Τώρα μπορείτε να δείτε τα νέα κλειδιά που δημιούργησε ο οδηγός για εσάς.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Δημιουργήστε μια εφαρμογή προγράμματος-πελάτη που λειτουργεί με το κρυπτογραφημένων δεδομένων

Τώρα που έχει ρυθμιστεί η πάντα κρυπτογραφημένα, μπορείτε να δημιουργήσετε μια εφαρμογή που εκτελεί *εισάγει* και *επιλέγει* σε κρυπτογραφημένο στηλών. Για να εκτελέσετε με επιτυχία το δείγμα εφαρμογής, πρέπει να εκτελέσετε το στον ίδιο υπολογιστή όπου εκτελέσατε τον Οδηγό πάντα κρυπτογραφημένα. Για να εκτελέσετε την εφαρμογή σε άλλον υπολογιστή, πρέπει να το αναπτύξετε πιστοποιητικών πάντα κρυπτογραφημένα στον υπολογιστή που εκτελεί την εφαρμογή υπολογιστή-πελάτη.  

> [AZURE.IMPORTANT] Η εφαρμογή σας πρέπει να χρησιμοποιήσετε αντικείμενα [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) κατά τη διέλευση δεδομένα απλού κειμένου στο διακομιστή με πάντα κρυπτογραφημένα στήλες. Διέρχεται ακριβές τιμές χωρίς τη χρήση αντικειμένων SqlParameter θα έχει ως αποτέλεσμα μια εξαίρεση.


1. Ανοίξτε το Visual Studio και να δημιουργήσετε μια νέα εφαρμογή κονσόλας C#. Βεβαιωθείτε ότι το έργο σας έχει οριστεί σε **.NET Framework 4.6** ή νεότερη έκδοση.
2. Ονομάστε το έργο **AlwaysEncryptedConsoleApp** και κάντε κλικ στο κουμπί **OK**.

![Νέα εφαρμογή κονσόλας](./media/sql-database-always-encrypted/console-app.png)



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Τροποποιήστε το συμβολοσειρά σύνδεσης για να ενεργοποιήσετε την κρυπτογράφηση πάντα

Αυτή η ενότητα εξηγεί τον τρόπο για να ενεργοποιήσετε την κρυπτογράφηση πάντα στη συμβολοσειρά σύνδεσης βάσης δεδομένων. Θα μπορείτε να τροποποιήσετε την εφαρμογή κονσόλας που μόλις δημιουργήσατε στην επόμενη ενότητα, "Πάντα κρυπτογραφημένο εφαρμογής κονσόλας δείγμα".


Για να ενεργοποιήσετε πάντα κρυπτογραφημένα, χρειάζεστε για να προσθέσετε τη λέξη-κλειδί **Ρύθμιση κρυπτογράφησης στήλη** συμβολοσειρά σύνδεσης και να ορίσετε την επιλογή **ενεργοποιημένη**.

Μπορείτε να το ορίσετε απευθείας στη συμβολοσειρά σύνδεσης ή μπορείτε να τη ρυθμίσετε με τη χρήση μιας [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Το δείγμα εφαρμογής στην επόμενη ενότητα δείχνει πώς μπορείτε να χρησιμοποιήσετε **SqlConnectionStringBuilder**.

> [AZURE.NOTE] Αυτή είναι η μόνη αλλαγή απαιτείται σε μια εφαρμογή προγράμματος-πελάτη που είναι συγκεκριμένες για πάντα κρυπτογραφημένα. Εάν έχετε μια υπάρχουσα εφαρμογή όπου αποθηκεύονται τα εξωτερικά τη συμβολοσειρά σύνδεσης (δηλαδή, σε ένα αρχείο ρύθμισης παραμέτρων), ίσως είστε σε θέση να ενεργοποιήσετε πάντα κρυπτογραφημένα χωρίς να αλλάξετε οποιονδήποτε κωδικό.


### <a name="enable-always-encrypted-in-the-connection-string"></a>Ενεργοποίηση πάντα κρυπτογραφημένα στη συμβολοσειρά σύνδεσης

Προσθέστε την ακόλουθη λέξη-κλειδί σας συμβολοσειρά σύνδεσης:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Ενεργοποίηση πάντα κρυπτογραφημένα με μια SqlConnectionStringBuilder

Ο ακόλουθος κώδικας εμφανίζει τον τρόπο ενεργοποίησης πάντα κρυπτογραφημένα, ορίζοντας την [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) [ενεργοποιημένη](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Πάντα κρυπτογραφημένο εφαρμογής κονσόλας δείγματος

Αυτό το δείγμα παρουσιάζει πώς μπορείτε να:

- Τροποποίηση συμβολοσειρά σύνδεσης για να ενεργοποιήσετε την πάντα κρυπτογραφημένα.
- Εισαγωγή δεδομένων σε κρυπτογραφημένο στηλών.
- Επιλέξτε μια εγγραφή με την εφαρμογή φίλτρου για μια συγκεκριμένη τιμή σε μια στήλη κρυπτογραφημένο.

Αντικαταστήστε τα περιεχόμενα του **Program.cs** με τον ακόλουθο κώδικα. Αντικαταστήστε τη συμβολοσειρά σύνδεσης για τη μεταβλητή συμβολοσειρά καθολικής σύνδεσης στη γραμμή ακριβώς επάνω από τη μέθοδο κύριες με σας έγκυρη συμβολοσειρά σύνδεσης από την πύλη του Azure. Αυτή είναι η μόνη αλλαγή που πρέπει να κάνετε σε αυτόν τον κωδικό.

Εκτελέστε την εφαρμογή για να δείτε κρυπτογραφημένα πάντα στην πράξη.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
         VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }


## <a name="verify-that-the-data-is-encrypted"></a>Βεβαιωθείτε ότι τα δεδομένα είναι κρυπτογραφημένα

Μπορείτε γρήγορα να ελέγξετε ότι τα πραγματικά δεδομένα στο διακομιστή είναι κρυπτογραφημένα κατά την υποβολή ερωτημάτων τα δεδομένα **ασθενείς** με SSMS. (Χρησιμοποιήστε την τρέχουσα σύνδεσή σας όπου η ρύθμιση κρυπτογράφησης στήλης δεν είναι ακόμη ενεργοποιημένη.)

Εκτελέστε το ακόλουθο ερώτημα στη βάση δεδομένων περιβάλλον.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Μπορείτε να δείτε ότι το κρυπτογραφημένο στήλες δεν περιέχουν δεδομένα απλού κειμένου.

   ![Νέα εφαρμογή κονσόλας](./media/sql-database-always-encrypted/ssms-encrypted.png)


Για να χρησιμοποιήσετε SSMS για να αποκτήσετε πρόσβαση στα δεδομένα απλού κειμένου, μπορείτε να προσθέσετε το **ρύθμιση κρυπτογράφησης στήλη = με δυνατότητα** παραμέτρου για τη σύνδεση.

1. Στο SSMS, κάντε δεξιό κλικ στο διακομιστή σας στην **Εξερεύνηση αντικείμενο**και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποσύνδεση**.
2. Κάντε κλικ στην επιλογή **σύνδεση** > **Του μηχανισμού βάσεων δεδομένων** για να ανοίξετε το παράθυρο **σύνδεση με το διακομιστή** και, στη συνέχεια, κάντε κλικ στο κουμπί **Επιλογές**.
3. Κάντε κλικ στην επιλογή **Πρόσθετες παράμετροι σύνδεσης** και πληκτρολογήστε **ρύθμιση κρυπτογράφησης στήλη = με δυνατότητα**.

    ![Νέα εφαρμογή κονσόλας](./media/sql-database-always-encrypted/ssms-connection-parameter.png)

4. Εκτελέστε το ακόλουθο ερώτημα στη βάση δεδομένων **περιβάλλον** .

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Τώρα μπορείτε να δείτε τα δεδομένα απλού κειμένου στις στήλες κρυπτογραφημένο.


    ![Νέα εφαρμογή κονσόλας](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [AZURE.NOTE] Εάν συνδέεστε με SSMS (ή οποιονδήποτε υπολογιστή-πελάτη) από έναν άλλο υπολογιστή, δεν θα έχουν πρόσβαση στα κλειδιά κρυπτογράφησης και δεν θα μπορείτε να αποκρυπτογραφήσετε τα δεδομένα.



## <a name="next-steps"></a>Επόμενα βήματα
Αφού δημιουργήσετε μια βάση δεδομένων που χρησιμοποιεί κρυπτογράφηση πάντα, ίσως θελήσετε να κάνετε τα εξής:

- Εκτέλεση αυτού του δείγματος από έναν άλλο υπολογιστή. Αυτό δεν θα έχουν πρόσβαση σε κλειδιών κρυπτογράφησης, ώστε δεν θα έχουν πρόσβαση στα δεδομένα απλού κειμένου και δεν θα εκτελεστεί με επιτυχία.
- [Περιστροφή και εκκαθάριση των αριθμών-κλειδιών](https://msdn.microsoft.com/library/mt607048.aspx).
- [Μετεγκατάσταση δεδομένων που είναι ήδη κρυπτογραφημένα με πάντα κρυπτογραφημένα](https://msdn.microsoft.com/library/mt621539.aspx).
- [Πάντα ανάπτυξη κρυπτογραφημένα πιστοποιητικά στους άλλους υπολογιστές-πελάτες](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (ανατρέξτε στην ενότητα "Πραγματοποίηση πιστοποιητικά διαθέσιμες σε εφαρμογές και Χρήστες").

## <a name="related-information"></a>Σχετικές πληροφορίες

- [Πάντα κρυπτογραφημένα (ανάπτυξη προγράμματος-πελάτη)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Κρυπτογράφηση διαφανή δεδομένων](https://msdn.microsoft.com/library/bb934049.aspx)
- [SQL Server κρυπτογράφησης](https://msdn.microsoft.com/library/bb510663.aspx)
- [Πάντα κρυπτογραφημένο οδηγού](https://msdn.microsoft.com/library/mt459280.aspx)
- [Πάντα κρυπτογραφημένο ιστολογίου](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
