<properties
   pageTitle="Σύνταξη από κοινού πρότυπα με επεκτάσεις Εικονική Windows | Microsoft Azure"
   description="Μάθετε περισσότερα σχετικά με τη σύνταξη από κοινού από διαχειριστή πόρων Azure πρότυπα με επεκτάσεις για ΣΠΣ των Windows"
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

# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure με επεκτάσεις Εικονική των Windows

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Από το Azure PowerShell, εκτελέστε το ακόλουθο cmdlet του PowerShell Azure:

      Get-AzureVMAvailableExtension


Αυτό το cmdlet επιστρέφει το όνομα του publisher, επέκταση ονόματος και έκδοση ως εξής:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Αυτές οι τρεις ιδιότητες αντιστοιχίστε "publisher", "Τύπος" και "typeHandlerVersion" αντίστοιχα στο τμήμα προτύπου παραπάνω.

>[AZURE.NOTE]Πάντα συνιστάται να χρησιμοποιείτε την πιο πρόσφατη έκδοση επέκταση για να λάβετε την πιο ενημερωμένη λειτουργικότητα.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Εντοπισμός του σχήματος για τη ρύθμιση παραμέτρων επέκτασης

Το επόμενο βήμα με σύνταξη από κοινού ένα πρότυπο επέκταση είναι για να προσδιορίσετε τη μορφή για την παροχή παραμέτρους. Κάθε επέκταση υποστηρίζει το δικό της σύνολο παραμέτρων.

Για να δείτε τις ρυθμίσεις παραμέτρων δείγμα για τις επεκτάσεις των Windows, ανατρέξτε στο θέμα [δείγματα επεκτάσεις των Windows](virtual-machines-windows-extensions-configuration-samples.md).


Ανατρέξτε τα εξής για να λάβετε ένα πλήρως ολοκλήρωσης πρότυπο με επεκτάσεις Εικονική.

[Επέκταση προσαρμοσμένων δεσμών ενεργειών σε μια Εικονική Windows](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)


Μετά τη σύνταξη από κοινού το πρότυπο, μπορείτε να αναπτύξετε χρησιμοποιώντας Azure PowerShell.
