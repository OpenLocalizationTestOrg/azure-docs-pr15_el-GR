<properties
    pageTitle="Αλλαγή μεγέθους Windows Εικονική | Microsoft Azure"
    description="Αλλαγή μεγέθους μια εικονική μηχανή Windows που δημιουργήσατε στο μοντέλο ανάπτυξης για τη διαχείριση πόρων, με χρήση του Powershell Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>

    
# <a name="resize-a-windows-vm"></a>Αλλαγή μεγέθους Windows Εικονική

Αυτό το άρθρο παρουσιάζει τον τρόπο αλλαγής μεγέθους μια Εικονική Windows δημιουργήσατε στο μοντέλο ανάπτυξης διαχείρισης πόρων με χρήση του Powershell Azure.

Αφού δημιουργήσετε μια εικονική μηχανή (Εικονική), μπορείτε να κλιμάκωση η Εικονική προς τα επάνω ή προς τα κάτω, αλλάζοντας το μέγεθος Εικονική. Σε ορισμένες περιπτώσεις, πρέπει να πρώτα να καταργήσετε την εικονική Μηχανή. Αυτό μπορεί να συμβεί εάν το νέο μέγεθος δεν είναι διαθέσιμη στο σύμπλεγμα υλικό που φιλοξενεί την εικονική Μηχανή.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Αλλαγή μεγέθους μια Εικονική Windows όχι σε ένα σύνολο διαθεσιμότητα

1. Λίστα με τα μεγέθη Εικονική που είναι διαθέσιμες στο σύμπλεγμα υλικού όπου φιλοξενείται η Εικονική. 

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```

2. Εάν εμφανίζεται το επιθυμητό μέγεθος, εκτελέστε τις ακόλουθες εντολές για να αλλάξετε το μέγεθος του Εικονική. Εάν δεν παρατίθεται στο επιθυμητό μέγεθος, προχωρήστε στο βήμα 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Εάν το μέγεθος που θέλετε δεν εμφανίζεται, εκτελέστε τις ακόλουθες εντολές για να καταργήσετε την εκχώρηση του Εικονική αλλάξετε το μέγεθός του και επανεκκινήστε την εικονική Μηχανή.

    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [AZURE.WARNING] Κατάργηση εκχώρησης η Εικονική εκδόσεις οποιαδήποτε δυναμικές διευθύνσεις IP που έχουν εκχωρηθεί σε η Εικονική. Το δίσκων λειτουργικό σύστημα και τα δεδομένα δεν επηρεάζονται. 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Αλλαγή μεγέθους μια Εικονική των Windows σε ένα σύνολο διαθεσιμότητα

Εάν το νέο μέγεθος για μια Εικονική στα ένα σύνολο διαθεσιμότητα δεν είναι διαθέσιμη στο σύμπλεγμα υλικού φιλοξενίας αυτήν τη στιγμή η Εικονική, στη συνέχεια, όλα ΣΠΣ του συνόλου διαθεσιμότητα θα πρέπει να αναίρεση της εκχώρησης για να αλλάξετε το μέγεθος του Εικονική. Ίσως χρειαστεί να ενημερώσετε το μέγεθος των άλλων ΣΠΣ στο τη διαθεσιμότητα Ορισμός αφού μία Εικονική έχει αλλάξει. Για να αλλάξετε το μέγεθος μια Εικονική στα ένα σύνολο διαθεσιμότητα, ακολουθήστε τα παρακάτω βήματα.

1. Λίστα με τα μεγέθη Εικονική που είναι διαθέσιμες στο σύμπλεγμα υλικού όπου φιλοξενείται η Εικονική.

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```

2. Εάν εμφανίζεται το επιθυμητό μέγεθος, εκτελέστε τις ακόλουθες εντολές για να αλλάξετε το μέγεθος του Εικονική. Εάν δεν παρατίθεται, μεταβείτε στο βήμα 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Εάν το μέγεθος που θέλετε δεν εμφανίζεται, συνεχίστε με τα παρακάτω βήματα για την εκχώρηση όλα ΣΠΣ του συνόλου διαθεσιμότητα, αλλαγή μεγέθους ΣΠΣ και επανεκκινήστε τους.

4.  Διακοπή όλα ΣΠΣ του συνόλου διαθεσιμότητα.

    ```powershell
    $rg = "<resourceGroupName>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
    } 
    ```
              
5.  Αλλαγή μεγέθους και κάντε επανεκκίνηση του ΣΠΣ του συνόλου διαθεσιμότητα.

    ```powershell
    $rg = "<resourceGroupName>"
    $newSize = "<newVmSize>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
        $vm.HardwareProfile.VmSize = $newSize
        Update-AzureRmVM -ResourceGroupName $rg -VM $vm
        Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
    }
    ```

## <a name="next-steps"></a>Επόμενα βήματα

- Για πρόσθετες κλιμάκωση, εκτελέστε πολλές παρουσίες Εικονική και διαβάθμιση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αυτόματης κλιμάκωσης μηχανές Windows σε ένα σύνολο κλίμακα εικονική μηχανή](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).



