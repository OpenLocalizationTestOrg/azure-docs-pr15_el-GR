<properties
   pageTitle="Πώς μπορείτε να δημιουργήσετε NSGs σε λειτουργία ARM χρησιμοποιώντας ένα πρότυπο | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε και να αναπτύξετε NSGs στο ARM χρησιμοποιώντας ένα πρότυπο"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-using-a-template"></a>Πώς μπορείτε να δημιουργήσετε NSGs χρησιμοποιώντας ένα πρότυπο

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε επίσης να [δημιουργήσετε NSGs στο μοντέλο κλασική ανάπτυξης](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a>NSG πόρων σε ένα αρχείο προτύπου

Μπορείτε να προβάλετε και να κάνετε λήψη του [προτύπου δείγματος](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).

Στην παρακάτω ενότητα δείχνει τον ορισμό του προσκηνίου NSG, με βάση το παραπάνω σενάριο.

      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('frontEndNSGName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "NSG - Front End"
      },
      "properties": {
        "securityRules": [
          {
            "name": "rdp-rule",
            "properties": {
              "description": "Allow RDP",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 100,
              "direction": "Inbound"
            }
          },
          {
            "name": "web-rule",
            "properties": {
              "description": "Allow WEB",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "80",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 101,
              "direction": "Inbound"
            }
          }
        ]
      }

Για να συσχετίσετε το NSG στο προσκήνιο υποδίκτυο, πρέπει να αλλάξετε τον ορισμό υποδικτύου στο πρότυπο και να χρησιμοποιήσετε το αναγνωριστικό αναφοράς για το NSG.

        "subnets": [
          {
            "name": "[parameters('frontEndSubnetName')]",
            "properties": {
              "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
              "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
              }
            }
          }, ...

Παρατηρήστε το ίδιο γίνεται για παρασκηνίου NSG και το υποδίκτυο παρασκηνίου στο πρότυπο.

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Ανάπτυξη του προτύπου ARM με κλικ για την ανάπτυξη

Το δείγμα προτύπου διαθέσιμη στο δημόσιο αποθετήριο χρησιμοποιεί ένα αρχείο παραμέτρων που περιέχει την προεπιλεγμένη τιμών που χρησιμοποιούνται για τη δημιουργία το σενάριο που περιγράφονται παραπάνω. Για να αναπτύξετε αυτό το πρότυπο χρησιμοποιώντας κάντε κλικ στην επιλογή για να αναπτύξετε, ακολουθήστε [αυτήν τη σύνδεση](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), κάντε κλικ στην επιλογή **Ανάπτυξη να Azure**, αντικαταστήστε τις προεπιλεγμένες τιμές παραμέτρων Εάν είναι απαραίτητο, και ακολουθήστε τις οδηγίες στην πύλη του.

## <a name="deploy-the-arm-template-by-using-powershell"></a>Ανάπτυξη του προτύπου ARM με χρήση του PowerShell

Για να αναπτύξετε το πρότυπο ARM που λάβατε με χρήση του PowerShell, ακολουθήστε τα παρακάτω βήματα.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../powershell-install-configure.md) και ακολουθήστε τις οδηγίες προς το τέλος για να συνδεθείτε στο Azure και επιλέξτε τη συνδρομή σας.

3. Εκτελέστε το **`New-AzureRmResourceGroup`** cmdlet για να δημιουργήσετε μια ομάδα πόρων χρησιμοποιώντας το πρότυπο.

        New-AzureRmResourceGroup -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Αναμενόμενο αποτέλεσμα:

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            sqlAvSet            Microsoft.Compute/availabilitySets       westus  
                            webAvSet            Microsoft.Compute/availabilitySets       westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            Web1                Microsoft.Compute/virtualMachines        westus  
                            Web2                Microsoft.Compute/virtualMachines        westus  
                            TestNICSQL1         Microsoft.Network/networkInterfaces      westus  
                            TestNICSQL2         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb1         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb2         Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            TestPIPSQL1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPSQL2         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb2         Microsoft.Network/publicIPAddresses      westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG

## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Ανάπτυξη του προτύπου ARM, χρησιμοποιώντας το CLI Azure

Για να αναπτύξετε το πρότυπο ARM χρησιμοποιώντας το Azure CLI, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.
2. Εκτελέστε το **`azure config mode`** εντολή για να μεταβείτε σε λειτουργία διαχείρισης πόρων, όπως φαίνεται παρακάτω.

        azure config mode arm

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        info:    New mode is arm

4. Εκτελέστε το **`azure group deployment create`** cmdlet για να αναπτύξετε το νέο VNet χρησιμοποιώντας το πρότυπο και η παράμετρος αρχεία που έχουν ληφθεί και τροποποίησης παραπάνω. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Creating resource group TestRG
        info:    Created resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK

    - **-n (ή--όνομα)**. Το όνομα της ομάδας πόρων που θα δημιουργηθεί.
    - **-l (ή--θέση)**. Azure περιοχής όπου θα δημιουργηθεί η ομάδα πόρων.
    - **-f (ή--αρχείο προτύπου)**. Διαδρομή προς το αρχείο προτύπου ARM.
    - **-e (ή--παράμετροι-αρχείο)**. Διαδρομή προς το αρχείο σας ARM παράμετροι.
