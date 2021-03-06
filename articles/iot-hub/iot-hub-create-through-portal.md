<properties
     pageTitle="Χρησιμοποιήστε την πύλη του Azure για να δημιουργήσετε ένα διανομέα IoT | Microsoft Azure"
     description="Μάθετε πώς να δημιουργήσετε και να διαχειριστείτε διανομείς Azure IoT μέσω της πύλης Azure"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-the-azure-portal"></a>Δημιουργήστε ένα διανομέα IoT με την πύλη Azure

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>Εισαγωγή

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να βρείτε την υπηρεσία IoT διανομέα στην πύλη του Azure και πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε IoT διανομείς.

## <a name="where-to-find-iot-hubs"></a>Πού θα βρείτε διανομείς IoT

Υπάρχουν διάφορες θέσεις όπου μπορείτε να βρείτε IoT διανομείς.

1. **+ Δημιουργία**: **Azure IoT διανομέα** είναι μια υπηρεσία IoT και μπορείτε να βρείτε στην κατηγορία **Internet των στοιχείων**, στην περιοχή **+ Δημιουργία**, παρόμοια με άλλες υπηρεσίες.

2. Διανομείς IoT επίσης είναι δυνατή η πρόσβαση μέσω του Marketplace ως την κύρια υπηρεσία στην περιοχή **Internet των στοιχείων**.

## <a name="create-an-iot-hub"></a>Δημιουργήστε ένα διανομέα IoT

Μπορείτε να δημιουργήσετε ένα διανομέα IoT χρησιμοποιώντας τις ακόλουθες μεθόδους:

- Τη δημιουργία ενός διανομέα IoT μέσω της επιλογής **+ Δημιουργία** υποψήφιων πελατών για να το blade φαίνεται στην επόμενη οθόνη στιγμιότυπο. Τα βήματα για να δημιουργήσετε την ενότητα IoT μέσω αυτήν τη μέθοδο και το marketplace είναι πανομοιότυπες.

- Δημιουργία ενός διανομέα IoT έως το Marketplace: κάνοντας κλικ στην επιλογή **Δημιουργία** ανοίγει μια blade που είναι ίδιος με το προηγούμενο blade για την εμπειρία **+ νέο** . Οι επόμενες ενότητες λίστας τα πολλά βήματα που σχετίζεται με ένα διανομέα IoT Δημιουργία.

### <a name="choose-the-name-of-the-iot-hub"></a>Επιλέξτε το όνομα του διανομέα IoT

Για να δημιουργήσετε ένα IoT διανομέα, πρέπει να ονομάσετε την ενότητα. Αυτό το όνομα πρέπει να είναι μοναδικό σε οι διανομείς. Χωρίς αναπαραγωγή διανομείς επιτρέπεται από την πίσω πλευρά, επομένως συνιστάται ότι αυτή η ενότητα ονομάζεται όσο το δυνατόν μοναδικά.

### <a name="choose-the-pricing-tier"></a>Επιλέξτε το επίπεδο τιμολόγησης

Μπορείτε να επιλέξετε από τέσσερα επίπεδα: **δωρεάν**, **Τυπική 1** και **2 τυπική**και **Τυπική S3**. Η σειρά δωρεάν επιτρέπει σε συσκευές μόνο 500 να είστε συνδεδεμένοι στο διανομέα IoT και σε έως 8.000 μηνύματα την ημέρα.

**Τυπική S1**: η έκδοση IoT S1 διανομείς έχει σχεδιαστεί για λύσεις IoT που έχουν πολλές συσκευές δημιουργό σχετικά μικρές ποσότητες δεδομένων ανά συσκευή. Κάθε μονάδα της έκδοσης S1 επιτρέπει έως 400.000 μηνύματα ανά ημέρα σε όλες τις συνδεδεμένες συσκευές.

**Τυπική S2**: η έκδοση S2 διανομέα IoT έχει σχεδιαστεί για λύσεις IoT στην οποία συσκευές δημιουργία μεγάλες ποσότητες δεδομένων. Κάθε μονάδα της έκδοσης S2 επιτρέπει μηνύματα έως 6 εκατομμύρια ανά ημέρα ανάμεσα σε όλες τις συνδεδεμένες συσκευές.

**Τυπική S3**: η έκδοση S3 διανομέα IoT έχει σχεδιαστεί για λύσεις IoT που δημιουργούν μεγάλες ποσότητες δεδομένων. Κάθε μονάδα της έκδοσης S3 επιτρέπει μηνύματα έως 300 εκατομμύρια ανά ημέρα ανάμεσα σε όλες τις συνδεδεμένες συσκευές.

![][4]

> [AZURE.NOTE] Ενότητα IoT επιτρέπει μόνο μία δωρεάν διανομέα ανά Azure συνδρομή.

### <a name="iot-hub-units"></a>Μονάδες διανομέα IoT

Μια μονάδα διανομέα IoT περιλαμβάνει ορισμένα μηνύματα την ημέρα. Τον συνολικό αριθμό των μηνυμάτων που υποστηρίζονται για αυτό το διανομέα είναι ο αριθμός των μονάδων πολλαπλασιασμένο με τον αριθμό των μηνυμάτων ανά ημέρα για αυτήν τη σειρά. Για παράδειγμα, εάν θέλετε την ενότητα IoT για την υποστήριξη είσοδος 700,000 μηνύματα, μπορείτε να επιλέξετε δύο μονάδες επίπεδο S1.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Συσκευή για να τα διαμερίσματα cloud και ομάδα πόρων

Μπορείτε να αλλάξετε τον αριθμό των διαμερισμάτων για ένα διανομέα IoT. Προεπιλεγμένα διαμερίσματα έχουν οριστεί 4. Ωστόσο, μπορείτε να επιλέξετε διαφορετικό αριθμό διαμερίσματα από μια αναπτυσσόμενη λίστα.

Για τις ομάδες πόρων, δεν χρειάζεται να ρητά να δημιουργήσετε μια ομάδα κενή πόρων. Κατά τη δημιουργία ενός πόρου, μπορείτε να επιλέξετε για να δημιουργήσετε μια νέα ομάδα πόρων ή να χρησιμοποιήσετε μια υπάρχουσα ομάδα πόρων.

![][5]

### <a name="choose-subscriptions"></a>Επιλέξτε συνδρομών

Azure διανομέα IoT εμφανίζει αυτόματα τη λίστα Azure συνδρομών με την οποία συνδέεται ο λογαριασμός χρήστη. Μπορείτε να επιλέξετε μία από τις επιλογές για να συσχετίσετε την ενότητα IoT με αυτήν τη συνδρομή Azure.

### <a name="choose-the-location"></a>Επιλέξτε τη θέση

Η επιλογή θέσης παρέχει μια λίστα με τις περιοχές στις οποίες προσφέρεται IoT διανομέα. Ενότητα IoT είναι διαθέσιμη για την ανάπτυξη στις ακόλουθες θέσεις: Ανατολή Αυστραλίας, νοτιοανατολικής Αυστραλίας, Ανατολικής Ασίας, νοτιοανατολικής Ασίας, Βόρεια Ευρώπη, Δυτική Ευρώπη, Ιαπωνία Ανατολή, Δυτική, Ιαπωνία ΜΑΣ Ανατολή, Δυτική ΜΑΣ.

### <a name="create-the-iot-hub"></a>Δημιουργήστε την ενότητα IoT

Όταν ολοκληρώσετε όλα τα προηγούμενα βήματα, την ενότητα IoT είναι έτοιμος να δημιουργηθεί. Κάντε κλικ στην επιλογή **Δημιουργία** για να ξεκινήσετε τη διαδικασία παρασκηνίου της δημιουργίας αυτό διανομέα IoT με τις συγκεκριμένες επιλογές και να αναπτύξετε το στη θέση που καθορίζεται.

Μπορεί να χρειαστούν μερικά λεπτά για την ενότητα IoT να δημιουργηθεί, όπως ο χρόνος για την ανάπτυξη παρασκηνίου να εμφανίζεται στους διακομιστές κατάλληλη θέση.

## <a name="change-the-settings-of-the-iot-hub"></a>Αλλαγή των ρυθμίσεων του διανομέα IoT

Μπορείτε να αλλάξετε τις ρυθμίσεις από μια υπάρχουσα ενότητα IoT αφού δημιουργηθεί από το blade IoT διανομέα.

![][8]

**Κοινή χρήση πολιτικές πρόσβασης**: αυτές οι πολιτικές ορίζουν τα δικαιώματα για συσκευές και υπηρεσίες για να συνδεθείτε με το Κέντρο IoT. Μπορείτε να αποκτήσετε πρόσβαση σε αυτές τις πολιτικές κάνοντας κλικ στην επιλογή **Κοινή χρήση πολιτικές πρόσβασης** στην περιοχή **Γενικά**. Σε αυτό το blade, μπορείτε να τροποποιήσετε τις υπάρχουσες πολιτικές ή να προσθέσετε μια νέα πολιτική.

### <a name="create-a-policy"></a>Δημιουργία πολιτικής

- Κάντε κλικ στο κουμπί **Προσθήκη** για να ανοίξετε μια blade. Εδώ μπορείτε να εισαγάγετε το νέο όνομα πολιτικής και τα δικαιώματα που θέλετε να συσχετίσετε με αυτήν την πολιτική, όπως φαίνεται στην παρακάτω εικόνα.

    Υπάρχουν πολλά δικαιώματα που μπορούν να συσχετιστούν με αυτές τις πολιτικές κοινόχρηστο. Τα πρώτα δύο πολιτικές, **μητρώο ανάγνωση** και **εγγραφή μητρώου**, εκχώρηση Διαβάστε και τα δικαιώματα πρόσβασης εγγραφής στο χώρο αποθήκευσης ταυτότητας συσκευή ή το μητρώο ταυτότητα. Επιλογή της δυνατότητας εγγραφής αυτόματα επιλέγει την επιλογή Διαβάστε επίσης.

    Η πολιτική **σύνδεση υπηρεσίας** εκχωρεί δικαιώματα πρόσβασης τα τελικά σημεία πλευρά του cloud, όπως την ομάδα καταναλωτή για τη σύνδεση με την ενότητα IoT τις υπηρεσίες. Η πολιτική **σύνδεση συσκευής** εκχωρεί δικαιώματα για την αποστολή και λήψη μηνυμάτων σε τα τελικά σημεία συσκευή πλευρά του διανομέα IoT.

- Κάντε κλικ στην επιλογή **Δημιουργία** για να προσθέσετε αυτό που μόλις δημιουργήσατε πολιτικής στην υπάρχουσα λίστα.

![][10]

## <a name="messaging"></a>Ανταλλαγή μηνυμάτων

Κάντε κλικ στην επιλογή **μηνυμάτων** για να εμφανίσετε μια λίστα των μηνυμάτων ιδιότητες για την ενότητα IoT τροποποιούνται τη συγκεκριμένη στιγμή. Υπάρχουν δύο κύριοι τύποι ιδιοτήτων που μπορείτε να τροποποιήσετε ή να αντιγράψετε: **Cloud στη συσκευή** και **συσκευής στο Cloud**.

- Ρυθμίσεις **cloud συσκευή** : Αυτή η ρύθμιση έχει δύο subsettings: **Cloud προκειμένου να συσκευή TTL** (time-to-live) και **χρόνο διατήρησης** για τα μηνύματα. Όταν ο διανομέας IoT δημιουργείται για πρώτη φορά, και οι δύο αυτές τις ρυθμίσεις δημιουργούνται με προεπιλεγμένη τιμή από μία ώρα. Για να προσαρμόσετε αυτές τις τιμές, χρησιμοποιήστε τα ρυθμιστικά ή πληκτρολογήστε τις τιμές.

- Ρυθμίσεις **συσκευής στο Cloud** : Αυτή η ρύθμιση έχει πολλές subsettings, ορισμένες από τις οποίες με το όνομα/εκχωρούνται όταν ο διανομέας IoT δημιουργείται και μόνο μπορεί να αντιγραφεί σε άλλα subsettings με δυνατότητα προσαρμογής. Αυτές οι ρυθμίσεις παρατίθενται στην επόμενη ενότητα.

**Τα διαμερίσματα**: Αυτή η τιμή ορίζεται κατά την ενότητα IoT δημιουργείται και μπορεί να αλλάξει σε αυτήν τη ρύθμιση.

**Όνομα διανομέας συμβατό με το συμβάν και τελικού σημείου**: δημιουργείται όταν το IoT διανομέα, ένα συμβάν διανομέα δημιουργείται εσωτερικά ότι ενδέχεται να χρειάζεστε πρόσβαση υπό ορισμένες συνθήκες. Αυτό το όνομα διανομέας συμβατό με το συμβάν και τελικού σημείου δεν μπορεί να προσαρμοστεί, αλλά είναι διαθέσιμο για χρήση μέσω στο κουμπί **Αντιγραφή** .

**Ώρα διατήρησης**: Ορίστε σε μία ημέρα, από προεπιλογή, αλλά μπορεί να προσαρμοστεί για άλλες τιμές χρησιμοποιώντας την αναπτυσσόμενη λίστα. Αυτή η τιμή είναι σε ημέρες για τη συσκευή στο Cloud και όχι σε ώρες, όπως είναι η ρύθμιση παρόμοια για Cloud στη συσκευή.

**Ομάδες καταναλωτή**: καταναλωτή οι ομάδες είναι μια ρύθμιση παρόμοια με άλλα συστήματα ανταλλαγής μηνυμάτων που μπορεί να χρησιμοποιηθεί για την έλξη δεδομένων με συγκεκριμένους τρόπους για να συνδεθείτε άλλες εφαρμογές ή υπηρεσίες με IoT διανομέα. Κάθε ενότητα IoT δημιουργείται με μια προεπιλεγμένη ομάδα καταναλωτών. Ωστόσο, μπορείτε να προσθέσετε ή να διαγράψετε καταναλωτή ομάδες για να σας IoT διανομείς.

> [AZURE.NOTE] Στην προεπιλεγμένη ομάδα καταναλωτή δεν είναι δυνατή η επεξεργασία ή να διαγραφεί.

![][11]

## <a name="pricing-and-scale"></a>Τιμές και κλίμακας

Η τιμολόγηση από μια υπάρχουσα ενότητα IoT μπορεί να αλλάξει τις ρυθμίσεις **Τιμολόγηση** , με τις εξής εξαιρέσεις:

- Κατά την τρέχουσα εφαρμογή, δεν είναι δυνατό να αλλάξετε ένα διανομέα IoT με ένα δωρεάν SKU βαθμίδες σε μία από τις SKU επί πληρωμή, ή το αντίστροφο.
- Μπορεί να υπάρξει μόνο μία ενότητα IoT δωρεάν επιπέδων στην Azure συνδρομής.

![][12]

Μετακίνηση από το ανώτερο επίπεδο (S2 ή S3) στο κάτω επίπεδο (S1 ή S2) επιτρέπεται μόνο όταν ο αριθμός των μηνυμάτων που αποστέλλονται για την ημέρα που δεν βρίσκονται σε διένεξη. Για παράδειγμα, εάν ο αριθμός των μηνυμάτων ανά ημέρα υπερβαίνει 400,000, στη συνέχεια, το επίπεδο για την ενότητα IoT μπορεί να αλλάξει. Ωστόσο, εάν αλλάξετε στο επίπεδο S1, στη συνέχεια, την ενότητα είναι επιβραδύνει για αυτήν την ημέρα.

## <a name="delete-the-iot-hub"></a>Διαγράψτε την ενότητα IoT

Μπορείτε να περιηγηθείτε στο διανομέα IoT που θέλετε να διαγράψετε, κάνοντας κλικ στην επιλογή **Αναζήτηση**και, στη συνέχεια, επιλέξτε την κατάλληλη ενότητα για να διαγράψετε. Κάντε κλικ στο κουμπί **Διαγραφή** κάτω από το όνομα διανομέας για να διαγράψετε την ενότητα.

## <a name="next-steps"></a>Επόμενα βήματα

Ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα σχετικά με τη Διαχείριση Azure IoT διανομέα:

- [Μαζική Διαχείριση συσκευών IoT][lnk-bulk]
- [Χρήση μετρικά][lnk-metrics]
- [Λειτουργίες παρακολούθησης][lnk-monitor]

Για να εξερευνήσετε περαιτέρω τις δυνατότητες του IoT διανομέα, ανατρέξτε στα θέματα:

- [Οδηγός για προγραμματιστές][lnk-devguide]
- [Προσομοίωση μια συσκευή με το SDK IoT πύλης][lnk-gateway]
- [Ασφαλούς τη λύση IoT σας από το μηδέν προς τα επάνω][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md