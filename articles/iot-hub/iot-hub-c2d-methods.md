<properties
 pageTitle="Χρησιμοποιήστε απευθείας μεθόδους | Microsoft Azure"
 description="Αυτό το πρόγραμμα εκμάθησης σας δείχνει πώς μπορείτε να χρησιμοποιήσετε απευθείας μεθόδους"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>Πρόγραμμα εκμάθησης: Χρησιμοποιήστε την άμεση μεθόδους

## <a name="introduction"></a>Εισαγωγή

Azure IoT διανομέα είναι μια πλήρως διαχειριζόμενη υπηρεσία που επιτρέπει την αξιόπιστη και ασφαλής διπλής κατεύθυνσης επικοινωνιών μεταξύ εκατομμύρια IoT συσκευές και μια εφαρμογή, δημιουργήστε αντίγραφα τέλος. Προηγούμενη εκμάθηση ([Γρήγορα αποτελέσματα με το Κέντρο IoT] και [Αποστολή μηνυμάτων Cloud-σε-συσκευή με το Κέντρο IoT]) απεικόνιση τη βασική συσκευή στο cloud και cloud-σε-συσκευή ανταλλαγής μηνυμάτων λειτουργικότητα IoT διανομέα. Ενότητα IoT παρέχει επίσης τη δυνατότητα να καλέσετε μεθόδους μη διαρκή σε συσκευές από το cloud. Μέθοδοι αντιπροσωπεύει μια αίτηση απάντησης επικοινωνία με μια συσκευή που είναι παρόμοια με μια κλήση HTTP σε που να επιτύχει ή αποτύχει αμέσως (μετά από ένα χρονικό όριο που καθορίζεται από το χρήστη) για να επιτρέψετε στο χρήστη να γνωρίζει την κατάσταση της κλήσης. [Κλήση μιας μεθόδου απευθείας σε μια συσκευή] [ lnk-devguide-methods] περιγράφει μεθόδους με περισσότερες λεπτομέρειες και παρέχει οδηγίες σχετικά με το πότε να χρησιμοποιήσετε μεθόδους έναντι cloud-σε-συσκευή μηνύματα.

Αυτό το πρόγραμμα εκμάθησης σας δείχνει τον τρόπο για να:

- Χρησιμοποιήστε την πύλη του Azure για να δημιουργήσετε ένα διανομέα IoT και να δημιουργήσετε μια ταυτότητα συσκευής στο κέντρο IoT σας.
- Δημιουργήστε μια πρόχειρη συσκευή που έχει μια άμεση μέθοδος που μπορεί να καλείται από το cloud.
- Δημιουργία μιας εφαρμογής κονσόλας που καλεί μια μέθοδο απευθείας στη συσκευή προσομοιωμένη μέσω κέντρο IoT σας.

> [AZURE.NOTE] Προς το παρόν, άμεσες μεθόδους είναι προσβάσιμα μόνο από συσκευών που συνδέονται με χρήση του πρωτοκόλλου MQTT διανομέα IoT. Ανατρέξτε για την [υποστήριξη MQTT] [ lnk-devguide-mqtt] το άρθρο για οδηγίες σχετικά με τον τρόπο για να μετατρέψετε την υπάρχουσα εφαρμογή συσκευή για να χρησιμοποιήσετε MQTT.

Στο τέλος αυτού του προγράμματος εκμάθησης, έχετε δύο Node.js κονσόλα εφαρμογών:

* **CallMethodOnDevice.js**, το οποίο καλεί μια μέθοδο στη συσκευή προσομοιωμένη και εμφανίζει την απάντηση.
* **SimulatedDevice.js**, το οποίο συνδέεται με το Κέντρο IoT σας με την ταυτότητα της συσκευής που δημιουργήσατε νωρίτερα και αποκρίνεται σε τη μέθοδο που ονομάζεται από το cloud.

> [AZURE.NOTE] Το άρθρο [SDK διανομέα IoT] [ lnk-hub-sdks] παρέχει πληροφορίες σχετικά με τα διάφορα SDK που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε δύο εφαρμογές για να εκτελέσετε σε συσκευές και το τέλος πίσω λύση.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

+ Έκδοση node.js 0.10.x ή νεότερη έκδοση.

+ Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Δημιουργία εφαρμογής προσομοιωμένη συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που αποκρίνεται σε μια μέθοδο που ονομάζεται από το cloud.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **simulateddevice**. Στο φάκελο **simulateddevice** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```

2. Στο σας εντολών στο φάκελο **simulateddevice** , εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε το πακέτο SDK συσκευή **azure-iot-συσκευή** και το πακέτο **azure-iot-συσκευή-mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **SimulatedDevice.js** στο φάκελο **simulateddevice** .

4. Προσθέστε τα ακόλουθα `require` προτάσεις κατά την εκκίνηση του αρχείου **SimulatedDevice.js** :

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. Προσθέστε μια μεταβλητή **συμβολοσειρά σύνδεσης** και χρησιμοποιήστε το για να δημιουργήσετε ένα πρόγραμμα-πελάτη συσκευή. Αντικαταστήστε το **{συμβολοσειρά σύνδεσης συσκευή}** με τη συμβολοσειρά σύνδεσης που δημιουργήσατε στην ενότητα *Δημιουργία μιας συσκευής ταυτότητας* :

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. Προσθέστε την ακόλουθη συνάρτηση για την υλοποίηση της μεθόδου στη συσκευή:

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. Άνοιγμα της σύνδεσης στην ενότητα IoT και έναρξη προετοιμασία το ακροατήριο μέθοδο:

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. Αποθηκεύστε και κλείστε το αρχείο **SimulatedDevice.js** .

> [AZURE.NOTE] Για να διατηρήσετε πράγματα απλή, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως σύνδεσης "Επανάληψη"), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων][lnk-transient-faults].

## <a name="call-a-method-on-a-device"></a>Κλήση μιας μεθόδου σε μια συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που καλεί μια μέθοδο στην προσομοιωμένη συσκευή και, στη συνέχεια, εμφανίζει την απάντηση.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **callmethodondevice**. Στο φάκελο **callmethodondevice** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```

2. Στο σας εντολών στο φάκελο **callmethodondevice** , εκτελέστε την ακόλουθη εντολή για την εγκατάσταση του πακέτου **azure iothub** :

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα αρχείο **CallMethodOnDevice.js** στο φάκελο **callmethodondevice** .

4. Προσθέστε τα ακόλουθα `require` προτάσεις κατά την εκκίνηση του αρχείου **CallMethodOnDevice.js** :

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Προσθέστε την ακόλουθη δήλωση μεταβλητών και αντικαταστήστε την τιμή κράτησης θέσης με τη συμβολοσειρά σύνδεσης για το Κέντρο IoT σας:

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. Δημιουργήστε το πρόγραμμα-πελάτη για να ανοίξετε τη σύνδεση με το Κέντρο IoT σας.

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. Προσθέστε την ακόλουθη συνάρτηση για να καλέσετε τη μέθοδο συσκευή και να εκτυπώσετε η απόκριση της συσκευής στην κονσόλα:

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. Αποθηκεύστε και κλείστε το αρχείο **CallMethodOnDevice.js** .

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1. Σε μια γραμμή εντολών στο φάκελο **simulateddevice** , εκτελέστε την ακόλουθη εντολή για να ξεκινήσετε listening για τη μέθοδο κλήσεις από το Κέντρο IoT σας:

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. Σε μια γραμμή εντολών στο φάκελο **callmethodondevice** , εκτελέστε την ακόλουθη εντολή για να ξεκινήσετε την παρακολούθηση κέντρο IoT σας:

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. Θα δείτε τη συσκευή με αντικείμενο τη μέθοδο, εκτυπώνοντάς τις στο μήνυμα και την εφαρμογή που ονομάζεται την εμφάνιση μέθοδο απάντηση από τη συσκευή:

    ![][9]
    
## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, που έχει ρυθμιστεί διανομέα IoT στην πύλη του Azure και, στη συνέχεια, να δημιουργήσει μια ταυτότητα συσκευής στο μητρώο ταυτότητας την ενότητα. Χρησιμοποιείται αυτή η ταυτότητα συσκευή για να ενεργοποιήσετε την εφαρμογή προσομοιωμένη συσκευή προκειμένου να αντιμετωπίζεται μεθόδους που χρησιμοποιούνται από το cloud. Μπορείτε επίσης να δημιουργήσει μια εφαρμογή που καλεί μεθόδους στη συσκευή και εμφανίζει την απάντηση από τη συσκευή. 

Για να συνεχίσετε γρήγορα αποτελέσματα με το Κέντρο IoT και να εξερευνήσετε τα άλλα σενάρια IoT, ανατρέξτε στα θέματα:

- [Γρήγορα αποτελέσματα με το Κέντρο IoT]
- [Προγραμματισμός εργασιών σε πολλές συσκευές][lnk-devguide-jobs]

Για να μάθετε πώς μπορείτε να επεκτείνετε το IoT λύση και χρονοδιάγραμμα μέθοδο κλήσεις σε πολλές συσκευές, ανατρέξτε στο θέμα το [Χρονοδιάγραμμα και εκπομπής εργασιών] [ lnk-tutorial-jobs] το πρόγραμμα εκμάθησης.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Αποστολή μηνυμάτων Cloud-σε-συσκευή με το Κέντρο IoT]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Γρήγορα αποτελέσματα με το Κέντρο IoT]: iot-hub-node-node-getstarted.md
