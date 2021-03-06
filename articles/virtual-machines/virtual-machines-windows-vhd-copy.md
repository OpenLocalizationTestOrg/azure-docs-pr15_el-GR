<properties
    pageTitle="Δημιουργήστε ένα αντίγραφο της μια Εικονική εξειδικευμένες στο Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα αντίγραφο του ένα εξειδικευμένο Εικονική Windows εκτελείται στο Azure, στο μοντέλο ανάπτυξης διαχείρισης πόρων."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>Δημιουργήστε ένα αντίγραφο του ένα εξειδικευμένο Εικονική των Windows που εκτελείται στο Azure 

Σε αυτό το άρθρο σάς δείχνει πώς μπορείτε να χρησιμοποιήσετε το εργαλείο AZCopy για να δημιουργήσετε ένα αντίγραφο του VHD από έναν εξειδικευμένο Εικονική των Windows που εκτελείται στο Azure. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε το αντίγραφο του VHD για να δημιουργήσετε μια νέα Εικονική. 

- Εάν θέλετε να αντιγράψετε μια Εικονική γενικευμένη, δείτε [πώς μπορείτε να δημιουργήσετε μια Εικονική εικόνα από μια υπάρχουσα γενικευμένη Εικονική Azure](virtual-machines-windows-capture-image.md).

- Εάν θέλετε να αποστείλετε ένα VHD από μια Εικονική εσωτερικής εγκατάστασης, όπως μια που έχει δημιουργηθεί χρησιμοποιώντας το Hyper-V, δείτε [Αποστολή VHD Windows από μια Εικονική εσωτερικής εγκατάστασης για να Azure](virtual-machines-windows-upload-image.md).


## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Βεβαιωθείτε ότι μπορείτε:

- Παρέχουν πληροφορίες σχετικά με τους **λογαριασμούς χώρου αποθήκευσης προέλευσης και προορισμού**. Για την προέλευση Εικονική, πρέπει να ονόματα λογαριασμού και κοντέινερ χώρου αποθήκευσης. Συνήθως, το όνομα του κοντέινερ θα **τους VHD**. Πρέπει επίσης να έχετε ένα λογαριασμό του χώρου αποθήκευσης προορισμού. Εάν δεν έχετε ήδη ένα, μπορείτε να δημιουργήσετε ένα χρησιμοποιώντας είτε την πύλη του (**Περισσότερες υπηρεσίες** > λογαριασμοί χώρου αποθήκευσης > Προσθήκη) ή χρησιμοποιώντας τα cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) . 

- Έχετε Azure [PowerShell 1.0](../powershell-install-configure.md) (ή νεότερη έκδοση) εγκατεστημένο.

- Έχετε κάνει λήψη και εγκατάσταση του [εργαλείου AzCopy](../storage/storage-use-azcopy.md). 


## <a name="deallocate-the-vm"></a>Κατάργηση εκχώρησης την εικονική Μηχανή

Κατάργηση εκχώρησης Εικονική, το οποίο ελευθερώνει VHD να αντιγραφούν. 

- **Πύλη**: κάντε κλικ στην επιλογή **εικονικές μηχανές** > **myVM** > Διακοπή
- **PowerShell**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` καταργεί την εικονική Μηχανή με το όνομα **myVM** στην ομάδα πόρων **myResourceGroup**.

Η **κατάσταση** για την εικονική Μηχανή στην πύλη του Azure αλλάζει από **Διακοπή** **Διακοπή (Κατάργηση εκχώρησης)**.


## <a name="get-the-storage-account-urls"></a>Λάβετε τις διευθύνσεις URL λογαριασμού χώρου αποθήκευσης

Χρειάζεστε τις διευθύνσεις URL των λογαριασμών χώρου αποθήκευσης προέλευσης και προορισμού. Φαίνονται διευθύνσεις URL: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Εάν ήδη γνωρίζετε το όνομα του λογαριασμού και κοντέινερ χώρου αποθήκευσης, μπορείτε να αντικαταστήσετε μόνο τις πληροφορίες μεταξύ των αγκυλών για τη δημιουργία διεύθυνσης URL. 

Μπορείτε να χρησιμοποιήσετε το Azure πύλη ή Azure Powershell για να λάβετε τη διεύθυνση URL:

- **Πύλη**: κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες** > **λογαριασμούς χώρου αποθήκευσης**  >  <storage account> **αντικείμενα BLOB** και το αρχείο VHD προέλευσης είναι πιθανώς στο κοντέινερ **οι VHD** . Κάντε κλικ στην επιλογή **Ιδιότητες** για το κοντέινερ και αντιγράψτε το κείμενο με την ετικέτα **διεύθυνσης URL**. Θα χρειαστείτε τις διευθύνσεις URL των κοντέινερ προέλευσης και προορισμού. 

- **PowerShell**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` λαμβάνει αυτές τις πληροφορίες για Εικονική με το όνομα **myVM** στο την ομάδα πόρων **myResourceGroup**. Στα αποτελέσματα, ανατρέξτε στην ενότητα **προφίλ χώρου αποθήκευσης** για το **Vhd Uri**. Το πρώτο μέρος του Uri είναι η διεύθυνση URL στο κοντέινερ και το τελευταίο τμήμα είναι το όνομα του λειτουργικού Συστήματος VHD για την εικονική Μηχανή.

## <a name="get-the-storage-access-keys"></a>Λήψη των πλήκτρων πρόσβασης του χώρου αποθήκευσης

Εύρεση των πλήκτρων πρόσβασης για προέλευσης και προορισμού λογαριασμούς χώρου αποθήκευσης. Για περισσότερες πληροφορίες σχετικά με τα πλήκτρα πρόσβασης, ανατρέξτε στο θέμα [σχετικά με το Azure λογαριασμούς χώρου αποθήκευσης](../storage/storage-create-storage-account.md).

- **Πύλη**: κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες** > **λογαριασμούς χώρου αποθήκευσης**  >  <storage account> **Όλες τις ρυθμίσεις** > **πλήκτρων πρόσβασης**. Αντιγράψτε το κλειδί που επισημαίνεται ως **κλειδί1**.
- **PowerShell**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` λαμβάνει το κλειδί χώρου αποθήκευσης για το λογαριασμό χώρου αποθήκευσης **mystorageaccount** στην ομάδα του πόρου **myResourceGroup**. Αντιγράψτε το κλειδί που επισημαίνεται ως **κλειδί1**.


## <a name="copy-the-vhd"></a>Αντιγράψτε τον VHD 

Μπορείτε να αντιγράψετε αρχεία μεταξύ λογαριασμών χώρου αποθήκευσης με χρήση AzCopy. Για το κοντέινερ προορισμού, εάν δεν υπάρχει στο καθορισμένο κοντέινερ, θα δημιουργηθεί για εσάς. 

Για να χρησιμοποιήσετε AzCopy, ανοίξτε μια γραμμή εντολών στον τοπικό υπολογιστή σας και μεταβείτε στο φάκελο όπου είναι εγκατεστημένο το AzCopy. Θα είναι παρόμοιο με *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

Για να αντιγράψετε όλα τα αρχεία μέσα σε ένα κοντέινερ, μπορείτε να χρησιμοποιήσετε το διακόπτη **/S** . Αυτό μπορεί να χρησιμοποιηθεί για να αντιγράψετε τον VHD λειτουργικού Συστήματος και όλων των δίσκων δεδομένων εάν βρίσκονται στο ίδιο κοντέινερ. Αυτό το παράδειγμα δείχνει πώς μπορείτε να αντιγράψετε όλα τα αρχεία του το κοντέινερ **mysourcecontainer** στο λογαριασμό χώρου αποθήκευσης **mysourcestorageaccount** το κοντέινερ **mydestinationcontainer** στο λογαριασμό **mydestinationstorageaccount** χώρου αποθήκευσης. Αντικαταστήστε τα ονόματα των λογαριασμών χώρου αποθήκευσης και κοντέινερ με το δικό σας. Αντικατάσταση `<sourceStorageAccountKey1>` και `<destinationStorageAccountKey1>` με τα δικά σας πλήκτρα.

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Εάν θέλετε μόνο να αντιγράψετε έναν συγκεκριμένο VHD σε ένα κοντέινερ με πολλαπλά αρχεία, μπορείτε επίσης να καθορίσετε το όνομα του αρχείου χρησιμοποιώντας το διακόπτη /Pattern. Σε αυτό το παράδειγμα, θα αντιγραφούν μόνο το αρχείο με το όνομα **myFileName.vhd** .

```
    AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
        /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
        /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
        /Pattern:myFileName.vhd
```


Όταν ολοκληρωθεί, θα λάβετε ένα μήνυμα που μοιάζει με:

```
  Finished 2 of total 2 file(s).
  [2016/10/07 17:37:41] Transfer summary:
  -----------------
  Total files transferred: 2
  Transfer successfully:   2
  Transfer skipped:        0
  Transfer failed:         0
  Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

- Όταν χρησιμοποιείτε AZCopy, εάν βλέπετε το σφάλμα "διακομιστής απέτυχε να ελέγχουν την ταυτότητα της αίτησης. Βεβαιωθείτε ότι η τιμή της κεφαλίδας εξουσιοδότησης διαμορφώνεται σωστά συμπεριλαμβανομένης της υπογραφής." και χρησιμοποιείτε 2 αριθμό-κλειδί ή το κλειδί δευτερεύοντα αποθήκευσης, δοκιμάστε να χρησιμοποιήσετε το κλειδί πρωτεύον ή 1ης αποθήκευσης.


## <a name="next-steps"></a>Επόμενα βήματα

- Μπορείτε να δημιουργήσετε μια νέα Εικονική επισυνάπτοντας [το αντίγραφο του VHD σε μια Εικονική ως ένα δίσκο OS](virtual-machines-windows-create-vm-specialized.md).












