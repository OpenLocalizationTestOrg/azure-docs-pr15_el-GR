<properties
   pageTitle="Συνδέστε μια συσκευή χρησιμοποιώντας C σε mbed | Microsoft Azure"
   description="Περιγράφει τον τρόπο για να συνδέσετε μια συσκευή για την οικογένεια προγραμμάτων IoT Azure προ-διαμορφωμένες απομακρυσμένο λύση παρακολούθησης με μια εφαρμογή που έχει συνταχθεί σε C που εκτελείται σε μια συσκευή mbed."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Συνδέστε τη συσκευή με το απομακρυσμένο προδιαμορφωμένο λύση παρακολούθησης (mbed)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Δημιουργία και εκτέλεση της λύσης δείγμα C

Οι παρακάτω οδηγίες περιγράφουν τα βήματα για τη σύνδεση ενός [Με δυνατότητα mbed FRDM-K64F Freescale] [ lnk-mbed-home] συσκευή για να το απομακρυσμένο λύση παρακολούθησης.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Συνδέστε τη συσκευή mbed στον υπολογιστή σας δικτύου και υπολογιστή

1. Συνδέστε τη συσκευή mbed στο δίκτυό σας χρησιμοποιώντας ένα καλώδιο Ethernet. Αυτό το βήμα είναι απαραίτητο, επειδή το δείγμα εφαρμογής απαιτεί πρόσβαση στο internet.

2. Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το mbed] [ lnk-mbed-getstarted] για να συνδέσετε τη συσκευή mbed επιτραπέζιο υπολογιστή σας.

3. Εάν στον επιτραπέζιο Υπολογιστή σας εκτελεί Windows, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων του Υπολογιστή] [ lnk-mbed-pcconnect] για να ρυθμίσετε τις παραμέτρους πρόσβασης σειριακή θύρα στη συσκευή σας mbed.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Δημιουργήσετε ένα έργο mbed και να εισαγάγετε το δείγμα κώδικα

1. Στο πρόγραμμα περιήγησης web, μεταβείτε στην [τοποθεσία για προγραμματιστές](https://developer.mbed.org/)του mbed.org. Εάν δεν έχετε πραγματοποιήσει προς τα επάνω, θα δείτε μια επιλογή για να δημιουργήσετε ένα λογαριασμό (είναι δωρεάν). Διαφορετικά, συνδεθείτε με τα διαπιστευτήριά σας λογαριασμό. Στη συνέχεια, κάντε κλικ στην επιλογή **πρόγραμμα μεταγλώττισης** στην επάνω δεξιά γωνία της σελίδας. Αυτή η ενέργεια σάς προσφέρει στο περιβάλλον εργασίας του *χώρου εργασίας* .

2. Βεβαιωθείτε ότι η πλατφόρμα υλικού που χρησιμοποιείτε εμφανίζεται στην επάνω δεξιά γωνία του παραθύρου ή κάντε κλικ στο εικονίδιο στην δεξιά γωνία για να επιλέξετε την πλατφόρμα υλικού.

3. Κάντε κλικ στην επιλογή **Εισαγωγή** από το κύριο μενού. Στη συνέχεια, κάντε κλικ στην επιλογή **κάντε κλικ εδώ** για να εισαγάγετε από διεύθυνση URL σύνδεση δίπλα στην επιλογή το λογότυπο της υδρογείου mbed.

    ![][6]

4. Στο αναδυόμενο παράθυρο, εισαγάγετε τη σύνδεση για το δείγμα https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ κώδικα, στη συνέχεια, κάντε κλικ στην επιλογή **Εισαγωγή**.

    ![][7]

5. Μπορείτε να δείτε στο παράθυρο του προγράμματος μεταγλώττισης mbed ότι εισαγωγή αυτού του έργου εισάγει επίσης διάφορες βιβλιοθήκες. Ορισμένα παρέχονται και διατηρούνται από την ομάδα Azure IoT ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), ενώ άλλοι είναι διαθέσιμες στον κατάλογο βιβλιοθήκες mbed βιβλιοθήκες τρίτων κατασκευαστών.

    ![][8]

6. Ανοίξτε το αρχείο remote_monitoring\remote_monitoring.c και εντοπίστε τον παρακάτω κώδικα στο αρχείο:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Αντικατάσταση [αναγνωριστικό συσκευής] και [κλειδί συσκευής], με τα δεδομένα σας συσκευή για να ενεργοποιήσετε το δείγμα προγράμματος για να συνδεθείτε με το Κέντρο IoT σας. Χρησιμοποιήστε το όνομα κεντρικού υπολογιστή διανομέα IoT για να αντικαταστήσετε το [όνομα IoTHub] και [επίθημα IoTHub, δηλαδή azure devices.net] σύμβολα κράτησης θέσης. Για παράδειγμα, εάν σας Hostname διανομέα IoT είναι **contoso.azure devices.net**, **contoso** είναι το **hubName** και όλα τα στοιχεία αφού είναι το **hubSuffix**:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### <a name="walk-through-the-code"></a>Θα καθοδηγήσουν τον κώδικα

Εάν σας ενδιαφέρει πώς λειτουργεί το πρόγραμμα, αυτή η ενότητα περιγράφει ορισμένα βασικά τμήματα του δείγματος κώδικα. Εάν θέλετε απλώς να εκτελέσετε τον κώδικα, προχωρήστε για τη [Δημιουργία και εκτέλεση του προγράμματος](#buildandrun).

#### <a name="defining-the-model"></a>Καθορίζει το μοντέλο

Αυτό το δείγμα χρησιμοποιεί ο [σειριοποιητής] [ lnk-serializer] βιβλιοθήκη για να ορίσετε ένα μοντέλο που καθορίζει τα μηνύματα αποστολή IoT διανομέα και τη λήψη από το Κέντρο IoT στη συσκευή. Σε αυτό το δείγμα, ο χώρος ονομάτων **Contoso** ορίζει μοντέλο **θερμοστάτη** που καθορίζει τα δεδομένα τηλεμετρίας **θερμοκρασίας**, **ExternalTemperature**και **υγρασία** μαζί με μετα-δεδομένων όπως το αναγνωριστικό συσκευής, ιδιότητες των συσκευών και τις εντολές που αποκρίνεται τη συσκευή:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

Σχετικά με τον καθορισμό μοντέλο είναι οι ορισμοί για τις εντολές **SetTemperature** και **SetHumidity** που αποκρίνεται τη συσκευή:

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>Σύνδεση του μοντέλου στη βιβλιοθήκη

Οι συναρτήσεις **sendMessage** και **IoTHubMessage** είναι στερεότυπο κώδικα για την αποστολή τηλεμετρίας από τη συσκευή και τη σύνδεση μηνύματα από διανομέα IoT με τα προγράμματα χειρισμού εντολή.

#### <a name="the-remotemonitoringrun-function"></a>Η συνάρτηση remote_monitoring_run

Συνάρτηση **κύριο** το πρόγραμμα ενεργοποιεί τη λειτουργία **remote_monitoring_run** κατά την εκκίνηση της εφαρμογής για την εκτέλεση της συσκευής συμπεριφορά ως ένα πρόγραμμα-πελάτη συσκευή IoT διανομέα. Αυτή η συνάρτηση **remote_monitoring_run** κυρίως αποτελείται από ζεύγη ένθετων συναρτήσεων:

- **πλατφόρμα\_προετοιμασία** και **πλατφόρμα\_deinit** εκτελούν λειτουργίες προετοιμασίας και τερματισμού συγκεκριμένης πλατφόρμας.
- **σειριοποιητής\_προετοιμασία** και **σειριοποιητής\_deinit** προετοιμασία και καταργήστε την προετοιμασία της βιβλιοθήκης σειριοποιητής.
- **IoTHubClient\_δημιουργία** και **IoTHubClient\_καταστροφής** Δημιουργήστε μια λαβή προγράμματος-πελάτη, **iotHubClientHandle**, χρησιμοποιώντας τα διαπιστευτήρια συσκευή για τη σύνδεση με το Κέντρο IoT σας.

Στην ενότητα κύρια στοιχεία της συνάρτησης **remote_monitoring_run** , το πρόγραμμα εκτελεί τις ακόλουθες λειτουργίες χρησιμοποιώντας τη λαβή **iotHubClientHandle** :

- Δημιουργεί μια παρουσία του μοντέλου θερμοστάτη Contoso και ρυθμίζει το μήνυμα επιστροφές κλήσης για τις δύο εντολές.
- Αποστέλλει πληροφορίες σχετικά με τη συσκευή μόνη της, συμπεριλαμβάνοντας τις εντολές που υποστηρίζει, για να σας IoT διανομέα, χρησιμοποιώντας τη βιβλιοθήκη σειριοποιητής. Όταν ο διανομέας λάβει αυτό το μήνυμα, αλλάζει η κατάσταση συσκευής στον πίνακα εργαλείων από **σε εκκρεμότητα** για να **εκτελείται**.
- Ξεκινά η επανάληψη **ενώ** που στέλνει θερμοκρασίας, εξωτερική θερμοκρασία και υγρασία τιμές σε διανομέα IoT ανά δευτερόλεπτο.

Για αναφορά, ακολουθεί ένα δείγμα μηνύματος **DeviceInfo** που αποστέλλονται σε διανομέα IoT κατά την εκκίνηση:

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Για αναφορά, ακολουθεί ένα δείγμα μηνύματος **Τηλεμετρίας** που αποστέλλονται σε διανομέα IoT:

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

Για αναφορά, ακολουθεί ένα δείγμα **εντολής** που λάβατε από διανομέα IoT:

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### <a name="build-and-run-the-program"></a>Δημιουργία και εκτέλεση του προγράμματος

1. Κάντε κλικ στην επιλογή **μεταγλώττιση** για να δημιουργήσετε το πρόγραμμα. Μπορείτε με ασφάλεια να Παράβλεψη προειδοποιήσεων οποιαδήποτε, αλλά εάν η Δόμηση δημιουργεί σφάλματα, διορθώστε τις πριν να συνεχίσετε.

2. Εάν η Δόμηση είναι επιτυχής, την τοποθεσία Web του προγράμματος μεταγλώττισης mbed δημιουργεί ένα αρχείο .bin με το όνομα του έργου σας και κάνει λήψη του στον τοπικό υπολογιστή σας. Αντιγράψτε το αρχείο .bin στη συσκευή. Αποθήκευση του αρχείου .bin στη συσκευή έχει ως αποτέλεσμα τη συσκευή για να κάνετε επανεκκίνηση και εκτελέστε το πρόγραμμα που περιέχονται στο αρχείο .bin. Με μη αυτόματο τρόπο, μπορείτε να επανεκκινήσετε το πρόγραμμα ανά πάσα στιγμή, πατώντας το κουμπί "Επαναφορά" από τη συσκευή mbed.

3. Συνδεθείτε στη συσκευή χρησιμοποιώντας μια εφαρμογή προγράμματος-πελάτη SSH, όπως PuTTY. Μπορείτε να προσδιορίσετε τη σειριακή θύρα που χρησιμοποιεί τη συσκευή σας, επιλέγοντας Διαχείριση συσκευών των Windows.

    ![][11]

4. Στο PuTTY, κάντε κλικ στον τύπο σύνδεσης **σειρά** . Η συσκευή συνήθως συνδέεται στο 9600 baud, οπότε εισαγάγετε 9600 στο πλαίσιο **ταχύτητα** . Στη συνέχεια, κάντε κλικ στην επιλογή **Άνοιγμα**.

5. Το πρόγραμμα ξεκινά εκτέλεση. Ίσως χρειαστεί να επαναφέρετε τον πίνακα (πατήστε το συνδυασμό πλήκτρων CTRL + Break ή πατήστε το κουμπί "Επαναφορά" στον πίνακα) Εάν το πρόγραμμα δεν ξεκινά αυτόματα όταν συνδέεστε.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer
