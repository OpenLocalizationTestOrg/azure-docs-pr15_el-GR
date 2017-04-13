<properties
 pageTitle="Εκτέλεση της εφαρμογής δείγμα για την αποστολή μηνυμάτων συσκευής στο cloud | Microsoft Azure"
 description="Ανάπτυξη και να εκτελέσετε ένα δείγμα εφαρμογής για να σας Raspberry π 3 που στέλνει μηνύματα με διανομέα IoT και αναβοσβήνει η Ένδειξη."
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

# <a name="32-run-sample-application-to-send-device-to-cloud-messages"></a>3.2 εκτέλεση δείγμα εφαρμογής για την αποστολή μηνυμάτων συσκευής στο cloud

## <a name="321-what-you-will-do"></a>3.2.1 τι θα το κάνετε

Ανάπτυξη και να εκτελέσετε μια εφαρμογή του δείγματος σε σας 3 π Raspberry που στέλνει μηνύματα κέντρο IoT σας. Εάν πληροίτε τυχόν προβλήματα, αναζήτηση λύσεις με την [Αντιμετώπιση προβλημάτων σελίδας](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="322-what-you-will-learn"></a>3.2.2 τι θα μάθετε

- Μάθετε πώς μπορείτε να χρησιμοποιήσετε το εργαλείο gulp για να αναπτύξετε και να εκτελέσετε το δείγμα εφαρμογής Node.js στην το π.

## <a name="323-what-you-need"></a>3.2.3 ό, τι χρειάζεστε

- Θα πρέπει να έχετε ολοκληρώθηκε με επιτυχία την προηγούμενη ενότητα σε αυτό το μάθημα: [Δημιουργία εφαρμογής Azure συνάρτησης και ένα λογαριασμό Azure χώρου αποθήκευσης για την επεξεργασία και αποθήκευση IoT διανομέα μηνυμάτων](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="324-get-your-iot-hub-and-device-connection-strings"></a>3.2.4 γρήγορα τις συμβολοσειρές σύνδεσης IoT διανομέας και συσκευή

Συμβολοσειρά σύνδεσης συσκευή χρησιμοποιούνται για τη σύνδεση του π για το Κέντρο IoT σας. Συμβολοσειρά σύνδεσης από την ενότητα IoT χρησιμοποιούνται για τη σύνδεση κέντρο IoT σας για να την ταυτότητα της συσκευής που αντιπροσωπεύει το π στην ενότητα IoT.

- Λάβετε τη συμβολοσειρά σύνδεσης IoT διανομέα, εκτελέστε την παρακάτω εντολή Azure CLI:

```bash
az iot hub show-connection-string --name {my hub name} --resource-group iot-sample
```

`{my hub name}`είναι το όνομα που καθορίσατε στο Μάθημα 2. Χρήση `iot-sample` ως τιμή του `{resource group name}` Εάν δεν μπορείτε να αλλάξετε την τιμή στο Μάθημα 2.

- Λάβετε τη συμβολοσειρά σύνδεσης συσκευή, εκτελέστε την παρακάτω εντολή:

```bash
az iot device show-connection-string --hub {my hub name} --device-id myraspberrypi --resource-group iot-sample
```

`{my hub name}`σας μεταφέρει την ίδια τιμή με αυτή που χρησιμοποιείται με την προηγούμενη εντολή. Χρήση `iot-sample` ως τιμή του `{resource group name}` και χρησιμοποιήστε `myraspberrypi` ως τιμή του `{device id}` Εάν δεν μπορείτε να αλλάξετε την τιμή στο Μάθημα 2.

## <a name="325-configure-the-device-connection"></a>3.2.5 ρύθμιση παραμέτρων του τη σύνδεση της συσκευής

1. Προετοιμασία του αρχείου ρύθμισης παραμέτρων, εκτελώντας τις παρακάτω εντολές:

    ```bash
    npm install
    gulp init
    ```

2. Ανοίξτε το αρχείο ρύθμισης παραμέτρων συσκευής `config-raspberrypi.json` στο Visual Studio κώδικα, εκτελέστε την παρακάτω εντολή:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)

3. Κάντε το εξής αντικατάστασης ελέγχου στο το `config-raspberrypi.json` αρχείο:

  - Αντικατάσταση **[συσκευή όνομα κεντρικού υπολογιστή ή τη διεύθυνση IP]** με τη διεύθυνση IP συσκευή ή όνομα κεντρικού υπολογιστή που λάβατε από `device-discovery-cli` ή με την τιμή που έχουν μεταβιβαστεί από τι έχετε ρυθμίσει τις παραμέτρους στο Μάθημα 1.
  - Αντικατάσταση **[συμβολοσειρά σύνδεσης συσκευή IoT]** με το `device connection string` που που λαμβάνονται.
  - Αντικατάσταση **[συμβολοσειρά σύνδεσης διανομέα IoT]** με το `iot hub connection string` που που λαμβάνονται.

Μπορείτε να ενημερώσετε το `config-raspberrypi.json` αρχείου, έτσι ώστε να μπορείτε να αναπτύξετε το δείγμα εφαρμογής από τον υπολογιστή σας.

## <a name="326-deploy-and-run-the-sample-application"></a>3.2.6 αναπτύξετε και να εκτελέσετε το δείγμα εφαρμογής

Ανάπτυξη και να εκτελέσετε το δείγμα εφαρμογής σε του π, εκτελώντας την ακόλουθη εντολή:

```bash
gulp
```

> [AZURE.NOTE] Η εργασία gulp προεπιλογή εκτελείται `install-tools`, `deploy`, και `run` εργασίες διαδοχικά. Στο [Μάθημα 1](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md), εκτελέσατε αυτές τις εργασίες διαδοχικά ξεχωριστά.

## <a name="327-verify-the-sample-application-works"></a>3.2.7 επαλήθευση των έργων δείγμα εφαρμογής

Θα πρέπει να δείτε την Ένδειξη που είναι συνδεδεμένη με το π αναβοσβήνει κάθε δύο δευτερόλεπτα. Κάθε φορά που η ένδειξη LED αναβοσβήνει, το δείγμα εφαρμογής στέλνει ένα μήνυμα στο κέντρο σας IoT και επαληθεύει ότι το μήνυμα έχει αποσταλεί με επιτυχία με το διανομέα IoT. Επιπλέον, για κάθε έναν από το μήνυμα ελήφθη από την ενότητα IoT, το μήνυμα εκτυπώνεται στο παράθυρο της κονσόλας. Το δείγμα εφαρμογής τερματίζεται αυτόματα μετά την αποστολή μηνυμάτων 20.

![](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="328-summary"></a>3.2.8 σύνοψη

Έχετε αναπτύξει και εκτελέσετε τη νέα εφαρμογή δείγμα εναλλαγής φωτεινότητας σε σας π για την αποστολή μηνυμάτων συσκευής στο cloud με το διανομέα IoT. Μπορείτε να μετακινήσετε στην επόμενη ενότητα για να παρακολουθείτε τα μηνύματά σας, όπως αυτά εγγράφονται στο λογαριασμό αποθήκευσης Azure.

## <a name="next-steps"></a>Επόμενα βήματα

[3.3 Διαβάστε μηνύματα διατηρηθεί στο χώρο αποθήκευσης Azure](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)