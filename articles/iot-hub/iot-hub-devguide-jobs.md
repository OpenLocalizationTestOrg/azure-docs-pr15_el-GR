<properties
 pageTitle="Οδηγός για προγραμματιστές - έργα | Microsoft Azure"
 description="Οδηγός για προγραμματιστές Azure διανομέα IoT - Προγραμματισμός εργασιών για να εκτελέσετε σε πολλές συσκευές συνδεδεμένη σε διανομέα σας"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="schedule-jobs-on-multiple-devices-preview"></a>Προγραμματισμός εργασιών σε πολλές συσκευές (έκδοση preview)

## <a name="overview"></a>Επισκόπηση

Όπως περιγράφεται από προηγούμενη άρθρα, διανομέα IoT Azure επιτρέπει ένας αριθμός μπλοκ δόμησης ([διπλού ιδιότητες των συσκευών και ετικέτες] [ lnk-twin-devguide] και οι [μέθοδοι cloud-σε-συσκευή][lnk-dev-methods]).  Συνήθως, εφαρμογές παρασκηνίου IoT Ενεργοποίηση συσκευή διαχειριστές και τους τελεστές για να ενημερώσετε και να αλληλεπιδράσετε με συσκευές IoT μαζικά και την προγραμματισμένη ώρα.  Εργασίες συμπύκνωση την εκτέλεση της συσκευής διπλού ενημερώσεις και οι μέθοδοι C2D σε σχέση με ένα σύνολο των συσκευών κάθε φορά χρονοδιάγραμμα.  Για παράδειγμα, έναν τελεστή χρησιμοποιούσατε μια εφαρμογή παρασκηνίου που θα ξεκινήσετε και να παρακολουθήσετε μια εργασία σε ένα σύνολο των συσκευών κατά τη δημιουργία 43 και δάπεδο 3 κάθε φορά που δεν θα ενδέχεται να τις λειτουργίες του κτιρίου διακόψουν επανεκκίνηση.

### <a name="when-to-use"></a>Πότε να χρησιμοποιήσετε

Εξετάστε το ενδεχόμενο χρήσης των εργασιών όταν: μια λύση πίσω Τερματισμός ανάγκες να προγραμματίζουν και να παρακολουθήσετε την πρόοδο ένα από τα εξής σε ένα σύνολο από τη συσκευή:

- Ενημέρωση των ιδιοτήτων διπλού επιθυμητοί συσκευή
- Ενημέρωση ετικετών διπλού συσκευή
- Κλήση μεθόδων C2D

## <a name="job-lifecycle"></a>Κύκλος ζωής έργου

Εργασίες είναι ξεκίνησε από λύση παρασκηνίου και διατηρούνται από διανομέα IoT.  Μπορείτε να ξεκινήσετε μια εργασία μέσω ενός URI υπηρεσία τοποθεσία (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) και ερωτήματος για την πρόοδο σε μια εργασία Εκτέλεση μέσω μιας υπηρεσίας τοποθεσία URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`).  Όταν ξεκινά μια εργασία, υποβολή ερωτημάτων για εργασίες θα ενεργοποιήσετε την εφαρμογή παρασκηνίου για την ανανέωση της κατάστασης των εργασιών που εκτελούνται.

> [AZURE.NOTE] Όταν πραγματοποιείτε μια εργασία, τα ονόματα των ιδιοτήτων και τις τιμές μπορεί να περιέχει μόνο US-ASCII εκτυπώσιμη αλφαριθμητικό, εκτός από οποιαδήποτε στο εξής σύνολο: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

## <a name="reference-topics"></a>Αναφορά θέματα:

Τα παρακάτω θέματα αναφοράς παρέχει περισσότερες πληροφορίες σχετικά με τη χρήση εργασιών.

## <a name="jobs-to-execute-direct-methods"></a>Εργασίες για να εκτελέσετε απευθείας μεθόδους

Ακολουθεί τις λεπτομέρειες αίτηση HTTP 1.1 για την εκτέλεση μια [Άμεση μέθοδος] [ lnk-dev-methods] σε ένα σύνολο των συσκευών χρησιμοποιώντας μια εργασία:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>Εργασίες για να ενημερώσετε τις ιδιότητες των συσκευών διπλού

Ακολουθεί τις λεπτομέρειες αίτηση HTTP 1.1 για την ενημέρωση των ιδιοτήτων διπλού συσκευής χρησιμοποιώντας μια εργασία:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Υποβολή ερωτημάτων για την πρόοδο σε εργασίες

Τα παρακάτω είναι οι λεπτομέρειες αίτηση HTTP 1.1 για την [Υποβολή ερωτημάτων για τις εργασίες][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
Το continuationToken παρέχεται από την απάντηση.  

## <a name="jobs-properties"></a>Ιδιότητες εργασιών

Ακολουθεί μια λίστα με τις ιδιότητες και τις αντίστοιχες περιγραφές, τα οποία μπορούν να χρησιμοποιηθούν κατά την υποβολή ερωτήματος για εργασίες ή εργασία αποτελέσματα.

| Ιδιότητα | Περιγραφή |
| -------------- | -----------------|
| **jobId** | Εφαρμογή παρέχεται Αναγνωριστικό για την εργασία. |
| **ώρα έναρξης** | Εφαρμογή παρέχεται ώρα έναρξης (ISO 8601) για το έργο. |
| **ώρα λήξης** | Ο διανομέας IoT παρέχονται ημερομηνία (ISO 8601) για την ολοκλήρωση της εργασίας. Ισχύει μόνο όταν η εργασία φτάσει η κατάσταση "ολοκληρώθηκε". | 
| **Τύπος** | Οι τύποι εργασιών: |
| | **scheduledUpdateTwin**: μια εργασία που χρησιμοποιείται για την ενημέρωση ενός συνόλου διπλού επιθυμητοί ιδιότητες ή ετικέτες. |
| | **scheduledDeviceMethod**: μια εργασία που χρησιμοποιείται για την κλήση μιας μεθόδου συσκευή σε ένα σύνολο διπλού. |
| **κατάσταση** | Τρέχουσα κατάσταση του έργου. Πιθανές τιμές για την κατάσταση: |
| | **σε εκκρεμότητα** : και την αναμονή για να εντοπιστεί από την υπηρεσία εργασίας. |
| | **προγραμματισμένη** : έχει προγραμματιστεί για μια ώρα στο μέλλον. |
| | **Εκτέλεση** : αυτήν τη στιγμή ενεργού εργασίας. |
| | **ακυρώθηκε** : εργασία έχει ακυρωθεί. |
| | **απέτυχε η** : Απέτυχε η εργασία. |
| | **Ολοκλήρωση** : εργασία έχει ολοκληρωθεί. |
| **deviceJobStatistics** | Στατιστικά στοιχεία σχετικά με την εκτέλεση του έργου. |

Κατά την προεπισκόπηση, το αντικείμενο deviceJobStatistics είναι διαθέσιμο μόνο αφού ολοκληρωθεί η εργασία.

| Ιδιότητα | Περιγραφή |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | Αριθμός των συσκευών στην εργασία. |
| **deviceJobStatistics.failedCount** | Αριθμός των συσκευών, όπου η εργασία απέτυχε. |
| **deviceJobStatistics.succeededCount** | Αριθμός των συσκευών, όπου η εργασία ολοκληρώθηκε με επιτυχία. |
| **deviceJobStatistics.runningCount** | Αριθμός των συσκευών που εκτελούνται τη συγκεκριμένη στιγμή το έργο. |
| **deviceJobStatistics.pendingCount** | Αριθμός των συσκευών που είναι σε εκκρεμότητα για να εκτελέσετε την εργασία. |


### <a name="additional-reference-material"></a>Υλικό επιπλέον αναφοράς

Άλλα θέματα αναφοράς στον οδηγό για προγραμματιστές περιλαμβάνουν τα εξής:

- [Τα τελικά σημεία διανομέα IoT] [ lnk-endpoints] περιγράφει τα διάφορα τελικά σημεία που κάθε ενότητα IoT εκθέτει για διαχείριση χρόνου εκτέλεσης και λειτουργίες.
- [Περιορισμού και του ορίου] [ lnk-quotas] περιγράφει τα όρια που ισχύουν για την υπηρεσία IoT διανομέας και τη συμπεριφορά επιτάχυνσης να περιμένει όταν χρησιμοποιείτε την υπηρεσία.
- [SDK συσκευής και της υπηρεσίας διανομέα IoT] [ lnk-sdks] παραθέτει τα διάφορα γλώσσας SDK που μη χρήση κατά την ανάπτυξη και συσκευή και υπηρεσία εφαρμογές που αλληλεπιδρούν με IoT διανομέα.
- [Το ερώτημα γλώσσα για twins, μεθόδους και εργασίες] [ lnk-query] περιγράφει τη γλώσσα ερωτήματος που μπορείτε να χρησιμοποιήσετε για να ανακτήσετε πληροφορίες από διανομέα IoT σχετικά με τη συσκευή twins, μεθόδους και εργασίες.
- [Υποστήριξη IoT διανομέα MQTT] [ lnk-devguide-mqtt] παρέχει περισσότερες πληροφορίες σχετικά με την υποστήριξη IoT διανομέα για το πρωτόκολλο MQTT.

## <a name="next-steps"></a>Επόμενα βήματα

Εάν θέλετε να δοκιμάσετε μερικά από τα θέματα που περιγράφονται σε αυτό το άρθρο, μπορεί να σας ενδιαφέρουν στην παρακάτω ενότητα IoT εκμάθηση:

- [Χρονοδιάγραμμα και μετάδοση εργασίες][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
