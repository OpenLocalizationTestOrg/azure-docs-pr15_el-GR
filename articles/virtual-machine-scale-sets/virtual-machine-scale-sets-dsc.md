<properties
   pageTitle="Χρήση επιθυμητοί κατάσταση ρύθμισης παραμέτρων με σύνολα κλίμακα εικονική μηχανή | Microsoft Azure"
   description="Χρήση κλίμακας εικονική μηχανή ορίζει με την επέκταση DSC Azure"
   services="virtual-machine-scale-sets"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machine-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Χρήση κλίμακας εικονική μηχανή ορίζει με την επέκταση DSC Azure

[Εικονική μηχανή κλίμακα σύνολα (VMSS)](virtual-machine-scale-sets-overview.md) μπορεί να χρησιμοποιηθεί με το πρόγραμμα χειρισμού επέκταση [Ρύθμισης παραμέτρων κατάσταση επιθυμητοί Azure (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md) . VMSS παρέχει ένας τρόπος για να αναπτύξετε και να διαχειριστείτε μεγάλου αριθμού εικονικές μηχανές και να elastically κλίμακα και σμίκρυνση απάντηση για τη φόρτωση. DSC χρησιμοποιείται για να ρυθμίσετε τις παραμέτρους του ΣΠΣ καθώς και σε σύνδεση, ώστε να λειτουργούν με το λογισμικό παραγωγής.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Διαφορές μεταξύ της για την ανάπτυξη στον Εικονική και VMSS

Το υποκείμενο δομή προτύπου για VMSS είναι λίγο διαφορετικά από ένα μεμονωμένο Εικονική. Συγκεκριμένα, ένα μεμονωμένο Εικονική αναπτύσσει επεκτάσεις κάτω από τον κόμβο "virtualMachines". Υπάρχει μια καταχώρηση τύπου "επεκτάσεις" όπου DSC προστίθεται στο πρότυπο

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Έναν κόμβο VMSS περιλαμβάνει μια ενότητα "Ιδιότητες" με το "VirtualMachineProfile", "extensionProfile" χαρακτηριστικό. DSC προστίθεται στην περιοχή "επεκτάσεις"

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
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

## <a name="behavior-for-vmss"></a>Συμπεριφορά για VMSS

Η συμπεριφορά για VMSS είναι παρόμοια με τη συμπεριφορά για μια μεμονωμένη Εικονική. Όταν δημιουργείται μια νέα Εικονική, παρέχεται αυτόματα με την επέκταση DSC. Εάν μια πιο πρόσφατη έκδοση του WMF απαιτείται από την επέκταση, η Εικονική επανεκκίνηση πριν σύντομα online. Όταν είναι online, στοιχεία λήψης του DSC .zip ρύθμισης παραμέτρων και την προμήθεια στην η Εικονική. Μπορείτε να βρείτε περισσότερες λεπτομέρειες στην [Επισκόπηση Azure DSC επέκτασης](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md).

## <a name="next-steps"></a>Επόμενα βήματα ##
Εξετάστε το [πρότυπο διαχείρισης πόρων Azure για την επέκταση DSC](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md).

Μάθετε πώς η [επέκταση DSC χειρίζεται με ασφάλεια τα διαπιστευτήρια](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md). 

Για περισσότερες πληροφορίες σχετικά με το πρόγραμμα χειρισμού επέκταση Azure DSC, ανατρέξτε στο θέμα [Εισαγωγή στο πρόγραμμα χειρισμού επέκταση ρύθμισης παραμέτρων κατάσταση επιθυμητοί Azure](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md). 

Για περισσότερες πληροφορίες σχετικά με το PowerShell DSC, [επισκεφθείτε το Κέντρο τεκμηρίωση του PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 


