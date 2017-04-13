<properties
 pageTitle="Λάβετε Azure Εργαλεία (Windows 7 +) | Microsoft Azure"
 description="Εγκατάσταση Python και περιβάλλον γραμμής εντολών Azure (Azure CLI) σε Windows 7 και νεότερες εκδόσεις."
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

# <a name="21-get-azure-tools-windows-7-"></a>2.1 λάβετε Azure Εργαλεία (Windows 7 +)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 τι θα το κάνετε

Εγκατάσταση Python και το Azure γραμμής εντολών περιβάλλοντος εργασίας (Azure CLI). Εάν πληροίτε τυχόν προβλήματα, αναζήτηση λύσεις με την [Αντιμετώπιση προβλημάτων σελίδας](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2 τι θα μάθετε

- Μάθετε πώς μπορείτε να εγκαταστήσετε το Python.
- Μάθετε πώς μπορείτε να εγκαταστήσετε το Azure CLI.

## <a name="213-what-you-need"></a>2.1.3 ό, τι χρειάζεστε

- Έναν υπολογιστή Windows με σύνδεση στο Internet
- Μια ενεργή συνδρομή Azure. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/free/) σε λίγα λεπτά.

## <a name="214-install-python"></a>2.1.4 εγκατάσταση Python

Εγκατάσταση Python σε υπολογιστή με Windows. Μπορείτε να εγκαταστήσετε Python 2.7, 3.4 ή 3.5. Αυτό το πρόγραμμα εκμάθησης βασίζεται σε Python 2.7. Εάν έχετε ήδη εγκαταστήσει Python, μεταβείτε στην ενότητα 2.1.5.

[Λήψη Python για Windows](https://www.python.org/downloads/)

Πρέπει επίσης να προσθέσετε τη διαδρομή από τους φακέλους όπου python.exe και pip.exe είναι εγκατεστημένα στο σύστημα `PATH` μεταβλητή περιβάλλοντος. Από προεπιλογή, python.exe είναι εγκατεστημένο στον `C:\Python27` και pip.exe έχει εγκατασταθεί σε `C:\Python27\Scripts`.

## <a name="215-install-the-azure-cli"></a>2.1.5 εγκαταστήσετε το Azure CLI

Το Azure CLI παρέχει μια εμπειρία multiplatform γραμμής εντολών για Azure, μπορείτε να εργαστείτε απευθείας από τη γραμμή εντολών για την παροχή των και να διαχειριστείτε τους πόρους.

Για να εγκαταστήσετε Azure CLI, ακολουθήστε τα παρακάτω βήματα:

1. Ανοίξτε ένα παράθυρο γραμμής εντολών ως διαχειριστής.
2. Εκτελέστε τις εξής εντολές:

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```
3. Επιβεβαιώστε την εγκατάσταση, εκτελέστε την παρακάτω εντολή:

    ```bash
    az iot -h
    ```

Μπορείτε να δείτε το εξής αποτέλεσμα εάν η εγκατάσταση είναι επιτυχής.

![AZ iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="216-summary"></a>2.1.6 σύνοψη

Έχετε εγκαταστήσει Azure CLI. Συνεχίστε με την επόμενη ενότητα για να δημιουργήσετε την ταυτότητά σας Azure IoT διανομέας και συσκευής χρησιμοποιώντας το Azure CLI.

## <a name="next-steps"></a>Επόμενα βήματα

[2.2 δημιουργήσετε το Κέντρο IoT σας και να καταχωρήσετε το π Raspberry 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
