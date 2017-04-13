<properties
    pageTitle="Προσομοίωση μια συσκευή με το SDK πύλης IoT | Microsoft Azure"
    description="Azure SDK πύλης IoT οδηγίες χρησιμοποιώντας Linux για την απεικόνιση αποστολή τηλεμετρίας από μια συσκευή προσομοιωμένη χρησιμοποιώντας το SDK Azure IoT πύλης."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-linux"></a>Azure IoT πύλης SDK (beta) – αποστολή μηνυμάτων συσκευής στο cloud με προσομοιωμένη συσκευές που χρησιμοποιούν το Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Δημιουργία και εκτέλεση του δείγματος

Πριν ξεκινήσετε, πρέπει να:

- [Ρυθμίστε το περιβάλλον ανάπτυξης] [ lnk-setupdevbox] για την εργασία με το SDK στην Linux.
- [Δημιουργήστε ένα διανομέα IoT] [ lnk-create-hub] σε Azure συνδρομή σας, θα χρειαστεί το όνομα του Κέντρο σας για να ολοκληρώσετε αυτόν τον οδηγό. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.
- Προσθέστε δύο συσκευές με το διανομέα IoT και σημειώστε τα αναγνωριστικά και τα κλειδιά συσκευή. Μπορείτε να χρησιμοποιήσετε την [Εξερεύνηση συσκευή ή από την Εξερεύνηση iothub] [ lnk-explorer-tools] εργαλείο για να προσθέσετε τις συσκευές σας την ενότητα IoT που δημιουργήσατε στο προηγούμενο βήμα και να ανακτήσετε τους αριθμούς-κλειδιά.

Για να δημιουργήσετε το δείγμα:

1. Ανοίξτε ένα κέλυφος.
2. Μεταβείτε στον ριζικό φάκελο στο τοπικό αντίγραφο του αποθετηρίου **azure iot-πύλης sdk** .
3. Εκτελέστε τη δέσμη ενεργειών **tools/build.sh** . Αυτή η δέσμη ενεργειών χρησιμοποιεί το βοηθητικό πρόγραμμα **cmake** για να δημιουργήσετε ένα φάκελο που ονομάζεται **Δημιουργία** στον ριζικό φάκελο του τοπικού αντιγράφου του αποθετηρίου **azure iot-πύλης sdk** και δημιουργούν ένα αρχείο κατασκευής. Η δέσμη ενεργειών, στη συνέχεια, δημιουργεί τη λύση και εκτελεί τις δοκιμές.

> [AZURE.NOTE]  Κάθε φορά που εκτελείτε τη δέσμη ενεργειών **build.sh** , διαγράφει και, στη συνέχεια, δημιουργεί εκ νέου στο φάκελο που **δημιουργείτε** στο ριζικό φάκελο του τοπικού αντιγράφου του αποθετηρίου **azure-iot-πύλης-sdk** .

Για να εκτελέσετε το δείγμα:

Σε ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο **samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json** στο τοπικό αντίγραφο του αποθετηρίου **azure-iot-πύλης-sdk** . Αυτό το αρχείο ρυθμίζει τις ενότητες στο δείγμα πύλης:

- Η λειτουργική μονάδα **IoTHub** συνδέεται με το Κέντρο IoT σας. Θα πρέπει να ρυθμίσετε την αποστολή δεδομένων προς το Κέντρο IoT σας. Συγκεκριμένα, ορίστε την τιμή **IoTHubName** στο όνομα του Κέντρο σας IoT και ορίστε την τιμή **IoTHubSuffix** σε **azure devices.net**. Ορίστε την τιμή **μεταφοράς** σε μία από: "HTTP", "AMQP" ή "MQTT". Σημειώστε ότι τη συγκεκριμένη στιγμή, μόνο "HTTP" θα κάνετε κοινή χρήση μία σύνδεση TCP για όλα τα μηνύματα συσκευή. Εάν ορίσετε την τιμή "AMQP" ή "MQTT", η πύλη θα διατηρήσει μια ξεχωριστή σύνδεση TCP IoT διανομέα για κάθε συσκευή.
- Τη λειτουργική μονάδα **αντιστοίχισης** αντιστοιχίζει τις διευθύνσεις MAC προσομοιωμένη συσκευή σας ταυτότητες συσκευή IoT διανομέα. Βεβαιωθείτε ότι **deviceId** τιμές ταιριάζουν με τα αναγνωριστικά των δύο συσκευές που έχετε προσθέσει στο κέντρο σας IoT και ότι οι τιμές **deviceKey** περιέχουν τα πλήκτρα δύο συσκευές σας.
- Οι λειτουργικές μονάδες **BLE1** και **BLE2** είναι προσομοιωμένη συσκευές. Παρατηρήστε πώς τις διευθύνσεις MAC ταιριάζουν με τις τιμές της λειτουργικής μονάδας **αντιστοίχισης** .
- Η λειτουργική μονάδα **καταγραφής** καταγράφει τη δραστηριότητά σας πύλης σε ένα αρχείο.
- Η **διαδρομή λειτουργική μονάδα** τιμές που εμφανίζονται παρακάτω λαμβάνεται ως δεδομένο ότι θα μπορείτε να εκτελέσετε το δείγμα από το ριζικό κατάλογο της του τοπικού αντιγράφου του αποθετηρίου **azure iot-πύλης sdk** .
- Ο πίνακας **συνδέσεων** στο κάτω μέρος του αρχείου JSON συνδέεται το λειτουργικές μονάδες **BLE1** και **BLE2** για τη λειτουργική μονάδα **αντιστοίχισης** και τη λειτουργική μονάδα **αντιστοίχισης** στη λειτουργική μονάδα **IoTHub** . Επίσης, εξασφαλίζει ότι όλα τα μηνύματα που καταγράφονται από τη λειτουργική μονάδα **καταγραφής** .

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "./build/modules/iothub/libiothub_hl.so",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "./build/modules/identitymap/libidentity_map_hl.so",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "./build/modules/logger/liblogger_hl.so",
            "args":
            {
                "filename":"./deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}

```

Αποθηκεύστε τις αλλαγές που κάνατε στο αρχείο παραμέτρων.

Για να εκτελέσετε το δείγμα:

1. Στο κέλυφος σας, μεταβείτε στον ριζικό φάκελο στο τοπικό αντίγραφο του αποθετηρίου **azure-iot-πύλης-sdk** .
2. Εκτελέστε την ακόλουθη εντολή:

    ```
    ./build/samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ./samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```

3. Μπορείτε να χρησιμοποιήσετε την [Εξερεύνηση συσκευή ή από την Εξερεύνηση iothub] [ lnk-explorer-tools] εργαλείο για την παρακολούθηση της τα μηνύματα που λαμβάνει IoT διανομέα από την πύλη.

## <a name="next-steps"></a>Επόμενα βήματα

Εάν θέλετε να αποκτήσετε μια πιο σύνθετη Κατανόηση του SDK πύλης IoT και να πειραματιστείτε με ορισμένα παραδείγματα κώδικα, επισκεφθείτε τα παρακάτω προγράμματα εκμάθησης για προγραμματιστές και τους πόρους:

- [Αποστολή μηνυμάτων συσκευή-cloud από μια πραγματική συσκευή με το SDK IoT πύλης][lnk-physical-device]
- [SDK Azure IoT πύλης][lnk-gateway-sdk]

Για να εξερευνήσετε περαιτέρω τις δυνατότητες του IoT διανομέα, ανατρέξτε στα θέματα:

- [Οδηγός για προγραμματιστές][lnk-devguide]
- [Ασφαλούς τη λύση IoT σας από το μηδέν προς τα επάνω][lnk-securing]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-create-hub]: iot-hub-create-through-portal.md