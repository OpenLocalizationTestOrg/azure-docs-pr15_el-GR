<properties
    pageTitle="Εισαγωγή ενός αρχείου BACPAC για να δημιουργήσετε μια βάση δεδομένων Azure SQL με χρήση του PowerShell | Microsoft Azure"
    description="Εισαγωγή ενός αρχείου BACPAC για να δημιουργήσετε μια βάση δεδομένων Azure SQL με χρήση του PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Εισαγωγή ενός αρχείου BACPAC για να δημιουργήσετε μια βάση δεδομένων Azure SQL με χρήση του PowerShell

**Μία βάση δεδομένων**

> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Σε αυτό το άρθρο παρέχει οδηγίες για να δημιουργήσετε μια βάση δεδομένων Azure SQL με την εισαγωγή ενός αρχείου [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) με το PowerShell.

Η βάση δεδομένων έχει δημιουργηθεί από ένα αρχείο BACPAC (.bacpac) που έχουν εισαχθεί από ένα κοντέινερ Azure χώρο αποθήκευσης αντικειμένων blob. Εάν δεν έχετε ένα αρχείο BACPAC στο χώρο αποθήκευσης Azure, ανατρέξτε στο θέμα [αρχειοθέτησης μια βάση δεδομένων Azure SQL σε ένα αρχείο BACPAC με χρήση του PowerShell](sql-database-export-powershell.md). Εάν έχετε ήδη ένα αρχείο BACPAC που δεν βρίσκεται στο χώρο αποθήκευσης Azure, [Χρήση AzCopy για αποστολή εύκολα στο λογαριασμό σας χώρο αποθήκευσης Azure](../storage/storage-use-azcopy.md#blob-upload).

> [AZURE.NOTE] Βάση δεδομένων SQL του Azure δημιουργεί αυτόματα και διατηρεί αντίγραφα ασφαλείας για κάθε βάση δεδομένων χρήστη που μπορείτε να τον επαναφέρετε. Για λεπτομέρειες, ανατρέξτε στο θέμα [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md).


Για να εισαγάγετε μια βάση δεδομένων SQL, χρειάζεστε τα εξής:

- Μια συνδρομή του Azure. Εάν χρειάζεστε μια συνδρομή του Azure απλώς κάντε κλικ στην επιλογή **Δωρεάν δοκιμαστική έκδοση** στο επάνω μέρος αυτής της σελίδας και, στη συνέχεια, επιστρέψτε στο τέλος αυτού του άρθρου.
- Ένα αρχείο BACPAC της βάσης δεδομένων που θέλετε να εισαγάγετε. Το BACPAC πρέπει να βρίσκεται σε ένα κοντέινερ [λογαριασμός Azure χώρο αποθήκευσης](../storage/storage-create-storage-account.md) αντικειμένων blob.



[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="set-up-the-variables-for-your-environment"></a>Ρυθμίστε τις μεταβλητές για το περιβάλλον σας

Υπάρχουν μερικά μεταβλητές όπου πρέπει να αντικαταστήστε τις τιμές παράδειγμα με τις συγκεκριμένες τιμές για τη βάση δεδομένων και του λογαριασμού σας χώρου αποθήκευσης.

Το όνομα του διακομιστή πρέπει να είναι ένα διακομιστή που υπάρχει αυτήν τη στιγμή στο τη συνδρομή που επιλέξατε στο προηγούμενο βήμα. Θα πρέπει να το διακομιστή που θέλετε να δημιουργηθούν σε τη βάση δεδομένων. Εισαγωγή μιας βάσης δεδομένων απευθείας σε ένα χώρο συγκέντρωσης ελαστικά δεν υποστηρίζεται. Αλλά μπορείτε να εισαγάγετε πρώτα σε μια μεμονωμένη βάση δεδομένων και, στη συνέχεια, να μετακινήσετε τη βάση δεδομένων σε ένα χώρο συγκέντρωσης.

Το όνομα της βάσης δεδομένων είναι το όνομα που θέλετε για τη νέα βάση δεδομένων.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


Οι παρακάτω μεταβλητές είναι από το λογαριασμό χώρου αποθήκευσης όπου βρίσκεται το BACPAC. Στην [πύλη του Azure](https://portal.azure.com), μεταβείτε στο λογαριασμό σας χώρου αποθήκευσης για να λάβετε αυτές τις τιμές. Μπορείτε να βρείτε τον αριθμό-κλειδί πρωτεύοντος πρόσβασης κάνοντας κλικ στην επιλογή **όλες οι ρυθμίσεις** και, στη συνέχεια, **κλειδιά** από blade του λογαριασμού σας στο χώρο αποθήκευσης.

Το όνομα blob είναι το όνομα ενός υπάρχοντος αρχείου BACPAC που θέλετε να δημιουργήσετε τη βάση δεδομένων από. Πρέπει να συμπεριλάβετε την επέκταση .bacpac.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Εκτελείται η [Get-πιστοποίηση] (https://msdn.microsoft.com/library/azure/hh849815(v=azure.300\).aspx) cmdlet ανοίγει ένα παράθυρο για το όνομα χρήστη και τον κωδικό πρόσβασης. Εισαγάγετε τη διαχείρισης σύνδεσης και τον κωδικό πρόσβασης για το διακομιστή βάσης δεδομένων SQL ($ServerName από επάνω), και όχι το όνομα χρήστη και τον κωδικό πρόσβασης για το λογαριασμό σας Azure.

    $credential = Get-Credential


## <a name="import-the-database"></a>Εισαγωγή της βάσης δεδομένων

Αυτή η εντολή υποβάλλει μια αίτηση βάσης δεδομένων εισαγωγής για την υπηρεσία. Ανάλογα με το μέγεθος της βάσης δεδομένων σας, η λειτουργία εισαγωγής ενδέχεται να χρειαστεί κάποιος χρόνος για να ολοκληρωθεί.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>Παρακολούθηση της προόδου της λειτουργίας

Μετά την εκτέλεση [δημιουργία-AzureRmSqlDatabaseImport] (https://msdn.microsoft.com/library/azure/mt707793(v=azure.300\).aspx), μπορείτε να ελέγξετε την κατάσταση της αίτησης, εκτελώντας το [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx).

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>Δέσμη ενεργειών του PowerShell βάσης δεδομένων SQL εισαγωγής


    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε πώς μπορείτε να συνδεθείτε και υποβολή ερωτήματος σε μια βάση δεδομένων που έχουν εισαχθεί SQL, ανατρέξτε στο θέμα [σύνδεση με βάση δεδομένων SQL με SQL Server Management Studio και να εκτελέσετε ένα ερώτημα T SQL δείγμα](sql-database-connect-query-ssms.md)
