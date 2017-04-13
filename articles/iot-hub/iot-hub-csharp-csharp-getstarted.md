<properties
    pageTitle="Azure IoT διανομέας για C# γρήγορα αποτελέσματα | Microsoft Azure"
    description="Πρόγραμμα εκμάθησης για Azure IoT διανομέα με C# γρήγορα αποτελέσματα. Χρήση διανομέα IoT Azure και C# με το Microsoft Azure IoT SDK για την υλοποίηση μιας λύσης Internet των στοιχείων."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>Γρήγορα αποτελέσματα με το Κέντρο IoT Azure για .NET

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Στο τέλος αυτού του προγράμματος εκμάθησης, έχετε τρεις εφαρμογές κονσόλας Windows:

* **CreateDeviceIdentity**, που δημιουργεί μια συσκευή ταυτότητας και πλήκτρο συσχετισμένη ασφάλεια για να συνδέσετε τη συσκευή προσομοιωμένη.
* **ReadDeviceToCloudMessages**, που εμφανίζει το τηλεμετρίας αποστέλλονται από προσομοιωμένη τη συσκευή σας.
* **SimulatedDevice**, το οποίο συνδέεται με το Κέντρο IoT σας με την ταυτότητα της συσκευής που δημιουργήσατε νωρίτερα και στέλνει ένα μήνυμα τηλεμετρίας ανά δευτερόλεπτο με χρήση του πρωτοκόλλου AMQP.

> [AZURE.NOTE] Για πληροφορίες σχετικά με τα διάφορα SDK που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε δύο εφαρμογές για να εκτελέσετε σε συσκευές και το τέλος πίσω λύση, ανατρέξτε στο θέμα [SDK διανομέα IoT][lnk-hub-sdks].

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

+ Microsoft Visual Studio 2015.

+ Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Τώρα που έχετε δημιουργήσει το Κέντρο IoT σας και έχετε το όνομα κεντρικού υπολογιστή και η συμβολοσειρά σύνδεσης που χρειάζεστε για να ολοκληρώσετε τα υπόλοιπα αυτού του προγράμματος εκμάθησης.

## <a name="create-a-device-identity"></a>Δημιουργήστε μια ταυτότητα συσκευής

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Windows που δημιουργεί μια ταυτότητα συσκευής στο μητρώο ταυτότητας από την ενότητα IoT. Μια συσκευή δεν μπορεί να συνδεθεί με διανομέα IoT εκτός εάν έχει μια καταχώρηση στο μητρώο ταυτότητας συσκευή. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα "Συσκευή μητρώου ταυτότητα" του [Οδηγού για προγραμματιστές διανομέα IoT][lnk-devguide-identity]. Κατά την εκτέλεση αυτής της εφαρμογής κονσόλας, δημιουργεί μια συσκευή μοναδικό Αναγνωριστικό και το κλειδί που να χρησιμοποιήσετε τη συσκευή σας για να αναγνωρίζεται όταν στέλνει μηνύματα συσκευής στο cloud με IoT διανομέα.

1. Στο Visual Studio, προσθέστε ένα έργο Visual C# κλασική επιφάνεια εργασίας των Windows στην τρέχουσα λύση, χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Βεβαιωθείτε ότι η έκδοση του .NET Framework είναι 4.5.1 ή νεότερη έκδοση. Το όνομα του έργου **CreateDeviceIdentity**.

    ![Νέο έργο Visual C# κλασική επιφάνεια εργασίας των Windows][10]

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **CreateDeviceIdentity** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων Nuget**.

3. Στο παράθυρο **Διαχείριση πακέτου Nuget** , επιλέξτε **Αναζήτηση**, αναζητήστε **microsoft.azure.devices**, επιλέξτε **εγκατάσταση** για να εγκαταστήσετε το πακέτο **Microsoft.Azure.Devices** και αποδεχτείτε τους όρους χρήσης. Αυτή η διαδικασία στοιχεία λήψης, εγκαθιστά και προσθέτει την αναφορά στο [Microsoft Azure IoT υπηρεσίας SDK] [ lnk-nuget-service-sdk] Nuget πακέτου και τις εξαρτήσεις.

    ![Παράθυρο "Διαχείριση πακέτου Nuget"][11]

4. Προσθέστε τα ακόλουθα `using` δηλώσεις στο επάνω μέρος του αρχείου **Program.cs** :

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. Προσθέστε τα παρακάτω πεδία για το εκπαιδευτικό **πρόγραμμα** . Αντικαταστήστε την τιμή κράτησης θέσης με τη συμβολοσειρά σύνδεσης για την ενότητα IoT που δημιουργήσατε στην προηγούμενη ενότητα.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Προσθέστε την ακόλουθη μέθοδο για την τάξη **προγράμματος** :

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Αυτή η μέθοδος δημιουργεί μια ταυτότητα συσκευής με το Αναγνωριστικό **myFirstDevice**. (Εάν το Αναγνωριστικό συσκευής που υπάρχει ήδη στο μητρώο, ο κώδικας απλώς ανακτά τις υπάρχουσες πληροφορίες συσκευής.) Η εφαρμογή εμφανίζει, στη συνέχεια, το πρωτεύον κλειδί για αυτήν την ταυτότητα. Μπορείτε να χρησιμοποιήσετε αυτό το κλειδί στη προσομοιωμένη συσκευή για να συνδεθείτε με το Κέντρο IoT σας.

7. Τέλος, προσθέστε τις ακόλουθες γραμμές στην **κύρια** μέθοδο:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Εκτέλεση αυτής της εφαρμογής και σημειώστε τον αριθμό-κλειδί συσκευή.

    ![Κλειδί συσκευής που δημιουργείται από την εφαρμογή][12]

> [AZURE.NOTE] Το μητρώο ταυτότητας διανομέα IoT αποθηκεύει μόνο τις ταυτότητες συσκευή για να ενεργοποιήσετε την ασφαλή πρόσβαση στο διανομέα. Αποθηκεύει αναγνωριστικά συσκευών και αριθμούς-κλειδιά για να χρησιμοποιήσετε ως διαπιστευτηρίων ασφαλείας και μια σημαία ενεργοποίηση/απενεργοποίηση που μπορείτε να χρησιμοποιήσετε για να απενεργοποιήσετε την πρόσβαση για μια μεμονωμένη συσκευή. Εάν η εφαρμογή σας πρέπει να αποθηκεύσετε άλλα μετα-δεδομένα ειδικά για τη συσκευή, πρέπει να χρησιμοποιεί μια συγκεκριμένη εφαρμογή store. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές διανομέα IoT][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Λήψη μηνυμάτων συσκευής στο cloud

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Windows που διαβάζει τα μηνύματα συσκευής στο cloud από το Κέντρο IoT. Ένα διανομέα IoT εκθέτει μια [Διανομείς συμβάν Azure][lnk-event-hubs-overview]-συμβατά τελικό σημείο για να μπορέσετε να την ανάγνωση μηνυμάτων συσκευής στο cloud. Για να διατηρήσω τα πράγματα απλά, αυτό το πρόγραμμα εκμάθησης δημιουργεί ένα βασικό πρόγραμμα ανάγνωσης που δεν είναι κατάλληλη για μια ανάπτυξη υψηλής απόδοσης. Για να μάθετε πώς να επεξεργαστούν μηνύματα συσκευής στο cloud με κλίμακα, δείτε τα [μηνύματα συσκευή-cloud διαδικασία] [ lnk-process-d2c-tutorial] το πρόγραμμα εκμάθησης. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να επεξεργαστείτε μηνύματα από το συμβάν διανομείς, δείτε το παράθυρο [Γρήγορα αποτελέσματα με το συμβάν διανομείς] [ lnk-eventhubs-tutorial] το πρόγραμμα εκμάθησης. (Αυτό το πρόγραμμα εκμάθησης είναι ισχύει για τα τελικά σημεία συμβατά διανομέα συμβάν διανομέα IoT.)

> [AZURE.NOTE] Το τελικό σημείο συμβατό διανομέα συμβάντων για την ανάγνωση μηνυμάτων συσκευή-cloud πάντα χρησιμοποιεί το πρωτόκολλο AMQP.

1. Στο Visual Studio, προσθέστε ένα έργο Visual C# κλασική επιφάνεια εργασίας των Windows στην τρέχουσα λύση, χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Βεβαιωθείτε ότι η έκδοση του .NET Framework είναι 4.5.1 ή νεότερη έκδοση. Το όνομα του έργου **ReadDeviceToCloudMessages**.

    ![Νέο έργο Visual C# κλασική επιφάνεια εργασίας των Windows][10]

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **ReadDeviceToCloudMessages** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων Nuget**.

3. Στο παράθυρο **Διαχείριση πακέτου Nuget** , αναζητήστε **WindowsAzure.ServiceBus**, επιλέξτε **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης. Αυτή η διαδικασία στοιχεία λήψης, εγκαθιστά και προσθέτει μια αναφορά σε [Bus υπηρεσιών Azure][lnk-servicebus-nuget], με όλες τις εξαρτήσεις του. Αυτό το πακέτο επιτρέπει την εφαρμογή για να συνδεθείτε με το τελικό σημείο διανομέα συμβατό με το συμβάν στο κέντρο IoT σας.

4. Προσθέστε τα ακόλουθα `using` δηλώσεις στο επάνω μέρος του αρχείου **Program.cs** :

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. Προσθέστε τα παρακάτω πεδία για το εκπαιδευτικό **πρόγραμμα** . Αντικαταστήστε την τιμή κράτησης θέσης με τη συμβολοσειρά σύνδεσης για την ενότητα IoT που δημιουργήσατε στην ενότητα "Δημιουργία ένα διανομέα IoT".

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. Προσθέστε την ακόλουθη μέθοδο για την τάξη **προγράμματος** :

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    Αυτή η μέθοδος χρησιμοποιεί μια παρουσία **EventHubReceiver** να λαμβάνετε μηνύματα από όλα τα IoT διανομέα συσκευή-σε-cloud λαμβάνουν τα διαμερίσματα. Παρατηρήστε πώς περάσετε μια `DateTime.Now` παραμέτρου κατά τη δημιουργία του αντικειμένου **EventHubReceiver** , έτσι ώστε να λαμβάνει μόνο τα μηνύματα που αποστέλλονται μετά την εκκίνηση. Αυτό το φίλτρο είναι χρήσιμη σε περιβάλλον δοκιμής, ώστε να μπορείτε να δείτε το τρέχον σύνολο των μηνυμάτων. Σε ένα περιβάλλον παραγωγής τον κωδικό θα πρέπει να βεβαιωθείτε ότι επεξεργάζεται όλα τα μηνύματα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος για την επεξεργασία μηνυμάτων συσκευή-cloud διανομέα IoT] [ lnk-process-d2c-tutorial] το πρόγραμμα εκμάθησης.

7. Τέλος, προσθέστε τις ακόλουθες γραμμές στην **κύρια** μέθοδο:

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Δημιουργία εφαρμογής προσομοιωμένη συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Windows που προσομοιώνει μια συσκευή που στέλνει μηνύματα συσκευής στο cloud με ένα διανομέα IoT.

1. Στο Visual Studio, προσθέστε ένα έργο Visual C# κλασική επιφάνεια εργασίας των Windows στην τρέχουσα λύση, χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Βεβαιωθείτε ότι η έκδοση του .NET Framework είναι 4.5.1 ή νεότερη έκδοση. Το όνομα του έργου **SimulatedDevice**.

    ![Νέο έργο Visual C# κλασική επιφάνεια εργασίας των Windows][10]

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **SimulatedDevice** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων Nuget**.

3. Στο παράθυρο **Διαχείριση πακέτου Nuget** , επιλέξτε **Αναζήτηση**, αναζητήστε **Microsoft.Azure.Devices.Client**, επιλέξτε **εγκατάσταση** για να εγκαταστήσετε το πακέτο **Microsoft.Azure.Devices.Client** και αποδεχτείτε τους όρους χρήσης. Αυτήν τη διαδικασία στοιχεία λήψης, εγκαθιστά και προσθέτει την αναφορά του [Azure IoT - συσκευή SDK Nuget πακέτου] [ lnk-device-nuget] και τις εξαρτήσεις.

4. Προσθέστε τα ακόλουθα `using` δήλωση στο επάνω μέρος του αρχείου **Program.cs** :

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. Προσθέστε τα παρακάτω πεδία για το εκπαιδευτικό **πρόγραμμα** . Αντικαταστήστε τις τιμές κράτησης θέσης με το hostname διανομέα IoT που ανακτήσατε στην ενότητα "Δημιουργία ένα διανομέα IoT" και το κλειδί συσκευής ανάκτηση στην ενότητα "Δημιουργήστε μια ταυτότητα συσκευής".

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. Προσθέστε την ακόλουθη μέθοδο για την τάξη **προγράμματος** :

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Αυτή η μέθοδος αποστέλλει ένα νέο μήνυμα συσκευής στο cloud ανά δευτερόλεπτο. Το μήνυμα περιέχει ένα αντικείμενο σειριοποιημένο JSON, με το Αναγνωριστικό συσκευής και ένας τυχαίος αριθμός για να προσομοιώσετε αισθητήρα ταχύτητα ανέμου.

7. Τέλος, προσθέστε τις ακόλουθες γραμμές στην **κύρια** μέθοδο:

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  Από προεπιλογή, η μέθοδος **Create** δημιουργεί μια παρουσία **DeviceClient** που χρησιμοποιεί το πρωτόκολλο AMQP για να επικοινωνήσετε με το Κέντρο IoT. Για να χρησιμοποιήσετε το πρωτόκολλο HTTP, χρησιμοποιήστε την παράκαμψη της μεθόδου **Δημιουργία** που σας επιτρέπει να καθορίσετε το πρωτόκολλο. Εάν χρησιμοποιείτε το πρωτόκολλο HTTP, θα πρέπει επίσης να προσθέσετε το πακέτο Nuget **Microsoft.AspNet.WebApi.Client** στο έργο σας για να συμπεριλάβετε το χώρο ονομάτων **System.Net.Http.Formatting** .

Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί στα βήματα για να δημιουργήσετε ένα πρόγραμμα-πελάτη συσκευή IoT διανομέα. Μπορείτε επίσης να χρησιμοποιήσετε την [Υπηρεσία συνδεδεμένοι για διανομέα IoT Azure] [ lnk-connected-service] επέκταση Visual Studio για να προσθέσετε τον απαραίτητο κώδικα στην εφαρμογή υπολογιστή-πελάτη σας συσκευή.


> [AZURE.NOTE] Για να διατηρήσετε πράγματα απλή, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως μια εκθετική διπλασιασμών), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων][lnk-transient-faults].

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1.  Στο Visual Studio, στην Εξερεύνηση λύσεων, κάντε δεξί κλικ τη λύση σας και, στη συνέχεια, κάντε κλικ στην επιλογή **Ορισμός εκκίνησης έργα**. Επιλέξτε **πολλά έργα εκκίνησης**και, στη συνέχεια, επιλέξτε **Έναρξη** με την ενέργεια για το **ReadDeviceToCloudMessages** και **SimulatedDevice** έργα.

    ![Ιδιότητες εκκίνησης έργου][41]

2.  Πατήστε το πλήκτρο **F5** για να ξεκινήσετε και οι δύο εφαρμογές που εκτελούνται. Το αποτέλεσμα κονσόλας από την εφαρμογή **SimulatedDevice** εμφανίζει τα μηνύματα προσομοιωμένη συσκευή σας στέλνει το Κέντρο IoT σας. Το αποτέλεσμα κονσόλας από την εφαρμογή **ReadDeviceToCloudMessages** εμφανίζει τα μηνύματα που λαμβάνει το Κέντρο IoT σας.

    ![Κονσόλα εξόδου από εφαρμογές][42]

3. Το πλακίδιο **Χρήση** στην [πύλη του Azure] [ lnk-portal] εμφανίζει τον αριθμό των μηνυμάτων που αποστέλλονται σε την ενότητα:

    ![Azure πλακίδιο χρήση πύλης][43]


## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, που έχει ρυθμιστεί ένα διανομέα IoT στην πύλη του Azure και, στη συνέχεια, να δημιουργήσει μια ταυτότητα συσκευής στο μητρώο ταυτότητας την ενότητα. Χρησιμοποιείται αυτή η ταυτότητα συσκευή για να ενεργοποιήσετε την εφαρμογή προσομοίωση συσκευή για την αποστολή μηνυμάτων συσκευής στο cloud για την ενότητα. Μπορείτε επίσης να δημιουργήσει μια εφαρμογή που εμφανίζει τα μηνύματα που ελήφθησαν από την ενότητα. 

Για να συνεχίσετε γρήγορα αποτελέσματα με το Κέντρο IoT και να εξερευνήσετε τα άλλα σενάρια IoT, ανατρέξτε στα θέματα:

- [Συνδέει τη συσκευή σας][lnk-connect-device]
- [Γρήγορα αποτελέσματα με τη Διαχείριση συσκευών][lnk-device-management]
- [Γρήγορα αποτελέσματα με το SDK IoT πύλης][lnk-gateway-SDK]

Για να μάθετε πώς μπορείτε να επεκτείνετε τη λύση σας IoT και επεξεργασία των μηνυμάτων συσκευής στο cloud με κλίμακα, δείτε τα [μηνύματα συσκευή-cloud διαδικασία] [ lnk-process-d2c-tutorial] το πρόγραμμα εκμάθησης.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
