<properties
   pageTitle="Ανάπτυξη πίνακα εικονικές StorSimple 3 - ρύθμιση στη συσκευή εικονικού διακομιστή αρχείων"
   description="Αυτό το πρόγραμμα εκμάθησης τρίτο σε ανάπτυξη του πίνακα εικονικού StorSimple σας δίνει οδηγίες για να ρυθμίσετε μια εικονική συσκευή ως διακομιστή αρχείων."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/26/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---set-up-as-file-server"></a>Ανάπτυξη πίνακα εικονικές StorSimple - Ορισμός του ως διακομιστή αρχείων

![](./media/storsimple-ova-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Εισαγωγή 

Σε αυτό το άρθρο ισχύει για το Microsoft Azure StorSimple εικονικού πίνακα (γνωστό και ως το StorSimple εσωτερικής εικονικού ή StorSimple εικονικού συσκευής) εκτελείται έκδοση γενικής διαθεσιμότητας (GA) Μαρτίου 2016. Σε αυτό το άρθρο περιγράφει τον τρόπο για να εκτελέσετε αρχική ρύθμιση, καταχώρηση διακομιστή αρχείων StorSimple, ολοκληρώσετε τη ρύθμιση της συσκευής, και δημιουργία και συνδεθείτε σε κοινόχρηστα στοιχεία Ερωτήσεων. Αυτό είναι το τελευταίο άρθρο στη σειρά προγραμμάτων εκμάθησης ανάπτυξης που απαιτούνται για την ανάπτυξη εντελώς το εικονικό πίνακα ως διακομιστή αρχείων ή σε διακομιστή iSCSI.

Η διαδικασία εγκατάστασης και ρύθμισης παραμέτρων μπορεί να διαρκέσει περίπου 10 λεπτά για να ολοκληρωθεί.


## <a name="setup-prerequisites"></a>Προαπαιτούμενα στοιχεία εγκατάστασης

Πριν να ρυθμίσετε τις παραμέτρους και ρύθμιση της συσκευής εικονικού StorSimple, βεβαιωθείτε ότι:

-   Έχετε παρασχεθεί εικονική συσκευή και συνδεδεμένος με αυτό, όπως περιγράφεται στη [διάταξη ενός πίνακα εικονικού StorSimple σε Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) ή [παροχή μια εικονική πίνακας StorSimple VMware](storsimple-ova-deploy2-provision-vmware.md).

-   Έχετε το κλειδί εγγραφής υπηρεσίας από την υπηρεσία διαχείρισης StorSimple που δημιουργήσατε για να διαχειριστείτε συσκευές εικονικού StorSimple. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [βήμα 2: γρήγορα την υπηρεσία εγγραφής κλειδί](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) για StorSimple εικονικό πίνακα.

-   Εάν αυτή είναι η δεύτερη ή οι επόμενες εικονική συσκευή που καταχώρηση με μια υπάρχουσα υπηρεσία StorSimple Manager, θα πρέπει να έχετε του κλειδιού κρυπτογράφησης δεδομένων υπηρεσίας. Αυτό το κλειδί δημιουργήθηκε κατά την πρώτη συσκευή έχει καταχωρηθεί με επιτυχία με αυτήν την υπηρεσία. Εάν έχετε χάσει αυτό το κλειδί, ανατρέξτε στο θέμα [λήψη του κλειδιού κρυπτογράφησης δεδομένων υπηρεσίας](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) για το εικονικό πίνακα StorSimple.

## <a name="step-by-step-setup"></a>Το πρόγραμμα εγκατάστασης βήμα προς βήμα

Χρησιμοποιήστε τις παρακάτω οδηγίες βήμα προς βήμα για να ρυθμίσετε και να ρυθμίσετε τη συσκευή εικονικού σας StorSimple.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Βήμα 1: Ολοκληρώσετε τη ρύθμιση του περιβάλλοντος εργασίας Χρήστη τοπικό web και καταχώρηση τη συσκευή σας 


#### <a name="to-complete-the-setup-and-register-the-device"></a>Για να ολοκληρώσετε τη ρύθμιση και να καταχωρήσετε τη συσκευή

1.  Ανοίξτε ένα παράθυρο του προγράμματος περιήγησης και συνδεθείτε με το τοπικό web περιβάλλοντος εργασίας Χρήστη. Τύπος: 

    `https://<ip-address of network interface>`

    Χρησιμοποιήστε τη διεύθυνση URL σύνδεσης σημειώσατε στο προηγούμενο βήμα. Θα εμφανιστεί ένα σφάλμα που υποδηλώνει ότι υπάρχει κάποιο πρόβλημα με το πιστοποιητικό ασφαλείας της τοποθεσίας Web. Κάντε κλικ στο κουμπί **συνέχεια σε αυτήν την ιστοσελίδα**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image2.png)

1.  Πραγματοποιήστε είσοδο στο περιβάλλον εργασίας Χρήστη της συσκευής σας εικονικού web ως **StorSimpleAdmin**. Πληκτρολογήστε τον κωδικό πρόσβασης διαχειριστή συσκευής που αλλάξατε στο βήμα 3: εκκίνηση της συσκευής εικονικού παροχή [Μια εικονική πίνακας StorSimple Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) ή παροχή [Μια εικονική πίνακας StorSimple VMware](storsimple-ova-deploy2-provision-vmware.md).

    ![](./media/storsimple-ova-deploy3-fs-setup/image3.png)

1.  Θα μεταφερθείτε στη σελίδα **για οικιακή χρήση** . Αυτή η σελίδα περιγράφει τις διάφορες ρυθμίσεις απαιτείται να ρυθμίσετε τις παραμέτρους και να καταχωρήσετε την εικονική συσκευή με την υπηρεσία διαχείρισης StorSimple. Σημειώστε ότι το **δίκτυο ρυθμίσεις** **ρυθμίσεις διακομιστή μεσολάβησης Web**και **ρυθμίσεις ώρας** είναι προαιρετικό. Οι μόνο απαιτούμενες ρυθμίσεις είναι **Cloud ρυθμίσεις**και **Ρυθμίσεις συσκευής** .

    ![](./media/storsimple-ova-deploy3-fs-setup/image4.png)

1.  Στη σελίδα **ρυθμίσεων δικτύου** στην περιοχή **διασυνδέσεις δικτύου**, ΔΕΔΟΜΈΝΑ 0 θα ρυθμιστεί αυτόματα για εσάς. Κάθε διασύνδεση δικτύου έχει οριστεί από προεπιλογή για να πραγματοποιήσετε αυτόματη λήψη διεύθυνση IP (DHCP). Επομένως, μια διεύθυνση IP, υποδίκτυο και πύλη αυτόματα εκχωρούνται (για IPv4 και IPv6).

    ![](./media/storsimple-ova-deploy3-fs-setup/image5.png)

    Αν έχετε προσθέσει περισσότερες από μία διασύνδεση δικτύου κατά την προμήθεια της συσκευής, μπορείτε να ρυθμίσετε τους εδώ. Σημείωση Μπορείτε να ρυθμίσετε το περιβάλλον εργασίας του δικτύου ως IPv4 μόνο ή ως IPv4 και IPv6. Το IPv6 δεν υποστηρίζονται μόνο ρυθμίσεις παραμέτρων.

1.  Οι διακομιστές DNS απαιτούνται επειδή χρησιμοποιούνται όταν επιχειρεί τη συσκευή σας για να επικοινωνήσετε με τις υπηρεσίες παροχής χώρο αποθήκευσης στο cloud ή για να επιλύσετε τη συσκευή σας με βάση το όνομα όταν έχει ρυθμιστεί ως διακομιστής αρχείων. Στη σελίδα **ρυθμίσεων δικτύου** στην περιοχή τους **διακομιστές DNS**:

    1.  Διακομιστής DNS κύριας και δευτερεύουσας θα ρυθμιστούν αυτόματα. Εάν επιλέξετε να ρυθμίσετε τις παραμέτρους στατικές διευθύνσεις IP, μπορείτε να καθορίσετε τους διακομιστές DNS. Για υψηλή διαθεσιμότητα, συνιστάται να ρυθμίσετε μια κύρια και μια δευτερεύουσα διακομιστή DNS.

    2.  Κάντε κλικ στο κουμπί **εφαρμογή**. Θα ισχύουν και επικυρώστε τις ρυθμίσεις δικτύου.

2.  Στη σελίδα **Ρυθμίσεις συσκευής** :

    1.  Αντιστοιχίστε ένα μοναδικό **όνομα** για τη συσκευή σας. Αυτό το όνομα μπορεί να είναι 1-15 χαρακτήρες και μπορεί να περιέχει το γράμμα, αριθμών και παύλες.

    2.  Κάντε κλικ στο εικονίδιο **διακομιστή αρχείων** ![](./media/storsimple-ova-deploy3-fs-setup/image6.png) για τον **τύπο** της συσκευής που θέλετε να δημιουργήσετε. Διακομιστής αρχείων θα σας επιτρέψει να δημιουργήσετε κοινόχρηστους φακέλους.

    3.  Καθώς η συσκευή σας είναι ένα διακομιστή αρχείων, θα πρέπει να συνδέσετε τη συσκευή σε έναν τομέα. Πληκτρολογήστε ένα **όνομα τομέα**.

    1.  Κάντε κλικ στο κουμπί **εφαρμογή**.

2.  Θα εμφανιστεί ένα παράθυρο διαλόγου. Εισαγάγετε τα διαπιστευτήριά σας τομέα στην καθορισμένη μορφή. Κάντε κλικ στο εικονίδιο ελέγχου. Τα διαπιστευτήρια του τομέα σας, θα γίνει επαλήθευση. Εάν τα διαπιστευτήρια δεν είναι σωστοί, θα δείτε ένα μήνυμα σφάλματος.

    ![](./media/storsimple-ova-deploy3-fs-setup/image7.png)

1.  Κάντε κλικ στο κουμπί **εφαρμογή**. Θα ισχύουν και επικυρώστε τις ρυθμίσεις συσκευής.

    ![](./media/storsimple-ova-deploy3-fs-setup/image8.png)

    > [AZURE.NOTE]
    > 
    > Βεβαιωθείτε ότι το εικονικό πίνακα είναι στο δικό του οργανική μονάδα (OU) για την υπηρεσία καταλόγου Active Directory και είναι εφαρμοσμένη ή έχουν μεταβιβαστεί χωρίς αντικείμενα πολιτικής ομάδας (αντικείμενο πολιτικής ΟΜΆΔΑΣ). Πολιτική ομάδας μπορεί να εγκαταστήσετε εφαρμογές, όπως το λογισμικό προστασίας από ιούς σε έναν πίνακα εικονικού StorSimple. Εγκατάσταση πρόσθετου λογισμικού δεν υποστηρίζεται και μπορεί να οδηγήσει σε καταστροφή δεδομένων. 

1.  (Προαιρετικά) ρύθμιση παραμέτρων του διακομιστή μεσολάβησης web. Παρόλο που η ρύθμιση παραμέτρων διακομιστή μεσολάβησης web είναι προαιρετική, έχετε υπόψη ότι εάν χρησιμοποιείτε ένα διακομιστή μεσολάβησης web, μπορείτε επίσης να ρυθμίσετε το εδώ.

    ![](./media/storsimple-ova-deploy3-fs-setup/image9.png)

    Στη σελίδα **διακομιστή μεσολάβησης Web** :

    1.  Παρέχετε τη **διεύθυνση URL του διακομιστή μεσολάβησης Web** σε αυτήν τη μορφή: *http://&lt;διεύθυνση IP κεντρικού υπολογιστή ή το FDQN&gt;: τον αριθμό θύρας*. Σημειώστε ότι URL HTTPS δεν υποστηρίζονται.

    2.  Καθορίστε **τον έλεγχο ταυτότητας** ως **βασικές** ή **κανένα**.

    3.  Εάν χρησιμοποιείτε τον έλεγχο ταυτότητας, θα πρέπει επίσης να δώσετε ένα **όνομα χρήστη** και **τον κωδικό πρόσβασης**.

    4.  Κάντε κλικ στο κουμπί **εφαρμογή**. Αυτό θα επικύρωση και να εφαρμόσετε τις ρυθμίσεις διακομιστή μεσολάβησης έχει ρυθμιστεί web.

1.  (Προαιρετικά) ρυθμίστε τις παραμέτρους των ρυθμίσεων ώρας για τη συσκευή σας, όπως η ζώνη ώρας και τους διακομιστές NTP κύριας και δευτερεύουσας. Απαιτούνται διακομιστές NTP, επειδή η συσκευή σας πρέπει να συγχρονίσετε φορά, έτσι ώστε να έλεγχο ταυτότητας με τις υπηρεσίες παροχής cloud.

    ![](./media/storsimple-ova-deploy3-fs-setup/image10.png)

    Στη σελίδα **ρυθμίσεις χρόνου** :

    1.  Από την αναπτυσσόμενη λίστα, επιλέξτε τη **ζώνη ώρας** με βάση τη γεωγραφική θέση στην οποία αναπτύσσεται στη συσκευή. Η προεπιλεγμένη ζώνη ώρας για τη συσκευή σας είναι PST. Συσκευή σας θα χρησιμοποιήσει αυτήν τη ζώνη ώρας για όλες τις προγραμματισμένες εργασίες.

    2.  Καθορισμός **διακομιστή κύρια NTP** για τη συσκευή σας ή να αποδεχθείτε την προεπιλεγμένη τιμή του time.windows.com. Βεβαιωθείτε ότι το δίκτυό σας επιτρέπει την κυκλοφορία NTP για τη μεταβίβαση από το κέντρο δεδομένων στο Internet.

    3.  Προαιρετικά, καθορίστε έναν **δευτερεύοντα NTP server** για τη συσκευή σας.

    4.  Κάντε κλικ στο κουμπί **εφαρμογή**. Αυτό θα επικύρωση και να εφαρμόσετε τις ρυθμίσεις του καθορισμένου χρόνου.

1.  Ρυθμίστε τις παραμέτρους cloud για τη συσκευή σας. Σε αυτό το βήμα, θα ολοκληρώσετε τη ρύθμιση παραμέτρων τοπική συσκευή και, στη συνέχεια, να καταχωρήσετε τη συσκευή με την υπηρεσία StorSimple Manager.

    1.  Πληκτρολογήστε το **κλειδί υπηρεσίας εγγραφής** που λάβατε στο [βήμα 2: γρήγορα την υπηρεσία εγγραφής κλειδί](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) για StorSimple εικονικό πίνακα.

    2.  Παραλείψετε αυτό το βήμα, εάν πρόκειται για την πρώτη συσκευή την καταχώρηση με αυτήν την υπηρεσία και μεταβείτε στο επόμενο βήμα. Εάν αυτό δεν είναι η πρώτη συσκευή που καταχώρηση με αυτήν την υπηρεσία, θα πρέπει να παρέχουν του **κλειδιού κρυπτογράφησης δεδομένων υπηρεσίας**. Αυτό το κλειδί είναι υποχρεωτική για το κλειδί εγγραφής υπηρεσίας για να καταχωρήσετε πρόσθετες συσκευές με την υπηρεσία διαχείρισης StorSimple. Για περισσότερες πληροφορίες, ανατρέξτε για να λάβετε του [κλειδιού κρυπτογράφησης δεδομένων υπηρεσίας](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) για το τοπικό web περιβάλλοντος εργασίας Χρήστη.

    3.  Κάντε κλικ στην επιλογή **καταχώρηση**. Αυτό θα γίνει επανεκκίνηση της συσκευής. Ίσως χρειαστεί να περιμένετε 2-3 λεπτά πριν από τη συσκευή έχει καταχωρηθεί με επιτυχία. Μετά την επανεκκίνηση της συσκευής, θα μεταφερθείτε στη σελίδα εισόδου.

        ![](./media/storsimple-ova-deploy3-fs-setup/image13.png)
    

1.  Επιστρέψτε στην πύλη του Azure κλασική. Στη σελίδα **συσκευές** , βεβαιωθείτε ότι η συσκευή έχει συνδεθεί με επιτυχία με την υπηρεσία αναζητώντας την κατάσταση. Η κατάσταση συσκευής πρέπει να είναι **ενεργό**.

![](./media/storsimple-ova-deploy3-fs-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Βήμα 2: Ολοκληρώσετε τη ρύθμιση απαιτούμενη συσκευή

Για να ολοκληρώσετε τις παραμέτρους της συσκευής της συσκευής σας StorSimple, πρέπει να:

-   Επιλέξτε ένα λογαριασμό χώρου αποθήκευσης για να συσχετίσετε με τη συσκευή σας.

-   Επιλέξτε ρυθμίσεις κρυπτογράφησης για τα δεδομένα που αποστέλλονται στο cloud.

Εκτελέστε τα ακόλουθα βήματα στην [πύλη του Azure κλασική](https://manage.windowsazure.com/) για να ολοκληρώσετε τη ρύθμιση απαιτούμενη συσκευή.

#### <a name="to-complete-the-minimum-device-setup"></a>Για να ολοκληρώσετε τη ρύθμιση ελάχιστη συσκευή

1.  Από τη σελίδα **συσκευές** , επιλέξτε τη συσκευή που μόλις δημιουργήσατε. Αυτή η συσκευή θα εμφανίζεται ως **ενεργός**. Κάντε κλικ στο βέλος σε σχέση με το όνομα της συσκευής και, στη συνέχεια, κάντε κλικ στην επιλογή **Γρήγορης εκκίνησης**.

2.  Κάντε κλικ στην επιλογή **Ρύθμιση ολοκλήρωσης συσκευή** για να ξεκινήσετε τον Οδηγό ρύθμιση παραμέτρων συσκευή.

3.  Στον Οδηγό ρύθμισης συσκευής στη σελίδα **Βασικές ρυθμίσεις** , κάντε τα εξής:

    1.  Καθορίστε ένα λογαριασμό χώρου αποθήκευσης που θα χρησιμοποιηθεί με τη συσκευή σας. Μπορείτε να επιλέξετε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης σε αυτήν τη συνδρομή από την αναπτυσσόμενη λίστα ή να καθορίσετε **Προσθήκη περισσότερων** για να επιλέξετε ένα λογαριασμό από μια διαφορετική συνδρομή.

    2.  Ορισμός των ρυθμίσεων κρυπτογράφησης για όλα τα δεδομένα-σε αδράνεια (κρυπτογράφηση AES) που θα σταλεί στο cloud. Για την κρυπτογράφηση των δεδομένων σας, ελέγξτε το σύνθετο πλαίσιο για να **ενεργοποιήσετε την κλειδιού κρυπτογράφησης χώρο αποθήκευσης στο cloud**. Εισαγάγετε ένα κρυπτογράφησης χώρο αποθήκευσης στο cloud που περιέχει 32 χαρακτήρες. Ξανά το κλειδί για να τον επιβεβαιώσετε. Ένα κλειδί AES 256-bit θα χρησιμοποιηθεί με το κλειδί που ορίζονται από το χρήστη για την κρυπτογράφηση.

    3.  Κάντε κλικ στο εικονίδιο ελέγχου ![](./media/storsimple-ova-deploy3-fs-setup/image15.png).

        ![](./media/storsimple-ova-deploy3-fs-setup/image16.png)

Οι ρυθμίσεις θα ενημερωθούν. Αφού ρυθμίσεις είναι ενημερωθεί με επιτυχία, θα είναι γκριζαρισμένη το κουμπί εγκατάσταση πλήρους διάταξης. Θα επιστρέψετε στη σελίδα **Γρήγορης εκκίνησης** της συσκευής.

 ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)


> [AZURE.NOTE]                                                              
>
> Μπορείτε να τροποποιήσετε όλες οι άλλες ρυθμίσεις συσκευής ανά πάσα στιγμή, μεταβαίνοντας στη σελίδα **Ρύθμιση παραμέτρων** .

## <a name="step-3-add-a-share"></a>Βήμα 3: Προσθέστε ένα κοινόχρηστο στοιχείο

Εκτελέστε τα ακόλουθα βήματα στην [πύλη του Azure κλασική](https://manage.windowsazure.com/) για να δημιουργήσετε ένα κοινόχρηστο στοιχείο.

#### <a name="to-create-a-share"></a>Για να δημιουργήσετε ένα κοινόχρηστο στοιχείο

1.  Στη σελίδα **Γρήγορης εκκίνησης** συσκευή, κάντε κλικ στην επιλογή **Προσθήκη ένα κοινόχρηστο στοιχείο**. Ξεκινά τον Οδηγό κοινή χρήση προσθήκη.

    ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)

1.  Στη σελίδα **Βασικές ρυθμίσεις** , κάντε τα εξής:

    1.  Καθορίστε ένα μοναδικό όνομα για την κοινή χρήση. Το όνομα πρέπει να είναι μια συμβολοσειρά που περιέχει χαρακτήρες 3 έως 127.

    2.  (Προαιρετικό) Δώστε μια περιγραφή για την κοινή χρήση. Η περιγραφή θα σας βοηθήσει να τον προσδιορισμό των κατόχων κοινή χρήση.

    3.  Επιλογή χρήση τύπου για την κοινή χρήση. Ο τύπος χρήση μπορεί να είναι **Tiered** ή **τοπικά καρφιτσωμένα**, με ομότιμου γίνεται η προεπιλεγμένη. Φόρτους εργασίας που απαιτούν τις εγγυήσεις της τοπικής, χαμηλή των αδρανειών και υψηλότερη απόδοση, επιλέξτε ένα κοινόχρηστο στοιχείο **τοπικά καρφιτσωμένες** . Για όλα τα άλλα δεδομένα, επιλέξτε ένα κοινόχρηστο στοιχείο **Tiered** .

    Κοινόχρηστο στοιχείο τοπικά καρφιτσωμένη παρέχεται thickly και εξασφαλίζει ότι τα πρωτεύοντα δεδομένα στο κοινόχρηστο στοιχείο παραμένει τοπικά στη συσκευή και δεν γίνει υπερχείλιση του στο cloud. Κοινόχρηστο στοιχείο ομότιμου από την άλλη πλευρά αραιά παρέχεται. Όταν δημιουργείτε ένα κοινόχρηστο στοιχείο ομότιμου, 10% του χώρου που παρέχεται στο επίπεδο τοπικού και 90% του χώρου που παρέχεται στο cloud. Για παράδειγμα, εάν αποδοθεί ενός όγκου 1 TB, 100 GB θα βρίσκονται στο τοπικό χώρο και 900 GB θα χρησιμοποιούνται στο cloud όταν τις σειρές δεδομένων. Αυτό με τη σειρά σημαίνει ότι εάν εκτελείτε από ολόκληρο το τοπικό χώρο στη συσκευή, δεν είναι δυνατό να προμηθεύσουν ένα ομότιμου κοινή χρήση.

1.  Καθορίστε την προμήθεια του φακέλου δυναμικότητα για την κοινή χρήση. Σημειώστε ότι η καθορισμένη δυναμικότητα πρέπει να είναι μικρότερη από τη διαθέσιμη δυναμικότητα. Εάν χρησιμοποιείτε ένα κοινόχρηστο στοιχείο ομότιμου, το μέγεθος κοινή χρήση πρέπει να είναι μεταξύ 500 GB και 20 TB. Για ένα κοινόχρηστο στοιχείο τοπικά καρφιτσωμένη, καθορίστε το μέγεθος κοινή χρήση μεταξύ 50 GB και 2 TB. Χρησιμοποιήστε τη διαθέσιμη δυναμικότητα ως οδηγό για να προμηθεύσουν ένα κοινόχρηστο στοιχείο. Εάν η διαθέσιμη δυναμικότητα τοπικό είναι 0 GB, στη συνέχεια, που δεν θα επιτρέπεται η προμήθεια τοπικής ή ομότιμου κοινόχρηστα στοιχεία.

    ![](./media/storsimple-ova-deploy3-fs-setup/image18.png)

1.  Κάντε κλικ στο εικονίδιο βέλους ![](./media/storsimple-ova-deploy3-fs-setup/image19.png) για να μεταβείτε στην επόμενη σελίδα.

1.  Στη σελίδα **Πρόσθετες ρυθμίσεις** , εκχωρήσετε δικαιώματα για το χρήστη ή την ομάδα που θα έχουν πρόσβαση σε αυτό το κοινόχρηστο στοιχείο. Καθορίστε το όνομα του χρήστη ή την ομάδα χρηστών στο *<john@contoso.com>* μορφή. Συνιστάται να χρησιμοποιήσετε μια ομάδα χρηστών (αντί για ένα μεμονωμένο χρήστη) για να επιτρέψετε δικαιώματα διαχειριστή για να αποκτήσετε πρόσβαση σε αυτά τα κοινόχρηστα στοιχεία. Αφού έχετε εκχωρήσει τα δικαιώματα εδώ, μπορείτε να χρησιμοποιήσετε Εξερεύνηση των Windows, στη συνέχεια, να τροποποιήσετε αυτά τα δικαιώματα.

    ![](./media/storsimple-ova-deploy3-fs-setup/image20.png)

1.  Κάντε κλικ στο εικονίδιο ελέγχου ![](./media/storsimple-ova-deploy3-fs-setup/image21.png). Θα δημιουργηθεί μια κοινή χρήση με τις καθορισμένες ρυθμίσεις. Από προεπιλογή, παρακολούθηση και δημιουργία αντιγράφων ασφαλείας θα είναι ενεργοποιημένες για κοινή χρήση.

## <a name="step-4-connect-to-the-share"></a>Βήμα 4: Σύνδεση με την κοινή χρήση

Τώρα θα πρέπει να συνδεθείτε με το share(s) που δημιουργήσατε στο προηγούμενο βήμα. Ακολουθήστε αυτά τα βήματα στον του κεντρικού υπολογιστή Windows Server.

#### <a name="to-connect-to-the-share"></a>Για να συνδεθείτε με την κοινή χρήση

1.  Πατήστε το πλήκτρο ![](./media/storsimple-ova-deploy3-fs-setup/image22.png) + R. Στο παράθυρο "Εκτέλεση", καθορίστε το * \\ * ως τη διαδρομή, αντικαθιστώντας *όνομα διακομιστή του αρχείου* με το όνομα της συσκευής που έχει εκχωρηθεί στο διακομιστή του αρχείου σας. Κάντε κλικ στο **κουμπί OK**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image23.png)

2.  Αυτό θα ανοίξει τον Explorer. Τώρα θα πρέπει να μπορείτε να δείτε τα κοινόχρηστα στοιχεία που δημιουργήσατε με τους φακέλους. Επιλέξτε και κάντε διπλό κλικ σε ένα κοινόχρηστο στοιχείο (φάκελος) για να προβάλετε το περιεχόμενο.

    ![](./media/storsimple-ova-deploy3-fs-setup/image24.png)

3.  Τώρα, μπορείτε να προσθέσετε αρχεία στο αυτά τα κοινόχρηστα στοιχεία και να λαμβάνουν ένα αντίγραφο ασφαλείας.

![εικονίδιο βίντεο](./media/storsimple-ova-deploy3-fs-setup/video_icon.png) **βίντεο είναι διαθέσιμη**

Παρακολουθήστε το βίντεο για να δείτε πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να καταχωρήσετε έναν πίνακα εικονικού StorSimple ως διακομιστής αρχείων.

> [AZURE.VIDEO configure-a-storsimple-virtual-array]

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε πώς μπορείτε να χρησιμοποιήσετε το τοπικό web περιβάλλοντος εργασίας Χρήστη για να [διαχειριστείτε το εικονικό πίνακα StorSimple](storsimple-ova-web-ui-admin.md).
