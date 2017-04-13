<properties
   pageTitle="Σύνταξη από κοινού πρότυπα με επεκτάσεις Εικονική Linux | Microsoft Azure"
   description="Μάθετε περισσότερα σχετικά με τη σύνταξη από κοινού από διαχειριστή πόρων Azure πρότυπα με επεκτάσεις για ΣΠΣ Linux"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Σύνταξη από κοινού από διαχειριστή πόρων Azure πρότυπα με επεκτάσεις Εικονική Linux

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Από το Azure CLI, εκτελέστε την ακόλουθη εντολή:

      Azure VM extension list

Αυτή η εντολή επιστρέφει το όνομα του publisher, όνομα επέκτασης και έκδοση ως εξής:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Αυτές οι τρεις ιδιότητες αντιστοιχίστε "publisher", "Τύπος" και "typeHandlerVersion" αντίστοιχα στο τμήμα προτύπου παραπάνω.

>[AZURE.NOTE]Πάντα συνιστάται να χρησιμοποιείτε την πιο πρόσφατη έκδοση επέκταση για να λάβετε την πιο ενημερωμένη λειτουργικότητα.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Εντοπισμός του σχήματος για τη ρύθμιση παραμέτρων επέκτασης

Το επόμενο βήμα με σύνταξη από κοινού ένα πρότυπο επέκταση είναι για να προσδιορίσετε τη μορφή για την παροχή παραμέτρους. Κάθε επέκταση υποστηρίζει το δικό της σύνολο παραμέτρων.

Για να δείτε τις ρυθμίσεις παραμέτρων δείγμα για τις επεκτάσεις Linux, κάντε κλικ στην τεκμηρίωση για να δείτε [τα δείγματα eExtensions Linux](virtual-machines-linux-extensions-configuration-samples.md).

Ανατρέξτε τα εξής για να λάβετε ένα πλήρως ολοκλήρωσης πρότυπο με επεκτάσεις Εικονική.

[Επέκταση προσαρμοσμένων δεσμών ενεργειών σε μια Εικονική Linux](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Μετά τη σύνταξη από κοινού το πρότυπο, μπορείτε να αναπτύξετε χρησιμοποιώντας το Azure CLI.
