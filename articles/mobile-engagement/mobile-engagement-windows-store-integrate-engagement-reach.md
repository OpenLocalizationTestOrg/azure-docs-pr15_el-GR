<properties 
    pageTitle="Ενοποίηση SDK καθολικής Reach εφαρμογές των Windows" 
    description="Πώς μπορείτε να ενοποιήσετε Reach Azure δέσμευση κινητές συσκευές με καθολική εφαρμογές των Windows"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="windows-universal-apps-reach-sdk-integration"></a>Ενοποίηση SDK καθολικής Reach εφαρμογές των Windows

Πρέπει να ακολουθήσετε τη διαδικασία ενοποίησης που περιγράφεται σε την [Ενοποίηση καθολικής SDK δέσμευση του Windows](mobile-engagement-windows-store-integrate-engagement.md) πριν να ακολουθήσετε αυτόν τον οδηγό.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>Ενσωμάτωση του SDK επίτευξη δέσμευση στην καθολική Windows το έργο σας

Δεν έχετε κάτι για να προσθέσετε. `EngagementReach`αναφορές και πηγές βρίσκονται ήδη στο έργο σας.

> [AZURE.TIP] Μπορείτε να προσαρμόσετε τις εικόνες που βρίσκονται σε το `Resources` φάκελο του έργου σας, ειδικά το εικονίδιο της εμπορικής επωνυμίας (αυτή την προεπιλογή στο εικονίδιο δέσμευση). Στην καθολική εφαρμογές μπορείτε επίσης να μετακινήσετε το `Resources` φακέλου στο κοινόχρηστο έργο σας για να μοιραστείτε το περιεχόμενο μεταξύ των εφαρμογών, αλλά θα πρέπει να διατηρήσετε το `Resources\EngagementConfiguration.xml` αρχείων στην προεπιλεγμένη θέση, καθώς πρόκειται για πλατφόρμα εξαρτώμενα.

## <a name="enable-the-windows-notification-service"></a>Ενεργοποιήστε την υπηρεσία ειδοποιήσεων των Windows

### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x και Windows Phone 8.1 μόνο

Για να χρησιμοποιήσετε την **Υπηρεσία ειδοποιήσεων των Windows** (που αναφέρονται ως WNS) στο σας `Package.appxmanifest` αρχείων στο `Application UI` κάντε κλικ στην `All Image Assets` στο πλαίσιο αριστερό bot. Στα δεξιά του πλαισίου στο `Notifications`, αλλαγή `toast capable` από `(not set)` να `(Yes)`.

### <a name="all-platforms"></a>Όλες τις πλατφόρμες

Πρέπει να συγχρονίσετε την εφαρμογή σας με το λογαριασμό Microsoft και για να την πλατφόρμα δέσμευση. Για αυτό πρέπει να δημιουργήσετε ένα λογαριασμό ή να συνδεθείτε στο [Κέντρο ανάπτυξης των windows](https://dev.windows.com). Μετά από αυτό, Δημιουργία νέας εφαρμογής και βρείτε το αναγνωριστικό ΑΣΦΑΛΕΊΑΣ μυστικό κλειδί. Στην frontend τη δέσμευση, μεταβείτε στη ρύθμιση σας εφαρμογή στο `native push` και επικολλήστε τα διαπιστευτήριά σας. Μετά από αυτό, κάντε δεξί κλικ στο έργο σας, επιλέξτε `store` και `Associate App with the Store...`. Πρέπει απλώς να επιλέξτε την εφαρμογή που δημιουργήσατε πριν από την για να συγχρονίσετε.

## <a name="initialize-the-engagement-reach-sdk"></a>Προετοιμασία του Reach δέσμευση SDK

Τροποποίηση του `App.xaml.cs`:

-   Εισαγωγή `EngagementReach.Instance.Init` μόνο αφού `EngagementAgent.Instance.Init` στο σας `InitEngagement` μέθοδο:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
          EngagementReach.Instance.Init(e);
        }

    Το `EngagementReach.Instance.Init` εκτελείται σε ένα νήμα αποκλειστικό. Δεν χρειάζεται να το κάνετε εσείς.

> [AZURE.NOTE] Εάν χρησιμοποιείτε τις ειδοποιήσεις push σε κάποιο άλλο σημείο στην εφαρμογή σας, στη συνέχεια, πρέπει να [κάνουν κοινή χρήση του καναλιού push](#push-channel-sharing) με επίτευξη δέσμευση.

## <a name="integration"></a>Ενοποίηση

Δέσμευση παρέχει δύο τρόπους για να προσθέσετε το πανό της εφαρμογής Reach και interstitial προβολές για ανακοινώσεις και ψηφοφορίες στην εφαρμογή σας: την ενοποίηση επικάλυψη και την ενοποίηση μη αυτόματη προβολές web. Που δεν θα πρέπει να συνδυάσετε δύο προσέγγιση στην ίδια σελίδα.

Η επιλογή μεταξύ δύο ενσωμάτωση θα μπορούσε να συνοψίζονται αυτόν τον τρόπο:

-   Μπορείτε να επιλέξετε την ενοποίηση επικάλυψη εάν τις σελίδες σας ήδη μεταβιβάζονται από τον παράγοντα `EngagementPage`, είναι απλώς θέμα αντικατάστασης `EngagementPage` με `EngagementPageOverlay` και `xmlns:engagement="using:Microsoft.Azure.Engagement"` με `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` στις σελίδες σας.
-   Μπορείτε να επιλέξετε τις προβολές web μη αυτόματη ενοποίησης εάν θέλετε να τοποθετήσετε επακριβώς φτάσει στο περιβάλλον εργασίας Χρήστη του μέσα σε σελίδες σας ή εάν δεν θέλετε να προσθέσετε ένα άλλο επίπεδο μεταβίβασης στις σελίδες σας. 

### <a name="overlay-integration"></a>Ενοποίηση επικάλυψη

Η δέσμευση επικάλυψη προσθέτει δυναμικά τα στοιχεία περιβάλλοντος εργασίας Χρήστη που χρησιμοποιούνται για να εμφανίσετε εκστρατείες Reach στη σελίδα σας. Εάν το επικάλυψης δεν ανταποκρίνονται διάταξή σας θα πρέπει να τις προβολές web μη αυτόματη ενοποίηση αντί για αυτό.

Στο την αλλαγή του αρχείου .xaml `EngagementPage` αναφοράς`EngagementPageOverlay`

-   Προσθέστε τις δηλώσεις πεδία ονομάτων:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

-   Αντικατάσταση `engagement:EngagementPage` με `engagement:EngagementPageOverlay`:

**Με EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
        
            <!-- Your layout -->
        </engagement:EngagementPage>

**Με EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">
        
            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Στη συνέχεια, στο αρχείο σας .cs ετικέτα σας σελίδα στο `EngagementPageOverlay` αντί για `EngagementPage` και εισαγωγή `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

-   Αντικατάσταση `EngagementPage` με `EngagementPageOverlay`:

**Με EngagementPage:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**Με EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Δέσμευση επικάλυψης προσθέτει μια `Grid` στοιχείο επάνω από τη σελίδα σας που αποτελείται από τη διάταξη και τις δύο `WebView` στοιχεία μία για διαφημιστικό πλαίσιο και το άλλο για την προβολή interstitial.

Μπορείτε να προσαρμόσετε τα στοιχεία επικάλυψης απευθείας στο το `EngagementPageOverlay.cs` αρχείου.

### <a name="web-views-manual-integration"></a>Προβολές μη αυτόματη ενοποίηση με το Web

Reach θα πραγματοποιείτε αναζήτηση τις σελίδες σας για τις δύο `WebView` στοιχεία που είναι υπεύθυνος για την εμφάνιση του πλαισίου και την προβολή interstitial. Το μόνο που έχετε να κάνετε είναι να προσθέσετε αυτές τις δύο `WebView` στοιχεία κάπου στις σελίδες σας, ακολουθεί ένα παράδειγμα:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


Σε αυτό το παράδειγμα το `WebView` στοιχεία έχουν επεκταθεί για να χωρέσει το κοντέινερ που αλλάζει αυτόματα εκ νέου το μέγεθος τους σε περίπτωση οθόνης παράθυρο περιστροφής ή αλλαγή μεγέθους.

> [AZURE.WARNING] Είναι σημαντικό να διατηρήσετε το ίδιο ονομάτων `engagement_notification_content` και `engagement_announcement_content` για το `WebView` στοιχεία. Reach Αναγνωρίζοντας τις από το όνομα του ατόμου. 

## <a name="handle-datapush-optional"></a>Λαβή datapush (προαιρετικά)

Εάν θέλετε την εφαρμογή σας να μπορούν να λαμβάνουν Reach ωθεί δεδομένων, πρέπει να υλοποιήσετε δύο συμβάντα της κλάσης EngagementReach:

Στο App.xaml.cs στην κατασκευή App() Προσθήκη:

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };
            
            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

Μπορείτε να δείτε ότι η επιστροφή κλήσης κάθε μεθόδου επιστρέφει μια τιμή boolean. Δέσμευση στέλνει σχολίων για να το παρασκηνίου μετά την αποστολή του push δεδομένων. Εάν η επιστροφή κλήσης επιστρέφει false, η `exit` σχολίων θα αποστολή. Διαφορετικά, θα `action`. Εάν δεν υπάρχει επιστροφής κλήσης έχει οριστεί για τα συμβάντα, τα `drop` σχολίων θα επιστρέψετε δέσμευση.

> [AZURE.WARNING] Δέσμευση δεν είναι δυνατό να λαμβάνετε πολλαπλάσια σχολίων για ένα push δεδομένων. Εάν σκοπεύετε να ορίσετε διάφορες προγράμματα χειρισμού σε ένα συμβάν, λάβετε υπόψη ότι τα σχόλιά σας θα αντιστοιχούν στο τελευταίο μία αποστέλλονται. Σε αυτήν την περίπτωση, συνιστάται να επιστρέφει πάντα την ίδια τιμή για να αποφύγετε τη σύγχυση σχολίων σε του προσκηνίου.

## <a name="customize-ui-optional"></a>Προσαρμογή περιβάλλοντος εργασίας Χρήστη (προαιρετικό)

### <a name="first-step"></a>Πρώτο βήμα

Θα σας επιτρέπουν να προσαρμόσετε το reach περιβάλλοντος εργασίας Χρήστη.

Για να γίνει αυτό, πρέπει να δημιουργήσετε μια υποκατηγορία το `EngagementReachHandler` τάξης.

**Δείγμα κώδικα:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Στη συνέχεια, ορίστε το περιεχόμενο του το `EngagementReach.Instance.Handler` πεδίο με το προσαρμοσμένο αντικείμενο στο σας `App.xaml.cs` κλάση εντός το `App()` μέθοδο.

**Δείγμα κώδικα:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [AZURE.NOTE]Από προεπιλογή, δέσμευση χρησιμοποιεί τη δική του υλοποίηση `EngagementReachHandler`.
> Δεν χρειάζεται να δημιουργήσετε τις δικές σας και, εάν το κάνετε αυτό, δεν χρειάζεται να παρακάμψετε κάθε μέθοδο. Η προεπιλεγμένη συμπεριφορά είναι να επιλέξετε το αντικείμενο βάσης δέσμευση.

### <a name="web-view"></a>Προβολή Web

Από προεπιλογή, Reach θα χρησιμοποιήσει το ενσωματωμένο πόροι του DLL για να εμφανίσετε τις ειδοποιήσεις και τις σελίδες.

Για να δώσετε μια πλήρη προσαρμογή δυνατότητες χρησιμοποιούμε μόνο προβολή web. Εάν θέλετε να προσαρμόσετε τις διατάξεις, απευθείας παρακάμψετε τα αρχεία πόρους `EngagementAnnouncement.html` και `EngagementNotification.html`. Δέσμευση πρέπει όλο τον κώδικα στο `<body></body>` για να εκτελέσετε σωστά. Αλλά μπορείτε να προσθέσετε ετικέτα εξωτερικό `engagement_webview_area`.

Ωστόσο, μπορείτε να αποφασίσετε να χρησιμοποιήσετε τη δική σας πόρους.

Μπορείτε να παρακάμψετε `EngagementReachHandler` μεθόδων στο σας υποκατηγορίας για να υποδείξετε δέσμευση για να χρησιμοποιήσετε τις διατάξεις, αλλά φροντίστε να ενσωματωθεί το μηχανισμό δέσμευση:

**Δείγμα κώδικα:**
            
            // In your subclass of EngagementReachHandler
            
            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Από προεπιλογή, είναι AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`. Αντιπροσωπεύει το αρχείο html που σχεδίαση του περιεχομένου ενός μηνύματος push (ανακοίνωση κειμένου, Web anoucement και ανακοίνωση ψηφοφορίας). Είναι AnnouncementName `engagement_announcement_content`. Είναι το όνομα της σχεδίασης της προβολής Web στη σελίδα σας xaml.

Είναι NotfificationHTML `ms-appx-web:///Resources/EngagementNotification.html`. Αντιπροσωπεύει το αρχείο html που σχεδίαση την ειδοποίηση μηνύματος push. Είναι NotfificationName `engagement_notification_content`. Είναι το όνομα της σχεδίασης της προβολής Web στη σελίδα σας xaml.

### <a name="customization"></a>Προσαρμογή

Μπορείτε να προσαρμόσετε ειδοποίηση και ανακοίνωση για προβολή web περιλαμβάνει που θέλετε εάν μπορείτε να διατηρήσετε το αντικείμενο δέσμευση. Να είστε προσεκτικοί που προβολής Web αντικείμενο περιγράφεται τρεις φορές - την πρώτη φορά σε σας xaml δεύτερη φορά στο αρχείο σας .cs στη μέθοδο "setwebview()" και τρίτη φορά στο αρχείο html.

-   Στο σας xaml που περιγράφουν το τρέχον στοιχείο προβολής Web διάταξης γραφικών.
-   Στο αρχείο σας .cs μπορείτε να ορίσετε "setwebview()" με την οποία μπορείτε να ορίσετε τις διαστάσεις των δύο προβολής Web (ειδοποίηση, ανακοίνωση). Είναι πολύ αποτελεσματική κατά την αλλαγή του μεγέθους της εφαρμογής.
-   Στο αρχείο html δέσμευση περιγραφή του περιεχομένου της προβολής Web, σχεδίαση και τις θέσεις στοιχεία μεταξύ μεταξύ τους.

### <a name="launch-message"></a>Εκκίνηση μηνύματος

Όταν ένας χρήστης κάνει κλικ σε μια ειδοποίηση σύστημα (μια αναδυόμενη), δέσμευση ενεργοποιεί την εφαρμογή, φορτώσετε το περιεχόμενο των μηνυμάτων push και να εμφανίσετε τη σελίδα για την αντίστοιχη εκστρατείας.

Υπάρχει μια καθυστέρηση μεταξύ την εκκίνηση της εφαρμογής και της εμφάνισης της σελίδας (ανάλογα με την ταχύτητα του δικτύου σας).

Για να υποδείξετε στο χρήστη ότι γίνεται φόρτωση κάτι, θα πρέπει να παρέχουν μια οπτική πληροφοριών, όπως μια γραμμή προόδου ή μια ένδειξη προόδου. Δέσμευση δεν μπορεί να χειριστεί μόνη της, αλλά παρέχει ορισμένα προγράμματα χειρισμού για εσάς.

Για να υλοποιήσετε την επιστροφή κλήσης, στο App.xaml.cs στο "Δημόσια App() {}" Προσθήκη:

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
            
            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
            
            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Μπορείτε να ορίσετε την επιστροφή κλήσης στο τη μέθοδο "Δημόσια App() {}" της σας `App.xaml.cs` αρχείο, κατά προτίμηση πριν από την `EngagementReach.Instance.Init()` κλήσεων.

> [AZURE.TIP] Κάθε πρόγραμμα χειρισμού καλείται από το περιβάλλον εργασίας Χρήστη νήματος. Δεν χρειάζεται να ανησυχείτε κατά τη χρήση MessageBox ή κάτι περιβάλλοντος εργασίας Χρήστη που σχετίζονται με.

##<a id="push-channel-sharing"></a>Ειδοποιήσεις Push κοινή χρήση καναλιών

Εάν χρησιμοποιείτε τις ειδοποιήσεις push για κάποιον άλλο σκοπό στην εφαρμογή σας, στη συνέχεια, πρέπει να χρησιμοποιήσετε το κανάλι push δυνατότητα του SDK δέσμευση κοινής χρήσης. Πρόκειται για να αποφύγετε αναπάντητες push.

- Μπορείτε να παρέχετε τις δικές σας καναλιού push για την επίτευξη δέσμευση προετοιμασία. Το SDK θα χρησιμοποιήσετε αντί να ζητά ένα νέο.

Ενημέρωση της προετοιμασίας επίτευξη δέσμευση με το κανάλι push στο το `InitEngagement` μέθοδο από το `App.xaml.cs` αρχείο:
    
    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

- Εναλλακτικά, εάν θέλετε απλώς να εκμετάλλευση το κανάλι push μετά την προετοιμασία Reach, στη συνέχεια, μπορείτε να ορίσετε μια επιστροφή κλήσης σε δέσμευση επίτευξη για να λάβετε το κανάλι push μία φορά δημιουργείται από το SDK.

Ορίστε την επιστροφή κλήσης σε οποιοδήποτε σημείο **μετά** την προετοιμασία Reach:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Συμβουλή προσαρμοσμένου συνδυασμού

Παρέχουμε χρήση προσαρμοσμένου συνδυασμού. Μπορείτε να στείλετε διαφορετικό τύπο URI από frontend δέσμευση να χρησιμοποιηθεί με την εφαρμογή σας δέσμευση. Προεπιλεγμένο συνδυασμό όπως `http, ftp, ...` είναι διαχείριση από τα Windows, ένα παράθυρο θα προτροπή αν δεν έχει εγκατασταθεί στη συσκευή προεπιλεγμένη εφαρμογή. Μπορείτε επίσης να δημιουργήσετε το δικό σας προσαρμοσμένο συνδυασμό για την εφαρμογή σας.

Απλό τρόπο για να ορίσετε έναν προσαρμοσμένο συνδυασμό στην εφαρμογή σας είναι να ανοίξετε το `Package.appxmanifest` μεταβείτε στο `Declarations` πίνακα. Επιλέξτε `Protocol` στις διαθέσιμες δηλώσεις κύλιση πλαίσιο και να την προσθέσετε. Επεξεργασία του `Name` πεδίο με το νέο πρωτόκολλο το επιθυμητό όνομα.

Τώρα για να χρησιμοποιήσετε αυτό το πρωτόκολλο, επεξεργαστείτε το `App.xaml.cs` με το `OnActivated` μέθοδος και μην ξεχάσετε να προετοιμασία δέσμευση δείτε επίσης:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);
            
              //TODO design action to do when app is launch
            
              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;
            
                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion
 
