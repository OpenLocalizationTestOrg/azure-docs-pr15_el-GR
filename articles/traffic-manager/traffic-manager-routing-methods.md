<properties
    pageTitle="Διαχειριστής κίνηση - κυκλοφορίας μεθόδων δρομολόγησης | Microsoft Azure"
    description="Αυτό το άρθρο θα σας βοηθήσει να κατανοήσετε τις μεθόδους δρομολόγησης διαφορετική κυκλοφορία χρησιμοποιείται από τη Διαχείριση κίνηση"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-traffic-routing-methods"></a>Μέθοδοι δρομολόγησης κίνηση Manager κίνηση

Διαχείριση Azure κίνηση υποστηρίζει τρεις μέθοδοι δρομολόγησης κίνηση για να καθορίσετε τον τρόπο για να δρομολογήσετε την κίνηση του δικτύου για τα διάφορα τελικά σημεία υπηρεσίας. Διαχείριση κυκλοφορίας ισχύει τη μέθοδο δρομολόγησης κίνηση για κάθε ερώτημα DNS που λαμβάνει. Η μέθοδος δρομολόγησης κίνηση καθορίζει ποιο τελικό σημείο επιστραφεί στην απάντηση DNS.

Υποστήριξης της διαχείρισης πόρων Azure για τη Διαχείριση κίνηση χρησιμοποιεί διαφορετική ορολογία σε σχέση με το μοντέλο κλασική ανάπτυξης. Ο παρακάτω πίνακας εμφανίζει τις διαφορές μεταξύ τους όρους της διαχείρισης πόρων και κλασική:

| Διαχείριση πόρων όρων | Κλασική όρων |
|-----------------------|--------------|
| Μέθοδος δρομολόγησης κίνηση | Μέθοδος εξισορρόπησης φόρτου |
| Μέθοδος προτεραιότητα | Μέθοδος ανακατεύθυνσης |
| Μέθοδος σταθμισμένου | Μέθοδος ROUND robin |
| Μέθοδος επιδόσεων | Μέθοδος επιδόσεων |

Θα σας που βασίζεται σε σχόλια των πελατών, αλλάξει την ορολογία για να βελτιώσετε σαφήνεια και να μειώσετε κοινές παρανοήσεις. Δεν υπάρχει διαφορά στη λειτουργία.

Υπάρχουν τρεις μέθοδοι δρομολόγησης κίνηση στη Διαχείριση κίνηση:

- **Προτεραιότητα:** Επιλέξτε "Προτεραιότητα" όταν θέλετε να χρησιμοποιήσετε ένα τελικό σημείο κύρια υπηρεσία για όλη την κυκλοφορία και δώστε αντίγραφα ασφαλείας σε περίπτωση που η κύρια ή τα τελικά σημεία δημιουργίας αντιγράφων ασφαλείας δεν είναι διαθέσιμες.
- **Σταθμισμένων μέσων όρων:** Επιλογή 'σταθμισμένων μέσων όρων' όταν θέλετε να διανείμετε κίνηση σε ένα σύνολο τελικά σημεία, είτε ομοιόμορφα ή σύμφωνα με βάρους, όπου θα καθορίσετε.
- **Επιδόσεων:** Επιλέξτε 'Επιδόσεις' όταν τα τελικά σημεία σε διαφορετικές γεωγραφικές θέσεις και θέλετε οι τελικοί χρήστες για να χρησιμοποιήσετε το τελικό σημείο "πλησιέστερη" όσον αφορά τη χαμηλότερη λανθάνων χρόνος δικτύου.

Όλα τα προφίλ Manager κίνηση περιλαμβάνουν παρακολούθηση της εύρυθμης λειτουργίας του τελικού σημείου και αυτόματη τελικού σημείου ανακατεύθυνσης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση κίνηση τελικού σημείου παρακολούθησης](traffic-manager-monitoring.md). Ένα προφίλ κίνηση Manager να χρησιμοποιήσετε μόνο μία μέθοδο δρομολόγησης κίνηση. Μπορείτε να επιλέξετε μια διαφορετική κυκλοφορία δρομολόγησης μέθοδο για το προφίλ σας οποιαδήποτε στιγμή. Οι αλλαγές εφαρμόζονται μέσα σε ένα λεπτό και προκύψουν χωρίς χρόνου εκτός λειτουργίας. Κίνηση δρομολόγησης μεθόδους μπορεί να συνδυαστεί με τη χρήση ένθετων προφίλ διαχείρισης κίνηση. Ένθεση επιτρέπει εξελιγμένο και ευέλικτη δρομολόγησης κίνηση ρυθμίσεις παραμέτρων που ικανοποιούν τις ανάγκες της μεγαλύτερο, σύνθετες εφαρμογές. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ένθετη προφίλ διαχείρισης κίνηση](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Μέθοδος δρομολόγησης κίνηση προτεραιότητα

Συχνά μια εταιρεία θέλει να παρέχουν αξιοπιστία για τις υπηρεσίες κατά την ανάπτυξη μία ή περισσότερες υπηρεσίες δημιουργίας αντιγράφων ασφαλείας σε περίπτωση που τα κύρια υπηρεσία μεταβαίνει προς τα κάτω. Η μέθοδος δρομολόγησης κίνηση 'Προτεραιότητα' επιτρέπει Azure στους πελάτες για την υλοποίηση εύκολα αυτό το μοτίβο ανακατεύθυνσης.

![Διαχείριση Azure κίνηση 'Προτεραιότητα' μέθοδο δρομολόγησης κίνηση][1]

Στο προφίλ διαχειριστή κίνηση περιέχει μια λίστα προτεραιότητας της υπηρεσίας τελικά σημεία. Από προεπιλογή, η διαχείριση κίνηση αποστέλλει όλη την κυκλοφορία στο κύριο τελικό σημείο (υψηλότερη προτεραιότητα). Εάν το πρωτεύον τελικό σημείο δεν είναι διαθέσιμη, Manager κίνηση δρομολογεί την κίνηση στο δεύτερο τελικό σημείο. Εάν τα κύριας και δευτερεύουσας τελικά σημεία δεν είναι διαθέσιμες, η κίνηση μεταβαίνει το τρίτο, και ούτω καθεξής. Διαθεσιμότητα του τελικού σημείου είναι βάσει της κατάστασης ρύθμισης παραμέτρων (ενεργοποιημένη ή απενεργοποιημένη) και την παρακολούθηση βρίσκεται σε εξέλιξη τελικού σημείου.

### <a name="configuring-endpoints"></a>Ρύθμιση παραμέτρων τελικά σημεία

Με το Azure διαχείριση πόρων, μπορείτε να ρυθμίσετε την προτεραιότητα τελικού σημείου ρητά χρησιμοποιώντας την ιδιότητα "προτεραιότητα" για κάθε τελικό σημείο. Αυτή η ιδιότητα είναι μια τιμή μεταξύ 1 και 1000. Κατώτερες τιμές αντιπροσωπεύουν υψηλότερη προτεραιότητα. Τα τελικά σημεία δεν είναι δυνατό να κάνετε κοινή χρήση τιμές προτεραιότητας. Ρύθμιση της ιδιότητας είναι προαιρετικό. Όταν παραλειφθεί, χρησιμοποιείται η προεπιλεγμένη προτεραιότητα με βάση τη σειρά τελικού σημείου.

Με το περιβάλλον εργασίας κλασική, την προτεραιότητα τελικό σημείο έχει ρυθμιστεί ρητά. Η προτεραιότητα βασίζεται στη σειρά με την οποία παρατίθενται τα τελικά σημεία στον ορισμό προφίλ.

## <a name="weighted-traffic-routing-method"></a>Μέθοδος σταθμισμένου δρομολόγησης κίνηση

Η μέθοδος 'Σταθμισμένου' κίνηση δρομολόγησης σας επιτρέπει να ομοιόμορφη κατανομή κίνηση ή να χρησιμοποιήσετε μια προκαθορισμένη συντελεστή.

![Azure κίνηση 'Σταθμισμένου' κίνηση δρομολόγησης μέθοδο διαχείρισης][2]

Στη σταθμισμένου μέθοδο δρομολόγησης κίνηση, μπορείτε να εκχωρήσετε ένα πάχος κάθε τελικό σημείο στη ρύθμιση παραμέτρων προφίλ Manager κίνηση. Το πάχος είναι ακέραιος αριθμός από 1 έως 1000. Αυτή η παράμετρος είναι προαιρετικό. Εάν παραλειφθεί το όρισμα, οι διαχειριστές κίνηση χρησιμοποιεί ένα προεπιλεγμένο πάχος "1".

Για κάθε ερώτημα DNS που λάβατε, Διαχείριση κίνηση επιλέγει τυχαία διαθέσιμη τελικού σημείου. Την πιθανότητα της επιλογής ενός ορίου είναι με βάση τα βάρη που έχουν εκχωρηθεί σε όλα τα διαθέσιμα τελικά σημεία. Χρησιμοποιώντας το ίδιο βάρος σε όλα τα τελικά σημεία αποτελέσματα σε μια κατανομή even κίνηση. Χρήση μεγαλύτερη ή μικρότερη βάρους σε συγκεκριμένες τελικά σημεία προκαλεί αυτά τα τελικά σημεία για να επιστραφούν περισσότερο ή λιγότερο συχνά σε των αποκρίσεων DNS.

Η μέθοδος σταθμισμένου επιτρέπει ορισμένες χρήσιμες σενάρια:

- Αναβάθμιση σταδιακή εφαρμογή: εκχώρηση ποσοστό της κίνησης για να δρομολογήσετε ένα νέο τελικό σημείο και Αυξήστε σταδιακά την κυκλοφορία σταδιακά σε 100%.
- Εφαρμογή μετεγκατάστασης στο Azure: Δημιουργήστε ένα προφίλ με τόσο Azure και στις εξωτερικές τελικά σημεία. Προσαρμόστε το πάχος της τα τελικά σημεία για να προτιμάτε τα νέα τελικά σημεία.
- Σύννεφο πλούσια για πρόσθετη χωρητικότητα: ανάπτυξη γρήγορα μια ανάπτυξη εσωτερικής εγκατάστασης στο cloud, τοποθετώντας το πίσω από ένα προφίλ διαχείρισης κίνηση. Όταν χρειάζεστε επιπλέον χωρητικότητα στο cloud, μπορείτε να προσθέσετε ή να ενεργοποιήσετε περισσότερες τελικά σημεία και να καθορίσετε ποιο τμήμα της κυκλοφορίας μεταβαίνει σε κάθε τελικό σημείο.

Η νέα πύλη Azure υποστηρίζει τις παραμέτρους δρομολόγησης σταθμισμένου κίνηση. Βάρους δεν μπορεί να ρυθμιστεί στην πύλη του κλασική. Μπορείτε επίσης να ρυθμίσετε χρησιμοποιώντας την επιλογή διαχείρισης πόρων και κλασική εκδόσεις του Azure PowerShell, CLI και τα ΥΠΌΛΟΙΠΑ API βάρους.

Είναι σημαντικό να κατανοήσετε ότι DNS απαντήσεων αποθηκεύονται στο cache από προγράμματα-πελάτες και διακομιστές DNS της περιοδικότητας που χρησιμοποιούν τα προγράμματα-πελάτες για την επίλυση ονομάτων DNS. Σε αυτό σε cache μπορεί να επηρεάσουν στην κατανομές σταθμισμένου κίνηση. Όταν ο αριθμός των προγράμματα-πελάτες και διακομιστές DNS περιοδικότητας είναι μεγάλο, διανομής κίνηση λειτουργεί όπως αναμένεται. Ωστόσο, κατά τον αριθμό των πελατών ή επαναλαμβανόμενη οι διακομιστές DNS είναι μικρό, σε cache να σημαντικά skew την κατανομή κίνηση.

Συνήθεις περιπτώσεις χρήσης περιλαμβάνονται οι εξής:

- Περιβάλλοντα ανάπτυξης και δοκιμής
- Επικοινωνίας για εφαρμογής
- Εφαρμογές που στοχεύουν σε μια στενή χρήστη-βάση που κάνουν κοινή χρήση υποδομή κοινές επαναλαμβανόμενες DNS (για παράδειγμα, των υπαλλήλων της εταιρείας τη σύνδεση μέσω ενός διακομιστή μεσολάβησης)

Αυτά τα εφέ σε cache DNS είναι κοινά για όλα DNS με βάση την κυκλοφορία δρομολόγησης συστήματα, όχι μόνο Azure κίνηση Manager. Σε ορισμένες περιπτώσεις, ρητά εκκαθάριση του cache του DNS μπορεί να σας παρέχει μια λύση. Σε άλλες περιπτώσεις, ίσως είναι πιο κατάλληλη μια εναλλακτική μέθοδο δρομολόγησης κίνηση.

## <a name="performance-traffic-routing-method"></a>Μέθοδος δρομολόγησης κίνηση επιδόσεων

Ανάπτυξη τελικά σημεία σε δύο ή περισσότερες θέσεις σε ολόκληρο τον κόσμο να βελτιώσετε την ανταπόκριση της πολλές εφαρμογές με δρομολόγησης κίνηση στη θέση που είναι 'πλησιέστερη' για εσάς. Η μέθοδος δρομολόγησης κίνηση 'Επιδόσεις' παρέχει αυτήν τη δυνατότητα.

![Διαχείριση Azure κίνηση 'Επιδόσεις' μέθοδο δρομολόγησης κίνηση][3]

Το τελικό σημείο 'πλησιέστερη' δεν είναι απαραίτητα πλησιέστερη που μετράται με βάση τη γεωγραφική απόσταση. Αντί για αυτό, η μέθοδος δρομολόγησης κίνηση 'Επιδόσεις' καθορίζει το τελικό σημείο της πλησιέστερης μετρώντας λανθάνων χρόνος δικτύου. Διαχείριση κίνηση διατηρεί έναν πίνακα λανθάνων χρόνος Internet για την παρακολούθηση του χρόνου μετάβασης και επιστροφής μεταξύ περιοχές διευθύνσεων IP και κάθε Azure κέντρου δεδομένων.

Διαχείριση κίνηση αναζητά τη διεύθυνση IP προέλευσης της εισερχόμενης αίτησης DNS στον πίνακα λανθάνων χρόνος Internet. Διαχείριση κίνηση επιλέγει διαθέσιμη τελικού σημείου στο Azure κέντρο δεδομένων που περιλαμβάνει τη χαμηλότερη λανθάνων χρόνος για την περιοχή αυτή τη διεύθυνση IP και, στη συνέχεια, επιστρέφει αυτό το τελικό σημείο της απόκρισης DNS.

Όπως εξηγείται στο θέμα [Τρόπος λειτουργίας της διαχείρισης κίνηση](traffic-manager-how-traffic-manager-works.md), κίνηση Manager δεν λαμβάνει ερωτήματα DNS απευθείας από προγράμματα-πελάτες. Αντίθετα, τα ερωτήματα DNS που προέρχονται από της υπηρεσίας DNS επαναλαμβανόμενες ότι οι υπολογιστές-πελάτες έχουν ρυθμιστεί για να χρησιμοποιήσετε. Γι ' αυτό, η διεύθυνση IP χρησιμοποιείται για να προσδιορίσετε το τελικό σημείο 'πλησιέστερη' δεν είναι διεύθυνση IP του υπολογιστή-πελάτη, αλλά είναι η διεύθυνση IP της υπηρεσίας DNS περιοδικότητας. Στην πράξη, αυτή η διεύθυνση IP είναι μια καλή διακομιστή μεσολάβησης για το πρόγραμμα-πελάτη.

Διαχείριση κίνηση ενημερώνει τακτικά τον πίνακα λανθάνων χρόνος Internet για να αντιμετωπίσετε αλλαγές στο καθολικό Internet και νέες περιοχές Azure. Ωστόσο, επιδόσεις εφαρμογής ποικίλλει, ανάλογα με παραλλαγές σε πραγματικό χρόνο στο φόρτωσης μέσω του Internet. Κίνηση δρομολόγησης επιδόσεις δεν παρακολουθεί φόρτωση σε μια δεδομένη υπηρεσία τελικού σημείου. Ωστόσο, εάν ένα τελικό σημείο δεν είναι διαθέσιμη, Διαχείριση κίνηση σημαίνει δεν το σε αποκρίσεις ερωτήματος DNS.

Σημεία για να λάβετε υπόψη:

- Εάν το προφίλ σας περιέχει πολλές τελικά σημεία στην ίδια περιοχή Azure, στη συνέχεια, Διαχείριση κίνηση κατανέμει την κυκλοφορία ομοιόμορφα τα τελικά σημεία διαθέσιμα σε αυτήν την περιοχή. Εάν προτιμάτε μια κατανομή διαφορετική κυκλοφορία μέσα σε μια περιοχή, μπορείτε να χρησιμοποιήσετε [ένθετη προφίλ διαχείρισης κίνηση](traffic-manager-nested-profiles.md).

- Εάν όλα τα τελικά σημεία με δυνατότητα σε μια δεδομένη περιοχή Azure είναι υποβαθμισμένο, Διαχείριση κίνηση κατανέμει την κυκλοφορία όλα άλλα διαθέσιμα τα τελικά σημεία αντί για το επόμενο πλησιέστερη τελικό σημείο. Αυτή η λογική αποτρέπει που μεσολαβούν από δεν υπερφόρτωσης το επόμενο πλησιέστερη τελικό σημείο επικαλυπτόμενου αποτυχία. Εάν θέλετε να ορίσετε μια ακολουθία προτιμώμενη ανακατεύθυνσης, χρησιμοποιήστε [ένθετη προφίλ διαχείρισης κίνηση](traffic-manager-nested-profiles.md).

- Όταν χρησιμοποιείτε τη μέθοδο δρομολόγησης κίνηση απόδοσης με εξωτερική τελικά σημεία ή ένθετη τελικά σημεία, πρέπει να καθορίσετε τη θέση του αυτά τα τελικά σημεία. Επιλογή της πλησιέστερης για την ανάπτυξη Azure περιοχής. Αυτές τις θέσεις, οι τιμές που υποστηρίζεται από τον πίνακα λανθάνων χρόνος Internet.

- Ο αλγόριθμος που επιλέγει το τελικό σημείο είναι ντετερμινιστική. Επαναλαμβανόμενες ερωτήματα DNS από το ίδιο πρόγραμμα-πελάτη σας κατευθύνουν στο ίδιο τελικό σημείο. Συνήθως, οι υπολογιστές-πελάτες χρησιμοποιούν διαφορετικές επαναλαμβανόμενες διακομιστές DNS όταν ταξιδεύετε. Ο υπολογιστής-πελάτης μπορεί να δρομολογηθούν σε διαφορετικό endpoint. Δρομολόγηση μπορούν να ρυθμιστούν με ενημερώσεις για τον πίνακα λανθάνων χρόνος στο Internet. Γι ' αυτό, η μέθοδος δρομολόγησης κίνηση επιδόσεων δεν εγγυάται ότι ένα πρόγραμμα-πελάτη δρομολογείται πάντα στο ίδιο τελικό σημείο.

- Όταν ο πίνακας λανθάνων χρόνος Internet αλλάζει, ενδέχεται να παρατηρήσετε ότι ορισμένα προγράμματα-πελάτες σας κατευθύνουν σε διαφορετικό endpoint. Αυτή η αλλαγή δρομολόγησης είναι πιο ακριβή με βάση τα τρέχοντα δεδομένα λανθάνων χρόνος. Οι ενημερώσεις αυτές είναι απαραίτητες για να διατηρήσετε την ακρίβεια της απόδοσης κίνηση δρομολόγησης καθώς εξελίσσεται συνεχώς στο Internet.

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε πώς μπορείτε να αναπτύξετε εφαρμογές υψηλής διαθεσιμότητας χρησιμοποιώντας [κίνηση Manager τελικού σημείου παρακολούθησης](traffic-manager-monitoring.md)

Μάθετε πώς μπορείτε να [δημιουργήσετε ένα προφίλ διαχείρισης κίνηση](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png