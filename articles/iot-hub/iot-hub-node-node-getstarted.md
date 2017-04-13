<properties
    pageTitle="Azure IoT διανομέας για γρήγορα αποτελέσματα Node.js | Microsoft Azure"
    description="Πρόγραμμα εκμάθησης για Azure IoT διανομέα με Node.js γρήγορα αποτελέσματα. Χρήση διανομέα IoT Azure και Node.js με το Microsoft Azure IoT SDK για την υλοποίηση μιας λύσης Internet των στοιχείων."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Γρήγορα αποτελέσματα με το Κέντρο IoT Azure για Node.js

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Στο τέλος αυτού του προγράμματος εκμάθησης, έχετε τρεις εφαρμογές κονσόλας Node.js:

* **CreateDeviceIdentity.js**, που δημιουργεί μια συσκευή ταυτότητας και πλήκτρο συσχετισμένη ασφάλεια για να συνδέσετε τη συσκευή προσομοιωμένη.
* **ReadDeviceToCloudMessages.js**, που εμφανίζει το τηλεμετρίας αποστέλλονται από προσομοιωμένη τη συσκευή σας.
* **SimulatedDevice.js**, το οποίο συνδέεται με το Κέντρο IoT σας με την ταυτότητα της συσκευής που δημιουργήσατε νωρίτερα και στέλνει ένα μήνυμα τηλεμετρίας κάθε δεύτερη χρησιμοποιώντας το πρωτόκολλο AMQP.

> [AZURE.NOTE] Το άρθρο [SDK διανομέα IoT] [ lnk-hub-sdks] παρέχει πληροφορίες σχετικά με τα διάφορα SDK που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε δύο εφαρμογές για να εκτελέσετε σε συσκευές και το τέλος πίσω λύση.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

+ Έκδοση node.js 0.10.x ή νεότερη έκδοση.

+ Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Τώρα έχετε δημιουργήσει το Κέντρο IoT σας. Έχετε το όνομα κεντρικού υπολογιστή IoT διανομέας και τη συμβολοσειρά σύνδεσης IoT διανομέα που χρειάζεστε για να ολοκληρώσετε τα υπόλοιπα αυτού του προγράμματος εκμάθησης.

## <a name="create-a-device-identity"></a>Δημιουργήστε μια ταυτότητα συσκευής

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που δημιουργεί μια ταυτότητα συσκευής στο μητρώο ταυτότητας από την ενότητα IoT. Μια συσκευή δεν μπορεί να συνδεθεί με IoT διανομέα, εκτός εάν έχει μια καταχώρηση στο μητρώο ταυτότητας συσκευή. Ανατρέξτε στην ενότητα **Συσκευή ταυτότητας μητρώου** του [Οδηγού για προγραμματιστές διανομέα IoT] [ lnk-devguide-identity] για περισσότερες πληροφορίες. Κατά την εκτέλεση αυτής της εφαρμογής κονσόλας, δημιουργεί μια συσκευή μοναδικό Αναγνωριστικό και το κλειδί που να χρησιμοποιήσετε τη συσκευή σας για να προσδιορίσετε ίδια όταν στέλνει μηνύματα συσκευής στο cloud με IoT διανομέα.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **createdeviceidentity**. Στο φάκελο **createdeviceidentity** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```

2. Στο σας εντολών στο φάκελο **createdeviceidentity** , εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε το πακέτο SDK υπηρεσίας **azure iothub** :

    ```
    npm install azure-iothub --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα αρχείο **CreateDeviceIdentity.js** στο φάκελο **createdeviceidentity** .

4. Προσθέστε τα ακόλουθα `require` δήλωση κατά την εκκίνηση του αρχείου **CreateDeviceIdentity.js** :

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Προσθέστε τον ακόλουθο κώδικα στο αρχείο **CreateDeviceIdentity.js** και αντικαταστήστε την τιμή κράτησης θέσης με τη συμβολοσειρά σύνδεσης για την ενότητα IoT που δημιουργήσατε στην προηγούμενη ενότητα: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Προσθέστε τον ακόλουθο κώδικα για να δημιουργήσετε έναν ορισμό συσκευής στο μητρώο ταυτότητα συσκευής στο κέντρο IoT σας. Αυτός ο κώδικας δημιουργεί μια συσκευή εάν το αναγνωριστικό συσκευής δεν υπάρχει στο μητρώο, διαφορετικά επιστρέφει το κλειδί της υπάρχουσας συσκευής:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Αποθηκεύστε και κλείστε το αρχείο **CreateDeviceIdentity.js** .

8. Για να εκτελέσετε την εφαρμογή **createdeviceidentity** , εκτελέστε την ακόλουθη εντολή στη γραμμή εντολών στο φάκελο createdeviceidentity:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Σημειώστε το **αναγνωριστικό συσκευής** και το **κλειδί συσκευής**. Χρειάζεστε αυτές τις τιμές αργότερα όταν δημιουργείτε μια εφαρμογή η οποία συνδέεται με διανομέα IoT ως συσκευή.

> [AZURE.NOTE] Το μητρώο ταυτότητας διανομέα IoT αποθηκεύει μόνο τις ταυτότητες συσκευή για να ενεργοποιήσετε την ασφαλή πρόσβαση για την ενότητα. Αποθηκεύει αναγνωριστικά συσκευών και αριθμούς-κλειδιά για να χρησιμοποιήσετε ως διαπιστευτηρίων ασφαλείας και μια σημαία ενεργοποίηση/απενεργοποίηση που μπορείτε να χρησιμοποιήσετε για να απενεργοποιήσετε την πρόσβαση για μια μεμονωμένη συσκευή. Εάν η εφαρμογή σας πρέπει να αποθηκεύσετε άλλα μετα-δεδομένα ειδικά για τη συσκευή, πρέπει να χρησιμοποιεί μια συγκεκριμένη εφαρμογή store. Ανατρέξτε στον [Οδηγό για προγραμματιστές διανομέα IoT] [ lnk-devguide-identity] για περισσότερες πληροφορίες.

## <a name="receive-device-to-cloud-messages"></a>Λήψη μηνυμάτων συσκευής στο cloud

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που διαβάζει τα μηνύματα συσκευής στο cloud από το Κέντρο IoT. Ένα διανομέα IoT εκθέτει ένα [Συμβάν διανομείς][lnk-event-hubs-overview]-συμβατά τελικό σημείο για να μπορέσετε να την ανάγνωση μηνυμάτων συσκευής στο cloud. Για να διατηρήσω τα πράγματα απλά, αυτό το πρόγραμμα εκμάθησης δημιουργεί ένα βασικό πρόγραμμα ανάγνωσης που δεν είναι κατάλληλη για μια ανάπτυξη υψηλής απόδοσης. Τα [μηνύματα συσκευή-cloud διαδικασία] [ lnk-process-d2c-tutorial] πρόγραμμα εκμάθησης σας δείχνει πώς μπορείτε να επεξεργαστούν μηνύματα συσκευής στο cloud με κλίμακα. Το παράθυρο [Γρήγορα αποτελέσματα με το συμβάν διανομείς] [ lnk-eventhubs-tutorial] πρόγραμμα εκμάθησης παρέχει περαιτέρω πληροφορίες σχετικά με τον τρόπο για να επεξεργαστείτε μηνύματα από το συμβάν διανομείς και ισχύει για τα τελικά σημεία συμβατά διανομέα IoT διανομέα συμβάν.

> [AZURE.NOTE] Το τελικό σημείο συμβατό διανομέα συμβάντων για την ανάγνωση μηνυμάτων συσκευή-cloud πάντα χρησιμοποιεί το πρωτόκολλο AMQP.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **readdevicetocloudmessages**. Στο φάκελο **readdevicetocloudmessages** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```

2. Στο σας εντολών στο φάκελο **readdevicetocloudmessages** , εκτελέστε την ακόλουθη εντολή για την εγκατάσταση του πακέτου **azure-συμβάν-διανομείς** :

    ```
    npm install azure-event-hubs --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα αρχείο **ReadDeviceToCloudMessages.js** στο φάκελο **readdevicetocloudmessages** .

4. Προσθέστε τα ακόλουθα `require` προτάσεις κατά την εκκίνηση του αρχείου **ReadDeviceToCloudMessages.js** :

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Προσθέστε την ακόλουθη δήλωση μεταβλητών και αντικαταστήστε την τιμή κράτησης θέσης με τη συμβολοσειρά σύνδεσης για το Κέντρο IoT σας:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Προσθέστε τις ακόλουθες δύο συναρτήσεις που έξοδος στην κονσόλα εκτύπωσης:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Προσθέστε τον ακόλουθο κώδικα για να δημιουργήσετε το **EventHubClient**, ανοίξτε τη σύνδεση με το Κέντρο IoT σας και δημιουργήστε μια ακουστικό για κάθε διαμερίσματα. Αυτή η εφαρμογή χρησιμοποιεί ένα φίλτρο όταν δημιουργεί μια ακουστικό, έτσι ώστε το ακουστικό διαβάζει μόνο τα μηνύματα που αποστέλλονται σε διανομέα IoT μετά το ακουστικό αρχίζει να λειτουργεί. Αυτό το φίλτρο είναι χρήσιμη σε περιβάλλον δοκιμής, ώστε να βλέπετε μόνο το τρέχον σύνολο των μηνυμάτων. Σε ένα περιβάλλον παραγωγής, τον κωδικό πρέπει να βεβαιωθείτε ότι επεξεργάζεται όλα τα μηνύματα - δείτε [πώς μπορείτε να επεξεργαστούν μηνύματα συσκευή-cloud IoT διανομέα] [ lnk-process-d2c-tutorial] προγραμμάτων εκμάθησης για περισσότερες πληροφορίες:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Αποθηκεύστε και κλείστε το αρχείο **ReadDeviceToCloudMessages.js** .

## <a name="create-a-simulated-device-app"></a>Δημιουργία εφαρμογής προσομοιωμένη συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που προσομοιώνει μια συσκευή που στέλνει μηνύματα συσκευής στο cloud με ένα διανομέα IoT.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **simulateddevice**. Στο φάκελο **simulateddevice** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```

2. Στο σας εντολών στο φάκελο **simulateddevice** , εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε το πακέτο SDK συσκευή **azure-iot-συσκευή** και το πακέτο **azure-iot-συσκευή-amqp** :

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **SimulatedDevice.js** στο φάκελο **simulateddevice** .

4. Προσθέστε τα ακόλουθα `require` προτάσεις κατά την εκκίνηση του αρχείου **SimulatedDevice.js** :

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Προσθέστε μια μεταβλητή **συμβολοσειρά σύνδεσης** και χρησιμοποιήστε το για να δημιουργήσετε ένα πρόγραμμα-πελάτη συσκευή. Αντικατάσταση **{youriothostname}** με το όνομα του διανομέα IoT που δημιουργήσατε στην ενότητα *Δημιουργία ένα διανομέα IoT* . Αντικαταστήστε το **{yourdevicekey}** με την τιμή του κλειδιού συσκευή που δημιουργήσατε στην ενότητα *Δημιουργία μιας συσκευής ταυτότητας* :

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Προσθέστε την ακόλουθη συνάρτηση για να εμφανίσετε το αποτέλεσμα από την εφαρμογή:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Δημιουργία μιας επιστροφής κλήσης και χρησιμοποιήστε τη συνάρτηση **setInterval** για να στείλετε ένα νέο μήνυμα με το διανομέα IoT ανά δευτερόλεπτο:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

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

8. Ανοίξτε τη σύνδεση με το Κέντρο IoT σας και να ξεκινήσετε την αποστολή μηνυμάτων:

    ```
    client.open(connectCallback);
    ```

9. Αποθηκεύστε και κλείστε το αρχείο **SimulatedDevice.js** .

> [AZURE.NOTE] Για να διατηρήσετε πράγματα απλή, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως μια εκθετική διπλασιασμών), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων][lnk-transient-faults].


## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1. Σε μια γραμμή εντολών στο φάκελο **readdevicetocloudmessages** , εκτελέστε την ακόλουθη εντολή για να ξεκινήσετε την παρακολούθηση κέντρο IoT σας:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![Node.js διανομέα IoT υπηρεσίας εφαρμογή προγράμματος-πελάτη για την παρακολούθηση της συσκευής στο cloud μηνυμάτων][7]

2. Σε μια γραμμή εντολών στο φάκελο **simulateddevice** , εκτελέστε την ακόλουθη εντολή για να ξεκινήσει η αποστολή δεδομένα τηλεμετρίας για το Κέντρο IoT σας:

    ```
    node SimulatedDevice.js
    ```

    ![Node.js διανομέα IoT συσκευή εφαρμογή προγράμματος-πελάτη για την αποστολή μηνυμάτων συσκευής στο cloud][8]

3. Το πλακίδιο **Χρήση** στην [πύλη του Azure] [ lnk-portal] εμφανίζει τον αριθμό των μηνυμάτων που αποστέλλονται σε την ενότητα:

    ![Azure πύλης χρήση πλακιδίων που εμφανίζει αριθμό των μηνυμάτων που αποστέλλονται σε διανομέα IoT][43]

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, που έχει ρυθμιστεί διανομέα IoT στην πύλη του Azure και, στη συνέχεια, να δημιουργήσει μια ταυτότητα συσκευής στο μητρώο ταυτότητας την ενότητα. Χρησιμοποιείται αυτή η ταυτότητα συσκευή για να ενεργοποιήσετε την εφαρμογή προσομοιωμένη συσκευή για την αποστολή μηνυμάτων συσκευής στο cloud για την ενότητα. Μπορείτε επίσης να δημιουργήσει μια εφαρμογή που εμφανίζει τα μηνύματα που ελήφθησαν από την ενότητα. 

Για να συνεχίσετε γρήγορα αποτελέσματα με το Κέντρο IoT και να εξερευνήσετε τα άλλα σενάρια IoT, ανατρέξτε στα θέματα:

- [Συνδέει τη συσκευή σας][lnk-connect-device]
- [Γρήγορα αποτελέσματα με τη Διαχείριση συσκευών][lnk-device-management]
- [Γρήγορα αποτελέσματα με το SDK IoT πύλης][lnk-gateway-SDK]

Για να μάθετε πώς μπορείτε να επεκτείνετε τη λύση σας IoT και επεξεργασία των μηνυμάτων συσκευής στο cloud με κλίμακα, δείτε τα [μηνύματα συσκευή-cloud διαδικασία] [ lnk-process-d2c-tutorial] το πρόγραμμα εκμάθησης.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
