<properties
    pageTitle="Χρήση του χώρου αποθήκευσης Azure Premium με τον SQL Server | Microsoft Azure"
    description="Σε αυτό το άρθρο χρησιμοποιεί τους πόρους που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης και παρέχει οδηγίες για τη χρήση του χώρου αποθήκευσης Premium Azure με SQL Server που εκτελείται σε εικονικές μηχανές Windows Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="jhubbard"
    editor="monicar"    
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth"/>

# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Χρήση του χώρου αποθήκευσης Azure Premium με τον SQL Server σε εικονικές μηχανές


## <a name="overview"></a>Επισκόπηση

[Ο χώρος αποθήκευσης Premium Azure](../storage/storage-premium-storage.md) είναι η επόμενη γενιά χώρου αποθήκευσης που παρέχει χαμηλής λανθάνων χρόνος και υψηλή απόδοση εισόδου/ΕΞΌΔΟΥ. Λειτουργεί καλύτερα με βασικές εισόδου/ΕΞΌΔΟΥ εντατική φόρτους εργασίας, όπως ο SQL Server σε [εικονικές μηχανές](https://azure.microsoft.com/services/virtual-machines/)τους IaaS.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Σε αυτό το άρθρο παρέχει οδηγίες για τη μετεγκατάσταση μια εικονική μηχανή που εκτελεί τον SQL Server για να χρησιμοποιήσετε Premium χώρου αποθήκευσης και σχεδιασμού. Αυτό περιλαμβάνει Azure υποδομή (κοινωνικής δικτύωσης, χώρος αποθήκευσης) και βήματα Εικονική Windows επισκέπτη. Το παράδειγμα στο [προσάρτημα](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) εμφανίζει μια πλήρη ολοκληρωμένη μετεγκατάσταση τελικών πώς μπορείτε να μετακινήσετε μεγαλύτερο ΣΠΣ για να εκμεταλλευτείτε τις βελτιωμένες τοπικού χώρου αποθήκευσης SSD με το PowerShell.

Είναι σημαντικό να κατανοήσετε τη διαδικασία για να ολοκληρωμένες με χρήση αποθήκευσης Premium Azure με τον SQL Server σε VM IAAS. Αυτό περιλαμβάνει:

- Προσδιορισμός των τις προϋποθέσεις για να χρησιμοποιήσετε Premium χώρου αποθήκευσης.
- Παραδείγματα ανάπτυξης του SQL Server σε IaaS με το χώρο αποθήκευσης Premium για νέες αναπτύξεις.
- Παραδείγματα μετεγκατάσταση υπάρχοντος αναπτύξεις, μεμονωμένους διακομιστές και αναπτύξεις χρήση SQL πάντα στη διαθεσιμότητα ομάδων.
- Μετεγκατάσταση πιθανές προσεγγίσεις.
- Παράδειγμα πλήρη για ολοκληρωμένες που εμφανίζει τα βήματα Azure, τα Windows και SQL Server για τη μετεγκατάσταση του υπάρχοντος πάντα στην εφαρμογή.

Για περισσότερες πληροφορίες φόντου στον SQL Server σε εικονικές μηχανές Windows Azure, ανατρέξτε στο θέμα [SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-server-iaas-overview.md).

**Συντάκτης:** Χρήστος εδάφους **τεχνική αναθεωρητές:** ο Λευτέρης Νάνσυ Vargas ρέγγα, Κώστας Mishra, Pravin Mital, Juergen Παπαδόπουλος, Φωτίου Gonzalo.

## <a name="prerequisites-for-premium-storage"></a>Προαπαιτούμενα στοιχεία για το χώρο αποθήκευσης Premium

Υπάρχουν διάφορες προϋποθέσεις για τη χρήση του χώρου αποθήκευσης Premium.

### <a name="machine-size"></a>Μέγεθος του υπολογιστή

Για τη χρήση του χώρου αποθήκευσης Premium θα πρέπει να χρησιμοποιήσετε σειράς DS εικονικές μηχανές (Εικονική). Εάν δεν έχετε χρησιμοποιήσει σειράς DS μηχανές στην υπηρεσία cloud πριν, που πρέπει να διαγράψετε την υπάρχουσα εικονική Μηχανή διατήρηση των συνημμένων δίσκων και, στη συνέχεια, δημιουργήστε μια νέα υπηρεσία στο cloud πριν να δημιουργείτε την εικονική Μηχανή ως μέγεθος ρόλο DS *. Για περισσότερες πληροφορίες σχετικά με τα μεγέθη εικονική μηχανή, ανατρέξτε στο θέμα [εικονική μηχανή και τα μεγέθη υπηρεσία Cloud για Azure](virtual-machines-linux-sizes.md).

### <a name="cloud-services"></a>Υπηρεσίες cloud

Μπορείτε να χρησιμοποιήσετε DS * ΣΠΣ με αποθήκευσης Premium, μόνο όταν δημιουργούνται σε μια νέα υπηρεσία στο cloud. Εάν χρησιμοποιείτε SQL Server πάντα σε στο Azure, τα πάντα σε ακρόασης θα αναφέρονται στη διεύθυνση Azure εσωτερική ή εξωτερική IP εξισορρόπησης φόρτου που σχετίζεται με μια υπηρεσία στο cloud. Σε αυτό το άρθρο εστιάζει σχετικά με τη μετεγκατάσταση διατηρώντας διαθεσιμότητα σε αυτό το σενάριο.

> [AZURE.NOTE] Μια σειρά DS * πρέπει να είναι η πρώτη Εικονική που έχει αναπτυχθεί για τη νέα υπηρεσία Cloud.

### <a name="regional-vnets"></a>Τοπικές VNETS

Για DS * ΣΠΣ πρέπει να ρυθμίσετε το εικονικό δίκτυο (VNET) φιλοξενίας σας ΣΠΣ να είναι τοπικές ρυθμίσεις. Αυτό "διευρύνει" του VNET είναι για να επιτρέψετε τη μεγαλύτερη ΣΠΣ να είναι παρασχεθεί στο άλλων συμπλεγμάτων και να επιτρέπεται η επικοινωνία μεταξύ τους. Στο παρακάτω στιγμιότυπο οθόνης, επισημασμένη θέση εμφανίζει τοπικές VNETs, ενώ το πρώτο αποτέλεσμα εμφανίζει μια VNET "στενή".

![RegionalVNET][1]

Μπορείτε να αυξήσετε ένα δελτίο υποστήριξης της Microsoft για τη μετεγκατάσταση στο ένα τοπικές VNET, Microsoft θα κάνετε μια αλλαγή, στη συνέχεια, για να ολοκληρωθεί η μετεγκατάσταση για τοπικές VNETs, αλλάξτε την ιδιότητα AffinityGroup στις παραμέτρους δικτύου. Εξαγάγετε πρώτα τη ρύθμιση παραμέτρων δικτύου στο PowerShell και, στη συνέχεια, αντικαταστήστε την ιδιότητα **AffinityGroup** στο στοιχείο **VirtualNetworkSite** με μια ιδιότητα **θέση** . Καθορίστε `Location = XXXX` όπου `XXXX` είναι μια περιοχή Azure. Στη συνέχεια, εισαγάγετε τη νέα ρύθμιση παραμέτρων.

Για παράδειγμα, εξετάζοντας τις ακόλουθες ρυθμίσεις παραμέτρων VNET:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Για να μετακινήσετε μια τοπικές VNET στην Ευρώπη Δυτική αυτό, αλλάξτε τη ρύθμιση παραμέτρων με το εξής:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Λογαριασμοί χώρου αποθήκευσης

Θα πρέπει να δημιουργήσετε έναν νέο λογαριασμό του χώρου αποθήκευσης που έχει ρυθμιστεί για Premium χώρου αποθήκευσης. Σημειώστε ότι τη χρήση του χώρου αποθήκευσης Premium έχει ρυθμιστεί το λογαριασμό χώρου αποθήκευσης, όχι σε μεμονωμένα VHD, ωστόσο, όταν χρησιμοποιείτε μια Εικονική σειράς DS * μπορείτε να επισυνάψετε της VHD από λογαριασμούς Premium και τυπική χώρου αποθήκευσης. Μπορείτε να αυτό εάν δεν θέλετε να τοποθετήσετε το λειτουργικό σύστημα VHD με το λογαριασμό χώρου αποθήκευσης Premium.

Την ακόλουθη εντολή **Δημιουργία AzureStorageAccountPowerShell** με το "Premium_LRS" **Τύπος** δημιουργεί ένα άρτιο χώρου αποθήκευσης:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Ρυθμίσεις του Cache VHD

Η βασική διαφορά μεταξύ δημιουργία δίσκων που αποτελούν μέρος ενός λογαριασμού Premium χώρου αποθήκευσης είναι η ρύθμιση cache δίσκου. Για SQL Server Data ένταση δίσκων συνιστάται να χρησιμοποιήσετε '**Σε cache ανάγνωση**'. Για όγκους αρχείο καταγραφής συναλλαγών, η ρύθμιση cache δίσκου θα πρέπει να έχει οριστεί σε "**καμία**". Αυτό είναι διαφορετικό από τις συστάσεις για τους λογαριασμούς τυπική χώρου αποθήκευσης.

Μόλις το VHD έχουν συνδεθεί, δεν μπορεί να αλλάξει τη ρύθμιση cache. Θα πρέπει να αποσυνδέσετε και να επανασυνδέσετε VHD με τη ρύθμιση ενημερωμένη cache.

### <a name="windows-storage-spaces"></a>Χώροι αποθήκευσης των Windows

Μπορείτε να χρησιμοποιήσετε [Χώρων αποθήκευσης του Windows](https://technet.microsoft.com/library/hh831739.aspx) όπως κάνατε με το προηγούμενο τυπική χώρου αποθήκευσης, αυτό θα σας επιτρέψει να μετεγκαταστήσετε μια Εικονική που ήδη με χρήση χώρου αποθήκευσης κενά διαστήματα. Το παράδειγμα στο [προσάρτημα](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) (βήμα 9 και προς τα εμπρός) παρουσιάζει τον κώδικα του Powershell για να εξαγάγετε και να εισαγάγετε μια Εικονική με πολλά συνημμένα VHD.

Χώρος αποθήκευσης χώρους συγκέντρωσης χρησιμοποιήθηκαν με το λογαριασμό χώρου αποθήκευσης τυπική Azure για βελτίωση της απόδοσης και να μειώσετε λανθάνων χρόνος. Μπορεί να σας φανούν τιμή κατά τη δοκιμή χώρους συγκέντρωσης χώρου αποθήκευσης με Premium αποθήκευσης για νέες αναπτύξεις, αλλά μπορούν να προσθέτουν επιπλέον πολυπλοκότητα με το πρόγραμμα εγκατάστασης του χώρου αποθήκευσης.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Πώς μπορείτε να βρείτε χάρτη που Azure εικονικών δίσκων θα χώρους συγκέντρωσης χώρου αποθήκευσης

Καθώς υπάρχουν διαφορετικές cache ρύθμιση συστάσεις για συνημμένα VHD, μπορείτε να αποφασίσετε για να αντιγράψετε το VHD σε ένα λογαριασμό του χώρου αποθήκευσης Premium. Ωστόσο, όταν τους επανασυνδέσετε τη νέα σειρά DS Εικονική, ίσως χρειαστεί να τροποποιήσετε τις ρυθμίσεις του cache. Είναι απλούστερο να εφαρμόσετε την αποθήκευση Premium προτεινόμενες ρυθμίσεις του cache, όταν έχετε νέο VHD για τα αρχεία δεδομένων SQL και αρχεία καταγραφής (αντί για μια μεμονωμένη VHD που περιέχει και τα δύο).

> [AZURE.NOTE] Εάν έχετε αρχεία δεδομένων και καταγραφής SQL Server στην ίδια την ένταση, την επιλογή σε cache που επιλέγετε εξαρτάται από τα μοτίβα πρόσβασης εισόδου/ΕΞΌΔΟΥ για το φόρτο εργασίας βάσης δεδομένων. Μόνο δοκιμές μπορεί να αποδείξει ποια επιλογή προσωρινής αποθήκευσης είναι καλύτερη για αυτό το σενάριο.

Ωστόσο, εάν χρησιμοποιείτε Windows κενά διαστήματα χώρου αποθήκευσης που αποτελούνται από πολλούς VHD θα πρέπει να κοιτάξετε το αρχικό δέσμες ενεργειών για τον προσδιορισμό που έχουν επισυναφθεί VHD είναι σε ποια συγκεκριμένη ομάδα, επομένως μπορείτε να ορίσετε τις ρυθμίσεις του cache αντίστοιχα για κάθε δίσκο.

Εάν δεν έχετε αρχικό δέσμης ενεργειών διαθέσιμη για να σας δείξουν που VHD αντιστοίχιση με το χώρο συγκέντρωσης χώρου αποθήκευσης, μπορείτε να χρησιμοποιήσετε τα παρακάτω βήματα για να προσδιορίσετε την αντιστοίχιση χώρου συγκέντρωσης/χώρου αποθήκευσης στο δίσκο.

Για κάθε δίσκο, χρησιμοποιήστε τα ακόλουθα βήματα:

1. Λάβετε λίστα δίσκων που έχουν επισυναφθεί σε Εικονική με την εντολή **Get-AzureVM** :

    Get-AzureVM - όνομα_υπηρεσίας <servicename> -όνομα <vmname> | Get-AzureDataDisk

1. Σημειώστε τον Diskname και LUN.

    ![DisknameAndLUN][2]

1. Σύνδεση απομακρυσμένης επιφάνειας εργασίας σε η Εικονική. Στη συνέχεια, επιλέξτε **Διαχείριση υπολογιστή** | **Διαχείριση συσκευών** | **μονάδων δίσκου**. Κοιτάξτε τις ιδιότητες κάθε των 'Microsoft Virtual δίσκων'

    ![VirtualDiskProperties][3]

1. Ο αριθμός LUN εδώ είναι μια αναφορά στον αριθμό LUN που καθορίζετε κατά την επισύναψη VHD για την εικονική Μηχανή.
1. Για το "Microsoft Virtual δίσκος", μεταβείτε στην καρτέλα " **Λεπτομέρειες** ", στη συνέχεια, στη λίστα **ιδιοτήτων** , μεταβείτε **Κλειδί του προγράμματος οδήγησης**. Την **τιμή**, σημειώστε το που **Μετατόπιση**, που είναι 0002 στο παρακάτω στιγμιότυπο οθόνης. Το 0002 υποδηλώνει το PhysicalDisk2 που αναφέρεται το χώρο συγκέντρωσης χώρου αποθήκευσης.

    ![VirtualDiskPropertyDetails][4]

2. Για κάθε χώρο συγκέντρωσης χώρου αποθήκευσης, η ένδειξη εκτός των συσχετισμένη δίσκων:

    Get-StoragePool - FriendlyName AMS1pooldata | Get-φυσικός δίσκος

    ![GetStoragePool][5]

Τώρα μπορείτε να χρησιμοποιήσετε αυτές τις πληροφορίες για να συσχετίσετε επισύναψη VHD φυσικών δίσκων σε χώρους συγκέντρωσης χώρου αποθήκευσης.

Μόλις που έχουν αντιστοιχιστεί VHD φυσικών δίσκων σε χώρους συγκέντρωσης χώρου αποθήκευσης, στη συνέχεια, μπορείτε να αποσπάσετε και να αντιγράψτε τα επάνω σε ένα λογαριασμό Premium χώρου αποθήκευσης, στη συνέχεια, επισυνάψτε τα με τη ρύθμιση σωστή cache. Ανατρέξτε στο θέμα το παράδειγμα στο [προσάρτημα](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), τα βήματα 8 έως 12. Αυτά τα βήματα σας δείχνουν πώς μπορείτε να εξαγάγετε μια ρύθμιση παραμέτρων του δίσκου VHD Εικονική επισυναφθεί σε ένα αρχείο CSV, αντιγράψτε το VHD, αλλάξτε τις ρυθμίσεις δίσκου ρύθμιση παραμέτρων cache και τέλος αναπτύξτε ξανά την εικονική Μηχανή ως μια σειρά DS Εικονική με όλων των συνημμένων δίσκων.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>Εικονική εύρους ζώνης χώρου αποθήκευσης και απόδοση VHD χώρου αποθήκευσης

Το μέγεθος του χώρου αποθήκευσης επιδόσεων εξαρτάται από το καθορισμένο μέγεθος DS * Εικονική και τα μεγέθη VHD. Το ΣΠΣ έχουν διαφορετικό τα όρια για τον αριθμό των VHD που μπορεί να συνδεθεί και το μέγιστο εύρος ζώνης που θα υποστηρίζουν (MB/s). Για τους αριθμούς συγκεκριμένο εύρος ζώνης, ανατρέξτε στο θέμα [εικονική μηχανή και τα μεγέθη υπηρεσία Cloud για Azure](virtual-machines-linux-sizes.md).

Αυξημένη IOP Προέλευσης είναι δυνατό με μεγαλύτερα μεγέθη δίσκου. Θα πρέπει να Όταν σκέφτεστε σχετικά με τη διαδρομή μετεγκατάστασης. Για λεπτομέρειες, [ανατρέξτε στον πίνακα για IOP Προέλευσης και τους τύπους του δίσκου](../storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

Τέλος, λάβετε υπόψη ότι ΣΠΣ έχουν διαφορετικό δίσκο μέγιστο εύρος ζώνης που θα υποστηρίζουν για όλων των δίσκων συνημμένα. Υψηλός φόρτος, θα μπορούσε να κορεσμός του εύρους μέγιστο δίσκου είναι διαθέσιμο για αυτό το μέγεθος ρόλο Εικονική. Για παράδειγμα, μια Standard_DS14 θα υποστηρίζει έως 512 MB/s, Επομένως, με τρεις δίσκων P30 που θα μπορούσε να Κορεσμός το εύρος ζώνης του δίσκου του η Εικονική. Αλλά σε αυτό το παράδειγμα, θα μπορούσε να είναι υπέρβαση του ορίου μετάδοσης ανάλογα με το συνδυασμό ανάγνωσης και εγγραφής IOs.

## <a name="new-deployments"></a>Νέα αναπτύξεις

Στις επόμενες δύο ενότητες δείχνουν πώς μπορείτε να αναπτύξετε ΣΠΣ SQL Server με τον χώρο αποθήκευσης Premium. Όπως αναφέρθηκε πριν, δεν χρειάζεται απαραίτητα για να τοποθετήσετε το δίσκο OS σε Premium χώρου αποθήκευσης. Μπορείτε να επιλέξετε να το κάνετε αυτό, εάν σκοπεύετε να τοποθετήσετε οποιαδήποτε εντατική φόρτους εργασίας εισόδου/ΕΞΌΔΟΥ στον OS VHD.

Το πρώτο παράδειγμα παρουσιάζει με χρήση υπάρχουσας Azure συλλογή εικόνων. Το δεύτερο παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε μια προσαρμοσμένη εικόνα Εικονική που έχετε σε έναν υπάρχοντα λογαριασμό τυπική χώρου αποθήκευσης.

> [AZURE.NOTE] Αυτά τα παραδείγματα προϋποθέτουν ότι έχετε ήδη δημιουργήσει ένα VNET τοπικές ρυθμίσεις.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Δημιουργήστε μια νέα Εικονική με Premium χώρου αποθήκευσης με συλλογή εικόνων

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να τοποθετήσετε το λειτουργικό σύστημα VHD στο χώρο αποθήκευσης premium και την επισύναψη VHD αποθήκευσης Premium. Ωστόσο, μπορείτε επίσης τοποθετήστε το δίσκο OS σε λογαριασμό τυπική χώρου αποθήκευσης και στη συνέχεια, επισυνάψτε VHD που βρίσκονται σε ένα λογαριασμό του χώρου αποθήκευσης Premium. Εκδηλώνονται και τα δύο σενάρια.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Βήμα 1: Δημιουργία λογαριασμού χώρου αποθήκευσης Premium


    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Βήμα 2: Δημιουργήστε μια νέα υπηρεσία στο Cloud

    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Βήμα 3: Κράτηση ενός VIP υπηρεσία Cloud (προαιρετικά)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Βήμα 4: Δημιουργία κοντέινερ Εικονική
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Βήμα 5: Τοποθέτηση OS VHD σε τυπική ή Premium χώρου αποθήκευσης
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Βήμα 6: Δημιουργία Εικονική
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Δημιουργήστε μια νέα Εικονική για να χρησιμοποιήσετε Premium χώρου αποθήκευσης με μια προσαρμοσμένη εικόνα

Σε αυτό το σενάριο δείχνει όπου έχετε υπάρχουσες προσαρμοσμένες εικόνες που βρίσκονται σε ένα λογαριασμό τυπική χώρου αποθήκευσης. Όπως αναφέρθηκε εάν θέλετε να τοποθετήσετε το λειτουργικό σύστημα VHD στο χώρο αποθήκευσης Premium θα πρέπει να αντιγράψετε την εικόνα που υπάρχει στο λογαριασμό τυπική χώρου αποθήκευσης και μεταφορά τους σε ένα χώρο αποθήκευσης Premium πριν να μπορεί να χρησιμοποιηθεί. Εάν έχετε μια εικόνα στην εσωτερική εγκατάσταση, μπορείτε επίσης να χρησιμοποιήσετε αυτήν τη μέθοδο για να αντιγράψετε που απευθείας με το λογαριασμό χώρου αποθήκευσης Premium.

#### <a name="step-1-create-storage-account"></a>Βήμα 1: Δημιουργία λογαριασμού χώρου αποθήκευσης
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Βήμα 2, δημιουργήστε μια υπηρεσία Cloud
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Βήμα 3: Χρήση υπάρχουσα εικόνα
Μπορείτε να χρησιμοποιήσετε μια υπάρχουσα εικόνα. Εναλλακτικά, μπορείτε να [λαμβάνουν μια εικόνα από μια υπάρχουσα υπολογιστή](virtual-machines-windows-classic-capture-image.md). Σημείωση η μηχανή εικόνας που δεν χρειάζεται να είναι DS* υπολογιστή. Όταν έχετε ολοκληρώσει την εικόνα, ακολουθήστε τα παρακάτω βήματα δείχνουν τον τρόπο για να το αντιγράψετε στο λογαριασμό Premium χώρου αποθήκευσης με την * *Έναρξη-AzureStorageBlobCopy** commandlet του PowerShell.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Βήμα 4: Αντιγραφή αντικειμένων Blob μεταξύ λογαριασμών χώρου αποθήκευσης
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Βήμα 5: Έλεγχος τακτικά Αντιγραφή κατάστασης:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Βήμα 6: Προσθήκη εικόνας δίσκου Azure δίσκο αποθετήριο συνδρομή
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [AZURE.NOTE] Μπορείτε να διαπιστώσετε ότι παρόλο που οι αναφορές κατάστασης με επιτυχία, θα μπορούσε να εξακολουθείτε να λαμβάνετε σφάλμα μίσθωσης στο δίσκο. Σε αυτήν την περίπτωση, περιμένετε περίπου 10 λεπτά.

#### <a name="step-7--build-the-vm"></a>Βήμα 7: Δημιουργήστε την εικονική Μηχανή
Δείτε εδώ δημιουργείτε την εικονική Μηχανή από την εικόνα σας και επισύναψη δύο VHD αποθήκευσης Premium:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Υπάρχουσες αναπτύξεις που δεν χρησιμοποιείτε πάντα σε ομάδες διαθεσιμότητας

> [AZURE.NOTE] Για υπάρχοντα αναπτύξεις, ανατρέξτε πρώτα στην ενότητα [προαπαιτούμενα στοιχεία για τις](#prerequisites-for-premium-storage) αυτού του θέματος.

Υπάρχουν διαφορετικά θέματα για αναπτύξεις SQL Server που δεν χρησιμοποιούν πάντα σε ομάδες διαθεσιμότητα και αυτές που κάνετε. Εάν δεν χρησιμοποιείτε πάντα από και έχετε μια υπάρχουσα μεμονωμένη SQL Server, μπορείτε να αναβαθμίσετε με το χώρο αποθήκευσης Premium, χρησιμοποιώντας έναν νέο λογαριασμό υπηρεσία και χώρο αποθήκευσης στο cloud. Λάβετε υπόψη τις ακόλουθες επιλογές:

- **Δημιουργία ενός νέου Εικονική SQL Server**. Μπορείτε να δημιουργήσετε μια νέα Εικονική διακομιστή SQL που χρησιμοποιεί ένα λογαριασμό Premium χώρου αποθήκευσης, όπως περιγράφεται στις νέες αναπτύξεις. Στη συνέχεια, δημιουργήστε αντίγραφα ασφαλείας και να επαναφέρετε τις βάσεις δεδομένων ρύθμισης παραμέτρων και χρήστη του SQL Server. Η εφαρμογή θα πρέπει να ενημερωθούν ώστε να αναφέρονται το νέο SQL Server, εάν η πρόσβαση εσωτερικά ή εξωτερικά. Θα πρέπει να αντιγράψετε όλα 'εκτός db' αντικείμενα όπως εάν κάνατε μια μετεγκατάσταση σε παράθεση (SxS) SQL Server. Αυτό περιλαμβάνει αντικείμενα όπως συνδέσεις, τα πιστοποιητικά και συνδεδεμένοι διακομιστές.
- **Μετεγκατάσταση Εικονική μηχανή του υπάρχοντος SQL Server**. Αυτό θα απαιτούν να διαρκεί η Εικονική SQL Server για εργασία χωρίς σύνδεση και, στη συνέχεια, που μεταφέρουν σε μια νέα υπηρεσία cloud, περιλαμβάνει όλα τα συνημμένα VHD αντιγραφή με το λογαριασμό χώρου αποθήκευσης Premium. Όταν η Εικονική προέρχονται online, η εφαρμογή θα αναφοράς το όνομα κεντρικού υπολογιστή του διακομιστή ως πριν από. Πρέπει να γνωρίζετε ότι το μέγεθος του υπάρχοντος δίσκου θα επηρεάσει τα χαρακτηριστικά επιδόσεων. Για παράδειγμα, ένα δίσκο 400 GB λαμβάνει στρογγυλοποιείται προς τα επάνω σε μια ρ20. Εάν γνωρίζετε ότι δεν απαιτεί ότι η απόδοση δίσκου, στη συνέχεια, που θα μπορούσε να αναδημιουργήσετε το Εικονική ως μια Εικονική σειράς DS, και επισύναψη Premium VHD χώρου αποθήκευσης της προδιαγραφής μέγεθος/απόδοσης που απαιτούν. Στη συνέχεια, να μπορείτε να αποσπάσετε και να επανασυνδέσετε τα αρχεία SQL DB.

> [AZURE.NOTE] Κατά την αντιγραφή του δίσκων VHD θα πρέπει να γνωρίζετε το μέγεθος, ανάλογα με το μέγεθος θα σημαίνει ότι τι τύπο δίσκου αποθήκευσης Premium εμπίπτουν σε, αυτό καθορίζει προδιαγραφή επιδόσεων δίσκου. Azure θα στρογγυλοποίηση προς τον πλησιέστερο δίσκο μεγέθους, επομένως εάν έχετε ένα δίσκο 400 GB, αυτό στρογγυλοποιείται προς τα επάνω σε μια ρ20. Ανάλογα με τις υπάρχουσες εισόδου/ΕΞΌΔΟΥ τις απαιτήσεις σας του VHD λειτουργικό σύστημα, ίσως χρειαστεί δεν για τη μετεγκατάσταση έτσι σε ένα λογαριασμό Premium χώρου αποθήκευσης.

Εάν ο SQL Server είναι εξωτερική πρόσβαση, στη συνέχεια, θα αλλάξει το VIP υπηρεσία cloud. Θα πρέπει επίσης να ενημέρωση τελικά σημεία, ACL και DNS ρυθμίσεις.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Υπάρχουσες αναπτύξεις που χρησιμοποιούν πάντα σε ομάδες διαθεσιμότητας

> [AZURE.NOTE] Για υπάρχοντα αναπτύξεις, ανατρέξτε πρώτα στην ενότητα [προαπαιτούμενα στοιχεία για τις](#prerequisites-for-premium-storage) αυτού του θέματος.

Αρχικά σε αυτήν την ενότητα θα εξετάσουμε πώς πάντα σε αλληλεπιδρά με Azure δικτύου. Θα σας, στη συνέχεια, θα διασπάστε μετεγκαταστάσεις στο σε δύο σενάρια: μετεγκαταστάσεις όπου μπορεί να επιτρέπεται ορισμένες χρόνου εκτός λειτουργίας και μετεγκαταστάσεις όπου θα πρέπει να επιτύχετε ελάχιστο χρόνο εκτός λειτουργίας.

Εσωτερικής εγκατάστασης ομάδες SQL Server πάντα στη διαθεσιμότητα Χρησιμοποιήστε μια ακρόασης εσωτερικής εγκατάστασης που καταχωρεί ένα εικονικό όνομα DNS μαζί με μια διεύθυνση IP που χρησιμοποιείται από κοινού μεταξύ ενός ή περισσότερων διακομιστών SQL Server. Όταν οι υπολογιστές-πελάτες συνδέονται δρομολογούνται μέσω του IP ακρόασης στον πρωτεύοντα διακομιστή SQL. Αυτός είναι ο διακομιστής που ανήκει ο πόρος πάντα στην IP εκείνη τη στιγμή.

![DeploymentsUseAlways σε][6]

Στο Microsoft Azure που μπορείτε να έχετε μόνο μία διεύθυνση IP που έχουν εκχωρηθεί σε NIC σε η Εικονική, επομένως, για να επιτύχετε το ίδιο επίπεδο της αφαίρεσης ως εσωτερικής εγκατάστασης, Azure χρησιμοποιεί τη διεύθυνση IP στην οποία έχει ανατεθεί η φόρτωση Balancers εσωτερική/εξωτερική (ILB ELB). Ο πόρος IP που χρησιμοποιείται από κοινού μεταξύ τους διακομιστές έχει οριστεί σε το ίδιο IP ως το ILB/ELB. Αυτό είναι Δημοσιευμένος στην το DNS και κίνησης προγράμματος-πελάτη είναι πέρασε το ILB/ELB στη ρεπλίκα πρωτεύοντος SQL Server. Το ILB/ELB γνωρίζει που SQL Server είναι πρωτεύον επειδή χρησιμοποιεί καθετήρες για τη διερεύνηση στον πόρο πάντα στην διευθύνσεων IP. Στο προηγούμενο παράδειγμα, το probes κάθε κόμβο που έχει ένα τελικό σημείο στα οποία γίνεται αναφορά από το ELB/ILB, όποιο από τα δύο αποκρίνεται είναι το κύριο SQL Server.

> [AZURE.NOTE] Η ILB και ELB αντιστοιχίζονται σε μια υπηρεσία cloud συγκεκριμένο Azure, επομένως οποιαδήποτε μετεγκατάστασης cloud στο Azure θα πιθανώς σημαίνει ότι θα αλλάξει το IP εξισορρόπησης φόρτου.

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Η μετεγκατάσταση πάντα σε αναπτύξεις που μπορούν να σας επιτρέπει ορισμένες χρόνου εκτός λειτουργίας

Υπάρχουν δύο στρατηγικές για τη μετεγκατάσταση πάντα σε αναπτύξεις που επιτρέπουν ορισμένες χρόνου εκτός λειτουργίας:

1. **Προσθέσετε περισσότερες δευτερεύουσες αντίγραφα ένα υπάρχον πάντα στο σύμπλεγμα**
1. **Μετεγκατάσταση σε νέο πάντα στο σύμπλεγμα**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. προσθέσετε περισσότερες δευτερεύουσες αντίγραφα ένα υπάρχον πάντα στο σύμπλεγμα

Μια στρατηγική είναι για να προσθέσετε περισσότερες secondaries τα πάντα στη διαθεσιμότητα ομάδα. Πρέπει να προσθέσετε αυτές τις σε μια νέα υπηρεσία στο cloud και ενημερώστε το ακροατήριο με το νέο IP εξισορρόπησης φόρτου.

##### <a name="points-of-downtime"></a>Σημεία του χρόνου εκτός λειτουργίας:

- Σύμπλεγμα επικύρωσης.
- Έλεγχος πάντα σε ανακατευθύνσεις για νέα Secondaries.

Εάν χρησιμοποιείτε Windows αποθήκευσης χώρους συγκέντρωσης εντός του Εικονική για μεγαλύτερη ταχύτητα μετάδοσης εισόδου/ΕΞΌΔΟΥ, στη συνέχεια, αυτά θα πραγματοποιηθεί χωρίς σύνδεση κατά τη διάρκεια μιας πλήρους επικύρωσης σύμπλεγμα. Όταν προσθέτετε κόμβους στο σύμπλεγμα απαιτείται η δοκιμή επικύρωσης. Το χρόνο που χρειάζεται για την εκτέλεση της δοκιμής μπορεί να διαφέρει, έτσι θα πρέπει να το δοκιμάσετε στο περιβάλλον σας αντιπρόσωπος δοκιμή για να λάβετε μια κατά προσέγγιση ώρα της πόσο αυτό θα διαρκέσει.

Θα πρέπει να προμηθεύσουν ώρας, όπου μπορείτε να εκτελείτε μη αυτόματη ανακατεύθυνση και χάος δοκιμές σε τους κόμβους που προστέθηκε πρόσφατα για να βεβαιωθείτε ότι πάντα σε υψηλή διαθεσιμότητα συναρτήσεις, όπως αναμένεται.

![DeploymentUseAlways On2][7]

> [AZURE.NOTE] Θα πρέπει να διακόψετε όλες τις εμφανίσεις του SQL Server όπου χρησιμοποιούνται τα σύνολα αποθήκευσης πριν από την εκτέλεση της επικύρωσης.
##### <a name="high-level-steps"></a>Βήματα υψηλού επιπέδου

1. Δημιουργήστε δύο νέα διακομιστές SQL σε νέα υπηρεσία στο cloud με συνημμένο αποθήκευσης Premium.
1. Αντιγράψτε επάνω σε ΠΛΉΡΗ αντίγραφα ασφαλείας και επαναφορά με **NORECOVERY**.
1. Αντιγραφή πάνω από 'από το χρήστη DB' εξαρτώμενα αντικείμενα, όπως συνδέσεις κ.λπ.
1. Δημιουργία νέας μια νέα εξισορρόπησης εσωτερικό φόρτωσης (ILB) ή χρησιμοποιήστε μια εξωτερική εξισορρόπησης φόρτου (ELB) και, στη συνέχεια, ρυθμίστε τα τελικά σημεία εξισορρόπηση φόρτου σε δύο νέα κόμβους.
> [AZURE.NOTE] Επιλέξτε όλους τους κόμβους έχουν τη σωστή ρύθμιση παραμέτρων ορίου πριν να συνεχίσετε

1. Διακοπή πρόσβασης/εφαρμογή χρήστη με τον SQL Server (Εάν χρησιμοποιείτε χώρους συγκέντρωσης χώρο αποθήκευσης).
1. Διακοπή υπηρεσιών μηχανισμός του SQL Server σε όλους τους κόμβους (Εάν χρησιμοποιείτε χώρους συγκέντρωσης χώρο αποθήκευσης).
1. Προσθήκη νέου κόμβοι να συμπλέγματος και να εκτελέσετε πλήρη επικύρωσης.
1. Όταν η επικύρωση είναι επιτυχής, ξεκινήσετε όλες τις υπηρεσίες SQL Server.
1. Δημιουργία αντιγράφων ασφαλείας αρχείων καταγραφής συναλλαγών και επαναφορά βάσεων δεδομένων χρήστη.
1. Προσθήκη νέου κόμβους στη πάντα στη διαθεσιμότητα ομάδα και τοποθετήστε αναπαραγωγής σε **σύγχρονη**.
1. Προσθέστε τον πόρο της διεύθυνσης IP του το νέο Cloud υπηρεσίας ILB/ELB μέσω του PowerShell για πάντα με βάση το παράδειγμα πολλών τοποθεσίας στο [προσάρτημα](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage). Στα Windows σύμπλεγμα, ορίστε των **πιθανών κατόχων** του πόρου **Διεύθυνση IP** για το νέο κόμβους παλιά. Ανατρέξτε στην ενότητα 'Προσθήκη πόρου διεύθυνσης IP στο ίδιο υποδίκτυο' του [προσαρτήματος](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
1. Ανακατεύθυνση σε έναν από τους κόμβους νέα.
1. Κάντε το νέο κόμβους ανακατευθύνσεις αυτόματη ανακατεύθυνση συνεργάτες και έλεγχος.
1. Κατάργηση αρχικό κόμβους από ομάδας διαθεσιμότητας.

##### <a name="advantages"></a>Τα πλεονεκτήματα

- Νέων διακομιστών SQL μπορεί να ελεγχθεί (SQL Server και εφαρμογή) πριν από την προσθήκη τους πάντα σε.
- Μπορείτε να αλλάξετε το μέγεθος του Εικονική και να προσαρμόσετε το χώρο αποθήκευσης με τις απαιτήσεις σας ακριβή. Ωστόσο, θα ήταν χρήσιμη για να διατηρήσετε το ίδιο όλες οι διαδρομές αρχείου SQL.
- Μπορείτε να ελέγχετε πότε ξεκινούν τη μεταφορά των αντιγράφων ασφαλείας DB για τη δευτερεύουσα αντίγραφα. Αυτό διαφέρει από τη χρήση commandlet Azure **AzureStorageBlobCopy έναρξης** για να αντιγράψετε VHD, καθώς αυτό είναι ένα αντίγραφο ασύγχρονης.

##### <a name="disadvantages"></a>Μειονεκτήματα
- Όταν χρησιμοποιείτε σύνολα χώρου αποθήκευσης των Windows, υπάρχει χρόνος εκτός λειτουργίας σύμπλεγμα κατά την πλήρη επικύρωση σύμπλεγμα για το νέο επιπλέον κόμβους.
- Ανάλογα με την έκδοση του SQL Server και τον υπάρχοντα αριθμό δευτερεύοντα αντίγραφα, ενδέχεται να μην μπορείτε να προσθέσετε περισσότερες δευτερεύουσες αντίγραφα χωρίς να καταργήσετε υπάρχοντα secondaries.
- Ενδέχεται να υπάρχουν πολύ χρόνο μεταφορά δεδομένων SQL κατά τη ρύθμιση του secondaries.
- Υπάρχει πρόσθετο κόστος κατά τη διάρκεια της μετεγκατάστασης, ενώ έχετε νέα μηχανήματα εκτελείται παράλληλα.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. μετεγκατάσταση σε νέο πάντα στο σύμπλεγμα

Μια άλλη στρατηγική είναι για να δημιουργήσετε ένα ολοκαίνουργιο πάντα σε σύμπλεγμα με ολοκαίνουργιο κόμβους σε νέα υπηρεσία στο cloud και, στη συνέχεια, να ανακατευθύνετε τα προγράμματα-πελάτες για να το χρησιμοποιήσετε.

##### <a name="points-of-downtime"></a>Σημεία του χρόνου εκτός λειτουργίας

Υπάρχει χρόνος εκτός λειτουργίας κατά τη μεταφορά εφαρμογές και τους χρήστες για το νέο πάντα σε πρόγραμμα ακρόασης. Ο χρόνος εκτός λειτουργίας εξαρτάται:

- Ο χρόνος που λαμβάνονται για να επαναφέρετε αντίγραφα ασφαλείας καταγραφής συναλλαγών τελικό βάσεις δεδομένων σε νέα διακομιστές.
- Ο χρόνος που λαμβάνονται για να ενημερώσετε εφαρμογές προγράμματος-πελάτη για να χρησιμοποιήσετε νέα πάντα σε ακρόασης.

##### <a name="advantages"></a>Τα πλεονεκτήματα

- Μπορείτε να ελέγξετε το περιβάλλον πραγματική παραγωγής, SQL Server, και OS Δημιουργήστε τις αλλαγές.
- Έχετε την επιλογή για να προσαρμόσετε το χώρο αποθήκευσης και πιθανώς μείωσης μεγέθους Εικονική. Αυτό θα μπορούσε να έχει ως αποτέλεσμα μείωση κόστους.
- Μπορείτε να ενημερώσετε το SQL Server Δόμηση ή η έκδοση κατά τη διάρκεια αυτής της διαδικασίας. Μπορείτε επίσης να αναβαθμίσετε το λειτουργικό σύστημα.
- Το προηγούμενο πάντα σε σύμπλεγμα μπορεί να λειτουργήσει ως προορισμό συμπαγές επαναφοράς.

##### <a name="disadvantages"></a>Μειονεκτήματα

- Πρέπει να αλλάξετε το όνομα DNS του προγράμματος ακρόασης εάν θέλετε τόσο συμπλεγμάτων πάντα στην εκτέλεση ταυτόχρονα. Αυτό προσθέτει διαχείρισης επιβάρυνσης κατά τη διάρκεια της μετεγκατάστασης, όπως συμβολοσειρές εφαρμογή προγράμματος-πελάτη πρέπει να αντικατοπτρίζει το νέο όνομα ακρόασης.
- Πρέπει να υλοποιήσετε ένα μηχανισμό συγχρονισμού μεταξύ των δύο περιβάλλοντα για να διατηρήσετε τους όσο το δυνατό για να ελαχιστοποιήσετε τις απαιτήσεις τελικό συγχρονισμού πριν από την μετεγκατάσταση.
- Θα προστεθεί κόστος κατά τη διάρκεια της μετεγκατάστασης ενώ έχετε το νέο περιβάλλον που εκτελείται.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Η μετεγκατάσταση πάντα στην αναπτύξεων για τον ελάχιστο χρόνο εκτός λειτουργίας

Υπάρχουν δύο στρατηγικές για τη μετεγκατάσταση πάντα σε αναπτύξεις για ελάχιστο χρόνο εκτός λειτουργίας:

1. **Χρησιμοποιούν ένα υπάρχον δευτερεύον: μίας τοποθεσίας**
1. **Χρησιμοποιούν υπάρχοντα δευτερεύοντα Replica(s): πολλών τοποθεσίας**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. χρησιμοποιούν ένα υπάρχον δευτερεύον: μίας τοποθεσίας

Μια στρατηγική για ελάχιστο χρόνο εκτός λειτουργίας είναι για να τραβήξετε μια υπάρχουσα cloud δευτερεύοντα και να την καταργήσετε από την τρέχουσα υπηρεσία cloud. Στη συνέχεια, αντιγράψτε το VHD με το νέο λογαριασμό Premium χώρου αποθήκευσης και δημιουργήστε την εικονική Μηχανή στην νέα υπηρεσία cloud. Στη συνέχεια, ενημερώστε το ακροατήριο στο σύμπλεγμα και ανακατεύθυνσης.

##### <a name="points-of-downtime"></a>Σημεία του χρόνου εκτός λειτουργίας

- Υπάρχει χρόνος εκτός λειτουργίας, όταν ενημερώνετε το τελικό κόμβο με το τελικό σημείο εξισορρόπηση φόρτου.
- Το πρόγραμμα-πελάτη επανασύνδεση μπορεί να καθυστερήσει ανάλογα με τη ρύθμιση παραμέτρων του προγράμματος-πελάτη/DNS σας.
- Υπάρχει επιπλέον χρόνου εκτός λειτουργίας, εάν επιλέξετε να κάνετε την ομάδα πάντα σε σύμπλεγμα εκτός σύνδεσης για να εναλλαγή ανάληψη στις διευθύνσεις IP. Μπορείτε να αποφύγετε αυτό, χρησιμοποιώντας μια εξάρτηση OR και πιθανών κατόχων για τον πόρο πρόσθετη διεύθυνση IP. Ανατρέξτε στην ενότητα 'Προσθήκη πόρου διεύθυνσης IP στο ίδιο υποδίκτυο' του [προσαρτήματος](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).

> [AZURE.NOTE] Όταν θέλετε στον κόμβο προστέθηκε για να partake στο ως ένα πάντα στην ανακατεύθυνσης συνεργάτη, πρέπει να προσθέσετε ένα τελικό σημείο Azure με μια αναφορά φόρτωση εξισορρόπηση συνόλου. Όταν εκτελείτε την εντολή **Προσθήκη AzureEndpoint** να το κάνετε αυτό, τρέχουσες συνδέσεις για να παραμείνει ανοικτό, αλλά νέα συνδέσεις για το ακροατήριο δεν θα μπορούν να καθοριστούν μέχρι τη μονάδα εξισορρόπησης φόρτου έχει ενημερωθεί. Κατά τη δοκιμή αυτή ήταν φαίνεται να τελευταίες 90-120seconds, αυτό θα πρέπει να ελεγχθεί.

##### <a name="advantages"></a>Τα πλεονεκτήματα

- Χωρίς επιπλέον κόστος που προκύπτει κατά τη διάρκεια της μετεγκατάστασης.
- Ένα-προς-ένα μετεγκατάστασης.
- Μειωμένη πολυπλοκότητα.
- Επιτρέπει αυξημένη IOP Προέλευσης από Premium αποθήκευσης SKU. Όταν το δίσκων είναι αποσυνδεδεμένο από την εικονική Μηχανή και αντιγραφεί σε νέα υπηρεσία cloud, τρίτου κατασκευαστή εργαλείο μπορεί να χρησιμοποιηθεί για να αυξήσετε το μέγεθος VHD, η οποία παρέχει υψηλότερη throughputs. Για την αύξηση της μεγέθη VHD, ανατρέξτε στο θέμα αυτό [φόρουμ συζήτησης](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Μειονεκτήματα

- Υπάρχει ένα προσωρινό απώλεια HA και DR κατά τη διάρκεια της μετεγκατάστασης.
- Καθώς πρόκειται για μια μετεγκατάσταση 1:1, θα πρέπει να χρησιμοποιήσετε ένα ελάχιστο μέγεθος Εικονική που θα υποστηρίζει τον αριθμό των VHD, ώστε να μπορεί να μην μπορείτε να downsize ΣΠΣ σας.
- Αυτό το σενάριο θα χρησιμοποιήσετε το Azure **Έναρξη-AzureStorageBlobCopy** commandlet, που είναι ασύγχρονη. Δεν υπάρχει καμία SLA μετά την ολοκλήρωση αντίγραφο. Την ώρα της τα αντίγραφα ποικίλλει, ενώ αυτό εξαρτάται από αναμονής στην ουρά του επίσης εξαρτάται από την ποσότητα των δεδομένων για τη μεταφορά. Ο χρόνος αντιγραφής αυξάνεται αν η μεταφορά πρόκειται να άλλο κέντρο Azure δεδομένων που υποστηρίζει Premium χώρου αποθήκευσης σε μια άλλη περιοχή. Εάν έχετε απλώς 2 κόμβους, εξετάστε το ενδεχόμενο μια πιθανή μετριασμού σε περίπτωση που το αντίγραφο διαρκεί περισσότερο από κατά τη δοκιμή. Αυτό θα μπορούσε να περιλαμβάνει τα παρακάτω ιδέες.
    - Προσθέστε ένα προσωρινό 3η κόμβο SQL Server για HA πριν από τη μετεγκατάσταση με αποδεκτής χρόνου εκτός λειτουργίας.
    - Εκτελέστε τη μετεγκατάσταση εκτός του Azure προγραμματισμένη συντήρηση.
    - Βεβαιωθείτε ότι έχετε ρυθμίσει τις παραμέτρους του απαρτία συμπλέγματος σωστά.  

##### <a name="high-level-steps"></a>Βήματα υψηλού επιπέδου

Αυτό το έγγραφο δεν δείχνουν μια ολοκληρωμένη παράδειγμα τελικών, ωστόσο [προσάρτημα](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) παρέχει λεπτομέρειες που μπορούν να χρησιμοποιηθούν για να εκτελέσετε αυτό.

![MinimalDowntime][8]

- Ρύθμιση παραμέτρων του δίσκου να συγκεντρώσετε και να καταργήσετε τον κόμβο (μην διαγράψετε συνημμένο VHD).
- Δημιουργία λογαριασμού χώρου αποθήκευσης Premium και αντιγράψτε VHD από το λογαριασμό τυπική χώρου αποθήκευσης
- Δημιουργία νέας υπηρεσίας cloud και αναπτύξτε ξανά τα Εικονική SQL2 στα αυτήν την υπηρεσία cloud. Δημιουργήστε την εικονική Μηχανή χρησιμοποιώντας το αντιγραμμένο αρχικό VHD λειτουργικό σύστημα και το αντιγραμμένο VHD επισύναψη.
- Ρύθμιση παραμέτρων ILB / ELB και προσθέστε τα τελικά σημεία.
- Ενημερώστε ακρόασης, είτε:
    - Λήψη πάντα στην ομάδα χωρίς σύνδεση και ενημέρωση τα πάντα σε ακρόασης με νέα ILB / διεύθυνση ELB IP.
    - Ή προσθέτοντας τον πόρο διεύθυνση IP του νέου Cloud υπηρεσίας ILB/ELB μέσω του PowerShell στο σύμπλεγμα Windows. Στη συνέχεια, ορίστε το πιθανών κατόχων του πόρου διεύθυνση IP στον κόμβο μετεγκαταστάθηκαν, SQL2, και ορίσετε ως εξάρτηση ή στο όνομα του δικτύου. Ανατρέξτε στην ενότητα 'Προσθήκη πόρου διεύθυνσης IP στο ίδιο υποδίκτυο' του [προσαρτήματος](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
- Έλεγχος ρύθμισης παραμέτρων DNS/αποστολέας στα προγράμματα-πελάτες.
- Μετεγκατάσταση Εικονική SQL1 και ακολουθήσετε τα βήματα 2-4.
- Εάν χρησιμοποιείτε 5ii βήματα, στη συνέχεια, προσθέστε SQL1 ως κάτοχος πιθανές για τον πόρο διεύθυνσης IP που προστέθηκε
- Δοκιμή ανακατευθύνσεις.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. χρησιμοποιούν υπάρχοντα δευτερεύοντα replica(s): πολλών τοποθεσίας

Εάν έχετε κόμβους σε περισσότερα από ένα κέντρο δεδομένων Azure (ελεγκτή Τομέα) ή εάν έχετε ένα υβριδικό περιβάλλον, στη συνέχεια, μπορείτε να χρησιμοποιήσετε μια πάντα στη ρύθμιση παραμέτρων του σε αυτό το περιβάλλον για να ελαχιστοποιήσετε το χρόνο εκτός λειτουργίας.

Η προσέγγιση είναι να αλλάξετε το συγχρονισμό πάντα σε σε σύγχρονη για τα εσωτερικής εγκατάστασης ή δευτερεύοντα Azure ελεγκτή Τομέα και, στη συνέχεια, ανακατεύθυνση πάνω από τον SQL Server. Στη συνέχεια, αντιγράψτε το VHD σε ένα λογαριασμό του χώρου αποθήκευσης Premium, και αναπτύξτε ξανά υπολογιστή σε μια νέα υπηρεσία στο cloud. Ενημερώστε το ακροατήριο και, στη συνέχεια, αποτύχει ξανά.

##### <a name="points-of-downtime"></a>Σημεία του χρόνου εκτός λειτουργίας

Ο χρόνος εκτός λειτουργίας αποτελείται από την ώρα για να ανακατεύθυνσης για την εναλλακτική ελεγκτή Τομέα και πίσω. Επίσης εξαρτάται από τη ρύθμιση παραμέτρων του προγράμματος-πελάτη/DNS σας και ενδέχεται να καθυστερήσει σας επανασύνδεση προγράμματος-πελάτη.
Εξετάστε το ακόλουθο παράδειγμα μιας υβριδικής διαμόρφωσης πάντα σε:

![MultiSite1][9]

##### <a name="advantages"></a>Τα πλεονεκτήματα

- Μπορείτε να χρησιμοποιήσετε την υπάρχουσα υποδομή.
- Έχετε την επιλογή προ-αναβάθμισης του Azure αποθήκευσης στον ελεγκτή Τομέα DR Azure πρώτα.
- Ο χώρος αποθήκευσης του ελεγκτή Τομέα DR Azure μπορούν να ρυθμιστούν.
- Υπάρχει τουλάχιστον δύο ανακατευθύνσεις κατά τη διάρκεια της μετεγκατάστασης, εκτός από τα ανακατευθύνσεις δοκιμής.
- Δεν χρειάζεται να μετακινήσετε δεδομένα του SQL Server με αντίγραφα ασφαλείας και επαναφορά.

##### <a name="disadvantages"></a>Μειονεκτήματα

- Ανάλογα με το πρόγραμμα-πελάτη πρόσβαση σε SQL Server, ενδέχεται να υπάρχουν αυξημένη λανθάνων χρόνος όταν εκτελείται το SQL Server σε ένα κέντρο Δεδομένων του εναλλακτικού στην εφαρμογή.
- Την ώρα αντίγραφο της VHD με το χώρο αποθήκευσης Premium θα μπορούσε να είναι μεγάλο. Αυτό μπορεί να επηρεάσει την απόφασή σας σε εάν θέλετε να διατηρήσετε τον κόμβο στην ομάδα διαθεσιμότητα. Λάβετε υπόψη το εξής για όταν καταγραφής δουλεύετε φορτία εκτελούνται κατά τη διάρκεια της μετεγκατάστασης απαιτείται, επειδή ο κύριος κόμβος θα πρέπει να διατηρήσετε τις έχετε συναλλαγές σε το αρχείο καταγραφής συναλλαγών. Επομένως, αυτό θα μπορούσε να αναπτυχθούν σημαντικά.
- Αυτό το σενάριο θα χρησιμοποιήσετε το Azure **Έναρξη-AzureStorageBlobCopy** commandlet, που είναι ασύγχρονη. Δεν υπάρχει καμία SLA μετά την ολοκλήρωση. Την ώρα της τα αντίγραφα ποικίλλει, ενώ αυτό εξαρτάται από αναμονής στην ουρά, αυτό θα επίσης εξαρτώνται από την ποσότητα των δεδομένων για τη μεταφορά. Επομένως, έχετε απλώς έναν κόμβο στο κέντρο δεδομένων σας 2η, θα πρέπει να λάβετε βήματα μετριασμού σε περίπτωση που το αντίγραφο διαρκεί περισσότερο από κατά τη δοκιμή. Αυτό θα μπορούσε να περιλαμβάνει τα παρακάτω ιδέες.
    - Προσθέστε ένα προσωρινό 2η κόμβο SQL για HA πριν από τη μετεγκατάσταση με αποδεκτής χρόνου εκτός λειτουργίας.
    - Εκτελέστε τη μετεγκατάσταση εκτός του Azure προγραμματισμένη συντήρηση.
    - Βεβαιωθείτε ότι έχετε ρυθμίσει τις παραμέτρους του απαρτία συμπλέγματος σωστά.

Αυτό το σενάριο προϋποθέτει ότι έχετε τεκμηριώνονται την εγκατάσταση και γνωρίζετε πώς έχει αντιστοιχιστεί ο χώρος αποθήκευσης για να κάνετε αλλαγές για ρυθμίσεις του cache βέλτιστη δίσκου.

##### <a name="high-level-steps"></a>Βήματα υψηλού επιπέδου
![Multisite2][10]

- Κάντε το εσωτερικής / εναλλάξ το πρωτεύον του SQL Server Azure ελεγκτή Τομέα και, ώστε τα άλλα αυτόματη ανακατεύθυνση συνεργάτη (AFP).
- Συλλογή πληροφοριών ρύθμισης παραμέτρων δίσκου από SQL2 και καταργήστε τον κόμβο (μην διαγράψετε συνημμένο VHD).
- Δημιουργία λογαριασμού χώρου αποθήκευσης Premium και αντιγράψτε VHD από το λογαριασμό τυπική χώρου αποθήκευσης.
- Δημιουργήστε μια νέα υπηρεσία στο cloud και δημιουργήστε την εικονική Μηχανή SQL2 με το χώρο αποθήκευσης τιμολογίων δίσκων συνημμένα.
- Ρύθμιση παραμέτρων ILB / ELB και προσθέστε τα τελικά σημεία.
- Ενημερώστε τα πάντα σε ακρόασης με νέα ILB / ELB IP διεύθυνση και δοκιμής ανακατεύθυνσης.
- Ελέγξτε τις παραμέτρους του DNS.
- Αλλαγή του AFP σε SQL2, και, στη συνέχεια, μετεγκατάσταση SQL1 και ακολουθήσετε τα βήματα 2-5.
- Δοκιμή ανακατευθύνσεις.
- Εναλλαγή του AFP back to SQL1 και SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Προσάρτημα: Μετεγκατάσταση μιας Multisite πάντα σε σύμπλεγμα στο χώρο αποθήκευσης Premium

Το υπόλοιπο της αυτό το θέμα παρέχει ένα αναλυτικό παράδειγμα της μετατροπής του πολλών τοποθεσίας πάντα σε σύμπλεγμα με το χώρο αποθήκευσης Premium. Επίσης μετατρέπει το ακροατήριο από τη χρήση ενός εξωτερικού εξισορρόπηση φόρτου (ELB) σε μια εσωτερική εξισορρόπηση φόρτου (ILB).

### <a name="environment"></a>Περιβάλλον

- Windows 2k 12 / SQL 2k 12
- 1 DB αρχεία σε SP
- 2 x χώρους συγκέντρωσης χώρου αποθήκευσης ανά κόμβου

![Appendix1][11]

### <a name="vm"></a>ΕΙΚΟΝΙΚΉ:

Σε αυτό το παράδειγμα, πρόκειται να δείχνουν Μετακίνηση από ένα ELB στο ILB. ELB ήταν διαθέσιμες πριν από την ILB, ώστε η επιλογή αυτή εμφανίζει τον τρόπο για να μεταβείτε σε αυτό κατά τη διάρκεια της μετεγκατάστασης.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Βήματα PRE: Σύνδεση με τη συνδρομή

    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Βήμα 1: Δημιουργία νέου λογαριασμού χώρου αποθήκευσης και υπηρεσία στο Cloud
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Βήμα 2: Αύξηση τα ρητά αποτυχίες πόροι<Optional>
Σε ορισμένους πόρους που ανήκουν σε πάντα στη διαθεσιμότητα την ομάδα σας υπάρχουν όρια πόσες σφάλματα που μπορεί να προκύψουν σε μια περίοδο, όπου η υπηρεσία συμπλέγματος θα επιχειρήσει να επανεκκινήσετε την ομάδα των πόρων. Συνιστάται να αυξήσετε αυτήν ενώ που ενέργειες μέσω αυτήν τη διαδικασία, δεδομένου ότι εάν δεν το χρησιμοποιείτε με μη αυτόματο τρόπο ανακατεύθυνσης και έναυσμα ανακατευθύνσεις κλείνοντας μηχανές μπορείτε να μεταβείτε σε αυτό το όριο.

Θα ήταν συνετή σε διπλό την αποτυχία έκπτωση, να το κάνετε αυτό στη Διαχείριση ανακατεύθυνσης συμπλεγμάτων, μεταβείτε στις ιδιότητες της ομάδας πόρων πάντα σε:

![Appendix3][13]

Αλλάξτε το μέγιστο αποτυχίες 6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Βήμα 3: Προσθήκη IP Address πόρων για την ομάδα συμπλέγματος<Optional>

Εάν έχετε μόνο μία διεύθυνση IP για την ομάδα σύμπλεγμα και αυτό είναι στοιχισμένο στο cloud υποδίκτυο, έχετε υπόψη σας, εάν κατά λάθος λαμβάνετε εκτός σύνδεσης όλους τους κόμβους συμπλέγματος στο cloud σε αυτό το δίκτυο, στη συνέχεια, ο πόρος συμπλέγματος IP και όνομα δικτύου του συμπλέγματος δεν θα μπορείτε να μεταφερθεί σε σύνδεση. Σε περίπτωση αυτή θα αποτρέψει ενημερώσεις με άλλους πόρους συμπλέγματος.

#### <a name="step-4-dns-configuration"></a>Βήμα 4: Ρύθμιση παραμέτρων του DNS

Για την υλοποίηση ομαλή μετάβασης εξαρτάται από τον τρόπο που DNS χρησιμοποιούμενη και ενημερώνονται.
Όταν πάντα σε είναι εγκατεστημένο, δημιουργεί μια ομάδα πόρων συμπλέγματος των Windows, εάν ανοίξετε τη Διαχείριση συμπλέγματος ανακατεύθυνσης, θα δείτε ότι αυτό θα έχουν τουλάχιστον τρεις πόρους, τις δύο που αναφέρεται στο έγγραφο:

- Εικονικό όνομα δικτύου (VNN) – αυτό είναι το όνομα DNS που σύνδεση προγράμματος-πελάτη όταν θελήσετε να συνδεθείτε με διακομιστές SQL μέσω πάντα σε.
- Πόρος διεύθυνση IP – αυτή είναι η διεύθυνση IP που σχετίζεται με το VNN, μπορείτε να έχετε περισσότερες από μία και σε μια ρύθμιση παραμέτρων του που θα έχει μια διεύθυνση IP ανά τοποθεσία/υποδίκτυο.

Κατά τη σύνδεση με τον SQL Server, το SQL Server Client πρόγραμμα οδήγησης θα ανακτήσει τις εγγραφές DNS που σχετίζονται με την παρακολούθηση και προσπαθήστε να συνδεθείτε με κάθε πάντα σε συσχετισμένη διεύθυνση IP, κάτω από το στοιχείο αναλύονται ορισμένα παράγοντες που μπορούν να επηρεάσουν αυτό.

Τον αριθμό των ταυτόχρονες τις εγγραφές DNS που συσχετίζονται με το όνομα ακρόασης εξαρτάται όχι μόνο από τον αριθμό των διευθύνσεων IP που σχετίζεται, αλλά το ' RegisterAllIpProviders'setting στο συμπλέγματος ανακατεύθυνσης για τον πόρο πάντα ON VNN.

Κατά την ανάπτυξη πάντα σε στο Azure υπάρχουν διαφορετικά βήματα για να δημιουργήσετε το ακροατήριο και διευθύνσεις IP, πρέπει να ρυθμίσετε με μη αυτόματο τρόπο το 'RegisterAllIpProviders' σε 1, αυτό είναι διαφορετικό σε ένα στην εσωτερική εγκατάσταση πάντα στην ανάπτυξη όπου έχει ήδη οριστεί σε 1.

Εάν 'RegisterAllIpProviders' είναι 0, στη συνέχεια, θα βλέπετε μόνο μία εγγραφή DNS σε DNS που σχετίζονται με την παρακολούθηση:

![Appendix4][14]

Εάν 'RegisterAllIpProviders' είναι 1:

![Appendix5][15]

Τον παρακάτω κώδικα θα ένδειξη εκτός των ρυθμίσεων VNN και ρυθμίσετε για εσάς, σημειώστε, για την αλλαγή να τεθούν σε ισχύ, θα χρειαστεί να μεταφέρετε το VNN εκτός σύνδεσης και πάλι σε σύνδεση, τη μετατρέψετε σε αυτό το ακροατήριο χωρίς σύνδεση προκαλεί διαρκεί διαταραχή συνδεσιμότητα υπολογιστή-πελάτη.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

Μετεγκατάσταση αργότερα θα πρέπει να ενημερώσετε το ακροατήριο πάντα σε με ενημερωμένη διεύθυνση IP που θα αναφέρονται σε μια μονάδα εξισορρόπησης φόρτου, αυτό θα περιλαμβάνουν μια διεύθυνση IP Κατάργηση πόρων και προσθήκη. Μετά την ενημέρωση διευθύνσεων IP, πρέπει να βεβαιωθείτε ότι η νέα διεύθυνση IP έχει ενημερωθεί στη ζώνη DNS και ότι οι υπολογιστές-πελάτες ενημερώνετε το τοπικό cache DNS.

Εάν οι υπολογιστές-πελάτες που βρίσκονται σε ένα τμήμα διαφορετικό δίκτυο και αναφέρονται σε διαφορετικό διακομιστή DNS, πρέπει να σκεφτείτε τι συμβαίνει σχετικά με τη μεταφορά ζώνης DNS κατά τη διάρκεια της μετεγκατάστασης, όπως συνδεθείτε ξανά την εφαρμογή φορά που θα περιορίζεται από τουλάχιστον την ώρα ζώνης μεταφέρετε οποιαδήποτε νέα διευθύνσεων IP για το ακροατήριο. Εάν βρίσκεστε στην περιοχή ώρα περιορισμού εδώ, θα πρέπει να μπορείτε να συζητάτε και να δοκιμάσετε να επιβάλετε μια επαυξητική μεταφορά ζώνης με τις ομάδες σας Windows και επίσης να τοποθετήσετε την εγγραφή DNS κεντρικού υπολογιστή για ένα κάτω χρόνου To Live (TTL), ώστε τα προγράμματα-πελάτες ενημέρωση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αυξάνονται μεταφορές ζώνης](https://technet.microsoft.com/library/cc958973.aspx) και [Έναρξη-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Από προεπιλογή το TTL για εγγραφή DNS που είναι συσχετισμένη με το ακροατήριο σε πάντα στο Azure είναι 1200 δευτερόλεπτα. Ίσως θελήσετε να μειώσετε αυτό εάν βρίσκεστε στην περιοχή χρόνος περιορισμού κατά τη διάρκεια της μετεγκατάστασης για να βεβαιωθείτε ότι οι υπολογιστές-πελάτες ενημέρωση τους DNS με τη διεύθυνση ΙΡ του ενημερωμένο το ακροατήριο. Μπορείτε να δείτε και να τροποποιήσετε τη ρύθμιση παραμέτρων με ντάμπινγκ ανάληψη τη ρύθμιση παραμέτρων του VNN:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Σημειώστε, στο κάτω το 'HostRecordTTL', θα παρουσιαστεί υψηλότερου ποσού κυκλοφορία DNS.

##### <a name="client-application-settings"></a>Ρυθμίσεις εφαρμογής προγράμματος-πελάτη

Εάν η εφαρμογή προγράμματος-πελάτη σας SQL υποστηρίζει το .net διαίρεσης 4,5 SQLClient, στη συνέχεια, μπορείτε να χρησιμοποιήσετε ' MULTISUBNETFAILOVER = TRUE' λέξεων-κλειδιών, αυτό συνιστάται να εφαρμοστούν καθώς επιτρέπει την ταχύτερη σύνδεσης για να πάντα στη διαθεσιμότητα ομάδα SQL κατά την ανακατεύθυνση. Απαριθμεί μέσω όλες τις διευθύνσεις IP που σχετίζεται με την παρακολούθηση πάντα σε παράλληλα και εκτελεί μια πιο αυστηρές ταχύτητα "Επανάληψη" σύνδεσης TCP κατά τη διάρκεια της ανακατεύθυνσης.

Για περισσότερες πληροφορίες σχετικά με τις παραπάνω ρυθμίσεις, ανατρέξτε στο θέμα [MultiSubnetFailover λέξη-κλειδί και δυνατότητες που σχετίζονται](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Δείτε επίσης [SqlClient υποστήριξη για υψηλή διαθεσιμότητα, αποκατάσταση](https://msdn.microsoft.com/library/hh205662(v=vs.110).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Βήμα 5: Ρυθμίσεις απαρτίας συμπλέγματος

Καθώς πρόκειται να απομάκρυνση τουλάχιστον μία SQL Server προς τα κάτω κάθε φορά, θα πρέπει να τροποποιήσετε τη ρύθμιση απαρτίας συμπλέγματος, εάν χρησιμοποιείτε μαρτυρίας κοινή χρήση αρχείου (FSW) με 2 κόμβους, θα πρέπει να ορίσετε το απαρτίας να επιτρέπουν μεγαλύτερο μέρος κόμβο και να χρησιμοποιούν δυναμική εκλογής και πρόκειται για να επιτρέψετε για έναν μεμονωμένο κόμβο να διατηρείτε θέση κατάταξης.


    Set-ClusterQuorum -NodeMajority  

Για περισσότερες πληροφορίες σχετικά με τη διαχείριση και ρύθμιση παραμέτρων του συμπλέγματος απαρτίας, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων και διαχείριση του απαρτίας σε ένα σύμπλεγμα ανακατεύθυνσης Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Βήμα 6: Εξαγωγή υπάρχοντα τελικά σημεία και ACL
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Αποθηκεύστε τις σε ένα αρχείο κειμένου.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Βήμα 7: Αλλαγή λειτουργιών αναπαραγωγής και συνεργάτες ανακατεύθυνσης

Εάν έχετε περισσότερους από 2 διακομιστές SQL, πρέπει να αλλάξετε την ανακατεύθυνση της άλλο δευτερεύον στο άλλο ελεγκτή Τομέα ή στην εσωτερική εγκατάσταση σε 'Σύγχρονη' και να είναι ένα συνεργάτη αυτόματη ανακατεύθυνση (AFP), αυτό είναι, ώστε να μπορείτε να διατηρήσετε HA ενώ πραγματοποιείτε αλλαγές. Μπορείτε να το κάνετε μέσω TSQL της τροποποίηση μέσω SSMS:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Βήμα 8: Κατάργηση δευτερεύοντα Εικονική από μια υπηρεσία cloud

Θα πρέπει να σχεδιάζετε να μετεγκαταστήσετε έναν δευτερεύοντα κόμβο cloud πρώτα, εάν πρόκειται για τη συγκεκριμένη στιγμή κύρια, θα πρέπει να μπορείτε να ξεκινήσετε μια μη αυτόματη ανακατεύθυνση.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Βήμα 9: Αλλάξτε δίσκου προσωρινή αποθήκευση ρυθμίσεων στο αρχείο CSV και αποθήκευση

Για όγκους δεδομένων αυτά θα πρέπει να οριστεί ως μόνο για ΑΝΆΓΝΩΣΗ.

Για TLOG όγκους αυτά θα πρέπει να οριστεί σε ΚΑΝΈΝΑ.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Βήμα 10: Αντιγραφή τους VHD
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Μπορείτε να ελέγξετε την κατάσταση αντίγραφο του VHD με το λογαριασμό χώρου αποθήκευσης Premium:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Περιμένετε έως ότου όλα αυτά τα δεδομένα καταχωρούνται ως επιτυχίας.

Για πληροφορίες για μεμονωμένα αντικείμενα BLOB:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Βήμα 11: Register OS δίσκου

    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Βήμα 12: Εισαγωγή δευτερεύοντα σε νέα υπηρεσία στο cloud

Ο παρακάτω κώδικας χρησιμοποιεί επίσης την επιλογή προστέθηκε Εδώ μπορείτε να εισαγάγετε τον υπολογιστή και να χρησιμοποιήσετε το retainable VIP.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Βήμα 13: Δημιουργία ILB στη νέα Cloud Svc, προσθέστε φόρτωσης εξισορρόπηση τελικά σημεία και ACL
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

####<a name="step-14-update-always-on"></a>Βήμα 14: Ενημέρωση πάντα σε
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Τώρα μπορείτε να καταργήσετε την παλιά υπηρεσία cloud διεύθυνση IP.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Βήμα 15: Η ενημέρωση DNS ελέγχου

Μπορείτε τώρα θα πρέπει να ελέγξετε διακομιστές DNS για το πρόγραμμα-πελάτης δίκτυα SQL Server και να βεβαιωθείτε ότι η σύμπλεγμα έχει προσθέσει την εγγραφή επιπλέον κεντρικού υπολογιστή για τη διεύθυνση IP που προσθέσατε. Εάν αυτές οι διακομιστές DNS δεν έχουν ενημερωθεί, εξετάστε το ενδεχόμενο να επιβάλετε μια μεταφορά ζώνης DNS και βεβαιωθείτε ότι οι υπολογιστές-πελάτες εκεί υποδικτύου έχουν τη δυνατότητα να επιλύσετε σε δύο πάντα σε διευθύνσεις IP, αυτό είναι έτσι δεν χρειάζεται να περιμένουν για αυτόματη αναπαραγωγή DNS.

#### <a name="step-16-reconfigure-always-on"></a>Βήμα 16: Ρυθμίστε ξανά τις παραμέτρους πάντα σε

Σε αυτό το σημείο που περιμένετε για τη δευτερεύουσα αυτόν τον κόμβο που μετεγκαταστάθηκαν να πλήρως συγχρονίστε με τον κόμβο εσωτερική εγκατάσταση και μεταβείτε κόμβο σύγχρονη αναπαραγωγής και να είναι η AFP.  

#### <a name="step-17-migrate-second-node"></a>Βήμα 17: Μετεγκατάσταση δεύτερο κόμβου
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Βήμα 18: Αλλάξτε δίσκου προσωρινή αποθήκευση ρυθμίσεων στο αρχείο CSV και αποθήκευση

Για όγκους δεδομένων αυτά θα πρέπει να οριστεί ως μόνο για ΑΝΆΓΝΩΣΗ.

Για TLOG όγκους αυτά θα πρέπει να οριστεί σε ΚΑΝΈΝΑ.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Βήμα 19: Δημιουργία νέου λογαριασμού ανεξάρτητη χώρου αποθήκευσης για δευτερεύοντα κόμβο
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Βήμα 20: Αντιγραφή οι VHD
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Μπορείτε να ελέγξετε την κατάσταση αντίγραφο VHD για όλους τους VHD: ForEach ($disk στο $diskobjects) {$lun = $disk. LUN $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Ετικέτα δίσκου $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Περιμένετε έως ότου όλες αυτές τις καταγράφονται ως επιτυχίας.

Για πληροφορίες για μεμονωμένα αντικείμενα BLOB: κατάσταση blob induvidual #Check Get AzureStorageBlobCopyState-αντικειμένων Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd"-κοντέινερ $containerName-περιβάλλοντος $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Βήμα 21: Register OS δίσκου
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Βήμα 22: Προσθήκη φόρτωση εξισορρόπηση τελικά σημεία και ACL
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure classic portal or Machine Endpoints through powershell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Βήμα 23: Δοκιμή ανακατεύθυνσης

Τώρα που θα πρέπει να ενημερώσετε τον κόμβο μετεγκαταστάθηκαν συγχρονισμός με την εσωτερική εγκατάσταση πάντα σε κόμβο, τοποθέτηση σε λειτουργία αναπαραγωγής σύγχρονη και περιμένετε μέχρι να συγχρονιστεί. Στη συνέχεια, μετεγκατάσταση ανακατεύθυνσης από εσωτερικής εγκατάστασης στον πρώτο κόμβο, ποιο είναι το AFP. Μετά που έχει ολοκληρώθηκε με επιτυχία, η αλλαγή του τελευταίου κόμβου μετεγκαταστάθηκαν το AFP.

Πρέπει να ελέγξετε ανακατευθύνσεις ανάμεσα σε όλους τους κόμβους και εκτελέστε αν αναμένεται χάος δοκιμές για να βεβαιωθείτε ότι ανακατευθύνσεις εργασίας ως και σε μια έγκαιρη manor.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Βήμα 24: Τοποθέτηση ξανά ρυθμίσεις απαρτίας συμπλέγματος / DNS TTL / Pntrs ανακατεύθυνσης / ρυθμίσεων συγχρονισμού
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Προσθήκη πόρου διεύθυνσης IP στο ίδιο υποδίκτυο

Εάν έχετε μόνο 2 διακομιστές SQL και θέλετε να μετεγκαταστήσετε τα σε μια νέα υπηρεσία cloud, αλλά θέλετε να διατηρήσετε τους στο ίδιο υποδίκτυο, μπορείτε να αποφύγετε διαρκεί η ακρόαση εκτός σύνδεσης για να διαγράψετε την αρχική πάντα στη διεύθυνση IP και να προσθέσετε τη νέα διεύθυνση IP. Εάν κάνετε μετεγκατάσταση του ΣΠΣ άλλο υποδίκτυο δεν θα πρέπει να το κάνετε αυτό, όπως θα υπάρξει ένα δίκτυο επιπλέον σύμπλεγμα που θα αναφέρονται σε αυτό το δευτερεύον δίκτυο.

Αφού έφερε προς τα επάνω στο δευτερεύον μετεγκαταστάθηκαν και προστεθεί στον νέο πόρο διεύθυνση IP για τη νέα υπηρεσία cloud πριν από την ανακατεύθυνση το υπάρχον πρωτεύον, θα πρέπει να κάνετε αυτά τα βήματα μέσα σε Διαχείριση ανακατεύθυνσης συμπλέγματος:

Για να προσθέσετε στο πλαίσιο διεύθυνση IP, ανατρέξτε στο θέμα [προσάρτημα](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), βήμα 14.

1. Για τον τρέχοντα πόρο διεύθυνση IP, η αλλαγή του κατόχου πιθανές σε 'Υπάρχον πρωτεύον SQL Server', στο παρακάτω παράδειγμα, 'dansqlams4':

    ![Appendix13][23]

1. Για το νέο πόρο διεύθυνση IP, η αλλαγή του κατόχου πιθανές σε 'Migrated δευτερεύοντα SQL Server', στο παρακάτω παράδειγμα, 'dansqlams5':

    ![Appendix14][24]

1. Όταν έχει οριστεί μπορείτε να κάνετε ανακατεύθυνση και, όταν ο τελευταίος κόμβος τη μετεγκατάσταση των πιθανών κατόχων πρέπει να υποστεί επεξεργασία, ώστε να αυτόν τον κόμβο προστίθεται ως κάτοχος πιθανές:

    ![Appendix15][25]

## <a name="additional-resources"></a>Πρόσθετοι πόροι
- [Χώρος αποθήκευσης Azure Premium](../storage/storage-premium-storage.md)
- [Εικονικές μηχανές](https://azure.microsoft.com/services/virtual-machines/)
- [SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
