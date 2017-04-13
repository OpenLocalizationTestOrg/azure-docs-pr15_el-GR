<properties 
    pageTitle="Διαδικασίες αναβάθμισης καθολικής εφαρμογές SDK των Windows" 
    description="Καθολική εφαρμογές Windows SDK αναβάθμισης διαδικασίες για τη δέσμευση Azure κινητές συσκευές"                     
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

#<a name="windows-universal-apps-sdk-upgrade-procedures"></a>Διαδικασίες αναβάθμισης καθολικής εφαρμογές SDK των Windows

Εάν έχετε ήδη έχετε συνδέσει μια παλαιότερη έκδοση του δέσμευση στην εφαρμογή σας, πρέπει να λάβετε υπόψη σας τα ακόλουθα σημεία κατά την αναβάθμιση του SDK.

Ίσως χρειαστεί να ακολουθήσετε διάφορες διαδικασίες, εάν έχετε παραβλέψει πολλές εκδόσεις του SDK. Για παράδειγμα, εάν κάνετε μετεγκατάσταση από 0.10.1 σε 0.11.0 πρέπει να πρώτα να ακολουθήστε τη διαδικασία "από 0.9.0 να 0.10.1", στη συνέχεια, τη διαδικασία "από 0.10.1 να 0.11.0".

##<a name="from-330-to-340"></a>Από το 3.3.0 να 3.4.0

### <a name="test-logs"></a>Αρχεία καταγραφής ελέγχου

Αρχεία καταγραφής κονσόλας που δημιουργήθηκαν με το SDK τώρα μπορεί να είναι ενεργοποιημένες/απενεργοποίησης/φιλτραρισμένη. Για να προσαρμόσετε έτσι, ενημερώστε την ιδιότητα `EngagementAgent.Instance.TestLogEnabled` σε μία από την τιμή που είναι διαθέσιμα από το `EngagementTestLogLevel` απαρίθμηση, για παράδειγμα:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Πόροι

Έχουν βελτιωθεί Reach επικάλυψης. Είναι μέρος οι πόροι του πακέτου SDK NuGet.

Κατά την αναβάθμιση στη νέα έκδοση του SDK μπορείτε να επιλέξετε εάν θέλετε να διατηρήσετε τα υπάρχοντα αρχεία από το φάκελο επικάλυψη τους πόρους σας ή όχι:

* Εάν το προηγούμενο επικάλυψη εργάζεται για εσάς ή που ενοποίηση του `WebView` στοιχεία με μη αυτόματο τρόπο, στη συνέχεια, μπορείτε να αποφασίσετε να διατηρήσετε τα αρχεία σας Τερματισμός λειτουργίας, αυτό θα εξακολουθούν να λειτουργούν. 
* Εάν θέλετε να ενημερώσετε το νέο επικάλυψη, απλά αντικαταστήστε το σύνολο `overlay` φάκελο από τους πόρους σας με τη νέα από το πακέτο SDK (UWP εφαρμογές: μετά την αναβάθμιση, μπορείτε να λάβετε τον νέο φάκελο επικάλυψη από % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Χρήση της νέας επικάλυψης θα αντικαταστήσει τυχόν προσαρμογές που έγιναν από την προηγούμενη έκδοση.

##<a name="from-320-to-330"></a>Από το 3.2.0 να 3.3.0

### <a name="resources"></a>Πόροι
Αυτό το βήμα αφορά μόνο προσαρμοσμένες πόρους. Εάν έχετε προσαρμόσει τους πόρους που παρέχονται από το SDK (html, εικόνες, επικάλυψη), στη συνέχεια, πρέπει να δημιουργήσετε αντίγραφα ασφαλείας πριν από την αναβάθμιση και εκ νέου εφαρμογή Προσαρμογή σας στην αναβαθμισμένη πόρους.

##<a name="from-310-to-320"></a>Από το 3.1.0 να 3.2.0

### <a name="resources"></a>Πόροι
Αυτό το βήμα αφορά μόνο προσαρμοσμένες πόρους. Εάν έχετε προσαρμόσει τους πόρους που παρέχονται από το SDK (html, εικόνες, επικάλυψη), στη συνέχεια, πρέπει να δημιουργήσετε αντίγραφα ασφαλείας πριν από την αναβάθμιση και εκ νέου εφαρμογή Προσαρμογή σας στην αναβαθμισμένη πόρους.

### <a name="webview-integration"></a>Ενοποίηση προβολής Web
Ορισμένες βελτιώσεις ώστε να ταιριάζει με διαφορετική συσκευή φόρμας παράγοντες έχουν εισαχθεί σε αυτήν την έκδοση. Βεβαιωθείτε ότι το ενοποίησης του την προβολή Web ταιριάζουν με τα εξής:

Στο σας (σελίδα XAML):

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

Και στο αρχείο σας συσχετισμένη .cs:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
            
              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }
              
              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif
            
              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }
            
              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

##<a name="from-200-to-300"></a>Από το 2.0.0 να 3.0.0

### <a name="resources"></a>Πόροι
Αυτό το βήμα αφορά μόνο προσαρμοσμένες πόρους. Εάν έχετε προσαρμόσει τους πόρους που παρέχονται από το SDK (html, εικόνες, επικάλυψη), στη συνέχεια, πρέπει να δημιουργήσετε αντίγραφα ασφαλείας πριν από την αναβάθμιση και εκ νέου εφαρμογή Προσαρμογή σας στην αναβαθμισμένη πόρους.

##<a name="from-111-to-200"></a>Από 1.1.1 να 2.0.0

Παρακάτω περιγράφεται ο τρόπος για να μετεγκαταστήσετε μια ενοποίηση SDK από την υπηρεσία Capptain που παρέχεται από Capptain συσχετισμούς Ασφαλείας σε μια εφαρμογή που υποστηρίζεται από Azure Mobile δέσμευση. 

> [Azure.IMPORTANT] Capptain και δέσμευση Mobile δεν είναι τις ίδιες υπηρεσίες και τη διαδικασία που ακολουθεί επισημαίνει μόνο με τη μετεγκατάσταση στην εφαρμογή υπολογιστή-πελάτη. Μετεγκατάσταση στο SDK στην εφαρμογή θα δεν μετεγκατάσταση των δεδομένων σας από τους διακομιστές Capptain με τους διακομιστές Mobile δέσμευση

Εάν κάνετε μετεγκατάσταση από μια προηγούμενη έκδοση, συμβουλευτείτε την τοποθεσία web Capptain για μετεγκατάσταση σε 1.1.1 πρώτα και, στη συνέχεια, εφαρμόστε την ακόλουθη διαδικασία

### <a name="nuget-package"></a>Το πακέτο Nuget

Αντικατάσταση **Capptain.WindowsPhone** από το πακέτο Nuget **MicrosoftAzure.MobileEngagement** .

### <a name="applying-mobile-engagement"></a>Εφαρμογή δέσμευση κινητές συσκευές

Το SDK χρησιμοποιεί τον όρο `Engagement`. Πρέπει να ενημερώσετε το έργο σας ώστε να ταιριάζει με αυτήν την αλλαγή.

Πρέπει να καταργήσετε την εγκατάσταση του τρέχοντος πακέτου nuget Capptain. Λάβετε υπόψη ότι θα καταργηθούν όλες οι αλλαγές σας στο φάκελο Capptain πόρους. Εάν θέλετε να διατηρήσετε αυτά τα αρχεία και, στη συνέχεια, δημιουργήστε ένα αντίγραφο του τους.

Μετά από αυτό, εγκαταστήστε το νέο πακέτο nuget Microsoft Azure δέσμευση στο έργο σας. Μπορείτε να το βρείτε απευθείας στην [nuget τοποθεσία Web]. ή εδώ ευρετηρίου. Αυτή η ενέργεια αντικαθιστά όλα τα αρχεία τους πόρους που χρησιμοποιούνται από δέσμευση και προσθέτει το νέο DLL δέσμευση για αναφορές του έργου σας.

Πρέπει να εκκαθαρίσετε αναφορές του έργου σας με τη διαγραφή Capptain DLL αναφορές. Εάν δεν κάνετε αυτό, θα υπάρχει διένεξη μεταξύ της έκδοσης του Capptain και θα συμβεί σφάλματα.

Εάν έχετε προσαρμόσει Capptain πόρους, αντιγράψτε το περιεχόμενό σας παλιά αρχεία και επικολλήστε τα στο τα νέα αρχεία δέσμευση. Σημειώστε ότι τα αρχεία xaml και στρατηγικής συνεργασίας ανά χώρα πρέπει να ενημερωθούν.

Όταν ολοκληρωθούν αυτά τα βήματα, έχετε μόνο για να αντικαταστήσετε το παλιό Capptain αναφορές από τις νέες αναφορές δέσμευση.

1. Όλοι οι χώροι ονομάτων Capptain πρέπει να ενημερωθούν.

    Πριν από την μετεγκατάσταση:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Μετά τη μετεγκατάσταση:
    
        using Microsoft.Azure.Engagement;

2. Όλες οι κλάσεις Capptain που περιέχουν "Capptain" πρέπει να περιέχει "Δέσμευση".

    Πριν από την μετεγκατάσταση:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Μετά τη μετεγκατάσταση:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Για τα αρχεία xaml Capptain χώρο ονομάτων και χαρακτηριστικά επίσης να αλλάξουν.

    Πριν από την μετεγκατάσταση:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Μετά τη μετεγκατάσταση:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Επικάλυψη αλλαγές σελίδας
    > [AZURE.IMPORTANT] Επικάλυψη αλλάζει επίσης. Είναι το νέο χώρο ονομάτων `Microsoft.Azure.Engagement.Overlay`. Πρέπει να χρησιμοποιηθούν σε αρχεία xaml και στρατηγικής συνεργασίας ανά χώρα. Επιπλέον `CapptainGrid` είναι να ονομαστεί `EngagementGrid`, `capptain_notification_content` και `capptain_announcement_content` ονομάζονται `engagement_notification_content` και `engagement_announcement_content`.
    
    Για επικάλυψη:
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    Γίνεται:
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. Για τους άλλους πόρους όπως Capptain εικόνων και αρχείων HTML, έχετε υπόψη ότι αυτά επίσης έχουν μετονομαστεί για να χρησιμοποιήσετε "Δέσμευση".

### <a name="project-declaration"></a>Δήλωση έργου

Στην Package.appxmanifest `File Type Associations` έχει ενημερωθεί από:

 -   capptain\_επίτευξη\_περιεχομένου για τη δέσμευση\_επίτευξη\_περιεχομένου
 -   capptain\_καταγραφής\_αρχείου για δέσμευση\_καταγραφής\_αρχείου

### <a name="application-id--sdk-key"></a>Αναγνωριστικό εφαρμογής / κλειδί SDK

Δέσμευση χρησιμοποιεί μια συμβολοσειρά σύνδεσης. Δεν χρειάζεται να καθορίσετε ένα Αναγνωριστικό εφαρμογής και ένας αριθμός-κλειδί SDK με δέσμευση Mobile, έχετε μόνο για να καθορίσετε μια συμβολοσειρά σύνδεσης. Μπορείτε να την ορίσετε στο αρχείο σας EngagementConfiguration.

Η ρύθμιση παραμέτρων δέσμευση μπορούν να ρυθμιστούν στο σας `Resources\EngagementConfiguration.xml` αρχείο του έργου σας.

Επεξεργαστείτε αυτό το αρχείο για να καθορίσετε:

-   Η συμβολοσειρά σύνδεσης εφαρμογής μεταξύ ετικετών `<connectionString>` και `<\connectionString>`.

Εάν θέλετε να καθορίσετε κατά το χρόνο εκτέλεσης, μπορείτε να καλέσετε την ακόλουθη μέθοδο πριν από την προετοιμασία παράγοντας δέσμευση:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

Εμφανίζεται η συμβολοσειρά σύνδεσης για την εφαρμογή σας στην πύλη κλασική Azure.

### <a name="items-name-change"></a>Αλλαγή του ονόματος στοιχείων

Όλα τα στοιχεία με το όνομα *capptain* έχετε ονομαστεί *Δέσμευση*. Ομοίως για *Capptain* *Δέσμευση*.

Παραδείγματα των στοιχείων που χρησιμοποιείται συχνά Capptain:

-   Τώρα που ονομάζεται EngagementConfiguration CapptainConfiguration
-   Τώρα που ονομάζεται EngagementAgent CapptainAgent
-   Τώρα που ονομάζεται EngagementReach CapptainReach
-   Τώρα που ονομάζεται EngagementHttpConfig CapptainHttpConfig
-   Τώρα που ονομάζεται GetEngagementPageName GetCapptainPageName

Σημειώστε ότι η μετονομασία επηρεάζει επίσης αντικατασταθεί μεθόδων.

 
