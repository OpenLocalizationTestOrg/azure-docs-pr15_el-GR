<properties 
   pageTitle="Δοκιμές τις επιδόσεις της μια υπηρεσία cloud | Microsoft Azure"
   description="Ελέγξτε την απόδοση του μια υπηρεσία στο cloud χρησιμοποιώντας το profiler Visual Studio"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="testing-the-performance-of-a-cloud-service"></a>Δοκιμές τις επιδόσεις της υπηρεσίας cloud 

##<a name="overview"></a>Επισκόπηση

Μπορείτε να ελέγξετε τις επιδόσεις της μια υπηρεσία cloud με τους εξής τρόπους:

- Χρησιμοποιήστε Διαγνωστικά του Azure για τη συλλογή πληροφοριών σχετικά με τις αιτήσεις και συνδέσεις και για να δείτε τα στατιστικά στοιχεία τοποθεσίας που δείχνουν τον τρόπο που πραγματοποιεί της υπηρεσίας από μια προοπτική πελατών. Για να ξεκινήσετε, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων διαγνωστικών για τις υπηρεσίες Cloud Azure και εικονικές μηχανές]( http://go.microsoft.com/fwlink/p/?LinkId=623009).

- Χρησιμοποιήστε το Visual Studio profiler για να λάβετε μια εις βάθος ανάλυση των υπολογιστική πτυχών του τρόπου εκτέλεσης της υπηρεσίας. Καθώς αυτό το θέμα περιγράφει, μπορείτε να χρησιμοποιήσετε το profiler να μετρήσετε επιδόσεων, όπως μια υπηρεσία που εκτελείται στο Azure. Για πληροφορίες σχετικά με τον τρόπο χρήσης του profiler για μέτρηση της απόδοσης με μια υπηρεσία εκτελείται τοπικά στο ένα προσομοίωσης υπολογισμού, ανατρέξτε στο θέμα [Έλεγχος τις επιδόσεις της μια Azure Cloud υπηρεσίας τοπικά την υπολογιστική προσομοίωσης χρησιμοποιώντας το Visual Studio Profiler](http://go.microsoft.com/fwlink/p/?LinkId=262845).



## <a name="choosing-a-performance-testing-method"></a>Επιλογή ενός επιδόσεων μέθοδο δοκιμής

###<a name="use-azure-diagnostics-to-collect"></a>Χρησιμοποιήστε Διαγνωστικά του Azure για τη συλλογή:###

- Στατιστικά στοιχεία σε σελίδες web ή υπηρεσίες, όπως αιτήσεις και συνδέσεις.

- Στατιστικά στοιχεία σχετικά με τους ρόλους, όπως πόσο συχνά γίνει επανεκκίνηση του ρόλου.

- Συνολική πληροφορίες σχετικά με τη χρήση μνήμης, όπως το ποσοστό του χρόνου που τίθεται σε της συλλογής απορριμμάτων ή η μνήμη συνόλου εκτελείται ρόλου.

###<a name="use-the-visual-studio-profiler-to"></a>Χρησιμοποιήστε το profiler Visual Studio για:###

- Καθορισμός που λειτουργεί χρειαστεί περισσότερο χρόνο.

- Μετρήστε πόσος χρόνος χρειάζεται κάθε μέρος ενός προγράμματος υπολογιστικά εντατική.

- Συγκρίνετε απόδοση λεπτομερείς αναφορές για τις δύο εκδόσεις μιας υπηρεσίας.

- Ανάλυση εκχώρηση μνήμης με περισσότερες λεπτομέρειες από το επίπεδο του εκχωρήσεις μεμονωμένα μνήμης.

- Ανάλυση προβλήματα συγχρονισμού στο πολυνηματική κώδικα.

Όταν χρησιμοποιείτε το profiler, μπορείτε να συλλέξετε δεδομένα όταν εκτελείται μια υπηρεσία cloud τοπικά ή στο Azure.

###<a name="collect-profiling-data-locally-to"></a>Συλλογή προφίλ δεδομένων τοπικά για να:###

- Ελέγξτε την απόδοση του τμήματος μια υπηρεσία cloud, όπως η εκτέλεση του ρόλου συγκεκριμένες εργασίας, που δεν απαιτεί μια ρεαλιστική προσομοίωση φόρτωση.

- Ελέγξτε την απόδοση του μια υπηρεσία cloud μεμονωμένα, υπό όρους που ελέγχεται.

- Ελέγξτε την απόδοση του μια υπηρεσία cloud πριν από την ανάπτυξή του για Azure.

- Ελέγξτε την απόδοση του μια υπηρεσία cloud ιδιωτικά, χωρίς να επηρεαστεί η υπάρχουσα αναπτύξεις.

- Ελέγξτε τις επιδόσεις της υπηρεσίας χωρίς τις χρεώσεις για την εκτέλεση στο Azure.

###<a name="collect-profiling-data-in-azure-to"></a>Συλλογή δεδομένων προφίλ στο Azure για να:###

- Ελέγξτε την απόδοση του μια υπηρεσία cloud στην περιοχή μια πρόχειρη είτε πραγματικό φόρτωση.

- Χρησιμοποιήστε τη μέθοδο οργάνων της συλλογής δεδομένων προφίλ, καθώς αυτό το θέμα περιγράφει αργότερα.

- Ελέγξτε τις επιδόσεις της υπηρεσίας στο ίδιο περιβάλλον ως όταν εκτελείται η υπηρεσία στο παραγωγής.

Συνήθως, μπορείτε να προσομοιώσετε μια φόρτωση για τις υπηρεσίες cloud δοκιμής στην περιοχή κανονική ή φόρτο συνθήκες.

## <a name="profiling-a-cloud-service-in-azure"></a>Δημιουργία προφίλ μια υπηρεσία στο cloud στο Azure

Όταν δημοσιεύετε την υπηρεσία cloud από το Visual Studio, μπορείτε να προφίλ της υπηρεσίας και να καθορίσετε τις ρυθμίσεις προφίλ που θα σας δώσει τις πληροφορίες που θέλετε. Για κάθε παρουσία του ρόλου ξεκινήσει μια περίοδο λειτουργίας δημιουργίας προφίλ. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να δημοσιεύσετε την υπηρεσία από το Visual Studio, ανατρέξτε στο θέμα [δημοσίευσης σε μια υπηρεσία Cloud Azure από το Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx).

Για να μάθετε περισσότερα σχετικά με τις επιδόσεις Δημιουργία προφίλ στο Visual Studio, ανατρέξτε στο θέμα [Οδηγός για αρχάριους για τη δημιουργία προφίλ επιδόσεων](https://msdn.microsoft.com/library/azure/ms182372.aspx) και [Την ανάλυση απόδοσης εφαρμογών με χρήση εργαλεία δημιουργία προφίλ](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

>[AZURE.NOTE] Μπορείτε να ενεργοποιήσετε IntelliTrace ή δημιουργία προφίλ όταν δημοσιεύετε την υπηρεσία cloud. Δεν μπορείτε να ενεργοποιήσετε και τα δύο.

###<a name="profiler-collection-methods"></a>Μέθοδοι συλλογής Profiler

Μπορείτε να χρησιμοποιήσετε διαφορετική συλλογή μεθόδους για τη δημιουργία προφίλ, με βάση τα θέματα απόδοσης σας:

- **Δειγματοληψία CPU** - αυτή η μέθοδος συλλέγει στατιστικά στοιχεία εφαρμογών που είναι χρήσιμες για την αρχική ανάλυση των προβλημάτων χρήση CPU. Δειγματοληψία CPU είναι η προτεινόμενη μέθοδος για την εκκίνηση περισσότερες έρευνες επιδόσεων. Υπάρχει μια χαμηλή επίδραση σχετικά με την εφαρμογή που δημιουργία προφίλ όταν συλλέγετε δεδομένα δειγματοληψία CPU.

- **Οργάνων** -αυτή η μέθοδος συλλέγει χρονισμού λεπτομερή δεδομένα που είναι χρήσιμη για εστιασμένου ανάλυσης και για την ανάλυση θέματα απόδοσης εισόδου/εξόδου. Η μέθοδος οργάνων εγγραφών κάθε εγγραφή, Έξοδος και συνάρτηση κλήση από τις συναρτήσεις σε μια λειτουργική μονάδα κατά την εκτέλεση ενός προφίλ. Αυτή η μέθοδος είναι χρήσιμη για τη συγκέντρωση χρονισμού λεπτομερείς πληροφορίες σχετικά με μια ενότητα του κώδικα και για να κατανοήσετε την επίδραση των λειτουργιών εισόδου και εξόδου στο επιδόσεις εφαρμογής. Αυτή η μέθοδος είναι απενεργοποιημένη για έναν υπολογιστή που εκτελεί λειτουργικό σύστημα 32-bit. Αυτή η επιλογή είναι διαθέσιμη μόνο όταν εκτελείτε την υπηρεσία cloud στο Azure, δεν τοπικά στο προσομοίωσης του υπολογισμού.

- **Εκχώρηση μνήμης .NET** - αυτή η μέθοδος συλλέγει δεδομένα εκχώρηση μνήμης .NET Framework, χρησιμοποιώντας τη μέθοδο τη δημιουργία προφίλ δειγματοληψία. Τα δεδομένα που έχουν συλλεχθεί περιλαμβάνει τον αριθμό και το μέγεθος των αντικειμένων που έχει εκχωρηθεί.

- **Ταυτόχρονης εκτέλεσης** - αυτή η μέθοδος συλλέγει δεδομένα διένεξης πόρων και διαδικασία νήμα εκτέλεσης δεδομένα που είναι χρήσιμη για την ανάλυση πολυνηματικού και πολλαπλών διαδικασιών στις εφαρμογές. Η μέθοδος ταυτόχρονης εκτέλεσης συλλέγει δεδομένα για κάθε συμβάν που μπλοκ εκτέλεσης του κωδικού σας, όπως όταν ένα νήμα περιμένει κλειδώσει την πρόσβαση σε έναν πόρο της εφαρμογής για να ελευθερωθεί. Αυτή η μέθοδος είναι χρήσιμη για την ανάλυση πολυνηματικού εφαρμογές.

- Μπορείτε επίσης να ενεργοποιήσετε τη **Δημιουργία προφίλ για το επίπεδο επικοινωνίας**, που παρέχει πρόσθετες πληροφορίες σχετικά με τις ώρες εκτέλεσης της συγχρονισμένης ADO.NET κλήσεις σε συναρτήσεις πολλών επιπέδων εφαρμογών που επικοινωνούν με μία ή περισσότερες βάσεις δεδομένων. Μπορείτε να συλλέξετε δεδομένων επιπέδων αλληλεπίδραση με οποιαδήποτε από τις μεθόδους δημιουργίας προφίλ. Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προφίλ για το επίπεδο επικοινωνίας, ανατρέξτε στο θέμα [Προβολή αλληλεπιδράσεις επίπεδο](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Ρύθμιση παραμέτρων δημιουργίας προφίλ

Η παρακάτω εικόνα δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους του προφίλ από το παράθυρο διαλόγου δημοσίευση εφαρμογής Azure.

![Ρύθμιση παραμέτρων δημιουργίας προφίλ ρυθμίσεων](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

>[AZURE.NOTE] Για να ενεργοποιήσετε το πλαίσιο ελέγχου **Ενεργοποίηση δημιουργίας προφίλ** , πρέπει να έχετε το profiler στον τοπικό υπολογιστή που χρησιμοποιείτε για να δημοσιεύσετε την υπηρεσία cloud. Από προεπιλογή, η δημιουργία προφίλ έχει εγκατασταθεί κατά την εγκατάσταση του Visual Studio.

### <a name="to-configure-profiling-settings"></a>Για να ρυθμίσετε τις παραμέτρους προφίλ

1. Στην Εξερεύνηση λύσεων, ανοίξτε το μενού συντόμευσης για το έργο σας Azure και, στη συνέχεια, επιλέξτε το στοιχείο **Δημοσίευση**. Για λεπτομερείς οδηγίες σχετικά με τον τρόπο για να δημοσιεύσετε μια υπηρεσία cloud, ανατρέξτε στο θέμα [Δημοσίευση ένα σύννεφο υπηρεσίας χρησιμοποιώντας τα εργαλεία Azure](http://go.microsoft.com/fwlink/p?LinkId=623012).

1. Στο παράθυρο διαλόγου **Δημοσίευση εφαρμογής Azure** , επιλέξετε την καρτέλα **Ρυθμίσεις για προχωρημένους** .

1. Για να ενεργοποιήσετε τη δημιουργία προφίλ, επιλέξτε το πλαίσιο ελέγχου **Ενεργοποίηση δημιουργία προφίλ** .

1. Για να ρυθμίσετε τις παραμέτρους των ρυθμίσεων δημιουργίας προφίλ, επιλέξτε την υπερ-σύνδεση **Ρυθμίσεις** . Εμφανίζεται το παράθυρο διαλόγου Ρυθμίσεις προφίλ.

1. Από τα κουμπιά επιλογής **ποια μέθοδο δημιουργίας προφίλ που θα θέλατε να χρησιμοποιήσετε** , επιλέξτε τον τύπο του προφίλ που χρειάζεστε.

1. Για τη συλλογή δεδομένων προφίλ αλληλεπίδραση επίπεδο, επιλέξτε το πλαίσιο ελέγχου **Ενεργοποίηση επίπεδο επικοινωνίας δημιουργία προφίλ** .

1. Για να αποθηκεύσετε τις ρυθμίσεις, επιλέξτε το κουμπί **OK** .

    Όταν δημοσιεύετε αυτής της εφαρμογής, αυτές οι ρυθμίσεις χρησιμοποιούνται για τη δημιουργία της περιόδου λειτουργίας δημιουργίας προφίλ για κάθε ρόλο.

## <a name="viewing-profiling-reports"></a>Προβολή αναφορών τη δημιουργία προφίλ

Για κάθε παρουσία του ρόλου στην υπηρεσία cloud, δημιουργείται μια περίοδο λειτουργίας δημιουργίας προφίλ. Για να προβάλετε τις αναφορές του προφίλ κάθε περιόδου λειτουργίας από το Visual Studio, μπορείτε να προβάλετε το παράθυρο της Εξερεύνησης διακομιστή και να, στη συνέχεια, επιλέξτε τον κόμβο τον υπολογισμό Azure για να επιλέξετε μια παρουσία του ρόλου. Μπορείτε να προβάλετε την αναφορά προφίλ, στη συνέχεια, όπως φαίνεται στην παρακάτω εικόνα.

![Προβολή προφίλ αναφοράς από το Azure](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Για να προβάλετε αναφορές προφίλ

1. Για να προβάλετε το παράθυρο της Εξερεύνησης διακομιστή στο Visual Studio, στη γραμμή μενού επιλέξτε Προβολή, με Εξερεύνηση Server.

1. Επιλέξτε τον κόμβο τον υπολογισμό Azure και, στη συνέχεια, επιλέξτε τον κόμβο Azure ανάπτυξης για την υπηρεσία cloud που επιλέξατε στο προφίλ κατά τη δημοσίευση από το Visual Studio.

1. Για να προβάλετε το προφίλ αναφορών για μια παρουσία, επιλέξτε το ρόλο στην υπηρεσία, άνοιγμα του μενού συντόμευσης για μια συγκεκριμένη παρουσία και, στη συνέχεια, επιλέξτε **Προβολή αναφοράς δημιουργία προφίλ**.

    Η αναφορά, ένα αρχείο .vsp, γίνεται λήψη τώρα από το Azure και την κατάσταση της λήψης εμφανίζεται στο αρχείο καταγραφής της δραστηριότητας Azure. Όταν ολοκληρωθεί η λήψη, η αναφορά του προφίλ εμφανίζεται σε μια καρτέλα στο πρόγραμμα επεξεργασίας για το Visual Studio, που ονομάζεται <Role name> _<Instance Number>_ <identifier>.vsp. Εμφανίζεται το παράθυρο συνοπτικά δεδομένα για την αναφορά.

1. Για να εμφανίσετε διαφορετικές προβολές της έκθεσης, στη λίστα τρέχουσα προβολή, επιλέξτε τον τύπο της προβολής που θέλετε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία προφίλ προβολές εργαλεία αναφοράς](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Επόμενα βήματα

[Υπηρεσίες Cloud εντοπισμού σφαλμάτων](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Δημοσίευση σε μια υπηρεσία Azure Cloud από το Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)
