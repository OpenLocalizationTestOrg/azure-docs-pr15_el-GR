<properties
    pageTitle="SQL Server παράγοντας επέκταση για SQL Server ΣΠΣ (Διαχείριση πόρων) | Microsoft Azure"
    description="Αυτό το θέμα περιγράφει πώς μπορείτε να διαχειριστείτε την επέκταση παράγοντας SQL Server, η οποία αυτοματοποιεί συγκεκριμένες εργασίες διαχείρισης SQL Server. Αυτά περιλαμβάνουν αυτόματης δημιουργίας αντιγράφων ασφαλείας, αυτόματη διόρθωση και ενσωμάτωση θάλαμο Azure αριθμού-κλειδιού. Αυτό το θέμα χρησιμοποιεί τη λειτουργία ανάπτυξης διαχείρισης πόρων."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-resource-manager"></a>SQL Server παράγοντας επέκταση για SQL Server ΣΠΣ (Διαχείριση πόρων)

> [AZURE.SELECTOR]
- [Διαχείριση πόρων](virtual-machines-windows-sql-server-agent-extension.md)
- [Κλασικό](virtual-machines-windows-classic-sql-server-agent-extension.md)

Επέκταση παράγοντας IaaS του SQL Server (SQLIaaSExtension) εκτελείται σε Azure εικονικές μηχανές, για την αυτοματοποίηση εργασιών διαχείρισης. Αυτό το θέμα παρέχει μια επισκόπηση των υπηρεσιών που υποστηρίζονται από το εσωτερικό, καθώς και οδηγίες για την εγκατάσταση, κατάσταση και την κατάργηση.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης. Για να προβάλετε την κλασική έκδοση αυτού του άρθρου, ανατρέξτε στο θέμα [Επέκταση παράγοντα διακομιστή SQL για κλασική ΣΠΣ του SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Υποστηριζόμενες υπηρεσίες

Η επέκταση SQL Server IaaS παράγοντας υποστηρίζει τις εξής εργασίες διαχείρισης:

| Η δυνατότητα διαχείρισης | Περιγραφή |
|---------------------|-------------------------------|
| **SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας** | Εκτελεί αυτόματα τον προγραμματισμό της δημιουργίας αντιγράφων ασφαλείας για όλες τις βάσεις δεδομένων για την προεπιλεγμένη παρουσία του SQL Server σε η Εικονική. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αυτόματης δημιουργίας αντιγράφων ασφαλείας για τον SQL Server σε εικονικές μηχανές Windows Azure (Διαχείριση πόρων)](virtual-machines-windows-sql-automated-backup.md).|
| **SQL αυτοματοποιημένη ενημέρωση κώδικα** | Ρυθμίζει τις παραμέτρους μιας συντήρησης κατά την οποία σας Εικονική μπορεί να χρειαστούν ενημερώσεις θέση, ώστε να μπορείτε να αποφύγετε τις ενημερώσεις κατά τη διάρκεια ώρες αιχμής για του φόρτου εργασίας σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ενημέρωση αυτόματης για τον SQL Server σε εικονικές μηχανές Windows Azure (Διαχείριση πόρων)](virtual-machines-windows-sql-automated-patching.md).|
| **Ενοποίηση του Azure θάλαμο κλειδιού** | Σας επιτρέπει να αυτόματα εγκατάσταση και ρύθμιση παραμέτρων θάλαμο κλειδί Azure στο σας Εικονική SQL Server. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων Azure κλειδί θάλαμο ενοποίησης για τον SQL Server σε VM Azure (Διαχείριση πόρων)](virtual-machines-windows-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Απαιτήσεις για να χρησιμοποιήσετε την επέκταση SQL Server IaaS παράγοντας σε σας Εικονική:

**Λειτουργικό σύστημα**:

- Windows Server 2012
- Windows Server 2012 R2

**Εκδόσεις του SQL Server**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell**:

- [Λήψη και να ρυθμίσετε τις πιο πρόσφατες εντολές του PowerShell Azure](../powershell-install-configure.md)

## <a name="installation"></a>Εγκατάσταση

Η επέκταση SQL Server IaaS παράγοντας εγκαθίσταται αυτόματα κατά την προμήθεια μία από τις εικόνες συλλογή εικονική μηχανή SQL Server.

Εάν δημιουργήσετε μια εικονική μηχανή μόνο λειτουργικό σύστημα Windows Server, μπορείτε να εγκαταστήσετε την επέκταση με μη αυτόματο τρόπο, χρησιμοποιώντας το cmdlet **Set-AzureVMSqlServerExtension** PowerShell. Για παράδειγμα, την παρακάτω εντολή εγκαθιστά την επέκταση σε ένα μόνο λειτουργικό σύστημα Windows Server εικονική Μηχανή και την ονομάζει "SQLIaaSExtension".

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2"

Εάν ενημερώσετε στην πιο πρόσφατη έκδοση της επέκτασης παράγοντας IaaS SQL, πρέπει να επανεκκινήσετε την εικονική μηχανή σας μετά την ενημέρωση της επέκτασης.

>[AZURE.NOTE] Εάν εγκαταστήσετε την επέκταση παράγοντας SQL Server IaaS με μη αυτόματο τρόπο σε μια Εικονική Windows Server, πρέπει να χρησιμοποιήσετε και διαχείριση των δυνατοτήτων χρησιμοποιώντας τις εντολές του PowerShell. Το περιβάλλον εργασίας πύλης είναι διαθέσιμη μόνο για SQL Server συλλογή εικόνων.

## <a name="status"></a>Κατάσταση

Είναι ένας τρόπος για να βεβαιωθείτε ότι έχει εγκατασταθεί η επέκταση για να προβάλετε την κατάσταση παράγοντας στην πύλη του Azure. Επιλέξτε **όλες οι ρυθμίσεις** στο το blade εικονική μηχανή και, στη συνέχεια, κάντε κλικ στην επιλογή από τις **επεκτάσεις**. Θα πρέπει να δείτε την επέκταση **SQLIaaSExtension** που παρατίθενται.

![SQL Server IaaS παράγοντας επέκταση στην πύλη του Azure](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Μπορείτε επίσης να χρησιμοποιήσετε το cmdlet **Get-AzureVMSqlServerExtension** Azure Powershell.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Της προηγούμενης εντολής επιβεβαιώνει τον παράγοντα έχει εγκατασταθεί και παρέχει πληροφορίες γενική κατάσταση. Μπορείτε επίσης να λάβετε συγκεκριμένες πληροφορίες κατάστασης σχετικά με την αυτόματη δημιουργία αντιγράφων ασφαλείας και ενημερώσεων με τις παρακάτω εντολές.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Κατάργηση   

Στην πύλη του Azure, μπορείτε να την καταργήσετε την επέκταση, κάνοντας κλικ στα αποσιωπητικά σε το blade **επεκτάσεις** σας ιδιοτήτων εικονική μηχανή. Στη συνέχεια, κάντε κλικ στην επιλογή **Διαγραφή**.

![Κατάργηση της εγκατάστασης την επέκταση SQL Server IaaS παράγοντας στην πύλη του Azure](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Μπορείτε επίσης να χρησιμοποιήσετε το cmdlet του Powershell **Κατάργηση AzureRmVMSqlServerExtension** .

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Επόμενα βήματα

Ξεκινήστε χρησιμοποιώντας μία από τις υπηρεσίες που υποστηρίζεται από την επέκταση. Για περισσότερες λεπτομέρειες, ανατρέξτε στα θέματα που αναφέρονται στην ενότητα [υποστηριζόμενες υπηρεσίες](#supported-services) αυτού του άρθρου.

Για περισσότερες πληροφορίες σχετικά με την εκτέλεση SQL Server σε εικονικές μηχανές Windows Azure, ανατρέξτε στο θέμα [SQL Server σε εικονικές μηχανές Windows Azure Επισκόπηση](virtual-machines-windows-sql-server-iaas-overview.md).
