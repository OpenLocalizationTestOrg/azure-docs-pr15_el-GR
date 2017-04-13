<properties 
    pageTitle="Επισκόπηση των API διανομείς Azure συμβάν | Microsoft Azure"
    description="Μια σύνοψη των ορισμένα από το πλήκτρο .NET διανομείς συμβάν πρόγραμμα-πελάτη APIs."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm" />

# <a name="event-hubs-api-overview"></a>Επισκόπηση API διανομείς συμβάντων

Σε αυτό το άρθρο συνοψίζει ορισμένες από τον αριθμό-κλειδί .NET διανομείς συμβάν προγράμματος-πελάτη APIs. Υπάρχουν δύο κατηγορίες: διαχείρισης και χρόνου εκτέλεσης API του Yammer. APIs χρόνου εκτέλεσης αποτελείται από όλες τις εργασίες που απαιτείται για την αποστολή και λήψη μηνύματος. Λειτουργίες διαχείρισης σάς επιτρέπουν να διαχειρίζεστε μια κατάσταση οντότητα διανομείς συμβάν κατά τη δημιουργία, ενημέρωση και διαγραφή οντοτήτων.

Σενάρια εποπτείας καλύπτουν διαχείρισης και χρόνου εκτέλεσης. Για λεπτομερή παραπομπή τεκμηρίωση για τα API .NET, ανατρέξτε στις αναφορές [.NET Bus υπηρεσίας](https://msdn.microsoft.com/library/azure/mt419900.aspx) και [EventProcessorHost API](https://msdn.microsoft.com/library/azure/mt445521.aspx) .

## <a name="management-apis"></a>APIs διαχείρισης

Για να εκτελέσετε τις ακόλουθες λειτουργίες διαχείρισης, πρέπει να έχετε **Διαχείριση** δικαιωμάτων στο χώρο ονομάτων διανομείς συμβάν:

### <a name="create"></a>Δημιουργία

```
// Create the Event Hub
EventHubDescription ehd = new EventHubDescription(eventHubName);
ehd.PartitionCount = SampleManager.numPartitions;
namespaceManager.CreateEventHubAsync(ehd).Wait();
```

### <a name="update"></a>Ενημέρωση

```
EventHubDescription ehd = await namespaceManager.GetEventHubAsync(eventHubName);

// Create a customer SAS rule with Manage permissions
ehd.UserMetadata = "Some updated info";
string ruleName = "myeventhubmanagerule";
string ruleKey = SharedAccessAuthorizationRule.GenerateRandomKey();
ehd.Authorization.Add(new SharedAccessAuthorizationRule(ruleName, ruleKey, new AccessRights[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send} )); 
namespaceManager.UpdateEventHubAsync(ehd).Wait();
```

### <a name="delete"></a>Διαγραφή

```
namespaceManager.DeleteEventHubAsync("Event Hub name").Wait();
```

## <a name="run-time-apis"></a>APIs χρόνου εκτέλεσης

### <a name="create-publisher"></a>Δημιουργία δημοσίευσης

```
// EventHubClient model (uses implicit factory instance, so all links on same connection)
EventHubClient eventHubClient = EventHubClient.Create("Event Hub name");
```

### <a name="publish-message"></a>Δημοσίευση μηνύματος

```
// Create the device/temperature metric
MetricEvent info = new MetricEvent() { DeviceId = random.Next(SampleManager.NumDevices), Temperature = random.Next(100) };
EventData data = new EventData(new byte[10]); // Byte array
EventData data = new EventData(Stream); // Stream 
EventData data = new EventData(info, serializer) //Object and serializer 
    {
       PartitionKey = info.DeviceId.ToString()
    };

// Set user properties if needed
data.Properties.Add("Type", "Telemetry_" + DateTime.Now.ToLongTimeString());

// Send single message async
await client.SendAsync(data);
```

### <a name="create-consumer"></a>Δημιουργία καταναλωτή

```
// Create the Event Hubs client
EventHubClient eventHubClient = EventHubClient.Create(EventHubName);

// Get the default consumer group
EventHubConsumerGroup defaultConsumerGroup = eventHubClient.GetDefaultConsumerGroup();

// All messages
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index);

// From one day ago
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index, startingDateTimeUtc:DateTime.Now.AddDays(-1));
                        
// From specific offset, -1 means oldest
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index,startingOffset:-1); 
```

### <a name="consume-message"></a>Εκμετάλλευση μηνύματος

```
var message = await consumer.ReceiveAsync();

// Provide a serializer
var info = message.GetBody<Type>(Serializer)
                                    
// Get a byte[]
var info = message.GetBytes(); 
msg = UnicodeEncoding.UTF8.GetString(info);
```

## <a name="event-processor-host-apis"></a>Κεντρικός υπολογιστής επεξεργαστής συμβάν APIs

Αυτά τα API παρέχουν ανοχή για διαδικασίες εργασίας που μπορεί να είναι διαθέσιμα, με τη διανομή shards σε διαθέσιμη εργαζομένων.

```
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.
// Use the EventData.Offset value for checkpointing yourself, this value is unique per partition.

string eventHubConnectionString = System.Configuration.ConfigurationManager.AppSettings["Microsoft.ServiceBus.ConnectionString"];
string blobConnectionString = System.Configuration.ConfigurationManager.AppSettings["AzureStorageConnectionString"]; // Required for checkpoint/state

EventHubDescription eventHubDescription = new EventHubDescription(EventHubName);
EventProcessorHost host = new EventProcessorHost(WorkerName, EventHubName, defaultConsumerGroup.GroupName, eventHubConnectionString, blobConnectionString);
            host.RegisterEventProcessorAsync<SimpleEventProcessor>();

// To close
host.UnregisterEventProcessorAsync().Wait();   
```

Το περιβάλλον εργασίας [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) ορίζεται ως εξής:

```
public class SimpleEventProcessor : IEventProcessor
{
    IDictionary<string, string> map;
    PartitionContext partitionContext;

    public SimpleEventProcessor()
    {
        this.map = new Dictionary<string, string>();
    }

    public Task OpenAsync(PartitionContext context)
    {
        this.partitionContext = context;

        return Task.FromResult<object>(null);
    }

    public async Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData message in messages)
        {
            Process messages here
        }
        
        // Checkpoint when appropriate
        await context.CheckpointAsync();

    }


    public async Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
}
```

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με το συμβάν διανομείς σενάρια, επισκεφθείτε αυτές τις συνδέσεις:

- [Τι είναι το Azure συμβάν διανομείς;](event-hubs-what-is-event-hubs.md)
- [Επισκόπηση διανομείς συμβάντων](event-hubs-overview.md)
- [Οδηγός προγραμματισμού διανομείς συμβάντος](event-hubs-programming-guide.md)
- [Δείγματα κώδικα διανομείς συμβάντος](http://code.msdn.microsoft.com/site/search?query=event hub&f[0].Value=event hubs&f[0].Type=SearchText&ac=5)

Οι αναφορές .NET API είναι εδώ:

- [Υπηρεσία Bus και συμβάντων διανομείς .NET API αναφορών](https://msdn.microsoft.com/library/azure/mt419900.aspx)
- [Κεντρικός υπολογιστής επεξεργαστής συμβάν API αναφοράς](https://msdn.microsoft.com/library/azure/mt445521.aspx)
