<properties
 pageTitle="Δημιουργήστε ένα Azure συνάρτηση και αποθήκευσης Azure λογαριασμό | Microsoft Azure"
 description="Η εφαρμογή Azure συνάρτηση ακρόασης σε συμβάντα διανομέα Azure IoT, επεξεργάζεται εισερχόμενα μηνύματα και τις γράφει με το χώρο αποθήκευσης πινάκων του Azure."
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

# <a name="31-create-an-azure-function-app-and-azure-storage-account"></a>3.1 Δημιουργία ένα Azure συνάρτηση και αποθήκευσης Azure λογαριασμού

[Συναρτήσεις Azure](../../articles/azure-functions/functions-overview.md) είναι μια λύση για την εκτέλεση εύκολα μικρά τμήματα κώδικα, που ονομάζεται "Λειτουργίες", στο cloud. Συνάρτηση Azure εφαρμογής φιλοξενεί την εκτέλεση του συναρτήσεων στο Azure.

## <a name="311-what-will-you-do"></a>3.1.1 τι θα μπορείτε να κάνετε

Χρησιμοποιήστε ένα πρότυπο από διαχειριστή πόρων Azure για να δημιουργήσετε μια εφαρμογή Azure συνάρτησης και ένα λογαριασμό αποθήκευσης Azure. Η εφαρμογή Azure συνάρτηση ακρόασης σε συμβάντα διανομέα Azure IoT, επεξεργάζεται εισερχόμενα μηνύματα και τις γράφει με το χώρο αποθήκευσης πινάκων του Azure. Εάν πληροίτε τυχόν προβλήματα, αναζήτηση λύσεις με την [Αντιμετώπιση προβλημάτων σελίδας](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="312-what-will-you-learn"></a>3.1.2 τι θα μάθετε

- Πώς να χρησιμοποιήσετε [Για τη διαχείριση πόρων Azure](../../articles/azure-resource-manager/resource-group-overview.md) για την ανάπτυξη του Azure πόρους.
- Πώς να χρησιμοποιήσετε μια εφαρμογή Azure συνάρτηση για να επεξεργαστούν IoT διανομέα μηνύματα και να γράφετε τους σε έναν πίνακα στο χώρο αποθήκευσης πινάκων του Azure.

## <a name="313-what-do-you-need"></a>3.1.3 τι χρειάζεστε

- Θα πρέπει να ολοκληρώθηκε με επιτυχία την προηγούμενη μαθήματα: [Γρήγορα αποτελέσματα με το π 3 Raspberry](iot-hub-raspberry-pi-kit-node-get-started.md) και [Δημιουργία κέντρο Azure IoT σας](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="314-open-the-sample-app"></a>3.1.4, ανοίξτε την εφαρμογή δείγματος

Ανοίξτε το δείγμα έργου στον κώδικα Visual Studio, εκτελώντας τις παρακάτω εντολές:

```bash
cd Lesson3
code .
```

![Δομή repo](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

- Το `app.js` το αρχείο του `app` υποφάκελος είναι το αρχείο προέλευσης κλειδιού. Αυτό το αρχείο προέλευσης περιέχει τον κωδικό για να στείλετε ένα μήνυμα 20 ώρες προς το Κέντρο IoT σας και να αναβοσβήνει η Ένδειξη για κάθε μήνυμα που στέλνει.
- Το `arm-template.json` αρχείο είναι το πρότυπο διαχείρισης πόρων Azure που περιέχει μια εφαρμογή Azure συνάρτησης και ένα λογαριασμό αποθήκευσης Azure.
- Το `arm-template-param.json` αρχείο είναι το αρχείο ρύθμισης παραμέτρων που χρησιμοποιείται από το πρότυπο διαχείρισης πόρων Azure.
- Το `ReceiveDeviceMessages` υποφάκελος περιέχει τον κώδικα Node.js για τη συνάρτηση Azure.

## <a name="315-configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>3.1.5 ρύθμιση παραμέτρων για πρότυπα Azure διαχείρισης πόρων και δημιουργία πόρων στο Azure

Ενημέρωση του `arm-template-param.json` αρχείου στο Visual Studio κώδικα.

![Azure παραμέτρους προτύπου για τη διαχείριση πόρων](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

- Αντικαταστήστε **[το όνομα διανομέας IoT]** με **{μου όνομα διανομέας}** που καθορίσατε στο [Μάθημα 2](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).
- Αντικαταστήστε **[συμβολοσειρά προθέματος νέων πόρων]** με οποιαδήποτε πρόθεμα που θέλετε. Το πρόθεμα εξασφαλίζει ότι το όνομα του πόρου είναι καθολικά μοναδικό για να αποφύγετε τη διένεξη. Μην χρησιμοποιείτε παύλας ή τον αριθμό αρχικό στο το πρόθεμα.

> [AZURE.NOTE] Δεν χρειάζεται `azure_storage_connection_string` σε αυτήν την ενότητα. Διατήρηση όπως είναι.

Μετά την ενημέρωση του `arm-template-param.json` αρχείων, ανάπτυξη τους πόρους Azure, εκτελώντας την ακόλουθη εντολή:

```bash
az resource group deployment create --template-file-path arm-template.json --parameters-file-path arm-template-param.json -g iot-sample -n mydeployment
```

Χρειάζονται περίπου πέντε λεπτά για να δημιουργήσετε αυτούς τους πόρους. Κατά τη δημιουργία πόρων βρίσκεται σε εξέλιξη, μπορείτε να μετακινήσετε σε στην επόμενη ενότητα.

## <a name="316-summary"></a>3.1.6 σύνοψη

Έχετε δημιουργήσει την εφαρμογή σας Azure συνάρτηση για την επεξεργασία IoT διανομέα μηνύματα και ένα λογαριασμό αποθήκευσης Azure για να αποθηκεύσετε αυτά τα μηνύματα. Μπορείτε να μετακινήσετε στην επόμενη ενότητα για να αναπτύξετε και να εκτελέσετε το δείγμα για την αποστολή μηνυμάτων συσκευής στο cloud στο το π.

## <a name="next-steps"></a>Επόμενα βήματα

[3.2 εκτέλεση δείγμα εφαρμογής για την αποστολή μηνυμάτων συσκευής στο cloud στο σας Raspberry π 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

