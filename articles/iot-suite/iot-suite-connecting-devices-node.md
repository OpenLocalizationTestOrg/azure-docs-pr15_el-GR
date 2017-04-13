<properties
   pageTitle="Συνδέστε μια συσκευή με χρήση Node.js | Microsoft Azure"
   description="Περιγράφει τον τρόπο για να συνδέσετε μια συσκευή για την οικογένεια προγραμμάτων IoT Azure προ-διαμορφωμένες απομακρυσμένο λύση παρακολούθησης με μια εφαρμογή που έχει συνταχθεί σε Node.js."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Συνδέστε τη συσκευή με το απομακρυσμένο προδιαμορφωμένο λύση παρακολούθησης (Node.js)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Δημιουργία και εκτέλεση της λύσης δείγμα node.js

1. Για να κλωνοποίηση του αποθετηρίου GitHub *Microsoft Azure IoT SDK* και να εγκαταστήσετε το *Microsoft Azure IoT συσκευή SDK για Node.js* στο περιβάλλον της επιφάνειας εργασίας σας, ακολουθήστε την [Προετοιμασία περιβάλλον ανάπτυξής σας] [ lnk-github-prepare] οδηγίες.

2. Από το τοπικό αντίγραφο της το [azure-iot-sdks] [ lnk-github-repo] αποθετήριο, αντιγράψτε τα παρακάτω δύο αρχεία από το φάκελο κόμβο/συσκευή δείγματα σε ένα φάκελο στη συσκευή σας:

  - Packages.JSON
  - remote_monitoring.js

3. Ανοίξτε το αρχείο remote_monitoring.js και αναζητήστε τη μεταβλητή παρακάτω:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

4. Αντικαταστήστε **[συμβολοσειρά σύνδεσης συσκευή διανομέα IoT]** με τη συμβολοσειρά σύνδεσης τη συσκευή σας. Μπορείτε να βρείτε τις τιμές για το Κέντρο IoT σας hostname, αναγνωριστικό συσκευής και κλειδί συσκευής στον απομακρυσμένο πίνακα εργαλείων παρακολούθησης λύση. Μια συμβολοσειρά σύνδεσης συσκευή έχει την παρακάτω μορφή:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Εάν το όνομα κεντρικού υπολογιστή IoT διανομέα είναι **contoso** και το αναγνωριστικό συσκευής είναι **mydevice**, η συμβολοσειρά σύνδεσης έχει την παρακάτω:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

5. Αποθηκεύστε το αρχείο. Εκτελέστε τις ακόλουθες εντολές σε μια γραμμή εντολών στο φάκελο που περιέχει αυτά τα αρχεία για να εγκαταστήσετε τα απαραίτητα πακέτα και, στη συνέχεια, εκτελέστε το δείγμα εφαρμογής:

    ```
    npm install --save
    node remote_monitoring.js
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdks
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md