<properties 
    pageTitle="Ενοποίηση SDK δέσμευση Universal εφαρμογές των Windows" 
    description="Πώς μπορείτε να ενοποιήσετε Azure δέσμευση κινητές συσκευές με καθολική εφαρμογές των Windows"                  
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

# <a name="windows-universal-apps-engagement-sdk-integration"></a>Ενοποίηση SDK δέσμευση καθολικής εφαρμογές των Windows

> [AZURE.SELECTOR] 
- [Καθολική των Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Αυτή η διαδικασία περιγράφει ο πιο απλός τρόπος για να ενεργοποιήσετε την ανάλυση της δέσμευσης και παρακολούθηση συναρτήσεις στο Windows καθολική εφαρμογή σας.

Τα ακόλουθα βήματα είναι αρκετά για να ενεργοποιήσετε την αναφορά των αρχείων καταγραφής χρειάζεται, για να υπολογίσετε όλα τα στατιστικά στοιχεία σχετικά με τους χρήστες, οι περίοδοι λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και Technicals. Η έκθεση της καταγραφής που είναι απαραίτητα για τον υπολογισμό άλλων στατιστικών στοιχείων όπως συμβάντα, σφάλματα και οι εργασίες πρέπει να γίνουν με μη αυτόματο τρόπο χρησιμοποιώντας το API δέσμευση (ανατρέξτε στο θέμα [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API στην εφαρμογή Windows καθολικής](mobile-engagement-windows-store-use-engagement-api.md) εφόσον αυτά τα στατιστικά στοιχεία έχουν εξαρτάται από την εφαρμογή.

## <a name="supported-versions"></a>Υποστηριζόμενες εκδόσεις

Η δέσμευση SDK για Windows καθολικής εφαρμογές του Mobile μπορεί μόνο να ενσωματώνει χρόνου εκτέλεσης των Windows και εφαρμογές καθολικής πλατφόρμα Windows στόχευσης:

-   Windows 8
-   Windows 8.1
-   Windows Phone 8.1
-   Windows 10 (υπολογιστή και κινητών οικογένειες)

> [AZURE.NOTE] Εάν στοχεύετε σε Windows Phone Silverlight, ανατρέξτε στη [διαδικασία ενοποίησης Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md).

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>Εγκατάσταση του SDK καθολικής εφαρμογές δέσμευση κινητές συσκευές

### <a name="all-platforms"></a>Όλες τις πλατφόρμες

Η δέσμευση SDK για Windows καθολική εφαρμογή Mobile είναι διαθέσιμη ως ένα πακέτο Nuget που ονομάζεται *MicrosoftAzure.MobileEngagement*. Μπορείτε να το εγκαταστήσετε από το Visual Studio Nuget διαχείρισης πακέτου.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x και Windows Phone 8.1

NuGet αναπτύσσει αυτόματα τους πόρους SDK το `Resources` φάκελος στον ριζικό κατάλογο του έργου σας εφαρμογής.

### <a name="windows-10-universal-windows-platform-applications"></a>Εφαρμογές των Windows 10 Universal πλατφόρμας των Windows

NuGet αυτόματα το αναπτύξετε τους πόρους SDK στην εφαρμογή UWP ακόμη. Πρέπει να γίνει με μη αυτόματο τρόπο, μέχρι να πόρους ανάπτυξης είναι επανέλθει σε NuGet:

1.  Ανοίξτε την Εξερεύνηση αρχείων.
2.  Μεταβείτε στην εξής θέση (**x.x.x** είναι η έκδοση του δέσμευση που εγκαθιστάτε): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  Σύρετε και αποθέστε το φάκελο " **πόροι** " από την Εξερεύνηση αρχείων στον ριζικό κατάλογο του έργου σας στο Visual Studio.
4.  Στο Visual Studio επιλέξτε το έργο σας και να ενεργοποιήσετε το εικονίδιο " **Εμφάνιση όλων των αρχείων** " επάνω από την **Εξερεύνηση λύσεων**.
5.  Ορισμένα αρχεία δεν περιλαμβάνονται στο έργο. Για να εισαγάγετε τους ταυτόχρονα κάντε δεξί κλικ στον φάκελο **πόρων** , **εξαίρεση από το project** , στη συνέχεια, μια άλλη δεξί κλικ στο φάκελο **πόρους** , **συμπεριλάβετε στο έργο** για να συμπεριλάβετε εκ νέου ολόκληρο το φάκελο. Τώρα περιλαμβάνει όλα τα αρχεία από το φάκελο **πόρων** στο έργο σας.

## <a name="add-the-capabilities"></a>Προσθέστε τις δυνατότητες

Η δέσμευση SDK πρέπει ορισμένες δυνατότητες του SDK των Windows για να λειτουργήσει σωστά.

Άνοιγμα του `Package.appxmanifest` αρχείου και βεβαιωθείτε ότι έχουν δηλωθεί τις ακόλουθες δυνατότητες:

-   `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>Προετοιμασία την πρόσληψη SDK

### <a name="engagement-configuration"></a>Ρύθμιση παραμέτρων δέσμευση

Η ρύθμιση παραμέτρων δέσμευση είναι συγκεντρωμένα σε το `Resources\EngagementConfiguration.xml` αρχείο του έργου σας.

Επεξεργαστείτε αυτό το αρχείο για να καθορίσετε:

-   Η συμβολοσειρά σύνδεσης εφαρμογής μεταξύ ετικετών `<connectionString>` και `<\connectionString>`.

Εάν θέλετε να καθορίσετε κατά το χρόνο εκτέλεσης, μπορείτε να καλέσετε την ακόλουθη μέθοδο πριν από την προετοιμασία παράγοντας δέσμευση:
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

Εμφανίζεται η συμβολοσειρά σύνδεσης για την εφαρμογή σας στην πύλη κλασική Azure.

### <a name="engagement-initialization"></a>Προετοιμασία δέσμευση

Όταν δημιουργείτε ένα νέο έργο, μια `App.xaml.cs` δημιουργείται αρχείο. Αυτή η κλάση μεταβιβάζονται από `Application` και περιέχει πολλές σημαντικές μεθόδους. Θα επίσης να χρησιμοποιηθούν για την προετοιμασία του SDK δέσμευση.

Τροποποίηση του `App.xaml.cs`:

-   Προσθήκη για να σας `using` προτάσεις:

        using Microsoft.Azure.Engagement;

-   Ορίστε μια μέθοδο για την κοινή χρήση της προετοιμασίας δέσμευση μία φορά για όλες τις κλήσεις:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
        
          // or
        
          EngagementAgent.Instance.Init(e, engagementConfiguration);
        }
        
-   Κλήση `InitEngagement` στο το `OnLaunched` μέθοδο:

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   Όταν η εφαρμογή σας εκκινείται χρησιμοποιώντας έναν προσαρμοσμένο συνδυασμό, μια άλλη εφαρμογή ή τη γραμμή εντολών, στη συνέχεια, το `OnActivated` μέθοδος καλείται. Πρέπει επίσης να ξεκινήσετε το SDK δέσμευση όταν είναι ενεργοποιημένη η εφαρμογή σας. Για να το κάνετε, παρακάμψτε `OnActivated` μέθοδο:

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }

> [AZURE.IMPORTANT] Δεν συνιστάται να προσθέσετε την προετοιμασία δέσμευσης σε άλλο σημείο της εφαρμογής σας.

## <a name="basic-reporting"></a>Βασική αναφορά

### <a name="recommended-method-overload-your-page-classes"></a>Προτεινόμενη μέθοδος: υπερφόρτωσης σας `Page` κλάσεις

Για να ενεργοποιήσετε την αναφορά της όλα τα αρχεία καταγραφής που απαιτούνται από δέσμευση για τον υπολογισμό χρηστών, οι περίοδοι λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και τεχνικές στατιστικά στοιχεία, μπορείτε να απλώς κάνετε όλων σας `Page` δευτερεύουσες κλάσεις μεταβιβάζονται από το `EngagementPage` τάξεις τους.

Ακολουθεί ένα παράδειγμα του τρόπου να το κάνετε αυτό για μια σελίδα της εφαρμογής σας. Μπορείτε να κάνετε το ίδιο για όλες τις σελίδες της εφαρμογής σας.

#### <a name="c-source-file"></a>Αρχείο προέλευσης C#

Τροποποίηση της σελίδας σας `.xaml.cs` αρχείο:

-   Προσθήκη για να σας `using` προτάσεις:

        using Microsoft.Azure.Engagement;

-   Αντικατάσταση `Page` με `EngagementPage`:

**Χωρίς δέσμευση:**
    
        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Με δέσμευση:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Εάν η σελίδα σας που παρακάμπτει το `OnNavigatedTo` μέθοδο, φροντίστε να καλέσετε `base.OnNavigatedTo(e)`. Διαφορετικά, τη δραστηριότητα δεν θα αναφερθεί (το `EngagementPage` κλήσεις `StartActivity` μέσα το `OnNavigatedTo` μέθοδος).

#### <a name="xaml-file"></a>Αρχείο XAML

Τροποποίηση της σελίδας σας `.xaml` αρχείο:

-   Προσθέστε τις δηλώσεις πεδία ονομάτων:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Αντικατάσταση `Page` με `engagement:EngagementPage`:

**Χωρίς δέσμευση:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Με δέσμευση:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Παράκαμψη την προεπιλεγμένη συμπεριφορά

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

Εάν δεν μπορείτε ή δεν θέλετε να υπερφόρτωσης σας `Page` κλάσεις, αντί για αυτό, μπορείτε να ξεκινήσετε τις δραστηριότητές σας καλώντας `EngagementAgent` μεθόδους απευθείας.

Συνιστάται να καλέσετε `StartActivity` εσωτερικό σας `OnNavigatedTo` μέθοδο της σελίδας σας.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Βεβαιωθείτε ότι τερματίσετε την περίοδο λειτουργίας σας σωστά.
> 
> Καθολική SDK των Windows καλεί αυτόματα το `EndActivity` μέθοδο όταν η εφαρμογή είναι κλειστό. Έτσι, είναι **ΙΔΙΑΊΤΕΡΑ** συνιστάται να καλέσετε το `StartActivity` μέθοδο κάθε φορά που τη δραστηριότητα του χρήστη για αλλαγή και **ποτέ** κλήση του `EndActivity` μέθοδο, αυτή η μέθοδος αποστέλλει δέσμευση διακομιστή που τρέχοντα χρήστη διαθέτει αποχώρηση από την εφαρμογή, αυτό θα επηρεάζει όλα τα αρχεία καταγραφής εφαρμογών.

## <a name="advanced-reporting"></a>Σύνθετες αναφοράς

Προαιρετικά, ενδέχεται να θέλετε να αναφορά εφαρμογής συγκεκριμένα συμβάντα, σφάλματα και εργασιών, για να το κάνετε αυτό, χρησιμοποιήστε τις άλλες μεθόδους που βρέθηκε στο το `EngagementAgent` τάξης. Το API δέσμευση σας επιτρέπει να χρησιμοποιήσετε όλες οι δυνατότητες για προχωρημένους του δέσμευση.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API στην εφαρμογή Windows καθολικής](mobile-engagement-windows-store-use-engagement-api.md).

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
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Κατάσταση λειτουργίας καταιγισμού

Από προεπιλογή, οι αναφορές εξυπηρέτησης δέσμευση καταγράφει σε πραγματικό χρόνο. Εάν η εφαρμογή σας αναφέρει αρχεία καταγραφής πολύ συχνά, είναι καλύτερα να buffer τα αρχεία καταγραφής και την αναφορά τους όλα ταυτόχρονα σε μια κανονική ώρα base (αυτό ονομάζεται "λειτουργία καταιγισμού").

Για να το κάνετε αυτό, η κλήση της μεθόδου:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Το όρισμα είναι μια τιμή σε **χιλιοστά του δευτερολέπτου**. Οποιαδήποτε στιγμή, εάν θέλετε να ενεργοποιήσετε ξανά την καταγραφή σε πραγματικό χρόνο, απλώς κλήση της μεθόδου χωρίς παραμέτρους ή με την τιμή 0.

Η κατάσταση λειτουργίας καταιγισμού ελαφρώς αυξήσετε τη διάρκεια ζωής μπαταριών αλλά έχει επιπτώσεις στην οθόνη δέσμευση: όλες οι περίοδοι λειτουργίας και εργασιών διάρκεια στρογγυλοποιείται σε το όριο καταιγισμού (έτσι, περιόδους λειτουργίας και εργασίες που είναι μικρότερες από το όριο καταιγισμού ενδέχεται να μην είναι ορατά). Συνιστάται να χρησιμοποιήσετε ένα όριο καταιγισμού δεν είναι πλέον από 30000 (30s). Πρέπει να έχετε υπόψη που αποθηκεύονται αρχεία καταγραφής περιορίζονται σε 300 στοιχεία. Εάν η αποστολή είναι πολύ μεγάλη μπορεί να χάσετε ορισμένα αρχεία καταγραφής.

> [AZURE.WARNING] Το όριο καταιγισμού δεν είναι δυνατό να ρυθμιστεί ώστε να μικρότερο από 1s τελεία. Εάν προσπαθήσετε να το κάνετε, στο SDK θα εμφανιστεί μια ανίχνευση με το σφάλμα και αυτόματα επανέρχεται στην προεπιλεγμένη τιμή, π.χ., 0s. Αυτό θα ενεργοποιήσει το SDK για την αναφορά των αρχείων καταγραφής σε πραγματικό χρόνο.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
 
