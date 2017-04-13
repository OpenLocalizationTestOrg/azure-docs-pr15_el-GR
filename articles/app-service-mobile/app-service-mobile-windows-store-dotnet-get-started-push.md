<properties
    pageTitle="Προσθήκη ειδοποιήσεων push σε εφαρμογή της γενικής χρήσης πλατφόρμας των Windows (UWP) | Azure εφαρμογές για κινητές συσκευές"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε εφαρμογές του Mobile Azure εφαρμογής υπηρεσίας και διανομείς Azure ειδοποίηση για την αποστολή ειδοποιήσεων push σε εφαρμογή της γενικής χρήσης πλατφόρμας των Windows (UWP)."
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>Προσθέστε τις ειδοποιήσεις push για την εφαρμογή των Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Επισκόπηση

Σε αυτό το πρόγραμμα εκμάθησης, προσθέτετε τις ειδοποιήσεις push στο έργο [έναρξης των Windows γρήγορης](app-service-mobile-windows-store-dotnet-get-started.md) ώστε να αποστέλλεται μια ειδοποίηση push στη συσκευή κάθε φορά που έχει εισαχθεί μια εγγραφή.

Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, θα χρειαστεί το πακέτο επέκτασης ειδοποιήσεων push. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="configure-hub"></a>Ρύθμιση παραμέτρων διανομέα ειδοποίησης

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>Καταχώρηση της εφαρμογής για τις ειδοποιήσεις push

Πρέπει να υποβάλετε την εφαρμογή σας στο Windows Store και, στη συνέχεια, ρύθμιση παραμέτρων του έργου σας διακομιστή για την ενσωμάτωση με το Windows ειδοποίηση υπηρεσιών (WNS) για να στείλετε push.

1. Στην Εξερεύνηση λύσεων Visual Studio, κάντε δεξί κλικ στο έργο εφαρμογής UWP, κάντε κλικ στην επιλογή **Αποθήκευση** > **Συσχετίσετε εφαρμογή με το χώρο αποθήκευσης...**. 

    ![Συσχετισμός εφαρμογή με το Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. Στον οδηγό, κάντε κλικ στο κουμπί **Επόμενο**, συνδεθείτε με το λογαριασμό Microsoft, πληκτρολογήστε ένα όνομα για την εφαρμογή σας στο **απόθεμα ένα νέο όνομα εφαρμογής**και κατόπιν κάντε κλικ στην επιλογή **απόθεμα**.

3. Μετά την καταχώρηση εφαρμογής δημιουργήθηκε με επιτυχία, επιλέξτε το νέο όνομα εφαρμογής, κάντε κλικ στο κουμπί **Επόμενο**και, στη συνέχεια, κάντε κλικ στην επιλογή **Συσχέτιση**. Αυτό προσθέτει τις απαιτούμενες πληροφορίες εγγραφής του Windows Store η δήλωση εφαρμογής.  

7. Μεταβείτε στο [Κέντρο ανάπτυξης των Windows](https://dev.windows.com/en-us/overview), πραγματοποιήστε είσοδο με το λογαριασμό της Microsoft, κάντε κλικ στην επιλογή νέα εφαρμογή εγγραφές **οι εφαρμογές μου**, στη συνέχεια, αναπτύξτε τις **υπηρεσίες** > **ειδοποιήσεις προώθησης**. 

8. Στη σελίδα **ειδοποιήσεις Push** , κάντε κλικ στην επιλογή **τοποθεσία Live υπηρεσιών** στην περιοχή **Microsoft Azure Mobile υπηρεσίες**.

9. Στη σελίδα εγγραφής, σημειώστε την τιμή στην περιοχή **εφαρμογή απορρήτου** και το **Αναγνωριστικό πακέτου ΑΣΦΑΛΕΊΑΣ**, που θα χρησιμοποιήσετε Επόμενο για να ρυθμίσετε τις παραμέτρους του παρασκηνίου εφαρμογής για κινητές συσκευές. 

    ![Συσχετισμός εφαρμογή με το Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] Το μυστικό προγράμματος-πελάτη και πακέτο αναγνωριστικό ΑΣΦΑΛΕΊΑΣ είναι τα διαπιστευτήριά σημαντική ασφαλείας. Μην κάνετε κοινή χρήση αυτές τις τιμές με οποιονδήποτε ή διανείμετε μαζί με την εφαρμογή σας. Το **Αναγνωριστικό εφαρμογής** χρησιμοποιείται με το μυστικό για ρύθμιση παραμέτρων ελέγχου ταυτότητας λογαριασμού Microsoft.

##<a name="configure-the-backend-to-send-push-notifications"></a>Ρύθμιση παραμέτρων του υπολογιστή στο παρασκήνιο για την αποστολή ειδοποιήσεων push

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a id="update-service"></a>Ενημέρωση του διακομιστή για την αποστολή ειδοποιήσεων push

Χρησιμοποιήστε την παρακάτω διαδικασία που ταιριάζει με τον τύπο έργου παρασκηνίου&mdash; [παρασκηνίου .NET](#dotnet) ή [Node.js παρασκηνίου](#nodejs).

### <a name="dotnet"></a>Έργο παρασκηνίου .NET

1. Στο Visual Studio, κάντε δεξί κλικ στο έργο διακομιστή και κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**, αναζητήστε Microsoft.Azure.NotificationHubs και κατόπιν κάντε κλικ στην επιλογή **εγκατάσταση**. Αυτό εγκαθιστά στη βιβλιοθήκη του προγράμματος-πελάτη ειδοποίηση διανομείς.

2. Ανάπτυξη **ελεγκτές**, Άνοιγμα TodoItemController.cs και προσθέστε τα ακόλουθα χρήση προτάσεων:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. Στη μέθοδο **PostTodoItem** , προσθέστε τον ακόλουθο κώδικα μετά την κλήση σε **InsertAsync**:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Αυτός ο κωδικός σας ενημερώνει για την ενότητα "Ειδοποίηση" για να στείλετε μια ειδοποίηση push μετά την εισαγωγή ενός νέου στοιχείου.

4. Αναδημοσίευση του project server.

### <a name="nodejs"></a>Node.js παρασκηνίου έργου

1. Εάν να έχετε ήδη κάνει, [κάντε λήψη του έργου γρήγορη έναρξη](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) ή να χρησιμοποιήσετε τον [online πρόγραμμα επεξεργασίας στην πύλη του Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. Αντικαταστήσετε τον υπάρχοντα κωδικό στο αρχείο todoitem.js με τα εξής:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    Αυτή στέλνει μια αναδυόμενη ειδοποίηση WNS που περιέχει το item.text όταν προστίθεται ένα νέο στοιχείο todo.

2. Όταν επεξεργάζεστε το αρχείο στον τοπικό σας υπολογιστή, δημοσιεύστε ξανά το project server.

##<a id="update-app"></a>Προσθέστε τις ειδοποιήσεις push για την εφαρμογή σας

Στη συνέχεια, την εφαρμογή σας πρέπει να εγγραφείτε για τις ειδοποιήσεις push σε εκκίνησης. Όταν έχετε ενεργοποιήσει ήδη τον έλεγχο ταυτότητας, βεβαιωθείτε ότι ο χρήστης πρόσημα στο πριν να επιχειρήσετε να εγγραφείτε για τις ειδοποιήσεις push.

1. Ανοίξτε το αρχείο έργου **App.xaml.cs** και προσθέστε το εξής `using` προτάσεις:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. Στο ίδιο αρχείο, προσθέστε τον ακόλουθο ορισμό μέθοδο **InitNotificationsAsync** την κλάση **εφαρμογής** :

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Αυτός ο κωδικός ανακτά την ChannelURI για την εφαρμογή από WNS και, στη συνέχεια, να καταχωρεί που ChannelURI με την εφαρμογή εφαρμογής υπηρεσίας κινητών.

3. Στο επάνω μέρος του προγράμματος χειρισμού συμβάντων **OnLaunched** στο **App.xaml.cs**, προσθέστε το πλήκτρο τροποποίησης **ασύγχρονης** για τον ορισμό της μεθόδου και προσθέστε την παρακάτω κλήση για τη νέα μέθοδο **InitNotificationsAsync** , όπως στο ακόλουθο παράδειγμα:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Αυτό εγγυάται ότι το μικρής διάρκειας ChannelURI έχει καταχωρηθεί κάθε φορά που γίνεται εκκίνηση της εφαρμογής.

4. Δημιουργήστε ξανά το έργο σας UWP εφαρμογής. Εφαρμογή σας είναι τώρα έτοιμο για να λαμβάνετε ειδοποιήσεις αναδυόμενη.

##<a id="test"></a>Ειδοποιήσεις push δοκιμής στην εφαρμογή

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με τις ειδοποιήσεις push:

* [Πώς μπορείτε να χρησιμοποιήσετε το πρόγραμμα-πελάτη διαχειριζόμενων για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
Πρότυπα ευελιξία για την αποστολή ωθεί πλατφόρμες και μεταφρασμένα ωθεί. Μάθετε πώς μπορείτε να καταχωρήσετε τα πρότυπα.

* [Διάγνωση θεμάτων ειδοποιήσεων push](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Υπάρχουν διάφορες αιτίες γιατί ειδοποιήσεις ενδέχεται να λάβετε αποτεθεί ή δεν τελειώνει σε συσκευές. Αυτό το θέμα δείχνει πώς μπορείτε να αναλύσετε και να υπολογίσετε τη ρίζα αποτυχιών ειδοποιήσεων push. 

Μπορείτε να συνεχίσετε με ένα από τα παρακάτω προγράμματα εκμάθησης:

+ [Προσθήκη ελέγχου ταυτότητας για την εφαρμογή σας](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Μάθετε πώς να ελέγχουν την ταυτότητα χρηστών από την εφαρμογή σας με μια υπηρεσία παροχής ταυτότητας.

+ [Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή σας](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Μάθετε πώς μπορείτε να προσθέσετε την εφαρμογή σας χρησιμοποιώντας μια εφαρμογή Mobile παρασκηνίου υποστήριξη χωρίς σύνδεση. Συγχρονισμού χωρίς σύνδεση επιτρέπει στους τελικούς χρήστες να αλληλεπιδράσετε με μια εφαρμογή για κινητές συσκευές&mdash;προβολή, προσθήκη ή τροποποίηση δεδομένων&mdash;ακόμα και όταν δεν υπάρχει σύνδεση δικτύου.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

