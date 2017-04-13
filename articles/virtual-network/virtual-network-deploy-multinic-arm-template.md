<properties
   pageTitle="Ανάπτυξη πολλούς NIC ΣΠΣ χρησιμοποιώντας ένα πρότυπο στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αναπτύξετε το ΣΠΣ NIC πολλούς χρησιμοποιώντας ένα πρότυπο στη Διαχείριση πόρων"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="deploy-multi-nic-vms-using-a-template"></a>Ανάπτυξη πολλούς NIC ΣΠΣ χρησιμοποιώντας ένα πρότυπο

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][μοντέλο κλασική ανάπτυξης](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Δεδομένου ότι σε αυτό το σημείο στιγμή δεν έχετε ΣΠΣ με ένα μεμονωμένο NIC και ΣΠΣ με πολλές κάρτες διασύνδεσης δικτύου στην ίδια ομάδα πόρων, θα υλοποιήσετε τους διακομιστές παρασκηνίου σε μια ομάδα πόρων και όλα τα άλλα στοιχεία σε μια άλλη ομάδα ασφαλείας. Τα παρακάτω βήματα Χρησιμοποιήστε μια ομάδα πόρων με το όνομα *IaaSStory* για την ομάδα κύριο πόρων και *IaaSStory παρασκηνίου* για τους διακομιστές παρασκηνίου.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να αναπτύξετε τους διακομιστές παρασκηνίου, πρέπει να αναπτύξετε την ομάδα των πόρων κύριο με όλους τους πόρους που είναι απαραίτητο για αυτό το σενάριο. Για να αναπτύξετε αυτούς τους πόρους, ακολουθήστε τα παρακάτω βήματα.

1. Μεταβείτε [στη](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)σελίδα προτύπου.
2. Στη σελίδα του προτύπου, στα δεξιά της **γονικής ομάδας πόρων**, κάντε κλικ στην επιλογή **Ανάπτυξη να Azure**.
3. Εάν είναι απαραίτητο, αλλάξτε τις τιμές παραμέτρων και, στη συνέχεια, ακολουθήστε τα βήματα στην πύλη του Azure προεπισκόπηση για να αναπτύξετε την ομάδα των πόρων.

> [AZURE.IMPORTANT] Βεβαιωθείτε ότι τα ονόματα λογαριασμού χώρου αποθήκευσης είναι μοναδικό. Δεν μπορείτε να έχετε ονόματα λογαριασμών διπλότυπων χώρου αποθήκευσης στο Azure.

## <a name="understand-the-deployment-template"></a>Κατανόηση του προτύπου ανάπτυξης

Πριν να αναπτύξετε το πρότυπο που παρέχεται με αυτήν την τεκμηρίωση, βεβαιωθείτε ότι γνωρίζετε τι κάνει. Τα παρακάτω βήματα παρέχουν μια επαρκής επισκόπηση των εν λόγω του προτύπου.

1. Μεταβείτε [στη](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)σελίδα προτύπου.
2. Κάντε κλικ στην επιλογή **azuredeploy.json** για να ανοίξετε το αρχείο προτύπου.
3. Παρατηρήστε ότι η παράμετρος *osType* που παρατίθενται παρακάτω. Αυτή η παράμετρος χρησιμοποιείται για να επιλέξετε ποια Εικονική εικόνα για να χρησιμοποιήσετε για το διακομιστή βάσης δεδομένων, μαζί με πολλές λειτουργικό σύστημα που σχετίζονται με ρυθμίσεις.

        "osType": {
          "type": "string",
          "defaultValue": "Windows",
          "allowedValues": [
            "Windows",
            "Ubuntu"
          ],
          "metadata": {
            "description": "Type of OS to use for VMs: Windows or Ubuntu."
          }
        },

4. Κάντε κύλιση προς τα κάτω για τη λίστα των μεταβλητών και ελέγξτε τον ορισμό για τις μεταβλητές **dbVMSetting** , που αναφέρονται παρακάτω. Λάβει ένα από τα στοιχεία του πίνακα που περιέχεται στη μεταβλητή **dbVMSettings** . Εάν είστε εξοικειωμένοι με την ορολογία ανάπτυξης λογισμικού, μπορείτε να προβάλετε τη μεταβλητή **dbVMSettings** ως ένα hashtable ή ένα dictionay.

        "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"

5. Ας υποθέσουμε ότι μπορείτε να αποφασίσετε για την ανάπτυξη του Windows ΣΠΣ εκτελεί SQL στο παρασκήνιο. Στη συνέχεια, η τιμή για **osType** θα είναι *των Windows*και η μεταβλητή **dbVMSetting** θα περιέχει το στοιχείο που παρατίθενται παρακάτω, που αντιπροσωπεύει την πρώτη τιμή στη μεταβλητή **dbVMSettings** .

          "Windows": {
            "vmSize": "Standard_DS3",
            "publisher": "MicrosoftSQLServer",
            "offer": "SQL2014SP1-WS2012R2",
            "sku": "Standard",
            "version": "latest",
            "vmName": "DB",
            "osdisk": "osdiskdb",
            "datadisk": "datadiskdb",
            "nicName": "NICDB",
            "ipAddress": "192.168.2.",
            "extensionDeployment": "",
            "avsetName": "ASDB",
            "remotePort": 3389,
            "dbPort": 1433
          },

6. Παρατηρήστε το **vmSize** περιέχει την τιμή *Standard_DS3*. Μόνο ορισμένων Εικονική μεγέθη επιτρέπεται τη χρήση πολλών NIC. Μπορείτε να επαληθεύσετε ποια μεγέθη Εικονική είναι πολλούς NIC που είναι ενεργοποιημένη, επισκεφθείτε την [Επισκόπηση NIC πολλούς](virtual-networks-multiple-nics.md).
7. Κάντε κύλιση προς τα κάτω, **τους πόρους** και παρατηρήστε το πρώτο στοιχείο. Περιγράφει ένα λογαριασμό του χώρου αποθήκευσης. Αυτόν το λογαριασμό χώρου αποθήκευσης θα χρησιμοποιηθεί για τη διατήρηση των δίσκων δεδομένων που χρησιμοποιούνται από κάθε βάση δεδομένων Εικονική. Σε αυτό το σενάριο, κάθε βάση δεδομένων Εικονική έχει ένα δίσκο λειτουργικό σύστημα είναι αποθηκευμένα σε κανονική χώρου αποθήκευσης και δύο δίσκων δεδομένων που είναι αποθηκευμένα στο χώρο αποθήκευσης SSD (premium).

        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('prmStorageName')]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "Storage Account - Premium"
          },
          "properties": {
            "accountType": "[parameters('prmStorageType')]"
          }
        },

8. Κάντε κύλιση προς τα κάτω τον επόμενο πόρο, όπως αναφέρονται παρακάτω. Αυτός ο πόρος αντιπροσωπεύει το NIC που χρησιμοποιούνται για την πρόσβαση στη βάση δεδομένων σε κάθε βάση δεδομένων Εικονική. Παρατηρήστε ότι η χρήση της συνάρτησης **αντίγραφο** σε αυτόν τον πόρο. Το πρότυπο σάς επιτρέπει να αναπτύξετε όσες ΣΠΣ όπως θέλετε, με βάση την παράμετρο **dbCount** . Επομένως, πρέπει να δημιουργήσετε την ίδια ποσότητα NIC για πρόσβαση στη βάση δεδομένων, μία για κάθε Εικονική.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB DA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

9. Κάντε κύλιση προς τα κάτω τον επόμενο πόρο, όπως αναφέρονται παρακάτω. Αυτός ο πόρος αντιπροσωπεύει το NIC χρησιμοποιείται για τη διαχείριση σε κάθε βάση δεδομένων Εικονική. Και πάλι, χρειάζεστε ένα από αυτά τα NIC για κάθε βάση δεδομένων Εικονική. Παρατηρήστε ότι το στοιχείο **networkSecurityGroup** , σύνδεση μια NSG που επιτρέπει την πρόσβαση σε αυτό το NIC μόνο RDP/SSH.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB RA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
                  },
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

10. Κάντε κύλιση προς τα κάτω τον επόμενο πόρο, όπως αναφέρονται παρακάτω. Αυτός ο πόρος αντιπροσωπεύει μια διαθεσιμότητα ρύθμιση για κοινή χρήση με όλους VMs βάσης δεδομένων. Με αυτόν τον τρόπο που εγγυάται ότι θα υπάρχει πάντα μία Εικονική του συνόλου εκτελείται κατά τη συντήρηση.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/availabilitySets",
          "name": "[variables('dbVMSetting').avsetName]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "AvailabilitySet - DB"
          }
        },

11. Κάντε κύλιση προς τα κάτω τον επόμενο πόρο. Αυτός ο πόρος αντιπροσωπεύει τη βάση δεδομένων ΣΠΣ, όπως φαίνεται στο πρώτο μερικές γραμμές που παρατίθενται παρακάτω. Παρατηρήστε ότι η χρήση της συνάρτησης **αντίγραφο** ξανά, εξασφαλίζοντας ότι πολλές ΣΠΣ δημιουργούνται με βάση την παράμετρο **dbCount** . Παρατηρήστε επίσης τη συλλογή **dependsOn** . Παραθέτει σε λίστα δύο NIC που είναι απαραίτητο να δημιουργηθεί πριν από την εικονική Μηχανή έχει αναπτυχθεί, μαζί με το σύνολο διαθεσιμότητα και το λογαριασμό χώρου αποθήκευσης.

          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
          "location": "[variables('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
            "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
            "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
          ],
          "tags": {
            "displayName": "VMs - DB"
          },
          "copy": {
            "name": "dbvmcount",
            "count": "[parameters('dbCount')]"
          },

12. Κάντε κύλιση προς τα κάτω στον πόρο Εικονική στο στοιχείο **networkProfile** , όπως αναφέρονται παρακάτω. Παρατηρήστε ότι υπάρχουν δύο NIC γίνεται αναφορά για κάθε Εικονική. Όταν δημιουργείτε πολλά NIC για μια Εικονική, πρέπει να ορίσετε το **πρωτεύον** ιδιότητα μίας από το NIC σε *true*και τα υπόλοιπα σε *false*.

        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
              "properties": { "primary": true }
            },
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
              "properties": { "primary": false }
            }
          ]
        }
      }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Ανάπτυξη του προτύπου ARM με κλικ για την ανάπτυξη

> [AZURE.IMPORTANT] Βεβαιωθείτε ότι μπορείτε να ακολουθήσετε τα βήματα [προαπαιτούμενα](#Pre-requisites) πριν να ακολουθήσετε τις παρακάτω οδηγίες.

Το δείγμα προτύπου διαθέσιμη στο δημόσιο αποθετήριο χρησιμοποιεί ένα αρχείο παραμέτρων που περιέχει την προεπιλεγμένη τιμών που χρησιμοποιούνται για τη δημιουργία το σενάριο που περιγράφονται παραπάνω. Για να αναπτύξετε αυτό το πρότυπο χρησιμοποιώντας κάντε κλικ στην επιλογή για να αναπτύξετε, ακολουθήστε [αυτήν τη σύνδεση](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), στα δεξιά του **πόρου παρασκηνίου ομαδοποίηση (ανατρέξτε στην τεκμηρίωση)** κάντε κλικ στην επιλογή **Ανάπτυξη Azure**, αντικαταστήστε τις προεπιλεγμένες τιμές παραμέτρων Εάν είναι απαραίτητο, και ακολουθήστε τις οδηγίες στην πύλη.

Η παρακάτω εικόνα εμφανίζει τα περιεχόμενα της νέας ομάδας πόρων, μετά την ανάπτυξη.

![Ομάδα πόρων παρασκηνίου](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>Ανάπτυξη του προτύπου με χρήση του PowerShell

Για να αναπτύξετε το πρότυπο που λάβατε με χρήση του PowerShell, ακολουθήστε τα παρακάτω βήματα.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Εκτελέστε το **`New-AzureRmResourceGroup`** cmdlet για να δημιουργήσετε μια ομάδα πόρων χρησιμοποιώντας το πρότυπο.

        New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'

    Αναμενόμενο αποτέλεσμα:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                 Type                                 Location
                            ===================  ===================================  ========
                            ASDB                 Microsoft.Compute/availabilitySets   westus  
                            DB1                  Microsoft.Compute/virtualMachines    westus  
                            DB2                  Microsoft.Compute/virtualMachines    westus  
                            NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                            wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Ανάπτυξη του προτύπου με τη χρήση του CLI Azure

Για να αναπτύξετε το πρότυπο χρησιμοποιώντας το Azure CLI, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.
2. Εκτελέστε το **`azure config mode`** εντολή για να μεταβείτε σε λειτουργία διαχείρισης πόρων, όπως φαίνεται παρακάτω.

        azure config mode arm

    Εδώ είναι το αναμενόμενο αποτέλεσμα για την παραπάνω εντολή:

        info:    New mode is arm

3. Ανοίξτε το [αρχείο παραμέτρων](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), επιλέξτε τα περιεχόμενά και αποθηκεύστε το σε ένα αρχείο στον υπολογιστή σας. Για αυτό το παράδειγμα, θα σας να αποθηκεύσει το αρχείο παραμέτρους για να *parameters.json*.

4. Εκτελέστε το **`azure group deployment create`** cmdlet για να αναπτύξετε το νέο VNet χρησιμοποιώντας το πρότυπο και η παράμετρος αρχεία που έχουν ληφθεί και τροποποίησης παραπάνω. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται.

        azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
