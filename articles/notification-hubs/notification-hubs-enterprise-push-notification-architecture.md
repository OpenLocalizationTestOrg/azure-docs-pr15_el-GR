<properties
    pageTitle="Ειδοποίηση διανομείς - αρχιτεκτονική Push για μεγάλες επιχειρήσεις"
    description="Καθοδήγηση σχετικά με τη χρήση Azure ειδοποίηση διανομείς σε ένα εταιρικό περιβάλλον"
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="enterprise-push-architectural-guidance"></a>Αρχιτεκτονική οδηγίες push για μεγάλες επιχειρήσεις

Επιχειρήσεις σήμερα είναι σταδιακά Μετακίνηση για τη δημιουργία εφαρμογές για κινητές συσκευές για τους τελικούς χρήστες (εξωτερικό) ή για τους εργαζόμενους (εσωτερική). Έχουν υπάρχοντα συστήματα παρασκηνίου στη θέση είτε πρόκειται για τους κεντρικούς υπολογιστές ή σε ορισμένες εφαρμογές LoB που πρέπει να ενοποιηθεί η αρχιτεκτονική κινητής εφαρμογής. Αυτός ο οδηγός θα μιλήσετε σχετικά με τον καλύτερο τρόπο για να το κάνετε αυτό ενοποίησης συνιστά πιθανή λύση για να συνηθισμένα σενάρια.

Συχνές απαίτηση είναι για την αποστολή ειδοποιήσεων push στους χρήστες μέσω κινητής εφαρμογής τους, όταν παρουσιάζεται κάποιο συμβάν που σας ενδιαφέρουν στα συστήματα παρασκηνίου. Π.χ. ένας πελάτης τράπεζα που έχει τραπεζική τραπεζικές εφαρμογής από το iPhone θέλει να λαμβάνετε ειδοποιήσεις όταν γίνεται χρεωστική επάνω από ένα συγκεκριμένο ποσό από το λογαριασμό ή ένα σενάριο intranet όπου έναν υπάλληλο από το οικονομικό τμήμα που έχει μια εφαρμογή έγκρισης προϋπολογισμού από το Windows Phone θέλει να ενημερώνεστε όταν να λαμβάνει μια αίτηση έγκρισης.

Τραπεζικού λογαριασμού ή της επεξεργασίας έγκρισης είναι πιθανό να γίνουν με ορισμένες συστήματος υπολογιστή στο παρασκήνιο, το οποίο πρέπει να ξεκινήσετε μια push στο χρήστη. Μπορεί να υπάρχουν πολλά όπως συστήματα παρασκηνίου που όλα πρέπει να δημιουργήσετε το ίδιο είδος λογική για την υλοποίηση push όταν ένα συμβάν ενεργοποιεί μια ειδοποίηση. Δείτε την πολυπλοκότητα βρίσκεται στην ενοποίηση διάφορα συστήματα παρασκηνίου μαζί με ένα μεμονωμένο push σύστημα όπου οι τελικοί χρήστες ενδέχεται να έχετε εγγραφεί σε διαφορετικές ειδοποιήσεις και μπορεί να έχουν πολλές εφαρμογές για κινητές συσκευές π.χ., στην περίπτωση intranet εφαρμογές για κινητές συσκευές όπου μία κινητή εφαρμογή μπορεί να θέλετε να λαμβάνετε ειδοποιήσεις από πολλούς τέτοιου είδους συστήματα παρασκηνίου. Τα συστήματα παρασκηνίου δεν γνωρίζετε ή πρέπει να γνωρίζετε push σημασιολογία/τεχνολογίας, ώστε να δείτε μια συνηθισμένη λύση παραδοσιακά έχει γίνει να εισαγάγει ένα στοιχείο το οποίο σταθμοσκοπεί τα συστήματα παρασκηνίου για τυχόν συμβάντων που σας ενδιαφέρουν και είναι υπεύθυνος για την αποστολή των μηνυμάτων push στον υπολογιστή-πελάτη.
Εδώ, θα αναφερθούμε μια λύση ακόμα καλύτερα με χρήση Azure Service Bus - μοντέλο θέμα/συνδρομή, τα οποία θα μειώστε την πολυπλοκότητα κατά τη δημιουργία της λύσης μεταβλητού μεγέθους.

Ακολουθεί η γενική αρχιτεκτονική της λύσης (γενικευμένη με πολλές εφαρμογές για κινητές συσκευές αλλά εξίσου ισχύει όταν υπάρχει μόνο μία εφαρμογή για κινητές συσκευές)

## <a name="architecture"></a>Αρχιτεκτονική

![][1]

Το τμήμα κλειδιού σε αυτό το διάγραμμα αρχιτεκτονική είναι Bus υπηρεσίας Azure που παρέχει μια θέματα/συνδρομές προγραμματισμού μοντέλο (περισσότερες πληροφορίες σε αυτό στην [υπηρεσία Bus Pub/Sub προγραμματισμού]). Το ακουστικό, που βρίσκεται σε αυτήν την περίπτωση, κινητή υπόβαθρο (συνήθως [Υπηρεσίας κινητών τηλεφώνων του Azure], η οποία θα ξεκινήσει μια push στις εφαρμογές του mobile) δεν λαμβάνει μηνύματα απευθείας από τα συστήματα παρασκηνίου, αλλά αντί για αυτό που έχετε ένα επίπεδο ενδιάμεσου αφαίρεσης που παρέχεται από [Bus υπηρεσίας Azure] που επιτρέπει την κινητή υπόβαθρο για να λαμβάνετε μηνύματα από ένα ή περισσότερα συστήματα παρασκηνίου. Ένα θέμα Bus υπηρεσίας πρέπει να δημιουργηθούν για κάθε ένα από τα συστήματα παρασκηνίου λογαριασμό π.χ., HR, τιμολογίων, οι οποίες είναι στην ουσία "θέματα", που σας ενδιαφέρουν που θα ξεκινήσετε την αποστολή ως ειδοποιήσεων push μηνυμάτων. Τα συστήματα υποστήριξης θα σας στέλνει μηνύματα σε αυτά τα θέματα. Ένα κινητό παρασκηνίου να εγγραφείτε σε ένα ή περισσότερα θέματα, δημιουργώντας μια συνδρομή Bus υπηρεσίας. Αυτό θα παρέχουν δικαίωμα υπόβαθρο κινητές συσκευές για να λάβετε μια ειδοποίηση από το αντίστοιχο σύστημα διακομιστή. Κινητές συσκευές παρασκηνίου εξακολουθεί να ακούσετε για τα μηνύματα σε τους συνδρομές και μόλις φτάσει ένα μήνυμα, ενεργοποιεί ξανά και αποστέλλει ως ειδοποίηση για να το διανομέα της ειδοποίησης. Ειδοποίηση διανομείς, στη συνέχεια, τελικά προσφέρει το μήνυμα για την εφαρμογή για κινητές συσκευές. Επομένως, για να συνοψίσετε τα βασικά στοιχεία, έχουμε:

1. Συστήματα παρασκηνίου (συστήματα LoB/παλαιού τύπου)
    - Δημιουργεί θέμα Bus υπηρεσίας
    - Στέλνει μήνυμα
2. Κινητές συσκευές παρασκηνίου
    - Δημιουργεί τη συνδρομή υπηρεσίας
    - Λαμβάνει μήνυμα (από το σύστημα παρασκηνίου)
    - Στέλνει ειδοποίηση στους υπολογιστές-πελάτες (μέσω διανομέας ειδοποίηση Azure)
3. Εφαρμογή κινητές συσκευές
    - Λαμβάνει και να εμφανίσετε ειδοποίησης

###<a name="benefits"></a>Πλεονεκτήματα:

1. Η αποσύνδεση μεταξύ του δέκτη (κινητής εφαρμογής/υπηρεσία μέσω διανομέα ειδοποίηση) και του αποστολέα (συστήματα παρασκηνίου) επιτρέπει επιπλέον παρασκηνίου συστήματα που ενσωματωμένο με ελάχιστους αλλαγή.
2. Αυτό σημαίνει ότι το σενάριο από πολλές εφαρμογές για κινητές συσκευές Αδυναμία λήψης συμβάντα από ένα ή περισσότερα συστήματα παρασκηνίου.  

## <a name="sample"></a>Δείγμα:

###<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Θα πρέπει να ολοκληρώσετε τα παρακάτω προγράμματα εκμάθησης για να εξοικειωθείτε με το έννοιες, καθώς και δημιουργίας κοινών & βήματα ρύθμισης παραμέτρων:

1. [Υπηρεσία Bus Pub/Sub προγραμματισμού] - αυτό εξηγεί τις λεπτομέρειες της εργασίας με θέματα Bus υπηρεσίας/συνδρομές, πώς μπορείτε να δημιουργήσετε ένα χώρο ονομάτων ώστε να περιέχει θέματα/συνδρομές, πώς μπορείτε να στέλνετε και λαμβάνετε μηνύματα από αυτά.
2. [Ειδοποίηση διανομείς - Windows καθολικής πρόγραμμα εκμάθησης] - αυτό εξηγεί τον τρόπο για να ρυθμίσετε μια εφαρμογή για Windows Store και χρήση ειδοποιήσεων διανομείς για την καταχώρηση και, στη συνέχεια, να λαμβάνετε ειδοποιήσεις.

###<a name="sample-code"></a>Δείγμα κώδικα

Το πλήρες δείγμα κώδικα είναι διαθέσιμη από την [Ειδοποίηση διανομέα δείγματα]. Χωρίζεται σε τρία στοιχεία:

1. **EnterprisePushBackendSystem**

    μια. Αυτό το έργο χρησιμοποιεί το πακέτο Nuget *WindowsAzure.ServiceBus* και βασίζεται σε [υπηρεσία Bus Pub/Sub προγραμματισμού].

    β. Αυτή είναι μια απλή C# κονσόλα εφαρμογή για να προσομοιώσετε ένα σύστημα LoB που ξεκινά το μήνυμα να παραδοθεί την εφαρμογή για κινητές συσκευές.

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c. `CreateTopic`χρησιμοποιείται για να δημιουργήσετε το θέμα της υπηρεσίας Bus όπου θα στείλουμε μηνύματα.

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d. `SendMessage`χρησιμοποιείται για να στείλετε τα μηνύματα σε αυτό το θέμα Bus υπηρεσίας. Εδώ θα σας στέλνουν μηνύματα απλώς ένα σύνολο τυχαία μηνύματα στο θέμα περιοδικά για το δείγμα. Κανονικά θα υπάρξει ένα σύστημα υπολογιστή στο παρασκήνιο, το οποίο θα σας στέλνει μηνύματα όταν παρουσιάζεται κάποιο συμβάν.

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    μια. Αυτό το έργο χρησιμοποιεί τα πακέτα *WindowsAzure.ServiceBus* και *Microsoft.Web.WebJobs.Publish* Nuget και βασίζεται σε [υπηρεσία Bus Pub/Sub προγραμματισμού].

    β. Αυτή είναι μια άλλη C# κονσόλα app το οποίο θα μπορούμε να εκτελέσουμε ως μια [Azure WebJob] δεδομένου ότι πρέπει να εκτελεστούν συνεχώς για να ακούσετε για τα μηνύματα από τα συστήματα LoB/παρασκηνίου. Αυτό θα είναι μέρος του κινητού παρασκηνίου.

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c. `CreateSubscription`χρησιμοποιείται για τη δημιουργία μιας συνδρομής Bus υπηρεσίας για το θέμα όπου το σύστημα υποστήριξης θα σας στέλνει μηνύματα. Ανάλογα με το σενάριο επιχειρήσεις, αυτό το στοιχείο θα δημιουργήσετε μία ή περισσότερες εγγραφές σε αντίστοιχο θέματα (π.χ., ορισμένες πιθανό να λαμβάνει μηνύματα από το σύστημα HR, ορισμένες από το σύστημα οικονομικό και ούτω καθεξής)

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d. ReceiveMessageAndSendNotification χρησιμοποιείται για την ανάγνωση του μηνύματος από το θέμα χρησιμοποιώντας συνδρομή του και εάν η ανάγνωση είναι επιτυχής, στη συνέχεια, δημιουργήστε μια ειδοποίηση (στο σενάριο δείγμα μια εγγενή αναδυόμενη ειδοποίηση Windows) για να σταλεί στην κινητή εφαρμογή χρησιμοποιώντας Azure ειδοποίηση διανομείς.

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    ε. Για τη δημοσίευση ενός **WebJob**αυτό, κάντε δεξί κλικ στην της λύσης στο Visual Studio και επιλέξτε **Δημοσίευση ως WebJob**

    ![][2]

    f. Επιλέξτε το προφίλ σας δημοσίευσης και δημιουργήστε μια νέα τοποθεσία Web Azure Εάν δεν υπάρχει ήδη που θα φιλοξενήσει αυτό WebJob και όταν έχετε ολοκληρώσει την τοποθεσία Web, στη συνέχεια, **Δημοσίευση**.

    ![][3]

    γ. Ρυθμίστε τις παραμέτρους της εργασίας να είναι "Εκτέλεση συνεχώς", έτσι ώστε όταν συνδέεστε στο [Azure κλασική πύλη] θα πρέπει να δείτε κάτι παρόμοιο με το εξής:

    ![][4]


3. **EnterprisePushMobileApp**

    μια. Αυτή είναι μια εφαρμογή του Windows Store που θα λαμβάνουν ειδοποιήσεις αναδυόμενη από το WebJob εκτελείται ως μέρος του κινητού παρασκηνίου και να την εμφανίσετε. Αυτό βασίζεται στην [Ειδοποίηση διανομείς - Windows καθολικής πρόγραμμα εκμάθησης].  

    β. Βεβαιωθείτε ότι είναι ενεργοποιημένη η εφαρμογή σας για να λαμβάνετε ειδοποιήσεις αναδυόμενη.

    c. Βεβαιωθείτε ότι οι παρακάτω διανομείς ειδοποίησης που ονομάζεται κώδικα εγγραφής με την εφαρμογή εκκίνησης (αφού αντικαταστήσετε το *HubName* και *DefaultListenSharedAccessSignature*:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Δείγμα εκτελείται:

1. Βεβαιωθείτε ότι το WebJob εκτελείται με επιτυχία και έχει προγραμματιστεί να "Εκτέλεση συνεχώς".
2. Εκτελέστε το **EnterprisePushMobileApp** που θα ξεκινήσετε την εφαρμογή Windows Store.
3. Εκτελέστε την εφαρμογή κονσόλας **EnterprisePushBackendSystem** που να προσομοιώσετε υπόβαθρο LoB και θα ξεκινήσετε την αποστολή μηνυμάτων και θα πρέπει να βλέπετε ειδοποιήσεις αναδυόμενη εμφανίζονται όπως το εξής:

    ![][5]

4. Τα μηνύματα έχουν σταλεί αρχικά σε θέματα Bus υπηρεσίας που ήταν παρακολουθείται από συνδρομές Bus υπηρεσίας στην εργασία σας Web. Όταν ένα μήνυμα παραλήφθηκε, μια ειδοποίηση δημιουργήθηκε και στάλθηκε για την εφαρμογή για κινητές συσκευές. Μπορείτε να δείτε τα αρχεία καταγραφής WebJob για να επιβεβαιώσετε την επεξεργασία όταν μεταβείτε στη σύνδεση αρχείων καταγραφής στην [Πύλη κλασική Azure] για την εργασία σας Web:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Δείγματα διανομέα ειδοποίησης]: https://github.com/Azure/azure-notificationhubs-samples
[Azure υπηρεσία κινητών τηλεφώνων]: http://azure.microsoft.com/documentation/services/mobile-services/
[Bus Azure υπηρεσίας]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Υπηρεσία Bus Pub/Sub προγραμματισμού]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Ειδοποίηση διανομείς - Windows καθολικής πρόγραμμα εκμάθησης]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure κλασική πύλη]: https://manage.windowsazure.com/
