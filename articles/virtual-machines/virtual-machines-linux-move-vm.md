<properties
    pageTitle="Μετακίνηση μια Εικονική Linux | Microsoft Azure"
    description="Μετακίνηση μια Εικονική Linux άλλο Azure συνδρομή ή την ομάδα πόρων στο μοντέλο ανάπτυξης διαχείρισης πόρων."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Μετακίνηση μια Εικονική Linux σε μια άλλη συνδρομή ή την ομάδα πόρων

Σε αυτό το άρθρο σάς καθοδηγεί σε πώς μπορείτε να μετακινήσετε μια Εικονική Linux μεταξύ των ομάδων πόρων ή συνδρομών. Μετακίνηση μια Εικονική μεταξύ συνδρομών μπορεί να είναι χρήσιμες, εάν έχετε δημιουργήσει μια Εικονική σε μια συνδρομή για προσωπική χρήση και τώρα θέλετε να μετακινήσετε σε συνδρομής της εταιρείας σας.

> [AZURE.NOTE] Νέα αναγνωριστικά πόρου δημιουργούνται ως μέρος της μετακίνησης. Όταν η Εικονική έχει μετακινηθεί, πρέπει να ενημερώσετε το εργαλεία και χρησιμοποιήστε το νέο πόρο αναγνωριστικών των δεσμών ενεργειών. 


## <a name="use-the-azure-cli-to-move-a-vm"></a>Χρησιμοποιήστε το CLI Azure για να μετακινήσετε μια εικονική Μηχανή 

Για να μετακινήσετε με επιτυχία μια Εικονική, πρέπει να μετακινήσετε την εικονική Μηχανή και όλους τους πόρους υποστήριξης. Χρησιμοποιήστε την εντολή **Εμφάνιση azure ομάδας** για μια λίστα όλων των πόρων σε μια ομάδα πόρων και τα αναγνωριστικά τους. Βοηθά στη διοχέτευση το αποτέλεσμα αυτής της εντολής σε ένα αρχείο, ώστε να μπορείτε να αντιγράψετε και να επικολλήσετε τα αναγνωριστικά σε νεότερη έκδοση εντολές.

    azure group show <resourceGroupName>

Για να μετακινήσετε μια Εικονική και τους πόρους του σε μια άλλη ομάδα πόρων, χρησιμοποιήστε την εντολή CLI **Μετακίνηση azure πόρων** . Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να μετακινήσετε μια Εικονική και τους πιο κοινούς πόρους απαιτεί. Χρησιμοποιούμε την παράμετρο **-i** και μεταβιβάζουν σε μια λίστα διαχωρισμένες με κόμματα (χωρίς κενά διαστήματα) των αναγνωριστικών για τους πόρους για να μετακινήσετε.

    
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      
    
    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
    
Εάν θέλετε να μετακινήσετε την εικονική Μηχανή και τους πόρους σε μια διαφορετική συνδρομή, προσθέστε το **--προορισμού subscriptionId & #60; destinationSubscriptionID & #62;** παραμέτρου για να καθορίσετε τη συνδρομή προορισμού.

Εάν εργάζεστε από τη γραμμή εντολών σε υπολογιστή με Windows, πρέπει να προσθέσετε μια **$** μπροστά από τα ονόματα των μεταβλητών κατά τη δήλωση τους. Αυτό δεν είναι απαραίτητο στο Linux.

Σας ζητηθεί να επιβεβαιώσετε ότι θέλετε να μετακινήσετε το συγκεκριμένο πόρο. Πληκτρολογήστε **Y** για να επιβεβαιώσετε ότι θέλετε να μετακινήσετε τους πόρους.
    

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να μετακινήσετε πολλούς διαφορετικούς τύπους πόρων μεταξύ των ομάδων πόρων και συνδρομών. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Μετακίνηση πόρων για νέα ομάδα πόρων ή τη συνδρομή](../resource-group-move-resources.md).    