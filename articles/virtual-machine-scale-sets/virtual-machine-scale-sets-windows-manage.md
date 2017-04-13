<properties
    pageTitle="Διαχείριση ΣΠΣ σε ένα σύνολο κλίμακα εικονική μηχανή | Microsoft Azure"
    description="Διαχείριση εικονικές μηχανές σε μια εικονική μηχανή κλίμακα συνόλου με χρήση του PowerShell Azure."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Διαχείριση εικονικές μηχανές σε ένα σύνολο κλίμακα εικονική μηχανή

Χρησιμοποιήστε τις εργασίες σε αυτό το άρθρο για να διαχειριστείτε εικονικές μηχανές στο σύνολο σας εικονική μηχανή κλίμακα.

Περισσότερες από τις εργασίες που αφορούν τη διαχείριση μια εικονική μηχανή σε ένα σύνολο κλίμακα απαιτούν ότι γνωρίζετε το Αναγνωριστικό εμφάνισης του υπολογιστή που θέλετε να διαχειριστείτε. Μπορείτε να χρησιμοποιήσετε [Azure Explorer πόρων](https://resources.azure.com) για να βρείτε το Αναγνωριστικό εμφάνισης της μια εικονική μηχανή σε ένα σύνολο κλίμακα. Μπορείτε επίσης να χρησιμοποιήσετε Εξερεύνηση των πόρων για να επιβεβαιώσετε την κατάσταση των εργασιών που ολοκληρώσετε.

Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για πληροφορίες σχετικά με την εγκατάσταση την πιο πρόσφατη έκδοση του Azure PowerShell, επιλέγοντας τη συνδρομή σας και είσοδος στο λογαριασμό σας.

## <a name="display-information-about-a-scale-set"></a>Εμφάνιση πληροφοριών σχετικά με ένα σύνολο κλίμακας

Μπορείτε να λάβετε γενικές πληροφορίες σχετικά με ένα σύνολο κλίμακα, το οποίο είναι επίσης γνωστή ως προβολή παρουσίας. Εναλλακτικά, μπορείτε να λάβετε πιο συγκεκριμένες πληροφορίες, όπως οι πληροφορίες σχετικά με τους πόρους του συνόλου κλίμακα.

Αντικαταστήστε τις τιμές εισαγωγικά με το όνομα ή την ομάδα πόρων και την κλίμακα των ρύθμιση και, στη συνέχεια, εκτελέστε την εντολή:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Η συνάρτηση επιστρέφει κάπως έτσι:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded
    
Αντικαταστήστε τις τιμές εισαγωγικά με το όνομα του συνόλου ομάδας και την κλίμακα του πόρου. Αντικατάσταση *#* με το αναγνωριστικό παρουσίας του η εικονική μηχανή που θέλετε να λάβετε πληροφορίες και, στη συνέχεια, εκτελέστε το:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
        
Η συνάρτηση επιστρέφει κάτι παρόμοιο με αυτό το παράδειγμα:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded
        
## <a name="start-a-virtual-machine-in-a-scale-set"></a>Ξεκινήστε μια εικονική μηχανή σε ένα σύνολο κλίμακας

Αντικαταστήστε τις τιμές εισαγωγικά με το όνομα του συνόλου ομάδας και την κλίμακα του πόρου. Αντικατάσταση *#* με το αναγνωριστικό του η εικονική μηχανή που θέλετε να ξεκινήσετε και, στη συνέχεια, εκτελέστε το:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Στην Εξερεύνηση των πόρων, μπορούμε να δούμε ότι η κατάσταση της παρουσίας είναι **εκτελείται**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

Μπορείτε να ξεκινήσετε όλες τις εικονικές μηχανές στην κλίμακα ρύθμιση χωρίς να χρησιμοποιήσετε την παράμετρο - αναγνωριστικό εμφάνισης.
    
## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Διακοπή μια εικονική μηχανή σε ένα σύνολο κλίμακας

Αντικαταστήστε τις τιμές εισαγωγικά με το όνομα του συνόλου ομάδας και την κλίμακα του πόρου. Αντικατάσταση *#* με το αναγνωριστικό του η εικονική μηχανή που θέλετε να διακόψετε και, στη συνέχεια, εκτελέστε το:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Στην Εξερεύνηση των πόρων, μπορούμε να δούμε ότι η κατάσταση της παρουσίας είναι **Κατάργηση εκχώρησης**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]
    
Για να διακόψετε μια εικονική μηχανή και να μην καταργήσετε την εκχώρηση του, χρησιμοποιήστε την παράμετρο - StayProvisioned. Μπορείτε να διακόψετε όλες τις εικονικές μηχανές του συνόλου χωρίς να χρησιμοποιήσετε την παράμετρο - αναγνωριστικό εμφάνισης.
    
## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Επανεκκινήστε μια εικονική μηχανή σε ένα σύνολο κλίμακας

Αντικαταστήστε τις τιμές εισαγωγικά με το όνομα της ομάδας σας πόρων και το σύνολο κλίμακα. Αντικατάσταση *#* με το αναγνωριστικό του η εικονική μηχανή που θέλετε να κάνετε επανεκκίνηση και, στη συνέχεια, εκτελέστε το:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
    
Μπορείτε να κάνετε επανεκκίνηση σε όλες τις εικονικές μηχανές του συνόλου χωρίς να χρησιμοποιήσετε την παράμετρο - αναγνωριστικό εμφάνισης.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Καταργήστε μια εικονική μηχανή από ένα σύνολο κλίμακας

Αντικαταστήστε τις τιμές εισαγωγικά με το όνομα της ομάδας σας πόρων και το σύνολο κλίμακα. Αντικατάσταση *#* με το αναγνωριστικό του η εικονική μηχανή που θέλετε να καταργήσετε και, στη συνέχεια, εκτελέστε το:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Μπορείτε να καταργήσετε όλα ταυτόχρονα το σύνολο κλίμακα εικονική μηχανή χωρίς να χρησιμοποιήσετε την παράμετρο - αναγνωριστικό εμφάνισης.

## <a name="change-the-capacity-of-a-scale-set"></a>Αλλαγή η δυναμικότητα των ένα σύνολο κλίμακας

Μπορείτε να προσθέσετε ή να καταργήσετε εικονικές μηχανές, αλλάζοντας την ικανότητα του συνόλου. Λάβετε το σύνολο κλίμακας που θέλετε να αλλάξετε, ορίστε την ικανότητα με αυτό που θέλετε να είναι και, στη συνέχεια, ενημερώστε την κλίμακα που έχουν οριστεί με τη νέα δυνατότητα. Σε αυτές τις εντολές, αντικαταστήστε τις τιμές εισαγωγικά με το όνομα της ομάδας σας πόρων και το σύνολο κλίμακα.

  $vmss = get-AzureRmVmss - ResourceGroupName "ομάδα όνομα πόρου" - VMScaleSetName "όνομα σύνολο κλίμακα" $vmss.sku.capacity = 5 ενημέρωση AzureRmVmss - ResourceGroupName "ομάδα όνομα πόρου"-όνομα "κλίμακα όνομα συνόλου" - VirtualMachineScaleSet $vmss 

Εάν πρόκειται να καταργήσετε εικονικές μηχανές από το σύνολο κλίμακα, τις εικονικές μηχανές με την υψηλότερη αναγνωριστικά καταργούνται πρώτα.
