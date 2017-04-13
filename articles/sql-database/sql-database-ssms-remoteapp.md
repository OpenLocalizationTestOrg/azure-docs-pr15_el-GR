<properties
    pageTitle="Σύνδεση με βάση δεδομένων SQL χρησιμοποιώντας SQL Server Management Studio στο Azure RemoteApp | Microsoft Azure"
    description="Χρησιμοποιήστε αυτό το πρόγραμμα εκμάθησης για να μάθετε πώς να χρησιμοποιείτε το SQL Server Management Studio στο Azure RemoteApp για την ασφάλεια και επιδόσεων κατά τη σύνδεση με βάση δεδομένων SQL"
    services="sql-database"
    documentationCenter=""
    authors="adhurwit"
    manager="jhubbard"/>

<tags
    ms.service="sql-database"
    ms.workload="data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>Χρήση του SQL Server Management Studio στο Azure RemoteApp για σύνδεση με βάση δεδομένων SQL

## <a name="introduction"></a>Εισαγωγή  
Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε SQL Server Management Studio (SSMS) για σύνδεση με βάση δεδομένων SQL Azure RemoteApp. Σας καθοδηγεί κατά τη διαδικασία τη ρύθμιση του SQL Server Management Studio στο Azure RemoteApp, εξηγεί τα πλεονεκτήματα και εμφανίζει δυνατότητες ασφαλείας που μπορείτε να χρησιμοποιήσετε στο Azure Active Directory.

**Εκτιμώμενη χρόνο για να ολοκληρωθεί:** 45 λεπτά

## <a name="ssms-in-azure-remoteapp"></a>SSMS στο Azure RemoteApp

Azure RemoteApp είναι μια υπηρεσία RDS στο Azure που προσφέρει εφαρμογές. Να μάθετε περισσότερα σχετικά με το εδώ: [Τι είναι το RemoteApp;](../remoteapp/remoteapp-whatis.md)

SSMS εκτελείται στο Azure RemoteApp σάς παρέχει την ίδια εμπειρία με την εκτέλεση SSMS τοπικά.

![Στιγμιότυπο οθόνης που εμφανίζει SSMS εκτελείται στο Azure RemoteApp][1]



## <a name="benefits"></a>Πλεονεκτήματα

Υπάρχουν πολλά οφέλη από τη χρήση SSMS στο Azure RemoteApp, όπως:

- Θύρα 1433 Azure SQL Server δεν έχει να εμφανίζονται εξωτερικά (εκτός του Azure).
- Χωρίς να χρειάζεται να διατηρήσετε Προσθήκη και κατάργηση διευθύνσεις IP στο τείχος προστασίας Azure SQL Server.
- Όλες οι συνδέσεις Azure RemoteApp προκύψουν μέσω HTTPS σχετικά με τη χρήση θύρα 443 κρυπτογραφημένα πρωτόκολλο απομακρυσμένης επιφάνειας εργασίας
- Είναι πολλών χρηστών και να περιορίσετε το μέγεθος.
- Υπάρχει κέρδος απόδοσης την SSMS στην ίδια περιοχή ως τη βάση δεδομένων SQL.
- Μπορείτε να ελέγχετε χρήση του Azure RemoteApp με την έκδοση Premium του Azure Active Directory που έχει αναφορές δραστηριότητας χρήστη.
- Μπορείτε να ενεργοποιήσετε τον έλεγχο ταυτότητας πολλών παραγόντων (MFA).
- Πρόσβαση SSMS σε οποιοδήποτε σημείο όταν χρησιμοποιείτε οποιαδήποτε από αυτά τα υποστηριζόμενα προγράμματα Azure RemoteApp που περιλαμβάνει iOS, Android, Mac, Windows Phone και του Υπολογιστή με Windows.


## <a name="create-the-azure-remoteapp-collection"></a>Δημιουργία της συλλογής Azure RemoteApp

Ακολουθούν τα βήματα για να δημιουργήσετε τη συλλογή Azure RemoteApp με SSMS:


### <a name="1-create-a-new-windows-vm-from-image"></a>1. Δημιουργήστε μια νέα Εικονική Windows από εικόνα
Χρησιμοποιήστε την εικόνα "Windows Server απομακρυσμένης επιφάνειας εργασίας περιόδου λειτουργίας κεντρικού υπολογιστή Windows Server 2012 R2" από τη συλλογή για να κάνετε το νέο Εικονική.


### <a name="2-install-ssms-from-sql-express"></a>2. εγκατάσταση SSMS από SQL Express

Μεταβείτε στο τη νέα Εικονική και περιηγηθείτε σε αυτήν τη σελίδα λήψης: [Express Microsoft® SQL Server® 2014](https://www.microsoft.com/en-us/download/details.aspx?id=42299)

Υπάρχει επιλογή για λήψη μόνο SSMS. Μετά τη λήψη, μεταβείτε στον κατάλογο εγκατάσταση και εκτέλεση του προγράμματος εγκατάστασης για να εγκαταστήσετε το SSMS.

Επίσης, πρέπει να εγκαταστήσετε το SQL Server 2014 Service Pack 1. Μπορείτε να το αποκτήσετε εδώ: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/en-us/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 περιλαμβάνει βασικές λειτουργίες για την εργασία με βάση δεδομένων SQL Azure.


### <a name="3-run-validate-script-and-sysprep"></a>3. Εκτελέστε επικύρωση δέσμης ενεργειών και Sysprep

Στην επιφάνεια εργασίας από την εικονική Μηχανή μια δέσμη ενεργειών PowerShell ονομάζεται επικύρωση. Εκτελέστε αυτή κάνοντας διπλό κλικ. Επαληθεύει ότι η Εικονική είναι έτοιμος να χρησιμοποιηθεί για τη φιλοξενία remote με τις εφαρμογές του. Μόλις ολοκληρωθεί η επαλήθευση, αυτό θα σας ζητήσει να εκτελέσετε το sysprep - επιλογή για να το εκτελέσετε.

Όταν ολοκληρωθεί το sysprep, θα τερματιστεί η Εικονική.

Για να μάθετε περισσότερα σχετικά με τη δημιουργία μιας εικόνας Azure RemoteApp, ανατρέξτε στο θέμα: [πώς μπορείτε να δημιουργήσετε μια εικόνα προτύπου RemoteApp στο Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)


### <a name="4-capture-image"></a>4. καταγράψετε εικόνα

Όταν η Εικονική σταμάτησε να λειτουργεί, εντοπίστε την στην πύλη του τρέχοντος και την καταγραφή.

Για να μάθετε περισσότερα σχετικά με την καταγραφή μιας εικόνας, ανατρέξτε στο θέμα [καταγράψετε μια εικόνα από μια εικονική μηχανή Azure Windows που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης](../virtual-machines/virtual-machines-windows-classic-capture-image.md)


### <a name="5-add-to-azure-remoteapp-template-images"></a>5. Προσθέστε εικόνες των προτύπων RemoteApp Azure

Στην ενότητα Azure RemoteApp της τρέχουσας πύλης, μεταβείτε στην καρτέλα εικόνες των προτύπων και κάντε κλικ στην επιλογή Προσθήκη. Στο αναδυόμενο πλαίσιο, επιλέξτε "Εισαγωγή εικόνας από τη βιβλιοθήκη σας εικονικές μηχανές" και, στη συνέχεια, επιλέξτε την εικόνα που μόλις δημιουργήσατε.



### <a name="6-create-cloud-collection"></a>6. Δημιουργία συλλογής cloud

Στην πύλη του τρέχοντος, δημιουργήστε μια νέα συλλογή Cloud RemoteApp Azure. Επιλέξτε την εικόνα με το πρότυπο που έχετε εισαγάγει με εγκατεστημένο το SSMS.

![Δημιουργία νέας συλλογής cloud][2]


### <a name="7-publish-ssms"></a>7. δημοσίευση SSMS

Στη δημοσίευση καρτέλα του της νέας συλλογής cloud, επιλέξτε δημοσίευση μιας εφαρμογής από το μενού Έναρξη και, στη συνέχεια, επιλέξτε SSMS από τη λίστα.

![Δημοσίευση εφαρμογής][5]

### <a name="8-add-users"></a>8. Προσθήκη χρηστών

Στην καρτέλα πρόσβασης των χρηστών, μπορείτε να επιλέξετε τους χρήστες που θα έχουν πρόσβαση σε αυτήν τη συλλογή Azure RemoteApp που περιλαμβάνει μόνο SSMS.

![Προσθήκη χρήστη][6]


### <a name="9-install-the-azure-remoteapp-client-application"></a>9. Εγκαταστήστε την εφαρμογή-πελάτη Azure RemoteApp

Μπορείτε να κάνετε λήψη και εγκατάσταση ενός προγράμματος-πελάτη του Azure RemoteApp εδώ: [λήψης | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)



## <a name="configure-azure-sql-server"></a>Ρύθμιση παραμέτρων του διακομιστή Azure SQL

Είναι η μόνη ρύθμιση που απαιτείται για να βεβαιωθείτε ότι είναι ενεργοποιημένη υπηρεσιών Azure για το τείχος προστασίας. Εάν χρησιμοποιείτε αυτήν τη λύση, στη συνέχεια, δεν χρειάζεται να προσθέσετε τις διευθύνσεις IP για να ανοίξετε το τείχος προστασίας. Η κυκλοφορία του δικτύου που επιτρέπεται για τον SQL Server είναι από άλλες υπηρεσίες Azure.


![Να επιτρέπεται Azure][4]



## <a name="multi-factor-authentication-mfa"></a>Έλεγχος ταυτότητας πολλών παραγόντων (MFA)

MFA μπορεί να ενεργοποιηθεί για αυτήν την εφαρμογή συγκεκριμένα. Μεταβείτε στην καρτέλα εφαρμογές από το Azure Active Directory. Θα βρείτε μια καταχώρηση για το Microsoft Azure RemoteApp. Εάν κάνετε κλικ αυτήν την εφαρμογή και, στη συνέχεια, ρύθμιση παραμέτρων, θα δείτε το παρακάτω σελίδα όπου μπορείτε να ενεργοποιήσετε την MFA για αυτήν την εφαρμογή.

![Ενεργοποίηση MFA][3]



## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Έλεγχος δραστηριότητας χρήστη με το Azure Active Directory Premium

Εάν δεν έχετε Azure AD Premium, στη συνέχεια, πρέπει να την ενεργοποιήσετε στην ενότητα άδειες χρήσης του καταλόγου σας. Με Premium ενεργοποιημένο, μπορείτε να εκχωρήσετε στους χρήστες στο επίπεδο Premium.

Όταν μεταβαίνετε σε ένα χρήστη στο σας Azure Active Directory, στη συνέχεια, μπορείτε να μεταβείτε στην καρτέλα δραστηριότητας για να δείτε πληροφορίες σύνδεσης για να Azure RemoteApp.



## <a name="next-steps"></a>Επόμενα βήματα

Μετά την ολοκλήρωση όλα τα παραπάνω βήματα, θα μπορείτε να εκτελέσετε το πρόγραμμα-πελάτη Azure RemoteApp και καταγραφής στο με ένα χρήστη που έχει αντιστοιχιστεί. Θα εμφανιστεί με SSMS ως μία από τις εφαρμογές σας και μπορείτε να το εκτελέσετε όπως θα κάνατε εάν έχουν εγκατεστημένο στον υπολογιστή σας με πρόσβαση σε Azure SQL Server.

Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να κάνετε τη σύνδεση με βάση δεδομένων SQL, ανατρέξτε στο θέμα [σύνδεση με βάση δεδομένων SQL με SQL Server Management Studio και να εκτελέσετε ένα ερώτημα T SQL δείγμα](sql-database-connect-query-ssms.md).


Που είναι όλα τα στοιχεία προς το παρόν. Απολαύστε!



<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png
