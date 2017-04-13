<properties
    pageTitle="Ρύθμιση παραμέτρων πάντα στην ομάδα διαθεσιμότητα σε Εικονική Azure αυτόματα - διαχείριση πόρων"
    description="Δημιουργήστε μια ομάδα διαθεσιμότητα πάντα σε με Azure εικονικές μηχανές σε λειτουργία Azure διαχείριση πόρων. Αυτό το πρόγραμμα εκμάθησης κυρίως χρησιμοποιεί το περιβάλλον εργασίας χρήστη για να δημιουργήσετε αυτόματα ολόκληρη τη λύση."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-always-on-availability-group-in-azure-vm-automatically---resource-manager"></a>Ρύθμιση παραμέτρων πάντα στην ομάδα διαθεσιμότητα σε Εικονική Azure αυτόματα - διαχείριση πόρων

> [AZURE.SELECTOR]
- [Διαχείριση πόρων: προτύπου](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Διαχείριση πόρων: μη αυτόματος](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Κλασική: περιβάλλον εργασίας Χρήστη](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Κλασική: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

Αυτό το πρόγραμμα εκμάθησης-τελικών δείχνει πώς μπορείτε να δημιουργήσετε μια ομάδα διαθεσιμότητα SQL Server με τη διαχείριση πόρων Azure εικονικές μηχανές. Το πρόγραμμα εκμάθησης χρησιμοποιεί Azure λεπίδες για να ρυθμίσετε τις παραμέτρους ενός προτύπου. Θα δείτε τις προεπιλεγμένες ρυθμίσεις, πληκτρολογήστε τις απαιτούμενες ρυθμίσεις, και ενημερώστε τις λεπίδες στην πύλη όπως καθοδηγήσει αυτό το πρόγραμμα εκμάθησης.

Στο τέλος του προγράμματος εκμάθησης, η λύση ομάδα διαθεσιμότητα SQL Server στο Azure αποτελείται από τα εξής στοιχεία:

- Ένα εικονικό δίκτυο που περιέχει πολλά δευτερεύοντα δίκτυα, όπως μια προσκηνίου και παρασκηνίου υποδίκτυο

- Δύο ελεγκτή με έναν τομέα Active Directory (AD)

- Δύο SQL Server ΣΠΣ αναπτύσσεται στο υποδίκτυο παρασκηνίου και να συνδεθεί με τον τομέα AD

- Ένα σύμπλεγμα WSFC 3-κόμβο με το μοντέλο απαρτίας μεγαλύτερο μέρος κόμβου

- Μια ομάδα διαθεσιμότητα με δύο αντίγραφα σύγχρονη ολοκλήρωσης μιας βάσης δεδομένων διαθεσιμότητας

Η παρακάτω εικόνα αποτελεί γραφική αναπαράσταση της λύσης.

![Δοκιμή αρχιτεκτονική εργαστήριο για AG στο Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Όλοι οι πόροι σε αυτήν τη λύση ανήκουν σε μια ομάδα μεμονωμένο πόρο.

Αυτό το πρόγραμμα εκμάθησης προϋποθέτει τα εξής:

- Έχετε ήδη ένα λογαριασμό Azure. Εάν δεν έχετε ένα, [εγγραφείτε για ένα λογαριασμό δοκιμαστικής έκδοσης](http://azure.microsoft.com/pricing/free-trial/).

- Γνωρίζετε ήδη τον τρόπο παροχής μια Εικονική SQL Server από τη συλλογή εικονική μηχανή χρησιμοποιώντας το Γραφικών. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [προμήθεια μια εικονική μηχανή SQL Server στο Azure](virtual-machines-windows-portal-sql-server-provision.md)

- Έχετε ήδη ένα συμπαγές Κατανόηση των ομάδων διαθεσιμότητα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πάντα σε ομάδες διαθεσιμότητας (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Εάν σας ενδιαφέρει χρησιμοποιώντας διαθεσιμότητα ομάδες με το SharePoint, δείτε επίσης [Ρύθμιση παραμέτρων του SQL Server 2012 πάντα σε ομάδες διαθεσιμότητα για το SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).

Σε αυτό το πρόγραμμα εκμάθησης θα χρησιμοποιήσετε το Azure πύλη για να:

- Επιλέξτε το το πρότυπο πάντα σε από την πύλη

- Ελέγξτε τις ρυθμίσεις του προτύπου και ενημερώστε τις μερικές ρυθμίσεις παραμέτρων για το περιβάλλον σας

- Οθόνη Azure όπως δημιουργεί ολόκληρο το περιβάλλον

- Σύνδεση σε έναν από τους ελεγκτές τομέα και, στη συνέχεια, σε έναν από τους διακομιστές SQL

[AZURE.INCLUDE [availability-group-template](../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]


## <a name="provision-the-cluster-from-the-gallery"></a>Παροχή του συμπλέγματος από τη συλλογή

Azure παρέχει μια εικόνα της συλλογής για ολόκληρη τη λύση. Για να βρείτε το πρότυπο:

1.  Συνδεθείτε στην πύλη του Azure χρησιμοποιώντας το λογαριασμό σας.
1.  Στην περιοχή Επιλέξτε Azure πύλης **+ Δημιουργία.** Η πύλη θα ανοίξει το νέο blade.
1.  Στη νέα blade αναζήτηση **AlwaysOn**.
![Βρείτε το πρότυπο AlwaysOn](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
1.  Στα αποτελέσματα αναζήτησης, εντοπίστε **Συμπλέγματος AlwaysOn του SQL Server**.
![Πρότυπο AlwaysOn](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
1.  Στην **επιλογή ένα μοντέλο ανάπτυξης** , επιλέξτε **Διαχείριση πόρων**.

### <a name="basics"></a>Βασικά στοιχεία

Κάντε κλικ στην **βασικά στοιχεία** και να ρυθμίσετε τα ακόλουθα:

- **Όνομα χρήστη του διαχειριστή** είναι ένας λογαριασμός χρήστη με δικαιώματα διαχειριστή τομέα και ένα μέλος του ρόλου σταθερό διακομιστή sysadmin SQL Server σε δύο παρουσίες του SQL Server. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **διαχείρισης τομέα**.

- **Κωδικός πρόσβασης** είναι ο κωδικός πρόσβασης για το λογαριασμό διαχειριστή τομέα. Χρησιμοποιήστε μια σύνθετη κωδικό πρόσβασης. Επιβεβαιώστε τον κωδικό πρόσβασης.

- **Η συνδρομή** είναι τη συνδρομή στην οποία θα γραμματίου Azure για να εκτελέσετε όλους τους πόρους που αναπτύσσεται για την ομάδα διαθεσιμότητα. Μπορείτε να καθορίσετε μια διαφορετική συνδρομή, εάν ο λογαριασμός σας έχει πολλές συνδρομές.

- **Ομάδα πόρων** είναι το όνομα για την ομάδα ότι όλοι οι πόροι Azure δημιουργήθηκε από αυτό το πρόγραμμα εκμάθησης, θα ανήκουν σε. Για αυτό το πρόγραμμα εκμάθησης χρήση **SQL-HA-RG**. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα (επισκόπηση διαχείρισης πόρων Azure) [- ομάδα-overview.md / # πόρων-ομάδες πόρων].

- **Θέση** είναι η περιοχή Azure όπου θα δημιουργηθεί τους πόρους για αυτό το πρόγραμμα εκμάθησης. Επιλέξτε μια περιοχή Azure για τη φιλοξενία της υποδομής.

Ακολουθεί το πώς θα μοιάζει το blade **βασικά στοιχεία** :

![Βασικά στοιχεία](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

- Κάντε κλικ στο **κουμπί OK**.

### <a name="domain-and-network-settings"></a>Ρυθμίσεις δικτύου και τομέα

Αυτό το πρότυπο συλλογή Azure δημιουργεί ένα νέο τομέα με νέους ελεγκτές τομέα. Επίσης, δημιουργεί ένα νέο δίκτυο και δύο δευτερευόντων δικτύων. Το πρότυπο δεν επιτρέπει τη δημιουργία τους διακομιστές σε έναν υπάρχοντα τομέα ή εικονικού δικτύου. Το επόμενο βήμα είναι να ρυθμίσετε τις παραμέτρους δικτύου και τον τομέα.

Σχετικά με τις **Ρυθμίσεις δικτύου και τομέα** blade εξετάστε τις προκαθορισμένες τιμές για τις ρυθμίσεις δικτύου και τον τομέα:

- **Όνομα τομέα ριζικού δάσος** είναι το όνομα τομέα που θα χρησιμοποιηθεί για τον τομέα AD που θα φιλοξενήσει το σύμπλεγμα. Για το πρόγραμμα εκμάθησης, χρησιμοποιήστε **contoso.com**.

- **Όνομα εικονικού δικτύου** είναι το όνομα του δικτύου για το Azure εικονικού δικτύου. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **autohaVNET**.

- **Όνομα υποδικτύου ελεγκτή τομέα** είναι το όνομα του ένα τμήμα του εικονικού δικτύου που φιλοξενεί ελεγκτή τομέα. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **υποδικτύου-1**. Αυτό το υποδίκτυο θα χρησιμοποιήσει τη διεύθυνση πρόθεμα **10.0.0.0/24**.

- **Όνομα δευτερεύοντος δικτύου του SQL Server** είναι το όνομα του ένα τμήμα του εικονικού δικτύου που φιλοξενεί τους διακομιστές SQL και το αρχείο κοινή χρήση μαρτυρίας. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **υποδικτύου-2**. Αυτό το υποδίκτυο θα χρησιμοποιήσει τη διεύθυνση πρόθεμα **10.0.1.0/26**.

Για να μάθετε περισσότερα σχετικά με τα δίκτυα εικονικού στο [Azure ανατρέξτε στο θέμα Επισκόπηση εικονικού δικτύου](../virtual-network/virtual-networks-overview.md).  

Οι **Ρυθμίσεις δικτύου και τομέα** πρέπει να μοιάζει ως εξής:

![Ρυθμίσεις δικτύου και τομέα](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Εάν είναι απαραίτητο, μπορείτε να αλλάξετε αυτές τις τιμές. Για αυτό το πρόγραμμα εκμάθησης χρησιμοποιούμε τις προκαθορισμένες τιμές.

- Ελέγξτε τις ρυθμίσεις και κάντε κλικ στο κουμπί **OK**.

###<a name="availability-group-settings"></a>Οι ρυθμίσεις της ομάδας διαθεσιμότητας

Σχετικά με τις **ρυθμίσεις ομάδας διαθεσιμότητας** , εξετάστε τις προκαθορισμένες τιμές για την ομάδα διαθεσιμότητα και την παρακολούθηση.

- **Όνομα ομάδας διαθεσιμότητας** είναι το όνομα του πόρου συμπλέγματος για την ομάδα διαθεσιμότητα. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **Contoso ag**.

- **όνομα ακρόασης ομάδας διαθεσιμότητα** χρησιμοποιείται από το σύμπλεγμα και την εσωτερική εξισορρόπηση φόρτου. Πελάτες που συνδέονται με τον SQL Server να χρησιμοποιήσετε αυτό το όνομα για να συνδεθείτε με την κατάλληλη ρεπλίκα της βάσης δεδομένων. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **Contoso-ακρόασης**.

-  **θύρα ακρόασης ομάδα διαθεσιμότητα** Καθορίζει τη θύρα TCP το ακροατήριο SQL Server θα χρησιμοποιήσετε. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε την προεπιλεγμένη θύρα **1433**.

Εάν είναι απαραίτητο, μπορείτε να αλλάξετε αυτές τις τιμές. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε τις προκαθορισμένες τιμές.  

![Οι ρυθμίσεις της ομάδας διαθεσιμότητας](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

- Κάντε κλικ στο **κουμπί OK**.

###<a name="vm-size-storage-settings"></a>Εικονική μέγεθος, ρυθμίσεις χώρου αποθήκευσης

**Εικονική μέγεθος** , ρυθμίσεις αποθήκευσης επιλογή μεγέθους εικονική μηχανή SQL Server και εξετάστε τις άλλες ρυθμίσεις.

- **Μέγεθος εικονική μηχανή SQL Server** είναι το μέγεθος Azure εικονική μηχανή για τους δύο διακομιστές SQL. Επιλέξτε ένα μέγεθος εικονική μηχανή κατάλληλη για το φόρτο εργασίας. Εάν δημιουργείτε αυτού του περιβάλλοντος για το πρόγραμμα εκμάθησης Χρησιμοποιήστε **DS2**. Για την παραγωγή φόρτους εργασίας επιλέξτε ένα μέγεθος εικονική μηχανή που μπορεί να υποστηρίξει το φόρτο εργασίας. Πολλά φόρτους εργασίας παραγωγής θα απαιτούν **DS4** ή μεγαλύτερο. Το πρότυπο θα δημιουργήσετε δύο εικονικές μηχανές αυτού του μεγέθους και να εγκαταστήσετε το SQL Server σε κάθε μία. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [μεγέθη για εικονικές μηχανές](virtual-machines-linux-sizes.md).

>[AZURE.NOTE]Azure θα εγκαταστήσετε το Enterprise Edition SQL Server. Το κόστος εξαρτάται από την έκδοση και το μέγεθος εικονική μηχανή. Για λεπτομερείς πληροφορίες σχετικά με το τρέχον κόστος, ανατρέξτε στο θέμα [εικονικές μηχανές τις πληροφορίες τιμολόγησης](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

- **Μέγεθος εικονική μηχανή ελεγκτή τομέα** είναι το μέγεθος εικονική μηχανή για τους ελεγκτές τομέα. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **D2**.

- **Μέγεθος εικονική μηχανή μαρτυρίας κοινή χρήση αρχείου** είναι το μέγεθος εικονική μηχανή για το αρχείο μαρτυρίας κοινή χρήση. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **A1**.

- **Ο λογαριασμός χώρου αποθήκευσης SQL** είναι το όνομα του λογαριασμού χώρου αποθήκευσης για να χωρέσουν τα δεδομένα του SQL Server και το λειτουργικό σύστημα δίσκων. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **alwaysonsql01**.

- **Λογαριασμός κέντρο Δεδομένων του χώρου αποθήκευσης** είναι το όνομα του λογαριασμού χώρου αποθήκευσης για ελεγκτές τομέα. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **alwaysondc01**.

- **Δεδομένα του SQL Server στο δίσκο μεγέθους** στο TB είναι το μέγεθος του δίσκου δεδομένων SQL Server στο TB. Καθορίστε έναν αριθμό από 1 έως 4. Αυτό είναι το μέγεθος του δίσκου δεδομένων που θα επισυναφθεί σε κάθε SQL Server. Για αυτό το πρόγραμμα εκμάθησης χρήσης **1**.

- **Βελτιστοποίηση αποθήκευσης** ορίζει ρυθμίσεις παραμέτρων συγκεκριμένο χώρο αποθήκευσης για τις εικονικές μηχανές SQL Server με βάση τον τύπο φόρτο εργασίας. Όλους τους διακομιστές SQL σε αυτό το σενάριο Χρησιμοποιήστε premium χώρου αποθήκευσης με μνήμη cache host Azure δίσκου μόνο για ανάγνωση. Επιπλέον, μπορείτε να βελτιστοποιήσετε ρυθμίσεις SQL Server για το φόρτο εργασίας, επιλέγοντας μία από αυτές τις τρεις ρυθμίσεις:

    - **Φόρτο εργασίας γενικά** σύνολα δεν υπάρχουν ρυθμίσεις συγκεκριμένη ρύθμιση παραμέτρων

    - **Επεξεργασία συναλλαγών** ορίζει τη σημαία παρακολούθησης 1117 και 1118

    - **Αποθήκευση δεδομένων** ορίζει τη σημαία παρακολούθησης 1117 και 610

Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **Γενικά φόρτο εργασίας**.

![Εικονική μέγεθος αποθήκευσης ρυθμίσεων](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

- Ελέγξτε τις ρυθμίσεις και κάντε κλικ στο κουμπί **OK**.

####<a name="a-note-about-storage"></a>Σημείωση σχετικά με το χώρο αποθήκευσης

Πρόσθετες βελτιστοποιήσεις εξαρτώνται από το μέγεθος των δίσκων δεδομένων SQL Server. Για κάθε terabyte του δίσκου δεδομένων, Azure προσθέτει μια επιπλέον χώρο αποθήκευσης premium 1 TB (SSD). Όταν ένας διακομιστής απαιτεί 2 TB ή περισσότερα, το πρότυπο δημιουργεί ένα χώρο συγκέντρωσης χώρου αποθήκευσης σε κάθε SQL Server. Ένα σύνολο χώρου αποθήκευσης είναι μια φόρμα αναπαράστασης αποθήκευσης όπου πολλών δίσκων έχουν ρυθμιστεί για την παροχή υψηλότερη δυναμικότητας, υποστηρίζεται και επιδόσεων.  Το πρότυπο, στη συνέχεια, δημιουργεί ένα χώρο αποθήκευσης στο χώρο συγκέντρωσης χώρου αποθήκευσης και αυτό παρουσιάζει ως μεμονωμένο δεδομένων για το λειτουργικό σύστημα. Το πρότυπο ορίζει αυτού του δίσκου ως ο δίσκος δεδομένων για τον SQL Server. Το πρότυπο ρύθμισης του χώρου συγκέντρωσης χώρου αποθήκευσης για τον SQL Server με τις ακόλουθες ρυθμίσεις:

- Μέγεθος διαγράμμισης είναι η ρύθμιση παρεμβολή για το εικονικό δίσκο. Για συναλλαγών φόρτους εργασίας αυτό έχει οριστεί σε 64 KB. Για τα δεδομένα αποταμίευσης φόρτους εργασίας η ρύθμιση είναι 256 KB.

- Υποστηρίζεται είναι απλή (δεν υποστηρίζεται).

>[AZURE.NOTE] Azure premium χώρου αποθήκευσης είναι περιττός τοπικά και διατηρεί τρία αντίγραφα των δεδομένων μέσα σε μία μόνο περιοχή, έτσι δεν απαιτείται επιπλέον υποστηρίζεται στο χώρο συγκέντρωσης χώρου αποθήκευσης.

- Πλήθος στηλών ισούται με τον αριθμό των δίσκων στο χώρο συγκέντρωσης χώρου αποθήκευσης.

Για πρόσθετες πληροφορίες σχετικά με το χώρο αποθήκευσης διάστημα και αποθήκευσης χώρους συγκέντρωσης ανατρέξτε στα θέματα:

- [Επισκόπηση του χώρου αποθήκευσης κενά διαστήματα](http://technet.microsoft.com/library/hh831739.aspx).

- [Δημιουργία αντιγράφων ασφαλείας του Windows Server και σύνολα χώρου αποθήκευσης](http://technet.microsoft.com/library/dn390929.aspx)

Για περισσότερες πληροφορίες σχετικά με τις βέλτιστες πρακτικές ρύθμισης παραμέτρων SQL Server, ανατρέξτε στο θέμα [επιδόσεις βέλτιστες πρακτικές για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-performance.md)


###<a name="sql-server-settings"></a>Ρυθμίσεις του SQL Server

Σχετικά με τις **ρυθμίσεις του SQL Server** , διαβάστε και τροποποιήστε το πρόθεμα ονόματος Εικονική SQL Server, έκδοση του SQL Server, λογαριασμός υπηρεσίας SQL Server και τον κωδικό πρόσβασης και SQL Αυτόματη Διόρθωση χρονοδιάγραμμα συντήρησης.

- **SQL Server όνομα πρόθεμα** χρησιμοποιείται για να δημιουργήσετε ένα όνομα για κάθε SQL Server. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **Contoso ag**. Τα ονόματα των SQL Server θα *Contoso-ag-0* και *Contoso-ag-1*.

- **Έκδοση SQL Server** είναι η έκδοση του SQL Server. Για αυτό το πρόγραμμα εκμάθησης χρήση **SQL Server 2014**. Μπορείτε επίσης να επιλέξετε **SQL Server 2012** ή **SQL Server 2016**.

- **Όνομα χρήστη του λογαριασμού υπηρεσίας SQL Server** είναι το όνομα λογαριασμού τομέα για την υπηρεσία SQL Server. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε **sqlservice**.

- **Κωδικός πρόσβασης** είναι ο κωδικός πρόσβασης για το λογαριασμό της υπηρεσίας SQL Server.  Χρησιμοποιήστε μια σύνθετη κωδικό πρόσβασης. Επιβεβαιώστε τον κωδικό πρόσβασης.

- **Ενημέρωση αυτόματης SQL χρονοδιάγραμμα συντήρησης** προσδιορίζει την ημέρα της εβδομάδας που Azure αυτόματα θα κώδικα SQL Servers. Για αυτό το πρόγραμμα εκμάθησης, πληκτρολογήστε **Κυριακή**.

- **SQL Αυτόματη Διόρθωση ώρα έναρξης συντήρησης** είναι την ώρα της ημέρας για την περιοχή Azure όταν θα ξεκινήσει η Αυτόματη διόρθωση.

>[AZURE.NOTE]Το παράθυρο ενημέρωσης κώδικα για κάθε Εικονική είναι ασταθές κατά μία ώρα. Μόνο μία εικονική μηχανή έχει ενημερωθεί κάθε φορά για να αποτρέψετε την διακοπή των υπηρεσιών.

![Ρυθμίσεις του SQL Server](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Ελέγξτε τις ρυθμίσεις και κάντε κλικ στο κουμπί **OK**.

###<a name="summary"></a>Σύνοψη

Στη σελίδα σύνοψης του Azure επικυρώσει τις ρυθμίσεις. Μπορείτε επίσης να κάνετε λήψη του προτύπου. Αναθεωρήστε τη σύνοψη. Κάντε κλικ στο **κουμπί OK**.

###<a name="buy"></a>Αγορά

Αυτό τελικό blade περιλαμβάνει **τους όρους χρήσης**, και την **πολιτική προστασίας προσωπικών δεδομένων**. Διαβάστε αυτές τις πληροφορίες. Όταν είστε έτοιμοι για Azure για να ξεκινήσετε τη δημιουργία του εικονικές μηχανές και όλων των άλλων απαιτείται πόρων για την ομάδα διαθεσιμότητα, κάντε κλικ στην επιλογή **Δημιουργία**.

Πύλη του Azure θα δημιουργήσει την ομάδα των πόρων και όλους τους πόρους.

##<a name="monitor-deployment"></a>Οθόνη ανάπτυξης

Παρακολούθηση της προόδου ανάπτυξης από την πύλη του Azure. Ένα εικονίδιο που αντιπροσωπεύει την ανάπτυξη θα καρφιτσωθεί αυτόματα το Azure πύλης στον πίνακα εργαλείων.

![Azure πίνακα εργαλείων](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

##<a name="connect-to-sql-server"></a>Σύνδεση με τον SQL Server

Εκτελούνται οι νέες παρουσίες του SQL Server σε εικονικές μηχανές που δεν έχουν συνδέσεις στο internet. Ωστόσο, οι ελεγκτές τομέα έχει αντικριστές σύνδεσης στο internet. Για να συνδεθείτε με τους διακομιστές SQL με σύνδεση απομακρυσμένης επιφάνειας εργασίας, πρώτο RDP σε έναν από τους ελεγκτές τομέα. Από τον ελεγκτή τομέα, ανοίξτε μια δεύτερη RDP με τον SQL Server.

Για να RDP στον ελεγκτή πρωτεύοντα τομέα, ακολουθήστε τα παρακάτω βήματα:

1.  Από το Azure πύλης πίνακα εργαλείων πολύ ότι ολοκληρώθηκε με επιτυχία την ανάπτυξη.

1.  Κάντε κλικ στην επιλογή **πόροι**.

1.  Στο το blade **πόρων** , κάντε κλικ στην επιλογή **ad-κύρια-ελεγκτή τομέα** , η οποία είναι το όνομα του υπολογιστή από την εικονική μηχανή για πρωτεύοντα ελεγκτή τομέα.

1.  Στην το blade για **ad-κύρια-ελεγκτή τομέα** , κάντε κλικ στην επιλογή **σύνδεση**. Το πρόγραμμα περιήγησης θα σας ζητήσει να εάν θέλετε να ανοίξετε ή να αποθηκεύσετε το αντικείμενο απομακρυσμένης σύνδεσης. Κάντε κλικ στην επιλογή **Άνοιγμα**.
![Σύνδεση στο κέντρο Δεδομένων](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/13-ad-primary-dc-connect.png)
1.  **Σύνδεση απομακρυσμένης επιφάνειας εργασίας** ενδέχεται να σας ειδοποιήσει ότι δεν μπορούν να προσδιοριστούν τον εκδότη αυτής της απομακρυσμένης σύνδεσης. Κάντε κλικ στην επιλογή **σύνδεση**.

1.  Ασφάλεια των Windows σάς ζητά να εισαγάγετε τα διαπιστευτήριά σας για να συνδεθείτε με τη διεύθυνση IP του πρωτεύοντα ελεγκτή τομέα. Κάντε κλικ στην επιλογή **Χρησιμοποιήστε έναν άλλο λογαριασμό**. Για **το όνομα χρήστη** , πληκτρολογήστε **contoso\DomainAdmin**. Αυτός είναι ο λογαριασμός που επιλέξατε για το όνομα χρήστη του διαχειριστή. Χρησιμοποιήστε τα σύνθετα τον κωδικό πρόσβασης που επιλέξατε όταν ρυθμίσατε τις παραμέτρους του προτύπου.

1.  **Σύνδεση απομακρυσμένης επιφάνειας εργασίας** ενδέχεται να σας ειδοποιήσει ότι ο απομακρυσμένος υπολογιστής ενδέχεται να μην γίνει έλεγχος ταυτότητας οφείλεται σε προβλήματα με το πιστοποιητικό ασφαλείας. Αυτό θα εμφανιστεί το όνομα του πιστοποιητικού ασφαλείας. Στην περίπτωση που ακολουθήσατε το πρόγραμμα εκμάθησης το όνομα θα **ad-κύρια-dc.contoso.com**. Κάντε κλικ στο κουμπί **Ναι**.

Τώρα είστε συνδεδεμένοι στον ελεγκτή πρωτεύοντα τομέα. Για να RDP με τον SQL Server, ακολουθήστε τα παρακάτω βήματα:

1.  Στον ελεγκτή τομέα, ανοίξτε τη **Σύνδεση απομακρυσμένης επιφάνειας εργασίας**.

1.  Για **υπολογιστή**, πληκτρολογήστε το όνομα του έναν από τους διακομιστές SQL. Σε αυτό το πρόγραμμα εκμάθησης, πληκτρολογήστε **0 της**.

1.  Χρησιμοποιήστε το ίδιο λογαριασμό χρήστη και τον κωδικό πρόσβασης που χρησιμοποιήσατε για να RDP στον ελεγκτή τομέα.

Τώρα είστε συνδεδεμένοι με το RDP με τον SQL Server. Για να ανοίξετε SQL Server management studio, σύνδεση με την προεπιλεγμένη παρουσία του SQL Server και επαληθεύστε την ομάδα availabilty έχει ρυθμιστεί.

