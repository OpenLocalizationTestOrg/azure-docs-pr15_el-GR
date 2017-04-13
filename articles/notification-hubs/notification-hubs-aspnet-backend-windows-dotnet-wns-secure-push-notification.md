<properties
    pageTitle="Ειδοποίηση Azure διανομείς ασφαλούς Push"
    description="Μάθετε πώς μπορείτε να στείλετε τις ειδοποιήσεις push ασφαλή στο Azure. Δείγματα κώδικα γραμμένο σε C# με χρήση του API .NET."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Ειδοποίηση Azure διανομείς ασφαλούς Push

> [AZURE.SELECTOR]
- [Παγκόσμια των Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Επισκόπηση

Υποστήριξη ειδοποιήσεων Push στο Microsoft Azure επιτρέπει να αποκτήσετε πρόσβαση σε μια υποδομής εύκολο για χρήση, multiplatform, κλιμάκωση ανάληψης push που απλοποιεί σε μεγάλο βαθμό την εφαρμογή των ειδοποιήσεων push για καταναλωτές και για μεγάλες επιχειρήσεις στις εφαρμογές για κινητές συσκευές πλατφόρμες.

Λόγω ρυθμιστικές ή τους περιορισμούς ασφαλείας, μερικές φορές μια εφαρμογή μπορεί να θέλετε να συμπεριλάβετε κάτι στην ειδοποίηση που δεν είναι δυνατό να μεταδοθούν μέσω της υποδομής κοινοποίηση τυπική push. Αυτό το πρόγραμμα εκμάθησης περιγράφει τον τρόπο για να επιτύχετε το ίδιο εμπειρία με την αποστολή ευαίσθητες πληροφορίες μέσω ασφαλή, με έλεγχο ταυτότητας σύνδεσης μεταξύ της συσκευής προγράμματος-πελάτη και την εφαρμογή παρασκηνίου.

Σε υψηλό επίπεδο, η ροή είναι ως εξής:

1. Η εφαρμογή παρασκηνίου:
    - Ασφαλής φορτίο αποθηκεύονται στη βάση δεδομένων παρασκηνίου.
    - Στέλνει το Αναγνωριστικό του αυτή η ειδοποίηση στη συσκευή (καμία ασφαλούς πληροφορία δεν αποστέλλεται).
2. Η εφαρμογή από τη συσκευή, όταν λάβετε την ειδοποίηση:
    - Η συσκευή επαφών του παρασκηνίου που ζητά το φορτίο ασφαλή.
    - Η εφαρμογή μπορεί να εμφανίσει το φορτίο ως μια ειδοποίηση στη συσκευή.

Είναι σημαντικό να λάβετε υπόψη ότι το προηγούμενο ροής (και σε αυτό το πρόγραμμα εκμάθησης), θα σας λαμβάνεται ως δεδομένο ότι η συσκευή αποθηκεύει ένα διακριτικό ελέγχου ταυτότητας στον τοπικό χώρο αποθήκευσης, αφού ο χρήστης συνδέεται. Αυτό εγγυάται μια εντελώς απρόσκοπτη εμπειρία, όπως η συσκευή να ανακτήσετε την ειδοποίηση ασφαλούς φορτίο χρησιμοποιώντας αυτό το διακριτικό. Εάν η εφαρμογή σας δεν αποθηκεύει κωδικοί ελέγχου ταυτότητας στη συσκευή ή εάν αυτά τα διακριτικά μπορεί να έχει λήξει, την εφαρμογή συσκευή, μόλις λάβετε την ειδοποίηση πρέπει να εμφανίζει μια γενική ειδοποίηση ερώτηση στο χρήστη για να ξεκινήσετε την εφαρμογή. Η εφαρμογή πραγματοποιεί έλεγχο ταυτότητας χρήστη, στη συνέχεια, και εμφανίζει το φορτίο ειδοποίησης.

Αυτό το πρόγραμμα εκμάθησης ασφαλούς Push δείχνει πώς μπορείτε να στείλετε μια ειδοποίηση push με ασφάλεια. Το πρόγραμμα εκμάθησης δημιουργεί στην το πρόγραμμα εκμάθησης [Ειδοποιήστε τους χρήστες](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) , έτσι θα πρέπει να ολοκληρώσετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης πρώτα.

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε δημιουργήσει και ρυθμιστεί το Κέντρο ειδοποίηση σας, όπως περιγράφεται σε [Γρήγορα αποτελέσματα με ειδοποίηση διανομείς (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
Επίσης, σημειώστε ότι το Windows Phone 8.1 απαιτεί διαπιστευτήρια των Windows (όχι Windows Phone) και ότι δεν λειτουργούν σε Windows Phone 8.0 ή Silverlight 8.1 εργασίες στο παρασκήνιο. Για τις εφαρμογές του Windows Store, μπορείτε να λαμβάνετε ειδοποιήσεις μέσω μιας εργασίας φόντου μόνο εάν η εφαρμογή είναι ενεργοποιημένες οθόνη κλειδώματος (κάντε κλικ στο πλαίσιο ελέγχου στο το Appmanifest).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Τροποποίηση του έργου Windows Phone

1. Στο έργο **NotifyUserWindowsPhone** , προσθέστε τον ακόλουθο κώδικα στο App.xaml.cs για να καταχωρήσετε την εργασία στο παρασκήνιο push. Προσθέστε την ακόλουθη γραμμή κώδικα στο τέλος του `OnLaunched()` μέθοδο:

        RegisterBackgroundTask();

2. Ενώ βρίσκεστε ακόμη στο App.xaml.cs, προσθέστε τα ακόλουθα κώδικα αμέσως μετά το `OnLaunched()` μέθοδο:

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. Προσθέστε τα ακόλουθα `using` δηλώσεις στο επάνω μέρος του αρχείου App.xaml.cs:

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. Από το μενού " **αρχείο** " στο Visual Studio, κάντε κλικ στην επιλογή **Αποθήκευση όλων**.

## <a name="create-the-push-background-component"></a>Δημιουργία στοιχείου φόντου Push

Το επόμενο βήμα είναι να δημιουργήσετε το στοιχείο push φόντου.

1. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στον κόμβο ανώτατου επιπέδου της λύσης (**SecurePush λύση** σε αυτήν την περίπτωση), στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**, στη συνέχεια, κάντε κλικ στην επιλογή **Νέο έργο**.

2. Ανάπτυξη **Εφαρμογών του χώρου αποθήκευσης**, στη συνέχεια, κάντε κλικ στην επιλογή **Windows Phone εφαρμογές**και, στη συνέχεια, κάντε κλικ στο **Στοιχείο χρόνου εκτέλεσης των Windows (Windows Phone)**. Ονομάστε το έργο **PushBackgroundComponent**και, στη συνέχεια, κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο.

    ![][12]

3. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **PushBackgroundComponent (Windows Phone 8.1)** , στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **τάξης**. Το όνομα της νέας κλάσης **PushBackgroundTask.cs**. Κάντε κλικ στο κουμπί **Προσθήκη** για να δημιουργήσετε την κλάση.

4. Αντικαταστήστε όλα τα περιεχόμενα του ορισμού της **PushBackgroundComponent** χώρο ονομάτων με τον ακόλουθο κώδικα, αντικαθιστώντας το σύμβολο κράτησης θέσης `{back-end endpoint}` με το τελικό σημείο παρασκηνίου που λαμβάνονται κατά την ανάπτυξη του παρασκηνίου:

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **PushBackgroundComponent (Windows Phone 8.1)** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.

6. Στην αριστερή πλευρά, κάντε κλικ στην επιλογή **Online**.

7. Στο πλαίσιο **αναζήτησης** , πληκτρολογήστε **Http προγράμματος-πελάτη**.

8. Στη λίστα των αποτελεσμάτων, κάντε κλικ στην επιλογή **Microsoft βιβλιοθήκες προγράμματος-πελάτη HTTP**και, στη συνέχεια, κάντε κλικ στην επιλογή **εγκατάσταση**. Ολοκλήρωση της εγκατάστασης.

9. Πίσω στο πλαίσιο NuGet **αναζήτησης** , πληκτρολογήστε **Json.net**. Εγκατάσταση του πακέτου **Json.NET** και, στη συνέχεια, κλείστε το παράθυρο Διαχείριση πακέτου NuGet.

10. Προσθέστε τα ακόλουθα `using` δηλώσεις στο επάνω μέρος του αρχείου **PushBackgroundTask.cs** :

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. Στην Εξερεύνηση λύσεων, μέσα στο έργο **NotifyUserWindowsPhone (Windows Phone 8.1)** , κάντε δεξί κλικ σε **αναφορές**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη αναφοράς...**. Στο παράθυρο διαλόγου Διαχείριση αναφορά, επιλέξτε το πλαίσιο δίπλα στην επιλογή **PushBackgroundComponent**και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

12. Στην Εξερεύνηση λύσεων, κάντε διπλό κλικ στο **Package.appxmanifest** του έργου **NotifyUserWindowsPhone (Windows Phone 8.1)** . Στην περιοχή **ειδοποιήσεων**, ορίστε **Με δυνατότητα αναδυόμενη** σε **Ναι**.

    ![][3]

13. Ενώ βρίσκεστε ακόμη στο **Package.appxmanifest**, κάντε κλικ στο μενού **δηλώσεων** κοντά στο επάνω μέρος. Στην αναπτυσσόμενη λίστα **Διαθέσιμες δηλώσεων** , κάντε κλικ στην επιλογή **Εργασίες στο παρασκήνιο**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

14. Στο **Package.appxmanifest**, στην ενότητα **Ιδιότητες**, επιλέξτε **Push ειδοποίησης**.

15. Στο **Package.appxmanifest**, στην περιοχή **Ρυθμίσεις εφαρμογής**, πληκτρολογήστε **PushBackgroundComponent.PushBackgroundTask** στο πεδίο **Σημείο εισόδου** .

    ![][13]

16. Από το μενού **αρχείο** , κάντε κλικ στην επιλογή **Αποθήκευση όλων**.

## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή

Για να εκτελέσετε την εφαρμογή, κάντε τα εξής:

1. Στο Visual Studio, εκτελέστε την εφαρμογή Web API **AppBackend** . Εμφανίζεται μια σελίδα web ASP.NET.

2. Στο Visual Studio, εκτελέστε την εφαρμογή Windows Phone **NotifyUserWindowsPhone (Windows Phone 8.1)** . Το Windows Phone προσομοίωσης εκτελείται και φορτώνει αυτόματα την εφαρμογή.

3. Στην εφαρμογή **NotifyUserWindowsPhone** περιβάλλοντος εργασίας Χρήστη, πληκτρολογήστε ένα όνομα χρήστη και τον κωδικό πρόσβασης. Αυτά μπορεί να είναι οποιαδήποτε συμβολοσειρά, αλλά ο χρήστης πρέπει να έχει την ίδια τιμή.

4. Στην εφαρμογή **NotifyUserWindowsPhone** περιβάλλοντος εργασίας Χρήστη, κάντε κλικ στην επιλογή **συνδεθείτε και καταχώρηση**. Στη συνέχεια, κάντε κλικ στην επιλογή **Αποστολή push**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
