<properties 
   pageTitle="Συνήθεις εντολές του PowerShell για ΣΠΣ | Microsoft Azure"
   description="Συνήθεις εντολές του PowerShell για να ξεκινήσετε τη δημιουργία και διαχείριση του ΣΠΣ στο Azure στα Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="09/27/2016"
   ms.author="davidmu" />

# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Συνήθεις εντολές του PowerShell για τη δημιουργία και διαχείριση ΣΠΣ

Σε αυτό το άρθρο περιγράφει ορισμένες από τις εντολές του PowerShell Azure που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε και να διαχειριστείτε εικονικές μηχανές στο Azure τη συνδρομή σας.  Για πιο αναλυτική Βοήθεια με συγκεκριμένες διακόπτες γραμμής εντολών και επιλογές, μπορείτε να χρησιμοποιήσετε **Λήψη Βοήθειας** *εντολή*.

Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για πληροφορίες σχετικά με την εγκατάσταση την πιο πρόσφατη έκδοση του Azure PowerShell, επιλέγοντας τη συνδρομή σας και είσοδος στο λογαριασμό σας.

## <a name="create-a-vm"></a>Δημιουργήστε μια εικονική Μηχανή

Εργασία | Εντολή
-------------- | -------------------------
Δημιουργήστε μια Εικονική ρύθμιση παραμέτρων | $vm = [Δημιουργία AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) - VMName "vm_name" - VMSize "vm_size"<BR></BR><BR></BR>Η ρύθμιση παραμέτρων Εικονική χρησιμοποιείται για να ορίσετε ή ενημέρωση των ρυθμίσεων για την εικονική Μηχανή. Η ρύθμιση παραμέτρων έχει προετοιμαστεί με το όνομα του η Εικονική και το [μέγεθος](virtual-machines-windows-sizes.md).
Προσθήκη ρυθμίσεων παραμέτρων | $vm = [Σύνολο AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) - Εικονική $vm-Windows - όνομα υπολογιστή "όνομα_υπολογιστή"-διαπιστευτηρίων $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Ρυθμίσεις του λειτουργικού συστήματος όπως [τα διαπιστευτήρια](https://technet.microsoft.com/library/hh849815.aspx) προστίθεται το αντικείμενο ρύθμισης παραμέτρων που δημιουργήσατε προηγουμένως με νέα AzureRmVMConfig.
Προσθέστε ένα περιβάλλον εργασίας δικτύου | $vm = [Προσθήκη AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) - Εικονική $vm-αναγνωριστικό $nic. Αναγνωριστικό<BR></BR><BR></BR>Μια Εικονική πρέπει να έχετε μια [διασύνδεση δικτύου](virtual-machines-windows-ps-create.md) για την επικοινωνία σε ένα εικονικό δίκτυο. Μπορείτε επίσης να χρησιμοποιήσετε [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) για την ανάκτηση ενός υπάρχοντος αντικειμένου περιβάλλοντος εργασίας δικτύου.
Καθορίστε μια εικόνα πλατφόρμα | $vm = - PublisherName - Εικονική $vm [Σύνολο AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) "publisher_name"-προσφέρουν "publisher_offer" - τα SKU "product_sku"-"το πιο πρόσφατο" έκδοση<BR></BR><BR></BR>[Πληροφορίες εικόνας](virtual-machines-windows-cli-ps-findimage.md) προστίθεται το αντικείμενο ρύθμισης παραμέτρων που δημιουργήσατε προηγουμένως με νέα AzureRmVMConfig. Το αντικείμενο που επιστρέφεται από αυτή την εντολή χρησιμοποιείται μόνο κατά τη ρύθμιση του δίσκου λειτουργικό σύστημα για να χρησιμοποιήσετε μια εικόνα πλατφόρμας.
Ορισμός δίσκο λειτουργικό σύστημα για να χρησιμοποιήσετε μια εικόνα πλατφόρμα | $vm = [Σύνολο AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) - Εικονική $vm-http://mystore1.blob.core.windows.net/vhds/disk_name.vhd"όνομα"disk_name"- VhdUri" - CreateOption FromImage<BR></BR><BR></BR>Το όνομα του δίσκου λειτουργικό σύστημα και τη θέση του στο [χώρο αποθήκευσης](../storage/storage-powershell-guide-full.md) προστίθεται το αντικείμενο ρύθμισης παραμέτρων που δημιουργήσατε προηγουμένως.
Ορισμός OS δίσκου για να χρησιμοποιήσετε μια εικόνα γενικευμένη | $vm = σύνολο AzureRmVMOSDisk - Εικονική $vm-https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk"όνομα"disk_name"- SourceImageUri. {guid} .vhd"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage-Windows<BR></BR><BR></BR>Το όνομα του δίσκου λειτουργικό σύστημα, τη θέση της εικόνας προέλευσης και του δίσκου θέση στο [χώρο αποθήκευσης](../storage/storage-powershell-guide-full.md) προστίθεται το αντικείμενο ρύθμισης παραμέτρων.
Ορισμός OS δίσκου για να χρησιμοποιήσετε μια εικόνα εξειδικευμένες | $vm = σύνολο AzureRmVMOSDisk - Εικονική $vm-http://mystore1.blob.core.windows.net/vhds/"όνομα"name_of_disk"- VhdUri" Επισύναψη - CreateOption - Windows
Δημιουργήστε μια εικονική Μηχανή | [Δημιουργία AzureRmVM]() - ResourceGroupName "resource_group_name"-θέση "location_name" - Εικονική $vm<BR></BR><BR></BR>Όλοι οι πόροι δημιουργούνται σε μια [ομάδα πόρων](../powershell-azure-resource-manager.md). Πρέπει να δημιουργηθεί η Εικονική στην ίδια [θέση](https://msdn.microsoft.com/library/azure/dn495177.aspx) με την ομάδα των πόρων. Πριν από την εκτέλεση αυτής της εντολής, εκτελέστε δημιουργία AzureRmVMConfig, ορισμός AzureRmVMOperatingSystem, σύνολο AzureRmVMSourceImage, προσθήκη AzureRmVMNetworkInterface και σύνολο AzureRmVMOSDisk.

## <a name="get-information-about-vms"></a>Λήψη πληροφοριών σχετικά με ΣΠΣ

Εργασία | Εντολή
-------------- | -------------------------
Λίστα ΣΠΣ σε μια συνδρομή| [Get-AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx)
Λίστα ΣΠΣ σε μια ομάδα πόρων | Get-AzureRmVM - ResourceGroupName "resource_group_name"<BR></BR><BR></BR>Για να λάβετε μια λίστα με τις ομάδες πόρων στη συνδρομή σας, χρησιμοποιήστε [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx).
Λήψη πληροφοριών σχετικά με μια εικονική Μηχανή | Get-AzureRmVM - ResourceGroupName "resource_group_name"-όνομα "vm_name"

## <a name="manage-vms"></a>Διαχείριση VM

Εργασία | Εντολή
-------------- | -------------------------
Ξεκινήστε μια εικονική Μηχανή | [Έναρξη-AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) - ResourceGroupName "resource_group_name"-όνομα "vm_name"
Διακοπή μια εικονική Μηχανή | [Διακοπή AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) - ResourceGroupName "resource_group_name"-όνομα "vm_name"
Επανεκκινήστε μια Εικονική εκτελείται | [Επανεκκίνηση-AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) - ResourceGroupName "resource_group_name"-όνομα "vm_name"
Διαγράψτε μια εικονική Μηχανή | [Κατάργηση AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) - ResourceGroupName "resource_group_name"-όνομα "vm_name"
Γενίκευση μια εικονική Μηχανή | [Ορισμός AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) - ResourceGroupName YourResourceGroup-όνομα "vm_name"-γενίκευση<BR></BR><BR></BR>Εκτέλεση αυτής της εντολής πριν από την εκτέλεση AzureRmVMImage αποθήκευση.
Καταγράψτε μια εικονική Μηχανή | [Αποθήκευση AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) - ResourceGroupName "resource_group_name" - VMName "vm_name" - DestinationContainerName "image_container" - VHDNamePrefix "image_name_prefix"-διαδρομή "C:\filepath\filename.json"<BR></BR><BR></BR>Μια εικονική μηχανή πρέπει να [τερματίσετε τη γενίκευση και](virtual-machines-windows-generalize-vhd.md) να χρησιμοποιηθεί για να δημιουργήσετε μια εικόνα. Πριν από την εκτέλεση αυτής της εντολής, εκτελέστε το σύνολο AzureRmVm.
Ενημέρωση μια εικονική Μηχανή | [Ενημέρωση AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) - ResourceGroupName "resource_group_name" - Εικονική $vm<BR></BR><BR></BR>Λάβετε την τρέχουσα ρύθμιση παραμέτρων Εικονική χρησιμοποιώντας Get-AzureRmVM, αλλάξτε τις ρυθμίσεις παραμέτρων από το αντικείμενο Εικονική και, στη συνέχεια, να εκτελέσετε αυτήν την εντολή.
Προσθέσετε ένα δίσκο δεδομένων σε μια εικονική Μηχανή | [Προσθήκη AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) - Εικονική $vm-https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"όνομα"disk_name"- VhdUri" - LUN #-σε cache ReadWrite - DiskSizeinGB # - CreateOption είναι κενή<BR></BR><BR></BR>Χρησιμοποιήστε Get-AzureRmVM για να λάβετε το αντικείμενο Εικονική. Καθορίστε τον αριθμό LUN και το μέγεθος του δίσκου. Εκτελέστε ενημέρωση-AzureRmVM για να εφαρμόσετε τις αλλαγές ρύθμισης παραμέτρων του Εικονική. Ο δίσκος που προσθέτετε δεν έχει προετοιμαστεί. Για πληροφορίες σχετικά με την προετοιμασία δίσκων που έχουν προστεθεί, ανατρέξτε στο θέμα [Διαχείριση εικονικές μηχανές Windows Azure χρήση της διαχείρισης πόρων και PowerShell](virtual-machines-windows-ps-manage.md).
Καταργήστε ένα δίσκο δεδομένων από μια εικονική Μηχανή | [Κατάργηση AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) - Εικονική $vm-όνομα "disk_name"<BR></BR><BR></BR>Χρησιμοποιήστε Get-AzureRmVM για να λάβετε το αντικείμενο Εικονική. Εκτελέστε Update-AzureRmVM για να εφαρμόσετε τις αλλαγές ρύθμισης παραμέτρων του Εικονική.
Προσθήκη μια επέκταση σε μια εικονική Μηχανή | [Ορισμός AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) - ResourceGroupName "resource_group_name"-vm_name"θέση"azure_location"- VMName"-όνομα "extension_name"-Publisher "publisher_name"-τύπου "extension_type" - TypeHandlerVersion "#. #"-Ρυθμίσεις $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Εκτέλεση αυτής της εντολής με τις κατάλληλες [πληροφορίες ρύθμισης παραμέτρων](virtual-machines-windows-extensions-configuration-samples.md) για την επέκταση που θέλετε να εγκαταστήσετε.
Καταργήστε μια Εικονική επέκταση | [Κατάργηση AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) - ResourceGroupName "resource_group_name"-vm_name"όνομα"extension_name"- VMName"

## <a name="next-steps"></a>Επόμενα βήματα

- Δείτε τα βασικά βήματα για να δημιουργήσετε μια εικονική μηχανή στη [Δημιουργία μια Εικονική Windows χρήση της διαχείρισης πόρων και PowerShell](virtual-machines-windows-ps-create.md).

