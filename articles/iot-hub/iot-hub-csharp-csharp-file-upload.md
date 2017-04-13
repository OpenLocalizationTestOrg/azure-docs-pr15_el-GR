<properties
    pageTitle="Αποστολή αρχείων από συσκευές χρησιμοποιώντας το Κέντρο IoT | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης για να μάθετε πώς μπορείτε να αποστείλετε αρχεία από συσκευές με διανομέα IoT Azure C#."
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
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Πρόγραμμα εκμάθησης: Πώς μπορείτε να αποστείλετε αρχεία από συσκευές στο cloud με το διανομέα IoT

## <a name="introduction"></a>Εισαγωγή

Azure IoT διανομέα είναι μια πλήρως διαχειριζόμενη υπηρεσία που επιτρέπει την αξιόπιστη και ασφαλής διπλής κατεύθυνσης επικοινωνιών μεταξύ εκατομμύρια IoT συσκευές και μια εφαρμογή, δημιουργήστε αντίγραφα τέλος. Προηγούμενη εκμάθηση ([Γρήγορα αποτελέσματα με το Κέντρο IoT] και [Αποστολή μηνυμάτων Cloud-σε-συσκευή με το Κέντρο IoT]) απεικόνιση βασικές συσκευή-σε-cloud και cloud-σε-συσκευή λειτουργικότητα ανταλλαγής μηνυμάτων IoT διανομέας και το πρόγραμμα εκμάθησης [μηνύματα διαδικασία συσκευή-σε-Cloud] περιγράφει τον τρόπο για την αξιόπιστη αποθήκευση μηνυμάτων συσκευής στο cloud στο χώρο αποθήκευσης αντικειμένων blob του Azure. Ωστόσο, σε ορισμένα σενάρια δεν μπορείτε εύκολα να αντιστοιχίσετε τα δεδομένα των συσκευών Αποστολή σε τα σχετικά μικρό μηνύματα συσκευής στο cloud που δέχεται IoT διανομέα. Παραδείγματα μεγάλα αρχεία που περιέχουν εικόνες, βίντεο, δόνησης δεδομένων σε υψηλή συχνότητα ή που περιέχουν ορισμένων φόρμας προ-επεξεργασία δεδομένων. Αυτά τα αρχεία είναι συνήθως μαζική επεξεργασία στο cloud με εργαλεία όπως το [Azure εργοστασίου δεδομένων] ή στη στοίβα [Hadoop] . Όταν προτιμούν να στέλνει συμβάντα αποστολών αρχείων από μια συσκευή, μπορείτε να χρησιμοποιήσετε λειτουργίες ασφάλεια και αξιοπιστία IoT διανομέα.

Αυτό το πρόγραμμα εκμάθησης δημιουργεί τον κωδικό στην [Αποστολή μηνυμάτων Cloud-σε-συσκευή με το Κέντρο IoT] εκμάθηση για να σας δείξουν πώς μπορείτε να χρησιμοποιήσετε τις δυνατότητες αποστολής αρχείου του IoT διανομέα. Σας δείχνει πώς μπορείτε να δώσετε μια συσκευή με ασφάλεια ένα Azure αντικειμένων blob URI για την αποστολή ενός αρχείου και πώς μπορείτε να χρησιμοποιήσετε τις ειδοποιήσεις διανομέα IoT αρχείου αποστολής για να ενεργοποιήσετε την επεξεργασία του αρχείου σε σας εφαρμογή παρασκηνίου.

Στο τέλος αυτού του προγράμματος εκμάθησης εκτέλεση δύο εφαρμογών κονσόλας Windows:

* **SimulatedDevice**, μια τροποποιημένη έκδοση της εφαρμογής που έχουν δημιουργηθεί με την [Αποστολή μηνυμάτων Cloud-σε-συσκευή με το Κέντρο IoT] το πρόγραμμα εκμάθησης, οποία αποστέλλει ένα αρχείο με το χώρο αποθήκευσης χρησιμοποιώντας ένα URI συσχετισμών Ασφαλείας που παρέχονται από το Κέντρο IoT σας.
* **ReadFileUploadNotification**, που λαμβάνει τις ειδοποιήσεις Αποστολή αρχείου από το Κέντρο IoT σας.

> [AZURE.NOTE] Διανομέα IoT υποστηρίζει πολλές πλατφόρμες συσκευών και γλώσσες (συμπεριλαμβανομένων των C, Java και Javascript) μέσω της συσκευής Azure IoT SDK. Ανατρέξτε στο [Κέντρο για προγραμματιστές του Azure IoT] για οδηγίες βήμα προς βήμα σχετικά με τον τρόπο για να συνδέσετε τη συσκευή για να τον κώδικα που εμφανίζεται σε αυτό το πρόγραμμα εκμάθησης και γενικά Azure IoT διανομέα.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

+ Microsoft Visual Studio 2015,

+ Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Συσχετισμός λογαριασμού αποθήκευσης Azure IoT διανομέα

Επειδή προσομοιωμένη συσκευή αποστέλλει ένα αρχείο σε ένα αντικείμενο blob, πρέπει να έχετε ένα λογαριασμό [Αποθήκευσης Azure] που σχετίζεται με IoT διανομέα. Όταν μπορείτε να συσχετίσετε ένα λογαριασμό αποθήκευσης Azure με ένα διανομέα IoT, την ενότητα μπορεί να δημιουργήσει ένα URI συσχετισμών Ασφαλείας που να χρησιμοποιήσετε μια συσκευή για την ασφαλή αποστείλετε ένα αρχείο σε ένα κοντέινερ αντικειμένων blob. Η υπηρεσία IoT διανομέας και τη συσκευή SDK συντονισμό τη διαδικασία που δημιουργεί το URI συσχετισμών Ασφαλείας και κάνει διαθέσιμο σε μια συσκευή για να χρησιμοποιήσετε για την αποστολή ενός αρχείου.

Ακολουθήστε τις οδηγίες στο θέμα [Ρύθμιση παραμέτρων αρχείου αποστολές με την πύλη Azure] [ lnk-configure-upload] για να συσχετίσετε ένα λογαριασμό αποθήκευσης Azure για το Κέντρο IoT σας.

## <a name="upload-a-file-from-a-simulated-device"></a>Αποστολή αρχείου από μια πρόχειρη συσκευή

Σε αυτήν την ενότητα, μπορείτε να τροποποιήσετε την εφαρμογή προσομοιωμένη συσκευή που δημιουργήσατε στην ενότητα [Αποστολή μηνυμάτων Cloud-σε-συσκευή με το Κέντρο IoT] να λαμβάνετε μηνύματα cloud-σε-συσκευή από την ενότητα IoT.

1. Στο Visual Studio, κάντε δεξί κλικ στο έργο **SimulatedDevice** , κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **Υπάρχον στοιχείο**. Μεταβείτε σε ένα αρχείο εικόνας και να συμπεριλάβετε στο έργο σας. Αυτό το πρόγραμμα εκμάθησης, προϋποθέτει ότι η εικόνα έχει το όνομα `image.jpg`.

2. Κάντε δεξί κλικ στην εικόνα και, στη συνέχεια, κάντε κλικ στην επιλογή **Ιδιότητες**. Βεβαιωθείτε ότι **η αντιγραφή στον κατάλογο εξόδου** έχει οριστεί σε **πάντα αντίγραφο**.

    ![][1]

3. Στο αρχείο **Program.cs** , προσθέστε τις παρακάτω προτάσεις στο επάνω μέρος του αρχείου:

        using System.IO;

4. Προσθέστε την ακόλουθη μέθοδο για την τάξη **προγράμματος** :
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    Το `UploadToBlobAsync` μέθοδος λαμβάνει το αρχείο προέλευσης όνομα και η ροή του αρχείου να αποσταλεί και χειρισμού η αποστολή με το χώρο αποθήκευσης. Η εφαρμογή κονσόλας εμφανίζει το χρόνο που χρειάζεται για την αποστολή του αρχείου.

5. Προσθέστε την ακόλουθη μέθοδο στη μέθοδο **κύριες** , αμέσως πριν την `Console.ReadLine()` γραμμής:

        SendToBlobAsync();

> [AZURE.NOTE] Για χάρη της απλότητα, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως εκθετική διπλασιασμών), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων].

## <a name="receive-a-file-upload-notification"></a>Λαμβάνετε μια ειδοποίηση αποστολής αρχείου

Σε αυτήν την ενότητα, μπορείτε να συντάξετε μια εφαρμογή κονσόλας Windows που λαμβάνει μηνύματα ειδοποιήσεων αποστολής αρχείου από το Κέντρο IoT.

1. Στην τρέχουσα λύση Visual Studio, δημιουργήστε ένα νέο έργο Visual C# Windows χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Το όνομα του έργου **ReadFileUploadNotification**.

    ![Νέο έργο στο Visual Studio][2]

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **ReadFileUploadNotification** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.

    Εμφανίζει το παράθυρο "Διαχείριση πακέτων NuGet".

2. Αναζήτηση για `Microsoft.Azure.Devices`, κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης. 

    Αυτό στοιχεία λήψης, εγκαθιστά και προσθέτει την αναφορά του [Azure IoT - πακέτου NuGet SDK υπηρεσίας] του **ReadFileUploadNotification** έργου.

3. Στο αρχείο **Program.cs** , προσθέστε τις παρακάτω προτάσεις στο επάνω μέρος του αρχείου:

        using Microsoft.Azure.Devices;

4. Προσθέστε τα παρακάτω πεδία για το εκπαιδευτικό **πρόγραμμα** . Αντικαταστήστε την τιμή κράτησης θέσης με τη συμβολοσειρά σύνδεσης διανομέα IoT από [Γρήγορα αποτελέσματα με το Κέντρο IoT]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. Προσθέστε την ακόλουθη μέθοδο για την τάξη **προγράμματος** :
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Σημειώστε ότι το μοτίβο παραλαβής είναι το ίδιο με αυτό που χρησιμοποιείται για τη λήψη μηνυμάτων cloud-σε-συσκευή από την εφαρμογή συσκευή.

6. Τέλος, προσθέστε τις ακόλουθες γραμμές στην **κύρια** μέθοδο:

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1. Στο Visual Studio, κάντε δεξί κλικ τη λύση σας και επιλέξτε **Ορισμός εκκίνησης έργα**. Επιλέξτε **πολλά έργα εκκίνησης**και, στη συνέχεια, επιλέξτε την ενέργεια **Εκκίνηση** για **ReadFileUploadNotification** και **SimulatedDevice**.

2. Πατήστε το πλήκτρο **F5**. Πρέπει να ξεκινά δύο εφαρμογές. Θα πρέπει να βλέπετε η αποστολή ολοκληρώθηκε στην εφαρμογή μία κονσόλα και το μήνυμα ειδοποίησης αποστολής που λαμβάνονται από τα άλλα κονσόλας εφαρμογής. Μπορείτε να χρησιμοποιήσετε το [Azure πύλη] ή Εξερεύνηση Server Visual Studio για να ελέγξετε για την ύπαρξη του αρχείου έχουν αποσταλεί στο λογαριασμό σας στο χώρο αποθήκευσης Azure.

  ![][50]


## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς μπορείτε να αξιοποιήσετε τις δυνατότητες αποστολής αρχείου του διανομέα IoT για να απλοποιήσετε αποστολών αρχείων από συσκευές. Μπορείτε να συνεχίσετε να εξερευνήσετε δυνατότητες διανομέα IoT και σενάρια με τα ακόλουθα άρθρα:

- [Δημιουργήστε ένα διανομέα IoT μέσω προγραμματισμού][lnk-create-hub]
- [Εισαγωγή στις C SDK][lnk-c-sdk]
- [Ο διανομέας IoT SDK][lnk-sdks]

Για να εξερευνήσετε περαιτέρω τις δυνατότητες του IoT διανομέα, ανατρέξτε στα θέματα:

- [Προσομοίωση μια συσκευή με το SDK IoT πύλης][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Πύλη του Azure]: https://portal.azure.com/

[Εργοστασιακές Azure δεδομένων]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Αποστολή μηνυμάτων Cloud-σε-συσκευή με το Κέντρο IoT]: iot-hub-csharp-csharp-c2d.md
[Διεργασία μηνύματα συσκευής στο Cloud]: iot-hub-csharp-csharp-process-d2c.md
[Γρήγορα αποτελέσματα με το Κέντρο IoT]: iot-hub-csharp-csharp-getstarted.md
[Κέντρο για προγραμματιστές IoT Azure]: http://www.azure.com/develop/iot

[Χειρισμός μεταβατικές σφαλμάτων]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure χώρου αποθήκευσης]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT - πακέτου NuGet SDK υπηρεσίας]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


