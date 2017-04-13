<properties
    pageTitle="Απόσπαση ένα δίσκο δεδομένων από μια Εικονική Windows | Microsoft Azure"
    description="Μάθετε πώς να αποσπάσετε ένα δίσκο δεδομένων από μια εικονική μηχανή στο Azure χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>



# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Πώς να αποσπάσετε ένα δίσκο δεδομένων από μια εικονική μηχανή των Windows


Όταν δεν χρειάζεστε πλέον ένα δίσκο δεδομένων που είναι συνημμένα σε μια εικονική μηχανή, μπορείτε εύκολα να την αποσύνδεση. Αυτό καταργεί το δίσκο από την εικονική μηχανή, αλλά δεν την καταργήσετε από το χώρο αποθήκευσης. 

> [AZURE.WARNING] Εάν αποσυνδεθείτε ένα δίσκο δεν διαγράφεται αυτόματα. Εάν έχετε εγγραφεί στο χώρο αποθήκευσης Premium, θα συνεχίσετε να χρεωθείτε χώρου αποθήκευσης για το δίσκο. Για περισσότερες πληροφορίες ανατρέξτε [Τιμολόγηση και τις χρεώσεις κατά τη χρήση του χώρου αποθήκευσης Premium](../storage/storage-premium-storage.md#pricing-and-billing). 

Εάν θέλετε να χρησιμοποιήσετε ξανά τα υπάρχοντα δεδομένα στο δίσκο, μπορείτε να επισυνάψετε ξανά την ίδια εικονική μηχανή ή κάποιο άλλο.  


## <a name="detach-a-data-disk-using-the-portal"></a>Απόσπαση ένα δίσκο δεδομένων με την πύλη

1. Στην ενότητα πύλης, επιλέξτε **εικονικές μηχανές**.

2. Επιλέξτε την εικονική μηχανή που έχει στο δίσκο δεδομένων που θέλετε να αποσπάσετε και, στη συνέχεια, κάντε κλικ στην επιλογή **όλες οι ρυθμίσεις**.

3. Στο το blade **Ρυθμίσεις** , επιλέξτε **δίσκων**.

4. Στο το blade **δίσκων** , επιλέξτε τον δίσκο δεδομένων που θέλετε να αποσπάσετε.

5. Στο το blade για το δίσκο δεδομένων, κάντε κλικ στην επιλογή **Αποσύνδεση**.


    ![Στιγμιότυπο οθόνης που εμφανίζει το κουμπί Αποσύνδεση.](./media/virtual-machines-windows-detach-disk/detach-disk.png)

Ο δίσκος παραμένει στο χώρο αποθήκευσης, αλλά δεν είναι πλέον συνδεδεμένη με ένα εικονικό μηχάνημα.


## <a name="detach-a-data-disk-using-powershell"></a>Απόσπαση ένα δίσκο δεδομένων με χρήση του PowerShell

Σε αυτό το παράδειγμα, η πρώτη εντολή λαμβάνει την εικονική μηχανή με το όνομα **MyVM07** στην ομάδα πόρων **RG11** χρησιμοποιώντας το cmdlet Get-AzureRmVM. Η εντολή αποθηκεύει την εικονική μηχανή στη μεταβλητή **$VirtualMachine** . 

Η δεύτερη εντολή καταργεί το δίσκο δεδομένων με το όνομα DataDisk3 από την εικονική μηχανή. 

Η τελική εντολή ενημερώνει την κατάσταση της μηχανής εικονικές για να ολοκληρώσετε τη διαδικασία κατάργησης του δίσκου δεδομένων.

    $VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" 
    Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
    Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine


Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Κατάργηση AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx)

## <a name="next-steps"></a>Επόμενα βήματα

Εάν θέλετε να χρησιμοποιήσετε εκ νέου το δίσκο δεδομένων, μπορείτε να κάνετε απλώς [επισύναψή του σε μια άλλη Εικονική](virtual-machines-windows-attach-disk-portal.md)
