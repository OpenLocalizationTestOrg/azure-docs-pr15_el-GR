<properties
    pageTitle="Κρυπτογράφηση πλευρά του προγράμματος-πελάτη με Java για χώρο αποθήκευσης Azure | Microsoft Azure"
    description="Στη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για Java υποστηρίζει κρυπτογράφηση πλευρά του προγράμματος-πελάτη και ενοποίηση με το Azure θάλαμο αριθμού-κλειδιού για μέγιστη ασφάλεια για τις εφαρμογές σας χώρο αποθήκευσης Azure."
    services="storage"
    documentationCenter="java"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-java-for-microsoft-azure-storage"></a>Κρυπτογράφηση πλευρά του προγράμματος-πελάτη με Java για χώρο αποθήκευσης Azure   

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Επισκόπηση  

Η [Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης για Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) υποστηρίζει κρυπτογράφηση δεδομένων μέσα σε εφαρμογές προγράμματος-πελάτη πριν από την αποστολή στο χώρο αποθήκευσης Azure και αποκρυπτογράφηση δεδομένων κατά τη λήψη του στον υπολογιστή-πελάτη. Στη βιβλιοθήκη υποστηρίζει επίσης ενοποίηση με το [Azure θάλαμο αριθμού-κλειδιού](https://azure.microsoft.com/services/key-vault/) για τη Διαχείριση βασικών λογαριασμού χώρου αποθήκευσης.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Κρυπτογράφηση και αποκρυπτογράφηση μέσω την τεχνική φακέλου    
Οι διεργασίες κρυπτογράφηση και αποκρυπτογράφηση ακολουθήστε την τεχνική φακέλου.  

### <a name="encryption-via-the-envelope-technique"></a>Κρυπτογράφηση μέσω την τεχνική φακέλου  
Η κρυπτογράφηση μέσω του φακέλου τεχνική λειτουργεί με τον εξής τρόπο:  

1.  Στη βιβλιοθήκη του προγράμματος-πελάτη Azure αποθήκευσης δημιουργεί ένα κλειδί περιεχομένου κρυπτογράφησης (CEK), το οποίο είναι ένα πλήκτρο συμμετρική μία φορά χρήση.  

2.  Τα δεδομένα των χρηστών είναι κρυπτογραφημένη χρησιμοποιώντας αυτό CEK.   

3.  Το CEK, στη συνέχεια, αναδιπλώνεται (κρυπτογραφημένα) χρησιμοποιώντας το κλειδί κλειδιού κρυπτογράφησης (KEK). Το KEK προσδιορίζεται από ένα πλήκτρο αναγνωριστικό και μπορεί να είναι ένα ζεύγος κλειδιών ασύμμετρη ή έναν αριθμό-κλειδί συμμετρική και μπορεί να είναι διαχειριζόμενο τοπικά ή είναι αποθηκευμένα στο Azure χώροι φύλαξης αριθμού-κλειδιού.  
Ολόκληρη η βιβλιοθήκη προγράμματος-πελάτη αποθήκευσης ποτέ αποκτά πρόσβαση KEK. Η βιβλιοθήκη καλεί ο αλγόριθμος αναδίπλωσης κλειδιού που παρέχεται από θάλαμο αριθμού-κλειδιού. Οι χρήστες μπορούν να χρησιμοποιήσετε προσαρμοσμένες υπηρεσίες παροχής για κλειδιού αναδίπλωσης/κατάργηση αναδίπλωσης, εάν θέλετε.  

4.  Το κρυπτογραφημένων δεδομένων είναι στη συνέχεια, που έχουν αποσταλεί με την υπηρεσία αποθήκευσης Azure. Το κλειδί αναδιπλωμένο μαζί με ορισμένα μετα-δεδομένα επιπλέον κρυπτογράφησης αποθηκεύονται ως μετα-δεδομένα (σε ένα αντικείμενο blob) ή παρεμβολή με τα κρυπτογραφημένα δεδομένα (ουρά μηνυμάτων και οντοτήτων πίνακα).

### <a name="decryption-via-the-envelope-technique"></a>Αποκρυπτογράφηση μέσω την τεχνική φακέλου  
Αποκρυπτογράφηση μέσω του φακέλου τεχνική λειτουργεί με τον εξής τρόπο:  

1.  Στη βιβλιοθήκη του προγράμματος-πελάτη προϋποθέτει ότι ο χρήστης είναι διαχείριση του κλειδιού κλειδιού κρυπτογράφησης (KEK) τοπικά ή στον χώροι φύλαξης κλειδί Azure. Ο χρήστης δεν χρειάζεται να γνωρίζετε το συγκεκριμένο αριθμό-κλειδί που χρησιμοποιήθηκε για την κρυπτογράφηση. Αντί για αυτό, ενός κλειδιού επίλυσης που επιλύεται διαφορετικό κλειδιού αναγνωριστικά σε πλήκτρα μπορεί να ρυθμίσετε και να χρησιμοποιηθεί.  

2.  Στη βιβλιοθήκη του προγράμματος-πελάτη κάνει λήψη του κρυπτογραφημένων δεδομένων μαζί με οποιοδήποτε υλικό κρυπτογράφησης που είναι αποθηκευμένα στην υπηρεσία.  

3.  Το κλειδί αναδιπλωμένο περιεχομένου κρυπτογράφησης (CEK) είναι αποδεσμευτεί (αποκρυπτογράφηση) με χρήση του κλειδιού κρυπτογράφησης βασικές (KEK). Δείτε ξανά, τη βιβλιοθήκη προγράμματος-πελάτη δεν έχουν πρόσβαση στο KEK. Απλώς ενεργοποιεί την προσαρμοσμένη ή θάλαμο κλειδί της υπηρεσίας παροχής του αλγόριθμου unwrapping.  

4.  Του κλειδιού κρυπτογράφησης περιεχομένου (CEK) χρησιμοποιείται για να αποκρυπτογραφήσετε τα δεδομένα κρυπτογραφημένο χρήστη.

## <a name="encryption-mechanism"></a>Μηχανισμός κρυπτογράφησης  
Η βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης χρησιμοποιεί [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) προκειμένου να κρυπτογραφήσετε τα δεδομένα των χρηστών. Συγκεκριμένα, [Κρυπτογράφηση μπλοκ αλυσιδωτή (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) λειτουργία με AES. Κάθε υπηρεσία λειτουργεί κάπως διαφορετικά, έτσι θα αναλύονται κάθε έναν από τους εδώ.

### <a name="blobs"></a>Αντικείμενα BLOB  
Στη βιβλιοθήκη του προγράμματος-πελάτη υποστηρίζει επί του παρόντος κρυπτογράφησης από ολόκληρο αντικείμενα blob. Συγκεκριμένα, κρυπτογράφηση υποστηρίζεται όταν οι χρήστες χρησιμοποιούν την **Αποστολή** * μεθόδους ή το * *openOutputStream** μέθοδο. Για στοιχεία λήψης, και τα δύο ολοκλήρωσης και στοιχεία λήψης περιοχή υποστηρίζονται.  

Κατά τη διάρκεια της κρυπτογράφησης, τη βιβλιοθήκη προγράμματος-πελάτη θα δημιουργήσετε μια τυχαία ανύσματος προετοιμασίας (IV) 16 byte, μαζί με ένα κλειδί τυχαία περιεχομένου κρυπτογράφησης (CEK) 32 byte, και να εκτελέσει φακέλου κρυπτογράφηση των δεδομένων blob χρησιμοποιώντας αυτές τις πληροφορίες. Το αναδιπλωμένο CEK και ορισμένα μετα-δεδομένα επιπλέον κρυπτογράφησης αποθηκεύονται, στη συνέχεια, όπως αντικειμένων blob μετα-δεδομένων μαζί με το κρυπτογραφημένο blob στην υπηρεσία.

>[AZURE.WARNING] Εάν επεξεργάζεστε ή αποστολή τη δική σας μετα-δεδομένων για το αντικείμενο blob, πρέπει να βεβαιωθείτε ότι διατηρούνται αυτό μετα-δεδομένων. Εάν κάνετε αποστολή νέου μετα-δεδομένων χωρίς αυτό μετα-δεδομένων, το αναδιπλωμένο CEK, IV και άλλα μετα-δεδομένα θα χαθούν και το περιεχόμενο blob ποτέ θα ανακτήσιμη ξανά.

Λήψη ένα κρυπτογραφημένο αντικείμενο blob περιλαμβάνει την ανάκτηση του περιεχομένου από το ολόκληρο blob χρησιμοποιώντας τη * *λήψη*/openInputStream** ευκολία μεθόδους. Το αναδιπλωμένο CEK είναι περισσοτέρων και να χρησιμοποιηθεί μαζί με το IV (έχουν αποθηκευτεί ως blob μετα-δεδομένων σε αυτήν την περίπτωση) για να επιστρέψει την αποκρυπτογράφηση δεδομένων στους χρήστες.

Λήψη μιας περιοχής αυθαίρετο (**downloadRange*** μεθόδους) στο κρυπτογραφημένο αντικείμενο blob περιλαμβάνει την προσαρμογή της περιοχής που παρέχεται από τους χρήστες για να λάβετε μια μικρή ποσότητα πρόσθετα δεδομένα που μπορούν να χρησιμοποιηθούν για να αποκρυπτογραφήσετε με επιτυχία η περιοχή που ζητήθηκε.  

Όλα αντικειμένων blob τύποι (αποκλεισμός αντικείμενα BLOB σελίδα αντικείμενα BLOB και προσάρτηση αντικείμενα BLOB) μπορεί να είναι Κρυπτογράφηση/αποκρυπτογράφηση χρησιμοποιώντας αυτό το σχήμα.

### <a name="queues"></a>Ουρές  
Εφόσον ουρά μηνυμάτων μπορεί να είναι από οποιαδήποτε μορφή, τη βιβλιοθήκη προγράμματος-πελάτη καθορίζει μια προσαρμοσμένη μορφή που περιλαμβάνει την προετοιμασία ανύσματος IV και το πλήκτρο κρυπτογραφημένο περιεχομένου κρυπτογράφησης (CEK) στο κείμενο του μηνύματος.  

Κατά τη διάρκεια της κρυπτογράφησης, η βιβλιοθήκη προγράμματος-πελάτη δημιουργεί έναν τυχαίο IV του 16 byte μαζί με μια τυχαία CEK byte 32 και εκτελεί κρυπτογράφησης φακέλου από το κείμενο του μηνύματος ουρά χρησιμοποιώντας αυτές τις πληροφορίες. Το αναδιπλωμένο CEK και ορισμένα μετα-δεδομένα επιπλέον κρυπτογράφησης, στη συνέχεια, προστίθενται στο ουρά κρυπτογραφημένο μήνυμα. Αυτό το μήνυμα έχουν τροποποιηθεί (απεικονίζεται παρακάτω) είναι αποθηκευμένη στην υπηρεσία.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Κατά τη διάρκεια αποκρυπτογράφηση, τον αριθμό-κλειδί αναδιπλωμένο είναι που έχουν εξαχθεί από το μήνυμα ουρά και να περισσοτέρων. Το IV είναι επίσης να που έχουν εξαχθεί από το μήνυμα ουρά και να χρησιμοποιηθεί μαζί με τον αριθμό-κλειδί αποδεσμευτεί να αποκρυπτογραφήσετε τα δεδομένα μήνυμα ουρά. Σημειώστε ότι τα μετα-δεδομένα κρυπτογράφησης είναι μικρές (στην περιοχή 500 byte), ώστε να ενώ το μετρήσετε προς το όριο των 64KB για ένα μήνυμα ουρά, την επίδραση πρέπει να διαχειρίσιμα.

### <a name="tables"></a>Πίνακες  
Η βιβλιοθήκη προγράμματος-πελάτη υποστηρίζει κρυπτογράφηση οντότητα ιδιοτήτων για εισαγωγή και αντικατάσταση λειτουργίες.

>[AZURE.NOTE] Συγχώνευση δεν υποστηρίζεται αυτήν τη στιγμή. Επειδή ένα υποσύνολο των ιδιοτήτων ενδέχεται να έχουν τεθεί κρυπτογραφημένα προηγουμένως, χρησιμοποιώντας έναν διαφορετικό αριθμό-κλειδί, απλώς τη συγχώνευση οι νέες ιδιότητες και την ενημέρωση των μετα-δεδομένων θα έχει ως αποτέλεσμα απώλεια δεδομένων. Συγχώνευση είτε απαιτεί να πραγματοποιήσετε κλήσεις επιπλέον υπηρεσίας για να διαβάσετε την προ-υπαρχόντων οντότητα από την υπηρεσία ή χρησιμοποιώντας έναν νέο αριθμό-κλειδί ανά ιδιότητα, οι οποίες δεν είναι κατάλληλη για λόγους απόδοσης.

Κρυπτογράφηση δεδομένων πίνακα λειτουργεί ως εξής:  

1.  Οι χρήστες καθορίζουν τις ιδιότητες για να είναι κρυπτογραφημένα.  

2.  Στη βιβλιοθήκη του προγράμματος-πελάτη δημιουργεί μια τυχαία ανύσματος προετοιμασίας (IV) 16 byte μαζί με ένα κλειδί κρυπτογράφησης τυχαία περιεχομένου (CEK) byte 32 για κάθε οντότητα, και εκτελεί κρυπτογράφησης φακέλου σε τις μεμονωμένες ιδιότητες για να είναι κρυπτογραφημένα με που προκύπτει ένα νέο IV ανά ιδιότητα. Η ιδιότητα κρυπτογραφημένο αποθηκεύονται ως δυαδικά δεδομένα.  

3.  Το αναδιπλωμένο CEK και ορισμένα μετα-δεδομένα πρόσθετης κρυπτογράφησης, στη συνέχεια, αποθηκεύονται ως δύο πρόσθετες ιδιότητες δεσμευμένη. Στην πρώτη ιδιότητα δεσμευμένη (_ClientEncryptionMetadata1) είναι μια ιδιότητα συμβολοσειρά που περιέχει τις πληροφορίες σχετικά με το IV, την έκδοση και αναδιπλωμένο κλειδί. Η δεύτερη δεσμευμένη ιδιότητα (_ClientEncryptionMetadata2) είναι μια δυαδική ιδιότητα που περιέχει τις πληροφορίες σχετικά με τις ιδιότητες που είναι κρυπτογραφημένα. Οι πληροφορίες σε αυτήν την ιδιότητα δεύτερο (_ClientEncryptionMetadata2) είναι ίδιο κρυπτογραφημένα.  

4.  Λόγω αυτές τις πρόσθετες δεσμευμένες ιδιότητες που απαιτούνται για την κρυπτογράφηση, οι χρήστες τώρα μπορεί να έχει μόνο 250 προσαρμοσμένες ιδιότητες αντί για 252. Το συνολικό μέγεθος της οντότητας πρέπει να είναι λιγότερο από 1MB.  

    Σημειώστε ότι μόνο οι ιδιότητες συμβολοσειρών μπορούν να είναι κρυπτογραφημένα. Εάν οι άλλοι τύποι ιδιοτήτων είναι κρυπτογράφησης, πρέπει να μετατρέπονται σε συμβολοσειρές. Το κρυπτογραφημένο συμβολοσειρές που είναι αποθηκευμένα στην υπηρεσία ως δυαδικό ιδιότητες και μετατρέπονται σε συμβολοσειρές μετά την αποκρυπτογράφηση.

    Για πίνακες, εκτός από την πολιτική κρυπτογράφησης, οι χρήστες πρέπει να καθορίσετε τις ιδιότητες για να είναι κρυπτογραφημένο. Αυτό μπορεί να γίνει καθορίζοντας είτε ένα χαρακτηριστικό [κρυπτογράφηση] (για POCO οντοτήτων που προέρχεται από TableEntity) ή ένα επίλυσης κρυπτογράφησης στις επιλογές αίτηση. Μια επίλυσης κρυπτογράφησης είναι ένας πληρεξούσιος που λαμβάνει ένα πλήκτρο διαμερίσματα, γραμμή κλειδί και όνομα ιδιότητας και επιστρέφει μια τιμή boolean που υποδεικνύει εάν θα πρέπει να κρυπτογραφούνται αυτήν την ιδιότητα. Κατά τη διάρκεια της κρυπτογράφησης, η βιβλιοθήκη προγράμματος-πελάτη θα χρησιμοποιήσει αυτές τις πληροφορίες για να αποφασίσετε εάν θα πρέπει να κρυπτογραφούνται μια ιδιότητα κατά την εγγραφή σύρματος. Ο πληρεξούσιος παρέχει επίσης τη δυνατότητα της λογικής γύρω από τον τρόπο που είναι κρυπτογραφημένα ιδιότητες. (Για παράδειγμα, εάν X, στη συνέχεια κρυπτογράφηση ιδιότητα A; διαφορετικά κρυπτογράφηση ιδιότητες Α και β.) Σημειώστε ότι δεν είναι απαραίτητο να δώσετε αυτές τις πληροφορίες κατά την ανάγνωση ή την υποβολή ερωτημάτων οντοτήτων.

### <a name="batch-operations"></a>Λειτουργίες δέσμης  
Λειτουργίες μαζικής επεξεργασίας, το ίδιο KEK θα χρησιμοποιούνται σε όλες τις γραμμές σε αυτήν την λειτουργία δέσμη επειδή στη βιβλιοθήκη του προγράμματος-πελάτη επιτρέπει μόνο ένα αντικείμενο επιλογές (και συνεπώς μία πολιτική/KEK) ανά λειτουργίας δέσμης. Ωστόσο, τη βιβλιοθήκη προγράμματος-πελάτη θα εσωτερικά δημιουργήσει μια νέα τυχαίες IV και τυχαίες CEK ανά γραμμή στη δέσμη. Οι χρήστες μπορούν επίσης να κρυπτογραφήσετε διαφορετικές ιδιότητες για κάθε λειτουργία στη δέσμη ορίζοντας αυτήν τη συμπεριφορά στα η επίλυση κρυπτογράφησης.

### <a name="queries"></a>Ερωτήματα  
Για να εκτελέσετε λειτουργίες ερωτήματος, πρέπει να καθορίσετε ένα πλήκτρο επίλυσης που είναι σε θέση να επιλύσετε όλα τα πλήκτρα στο σύνολο αποτελεσμάτων. Εάν μια οντότητα που περιέχονται στο αποτέλεσμα του ερωτήματος δεν μπορεί να επιλυθεί σε μια υπηρεσία παροχής, τη βιβλιοθήκη προγράμματος-πελάτη θα εμφανίσουν ένα σφάλμα. Για οποιοδήποτε ερώτημα που εκτελεί προβλέψεις πλευρά του διακομιστή, τη βιβλιοθήκη προγράμματος-πελάτη θα προσθέσει τις ιδιότητες μετα-δεδομένων ειδική κρυπτογράφησης (_ClientEncryptionMetadata1 και _ClientEncryptionMetadata2) από προεπιλογή στις επιλεγμένες στήλες.

## <a name="azure-key-vault"></a>Azure θάλαμο κλειδιού  
Azure θάλαμο αριθμού-κλειδιού βοηθά στη διαφύλαξη κλειδιών κρυπτογράφησης και απορρήτου που χρησιμοποιείται από το cloud εφαρμογών και υπηρεσιών. Με τη χρήση θάλαμο κλειδί Azure, οι χρήστες να κρυπτογραφήσετε πλήκτρα και απορρήτου (όπως κλειδιά ελέγχου ταυτότητας, πλήκτρα λογαριασμού χώρου αποθήκευσης, κλειδιά κρυπτογράφησης δεδομένων. PFX αρχεία και τους κωδικούς πρόσβασης), χρησιμοποιώντας τα πλήκτρα που προστατεύονται από λειτουργικές μονάδες ασφαλείας υλικού (HSMs). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι το Azure κλειδί θάλαμο;](../key-vault/key-vault-whatis.md).

Στη βιβλιοθήκη του προγράμματος-πελάτη χώρου αποθήκευσης χρησιμοποιεί τη βιβλιοθήκη πυρήνα θάλαμο αριθμού-κλειδιού για να παρέχουν ένα κοινό πλαίσιο Azure για τη διαχείριση των αριθμών-κλειδιών. Χρήστες έχουν επίσης το επιπλέον πλεονέκτημα της χρήσης της βιβλιοθήκης επεκτάσεις θάλαμο αριθμού-κλειδιού. Η βιβλιοθήκη επεκτάσεις παρέχει χρήσιμες λειτουργίες γύρω από απλές και απρόσκοπτη Symmetric/RSA τοπικό και υπηρεσίες παροχής κλειδιού cloud, καθώς και με συνάθροισης και σε cache.

### <a name="interface-and-dependencies"></a>Περιβάλλον εργασίας και εξαρτήσεις  
Υπάρχουν τρεις πακέτων θάλαμο αριθμού-κλειδιού:  

- Azure keyvault πυρήνα περιλαμβάνει την IKey και IKeyResolver. Είναι ένα μικρό πακέτο με χωρίς εξαρτήσεις. Στη βιβλιοθήκη του προγράμματος-πελάτη χώρου αποθήκευσης για Java Καθορίζει το ως εξάρτηση.
- Azure keyvault περιέχει το πρόγραμμα-πελάτη ΥΠΌΛΟΙΠΑ θάλαμο αριθμού-κλειδιού.  
- Azure-keyvault-επεκτάσεις περιέχει κώδικα επέκτασης που περιλαμβάνει υλοποιήσεις αλγορίθμους κρυπτογράφησης και ένα RSAKey και ένα SymmetricKey. Αυτό εξαρτάται από το μέγεθος των κύριων και KeyVault χώροι ονομάτων και παρέχει λειτουργικότητα για να ορίσετε μια συγκεντρωτικών αποτελεσμάτων επίλυσης (όταν οι χρήστες που θέλετε να χρησιμοποιήσετε πολλές υπηρεσίες παροχής κλειδιού) και ένα πλήκτρο επίλυση στη μνήμη cache. Παρόλο που στη βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης δεν απευθείας εξαρτώνται από αυτό το πακέτο, εάν οι χρήστες που θέλετε να χρησιμοποιήσετε Azure θάλαμο αριθμού-κλειδιού για να αποθηκεύσετε τους αριθμούς-κλειδιά ή να χρησιμοποιήσετε τις επεκτάσεις θάλαμο αριθμού-κλειδιού για εκμετάλλευση την τοπική και στο cloud παροχής κρυπτογράφησης, αυτά θα χρειαστεί αυτό το πακέτο.  

  Πλήκτρο θάλαμο έχει σχεδιαστεί για υψηλή τιμή πρωτεύοντα κλειδιά και επιτάχυνσης όρια ανά κλειδί θάλαμο έχουν σχεδιαστεί με το εξής υπόψη. Κατά την εκτέλεση κρυπτογράφησης πλευρά του προγράμματος-πελάτη με αριθμό-κλειδί θάλαμο, το μοντέλο προτιμώμενη είναι να χρησιμοποιήσετε τοπικά συμμετρική πρωτεύοντα κλειδιά αποθηκεύονται ως απόρρητο στο θάλαμο κλειδί και στο cache. Οι χρήστες πρέπει να κάνετε τα εξής:  

1.  Δημιουργήστε ένα μυστικό χωρίς σύνδεση και στείλτε το στο θάλαμο αριθμού-κλειδιού.  

2.  Χρησιμοποιήστε το μυστικό βάσης αναγνωριστικό ως παράμετρο, για να επιλύσετε την τρέχουσα έκδοση του το μυστικό της κρυπτογράφησης και προσωρινή αποθήκευση τοπικά αυτές τις πληροφορίες. Χρήση CachingKeyResolver για προσωρινή αποθήκευση; Οι χρήστες δεν αναμένεται για την υλοποίηση μόνος του σε cache λογικής.  

3.  Χρησιμοποιήστε την επίλυση στη μνήμη cache ως εισαγωγή κατά τη δημιουργία της πολιτικής κρυπτογράφησης.
Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με τη χρήση αριθμού-κλειδιού θάλαμο στα δείγματα κώδικα κρυπτογράφησης. <fix URL>  

## <a name="best-practices"></a>Βέλτιστες πρακτικές  
Υποστήριξη κρυπτογράφησης είναι διαθέσιμη μόνο στη βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για Java.

>[AZURE.IMPORTANT] Λάβετε υπόψη τα παρακάτω σημαντικά σημεία κατά τη χρήση της κρυπτογράφησης πλευρά του προγράμματος-πελάτη:
>  
>- Κατά την ανάγνωση ή την εγγραφή σε ένα κρυπτογραφημένο αντικείμενο blob, χρησιμοποιήστε ολόκληρη blob αποστολής και εντολές λήψη blob περιοχή/ακέραιος. Αποφύγετε την εγγραφή σε ένα κρυπτογραφημένο αντικείμενο blob χρήση λειτουργιών πρωτόκολλο όπως τοποθέτηση μπλοκ, τοποθέτηση λίστα μπλοκ, γράψτε σελίδες, καταργήστε σελίδες ή προσάρτηση μπλοκ: Διαφορετικά που μπορεί να καταστρέψει το κρυπτογραφημένο blob και να είναι δυνατή η ανάγνωση.
>
>- Για πίνακες, υπάρχει μια παρόμοια περιορισμού. Να είστε προσεκτικοί για να ενημερώσετε δεν κρυπτογραφημένο ιδιότητες χωρίς να ενημερώσετε τα μετα-δεδομένα κρυπτογράφησης.  
>
>- Εάν ορίσετε μετα-δεδομένων σε το κρυπτογραφημένο blob, ενδέχεται να μπορείτε να αντικαταστήσετε το που σχετίζονται με την κρυπτογράφηση μετα-δεδομένων απαιτείται για την αποκρυπτογράφηση, επειδή ο ορισμός μετα-δεδομένων δεν είναι προσθετικά. Αυτό ισχύει επίσης για στιγμιότυπα; αποφύγετε καθορίζοντας μετα-δεδομένων κατά τη δημιουργία ένα στιγμιότυπο του ένα κρυπτογραφημένο αντικείμενο blob. Εάν πρέπει να οριστούν μετα-δεδομένων, βεβαιωθείτε ότι η κλήση της μεθόδου **downloadAttributes** πρώτα, για να λάβετε την τρέχουσα μετα-δεδομένα κρυπτογράφησης, και να αποφύγετε ταυτόχρονες εγγραφές ενώ ορίζεται μετα-δεδομένων.  
>
>- Ενεργοποίηση της σημαίας **requireEncryption** στο τις προεπιλεγμένες επιλογές αίτηση για τους χρήστες που θα πρέπει να λειτουργούν μόνο με κρυπτογραφημένων δεδομένων. Δείτε παρακάτω για περισσότερες πληροφορίες.  

## <a name="client-api--interface"></a>API του προγράμματος-πελάτη / περιβάλλοντος εργασίας  
Κατά τη δημιουργία ενός αντικειμένου EncryptionPolicy, οι χρήστες μπορούν να παρέχουν μόνο έναν αριθμό-κλειδί (εφαρμογής IKey), μόνο μια επίλυση (εφαρμογής IKeyResolver) ή και τα δύο. IKey είναι ο τύπος του βασικού κλειδιού που προσδιορίζεται χρησιμοποιώντας ένα πλήκτρο αναγνωριστικό και που παρέχει τη λογική για αναδίπλωση/κατάργηση αναδίπλωσης. IKeyResolver χρησιμοποιείται για την επίλυση ενός κλειδιού κατά τη διαδικασία αποκρυπτογράφησης. Καθορίζει μια μέθοδο ResolveKey που επιστρέφει μια IKey δεδομένο ένα αναγνωριστικό κλειδιού. Αυτό παρέχει στους χρήστες τη δυνατότητα να επιλέξετε μεταξύ πολλών πλήκτρων που πραγματοποιείται σε πολλές θέσεις.

- Για την κρυπτογράφηση, χρησιμοποιείται πάντα το κλειδί και την απουσία έναν αριθμό-κλειδί θα έχει ως αποτέλεσμα σφάλμα.  
- Για αποκρυπτογράφηση:  
    - Η επίλυση κλειδιού, ενεργοποιείται εάν καθοριστεί για να λάβετε τον αριθμό-κλειδί. Εάν η επίλυση έχει καθοριστεί, αλλά δεν έχει μια αντιστοίχιση για το αναγνωριστικό του κλειδιού, παρουσιαστεί σφάλμα.  
    - Εάν δεν έχει καθοριστεί επίλυσης αλλά έχει οριστεί έναν αριθμό-κλειδί, το πλήκτρο χρησιμοποιείται εάν το αναγνωριστικό συμφωνεί με το αναγνωριστικό του κλειδιού απαιτείται. Εάν το αναγνωριστικό δεν συμφωνούν, παρουσιαστεί σφάλμα.  

      Τα [δείγματα κρυπτογράφησης](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) <fix URL>δείχνουν μια πιο λεπτομερή τέλος-σεναρίου για αντικείμενα blob, ουρές και πίνακες, μαζί με ενσωμάτωση του αριθμού-κλειδιού θάλαμο.

### <a name="requireencryption-mode"></a>Λειτουργία RequireEncryption  
Οι χρήστες μπορούν να ενεργοποιήσουν προαιρετικά λειτουργίας όπου πρέπει να είναι κρυπτογραφημένα όλες τις αποστολές και λήψεις. Σε αυτήν τη λειτουργία προσπάθειες δεδομένα χωρίς μια πολιτική κρυπτογράφησης η αποστολή ή λήψη δεδομένων που δεν είναι κρυπτογραφημένα στην υπηρεσία θα αποτύχουν του υπολογιστή-πελάτη. Σημαία **requireEncryption** το αντικείμενο επιλογές αίτηση ελέγχου αυτήν τη συμπεριφορά. Εάν η εφαρμογή σας θα κρυπτογράφηση όλων των αντικειμένων που είναι αποθηκευμένα στο χώρο αποθήκευσης Azure, στη συνέχεια, μπορείτε να ορίσετε την ιδιότητα **requireEncryption** στην τις προεπιλεγμένες επιλογές αίτηση για το αντικείμενο υπηρεσίας προγράμματος-πελάτη.   

Για παράδειγμα, μπορείτε να χρησιμοποιήσετε **CloudBlobClient.getDefaultRequestOptions().setRequireEncryption(true)** ώστε να απαιτεί κρυπτογράφηση για όλες τις εργασίες blob που εκτελούνται σε αυτό το αντικείμενο προγράμματος-πελάτη.

### <a name="blob-service-encryption"></a>Κρυπτογράφηση υπηρεσίας BLOB  
Δημιουργία ενός αντικειμένου **BlobEncryptionPolicy** και ορίστε την στην πρόσκληση σε επιλογών (ανά API ή σε επίπεδο προγράμματος-πελάτη με τη χρήση **DefaultRequestOptions**). Οτιδήποτε άλλο θα γίνεται ο χειρισμός από τη βιβλιοθήκη του προγράμματος-πελάτη εσωτερικά.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions();
    options.setEncryptionPolicy(policy);

    // Upload the encrypted contents to the blob.
    blob.upload(stream, size, null, options, null);

    // Download and decrypt the encrypted contents from the blob.
    ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
    blob.download(outputStream, null, options, null);

### <a name="queue-service-encryption"></a>Κρυπτογράφηση της υπηρεσίας Ουράς  
Δημιουργία ενός αντικειμένου **QueueEncryptionPolicy** και ορίστε την στις επιλογές αίτηση (ανά API ή σε επίπεδο προγράμματος-πελάτη με τη χρήση **DefaultRequestOptions**). Οτιδήποτε άλλο θα γίνεται ο χειρισμός από τη βιβλιοθήκη του προγράμματος-πελάτη εσωτερικά.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

    // Add message
    QueueRequestOptions options = new QueueRequestOptions();
    options.setEncryptionPolicy(policy);

    queue.addMessage(message, 0, 0, options, null);

    // Retrieve message
    CloudQueueMessage retrMessage = queue.retrieveMessage(30, options, null);

### <a name="table-service-encryption"></a>Κρυπτογράφηση υπηρεσίας πίνακα  
Εκτός από τη δημιουργία μιας πολιτικής κρυπτογράφησης και τον ορισμό της σχετικά με τις επιλογές αίτηση, μπορείτε πρέπει να καθορίσετε μια **EncryptionResolver** στο **TableRequestOptions**ή να ορίσετε το χαρακτηριστικό [κρυπτογράφηση] στο αποτέλεσμα την και setter η οντότητα.

### <a name="using-the-resolver"></a>Χρησιμοποιώντας την επίλυση  

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

    TableRequestOptions options = new TableRequestOptions()
    options.setEncryptionPolicy(policy);
    options.setEncryptionResolver(new EncryptionResolver() {
        public boolean encryptionResolver(String pk, String rk, String key) {
            if (key == "foo")
            {
                return true;
            }
            return false;
        }
    });

    // Insert Entity
    currentTable.execute(TableOperation.insert(ent), options, null);

    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions()
    retrieveOptions.setEncryptionPolicy(policy);

    TableOperation operation = TableOperation.retrieve(ent.PartitionKey, ent.RowKey, DynamicTableEntity.class);
    TableResult result = currentTable.execute(operation, retrieveOptions, null);

### <a name="using-attributes"></a>Χρήση χαρακτηριστικά  
Όπως προαναφέρθηκε, εάν η οντότητα εφαρμόζει TableEntity, στη συνέχεια, το αποτέλεσμα την ιδιότητες και setter μπορεί να διαθέτει το χαρακτηριστικό [κρυπτογράφηση] αντί για τον καθορισμό του **EncryptionResolver**.

    private string encryptedProperty1;

    @Encrypt
    public String getEncryptedProperty1 () {
        return this.encryptedProperty1;
    }

    @Encrypt
    public void setEncryptedProperty1(final String encryptedProperty1) {
        this.encryptedProperty1 = encryptedProperty1;
    }

## <a name="encryption-and-performance"></a>Κρυπτογράφηση και επιδόσεων  
Λάβετε υπόψη ότι κρυπτογράφηση τα αποτελέσματα δεδομένων χώρου αποθήκευσης στο επιβάρυνσης επιπλέον επιδόσεων. Πρέπει να έχει δημιουργηθεί το περιεχόμενο κλειδί και IV, το περιεχόμενο μόνο πρέπει να είναι κρυπτογραφημένες και πρόσθετες μετα-δεδομένα πρέπει να είναι μορφοποιημένο και να αποστείλει. Αυτό επιβάρυνσης θα ποικίλλουν ανάλογα με την ποσότητα των δεδομένων που είναι κρυπτογραφημένα. Συνιστάται να ότι οι πελάτες δοκιμή πάντα των αιτήσεων για απόδοσης κατά την ανάπτυξη.

## <a name="next-steps"></a>Επόμενα βήματα  

- Κάντε λήψη του [Azure βιβλιοθήκη προγράμματος-πελάτη χώρου αποθήκευσης για το πακέτο Java Maven](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)  
- Κάντε λήψη του [Azure αποθήκευσης βιβλιοθήκη προγράμματος-πελάτη για Java πηγαίου κώδικα από GitHub](https://github.com/Azure/azure-storage-java)   
- Λήψη στη βιβλιοθήκη Maven Azure κλειδί θάλαμο για Java Maven πακέτα:
    - [Βασικό](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault-core) πακέτο
    - Πακέτο [προγράμματος-πελάτη](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault)
- Επισκεφθείτε το [κλειδί Azure φύλαξης τεκμηρίωση](../key-vault/key-vault-whatis.md)  
