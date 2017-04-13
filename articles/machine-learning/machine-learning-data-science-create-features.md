<properties
    pageTitle="Δυνατότητα μηχανικής της διεργασίας ανάλυσης Cortana | Microsoft Azure" 
    description="Εξηγεί τους σκοπούς της δυνατότητας μηχανικής και παρέχει παραδείγματα σχετικά με το ρόλο στη διαδικασία δεδομένων βελτίωσης της μηχανικής εκμάθησης."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-in-the-cortana-analytics-process"></a>Η δυνατότητα μηχανικής της διεργασίας ανάλυσης Cortana 

Αυτό το θέμα εξηγεί τους σκοπούς της δυνατότητας μηχανικής και παρέχει παραδείγματα σχετικά με το ρόλο στη διαδικασία βελτίωσης δεδομένων της μηχανικής εκμάθησης. Τα παραδείγματα χρησιμοποιείται για την απεικόνιση αυτή η διαδικασία σχεδιάζονται από Azure μηχανικής εκμάθησης Studio. 

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Το **μενού** συνδέσεις σε θέματα τα οποία περιγράφουν τον τρόπο δημιουργίας δυνατοτήτων για τα δεδομένα σε διάφορα περιβάλλοντα. Αυτή η εργασία είναι ένα βήμα για τη [Διαδικασία Science δεδομένων ομάδας (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Η δυνατότητα μηχανικής προσπάθειες για την αύξηση της πρόβλεψης ισχύος του εκμάθησης αλγόριθμοι, δημιουργώντας δυνατότητες από ανεπεξέργαστα δεδομένα που να διευκολύνετε τη διαδικασία εκμάθησης. Την μηχανικής και την επιλογή των δυνατοτήτων είναι ένα μέρος του TDSP που περιγράφονται σε το [ποια είναι η διαδικασία ομάδας δεδομένων Science;](data-science-process-overview.md) Η δυνατότητα μηχανικής και επιλογή είναι τμήματα της το βήμα **δυνατότητες ανάπτυξη** του TDSP. 

* **η δυνατότητα μηχανικής**: επιχειρήσει αυτήν τη διαδικασία για να δημιουργήσετε πρόσθετες σχετικές δυνατότητες από τις υπάρχουσες δυνατότητες ανεπεξέργαστα των δεδομένων και για την αύξηση της πρόβλεψης ισχύος του αλγόριθμου εκμάθησης.

* **η δυνατότητα επιλογής**: Αυτή η διαδικασία επιλέγει το πλήκτρο υποσύνολο αρχικό δυνατότητες των δεδομένων σε μια προσπάθεια για να μειώσετε το διαστατικότητα του προβλήματος εκπαίδευση.

Κανονικά **μηχανικής δυνατότητα** εφαρμόζεται πρώτα για να δημιουργήσετε πρόσθετες δυνατότητες του και, στη συνέχεια, το βήμα **δυνατότητα επιλογής** πραγματοποιείται για να αποκλείσετε σημασία, πλεονάζουν ή ιδιαίτερα συσχετισμένης δυνατότητες.

Εκπαίδευση δεδομένων που χρησιμοποιούνται σε εκμάθησης υπολογιστή μπορούν να βελτιωθούν συχνά κατά την εξαγωγή των δυνατοτήτων από τα ανεπεξέργαστα δεδομένα που συλλέγονται. Παράδειγμα σχεδιασμένες δυνατότητα του στο περιβάλλον της μαθαίνοντας πώς να ταξινομήσουν τις εικόνες της χειρόγραφης χαρακτήρες είναι η δημιουργία λίγο πυκνότητας χάρτη συνταχθεί από τα δεδομένα κατανομής ανεπεξέργαστα bit. Αυτή η αντιστοίχιση μπορούν να σας βοηθήσουν να εντοπίσετε πιο αποδοτικά από χρησιμοποιώντας απλώς τα ανεπεξέργαστα διανομής απευθείας στα άκρα από τους χαρακτήρες.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="creating-features-from-your-data---feature-engineering"></a>Δημιουργία δυνατότητες από τα δεδομένα σας - δυνατοτήτων μηχανικής

Τα δεδομένα του εκπαιδευτικού αποτελείται από μια μήτρα που αποτελείται από παραδείγματα (εγγραφές ή παρατηρήσεις που είναι αποθηκευμένα σε γραμμές), τα οποία έχει ένα σύνολο δυνατοτήτων (μεταβλητές ή πεδία που είναι αποθηκευμένα στις στήλες). Οι δυνατότητες που καθορίζεται στη σχεδίαση πειραματικής αναμένεται να χαρακτηρίζει τα μοτίβα στα δεδομένα. Μολονότι πολλά από τα πεδία ανεπεξέργαστα δεδομένα να απευθείας συμπεριληφθεί στο σύνολο επιλεγμένη δυνατότητα που χρησιμοποιείται για την εκπαίδευση μοντέλου, είναι συχνά στην υπόθεση ότι πρόσθετες δυνατότητες (σχεδιασμένες) πρέπει να είναι συνταχθεί από τις δυνατότητες του τα ανεπεξέργαστα δεδομένα για τη δημιουργία ενός συνόλου δεδομένων βελτιωμένη εκπαίδευσης.

Τι είδους δυνατότητες θα πρέπει να δημιουργηθεί για τη βελτίωση του συνόλου δεδομένων κατά την εκπαίδευση μοντέλου; Από αποσυμπίληση δυνατότητες που βελτιώνουν την εκπαίδευση παρέχει πληροφορίες που διαφοροποιούν καλύτερα τα μοτίβα στα δεδομένα. Αναμένεται τις νέες δυνατότητες για να δώσετε επιπλέον πληροφορίες που δεν είναι σαφώς τραβήξατε ή εύκολα εμφανή στο πλαίσιο Ορισμός αρχικής ή υπάρχουσα δυνατότητα. Αλλά αυτή η διαδικασία είναι μια εικόνα. Ήχος και παραγωγικοί αποφάσεις συχνά απαιτούν ορισμένες γνώσεις τομέα.

Κατά την εκκίνηση με Azure μηχανικής εκμάθησης, είναι πιο εύκολο να μπορεί να κρατιέται αυτήν τη διαδικασία χρησιμοποιώντας συγκεκριμένα δείγματα που παρέχονται στο το Studio. Δύο παραδείγματα παρουσιάζονται εδώ:

* Ένα παράδειγμα παλινδρόμησης [πρόβλεψη του αριθμού των ποδηλάτων μισθώματα](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) σε μια έρευνα επιβλεπόμενες όπου είναι γνωστό τις τιμές προορισμού
* Ένα παράδειγμα ταξινόμηση εξόρυξης κειμένου χρησιμοποιώντας [Τη δυνατότητα ο κατακερματισμός](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

### <a name="example-1-adding-temporal-features-for-regression-model"></a>Παράδειγμα 1: Προσθήκη χρονικό δυνατότητες για μοντέλο παλινδρόμησης ###

Ας χρησιμοποιήσουμε το έρευνας "ζήτηση προβλέψεις των ποδηλάτων" στο Azure μηχανικής εκμάθησης Studio για να παρουσιάζουν τον τρόπο αποσυμπίληση δυνατοτήτων για μια εργασία παλινδρόμησης. Ο στόχος της έρευνας αυτό είναι να προβλέψει η ζήτηση για τα ποδήλατα, αυτό σημαίνει ότι ο αριθμός των ποδηλάτων μισθώματα μέσα σε ένα συγκεκριμένο μήνα/ημέρα/ώρα. Το σύνολο δεδομένων "ενοικίασης ποδηλάτων UCI dataset" χρησιμοποιείται ως τα ανεπεξέργαστα δεδομένα εισόδου. Σε αυτό το σύνολο δεδομένων βασίζεται σε πραγματικά δεδομένα από την εταιρεία Bikeshare κεφαλαίο που διατηρεί δικτύου ενοικίασης ποδηλάτων στον ελεγκτή Τομέα Washington στις Ηνωμένες Πολιτείες. Το σύνολο δεδομένων αντιπροσωπεύει τον αριθμό των ποδηλάτων μισθώματα μέσα σε μια συγκεκριμένη ώρα της ημέρας κατά τα έτη 2011 και έτος 2012 και περιέχει 17379 γραμμές και στήλες 17. Το σύνολο των δυνατοτήτων ανεπεξέργαστα περιέχει καιρού συνθήκες (θερμοκρασίας/υγρασία ανέμου ταχύτητα) και τον τύπο της ημέρας (αργία weekday). Το πεδίο πρόβλεψη είναι "cnt", μια μέτρηση που αντιπροσωπεύει το μισθώματα ποδηλάτων μέσα σε μια συγκεκριμένη ώρα και που κυμαίνεται εύρος από 1 έως 977.

Με το στόχο η κατασκευή αποτελεσματικές δυνατότητες στα δεδομένα εκπαίδευση, τέσσερα παλινδρόμησης μοντέλα δημιουργούνται χρησιμοποιώντας τον ίδιο αλγόριθμο αλλά με τέσσερα διαφορετικά εκπαίδευση συνόλων δεδομένων. Τα τέσσερα σύνολα δεδομένων αντιπροσωπεύει τα ίδια ανεπεξέργαστα δεδομένα εισαγωγής, αλλά με έναν κωδικό αυξανόμενη δυνατοτήτων ορίσετε. Αυτές οι δυνατότητες είναι ομαδοποιημένα σε τέσσερις κατηγορίες:

1. A = καιρού + αργία + weekday + Σαββατοκύριακο δυνατότητες της προβλεπόμενης τιμής ημέρας
2. B = αριθμός των ποδηλάτων που έχουν μισθώσει σε κάθε μία από τις προηγούμενες 12 ώρες
3. C = αριθμός των ποδηλάτων που έχουν μισθώσει σε κάθε μία από τις προηγούμενες 12 ημέρες κατά την ίδια ώρα
4. D = αριθμός των ποδηλάτων που έχουν μισθώσει σε κάθε μία από τις προηγούμενες 12 εβδομάδες στην ίδια ώρα και την ίδια ημέρα

Εκτός από το A σύνολο δυνατοτήτων που υπάρχουν ήδη στο τα αρχικά ανεπεξέργαστα δεδομένα, τα άλλα τρία σύνολα δυνατοτήτων δημιουργούνται μέσω της δυνατότητας μηχανική διαδικασία. Η δυνατότητα ρυθμιστεί καταγραφές B πολύ πρόσφατο αίτημα για τα ποδήλατα. Η δυνατότητα ρυθμιστεί καταγραφές C η ζήτηση για τα ποδήλατα μια συγκεκριμένη ώρα. Η δυνατότητα ρυθμιστεί D καταγραφές απαιτήσεων για τα ποδήλατα σε συγκεκριμένη ώρα και συγκεκριμένη ημέρα της εβδομάδας. Το τα σύνολα δεδομένων τέσσερις εκπαίδευση κάθε περιλαμβάνει ένα σύνολο δυνατοτήτων, A + B, A + B + C και A + B + C + D, αντίστοιχα.

Στο το Azure μηχανικής εκμάθησης έρευνας, αυτά τα τέσσερα σύνολα δεδομένων εκπαίδευση είναι μορφοποιημένη μέσω τέσσερις διακλαδώσεις από την προ-επεξεργασία συνόλου δεδομένων εισόδου. Εκτός από τα αριστερά περιέχει περισσότερες κλάδο, κάθε μία από αυτές τις διακλαδώσεις μια λειτουργική μονάδα [δέσμης ενεργειών R Execute](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) , στις οποίες ένα σύνολο προέρχεται δυνατότητες (δυνατότητα οριστεί B, C και D) αντίστοιχα έχουν συνταχθεί και προσαρτημένο σε του συνόλου δεδομένων που έχουν εισαχθεί. Η παρακάτω εικόνα δείχνει τη δέσμη ενεργειών R που χρησιμοποιούνται για τη δημιουργία σύνολο δυνατοτήτων B στον δεύτερο αριστερό κλάδο.

![Δημιουργία δυνατότητες](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

Τη σύγκριση των αποτελεσμάτων επιδόσεων από τα τέσσερα μοντέλα συνοψίζονται στον παρακάτω πίνακα. Τα καλύτερα αποτελέσματα εμφανίζονται με δυνατότητες A + B + C. Σημειώστε ότι το επιτόκιο σφάλματος μειώνει πότε σύνολο δυνατοτήτων επιπλέον περιλαμβάνονται στα δεδομένα εκπαίδευσης. Επιβεβαιώνει μας υπόθεση ότι το σύνολο των δυνατοτήτων B, C παρέχουν πρόσθετες σχετικές πληροφορίες για την εργασία παλινδρόμησης. Αλλά η προσθήκη της δυνατότητας D δεν φαίνεται να παρέχουν οποιαδήποτε πρόσθετη μείωση του ποσοστού σφάλματος.

![αποτέλεσμα σύγκρισης](./media/machine-learning-data-science-create-features/result1.png)

### <a name="example2"></a>Παράδειγμα 2: Δημιουργία δυνατότητες σε εξόρυξης κειμένου  

Η δυνατότητα μηχανικής ευρέως εφαρμόζεται σε εργασίες που σχετίζονται με εξόρυξης κειμένου, όπως το έγγραφο ταξινόμησης και άποψη ανάλυσης. Για παράδειγμα, όταν θέλετε να ταξινομήσετε τα έγγραφα σε διάφορες κατηγορίες, ένα τυπικό υπόθεση είναι ότι το word/φράσεις που περιλαμβάνονται στην κατηγορία για ένα έγγραφο είναι λιγότερο πιθανό να προκύψουν σε άλλο έγγραφο κατηγορία. Σε ένα άλλο λέξεις, τη συχνότητα της κατανομής λέξεις/φράσεις είναι δυνατό να χαρακτηρίσετε κατηγορίες άλλο έγγραφο. Στις εφαρμογές εξόρυξης κειμένου, επειδή μεμονωμένα τμήματα των περιεχομένων του κειμένου συνήθως χρησιμοποιηθεί ως δεδομένα εισόδου, η δυνατότητα μηχανική διαδικασία είναι απαραίτητη για να δημιουργήσετε τις δυνατότητες που αφορούν το word/φράση συχνότητας.

Για να επιτύχετε αυτήν την εργασία, μια τεχνική που ονομάζεται **ο κατακερματισμός δυνατότητα** εφαρμόζεται σε αποτελεσματικά μετατρέψετε δυνατότητες αυθαίρετο κειμένου σε δείκτες. Αντί να συσχετίζοντας κάθε δυνατότητα κειμένου (λέξεις φράσεις) σε ένα συγκεκριμένο ευρετήριο, αυτή τη μέθοδο συναρτήσεις εφαρμογή μιας συνάρτησης κατακερματισμός στις δυνατότητες και χρησιμοποιώντας τις τιμές τους κατακερματισμός ως δείκτες απευθείας.

Στο Azure μηχανικής εκμάθησης, υπάρχει μια λειτουργική μονάδα [Ο κατακερματισμός δυνατότητα](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) , η οποία δημιουργεί αυτές τις word/φράση δυνατότητες εύκολα. Η εικόνα που ακολουθεί παρουσιάζει ένα παράδειγμα της χρήσης αυτήν τη λειτουργική μονάδα. Το σύνολο δεδομένων εισόδου περιέχει δύο στήλες: ο χαρακτηρισμός βιβλίο τιμές από 1 έως 5, και το περιεχόμενο της πραγματικής αναθεώρηση. Ο στόχος της λειτουργικής μονάδας αυτή [Η δυνατότητα ο κατακερματισμός](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) είναι για να ανακτήσετε μια ομάδα νέες δυνατότητες που εμφανίζουν τη συχνότητα εμφάνισης των αντίστοιχων λέξεων / αναθεώρηση phrase(s) το συγκεκριμένο βιβλίο. Για να χρησιμοποιήσετε αυτήν τη λειτουργική μονάδα, πρέπει να ολοκληρώσετε τα παρακάτω βήματα:

* Πρώτα, επιλέξτε τη στήλη που περιέχει το κείμενο εισαγωγής ("Στηλ2" σε αυτό το παράδειγμα).
* Δεύτερον, ορίστε το "bitsize Hashing" σε 8, γεγονός που σημαίνει 2 ^ 8 = 256 δυνατότητες θα δημιουργηθεί. Η φάση/λέξη σε όλο το κείμενο θα είναι κατακερματισμός για να τα ευρετήρια 256. Η παράμετρος "Hashing bitsize" περιοχές από 1 έως το 31. Λέξεων / phrase(s) είναι λιγότερο πιθανό να κατακερματίζεται στο ίδιο ευρετήριο εάν τη ρύθμιση να είναι ένα μεγαλύτερο αριθμό.
* Τρίτη, ορίστε την παράμετρο "Ν-^ g" σε 2. Αυτό αποκτά τη συχνότητα εμφάνισης των unigrams (μια δυνατότητα για κάθε μία λέξη) και bigrams (μια δυνατότητα για κάθε ζεύγος γειτονικά λέξεων) από το κείμενο εισαγωγής. Η παράμετρος "Ν-^ g" κυμαίνεται από 0 έως 10, η οποία υποδεικνύει ο μέγιστος αριθμός διαδοχικών λέξεις που θα συμπεριληφθούν στην μια δυνατότητα.  

!["Δυνατότητα Hashing" λειτουργική μονάδα](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

Η παρακάτω εικόνα δείχνει τι αυτά τα νέα δυνατότητα εμφάνισης όπως.

![Παράδειγμα "Δυνατότητα Hashing"](./media/machine-learning-data-science-create-features/feature-Hashing2.png)


## <a name="conclusion"></a>Ολοκλήρωση

Δυνατότητες σχεδιασμένες και επιλεγμένη αύξηση της αποτελεσματικότητας της διαδικασίας εκπαίδευσης που προσπαθεί να εξαγάγετε τις πληροφορίες του κλειδιού που περιέχονται στα δεδομένα. Επίσης, βελτιώνουν στη δύναμη του αυτά τα μοντέλα για να ταξινομήσετε τα δεδομένα εισόδου με ακρίβεια και πρόβλεψη πιο robustly τα αποτελέσματα που σας ενδιαφέρουν. Η δυνατότητα μηχανικής και επιλογή επίσης να συνδυάσετε για να κάνετε πιο υπολογιστικά tractable την εκμάθηση. Αυτό γίνεται με τη βελτίωση και, στη συνέχεια, να μειώσετε τον αριθμό των δυνατοτήτων που χρειάζεται για να ρυθμίσετε ή να εκπαιδεύσετε μοντέλου. Μαθηματικά μιλάτε, οι δυνατότητες επιλεγμένο για την εκπαίδευση του μοντέλου είναι ένα ελάχιστο σύνολο ανεξάρτητων μεταβλητών που εξηγούν τα μοτίβα στα δεδομένα και, στη συνέχεια, πρόβλεψης αποτελέσματα με επιτυχία.

Σημειώστε ότι δεν είναι πάντα απαραίτητα για να εκτελέσετε τη δυνατότητα μηχανικής ή μια δυνατότητα επιλογής. Εάν είναι απαραίτητο ή όχι εξαρτάται από τα δεδομένα σας έχουν ή συλλογής, ο αλγόριθμος επιλέγουμε και το στόχο της την έρευνα.
 