<properties
   pageTitle="Διαχείριση σας εικονικές μηχανές με χρήση του Azure PowerShell | Microsoft Azure"
   description="Μάθετε εντολές που μπορείτε να χρησιμοποιήσετε για την αυτοματοποίηση εργασιών στη Διαχείριση σας εικονικές μηχανές."
   services="virtual-machines-windows"
   documentationCenter="windows"
   authors="singhkays"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="kasing"/>

# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Διαχείριση σας εικονικές μηχανές με χρήση του Azure PowerShell

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Πολλές εργασίες που μπορείτε να κάνετε κάθε ημέρας για τη διαχείριση του ΣΠΣ μπορεί να είναι αυτοματοποιημένη με τη χρήση των cmdlet του Azure PowerShell. Σε αυτό το άρθρο παρέχει παράδειγμα εντολές για πιο απλές εργασίες και συνδέσεις για άρθρα που εμφανίζουν τις εντολές για πιο σύνθετες εργασίες.

>[AZURE.NOTE] Εάν δεν έχετε εγκαταστήσει και να ρυθμίσει τις παραμέτρους του PowerShell Azure ακόμη, μπορείτε να λάβετε οδηγίες στο άρθρο [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

## <a name="how-to-use-the-example-commands"></a>Πώς μπορείτε να χρησιμοποιήσετε τις εντολές παράδειγμα
Θα χρειαστεί να αντικαταστήσετε μέρος του κειμένου στις εντολές με το κείμενο που είναι κατάλληλη για το περιβάλλον σας. Το < και > σύμβολα υποδεικνύουν πρέπει να αντικαταστήσετε κείμενο. Όταν αντικαθιστάτε το κείμενο, καταργήστε τα σύμβολα αλλά αφήσετε τα σημάδια προσφορά στη θέση.

## <a name="get-a-vm"></a>Λάβετε μια εικονική Μηχανή
Αυτή είναι μια βασική εργασία που θα χρησιμοποιείτε συχνά. Χρησιμοποιήστε το για να λάβετε πληροφορίες σχετικά με μια Εικονική, εκτελούν εργασίες σε μια Εικονική ή γρήγορα την έξοδο για να αποθηκεύσετε σε μια μεταβλητή.

Για πληροφορίες σχετικά με την εικονική Μηχανή, εκτελέσετε αυτήν την εντολή, αντικαθιστώντας τα πάντα σε τις προσφορές, συμπεριλαμβανομένων του < και > χαρακτήρες:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Για να αποθηκεύσετε το αποτέλεσμα σε μεταβλητή $vm, εκτελέστε:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Συνδεθείτε στο μια Εικονική βασίζεται στα Windows

Εκτέλεση αυτών των εντολών:

>[AZURE.NOTE] Μπορείτε να λάβετε το όνομα της υπηρεσίας εικονική μηχανή και cloud από την εμφάνιση της την εντολή **Get-AzureVM** .
>
    $svcName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile = $localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## <a name="stop-a-vm"></a>Διακοπή μια εικονική Μηχανή

Εκτέλεση αυτής της εντολής:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT]Χρησιμοποιήστε αυτήν την παράμετρο για να διατηρήσετε το εικονικό IP (VIP) από την υπηρεσία cloud, σε περίπτωση που η τελευταία Εικονική στα αυτήν την υπηρεσία cloud. <br><br> Εάν χρησιμοποιήσετε την παράμετρο StayProvisioned, μπορείτε ακόμα να χρεωθούν για την εικονική Μηχανή.

## <a name="start-a-vm"></a>Ξεκινήστε μια εικονική Μηχανή

Εκτέλεση αυτής της εντολής:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Επισυνάψτε ένα δίσκο δεδομένων
Αυτή η εργασία απαιτεί λίγα βήματα. Πρώτα, μπορείτε να χρησιμοποιήσετε το *** Προσθήκη-AzureDataDisk *** cmdlet για να προσθέσετε το δίσκο στο αντικείμενο $vm. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε το cmdlet **Ενημέρωση AzureVM** για να ενημερώσετε τις παραμέτρους του η Εικονική.

Θα χρειαστεί επίσης να αποφασίσετε εάν θα την επισύναψη ενός νέου δίσκου ή ένα που περιέχει τα δεδομένα. Για ένα νέο δίσκο, η εντολή δημιουργεί το αρχείο .vhd και επισυνάπτει.

Για να επισυνάψετε ένα νέο δίσκο, εκτέλεση αυτής της εντολής:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Για να επισυνάψετε ένα υπάρχον δίσκου δεδομένων, εκτέλεση αυτής της εντολής:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Για να επισυνάψετε δίσκων δεδομένων από ένα υπάρχον αρχείο .vhd στο χώρο αποθήκευσης αντικειμένων blob, εκτέλεση αυτής της εντολής:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Δημιουργήστε μια Εικονική βασίζεται στα Windows

Για να δημιουργήσετε ένα νέο μηχάνημα εικονικές βασίζεται σε Windows Azure, χρησιμοποιήστε τις οδηγίες στη [Χρήση του PowerShell Azure για να δημιουργήσετε και να ρυθμίσουν εκ των προτέρων εικονικές μηχανές που βασίζεται στα Windows](virtual-machines-windows-classic-create-powershell.md). Βήματα σε αυτό το θέμα που μέσω της δημιουργίας ενός Azure PowerShell εντολή Ορισμός που δημιουργεί μια Εικονική που βασίζεται σε Windows που μπορεί να είναι προ-διαμορφωμένες:

- Με ιδιότητα μέλους τομέα της υπηρεσίας καταλόγου Active Directory.
- Με επιπλέον δίσκων.
- Ως μέλος μιας υπάρχουσας εξισορρόπηση φόρτου Ορισμός.
- Με μια στατική διεύθυνση IP.
