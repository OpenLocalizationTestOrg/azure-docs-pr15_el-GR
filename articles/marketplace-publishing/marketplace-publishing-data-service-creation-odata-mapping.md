<properties
   pageTitle="Οδηγός για τη δημιουργία μιας υπηρεσίας δεδομένων για το Marketplace | Microsoft Azure"
   description="Λεπτομερείς οδηγίες σχετικά με τη δημιουργία, πιστοποίηση και την ανάπτυξη μιας υπηρεσίας δεδομένων για αγορά στο το Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Αντιστοίχιση μιας υπάρχουσας υπηρεσίας web με OData έως CSDL

>[AZURE.IMPORTANT] **Αυτήν τη στιγμή δεν είναι πλέον προσθήκης λογαριασμών οποιαδήποτε νέα εκδότες υπηρεσία δεδομένων. Δεν θα λάβετε εγκριθεί νέα dataservices για τη λίστα.** Εάν έχετε μια εφαρμογή επιχειρήσεις ΑΔΑ που θέλετε να δημοσιεύσετε στο AppSource μπορείτε να βρείτε περισσότερες πληροφορίες [εδώ](https://appsource.microsoft.com/partners). Εάν έχετε μια εφαρμογές IaaS ή προγραμματιστής υπηρεσία που θέλετε να δημοσιεύσετε στο Azure Marketplace, μπορείτε να βρείτε περισσότερες πληροφορίες [εδώ](https://azure.microsoft.com/marketplace/programs/certified/).

Σε αυτό το άρθρο παρέχει μια επισκόπηση σχετικά με τη χρήση μιας CSDL για να αντιστοιχίσετε μια υπάρχουσα υπηρεσία σε μια υπηρεσία συμβατή με OData. Εξηγεί τον τρόπο για να δημιουργήσετε το έγγραφο αντιστοίχισης (CSDL) που μετασχηματισμών εισαγωγής αίτησης από το πρόγραμμα-πελάτη μέσω μιας κλήσης υπηρεσίας και το αποτέλεσμα (δεδομένα) πίσω στον υπολογιστή-πελάτη μέσω OData συμβατά τροφοδοσία. Της Microsoft Azure Marketplace εκθέτει υπηρεσίες οι τελικοί χρήστες χρησιμοποιώντας το πρωτόκολλο OData. Υπηρεσίες που εκτίθενται από υπηρεσίες παροχής περιεχομένου (δεδομένων κάτοχοι) εκτίθενται σε διάφορες μορφές, όπως το ΥΠΌΛΟΙΠΟ, SOAP, κ.λπ.

## <a name="what-is-a-csdl-and-its-structure"></a>Τι είναι μια CSDL και τη δομή του;
Μια CSDL (εννοιολογική σχήματος Definition Language) είναι μια προδιαγραφή καθορίζει τον τρόπο για να περιγράψετε υπηρεσίας web ή βάσης δεδομένων της υπηρεσίας στο κοινό φρασεολογία XML με το Azure Marketplace.

Απλή Επισκόπηση της το **αίτηση ροής:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

Η **ροή δεδομένων** είναι προς την αντίθετη κατεύθυνση:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Σχήμα 1** διαγραμμάτων πώς ένας υπολογιστής-πελάτης θα λήψη δεδομένων από μια υπηρεσία παροχής περιεχομένου (υπηρεσία) από το Azure Marketplace.  Το CSDL χρησιμοποιείται από το στοιχείο αντιστοίχισης/μετασχηματισμό για να διαχειριστεί την αίτηση και μεταβίβαση δεδομένων μεταξύ της υπηρεσίας παροχής περιεχομένου υπηρεσίες και ο υπολογιστής-πελάτης ζητά.

*Εικόνα 1: Λεπτομερείς ροής από το πρόγραμμα-πελάτης ζητά να υπηρεσία παροχής περιεχομένου μέσω του Azure Marketplace*

  ![σχέδιο](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Για το φόντο σε Atom, Atom Pub και το πρωτόκολλο OData κατά την οποία δημιουργείτε τις επεκτάσεις Azure Marketplace, πρέπει να ελέγξετε: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Απόσπασμα από σημείο άνωθεν σύνδεση:     *"ο σκοπός του πρωτοκόλλου Άνοιγμα δεδομένων (στο εξής αναφέρεται ως OData) είναι να δώσετε ένα πρωτόκολλο που βασίζεται στο ΥΠΌΛΟΙΠΟ για λειτουργίες CRUD στυλ (δημιουργία, ανάγνωση, ενημέρωση και διαγραφή) σε σχέση με τους πόρους που εκτίθεται ως υπηρεσίες δεδομένων. "Υπηρεσία δεδομένων" είναι ένα τελικό σημείο όπου δεν υπάρχει δεδομένων που εκτίθεται από μία ή περισσότερες "Συλλογές" κάθε με κανέναν ή περισσότερους "εγγραφές", οι οποίες περιλαμβάνουν πληκτρολογήσατε ζεύγη τιμών με το όνομα. OData δημοσιεύεται από τη Microsoft στην περιοχή προτύπων OASIS (εταιρεία για την προαγωγή δομημένες πληροφορίες προτύπων), ώστε όλα τα άτομα που θέλει να να δημιουργήσουμε διακομιστές, πελάτες ή εργαλεία χωρίς δικαιώματα ή περιορισμούς."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Τρεις βασικές που πρέπει να καθοριστούν από το CSDL είναι οι εξής:

- Το **τελικό σημείο** του την υπηρεσία παροχής το Web διεύθυνση (URI) της υπηρεσίας
- Τις **παραμέτρους δεδομένων** που διαβιβάζεται ως είσοδο στην υπηρεσία παροχής που στέλνεται οι ορισμοί των παραμέτρων της υπηρεσίας παροχής περιεχομένου υπηρεσία προς τα κάτω τον τύπο δεδομένων.
- Το **σχήμα** των δεδομένων που επιστρέφονται με την υπηρεσία ζητά τη διάταξη των δεδομένων παραδίδονται από την υπηρεσία της υπηρεσίας παροχής περιεχομένου, όπως κοντέινερ, συλλογές/πίνακες, μεταβλητές/στήλες και τύπους δεδομένων.

Το παρακάτω διάγραμμα παρουσιάζει μια επισκόπηση της ροής της από όπου το πρόγραμμα-πελάτη εισάγει τη δήλωση OData (κλήση στην υπηρεσία web της υπηρεσίας παροχής περιεχομένου) για να επιστρέψετε γρήγορα αποτελέσματα δεδομένων.

  ![σχέδιο](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>Τα βήματα:

1. Πρόγραμμα-πελάτης στέλνει την αίτηση μέσω κλήσης υπηρεσίας ολοκλήρωσης με παραμέτρους εισόδου που ορίζονται σε XML με το Azure Marketplace
2. CSDL χρησιμοποιείται για την επικύρωση κλήσης στην υπηρεσία.
    - Η μορφοποίηση υπηρεσίας κλήσεων, στη συνέχεια, αποστέλλεται η υπηρεσία παροχής περιεχομένου από το Azure Marketplace
3. Το Webservice εκτελεί και preforms η ενέργεια του ρήματος Http (δηλαδή ΈΧΕΤΕ) τα δεδομένα που επιστρέφονται με το Azure Marketplace όπου τα δεδομένα που ζητήθηκαν (εάν υπάρχουν) είναι εκθέτει σε μορφή XML στον υπολογιστή-πελάτη χρησιμοποιώντας την αντιστοίχιση που ορίζονται στο το CSDL.
4. Το πρόγραμμα-πελάτη αποστέλλονται τα δεδομένα (εάν υπάρχουν) σε μορφή XML ή JSON

## <a name="definitions"></a>Ορισμοί

### <a name="odata-atom-pub"></a>Pub OData ATOM

Ορίστε μια επέκταση για να το pub ATOM όπου κάθε καταχώρηση αντιπροσωπεύει μία γραμμή του αποτελέσματος ενός. Το τμήμα του περιεχομένου της καταχώρησης είναι βελτιωμένη ώστε να περιέχει τις τιμές της γραμμής-ως ζεύγη τιμή του κλειδιού. Περισσότερες πληροφορίες βρίσκεται εδώ: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - γλώσσα ορισμού εννοιολογική σχήματος

Επιτρέπει τον ορισμό συναρτήσεις (SPROCs) και οντοτήτων που εκτίθενται μέσω μιας βάσης δεδομένων. Περισσότερες πληροφορίες εδώ: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [AZURE.TIP] Κάντε κλικ στην αναπτυσσόμενη λίστα **άλλες εκδόσεις** και επιλέξτε μια έκδοση, εάν δεν βλέπετε το άρθρο.

### <a name="edm---entry-data-model"></a>EDM - μοντέλο καταχώρησης δεδομένων
- Επισκόπηση: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink] [OverviewLink]: http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx
- Προεπισκόπηση: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink] [PreviewLink]: http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx
- Τύποι δεδομένων: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink] [DataTypesLink]: http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx

Πίσω στο ακόλουθο ροής λεπτομερείς αριστερά προς τα δεξιά από το σημείο όπου το πρόγραμμα-πελάτη εισάγει τη δήλωση OData (κλήση στην υπηρεσία web της υπηρεσίας παροχής περιεχομένου) για να γρήγορα αποτελέσματα δεδομένων:

  ![σχέδιο](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)


## <a name="csdl-basics"></a>Βασικά στοιχεία CSDL

Μια CSDL (εννοιολογική σχήματος Definition Language) είναι μια προδιαγραφή καθορίζει τον τρόπο για να περιγράψετε υπηρεσίας web ή βάσης δεδομένων της υπηρεσίας στο κοινό φρασεολογία XML με το Azure Marketplace. Περιγράφει την κρίσιμη CSDL τεμάχια που **κάνει τη μεταβίβαση δεδομένων από την προέλευση δεδομένων με το Azure Marketplace πιθανές.** Κύριο τμήματα περιγράφονται εδώ:

- Πληροφορίες για τη διασύνδεση που περιγράφει όλες τις συναρτήσεις διαθέσιμη στο κοινό (FunctionImport κόμβο)
- Πληροφορίες τύπου δεδομένων για όλα τα requests(input) μήνυμα και μήνυμα responses(outputs) (κόμβοι EntityContainer/EntitySet/τύπος οντότητας)
- Πληροφορίες σχετικά με το πρωτόκολλο μεταφοράς να σύνδεσης χρησιμοποιούνται (κεφαλίδα κόμβο)
- Πληροφορίες για τη διεύθυνση για τον εντοπισμό της συγκεκριμένης υπηρεσίας (BaseURI χαρακτηριστικό)

Με λίγα λόγια, η CSDL αντιπροσωπεύει ένα συμβόλαιο πλατφόρμα και γλώσσας-ανεξάρτητο μεταξύ αιτούντα υπηρεσίας και η υπηρεσία παροχής. Χρησιμοποιώντας το CSDL, ένα πρόγραμμα-πελάτη να εντοπίσετε μια υπηρεσία/βάση δεδομένων της υπηρεσίας web και να καλέσετε οποιαδήποτε από τις συναρτήσεις διαθέσιμη στο κοινό.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Συσχέτιση CSDL μια βάση δεδομένων ή σε μια συλλογή
**Η προδιαγραφή CSDL**

CSDL είναι γραμματικός έλεγχος XML για που περιγράφει μια υπηρεσία web. Η προδιαγραφή ίδια χωρίζεται σε κύρια στοιχεία του 4: EntitySet, FunctionImport; Χώρος ονομάτων και τον τύπο οντότητας.

Για να διευκολύνετε αυτή αφαίρεσης για να κατανοήσετε σάς επιτρέπει να συσχετίσετε ένα CSDL σε έναν πίνακα.

Να θυμάστε;

  CSDL αντιπροσωπεύει ένα συμβόλαιο πλατφόρμα και γλώσσας-ανεξάρτητο μεταξύ **αίτησης υπηρεσίας** και η **υπηρεσία παροχής**. Χρήση CSDL, ένα **πρόγραμμα-πελάτη** μπορούν να εντοπίστε μια **υπηρεσία/βάση δεδομένων της υπηρεσίας web** και κλήση οποιαδήποτε από τις διαθέσιμες στο κοινό **συναρτήσεις.**

Για μια υπηρεσία δεδομένων μπορεί να θεωρηθεί τα τέσσερα μέρη ενός CSDL όσον αφορά μια βάση δεδομένων, πίνακα, στήλη και τη διαδικασία αποθήκευσης.

Σχετικά με αυτές τις ως εξής για μια υπηρεσία δεδομένων:

- EntityContainer ~ = βάση δεδομένων
- EntitySet ~ = πίνακα
- Τύπος οντότητας ~ = στηλών
- FunctionImport ~ = αποθηκευμένη διαδικασία

**Επιτρέπεται ρήματα HTTP**
- GET-επιστρέφει τις τιμές από την db (επιστρέφει μια συλλογή)
- ΔΗΜΟΣΊΕΥΣΗ – χρησιμοποιείται για τη μεταβίβαση δεδομένων σε και προαιρετικά τιμές επιστροφής από το db (Δημιουργία νέας καταχώρησης στη συλλογή, επιστροφής αναγνωριστικό/URI)
- ΔΙΑΓΡΑΦΉ – διαγράφει τα δεδομένα από το DB (διαγράφει μια συλλογή)
- ΤΟΠΟΘΈΤΗΣΗ – ενημέρωση δεδομένων σε ένα DB (αντικατάσταση μιας συλλογής ή να δημιουργήσετε μία)

## <a name="metadatamapping-document"></a>Έγγραφο μετα-δεδομένων/αντιστοίχισης

Το έγγραφο μετα-δεδομένων/αντιστοίχισης χρησιμοποιείται για να αντιστοιχίσετε μια υπηρεσία παροχής περιεχομένου του υπάρχουσες υπηρεσίες web, έτσι ώστε να είναι εκτίθεται ως υπηρεσία web OData από το σύστημα Azure Marketplace. Βασίζεται σε CSDL και υλοποιεί μερικά επεκτάσεις CSDL σύμφωνα με τις ανάγκες την υπόλοιπη με βάση τις υπηρεσίες web που εκτίθεται μέσω του Azure Marketplace. Οι επεκτάσεις βρίσκονται στο χώρο ονομάτων [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) .

Ακολουθεί ένα παράδειγμα του CSDL: (αντιγράψετε και να επικολλήσετε το παρακάτω παράδειγμα CSDL σε ένα πρόγραμμα επεξεργασίας XML και αλλαγή ώστε να ταιριάζει με την υπηρεσία.  Στη συνέχεια, επικολλήστε σε CSDL αντιστοίχιση στην καρτέλα DataService κατά τη δημιουργία της υπηρεσίας στην [Πύλη δημοσιεύσεων Azure Marketplace](https://publish.windowsazure.com)).

**Όρους:** Σχετικά με τους όρους CSDL τους όρους [Πύλη δημοσιεύσεων](https://publish.windowsazure.com) περιβάλλοντος εργασίας Χρήστη (PPUI).
- Προσφορά "Τίτλος" στο το PPUI σχετίζεται με MyWebOffer
- MyCompany στο το PPUI σχετίζεται με **Τον Publisher εμφανιζόμενο όνομα** στο [Κέντρο για προγραμματιστές του Microsoft](http://dev.windows.com/registration?accountprogram=azure) περιβάλλοντος εργασίας Χρήστη
- Το API σχετίζεται με ένα Web ή την υπηρεσία δεδομένων (ένα πρόγραμμα του PPUI)

**Ιεραρχία:** 
 μια εταιρεία (υπηρεσία παροχής περιεχομένου) κατέχει προσφορές που έχουν Plan(s), δηλαδή service(s), ποια γραμμή προς τα επάνω με API.

### <a name="webservice-csdl-example"></a>Παράδειγμα WebService CSDL

Συνδέεται με μια υπηρεσία που εκτίθεται από ένα τελικό σημείο εφαρμογής web (όπως μια εφαρμογή C#)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID" d:IsPrimaryKey="True" Type="Int32"  Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount" Type="Double"   Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"   Type="String"   Nullable="false" d:Map="./City"/>
        <Property Name="State"  Type="String"   Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime" Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [AZURE.TIP] Δείτε περισσότερα παραδείγματα υπηρεσίας Web CSDL στο άρθρο [παραδείγματα αντιστοίχιση μια υπάρχουσα υπηρεσία web με OData έως CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)

###<a name="dataservice-csdl-example"></a>Παράδειγμα DataService CSDL

Συνδέεται με μια υπηρεσία που εκτίθεται έναν πίνακα βάσης δεδομένων ή την προβολή, όπως ένα τελικό σημείο παρακάτω παράδειγμα εμφανίζει δύο API για βάση δεδομένων βάσει CSDL API (να χρησιμοποιήσετε προβολές αντί για πίνακες).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Δείτε επίσης
- Εάν σας ενδιαφέρει εκμάθησης και κατανόηση τους κόμβους συγκεκριμένες και τις παραμέτρους, διαβάστε αυτό το άρθρο [Δεδομένων υπηρεσίας OData αντιστοίχιση κόμβους](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) για ορισμούς και εξηγήσεις, παραδείγματα και χρήση πεζών-κεφαλαίων περιβάλλοντος.
- Εάν σας ενδιαφέρει αναθεώρηση παραδείγματα, διαβάστε αυτό το άρθρο [Παραδείγματα αντιστοίχιση OData υπηρεσίας δεδομένων](marketplace-publishing-data-service-creation-odata-mapping-examples.md) για να δείτε δείγματα κώδικα και κατανόηση σύνταξη κώδικα και περιβάλλοντος.
- Για να επιστρέψετε στην προκαθορισμένη διαδρομή για τη δημοσίευση δεδομένων υπηρεσίας για το Azure Marketplace, διαβάστε αυτό το άρθρο [Οδηγού δημοσίευσης υπηρεσίας δεδομένων](marketplace-publishing-data-service-creation.md).
