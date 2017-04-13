<properties
    pageTitle="Χρήση ιδιοτήτων διπλού | Microsoft Azure"
    description="Αυτό το πρόγραμμα εκμάθησης σας δείχνει πώς μπορείτε να χρησιμοποιήσετε διπλού ιδιότητες"
    services="iot-hub"
    documentationCenter=".net"
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

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Πρόγραμμα εκμάθησης: Χρησιμοποιήστε επιθυμητές ιδιότητες για να ρυθμίσετε τις συσκευές (έκδοση preview)

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Στο τέλος αυτού του προγράμματος εκμάθησης, θα έχετε δύο Node.js κονσόλα εφαρμογών:

* **SimulateDeviceConfiguration.js**, μια εφαρμογή προσομοιωμένη τη συσκευή που χρειάζεται να περιμένει για μια ενημερωμένη έκδοση επιθυμητή ρύθμιση παραμέτρων και αναφέρει την κατάσταση της μια διαδικασία ενημέρωσης προσομοιωμένη ρύθμισης παραμέτρων.
* **SetDesiredConfigurationAndQuery.js**, μια εφαρμογή Node.js σκοπεύατε να να εκτελεστεί από παρασκηνίου, το οποίο ορίζει την επιθυμητή ρύθμιση παραμέτρων σε μια συσκευή και ερωτήματα τη διαδικασία της ενημέρωσης ρύθμισης παραμέτρων.

> [AZURE.NOTE] Το άρθρο [SDK διανομέα IoT] [ lnk-hub-sdks] παρέχει πληροφορίες σχετικά με τα διάφορα SDK που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε εφαρμογές συσκευής και παρασκηνίου.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης χρειάζεστε τα εξής:

+ Έκδοση node.js 0.10.x ή νεότερη έκδοση.

+ Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

Εάν ακολουθήσατε τα [Γρήγορα αποτελέσματα με τη συσκευή twins] [ lnk-twin-tutorial] το πρόγραμμα εκμάθησης, έχετε ήδη ένα διανομέα Ενεργοποίηση διαχείρισης συσκευών και μια ταυτότητα συσκευής που ονομάζεται **myDeviceId**; και μπορείτε να παραλείψετε για τη [Δημιουργία της εφαρμογής προσομοιωμένη συσκευή] [ lnk-how-to-configure-createapp] ενότητας.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>Δημιουργία της εφαρμογής προσομοιωμένη συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που συνδέεται με το Κέντρο σας ως **myDeviceId**, χρειάζεται να περιμένει για μια ενημερωμένη έκδοση επιθυμητή ρύθμιση παραμέτρων και, στη συνέχεια, αναφέρει ενημερώσεις σχετικά με τη διαδικασία ενημέρωσης προσομοιωμένη ρύθμισης παραμέτρων.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **simulatedeviceconfiguration**. Στο φάκελο **simulatedeviceconfiguration** , δημιουργήστε ένα νέο αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```

2. Στο σας εντολών στο φάκελο **simulatedeviceconfiguration** , εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε το **azure-iot-συσκευή**και πακέτο **azure-iot-συσκευή-mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **SimulateDeviceConfiguration.js** στο φάκελο **simulatedeviceconfiguration** .

4. Προσθέστε τον ακόλουθο κώδικα στο αρχείο **SimulateDeviceConfiguration.js** και αντικαταστήστε το σύμβολο κράτησης θέσης **{συμβολοσειρά σύνδεσης συσκευή}** με τη συμβολοσειρά σύνδεσης που αντιγράψατε όταν δημιουργήσατε την ταυτότητα συσκευή **myDeviceId** :

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    Το αντικείμενο **προγράμματος-πελάτη** εκθέτει όλες τις μεθόδους που απαιτούνται για να αλληλεπιδράσετε με twins συσκευή από τη συσκευή. Προηγούμενο κώδικα, αφού προετοιμάζει το αντικείμενο **προγράμματος-πελάτη** και ανακτά την διπλού για **myDeviceId**και επισυνάπτει ένα πρόγραμμα χειρισμού για την ενημέρωση σχετικά με τις επιθυμητές ιδιότητες. Το πρόγραμμα χειρισμού επαληθεύει ότι υπάρχει είναι μια αίτηση αλλαγής πραγματική ρύθμισης παραμέτρων, συγκρίνοντας το configIds, στη συνέχεια, καλεί μια μέθοδο που ξεκινά η αλλαγή της ρύθμισης παραμέτρων.

    Σημειώστε ότι λόγους απλούστευσης τον προηγούμενο κώδικα χρησιμοποιείται από προεπιλογή σχεδιασμένου για την αρχική ρύθμιση παραμέτρων. Μια εφαρμογή για πραγματικό πιθανότατα θα φορτώνει ότι η ρύθμιση παραμέτρων από ένα τοπικό χώρο αποθήκευσης.
    
    > [AZURE.IMPORTANT] Συμβάντα αλλαγής επιθυμητή ιδιότητα είναι πάντα που εκπέμπει μία φορά στη σύνδεση της συσκευής, βεβαιωθείτε ότι για να ελέγξετε ότι υπάρχει μια πραγματική αλλαγή στις επιθυμητές ιδιότητες πριν να εκτελέσετε κάποια ενέργεια.

5. Προσθέστε τις ακόλουθες μεθόδους πριν από την `client.open()` κλήση:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    Η μέθοδος **initConfigChange** ενημερώνει αναφερόμενου ιδιότητες στο αντικείμενο τοπικού διπλού με την αίτηση ενημέρωσης ρύθμισης παραμέτρων και ορίζει την κατάσταση **σε εκκρεμότητα**, στη συνέχεια, ενημερώνει το διπλού συσκευής στην υπηρεσία. Μετά την ενημέρωση με επιτυχία το διπλού, το προσομοιώνει μια διαδικασία μεγάλης διάρκειας που τερματίζει κατά την εκτέλεση των **completeConfigChange**. Αυτή η μέθοδος ενημερώσεις του τοπικού διπλού αναφερόμενου ιδιότητες τη ρύθμιση της κατάστασης για την **επιτυχία** και την κατάργηση του αντικειμένου **pendingConfig** . Στη συνέχεια, ενημερώνει το διπλού στην υπηρεσία.

    Λάβετε υπόψη ότι, για να αποθηκεύσετε το εύρος ζώνης, αναφερθεί ενημερώνονται ιδιότητες, καθορίζοντας μόνο τις ιδιότητες για να είναι δυνατή η τροποποίηση (επώνυμο του **κώδικα** στον κώδικα παραπάνω), αντί να αντικαταστήσετε ολόκληρο το έγγραφο.

    > [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης δεν προσομοίωση οποιαδήποτε συμπεριφορά για ενημερώσεις ταυτόχρονες ρύθμισης παραμέτρων. Ορισμένες διεργασίες ενημέρωσης ρύθμισης παραμέτρων μπορεί να είναι σε θέση να δέχεται τις αλλαγές της ρύθμισης παραμέτρων προορισμού, ενώ εκτελείται η ενημέρωση και άλλους ενδέχεται να χρειαστεί να ουρά τους άλλους χρήστες μπορεί να τα απορρίψετε με μια συνθήκη σφάλματος. Βεβαιωθείτε ότι πρέπει να λάβετε υπόψη τη συμπεριφορά που θέλετε για τη διαδικασία ρύθμισης παραμέτρων συγκεκριμένων και προσθέστε την κατάλληλη λογική πριν από την προετοιμασία την αλλαγή της ρύθμισης παραμέτρων.

6. Εκτελέστε την εφαρμογή συσκευή:

        node SimulateDeviceConfiguration.js

    Θα πρέπει να βλέπετε το μήνυμα `retrieved device twin`. Διατήρηση της εφαρμογής που εκτελείται.

## <a name="create-the-service-app"></a>Δημιουργία της εφαρμογής υπηρεσίας

Σε αυτήν την ενότητα, θα δημιουργήσετε μια εφαρμογή κονσόλας Node.js που ενημερώνει τις *επιθυμητές ιδιότητες* στην το διπλού που σχετίζονται με **myDeviceId** με ένα νέο αντικείμενο ρύθμισης παραμέτρων τηλεμετρίας. Στη συνέχεια, τα ερωτήματα του twins τη συσκευή που είναι αποθηκευμένα στην ενότητα και εμφανίζει τη διαφορά μεταξύ τις επιθυμητές και αναφερόμενου παραμέτρων της συσκευής.

1. Δημιουργήστε ένα νέο, κενό φάκελο που ονομάζεται **setdesiredandqueryapp**. Στο φάκελο **setdesiredandqueryapp** , δημιουργήστε ένα νέο αρχείο package.json χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Αποδεχτείτε τις προεπιλεγμένες τιμές:

    ```
    npm init
    ```

2. Στο σας εντολών στο φάκελο **setdesiredandqueryapp** , εκτελέστε την ακόλουθη εντολή για την εγκατάσταση του πακέτου **azure iothub** :

    ```
    npm install azure-iothub@dtpreview node-uuid --save
    ```

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα νέο αρχείο **SetDesiredAndQuery.js** στο φάκελο **addtagsandqueryapp** .

4. Προσθέστε τον ακόλουθο κώδικα στο αρχείο **SetDesiredAndQuery.js** και αντικαταστήστε το σύμβολο κράτησης θέσης **{συμβολοσειρά σύνδεσης υπηρεσίας}** με τη συμβολοσειρά σύνδεσης που αντιγράψατε όταν δημιουργήσατε το Κέντρο σας:

        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{service connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
         
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });
            

    Το αντικείμενο **μητρώου** εκθέτει όλες τις μεθόδους που απαιτούνται για να αλληλεπιδράσετε με συσκευή twins από την υπηρεσία. Προηγούμενο κώδικα, αφού προετοιμάζει το αντικείμενο **μητρώου** , ανακτά την διπλού για **myDeviceId**και ενημερώνει τις επιθυμητές ιδιότητες με ένα νέο αντικείμενο ρύθμισης παραμέτρων τηλεμετρίας. Μετά από αυτό, καλεί το συμβάν συνάρτηση **queryTwins** 10 δευτερόλεπτα.

    > [AZURE.IMPORTANT] Αυτή η εφαρμογή ερωτήματα διανομέα IoT κάθε 10 δευτερόλεπτα επεξήγηση. Χρήση ερωτημάτων για τη δημιουργία αναφορών άμεσα προσβάσιμη από το χρήστη σε πολλές συσκευές και όχι για τον εντοπισμό αλλαγών. Εάν η λύση απαιτεί σε πραγματικό χρόνο ειδοποιήσεις για συμβάντα συσκευή χρησιμοποιούν [τη συσκευή για να cloud μηνύματα][lnk-d2c].

5. Προσθέστε τα εξής δεξιά κώδικα πριν από την `registry.getDeviceTwin()` κλήσης για την υλοποίηση της συνάρτησης **queryTwins** :

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };

    Τα ερωτήματα προηγούμενο κώδικα του twins είναι αποθηκευμένα στην ενότητα και εκτυπώνει τις ρυθμίσεις παραμέτρων επιθυμητή και αναφερόμενου τηλεμετρίας. Ανατρέξτε στην [ενότητα IoT γλώσσα ερωτημάτων] [ lnk-query] για να μάθετε πώς να δημιουργείτε αναφορές εμπλουτισμένου σε όλες τις συσκευές σας.


8. Ενώ εκτελείται το **SimulateDeviceConfiguration.js** , εκτελέστε την εφαρμογή με:

        node SetDesiredAndQuery.js 5m

    Θα πρέπει να βλέπετε το αναφερόμενου Αλλαγή ρύθμισης παραμέτρων από το να **επιτύχουν** ως **σε εκκρεμότητα** για την **επιτυχία** ξανά με το νέο ενεργό αποστολή συχνότητα πέντε λεπτά αντί για 24 ώρες.

    > [AZURE.IMPORTANT] Υπάρχει μια καθυστέρηση έως και ένα λεπτό μεταξύ της λειτουργίας αναφοράς συσκευή και το αποτέλεσμα του ερωτήματος. Αυτή είναι η ενεργοποίηση της υποδομής ερωτήματος για να εργαστείτε με πολύ υψηλή κλίμακα. Για να ανακτήσετε συνεπή προβολών σε μια μεμονωμένη διπλού, χρησιμοποιήστε τη μέθοδο **getDeviceTwin** στην τάξη **μητρώου** .

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, ορίστε μια επιθυμητή ρύθμιση παραμέτρων ως *επιθυμητές ιδιότητες* από την εφαρμογή παρασκηνίου και συντάξατε μια εφαρμογή προσομοιωμένη συσκευή για τον εντοπισμό που αλλαγή και να προσομοιώνουν μια διαδικασία της ενημέρωσης πολλών βήμα αναφοράς, η κατάστασή της ως *αναφερθεί ιδιότητες* για να το διπλού.

Χρησιμοποιήστε τους παρακάτω πόρους για να μάθετε πώς μπορείτε να:

- Αποστολή τηλεμετρίας από συσκευές με το παράθυρο [Γρήγορα αποτελέσματα με το Κέντρο IoT] [ lnk-iothub-getstarted] το πρόγραμμα εκμάθησης,
- Προγραμματισμός ή εκτελούν λειτουργίες σε μεγάλα σύνολα συσκευές ανατρέξτε στο θέμα [Χρήση εργασιών για να προγραμματίσετε και εκπομπή λειτουργιών συσκευών] [ lnk-schedule-jobs] το πρόγραμμα εκμάθησης.
- ελέγξετε συσκευές αλληλεπιδραστικά (όπως η ενεργοποίηση αρέσουν από μια εφαρμογή ελέγχεται από το χρήστη), με τις [μεθόδους άμεση χρήση] [ lnk-methods-tutorial] το πρόγραμμα εκμάθησης.


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
