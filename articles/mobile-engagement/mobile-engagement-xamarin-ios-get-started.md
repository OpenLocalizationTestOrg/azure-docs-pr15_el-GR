<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure δέσμευση κινητές συσκευές για Xamarin.iOS"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Mobile δέσμευση με ανάλυση και τις ειδοποιήσεις Push για τις εφαρμογές Xamarin.iOS."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinios-apps"></a>Γρήγορα αποτελέσματα με το Azure δέσμευση κινητές συσκευές για τις εφαρμογές Xamarin.iOS

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure Mobile δέσμευση για να κατανοήσετε σας χρήσης εφαρμογής και αποστολή ειδοποιήσεων push σε τμηματική χρήστες σε μια εφαρμογή Xamarin.iOS.
Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε μια κενή εφαρμογή Xamarin.iOS που συλλέγει βασικές δεδομένα και να λαμβάνει τις ειδοποιήσεις push χρησιμοποιώντας σύστημα ειδοποιήσεων Push της Apple (APNS).

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ [Xamarin Studio](http://xamarin.com/studio). Μπορείτε επίσης να χρησιμοποιήσετε το Visual Studio με Xamarin, αλλά αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί Xamarin Studio. Για οδηγίες εγκατάστασης, ανατρέξτε στο θέμα [ρύθμιση και εγκατάσταση για Visual Studio και Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 
+ [Κινητές συσκευές δέσμευση Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).

##<a id="setup-azme"></a>Ρύθμιση Mobile δέσμευση για την εφαρμογή σας iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Σύνδεση της εφαρμογής σας στον υπολογιστή στο παρασκήνιο Mobile δέσμευση

Αυτό το πρόγραμμα εκμάθησης παρουσιάζει μια "βασικές ενοποίησης" που είναι η ελάχιστη που απαιτείται για τη συλλογή δεδομένων και να στείλετε μια ειδοποίηση push.

Θα δημιουργήσουμε μια βασική εφαρμογή με Xamarin για μια επίδειξη την ενοποίηση:

###<a name="create-a-new-xamarinios-project"></a>Δημιουργήστε ένα νέο έργο Xamarin.iOS

1. Εκκίνηση Xamarin Studio. Επιλέξτε **αρχείο** -> **νέα** -> **λύσης** 

    ![][1]

2. Επιλέξτε **Μία μόνο εφαρμογή προβολής**, βεβαιωθείτε ότι είναι επιλεγμένη γλώσσα **C#** και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![][2]

3. Συμπληρώστε το **Όνομα εφαρμογής** και το **Αναγνωριστικό εταιρείας** και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. 

    ![][3]

    > [AZURE.IMPORTANT] Βεβαιωθείτε ότι το προφίλ δημοσίευσης χρησιμοποιείτε τελικά για να αναπτύξετε την εφαρμογή iOS είναι χρησιμοποιεί ένα Αναγνωριστικό εφαρμογής που αντιστοιχεί ακριβώς με το αναγνωριστικό πακέτου εδώ έχετε. 

4. Ενημερώστε το **Όνομα του έργου**, **Όνομα λύσης** και τη **θέση** , εάν απαιτείται και κάντε κλικ στην επιλογή **Δημιουργία**.

    ![][4]
 
Xamarin Studio θα δημιουργήσει την εφαρμογή επίδειξη στην οποία θα σας θα ενοποίηση δέσμευση Mobile. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Σύνδεση της εφαρμογής σας στο κινητό δέσμευση παρασκηνίου

1. Κάντε δεξί κλικ στο φάκελο **πακέτων** στα παράθυρα λύσης και επιλέξτε **Προσθήκη πακέτων...**

    ![][5]

2. Αναζήτηση για το **Microsoft Azure Mobile δέσμευση Xamarin SDK** και προσθέστε το στη λύση σας.  

    ![][6]
   
3. Ανοίξτε **AppDelegate.cs** και προσθέστε την ακόλουθη πρόταση χρησιμοποιώντας:

        using Microsoft.Azure.Engagement.Xamarin;

4. Στη μέθοδο **FinishedLaunching** , προσθέστε τα εξής για να προετοιμασίας της σύνδεσης με το κινητό δέσμευση παρασκηνίου. Βεβαιωθείτε ότι για να προσθέσετε το **συμβολοσειρά σύνδεσης**. Αυτός ο κωδικός χρησιμοποιεί επίσης μια εικονική **NotificationIcon** που προστίθεται από το SDK δέσμευση Mobile, το οποίο μπορεί να θέλετε να αντικαταστήσετε. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a id="monitor"></a>Ενεργοποίηση παρακολούθησης σε πραγματικό χρόνο

Για να ξεκινήσετε την αποστολή δεδομένων και ταυτόχρονα εξασφαλίζετε ότι οι χρήστες είναι ενεργό, πρέπει να στείλετε τουλάχιστον μία οθόνη στον υπολογιστή στο παρασκήνιο δέσμευση Mobile.

1. Ανοίξτε **ViewController.cs** και προσθέστε την ακόλουθη πρόταση χρησιμοποιώντας:

        using Microsoft.Azure.Engagement.Xamarin;

2. Αντικαταστήστε την κλάση από την οποία `ViewController` μεταβιβάζονται από `UIViewController` να `EngagementViewController`. 

##<a id="monitor"></a>Σύνδεση εφαρμογής με παρακολούθηση σε πραγματικό χρόνο

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ενεργοποίηση ειδοποιήσεων push και της εφαρμογής ανταλλαγής μηνυμάτων

Δέσμευση Mobile σάς επιτρέπει να αλληλεπιδρούν οι χρήστες σας και να ΕΠΙΚΟΙΝΩΝΉΣΕΤΕ με τις ειδοποιήσεις push και την ανταλλαγή μηνυμάτων στο περιβάλλον της εκστρατείες της εφαρμογής. Η ενότητα αυτή ονομάζεται REACH στην πύλη του δέσμευση Mobile.
Οι παρακάτω ενότητες ρύθμιση της εφαρμογής σας για τη λήψη τους.

### <a name="modify-your-application-delegate"></a>Τροποποίηση εφαρμογής πληρεξούσιο

1. Ανοίξτε το **AppDelegate.cs** και προσθέστε την ακόλουθη πρόταση χρησιμοποιώντας:

        using System; 

2. Τώρα εντός του `FinishedLaunching` μέθοδο, προσθέστε τα εξής για να εγγραφείτε για το push μηνυμάτων μετά την`EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Τέλος - ενημερώσετε ή να προσθέσετε τις παρακάτω μεθόδους:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. Στο αρχείο σας **Info.plist** στη λύση, επιβεβαιώστε ότι το **Αναγνωριστικό πακέτου** συμφωνεί με με το **Αναγνωριστικό εφαρμογής** που έχετε στο προφίλ σας παροχής στο κέντρο ανάπτυξης του Apple. 

    ![][7]

5. Στο ίδιο αρχείο **Info.plist** , βεβαιωθείτε ότι έχετε επιλέξει την **Ενεργοποίηση λειτουργιών φόντου** και **Απομακρυσμένο ειδοποιήσεων**. 

    ![][8]

6. Εκτελέστε την εφαρμογή στη συσκευή που έχουν που σχετίζονται με αυτό το προφίλ δημοσίευσης. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png
