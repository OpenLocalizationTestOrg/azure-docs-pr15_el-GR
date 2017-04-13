<properties
    pageTitle="Azure IoT διανομέα για γρήγορα αποτελέσματα Java | Microsoft Azure"
    description="Πρόγραμμα εκμάθησης για Azure IoT διανομέα με Java γρήγορα αποτελέσματα. Χρήση διανομέα IoT Azure και Java με το Microsoft Azure IoT SDK για την υλοποίηση μιας λύσης Internet των στοιχείων."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>Γρήγορα αποτελέσματα με το Azure IoT διανομέα για Java

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Στο τέλος αυτού του προγράμματος εκμάθησης, έχετε τρεις εφαρμογές κονσόλας Java:

* **Δημιουργία-συσκευή-ταυτότητα**, που δημιουργεί μια συσκευή ταυτότητας και πλήκτρο συσχετισμένη ασφάλεια για να συνδέσετε τη συσκευή προσομοιωμένη.
* **Ανάγνωση-d2c-μηνυμάτων**, που εμφανίζει το τηλεμετρίας αποστέλλονται από προσομοιωμένη τη συσκευή σας.
* **προσομοιωμένη συσκευή**, η οποία συνδέεται με το Κέντρο IoT σας με την ταυτότητα της συσκευής που δημιουργήσατε νωρίτερα και στέλνει ένα μήνυμα τηλεμετρίας κάθε δεύτερη χρησιμοποιώντας το πρωτόκολλο AMQP.

> [AZURE.NOTE] Το άρθρο [SDK διανομέα IoT] [ lnk-hub-sdks] παρέχει πληροφορίες σχετικά με τα διάφορα SDK που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε δύο εφαρμογές για να εκτελέσετε σε συσκευές και το τέλος πίσω λύση.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

+ Java SE 8. <br/> [Προετοιμασία περιβάλλον ανάπτυξής σας] [ lnk-dev-setup] περιγράφει τον τρόπο εγκατάστασης Java για αυτό το πρόγραμμα εκμάθησης στα Windows ή Linux.

+ Maven 3.  <br/> [Προετοιμασία περιβάλλον ανάπτυξής σας] [ lnk-dev-setup] περιγράφει τον τρόπο εγκατάστασης Maven για αυτό το πρόγραμμα εκμάθησης στα Windows ή Linux.

+ Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Ως τελικό βήμα, σημειώστε την τιμή **πρωτεύοντος κλειδιού** και, στη συνέχεια, κάντε κλικ στην επιλογή **μηνυμάτων**. Στην blade τα **μηνύματα** , σημειώστε το **όνομα διανομέας συμβατό με το συμβάν** και το **τελικό σημείο διανομέα συμβατό με το συμβάν**. Χρειάζεστε αυτές τις τρεις τιμές κατά τη δημιουργία εφαρμογής **Ανάγνωση-d2c-μηνυμάτων** σας.

![Azure πύλης blade IoT το Κέντρο μηνυμάτων][6]

Τώρα που έχετε δημιουργήσει το Κέντρο IoT σας και έχετε το hostname IoT διανομέα, συμβολοσειρά σύνδεσης IoT διανομέα, IoT διανομέα πρωτεύον κλειδί, όνομα διανομέας συμβατό με το συμβάν και τελικού σημείου διανομέα συμβατό με το συμβάν που χρειάζεστε για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης.

## <a name="create-a-device-identity"></a>Δημιουργήστε μια ταυτότητα συσκευής

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Java που δημιουργεί μια νέα ταυτότητα συσκευής στο μητρώο ταυτότητας από την ενότητα IoT. Μια συσκευή δεν μπορεί να συνδεθεί με IoT διανομέα, εκτός εάν έχει μια καταχώρηση στο μητρώο ταυτότητας συσκευή. Ανατρέξτε στην ενότητα **Συσκευή ταυτότητας μητρώου** του [Οδηγού για προγραμματιστές διανομέα IoT] [ lnk-devguide-identity] για περισσότερες πληροφορίες. Κατά την εκτέλεση αυτής της εφαρμογής κονσόλας, δημιουργεί μια συσκευή μοναδικό Αναγνωριστικό και το κλειδί που να χρησιμοποιήσετε τη συσκευή σας για να αναγνωρίζεται όταν στέλνει μηνύματα συσκευής στο cloud με IoT διανομέα.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται iot-java-get-αποτελέσματα. Στο φάκελο iot-java-get-αποτελέσματα, δημιουργήστε ένα νέο έργο Maven που ονομάζεται **Δημιουργία-συσκευή-ταυτότητας** χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Σημειώστε ότι αυτό είναι μια εντολή μεγάλου:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Από την γραμμή εντολών, μεταβείτε στο νέο φάκελο ταυτότητα-δημιουργία-συσκευή.

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο pom.xml στο φάκελο δημιουργία-συσκευή-ταυτότητας και προσθέστε την ακόλουθη εξάρτηση στον κόμβο **εξαρτήσεις** . Αυτό σας επιτρέπει να χρησιμοποιήσετε το πακέτο iothub-υπηρεσία-sdk στην εφαρμογή σας:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. Αποθηκεύστε και κλείστε το αρχείο pom.xml.

5. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο create-device-identity\src\main\java\com\mycompany\app\App.java.

6. Προσθέστε τις παρακάτω προτάσεις για την **Εισαγωγή** του αρχείου:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Προσθέστε τις ακόλουθες μεταβλητές επίπεδο κλάσης στην **εφαρμογή** κλάση, αντικαθιστώντας **{yourhubconnectionstring}** με την τιμή σας αναφέρθηκε προηγουμένως:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. Τροποποιήστε την υπογραφή του **κύριου** μέθοδο για να συμπεριλάβετε τις εξαιρέσεις ως εξής:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Προσθέστε τον ακόλουθο κώδικα ως σώμα του **κύριου** τη μέθοδο. Αυτός ο κώδικας δημιουργεί μια συσκευή που ονομάζεται *javadevice* στο μητρώο την ταυτότητά σας IoT διανομέα, εάν δεν υπάρχει ήδη. Στη συνέχεια, εμφανίζει το αναγνωριστικό συσκευής και το κλειδί που χρειάζεστε αργότερα:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Αποθηκεύστε και κλείστε το αρχείο App.java.

11. Για να δημιουργήσετε την εφαρμογή **Δημιουργία-συσκευή-ταυτότητας** χρησιμοποιώντας Maven, εκτελέστε την ακόλουθη εντολή στη γραμμή εντολών στο φάκελο δημιουργία-συσκευή-ταυτότητας:

    ```
    mvn clean package -DskipTests
    ```

12. Για να εκτελέσετε την εφαρμογή **Δημιουργία-συσκευή-ταυτότητας** χρησιμοποιώντας Maven, εκτελέστε την ακόλουθη εντολή στη γραμμή εντολών στο φάκελο δημιουργία-συσκευή-ταυτότητας:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Σημειώστε το **αναγνωριστικό συσκευής** και το **κλειδί συσκευής**. Θα χρειαστείτε αυτές τις νεότερες όταν δημιουργείτε μια εφαρμογή η οποία συνδέεται με διανομέα IoT ως συσκευή.

> [AZURE.NOTE] Το μητρώο ταυτότητας διανομέα IoT αποθηκεύει μόνο τις ταυτότητες συσκευή για να ενεργοποιήσετε την ασφαλή πρόσβαση για την ενότητα. Αποθηκεύει αναγνωριστικά συσκευών και αριθμούς-κλειδιά για να χρησιμοποιήσετε ως διαπιστευτηρίων ασφαλείας και μια σημαία ενεργοποίηση/απενεργοποίηση που μπορείτε να χρησιμοποιήσετε για να απενεργοποιήσετε την πρόσβαση για μια μεμονωμένη συσκευή. Εάν η εφαρμογή σας πρέπει να αποθηκεύσετε άλλα μετα-δεδομένα ειδικά για τη συσκευή, πρέπει να χρησιμοποιεί μια συγκεκριμένη εφαρμογή store. Ανατρέξτε στον [Οδηγό για προγραμματιστές διανομέα IoT] [ lnk-devguide-identity] για περισσότερες πληροφορίες.

## <a name="receive-device-to-cloud-messages"></a>Λήψη μηνυμάτων συσκευής στο cloud

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Java που διαβάζει τα μηνύματα συσκευής στο cloud από το Κέντρο IoT. Ένα διανομέα IoT εκθέτει ένα [Συμβάν διανομέα][lnk-event-hubs-overview]-συμβατά τελικό σημείο για να μπορέσετε να την ανάγνωση μηνυμάτων συσκευής στο cloud. Για να διατηρήσω τα πράγματα απλά, αυτό το πρόγραμμα εκμάθησης δημιουργεί ένα βασικό πρόγραμμα ανάγνωσης που δεν είναι κατάλληλη για μια ανάπτυξη υψηλής απόδοσης. Τα [μηνύματα συσκευή-cloud διαδικασία] [ lnk-process-d2c-tutorial] πρόγραμμα εκμάθησης σας δείχνει πώς μπορείτε να επεξεργαστούν μηνύματα συσκευής στο cloud με κλίμακα. Το παράθυρο [Γρήγορα αποτελέσματα με το συμβάν διανομείς] [ lnk-eventhubs-tutorial] πρόγραμμα εκμάθησης παρέχει περαιτέρω πληροφορίες σχετικά με τον τρόπο για να επεξεργαστείτε μηνύματα από το συμβάν διανομείς και ισχύει για τα τελικά σημεία συμβατά διανομέα IoT διανομέα συμβάν.

> [AZURE.NOTE] Το τελικό σημείο συμβατό διανομέα συμβάντων για την ανάγνωση μηνυμάτων συσκευή-cloud πάντα χρησιμοποιεί το πρωτόκολλο AMQP.

1. Στο iot-java-get-αποτελέσματα φάκελο που δημιουργήσατε στην ενότητα *Δημιουργία μιας συσκευής ταυτότητας* , Δημιουργία νέου έργου Maven που ονομάζεται **Ανάγνωση-d2c-μηνυμάτων** , χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Σημειώστε ότι αυτό είναι μια εντολή μεγάλου:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Από την γραμμή εντολών, μεταβείτε στο νέο φάκελο ανάγνωση-d2c-μηνυμάτων.

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο pom.xml στον φάκελο ανάγνωση-d2c-μηνυμάτων και προσθέστε την ακόλουθη εξάρτηση στον κόμβο **εξαρτήσεις** . Αυτό σας επιτρέπει να χρησιμοποιήσετε το πακέτο eventhubs-πελάτη στην εφαρμογή σας για να διαβάσετε από το τελικό σημείο διανομέα συμβατό με το συμβάν:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. Αποθηκεύστε και κλείστε το αρχείο pom.xml.

5. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο read-d2c-messages\src\main\java\com\mycompany\app\App.java.

6. Προσθέστε τις παρακάτω προτάσεις για την **Εισαγωγή** του αρχείου:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Προσθέστε τις ακόλουθες μεταβλητές επίπεδο κλάσης την κλάση **εφαρμογής** . Αντικατάσταση **{youriothubkey}**, **{youreventhubcompatibleendpoint}**και **{youreventhubcompatiblename}** με τις τιμές που σημειώσατε προηγουμένως:

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Προσθέστε την ακόλουθη μέθοδο **receiveMessages** στην **εφαρμογή** κλάση. Αυτή η μέθοδος δημιουργεί μια παρουσία **EventHubClient** για να συνδεθείτε με το τελικό σημείο διανομέα συμβατό με το συμβάν και, στη συνέχεια, ασύγχρονα δημιουργεί μια παρουσία **PartitionReceiver** ανάγνωσης από ένα διαμερίσματα διανομέα συμβάν. Το βρόχοι συνεχώς και εκτυπώνει τις λεπτομέρειες μηνύματος μέχρι να τερματιστεί η εφαρμογή.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Αυτή η μέθοδος χρησιμοποιεί ένα φίλτρο όταν δημιουργεί το ακουστικό, έτσι ώστε το ακουστικό διαβάζει μόνο τα μηνύματα που αποστέλλονται σε διανομέα IoT μετά το ακουστικό αρχίζει να λειτουργεί. Αυτό είναι χρήσιμο σε περιβάλλον δοκιμής, ώστε να μπορείτε να δείτε το τρέχον σύνολο των μηνυμάτων. Σε ένα περιβάλλον παραγωγής πρέπει να βεβαιωθείτε ότι ο κώδικας να επεξεργάζεται όλα τα μηνύματα - δείτε [τον τρόπο για την επεξεργασία μηνυμάτων συσκευή-cloud διανομέα IoT] [ lnk-process-d2c-tutorial] προγραμμάτων εκμάθησης για περισσότερες πληροφορίες.

9. Τροποποιήστε την υπογραφή του **κύριου** μέθοδο για να συμπεριλάβετε την εξαίρεση ως εξής:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. Προσθέστε τον ακόλουθο κώδικα στην **κύρια** μέθοδο στην τάξη **εφαρμογής** . Αυτός ο κωδικός δημιουργεί τις δύο παρουσίες **EventHubClient** και **PartitionReceiver** και σας δίνει τη δυνατότητα να κλείσετε την εφαρμογή, όταν ολοκληρώσετε την επεξεργασία μηνυμάτων:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] Αυτός ο κωδικός προϋποθέτει που δημιουργήσατε το Κέντρο IoT σας στο επίπεδο F1 (δωρεάν). Ένα δωρεάν διανομέα IoT έχει δύο διαμερίσματα με το όνομα "0" και "1".

11. Αποθηκεύστε και κλείστε το αρχείο App.java.

12. Για να δημιουργήσετε την εφαρμογή **Ανάγνωση-d2c-μηνυμάτων** χρησιμοποιώντας Maven, εκτελέστε την ακόλουθη εντολή στη γραμμή εντολών στο φάκελο ανάγνωση-d2c-μηνυμάτων:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Δημιουργία εφαρμογής προσομοιωμένη συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Java που προσομοιώνει μια συσκευή που στέλνει μηνύματα συσκευής στο cloud με ένα διανομέα IoT.

1. Στο iot-java-get-αποτελέσματα φάκελο που δημιουργήσατε στην ενότητα *Δημιουργία μιας συσκευής ταυτότητας* , Δημιουργία νέου έργου Maven που ονομάζεται **προσομοιωμένη συσκευή** χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Σημειώστε ότι αυτό είναι μια εντολή μεγάλου:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Από την γραμμή εντολών, μεταβείτε στο νέο φάκελο προσομοιωμένη συσκευή.

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο pom.xml στο φάκελο προσομοιωμένη συσκευή και προσθέστε τις ακόλουθες εξαρτήσεις στον κόμβο **εξαρτήσεις** . Αυτό σας επιτρέπει να χρησιμοποιήσετε το πακέτο iothub-java-πελάτη στην εφαρμογή σας για να επικοινωνήσετε με το Κέντρο IoT σας και να σειριοποιεί αντικείμενα Java σε JSON:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Αποθηκεύστε και κλείστε το αρχείο pom.xml.

5. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο simulated-device\src\main\java\com\mycompany\app\App.java.

6. Προσθέστε τις παρακάτω προτάσεις για την **Εισαγωγή** του αρχείου:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Προσθέστε τις ακόλουθες μεταβλητές επίπεδο κλάσης στην **εφαρμογή** κλάση, αντικαθιστώντας **{youriothubname}** με το όνομα διανομέας IoT και **{yourdevicekey}** με την τιμή του κλειδιού συσκευή που δημιουργήσατε στην ενότητα *Δημιουργία μιας συσκευής ταυτότητας* :

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    Αυτή η εφαρμογή δείγμα χρησιμοποιεί τη μεταβλητή **πρωτόκολλο** όταν το εμφανίζει ένα αντικείμενο **DeviceClient** . Μπορείτε να χρησιμοποιήσετε το HTTP ή AMQP πρωτόκολλο, για να επικοινωνήσετε με το Κέντρο IoT.

8. Προσθέστε τα εξής ένθετες κλάσης **TelemetryDataPoint** μέσα την κλάση **εφαρμογής** για να καθορίσετε τα δεδομένα τηλεμετρίας τη συσκευή σας στέλνει το Κέντρο IoT σας:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Προσθέστε την ακόλουθη ένθετη κλάση **EventCallback** μέσα την κλάση **εφαρμογής** για να εμφανίσετε την κατάσταση βεβαίωση λήψης που επιστρέφει ο διανομέας IoT κατά την επεξεργασία ενός μηνύματος από τη συσκευή προσομοιωμένη. Αυτή η μέθοδος ειδοποιεί επίσης το κύριο νήμα στην εφαρμογή, όταν το μήνυμα έχει υποστεί επεξεργασία:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Προσθέστε την ακόλουθη ένθετη κλάση **MessageSender** μέσα την κλάση **εφαρμογής** . Η μέθοδος **Εκτέλεση** αυτής της κατηγορίας δημιουργεί δείγμα δεδομένα τηλεμετρίας για να στείλετε με το διανομέα IoT και περιμένει μια βεβαίωση λήψης πριν από την αποστολή του επόμενου μηνύματος:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Αυτή η μέθοδος αποστέλλει ένα νέο μήνυμα συσκευής στο cloud ένα δευτερόλεπτο μετά την ενότητα IoT αναγνωρίσει του προηγούμενου μηνύματος. Το μήνυμα περιέχει ένα αντικείμενο σειριοποιημένο JSON με το deviceId και σε έναν αριθμό που δημιούργησε τυχαία για να προσομοιώσετε αισθητήρα ταχύτητα ανέμου.

11. Αντικαταστήστε το **κύριο** μέθοδο με τον ακόλουθο κώδικα που δημιουργεί ένα νήμα για την αποστολή μηνυμάτων συσκευής στο cloud για το Κέντρο IoT σας:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. Αποθηκεύστε και κλείστε το αρχείο App.java.

13. Για να δημιουργήσετε την εφαρμογή **προσομοιωμένη συσκευής** χρησιμοποιώντας Maven, εκτελέστε την ακόλουθη εντολή στη γραμμή εντολών στο φάκελο προσομοιωμένη συσκευή:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Για να διατηρήσετε πράγματα απλή, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως μια εκθετική διπλασιασμών), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων][lnk-transient-faults].

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1. Σε μια γραμμή εντολών στο φάκελο d2c ανάγνωση, εκτελέστε την ακόλουθη εντολή για να ξεκινήσετε την παρακολούθηση του πρώτου διαμερίσματα στο κέντρο IoT σας:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Εφαρμογή προγράμματος-πελάτη υπηρεσίας διανομέα IoT Java για την παρακολούθηση της συσκευής στο cloud μηνυμάτων][7]

2. Σε μια γραμμή εντολών στο φάκελο προσομοιωμένη συσκευή, εκτελέστε την ακόλουθη εντολή για να ξεκινήσει η αποστολή δεδομένα τηλεμετρίας για το Κέντρο IoT σας:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Εφαρμογή προγράμματος-πελάτη συσκευή διανομέα IoT Java για την αποστολή μηνυμάτων συσκευής στο cloud][8]

3. Το πλακίδιο **Χρήση** στην [πύλη του Azure] [ lnk-portal] εμφανίζει τον αριθμό των μηνυμάτων που αποστέλλονται σε την ενότητα:

    ![Azure πύλης χρήση πλακιδίων που εμφανίζει αριθμό των μηνυμάτων που αποστέλλονται σε διανομέα IoT][43]

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, που έχει ρυθμιστεί διανομέα IoT στην πύλη του Azure και, στη συνέχεια, να δημιουργήσει μια ταυτότητα συσκευής στο μητρώο ταυτότητας την ενότητα. Χρησιμοποιείται αυτή η ταυτότητα συσκευή για να ενεργοποιήσετε την εφαρμογή προσομοίωση συσκευή για την αποστολή μηνυμάτων συσκευής στο cloud για την ενότητα. Μπορείτε επίσης να δημιουργήσει μια εφαρμογή που εμφανίζει τα μηνύματα που ελήφθησαν από την ενότητα. 

Για να συνεχίσετε γρήγορα αποτελέσματα με το Κέντρο IoT και να εξερευνήσετε τα άλλα σενάρια IoT ανατρέξτε στα θέματα:

- [Συνδέει τη συσκευή σας][lnk-connect-device]
- [Γρήγορα αποτελέσματα με τη Διαχείριση συσκευών][lnk-device-management]
- [Γρήγορα αποτελέσματα με το SDK IoT πύλης][lnk-gateway-SDK]

Για να μάθετε πώς μπορείτε να επεκτείνετε τη λύση σας IoT και επεξεργασία των μηνυμάτων συσκευής στο cloud με κλίμακα, δείτε τα [μηνύματα συσκευή-cloud διαδικασία] [ lnk-process-d2c-tutorial] το πρόγραμμα εκμάθησης.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/