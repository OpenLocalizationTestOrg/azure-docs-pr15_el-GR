<properties
    pageTitle="Αποστολή μηνυμάτων cloud-σε-συσκευή με το Κέντρο IoT | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης για να μάθετε πώς μπορείτε να στείλετε μηνύματα cloud-σε-συσκευή χρησιμοποιώντας το Azure IoT διανομέα με Java."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/23/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-nodejs"></a>Πρόγραμμα εκμάθησης: Πώς μπορείτε να στείλετε μηνύματα cloud-σε-συσκευή με το Κέντρο IoT και Node.js

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Εισαγωγή

Azure IoT διανομέα είναι μια πλήρως διαχειριζόμενη υπηρεσία που σας βοηθά να ενεργοποιήσετε αξιόπιστη και ασφαλή επικοινωνίες διπλής κατεύθυνσης μεταξύ εκατομμύρια IoT συσκευές και το τέλος ξανά μια εφαρμογή. Το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με το Κέντρο IoT] δείχνει πώς μπορείτε να δημιουργήσετε ένα διανομέα IoT, προμήθεια μιας συσκευής ταυτότητας σε αυτό και κώδικα μια πρόχειρη συσκευή που στέλνει μηνύματα συσκευής στο cloud.

Αυτό το πρόγραμμα εκμάθησης δημιουργεί στην [Γρήγορα αποτελέσματα με το Κέντρο IoT]. Σας δείχνει πώς να:

- Από την εφαρμογή cloud παρασκηνίου, αποστολή μηνυμάτων cloud-σε-συσκευή σε μία μόνο συσκευή μέσω IoT διανομέα.
- Λαμβάνετε μηνύματα cloud-σε-συσκευή σε μια συσκευή.
- Από το cloud εφαρμογή πίσω τέλος, ζητήσετε αποδεικτικό παράδοσης (*σχόλια*) για τα μηνύματα που αποστέλλονται σε μια συσκευή από διανομέα IoT.

Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με τα μηνύματα cloud-σε-συσκευή του [Οδηγού για προγραμματιστές διανομέα IoT][IoT Hub Developer Guide - C2D].

Στο τέλος αυτού του προγράμματος εκμάθησης, μπορείτε να εκτελέσετε δύο Node.js κονσόλα εφαρμογών:

* **SimulatedDevice**, μια τροποποιημένη έκδοση της εφαρμογής που έχουν δημιουργηθεί με [Γρήγορα αποτελέσματα με το Κέντρο IoT], το οποίο συνδέεται με το Κέντρο IoT σας και να λαμβάνει μηνύματα cloud-σε-συσκευή.
* **SendCloudToDeviceMessage**, που στέλνει ένα μήνυμα cloud-σε-συσκευή προσομοιωμένη συσκευή μέσω διανομέα IoT και, στη συνέχεια, λαμβάνει το αποδεικτικό παράδοσης.

> [AZURE.NOTE] Ο διανομέας IoT διαθέτει SDK υποστήριξης για πολλές γλώσσες (συμπεριλαμβανομένων των C, Java και Javascript) μέσω SDK συσκευή Azure IoT και πλατφόρμες συσκευών. Για οδηγίες βήμα προς βήμα σχετικά με τον τρόπο για να συνδέσετε τη συσκευή για αυτό το πρόγραμμα εκμάθησης του κώδικα και γενικά για Azure IoT διανομέα, ανατρέξτε στο [Κέντρο για προγραμματιστές του Azure IoT].

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

+ Έκδοση node.js 0.10.x ή νεότερη έκδοση.

+ Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

## <a name="receive-messages-on-the-simulated-device"></a>Λήψη μηνυμάτων από τη συσκευή προσομοιωμένη

Σε αυτήν την ενότητα, μπορείτε να τροποποιήσετε την εφαρμογή προσομοιωμένη συσκευή που δημιουργήσατε [Γρήγορα αποτελέσματα με το Κέντρο IoT] να λαμβάνετε μηνύματα cloud-σε-συσκευή από την ενότητα IoT.

1. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο SimulatedDevice.js.

2. Τροποποιήστε τη συνάρτηση **connectCallback** για να χειριστείτε τα μηνύματα που αποστέλλονται από διανομέα IoT. Σε αυτό το παράδειγμα, η συσκευή καλεί πάντα συνάρτησης **ολοκλήρωσης** να ειδοποιήσετε IoT διανομέα που να ολοκληρώσει την επεξεργασία του μηνύματος. Τη νέα έκδοση της συνάρτησης **connectCallback** μοιάζει κάπως έτσι:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

    > [AZURE.NOTE] Εάν χρησιμοποιείτε HTTP αντί για MQTT ή AMQP ως τη μεταφορά, την παρουσία **DeviceClient** ελέγχει για τα μηνύματα από σπάνια διανομέα IoT (λιγότερο από κάθε 25 λεπτά). Για περισσότερες πληροφορίες σχετικά με τις διαφορές μεταξύ MQTT, AMQP και HTTP υποστήριξης και περιορισμού IoT διανομέα, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές διανομέα IoT][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Αποστολή μηνύματος cloud-σε-συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που στέλνει μηνύματα cloud-σε-συσκευή στην εφαρμογή προσομοιωμένη συσκευή. Χρειάζεστε το αναγνωριστικό της συσκευής που προσθέσατε στην εκμάθηση [Γρήγορα αποτελέσματα με το Κέντρο IoT] συσκευή. Χρειάζεστε επίσης τη συμβολοσειρά σύνδεσης για το Κέντρο σας IoT που μπορείτε να βρείτε στην [πύλη του Azure].

1. Δημιουργήστε ένα κενό φάκελο που ονομάζεται **sendcloudtodevicemessage**. Στο φάκελο **sendcloudtodevicemessage** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```

2. Στο σας εντολών στο φάκελο **sendcloudtodevicemessage** , εκτελέστε την ακόλουθη εντολή για την εγκατάσταση του πακέτου **azure iothub** :

    ```
    npm install azure-iothub --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **SendCloudToDeviceMessage.js** στο φάκελο **sendcloudtodevicemessage** .

4. Προσθέστε τα ακόλουθα `require` προτάσεις κατά την εκκίνηση του αρχείου **SendCloudToDeviceMessage.js** :

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Προσθέστε τον ακόλουθο κώδικα **SendCloudToDeviceMessage.js** αρχείο. Αντικαταστήστε την τιμή κράτησης θέσης συμβολοσειρά σύνδεσης με τη συμβολοσειρά σύνδεσης για την ενότητα IoT που δημιουργήσατε στην εκμάθηση [Γρήγορα αποτελέσματα με το Κέντρο IoT] . Αντικαταστήστε το κείμενο κράτησης θέσης συσκευή προορισμού με το αναγνωριστικό συσκευής της συσκευής που προσθέσατε στην εκμάθηση [Γρήγορα αποτελέσματα με το Κέντρο IoT] :

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. Προσθέστε την ακόλουθη συνάρτηση για να εκτυπώσετε αποτελέσματα λειτουργίας στην κονσόλα:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Προσθέστε την ακόλουθη συνάρτηση για να εκτυπώσετε παράδοσης μηνυμάτων σχολίων στην κονσόλα:

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Προσθέστε τον παρακάτω κώδικα για να στείλετε ένα μήνυμα στη συσκευή σας και να χειρίζεται το μήνυμα σχολίων, όταν το μήνυμα cloud-σε-συσκευή αναγνωρίζει τη συσκευή:

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Αποθηκεύστε και κλείστε το αρχείο **SendCloudToDeviceMessage.js** .

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1. Στη γραμμή εντολών εντολή στο φάκελο **simulateddevice** , εκτελέστε την ακόλουθη εντολή για την αποστολή τηλεμετρίας IoT διανομέα και για να ακούσετε για μηνύματα cloud-σε-συσκευή:

    ```
    node SimulatedDevice.js 
    ```

    ![Εκτελέστε την εφαρμογή προσομοιωμένη συσκευή][img-simulated-device]

2. Σε μια γραμμή εντολών στο φάκελο **sendcloudtodevicemessage** , εκτελέστε την ακόλουθη εντολή για να στείλετε ένα μήνυμα cloud-σε-συσκευή και περιμένετε να εμφανιστεί το σχολίων επιβεβαίωση:

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![Εκτελέστε την εφαρμογή για να στείλετε την εντολή c2d][img-send-command]

    > [AZURE.NOTE] Για χάρη της απλότητα, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως εκθετική διπλασιασμών), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων].

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς μπορείτε να στέλνετε και να λαμβάνετε μηνύματα cloud-σε-συσκευή. 

Για να δείτε παραδείγματα ολοκλήρωσης για ολοκληρωμένες λύσεις που χρησιμοποιούν IoT διανομέα, ανατρέξτε στο θέμα [Οικογένεια IoT Azure].

Για να μάθετε περισσότερα σχετικά με την ανάπτυξη λύσεων με IoT διανομέα, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές IoT διανομέα].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Γρήγορα αποτελέσματα με το Κέντρο IoT]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Οδηγός για προγραμματιστές IoT διανομέα]: iot-hub-devguide.md
[Κέντρο για προγραμματιστές IoT Azure]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Χειρισμός μεταβατικές σφαλμάτων]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Πύλη του Azure]: https://portal.azure.com
[Azure IoT οικογένεια προγραμμάτων]: https://azure.microsoft.com/documentation/suites/iot-suite/