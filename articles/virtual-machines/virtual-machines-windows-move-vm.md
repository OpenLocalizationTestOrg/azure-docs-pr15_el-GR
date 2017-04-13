<properties
    pageTitle="Μετακίνηση Windows Εικονική | Microsoft Azure"
    description="Μετακίνηση μια Εικονική Windows άλλο Azure συνδρομή ή την ομάδα πόρων στο μοντέλο ανάπτυξης διαχείρισης πόρων."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Μετακίνηση μια Εικονική των Windows σε μια άλλη Azure συνδρομή ή την ομάδα πόρων 

Σε αυτό το άρθρο σάς καθοδηγεί σε πώς μπορείτε να μετακινήσετε μια Εικονική Windows μεταξύ των ομάδων πόρων ή συνδρομών. Μετακίνηση μεταξύ συνδρομών μπορεί να είναι χρήσιμες, εάν δημιουργήσατε μια Εικονική αρχικά σε μια συνδρομή για προσωπική χρήση και τώρα θέλετε να μετακινήσετε σε συνδρομής της εταιρείας σας για να συνεχίσετε την εργασία σας.

> [AZURE.NOTE] Νέα αναγνωριστικά πόρου θα δημιουργηθεί ως μέρος της μετακίνησης. Όταν η Εικονική έχει μετακινηθεί, θα πρέπει να ενημερώσετε το εργαλεία και χρησιμοποιήστε το νέο πόρο αναγνωριστικών των δεσμών ενεργειών. 


[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]


## <a name="use-powershell-to-move-a-vm"></a>Χρήση του Powershell για να μετακινήσετε μια εικονική Μηχανή

Για να μετακινήσετε μια εικονική μηχανή σε μια άλλη ομάδα πόρων, πρέπει να βεβαιωθείτε ότι μπορείτε επίσης να μετακινήσετε όλους τους πόρους εξαρτώμενα. Για να χρησιμοποιήσετε το cmdlet μετακίνηση AzureRMResource, χρειάζεστε το όνομα του πόρου και τον τύπο του πόρου. Μπορείτε να λάβετε και τα δύο από τα cmdlet Εύρεση AzureRMResource.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"
    

Για να μετακινήσετε μια Εικονική χρειαζόμαστε για να μετακινήσετε πολλούς πόρους. Να δημιουργήσετε απλώς ξεχωριστές μεταβλητές για κάθε πόρο και, στη συνέχεια, τις στη λίστα. Αυτό το παράδειγμα περιλαμβάνει περισσότερους πόρους βασικές για μια Εικονική, αλλά μπορείτε να προσθέσετε περισσότερα όπως απαιτείται.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"
    
    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"
    
    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Για να μετακινήσετε τους πόρους σε διαφορετική συνδρομή, συμπεριλάβετε την παράμετρο **- DestinationSubscriptionId** . 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Θα σας ζητηθεί να επιβεβαιώσετε ότι θέλετε να μετακινήσετε των καθορισμένων πόρων. Πληκτρολογήστε **Y** για να επιβεβαιώσετε ότι θέλετε να μετακινήσετε τους πόρους.

  
## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να μετακινήσετε πολλούς διαφορετικούς τύπους πόρων μεταξύ των ομάδων πόρων και συνδρομών. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Μετακίνηση πόρων για νέα ομάδα πόρων ή τη συνδρομή](../resource-group-move-resources.md).    