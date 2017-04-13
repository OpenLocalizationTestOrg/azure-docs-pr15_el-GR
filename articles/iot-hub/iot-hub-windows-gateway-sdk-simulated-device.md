<properties
    pageTitle="Προσομοίωση μια συσκευή με το SDK πύλης IoT | Microsoft Azure"
    description="Για την απεικόνιση αποστολή τηλεμετρίας από μια συσκευή προσομοιωμένη χρησιμοποιώντας το SDK Azure IoT πύλης χρησιμοποιώντας το Windows Azure οδηγίες IoT πύλης SDK."
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


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-windows"></a>Azure IoT πύλης SDK (beta) – αποστολή μηνυμάτων συσκευής στο cloud με προσομοιωμένη συσκευή με Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Δημιουργία και εκτέλεση του δείγματος

Πριν ξεκινήσετε, πρέπει να:

- [Ρυθμίστε το περιβάλλον ανάπτυξης] [ lnk-setupdevbox] για την εργασία με το SDK των Windows.
- [Δημιουργήστε ένα διανομέα IoT] [ lnk-create-hub] σε Azure συνδρομή σας, θα χρειαστεί το όνομα του Κέντρο σας για να ολοκληρώσετε αυτόν τον οδηγό. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό] [ lnk-free-trial] σε λίγα λεπτά.
- Προσθέστε δύο συσκευές με το διανομέα IoT και σημειώστε τα αναγνωριστικά και τα κλειδιά συσκευή. Μπορείτε να χρησιμοποιήσετε την [Εξερεύνηση συσκευή ή από την Εξερεύνηση iothub] [ lnk-explorer-tools] εργαλείο για να προσθέσετε τις συσκευές σας την ενότητα IoT που δημιουργήσατε στο προηγούμενο βήμα και να ανακτήσετε τους αριθμούς-κλειδιά.

Για να δημιουργήσετε το δείγμα:

1. Ανοίξτε μια γραμμή εντολών **για προγραμματιστές εντολών για VS2015** .
2. Μεταβείτε στον ριζικό φάκελο στο τοπικό αντίγραφο του αποθετηρίου **azure-iot-πύλης-sdk** .
3. Εκτελέστε το **Εργαλεία\\build.cmd** δέσμης ενεργειών. Αυτή η δέσμη ενεργειών δημιουργεί ένα αρχείο λύση Visual Studio, δημιουργεί τη λύση και εκτελεί τις δοκιμές. Μπορείτε να βρείτε τη λύση Visual Studio στο φάκελο **Δόμηση** στο τοπικό αντίγραφο του αποθετηρίου **azure-iot-πύλης-sdk** .

Για να εκτελέσετε το δείγμα:

Σε ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο **δείγματα\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** στο τοπικό αντίγραφο του αποθετηρίου **azure-iot-πύλης-sdk** . Αυτό το αρχείο ρυθμίζει τις ενότητες στο δείγμα πύλης:

- Η λειτουργική μονάδα **IoTHub** συνδέεται με το Κέντρο IoT σας. Θα πρέπει να ρυθμίσετε την αποστολή δεδομένων προς το Κέντρο IoT σας. Συγκεκριμένα, ορίστε την τιμή **IoTHubName** στο όνομα του Κέντρο σας IoT και ορίστε την τιμή **IoTHubSuffix** σε **azure devices.net**. Ορίστε την τιμή **μεταφοράς** σε μία από: "HTTP", "AMQP" ή "MQTT". Σημειώστε ότι τη συγκεκριμένη στιγμή, μόνο "HTTP" θα κάνετε κοινή χρήση μία σύνδεση TCP για όλα τα μηνύματα συσκευή. Εάν ορίσετε την τιμή "AMQP" ή "MQTT", η πύλη θα διατηρήσει μια ξεχωριστή σύνδεση TCP IoT διανομέα για κάθε συσκευή.
- Τη λειτουργική μονάδα **αντιστοίχισης** αντιστοιχίζει τις διευθύνσεις MAC προσομοιωμένη συσκευή σας ταυτότητες συσκευή IoT διανομέα. Βεβαιωθείτε ότι **deviceId** τιμές ταιριάζουν με τα αναγνωριστικά των δύο συσκευές που έχετε προσθέσει στο κέντρο σας IoT και ότι οι τιμές **deviceKey** περιέχουν τα πλήκτρα δύο συσκευές σας.
- Οι λειτουργικές μονάδες **BLE1** και **BLE2** είναι προσομοιωμένη συσκευές. Παρατηρήστε πώς τις διευθύνσεις MAC ταιριάζουν με τις τιμές της λειτουργικής μονάδας **αντιστοίχισης** .
- Η λειτουργική μονάδα **καταγραφής** καταγράφει τη δραστηριότητά σας πύλης σε ένα αρχείο.
- Τις τιμές **διαδρομή λειτουργική μονάδα** που φαίνεται παρακάτω λαμβάνεται ως δεδομένο ότι κλωνοποιηθεί του αποθετηρίου IoT πύλης SDK στη ρίζα της μονάδας δίσκου **C:** . Εάν έχετε κάνει λήψη του σε άλλη θέση, πρέπει να προσαρμόσετε τις τιμές **διαδρομή λειτουργικής μονάδας** αντίστοιχα.
- Ο πίνακας **συνδέσεων** στο κάτω μέρος του αρχείου JSON συνδέεται το λειτουργικές μονάδες **BLE1** και **BLE2** για τη λειτουργική μονάδα **αντιστοίχισης** και τη λειτουργική μονάδα **αντιστοίχισης** στη λειτουργική μονάδα **IoTHub** . Επίσης, εξασφαλίζει ότι όλα τα μηνύματα που καταγράφονται από τη λειτουργική μονάδα **καταγραφής** .

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\iothub\\Debug\\iothub_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\identitymap\\Debug\\identity_map_hl.dll",
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
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
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

1. Σε μια γραμμή εντολών, μεταβείτε στον ριζικό φάκελο του τοπικού αντιγράφου του αποθετηρίου **azure-iot-πύλης-sdk** .
2. Εκτελέστε την ακόλουθη εντολή:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. Μπορείτε να χρησιμοποιήσετε την [Εξερεύνηση συσκευή ή από την Εξερεύνηση iothub] [ lnk-explorer-tools] εργαλείο για την παρακολούθηση της τα μηνύματα που λαμβάνει IoT διανομέα από την πύλη.


## <a name="next-steps"></a>Επόμενα βήματα

Εάν θέλετε να αποκτήσετε μια πιο σύνθετη Κατανόηση του SDK πύλης IoT και να πειραματιστείτε με ορισμένα παραδείγματα κώδικα, επισκεφθείτε τα παρακάτω προγράμματα εκμάθησης για προγραμματιστές και τους πόρους:

- [Αποστολή μηνυμάτων συσκευής στο cloud από μια πραγματική συσκευή με το SDK IoT πύλης][lnk-physical-device]
- [SDK Azure IoT πύλης][lnk-gateway-sdk]

Για να εξερευνήσετε περαιτέρω τις δυνατότητες του IoT διανομέα, ανατρέξτε στα θέματα:

- [Οδηγός για προγραμματιστές][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 