<properties
    pageTitle="Αυτόματη δημιουργία αντιγράφων ασφαλείας για SQL Server εικονικές μηχανές (κλασικό) | Microsoft Azure"
    description="Εξηγεί τη δυνατότητα αυτόματης δημιουργίας αντιγράφων ασφαλείας για τον SQL Server που εκτελείται σε εικονικές μηχανές Windows Azure χρησιμοποιώντας τη διαχείριση πόρων. "
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

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Αυτόματη δημιουργία αντιγράφων ασφαλείας για τον SQL Server σε εικονικές μηχανές Windows (κλασικό) Azure

> [AZURE.SELECTOR]
- [Διαχείριση πόρων](virtual-machines-windows-sql-automated-backup.md)
- [Κλασικό](virtual-machines-windows-classic-sql-automated-backup.md)

Αυτόματη δημιουργία αντιγράφων ασφαλείας ρυθμίζει αυτόματα [Διαχειριζόμενων δημιουργίας αντιγράφων ασφαλείας του Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) για όλες τις υπάρχουσες και τις νέες βάσεις δεδομένων σε μια Εικονική Azure εκτελεί SQL Server 2014 Standard ή για μεγάλες επιχειρήσεις. Αυτό σας επιτρέπει να ρυθμίσετε τις παραμέτρους αντίγραφα ασφαλείας κανονική βάση δεδομένων που χρησιμοποιούν διαρκή αποθήκευσης blob του Azure. Αυτόματη δημιουργία αντιγράφων ασφαλείας εξαρτάται από την [Επέκταση παράγοντας IaaS του SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Για να προβάλετε τη διαχείριση πόρων έκδοση αυτού του άρθρου, ανατρέξτε στο θέμα [Αυτόματης δημιουργίας αντιγράφων ασφαλείας για τον SQL Server στο Azure εικονικές μηχανές από διαχειριστή πόρων](virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να χρησιμοποιήσετε αυτόματης δημιουργίας αντιγράφων ασφαλείας, λάβετε υπόψη τις ακόλουθες προϋποθέσεις:

**Λειτουργικό σύστημα**:

- Windows Server 2012
- Windows Server 2012 R2

**Έκδοση SQL Server/edition**:

- Πρότυπο του SQL Server 2014
- SQL Server 2014 για μεγάλες επιχειρήσεις

>[AZURE.NOTE] SQL Server 2016 δεν υποστηρίζεται ακόμη για αυτόματη δημιουργία αντιγράφων ασφαλείας.

**Ρύθμιση παραμέτρων βάσης δεδομένων**:

- Βάσεις δεδομένων προορισμού πρέπει να χρησιμοποιήσετε το μοντέλο πλήρους ανάκτησης.

**Azure PowerShell**:

- [Εγκαταστήστε τις πιο πρόσφατες εντολές Azure PowerShell](../powershell-install-configure.md).

**Επέκταση IaaS του SQL Server**:

- [Εγκαταστήστε την επέκταση IaaS του SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Ρυθμίσεις

Ο παρακάτω πίνακας περιγράφει τις επιλογές που μπορούν να ρυθμιστούν για αυτόματη δημιουργία αντιγράφων ασφαλείας. Για κλασική ΣΠΣ, πρέπει να χρησιμοποιήσετε PowerShell για να ρυθμίσετε αυτές τις ρυθμίσεις.

|Ρύθμιση|Περιοχή (προεπιλογή)|Περιγραφή|
|---|---|---|
|**Αυτόματη δημιουργία αντιγράφων ασφαλείας**|Ενεργοποίηση/Απενεργοποίηση (απενεργοποιημένο)|Ενεργοποιεί ή απενεργοποιεί αυτόματης δημιουργίας αντιγράφων ασφαλείας για μια Εικονική Azure εκτελεί SQL Server 2014 Standard ή για μεγάλες επιχειρήσεις.|
|**Περίοδος διατήρησης**|1-30 ημέρες (30 ημέρες)|Ο αριθμός των ημερών για να διατηρήσετε ένα αντίγραφο ασφαλείας.|
|**Το λογαριασμό χώρου αποθήκευσης**|Λογαριασμός Azure χώρου αποθήκευσης (το λογαριασμό χώρου αποθήκευσης που έχει δημιουργηθεί για την καθορισμένη εικονική Μηχανή)|Ένας λογαριασμός Azure χώρου αποθήκευσης για να χρησιμοποιήσετε για την αποθήκευση αρχείων αυτόματης δημιουργίας αντιγράφων ασφαλείας στο χώρο αποθήκευσης αντικειμένων blob. Δημιουργείται ένα κοντέινερ σε αυτήν τη θέση για την αποθήκευση όλα τα αρχεία αντιγράφων ασφαλείας. Το αρχείο αντιγράφου ασφαλείας κανόνες ονοματοθεσίας περιλαμβάνει την ημερομηνία, ώρα και το όνομα του υπολογιστή.|
|**Κρυπτογράφηση**|Ενεργοποίηση/Απενεργοποίηση (απενεργοποιημένο)|Ενεργοποιεί ή απενεργοποιεί κρυπτογράφησης. Όταν είναι ενεργοποιημένη η κρυπτογράφηση, τα πιστοποιητικά που χρησιμοποιούνται για να επαναφέρετε το αντίγραφο ασφαλείας βρίσκονται στο λογαριασμό καθορισμένο χώρο αποθήκευσης στο ίδιο κοντέινερ automaticbackup χρησιμοποιώντας το ίδιο κανόνες ονοματοθεσίας. Εάν αλλάξει τον κωδικό πρόσβασης, δημιουργείται ένα νέο πιστοποιητικό με αυτόν τον κωδικό πρόσβασης, αλλά το παλιό πιστοποιητικό παραμένει για να επαναφέρετε εκ των προτέρων δημιουργίας αντιγράφων ασφαλείας.|
|**Κωδικός πρόσβασης**|Κείμενο τον κωδικό πρόσβασης (καμία)|Ένας κωδικός πρόσβασης για κλειδιά κρυπτογράφησης. Αυτό είναι μόνο απαιτείται εάν είναι ενεργοποιημένη η κρυπτογράφηση. Για να επαναφέρετε ένα κρυπτογραφημένο αντίγραφο ασφαλείας, πρέπει να έχετε το σωστό κωδικό πρόσβασης και σχετικές πιστοποιητικό που χρησιμοποιήθηκε κατά το χρόνο που λήφθηκε το αντίγραφο ασφαλείας.|

## <a name="configuration-with-powershell"></a>Ρύθμιση παραμέτρων με το PowerShell

Στο παρακάτω παράδειγμα PowerShell, αυτόματης δημιουργίας αντιγράφων ασφαλείας έχει ρυθμιστεί για μια υπάρχουσα Εικονική 2014 του SQL Server. Η εντολή **Δημιουργία AzureVMSqlServerAutoBackupConfig** ρυθμίζει τις ρυθμίσεις αυτόματης δημιουργίας αντιγράφων ασφαλείας για την αποθήκευση αντιγράφων ασφαλείας στο λογαριασμό Azure χώρου αποθήκευσης που καθορίζονται από τη μεταβλητή $storageaccount. Αυτά τα αντίγραφα ασφαλείας θα διατηρηθούν για 10 ημερών. Η εντολή **Set-AzureVMSqlServerExtension** ενημερώνει το καθορισμένο Εικονική Azure με αυτές τις ρυθμίσεις.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Μπορεί να χρειαστούν αρκετά λεπτά για την εγκατάσταση και ρύθμιση παραμέτρων παράγοντα IaaS του SQL Server.

Για να ενεργοποιήσετε την κρυπτογράφηση, τροποποιήστε την προηγούμενη δέσμη ενεργειών για τη μεταβίβαση την παράμετρο EnableEncryption μαζί με έναν κωδικό πρόσβασης (ασφαλής συμβολοσειρά) για την παράμετρο CertificatePassword. Η ακόλουθη δέσμη ενεργειών ενεργοποιεί τις ρυθμίσεις αυτόματης δημιουργίας αντιγράφων ασφαλείας στο προηγούμενο παράδειγμα και προσθέτει κρυπτογράφησης.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Για να απενεργοποιήσετε την αυτόματη δημιουργία αντιγράφων ασφαλείας, εκτελέστε την ίδια δέσμη ενεργειών χωρίς το **-Ενεργοποίηση** παραμέτρου για τη **Δημιουργία AzureVMSqlServerAutoBackupConfig**. Όπως με την εγκατάσταση, μπορεί να χρειαστούν αρκετά λεπτά για να απενεργοποιήσετε την αυτόματη δημιουργία αντιγράφων ασφαλείας.

>[AZURE.NOTE] Απενεργοποίηση και κατάργηση της εγκατάστασης του SQL Server IaaS Agent δεν καταργεί το προηγουμένως ρυθμίσεις διαχειριζόμενων δημιουργίας αντιγράφων ασφαλείας. Θα πρέπει να απενεργοποιήσετε την αυτόματη δημιουργία αντιγράφων ασφαλείας πριν από την απενεργοποίηση ή κατάργηση της εγκατάστασης του SQL Server IaaS Agent.

## <a name="next-steps"></a>Επόμενα βήματα

Αυτόματη δημιουργία αντιγράφων ασφαλείας ρυθμίζει τις παραμέτρους διαχειριζόμενων αντίγραφο ασφαλείας ΣΠΣ Azure. Επομένως, είναι σημαντικό να [αναθεωρήσετε την τεκμηρίωση για δημιουργία αντιγράφων ασφαλείας διαχειριζόμενων](https://msdn.microsoft.com/library/dn449496.aspx) για να κατανοήσετε τη συμπεριφορά και τις συνέπειες.

Μπορείτε να βρείτε πρόσθετες δημιουργίας αντιγράφων ασφαλείας και να επαναφέρετε οδηγίες για τον SQL Server σε VM Azure στο ακόλουθο θέμα: [Δημιουργία αντιγράφων ασφαλείας και επαναφοράς για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-backup-recovery.md).

Για πληροφορίες σχετικά με άλλες εργασίες διαθέσιμη αυτοματισμού, ανατρέξτε στο θέμα [Επέκταση παράγοντας IaaS του SQL Server](virtual-machines-windows-classic-sql-server-agent-extension.md).

Για περισσότερες πληροφορίες σχετικά με την εκτέλεση του SQL Server σε VM Azure, ανατρέξτε στο θέμα [SQL Server σε εικονικές μηχανές Windows Azure Επισκόπηση](virtual-machines-windows-sql-server-iaas-overview.md).
