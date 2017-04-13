<properties
    pageTitle="Ενεργοποίηση μετρικά αποθήκευσης στην πύλη του Azure | Microsoft Azure"
    description="Πώς μπορείτε να ενεργοποιήσετε μετρικά αποθήκευσης για τις υπηρεσίες Blob, ουρά, πίνακα και αρχείων"
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a>Ενεργοποίηση μετρικά αποθήκευσης Azure και προβολή δεδομένων μετρικά

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Επισκόπηση

Από προεπιλογή, μετρικών αποθήκευσης που δεν είναι ενεργοποιημένη για τις υπηρεσίες σας χώρου αποθήκευσης. Μπορείτε να ενεργοποιήσετε την παρακολούθηση μέσω της [Πύλης Azure](https://portal.azure.com) ή του Windows PowerShell, ή μέσω προγραμματισμού μέσω της βιβλιοθήκης του προγράμματος-πελάτη χώρου αποθήκευσης.

Όταν ενεργοποιείτε μετρικά αποθήκευσης, πρέπει να επιλέξετε μια περίοδος διατήρησης για τα δεδομένα: αυτήν την περίοδο καθορίζει για πόσο χώρο αποθήκευσης την υπηρεσία διατηρεί τα μετρήσεις και τα έξοδα για το διάστημα απαιτείται για την αποθήκευση τους. Συνήθως, θα πρέπει να χρησιμοποιείτε μια μικρότερη περίοδος διατήρησης για minute μετρικά από ωριαία μετρικά λόγω το σημαντική επιπλέον χώρο που απαιτείται για minute μετρικά. Θα πρέπει να επιλέξετε μια περίοδος διατήρησης, ώστε να έχετε αρκετό χρόνο για να αναλύσετε τα δεδομένα και να κάνετε λήψη τις μετρήσεις που θέλετε να κρατήσετε για σκοπούς αναφοράς ή ανάλυσης χωρίς σύνδεση. Να θυμάστε ότι επίσης θα χρεώνεστε για τη λήψη δεδομένων μετρικά από το λογαριασμό χώρου αποθήκευσης.

## <a name="how-to-enable-metrics-using-the-azure-portal"></a>Πώς μπορείτε να ενεργοποιήσετε μετρικά με την πύλη Azure

Ακολουθήστε τα παρακάτω βήματα για να ενεργοποιήσετε την μετρικά στην [Πύλη του Azure](https://portal.azure.com):

1. Μεταβείτε στο λογαριασμό σας χώρου αποθήκευσης.
1. Ανοίξτε το blade **Ρυθμίσεις** και επιλέξτε **Διαγνωστικά**.
1. Βεβαιωθείτε ότι η **κατάσταση** έχει οριστεί σε **σε**.
1. Επιλέξτε τις μετρήσεις για τις υπηρεσίες που θέλετε για την παρακολούθηση.
2. Καθορίστε μια πολιτική διατήρησης για να υποδείξετε πόσο χρόνο για να διατηρήσετε μετρικά και καταγραφή δεδομένων.

Σημειώστε ότι η [Πύλη Azure](https://portal.azure.com) δεν αυτήν τη στιγμή σάς επιτρέπουν να ρυθμίζετε minute μετρικά στο λογαριασμό σας στο χώρο αποθήκευσης; πρέπει να ενεργοποιήσετε το λεπτό μετρικά χρήση του PowerShell ή μέσω προγραμματισμού.

## <a name="how-to-enable-metrics-using-powershell"></a>Πώς μπορείτε να ενεργοποιήσετε μετρικά χρήση του PowerShell

Μπορείτε να χρησιμοποιήσετε PowerShell στον τοπικό υπολογιστή σας για τη ρύθμιση παραμέτρων μετρικά αποθήκευσης στο λογαριασμό σας στο χώρο αποθήκευσης, χρησιμοποιώντας το Azure PowerShell cmdlet Get-AzureStorageServiceMetricsProperty για να ανακτήσετε τις τρέχουσες ρυθμίσεις και το cmdlet Set-AzureStorageServiceMetricsProperty για να αλλάξετε τις τρέχουσες ρυθμίσεις.

Τα cmdlet που ελέγχουν μετρικά αποθήκευσης χρησιμοποιήστε τις παρακάτω παραμέτρους:

- MetricsType: πιθανές τιμές είναι ώρα και λεπτά.

- ServiceType: πιθανές τιμές είναι Blob, ουρά και πίνακα.

- MetricsLevel: πιθανές τιμές είναι κανένα, την υπηρεσία και ServiceAndApi.

Για παράδειγμα, την παρακάτω εντολή πραγματοποιεί εναλλαγή minute μετρικά για την υπηρεσία του προεπιλεγμένου λογαριασμού χώρου αποθήκευσης αντικειμένων Blob με το διάστημα που καθορίζεται σε πέντε ημέρες διατήρησης:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

Η ακόλουθη εντολή ανακτά την τρέχουσα ωριαία μετρικά επίπεδο και διατήρησης ημερών για την υπηρεσία του προεπιλεγμένου λογαριασμού χώρου αποθήκευσης αντικειμένων blob:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Για πληροφορίες σχετικά με τον τρόπο ρύθμισης παραμέτρων τα cmdlet του PowerShell Azure για να εργαστείτε με τη συνδρομή σας στο Azure και πώς μπορείτε να επιλέξετε τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης για να χρησιμοποιήσετε, δείτε: [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Πώς μπορείτε να ενεργοποιήσετε μετρικά αποθήκευσης μέσω προγραμματισμού

Το παρακάτω C# τμήμα κώδικα δείχνει πώς μπορείτε να ενεργοποιήσετε μετρικά και καταγραφής για την υπηρεσία Blob χρησιμοποιώντας τη βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για το .NET:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days.
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);


## <a name="viewing-storage-metrics"></a>Προβολή μετρικά αποθήκευσης

Αφού ρυθμίσετε μετρικά αποθήκευσης ανάλυσης για την παρακολούθηση της το λογαριασμό χώρου αποθήκευσης, αποθήκευσης ανάλυση εγγραφών τα μετρικά σε ένα σύνολο γνωστών πινάκων στο λογαριασμό σας στο χώρο αποθήκευσης. Μπορείτε να ρυθμίσετε τα γραφήματα για να προβάλετε ωριαία μετρικά στην [Πύλη του Azure](https://portal.azure.com):

1. Μεταβείτε στο λογαριασμό σας χώρου αποθήκευσης με την [Πύλη Azure](https://portal.azure.com).
2. Στην ενότητα **Παρακολούθηση** , κάντε κλικ στην επιλογή **Προσθήκη πλακιδίων** για να προσθέσετε ένα νέο γράφημα. Στη **Συλλογή πλακίδιο**, επιλέξτε τη μέτρηση που θέλετε να προβάλετε και σύρετέ το στην ενότητα **παρακολούθησης** .
3. Για να επεξεργαστείτε ποια μετρικά εμφανίζονται σε ένα γράφημα, κάντε κλικ στη σύνδεση **Επεξεργασία** . Μπορείτε να προσθέσετε ή να καταργήσετε μεμονωμένα μετρικά επιλέγοντας ή καταργώντας την επιλογή τους.
4. Όταν ολοκληρώσετε την επεξεργασία μετρικά, κάντε κλικ στην επιλογή **Αποθήκευση** .

Εάν θέλετε να κάνετε λήψη των μετρικών για μακροπρόθεσμες χώρο αποθήκευσης ή να αναλύσετε τα τοπικά, θα πρέπει να:

- Χρησιμοποιήστε ένα εργαλείο που γνωρίζει σε αυτούς τους πίνακες και θα σας επιτρέψει να προβάλετε και να κάνετε λήψη τους.
- Συντάξτε μια προσαρμοσμένη εφαρμογή ή δέσμη ενεργειών για να διαβάσετε και να αποθηκεύσετε τους πίνακες.

Πολλά εργαλεία περιήγησης στο χώρο αποθήκευσης τρίτων κατασκευαστών γνωρίζετε από αυτούς τους πίνακες και σας επιτρέπουν να τα προβάλετε απευθείας.
Ανατρέξτε στο θέμα [Azure εξερεύνησης χώρου αποθήκευσης](storage-explorers.md) για μια λίστα των διαθέσιμων εργαλείων.

> [AZURE.NOTE] Ξεκινώντας με την έκδοση 0.8.0 του [Εξερεύνηση του Microsoft Azure αποθήκευσης] (http://storageexplorer.com/), τώρα θα μπορείτε να προβάλετε και να κάνετε λήψη των πινάκων μετρικά ανάλυσης.

Για να αποκτήσετε πρόσβαση την ανάλυση πινάκων μέσω προγραμματισμού, σημειώστε ότι ανάλυση πινάκων δεν εμφανίζεται εάν λίστας όλους τους πίνακες στο λογαριασμό σας στο χώρο αποθήκευσης. Μπορείτε είτε πρόσβαση σε αυτά απευθείας με το όνομα, ή να χρησιμοποιήσετε το [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) στη βιβλιοθήκη του προγράμματος-πελάτη .NET ερώτημα για τα ονόματα πινάκων.

### <a name="hourly-metrics"></a>Ωριαία μετρικά
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>MINUTE μετρικά
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Χωρητικότητα
- $MetricsCapacityBlob

Μπορείτε να βρείτε πλήρεις λεπτομέρειες των σχημάτων για αυτούς τους πίνακες στο [Σχήμα πινάκων μετρικά ανάλυση χώρου αποθήκευσης](https://msdn.microsoft.com/library/azure/hh343264.aspx). Το παρακάτω δείγμα γραμμές εμφανίζονται μόνο ένα υποσύνολο από τις διαθέσιμες στήλες, αλλά απεικόνιση ορισμένες σημαντικές δυνατότητες του τρόπου με τον μετρικών αποθήκευσης που αποθηκεύει αυτές τις μετρήσεις:

| PartitionKey  |       RowKey       |                    Χρονική σήμανση | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Διαθεσιμότητα | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      χρήστη. Όλων      | 2014-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | χρήστη. QueryEntities | 2014-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143.8             | 7.8                  | 100            |
| 20140522T1100 |  χρήστη. QueryEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | χρήστη. UpdateEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

Σε αυτά τα δεδομένα minute μετρικά παράδειγμα, το πλήκτρο partition χρησιμοποιεί την ώρα σε minute ανάλυση. Το κλειδί γραμμή προσδιορίζει τον τύπο των πληροφοριών που είναι αποθηκευμένα στη γραμμή και αυτό αποτελείται από δύο στοιχεία πληροφοριών, τον τύπο πρόσβασης και τον τύπο αίτησης:

- Ο τύπος πρόσβασης είναι χρήστη ή συστήματος, όπου χρήστη αναφέρεται σε όλες τις αιτήσεις χρήστη με την υπηρεσία αποθήκευσης και σύστημα αναφέρεται σε αιτήματα της ανάλυσης χώρου αποθήκευσης.

- Ο τύπος αίτησης είναι είτε όλες στην οποία περίπτωση είναι μια συνοπτική γραμμή ή προσδιορίζει το συγκεκριμένο API όπως QueryEntity ή UpdateEntity.


Το παραπάνω δείγμα δεδομένων εμφανίζει όλες τις εγγραφές για ένα μεμονωμένο λεπτό (ξεκινώντας από 11:00 πμ), ώστε ο αριθμός των αιτήσεων QueryEntities καθώς και τον αριθμό των QueryEntity αιτήσεις συν τον αριθμό των UpdateEntity αιτήσεις Προσθήκη έως επτά, δηλαδή το συνολικό εμφανίζεται στη γραμμή χρήστη: όλων. Ομοίως, μπορείτε να προέρχεται ο μέσος όρος καθυστέρησης για ολοκληρωμένες 104.4286 στη γραμμή χρήστη: όλων από τον υπολογισμό ((143.8 * 5) + 3 + 9) / 7.

Πρέπει να λάβετε υπόψη τη ρύθμιση των ειδοποιήσεων στην [Πύλη του Azure](https://portal.azure.com) στη σελίδα "οθόνη", ώστε να μετρικών αποθήκευσης που μπορεί να ειδοποιήσει αυτόματα τις σημαντικές αλλαγές στην συμπεριφορά των υπηρεσιών σας χώρου αποθήκευσης. Εάν χρησιμοποιείτε ένα εργαλείο Εξερεύνηση χώρου αποθήκευσης για να κάνετε λήψη αυτών των δεδομένων μετρικά σε οριοθετημένου με μορφή, μπορείτε να χρησιμοποιήσετε το Microsoft Excel για να αναλύσετε τα δεδομένα. Ανατρέξτε στην καταχώρηση [Εξερεύνησης χώρου αποθήκευσης του Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) για μια λίστα με τα εργαλεία explorer διαθέσιμο χώρο αποθήκευσης ιστολογίου.



## <a name="accessing-metrics-data-programmatically"></a>Πρόσβαση στα δεδομένα του μετρικά μέσω προγραμματισμού

Η ακόλουθη λίστα εμφανίζει δείγμα C# κώδικα που έχει πρόσβαση των λεπτών μετρικών για μια περιοχή λεπτά και εμφανίζει τα αποτελέσματα σε ένα παράθυρο κονσόλας. Χρησιμοποιεί τη βιβλιοθήκη αποθήκευσης Azure έκδοση 4, η οποία περιλαμβάνει την κλάση CloudAnalyticsClient που απλοποιεί την πρόσβαση των πινάκων μετρήσεις στο χώρο αποθήκευσης.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
    select entity;

    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }

    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;

    }




## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Τι χρεώσεων κάντε λαμβάνετε όταν ενεργοποιείτε μετρικά αποθήκευσης;

Γράψτε αιτήσεις για τη δημιουργία πίνακα οντοτήτων για μετρικά θα χρεωθεί με τις τυπικές χρεώσεις που ισχύουν για όλες τις εργασίες αποθήκευσης Azure.

Ανάγνωση και διαγραφή αιτήσεις από ένα πρόγραμμα-πελάτη με μετρικά δεδομένα είναι επίσης χρεώσιμων τυπική επιτόκια. Εάν έχετε ρυθμίσει μια πολιτική διατήρησης δεδομένων, που δεν θα χρεωθείτε όταν αποθήκευσης Azure διαγράφει τα παλιά μετρικά δεδομένα. Ωστόσο, εάν διαγράψετε ανάλυση δεδομένων, ο λογαριασμός σας θα χρεωθεί για τις λειτουργίες delete.

Η δυναμικότητα που χρησιμοποιούνται από τους πίνακες μετρικά είναι επίσης χρεώσιμων: μπορείτε να χρησιμοποιήσετε τα εξής για να εκτιμήσετε την ποσότητα δυναμικότητας που χρησιμοποιείται για την αποθήκευση δεδομένων μετρικά:

- Εάν κάθε ώρα υπηρεσίας χρησιμοποιεί κάθε API σε κάθε υπηρεσία, στη συνέχεια, κατά προσέγγιση 148 Γνωσιακής Βάσης δεδομένων αποθηκεύεται κάθε ώρα στους πίνακες κίνησης μετρικά εάν έχετε ενεργοποιήσει την υπηρεσία και επίπεδο API σύνοψης.

- Εάν κάθε ώρα υπηρεσίας χρησιμοποιεί κάθε API σε κάθε υπηρεσία, στη συνέχεια, κατά προσέγγιση 12 Γνωσιακής Βάσης δεδομένων αποθηκεύεται κάθε ώρα στους πίνακες κίνησης μετρικά εάν έχετε ενεργοποιήσει το μόνο επίπεδο υπηρεσίας σύνοψης.

- Ο πίνακας χωρητικότητα για αντικείμενα blob έχει δύο γραμμές προστίθενται κάθε ημέρα (εφόσον ο χρήστης έχει επιλέξει για τα αρχεία καταγραφής): Αυτό σημαίνει ότι κάθε ημέρα το μέγεθος του πίνακα αυξάνεται κατά έως περίπου 300 byte.

## <a name="next-steps"></a>Επόμενο βήμα:
[Ενεργοποίηση αποθήκευσης καταγραφή και πρόσβαση σε δεδομένα του αρχείου καταγραφής](https://msdn.microsoft.com/library/dn782840.aspx)
