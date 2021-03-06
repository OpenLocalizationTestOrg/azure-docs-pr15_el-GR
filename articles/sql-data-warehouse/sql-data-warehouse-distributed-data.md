<properties
   pageTitle="Κατανεμημένων δεδομένων και διανέμεται πίνακα επιλογών για τα συστήματα μαζικά παράλληλες επεξεργασίας (MPP) των αποθήκη δεδομένων του SQL και Parallel Data Warehouse | Microsoft Azure"
   description="Μάθετε πώς κατανέμεται δεδομένων για μαζικά παράλληλες επεξεργασίας (MPP) και τις επιλογές για τη διανομή πίνακες στην αποθήκη δεδομένων του SQL Azure και παράλληλη αποθήκη δεδομένων."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="barbkess"/>


# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Κατανεμημένη δεδομένων και κατανέμεται πινάκων για μαζικά παράλληλες επεξεργασίας (MPP)

Μάθετε πώς τα δεδομένα των χρηστών κατανέμεται στο αποθήκη δεδομένων του SQL Azure και παράλληλη αποθήκη δεδομένων, οι οποίες είναι συστήματα μαζικά παράλληλες επεξεργασίας (MPP) της Microsoft. Σχεδίαση σας αποθήκη δεδομένων για να χρησιμοποιήσετε αποτελεσματικά κατανεμημένων δεδομένων σάς βοηθά να επιτύχετε το ερώτημα επεξεργασίας πλεονεκτήματα της αρχιτεκτονικής MPP. Μερικές επιλογές σχεδίασης βάσης δεδομένων μπορεί να έχει σημαντική επίδραση στο τη βελτίωση της απόδοσης του ερωτήματος.  

>[AZURE.NOTE] Azure αποθήκη δεδομένων του SQL και Parallel Data Warehouse χρησιμοποιούν την ίδια σχεδίαση μαζικά παράλληλες επεξεργασίας (MPP), αλλά έχουν μερικές διαφορές λόγω της υποκείμενης πλατφόρμας. Αποθήκη δεδομένων του SQL είναι μια πλατφόρμα ως υπηρεσία (PaaS) που εκτελείται στον Azure. Παράλληλη αποθήκη δεδομένων εκτελείται στην ανάλυση πλατφόρμα σύστημα (σημεία Πρόσβασης) που είναι μια συσκευή εσωτερικής εγκατάστασης που εκτελείται σε Windows Server.

## <a name="what-is-distributed-data"></a>Τι είναι το κατανεμημένων δεδομένων;

Αποθήκη δεδομένων του SQL και Parallel Data Warehouse, κατανεμημένων δεδομένων αναφέρεται τα δεδομένα των χρηστών που είναι αποθηκευμένο σε πολλές θέσεις μέσω του συστήματος MPP. Κάθε μία από αυτές τις θέσεις λειτουργεί ως μια ανεξάρτητη χώρου αποθήκευσης και μονάδα επεξεργασίας που εκτελεί ερωτήματα για το τμήμα των δεδομένων. Κατανεμημένη δεδομένων είναι θεμελιώδεις με την εκτέλεση ερωτημάτων παράλληλα να επιτύχετε επιδόσεις ερωτημάτων υψηλή.

Για τη διανομή δεδομένων, αποθήκη δεδομένων αντιστοιχίζει κάθε γραμμή του πίνακα χρήστη σε μία θέση κατανεμημένη.  Μπορείτε να διανείμετε πίνακες με μια μέθοδο κατακερματισμός κατανομή ή μια μέθοδο round robin. Η μέθοδος διανομής καθορίζεται στην πρόταση CREATE TABLE. 

## <a name="hash-distributed-tables"></a>Κατανεμημένο κατακερματισμός πίνακες
  
Το παρακάτω διάγραμμα παρουσιάζει πώς ένα πλήρες (μη κατανεμημένο πίνακα) αποθηκεύονται ως πίνακα κατανεμημένο κατακερματισμός. Μια συνάρτηση ντετερμινιστική αντιστοιχίζει κάθε γραμμή στην οποία θα ανήκει μία διανομής. Στον ορισμό πίνακα, μία από τις στήλες ορίζεται ως τη στήλη διανομής. Η συνάρτηση κατακερματισμός χρησιμοποιεί την τιμή στη στήλη διανομής για να αντιστοιχίσετε κάθε γραμμή σε μια κατανομή.

Υπάρχουν ζητήματα επιδόσεων για την επιλογή μιας στήλης διανομής, όπως τη διάκριση, συνάρτηση skew δεδομένων και τους τύπους ερωτημάτων που εκτελούνται στο σύστημα.
  
![Κατανεμημένη πίνακα] (media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Κατανεμημένη πίνακα")  
  
-   Κάθε γραμμή ανήκει σε μία διανομής.  
  
-   Ένας αλγόριθμος ντετερμινιστική κατακερματισμός αντιστοιχίζει κάθε γραμμή σε μία διανομής.  
  
-   Τον αριθμό των γραμμών πίνακα ανά διανομής ποικίλλει, όπως φαίνεται από τα διαφορετικά μεγέθη των πινάκων.

## <a name="round-robin-distributed-tables"></a>Κατανεμημένη πίνακες ROUND robin

Κατανεμημένη πίνακα round robin κατανέμει τις γραμμές αντιστοιχίζοντας κάθε γραμμή σε μια κατανομή με διαδοχική τρόπο. Είναι γρήγορη να φορτώσετε δεδομένα σε έναν πίνακα round robin, αλλά μπορεί να είναι πιο αργές επιδόσεις ερωτημάτων.  Συμμετοχή σε έναν πίνακα round robin συνήθως απαιτεί reshuffling τις γραμμές για να ενεργοποιήσετε το ερώτημα για να δημιουργήσετε μια ακριβή αποτέλεσμα, η οποία διαρκεί.

## <a name="distributed-storage-locations-are-called-distributions"></a>Θέσεις αποθήκευσης κατανέμεται ονομάζονται κατανομές

Κάθε θέση κατανέμεται ονομάζεται μιας κατανομής. Κατά την εκτέλεση του ερωτήματος παράλληλα, κάθε κατανομή εκτελεί ένα ερώτημα SQL για το τμήμα των δεδομένων. Αποθήκη δεδομένων του SQL χρησιμοποιεί βάση δεδομένων SQL για να εκτελέσετε το ερώτημα. Παράλληλη αποθήκη δεδομένων χρησιμοποιεί SQL Server για να εκτελέσετε το ερώτημα. Αυτός ο σχεδιασμός αρχιτεκτονική τίποτα θέσει σε κοινή χρήση είναι θεμελιώδεις για την επίτευξη παράλληλες υπολογιστική κλιμάκωσης.

### <a name="can-i-view-the-distributions"></a>Μπορώ να προβάλω το κατανομές;

Κάθε διανομής έχει ένα Αναγνωριστικό διανομής και είναι ορατό σε προβολές συστήματος που σχετίζονται με την αποθήκη δεδομένων του SQL και παράλληλη αποθήκη δεδομένων. Μπορείτε να χρησιμοποιήσετε το Αναγνωριστικό διανομής για την αντιμετώπιση προβλημάτων επιδόσεις ερωτημάτων και άλλα προβλήματα. Για μια λίστα με τις προβολές συστήματος, ανατρέξτε στο θέμα [προβολή συστήματος MPP](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Διαφορά μεταξύ μιας κατανομής και έναν κόμβο υπολογισμού

Μια κατανομή είναι η βασική μονάδα για την αποθήκευση κατανεμημένων δεδομένων και επεξεργασία παράλληλες ερωτημάτων. Διανομή είναι ομαδοποιημένα σε κόμβους υπολογιστικών. Κάθε κόμβος παρακολουθεί μία ή περισσότερες κατανομές.  

-   Σύστημα αναλύσεων πλατφόρμα χρησιμοποιεί κόμβους υπολογιστικών ως κεντρική στοιχείο τις δυνατότητες αρχιτεκτονική και την κλίμακα ανάληψης υλικού. Πάντα χρησιμοποιεί οκτώ κατανομές ανά κόμβος, όπως φαίνεται στο διάγραμμα για κατανεμημένο κατακερματισμός πίνακες. Ο αριθμός των κόμβους υπολογιστικών και, επομένως, τον αριθμό των κατανομές, προσδιορίζεται από τον αριθμό των κόμβους υπολογιστικών αγοράσετε για τη συσκευή. Για παράδειγμα, εάν αγοράσετε οκτώ κόμβους υπολογιστικών, λαμβάνετε 64 κατανομές (8 κόμβοι x 8 κατανομές/κόμβος). 

-   Αποθήκη δεδομένων του SQL έχει έναν σταθερό αριθμό 60 κατανομές και έναν αριθμό ευέλικτη κόμβους υπολογιστικών. Κόμβους υπολογιστικών υλοποιούνται με Azure υπολογιστική και αποθήκευσης των πόρων. Να αλλάξετε τον αριθμό των κόμβους υπολογιστικών ανάλογα με το φόρτο εργασίας της υπηρεσίας υποστήριξης και την υπολογιστική χωρητικότητα (DWUs) που ορίζετε για την αποθήκη δεδομένων. Κατά την αλλαγή του πλήθους των κόμβους υπολογιστικών, τον αριθμό των κατανομές ανά κόμβος επίσης αλλάζει. 

### <a name="can-i-view-the-compute-nodes"></a>Μπορώ να προβάλω κόμβους υπολογιστικών;

Κάθε κόμβος έχει ένα Αναγνωριστικό κόμβου και είναι ορατό σε προβολές συστήματος που σχετίζονται με την αποθήκη δεδομένων του SQL και παράλληλη αποθήκη δεδομένων.  Μπορείτε να δείτε το κόμβος αναζητώντας στήλη node_id στο σύστημα προβολές των οποίων τα ονόματα ξεκινούν με sys.pdw_nodes. Για μια λίστα με τις προβολές συστήματος, ανατρέξτε στο θέμα [προβολή συστήματος MPP](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Αναπαραγωγή πίνακες για παράλληλη αποθήκη δεδομένων 
  
Ισχύει για: παράλληλη αποθήκη δεδομένων

Εκτός από τη χρήση κατανέμεται πινάκων, Parallel Data Warehouse προσφέρει μια επιλογή για την αναπαραγωγή πίνακες. Έναν *πίνακα από αναπαραγωγή* είναι ένας πίνακας που είναι αποθηκευμένο στο σύνολό σε κάθε κόμβο υπολογισμού. Αναπαραγωγή ενός πίνακα, καταργεί την ανάγκη για να μεταφέρετε τις γραμμές πίνακα μεταξύ των κόμβους υπολογιστικών πριν χρησιμοποιώντας τον πίνακα σε ένα σύνδεσμο ή συνάθροιση. Πίνακες από αναπαραγωγή είναι εφικτό με μικρές πίνακες μόνο λόγω του επιπλέον χώρο αποθήκευσης που απαιτείται για την αποθήκευση του πλήρους πίνακα σε κάθε κόμβο υπολογισμού.  
  
Το παρακάτω διάγραμμα παρουσιάζει έναν πίνακα από αναπαραγωγή που είναι αποθηκευμένο σε κάθε κόμβο υπολογισμού. Ο πίνακας από αναπαραγωγή είναι αποθηκευμένο σε όλων των δίσκων στους οποίους έχουν ανατεθεί στον κόμβο υπολογισμού. Αυτή η στρατηγική δίσκου έχει υλοποιηθεί με τη χρήση filegroups SQL Server.  
  
![Πίνακας από αναπαραγωγή] (media/sql-data-warehouse-distributed-data/replicated-table.png "Πίνακας από αναπαραγωγή") 
  
## <a name="next-steps"></a>Επόμενα βήματα
  
Για να χρησιμοποιήσετε αποτελεσματικά κατανέμεται πινάκων, ανατρέξτε στο θέμα [Distributing πίνακες στην αποθήκη δεδομένων του SQL](sql-data-warehouse-tables-distribute.md)  
  



  
