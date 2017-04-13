<properties
    pageTitle="Λίστα αποτελεσματική διαχείριση ερωτημάτων σε δέσμη Azure | Microsoft Azure"
    description="Αύξηση απόδοσης κατά το φιλτράρισμα των ερωτημάτων σας κατά την αίτηση για πληροφορίες σχετικά με πόρους δέσμη, όπως σύνολα, εργασίες και τις εργασίες και τον υπολογισμό κόμβους."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="query-the-azure-batch-service-efficiently"></a>Υποβολή ερωτήματος στην υπηρεσία δέσμη Azure αποτελεσματικά

Εδώ θα μάθετε πώς μπορείτε να αυξήσετε τις επιδόσεις της εφαρμογής σας δέσμη Azure μειώνοντας το μέγεθος των δεδομένων που επιστρέφεται από την υπηρεσία ερωτημάτων εργασίες, εργασίες, και τον υπολογισμό κόμβοι με το [.NET δέσμη] [ api_net] βιβλιοθήκη.

Σχεδόν όλες τις εφαρμογές δέσμης πρέπει να εκτελέσετε ορισμένες τον τύπο της λειτουργίας παρακολούθησης ή άλλο που εκτελεί ερωτήματα στην υπηρεσία δέσμη, συχνά σε τακτά χρονικά διαστήματα. Για παράδειγμα, για να προσδιορίσετε αν υπάρχουν όλες τις εργασίες σε ουρά που απομένει σε μια εργασία, πρέπει να λαμβάνετε δεδομένα σε κάθε εργασία της εργασίας. Για να προσδιορίσετε την κατάσταση της τους κόμβους του χώρου συγκέντρωσης, πρέπει να λάβετε δεδομένα σε κάθε κόμβο στο χώρο συγκέντρωσης. Σε αυτό το άρθρο εξηγεί τον τρόπο για την εκτέλεση αυτών ερωτημάτων με τον πιο αποτελεσματικό τρόπο.

## <a name="meet-the-detaillevel"></a>Πληροί τα DetailLevel

Σε μια εφαρμογή δέσμης τεχνική, οντοτήτων όπως εργασίες, τις εργασίες και κόμβους υπολογιστικών να αριθμήσετε στο χιλιάδων. Όταν κάνετε αίτηση για πληροφορίες σχετικά με αυτούς τους πόρους, πιθανώς μεγάλη ποσότητα δεδομένων πρέπει να "cross σύρματος" από την υπηρεσία δέσμη στην εφαρμογή σας σε κάθε ερώτημα. Με περιορισμό τον αριθμό των στοιχείων και τον τύπο των πληροφοριών που επιστρέφονται από ένα ερώτημα, μπορείτε να αυξήσετε την ταχύτητα των ερωτημάτων σας και, επομένως, οι επιδόσεις της εφαρμογής σας.

Αυτό [.NET δέσμη] [ api_net] τμήμα κώδικα API παραθέτει *κάθε* εργασίας που είναι συσχετισμένη με μια εργασία, μαζί με *όλες* τις ιδιότητες κάθε εργασίας:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Μπορείτε να εκτελέσετε ένα ερώτημα πολύ πιο αποτελεσματικές λίστας, ωστόσο, με την εφαρμογή "επίπεδο λεπτομερειών" στο ερώτημά σας. Κάνετε αυτό, παρέχοντας ένα [ODATADetailLevel] [ odata] αντικειμένου για να το [JobOperations.ListTasks] [ net_list_tasks] μέθοδο. Σε αυτό το τμήμα κώδικα επιστρέφει μόνο το Αναγνωριστικό γραμμής εντολών και ιδιότητες πληροφορίες κόμβου υπολογισμού των ολοκληρωμένων εργασιών:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

Σε αυτό το σενάριο παράδειγμα, εάν υπάρχουν χιλιάδες εργασίες της εργασίας, τα αποτελέσματα από το δεύτερο ερώτημα θα συνήθως επιστραφεί ιδιαίτερα γρηγορότερα τις από την πρώτη γραμμή. Περισσότερες πληροφορίες σχετικά με τη χρήση ODATADetailLevel όταν η λίστα των στοιχείων με το API .NET δέσμη είναι περιλαμβάνεται [κάτω από](#efficient-querying-in-batch-net).

> [AZURE.IMPORTANT]
> Προτείνουμε να που μπορείτε *πάντα* να παρέχετε ένα αντικείμενο ODATADetailLevel στις κλήσεις σας λίστα .NET API για να βεβαιωθείτε ότι μέγιστο απόδοσης και επίδοσης της εφαρμογής σας. Καθορίζοντας ένα επίπεδο λεπτομερειών, μπορείτε να βοηθήσετε να κάτω χρόνους απόκρισης υπηρεσίας δέσμη, βελτίωση της χρήσης δικτύου και ελαχιστοποίηση χρήση μνήμης με εφαρμογές προγράμματος-πελάτη.

## <a name="filter-select-and-expand"></a>Φιλτράρισμα, επιλέξτε και ανάπτυξη

[Δέσμη .NET] [ api_net] και [ΤΟΠΟΘΕΤΉΣΤΕ δέσμη] [ api_rest] APIs παρέχουν τη δυνατότητα να μειώσετε το πλήθος των στοιχείων που επιστρέφονται σε μια λίστα, καθώς και την ποσότητα των πληροφοριών που επιστρέφεται για κάθε μία. Μπορείτε να το κάνετε αυτό, καθορίζοντας **φίλτρο**, **Επιλέξτε**και **Ανάπτυξη συμβολοσειρές** κατά την εκτέλεση ερωτημάτων λίστας.

### <a name="filter"></a>Φίλτρο
Η συμβολοσειρά φίλτρου είναι μια παράσταση που μειώνει τον αριθμό των στοιχείων που επιστρέφονται. Για παράδειγμα, λίστα μόνο τις εργασίες που εκτελούνται για ένα έργο, ή τη λίστα μόνο κόμβους υπολογιστικών που είναι έτοιμες για την εκτέλεση εργασιών.

- Η συμβολοσειρά φίλτρου αποτελείται από μία ή περισσότερες εκφράσεις, με μια παράσταση που αποτελείται από ένα όνομα ιδιότητας, τελεστή και τιμή. Οι ιδιότητες που μπορεί να έχει καθοριστεί είναι συγκεκριμένες για κάθε τύπο οντότητα που υποβάλλετε ερώτημα, όπως είναι οι τελεστές που υποστηρίζονται για κάθε ιδιότητα.
- Πολλές παραστάσεις μπορεί να συνδυαστεί με τη χρήση τους λογικούς τελεστές `and` και `or`.
- Αυτό το παράδειγμα συμβολοσειρά λίστες φίλτρων μόνο για την εκτέλεση "απόδοση" εργασίες: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Επιλέξτε
Η επιλογή συμβολοσειρά όρια τις τιμές ιδιοτήτων που επιστρέφονται για κάθε στοιχείο. Καθορίστε μια λίστα με τα ονόματα των ιδιοτήτων και επιστρέφονται μόνο τις τιμές ιδιοτήτων για τα στοιχεία στα αποτελέσματα του ερωτήματος.

- Η επιλογή συμβολοσειρά που αποτελείται από μια λίστα διαχωρισμένες με κόμματα ονόματα ιδιοτήτων. Μπορείτε να καθορίσετε οποιαδήποτε από τις ιδιότητες για τον τύπο οντότητας ερωτήματός σας.
- Σε αυτό το παράδειγμα επιλέξτε συμβολοσειρά Καθορίζει ότι πρέπει να επιστραφεί μόνο τρεις τιμές ιδιοτήτων για κάθε εργασία: `id, state, stateTransitionTime`.

### <a name="expand"></a>Αναπτύξτε το στοιχείο
Η συμβολοσειρά ανάπτυξη μειώνει το πλήθος των κλήσεων API που απαιτούνται για να αποκτήσετε ορισμένες πληροφορίες. Όταν χρησιμοποιείτε μια συμβολοσειρά ανάπτυξη, μπορείτε να αποκτήσετε περισσότερες πληροφορίες σχετικά με κάθε στοιχείο με μια μεμονωμένη κλήση API. Αντί να πρώτα να αποκτήσετε τη λίστα των οντοτήτων, στη συνέχεια, να ζητά πληροφορίες για κάθε στοιχείο στη λίστα, μπορείτε να χρησιμοποιήσετε μια συμβολοσειρά ανάπτυξης για να αποκτήσετε τις ίδιες πληροφορίες σε μια μεμονωμένη κλήση API. Λιγότερο κλήσεις API σημαίνει καλύτερη απόδοση.

- Παρόμοια με την επιλογή συμβολοσειρά, η συμβολοσειρά ανάπτυξη ελέγχει αν ορισμένα δεδομένα περιλαμβάνεται στη λίστα αποτελέσματα ερωτήματος.
- Η συμβολοσειρά ανάπτυξη υποστηρίζεται μόνο όταν χρησιμοποιείται σε καταχώρηση εργασίες, χρονοδιαγράμματα έργου, εργασίες και τα σύνολα. Προς το παρόν, υποστηρίζει μόνο στατιστικά στοιχεία.
- Όταν όλες οι ιδιότητες που απαιτούνται και να έχει καθοριστεί χωρίς επιλογή συμβολοσειρά, συμβολοσειρά ανάπτυξη, *πρέπει να* χρησιμοποιηθεί για να λάβετε πληροφορίες στατιστικών στοιχείων. Εάν μια συμβολοσειρά επιλογής χρησιμοποιείται για να αποκτήσετε ένα υποσύνολο των ιδιοτήτων, στη συνέχεια, `stats` μπορεί να καθοριστεί στη συμβολοσειρά επιλογής και, στη συμβολοσειρά επέκτασης δεν χρειάζεται να έχει καθοριστεί.
- Αυτό το παράδειγμα ανάπτυξη συμβολοσειρά Καθορίζει ότι θα πρέπει να επιστραφούν στατιστικά στοιχεία για κάθε στοιχείο της λίστας: `stats`.

> [AZURE.NOTE] Όταν η κατασκευή οποιονδήποτε από τους τύπους συμβολοσειρά τρεις ερωτήματος (φιλτράρισμα, επιλέξτε και αναπτύξτε το στοιχείο), θα πρέπει να βεβαιωθείτε ότι τα ονόματα ιδιοτήτων και πεζών-κεφαλαίων ταιριάζουν από τα αντίστοιχα στοιχείο REST API. Για παράδειγμα, όταν εργάζεστε με την κλάση.NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) , πρέπει να καθορίσετε **κατάσταση** αντί για το **νομό**, ακόμα και όταν η ιδιότητα .NET είναι [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Ανατρέξτε στους πίνακες παρακάτω για αντιστοιχίσεις ιδιοτήτων μεταξύ του .NET και REST API του Yammer.

### <a name="rules-for-filter-select-and-expand-strings"></a>Κανόνες για το φίλτρο, επιλέξτε και ανάπτυξη συμβολοσειρές

- Ιδιότητες ονόματα στο φίλτρο, επιλέξτε και ανάπτυξη συμβολοσειρές θα πρέπει να εμφανίζονται όπως εμφανίζονται τα [ΥΠΌΛΟΙΠΑ δέσμη] [ api_rest] API--ακόμα και όταν χρησιμοποιείτε [Δέσμη .NET] [ api_net] ή σε ένα από τα άλλα δέσμη SDK.
- Όλα τα ονόματα ιδιότητα διάκριση πεζών-κεφαλαίων, αλλά οι τιμές ιδιοτήτων είναι διάκριση πεζών-κεφαλαίων.
- Ημερομηνία/ώρα συμβολοσειρών μπορεί να είναι μία από δύο μορφές και πρέπει να προηγείται με `DateTime`.

  - Παράδειγμα μορφή W3C-DTF:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - Παράδειγμα μορφής RFC 1123:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Δυαδική συμβολοσειρές είτε βρίσκονται `true` ή `false`.
- Εάν έχει καθοριστεί μια ιδιότητα δεν είναι έγκυρη ή τελεστή, μια `400 (Bad Request)` θα έχει ως αποτέλεσμα σφάλμα.

## <a name="efficient-querying-in-batch-net"></a>Αποτελεσματική υποβολή ερωτημάτων στο .NET δέσμης

Εντός του [.NET δέσμη] [ api_net] API, το [ODATADetailLevel] [ odata] κλάση χρησιμοποιείται για την παροχή φίλτρο, επιλέξτε και ανάπτυξη συμβολοσειρών σε λίστα εργασίες. Η κλάση ODataDetailLevel έχει τρεις ιδιότητες δημόσια συμβολοσειρά που μπορεί να καθορίσει στην κατασκευή ή να ορίσετε απευθείας στο αντικείμενο. Στη συνέχεια, περάσετε το αντικείμενο ODataDetailLevel ως παράμετρο, για να τις διάφορες λειτουργίες λίστας όπως [ListPools][net_list_pools], [ListJobs][net_list_jobs], και [ListTasks][net_list_tasks].

- [ODATADetailLevel][odata]. [FilterClause] [ odata_filter]: Περιορισμός του αριθμού των στοιχείων που επιστρέφονται.
- [ODATADetailLevel][odata]. [SelectClause] [ odata_select]: Καθορίστε ποιες τιμές ιδιοτήτων επιστρέφονται με κάθε στοιχείο.
- [ODATADetailLevel][odata]. [ExpandClause] [ odata_expand]: Ανάκτηση δεδομένων για όλα τα στοιχεία σε ένα μεμονωμένο API κλήση αντί για το νέο κλήσεις για κάθε στοιχείο.

Το παρακάτω τμήμα κώδικα χρησιμοποιεί το API .NET δέσμη αποτελεσματικά ερώτημα για την υπηρεσία δέσμη για τα στατιστικά στοιχεία από ένα συγκεκριμένο σύνολο χώρους συγκέντρωσης. Σε αυτό το σενάριο, ο χρήστης δέσμη έχει δοκιμών και παραγωγής χώρους συγκέντρωσης. Ο χώρος συγκέντρωσης δοκιμής αναγνωριστικά είναι το πρόθεμα "δοκιμή" και το χώρο συγκέντρωσης παραγωγής αναγνωριστικά είναι το πρόθεμα "Ειδών". Στο το τμήμα κώδικα, *myBatchClient* είναι μια έχει προετοιμαστεί κατάλληλα παρουσία της κλάσης [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) .

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] Μια παρουσία του [ODATADetailLevel] [ odata] που έχει ρυθμιστεί με επιλογή και ανάπτυξη όρους επίσης μπορούν να περάσουν στα κατάλληλα μεθόδους Get, όπως [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), για να περιορίσετε την ποσότητα των δεδομένων που επιστρέφονται.

## <a name="batch-rest-to-net-api-mappings"></a>Μαζική REST .NET API αντιστοιχίσεις

Ονόματα ιδιοτήτων στο φίλτρο, επιλέξτε και ανάπτυξη συμβολοσειρές *πρέπει να* αντικατοπτρίζουν τα αντίστοιχα REST API, τόσο στο όνομα και πεζών-κεφαλαίων. Οι παρακάτω πίνακες παρέχουν αντιστοιχίσεις μεταξύ τα αντίστοιχα .NET και REST API.

### <a name="mappings-for-filter-strings"></a>Αντιστοιχίσεις για τις συμβολοσειρές φίλτρου

- **Μέθοδοι λίστα .NET**: κάθε μία από τις μεθόδους .NET API σε αυτήν τη στήλη αποδέχεται μια [ODATADetailLevel] [ odata] αντικείμενο ως παράμετρο.
- **ΥΠΌΛΟΙΠΟ αιτήσεις λίστας**: κάθε REST API σελίδα συνδέεται με σε αυτήν τη στήλη περιέχει έναν πίνακα που καθορίζει τις ιδιότητες και τις λειτουργίες που επιτρέπεται σε συμβολοσειρές *φίλτρο* . Θα χρησιμοποιήσετε αυτές τις λειτουργίες και τα ονόματα των ιδιοτήτων όταν δημιουργείτε μια [ODATADetailLevel.FilterClause] [ odata_filter] συμβολοσειρά.

| Μέθοδοι λίστα .NET | ΥΠΌΛΟΙΠΟ αιτήσεις λίστας |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [Λίστα με τα πιστοποιητικά σε ένα λογαριασμό][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [Λίστα των αρχείων που σχετίζονται με μια εργασία][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [Λίστα της κατάστασης των το εργασία προετοιμασίας και τις εργασίες κυκλοφορία για ένα έργο][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [Λίστα των εργασιών σε ένα λογαριασμό][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [Λίστα με τα αρχεία σε έναν κόμβο][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [Λίστα των εργασιών που σχετίζονται με μια εργασία][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [Λίστα με τα χρονοδιαγράμματα εργασία σε ένα λογαριασμό][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [Λίστα με τις εργασίες που σχετίζονται με ένα χρονοδιάγραμμα έργου][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [Λίστα τους κόμβους υπολογισμού σε ένα χώρο συγκέντρωσης][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [Λίστα με τους χώρους συγκέντρωσης σε ένα λογαριασμό][rest_list_pools]

### <a name="mappings-for-select-strings"></a>Αντιστοιχίσεις για επιλογή συμβολοσειρές

- **Τύποι .NET δέσμη**: τύποι API .NET δέσμης.
- **Οντοτήτων REST API**: κάθε σελίδα σε αυτήν τη στήλη περιέχει έναν ή περισσότερους πίνακες που παραθέτουν τα ονόματα των ιδιοτήτων REST API για τον τύπο. Αυτά τα ονόματα των ιδιοτήτων χρησιμοποιούνται όταν δημιουργείτε *Επιλέξτε* συμβολοσειρές. Θα μπορείτε να χρησιμοποιήσετε αυτά τα ίδια ονόματα ιδιοτήτων όταν δημιουργείτε μια [ODATADetailLevel.SelectClause] [ odata_select] συμβολοσειρά.

| Τύποι .NET δέσμης | Οντοτήτων REST API |
|---|---|
| [Πιστοποιητικό][net_cert] | [Λήψη πληροφοριών σχετικά με ένα πιστοποιητικό][rest_get_cert] |
| [CloudJob][net_job] | [Λήψη πληροφοριών σχετικά με μια εργασία][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [Λήψη πληροφοριών σχετικά με ένα χρονοδιάγραμμα έργου][rest_get_schedule] |
| [ComputeNode][net_node] | [Λήψη πληροφοριών σχετικά με έναν κόμβο][rest_get_node] |
| [CloudPool][net_pool] | [Λήψη πληροφοριών σχετικά με ένα χώρο συγκέντρωσης][rest_get_pool] |
| [CloudTask][net_task] | [Λήψη πληροφοριών σχετικά με μια εργασία][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Παράδειγμα: δημιουργήσετε μια συμβολοσειρά φίλτρου

Όταν δημιουργείτε μια συμβολοσειρά φίλτρου για [ODATADetailLevel.FilterClause][odata_filter], συμβουλευτείτε παραπάνω πίνακα στην περιοχή "Αντιστοιχίσεις για τις συμβολοσειρές φίλτρο" στη σελίδα τεκμηρίωση Εύρεση το REST API που αντιστοιχεί στη λειτουργία λίστας που θέλετε να εκτελέσετε. Θα βρείτε τις ιδιότητες με δυνατότητα φιλτραρίσματος και τους τελεστές που υποστηρίζονται στον πρώτο multirow πίνακα σε αυτήν τη σελίδα. Εάν θέλετε να ανακτήσετε όλες τις εργασίες του οποίου κωδικό εξόδου ήταν εκτός από μηδέν, για παράδειγμα, αυτή η γραμμή στη [λίστα των εργασιών που σχετίζονται με μια εργασία] [ rest_list_tasks] Καθορίζει τη συμβολοσειρά που εφαρμόζονται ιδιότητα και το επιτρεπόμενο τελεστές:

| Ιδιότητα | Λειτουργίες που επιτρέπονται | Τύπος |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

Έτσι, η συμβολοσειρά φίλτρου για παραθέτει όλες τις εργασίες με έναν κωδικό εκτός από μηδέν εξόδου θα είναι:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Παράδειγμα: δημιουργία συμβολοσειράς επιλογής

Για να δημιουργήσετε [ODATADetailLevel.SelectClause][odata_select], συμβουλευτείτε την παραπάνω πίνακα, στην περιοχή "Αντιστοιχίσεις για επιλογή συμβολοσειρές" και μεταβείτε στη σελίδα REST API που αντιστοιχεί στον τύπο της οντότητας που που καταχώρηση. Θα βρείτε τις ιδιότητες δυνατότητα επιλογής και τους τελεστές που υποστηρίζονται στον πρώτο πίνακα multirow σε αυτήν τη σελίδα. Εάν θέλετε να ανακτήσετε μόνο το Αναγνωριστικό και τη γραμμή εντολών για κάθε εργασία σε μια λίστα, για παράδειγμα, θα βρείτε αυτές τις γραμμές στον πίνακα που εφαρμόζονται σε [λήψη πληροφοριών σχετικά με μια εργασία][rest_get_task]:

| Ιδιότητα | Τύπος | Σημειώσεις |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

Η επιλογή συμβολοσειρά για τη συμπερίληψη μόνο το Αναγνωριστικό και τη γραμμή εντολών με κάθε εργασίας που παρατίθενται, στη συνέχεια, πρέπει να είναι:

`id, commandLine`

## <a name="code-samples"></a>Δείγματα κώδικα

### <a name="efficient-list-queries-code-sample"></a>Δείγμα κώδικα ερωτήματα λίστας μικρότερες απαιτήσεις

Ανατρέξτε στο θέμα το [EfficientListQueries] [ efficient_query_sample] δείγμα έργου σε GitHub για να δείτε πώς αποτελεσματική υποβολή ερωτημάτων λίστας μπορεί να επηρεάσει την απόδοση σε μια εφαρμογή του. Αυτό εφαρμογής κονσόλας C# δημιουργεί και προσθέτει μια πολλές εργασίες σε μια εργασία. Στη συνέχεια, πραγματοποιεί πολλές κλήσεις σε το [JobOperations.ListTasks] [ net_list_tasks] μέθοδο και τις φάσεις [ODATADetailLevel] [ odata] αντικειμένων που έχουν ρυθμιστεί με άλλες τιμές ιδιοτήτων για να αλλάξετε την ποσότητα των δεδομένων που θα επιστραφεί. Υπολογίζονται εξόδου παρόμοια με τα εξής:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Όπως φαίνεται στην τις ώρες που πέρασε, μπορείτε να μειώσετε σημαντικά χρόνους απόκρισης ερωτήματος με περιορισμό τις ιδιότητες και τον αριθμό των στοιχείων που επιστρέφονται. Μπορείτε να βρείτε αυτό και άλλα έργα δείγμα [azure-δέσμη-δείγματα] [ github_samples] αποθετήριο δεδομένων σε GitHub.

### <a name="batchmetrics-library-and-code-sample"></a>Δείγμα βιβλιοθήκης και τον κωδικό BatchMetrics

Εκτός από το EfficientListQueries δείγμα κώδικα παραπάνω, μπορείτε να βρείτε το [BatchMetrics] [ batch_metrics] έργου [azure-δέσμη-δείγματα] [ github_samples] GitHub αποθετήριο. Το δείγμα έργου BatchMetrics παρουσιάζει τον τρόπο για την παρακολούθηση της προόδου εργασίας Azure δέσμη με χρήση του API δέσμη αποτελεσματικά.

Το [BatchMetrics] [ batch_metrics] δείγμα περιλαμβάνει ενός έργου βιβλιοθήκη κλάσης .NET που μπορείτε να ενσωματώσετε σε δικές σας εργασίες και ένα απλό πρόγραμμα της γραμμής εντολών για να άσκηση και δείχνουν τη χρήση της βιβλιοθήκης.

Το δείγμα εφαρμογής μέσα στο έργο παρουσιάζει τις ακόλουθες λειτουργίες:

1. Επιλογή συγκεκριμένες ιδιότητες για να κάνετε λήψη μόνο τις ιδιότητες που χρειάζεστε
2. Φιλτράρισμα σε κατάσταση μετάβασης ώρες προκειμένου να λήψη μόνο αλλαγών μετά την τελευταία ερωτήματος

Για παράδειγμα, την ακόλουθη μέθοδο εμφανίζεται στη βιβλιοθήκη BatchMetrics. Επιστρέφει μια ODATADetailLevel που καθορίζει ότι μόνο η `id` και `state` ιδιότητες θα πρέπει να ληφθούν για την οντοτήτων που υποβάλλεται ερώτημα. Που καθορίζει επίσης μόνο οντοτήτων του οποίου η κατάσταση έχει αλλάξει από το καθορισμένο `DateTime` πρέπει να επιστραφεί η παράμετρος.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Επόμενα βήματα

### <a name="parallel-node-tasks"></a>Εργασίες παράλληλες κόμβου

[Μεγιστοποίηση δέσμη Azure τον υπολογισμό χρήση πόρων με ταυτόχρονες κόμβο εργασίες](batch-parallel-node-tasks.md) είναι ένα άλλο άρθρο που σχετίζονται με τις επιδόσεις εφαρμογής δέσμης. Ορισμένοι τύποι φόρτους εργασίας μπορούν να επωφεληθούν από την εκτέλεση παράλληλες εργασίες σε μεγαλύτερο--αλλά λιγότερες--τον υπολογισμό κόμβους. Δείτε το [παράδειγμα σενάριο](batch-parallel-node-tasks.md#example-scenario) στο άρθρο για λεπτομέρειες σχετικά με ένα τέτοιο σενάριο.

### <a name="batch-forum"></a>Φόρουμ δέσμης

Το [Φόρουμ δέσμη Azure] [ forum] στο MSDN είναι ένα καλό σημείο για να συζητήσετε δέσμης και να υποβάλετε ερωτήσεις σχετικά με την υπηρεσία. Κεφαλίδας στο επάνω από για χρήσιμες δημοσιεύσεις "ηλεκτρονικές", και δημοσιεύστε τις ερωτήσεις σας καθώς προκύπτουν ενώ δημιουργείτε τις λύσεις δέσμης.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
