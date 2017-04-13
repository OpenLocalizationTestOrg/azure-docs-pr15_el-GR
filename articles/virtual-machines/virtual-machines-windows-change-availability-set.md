<properties
    pageTitle="Αλλαγή ενός συνόλου διαθεσιμότητα ΣΠΣ | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να αλλάξετε τη διαθεσιμότητα για τις εικονικές μηχανές με χρήση του Azure PowerShell και το μοντέλο ανάπτυξης διαχείρισης πόρων."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="drewm"/>



# <a name="change-the-availability-set-for-a-windows-vm"></a>Αλλάξτε τη ρύθμιση για μια Εικονική Windows διαθεσιμότητα

Ακολουθήστε τα παρακάτω βήματα περιγράφουν πώς μπορείτε να αλλάξετε το σύνολο διαθεσιμότητα μια Εικονική με χρήση του PowerShell Azure. Μια Εικονική μόνο μπορούν να προστεθούν σε μια ρύθμιση κατά τη δημιουργία διαθεσιμότητα. Για να αλλάξετε τη διαθεσιμότητα ρύθμιση, πρέπει να διαγράψετε και να δημιουργήσετε ξανά την εικονική μηχανή. 

## <a name="change-the-availability-set-using-powershell"></a>Αλλάξτε τη διαθεσιμότητα συνόλου με χρήση του PowerShell

1. Καταγράψτε τις ακόλουθες σημαντικές λεπτομέρειες από την εικονική Μηχανή να τροποποιηθεί.

    Όνομα της την εικονική Μηχανή
    
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
 
    Εικονική μέγεθος
    
    ```powershell
    $vm.HardwareProfile.VmSize
    ```

    Περιβάλλον εργασίας κύριο δίκτυο δικτύου και διασυνδέσεις προαιρετικό δικτύου Εάν υπάρχουν στην την εικονική Μηχανή
    
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```

    Λειτουργικό σύστημα δίσκου προφίλ

    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```

    Προφίλ δίσκου για κάθε δίσκο δεδομένων 
    
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```

    Εικονική επεκτάσεις εγκατεστημένο 
    
    ```powershell
    $vm.Extensions
    ```

2. Διαγράψτε την εικονική Μηχανή χωρίς να διαγράψετε οποιαδήποτε δίσκων ή τις διασυνδέσεις δικτύου.

    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```

3. Δημιουργήστε τη διαθεσιμότητα ορίσετε εάν δεν υπάρχει ήδη

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```

4. Δημιουργήστε ξανά την εικονική Μηχανή χρησιμοποιώντας το νέο σύνολο διαθεσιμότητα

    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>

    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]

    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 

    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 

5. Προσθήκη δεδομένων δίσκων και επεκτάσεις. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισύναψη δίσκου δεδομένων σε Εικονική](virtual-machines-windows-attach-disk-portal.md) και [Δείγματα ρύθμισης παραμέτρων επέκτασης](virtual-machines-windows-extensions-configuration-samples.md). Για να την εικονική Μηχανή με τη χρήση του PowerShell ή Azure CLI, μπορούν να προστεθούν δίσκων δεδομένων και επεκτάσεις.

## <a name="example-script"></a>Παράδειγμα δέσμης ενεργειών

Η ακόλουθη δέσμη ενεργειών παρέχει ένα παράδειγμα τη συγκέντρωση τις απαιτούμενες πληροφορίες, διαγράφοντας το αρχικό Εικονική και, στη συνέχεια, να το δημιουργείτε σε ένα νέο σύνολο διαθεσιμότητα.

```powershell
    #set variables
    $rg = "demo-resource-group"
    $vmName = "demo-vm"
    $newAvailSetName = "demo-as"
    $outFile = "C:\temp\outfile.txt"

    #Get VM Details
    $OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

    #Output VM details to file
    "VM Name: " | Out-File -FilePath $outFile 
    $OriginalVM.Name | Out-File -FilePath $outFile -Append

    "Extensions: " | Out-File -FilePath $outFile -Append
    $OriginalVM.Extensions | Out-File -FilePath $outFile -Append

    "VMSize: " | Out-File -FilePath $outFile -Append
    $OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

    "NIC: " | Out-File -FilePath $outFile -Append
    $OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

    "OSType: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

    "OS Disk: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

    if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
    }

    #Remove the original VM
    Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

    #Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
    if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
    }

    #Create the basic configuration for the replacement VM
    $newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
    Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

    #Add Data Disks
    foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
    }

    #Add NIC(s)
    foreach ($nic in $OriginalVM.NetworkInterfaceIDs) {
        Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic
    }

    #Create the VM
    New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## <a name="next-steps"></a>Επόμενα βήματα

Προσθέστε επιπλέον χώρο αποθήκευσης για να σας Εικονική, προσθέτοντας ένα πρόσθετο [δίσκο δεδομένων](virtual-machines-windows-attach-disk-portal.md).

