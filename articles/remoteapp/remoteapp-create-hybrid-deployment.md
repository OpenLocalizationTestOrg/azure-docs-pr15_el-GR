<properties
    pageTitle="Πώς μπορείτε να δημιουργήσετε μια συλλογή υβριδική για Azure RemoteApp | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια ανάπτυξη του RemoteApp που συνδέεται με το εσωτερικό σας δίκτυο."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo"/>

# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>Πώς μπορείτε να δημιουργήσετε μια συλλογή υβριδική για Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Υπάρχουν δύο είδη Azure RemoteApp συλλογές:

- Cloud: πλήρως βρίσκεται στο Azure. Μπορείτε να επιλέξετε την αποθήκευση όλων των δεδομένων στο cloud (, ώστε μια συλλογή μόνο στο cloud) ή για να συνδεθείτε στη συλλογή ένα VNET και αποθήκευση δεδομένων εκεί.   
- Υβριδική: περιλαμβάνει ένα εικονικό δίκτυο για πρόσβαση στην εσωτερική εγκατάσταση - απαιτεί τη χρήση του Azure AD και ένα περιβάλλον υπηρεσίας καταλόγου Active Directory εσωτερικής εγκατάστασης.

Δεν γνωρίζετε που χρειάζεστε; Δείτε [το είδος της συλλογής χρειάζεται για Azure RemoteApp](remoteapp-collections.md).

Αυτό το πρόγραμμα εκμάθησης σας καθοδηγεί στη διαδικασία δημιουργίας μιας συλλογής υβριδική. Υπάρχουν οκτώ βήματα:

1.  Αποφασίστε ποια [εικόνα](remoteapp-imageoptions.md) που θα χρησιμοποιηθεί για τη συλλογή. Μπορείτε να δημιουργήσετε μια προσαρμοσμένη εικόνα ή να χρησιμοποιήσετε μία από τις εικόνες της Microsoft που περιλαμβάνονται με τη συνδρομή σας.
2. Ρύθμιση του δικτύου σας εικονικού. Ανατρέξτε στο θέμα πληροφορίες για το [σχεδιασμό VNET](remoteapp-planvnet.md) και [Αλλαγή μεγέθους](remoteapp-vnetsizing.md) .
2.  Δημιουργία συλλογής.
2.  Συμμετοχή σε συλλογή σας στον τοπικό τομέα σας.
3.  Προσθέστε μια εικόνα προτύπου στη συλλογή σας.
4.  Ρύθμιση παραμέτρων συγχρονισμού καταλόγου. Azure RemoteApp απαιτεί ότι ενοποίηση με το Azure Active Directory, είτε 1) τη ρύθμιση των παραμέτρων Azure Active Συγχρονισμός καταλόγου με την επιλογή συγχρονισμό κωδικού πρόσβασης ή 2) τη ρύθμιση των παραμέτρων Azure Active συγχρονισμού καταλόγου χωρίς την επιλογή συγχρονισμό κωδικού πρόσβασης, αλλά χρησιμοποιώντας έναν τομέα που είναι ομόσπονδες σε AD FS. Δείτε τις [πληροφορίες ρύθμισης παραμέτρων για την υπηρεσία καταλόγου Active Directory με RemoteApp](remoteapp-ad.md).
5.  Δημοσίευση εφαρμογών RemoteApp.
6.  Ρυθμίστε τις παραμέτρους πρόσβασης χρήστη.

**Πριν ξεκινήσετε**

Πρέπει να κάνετε τα εξής πριν από τη δημιουργία της συλλογής:

- [Εγγραφείτε](https://azure.microsoft.com/services/remoteapp/) για Azure RemoteApp.
- Δημιουργήστε ένα λογαριασμό χρήστη στην υπηρεσία καταλόγου Active Directory για να χρησιμοποιήσετε ως ο λογαριασμός υπηρεσίας Azure RemoteApp. Περιορίστε τα δικαιώματα για αυτόν το λογαριασμό, έτσι ώστε να συνδεθεί μόνο σε μηχανές στον τομέα.
- Συλλογή πληροφοριών σχετικά με το δίκτυο εσωτερικής εγκατάστασης: διεύθυνση IP, πληροφορίες και λεπτομέρειες συσκευής VPN.
- Εγκαταστήστε τη λειτουργική μονάδα [Azure PowerShell](../powershell-install-configure.md) .
- Συλλογή πληροφοριών σχετικά με τους χρήστες που θέλετε να εκχωρήσετε πρόσβαση σε. Θα χρειαστεί το κύριο όνομα χρήστη Azure Active Directory (για παράδειγμα, name@contoso.com) για κάθε χρήστη. Βεβαιωθείτε ότι το UPN συμφωνεί με μεταξύ Azure AD και υπηρεσίας καταλόγου Active Directory.
- Επιλέξτε την εικόνα σας πρότυπο. Μια εικόνα προτύπου Azure RemoteApp περιέχει τις εφαρμογές και τα προγράμματα που θέλετε να δημοσιεύσετε για τους χρήστες σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επιλογές εικόνας Azure RemoteApp](remoteapp-imageoptions.md) .
- Θέλετε να χρησιμοποιήσετε το Office 365 ProPlus εικόνα; Ανατρέξτε στο θέμα πληροφορίες [εδώ](remoteapp-officesubscription.md).
- [Ρύθμιση παραμέτρων της υπηρεσίας καταλόγου Active Directory για RemoteApp](remoteapp-ad.md).



## <a name="step-1-set-up-your-virtual-network"></a>Βήμα 1: Ρύθμιση του δικτύου σας εικονικού
Μπορείτε να αναπτύξετε μια συλλογή υβριδική που χρησιμοποιεί ένα υπάρχον Azure εικονικό δίκτυο ή μπορείτε να δημιουργήσετε ένα νέο εικονικό δίκτυο. Ένα εικονικό δίκτυο σας επιτρέπει να τα δεδομένα της access τους χρήστες του τοπικού δικτύου σας μέσω απομακρυσμένου RemoteApp πόρων. Χρησιμοποιώντας ένα Azure εικονικού δικτύου σας δίνει την πρόσβασή σας συλλογή απευθείας με το δίκτυο σε άλλες υπηρεσίες του Azure και εικονικές μηχανές αναπτυχθεί σε αυτό το εικονικό δίκτυο.

Βεβαιωθείτε ότι μπορείτε αναθεωρήσετε τις πληροφορίες [VNET σχεδιασμού](remoteapp-planvnet.md) και [μέγεθος VNET](remoteapp-vnetsizing.md) πριν να δημιουργήσετε το VNET.

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Δημιουργήστε μια VNET Azure και συμμετοχή σε αυτήν για την ανάπτυξη της υπηρεσίας καταλόγου Active Directory

Ξεκινήστε με τη δημιουργία ενός [εικονικού δικτύου](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Αυτό γίνεται στην καρτέλα " **δίκτυο** " στην πύλη του Azure. Πρέπει να συνδεθείτε εικονικού το δίκτυό σας με την ανάπτυξη της υπηρεσίας καταλόγου Active Directory που συγχρονίζονται με το μισθωτή του Azure Active Directory.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία εικονικών δικτύου με την πύλη Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Βεβαιωθείτε ότι είναι έτοιμος για Azure RemoteApp εικονικού το δίκτυό σας
Ας πριν να δημιουργήσετε τη συλλογή, βεβαιωθείτε ότι το δίκτυό σας νέο εικονικό είναι έτοιμη. Μπορείτε να επικυρώσετε αυτό, κάνοντας τα εξής:

1. Δημιουργήστε μια εικονική μηχανή Azure μέσα στο υποδίκτυο του εικονικού δικτύου που μόλις δημιουργήσατε για RemoteApp.
2. Χρησιμοποιήστε απομακρυσμένης επιφάνειας εργασίας για να συνδεθείτε με την εικονική μηχανή. (Κάντε κλικ στην επιλογή **σύνδεση**.)
3. Συμμετοχή σε αυτήν την ίδια ανάπτυξη υπηρεσίας καταλόγου Active Directory που θέλετε να χρησιμοποιήσετε για RemoteApp.

Που λειτουργούν; Το εικονικό δίκτυο και υποδικτύου είναι έτοιμα για Azure RemoteApp!

Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με τη δημιουργία Azure εικονικές μηχανές και σύνδεση απομακρυσμένης επιφάνειας εργασίας [εδώ](https://msdn.microsoft.com/library/azure/jj156003.aspx)σε αυτά.

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Βήμα 2: Δημιουργία μιας συλλογής Azure RemoteApp ##



1. Στην [πύλη του Azure](http://manage.windowsazure.com), μεταβείτε στη σελίδα Azure RemoteApp.
2. Κάντε κλικ στην επιλογή **Δημιουργία > Δημιουργία με VNET**.
3. Πληκτρολογήστε ένα όνομα για τη συλλογή.
4. Επιλέξτε το πρόγραμμα που θέλετε να χρησιμοποιήσετε - τυπικά ή βασικές.
5. Επιλέξτε το VNET από την αναπτυσσόμενη λίστα και, στη συνέχεια, σας υποδίκτυο.
6. Επιλέξτε για να συμμετάσχετε σε αυτή για τον τομέα σας.
5. Κάντε κλικ στην επιλογή **Δημιουργία RemoteApp συλλογής**.

Μετά τη δημιουργία συλλογής σας Azure RemoteApp, κάντε διπλό κλικ στο όνομα της συλλογής. Που θα εμφανιστεί η σελίδα **Γρήγορης εκκίνησης** - αυτό είναι όταν ολοκληρώσετε τη ρύθμιση παραμέτρων της συλλογής.

Κάτι πήγε καλά; Δείτε τις [πληροφορίες αντιμετώπισης προβλημάτων υβριδική συλλογής](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>Βήμα 3: Σύνδεση συλλογής σας με τον τοπικό τομέα ##


1. Στη σελίδα **Γρήγορη εκκίνηση** , κάντε κλικ στην επιλογή **συμμετοχή σε έναν τοπικό τομέα**.
2. Προσθήκη του λογαριασμού υπηρεσίας Azure RemoteApp στον τομέα σας τοπική υπηρεσία καταλόγου Active Directory. Θα χρειαστεί το όνομα τομέα, οργανική μονάδα, υπηρεσία όνομα χρήστη και τον κωδικό πρόσβασης.

    Πρόκειται για τις πληροφορίες που συλλέγονται εάν ακολουθήσατε τα βήματα στο θέμα [Ρύθμιση παραμέτρων υπηρεσίας καταλόγου Active Directory για Azure RemoteApp](remoteapp-ad.md).


## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Βήμα 4: Σύνδεση με μια εικόνα Azure RemoteApp ##

Μια εικόνα προτύπου Azure RemoteApp περιλαμβάνει τα προγράμματα που θέλετε να χρησιμοποιήσετε από κοινού με τους χρήστες. Μπορείτε να δημιουργήσετε μια νέα [εικόνα προτύπου](remoteapp-imageoptions.md) ή να συνδεθείτε σε μια υπάρχουσα εικόνα (ένα ήδη εισαχθεί ή αποστέλλεται Azure RemoteApp). Μπορείτε επίσης να συνδέσετε με μία από τις Azure RemoteApp [εικόνες των προτύπων](remoteapp-images.md) που περιέχουν στο Office 365 ή προγράμματα του Office 2013 (για χρήση δοκιμαστικής έκδοσης).

Εάν στέλνετε τη νέα εικόνα, πρέπει να εισαγάγετε το όνομα και επιλέξτε τη θέση για την εικόνα. Στην επόμενη σελίδα του οδηγού, θα δείτε ένα σύνολο cmdlet του PowerShell - αντιγραφή και εκτέλεση αυτών των cmdlet από μια αναβαθμισμένη εντολών του Windows PowerShell για να αποστείλετε την καθορισμένη εικόνα.

Εάν συνδέεστε σε μια υπάρχουσα εικόνα προτύπου, απλώς Καθορίστε το όνομα εικόνας, θέση και σχετικές Azure συνδρομής.



## <a name="step-5-configure-active-directory-directory-synchronization"></a>Βήμα 5: Ρύθμιση παραμέτρων συγχρονισμού καταλόγου Active Directory ##

Azure RemoteApp απαιτεί ότι ενοποίηση με το Azure Active Directory, είτε 1) τη ρύθμιση των παραμέτρων Azure Active Συγχρονισμός καταλόγου με την επιλογή συγχρονισμό κωδικού πρόσβασης ή 2) τη ρύθμιση των παραμέτρων Azure Active συγχρονισμού καταλόγου χωρίς την επιλογή συγχρονισμό κωδικού πρόσβασης, αλλά χρησιμοποιώντας έναν τομέα που είναι ομόσπονδες σε AD FS.

Ανατρέξτε στο θέμα [σύνδεση AD](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) - σε αυτό το άρθρο σάς βοηθά να ρυθμίσετε την ενοποίηση καταλόγου σε 4 βήματα.

Ανατρέξτε στο θέμα [Χάρτης συγχρονισμού καταλόγου](http://msdn.microsoft.com//library/azure/hh967642.aspx) για το σχεδιασμό των πληροφορίες και λεπτομερείς οδηγίες.

## <a name="step-6-publish-apps"></a>Βήμα 6: Δημοσίευση εφαρμογών ##

Μια εφαρμογή Azure RemoteApp είναι η εφαρμογή ή το πρόγραμμα που παρέχετε στους χρήστες σας. Βρίσκεται στην εικόνα προτύπου έχετε αποστείλει για τη συλλογή. Όταν ένας χρήστης αποκτά πρόσβαση σε μια εφαρμογή, εμφανίζεται για να εκτελέσετε σε τοπικό περιβάλλον τους, αλλά πραγματικά εκτελείται στο Azure.

Πριν από τους χρήστες σας να αποκτήσετε πρόσβαση σε εφαρμογές, θα πρέπει να δημοσιεύσετε τις – αυτό σας επιτρέπει να οι χρήστες αποκτούν πρόσβαση στις εφαρμογές μέσω του προγράμματος-πελάτη απομακρυσμένης επιφάνειας εργασίας.

Μπορείτε να δημοσιεύσετε πολλές εφαρμογές στη συλλογή σας. Από τη σελίδα δημοσίευσης, κάντε κλικ στην επιλογή **Δημοσίευση** για να προσθέσετε μια εφαρμογή. Μπορείτε είτε να δημοσιεύσετε από το μενού **Έναρξη** της εικόνας πρότυπο ή καθορίζοντας τη διαδρομή στην εικόνα προτύπου για την εφαρμογή. Εάν επιλέξετε να προσθέσετε από το μενού **Έναρξη** , επιλέξτε το πρόγραμμα για να προσθέσετε. Εάν επιλέξετε να καταχωρήσετε τη διαδρομή προς την εφαρμογή, δώστε ένα όνομα για την εφαρμογή και τη διαδρομή προς όπου έχει εγκατασταθεί η εικόνα προτύπου.

## <a name="step-7-configure-user-access"></a>Βήμα 7: Ρύθμιση πρόσβασης των χρηστών ##

Τώρα που έχετε δημιουργήσει τη συλλογή σας, πρέπει να προσθέσετε τους χρήστες που θέλετε να έχετε τη δυνατότητα να χρησιμοποιήσετε τον απομακρυσμένο πόρους. Οι χρήστες που παρέχετε πρόσβαση σε πρέπει να υπάρχει μισθωτή υπηρεσίας καταλόγου Active Directory που σχετίζεται με τη συνδρομή που χρησιμοποιήσατε για να δημιουργήσετε αυτήν τη συλλογή Azure RemoteApp.

1.  Από τη σελίδα γρήγορης εκκίνησης, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων πρόσβασης χρήστη**.
2.  Εισαγάγετε το λογαριασμό εργασίας (από την υπηρεσία καταλόγου Active Directory) ή το λογαριασμό Microsoft που θέλετε να εκχωρήσετε πρόσβαση για.

    **Σημειώσεις:**

    Βεβαιωθείτε ότι χρησιμοποιείτε το “user@domain.com” μορφή.

    Εάν χρησιμοποιείτε Office 365 ProPlus στη συλλογή σας, πρέπει να χρησιμοποιήσετε την υπηρεσία καταλόγου Active Directory ταυτότητες για τους χρήστες σας. Αυτό σας βοηθά να επικυρώσετε παραχώρησης πολλαπλών αδειών χρήσης.


3.  Μόλις επικυρώνονται τους χρήστες, κάντε κλικ στην επιλογή **Αποθήκευση**.


## <a name="next-steps"></a>Επόμενα βήματα ##
Που είναι η κατάλληλη επιλογή-έχετε δημιουργήσει και αναπτύξει τη συλλογή υβριδική Azure RemoteApp με επιτυχία. Το επόμενο βήμα είναι να έχουν οι χρήστες σας, κάντε λήψη και εγκαταστήστε το πρόγραμμα-πελάτη απομακρυσμένης επιφάνειας εργασίας. Μπορείτε να βρείτε τη διεύθυνση URL για το πρόγραμμα-πελάτη στη σελίδα Azure RemoteApp γρήγορης εκκίνησης. Στη συνέχεια, έχουν οι χρήστες θα συνδέονται στο πρόγραμμα-πελάτη και αποκτήστε πρόσβαση στις εφαρμογές που δημοσιεύσατε.



### <a name="help-us-help-you"></a>Βοηθήστε μας να σας βοηθήσει να
Γνωρίζατε ότι εκτός από χαρακτηρισμού σε αυτό το άρθρο και πραγματοποίηση σχόλια προς τα κάτω, κάτω από το στοιχείο, μπορείτε να κάνετε αλλαγές στο ίδιο το άρθρο; Κάτι που λείπουν; Κάποιο πρόβλημα; Να συντάξω κάτι που είναι απλώς σύγχυση; Κύλιση προς τα επάνω και κάντε κλικ στην επιλογή **Επεξεργασία σε GitHub** για να κάνετε αλλαγές - αυτές να παραδίδεται στο μας για αναθεώρηση και, στη συνέχεια, αφού εγγραφή σε αυτά, θα δείτε τις αλλαγές και οι βελτιώσεις εδώ.