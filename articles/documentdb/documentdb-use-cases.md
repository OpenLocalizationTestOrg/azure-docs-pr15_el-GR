<properties 
    pageTitle="Κοινές DocumentDB περιπτώσεις χρήσης | Microsoft Azure" 
    description="Μάθετε περισσότερα σχετικά με την αρχή πέντε περιπτώσεις χρήσης για DocumentDB: χρήστη που δημιουργούνται από το περιεχόμενο, καταγραφή συμβάντων, στον κατάλογο δεδομένων, δεδομένα προτιμήσεις των χρηστών και Internet πράγματα (IoT)." 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>Συνήθεις περιπτώσεις χρήσης DocumentDB
Σε αυτό το άρθρο παρέχει μια επισκόπηση του διάφορες περιπτώσεις κοινής χρήσης για DocumentDB.  Τις συστάσεις σε αυτό το άρθρο χρησιμεύσει ως σημείο εκκίνησης ως αναπτύσσετε την εφαρμογή σας με DocumentDB.   

Μετά την ανάγνωση αυτό το άρθρο, θα έχετε τη δυνατότητα να απαντούν στα παρακάτω ερωτήματα: 
 
- Τι είναι οι συνήθεις περιπτώσεις χρήσης για DocumentDB;
- Ποια είναι τα πλεονεκτήματα της χρήσης DocumentDB ως χρήστης που δημιουργούνται από το χώρο αποθήκευσης περιεχομένου;
- Ποια είναι τα πλεονεκτήματα της χρήσης DocumentDB ως κατάλογο δεδομένων αποθηκεύουν;
- Ποια είναι τα πλεονεκτήματα της χρήσης DocumentDB ως ένα αρχείο καταγραφής συμβάντων αποθηκεύουν;
- Ποια είναι τα πλεονεκτήματα της χρήσης DocumentDB ως δεδομένων προτιμήσεων χρήστη αποθηκεύουν;
- Ποια είναι τα πλεονεκτήματα της χρήσης DocumentDB ως χώρος αποθήκευσης δεδομένων για συστήματα Internet πράγματα (IoT);

## <a name="common-use-cases-for-documentdb"></a>Συνήθεις περιπτώσεις χρήσης για DocumentDB
Azure DocumentDB είναι μια βάση δεδομένων NoSQL γενικής χρήσης που χρησιμοποιείται σε ένα ευρύ φάσμα εφαρμογών και περιπτώσεις χρήσης. Είναι μια καλή επιλογή για οποιαδήποτε εφαρμογή που χρειάζεται χρόνους χαμηλής σειρά χιλιοστών του δευτερολέπτου απόκρισης και πρέπει να κλιμακωθεί γρήγορα. Ακολουθούν ορισμένα χαρακτηριστικά DocumentDB που να είναι κατάλληλη για εφαρμογές υψηλών επιδόσεων.

- DocumentDB διαμερίσματα εγγενώς τα δεδομένα σας για υψηλή διαθεσιμότητα και κλιμάκωση.
- Του DocumentDB έχει SSD αντίγραφα χώρου αποθήκευσης με το χρόνο απόκρισης σειρά χιλιοστών του δευτερολέπτου χαμηλής αδράνειας.
- Υποστήριξη του DocumentDB για επίπεδα συνέπειας όπως ενδεχόμενες, την περίοδο λειτουργίας και δεσμεύεται staleness επιτρέπει χαμηλής κόστος σε απόδοσης-αναλογία. 
- DocumentDB έχει ευέλικτη φιλικούς προς δεδομένων τιμολόγησης μοντέλο που μέτρα χώρου αποθήκευσης και απόδοση ανεξάρτητα.
- Μοντέλο δεσμευμένη μετάδοσης του DocumentDB σάς επιτρέπει να πιστεύετε ότι όσον αφορά αριθμό διαβάζει/εγγραφών αντί για μνήμης/CPU/IOP από το υποκείμενο υλικό.
- Του DocumentDB σχεδίαση σάς επιτρέπει να μπορείτε να προσθέσετε πρόσκληση σε μεγάλους όγκους με τη σειρά των δισεκατομμύρια αιτήσεις ανά ημέρα.

Αυτά τα χαρακτηριστικά είναι ιδιαίτερα χρήσιμη όταν θέλετε να web, κινητές συσκευές, παιχνιδιών και τις εφαρμογές IoT που χρειάζεται χρόνους χαμηλής απόκρισης και πρέπει να χειριστείτε τεράστιες ποσότητες ανάγνωσης και εγγραφής. 

## <a name="user-generated-content"></a>Το περιεχόμενο που δημιουργούνται από το χρήστη
Μια υπόθεση κοινής χρήσης για DocumentDB είναι για την αποθήκευση και ερωτήματος χρήστη που δημιουργούνται από το περιεχόμενο (UGC) για το web και εφαρμογές για κινητές συσκευές, εφαρμογές μέσα ιδιαίτερα κοινωνικής δικτύωσης.  Ορισμένα παραδείγματα UGC είναι τις περιόδους λειτουργίας συνομιλίας, tweets, καταχωρήσεων ιστολογίου, χαρακτηρισμούς και σχόλια.  Συχνά, το UGC σε εφαρμογές μέσα κοινωνικής δικτύωσης είναι ένας συνδυασμός της ελεύθερης μορφής κείμενο, ιδιότητες, τις ετικέτες και σχέσεις που δεν είναι δεσμεύεται από σκληρού δομή.   

Περιεχομένου όπως συνομιλίες, τα σχόλια και οι καταχωρήσεις που μπορούν να αποθηκευτούν σε DocumentDB χωρίς να απαιτείται μετασχηματισμοί ή σύνθετη αντικειμένων σε επίπεδα σχεσιακές αντιστοίχισης.  Ιδιότητες δεδομένων μπορούν να προστεθούν ή να τροποποιηθούν εύκολα ώστε να ταιριάζει με απαιτήσεις όπως προγραμματιστές διαδοχικές προσεγγίσεις μέσω τον κώδικα της εφαρμογής, επομένως προώθηση ταχεία ανάπτυξη.  

Εφαρμογές που ενοποιούνται με διάφορα κοινωνικά δίκτυα πρέπει να ανταποκρίνονται Αλλαγή σχημάτων από αυτά τα δίκτυα.  Καθώς δεδομένων δημιουργείται αυτόματα ένα ευρετήριο από προεπιλογή σε DocumentDB, δεδομένων είναι έτοιμη για να αναζητηθούν οποιαδήποτε στιγμή.  Ως εκ τούτου, αυτές οι εφαρμογές έχετε την ευελιξία να ανακτήσετε προβλέψεις σύμφωνα με τις ανάγκες αντίστοιχα τους.       

Πολλές από τις εφαρμογές του κοινού εκτέλεση στο καθολικό κλίμακα και να παρουσιάζουν μοτίβα απρόσμενα χρήσης.  Ευελιξία στη κλίμακας ο χώρος αποθήκευσης δεδομένων είναι απαραίτητο, όπως το επίπεδο εφαρμογής κλίμακες ανάλογα με τη χρήση ζήτηση.  Μπορείτε να κλιμάκωση ανάληψη, προσθέτοντας τα διαμερίσματα πρόσθετα δεδομένα κάτω από ένα λογαριασμό DocumentDB.  Επιπλέον, μπορείτε επίσης να δημιουργήσετε επιπλέον λογαριασμούς DocumentDB σε πολλές περιοχές. Για τη διαθεσιμότητα περιοχή DocumentDB υπηρεσίας, ανατρέξτε στο θέμα [Azure περιοχές](https://azure.microsoft.com/regions/#services).   

## <a name="catalog-data"></a>Στον κατάλογο δεδομένων
Σενάρια χρήσης του καταλόγου δεδομένων αφορούν την αποθήκευση και υποβολή ερωτημάτων σε ένα σύνολο χαρακτηριστικών για οντοτήτων όπως άτομα, ψηφία και προϊόντα.  Ορισμένα παραδείγματα στον κατάλογο δεδομένων είναι λογαριασμούς χρηστών, καταλόγους προϊόντων, συσκευή μητρώα για IoT και κάσα υλικά συστημάτων.  Χαρακτηριστικά για αυτά τα δεδομένα μπορεί να διαφέρουν και να αλλάξετε μέσα στο χρόνο για να προσαρμοστεί απαιτήσεις της εφαρμογής.  

Εξετάστε το ενδεχόμενο παράδειγμα με έναν κατάλογο προϊόντων για έναν προμηθευτή αυτοκινήτων τμήματα. Κάθε τμήμα μπορεί να έχει το δικό της χαρακτηριστικά εκτός από τα κοινά χαρακτηριστικά που κάνουν κοινή χρήση όλα τα τμήματα.  Επιπλέον, χαρακτηριστικά για ένα συγκεκριμένο τμήμα, να αλλάξετε το επόμενο έτος όταν κυκλοφορήσει ένα νέο μοντέλο.  Ως ένα χώρο αποθήκευσης εγγράφων JSON, DocumentDB υποστηρίζει ευέλικτη σχήματα και σας επιτρέπει να αντιπροσωπεύει τα δεδομένα με ένθετη ιδιότητες και, επομένως, είναι επίσης και κατάλληλοι για την αποθήκευση των δεδομένων στον κατάλογο προϊόντων.       

## <a name="logging-and-time-series-data"></a>Καταγραφή και χρονολογική σειρά δεδομένων
Καταγραφή από την εφαρμογή αποστέλλεται συχνά σε μεγάλους όγκους και ενδέχεται να έχουν διάφορα χαρακτηριστικά, ανάλογα με την έκδοση της εφαρμογής ή το στοιχείο καταγραφή συμβάντων.  Δεδομένα του αρχείου καταγραφής, δεν δεσμεύεται από σύνθετες σχέσεις ή σταθερό δομές. Αυξανόμενη, δεδομένα του αρχείου καταγραφής είναι μόνιμα σε μορφή JSON εφόσον JSON είναι πιο απλές και εύκολη για τους ανθρώπους για ανάγνωση.
   
Συνήθως, υπάρχουν δύο κύριες χρήση περιπτώσεις που σχετίζονται με δεδομένα του αρχείου καταγραφής συμβάντων.  Είναι η πρώτη περίπτωση χρήσης για την εκτέλεση ερωτημάτων ad-hoc πάνω από ένα υποσύνολο των δεδομένων για την αντιμετώπιση προβλημάτων.  Κατά την αντιμετώπιση προβλημάτων, ένα υποσύνολο των δεδομένων πρώτα ανακτώνται από τα αρχεία καταγραφής, συνήθως με χρονολογική σειρά.  Στη συνέχεια, τη διερεύνηση πραγματοποιείται από το φιλτράρισμα του συνόλου δεδομένων με τα επίπεδα σφάλματος ή μηνύματα σφάλματος. Αυτό είναι το σημείο όπου την αποθήκευση αρχείων καταγραφής συμβάντων στο DocumentDB είναι πλεονέκτημα. Αρχείο καταγραφής δεδομένα αποθηκευμένα σε DocumentDB δημιουργείται αυτόματα ένα ευρετήριο από προεπιλογή και, επομένως, είναι έτοιμο για να αναζητηθούν οποιαδήποτε στιγμή. Επιπλέον, δεδομένα του αρχείου καταγραφής μπορεί να είναι σταθερές σε διαμερίσματα δεδομένων ως μια σειρά ώρας. Παλαιότερων αρχείων καταγραφής μπορεί να είναι ανάπτυξης των ψυκτικές ανά την πολιτική διατήρησης.          

Η δεύτερη περίπτωση χρήσης περιλαμβάνει χρόνο εκτέλεση εργασιών ανάλυσης δεδομένων που εκτελείται χωρίς σύνδεση επάνω από ένα μεγάλο όγκο δεδομένων αρχείου καταγραφής.  Χρησιμοποιήστε αυτήν την περίπτωση παραδείγματα ανάλυσης διαθεσιμότητα διακομιστή, εφαρμογή σφάλματος ανάλυσης και ανάλυση δεδομένων clickstream.  Συνήθως, Hadoop χρησιμοποιείται για την εκτέλεση αυτών των αναλύσεων τύπων.  Με τη σύνδεση Hadoop για DocumentDB, βάσεις δεδομένων DocumentDB λειτουργεί ως προελεύσεις δεδομένων και δέκτες για γουρούνι, ομάδας και χάρτη/μείωση εργασίες. Για λεπτομέρειες σχετικά με τη γραμμή σύνδεσης Hadoop για DocumentDB, ανατρέξτε στο θέμα [εκτελέσετε μια εργασία Hadoop με DocumentDB και HDInsight](documentdb-run-hadoop-with-hdinsight.md).      

## <a name="gaming"></a>Παιχνιδιών
Το επίπεδο βάσης δεδομένων είναι κρίσιμης σημασίας στοιχείο παιχνιδιών εφαρμογών. Μοντέρνα αγώνων εκτελούν επεξεργασία γραφικών σε προγράμματα-πελάτες κινητού/console, αλλά βασίζονται στο cloud για την παροχή προσαρμοσμένων και προσαρμοσμένων περιεχομένου όπως στατιστικές στο παιχνίδι ενσωμάτωσης μέσων κοινωνικής δικτύωσης και πίνακες βαθμολογίας υψηλής κατάταξης. Αγώνων απαιτείται πολύ χαμηλή των αδρανειών για διαβάζει και εγγραφές για την παροχή ενός ελκυστικές στο παιχνίδι εμπειρία και το επίπεδο βάσης δεδομένων πρέπει να χειρίζεται τις υψηλές και χαμηλές επιδόσεις των μαθητών στην αίτηση χρεώσεις κατά τη διάρκεια της νέας παιχνιδιών εκκινήσεις και ενημερώσεις δυνατοτήτων.

DocumentDB χρησιμοποιείται από αγώνων μαζικών κλίμακας όπως [η παρουσίαση μερικών νεκρού: Land του χωρίς Άντρας](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) τα [Επόμενα αγώνων](http://www.nextgames.com/), και [Halo 5: τους κηδεμόνες τους](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Και στις δύο περιπτώσεις χρήσης, τα βασικά πλεονεκτήματα των DocumentDB έχουν τα εξής:

- DocumentDB επιτρέπει απόδοσης για να κλιμακωθεί προς τα επάνω ή προς τα κάτω elastically. Αυτό σας επιτρέπει αγώνων χειρισμού ενημέρωση προφίλ και στατιστικά όγκου από δεκάδες να εκατομμύρια ταυτόχρονη παίκτες, κάνοντας μία κλήση API.
- DocumentDB υποστηρίζει χιλιοστά του δευτερολέπτου διαβάζει και εγγράφει για να αποφύγετε τυχόν υστέρηση κατά τη διάρκεια παιχνίδι.
- DocumentDB της αυτόματης δημιουργίας ευρετηρίου για επιτρέπει για το φιλτράρισμα σε σχέση με πολλές διαφορετικές ιδιότητες σε πραγματικό χρόνο, π.χ., εντοπίστε παίκτες, τους εσωτερικού προγράμματος αναπαραγωγής αναγνωριστικών ή τους GameCenter, Facebook, αναγνωριστικά Google ή ερώτημα που βασίζεται σε πρόγραμμα αναπαραγωγής ιδιότητα μέλους σε μια guild. Αυτό είναι δυνατό χωρίς τη δημιουργία ευρετηρίου σύνθετες ή sharding υποδομή.
- Κοινωνικές δυνατότητες όπως μηνύματα συνομιλίας στο παιχνίδι, ιδιότητες μελών guild προγράμματος αναπαραγωγής, προκλήσεις ολοκληρώθηκε, πίνακες βαθμολογίας υψηλής κατάταξης και κοινωνικών γραφήματα είναι ευκολότερο να υλοποιήσετε με μια ευέλικτη διάταξη.
- DocumentDB ως μια πλατφόρμα-ως-a-υπηρεσίας διαχειριζόμενων (PaaS) απαιτείται ελάχιστους ρύθμιση και διαχείριση εργασίας για να επιτρέψετε τη γρήγορη διαδοχικών προσεγγίσεων και μείωση του χρόνου αγοράς.


## <a name="user-preferences-data"></a>Δεδομένα τις προτιμήσεις χρήστη
Σήμερα, πιο σύγχρονο web και εφαρμογές για κινητές συσκευές σας με σύνθετη προβολές και εμπειρίες. Αυτές οι προβολές και εμπειρίες είναι συνήθως δυναμική, τροφοδοσίας προτιμήσεις χρήστη ή διαθέσεις και εμπορική προσαρμογή τις ανάγκες.  Ως εκ τούτου, εφαρμογές πρέπει να είναι σε θέση να ανακτήσετε προσωπικές ρυθμίσεις αποτελεσματική προκειμένου να απόδοση στοιχεία περιβάλλοντος εργασίας Χρήστη και εμπειρίες γρήγορα. 

JSON είναι μια αποτελεσματική μορφή για την αναπαράσταση δεδομένων διάταξη περιβάλλοντος εργασίας Χρήστη δεν είναι μόνο ελαφριές, αλλά επίσης να εύκολα ερμηνεύεται από το JavaScript.  DocumentDB προσφέρει επίπεδα tunable συνέπειας που επιτρέπουν γρήγορη διαβάζει με χαμηλή λανθάνων χρόνος εγγραφές. Ως εκ τούτου, την αποθήκευση δεδομένων διάταξη περιβάλλοντος εργασίας Χρήστη, συμπεριλαμβανομένων των προσωπικών ρυθμίσεων ως JSON έγγραφα σε DocumentDB αποτελεί αποτελεσματικό μέσο για να μεταδώσετε το σύρματος αυτών των δεδομένων.

## <a name="iot-and-device-sensor-data"></a>Δεδομένα αισθητήρα IoT και συσκευή
Περιπτώσεις χρήσης IoT συνήθως μοιραστείτε ορισμένες μοτίβα σε πώς μπορούν ingest, επεξεργασία και αποθήκευση δεδομένων.  Πρώτα, αυτά τα συστήματα επιτρέπεται εισαγωγής δεδομένων που μπορούν να ingest καταιγισμό δεδομένων από τη συσκευή αισθητήρες από διάφορες τοπικές ρυθμίσεις.  Στη συνέχεια, αυτά τα συστήματα διεργασίας και ανάλυση ροής δεδομένων για την παραγωγή ιδέες πραγματικό χρόνο. Και τελευταίος πιο αλλά δεν τουλάχιστον, εάν δεν όλα τα δεδομένα που θα εμφανίσει ξεκινάτε με ένα χώρο αποθήκευσης δεδομένων για ανάλυση ad-hoc στο κατά την υποβολή ερωτήματος και χωρίς σύνδεση.    

Microsoft Azure προσφέρει εμπλουτισμένες υπηρεσίες που μπορούν να χρησιμοποιηθούν για IoT περιπτώσεις χρήσης.  Azure IoT υπηρεσίες είναι ένα σύνολο από υπηρεσίες όπως το Azure συμβάν διανομείς, Azure DocumentDB, Azure ροή ανάλυση, Azure ειδοποίηση διανομέα, Azure μηχανικής εκμάθησης, Azure HDInsight και PowerBI. 

Καταιγισμό δεδομένων μπορεί να κατάποση με διανομείς συμβάν Azure καθώς προσφέρει κατάποσης δεδομένων υψηλής απόδοσης με χαμηλή λανθάνοντα χρόνο. Δεδομένα κατάποση που πρέπει να υποβάλλονται σε επεξεργασία για πληροφορίες πραγματικού χρόνου για να είναι funneled για ανάλυση ροή Azure για ανάλυση πραγματικό χρόνο. Δεδομένα μπορεί να μεταφερθεί σε DocumentDB για την υποβολή ερωτημάτων ad-hoc στο. Όταν τα δεδομένα φορτώνεται σε DocumentDB, τα δεδομένα είναι έτοιμη για να αναζητηθούν.  Τα δεδομένα στο DocumentDB μπορεί να χρησιμοποιηθεί ως δεδομένα αναφοράς ως μέρος της ανάλυσης πραγματικό χρόνο. Επιπλέον, δεδομένων να περαιτέρω είναι περιορισμένο και να υποβάλλονται σε επεξεργασία με τη σύνδεση δεδομένων DocumentDB σε HDInsight για γουρούνι, ομάδα ή χάρτη/μείωση εργασίες.  Εκλεπτυσμένη φόρτωση των δεδομένων είναι στη συνέχεια, επιστρέψτε στο DocumentDB για την αναφορά.   

Για μια λύση IoT δείγμα χρησιμοποιώντας DocumentDB, EventHubs και καταιγίδας, ανατρέξτε στο θέμα το [αρχείο φύλαξης hdinsight-καταιγίδας-παραδείγματα στο GitHub](https://github.com/hdinsight/hdinsight-storm-examples/).

Για περισσότερες πληροφορίες σχετικά με Azure προσφορές για IoT, ανατρέξτε στο θέμα [Δημιουργία το Internet σας στοιχεία](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx).

## <a name="next-steps"></a>Επόμενα βήματα
 
Για να ξεκινήσετε με DocumentDB, μπορείτε να δημιουργήσει ένα [λογαριασμό](https://azure.microsoft.com/pricing/free-trial/) και, στη συνέχεια, ακολουθήστε τη [διαδικασία εκμάθησης](https://azure.microsoft.com/documentation/learning-paths/documentdb/) για να μάθετε περισσότερα σχετικά με DocumentDB και να βρείτε τις πληροφορίες που χρειάζεστε. 

Ή, εάν θέλετε να διαβάσετε περισσότερα σχετικά με τους πελάτες που χρησιμοποιούν DocumentDB, τα παρακάτω πελατών ιστορίες είναι διαθέσιμες:

- [Επόμενη αγώνων](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Το νεκρού Walking: χωρίς Άντρας του Land παιχνίδι soars σε 1 # που υποστηρίζονται από το Azure DocumentDB.
- [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Πώς Halo 5 υλοποιηθεί κοινωνικών παιχνίδι χρησιμοποιώντας Azure DocumentDB.
- [Συλλογή Analytics Cortana](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Συλλογή Analytics Cortana - τοποθεσίας Κοινότητας με ενσωματωμένη σε Azure DocumentDB.
- [Παιχνιδάκι](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Αποτέλεσμα ολοκληρωτή παρέχει πληροφορίες για καθολική πολυεθνικές επιχειρήσεις σε λεπτά με τις τεχνολογίες ευέλικτοι Cloud.
- [Δημοκρατία συζητήσεων](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Προσθήκη πληροφοριών σε τις πληροφορίες για την παροχή πληροφοριών με σκοπό κατειλημμένο πολίτες. 
- [Διεθνής SGS](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Κύρια σήματα για συνεπή χρώμα σε ολόκληρο τον κόσμο, ενεργοποιήσετε SGS. Και SGS θα γίνει Azure.
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Καθολικό επικεφαλής Telenor χρησιμοποιεί στο cloud για να μετακινήσετε με την ταχύτητα της μιας εκκίνησης. 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Ο χώρος αποθήκευσης από το μέλλον εκτελείται σε ταχεία αναζήτηση και την εύκολη ροής δεδομένων.
