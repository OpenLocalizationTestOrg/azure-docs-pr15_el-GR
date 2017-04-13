<properties
 pageTitle="Λάβετε Azure Εργαλεία (macOS 10.10) | Microsoft Azure"
 description="Εγκατάσταση Python και περιβάλλον γραμμής εντολών Azure (Azure CLI) σε macOS."
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

# <a name="21-get-azure-tools-macos-1010"></a>2.1 λάβετε Azure Εργαλεία (macOS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 τι θα το κάνετε

Εγκαταστήστε το Azure περιβάλλον γραμμής εντολών (Azure CLI). Εάν πληροίτε τυχόν προβλήματα, αναζήτηση λύσεις με την [Αντιμετώπιση προβλημάτων σελίδας](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2 τι θα μάθετε

- Μάθετε πώς μπορείτε να εγκαταστήσετε το Azure CLI.
- Μάθετε πώς μπορείτε να προσθέσετε το υποσύνολο IoT του Azure CLI.

## <a name="213-what-you-need"></a>2.1.3 ό, τι χρειάζεστε

- Mac με σύνδεση στο Internet
- Μια ενεργή συνδρομή Azure. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/free/) σε λίγα λεπτά.

## <a name="214-install-python"></a>2.1.4 εγκατάσταση Python

Παρόλο που macOS συνοδεύεται από Python 2.7 από το πλαίσιο, συνιστάται να εγκαταστήσετε το Python μέσω Homebrew. Ανατρέξτε στο θέμα [Python κατά την εγκατάσταση σε macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).

Εγκαταστήστε Python και pip, εκτελώντας την ακόλουθη εντολή:

```bash
brew install python
```

## <a name="215-install-the-azure-cli"></a>2.1.5 εγκαταστήσετε το Azure CLI

Το Azure CLI παρέχει μια εμπειρία multiplatform γραμμής εντολών για Azure, μπορείτε να εργαστείτε απευθείας από τη γραμμή εντολών για την παροχή των και να διαχειριστείτε τους πόρους. 

Για να εγκαταστήσετε την πιο πρόσφατη CLI Azure, ακολουθήστε τα παρακάτω βήματα:

1. Εκτελέστε τις ακόλουθες εντολές σε ένα παράθυρο τερματικού. Μπορεί να χρειαστεί πέντε λεπτά για να εγκαταστήσετε το Azure CLI.

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```

2. Επαλήθευση εγκατάστασης, εκτελέστε την παρακάτω εντολή:

    ```bash
    az iot -h
    ```
  
Εάν η εγκατάσταση είναι επιτυχής, θα πρέπει να βλέπετε το παρακάτω αποτέλεσμα.

![AZ iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="215-summary"></a>2.1.5 σύνοψη

Έχετε εγκαταστήσει Azure CLI. Συνεχίστε με την επόμενη ενότητα για να δημιουργήσετε την ταυτότητά σας Azure IoT διανομέας και συσκευής χρησιμοποιώντας το Azure CLI.

## <a name="next-steps"></a>Επόμενα βήματα

[2.2 δημιουργήσετε το Κέντρο IoT σας και να καταχωρήσετε το π Raspberry 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
