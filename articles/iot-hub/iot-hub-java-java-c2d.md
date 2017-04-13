<properties
    pageTitle="Αποστολή μηνυμάτων cloud-σε-συσκευή με το Κέντρο IoT | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης για να μάθετε πώς μπορείτε να στείλετε μηνύματα cloud-σε-συσκευή χρησιμοποιώντας το Azure IoT διανομέα με Java."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>Πρόγραμμα εκμάθησης: Πώς μπορείτε να στείλετε μηνύματα cloud-σε-συσκευή με το Κέντρο IoT και Java

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Εισαγωγή

Azure IoT διανομέα είναι μια πλήρως διαχειριζόμενη υπηρεσία που σας βοηθά να ενεργοποιήσετε αξιόπιστη και ασφαλή επικοινωνίες διπλής κατεύθυνσης μεταξύ εκατομμύρια IoT συσκευές και το τέλος ξανά μια εφαρμογή. Το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με το Κέντρο IoT] δείχνει πώς μπορείτε να δημιουργήσετε ένα διανομέα IoT, προμήθεια μιας συσκευής ταυτότητας σε αυτό και κώδικα μια πρόχειρη συσκευή που στέλνει μηνύματα συσκευής στο cloud.

Αυτό το πρόγραμμα εκμάθησης δημιουργεί στην [Γρήγορα αποτελέσματα με το Κέντρο IoT]. Σας δείχνει πώς να:

- Από την εφαρμογή cloud παρασκηνίου, αποστολή μηνυμάτων cloud-σε-συσκευή σε μία μόνο συσκευή μέσω IoT διανομέα.
- Λαμβάνετε μηνύματα cloud-σε-συσκευή σε μια συσκευή.
- Από το cloud εφαρμογή πίσω τέλος, ζητήσετε αποδεικτικό παράδοσης (*σχόλια*) για τα μηνύματα που αποστέλλονται σε μια συσκευή από διανομέα IoT.

Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με τα μηνύματα cloud-σε-συσκευή του [Οδηγού για προγραμματιστές διανομέα IoT][IoT Hub Developer Guide - C2D].

Στο τέλος αυτού του προγράμματος εκμάθησης, μπορείτε να εκτελέσετε δύο Java κονσόλα εφαρμογών:

* **προσομοιωμένη συσκευής**, μια τροποποιημένη έκδοση της εφαρμογής που έχουν δημιουργηθεί με [Γρήγορα αποτελέσματα με το Κέντρο IoT], το οποίο συνδέεται με το Κέντρο IoT σας και να λαμβάνει μηνύματα cloud-σε-συσκευή.
* **Αποστολή-c2d-μηνυμάτων**, τα οποία αποστέλλει ένα μήνυμα cloud-σε-συσκευή προσομοιωμένη συσκευή μέσω διανομέα IoT και, στη συνέχεια, λαμβάνει το αποδεικτικό παράδοσης.

> [AZURE.NOTE] Ο διανομέας IoT διαθέτει SDK υποστήριξης για πολλές γλώσσες (συμπεριλαμβανομένων των C, Java και Javascript) μέσω SDK συσκευή Azure IoT και πλατφόρμες συσκευών. Για οδηγίες βήμα προς βήμα σχετικά με τον τρόπο για να συνδέσετε τη συσκευή για αυτό το πρόγραμμα εκμάθησης του κώδικα και γενικά για Azure IoT διανομέα, ανατρέξτε στο [Κέντρο για προγραμματιστές του Azure IoT].

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

+ Java SE 8. <br/> [Προετοιμασία περιβάλλον ανάπτυξής σας] [ lnk-dev-setup] περιγράφει τον τρόπο εγκατάστασης Java για αυτό το πρόγραμμα εκμάθησης στα Windows ή Linux.

+ Maven 3.  <br/> [Προετοιμασία περιβάλλον ανάπτυξής σας] [ lnk-dev-setup] περιγράφει τον τρόπο εγκατάστασης Maven για αυτό το πρόγραμμα εκμάθησης στα Windows ή Linux.

+ Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

## <a name="receive-messages-on-the-simulated-device"></a>Λήψη μηνυμάτων από τη συσκευή προσομοιωμένη

Σε αυτήν την ενότητα, μπορείτε να τροποποιήσετε την εφαρμογή προσομοιωμένη συσκευή που δημιουργήσατε [Γρήγορα αποτελέσματα με το Κέντρο IoT] να λαμβάνετε μηνύματα cloud-σε-συσκευή από την ενότητα IoT.

1. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο simulated-device\src\main\java\com\mycompany\app\App.java.

2. Προσθέστε την ακόλουθη κλάση **MessageCallback** ως ένθετη κλάση μέσα την κλάση **εφαρμογής** . Η **Εκτέλεση** μεθόδου ενεργοποιείται όταν η συσκευή λάβει ένα μήνυμα από διανομέα IoT. Σε αυτό το παράδειγμα, η συσκευή πάντα ειδοποιεί την ενότητα ώστε να ολοκληρωθεί το μήνυμα:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. Τροποποιήστε την **κύρια** μέθοδο για να δημιουργήσετε μια παρουσία **MessageCallback** και να καλέσετε τη μέθοδο **setMessageCallback** πριν από την ανοίγει το πρόγραμμα-πελάτη ως εξής:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] Εάν χρησιμοποιείτε HTTP αντί για MQTT ή AMQP ως τη μεταφορά, την παρουσία **DeviceClient** ελέγχει για τα μηνύματα από σπάνια διανομέα IoT (λιγότερο από κάθε 25 λεπτά). Για περισσότερες πληροφορίες σχετικά με τις διαφορές μεταξύ MQTT, AMQP και HTTP υποστήριξης και περιορισμού IoT διανομέα, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές διανομέα IoT][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Αποστολή μηνύματος cloud-σε-συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Java που στέλνει μηνύματα cloud-σε-συσκευή στην εφαρμογή προσομοιωμένη συσκευή. Χρειάζεστε το αναγνωριστικό της συσκευής που προσθέσατε στην εκμάθηση [Γρήγορα αποτελέσματα με το Κέντρο IoT] συσκευή. Χρειάζεστε επίσης τη συμβολοσειρά σύνδεσης για το Κέντρο σας IoT που μπορείτε να βρείτε στην [πύλη του Azure].

1. Δημιουργήστε ένα έργο Maven που ονομάζεται **Αποστολή-c2d-μηνυμάτων** , χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Σημειώστε ότι αυτό είναι μια εντολή μεγάλου:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Από την γραμμή εντολών, μεταβείτε στο νέο φάκελο αποστολή-c2d-μηνυμάτων.

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο pom.xml στο φάκελο αποστολή-c2d-μηνυμάτων και προσθέστε την ακόλουθη εξάρτηση στον κόμβο **εξαρτήσεις** . Προσθήκη την εξάρτηση σάς επιτρέπει να χρησιμοποιήσετε το πακέτο **iothub-java-υπηρεσία-πελάτη** στην εφαρμογή σας για να επικοινωνήσετε με την υπηρεσία διανομέα IoT:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Αποθηκεύστε και κλείστε το αρχείο pom.xml.

5. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο send-c2d-messages\src\main\java\com\mycompany\app\App.java.

6. Προσθέστε τις παρακάτω προτάσεις για την **Εισαγωγή** του αρχείου:

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Προσθέστε τις ακόλουθες μεταβλητές επίπεδο κλάσης στην **εφαρμογή** κλάση, αντικαθιστώντας **{yourhubconnectionstring}** και **{yourdeviceid}** με τις τιμές σας αναφέρθηκε προηγουμένως:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. Αντικαταστήστε το **κύριο** μέθοδο με τον ακόλουθο κώδικα που συνδέεται με το Κέντρο IoT σας, στέλνει ένα μήνυμα στη συσκευή σας και, στη συνέχεια, χρειάζεται να περιμένει για επιβεβαίωση ότι η συσκευή λάβει και υποβάλλονται σε επεξεργασία του μηνύματος:

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] Για χάρη της απλότητα, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως εκθετική διπλασιασμών), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων].

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1. Σε μια γραμμή εντολών στο φάκελο προσομοιωμένη συσκευή, εκτελέστε την ακόλουθη εντολή για να ξεκινήσει η αποστολή τηλεμετρίας για το Κέντρο IoT σας και για να ακούσετε για cloud-σε-συσκευή μηνύματα που αποστέλλονται από την ενότητα:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Εκτελέστε την εφαρμογή προσομοιωμένη συσκευή][img-simulated-device]

2. Σε μια γραμμή εντολών στο φάκελο αποστολή-c2d-μηνυμάτων, εκτελέστε την ακόλουθη εντολή για να στείλετε ένα μήνυμα cloud-σε-συσκευή και περιμένετε να εμφανιστεί μια επιβεβαίωση σχολίων:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Εκτελέστε την εντολή για να στείλετε το μήνυμα cloud-σε-συσκευή][img-send-command]

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς μπορείτε να στέλνετε και να λαμβάνετε μηνύματα cloud-σε-συσκευή. 

Για να δείτε παραδείγματα ολοκλήρωσης για ολοκληρωμένες λύσεις που χρησιμοποιούν IoT διανομέα, ανατρέξτε στο θέμα [Οικογένεια IoT Azure].

Για να μάθετε περισσότερα σχετικά με την ανάπτυξη λύσεων με IoT διανομέα, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές IoT διανομέα].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Γρήγορα αποτελέσματα με το Κέντρο IoT]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Οδηγός για προγραμματιστές IoT διανομέα]: iot-hub-devguide.md
[Κέντρο για προγραμματιστές IoT Azure]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Χειρισμός μεταβατικές σφαλμάτων]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Πύλη του Azure]: https://portal.azure.com
[Azure IoT οικογένεια προγραμμάτων]: https://azure.microsoft.com/documentation/suites/iot-suite/