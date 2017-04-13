<properties
    pageTitle="Ορίζει κλίμακα εικονική μηχανή Autoscale Windows | Microsoft Azure"
    description="Ρύθμιση του autoscaling για ένα Windows εικονική μηχανή κλίμακα ρύθμιση χρήση του Azure PowerShell"
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

# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Αυτόματης κλιμάκωσης μηχανές σε ένα σύνολο κλίμακα εικονική μηχανή

Εικονική μηχανή κλίμακα σύνολα διευκολύνετε τους για να αναπτύξετε και να διαχειριστείτε πανομοιότυπες εικονικές μηχανές ως σύνολο. Σύνολα κλίμακα παρέχουν ένα επίπεδο ιδιαίτερα μεταβλητού μεγέθους και προσαρμόσιμες υπολογισμού για εφαρμογές hyperscale και υποστηρίζουν εικόνες πλατφόρμα Windows, Linux πλατφόρμα εικόνες, προσαρμοσμένες εικόνες και επεκτάσεις. Για περισσότερες πληροφορίες σχετικά με τα σύνολα κλίμακας, ανατρέξτε στο θέμα [Εικονική μηχανή κλίμακα σύνολα](virtual-machine-scale-sets-overview.md).

Αυτό το πρόγραμμα εκμάθησης θα μάθετε πώς να δημιουργήσετε ένα σύνολο κλίμακα εικονικές μηχανές Windows και να προσαρμόζεται αυτόματα τις μηχανές του συνόλου. Μπορείτε να δημιουργήσετε την κλίμακα ορίσετε και να ρυθμίσετε την κλιμάκωση κατά τη δημιουργία ενός προτύπου για τη διαχείριση πόρων Azure και την ανάπτυξη χρησιμοποιώντας Azure PowerShell. Για περισσότερες πληροφορίες σχετικά με τα πρότυπα, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](../resource-group-authoring-templates.md). Για να μάθετε περισσότερα σχετικά με την αυτόματη κλιμάκωση της κλίμακας σύνολα, ανατρέξτε στο θέμα [αυτόματης κλίμακας και εικονική μηχανή κλίμακα σύνολα](virtual-machine-scale-sets-autoscale-overview.md).

Σε αυτό το άρθρο, μπορείτε να αναπτύξετε τους παρακάτω πόρους και επεκτάσεις:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Για περισσότερες πληροφορίες σχετικά με τους πόρους για τη διαχείριση πόρων, δείτε [τον υπολογισμό Azure, δικτύου, και υπηρεσίες παροχής χώρου αποθήκευσης στην περιοχή Διαχείριση πόρων Azure](../virtual-machines/virtual-machines-windows-compare-deployment-models.md).

## <a name="step-1-install-azure-powershell"></a>Βήμα 1: Εγκατάσταση του Azure PowerShell

Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για πληροφορίες σχετικά με την εγκατάσταση την πιο πρόσφατη έκδοση του Azure PowerShell, επιλέγοντας τη συνδρομή σας και είσοδος σε Azure.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Βήμα 2: Δημιουργήστε μια ομάδα πόρων και ένα λογαριασμό του χώρου αποθήκευσης

1. **Δημιουργία μιας ομάδας πόρων** – όλους τους πόρους πρέπει να αναπτυχθεί σε μια ομάδα πόρων. Χρησιμοποιήστε [Νέα AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) για να δημιουργήσετε μια ομάδα πόρων με το όνομα **vmsstestrg1**.

2. **Δημιουργία λογαριασμού χώρου αποθήκευσης** – αυτόν το λογαριασμό χώρου αποθήκευσης είναι όπου είναι αποθηκευμένο το πρότυπο. Χρησιμοποιήστε [Νέα AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) να δημιουργήσετε ένα λογαριασμό χώρου αποθήκευσης με την ονομασία **vmsstestsa**.

## <a name="step-3-create-the-template"></a>Βήμα 3: Δημιουργία του προτύπου
Ένα πρότυπο από διαχειριστή πόρων Azure δίνει τη δυνατότητα για να αναπτύξετε και να διαχειριστείτε πόρους Azure μαζί, χρησιμοποιώντας μια περιγραφή JSON για τους πόρους και τις παραμέτρους συσχετισμένη ανάπτυξης.

1. Στο πρόγραμμα επεξεργασίας Αγαπημένα, δημιουργήστε το αρχείο C:\VMSSTemplate.json και προσθέστε το αρχικό δομή JSON για την υποστήριξη του προτύπου.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
          ]
        }

2. Παράμετροι δεν είναι πάντα απαραίτητα, αλλά παρέχουν ένας τρόπος για να εισαγάγετε τιμές, όταν έχει αναπτυχθεί το πρότυπο. Προσθέστε αυτές τις παραμέτρους κάτω από το γονικό στοιχείο παράμετροι που έχετε προσθέσει στο πρότυπο.

        "vmName": { "type": "string" },
        "vmSSName": { "type": "string" },
        "instanceCount": { "type": "string" },
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" },
        "resourcePrefix": { "type": "string" }

    - Ορίστε ένα όνομα για την ξεχωριστή εικονική μηχανή που χρησιμοποιείται για να αποκτήσετε πρόσβαση οι υπολογιστές στην κλίμακα.
    - Το όνομα του λογαριασμού χώρου αποθήκευσης όπου είναι αποθηκευμένο το πρότυπο.
    - Ο αριθμός των εικονικές μηχανές για να δημιουργήσετε αρχικά στο σύνολο κλίμακα.
    - Το όνομα και τον κωδικό πρόσβασης του λογαριασμού του διαχειριστή στο τις εικονικές μηχανές.
    - Ορίστε ένα πρόθεμα για τους πόρους που έχουν δημιουργηθεί για την υποστήριξη της κλίμακας.

3. Μεταβλητές μπορούν να χρησιμοποιηθούν σε ένα πρότυπο για να καθορίσετε τιμές που μπορεί να αλλάζουν συχνά ή που πρέπει να δημιουργηθεί από ένα συνδυασμό των τιμών της παραμέτρου. Προσθέστε αυτές τις μεταβλητές κάτω από το γονικό στοιχείο μεταβλητές που έχετε προσθέσει στο πρότυπο.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
          "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

  - Ονόματα DNS που χρησιμοποιούνται από τις διασυνδέσεις δικτύου.
    - Τα ονόματα διευθύνσεων IP και στα προθέματα για το εικονικό δίκτυο και δευτερεύοντα δίκτυα.
    - Τα ονόματα και τα αναγνωριστικά εικονικού δικτύου και φόρτωση εξισορρόπησης και διασυνδέσεις δικτύου.
    - Ορισμός χώρου αποθήκευσης ονόματα λογαριασμών για τους λογαριασμούς που σχετίζονται με τις μηχανές στην κλίμακα.
    - Ρυθμίσεις για την επέκταση διαγνωστικά που είναι εγκατεστημένη στον τις εικονικές μηχανές. Για περισσότερες πληροφορίες σχετικά με την επέκταση Διαγνωστικά, ανατρέξτε στο θέμα [Δημιουργία εικονικών Windows μηχάνημα με παρακολούθηση και τα Διαγνωστικά χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).

4. Προσθέστε τον πόρο λογαριασμού χώρου αποθήκευσης κάτω από το γονικό στοιχείο τους πόρους που έχετε προσθέσει στο πρότυπο. Αυτό το πρότυπο χρησιμοποιεί επανάληψη για να δημιουργήσετε την προτεινόμενη πέντε λογαριασμούς χώρου αποθήκευσης, όπου είναι αποθηκευμένες οι το λειτουργικό σύστημα δίσκων και διαγνωστικών δεδομένων. Αυτό το σύνολο των λογαριασμών μπορούν να υποστηρίξουν έως 100 εικονικές μηχανές σε ένα σύνολο κλίμακα, η οποία είναι το τρέχον μέγιστο μέγεθος. Κάθε λογαριασμό χώρου αποθήκευσης ονομάζεται με ένα γράμμα προσδιορισμού που ορίστηκε στο τις μεταβλητές σε συνδυασμό με το πρόθεμα που παρέχετε στις παραμέτρους για το πρότυπο.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Προσθέστε τον πόρο εικονικού δικτύου. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Υπηρεσία παροχής πόρων δικτύου](../virtual-network/resource-groups-networking.md).

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "subnet1",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },

6. Προσθέστε τους δημόσια πόρους διεύθυνση IP που χρησιμοποιούνται από την εξισορρόπηση φόρτου και το περιβάλλον εργασίας δικτύου.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP1')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName1')]"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP2')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName2')]"
            }
          }
        },

7. Προσθέστε τον πόρο εξισορρόπησης φόρτου που χρησιμοποιείται από το σύνολο κλίμακα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Υποστήριξη διαχείρισης πόρων Azure για εξισορρόπηση φόρτου](../load-balancer/load-balancer-arm.md).

        {
          "apiVersion": "2015-06-15",
          "name": "[variables('loadBalancerName')]",
          "type": "Microsoft.Network/loadBalancers",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
          ],
          "properties": {
            "frontendIPConfigurations": [
              {
                "name": "loadBalancerFrontEnd",
                "properties": {
                  "publicIPAddress": {
                    "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
                  }
                }
              }
            ],
            "backendAddressPools": [ { "name": "bepool1" } ],
            "inboundNatPools": [
              {
                "name": "natpool1",
                "properties": {
                  "frontendIPConfiguration": {
                    "id": "[variables('frontEndIPConfigID')]"
                  },
                  "protocol": "tcp",
                  "frontendPortRangeStart": 50000,
                  "frontendPortRangeEnd": 50500,
                  "backendPort": 3389
                }
              }
            ]
          }
        },

8. Προσθήκη πόρου δικτύου του περιβάλλοντος εργασίας που χρησιμοποιείται από την ξεχωριστή εικονική μηχανή. Γιατί μηχανές σε ένα σύνολο κλίμακα δεν είναι προσβάσιμα μέσω μια δημόσια διεύθυνση IP, μια ξεχωριστή εικονική μηχανή δημιουργείται στο ίδιο δίκτυο εικονικού για απομακρυσμένη πρόσβαση οι υπολογιστές.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
            "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
                  },
                  "subnet": {
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                  }
                }
              }
            ]
          }
        },

9. Προσθέστε την ξεχωριστή εικονική μηχανή στο ίδιο δίκτυο με το σύνολο κλίμακα.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_A1" },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "[concat(parameters('resourcePrefix'), 'os1')]",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
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
            }
          }
        },

10. Προσθέστε την κλίμακα εικονική μηχανή ρύθμιση πόρων και ορίστε την επέκταση διαγνωστικά που είναι εγκατεστημένη στον όλες οι εικονικές μηχανές του συνόλου κλίμακα. Πολλές από τις ρυθμίσεις για αυτόν τον πόρο είναι παρόμοια με τον πόρο εικονική μηχανή. Οι κύριες διαφορές είναι το στοιχείο χωρητικότητας που καθορίζει τον αριθμό των εικονικές μηχανές στο σύνολο κλίμακα και upgradePolicy που καθορίζει τον τρόπο ενημερωμένες εκδόσεις μετατρέπονται σε εικονικές μηχανές. Το σύνολο της κλίμακας δεν δημιουργείται μέχρι να δημιουργούνται όλους τους λογαριασμούς του χώρου αποθήκευσης που ορίζονται με το στοιχείο dependsOn.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
              ],
              "sku": {
                "name": "Standard_A1",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              },
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "MicrosoftWindowsServer",
                      "offer": "WindowsServer",
                      "sku": "2012-R2-Datacenter",
                      "version": "latest"
                    }
                  },
                  "osProfile": {
                    "computerNamePrefix": "[parameters('vmSSName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                  },
                  "networkProfile": {
                    "networkInterfaceConfigurations": [
                      {
                        "name": "networkconfig1",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                                },
                                "loadBalancerBackendAddressPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                                  }
                                ],
                                "loadBalancerInboundNatPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                                  }
                                ]
                              }
                            }
                          ]
                        }
                      }
                    ]
                  },
                  "extensionProfile": {
                    "extensions": [
                      {
                        "name": "Microsoft.Insights.VMDiagnosticsSettings",
                        "properties": {
                          "publisher": "Microsoft.Azure.Diagnostics",
                          "type": "IaaSDiagnostics",
                          "typeHandlerVersion": "1.5",
                          "autoUpgradeMinorVersion": true,
                          "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                            "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                          },
                          "protectedSettings": {
                            "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11. Προσθέστε τον πόρο autoscaleSettings που καθορίζει τον τρόπο το σύνολο κλίμακα προσαρμόζεται με βάση τη χρήση επεξεργαστή σε υπολογιστές του συνόλου κλίμακα.

            {
              "type": "Microsoft.Insights/autoscaleSettings",
              "apiVersion": "2015-04-01",
              "name": "[concat(parameters('resourcePrefix'),'as1')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              ],
              "properties": {
                "enabled": true,
                "name": "[concat(parameters('resourcePrefix'),'as1')]",
                "profiles": [
                  {
                    "name": "Profile1",
                    "capacity": {
                      "minimum": "1",
                      "maximum": "10",
                      "default": "1"
                    },
                    "rules": [
                      {
                        "metricTrigger": {
                          "metricName": "\\Processor(_Total)\\% Processor Time",
                          "metricNamespace": "",
                          "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                          "timeGrain": "PT1M",
                          "statistic": "Average",
                          "timeWindow": "PT5M",
                          "timeAggregation": "Average",
                          "operator": "GreaterThan",
                          "threshold": 50.0
                        },
                        "scaleAction": {
                          "direction": "Increase",
                          "type": "ChangeCount",
                          "value": "1",
                          "cooldown": "PT5M"
                        }
                      }
                    ]
                  }
                ],
                "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              }
            }

    Για αυτό το πρόγραμμα εκμάθησης, αυτές οι τιμές είναι σημαντικές:

    - **metricName** - αυτή η τιμή είναι το ίδιο με το μετρητή επιδόσεων που ορίσαμε στη μεταβλητή wadperfcounter. Χρησιμοποιώντας αυτήν τη μεταβλητή, συλλέγει την επέκταση Διαγνωστικά του **Επεξεργαστής(_Σύνολο)\% χρόνου επεξεργαστή** μετρητή.
    - **metricResourceUri** - αυτή η τιμή είναι το αναγνωριστικό πόρου του συνόλου κλίμακα εικονική μηχανή.
    - **timeGrain** – αυτή η τιμή είναι το επίπεδο λεπτομερειών της τα μετρικά που έχουν συλλεχθεί. Σε αυτό το πρότυπο έχει ρυθμιστεί να ένα λεπτό.
    - **στατιστική τιμή** – αυτή η τιμή καθορίζει τον τρόπο τα μετρικά συνδυάζονται για να χωρέσει την αυτόματη ενέργεια κλίμακας. Οι πιθανές τιμές είναι: Μέσος όρος, ελάχιστο, μέγιστο. Σε αυτό το πρότυπο, το μέσο συνολικό η χρήση της CPU τις εικονικές μηχανές που συλλέγονται.
    - **timeWindow** – αυτή η τιμή είναι η περιοχή του χρόνου στο οποίο συλλέγονται δεδομένα της παρουσίας. Πρέπει να είναι μεταξύ 5 λεπτά και 12 ώρες.
    - **timeAggregation** – αυτή η τιμή που καθορίζει τον τρόπο θα πρέπει να συνδυάσετε τα δεδομένα που συλλέγονται μέσα στο χρόνο. Η προεπιλεγμένη τιμή είναι μέσο όρο. Οι πιθανές τιμές είναι: Μέσος όρος, ελάχιστο, μέγιστο, τελευταία, σύνολο, πλήθος.
    - **τελεστής** – αυτή η τιμή είναι ο τελεστής που χρησιμοποιείται για να συγκρίνετε τα δεδομένα μετρικό και το όριο. Οι πιθανές τιμές είναι: ισούται με, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    - **όριο** – αυτή η τιμή είναι η τιμή που ενεργοποιεί την ενέργεια κλίμακα. Σε αυτό το πρότυπο μηχανές προστίθεται η κλίμακα Ορισμός όταν η χρήση της CPU μέσος όρος μεταξύ των μηχανές του συνόλου είναι μεγαλύτερη από 50%.
    - **κατεύθυνση** – αυτή η τιμή καθορίζει την ενέργεια που πραγματοποιείται όταν το όριο είναι δυνατό. Οι πιθανές τιμές είναι αύξηση ή μείωση. Σε αυτό το πρότυπο, τον αριθμό των εικονικές μηχανές του συνόλου κλίμακα αυξάνεται εάν το όριο είναι μεγαλύτερη από 50% στο του καθορισμένου χρονικού διαστήματος.
    - **Τύπος** – αυτή η τιμή είναι τον τύπο της ενέργειας που πρέπει να πραγματοποιείται και πρέπει να έχει οριστεί σε ChangeCount.
    - **τιμή** – αυτή η τιμή είναι ο αριθμός των εικονικές μηχανές που έχουν προστεθεί ή καταργηθεί από το σύνολο κλίμακα. Αυτή η τιμή πρέπει να είναι 1 ή μεγαλύτερη. Η προεπιλεγμένη τιμή είναι 1. Σε αυτό το πρότυπο, τον αριθμό των μηχανές στην κλίμακα ορίστε αυξάνεται κατά 1 όταν ικανοποιείται το όριο.
    - **cooldown** – αυτή η τιμή είναι το χρονικό διάστημα για να περιμένετε από την τελευταία ενέργεια κλίμακας πριν από την επόμενη ενέργεια. Αυτή η τιμή πρέπει να είναι μεταξύ ένα λεπτό και μία εβδομάδα.

12. Αποθηκεύστε το αρχείο προτύπου.    

## <a name="step-4-upload-the-template-to-storage"></a>Βήμα 4: Αποστολή του προτύπου χώρου αποθήκευσης

Το πρότυπο μπορεί να αποσταλεί με την προϋπόθεση ότι γνωρίζετε το όνομα και το πρωτεύον κλειδί του λογαριασμού χώρου αποθήκευσης που δημιουργήσατε στο βήμα 1.

1.  Στο παράθυρο του Microsoft Azure PowerShell, ορίστε μια μεταβλητή που καθορίζει το όνομα του λογαριασμού χώρου αποθήκευσης που δημιουργήσατε στο βήμα 1.

            $storageAccountName = "vmstestsa"

2.  Ορίστε μια μεταβλητή που καθορίζει το πρωτεύον κλειδί του λογαριασμού χώρου αποθήκευσης.

            $storageAccountKey = "<primary-account-key>"

    Μπορείτε να λάβετε αυτό το κλειδί, κάνοντας κλικ στο εικονίδιο του κλειδιού κατά την προβολή του πόρου λογαριασμού χώρου αποθήκευσης στην πύλη του Azure.

3.  Δημιουργήστε το αντικείμενο περιβάλλοντος λογαριασμού χώρου αποθήκευσης που χρησιμοποιείται για την επικύρωση λειτουργίες με το λογαριασμό χώρου αποθήκευσης.

            $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

4.  Δημιουργία του κοντέινερ για την αποθήκευση του προτύπου.

            $containerName = "templates"
            New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob

5.  Αποστείλετε το αρχείο προτύπου στο νέο κοντέινερ.

            $blobName = "VMSSTemplate.json"
            $fileName = "C:\" + $BlobName
            Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx

## <a name="step-5-deploy-the-template"></a>Βήμα 5: Ανάπτυξη του προτύπου

Τώρα που έχετε δημιουργήσει το πρότυπο, μπορείτε να ξεκινήσετε την ανάπτυξη τους πόρους. Χρησιμοποιήστε αυτή την εντολή για να ξεκινήσετε τη διαδικασία:

    New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"

Όταν πατάτε το πλήκτρο enter, θα σας ζητηθεί να παράσχετε τιμές για τις μεταβλητές που ανατεθεί. Δώστε αυτές τις τιμές:

    vmName: vmsstestvm1
      vmSSName: vmsstest1
      instanceCount: 5
      adminUserName: vmadmin1
      adminPassword: VMpass1
      resourcePrefix: vmsstest

Θα χρειαστεί περίπου 15 λεπτά για όλους τους πόρους που θα αναπτυχθεί με επιτυχία.

>[AZURE.NOTE] Μπορείτε επίσης να χρησιμοποιήσετε τη δυνατότητα της πύλης για να αναπτύξετε τους πόρους. Χρησιμοποιήστε αυτήν τη σύνδεση: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"

## <a name="step-6-monitor-resources"></a>Βήμα 6: Παρακολούθηση πόρων

Μπορείτε να λάβετε ορισμένες πληροφορίες σχετικά με τα σύνολα κλίμακα εικονική μηχανή χρησιμοποιώντας τις παρακάτω μεθόδους:

 - Πύλη του Azure - μπορείτε να τη συγκεκριμένη στιγμή λάβετε ένα περιορισμένο χρονικό πληροφοριών με την πύλη.
 - Η [Εξερεύνηση πόρων Azure](https://resources.azure.com/) - αυτό το εργαλείο είναι η καλύτερη για την Εξερεύνηση την τρέχουσα κατάσταση του συνόλου κλίμακα. Ακολουθήστε αυτήν τη διαδρομή και θα πρέπει να δείτε την προβολή παρουσία του συνόλου κλίμακας που δημιουργήσατε:

        subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure PowerShell - Χρησιμοποιήστε αυτή την εντολή για να λάβετε ορισμένες πληροφορίες:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

        Or

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView

 - Σύνδεση με την ξεχωριστή εικονική μηχανή ακριβώς όπως θα κάνατε με οποιοδήποτε άλλο υπολογιστή και, στη συνέχεια, μπορείτε να αποκτήσετε απομακρυσμένη πρόσβαση τις εικονικές μηχανές της κλίμακας ρύθμιση για την παρακολούθηση μεμονωμένων διεργασιών.

>[AZURE.NOTE] Μπορείτε να βρείτε μια πλήρη REST API για τη λήψη πληροφοριών σχετικά με τα σύνολα κλίμακα στο [Ορίζει κλίμακα εικονική μηχανή](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>Βήμα 7: Κατάργηση των πόρων

Επειδή που θα χρεωθείτε για τους πόρους που χρησιμοποιούνται στο Azure, είναι πάντα καλό να διαγράψετε τους πόρους που δεν χρειάζονται πλέον. Δεν χρειάζεται να διαγράψετε κάθε πόρο ξεχωριστά από μια ομάδα πόρων. Μπορείτε να διαγράψετε την ομάδα των πόρων και όλους τους πόρους διαγράφονται αυτόματα.

    Remove-AzureRmResourceGroup -Name vmsstestrg1

Εάν θέλετε να διατηρήσετε την ομάδα πόρων, μπορείτε να διαγράψετε την κλίμακα ορίστε μόνο.

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"

## <a name="next-steps"></a>Επόμενα βήματα

- Διαχείριση του συνόλου κλίμακας που μόλις δημιουργήσατε χρησιμοποιώντας τις πληροφορίες σε [εικονικές μηχανές σε ένα σύνολο κλίμακα εικονική μηχανή Διαχείριση](virtual-machine-scale-sets-windows-manage.md).
- Μάθετε περισσότερα σχετικά με κατακόρυφη κλιμάκωση ανατρέχοντας [Κατακόρυφη autoscale με σύνολα κλίμακα εικονική μηχανή](virtual-machine-scale-sets-vertical-scale-reprovision.md)
- Βρείτε παραδείγματα της οθόνης Azure παρακολούθησης στο [Azure οθόνη PowerShell γρήγορης έναρξης δείγματα](../monitoring-and-diagnostics/insights-powershell-samples.md)
- Μάθετε περισσότερα σχετικά με δυνατότητες ειδοποίηση χρησιμοποιήσετε [autoscale ενέργειες για να στείλετε μηνύματα ηλεκτρονικού ταχυδρομείου και webhook οι ειδοποιήσεις στην οθόνη Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
- Μάθετε πώς μπορείτε να [καταγράφει χρήση ελέγχου για να στείλετε μηνύματα ηλεκτρονικού ταχυδρομείου και webhook οι ειδοποιήσεις στην οθόνη Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
