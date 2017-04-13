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

Στο τέλος αυτού του προγράμματος εκμάθησης, θα έχετε μια .NET και μια εφαρμογή κονσόλας Node.js:

* **AddTagsAndQuery.sln**, μια εφαρμογή .NET σκοπεύατε να να εκτελεστεί από παρασκηνίου, το οποίο προσθέτει ετικέτες και τα ερωτήματα twins συσκευή.
* **TwinSimulatedDevice.js**, μια εφαρμογή Node.js που προσομοιώνει μια συσκευή που συνδέεται με το Κέντρο IoT σας με την ταυτότητα της συσκευής που δημιουργήσατε νωρίτερα, τις αναφορές και της κατάστασης σύνδεσης.

> [AZURE.NOTE] Το άρθρο [SDK διανομέα IoT] [ lnk-hub-sdks] παρέχει πληροφορίες σχετικά με τα διάφορα SDK που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε εφαρμογές συσκευής και παρασκηνίου.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης χρειάζεστε τα εξής:

+ Microsoft Visual Studio 2015.

+ Έκδοση node.js 0.10.x ή νεότερη έκδοση.

+ Λογαριασμού Azure active. (Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Δημιουργία της εφαρμογής υπηρεσίας

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Node.js που προσθέτει το διπλού που σχετίζονται με **myDeviceId**θέση μετα-δεδομένα. Το, στη συνέχεια, τα ερωτήματα του twins που είναι αποθηκευμένα στην ενότητα επιλογή τις συσκευές που βρίσκονται σε Ηνωμένες Πολιτείες και, στη συνέχεια, αυτές που αναφοράς σύνδεσης κινητής τηλεφωνίας.

1. Στο Visual Studio, προσθέστε ένα έργο Visual C# κλασική επιφάνεια εργασίας των Windows στην τρέχουσα λύση, χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Το όνομα του έργου **AddTagsAndQuery**.

    ![Νέο έργο Visual C# κλασική επιφάνεια εργασίας των Windows][img-createapp]

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **AddTagsAndQuery** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων Nuget**.

3. Στο παράθυρο **Διαχείριση πακέτου Nuget** , βεβαιωθείτε ότι είναι επιλεγμένη η **Συμπερίληψη προέκδοση** , αναζητήστε **microsoft.azure.devices**, επιλέξτε **εγκατάσταση** για να εγκαταστήσετε το *την προέκδοση του την **Microsoft.Azure.Devices** * συσκευασία και αποδεχτείτε τους όρους χρήσης. Αυτή η διαδικασία στοιχεία λήψης, εγκαθιστά και προσθέτει την αναφορά στο [Microsoft Azure IoT υπηρεσίας SDK] [ lnk-nuget-service-sdk] Nuget πακέτου και τις εξαρτήσεις.

    ![Παράθυρο "Διαχείριση πακέτου Nuget"][img-servicenuget]

4. Προσθέστε τα ακόλουθα `using` δηλώσεις στο επάνω μέρος του αρχείου **Program.cs** :

        using Microsoft.Azure.Devices;

5. Προσθέστε τα παρακάτω πεδία για το εκπαιδευτικό **πρόγραμμα** . Αντικαταστήστε την τιμή κράτησης θέσης με τη συμβολοσειρά σύνδεσης για την ενότητα IoT που δημιουργήσατε στην προηγούμενη ενότητα.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Προσθέστε την ακόλουθη μέθοδο για την τάξη **προγράμματος** :

        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }

    Η κλάση **RegistryManager** εκθέτει όλες τις μεθόδους που απαιτούνται για να αλληλεπιδράσετε με συσκευή twins από την υπηρεσία. Τον προηγούμενο κώδικα προετοιμάζει πρώτα το αντικείμενο **registryManager** και, στη συνέχεια, ανακτά την διπλού για **myDeviceId**και τέλος ενημερώνει τις ετικέτες με τις πληροφορίες στη θέση που θέλετε.

    Μετά την ενημέρωση, εκτελείται δύο ερωτήματα: μόνο η συσκευή twins συσκευών που βρίσκεται στη μονάδα **Redmond43** επιλέγει το πρώτο και το δεύτερο έχει βελτιώσει το ερώτημα για να επιλέξετε μόνο τις συσκευές που συνδέονται μέσω του δικτύου κινητής τηλεφωνίας.

    Σημειώστε ότι το προηγούμενο κωδικό, όταν δημιουργεί το αντικείμενο **ερωτήματος** , καθορίζει το μέγιστο αριθμό των εγγράφων που επιστρέφεται. Το αντικείμενο **ερωτήματος** περιέχει μια δυαδική ιδιότητα **HasMoreResults** που μπορείτε να χρησιμοποιήσετε για να καλέσετε τις μεθόδους **GetNextAsTwinAsync** πολλές φορές για την ανάκτηση όλων των αποτελεσμάτων. Μια μέθοδο που ονομάζεται **GetNextAsJson** είναι διαθέσιμη για τα αποτελέσματα που δεν είναι συσκευή twins για παράδειγμα, αποτελέσματα ερωτημάτων συνάθροισης.

7. Τέλος, προσθέστε τις ακόλουθες γραμμές στην **κύρια** μέθοδο:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

8. Εκτέλεση αυτής της εφαρμογής και θα πρέπει να βλέπετε μία συσκευή στα αποτελέσματα για το ερώτημα που σας ρωτά για όλες τις συσκευές που βρίσκεται στο **Redmond43** και καμία για το ερώτημα που περιορίζει τα αποτελέσματα σε συσκευές που χρησιμοποιούν το δίκτυο κινητής τηλεφωνίας.

    ![Αποτελέσματα ερωτήματος στο παράθυρο][img-addtagapp]

Στην επόμενη ενότητα μπορείτε να δημιουργήσετε μια εφαρμογή για τη συσκευή που αναφέρει τις πληροφορίες σύνδεσης και αλλάζει το αποτέλεσμα του ερωτήματος στην προηγούμενη ενότητα.

## <a name="create-the-device-app"></a>Δημιουργία της εφαρμογής συσκευή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα Node.js εφαρμογής κονσόλας που συνδέεται με το Κέντρο σας ως **myDeviceId**και, στη συνέχεια, ενημερώνει τις διπλού αναφερθεί ιδιότητες, για να περιέχουν τις πληροφορίες που είναι συνδεδεμένος με ένα δίκτυο κινητής τηλεφωνίας.

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

6. Τώρα που η συσκευή αναφερθεί τις πληροφορίες για τη σύνδεση, θα πρέπει να εμφανίζεται σε δύο ερωτήματα. Εκτελέστε την εφαρμογή.NET **AddTagsAndQuery** για να εκτελέσετε ξανά τα ερωτήματα. Αυτό ώρα **myDeviceId** πρέπει να εμφανίζεται στα δύο αποτελέσματα ερωτήματος.

    ![][img-addtagapp2]

## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το πρόγραμμα εκμάθησης, που έχει ρυθμιστεί διανομέα IoT στην πύλη του Azure και, στη συνέχεια, να δημιουργήσει μια ταυτότητα συσκευής στο μητρώο ταυτότητας την ενότητα. Προσθέσατε συσκευή μετα-δεδομένα ως ετικέτες από μια εφαρμογή παρασκηνίου, και συντάξατε μια εφαρμογή προσομοιωμένη συσκευή στις πληροφορίες συνδεσιμότητας συσκευή αναφοράς στο διπλού τη συσκευή. Μάθατε επίσης πώς ερώτημα για αυτές τις πληροφορίες χρησιμοποιώντας την ενότητα IoT γλώσσα ερωτημάτων μοιάζει με SQL.

Χρησιμοποιήστε τους παρακάτω πόρους για να μάθετε πώς μπορείτε να:

- Αποστολή τηλεμετρίας από συσκευές με το παράθυρο [Γρήγορα αποτελέσματα με το Κέντρο IoT] [ lnk-iothub-getstarted] το πρόγραμμα εκμάθησης,
- ρύθμιση παραμέτρων των συσκευών με χρήση του διπλού επιθυμητές ιδιότητες με τη [Χρήση επιθυμητή ιδιότητες, για να ρυθμίσετε τις παραμέτρους συσκευών] [ lnk-twin-how-to-configure] το πρόγραμμα εκμάθησης,
- ελέγξετε συσκευές αλληλεπιδραστικά (όπως η ενεργοποίηση αρέσουν από μια εφαρμογή ελέγχεται από το χρήστη) με τις [μεθόδους άμεση χρήση] [ lnk-methods-tutorial] το πρόγραμμα εκμάθησης.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

