
<properties 
    pageTitle="Πώς το Azure RemoteApp αποθήκευση δεδομένων χρήστη και τις ρυθμίσεις; | Microsoft Azure"
    description="Μάθετε πώς Azure RemoteApp αποθηκεύει τα δεδομένα των χρηστών με χρήση του δίσκου προφίλ χρήστη."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Πώς το Azure RemoteApp αποθήκευση δεδομένων χρήστη και τις ρυθμίσεις;

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Azure RemoteApp αποθηκεύει ταυτότητα χρήστη και τις προσαρμογές σε διάφορες συσκευές και περιόδους λειτουργίας. Αυτά τα δεδομένα χρήστη αποθηκεύονται σε ένα δίσκο ανά συλλογή ανά χρήστη, γνωστό ως ένα δίσκο προφίλ χρήστη (UPD). Ο δίσκος ακολουθεί το χρήστη και εξασφαλίζει ότι ο χρήστης έχει μια συνεπή εμπειρία, ανεξάρτητα από το σημείο όπου είσοδο σε. αποθηκεύει 

Δίσκων προφίλ χρήστη είναι απόλυτα διαφανής για το χρήστη — οι χρήστες αποθηκεύουν έγγραφα τους φάκελο έγγραφα (σε τι φαίνεται να είναι μια τοπική μονάδα δίσκου) και αλλαγή των ρυθμίσεων της εφαρμογής τους ως συνήθως. Την ίδια στιγμή, όλες τις προσωπικές ρυθμίσεις παραμένει κατά τη σύνδεση με Azure RemoteApp από οποιαδήποτε συσκευή. Μόνο ο χρήστης δεν βλέπει είναι τα δεδομένα στην ίδια θέση.

Κάθε UPD έχει 50GB μόνιμη χώρου αποθήκευσης και περιέχει και τις δύο ρυθμίσεις δεδομένων και των εφαρμογών του χρήστη. 

Διαβάστε παρακάτω για λεπτομέρειες σχετικά με δεδομένα προφίλ χρήστη.

>[AZURE.NOTE] Πρέπει να απενεργοποιήσετε το UPD; Μπορείτε να κάνετε αυτό τώρα - ελέγχου του Pavithra δημοσίευση ιστολογίου, [Απενεργοποίηση χρήστη προφίλ δίσκων (UPDs) στο Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/), για λεπτομέρειες.


## <a name="how-can-an-admin-get-to-the-data"></a>Πώς μπορεί ένας διαχειριστής πρόσβαση στα τα δεδομένα;

Εάν πρέπει να αποκτήσετε πρόσβαση στα δεδομένα για έναν από τους χρήστες σας (για αποκατάσταση ή εάν ο χρήστης αφήσει την εταιρεία), επικοινωνήστε με την υποστήριξη Azure και παρέχει πληροφορίες για τη συνδρομή για τη συλλογή και την ταυτότητα χρήστη. Η ομάδα Azure RemoteApp θα σας δώσει μια διεύθυνση URL στο VHD. Λήψη που VHD και να ανακτήσετε όλα τα έγγραφα ή αρχεία που χρειάζεστε. Σημειώστε ότι η VHD 50GB, έτσι θα διαρκέσει σε bit για τη λήψη της.


## <a name="is-the-data-backed-up"></a>Είναι αντίγραφα ασφαλείας των δεδομένων;

Ναι, αποθηκεύστε ένα αντίγραφο ασφαλείας των δεδομένων χρήστη ανά γεωγραφική θέση. Τα δεδομένα είναι μόνο για ανάγνωση και είναι δυνατή η πρόσβαση με τον ίδιο τρόπο όπως το κανονικό δεδομένων θα (επαφή Azure RemoteApp απόκτησης), εάν είναι το κέντρο δεδομένων πρωτεύοντος προς τα κάτω. Τα δεδομένα αντιγράφονται σε πραγματικό χρόνο για να τη θέση των αντιγράφων ασφαλείας και θα σας δεν διατηρεί αντίγραφα των διαφορετικών εκδόσεων. Επομένως, στην καταστροφή δεδομένων, θα σας δεν θα μπορείτε να το επαναφέρετε σε προηγουμένως γνωστό καλό έκδοση αλλά εάν είναι το κέντρο δεδομένων πρωτεύοντος προς τα κάτω, θα μπορούν να λαμβάνουν τα δεδομένα των χρηστών από την άλλη θέση.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Πώς οι χρήστες βλέπουν το UPD στην πλευρά του διακομιστή;

Κάθε χρήστης θα έχει το δικό τους καταλόγου στο διακομιστή που αντιστοιχίζει τους UPD: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Τι είναι ο καλύτερος τρόπος για να χρησιμοποιήσετε το Outlook και UPD;

Azure RemoteApp αποθηκεύει την κατάσταση του Outlook (γραμματοκιβώτια, pst) ανάμεσα στις περιόδους λειτουργίας. Για να ενεργοποιήσετε αυτή, πρέπει το αρχείο PST να είναι αποθηκευμένο στο παράθυρο δεδομένων προφίλ χρήστη (c:\users\<username >). Αυτή είναι η προεπιλεγμένη θέση για τα δεδομένα, επομένως, αρκεί να μην αλλάξετε τη θέση, τα δεδομένα θα παραμένει ανάμεσα στις περιόδους λειτουργίας.

Συνιστάται επίσης να χρησιμοποιήσετε τη λειτουργία "στο cache" στο Outlook και να χρησιμοποιήσετε λειτουργία "διακομιστή/online" για την αναζήτηση.

Δείτε [αυτό το άρθρο](remoteapp-outlook.md) για περισσότερες πληροφορίες σχετικά με τη χρήση του Outlook και το Azure RemoteApp.

## <a name="what-about-redirection"></a>Τι γίνεται με ανακατεύθυνση;
Μπορείτε να ρυθμίσετε Azure RemoteApp για να επιτρέψετε στους χρήστες πρόσβαση σε τοπική συσκευές με τη ρύθμιση της [ανακατεύθυνσης](remoteapp-redirection.md). Τοπικές συσκευές, στη συνέχεια, θα μπορείτε να πραγματοποιήσετε πρόσβαση στα δεδομένα του UPD.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Μπορώ να χρησιμοποιήσω μου UPD ως ένα κοινόχρηστο στοιχείο δικτύου;
Όχι. UPDs δεν μπορεί να χρησιμοποιηθεί ως ένα κοινόχρηστο στοιχείο δικτύου. Μια UPD διατίθεται μόνο στο χρήστη όταν ο χρήστης είναι συνδεδεμένος ενεργά Azure RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Εάν να διαγράψετε ένα χρήστη από μια συλλογή, διαγράφεται τους UPD;

Όχι, όταν διαγράφετε ένα χρήστη, θα σας δεν διαγράφει αυτόματα το UPD - αντί για αυτό, αποθηκεύει τα δεδομένα μέχρι να διαγράψετε τη συλλογή. 90 ημέρες μετά τη διαγραφή της συλλογής, διαγραφή όλων των UPDs. 

Εάν θέλετε να διαγράψετε μια UPD από μια συλλογή, επικοινωνήστε με Azure RemoteApp - θα σας να διαγράψετε UPD από την πλευρά.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Μπορώ να αποκτήσω πρόσβαση των χρηστών μου UPDs (είτε τρέχουσα ή διαγραφεί οι χρήστες);

Ναι, εάν επικοινωνείτε [Azure RemoteApp](mailto:remoteappforum@microsoft.com), θα σας να ρυθμίσετε μπορείτε με μια διεύθυνση URL για πρόσβαση στα δεδομένα. Θα έχετε περίπου 10 ώρες για να κάνετε λήψη οποιαδήποτε αρχεία δεδομένων ή από το UPD πριν από τη λήξη της access.

## <a name="are-upds-available-offline"></a>Είναι UPDs διαθέσιμα χωρίς σύνδεση;

Δεξιά τώρα δεν παρέχουμε πρόσβαση χωρίς σύνδεση για να UPDs, πέρα από το παράθυρο της access 10 ώρα που περιγράφονται παραπάνω. Αυτό σημαίνει ότι δεν αυτήν τη στιγμή έχουμε ένας τρόπος να σας παρέχει πρόσβαση για το χρονικό διάστημα αρκετό για να ολοκληρωθεί πιο σύνθετες εργασίες, όπως εκτέλεση λογισμικού προστασίας από ιούς στον το UPDs ή την πρόσβαση σε δεδομένα για ελέγχου.

## <a name="do-registry-key-settings-persist"></a>Ρυθμίσεις του κλειδιού μητρώου παραμένει;
Ναι, οτιδήποτε γραμμένο σε HKEY_Current_User αποτελεί μέρος του UPD.

## <a name="can-i-disable-upds-for-a-collection"></a>Μπορώ να απενεργοποιήσω UPDs για μια συλλογή;

Ναι, μπορείτε να ζητήσετε από Azure RemoteApp για να απενεργοποιήσετε την UPDs για μια συνδρομή, αλλά δεν μπορείτε να κάνετε αυτόν τον εαυτό σας. Αυτό σημαίνει ότι θα είναι απενεργοποιημένη UPDs για όλες τις συλλογές τη συνδρομή.

Ενδέχεται να θέλετε να απενεργοποιήσετε UPDs σε οποιαδήποτε από τις ακόλουθες περιπτώσεις: 

- Χρειάζεστε ολοκληρωμένη πρόσβαση και τον έλεγχο των δεδομένων χρήστη (για ελέγχου και αναθεώρηση σκοπούς όπως οικονομικών ιδρύματα).
- Έχετε χρήστη άλλου κατασκευαστή προφίλ Διαχείριση λύσεων εσωτερικής εγκατάστασης και θέλετε να συνεχίσετε να χρησιμοποιείτε τους στην ανάπτυξή σας Azure RemoteApp τομέα. Αυτό θα απαιτήσει τον παράγοντα προφίλ φόρτωση σε χρυσό εικόνας. 
- Δεν χρειάζεται οποιαδήποτε τοπική αποθήκευση δεδομένων ή έχετε όλα τα δεδομένα στην κοινή χρήση cloud ή αρχείο και θα θέλατε να ελέγχου Αποθήκευση των δεδομένων με χρήση τοπικά Azure RemoteApp.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Απενεργοποίηση χρήστη προφίλ δίσκων (UPDs) στο Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) .

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Μπορώ να εμποδίσετε τους χρήστες από την αποθήκευση δεδομένων στη μονάδα δίσκου συστήματος;

Ναι, αλλά θα χρειαστεί να ρυθμίσετε που στην εικόνα προτύπου πριν να δημιουργήσετε τη συλλογή. Χρησιμοποιήστε τα παρακάτω βήματα για να αποκλείσετε την πρόσβαση στη μονάδα δίσκου συστήματος:

1. Εκτελέστε **gpedit.msc** στην εικόνα του προτύπου.
2. Μεταβείτε στο **Ρυθμίσεις χρήστη > πρότυπα διαχείρισης > στοιχείων των Windows > Explorer**.
3. Επιλέξτε τις ακόλουθες επιλογές:
    - **Απόκρυψη των μονάδων που καθορίζονται στον υπολογιστή μου**
    - **Εμποδίζει την πρόσβαση σε μονάδες από τον υπολογιστή μου**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Μπορώ να σπείρεται UPDs; Θέλω να τοποθετήσετε ορισμένα δεδομένα στο το UPD που είναι διαθέσιμη η πρώτη φορά που ο χρήστης που πραγματοποιεί είσοδο.

Ναι, όταν δημιουργείτε την εικόνα του προτύπου, μπορείτε να προσθέσετε πληροφορίες στο προεπιλεγμένο προφίλ. Αυτά τα στοιχεία, στη συνέχεια, προστίθεται το UPD.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Μπορώ να αλλάξω το μέγεθος του το UPD ανάλογα με τον όγκο δεδομένων που θέλετε να αποθηκεύσετε;

Όχι, όλα UPDs έχετε 50 GB χώρου αποθήκευσης. Εάν θέλετε να αποθηκεύσετε διαφορετικές ποσότητες δεδομένων, δοκιμάστε τα εξής:

1. Απενεργοποίηση UPDs για τη συλλογή.
2. Ρυθμίστε ένα κοινόχρηστο αρχείο για τους χρήστες για να αποκτήσετε πρόσβαση.
3. Τοποθετήστε την κοινή χρήση αρχείων, χρησιμοποιώντας μια δέσμη ενεργειών εκκίνησης. Δείτε παρακάτω για λεπτομέρειες σχετικά με τις δέσμες ενεργειών εκκίνησης στο Azure RemoteApp.
4. Κοινή χρήση απευθείας τους χρήστες για την αποθήκευση όλων των δεδομένων για το αρχείο.


## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Πώς μπορώ να εκτελέσω μια δέσμη ενεργειών εκκίνησης στο Azure RemoteApp;

Εάν θέλετε να εκτελέσετε μια δέσμη ενεργειών εκκίνησης, ξεκινήστε με τη δημιουργία μιας προγραμματισμένης εργασίας στην εικόνα προτύπου που πρόκειται να χρησιμοποιήσετε για τη συλλογή. (Κάντε αυτό *πριν από την* εκτέλεση του sysprep.) 

![Δημιουργία μιας εργασίας συστήματος](./media/remoteapp-upd/upd1.png)

![Δημιουργία εργασίας συστήματος που θα εκτελείται όταν ένας χρήστης συνδέεται σε](./media/remoteapp-upd/upd2.png)

Στην καρτέλα **Γενικά** , φροντίστε να αλλάξετε το **Λογαριασμό χρήστη** στην περιοχή ασφάλεια για να στο "BUILTIN\Users".

![Αλλαγή του λογαριασμού χρήστη σε μια ομάδα](./media/remoteapp-upd/upd4.png)

Η προγραμματισμένη εργασία θα ξεκινήσει δέσμη ενεργειών εκκίνησης, χρησιμοποιώντας τα διαπιστευτήρια του χρήστη. Προγραμματισμός της εργασίας για να εκτελέσετε κάθε φορά ένας χρήστης συνδέεται.

![Ορίστε το έναυσμα για την εργασία, όπως "κατά τη σύνδεση"](./media/remoteapp-upd/upd3.png)

Μπορείτε επίσης να χρησιμοποιήσετε τις [δέσμες ενεργειών εκκίνησης βάσει πολιτικής ομάδας](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Τι γίνεται με τοποθετώντας μια δέσμη ενεργειών εκκίνησης στο μενού "Έναρξη"; Θα που λειτουργούν;

Με άλλα λόγια, μπορώ να δημιουργήσετε ένα αρχείο .bat που εκτελεί μια δέσμη ενεργειών ρύθμισης παραμέτρων παράθυρο και αποθηκεύστε το στο φάκελο Menu\Programs\StartUp c:\ProgramData\Microsoft\Windows\Start, και, στη συνέχεια, έχετε αυτή τη δέσμη ενεργειών εκτελείται κάθε φορά που ένας χρήστης ξεκινήσει μια περίοδο λειτουργίας RemoteApp;

Όχι, που δεν υποστηρίζεται με Azure RemoteApp, το οποίο χρησιμοποιεί RDSH, το οποίο επίσης δεν υποστηρίζει δέσμες ενεργειών εκκίνησης στο μενού "Έναρξη".

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Μπορώ να χρησιμοποιήσω mstsc.exe (το πρόγραμμα απομακρυσμένης επιφάνειας εργασίας) για να ρυθμίσετε τις παραμέτρους των δεσμών ενεργειών σύνδεσης;

Nope, δεν υποστηρίζεται από Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Μπορώ να αποθηκεύσετε δεδομένα σε η Εικονική τοπικά;

ΌΧΙ, δεδομένα που είναι αποθηκευμένα σε οποιοδήποτε σημείο στην την Εικονική εκτός από στα το UPD θα χαθούν. Υπάρχει υψηλή ευκαιρία ο χρήστης, δεν θα έχουν το ίδιο Εικονική την επόμενη φορά που δεν μπορούν να συνδεθούν στο Azure RemoteApp. Θα σας διατηρεί διατήρησης Εικονική χρήστη, ώστε ο χρήστης δεν θα συνδεθείτε στο το ίδιο Εικονική και τα δεδομένα θα χαθούν. Επιπλέον, όταν ενημερώνουμε τη συλλογή, την υπάρχουσα ΣΠΣ αντικαθίστανται με ένα νέο σύνολο ΣΠΣ - που σημαίνει ότι τα δεδομένα που είναι αποθηκευμένα στο την εικονική Μηχανή ίδια θα χαθούν. Η σύσταση είναι για την αποθήκευση δεδομένων στο το UPD, κοινόχρηστο χώρο αποθήκευσης, όπως Azure, ένα διακομιστή αρχείων μέσα σε μια VNET ή το στο cloud χρησιμοποιώντας ένα σύστημα χώρο αποθήκευσης στο cloud όπως το DropBox.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Πώς να να τοποθετήσετε ένα κοινόχρηστο αρχείο Azure σε μια Εικονική, χρησιμοποιώντας το PowerShell;

Μπορείτε να χρησιμοποιήσετε το cmdlet καθαρής PSDrive για να τοποθετήσετε τη μονάδα δίσκου, ως εξής:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Μπορείτε επίσης να αποθηκεύσετε τα διαπιστευτήριά σας, εκτελώντας τα εξής:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Που σας επιτρέπει να παραλείψετε - διαπιστευτηρίων παραμέτρου στο το cmdlet New-PSDrive.