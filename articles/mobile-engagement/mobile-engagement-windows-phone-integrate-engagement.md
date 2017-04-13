<properties 
    pageTitle="Ενοποίηση SDK του Windows Phone Silverlight δέσμευση" 
    description="Πώς μπορείτε να ενοποιήσετε Azure δέσμευση κινητές συσκευές με Windows Phone Silverlight εφαρμογές"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-engagement-sdk-integration"></a>Ενοποίηση SDK του Windows Phone Silverlight δέσμευση

> [AZURE.SELECTOR] 
- [Παγκόσμια των Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Αυτή η διαδικασία περιγράφει ο πιο απλός τρόπος για να ενεργοποιήσετε το Azure Mobile δέσμευση ανάλυση και παρακολούθηση συναρτήσεις στην εφαρμογή Windows Phone Silverlight.

Τα ακόλουθα βήματα είναι αρκετά για να ενεργοποιήσετε την αναφορά των αρχείων καταγραφής χρειάζεται, για να υπολογίσετε όλα τα στατιστικά στοιχεία σχετικά με τους χρήστες, οι περίοδοι λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και Technicals. Η έκθεση της καταγραφής που είναι απαραίτητα για τον υπολογισμό άλλων στατιστικών στοιχείων όπως συμβάντα, σφάλματα και οι εργασίες πρέπει να γίνουν με μη αυτόματο τρόπο χρησιμοποιώντας το API δέσμευση (δείτε παρακάτω [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API στην εφαρμογή Silverlight Windows Phone](mobile-engagement-windows-phone-use-engagement-api.md) ) εφόσον αυτά τα στατιστικά στοιχεία έχουν εξαρτάται από την εφαρμογή.

##<a name="supported-versions"></a>Υποστηριζόμενες εκδόσεις

Δέσμευση SDK το Mobile για Windows Silverlight μπορεί μόνο να ενσωματώνει εφαρμογές στόχευσης:

-   Windows Phone 8.0
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Εάν στοχεύετε σε Windows Phone 8.1 (μη Silverlight), στη συνέχεια, ανατρέξτε στην [καθολική Windows διαδικασία ενοποίησης](mobile-engagement-windows-store-integrate-engagement.md).

##<a name="install-the-mobile-engagement-silverlight-sdk"></a>Εγκατάσταση του SDK Silverlight δέσμευση κινητές συσκευές

Δέσμευση SDK το Mobile για Windows Silverlight είναι διαθέσιμη ως ένα πακέτο Nuget που ονομάζεται *MicrosoftAzure.MobileEngagement*. Μπορείτε να το εγκαταστήσετε από το Visual Studio Nuget διαχείρισης πακέτου. 

##<a name="add-the-capabilities"></a>Προσθέστε τις δυνατότητες

Στο SDK δέσμευση χρειάζεται ορισμένες δυνατότητες του Windows Phone Silverlight SDK για να λειτουργήσει σωστά.

Άνοιγμα του `WMAppManifest.xml` αρχείου και βεβαιωθείτε ότι οι παρακάτω δυνατότητες δηλώνονται στο το `Capabilities` πίνακα:

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

##<a name="initialize-the-engagement-sdk"></a>Προετοιμασία την πρόσληψη SDK

### <a name="engagement-configuration"></a>Ρύθμιση παραμέτρων δέσμευση

Η ρύθμιση παραμέτρων δέσμευση είναι συγκεντρωμένα σε το `Resources\EngagementConfiguration.xml` αρχείο του έργου σας.

Επεξεργαστείτε αυτό το αρχείο για να καθορίσετε:

-   Η συμβολοσειρά σύνδεσης εφαρμογής μεταξύ ετικετών `<connectionString>` και `<\connectionString>`.

Εάν θέλετε να καθορίσετε κατά το χρόνο εκτέλεσης, μπορείτε να καλέσετε την ακόλουθη μέθοδο πριν από την προετοιμασία παράγοντας δέσμευση:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

Εμφανίζεται η συμβολοσειρά σύνδεσης για την εφαρμογή σας στην πύλη κλασική Azure.

### <a name="engagement-initialization"></a>Προετοιμασία δέσμευση

Όταν δημιουργείτε ένα νέο έργο, μια `App.xaml.cs` δημιουργείται αρχείο. Αυτή η κλάση μεταβιβάζονται από `Application` και περιέχει πολλές σημαντικές μεθόδους. Θα επίσης να χρησιμοποιηθούν για την προετοιμασία του SDK δέσμευση.

Τροποποίηση του `App.xaml.cs`:

-   Προσθήκη για να σας `using` προτάσεις:

        using Microsoft.Azure.Engagement;

-   Εισαγωγή `EngagementAgent.Instance.Init` στο το `Application_Launching` μέθοδο:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   Εισαγωγή `EngagementAgent.Instance.OnActivated` στο το `Application_Activated` μέθοδο:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }

> [AZURE.WARNING] Δεν συνιστάται να προσθέσετε την προετοιμασία δέσμευσης σε άλλο σημείο της εφαρμογής σας. Ωστόσο, έχετε υπόψη που το `EngagementAgent.Instance.Init` μέθοδο εκτελείται σε ένα νήμα αποκλειστικό και όχι στο νήμα περιβάλλοντος εργασίας Χρήστη.

##<a name="basic-reporting"></a>Βασική αναφορά

### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Προτεινόμενη μέθοδος: υπερφόρτωσης σας `PhoneApplicationPage` κλάσεις

Για να ενεργοποιήσετε την αναφορά της όλα τα αρχεία καταγραφής που απαιτούνται από δέσμευση για τον υπολογισμό χρηστών, οι περίοδοι λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και τεχνικές στατιστικά στοιχεία, μπορείτε να απλώς κάνετε όλων σας `PhoneApplicationPage` δευτερεύουσες κλάσεις μεταβιβάζονται από το `EngagementPage` τάξεις τους.

Ακολουθεί ένα παράδειγμα του τρόπου να το κάνετε αυτό για μια σελίδα της εφαρμογής σας. Μπορείτε να κάνετε το ίδιο για όλες τις σελίδες της εφαρμογής σας.

#### <a name="c-source-file"></a>Αρχείο προέλευσης C#

Τροποποίηση της σελίδας σας `.xaml.cs` αρχείο:

-   Προσθήκη για να σας `using` προτάσεις:

        using Microsoft.Azure.Engagement;

-   Αντικατάσταση `PhoneApplicationPage` με `EngagementPage` :

**Χωρίς δέσμευση:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Με δέσμευση:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] Εάν σας σελίδα μεταβιβάζονται από το `OnNavigatedTo` μέθοδο, πρέπει να είστε προσεκτικοί για να επιτρέψετε την `base.OnNavigatedTo(e)` κλήσεων. Διαφορετικά, τη δραστηριότητα δεν θα αναφερθούν. Στην πραγματικότητα, το `EngagementPage` καλεί `StartActivity` μέσα το `OnNavigatedTo` μέθοδο.

#### <a name="xaml-file"></a>Αρχείο XAML

Τροποποίηση της σελίδας σας `.xaml` αρχείο:

-   Προσθέστε τις δηλώσεις πεδία ονομάτων:

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   Αντικατάσταση `phone:PhoneApplicationPage` με `engagement:EngagementPage` :

**Χωρίς δέσμευση:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Με δέσμευση:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
        
            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Παράκαμψη την προεπιλεγμένη συμπεριφορά

Από προεπιλογή, το όνομα κλάσης της σελίδας αναφέρεται ως το όνομα της δραστηριότητας, με χωρίς επιπλέον. Εάν η κλάση χρησιμοποιεί το επίθημα "Σελίδα", δέσμευση θα καταργήσει το.

Εάν θέλετε να αντικαταστήσετε την προεπιλεγμένη συμπεριφορά για το όνομα, απλώς προσθέστε αυτό σας κώδικα:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Εάν θέλετε να αναφέρετε ορισμένες επιπλέον πληροφορίες με τη δραστηριότητά σας, μπορείτε να το προσθέσετε κώδικα:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Αυτές οι μέθοδοι ονομάζονται μέσα από το `OnNavigatedTo` μέθοδο της σελίδας σας.

### <a name="alternate-method-call-startactivity-manually"></a>Εναλλακτική μέθοδο: κλήση `StartActivity()` με μη αυτόματο τρόπο

Εάν δεν μπορείτε ή δεν θέλετε να υπερφόρτωσης σας `PhoneApplicationPage` κλάσεις, αντί για αυτό, μπορείτε να ξεκινήσετε τις δραστηριότητές σας καλώντας `EngagementAgent` μεθόδους απευθείας.

Σας συνιστούμε να καλείτε `StartActivity` εσωτερικό σας `OnNavigatedTo` μέθοδο PhoneApplicationPage σας.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] Βεβαιωθείτε ότι τερματίσετε την περίοδο λειτουργίας σας σωστά.
>
> Το SDK καλεί αυτόματα το `EndActivity` μέθοδο όταν η εφαρμογή είναι κλειστό. Έτσι, είναι **ΙΔΙΑΊΤΕΡΑ** συνιστάται να καλέσετε το `StartActivity` μέθοδο κάθε φορά που τη δραστηριότητα του χρήστη για αλλαγή και **ποτέ** κλήση του `EndActivity` μέθοδο. Αυτή η μέθοδος στέλνει ένα μήνυμα στο διακομιστή δέσμευση που έφυγε από τον τρέχοντα χρήστη της εφαρμογής και αυτό επηρεάζει όλα τα αρχεία καταγραφής εφαρμογών.

##<a name="advanced-reporting"></a>Σύνθετες αναφοράς

Προαιρετικά, ενδέχεται να θέλετε να αναφορά εφαρμογής συγκεκριμένα συμβάντα, σφάλματα και εργασιών, για να το κάνετε αυτό, χρησιμοποιήστε τις άλλες μεθόδους που βρέθηκε στο το `EngagementAgent` κλάση. Το API δέσμευση σας επιτρέπει να χρησιμοποιήσετε όλες οι δυνατότητες για προχωρημένους του δέσμευση.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API στην εφαρμογή Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md).

##<a name="advanced-configuration"></a>Ρύθμιση παραμέτρων για προχωρημένους

### <a name="disable-automatic-crash-reporting"></a>Απενεργοποίηση της αυτόματης σφάλμα αναφοράς

Μπορείτε να απενεργοποιήσετε την αυτόματη σφάλμα δυνατότητα της δέσμευσης αναφοράς. Στη συνέχεια, όταν μια ανεπίλυτη εξαίρεση, θα συμβεί δέσμευση δεν κάνετε τίποτα.

> [AZURE.WARNING] Εάν σκοπεύετε να απενεργοποιήσετε αυτήν τη δυνατότητα, έχετε υπόψη ότι όταν θα παρουσιαστεί σφάλμα ανεπίλυτη στην εφαρμογή, δέσμευση δεν θα στείλει το σφάλμα **και** δεν θα κλείσει η περίοδος λειτουργίας και οι εργασίες.

Για να απενεργοποιήσετε την αυτόματη σφάλμα αναφοράς, απλώς προσαρμογή της ρύθμισης παραμέτρων ανάλογα με τον τρόπο που έχουν δηλωθεί:

#### <a name="from-engagementconfigurationxml-file"></a>Από το `EngagementConfiguration.xml` αρχείου

Ορισμός αναφοράς σφαλμάτων για να `false` μεταξύ `<reportCrash>` και `</reportCrash>` ετικέτες.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Από το `EngagementConfiguration` αντικειμένου κατά το χρόνο εκτέλεσης

Ορισμός αναφοράς σφαλμάτων FALSE (ψευδής) χρησιμοποιώντας το αντικείμενο EngagementConfiguration.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Κατάσταση λειτουργίας καταιγισμού

Από προεπιλογή, οι αναφορές εξυπηρέτησης δέσμευση καταγράφει σε πραγματικό χρόνο. Εάν η εφαρμογή σας αναφέρει αρχεία καταγραφής πολύ συχνά, είναι καλύτερα να buffer τα αρχεία καταγραφής και να αναφέρετε όλα ταυτόχρονα σε μια κανονική ώρα base (αυτό ονομάζεται "λειτουργία καταιγισμού").

Για να το κάνετε αυτό, η κλήση της μεθόδου:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Το όρισμα είναι μια τιμή σε **χιλιοστά του δευτερολέπτου**. Οποιαδήποτε στιγμή, εάν θέλετε να ενεργοποιήσετε ξανά την καταγραφή σε πραγματικό χρόνο, απλώς κλήση της μεθόδου χωρίς παραμέτρους ή με την τιμή 0.

Η κατάσταση λειτουργίας καταιγισμού ελαφρώς αυξήσετε τη διάρκεια ζωής μπαταριών αλλά έχει επιπτώσεις στην οθόνη δέσμευση: όλες οι περίοδοι λειτουργίας και εργασιών διάρκεια στρογγυλοποιείται σε το όριο καταιγισμού (έτσι, περιόδους λειτουργίας και εργασίες που είναι μικρότερες από το όριο καταιγισμού ενδέχεται να μην είναι ορατά). Συνιστάται να χρησιμοποιήσετε ένα όριο καταιγισμού δεν είναι πλέον από 30000 (30s). Πρέπει να έχετε υπόψη που αποθηκεύονται αρχεία καταγραφής περιορίζονται σε 300 στοιχεία. Εάν η αποστολή είναι πολύ μεγάλη μπορεί να χάσετε ορισμένα αρχεία καταγραφής.

> [AZURE.WARNING] Το όριο καταιγισμού δεν μπορεί να ρυθμιστεί σε μια περίοδο μικρότερο από ένα δευτερόλεπτο. Εάν προσπαθείτε να κάνετε αυτό, το SDK θα εμφανιστεί μια ανίχνευση με το σφάλμα και θα αυτόματα επαναφέρετε την προεπιλεγμένη τιμή, δηλαδή, μηδέν δευτερόλεπτα. Αυτό θα ενεργοποιήσει το SDK για την αναφορά των αρχείων καταγραφής σε πραγματικό χρόνο.
 
