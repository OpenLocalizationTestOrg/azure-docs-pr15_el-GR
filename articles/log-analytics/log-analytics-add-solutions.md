<properties
    pageTitle="Προσθήκη λύσεις καταγραφής αναλυτικών στοιχείων από τη συλλογή λύσεων | Microsoft Azure"
    description="Αρχείο καταγραφής ανάλυσης λύσεις είναι μια συλλογή λογικής, απεικόνιση και απόκτηση δεδομένων κανόνες που παρέχουν μετρικά περιστρεφόμενη γύρω από μια περιοχή συγκεκριμένου προβλήματος."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Προσθήκη λύσεις καταγραφής αναλυτικών στοιχείων από τη συλλογή λύσεων

Λύσεις αναλυτικών στοιχείων καταγραφής είναι μια συλλογή **λογικής**, **απεικόνιση** και **κανόνες acquisition δεδομένων** που παρέχουν μετρικά περιστρεφόμενη γύρω από μια περιοχή συγκεκριμένου προβλήματος. Σε αυτό το άρθρο λύσεις λίστες υποστηρίζονται καταγραφής ανάλυσης και θα δείτε πώς μπορείτε να προσθέσετε και να τα καταργήσετε χρησιμοποιώντας τη συλλογή λύσεων.

Λύσεις επιτρέψετε βαθύτερη ιδέες για να:

- την διερεύνηση και επίλυση προβλημάτων λειτουργικές πιο γρήγορα
- συλλογή και ο συσχετισμός διάφορους τύπους δεδομένων υπολογιστή
- σάς βοηθούν να έγκαιρη με δραστηριότητες όπως χωρητικότητας, αναφοράς κατάστασης ενημέρωσης κώδικα και ελέγχου ασφαλείας.


>[AZURE.NOTE] Ανάλυση καταγραφής περιλαμβάνει λειτουργικότητα αναζήτησης καταγραφής, έτσι δεν χρειάζεται να εγκαταστήσετε μια λύση για να την ενεργοποιήσετε. Ωστόσο, μπορείτε να λάβετε απεικονίσεις δεδομένων, προτεινόμενες αναζητήσεις και τις ιδέες, προσθέτοντας λύσεις από τη συλλογή λύσεων.

Αφού προσθέσετε μια λύση, τα δεδομένα που συλλέγονται από τους διακομιστές της υποδομής σας και αποστολή στην υπηρεσία OMS. Επεξεργασία από το OMS υπηρεσίας διαρκεί συνήθως λίγα λεπτά σε ώρες. Μετά την υπηρεσία επεξεργάζεται τα δεδομένα, μπορείτε να το προβάλετε σε OMS.

Μπορείτε εύκολα να καταργήσετε μια λύση όταν δεν είναι πλέον απαραίτητη. Όταν καταργείτε μια λύση, τα δεδομένα δεν αποστέλλονται στη OMS, η οποία να μειώσετε την ποσότητα των δεδομένων που χρησιμοποιούνται από το όριο του ημερήσιου, εάν έχετε ένα.


## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>Λύσεις που υποστηρίζεται από τον παράγοντα παρακολούθησης της Microsoft

Προς το παρόν, τους διακομιστές που είναι συνδεδεμένοι στο OMS χρησιμοποιώντας το Microsoft Agent παρακολούθηση να χρησιμοποιήσετε τις περισσότερες από τις λύσεις που είναι διαθέσιμες, συμπεριλαμβανομένων των:

- Αξιολόγηση της υπηρεσίας καταλόγου Active Directory
- Ειδοποίηση διαχείρισης (χωρίς SCOM ειδοποιήσεις)
- Λογισμικό κακόβουλης λειτουργίας
- Παρακολούθηση αλλαγών
- Ασφάλεια
- Αξιολόγηση SQL
- Ενημερώσεις συστήματος

Ωστόσο, τις παρακάτω λύσεις είναι *δεν* υποστηρίζονται με τον αντιπρόσωπο της Microsoft παρακολούθησης και απαιτούν παράγοντας συστήματος κέντρο Operations Manager (SCOM).

- Ειδοποίηση διαχείρισης (συμπεριλαμβανομένων των ειδοποιήσεων SCOM)
- Δυναμικότητας διαχείρισης
- Αξιολόγηση ρύθμισης παραμέτρων

Ανατρέξτε [Τη σύνδεση Operations Manager για ανάλυση καταγραφής](log-analytics-om-agents.md) για πληροφορίες σχετικά με τη σύνδεση τον παράγοντα SCOM στο αρχείο καταγραφής ανάλυσης.

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>Για να προσθέσετε μια λύση, χρησιμοποιώντας τη συλλογή λύσεων

1. Στη σελίδα Επισκόπηση στην OMS, κάντε κλικ στο πλακίδιο **Συλλογή λύσεων** .    
    ![συλλογή λύσεων](./media/log-analytics-add-solutions/sol-gallery.png)
2. Στη σελίδα συλλογή λύσεων OMS, μάθετε σχετικά με όλες τις διαθέσιμες λύσεις. Κάντε κλικ στο όνομα της λύσης που θέλετε να προσθέσετε OMS.
3. Στη σελίδα για τη λύση που επιλέξατε, εμφανίζεται λεπτομερείς πληροφορίες σχετικά με τη λύση. Κάντε κλικ στην επιλογή **Προσθήκη**.
4. Ένα νέο πλακίδιο για τη λύση που προσθέσατε εμφανίζεται την επισκόπηση σελίδας στο OMS και μπορείτε να αρχίσετε να το χρησιμοποιείτε μετά την υπηρεσία OMS επεξεργάζεται τα δεδομένα σας.

## <a name="to-configure-solutions"></a>Για να ρυθμίσετε τις παραμέτρους λύσεων
1. Θα χρειαστεί να ρυθμίσετε τις παραμέτρους ορισμένες λύσεις. Για παράδειγμα, θα πρέπει να ρυθμίσετε τις παραμέτρους Αυτοματισμός, Azure Αποκατάσταση τοποθεσιών και δημιουργία αντιγράφων ασφαλείας πριν μπορείτε να τις χρησιμοποιήσετε.
2. Για οποιαδήποτε από αυτές τις λύσεις, επιλέξτε το πλακίδιο στη σελίδα Επισκόπηση.  
    ![ρύθμιση παραμέτρων λύσεων](./media/log-analytics-add-solutions/configure-additional.png)
3. Στη συνέχεια, ρύθμιση παραμέτρων της λύσης με τις απαραίτητες πληροφορίες και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.  
    ![ρύθμιση παραμέτρων λύσεων](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>Για να καταργήσετε μια λύση, χρησιμοποιώντας τη συλλογή λύσεων

1. Στη σελίδα Επισκόπηση στην OMS, κάντε κλικ στο πλακίδιο **Ρυθμίσεις** .
2. Στη σελίδα "Ρυθμίσεις", στην καρτέλα λύσεις, κάντε κλικ στην επιλογή **Κατάργηση** για τη λύση που θέλετε να καταργήσετε.
3. Στο παράθυρο διαλόγου επιβεβαίωσης, κάντε κλικ στο κουμπί **Ναι** για να καταργήσετε τη λύση.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>Λεπτομέρειες συλλογής δεδομένων για OMS δυνατότητες και λύσεις

Ο παρακάτω πίνακας εμφανίζει μεθόδους συλλογής δεδομένων και άλλες λεπτομέρειες σχετικά με τον τρόπο που συλλέγονται δεδομένα για OMS δυνατότητες και λύσεις. Άμεση παράγοντες και SCOM παράγοντες είναι ουσιαστικά ίδια, ωστόσο τον παράγοντα απευθείας περιλαμβάνει πρόσθετες λειτουργίες για να μπορεί να συνδεθεί στο χώρο εργασίας OMS και δρομολόγηση μέσω ενός διακομιστή μεσολάβησης. Εάν χρησιμοποιείτε έναν παράγοντα SCOM, πρέπει να είναι στοχευμένες ως παράγοντας OMS για να επικοινωνήσετε με OMS. Παράγοντες SCOM σε αυτόν τον πίνακα, οι OMS παράγοντες που είναι συνδεδεμένοι στο SCOM. Για πληροφορίες σχετικά με τη σύνδεση υπάρχον περιβάλλον SCOM OMS, ανατρέξτε στο θέμα [Σύνδεση Operations Manager για ανάλυση καταγραφής](log-analytics-om-agents.md) .

>[AZURE.NOTE] Ο τύπος του παράγοντα που χρησιμοποιείτε καθορίζει τον τρόπο δεδομένα αποστέλλονται OMS, με τις ακόλουθες συνθήκες:

- Μπορείτε είτε να χρησιμοποιήσετε τον παράγοντα απευθείας ή έναν παράγοντα OMS SCOM συνημμένα.
- Όταν απαιτείται SCOM, SCOM παράγοντας δεδομένων για τη λύση πάντα αποστέλλεται OMS χρήση της ομάδας διαχείρισης SCOM. Επιπλέον, όταν απαιτείται SCOM, μόνο ο παράγοντας SCOM χρησιμοποιείται από τη λύση.
- Όταν δεν απαιτείται SCOM και ο πίνακας εμφανίζει δεδομένα παράγοντας SCOM αποστολής OMS χρήση της ομάδας διαχείρισης, στη συνέχεια, SCOM παράγοντας δεδομένων πάντα αποστέλλεται OMS χρησιμοποιώντας τις ομάδες διαχείρισης. Άμεση παράγοντες παράκαμψη της ομάδας διαχείρισης και στείλτε τα δεδομένα απευθείας στο OMS.
- Όταν τα δεδομένα παράγοντας SCOM δεν αποστέλλονται χρησιμοποιώντας μια ομάδα διαχείρισης, στη συνέχεια, τα δεδομένα αποστέλλονται απευθείας στο OMS — παρακάμπτοντας την ομάδα διαχείρισης.


|Τύπος δεδομένων| πλατφόρμα | Άμεση παράγοντα | Παράγοντας SCOM | Azure χώρου αποθήκευσης | SCOM απαιτείται; | SCOM παράγοντας δεδομένων που αποστέλλονται μέσω ομάδας διαχείρισης | συχνότητα συλλογής |
|---|---|---|---|---|---|---|---|
|Αξιολόγηση AD|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|  7 ημέρες|
|Κατάσταση αναπαραγωγής AD|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 ημερών|
|Ειδοποιήσεις (Nagios)|Linux|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|κατά την άφιξή τους|
|Ειδοποιήσεις (Zabbix)|Linux|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|1 λεπτό|
|Ειδοποιήσεις (Operations Manager)|Windows|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 λεπτά|
|Λογισμικό κακόβουλης λειτουργίας|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)| ανά ώρα|
|Δυναμικότητας διαχείρισης|Windows|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)| ανά ώρα|
|Παρακολούθηση αλλαγών|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)| ανά ώρα|
|Παρακολούθηση αλλαγών|Linux|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|ανά ώρα|
|Ρύθμιση παραμέτρων αξιολόγησης (παλαιού τύπου Σύμβουλος)|Windows|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)| δύο φορές την ημέρα|
|ETW|Windows|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 λεπτά|
|Αρχεία καταγραφής των υπηρεσιών IIS|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 λεπτά|
|Χώροι φύλαξης αριθμού-κλειδιού|Windows|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 λεπτά|
|Πύλες εφαρμογή δικτύου|Windows|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 λεπτά|
|Ομάδες ασφαλείας δικτύου|Windows|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 λεπτά|
|Office 365|Windows|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|ειδοποίηση|
|Μετρητές επιδόσεων|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|σύμφωνα με το χρονοδιάγραμμα, τουλάχιστον 10 δευτερόλεπτα|
|Μετρητές επιδόσεων|Linux|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|σύμφωνα με το χρονοδιάγραμμα, τουλάχιστον 10 δευτερόλεπτα|
|Υπηρεσία ύφασμα|Windows|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 λεπτά|
|Αξιολόγηση SQL|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)| 7 ημέρες|
|SurfaceHub|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|κατά την άφιξή τους|
|SYSLOG|Linux|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|από το Azure χώρου αποθήκευσης: 10 λεπτά. από παράγοντα: κατά την άφιξή τους|
|Ενημερώσεις συστήματος|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)| τουλάχιστον 2 ώρες ανά ημέρα και 15 λεπτά μετά την εγκατάσταση μιας ενημέρωσης|
|Αρχεία καταγραφής συμβάντων ασφαλείας των Windows|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)| για αποθήκευση Azure: 10 min; για τον παράγοντα: κατά την άφιξή τους|
|Αρχεία καταγραφής τείχους προστασίας των Windows|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)| κατά την άφιξή τους|
|Αρχεία καταγραφής συμβάντων των Windows|Windows|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)| για αποθήκευση Azure: 1 min; για τον παράγοντα: κατά την άφιξή τους|
|Γραμμικό δεδομένων|Windows (2012 R2 / 8.1 ή νεότερη έκδοση)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ναι](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Όχι](./media/log-analytics-add-solutions/oms-bullet-red.png)| κάθε 1 λεπτό|

## <a name="log-analytics-preview-solutions-and-features"></a>Αρχείο καταγραφής ανάλυσης προεπισκόπηση λύσεις και δυνατότητες

Εκτελεί μια υπηρεσία και ακολουθώντας πρακτικές devops συνεχίζουμε να μπορούν να συνεργάζονται με τους πελάτες για την ανάπτυξη δυνατότητες και λύσεις.

Κατά την προεπισκόπηση ιδιωτικό θα σας να δώσετε μια μικρή ομάδα πελάτες πρόσβαση σε μια πρώιμη υλοποίηση της δυνατότητας ή λύση για να αποκτήσετε σχολίων και βελτιώσεις. Αυτή η πρώιμη υλοποίηση διαθέτει ελάχιστους λειτουργικές δυνατότητες.

Στόχος μας είναι να δοκιμάσετε πράγματα γρήγορα, ώστε να μπορούμε να βρούμε τι λειτουργεί και τι δεν λειτουργεί. Θα σας επαναλάβετε αυτήν τη διαδικασία έως ότου τα σχόλια από τους πελάτες ιδιωτικό προεπισκόπηση μας πληροφορεί ότι είστε έτοιμοι για μια δημόσια προεπισκόπηση.

Κατά τη διάρκεια του public preview, κάνουμε τη δυνατότητα ή λύση διαθέσιμη για όλους τους χρήστες για να λάβετε περισσότερα σχόλια και να επικυρώσετε μας κλίμακας και της αποδοτικότητας. Σε αυτήν τη φάση:

- Προεπισκόπηση δυνατότητες θα εμφανίζονται στην καρτέλα ρυθμίσεις και μπορεί να ενεργοποιηθεί από οποιονδήποτε χρήστη
- Προεπισκόπηση λύσεις μπορούν να προστεθούν μέσω της συλλογής ή τη χρήση μιας δημοσιευμένης δέσμης ενεργειών

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Τι πρέπει να γνωρίζω σχετικά με την προεπισκόπηση δυνατότητες και λύσεις;

Χαρήκαμε σχετικά με τις νέες δυνατότητες και λύσεις και θα σας αρέσει με την εργασία με να αναπτύξετε τους.

Προεπισκόπηση δυνατότητες και λύσεις δεν είναι κατάλληλο για όλους τους χρήστες μέσω, επομένως πριν να ζητήσετε να συμμετάσχετε σε μια ιδιωτική προεπισκόπηση ή ενεργοποίηση μια δημόσια προεπισκόπηση βεβαιωθείτε ότι OK εργάζεστε με κάτι που βρίσκεται κάτω από την ανάπτυξη.

Κατά την ενεργοποίηση της δυνατότητας μια προεπισκόπηση μέσω της πύλης θα εμφανίζεται μια προειδοποίηση που σας υπενθυμίζει ότι η δυνατότητα είναι στην προεπισκόπηση.

#### <a name="for-both-private-and-public-preview"></a>Για προεπισκόπηση τόσο *ιδιωτικά* και *δημόσια*

Τα ακόλουθα ισχύουν για δημόσιες και ιδιωτικές προεπισκοπήσεις:

- Πράγματα που πρέπει να μην πάντα λειτουργούν σωστά.
  - Περιοχή θέματα από μια μικρή ενόχληση μέσω σε κάποιο στοιχείο δεν λειτουργεί καθόλου
- Υπάρχει πιθανότητα για την προεπισκόπηση για να έχουν αρνητικές επιπτώσεις σε σας συστήματα / περιβάλλον
  - Προσπαθούμε να αποφύγετε την αρνητική πράγματα συμβαίνει με τα συστήματα που χρησιμοποιείτε με OMS, αλλά μερικές φορές μη αναμενόμενο πράγματα συμβούν
- Απώλεια δεδομένων / ενδέχεται να προκύψει καταστροφή
- Θα σας μπορεί να σας ζητήσει να συλλέξετε αρχεία καταγραφής διαγνωστικών ή άλλα δεδομένα για να βοηθήσετε στην αντιμετώπιση προβλημάτων
- Η δυνατότητα ή λύση ενδέχεται να καταργηθούν (προσωρινά ή μόνιμα)
  - Βάση μας learnings κατά τη διάρκεια της προεπισκόπησης που θα σας μπορεί να αποφασίσει να μην Αφήστε τη δυνατότητα ή λύση
- Προεπισκοπήσεις ενδέχεται να μην λειτουργεί ή μπορεί να μην έχουν ελεγχθεί με όλες τις παραμέτρους και θα σας μπορεί να περιορίζει:
  - Τα λειτουργικά συστήματα που μπορούν να χρησιμοποιηθούν (π.χ. μια δυνατότητα ενδέχεται να ισχύουν μόνο για Linux ενώ βρίσκεστε σε προεπισκόπηση)
  - Ο τύπος του παράγοντα (MMA, SCOM) που μπορούν να χρησιμοποιηθούν (π.χ. μια δυνατότητα ενδέχεται να μην λειτουργούν με SCOM ενώ βρίσκεστε σε προεπισκόπηση)  
- Δυνατότητες και λύσεις Preview δεν εμπίπτουν στο επίπεδο σύμβαση παροχής υπηρεσιών
- Χρήση των δυνατοτήτων προεπισκόπησης θα χρεωθείτε χρήση
- Δυνατότητες που χρειάζεστε για τη δυνατότητα / λύση για να είναι χρήσιμες ενδέχεται να λείπουν ή είναι ελλιπείς ή δυνατότητες
- Δυνατότητες / λύσεις ενδέχεται να μην είναι διαθέσιμες σε όλες τις περιοχές
- Δυνατότητες / λύσεις μπορεί να μην μεταφραστεί
- Δυνατότητες / λύσεις ενδέχεται να έχει ένα όριο στον αριθμό των πελατών ή συσκευές που μπορούν να το χρησιμοποιήσετε
- Ίσως χρειαστεί να χρησιμοποιήσετε δέσμες ενεργειών για την εκτέλεση ρύθμισης παραμέτρων και να ενεργοποιήσετε τη λύση/δυνατότητα
- Το περιβάλλον εργασίας χρήστη (UI), δεν θα ολοκληρωθεί και μπορεί να αλλάξει από ημέρα για την ημέρα
- Δημόσια προεπισκοπήσεις ενδέχεται να μην είναι κατάλληλο για την παραγωγή / κρίσιμες συστήματα

#### <a name="for-private-preview"></a>Για προεπισκόπηση *ιδιωτική*

Εκτός από τα παραπάνω στοιχεία, ακολουθεί ειδικά για προεπισκοπήσεις ιδιωτικό:

- Αναμένεται να δώσετε μας με σχόλια για την εμπειρία σας, έτσι ώστε να κάνουμε τη δυνατότητα/λύση καλύτερη
- Ενδέχεται να επικοινωνήσουμε μαζί σας για σχόλια χρήσης έρευνες, τηλεφωνικές κλήσεις ή ηλεκτρονικού ταχυδρομείου
- Πράγματα που πρέπει πάντα δεν θα λειτουργεί σωστά
- Θα σας μπορεί να απαιτεί μια συμφωνία μη αποκάλυψης (NDA) για τη συμμετοχή ή ενδέχεται να περιλαμβάνουν εμπιστευτικές περιεχομένου
  - Πριν από την δημιουργία ιστολογίου, tweeting ή αλλιώς κοινοποίηση σε τρίτους, ελέγξτε με τη Διαχείριση προγράμματος υπεύθυνοι για την προεπισκόπηση για να κατανοήσετε τους όποιους περιορισμούς υπάρχουν κοινοποίηση
- Δεν λειτουργούν παραγωγής / κρίσιμες συστήματα


### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Πώς μπορώ να αποκτήσω πρόσβαση σε ιδιωτικά προεπισκόπηση δυνατότητες και λύσεις;

Σας προσκαλούμε στους πελάτες να ιδιωτικό προεπισκοπήσεις έως πολλούς διαφορετικούς τρόπους, ανάλογα με την προεπισκόπηση.

- Απάντηση στην έρευνα μηνιαία πελατών και μας εκχωρεί δικαιώματα για εσάς βελτιώνει τις πιθανότητες που έχουν προσκληθεί σε μια ιδιωτική προεπισκόπηση.
- Ομάδα το λογαριασμό σας Microsoft να χαρακτηρίζουν που.
- Μπορείτε να εγγραφείτε που βασίζονται σε λεπτομέρειες δημοσιεύσει στο twitter [msopsmgmt](https://twitter.com/msopsmgmt)
- Μπορείτε να εγγραφείτε με βάση τα συμβάντα κοινόχρηστο Κοινότητας λεπτομέρειες – δείτε για μας άμεσης ups, διασκέψεις και ηλεκτρονικές κοινότητες.


## <a name="next-steps"></a>Επόμενα βήματα

- [Αρχεία καταγραφής από την αναζήτηση](log-analytics-log-searches.md) για να προβάλετε λεπτομερείς πληροφορίες που συλλέγονται από λύσεις.
