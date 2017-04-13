<properties
    pageTitle="Τι είναι οι κλίμακα Εικονική ορίζει; | Microsoft Azure"
    description="Μάθετε σχετικά με τα σύνολα κλίμακα Εικονική."
    keywords="Ορίζει κλίμακα εικονική μηχανή Linux εικονικό υπολογιστή," 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/24/2016"
    ms.author="gatneil"/>

# <a name="what-are-virtual-machine-scale-sets"></a>Τι είναι οι κλίμακα εικονική μηχανή ορίζει;

Εικονική μηχανή κλίμακα σύνολα σάς επιτρέπουν να διαχειρίζεστε πολλά ΣΠΣ ως σύνολο. Σε υψηλό επίπεδο, σύνολα κλίμακα έχουν τα ακόλουθα πλεονεκτήματα και μειονεκτήματα:

Οι επαγγελματίες τεχνολογιών πληροφορικής:

1. Υψηλή διαθεσιμότητα. Κάθε σύνολο κλίμακα τοποθετεί το ΣΠΣ σε ένα σύνολο διαθεσιμότητα με 5 σφαλμάτων τομείς (FDs) και 5 ενημέρωση τομείς (UDs) για να βεβαιωθείτε ότι διαθεσιμότητας (για περισσότερες πληροφορίες για FDs και UDs, ανατρέξτε στο θέμα [διαθεσιμότητα Εικονική](./virtual-machines-linux-manage-availability.md)). 
2. Εύκολη ενοποίηση με το Azure εξισορρόπηση φόρτου και εφαρμογή πύλης.
3. Εύκολη ενοποίηση με το Azure Autoscale.
4. Ανάπτυξη, διαχείριση, απλοποιημένα και εκκαθάριση του ΣΠΣ.
5. Υποστηρίζει κοινές μπορεί να των Windows και το Linux είναι, καθώς και προσαρμοσμένες εικόνες.

Τα μειονεκτήματα:

1. Δεν είναι δυνατό να επισυνάψετε δίσκων δεδομένων Εικονική παρουσίες σε ένα σύνολο κλίμακα. Αντί για αυτό, πρέπει να χρησιμοποιήσετε χώρο αποθήκευσης αντικειμένων Blob, αρχεία Azure, πίνακες Azure ή άλλη λύση χώρου αποθήκευσης.

## <a name="quick-create-using-azure-cli"></a>Γρήγορη-δημιουργία χρησιμοποιώντας Azure CLI

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Για γενικές πληροφορίες, ανατρέξτε στο θέμα στη [σελίδα κύριο προορισμού για σύνολα κλίμακα](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

Για περισσότερες τεκμηρίωση, ελέγξτε την [τεκμηρίωση κύρια σελίδα για την κλίμακα σύνολα](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Για παράδειγμα πρότυπα διαχείρισης πόρων χρησιμοποιώντας κλίμακα σύνολα, πραγματοποιήστε αναζήτηση για "vmss" στο τα [πρότυπα γρήγορη έναρξη Azure github repo](https://github.com/Azure/azure-quickstart-templates).

