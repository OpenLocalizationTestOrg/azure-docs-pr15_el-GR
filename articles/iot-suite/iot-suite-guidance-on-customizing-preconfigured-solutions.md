<properties
    pageTitle="Προσαρμογή προ-διαμορφωμένες λύσεις | Microsoft Azure"
    description="Παρέχει οδηγίες σχετικά με τον τρόπο για να προσαρμόσετε τις λύσεις Azure IoT οικογένεια προ-διαμορφωμένες."
    services=""
    suite="iot-suite"
    documentationCenter=".net"
    authors="aguilaaj"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/11/2016"
     ms.author="aguilaaj"/>

# <a name="customize-a-preconfigured-solution"></a>Προσαρμογή προδιαμορφωμένο λύσης

Τις προκαθορισμένες λύσεις που παρέχονται με την οικογένεια IoT Azure δείχνουν τις υπηρεσίες κατά την οικογένεια συνεργάζονται για να παρέχουν μια λύση για να ολοκληρωμένες. Από αυτό το σημείο εκκίνησης, υπάρχουν διάφορα σημεία όπου μπορείτε να επεκτείνετε και προσαρμογή της λύσης για συγκεκριμένα σενάρια. Οι παρακάτω ενότητες περιγράφουν αυτά τα κοινά σημεία προσαρμογής.

## <a name="finding-the-source-code"></a>Μπορείτε να βρείτε τον πηγαίο κώδικα

Ο κωδικός προέλευσης για τις προκαθορισμένες λύσεις είναι διαθέσιμη στο GitHub των εξής αποθετηρίων:

- Απομακρυσμένη παρακολούθηση: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- Πρόβλεψης συντήρηση: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

Ο κωδικός προέλευσης για τις προκαθορισμένες λύσεις παρέχεται για μια επίδειξη πρακτικές που χρησιμοποιούνται για την υλοποίηση τη λειτουργικότητα για ολοκληρωμένες μια λύση IoT χρησιμοποιώντας οικογένεια IoT Azure και τα μοτίβα. Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με το πώς μπορείτε να δημιουργήσετε και να αναπτύξετε τις λύσεις των αποθετηρίων GitHub.

## <a name="changing-the-preconfigured-rules"></a>Αλλαγή των προδιαμορφωμένο κανόνων

Η απομακρυσμένη λύση παρακολούθησης περιλαμβάνει τρία [Azure ανάλυση ροής](https://azure.microsoft.com/services/stream-analytics/) εργασιών για την υλοποίηση της συσκευής πληροφορίες, τηλεμετρίας και κανόνες λογικής εμφανίζονται για τη λύση.

Τις εργασίες ανάλυσης τρεις ροής και τους σύνταξη περιγράφεται σε βάθος στο το [απομακρυσμένο παρακολούθηση προδιαμορφωμένο λύση αναλυτικές οδηγίες](iot-suite-remote-monitoring-sample-walkthrough.md). 

Μπορείτε να επεξεργαστείτε αυτές τις εργασίες απευθείας για την τροποποίηση της λογικής ή Προσθήκη λογικής συγκεκριμένες για το σενάριό σας. Μπορείτε να βρείτε τις εργασίες ροής ανάλυση ως εξής:
 
1. Μεταβείτε στην [πύλη του Azure](https://portal.azure.com).
2. Μεταβείτε στην ομάδα πόρων με το ίδιο όνομα με τη λύση IoT σας. 
3. Επιλέξτε το έργο Azure ροή ανάλυσης που θέλετε να τροποποιήσετε. 
4. Διακοπή της εργασίας, επιλέγοντας **Διακοπή**του συνόλου των εντολών. 
5. Επεξεργαστείτε το εισόδων ερωτήματος και εξόδους.

    Μια απλή τροποποίηση είναι να αλλάξετε το ερώτημα για το έργο **κανόνες** για να χρησιμοποιήσετε μια **"<"** αντί για ένα **">"**. Θα εξακολουθούν να εμφανίζουν την πύλη λύση **">"** όταν μπορείτε να επεξεργαστείτε έναν κανόνα, αλλά θα παρατηρήσετε η συμπεριφορά είναι ανεστραμμένο λόγω την αλλαγή της υποκείμενης εργασίας.

6. Εκκίνηση της εργασίας

> [AZURE.NOTE] Στον πίνακα εργαλείων απομακρυσμένης παρακολούθησης εξαρτάται από συγκεκριμένα δεδομένα, ώστε να τροποποιούν τις εργασίες μπορούν να προκαλέσουν τον πίνακα εργαλείων αποτυχία.

## <a name="adding-your-own-rules"></a>Προσθήκη κανόνων το δικό σας

Εκτός από την αλλαγή τις προκαθορισμένες εργασίες Azure ροή ανάλυση, μπορείτε να χρησιμοποιήσετε την πύλη του Azure για να προσθέσετε νέες εργασίες ή να προσθέσετε νέα ερωτήματα σε υπάρχουσες εργασίες.

## <a name="customizing-devices"></a>Προσαρμογή συσκευές

Μία από τις πιο συνηθισμένες δραστηριότητες επέκταση λειτουργεί με συσκευές ειδικά για το σενάριό σας. Υπάρχουν διάφορες μεθόδους για την εργασία με συσκευές. Τις παρακάτω μεθόδους περιλαμβάνουν τροποποίηση συσκευή προσομοιωμένη ώστε να ταιριάζει με το σενάριό σας ή τη χρήση του [SDK συσκευή IoT][] να συνδέσετε τη συσκευή φυσικής στη λύση.

Για οδηγίες βήμα προς βήμα για να προσθέσετε συσκευές σε απομακρυσμένο λύση προδιαμορφωμένο παρακολούθησης, ανατρέξτε στο θέμα [Iot οικογένεια προγραμμάτων τη σύνδεση συσκευές](iot-suite-connecting-devices.md) και το [Απομακρυσμένο παρακολούθησης δείγμα SDK C](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring) που έχει σχεδιαστεί για λειτουργία με το απομακρυσμένο προδιαμορφωμένο λύση παρακολούθησης.

### <a name="creating-your-own-simulated-device"></a>Δημιουργία της δικής σας προσομοιωμένη συσκευή

Περιλαμβάνονται στον απομακρυσμένο παρακολούθησης λύση πηγαίο κώδικα (που αναφέρονται παραπάνω), είναι προσομοιωτή .NET. Simulator αυτό είναι ένα παρασχεθεί ως μέρος της λύσης και μπορεί να τροποποιηθεί για να στείλετε διαφορετικά μετα-δεδομένα, τηλεμετρίας ή απάντηση σε διαφορετικές εντολές.

Το προδιαμορφωμένο simulator της απομακρυσμένης παρακολούθησης προδιαμορφωμένο λύσης είναι μια πιο ψυχρή συσκευή που εκπέμπει θερμοκρασίας και υγρασίας τηλεμετρίας, μπορείτε να τροποποιήσετε το simulator του έργου [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) όταν έχετε forked του αποθετηρίου GitHub.

### <a name="available-locations-for-simulated-devices"></a>Διαθέσιμες θέσεις για προσομοιωμένη συσκευές

Είναι το προεπιλεγμένο σύνολο θέσεις στο Σιάτλ/Redmond, Washington, των Ηνωμένων Πολιτειών. Μπορείτε να αλλάξετε αυτές τις θέσεις στο [SampleDeviceFactory.cs][lnk-sample-device-factory].


### <a name="building-and-using-your-own-physical-device"></a>Δημιουργία και χρήση τη δική σας συσκευή (φυσική)

Το [Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) παρέχουν βιβλιοθηκών για τη σύνδεση πολυάριθμους τύπους συσκευή (γλώσσες και λειτουργικά συστήματα) σε IoT λύσεις.

## <a name="modifying-dashboard-limits"></a>Η τροποποίηση όρια πίνακα εργαλείων

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Αριθμός των συσκευών που εμφανίζεται στην αναπτυσσόμενη λίστα πίνακα εργαλείων

Η προεπιλογή είναι 200. Μπορείτε να αλλάξετε τον αριθμό αυτόν στον [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Αριθμό PIN, προκειμένου να εμφανίσετε σε στοιχείο ελέγχου χάρτη Bing

Η προεπιλογή είναι 200. Μπορείτε να αλλάξετε τον αριθμό αυτόν στον [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Χρονική περίοδο τηλεμετρίας γραφήματος

Η προεπιλογή είναι 10 λεπτά. Μπορείτε να το αλλάξετε σε [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Μη αυτόματη ρύθμιση ρόλων εφαρμογής

Η παρακάτω διαδικασία περιγράφει τον τρόπο προσθήκης **διαχείρισης** και **μόνο για ανάγνωση** ρόλους εφαρμογής σε μια προκαθορισμένη λύση. Σημειώστε ότι προκαθορισμένες λύσεις που έχουν παρασχεθεί από την τοποθεσία azureiotsuite.com ήδη περιλαμβάνουν τους ρόλους **διαχειριστή** και **μόνο για ανάγνωση** .

Τα μέλη του ρόλου **μόνο για ανάγνωση** να δείτε τον πίνακα εργαλείων και λίστα συσκευών, αλλά δεν μπορείτε να προσθέσετε συσκευές, αλλαγή συσκευής χαρακτηριστικά ή αποστολή εντολών.  Τα μέλη του ρόλου **διαχειριστή** έχουν πλήρη πρόσβαση σε όλες τις λειτουργίες του στη λύση.

1. Μεταβείτε στην [πύλη του Azure κλασική][lnk-classic-portal].

2. Επιλέξτε την **υπηρεσία καταλόγου Active Directory**.

3. Κάντε κλικ στο όνομα του μισθωτή AAD που χρησιμοποιήσατε κατά την παροχή της υπηρεσίας τη λύση σας.

4. Κάντε κλικ στην επιλογή **εφαρμογές**.

5. Κάντε κλικ στο όνομα της εφαρμογής που να ταιριάζει με το όνομα της λύσης προδιαμορφωμένο. Εάν δεν βλέπετε την εφαρμογή σας στη λίστα, επιλέξτε **εφαρμογές που ανήκει η εταιρεία μου** στο πλαίσιο **Εμφάνιση** αναπτυσσόμενη λίστα και κάντε κλικ στο σημάδι ελέγχου.

6.  Στο κάτω μέρος της σελίδας, κάντε κλικ στην επιλογή **Διαχείριση δήλωσης** και στη συνέχεια, να **Κάνετε λήψη δήλωσης**.

7. Αυτό γίνεται λήψη ενός αρχείου .json στον τοπικό υπολογιστή.  Άνοιγμα αυτού του αρχείου για επεξεργασία σε ένα πρόγραμμα επεξεργασίας κειμένου της επιλογής σας.

8. Στην τρίτη γραμμή του αρχείου .json, θα βρείτε:

  ```
  "appRoles" : [],
  ```
  Αντικαταστήσετε με τα εξής:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Αποθηκεύστε το αρχείο ενημερωμένων .json (μπορείτε να αντικαταστήσετε το υπάρχον αρχείο).

10.  Στην πύλη διαχείρισης του Azure, στο κάτω μέρος της σελίδας, επιλέξτε **Διαχείριση δήλωσης** , στη συνέχεια, **Αποστείλετε δήλωσης** για να αποστείλετε το αρχείο .json που αποθηκεύσατε στο προηγούμενο βήμα.

11. Τώρα που έχετε προσθέσει τους ρόλους **διαχειριστή** και **μόνο για ανάγνωση** στην εφαρμογή σας.

12. Για να αντιστοιχίσετε έναν από αυτούς τους ρόλους σε ένα χρήστη στον κατάλογό σας, ανατρέξτε στο θέμα [δικαιωμάτων στην τοποθεσία azureiotsuite.com][lnk-permissions].

## <a name="feedback"></a>Σχόλια

Έχετε μια προσαρμογή που θέλετε να δείτε τα εμπλεκόμενα σε αυτό το έγγραφο; Προσθέστε τη δυνατότητα προτάσεις [Φωνής χρήστη](https://feedback.azure.com/forums/321918-azure-iot)ή το σχόλιο στο παρακάτω σε αυτό το άρθρο. 

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με τις επιλογές για την προσαρμογή τις προκαθορισμένες λύσεις, ανατρέξτε στα θέματα:

- [Σύνδεση λογική εφαρμογής τη λύση σας Azure IoT οικογένεια απομακρυσμένη παρακολούθηση προ-διαμορφωμένες][lnk-logicapp]
- [Χρήση δυναμικής τηλεμετρίας με το απομακρυσμένο προδιαμορφωμένο λύση παρακολούθησης][lnk-dynamic]
- [Συσκευή πληροφορίες μετα-δεδομένων σε απομακρυσμένη παρακολούθηση προ-διαμορφωμένες λύσης][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[Συσκευή IoT SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com