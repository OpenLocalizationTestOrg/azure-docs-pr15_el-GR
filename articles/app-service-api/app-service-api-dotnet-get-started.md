<properties
    pageTitle="Γρήγορα αποτελέσματα με το API εφαρμογές και ASP.NET στην εφαρμογή υπηρεσίας | Microsoft Azure"
    description="Μάθετε πώς να δημιουργείτε, ανάπτυξη και εκμετάλλευση εφαρμογής API του ASP.NET στο Azure εφαρμογής υπηρεσίας, με χρήση του Visual Studio 2015."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="rachelap"/>

# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Γρήγορα αποτελέσματα με το API εφαρμογές ASP.NET και Swagger στο Azure εφαρμογής υπηρεσίας

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Αυτή είναι η πρώτη σε μια σειρά προγραμμάτων εκμάθησης που σας δείχνουν πώς μπορείτε να χρησιμοποιήσετε τις δυνατότητες της εφαρμογής υπηρεσίας Azure που είναι χρήσιμα για τη φιλοξενία RESTful APIs και ανάπτυξη.  Αυτό το πρόγραμμα εκμάθησης καλύπτει υποστήριξη για το API μετα-δεδομένων σε μορφή Swagger.

Θα μάθετε:

* Πώς μπορείτε να δημιουργήσετε και να αναπτύξετε [εφαρμογές API](app-service-api-apps-why-best-platform.md) στο Azure εφαρμογής υπηρεσίας, χρησιμοποιώντας εργαλεία που είναι ενσωματωμένο στο Visual Studio 2015.
* Μάθετε πώς μπορείτε να αυτοματοποιήσετε API εντοπισμού, χρησιμοποιώντας το πακέτο Swashbuckle NuGet για να υποδείξουν Swagger API μετα-δεδομένων.
* Πώς μπορείτε να χρησιμοποιήσετε Swagger API μετα-δεδομένων για την αυτόματη δημιουργία κώδικα προγράμματος-πελάτη για μια εφαρμογή API.

## <a name="sample-application-overview"></a>Δείγμα εφαρμογής Επισκόπηση

Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να εργαστείτε με μια απλή εκκρεμών εργασιών λίστα δείγμα εφαρμογής. Η εφαρμογή έχει ένα προσκήνιο μίας σελίδας εφαρμογής (SPA), ένα ASP.NET Web API μεσαίας και μια σειρά δεδομένων ASP.NET Web API.

![Εφαρμογές API δείγμα εφαρμογής διαγράμματος](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Ακολουθεί ένα στιγμιότυπο οθόνης του προσκηνίου της [AngularJS](https://angularjs.org/) .

![Εφαρμογές API δείγμα εφαρμογής λίστα εκκρεμών εργασιών](./media/app-service-api-dotnet-get-started/todospa.png)

Η λύση Visual Studio περιλαμβάνει τρία έργα:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** - προσκήνιο: μια SPA AngularJS που καλεί στο μεσαίο επίπεδο.

* **ToDoListAPI** - Μεσαίο επίπεδο: ένα έργο ASP.NET Web API που καλεί η σειρά δεδομένων για να εκτελέσετε λειτουργίες CRUD στην εκκρεμών εργασιών.

* **ToDoListDataAPI** - η σειρά δεδομένων: ένα έργο ASP.NET Web API που εκτελεί λειτουργίες CRUD σε εκκρεμών εργασιών.

Η αρχιτεκτονική τριών επιπέδων είναι μία από πολλές αρχιτεκτονικές που μπορείτε να υλοποιήσετε με χρήση του API εφαρμογές και εδώ χρησιμοποιούνται μόνο για σκοπούς επίδειξης. Ο κώδικας σε κάθε επίπεδο είναι τόσο απλή όσο το δυνατόν πιο για μια επίδειξη δυνατοτήτων API εφαρμογές. Για παράδειγμα, η σειρά δεδομένων χρησιμοποιεί μνήμη του διακομιστή και όχι μια βάση δεδομένων ως ο μηχανισμός της διατήρησης.

Στην ολοκλήρωση αυτό το πρόγραμμα εκμάθησης, θα έχετε τα δύο έργα Web API προς τα επάνω και την εκτέλεση στο cloud σε εφαρμογές της εφαρμογής υπηρεσίας API.

Το επόμενο πρόγραμμα εκμάθησης στη σειρά αναπτύσσει προσκηνίου της SPA στο cloud.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* ASP.NET Web API - τις οδηγίες του προγράμματος εκμάθησης θεωρείται ότι έχετε βασικές γνώσεις για τον τρόπο εργασίας με ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) στο Visual Studio.

* Λογαριασμός Azure - μπορείτε να [ανοίξετε ένα λογαριασμό Azure δωρεάν](/pricing/free-trial/?WT.mc_id=A261C142F) ή [Ενεργοποίηση του Visual Studio συνδρομητών πλεονεκτήματα](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν εγγραφείτε για ένα λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751). Εκεί, μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή μικρής διάρκειας starter στην εφαρμογή υπηρεσίας — **χωρίς πιστωτική κάρτα απαιτείται**και χωρίς δεσμεύσεις.

* Visual Studio 2015 με το [Azure SDK για .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) - το SDK εγκαθιστά αυτόματα Visual Studio 2015, εάν δεν το έχετε ήδη.

    * Στο Visual Studio, κάντε κλικ στο κουμπί Βοήθεια -> σχετικά με το Microsoft Visual Studio και βεβαιωθείτε ότι έχετε τα "Εργαλεία Azure εφαρμογής υπηρεσίας v2.9.1" ή νεότερη έκδοση.

    ![Azure vesion εργαλεία εφαρμογής](./media/app-service-api-dotnet-get-started/apiversion.png)

    >[AZURE.NOTE] Ανάλογα με το πόσες από τις εξαρτήσεις SDK που έχετε ήδη στον υπολογιστή σας, η εγκατάσταση του SDK μπορεί να διαρκέσει μεγάλο χρονικό διάστημα, από μερικά λεπτά για να μισή ώρα ή περισσότερα.

## <a name="download-the-sample-application"></a>Κάντε λήψη του δείγματος εφαρμογής

1. Κάντε λήψη του αποθετηρίου [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) .

    Μπορείτε να κάντε κλικ στο κουμπί **Λήψη ZIP** ή κλωνοποίηση του αποθετηρίου στον τοπικό υπολογιστή σας.

2. Ανοίξτε τη λύση ToDoList στο Visual Studio 2015 ή 2013.
   1. Θα πρέπει να εμπιστευτείτε όλες τις λύσεις.
        ![Προειδοποίηση ασφαλείας](./media/app-service-api-dotnet-get-started/securitywarning.png)

3. Δημιουργία της λύσης (CTRL + SHIFT + B) για να επαναφέρετε τα πακέτα NuGet.

    Εάν θέλετε να δείτε την εφαρμογή σε λειτουργία πριν να την αναπτύξετε, μπορείτε να το εκτελέσετε τοπικά. Βεβαιωθείτε ότι ToDoListDataAPI είναι το έργο σας εκκίνησης και εκτελέστε τη λύση. Θα πρέπει να περιμένουν για να εμφανιστεί το μήνυμα σφάλματος HTTP 403 στο πρόγραμμα περιήγησης.

## <a name="use-swagger-api-metadata-and-ui"></a>Χρήση του Swagger API μετα-δεδομένων και περιβάλλον εργασίας Χρήστη

Υποστήριξη για το API [Swagger](http://swagger.io/) 2.0 μετα-δεδομένων είναι ενσωματωμένο στο Azure εφαρμογής υπηρεσίας. Κάθε εφαρμογή API να καθορίσετε ένα τελικό σημείο διεύθυνση URL που επιστρέφει μετα-δεδομένων για το API στη μορφή Swagger JSON. Τα μετα-δεδομένα που επιστρέφονται από αυτό το τελικό σημείο μπορεί να χρησιμοποιηθεί για τη δημιουργία κώδικα προγράμματος-πελάτη.

Ένα έργο ASP.NET Web API δυναμικά μπορεί να δημιουργήσουν Swagger μετα-δεδομένων χρησιμοποιώντας το πακέτο NuGet [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) . Το πακέτο Swashbuckle NuGet είναι ήδη εγκατεστημένο το ToDoListDataAPI και ToDoListAPI έργα που έχετε λάβει.

Σε αυτήν την ενότητα του προγράμματος εκμάθησης, εξετάστε τα που δημιουργήθηκε μετα-δεδομένα Swagger 2.0 και, στη συνέχεια, δοκιμάσετε μια δοκιμαστική περιβάλλοντος εργασίας Χρήστη που βασίζεται σε τα μετα-δεδομένα Swagger.

1. Ορίστε το έργο ToDoListDataAPI (**μην** το έργο ToDoListAPI) με το έργο της εκκίνησης.

    ![Ορισμός ToDoDataAPI ως εκκίνησης έργου](./media/app-service-api-dotnet-get-started/startupproject.png)

2. Πατήστε το πλήκτρο F5 ή κάντε κλικ στην επιλογή **Εντοπισμός σφαλμάτων > Έναρξη εντοπισμού** για να εκτελέσετε το έργο σε λειτουργία εντοπισμού σφαλμάτων.

    Ανοίγει το πρόγραμμα περιήγησης και εμφανίζει τη σελίδα σφάλματος HTTP 403.

3. Στη γραμμή διευθύνσεων του προγράμματος περιήγησης, προσθέστε `swagger/docs/v1` μέχρι το τέλος της γραμμής και, στη συνέχεια, πατήστε το πλήκτρο Return. (Η διεύθυνση URL είναι `http://localhost:45914/swagger/docs/v1`.)

    Αυτή είναι η προεπιλεγμένη διεύθυνση URL που χρησιμοποιούνται από Swashbuckle για να επιστρέψετε Swagger 2.0 JSON μετα-δεδομένων για το API.

    Εάν χρησιμοποιείτε τον Internet Explorer, το πρόγραμμα περιήγησης σάς ζητά να κάνετε λήψη ενός αρχείου *v1.json* .

    ![Λήψη μετα-δεδομένων JSON στο IE](./media/app-service-api-dotnet-get-started/iev1json.png)

    Εάν χρησιμοποιείτε το Chrome, Firefox ή το άκρο, το πρόγραμμα περιήγησης εμφανίζει το JSON στο παράθυρο του προγράμματος περιήγησης. Διάφορα προγράμματα περιήγησης χειρισμού JSON διαφορετικά και παράθυρο του προγράμματος περιήγησης μπορεί να μην εμφανίζεται ακριβώς όπως στο παράδειγμα.

    ![Μετα-δεδομένων JSON στο Chrome](./media/app-service-api-dotnet-get-started/chromev1json.png)

    Το παρακάτω παράδειγμα δείχνει η πρώτη ενότητα μετα-δεδομένα, Swagger για το API, με τον ορισμό για τη μέθοδο Get. Αυτό μετα-δεδομένων είναι τι μονάδες Swagger περιβάλλον εργασίας Χρήστη που χρησιμοποιείτε στα ακόλουθα βήματα και χρησιμοποιείτε στην επόμενη ενότητα του προγράμματος εκμάθησης για την αυτόματη δημιουργία κώδικα προγράμματος-πελάτη.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

4. Κλείστε το πρόγραμμα περιήγησης και να διακόψετε τον εντοπισμό σφαλμάτων Visual Studio.

5. Στο έργο ToDoListDataAPI στην **Εξερεύνηση λύσεων**, ανοίξτε το αρχείο *App_Start\SwaggerConfig.cs* , στη συνέχεια, κάντε κύλιση προς τα κάτω γραμμής 174 και καταργήστε τα σχόλια από τον παρακάτω κώδικα.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    Το αρχείο *SwaggerConfig.cs* δημιουργείται κατά την εγκατάσταση του πακέτου Swashbuckle σε ένα έργο. Το αρχείο παρέχει πολλούς τρόπους για να ρυθμίσετε τις παραμέτρους Swashbuckle.

    Ο κώδικας που έχετε uncommented επιτρέπει Swagger περιβάλλον εργασίας Χρήστη που χρησιμοποιείτε στα παρακάτω βήματα. Όταν δημιουργείτε ένα έργο Web API χρησιμοποιώντας το πρότυπο έργου εφαρμογής API, αυτός ο κώδικας είναι σχόλια από προεπιλογή ως μέτρο ασφαλείας.

6. Εκτελέστε ξανά το έργο.

7. Στη γραμμή διευθύνσεων του προγράμματος περιήγησης, προσθέστε `swagger` μέχρι το τέλος της γραμμής και, στη συνέχεια, πατήστε το πλήκτρο Return. (Η διεύθυνση URL είναι `http://localhost:45914/swagger`.)

8. Όταν εμφανιστεί η σελίδα Swagger περιβάλλοντος εργασίας Χρήστη, κάντε κλικ στην επιλογή **ToDoList** για να δείτε τις διαθέσιμες μεθόδους.

    ![Swagger διαθέσιμες μεθόδους περιβάλλοντος εργασίας Χρήστη](./media/app-service-api-dotnet-get-started/methods.png)

9. Κάντε κλικ στο πρώτο κουμπί **λήψη** στη λίστα.

10. Στην ενότητα **παραμέτρων** , εισαγάγετε έναν αστερίσκο ως τιμή του το `owner` παράμετρο, και, στη συνέχεια, κάντε κλικ στην επιλογή **προσπαθήστε να το εμφανίσετε**.

    Όταν προσθέτετε ελέγχου ταυτότητας σε νεότερες εκδόσεις προγραμμάτων εκμάθησης, μεσαίο επίπεδο παρέχει το Αναγνωριστικό χρήστη πραγματική στο επίπεδο δεδομένων. Προς το παρόν, όλες οι εργασίες θα έχουν αστερίσκο ως Αναγνωριστικού του κατόχου ενώ η εφαρμογή θα εκτελείται χωρίς έλεγχο ταυτότητας με δυνατότητα.

    ![Περιβάλλον εργασίας Χρήστη swagger δοκιμή](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    Swagger περιβάλλον εργασίας Χρήστη του κλήσεις γρήγορα ToDoList μέθοδο και εμφανίζει τον κωδικό απάντησης και JSON τα αποτελέσματα.

    ![Περιβάλλον εργασίας Χρήστη swagger δοκιμή αποτελεσμάτων](./media/app-service-api-dotnet-get-started/gettryitout.png)

11. Κάντε κλικ στην **καταχώρηση**και, στη συνέχεια, κάντε κλικ στο πλαίσιο κάτω από το **Σχήμα μοντέλου**.

    Κάνοντας κλικ στην επιλογή το σχήμα μοντέλου prefills πλαίσιο εισαγωγής, όπου μπορείτε να καθορίσετε την τιμή παραμέτρου για τη μέθοδο Post. (Εάν αυτό δεν λειτουργεί στον Internet Explorer, χρησιμοποιήστε ένα διαφορετικό πρόγραμμα περιήγησης ή εισαγάγετε με μη αυτόματο τρόπο την τιμή της παραμέτρου στο επόμενο βήμα.)  

    ![Περιβάλλον εργασίας Χρήστη swagger δοκιμή δημοσίευση](./media/app-service-api-dotnet-get-started/post.png)

12. Αλλαγή του JSON στο το `todo` παράμετρο εισόδου πλαίσιο ώστε να εμφανίζεται το παρακάτω παράδειγμα, ή να αντικαταστήσετε το δικό σας κείμενο περιγραφή:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

13. Κάντε κλικ στην επιλογή **προσπαθήστε να το εμφανίσετε**.

    Το API ToDoList επιστρέφει έναν κωδικό απόκρισης HTTP 204 που υποδεικνύει την επιτυχία.

14. Κάντε κλικ στο πρώτο κουμπί **λήψη** και, στη συνέχεια, σε αυτήν την ενότητα της σελίδας, κάντε κλικ στο κουμπί **προσπαθήστε να το εμφανίσετε** .

    Η απόκριση μέθοδο Get περιλαμβάνει τώρα το νέο στοιχείο.

15. Προαιρετικό: Δοκιμάστε επίσης τη θέση, διαγραφή και να, μεθόδους Αναγνωριστικό.

16. Κλείστε το πρόγραμμα περιήγησης και να διακόψετε τον εντοπισμό σφαλμάτων Visual Studio.

Swashbuckle λειτουργεί με οποιοδήποτε έργο ASP.NET Web API. Εάν θέλετε να προσθέσετε Swagger γενιάς μετα-δεδομένων σε ένα υπάρχον έργο, απλώς εγκαταστήστε το πακέτο Swashbuckle.

>[AZURE.NOTE] Swagger μετα-δεδομένων περιλαμβάνει ένα μοναδικό Αναγνωριστικό για κάθε εργασία API. Από προεπιλογή, Swashbuckle μπορεί να δημιουργήσουν διπλότυπες Swagger λειτουργίας αναγνωριστικών για τις μεθόδους ελεγκτή Web API. Αυτό συμβαίνει εάν ελεγκτή σας έχει φορτωθεί όπως υπερβολικά μεθόδους HTTP, `Get()` και `Get(id)`. Για πληροφορίες σχετικά με τον τρόπο χειρισμού των τύποι, ανατρέξτε στο θέμα [που δημιουργούνται από το Προσαρμογή Swashbuckle API ορισμών](app-service-api-dotnet-swashbuckle-customize.md). Εάν δημιουργήσετε ένα έργο Web API στο Visual Studio, χρησιμοποιώντας το πρότυπο εφαρμογής API Azure, κώδικα που δημιουργεί τη λειτουργία μοναδικών αναγνωριστικών προστίθεται αυτόματα στο αρχείο *SwaggerConfig.cs* .  

## <a id="createapiapp"></a>Δημιουργία εφαρμογής API στο Azure και ανάπτυξη κώδικα σε αυτό

Σε αυτήν την ενότητα, μπορείτε να χρησιμοποιήσετε Azure εργαλεία που είναι ενσωματωμένο στο τον οδηγό του Visual Studio **Δημοσίευση Web** για να δημιουργήσετε μια νέα εφαρμογή API στο Azure. Στη συνέχεια, αναπτύξτε το έργο ToDoListDataAPI νέα εφαρμογή API και καλέστε το API, εκτελώντας το περιβάλλον εργασίας Χρήστη Swagger.

1. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο ToDoListDataAPI και, στη συνέχεια, κάντε κλικ στο κουμπί **Δημοσίευση**.

    ![Κάντε κλικ στην επιλογή δημοσίευση στο Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)

2.  Στο **προφίλ** βήμα του οδηγού **Δημοσίευση Web** , κάντε κλικ στην επιλογή **Microsoft Azure εφαρμογής υπηρεσίας**.

    ![Κάντε κλικ στην επιλογή Azure εφαρμογής υπηρεσίας σε δημοσίευση Web](./media/app-service-api-dotnet-get-started/selectappservice.png)

3. Πραγματοποιήστε είσοδο στο λογαριασμό σας Azure, εάν δεν το έχετε κάνει ήδη, ή να ανανεώσετε τα διαπιστευτήριά σας, εάν σας έχει λήξει.

4. Στο παράθυρο διαλόγου εφαρμογής υπηρεσίας, επιλέξτε το Azure **συνδρομή** που θέλετε να χρησιμοποιήσετε και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Κάντε κλικ στην επιλογή "Δημιουργία" στο παράθυρο διαλόγου εφαρμογή υπηρεσίας](./media/app-service-api-dotnet-get-started/clicknew.png)

    Εμφανίζεται η καρτέλα **κεντρικού υπολογιστή** στο παράθυρο διαλόγου **Δημιουργία εφαρμογής υπηρεσίας** .

    Επειδή αναπτύξετε ένα έργο Web API που έχει εγκατασταθεί Swashbuckle, Visual Studio προϋποθέτει ότι θέλετε να δημιουργήσετε μια εφαρμογή API. Υποδεικνύεται από τον τίτλο **Όνομα εφαρμογής API** και από το γεγονός ότι η **Αλλαγή τύπου** αναπτυσσόμενης λίστας έχει οριστεί σε **Εφαρμογή API**.

    ![Εφαρμογή τύπος στο παράθυρο διαλόγου εφαρμογή υπηρεσίας](./media/app-service-api-dotnet-get-started/apptype.png)

5. Πληκτρολογήστε ένα **Όνομα εφαρμογής API** που είναι μοναδικές στον τομέα *azurewebsites.net* . Μπορείτε να αποδεχθείτε το προεπιλεγμένο όνομα που προτείνει το Visual Studio.

    Εάν εισαγάγετε ένα όνομα που κάποιος άλλος έχει χρησιμοποιήσει ήδη, μπορείτε να δείτε ένα κόκκινο θαυμαστικό προς τα δεξιά.

    Θα είναι η διεύθυνση URL της εφαρμογής API `{API app name}.azurewebsites.net`.

6. Στο την αναπτυσσόμενη λίστα **Ομάδα πόρων** , κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, πληκτρολογήστε "ToDoListGroup" ή κάποιο άλλο όνομα εάν προτιμάτε.

    Μια ομάδα πόρων είναι μια συλλογή Azure πόρων όπως API εφαρμογές, βάσεις δεδομένων, ΣΠΣ, και ούτω καθεξής. Για αυτό το πρόγραμμα εκμάθησης, είναι καλύτερα να δημιουργήσετε μια νέα ομάδα πόρων, επειδή που διευκολύνει την διαγραφή σε ένα βήμα όλους τους πόρους Azure που δημιουργείτε για το πρόγραμμα εκμάθησης.

    Αυτό το πλαίσιο σάς επιτρέπει να επιλέξετε μια υπάρχουσα [ομάδα πόρων](../azure-resource-manager/resource-group-overview.md) ή να δημιουργήσετε ένα νέο, πληκτρολογώντας σε ένα όνομα που είναι διαφορετικό από υπάρχουσες ομάδες πόρων στη συνδρομή σας.

7. Κάντε κλικ στο κουμπί **Δημιουργία** δίπλα στην επιλογή **Εφαρμογή υπηρεσίας Σχεδιασμός** αναπτυσσόμενο μενού.

    Το στιγμιότυπο οθόνης εμφανίζει τιμές δείγματος για το **Όνομα εφαρμογής API**, **συνδρομής**και **Ομάδα πόρων** --σας τιμές θα είναι διαφορετικές.

    ![Δημιουργία εφαρμογής υπηρεσίας παραθύρου διαλόγου](./media/app-service-api-dotnet-get-started/createas.png)

    Στα παρακάτω βήματα μπορείτε να δημιουργήσετε ένα πρόγραμμα εφαρμογής υπηρεσίας για τη νέα ομάδα πόρων. Ένα πρόγραμμα εφαρμογής υπηρεσίας καθορίζει τους πόρους υπολογισμού που εκτελεί την εφαρμογή API σε. Για παράδειγμα, εάν επιλέξετε το επίπεδο δωρεάν, την εφαρμογή API εκτελείται σε κοινόχρηστο ΣΠΣ, ενώ για ορισμένες σειρές επί πληρωμή εκτελείται σε αποκλειστική VM. Για πληροφορίες σχετικά με τα σχέδια εφαρμογής υπηρεσίας, ανατρέξτε στο θέμα [Επισκόπηση προγράμματος εφαρμογής υπηρεσίας](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

8. Στο παράθυρο διαλόγου **Ρύθμιση παραμέτρων πρόγραμμα εφαρμογής υπηρεσίας** , πληκτρολογήστε "ToDoListPlan" ή κάποιο άλλο όνομα εάν προτιμάτε.

9. Στην αναπτυσσόμενη λίστα **θέση** , επιλέξτε τη θέση που βρίσκεται πιο κοντά σε εσάς.

    Αυτή η ρύθμιση καθορίζει ποιες Azure κέντρο δεδομένων της εφαρμογής σας θα εκτελείται σε. Επιλέξτε μια θέση Κλείσιμο για να ελαχιστοποιήσετε [λανθάνοντος χρόνου](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

10. Στο την αναπτυσσόμενη λίστα **μέγεθος** , κάντε κλικ στην επιλογή **δωρεάν**.

    Για αυτό το πρόγραμμα εκμάθησης, η σειρά δωρεάν τιμολόγησης παρέχει επαρκείς επιδόσεις.

11. Στο παράθυρο διαλόγου **Ρύθμιση παραμέτρων πρόγραμμα εφαρμογής υπηρεσίας** , κάντε κλικ στο **κουμπί OK**.

    ![Κάντε κλικ στο κουμπί OK σε ρύθμιση παραμέτρων πρόγραμμα εφαρμογής υπηρεσίας](./media/app-service-api-dotnet-get-started/configasp.png)

12. Στο παράθυρο διαλόγου **Δημιουργία εφαρμογής υπηρεσίας** , κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Κάντε κλικ στην επιλογή "Δημιουργία" στο παράθυρο διαλόγου Δημιουργία εφαρμογής υπηρεσίας](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio δημιουργεί την εφαρμογή API και δημοσίευση προφίλ που περιέχει όλες τις απαιτούμενες ρυθμίσεις για την εφαρμογή API. Στη συνέχεια, ανοίγει τον οδηγό **Δημοσίευση Web** , το οποίο θα χρησιμοποιήσετε για να αναπτύξετε το έργο.

    Ο οδηγός **Δημοσίευση Web** ανοίγει στην καρτέλα " **σύνδεση** " (απεικονίζεται παρακάτω).

    Στην καρτέλα **σύνδεση** , τις ρυθμίσεις του **διακομιστή** και **όνομα τοποθεσίας** οδηγούν σε εφαρμογή API. Το **όνομα χρήστη** και **τον κωδικό πρόσβασης** είναι τα διαπιστευτήριά ανάπτυξης που δημιουργεί Azure για εσάς. Μετά την ανάπτυξη, Visual Studio, ανοίγει ένα πρόγραμμα περιήγησης για τη **Διεύθυνση URL προορισμού** (που είναι ο σκοπός μόνο για τη **Διεύθυνση URL προορισμού**).  

13. Κάντε κλικ στο κουμπί **Επόμενο**.

    ![Κάντε κλικ στο κουμπί "Επόμενο" στην καρτέλα σύνδεση του δημοσίευση Web](./media/app-service-api-dotnet-get-started/connnext.png)

    Στην επόμενη καρτέλα είναι η καρτέλα **Ρυθμίσεις** (απεικονίζεται παρακάτω). Εδώ μπορείτε να αλλάξετε στην καρτέλα ρύθμισης παραμέτρων Δόμηση για να αναπτύξετε μια Δόμηση εντοπισμού σφαλμάτων για [τον εντοπισμό σφαλμάτων απομακρυσμένο](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). Στην καρτέλα επίσης προσφέρει πολλές **Επιλογές δημοσίευση αρχείου**:

    * Κατάργηση επιπλέον αρχείων στον προορισμό
    * Προ-μεταγλώττιση κατά τη δημοσίευση
    * Αποκλεισμός αρχείων από το φάκελο App_Data

    Για αυτό το πρόγραμμα εκμάθησης δεν χρειάζεστε κάποια από τα εξής. Για λεπτομερείς εξηγήσεις των τι κάνουν, ανατρέξτε στο θέμα [Τρόπος: ανάπτυξη ενός Web με ένα κλικ δημοσίευση έργου στο Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).

14. Κάντε κλικ στο κουμπί **Επόμενο**.

    ![Κάντε κλικ στο κουμπί "Επόμενο" στην καρτέλα "Ρυθμίσεις" του δημοσίευση Web](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Επόμενο είναι η **Προεπισκόπηση** καρτέλα (απεικονίζεται παρακάτω), που σας δίνει μια ευκαιρία για να δείτε ποια αρχεία πρόκειται να αντιγραφούν από το έργο σας στην εφαρμογή API. Όταν την ανάπτυξη ενός έργου σε μια εφαρμογή API που έχετε ήδη αναπτυχθεί σε προηγούμενη έκδοση, αντιγράφονται μόνο τα αρχεία που τροποποιήθηκαν. Εάν θέλετε να δείτε μια λίστα με τι θα αντιγραφούν, μπορείτε να κάνετε κλικ στο κουμπί **Έναρξη Preview** .

15. Κάντε κλικ στο κουμπί **Δημοσίευση**.

    ![Κάντε κλικ στην επιλογή "Δημοσίευση" στην καρτέλα προεπισκόπηση του δημοσίευση Web](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Visual Studio αναπτύσσει το έργο ToDoListDataAPI νέα εφαρμογή API. Στο παράθυρο **εξόδου** καταγράφει επιτυχής ανάπτυξης και μια σελίδα "δημιουργήθηκε με επιτυχία" εμφανίζεται στο παράθυρο του προγράμματος περιήγησης ανοίξει στη διεύθυνση URL της εφαρμογής API.

    ![Επιτυχής ανάπτυξης παραθύρου εξόδου](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Νέα σελίδα εφαρμογής που δημιουργήθηκε με επιτυχία API](./media/app-service-api-dotnet-get-started/appcreated.png)

16. Προσθήκη "swagger" στη διεύθυνση URL στη γραμμή διευθύνσεων του προγράμματος περιήγησης και, στη συνέχεια, πατήστε το πλήκτρο Enter. (Η διεύθυνση URL είναι `http://{apiappname}.azurewebsites.net/swagger`.)

    Το πρόγραμμα περιήγησης εμφανίζει το ίδιο Swagger περιβάλλον εργασίας Χρήστη που είδατε προηγουμένως, αλλά τώρα εκτελείται στο cloud. Δοκιμάστε τη μέθοδο Get και δείτε ότι είστε πίσω στα προεπιλεγμένα στοιχεία 2 εκκρεμών εργασιών. Οι αλλαγές που κάνατε προηγουμένως αποθηκεύτηκαν στη μνήμη στον τοπικό υπολογιστή.

17. Ανοίξτε την [πύλη του Azure](https://portal.azure.com/).

    Πύλη του Azure είναι περιβάλλον web για τη Διαχείριση Azure πόρους όπως API εφαρμογές.

18. Κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες > εφαρμογή υπηρεσιών**.

    ![Αναζήτηση υπηρεσιών εφαρμογής](./media/app-service-api-dotnet-get-started/browseas.png)

19. Στο η **Εφαρμογή υπηρεσιών** blade, βρείτε και επιλέξτε τη νέα εφαρμογή API. (Στην πύλη του Azure, των windows που ανοίγουν προς τα δεξιά ονομάζονται *λεπίδες*.)

    ![Εφαρμογή υπηρεσιών blade](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    Άνοιγμα δύο πτερύγια. Μία blade έχει μια επισκόπηση της εφαρμογής API και μία έχει μια μεγάλη λίστα των ρυθμίσεων που μπορείτε να προβάλετε και να αλλάξετε.

20. Στο το blade **Ρυθμίσεις** , βρείτε την ενότητα **API** και κάντε κλικ στην επιλογή **Ορισμός API**.

    ![Ορισμός API στο blade ρυθμίσεις](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    Το **API ορισμού** blade σάς επιτρέπει να καθορίσετε τη διεύθυνση URL που επιστρέφει Swagger 2.0 μετα-δεδομένων σε μορφή JSON. Όταν το Visual Studio δημιουργεί την εφαρμογή API, ορίζει τη διεύθυνση URL ορισμού API για την προεπιλεγμένη τιμή για που δημιουργούνται από το Swashbuckle μετα-δεδομένα που είδατε προηγουμένως, δηλαδή την εφαρμογή API της βάσης διεύθυνση URL συν `/swagger/docs/v1`.

    ![Διεύθυνση URL ορισμού API](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Όταν επιλέγετε μια εφαρμογή API για τη δημιουργία κώδικα προγράμματος-πελάτη για αυτό, Visual Studio ανακτά τα μετα-δεδομένα από αυτήν τη διεύθυνση URL.

## <a id="codegen"></a>Δημιουργία κώδικα προγράμματος-πελάτη για τη σειρά δεδομένων

Ένα από τα πλεονεκτήματα της ενσωμάτωση Swagger σε εφαρμογές Azure API είναι Δημιουργία αυτόματης κώδικα. Κλάσεις προγράμματος-πελάτη που έχει δημιουργηθεί διευκολύνουν σύνταξη κώδικα που καλεί μια εφαρμογή API.

Το έργο ToDoListAPI έχει ήδη τον κώδικα που έχει δημιουργηθεί προγράμματος-πελάτη, αλλά στα ακόλουθα βήματα που θα διαγράψετε και να δημιουργήσετε ξανά, για να δείτε πώς μπορείτε να κάνετε τη δημιουργία κώδικα.

1. Στο Visual Studio **Εξερεύνηση λύσεων**, μέσα στο έργο ToDoListAPI, διαγράψτε το φάκελο *ToDoListDataAPI* . **Προσοχή: Διαγράψτε μόνο το φάκελο, δεν ToDoListDataAPI έργου.**

    ![Διαγραφή που δημιουργούνται από το πρόγραμμα-πελάτη κώδικα](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    Αυτός ο φάκελος που δημιουργήθηκε χρησιμοποιώντας τη διαδικασία δημιουργίας κώδικα που πρόκειται να ακολουθήσετε.

2. Κάντε δεξί κλικ στο έργο ToDoListAPI και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη > REST API πελάτη**.

    ![Προσθήκη REST API προγράμματος-πελάτη στο Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. Στο παράθυρο διαλόγου **Προσθήκη REST API του προγράμματος-πελάτη** , κάντε κλικ στην επιλογή **Swagger διεύθυνση URL**και, στη συνέχεια, κάντε κλικ στην επιλογή **Επιλογή διαθέσιμου στοιχείου Azure**.

    ![Επιλέξτε Azure περιουσιακών στοιχείων](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

4. Στο παράθυρο διαλόγου **Εφαρμογής υπηρεσίας** , αναπτύξτε την ομάδα των πόρων που χρησιμοποιείτε για αυτό το πρόγραμμα εκμάθησης και επιλέξτε την εφαρμογή API σας και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

    ![Επιλέξτε εφαρμογή API για τη δημιουργία κώδικα](./media/app-service-api-dotnet-get-started/codegenselect.png)

    Παρατηρήστε ότι όταν επιστρέψετε στο παράθυρο διαλόγου **Προσθήκη REST API του προγράμματος-πελάτη** , στο πλαίσιο κειμένου έχει συμπληρωθεί με τον ορισμό API τιμή διεύθυνσης URL που είδατε προηγουμένως στην πύλη.

    ![Διεύθυνση URL ορισμού API](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Ένας εναλλακτικός τρόπος για να λάβετε μετα-δεδομένων για τη δημιουργία κώδικα είναι να εισαγάγετε τη διεύθυνση URL απευθείας αντί να μεταβείτε στο παράθυρο αναζήτηση. Ή εάν θέλετε να δημιουργήσετε κωδικός προγράμματος-πελάτη πριν από την ανάπτυξη στον Azure, που θα μπορούσε να εκτελέσετε τοπικά το έργο Web API, μεταβείτε στη διεύθυνση URL που παρέχει το αρχείο Swagger JSON, αποθηκεύστε το αρχείο και χρησιμοποιήστε την επιλογή **Επιλέξτε υπάρχον αρχείο Swagger μετα-δεδομένων** .

5. Στο παράθυρο διαλόγου **Προσθήκη REST API του προγράμματος-πελάτη** , κάντε κλικ στο **κουμπί OK**.

    Visual Studio δημιουργεί ένα φάκελο που ονομάζεται μετά την εφαρμογή API και κλάσεις προγράμματος-πελάτη.

    ![Αρχεία κώδικα για το πρόγραμμα-πελάτη που έχει δημιουργηθεί](./media/app-service-api-dotnet-get-started/codegenfiles.png)

6. Στο έργο ToDoListAPI, ανοίξτε *Controllers\ToDoListController.cs* για να δείτε τον κώδικα στη γραμμή 40 που καλεί το API χρησιμοποιώντας το πρόγραμμα-πελάτη που έχει δημιουργηθεί.

    Το παρακάτω τμήμα κώδικα δείχνει τον τρόπο ο κώδικας εμφανίζει το αντικείμενο προγράμματος-πελάτη και καλεί τη μέθοδο Get.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }

        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    Η παράμετρος κατασκευή λαμβάνει το τελικό σημείο URL από τη `toDoListDataAPIURL` ρύθμιση app. Στο αρχείο Web.config, αυτήν την τιμή έχει οριστεί στην τοπική των υπηρεσιών IIS Express διεύθυνση URL του API του έργου, ώστε να μπορείτε να εκτελέσετε την εφαρμογή τοπικά. Εάν παραλειφθεί η παράμετρος κατασκευή, το προεπιλεγμένο τελικό σημείο είναι η διεύθυνση URL που δημιούργησε τον κωδικό από.

7. Τάξη σας προγράμματος-πελάτη θα δημιουργηθούν με διαφορετικό όνομα με βάση το όνομα εφαρμογής API; Αλλάξτε τον κώδικα στο *Controllers\ToDoListController.cs* έτσι ώστε να ταιριάζει με το όνομα του τύπου, τι έχει δημιουργηθεί στο έργο σας. Για παράδειγμα, εάν ονομάζεται σας ToDoListDataAPI071316 εφαρμογή API, θα μπορείτε να αλλάξετε αυτόν τον κωδικό:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

σε αυτό:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>Δημιουργία εφαρμογής API για τη φιλοξενία στο μεσαίο επίπεδο

Προηγούμενη μπορείτε να [δημιουργήσει την εφαρμογή API σειρά δεδομένων και ανάπτυξη κώδικα σε αυτήν](#createapiapp).  Τώρα μπορείτε να ακολουθήσετε την ίδια διαδικασία για την εφαρμογή API Μεσαία σειρά.

1. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο μεσαίο επίπεδο ToDoListAPI του project (όχι η σειρά δεδομένων ToDoListDataAPI) και, στη συνέχεια, κάντε κλικ στο κουμπί **Δημοσίευση**.

    ![Κάντε κλικ στην επιλογή δημοσίευση στο Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

2.  Στην καρτέλα **προφίλ** του οδηγού **Δημοσίευση Web** , κάντε κλικ στην επιλογή **Microsoft Azure εφαρμογής υπηρεσίας**.

3. Στο παράθυρο διαλόγου **Εφαρμογής υπηρεσίας** , κάντε κλικ στην επιλογή **Δημιουργία**.

4. Στην καρτέλα **κεντρικού υπολογιστή** , στο παράθυρο διαλόγου **Δημιουργία εφαρμογής υπηρεσίας** , αποδεχθείτε το προεπιλεγμένο **Όνομα εφαρμογής API** ή πληκτρολογήστε ένα μοναδικό όνομα στον τομέα *azurewebsites.net* .

5. Επιλέξτε το Azure **συνδρομή** που χρησιμοποιείτε.

6. Στο την αναπτυσσόμενη λίστα **Ομάδα πόρων** , επιλέξτε την ίδια ομάδα των πόρων που δημιουργήσατε νωρίτερα.

7. Στο την αναπτυσσόμενη λίστα **Σχέδιο παροχής υπηρεσιών εφαρμογής** , επιλέξτε το ίδιο πρόγραμμα που δημιουργήσατε νωρίτερα. Θα προεπιλεγμένο σε αυτήν την τιμή.

8. Κάντε κλικ στην επιλογή **Δημιουργία**.

    Visual Studio δημιουργεί την εφαρμογή API, δημιουργεί ένα προφίλ δημοσίευση για αυτό και εμφανίζει τη **σύνδεση** βήμα του οδηγού **Δημοσίευσης Web** .

9.  Στο βήμα του οδηγού **Δημοσίευση Web** **σύνδεσης** , κάντε κλικ στο κουμπί **Δημοσίευση**.

    Visual Studio αναπτύσσει το έργο ToDoListAPI νέα εφαρμογή API και ανοίγει ένα πρόγραμμα περιήγησης στη διεύθυνση URL της εφαρμογής API. Εμφανίζεται η σελίδα "δημιουργήθηκε με επιτυχία".

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>Ρύθμιση παραμέτρων στο μεσαίο επίπεδο για να καλέσετε τη σειρά δεδομένων

Αν η εφαρμογή μεσαίας API καλέσει τώρα, αυτό θα προσπαθήστε να καλέσετε τη σειρά δεδομένων χρησιμοποιώντας τη διεύθυνση URL του τοπικού κεντρικού υπολογιστή που βρίσκεται ακόμη στο αρχείο Web.config. Σε αυτήν την ενότητα, μπορείτε να εισαγάγετε τη διεύθυνση URL εφαρμογής API δεδομένων επιπέδων σε μια ρύθμιση περιβάλλοντος στην εφαρμογή API Μεσαία σειρά. Όταν ο κώδικας στην εφαρμογή μεσαίας API ανακτά τη ρύθμιση της διεύθυνσης URL σειρά δεδομένων, η ρύθμιση περιβάλλον θα αντικαταστήσει τι υπάρχει στο αρχείο Web.config.

1. Μεταβείτε στην [πύλη του Azure](https://portal.azure.com/)και, στη συνέχεια, αναζητήστε την **Εφαρμογή API** blade για την εφαρμογή API που δημιουργήσατε για τη φιλοξενία του έργου TodoListAPI (Μεσαία σειρά).

2. Στο blade **Ρυθμίσεις** του API της εφαρμογής, κάντε κλικ στην επιλογή **Ρυθμίσεις εφαρμογής**.

3. Σε blade **Ρυθμίσεις εφαρμογής** του API της εφαρμογής, κάντε κύλιση προς τα κάτω στην ενότητα **Ρυθμίσεις εφαρμογής** και προσθέστε το ακόλουθο κλειδί και τιμή. Η τιμή θα είναι η διεύθυνση URL της πρώτης εφαρμογής API που δημοσιεύσατε σε αυτό το πρόγραμμα εκμάθησης.

  	| **Πλήκτρο** | toDoListDataAPIURL |
  	|---|---|
  	| **Τιμή** | όνομα εφαρμογής επίπεδο API δεδομένων https://{your} .azurewebsites .net |
  	| **Παράδειγμα** | https://todolistdataapi.azurewebsites.NET |

4. Κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Κάντε κλικ στην επιλογή Αποθήκευση για τις ρυθμίσεις εφαρμογής](./media/app-service-api-dotnet-get-started/asinportal.png)

    Όταν ο κώδικας που εκτελείται στο Azure, αυτή η τιμή τώρα θα αντικαταστήσει τη διεύθυνση URL του τοπικού κεντρικού υπολογιστή που βρίσκεται στο αρχείο Web.config.

## <a name="test"></a>Έλεγχος

1. Στο παράθυρο του προγράμματος περιήγησης, μεταβείτε στη διεύθυνση URL της νέας εφαρμογής API μεσαίας που μόλις δημιουργήσατε για ToDoListAPI. Μπορείτε να λάβετε εκεί, κάνοντας κλικ στη διεύθυνση URL στην κύρια blade του API της εφαρμογής στην πύλη.

2. Προσθήκη "swagger" στη διεύθυνση URL στη γραμμή διευθύνσεων του προγράμματος περιήγησης και, στη συνέχεια, πατήστε το πλήκτρο Enter. (Η διεύθυνση URL είναι `http://{apiappname}.azurewebsites.net/swagger`.)

    Το πρόγραμμα περιήγησης εμφανίζει το ίδιο Swagger περιβάλλον εργασίας Χρήστη που είδατε προηγουμένως για ToDoListDataAPI, αλλά τώρα `owner` δεν είναι απαιτούμενο πεδίο για τη λειτουργία Get, επειδή η εφαρμογή μεσαίας API αποστολής αυτήν την τιμή στην εφαρμογή API σειρά δεδομένων για εσάς. (Όταν κάνετε τα προγράμματα εκμάθησης για τον έλεγχο ταυτότητας, μεσαίο επίπεδο θα σας στείλει αναγνωριστικά πραγματική χρήστη το `owner` παραμέτρου; για τώρα το δύσκολο κωδικοποίηση αστερίσκο.)

3. Δοκιμάστε τη μέθοδο Get και τις άλλες μεθόδους για να επικυρώσετε ότι η εφαρμογή API μεσαίας καλεί με επιτυχία την εφαρμογή API σειρά δεδομένων.

    ![Swagger μέθοδος Get περιβάλλοντος εργασίας Χρήστη](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

Σε περίπτωση που αντιμετωπίσετε κάποιο πρόβλημα καθώς προχωράτε με αυτό το πρόγραμμα εκμάθησης εδώ θα βρείτε ορισμένες ιδέες αντιμετώπισης προβλημάτων:

* Βεβαιωθείτε ότι χρησιμοποιείτε την πιο πρόσφατη έκδοση του [Azure SDK για .NET](http://go.microsoft.com/fwlink/?linkid=518003).

* Δύο από τα ονόματα των έργων είναι παρόμοια (ToDoListAPI, ToDoListDataAPI). Εάν στοιχεία δεν εμφανίζονται όπως περιγράφεται στις οδηγίες όταν εργάζεστε με ένα έργο, βεβαιωθείτε ότι έχετε ανοίξει το σωστό έργο.

* Εάν είστε σε ένα εταιρικό δίκτυο και προσπαθείτε να αναπτύξετε σε Azure εφαρμογής υπηρεσίας μέσω του τείχους προστασίας, βεβαιωθείτε ότι οι θύρες 443 και 8172 είναι ανοιχτό για ανάπτυξη Web. Εάν δεν μπορείτε να ανοίξετε αυτές τις θύρες, μπορείτε να χρησιμοποιήσετε άλλες μεθόδους ανάπτυξης.  Ανατρέξτε στο θέμα [Ανάπτυξη την εφαρμογή Azure εφαρμογής υπηρεσίας](../app-service-web/web-sites-deploy.md).

* "Δρομολόγηση πρέπει να είναι μοναδικά" σφάλματα--μπορεί να λάβετε αυτές τις εάν κατά λάθος αναπτύξτε το έργο λάθος σε μια εφαρμογή API και, στη συνέχεια, να αναπτύξετε αργότερα σε αυτό το σωστό αρχείο. Για να διορθώσετε αυτό το πρόβλημα, επανάληψη ανάπτυξης του έργου σωστή εφαρμογή API και, στην καρτέλα " **Ρυθμίσεις** " του οδηγού **Δημοσίευσης Web** επιλέξτε **Κατάργηση πρόσθετων αρχείων στον προορισμό**.

Αφού έχετε την εφαρμογή σας ASP.NET API εκτελείται στο Azure εφαρμογής υπηρεσίας, μπορείτε να μάθετε περισσότερα σχετικά με τις δυνατότητες του Visual Studio που απλοποιήσετε αντιμετώπισης προβλημάτων. Για πληροφορίες σχετικά με την καταγραφή, ο απομακρυσμένος εντοπισμός σφαλμάτων και περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων Azure εφαρμογής υπηρεσίας εφαρμογών στο Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Επόμενα βήματα

Είδατε πώς μπορείτε να αναπτύξετε υπάρχοντα έργα Web API API εφαρμογές, δημιουργία κώδικα προγράμματος-πελάτη για τις εφαρμογές του API και κατανάλωση API εφαρμογές από προγράμματα-πελάτες του .NET. Το επόμενο πρόγραμμα εκμάθησης σε αυτήν τη σειρά που δείχνει πώς μπορείτε να [χρησιμοποιήσετε CORS για την εκμετάλλευση API εφαρμογές από προγράμματα-πελάτες του JavaScript](app-service-api-cors-consume-javascript.md).

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία κώδικα προγράμματος-πελάτη, ανατρέξτε στο θέμα του αποθετηρίου [Azure/AutoRest](https://github.com/azure/autorest) GitHub.com. Για βοήθεια σχετικά με ζητήματα κατά τη χρήση του προγράμματος-πελάτη που έχει δημιουργηθεί, ανοίξτε ένα [θέμα στο αποθετήριο AutoRest](https://github.com/azure/autorest/issues).

Εάν θέλετε να δημιουργήσετε νέα έργα εφαρμογής API από την αρχή, χρησιμοποιήστε το πρότυπο **Azure API εφαρμογής** .

![Πρότυπο εφαρμογής API στο Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

Το πρότυπο έργου **Azure API εφαρμογή** είναι ισοδύναμη με την επιλογή το **κενό** ASP.NET 4.5.2 πρότυπο, κάνοντας κλικ στο πλαίσιο ελέγχου για να προσθέσετε υποστήριξη Web API και την εγκατάσταση του πακέτου Swashbuckle NuGet. Επιπλέον, το πρότυπο προσθέτει ορισμένες κωδικό ρύθμισης παραμέτρων Swashbuckle που έχει σχεδιαστεί για να αποτρέψετε τη δημιουργία διπλότυπων Swagger λειτουργίας αναγνωριστικά. Μετά τη δημιουργία ενός έργου, η εφαρμογή API, μπορείτε να αναπτύξετε το σε μια εφαρμογή API τον ίδιο τρόπο που είδατε σε αυτό το πρόγραμμα εκμάθησης.