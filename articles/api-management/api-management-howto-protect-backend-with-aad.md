<properties
    pageTitle="Πώς να προστατεύσετε έναν υπολογιστή στο παρασκήνιο Web API με το Azure Active Directory και το API διαχείρισης"
    description="Μάθετε πώς να προστατεύσετε έναν υπολογιστή στο παρασκήνιο Web API με το Azure Active Directory και το API διαχείρισης." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Πώς να προστατεύσετε έναν υπολογιστή στο παρασκήνιο Web API με το Azure Active Directory και το API διαχείρισης

Το παρακάτω βίντεο σας δείχνει πώς μπορείτε να δημιουργήσετε έναν υπολογιστή στο παρασκήνιο Web API και προστασία χρησιμοποιώντας πρωτόκολλο διακριτικό 2.0 με το Azure Active Directory και το API διαχείρισης.  Σε αυτό το άρθρο παρέχει μια επισκόπηση και πρόσθετες πληροφορίες για τα βήματα του βίντεο. Αυτό το βίντεο 24 λεπτά δείχνει πώς να:

-   Δημιουργήστε έναν υπολογιστή στο παρασκήνιο Web API και να διασφαλίσει με AAD - ξεκινώντας από το 1:30
-   Εισαγάγετε το API στο API διαχείρισης - ξεκινώντας από 7:10
-   Ρύθμιση παραμέτρων την πύλη για προγραμματιστές για να καλέσετε το API - ξεκινώντας από 9:09
-   Ρύθμιση παραμέτρων μιας εφαρμογής επιφάνειας εργασίας για να καλέσετε το API - ξεκινώντας από 18:08
-   Ρύθμιση παραμέτρων μιας πολιτικής επικύρωσης JWT για να προ-εξουσιοδοτήσετε αιτήσεις - ξεκινώντας από το 20:47

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>Δημιουργία ενός καταλόγου Azure AD

Για την ασφάλιση του API Web αντίγραφα χρησιμοποιώντας Azure Active Directory, πρέπει πρώτα να έχετε ένα ένα μισθωτή του AAD. Σε αυτό το βίντεο χρησιμοποιείται ένας μισθωτής που ονομάζεται **APIMDemo** . Για να δημιουργήσετε ένα μισθωτή AAD, είσοδος στην [Πύλη κλασική Azure](https://manage.windowsazure.com) και κάντε κλικ στην επιλογή **Δημιουργία**->**Εφαρμογή υπηρεσιών**->**Υπηρεσίας καταλόγου Active Directory**->**καταλόγου**->**Δημιουργία προσαρμοσμένης**. 

![Καταλόγου Azure Active Directory][api-management-create-aad-menu]

Σε αυτό το παράδειγμα έναν κατάλογο με όνομα **APIMDemo** δημιουργείται με έναν προεπιλεγμένο τομέα με το όνομα **DemoAPIM.onmicrosoft.com**. Αυτός ο κατάλογος χρησιμοποιείται σε όλο το βίντεο.

![Καταλόγου Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Δημιουργία μιας υπηρεσίας Web API ασφαλές από το Azure Active Directory

Σε αυτό το βήμα, δημιουργείται ένα παρασκηνίου Web API με χρήση του Visual Studio 2013. Αυτό το βήμα του βίντεο ξεκινά από 1:30. Για να δημιουργήσετε το API Web παρασκηνίου έργου στο Visual Studio, κάντε κλικ στο **αρχείο**->**Δημιουργία**->**έργου**, και επιλέξτε **Την εφαρμογή Web ASP.NET** από τη λίστα πρότυπα **Web** . Σε αυτό το βίντεο του έργου ονομάζεται **APIMAADDemo**. Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο. 

![Visual Studio][api-management-new-web-app]

Κάντε κλικ στην επιλογή **Web API** από την **επιλογή πρότυπο λίστας** για να δημιουργήσετε ένα έργο Web API. Για τη ρύθμιση παραμέτρων ελέγχου ταυτότητας καταλόγου Azure, κάντε κλικ στην επιλογή **Αλλαγή ελέγχου ταυτότητας**.

![Νέο έργο][api-management-new-project]

Κάντε κλικ στην επιλογή **Εταιρικοί λογαριασμοί**και καθορίστε τον **τομέα** του μισθωτή σας AAD. Σε αυτό το παράδειγμα, ο τομέας είναι **DemoAPIM.onmicrosoft.com**. Τον τομέα του καταλόγου σας μπορεί να ληφθεί από την καρτέλα **τομέων** του καταλόγου σας.

![Τομείς][api-management-aad-domains]

Πραγματοποιήστε τις επιθυμητές ρυθμίσεις στο παράθυρο διαλόγου **Αλλαγή ελέγχου ταυτότητας** και κάντε κλικ στο **κουμπί OK**.

![Αλλαγή ελέγχου ταυτότητας][api-management-change-authentication]

Όταν κάνετε κλικ στο **κουμπί OK** Visual Studio θα επιχειρήσει να καταχωρήσετε την εφαρμογή σας με τον κατάλογο Azure AD και μπορεί να σας ζητηθεί να εισέλθετε με Visual Studio. Συνδεθείτε χρησιμοποιώντας ένα λογαριασμό διαχειριστή για τον κατάλογο.

![Πραγματοποιήστε είσοδο στο Visual Studio][api-management-sign-in-vidual-studio]

Για να ρυθμίσετε τις παραμέτρους αυτό το έργο ως ένα Azure Web API, επιλέξτε το πλαίσιο για τον **κεντρικό υπολογιστή στο cloud** και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

![Νέο έργο][api-management-new-project-cloud]

Ίσως σας ζητηθεί να εισέλθετε στο Azure και, στη συνέχεια, μπορείτε να ρυθμίσετε τις παραμέτρους της εφαρμογής Web.

![Ρύθμιση παραμέτρων][api-management-configure-web-app]

Σε αυτό το παράδειγμα έχει καθοριστεί σε νέο **πρόγραμμα εφαρμογής υπηρεσίας** με το όνομα **APIMAADDemo** .

Κάντε κλικ στο **κουμπί OK** για να ρυθμίσετε τις παραμέτρους της εφαρμογής Web και δημιουργήστε το έργο.

## <a name="add-the-code-to-the-web-api-project"></a>Προσθήκη του κώδικα στο έργο Web API

Το επόμενο βήμα στο βίντεο που προσθέτει τον κώδικα στο έργο Web API. Αυτό το βήμα ξεκινά από 4:35.

Το API Web σε αυτό το παράδειγμα υλοποιεί μια βασική Αριθμομηχανή υπηρεσία χρησιμοποιώντας ένα μοντέλο και ελεγκτή. Για να προσθέσετε το μοντέλο για την υπηρεσία, κάντε δεξί κλικ σε **μοντέλα** σε **Εξερεύνηση λύσεων** και επιλέξτε **Προσθήκη**, **τάξης**. Δώστε ένα όνομα τάξη `CalcInput` και κάντε κλικ στην επιλογή **Προσθήκη**.

Προσθέστε τα ακόλουθα `using` δήλωση στο επάνω μέρος του `CalcInput.cs` αρχείου.

    using Newtonsoft.Json;

 Αντικαταστήστε κλάσης που δημιουργήθηκε με τον ακόλουθο κώδικα.

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

Κάντε δεξί κλικ **ελεγκτές** στην **Εξερεύνηση λύσεων** και επιλέξτε **Προσθήκη**->**ελεγκτή**. Επιλέξτε **Ελεγκτή 2 API Web - είναι κενή** και κάντε κλικ στην επιλογή **Προσθήκη**. Πληκτρολογήστε **CalcController** για το όνομα του ελεγκτή και κάντε κλικ στην επιλογή **Προσθήκη**.

![Προσθήκη ελεγκτή][api-management-add-controller]

Προσθέστε τα ακόλουθα `using` δήλωση στο επάνω μέρος του `CalcController.cs` αρχείου.

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

Αντικαταστήστε την κλάση ελεγκτή που δημιουργείται με τον ακόλουθο κώδικα. Αυτός ο κωδικός υλοποιεί το `Add`, `Subtract`, `Multiply`, και `Divide` λειτουργίες του βασικού API Αριθμομηχανής.

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

Πατήστε το πλήκτρο **F6** για να δημιουργήσετε και να επαληθεύσετε τη λύση.

## <a name="publish-the-project-to-azure"></a>Δημοσίευση έργου στο Azure

Σε αυτό το βήμα του Visual Studio για να Azure δημοσίευση έργου. Αυτό το βήμα του βίντεο ξεκινά από 5:45.

Για να δημοσιεύσετε το έργο Azure, κάντε δεξί κλικ στο έργο **APIMAADDemo** στο Visual Studio και επιλέξτε **Δημοσίευση**. Διατηρήστε τις προεπιλεγμένες ρυθμίσεις στο παράθυρο διαλόγου **Δημοσίευση Web** και κάντε κλικ στο κουμπί **Δημοσίευση**.

![Δημοσίευση Web][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Εκχώρηση δικαιωμάτων σε εφαρμογή της υπηρεσίας υποστήριξης της Azure AD

Δημιουργείται μια νέα εφαρμογή για την υπηρεσία υποστήριξης στον κατάλογό σας Azure AD ως μέρος της διαδικασίας τη ρύθμιση των παραμέτρων και δημοσίευσης του έργου σας Web API. Σε αυτό το βήμα του βίντεο, ξεκινώντας από 6:13, τα δικαιώματα εκχωρούνται στον υπολογιστή στο παρασκήνιο Web API.

![Εφαρμογή][api-management-aad-backend-app]

Κάντε κλικ στο όνομα της εφαρμογής για να ρυθμίσετε τα απαραίτητα δικαιώματα. Μεταβείτε στη σελίδα " **Ρύθμιση παραμέτρων** " και κάντε κύλιση προς τα κάτω στην ενότητα **δικαιώματα σε άλλες εφαρμογές** . Κάντε κλικ στην επιλογή **Δικαιώματα εφαρμογής** αναπτυσσόμενη λίστα δίπλα στο στοιχείο **Windows** **Azure Active Directory**, επιλέξτε το πλαίσιο ελέγχου για **Ανάγνωση καταλόγου δεδομένων**και κάντε κλικ στην επιλογή **Αποθήκευση**.

![Προσθήκη δικαιωμάτων][api-management-aad-add-permissions]

>[AZURE.NOTE] Εάν το **Windows** **Azure Active Directory** δεν παρατίθεται στην περιοχή δικαιώματα σε άλλες εφαρμογές, κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής** και προσθέστε το από τη λίστα.

Σημειώστε το **Αναγνωριστικό εφαρμογής διεύθυνση URI** για χρήση στο επόμενο βήμα όταν μια εφαρμογή του Azure AD έχει ρυθμιστεί για την πύλη διαχείρισης API για προγραμματιστές.

![Αναγνωριστικό εφαρμογής URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Εισαγάγετε το API Web στο API διαχείρισης

APIs ρυθμίζονται από την πύλη publisher API, το οποίο έχετε πρόσβαση μέσω της πύλης κλασική Azure. Για την επίτευξη πύλη του publisher, κάντε κλικ στην επιλογή **Διαχείριση** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API. Εάν δεν έχετε δημιουργήσει ακόμη μια παρουσία της υπηρεσίας διαχείρισης API, ανατρέξτε στο θέμα [Δημιουργία μια παρουσία της υπηρεσίας διαχείρισης API][] στην εκμάθηση [Διαχείριση το πρώτο API][] .

![Πύλη του Publisher][api-management-management-console]

Λειτουργίες μπορεί να [προσθέσει προς API με μη αυτόματο τρόπο](api-management-howto-add-operations.md)ή να μπορούν να εισαχθούν. Σε αυτό το βίντεο, λειτουργίες έχουν εισαχθεί σε μορφή Swagger ξεκινώντας από 6:40.

Δημιουργήστε ένα αρχείο με το όνομα `calcapi.json` με το παρακάτω περιεχόμενο και αποθηκεύστε το στον υπολογιστή σας. Βεβαιωθείτε ότι το `host` χαρακτηριστικών σημεία για το Web API παρασκηνίου. Σε αυτό το παράδειγμα `"host": "apimaaddemo.azurewebsites.net"` χρησιμοποιούνται.

{"swagger": "2.0", "πληροφορίες": {"Τίτλος": "Υπολογιστής", "Περιγραφή": "Arithmetics μέσω HTTP!", "έκδοση": "1.0"}, "host": "apimaaddemo.azurewebsites.net", "basePath": "/ api", "σχήματα": ["http"], "Διαδρομές": {"/ Προσθήκη; μια = {a} & b = {b}": {"Λήψη": {"Περιγραφή": "Απόκριση με το άθροισμα των δύο αριθμών.", "operationId": "Προσθήκη δύο ακέραιους αριθμούς", "Παράμετροι": [{"όνομα": "a", "σε": "ερωτήματος", "Περιγραφή": "πρώτη τελεστέος. Η προεπιλεγμένη τιμή είναι <code>51</code>. ","απαιτείται": την τιμή true,"Προεπιλογή":"51","απαρίθμησης": ["51"]}, {"όνομα":"b","σε":"ερωτήματος","Περιγραφή":"δεύτερο τελεστέου. Η προεπιλεγμένη τιμή είναι <code>49</code>. ","απαιτείται": την τιμή true,"Προεπιλογή":"49","απαρίθμησης": ["49"]}],"απαντήσεων": {}}}," / sub?a = {a} & b = {b} ": {"Λήψη": {"Περιγραφή":"Απόκριση με διαφορά μεταξύ δύο αριθμών.","operationId":"Αφαίρεσης δύο ακέραιοι","Παράμετροι": [{"όνομα":"a","σε":"ερωτήματος","Περιγραφή":"πρώτη τελεστέος. Η προεπιλεγμένη τιμή είναι <code>100</code>. ","απαιτείται": την τιμή true,"Προεπιλογή":"100","απαρίθμησης": ["100"]}, {"όνομα":"b","σε":"ερωτήματος","Περιγραφή":"δεύτερο τελεστέου. Η προεπιλεγμένη τιμή είναι <code>50</code>. ","απαιτείται": την τιμή true,"Προεπιλογή":"50","απαρίθμησης": ["50"]}],"απαντήσεων": {}}}," / div?a = {a} & b = {b} ": {"Λήψη": {"Περιγραφή":"Απόκριση με μια πηλίκο των δύο αριθμών.","operationId":"Διαίρεση δύο ακέραιοι","Παράμετροι": [{"όνομα":"a","σε":"ερωτήματος","Περιγραφή":"πρώτη τελεστέος. Η προεπιλεγμένη τιμή είναι <code>100</code>. ","απαιτείται": την τιμή true,"Προεπιλογή":"100","απαρίθμησης": ["100"]}, {"όνομα":"b","σε":"ερωτήματος","Περιγραφή":"δεύτερος τελεστέος. Η προεπιλεγμένη τιμή είναι <code>20</code>. ","απαιτείται": την τιμή true,"Προεπιλογή":"20","απαρίθμησης": ["20"]}],"απαντήσεων": {}}}," / mul?a = {a} & b = {b} ": {"Λήψη": {"Περιγραφή":"Απόκριση με ένα προϊόν του δύο αριθμών.","operationId":"Πολλαπλασιασμός δύο ακέραιοι","Παράμετροι": [{"όνομα":"a","σε":"ερωτήματος","Περιγραφή":"πρώτη τελεστέος. Η προεπιλεγμένη τιμή είναι <code>20</code>. ","απαιτείται": την τιμή true,"Προεπιλογή":"20","απαρίθμησης": ["20"]}, {"όνομα":"b","σε":"ερωτήματος","Περιγραφή":"δεύτερος τελεστέος. Η προεπιλεγμένη τιμή είναι <code>5</code>. ","απαιτείται": την τιμή true,"Προεπιλογή":"5","απαρίθμησης": ["5"]}],"απαντήσεων": {}}}}}

Για να εισαγάγετε την Αριθμομηχανή API, κάντε κλικ στην επιλογή **APIs** από το **API διαχείρισης** μενού στα αριστερά και, στη συνέχεια, κάντε κλικ στην επιλογή **Εισαγωγή API**.

![Κουμπί "Εισαγωγή API"][api-management-import-api]

Ακολουθήστε τα παρακάτω βήματα για να ρυθμίσετε την Αριθμομηχανή API.

1. Κάντε κλικ στην επιλογή **από αρχείο**, κάντε αναζήτηση για να το `calculator.json` αρχείο θα αποθηκευτεί και κάντε κλικ στο κουμπί επιλογής **Swagger** .
2. Πληκτρολογήστε **Υπολογισμός** στο πλαίσιο κειμένου **διεύθυνση URL API Web επίθημα** .
3. Κάντε κλικ στο πλαίσιο **προϊόντα (προαιρετικό)** και επιλέξτε **Starter**.
4. Κάντε κλικ στο κουμπί **Αποθήκευση** για να εισαγάγετε το API.

![Προσθήκη νέου API][api-management-import-new-api]

Μόλις εισαχθούν τα API, τη σελίδα σύνοψης για το API εμφανίζεται στην πύλη του publisher.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Κλήση του API προσπαθήσει από την πύλη για προγραμματιστές

Σε αυτό το σημείο, το API έχει εισαχθεί σε API διαχείρισης, αλλά δεν είναι ακόμη να ονομάζεται με επιτυχία από την πύλη για προγραμματιστές επειδή η υπηρεσία υποστήριξης προστατεύεται με έλεγχο ταυτότητας Azure AD. Αυτό είναι επίδειξη του βίντεο ξεκινώντας από 7:40 ακολουθώντας τα παρακάτω βήματα.

Κάντε κλικ στην **πύλη για προγραμματιστές** από την επάνω δεξιά πλευρά της την πύλη του publisher.

![Πύλη για προγραμματιστές][api-management-developer-portal-menu]

Κάντε κλικ στην επιλογή **APIs** και επιλέξτε την **Αριθμομηχανή** API.

![Πύλη για προγραμματιστές][api-management-dev-portal-apis]

Κάντε κλικ στην επιλογή **Δοκιμή**.

![Δοκιμάστε το][api-management-dev-portal-try-it]

Κάντε κλικ στην επιλογή **Αποστολή** και σημειώστε την κατάσταση απόκρισης **401 εξουσιοδότηση**.

![Αποστολή][api-management-dev-portal-send-401]

Η αίτηση είναι μη εξουσιοδοτημένη επειδή υπόβαθρο API προστατεύεται από Azure Active Directory. Πριν από την κλήση με επιτυχία το API ο προγραμματιστής πύλη πρέπει να ρυθμιστεί για να εξουσιοδοτήσετε τους προγραμματιστές που χρησιμοποιούν διακριτικό 2.0. Αυτή η διαδικασία περιγράφεται στις παρακάτω ενότητες.

## <a name="register-the-developer-portal-as-an-aad-application"></a>Καταχωρήσετε την πύλη για προγραμματιστές ως εφαρμογή AAD

Το πρώτο βήμα για τη ρύθμιση των παραμέτρων την πύλη για προγραμματιστές για να εξουσιοδοτήσετε τους προγραμματιστές που χρησιμοποιούν διακριτικό 2.0 είναι να καταχωρήσετε την πύλη για προγραμματιστές ως εφαρμογή AAD. Αυτό είναι επίδειξη ξεκινώντας από 8:27 στο βίντεο.

Περιήγηση στο μισθωτή Azure AD από το πρώτο βήμα αυτού του βίντεο, σε αυτό το παράδειγμα **APIMDemo** και μεταβείτε στη σελίδα " **εφαρμογές** ".

![Νέα εφαρμογή][api-management-aad-new-application-devportal]

Κάντε κλικ στο κουμπί **Προσθήκη** για να δημιουργήσετε μια νέα εφαρμογή Azure Active Directory και επιλέξτε **Προσθήκη εφαρμογής ανάπτυξη την εταιρεία μου**.

![Νέα εφαρμογή][api-management-new-aad-application-menu]

Επιλέξτε **την εφαρμογή Web ή/και το API Web**, πληκτρολογήστε ένα όνομα και κάντε κλικ στο βέλος Επόμενο. Σε αυτό το παράδειγμα χρησιμοποιείται **APIMDeveloperPortal** .

![Νέα εφαρμογή][api-management-aad-new-application-devportal-1]

Για **είσοδο στη διεύθυνση URL** , πληκτρολογήστε τη διεύθυνση URL της υπηρεσίας διαχείρισης API και προσάρτηση `/signin`. Σε αυτό το παράδειγμα χρησιμοποιείται **https://contoso5.portal.azure-api.net/signin **.

Για τη **Διεύθυνση URL αναγνωριστικό εφαρμογής** πληκτρολογήστε τη διεύθυνση URL της υπηρεσίας διαχείρισης API και προσάρτηση ορισμένους μοναδικούς χαρακτήρες. Αυτές είναι οι χαρακτήρες που θέλετε και σε αυτό το παράδειγμα χρησιμοποιείται **https://contoso5.portal.azure-api.net/dp** . Όταν έχουν ρυθμιστεί οι παράμετροι τις επιθυμητές **Ιδιότητες εφαρμογής** , κάντε κλικ στο σημάδι ελέγχου για να δημιουργήσετε την εφαρμογή.

![Νέα εφαρμογή][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Ρύθμιση παραμέτρων ενός διακομιστή εξουσιοδότησης API διαχείρισης διακριτικό 2.0

Το επόμενο βήμα είναι να ρυθμίσετε τις παραμέτρους ενός διακομιστή εξουσιοδότησης 2.0 διακριτικό API διαχείρισης. Αυτό το βήμα είναι επίδειξη του βίντεο ξεκινώντας από 9:43.

Κάντε κλικ στην επιλογή " **ασφάλεια** " από το μενού API διαχείρισης στα αριστερά, κάντε κλικ **OAuth 2.0**, και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη άδειας** διακομιστή.

![Προσθήκη άδειας διακομιστή][api-management-add-authorization-server]

Πληκτρολογήστε ένα όνομα και μια προαιρετική περιγραφή στα πεδία **όνομα** και μια **Περιγραφή** . Για να προσδιορίσετε το διακριτικό 2.0 διακομιστή εξουσιοδότησης εντός της παρουσίας της υπηρεσίας διαχείρισης API χρησιμοποιούνται αυτά τα πεδία. Σε αυτό το παράδειγμα χρησιμοποιείται **επίδειξη διακομιστή εξουσιοδότησης** . Αργότερα όταν ορίζετε μια 2.0 διακριτικό διακομιστή που θα χρησιμοποιηθεί για τον έλεγχο ταυτότητας για το API, θα μπορείτε να επιλέξετε αυτό το όνομα.

Για τη **διεύθυνση URL της σελίδας καταχώρηση προγράμματος-πελάτη** , καταχωρήστε μια τιμή κράτησης θέσης όπως `http://localhost`.  Η **διεύθυνση URL της σελίδας δήλωσης πελάτη** σημείων στη σελίδα που μπορούν να χρησιμοποιούν οι χρήστες για τη δημιουργία και ρύθμιση παραμέτρων τις δικές τους λογαριασμούς για το διακριτικό 2.0 υπηρεσίες παροχής που υποστηρίζουν διαχείριση λογαριασμών χρήστη. Σε αυτό το παράδειγμα οι χρήστες δεν δημιουργήσετε και ρυθμίσετε τις δικές τους λογαριασμούς, ώστε να χρησιμοποιείται ένα πλαίσιο κράτησης θέσης.

![Προσθήκη άδειας διακομιστή][api-management-add-authorization-server-1]

Στη συνέχεια, καθορίστε **διεύθυνση URL τελικού σημείου εξουσιοδότησης** και **διακριτικό διεύθυνση URL τελικού σημείου**.

![Εξουσιοδότηση διακομιστή][api-management-add-authorization-server-1a]

Αυτές οι τιμές μπορεί να ανακτηθεί από τη σελίδα **Τελικά σημεία εφαρμογής** της εφαρμογής AAD που δημιουργήσατε για την πύλη για προγραμματιστές. Για να αποκτήσω πρόσβαση τα τελικά σημεία, μεταβείτε στην καρτέλα **Ρύθμιση παραμέτρων** για την εφαρμογή AAD και κάντε κλικ στην επιλογή **Προβολή τελικά σημεία**.

![Εφαρμογή][api-management-aad-devportal-application]

![Προβολή τελικά σημεία][api-management-aad-view-endpoints]

Αντιγράψτε το **τελικό σημείο εξουσιοδότησης διακριτικό 2.0** και επικολλήστε τον στο πλαίσιο κειμένου **διεύθυνση URL τελικού σημείου εξουσιοδότησης** .

![Προσθήκη άδειας διακομιστή][api-management-add-authorization-server-2]

Αντιγράψτε το **διακριτικό 2.0 διακριτικού τελικού σημείου** και επικολλήστε τον στο πλαίσιο κειμένου **διεύθυνση URL τελικού σημείου διακριτικού** .

![Προσθήκη άδειας διακομιστή][api-management-add-authorization-server-2a]

Εκτός από την επικόλληση στο το διακριτικό τελικό σημείο, προσθέστε μια παράμετρο επιπλέον σώμα με το όνομα **πόρου** και για τη χρήση τιμή το **URI αναγνωριστικό εφαρμογής** από την εφαρμογή AAD για την υπηρεσία υποστήριξης που δημιουργήθηκε όταν δημοσιεύτηκε το έργο Visual Studio.

![Αναγνωριστικό εφαρμογής URI][api-management-aad-sso-uri]

Στη συνέχεια, καθορίστε τα διαπιστευτήρια προγράμματος-πελάτη. Αυτά είναι τα διαπιστευτήρια για τον πόρο που θέλετε να αποκτήσετε πρόσβαση, σε αυτήν την περίπτωση της υπηρεσίας υποστήριξης.

![Τα διαπιστευτήρια προγράμματος-πελάτη][api-management-client-credentials]

Για να λάβετε το **Αναγνωριστικό υπολογιστή-πελάτη**, μεταβείτε στην καρτέλα **Ρύθμιση παραμέτρων** της εφαρμογής AAD για την υπηρεσία υποστήριξης και αντιγράψτε το **Αναγνωριστικό υπολογιστή-πελάτη**.

Για να λάβετε το **Μυστικό προγράμματος-πελάτη** , κάντε κλικ στην επιλογή το **Επιλέξτε διάρκεια** αναπτυσσόμενη λίστα στην ενότητα **πλήκτρα** και καθορίστε ένα χρονικό διάστημα. Σε αυτό το παράδειγμα χρησιμοποιείται ενός έτους.

![Αναγνωριστικό υπολογιστή-πελάτη][api-management-aad-client-id]

Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε τη ρύθμιση παραμέτρων και να εμφανίσετε τον αριθμό-κλειδί. 

>[AZURE.IMPORTANT] Σημειώστε αυτού του κλειδιού. Αφού κλείσετε το παράθυρο Ρύθμιση παραμέτρων Azure Active Directory, τον αριθμό-κλειδί δεν μπορεί να εμφανιστεί ξανά.

Αντιγράψτε τον αριθμό-κλειδί στο Πρόχειρο, μεταβείτε ξανά στην πύλη του publisher, επικολλήστε τον αριθμό-κλειδί στο πλαίσιο κειμένου **Μυστικό προγράμματος-πελάτη** και κάντε κλικ στην επιλογή **Αποθήκευση**.

![Προσθήκη άδειας διακομιστή][api-management-add-authorization-server-3]

Αμέσως μετά τα διαπιστευτήρια προγράμματος-πελάτη είναι μια εκχώρηση άδειας κώδικα. Αντιγράψτε αυτόν τον κωδικό εξουσιοδότησης και επιστρέψτε στην εφαρμογή πύλης σας Azure AD προγραμματιστής Διαμόρφωση σελίδας και επικόλληση άδειας Παραχωρήστε στο πεδίο **Διεύθυνση URL απάντησης** και κάντε ξανά κλικ στο κουμπί **Αποθήκευση** .

![Διεύθυνση URL απάντηση][api-management-aad-reply-url]

Το επόμενο βήμα είναι να ρυθμίσετε τις παραμέτρους των δικαιωμάτων για την πύλη για προγραμματιστές εφαρμογών AAD. Κάντε κλικ στην επιλογή **Δικαιώματα εφαρμογών** και επιλέξτε το πλαίσιο ελέγχου για **Ανάγνωση καταλόγου δεδομένων**. Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε αυτήν την αλλαγή και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής**.

![Προσθήκη δικαιωμάτων][api-management-add-devportal-permissions]

Κάντε κλικ στο εικονίδιο αναζήτησης, πληκτρολογήστε **APIM** στην Έναρξη με πλαίσιο, επιλέξτε **APIMAADDemo**, και κάντε κλικ στο σημάδι ελέγχου για να αποθηκεύσετε.

![Προσθήκη δικαιωμάτων][api-management-aad-add-app-permissions]

Κάντε κλικ στην επιλογή **Δικαιώματα με ανάθεση** για **APIMAADDemo** και επιλέξτε το πλαίσιο ελέγχου για **APIMAADDemo πρόσβασης**και κάντε κλικ στην επιλογή **Αποθήκευση**. Αυτό σας επιτρέπει τον προγραμματιστή της εφαρμογής πύλης για πρόσβαση στην υπηρεσία υποστήριξης.

![Προσθήκη δικαιωμάτων][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Ενεργοποίηση OAuth 2.0 εξουσιοδότηση χρήστη για το API Αριθμομηχανή

Τώρα που έχει ρυθμιστεί στο διακομιστή OAuth 2.0, μπορείτε να το καθορίσετε στις ρυθμίσεις ασφαλείας για το API. Αυτό το βήμα είναι επίδειξη του βίντεο, ξεκινώντας από το 14:30.

Κάντε κλικ στην επιλογή **APIs** στο αριστερό μενού και κάντε κλικ στην επιλογή **υπολογιστής** για να προβάλετε και να ρυθμίσετε τις παραμέτρους των ρυθμίσεων.

![Αριθμομηχανής API][api-management-calc-api]

Μεταβείτε στην καρτέλα **ασφάλεια** , επιλέξτε το πλαίσιο ελέγχου **OAuth 2.0** , επιλέξτε το επιθυμητό εξουσιοδότησης διακομιστή από την αναπτυσσόμενη λίστα **εξουσιοδότησης διακομιστή** και κάντε κλικ στην επιλογή **Αποθήκευση**.

![Αριθμομηχανής API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Κλήση με επιτυχία το API Αριθμομηχανής από την πύλη για προγραμματιστές

Τώρα που η άδεια OAuth 2.0 έχει ρυθμιστεί στο το API, τις εργασίες του μπορεί να ονομάζεται με επιτυχία από το Κέντρο για προγραμματιστές. Αυτό το βήμα είναι επίδειξη του βίντεο ξεκινώντας από 15:00.

Μεταβείτε ξανά τη λειτουργία **δύο ακέραιους αριθμούς Προσθήκη** της υπηρεσίας Αριθμομηχανής στην πύλη για προγραμματιστές και κάντε κλικ στην επιλογή **Δοκιμή**. Σημείωση το νέο στοιχείο στην ενότητα **εξουσιοδότηση** αντιστοιχεί στο διακομιστή εξουσιοδότησης που μόλις προσθέσατε.

![Αριθμομηχανής API][api-management-calc-authorization-server]

Επιλέξτε **εξουσιοδότηση κώδικα** από την αναπτυσσόμενη λίστα εξουσιοδότησης και εισαγάγετε τα διαπιστευτήρια του λογαριασμού για να χρησιμοποιήσετε. Εάν είστε ήδη συνδεδεμένοι με το λογαριασμό που δεν μπορεί να σας ζητηθεί.

![Αριθμομηχανής API][api-management-devportal-authorization-code]

Κάντε κλικ στην επιλογή **Αποστολή** και σημειώστε την **κατάσταση απόκρισης** **200 OK** και τα αποτελέσματα της λειτουργίας στο περιεχόμενο απόκρισης.

![Αριθμομηχανής API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Ρύθμιση παραμέτρων μιας εφαρμογής επιφάνειας εργασίας για να καλέσετε το API

Στην επόμενη διαδικασία του βίντεο ξεκινά από 16:30 πμ και ρυθμίζει τις παραμέτρους μιας απλής εφαρμογής υπολογιστή για να καλέσετε το API. Το πρώτο βήμα είναι να καταχωρήσετε την εφαρμογή υπολογιστή στο Azure AD και να της δώσετε πρόσβαση στον κατάλογο και να την υπηρεσία υποστήριξης. Στο 18:25 υπάρχει μια επίδειξη σχετικά με την εφαρμογή υπολογιστή κλήση μιας λειτουργίας σε την Αριθμομηχανή API.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Ρύθμιση παραμέτρων μιας πολιτικής επικύρωσης JWT για να προ-εξουσιοδοτήσετε αιτήσεις

Η τελική διαδικασία του βίντεο ξεκινά στο 20:48 και σας δείχνει πώς μπορείτε να χρησιμοποιήσετε την πολιτική [JWT επικύρωση](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) για να προ-εξουσιοδοτήσετε αιτήσεις από την επικύρωση τα διακριτικά πρόσβασης κάθε εισερχόμενη αίτηση. Εάν η αίτηση δεν είναι επικύρωση από την πολιτική επικύρωση JWT, την αίτηση έχει αποκλειστεί από το API διαχείρισης και δεν μεταβιβάζεται κατά μήκος στον υπολογιστή στο παρασκήνιο.

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

Για μια άλλη επίδειξη σχετικά με τη ρύθμιση των παραμέτρων και τη χρήση αυτής της πολιτικής, ανατρέξτε στο θέμα [177 επεισοδίου συνοδευτική Cloud: περισσότερες δυνατότητες διαχείρισης API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) και προώθηση ταινίας έως 13:50. Γρήγορη προώθηση να 15:00 για να δείτε τις πολιτικές που έχουν ρυθμιστεί στο πρόγραμμα επεξεργασίας πολιτικής και, στη συνέχεια, να 18:50 για μια επίδειξη σχετικά με την κλήση μιας λειτουργίας από την πύλη για προγραμματιστές τόσο με και χωρίς το διακριτικό απαιτείται άδεια.

## <a name="next-steps"></a>Επόμενα βήματα
-   Δείτε περισσότερα [βίντεο](https://azure.microsoft.com/documentation/videos/index/?services=api-management) σχετικά με τη Διαχείριση API.
-   Για άλλους τρόπους για την ασφάλιση της υπηρεσίας υποστήριξης, ανατρέξτε στο θέμα [αμοιβαία πιστοποιητικό ελέγχου ταυτότητας](api-management-howto-mutual-certificates.md) και [σύνδεση μέσω VPN ή ExpressRoute](api-management-howto-setup-vpn.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Δημιουργήστε μια παρουσία της υπηρεσίας διαχείρισης API]: api-management-get-started.md#create-service-instance
[Διαχείριση του πρώτου API]: api-management-get-started.md
