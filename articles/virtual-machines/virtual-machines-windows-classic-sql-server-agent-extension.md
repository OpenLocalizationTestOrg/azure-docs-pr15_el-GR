<properties
    pageTitle="SQL Server παράγοντας επέκταση για SQL Server ΣΠΣ (κλασικό) | Microsoft Azure"
    description="Αυτό το θέμα περιγράφει πώς μπορείτε να διαχειριστείτε την επέκταση παράγοντας SQL Server, η οποία αυτοματοποιεί συγκεκριμένες εργασίες διαχείρισης SQL Server. Αυτά περιλαμβάνουν αυτόματης δημιουργίας αντιγράφων ασφαλείας, αυτόματη διόρθωση και ενσωμάτωση θάλαμο Azure αριθμού-κλειδιού. Αυτό το θέμα χρησιμοποιεί τη λειτουργία κλασική ανάπτυξης."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-classic"></a>SQL Server παράγοντας επέκταση για SQL Server ΣΠΣ (κλασικό)

> [AZURE.SELECTOR]
- [Διαχείριση πόρων](virtual-machines-windows-sql-server-agent-extension.md)
- [Κλασικό](virtual-machines-windows-classic-sql-server-agent-extension.md)

Επέκταση παράγοντας IaaS του SQL Server (SQLIaaSAgent) εκτελείται σε Azure εικονικές μηχανές για την αυτοματοποίηση εργασιών διαχείρισης. Αυτό το θέμα παρέχει μια επισκόπηση των υπηρεσιών που υποστηρίζονται από το εσωτερικό, καθώς και οδηγίες για την εγκατάσταση, κατάσταση και την κατάργηση.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Για να προβάλετε τη διαχείριση πόρων έκδοση αυτού του άρθρου, ανατρέξτε στο θέμα [SQL Server παράγοντας επέκταση για SQL Server ΣΠΣ διαχείριση πόρων](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Υποστηριζόμενες υπηρεσίες

Η επέκταση SQL Server IaaS παράγοντας υποστηρίζει τις εξής εργασίες διαχείρισης:

| Η δυνατότητα διαχείρισης | Περιγραφή |
|---------------------|-------------------------------|
| **SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας** | Εκτελεί αυτόματα τον προγραμματισμό της δημιουργίας αντιγράφων ασφαλείας για όλες τις βάσεις δεδομένων για την προεπιλεγμένη παρουσία του SQL Server σε η Εικονική. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αυτόματης δημιουργίας αντιγράφων ασφαλείας για τον SQL Server σε εικονικές μηχανές Windows Azure (κλασικό)](virtual-machines-windows-classic-sql-automated-backup.md).|
| **SQL αυτοματοποιημένη ενημέρωση κώδικα** | Ρυθμίζει τις παραμέτρους μιας συντήρησης κατά την οποία σας Εικονική μπορεί να χρειαστούν ενημερώσεις θέση, ώστε να μπορείτε να αποφύγετε τις ενημερώσεις κατά τη διάρκεια ώρες αιχμής για του φόρτου εργασίας σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αυτόματη ενημέρωση κώδικα για τον SQL Server σε εικονικές μηχανές Windows Azure (κλασικό)](virtual-machines-windows-classic-sql-automated-patching.md).|
| **Ενοποίηση του Azure θάλαμο κλειδιού** | Σας επιτρέπει να αυτόματα εγκατάσταση και ρύθμιση παραμέτρων θάλαμο κλειδί Azure στο σας Εικονική SQL Server. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων Azure κλειδί θάλαμο ενοποίηση για τον SQL Server σε VM Azure (κλασικό)](virtual-machines-windows-classic-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Απαιτήσεις για να χρησιμοποιήσετε την επέκταση SQL Server IaaS παράγοντας σε Εικονική σας:

### <a name="operating-system"></a>Λειτουργικό σύστημα:

- Windows Server 2012
- Windows Server 2012 R2

### <a name="sql-server-versions"></a>Εκδόσεις του SQL Server:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:

[Λήψη και να ρυθμίσετε τις πιο πρόσφατες εντολές του PowerShell Azure](../powershell-install-configure.md).

Εκκινήστε το Windows PowerShell και συνδέστε τη με τη συνδρομή σας Azure με την εντολή **Προσθήκη AzureAccount** .

    Add-AzureAccount

Εάν έχετε πολλές συνδρομές, χρησιμοποιήστε **AzureSubscription επιλογή** για να επιλέξετε τη συνδρομή που περιέχει το προορισμού κλασική Εικονική.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Σε αυτό το σημείο, μπορείτε να λάβετε μια λίστα με τις κλασική εικονικές μηχανές και τα ονόματα υπηρεσιών σχετίζεται με την εντολή **Get-AzureVM** .

    Get-AzureVM

## <a name="installation"></a>Εγκατάσταση

Για κλασική ΣΠΣ, πρέπει να χρησιμοποιήσετε PowerShell για να εγκαταστήσετε το SQL Server IaaS παράγοντας επέκταση και να ρυθμίσετε τις αντίστοιχες υπηρεσίες. Χρησιμοποιήστε το cmdlet **Set-AzureVMSqlServerExtension** του PowerShell για να εγκαταστήσετε την επέκταση. Για παράδειγμα, την παρακάτω εντολή εγκαθιστά την επέκταση σε μια Εικονική διακομιστή των Windows (κλασικό) και την ονομάζει "SQLIaaSExtension".

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Εάν ενημερώσετε στην πιο πρόσφατη έκδοση της επέκτασης παράγοντας IaaS SQL, πρέπει να επανεκκινήσετε την εικονική μηχανή σας μετά την ενημέρωση της επέκτασης.

>[AZURE.NOTE] Κλασική εικονικές μηχανές δεν έχετε μια επιλογή για να εγκαταστήσετε και να ρυθμίσετε την επέκταση SQL IaaS παράγοντας μέσω της πύλης.

## <a name="status"></a>Κατάσταση

Είναι ένας τρόπος για να βεβαιωθείτε ότι έχει εγκατασταθεί η επέκταση για να προβάλετε την κατάσταση παράγοντας στην πύλη του Azure. Επιλέξτε **όλες οι ρυθμίσεις** στο το blade εικονική μηχανή και, στη συνέχεια, κάντε κλικ στην επιλογή από τις **επεκτάσεις**. Θα πρέπει να δείτε την επέκταση **SQLIaaSAgent** που παρατίθενται.

![SQL Server IaaS παράγοντας επέκταση στην πύλη του Azure](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Μπορείτε επίσης να χρησιμοποιήσετε το cmdlet **Get-AzureVMSqlServerExtension** Azure Powershell.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Κατάργηση   

Στην πύλη του Azure, μπορείτε να την καταργήσετε την επέκταση, κάνοντας κλικ στα αποσιωπητικά σε το blade **επεκτάσεις** σας ιδιοτήτων εικονική μηχανή. Στη συνέχεια, κάντε κλικ στην επιλογή **Διαγραφή**.

![Κατάργηση της εγκατάστασης την επέκταση SQL Server IaaS Agent στην πύλη του Azure](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Μπορείτε επίσης να χρησιμοποιήσετε το cmdlet του Powershell **Κατάργηση AzureVMSqlServerExtension** .

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Επόμενα βήματα

Ξεκινήστε χρησιμοποιώντας μία από τις υπηρεσίες που υποστηρίζεται από την επέκταση. Για περισσότερες λεπτομέρειες, ανατρέξτε στα θέματα που αναφέρονται στην ενότητα [υποστηριζόμενες υπηρεσίες](#supported-services) αυτού του άρθρου.

Για περισσότερες πληροφορίες σχετικά με την εκτέλεση SQL Server σε εικονικές μηχανές Windows Azure, ανατρέξτε στο θέμα [SQL Server σε εικονικές μηχανές Windows Azure Επισκόπηση](virtual-machines-windows-sql-server-iaas-overview.md).
