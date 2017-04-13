<properties
    pageTitle="Υποβολή ερωτήματος του ευρετηρίου αναζήτησης Azure χρησιμοποιώντας το REST API | Microsoft Azure | Υπηρεσία αναζήτησης φιλοξενούμενη cloud"
    description="Δημιουργήστε ένα ερώτημα αναζήτησης στο πλαίσιο Αναζήτηση Azure και χρησιμοποιήστε τις παραμέτρους αναζήτησης σε φίλτρα και ταξινόμηση αποτελεσμάτων αναζήτησης."
    services="search"
    documentationCenter=""
    manager="jhubbard"
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Υποβολή ερωτήματος του ευρετηρίου αναζήτησης Azure χρησιμοποιώντας το REST API
> [AZURE.SELECTOR]
- [Επισκόπηση](search-query-overview.md)
- [Πύλη](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [ΥΠΌΛΟΙΠΟ](search-query-rest-api.md)

Σε αυτό το άρθρο θα σας δείξει πώς να απευθύνετε το ερώτημα ευρετηρίου με χρήση του [Azure Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx).

Πριν να ξεκινήσετε αυτόν τον οδηγό, θα πρέπει να έχετε ήδη [δημιουργήσει ένα ευρετήριο αναζήτησης Azure](search-what-is-an-index.md) και [συμπληρωμένο με δεδομένα](search-what-is-data-import.md).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Να. Προσδιορισμός της υπηρεσίας αναζήτησης Azure σας ερώτημα api-κλειδί
Βασικό στοιχείο κάθε λειτουργίας αναζήτησης σε σχέση με το Azure Search REST API είναι το *api-κλειδί* που δημιουργήθηκε για την υπηρεσία που παροχή της υπηρεσίας. Έχετε έναν έγκυρο αριθμό-κλειδί εμπιστευτεί, με βάση ανά αίτηση, μεταξύ της εφαρμογής που στέλνει την αίτηση και την υπηρεσία που χειρίζεται αυτό.

1. Για να βρείτε την υπηρεσία api-κλειδιά πρέπει να συνδεθείτε στην [Πύλη του Azure](https://portal.azure.com/)
2. Μεταβείτε στην υπηρεσία αναζήτησης Azure blade
3. Κάντε κλικ στο εικονίδιο "Πλήκτρα"

Υπηρεσία σας θα έχει *διαχείρισης* και τα *κλειδιά ερωτήματος*.

 - Σας κύριας και δευτερεύουσας *διαχείρισης κλειδιών* εκχώρηση πλήρη δικαιώματα για όλες τις λειτουργίες, συμπεριλαμβανομένης της δυνατότητας για να διαχειριστείτε την υπηρεσία, να δημιουργήσετε και να διαγράψετε τα ευρετήρια, οι δεικτοδότες και προελεύσεις δεδομένων. Υπάρχουν δύο κλειδιά, ώστε να μπορείτε να συνεχίσετε να χρησιμοποιείτε το δευτερεύον κλειδί Εάν αποφασίσετε να δημιουργήσετε ξανά το πρωτεύον κλειδί και το αντίστροφο.
 - Το *ερώτημα πλήκτρα* εκχωρήσετε πρόσβαση μόνο για ανάγνωση σε ευρετήρια και έγγραφα και διανέμονται συνήθως σε εφαρμογές προγράμματος-πελάτη που θεμάτων αιτήσεις αναζήτησης.

Για τους σκοπούς της υποβολή ερωτημάτων ένα ευρετήριο, μπορείτε να χρησιμοποιήσετε ένα από τα πλήκτρα ερωτήματος. Το πλήκτρα διαχείρισης μπορεί επίσης να χρησιμοποιηθεί για ερωτήματα, αλλά θα πρέπει να μπορείτε να χρησιμοποιήσετε έναν αριθμό-κλειδί ερωτήματος στον κώδικα της εφαρμογής σας, όπως αυτή η [αρχή των ελάχιστων δικαιωμάτων](https://en.wikipedia.org/wiki/Principle_of_least_privilege)που ακολουθεί καλύτερα.

## <a name="ii-formulate-your-query"></a>II. Διατύπωση το ερώτημά σας
Υπάρχουν δύο τρόποι για να [αναζητήσετε το ευρετήριό σας χρησιμοποιώντας το REST API](https://msdn.microsoft.com/library/azure/dn798927.aspx). Ένας τρόπος είναι να εκδώσετε μια αίτηση HTTP POST όπου θα οριστεί σας παράμετροι ερωτήματος σε ένα αντικείμενο JSON στο σώμα της αίτησης. Ο άλλος τρόπος είναι να εκδώσετε μια αίτηση HTTP GET όπου θα οριστεί σας παράμετροι ερωτήματος μέσα στο URL της αίτησης. Σημειώστε ότι ΚΑΤΑΧΏΡΗΣΗ έχει περισσότερες [υπάρξουν όρια](https://msdn.microsoft.com/library/azure/dn798927.aspx) στο μέγεθος της παράμετροι ερωτήματος από ΛΉΨΗ. Για αυτόν το λόγο, συνιστάται να χρησιμοποιείτε ΔΗΜΟΣΊΕΥΣΗ, εκτός αν έχετε ειδικές περιπτώσεις όπου ΛΉΨΗ θα είναι πιο εύκολη.

Τόσο για ΔΗΜΟΣΊΕΥΣΗ και ΛΉΨΗ, πρέπει να παρέχετε το *όνομα της υπηρεσίας*, *ευρετηρίου*και τη σωστή *έκδοση API* (η τρέχουσα έκδοση API είναι `2015-02-28` τη στιγμή της δημοσίευσης αυτού του εγγράφου) στην αίτηση URL. Για ΛΉΨΗ, η *συμβολοσειρά ερωτήματος* στο τέλος της διεύθυνσης URL θα όπου μπορείτε να παρέχετε τις παραμέτρους ερωτήματος. Δείτε παρακάτω για τη μορφή διεύθυνσης URL:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

Η μορφή για τη ΔΗΜΟΣΊΕΥΣΉ είναι το ίδιο, αλλά με api έκδοση μόνο στις παραμέτρους συμβολοσειράς ερωτήματος.



#### <a name="example-queries"></a>Παράδειγμα ερωτήματα

Ακολουθούν μερικά ερωτήματα παράδειγμα σε ένα ευρετήριο με το όνομα "ξενοδοχεία". Αυτά τα ερωτήματα εμφανίζονται στη μορφή GET και ΔΗΜΟΣΊΕΥΣΗ.

Αναζητήστε τον όρο 'προϋπολογισμού' ολόκληρο το ευρετήριο και να επιστρέψετε μόνο το `hotelName` πεδίο:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Εφαρμογή φίλτρου σε ευρετήριο για να βρείτε κοστίζει από 150 $ ανά νυχτερινής ξενοδοχεία και επιστρέφει το `hotelId` και `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Αναζήτηση ολόκληρο το ευρετήριο, σειρά από ένα συγκεκριμένο πεδίο (`lastRenovationDate`) σε φθίνουσα σειρά, λαμβάνουν τα αποτελέσματα πρώτες δύο και εμφάνιση μόνο `hotelName` και `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. Υποβολή της αίτησης HTTP
Τώρα που έχετε έχουν τυποποιημένη το ερώτημά σας ως τμήμα της διεύθυνσης URL αίτηση HTTP (για ΛΉΨΗ) ή σώμα (για ΔΗΜΟΣΊΕΥΣΗ), μπορείτε να ορίσετε τις κεφαλίδες αίτησης και να υποβάλετε το ερώτημά σας.

#### <a name="request-and-request-headers"></a>Αίτηση και κεφαλίδες αίτησης
Πρέπει να καθορίσετε δύο κεφαλίδες αίτησης για ΛΉΨΗ ή τριών για ΔΗΜΟΣΊΕΥΣΗ:
1. Το `api-key` κεφαλίδας θα πρέπει να οριστεί το κλειδί ερωτήματος που βρίσκεται στο βήμα να παραπάνω. Σημειώστε ότι μπορείτε επίσης να χρησιμοποιήσετε ένα κλειδί διαχείρισης ως το `api-key` κεφαλίδας, αλλά συνιστάται να χρησιμοποιήσετε έναν αριθμό-κλειδί ερωτήματος κατά την αποκλειστική χρήση εκχωρεί πρόσβαση μόνο για ανάγνωση για τα ευρετήρια και έγγραφα.
2. Το `Accept` κεφαλίδας πρέπει να οριστεί σε `application/json`.
3. Για ΔΗΜΟΣΊΕΥΣΗ μόνο, το `Content-Type` κεφαλίδας πρέπει να ρυθμιστεί για να `application/json`.

Δείτε παρακάτω για μια αίτηση HTTP GET για να πραγματοποιήσετε αναζήτηση στο ευρετήριο "ξενοδοχεία" με χρήση του Azure Search REST API, με χρήση ενός απλού ερωτήματος που πραγματοποιεί αναζήτηση για τον όρο "motel":

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Εδώ είναι το ίδιο ερώτημα παράδειγμα, χρησιμοποιώντας HTTP POST αυτή τη φορά:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Επιτυχής ερώτημα θα έχει ως αποτέλεσμα έναν κωδικό κατάστασης του `200 OK` και τα αποτελέσματα της αναζήτησης επιστρέφονται ως JSON στο σώμα απόκρισης. Παρακάτω θα δείτε τι τα αποτελέσματα για τους παραπάνω ερώτημα όπως, υπό την προϋπόθεση ότι το ευρετήριο "ξενοδοχεία" συμπληρώνεται με το δείγμα δεδομένων σε [Εισαγωγή δεδομένων στο πλαίσιο Αναζήτηση Azure χρησιμοποιώντας το REST API](search-import-data-rest-api.md) (Σημειώστε ότι έχει διαμορφωθεί η JSON για σαφήνεια).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Για να μάθετε περισσότερα, επισκεφθείτε την ενότητα "Απάντηση" [Αναζήτηση εγγράφων](https://msdn.microsoft.com/library/azure/dn798927.aspx). Για περισσότερες πληροφορίες σχετικά με άλλους κωδικούς κατάστασης HTTP που μπορεί να επιστραφεί σε περίπτωση αποτυχίας, ανατρέξτε στο θέμα [κωδικοί κατάστασης HTTP (Azure αναζήτηση)](https://msdn.microsoft.com/library/azure/dn798925.aspx).
