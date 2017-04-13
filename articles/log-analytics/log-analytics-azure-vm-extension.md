<properties
    pageTitle="Σύνδεση Azure εικονικές μηχανές στο αρχείο καταγραφής ανάλυσης | Microsoft Azure"
    description="Για τα Windows και Linux εικονικές μηχανές εκτελούνται στο Azure, τον προτεινόμενο τρόπο που έχουν συλλεχθεί αρχεία καταγραφής και μετρήσεις είναι με την εγκατάσταση την επέκταση αρχείου καταγραφής ανάλυσης Azure Εικονική. Μπορείτε να χρησιμοποιήσετε το Azure πύλη ή το PowerShell για να εγκαταστήσετε την επέκταση αρχείου καταγραφής ανάλυσης εικονική μηχανή σε ΣΠΣ Azure."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="richrund"/>

# <a name="connect-azure-virtual-machines-to-log-analytics"></a>Σύνδεση Azure εικονικές μηχανές στο αρχείο καταγραφής ανάλυσης

Για τα Windows και Linux υπολογιστές, η προτεινόμενη μέθοδος για τη συλλογή αρχείων καταγραφής και μετρήσεις είναι μέσω της εγκατάστασης τον παράγοντα καταγραφής ανάλυσης.

Ο ευκολότερος τρόπος για να εγκαταστήσετε τον παράγοντα ανάλυση καταγραφής σε Azure εικονικές μηχανές είναι μέσω την επέκταση αρχείου καταγραφής ανάλυσης Εικονική.  Χρησιμοποιώντας την επέκταση απλοποιεί τη διαδικασία εγκατάστασης και ρυθμίζει αυτόματα τον παράγοντα για την αποστολή δεδομένων στο χώρο εργασίας του αρχείου καταγραφής ανάλυσης που καθορίζετε. Ο παράγοντας αναβαθμίζεται επίσης αυτόματα, εξασφαλίζοντας ότι έχετε τις πιο πρόσφατες δυνατότητες και επιδιορθώσεις.

Για εικονικές μηχανές Windows, μπορείτε να ενεργοποιήσετε την επέκταση εικονική μηχανή *Microsoft Agent παρακολούθησης* .
Για Linux εικονικές μηχανές, μπορείτε να ενεργοποιήσετε την επέκταση εικονική μηχανή *OMS παράγοντας για Linux* .

Μάθετε περισσότερα σχετικά με [τις επεκτάσεις Azure εικονική μηχανή](../virtual-machines/virtual-machines-windows-extensions-features.md) και [παράγοντας Linux] (... / virtual-machines/virtual-machines-linux-agent-user-guide.md).

Όταν χρησιμοποιείτε βάσει παράγοντας συλλογής για δεδομένα του αρχείου καταγραφής, πρέπει να ρυθμίσετε [προελεύσεις δεδομένων στο αρχείο καταγραφής ανάλυσης](log-analytics-data-sources.md) για να καθορίσετε τα αρχεία καταγραφής και τις μετρήσεις που θέλετε να συλλέξετε.

>[AZURE.IMPORTANT] Εάν ρυθμίζετε καταγραφής ανάλυσης δημιουργία ευρετηρίου καταγραφής δεδομένων, χρησιμοποιώντας τα [Διαγνωστικά Azure](log-analytics-azure-storage.md)και ρυθμίζετε τον παράγοντα για τη συλλογή τα ίδια αρχεία καταγραφής, στη συνέχεια, τα αρχεία καταγραφής συλλέγονται δύο φορές. Που θα χρεωθείτε για τις δύο προελεύσεις δεδομένων. Εάν έχετε εγκαταστήσει τον παράγοντα, στη συνέχεια, θα πρέπει να συλλέγετε δεδομένα του αρχείου καταγραφής χρησιμοποιώντας τον παράγοντα από μόνο του-δεν ρύθμιση παραμέτρων καταγραφής αναλυτικών στοιχείων για τη συλλογή δεδομένων καταγραφής από το Azure Διαγνωστικά.

Υπάρχουν τρεις εύκολοι τρόποι για να ενεργοποιήσετε την επέκταση αρχείου καταγραφής ανάλυσης εικονική μηχανή:

+ Χρησιμοποιώντας την πύλη του Azure
+ Με χρήση του Azure PowerShell
+ Με τη χρήση ενός προτύπου για τη διαχείριση πόρων Azure

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>Ενεργοποιήστε την επέκταση Εικονική στην πύλη του Azure

Μπορείτε να εγκαταστήσετε τον παράγοντα για ανάλυση καταγραφής και να συνδέσετε το Azure εικονική μηχανή που εκτελείται στον χρησιμοποιώντας την [πύλη του Azure](https://portal.azure.com).

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>Για να εγκαταστήσετε τον παράγοντα καταγραφής ανάλυσης και σύνδεση η εικονική μηχανή σε ένα χώρο εργασίας ανάλυσης αρχείου καταγραφής

1.  Πραγματοποιήστε είσοδο στην [πύλη του Azure](http://portal.azure.com).
2.  Επιλέξτε " **Αναζήτηση** " στην αριστερή πλευρά της πύλης, και, στη συνέχεια, μεταβείτε στο **Αρχείο καταγραφής ανάλυσης (OMS)** και επιλέξτε την.
3.  Στη λίστα των χώρων εργασίας καταγραφής αναλυτικών στοιχείων, επιλέξτε αυτήν που θέλετε να χρησιμοποιήσετε με το Azure Εικονική.  
    ![Χώροι εργασίας OMS](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4.  Στην περιοχή **Διαχείριση ανάλυσης αρχείου καταγραφής**, επιλέξτε **εικονικές μηχανές**.  
    ![Εικονικές μηχανές](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5.  Στη λίστα των **εικονικές μηχανές**, επιλέξτε την εικονική μηχανή στην οποία θέλετε να εγκαταστήσετε τον παράγοντα. Η **κατάσταση σύνδεσης OMS** για την εικονική Μηχανή υποδεικνύει ότι είναι **χωρίς σύνδεση**.  
    ![Εικονική δεν είστε συνδεδεμένοι](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6.  Στις λεπτομέρειες για την εικονική μηχανή σας, επιλέξτε **σύνδεση**. Ο παράγοντας εγκαθίσταται αυτόματα και ρύθμιση των παραμέτρων για το χώρο εργασίας του αρχείου καταγραφής ανάλυσης. Αυτή η διαδικασία διαρκεί μερικά λεπτά, κατά την οποία η κατάσταση σύνδεσης OMS είναι *τη σύνδεση...*  
    ![Σύνδεση Εικονική](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7.  Μετά την εγκατάσταση και τη σύνδεση τον παράγοντα, την κατάσταση **σύνδεσης OMS** θα ενημερώνεται για να εμφανίσετε **αυτόν το χώρο εργασίας**.  
    ![Συνδεδεμένοι](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)


## <a name="enable-the-vm-extension-using-powershell"></a>Ενεργοποιήστε την επέκταση Εικονική χρήση του PowerShell

Υπάρχουν διαφορετικές εντολές για Azure κλασική εικονικές μηχανές και διαχείριση πόρων εικονικές μηχανές. Ακολουθούν παραδείγματα για κλασική και εικονικές μηχανές διαχείριση πόρων.

Για κλασική εικονικές μηχανές, χρησιμοποιήστε το παρακάτω παράδειγμα PowerShell:

```
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Για τη διαχείριση πόρων εικονικές μηχανές, χρησιμοποιήστε το παρακάτω παράδειγμα PowerShell:

```
Login-AzureRMAccount
Select-AzureSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```
Όταν ρυθμίζετε την εικονική μηχανή σας με χρήση του PowerShell, πρέπει να παρέχετε το **Αναγνωριστικό χώρου εργασίας** και το **Πρωτεύον κλειδί**. Μπορείτε να βρείτε το αναγνωριστικό και το κλειδί στη σελίδα **ρυθμίσεων** της πύλης OMS ή με τη χρήση του PowerShell, όπως φαίνεται στο παραπάνω παράδειγμα.

![Αναγνωριστικό χώρου εργασίας και πρωτεύοντος κλειδιού](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

## <a name="deploy-the-vm-extension-using-a-template"></a>Αναπτύξτε την επέκταση Εικονική χρησιμοποιώντας ένα πρότυπο

Χρησιμοποιώντας τη διαχείριση πόρων Azure, μπορείτε να δημιουργήσετε ένα απλό πρότυπο (σε μορφή JSON) που καθορίζει το ανάπτυξης και ρύθμισης παραμέτρων της εφαρμογής σας. Αυτό το πρότυπο είναι γνωστό ως πρότυπο διαχείρισης πόρων και παρέχει δηλωτικό τρόπο για να ορίσετε μια ανάπτυξη. Χρησιμοποιώντας ένα πρότυπο, επανειλημμένα μπορείτε να αναπτύξετε την εφαρμογή σας σε όλο τον κύκλο ζωής εφαρμογή και να πιστεύετε ότι τους πόρους σας είναι να αναπτυχθεί σε μια συνεπή κατάσταση.

Συμπεριλαμβάνοντας τον παράγοντα ανάλυση καταγραφής ως μέρος του προτύπου σας για τη διαχείριση πόρων, μπορείτε να εξασφαλίσετε ότι κάθε εικονική μηχανή είναι προκαθορισμένες ρυθμίσεις παραμέτρων για την αναφορά στο χώρο εργασίας σας καταγραφής ανάλυσης.

Για περισσότερες πληροφορίες σχετικά με τα πρότυπα για τη διαχείριση πόρων, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](../resource-group-authoring-templates.md).

Ακολουθεί ένα παράδειγμα ενός προτύπου για τη διαχείριση πόρων που χρησιμοποιείται για την ανάπτυξη μια εικονική μηχανή που εκτελεί Windows με την επέκταση Microsoft Agent παρακολούθηση εγκατεστημένο. Αυτό το πρότυπο είναι ένα πρότυπο τυπικές εικονική μηχανή, με τις ακόλουθες προσθήκες:

+ παράμετροι workspaceId και workspaceName
+ Ενότητα επέκταση Microsoft.EnterpriseCloud.Monitoring πόρων
+ Εξόδους για να αναζητήσετε το workspaceId και workspaceSharedKey


```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMD workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Μπορείτε να αναπτύξετε ένα πρότυπο, χρησιμοποιώντας την ακόλουθη εντολή PowerShell:

```
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-windows-virtual-machines"></a>Αντιμετώπιση προβλημάτων σε εικονικές μηχανές Windows

Εάν η επέκταση *Microsoft Agent παρακολούθηση* Εικονική παράγοντα δεν είναι κατά την εγκατάσταση ή αναφοράς μπορείτε να εκτελέσετε τα παρακάτω βήματα για να αντιμετωπίσετε το πρόβλημα.

1. Έλεγχος εάν έχει εγκατασταθεί τον παράγοντα Εικονική Azure και λειτουργεί σωστά, χρησιμοποιώντας τα βήματα που περιγράφονται σε [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
  + Μπορείτε επίσης να αναθεωρήσετε το αρχείο καταγραφής Εικονική παράγοντα`C:\WindowsAzure\logs\WaAppAgent.log`
  + Εάν δεν υπάρχει το αρχείο καταγραφής, τον παράγοντα Εικονική δεν έχει εγκατασταθεί.
    - [Εγκατάσταση τον παράγοντα Εικονική Azure σε κλασική ΣΠΣ](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md)
2. Επιβεβαιώστε ότι η εργασία συγχρονισμό επέκταση Microsoft Agent παρακολούθηση λειτουργεί με τα παρακάτω βήματα:
  + Συνδεθείτε με την εικονική μηχανή
  + Ανοίξτε το Χρονοδιάγραμμα εργασιών και να βρείτε το `update_azureoperationalinsight_agent_heartbeat` εργασίας
  + Επιβεβαίωση της εργασίας είναι ενεργοποιημένη και εκτελείται κάθε ένα λεπτό
  + Μεταβίβαση ελέγχου του αρχείου καταγραφής συγχρονισμό`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Εξετάστε τα αρχεία καταγραφής επέκταση εικονική Μηχανή παράγοντας παρακολούθησης της Microsoft στο`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
3. Βεβαιωθείτε ότι η εικονική μηχανή μπορούν να εκτελούν δέσμες ενεργειών του PowerShell
4. Βεβαιωθείτε ότι έχουν δικαιώματα σε C:\Windows\temp δεν έχουν αλλάξει
5. Προβολή της κατάστασης του Microsoft Agent παρακολούθησης, πληκτρολογώντας τα εξής σε ένα παράθυρο του PowerShell αναβαθμισμένα δικαιώματα στον υπολογιστή εικονικές`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. Εξετάστε τα αρχεία καταγραφής εγκατάστασης Microsoft Agent παρακολούθησης στο`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Για περισσότερες πληροφορίες, ανατρέξτε στην [Αντιμετώπιση προβλημάτων επεκτάσεις των Windows](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md).

## <a name="troubleshooting-linux-virtual-machines"></a>Αντιμετώπιση προβλημάτων Linux εικονικές μηχανές

Εάν η επέκταση παράγοντας Εικονική *Παράγοντας OMS για Linux* δεν είναι κατά την εγκατάσταση ή αναφοράς μπορείτε να εκτελέσετε τα παρακάτω βήματα για να αντιμετωπίσετε το πρόβλημα.

1. Αν η κατάσταση επέκταση είναι *Άγνωστη* ελέγχου εάν έχει εγκατασταθεί τον παράγοντα Εικονική Azure και λειτουργεί σωστά αναθεώρηση με το αρχείο καταγραφής Εικονική παράγοντα`/var/log/waagent.log`
  + Εάν δεν υπάρχει το αρχείο καταγραφής, τον παράγοντα Εικονική δεν έχει εγκατασταθεί.
  - [Εγκατάσταση τον παράγοντα Azure Εικονική σε ΣΠΣ Linux](../virtual-machines/virtual-machines-linux-agent-user-guide.md)
2. Για άλλες καταστάσεις κατεστραμμένες, εξετάστε τον παράγοντα OMS για την επέκταση Εικονική Linux των αρχείων καταγραφής `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` και`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Εάν η κατάσταση επέκταση είναι σε καλή κατάσταση, αλλά δεν γίνεται αποστολή δεδομένων, εξετάστε τον παράγοντα OMS για τα αρχεία καταγραφής Linux στο`/var/opt/microsoft/omsagent/log/omsagent.log`

Για περισσότερες πληροφορίες, ανατρέξτε στην [Αντιμετώπιση προβλημάτων Linux επεκτάσεις](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md).


## <a name="next-steps"></a>Επόμενα βήματα

+ Ρύθμιση παραμέτρων [προελεύσεις δεδομένων στο αρχείο καταγραφής ανάλυσης](log-analytics-data-sources.md) για να καθορίσετε τα αρχεία καταγραφής και μετρήσεις για τη συλλογή.
+ Για τη συγκέντρωση δεδομένων από εικονικές μηχανές [λύσεις Προσθήκη αρχείου καταγραφής ανάλυση από τη συλλογή λύσεων](log-analytics-add-solutions.md).
+ [Συλλογή δεδομένων, χρησιμοποιώντας τα Διαγνωστικά Azure](log-analytics-azure-storage.md) για άλλους πόρους που εκτελούνται στο Azure.

Για υπολογιστές που δεν βρίσκονται σε Azure, μπορείτε να εγκαταστήσετε τον παράγοντα καταγραφής ανάλυση, χρησιμοποιώντας τις μεθόδους που περιγράφονται στα ακόλουθα άρθρα:

+ [Σύνδεση υπολογιστές με Windows στο αρχείο καταγραφής ανάλυσης](log-analytics-windows-agents.md)
+ [Σύνδεση Linux υπολογιστές στο αρχείο καταγραφής ανάλυσης](log-analytics-linux-agents.md)
