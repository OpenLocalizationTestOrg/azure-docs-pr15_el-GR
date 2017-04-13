<properties 
    pageTitle="Ανάπτυξη QuickBooks στο Azure RemoteApp | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να κάνετε κοινή χρήση QuickBooks με Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Πώς μπορώ να αναπτύξω QuickBooks στο Azure RemoteApp;

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Χρησιμοποιήστε τις ακόλουθες πληροφορίες για την κοινή χρήση QuickBooks ως εφαρμογής στο Azure RemoteApp.


Μπορείτε να μοιραστείτε QuickBooks για μεγάλες επιχειρήσεις 2015 με Azure RemoteApp στη συλλογή μια υβριδική ή στο cloud. Το αρχείο εταιρεία πρέπει να βρίσκονται σε μια Εικονική που εκτελεί QuickBooks διακομιστή της βάσης δεδομένων που είναι ξεχωριστό από τους διακομιστές Azure RemoteApp. Ποτέ να αποθηκεύσετε το αρχείο εταιρεία στην εικόνα σας Azure RemoteApp - αναμένεται απώλεια δεδομένων εάν το κάνετε αυτό. Μόνο για μεγάλες επιχειρήσεις QuickBooks υποστηρίζει φιλοξενίας το αρχείο QuickBooks σε μια εξωτερική κοινή χρήση με το διακομιστή της βάσης δεδομένων QuickBooks προσβάσιμα μέσω τυπική δικτύωσης των Windows.   

> [AZURE.IMPORTANT] Ο διακομιστής βάσης δεδομένων QuickBooks που φιλοξενεί το αρχείο της εταιρείας πρέπει να βρίσκονται σε ένα ξεχωριστό Εικονική εντός του ίδιου VNET ως τη συλλογή Azure RemoteApp.  

## <a name="steps-to-deploy-quickbooks"></a>Βήματα για την ανάπτυξη του QuickBooks

1. Δημιουργήστε μια Εικονική Azure και εγκαταστήσετε QuickBooks, QuickBooks διακομιστή βάσης δεδομένων, και τοποθετήστε το αρχείο σε μια Εικονική Azure της εταιρείας.  Βεβαιωθείτε ότι έχετε ρυθμίσει σωστά κανόνες τείχους προστασίας.
2. Εγκατάσταση του QuickBooks σε μια [προσαρμοσμένη εικόνα](remoteapp-imageoptions.md) και να δημιουργήσετε μια [συλλογή Azure RemoteApp](remoteapp-collections.md), cloud ή υβριδικό, μέσα σε την ακριβή ίδιο VNET όπου βρίσκεται η Εικονική φιλοξενίας του διακομιστή βάσης δεδομένων QuickBooks με τα αρχεία εταιρείας. 
3.  [Δημοσίευση](remoteapp-publish.md) Εφαρμογή QuickBooks στους χρήστες
4.  Εκκίνηση του προγράμματος-πελάτη που φιλοξενείται στο Azure RemoteApp QuickBooks, μεταβείτε με τυπική Windows δικτύωσης για την εικονική Μηχανή φιλοξενίας του διακομιστή βάσης δεδομένων QuickBooks και ανοίξτε το αρχείο εταιρείας. 

## <a name="documentation-references"></a>Τεκμηρίωση αναφορές

- QuickBooks [υποστηριζόμενες ρυθμίσεις παραμέτρων](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
- QuickBooks [Επιλογές ανάπτυξης](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Μπορείτε επίσης να ανατρέξετε μου Ignite παρουσίαση, [τα θεμελιώδη στοιχεία του Microsoft Azure RemoteApp διαχείρισης και διαχείριση](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) - προώθηση ταινίας σε 1:02:45 για να μεταβείτε στο τμήμα QuickBooks.

## <a name="deployment-architecture"></a>Αρχιτεκτονική ανάπτυξης

![QuickBooks + Azure RemoteApp ανάπτυξης](./media/remoteapp-quickbooks/ra-quickbooks.png)