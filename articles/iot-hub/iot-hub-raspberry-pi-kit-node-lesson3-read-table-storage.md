<properties
 pageTitle="Ανάγνωση μηνυμάτων διατηρηθεί στο χώρο αποθήκευσης Azure | Microsoft Azure"
 description="Παρακολουθήστε τα μηνύματα συσκευής στο cloud, όπως γίνεται εγγραφή σε ο χώρος αποθήκευσης πινάκων του Azure."
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

# <a name="33-read-messages-persisted-in-azure-storage"></a>3.3 Διαβάστε μηνύματα διατηρηθεί στο χώρο αποθήκευσης Azure

## <a name="331-what-will-you-do"></a>3.3.1 τι θα μπορείτε να κάνετε

Παρακολουθήστε τα μηνύματα συσκευής στο cloud που αποστέλλονται από το π 3 Raspberry για το Κέντρο IoT σας καθώς τα μηνύματα που έχουν εγγραφεί για το χώρο αποθήκευσης πινάκων του Azure. Εάν πληροίτε τυχόν προβλήματα, αναζήτηση λύσεις με την [Αντιμετώπιση προβλημάτων σελίδας](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="332-what-will-you-learn"></a>3.3.2 τι θα μάθετε

- Πώς μπορείτε να χρησιμοποιήσετε την εργασία gulp ανάγνωση μηνύματος για την ανάγνωση μηνυμάτων μόνιμων στο χώρο αποθήκευσης πινάκων του Azure του.

## <a name="333-what-do-you-need"></a>3.3.3 τι χρειάζεστε

- Θα πρέπει να ολοκληρώθηκε με επιτυχία την προηγούμενη ενότητα, [εκτελέστε την εφαρμογή δείγμα Azure εναλλαγής φωτεινότητας σε σας Raspberry π 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="334-read-new-messages-from-your-storage-account"></a>3.3.4 Διαβάστε νέα μηνύματα από το λογαριασμό χώρου αποθήκευσης

Στην προηγούμενη ενότητα, εκτελέσατε ένα δείγμα εφαρμογής στον το π. Δείγμα εφαρμογής αποστέλλονται μηνύματα, για να το Κέντρο Azure IoT σας. Τα μηνύματα που αποστέλλονται με το διανομέα IoT αποθηκεύονται σε ο χώρος αποθήκευσης πινάκων του Azure μέσω της εφαρμογής Azure συνάρτηση. Χρειάζεστε τη συμβολοσειρά σύνδεσης αποθήκευσης Azure για ανάγνωση μηνυμάτων από το χώρο αποθήκευσης πινάκων του Azure.

Για να διαβάσετε τα μηνύματα που είναι αποθηκευμένα στο χώρο αποθήκευσης πινάκων του Azure του, ακολουθήστε τα παρακάτω βήματα:

1. Λάβετε τη συμβολοσειρά σύνδεσης, εκτελώντας τις παρακάτω εντολές:

    ```bash
    az storage account list -g iot-sample --query [].name
    az storage account show-connection-string -g iot-sample -n {storage name}
    ```

    Η πρώτη εντολή ανακτά την `storage name` που χρησιμοποιείται σε τη δεύτερη εντολή για να λάβετε τη συμβολοσειρά σύνδεσης. `iot-sample`είναι η προεπιλεγμένη τιμή του `{resource group name}` Εάν δεν μπορείτε να αλλάξετε την τιμή στο Μάθημα 2.

2. Ανοίξτε το αρχείο ρύθμισης παραμέτρων `config-raspberrypi.json` αρχείο στον κώδικα Visual Studio, εκτελώντας την ακόλουθη εντολή:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Αντικατάσταση `[Azure Storage connection string]` με τη συμβολοσειρά σύνδεσης που λάβατε στο βήμα 1.
4. Αποθηκεύστε το `config-raspberrypi.json` αρχείου.
5. Επανάληψη αποστολής μηνυμάτων και να τις διαβάζετε από το χώρο αποθήκευσης πινάκων του Azure, εκτελώντας την ακόλουθη εντολή:

    ```bash
    gulp run --read-storage
    ```

    Η λογική για ανάγνωση από χώρο αποθήκευσης πινάκων του Azure είναι στο το `azure-table.js` αρχείου.

    ![εκτέλεση--gulp ανάγνωση αποθήκευσης](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="335-summary"></a>3.3.5 σύνοψη

Έχετε με επιτυχία συνδεδεμένο π σας με το Κέντρο IoT σας στο cloud και χρησιμοποιείται το δείγμα εφαρμογής εναλλαγής φωτεινότητας για αποστολή μηνυμάτων συσκευής στο cloud. Χρησιμοποιείται επίσης την εφαρμογή Azure συνάρτηση για να αποθηκεύουν τα εισερχόμενα μηνύματα διανομέα IoT για το χώρο αποθήκευσης πινάκων του Azure. Μπορείτε να μετακινήσετε για να την επόμενη μάθημα σχετικά με τον τρόπο για την αποστολή μηνυμάτων cloud-σε-συσκευή από το Κέντρο IoT σας για να σας π.

## <a name="next-steps"></a>Επόμενα βήματα

[Μάθημα 4: Αποστολή μηνυμάτων cloud-σε-συσκευή](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)
