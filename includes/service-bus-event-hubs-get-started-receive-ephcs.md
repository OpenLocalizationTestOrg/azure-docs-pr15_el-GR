## <a name="receive-messages-with-eventprocessorhost"></a>Λήψη μηνυμάτων με EventProcessorHost

[EventProcessorHost][] μιας κλάσης .NET που απλοποιεί λήψη συμβάντα από διανομείς συμβάν κατά τη Διαχείριση μόνιμη σημεία ελέγχου και παράλληλα λαμβάνει από αυτές τις διανομείς συμβάν. Χρήση [EventProcessorHost][], μπορείτε να διαιρέσετε συμβάντα σε πολλαπλούς δέκτες, ακόμα και όταν φιλοξενούνται σε διαφορετικούς κόμβους. Αυτό το παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε [EventProcessorHost][] για ένα μεμονωμένο δέκτη. Το δείγμα [Κλιμάκωση ανάληψη συμβάν επεξεργασίας][] δείχνει πώς μπορείτε να χρησιμοποιήσετε [EventProcessorHost][] με πολλαπλούς δέκτες.

Για να χρησιμοποιήσετε [EventProcessorHost][], πρέπει να έχετε ένα [λογαριασμό αποθήκευσης Azure][]:

1. Συνδεθείτε στην [πύλη του Azure][]και κάντε κλικ στην επιλογή " **Δημιουργία** " στο επάνω αριστερό μέρος της οθόνης.

2. Κάντε κλικ στην επιλογή **δεδομένα + χώρος αποθήκευσης**και, στη συνέχεια, κάντε κλικ στο **λογαριασμό του χώρου αποθήκευσης**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)

3. Στο blade τη **Δημιουργία λογαριασμού χώρου αποθήκευσης** , πληκτρολογήστε ένα όνομα για το λογαριασμό χώρου αποθήκευσης. Επιλέξτε μια συνδρομή Azure, ομάδα πόρων και τη θέση στην οποία θέλετε να δημιουργήσετε τον πόρο. Στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)

4. Στη λίστα λογαριασμών χώρου αποθήκευσης, κάντε κλικ στο λογαριασμό που μόλις δημιουργήσατε χώρου αποθήκευσης.

5. Στο blade το λογαριασμό χώρου αποθήκευσης, κάντε κλικ στην επιλογή **πλήκτρα πρόσβασης**. Αντιγράψτε την τιμή του **κλειδί1** για να χρησιμοποιήσετε αργότερα σε αυτό το πρόγραμμα εκμάθησης.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)

4. Στο Visual Studio, δημιουργήστε ένα νέο έργο Visual C# επιφάνειας εργασίας εφαρμογών χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Το όνομα του έργου **δέκτη**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)

5. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ τη λύση και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet για τη λύση**.

6. Κάντε κλικ στην καρτέλα **Αναζήτηση** και, στη συνέχεια, αναζητήστε το `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Βεβαιωθείτε ότι το όνομα του έργου (**ακουστικό**) έχει καθοριστεί στο πλαίσιο **εκδόσεις** . Κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)

    Visual Studio στοιχεία λήψης, εγκαθιστά και προσθέτει μια αναφορά σε [Διανομέα Bus συμβάν Azure Service - EventProcessorHost NuGet πακέτου](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), με όλες τις εξαρτήσεις του.

7. Κάντε δεξί κλικ στο έργο **ακουστικό** , κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **τάξης**. Ονομάστε τη νέα κλάση **SimpleEventProcessor**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη** για να δημιουργήσετε την κλάση.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)

8. Προσθέστε τις παρακάτω προτάσεις στο επάνω μέρος του αρχείου SimpleEventProcessor.cs:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Στη συνέχεια, αντικαταστήστε τον παρακάτω κώδικα για το σώμα της κλάσης:

    ```
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());

                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }
    ```

    Αυτή η κλάση θα ονομάζεται από το **EventProcessorHost** σε διαδικασία συμβάντα που λάβατε από την ενότητα συμβάντων. Λάβετε υπόψη ότι η `SimpleEventProcessor` κλάση χρησιμοποιεί χρονόμετρου για να καλέσετε περιοδικά τη μέθοδο σημείο ελέγχου σε περιβάλλον **EventProcessorHost** . Αυτό εξασφαλίζει ότι, εάν ο παραλήπτης είναι η επανεκκίνηση, τότε θα χαθεί δεν περισσότερο από πέντε λεπτά επεξεργασίας εργασίας.

9. Στο **πρόγραμμα** τάξη, προσθέστε τα ακόλουθα `using` δήλωση στο επάνω μέρος του αρχείου:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Στη συνέχεια, αντικαταστήστε το `Main` μέθοδο σε το `Program` κλάσης με τον ακόλουθο κώδικα, αντικαθιστώντας το όνομα του συμβάντος διανομέας και τη συμβολοσειρά σύνδεσης επιπέδου χώρο ονομάτων που έχετε αποθηκεύσει προηγουμένως, και το λογαριασμό χώρου αποθήκευσης και το κλειδί που αντιγράψατε στις προηγούμενες ενότητες. 

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{Event Hub connection string}";
      string eventHubName = "{Event Hub name}";
      string storageAccountName = "{storage account name}";
      string storageAccountKey = "{storage account key}";
      string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
      Console.WriteLine("Registering EventProcessor...");
      var options = new EventProcessorOptions();
      options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
      eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί μια μεμονωμένη περίοδο λειτουργίας του [EventProcessorHost][]. Για να αυξήσετε την ταχύτητα, συνιστάται να εκτελείτε πολλές παρουσίες των [EventProcessorHost][], όπως φαίνεται στο δείγμα [Κλιμάκωση ανάληψη Επεξεργασία συμβάντος][] . Σε αυτές τις περιπτώσεις, τις διάφορες παρουσίες αυτόματα συντονισμό μεταξύ τους για να φορτώσετε το υπόλοιπο τα συμβάντα που λάβατε. Εάν θέλετε πολλαπλούς δέκτες κάθε διαδικασία *όλα* τα συμβάντα, πρέπει να χρησιμοποιήσετε την έννοια **ConsumerGroup** . Κατά τη λήψη συμβάντα από διαφορετικούς υπολογιστές, ίσως είναι χρήσιμο για να ορίσετε ονόματα για παρουσίες [EventProcessorHost][] με βάση το μηχανές (ή τους ρόλους) στο οποίο έχουν αναπτυχθεί. Για περισσότερες πληροφορίες σχετικά με αυτά τα θέματα, ανατρέξτε στα θέματα [Επισκόπηση διανομείς συμβάντων][] και [Οδηγού προγραμματισμού συμβάντων διανομείς][] .

<!-- Links -->
[Επισκόπηση διανομείς συμβάντων]: ../articles/event-hubs/event-hubs-overview.md
[Οδηγός προγραμματισμού διανομείς συμβάν]: ../articles/event-hubs/event-hubs-programming-guide.md
[Κλιμάκωση ανάληψη Επεξεργασία συμβάντος]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Λογαριασμός Azure χώρου αποθήκευσης]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Πύλη του Azure]: https://portal.azure.com