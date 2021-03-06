<properties
    pageTitle="Γρήγορα αποτελέσματα με το συγχρονισμό δεδομένων βάσεις δεδομένων SQL"
    description="Αυτό το πρόγραμμα εκμάθησης σάς βοηθά να γρήγορα αποτελέσματα με το συγχρονισμό δεδομένων SQL Azure (έκδοση Preview)."
    services="sql-database"
    documentationCenter=""
    authors="jennieHubbard"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/11/2016"
    ms.author="jhubbard"/>


#<a name="getting-started-with-azure-sql-data-sync-preview"></a>Γρήγορα αποτελέσματα με το συγχρονισμό δεδομένων Azure SQL (έκδοση Preview)
Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε τα βασικά στοιχεία της συγχρονισμός δεδομένων SQL Azure με την πύλη κλασική Azure.

Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ελάχιστους εκ των προτέρων εμπειρία με το SQL Server και βάση δεδομένων SQL Azure. Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε μια ομάδα συγχρονισμού υβριδική (SQL Server και βάση δεδομένων SQL παρουσίες) πλήρως ρυθμιστεί και συγχρονισμό με το χρονοδιάγραμμα που ορίζετε.

> [AZURE.NOTE] Την πλήρη τεχνική τεκμηρίωση για συγχρονισμό δεδομένων Azure SQL, πρώην που βρίσκεται στο MSDN, είναι διαθέσιμη ως μια μορφή .pdf. Κάντε λήψη του [εδώ](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

## <a name="step-1-connect-to-the-azure-sql-database"></a>Βήμα 1: Σύνδεση με τη βάση δεδομένων SQL Azure

1. Είσοδος στην [πύλη κλασική](http://manage.windowsazure.com).

2. Στο αριστερό παράθυρο, κάντε κλικ στην επιλογή **Βάσεις ΔΕΔΟΜΈΝΩΝ SQL** .

3. Κάντε κλικ στο κουμπί **ΣΥΓΧΡΟΝΙΣΜΌΣ** στο κάτω μέρος της σελίδας. Όταν κάνετε κλικ στο κουμπί "ΣΥΓΧΡΟΝΙΣΜΌΣ", εμφανίζεται μια λίστα που εμφανίζει τις δυνατότητες που μπορείτε να προσθέσετε - **Νέα ομάδα συγχρονισμού** και **Νέα παράγοντα συγχρονισμού**.

4. Για να ξεκινήσετε τον Οδηγό νέου παράγοντα συγχρονισμού δεδομένων SQL, κάντε κλικ στην επιλογή **Δημιουργία παράγοντα συγχρονισμού**.

5. Εάν δεν έχετε προσθέσει παράγοντας πριν, **κάντε κλικ στην επιλογή λήψη του εδώ**.

    ![Image1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## <a name="step-2-add-a-client-agent"></a>Βήμα 2: Προσθήκη παράγοντα προγράμματος-πελάτη
Αυτό το βήμα απαιτείται μόνο αν πρόκειται να έχετε μια βάση δεδομένων SQL Server εσωτερικής εγκατάστασης περιλαμβάνονται στην ομάδα σας συγχρονισμού. Εάν η ομάδα σας συγχρονισμού έχει μόνο παρουσίες βάση δεδομένων SQL, προχωρήστε στο βήμα 4.

<a id="InstallRequiredSoftware"></a>
### <a name="step-2a-install-the-required-software"></a>Βήμα 2α: Εγκαταστήστε το απαιτούμενο λογισμικό
Βεβαιωθείτε ότι έχετε τα ακόλουθα εγκατεστημένα στον υπολογιστή όπου μπορείτε να εγκαταστήσετε τον παράγοντα προγράμματος-πελάτη.

- **.NET framework 4.0**

 Εγκαταστήστε το .NET Framework 4.0 από [εδώ](http://go.microsoft.com/fwlink/?linkid=205836).

- **Microsoft SQL Server 2008 R2 SP1 τύποι CLR του συστήματος (x86)**

 Εγκατάσταση του Microsoft SQL Server 2008 R2 SP1 τύποι CLR του συστήματος (x86) από [εδώ](http://www.microsoft.com/download/en/details.aspx?id=26728)

- **Microsoft SQL Server 2008 R2 SP1 κοινόχρηστα αντικείμενα διαχείρισης (x86)**

 Εγκατάσταση του Microsoft SQL Server 2008 R2 SP1 κοινόχρηστα διαχείρισης αντικειμένων (x86) από [εδώ](http://www.microsoft.com/download/en/details.aspx?id=26728)



<a id="InstallClient"></a>
### <a name="step-2b-install-a-new-client-agent"></a>Βήμα 2β: εγκατάσταση ενός νέου παράγοντα προγράμματος-πελάτη

Ακολουθήστε τις οδηγίες στο θέμα [εγκατάσταση έναν παράγοντα προγράμματος-πελάτη (Συγχρονισμός δεδομένων SQL)](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf) για να εγκαταστήσετε τον παράγοντα.



<a id="RegisterSSDb"></a>
### <a name="step-2c-finish-the-new-sql-data-sync-agent-wizard"></a>Βήμα 2c: ολοκλήρωση του Οδηγού νέου παράγοντα συγχρονισμού δεδομένων SQL

1.  Επιστρέψτε στον Οδηγό νέου παράγοντα συγχρονισμού δεδομένων SQL.
2.  Δώστε τον παράγοντα ένα χαρακτηριστικό όνομα.
3.  Από την αναπτυσσόμενη λίστα, επιλέξτε την **ΠΕΡΙΟΧΉ** (κέντρο δεδομένων) για τη φιλοξενία αυτός ο παράγοντας.
4.  Από την αναπτυσσόμενη λίστα, επιλέξτε τη **ΣΥΝΔΡΟΜΉ** για τη φιλοξενία αυτός ο παράγοντας.
5.  Κάντε κλικ στο βέλος δεξιά.



## <a name="step-3-register-a-sql-server-database-with-the-client-agent"></a>Βήμα 3: Καταχωρήσετε μια βάση δεδομένων SQL Server με τον παράγοντα προγράμματος-πελάτη

Μετά την εγκατάσταση του παράγοντα προγράμματος-πελάτη, η καταχώρηση κάθε βάση δεδομένων SQL Server εσωτερικής εγκατάστασης που σκοπεύετε να συμπεριλάβετε σε μια ομάδα συγχρονισμού με τον παράγοντα.
Για να καταχωρήσετε μια βάση δεδομένων με τον παράγοντα, ακολουθήστε τις οδηγίες στην [καταχώρηση μια βάση δεδομένων SQL Server με έναν παράγοντα προγράμματος-πελάτη](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).



## <a name="step-4-create-a-sync-group"></a>Βήμα 4: Δημιουργία ομάδας συγχρονισμού


<a id="StartNewSGWizard"></a>
### <a name="step-4a-start-the-new-sync-group-wizard"></a>Βήμα 4α: εκκίνηση του Οδηγού νέας ομάδας συγχρονισμού

1.  Επιστροφή στην [κλασική πύλη](http://manage.windowsazure.com).
2.  Κάντε κλικ στην επιλογή **βάσεις ΔΕΔΟΜΈΝΩΝ SQL**.
3.  Κάντε κλικ στην επιλογή **Προσθήκη ΣΥΓΧΡΟΝΙΣΜΟΎ** στο κάτω μέρος της σελίδας και, στη συνέχεια, επιλέξτε νέα ομάδα συγχρονισμού από το σχέδιο.

    ![Image2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### <a name="step-4b-enter-the-basic-settings"></a>Βήμα 4 β: Εισαγάγετε τις βασικές ρυθμίσεις


1.  Πληκτρολογήστε ένα χαρακτηριστικό όνομα για την ομάδα συγχρονισμού.
2.  Από την αναπτυσσόμενη λίστα, επιλέξτε την **ΠΕΡΙΟΧΉ** (κέντρο δεδομένων) για τη φιλοξενία αυτής της ομάδας συγχρονισμού.
3. Κάντε κλικ στο βέλος δεξιά.

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### <a name="step-4c-define-the-sync-hub"></a>Βήμα 4c: Ορισμός ενότητα συγχρονισμού

1. Από την αναπτυσσόμενη λίστα, επιλέξτε την παρουσία της βάσης δεδομένων SQL για να λειτουργήσει ως την ενότητα ομάδας συγχρονισμού.
2. Εισαγάγετε τα διαπιστευτήρια για αυτήν τη βάση δεδομένων SQL παρουσία - **ΔΙΑΝΟΜΈΑ όνομα ΧΡΉΣΤΗ** και **Τον κωδικό ΠΡΌΣΒΑΣΗΣ ΔΙΑΝΟΜΈΑ**.
3. Περιμένετε έως ότου συγχρονισμού δεδομένων SQL για να επιβεβαιώσετε το όνομα ΧΡΉΣΤΗ και κωδικό ΠΡΌΣΒΑΣΗΣ. Θα δείτε ένα πράσινο σημάδι ελέγχου που εμφανίζεται στα δεξιά του κωδικού ΠΡΌΣΒΑΣΗΣ όταν επιβεβαιώσει τα διαπιστευτήρια.
4. Από την αναπτυσσόμενη λίστα, επιλέξτε την πολιτική **ΕΠΊΛΥΣΗ ΔΙΕΝΈΞΕΩΝ** .

 **Κερδίζει διανομέα** - οποιαδήποτε αλλαγή γραμμένο σε την εγγραφή διανομέα βάσης δεδομένων στις βάσεις δεδομένων της αναφοράς, αντικατάσταση αλλαγές στην ίδια αναφορά εγγραφή στη βάση δεδομένων. Λειτουργικά, αυτό σημαίνει ότι στην πρώτη αλλαγή γραμμένο με το διανομέα μεταδίδει με τις άλλες βάσεις δεδομένων.


 **Κερδίζει προγράμματος-πελάτη** - αλλαγές γραμμένο με το διανομέα αντικαθίστανται από τις αλλαγές στις βάσεις δεδομένων αναφοράς. Λειτουργικά, αυτό σημαίνει ότι η τελευταία αλλαγή γραμμένο στο διανομέα αυτήν διατηρούνται και έχουν μεταδοθεί στις άλλες βάσεις δεδομένων.

5.  Κάντε κλικ στο βέλος δεξιά.

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### <a name="step-4d-add-a-reference-database"></a>Βήμα 4d: Προσθήκη βάσης δεδομένων αναφοράς


Επαναλάβετε αυτό το βήμα για κάθε επιπλέον βάση δεδομένων που θέλετε να προσθέσετε στην ομάδα συγχρονισμού.

1. Από την αναπτυσσόμενη λίστα, επιλέξτε τη βάση δεδομένων για να προσθέσετε.

    Βάσεις δεδομένων στην αναπτυσσόμενη λίστα περιλαμβάνουν δύο βάσεις δεδομένων SQL Server που έχουν καταχωρηθεί με τον παράγοντα και εμφανίσεις βάσης δεδομένων SQL.
2.  Εισαγάγετε τα διαπιστευτήρια για αυτήν τη βάση δεδομένων - **όνομα ΧΡΉΣΤΗ** και **τον κωδικό ΠΡΌΣΒΑΣΗΣ**.
3.  Από την αναπτυσσόμενη λίστα, επιλέξτε την **ΚΑΤΕΎΘΥΝΣΗ ΣΥΓΧΡΟΝΙΣΜΟΎ** για αυτήν τη βάση δεδομένων.

    **Διπλής κατεύθυνσης** - αλλαγές στη βάση δεδομένων αναφοράς είναι γραμμένο στη βάση δεδομένων του διανομέα και αλλαγές στη βάση δεδομένων του διανομέα γράφονται με τη βάση δεδομένων αναφοράς.

    **Συγχρονισμός από την ενότητα** - της βάσης δεδομένων να λαμβάνει ενημερώσεις από την ενότητα. Δεν αποστέλλει αλλαγές για την ενότητα.

    **Συγχρονισμός με την ενότητα** - η βάση δεδομένων στέλνει ενημερώσεις για την ενότητα. Αλλαγές στην ενότητα δεν εγγράφονται σε αυτήν τη βάση δεδομένων.

4.  Για να ολοκληρώσετε τη δημιουργία της ομάδας συγχρονισμού, κάντε κλικ στο σημάδι ελέγχου στην κάτω δεξιά γωνία του οδηγού. Περιμένετε έως ότου ο συγχρονισμός δεδομένων SQL για να επιβεβαιώσετε τα διαπιστευτήρια. Ένα πράσινο σημάδι ελέγχου υποδεικνύει ότι τα διαπιστευτήρια είναι επιβεβαιώθηκε.

5.  Κάντε κλικ στο σημάδι ελέγχου δεύτερη φορά. Επιστρέφει το στη σελίδα " **ΣΥΓΧΡΟΝΙΣΜΌΣ** " στην περιοχή βάσεις δεδομένων SQL. Αυτή η ομάδα συγχρονισμού εμφανίζεται τώρα με τις άλλες ομάδες συγχρονισμού και παραγόντων.

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## <a name="step-5-define-the-data-to-sync"></a>Βήμα 5: Ορίστε τα δεδομένα για να συγχρονίσετε

Συγχρονισμός δεδομένων Azure SQL σάς επιτρέπει να επιλέξετε πίνακες και στήλες για να συγχρονίσετε. Εάν θέλετε επίσης να φιλτράρετε μια στήλη, ώστε να που μόνο οι γραμμές με συγκεκριμένες τιμές (όπως, Age > = 65) συγχρονισμό, χρησιμοποιήστε την πύλη του συγχρονισμού δεδομένων SQL στο Azure και την τεκμηρίωση στο επιλέξτε το πινάκων, στηλών και γραμμών για συγχρονισμό για να προσδιορίσετε τα δεδομένα για να συγχρονίσετε.

1.  Επιστροφή στην [κλασική πύλη](http://manage.windowsazure.com).
2.  Κάντε κλικ στην επιλογή **βάσεις ΔΕΔΟΜΈΝΩΝ SQL**.
3.  Κάντε κλικ στην καρτέλα **ΣΥΓΧΡΟΝΙΣΜΌΣ** .
4.  Κάντε κλικ στο όνομα αυτής της ομάδας συγχρονισμού.
5.  Κάντε κλικ στην καρτέλα **ΚΑΝΌΝΕΣ ΣΥΓΧΡΟΝΙΣΜΟΎ** .
6.  Επιλέξτε τη βάση δεδομένων που θέλετε να παρέχετε το σχήμα ομάδα συγχρονισμού.
7.  Κάντε κλικ στο βέλος δεξιά.
8.  Κάντε κλικ στην επιλογή **ΑΝΑΝΈΩΣΗ ΣΧΉΜΑΤΟΣ**.
9.  Για κάθε πίνακα στη βάση δεδομένων, επιλέξτε τις στήλες για να συμπεριλάβετε στο το συγχρονισμών.
    - Είναι δυνατή η επιλογή στηλών με μη υποστηριζόμενους τύπους δεδομένων.
    - Εάν έχουν επιλεγεί χωρίς στήλες σε έναν πίνακα, ο πίνακας δεν περιλαμβάνεται στην ομάδα συγχρονισμού.
    - Για να καταργήσετε την επιλογή/επιλέξτε όλους τους πίνακες, κάντε κλικ στο κουμπί ΕΠΙΛΟΓΉ στο κάτω μέρος της οθόνης.
10. Κάντε κλικ στην επιλογή **ΑΠΟΘΉΚΕΥΣΗ**και, στη συνέχεια, περιμένετε για την ομάδα συγχρονισμού για να ολοκληρώσετε την προμήθεια του.
11. Για να επιστρέψετε στη σελίδα προορισμού συγχρονισμού δεδομένων, κάντε κλικ στο βέλος πίσω στο επάνω αριστερό μέρος της οθόνης (επάνω από το όνομα της ομάδας συγχρονισμού).

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## <a name="step-6-configure-your-sync-group"></a>Βήμα 6: Ρύθμιση των παραμέτρων σας ομάδα συγχρονισμού

Πάντα, μπορείτε να συγχρονίσετε μια ομάδα συγχρονισμού κάνοντας κλικ στην επιλογή ΣΥΓΧΡΟΝΙΣΜΟΎ στο κάτω μέρος της σελίδας προορισμού συγχρονισμού δεδομένων.
Για να συγχρονίσετε με το χρονοδιάγραμμα, μπορείτε να ρυθμίσετε την ομάδα συγχρονισμού.

1.  Επιστροφή στην [κλασική πύλη](http://manage.windowsazure.com).
2.  Κάντε κλικ στην επιλογή **βάσεις ΔΕΔΟΜΈΝΩΝ SQL**.
3.  Κάντε κλικ στην καρτέλα **ΣΥΓΧΡΟΝΙΣΜΌΣ** .
4.  Κάντε κλικ στο όνομα αυτής της ομάδας συγχρονισμού.
5.  Κάντε κλικ στην καρτέλα **Ρύθμιση ΠΑΡΑΜΈΤΡΩΝ** .
6.  **ΑΥΤΌΜΑΤΟ ΣΥΓΧΡΟΝΙΣΜΌ**
    - Για να ρυθμίσετε τις παραμέτρους της ομάδας συγχρονισμού για να συγχρονίσετε σε ένα σύνολο συχνότητα, κάντε κλικ στην επιλογή **Ενεργοποίηση**. Μπορείτε να συγχρονίσετε εξακολουθεί να ζήτηση κάνοντας κλικ στο κουμπί ΣΥΓΧΡΟΝΙΣΜΌΣ.
    - Κάντε κλικ στην επιλογή **ΑΝΕΝΕΡΓΌ** , για να ρυθμίσετε τις παραμέτρους της ομάδας συγχρονισμού για να συγχρονίσετε μόνο όταν κάνετε κλικ στο κουμπί ΣΥΓΧΡΟΝΙΣΜΌΣ.
7.  **ΣΥΧΝΌΤΗΤΑ ΣΥΓΧΡΟΝΙΣΜΟΎ**
    - Εάν ΑΥΤΌΜΑΤΟ ΣΥΓΧΡΟΝΙΣΜΌ είναι ΕΝΕΡΓΟΠΟΙΗΜΈΝΗ, ορίστε τη συχνότητα συγχρονισμού. Η συχνότητα πρέπει να είναι μεταξύ 5 λεπτά και 1 μήνας.
8.  Κάντε κλικ στην επιλογή **ΑΠΟΘΉΚΕΥΣΗ**.

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

Συγχαρητήρια. Έχετε δημιουργήσει μια ομάδα συγχρονισμού που περιλαμβάνει μια παρουσία της βάσης δεδομένων SQL και μια βάση δεδομένων SQL Server.

## <a name="next-steps"></a>Επόμενα βήματα
Για πρόσθετες πληροφορίες σχετικά με βάση δεδομένων SQL και συγχρονισμός δεδομένων SQL ανατρέξτε στο θέμα:

* [Λήψη τον πλήρη τεχνικό φάκελο συγχρονισμού δεδομένων SQL](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)
* [Επισκόπηση βάσης δεδομένων SQL](sql-database-technical-overview.md)
* [Διαχείριση κύκλου ζωής βάσης δεδομένων](https://msdn.microsoft.com/library/jj907294.aspx)
 

 
