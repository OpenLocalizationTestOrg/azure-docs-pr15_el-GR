<properties
    pageTitle="Παροχή του SQL Server εικονικό μηχάνημα | Microsoft Azure"
    description="Δημιουργία και να συνδεθείτε με μια εικονική μηχανή SQL Server στο Azure με την πύλη. Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί τη λειτουργία διαχείρισης πόρων."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Προμήθεια μια εικονική μηχανή SQL Server στην πύλη του Azure

> [AZURE.SELECTOR]
- [Πύλη](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

Αυτό το πρόγραμμα εκμάθησης-τελικών δείχνει πώς μπορείτε να χρησιμοποιήσετε την πύλη του Azure για παροχή μια εικονική μηχανή που εκτελεί τον SQL Server.

Η συλλογή Azure εικονική μηχανή (Εικονική) περιλαμβάνει πολλές εικόνες που περιέχουν Microsoft SQL Server. Με λίγα μόνο κλικ, μπορείτε να επιλέξετε μία από τις εικόνες Εικονική SQL από τη συλλογή και να παράσχετε στο περιβάλλον του Azure.

Σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει:

- [Επιλέξτε μια εικόνα Εικονική SQL από τη συλλογή](#select-a-sql-vm-image-from-the-gallery)
- [Ρύθμιση παραμέτρων και δημιουργήστε την εικονική Μηχανή](#configure-the-vm)
- [Ανοίξτε την εικονική Μηχανή με σύνδεση απομακρυσμένης επιφάνειας εργασίας](#open-the-vm-with-remote-desktop)
- [Απομακρυσμένη σύνδεση με τον SQL Server](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Επιλέξτε μια εικόνα Εικονική SQL από τη συλλογή

1. Συνδεθείτε στην [πύλη του Azure](https://portal.azure.com) χρησιμοποιώντας το λογαριασμό σας.

    >[AZURE.NOTE] Εάν δεν έχετε λογαριασμό Azure, επισκεφθείτε [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

1. Στην πύλη του Azure, κάντε κλικ στην επιλογή **Δημιουργία**. Πύλη του ανοίγει το **νέο** blade. Οι πόροι Εικονική SQL Server είναι στην ομάδα **εικονικές μηχανές** του το Marketplace.

1. Στο blade τη **Δημιουργία** , κάντε κλικ στην επιλογή **εικονικές μηχανές**.

1. Για να δείτε όλες τις διαθέσιμες εικόνες, κάντε κλικ στην επιλογή **δείτε όλα** τα blade **εικονικές μηχανές** .

    ![Azure εικονικές μηχανές Blade](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. Στην περιοχή **διακομιστές βάσεων δεδομένων**, κάντε κλικ στην επιλογή **SQL Server**. Ίσως χρειαστεί να κάνετε κύλιση προς τα κάτω για να εντοπίσετε **τους διακομιστές βάσης δεδομένων**. Αναθεωρήστε τα διαθέσιμα πρότυπα του SQL Server.

    ![Συλλογή εικονικού υπολογιστή SQL εικόνες](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Κάθε πρότυπο προσδιορίζει μια έκδοση του SQL Server και σε λειτουργικό σύστημα. Επιλέξτε μία από αυτές τις εικόνες από τη λίστα. Στη συνέχεια, εξετάστε το blade λεπτομέρειες που παρέχει μια περιγραφή για την εικόνα εικονική μηχανή.

    >[AZURE.NOTE] Εικονική SQL εικόνες περιλαμβάνουν τα έξοδα αδειών χρήσης για τον SQL Server σε την τιμολόγηση ανά λεπτό από την εικονική Μηχανή που δημιουργείτε. Υπάρχει μια άλλη επιλογή για να εμφανιστεί η-κάτοχος-αδειών χρήσης (BYOL) και πληρωμή μόνο για την εικονική Μηχανή. Αυτά τα ονόματα εικόνων είναι το πρόθεμα {BYOL}. Για περισσότερες πληροφορίες σχετικά με αυτήν την επιλογή, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-server-iaas-overview.md).

1. Στην περιοχή **Επιλέξτε ένα μοντέλο ανάπτυξης**, επιβεβαιώστε ότι είναι ενεργοποιημένη η **Διαχείριση πόρων** . Διαχείριση πόρων είναι το μοντέλο προτεινόμενα ανάπτυξης για νέα εικονικές μηχανές. Κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία Εικονική SQL με τη διαχείριση πόρων](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>Ρύθμιση παραμέτρων την εικονική Μηχανή
Υπάρχουν πέντε λεπίδες για τη ρύθμιση παραμέτρων μια εικονική μηχανή SQL Server.

| Βήμα               | Περιγραφή                          |
|---------------------|-------------------------------|
| **Βασικά στοιχεία**              | [Ρύθμιση παραμέτρων βασικές](#1-configure-basic-settings)      |
| **Μέγεθος**                | [Επιλογή μεγέθους εικονική μηχανή](#2-choose-virtual-machine-size)   |
| **Ρυθμίσεις**            | [Ρύθμιση παραμέτρων προαιρετικές δυνατότητες](#3-configure-optional-features)   |
| **Ρυθμίσεις του SQL Server** | [Ρύθμιση παραμέτρων του SQL server](#4-configure-sql-server-settings) |
| **Σύνοψη**             | [Αναθεώρηση της σύνοψης](#5-review-the-summary)            |

## <a name="1-configure-basic-settings"></a>1. ρύθμιση παραμέτρων βασικές
Στη blade τα **βασικά στοιχεία** , δώστε τις ακόλουθες πληροφορίες:

* Εισαγάγετε μια εικονική μηχανή μοναδικό **όνομα**.
* Καθορίστε ένα **όνομα χρήστη** για το λογαριασμό του τοπικού διαχειριστή στην η Εικονική. Αυτός ο λογαριασμός προστίθεται επίσης **σταθερό διακομιστή sysadmin SQL Server** .
* Δώστε έναν ισχυρό **κωδικό πρόσβασης**.
* Εάν έχετε πολλές συνδρομές, βεβαιωθείτε ότι η συνδρομή είναι σωστές για το νέο Εικονική.
* Στο πλαίσιο " **ομάδα πόρων** ", πληκτρολογήστε ένα όνομα για μια νέα ομάδα πόρων. Εναλλακτικά, για να χρησιμοποιήσετε έναν υπάρχοντα πόρο ομάδα κάντε κλικ στην επιλογή **Επιλέξτε υπάρχουσα**. Μια ομάδα πόρων είναι μια συλλογή από Σχετικοί πόροι στο Azure (εικονικές μηχανές, λογαριασμούς χώρου αποθήκευσης, εικονικό δίκτυα, κ.λπ.).

    >[AZURE.NOTE] Χρήση νέας ομάδας πόρων είναι χρήσιμο εάν απλώς δοκιμές ή εκμάθησης σχετικά με τις αναπτύξεις SQL Server στο Azure. Αφού ολοκληρώσετε με δοκιμή σας, διαγράψτε την ομάδα των πόρων για να διαγράψετε αυτόματα η Εικονική και όλους τους πόρους που σχετίζονται με αυτήν την ομάδα πόρων. Για περισσότερες πληροφορίες σχετικά με τις ομάδες πόρων, ανατρέξτε στο θέμα [Επισκόπηση της διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md).

* Επιλέξτε μια **θέση** για αυτήν την ανάπτυξη.
* Κάντε κλικ στο **κουμπί OK** για να αποθηκεύσετε τις ρυθμίσεις.

    ![Blade βασικά στοιχεία SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. επιλογή μεγέθους εικονική μηχανή
Στο βήμα **μέγεθος** , επιλέξτε ένα μέγεθος εικονική μηχανή στο το blade **Επιλέξτε ένα μέγεθος** . Το blade αρχικά εμφανίζει μεγέθη προτεινόμενα υπολογιστή με βάση το πρότυπο που έχετε επιλέξει. Το υπολογίζει επίσης το μηνιαίο κόστος για να εκτελέσετε την εικονική Μηχανή.

![Επιλογές μεγέθους Εικονική SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Για φόρτους εργασίας παραγωγής, συνιστάται η επιλογή ένα μέγεθος εικονικό μηχάνημα που υποστηρίζει το [Χώρο αποθήκευσης Premium](../storage/storage-premium-storage.md). Εάν δεν απαιτούν αυτό το επίπεδο επιδόσεων, χρησιμοποιήστε το κουμπί **Προβολή όλων** , το οποίο εμφανίζει όλες τις επιλογές μεγέθους υπολογιστή. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε ένα μικρότερο μέγεθος υπολογιστή για ανάπτυξη ή περιβάλλον δοκιμής.

>[AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με την εικονική μηχανή μεγέθη ανατρέξτε στο θέμα [μεγέθη για εικονικές μηχανές](virtual-machines-windows-sizes.md). Για ζητήματα σχετικά με τα μεγέθη Εικονική SQL Server, ανατρέξτε στο θέμα [επιδόσεις βέλτιστες πρακτικές για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-performance.md).

Επιλέξτε το μέγεθος του υπολογιστή σας και, στη συνέχεια, κάντε κλικ στην **επιλογή**.

## <a name="3-configure-optional-features"></a>3. ρύθμιση παραμέτρων προαιρετικές δυνατότητες
Στον το blade **Ρυθμίσεις** , ρυθμίστε το Azure χώρο αποθήκευσης, δικτύου, και την παρακολούθηση για την εικονική μηχανή.

- Στην περιοχή **χώρου αποθήκευσης**, καθορίστε ένα **δίσκο πληκτρολογήστε** τυπική ή Premium (SSD). Χώρος αποθήκευσης Premium προτείνεται για παραγωγής φόρτους εργασίας.

>[AZURE.NOTE] Εάν επιλέξετε Premium (SSD) για το μέγεθος του υπολογιστή που δεν υποστηρίζει αποθήκευσης Premium, το μέγεθος του υπολογιστή σας αλλάζει αυτόματα.  

- Στην περιοχή **λογαριασμός χώρου αποθήκευσης**, μπορείτε να αποδεχθείτε το όνομα λογαριασμού του χώρου αποθήκευσης αυτόματα προμήθεια του φακέλου. Μπορείτε επίσης να κάνετε κλικ στο **λογαριασμό του χώρου αποθήκευσης** για να επιλέξετε έναν υπάρχοντα λογαριασμό και να ρυθμίσετε τον τύπο λογαριασμού χώρου αποθήκευσης. Από προεπιλογή, Azure δημιουργεί ένα νέο λογαριασμό του χώρου αποθήκευσης με τοπικά πλεονάζοντα χώρο αποθήκευσης. Για περισσότερες πληροφορίες σχετικά με τις επιλογές αποθήκευσης, ανατρέξτε στο θέμα [αποθήκευσης Azure αναπαραγωγής](../storage/storage-redundancy.md).

- Στην περιοχή **δικτύου**, μπορείτε να αποδεχτείτε την αυτόματη συμπλήρωση τιμών. Μπορείτε επίσης να κάνετε κλικ σε κάθε δυνατότητα μη αυτόματης ρύθμισης παραμέτρων του **δικτύου εικονικές**, **υποδίκτυο**, **δημόσια διεύθυνση IP**και **Ομάδα ασφαλείας δικτύου**. Για τους σκοπούς αυτής της εκμάθησης, διατηρήστε τις προεπιλεγμένες τιμές.

- Azure επιτρέπει **παρακολούθησης** από προεπιλογή με τον ίδιο λογαριασμό χώρου αποθήκευσης που έχει καθοριστεί για την εικονική Μηχανή. Μπορείτε να αλλάξετε αυτές τις ρυθμίσεις εδώ.

- Στην περιοχή **Ορισμός διαθεσιμότητας**, καθορίστε ένα σύνολο διαθεσιμότητα. Για τους σκοπούς αυτής της εκμάθησης, μπορείτε να επιλέξετε **καμία**. Εάν σκοπεύετε να ρυθμίσετε ομάδες διαθεσιμότητας AlwaysOn SQL, ρύθμιση παραμέτρων της διαθεσιμότητας για να αποφύγετε να δημιουργείτε την εικονική μηχανή.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση τη διαθεσιμότητα των εικονικές μηχανές](virtual-machines-windows-manage-availability.md).

Όταν ολοκληρώσετε τη ρύθμιση των παραμέτρων αυτές τις ρυθμίσεις, κάντε κλικ στο **κουμπί OK**.

## <a name="4-configure-sql-server-settings"></a>4. ρύθμιση παραμέτρων SQL server
Στην το blade **ρυθμίσεις του SQL Server** , ρυθμίστε τις παραμέτρους συγκεκριμένες ρυθμίσεις και βελτιστοποιήσεις για τον SQL Server. Οι ρυθμίσεις που μπορείτε να ρυθμίσετε τις παραμέτρους για τον SQL Server περιλαμβάνουν τα εξής.

| Ρύθμιση               |
|---------------------|
| [Συνδεσιμότητα](#connectivity)              |
| [Έλεγχος ταυτότητας](#authentication)                |
| [Ρύθμιση παραμέτρων αποθήκευσης](#storage-configuration)            |
| [Αυτόματη Διόρθωση](#automated-patching) |
| [Αυτόματη δημιουργία αντιγράφων ασφαλείας](#automated-backup)             |
| [Ενοποίηση του Azure θάλαμο κλειδιού](#azure-key-vault-integration)             |
| [Υπηρεσίες R](#r-services) |

### <a name="connectivity"></a>Συνδεσιμότητα
Στην περιοχή **συνδεσιμότητα SQL**, καθορίστε τον τύπο πρόσβασης που θέλετε να την παρουσία του SQL Server σε αυτό Εικονική. Για τους σκοπούς αυτής της εκμάθησης, επιλέξτε **δημόσια (internet)** να επιτρέπουν συνδέσεις με τον SQL Server μηχανές ή των υπηρεσιών στο internet. Με αυτήν την επιλογή, Azure ρυθμίζει αυτόματα το τείχος προστασίας και την ομάδα ασφαλείας δικτύου για να επιτρέψετε την κυκλοφορία στη θύρα 1433.  

![Επιλογές συνδεσιμότητας SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Για να συνδεθείτε με τον SQL Server μέσω του internet, πρέπει να ενεργοποιείτε επίσης έλεγχο ταυτότητας διακομιστή SQL, η οποία περιγράφεται στην επόμενη ενότητα.

>[AZURE.NOTE] Είναι δυνατή η προσθήκη επιπλέον περιορισμούς για τις επικοινωνίες δικτύου σας Εικονική SQL Server. Μπορείτε να το κάνετε με την επεξεργασία της ομάδας ασφαλείας δικτύου, αφού δημιουργηθεί η Εικονική. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι μια ομάδα ασφαλείας δικτύου (NSG);](../virtual-network/virtual-networks-nsg.md)

Εάν προτιμάτε να μην ενεργοποιήσετε τις συνδέσεις για να το μηχανισμό βάσεων δεδομένων μέσω του internet, ορίστε μία από τις ακόλουθες επιλογές:

- **Τοπική (μέσα μόνο Εικονική)** να επιτρέπουν συνδέσεις με τον SQL Server μόνο από μέσα του Εικονική.
- **Ιδιωτική (εντός εικονικό δίκτυο)** να επιτρέπουν συνδέσεις με τον SQL Server μηχανές ή των υπηρεσιών στο ίδιο δίκτυο εικονικού.

>[AZURE.NOTE] Η εικόνα εικονική μηχανή για SQL Server Express edition δεν ενεργοποιεί αυτόματα το πρωτόκολλο TCP/IP. Αυτό ισχύει ακόμη και για τις επιλογές συνδεσιμότητας δημόσια και ιδιωτικά. Για Express edition, πρέπει να χρησιμοποιήσετε SQL Server Configuration Manager για να [ενεργοποιήσετε με μη αυτόματο τρόπο το πρωτόκολλο TCP/IP](#configure-sql-server-to-listen-on-the-tcp-protocol) μετά τη δημιουργία του Εικονική.

Σε γενικές γραμμές, μπορείτε να βελτιώσετε την ασφάλεια, επιλέγοντας τη συνδεσιμότητα πιο περιοριστική που επιτρέπει την δική σας περίπτωση. Αλλά όλες οι επιλογές είναι δυνατότητα ασφάλισης μέσω κανόνων ομάδα ασφαλείας δικτύου και τον έλεγχο ταυτότητας των Windows SQL.

Προεπιλεγμένη **θύρα** 1433. Μπορείτε να καθορίσετε διαφορετικό αριθμό θύρας.
Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα σύνδεση σε SQL Server εικονικό μηχάνημα (Διαχείριση πόρων) [| Microsoft Azure](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Έλεγχος ταυτότητας
Εάν απαιτείται έλεγχος ταυτότητας του SQL Server, κάντε κλικ στην επιλογή **Ενεργοποίηση** στην περιοχή **Έλεγχος ταυτότητας SQL**.

![Έλεγχος ταυτότητας του SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] Εάν σχεδιάζετε να αποκτήσετε πρόσβαση σε SQL Server μέσω του internet (π.χ., η επιλογή συνδεσιμότητα με δημόσια δίκτυα), πρέπει να ενεργοποιήσετε τον έλεγχο ταυτότητας SQL εδώ. Δημόσια πρόσβαση στον SQL Server απαιτεί τη χρήση του ελέγχου ταυτότητας SQL.

Εάν ενεργοποιήσετε τον έλεγχο ταυτότητας του SQL Server, καθορίστε ένα **όνομα σύνδεσης** και **τον κωδικό πρόσβασης**. Αυτό το όνομα χρήστη έχει ρυθμιστεί ως είσοδο με τον έλεγχο ταυτότητας του SQL Server και μέλος **sysadmin** σταθερής ρόλος διακομιστή. Για περισσότερες πληροφορίες σχετικά με τις λειτουργίες ελέγχου ταυτότητας, ανατρέξτε στο θέμα [επιλογή μια λειτουργία ελέγχου ταυτότητας](http://msdn.microsoft.com/library/ms144284.aspx) .

Εάν δεν ενεργοποιήσετε τον έλεγχο ταυτότητας του SQL Server, στη συνέχεια, μπορείτε να χρησιμοποιήσετε λογαριασμού του τοπικού διαχειριστή στην η Εικονική για να συνδεθείτε με την παρουσία του SQL Server.

### <a name="storage-configuration"></a>Ρύθμιση παραμέτρων αποθήκευσης
Κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων αποθήκευσης** για να καθορίσετε τις απαιτήσεις χώρου αποθήκευσης.

![Ρύθμιση παραμέτρων του χώρου αποθήκευσης SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Εάν επιλέξετε τυπική χώρου αποθήκευσης, αυτή η επιλογή δεν είναι διαθέσιμη. Βελτιστοποίηση αυτόματης αποθήκευσης είναι διαθέσιμη μόνο για το χώρο αποθήκευσης Premium.

Μπορείτε να καθορίσετε απαιτήσεις ως λειτουργίες εισαγωγής/εξαγωγής ανά δευτερόλεπτο (IOP), μετάδοση σε MB/s και το συνολικό χώρο αποθήκευσης μέγεθος. Ρύθμιση παραμέτρων αυτές τις τιμές, χρησιμοποιώντας τις κλίμακες ολισθαίνουσες. Η πύλη υπολογίζει αυτόματα τον αριθμό των δίσκων που βασίζονται σε αυτές τις απαιτήσεις.

Από προεπιλογή, το Azure βελτιστοποιεί το χώρο αποθήκευσης για 5000 IOP, 200 MB και 1 TB χώρου αποθήκευσης. Μπορείτε να αλλάξετε αυτές τις ρυθμίσεις αποθήκευσης που βασίζεται σε φόρτο εργασίας. Στην περιοχή **αποθήκευσης βελτιστοποιημένη για**, επιλέξτε μία από τις ακόλουθες επιλογές:

- **Γενικά** είναι η προεπιλεγμένη ρύθμιση και υποστηρίζει περισσότερους φόρτους εργασίας.
- Επεξεργασία **συναλλαγών** βελτιστοποιεί τον χώρο αποθήκευσης για φόρτους εργασίας Συναλλαγής παραδοσιακή βάση δεδομένων.
- **Αποθήκευση** βελτιστοποιεί τον χώρο αποθήκευσης για ανάλυσης και αναφοράς φόρτους εργασίας.

>[AZURE.NOTE] Τα όρια επάνω στην τα ρυθμιστικά ποικίλλουν ανάλογα με το μέγεθος του επιλεγμένου εικονική μηχανή σας.

### <a name="automated-patching"></a>Αυτόματη Διόρθωση
**Αυτόματη Διόρθωση** είναι ενεργοποιημένη από προεπιλογή. Αυτόματη ενημέρωση επιτρέπει Azure για αυτόματη ενημέρωση κώδικα SQL Server και το λειτουργικό σύστημα. Καθορίστε την ημέρα της εβδομάδας, ώρα, και την διάρκεια για ένα παράθυρο Συντήρηση. Azure εκτελεί ενημέρωση σε αυτό το παράθυρο Συντήρηση. Το χρονοδιάγραμμα παράθυρο Συντήρηση χρησιμοποιεί τις τοπικές ρυθμίσεις Εικονική για χρόνο. Εάν δεν θέλετε Azure για αυτόματη ενημέρωση κώδικα SQL Server και το λειτουργικό σύστημα, κάντε κλικ στην επιλογή **Απενεργοποίηση**.  

![SQL αυτοματοποιημένη ενημέρωση κώδικα](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αυτόματη ενημέρωση κώδικα για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Αυτόματη δημιουργία αντιγράφων ασφαλείας
Ενεργοποίηση αυτόματων βάσης δεδομένων αντιγράφων ασφαλείας για όλες τις βάσεις δεδομένων στην περιοχή **αυτόματης δημιουργίας αντιγράφων ασφαλείας**. Αυτόματη δημιουργία αντιγράφων ασφαλείας είναι απενεργοποιημένη από προεπιλογή.

Όταν ενεργοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας SQL, μπορείτε να ρυθμίσετε τα εξής:

- Περίοδος διατήρησης (ημέρες) για δημιουργία αντιγράφων ασφαλείας
- Λογαριασμός χώρου αποθήκευσης για δημιουργία αντιγράφων ασφαλείας
- Επιλογή κρυπτογράφηση και τον κωδικό πρόσβασης για δημιουργία αντιγράφων ασφαλείας

Για να κρυπτογραφήσετε το αντίγραφο ασφαλείας, κάντε κλικ στην επιλογή **Ενεργοποίηση**. Στη συνέχεια, καθορίστε το **τον κωδικό πρόσβασης**. Azure δημιουργεί ένα πιστοποιητικό για να κρυπτογραφήσετε τη δημιουργία αντιγράφων ασφαλείας και χρησιμοποιεί τον καθορισμένο κωδικό πρόσβασης για να προστατεύσετε αυτό το πιστοποιητικό.

![SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αυτόματης δημιουργίας αντιγράφων ασφαλείας για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Ενοποίηση του Azure θάλαμο αριθμού-κλειδιού
Για να αποθηκεύσετε απόρρητο ασφαλείας στο Azure κρυπτογράφησης, κάντε κλικ στην επιλογή **Azure κλειδιού θάλαμο ενοποίηση** και επιλέξτε **Ενεργοποίηση**.

![Ενοποίηση του SQL Azure θάλαμο κλειδιού](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

Ο παρακάτω πίνακας παραθέτει τις παραμέτρους που απαιτούνται για τη ρύθμιση παραμέτρων ενοποίησης θάλαμο Azure αριθμού-κλειδιού.

|ΠΑΡΆΜΕΤΡΟΣ|ΠΕΡΙΓΡΑΦΉ|ΠΑΡΆΔΕΙΓΜΑ|
|----------|----------|-------|
|**Διεύθυνση URL θάλαμο κλειδιού** |Η θέση του κλειδιού θάλαμο.|https://contosokeyvault.vault.Azure.NET/ |
|**Κύριο όνομα** |Azure Active Directory κύριο όνομα υπηρεσίας. Αυτό το όνομα είναι επίσης γνωστή ως το αναγνωριστικό υπολογιστή-πελάτη.  |fde2b411 - 33 δ 5-4e11-af04eb07b669ccf2|
| **Μυστικό κεφαλαίου**|Μυστικό κεφαλαίου την Azure Active Directory υπηρεσίας. Σε αυτό το μυστικό είναι γνωστή και ως το μυστικό προγράμματος-πελάτη. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =|
|**Όνομα διαπιστευτηρίων**|**Όνομα διαπιστευτηρίων**: ενοποίηση AKV δημιουργεί μια πιστοποίηση μέσα σε SQL Server, επιτρέποντας την εικονική Μηχανή να έχουν πρόσβαση σε του κλειδιού θάλαμο. Επιλέξτε ένα όνομα για αυτά τα διαπιστευτήρια.| mycred1|

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων Azure κλειδί θάλαμο ενοποίησης για τον SQL Server σε VM Azure](virtual-machines-windows-ps-sql-keyvault.md).

Όταν ολοκληρώσετε τη ρύθμιση παραμέτρων του SQL Server, κάντε κλικ στο **κουμπί OK**.

### <a name="r-services"></a>Υπηρεσίες R
Για το SQL Server 2016 Enterprise edition, έχετε την επιλογή για να ενεργοποιήσετε την [Υπηρεσιών R του SQL Server](https://msdn.microsoft.com/library/mt604845.aspx). Αυτό σας επιτρέπει να χρησιμοποιήσετε προηγμένη ανάλυση με το SQL Server 2016. Κάντε κλικ στην επιλογή **Ενεργοποίηση** στην blade του **SQL Server Settings** .

![Ενεργοποίηση των υπηρεσιών SQL Server R](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

>[AZURE.NOTE] Για εικόνες SQL Server που δεν είναι έκδοση Enterprise 2016, την επιλογή για να ενεργοποιήσετε τις υπηρεσίες R είναι απενεργοποιημένη.

## <a name="5-review-the-summary"></a>5. Αναθεωρήστε τη σύνοψη
Στη **Σύνοψη** του blade, αναθεωρήστε τη σύνοψη και κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε SQL Server, ομάδα πόρων και τους πόρους που έχουν καθοριστεί για αυτό Εικονική.

Μπορείτε να παρακολουθείτε την ανάπτυξη από την πύλη του azure. Το κουμπί " **ειδοποιήσεις** " στο επάνω μέρος της οθόνης εμφανίζει βασικές την κατάσταση της ανάπτυξης.

>[AZURE.NOTE] Να σας παρέχει μια γενική χρόνους ανάπτυξης, να αναπτυχθεί μια Εικονική SQL στην περιοχή Ανατολικής η.π.α. με τις προεπιλεγμένες ρυθμίσεις. Αυτή η ανάπτυξη δοκιμής εκτελέσατε συνολικά 26 λεπτά για να ολοκληρωθεί. Αλλά που ενδέχεται να συναντήσετε έναν ταχύτερο ή πιο αργό χρόνο ανάπτυξης ενεργοποιήσει ρυθμίσεις και με βάση την περιοχή σας.

## <a name="open-the-vm-with-remote-desktop"></a>Ανοίξτε την εικονική Μηχανή με σύνδεση απομακρυσμένης επιφάνειας εργασίας

Χρησιμοποιήστε τα ακόλουθα βήματα για να συνδεθείτε με την εικονική μηχανή με σύνδεση απομακρυσμένης επιφάνειας εργασίας:

1. Αφού δημιουργηθεί η Εικονική Azure, το εικονίδιο για την εικονική Μηχανή εμφανίζεται στον πίνακα εργαλείων σας Azure. Μπορείτε επίσης να το βρείτε αναζητώντας το υπάρχον εικονικές μηχανές. Κάντε κλικ στον υπολογιστή σας νέα εικονική SQL. Μια **εικονική μηχανή** blade εμφανίζει τις λεπτομέρειες του εικονική μηχανή σας.
1. Στο επάνω μέρος του blade **εικονική μηχανή** , κάντε κλικ στην επιλογή **σύνδεση**.
1. Το πρόγραμμα περιήγησης στοιχεία λήψης ένα αρχείο RDP για την εικονική Μηχανή. Ανοίξτε το αρχείο RDP.
    ![Σύνδεση απομακρυσμένης επιφάνειας εργασίας για να Εικονική SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. Η σύνδεση απομακρυσμένης επιφάνειας εργασίας σάς ειδοποιεί ότι δεν μπορούν να προσδιοριστούν τον εκδότη αυτής της απομακρυσμένης σύνδεσης. Κάντε κλικ στην επιλογή **σύνδεση** για να συνεχίσετε.
1. Στο παράθυρο διαλόγου **Ασφαλείας των Windows** , κάντε κλικ στην επιλογή **Χρησιμοποιήστε έναν άλλο λογαριασμό**.
1. Για το **όνομα χρήστη** τύπος ** \<όνομα χρήστη >**, όπου <user name> είναι το όνομα χρήστη που καθορίσατε όταν έχετε ρυθμίσει τις παραμέτρους του Εικονική. Πρέπει να προσθέσετε ένα αρχικό ανάστροφη κάθετο πριν από το όνομα.
1. Πληκτρολογήστε **τον κωδικό πρόσβασης** που ορίσατε προηγουμένως για αυτό Εικονική και, στη συνέχεια, κάντε κλικ στο **κουμπί OK** για να συνδεθείτε.
1. Εάν ένα άλλο παράθυρο διαλόγου **Σύνδεση απομακρυσμένης επιφάνειας εργασίας** σάς ρωτήσει αν θέλετε να συνδεθείτε, κάντε κλικ στο κουμπί **Ναι**.

Αφού συνδεθείτε στον υπολογιστή εικονικές SQL Server, μπορείτε να εκκίνηση SQL Server Management Studio και να συνδεθείτε με έλεγχο ταυτότητας των Windows, χρησιμοποιώντας τα διαπιστευτήρια του τοπικού διαχειριστή. Εάν έχετε ενεργοποιήσει τον έλεγχο ταυτότητας του SQL Server, μπορείτε επίσης να συνδεθείτε με τον έλεγχο ταυτότητας SQL χρησιμοποιώντας τη σύνδεση SQL και τον κωδικό πρόσβασης που έχετε ρυθμίσει τις παραμέτρους κατά την προμήθεια του.

Πρόσβαση στον υπολογιστή σας επιτρέπει να αλλάξετε απευθείας υπολογιστή και ρυθμίσεις SQL Server με βάση τις απαιτήσεις σας. Για παράδειγμα, που θα μπορούσε να ρυθμίσετε τις παραμέτρους του τείχους προστασίας ή να αλλάξετε τις ρυθμίσεις παραμέτρων του SQL Server.

## <a name="connect-to-sql-server-remotely"></a>Απομακρυσμένη σύνδεση με τον SQL Server

Σε αυτό το πρόγραμμα εκμάθησης, θα σας επιλεγμένο **δημόσιας** πρόσβασης για το εικονικό μηχάνημα και **Έλεγχος ταυτότητας του SQL Server**. Αυτές οι ρυθμίσεις ρυθμιστεί αυτόματα η εικονική μηχανή να επιτρέπουν συνδέσεις SQL Server από οποιοδήποτε πρόγραμμα-πελάτη μέσω του internet (υπό την προϋπόθεση ότι έχουν τη σωστή σύνδεση SQL).

>[AZURE.NOTE] Εάν δεν επιλέξατε δημόσια κατά την προμήθεια, απαιτούνται επιπλέον βήματα για να αποκτήσετε πρόσβαση σε παρουσία του SQL Server σας μέσω του internet. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [σύνδεση σε έναν υπολογιστή εικονικές του SQL Server](virtual-machines-windows-sql-connect.md).

Οι ενότητες που ακολουθούν δείχνουν πώς μπορείτε να συνδεθείτε με την παρουσία του SQL Server στην Εικονική σας από έναν άλλο υπολογιστή μέσω του internet.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Επόμενα βήματα
Για άλλες πληροφορίες σχετικά με τη χρήση SQL Server Azure, ανατρέξτε στο θέμα [SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-server-iaas-overview.md) και τις [Συνήθεις ερωτήσεις](virtual-machines-windows-sql-server-iaas-faq.md).

Για μια επισκόπηση του βίντεο του SQL Server σε εικονικές μηχανές Windows Azure, παρακολουθήστε [Εικονική Azure είναι η καλύτερη πλατφόρμα για SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

[Εξερεύνηση της διαδρομής εκμάθησης](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) για τον SQL Server σε εικονικές μηχανές οι Azure.