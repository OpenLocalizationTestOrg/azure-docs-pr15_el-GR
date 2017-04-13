<properties
   pageTitle="Αναβάθμιση στο SDK .NET αναζήτησης Azure έκδοση 1.1 | Microsoft Azure | Υπηρεσία αναζήτησης φιλοξενούμενη cloud"
   description="Αναβάθμιση στο SDK .NET αναζήτησης Azure έκδοση 1.1"
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/15/2016"
   ms.author="brjohnst"/>

# <a name="upgrading-to-the-azure-search-net-sdk-version-20-preview"></a>Αναβάθμιση σε το Azure αναζήτησης .NET SDK έκδοση 2.0-preview

Εάν χρησιμοποιείτε έκδοση 1.1 ή παλαιότερη από το [Azure αναζήτησης .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx), αυτό το άρθρο θα σας βοηθήσει να αναβαθμίσετε την εφαρμογή σας για να χρησιμοποιήσετε την επόμενη κύρια έκδοση, 2.0 preview.

Για μια πιο γενική περιγραφή του SDK συμπεριλαμβανομένων παραδείγματα, δείτε [πώς μπορείτε να χρησιμοποιήσετε την αναζήτηση Azure από μια εφαρμογή .NET](search-howto-dotnet-sdk.md).

Προεπισκόπηση έκδοση 2.0 του SDK Azure αναζήτησης .NET περιέχει ορισμένες αλλαγές από προηγούμενες εκδόσεις. Αυτά είναι κυρίως δευτερεύουσες, ώστε να αλλάξετε τον κωδικό, θα πρέπει να απαιτούν μόνο ελάχιστη προσπάθεια. Δείτε [τα βήματα για να αναβαθμίσετε](#UpgradeSteps) για οδηγίες σχετικά με τον τρόπο για να αλλάξετε τον κωδικό για να χρησιμοποιήσετε τη νέα έκδοση SDK.

> [AZURE.NOTE] Εάν χρησιμοποιείτε την έκδοση preview 0,13 ή παλαιότερης έκδοσης, θα πρέπει να μπορείτε να κάνετε αναβάθμιση στην έκδοση 1.1 πρώτη και, στη συνέχεια, αναβάθμιση σε προεπισκόπηση 2.0. Ανατρέξτε στο θέμα [προσάρτημα: βήματα για να κάνετε αναβάθμιση στην έκδοση 1.1](#UpgradeStepsV1) για οδηγίες.

<a name="WhatsNew"></a>
## <a name="whats-new-in-version-20-preview"></a>Τι νέο υπάρχει στην έκδοση 2.0-preview

Έκδοση 2.0-preview είναι η πρώτη έκδοση του Azure SDK .NET αναζήτησης για να στοχεύουν σε μια προέκδοση των το Azure Search REST API, ειδικά 2015-02-28-preview. Αυτό δίνει τη δυνατότητα να χρησιμοποιήσετε πολλές δυνατότητες προεπισκόπηση Azure αναζήτησης από μια εφαρμογή .NET, συμπεριλαμβανομένων των εξής:

- [Προσαρμοσμένη αναλυτές](https://aka.ms/customanalyzers)
- Υποστήριξη ευρετηρίου [Χώρο αποθήκευσης Blob του Azure](search-howto-indexing-azure-blob-storage.md) και [Χώρος αποθήκευσης πινάκων του Azure](search-howto-indexing-azure-tables.md)
- Προσαρμογή ευρετηρίου μέσω [αντιστοιχίσεις πεδίων](search-indexer-field-mappings.md)
- Υποστήριξη ETags για να ενεργοποιήσετε την ασφαλή ταυτόχρονες ενημέρωση ευρετηρίου ορισμών, οι δεικτοδότες και προελεύσεις δεδομένων
- Υποστήριξη για .NET πυρήνα και προφίλ Portable .NET 111

<a name="UpgradeSteps"></a>
## <a name="steps-to-upgrade"></a>Βήματα για να κάνετε αναβάθμιση

Πρώτα, ενημερώστε την αναφορά NuGet για `Microsoft.Azure.Search` χρησιμοποιώντας είτε την Κονσόλα διαχείρισης πακέτου NuGet ή δεξί κλικ σε αναφορές του έργου σας και η επιλογή "Διαχείριση NuGet πακέτων..." στο Visual Studio. Βεβαιωθείτε ότι ενεργοποιείτε την προ-έκδοση πακέτων, επιλέγοντας "Συμπερίληψη προέκδοση" στο το NuGet "Διαχείριση πακέτων" παράθυρο στο Visual Studio ή με τη χρήση του `-IncludePrerelease` εναλλαγή Εάν χρησιμοποιείτε Κονσόλα διαχείρισης πακέτου NuGet.

Αφού NuGet έχει ληφθεί τα νέα πακέτα και τους εξαρτήσεις, δημιουργήστε ξανά το έργο σας. Ανάλογα με τη δομή του κώδικα, αυτό μπορεί να εκ νέου δημιουργία με επιτυχία. Εάν Ναι, είστε έτοιμοι να ξεκινήσετε!

Εάν αποτύχει η Δόμηση σας, θα πρέπει να βλέπετε ένα σφάλμα Δόμηση, όπως τα εξής:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Το επόμενο βήμα είναι να διορθώσετε αυτό το σφάλμα Δόμηση. Για λεπτομέρειες σχετικά με τι έχει ως αποτέλεσμα το σφάλμα και πώς μπορείτε να διορθώσετε αυτό το πρόβλημα, ανατρέξτε στο θέμα [Διακοπή αλλαγές στην έκδοση 2.0-προεπισκόπηση](#ListOfChanges) .

Μπορείτε να δείτε πρόσθετες Δόμηση προειδοποιήσεις σχετικά με την παλιά μεθόδους ή τις ιδιότητες. Οι ειδοποιήσεις θα περιλαμβάνει οδηγίες σχετικά με το τι μπορείτε να χρησιμοποιήσετε αντί της δυνατότητας που έχουν καταργηθεί. Για παράδειγμα, εάν η εφαρμογή σας χρησιμοποιεί το `SearchRequestOptions.RequestId` ιδιότητα, θα πρέπει να λαμβάνετε ένα προειδοποιητικό μήνυμα που αναφέρει ότι`"This property is deprecated. Please use ClientRequestId instead."`

Μόλις διορθώσετε σφάλματα Δόμηση, μπορείτε να κάνετε αλλαγές στην εφαρμογή σας για να εκμεταλλευτείτε τις νέες λειτουργίες, εάν θέλετε. Νέες δυνατότητες στο SDK είναι λεπτομερή σε [Τι είναι το νέο στην έκδοση 2.0-προεπισκόπηση](#WhatsNew).

<a name="ListOfChanges"></a>
## <a name="breaking-changes-in-version-20-preview"></a>Διακοπή αλλαγές στην έκδοση 2.0 preview

Υπάρχει μόνο μία αλλαγή πρόσφατες στην έκδοση 2.0-προεπισκόπηση που ενδέχεται να απαιτείται κωδικός αλλαγές εκτός από φτιάχνετε την εφαρμογή σας.

### <a name="indexesgetclient-return-type"></a>Τύπος επιστροφής Indexes.GetClient

Το `Indexes.GetClient` μέθοδος έχει έναν νέο τύπο επιστροφής. Στο παρελθόν, επιστρέφεται `SearchIndexClient`, αλλά αυτό έχει αλλάξει σε `ISearchIndexClient` στην έκδοση 2.0-προεπισκόπηση. Πρόκειται για την υποστήριξη πελατών, στους οποίους θέλετε να mock το `GetClient` μέθοδο για τις δοκιμές μονάδας, επιστρέφοντας μια mock υλοποίηση του `ISearchIndexClient`.

#### <a name="example"></a>Παράδειγμα

Εάν ο κώδικας είναι κάπως έτσι:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Μπορείτε να την αλλάξετε σε αυτήν για να διορθώσετε σφάλματα Δόμηση:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

## <a name="conclusion"></a>Ολοκλήρωση
Εάν χρειάζεστε περισσότερες λεπτομέρειες σχετικά με το .NET SDK Azure αναζήτησης, ανατρέξτε στο θέμα τα πιο πρόσφατα ενημερωμένα [οδηγίες](search-howto-dotnet-sdk.md).

Θα σας Καλώς ορίσατε τα σχόλιά σας στο SDK. Εάν αντιμετωπίσετε προβλήματα, μην διστάσεις να μας ζητήστε βοήθεια στο [Φόρουμ του Azure στο MSDN αναζήτησης](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuresearch). Εάν δεν μπορείτε να βρείτε ένα σφάλμα, μπορείτε να υποβάλετε ένα ζήτημα στο [αποθετήριο δεδομένων Azure .NET SDK GitHub](https://github.com/Azure/azure-sdk-for-net/issues). Βεβαιωθείτε ότι έχετε τον τίτλο πρόβλημα με το πρόθεμα "Αναζήτηση SDK:".

Σας ευχαριστούμε για τη χρήση αναζήτησης Azure!

<a name="UpgradeStepsV1"></a>
## <a name="appendix-steps-to-upgrade-to-version-11"></a>Προσάρτημα: Βήματα για να κάνετε αναβάθμιση στην έκδοση 1.1 

> [AZURE.NOTE] Αυτή η ενότητα ισχύει μόνο για τους χρήστες της έκδοσης Azure αναζήτησης .NET SDK 0,13 προεπισκόπηση και παλαιότερες εκδόσεις.

Πρώτα, ενημερώστε την αναφορά NuGet για `Microsoft.Azure.Search` χρησιμοποιώντας είτε την Κονσόλα διαχείρισης πακέτου NuGet ή δεξί κλικ σε αναφορές του έργου σας και η επιλογή "Διαχείριση NuGet πακέτων..." στο Visual Studio.

Αφού NuGet έχει ληφθεί τα νέα πακέτα και τους εξαρτήσεις, δημιουργήστε ξανά το έργο σας.

Εάν χρησιμοποιούσατε προηγουμένως 1.0.0-preview έκδοση, 1.0.1-preview ή 1.0.2-preview, θα πρέπει να ολοκληρωθεί με επιτυχία η Δόμηση και είστε έτοιμοι να ξεκινήσετε!

Εάν χρησιμοποιούσατε προηγουμένως 0.13.0-preview έκδοση ή παλαιότερης έκδοσης, θα πρέπει να βλέπετε Δόμηση σφάλματα, όπως τα εξής:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Το επόμενο βήμα είναι να διορθώσετε τα σφάλματα Δόμηση ένα προς ένα. Οι περισσότερες θα απαιτούν ορισμένα ονόματα κλάσης και τη μέθοδο που έχουν μετονομαστεί στο SDK για την αλλαγή. [Λίστα του ορίου αλλαγές στην έκδοση 1.1](#ListOfChangesV1) περιέχει μια λίστα με αυτές τις αλλαγές ονομάτων.

Εάν χρησιμοποιείτε προσαρμοσμένες κλάσεις μοντέλο τα έγγραφά σας, ενώ οι κατηγορίες ιδιότητες της μη τύπο nullable στοιχειώδεις τύπους (για παράδειγμα, `int` ή `bool` σε C#), υπάρχει μια επιδιόρθωση σφαλμάτων στην έκδοση 1.1 του SDK των οποίων θα πρέπει να γνωρίζετε. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [διορθώσεις σφαλμάτων στην έκδοση 1.1](#BugFixesV1) .

Τέλος, μόλις διορθώσετε σφάλματα Δόμηση, μπορείτε να κάνετε αλλαγές στην εφαρμογή σας για να εκμεταλλευτείτε τις νέες λειτουργίες, εάν θέλετε.

<a name="ListOfChangesV1"></a>
### <a name="list-of-breaking-changes-in-version-11"></a>Λίστα του ορίου αλλαγές στην έκδοση 1.1

Η παρακάτω λίστα είναι ταξινομημένες κατά την πιθανότητα ότι η αλλαγή θα επηρεάσει κώδικα της εφαρμογής σας.

#### <a name="indexbatch-and-indexaction-changes"></a>Αλλαγές IndexBatch και IndexAction

`IndexBatch.Create`έχει μετονομαστεί σε `IndexBatch.New` και δεν έχει πλέον ένα `params` όρισμα. Μπορείτε να χρησιμοποιήσετε `IndexBatch.New` για τις δέσμες που Δημιουργήστε διαφορετικούς τύπους ενεργειών (συγχωνεύει, διαγράφει, κ.λπ.). Επιπλέον, υπάρχουν νέα στατική μέθοδοι για τη δημιουργία δέσμες όπου όλες τις ενέργειες που είναι ίδιες: `Delete`, `Merge`, `MergeOrUpload`, και `Upload`.

`IndexAction`δεν έχει πλέον δημόσια κατασκευή και τώρα είναι αμετάβλητες και τις ιδιότητές του. Θα πρέπει να χρησιμοποιείτε τις νέες στατική μεθόδους για τη δημιουργία ενέργειες για διαφορετικούς λόγους: `Delete`, `Merge`, `MergeOrUpload`, και `Upload`. `IndexAction.Create`έχει καταργηθεί. Αν χρησιμοποιήσατε το υπερφόρτωσης που χρειάζονται μόνο ένα έγγραφο, βεβαιωθείτε ότι χρησιμοποιείτε `Upload` αντί για αυτό.

##### <a name="example"></a>Παράδειγμα

Εάν ο κώδικας είναι κάπως έτσι:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Μπορείτε να την αλλάξετε σε αυτήν για να διορθώσετε σφάλματα Δόμηση:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Εάν θέλετε, μπορείτε να περαιτέρω απλοποιήσετε το εξής:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>Αλλαγές IndexBatchException

Το `IndexBatchException.IndexResponse` ιδιότητα έχει μετονομαστεί σε `IndexingResults`, και ο τύπος είναι τώρα `IList<IndexingResult>`.

##### <a name="example"></a>Παράδειγμα

Εάν ο κώδικας είναι κάπως έτσι:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Μπορείτε να την αλλάξετε σε αυτήν για να διορθώσετε σφάλματα Δόμηση:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>
#### <a name="operation-method-changes"></a>Οι αλλαγές της μεθόδου εργασίας

Κάθε λειτουργία στο SDK Azure αναζήτησης .NET εκτίθεται ως σύνολο τύποι μέθοδο για σύγχρονη και ασύγχρονης καλούντες. Οι υπογραφές και μορφής της αυτές οι τύποι μέθοδος έχει αλλάξει στην έκδοση 1.1.

Για παράδειγμα, η λειτουργία "Λήψη ευρετηρίου στατιστικά στοιχεία" σε παλαιότερες εκδόσεις του SDK εκτίθεται αυτές οι υπογραφές:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Οι υπογραφές μέθοδο για την ίδια λειτουργία στην έκδοση 1.1 μοιάζει κάπως έτσι:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Ξεκινώντας με την έκδοση 1.1, το Azure αναζήτησης .NET SDK οργανώνουν μεθόδους λειτουργίας με διαφορετικό τρόπο:

 - Προαιρετικές παράμετροι μοντελοποιούνται τώρα ως προεπιλεγμένου παράμετροι προτιμάτε από τη μέθοδο επιπλέον τύποι. Αυτό μειώνει τον αριθμό των τύποι μεθόδου, ορισμένες φορές εντυπωσιακά.
 - Οι μέθοδοι επέκταση Απόκρυψη τώρα πολλές τα περιττά λεπτομέρειες HTTP από τον καλούντα. Για παράδειγμα, παλαιότερες εκδόσεις του SDK επέστρεψε μια απόκριση αντικείμενο με έναν κωδικό κατάστασης HTTP, το οποίο συχνά δεν πρέπει να ελέγξετε επειδή εμφανίσουν μεθόδους λειτουργίας `CloudException` για οποιοδήποτε κωδικός κατάστασης που υποδεικνύει ένα μήνυμα σφάλματος. Η νέα μέθοδοι επέκταση επιστρέφουν μόνο μοντέλο αντικειμένων, χωρίς να τον κόπο της χρειάζεται να αναιρέσετε την αναδίπλωση τους στον κώδικά σας.
 - Αντίθετα, το μέγεθος των κύριων περιβάλλοντα εργασίας τώρα μεθόδους η έκθεση που παρέχουν μεγαλύτερο έλεγχο σε επίπεδο HTTP εάν τον χρειάζεστε. Μπορείτε τώρα να περάσετε στις προσαρμοσμένες κεφαλίδες HTTP που θα συμπεριληφθούν στην αιτήσεις και το νέο `AzureOperationResponse<T>` επιστρεφόμενος τύπος παρέχει άμεση πρόσβαση για να το `HttpRequestMessage` και `HttpResponseMessage` για τη λειτουργία. `AzureOperationResponse`έχει οριστεί στο το `Microsoft.Rest.Azure` χώρο ονομάτων και αντικαθιστά `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>Αλλαγές ScoringParameters

Μια νέα κλάση με το όνομα `ScoringParameter` έχει προστεθεί στην πιο πρόσφατη SDK για να σας διευκολύνουν να παρέχουν παραμέτρους βαθμολογίας προφίλ σε ένα ερώτημα αναζήτησης. Προηγουμένως το `ScoringProfiles` ιδιότητα το `SearchParameters` κλάσης πληκτρολογήθηκε ως `IList<string>`; Μετά την πληκτρολόγηση ως `IList<ScoringParameter>`.

##### <a name="example"></a>Παράδειγμα

Εάν ο κώδικας είναι κάπως έτσι:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Μπορείτε να την αλλάξετε σε αυτήν για να διορθώσετε σφάλματα Δόμηση: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Μοντέλο κλάσης αλλαγές

Λόγω της υπογραφής αλλάζει περιγράφεται στη [λειτουργία μέθοδος αλλάζει](#OperationMethodChanges), πολλές κλάσεις στο το `Microsoft.Azure.Search.Models` χώρος ονομάτων έχουν μετονομαστεί ή καταργηθεί. Για παράδειγμα:

 - `IndexDefinitionResponse`έχει αντικατασταθεί από`AzureOperationResponse<Index>`
 - `DocumentSearchResponse`έχει μετονομαστεί σε`DocumentSearchResult`
 - `IndexResult`έχει μετονομαστεί σε`IndexingResult`
 - `Documents.Count()`Επιστρέφει τώρα ένα `long` με το πλήθος των εγγράφων αντί για ένα`DocumentCountResponse`
 - `IndexGetStatisticsResponse`έχει μετονομαστεί σε`IndexGetStatisticsResult`
 - `IndexListResponse`έχει μετονομαστεί σε`IndexListResult`

Για να συνοψίσετε, `OperationResponse`-προέρχεται κλάσεις που υπήρχαν μόνο για να αναδιπλώσετε έχουν καταργηθεί ένα μοντέλο αντικειμένου. Οι υπόλοιπες κλάσεις είχαν τους επίθημα άλλαξε από `Response` να `Result`.

##### <a name="example"></a>Παράδειγμα

Εάν ο κώδικας είναι κάπως έτσι:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Μπορείτε να την αλλάξετε σε αυτήν για να διορθώσετε σφάλματα Δόμηση:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Απόκριση κλάσεις και IEnumerable

Μια επιπλέον αλλαγή που ενδέχεται να επηρεάσουν τον κωδικό είναι ότι δεν είναι πλέον υλοποίηση κλάσεις απάντηση που κρατήστε πατημένο το πλήκτρο συλλογές `IEnumerable<T>`. Αντί για αυτό, μπορείτε να μεταβείτε απευθείας στην ιδιότητα συλλογής. Για παράδειγμα, εάν ο κώδικας είναι κάπως έτσι:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Μπορείτε να την αλλάξετε σε αυτήν για να διορθώσετε σφάλματα Δόμηση:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="important-note-for-web-applications"></a>Σημαντική σημείωση για τις εφαρμογές web

Εάν έχετε μια εφαρμογή web που τοποθετεί σειριακά `DocumentSearchResponse` άμεσα για να στείλετε τα αποτελέσματα αναζήτησης στο πρόγραμμα περιήγησης, θα χρειαστεί να αλλάξετε τον κωδικό ή τα αποτελέσματα δεν σειριοποιεί σωστά. Για παράδειγμα, εάν ο κώδικας είναι κάπως έτσι:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Μπορείτε να τον αλλάξετε αυξάνοντας τη `.Results` ιδιότητα της απόκρισης αναζήτησης για να διορθώσετε απόδοσης αποτέλεσμα αναζήτησης:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Θα πρέπει να πραγματοποιήσει αναζήτηση για αυτές τις περιπτώσεις στον κώδικά σας μόνοι σας. **Το πρόγραμμα μεταγλώττισης δεν θα σας ειδοποιήσει που** επειδή `JsonResult.Data` είναι τύπου `object`.

#### <a name="cloudexception-changes"></a>Αλλαγές CloudException

Το `CloudException` κλάσης έχει μετακινηθεί από το `Hyak.Common` χώρος ονομάτων για να το `Microsoft.Rest.Azure` χώρο ονομάτων. Επίσης, το `Error` ιδιότητα έχει μετονομαστεί σε `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>Αλλαγές SearchServiceClient και SearchIndexClient

Τον τύπο του `Credentials` ιδιότητα έχει αλλάξει από `SearchCredentials` τη βασική κλάση, `ServiceClientCredentials`. Εάν πρέπει να έχετε πρόσβαση το `SearchCredentials` από ένα `SearchIndexClient` ή `SearchServiceClient`, χρησιμοποιήστε το νέο `SearchCredentials` ιδιότητα.

Σε παλαιότερες εκδόσεις του SDK, `SearchServiceClient` και `SearchIndexClient` είχατε κατασκευές που χρειάστηκαν μια `HttpClient` παραμέτρου. Αυτές έχουν αντικατασταθεί με κατασκευές που λαμβάνουν ένα `HttpClientHandler` και ένας πίνακας με `DelegatingHandler` αντικείμενα. Αυτό διευκολύνει για να εγκαταστήσετε τα προγράμματα χειρισμού προσαρμοσμένων προ-επεξεργασία αιτήσεων HTTP, εάν είναι απαραίτητο.

Τέλος, το κατασκευές που χρειάστηκαν μια `Uri` και `SearchCredentials` έχουν αλλάξει. Για παράδειγμα, εάν έχετε κώδικα που έχει την παρακάτω μορφή:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Μπορείτε να την αλλάξετε σε αυτήν για να διορθώσετε σφάλματα Δόμηση:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Επίσης, σημειώστε ότι ο τύπος της παραμέτρου διαπιστευτήρια έχει αλλάξει για να `ServiceClientCredentials`. Αυτό δεν είναι πιθανό να επηρεάσουν τον κωδικό από `SearchCredentials` προέρχεται από `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Που περνά μέσα σε ένα Αναγνωριστικό αίτησης

Σε παλαιότερες εκδόσεις του SDK, μπορείτε να ορίσετε ένα Αναγνωριστικό αίτησης στην το `SearchServiceClient` ή `SearchIndexClient` και θέλετε να συμπεριληφθεί σε κάθε αίτηση για το REST API. Αυτό είναι χρήσιμο για την αντιμετώπιση προβλημάτων με την υπηρεσία αναζήτησης, εάν πρέπει να επικοινωνήσετε με την υποστήριξη. Ωστόσο, είναι πιο χρήσιμο για να ορίσετε ένα μοναδικό αίτηση Αναγνωριστικό για κάθε λειτουργία αντί να χρησιμοποιήσετε το ίδιο Αναγνωριστικό για όλες τις εργασίες. Για αυτόν το λόγο, η `SetClientRequestId` μεθόδους `SearchServiceClient` και `SearchIndexClient` έχει καταργηθεί. Αντί για αυτό, τότε μπορείτε να καταχωρήσετε ένα Αναγνωριστικό αίτησης σε κάθε μέθοδο λειτουργία μέσω την προαιρετική `SearchRequestOptions` παραμέτρου.

> [AZURE.NOTE] Σε μια μελλοντική έκδοση του SDK, θα προσθέσουμε ένα νέο μηχανισμό για τη ρύθμιση ενός Αναγνωριστικό αίτησης καθολικά του υπολογιστή-πελάτη αντικείμενα που είναι συνεπείς με η προσέγγιση που χρησιμοποιείται από άλλα SDK Azure.

#### <a name="example"></a>Παράδειγμα

Εάν έχετε κώδικα που έχει την παρακάτω μορφή:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Μπορείτε να την αλλάξετε σε αυτήν για να διορθώσετε σφάλματα Δόμηση:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Αλλαγές ονομάτων περιβάλλοντος εργασίας

Όλα τα ονόματα λειτουργία ομάδα περιβάλλοντος εργασίας έχει αλλάξει ώστε να είναι συνεπείς με τα αντίστοιχα ονόματα ιδιότητα:

 - Τον τύπο του `ISearchServiceClient.Indexes` έχει μετονομαστεί από `IIndexOperations` να `IIndexesOperations`.
 - Τον τύπο του `ISearchServiceClient.Indexers` έχει μετονομαστεί από `IIndexerOperations` να `IIndexersOperations`.
 - Τον τύπο του `ISearchServiceClient.DataSources` έχει μετονομαστεί από `IDataSourceOperations` να `IDataSourcesOperations`.
 - Τον τύπο του `ISearchIndexClient.Documents` έχει μετονομαστεί από `IDocumentOperations` να `IDocumentsOperations`.

Αυτή η αλλαγή δεν είναι πιθανό να επηρεάσουν τον κωδικό, εκτός εάν δημιουργηθεί mocks αυτών των διασυνδέσεων για σκοπούς δοκιμής.

<a name="BugFixesV1"></a>
### <a name="bug-fixes-in-version-11"></a>Διορθώσεις σφαλμάτων στην έκδοση 1.1

Παρουσιάστηκε ένα σφάλμα σε παλαιότερες εκδόσεις του SDK .NET Azure αναζήτησης που σχετίζεται με τη σειριοποίηση κλάσεων προσαρμοσμένο μοντέλο. Το σφάλμα μπορεί να προκύψει εάν έχετε δημιουργήσει μια προσαρμοσμένη μοντέλο κλάση με μια ιδιότητα ενός τύπου μη τύπο nullable τιμής.

#### <a name="steps-to-reproduce"></a>Βήματα για την αναπαραγωγή

Δημιουργία μιας κλάσης προσαρμοσμένο μοντέλο με μια ιδιότητα τύπου μη τύπο nullable τιμής. Για παράδειγμα, προσθέστε μια δημόσια `UnitCount` ιδιότητα τύπου `int` αντί για `int?`.

Εάν δημιουργήσετε ευρετήριο σε ένα έγγραφο με την προεπιλεγμένη τιμή αυτού του τύπου (για παράδειγμα, 0 για `int`), το πεδίο θα είναι null στο πλαίσιο Αναζήτηση Azure. Εάν πραγματοποιείτε αναζήτηση αργότερα για αυτό το έγγραφο, το `Search` κλήση θα εμφανίσουν `JsonSerializationException` παραπονούμενοι που δεν δεν μπορεί να μετατρέψει `null` να `int`.

Επίσης, τα φίλτρα ενδέχεται να μην λειτουργούν με τον αναμενόμενο τρόπο εφόσον έχει συνταχθεί null στο ευρετήριο αντί για το οποίο προορίζεται τιμή.

#### <a name="fix-details"></a>Διόρθωση λεπτομέρειες

Θα σας έχουν διορθωθεί αυτό το πρόβλημα στην έκδοση 1.1 του SDK. Τώρα, εάν έχετε μια κλάση μοντέλου ως εξής:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

και ορίσετε `IntValue` σε 0, αυτήν την τιμή είναι τώρα σωστά σειριοποιηθεί ως 0 σε σύρματος και έχουν αποθηκευτεί ως 0 στο ευρετήριο. Αμφίδρομη επίσης λειτουργεί όπως αναμένεται.

Υπάρχει ένα πιθανό ζήτημα πρέπει να γνωρίζετε με αυτήν την προσέγγιση: Εάν χρησιμοποιείτε έναν τύπο μοντέλο με μια ιδιότητα μη τύπο nullable, πρέπει να **εξασφαλίσετε** ότι δεν υπάρχουν έγγραφα του ευρετηρίου περιέχουν τιμή null για το αντίστοιχο πεδίο. Το SDK ούτε το Azure Search REST API θα σας βοηθήσει να επιβάλλετε αυτή.

Αυτό δεν είναι απλώς ένα πρόβλημα υποθετική: φανταστείτε ένα σενάριο όπου μπορείτε να προσθέσετε ένα νέο πεδίο σε ένα υπάρχον ευρετήριο που είναι του τύπου `Edm.Int32`. Αφού ενημερώσετε τον ορισμό ευρετηρίου, όλα τα έγγραφα που θα έχει μια τιμή null για αυτό το νέο πεδίο (εφόσον όλοι οι τύποι έχουν τύπο nullable στο πλαίσιο Αναζήτηση Azure). Εάν χρησιμοποιείτε, στη συνέχεια, μια κλάση μοντέλου με μια μη-τύπο nullable `int` ιδιότητα για αυτό το πεδίο, θα λάβετε μια `JsonSerializationException` ως εξής, όταν προσπαθείτε να ανακτήσετε έγγραφα:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Για αυτόν το λόγο, εξακολουθεί να συνιστάται να χρησιμοποιείτε τύπο nullable τύπους σε τις σχολικές τάξεις μοντέλο ως βέλτιστη πρακτική.

Για περισσότερες λεπτομέρειες σχετικά με αυτό το σφάλμα και την ενημέρωση κώδικα, ανατρέξτε στο θέμα [αυτού του ζητήματος σε GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).
