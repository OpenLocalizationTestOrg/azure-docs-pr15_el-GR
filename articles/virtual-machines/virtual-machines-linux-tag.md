<properties
   pageTitle="Πώς μπορείτε να προσθέσετε ετικέτες σε μια εικονική μηχανή Linux | Microsoft Azure"
   description="Μάθετε περισσότερα σχετικά με την προσθήκη ετικετών σε μια εικονική μηχανή Linux που έχουν δημιουργηθεί με Azure χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Πώς μπορείτε να προσθέσετε ετικέτες σε μια εικονική μηχανή Linux στο Azure

Σε αυτό το άρθρο περιγράφει διαφορετικούς τρόπους για να προσθέσετε ετικέτα μια εικονική μηχανή Linux στο Azure μέσω του μοντέλου ανάπτυξης διαχείρισης πόρων. Οι ετικέτες είναι ζεύγη που ορίζονται από το χρήστη κλειδιού/τιμής που μπορούν να τοποθετηθούν απευθείας σε έναν πόρο ή μια ομάδα πόρων. Azure υποστηρίζει επί του παρόντος έως 15 ετικέτες ανά πόρο και ομάδα πόρων. Οι ετικέτες μπορεί να είναι τοποθετείται σε έναν πόρο κατά τη δημιουργία του ή να προστεθεί σε έναν υπάρχοντα πόρο. Σημειώστε, ετικέτες που υποστηρίζονται για τους πόρους που δημιουργήθηκε μέσω μόνο το μοντέλο ανάπτυξης διαχείρισης πόρων.

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Προσθήκη ετικετών με Azure CLI

Για να ξεκινήσετε, [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-azure-resource-manager.md) και βεβαιωθείτε ότι βρίσκεστε σε λειτουργία διαχείρισης πόρων (`azure config mode arm`).

Μπορείτε να προβάλετε όλες τις ιδιότητες για μια δεδομένη εικονική μηχανή, συμπεριλαμβανομένων των ετικετών, χρήση αυτής της εντολής:

        azure vm show -g MyResourceGroup -n MyTestVM

Για να προσθέσετε μια νέα ετικέτα Εικονική μέσω του Azure CLI, μπορείτε να χρησιμοποιήσετε το `azure vm set` εντολής μαζί με την παράμετρο ετικέτα **-t**:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Για να καταργήσετε όλες τις ετικέτες, μπορείτε να χρησιμοποιήσετε την παράμετρο **– T** στο το `azure vm set` εντολή.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Τώρα που έχουμε εφαρμόσει ετικέτες για να μας πόρων Azure CLI και την πύλη, ας ρίξουμε μια ματιά τις λεπτομέρειες χρήσης για να δείτε τις ετικέτες στην πύλη του χρέωσης.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Επόμενα βήματα

* Για να μάθετε περισσότερα σχετικά με την προσθήκη ετικετών σε Azure τους πόρους σας, ανατρέξτε στο θέμα [Επισκόπηση διαχείρισης πόρων Azure][] και [Χρήση ετικετών για να οργανώσετε τους πόρους σας Azure][].
* Για να δείτε πώς οι ετικέτες μπορούν να σας βοηθήσουν να διαχειριστείτε τη χρήση του Azure πόρους, ανατρέξτε στο θέμα [Κατανόηση της χρέωσής σας Azure][] και να [αποκτήσει ιδέες σε σας Microsoft Azure κατανάλωση πόρων][].





[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Azure Επισκόπηση της διαχείρισης πόρων]: ../azure-resource-manager/resource-group-overview.md
[Χρήση ετικετών για να οργανώσετε τους πόρους σας Azure]: ../resource-group-using-tags.md
[Κατανόηση της χρέωσής σας Azure]: ../billing/billing-understand-your-bill.md
[Ενίσχυση ιδέες σε την κατανάλωση πόρων Microsoft Azure]: ../billing-usage-rate-card-overview.md
