<properties
    pageTitle="Αυτόματη ενημέρωση για το SQL Server ΣΠΣ (Διαχείριση πόρων) | Microsoft Azure"
    description="Εξηγεί τη δυνατότητα "Αυτόματη Διόρθωση" για SQL Server εικονικές μηχανές εκτελούνται στο Azure χρησιμοποιώντας τη διαχείριση πόρων."
    services="virtual-machines-windows"
    documentationCenter="na"
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
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Αυτόματη ενημέρωση κώδικα για τον SQL Server σε εικονικές μηχανές Windows (Διαχείριση πόρων) Azure

> [AZURE.SELECTOR]
- [Διαχείριση πόρων](virtual-machines-windows-sql-automated-patching.md)
- [Κλασικό](virtual-machines-windows-classic-sql-automated-patching.md)

Αυτοματοποιημένη ενημερώσεων δημιουργεί ένα παράθυρο συντήρησης για ένα Azure εικονική μηχανή εκτελεί τον SQL Server. Αυτοματοποιημένη ενημερώσεις μπορεί να εγκατασταθεί μόνο κατά τη διάρκεια αυτό το παράθυρο Συντήρηση. Για τον SQL Server, αυτό rescriction εξασφαλίζει ότι οι ενημερώσεις συστήματος και τις σχετικές επανεκκίνηση του παρουσιαστεί την καλύτερη δυνατή ώρα για τη βάση δεδομένων. Αυτοματοποιημένη ενημερώσεων εξαρτάται από την [Επέκταση παράγοντας IaaS του SQL Server](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης. Για να προβάλετε την κλασική έκδοση αυτού του άρθρου, ανατρέξτε στο θέμα [Αυτόματη ενημέρωση κώδικα για τον SQL Server σε εικονικές μηχανές Azure κλασική](virtual-machines-windows-classic-sql-automated-patching.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να χρησιμοποιήσετε την αυτόματη διόρθωση, λάβετε υπόψη τις ακόλουθες προϋποθέσεις:

**Λειτουργικό σύστημα**:

- Windows Server 2012
- Windows Server 2012 R2

**Έκδοση SQL Server**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell**:

- [Εγκαταστήστε τις πιο πρόσφατες εντολές του PowerShell Azure](../powershell-install-configure.md) εάν σκοπεύετε να ρυθμίσετε τις παραμέτρους αυτοματοποιημένη ενημέρωση με το PowerShell.

>[AZURE.NOTE] Αυτοματοποιημένη ενημερώσεων εξαρτάται από την επέκταση παράγοντας SQL Server IaaS. Τρέχουσα SQL εικονική μηχανή συλλογή εικόνων προσθέστε αυτήν την επέκταση από προεπιλογή. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επέκταση παράγοντας IaaS του SQL Server](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Ρυθμίσεις

Ο παρακάτω πίνακας περιγράφει τις επιλογές που μπορεί να ρυθμιστεί για την αυτόματη διόρθωση. Τα βήματα ρύθμισης παραμέτρων πραγματική ποικίλλουν ανάλογα με το εάν χρησιμοποιείτε το Azure πύλη ή τις εντολές του Azure Windows PowerShell.

|Ρύθμιση|Πιθανές τιμές|Περιγραφή|
|---|---|---|
|**Αυτόματη Διόρθωση**|Ενεργοποίηση/Απενεργοποίηση (απενεργοποιημένο)|Ενεργοποιεί ή απενεργοποιεί την αυτόματη διόρθωση για μια εικονική μηχανή Azure.|
|**Χρονοδιάγραμμα συντήρησης**|Καθημερινή, Δευτέρα, Τρίτη, Τετάρτη, Πέμπτη, Παρασκευή, Σάββατο, Κυριακή|Το χρονοδιάγραμμα για τη λήψη και εγκατάσταση ενημερώσεων των Windows, SQL Server και Microsoft για την εικονική μηχανή σας.|
|**Ώρα έναρξης συντήρηση**|0-24|Την τοπική ώρα για να ενημερώσετε την εικονική μηχανή έναρξης.|
|**Συντήρηση παραθύρου διάρκεια**|30-180|Τον αριθμό των λεπτών που επιτρέπεται για να ολοκληρώσετε τη λήψη και εγκατάσταση των ενημερώσεων.|
|**Κατηγορία ενημέρωσης κώδικα**|Σημαντικό|Η κατηγορία των ενημερωμένων εκδόσεων για να κάνετε λήψη και εγκατάσταση.|

## <a name="configuration-in-the-portal"></a>Ρύθμιση παραμέτρων στην πύλη
Μπορείτε να χρησιμοποιήσετε την πύλη του Azure για να ρυθμίσετε Αυτόματη Διόρθωση κατά την προμήθεια του ή για υπάρχοντα ΣΠΣ.

### <a name="new-vms"></a>Νέα ΣΠΣ
Χρησιμοποιήστε την πύλη του Azure για να ρυθμίσετε Αυτόματη Διόρθωση όταν δημιουργείτε μια νέα εικονική μηχανή του SQL Server στο μοντέλο ανάπτυξης διαχείρισης πόρων.

Στο blade του **SQL Server ρυθμίσεις** , επιλέξτε **Ενημέρωση αυτόματης**. Το παρακάτω στιγμιότυπο οθόνης Azure πύλης εμφανίζει το blade **Αυτοματοποιημένη ενημέρωση κώδικα SQL** .

![Αυτόματη ενημέρωση κώδικα SQL στην πύλη του Azure](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Για το περιβάλλον, ανατρέξτε στο θέμα ολοκλήρωσης στην [προμήθεια μια εικονική μηχανή SQL Server στο Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Υπάρχουσα ΣΠΣ
Για υπάρχοντα εικονικές μηχανές SQL Server, επιλέξτε την εικονική μηχανή SQL Server. Στη συνέχεια, επιλέξτε την ενότητα **ρύθμισης παραμέτρων του SQL Server** από το blade **Ρυθμίσεις** .

![SQL αυτόματη ενημέρωση κώδικα για υπάρχοντα ΣΠΣ](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

Στο το blade **ρύθμισης παραμέτρων του SQL Server** , κάντε κλικ στο κουμπί **Επεξεργασία** στην το αυτόματο ενημερωμένων ενότητας.

![Ρύθμιση παραμέτρων αυτοματοποιημένη ενημέρωση κώδικα SQL για υπάρχοντα ΣΠΣ](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Όταν τελειώσετε, κάντε κλικ στο κουμπί **OK** στο κάτω μέρος του blade **ρύθμισης παραμέτρων του SQL Server** για να αποθηκεύσετε τις αλλαγές σας.

Εάν ενεργοποιείτε την αυτόματη διόρθωση για πρώτη φορά, Azure ρυθμίζει τις παραμέτρους του SQL Server IaaS Agent στο παρασκήνιο. Σε αυτό το διάστημα, η πύλη του Azure μπορεί να μην εμφανίζεται ότι έχει ρυθμιστεί η Αυτόματη διόρθωση. Περιμένετε μερικά λεπτά για τον παράγοντα για την εγκατάσταση, να ρυθμίσει τις παραμέτρους. Μετά από αυτό το Azure πύλη απεικονίζει τις νέες ρυθμίσεις.

>[AZURE.NOTE] Μπορείτε επίσης να ρυθμίσετε Αυτόματη Διόρθωση χρησιμοποιώντας ένα πρότυπο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πρότυπο Azure γρήγορης έναρξης για αυτόματη διόρθωση](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).

## <a name="configuration-with-powershell"></a>Ρύθμιση παραμέτρων με το PowerShell

Μετά την προμήθεια του Εικονική SQL, χρήση του PowerShell για να ρυθμίσετε την αυτόματη διόρθωση.

Στο παρακάτω παράδειγμα, χρησιμοποιείται PowerShell για να ρυθμίσετε Αυτόματη διόρθωση σε μια υπάρχουσα Εικονική SQL Server. Η εντολή **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** ρυθμίζει ένα νέο παράθυρο συντήρησης για τις Αυτόματες ενημερώσεις.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Που βασίζονται σε αυτό το παράδειγμα, ο παρακάτω πίνακας περιγράφει το πρακτικό εφέ στον προορισμό Εικονική Azure:

|Παράμετρος|Εφέ|
|---|---|
|**DayOfWeek**|Κάθε Πέμπτη εγκατεστημένες ενημερωμένες εκδόσεις κώδικα.|
|**MaintenanceWindowStartingHour**|Ξεκινήστε να ενημερώνει στο 11:00 π.μ.|
|**MaintenanceWindowsDuration**|Πρέπει να εγκατασταθούν ενημερώσεις κώδικα εντός 120 λεπτά. Με βάση την ώρα έναρξης, πρέπει να ολοκληρώσει από 1:00 μ.μ.|
|**PatchCategory**|Η ρύθμιση πιθανές μόνο για αυτήν την παράμετρο είναι **σημαντικές**.|

Μπορεί να χρειαστούν αρκετά λεπτά για την εγκατάσταση και ρύθμιση παραμέτρων παράγοντα IaaS του SQL Server.

Για να απενεργοποιήσετε την αυτόματη διόρθωση, εκτελέστε την ίδια δέσμη ενεργειών χωρίς το **-Ενεργοποίηση** παράμετρο, για να το **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**. Δεν υπάρχει το **-Ενεργοποίηση** παραμέτρου σήματα την εντολή για να απενεργοποιήσετε τη δυνατότητα.

## <a name="next-steps"></a>Επόμενα βήματα

Για πληροφορίες σχετικά με άλλες εργασίες διαθέσιμη αυτοματισμού, ανατρέξτε στο θέμα [Επέκταση παράγοντας IaaS του SQL Server](virtual-machines-windows-sql-server-agent-extension.md).

Για περισσότερες πληροφορίες σχετικά με την εκτέλεση του SQL Server σε VM Azure, ανατρέξτε στο θέμα [SQL Server σε εικονικές μηχανές Windows Azure Επισκόπηση](virtual-machines-windows-sql-server-iaas-overview.md).
