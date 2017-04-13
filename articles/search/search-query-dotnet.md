<properties
    pageTitle="Υποβολή ερωτήματος του ευρετηρίου αναζήτησης Azure χρησιμοποιώντας το .NET SDK | Microsoft Azure | Υπηρεσία αναζήτησης φιλοξενούμενη cloud"
    description="Δημιουργήστε ένα ερώτημα αναζήτησης στο πλαίσιο Αναζήτηση Azure και χρησιμοποιήστε τις παραμέτρους αναζήτησης σε φίλτρα και ταξινόμηση αποτελεσμάτων αναζήτησης."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="query-your-azure-search-index-using-the-net-sdk"></a>Υποβολή ερωτήματος του ευρετηρίου αναζήτησης Azure χρησιμοποιώντας το .NET SDK
> [AZURE.SELECTOR]
- [Επισκόπηση](search-query-overview.md)
- [Πύλη](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [ΥΠΌΛΟΙΠΟ](search-query-rest-api.md)

Σε αυτό το άρθρο θα σας δείξει πώς να απευθύνετε το ερώτημα ευρετηρίου χρησιμοποιώντας το [Azure αναζήτησης .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx).

Πριν να ξεκινήσετε αυτόν τον οδηγό, θα πρέπει να έχετε ήδη [δημιουργήσει ένα ευρετήριο αναζήτησης Azure](search-what-is-an-index.md) και [συμπληρωμένο με δεδομένα](search-what-is-data-import.md).

Σημειώστε ότι όλα τα δείγματα κώδικα σε αυτό το άρθρο είναι γραμμένο σε C#. Μπορείτε να βρείτε την πλήρη προέλευση κωδικό [στο GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Να. Προσδιορισμός της υπηρεσίας αναζήτησης Azure σας ερώτημα api-κλειδί
Τώρα που έχετε δημιουργήσει ένα ευρετήριο αναζήτησης Azure, είστε σχεδόν έτοιμοι για την έκδοση ερωτημάτων με το .NET SDK. Πρώτα, θα πρέπει να αποκτήσετε ένα από τα ερωτήματος api-κλειδιά που δημιουργήθηκε για την υπηρεσία αναζήτησης που παροχή της υπηρεσίας. Το .NET SDK θα στείλει αυτό το api-κλειδί σε κάθε αίτηση στην υπηρεσία σας. Έχετε έναν έγκυρο αριθμό-κλειδί εμπιστευτεί, με βάση ανά αίτηση, μεταξύ της εφαρμογής που στέλνει την αίτηση και την υπηρεσία που χειρίζεται αυτό.

1. Για να βρείτε την υπηρεσία api-κλειδιά πρέπει να συνδεθείτε στην [Πύλη του Azure](https://portal.azure.com/)
2. Μεταβείτε στην υπηρεσία αναζήτησης Azure blade
3. Κάντε κλικ στο εικονίδιο "Πλήκτρα"

Υπηρεσία σας θα έχει *διαχείρισης* και τα *κλειδιά ερωτήματος*.

  - Σας κύριας και δευτερεύουσας *διαχείρισης κλειδιών* εκχώρηση πλήρη δικαιώματα για όλες τις λειτουργίες, συμπεριλαμβανομένης της δυνατότητας για να διαχειριστείτε την υπηρεσία, να δημιουργήσετε και να διαγράψετε τα ευρετήρια, οι δεικτοδότες και προελεύσεις δεδομένων. Υπάρχουν δύο κλειδιά, ώστε να μπορείτε να συνεχίσετε να χρησιμοποιείτε το δευτερεύον κλειδί Εάν αποφασίσετε να δημιουργήσετε ξανά το πρωτεύον κλειδί και το αντίστροφο.
  - Το *ερώτημα πλήκτρα* εκχωρήσετε πρόσβαση μόνο για ανάγνωση σε ευρετήρια και έγγραφα και διανέμονται συνήθως σε εφαρμογές προγράμματος-πελάτη που θεμάτων αιτήσεις αναζήτησης.

Για τους σκοπούς της υποβολή ερωτημάτων ένα ευρετήριο, μπορείτε να χρησιμοποιήσετε ένα από τα πλήκτρα ερωτήματος. Το πλήκτρα διαχείρισης μπορεί επίσης να χρησιμοποιηθεί για ερωτήματα, αλλά θα πρέπει να μπορείτε να χρησιμοποιήσετε έναν αριθμό-κλειδί ερωτήματος στον κώδικα της εφαρμογής σας, όπως αυτή η [αρχή των ελάχιστων δικαιωμάτων](https://en.wikipedia.org/wiki/Principle_of_least_privilege)που ακολουθεί καλύτερα.

## <a name="ii-create-an-instance-of-the-searchindexclient-class"></a>II. Δημιουργήστε μια παρουσία της κλάσης SearchIndexClient
Για την έκδοση ερωτημάτων με το Azure αναζήτησης .NET SDK, θα πρέπει να δημιουργήσετε μια παρουσία του το `SearchIndexClient` τάξης. Αυτή η κλάση έχει πολλές κατασκευές. Αυτό που θέλετε μεταφέρει το όνομα της υπηρεσίας αναζήτησης, όνομα index, και μια `SearchCredentials` αντικείμενο ως παράμετροι. `SearchCredentials`να αναδιπλώνεται api-κλειδιού.

Ο παρακάτω κώδικας δημιουργεί μια νέα `SearchIndexClient` για το ευρετήριο "ξενοδοχεία" (που δημιουργήθηκε στο θέμα [Δημιουργία ευρετηρίου αναζήτησης Azure χρησιμοποιώντας το .NET SDK](search-create-index-dotnet.md)) χρησιμοποιώντας τιμές για το όνομα της υπηρεσίας αναζήτησης και το api-κλειδί που είναι αποθηκευμένα στο αρχείο παραμέτρων της εφαρμογής (`app.config` ή `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient`περιλαμβάνει μια `Documents` την ιδιότητα. Αυτή η ιδιότητα παρέχει όλες τις μεθόδους που πρέπει να ερωτήματος αναζήτησης Azure ευρετήρια.

## <a name="iii-query-your-index"></a>III. Ερώτημα το ευρετήριό σας
Η αναζήτηση με το .NET SDK είναι τόσο απλή όσο η κλήση του `Documents.Search` μέθοδος σε σας `SearchIndexClient`. Αυτή η μέθοδος λαμβάνει μερικά παραμέτρους, όπως το κείμενο αναζήτησης, μαζί με μια `SearchParameters` αντικείμενο το οποίο μπορεί να χρησιμοποιηθεί για να βελτιώσετε περαιτέρω το ερώτημα.

#### <a name="types-of-queries"></a>Τύποι ερωτημάτων
Οι δύο βασικοί [τύποι ερωτήματος](search-query-overview.md#types-of-queries) που θα χρησιμοποιήσετε είναι `search` και `filter`. A `search` ερωτήματος αναζητήσεις για έναν ή περισσότερους όρους σε όλα τα πεδία _με δυνατότητα αναζήτησης_ του ευρετηρίου. A `filter` ερωτήματος αξιολογεί μια δυαδική παράσταση σε όλους _με δυνατότητα φιλτραρίσματος_ πεδία σε ένα ευρετήριο.

Αναζητήσεις και φίλτρα είναι που εκτελούνται με τη `Documents.Search` μέθοδο. Ένα ερώτημα αναζήτησης μπορούν να καταχωρηθούν σε το `searchText` παράμετρο, ενώ μια παράσταση φίλτρου μπορούν να καταχωρηθούν σε το `Filter` ιδιότητα το `SearchParameters` τάξης. Για να φιλτράρετε χωρίς αναζήτηση, απλώς μεταβιβάζουν `"*"` για το `searchText` παραμέτρου. Για να πραγματοποιήσετε αναζήτηση χωρίς φιλτράρισμα, απλώς αφήστε το `Filter` ιδιότητα ακύρωση, ή να μεταβιβάσει μια `SearchParameters` παρουσίας σε όλα.

#### <a name="example-queries"></a>Παράδειγμα ερωτήματα

Το παρακάτω δείγμα κώδικα εμφανίζει μερικά διαφορετικούς τρόπους για να υποβάλετε ερώτημα στο ευρετήριο "ξενοδοχεία" ορίζεται στο θέμα [Δημιουργία ευρετηρίου αναζήτησης Azure χρησιμοποιώντας το .NET SDK](search-create-index-dotnet.md#DefineIndex). Σημειώστε ότι τα έγγραφα που επιστρέφονται με τα αποτελέσματα αναζήτησης είναι παρουσίες του `Hotel` κλάση, η οποία έχει οριστεί στην [Εισαγωγή δεδομένων στο πλαίσιο Αναζήτηση Azure χρησιμοποιώντας το .NET SDK](search-import-data-dotnet.md#HotelClass). Το δείγμα κώδικα κάνει χρήση ενός `WriteDocuments` μέθοδο για να εξαγάγετε τα αποτελέσματα αναζήτησης στην κονσόλα. Αυτή η μέθοδος περιγράφεται στην επόμενη ενότητα.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="iv-handle-search-results"></a>IV. Λαβή αποτελεσμάτων αναζήτησης
Το `Documents.Search` μέθοδος επιστρέφει μια `DocumentSearchResult` αντικείμενο το οποίο περιέχει τα αποτελέσματα του ερωτήματος. Το παράδειγμα στην προηγούμενη ενότητα χρησιμοποιείται μια μέθοδο που ονομάζεται `WriteDocuments` για να εξαγάγετε τα αποτελέσματα αναζήτησης στην κονσόλα:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Δείτε πώς είναι τα αποτελέσματα για τα ερωτήματα στην προηγούμενη ενότητα, υπό την προϋπόθεση ότι το ευρετήριο "ξενοδοχεία" συμπληρώνεται με το δείγμα δεδομένων στην [Εισαγωγή δεδομένων στο πλαίσιο Αναζήτηση Azure χρησιμοποιώντας το .NET SDK](search-import-data-dotnet.md):

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

```

Το παραπάνω δείγμα κώδικα χρησιμοποιεί την κονσόλα για την εξαγωγή αποτελεσμάτων αναζήτησης. Παρομοίως θα πρέπει να την εμφάνιση των αποτελεσμάτων αναζήτησης στη δική σας εφαρμογή. Δείτε ένα παράδειγμα του τρόπου για την απόδοση αποτελεσμάτων αναζήτησης σε μια εφαρμογή web ASP.NET MVC βάσει [αυτού του δείγματος στην GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) .
