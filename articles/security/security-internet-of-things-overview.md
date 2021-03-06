<properties
   pageTitle="Επισκόπηση ασφαλείας Internet του πράγματα | Microsoft Azure"
   description=" Azure internet των υπηρεσιών πράγματα (IoT) προσφέρουν ένα ευρύ φάσμα δυνατοτήτων. Σε αυτό το άρθρο σάς βοηθά να κατανοήσετε τον τρόπο για την ασφάλιση σας λύσεις IoT στο Azure. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="internet-of-things-security-overview"></a>Επισκόπηση ασφαλείας Internet των στοιχείων

Azure internet των υπηρεσιών πράγματα (IoT) προσφέρουν ένα ευρύ φάσμα δυνατοτήτων. Αυτές οι υπηρεσίες βαθμολογίας για μεγάλες επιχειρήσεις σάς επιτρέπουν να:

- Συλλογή δεδομένων από συσκευές
- Ανάλυση δεδομένων ροών στο κίνησης
- Αποθήκευση και ερωτήματος μεγάλα σύνολα δεδομένων
- Απεικόνιση δεδομένων σε πραγματικό χρόνο και ιστορικού
- Ενοποίηση με συστήματα πίσω στο office

Για την παροχή αυτών των δυνατοτήτων, οικογένεια IoT Azure πακέτων μαζί πολλών Azure υπηρεσιών με προσαρμοσμένες επεκτάσεις ως προκαθορισμένες λύσεις. Αυτές οι προκαθορισμένες λύσεις είναι βάσης υλοποιήσεις κοινά μοτίβων λύση IoT που σας βοηθούν να μειώσετε το χρόνο που αφιερώνετε για την παράδοση του IoT λύσεις. Χρήση του Κιτ ανάπτυξης λογισμικού IoT, μπορείτε να προσαρμόσετε και επέκτασης αυτών των λύσεων να πληροί τις απαιτήσεις του δικού σας. Μπορείτε επίσης να χρησιμοποιήσετε αυτές τις λύσεις ως παραδείγματα ή πρότυπα, όταν σχεδιάζετε νέες IoT λύσεις.

Την οικογένεια προγραμμάτων του Azure IoT είναι μια ισχυρή λύση για τις ανάγκες σας IoT. Ωστόσο, είναι upmost σημασίας που έχουν σχεδιαστεί σας λύσεις IoT με ασφάλεια υπόψη από την αρχή. Λόγω του μεγάλου αριθμού IoT συσκευών, οποιοδήποτε συμβάν ασφαλείας μπορεί να γίνει γρήγορα ένα εκτεταμένο συμβάν με σημαντικές συνέπειες.

Για να σας βοηθήσει να κατανοήσετε τον τρόπο για την ασφάλιση σας λύσεις IoT, έχουμε τις ακόλουθες πληροφορίες.

## <a name="security-architecture"></a>Αρχιτεκτονική ασφαλείας

Όταν σχεδιάζετε ένα σύστημα, είναι σημαντικό να κατανοήσετε τις πιθανές απειλές σε αυτό το σύστημα και προσθέστε κατάλληλες λογισμικού άμυνας αντίστοιχα, όπως το σύστημα έχει σχεδιαστεί και σχεδιασμένος. Είναι ιδιαίτερα σημαντικό για να σχεδιάσετε το προϊόν από την αρχή με βάση την ασφάλεια, επειδή η κατανόηση πώς ένας εισβολέας ίσως να υπονομεύσει το σύστημά σας βοηθά να είστε βέβαιοι κατάλληλα μετριασμούς είναι στη θέση από την αρχή.

Μπορείτε να μάθετε σχετικά με την αρχιτεκτονική ασφαλείας IoT διαβάζοντας [Αρχιτεκτονική ασφαλείας Internet πράγματα](../iot-suite/iot-security-architecture.md).

Σε αυτό το άρθρο αναλύει τα ακόλουθα θέματα:

- [Ασφάλεια ξεκινά με ένα μοντέλο απειλή](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
- [Ασφάλεια σε IoT](../iot-suite/iot-security-architecture.md#security-in-iot)
- [Απειλή μοντελοποίησης η αρχιτεκτονική Azure IoT αναφοράς](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a>Ασφάλεια από το μηδέν προς τα επάνω

Το IoT αποτελεί μοναδική ασφάλεια, προστασία προσωπικών δεδομένων και συμμόρφωση προκλήσεις για επιχειρήσεις σε όλο τον κόσμο. Σε αντίθεση με τεχνολογία παραδοσιακή cyber όπου αυτά τα θέματα μισή περιστροφή γύρω από λογισμικό και το πώς έχει υλοποιηθεί, IoT ζητήματα για το τι συμβαίνει όταν το cyber και στον κόσμο φυσικής προσέγγιση. Προστασία λύσεις IoT απαιτεί ταυτόχρονα εξασφαλίζετε ότι οι ασφαλή προμήθεια των συσκευών, ασφαλή σύνδεση ανάμεσα σε αυτές τις συσκευές και το σύννεφο και προστασία ασφαλούς δεδομένων στο cloud κατά τη διάρκεια της επεξεργασίας και αποθήκευσης. Εργασία σε σχέση με αυτή τη λειτουργικότητα, ωστόσο, είναι περιορισμένες πόρων συσκευές, γεωγραφική κατανομή των αναπτύξεις και μεγάλου αριθμού συσκευές μέσα σε μια λύση.

Μπορείτε να μάθετε τον τρόπο χειρισμού ασφαλείας σε αυτές τις περιοχές, διαβάζοντας [ασφαλείας Internet των στοιχείων από το μηδέν προς τα επάνω](../iot-suite/securing-iot-ground-up.md).

Το άρθρο περιγράφει τα ακόλουθα θέματα:

- [Ασφαλή υποδομή από το μηδέν προς τα επάνω](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
- [Microsoft Azure – ασφαλή IoT υποδομή για την επιχείρησή σας](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Βέλτιστες πρακτικές

Ασφάλιση μιας υποδομής IoT απαιτεί μια αυστηρών στρατηγική ασφαλείας σε βάθος. Ξεκινώντας από την προστασία των δεδομένων στο cloud, για να προστασία ακεραιότητας δεδομένων ενώ κατά τη μεταφορά μέσω του δημόσιου internet και παρέχει τη δυνατότητα να ασφαλή προμήθεια συσκευές, κάθε επίπεδο δημιουργεί μεγαλύτερη ασφάλεια διασφάλιση στην συνολική υποδομής.

Μπορείτε να μάθετε σχετικά με την ασφάλεια στο Internet του πράγματα βέλτιστες πρακτικές διαβάζοντας [βέλτιστες πρακτικές ασφαλείας Internet των στοιχείων](../iot-suite/iot-security-best-practices.md).

Το άρθρο περιγράφει τα ακόλουθα θέματα:

- [Κατασκευαστής υλικού IoT/ολοκληρωτή](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
- [Για προγραμματιστές λύσεων IoT](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
- [Deployer λύση IoT](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
- [Τελεστής λύση IoT](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
