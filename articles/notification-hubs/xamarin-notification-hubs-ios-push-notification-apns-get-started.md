<properties
    pageTitle="iOS τις ειδοποιήσεις Push με διανομείς ειδοποίησης για τις εφαρμογές Xamarin | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων push σε μια εφαρμογή iOS Xamarin."
    services="notification-hubs"
    keywords="IOS push ειδοποιήσεις push μηνύματα, ειδοποιήσεις push, push μηνύματος"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS τις ειδοποιήσεις Push με διανομείς ειδοποίησης για τις εφαρμογές Xamarin

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Επισκόπηση
> [AZURE.IMPORTANT] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων push σε μια εφαρμογή του iOS.
Θα μπορείτε να δημιουργήσετε μια κενή εφαρμογή Xamarin.iOS που λαμβάνει τις ειδοποιήσεις push, χρησιμοποιώντας την [Υπηρεσία ειδοποιήσεων Push Apple (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). Όταν ολοκληρώσετε την εργασία, θα έχετε τη δυνατότητα να χρησιμοποιήσετε το Κέντρο ειδοποίηση σας μετάδοση ειδοποιήσεων push σε όλες τις συσκευές που εκτελεί την εφαρμογή σας. Ο κώδικας ολοκληρωθεί είναι διαθέσιμη στην [εφαρμογή NotificationHubs] [ GitHub] δείγμα.

Αυτό το πρόγραμμα εκμάθησης δείχνει το απλό push σενάριο εκπομπής μήνυμα με ειδοποίηση διανομείς.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ [Xcode 6.0][Install Xcode]
+ Μια iOS 7.0 (ή νεότερη έκδοση) συμβατή συσκευή
+ iOS συμμετοχή ως μέλος προγράμματος για προγραμματιστές
+ [Xamarin Studio]

   > [AZURE.NOTE] Λόγω απαιτήσεις ρύθμισης παραμέτρων για ειδοποιήσεις, push iOS πρέπει να μπορείτε να αναπτύξετε και να ελέγξετε το δείγμα εφαρμογής σε μια συσκευή φυσικής iOS (iPhone ή iPad) αντί στο το simulator.

Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλους άλλα προγράμματα εκμάθησης διανομείς ειδοποίηση για τις εφαρμογές iOS Xamarin.


[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##<a name="configure-your-notification-hub"></a>Ρύθμιση των παραμέτρων σας διανομέα ειδοποίησης

Αυτή η ενότητα σας καθοδηγεί στις τη δημιουργία διανομέα νέα ειδοποίηση και ρύθμιση παραμέτρων ελέγχου ταυτότητας με APNS χρησιμοποιώντας το πιστοποιητικό push **.p12** που δημιουργήσατε. Εάν θέλετε να χρησιμοποιήσετε ένα διανομέα ειδοποίησης που έχετε ήδη δημιουργήσει, μπορείτε να μεταβείτε στο βήμα 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>Καθώς θέλουμε να ρυθμίσει τις παραμέτρους APNS σύνδεσης, στην πύλη του Azure, ανοίξτε τις ρυθμίσεις ειδοποίησης διανομέα, κάντε κλικ στην <b>Ειδοποίηση υπηρεσίες</b>ande, και, στη συνέχεια, κάντε κλικ στο στοιχείο <b>Apple (APNS)</b> στη λίστα. Μετά την ολοκλήρωση, κάντε κλικ στην εντολή <b>Αποστολή πιστοποιητικό</b> και επιλέξτε το πιστοποιητικό <b>.p12</b> που εξαγάγατε νωρίτερα, καθώς και τον κωδικό πρόσβασης για το πιστοποιητικό.</p>
<p>Βεβαιωθείτε ότι έχετε επιλέξει τη λειτουργία <b>Sandbox</b> δεδομένου ότι θα στέλνετε μηνύματα push σε ένα περιβάλλον ανάπτυξης. Χρησιμοποιήστε τη ρύθμιση <b>παραγωγής</b> μόνο εάν θέλετε να στείλετε τις ειδοποιήσεις push στους χρήστες που έχουν αγοράσει ήδη την εφαρμογή από το χώρο αποθήκευσης.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)


Το Κέντρο ειδοποίηση σας τώρα έχει ρυθμιστεί ώστε να λειτουργεί με APNS και έχετε τις συμβολοσειρές σύνδεσης για να καταχωρήσετε την εφαρμογή και αποστολή ειδοποιήσεων push.


##<a name="connect-your-app-to-the-notification-hub"></a>Σύνδεση της εφαρμογής σας με την ενότητα "Ειδοποίηση"

#### <a name="create-a-new-project"></a>Δημιουργία νέου έργου

1. Στο Xamarin Studio, δημιουργήστε ένα νέο έργο iOS και επιλέξτε το **Ενοποιημένο σύστημα API** > **Μόνο Προβολή εφαρμογή** προτύπου.

    ![Xamarin Studio - τύπος επιλογής εφαρμογής][31]

2. Προσθέστε μια αναφορά στο στοιχείο Azure μηνυμάτων. Στην προβολή λύσης, κάντε δεξί κλικ στο φάκελο **στοιχεία** για το έργο σας και επιλέξτε **Λάβετε περισσότερα στοιχεία**. Αναζήτηση για το στοιχείο **Azure ανταλλαγής μηνυμάτων** και προσθέστε το στοιχείο στο έργο σας.

3. Στο **AppDelegate.cs**, προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση:

        using WindowsAzure.Messaging;

4. Δήλωση μια παρουσία του **SBNotificationHub**:

        private SBNotificationHub Hub { get; set; }

5. Δημιουργία μιας κλάσης **Constants.cs** με τις ακόλουθες μεταβλητές:

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6. Στο **AppDelegate.cs**, ενημερώστε **FinishedLaunching()** ώστε να συμφωνεί με τα εξής:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }

            return true;
        }

7. Παράκαμψη της μεθόδου **RegisteredForRemoteNotifications()** στο **AppDelegate.cs**:

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);

            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }

                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }

8. Παράκαμψη της μεθόδου **ReceivedRemoteNotification()** στο **AppDelegate.cs**:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. Δημιουργήστε την ακόλουθη μέθοδο **ProcessNotification()** στο **AppDelegate.cs**:

        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

                string alert = string.Empty;

                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();

                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }

    > [AZURE.NOTE] Μπορείτε να επιλέξετε την παράκαμψη **FailedToRegisterForRemoteNotifications()** να χειρίζεται τις περιπτώσεις όπως χωρίς σύνδεση δικτύου. Αυτό είναι ιδιαίτερα σημαντικό, όπου ο χρήστης μπορεί να εκκινήσετε την εφαρμογή σας σε λειτουργία χωρίς σύνδεση (π.χ., αεροπλάνου) και θέλετε να χειριστείτε push μηνυμάτων συγκεκριμένα σενάρια για την εφαρμογή σας.


10. Εκτελέστε την εφαρμογή στη συσκευή σας.


## <a name="sending-push-notifications"></a>Αποστολή ειδοποιήσεων Push


Μπορείτε να δοκιμάσετε να λαμβάνω ειδοποιήσεις push στην εφαρμογή σας με την αποστολή ειδοποιήσεων στην [Πύλη του Azure] μέσω τη δυνατότητα **Δοκιμή αποστολή** σε το σύνολο εργαλείων **Αντιμετώπιση προβλημάτων** , δεξιά στη σελίδα διανομέα ειδοποίησης, όπως φαίνεται στην παρακάτω οθόνη.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Ειδοποιήσεις Push κανονικά αποστέλλονται μέσω της υπηρεσίας υποστήριξης όπως υπηρεσίες Mobile ή ASP.NET χρησιμοποιώντας μια συμβατή βιβλιοθήκη. Μπορείτε επίσης να χρησιμοποιήσετε το REST API απευθείας για την αποστολή μηνυμάτων push εάν η βιβλιοθήκη δεν είναι διαθέσιμη στη δική σας περίπτωση. 

Σε αυτό το πρόγραμμα εκμάθησης, θα Διατηρήστε το απλό και μόνο δείχνουν δοκιμή της εφαρμογής υπολογιστή-πελάτη σας με την αποστολή ειδοποιήσεων με χρήση του SDK .NET για διανομείς ειδοποίηση σε μια εφαρμογή κονσόλας αντί για μια υπηρεσία υποστήριξης. Συνιστάται να το πρόγραμμα εκμάθησης [Χρήση διανομείς ειδοποίησης για τις ειδοποιήσεις push σε χρήστες](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) με το επόμενο βήμα για την αποστολή ειδοποιήσεων από μια παρασκηνίου ASP.NET. Ωστόσο, οι ακόλουθες προσεγγίσεις μπορεί να χρησιμοποιηθεί για την αποστολή ειδοποιήσεων:

* **Περιβάλλον εργασίας ΥΠΌΛΟΙΠΟ**: που μπορεί να υποστηρίξει ειδοποιήσεων push σε οποιαδήποτε πλατφόρμα παρασκηνίου χρησιμοποιώντας το [ΥΠΌΛΟΙΠΟ περιβάλλοντος εργασίας](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure ειδοποίηση διανομείς .NET SDK**: στο το Nuget διαχείρισης πακέτου για το Visual Studio, εκτελέστε [Microsoft.Azure.NotificationHubs πακέτου εγκατάστασης](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση από Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Εφαρμογές του Mobile**: για παράδειγμα του τρόπου για την αποστολή ειδοποιήσεων από μια παρασκηνίου Azure εφαρμογής υπηρεσίας κινητών εφαρμογών που είναι ενσωματωμένο στο διανομείς ειδοποίησης, ανατρέξτε στο θέμα [Προσθήκη ειδοποιήσεων push για την εφαρμογή για κινητές συσκευές](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java / PHP**: για ένα παράδειγμα του τρόπου αποστολής τις ειδοποιήσεις push, χρησιμοποιώντας τα ΥΠΌΛΟΙΠΑ API, ανατρέξτε στην ενότητα "Πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση από Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


####<a name="optional-send-push-notifications-from-a-net-console-app"></a>(Προαιρετικό) Αποστολή ειδοποιήσεων Push από μια εφαρμογή κονσόλας .NET

Σε αυτήν την ενότητα, θα στείλουμε τις ειδοποιήσεις push, χρησιμοποιώντας μια απλή εφαρμογή κονσόλας .NET της. Για τους σκοπούς της αυτό το παράδειγμα, θα σας θα μεταβείτε σε ένα περιβάλλον ανάπτυξης των Windows που έχει ήδη εγκατεστημένο το Visual Studio.

1. Στο Visual Studio, δημιουργήστε μια νέα εφαρμογή κονσόλας Visual C#:

    ![Visual Studio - Δημιουργία νέας εφαρμογής κονσόλας][213]

2. Στο Visual Studio, κάντε κλικ στην επιλογή **Εργαλεία**, κάντε κλικ στην επιλογή **Διαχείριση πακέτου NuGet**και, στη συνέχεια, κάντε κλικ στην επιλογή **Κονσόλα διαχείρισης πακέτου**.

    Θα πρέπει να εμφανίζεται η αγκύρωση στο κάτω μέρος του χώρου εργασίας του Visual Studio την Κονσόλα διαχείρισης πακέτου.

3. Στο παράθυρο Κονσόλα διαχείρισης πακέτου, ορίστε την **προεπιλεγμένη έργου** στο έργο σας νέα εφαρμογή κονσόλας και, στη συνέχεια, στο παράθυρο της κονσόλας, εκτελέστε την ακόλουθη εντολή:

        Install-Package Microsoft.Azure.NotificationHubs

    Αυτό προσθέτει μια αναφορά στο SDK διανομείς ειδοποίηση Azure χρησιμοποιώντας το <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">πακέτο Microsoft.Azure.Notification NuGet διανομείς</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Άνοιγμα του `Program.cs` αρχείων και προσθέστε το εξής `using` πρόταση, εξασφαλίζοντας ότι μπορούμε να χρησιμοποιήσουμε Azure κλάσεις και συναρτήσεων μέσα σε κύρια τάξη σας:

        using Microsoft.Azure.NotificationHubs;

3. Στο σας `Program` κλάση, προσθέστε την ακόλουθη μέθοδο (μην ξεχάσετε να αντικαταστήσετε τη **συμβολοσειρά σύνδεσης** και το **όνομα διανομέας**):

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. Προσθέστε τις ακόλουθες γραμμές στο σας `Main` μέθοδο:

         SendNotificationAsync();
         Console.ReadLine();

5. Πατήστε το πλήκτρο F5 για να εκτελέσετε την εφαρμογή. Μέσα σε δευτερόλεπτα, θα πρέπει να δείτε μια ειδοποίηση push εμφανίζονται στη συσκευή σας. Εάν χρησιμοποιείτε το Wi-Fi ή δίκτυο δεδομένων κινητής τηλεφωνίας, βεβαιωθείτε ότι έχετε μια ενεργή σύνδεση στο internet στη συσκευή.

Μπορείτε να βρείτε όλων των πιθανών φορτίων στο Apple [τοπικών και Οδηγό προγραμματισμού ειδοποιήσεων Push].

####<a name="optional-send-notifications-from-a-mobile-service"></a>(Προαιρετικό) Αποστολή ειδοποιήσεων από μια υπηρεσία κινητών τηλεφώνων

Σε αυτήν την ενότητα, θα στείλουμε ειδοποιήσεων push με μια κινητή υπηρεσία μέσω μιας δέσμης ενεργειών κόμβο.

Για να στείλετε μια ειδοποίηση, χρησιμοποιώντας μια υπηρεσία κινητών τηλεφώνων, ακολουθήστε [Γρήγορα αποτελέσματα με τις υπηρεσίες του Mobile], και στη συνέχεια:

1. Είσοδος στην [Πύλη κλασική Azure]και επιλέξτε την υπηρεσία κινητών τηλεφώνων.

2. Επιλέξτε την καρτέλα **Χρονοδιάγραμμα** στο επάνω μέρος.

    ![Azure κλασική πύλης - χρονοδιαγράμματος][215]

3. Δημιουργήστε μια νέα προγραμματισμένη εργασία, εισαγάγετε ένα όνομα και επιλέξτε **ζήτηση**.

    ![Azure κλασική πύλης - Δημιουργία νέας εργασίας][216]

4. Όταν δημιουργείται η εργασία, κάντε κλικ στο όνομα του έργου. Στη συνέχεια, κάντε κλικ στην καρτέλα **δέσμης ενεργειών** στην επάνω γραμμή.

5. Εισαγάγετε την ακόλουθη δέσμη ενεργειών μέσα στη συνάρτηση του χρονοδιαγράμματος. Βεβαιωθείτε ότι για να αντικαταστήσετε τους χαρακτήρες κράτησης θέσης με το όνομα διανομέας ειδοποίησης και τη συμβολοσειρά σύνδεσης για *DefaultFullSharedAccessSignature* που λάβατε νωρίτερα. Κάντε κλικ στην επιλογή **Αποθήκευση**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );


6. Κάντε κλικ στην επιλογή **Εκτέλεση μία φορά** στην κάτω γραμμή. Θα πρέπει να λαμβάνετε μια ειδοποίηση στη συσκευή σας.

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το απλό παράδειγμα μετάδοση ειδοποιήσεων push για όλες τις συσκευές σας iOS. Για να στοχεύετε σε συγκεκριμένους χρήστες, ανατρέξτε στο το πρόγραμμα εκμάθησης [Χρήση διανομείς ειδοποίησης για τις ειδοποιήσεις push στους χρήστες]. Εάν θέλετε να χωρίσετε τους χρήστες σας από τις ομάδες που σας ενδιαφέρουν, μπορείτε να διαβάσετε [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις]. Μάθετε περισσότερα σχετικά με τη χρήση διανομείς ειδοποίηση στο [Ειδοποίηση διανομείς καθοδήγηση] και την [Ειδοποίηση διανομείς οδηγίες για iOS].


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Γρήγορα αποτελέσματα με τις υπηρεσίες του Mobile]: /develop/mobile/tutorials/get-started-xamarin-ios
[Azure κλασική πύλη]: https://manage.windowsazure.com/
[Ειδοποίηση διανομείς καθοδήγηση]: http://msdn.microsoft.com/library/jj927170.aspx
[Ειδοποίηση διανομείς οδηγίες για iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Χρήση διανομείς ειδοποίησης για τις ειδοποιήσεις push στους χρήστες]: /manage/services/notification-hubs/notify-users-aspnet
[Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις]: /manage/services/notification-hubs/breaking-news-dotnet

[Τοπική και Οδηγός προγραμματισμού ειδοποιήσεων Push]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Πύλη του Azure]: https://portal.azure.com
