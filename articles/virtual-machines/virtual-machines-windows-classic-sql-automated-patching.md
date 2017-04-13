<properties
    pageTitle="Αυτόματη ενημέρωση για το SQL Server ΣΠΣ (κλασικό) | Microsoft Azure"
    description="Εξηγεί τη δυνατότητα "Αυτόματη Διόρθωση" για SQL Server εικονικές μηχανές εκτελούνται στο Azure χρησιμοποιώντας τη λειτουργία κλασική ανάπτυξης."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Αυτόματη ενημέρωση κώδικα για τον SQL Server σε εικονικές μηχανές Windows (κλασικό) Azure

> [AZURE.SELECTOR]
- [Διαχείριση πόρων](virtual-machines-windows-sql-automated-patching.md)
- [Κλασικό](virtual-machines-windows-classic-sql-automated-patching.md)

Αυτοματοποιημένη ενημερώσεων δημιουργεί ένα παράθυρο συντήρηση για μια Azure εικονική μηχανή εκτελεί τον SQL Server. Αυτοματοποιημένη ενημερώσεις μπορεί να εγκατασταθεί μόνο κατά τη διάρκεια αυτό το παράθυρο Συντήρηση. Για τον SQL Server, αυτό εξασφαλίζει ότι οι ενημερώσεις συστήματος και τις σχετικές επανεκκίνηση του προκύψει την καλύτερη δυνατή ώρα για τη βάση δεδομένων. Αυτοματοποιημένη ενημερώσεων εξαρτάται από την [Επέκταση παράγοντας IaaS του SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Για να προβάλετε τη διαχείριση πόρων έκδοση αυτού του άρθρου, ανατρέξτε στο θέμα [Αυτόματη ενημέρωση κώδικα για τον SQL Server στο Azure εικονικές μηχανές από διαχειριστή πόρων](virtual-machines-windows-sql-automated-patching.md).

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

- [Εγκαταστήστε τις πιο πρόσφατες εντολές Azure PowerShell](../powershell-install-configure.md).

**Επέκταση IaaS του SQL Server**:

- [Εγκαταστήστε την επέκταση IaaS του SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Ρυθμίσεις

Ο παρακάτω πίνακας περιγράφει τις επιλογές που μπορεί να ρυθμιστεί για την αυτόματη διόρθωση. Για κλασική ΣΠΣ, πρέπει να χρησιμοποιήσετε PowerShell για να ρυθμίσετε αυτές τις ρυθμίσεις.

|Ρύθμιση|Πιθανές τιμές|Περιγραφή|
|---|---|---|
|**Αυτόματη Διόρθωση**|Ενεργοποίηση/Απενεργοποίηση (απενεργοποιημένο)|Ενεργοποιεί ή απενεργοποιεί την αυτόματη διόρθωση για μια εικονική μηχανή Azure.|
|**Χρονοδιάγραμμα συντήρησης**|Καθημερινή, Δευτέρα, Τρίτη, Τετάρτη, Πέμπτη, Παρασκευή, Σάββατο, Κυριακή|Το χρονοδιάγραμμα για τη λήψη και εγκατάσταση ενημερώσεων των Windows, SQL Server και Microsoft για την εικονική μηχανή σας.|
|**Ώρα έναρξης συντήρηση**|0-24|Την τοπική ώρα για να ενημερώσετε την εικονική μηχανή έναρξης.|
|**Συντήρηση παραθύρου διάρκεια**|30-180|Τον αριθμό των λεπτών που επιτρέπεται για να ολοκληρώσετε τη λήψη και εγκατάσταση των ενημερώσεων.|
|**Κατηγορία ενημέρωσης κώδικα**|Σημαντικό|Η κατηγορία των ενημερωμένων εκδόσεων για να κάνετε λήψη και εγκατάσταση.|

## <a name="configuration-with-powershell"></a>Ρύθμιση παραμέτρων με το PowerShell

Στο παρακάτω παράδειγμα, χρησιμοποιείται PowerShell για να ρυθμίσετε Αυτόματη διόρθωση σε μια υπάρχουσα Εικονική SQL Server. Η εντολή **Δημιουργία AzureVMSqlServerAutoPatchingConfig** ρυθμίζει ένα νέο παράθυρο συντήρησης για τις Αυτόματες ενημερώσεις.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Που βασίζονται σε αυτό το παράδειγμα, ο παρακάτω πίνακας περιγράφει την πρακτική επίδραση στον προορισμό Εικονική Azure:

|Παράμετρος|Εφέ|
|---|---|
|**DayOfWeek**|Κάθε Πέμπτη εγκατεστημένες ενημερωμένες εκδόσεις κώδικα.|
|**MaintenanceWindowStartingHour**|Ξεκινήστε να ενημερώνει στο 11:00 π.μ.|
|**MaintenanceWindowsDuration**|Πρέπει να εγκατασταθούν ενημερώσεις κώδικα εντός 120 λεπτά. Με βάση την ώρα έναρξης, πρέπει να ολοκληρώσει από 1:00 μ.μ.|
|**PatchCategory**|Η ρύθμιση μόνο πιθανές για αυτή η παράμετρος είναι "Σημαντικό".|

Μπορεί να χρειαστούν αρκετά λεπτά για την εγκατάσταση και ρύθμιση παραμέτρων παράγοντα IaaS του SQL Server.

Για να απενεργοποιήσετε την αυτόματη διόρθωση, εκτελέστε το ίδιο δέσμης ενεργειών χωρίς την παράμετρο-ενεργοποίηση για τη δημιουργία-AzureVMSqlServerAutoPatchingConfig. Όπως με την εγκατάσταση, μπορεί να χρειαστούν αρκετά λεπτά για να απενεργοποιήσετε την αυτόματη διόρθωση.

## <a name="next-steps"></a>Επόμενα βήματα

Για πληροφορίες σχετικά με άλλες εργασίες διαθέσιμη αυτοματισμού, ανατρέξτε στο θέμα [Επέκταση παράγοντας IaaS του SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

Για περισσότερες πληροφορίες σχετικά με την εκτέλεση του SQL Server σε VM Azure, ανατρέξτε στο θέμα [SQL Server σε εικονικές μηχανές Windows Azure Επισκόπηση](virtual-machines-windows-sql-server-iaas-overview.md).
