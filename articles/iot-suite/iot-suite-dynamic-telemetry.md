<properties
    pageTitle="Χρήση του δυναμικού τηλεμετρίας | Microsoft Azure"
    description="Ακολουθήστε αυτό το πρόγραμμα εκμάθησης για να μάθετε πώς μπορείτε να χρησιμοποιήσετε δυναμικές τηλεμετρίας με το απομακρυσμένο προδιαμορφωμένο λύση παρακολούθησης."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="dobett"/>

# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Χρήση δυναμικής τηλεμετρίας με το απομακρυσμένο προδιαμορφωμένο λύση παρακολούθησης

## <a name="introduction"></a>Εισαγωγή

Δυναμική τηλεμετρίας σάς επιτρέπει να απεικόνιση οποιαδήποτε τηλεμετρίας που αποστέλλονται σε απομακρυσμένο λύση προδιαμορφωμένο παρακολούθησης. Προσομοιωμένη συσκευές που αναπτύσσουν με τη λύση προδιαμορφωμένο αποστολή θερμοκρασίας και υγρασίας τηλεμετρίας, τα οποία μπορείτε να οπτικοποιήσετε στον πίνακα εργαλείων. Εάν προσαρμόσετε υπάρχουσες προσομοιωμένη συσκευές, δημιουργήστε νέες συσκευές προσομοιωμένη ή σύνδεση φυσικής συσκευές προδιαμορφωμένο λύση μπορείτε να στείλετε άλλες τιμές τηλεμετρίας όπως η εξωτερική θερμοκρασία, RPM ή ταχύτητα ανέμου. Μπορείτε, στη συνέχεια, να απεικονίσετε οπτικά αυτό επιπλέον τηλεμετρίας στον πίνακα εργαλείων.

Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί μια απλή συσκευή προσομοιωμένη Node.js που μπορείτε να τροποποιήσετε εύκολα να πειραματιστείτε με δυναμική τηλεμετρίας.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, θα πρέπει:

- Μια ενεργή συνδρομή Azure. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστική έκδοση][lnk_free_trial].
- [Node.js] [ lnk-node] έκδοση 0.12.x ή νεότερη έκδοση.

Μπορείτε να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης σε οποιοδήποτε λειτουργικό σύστημα, όπως το Windows ή Linux, όπου μπορείτε να εγκαταστήσετε το Node.js.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="configure-the-nodejs-simulated-device"></a>Ρύθμιση παραμέτρων της συσκευής προσομοιωμένη Node.js

1. Στον απομακρυσμένο πίνακα εργαλείων παρακολούθησης, κάντε κλικ στο κουμπί **+ Προσθήκη συσκευής** και, στη συνέχεια, προσθέστε μια προσαρμοσμένη συσκευή. Δημιουργήστε μια σημείωση από την ενότητα IoT όνομα κεντρικού υπολογιστή, το αναγνωριστικό συσκευής και κλειδί συσκευής. Χρειάζεστε τις παρακάτω σε αυτό το πρόγραμμα εκμάθησης κατά την προετοιμασία της εφαρμογής remote_monitoring.js συσκευή-πελάτη.

2. Βεβαιωθείτε ότι η έκδοση Node.js 0.12.x ή νεότερη έκδοση είναι εγκατεστημένο στον υπολογιστή σας στην ανάπτυξη. Εκτέλεση `node --version` σε μια γραμμή εντολών ή σε κέλυφος για να ελέγξετε την έκδοση. Για πληροφορίες σχετικά με τη χρήση ενός διευθυντή του πακέτου εγκατάστασης Node.js σε Linux, ανατρέξτε στο θέμα [Node.js κατά την εγκατάσταση μέσω διαχείρισης πακέτου][node-linux].

3. Όταν έχετε εγκαταστήσει Node.js, κλωνοποίηση την πιο πρόσφατη έκδοση του [azure-iot-sdks] [ lnk-github-repo] αποθετήριο δεδομένων στον υπολογιστή σας στην ανάπτυξη. Χρησιμοποιείτε πάντα την **κύρια** κλάδο για την πιο πρόσφατη έκδοση του τις βιβλιοθήκες και δείγματα.

4. Από το τοπικό αντίγραφο της το [azure-iot-sdks] [ lnk-github-repo] αποθετήριο, αντίγραφο των παρακάτω δύο αρχεία από το φάκελο κόμβο/συσκευή δείγματα σε έναν κενό φάκελο στον υπολογιστή σας στην ανάπτυξη:

  - Packages.JSON
  - remote_monitoring.js

5. Ανοίξτε το αρχείο remote_monitoring.js και αναζητήστε τον ορισμό της μεταβλητής παρακάτω:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. Αντικαταστήστε **[συμβολοσειρά σύνδεσης συσκευή διανομέα IoT]** με τη συμβολοσειρά σύνδεσης τη συσκευή σας. Χρησιμοποιήστε τις τιμές για όνομα κεντρικού υπολογιστή IoT διανομέα, αναγνωριστικό συσκευής και κλειδί συσκευής που κάνατε σε σημείωση της στο βήμα 1. Μια συμβολοσειρά σύνδεσης συσκευή έχει την παρακάτω μορφή:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Εάν το όνομα κεντρικού υπολογιστή IoT διανομέα είναι **contoso** και το αναγνωριστικό συσκευής είναι **mydevice**, η συμβολοσειρά σύνδεσης μοιάζει με τα εξής:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Αποθηκεύστε το αρχείο. Εκτελέστε τις ακόλουθες εντολές σε κέλυφος ή εντολών στο φάκελο που περιέχει αυτά τα αρχεία για να εγκαταστήσετε τα απαραίτητα πακέτα και, στη συνέχεια, εκτελέστε το δείγμα εφαρμογής:

    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Παρατηρήστε δυναμικής τηλεμετρίας σε δράση

Πίνακας εργαλείων εμφανίζει τη θερμοκρασία και υγρασία τηλεμετρίας από υπάρχουσες προσομοιωμένη συσκευές:

![Τον προεπιλεγμένο πίνακα εργαλείων][image1]

Εάν επιλέξετε τη συσκευή προσομοιωμένη Node.js εκτελέσατε στην προηγούμενη ενότητα, βλέπετε θερμοκρασία, υγρασία και τηλεμετρίας εξωτερικών θερμοκρασίας:

![Προσθήκη εξωτερικών θερμοκρασίας στον πίνακα εργαλείων][image2]

Η απομακρυσμένη λύση παρακολούθησης αυτόματα εντοπίζει τον τύπο τηλεμετρίας επιπλέον εξωτερικών θερμοκρασίας και το προσθέτει στο γράφημα στον πίνακα εργαλείων.

## <a name="add-a-telemetry-type"></a>Προσθήκη τύπου τηλεμετρίας

Το επόμενο βήμα είναι να αντικαταστήσετε το τηλεμετρίας που δημιουργούνται από τη συσκευή προσομοιωμένη Node.js με ένα νέο σύνολο τιμών:

1. Διακόψτε τη συσκευή προσομοιωμένη Node.js, πληκτρολογώντας **Ctrl + C** στη γραμμή εντολών ή κελύφους σας.

2. Στο αρχείο remote_monitoring.js, μπορείτε να δείτε τις τιμές της βάσης δεδομένων για την υπάρχουσα θερμοκρασίας υγρασία και τηλεμετρίας εξωτερικών θερμοκρασίας. Προσθέστε μια τιμή βάσης δεδομένων για **rpm** ως εξής:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Η συσκευή προσομοιωμένη Node.js χρησιμοποιεί τη συνάρτηση **generateRandomIncrement** στο αρχείο remote_monitoring.js για να προσθέσετε μια διαβάθμιση τυχαία στις τιμές της βάσης δεδομένων. Τυχαία την τιμή **rpm** , προσθέτοντας μια γραμμή κώδικα μετά την υπάρχουσα randomizations ως εξής:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Προσθέστε τη νέα τιμή rpm το φορτίο JSON στη συσκευή σας στείλει σε διανομέα IoT:

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Εκτελέστε τη συσκευή προσομοιωμένη Node.js χρησιμοποιώντας την ακόλουθη εντολή:

    ```
    node remote_monitoring.js
    ```

6. Παρατηρήστε τον νέο τύπο τηλεμετρίας RPM που εμφανίζεται στο γράφημα στον πίνακα εργαλείων:

![Προσθήκη RPM στον πίνακα εργαλείων][image3]

> [AZURE.NOTE] Ίσως χρειαστεί να απενεργοποιήσετε και, στη συνέχεια, ενεργοποιήστε τη συσκευή Node.js στη σελίδα **συσκευές** στον πίνακα εργαλείων για να δείτε την αλλαγή αμέσως.

## <a name="customize-the-dashboard-display"></a>Προσαρμογή της εμφάνισης του πίνακα εργαλείων

Το μήνυμα **Συσκευή-πληροφορίες** μπορούν να περιλαμβάνουν μετα-δεδομένα σχετικά με το τηλεμετρίας να στείλετε τη συσκευή με IoT διανομέα. Αυτό μετα-δεδομένων να καθορίσετε τους τύπους τηλεμετρίας στέλνει στη συσκευή. Τροποποιήστε την τιμή **deviceMetaData** στο αρχείο remote_monitoring.js για να συμπεριλάβετε έναν ορισμό **Τηλεμετρίας** μετά από τον ορισμό **εντολές** . Το παρακάτω τμήμα κώδικα δείχνει τον ορισμό **εντολές** (θα πρέπει να προσθέσετε μια `,` μετά από τον ορισμό **εντολές** ):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] Η απομακρυσμένη λύση παρακολούθησης χρησιμοποιεί διάκριση πεζών-κεφαλαίων συμφωνία για να συγκρίνετε τον ορισμό μετα-δεδομένα με τα δεδομένα στη ροή τηλεμετρίας.

Προσθήκη ενός ορισμού **Τηλεμετρίας** , όπως φαίνεται στο προηγούμενο τμήμα κώδικα δεν αλλάζει τη συμπεριφορά του πίνακα εργαλείων. Ωστόσο, τα μετα-δεδομένα επίσης να συμπεριλάβετε ένα χαρακτηριστικό **εμφανιζόμενο όνομα** για να προσαρμόσετε την εμφάνιση στον πίνακα εργαλείων. Ενημερώστε τον ορισμό μετα-δεδομένα **Τηλεμετρίας** , όπως φαίνεται στην το παρακάτω τμήμα κώδικα:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Το παρακάτω στιγμιότυπο οθόνης εμφανίζει πώς αυτή η αλλαγή τροποποιεί το υπόμνημα στο γράφημα στον πίνακα εργαλείων:

![Προσαρμογή του υπομνήματος γραφήματος][image4]

> [AZURE.NOTE] Ίσως χρειαστεί να απενεργοποιήσετε και, στη συνέχεια, ενεργοποιήστε τη συσκευή Node.js στη σελίδα **συσκευές** στον πίνακα εργαλείων για να δείτε την αλλαγή αμέσως.

## <a name="filter-the-telemetry-types"></a>Φιλτράρισμα των τύπων τηλεμετρίας

Από προεπιλογή, το γράφημα στον πίνακα εργαλείων εμφανίζει κάθε σειρά δεδομένων στη ροή τηλεμετρίας. Μπορείτε να χρησιμοποιήσετε τα μετα-δεδομένα **Συσκευή-πληροφορίες** για να διακόψετε την εμφάνιση συγκεκριμένων τηλεμετρίας τύπων στο γράφημα. 

Για να κάνετε το γράφημα Εμφάνιση μόνο τηλεμετρίας θερμοκρασίας και υγρασίας, παραλείψτε **ExternalTemperature** από τα μετα-δεδομένα **Τηλεμετρίας** **Συσκευή-πληροφορίες** ως εξής:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

Η **Αθλητική θερμοκρασίας** δεν εμφανίζεται πλέον στο γράφημα:

![Φιλτράρετε την τηλεμετρίας στον πίνακα εργαλείων][image5]

Αυτή η αλλαγή επηρεάζει μόνο την εμφάνιση του γραφήματος. Οι τιμές δεδομένων **ExternalTemperature** εξακολουθούν να αποθηκεύονται και κάνατε διαθέσιμα για οποιονδήποτε επεξεργασία παρασκηνίου.

> [AZURE.NOTE] Ίσως χρειαστεί να απενεργοποιήσετε και, στη συνέχεια, ενεργοποιήστε τη συσκευή Node.js στη σελίδα **συσκευές** στον πίνακα εργαλείων για να δείτε την αλλαγή αμέσως.

## <a name="handle-errors"></a>Χειρισμού σφαλμάτων

Για μια ροή δεδομένων για να εμφανίζονται στο γράφημα, τον **τύπο** της στο τα μετα-δεδομένα **Συσκευή-πληροφορίες** πρέπει να συμφωνεί με τον τύπο δεδομένων από τις τιμές τηλεμετρίας. Για παράδειγμα, εάν τα μετα-δεδομένα Καθορίζει ότι ο **Τύπος** δεδομένων υγρασία είναι **int** και **διπλά** βρίσκεται στη ροή τηλεμετρίας, στη συνέχεια, την υγρασία τηλεμετρίας δεν εμφανίζονται στο γράφημα. Ωστόσο, οι τιμές **υγρασία** εξακολουθούν να αποθηκεύονται και κάνατε διαθέσιμα για οποιονδήποτε επεξεργασία παρασκηνίου.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που είδατε πώς μπορείτε να χρησιμοποιήσετε δυναμικές τηλεμετρίας, μπορείτε να μάθετε περισσότερα σχετικά με τον τρόπο τις προκαθορισμένες λύσεις χρήσης πληροφοριών συσκευής: [συσκευή πληροφορίες μετα-δεδομένων σε ο απομακρυσμένος παρακολούθηση προδιαμορφωμένο λύση][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks
