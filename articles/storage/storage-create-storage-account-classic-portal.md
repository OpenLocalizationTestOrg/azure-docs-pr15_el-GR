<properties
    pageTitle="Πώς μπορείτε να δημιουργήσετε, να διαχειρίζεστε ή διαγραφή ενός λογαριασμού χώρου αποθήκευσης στην πύλη κλασική Azure | Microsoft Azure"
    description="Δημιουργία νέου λογαριασμού χώρου αποθήκευσης, Διαχείριση σας πλήκτρα πρόσβασης λογαριασμού ή διαγραφή ενός λογαριασμού χώρου αποθήκευσης στην πύλη του Azure. Μάθετε σχετικά με τους λογαριασμούς τυπική και premium χώρου αποθήκευσης."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>Σχετικά με τους λογαριασμούς Azure χώρου αποθήκευσης

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Επισκόπηση

Ένα λογαριασμό Azure χώρου αποθήκευσης σάς προσφέρει πρόσβαση στις υπηρεσίες αντικειμένων Blob του Azure, ουρά, πίνακα και αρχείων στο χώρο αποθήκευσης Azure. Το λογαριασμό χώρου αποθήκευσης παρέχει το μοναδικό χώρο ονομάτων για το χώρο αποθήκευσης Azure αντικείμενα δεδομένων. Από προεπιλογή, τα δεδομένα στο λογαριασμό σας είναι διαθέσιμη μόνο σε εσάς, ο κάτοχος του λογαριασμού.

Υπάρχουν δύο τύποι αποθήκευσης λογαριασμών:

- Ένα λογαριασμό τυπική αποθήκευσης περιλαμβάνει χώρο αποθήκευσης αντικειμένων Blob, πίνακα, ουρά και αρχείων.
- Ένα άρτιο χώρου αποθήκευσης υποστηρίζει επί του παρόντος μόνο δίσκων Azure εικονική μηχανή. Ανατρέξτε στο θέμα [αποθήκευσης Premium: χώρος αποθήκευσης υψηλών επιδόσεων για φόρτους εργασίας εικονική μηχανή Azure](storage-premium-storage.md) για μια επισκόπηση αναλυτικά Premium χώρου αποθήκευσης.

## <a name="storage-account-billing"></a>Χώρος αποθήκευσης λογαριασμού χρέωσης

Χρέωσης για χρήση του χώρου αποθήκευσης Azure που βασίζονται στο λογαριασμό σας στο χώρο αποθήκευσης. Κόστος αποθήκευσης βασίζεται σε τέσσερα παράγοντες: χωρητικότητα αποθήκευσης, συνδυασμού αναπαραγωγής, συναλλαγές χώρου αποθήκευσης και δεδομένων εξόδου.

- Χωρητικότητα αποθήκευσης αναφέρεται σε τον όγκο σας υποστήριξης λογαριασμού χώρου αποθήκευσης που θα χρησιμοποιείτε για την αποθήκευση δεδομένων. Το κόστος των απλώς την αποθήκευση των δεδομένων σας καθορίζεται από τον όγκο δεδομένων που αποθηκευμένο και πώς την αναπαραγωγή.
- Αναπαραγωγή καθορίζει τον αριθμό των αντιτύπων τα δεδομένα διατηρούνται ταυτόχρονα και σε ποιες θέσεις.
- Συναλλαγές αναφέρονται όλα λειτουργίες ανάγνωσης και εγγραφής στο χώρο αποθήκευσης Azure.
- Δεδομένα εξόδου αναφέρεται σε δεδομένα μεταφέρονται εκτός μιας περιοχής Azure. Όταν τα δεδομένα στο λογαριασμό σας στο χώρο αποθήκευσης είναι προσβάσιμη από μια εφαρμογή που δεν εκτελείται στην ίδια περιοχή, εάν αυτή η εφαρμογή είναι μια υπηρεσία cloud ή κάποιον άλλο τύπο της εφαρμογής, στη συνέχεια, που θα χρεωθείτε για δεδομένα εξόδου. (Για τις υπηρεσίες του Azure, μπορείτε να λάβετε μέτρα για την ομαδοποίηση των δεδομένων και των υπηρεσιών στο το ίδιο κέντρα δεδομένων για να μειώσετε ή να αποκλείσετε χρεώσεις δεδομένων εξόδου.)  

Στη σελίδα [Τιμολόγηση αποθήκευσης Azure](https://azure.microsoft.com/pricing/details/storage) παρέχει λεπτομερείς πληροφορίες τιμολόγησης για χωρητικότητα αποθήκευσης, αναπαραγωγής και συναλλαγές. Τη σελίδα [Λεπτομερειών τις τιμές δεδομένων μεταφορές](https://azure.microsoft.com/pricing/details/data-transfers/) παρέχει λεπτομερείς πληροφορίες τιμολόγησης για δεδομένα εξόδου.

Για λεπτομέρειες σχετικά με το χώρο αποθήκευσης λογαριασμού χωρητικότητας και της απόδοσης των προορισμών, ανατρέξτε στο θέμα [Azure κλιμάκωση χώρου αποθήκευσης και τους στόχους επιδόσεων](storage-scalability-targets.md).

> [AZURE.NOTE] Όταν δημιουργείτε μια εικονική μηχανή Azure, ένα λογαριασμό του χώρου αποθήκευσης δημιουργείται για εσάς αυτόματα στη θέση ανάπτυξης Εάν δεν έχετε ήδη ένα λογαριασμό του χώρου αποθήκευσης σε αυτήν τη θέση. Επομένως, δεν είναι απαραίτητο να ακολουθήσετε τα παρακάτω βήματα για να δημιουργήσετε ένα λογαριασμό χώρου αποθήκευσης για δίσκων σας εικονική μηχανή. Το όνομα του λογαριασμού χώρου αποθήκευσης θα είναι με βάση το όνομα του υπολογιστή εικονική. Ανατρέξτε στην [τεκμηρίωση εικονικές μηχανές Windows Azure](https://azure.microsoft.com/documentation/services/virtual-machines/) για περισσότερες λεπτομέρειες.

## <a name="create-a-storage-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης

1. Είσοδος στην [πύλη του Azure κλασική](https://manage.windowsazure.com).

2. Στη γραμμή εργασιών στο κάτω μέρος της σελίδας, κάντε κλικ στην επιλογή **Δημιουργία** . Επιλέξτε **Υπηρεσίες δεδομένων** | **χώρου αποθήκευσης**, και, στη συνέχεια, κάντε κλικ στην επιλογή **Γρήγορη δημιουργία**.

    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. Στη **διεύθυνση URL**, πληκτρολογήστε ένα όνομα για το λογαριασμό χώρου αποθήκευσης.

    > [AZURE.NOTE] Ονόματα λογαριασμών χώρου αποθήκευσης πρέπει να είναι μεταξύ 3 και 24 χαρακτήρες και μπορεί να περιέχει αριθμούς και πεζών γραμμάτων.
    >  
    > Το όνομα του λογαριασμού χώρου αποθήκευσης πρέπει να είναι μοναδικό μέσα σε Azure. Κλασική πύλη του Azure που υποδεικνύει εάν έχει ήδη χρησιμοποιηθεί το όνομα του λογαριασμού χώρου αποθήκευσης που επιλέγετε.

    Για λεπτομέρειες σχετικά με το πώς θα χρησιμοποιηθεί το όνομα του λογαριασμού χώρου αποθήκευσης για να αντιμετωπίσετε τα αντικείμενα στο χώρο αποθήκευσης Azure, ανατρέξτε στο θέμα [τελικά σημεία λογαριασμού χώρου αποθήκευσης](#storage-account-endpoints) κάτω από το στοιχείο.

4. Στην **Ομάδα θέση/συνάφειας**, επιλέξτε μια θέση για το λογαριασμό χώρου αποθήκευσης που είναι κοντά που ή στους πελάτες σας. Εάν θα υπάρχει πρόσβαση στα δεδομένα στο λογαριασμό σας στο χώρο αποθήκευσης από άλλη υπηρεσία Azure, όπως μια Azure εικονική μηχανή ή μια υπηρεσία cloud, μπορείτε να επιλέξετε μια ομάδα συσχέτισης από τη λίστα για να ομαδοποιήσετε το λογαριασμό χώρου αποθήκευσης στο ίδιο κέντρο δεδομένων με άλλες υπηρεσίες Azure που χρησιμοποιείτε για να βελτιώσετε την απόδοση και χαμηλότερο κόστος.

    Σημειώστε ότι πρέπει να επιλέξετε μια ομάδα συσχέτισης όταν δημιουργείται το λογαριασμό χώρου αποθήκευσης. Δεν μπορείτε να μετακινήσετε έναν υπάρχοντα λογαριασμό σε μια ομάδα συσχέτισης. Για περισσότερες πληροφορίες σχετικά με ομάδες συσχέτισης, ανατρέξτε στο θέμα παρακάτω [θέση υπηρεσίας από κοινού με μια ομάδα συσχέτισης](#service-co-location-with-an-affinity-group) .

    >[AZURE.IMPORTANT] Για να προσδιορίσετε των θέσεων που είναι διαθέσιμες για τη συνδρομή σας, μπορείτε να καλέσετε τη λειτουργία [λίστα όλες τις υπηρεσίες παροχής πόρων](https://msdn.microsoft.com/library/azure/dn790524.aspx) . Για να λίστα υπηρεσιών παροχής από το PowerShell, καλέστε [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). Από το .NET, χρησιμοποιήστε τη μέθοδο [λίστα](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) της κλάσης ProviderOperationsExtensions.
    >
    >Επιπλέον, ανατρέξτε στο θέμα [Azure περιοχές](https://azure.microsoft.com/regions/#services) για περισσότερες πληροφορίες σχετικά με το ποιες υπηρεσίες είναι διαθέσιμες σε ποια περιοχή.


5. Εάν έχετε περισσότερες από μία συνδρομές Azure, στη συνέχεια, εμφανίζεται το πεδίο " **συνδρομή** ". Σε **συνδρομή**, εισαγάγετε το Azure συνδρομή που θέλετε να χρησιμοποιήσετε το λογαριασμό χώρου αποθήκευσης με.

6. Στην **αναπαραγωγής**, επιλέξτε το επιθυμητό επίπεδο της αναπαραγωγής για το λογαριασμό χώρου αποθήκευσης. Η επιλογή προτεινόμενα αναπαραγωγής είναι παν πλεονάζοντα αναπαραγωγή, που παρέχει μέγιστη διάρκεια ζωής για τα δεδομένα σας. Για περισσότερες λεπτομέρειες σχετικά με τις επιλογές αναπαραγωγής Azure χώρου αποθήκευσης, ανατρέξτε στο θέμα [αποθήκευσης Azure αναπαραγωγής](storage-redundancy.md).

6. Κάντε κλικ στην επιλογή **Δημιουργία λογαριασμού χώρου αποθήκευσης**.

    Ενδέχεται να χρειαστούν μερικά λεπτά για να δημιουργήσετε το λογαριασμό χώρου αποθήκευσης. Για να ελέγξετε την κατάσταση, μπορείτε να παρακολουθείτε τις ειδοποιήσεις στο κάτω μέρος της πύλης κλασική Azure. Μετά τη δημιουργία του λογαριασμού χώρου αποθήκευσης, το νέο λογαριασμό σας στο χώρο αποθήκευσης έχει κατάσταση σε **σύνδεση** και είναι έτοιμη για χρήση.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### <a name="storage-account-endpoints"></a>Τελικά σημεία λογαριασμού χώρου αποθήκευσης

Κάθε αντικείμενο που αποθηκεύετε στο χώρο αποθήκευσης Azure έχει μια μοναδική διεύθυνση URL. Το όνομα του λογαριασμού χώρου αποθήκευσης φόρμες ο δευτερεύων τομέας από αυτήν τη διεύθυνση. Ο συνδυασμός του δευτερεύοντος τομέα και ο τομέας όνομα, το οποίο είναι συγκεκριμένη για κάθε υπηρεσία, φόρμες *τελικό σημείο* για το λογαριασμό χώρου αποθήκευσης.

Για παράδειγμα, εάν ο λογαριασμός σας χώρο αποθήκευσης ονομάζεται *mystorageaccount*, στη συνέχεια, τα τελικά σημεία προεπιλογή για το λογαριασμό χώρου αποθήκευσης είναι:

- Αντικειμένων blob υπηρεσίας: http://*mystorageaccount*. blob.core.windows.net

- Πίνακας υπηρεσίας: http://*mystorageaccount*. table.core.windows.net

- Ουρά υπηρεσίας: http://*mystorageaccount*. queue.core.windows.net

- Αρχείο υπηρεσίας: http://*mystorageaccount*. file.core.windows.net

Μπορείτε να δείτε τα τελικά σημεία για το λογαριασμό χώρου αποθήκευσης στον πίνακα εργαλείων του χώρου αποθήκευσης στην [Πύλη κλασική Azure](https://manage.windowsazure.com) μετά τη δημιουργία του λογαριασμού.

Η διεύθυνση URL για την πρόσβαση σε ένα αντικείμενο σε ένα λογαριασμό του χώρου αποθήκευσης είναι ενσωματωμένη προσαρτώντας η θέση του αντικειμένου στο λογαριασμό χώρου αποθήκευσης για το τελικό σημείο. Για παράδειγμα, μια διεύθυνση blob μπορεί να έχει αυτήν τη μορφή: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Μπορείτε επίσης να ρυθμίσετε ένα προσαρμοσμένο όνομα τομέα για να χρησιμοποιήσετε με το λογαριασμό χώρου αποθήκευσης. Για λεπτομέρειες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ενός προσαρμοσμένου ονόματος τομέα για το τελικό σημείο αποθήκευσης αντικειμένων blob](storage-custom-domain-name.md) .

### <a name="service-co-location-with-an-affinity-group"></a>Θέση υπηρεσίας από κοινού με μια ομάδα συσχέτισης

Μια *ομάδα συσχέτισης* είναι μια γεωγραφικά ομαδοποίηση των υπηρεσιών Azure και ΣΠΣ με το λογαριασμό σας Azure χώρου αποθήκευσης. Μια ομάδα συσχέτισης να βελτιώσετε τις επιδόσεις υπηρεσίας εντοπίζοντας φόρτους εργασίας του υπολογιστή σας στο ίδιο κέντρο δεδομένων ή κοντά το ακροατήριο στο χρήστη. Επίσης, δεν υπάρχει χρεώσεων χρεώσεις είναι που προκύπτει για εξόδου κατά την πρόσβαση σε δεδομένα σε ένα λογαριασμό του χώρου αποθήκευσης από άλλη υπηρεσία που είναι μέρος της ίδιας ομάδας συνάφεια.

> [AZURE.NOTE]  Για να δημιουργήσετε μια ομάδα συσχέτισης, ανοίξτε στην περιοχή <b>Ρυθμίσεις</b> από την [Πύλη κλασική Azure](https://manage.windowsazure.com), κάντε κλικ στην επιλογή <b>ομάδες συσχέτισης</b>και, στη συνέχεια, κάντε κλικ στην επιλογή <b>Προσθήκη μιας ομάδας συσχέτισης</b> ή στο κουμπί <b>Προσθήκη</b> . Μπορείτε επίσης να δημιουργήσετε και να διαχείριση ομάδων συσχέτισης χρησιμοποιώντας το API διαχείρισης υπηρεσίας Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">Λειτουργίες συσχέτισης ομάδες</a> .

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Προβολή και αντιγραφή αναδημιουργήσετε πλήκτρα πρόσβασης χώρου αποθήκευσης

Όταν δημιουργείτε ένα λογαριασμό του χώρου αποθήκευσης, Azure δημιουργεί δύο πλήκτρα πρόσβασης 512 bit χώρου αποθήκευσης, που χρησιμοποιούνται για τον έλεγχο ταυτότητας κατά την πρόσβαση σε λογαριασμό του χώρου αποθήκευσης. Παρέχοντας δύο πλήκτρα πρόσβασης χώρου αποθήκευσης, Azure σάς επιτρέπει να δημιουργήσετε ξανά τα πλήκτρα με πρόσβαση σε αυτή την υπηρεσία ή καμία διακοπή στην υπηρεσία σας χώρου αποθήκευσης.

> [AZURE.NOTE] Συνιστάται να αποφύγετε την κοινή χρήση του χώρου αποθήκευσης πλήκτρα πρόσβασης με οποιοδήποτε άλλο άτομο. Για να επιτρέπετε την πρόσβαση σε πόρους αποθήκευσης χωρίς να δίνετε σας πλήκτρα πρόσβασης, μπορείτε να χρησιμοποιήσετε μια *υπογραφή κοινόχρηστη πρόσβαση*. Μια υπογραφή κοινόχρηστη πρόσβαση παρέχει πρόσβαση σε έναν πόρο στο λογαριασμό σας για ένα χρονικό διάστημα που έχετε καθορίσει και με τα δικαιώματα που καθορίζετε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση κοινόχρηστων Access υπογραφές (συσχετισμών Ασφαλείας)](storage-dotnet-shared-access-signature-part-1.md) .

Στην [Κλασική πύλη Azure](https://manage.windowsazure.com), χρησιμοποιήστε **Διαχείριση κλειδιών** στον πίνακα εργαλείων ή στη σελίδα **χώρος αποθήκευσης** για να προβάλετε, να αντιγράψετε και να αναδημιουργήσετε των πλήκτρων πρόσβασης του χώρου αποθήκευσης που χρησιμοποιούνται για την πρόσβαση στις υπηρεσίες Blob πίνακα και ουρά.

### <a name="copy-a-storage-access-key"></a>Αντιγράψτε έναν αριθμό-κλειδί πρόσβασης χώρου αποθήκευσης  

Μπορείτε να χρησιμοποιήσετε τη **Διαχείριση κλειδιών** για να αντιγράψετε ένα πλήκτρο πρόσβασης χώρου αποθήκευσης για να χρησιμοποιήσετε σε μια συμβολοσειρά σύνδεσης. Η συμβολοσειρά σύνδεσης απαιτεί το όνομα του λογαριασμού χώρου αποθήκευσης και έναν αριθμό-κλειδί για να χρησιμοποιήσετε στον έλεγχο ταυτότητας. Για πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων συμβολοσειρές σύνδεσης για να αποκτήσετε πρόσβαση σε υπηρεσίες Azure αποθήκευσης, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων Azure συμβολοσειρές σύνδεσης χώρου αποθήκευσης](storage-configure-connection-string.md).

1. Στην [Κλασική Azure πύλη](https://manage.windowsazure.com), κάντε κλικ στην επιλογή **Αποθήκευση**και, στη συνέχεια, κάντε κλικ στο όνομα του λογαριασμού χώρου αποθήκευσης για να ανοίξετε τον πίνακα εργαλείων.

2. Κάντε κλικ στην επιλογή **Διαχείριση κλειδιών**.

    Ανοίγει η **Διαχείριση των πλήκτρων πρόσβασης** .

    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Για να αντιγράψετε ένα πλήκτρο πρόσβασης χώρου αποθήκευσης, επιλέξτε το βασικό κείμενο. Στη συνέχεια, κάντε δεξί κλικ και επιλέξτε **Αντιγραφή**.

### <a name="regenerate-storage-access-keys"></a>Αναδημιουργήσετε πλήκτρα πρόσβασης χώρου αποθήκευσης
Συνιστάται να αλλάζετε τα πλήκτρα πρόσβασης στο λογαριασμό σας χώρο αποθήκευσης περιοδικά για να διατηρήσει ασφαλείς τις συνδέσεις σας χώρου αποθήκευσης. Δύο πλήκτρα πρόσβασης έχουν ανατεθεί, έτσι ώστε να μπορείτε να διατηρήσετε τις συνδέσεις με το λογαριασμό χώρου αποθήκευσης, χρησιμοποιώντας ένα πλήκτρο πρόσβασης, ενώ μπορείτε να αναδημιουργήσετε το άλλο πλήκτρο πρόσβασης.

> [AZURE.WARNING] Αναδημιουργία σας πλήκτρα πρόσβασης μπορεί να επηρεάσει τις υπηρεσίες στο Azure, καθώς και τις δικές σας εφαρμογές που εξαρτώνται από το λογαριασμό χώρου αποθήκευσης. Όλοι οι πελάτες που χρησιμοποιούν το πλήκτρο πρόσβασης για να αποκτήσετε πρόσβαση στο λογαριασμό του χώρου αποθήκευσης πρέπει να ενημερωθούν για να χρησιμοποιήσετε το νέο κλειδί.

**Υπηρεσίες πολυμέσων** - εάν έχετε υπηρεσίες πολυμέσων που εξαρτώνται από το λογαριασμό χώρου αποθήκευσης, που πρέπει να επαναλάβετε το συγχρονισμό των πλήκτρων πρόσβασης με την υπηρεσία πολυμέσων αφού αναδημιουργήσετε τα πλήκτρα.

**Εφαρμογές** - εάν έχετε εφαρμογές web ή τις υπηρεσίες cloud που χρησιμοποιούν το λογαριασμό χώρου αποθήκευσης, θα χάσετε τις συνδέσεις αν αναδημιουργήσετε πλήκτρα, εκτός εάν φέρετε των αριθμών-κλειδιών. 

**Εξερεύνησης χώρου αποθήκευσης** - Εάν χρησιμοποιείτε τις [εφαρμογές Εξερεύνηση χώρου αποθήκευσης](storage-explorers.md), πιθανότατα θα πρέπει να ενημερώσετε το κλειδί χώρου αποθήκευσης που χρησιμοποιείται από αυτές τις εφαρμογές.

Ακολουθεί η διαδικασία για την περιστροφή του χώρου αποθήκευσης πλήκτρα πρόσβασης:

1. Ενημέρωση των συμβολοσειρών σύνδεσης κώδικα της εφαρμογής σας για να αναφέρονται στο κλειδί δευτερεύοντα πρόσβασης του λογαριασμού χώρου αποθήκευσης.

2. Αναδημιουργήσετε το κλειδί πρωτεύοντος πρόσβασης για το λογαριασμό χώρου αποθήκευσης. Στην [Κλασική πύλη Azure](https://manage.windowsazure.com), από τον πίνακα εργαλείων ή στη σελίδα **Ρύθμιση παραμέτρων** , κάντε κλικ στην επιλογή **Διαχείριση κλειδιών**. Κάντε κλικ στην επιλογή **Αναδημιουργία** κάτω από το access πρωτεύον κλειδί και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε ότι θέλετε να δημιουργήσετε ένα νέο αριθμό-κλειδί.

3. Ενημερώστε τις συμβολοσειρές σύνδεσης στον κώδικά σας για το νέο κλειδί πρωτεύοντος access αναφορά.

4. Αναδημιουργήσετε το κλειδί δευτερεύοντα πρόσβασης.

## <a name="delete-a-storage-account"></a>Διαγραφή ενός λογαριασμού χώρου αποθήκευσης

Για να καταργήσετε ένα λογαριασμό χώρου αποθήκευσης που χρησιμοποιείτε δεν είναι πλέον, χρησιμοποιήστε **Διαγραφή** στον πίνακα εργαλείων ή στη σελίδα **Ρύθμιση παραμέτρων** . **Διαγραφή** διαγράφει το λογαριασμό ολόκληρου χώρου αποθήκευσης, συμπεριλαμβανομένων όλων των αντικειμένων blob, πίνακες και ουρές στο λογαριασμό.

> [AZURE.WARNING] Δεν είναι δυνατή η επαναφορά ενός λογαριασμού διαγραμμένων χώρου αποθήκευσης ή ανάκτηση το περιεχόμενο που περιείχε πριν από τη διαγραφή. Φροντίστε να δημιουργήσετε αντίγραφα ασφαλείας ό, τι θέλετε να αποθηκεύσετε πριν να διαγράψετε το λογαριασμό. Το ίδιο ισχύει true για όλους τους πόρους στο λογαριασμό — μόλις διαγράψετε ένα blob, πίνακα, ουρά ή αρχείο, διαγράφεται μόνιμα.
>
> Εάν ο λογαριασμός σας χώρο αποθήκευσης περιέχει αρχεία VHD για μια εικονική μηχανή Azure, στη συνέχεια, πρέπει να διαγράψετε τις εικόνες και δίσκων που χρησιμοποιούν αυτά τα αρχεία VHD για να διαγράψετε το λογαριασμό χώρου αποθήκευσης. Αρχικά, διακόψετε την εικονική μηχανή εάν εκτελείται και, στη συνέχεια, διαγράψτε το. Για να διαγράψετε δίσκων, μεταβείτε στην καρτέλα **δίσκων** και να διαγράψετε οποιοδήποτε δίσκων εκεί. Για να διαγράψετε εικόνες, μεταβείτε στην καρτέλα " **εικόνες** " και διαγράψτε τις εικόνες που είναι αποθηκευμένες στο λογαριασμό.

1. Στην [Κλασική Azure πύλη](https://manage.windowsazure.com), κάντε κλικ στο **χώρο αποθήκευσης**.

2. Κάντε κλικ σε οποιοδήποτε σημείο στην εγγραφή λογαριασμού χώρου αποθήκευσης εκτός από το όνομα και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαγραφή**.

     - Ή -

    Κάντε κλικ στο όνομα του λογαριασμού χώρου αποθήκευσης για να ανοίξετε τον πίνακα εργαλείων και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαγραφή**.

3. Κάντε κλικ στο κουμπί **Ναι** για να επιβεβαιώσετε ότι θέλετε να διαγράψετε το λογαριασμό χώρου αποθήκευσης.

## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε περισσότερα σχετικά με το χώρο αποθήκευσης Azure, ανατρέξτε στην [τεκμηρίωση αποθήκευσης Azure](https://azure.microsoft.com/documentation/services/storage/).
- Επισκεφθείτε το [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης](http://blogs.msdn.com/b/windowsazurestorage/).
- [Μεταφορά δεδομένων με το βοηθητικό πρόγραμμα γραμμής εντολών AzCopy](storage-use-azcopy.md)
