<properties
 pageTitle="Λάβετε τα Εργαλεία (macOS 10.10) | Microsoft Azure"
 description="Κάντε λήψη και εγκαταστήστε τα απαραίτητα εργαλεία και λογισμικού για το πρώτο δείγμα εφαρμογής για το π σε macOS."
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

# <a name="12-get-the-tools-macos-1010"></a>1.2 λάβετε τα Εργαλεία (macOS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 τι θα το κάνετε

Κάντε λήψη των εργαλείων ανάπτυξης και του λογισμικού για το πρώτο δείγμα εφαρμογής για το π Raspberry 3. Εάν πληροίτε τυχόν προβλήματα, αναζήτηση λύσεις με την [Αντιμετώπιση προβλημάτων σελίδας](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2 τι θα μάθετε
Σε αυτήν την ενότητα, θα μάθετε:

- Πώς να εγκαταστήσετε το Git και Node.js
    - [Git](https://git-scm.com) είναι ένα σύστημα ελέγχου Άνοιγμα αρχείου προέλευσης κατανεμημένο έκδοσης. Το δείγμα εφαρμογής για αυτό το μάθημα είναι αποθηκευμένο στο Git.
    - [Node.js](https://nodejs.org/en/) είναι ένα χρόνο εκτέλεσης JavaScript με ένα περιβάλλον εμπορικής προσαρμογής εμπλουτισμένου πακέτο.
- Πώς μπορείτε να χρησιμοποιήσετε NPM για να εγκαταστήσετε πρόσθετα εργαλεία ανάπτυξης Node.js.
  - Η ελάχιστη απαιτούμενη έκδοση του Node.js είναι 4,5 Αποτελεσμάτων.
  - [NPM](https://www.npmjs.com) είναι ένας από τους διευθυντές πακέτου για Node.js.

## <a name="123-what-you-need"></a>1.2.3 ό, τι χρειάζεστε

- Σύνδεση στο Internet για να κάνετε λήψη των εργαλείων ανάπτυξης και του λογισμικού
- Mac που εκτελείται macOS Yosemite (10.10) ή νεότερη έκδοση

## <a name="124-install-git-and-nodejs"></a>1.2.4 εγκατάσταση του Git και Node.js

Για να εγκαταστήσετε Git και Node.js, χρησιμοποιήστε το βοηθητικό πρόγραμμα [Homebrew](http://brew.sh) πακέτο διαχείρισης, ακολουθώντας τα παρακάτω βήματα:

1. Εγκαταστήστε το Homebrew. Εάν έχετε ήδη εγκαταστήσει Homebrew, μεταβείτε στο βήμα 2.
  1. Πατήστε το πλήκτρο `Cmd + Space` και πληκτρολογήστε `Terminal` για να ανοίξετε ένα παράθυρο τερματικού.
  2. Εκτελέστε την ακόλουθη εντολή:

    ```bash
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
2. Εγκαταστήστε Git και Node.js, εκτελώντας την ακόλουθη εντολή:

    ```bash
    brew install node git
    ```

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 εγκατάσταση πρόσθετα εργαλεία ανάπτυξης Node.js

Χρήση [gulp.js](http://gulpjs.com) για την αυτοματοποίηση της ανάπτυξης των το δείγμα εφαρμογής για το π. Μπορείτε επίσης να χρησιμοποιήσετε τη [συσκευή-εντοπισμού-cli](https://github.com/Azure/device-discovery-cli) για να ανακτήσετε δικτύου πληροφορίες σχετικά με τις συσκευές σας IoT.

Εγκατάσταση `gulp` και `device-discovery-cli` , εκτελέστε την παρακάτω εντολή στο παράθυρο τερματικού:

```bash
sudo npm install -g device-discovery-cli gulp
```

Εάν αντιμετωπίζετε προβλήματα κατά την εγκατάσταση Node.js και αυτά τα εργαλεία επιπλέον ανάπτυξης macOS, ανατρέξτε στο άρθρο του [Οδηγού αντιμετώπισης προβλημάτων](iot-hub-raspberry-pi-kit-node-troubleshooting.md) για λύσεις σε συνηθισμένα προβλήματα.

## <a name="126-install-visual-studio-code"></a>1.2.6 εγκατάσταση κώδικα Visual Studio

[Κάντε λήψη](https://code.visualstudio.com/docs/setup/osx) και εγκαταστήστε το Visual Studio κώδικα. Visual Studio κώδικα είναι ένα πρόγραμμα επεξεργασίας ελαφριά αλλά ισχυρή πηγαίου κώδικα για Windows, Linux και macOS. Μπορείτε να χρησιμοποιήσετε αυτό το πρόγραμμα επεξεργασίας αργότερα στην εκμάθηση για να επεξεργαστείτε το δείγμα κώδικα.

## <a name="127-summary"></a>1.2.7 σύνοψη

Έχετε εγκαταστήσει τα εργαλεία απαιτείται ανάπτυξη και λογισμικού για το πρώτο δείγμα εφαρμογής. Στην επόμενη ενότητα, δημιουργία, ανάπτυξης και εκτελέστε το δείγμα εφαρμογής στην το π.

## <a name="next-steps"></a>Επόμενα βήματα

[1.3 Δημιουργία και ανάπτυξη της εφαρμογής δείγμα εναλλαγής φωτεινότητας](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
