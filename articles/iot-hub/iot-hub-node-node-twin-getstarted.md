<properties
    pageTitle="Γρήγορα αποτελέσματα με το twins | Microsoft Azure"
    description="Αυτό το πρόγραμμα εκμάθησης σας δείχνει πώς μπορείτε να χρησιμοποιήσετε twins"
    services="iot-hub"
    documentationCenter="node"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-get-started-with-device-twins-preview"></a>Πρόγραμμα εκμάθησης: Γρήγορα αποτελέσματα με τη συσκευή twins (έκδοση preview)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Στο τέλος αυτού του προγράμματος εκμάθησης, θα έχετε δύο Node.js κονσόλα εφαρμογών:

* **AddTagsAndQuery.js**, μια εφαρμογή Node.js σκοπεύατε να να εκτελεστεί από παρασκηνίου, το οποίο προσθέτει ετικέτες και τα ερωτήματα twins συσκευή.
* **TwinSimulatedDevice.js**, μια εφαρμογή Node.js που προσομοιώνει μια συσκευή που συνδέεται με το Κέντρο IoT σας με την ταυτότητα της συσκευής που δημιουργήσατε νωρίτερα, τις αναφορές και της κατάστασης σύνδεσης.

> [AZURE.NOTE] Το άρθρο [SDK διανομέα IoT] [ lnk-hub-sdks] παρέχει πληροφορίες σχετικά με τα διάφορα SDK που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε εφαρμογές συσκευής και παρασκηνίου.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης χρειάζεστε τα εξής:

+ Έκδοση node.js 0.10.x ή νεότερη έκδοση.

+ Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Δημιουργία της εφαρμογής υπηρεσίας

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που προσθέτει το διπλού που σχετίζονται με **myDeviceId**θέση μετα-δεδομένα. Το, στη συνέχεια, τα ερωτήματα του twins που είναι αποθηκευμένα στην ενότητα επιλογή τις συσκευές που βρίσκονται σε Ηνωμένες Πολιτείες και, στη συνέχεια, αυτές που αναφοράς σύνδεσης κινητής τηλεφωνίας.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **addtagsandqueryapp**. Στο φάκελο **addtagsandqueryapp** , δημιουργήστε ένα νέο αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```

2. Στο σας εντολών στο φάκελο **addtagsandqueryapp** , εκτελέστε την ακόλουθη εντολή για την εγκατάσταση του πακέτου **azure iothub** :

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **AddTagsAndQuery.js** στο φάκελο **addtagsandqueryapp** .

4. Προσθέστε τον ακόλουθο κώδικα στο αρχείο **AddTagsAndQuery.js** και αντικαταστήστε το σύμβολο κράτησης θέσης **{συμβολοσειρά σύνδεσης υπηρεσίας}** με τη συμβολοσειρά σύνδεσης που αντιγράψατε όταν δημιουργήσατε το Κέντρο σας:

        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{service hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
             
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });

    Το αντικείμενο **μητρώου** εκθέτει όλες τις μεθόδους που απαιτούνται για να αλληλεπιδράσετε με συσκευή twins από την υπηρεσία. Τον προηγούμενο κώδικα προετοιμάζει πρώτα το αντικείμενο **μητρώου** και, στη συνέχεια, ανακτά την διπλού για **myDeviceId**και τέλος ενημερώνει τις ετικέτες με τις πληροφορίες στη θέση που θέλετε.

    Μετά την ενημέρωση των ετικετών καλεί τη συνάρτηση **queryTwins** .

7. Προσθέστε τον ακόλουθο κώδικα στο τέλος της **AddTagsAndQuery.js** για την υλοποίηση της συνάρτησης **queryTwins** :

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
            
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };

    Τον προηγούμενο κώδικα εκτελεί δύο ερωτήματα: μόνο η συσκευή twins συσκευών που βρίσκεται στη μονάδα **Redmond43** επιλέγει το πρώτο και το δεύτερο έχει βελτιώσει το ερώτημα για να επιλέξετε μόνο τις συσκευές που συνδέονται μέσω του δικτύου κινητής τηλεφωνίας.

    Σημειώστε ότι το προηγούμενο κωδικό, όταν δημιουργεί το αντικείμενο **ερωτήματος** , καθορίζει το μέγιστο αριθμό των εγγράφων που επιστρέφεται. Το αντικείμενο **ερωτήματος** περιέχει μια δυαδική ιδιότητα **hasMoreResults** που μπορείτε να χρησιμοποιήσετε για να καλέσετε τις μεθόδους **nextAsTwin** πολλές φορές για την ανάκτηση όλων των αποτελεσμάτων. Μια μέθοδο που ονομάζεται **επόμενη** είναι διαθέσιμη για τα αποτελέσματα που δεν είναι συσκευή twins για παράδειγμα, αποτελέσματα ερωτημάτων συνάθροισης.

8. Εκτελέστε την εφαρμογή με:

        node AddTagsAndQuery.js

    Θα πρέπει να βλέπετε μία συσκευή στα αποτελέσματα για το ερώτημα που σας ρωτά για όλες τις συσκευές που βρίσκεται στο **Redmond43** και καμία για το ερώτημα που περιορίζει τα αποτελέσματα σε συσκευές που χρησιμοποιούν το δίκτυο κινητής τηλεφωνίας.

    ![][1]

Στην επόμενη ενότητα μπορείτε να δημιουργήσετε μια εφαρμογή για τη συσκευή που αναφέρει τις πληροφορίες σύνδεσης και αλλάζει το αποτέλεσμα του ερωτήματος στην προηγούμενη ενότητα.

## <a name="create-the-device-app"></a>Δημιουργία της εφαρμογής συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα Node.js εφαρμογής κονσόλας που συνδέεται με το Κέντρο σας ως **myDeviceId**και, στη συνέχεια, ενημερώνει τις διπλού αναφερθεί ιδιότητες, για να περιέχουν τις πληροφορίες που είναι συνδεδεμένος με ένα δίκτυο κινητής τηλεφωνίας.

> [AZURE.NOTE] Προς το παρόν, twins συσκευή είναι προσβάσιμα μόνο από συσκευών που συνδέονται με χρήση του πρωτοκόλλου MQTT διανομέα IoT. Ανατρέξτε για την [υποστήριξη MQTT] [ lnk-devguide-mqtt] το άρθρο για οδηγίες σχετικά με τον τρόπο για να μετατρέψετε την υπάρχουσα εφαρμογή συσκευή για να χρησιμοποιήσετε MQTT.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **reportconnectivity**. Στο φάκελο **reportconnectivity** , δημιουργήστε ένα νέο αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```

2. Στο σας εντολών στο φάκελο **reportconnectivity** , εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε το **azure-iot-συσκευή**και πακέτο **azure-iot-συσκευή-mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **ReportConnectivity.js** στο φάκελο **reportconnectivity** .

4. Προσθέστε τον ακόλουθο κώδικα στο αρχείο **ReportConnectivity.js** και αντικαταστήστε το σύμβολο κράτησης θέσης **{συμβολοσειρά σύνδεσης συσκευή}** με τη συμβολοσειρά σύνδεσης που αντιγράψατε όταν δημιουργήσατε την ταυτότητα συσκευή **myDeviceId** :

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    Το αντικείμενο **προγράμματος-πελάτη** εκθέτει όλες τις μεθόδους που χρειάζεστε για να αλληλεπιδράσετε με twins συσκευή από τη συσκευή. Προηγούμενο κώδικα, αφού προετοιμάζει το αντικείμενο **προγράμματος-πελάτη** και ανακτά την διπλού για **myDeviceId** και ενημερώνει ιδιότητά του αναφερόμενου με τις πληροφορίες σύνδεσης.

5. Εκτελέστε την εφαρμογή συσκευή

        node ReportConnectivity.js

    Θα πρέπει να βλέπετε το μήνυμα `twin state reported`.

6. Τώρα που η συσκευή αναφερθεί τις πληροφορίες για τη σύνδεση, θα πρέπει να εμφανίζεται σε δύο ερωτήματα. Επιστρέψτε στο φάκελο **addtagsandqueryapp** και εκτελέστε ξανά τα ερωτήματα:

        node AddTagsAndQuery.js

    Αυτό ώρα **myDeviceId** πρέπει να εμφανίζεται στα δύο αποτελέσματα ερωτήματος.

    ![][3]

## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το πρόγραμμα εκμάθησης, που έχει ρυθμιστεί διανομέα IoT στην πύλη του Azure και, στη συνέχεια, να δημιουργήσει μια ταυτότητα συσκευής στο μητρώο ταυτότητας την ενότητα. Προσθέσατε συσκευή μετα-δεδομένα ως ετικέτες από μια εφαρμογή παρασκηνίου, και συντάξατε μια εφαρμογή προσομοιωμένη συσκευή στις πληροφορίες συνδεσιμότητας συσκευή αναφοράς στο διπλού τη συσκευή. Μάθατε επίσης πώς ερώτημα για αυτές τις πληροφορίες χρησιμοποιώντας την ενότητα IoT γλώσσα ερωτημάτων μοιάζει με SQL.

Χρησιμοποιήστε τους παρακάτω πόρους για να μάθετε πώς μπορείτε να:

- Αποστολή τηλεμετρίας από συσκευές με το παράθυρο [Γρήγορα αποτελέσματα με το Κέντρο IoT] [ lnk-iothub-getstarted] το πρόγραμμα εκμάθησης,
- ρύθμιση παραμέτρων των συσκευών με χρήση του διπλού επιθυμητές ιδιότητες με τη [Χρήση επιθυμητή ιδιότητες, για να ρυθμίσετε τις παραμέτρους συσκευών] [ lnk-twin-how-to-configure] το πρόγραμμα εκμάθησης,
- ελέγξετε συσκευές αλληλεπιδραστικά (όπως η ενεργοποίηση αρέσουν από μια εφαρμογή ελέγχεται από το χρήστη), με τις [μεθόδους άμεση χρήση] [ lnk-methods-tutorial] το πρόγραμμα εκμάθησης.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md