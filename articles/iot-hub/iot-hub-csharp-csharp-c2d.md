<properties
    pageTitle="Αποστολή μηνυμάτων cloud-σε-συσκευή με το Κέντρο IoT | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης για να μάθετε πώς μπορείτε να στείλετε μηνύματα cloud-σε-συσκευή χρησιμοποιώντας το Azure IoT διανομέα με C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>Πρόγραμμα εκμάθησης: Πώς μπορείτε να στείλετε μηνύματα cloud-σε-συσκευή με το Κέντρο IoT και .net

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Εισαγωγή

Azure IoT διανομέα είναι μια πλήρως διαχειριζόμενη υπηρεσία που σας βοηθά να ενεργοποιήσετε αξιόπιστη και ασφαλή επικοινωνίες διπλής κατεύθυνσης μεταξύ εκατομμύρια IoT συσκευές και το τέλος ξανά μια εφαρμογή. Το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με το Κέντρο IoT] δείχνει πώς μπορείτε να δημιουργήσετε ένα διανομέα IoT, προμήθεια μιας συσκευής ταυτότητας σε αυτό και κώδικα μια πρόχειρη συσκευή που στέλνει μηνύματα συσκευής στο cloud.

Αυτό το πρόγραμμα εκμάθησης δημιουργεί στην [Γρήγορα αποτελέσματα με το Κέντρο IoT]. Σας δείχνει πώς να:

- Από την εφαρμογή cloud παρασκηνίου, αποστολή μηνυμάτων cloud-σε-συσκευή σε μία μόνο συσκευή μέσω IoT διανομέα.
- Λαμβάνετε μηνύματα cloud-σε-συσκευή σε μια συσκευή.
- Από το cloud εφαρμογή πίσω τέλος, ζητήσετε αποδεικτικό παράδοσης (*σχόλια*) για τα μηνύματα που αποστέλλονται σε μια συσκευή από διανομέα IoT.

Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με τα μηνύματα cloud-σε-συσκευή του [Οδηγού για προγραμματιστές διανομέα IoT][IoT Hub Developer Guide - C2D].

Στο τέλος αυτού του προγράμματος εκμάθησης, θα εκτελέσετε δύο εφαρμογές κονσόλας Windows:

* **SimulatedDevice**, μια τροποποιημένη έκδοση της εφαρμογής που έχουν δημιουργηθεί με [Γρήγορα αποτελέσματα με το Κέντρο IoT], το οποίο συνδέεται με το Κέντρο IoT σας και να λαμβάνει μηνύματα cloud-σε-συσκευή.
* **SendCloudToDevice**, που στέλνει ένα μήνυμα cloud-σε-συσκευή προσομοιωμένη συσκευή μέσω διανομέα IoT και, στη συνέχεια, λαμβάνει το αποδεικτικό παράδοσης.

> [AZURE.NOTE] Ο διανομέας IoT διαθέτει SDK υποστήριξης για πολλές γλώσσες (συμπεριλαμβανομένων των C, Java και Javascript) μέσω SDK συσκευή Azure IoT και πλατφόρμες συσκευών. Για οδηγίες βήμα προς βήμα σχετικά με τον τρόπο για να συνδέσετε τη συσκευή για αυτό το πρόγραμμα εκμάθησης του κώδικα και γενικά για Azure IoT διανομέα, ανατρέξτε στο [Κέντρο για προγραμματιστές του Azure IoT].

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, θα χρειαστείτε τα εξής:

+ Microsoft Visual Studio 2015

+ Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

## <a name="receive-messages-on-the-simulated-device"></a>Λήψη μηνυμάτων από τη συσκευή προσομοιωμένη

Σε αυτήν την ενότητα, θα μπορείτε να τροποποιήσετε την εφαρμογή προσομοιωμένη συσκευή που δημιουργήσατε [Γρήγορα αποτελέσματα με το Κέντρο IoT] να λαμβάνετε μηνύματα cloud-σε-συσκευή από την ενότητα IoT.

1. Στο Visual Studio, μέσα στο έργο **SimulatedDevice** , προσθέστε την ακόλουθη μέθοδο για την τάξη **πρόγραμμα** .

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    Το `ReceiveAsync` μέθοδος επιστρέφει ασύγχρονα το μήνυμα που λάβατε κατά τη στιγμή που λαμβάνεται από τη συσκευή. Επιστρέφει *τιμή null* μετά από ένα χρονικό όριο specifiable (σε αυτήν την περίπτωση, χρησιμοποιείται το προεπιλεγμένο από ένα λεπτό). Όταν συμβαίνει αυτό, ο κώδικας θα πρέπει να συνεχίσετε αναμονή για νέα μηνύματα. Αυτό είναι ο λόγος για την `if (receivedMessage == null) continue` γραμμής.

    Η κλήση `CompleteAsync()` ειδοποιεί διανομέα IoT ότι το μήνυμα έχει υποστεί επεξεργασία με επιτυχία. Το μήνυμα μπορούν να καταργηθούν ασφαλής από την ουρά συσκευής. Εάν κάτι συνέβη που εμπόδισε την ολοκλήρωση της επεξεργασίας του μηνύματος στην εφαρμογή συσκευή, διανομέα IoT παραδίδει ξανά. Είναι σημαντικό ότι η λογική επεξεργασίας μηνύματος στην εφαρμογή συσκευή είναι *idempotent*, στη συνέχεια, ώστε να λαμβάνει το ίδιο μήνυμα πολλές φορές παράγει το ίδιο αποτέλεσμα. Μια εφαρμογή μπορεί να εγκαταλείψετε επίσης προσωρινά ένα μήνυμα το οποίο έχει ως αποτέλεσμα διανομέα IoT διατηρώντας το μήνυμα στην ουρά για μελλοντική κατανάλωση. Εναλλακτικά, η εφαρμογή μπορεί να απορρίψετε ένα μήνυμα, το οποίο καταργεί οριστικά το μήνυμα από την ουρά. Για περισσότερες πληροφορίες σχετικά με τον κύκλο ζωής μήνυμα cloud-σε-συσκευή, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές διανομέα IoT][IoT Hub Developer Guide - C2D].

    > [AZURE.NOTE] Κατά τη χρήση HTTP αντί για MQTT ή AMQP ως μεταφορά, τα `ReceiveAsync` μέθοδος επιστρέφει αμέσως. Τα υποστηριζόμενα μοτίβο για μηνύματα cloud-σε-συσκευή με HTTP είναι περιστασιακά συνδεδεμένες συσκευές που ελέγχουν για μηνύματα σπάνια (λιγότερο από κάθε 25 λεπτά). Έκδοση περισσότερες HTTP λαμβάνει αποτελεσμάτων στην ενότητα IoT περιορισμού τις αιτήσεις. Για περισσότερες πληροφορίες σχετικά με τις διαφορές μεταξύ MQTT, AMQP και HTTP υποστήριξης και περιορισμού IoT διανομέα, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές διανομέα IoT][IoT Hub Developer Guide - C2D].

2. Προσθέστε την ακόλουθη μέθοδο στη μέθοδο **κύριες** , αμέσως πριν την `Console.ReadLine()` γραμμής:

        ReceiveC2dAsync();

> [AZURE.NOTE] Για χάρη της απλότητα, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως εκθετική διπλασιασμών), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων].

## <a name="send-a-cloud-to-device-message"></a>Αποστολή μηνύματος cloud-σε-συσκευή

Σε αυτήν την ενότητα, θα μπορείτε να συντάξετε μια εφαρμογή κονσόλας Windows που στέλνει μηνύματα cloud-σε-συσκευή στην εφαρμογή προσομοιωμένη συσκευή.

1. Στην τρέχουσα λύση Visual Studio, δημιουργήστε ένα νέο έργο Visual C# επιφάνειας εργασίας εφαρμογής χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Το όνομα του έργου **SendCloudToDevice**.

    ![Νέο έργο στο Visual Studio][20]

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ τη λύση και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet για... λύση**. 

    Έτσι ανοίγει το παράθυρο " **Διαχείριση πακέτων NuGet** ".

3. Αναζήτηση για `Microsoft Azure Devices`, κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης. 

    Αυτό στοιχεία λήψης, εγκαθιστά και προσθέτει την αναφορά του [Azure IoT - πακέτου NuGet SDK υπηρεσίας].

4. Προσθέστε τα ακόλουθα `using` δήλωση στο επάνω μέρος του αρχείου **Program.cs** :

        using Microsoft.Azure.Devices;

5. Προσθέστε τα παρακάτω πεδία για το εκπαιδευτικό **πρόγραμμα** . Αντικαταστήστε την τιμή κράτησης θέσης με τη συμβολοσειρά σύνδεσης διανομέα IoT από [Γρήγορα αποτελέσματα με το Κέντρο IoT]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. Προσθέστε την ακόλουθη μέθοδο για την τάξη **προγράμματος** :

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    Αυτή η μέθοδος αποστέλλει ένα νέο μήνυμα cloud-σε-συσκευή στη συσκευή με το Αναγνωριστικό, `myFirstDevice`. Αλλάξτε αυτήν την παράμετρο αντίστοιχα, σε περίπτωση που τροποποιήσατε από αυτόν που χρησιμοποιήθηκε [γρήγορα]αποτελέσματα με το διανομέα IoT.

7. Τέλος, προσθέστε τις ακόλουθες γραμμές στην **κύρια** μέθοδο:

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. Από μέσα σε Visual Studio, κάντε δεξί κλικ τη λύση σας και επιλέξτε **Ορισμός εκκίνησης έργα...**. Επιλέξτε **πολλά έργα εκκίνησης**και, στη συνέχεια, επιλέξτε την ενέργεια **Εκκίνηση** για **ProcessDeviceToCloudMessages**, **SimulatedDevice**και **SendCloudToDevice**.

9.  Πατήστε το πλήκτρο **F5**. Οι τρεις εφαρμογές πρέπει να ξεκινά. Επιλέξτε τα παράθυρα **SendCloudToDevice** και πατήστε το πλήκτρο **Enter**. Θα πρέπει να βλέπετε το μήνυμα που λάβατε από την εφαρμογή προσομοιωμένη.

    ![Εφαρμογή λήψη μηνύματος][21]

## <a name="receive-delivery-feedback"></a>Λαμβάνουν σχόλια παράδοσης
Είναι δυνατό να αίτηση παράδοσης (ή λήξης) επιβεβαίωση από διανομέα IoT για κάθε μήνυμα cloud-σε-συσκευή. Αυτή η δυνατότητα επιτρέπει παρασκηνίου cloud για να ενημερώσετε εύκολα "Επανάληψη" ή αποζημίωση λογικής. Για περισσότερες πληροφορίες σχετικά με το cloud-σε-συσκευή σχολίων, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές διανομέα IoT][IoT Hub Developer Guide - C2D].

Σε αυτήν την ενότητα, θα μπορείτε να τροποποιήσετε την εφαρμογή **SendCloudToDevice** για να ζητήσετε σχόλια και λαμβάνετε το από διανομέα IoT.

1. Στο Visual Studio, μέσα στο έργο **SendCloudToDevice** , προσθέστε την ακόλουθη μέθοδο για την τάξη **πρόγραμμα** .
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    Σημειώστε ότι το μοτίβο παραλαβής είναι το ίδιο με αυτό που χρησιμοποιείται για τη λήψη μηνυμάτων cloud-σε-συσκευή από την εφαρμογή συσκευή.

2. Προσθέστε την ακόλουθη μέθοδο στη μέθοδο **κύριες** , αμέσως μετά το `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` γραμμής:

        ReceiveFeedbackAsync();

3. Για να ζητήσετε σχόλια για την παράδοση του μηνύματός σας cloud-σε-συσκευή, πρέπει να καθορίσετε μια ιδιότητα στη μέθοδο **SendCloudToDeviceMessageAsync** . Προσθέστε την ακόλουθη γραμμή, αμέσως μετά το `var commandMessage = new Message(...);` γραμμής:

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  Εκτελέστε τις εφαρμογές, πατώντας το πλήκτρο **F5**. Θα πρέπει να βλέπετε όλες τις τρεις εφαρμογές Έναρξη. Επιλέξτε τα παράθυρα **SendCloudToDevice** και πατήστε το πλήκτρο **Enter**. Θα πρέπει να βλέπετε το μήνυμα που λάβατε από την εφαρμογή προσομοιωμένη και μετά από μερικά δευτερόλεπτα, το μήνυμα σχολίων που λαμβάνονται από την εφαρμογή σας **SendCloudToDevice** .

    ![Εφαρμογή λήψη μηνύματος][22]

> [AZURE.NOTE] Για χάρη της απλότητα, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως εκθετική διπλασιασμών), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων].

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς μπορείτε να στέλνετε και να λαμβάνετε μηνύματα cloud-σε-συσκευή. 

Για να δείτε παραδείγματα ολοκλήρωσης για ολοκληρωμένες λύσεις που χρησιμοποιούν IoT διανομέα, ανατρέξτε στο θέμα [Οικογένεια IoT Azure].

Για να μάθετε περισσότερα σχετικά με την ανάπτυξη λύσεων με IoT διανομέα, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές IoT διανομέα].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT - πακέτου NuGet SDK υπηρεσίας]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Χειρισμός μεταβατικές σφαλμάτων]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[Οδηγός για προγραμματιστές IoT διανομέα]: iot-hub-devguide.md
[Γρήγορα αποτελέσματα με το Κέντρο IoT]: iot-hub-csharp-csharp-getstarted.md
[Κέντρο για προγραμματιστές IoT Azure]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT οικογένεια προγραμμάτων]: https://azure.microsoft.com/documentation/suites/iot-suite/