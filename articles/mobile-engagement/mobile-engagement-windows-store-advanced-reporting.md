<properties
    pageTitle="Παγκόσμια Windows σύνθετες αναφοράς με MobileApps δέσμευση"
    description="Πώς μπορείτε να ενοποιήσετε Azure δέσμευση κινητές συσκευές με καθολική εφαρμογές των Windows"                  
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Δημιουργία αναφορών με τη δέσμευση καθολικής εφαρμογές Windows SDK για προχωρημένους

> [AZURE.SELECTOR]
- [Καθολική των Windows](mobile-engagement-windows-store-advanced-reporting.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

Αυτό το θέμα περιγράφει επιπλέον σενάρια δημιουργίας αναφορών στο Windows καθολική εφαρμογή σας. Αυτά τα σενάρια περιλαμβάνουν επιλογές που μπορείτε να επιλέξετε για να εφαρμόσετε στην εφαρμογή που έχουν δημιουργηθεί με το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα](mobile-engagement-windows-store-dotnet-get-started.md) .

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Πριν να ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να ολοκληρώσετε πρώτα το [Γρήγορα αποτελέσματα](mobile-engagement-windows-store-dotnet-get-started.md) πρόγραμμα εκμάθησης, δηλαδή σκοπίμως άμεσο και απλό. Αυτό το πρόγραμμα εκμάθησης καλύπτει πρόσθετες επιλογές που μπορείτε να επιλέξετε από.

## <a name="specifying-engagement-configuration-at-runtime"></a>Καθορισμός δέσμευση ρύθμισης παραμέτρων κατά το χρόνο εκτέλεσης

Η ρύθμιση παραμέτρων δέσμευση είναι συγκεντρωμένα σε το `Resources\EngagementConfiguration.xml` αρχείο του έργου σας, η οποία είναι όπου έχει καθοριστεί στο θέμα [Γρήγορα αποτελέσματα](mobile-engagement-windows-store-dotnet-get-started.md) .

Αλλά μπορείτε επίσης να καθορίσετε κατά το χρόνο εκτέλεσης: μπορείτε να καλέσετε την ακόλουθη μέθοδο πριν από την προετοιμασία παράγοντας δέσμευση:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Προτεινόμενη μέθοδος: υπερφόρτωσης σας `Page` κλάσεις

Για να ενεργοποιήσετε το αναφοράς όλα τα αρχεία καταγραφής που απαιτούνται από δέσμευση για τον υπολογισμό χρηστών, οι περίοδοι λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και τεχνικές στατιστικά στοιχεία, κάνετε όλων σας `Page` δευτερεύουσες κλάσεις μεταβιβάζονται από το `EngagementPage` τάξεις τους.

Ακολουθεί ένα παράδειγμα για μια σελίδα της εφαρμογής σας. Μπορείτε να κάνετε το ίδιο για όλες τις σελίδες της εφαρμογής σας.

### <a name="c-source-file"></a>Αρχείο προέλευσης C#

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

> [AZURE.IMPORTANT] Εάν η σελίδα σας που παρακάμπτει το `OnNavigatedTo` μέθοδο, φροντίστε να καλέσετε `base.OnNavigatedTo(e)`. Διαφορετικά, δεν είναι να αναφερθεί τη δραστηριότητα (το `EngagementPage` κλήσεις `StartActivity` μέσα το `OnNavigatedTo` μέθοδος).

### <a name="xaml-file"></a>Αρχείο XAML

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

### <a name="override-the-default-behaviour"></a>Παράκαμψη την προεπιλεγμένη συμπεριφορά

Από προεπιλογή, το όνομα κλάσης της σελίδας αναφέρεται ως το όνομα της δραστηριότητας, με χωρίς επιπλέον. Εάν η κλάση χρησιμοποιεί το επίθημα "Σελίδα", δέσμευση καταργεί το φάκελο.

Για να παρακάμψετε την προεπιλεγμένη συμπεριφορά για το όνομα, προσθέστε αυτόν τον κώδικα:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Για να αναφέρετε επιπλέον πληροφορίες με τη δραστηριότητά σας, να προσθέσετε αυτόν τον κώδικα:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Αυτές οι μέθοδοι ονομάζονται μέσα από το `OnNavigatedTo` μέθοδο της σελίδας σας.

### <a name="alternate-method-call-startactivity-manually"></a>Εναλλακτική μέθοδο: κλήση `StartActivity()` με μη αυτόματο τρόπο

Εάν δεν μπορείτε ή δεν θέλετε να υπερφόρτωσης σας `Page` κλάσεις, αντί για αυτό, μπορείτε να ξεκινήσετε τις δραστηριότητές σας καλώντας `EngagementAgent` μεθόδους απευθείας.

Σας συνιστούμε να καλείτε `StartActivity` εσωτερικό σας `OnNavigatedTo` μέθοδο της σελίδας σας.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Βεβαιωθείτε ότι τερματίσετε την περίοδο λειτουργίας σας σωστά.
>
> Καθολική SDK των Windows καλεί αυτόματα το `EndActivity` μέθοδο όταν η εφαρμογή είναι κλειστό. Έτσι, είναι **ΙΔΙΑΊΤΕΡΑ** συνιστάται να καλέσετε το `StartActivity` μέθοδο κάθε φορά που τη δραστηριότητα του χρήστη για αλλαγή και **ποτέ** κλήση του `EndActivity` μέθοδο. Αυτή η μέθοδος ειδοποιεί το διακομιστή δέσμευση ότι έφυγε από τον τρέχοντα χρήστη της εφαρμογής, η οποία θα επηρεάσει όλα τα αρχεία καταγραφής εφαρμογών.

## <a name="advanced-reporting"></a>Σύνθετες αναφοράς

Προαιρετικά, ενδέχεται να θέλετε να αναφέρετε συγκεκριμένη εφαρμογή συμβάντα, σφάλματα και εργασιών, για να το κάνετε αυτό, χρησιμοποιήστε τις άλλες μεθόδους που βρέθηκε στο το `EngagementAgent` τάξης. Το API δέσμευση επιτρέπει χρήση όλα δέσμευση σύνθετες δυνατότητες.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API στην εφαρμογή Windows καθολικής](mobile-engagement-windows-store-use-engagement-api.md).
