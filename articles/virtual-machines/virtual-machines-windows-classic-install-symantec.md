<properties
    pageTitle="Εγκατάσταση του Symantec Endpoint Protection σε μια Εικονική | Microsoft Azure"
    description="Μάθετε πώς να εγκαταστήσετε και να ρυθμίσετε την επέκταση Symantec Endpoint Protection ασφαλείας σε ένα νέο ή υπάρχον Εικονική Azure που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="iainfou"/>

# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους Symantec Endpoint Protection σε μια Εικονική των Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Σε αυτό το άρθρο σάς δείχνει πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του προγράμματος-πελάτη Symantec Endpoint Protection σε μια υπάρχουσα εικονική μηχανή (Εικονική) εκτελούν τον Windows Server. Αυτό είναι το πλήρες πρόγραμμα-πελάτη, που περιλαμβάνει υπηρεσίες, όπως από ιούς και λογισμικό spyware προστασία, τείχος προστασίας και αποτροπής προεξοχή. Το πρόγραμμα-πελάτη είναι εγκατεστημένη ως επέκταση ασφαλείας, χρησιμοποιώντας τον παράγοντα Εικονική.

Εάν έχετε μια υπάρχουσα συνδρομή από Symantec για μια λύση εσωτερικής εγκατάστασης, μπορείτε να το χρησιμοποιήσετε για να προστατεύσετε το Azure εικονικές μηχανές. Εάν δεν είστε πελάτης ακόμα, μπορείτε να εγγραφείτε για μια δοκιμαστική συνδρομή. Για περισσότερες πληροφορίες σχετικά με αυτήν τη λύση, ανατρέξτε στο θέμα [Symantec Endpoint Protection στην πλατφόρμα του Microsoft Azure][Symantec]. Αυτή η σελίδα έχει επίσης συνδέσεις σε πληροφορίες της άδειας χρήσης και οδηγίες για την εγκατάσταση του προγράμματος-πελάτη, εάν είστε ήδη πελάτης Symantec.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Εγκατάσταση του Symantec Endpoint Protection σε υπάρχουσα Εικονική μηχανή

Προτού ξεκινήσετε, χρειάζεστε τα εξής:

- Η λειτουργική μονάδα Azure PowerShell, έκδοση 0.8.2 ή νεότερη έκδοση, σε υπολογιστή με την εργασία. Μπορείτε να ελέγξετε την έκδοση του Azure PowerShell που έχετε εγκαταστήσει το με το **Get-λειτουργική μονάδα azure | μορφοποίηση πίνακα έκδοση** εντολή. Για οδηγίες και μια σύνδεση προς την πιο πρόσφατη έκδοση, ανατρέξτε στο θέμα [Πώς να εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure][PS]. Συνδεθείτε στο σας χρησιμοποιώντας τη συνδρομή Azure `Add-AzureAccount`.

- Ο παράγοντας Εικονική που εκτελείται στον υπολογιστή εικονικές Azure.

Πρώτα, βεβαιωθείτε ότι ο παράγοντας Εικονική είναι ήδη εγκατεστημένη στον υπολογιστή εικονική. Συμπληρώστε το όνομα υπηρεσία cloud και εικονική μηχανή και, στη συνέχεια, εκτελέστε τις ακόλουθες εντολές σε μια γραμμή εντολών του PowerShell Azure επιπέδου διαχειριστή. Αντικαταστήστε τα πάντα μέσα σε τις προσφορές, συμπεριλαμβανομένων του < και > χαρακτήρες.

> [AZURE.TIP] Εάν δεν γνωρίζετε την υπηρεσία στο cloud και τα ονόματα εικονική μηχανή, εκτελέστε **Get-AzureVM** για να παραθέσετε τα ονόματα για όλες οι εικονικές μηχανές στην τρέχουσα συνδρομή σας.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Εάν η εντολή **εγγραφής host** εμφανίζει **True**, ο παράγοντας Εικονική είναι εγκατεστημένο. Εάν εμφανίζεται η **τιμή False**, δείτε τις οδηγίες και μια σύνδεση για τη λήψη από το ιστολόγιο του Azure δημοσιεύσετε [παράγοντας Εικονική και επεκτάσεις - μέρος 2][Agent].

Εάν ο παράγοντας Εικονική είναι εγκατεστημένο, εκτελέστε αυτές οι εντολές για να εγκαταστήσετε τον παράγοντα Symantec Endpoint Protection.

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

Για να βεβαιωθείτε ότι η επέκταση ασφαλείας Symantec έχει εγκατασταθεί και είναι ενημερωμένο:

1.  Συνδεθείτε με την εικονική μηχανή. Για οδηγίες, ανατρέξτε στο θέμα [πώς μπορείτε να συνδεθείτε στο μια εικονική μηχανή εκτελεί Windows Server][Logon].
2.  Για Windows Server 2008 R2, κάντε κλικ στην επιλογή **Έναρξη > Symantec Endpoint Protection**. Για το Windows Server 2012 ή Windows Server 2012 R2, από την οθόνη έναρξης, πληκτρολογήστε **Symantec**και, στη συνέχεια, κάντε κλικ στην επιλογή **Symantec Endpoint Protection**.
3.  Από την καρτέλα **κατάσταση** του παραθύρου **Κατάστασης Symantec Endpoint Protection** , εφαρμογή ενημερώσεων ή επανεκκίνηση εάν είναι απαραίτητο.

## <a name="additional-resources"></a>Πρόσθετοι πόροι

[Πώς μπορείτε να συνδεθείτε στο μια εικονική μηχανή εκτελούν τον Windows Server][Logon]

[Επεκτάσεις Azure Εικονική και δυνατότητες][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493