<properties
 pageTitle="Πώς να προγραμματίζουν εργασίες | Microsoft Azure"
 description="Θα μάθετε πώς να προγραμματίζουν εργασίες με αυτό το πρόγραμμα εκμάθησης"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-schedule-and-broadcast-jobs-preview"></a>Πρόγραμμα εκμάθησης: Προγραμματισμός και εκπομπή εργασίες (έκδοση preview)

## <a name="introduction"></a>Εισαγωγή
Azure IoT διανομέα είναι μια πλήρως διαχειριζόμενη υπηρεσία που επιτρέπει σε μια εφαρμογή πίσω end για να δημιουργείτε και να παρακολουθείτε εργασίες που Προγραμματισμός και ενημέρωση εκατομμύρια συσκευές.  Εργασίες μπορούν να χρησιμοποιηθούν για τις ακόλουθες ενέργειες:

- Ενημέρωση των ιδιοτήτων διπλού επιθυμητοί συσκευή
- Ενημέρωση ετικετών διπλού συσκευή
- Κλήση μεθόδους cloud-σε-συσκευή

Εννοιολογικά, μια εργασία αναδιπλώνεται μία από αυτές τις ενέργειες και παρακολουθεί την πρόοδο της εκτέλεσης σε σχέση με ένα σύνολο των συσκευών, που ορίζεται από ένα ερώτημα διπλού.  Για παράδειγμα, χρησιμοποιώντας μια εργασία μια εφαρμογή παρασκηνίου να ενεργοποιήσετε μια μέθοδο επανεκκινήστε τον υπολογιστή σε 10.000 συσκευές, που καθορίζονται από ένα ερώτημα διπλού και έχει προγραμματιστεί στο μέλλον.  Αυτήν την εφαρμογή, στη συνέχεια, να παρακολουθείτε την πρόοδο καθώς κάθε μία από αυτές τις συσκευές λήψη και εκτέλεση της μεθόδου επανεκκινήστε τον υπολογιστή.

Μάθετε περισσότερα σχετικά με κάθε μία από αυτές τις δυνατότητες σε αυτά τα άρθρα:

- Συσκευή διπλού και τις ιδιότητες: [Γρήγορα αποτελέσματα με το διπλού] [ lnk-get-started-twin] και [πρόγραμμα εκμάθησης: πώς μπορείτε να χρησιμοποιήσετε τις ιδιότητες διπλού][lnk-twin-props]
- Μέθοδοι cloud-σε-συσκευή: [Οδηγός για προγραμματιστές - άμεσες μεθόδους] [ lnk-dev-methods] και [πρόγραμμα εκμάθησης: μεθόδους C2D][lnk-c2d-methods]

Αυτό το πρόγραμμα εκμάθησης σας δείχνει τον τρόπο για να:

- Δημιουργήστε μια πρόχειρη συσκευή που έχει μια άμεση μέθοδος που σας επιτρέπει lockDoor οποίο μπορεί να ονομάζεται από την εφαρμογή ξανά τέλος.
- Δημιουργία μιας εφαρμογής κονσόλας που καλεί τη μέθοδο lockDoor απευθείας στη συσκευή προσομοιωμένη χρησιμοποιώντας μια εργασία και ενημερώνει τις ιδιότητες διπλού επιθυμητοί χρησιμοποιώντας μια συσκευή εργασία.

Στο τέλος αυτού του προγράμματος εκμάθησης, έχετε δύο Node.js κονσόλα εφαρμογών:

**simDevice.js**, το οποίο συνδέεται με το Κέντρο IoT σας με τη συσκευή ταυτότητα και λαμβάνει μια άμεση μέθοδος lockDoor.

**scheduleJobService.js**, το οποίο καλεί μια μέθοδο απευθείας στη συσκευή προσομοιωμένη και ενημερώστε το διπλού disired ιδιότητες χρησιμοποιώντας μια εργασία.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

Έκδοση node.js 0.12.x ή νεότερη έκδοση, <br/>  [Προετοιμασία περιβάλλον ανάπτυξής σας] [ lnk-dev-setup] περιγράφει τον τρόπο εγκατάστασης Node.js για αυτό το πρόγραμμα εκμάθησης στα Windows ή Linux.

Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Δημιουργία εφαρμογής προσομοιωμένη συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που αποκρίνεται σε μια άμεση μέθοδο που ονομάζεται από το cloud, η οποία ενεργοποιεί μια συσκευή προσομοιωμένη επανεκκίνηση και χρησιμοποιεί τη συσκευή διπλού αναφερθεί ιδιότητες, για να ενεργοποιήσετε τα ερωτήματα διπλού συσκευή για να προσδιορίσετε συσκευές και όταν αυτά τελευταία επανεκκίνηση.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **simDevice**.  Στο φάκελο **simDevice** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών.  Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```
    
2. Στο σας εντολών στο φάκελο **simDevice** , εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε το πακέτο SDK συσκευή **azure-iot-συσκευή** και το πακέτο **azure-iot-συσκευή-mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **simDevice.js** στο φάκελο **simDevice** .

4. Προσθέστε τα εξής 'απαίτηση' προτάσεις κατά την εκκίνηση του αρχείου **simDevice.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Προσθέστε μια μεταβλητή **συμβολοσειρά σύνδεσης** και χρησιμοποιήστε το για να δημιουργήσετε ένα πρόγραμμα-πελάτη συσκευή.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Προσθέστε την ακόλουθη συνάρτηση χειρισμού της μεθόδου lockDoor.

    ```
    var onLockDoor = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        console.log('Locking Door!');
    };
    ```

7. Προσθέστε τον ακόλουθο κώδικα για την καταχώρηση του προγράμματος χειρισμού για τη μέθοδο lockDoor.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for reboot direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
    
8. Αποθηκεύστε και κλείστε το αρχείο **simDevice.js** .

> [AZURE.NOTE] Για να διατηρήσετε πράγματα απλή, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως μια εκθετική διπλασιασμών), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων][lnk-transient-faults].

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-twins-properties"></a>Προγραμματίζουν εργασίες για κλήση μιας μεθόδου απευθείας και την ενημέρωση ενός διπλού ιδιότητες

Σε αυτήν την ενότητα, δημιουργείτε μια εφαρμογή κονσόλας Node.js που ξεκινά μια απομακρυσμένη lockDoor σε συσκευή με τη μέθοδο του απευθείας και ενημέρωση των ιδιοτήτων του διπλού.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **scheduleJobService**.  Στο φάκελο **scheduleJobService** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών.  Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```
    
2. Στο σας εντολών στο φάκελο **scheduleJobService** , εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε το πακέτο SDK συσκευή **azure iothub** και πακέτο **azure-iot-συσκευή-mqtt** :

    ```
    npm install azure-iothub@dtpreview uuid --save
    ```
    
3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **scheduleJobService.js** στο φάκελο **scheduleJobService** .

4. Προσθέστε τα εξής 'απαίτηση' προτάσεις κατά την εκκίνηση του αρχείου **dmpatterns_gscheduleJobServiceetstarted_service.js** :

    ```
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Προσθέστε τις ακόλουθες δηλώσεις μεταβλητών και αντικαταστήστε τις τιμές του πλαισίου κράτησης θέσης:

    ```
    var connectionString = '{iothubconnectionstring}';
    var deviceArray = ['myDeviceId'];
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
    
6. Προσθέστε την ακόλουθη συνάρτηση που θα χρησιμοποιηθεί για την παρακολούθηση της εκτέλεσης της εργασίας:

    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Προσθέστε τον ακόλουθο κώδικα για τον προγραμματισμό της εργασίας που καλεί τη μέθοδο συσκευή:

    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        timeoutInSeconds: 45
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                deviceArray,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
        
8. Προσθέστε τον ακόλουθο κώδικα για τον προγραμματισμό της εργασίας για να ενημερώσετε το διπλού:

    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                deviceArray,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
    
9. Αποθηκεύστε και κλείστε το αρχείο **scheduleJobService.js** .

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1. Στη γραμμή-εντολών στο φάκελο **simDevice** , εκτελέστε την ακόλουθη εντολή για να ξεκινήσει η ακρόαση για την άμεση μέθοδος επανεκκινήστε τον υπολογιστή.

    ```
    node simDevice.js
    ```

2. Στη γραμμή εντολών εντολή στο φάκελο **scheduleJobService** , εκτελέστε την ακόλουθη εντολή έναυσμα η απομακρυσμένη επανεκκινήστε τον υπολογιστή και ερωτήματος για τη συσκευή διπλού για να βρείτε την τελευταία επανεκκίνηση της ώρας.

    ```
    node scheduleJobService.js
    ```

3. Θα δείτε την έξοδο από τη συσκευή και εφαρμογές παρασκηνίου.


## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, χρησιμοποιούσατε μια εργασία για να προγραμματίσετε μια μέθοδο απευθείας σε μια συσκευή και την ενημέρωση των ιδιοτήτων του διπλού τη συσκευή.

Για να συνεχίσετε γρήγορα αποτελέσματα με το διανομέα IoT και μοτίβα διαχείρισης συσκευών όπως remote πάνω από την ενημέρωση του υλικολογισμικού αέρα, ανατρέξτε στα θέματα:

[Πρόγραμμα εκμάθησης: Πώς μπορείτε να κάνετε μια ενημερωμένη έκδοση υλικολογισμικού][lnk-fwupdate]

Για να συνεχίσετε γρήγορα αποτελέσματα με το Κέντρο IoT, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το SDK πύλης IoT][lnk-gateway-SDK].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-firmware-update.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx