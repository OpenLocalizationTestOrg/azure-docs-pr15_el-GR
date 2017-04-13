<properties
    pageTitle="Διαχείριση ΣΠΣ χρήση της διαχείρισης πόρων και PowerShell | Microsoft Azure"
    description="Διαχείριση εικονικές μηχανές χρήση της διαχείρισης πόρων Azure και PowerShell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>Διαχείριση εικονικές μηχανές Windows Azure χρήση της διαχείρισης πόρων και PowerShell

## <a name="install-azure-powershell"></a>Εγκατάσταση του Azure PowerShell
 
Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για πληροφορίες σχετικά με την εγκατάσταση την πιο πρόσφατη έκδοση του Azure PowerShell, επιλέγοντας τη συνδρομή σας και είσοδος στο λογαριασμό σας.

## <a name="set-variables"></a>Ορισμός μεταβλητών

Όλες οι εντολές στο άρθρο απαιτούν το όνομα της ομάδας πόρων όπου βρίσκεται η εικονική μηχανή και το όνομα του υπολογιστή εικονικές για τη διαχείριση. Αντικαταστήστε την τιμή του **$rgName** με το όνομα της ομάδας πόρων που περιέχει την εικονική μηχανή. Αντικαταστήστε την τιμή του **$vmName** με το όνομα του η Εικονική. Δημιουργήστε τις μεταβλητές.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>Εμφάνιση πληροφοριών σχετικά με μια εικονική μηχανή

Λάβετε τις πληροφορίες εικονική μηχανή.
  
    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Η συνάρτηση επιστρέφει κάτι παρόμοιο με αυτό το παράδειγμα:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>Διακοπή μια εικονική μηχανή

Διακόψτε την εκτελείται εικονική μηχανή.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Ερωτηθείτε για επιβεβαίωση:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
        
Πληκτρολογήστε **Y** για να διακόψετε την εικονική μηχανή.

Μετά από μερικά λεπτά, η συνάρτηση επιστρέφει κάτι παρόμοιο με αυτό το παράδειγμα:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>Ξεκινήστε μια εικονική μηχανή

Ξεκινήστε την εικονική μηχανή εάν έχει διακοπεί.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Μετά από μερικά λεπτά, η συνάρτηση επιστρέφει κάτι παρόμοιο με αυτό το παράδειγμα:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

Εάν θέλετε να κάνετε επανεκκίνηση μια εικονική μηχανή που εκτελείται ήδη, χρήση **Επανεκκίνηση AzureRmVM** περιγράφεται Επόμενο.

## <a name="restart-a-virtual-machine"></a>Επανεκκινήστε μια εικονική μηχανή

Επανεκκινήστε την εκτελείται εικονική μηχανή.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Η συνάρτηση επιστρέφει κάτι παρόμοιο με αυτό το παράδειγμα:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>Διαγράψτε μια εικονική μηχανή

Διαγράψτε την εικονική μηχανή.  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε το **-ισχύ** παραμέτρου για να μεταβείτε γρήγορα στην ερώτηση επιβεβαίωσης.

Εάν δεν μπορείτε να χρησιμοποιήσετε την ισχύ παράμετρο-, σας ζητηθεί επιβεβαίωση:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Η συνάρτηση επιστρέφει κάτι παρόμοιο με αυτό το παράδειγμα:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>Ενημέρωση μια εικονική μηχανή

Αυτό το παράδειγμα δείχνει πώς μπορείτε να ενημερώσετε το μέγεθος του η εικονική μηχανή.
        
    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
Η συνάρτηση επιστρέφει κάτι παρόμοιο με αυτό το παράδειγμα:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK
                              
Ανατρέξτε στο θέμα [μεγέθη για εικονικές μηχανές στο Azure](virtual-machines-windows-sizes.md) για μια λίστα με τις διαθέσιμες μεγέθη για μια εικονική μηχανή.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Προσθέσετε ένα δίσκο δεδομένων σε μια εικονική μηχανή

Αυτό το παράδειγμα δείχνει πώς μπορείτε να προσθέσετε ένα δίσκο δεδομένων σε μια υπάρχουσα εικονική μηχανή.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

Ο δίσκος που προσθέτετε δεν έχει προετοιμαστεί. Για να προετοιμάσετε το δίσκο, μπορείτε να συνδέεστε σε αυτό και να χρησιμοποιήσετε τη Διαχείριση δίσκων. Εάν έχετε εγκαταστήσει WinRM και ένα πιστοποιητικό σε αυτό κατά τη δημιουργία του, μπορείτε να χρησιμοποιήσετε το απομακρυσμένο PowerShell για να προετοιμάσετε το δίσκο. Μπορείτε επίσης να χρησιμοποιήσετε μια επέκταση προσαρμοσμένη δέσμη ενεργειών: 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

Το αρχείο δέσμης ενεργειών μπορεί να περιέχει κάτι παρόμοιο με αυτόν τον κωδικό για την προετοιμασία των δίσκων:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>Επόμενα βήματα

Εάν υπάρχουν ζητήματα με μια ανάπτυξη, ενδέχεται να κοιτάξετε [αναπτύξεις ομάδα πόρων αντιμετώπιση προβλημάτων με την πύλη Azure](../resource-manager-troubleshoot-deployments-portal.md)
