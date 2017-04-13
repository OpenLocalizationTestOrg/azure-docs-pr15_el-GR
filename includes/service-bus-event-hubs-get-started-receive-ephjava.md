## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Λήψη μηνυμάτων με EventProcessorHost σε Java

EventProcessorHost μιας κλάσης Java που απλοποιεί λήψη συμβάντα από διανομείς συμβάν κατά τη Διαχείριση μόνιμη σημεία ελέγχου και παράλληλα λαμβάνει από αυτές τις διανομείς συμβάν. Χρήση EventProcessorHost μπορείτε να διαιρέσετε συμβάντα σε πολλαπλούς δέκτες, ακόμα και όταν σε διαφορετικούς κόμβους. Αυτό το παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε EventProcessorHost για ένα μεμονωμένο δέκτη.

###<a name="create-a-storage-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης

Για να χρησιμοποιήσετε EventProcessorHost, πρέπει να έχετε ένα [λογαριασμό αποθήκευσης Azure][]:

1. Συνδεθείτε στην [πύλη του Azure κλασική][]και κάντε κλικ στο κουμπί **ΔΗΜΙΟΥΡΓΊΑ** στο κάτω μέρος της οθόνης.

2. Κάντε κλικ στην επιλογή **Υπηρεσίες δεδομένων**, στη συνέχεια, **χώρος αποθήκευσης**, στη συνέχεια, **Γρήγορης δημιουργίας**και, στη συνέχεια, πληκτρολογήστε ένα όνομα για το λογαριασμό χώρου αποθήκευσης. Επιλέξτε την επιθυμητή περιοχής και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία λογαριασμού χώρου αποθήκευσης**.

    ![][11]

3. Κάντε κλικ στο λογαριασμό που μόλις δημιουργήθηκε χώρου αποθήκευσης και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πλήκτρα πρόσβασης**:

    ![][12]

    Αντιγράψτε το πλήκτρο πρωτεύοντα πρόσβασης για να χρησιμοποιήσετε αργότερα σε αυτό το πρόγραμμα εκμάθησης.

###<a name="create-a-java-project-using-the-eventprocessor-host"></a>Δημιουργία έργου Java χρησιμοποιώντας τον κεντρικό υπολογιστή του EventProcessor

Η βιβλιοθήκη προγράμματος-πελάτη Java για διανομείς συμβάν δεν είναι διαθέσιμη για χρήση σε έργα Maven από το [Κεντρικό αποθετήριο Maven][Maven Package], και μπορούν να χρησιμοποιηθούν με την ακόλουθη δήλωση εξάρτηση αρχείο έργου σας Maven:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Για διαφορετικούς τύπους Δόμηση περιβάλλοντα, μπορείτε να ρητά να αποκτήσετε το πιο πρόσφατο τελική ΒΆΖΟ τα αρχεία από το [Κεντρικό αποθετήριο Maven] [ Maven Package] ή από [το σημείο διανομής κυκλοφορίας στην GitHub](https://github.com/Azure/azure-event-hubs/releases).  

1. Για το παρακάτω παράδειγμα, πρώτα να δημιουργήσετε ένα νέο έργο Maven για μια εφαρμογή κονσόλας/κελύφους στο περιβάλλον ανάπτυξής σας Αγαπημένα Java. Τάξη θα καλούνται ```ErrorNotificationHandler```.     

    ``` Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```

2. Χρησιμοποιήστε τον ακόλουθο κώδικα για να δημιουργήσετε μια νέα κλάση που ονομάζεται ```EventProcessor```.

    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;

    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
                
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```

3. Δημιουργήσετε ένα ολοκληρωμένο κλάσης που ονομάζεται ```EventProcessorSample```, χρησιμοποιώντας τον παρακάτω κώδικα.

    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;

    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";

            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
            
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
            
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
            
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }

            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
                
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
            
            System.out.println("End of sample");
        }
    }
    ```

4. Αντικαταστήστε τα παρακάτω πεδία με τις τιμές που χρησιμοποιούνται όταν δημιουργήσατε το συμβάν διανομέα και του λογαριασμού χώρου αποθήκευσης.

    ``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";

    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";

    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί μια μεμονωμένη περίοδο λειτουργίας του EventProcessorHost. Για να αυξήσετε την ταχύτητα, συνιστάται να εκτελείτε πολλές παρουσίες των EventProcessorHost. Σε αυτές τις περιπτώσεις, τις διάφορες παρουσίες αυτόματα συντονισμό μεταξύ τους για να φορτώσετε το υπόλοιπο τα συμβάντα που λάβατε. Εάν θέλετε πολλαπλούς δέκτες κάθε διαδικασία *όλα* τα συμβάντα, πρέπει να χρησιμοποιήσετε την έννοια **ConsumerGroup** . Κατά τη λήψη συμβάντα από διαφορετικούς υπολογιστές, ίσως είναι χρήσιμο για να ορίσετε ονόματα για παρουσίες EventProcessorHost με βάση το μηχανές (ή τους ρόλους) στο οποίο έχουν αναπτυχθεί.

<!-- Links -->
[Event Hubs overview]: ../articles/event-hubs/event-hubs-overview.md
[Λογαριασμός Azure χώρου αποθήκευσης]: ../articles/storage/storage-create-storage-account.md
[Azure κλασική πύλη]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

