<properties
    pageTitle="Δημιουργήστε και στείλτε μια Εικονική εικόνα με χρήση του Powershell | Microsoft Azure"
    description="Μάθετε πώς να δημιουργήσετε και να στείλετε μια γενική εικόνα Windows Server (VHD) χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης και Azure Powershell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Δημιουργία και αποστολή ενός VHD διακομιστή Windows Azure

Αυτό το άρθρο σας δείχνει πώς μπορείτε να αποστείλετε τη δική σας εικόνα γενικευμένη Εικονική ως εικονικό σκληρό δίσκο (VHD), ώστε να μπορείτε να το χρησιμοποιήσετε για να δημιουργήσετε εικονικές μηχανές. Για περισσότερες λεπτομέρειες σχετικά με την δίσκων και VHD στο Microsoft Azure, ανατρέξτε στο θέμα [σχετικά με δίσκων και VHD για εικονικές μηχανές](virtual-machines-linux-about-disks-vhds.md).


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. Μπορείτε επίσης να [αποστείλετε](virtual-machines-windows-upload-image.md) μια εικονική μηχανή χρησιμοποιώντας το μοντέλο από διαχειριστή πόρων. 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Σε αυτό το άρθρο προϋποθέτει ότι έχετε:

- **Συνδρομή του Azure** - Εάν δεν έχετε, μπορείτε να [ανοίξετε ένα λογαριασμό Azure δωρεάν](/pricing/free-trial/?WT.mc_id=A261C142F).

- **[Microsoft Azure PowerShell](../powershell-install-configure.md)** - έχετε τη λειτουργική μονάδα της Microsoft Azure PowerShell εγκατασταθεί και ρυθμιστεί για να χρησιμοποιήσετε τη συνδρομή σας. 

- **A . Αρχείο VHD** - υποστηριζόμενο λειτουργικό σύστημα αποθηκεύονται σε ένα αρχείο .vhd και που έχουν επισυναφθεί σε μια εικονική μηχανή των Windows. Ελέγξτε εάν τους ρόλους διακομιστή που εκτελούνται στον VHD που υποστηρίζονται από το Sysprep. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Υποστήριξη Sysprep για τους ρόλους διακομιστή](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.IMPORTANT] Η μορφή VHDX δεν υποστηρίζεται στο Microsoft Azure. Μπορείτε να μετατρέψετε το δίσκο VHD μορφή χρησιμοποιώντας το Hyper-V Manager ή το [cmdlet VHD μετατροπή](http://technet.microsoft.com/library/hh848454.aspx). Για λεπτομέρειες, ανατρέξτε στο θέμα αυτό [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Βήμα 1: Προετοιμασία VHD 

Πριν από την αποστολή του VHD Azure, πρέπει να είναι γενίκευση, χρησιμοποιώντας το εργαλείο Sysprep. Προετοιμάζει VHD θα χρησιμοποιηθεί ως εικόνα. Για λεπτομέρειες σχετικά με το Sysprep, ανατρέξτε στο θέμα [τον τρόπο χρήσης Sysprep: μια εισαγωγή](http://technet.microsoft.com/library/bb457073.aspx). Δημιουργία αντιγράφου ασφαλείας του Εικονική πριν από την εκτέλεση του Sysprep.

Από το εικονικό υπολογιστή που έχει εγκατεστημένο το λειτουργικό σύστημα για να, ολοκληρώστε την ακόλουθη διαδικασία:

1. Είσοδος στο λειτουργικό σύστημα.

2. Ανοίξτε ένα παράθυρο γραμμής εντολών ως διαχειριστής. Αλλάξτε τον κατάλογο με **%windir%\system32\sysprep**και, στη συνέχεια, εκτελέστε `sysprep.exe`.

    ![Ανοίξτε ένα παράθυρο γραμμής εντολών](./media/virtual-machines-windows-classic-createupload-vhd/sysprep_commandprompt.png)

3.  Εμφανίζεται το παράθυρο διαλόγου **Εργαλείο προετοιμασίας συστήματος** .

    ![Έναρξη Sysprep](./media/virtual-machines-windows-classic-createupload-vhd/sysprepgeneral.png)

4.  Στο **Εργαλείο προετοιμασίας συστήματος**, επιλέξτε **Εισαγωγή σύστημα εκτός του πλαισίου ΕΠΑΦΉ** και βεβαιωθείτε ότι είναι επιλεγμένο **Generalize** .

5.  Στις **Επιλογές τερματισμού**, επιλέξτε **τερματισμού**.

6.  Κάντε κλικ στο **κουμπί OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Βήμα 2: Δημιουργήστε ένα λογαριασμό του χώρου αποθήκευσης και ένα κοντέινερ

Χρειάζεστε ένα λογαριασμό χώρου αποθήκευσης στο Azure, ώστε να έχετε μια θέση για να αποστείλετε το αρχείο .vhd. Αυτό το βήμα δείχνει πώς μπορείτε να δημιουργήσετε ένα λογαριασμό ή να λάβετε τις πληροφορίες που χρειάζεστε από έναν υπάρχοντα λογαριασμό. Αντικαταστήστε τις μεταβλητές σε &lsaquo; αγκύλες &rsaquo; με τις πληροφορίες σας.

1. Σύνδεση

        Add-AzureAccount

1. Ορίστε τη συνδρομή σας στο Azure.

        Select-AzureSubscription -SubscriptionName <SubscriptionName> 

2. Δημιουργία νέου λογαριασμού χώρου αποθήκευσης. Το όνομα του λογαριασμού χώρου αποθήκευσης θα πρέπει να είναι μοναδικό, 3-24 χαρακτήρες. Το όνομα μπορεί να είναι οποιονδήποτε συνδυασμό γραμμάτων και αριθμών. Πρέπει επίσης να καθορίσετε μια θέση, όπως "Ανατολή ΜΑΣ"
        
        New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>

3. Ορισμός του νέου λογαριασμού χώρου αποθήκευσης ως προεπιλογής.
        
        Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>

4. Δημιουργήστε ένα νέο κοντέινερ.

        New-AzureStorageContainer -Name <ContainerName> -Permission Off

 

## <a name="step-3-upload-the-vhd-file"></a>Βήμα 3: Αποστολή του αρχείου .vhd

Χρησιμοποιήστε το [Πρόσθετο AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) για την αποστολή του VHD.

Από το παράθυρο Azure PowerShell που χρησιμοποιήσατε στο προηγούμενο βήμα, πληκτρολογήστε την παρακάτω εντολή και να αντικαταστήσετε τις μεταβλητές σε &lsaquo; αγκύλες &rsaquo; με τις πληροφορίες σας.

        Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>


## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Βήμα 4: Προσθέστε την εικόνα στη λίστα των προσαρμοσμένων εικόνων

Χρησιμοποιήστε το cmdlet [AzureVMImage Προσθήκη](https://msdn.microsoft.com/library/mt589167.aspx) για να προσθέσετε την εικόνα στη λίστα των προσαρμοσμένων εικόνων σας.

        Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"


## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε πλέον να [δημιουργήσετε ένα προσαρμοσμένο Εικονική](virtual-machines-windows-classic-createportal.md) χρησιμοποιώντας την εικόνα που έχετε αποστείλει.

