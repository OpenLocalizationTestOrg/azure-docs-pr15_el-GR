<properties
   pageTitle="Αντιμετώπιση προβλημάτων αποτυχίες επέκταση Εικονική Windows | Microsoft Azure"
   description="Μάθετε περισσότερα σχετικά με την αντιμετώπιση προβλημάτων αποτυχίες επέκταση Εικονική Windows Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Αντιμετώπιση προβλημάτων αποτυχίες επέκταση Εικονική Windows Azure

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Προβολή της κατάστασης επέκτασης
Azure πρότυπα διαχείρισης πόρων μπορεί να εκτελεστεί από το Azure PowerShell. Όταν εκτελείται το πρότυπο, την κατάσταση επέκταση μπορούν να προβληθούν από την Εξερεύνηση πόρων Azure ή τα εργαλεία της γραμμής εντολών.

Ακολουθεί ένα παράδειγμα:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

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

## <a name="troubleshooting-extension-failures"></a>Αντιμετώπιση προβλημάτων αποτυχίες επέκτασης

### <a name="re-running-the-extension-on-the-vm"></a>Εκτέλεση εκ νέου την επέκταση σε την εικονική Μηχανή

Εάν εκτελείτε δέσμες ενεργειών σε την εικονική Μηχανή χρησιμοποιώντας επέκταση προσαρμοσμένη δέσμη ενεργειών, μπορεί να αντιμετωπίσετε μερικές φορές ένα σφάλμα όπου Εικονική δημιουργήθηκε με επιτυχία, αλλά απέτυχε η δέσμη ενεργειών. Στην περιοχή αυτές τις συνθήκες, το συνιστώμενο τρόπο για την ανάκτηση από αυτό το σφάλμα είναι να καταργήσετε την επέκταση και εκτελέστε ξανά το πρότυπο ξανά.
Σημείωση: στο μέλλον, αυτή η λειτουργία μπορεί να βελτιωθούν για να καταργήσετε την ανάγκη για την κατάργηση της επέκτασης.


#### <a name="remove-the-extension-from-azure-powershell"></a>Κατάργηση της επέκτασης από Azure PowerShell

    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Όταν έχει καταργηθεί η επέκταση, το πρότυπο μπορεί να είναι εκτελεστεί εκ νέου για να εκτελέσετε τις δέσμες ενεργειών σε η Εικονική.
