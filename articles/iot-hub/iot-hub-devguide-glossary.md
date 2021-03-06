<properties
 pageTitle="Οδηγός για προγραμματιστές - Γλωσσάρι | Microsoft Azure"
 description="Ένα γλωσσάρι κοινών όρων που σχετίζονται με το Κέντρο IoT"
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

# <a name="glossary-of-iot-hub-terms"></a>Γλωσσάρι όρων IoT διανομέα

Σε αυτό το άρθρο παραθέτει ορισμένα από τα κοινών όρων που σχετίζεται με το Κέντρο IoT.

## <a name="advanced-message-queueing-protocol-amqp"></a>Πρωτόκολλο ουράς μηνυμάτων για προχωρημένους (AMQP)

[AMQP](https://www.amqp.org/) είναι ένα από τα πρωτόκολλα ανταλλαγής μηνυμάτων που υποστηρίζει το Κέντρο IoT για την επικοινωνία με συσκευές. Για περισσότερες πληροφορίες σχετικά με τα πρωτόκολλα ανταλλαγής μηνυμάτων που υποστηρίζει IoT διανομέα, ανατρέξτε στο θέμα [Αποστολή και λήψη μηνυμάτων με IoT διανομέα](iot-hub-devguide-messaging.md).

## <a name="cloud-to-device-c2d"></a>Cloud-σε-συσκευή (C2D)

Χρησιμοποιείται συνήθως για την αναφορά στα μηνύματα που αποστέλλονται από διανομέα IoT σε μια συνδεδεμένη συσκευή. Συχνά, αυτά τα μηνύματα είναι εντολές που πείτε τη συσκευή για να πραγματοποιήσετε κάποια ενέργεια. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αποστολή και λήψη μηνυμάτων με IoT διανομέα](iot-hub-devguide-messaging.md).

## <a name="condition"></a>Η συνθήκη

Αναφέρεται σε πληροφορίες κατάστασης συσκευή, όπως τη συνδεσιμότητα μέθοδο αυτήν τη στιγμή, όπως αναφέρονται από μια εφαρμογή για τη συσκευή. Συσκευές επίσης να αναφέρετε τις δυνατότητες. Μπορείτε να υποβάλετε ερώτημα συνθήκη και τη δυνατότητα με χρήση του διπλού συσκευή.

## <a name="data-point-message"></a>Μήνυμα σημείο δεδομένων

Ένα σημείο δεδομένων μήνυμα είναι ένα μήνυμα cloud-σε-συσκευή που περιέχει τα δεδομένα τηλεμετρίας όπως ταχύτητα ανέμου ή τη θερμοκρασία.

## <a name="desired-properties"></a>Επιθυμητές ιδιότητες

Στο περιβάλλον της συσκευής twins, επιθυμητές ιδιότητες χρησιμοποιούνται σε συνδυασμό με *αναφερθεί ιδιότητες* για να συγχρονίσετε ρύθμισης παραμέτρων συσκευής ή συνθήκη. Επιθυμητή ιδιότητες μπορεί να οριστεί μόνο από την εφαρμογή ξανά τέλος και τηρούνται από την εφαρμογή συσκευή. 

## <a name="device-to-cloud-d2c"></a>Συσκευή στο cloud (D2C)

Χρησιμοποιείται συνήθως για την αναφορά στα μηνύματα που αποστέλλονται από μια συσκευή συνδεδεμένη σε διανομέα IoT. Αυτά τα μηνύματα μπορεί να είναι το [σημείο δεδομένων](#data-point-message) ή [αλληλεπίδρασης με](#interactive-message) τα μηνύματα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αποστολή και λήψη μηνυμάτων με IoT διανομέα](iot-hub-devguide-messaging.md).

## <a name="device-identity-registry"></a>Συσκευή ταυτότητας μητρώου

[Συσκευή ταυτότητας μητρώου](iot-hub-devguide-identity-registry.md) είναι το ενσωματωμένο στοιχείο με ένα διανομέα IoT που αποθηκεύει πληροφορίες σχετικά με τις μεμονωμένες συσκευές που επιτρέπεται να συνδεθούν με ένα διανομέα.

## <a name="device"></a>Συσκευή

Στο περιβάλλον της IoT, μια συσκευή είναι συνήθως μικρού μεγέθους, μεμονωμένη υπολογιστική συσκευή που μπορεί να συλλέγετε δεδομένα ή τον έλεγχο άλλων συσκευών. Για παράδειγμα μια συσκευή μπορεί να είναι, συσκευή περιβαλλοντικό παρακολούθησης, ή έναν ελεγκτή για τα συστήματα ποτίσματος και εξαερισμού στο θερμοκηπίου.

## <a name="device-twin"></a>Συσκευή διπλού

Μια [συσκευή διπλού](iot-hub-devguide-device-twins.md) είναι ένα αντίγραφο στην ενότητα IoT της των συνθήκης και τη ρύθμιση παραμέτρων ρυθμίσεων μια φυσική συσκευή. Μπορείτε να χρησιμοποιήσετε μια διπλού συσκευή για να διαχειριστείτε τη ρύθμιση παραμέτρων του η φυσική συσκευή.

## <a name="direct-method"></a>Άμεση μέθοδος

[Άμεση μέθοδος](iot-hub-devguide-direct-methods.md) είναι ένας τρόπος για να ενεργοποιήσετε μια μέθοδο για την εκτέλεση σε μια συσκευή καλώντας API στο κέντρο IoT σας.

## <a name="event-hub-compatible-endpoint"></a>Τελικό σημείο συμβατό με το Κέντρο συμβάντος

Για να διαβάσετε συσκευή-cloud μηνύματα που αποστέλλονται σε κέντρο IoT σας, μπορείτε να συνδεθείτε με ένα τελικό σημείο στο κέντρο σας και να χρησιμοποιήσετε οποιαδήποτε μέθοδο συμβατό διανομέα συμβάντος για να διαβάσετε αυτά τα μηνύματα. Οι μέθοδοι συμβατό με το Κέντρο συμβάν περιλαμβάνουν χρησιμοποιώντας το συμβάν διανομείς SDK και ανάλυση ροή Azure.

## <a name="field-gateway"></a>Πεδίο πύλης

Μια πύλη πεδίο επιτρέπει τη σύνδεση για συσκευές που δεν μπορεί να συνδεθεί απευθείας με διανομέα IoT και συνήθως έχει αναπτυχθεί τοπικά με τις συσκευές σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι το Κέντρο IoT Azure;](iot-hub-what-is-iot-hub.md).

## <a name="job"></a>Εργασία

Σας παρασκηνίου λύση μπορεί να χρησιμοποιήσετε εργασίες να προγραμματίζουν και να παρακολουθήσετε δραστηριότητες σε ένα σύνολο των συσκευών που έχουν καταχωρηθεί με το Κέντρο IoT σας. Οι δραστηριότητες περιλαμβάνουν ενημέρωση διπλού επιθυμητοί ιδιότητες των συσκευών, η ενημέρωση ετικετών διπλού συσκευή και κλήση άμεσες μεθόδους.

## <a name="protocol-gateway"></a>Πρωτόκολλο πύλης

Μια πύλη πρωτόκολλο συνήθως έχει αναπτυχθεί στο cloud και παρέχει πρωτόκολλο υπηρεσίες μετάφρασης για τη σύνδεση με το Κέντρο IoT συσκευές. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι το Κέντρο IoT Azure;](iot-hub-what-is-iot-hub.md).

## <a name="interactive-message"></a>Αλληλεπιδραστικό μήνυμα

Ένα αλληλεπιδραστικό μήνυμα είναι ένα μήνυμα cloud-σε-συσκευή που προκαλεί μια άμεση ενέργεια σε εφαρμογή παρασκηνίου. Για παράδειγμα, μια συσκευή ενδέχεται να στείλετε μια ειδοποίηση σχετικά με ένα σφάλμα το οποίο θα πρέπει να συνδεθείτε αυτόματα σε ένα σύστημα CRM.

## <a name="iot-hub"></a>Ο διανομέας IoT

IoT διανομέα είναι μια πλήρως διαχειριζόμενη υπηρεσία Azure που επιτρέπει σε αξιόπιστη και ασφαλή διπλής κατεύθυνσης επικοινωνιών μεταξύ εκατομμύρια IoT συσκευές και μια λύση παρασκηνίου. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι το Κέντρο IoT Azure;](iot-hub-what-is-iot-hub.md).

## <a name="iot-suite"></a>IoT οικογένεια προγραμμάτων

Azure οικογένεια IoT μαζί πακέτων πολλές υπηρεσίες Azure με προκαθορισμένες λύσεις. Αυτές τις προκαθορισμένες λύσεις σάς επιτρέπουν να γρήγορα αποτελέσματα με υλοποιήσεις λήξης στο τέλος της συνηθισμένα σενάρια IoT. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι η οικογένεια IoT Azure;](../iot-suite/iot-suite-overview.md).

## <a name="job"></a>Εργασία

Μια [εργασία](iot-hub-devguide-jobs.md) σε διανομέα IoT σάς επιτρέπει να εκτελέσετε λειτουργίες όπως μια υλικολογισμικού αναβάθμισης σε πολλές συσκευές συνδεδεμένη σε διανομέα σας.

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/) είναι ένα από τα πρωτόκολλα ανταλλαγής μηνυμάτων που υποστηρίζει το Κέντρο IoT για την επικοινωνία με συσκευές. Για περισσότερες πληροφορίες σχετικά με τα πρωτόκολλα ανταλλαγής μηνυμάτων που υποστηρίζει IoT διανομέα, ανατρέξτε στο θέμα [Αποστολή και λήψη μηνυμάτων με IoT διανομέα](iot-hub-devguide-messaging.md).

## <a name="reported-properties"></a>Ιδιότητες αναφερόμενου

Στο πλαίσιο της συσκευής twins, αναφερόμενου ιδιότητες χρησιμοποιούνται σε συνδυασμό με τις *επιθυμητές ιδιότητες* για να συγχρονίσετε ρύθμισης παραμέτρων συσκευής ή συνθήκης. Ιδιότητες αναφερόμενου μπορεί να οριστεί μόνο από την εφαρμογή συσκευή και μπορεί να διαβάσει και να αναζητηθούν με εφαρμογή παρασκηνίου.

## <a name="tags"></a>Ετικέτες

Στο πλαίσιο devcie twins, ετικέτες είναι συσκευή μετα-δεδομένα αποθηκεύονται και ανακτώνται από παρασκηνίου εφαρμογή με τη μορφή εγγράφου JSON. Οι ετικέτες δεν είναι ορατές σε εφαρμογές σε μια συσκευή.

## <a name="system-properties"></a>Ιδιότητες συστήματος

Στο πλαίσιο twins συσκευή, ιδιότητες συστήματος είναι μόνο για ανάγνωση και περιλαμβάνουν πληροφορίες σχετικά με τη χρήση συσκευής όπως τελευταία δραστηριότητα κατάσταση χρόνου και σύνδεσης.