<properties
    pageTitle="Αλλαγή μεγέθους ενός κλασική Εικονική Windows | Microsoft Azure"
    description="Αλλαγή μεγέθους μια εικονική μηχανή Windows που δημιουργήσατε στο μοντέλο κλασική ανάπτυξης, με χρήση του Powershell Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>


# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Αλλαγή μεγέθους μια Εικονική Windows που έχουν δημιουργηθεί με το μοντέλο κλασική ανάπτυξης

Αυτό το άρθρο παρουσιάζει τον τρόπο αλλαγής μεγέθους μια Εικονική Windows δημιουργήσατε στο μοντέλο κλασική ανάπτυξης χρησιμοποιώντας το Azure Powershell.

Όταν εξετάζετε τη δυνατότητα να αλλάξετε το μέγεθος μια Εικονική, υπάρχουν δύο έννοιες που ελέγχουν την περιοχή των μεγέθη που είναι διαθέσιμα για να αλλάξετε το μέγεθος η εικονική μηχανή. Την πρώτη έννοια είναι η περιοχή στην οποία έχει αναπτυχθεί το Εικονική. Η λίστα μεγέθη Εικονική είναι διαθέσιμα στην περιοχή είναι κάτω από την καρτέλα υπηρεσίες της ιστοσελίδας Azure περιοχές. Η δεύτερη έννοια είναι το φυσικό υλικό αυτήν τη στιγμή φιλοξενίας σας Εικονική. Φυσική διακομιστές που φιλοξενούν ΣΠΣ ομαδοποιούνται σε συμπλεγμάτων κοινές φυσικό υλικό. Η μέθοδος αλλαγής μεγέθους μια Εικονική διαφέρει ανάλογα με εάν το επιθυμητό μέγεθος νέα Εικονική υποστηρίζεται από το σύμπλεγμα υλικού φιλοξενίας αυτήν τη στιγμή η Εικονική.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Μπορείτε επίσης να [αλλάξετε το μέγεθος μια Εικονική που δημιουργήσατε στο μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-machines-windows-resize-vm.md).


## <a name="add-your-account"></a>Προσθέστε το λογαριασμό σας

Πρέπει να ρυθμίσετε Azure PowerShell για να εργαστείτε με κλασική Azure πόρους. Ακολουθήστε τα παρακάτω βήματα για να ρυθμίσετε τις παραμέτρους Azure PowerShell για τη Διαχείριση κλασική πόρων.

1. Στη γραμμή εντολών του PowerShell, πληκτρολογήστε `Add-AzureAccount` και πιέστε το πλήκτρο **Enter**. 
2. Πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου που σχετίζονται με τη συνδρομή σας στο Azure και κάντε κλικ στην επιλογή **Continue**. 
3. Πληκτρολογήστε τον κωδικό πρόσβασης για το λογαριασμό σας. 
4. Κάντε κλικ στην επιλογή **Είσοδος**. 


## <a name="resize-in-the-same-hardware-cluster"></a>Αλλαγή του μεγέθους του ίδιου συμπλέγματος υλικού

Για να αλλάξετε το μέγεθος μια Εικονική σε μέγεθος που είναι διαθέσιμες στο σύμπλεγμα υλικό που φιλοξενεί την εικονική Μηχανή, ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε την ακόλουθη εντολή PowerShell για να παραθέσετε τα διαθέσιμα στο σύμπλεγμα υλικό που φιλοξενεί την υπηρεσία cloud, η οποία περιέχει το Εικονική Εικονική μεγέθη.

    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```

2. Εκτελέστε τις ακόλουθες εντολές για να αλλάξετε το μέγεθος του Εικονική.

    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Αλλαγή μεγέθους σε ένα νέο σύμπλεγμα υλικού

Για να αλλάξετε το μέγεθος μια Εικονική σε μέγεθος δεν είναι διαθέσιμη στο σύμπλεγμα υλικού φιλοξενίας που βασίζεται στο cloud η Εικονική υπηρεσία και όλες ΣΠΣ στην υπηρεσία cloud πρέπει να δημιουργήσετε ξανά. Κάθε υπηρεσία cloud φιλοξενείται σε ένα σύμπλεγμα μία υλικό, ώστε όλα ΣΠΣ στην υπηρεσία cloud πρέπει να είναι ένα μέγεθος που υποστηρίζεται σε ένα σύμπλεγμα υλικού. Ακολουθήστε τα παρακάτω βήματα θα περιγράφουν τον τρόπο αλλαγής μεγέθους μια Εικονική, διαγράφοντας και, στη συνέχεια, να δημιουργείτε την υπηρεσία cloud.

1. Εκτελέστε την ακόλουθη εντολή PowerShell για να παραθέσετε τα διαθέσιμα μεγέθη Εικονική, στην περιοχή. 

    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```

2. Σημειώστε όλες τις ρυθμίσεις παραμέτρων για κάθε Εικονική στο στην υπηρεσία cloud, η οποία περιέχει το Εικονική μπορείτε να αλλάξετε το μέγεθος. 
3. Διαγραφή όλων των ΣΠΣ στην υπηρεσία cloud επιλέγοντας την επιλογή για να διατηρήσετε το δίσκων για κάθε Εικονική.
4. Δημιουργήστε ξανά την εικονική Μηχανή μπορείτε να αλλάξετε χρησιμοποιώντας το επιθυμητό μέγεθος Εικονική.
5. Δημιουργήστε ξανά όλα άλλες ΣΠΣ που είχαν στην υπηρεσία cloud χρησιμοποιώντας μια Εικονική μέγεθος που είναι διαθέσιμα στο σύμπλεγμα υλικού τώρα φιλοξενεί την υπηρεσία cloud.

Μπορείτε να βρείτε ένα δείγμα δέσμης ενεργειών για τη διαγραφή και να δημιουργείτε μια υπηρεσία στο cloud χρησιμοποιώντας ένα νέο μέγεθος Εικονική [εδώ](https://github.com/Azure/azure-vm-scripts). 


## <a name="next-steps"></a>Επόμενα βήματα

- [Αλλαγή μεγέθους μια Εικονική που δημιουργήσατε στο μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-machines-windows-resize-vm.md).