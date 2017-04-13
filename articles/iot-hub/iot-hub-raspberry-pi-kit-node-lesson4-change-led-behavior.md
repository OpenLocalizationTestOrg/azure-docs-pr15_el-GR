<properties
 pageTitle="Προαιρετική ενότητα - Αλλαγή την ενεργοποίηση και απενεργοποίηση συμπεριφορά την Ένδειξη | Microsoft Azure"
 description="Προσαρμόστε τα μηνύματα για να αλλάξετε την Ένδειξη του και να απενεργοποιήσετε τη συμπεριφορά."
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

# <a name="42-optional-section-change-the-on-and-off-behavior-of-the-led"></a>4.2 προαιρετική ενότητα: Αλλάξτε την ενεργοποίηση και απενεργοποίηση συμπεριφορά την Ένδειξη

## <a name="421-what-you-will-do"></a>4.2.1 τι θα το κάνετε

Προσαρμόστε τα μηνύματα για να αλλάξετε την Ένδειξη του και να απενεργοποιήσετε τη συμπεριφορά. Εάν πληροίτε τυχόν προβλήματα, αναζήτηση λύσεις με την [Αντιμετώπιση προβλημάτων σελίδας](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="422-what-you-will-learn"></a>4.2.2 τι θα μάθετε

Χρησιμοποιήστε πρόσθετες λειτουργίες Node.js για να αλλάξετε την Ένδειξη του και να απενεργοποιήσετε τη συμπεριφορά.

## <a name="423-what-you-need"></a>4.2.3 ό, τι χρειάζεστε

Θα πρέπει να ολοκληρώθηκε με επιτυχία [4.1 εκτελέστε μια εφαρμογή δείγματος για το π Raspberry να λαμβάνετε cloud σε συσκευή μηνύματα](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="424-add-nodejs-functions"></a>4.2.4 Προσθήκη Node.js συναρτήσεων

1. Ανοίξτε το δείγμα εφαρμογής στον κώδικα Visual Studio, εκτελώντας τις παρακάτω εντολές:

    ```bash
    cd Lesson4
    code .
    ```

2. Άνοιγμα του `app.js` αρχείων και, στη συνέχεια, προσθέστε τις ακόλουθες συναρτήσεις στο τέλος:

    ```javascript
    function turnOnLED() {
      wpi.digitalWrite(CONFIG_PIN, 1);
    }

    function turnOffLED() {
      wpi.digitalWrite(CONFIG_PIN, 0);
    }
    ```

    ![Ενημέρωση app.js](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)

3. Προσθέστε τις ακόλουθες συνθήκες πριν από την προεπιλογή μία στο μπλοκ περίπτωση διακόπτη το `receiveMessageCallback` συνάρτηση:

    ```javascript
    case 'on':
      turnOnLED();
      break;
    case 'off':
      turnOffLED();
      break;
    ```

    Τώρα έχετε ρυθμίσει τις παραμέτρους του δείγματος εφαρμογής για να απαντήσετε σε περισσότερες οδηγίες μέσω μηνυμάτων. Η εντολή "σε" Ενεργοποιεί την Ένδειξη και την οδηγία "Απενεργοποίηση" απενεργοποιεί το LED.

4. Ανοίξτε το αρχείο gulpfile.js και, στη συνέχεια, προσθέστε μια νέα συνάρτηση πριν από τη συνάρτηση `sendMessage`:

    ```javascript
    var buildCustomMessage = function (messageId) {
      if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
        return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
      } else if (messageId < MAX_MESSAGE_COUNT) {
        return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
      } else {
        return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
      }
    }
    ```

    ![Ενημέρωση gulpfile](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)

5. Στο το `sendMessage` λειτουργούν, αντικαταστήστε τη γραμμή `var message = buildMessage(sentMessageCount);` με τη νέα γραμμή που εμφανίζεται στο το παρακάτω τμήμα κώδικα:

    ```javascript
    var message = buildCustomMessage(sentMessageCount);
    ```

6. Αποθηκεύστε όλες τις αλλαγές.

### <a name="425-deploy-and-run-the-sample-application"></a>4.2.5 αναπτύξετε και να εκτελέσετε το δείγμα εφαρμογής

Ανάπτυξη και να εκτελέσετε το δείγμα εφαρμογής σε του π, εκτελώντας την ακόλουθη εντολή:

```bash
gulp
```

Θα πρέπει να δείτε την Ένδειξη ενεργοποιημένη για δύο δευτερόλεπτα και, στη συνέχεια, απενεργοποιήσει το άλλο δύο δευτερόλεπτα. Το τελευταίο μήνυμα "Διακοπή" διακόπτει την εκτέλεση της δείγμα εφαρμογής.

![Ενεργοποίηση και απενεργοποίηση](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Συγχαρητήρια! Έχετε προσαρμόσει τα μηνύματα που αποστέλλονται σε το π από το Κέντρο IoT σας με επιτυχία.

### <a name="427-summary"></a>4.2.7 σύνοψη

Αυτή η προαιρετική ενότητα demos πώς μπορείτε να προσαρμόσετε τα μηνύματα, έτσι ώστε το δείγμα εφαρμογής μπορούν να ελέγχουν την ενεργοποίηση και απενεργοποίηση συμπεριφορά την Ένδειξη με διαφορετικό τρόπο.

