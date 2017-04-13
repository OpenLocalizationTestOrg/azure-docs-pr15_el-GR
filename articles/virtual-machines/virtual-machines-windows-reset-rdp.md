<properties
    pageTitle="Επαναφορά του κωδικού πρόσβασης ή της ρύθμισης παραμέτρων απομακρυσμένης επιφάνειας εργασίας σε μια Εικονική Windows | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να επαναφέρετε έναν κωδικό πρόσβασης λογαριασμού ή υπηρεσίες απομακρυσμένης επιφάνειας εργασίας σε μια Εικονική Windows χρησιμοποιώντας την πύλη του Azure ή Azure PowerShell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Πώς μπορείτε να επαναφέρετε την υπηρεσία απομακρυσμένης επιφάνειας εργασίας ή το κωδικό πρόσβασης σε μια Εικονική των Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Εάν δεν μπορείτε να συνδεθείτε με ένα εικονικό μηχάνημα των Windows (Εικονική), μπορείτε να επαναφέρετε τον κωδικό πρόσβασης του τοπικού διαχειριστή ή να επαναφέρετε τη ρύθμιση παραμέτρων της υπηρεσίας απομακρυσμένης επιφάνειας εργασίας. Μπορείτε να χρησιμοποιήσετε την πύλη του Azure ή την επέκταση Εικονική πρόσβαση στο Azure PowerShell για να επαναφέρετε τον κωδικό πρόσβασης. Εάν χρησιμοποιείτε το PowerShell, βεβαιωθείτε ότι έχετε εγκαταστήσει την πιο πρόσφατη λειτουργικής μονάδας PowerShell εγκατεστημένο στον υπολογιστή σας την εργασία και έχετε πραγματοποιήσει είσοδο σε Azure τη συνδρομή σας. Για λεπτομερείς οδηγίες, διαβάστε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

> [AZURE.TIP] Μπορείτε να ελέγξετε την έκδοση του PowerShell που έχετε εγκαταστήσει χρησιμοποιώντας`Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version`

## <a name="windows-vms-in-resource-manager-deployment-model"></a>Windows ΣΠΣ στο μοντέλο ανάπτυξης για τη διαχείριση πόρων

### <a name="azure-portal"></a>Πύλη του Azure
Επιλέξτε Εικονική σας κάνοντας κλικ στην επιλογή **Αναζήτηση** > **εικονικές μηχανές** > *σας Windows εικονική μηχανή* > **όλες τις ρυθμίσεις** > **Επαναφορά κωδικού πρόσβασης**. Εμφανίζεται το blade επαναφορά κωδικού πρόσβασης:

![Σελίδα επαναφορά κωδικού πρόσβασης](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Πληκτρολογήστε το όνομα χρήστη και έναν νέο κωδικό πρόσβασης και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**. Δοκιμάστε να συνδεθείτε ξανά με το Εικονική.

### <a name="vmaccess-extension-and-powershell"></a>Επέκταση VMAccess και PowerShell

Βεβαιωθείτε ότι έχετε Azure PowerShell 1.0 ή νεότερη έκδοση εγκατεστημένο και έχετε πραγματοποιήσει είσοδο με τη χρήση του λογαριασμού σας του `Login-AzureRmAccount` cmdlet.

#### <a name="reset-the-local-administrator-account-password"></a>**Επαναφέρετε τον κωδικό πρόσβασης του λογαριασμού τοπικού διαχειριστή**

Μπορείτε να επαναφέρετε το διαχειριστή του κωδικού πρόσβασης ή το όνομα χρήστη, χρησιμοποιώντας την εντολή PowerShell [Σύνολο AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) .

Δημιουργήστε διαπιστευτήρια λογαριασμού του τοπικού διαχειριστή, χρησιμοποιώντας την ακόλουθη εντολή:

    $cred=Get-Credential

Εάν πληκτρολογήσετε ένα διαφορετικό όνομα από τον τρέχοντα λογαριασμό, την παρακάτω εντολή επέκταση VMAccess μετονομάζει λογαριασμού του τοπικού διαχειριστή, εκχωρεί τον κωδικό πρόσβασης σε αυτόν το λογαριασμό και ζητήματα ένα συμβάν αποσύνδεσης απομακρυσμένης επιφάνειας εργασίας. Εάν ο λογαριασμός τοπικού διαχειριστή είναι απενεργοποιημένη, επιτρέπει την επέκταση VMAccess του.

Χρησιμοποιήστε την επέκταση access Εικονική για να ορίσετε τη νέα διαπιστευτήρια ως εξής:

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
        -Location WestUS -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Αντικατάσταση `myRG`, `myVM`, `myVMAccess`, και μια θέση με τιμές που σχετίζονται με τις ρυθμίσεις σας.


#### <a name="reset-the-remote-desktop-service-configuration"></a>**Επαναφέρει τη ρύθμιση παραμέτρων της υπηρεσίας απομακρυσμένης επιφάνειας εργασίας**

Μπορείτε να επαναφέρετε απομακρυσμένη πρόσβαση στις Εικονική σας, χρησιμοποιώντας το [Σύνολο AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) ή [Σύνολο AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx), ως εξής. (Αντικαταστήστε το `myRG`, `myVM`, `myVMAccess` και τη θέση με το δικό σας τιμές.)

    Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
        -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Ή:<br>

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Και οι δύο εντολές προσθέσετε έναν νέο επώνυμο παράγοντα access Εικονική η εικονική μηχανή. Σε οποιοδήποτε σημείο, μια Εικονική μπορεί να έχει μόνο μία Εικονική πρόσβασης παράγοντα. Για να ορίσετε την πρόσβαση Εικονική ιδιοτήτων παράγοντα με επιτυχία, καταργήστε τον παράγοντα πρόσβαση προηγουμένως καθοριστεί χρησιμοποιώντας είτε `Remove-AzureRmVMAccessExtension` ή `Remove-AzureRmVMExtension`. Ξεκινώντας από το Azure PowerShell έκδοση 1.2.2, μπορείτε να αποφύγετε αυτό το βήμα κατά τη χρήση `Set-AzureRmVMExtension` με ένα `-ForceRerun` την επιλογή. Κατά τη χρήση `-ForceRerun`, βεβαιωθείτε ότι χρησιμοποιείτε το ίδιο όνομα για τον παράγοντα access Εικονική όπως έχουν οριστεί από την προηγούμενη εντολή.

Εάν εξακολουθείτε να μπορείτε να συνδεθείτε από απόσταση για να την εικονική μηχανή σας, ανατρέξτε στο θέμα περισσότερα βήματα για να δοκιμάσετε στην [Αντιμετώπιση προβλημάτων συνδέσεις απομακρυσμένης επιφάνειας εργασίας σε έναν υπολογιστή με Windows Azure εικονική](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="windows-vms-in-the-classic-deployment-model"></a>Windows ΣΠΣ στο μοντέλο κλασική ανάπτυξης

### <a name="azure-portal"></a>Πύλη του Azure

Για εικονικές μηχανές που δημιουργούνται με χρήση του μοντέλου κλασική ανάπτυξης, μπορείτε να χρησιμοποιήσετε την [πύλη του Azure](https://portal.azure.com) για να επαναφέρετε την υπηρεσία απομακρυσμένης επιφάνειας εργασίας. Κάντε κλικ στην επιλογή: **Αναζήτηση** > **εικονικές μηχανές (κλασική)** > *σας Windows εικονική μηχανή* > **Επαναφορά απομακρυσμένο...**. Εμφανίζεται η σελίδα παρακάτω.

![Επαναφορά σελίδας ρύθμισης παραμέτρων RDP](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

Μπορείτε επίσης να δοκιμάσετε την επαναφορά το όνομα και τον κωδικό πρόσβασης του λογαριασμού τοπικού διαχειριστή. Κάντε κλικ στην επιλογή: **Αναζήτηση** > **εικονικές μηχανές (κλασική)** > *σας Windows εικονική μηχανή* > **όλες τις ρυθμίσεις** > **Επαναφορά κωδικού πρόσβασης**. Εμφανίζεται η σελίδα παρακάτω.

![Σελίδα επαναφορά κωδικού πρόσβασης](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Αφού εισαγάγετε το νέο όνομα χρήστη και κωδικό πρόσβασης, κάντε κλικ στην επιλογή **Αποθήκευση**.

### <a name="vmaccess-extension-and-powershell"></a>Επέκταση VMAccess και PowerShell

Βεβαιωθείτε ότι ο παράγοντας Εικονική είναι εγκατεστημένο στον υπολογιστή εικονική. Η επέκταση VMAccess δεν πρέπει να εγκατασταθούν πριν να τη χρησιμοποιήσετε, με την προϋπόθεση ότι ο παράγοντας Εικονική είναι διαθέσιμη. Βεβαιωθείτε ότι ο παράγοντας Εικονική έχει ήδη εγκατασταθεί, χρησιμοποιώντας την ακόλουθη εντολή. (Αντικατάσταση "myCloudService" και "myVM" με τα ονόματα των την υπηρεσία cloud και σας Εικονική, αντίστοιχα. Μπορείτε να μάθετε αυτά τα ονόματα, εκτελώντας `Get-AzureVM` χωρίς παραμέτρους.)

    $vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
    write-host $vm.VM.ProvisionGuestAgent

Εάν η εντολή **εγγραφής host** εμφανίζει **True**, ο παράγοντας Εικονική είναι εγκατεστημένο. Εάν εμφανίζεται η **τιμή False**, δείτε τις οδηγίες και μια σύνδεση για τη λήψη στην καταχώρηση ιστολογίου Azure [παράγοντας Εικονική και επεκτάσεις - μέρος 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) .

Εάν έχετε δημιουργήσει την εικονική μηχανή χρησιμοποιώντας την πύλη, ελέγξτε εάν `$vm.GetInstance().ProvisionGuestAgent` επιστρέφει την **τιμή True**. Εάν δεν είναι, μπορείτε να τη ρυθμίσετε με τη χρήση αυτής της εντολής:

    $vm.GetInstance().ProvisionGuestAgent = $true

Αυτή η εντολή αποτρέπει το ακόλουθο σφάλμα όταν χρησιμοποιείτε την εντολή **Ορισμός AzureVMExtension** στα επόμενα βήματα: "Παράγοντας επισκέπτη διάταξη πρέπει να ενεργοποιηθεί στο αντικείμενο Εικονική πριν από τη ρύθμιση επέκταση Access Εικονική IaaS."

#### <a name="reset-the-local-administrator-account-password"></a>**Επαναφέρετε τον κωδικό πρόσβασης του λογαριασμού τοπικού διαχειριστή**

Δημιουργήστε μια πιστοποίηση είσοδος με το τρέχον όνομα του λογαριασμού τοπικού διαχειριστή και έναν νέο κωδικό πρόσβασης και, στη συνέχεια, εκτελέστε το `Set-AzureVMAccessExtension` ως εξής.

    $cred=Get-Credential
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Εάν πληκτρολογήσετε ένα διαφορετικό όνομα από τον τρέχοντα λογαριασμό, την επέκταση VMAccess μετονομάζει λογαριασμού του τοπικού διαχειριστή, εκχωρεί τον κωδικό πρόσβασης σε αυτόν το λογαριασμό και ζητήματα sign-out μια απομακρυσμένη επιφάνεια εργασίας. Εάν ο λογαριασμός τοπικού διαχειριστή είναι απενεργοποιημένη, επιτρέπει την επέκταση VMAccess του.

Αυτές οι εντολές επαναφορά επίσης τη ρύθμιση παραμέτρων της υπηρεσίας απομακρυσμένης επιφάνειας εργασίας.

#### <a name="reset-the-remote-desktop-service-configuration"></a>**Επαναφέρει τη ρύθμιση παραμέτρων της υπηρεσίας απομακρυσμένης επιφάνειας εργασίας**

Για να επαναφέρετε τη ρύθμιση παραμέτρων της υπηρεσίας απομακρυσμένης επιφάνειας εργασίας, εκτελέστε την ακόλουθη εντολή:

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

Η επέκταση VMAccess εκτελείται δύο εντολές στον υπολογιστή εικονικές:

- `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

Αυτή η εντολή επιτρέπει στην ενσωματωμένη ομάδα τείχος προστασίας των Windows που επιτρέπει την εισερχόμενη κυκλοφορία απομακρυσμένης επιφάνειας εργασίας, που χρησιμοποιεί αλλάξει.

- `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

Αυτή η εντολή ορίζει το fDenyTSConnections τιμή μητρώου σε 0, η ενεργοποίηση συνδέσεις απομακρυσμένης επιφάνειας εργασίας.


## <a name="next-steps"></a>Επόμενα βήματα

Εάν δεν ανταποκρίνεται την επέκταση access Εικονική Azure και δεν μπορείτε να επαναφέρετε τον κωδικό πρόσβασης, μπορείτε να [επαναφέρετε τον τοπικό Windows κωδικό πρόσβασης χωρίς σύνδεση](virtual-machines-windows-reset-local-password-without-agent.md). Αυτή η μέθοδος είναι μια πιο σύνθετη διαδικασία και απαιτεί να συνδεθείτε εικονικό σκληρό δίσκο του το προβληματικό Εικονική σε ένα άλλο Εικονική. Ακολουθήστε τα βήματα που τεκμηριώνονται σε αυτό το άρθρο πρώτα και η προσπάθεια μόνο τη μέθοδο επαναφορά κωδικού πρόσβασης χωρίς σύνδεση ως τελευταία λύση.

[Azure Εικονική επεκτάσεις και δυνατότητες](virtual-machines-windows-extensions-features.md)

[Σύνδεση με μια εικονική μηχανή Azure με RDP ή SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Αντιμετώπιση προβλημάτων συνδέσεις απομακρυσμένης επιφάνειας εργασίας σε έναν υπολογιστή με Windows Azure εικονικές](virtual-machines-windows-troubleshoot-rdp-connection.md)
