<properties 
   pageTitle="Προβολή και διαχείριση ειδοποιήσεων StorSimple | Microsoft Azure"
   description="Περιγράφει StorSimple συνθήκες ειδοποίησης και της σοβαρότητας, πώς μπορείτε να ρυθμίσετε τις παραμέτρους οι ειδοποιήσεις και πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία διαχείρισης StorSimple για να διαχειριστείτε τις ειδοποιήσεις."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="anbacker" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-alerts"></a>Χρήση της υπηρεσίας διαχείρισης StorSimple για προβολή και διαχείριση ειδοποιήσεων StorSimple

## <a name="overview"></a>Επισκόπηση

Στην καρτέλα **ειδοποιήσεις** στην υπηρεσία διαχείρισης StorSimple παρέχει έναν τρόπο για να εξετάσετε και καταργήστε την επιλογή StorSimple ειδοποιήσεις που σχετίζονται με τη συσκευή με βάση τη σε πραγματικό χρόνο. Από αυτήν την καρτέλα, μπορείτε να παρακολουθείτε κεντρικά τα προβλήματα εύρυθμη λειτουργία των συσκευών StorSimple και τη συνολική λύση Microsoft Azure StorSimple.

Αυτό το πρόγραμμα εκμάθησης περιγράφει συνήθεις ειδοποίησης συνθήκες, επίπεδα σοβαρότητας προειδοποιήσεων και πώς μπορείτε να ρυθμίσετε τις παραμέτρους οι ειδοποιήσεις. Επιπλέον, περιλαμβάνει πίνακες ειδοποίησης γρήγορης αναφοράς, οι οποίοι δίνουν τη δυνατότητα να εντοπίσετε μια συγκεκριμένη ειδοποίηση γρήγορα και να απαντούν σωστά.

![Σελίδα ειδοποιήσεων](./media/storsimple-manage-alerts/HCS_AlertsPage.png)

## <a name="common-alert-conditions"></a>Κοινών συνθηκών ειδοποίησης

Συσκευή StorSimple δημιουργεί ειδοποιήσεις απόκριση σε μια ποικιλία συνθήκες. Τα παρακάτω είναι οι πιο συνηθισμένες τύποι ειδοποίησης συνθήκες:

- **Ζητήματα υλικού** – αυτές τις ειδοποιήσεις σας ενημερώσει για την εύρυθμη λειτουργία του υλικού σας. Που σας επιτρέπουν να γνωρίζετε αν αναβαθμίσεις υλικολογισμικού είναι απαραίτητο, εάν μια διασύνδεση δικτύου έχει προβλήματα ή αν υπάρχει πρόβλημα με μία από τις μονάδες δίσκου δεδομένων.

- **Αντιμετωπίζετε προβλήματα συνδεσιμότητας** -αυτές οι προειδοποιήσεις προκύπτουν όταν υπάρχει δυσκολία τη μεταφορά δεδομένων. Θέματα επικοινωνίας μπορεί να συμβεί κατά τη μεταφορά δεδομένων σε και από το λογαριασμό χώρου αποθήκευσης Azure ή λόγω έλλειψης σύνδεσης μεταξύ των συσκευών και της υπηρεσίας διαχείρισης StorSimple. Ζητήματα επικοινωνίας είναι ορισμένα από τα πιο δύσκολες για να διορθώσετε επειδή υπάρχουν τόσοι πολλοί σημείων αποτυχίας. Μπορείτε πάντα πρώτα πρέπει να επαληθεύσετε ότι η συνδεσιμότητα του δικτύου και πρόσβαση στο Internet είναι διαθέσιμα πριν να συνεχίσετε με την αντιμετώπιση προβλημάτων για πιο προχωρημένους. Για βοήθεια σχετικά με την αντιμετώπιση προβλημάτων, μεταβείτε στην [Αντιμετώπιση προβλημάτων με το cmdlet δοκιμή σύνδεσης](storsimple-troubleshoot-deployment.md).

- **Θέματα απόδοσης** – προκαλούνται αυτές τις ειδοποιήσεις όταν το σύστημά σας δεν εκτελεί ιδανικά, όπως όταν είναι ένα φόρτο εργασίας.

Επιπλέον, μπορεί να βλέπετε ειδοποιήσεις που σχετίζονται με την ασφάλεια, ενημερώσεις ή αποτυχίες των εργασιών.

## <a name="alert-severity-levels"></a>Επίπεδα σοβαρότητας προειδοποιήσεων

Ειδοποιήσεις έχουν επίπεδα σοβαρότητας διαφορετικά, ανάλογα με την επίδραση που θα έχει την ειδοποίηση κατάσταση και την ανάγκη για απόκριση την ειδοποίηση. Τα επίπεδα σοβαρότητας είναι:

- **Κρίσιμη** – αυτή η ειδοποίηση είναι απόκριση σε μια συνθήκη που επηρεάζει την επιτυχή απόδοση του συστήματος. Για να βεβαιωθείτε ότι η υπηρεσία δεν διακόπτεται StorSimple απαιτείται ενέργεια.

- **Προειδοποίηση** – αυτή η συνθήκη μπορεί να μετατραπεί σε κρίσιμες Εάν δεν επιλυθεί. Πρέπει να εξετάσετε την κατάσταση και να κάνετε κάποια ενέργεια που απαιτούνται για να καταργήσετε το ζήτημα.

- **Πληροφορίες** – αυτή η ειδοποίηση περιέχει πληροφορίες που μπορεί να είναι χρήσιμη για την παρακολούθηση και τη διαχείριση του συστήματος.

## <a name="configure-alert-settings"></a>Ρύθμιση παραμέτρων ειδοποίησης

Μπορείτε να επιλέξετε εάν θέλετε να λαμβάνετε ειδοποιήσεις μέσω ηλεκτρονικού ταχυδρομείου ειδοποίησης συνθηκών για κάθε μία από τις συσκευές σας StorSimple. Επιπλέον, μπορείτε να αναγνωρίσετε τους άλλους παραλήπτες ειδοποιήσεις, εισάγοντας τις διευθύνσεις ηλεκτρονικού ταχυδρομείου στο πλαίσιο **τους άλλους παραλήπτες ηλεκτρονικού ταχυδρομείου** , διαχωρισμένα με ελληνικά ερωτηματικά.

>[AZURE.NOTE] Μπορείτε να εισαγάγετε έως 20 διευθύνσεις ηλεκτρονικού ταχυδρομείου ανά συσκευή.

Αφού ενεργοποιήσετε την ειδοποίηση μέσω ηλεκτρονικού ταχυδρομείου για μια συσκευή, τα μέλη της λίστας ειδοποίηση θα λάβετε ένα μήνυμα ηλεκτρονικού ταχυδρομείου κάθε φορά που προκύπτει ένα κρίσιμη ειδοποίηση. Τα μηνύματα θα αποστέλλονται από *storsimple-alerts-noreply@mail.windowsazure.com* και θα περιγράφουν τη συνθήκη ειδοποίησης. Οι παραλήπτες μπορούν να κάντε κλικ στην επιλογή **Κατάργηση** για να καταργήσετε τον εαυτό τους από τη λίστα ειδοποιήσεων ηλεκτρονικού ταχυδρομείου.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>Για να ενεργοποιήσετε την ειδοποίηση μέσω ηλεκτρονικού ταχυδρομείου των ειδοποιήσεων για μια συσκευή

1. Μεταβείτε στις **συσκευές** > **Ρύθμιση παραμέτρων** για τη συσκευή.

2. Στην περιοχή **Ρυθμίσεις ειδοποίησης**, ορίστε τα εξής:

    1. Στο πεδίο **Αποστολή ειδοποίησης ηλεκτρονικού ταχυδρομείου** , επιλέξτε **Ναι**.

    2. Στο πεδίο **διαχειριστές υπηρεσίας ηλεκτρονικού ταχυδρομείου** , επιλέξτε **Ναι** εάν θέλετε να έχετε το διαχειριστή της υπηρεσίας και όλων των διαχειριστών από κοινού λαμβάνουν τις ειδοποιήσεις ειδοποίησης.

    3. Στο πεδίο **τους άλλους παραλήπτες ηλεκτρονικού ταχυδρομείου** , πληκτρολογήστε τις διευθύνσεις ηλεκτρονικού ταχυδρομείου από όλους τους άλλους παραλήπτες που πρέπει να λαμβάνουν τις ειδοποιήσεις ειδοποίησης. Πληκτρολογήστε τα ονόματα στη μορφή *someone@somewhere.com*. Χρησιμοποιήστε ελληνικά ερωτηματικά για να διαχωρίσετε τις διευθύνσεις ηλεκτρονικού ταχυδρομείου. Μπορείτε να ρυθμίσετε έως 20 διευθύνσεις ηλεκτρονικού ταχυδρομείου ανά συσκευή. 

        ![Ρύθμιση παραμέτρων ειδοποιήσεων ειδοποιήσεων](./media/storsimple-manage-alerts/AlertNotify.png)

3. Για να στείλετε μια ειδοποίηση μέσω ηλεκτρονικού ταχυδρομείου δοκιμής, κάντε κλικ στο εικονίδιο βέλους δίπλα στην επιλογή **Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου δοκιμής**. Η υπηρεσία διαχείρισης StorSimple θα εμφανίσει μηνύματα κατάστασης όπως προωθεί την ειδοποίηση δοκιμής. 

4. Όταν εμφανιστεί το ακόλουθο μήνυμα, κάντε κλικ στο **κουμπί OK**. 

    ![Ειδοποιήσεις δοκιμή ειδοποίησης ηλεκτρονικού ταχυδρομείου που στέλνεται](./media/storsimple-manage-alerts/HCS_AlertNotificationConfig3.png)

    >[AZURE.NOTE] Εάν το μήνυμα ειδοποίησης δοκιμή δεν μπορεί να αποσταλεί, η υπηρεσία διαχείρισης StorSimple θα εμφανίσει ένα κατάλληλο μήνυμα. Κάντε κλικ στο κουμπί **OK**, περιμένετε λίγα λεπτά και, στη συνέχεια, προσπαθήστε ξανά να στείλετε το μήνυμα ειδοποίησης δοκιμής. 

## <a name="view-and-track-alerts"></a>Προβάλετε και να παρακολουθήσετε τις ειδοποιήσεις

Ο πίνακας εργαλείων service StorSimple Manager σάς παρέχει μια γρήγορη ματιά στο τον αριθμό των ειδοποιήσεων στις συσκευές σας, Τακτοποίηση κατά επίπεδο σοβαρότητας.

![Ειδοποιήσεις πίνακα εργαλείων](./media/storsimple-manage-alerts/admin_alerts_dashboard.png)

Κάνοντας κλικ στο επίπεδο της σοβαρότητας ανοίγει στην καρτέλα **ειδοποιήσεις** . Τα αποτελέσματα περιλαμβάνει μόνο τις ειδοποιήσεις που ταιριάζουν με αυτό το επίπεδο σοβαρότητας.

![Ειδοποιήσεις αναφορά έχει ρυθμιστεί για τον τύπο ειδοποίησης](./media/storsimple-manage-alerts/admin_alerts_scoped.png)

Κάνοντας κλικ στην επιλογή μια ειδοποίηση στη λίστα σάς παρέχει περισσότερες λεπτομέρειες για την ειδοποίηση, την τελευταία φορά που αναφέρθηκε την ειδοποίηση, συμπεριλαμβανομένου του αριθμού των εμφανίσεων της ειδοποίησης για τη συσκευή και την προτεινόμενη ενέργεια για να επιλύσετε την ειδοποίηση. Εάν πρόκειται για μια ειδοποίηση υλικό, θα αναγνωρίσει επίσης το στοιχείο υλικού.

![Παράδειγμα ειδοποίησης υλικού](./media/storsimple-manage-alerts/admin_alerts_hardware.png)

Εάν θέλετε να στείλετε τις πληροφορίες στην υποστήριξη της Microsoft, μπορείτε να αντιγράψετε τις λεπτομέρειες της ειδοποίησης σε ένα αρχείο κειμένου. Αφού ακολουθήσατε τις προτάσεις και επιλυθεί το ειδοποίησης συνθήκη εσωτερικής εγκατάστασης, θα πρέπει να καταργήσετε την ειδοποίηση από τη συσκευή, επιλέγοντας την ειδοποίηση στην καρτέλα **ειδοποιήσεις** και κάνοντας κλικ στην επιλογή **Απαλοιφή**. Για να καταργήσετε πολλές ειδοποιήσεις, επιλέξτε κάθε ειδοποίηση, κάντε κλικ σε οποιαδήποτε στήλη εκτός από τη στήλη **ειδοποίηση** και, στη συνέχεια, κάντε κλικ στο κουμπί **Απαλοιφή** μόλις επιλέξετε όλες τις ειδοποιήσεις για να διαγραφούν. Σημειώστε ότι ορισμένες ειδοποιήσεις καταργούνται αυτόματα όταν το ζήτημα έχει επιλυθεί ή όταν το σύστημα ενημερώνει την ειδοποίηση με νέες πληροφορίες.

Όταν κάνετε κλικ στο κουμπί **Απαλοιφή**, θα έχετε την ευκαιρία να παράσχετε σχόλια σχετικά με την ειδοποίηση και τα βήματα που εκτελέσατε για να επιλύσετε το ζήτημα. Ορισμένα συμβάντα θα καταργηθεί από το σύστημα εάν ένα άλλο συμβάν ενεργοποιείται με νέες πληροφορίες. Σε αυτή την περίπτωση, θα δείτε το ακόλουθο μήνυμα.

![Καταργήστε την επιλογή ειδοποίηση μηνύματος](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Ταξινόμηση και αναθεώρηση ειδοποιήσεις

Ίσως σας φανεί πιο αποδοτικό να εκτελέσετε αναφορές στις ειδοποιήσεις, ώστε να μπορείτε να ελέγξετε και να καταργήσετε την επιλογή τους σε ομάδες. Επιπλέον, στην καρτέλα **ειδοποιήσεις** μπορεί να εμφανίσει έως 250 ειδοποιήσεις. Εάν έχετε υπερβεί αυτόν τον αριθμό των ειδοποιήσεων, όχι όλες τις ειδοποιήσεις θα εμφανιστεί στην προεπιλεγμένη προβολή. Μπορείτε να συνδυάσετε τα παρακάτω πεδία για να προσαρμόσετε τις ειδοποιήσεις που εμφανίζονται:

- **Κατάσταση** – μπορείτε να εμφανίσετε **ενεργή** ή **Απαλοιφή** ειδοποιήσεις. Ενεργό ειδοποιήσεις εξακολουθεί να που ενεργοποιούνται στο σύστημά σας, ενώ απενεργοποιημένο ειδοποιήσεις έχουν είτε καταργημένη με μη αυτόματο τρόπο από ένα διαχειριστή ή μέσω προγραμματισμού απαλειφθούν, επειδή το σύστημα ενημερώσει τη συνθήκη ειδοποίησης με νέες πληροφορίες.

- **Σοβαρότητας** – μπορείτε να εμφανίσετε τις ειδοποιήσεις όλα τα επίπεδα σοβαρότητας (κρίσιμη, προειδοποίηση, πληροφορίες) ή μόνο ένα συγκεκριμένο σοβαρότητας, όπως μόνο κρίσιμες ειδοποιήσεις.

- **Προέλευση** – μπορείτε να εμφανίσετε ειδοποιήσεων από όλες τις προελεύσεις ή να περιορίσετε τις ειδοποιήσεις με εκείνα που παρέχονται από την υπηρεσία ή μία ή όλες τις συσκευές.

- **Χρονικό διάστημα** –, καθορίζοντας τις ημερομηνίες **από** και **έως** και χρονικές σημάνσεις, μπορείτε να ανατρέξετε σε ειδοποιήσεις κατά τη χρονική περίοδο που σας ενδιαφέρει.

## <a name="alerts-quick-reference"></a>Γρήγορη αναφορά ειδοποιήσεων

Στους παρακάτω πίνακες παρατίθενται ορισμένες από τις ειδοποιήσεις του Microsoft Azure StorSimple που ενδέχεται να συναντήσετε, καθώς και πρόσθετες πληροφορίες και συστάσεις όπου είναι διαθέσιμο. Ειδοποιήσεις συσκευή StorSimple εμπίπτουν σε μία από τις εξής κατηγορίες:

- [Ειδοποιήσεις συνδεσιμότητας cloud](#cloud-connectivity-alerts)

- [Ειδοποιήσεις συμπλέγματος](#cluster-alerts)

- [Ειδοποιήσεις αποκατάστασης από καταστροφή](#disaster-recovery-alerts)

- [Ειδοποιήσεις υλικού](#hardware-alerts)

- [Προειδοποιήσεις αποτυχίας εργασίας](#job-failure-alerts)

- [Ειδοποιήσεις τοπικά καρφιτσωμένη έντασης ήχου](#locally-pinned-volume-alerts)

- [Ειδοποιήσεις δικτύωση](#networking-alerts)

- [Ειδοποιήσεις επιδόσεων](#performance-alerts)

- [Προειδοποιήσεις ασφαλείας](#security-alerts)

- [Υποστήριξη ειδοποιήσεων πακέτου](#support-package-alerts)

- [Ειδοποιήσεις ενημέρωσης](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Ειδοποιήσεις συνδεσιμότητας cloud

|Ειδοποίηση κειμένου|Συμβάν|Περισσότερες πληροφορίες / προτεινόμενες ενέργειες|
|:---|:---|:---|
|Δεν είναι δυνατή η συνδεσιμότητα <*όνομα διαπιστευτηρίων cloud*>.|Δεν είναι δυνατή η σύνδεση με το λογαριασμό χώρου αποθήκευσης.|Φαίνεται ότι μπορεί να υπάρχει κάποιο ζήτημα συνδεσιμότητας με τη συσκευή σας. Εκτελέστε το `Test-HcsmConnection` cmdlet από το περιβάλλον εργασίας των Windows PowerShell για StorSimple στη συσκευή σας για να εντοπίσετε και να διορθώσετε το πρόβλημα. Εάν οι ρυθμίσεις είναι σωστές, το πρόβλημα μπορεί να με τα διαπιστευτήρια του λογαριασμού χώρου αποθήκευσης για το οποίο έχει υψωμένο την ειδοποίηση. Σε αυτήν την περίπτωση, χρησιμοποιήστε το `Test-HcsStorageAccountCredential` cmdlet για να προσδιορίσετε εάν υπάρχουν ζητήματα που μπορείτε να επιλύσετε.<ul><li>Ελέγξτε τις ρυθμίσεις δικτύου.</li><li>Ελέγξτε τα διαπιστευτήριά σας λογαριασμό χώρου αποθήκευσης.</li></ul>|
|Δεν έχουμε μια συγχρονισμό από τη συσκευή σας για τα τελευταία <*αριθμός*> λεπτά.|Δεν είναι δυνατή η σύνδεση με τη συσκευή.|Φαίνεται ότι υπάρχει κάποιο ζήτημα συνδεσιμότητας με τη συσκευή σας. Χρησιμοποιήστε το `Test-HcsmConnection` cmdlet από το περιβάλλον εργασίας των Windows PowerShell για StorSimple στη συσκευή σας για να προσδιορίσετε και να διορθώσετε το πρόβλημα ή επικοινωνήστε με το διαχειριστή του δικτύου σας.|

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>Συμπεριφορά StorSimple όταν αποτύχει η συνδεσιμότητα cloud

Τι συμβαίνει εάν αποτύχει η συνδεσιμότητα cloud για συσκευή μου StorSimple εκτελείται στο παραγωγής;

Εάν αποτύχει η συνδεσιμότητα cloud στη συσκευή σας παραγωγής StorSimple, στη συνέχεια, ανάλογα με την κατάσταση της συσκευής σας, τα ακόλουθα μπορεί να προκύψει: 

- **Για τα τοπικά δεδομένα στη συσκευή σας**: για κάποιο χρονικό διάστημα, δεν θα υπάρξει καμία διακοπή και θα συνεχίσουμε διαβάζει πράξης. Ωστόσο, όπως τον αριθμό των εκκρεμών IOs αυξάνεται και υπερβαίνει το όριο, το διαβάζει θα μπορούσε να αρχίσουν να αποτυγχάνουν. 

    Ανάλογα με την ποσότητα των δεδομένων στη συσκευή σας, οι εγγραφές θα εξακολουθήσει επίσης να προκύψουν για την πρώτη λίγες ώρες μετά τη διακοπή της στο τη συνδεσιμότητα cloud. Οι εγγραφές θα, στη συνέχεια, να επιβραδύνετε και τελικά αρχίστε να αποτύχει εάν διακοπεί η σύνδεση cloud για μερικές ώρες. (Υπάρχει προσωρινό χώρο αποθήκευσης στη συσκευή για τα δεδομένα που θα είναι δυνατή η προώθηση στο cloud. Αυτή η περιοχή είναι εκκαθάριση κατά την αποστολή των δεδομένων. Εάν αποτύχει η σύνδεση, θα δεν είναι δυνατή η προώθηση δεδομένων σε αυτήν την περιοχή αποθήκευσης στο cloud, και εισόδου/ΕΞΌΔΟΥ θα αποτύχει.)   

 
- **Για τα δεδομένα στο cloud**: για τα περισσότερα σφάλματα συνδεσιμότητας cloud, επιστρέφεται σφάλμα. Μόλις επαναφέρεται τη συνδεσιμότητα, χωρίς να χρειάζεται να μεταφέρετε την ένταση online ο χρήστης γίνει επαναφορά των του IOs. Σπάνια, ενδεχομένως να απαιτούνται για να εμφανίσετε ξανά την ένταση ήχου online από την πύλη του Azure κλασική παρέμβαση του χρήστη. 
 
- **Για το cloud στιγμιότυπα σε εξέλιξη**: Η λειτουργία έχει επαναληφθεί μερικές φορές μέσα σε 4-5 ώρες και εάν δεν είναι δυνατή η επαναφορά τη συνδεσιμότητα, τα στιγμιότυπα cloud θα αποτύχει.


### <a name="cluster-alerts"></a>Ειδοποιήσεις συμπλέγματος

|Ειδοποίηση κειμένου|Συμβάν|Περισσότερες πληροφορίες / προτεινόμενες ενέργειες|
|:---|:---|:---|
|Η συσκευή δεν μέσω <*όνομα συσκευής*>.|Συσκευή είναι σε λειτουργία συντήρησης.|Η συσκευή δεν μέσω λόγω πληκτρολογώντας ή έξοδος από την κατάσταση λειτουργίας συντήρησης. Αυτό είναι κανονική και δεν χρειάζεται καμία ενέργεια. Αφού έχετε αναγνωρίζεται αυτή η ειδοποίηση, καταργήστε την από τη σελίδα ειδοποιήσεων.|
|Η συσκευή δεν μέσω <*όνομα συσκευής*>.|Μόλις ενημερώθηκε υλικολογισμικού συσκευή ή το λογισμικό.|Παρουσιάστηκε ένα ανακατεύθυνση συμπλέγματος λόγω μια ενημερωμένη έκδοση. Αυτό είναι κανονική και δεν χρειάζεται καμία ενέργεια. Αφού έχετε αναγνωρίζεται αυτή η ειδοποίηση, καταργήστε την από τη σελίδα ειδοποιήσεις.|
|Η συσκευή δεν μέσω <*όνομα συσκευής*>.|Ελεγκτή έχει τερματιστεί ή επανεκκίνηση του.|Συσκευή απέτυχε μέσω, επειδή το ενεργό ελεγκτή έχει τερματιστεί ή επανεκκίνηση του από ένα διαχειριστή. Δεν χρειάζεται καμία ενέργεια. Αφού έχετε αναγνωρίζεται αυτή η ειδοποίηση, καταργήστε την από τη σελίδα ειδοποιήσεις.|
|Η συσκευή δεν μέσω <*όνομα συσκευής*>.|Προγραμματισμένη ανακατεύθυνσης.|Βεβαιωθείτε ότι αυτό είναι προγραμματισμένη ανακατεύθυνσης. Αφού έχετε αναλάβει κατάλληλη ενέργεια, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.|
|Η συσκευή δεν μέσω <*όνομα συσκευής*>.|Μη προγραμματισμένη ανακατεύθυνσης.|StorSimple είναι σχεδιασμένες για την αυτόματη ανάκτηση από μη προγραμματισμένη ανακατευθύνσεις. Εάν βλέπετε μεγάλου αριθμού αυτές τις ειδοποιήσεις, επικοινωνήστε με την υποστήριξη της Microsoft.|
|Η συσκευή δεν μέσω <*όνομα συσκευής*>.|Άλλα/Άγνωστη αιτία.|Εάν βλέπετε μεγάλου αριθμού αυτές τις ειδοποιήσεις, επικοινωνήστε με την υποστήριξη της Microsoft. Αφού το ζήτημα έχει επιλυθεί, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.|
|Μια υπηρεσία συσκευών που είναι σημαντικά αναφορών κατάστασης, καθώς απέτυχε.|Αποτυχία υπηρεσίας DataPath. |Επικοινωνήστε με την υπηρεσία υποστήριξης της Microsoft για βοήθεια.|
|Εικονική διεύθυνση IP για το περιβάλλον εργασίας δικτύου <*ΔΕΔΟΜΈΝΩΝ #*> αναφορές κατάστασης, καθώς απέτυχε. |Άλλα/Άγνωστη αιτία. |Μερικές φορές προσωρινό συνθήκες μπορεί να προκαλέσει αυτές τις ειδοποιήσεις. Εάν πρόκειται για την περίπτωση, στη συνέχεια, αυτή η ειδοποίηση θα διαγραφούν αυτόματα μετά από κάποιο χρονικό διάστημα. Εάν το πρόβλημα δεν επιλυθεί, επικοινωνήστε με την υποστήριξη της Microsoft.|
|Εικονική διεύθυνση IP για το περιβάλλον εργασίας δικτύου <*ΔΕΔΟΜΈΝΩΝ #*> αναφορές κατάστασης, καθώς απέτυχε.|Όνομα περιβάλλοντος εργασίας: <*ΔΕΔΟΜΈΝΩΝ #*> διεύθυνση IP <IP address> δεν είναι δυνατή η σύνδεση επειδή εντοπίστηκε μια διπλότυπη διεύθυνση IP του δικτύου. |Βεβαιωθείτε ότι η διπλότυπη διεύθυνση IP έχει καταργηθεί από το δίκτυο ή να ρυθμίσει ξανά τις παραμέτρους του περιβάλλοντος εργασίας με μια διαφορετική διεύθυνση IP.|


### <a name="disaster-recovery-alerts"></a>Ειδοποιήσεις αποκατάστασης από καταστροφή

|Ειδοποίηση κειμένου|Συμβάν|Περισσότερες πληροφορίες / προτεινόμενες ενέργειες|
|:---|:---|:---|
|Αξιοποίηση δεν μπόρεσε να επαναφέρει όλες τις ρυθμίσεις για αυτήν την υπηρεσία. Δεδομένα ρύθμισης παραμέτρων συσκευή βρίσκεται σε κατάσταση χωρίς συνέπεια για ορισμένες συσκευές.|Ασυνέπεια δεδομένων εντοπίζεται μετά την αποκατάσταση.|Κρυπτογραφημένων δεδομένων στην υπηρεσία δεν είναι συγχρονισμένο με αυτό στη συσκευή. Εγκρίνετε τη συσκευή <*όνομα συσκευής*> από τη Διαχείριση StorSimple για να ξεκινήσετε τη διαδικασία συγχρονισμού. Χρησιμοποιήστε το περιβάλλον εργασίας των Windows PowerShell για StorSimple, για να εκτελέσετε το `Restore-HcsmEncryptedServiceData` στη συσκευή <*όνομα συσκευής*> cmdlet, παρέχοντας τον παλιό κωδικό πρόσβασης ως είσοδο σε αυτό το cmdlet για να επαναφέρετε το προφίλ ασφαλείας. Στη συνέχεια, εκτελέστε το `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet για την ενημέρωση του κλειδιού κρυπτογράφησης δεδομένων υπηρεσίας. Αφού έχετε αναλάβει κατάλληλη ενέργεια, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.|


### <a name="hardware-alerts"></a>Ειδοποιήσεις υλικού

|Ειδοποίηση κειμένου|Συμβάν|Περισσότερες πληροφορίες / προτεινόμενες ενέργειες|
|:---|:---|:---|
|Στοιχείο υλικού <*Αναγνωριστικό στοιχείου*> αναφορές κατάστασης ως <*κατάστασης*>.||Μερικές φορές προσωρινό συνθήκες μπορεί να προκαλέσει αυτές τις ειδοποιήσεις. Εάν Ναι, αυτή η ειδοποίηση θα διαγραφούν αυτόματα μετά από κάποιο χρονικό διάστημα. Εάν το πρόβλημα δεν επιλυθεί, επικοινωνήστε με την υποστήριξη της Microsoft.|
|Παθητικές ελεγκτή λειτουργεί σωστά.|Το παθητικό ελεγκτή (δευτερεύουσα) δεν λειτουργεί.|Η συσκευή σας είναι λειτουργικές, αλλά μία από τις ελεγκτές λειτουργεί σωστά. Δοκιμάστε να κάνετε επανεκκίνηση αυτού του ελεγκτή. Εάν το πρόβλημα δεν επιλυθεί, επικοινωνήστε με την υποστήριξη της Microsoft.|

### <a name="job-failure-alerts"></a>Προειδοποιήσεις αποτυχίας εργασίας

|Ειδοποίηση κειμένου|Συμβάν|Περισσότερες πληροφορίες / προτεινόμενες ενέργειες|
|:---|:---|:---|
|Δημιουργία αντιγράφου ασφαλείας του <*Αναγνωριστικό ομάδας ένταση προέλευσης*> απέτυχε.|Απέτυχε η εργασία δημιουργίας αντιγράφων ασφαλείας.|Αντιμετωπίζετε προβλήματα συνδεσιμότητας ενδέχεται να εμποδίζει την επιτυχή ολοκλήρωση της λειτουργίας δημιουργίας αντιγράφων ασφαλείας. Εάν υπάρχουν ζητήματα συνδεσιμότητας, ενδέχεται να έχετε φτάσει τον μέγιστο αριθμό των αντιγράφων ασφαλείας. Διαγράψτε τα αντίγραφα ασφαλείας που δεν χρειάζονται πλέον και επαναλάβετε τη λειτουργία. Αφού έχετε αναλάβει κατάλληλη ενέργεια, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.|
|Απέτυχε η κλωνοποίηση <*αντιγράφου ασφαλείας στοιχείο προέλευσης αναγνωριστικά*> σε <*προορισμού ένταση σειριακούς αριθμούς*>.|Απέτυχε η κλωνοποίηση εργασίας.|Ανανεώστε τη λίστα αντιγράφων ασφαλείας για να επιβεβαιώσετε ότι το αντίγραφο ασφαλείας εξακολουθεί να είναι έγκυρο. Εάν το αντίγραφο ασφαλείας είναι έγκυρες, είναι πιθανό ότι αντιμετωπίζετε προβλήματα συνδεσιμότητας cloud αποτρέπουν την επιτυχή ολοκλήρωση της λειτουργίας αντιγραφής. Εάν υπάρχουν ζητήματα συνδεσιμότητας, ενδέχεται να έχετε φτάσει το όριο χώρου αποθήκευσης. Διαγράψτε τα αντίγραφα ασφαλείας που δεν χρειάζονται πλέον και επαναλάβετε τη λειτουργία. Αφού έχετε αναλάβει κατάλληλη ενέργεια για να επιλύσετε το ζήτημα, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.|
|Επαναφορά <*αντιγράφου ασφαλείας στοιχείο προέλευσης αναγνωριστικά*> απέτυχε.|Απέτυχε η εργασία επαναφοράς.|Ανανεώστε τη λίστα αντιγράφων ασφαλείας για να επιβεβαιώσετε ότι το αντίγραφο ασφαλείας εξακολουθεί να είναι έγκυρο. Εάν το αντίγραφο ασφαλείας είναι έγκυρες, είναι πιθανό ότι αντιμετωπίζετε προβλήματα συνδεσιμότητας cloud αποτρέπουν την επιτυχή ολοκλήρωση της λειτουργίας επαναφοράς. Εάν υπάρχουν ζητήματα συνδεσιμότητας, ενδέχεται να έχετε φτάσει το όριο χώρου αποθήκευσης. Διαγράψτε τα αντίγραφα ασφαλείας που δεν χρειάζονται πλέον και επαναλάβετε τη λειτουργία. Αφού έχετε αναλάβει κατάλληλη ενέργεια για να επιλύσετε το ζήτημα, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.|

### <a name="locally-pinned-volume-alerts"></a>Ειδοποιήσεις τοπικά καρφιτσωμένη έντασης ήχου

|Ειδοποίηση κειμένου|Συμβάν|Περισσότερες πληροφορίες / προτεινόμενες ενέργειες|
|:---|:---|:---|
|Δημιουργία τοπικού τόμου <*όνομα τόμου*> απέτυχε.| Απέτυχε η εργασία δημιουργίας έντασης ήχου. <*Μήνυμα σφάλματος που αντιστοιχεί στον κωδικό σφάλματος αποτυχίας*>.|Αντιμετωπίζετε προβλήματα συνδεσιμότητας ενδέχεται να εμποδίζει την επιτυχή ολοκλήρωση της λειτουργίας δημιουργίας χώρου. Τοπικά καρφιτσωμένη όγκους thickly παροχή της υπηρεσίας και η διαδικασία δημιουργίας χώρου περιλαμβάνει την εξάπλωση ομότιμου όγκους στο cloud. Εάν υπάρχουν ζητήματα συνδεσιμότητας, ενδέχεται να έχετε αποθεμάτων τον τοπικό χώρο στη συσκευή. Προσδιορίστε αν υπάρχει χώρο στη συσκευή πριν να επιχειρήσετε ξανά αυτή η λειτουργία.|
|Απέτυχε η επέκταση του τοπικού τόμου <*όνομα τόμου*>.|Η εργασία τροποποίηση ένταση απέτυχε λόγω <*μήνυμα σφάλματος που αντιστοιχεί στον κωδικό σφάλματος αποτυχίας*>.|Αντιμετωπίζετε προβλήματα συνδεσιμότητας ενδέχεται να εμποδίζει την επιτυχή ολοκλήρωση της λειτουργίας επέκτασης έντασης ήχου. Τοπικά καρφιτσωμένη όγκους thickly παροχή της υπηρεσίας και η διαδικασία επέκτασης υπάρχοντα χώρο περιλαμβάνει εξάπλωση ομότιμου όγκους στο cloud. Εάν υπάρχουν ζητήματα συνδεσιμότητας, ενδέχεται να έχετε αποθεμάτων τον τοπικό χώρο στη συσκευή. Προσδιορίστε αν υπάρχει χώρο στη συσκευή πριν να επιχειρήσετε ξανά αυτή η λειτουργία.|
|Απέτυχε η μετατροπή τόμου <*όνομα τόμου*>.|Η εργασία μετατροπής έντασης ήχου για να μετατρέψετε τον τύπο του τόμου από τοπικά καρφιτσωμένα στη επιπέδων αποτυχίας.|Μετατροπή του όγκου από τον τύπο τοπικά καρφιτσωμένα στη επιπέδων δεν ήταν δυνατό να ολοκληρωθεί. Βεβαιωθείτε ότι δεν υπάρχουν ζητήματα συνδεσιμότητας εμποδίζουν την επιτυχή ολοκλήρωση της λειτουργίας. Για την αντιμετώπιση προβλημάτων συνδεσιμότητας μεταβείτε στην [Αντιμετώπιση προβλημάτων με το cmdlet HcsmConnection δοκιμής](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Η αρχική τοπικά καρφιτσωμένη ένταση τώρα έχει επισημανθεί ως ενός ομότιμου όγκου δεδομένου ότι ορισμένα από τα δεδομένα από τον όγκο τοπικά καρφιτσωμένη έχει έχουν στο cloud κατά τη μετατροπή. Η ένταση προκύπτει ομότιμου εξακολουθεί να είναι καταλαμβάνουν τοπικό χώρο στη συσκευή που δεν μπορεί να ανακτηθεί για μελλοντική τοπικό όγκους.<br>Επιλύσετε τυχόν προβλήματα συνδεσιμότητας, καταργήστε την επιλογή ειδοποίηση και μετατροπή αυτού του τόμου ο αρχικός τύπος τοπικά καρφιτσωμένη έντασης ήχου για να βεβαιωθείτε ότι όλα τα δεδομένα είναι που καθίστανται διαθέσιμες τοπικά ξανά.|
|Απέτυχε η μετατροπή τόμου <*όνομα τόμου*>.|Η εργασία μετατροπής έντασης ήχου για να μετατρέψετε τον τύπο του τόμου από επιπέδων σε τοπικά καρφιτσωμένα απέτυχε.|Μετατροπή του όγκου από τον τύπο επιπέδων σε τοπικά καρφιτσωμένα δεν ήταν δυνατό να ολοκληρωθεί. Βεβαιωθείτε ότι δεν υπάρχουν ζητήματα συνδεσιμότητας εμποδίζουν την επιτυχή ολοκλήρωση της λειτουργίας. Για την αντιμετώπιση προβλημάτων συνδεσιμότητας μεταβείτε στην [Αντιμετώπιση προβλημάτων με το cmdlet HcsmConnection δοκιμής](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Η αρχική ομότιμου ένταση τώρα έχει επισημανθεί ως τοπικά καρφιτσωμένη τόμου ως μέρος της διαδικασίας μετατροπής εξακολουθεί να έχουν δεδομένα που βρίσκονται στο cloud, ενώ το thickly προμήθεια του φακέλου χώρος στη συσκευή για αυτού του τόμου μπορεί να ανακτηθεί δεν είναι πλέον για μελλοντική τοπικό όγκους.<br>Επιλύσετε τυχόν προβλήματα συνδεσιμότητας, καταργήστε την επιλογή ειδοποίηση και μετατροπή αυτού του τόμου ο αρχικός τύπος ομότιμου έντασης ήχου για να βεβαιωθείτε ότι τοπικό χώρος thickly παρασχεθεί στη συσκευή μπορεί να ανακτηθεί.|
|Nearing κατανάλωση τοπικού χώρου για τοπική στιγμιότυπων των <*όνομα ομάδας έντασης ήχου*>|Τοπική στιγμιότυπα για την πολιτική ασφαλείας σύντομα ενδέχεται να εξαντλείται ο χώρος και να ακυρωθεί για να αποφύγετε αποτυχιών εγγραφής κεντρικού υπολογιστή.|Συχνές τοπικό στιγμιότυπα μαζί με ένα churn υψηλής δεδομένων του όγκου που σχετίζεται με αυτήν την ομάδα ασφαλείας πολιτικής προκαλούν τοπικό χώρο στη συσκευή να καταναλωθεί γρήγορα. Διαγραφή τοπικού στιγμιότυπα που δεν χρειάζονται πλέον. Επίσης, να ενημερώσετε το τοπικό στιγμιότυπο χρονοδιαγράμματα για αυτή την πολιτική ασφαλείας για να τραβήξετε λιγότερο συχνά τοπικό στιγμιότυπα, και βεβαιωθείτε ότι cloud στιγμιότυπα λαμβάνονται τακτικά. Εάν αυτές οι ενέργειες δεν έχουν ληφθεί, τοπικό χώρο για αυτά τα στιγμιότυπα μπορεί να είναι σύντομα των αποθεμάτων και το σύστημα θα διαγράψει αυτόματα τους για να βεβαιωθείτε ότι οι εγγραφές κεντρικού υπολογιστή εξακολουθεί να γίνεται με επιτυχία.|
|Τοπική στιγμιότυπα για <*όνομα ομάδας ένταση*> δεν είναι πλέον έγκυροι.|Τα στιγμιότυπα τοπικό για <*όνομα ομάδας ένταση*> έχουν ακυρώνονται και, στη συνέχεια, διαγραφή, επειδή ότι έχετε υπερβεί τον τοπικό χώρο στη συσκευή.|Για να βεβαιωθείτε ότι αυτό δεν επαναλαμβάνεται στο μέλλον, εξετάστε τα χρονοδιαγράμματα τοπικό στιγμιότυπο για αυτή την πολιτική ασφαλείας και διαγραφή τοπικού στιγμιότυπα που δεν χρειάζονται πλέον. Συχνές τοπικό στιγμιότυπα μαζί με ένα churn υψηλής δεδομένων του όγκου που σχετίζεται με αυτήν την ομάδα ασφαλείας πολιτική ενδέχεται να προκαλέσει τοπικό χώρο στη συσκευή να καταναλωθεί γρήγορα.|
|Επαναφορά <*αντιγράφου ασφαλείας στοιχείο προέλευσης αναγνωριστικά*> απέτυχε.|Απέτυχε η εργασία επαναφοράς.|Εάν που έχουν καρφιτσωμένα τοπικά ή συνδυασμό τοπικά καρφιτσωμένη και ομότιμου όγκους σε αυτήν την πολιτική ασφαλείας, ανανεώστε τη λίστα αντιγράφων ασφαλείας για να επιβεβαιώσετε ότι το αντίγραφο ασφαλείας είναι εξακολουθεί να ισχύει. Εάν το αντίγραφο ασφαλείας είναι έγκυρες, είναι πιθανό ότι αντιμετωπίζετε προβλήματα συνδεσιμότητας cloud αποτρέπουν την επιτυχή ολοκλήρωση της λειτουργίας επαναφοράς. Τα τοπικά καρφιτσωμένη όγκους που ανακτήθηκαν ως μέρος αυτής της ομάδας στιγμιότυπο δεν έχουν όλα τα δεδομένα που έχουν ληφθεί στη συσκευή και, εάν έχετε ένα μείγμα ομότιμου και τοπικά καρφιτσωμένη όγκους σε αυτή την ομάδα στιγμιότυπο, δεν θα είναι σε συγχρονισμό μεταξύ τους. Για να ολοκληρωθεί με επιτυχία η λειτουργία επαναφοράς, για να κρατήσετε το όγκους σε αυτήν την ομάδα για εργασία χωρίς σύνδεση στον κεντρικό υπολογιστή και προσπαθήστε ξανά τη λειτουργία επαναφοράς. Σημειώστε ότι όποιες αλλαγές στα δεδομένα τόμου που εκτελέστηκαν κατά τη διαδικασία επαναφοράς θα χαθούν.|

### <a name="networking-alerts"></a>Ειδοποιήσεις δικτύωση

|Ειδοποίηση κειμένου|Συμβάν|Περισσότερες πληροφορίες / προτεινόμενες ενέργειες|
|:---|:---|:---|
|Δεν ήταν δυνατή η εκκίνηση StorSimple υπηρεσίες.|Σφάλμα DataPath |Εάν το πρόβλημα δεν επιλυθεί, επικοινωνήστε με την υποστήριξη της Microsoft.|
|Διπλή διεύθυνση IP για τον 'Data0'.| |Το σύστημα έχει εντοπίσει μια διένεξη για τη διεύθυνση IP '10.0.0.1'. Ο πόρος δικτύου 'Data0' στη συσκευή *<device1>* είναι εκτός σύνδεσης. Βεβαιωθείτε ότι αυτή η διεύθυνση IP δεν χρησιμοποιείται από οποιαδήποτε άλλη οντότητα σε αυτό το δίκτυο. Για την αντιμετώπιση προβλημάτων δικτύου, μεταβείτε στην [Αντιμετώπιση προβλημάτων με το cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Επικοινωνήστε με το διαχειριστή του δικτύου σας για βοήθεια για την επίλυση αυτού του ζητήματος. Εάν το πρόβλημα δεν επιλυθεί, επικοινωνήστε με την υποστήριξη της Microsoft. |
|Διεύθυνση IPv4 (ή IPv6) 'Data0' είναι χωρίς σύνδεση.| |Ο πόρος δικτύου 'Data0' με διεύθυνση IP '10.0.0.1'. και το μήκος '22' στη συσκευή του προθέματος *<device1>* είναι εκτός σύνδεσης. Βεβαιωθείτε ότι οι θύρες εναλλαγή στην οποία είναι συνδεδεμένη αυτή η διασύνδεση είναι λειτουργικές. Για την αντιμετώπιση προβλημάτων δικτύου, μεταβείτε στην [Αντιμετώπιση προβλημάτων με το cmdlet Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
 

### <a name="performance-alerts"></a>Ειδοποιήσεις επιδόσεων

|Ειδοποίηση κειμένου|Συμβάν|Περισσότερες πληροφορίες / προτεινόμενες ενέργειες|
|:---|:---|:---|
|Η φόρτωση συσκευή έχει υπερβεί <*όριο*>.|Μικρότερη από αναμένεται χρόνους απόκρισης.|Συσκευή σας αναφορές χρήσης ένα φόρτο εργασίας εισόδου/εξόδου. Αυτό μπορεί να προκαλέσει τη συσκευή σας να μην λειτουργεί επίσης όπως θα έπρεπε. Εξέταση του όγκου εργασίας που έχετε επισυνάψει στη συσκευή και να προσδιορίσετε αν υπάρχουν οποιαδήποτε που ήταν δυνατή η μετακίνηση σε άλλη συσκευή ή που δεν είναι πλέον απαραίτητες.<br>Για να κατανοήσετε την τρέχουσα κατάσταση, μεταβείτε στο θέμα [χρήση της υπηρεσίας διαχείρισης StorSimple για την παρακολούθηση της συσκευής σας](storsimple-monitor-device.md)|

### <a name="security-alerts"></a>Προειδοποιήσεις ασφαλείας

|Ειδοποίηση κειμένου|Συμβάν|Περισσότερες πληροφορίες / προτεινόμενες ενέργειες|
|:---|:---|:---|
|Έχει ξεκινήσει η περίοδος λειτουργίας υποστήριξης της Microsoft.|Τρίτων κατασκευαστών προσβάσιμες υποστήριξη περιόδου λειτουργίας.|Επιβεβαιώστε επιτρέπεται η πρόσβαση σε αυτήν. Αφού έχετε αναλάβει κατάλληλη ενέργεια, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.|
|Κωδικός πρόσβασης για <*στοιχείο*> θα λήξει <*χρονικό διάστημα*>.|Πλησιάζει λήξη κωδικού πρόσβασης.|Αλλαγή κωδικού πρόσβασης πριν από τη λήξη.|
|Πληροφορίες ρύθμισης παραμέτρων ασφαλείας λείπει το για <*Αναγνωριστικό στοιχείου*>.||Ο όγκος που σχετίζονται με αυτό το κοντέινερ ένταση δεν μπορούν να χρησιμοποιηθούν για την αναπαραγωγή της ρύθμισης παραμέτρων StorSimple. Για να εξασφαλίσετε ότι τα δεδομένα σας έχουν αποθηκευτεί με ασφάλεια, συνιστάται να διαγράψετε το κοντέινερ ένταση και οποιαδήποτε όγκους που σχετίζεται με το κοντέινερ έντασης ήχου. Αφού έχετε αναλάβει κατάλληλη ενέργεια, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.|
|<*αριθμός*> προσπάθειες σύνδεσης απέτυχε για <*Αναγνωριστικό στοιχείου*>.|Πολλές αποτυχημένων προσπαθειών.|Συσκευή σας ενδέχεται να δέχεται επίθεση ή εξουσιοδοτημένος χρήστης προσπαθήσει να συνδεθείτε με έναν εσφαλμένο κωδικό πρόσβασης.<ul><li>Επικοινωνήστε με τους εξουσιοδοτημένους χρήστες σας και βεβαιωθείτε ότι αυτές οι προσπάθειες ήταν από μια νόμιμη προέλευση. Εάν εξακολουθείτε να βλέπετε μεγάλου αριθμού ανεπιτυχείς προσπάθειες σύνδεσης, εξετάστε το ενδεχόμενο απενεργοποίηση της απομακρυσμένης διαχείρισης και να επικοινωνήσετε με το διαχειριστή του δικτύου σας. Αφού έχετε αναλάβει κατάλληλη ενέργεια, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.</li><li>Ελέγξτε ότι το παρουσίες Διαχείριση στιγμιοτύπων έχουν ρυθμιστεί με τον σωστό κωδικό πρόσβασης. Αφού έχετε αναλάβει κατάλληλη ενέργεια, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.</li></ul>Για περισσότερες πληροφορίες, μεταβείτε στο θέμα [Αλλαγή κωδικός πρόσβασης έχει λήξει συσκευή](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password).|
|Παρουσιάστηκε ένα ή περισσότερα αποτυχίες κατά την αλλαγή του κλειδιού κρυπτογράφησης δεδομένων υπηρεσίας.||Παρουσιάστηκαν σφάλματα κατά την αλλαγή του κλειδιού κρυπτογράφησης δεδομένων υπηρεσίας. Αφού έχετε απαντήσει τις συνθήκες σφάλματος, εκτελέστε το `Invoke-HcsmServiceDataEncryptionKeyChange` cmdlet από το περιβάλλον εργασίας των Windows PowerShell για StorSimple στη συσκευή σας για να ενημερώσετε την υπηρεσία. Εάν αυτό το πρόβλημα δεν επιλυθεί, επικοινωνήστε με την υποστήριξη της Microsoft. Μόλις διορθώσετε το πρόβλημα, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.|

### <a name="support-package-alerts"></a>Υποστήριξη ειδοποιήσεων πακέτου

|Ειδοποίηση κειμένου|Συμβάν|Περισσότερες πληροφορίες / προτεινόμενες ενέργειες|
|:---|:---|:---|
|Απέτυχε η δημιουργία πακέτου υποστήριξης.|StorSimple δεν μπόρεσε να δημιουργήσει το πακέτο.|Επαναλάβετε αυτήν τη λειτουργία. Εάν το πρόβλημα δεν επιλυθεί, επικοινωνήστε με την υποστήριξη της Microsoft. Αφού έχετε επιλύσει το πρόβλημα, καταργήστε την επιλογή αυτήν την ειδοποίηση από τη σελίδα ειδοποιήσεις.|

### <a name="update-alerts"></a>Ειδοποιήσεις ενημέρωσης

|Ειδοποίηση κειμένου|Συμβάν|Περισσότερες πληροφορίες / προτεινόμενες ενέργειες|
|:---|:---|:---|
|Εγκατεστημένη επείγουσα επιδιόρθωση.|Η ενημέρωση λογισμικού/υλικολογισμικού ολοκληρώθηκε.|Η επείγουσα επιδιόρθωση εγκαταστάθηκε με επιτυχία στη συσκευή σας.|
|Μη αυτόματες ενημερώσεις διαθέσιμες.|Ειδοποίηση για διαθέσιμες ενημερώσεις.|Χρησιμοποιήστε το περιβάλλον εργασίας του Windows PowerShell για StorSimple στη συσκευή σας για να εγκαταστήσετε αυτές τις ενημερώσεις. <br>Για περισσότερες πληροφορίες, μεταβείτε για να [ενημερώσετε τη συσκευή σας σειρά 8000 StorSimple](storsimple-update-device.md).|
|Διαθέσιμες νέες ενημερώσεις.|Ειδοποίηση για διαθέσιμες ενημερώσεις.|Μπορείτε να εγκαταστήσετε αυτές τις ενημερωμένες εκδόσεις, είτε από τη σελίδα **συντήρησης** ή χρησιμοποιώντας το περιβάλλον εργασίας του Windows PowerShell για StorSimple στη συσκευή σας. <br>Για περισσότερες πληροφορίες, μεταβείτε για να [ενημερώσετε τη συσκευή σας σειρά 8000 StorSimple](storsimple-update-device.md).|
|Απέτυχε η εγκατάσταση των ενημερώσεων.|Ενημερώσεις δεν εγκαταστάθηκαν με επιτυχία.|Το σύστημα δεν ήταν δυνατό να εγκαταστήσετε τις ενημερώσεις. Μπορείτε να εγκαταστήσετε αυτές τις ενημερωμένες εκδόσεις, είτε από τη σελίδα **συντήρησης** ή χρησιμοποιώντας το περιβάλλον εργασίας του Windows PowerShell για StorSimple στη συσκευή σας. Εάν το πρόβλημα δεν επιλυθεί, επικοινωνήστε με την υποστήριξη της Microsoft. <br>Για περισσότερες πληροφορίες, μεταβείτε για να [ενημερώσετε τη συσκευή σας σειρά 8000 StorSimple](storsimple-update-device.md).|
|Δεν είναι δυνατός ο αυτόματος έλεγχος για νέες ενημερώσεις.|Απέτυχε η αυτόματη.|Μπορείτε να ελέγξετε με μη αυτόματο τρόπο για νέες ενημερώσεις από τη σελίδα **συντήρησης** .|
|Δημιουργία παράγοντα της WUA διαθέσιμη.|Ειδοποίηση για διαθέσιμες ενημερώσεις.|Κάντε λήψη του τελευταίου Windows Update Agent και να το εγκαταστήσετε από το περιβάλλον εργασίας του Windows PowerShell.|
|Έκδοση υλικολογισμικού στοιχείου <*Αναγνωριστικό στοιχείου*> δεν συμφωνεί με το υλικό.|Ενημερωμένες υλικολογισμικού δεν εγκαταστάθηκαν με επιτυχία.|Επικοινωνήστε με την υποστήριξη της Microsoft.|

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με [σφάλματα StorSimple και αντιμετώπιση προβλημάτων συσκευής λειτουργικές](storsimple-troubleshoot-operational-device.md).
