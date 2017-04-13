<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure Mobile δέσμευση για Windows Phone Silverlight εφαρμογές"
    description="Μάθετε πώς να χρησιμοποιείτε Azure δέσμευση Mobile με τις ειδοποιήσεις ανάλυση και push για Windows Phone Silverlight εφαρμογές."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Γρήγορα αποτελέσματα με το Azure Mobile δέσμευση για Windows Phone Silverlight εφαρμογές

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure Mobile δέσμευση για να κατανοήσετε τις χρήσης εφαρμογής και αποστολή ειδοποιήσεων push σε τμηματική χρήστες μιας εφαρμογής Silverlight Windows Phone.
Αυτό το πρόγραμμα εκμάθησης δείχνει το απλό σενάριο εκπομπής χρησιμοποιώντας δέσμευση Mobile. Σε αυτό, μπορείτε να δημιουργήσετε μια κενή εφαρμογή Windows Phone Silverlight που συλλέγει βασικές δεδομένα και να λαμβάνει τις ειδοποιήσεις push χρησιμοποιώντας υπηρεσία ειδοποιήσεων Push της Microsoft (MPNS).

> [AZURE.NOTE] Εάν στοχεύετε σε Windows Phone 8.1 (μη Silverlight), ανατρέξτε στο της [καθολικής Windows πρόγραμμα εκμάθησης](mobile-engagement-windows-store-dotnet-get-started.md).

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ Visual Studio 2013
+ Το πακέτο Nuget [MicrosoftAzure.MobileEngagement]

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).

##<a id="setup-azme"></a>Ρύθμιση κινητών δέσμευση για το Windows Phone app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Σύνδεση της εφαρμογής σας στον υπολογιστή στο παρασκήνιο Mobile δέσμευση

Αυτό το πρόγραμμα εκμάθησης παρουσιάζει μια "βασικές ολοκλήρωσης", που είναι το ελάχιστο σύνολο που απαιτείται για τη συλλογή δεδομένων και να στείλετε μια ειδοποίηση push. Μπορείτε να βρείτε την τεκμηρίωση ολοκλήρωσης ενοποίησης στο την [ενοποίηση Mobile δέσμευση Windows Phone SDK](mobile-engagement-windows-phone-sdk-overview.md)

Θα δημιουργήσουμε μια βασική εφαρμογή με το Visual Studio για μια επίδειξη την ενοποίηση.

###<a name="create-a-new-windows-phone-silverlight-project"></a>Δημιουργήστε ένα νέο έργο Silverlight Windows Phone

Ακολουθήστε τα παρακάτω βήματα θεωρείται ότι τη χρήση του Visual Studio 2015, αν και τα βήματα είναι παρόμοια σε παλαιότερες εκδόσεις του Visual Studio. 

1. Ξεκινήστε Visual Studio και στην **αρχική** οθόνη, επιλέξτε **Νέο έργο**.

2. Στο αναδυόμενο παράθυρο, επιλέξτε **Windows 8** -> **Windows Phone** -> **Κενή εφαρμογή (Windows Phone Silverlight)**. Συμπληρώστε την εφαρμογή **όνομα** και το **όνομα της λύσης**και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

    ![][1]

3. Μπορείτε να επιλέξετε να στοχεύετε σε **Windows Phone 8.0** ή **Windows Phone 8.1**.

Τώρα έχετε δημιουργήσει μια νέα εφαρμογή Windows Phone Silverlight στο οποίο θα σας θα ενοποίηση του SDK Azure Mobile δέσμευση.

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Σύνδεση της εφαρμογής σας στον υπολογιστή στο παρασκήνιο Mobile δέσμευση

1. Εγκαταστήστε το πακέτο nuget [MicrosoftAzure.MobileEngagement] στο έργο σας.

2. Άνοιγμα `WMAppManifest.xml` (κάτω από το φάκελο ιδιότητες) και βεβαιωθείτε ότι τα ακόλουθα δηλώνονται (προσθήκη τους εάν δεν βρίσκονται) στο το `<Capabilities />` ετικέτα:

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Επικολλήστε τη συμβολοσειρά σύνδεσης που αντιγράψατε προηγουμένως για την εφαρμογή Mobile δέσμευση σας τώρα και να την επικολλήσετε σε το `Resources\EngagementConfiguration.xml` αρχείο, μεταξύ του `<connectionString>` και `</connectionString>` ετικέτες:

    ![][3]

4. Στο το `App.xaml.cs` αρχείο:

    μια. Προσθήκη του `using` δήλωση:

            using Microsoft.Azure.Engagement;

    β. Προετοιμασία του SDK στο το `Application_Launching` μέθοδο:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c. Εισαγάγετε τα εξής στο το `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>Ενεργοποίηση παρακολούθησης σε πραγματικό χρόνο

Για να ξεκινήσετε την αποστολή δεδομένων και την εξασφάλιση ότι οι χρήστες είναι ενεργό, πρέπει να στείλετε τουλάχιστον μία οθόνη (δραστηριότητα) στον υπολογιστή στο παρασκήνιο δέσμευση Mobile.

1. Στο το MainPage.xaml.cs, προσθέστε το `using` δήλωση:

        using Microsoft.Azure.Engagement;

2. Αντικαταστήστε βασική κλάση του **MainPage**, που είναι πριν από την **PhoneApplicationPage**, με **EngagementPage**.

        class MainPage : EngagementPage 
    
3. Στο σας `MainPage.xml` αρχείο:

    μια. Προσθέστε τις δηλώσεις πεδία ονομάτων:

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    β. Αντικατάσταση `phone:PhoneApplicationPage` στο όνομα της ετικέτας XML με `engagement:EngagementPage`.

##<a id="monitor"></a>Σύνδεση εφαρμογής με παρακολούθηση σε πραγματικό χρόνο

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ενεργοποίηση ειδοποιήσεων push και της εφαρμογής ανταλλαγής μηνυμάτων

Δέσμευση Mobile σάς επιτρέπει να αλληλεπιδρούν και επίτευξη τους χρήστες σας με τις ειδοποιήσεις Push και μηνύματα της εφαρμογής στο περιβάλλον της εκστρατείες. Η ενότητα αυτή ονομάζεται REACH στην πύλη του δέσμευση Mobile.
Οι παρακάτω ενότητες ρύθμιση της εφαρμογής σας για τη λήψη τους.

###<a name="enable-your-app-to-receive-mpns-push-notifications"></a>Ενεργοποιήστε την εφαρμογή σας για να λαμβάνετε ειδοποιήσεις Push MPNS

Προσθέστε νέες δυνατότητες για να σας `WMAppManifest.xml` αρχείο:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###<a name="initialize-the-reach-sdk"></a>Προετοιμασία του SDK REACH

1. Στο `App.xaml.cs`, κλήση `EngagementReach.Instance.Init();` στη συνάρτηση **Application_Launching** , αμέσως μετά από την προετοιμασία παράγοντα:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. Στο `App.xaml.cs`, κλήση `EngagementReach.Instance.OnActivated(e);` στη συνάρτηση **Application_Activated** , αμέσως μετά από την προετοιμασία παράγοντα:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Είστε έτοιμοι. Τώρα θα σας θα επαληθεύσετε ότι έχετε σωστά Φώναξε ανάληψη αυτή η βασική ενσωμάτωση.

##<a id="send"></a>Στείλτε μια ειδοποίηση για την εφαρμογή σας

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Θα πρέπει τώρα βλέπετε μια ειδοποίηση στη συσκευή σας, το οποίο θα εμφανίζεται ως μια ειδοποίηση της εφαρμογής εάν η εφαρμογή είναι ανοιχτή διαφορετικά ως μια αναδυόμενη ειδοποίηση όπως το εξής: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
