<properties
    pageTitle="Δημιουργία ευρετηρίου αναζήτησης Azure χρησιμοποιώντας το .NET SDK | Microsoft Azure | Υπηρεσία αναζήτησης φιλοξενούμενη cloud"
    description="Δημιουργήστε ένα ευρετήριο σε κώδικα, χρησιμοποιώντας το .NET SDK Azure αναζήτησης."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Δημιουργία ευρετηρίου αναζήτησης Azure χρησιμοποιώντας το .NET SDK
> [AZURE.SELECTOR]
- [Επισκόπηση](search-what-is-an-index.md)
- [Πύλη](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [ΥΠΌΛΟΙΠΟ](search-create-index-rest-api.md)


Σε αυτό το άρθρο θα σας καθοδηγήσει στη διαδικασία δημιουργίας Azure αναζήτηση [ευρετηρίου](https://msdn.microsoft.com/library/azure/dn798941.aspx) χρησιμοποιώντας το [Azure αναζήτησης .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx).

Πριν από την εκτέλεση αυτού του οδηγού και τη δημιουργία ευρετηρίου, θα πρέπει να έχετε ήδη [δημιουργήσει μια υπηρεσία αναζήτησης Azure](search-create-service-portal.md).

Σημειώστε ότι όλα τα δείγματα κώδικα σε αυτό το άρθρο είναι γραμμένο σε C#. Μπορείτε να βρείτε την πλήρη προέλευση κωδικό [στο GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Να. Προσδιορισμός κλειδιού api διαχείρισης της υπηρεσίας αναζήτησης Azure σας
Τώρα που έχουν παρασχεθεί μια υπηρεσία αναζήτησης Azure, είστε σχεδόν έτοιμοι για την έκδοση προσκλήσεις σε σχέση με το τελικό σημείο υπηρεσίας σας χρησιμοποιώντας το .NET SDK. Πρώτα, θα πρέπει να αποκτήσετε ένα από τα διαχείρισης api-κλειδιά που δημιουργήθηκε για την υπηρεσία αναζήτησης που παροχή της υπηρεσίας. Το .NET SDK θα στείλει αυτό το api-κλειδί σε κάθε αίτηση στην υπηρεσία σας. Έχετε έναν έγκυρο αριθμό-κλειδί εμπιστευτεί, με βάση ανά αίτηση, μεταξύ της εφαρμογής που στέλνει την αίτηση και την υπηρεσία που χειρίζεται αυτό.

1. Για να βρείτε την υπηρεσία api-κλειδιά πρέπει να συνδεθείτε στην [Πύλη του Azure](https://portal.azure.com/)
2. Μεταβείτε στην υπηρεσία αναζήτησης Azure blade
3. Κάντε κλικ στο εικονίδιο "Πλήκτρα"

Υπηρεσία σας θα έχει *διαχείρισης* και τα *κλειδιά ερωτήματος*.

  - Σας κύριας και δευτερεύουσας *διαχείρισης κλειδιών* εκχώρηση πλήρη δικαιώματα για όλες τις λειτουργίες, συμπεριλαμβανομένης της δυνατότητας για να διαχειριστείτε την υπηρεσία, να δημιουργήσετε και να διαγράψετε τα ευρετήρια, οι δεικτοδότες και προελεύσεις δεδομένων. Υπάρχουν δύο κλειδιά, ώστε να μπορείτε να συνεχίσετε να χρησιμοποιείτε το δευτερεύον κλειδί Εάν αποφασίσετε να δημιουργήσετε ξανά το πρωτεύον κλειδί και το αντίστροφο.
  - Το *ερώτημα πλήκτρα* εκχωρήσετε πρόσβαση μόνο για ανάγνωση σε ευρετήρια και έγγραφα και διανέμονται συνήθως σε εφαρμογές προγράμματος-πελάτη που θεμάτων αιτήσεις αναζήτησης.

Για τους σκοπούς της δημιουργίας ευρετηρίου, μπορείτε να χρησιμοποιήσετε τον αριθμό-κλειδί πρωτεύον ή δευτερεύον διαχείρισης.

<a name="CreateSearchServiceClient"></a>
## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. Δημιουργήστε μια παρουσία της κλάσης SearchServiceClient
Για να ξεκινήσετε να χρησιμοποιείτε το Azure αναζήτησης .NET SDK, θα πρέπει να δημιουργήσετε μια παρουσία του `SearchServiceClient` τάξης. Αυτή η κλάση έχει πολλές κατασκευές. Αυτό που θέλετε να λαμβάνει το όνομα της υπηρεσίας αναζήτησης και μια `SearchCredentials` αντικείμενο ως παράμετροι. `SearchCredentials`να αναδιπλώνεται api-κλειδιού.

Ο παρακάτω κώδικας δημιουργεί μια νέα `SearchServiceClient` χρησιμοποιώντας τιμές για το όνομα της υπηρεσίας αναζήτησης και το api-κλειδί που είναι αποθηκευμένα στο αρχείο παραμέτρων της εφαρμογής (`app.config` ή `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`περιλαμβάνει μια `Indexes` την ιδιότητα. Αυτή η ιδιότητα παρέχει όλες τις μεθόδους που χρειάζεστε για να δημιουργήσετε, λίστα, να ενημερώσετε ή να διαγράψετε τα ευρετήρια Azure αναζήτησης.

> [AZURE.NOTE] Το `SearchServiceClient` κλάση διαχειρίζεται συνδέσεις με την υπηρεσία αναζήτησης. Για να αποφύγετε Άνοιγμα πάρα πολλές συνδέσεις, θα πρέπει να προσπαθήσετε να μοιραστείτε μια μοναδική παρουσία `SearchServiceClient` στην εφαρμογή σας εάν είναι δυνατό. Τις μεθόδους είναι ασφαλείς νήματος για να ενεργοποιήσετε την όπως κοινή χρήση.

<a name="DefineIndex"></a>
## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. Ορίσετε το ευρετήριο αναζήτησης Azure χρησιμοποιώντας το `Index` τάξης
Μια μεμονωμένη κλήση για να το `Indexes.Create` μέθοδο θα δημιουργήσει το ευρετήριο. Αυτή η μέθοδος λαμβάνει ως παράμετρο μια `Index` αντικείμενο το οποίο ορίζει το ευρετήριο αναζήτησης Azure. Πρέπει να δημιουργήσετε ένα `Index` αντικειμένου και να προετοιμάσετε ως εξής:

1. Ορίστε το `Name` ιδιότητα το `Index` αντικειμένου στο όνομα του ευρετηρίου σας.
2. Ορίστε το `Fields` ιδιότητα το `Index` αντικειμένου σε έναν πίνακα `Field` αντικείμενα. Κάθε ένα από τα `Field` αντικείμενα ορίζει τη συμπεριφορά ενός πεδίου του ευρετηρίου. Μπορείτε να παρέχετε το όνομα του πεδίου στην κατασκευή, μαζί με τον τύπο δεδομένων (ή "Ανάλυση" για τα πεδία συμβολοσειρά). Μπορείτε επίσης να ορίσετε άλλες ιδιότητες όπως `IsSearchable`, `IsFilterable`κ.λπ.

Είναι σημαντικό να διατηρείτε αναζήτηση χρήστη εμπειρία και επιχειρηματικές ανάγκες σας υπόψη κατά τη σχεδίαση του ευρετηρίου ως κάθε `Field` πρέπει να έχει εκχωρηθεί τις [κατάλληλες ιδιότητες](https://msdn.microsoft.com/library/azure/dn798941.aspx). Εφαρμογή αυτών των ιδιοτήτων ελέγχετε τα αναζήτηση δυνατότητες (φιλτράρισμα, faceting, ταξινόμηση αναζήτησης πλήρους κειμένου, κ.λπ.) σε ποια πεδία. Για οποιαδήποτε ιδιότητα δεν ορίσετε ρητά, το `Field` κλάση προεπιλογών για απενεργοποίηση η αντίστοιχη δυνατότητα αναζήτησης, εκτός εάν ενεργοποιήσετε ειδικά.

Για παράδειγμά μας, θα σας έχετε με το όνομα ευρετήριό μας "ξενοδοχεία" και να ορίσει τα πεδία ως εξής:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

Θα σας έχετε επιλέξει προσεκτικά τις τιμές ιδιοτήτων για κάθε `Field` με βάση τον τρόπο θα σας πιστεύετε ότι θα χρησιμοποιηθεί σε μια εφαρμογή του. Για παράδειγμα, είναι πιθανό ότι θα σας ενδιαφέρουν συμφωνίες λέξεων-κλειδιών σε άτομα που αναζητούν ξενοδοχεία το `description` πεδίων, ώστε να μας Ενεργοποίηση αναζήτησης πλήρους κειμένου για αυτό το πεδίο με τη ρύθμιση `IsSearchable` να `true`.

Σημείωση που ακριβώς ένα πεδίο του ευρετηρίου του τύπου `DataType.String` πρέπει να καθοριστεί ως πεδίο _αριθμού-κλειδιού_ , ορίζοντας `IsKey` να `true` (ανατρέξτε στο θέμα `hotelId` στο παραπάνω παράδειγμα).

Ο ορισμός ευρετηρίου παραπάνω χρησιμοποιεί έναν αναλυτή προσαρμοσμένης γλώσσας για τα `description_fr` πεδίου επειδή πρόκειται να αποθηκεύσετε το κείμενο στα γαλλικά. Δείτε [τη γλώσσα που υποστηρίζει το θέμα στο MSDN](https://msdn.microsoft.com/library/azure/dn879793.aspx) , καθώς και το αντίστοιχο [ιστολογίου](https://azure.microsoft.com/blog/language-support-in-azure-search/) για περισσότερες πληροφορίες σχετικά με τη γλώσσα αναλυτές.

> [AZURE.NOTE]  Λάβετε υπόψη ότι, μεταφέροντας `AnalyzerName.FrLucene` στην κατασκευή, το `Field` αυτόματα θα είναι του τύπου `DataType.String` και θα έχουν `IsSearchable` οριστεί σε `true`.

## <a name="iv-create-the-index"></a>IV. Δημιουργήστε το ευρετήριο
Τώρα που έχετε μια προετοιμασία `Index` αντικείμενο, μπορείτε να δημιουργήσετε το ευρετήριο απλώς καλώντας `Indexes.Create` σε σας `SearchServiceClient` αντικειμένου:

```csharp
serviceClient.Indexes.Create(definition);
```

Για μια αίτηση επιτυχής, η μέθοδος θα επιστρέψει κανονικά. Εάν υπάρχει πρόβλημα με την πρόσκληση, όπως μια μη έγκυρη παράμετρος, η μέθοδος θα εμφανίσουν `CloudException`.

Όταν ολοκληρώσετε την εργασία με ένα ευρετήριο και θέλετε να τη διαγράψετε, κλήση μόνο το `Indexes.Delete` μέθοδος σε σας `SearchServiceClient`. Για παράδειγμα, αυτό είναι πώς θα σας να διαγράψετε το ευρετήριο "ξενοδοχεία":

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [AZURE.NOTE] Το παράδειγμα κώδικα σε αυτό το άρθρο χρησιμοποιεί τις μεθόδους σύγχρονη του SDK Azure αναζήτησης .NET για λόγους ευκολίας. Συνιστάται να χρησιμοποιείτε τις μεθόδους ασύγχρονης στις εφαρμογές δική σας για να διατηρήσετε τους μεταβλητού μεγέθους και αποκρίνεται. Για παράδειγμα, στα παραπάνω παραδείγματα που μπορούν να χρησιμοποιήσουν `CreateAsync` και `DeleteAsync` αντί για `Create` και `Delete`.

## <a name="next"></a>Επόμενο
Αφού δημιουργήσετε ένα ευρετήριο αναζήτησης Azure, θα είστε έτοιμοι να [στείλετε το περιεχόμενο στο ευρετήριο](search-what-is-data-import.md) , ώστε να μπορείτε να ξεκινήσετε την αναζήτηση των δεδομένων σας.
