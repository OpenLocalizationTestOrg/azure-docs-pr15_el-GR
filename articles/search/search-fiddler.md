<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε Fiddler να αξιολογήσετε και να ελέγξετε Azure Search REST APIs | Microsoft Azure | Υπηρεσία αναζήτησης φιλοξενούμενη cloud"
    description="Χρήση Fiddler για μια προσέγγιση χωρίς κώδικα για την επαλήθευση διαθεσιμότητας Azure αναζήτησης και δοκιμάζοντας τα REST API."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Χρήση Fiddler να αξιολογήσετε και να ελέγξετε APIs ΥΠΌΛΟΙΠΑ αναζήτησης Azure
> [AZURE.SELECTOR]
- [Επισκόπηση](search-query-overview.md)
- [Εξερεύνηση αναζήτησης](search-explorer.md)
- [Fiddler](search-fiddler.md)
- [.NET](search-query-dotnet.md)
- [ΥΠΌΛΟΙΠΟ](search-query-rest-api.md)

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να χρησιμοποιήσετε Fiddler, διαθέσιμο ως μια [δωρεάν λήψη από Telerik](http://www.telerik.com/fiddler), για να τις αιτήσεις HTTP το ζήτημα και Προβολή απαντήσεων χρησιμοποιώντας το Azure Search REST API, χωρίς να χρειάζεται να γράφετε κώδικα. Azure αναζήτησης πλήρως-γίνεται, φιλοξενούνται υπηρεσία αναζήτησης στο cloud στο Microsoft Azure, εύκολα προγραμματιζόμενο μέσω .NET και REST API του Yammer. Η υπηρεσία αναζήτησης Azure APIs ΥΠΌΛΟΙΠΑ τεκμηριώνονται στο [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

Στα παρακάτω βήματα, που θα δημιουργηθεί ένα ευρετήριο, αποστολή εγγράφων, ερώτημα στο ευρετήριο, και ερωτήματος, στη συνέχεια, το σύστημα για πληροφορίες υπηρεσίας.

Για να ολοκληρώσετε αυτά τα βήματα, χρειάζεστε μια υπηρεσία αναζήτησης Azure και `api-key`. Για οδηγίες σχετικά με τον τρόπο για να ξεκινήσετε, ανατρέξτε στο θέμα [Δημιουργία μιας υπηρεσίας αναζήτησης Azure στην πύλη](search-create-service-portal.md) .

## <a name="create-an-index"></a>Δημιουργία ευρετηρίου

1. Έναρξη Fiddler. Στο μενού **αρχείο** , απενεργοποιήστε την **Καταγραφή κίνηση** για να αποκρύψετε άσχετοι δραστηριότητας HTTP που δεν σχετίζεται με την τρέχουσα εργασία.

3. Στην καρτέλα **σύνθεσης** , θα διατύπωση μια αίτηση που μοιάζει με το παρακάτω στιγμιότυπο οθόνης.

    ![][1]

2. Επιλέξτε **ΤΟΠΟΘΈΤΗΣΗ**.

3. Πληκτρολογήστε μια διεύθυνση URL που καθορίζει τη διεύθυνση URL της υπηρεσίας, αίτηση χαρακτηριστικά και την έκδοση api. Ορισμένες πληροφορίες πρέπει να λάβετε υπόψη:
   + Χρησιμοποιήστε το ως το πρόθεμα HTTPS.
   + Χαρακτηριστικό αίτησης είναι "/ ευρετήρια/ξενοδοχεία". Αυτό σας ενημερώνει για αναζήτηση για να δημιουργήσετε ένα ευρετήριο με το όνομα 'ξενοδοχεία'.
   + Έκδοση API είναι πεζών-κεφαλαίων, που έχουν καθοριστεί ως "; έκδοση api = 2015-02-28". Εκδόσεις API είναι σημαντικό επειδή αναζήτησης Azure αναπτύσσει τακτικά ενημερώσεις. Σπάνιων φορές, μια ενημερωμένη έκδοση υπηρεσία ενδέχεται να δημιουργήσει μια αλλαγή πρόσφατες για το API. Για αυτόν το λόγο, Azure αναζήτησης απαιτεί μια έκδοση api σε κάθε αίτηση, έτσι ώστε να τον πλήρη έλεγχο επάνω σε μία από τις που χρησιμοποιείται.

    Την πλήρη διεύθυνση URL πρέπει να είναι παρόμοιο με το παρακάτω παράδειγμα.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Καθορίστε την κεφαλίδα αίτησης, αντικαθιστώντας του κεντρικού υπολογιστή και του api κλειδιού με τιμές που είναι έγκυροι για την υπηρεσία.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  Στο σώμα αίτηση, επικολλήστε τα πεδία που απαρτίζουν τον ορισμό του ευρετηρίου.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Κάντε κλικ στην επιλογή **Εκτέλεση**.

Σε μερικά δευτερόλεπτα, θα πρέπει να βλέπετε μια απόκριση HTTP 201 στη λίστα περίοδο λειτουργίας, που υποδεικνύει ότι το ευρετήριο που δημιουργήθηκε με επιτυχία.

Εάν λάβετε HTTP 504, επιβεβαιώστε τη διεύθυνση URL καθορίζει HTTPS. Εάν βλέπετε HTTP 400 ή 404, ελέγξτε το σώμα της αίτησης για να επαληθεύσετε ότι παρουσιάστηκαν σφάλματα χωρίς αντιγραφή επικόλληση. Ένα HTTP 403 συνήθως υποδεικνύει ένα πρόβλημα με το πλήκτρο api (μη έγκυρο κλειδί ή κάποιο πρόβλημα σύνταξη με το πώς έχει καθοριστεί το api-key).

## <a name="load-documents"></a>Φόρτωση εγγράφων

Στην καρτέλα **σύνθεσης** , την αίτησή σας για να δημοσιεύσετε έγγραφα θα είναι ως εξής. Στο σώμα της πρόσκλησης σε περιέχει τα δεδομένα αναζήτησης για το 4 ξενοδοχεία.

   ![][2]

1. Επιλέξτε **ΚΑΤΑΧΏΡΗΣΗ**.

2.  Πληκτρολογήστε μια διεύθυνση URL που ξεκινά με το πρόθεμα HTTPS, ακολουθούμενο από σας διεύθυνση URL της υπηρεσίας, ακολουθούμενο από "/indexes/ <' όνομα_ευρετηρίου' >/έγγραφα/ευρετηρίου; έκδοση api = 2015-02-28". Την πλήρη διεύθυνση URL πρέπει να είναι παρόμοιο με το παρακάτω παράδειγμα.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Κεφαλίδα αίτησης πρέπει να είναι η ίδια με την προηγούμενη. Μην ξεχάσετε να αντικατασταθεί ο κεντρικός υπολογιστής και api του κλειδιού με τιμές που είναι έγκυροι για την υπηρεσία.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Το κυρίως κείμενο της αίτησης περιέχει τέσσερα έγγραφα που θα προστεθεί στο ευρετήριο ξενοδοχεία.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
                "hotelName": "Fancy Stay",
                "category": "Luxury",
                "tags": ["pool", "view", "wifi", "concierge"],
                "parkingIncluded": false,
                "smokingAllowed": false,
                "lastRenovationDate": "2010-06-27T00:00:00Z",
                "rating": 5,
                "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
              },
              {
                "@search.action": "upload",
                "hotelId": "2",
                "baseRate": 79.99,
                "description": "Cheapest hotel in town",
                "hotelName": "Roach Motel",
                "category": "Budget",
                "tags": ["motel", "budget"],
                "parkingIncluded": true,
                "smokingAllowed": true,
                "lastRenovationDate": "1982-04-28T00:00:00Z",
                "rating": 1,
                "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
              },
              {
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Κάντε κλικ στην επιλογή **Εκτέλεση**.

Σε μερικά δευτερόλεπτα, θα πρέπει να βλέπετε μια απόκριση HTTP 200 στη λίστα περιόδου λειτουργίας. Αυτό υποδεικνύει τα έγγραφα που έχουν δημιουργηθεί με επιτυχία. Εάν λάβετε ένα 207, τουλάχιστον ένα έγγραφο απέτυχε η αποστολή. Εάν λάβετε ένα 404, έχετε ένα συντακτικό σφάλμα στην κεφαλίδα ή στο κυρίως κείμενο της αίτησης.

## <a name="query-the-index"></a>Ερώτημα στο ευρετήριο

Τώρα που έχουν φορτωθεί ένα ευρετήριο και έγγραφα, μπορείτε να εκδώσετε ερωτήματα σε σχέση με τους.  Στην καρτέλα **σύνθεσης** , μια εντολή **GET** που εκτελεί ερωτήματα υπηρεσία σας θα είναι παρόμοιο με το παρακάτω στιγμιότυπο οθόνης.

   ![][3]

1.  Επιλέξτε **ΛΉΨΗ**.

2.  Πληκτρολογήστε μια διεύθυνση URL που ξεκινά με το πρόθεμα HTTPS, ακολουθούμενο από σας διεύθυνση URL της υπηρεσίας, ακολουθούμενο από "/indexes/ <' όνομα_ευρετηρίου' > / έγγραφα;" και στη συνέχεια παράμετροι ερωτήματος. Για παράδειγμα, χρησιμοποιήστε την παρακάτω διεύθυνση URL, αντικαθιστώντας το δείγμα όνομα κεντρικού υπολογιστή με μία που είναι έγκυροι για την υπηρεσία.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Αυτό το ερώτημα αναζητά ο όρος "motel" και ανακτά όψη κατηγορίες για τους χαρακτηρισμούς.

3.  Κεφαλίδα αίτησης πρέπει να είναι η ίδια με την προηγούμενη. Μην ξεχάσετε να αντικατασταθεί ο κεντρικός υπολογιστής και api του κλειδιού με τιμές που είναι έγκυροι για την υπηρεσία.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

Ο κωδικός απόκρισης πρέπει να είναι 200 και το αποτέλεσμα απάντησης θα πρέπει να είναι παρόμοιο με το παρακάτω στιγμιότυπο οθόνης.

   ![][4]

Το παρακάτω παράδειγμα ερωτήματος είναι από τη [λειτουργία αναζήτησης ευρετηρίου (Azure API αναζήτησης)](http://msdn.microsoft.com/library/dn798927.aspx) στο MSDN. Πολλά από τα ερωτήματα παράδειγμα σε αυτό το θέμα περιέχει κενά διαστήματα, η οποία δεν επιτρέπεται στην Fiddler. Αντικατάσταση κάθε χώρος με έναν + χαρακτήρα πριν από την επικόλληση στο ερώτημα συμβολοσειράς πριν να επιχειρήσετε το ερώτημα στο Fiddler.

**Πριν από την κενά διαστήματα αντικαθίστανται:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Αφού κενά διαστήματα αντικαθίστανται με +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>Ερώτημα στο σύστημα

Μπορείτε επίσης να υποβάλετε ερώτημα στο σύστημα για να λάβετε καταμετρά εγγράφου και κατανάλωση χώρου αποθήκευσης. Στην καρτέλα **σύνθεσης** , θα είναι παρόμοιο με το ακόλουθο την αίτησή σας και την απόκριση θα επιστρέψει το πλήθος για τον αριθμό των εγγράφων και χώρο που χρησιμοποιείται.

 ![][5]

1.  Επιλέξτε **ΛΉΨΗ**.

2.  Πληκτρολογήστε μια διεύθυνση URL που περιλαμβάνει σας διεύθυνση URL της υπηρεσίας, ακολουθούμενο από "/ ευρετήρια/ξενοδοχεία στατιστικές; έκδοση api = 2015-02-28":

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Καθορίστε την κεφαλίδα αίτησης, αντικαθιστώντας του κεντρικού υπολογιστή και του api κλειδιού με τιμές που είναι έγκυροι για την υπηρεσία.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Αφήστε κενό το σώμα της αίτησης.

5.  Κάντε κλικ στην επιλογή **Εκτέλεση**. Θα πρέπει να βλέπετε έναν κωδικό κατάστασης HTTP 200 στη λίστα περιόδου λειτουργίας. Επιλέξτε την εγγραφή που έχουν καταχωρηθεί για την εντολή σας.

6.  Κάντε κλικ στην καρτέλα **επιθεωρήσεις** , κάντε κλικ στην καρτέλα **κεφαλίδες** και, στη συνέχεια, επιλέξτε τη μορφή JSON. Θα πρέπει να βλέπετε το μέγεθος count και αποθήκευσης εγγράφων (σε KB).

## <a name="next-steps"></a>Επόμενα βήματα

Ανατρέξτε στο θέμα [Διαχείριση της υπηρεσίας αναζήτησης σε Azure](search-manage.md) για μια χωρίς κώδικα προσεγγίσετε για τη διαχείριση και με χρήση της αναζήτησης Azure.


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
