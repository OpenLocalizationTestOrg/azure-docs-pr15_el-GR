<properties
    pageTitle="Υποστήριξη CORS στα εφαρμογής υπηρεσίας | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υποστήριξη CORS στο Azure Azure εφαρμογής υπηρεσίας."
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
    ms.topic="get-started-article"
    ms.date="08/27/2016"
    ms.author="rachelap"/>

# <a name="consume-an-api-app-from-javascript-using-cors"></a>Χρήση εφαρμογής API από JavaScript χρησιμοποιώντας CORS

Εφαρμογή υπηρεσίας προσφέρει ενσωματωμένη υποστήριξη για [Διασταυρούμενο Origin πόρων κοινής χρήσης (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), που σας επιτρέπει JavaScript προγράμματα-πελάτες για να κάνετε κλήσεις μεταξύ τομέων API που φιλοξενούνται σε εφαρμογές API. Εφαρμογή υπηρεσίας σάς επιτρέπει να ρυθμίσετε τις παραμέτρους πρόσβασης CORS για το API χωρίς τη σύνταξη κώδικα στο API σας.

Σε αυτό το άρθρο περιέχει δύο ενότητες:

* Στην ενότητα [πώς μπορείτε να ρυθμίσετε τις παραμέτρους CORS](#corsconfig) γενικά εξηγεί πώς μπορείτε να ρυθμίσετε τις παραμέτρους CORS για οποιαδήποτε εφαρμογή API, εφαρμογή web ή εφαρμογή για κινητές συσκευές. Αυτό ισχύει και για όλα τα πλαίσια που υποστηρίζονται από εφαρμογής υπηρεσίας, συμπεριλαμβανομένης της .NET, Node.js και Java. 

* Ξεκινώντας με την ενότητα [συνεχίσετε τα προγράμματα εκμάθησης για το .NET γρήγορα αποτελέσματα](#tutorialstart) , το άρθρο είναι ένα πρόγραμμα εκμάθησης που δείχνει CORS υποστήριξη με δόμηση σε τι κάνατε στο [πρόγραμμα εκμάθησης για το πρώτο εφαρμογές API γρήγορα αποτελέσματα](app-service-api-dotnet-get-started.md). 

## <a id="corsconfig"></a>Τρόπος ρύθμισης παραμέτρων CORS στο Azure εφαρμογής υπηρεσίας

Μπορείτε να ρυθμίσετε CORS στην πύλη του Azure ή χρησιμοποιώντας τα εργαλεία [Διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md) .

#### <a name="configure-cors-in-the-azure-portal"></a>Ρύθμιση παραμέτρων CORS στην πύλη του Azure

8. Σε ένα πρόγραμμα περιήγησης, μεταβείτε στην [πύλη του Azure](https://portal.azure.com/).

2. Κάντε κλικ στην επιλογή **Εφαρμογή υπηρεσιών**και, στη συνέχεια, κάντε κλικ στο όνομα της εφαρμογής API.

    ![Επιλέξτε εφαρμογή API στην πύλη](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Στο το blade **Ρυθμίσεις** που ανοίγει στη δεξιά πλευρά του blade **εφαρμογής API** , βρείτε την ενότητα **API** και, στη συνέχεια, κάντε κλικ στην επιλογή **CORS**.

    ![Επιλέξτε CORS στο blade ρυθμίσεις](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. Το κείμενο του πλαισίου εισαγάγετε τη διεύθυνση URL ή τις διευθύνσεις URL που θέλετε να επιτρέψετε τις κλήσεις JavaScript για να προέρχονται από.


    Για παράδειγμα, εάν αναπτύξει την εφαρμογή σας JavaScript σε μια εφαρμογή web με το όνομα todolistangular, πληκτρολογήστε "https://todolistangular.azurewebsites.net". Ως εναλλακτική λύση, μπορείτε να εισαγάγετε έναν αστερίσκο (*) για να καθορίσετε ότι όλοι οι τομείς origin γίνονται δεκτές.


13. Κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Κάντε κλικ στην επιλογή Αποθήκευση](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Αφού κάνετε κλικ στο κουμπί **Αποθήκευση**, την εφαρμογή API αποδέχεται JavaScript κλήσεις από τις καθορισμένες διευθύνσεις URL.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Ρύθμιση παραμέτρων CORS χρησιμοποιώντας τα εργαλεία διαχείρισης πόρων Azure

Μπορείτε επίσης να ρυθμίσετε CORS για μια εφαρμογή API με τη χρήση [προτύπων από διαχειριστή πόρων Azure](../resource-group-authoring-templates.md) στη γραμμή εντολών εργαλεία όπως το [Azure PowerShell](../powershell-install-configure.md) και το [Azure CLI](../xplat-cli-install.md). 

Για ένα παράδειγμα ενός προτύπου για τη διαχείριση πόρων Azure που ορίζει την ιδιότητα CORS, ανοίξτε το [αρχείο azuredeploy.json στο αποθετήριο δεδομένων για αυτό το πρόγραμμα εκμάθησης του δείγματος εφαρμογής](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Βρείτε την ενότητα του προτύπου που μοιάζει με το ακόλουθο παράδειγμα:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>Συνεχίσετε το πρόγραμμα εκμάθησης ξεκινήσατε .NET

Εάν ακολουθείτε την Node.js ή Java γρήγορα αποτελέσματα σειράς για τις εφαρμογές του API, έχετε ολοκληρώσει τη σειρά γρήγορα αποτελέσματα. Μεταβείτε στην ενότητα [επόμενα βήματα](#next-steps) για να βρείτε προτάσεις για περαιτέρω πληροφορίες σχετικά με το API εφαρμογές.

Το υπόλοιπο της σε αυτό το άρθρο είναι συνέχισης της σειράς .NET γρήγορα αποτελέσματα και προϋποθέτει ότι ολοκληρώθηκε με επιτυχία [το πρώτο πρόγραμμα εκμάθησης](app-service-api-dotnet-get-started.md).

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>Αναπτύξτε το έργο ToDoListAngular σε μια νέα εφαρμογή web

Με [το πρώτο πρόγραμμα εκμάθησης](app-service-api-dotnet-get-started.md), δημιουργήσατε μια εφαρμογή API μεσαίας και μια εφαρμογή API σειρά δεδομένων. Σε αυτό το πρόγραμμα εκμάθησης δημιουργείτε μια εφαρμογή web μίας σελίδας εφαρμογής (SPA) που κλήσεις το API μεσαίας εφαρμογή. Για το SPA εργασίας που έχετε για να ενεργοποιήσετε την εφαρμογή μεσαίας API CORS. 

Στην [ToDoList δείγμα εφαρμογής](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), το έργο ToDoListAngular είναι μια απλή AngularJS πρόγραμμα-πελάτης που καλεί το έργο ToDoListAPI Web API Μεσαία σειρά. Τον κώδικα JavaScript στο αρχείο *app/scripts/todoListSvc.js* καλεί το API χρησιμοποιώντας την υπηρεσία παροχής AngularJS HTTP. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
        
            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Δημιουργία νέας εφαρμογής web για το έργο ToDoListAngular

Η διαδικασία για να δημιουργήσετε μια νέα εφαρμογή web εφαρμογής υπηρεσίας και ανάπτυξη ενός έργου σε αυτό είναι παρόμοιο με τι είδατε για [τη δημιουργία και την ανάπτυξη εφαρμογής API της πρώτης εκμάθησης σε αυτήν τη σειρά](app-service-api-dotnet-get-started.md#createapiapp). Η μόνη διαφορά είναι ότι ο τύπος της εφαρμογής είναι **Web App** αντί για το **API εφαρμογής**.  Για στιγμιότυπα οθόνης από τα παράθυρα διαλόγου, ανατρέξτε στο θέμα 

1. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο ToDoListAngular και, στη συνέχεια, κάντε κλικ στο κουμπί **Δημοσίευση**.

3.  Στην καρτέλα **προφίλ** του οδηγού **Δημοσίευση Web** , κάντε κλικ στην επιλογή **Microsoft Azure εφαρμογής υπηρεσίας**.

5. Στο παράθυρο διαλόγου **Εφαρμογής υπηρεσίας** , κάντε κλικ στην επιλογή **Δημιουργία**.

3. Στην καρτέλα **κεντρικού υπολογιστή** , στο παράθυρο διαλόγου **Δημιουργία εφαρμογής υπηρεσίας** , πληκτρολογήστε ένα **Όνομα εφαρμογής Web** που είναι μοναδικές στον τομέα *azurewebsites.net* . 

5. Επιλέξτε το Azure που θέλετε να εργαστείτε με **τη συνδρομή** .

6. Στη λίστα αναπτυσσόμενη λίστα " **Ομάδα πόρων** ", επιλέξτε την ίδια ομάδα των πόρων που δημιουργήσατε νωρίτερα.

4. Στην αναπτυσσόμενη λίστα **Σχέδιο παροχής υπηρεσιών εφαρμογής** , επιλέξτε το ίδιο πρόγραμμα που δημιουργήσατε νωρίτερα. 

7. Κάντε κλικ στην επιλογή **Δημιουργία**.

    Visual Studio δημιουργεί την εφαρμογή web δημιουργεί ένα προφίλ δημοσίευση για αυτό και εμφανίζει τη **σύνδεση** βήμα του οδηγού **Δημοσίευσης Web** .

    Μην κάνετε κλικ **Δημοσίευση** ακόμη. Στην παρακάτω ενότητα, μπορείτε να ρυθμίσετε τη νέα εφαρμογή web για να καλέσετε την εφαρμογή API μεσαίας που εκτελείται στο εφαρμογής υπηρεσίας. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>Ορισμός της διεύθυνσης URL μεσαίας στο των ρυθμίσεων της εφαρμογής web

1. Μεταβείτε στην [πύλη του Azure](https://portal.azure.com/)και, στη συνέχεια, αναζητήστε το blade **Web App** για την εφαρμογή web που δημιουργήσατε για τη φιλοξενία του έργου TodoListAngular (προσκηνίου).

2. Κάντε κλικ στην επιλογή **Ρυθμίσεις > Ρυθμίσεις εφαρμογής**.

3. Στην ενότητα **Ρυθμίσεις εφαρμογής** , προσθέστε το ακόλουθο κλειδί και τιμή:

  	|Πλήκτρο|Τιμή|Παράδειγμα
  	|---|---|---|
  	|toDoListAPIURL|όνομα εφαρμογής μεσαίας API https://{your} .azurewebsites .net|https://todolistapi0121.azurewebsites.NET|

4. Κάντε κλικ στην επιλογή **Αποθήκευση**.

    Όταν ο κώδικας που εκτελείται στο Azure, αυτή η τιμή αντικαθιστά τη διεύθυνση URL του τοπικού κεντρικού υπολογιστή που βρίσκεται στο αρχείο *Web.config* . 

    Ο κώδικας που λαμβάνει την τιμή ρύθμισης είναι στο *index.cshtml*:

        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>

    Ο κώδικας *todoListSvc.js* χρησιμοποιεί τη ρύθμιση:

        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>Αναπτύξτε το έργο web ToDoListAngular για τη νέα εφαρμογή web

*  Στο Visual Studio, στο βήμα του οδηγού **Δημοσίευση Web** , **σύνδεσης** , κάντε κλικ στο κουμπί **Δημοσίευση**.

    Visual Studio αναπτύσσει το έργο ToDoListAngular για τη νέα εφαρμογή web και ανοίγει ένα πρόγραμμα περιήγησης στη διεύθυνση URL της εφαρμογής web. 

### <a name="test-the-application-without-cors-enabled"></a>Δοκιμή της εφαρμογής χωρίς CORS με δυνατότητα 

2. Στο πρόγραμμα περιήγησης εργαλεία για προγραμματιστές, ανοίξτε το παράθυρο της κονσόλας.

3. Στο παράθυρο του προγράμματος περιήγησης που εμφανίζει το περιβάλλον εργασίας Χρήστη AngularJS, κάντε κλικ στη σύνδεση **Λίστα εκκρεμών εργασιών** .

    Ο κώδικας JavaScript προσπαθεί να καλέσετε την εφαρμογή μεσαίας API, αλλά η κλήση αποτυγχάνει επειδή το προσκήνιο εκτελείται σε διαφορετικό τομέα από παρασκηνίου. Παράθυρο κονσόλας εργαλεία για προγραμματιστές του προγράμματος περιήγησης εμφανίζει ένα μήνυμα σφάλματος origin σταυρό.

    ![Μήνυμα σφάλματος origin σταυρού](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>Ρύθμιση παραμέτρων CORS για την εφαρμογή API Μεσαία σειρά

Σε αυτήν την ενότητα, μπορείτε να ρυθμίσετε τη ρύθμιση CORS στο Azure για το μεσαίο επίπεδο εφαρμογής ToDoListAPI API. Αυτή η ρύθμιση επιτρέπει στο μεσαίο επίπεδο εφαρμογής API για να λαμβάνετε κλήσεις JavaScript από την εφαρμογή web που δημιουργήσατε για το έργο ToDoListAngular.

8. Σε ένα πρόγραμμα περιήγησης, μεταβείτε στην [πύλη του Azure](https://portal.azure.com/).

2. Κάντε κλικ στην επιλογή **Εφαρμογή υπηρεσιών**και, στη συνέχεια, κάντε κλικ στην εφαρμογή API (μεσαίας) ToDoListAPI.

    ![Επιλέξτε εφαρμογή API στην πύλη](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. Στο το blade **Ρυθμίσεις** που ανοίγει στη δεξιά πλευρά του blade **εφαρμογής API** , βρείτε την ενότητα **API** και, στη συνέχεια, κάντε κλικ στην επιλογή **CORS**.

    ![Επιλέξτε CORS στην πύλη](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. Στο πλαίσιο κειμένου, πληκτρολογήστε τη διεύθυνση URL για την εφαρμογή web ToDoListAngular (προσκηνίου). Για παράδειγμα, αν το έργο ToDoListAngular αναπτύξει σε μια εφαρμογή web με το όνομα todolistangular0121, αφήστε τις κλήσεις από τη διεύθυνση URL `https://todolistangular0121.azurewebsites.net`.

    Ως εναλλακτική λύση, μπορείτε να εισαγάγετε έναν αστερίσκο (*) για να καθορίσετε ότι όλοι οι τομείς origin γίνονται δεκτές.

13. Κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Κάντε κλικ στην επιλογή Αποθήκευση](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Αφού κάνετε κλικ στο κουμπί **Αποθήκευση**, την εφαρμογή API αποδέχεται JavaScript κλήσεις από την καθορισμένη διεύθυνση URL. Σε αυτό το στιγμιότυπο οθόνης, η εφαρμογή ToDoListAPI0223 API αποδέχεται JavaScript κλήσεις προγράμματος-πελάτη από την εφαρμογή web ToDoListAngular.

### <a name="test-the-application-with-cors-enabled"></a>Δοκιμή της εφαρμογής με CORS με δυνατότητα

* Ανοίξτε ένα πρόγραμμα περιήγησης στη διεύθυνση URL HTTPS του web app. 

    Αυτήν τη στιγμή της εφαρμογής σας επιτρέπει να προβολή, προσθήκη, επεξεργασία και διαγραφή στοιχείων εκκρεμών εργασιών. 

    ![Λίστα εκκρεμών εργασιών σελίδα της εφαρμογής δείγματος](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>Εφαρμογή υπηρεσίας CORS έναντι Web API CORS

Σε ένα έργο Web API, μπορείτε να εγκαταστήσετε το πακέτο [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet για να καθορίσετε στον κώδικα των τομέων σας API αποδέχεται JavaScript κλήσεις από.
 
Η υποστήριξη API CORS Web είναι πιο ευέλικτη από CORS εφαρμογής υπηρεσίας υποστήριξης. Για παράδειγμα, στον κώδικα να καθορίσετε διαφορετικών προελεύσεων αποδεκτή για μεθόδους διαφορετική ενέργεια, ενώ για εφαρμογή υπηρεσίας CORS μπορείτε να καθορίσετε ένα σύνολο αποδεκτή προελεύσεις για όλες της εφαρμογής API μεθόδους.

> [AZURE.NOTE] Δεν προσπαθήστε να χρησιμοποιήσετε Web API CORS και εφαρμογή υπηρεσίας CORS σε μια εφαρμογή του API. Εφαρμογή υπηρεσίας CORS θα έχει προτεραιότητα και Web API CORS θα έχει κανένα αποτέλεσμα. Για παράδειγμα, εάν ενεργοποιήσετε έναν τομέα origin στο εφαρμογής υπηρεσίας και ενεργοποίηση όλων των τομέων origin στον κώδικα Web API, την εφαρμογή του Azure API θα δέχεται κλήσεις μόνο από τον τομέα που καθορίσατε στο Azure.

### <a name="how-to-enable-cors-in-web-api-code"></a>Πώς μπορείτε να ενεργοποιήσετε CORS στον κώδικα Web API

Ακολουθήστε τα παρακάτω βήματα σύνοψη της διαδικασίας για ενεργοποίηση υποστήριξης Web API CORS. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ενεργοποίηση αιτήσεις σταυρό Origin στο ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. Σε ένα έργο Web API, εγκαταστήστε το πακέτο NuGet [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) .

1. Συμπεριλάβετε μια `config.EnableCors()` γραμμή κώδικα στη μέθοδο **καταχώρηση** της κλάσης **WebApiConfig** , όπως στο παρακάτω παράδειγμα. 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. Στο ελεγκτή σας Web API, προσθέστε μια `using` δήλωση για το `System.Web.Http.Cors` χώρο ονομάτων, και προσθέστε το `EnableCors` χαρακτηριστικό σε τάξη ελεγκτή ή σε μεμονωμένα ενέργεια μεθόδους. Στο παρακάτω παράδειγμα, υποστήριξη CORS ισχύει για ολόκληρο τον ελεγκτή.

        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController
 
## <a name="using-azure-api-management-with-api-apps"></a>Χρήση της διαχείρισης Azure API με εφαρμογές API

Εάν χρησιμοποιείτε το Azure API διαχείρισης με μια εφαρμογή API, ρύθμιση παραμέτρων CORS API διαχείρισης αντί στην εφαρμογή API. Για περισσότερες πληροφορίες, ανατρέξτε στους ακόλουθους πόρους:

* [Επισκόπηση της διαχείρισης Azure API (βίντεο: CORS ξεκινά από 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [API διαχείρισης διασταύρωσης πολιτικές τομέα](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)
 
## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

Σε περίπτωση που αντιμετωπίσετε κάποιο πρόβλημα κατά διέρχονται από αυτό το πρόγραμμα εκμάθησης, εδώ θα βρείτε ορισμένες ιδέες αντιμετώπισης προβλημάτων.

* Βεβαιωθείτε ότι χρησιμοποιείτε την πιο πρόσφατη έκδοση του [Azure SDK για .NET Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).

* Βεβαιωθείτε ότι έχετε εισαγάγει `https` στη ρύθμιση CORS, και βεβαιωθείτε ότι χρησιμοποιείτε `https` για να εκτελέσετε την εφαρμογή web προσκηνίου.

* Βεβαιωθείτε ότι πληκτρολογήσατε τη ρύθμιση CORS στην εφαρμογή API μεσαίας και όχι στην εφαρμογή web προσκηνίου.

* Εάν τη ρύθμιση παραμέτρων CORS σε κώδικα της εφαρμογής και Azure εφαρμογής υπηρεσίας, σημειώστε ότι η ρύθμιση της εφαρμογής υπηρεσίας CORS θα αντικαταστήσει ό, τι ασχολείστε τη δεδομένη στιγμή στον κώδικα της εφαρμογής. 

Για να μάθετε περισσότερα σχετικά με τις δυνατότητες του Visual Studio που θα απλοποιήσει αντιμετώπισης προβλημάτων, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων Azure εφαρμογής υπηρεσίας εφαρμογών στο Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Επόμενα βήματα 

Σε αυτό το άρθρο, είδατε πώς μπορείτε να ενεργοποιήσετε την υποστήριξη εφαρμογών υπηρεσίας CORS ώστε να κώδικα JavaScript προγράμματος-πελάτη μπορούν να καλέσουν το API σε διαφορετικό τομέα. Για να μάθετε περισσότερα σχετικά με τις εφαρμογές API, διαβάστε την [Εισαγωγή με τον έλεγχο ταυτότητας στο εφαρμογής υπηρεσίας](../app-service/app-service-authentication-overview.md)και, στη συνέχεια, επιλέξτε το πρόγραμμα εκμάθησης [τον έλεγχο ταυτότητας χρήστη για το API εφαρμογές](app-service-api-dotnet-user-principal-auth.md) .
