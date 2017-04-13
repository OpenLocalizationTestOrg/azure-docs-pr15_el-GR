<properties
 pageTitle="Λειτουργίες διανομέα IoT παρακολούθησης"
 description="Μια επισκόπηση των λειτουργιών διανομέα IoT Azure παρακολούθησης, μπορείτε να παρακολουθείτε την κατάσταση των λειτουργιών στο κέντρο σας IoT σε πραγματικό χρόνο"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-operations-monitoring"></a>Εισαγωγή στις λειτουργίες παρακολούθησης

Λειτουργίες διανομέα IoT παρακολούθηση σάς επιτρέπει να παρακολουθείτε την κατάσταση των λειτουργιών στο κέντρο σας IoT σε πραγματικό χρόνο. Ενότητα IoT παρακολουθεί συμβάντα σε διάφορες κατηγορίες των λειτουργιών, και μπορείτε να επιλέξετε σε αποστολή συμβάντων από μία ή περισσότερες κατηγορίες σε ένα τελικό σημείο από το Κέντρο IoT σας για επεξεργασία. Μπορείτε να παρακολουθείτε τα δεδομένα για σφάλματα ή να ρυθμίσετε πιο σύνθετη επεξεργασία που βασίζεται σε μοτίβα δεδομένων.

Ο διανομέας IoT παρακολουθεί πέντε κατηγορίες συμβάντων:

- Λειτουργίες ταυτότητα συσκευής
- Συσκευή τηλεμετρίας
- Εντολές cloud-σε-συσκευή
- Συνδέσεις
- Αποστολή αρχείου

## <a name="how-to-enable-operations-monitoring"></a>Πώς μπορείτε να ενεργοποιήσετε τις λειτουργίες παρακολούθησης

1. Δημιουργήστε ένα διανομέα IoT. Μπορείτε να βρείτε οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα διανομέα IoT τα [Γρήγορα αποτελέσματα] [ lnk-get-started] τον οδηγό.

2. Ανοίξτε το blade από το Κέντρο IoT σας. Από εκεί, κάντε κλικ στην επιλογή **Παρακολούθηση Λειτουργίες**.

    ![][1]

3. Επιλέξτε τις κατηγορίες παρακολούθησης που θέλετε να παρακολουθήσετε και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**. Τα συμβάντα είναι διαθέσιμα για ανάγνωση από το τελικό σημείο διανομέα συμβατό με το συμβάν που παρατίθενται στις **ρυθμίσεις παρακολούθησης**. Το τελικό σημείο διανομέα IoT ονομάζεται `messages/operationsmonitoringevents`.

    ![][2]

## <a name="event-categories-and-how-to-use-them"></a>Κατηγορίες συμβάντων και πώς μπορείτε να τις χρησιμοποιήσετε

Κάθε λειτουργίες παρακολούθησης κομμάτια κατηγορία διαφορετικό τύπο επικοινωνίας με το Κέντρο IoT και παρακολούθησης κάθε κατηγορία έχει ένα σχήμα που ορίζει δομής συμβάντα σε αυτή την κατηγορία.

### <a name="device-identity-operations"></a>Λειτουργίες ταυτότητα συσκευής

Στην κατηγορία λειτουργίες ταυτότητας συσκευή παρακολουθεί σφάλματα που προκύπτουν όταν προσπαθείτε να δημιουργήσετε, ενημέρωση ή διαγράψτε μια καταχώρηση στο μητρώο την ταυτότητά σας IoT διανομέα. Παρακολούθηση αυτής της κατηγορίας είναι χρήσιμη για παροχή σενάρια.

    {
        "time": "UTC timestamp",
         "operationName": "create",
         "category": "DeviceIdentityOperations",
         "level": "Error",
         "statusCode": 4XX,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "durationMs": 1234,
         "userAgent": "userAgent",
         "sharedAccessPolicy": "accessPolicy"
    }

### <a name="device-telemetry"></a>Συσκευή τηλεμετρίας

Στην κατηγορία τηλεμετρίας συσκευή παρακολουθεί σφάλματα που προκύπτουν στην ενότητα IoT και σχετίζονται με τη διαδικασία τηλεμετρίας. Αυτή η κατηγορία περιλαμβάνει σφάλματα που προκύπτουν κατά την αποστολή τηλεμετρίας συμβάντα (όπως περιορισμού) και λήψη τηλεμετρίας συμβάντα (όπως μη εξουσιοδοτημένη reader). Σημειώστε ότι αυτή η κατηγορία δεν είναι δυνατό να ενημερωθείτε σφαλμάτων που προκαλούνται από κώδικα που εκτελείται στην ίδια τη συσκευή.

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "time": "UTC timestamp",
         "operationName": "ingress",
         "category": "DeviceTelemetry",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": "UTC timestamp"
    }

### <a name="cloud-to-device-commands"></a>Εντολές cloud-σε-συσκευή

Στην κατηγορία εντολές cloud-σε-συσκευή παρακολουθεί σφάλματα που προκύπτουν στην ενότητα IoT και σχετίζονται με τη διαδικασία εντολή συσκευή. Αυτή η κατηγορία περιλαμβάνει σφάλματα που προκύπτουν όταν αποστολή εντολές (όπως μη εξουσιοδοτημένη αποστολέα), λαμβάνει εντολές (όπως παράδοσης υπέρβαση του πλήθους μεταπηδήσεων) και λαμβάνετε σχόλια εντολή (όπως σχόλια έχει λήξει). Αυτή η κατηγορία δεν αντιμετωπίσει σφάλματα από μια συσκευή που χειρίζεται σωστά μια εντολή, εάν η εντολή παραδόθηκε με επιτυχία.

    {
         "messageSizeInBytes": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "deliveryAcknowledgement": 0,
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "C2DCommands",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": “UTC timestamp"
    }

### <a name="connections"></a>Συνδέσεις

Στην κατηγορία συνδέσεων παρακολουθεί σφάλματα που προκύπτουν όταν συσκευές σύνδεση ή αποσύνδεση από μια ενότητα IoT. Παρακολούθηση αυτής της κατηγορίας είναι χρήσιμη για τον εντοπισμό προσπαθειών μη εξουσιοδοτημένη σύνδεσης και για την παρακολούθηση της σύνδεσης που έχουν χαθεί για συσκευές στις περιοχές κακής σύνδεσης.

    {
         "durationMs": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "deviceConnect",
         "category": "Connections",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID"
    }

### <a name="file-uploads"></a>Αποστολή αρχείου

Στην κατηγορία αποστολής αρχείου παρακολουθεί σφάλματα που προκύπτουν στην ενότητα IoT και σχετίζονται με λειτουργίες αποστολής αρχείου. Αυτή η κατηγορία περιλαμβάνει σφάλματα που προκύπτουν με το URI συσχετισμών Ασφαλείας (όπως όταν λήγει πριν από την ενότητα της μια ολοκληρωμένη αποστολή ειδοποιεί μια συσκευή), αποτυχίας αποστολές αναφερθεί από τη συσκευή και, όταν ένα αρχείο που δεν βρίσκεται στο χώρο αποθήκευσης κατά τη δημιουργία μηνύματος ειδοποίησης IoT διανομέα. Σημειώστε ότι αυτή η κατηγορία δεν είναι δυνατό να ενημερωθείτε σφάλματα που προκύπτουν απευθείας ενώ η συσκευή είναι η αποστολή ενός αρχείου με το χώρο αποθήκευσης.

    {
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "HTTP",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "fileUpload",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "blobUri": "http//bloburi.com",
         "durationMs": 1234
    }

## <a name="next-steps"></a>Επόμενα βήματα

Για να εξερευνήσετε περαιτέρω τις δυνατότητες του IoT διανομέα, ανατρέξτε στα θέματα:

- [Οδηγός για προγραμματιστές][lnk-devguide]
- [Προσομοίωση μια συσκευή με το SDK IoT πύλης][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md