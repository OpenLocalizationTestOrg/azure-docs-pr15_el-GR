<properties
    pageTitle="Καταγράψτε μια εικόνα Εικονική από γενικευμένη Εικονική Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να καταγράψετε μια εικόνα Εικονική από μια γενικευμένη Εικονική Azure που δημιουργήσατε στο μοντέλο ανάπτυξης για τη διαχείριση πόρων"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>

# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Πώς μπορείτε να καταγράψετε μια εικόνα Εικονική από μια Εικονική γενικευμένη Azure


Αυτό το άρθρο παρουσιάζει τον τρόπο χρήσης του Azure PowerShell για να δημιουργήσετε μια εικόνα από μια Εικονική γενικευμένη Azure. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε την εικόνα για να δημιουργήσετε μια άλλη Εικονική. Η εικόνα περιλαμβάνει το λειτουργικό σύστημα δίσκο και των δίσκων δεδομένων που είναι συνημμένα σε η εικονική μηχανή. Η εικόνα δεν περιλαμβάνει τους πόρους εικονικού δικτύου, ώστε να πρέπει να ρυθμίσετε αυτούς τους πόρους κατά τη δημιουργία του νέου Εικονική. 


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- Πρέπει να έχετε ήδη [γενίκευση η Εικονική](virtual-machines-windows-generalize-vhd.md). Γενίκευση μια Εικονική καταργεί όλα προσωπικών πληροφοριών του λογαριασμού σας, μεταξύ άλλων, και προετοιμάζει τον υπολογιστή θα χρησιμοποιηθεί ως εικόνα.

- Πρέπει να έχετε την έκδοση του PowerShell Azure 1.0.x ή νεότερη έκδοση. Εάν δεν έχετε εγκαταστήσει ήδη PowerShell, διαβάστε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για τα βήματα εγκατάστασης.


## <a name="log-in-to-azure-powershell"></a>Συνδεθείτε στο Azure PowerShell

1. Ανοίξτε το Azure PowerShell και πραγματοποιήστε είσοδο στο λογαριασμό σας Azure.

    ```powershell
    Login-AzureRmAccount
    ```

    Ανοίγει ένα αναδυόμενο παράθυρο για να εισαγάγετε τα διαπιστευτήριά σας λογαριασμός Azure.

2. Λάβετε τη συνδρομή αναγνωριστικών για τις συνδρομές σας διαθέσιμη.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Ορίστε τη σωστή συνδρομή χρησιμοποιώντας το αναγνωριστικό συνδρομής.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Εκχώρηση η Εικονική και να ορίσετε την κατάσταση για γενίκευση       

1. Κατάργηση εκχώρησης πόρων Εικονική.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    Η *κατάσταση* για την εικονική Μηχανή στην πύλη του Azure αλλάζει από **Διακοπή** **Διακοπή (Κατάργηση εκχώρησης)**.

2. Ορίστε την κατάσταση της η εικονική μηχανή σε **Generalized**. 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```

3. Έλεγχος της κατάστασης του Εικονική. Στην ενότητα **OSState/γενίκευση** για την εικονική Μηχανή θα πρέπει να έχετε το **DisplayStatus** οριστεί σε **Εικονική γενίκευση**.  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Δημιουργία της εικόνας 

1. Αντιγράψτε την εικόνα εικονική μηχανή στο κοντέινερ χώρου αποθήκευσης προορισμού χρήση αυτής της εντολής. Η εικόνα έχει δημιουργηθεί στον ίδιο λογαριασμό χώρου αποθήκευσης ως η αρχική εικονική μηχανή. Το `-Path` παραμέτρου αποθηκεύει ένα αντίγραφο του προτύπου JSON τοπικά. Το `-DestinationContainerName` παραμέτρου είναι το όνομα του κοντέινερ που θέλετε να περιέχει τις εικόνες σας. Εάν δεν υπάρχει το κοντέινερ, δημιουργείται για εσάς.

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    Μπορείτε να λάβετε τη διεύθυνση URL της εικόνας σας από το πρότυπο αρχείου JSON. Επιλέξτε τους **πόρους** > **storageProfile** > **osDisk** > **εικόνα** > **uri** στην ενότητα για την πλήρη διαδρομή της εικόνας σας. Η διεύθυνση URL της εικόνας που μοιάζει με: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
    
    Μπορείτε επίσης να επαληθεύσετε το URI στην πύλη. Η εικόνα αντιγράφεται σε ένα κοντέινερ που ονομάζεται **σύστημα** στο λογαριασμό σας στο χώρο αποθήκευσης. 


## <a name="next-steps"></a>Επόμενα βήματα

- Τώρα μπορείτε να [δημιουργήσετε μια Εικονική από την εικόνα](virtual-machines-windows-create-vm-generalized.md).

