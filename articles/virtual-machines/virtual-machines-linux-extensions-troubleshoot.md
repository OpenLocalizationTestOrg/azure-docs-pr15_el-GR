<properties
   pageTitle="Αντιμετώπιση προβλημάτων αποτυχίες επέκταση Εικονική Linux | Microsoft Azure"
   description="Μάθετε περισσότερα σχετικά με την αντιμετώπιση προβλημάτων αποτυχίες επέκταση Εικονική Linux Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Αντιμετώπιση προβλημάτων αποτυχίες επέκταση Εικονική Linux Azure

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Προβολή της κατάστασης επέκτασης
Azure πρότυπα διαχείρισης πόρων μπορεί να εκτελεστεί από το Azure CLI. Όταν εκτελείται το πρότυπο, την κατάσταση επέκταση μπορούν να προβληθούν από την Εξερεύνηση πόρων Azure ή τα εργαλεία της γραμμής εντολών.

Ακολουθεί ένα παράδειγμα:

Azure CLI:

      azure vm get-instance-view


Εδώ είναι το αποτέλεσμα του δείγματος:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extenson-failures"></a>Αντιμετώπιση προβλημάτων Extenson αποτυχίες:

### <a name="re-running-the-extension-on-the-vm"></a>Εκτέλεση εκ νέου την επέκταση σε την εικονική Μηχανή

Εάν εκτελείτε δέσμες ενεργειών σε την εικονική Μηχανή χρησιμοποιώντας επέκταση προσαρμοσμένη δέσμη ενεργειών, μπορεί να αντιμετωπίσετε μερικές φορές ένα σφάλμα όπου Εικονική δημιουργήθηκε με επιτυχία, αλλά απέτυχε η δέσμη ενεργειών. Στην περιοχή αυτές τις συνθήκες, το συνιστώμενο τρόπο για την ανάκτηση από αυτό το σφάλμα είναι να καταργήσετε την επέκταση και εκτελέστε ξανά το πρότυπο ξανά.
Σημείωση: στο μέλλον, αυτή η λειτουργία μπορεί να βελτιωθούν για να καταργήσετε την ανάγκη για την κατάργηση της επέκτασης.

#### <a name="remove-the-extension-from-azure-cli"></a>Κατάργηση της επέκτασης από Azure CLI

      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

Όπου "publsher-όνομα" αντιστοιχεί στον τύπο επέκταση από το αποτέλεσμα της "azure εικονική get-παρουσία-προβολή" και όνομα είναι το όνομα του πόρου επέκταση από το πρότυπο

Όταν έχει καταργηθεί η επέκταση, το πρότυπο μπορεί να είναι εκτελεστεί εκ νέου για να εκτελέσετε τις δέσμες ενεργειών σε η Εικονική.
