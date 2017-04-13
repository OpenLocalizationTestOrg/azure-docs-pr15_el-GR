## <a name="send-messages-to-event-hubs"></a>Αποστολή μηνυμάτων με διανομείς συμβάντος

Σε αυτήν την ενότητα, θα μπορείτε να συντάξετε μια εφαρμογή κονσόλας Windows που στέλνει συμβάντα σε κέντρο σας συμβάν.

1. Στο Visual Studio, δημιουργήστε ένα νέο έργο Visual C# επιφάνειας εργασίας εφαρμογών χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Το όνομα του **αποστολέα**έργου.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ τη λύση και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet για τη λύση**. 

3. Κάντε κλικ στην καρτέλα **Αναζήτηση** και, στη συνέχεια, αναζητήστε το `Microsoft Azure Service Bus`. Βεβαιωθείτε ότι το όνομα του έργου (**αποστολέα**) έχει καθοριστεί στο πλαίσιο **εκδόσεις** . Κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio στοιχεία λήψης, εγκαθιστά και προσθέτει μια αναφορά στο [πακέτο NuGet Azure Service Bus βιβλιοθήκης](https://www.nuget.org/packages/WindowsAzure.ServiceBus).

4. Προσθέστε τα ακόλουθα `using` δηλώσεις στο επάνω μέρος του αρχείου **Program.cs** :

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Προσθέστε τα ακόλουθα πεδία στην κλάση **προγράμματος** , αντικαθιστώντας τις τιμές κράτησης θέσης με το όνομα στο συμβάν διανομέα που δημιουργήσατε στην προηγούμενη ενότητα και τη συμβολοσειρά σύνδεσης επιπέδου χώρο ονομάτων που έχετε αποθηκεύσει προηγουμένως.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. Προσθέστε την ακόλουθη μέθοδο για να την κλάση **προγράμματος** :

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    Αυτή η μέθοδος στέλνει συνεχώς συμβάντα με το συμβάν διανομέα με μια καθυστέρηση 200-ms.

7. Τέλος, προσθέστε τις ακόλουθες γραμμές στη μέθοδο **κύριες** :

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
