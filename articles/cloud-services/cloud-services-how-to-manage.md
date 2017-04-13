<properties 
    pageTitle="Εργασίες διαχείρισης υπηρεσίας κοινές cloud (κλασικό) | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να διαχειριστείτε τις υπηρεσίες cloud στην πύλη του Azure κλασική." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/10/2016"
    ms.author="adegeo"/>





# <a name="how-to-manage-cloud-services"></a>Πώς μπορείτε να διαχειριστείτε τις υπηρεσίες Cloud

> [AZURE.SELECTOR]
- [Πύλη του Azure](cloud-services-how-to-manage-portal.md)
- [Azure κλασική πύλη](cloud-services-how-to-manage.md)

Στην περιοχή **Υπηρεσίες Cloud** της πύλης Azure κλασική, μπορείτε να ενημερώσετε ένα ρόλο υπηρεσίας ή μια ανάπτυξη, Προβιβάστε μια σταδιακή ανάπτυξη παραγωγή, σύνδεση πόρους με την υπηρεσία cloud, ώστε να μπορείτε να δείτε τις εξαρτήσεις πόρων και κλιμάκωση μαζί τους πόρους, και να διαγράψετε μια υπηρεσία cloud ή σε μια ανάπτυξη.


## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Πώς μπορείτε να: Ενημερώστε ένα ρόλο υπηρεσιών cloud ή ανάπτυξης

Εάν πρέπει να ενημερώσετε τον κώδικα της εφαρμογής για την υπηρεσία cloud, χρησιμοποιήστε **Ενημέρωση** σε του πίνακα εργαλείων, **Τις υπηρεσίες Cloud της** σελίδας ή **παρουσίες** σελίδας. Μπορείτε να ενημερώσετε ένα μεμονωμένο ρόλο ή όλους τους ρόλους. Θα χρειαστεί να αποστείλετε ένα νέο πακέτο υπηρεσίας και αρχείου ρύθμισης παραμέτρων υπηρεσίας.

1. Στην [πύλη του Azure κλασική](https://manage.windowsazure.com/), στην πίνακα εργαλείων, **Τις υπηρεσίες Cloud της** σελίδας ή **παρουσίες** σελίδα, κάντε κλικ στην επιλογή **Ενημέρωση**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. Στην **Ανάπτυξη ετικέτας**, πληκτρολογήστε ένα όνομα για τον προσδιορισμό της ανάπτυξης (για παράδειγμα, mycloudservice4). Θα βρείτε την ετικέτα ανάπτυξης στην περιοχή **γρήγορης εκκίνησης** στον πίνακα εργαλείων.

3. Στο **πακέτο**, χρησιμοποιήστε **Αναζήτηση** για να αποστείλετε το αρχείο πακέτου υπηρεσίας (.cspkg).

4. Με τη **ρύθμιση των παραμέτρων**, χρησιμοποιήστε **Αναζήτηση** για να αποστείλετε το αρχείο ρύθμισης παραμέτρων υπηρεσίας (.cscfg).

5. Στο **ρόλο**, επιλέξτε **όλων,** εάν θέλετε να αναβαθμίσετε όλους τους ρόλους στο την υπηρεσία cloud. Για να εκτελέσετε μια ενημέρωση μοναδικού ρόλου, επιλέξτε το ρόλο που θέλετε να ενημερώσετε. Ακόμα και αν επιλέξετε ένα συγκεκριμένο ρόλο για να ενημερώσετε, τις ενημερώσεις στο αρχείο ρύθμισης παραμέτρων της υπηρεσίας εφαρμόζονται σε όλους τους ρόλους.

6. Εάν η ενημερωμένη έκδοση αλλάζει τον αριθμό των ρόλων ή το μέγεθος του οποιονδήποτε ρόλο, επιλέξτε το πλαίσιο ελέγχου **να επιτρέπεται η ενημέρωση εάν μεγέθη ρόλο ή τον αριθμό των ρόλων αλλάζει** για να ενεργοποιήσετε την ενημέρωση για να συνεχίσετε. 

    Πρέπει να γνωρίζετε ότι εάν αλλάξετε το μέγεθος του ρόλου (δηλαδή, το μέγεθος των μια εικονική μηχανή που φιλοξενεί μια παρουσία ρόλο) ή τον αριθμό των ρόλων, κάθε παρουσία ρόλο (εικονικό μηχάνημα) πρέπει να είναι εκ νέου τους και οποιαδήποτε τοπικών δεδομένων θα χαθούν.

7. Εάν οποιαδήποτε υπηρεσία ρόλους έχετε μόνο μία παρουσία ρόλο, επιλέξτε την **Ενημέρωση ακόμα και εάν μία ή περισσότερες ρόλο περιέχει ένα πλαίσιο ελέγχου μεμονωμένη παρουσία** για να ενεργοποιήσετε την αναβάθμιση για να συνεχίσετε. 

    Azure μόνο να εγγυάται διαθεσιμότητα υπηρεσίας τοις εκατό 99.95 κατά τη διάρκεια μιας ενημέρωσης υπηρεσία cloud εάν κάθε ρόλο έχει τουλάχιστον δύο παρουσίες ρόλο (εικονικές μηχανές). Που ενεργοποιεί μία εικονική μηχανή για επεξεργασία αιτήσεων προγράμματος-πελάτη, ενώ το άλλο ενημερώνεται.

8. Κάντε κλικ στο **κουμπί OK** (σημάδι ελέγχου) για να ξεκινήσει η ενημέρωση της υπηρεσίας.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Πώς μπορείτε να: εναλλαγή αναπτύξεις για να προβιβάσετε μια σταδιακή ανάπτυξη παραγωγή

Χρησιμοποιήστε την **Εναλλαγή** για να προβιβάσετε μια ενδιάμεσου σταδίου ανάπτυξη από μια υπηρεσία cloud παραγωγή. Εάν αποφασίσετε να αναπτύξετε μια νέα έκδοση της μια υπηρεσία cloud, μπορείτε να του σταδίου και δοκιμή σας νέα έκδοση στο περιβάλλον ενδιάμεσου σταδίου σας υπηρεσία cloud, ενώ οι πελάτες σας που χρησιμοποιούν την τρέχουσα έκδοση της παραγωγής. Όταν είστε έτοιμοι για να προωθήσετε τη νέα έκδοση παραγωγή, μπορείτε να χρησιμοποιήσετε την **Εναλλαγή** για να αλλάξετε τις διευθύνσεις URL με τον οποίο αντιμετωπίζονται τα δύο αναπτύξεις. 

Μπορείτε να κάνετε εναλλαγή αναπτύξεις από τη σελίδα **Υπηρεσίες Cloud** ή τον πίνακα εργαλείων.

1. Στην [πύλη του Azure κλασική](https://manage.windowsazure.com/), κάντε κλικ στην επιλογή **Υπηρεσίες Cloud**.

2. Στη λίστα των υπηρεσιών cloud, κάντε κλικ στην υπηρεσία cloud για να το επιλέξετε.

3. Κάντε κλικ στο κουμπί **Εναλλαγή**.

    Ανοίγει το ακόλουθο μήνυμα επιβεβαίωσης.

    ![Κάνετε εναλλαγή υπηρεσίες cloud](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Αφού επαληθεύσετε τις πληροφορίες ανάπτυξης, κάντε κλικ στο κουμπί **Ναι** για να εναλλαγή του αναπτύξεις.

    Της ανταλλαγής ανάπτυξης γρήγορα συμβαίνει επειδή το μόνο πράγμα που αλλάζει το διευθύνσεις IP (VIPs) για το αναπτύξεις.

    Για να αποθηκεύσετε υπολογισμού κόστους, μπορείτε να διαγράψετε την ανάπτυξη του στο περιβάλλον ενδιάμεσου σταδίου όταν είστε βέβαιοι για το νέο ανάπτυξη παραγωγής εκτελεί όπως αναμένεται.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Πώς μπορείτε να: σύνδεση ενός πόρου σε μια υπηρεσία cloud

Για να εμφανίσετε το cloud εξαρτήσεις της υπηρεσίας σε άλλους πόρους, μπορείτε να συνδέσετε μια παρουσία βάσης δεδομένων SQL Azure ή ένα λογαριασμό του χώρου αποθήκευσης με την υπηρεσία cloud. Μπορείτε να συνδέσετε και αποσύνδεση πόρους στη σελίδα **Συνδεδεμένες πόρων** και, στη συνέχεια, να παρακολουθείτε τους χρήση στον πίνακα εργαλείων υπηρεσία cloud. Εάν ένα συνδεδεμένο χώρο αποθήκευσης λογαριασμός έχει παρακολούθηση ενεργοποιημένη, μπορείτε να παρακολουθήσετε το σύνολο των αιτήσεων στον πίνακα εργαλείων υπηρεσία cloud.

Χρήση **σύνδεσης** για να συνδέσετε μια νέα ή υπάρχουσα βάση δεδομένων SQL παρουσία χώρου αποθήκευσης λογαριασμό ή με την υπηρεσία cloud. Μπορείτε, στη συνέχεια, να κλιμάκωση της βάσης δεδομένων μαζί με το ρόλο υπηρεσιών cloud που χρησιμοποιεί στη σελίδα " **κλίμακα** ". (Ένα λογαριασμό του χώρου αποθήκευσης κλιμακώνει αυτόματα ως χρήση αυξάνεται.) Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος για να κλιμακωθεί μια υπηρεσία στο Cloud και συνδεδεμένων πόρους](cloud-services-how-to-scale.md). 

Μπορείτε επίσης να παρακολουθείτε, διαχείριση και κλιμάκωση της βάσης δεδομένων στον κόμβο **βάσεις δεδομένων** της πύλης Azure κλασική. 

"Σύνδεση" ενός πόρου άποψη αυτή δεν σύνδεση της εφαρμογής σας στον πόρο. Εάν δημιουργήσετε μια νέα βάση δεδομένων με χρήση **σύνδεσης**, θα χρειαστεί να προσθέσετε τις συμβολοσειρές σύνδεσης κώδικα της εφαρμογής σας και, στη συνέχεια, να αναβαθμίσετε την υπηρεσία cloud. Μπορείτε, επίσης, θα χρειαστεί να προσθέσετε συμβολοσειρές σύνδεσης, εάν η εφαρμογή σας χρησιμοποιεί πόρων σε ένα χώρο αποθήκευσης στο συνδεδεμένο λογαριασμό.

Η παρακάτω διαδικασία περιγράφει πώς μπορείτε να συνδέσετε μια νέα βάση δεδομένων SQL παρουσία, αναπτυχθεί σε μια νέα βάση δεδομένων SQL server, σε μια υπηρεσία cloud.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Για να συνδέσετε μια παρουσία της βάσης δεδομένων SQL σε μια υπηρεσία cloud

1. Στην [πύλη του Azure κλασική](http://manage.windowsazure.com/), κάντε κλικ στην επιλογή **Υπηρεσίες Cloud**. Στη συνέχεια, κάντε κλικ στο όνομα της υπηρεσίας cloud για να ανοίξετε τον πίνακα εργαλείων.

2. Κάντε κλικ στην επιλογή **συνδεδεμένες πόρους**.

    Ανοίγει η σελίδα **Συνδεδεμένες πόρους** .

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Κάντε κλικ στην επιλογή **σύνδεση έναν πόρο** ή **σύνδεσης**.

    Ξεκινά ο οδηγός **Σύνδεσης πόρο** .

    ![Σύνδεση Σελίδα1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Κάντε κλικ στην επιλογή **Δημιουργία νέου πόρου** ή τη **σύνδεση ενός υπάρχοντος πόρου**.

5. Επιλέξτε τον τύπο του πόρου για να συνδέσετε. Στην [πύλη του Azure κλασική](http://manage.windowsazure.com/), κάντε κλικ στην επιλογή **Βάση δεδομένων SQL**. (Κλασική πύλη του Preview Azure δεν υποστηρίζει σύνδεση ενός λογαριασμού χώρου αποθήκευσης σε μια υπηρεσία cloud.)

6. Για να ολοκληρώσετε τη ρύθμιση παραμέτρων της βάσης δεδομένων, ακολουθήστε οδηγίες στο θέμα Βοήθεια για την περιοχή **Βάσεις δεδομένων SQL** του Azure κλασική πύλη.

    Μπορείτε να ακολουθήσετε την πρόοδο του τη λειτουργία σύνδεσης στην περιοχή του μηνύματος.

    ![Σύνδεση προόδου](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkProgress.png)

    Όταν ολοκληρωθεί η σύνδεση, μπορείτε να παρακολουθείτε την κατάσταση του συνδεδεμένου πόρου στον πίνακα εργαλείων υπηρεσία cloud. Για πληροφορίες σχετικά με την κλιμάκωση μια συνδεδεμένη βάση δεδομένων SQL, δείτε [πώς μπορείτε να κλιμακωθεί μια υπηρεσία στο Cloud και συνδεδεμένων πόρους](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Για να αποσυνδέσετε ένα συνδεδεμένο πόρων

1. Στην [πύλη του Azure κλασική](http://manage.windowsazure.com/), κάντε κλικ στην επιλογή **Υπηρεσίες Cloud**. Στη συνέχεια, κάντε κλικ στο όνομα της υπηρεσίας cloud για να ανοίξετε τον πίνακα εργαλείων.

2. Κάντε κλικ στην επιλογή **Συνδεδεμένες πόρων**και, στη συνέχεια, επιλέξτε τον πόρο.

3. Κάντε κλικ στην επιλογή **Κατάργηση σύνδεσης**. Στη συνέχεια, κάντε κλικ στην επιλογή **Ναι** στην ερώτηση επιβεβαίωσης.

    Κατάργηση σύνδεσης σε μια βάση δεδομένων SQL δεν έχει καμία επίδραση στην τη βάση δεδομένων ή της εφαρμογής των συνδέσεων στη βάση δεδομένων. Εξακολουθείτε να μπορείτε να διαχειριστείτε τη βάση δεδομένων στην περιοχή **Βάσεις δεδομένων SQL** Azure κλασική πύλη.



## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Πώς μπορείτε να: διαγραφή αναπτύξεις και μια υπηρεσία στο cloud

Για να διαγράψετε μια υπηρεσία cloud, πρέπει να διαγράψετε κάθε υπάρχουσας ανάπτυξης.

Για να αποθηκεύσετε υπολογισμού κόστους, μπορείτε να διαγράψετε την ανάπτυξη ενδιάμεσου σταδίου μετά από την επαλήθευση ότι σας ανάπτυξη παραγωγής λειτουργεί όπως αναμένεται. Είστε χρεωθεί υπολογισμού κόστους για το ρόλο παρουσίες ακόμα και αν δεν εκτελείται μια υπηρεσία στο cloud.

Χρησιμοποιήστε την ακόλουθη διαδικασία για να διαγράψετε μια ανάπτυξη ή την υπηρεσία cloud. 

1. Στην [πύλη του Azure κλασική](http://manage.windowsazure.com/), κάντε κλικ στην επιλογή **Υπηρεσίες Cloud**.

2. Επιλέξτε την υπηρεσία cloud, και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαγραφή**. (Για να επιλέξετε μια υπηρεσία cloud, χωρίς να ανοίξετε τον πίνακα εργαλείων, κάντε κλικ σε οποιοδήποτε σημείο εκτός από το όνομα στην καταχώρηση υπηρεσία cloud.)

    Εάν έχετε μια ανάπτυξη σε οργάνωσης ή παραγωγής, θα δείτε ένα μενού με επιλογές παρόμοιο με το εξής στο κάτω μέρος του παραθύρου. Για να διαγράψετε την υπηρεσία cloud, πρέπει να διαγράψετε οποιοδήποτε υπάρχον αναπτύξεις.

    ![Διαγραφή μενού](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)


3. Για να διαγράψετε μια ανάπτυξη, κάντε κλικ στην επιλογή **Διαγραφή ανάπτυξη παραγωγής** ή **Διαγραφή ενδιάμεσου σταδίου ανάπτυξης**. Στη συνέχεια, στην ερώτηση επιβεβαίωσης, κάντε κλικ στο κουμπί **Ναι**. 

4. Εάν σκοπεύετε να διαγράψετε την υπηρεσία cloud, επαναλάβετε το βήμα 3, εάν είναι απαραίτητο, για να διαγράψετε άλλες ανάπτυξής σας.

5. Για να διαγράψετε την υπηρεσία cloud, κάντε κλικ στην επιλογή **Διαγραφή υπηρεσία στο cloud**. Στη συνέχεια, στην ερώτηση επιβεβαίωσης, κάντε κλικ στο κουμπί **Ναι**.

> [AZURE.NOTE]
> Εάν λεπτομερής παρακολούθηση έχει ρυθμιστεί για την υπηρεσία cloud, Azure δεν διαγράφει το παρακολούθησης δεδομένων από το λογαριασμό χώρου αποθήκευσης όταν διαγράφετε την υπηρεσία cloud. Θα πρέπει να διαγράψετε τα δεδομένα με μη αυτόματο τρόπο. Για πληροφορίες σχετικά με τη θέση για να βρείτε τους πίνακες μετρήσεις, ανατρέξτε στο θέμα "Πώς να: Access λεπτομερή δεδομένα έξω από την πύλη του Azure κλασική παρακολούθησης" στο θέμα [Πώς να τις υπηρεσίες Cloud της οθόνης](cloud-services-how-to-monitor.md).

## <a name="next-steps"></a>Επόμενα βήματα

 * [Γενικές ρύθμιση παραμέτρων για την υπηρεσία cloud](cloud-services-how-to-configure.md).
* Μάθετε πώς να [αναπτύξετε μια υπηρεσία στο cloud](cloud-services-how-to-create-deploy.md).
* Ρύθμιση παραμέτρων ένα [προσαρμοσμένο όνομα τομέα](cloud-services-custom-domain-name.md).
* Ρύθμιση παραμέτρων [πιστοποιητικά ssl](cloud-services-configure-ssl-certificate.md).