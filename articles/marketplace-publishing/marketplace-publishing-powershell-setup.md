<properties
   pageTitle="Ρύθμιση του PowerShell για να δημιουργήσετε μια Εικονική για αγορά του | Microsoft Azure"
   description="Οδηγίες για τη ρύθμιση του Azure PowerShell και η χρήση της ως προαιρετικό διεργασία ροής για να δημιουργήσετε Εικονική εικόνες για ανάπτυξη και να πουλήσετε σε το Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/04/2016"
   ms.author="hascipio"/>

# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Ρύθμιση του Azure PowerShell για να δημιουργήσετε μια προσφορά για το Azure Marketplace
Για λεπτομερείς πληροφορίες σχετικά με τη ρύθμιση του PowerShell στο Azure, δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md). Μια απλή προσέγγιση είναι να χρησιμοποιήσετε τη μέθοδο πιστοποιητικού, η οποία πραγματοποιεί λήψη και εισάγει ένα πιστοποιητικό που απαιτείται για τον έλεγχο ταυτότητας. Για να αποκτήσετε το πιστοποιητικό απαιτείται, χρησιμοποιήστε το cmdlet **Get-AzurePublishSettingsFile** . Όταν σας ζητηθεί, αποθηκεύστε το αρχείο. Για να εισαγάγετε το πιστοποιητικό σε μια περίοδο λειτουργίας PowerShell, χρησιμοποιήστε το cmdlet **AzurePublishSettingsFile εισαγωγής** .

Για να ρυθμίσετε τις παραμέτρους και να αποθηκεύσετε τις κοινές ρυθμίσεις Microsoft Azure συνδρομή για την περίοδο λειτουργίας PowerShell, χρησιμοποιήστε το cmdlet **Set-AzureSubscription** και **Επιλέξτε AzureSubscription** :

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Η πρώτη εντολή συσχετίζει έναν προεπιλεγμένο λογαριασμό χώρου αποθήκευσης με τη συνδρομή (χρειάζεται για κάποιες εργασίες προετοιμασίας Εικονική).  Η δεύτερη κάνει τη συνδρομή το τρέχον αρχείο (αναγνωρίζεται από άλλα cmdlet του).

## <a name="see-also"></a>Δείτε επίσης
- [Γρήγορα αποτελέσματα: πώς μπορείτε να δημοσιεύσετε μια προσφορά για να το Azure Marketplace](marketplace-publishing-getting-started.md)
- [Δημιουργία μιας εικόνας εικονική μηχανή για το Marketplace](marketplace-publishing-vm-image-creation.md)
