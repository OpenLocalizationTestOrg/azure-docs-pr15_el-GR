
<properties 
    pageTitle="Προσαρμογή των ορισμών που δημιουργούνται από το Swashbuckle API" 
    description="Μάθετε πώς μπορείτε να προσαρμόσετε ορισμών Swagger API που δημιουργούνται από Swashbuckle για μια εφαρμογή API στο Azure εφαρμογής υπηρεσίας." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/29/2016" 
    ms.author="rachelap"/>

# <a name="customize-swashbuckle-generated-api-definitions"></a>Προσαρμογή των ορισμών που δημιουργούνται από το Swashbuckle API 

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να προσαρμόσετε Swashbuckle χειρισμού συνηθισμένα σενάρια όπου μπορεί να θέλετε να αλλάξετε την προεπιλεγμένη συμπεριφορά:

* Swashbuckle δημιουργεί αναγνωριστικά διπλότυπων λειτουργία για τύποι υπερφόρτωσης του ελεγκτή μεθόδους
* Swashbuckle προϋποθέτει ότι η απόκριση ισχύει μόνο από μια μέθοδο είναι 200 HTTP (OK) 
 
## <a name="customize-operation-identifier-generation"></a>Προσαρμογή γενιάς αναγνωριστικό λειτουργίας

Swashbuckle δημιουργεί Swagger λειτουργία αναγνωριστικά συνδέοντας ελεγκτή όνομα και το όνομα της μεθόδου. Αυτό το μοτίβο δημιουργεί ένα πρόβλημα, όταν έχετε πολλές τύποι μια μέθοδο: Swashbuckle δημιουργεί αναγνωριστικά διπλότυπων λειτουργία, που είναι JSON Swagger δεν είναι έγκυρη.

Για παράδειγμα, ο ακόλουθος κώδικας ελεγκτή έχει ως αποτέλεσμα Swashbuckle για τη δημιουργία τρεις αναγνωριστικά λειτουργία Contact_Get.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Μπορείτε να επιλύσετε το πρόβλημα με μη αυτόματο τρόπο, δίνοντας τις μεθόδους μοναδικά ονόματα, όπως τα εξής για αυτό το παράδειγμα:

* Λήψη
* GetById
* GetPage

Η εναλλακτική λύση είναι να επεκτείνετε Swashbuckle για να δημιουργήσετε αυτόματα λειτουργία μοναδικών αναγνωριστικών.

Τα ακόλουθα βήματα δείχνουν πώς μπορείτε να προσαρμόσετε Swashbuckle χρησιμοποιώντας το αρχείο *SwaggerConfig.cs* που περιλαμβάνεται στο έργο από το πρότυπο έργου Visual Studio API εφαρμογές Preview.  Μπορείτε επίσης να προσαρμόσετε Swashbuckle σε ένα έργο Web API που ρυθμίζετε τις παραμέτρους για ανάπτυξη ως εφαρμογής API.

1. Δημιουργήστε μια προσαρμοσμένη `IOperationFilter` εφαρμογή 

    Το `IOperationFilter` περιβάλλοντος εργασίας παρέχει ένα σημείο επεκτασιμότητα του για Swashbuckle τους χρήστες που θέλετε να προσαρμόσετε διάφορες πτυχές της διαδικασίας Swagger μετα-δεδομένων. Ο ακόλουθος κώδικας περιγράφει μία μέθοδο της αλλαγής της συμπεριφοράς id-η δημιουργία της λειτουργίας. Ο κώδικας τοποθετεί τα ονόματα παραμέτρων το όνομα της λειτουργίας αναγνωριστικό.  

        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
        
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }

2. Στο αρχείο *App_Start\SwaggerConfig.cs* , καλέστε την `OperationFilter` μέθοδο για να προκαλέσει Swashbuckle για να χρησιμοποιήσετε τη νέα `IOperationFilter` υλοποίηση.

        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();

    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)

    Το αρχείο *SwaggerConfig.cs* που ενσωματώνεται από το πακέτο Swashbuckle NuGet περιέχει πολλά παραδείγματα σχόλιο εκτός των σημείων επεκτασιμότητα του. Τα πρόσθετα σχόλια δεν εμφανίζονται εδώ. 

    Αφού κάνετε αυτήν την αλλαγή, τον `IOperationFilter` εφαρμογή χρησιμοποιείται και έχει ως αποτέλεσμα λειτουργία μοναδικών αναγνωριστικών σε δημιουργηθούν.
 
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>
    
## <a name="allow-response-codes-other-than-200"></a>Να επιτρέπεται απόκριση κώδικες εκτός από 200

Από προεπιλογή, Swashbuckle προϋποθέτει ότι μια απόκριση 200 HTTP (OK) είναι η νόμιμη απόκριση *μόνο* από μια μέθοδο Web API. Σε ορισμένες περιπτώσεις, ίσως θέλετε να επιστρέψετε άλλους κωδικούς απάντηση χωρίς να προκαλεί το πρόγραμμα-πελάτη για να βελτιώσετε μια εξαίρεση.  Για παράδειγμα, ο ακόλουθος κώδικας Web API περιγράφει ένα σενάριο όπου μπορεί να θέλετε το πρόγραμμα-πελάτη για να αποδεχτείτε μια 200 ή μια 404 ως έγκυρες απαντήσεων.

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

Σε αυτό το σενάριο, το Swagger που Swashbuckle δημιουργεί από προεπιλογή καθορίζει μόνο μία νόμιμους κωδικό κατάστασης HTTP, HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Επειδή το Visual Studio χρησιμοποιεί τον ορισμό Swagger API για τη δημιουργία κώδικα για το πρόγραμμα-πελάτη, δημιουργεί κώδικα προγράμματος-πελάτη που εμφανίζει μια εξαίρεση για κάθε απάντηση εκτός από ένα 200 HTTP. Ο παρακάτω κώδικας είναι από ένα C# πρόγραμμα-πελάτη που δημιουργούνται για αυτήν τη μέθοδο API Web του δείγματος.

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

Δύο τρόποι για προσαρμογή στη λίστα των αναμενόμενων κωδικών απόκρισης HTTP που δημιουργεί, παρέχει Swashbuckle χρησιμοποιώντας σχόλια XML ή το `SwaggerResponse` χαρακτηριστικό. Το χαρακτηριστικό είναι πιο εύκολη, αλλά μόνο είναι διαθέσιμες στο Swashbuckle 5.1.5 ή νεότερη έκδοση. Το πρότυπο API εφαρμογές preview νέο έργο στο Visual Studio 2013 περιλαμβάνει Swashbuckle έκδοση 5.0.0, ώστε να εάν χρησιμοποιείται το πρότυπο και δεν θέλετε να ενημερώσετε Swashbuckle, η μοναδική επιλογή είναι να χρησιμοποιήσετε τα σχόλια XML. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Προσαρμόστε τους κωδικούς Αναμένεται απάντηση με σχόλια XML

Χρησιμοποιήστε αυτήν τη μέθοδο για να καθορίσετε κωδικούς απάντηση, εάν η έκδοση Swashbuckle είναι προγενέστερη 5.1.5.

1. Πρώτα, προσθέστε σχόλια τεκμηρίωσης XML επάνω από τις μεθόδους που θέλετε να καθορίσετε κωδικούς απόκρισης HTTP για. Λήψη το δείγμα Web API ενέργεια εμφανίζεται παραπάνω και εφαρμόζοντας την τεκμηρίωση XML σε αυτό θα έχει ως αποτέλεσμα στον κώδικα, όπως το παρακάτω παράδειγμα. 

        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
        
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
        
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

1. Προσθήκη οδηγίες στο αρχείο *SwaggerConfig.cs* για να κατευθύνετε Swashbuckle να χρησιμοποιεί το XML αρχείο τεκμηρίωσης.

    * Ανοίξτε *SwaggerConfig.cs* και δημιουργήστε μια μέθοδο στην τάξη *SwaggerConfig* για να καθορίσετε τη διαδρομή προς το αρχείο XML τεκμηρίωση. 

            private static string GetXmlCommentsPath()
            {
                return string.Format(@"{0}\XmlComments.xml", 
                    System.AppDomain.CurrentDomain.BaseDirectory);
            }

    * Κάντε κύλιση προς τα κάτω στο αρχείο *SwaggerConfig.cs* μέχρι να δείτε το σχόλιο ανάληψης γραμμής του κώδικα που μοιάζουν με το στιγμιότυπο κάτω από την οθόνη. 

        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
    
    * Καταργήστε τα σχόλια από τη γραμμή για να ενεργοποιήσετε τα σχόλια XML επεξεργασίας κατά τη διάρκεια δημιουργίας Swagger. 
    
        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
    
1. Για να δημιουργήσετε το αρχείο τεκμηρίωσης XML, μεταβείτε στις ιδιότητες του έργου και ενεργοποίηση του αρχείου XML τεκμηρίωση, όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

Αφού εκτελέσετε αυτά τα βήματα, το JSON Swagger που δημιουργούνται με Swashbuckle θα αντανακλούν τους κωδικούς απόκρισης HTTP που έχετε καθορίσει στα σχόλια XML. Το παρακάτω στιγμιότυπο οθόνης δείχνει το νέο φορτίου JSON. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Κατά τη χρήση του Visual Studio για να δημιουργήσετε ξανά τον κωδικό προγράμματος-πελάτη για το REST API, ο κώδικας C# αποδέχεται τόσο το κουμπί OK HTTP και δεν βρέθηκε κωδικοί κατάστασης δίχως ανύψωση εξαίρεση, επιτρέποντάς σας καταναλώνει κώδικα για λήψη αποφάσεων σχετικά με τον τρόπο χειρισμού η επιστροφή του μια τιμή null εγγραφή επαφής. 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

Σε [αυτό το αποθετήριο δεδομένων GitHub](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse), μπορείτε να βρείτε τον κωδικό για αυτήν την επίδειξη. Μαζί με το API Web έργου που έχει επισημανθεί προς τα επάνω με σχόλια τεκμηρίωσης XML είναι ένα έργο εφαρμογής κονσόλας που περιέχει μια που έχει δημιουργηθεί προγράμματος-πελάτη για αυτό το API. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Προσαρμογή κώδικες Αναμένεται απάντηση χρησιμοποιώντας το χαρακτηριστικό SwaggerResponse

Το χαρακτηριστικό [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) είναι διαθέσιμα στο Swashbuckle 5.1.5 και νεότερες εκδόσεις. Σε περίπτωση που έχετε μια προηγούμενη έκδοση στο έργο σας, αυτή η ενότητα ξεκινά εξηγώντας πώς μπορείτε να ενημερώσετε το πακέτο Swashbuckle NuGet ώστε να μπορείτε να χρησιμοποιήσετε αυτό το χαρακτηριστικό.

1. Στην **Εξερεύνηση λύσεων**, κάντε δεξιό κλικ στο έργο σας Web API και κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)

1. Κάντε κλικ στο κουμπί *Ενημέρωση* δίπλα του πακέτου NuGet *Swashbuckle* . 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)

1. Προσθέστε τα χαρακτηριστικά *SwaggerResponse* τις μεθόδους ενέργεια Web API για την οποία θέλετε να καθορίσετε έγκυρους κωδικούς απόκρισης HTTP. 

        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();

            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

2. Προσθήκη μιας `using` δήλωση για το χαρακτηριστικό χώρου ονομάτων:

        using Swashbuckle.Swagger.Annotations;
        
1. Μεταβείτε στη διεύθυνση URL */swagger/docs/v1* του έργου σας και τους διάφορους κωδικούς απόκρισης HTTP θα είναι ορατός στη το JSON Swagger. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Σε [αυτό το αποθετήριο δεδομένων GitHub](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes), μπορείτε να βρείτε τον κωδικό για αυτήν την επίδειξη. Μαζί με το API Web έργου από διακοσμημένου με το χαρακτηριστικό *SwaggerResponse* είναι ένα έργο εφαρμογής κονσόλας που περιέχει μια που έχει δημιουργηθεί προγράμματος-πελάτη για αυτό το API. 

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο έχει δείξει πώς μπορείτε να προσαρμόσετε τον τρόπο Swashbuckle δημιουργεί αναγνωριστικά λειτουργίας και τους κωδικούς έγκυρη απάντηση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Swashbuckle σε GitHub](https://github.com/domaindrivendev/Swashbuckle).
 
