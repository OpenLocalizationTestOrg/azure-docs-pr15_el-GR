<properties
   pageTitle="Ανάπτυξη μια Εικονική με στατική δημόσια IP χρησιμοποιώντας ένα πρότυπο στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αναπτύξετε το ΣΠΣ με στατική δημόσια IP χρησιμοποιώντας ένα πρότυπο στη Διαχείριση πόρων"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-a-template"></a>Ανάπτυξη μια Εικονική με στατική δημόσια IP χρησιμοποιώντας ένα πρότυπο

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-resources-in-a-template-file"></a>Δημόσια IP πόρων σε ένα αρχείο προτύπου

Μπορείτε να προβάλετε και να κάνετε λήψη του [προτύπου δείγματος](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

Στην παρακάτω ενότητα δείχνει τον ορισμό του δημόσια πόρου IP, με βάση το παραπάνω σενάριο.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[variables('webVMSetting').pipName]",
        "location": "[variables('location')]",
        "properties": {
          "publicIPAllocationMethod": "Static"
        },
        "tags": {
          "displayName": "PublicIPAddress - Web"
        }
      },

Παρατηρήστε ότι η ιδιότητα **publicIPAllocationMethod** , που έχει οριστεί σε *στατικό*. Αυτή η ιδιότητα μπορεί να είναι είτε *δυναμική* (προεπιλεγμένη τιμή) ή το *στατικό*. Ρύθμιση του σε στατικό εγγυήσεις που ποτέ δεν θα αλλάξει στη δημόσια διεύθυνση IP που έχουν εκχωρηθεί.

Στην παρακάτω ενότητα εμφανίζει τη συσχέτιση του στη δημόσια διεύθυνση IP με το περιβάλλον εργασίας δικτύου.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[variables('webVMSetting').nicName]",
        "location": "[variables('location')]",
        "tags": {
          "displayName": "NetworkInterface - Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
          "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
          "ipConfigurations": [
            {
              "name": "ipconfig1",
              "properties": {
                "privateIPAllocationMethod": "Static",
                "privateIPAddress": "[variables('webVMSetting').ipAddress]",
                "publicIPAddress": {
                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
                },
                "subnet": {
                  "id": "[variables('frontEndSubnetRef')]"
                }
              }
            }
          ]
        }
      },

Παρατηρήστε ότι η ιδιότητα **publicIPAddress** που δείχνει το **αναγνωριστικό** του πόρου με το όνομα **variables('webVMSetting').pipName**. Που είναι το όνομα του πόρου δημόσια IP φαίνεται παραπάνω.

Τέλος, το περιβάλλον εργασίας δικτύου παραπάνω παρατίθεται στην ιδιότητα **networkProfile** η Εικονική που δημιουργούνται.

      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Ανάπτυξη του προτύπου με κλικ για την ανάπτυξη

Το δείγμα προτύπου διαθέσιμη στο δημόσιο αποθετήριο χρησιμοποιεί ένα αρχείο παραμέτρων που περιέχει την προεπιλεγμένη τιμών που χρησιμοποιούνται για τη δημιουργία το σενάριο που περιγράφονται παραπάνω. Για να αναπτύξετε αυτό το πρότυπο χρησιμοποιώντας κάντε κλικ στην επιλογή για την ανάπτυξη, κάντε κλικ στην επιλογή **Ανάπτυξη να Azure** στο αρχείο Readme.md για το πρότυπο [Εικονική με στατική PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) . Αντικαταστήστε τις προεπιλεγμένες τιμές παραμέτρων Εάν θέλετε και καταχωρήστε τιμές για τις παραμέτρους κενό.  Ακολουθήστε τις οδηγίες στην πύλη του για να δημιουργήσετε μια εικονική μηχανή με μια στατική δημόσια διεύθυνση IP.

## <a name="deploy-the-template-by-using-powershell"></a>Ανάπτυξη του προτύπου με χρήση του PowerShell

Για να αναπτύξετε το πρότυπο που λάβατε με χρήση του PowerShell, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../powershell-install-configure.md) και ακολουθήστε τις οδηγίες στα βήματα 1 έως 3.

2. Σε μια κονσόλα PowerShell, εκτελέστε το cmdlet **New-AzureRmResourceGroup** για να δημιουργήσετε μια νέα ομάδα πόρων, εάν είναι απαραίτητο. Εάν έχετε ήδη μια ομάδα πόρων που δημιουργήσατε, μεταβείτε στο βήμα 3.

        New-AzureRmResourceGroup -Name PIPTEST -Location westus

    Αναμενόμενο αποτέλεσμα:

        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. Σε μια κονσόλα PowerShell, εκτελέστε το cmdlet **New-AzureRmResourceGroupDeployment** για να αναπτύξετε το πρότυπο.

        New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
            -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json

    Αναμενόμενο αποτέλεσμα:

        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : <Deployment date> <Deployment time>
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0

        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         

        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Ανάπτυξη του προτύπου με τη χρήση του CLI Azure

Για να αναπτύξετε το πρότυπο χρησιμοποιώντας το Azure CLI, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ακολουθήστε τα βήματα στο άρθρο [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md) και, στη συνέχεια, τα βήματα για να συνδέσετε το CLI στη συνδρομή σας, στην ενότητα "Χρησιμοποιήστε azure σύνδεσης για τον έλεγχο ταυτότητας αλληλεπιδραστικά" αυτού του άρθρου [σύνδεση με μια συνδρομή του Azure από το περιβάλλον γραμμής εντολών Azure (Azure CLI)](../xplat-cli-connect.md) .
2. Εκτελέστε την εντολή **λειτουργία azure ρύθμισης παραμέτρων** για να μεταβείτε σε λειτουργία διαχείρισης πόρων, όπως φαίνεται παρακάτω.

        azure config mode arm

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        info:    New mode is arm

3. Ανοίξτε το [αρχείο παραμέτρων](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), επιλέξτε το περιεχόμενο και αποθηκεύστε το σε ένα αρχείο στον υπολογιστή σας. Για αυτό το παράδειγμα, οι παράμετροι αποθηκεύονται σε ένα αρχείο με το όνομα *parameters.json*. Αλλάξτε τις τιμές παραμέτρων μέσα στο αρχείο, εάν θέλετε, αλλά, τουλάχιστον, καλό είναι ότι μπορείτε να αλλάξετε την τιμή για την παράμετρο adminPassword σε ένα μοναδικό, σύνθετα τον κωδικό πρόσβασης.

4. Εκτελέστε το cmdlet **Δημιουργία azure ομάδα ανάπτυξης** για να αναπτύξετε το νέο VNet χρησιμοποιώντας τα αρχεία προτύπου και η παράμετρος που έχουν ληφθεί και τροποποίησης παραπάνω. Στο παρακάτω εντολή, αντικαταστήστε <path> με τη διαδρομή που αποθηκεύσατε το αρχείο. 

        azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json

    Αναμενόμενο αποτέλεσμα (λίστες τιμές παραμέτρων χρησιμοποιούνται):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/<Subscription ID>/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
