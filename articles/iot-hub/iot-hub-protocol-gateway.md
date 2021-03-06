<properties
   pageTitle="Azure πύλης πρωτόκολλο IoT | Microsoft Azure"
   description="Περιγράφει τον τρόπο χρήσης Azure IoT πρωτόκολλο πύλης για να επεκτείνετε τις δυνατότητες και υποστήριξη πρωτοκόλλου Azure IoT διανομέα."
   services="iot-hub"
   documentationCenter=""
   authors="kdotchkoff"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-hub"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="kdotchko"/>

# <a name="supporting-additional-protocols-for-iot-hub"></a>Υποστήριξη πρόσθετα πρωτόκολλα για διανομέα IoT

Azure διανομέα IoT υποστηρίζει εγγενώς την επικοινωνία μέσω των πρωτοκόλλων MQTT, AMQP και HTTP. Σε ορισμένες περιπτώσεις, συσκευές ή πεδίο πύλες ίσως να μην μπορείτε να χρησιμοποιήσετε μία από αυτές τις τυπικές πρωτόκολλα και θα απαιτούν προσαρμογή πρωτόκολλο. Σε αυτές τις περιπτώσεις, μπορείτε να χρησιμοποιήσετε μια προσαρμοσμένη πύλη. Μια προσαρμοσμένη πύλη να ενεργοποιήσετε πρωτοκόλλου προσαρμογή για τα τελικά σημεία διανομέα IoT γεφύρωση την κυκλοφορία προς και από το Κέντρο IoT. Μπορείτε να χρησιμοποιήσετε την [πύλη πρωτόκολλο Azure IoT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) ως προσαρμοσμένο πύλη για να ενεργοποιήσετε την προσαρμογή πρωτόκολλο για IoT διανομέα.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT πρωτόκολλο πύλης

Η πύλη πρωτόκολλο Azure IoT είναι ένα πλαίσιο για την προσαρμογή πρωτόκολλο που έχει σχεδιαστεί για υψηλή κλίμακας, διπλής κατεύθυνσης συσκευή επικοινωνία με το Κέντρο IoT. Η πύλη πρωτόκολλο είναι ένα στοιχείο διαβίβασης που δέχεται συνδέσεις συσκευών πάνω από ένα συγκεκριμένο πρωτόκολλο. Το γεφυρώνει την κυκλοφορία σε διανομέα IoT μέσω AMQP 1.0. Η πύλη πρωτόκολλο Azure IoT είναι διαθέσιμη ως έργο λογισμικό ανοιχτού κώδικα για την παροχή ευελιξία για την προσθήκη υποστήριξης για μια ποικιλία πρωτόκολλα και τις εκδόσεις πρωτόκολλο.

Μπορείτε να αναπτύξετε το πρωτόκολλο πύλης στο Azure με ιδιαίτερα με τον τρόπο, χρησιμοποιώντας τις υπηρεσίες Cloud Azure εργαζόμενου ρόλους. Επιπλέον, η πύλη πρωτόκολλο μπορούν να αναπτυχθούν στα περιβάλλοντα εσωτερικής εγκατάστασης, όπως το πεδίο πυλών.

Η πύλη πρωτοκόλλου Azure IoT περιλαμβάνει έναν προσαρμογέα πρωτοκόλλου MQTT που σας επιτρέπει να προσαρμόσετε τη συμπεριφορά πρωτοκόλλου MQTT εάν απαιτείται. Επειδή το διανομέα IoT παρέχει ενσωματωμένη υποστήριξη για το πρωτόκολλο v3.1.1 MQTT, που θα πρέπει να μόνο μπορείτε να χρησιμοποιήσετε στον προσαρμογέα πρωτοκόλλου MQTT εάν έχετε μια ανάγκη για τις προσαρμογές πρωτόκολλο ή συγκεκριμένες απαιτήσεις για πρόσθετη λειτουργικότητα.

Τον προσαρμογέα MQTT παρουσιάζει επίσης το μοντέλο προγραμματισμού για τη δημιουργία προσαρμογέων πρωτόκολλο για άλλα πρωτόκολλα. Επιπλέον, το μοντέλο προγραμματισμού Azure IoT πρωτόκολλο πύλης σάς επιτρέπει να συνδέετε προσαρμοσμένα στοιχεία για εξειδικευμένες επεξεργασία όπως προσαρμοσμένου ελέγχου ταυτότητας, μήνυμα μετασχηματισμοί, συμπίεση/αποσυμπίεση ή Κρυπτογράφηση/αποκρυπτογράφηση της κυκλοφορίας μεταξύ τις συσκευές και IoT διανομέα.

Για ευελιξία, την πύλη πρωτόκολλο και την εφαρμογή MQTT παρέχονται σε ένα έργο ανοιχτού κώδικα λογισμικού. Αυτό σας επιτρέπει να προσαρμόσετε την εφαρμογή, όπως απαιτείται.

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με την πύλη πρωτόκολλο Azure IoT και πώς μπορείτε να χρησιμοποιήσετε και να υλοποιήσετε ως μέρος της λύσης σας IoT, ανατρέξτε στα θέματα:

* [Azure IoT πρωτόκολλο πύλης αποθετήριο δεδομένων σε GitHub](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Azure IoT πρωτόκολλο πύλης Οδηγός για προγραμματιστές](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Για να μάθετε περισσότερα σχετικά με το σχεδιασμό της ανάπτυξής σας IoT διανομέα, ανατρέξτε στα θέματα:

- [Σύγκριση με διανομείς συμβάντος][lnk-compare]
- [Κλιμάκωση, HA και DR][lnk-scaling]

Για να εξερευνήσετε περαιτέρω τις δυνατότητες του IoT διανομέα, ανατρέξτε στα θέματα:

- [Οδηγός για προγραμματιστές][lnk-devguide]
- [Προσομοίωση μια συσκευή με το SDK IoT πύλης][lnk-gateway]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
