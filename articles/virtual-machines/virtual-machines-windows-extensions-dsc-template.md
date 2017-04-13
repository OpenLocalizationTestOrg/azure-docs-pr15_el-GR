<properties
   pageTitle="Επιθυμητοί πρότυπο διαχείρισης πόρων κατάσταση ρύθμισης παραμέτρων | Microsoft Azure"
   description="Ορισμός προτύπου για τη διαχείριση πόρων για ρύθμιση παραμέτρων επιθυμητοί κατάσταση στο Azure με παραδείγματα και αντιμετώπισης προβλημάτων"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>VMSS των Windows και τη ρύθμιση παραμέτρων επιθυμητοί κατάσταση με τα πρότυπα για τη διαχείριση πόρων Azure
Σε αυτό το άρθρο περιγράφει το πρότυπο διαχείρισης πόρων για το πρόγραμμα [χειρισμού επέκταση επιθυμητοί παραμέτρων κατάσταση](virtual-machines-windows-extensions-dsc-overview.md). 

## <a name="template-example-for-a-windows-vm"></a>Παράδειγμα προτύπου για μια Εικονική των Windows

Το παρακάτω τμήμα κώδικα τίθεται σε ενότητα πόρων του προτύπου.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## <a name="template-example-for-windows-vmss"></a>Παράδειγμα προτύπου για VMSS των Windows

Έναν κόμβο VMSS περιλαμβάνει μια ενότητα "Ιδιότητες" με το "VirtualMachineProfile", "extensionProfile" χαρακτηριστικό. DSC προστίθεται στην περιοχή "επεκτάσεις". 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="detailed-settings-information"></a>Λεπτομερείς ρυθμίσεις πληροφορίες

Το παρακάτω σχήμα είναι για το τμήμα ρυθμίσεις της επέκτασης Azure DSC σε ένα πρότυπο από διαχειριστή πόρων Azure.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Λεπτομέρειες
| Όνομα ιδιότητας | Τύπος | Περιγραφή |
| --- | --- | --- |
| settings.wmfVersion | συμβολοσειρά | Καθορίζει την έκδοση του Windows Management Framework που πρέπει να εγκατασταθούν στην Εικονική σας. Ρύθμιση αυτής της ιδιότητας για 'πιο πρόσφατη' εγκαταστάσεις την πιο ενημερωμένη έκδοση του WMF. Οι μόνο τα τρέχοντα πιθανές τιμές για αυτήν την ιδιότητα είναι **'4.0', '5.0', ' 5.0PP' και 'τελευταία'**. Αυτές οι πιθανές τιμές υπόκεινται ενημερώσεις. Η προεπιλεγμένη τιμή είναι 'πιο πρόσφατη'.|
| Settings.Configuration.URL | συμβολοσειρά | Καθορίζει τη θέση διεύθυνσης URL από την οποία θα κάνετε λήψη αρχείο zip παραμέτρων DSC. Εάν η διεύθυνση URL που παρέχεται απαιτεί ένα διακριτικό συσχετισμών Ασφαλείας για πρόσβαση, πρέπει να ορίσετε την ιδιότητα protectedSettings.configurationUrlSasToken της τιμής του το διακριτικό συσχετισμών Ασφαλείας. Αυτή η ιδιότητα απαιτείται εάν ορίζονται settings.configuration.script ή/και settings.configuration.function. |
| Settings.Configuration.Script | συμβολοσειρά | Καθορίζει το όνομα του αρχείου της δέσμης ενεργειών που περιέχει τον ορισμό των παραμέτρων σας DSC. Αυτή η δέσμη ενεργειών πρέπει να είναι στον ριζικό φάκελο από το αρχείο zip που έχουν ληφθεί από τη διεύθυνση URL που καθορίζεται από την ιδιότητα configuration.url. Αυτή η ιδιότητα απαιτείται εάν ορίζονται settings.configuration.url ή/και settings.configuration.script. |
| Settings.Configuration.Function | συμβολοσειρά | Καθορίζει το όνομα των παραμέτρων σας DSC. Η ρύθμιση παραμέτρων με το όνομα πρέπει να περιλαμβάνονται στη δέσμη ενεργειών που ορίζονται από configuration.script. Αυτή η ιδιότητα απαιτείται εάν ορίζονται settings.configuration.url ή/και settings.configuration.function. |
| settings.configurationArguments | Συλλογή | Καθορίζει τις παραμέτρους που θα θέλατε να μεταβιβάζουν ρυθμίσεις παραμέτρων σας DSC. Αυτή η ιδιότητα δεν είναι κρυπτογραφημένο. |
| settings.configurationData.url | συμβολοσειρά | Καθορίζει τη διεύθυνση URL από την οποία θέλετε να κάνετε λήψη του αρχείου δεδομένων (.pds1) ρύθμισης παραμέτρων για να χρησιμοποιήσετε ως είσοδο για τη ρύθμιση παραμέτρων DSC. Εάν η διεύθυνση URL που παρέχεται απαιτεί ένα διακριτικό συσχετισμών Ασφαλείας για πρόσβαση, πρέπει να ορίσετε την ιδιότητα protectedSettings.configurationDataUrlSasToken της τιμής του το διακριτικό συσχετισμών Ασφαλείας.|
| settings.privacy.dataEnabled | συμβολοσειρά | Ενεργοποιεί ή απενεργοποιεί τηλεμετρίας συλλογής. Οι μόνο πιθανές τιμές για αυτήν την ιδιότητα είναι **"Ενεργοποίηση", "Απενεργοποίηση", ", ή $null**. Κλείσετε αυτήν την ιδιότητα κενά ή null επιτρέπει τηλεμετρίας. Η προεπιλεγμένη τιμή είναι ''. [Περισσότερες πληροφορίες](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Συλλογή | Καθορίζει εναλλακτικές θέσεις από το οποίο θέλετε να κάνετε λήψη του WMF. [Περισσότερες πληροφορίες](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Συλλογή | Καθορίζει τις παραμέτρους θέλετε να μεταβιβάσετε στη ρύθμιση παραμέτρων σας DSC. Αυτή η ιδιότητα είναι κρυπτογραφημένο. |
| protectedSettings.configurationUrlSasToken | συμβολοσειρά | Καθορίζει το διακριτικό συσχετισμών Ασφαλείας για να αποκτήσετε πρόσβαση στη διεύθυνση URL που ορίζονται από configuration.url. Αυτή η ιδιότητα είναι κρυπτογραφημένο. |
| protectedSettings.configurationDataUrlSasToken | συμβολοσειρά | Καθορίζει το διακριτικό συσχετισμών Ασφαλείας για να αποκτήσετε πρόσβαση στη διεύθυνση URL που ορίζονται από configurationData.url. Αυτή η ιδιότητα είναι κρυπτογραφημένο. |

## <a name="settings-vs-protectedsettings"></a>Ρυθμίσεις έναντι ProtectedSettings
Όλες οι ρυθμίσεις αποθηκεύονται σε ένα αρχείο κειμένου ρυθμίσεις η Εικονική.
Ιδιότητες στην περιοχή "Ρυθμίσεις" είναι δημόσιες ιδιότητες, επειδή δεν είναι κρυπτογραφημένα στο αρχείο κειμένου ρυθμίσεις.
Ιδιότητες στην περιοχή 'protectedSettings' είναι κρυπτογραφημένα με ένα πιστοποιητικό και δεν εμφανίζονται σε μορφή απλού κειμένου σε αυτό το αρχείο κατά την εικονική Μηχανή.

Εάν η ρύθμιση παραμέτρων χρειάζονται διαπιστευτήρια, μπορούν να συμπεριληφθούν στο protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα χρησιμοποιεί που προέρχονται από την ενότητα "Γρήγορα αποτελέσματα" στη [σελίδα Επισκόπηση πρόγραμμα χειρισμού επέκταση DSC](virtual-machines-windows-extensions-dsc-overview.md).
Αυτό το παράδειγμα χρησιμοποιεί πρότυπα διαχείρισης πόρων αντί για το cmdlet για να αναπτύξετε την επέκταση. Αποθήκευση των παραμέτρων "IisInstall.ps1", τοποθετήστε το σε μια. ΣΥΜΠΊΕΣΗ αρχείου και αποστείλετε το αρχείο σε μια προσβάσιμη διεύθυνση URL. Αυτό το παράδειγμα χρησιμοποιεί χώρο αποθήκευσης αντικειμένων blob του Azure, αλλά είναι δυνατή η λήψη. Αρχεία από οποιαδήποτε θέση αυθαίρετο ZIP.

Στο πρότυπο Azure διαχείριση πόρων, ο ακόλουθος κώδικας καθοδηγεί το Εικονική για λήψη το σωστό αρχείο και την εκτέλεση της κατάλληλης συνάρτησης του PowerShell:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Ενημέρωση από την προηγούμενη μορφή
Τις ρυθμίσεις στην προηγούμενη μορφή (που περιέχει τις δημόσιες ιδιότητες ModulesUrl, ConfigurationFunction, SasToken ή ιδιότητες) αυτόματα προσαρμόσετε στην τρέχουσα μορφή και να εκτελέσετε ακριβώς όπως πριν.

Η παρακάτω διάταξη είναι τι το προηγούμενο ρυθμίσεις σχήμα ήταν:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
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
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Παρακάτω περιγράφεται ο τρόπος την προηγούμενη μορφή προσαρμόζεται στην τρέχουσα μορφή:

| Όνομα ιδιότητας | Προηγούμενη ισοδύναμο σχήματος |
| --- | --- |
| settings.wmfVersion | ρυθμίσεις. WMFVersion |
| Settings.Configuration.URL | ρυθμίσεις. ModulesUrl |
| Settings.Configuration.Script | Πρώτο μέρος των ρυθμίσεων. ConfigurationFunction (πριν από την '\\\\') |
| Settings.Configuration.Function | Δεύτερο μέρος των ρυθμίσεων. ConfigurationFunction (αφού '\\\\') |
| settings.configurationArguments | ρυθμίσεις. Ιδιότητες |
| settings.configurationData.url | protectedSettings.DataBlobUri (χωρίς διακριτικό συσχετισμών Ασφαλείας) |
| settings.privacy.dataEnabled | ρυθμίσεις. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | ρυθμίσεις. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | ρυθμίσεις. SasToken |
| protectedSettings.configurationDataUrlSasToken | Συσχετίσεις Ασφαλείας διακριτικού από protectedSettings.DataBlobUri |


## <a name="troubleshooting---error-code-1100"></a>Αντιμετώπιση προβλημάτων - κωδικός σφάλματος 1100
Κωδικός σφάλματος 1100 υποδηλώνει ότι υπάρχει πρόβλημα με την εισαγωγή από το χρήστη για την επέκταση DSC.
Το κείμενο από αυτά τα σφάλματα είναι μεταβλητή και ενδέχεται να αλλάξουν.
Εδώ θα βρείτε ορισμένα από τα σφάλματα που μπορεί να αντιμετωπίσετε και πώς μπορείτε να τα διορθώσετε.

### <a name="invalid-values"></a>Μη έγκυρες τιμές
"Είναι Privacy.dataCollection '{0}'. Οι πιθανές μόνο τιμές είναι '' "Ενεργοποίηση" και "Απενεργοποίηση" ""είναι WmfVersion '{0}'. Μόνο οι πιθανές τιμές είναι... και 'τελευταία' "

Πρόβλημα: Μια τιμή που δεν επιτρέπεται.

Λύση: Αλλάξτε τη μη έγκυρη τιμή σε μια έγκυρη τιμή. Ανατρέξτε στον πίνακα στην ενότητα λεπτομέρειες.

### <a name="invalid-url"></a>Μη έγκυρη διεύθυνση URL
"Είναι ConfigurationData.url '{0}'. Δεν είναι μια έγκυρη διεύθυνση URL""είναι DataBlobUri '{0}'. Δεν είναι μια έγκυρη διεύθυνση URL""είναι Configuration.url '{0}'. Δεν είναι μια έγκυρη διεύθυνση URL"

Πρόβλημα: A που παρέχονται URL δεν είναι έγκυρο.

Λύση: Επιλέξτε όλες τις διευθύνσεις URL που παρέχονται. Βεβαιωθείτε ότι όλες οι διευθύνσεις URL επιλύεται έγκυρες θέσεις ότι η επέκταση πρόσβαση στον απομακρυσμένο υπολογιστή.

### <a name="invalid-configurationargument-type"></a>Μη έγκυρο τύπο ConfigurationArgument
"Δεν είναι έγκυρη configurationArguments πληκτρολογήστε {0}"

Πρόβλημα: Δεν μπορεί να επιλύσει την ιδιότητα ConfigurationArguments σε ένα αντικείμενο Hashtable. 

Λύση: Καταστήστε την ιδιότητα ConfigurationArguments ενός Hashtable. Ακολουθήστε τη μορφή που παρέχονται στο προηγούμενο παράδειγμα. Προσέχετε προσφορές, κόμματα και άγκιστρα.

### <a name="duplicate-configurationarguments"></a>Αναπαραγωγή ConfigurationArguments
"Βρέθηκε διπλότυπο ορίσματα '{0}' σε δημόσιες και προστατευμένη configurationArguments"

Πρόβλημα: Το ConfigurationArguments στις ρυθμίσεις δημόσια και το ConfigurationArguments στο ρυθμίσεων προστατευμένης περιέχουν ιδιότητες με το ίδιο όνομα.

Λύση: Καταργήστε μία από τις διπλότυπες ιδιότητες.

### <a name="missing-properties"></a>Ιδιότητες που λείπουν
"Configuration.function απαιτεί ότι έχει καθοριστεί configuration.url ή configuration.module"

"Configuration.url απαιτεί configuration.script που έχει καθοριστεί"

"Configuration.script απαιτεί configuration.url που έχει καθοριστεί"

"Configuration.url απαιτεί configuration.function που έχει καθοριστεί"

"ConfigurationUrlSasToken απαιτεί configuration.url που έχει καθοριστεί"

"ConfigurationDataUrlSasToken απαιτεί configurationData.url που έχει καθοριστεί"

Πρόβλημα: Μια καθορισμένη ιδιότητα χρειάζεται μια άλλη ιδιότητα που λείπει.

Λύσεις: 
- Δώστε την ιδιότητα που λείπουν.
- Καταργήστε την ιδιότητα που χρειάζεται η ιδιότητα που λείπουν.


## <a name="next-steps"></a>Επόμενα βήματα
Μάθετε περισσότερα σχετικά με τα σύνολα κλίμακα DSC και εικονική μηχανή στο [Χρησιμοποιώντας εικονική μηχανή κλίμακα σύνολα με την επέκταση DSC Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Δείτε περισσότερες λεπτομέρειες σχετικά με τη [Διαχείριση ασφαλούς διαπιστευτηρίων του DSC](virtual-machines-windows-extensions-dsc-credentials.md). 

Για περισσότερες πληροφορίες σχετικά με το πρόγραμμα χειρισμού επέκταση Azure DSC, ανατρέξτε στο θέμα [Εισαγωγή στο πρόγραμμα χειρισμού επέκταση ρύθμισης παραμέτρων κατάσταση επιθυμητοί Azure](virtual-machines-windows-extensions-dsc-overview.md). 

Για περισσότερες πληροφορίες σχετικά με το PowerShell DSC, [επισκεφθείτε το Κέντρο τεκμηρίωση του PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 
