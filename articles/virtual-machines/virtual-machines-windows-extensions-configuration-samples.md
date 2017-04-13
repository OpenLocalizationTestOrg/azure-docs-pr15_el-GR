<properties
   pageTitle="Δείγμα ρύθμισης παραμέτρων για τις επεκτάσεις Εικονική Windows | Microsoft Azure"
   description="Δείγμα ρύθμισης παραμέτρων για τη σύνταξη προτύπων με επεκτάσεις"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="azure-windows-vm-extension-configuration-samples"></a>Δείγματα ρύθμισης παραμέτρων επέκταση Εικονική Azure Windows

> [AZURE.SELECTOR]
- [PowerShell - προτύπου](virtual-machines-windows-extensions-configuration-samples.md)
- [CLI - προτύπου](virtual-machines-linux-extensions-configuration-samples.md)

<br>

Σε αυτό το άρθρο παρέχει δείγμα ρύθμισης παραμέτρων για τη ρύθμιση παραμέτρων Azure Εικονική επεκτάσεις για Windows ΣΠΣ.

Για να μάθετε περισσότερα σχετικά με αυτές τις επεκτάσεις, ανατρέξτε στο θέμα [Azure Εικονική επεκτάσεις Επισκόπηση.](virtual-machines-windows-extensions-features.md)

Για να μάθετε περισσότερα σχετικά με τη σύνταξη από κοινού επέκταση προτύπων, ανατρέξτε στο θέμα [σύνταξης πρότυπα επέκταση.](virtual-machines-windows-extensions-authoring-templates.md)

Σε αυτό το άρθρο παραθέτει αναμενόμενο ρύθμισης παραμέτρων τιμές για ορισμένες από τις επεκτάσεις των Windows.

## <a name="sample-template-snippet-for-vm-extensions-with-iaas-vms"></a>Δείγμα τμήματος προτύπου για τις επεκτάσεις Εικονική με IaaS ΣΠΣ.
Το πρότυπο τμήμα κώδικα για την ανάπτυξη επεκτάσεις μοιάζει ως εξής:

      {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "MyExtension",
      "apiVersion": "2015-05-01-preview",
      "location": "[parameters('location')]",
      "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
      "properties":
      {
      "publisher": "Publisher Namespace",
      "type": "extension Name",
      "typeHandlerVersion": "extension version",
      "autoUpgradeMinorVersion":true,
      "settings": {
      // Extension specific configuration goes in here.
      }
      }
      }

## <a name="sample-template-snippet-for-vm-extensions-with-vm-scale-sets"></a>Δείγμα τμήματος κώδικα πρότυπο για τις επεκτάσεις Εικονική με Εικονική κλίμακα σύνολα.

    {
     "type":"Microsoft.Compute/virtualMachineScaleSets",
    ....
           "extensionProfile":{
           "extensions":[
             {
               "name":"extension Name",
               "properties":{
                 "publisher":"Publisher Namespace",
                 "type":"extension Name",
                 "typeHandlerVersion":"extension version",
                 "autoUpgradeMinorVersion":true,
                 "settings":{
                 // Extension specific configuration goes in here.
                 }
               }
              }
            }
          }

Πριν από την ανάπτυξη την επέκταση Ελέγξτε την πιο πρόσφατη έκδοση επέκταση και αντικαταστήστε το "typeHandlerVersion" με την τρέχουσα πιο πρόσφατη έκδοση.

Υπόλοιπο αυτού του άρθρου παρέχει δείγμα ρυθμίσεις παραμέτρων για τις επεκτάσεις Εικονική των Windows.

Πριν από την ανάπτυξη την επέκταση Ελέγξτε την πιο πρόσφατη έκδοση επέκταση και αντικαταστήστε το "typeHandlerVersion" με την τρέχουσα πιο πρόσφατη έκδοση.

### <a name="customscript-extension-14"></a>Επέκταση CustomScript 1.4.
      {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.4",
          "settings": {
              "fileUris": [
                  "http: //Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
              ],
              "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -start.ps1"
          },
          "protectedSettings": {
            "storageAccountName": "yourStorageAccountName",
            "storageAccountKey": "yourStorageAccountKey"
          }
      }

#### <a name="parameter-description"></a>Παράμετρος περιγραφή:

- fileUris: λίστα ψηφίων κόμμα διευθύνσεις URL των αρχείων που θα ληφθούν από την επέκταση σε η Εικονική. Λήψη αρχείων δεν εάν τίποτα έχει καθοριστεί. Εάν τα αρχεία που βρίσκονται στο χώρο αποθήκευσης Azure, το fileURLs μπορεί να επισημανθεί ως ιδιωτικό και το correspoding storageAccountName και storageAccountKey μπορούν να περάσουν ως ιδιωτικό παραμέτρους για να αποκτήσετε πρόσβαση σε αυτά τα αρχεία.
- commandToExecute: [υποχρεωτική παράμετρος]: Αυτή είναι η εντολή που θα εκτελεστεί από την επέκταση.
- storageAccountName: [προαιρετική παράμετρος]: όνομα λογαριασμού χώρου αποθήκευσης για την πρόσβαση του fileURLs, εάν έχουν σημανθεί ως ιδιωτικά.
- storageAccountKey: [προαιρετική παράμετρος]: κλειδί λογαριασμού χώρου αποθήκευσης για την πρόσβαση του fileURLs, εάν έχουν σημανθεί ως ιδιωτικά.

### <a name="customscript-extension-17"></a>Επέκταση CustomScript 1.7.

Ανατρέξτε στην CustomScript έκδοση 1.4 για περιγραφή παραμέτρου. Η έκδοση 1.7 εισάγει την υποστήριξη για την αποστολή parameters(commandToExecute) δέσμης ενεργειών ως protectedSettings, οπότε θα είναι κρυπτογραφημένες πριν από την αποστολή. η παράμετρος 'commandToExecute' μπορεί να καθοριστεί είτε στο ρυθμίσεις ή protectedSettings, αλλά όχι και στις δύο.

        {
            "publisher": "Microsoft.Compute",
            "type": "CustomScriptExtension",
            "typeHandlerVersion": "1.7",
            "settings": {
                "fileUris": [
                    "http: //Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
                ],
                "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -start.ps1"
            },
            "protectedSettings": {
              "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -start.ps1",
              "storageAccountName": "yourStorageAccountName",
              "storageAccountKey": "yourStorageAccountKey"
            }
        }

### <a name="vmaccess-extension"></a>Επέκταση VMAccess.

      {
          "publisher": "Microsoft.Compute",
          "type": "VMAccessAgent",
          "typeHandlerVersion": "2.0",
          "settings": {
            "UserName" : "New User Name"
          },
          "protectedSettings": {
            "Password" : "New Password"
          }
      }

### <a name="dsc-extension"></a>Επέκταση DSC.
      {
          "publisher": "Microsoft.Powershell",
          "type": "DSC",
          "typeHandlerVersion": "2.1(Recommendation is to use the latest version)",
          "settings": {
              "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
              "SasToken": "Optional : SAS Token if ModulesUrl points to Azure Blob Storage",
              "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
              "Properties": {
                  "ParameterToConfigurationFunction1": "Value1",
                  "ParameterToConfigurationFunction2": "Value2",
                  "ParameterOfTypePSCredential1": {
                      "UserName": "UsernameValue1",
                      "Password": "PrivateSettingsRef:Key1(Value is a reference to a member of the Items object in the protected settings)"
                  },
                  "ParameterOfTypePSCredential2": {
                      "UserName": "UsernameValue2",
                      "Password": "PrivateSettingsRef:Key2"
                  }
              }
          },
          "protectedSettings": {
              "Items": {
                  "Key1": "PasswordValue1",
                  "Key2": "PasswordValue2"
              },
              "DataBlobUri": "optional : https: //UrlToConfigurationData.psd1"
          }
      }


### <a name="symantec-endpoint-protection"></a>Προστασία Symantec τελικού σημείου.
      {
        "publisher": "SymantecEndpointProtection",
        "type": "Symantec",
        "typeHandlerVersion": "12.1",
        "settings": {}
      }

### <a name="trend-micro-deep-security-agent"></a>Ο παράγοντας Micro πολλά επίπεδα ασφαλείας τάσης.
      {
        "publisher": "TrendMicro.DeepSecurity",
        "type": "TrendMicroDSA",
        "typeHandlerVersion": "9.6",
        "settings": {
          "ManagerAddress" : "Enter the externally accessible DNS name or IP address of the Deep Security Manager. Please enter \"agents.deepsecurity.trendmicro.com\" if using Deep Security as a Service",

          "ActivationPort" : "Enter the port number of the Deep Security Manager, default value - 443",

          "TenantIdentifier" : "Enter the tenant ID, which is a hyphenated, 36-character string available in the Deployment Scripts dialog box in the Deep Security console. This parameter is mandatory if using Deep Security as a Service, or a multi-tenant installation of Deep Security Manager. Type NA if using a non multi-tenant installation of Deep Security Manager.",

          "TenantActivationPassword" : "Enter the tenant activation password, which is a hyphenated, 36-character string available in the Deployment Scripts dialog box in the Deep Security console. This parameter is mandatory if using Deep Security as a Service, or a multi-tenant installation of Deep Security Manager. Type NA if using a non multi-tenant installation of Deep Security Manager.",

          "SecurityPolicy" : "Optional : Enter the name or numeric ID of the security policy defined in the Deep Security Manager which will be applied on agent activation to protect this virtual machine (recommended). No security policy will be applied to the virtual machine if this parameter is blank. This parameter is optional if using Deep Security as a Service."
        }
      }

### <a name="vormertric-transparent-encryption-agent"></a>Παράγοντας διαφανή κρυπτογράφησης Vormertric.
            {
              "publisher": "Vormetric",
              "type": "VormetricTransparentEncryptionAgent",
              "typeHandlerVersion": "5.2",
              "settings": {
              }
            }

### <a name="puppet-enterprise-agent"></a>Παράγοντας puppet για μεγάλες επιχειρήσεις.
            {
              "publisher": "PuppetLabs",
              "type": "PuppetEnterpriseAgent",
              "typeHandlerVersion": "3.2",
              "settings": {
                "puppet_master_server" : "Puppet Master Server Name"
              }
            }  

### <a name="microsoft-monitoring-agent-for-azure-operational-insights"></a>Παρακολούθηση παράγοντας για Azure λειτουργικές ιδέες Microsoft
            {
              "publisher": "Microsoft.EnterpriseCloud.Monitoring",
              "type": "MicrosoftMonitoringAgent",
              "typeHandlerVersion": "1.0",
              "settings": {
                "workspaceId" : "The Workspace ID is available from within the Direct Agent Configuration section of the Azure Operational Insights portal"
              }
              "protectedSettings": {
                "workspaceKey"  : "The Workspace Key is a string that is available from within the Direct Agent Configuration section of the Azure Operational Insights portal"
              }
              }
            }

### <a name="mcafee-endpointsecurity"></a>McAfee EndpointSecurity
            {
              "publisher": "McAfee.EndpointSecurity",
              "type": "McAfeeEndpointSecurity",
              "typeHandlerVersion": "6.0",
              "settings": {
                "entitlementKey" : "Optional : Enter a valid entitlement key or leave blank for trial version",
                "featureVS"      : "Choose whether or not to install the Virus and Spyware Protection features : true|false",
                "featureBP"      : "Choose whether or not to install the Browser Protection feature : true|false",
                "featureFW"      : "Choose whether or not to install the Firewall Protection feature :true|false",
                "relayServer"    : "Allows VMs on the local subnet to receive updates through this VM when they are not connected to the internet : true|false"
              }
            }

### <a name="azure-iaas-antimalware"></a>Azure λογισμικό κακόβουλης λειτουργίας IaaS
          {
            "publisher": "Microsoft.Azure.Security",
            "type": "IaaSAntimalware",
            "typeHandlerVersion": "1.2",
            "settings": {
              "AntimalwareEnabled": "true",
              "ExclusionsPaths"        : "Optional : ExclusionsPaths",
              "ExclusionsExtensions"   : "Optional : ExclusionsExtensions",
              "ExclusionsProcesses"   : "Optional : ExclusionsProcesses",
              "RealtimeProtectionEnabled"   : "Optional : True|False",
              "ScheduledScanSettingsIsEnabled"   : "Optional : True|False",
              "ScheduledScanSettingsScanType"   : "Optional : Quick|Full",
              "ScheduledScanSettingsDay"   : "Optional : Sunday-Saturday",
              "ScheduledScanSettingsTime"   : "Optional : When to perform the scheduled scan, measured in minutes from midnight,0-1440"
            }
          }

### <a name="eset-file-security"></a>Επαναφορά αρχείων ασφαλείας
          {
            "publisher": "ESET",
            "type": "FileSecurity",
            "typeHandlerVersion": "6.0",
            "settings": {
            }
          }

### <a name="datadog-agent"></a>Παράγοντας Datadog
          {
            "publisher": "Datadog.Agent",
            "type": "DatadogWindowsAgent",
            "typeHandlerVersion": "0.5",
            "settings": {
              "api_key" : "API Key from https://app.datadoghq.com/account/settings#api"
            }
          }

### <a name="confer-advanced-threat-prevention-and-incident-response-for-azure"></a>Παρέχει αποτροπή απειλή για προχωρημένους και απόκριση περιστατικού για Azure
          {
            "publisher": "Confer",
            "type": "ConferForAzure",
            "typeHandlerVersion": "1.0",
            "settings": {
              "ConferRegisterCode" : "Optional : Valid product registration code or leave it blank to register later",
              "ConferRegisterCode" : "Enter a valid server name if your account requires a dedicated confer backend server or leave it blank"
            }
          }

### <a name="cloudlink-securevm-agent"></a>Παράγοντας SecureVM CloudLink
          {
            "publisher": "CloudLinkEMC.SecureVM",
            "type": "CloudLinkSecureVMWindowsAgent",
            "typeHandlerVersion": "4.0",
            "settings": {
              "CloudLinkCenter" : "specify valid IP/FQDN to CloudLinkCenter"
            }
          }

### <a name="barracuda-vpn-connectivity-agent-for-microsoft-azure"></a>Παράγοντας συνδεσιμότητας VPN barracuda για το Microsoft Azure
          {
            "publisher": "Barracuda.Azure.ConnectivityAgent",
            "type": "BarracudaConnectivityAgent",
            "typeHandlerVersion": "3.5",
            "settings": {
              "ServerAddress" : "Host name or IP address of the VPN server - AES, AES256, Blowfish,CAST,DES,3DES,None",
              "EncryptionAlgorithm" : "Algorithm used to encrypt VPN traffic - MD5,SHA1,SHA256,None",
              "PKCS12File" : "Url for file containing certificate and private key used to authenticate against the VPN server",
              "PKCS12FilePassword" : "Password for the file containing certificate and private key"
            }
          }

### <a name="alert-logic-log-manager"></a>Λογική καταγραφής Διαχείριση ειδοποιήσεων
          {
            "publisher": "AlertLogic.Extension",
            "type": "AlertLogicLM",
            "typeHandlerVersion": "1.9",
            "settings": {
              "registrationKey" : " Alert Logic Log Manager registration key"
            }
          }

### <a name="chef-agent"></a>Παράγοντας Chef
          {
            "publisher": "Chef.Bootstrap.WindowsAzure",
            "type": "ChefClient",
            "typeHandlerVersion": "1210.12",
            "settings": {
              "validation_key" : " Validation key",
              "client_rb" : "client_rb file",
              "runlist" : "Optional runlist"
            }
          }

### <a name="azure-diagnostics"></a>Διαγνωστικά του Azure

Για περισσότερες λεπτομέρειες σχετικά με τον τρόπο ρύθμισης παραμέτρων Διαγνωστικά, ανατρέξτε στο θέμα [Επέκταση Διαγνωστικά του Azure](virtual-machines-windows-extensions-diagnostics-template.md)

          {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "xmlCfg": "[base64(variables('wadcfgx'))]",
              "storageAccount": "[parameters('diagnosticsStorageAccount')]"
            },
            "protectedSettings": {
            "storageAccountName": "[parameters('diagnosticsStorageAccount')]",
            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
            "storageAccountEndPoint": "https://core.windows.net"
          }
          }

Στα παραπάνω παραδείγματα, αντικαταστήστε τον αριθμό έκδοσης με την πιο πρόσφατη αριθμό έκδοσης.

Ακολουθεί ένα παράδειγμα της πλήρους Εικονική πρότυπο με επέκταση προσαρμοσμένη δέσμη ενεργειών.

[Επέκταση προσαρμοσμένων δεσμών ενεργειών σε μια Εικονική Windows](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)
