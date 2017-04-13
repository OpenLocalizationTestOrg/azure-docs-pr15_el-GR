<properties
    pageTitle="Δημιουργήστε μια Εικονική με ένα πρότυπο από διαχειριστή πόρων | Microsoft Azure"
    description="Χρήση ενός προτύπου για τη διαχείριση πόρων και του PowerShell για να δημιουργήσετε εύκολα μια νέα εικονική μηχανή των Windows."
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
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Δημιουργήστε μια εικονική μηχανή Windows με ένα πρότυπο από διαχειριστή πόρων

Σε αυτό το άρθρο σάς παρουσιάζει ένα πρότυπο από διαχειριστή πόρων Azure και σας δείχνει πώς μπορείτε να χρησιμοποιήσετε PowerShell για να το αναπτύξετε. Το πρότυπο αναπτύσσει έναν εικονικές υπολογιστή με Windows Server σε ένα νέο εικονικό δίκτυο με ένα μόνο υποδίκτυο.

Θα πρέπει να χρειαστεί περίπου 20 λεπτά για να εκτελέσετε τα βήματα σε αυτό το άρθρο.

> [AZURE.IMPORTANT] Εάν θέλετε το Εικονική να αποτελεί τμήμα ενός συνόλου διαθεσιμότητα, προσθέσετε στο σύνολο κατά τη δημιουργία του Εικονική. Αυτήν τη στιγμή δεν υπάρχει ένας τρόπος για να προσθέσετε μια Εικονική διαθεσιμότητα Ορισμός αφού έχει δημιουργηθεί.

## <a name="step-1-create-the-template-file"></a>Βήμα 1: Δημιουργήστε το αρχείο προτύπου

Μπορείτε να δημιουργήσετε το δικό σας πρότυπο χρησιμοποιώντας τις πληροφορίες που βρίσκονται σε [σύνταξη από κοινού από διαχειριστή πόρων Azure πρότυπα](../resource-group-authoring-templates.md). Μπορείτε επίσης να αναπτύξετε τα πρότυπα που έχουν δημιουργηθεί για εσάς από τα [Πρότυπα γρήγορες εκκινήσεις Azure](https://azure.microsoft.com/documentation/templates/).

1. Ανοίξτε το πρόγραμμα επεξεργασίας κειμένου Αγαπημένα και προσθέστε το στοιχείο απαιτείται σχήματος και το στοιχείο απαιτείται contentVersion:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Παράμετροι](../resource-group-authoring-templates.md#parameters) δεν είναι πάντα απαραίτητα, αλλά παρέχουν ένας τρόπος για να εισαγάγετε τιμές, όταν έχει αναπτυχθεί το πρότυπο. Προσθέστε το στοιχείο παράμετροι και τα θυγατρικά στοιχεία μετά το στοιχείο contentVersion:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. [Μεταβλητές](../resource-group-authoring-templates.md#variables) μπορούν να χρησιμοποιηθούν σε ένα πρότυπο για να καθορίσετε τιμές που μπορεί να αλλάζουν συχνά ή που πρέπει να δημιουργηθεί από ένα συνδυασμό των τιμών της παραμέτρου. Προσθέστε το στοιχείο μεταβλητές μετά την ενότητα παράμετροι:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
        
4. [Πόροι](../resource-group-authoring-templates.md#resources) όπως η εικονική μηχανή, το εικονικό δίκτυο και το λογαριασμό χώρου αποθήκευσης ορίζονται δίπλα στο πρότυπο. Προσθέστε την ενότητα "Πόροι" μετά από την ενότητα μεταβλητές:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvn1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
          
    >[AZURE.NOTE] Σε αυτό το άρθρο δημιουργεί μια εικονική μηχανή εκτελεί μια έκδοση του λειτουργικού συστήματος των Windows Server. Για να μάθετε περισσότερα σχετικά με την επιλογή άλλες εικόνες, ανατρέξτε στο θέμα [Περιήγηση και επιλογή Azure εικονική μηχανή εικόνων με το Windows PowerShell και το Azure CLI](virtual-machines-linux-cli-ps-findimage.md).  
            
2. Αποθηκεύστε το αρχείο προτύπου ως *VirtualMachineTemplate.json*.

## <a name="step-2-create-the-parameters-file"></a>Βήμα 2: Δημιουργία του αρχείου παραμέτρων

Για να καθορίσετε τιμές για τις παραμέτρους του πόρου που έχουν οριστεί στο πρότυπο, δημιουργήστε ένα αρχείο παραμέτρων που περιέχει τις τιμές που χρησιμοποιούνται όταν έχει αναπτυχθεί το πρότυπο.

1. Στο πρόγραμμα επεξεργασίας κειμένου, αντιγράψτε αυτό το περιεχόμενο JSON σε ένα νέο αρχείο που ονομάζεται *Parameters.json*:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Δείτε περισσότερες πληροφορίες σχετικά με [τις απαιτήσεις όνομα χρήστη και τον κωδικό πρόσβασης](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. Αποθηκεύστε το αρχείο παραμέτρων.

## <a name="step-3-install-azure-powershell"></a>Βήμα 3: Εγκατάσταση του Azure PowerShell

Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για πληροφορίες σχετικά με την εγκατάσταση την πιο πρόσφατη έκδοση του Azure PowerShell, επιλέγοντας τη συνδρομή σας και είσοδος στο λογαριασμό σας.

## <a name="step-4-create-a-resource-group"></a>Βήμα 4: Δημιουργία μιας ομάδας πόρων

Όλοι οι πόροι πρέπει να αναπτυχθεί σε μια [ομάδα πόρων](../azure-resource-manager/resource-group-overview.md).

1. Λάβετε μια λίστα με τις διαθέσιμες θέσεις όπου μπορεί να δημιουργηθεί πόρους.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. Αντικαταστήστε την τιμή του **$locName** με μια θέση από τη λίστα, για παράδειγμα **Κεντρικές ΗΠΑ**. Δημιουργήστε τη μεταβλητή.

        $locName = "location name"
        
3. Αντικαταστήστε την τιμή του **$rgName** με το όνομα της νέας ομάδας πόρων. Δημιουργήστε τη μεταβλητή και την ομάδα των πόρων.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Θα πρέπει να δείτε κάτι παρόμοιο με αυτό το παράδειγμα:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5-create-the-resources-with-the-template-and-parameters"></a>Βήμα 5: Δημιουργία τους πόρους με το πρότυπο και τις παραμέτρους

Αντικαταστήστε την τιμή του **$templateFile** με τη διαδρομή και το όνομα του αρχείου του προτύπου. Αντικαταστήστε την τιμή του **$parameterFile** με τη διαδρομή και το όνομα του αρχείου παραμέτρους. Δημιουργία των μεταβλητών και, στη συνέχεια, αναπτύξτε το πρότυπο. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] Μπορείτε επίσης να αναπτύξετε τα πρότυπα και τις παραμέτρους από ένα λογαριασμό Azure χώρου αποθήκευσης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με το χώρο αποθήκευσης Azure](../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Επόμενα βήματα

- Εάν υπάρχουν προβλήματα με την ανάπτυξη, ένα επόμενο βήμα σας είναι να κοιτάξετε [αναπτύξεις ομάδα πόρων αντιμετώπιση προβλημάτων με την πύλη Azure](../resource-manager-troubleshoot-deployments-portal.md)
- Μάθετε πώς μπορείτε να διαχειριστείτε την εικονική μηχανή που δημιουργήσατε ανατρέχοντας στο θέμα [Διαχείριση εικονικές μηχανές χρήση της διαχείρισης πόρων Azure και PowerShell](virtual-machines-windows-ps-manage.md).
