<properties
    pageTitle="Χρήση ΥΠΌΛΟΙΠΑ δημιουργίας αντιγράφων ασφαλείας και επαναφορά εφαρμογές εφαρμογής υπηρεσίας"
    description="Μάθετε πώς να χρησιμοποιείτε τις κλήσεις RESTful API δημιουργίας αντιγράφων ασφαλείας και επαναφορά της εφαρμογής στο Azure εφαρμογής υπηρεσίας"
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
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>Χρήση ΥΠΌΛΟΙΠΑ δημιουργίας αντιγράφων ασφαλείας και επαναφορά εφαρμογές εφαρμογής υπηρεσίας

> [AZURE.SELECTOR]
- [PowerShell](../app-service/app-service-powershell-backup.md)
- [REST API](websites-csm-backup.md)

[Εφαρμογές εφαρμογής υπηρεσίας](https://azure.microsoft.com/services/app-service/web/) μπορεί να δημιουργηθεί αντίγραφο ασφαλείας ως αντικείμενα blob στο χώρο αποθήκευσης Azure. Το αντίγραφο ασφαλείας μπορεί επίσης να περιέχει βάσεις δεδομένων της εφαρμογής. Εάν η εφαρμογή έχει διαγραφεί ποτέ κατά λάθος ή εάν η εφαρμογή πρέπει να γίνει επαναφορά σε προηγούμενη έκδοση, μπορεί να γίνει επαναφορά από οποιαδήποτε προηγούμενο αντίγραφο ασφαλείας. Μπορεί να γίνει δημιουργίας αντιγράφων ασφαλείας οποιαδήποτε στιγμή στη ζήτηση ή δημιουργία αντιγράφων ασφαλείας μπορούν να προγραμματιστούν σε κατάλληλα χρονικά διαστήματα.

Σε αυτό το άρθρο εξηγεί τον τρόπο δημιουργίας αντιγράφων ασφαλείας και επαναφορά εφαρμογής με αιτήσεις RESTful API. Εάν θέλετε να δημιουργήσετε και να διαχειριστείτε εφαρμογή δημιουργίας αντιγράφων ασφαλείας γραφική μέσω της πύλης Azure, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>Γρήγορα αποτελέσματα
Για να στείλετε προσκλήσεις ΥΠΌΛΟΙΠΑ, πρέπει να γνωρίζετε το **όνομα**της εφαρμογής σας, **ομάδα πόρων**και **αναγνωριστικό συνδρομής**. Μπορείτε να βρείτε αυτές τις πληροφορίες κάνοντας κλικ στην επιλογή εφαρμογή σας στο το blade **Εφαρμογής υπηρεσίας** από την [πύλη του Azure](https://portal.azure.com). Για τα παραδείγματα σε αυτό το άρθρο, θα σας θα ρυθμίσετε την τοποθεσία Web **backuprestoreapiexamples.azurewebsites.net**. Αυτό είναι αποθηκευμένο στην ομάδα πόρων προεπιλεγμένη-Web-WestUS και εκτελείται σε μια συνδρομή με το Αναγνωριστικό 00001111-2222-3333-4444-555566667777.

![Πληροφορίες δείγμα τοποθεσίας Web][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>Δημιουργία αντιγράφων ασφαλείας και επαναφορά REST API
Τώρα θα ασχοληθούμε μερικά παραδείγματα σχετικά με τον τρόπο χρήσης του REST API δημιουργίας αντιγράφων ασφαλείας και επαναφορά της εφαρμογής. Παράδειγμα κάθε περιλαμβάνει μια διεύθυνση URL και HTTP σώμα αίτησης. Η διεύθυνση URL δείγμα περιέχει σύμβολα κράτησης θέσης που περικλείεται σε άγκιστρα, όπως {αναγνωριστικό συνδρομής}. Αντικαταστήστε τα σύμβολα κράτησης θέσης με τις σχετικές πληροφορίες για την εφαρμογή σας. Για αναφορά, θα δείτε μια επεξήγηση κάθε χαρακτήρα κράτησης θέσης που εμφανίζεται στις διευθύνσεις URL παράδειγμα.

* αναγνωριστικό συνδρομής – Αναγνωριστικό της συνδρομής Azure που περιέχει την εφαρμογή
* όνομα ομάδας πόρων-– το όνομα της ομάδας πόρων που περιέχει την εφαρμογή
* όνομα – όνομα της εφαρμογής
* Δημιουργία αντιγράφων ασφαλείας αναγνωριστικού – Αναγνωριστικό του αντιγράφου ασφαλείας εφαρμογής

Για την πλήρη τεκμηρίωση του API, συμπεριλαμβανομένων των αρκετές προαιρετικές παραμέτρους που μπορούν να συμπεριληφθούν στην αίτηση HTTP, ανατρέξτε στο θέμα η [Εξερεύνηση Azure πόρων](https://resources.azure.com/).

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>Δημιουργία αντιγράφων ασφαλείας εφαρμογής ζήτηση
Για να δημιουργήσετε αντίγραφα ασφαλείας εφαρμογής αμέσως, Αποστολή αίτησης **ΔΗΜΟΣΊΕΥΣΗ** **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.

Παρακάτω θα δείτε τη διεύθυνση URL μοιάζει με το παράδειγμα τοποθεσίας Web. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Δώστε ένα αντικείμενο JSON στο σώμα του την αίτησή σας για να καθορίσετε το λογαριασμό χώρου αποθήκευσης που θα χρησιμοποιήσετε για να αποθηκεύσετε το αντίγραφο ασφαλείας. Το αντικείμενο JSON πρέπει να έχετε μια ιδιότητα με το όνομα **storageAccountUrl**, που να περιέχει μια [Διεύθυνση URL συσχετισμών Ασφαλείας](../storage/storage-dotnet-shared-access-signature-part-1.md) εκχώρηση πρόσβασης εγγραφής στο κοντέινερ αποθήκευσης Azure που διατηρεί το αντίγραφο ασφαλείας blob. Εάν θέλετε να δημιουργήσετε αντίγραφα ασφαλείας τις βάσεις δεδομένων, πρέπει επίσης να δώσετε μια λίστα που περιέχει τα ονόματα, τύπους και συμβολοσειρές σύνδεσης των βάσεων δεδομένων σε δημιουργηθούν αντίγραφα ασφαλείας.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Ένα αντίγραφο ασφαλείας της εφαρμογής ξεκινά αμέσως μόλις την παραλαβή της αίτησης. Η διαδικασία δημιουργίας αντιγράφων ασφαλείας ενδέχεται να χρειαστούν μεγάλο χρονικό διάστημα για να ολοκληρωθεί. Η απόκριση HTTP περιέχει ένα Αναγνωριστικό που μπορείτε να χρησιμοποιήσετε στην άλλη πρόσκληση για να δείτε την κατάσταση του αντιγράφου ασφαλείας. Ακολουθεί ένα παράδειγμα της στο σώμα της απόκρισης HTTP για να μας αίτημα δημιουργίας αντιγράφων ασφαλείας.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] Μπορείτε να βρείτε μηνύματα σφάλματος στην ιδιότητα καταγραφής της απόκρισης HTTP.

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>Προγραμματισμός αυτόματης δημιουργίας αντιγράφων ασφαλείας
Εκτός από τη δημιουργία αντιγράφων ασφαλείας εφαρμογής ζήτηση, μπορείτε επίσης να προγραμματίσετε ένα αντίγραφο ασφαλείας για να συμβεί αυτόματα.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Ρύθμιση του νέου χρονοδιαγράμματος αυτόματης δημιουργίας αντιγράφων ασφαλείας
Για να ρυθμίσετε ένα χρονοδιάγραμμα δημιουργίας αντιγράφων ασφαλείας, Αποστολή **ΤΟΠΟΘΈΤΗΣΗ** αίτησης **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.

Παρακάτω θα δείτε τη διεύθυνση URL μοιάζει για μας παράδειγμα τοποθεσίας Web. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

Σώμα της αίτησης πρέπει να έχετε ένα αντικείμενο JSON που καθορίζει τη ρύθμιση παραμέτρων δημιουργίας αντιγράφων ασφαλείας. Ακολουθεί ένα παράδειγμα με όλες τις απαιτούμενες παραμέτρους.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

Αυτό το παράδειγμα ρυθμίζει τις παραμέτρους της εφαρμογής αυτόματη δημιουργία αντιγράφων ασφαλείας κάθε επτά ημέρες. Οι παράμετροι **frequencyInterval** και **frequencyUnit** μαζί καθορίζουν πόσο συχνά συμβεί η δημιουργία αντιγράφων ασφαλείας. Έγκυρες τιμές για **frequencyUnit** είναι **ημέρα**και **ώρα** . Για παράδειγμα, για να δημιουργήσετε αντίγραφα ασφαλείας εφαρμογής κάθε 12 ώρες, ορίστε frequencyInterval έως 12 και frequencyUnit ώρα.

Παλιά αντίγραφα ασφαλείας καταργούνται αυτόματα από το λογαριασμό χώρου αποθήκευσης. Μπορείτε να ελέγχετε τον τρόπο παλιά μπορεί να είναι τα αντίγραφα ασφαλείας, ορίζοντας την παράμετρο **retentionPeriodInDays** . Εάν θέλετε να έχουν πάντα τουλάχιστον ένα αντίγραφο ασφαλείας αποθηκευτεί, ανεξάρτητα από τον τρόπο παλιά, ορίζεται **keepAtLeastOneBackup** στην τιμή true.

### <a name="get-the-automatic-backup-schedule"></a>Λάβετε το χρονοδιάγραμμα αυτόματης δημιουργίας αντιγράφων ασφαλείας
Για να λάβετε μια εφαρμογή δημιουργίας αντιγράφων ασφαλείας ρύθμισης παραμέτρων, πρέπει να στείλετε μια αίτηση **ΔΗΜΟΣΊΕΥΣΗ** στη διεύθυνση URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.

Η διεύθυνση URL μας παράδειγμα τοποθεσίας είναι **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>Λάβετε την κατάσταση του αντιγράφου ασφαλείας
Ανάλογα με το μέγεθος που είναι η εφαρμογή, ένα αντίγραφο ασφαλείας ενδέχεται να χρειαστεί κάποιος χρόνος για να ολοκληρωθεί. Αντίγραφα ασφαλείας ενδέχεται να αποτύχει επίσης την ώρα αποχώρησης, ή μερικώς ολοκληρωθεί με επιτυχία. Για να δείτε την κατάσταση των αντιγράφων ασφαλείας μια εφαρμογή, να στείλετε μια αίτηση **ΛΉΨΗ** στη διεύθυνση URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.

Για να δείτε την κατάσταση των συγκεκριμένων αντίγραφο ασφαλείας, στείλτε ένα αίτημα GET για τη διεύθυνση URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Παρακάτω θα δείτε τη διεύθυνση URL μοιάζει για μας παράδειγμα τοποθεσίας Web. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

Στο σώμα απόκριση περιέχει ένα αντικείμενο JSON παρόμοιο με αυτό το παράδειγμα.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

Η κατάσταση ενός αντιγράφου ασφαλείας είναι απαριθμημένες. Εδώ είναι κάθε πιθανή κατάσταση.

* 0-"σε εξέλιξη": το αντίγραφο ασφαλείας έχει ξεκινήσει, αλλά δεν έχουν ακόμα ολοκληρωθεί.
* 1-απέτυχε: Το αντίγραφο ασφαλείας ήταν επιτυχής.
* 2-ολοκληρώθηκε με επιτυχία: Το αντίγραφο ασφαλείας ολοκληρώθηκε με επιτυχία.
* 3-λήξη χρόνου ορίου: Το αντίγραφο ασφαλείας δεν ολοκληρώθηκε σε χρόνο και ακυρώθηκε.
* 4-δημιουργήσει: Το αίτημα δημιουργίας αντιγράφων ασφαλείας σε ουρά αλλά δεν έχει ξεκινήσει.
* 5-παραλειφθούν: Το αντίγραφο ασφαλείας δεν συνεχίσετε οφείλεται σε ένα χρονοδιάγραμμα Ενεργοποίηση πάρα πολλά αντίγραφα ασφαλείας.
* 6-PartiallySucceeded: Το αντίγραφο ασφαλείας ολοκληρώθηκε με επιτυχία, αλλά ορισμένα αρχεία δεν δημιουργήθηκαν αντίγραφα επειδή δεν ήταν δυνατή η ανάγνωση. Αυτό συνήθως συμβαίνει επειδή αποκλειστικό κλείδωμα έχει τοποθετηθεί με τα αρχεία.
* 7 – DeleteInProgress: Το αντίγραφο ασφαλείας έχει ζητηθεί να διαγραφεί, αλλά δεν έχει διαγραφεί ακόμη.
* 8 – DeleteFailed: Δεν ήταν δυνατή η διαγραφή της δημιουργίας αντιγράφων ασφαλείας. Αυτό μπορεί να συμβεί επειδή η διεύθυνση URL συσχετισμών Ασφαλείας που χρησιμοποιήθηκε για τη δημιουργία αντιγράφων ασφαλείας έχει λήξει.
* 9 – διαγραφή: Το αντίγραφο ασφαλείας έχει διαγραφεί με επιτυχία.

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>Επαναφορά μιας εφαρμογής από ένα αντίγραφο ασφαλείας
Εάν η εφαρμογή σας έχει διαγραφεί ή εάν θέλετε να επαναφέρετε την εφαρμογή σας σε μια προηγούμενη έκδοση, μπορείτε να επαναφέρετε την εφαρμογή από ένα αντίγραφο ασφαλείας. Για να καλέσετε μια επαναφορά, αποστολή μιας αίτησης **POST** στη διεύθυνση URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.

Παρακάτω θα δείτε τη διεύθυνση URL μοιάζει για μας παράδειγμα τοποθεσίας Web. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/Restore**

Στο σώμα της αίτησης, στείλτε ένα αντικείμενο JSON που περιέχει τις ιδιότητες για τη λειτουργία επαναφοράς. Ακολουθεί ένα παράδειγμα που περιέχει όλες τις απαιτούμενες ιδιότητες:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Επαναφορά σε μια νέα εφαρμογή
Μερικές φορές μπορεί να θέλετε να δημιουργήσετε μια νέα εφαρμογή όταν μπορείτε να επαναφέρετε ένα αντίγραφο ασφαλείας, αντί να αντικατασταθούν μια ήδη υπάρχουσα εφαρμογή. Να το κάνετε αυτό, αλλάξτε τη διεύθυνση URL αίτηση στην οποία θα οδηγεί η νέα εφαρμογή που θέλετε να δημιουργήσετε και να αλλάξετε την ιδιότητα **Αντικατάσταση** στο το JSON σε **false**.

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>Διαγράψτε ένα αντίγραφο ασφαλείας της εφαρμογής
Εάν θέλετε να διαγράψετε ένα αντίγραφο ασφαλείας, θα πρέπει να στείλετε μια αίτηση **ΔΙΑΓΡΑΦΉ** για να τη διεύθυνση URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Παρακάτω θα δείτε τη διεύθυνση URL μοιάζει για μας παράδειγμα τοποθεσίας Web. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>Διαχείριση της διεύθυνσης URL συσχετισμών Ασφαλείας ένα αντίγραφο ασφαλείας
Azure εφαρμογής υπηρεσίας θα επιχειρήσει να διαγράψετε το αντίγραφο ασφαλείας από το χώρο αποθήκευσης Azure χρησιμοποιώντας τη διεύθυνση URL συσχετισμών Ασφαλείας που σας δόθηκε κατά τη δημιουργία αντιγράφου ασφαλείας. Εάν αυτή η διεύθυνση URL συσχετισμών Ασφαλείας δεν είναι πλέον έγκυρη, το αντίγραφο ασφαλείας δεν μπορούν να διαγραφούν έως το REST API. Ωστόσο, μπορείτε να ενημερώσετε τη διεύθυνση URL συσχετισμούς Ασφαλείας που σχετίζονται με ένα αντίγραφο ασφαλείας με την αποστολή μιας αίτησης **POST** στη διεύθυνση URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Παρακάτω θα δείτε τη διεύθυνση URL μοιάζει για μας παράδειγμα τοποθεσίας Web. **https://Management.Azure.com/Subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/List**

Στο σώμα της αίτησης, στείλτε ένα αντικείμενο JSON που περιέχει τη νέα διεύθυνση URL συσχετισμών Ασφαλείας. Ακολουθεί ένα παράδειγμα.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] Για λόγους ασφαλείας, τη διεύθυνση URL συσχετισμούς Ασφαλείας που σχετίζονται με ένα αντίγραφο ασφαλείας δεν επιστρέφεται όταν στέλνετε ένα αίτημα GET για ένα συγκεκριμένο αντίγραφο ασφαλείας. Εάν θέλετε να προβάλετε τη διεύθυνση URL συσχετισμούς Ασφαλείας που σχετίζονται με ένα αντίγραφο ασφαλείας, θα πρέπει να στείλετε μια αίτηση ΔΗΜΟΣΊΕΥΣΗ στην παραπάνω την ίδια διεύθυνση URL. Συμπεριλάβετε ένα κενό αντικείμενο JSON στο σώμα της αίτησης. Η απόκριση από το διακομιστή περιέχει όλες τις πληροφορίες του αντιγράφου ασφαλείας, συμπεριλαμβανομένης της διεύθυνσης URL συσχετισμών Ασφαλείας.

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
