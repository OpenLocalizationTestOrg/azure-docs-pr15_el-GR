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
* **SetDesiredConfigurationAndQuery**, μια εφαρμογή κονσόλας .NET σκοπεύατε να να εκτελεστεί από παρασκηνίου, που ορίζει την επιθυμητή ρύθμιση παραμέτρων σε μια συσκευή και ερωτήματα τη διαδικασία της ενημέρωσης ρύθμισης παραμέτρων.

> [AZURE.NOTE] Το άρθρο [SDK διανομέα IoT] [ lnk-hub-sdks] παρέχει πληροφορίες σχετικά με τα διάφορα SDK που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε εφαρμογές συσκευής και παρασκηνίου.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης χρειάζεστε τα εξής:

+ Microsoft Visual Studio 2015.

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

Σε αυτήν την ενότητα, θα δημιουργήσετε μια εφαρμογή κονσόλας .NET που ενημερώνει τις *επιθυμητές ιδιότητες* στην το διπλού που σχετίζονται με **myDeviceId** με ένα νέο αντικείμενο ρύθμισης παραμέτρων τηλεμετρίας. Στη συνέχεια, τα ερωτήματα του twins τη συσκευή που είναι αποθηκευμένα στην ενότητα και εμφανίζει τη διαφορά μεταξύ τις επιθυμητές και αναφερόμενου παραμέτρων της συσκευής.

1. Στο Visual Studio, προσθέστε ένα έργο Visual C# κλασική επιφάνεια εργασίας των Windows στην τρέχουσα λύση, χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Το όνομα του έργου **SetDesiredConfigurationAndQuery**.

    ![Νέο έργο Visual C# κλασική επιφάνεια εργασίας των Windows][img-createapp]

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **SetDesiredConfigurationAndQuery** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων Nuget**.

3. Στο παράθυρο **Διαχείριση πακέτου Nuget** , βεβαιωθείτε ότι είναι επιλεγμένη η **Συμπερίληψη προέκδοση** , αναζητήστε **microsoft.azure.devices**, επιλέξτε **εγκατάσταση** για να εγκαταστήσετε το *την προέκδοση του την **Microsoft.Azure.Devices** * συσκευασία και αποδεχτείτε τους όρους χρήσης. Αυτή η διαδικασία στοιχεία λήψης, εγκαθιστά και προσθέτει την αναφορά στο [Microsoft Azure IoT υπηρεσίας SDK] [ lnk-nuget-service-sdk] Nuget πακέτου και τις εξαρτήσεις.

    ![Παράθυρο "Διαχείριση πακέτου Nuget"][img-servicenuget]

4. Προσθέστε τα ακόλουθα `using` δηλώσεις στο επάνω μέρος του αρχείου **Program.cs** :

        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;

5. Προσθέστε τα παρακάτω πεδία για το εκπαιδευτικό **πρόγραμμα** . Αντικαταστήστε την τιμή κράτησης θέσης με τη συμβολοσειρά σύνδεσης για την ενότητα IoT που δημιουργήσατε στην προηγούμενη ενότητα.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Προσθέστε την ακόλουθη μέθοδο για την τάξη **προγράμματος** :

        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
            
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired state");

            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }

    Το αντικείμενο **μητρώου** εκθέτει όλες τις μεθόδους που απαιτούνται για να αλληλεπιδράσετε με συσκευή twins από την υπηρεσία. Προηγούμενο κώδικα, αφού προετοιμάζει το αντικείμενο **μητρώου** , ανακτά την διπλού για **myDeviceId**και ενημερώνει τις επιθυμητές ιδιότητες με ένα νέο αντικείμενο ρύθμισης παραμέτρων τηλεμετρίας.
    Μετά από αυτό, κάθε 10 δευτερόλεπτα, το ερωτήματα το twins που είναι αποθηκευμένα στην ενότητα και να εκτυπώνει τις ρυθμίσεις παραμέτρων επιθυμητή και αναφερόμενου τηλεμετρίας. Ανατρέξτε στην [ενότητα IoT γλώσσα ερωτημάτων] [ lnk-query] για να μάθετε πώς να δημιουργείτε αναφορές εμπλουτισμένου σε όλες τις συσκευές σας.

    > [AZURE.IMPORTANT] Αυτή η εφαρμογή ερωτήματα διανομέα IoT κάθε 10 δευτερόλεπτα επεξήγηση. Χρήση ερωτημάτων για τη δημιουργία αναφορών άμεσα προσβάσιμη από το χρήστη σε πολλές συσκευές και όχι για τον εντοπισμό αλλαγών. Εάν η λύση απαιτεί σε πραγματικό χρόνο ειδοποιήσεις για συμβάντα συσκευή χρησιμοποιούν [τη συσκευή για να cloud μηνύματα][lnk-d2c].

7. Τέλος, προσθέστε τις ακόλουθες γραμμές στην **κύρια** μέθοδο:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();

8. Ενώ εκτελείται το **SimulateDeviceConfiguration.js** , εκτελέστε την εφαρμογή .NET από το Visual Studio χρησιμοποιώντας **F5** και που θα πρέπει να δείτε τις παραμέτρους αναφερόμενου αλλαγή από το να **επιτύχουν** σε **σε εκκρεμότητα** για την **επιτυχία** ξανά με το νέο ενεργό αποστολή συχνότητα πέντε λεπτά αντί για 24 ώρες.

    > [AZURE.IMPORTANT] Υπάρχει μια καθυστέρηση έως και ένα λεπτό μεταξύ της λειτουργίας αναφοράς συσκευή και το αποτέλεσμα του ερωτήματος. Αυτή είναι η ενεργοποίηση της υποδομής ερωτήματος για να εργαστείτε με πολύ υψηλή κλίμακα. Για να ανακτήσετε συνεπή προβολών σε μια μεμονωμένη διπλού, χρησιμοποιήστε τη μέθοδο **getDeviceTwin** στην τάξη **μητρώου** .

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, ορίστε μια επιθυμητή ρύθμιση παραμέτρων ως *επιθυμητοί ιδιότητες* από παρασκηνίου και συντάξατε μια εφαρμογή για τη συσκευή για να εντοπίσει αυτήν την αλλαγή και να προσομοιώνουν μια διαδικασία της ενημέρωσης πολλών βήμα αναφοράς, η κατάστασή της ως *αναφερόμενου ιδιότητες* για να το διπλού.

Χρησιμοποιήστε τους παρακάτω πόρους για να μάθετε πώς μπορείτε να:

- Αποστολή τηλεμετρίας από συσκευές με το παράθυρο [Γρήγορα αποτελέσματα με το Κέντρο IoT] [ lnk-iothub-getstarted] το πρόγραμμα εκμάθησης,
- Προγραμματισμός ή εκτελούν λειτουργίες σε μεγάλα σύνολα συσκευές ανατρέξτε στο θέμα [Χρήση εργασιών για να προγραμματίσετε και εκπομπή λειτουργιών συσκευών] [ lnk-schedule-jobs] το πρόγραμμα εκμάθησης.
- ελέγξετε συσκευές αλληλεπιδραστικά (όπως η ενεργοποίηση αρέσουν από μια εφαρμογή ελέγχεται από το χρήστη), με τις [μεθόδους άμεση χρήση] [ lnk-methods-tutorial] το πρόγραμμα εκμάθησης.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

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
