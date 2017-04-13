<properties
 pageTitle="Γρήγορα αποτελέσματα με το π φραμπουάζ 3 | Microsoft Azure"
 description="Γρήγορα αποτελέσματα με το π Raspberry 3, δημιουργήστε το Κέντρο IoT σας Azure και σύνδεση π σας με την ενότητα IoT"
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="get-started-with-raspberry-pi-3"></a>Γρήγορα αποτελέσματα με το π φραμπουάζ 3

Σε αυτό το πρόγραμμα εκμάθησης, ξεκινάτε με τα βασικά στοιχεία για εργασία με 3 π Raspberry που εκτελείται Raspbian εκμάθησης. Μπορείτε, στη συνέχεια, μάθετε πώς να απρόσκοπτα συνδέστε τις συσκευές σας στο cloud με το [Azure IoT διανομέα](iot-hub-what-is-iot-hub.md). Για Windows 10 IoT πυρήνα δείγματα, επισκεφθείτε [windowsondevices.com](http://www.windowsondevices.com/).

## <a name="lesson-1-configure-your-device"></a>Μάθημα 1: Ρύθμιση παραμέτρων τη συσκευή σας

![Διάγραμμα E2E Μάθημα 1](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

Σε αυτό το μάθημα, που ρυθμίσετε τη συσκευή σας Raspberry π 3 με λειτουργικό σύστημα, ρυθμίστε το περιβάλλον ανάπτυξης και να αναπτύξετε μια εφαρμογή για το π.

### <a name="configure-your-device"></a>Ρύθμιση παραμέτρων τη συσκευή σας

Ρύθμιση παραμέτρων του 3 π Raspberry για χρήση για πρώτη φορά και εγκαταστήστε το λειτουργικό σύστημα Raspbian, μια δωρεάν λειτουργικό σύστημα που είναι βελτιστοποιημένες για το υλικό Raspberry π.

*Εκτιμώμενη χρόνο για να ολοκληρωθεί: 30 λεπτά* 

[Μετάβαση σε 'Ρυθμίσετε τη συσκευή σας'](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)

### <a name="get-the-tools"></a>Λάβετε τα εργαλεία
Κάντε λήψη των εργαλείων και λογισμικού για τη δημιουργία και ανάπτυξη της πρώτης εφαρμογής για το π Raspberry 3.

*Εκτιμώμενη χρόνο για να ολοκληρωθεί: 20 λεπτά* 

[Μετάβαση σε 'Get τα Εργαλεία'](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

### <a name="create-and-deploy-the-blink-application"></a>Δημιουργία και ανάπτυξη της εφαρμογής εναλλαγής φωτεινότητας

Κλωνοποίηση το δείγμα εφαρμογής Node.js από Github και gulp για την ανάπτυξη αυτής της εφαρμογής σας πίνακα Raspberry π 3. Αυτού του δείγματος εφαρμογής αναβοσβήνει η Ένδειξη συνδεδεμένο στον πίνακα κάθε δύο δευτερόλεπτα.

*Εκτιμώμενη χρόνο για να ολοκληρωθεί: 5 λεπτά* 

[Μεταβείτε στις επιλογές ' Δημιουργία και ανάπτυξη της εφαρμογής εναλλαγής φωτεινότητας '](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

## <a name="lesson-2-create-your-iot-hub"></a>Μάθημα 2: Δημιουργία του διανομέα IoT

![Διάγραμμα E2E Μάθημα 2](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

Σε αυτό το μάθημα, δημιουργείτε το δωρεάν λογαριασμό της Azure, προμήθεια κέντρο σας Azure IoT και δημιουργία πρώτη συσκευή σας στο διανομέα Azure IoT.

Ολοκληρώστε Μάθημα 1 πριν να ξεκινήσετε αυτό το μάθημα.

### <a name="get-the-azure-tools"></a>Λάβετε τα εργαλεία Azure

Εγκατάσταση του Azure περιβάλλον γραμμής εντολών (Azure CLI).

*Εκτιμώμενη χρόνο για να ολοκληρωθεί: 10 λεπτά* 

[Μεταβείτε στις "Λήψη Azure Εργαλεία"](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

### <a name="create-your-iot-hub-and-register-your-raspberry-pi-3"></a>Δημιουργήσετε το Κέντρο IoT σας και να καταχωρήσετε το π Raspberry 3

Δημιουργήστε την ομάδα σας πόρων, προμήθεια του πρώτου διανομέα Azure IoT και προσθέστε πρώτα τη συσκευή σας στο διανομέα IoT Azure χρησιμοποιώντας Azure CLI. 

*Εκτιμώμενη χρόνο για να ολοκληρωθεί: 10 λεπτά* 

[Μεταβείτε στις "Δημιουργία κέντρο σας IoT και καταχώρηση σας Raspberry π 3"](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)


## <a name="lesson-3-send-device-to-cloud-messages"></a>Μάθημα 3: Αποστολή μηνυμάτων συσκευής στο cloud

![Διάγραμμα E2E Μάθημα 3](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

Σε αυτό το μάθημα, μηνύματα που στέλνετε από το π για το Κέντρο IoT σας. Μπορείτε επίσης να δημιουργήσετε μια εφαρμογή Azure συνάρτηση που παίρνει εισερχόμενων μηνυμάτων από το Κέντρο IoT σας και τις γράφει με το χώρο αποθήκευσης πινάκων του Azure.

Ολοκληρώστε μαθήματα 1 και 2, πριν να ξεκινήσετε αυτό το μάθημα.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Δημιουργήστε ένα Azure συνάρτηση και αποθήκευσης Azure λογαριασμού

Χρησιμοποιήστε ένα πρότυπο από διαχειριστή πόρων Azure για να δημιουργήσετε μια εφαρμογή Azure συνάρτησης και ένα λογαριασμό αποθήκευσης Azure.

*Εκτιμώμενη χρόνο για να ολοκληρωθεί: 10 λεπτά* 

[Μεταβείτε στο 'Δημιουργία ένα Azure συνάρτηση και αποθήκευσης Azure λογαριασμού'](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

### <a name="run-sample-application-to-send-device-to-cloud-messages"></a>Εκτέλεση της εφαρμογής δείγμα για την αποστολή μηνυμάτων συσκευής στο cloud

Ανάπτυξη και να εκτελέσετε ένα δείγμα εφαρμογής στη συσκευή σας Raspberry π 3 που στέλνει μηνύματα IoT διανομέα.

*Εκτιμώμενη χρόνο για να ολοκληρωθεί: 10 λεπτά* 

[Μετάβαση σε 'Εκτέλεση δείγμα εφαρμογής για την αποστολή μηνυμάτων συσκευή-cloud'](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

### <a name="read-messages-persisted-in-azure-storage"></a>Ανάγνωση μηνυμάτων διατηρηθεί στο χώρο αποθήκευσης Azure
Παρακολουθήστε τα μηνύματα συσκευής στο cloud όπως γίνεται εγγραφή σε χώρο αποθήκευσης του Azure.

*Εκτιμώμενη χρόνο για να ολοκληρωθεί: 5 λεπτά* 

[Μεταβείτε στο 'ανάγνωση μηνυμάτων διατηρηθεί στο χώρο αποθήκευσης Azure'](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)


## <a name="lesson-4-send-cloud-to-device-messages"></a>Μάθημα 4: Αποστολή μηνυμάτων cloud-σε-συσκευή

![Διάγραμμα E2E Μάθημα 4](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

Αυτό το μάθημα demos πώς μπορείτε να στείλετε μηνύματα από το Κέντρο Azure IoT σας για να σας Raspberry π 3. Τα μηνύματα ελέγχουν την ενεργοποίηση και απενεργοποίηση συμπεριφορά την Ένδειξη που είναι συνδεδεμένη με το π. Ένα δείγμα εφαρμογής είναι έτοιμη για να επιτύχετε αυτήν την εργασία.

Ολοκληρώστε μαθήματα 1, 2 και 3 πριν να ξεκινήσετε αυτό το μάθημα.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Εκτελέστε το δείγμα εφαρμογής για να λάβετε μηνύματα cloud-σε-συσκευή

Το δείγμα εφαρμογής στο Μάθημα 4 εκτελείται σε το π και παρακολουθεί την εισερχόμενων μηνυμάτων από το Κέντρο IoT σας. Μια νέα εργασία gulp στέλνει μηνύματα σε π σας από το Κέντρο IoT σας να αναβοσβήνει η Ένδειξη.

*Εκτιμώμενη χρόνο για να ολοκληρωθεί: 10 λεπτά* 

[Μετάβαση σε 'Εκτέλεση του δείγματος εφαρμογής για να λάβετε μηνύματα cloud-σε-συσκευή'](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Προαιρετική ενότητα: Αλλάξτε την ενεργοποίηση και απενεργοποίηση συμπεριφορά την Ένδειξη

Προσαρμόστε τα μηνύματα για να αλλάξετε την Ένδειξη του και να απενεργοποιήσετε τη συμπεριφορά.

*Εκτιμώμενη χρόνο για να ολοκληρωθεί: 10 λεπτά* 

[Μεταβείτε στις επιλογές ' προαιρετική ενότητα: Αλλάξτε την ενεργοποίηση και απενεργοποίηση συμπεριφορά την Ένδειξη '](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

Εάν πληροίτε τις troubles κατά τη διάρκεια τα μαθήματα, μπορείτε να αναζητούν λύσεις σε αυτήν τη σελίδα.

[Μεταβείτε στο 'Αντιμετώπιση προβλημάτων'](iot-hub-raspberry-pi-kit-node-troubleshooting.md)
