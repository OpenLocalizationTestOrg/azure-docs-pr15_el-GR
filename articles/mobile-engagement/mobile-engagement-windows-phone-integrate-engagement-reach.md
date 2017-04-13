<properties 
    pageTitle="Windows Phone ενοποίησης SDK Silverlight Reach" 
    description="Πώς μπορείτε να ενοποιήσετε Reach Azure δέσμευση κινητές συσκευές με Windows Phone Silverlight εφαρμογές"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone ενοποίησης SDK Silverlight Reach

Πρέπει να ακολουθήσετε τη διαδικασία ενοποίησης που περιγράφεται σε [Windows Phone Silverlight δέσμευση SDK ενοποίησης](mobile-engagement-windows-phone-integrate-engagement.md) πριν να ακολουθήσετε αυτόν τον οδηγό.

##<a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>Ενσωμάτωση του SDK επίτευξη δέσμευση στο έργο σας Windows Phone Silverlight

Δεν έχετε κάτι για να προσθέσετε. `EngagementReach`αναφορές και πηγές βρίσκονται ήδη στο έργο σας.

> [AZURE.TIP]  Μπορείτε να προσαρμόσετε τις εικόνες που βρίσκονται σε το `Resources` φάκελο του έργου σας, ειδικά το εικονίδιο της εμπορικής επωνυμίας (αυτή την προεπιλογή στο εικονίδιο δέσμευση).

##<a name="add-the-capabilities"></a>Προσθέστε τις δυνατότητες

Το SDK επίτευξη δέσμευση πρέπει ορισμένες πρόσθετες δυνατότητες.

Άνοιγμα του `WMAppManifest.xml` αρχείου και βεβαιωθείτε ότι έχουν δηλωθεί τις ακόλουθες δυνατότητες:

-   `ID_CAP_PUSH_NOTIFICATION`
-   `ID_CAP_WEBBROWSERCOMPONENT`

Το πρώτο χρησιμοποιείται από την υπηρεσία MPNS για να επιτρέψετε την εμφάνιση των αναδυόμενη ειδοποίηση. Στο δεύτερο παράδειγμα χρησιμοποιείται για να ενσωματώσετε μια εργασία του προγράμματος περιήγησης σε SDK.

Επεξεργασία του `WMAppManifest.xml` αρχείων και να προσθέσετε μέσα το `<Capabilities />` ετικέτα:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

##<a name="enable-the-microsoft-push-notification-service"></a>Ενεργοποιήστε την υπηρεσία ειδοποιήσεων Push της Microsoft

Για να χρησιμοποιήσετε την **Υπηρεσία ειδοποιήσεων Push της Microsoft** (αναφέρονται ως MPNS) σας `WMAppManifest.xml` αρχείο πρέπει να έχει ένα `<App />` ετικέτα με ένα `Publisher` χαρακτηριστικό ορίστε το όνομα του έργου σας.

##<a name="initialize-the-engagement-reach-sdk"></a>Προετοιμασία του Reach δέσμευση SDK

### <a name="engagement-configuration"></a>Ρύθμιση παραμέτρων δέσμευση

Η ρύθμιση παραμέτρων δέσμευση είναι συγκεντρωμένα σε το `Resources\EngagementConfiguration.xml` αρχείο του έργου σας.

Επεξεργαστείτε αυτό το αρχείο για να καθορίσετε reach ρύθμισης παραμέτρων:

-   *Προαιρετικά*, υποδηλώνουν εάν τα εγγενή push (MPNS) είναι ενεργοποιημένη ή όχι μεταξύ `<enableNativePush>` και `</enableNativePush>` ετικετών, (`true` από προεπιλογή).
-   *Προαιρετικά*, υποδεικνύουν το όνομα του καναλιού push μεταξύ `<channelName>` και `</channelName>` ετικετών, δώστε το ίδιο που την εφαρμογή σας μπορεί να χρησιμοποιήσετε ή αφήστε κενό αυτήν τη στιγμή.

Εάν θέλετε να καθορίσετε κατά το χρόνο εκτέλεσης, μπορείτε να καλέσετε την ακόλουθη μέθοδο πριν από την προετοιμασία παράγοντας δέσμευση:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */
    
    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [AZURE.TIP] Μπορείτε να καθορίσετε το όνομα του καναλιού MPNS push της εφαρμογής σας. Από προεπιλογή, η δέσμευση δημιουργεί ένα όνομα με βάση το αναγνωριστικό εφαρμογής. Δεν χρειάζεται να καθορίσετε το όνομα στον εαυτό σας, εκτός εάν σκοπεύετε να χρησιμοποιήσετε το κανάλι push εκτός της δέσμευσης.

### <a name="engagement-initialization"></a>Προετοιμασία δέσμευση

Τροποποίηση του `App.xaml.cs`:

-   Προσθήκη για να σας `using` προτάσεις:

        using Microsoft.Azure.Engagement;

-   Εισαγωγή `EngagementReach.Instance.Init` μόνο αφού `EngagementAgent.Instance.Init` στο `Application_Launching` :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

-   Εισαγωγή `EngagementReach.Instance.OnActivated` στο το `Application_Activated` μέθοδο:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

> [AZURE.IMPORTANT] Το `EngagementReach.Instance.Init` εκτελείται σε ένα νήμα αποκλειστικό. Δεν χρειάζεται να το κάνετε εσείς.

##<a name="app-store-submission-considerations"></a>Ζητήματα υποβολής App store

Microsoft επιβάλλει ορισμένοι κανόνες όταν χρησιμοποιείτε τις ειδοποιήσεις push:

Από την τεκμηρίωση Microsoft [Πολιτικές εφαρμογής] , ενότητα 2.9:

1) Πρέπει να ζητήσετε από το χρήστη για να αποδεχτείτε να λαμβάνουν ειδοποιήσεις προώθησης. Στη συνέχεια, στις ρυθμίσεις σας, προσθέστε έναν τρόπο για να απενεργοποιήσετε τις ειδοποιήσεις push.

Το αντικείμενο EngagementReach παρέχει δύο μεθόδους για να διαχειριστείτε το συμμετοχής στο/συμμετοχής ανάληψης, `EnableNativePush()` και `DisableNativePush()`. Για παράδειγμα, μπορεί να, δημιουργείτε μια επιλογή στις ρυθμίσεις με ένα κουμπί εναλλαγής για απενεργοποίηση ή ενεργοποίηση MPNS.

Μπορείτε επίσης να αποφασίσετε να απενεργοποιήσετε MPNS μέσω της ρύθμισης παραμέτρων δέσμευση\<windows phone-sdk-reach-ρύθμιση παραμέτρων\>.

> 2.9.1) την εφαρμογή πρέπει να πρώτα περιγράφουν τις ειδοποιήσεις που παρέχονται και **αποκτήσετε ρητή άδεια του χρήστη (επιλέξετε στο)**, και **πρέπει να παρέχουν ένα μηχανισμό μέσω του οποίου ο χρήστης μπορεί να εξαιρεθείτε από λαμβάνει τις ειδοποιήσεις προώθησης**. Όλες τις ειδοποιήσεις που παρέχονται με την υπηρεσία ειδοποιήσεων Push της Microsoft, πρέπει να είναι συνεπείς με την περιγραφή που παρέχονται στο χρήστη και πρέπει να συμμορφώνονται με όλες τις ισχύουσες [Πολιτικές εφαρμογής]  [ Content Policies] και [Πρόσθετες απαιτήσεις για συγκεκριμένους τύπους εφαρμογών].

2) Δεν πρέπει να χρησιμοποιείτε τις ειδοποιήσεις push πάρα πολλά. Δέσμευση θα χειρίζεται τις ειδοποιήσεις για εσάς.

> 2.9.2) της εφαρμογής και η χρήση της υπηρεσίας ειδοποιήσεων Push του Microsoft πρέπει να δεν υπερβολικά Χρησιμοποιήστε δυναμικότητας δικτύου ή το εύρος ζώνης από την υπηρεσία ειδοποιήσεων Push της Microsoft, ή αλλιώς αδικαιολόγητα βαρύνουν υπερβολικά ένα Windows Phone ή άλλη συσκευή Microsoft ή υπηρεσία με τις ειδοποιήσεις push πλεονάζουσα, όπως προσδιορίζεται από τη Microsoft στο την αποκλειστική διακριτική λογικό ευχέρεια, και πρέπει να δεν βλάβη ή παρεμβάλλεται με οποιαδήποτε δίκτυα της Microsoft ή των διακομιστών ή οποιαδήποτε διακομιστές άλλων κατασκευαστών ή δίκτυα συνδεδεμένο με την υπηρεσία ειδοποιήσεων Push της Microsoft.

3) Βασίζεστε στη MPNS για την αποστολή πληροφοριών criticals. Δέσμευση χρησιμοποιεί MPNS, επομένως, αν αυτός ο κανόνας ισχύει επίσης για τις καμπάνιες δημιουργήθηκε μέσα την πρόσληψη προσκηνίου.

> 2.9.3) της Microsoft Push υπηρεσία ειδοποιήσεων ενδέχεται να μην χρησιμοποιείται για την αποστολή ειδοποιήσεων που είναι αποστολής κρίσιμες ή με άλλο τρόπο μπορεί να επηρεάσει θέματα ζωής ή θανάτου, συμπεριλαμβανομένων των χωρίς τον περιορισμό κρίσιμων ειδοποιήσεις σχετίζονται με ένα ιατρική συσκευή ή συνθήκης. MICROSOFT ΑΠΟΠΟΙΕΊΤΑΙ ΡΗΤΆ ΑΠΌ ΟΠΟΙΑΔΉΠΟΤΕ ΕΓΓΥΗΣΗΣ ΌΤΙ ΧΡΗΣΙΜΟΠΟΙΕΊΤΕ ΤΗΝ ΥΠΗΡΕΣΊΑ ΕΙΔΟΠΟΙΉΣΕΩΝ PUSH ΤΗΣ MICROSOFT Ή ΠΑΡΆΔΟΣΗΣ ΤΩΝ ΕΙΔΟΠΟΙΉΣΕΩΝ ΥΠΗΡΕΣΊΑ ΕΙΔΟΠΟΙΉΣΕΩΝ PUSH ΤΗΣ MICROSOFT ΘΑ ΑΠΡΌΣΚΟΠΤΗ, ΕΓΓΥΗΜΈΝΗ ΣΦΆΛΜΑΤΟΣ ΔΩΡΕΆΝ, Ή ΑΛΛΙΏΣ ΝΑ ΠΑΡΟΥΣΙΆΖΕΤΑΙ ΜΕ ΒΆΣΗ ΤΗ ΣΕ ΠΡΑΓΜΑΤΙΚΌ ΧΡΌΝΟ.

**Θα σας δεν είναι δυνατό να εξασφαλίσετε ότι η εφαρμογή σας θα περάσουν τη διαδικασία επαλήθευσης εάν οι που δεν αυτές τις συστάσεις.**

##<a name="handle-data-push-optional"></a>Λαβή δεδομένων push (προαιρετικά)

Εάν θέλετε την εφαρμογή σας να μπορούν να λαμβάνουν Reach ωθεί δεδομένων, πρέπει να υλοποιήσετε δύο συμβάντα της κλάσης EngagementReach:

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

##<a name="customize-ui-optional"></a>Προσαρμογή περιβάλλοντος εργασίας Χρήστη (προαιρετικό)

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

Στη συνέχεια, ορίστε το περιεχόμενο του το `EngagementReach.Instance.Handler` πεδίο με το προσαρμοσμένο αντικείμενο στο σας `App.xaml.cs` κλάση εντός το `Application_Launching` μέθοδο.

**Δείγμα κώδικα:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [AZURE.NOTE] Από προεπιλογή, δέσμευση χρησιμοποιεί τη δική του υλοποίηση `EngagementReachHandler`. Δεν χρειάζεται να δημιουργήσετε τις δικές σας και, εάν το κάνετε αυτό, δεν χρειάζεται να παρακάμψετε κάθε μέθοδο. Η προεπιλεγμένη συμπεριφορά είναι να επιλέξετε το αντικείμενο βάσης δέσμευση.

### <a name="layouts"></a>Διατάξεις

Από προεπιλογή, Reach θα χρησιμοποιήσει το ενσωματωμένο πόροι του DLL για να εμφανίσετε τις ειδοποιήσεις και τις σελίδες.

Ωστόσο, μπορείτε να αποφασίσετε να χρησιμοποιήσετε τη δική σας πόρους για να απεικονίζει την εμπορική σας επωνυμία σε αυτά τα στοιχεία.

Μπορείτε να παρακάμψετε `EngagementReachHandler` μεθόδων στο σας υποκατηγορίας για να υποδείξετε δέσμευση για να χρησιμοποιήσετε τις διατάξεις:

**Δείγμα κώδικα:**

    // In your subclass of EngagementReachHandler
    
    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetPollUri()
    {
       // return the path of your own xaml
    }
    
    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [AZURE.TIP] Το `CreateNotification` μέθοδος μπορεί να επιστρέψει null. Την ειδοποίηση δεν θα εμφανιστεί και της εκστρατείας reach θα καταργηθεί.

Για να απλοποιήσετε υλοποίηση της διάταξης, παρέχουμε επίσης τη δική μας xaml που μπορεί να χρησιμοποιηθεί ως βάση για τον κωδικό. Βρίσκονται στο αρχείο αρχειοθέτησης SDK δέσμευση (/ src/reach /).

> [AZURE.WARNING] Οι πηγές που παρέχουμε είναι ακριβώς ίδια αυτά που χρησιμοποιούμε. Επομένως, εάν θέλετε να τροποποιήσετε απευθείας, μην ξεχάσετε να αλλάξετε χώρου ονομάτων και το όνομα του.

### <a name="notification-position"></a>Ειδοποίηση θέση

Από προεπιλογή, εμφανίζεται μια ειδοποίηση της εφαρμογής στην κάτω αριστερή πλευρά της εφαρμογής. Μπορείτε να αλλάξετε αυτήν τη συμπεριφορά, παρακάμπτοντας την `GetNotificationPosition` μέθοδο το `EngagementReachHandler` αντικειμένου.

    // In your subclass of EngagementReachHandler
    
    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Προς το παρόν, μπορείτε να επιλέξετε μεταξύ του `BOTTOM` (προεπιλογή) και `TOP` τις θέσεις.

### <a name="launch-message"></a>Εκκίνηση μηνύματος

Όταν ένας χρήστης κάνει κλικ σε μια ειδοποίηση σύστημα (μια αναδυόμενη), δέσμευση ενεργοποιεί την εφαρμογή, φορτώσετε το περιεχόμενο των μηνυμάτων push και να εμφανίσετε τη σελίδα για την αντίστοιχη εκστρατείας.

Υπάρχει μια καθυστέρηση μεταξύ την εκκίνηση της εφαρμογής και της εμφάνισης της σελίδας (ανάλογα με την ταχύτητα του δικτύου σας).

Για να υποδείξετε στο χρήστη ότι γίνεται φόρτωση κάτι, θα πρέπει να παρέχουν μια οπτική πληροφοριών, όπως μια γραμμή προόδου ή μια ένδειξη προόδου. Δέσμευση δεν μπορεί να χειριστεί μόνη της, αλλά παρέχει ορισμένα προγράμματα χειρισμού για εσάς.

Για να υλοποιήσετε την επιστροφή κλήσης, κάντε:

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

Μπορείτε να ορίσετε την επιστροφή κλήσης στο σας `Application_Launching` μέθοδο σας `App.xaml.cs` αρχείο, κατά προτίμηση πριν από την `EngagementReach.Instance.Init()` κλήσεων.

> [AZURE.TIP] Κάθε πρόγραμμα χειρισμού καλείται από το περιβάλλον εργασίας Χρήστη νήματος. Δεν χρειάζεται να ανησυχείτε κατά τη χρήση MessageBox ή κάτι περιβάλλοντος εργασίας Χρήστη που σχετίζονται με.

[Πολιτικές εφαρμογής]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[Πρόσθετες απαιτήσεις για τους τύπους συγκεκριμένης εφαρμογής]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx
 
