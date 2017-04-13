<properties
    pageTitle="Ειδοποιήσεις push διαχωρισμός παν με διανομείς ειδοποίηση Azure και δεδομένων χώρου Bing | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς να κάνετε τις ειδοποιήσεις push βασίζεται σε θέση με διανομείς ειδοποίηση Azure και δεδομένων χώρου Bing."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="ειδοποιήσεων Push, ειδοποιήσεων push"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Ειδοποιήσεις push διαχωρισμός παν με διανομείς ειδοποίηση Azure και δεδομένων χώρου Bing
 
 > [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς να κάνετε τις ειδοποιήσεις push βασίζεται σε θέση με διανομείς ειδοποίηση Azure και δεδομένα χώρου Bing, δυνατή από μέσα σε μια εφαρμογή καθολικής πλατφόρμας των Windows.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Πρώτα και κύριο λόγο, πρέπει να βεβαιωθείτε ότι έχετε όλα του λογισμικού και των υπηρεσιών προαπαιτούμενα:

* [Visual Studio 2015 ενημέρωση 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) ή νεότερη έκδοση (η[Κοινότητα Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) θα κάνετε καθώς και). 
* Τελευταία έκδοση του [Azure SDK](https://azure.microsoft.com/downloads/). 
* [Κέντρο ανάπτυξης χάρτες Bing λογαριασμού](https://www.bingmapsportal.com/) (μπορείτε να δημιουργήσετε έναν δωρεάν και να συσχετίσετε με το λογαριασμό σας Microsoft). 

##<a name="getting-started"></a>Γρήγορα αποτελέσματα

Ας ξεκινήσουμε με τη δημιουργία του έργου. Στο Visual Studio, ξεκινήστε ένα νέο έργο του τύπου **Κενή εφαρμογή (Universal Windows)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Μόλις ολοκληρωθεί η δημιουργία έργου, θα πρέπει να έχετε τη δέσμη για την ίδια την εφαρμογή. Τώρα ας ρυθμίσουμε όλα τα στοιχεία για την υποδομή χωριστή παρουσίαση παν και διαχείριση. Επειδή πρόκειται να χρησιμοποιήσετε τις υπηρεσίες Bing για αυτό, υπάρχει μια δημόσια τελικού σημείου REST API που επιτρέπει την μας ερώτημα για συγκεκριμένη θέση πλαίσια:

    http://spatial.virtualearth.net/REST/v1/data/
    
Θα πρέπει να καθορίσετε τις ακόλουθες παραμέτρους για να λειτουργήσει:

* **Αναγνωριστικό προέλευσης δεδομένων** και το **Όνομα της προέλευσης δεδομένων** – API χάρτες Bing, προελεύσεις δεδομένων περιέχουν διάφορες bucketed μετα-δεδομένα, όπως θέσεις και εργάσιμες ώρες λειτουργίας. Μπορείτε να διαβάσετε περισσότερα σχετικά με αυτές τις εδώ. 
* **Όνομα οντότητας** – η οντότητα που θέλετε να χρησιμοποιήσετε ως σημείο αναφοράς για την ειδοποίηση. 
* **Πλήκτρο API χάρτες Bing** – αυτό είναι το κλειδί που λάβατε νωρίτερα όταν δημιουργήσατε το λογαριασμό κέντρο ανάπτυξης Bing.
 
Ας κάντε βάθος-κατάρρευση στην τη ρύθμιση για κάθε ένα από τα παραπάνω στοιχεία.

##<a name="setting-up-the-data-source"></a>Για τη ρύθμιση του αρχείου προέλευσης δεδομένων

Μπορείτε να το κάνετε στο κέντρο ανάπτυξης του χάρτες Bing. Απλώς κάντε κλικ στην επιλογή σε **προελεύσεις δεδομένων** στην επάνω γραμμή περιήγησης και επιλέξτε **Διαχείριση προελεύσεων δεδομένων**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Εάν δεν έχετε χρησιμοποιήσει το API χάρτες Bing πριν, πιθανότατα δεν υπάρχει οποιαδήποτε παρουσίαση, προελεύσεις δεδομένων, ώστε να μπορείτε απλώς να δημιουργήσετε ένα νέο, κάνοντας κλικ στο κουμπί Αποστολή δεδομένων σε μια προέλευση δεδομένων. Βεβαιωθείτε ότι μπορείτε να συμπληρώσετε όλα τα απαιτούμενα πεδία:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Ίσως αναρωτηθείτε – τι είναι το αρχείο δεδομένων και τι θα πρέπει να μπορείτε αποστολή; Για τους σκοπούς της αυτόν τον έλεγχο, μπορούμε να χρησιμοποιήσουμε απλώς το δείγμα βασίζεται σε κατακόρυφη γραμμή που πλαισίων μια περιοχή του το Σαν Φρανσίσκο waterfront:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
Τα παραπάνω αντιπροσωπεύει αυτή η οντότητα:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Απλώς αντιγράψτε και επικολλήστε την παραπάνω συμβολοσειρά σε ένα νέο αρχείο και αποθηκεύστε το ως **NotificationHubsGeofence.pipe**και στείλτε το στο κέντρο ανάπτυξης του Bing.

>[AZURE.NOTE]Ίσως σας ζητηθεί να καθορίσετε ένα νέο αριθμό-κλειδί για το **πρωτεύον κλειδί** που είναι διαφορετικό από το **Κλειδί ερωτήματος**. Απλώς δημιουργήστε ένα νέο κλειδί μέσω του πίνακα εργαλείων και ανανεώστε τη σελίδα αποστολής αρχείου προέλευσης δεδομένων.

Όταν κάνετε αποστολή του αρχείου δεδομένων, θα πρέπει να βεβαιωθείτε ότι μπορείτε να δημοσιεύσετε το αρχείο προέλευσης δεδομένων. 

Μεταβείτε στη **Διαχείριση προελεύσεων δεδομένων**, όπως κάναμε παραπάνω, βρείτε το αρχείο προέλευσης δεδομένων στη λίστα και κάντε κλικ στη **Δημοσίευση** στη στήλη **Actions** . Σε λίγο, θα πρέπει να βλέπετε το αρχείο προέλευσης δεδομένων στην καρτέλα **Δημοσίευση προελεύσεις δεδομένων** :

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Εάν κάνετε κλικ στο κουμπί " **Επεξεργασία**", θα μπορείτε να δείτε με μια ματιά τι θέσεις θα σας έχουν εισαχθεί σε αυτήν:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

Σε αυτό το σημείο, την πύλη δεν δείχνουν τα όρια για το geofence που δημιουργήσαμε – μόνο χρειαζόμαστε είναι μια επιβεβαίωσης που βρίσκεται στη θέση που καθορίζεται στην δεξιά περιοχή.

Τώρα έχετε όλες τις απαιτήσεις για την προέλευση δεδομένων. Για να λάβετε τις λεπτομέρειες στη διεύθυνση URL του αίτηση για την κλήση API, στο κέντρο ανάπτυξης του χάρτες Bing, κάντε κλικ στην επιλογή **προελεύσεις δεδομένων** και επιλέξτε **Πληροφορίες για την προέλευση δεδομένων**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

Η **Διεύθυνση URL ερωτήματος** είναι είμαστε μετά από αυτό το σημείο. Αυτό είναι το τελικό σημείο προς τα οποία θα σας μπορεί να εκτελέσει ερωτημάτων για να ελέγξετε αν η συσκευή είναι αυτήν τη στιγμή εντός των ορίων μια θέση ή όχι. Για να εκτελέσετε αυτόν τον έλεγχο, απλώς πρέπει να εκτελεί μια κλήση ΛΉΨΗ σε σχέση με τη διεύθυνση URL ερωτήματος, με τις ακόλουθες παραμέτρους προσαρτημένο:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Με αυτόν τον τρόπο που καθορίζετε σε σημείο προορισμού που θα σας αποκτήσετε από τη συσκευή και χάρτες Bing θα γίνει αυτόματα οι υπολογισμοί για να δείτε εάν είναι εντός του geofence. Όταν εκτελείτε την αίτηση μέσω ενός προγράμματος περιήγησης (ή καμπύλη), θα λάβετε τυπική απόκριση JSON:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Αυτή η απόκριση δεν συμβεί μόνο όταν το σημείο είναι στην πραγματικότητα εντός των ορίων που έχει οριστεί ως. Εάν δεν είναι, θα λάβετε μια κενή **αποτελέσματα** χρωματισμού:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##<a name="setting-up-the-uwp-application"></a>Εγκατάσταση της εφαρμογής UWP

Τώρα που έχουμε στην προέλευση δεδομένων που είναι έτοιμη, θα σας να αρχίσετε να εργάζεστε στην εφαρμογή του UWP που θα σας γίνεται εκκίνηση νωρίτερα.

Πρώτα και κύριο λόγο, θα σας πρέπει να ενεργοποιήσετε τις υπηρεσίες θέση για την εφαρμογή. Για να το κάνετε αυτό, κάντε διπλό κλικ στο `Package.appxmanifest` αρχείο στην **Εξερεύνηση λύσεων**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Στην καρτέλα Ιδιότητες πακέτου που μόλις ανοίξατε, κάντε κλικ στην επιλογή **δυνατότητες** και βεβαιωθείτε ότι έχετε επιλέξει **θέση**:

![](./media/notification-hubs-geofence/vs-package-location.png)

Όπως δηλώνεται τη δυνατότητα θέση, δημιουργήστε ένα νέο φάκελο στη λύση σας με το όνομα `Core`, και να προσθέσετε ένα νέο αρχείο μέσα σε αυτό, που ονομάζεται `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

Το `LocationHelper` κλάσης ίδια είναι αρκετά βασικές σε αυτό το σημείο-μόνο κάνει είναι μας για να αποκτήσετε τη θέση του χρήστη μέσω του συστήματος API επιτρέπει:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Μπορείτε να διαβάσετε περισσότερα σχετικά με τη λήψη τη θέση του χρήστη στις εφαρμογές UWP στο επίσημη [MSDN στο έγγραφο](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

Για να ελέγξετε την απόκτηση θέση λειτουργεί κανονικά, ανοίξτε το στο πλάι κώδικα από την κύρια σελίδα (`MainPage.xaml.cs`). Δημιουργήστε ένα νέο πρόγραμμα χειρισμού συμβάντων για τα `Loaded` συμβάντος στο ημερολόγιο του `MainPage` κατασκευή:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Η εφαρμογή του προγράμματος χειρισμού συμβάντων είναι ως εξής:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Παρατηρήστε ότι σας έχουν δηλωθεί το πρόγραμμα χειρισμού ως ασύγχρονης επειδή `GetCurrentLocation` είναι awaitable και, επομένως, απαιτεί να εκτελεστούν σε ένα περιβάλλον ασύγχρονης. Επίσης, επειδή υπό ορισμένες συνθήκες θα σας ίσως καταλήξετε με μια θέση null (π.χ. τη θέση είναι απενεργοποιημένες υπηρεσίες ή η εφαρμογή δεν επιτράπηκε δικαιώματα πρόσβασης θέση), πρέπει να βεβαιωθείτε ότι είναι σωστά χειρισμού με ένα σημάδι ελέγχου null.

Εκτελέστε την εφαρμογή. Βεβαιωθείτε ότι επιτρέπετε την πρόσβαση θέση:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Μία φορά την εφαρμογή εκκινήσεις, θα πρέπει να μπορούν να βλέπουν τις συντεταγμένες στο παράθυρο **εξόδου** :

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Τώρα που γνωρίζετε ότι λειτουργεί acquisition θέση – όποιους για να καταργήσετε το πρόγραμμα χειρισμού συμβάντων δοκιμή για Loaded, επειδή θα σας δεν θα τον χρησιμοποιείτε πλέον.

Το επόμενο βήμα είναι να καταγράψετε τις αλλαγές του θέση. Για αυτό, επιστρέψτε στο το `LocationHelper` κλάση και προσθέστε το πρόγραμμα χειρισμού συμβάντων για `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

Η εφαρμογή θα εμφανίσει τις συντεταγμένες θέση στο παράθυρο **εξόδου** :

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##<a name="setting-up-the-backend"></a>Για τη ρύθμιση του υπολογιστή στο παρασκήνιο

Κάντε λήψη του [Δείγματος παρασκηνίου .NET από GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). Μόλις ολοκληρωθεί η λήψη, ανοίξτε το `NotifyUsers` φακέλου, και στη συνέχεια – το `NotifyUsers.sln` αρχείου.

Ορίστε το `AppBackend` project με το **Project εκκίνησης** και εκκίνηση του προγράμματος.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Το έργο έχει ήδη ρυθμιστεί για την αποστολή ειδοποιήσεων push σε συσκευές προορισμού, έτσι θα πρέπει να κάνετε μόνο δύο πράγματα-εναλλαγή ανάληψη τη σωστή σύνδεση συμβολοσειράς για την ενότητα "Ειδοποίηση" και προσθέστε όριο αναγνώρισης για να στείλετε την ειδοποίηση μόνο όταν ο χρήστης είναι εντός του geofence.

Για να ρυθμίσετε τη συμβολοσειρά σύνδεσης, στο το `Models` ανοιχτό φάκελο `Notifications.cs`. Το `NotificationHubClient.CreateClientFromConnectionString` συνάρτηση θα πρέπει να περιέχει τις πληροφορίες σχετικά με το Κέντρο σας ειδοποίησης που μπορείτε να μεταβείτε στην [Πύλη του Azure](https://portal.azure.com) (Εμφάνιση εντός του blade **Πολιτικές πρόσβασης** στις **Ρυθμίσεις**). Αποθηκεύστε το αρχείο ρύθμισης παραμέτρων ενημερωμένο.

Τώρα πρέπει να δημιουργήσετε ένα μοντέλο για το αποτέλεσμα API χάρτες Bing. Ο ευκολότερος τρόπος για να το κάνετε που είναι κάντε δεξί κλικ στη το `Models` φακέλου, **Προσθήκη** > **τάξης**. Ονομάστε το `GeofenceBoundary.cs`. Μετά την ολοκλήρωση, αντιγράψτε το JSON από την απάντηση API που αναφέρθηκε στην πρώτη ενότητα και στο Visual Studio χρήση **Επεξεργασία** > **Ειδική επικόλληση** > **JSON Επικόλληση ως κλάσεις**. 

Με αυτόν τον τρόπο που θα σας βεβαιωθείτε ότι το αντικείμενο θα αποσειριοποιηθούν ακριβώς όπως επρόκειτο. Το σύνολο που προκύπτει τάξης πρέπει να μοιάζει με αυτό:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Στη συνέχεια, ανοίξτε `Controllers`  >  `NotificationsController.cs`. Πρέπει να τροποποιήστε την κλήση δημοσίευση στο λογαριασμό για το στόχο γεωγραφικού πλάτους και μήκους. Για αυτό, απλώς προσθέστε δύο συμβολοσειρών στην υπογραφή συνάρτησης – `latitude` και `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Δημιουργία μιας νέας κλάσης μέσα στο έργο που ονομάζεται `ApiHelper.cs` – θα χρησιμοποιήσουμε το για να συνδεθείτε στο Bing για να ελέγξετε οδηγούν διασταυρώσεις όριο. Εφαρμογή ενός `IsPointWithinBounds` συνάρτηση, ως εξής:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Βεβαιωθείτε ότι για να αντικαταστήσετε το τελικό σημείο API με τη διεύθυνση URL ερωτήματος που λάβατε νωρίτερα από το Κέντρο ανάπτυξης Bing (ίδιο ισχύει για το πλήκτρο API). 

Εάν υπάρχουν αποτελέσματα στο ερώτημα, που σημαίνει ότι το συγκεκριμένο σημείο είναι εντός των ορίων του geofence, έτσι θα σας επιστρέφει `true`. Εάν δεν υπάρχουν αποτελέσματα, Bing που σας ενημερώνει μας ότι είναι το σημείο έξω από το πλαίσιο αναζήτησης, έτσι θα σας επιστρέφει `false`.

Πίσω στο `NotificationsController.cs`, δημιουργήστε ένα σημάδι ελέγχου προς τα δεξιά πριν από την πρόταση αλλαγής:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Με αυτόν τον τρόπο, την ειδοποίηση αποστέλλεται μόνο όταν το σημείο είναι εντός των ορίων του.

##<a name="testing-push-notifications-in-the-uwp-app"></a>Δοκιμές ειδοποιήσεων push στην εφαρμογή UWP

Επιστροφή στην εφαρμογή UWP, θα σας τώρα πρέπει να μπορείτε να ελέγξετε τις ειδοποιήσεις. Εντός του `LocationHelper` κλάση, δημιουργήστε μια νέα συνάρτηση – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Εναλλαγή του `POST_URL` στη θέση της εφαρμογής σας ανεπτυγμένος web που δημιουργήσαμε στην προηγούμενη ενότητα. Προς το παρόν, είναι καλό να εκτελείται τοπικά, αλλά καθώς εργάζεστε σε μια δημόσια έκδοση για την ανάπτυξη, θα πρέπει να φιλοξενήσετε με μια εξωτερική υπηρεσία παροχής.

Ας βεβαιωθείτε ότι θα σας καταχώρηση της εφαρμογής UWP για τις ειδοποιήσεις προώθησης. Στο Visual Studio, κάντε κλικ στο **έργο** > **Αποθήκευση** > **συσχετίσετε εφαρμογή με το χώρο αποθήκευσης**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Αφού πραγματοποιήσετε είσοδο στο λογαριασμό σας για προγραμματιστές, βεβαιωθείτε ότι επιλέγετε μια υπάρχουσα εφαρμογή ή δημιουργήστε ένα νέο και συσχετίσετε το πακέτο με αυτό. 

Μεταβείτε στο κέντρο ανάπτυξης και ανοίξτε την εφαρμογή που μόλις δημιουργήσατε. Κάντε κλικ στην επιλογή **υπηρεσίες** > **Ειδοποιήσειςπροώθησης** > **τοποθεσία Live υπηρεσιών**.

![](./media/notification-hubs-geofence/ms-live-services.png)

Στην τοποθεσία, σημειώστε το **Μυστικό εφαρμογής** και το **Αναγνωριστικό του πακέτου ΑΣΦΑΛΕΊΑΣ**. Θα πρέπει τόσο στην πύλη του Azure –, ανοίξτε το Κέντρο ειδοποίηση σας, κάντε κλικ στο στοιχείο **Ρυθμίσεις** > **Υπηρεσίες ειδοποίησης** > **Των Windows (WNS)** και εισαγάγετε τις πληροφορίες στα απαιτούμενα πεδία.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Κάντε κλικ στην εντολή **Αποθήκευση**.

Κάντε δεξί κλικ στην **αναφορές** στην **Εξερεύνηση λύσεων** και επιλέξτε **Διαχείριση πακέτων NuGet**. Θα πρέπει να προσθέσετε μια αναφορά στο **Microsoft Azure Service Bus διαχειριζόμενων βιβλιοθήκη** – απλώς αναζητήστε `WindowsAzure.Messaging.Managed` και να την προσθέσετε στο έργο σας.

![](./media/notification-hubs-geofence/vs-nuget.png)

Για σκοπούς δοκιμής, μπορούμε να δημιουργήσουμε το `MainPage_Loaded` πάλι το πρόγραμμα χειρισμού συμβάντων και να προσθέσετε το τμήμα κώδικα:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Τα παραπάνω καταγράφει την εφαρμογή με την ενότητα "Ειδοποίηση". Είστε έτοιμοι να ξεκινήσετε! 

Στο `LocationHelper`, εσωτερικά το `Geolocator_PositionChanged` πρόγραμμα χειρισμού, μπορείτε να προσθέσετε ένα τμήμα κώδικα έλεγχο που θα τοποθετήσετε αναγκαστικά στη θέση εντός του geofence:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Επειδή δεν διέρχεται η πραγματική συντεταγμένες (το οποίο ενδέχεται να μην είναι εντός των ορίων του τη στιγμή) και χρησιμοποιούν τιμές προκαθορισμένες δοκιμής, θα σας θα δείτε μια ειδοποίηση που εμφανίζεται κατά την ενημέρωση:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##<a name="whats-next"></a>Τι ακολουθεί;

Υπάρχουν μερικά βήματα που ίσως χρειαστεί να ακολουθήσετε εκτός από τα παραπάνω για να βεβαιωθείτε ότι η λύση είναι έτοιμη για παραγωγής.

Πρώτα και κύριο λόγο, ίσως χρειαστεί να βεβαιωθείτε ότι είναι δυναμική geofences. Αυτό θα απαιτούν επιπλέον εργασίες με το API Bing προκειμένου να έχουν τη δυνατότητα να αποστείλετε νέα όρια εντός την υπάρχουσα προέλευση δεδομένων. Ανατρέξτε στην [τεκμηρίωση Bing χώρου δεδομένων API των υπηρεσιών](https://msdn.microsoft.com/library/ff701734.aspx) για περισσότερες λεπτομέρειες σχετικά με το θέμα.

Δεύτερον, καθώς εργάζεστε για να βεβαιωθείτε ότι έχει ολοκληρωθεί η παράδοση στους συμμετέχοντες δεξιά, ενδέχεται να θέλετε να στοχεύσετε τους μέσω [προσθήκης ετικετών](notification-hubs-tags-segment-push-message.md).

Η λύση φαίνεται παραπάνω περιγράφει ένα σενάριο στον οποίο μπορεί να έχετε μια μεγάλη ποικιλία πλατφόρμες προορισμού, ώστε να σας δεν έχει αλλάξει περιορίζει το geofencing δυνατότητες αφορούν ειδικά το σύστημα. Με την ευκαιρία, της καθολικής πλατφόρμας Windows προσφέρει δυνατότητες για τον [εντοπισμό geofences δεξιά από το-πρώτης](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).

Για περισσότερες λεπτομέρειες σχετικά με τις δυνατότητες διανομείς ειδοποίησης, ανατρέξτε στο θέμα μας [τεκμηρίωση πύλη](https://azure.microsoft.com/documentation/services/notification-hubs/).
