<properties
    pageTitle="Αρχειοθέτηση μια βάση δεδομένων Azure SQL σε ένα αρχείο BACPAC με χρήση του PowerShell"
    description="Αρχειοθέτηση μια βάση δεδομένων Azure SQL σε ένα αρχείο BACPAC με χρήση του PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Αρχειοθέτηση μια βάση δεδομένων Azure SQL σε ένα αρχείο BACPAC με χρήση του PowerShell

> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)


Αυτό το άρθρο παρέχει οδηγίες για την αρχειοθέτηση της βάσης δεδομένων Azure SQL σε ένα αρχείο [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) (αποθηκεύονται στο χώρο αποθήκευσης αντικειμένων Blob του Azure) χρησιμοποιώντας PowerShell.

Όταν χρειάζεστε για να δημιουργήσετε ένα αρχείο από μια βάση δεδομένων Azure SQL, μπορείτε να εξαγάγετε το σχήμα της βάσης δεδομένων και τα δεδομένα σε ένα αρχείο BACPAC. Ένα αρχείο BACPAC είναι απλώς ένα αρχείο ZIP με επέκταση .bacpac. Ένα αρχείο BACPAC αργότερα μπορούν να αποθηκευτούν στο χώρο αποθήκευσης αντικειμένων Blob του Azure ή στον τοπικό χώρο αποθήκευσης σε μια θέση εσωτερικής εγκατάστασης. Μπορείτε να εισαγάγετε επίσης ξανά σε βάση δεδομένων SQL Azure ή σε μια εγκατάσταση του SQL Server εσωτερικής εγκατάστασης.

**Ζητήματα**

- Για ένα αρχείο για να ενημερώσετε συνεπή, που πρέπει να βεβαιωθείτε ότι δεν υπάρχει εγγραφή δραστηριότητας παρουσιάζεται κατά την εξαγωγή ή που θέλετε να εξαγάγετε από [ενημερώσετε συνεπή αντίγραφο](sql-database-copy.md) της βάσης δεδομένων Azure SQL.
- Το μέγιστο μέγεθος του αρχείου BACPAC αρχειοθετηθούν με το χώρο αποθήκευσης αντικειμένων Blob του Azure είναι 200 GB. Για να αρχειοθετήσετε ένα μεγαλύτερο αρχείο BACPAC με τον τοπικό χώρο αποθήκευσης, χρησιμοποιήστε το βοηθητικό πρόγραμμα εντολών [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Αυτό το βοηθητικό πρόγραμμα διατίθεται με το Visual Studio και SQL Server. Μπορείτε επίσης να [κάνετε λήψη](https://msdn.microsoft.com/library/mt204009.aspx) την πιο πρόσφατη έκδοση του SQL Server Data Tools για να λάβετε αυτό το βοηθητικό πρόγραμμα.
- Αρχειοθέτηση με το χώρο αποθήκευσης Azure premium, χρησιμοποιώντας ένα αρχείο BACPAC δεν υποστηρίζεται.
- Εάν η λειτουργία εξαγωγής υπερβαίνει 20 ώρες, ίσως έχει ακυρωθεί. Για να αυξήσετε την απόδοση κατά την εξαγωγή, μπορείτε:
 - Προσωρινή αύξηση του επιπέδου υπηρεσίας.
 - Απόσβεση όλα ανάγνωση και εγγραφή δραστηριότητας κατά την εξαγωγή.
 - Χρησιμοποιήστε ένα [ευρετήριο συμπλέγματος](https://msdn.microsoft.com/library/ms190457.aspx) με μη μηδενικές τιμές σε όλους τους πίνακες μεγάλο. Χωρίς ευρετήρια ομαδοποιημένων, μια εξαγωγή ενδέχεται να αποτύχει εάν χρειάζονται περισσότερο από 6-12 ώρες. Αυτό συμβαίνει επειδή η υπηρεσία εξαγωγή πρέπει να ολοκληρωθούν σάρωσης του πίνακα για να δοκιμάσετε για να εξαγάγετε ολόκληρο τον πίνακα. Είναι ένας καλός τρόπος για να προσδιορίσετε εάν τους πίνακές σας είναι βελτιστοποιημένα για εξαγωγή για να εκτελέσετε **DBCC SHOW_STATISTICS** και βεβαιωθείτε ότι το *RANGE_HI_KEY* δεν είναι null και την τιμή που έχει καλή διανομής. Για λεπτομέρειες, ανατρέξτε στο θέμα [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [AZURE.NOTE] BACPACs δεν πρόκειται να χρησιμοποιηθεί για δημιουργία αντιγράφων ασφαλείας και επαναφορά λειτουργίες. Βάση δεδομένων SQL του Azure δημιουργεί αυτόματα αντίγραφα ασφαλείας για κάθε βάση δεδομένων χρήστη. Για λεπτομέρειες, ανατρέξτε στο θέμα [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md).

Για να ολοκληρώσετε αυτό το άρθρο, χρειάζεστε τα εξής:

- Μια συνδρομή του Azure.
- Μια βάση δεδομένων Azure SQL.
- Ένα [τυπικό αποθήκευσης Azure λογαριασμού](../storage/storage-create-storage-account.md), με ένα κοντέινερ αντικειμένων blob για να αποθηκεύσετε το BACPAC σε τυπική χώρου αποθήκευσης.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]




## <a name="export-your-database"></a>Εξαγάγετε τη βάση δεδομένων

Το κλειδί [δημιουργία-AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx) cmdlet υποβάλλει μια αίτηση Εξαγωγή βάσης δεδομένων για την υπηρεσία. Ανάλογα με το μέγεθος της βάσης δεδομένων σας, η λειτουργία εξαγωγής μπορεί να χρειαστεί κάποιος χρόνος για να ολοκληρωθεί.

> [AZURE.IMPORTANT] Για να εξασφαλίσετε ενημερώσετε συνεπή αρχείου BACPAC, που θα πρέπει να πρώτο, [Δημιουργήστε ένα αντίγραφο της βάσης δεδομένων σας](sql-database-copy-powershell.md)και, στη συνέχεια, εξαγωγή το αντίγραφο της βάσης δεδομένων.


     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>Παρακολούθηση της προόδου της λειτουργίας εξαγωγής

Μετά την εκτέλεση [δημιουργία-AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx), μπορείτε να ελέγξετε την κατάσταση της αίτησης, εκτελώντας [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx). Εκτελεί αυτό αμέσως μετά την αίτηση συνήθως επιστρέφει **κατάσταση: "σε εξέλιξη"**. Όταν δείτε το μήνυμα **κατάσταση: ολοκληρώθηκε με επιτυχία** η εξαγωγή έχει ολοκληρωθεί.


    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>Εξαγωγή παράδειγμα βάσης δεδομένων SQL

Το παρακάτω παράδειγμα εξάγει μια υπάρχουσα βάση δεδομένων SQL σε ένα BACPAC και, στη συνέχεια, δείτε πώς μπορείτε να ελέγξετε την κατάσταση της λειτουργίας εξαγωγής.

Για να εκτελέσετε το παράδειγμα, υπάρχουν μερικά μεταβλητές που χρειάζεστε για να αντικαταστήσετε με τις συγκεκριμένες τιμές για το λογαριασμό σας βάση δεδομένων και χώρου αποθήκευσης. Στην [πύλη του Azure](https://portal.azure.com), μεταβείτε στο λογαριασμό σας χώρου αποθήκευσης για να λάβετε το όνομα λογαριασμού χώρου αποθήκευσης blob όνομα κοντέινερ και τιμής κλειδιού. Μπορείτε να βρείτε τον αριθμό-κλειδί κάνοντας κλικ στην επιλογή **πλήκτρα πρόσβασης** σε σας blade λογαριασμού χώρου αποθήκευσης.

Αντικαταστήστε το εξής `VARIABLE-VALUES` με τιμές για τους πόρους σας συγκεκριμένες Azure. Το όνομα της βάσης δεδομένων είναι την υπάρχουσα βάση δεδομένων που θέλετε να εξαγάγετε.



    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
    $serverAdmin = "ADMIN-NAME"
    $serverPassword = "ADMIN-PASSWORD" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε πώς μπορείτε να εισαγάγετε μια βάση δεδομένων Azure SQL με χρήση του Powershell, ανατρέξτε στο θέμα [Εισαγωγή ενός BACPAC χρήση του PowerShell](sql-database-import-powershell.md).


## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Νέα AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)
- [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx)