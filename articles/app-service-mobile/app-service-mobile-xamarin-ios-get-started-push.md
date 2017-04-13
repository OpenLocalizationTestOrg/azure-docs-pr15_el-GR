<properties
    pageTitle="Προσθέστε τις ειδοποιήσεις push για την εφαρμογή σας Xamarin.iOS με Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure εφαρμογής υπηρεσίας για την αποστολή ειδοποιήσεων push σε εφαρμογή Xamarin.iOS"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>Προσθέστε τις ειδοποιήσεις push σας εφαρμογή Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Επισκόπηση

Σε αυτό το πρόγραμμα εκμάθησης, προσθέτετε τις ειδοποιήσεις push στο έργο [Xamarin.iOS Γρήγορη εκκίνηση](app-service-mobile-xamarin-ios-get-started.md) , έτσι ώστε να αποστέλλεται μια ειδοποίηση push στη συσκευή κάθε φορά που έχει εισαχθεί μια εγγραφή.

Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, θα χρειαστεί το πακέτο επέκτασης ειδοποιήσεων push. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Ολοκληρώστε το πρόγραμμα εκμάθησης [Xamarin.iOS γρήγορη έναρξη](app-service-mobile-xamarin-ios-get-started.md) .

* Μια συσκευή φυσικής iOS. Ειδοποιήσεις Push δεν υποστηρίζονται από το simulator iOS.

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Καταχώρηση της εφαρμογής για τις ειδοποιήσεις push στην πύλη για προγραμματιστές της Apple

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>Ρύθμιση των παραμέτρων σας εφαρμογή Mobile για την αποστολή ειδοποιήσεων push

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Ενημέρωση του project server για την αποστολή ειδοποιήσεων push

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>Ρύθμιση παραμέτρων του έργου σας Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>Προσθέστε τις ειδοποιήσεις push για την εφαρμογή σας

1. Στο **QSTodoService**, προσθέστε την ακόλουθη ιδιότητα ώστε να **AppDelegate** μπορεί να αποκτήσει το πρόγραμμα-πελάτη κινητών:

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. Προσθέστε τα ακόλουθα `using` δήλωση στο επάνω μέρος του αρχείου **AppDelegate.cs** .

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. Στο **AppDelegate**, παρακάμπτουν το συμβάν **FinishedLaunching** :

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. Στο ίδιο αρχείο, παρακάμπτουν το συμβάν **RegisteredForRemoteNotifications** . Σε αυτόν τον κωδικό δήλωση για μια ειδοποίηση απλό πρότυπο που θα σταλεί σε όλες τις υποστηριζόμενες πλατφόρμες από το διακομιστή.

    Για περισσότερες πληροφορίες σχετικά με τα πρότυπα με διανομείς ειδοποίησης, ανατρέξτε στο θέμα [πρότυπα](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. Παράκαμψη, στη συνέχεια, το συμβάν **DidReceivedRemoteNotification** :

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Εφαρμογή σας έχει ενημερωθεί για την υποστήριξη ειδοποιήσεων push.

## <a name="test"></a>Ειδοποιήσεις push δοκιμής στην εφαρμογή

1. Πατήστε το κουμπί **Εκτέλεση** για να δημιουργήσετε το έργο και ξεκινήστε την εφαρμογή στη συσκευή με δυνατότητα iOS και, στη συνέχεια, κάντε κλικ στο **κουμπί OK** για να αποδεχτείτε τις ειδοποιήσεις push.

    > [AZURE.NOTE] Πρέπει να αποδεχτείτε τις ειδοποιήσεις push ρητά από την εφαρμογή. Αυτή η αίτηση προκύπτει μόνο την πρώτη φορά που θα εκτελείται η εφαρμογή.

2. Στην εφαρμογή, πληκτρολογήστε μια εργασία και, στη συνέχεια, κάντε κλικ στο κουμπί συν (**+**) εικονίδιο.

3. Βεβαιωθείτε ότι μια ειδοποίηση λήψη, στη συνέχεια, κάντε κλικ στο **κουμπί OK** για να κλείσετε την ειδοποίηση.

4. Επαναλάβετε το βήμα 2 και αμέσως Κλείσιμο της εφαρμογής, στη συνέχεια, βεβαιωθείτε ότι εμφανίζεται μια ειδοποίηση.

Αυτό το πρόγραμμα εκμάθησης ολοκληρώθηκε με επιτυχία.

<!-- Images. -->

<!-- URLs. -->



