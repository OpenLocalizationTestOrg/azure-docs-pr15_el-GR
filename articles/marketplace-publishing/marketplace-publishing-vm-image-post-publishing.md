<properties
   pageTitle="Διαχείριση της εικόνας εικονική μηχανή στο το Azure Marketplace | Microsoft Azure"
   description="Λεπτομερή οδηγό σχετικά με τον τρόπο για να διαχειριστείτε την εικόνα σας εικονική μηχανή στο το Azure Marketplace μετά την αρχική δημοσίευση."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Azure"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="hascipio;"/>

# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Οδηγός μετά τη παραγωγής για εικονική μηχανή προσφορές από το Azure Marketplace

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να ενημερώσετε μια ζωντανή προσφορά εικονική μηχανή από το Azure Marketplace. Επίσης, παρέχει καθοδήγηση σχετικά με τη διαδικασία της προσθήκης ενός ή περισσότερων νέα SKU σε μια υπάρχουσα προσφορά και κατάργηση ζωντανή εικονική μηχανή προσφορά ή SKU από το Azure Marketplace.

Όταν μια προσφορά/SKU έχει τοποθετηθεί στην [Πύλη του Azure](http://portal.azure.com), δεν μπορείτε να αλλάξετε τα πεδία που ακολουθεί:

- **Προσφέρουν αναγνωριστικό:** [Δημοσίευσης πύλη -> εικονικές μηχανές -> Επιλογή -> η προσφορά καρτέλα εικόνες Εικονική -> προσφέρουν αναγνωριστικό]
- **Αναγνωριστικό SKU:** [Δημοσίευσης πύλη -> εικονικές μηχανές -> Επιλογή -> η προσφορά SKU καρτέλα -> Προσθήκη άλλες SKU]
- **Namespace publisher:** [Δημοσίευσης πύλη -> εικονικές μηχανές -> αναλυτικές οδηγίες καρτέλα -> Ενημέρωση μας για την εταιρεία σας (βρίσκονται στην περιοχή "Βήμα 2 καταχώρηση εταιρείας σας") -> Publisher Namespace -> Namespace]

Μετά την προσφορά/SKU παρατίθεται στην [Azure Marketplace](http://azure.microsoft.com/marketplace), δεν μπορείτε να αλλάξετε τα πεδία που ακολουθεί:

- **Προσφέρουν αναγνωριστικό:** [Δημοσίευσης πύλη -> εικονικές μηχανές -> Επιλογή -> η προσφορά καρτέλα εικόνες Εικονική -> προσφέρουν αναγνωριστικό]
- **Αναγνωριστικό SKU:** [Δημοσίευσης πύλη -> εικονικές μηχανές -> Επιλογή -> η προσφορά SKU καρτέλα -> Προσθήκη άλλες SKU]
- **Namespace publisher:** [Δημοσίευσης πύλη -> εικονικές μηχανές -> αναλυτικές οδηγίες -> καρτέλα Namespace Publisher πείτε μας σχετικά με τις εταιρείας σας (βρίσκονται στην περιοχή βήμα 2 καταχώρηση) -> Namespace]
- **Θύρες** [Δημοσίευσης πύλη -> εικονικές μηχανές -> Επιλογή -> η προσφορά καρτέλα εικόνες Εικονική -> Άνοιγμα θύρες]
- **Αλλαγή της λίστας SKU(s) τιμές**
- **Χρέωση μοντέλο αλλαγή που παρατίθενται SKU(s)**
- **Κατάργηση του χρεώσεις περιοχές του που παρατίθενται SKU(s)**
- **Αλλάζοντας το πλήθος δίσκου δεδομένων που παρατίθενται SKU(s)**



## <a name="1-how-to-update-the-technical-details-of-a-sku"></a>1. πώς μπορείτε να ενημερώσετε τις τεχνικές λεπτομέρειες σχετικά με άλλες SKU

Μπορείτε να προσθέσετε μια νέα έκδοση για να την που παρατίθενται SKU και να δημοσιεύσετε εκ νέου την προσφορά, ακολουθώντας τα βήματα που παρέχονται παρακάτω:

1. Συνδεθείτε στο τη [Δημοσίευση πύλη](https://publish.windowsazure.com).
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού, κάντε κλικ στην καρτέλα **Εικονική ΕΙΚΌΝΕΣ** .
4. Από την ενότητα **SKU** της καρτέλας **Εικονική ΕΙΚΌΝΩΝ** , εντοπίστε το SKU που θέλετε να ενημερώσετε.
5. Μετά από αυτό, προσθέστε έναν νέο αριθμό έκδοσης από την SKU και κάντε κλικ στο κουμπί **"+"** . Η νέα έκδοση πρέπει να είναι της μορφής x.y.z και όπου X, Y και Z είναι ακέραιοι αριθμοί. Αλλαγές στην έκδοση μόνο πρέπει να αυξάνονται.
6. Στο πλαίσιο **Διεύθυνση URL VHD λειτουργικό σύστημα** , προσθέστε την υπογραφή κοινόχρηστη πρόσβαση URI που έχει δημιουργηθεί για το λειτουργικό σύστημα VHD και να αποθηκεύσετε τις αλλαγές.

    >[AZURE.IMPORTANT] Που δεν είναι δυνατό να διαβάθμιση/μείωση το πλήθος δίσκου δεδομένων που παρατίθενται άλλες SKU. Πρέπει να δημιουργήσετε μια νέα SKU σε αυτήν την περίπτωση. Ανατρέξτε στην ενότητα [3. Πώς μπορείτε να προσθέσετε μια νέα SKU στην περιοχή μιας προσφοράς που παρατίθενται](#3-how-to-add-a-new-sku-under-a-live-offer) για λεπτομερείς οδηγίες.

7. Αφού κάνετε τις αλλαγές, μεταβείτε στην καρτέλα " **ΔΗΜΟΣΊΕΥΣΗ** " και κάντε κλικ στο κουμπί **PUSH για να ενδιάμεσου ΣΤΑΔΊΟΥ**. Για λεπτομερείς οδηγίες για τη δοκιμή την προσφορά στο περιβάλλον ανάπτυξης, ανατρέξτε σε αυτήν τη [σύνδεση](marketplace-publishing-vm-image-test-in-staging.md)
8. Αφού ελέγξετε την προσφορά στο ενδιάμεσου σταδίου, μεταβείτε στην καρτέλα **ΔΗΜΟΣΊΕΥΣΗ** δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku"></a>2. πώς μπορείτε να ενημερώσετε τις μη τεχνικές λεπτομέρειες προσφοράς ή άλλες SKU

Μπορείτε να ενημερώσετε τα μη τεχνικές (μάρκετινγκ, νομικές, υποστηρίζουν, κατηγορίες) λεπτομέρειες του live προσφορά ή SKU από το Azure Marketplace.

### <a name="21-update-the-offer-description-and-logos"></a>2.1 ενημερώστε την περιγραφή προσφοράς και λογότυπα

Μπορείτε να ενημερώσετε τις λεπτομέρειες προσφοράς και να δημοσιεύσετε εκ νέου την προσφορά, ακολουθώντας τα παρακάτω βήματα:

1. Συνδεθείτε στο τη [Δημοσίευση πύλη](https://publish.windowsazure.com).
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού πλευρά, κάντε κλικ στην καρτέλα **ΜΆΡΚΕΤΙΝΓΚ** .
4. Κάντε κλικ στο κουμπί **Στα ΑΓΓΛΙΚΆ (η.π.α.)** .
5. Από το αριστερό μενού πλευρά, κάντε κλικ στην καρτέλα **ΛΕΠΤΟΜΈΡΕΙΕΣ** . Κάτω από την ενότητα *ΠΕΡΙΓΡΑΦΉ* στην καρτέλα **ΛΕΠΤΟΜΈΡΕΙΕΣ** που μπορεί να ενημερώσετε τον τίτλο προσφορά, προσφέρουν σύνοψη, προσφέρουν μεγάλη σύνοψη και αποθηκεύστε τις αλλαγές.

    >[AZURE.NOTE] Επικοινωνήστε να χειριστείτε τα εξής κατά την ενημέρωση των λεπτομερειών SKU.
    **Δεν καταχωρείτε διπλότυπο κείμενο κάτω από την περιγραφή της προσφοράς και την περιγραφή SKU. Μην εισαγάγετε διπλότυπα κειμένου κάτω από τον τίτλο SKU και την προσφορά χρόνο σύνοψης. Μην εισαγάγετε διπλότυπα κειμένου κάτω από τον τίτλο SKU και τη σύνοψη την προσφορά.**

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)

6. Στην περιοχή *ΛΟΓΌΤΥΠΑ* στην καρτέλα " **ΛΕΠΤΟΜΈΡΕΙΕΣ** ", μπορείτε να ενημερώσετε τα λογότυπα. Ωστόσο, βεβαιωθείτε ότι τα λογότυπα, ακολουθήστε τις [οδηγίες Azure Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (ανατρέξτε στην ενότητα Βήμα 1: μάρκετινγκ περιεχομένου παρέχουν Marketplace -> λεπτομέρειες -> Azure Marketplace λογότυπο οδηγίες).

    >[AZURE.NOTE] Κύρια εικόνα εικονίδιο είναι προαιρετικό. Μπορείτε να επιλέξετε δεν για να αποστείλετε ένα εικονίδιο κύρια εικόνα. Ωστόσο, όταν αποστέλλεται εικονίδιο κύρια, στη συνέχεια, δεν υπάρχει πρόβλεψη για να το διαγράψετε από τη δημοσίευση πύλη. Σε αυτή την περίπτωση, πρέπει να ακολουθήσετε τις [οδηγίες εικονίδιο κύρια](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (ανατρέξτε στην ενότητα Βήμα 1: μάρκετινγκ περιεχομένου παρέχουν Marketplace -> λεπτομέρειες -> πρόσθετες οδηγίες για την κύρια λωρίδα λογότυπο).

7. Αφού κάνετε τις αλλαγές, μεταβείτε στην καρτέλα " **ΔΗΜΟΣΊΕΥΣΗ** " και κάντε κλικ στο κουμπί **PUSH για να ενδιάμεσου ΣΤΑΔΊΟΥ**. Για λεπτομερείς οδηγίες για τη δοκιμή την προσφορά στο περιβάλλον ανάπτυξης, ανατρέξτε σε αυτήν τη [σύνδεση](marketplace-publishing-vm-image-test-in-staging.md).
8. Αφού ελέγξετε την προσφορά στο ενδιάμεσου σταδίου, μεταβείτε στην καρτέλα **ΔΗΜΟΣΊΕΥΣΗ** δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="22-update-the-sku-description"></a>2.2. Ενημερώστε την περιγραφή SKU

Μπορείτε να ενημερώσετε τις λεπτομέρειες SKU και να δημοσιεύσετε εκ νέου την προσφορά, ακολουθώντας τα παρακάτω βήματα:

1. Σύνδεση για την [πύλη δημοσίευσης](https://publish.windowsazure.com)
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού πλευρά, κάντε κλικ στην καρτέλα **ΜΆΡΚΕΤΙΝΓΚ** .
4. Κάντε κλικ στο κουμπί **Στα ΑΓΓΛΙΚΆ (η.π.α.)** .
5. Από το αριστερό μενού πλευρά, κάντε κλικ στην καρτέλα **ΠΡΟΓΡΆΜΜΑΤΟΣ** . Κάτω από την ενότητα *SKU* καρτέλα **ΠΡΟΓΡΆΜΜΑΤΟΣ** μπορείτε να ενημερώσετε SKU τίτλος, SKU σύνοψης και λεπτομερειών περιγραφή SKU και να αποθηκεύσετε τις αλλαγές.

    >[AZURE.NOTE] Επικοινωνήστε να χειριστείτε τα εξής κατά την ενημέρωση των λεπτομερειών SKU. **Δεν καταχωρείτε διπλότυπο κείμενο κάτω από την περιγραφή της προσφοράς και την περιγραφή SKU. Δεν καταχωρείτε διπλότυπο κείμενο στην περιοχή τίτλου την SKU και την προσφορά χρόνο σύνοψης. Μην εισαγάγετε διπλότυπα κειμένου κάτω από τον τίτλο SKU και τη σύνοψη την προσφορά.**

6. Αφού κάνετε τις αλλαγές, μεταβείτε στην καρτέλα " **ΔΗΜΟΣΊΕΥΣΗ** " και κάντε κλικ στο κουμπί **PUSH για να ενδιάμεσου ΣΤΑΔΊΟΥ**. Για λεπτομερείς οδηγίες για τη δοκιμή την προσφορά στο περιβάλλον ανάπτυξης, ανατρέξτε σε αυτήν τη σύνδεση
7. Αφού ελέγξετε την προσφορά στο ενδιάμεσου σταδίου, μεταβείτε στην καρτέλα **ΔΗΜΟΣΊΕΥΣΗ** δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="23-change-the-existing-links-or-add-new-links"></a>2.3 αλλάξετε τις υπάρχουσες συνδέσεις ή να προσθέσετε νέες συνδέσεις

Μπορείτε να αλλάξετε τις υπάρχουσες συνδέσεις ή να προσθέσετε νέες συνδέσεις και, στη συνέχεια, να δημοσιεύσετε εκ νέου την προσφορά, ακολουθώντας τα παρακάτω βήματα:

1. Σύνδεση για την [πύλη δημοσίευσης](https://publish.windowsazure.com)
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού πλευρά, κάντε κλικ στην καρτέλα **ΜΆΡΚΕΤΙΝΓΚ** .
4. Κάντε κλικ στο κουμπί **Στα ΑΓΓΛΙΚΆ (η.π.α.)** .
5. Από το αριστερό μενού πλευρά, κάντε κλικ στην καρτέλα **ΣΥΝΔΈΣΕΙΣ** .
6. Εάν θέλετε να προσθέσετε μια νέα σύνδεση, στη συνέχεια, στην περιοχή οι *συνδέσεις* ενότητα κάντε κλικ στο κουμπί **Προσθήκη ΣΎΝΔΕΣΗΣ** . Θα ανοίξει το παράθυρο διαλόγου *"Προσθήκη σύνδεσης"* . Στο παράθυρο διαλόγου, μπορείτε να προσθέσετε τη σύνδεση τίτλου και διεύθυνση URL πεδία και να αποθηκεύσετε τις αλλαγές. Μπορείτε να εισαγάγετε οποιαδήποτε σύνδεση, το οποίο περιέχει πληροφορίες που μπορεί να βοηθήσει τους πελάτες.
7. Εάν θέλετε να ενημερώσετε ή διαγράψετε μια υπάρχουσα σύνδεση, στη συνέχεια, επιλέξτε την κατάλληλη σύνδεση και κάντε κλικ στο κουμπί Επεξεργασία ή στο κουμπί Διαγραφή ανάλογα.

    >[AZURE.NOTE] Βεβαιωθείτε ότι οι συνδέσεις που έχετε εισαγάγει σε αυτήν την ενότητα λειτουργούν σωστά, όπως αυτές τις συνδέσεις να επικυρώνονται κατά τη διάρκεια αίτησης διαδικασία παραγωγής.

8. Αφού κάνετε τις αλλαγές, μεταβείτε στην καρτέλα " **ΔΗΜΟΣΊΕΥΣΗ** " και κάντε κλικ στο κουμπί **PUSH για να ενδιάμεσου ΣΤΑΔΊΟΥ**. Για λεπτομερείς οδηγίες για τη δοκιμή την προσφορά στο περιβάλλον ανάπτυξης, ανατρέξτε σε αυτήν τη [σύνδεση](marketplace-publishing-vm-image-test-in-staging.md).
9. Αφού ελέγξετε την προσφορά στο ενδιάμεσου σταδίου, μεταβείτε στην καρτέλα **ΔΗΜΟΣΊΕΥΣΗ** δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="24-change-an-existing-sample-image-or-add-a-new-sample-image"></a>2.4 αλλάξετε μια υπάρχουσα εικόνα δείγμα ή να προσθέσετε ένα νέο δείγμα εικόνας

Μπορείτε να αλλάξετε μια υπάρχουσα δείγματα εικόνων ή προσθήκη νέου δείγμα εικόνων και, στη συνέχεια, να δημοσιεύσετε εκ νέου την προσφορά, ακολουθώντας τα παρακάτω βήματα:

>[AZURE.NOTE] Μόνο ένα δείγμα εικόνας εμφανίζεται στο το [https://portal.azure.com](https://portal.azure.com).

1. Σύνδεση για την [πύλη δημοσίευσης](https://publish.windowsazure.com)
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού πλευρά, κάντε κλικ στην καρτέλα **ΜΆΡΚΕΤΙΝΓΚ** .
4. Κάντε κλικ στο κουμπί **Στα ΑΓΓΛΙΚΆ (η.π.α.)** .
5. Από το αριστερό μενού, κάντε κλικ στην καρτέλα **ΔΕΊΓΜΑΤΑ ΕΙΚΌΝΩΝ** .
6. Εάν θέλετε να προσθέσετε ένα νέο δείγμα εικόνας, στη συνέχεια, στην ενότητα *Δείγμα εικόνες* κάντε κλικ στο κουμπί **ΑΠΟΣΤΟΛΉ ΝΈΑ ΕΙΚΌΝΑ** και, στη συνέχεια, αποθηκεύστε τις αλλαγές.

    >[AZURE.NOTE] Ένα δείγμα εικόνας όπως το βήμα είναι προαιρετικό.

7. Εάν θέλετε να ενημερώσετε ή να διαγράψετε μια υπάρχουσα εικόνα δείγμα, στη συνέχεια, εντοπίστε το κατάλληλο δείγμα εικόνας και κατόπιν κάντε κλικ στο κουμπί **ΑΝΤΙΚΑΤΆΣΤΑΣΗ ΕΙΚΌΝΑΣ** ή στο κουμπί Διαγραφή αντίστοιχα.

8. Αφού κάνετε τις αλλαγές, μεταβείτε στην καρτέλα " **ΔΗΜΟΣΊΕΥΣΗ** " και κάντε κλικ στο κουμπί **PUSH για να ενδιάμεσου ΣΤΑΔΊΟΥ**. Για λεπτομερείς οδηγίες για τη δοκιμή την προσφορά στο περιβάλλον ανάπτυξης, ανατρέξτε σε αυτήν τη [σύνδεση](marketplace-publishing-vm-image-test-in-staging.md).
9. Αφού ελέγξετε την προσφορά στο ενδιάμεσου σταδίου, μεταβείτε στην καρτέλα **ΔΗΜΟΣΊΕΥΣΗ** δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="25-update-the-legal-content"></a>2,5 ενημέρωση νομικού περιεχομένου

Μπορείτε να ενημερώσετε νομικού περιεχομένου και να δημοσιεύσετε εκ νέου την προσφορά, ακολουθώντας τα παρακάτω βήματα:

1. Σύνδεση για την [πύλη δημοσίευσης](https://publish.windowsazure.com)
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού πλευρά, κάντε κλικ στην καρτέλα **ΜΆΡΚΕΤΙΝΓΚ** .
4. Κάντε κλικ στο κουμπί **Στα ΑΓΓΛΙΚΆ (η.π.α.)** .
5. Από το αριστερό μενού, κάντε κλικ στην καρτέλα **ΝΟΜΙΚΉ** . Στην ενότητα *νομική* μπορείτε να ενημερώσετε τις πολιτικές/τους όρους χρήσης. Πληκτρολογήστε ή επικολλήστε πολιτικές/τους όρους της στο πλαίσιο κειμένου *Τους όρους χρήσης* και να αποθηκεύσετε τις αλλαγές.
6. Το όριο χαρακτήρων για την νομική τους όρους χρήσης που είναι 1.000.000 χαρακτήρες.
7. Αφού κάνετε τις αλλαγές, μεταβείτε στην καρτέλα " **ΔΗΜΟΣΊΕΥΣΗ** " και κάντε κλικ στο κουμπί **PUSH για να ενδιάμεσου ΣΤΑΔΊΟΥ**. Για λεπτομερείς οδηγίες για τη δοκιμή την προσφορά στο περιβάλλον ανάπτυξης, ανατρέξτε σε αυτήν τη [σύνδεση](marketplace-publishing-vm-image-test-in-staging.md)
8. Αφού ελέγξετε την προσφορά στο ενδιάμεσου σταδίου, μεταβείτε στην καρτέλα **ΔΗΜΟΣΊΕΥΣΗ** δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="26-update-the-support-information"></a>2.6 ενημέρωση των πληροφοριών υποστήριξης

Μπορείτε να ενημερώσετε τις πληροφορίες υποστήριξης και να δημοσιεύσετε εκ νέου την προσφορά, ακολουθώντας τα παρακάτω βήματα:

1. Σύνδεση για την [πύλη δημοσίευσης](https://publish.windowsazure.com)
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού πλευρά, κάντε κλικ στην καρτέλα **ΥΠΟΣΤΉΡΙΞΗ** .
4. Κάτω από την ενότητα *Μηχανικής επικοινωνήστε με* την **ΥΠΟΣΤΉΡΙΞΗ** καρτέλα μπορείτε να ενημερώσετε τα στοιχεία επικοινωνίας. Αυτές τις λεπτομέρειες που χρησιμοποιούνται για την εσωτερική επικοινωνία μεταξύ του συνεργάτη και Microsoft μόνο.
5. Στην ενότητα *Την υποστήριξη πελατών* της καρτέλας **ΥΠΟΣΤΉΡΙΞΗΣ** , μπορείτε να ενημερώσετε τα στοιχεία επικοινωνίας υποστήριξης, όπως το **όνομα, ηλεκτρονικό ταχυδρομείο, το τηλέφωνο** και **Διεύθυνση URL υποστήριξης**. Αυτές τις λεπτομέρειες που χρησιμοποιούνται για την εσωτερική επικοινωνία μεταξύ του συνεργάτη και Microsoft μόνο.

    >[AZURE.NOTE] Εάν θέλετε να δώσετε μόνο υποστήριξη μέσω ηλεκτρονικού ταχυδρομείου, δώστε έναν αριθμό τηλεφώνου εικονική κάτω από την ενότητα **Υποστήριξης πελατών** . Σε αυτήν την περίπτωση, ηλεκτρονικού ταχυδρομείου σας που θα χρησιμοποιηθεί αντί για αυτό.

6. Αφού κάνετε τις αλλαγές, μεταβείτε στην καρτέλα " **ΔΗΜΟΣΊΕΥΣΗ** " και κάντε κλικ στο κουμπί **PUSH για να ενδιάμεσου ΣΤΑΔΊΟΥ**. Για λεπτομερείς οδηγίες για τη δοκιμή την προσφορά στο περιβάλλον ανάπτυξης, ανατρέξτε σε αυτήν τη [σύνδεση](marketplace-publishing-vm-image-test-in-staging.md)
7. Αφού ελέγξετε την προσφορά στο ενδιάμεσου σταδίου, μεταβείτε στην καρτέλα **ΔΗΜΟΣΊΕΥΣΗ** δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="27-update-the-categories"></a>2.7 ενημερώστε τις κατηγορίες

Μπορείτε να ενημερώσετε την ενότητα κατηγορίες για την προσφορά σας και να δημοσιεύσετε εκ νέου την προσφορά, ακολουθώντας τα παρακάτω βήματα:

1. Σύνδεση για την [πύλη δημοσίευσης](https://publish.windowsazure.com)
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού, κάντε κλικ στην καρτέλα **ΚΑΤΗΓΟΡΊΕΣ** .
4. Στην ενότητα *κατηγορίες* μπορείτε να ενημερώσετε τις κατηγορίες για την προσφορά σας και να αποθηκεύσετε τις αλλαγές. Μπορείτε να επιλέξετε έως και πέντε κατηγορίες για τη συλλογή Azure Marketplace.
5. Αφού κάνετε τις αλλαγές, μεταβείτε στην καρτέλα " **ΔΗΜΟΣΊΕΥΣΗ** " και κάντε κλικ στο κουμπί **PUSH για να ενδιάμεσου ΣΤΑΔΊΟΥ**. Για λεπτομερείς οδηγίες για τη δοκιμή την προσφορά στο περιβάλλον ανάπτυξης, ανατρέξτε σε αυτήν τη [σύνδεση](marketplace-publishing-vm-image-test-in-staging.md)
6. Αφού ελέγξετε την προσφορά στο ενδιάμεσου σταδίου, μεταβείτε στην καρτέλα **ΔΗΜΟΣΊΕΥΣΗ** δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="3-how-to-add-a-new-sku-under-a-listed-offer"></a>3. πώς μπορείτε να προσθέσετε μια νέα SKU στην περιοχή μιας προσφοράς που παρατίθενται

Μπορείτε να προσθέσετε μια νέα SKU στην περιοχή προσφορά live σας, ακολουθώντας τα βήματα που παρέχονται παρακάτω:

1. Συνδεθείτε στο τη [Δημοσίευση πύλη](https://publish.windowsazure.com).
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού πλευρά, κάντε κλικ στην καρτέλα **SKU** . Μετά από αυτό κάντε κλικ στο κουμπί **ADD A SKU**.  Θα ανοίξει ένα νέο παράθυρο διαλόγου. Εισαγάγετε ένα αναγνωριστικό SKU με πεζά γράμματα. Επιλέξτε το πλαίσιο ελέγχου για μεταφορά-σας-κάτοχος χρεώσεων model(BYOL) Εάν θέλετε να δημοσιεύσετε το νέο SKU με BYOL χρεώσεων μοντέλο. Διαφορετικά, καταργήστε την επιλογή από το πλαίσιο ελέγχου για BYOL. Μετά από αυτό κάντε κλικ στο σημάδι υποδιαίρεσης στο παράθυρο διαλόγου για να δημιουργήσετε μια νέα SKU. Εάν δεν επιλέξετε για το μοντέλο BYOL χρέωσης για τη νέα SKU, στη συνέχεια, το μοντέλο χρεώσεων θα αυτόματα οριστεί για κάθε ώρα για τη νέα SKU. Εάν θέλετε να ενεργοποιήσετε το δωρεάν δοκιμαστική έκδοση 30days για ωριαία χρεώσεων μοντέλο, στη συνέχεια, κάντε κλικ στην επιλογή "Ένα μήνα" για "είναι μια δωρεάν δοκιμαστική έκδοση διαθέσιμη;". Διαφορετικά, επιλέξτε "ΧΩΡΊΣ δοκιμαστικής ΈΚΔΟΣΗΣ". [Σημείωση: Η επιλογή "είναι μια δωρεάν δοκιμαστική έκδοση διαθέσιμη;" εμφανίζεται μόνο εάν δεν έχετε επιλέξει BYOL στο παράθυρο διαλόγου κατά τη δημιουργία του νέου SKU.]

    >[AZURE.IMPORTANT] Η επιλογή "Απόκρυψη αυτό SKU από το Marketplace επειδή πρέπει πάντα να αγοράσατε μέσω ενός προτύπου λύσης" πρέπει να έχει επισημανθεί ως "Ναι" ΜΌΝΟ αν έχουν εγκριθεί για τη δημοσίευση μιας προσφοράς πρότυπο λύσης από το Azure Marketplace. Διαφορετικά, αυτή η επιλογή πρέπει πάντα να επισημαίνεται ως "ΌΧΙ".

4. Τώρα από το αριστερό μενού, κάντε κλικ στην καρτέλα **Εικονική ΕΙΚΌΝΕΣ** και βρείτε το νέο SKU που έχετε δημιουργήσει.
5. Για να ρυθμίσετε το νέο SKU, ανατρέξτε το ΒΉΜΑ 5 του αυτήν τη [σύνδεση](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) για οδηγίες.
6. Για να προσθέσετε στο υλικό μάρκετινγκ για τη νέα SKU, ανατρέξτε στην ενότητα Βήμα 1: μάρκετινγκ περιεχομένου παρέχουν Marketplace -> λεπτομέρειες -> σημείο αριθμών 2 έως 5 αυτής της [σύνδεσης](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Για να προσθέσετε τις πληροφορίες τιμολόγησης για τη νέα SKU, ανατρέξτε στην ενότητα 2.1. Ορίστε τις τιμές Εικονική αυτής της [σύνδεσης](marketplace-publishing-push-to-staging.md#step-2-set-your-prices)
8. Αφού κάνετε τις αλλαγές, μεταβείτε στην καρτέλα " **ΔΗΜΟΣΊΕΥΣΗ** " και κάντε κλικ στο κουμπί **PUSH για να ενδιάμεσου ΣΤΑΔΊΟΥ**. Για λεπτομερείς οδηγίες για τη δοκιμή την προσφορά στο περιβάλλον ανάπτυξης, ανατρέξτε σε αυτήν τη [σύνδεση](marketplace-publishing-vm-image-test-in-staging.md)
9. Αφού ελέγξετε την προσφορά στο ενδιάμεσου σταδίου, μεταβείτε στην καρτέλα **ΔΗΜΟΣΊΕΥΣΗ** δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="4-how-to-change-the-data-disk-count-for-a-listed-sku"></a>4. πώς μπορείτε να αλλάξετε το πλήθος δίσκου δεδομένων για άλλες SKU που παρατίθενται

Που δεν είναι δυνατό να διαβάθμιση/μείωση το πλήθος δίσκου δεδομένων που παρατίθενται άλλες SKU. Χρειάζεστε σε αυτήν την περίπτωση δημιουργείτε μια νέα SKU. Ανατρέξτε στην ενότητα [3. Πώς μπορείτε να προσθέσετε μια νέα SKU στην περιοχή μια ζωντανή προσφορά](#3-how-to-add-a-new-sku-under-a-live-offer) για λεπτομερείς οδηγίες.

## <a name="5---how-to-delete-a-listed-offer-from-the-azure-marketplace"></a>5. πώς μπορείτε να διαγράψετε μια προσφορά που παρατίθενται από το Azure Marketplace

Υπάρχουν διάφορες πτυχές που πρέπει να ληφθούν φροντίσει σε περίπτωση μια αίτηση για να καταργήσετε μια ζωντανή προσφορά. Ακολουθήστε τα παρακάτω βήματα για να λάβετε οδηγίες από την ομάδα υποστήριξης για να καταργήσετε μια προσφορά που παρατίθενται από το Azure Marketplace:

1.  Να αυξήσετε ένα δελτίο υποστήριξης χρησιμοποιώντας αυτήν τη [σύνδεση](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681)
2.  Επιλέξτε τον τύπο πρόβλημα ως **"Διαχείριση προσφορών"** και επιλέξτε κατηγορία ως **"Τροποποίηση μιας προσφοράς ή/και SKU ήδη στο παραγωγής"**
3.  Υποβολή της αίτησης

Η ομάδα υποστήριξης θα σας καθοδηγήσει η διαδικασία διαγραφής προσφορά/SKU.

>[AZURE.NOTE] Μπορείτε πάντα να διαγράψετε την προσφορά ενώ βρίσκεται σε μια κατάσταση Πρόχειρο (δηλαδή, όχι σε ΟΡΓΆΝΩΣΗΣ ή ΠΑΡΑΓΩΓΉΣ), κάνοντας κλικ στο κουμπί **ΠΡΌΧΕΙΡΟ ΑΠΌΡΡΙΨΗ** κάτω από την καρτέλα **ΙΣΤΟΡΙΚΌ** .

## <a name="6-how-to-delete-a-listed-sku-from-the-azure-marketplace"></a>6. πώς μπορείτε να διαγράψετε μια που παρατίθενται SKU από το Azure Marketplace

Μπορείτε να διαγράψετε μια που παρατίθενται SKU από το Azure Marketplace, ακολουθώντας τα βήματα που παρέχονται παρακάτω:

1. Συνδεθείτε στο τη [Δημοσίευση πύλη](https://publish.windowsazure.com).
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό πλαϊνό τμήμα παραθύρου, κάντε κλικ στην καρτέλα **τα SKU** .
4. Επιλέξτε την SKU που θέλετε να διαγράψετε και κάντε κλικ στο κουμπί διαγραφής σε σχέση με συγκεκριμένο SKU.
5. Μετά την ολοκλήρωση, μεταβείτε στην καρτέλα ΔΗΜΟΣΊΕΥΣΗ δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.
6. Μόλις την προσφορά λαμβάνει εκ νέου δημοσιευτεί από το Azure Marketplace, την SKU θα διαγραφούν από το Azure Marketplace και την πύλη Azure.

## <a name="7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace"></a>7. πώς μπορείτε να διαγράψετε την τρέχουσα έκδοση του που παρατίθενται άλλες SKU από το Azure Marketplace

Μπορείτε να διαγράψετε την τρέχουσα έκδοση του που παρατίθενται άλλες SKU από το Azure Marketplace, ακολουθώντας τα βήματα που παρέχονται παρακάτω. Μόλις ολοκληρωθεί η διαδικασία, την SKU θα γίνει επαναφορά την προηγούμενη έκδοση.

1. Συνδεθείτε στο τη [Δημοσίευση πύλη](https://publish.windowsazure.com).
2.  Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3.  Από το αριστερό πλαϊνό τμήμα παραθύρου, κάντε κλικ στην καρτέλα **Εικονική ΕΙΚΌΝΕΣ** .
4.  Επιλέξτε την SKU του οποίου η τρέχουσα έκδοση που θέλετε να διαγράψετε και κάντε κλικ στο κουμπί "Διαγραφή" σε σχέση με αυτήν την έκδοση.
5.  Μετά την ολοκλήρωση, μεταβείτε στην καρτέλα **ΔΗΜΟΣΊΕΥΣΗ** δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.
6.  Μόλις την προσφορά λαμβάνει εκ νέου δημοσιευτεί από το Azure Marketplace, την τρέχουσα έκδοση του την SKU που παρατίθενται θα διαγραφούν από το Azure Marketplace και την πύλη Azure. Το SKU θα γίνει επαναφορά την προηγούμενη έκδοση.

## <a name="8-how-to-revert-listing-price-to-production-values"></a>8. πώς μπορείτε να επαναφέρετε καταχώρηση τιμή παραγωγής τιμές
Να έχετε αλλάξει την τιμολόγηση των που παρατίθενται άλλες SKU (ή να έχετε καταργήσει χρεώσεων περιοχές του που παρατίθενται άλλες SKU). Επειδή δεν υποστηρίζεται από το Azure Marketplace, να θέλετε να επαναφέρετε τις αλλαγές μου στις τιμές παραγωγής. Πώς μπορώ να επιτύχετε που;

Ακολουθήστε τα βήματα που παρέχονται παρακάτω:

1. Συνδεθείτε στο τη [Δημοσίευση πύλη](https://publish.windowsazure.com).
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού, κάντε κλικ στην καρτέλα **ΤΙΜΟΛΌΓΗΣΗ** .
4. Στην καρτέλα Τιμολόγηση, επιλέξτε μια περιοχή του οποίου τις τιμές που θέλετε να επαναφέρετε.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)

5. Σε περίπτωση SKU με ωριαία χρεώσεων μοντέλο, επαναφέρετε τις τιμές για όλα τα πυρήνων ως έχουν την παραγωγή για την επιλεγμένη περιοχή. Για SKU με BYOL χρεώσεων μοντέλο, διάθεση την SKU στην περιοχή, επιλέγοντας το πλαίσιο ελέγχου σε σχέση με την SKU στην ενότητα ΔΙΑΘΕΣΙΜΌΤΗΤΑ SKU EXTERNALLY-LICENSED (BYOL) (δείτε το στιγμιότυπο οθόνης παρακάτω).

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)

6. Τώρα, κάντε κλικ στο κουμπί **AUTOPRICE ΆΛΛΕΣ ΑΓΟΡΈΣ ΒΆΣΕΙ ON ΤΙΜΈΣ σε UNITED ΜΈΛΗ**.

    >[AZURE.NOTE] Το κουμπί ετικέτα μπορεί να είναι διαφορετικά, ανάλογα με την περιοχή που έχετε επιλέξει. Εφόσον μας έχετε επιλέξει Ηνωμένες Πολιτείες κατά τη δημιουργία αυτού του εγγράφου, ώστε το κουμπί με το χαρακτηρισμό "Αυτόματη τιμή άλλες αγορές με βάση τις τιμές στις Ηνωμένες Πολιτείες" στο παρακάτω στιγμιότυπο οθόνης.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)

7. Θα ανοίξει ο οδηγός αυτόματο τιμή. Στην πρώτη σελίδα εμφανίζει την επιλογή για αγορά βάσης. Κάντε την ενότητα και μετακίνηση στην επόμενη σελίδα, κάνοντας κλικ στο κουμπί **"->"** .

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)

8. Επιλογή για να χρησιμοποιήσετε τα προγράμματα και πυρήνων θα εμφανίζονται στη σελίδα 2. Επιλέξτε την επιθυμητή σχέδια και το πυρήνων και κάντε κλικ στην επιλογή "->" κουμπί.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)

9. Σελίδα 3 εμφανίζει τις αγορές/περιοχές. Κάντε κλικ στο κουμπί εναλλαγής όλων για να επιλέξετε όλες τις περιοχές ή με μη αυτόματο τρόπο την επιλογή των πλαισίων ελέγχου για την περιοχή. Κάντε κλικ στο κουμπί "->" για να μετακινηθείτε στην επόμενη σελίδα.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)

10. Σελίδα 4 εμφανίζει τις χρεώσεις του exchange. Κάντε κλικ στο κουμπί Τέλος για να ολοκληρώσετε τα βήματα. Ο οδηγός θα επαναφέρετε την τιμολόγηση σύμφωνα με την επιλογή σας.

11. Τώρα, μεταβείτε στην καρτέλα τιμολόγησης και κάντε κλικ στο κουμπί "ΠΡΟΒΟΛΉ ΣΎΝΟΨΗΣ και ΑΛΛΑΓΏΝ".
Επιλέξτε "Πρόχειρο" στην ενότητα "Προβολή εκδόσεων" και "Παραγωγή" στην ενότητα "Σύγκριση με" (δείτε το στιγμιότυπο οθόνης παρακάτω). Εάν βλέπετε τις πληροφορίες τιμολόγησης διαφορά, αυτό σημαίνει τις τιμές έγινε επαναφορά στις τιμές παραγωγής με επιτυχία.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)

12. Αφού κάνετε τις αλλαγές, μεταβείτε στην καρτέλα "ΔΗΜΟΣΊΕΥΣΗ" και κάντε κλικ στο κουμπί **PUSH για να ενδιάμεσου ΣΤΑΔΊΟΥ**. Για λεπτομερείς οδηγίες για τη δοκιμή την προσφορά στο περιβάλλον ανάπτυξης, ανατρέξτε σε αυτήν τη [σύνδεση](marketplace-publishing-vm-image-test-in-staging.md)
13. Αφού ελέγξετε την προσφορά στο ενδιάμεσου σταδίου, μεταβείτε στην καρτέλα ΔΗΜΟΣΊΕΥΣΗ δημοσίευσης πύλη και κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

## <a name="9-how-to-revert-billing-model-to-production-values"></a>9. πώς μπορείτε να επαναφέρετε χρεώσεων μοντέλο παραγωγής τιμές
Να έχουν αλλάξει το μοντέλο χρέωσης που παρατίθενται άλλες SKU. Επειδή δεν υποστηρίζεται από το Azure Marketplace, να θέλετε να επαναφέρετε τις αλλαγές μου στις τιμές παραγωγής. Πώς μπορώ να επιτύχετε που;

Ακολουθήστε τα παρακάτω βήματα:

1. Συνδεθείτε στο τη [Δημοσίευση πύλη](https://publish.windowsazure.com).
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού, κάντε κλικ στην καρτέλα **τα SKU** .
4. Κάντε κλικ στο κουμπί "ΕΠΕΞΕΡΓΑΣΊΑ" για να επαναφέρετε το μοντέλο χρέωσης. Θα ανοίξει ένα παράθυρο. Επιλέξτε ή καταργήστε την επιλογή από το πλαίσιο ελέγχου **'χρεώσεις και τις άδειες χρήσης γίνεται εξωτερικά από το Azure (ή μεταφορά των δικών άδεια χρήσης)'** αντίστοιχα.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)

5. Μία φορά, ανατρέξτε στην απάντηση της ερώτησης 8 σε αυτό το έγγραφο για να επιστρέψετε ξανά την τιμολόγηση.
6. Αφού που μεταβείτε στην καρτέλα **ΔΗΜΟΣΊΕΥΣΗ** δημοσίευσης πύλης και push την προσφορά για να ενδιάμεσου για να την δοκιμάσετε. Μία φορά που γίνονται με δοκιμής την προσφορά, στη συνέχεια, κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

## <a name="10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value"></a>10. πώς μπορείτε να επαναφέρετε ρύθμιση ορατότητας άλλες SKU που παρατίθενται στην τιμή παραγωγής

Ακολουθήστε τα παρακάτω βήματα:

1. Συνδεθείτε στο τη [Δημοσίευση πύλη](https://publish.windowsazure.com).
2. Μεταβείτε στην καρτέλα **ΕΙΚΟΝΙΚΈΣ ΜΗΧΑΝΈΣ** και επιλέξτε την προσφορά.
3. Από το αριστερό μενού, κάντε κλικ στην καρτέλα **τα SKU** .
4. Επιλέξτε τις SKU και να επαναφέρετε τη ρύθμιση ορατότητας την SKU στην τιμή παραγωγής.

    ![σχέδιο](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)

5. Μία φορά που γίνονται με τις αλλαγές, στη συνέχεια, κάντε κλικ στο κουμπί **Αίτηση ΈΓΚΡΙΣΗΣ για PUSH για να ΠΑΡΑΓΩΓΉΣ** για να δημοσιεύσετε εκ νέου την προσφορά από το Azure Marketplace.

## <a name="see-also"></a>Δείτε επίσης
- [Γρήγορα αποτελέσματα: Πώς μπορείτε να δημοσιεύσετε μια προσφορά για να το Azure Marketplace](marketplace-publishing-getting-started.md)
- [Κατανόηση των πωλητή ιδέες αναφοράς](marketplace-publishing-report-seller-insights.md)
- [Κατανόηση των εξόφληση αναφοράς](marketplace-publishing-report-payout.md)
- [Πώς μπορείτε να αλλάξετε την υπηρεσία παροχής λύσεων Cloud προσφορά μεταπωλητή](marketplace-publishing-csp-incentive.md)
- [Αντιμετώπιση προβλημάτων συνηθισμένα προβλήματα δημοσίευσης στην αγορά](marketplace-publishing-support-common-issues.md)
- [Λήψη υποστήριξης ως εκδότη](marketplace-publishing-get-publisher-support.md)
- [Δημιουργία μιας εικόνας Εικονική εσωτερικής εγκατάστασης](marketplace-publishing-vm-image-creation-on-premise.md)
- [Δημιουργήστε μια εικονική μηχανή με Windows στην πύλη του Azure preview](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
