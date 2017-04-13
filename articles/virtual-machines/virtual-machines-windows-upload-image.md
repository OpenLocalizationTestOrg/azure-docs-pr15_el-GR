<properties
    pageTitle="Αποστολή Windows VHD για διαχείριση πόρων | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να αποστείλετε μια Windows εικονική μηχανή VHD από εσωτερικής εγκατάστασης για να Azure, χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε να αποστείλετε ένα VHD από είτε ένα γενικευμένη ή μια Εικονική εξειδικευμένες."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="upload-a-windows-vhd-from-an-on-premises-vm-to-azure"></a>Αποστολή Windows VHD από μια Εικονική εσωτερικής εγκατάστασης για να Azure 


Σε αυτό το άρθρο θα μάθετε πώς να δημιουργήσετε και να στείλετε μια εικονική σκληρού δίσκου των Windows (VHD) να χρησιμοποιηθεί με τη δημιουργία εικονική μηχανή Azure. Μπορείτε να αποστείλετε ένα VHD από μια Εικονική γενικευμένη ή μια Εικονική εξειδικευμένες. 

Για περισσότερες λεπτομέρειες σχετικά με την δίσκων και VHD στο Azure, ανατρέξτε στο θέμα [σχετικά με δίσκων και VHD για εικονικές μηχανές](virtual-machines-linux-about-disks-vhds.md).


## <a name="prepare-the-vm"></a>Προετοιμασία την εικονική Μηχανή 

Μπορείτε να αποστείλετε το γενικευμένη και εξειδικευμένες VHD στο Azure. Κάθε τύπος απαιτεί προετοιμασία η Εικονική πριν από την εκκίνηση.

- **Γενίκευση VHD** - VHD γενικευμένη είχε όλων των πληροφοριών σας προσωπικό λογαριασμό καταργηθεί χρησιμοποιώντας Sysprep. Εάν σκοπεύετε να χρησιμοποιήσετε τον VHD ως εικόνα για να δημιουργήσετε νέα ΣΠΣ από, θα πρέπει:
    - [Προετοιμασία VHD των Windows για να αποστείλετε στο Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). 
    - [Η εικονική μηχανή Generalize χρησιμοποιώντας το Sysprep](virtual-machines-windows-generalize-vhd.md). 

- **Εξειδικευμένες VHD** - VHD εξειδικευμένες διατηρεί τους λογαριασμούς χρηστών, εφαρμογών και άλλων δεδομένων κατάσταση από το αρχικό Εικονική. Εάν σκοπεύετε να χρησιμοποιήσετε τον VHD ως-είναι να δημιουργήσετε μια νέα Εικονική, βεβαιωθείτε ότι θα ολοκληρωθούν τα παρακάτω βήματα. 
    - [Προετοιμασία VHD των Windows για να αποστείλετε στο Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). **Μην κάνετε** γενίκευση η Εικονική χρησιμοποιώντας το Sysprep.
    - Καταργήστε τα εργαλεία virtualization επισκέπτη και παράγοντες που είναι εγκατεστημένα στον η Εικονική (π.χ., εργαλεία VMware).
    - Βεβαιωθείτε ότι η Εικονική έχει ρυθμιστεί ώστε να αποσπάσετε τη διεύθυνση IP και τις ρυθμίσεις DNS μέσω DHCP. Αυτό εξασφαλίζει ότι ο διακομιστής λαμβάνει μια διεύθυνση IP εντός του VNet όταν ξεκινά προς τα επάνω. 

## <a name="log-in-to-azure"></a>Συνδεθείτε στο Azure

Εάν δεν έχετε ήδη PowerShell έκδοση 1.4 ή νεότερη έκδοση εγκατεστημένο, διαβάστε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

1. Ανοίξτε το Azure PowerShell και πραγματοποιήστε είσοδο στο λογαριασμό σας Azure. Ανοίγει ένα αναδυόμενο παράθυρο για να εισαγάγετε τα διαπιστευτήριά σας λογαριασμός Azure.

    ```powershell
    Login-AzureRmAccount
    ```


2. Λάβετε τη συνδρομή αναγνωριστικών για τις συνδρομές σας διαθέσιμη.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Ορίστε τη σωστή συνδρομή χρησιμοποιώντας το αναγνωριστικό συνδρομής. Αντικατάσταση `<subscriptionID>` με το Αναγνωριστικό του τη σωστή συνδρομή.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Αποκτήστε το λογαριασμό χώρου αποθήκευσης

Χρειάζεστε ένα λογαριασμό χώρου αποθήκευσης στο Azure για να αποθηκεύσετε την εικόνα Εικονική που έχουν αποσταλεί. Μπορείτε να χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης ή να δημιουργήστε ένα νέο. 

Για να εμφανίσετε τους λογαριασμούς διαθέσιμο χώρο αποθήκευσης, πληκτρολογήστε:

```powershell
Get-AzureRmStorageAccount
```

Εάν θέλετε να χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης, προχωρήστε στην ενότητα [Αποστολή της εικόνας Εικονική](#upload-the-vm-vhd-to-your-storage-account) .

Εάν χρειάζεστε για να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης, ακολουθήστε τα παρακάτω βήματα:

1. Χρειάζεστε το όνομα της ομάδας πόρων όπου θα πρέπει να δημιουργηθεί το λογαριασμό χώρου αποθήκευσης. Για να ενημερωθείτε σχετικά με όλες τις ομάδες πόρων που βρίσκονται σε τη συνδρομή σας, πληκτρολογήστε:

    ```powershell
    Get-AzureRmResourceGroup
    ```

Για να δημιουργήσετε μια ομάδα πόρων με το όνομα **myResourceGroup** στην περιοχή **Δυτική η.π.α.** , πληκτρολογήστε:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Δημιουργία λογαριασμού χώρου αποθήκευσης με το όνομα **mystorageaccount** σε αυτή την ομάδα πόρων, χρησιμοποιώντας το cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) :

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" -SkuName "Standard_LRS" -Kind "Storage"
    ```
            
    Οι έγκυρες τιμές για - SkuName είναι οι εξής:

    - **Standard_LRS** - τοπικά πλεονάζοντα χώρο αποθήκευσης. 
    - **Standard_ZRS** - ζώνη πλεονάζοντα χώρο αποθήκευσης.
    - **Standard_GRS** - παν πλεονάζοντα χώρο αποθήκευσης. 
    - **Standard_RAGRS** - πλεονάζοντα χώρο αποθήκευσης παν πρόσβαση για ανάγνωση. 
    - **Premium_LRS** - Premium τοπικά πλεονάζοντα χώρο αποθήκευσης. 



## <a name="upload-the-vhd-to-your-storage-account"></a>Αποστολή του VHD στο λογαριασμό σας χώρου αποθήκευσης

Χρησιμοποιήστε το cmdlet [AzureRmVhd Προσθήκη](https://msdn.microsoft.com/library/mt603554.aspx) για να αποστείλετε την εικόνα σε κοντέινερ στο λογαριασμό σας στο χώρο αποθήκευσης. Αυτό το παράδειγμα στέλνει το αρχείο **myVHD.vhd** από `"C:\Users\Public\Documents\Virtual hard disks\"` με το χώρο αποθήκευσης ένα λογαριασμό με το όνομα **mystorageaccount** στην ομάδα πόρων **myResourceGroup** . Το αρχείο θα τοποθετηθούν σε κοντέινερ που ονομάζεται **mycontainer** και το νέο όνομα αρχείου θα **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Εάν είναι επιτυχής, μπορείτε να αποσπάσετε μια απάντηση που μοιάζει με αυτό:

```
  C:\> Add-AzureRmVhd -ResourceGroupName myResourceGroup -Destination https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
  MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
  MD5 hash calculation is completed.
  Elapsed time for the operation: 00:03:35
  Creating new page blob of size 53687091712...
  Elapsed time for upload: 01:12:49

  LocalFilePath           DestinationUri
  -------------           --------------
  C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Ανάλογα με τη σύνδεση δικτύου σας και το μέγεθος του αρχείου VHD, αυτή η εντολή ενδέχεται να χρειαστεί κάποιος χρόνος για την ολοκλήρωση


## <a name="next-steps"></a>Επόμενα βήματα

- [Δημιουργήστε μια Εικονική στο Azure από μια γενικευμένη VHD](virtual-machines-windows-create-vm-generalized.md)
- [Δημιουργία μια Εικονική στα Azure από έναν εξειδικευμένο VHD](virtual-machines-windows-create-vm-specialized.md) , επισυνάπτοντας ως ένα δίσκο OS όταν δημιουργείτε μια νέα Εικονική.


