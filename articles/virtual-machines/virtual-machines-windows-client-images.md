<properties
   pageTitle="Χρήση εικόνων προγράμματος-πελάτη των Windows για σενάρια ανάπτυξης/έλεγχο | Microsoft Azure"
   description="Πώς μπορείτε να χρησιμοποιήσετε τα πλεονεκτήματα της συνδρομής του Visual Studio για την ανάπτυξη των Windows 7/8/10 στο Azure για προγραμματιστές/έλεγχο σενάρια"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="iainfou"/>

# <a name="using-windows-client-in-azure-for-devtest-scenarios"></a>Χρήση προγράμματος-πελάτη των Windows σε Azure για προγραμματιστές/έλεγχο σενάρια

Μπορείτε να χρησιμοποιήσετε τα Windows 7, Windows 8, ή 10 Windows Azure για σενάρια ανάπτυξης/έλεγχο αρκεί να έχετε μια κατάλληλη συνδρομή στο Visual Studio (πρώην MSDN). Σε αυτό το άρθρο περιγράφει οι απαιτήσεις καταλληλότητας για εκτελείται προγράμματος-πελάτη των Windows σε Azure και χρησιμοποιήστε το Azure συλλογή εικόνων.


## <a name="subscription-eligibility"></a>Καταλληλότητα συνδρομή
Ενεργό Visual Studio συνδρομητές (άτομα που έχετε αγοράσει μια άδεια χρήσης συνδρομής του Visual Studio) να χρησιμοποιήσετε Windows προγράμματος-πελάτη για ανάπτυξη και έλεγχο. Πρόγραμμα-πελάτη Windows μπορεί να χρησιμοποιηθεί σε το δικό σας υλικό και Azure εικονικές μηχανές εκτελούνται σε οποιονδήποτε τύπο Azure συνδρομής. Πρόγραμμα-πελάτη Windows μπορεί να δεν είναι αναπτυχθεί σε ή χρησιμοποιούνται σε Azure για χρήση κανονική παραγωγής ή χρησιμοποιούνται από άτομα που δεν είναι ενεργό συνδρομητές Visual Studio.

Για τη διευκόλυνσή σας, έχουμε κάνει ορισμένες εικόνες Windows 10 διαθέσιμη από τη συλλογή Azure μέσα σε [προσφέρει την κατάλληλη/έλεγχο](#eligible-offers). Visual Studio συνδρομητές μέσα σε οποιονδήποτε τύπο προσφοράς επίσης να [καταλλήλως προετοιμασία και δημιουργία](virtual-machines-windows-prepare-for-upload-vhd-image.md) 64-bit Windows 7, Windows 8, ή Windows 10 εικόνα και, στη συνέχεια, [αποστείλετε Azure](virtual-machines-windows-upload-image.md). Η χρήση παραμένει περιορίζεται σε αποκλίσεις/έλεγχο από ενεργό τους συνδρομητές του Visual Studio.


## <a name="eligible-offers"></a>Κατάλληλη προσφορών
Ο παρακάτω πίνακας παρουσιάζει την προσφορά αναγνωριστικών που πληρούν τις απαιτήσεις για την ανάπτυξη των Windows 10 μέσω της συλλογής Azure. Οι εικόνες Windows 10 εμφανίζονται μόνο οι παρακάτω προσφορές. Οπτική συνδρομητές Studio που χρειάζονται για την εκτέλεση του προγράμματος-πελάτη των Windows σε έναν τύπο διαφορετικό προσφορά απαιτεί να [καταλλήλως προετοιμάσετε και να δημιουργήσετε](virtual-machines-windows-prepare-for-upload-vhd-image.md) μια 64-bit των Windows 7, Windows 8 ή Windows 10 εικόνα και [, στη συνέχεια, αποστείλετε Azure](virtual-machines-windows-upload-image.md).

| Όνομα προσφορά | Αριθμός προσφορά | Εικόνες διαθέσιμη προγράμματος-πελάτη |
|:-----------|:------------:|:-----------------------:|
| [Pay-As-You-Go ανάπτυξης/έλεγχο](https://azure.microsoft.com/offers/ms-azr-0023p/)                          | 0023P | Windows 10 |
| [Οι συνδρομητές Visual Studio για μεγάλες επιχειρήσεις (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/)      | 0029P | Windows 10 |
| [Οι συνδρομητές Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)          | 0059P | Windows 10 |
| [Οι συνδρομητές Visual Studio δοκιμής Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)     | 0060P | Windows 10 |
| [Visual Studio Premium με MSDN (όφελος)](https://azure.microsoft.com/offers/ms-azr-0061p/)       | 0061P | Windows 10 |
| [Οι συνδρομητές Visual Studio για μεγάλες επιχειρήσεις](https://azure.microsoft.com/offers/ms-azr-0063p/)            | 0063P | Windows 10 |
| [Οι συνδρομητές Visual Studio για μεγάλες επιχειρήσεις (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/) | 0064P | Windows 10 |
| [Για μεγάλες επιχειρήσεις ανάπτυξης/έλεγχο](https://azure.microsoft.com/ofers/ms-azr-0148p/)                              | 0148P | Windows 10 |


## <a name="check-your-azure-subscription"></a>Επιλέξτε τη συνδρομή σας στο Azure
Εάν δεν γνωρίζετε το Αναγνωριστικό προσφορά, μπορείτε να την αποκτήσετε από το Azure πύλη ή την πύλη του λογαριασμού.

Το Αναγνωριστικό προσφορά εγγραφής σημειώνεται στην το blade 'Συνδρομές' εντός της πύλης Azure:

![Λεπτομέρειες Αναγνωριστικό προσφορά από την πύλη του Azure](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

Μπορείτε επίσης να προβάλετε το Αναγνωριστικό προσφορά από την [καρτέλα 'Συνδρομές'](http://account.windowsazure.com/Subscriptions) της πύλης λογαριασμός Azure:

![Λεπτομέρειες Αναγνωριστικό προσφορά από την πύλη λογαριασμός Azure](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 


## <a name="next-steps"></a>Επόμενα βήματα
Μπορείτε να αναπτύξετε το ΣΠΣ με τη χρήση [του PowerShell](virtual-machines-windows-ps-create.md), [πρότυπα διαχείρισης πόρων](virtual-machines-windows-ps-template.md)ή [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
