<properties
    pageTitle="Χρήση του PowerShell για δημιουργία αντιγράφων ασφαλείας και επαναφορά εφαρμογές εφαρμογής υπηρεσίας"
    description="Μάθετε τον τρόπο χρήσης του PowerShell για να δημιουργήσετε αντίγραφα ασφαλείας και επαναφορά εφαρμογής στο Azure εφαρμογής υπηρεσίας"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>Χρήση του PowerShell για δημιουργία αντιγράφων ασφαλείας και επαναφορά εφαρμογές εφαρμογής υπηρεσίας

> [AZURE.SELECTOR]
- [PowerShell](app-service-powershell-backup.md)
- [REST API](../app-service-web/websites-csm-backup.md)

Μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure PowerShell για να δημιουργήσετε αντίγραφα ασφαλείας και επαναφορά [εφαρμογής υπηρεσίας εφαρμογών](https://azure.microsoft.com/services/app-service/web/). Για περισσότερες πληροφορίες σχετικά με αντίγραφα ασφαλείας εφαρμογών web, συμπεριλαμβανομένων των απαιτήσεις και τους περιορισμούς, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Για να χρησιμοποιήσετε PowerShell για να διαχειριστείτε τα αντίγραφα ασφαλείας της εφαρμογής, χρειάζεστε τα εξής:

- **Μια διεύθυνση URL συσχετισμών Ασφαλείας** που επιτρέπει την πρόσβαση ανάγνωσης και εγγραφής σε ένα κοντέινερ αποθήκευσης Azure. Ανατρέξτε στο θέμα [Κατανόηση του μοντέλου συσχετισμών Ασφαλείας](../storage/storage-dotnet-shared-access-signature-part-1.md) για μια εξήγηση των συσχετισμών Ασφαλείας διευθύνσεις URL. Για παραδείγματα σχετικά με τη Διαχείριση αποθήκευσης Azure μέσω του PowerShell, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με το χώρο αποθήκευσης Azure](../storage/storage-powershell-guide-full.md) .
- **Μια συμβολοσειρά σύνδεσης βάσης δεδομένων** εάν θέλετε να δημιουργήσετε αντίγραφα ασφαλείας μιας βάσης δεδομένων μαζί με την εφαρμογή web.

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Πώς να δημιουργείτε μια διεύθυνση URL συσχετισμών Ασφαλείας για χρήση με τα cmdlet δημιουργίας αντιγράφων ασφαλείας εφαρμογών web
Μια διεύθυνση URL συσχετισμών Ασφαλείας μπορούν να δημιουργηθούν με το PowerShell. Ακολουθεί ένα παράδειγμα του τρόπου για να δημιουργήσετε μια που μπορούν να χρησιμοποιηθούν με τα cmdlet που αναφέρονται σε αυτό το άρθρο.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Εγκατάσταση του Azure PowerShell 1.3.2 ή μεγαλύτερη

Για οδηγίες σχετικά με την εγκατάσταση και χρήση του Azure PowerShell, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure](../powershell-install-configure.md) .

## <a name="create-a-backup"></a>Δημιουργήστε ένα αντίγραφο ασφαλείας

Χρησιμοποιήστε το cmdlet New-AzureRmWebAppBackup για να δημιουργήσετε ένα αντίγραφο ασφαλείας της μια εφαρμογή web.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Με τον τρόπο αυτό δημιουργείται ένα αντίγραφο ασφαλείας με το όνομα που δημιουργείται αυτόματα. Εάν θέλετε να δώσετε ένα όνομα για το αντίγραφο ασφαλείας, χρησιμοποιήστε την προαιρετική παράμετρο BackupName.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Για να συμπεριλάβετε μια βάση δεδομένων στο αντίγραφο ασφαλείας, πρώτα δημιουργήστε μια ρύθμιση αντιγράφου ασφαλείας βάσης δεδομένων χρησιμοποιώντας το cmdlet New-AzureRmWebAppDatabaseBackupSetting, στη συνέχεια, παρέχετε αυτήν τη ρύθμιση στην παράμετρο βάσεις δεδομένων της το cmdlet New-AzureRmWebAppBackup. Η παράμετρος βάσεις δεδομένων δέχεται ένας πίνακας με ρυθμίσεις της βάσης δεδομένων, επιτρέποντάς σας να δημιουργήσετε αντίγραφα ασφαλείας περισσότερες από μία βάση δεδομένων.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Λάβετε αντίγραφα ασφαλείας

Το cmdlet Get-AzureRmWebAppBackupList επιστρέφει έναν πίνακα από όλα τα αντίγραφα ασφαλείας για μια εφαρμογή web. Πρέπει να πληκτρολογήσετε το όνομα του web app και του ομάδα πόρων.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Για να λάβετε ένα συγκεκριμένο αντίγραφο ασφαλείας, χρησιμοποιήστε το cmdlet Get-AzureRmWebAppBackup.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Μπορείτε επίσης να διοχέτευση ένα αντικείμενο εφαρμογής web σε οποιοδήποτε από τα cmdlet διαχείρισης δημιουργίας αντιγράφων ασφαλείας για τη διευκόλυνσή σας.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Προγραμματισμός αυτόματης δημιουργίας αντιγράφων ασφαλείας

Μπορείτε να προγραμματίσετε αντίγραφα ασφαλείας για να συμβεί αυτόματα σε καθορισμένο χρονικό διάστημα. Για να ρυθμίσετε ένα χρονοδιάγραμμα δημιουργίας αντιγράφων ασφαλείας, χρησιμοποιήστε το cmdlet AzureRmWebAppBackupConfiguration επεξεργασία. Αυτό το cmdlet λαμβάνει διάφορες παραμέτρους:

- **Όνομα** - το όνομα της εφαρμογής web.
- **ResourceGroupName** - στο όνομα της ομάδας πόρων που περιέχει την εφαρμογή web.
- **Υποδιαίρεση** - προαιρετικό. Το όνομα της εσοχής εφαρμογής web.
- **StorageAccountUrl** - η διεύθυνση URL συσχετισμών Ασφαλείας για το χώρο αποθήκευσης Azure κοντέινερ που χρησιμοποιείται για την αποθήκευση αντιγράφων ασφαλείας.
- **FrequencyInterval** - αριθμητική τιμή για το πόσο συχνά θα πρέπει να γίνουν τα αντίγραφα ασφαλείας. Πρέπει να είναι ένα θετικό ακέραιο αριθμό.
- **FrequencyUnit** - μονάδα χρόνου για πόσο συχνά θα πρέπει να γίνουν τα αντίγραφα ασφαλείας. Οι επιλογές είναι ημέρα και ώρα.
- **RetentionPeriodInDays** - πόσες ημέρες την αυτόματη δημιουργία αντιγράφων ασφαλείας θα πρέπει να αποθηκευτεί πριν από τη διαγραφή αυτόματα.
- **Ώρα έναρξης** - προαιρετικό. Ο χρόνος πότε θα πρέπει να ξεκινήσετε την αυτόματη δημιουργία αντιγράφων ασφαλείας. Δημιουργία αντιγράφων ασφαλείας ξεκινήστε αμέσως εάν αυτή είναι null. Πρέπει να είναι μια ημερομηνίας/ώρας.
- **Βάσεις δεδομένων** - προαιρετικό. Ένας πίνακας DatabaseBackupSettings για τις βάσεις δεδομένων για δημιουργία αντιγράφων ασφαλείας.
- Η αλλαγή **KeepAtLeastOneBackup** - προαιρετική παράμετρος. Παρέχετε αυτό αν ένα αντίγραφο ασφαλείας θα πρέπει πάντα να διατηρούνται στο λογαριασμό χώρου αποθήκευσης, ανεξάρτητα από το παλιό πώς είναι.

Ακολουθεί ένα παράδειγμα του τρόπου χρήσης αυτό το cmdlet.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Για να λάβετε το τρέχον χρονοδιάγραμμα αντιγράφων ασφαλείας, χρησιμοποιήστε το cmdlet Get-AzureRmWebAppBackupConfiguration. Αυτό μπορεί να είναι χρήσιμες για την τροποποίηση ενός προγράμματος που έχει ήδη ρυθμιστεί.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Επαναφέρετε μια εφαρμογή web από ένα αντίγραφο ασφαλείας

Για να επαναφέρετε μια εφαρμογή web από ένα αντίγραφο ασφαλείας, χρησιμοποιήστε το cmdlet επαναφορά AzureRmWebAppBackup. Ο ευκολότερος τρόπος για να χρησιμοποιήσετε αυτό το cmdlet είναι η κατακόρυφη γραμμή σε ένα αντικείμενο αντιγράφου ασφαλείας που ανακτήθηκαν από το cmdlet Get-AzureRmWebAppBackup ή το cmdlet Get-AzureRmWebAppBackupList.

Μόλις αντιγράφου ασφαλείας ενός αντικειμένου, μπορείτε να το διοχέτευση σε το cmdlet επαναφορά AzureRmWebAppBackup. Καθορίστε την παράμετρο-διακόπτη αντικατάσταση για να υποδείξει ότι πρόκειται να αντικαταστήσει τα περιεχόμενα της εφαρμογής web με τα περιεχόμενα του αντιγράφου ασφαλείας. Εάν το αντίγραφο ασφαλείας περιέχει βάσεις δεδομένων, γίνεται επαναφορά καθώς και αυτές τις βάσεις δεδομένων.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Ακολουθεί ένα παράδειγμα του τρόπου χρήσης του AzureRmWebAppBackup επαναφορά, καθορίζοντας όλες τις παραμέτρους.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Διαγράψτε ένα αντίγραφο ασφαλείας

Για να διαγράψετε ένα αντίγραφο ασφαλείας, χρησιμοποιήστε το cmdlet κατάργηση AzureRmWebAppBackup. Αυτό καταργεί το αντίγραφο ασφαλείας από το λογαριασμό χώρου αποθήκευσης. Καθορίστε το όνομα εφαρμογής, την ομάδα πόρων και το Αναγνωριστικό του αντιγράφου ασφαλείας που θέλετε να διαγράψετε.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Μπορείτε επίσης να διοχέτευση αντιγράφου ασφαλείας ενός αντικειμένου σε το cmdlet AzureRmWebAppBackup Κατάργηση για να τον διαγράψετε.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
