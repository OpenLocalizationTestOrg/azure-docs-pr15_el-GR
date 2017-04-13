<properties
 pageTitle="Γρήγορα αποτελέσματα με τη Διαχείριση συσκευών | Microsoft Azure"
 description="Αυτό το πρόγραμμα εκμάθησης σας δείχνει πώς μπορείτε να ξεκινήσετε με τη Διαχείριση συσκευών στην ενότητα IoT Azure"
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

# <a name="tutorial-get-started-with-device-management-preview"></a>Πρόγραμμα εκμάθησης: Γρήγορα αποτελέσματα με τη Διαχείριση Συσκευών (έκδοση preview)

## <a name="introduction"></a>Εισαγωγή
IoT cloud εφαρμογές μπορούν να χρησιμοποιήσουν στοιχειώδεις τύπους στην ενότητα IoT Azure, δηλαδή η συσκευή διπλού και απευθείας μεθόδους, για να ξεκινήσετε από απόσταση και να παρακολουθούν ενέργειες διαχείρισης συσκευών σε συσκευές.  Σε αυτό το άρθρο παρέχει οδηγίες και τον κωδικό για το πώς λειτουργεί η εφαρμογή cloud IoT και μια συσκευή για να ξεκινήσουν και να παρακολουθούν μια απομακρυσμένη συσκευή επανεκκινήστε τον υπολογιστή, χρησιμοποιώντας το Κέντρο IoT.

Για να ξεκινήσετε από απόσταση και να παρακολουθούν ενέργειες διαχείρισης συσκευών στις συσκευές σας από μια εφαρμογή που βασίζεται στο cloud, παρασκηνίου, χρησιμοποιήστε στοιχειώδεις τύπους διανομέα IoT Azure όπως [διπλού συσκευή] [ lnk-devtwin] και [cloud-σε-συσκευή μεθόδους (C2D)][lnk-c2dmethod]. Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μια εφαρμογή παρασκηνίου και μιας συσκευής που μπορούν να συνεργαστούν για να σας επιτρέψουν προετοιμασία και παρακολούθηση της απομακρυσμένης συσκευή επανεκκίνηση από διανομέα IoT.

Μπορείτε να χρησιμοποιήσετε μια μέθοδο C2D για να ξεκινήσετε δραστηριότητες διαχείρισης συσκευών (όπως επανεκκινήστε τον υπολογιστή, επαναφορά εργοστασίου και ενημέρωση του υλικολογισμικού) από μια εφαρμογή παρασκηνίου στο cloud. Η συσκευή είναι υπεύθυνος για:

- Χειρισμός η αίτηση μέθοδο που στέλνεται από διανομέα IoT.
- Προετοιμασία την αντίστοιχη ενέργεια συγκεκριμένη συσκευή στη συσκευή.
- Παροχή ενημερώσεις κατάστασης με τη συσκευή διπλού αναφερθεί ιδιότητες IoT διανομέα.

Μπορείτε να χρησιμοποιήσετε μια εφαρμογή παρασκηνίου στο cloud για εκτέλεση ερωτημάτων διπλού συσκευή για να αναφέρετε την πρόοδο των ενεργειών σας διαχείρισης συσκευών.

Αυτό το πρόγραμμα εκμάθησης σας δείχνει τον τρόπο για να:

- Χρησιμοποιήστε την πύλη του Azure για να δημιουργήσετε ένα διανομέα IoT και να δημιουργήσετε μια ταυτότητα συσκευής στο κέντρο IoT σας.
- Δημιουργήστε μια πρόχειρη συσκευή που έχει μια άμεση μέθοδος που σας επιτρέπει επανεκκινήστε τον υπολογιστή που μπορεί να καλείται από το cloud.
- Δημιουργία μιας εφαρμογής κονσόλας που καλεί τη μέθοδο απευθείας επανεκκινήστε τον υπολογιστή στη συσκευή προσομοιωμένη μέσω κέντρο IoT σας.

Στο τέλος αυτού του προγράμματος εκμάθησης, έχετε δύο Node.js κονσόλα εφαρμογών:

**dmpatterns_getstarted_device.js**, που συνδέεται με το Κέντρο IoT σας με την ταυτότητα της συσκευής που δημιουργήσατε νωρίτερα, λαμβάνει μια άμεση μέθοδος επανεκκινήστε τον υπολογιστή, προσομοιώνει φυσικής επανεκκίνηση και αναφέρει την ώρα για την τελευταία επανεκκίνηση.

**dmpatterns_getstarted_service.js**, που καλεί μια μέθοδο απευθείας στη συσκευή προσομοιωμένη, εμφανίζει την απάντηση και εμφανίζει το ενημερωμένο συσκευή διπλού αναφερθεί ιδιότητες.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

Έκδοση node.js 0.12.x ή νεότερη έκδοση, <br/>  [Προετοιμασία περιβάλλον ανάπτυξής σας] [ lnk-dev-setup] περιγράφει τον τρόπο εγκατάστασης Node.js για αυτό το πρόγραμμα εκμάθησης στα Windows ή Linux.

Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Δημιουργία εφαρμογής προσομοιωμένη συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που αποκρίνεται σε μια άμεση μέθοδο που ονομάζεται από το cloud, η οποία ενεργοποιεί μια συσκευή προσομοιωμένη επανεκκίνηση και χρησιμοποιεί τη συσκευή διπλού αναφερθεί ιδιότητες, για να ενεργοποιήσετε τα ερωτήματα διπλού συσκευή για να προσδιορίσετε συσκευές και όταν αυτά τελευταία επανεκκίνηση.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **manageddevice**.  Στο φάκελο **manageddevice** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών.  Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```
    
2. Στο σας εντολών στο φάκελο **manageddevice** , εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε το **azure-iot-device@dtpreview** πακέτο SDK συσκευή και **azure-iot-device-mqtt@dtpreview** πακέτου:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **dmpatterns_getstarted_device.js** στο φάκελο **manageddevice** .

4. Προσθέστε τα εξής 'απαίτηση' προτάσεις κατά την εκκίνηση του αρχείου **dmpatterns_getstarted_device.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Προσθέστε μια μεταβλητή **συμβολοσειρά σύνδεσης** και χρησιμοποιήστε το για να δημιουργήσετε ένα πρόγραμμα-πελάτη συσκευή.  Αντικαταστήστε τη συμβολοσειρά σύνδεσης με τη συμβολοσειρά σύνδεσης τη συσκευή σας.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Προσθέστε την ακόλουθη συνάρτηση για την υλοποίηση της μεθόδου απευθείας από τη συσκευή

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. Ανοίξτε τη σύνδεση με το Κέντρο IoT σας και να ξεκινήσετε την παρακολούθηση άμεση μέθοδος:

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. Αποθηκεύστε και κλείστε το αρχείο **dmpatterns_getstarted_device.js** .

 [AZURE.NOTE] Για να διατηρήσετε πράγματα απλή, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να υλοποιήσετε πολιτικές "Επανάληψη" (όπως μια εκθετική διπλασιασμών), όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων][lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Έναυσμα απομακρυσμένη επανεκκίνηση της συσκευής τη μέθοδο του απευθείας 

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που προετοιμάζει απομακρυσμένης εκκίνησης σε συσκευή με τη μέθοδο του απευθείας και χρησιμοποιεί ερωτήματα διπλού συσκευή για να βρείτε την τελευταία φορά επανεκκινήστε τον υπολογιστή για τη συγκεκριμένη συσκευή.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **triggerrebootondevice**.  Στο φάκελο **triggerrebootondevice** , δημιουργήστε ένα αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών.  Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```
    
2. Στο σας εντολών στο φάκελο **triggerrebootondevice** , εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε το **azure-iothub@dtpreview** πακέτο SDK συσκευή και **azure-iot-device-mqtt@dtpreview** πακέτου:

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **dmpatterns_getstarted_service.js** στο φάκελο **triggerrebootondevice** .

4. Προσθέστε τα εξής 'απαίτηση' προτάσεις κατά την εκκίνηση του αρχείου **dmpatterns_getstarted_service.js** :

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Προσθέστε τις ακόλουθες δηλώσεις μεταβλητών και αντικαταστήστε τις τιμές του πλαισίου κράτησης θέσης:

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. Προσθέστε την ακόλουθη συνάρτηση για να καλέσετε τη μέθοδο συσκευή για να κάνετε επανεκκίνηση της συσκευής προορισμού:

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Προσθέστε την ακόλουθη συνάρτηση για να υποβάλετε ένα ερώτημα για τη συσκευή και να λάβετε την τελευταία φορά επανεκκινήστε τον υπολογιστή:

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. Προσθέστε τον παρακάτω κώδικα για να καλέσετε τις συναρτήσεις που θα ενεργοποιεί τη μέθοδο απευθείας επανεκκινήστε τον υπολογιστή και ερωτήματος για τελευταία φορά επανεκκινήστε τον υπολογιστή:

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. Αποθηκεύστε και κλείστε το αρχείο **dmpatterns_getstarted_service.js** .

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1. Στη γραμμή-εντολών στο φάκελο **manageddevice** , εκτελέστε την ακόλουθη εντολή για να ξεκινήσει η ακρόαση για την άμεση μέθοδος επανεκκινήστε τον υπολογιστή.

    ```
    node dmpatterns_getstarted_device.js
    ```

2. Στη γραμμή εντολών εντολή στο φάκελο **triggerrebootondevice** , εκτελέστε την ακόλουθη εντολή έναυσμα η απομακρυσμένη επανεκκινήστε τον υπολογιστή και ερωτήματος για τη συσκευή διπλού για να βρείτε την τελευταία επανεκκίνηση της ώρας.

    ```
    node dmpatterns_getstarted_service.js
    ```

3. Θα δείτε το αντιδρούν τη μέθοδο απευθείας από την εκτύπωση του μηνύματος

## <a name="customize-and-extend-the-device-management-actions"></a>Προσαρμογή και την επέκταση της συσκευής ενέργειες διαχείρισης

Σας λύσεις IoT μπορεί να αναπτύξετε το καθορισμένο σύνολο μοτίβων διαχείρισης συσκευών ή να ενεργοποιήσετε προσαρμοσμένα μοτίβα χρησιμοποιώντας τη συσκευή διπλού και στοιχειώδεις τύπους μέθοδο C2D. Άλλες δραστηριότητες διαχείρισης συσκευών παραδείγματα επαναφορά εργοστασίου, ενημερωμένη έκδοση υλικολογισμικού, ενημερωμένη έκδοση λογισμικού, διαχείριση ενέργειας, δικτύου και συνδεσιμότητας διαχείρισης και κρυπτογράφηση δεδομένων.

## <a name="device-maintenance-windows"></a>Συσκευή windows συντήρηση

Συνήθως, μπορείτε να ρυθμίσετε τις συσκευές για την εκτέλεση ενεργειών κάθε φορά που ελαχιστοποιεί τις διακοπές και χρόνου εκτός λειτουργίας.  Συσκευή windows συντήρησης είναι ένα μοτίβο που χρησιμοποιείται συχνά για να ορίσετε την ώρα όταν μια συσκευή πρέπει να ενημερώσετε τις ρυθμίσεις παραμέτρων. Σας λύσεις παρασκηνίου μπορούν να χρησιμοποιούν τις επιθυμητές ιδιότητες του διπλού τη συσκευή για να ορίσετε και να ενεργοποιήσετε μια πολιτική στη συσκευή σας που επιτρέπει σε ένα παράθυρο Συντήρηση. Όταν μια συσκευή λαμβάνει την πολιτική παράθυρο συντήρησης, το να χρησιμοποιήσετε την ιδιότητα αναφερόμενου του διπλού τη συσκευή για να αναφέρετε την κατάσταση της πολιτικής. Η εφαρμογή παρασκηνίου, στη συνέχεια, να χρησιμοποιήσετε συσκευή διπλού ερωτημάτων για να πιστοποιούν συμμόρφωσης συσκευές και κάθε πολιτικής.

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, που χρησιμοποιήσατε μια άμεση μέθοδος για την ενεργοποίηση της απομακρυσμένης εκκίνησης σε μια συσκευή, χρησιμοποιείται το διπλού συσκευή αναφερθεί ιδιότητες, για να αναφέρετε την τελευταία φορά επανεκκινήστε τον υπολογιστή από τη συσκευή και ερώτημα με την διπλού συσκευή για να ανακαλύψετε την τελευταία φορά επανεκκίνηση της συσκευής από το cloud.

Για να συνεχίσετε γρήγορα αποτελέσματα με το διανομέα IoT και μοτίβα διαχείρισης συσκευών όπως remote πάνω από την ενημέρωση του υλικολογισμικού αέρα, ανατρέξτε στα θέματα:

[Πρόγραμμα εκμάθησης: Πώς μπορείτε να κάνετε μια ενημερωμένη έκδοση υλικολογισμικού][lnk-fwupdate]

Για να μάθετε πώς μπορείτε να επεκτείνετε το IoT λύση και χρονοδιάγραμμα μέθοδο κλήσεις σε πολλές συσκευές, ανατρέξτε στο θέμα το [Χρονοδιάγραμμα και εκπομπής εργασιών] [ lnk-tutorial-jobs] το πρόγραμμα εκμάθησης.

Για να συνεχίσετε γρήγορα αποτελέσματα με το Κέντρο IoT, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το SDK πύλης IoT][lnk-gateway-SDK].


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx