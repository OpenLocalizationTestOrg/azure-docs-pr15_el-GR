<properties
 pageTitle="Θέματα Οδηγός για προγραμματιστές για διανομέα IoT | Microsoft Azure"
 description="Azure Οδηγός για προγραμματιστές IoT διανομέα που περιλαμβάνει τα τελικά σημεία IoT διανομέα, ασφαλείας, συσκευή ταυτότητας μητρώο, Διαχείριση συσκευών και ανταλλαγή μηνυμάτων"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="azure-iot-hub-developer-guide"></a>Οδηγός για προγραμματιστές Azure IoT διανομέα

Azure IoT διανομέα είναι μια πλήρως διαχειριζόμενη υπηρεσία που σας βοηθά να ενεργοποιήσετε αξιόπιστη και ασφαλή επικοινωνίες διπλής κατεύθυνσης μεταξύ εκατομμύρια IoT συσκευές και το τέλος ξανά μια εφαρμογή.

Azure IoT ενότητα σας παρέχει με:

* Ασφαλείς επικοινωνίες, χρησιμοποιώντας τα διαπιστευτήρια ασφαλείας ανά συσκευής και έλεγχος πρόσβασης.
* Αξιόπιστη συσκευή στο cloud και cloud-σε-συσκευή hyper κλίμακας μηνυμάτων.
* Εύκολη συσκευή συνδεσιμότητα με βιβλιοθήκες συσκευή για τις πιο δημοφιλείς γλώσσες και πλατφόρμες.

Αυτός ο οδηγός για προγραμματιστές διανομέα IoT περιλαμβάνει τα ακόλουθα άρθρα:

- [Αποστολή και λήψη μηνυμάτων με διανομέα IoT] [ devguide-messaging] περιγράφει τις ανταλλαγής μηνυμάτων δυνατότητες (συσκευή στο cloud και cloud-σε-συσκευή) που εκθέτει IoT διανομέα. Το άρθρο περιλαμβάνει επίσης πληροφορίες σχετικά με θέματα όπως μορφές μηνυμάτων, και τα πρωτόκολλα υποστηριζόμενες επικοινωνίας και οι αριθμοί θυρών που χρησιμοποιούν.
- [Αποστολή αρχείων από μια συσκευή] [ devguide-upload] περιγράφει πώς μπορείτε να αποστείλετε αρχεία από μια συσκευή. Το άρθρο περιλαμβάνει επίσης πληροφορίες σχετικά με θέματα όπως οι ειδοποιήσεις να στείλετε τη διαδικασία uplaod.
- [Διαχείριση ταυτοτήτων συσκευή στο διανομέα IoT] [ devguide-identities] περιγράφει τι πληροφορίες του διανομέα IoT κάθε συσκευή ταυτότητας μητρώο αποθηκεύει και πώς μπορείτε να αποκτήσετε πρόσβαση και να τροποποιήσετε.
- [Έλεγχος της πρόσβασης σε διανομέα IoT] [ devguide-security] περιγράφει το μοντέλο ασφάλειας που χρησιμοποιείται για να εκχωρήσετε πρόσβαση σε διανομέα IoT λειτουργικότητα για δύο συσκευές και τα στοιχεία στο cloud. Το άρθρο περιλαμβάνει πληροφορίες σχετικά με τη χρήση διακριτικά και πιστοποιητικά X.509 και λεπτομερειών από τα δικαιώματα που μπορείτε να εκχωρήσετε.
- [Χρησιμοποιήστε twins συσκευή για να συγχρονίσετε νομό και ρυθμίσεις παραμέτρων] [ devguide-device-twins] περιγράφει την έννοια *διπλού συσκευή* και τη λειτουργικότητα εκθέτει όπως το συγχρονισμό μιας συσκευής με το διπλού. Το άρθρο περιλαμβάνει πληροφορίες σχετικά με τα δεδομένα που είναι αποθηκευμένα σε μια συσκευή διπλού.
- [Κλήση μιας μεθόδου απευθείας σε μια συσκευή] [ devguide-directmethods] περιγράφει τον κύκλο ζωής των μια άμεση μέθοδος, πληροφορίες σχετικά με τον τρόπο για να καλέσετε μεθόδους σε μια συσκευή από την εφαρμογή παρασκηνίου και χειρισμού της μεθόδου απευθείας στη συσκευή σας.
- [Προγραμματισμός εργασιών σε πολλές συσκευές] [ devguide-jobs] περιγράφει πώς μπορείτε να προγραμματίσετε εργασίες σε πολλές συσκευές. Το άρθρο περιγράφει τον τρόπο υποβολής εργασιών που εκτελούν εργασίες, όπως η εκτέλεση μια άμεση μέθοδος, ενημέρωση ενός devcie χρησιμοποιώντας μια διπλού devcie. Επίσης, περιγράφει τον τρόπο ερωτήματος την κατάσταση μιας εργασίας.
- [Αναφορά - διανομέα IoT τελικά σημεία] [ devguide-endpoints] περιγράφει τα διάφορα τελικά σημεία που κάθε ενότητα IoT εκθέτει για διαχείριση χρόνου εκτέλεσης και λειτουργίες. Επίσης, το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε μια πύλη πεδίο για να ενεργοποιήσετε ορισμένες συσκευές για να συνδεθείτε με το διανομέα IoT τελικά σημεία.
- [Αναφορά - γλώσσα ερωτημάτων για twins, μεθόδους και εργασίες] [ devguide-query] περιγράφει αυτήν τη γλώσσα ερωτήματος που σας επιτρέπει να ανακτήσετε πληροφορίες από το Κέντρο σας σχετικά με τη συσκευή twins και εργασίες.
- [Αναφορά - ορίων και περιορισμού] [ devguide-quotas] συνοψίζει τα όρια της υπηρεσίας IoT διανομέα και τη συμπεριφορά επιτάχυνσης μπορείτε να περιμένετε για να δείτε πότε υπερβαίνει ένα όριο.
- [Αναφορά - συσκευή και υπηρεσία SDK] [ devguide-sdks] λίστες του SDK που μπορείτε να χρησιμοποιήσετε το αναπτύξετε συσκευής και υπηρεσίας εφαρμογές που αλληλεπιδρούν με το Κέντρο IoT σας. Το άρθρο περιλαμβάνει συνδέσεις σε API τεκμηρίωσης στο Internet.
- [Αναφορά - υποστήριξη IoT διανομέα MQTT] [ devguide-mqtt] παρέχει λεπτομερείς πληροφορίες σχετικά με τον τρόπο διανομέα IoT υποστηρίζει το πρωτόκολλο MQTT. Το άρθρο περιγράφει την υποστήριξη για το πρωτόκολλο MQTT ενσωματωμένη για το SDK και παρέχει πληροφορίες σχετικά με τη χρήση του πρωτοκόλλου MQTT απευθείας.
- [Γλωσσάρι του] [ devguide-glossary] μια λίστα με τις κοινές IoT διανομέα που σχετίζονται με τους όρους.



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md

