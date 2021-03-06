<properties
    pageTitle="Διαχείριση συγχρονισμού στο χώρο αποθήκευσης του Windows Azure"
    description="Πώς μπορείτε να διαχειριστείτε ταυτόχρονης εκτέλεσης για τις υπηρεσίες Blob, ουρά, πίνακα και αρχείων"
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jahogg"/>

# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Διαχείριση συγχρονισμού στο χώρο αποθήκευσης του Windows Azure

## <a name="overview"></a>Επισκόπηση

Μοντέρνα εφαρμογές που βασίζονται στο Internet συνήθως υπάρχουν πολλοί χρήστες προβολή και ενημέρωση δεδομένων ταυτόχρονα. Αυτό απαιτεί προγραμματιστές εφαρμογών για να σκεφτείτε προσεκτικά σχετικά με τον τρόπο για την παροχή μια προβλέψιμα εμπειρία για τους τελικούς χρήστες, ιδιαίτερα σενάρια όπου πολλούς χρήστες να ενημερώσετε τα ίδια δεδομένα. Υπάρχουν τρία κύρια δεδομένα ταυτόχρονης εκτέλεσης στρατηγικές προγραμματιστές συνήθως θα λάβετε υπόψη:  


1.  Βέλτιστου – τελευταία μια εφαρμογή που εκτελούν μια ενημέρωση θα ως μέρος της ενημέρωσης το επαληθεύσετε εάν έχει αλλάξει και τα δεδομένα από την εφαρμογή ανάγνωση αυτών των δεδομένων. Για παράδειγμα, εάν δύο χρήστες που προβάλλουν σε σελίδα wiki, κάντε μια ενημέρωση στην ίδια σελίδα, στη συνέχεια, την πλατφόρμα wiki πρέπει να βεβαιωθείτε ότι η δεύτερη ενημερωμένη έκδοση δεν αντικαθιστά την πρώτη ενημερωμένη έκδοση- και ότι και οι δύο οι χρήστες κατανοούν εάν τους ενημέρωση ολοκληρώθηκε με επιτυχία ή όχι. Αυτή η στρατηγική χρησιμοποιείται πιο συχνά στις εφαρμογές web.
2.  Απαισιόδοξη ταυτόχρονης εκτέλεσης – μια εφαρμογή που θέλετε να εκτελέσετε μια ενημέρωση θα χρειαστούν ένα κλείδωμα σε ένα αντικείμενο αποτρέπει την ενημέρωση των δεδομένων μέχρι να κυκλοφορήσει το κλείδωμα άλλους χρήστες. Για παράδειγμα, σε ένα σενάριο αναπαραγωγή δεδομένων κύριες/εξαρτώμενες όπου μόνο το πρωτότυπο θα εκτελέσει ενημερώσεις στο υπόδειγμα θα συνήθως κρατήστε πατημένο το πλήκτρο αποκλειστικό κλείδωμα για ένα εκτεταμένο χρονικό διάστημα τα δεδομένα για να βεβαιωθείτε ότι κανένας άλλος να την ενημερώσετε.
3.  Την τελευταία εγγραφή κερδίζει – προσέγγισης που επιτρέπει τις λειτουργίες update για να συνεχίσετε χωρίς να επαληθεύσετε εάν οποιαδήποτε άλλη εφαρμογή έχει ενημερωθεί τα δεδομένα, από την εφαρμογή, πρώτα διαβάζει τα δεδομένα. Αυτό στρατηγικής (ή έλλειψης μια επίσημη στρατηγική) χρησιμοποιείται συνήθως όπου έχει διαμερίσματα δεδομένων έτσι ότι δεν υπάρχει καμία πιθανότητα ότι θα πρόσβαση των ίδιων δεδομένων σε πολλούς χρήστες. Μπορεί να είναι χρήσιμη όταν γίνεται επεξεργασία ροών μικρής διάρκειας δεδομένων.  

Σε αυτό το άρθρο παρέχει μια επισκόπηση του τρόπου της πλατφόρμας αποθήκευσης Azure απλοποιεί την ανάπτυξη, παρέχοντας κορυφαία υποστήριξη για τις τρεις από τις παρακάτω στρατηγικές ταυτόχρονης εκτέλεσης.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure αποθήκευσης – απλοποιεί την ανάπτυξη Cloud
Η υπηρεσία αποθήκευσης Azure υποστηρίζει όλες τις τρεις στρατηγικές, παρόλο που είναι χαρακτηριστικό δυνατότητες για την παροχή πλήρη υποστήριξη αισιόδοξες και απαισιόδοξες ταυτόχρονης εκτέλεσης επειδή σχεδιάστηκε η αξιοποίηση μοντέλου ισχυρό συνέπειας που εγγυάται ότι, όταν η υπηρεσία αποθήκευσης δεσμεύεται μια εισαγωγή δεδομένων ή λειτουργία ενημέρωσης όλων περαιτέρω αποκτά πρόσβαση σε ότι τα δεδομένα θα δείτε την πιο πρόσφατη ενημέρωση. Πλατφόρμες χώρου αποθήκευσης που χρησιμοποιούν ένα μοντέλο ενδεχόμενη συνέπειας έχουν μια καθυστέρηση μεταξύ όταν εκτελείται μια εγγραφή από ένα χρήστη και όταν τα ενημερωμένα δεδομένα μπορούν να προβληθούν από άλλους χρήστες, επομένως, περιπλέκοντας ανάπτυξη εφαρμογών προγράμματος-πελάτη για να αποτρέψετε ασυνέπειες να επηρεαστούν οι τελικοί χρήστες.  

Εκτός από την επιλογή μιας στρατηγικής κατάλληλο ταυτόχρονης εκτέλεσης προγραμματιστές πρέπει επίσης να γνωρίζουν πώς μια πλατφόρμα αποθήκευσης απομονώνει αλλαγές – ιδιαίτερα αλλαγές στο ίδιο αντικείμενο σε συναλλαγές. Η υπηρεσία αποθήκευσης Azure χρησιμοποιεί απομόνωσης στιγμιότυπο για να επιτρέψετε σε λειτουργίες ανάγνωσης για να συμβεί ταυτόχρονα με λειτουργίες εγγραφής μέσα σε ένα μεμονωμένο διαμερίσματα. Σε αντίθεση με άλλα επίπεδα απομόνωσης, απομόνωσης στιγμιότυπο εγγυάται ότι όλα διαβάζει δείτε μια συνεπή στιγμιότυπο των δεδομένων, ακόμα και όταν παρουσιάζονται ενημερώσεις – ουσιαστικά, επιστρέφοντας την τελευταία δεσμευμένη τιμή κατά την ενημέρωση συναλλαγής υποβάλλεται σε επεξεργασία.  

## <a name="managing-concurrency-in-blob-storage"></a>Διαχείριση συγχρονισμού στο χώρο αποθήκευσης αντικειμένων Blob
Μπορείτε να επιλέξετε να χρησιμοποιήσετε είτε μοντέλα αισιόδοξη ή απαισιόδοξη ταυτόχρονης εκτέλεσης για να διαχειριστείτε την πρόσβαση στα αντικείμενα BLOB και κοντέινερ στην υπηρεσία blob. Εάν δεν καθορίσετε ρητά wins εγγραφές τελευταία στρατηγικής είναι η προεπιλεγμένη.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Βέλτιστου για κοντέινερ και αντικείμενα BLOB  

Η υπηρεσία αποθήκευσης αντιστοιχίζει ένα αναγνωριστικό σε κάθε αντικείμενο που είναι αποθηκευμένο. Αυτό το αναγνωριστικό ενημερώνεται κάθε φορά που εκτελείται μια λειτουργία ενημέρωσης σε ένα αντικείμενο. Το αναγνωριστικό επιστρέφεται στον υπολογιστή-πελάτη ως μέρος μιας απόκρισης HTTP GET χρησιμοποιώντας την κεφαλίδα (tag οντότητας) ETag που καθορίζεται εντός του πρωτοκόλλου HTTP. Ένας χρήστης που εκτελεί μια ενημέρωση σε όπως ένα αντικείμενο να στείλετε με το αρχικό ETag μαζί με μια κεφαλίδα υπό όρους για να βεβαιωθείτε ότι μια ενημερωμένη έκδοση θα παρουσιαστεί μόνο αν ικανοποιείται μια συγκεκριμένη συνθήκη-στην περίπτωση αυτή η συνθήκη είναι μια κεφαλίδα "If-Match" που απαιτεί την υπηρεσία αποθήκευσης για να βεβαιωθείτε ότι η τιμή του το ETag που καθορίζεται στην πρόσκληση σε ενημερωμένη έκδοση είναι το ίδιο με το αποθηκευμένο στην υπηρεσία αποθήκευσης.  

Το περίγραμμα της αυτή η διαδικασία είναι ως εξής:  

1.  Ανάκτηση ενός blob από την υπηρεσία χώρου αποθήκευσης, η απόκριση που περιλαμβάνει μια τιμή κεφαλίδα ETag HTTP που προσδιορίζει την τρέχουσα έκδοση του αντικειμένου στην υπηρεσία αποθήκευσης.
2.  Όταν ενημερώνετε το αντικείμενο blob, περιλαμβάνει την τιμή ETag που λάβατε στο βήμα 1 στην κεφαλίδα υπό συνθήκη **If-Match** της αίτησης που στέλνετε στην υπηρεσία.
3.  Η υπηρεσία συγκρίνει την τιμή ETag στην πρόσκληση σε με την τρέχουσα τιμή ETag από το αντικείμενο blob.
4.  Εάν η τρέχουσα τιμή ETag από το αντικείμενο blob είναι μια διαφορετική έκδοση από το ETag στην κεφαλίδα υπό συνθήκη **If-Match** στην πρόσκληση σε, η υπηρεσία επιστρέφει ένα σφάλμα 412 στον υπολογιστή-πελάτη. Αυτό υποδεικνύει στο πρόγραμμα-πελάτη που κάποια άλλη διαδικασία έχει ενημερωθεί το αντικείμενο blob δεδομένου ότι ο υπολογιστής-πελάτης ανάκτηση του.
5.  Εάν η τρέχουσα τιμή ETag από το αντικείμενο blob είναι την ίδια έκδοση με το ETag στην κεφαλίδα υπό συνθήκη **If-Match** στην πρόσκληση σε, η υπηρεσία εκτελεί τη λειτουργία που ζητήθηκε και ενημερώνει την τρέχουσα τιμή ETag το αντικείμενο blob για την εμφάνιση που έχει δημιουργηθεί μια νέα έκδοση.  

Το παρακάτω C# τμήμα κώδικα (χρησιμοποιώντας τη βιβλιοθήκη χώρου αποθήκευσης του προγράμματος-πελάτη 4.2.0) εμφανίζει ένα απλό παράδειγμα του τρόπου για να δημιουργήσετε ένα **AccessCondition If-Match** ανάλογα με την τιμή ETag που είναι προσβάσιμες από τις ιδιότητες του ένα blob που ήταν προηγουμένως είτε ανάκτηση ή που έχουν εισαχθεί. Στη συνέχεια, χρησιμοποιεί το αντικείμενο **AccessCondition** κατά την ενημέρωση του blob: το αντικείμενο **AccessCondition** προσθέτει την κεφαλίδα **If-Match** στην πρόσκληση σε. Εάν κάποια άλλη διαδικασία έχει ενημερωθεί το αντικείμενο blob, η υπηρεσία blob επιστρέφει ένα μήνυμα κατάστασης HTTP 412 (τιμή απέτυχε). Μπορείτε να κάνετε λήψη του πλήρους δείγματος εδώ: [Διαχείριση συγχρονισμού με χώρο αποθήκευσης Azure](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

    // Retrieve the ETag from the newly created blob
    // Etag is already populated as UploadText should cause a PUT Blob call
    // to storage blob service which returns the etag in response.
    string orignalETag = blockBlob.Properties.ETag;

    // This code simulates an update by a third party.
    string helloText = "Blob updated by a third party.";

    // No etag, provided so orignal blob is overwritten (thus generating a new etag)
    blockBlob.UploadText(helloText);
    Console.WriteLine("Blob updated. Updated ETag = {0}",
    blockBlob.Properties.ETag);

    // Now try to update the blob using the orignal ETag provided when the blob was created
    try
    {
        Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
        blockBlob.UploadText(helloText,accessCondition:
        AccessCondition.GenerateIfMatchCondition(orignalETag));
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        {
            Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
            // TODO: client can decide on how it wants to handle the 3rd party updated content.
        }
        else
            throw;
    }  

Η υπηρεσία αποθήκευσης περιλαμβάνει επίσης υποστήριξη για επιπλέον κεφαλίδες υπό όρους όπως **If-τροποποίηση-από**, **If-χωρίς τροποποίηση-από** και **If-None-Match** καθώς και συνδυασμών τους. Για περισσότερες πληροφορίες ανατρέξτε στο θέμα [Καθορίζοντας υπό όρους των κεφαλίδων για λειτουργίες υπηρεσίας Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx) στο MSDN.  

Ο παρακάτω πίνακας συνοψίζει τις λειτουργίες κοντέινερ που αποδοχή υπό όρους κεφαλίδες όπως **If-Match** στην αίτηση και που επιστρέφει τιμή ETag στην απάντηση.  

| Η λειτουργία                | Επιστρέφει την τιμή ETag κοντέινερ | Αποδέχεται κεφαλίδες υπό όρους |
|:-------------------------|:-----------------------------|:----------------------------|
| Δημιουργία κοντέινερ         | Ναι                          | Όχι                          |
| Λήψη ιδιοτήτων κοντέινερ | Ναι                          | Όχι                          |
| Λήψη κοντέινερ μετα-δεδομένων   | Ναι                          | Όχι                          |
| Ορισμός κοντέινερ μετα-δεδομένων   | Ναι                          | Ναι                         |
| Λάβετε κοντέινερ ACL        | Ναι                          | Όχι                          |
| Ορισμός κοντέινερ ACL        | Ναι                          | Ναι (*)                     |
| Διαγραφή κοντέινερ         | Όχι                           | Ναι                         |
| Εκμίσθωση κοντέινερ          | Ναι                          | Ναι                         |
| Λίστα αντικείμενα BLOB               | Όχι                           | Όχι                          |

(*) Τα δικαιώματα που ορίζονται από SetContainerACL αποθηκεύονται στο cache και ενημερώσεις για αυτά τα δικαιώματα χρειαστούν 30 δευτερόλεπτα για μεταδοθούν περίοδο κατά την οποία δεν εγγυάται ενημερώσεις ώστε να είναι συνεπείς.  

Ο παρακάτω πίνακας συνοψίζει τις λειτουργίες blob που αποδοχή υπό όρους κεφαλίδες όπως **If-Match** στην αίτηση και που επιστρέφει τιμή ETag στην απάντηση.

| Η λειτουργία           | Επιστρέφει την τιμή ETag | Αποδέχεται κεφαλίδες υπό όρους           |
|:--------------------|:-------------------|:--------------------------------------|
| Τοποθέτηση αντικειμένων Blob            | Ναι                | Ναι                                   |
| Λήψη Blob            | Ναι                | Ναι                                   |
| Λήψη ιδιοτήτων Blob | Ναι                | Ναι                                   |
| Ορισμός ιδιοτήτων Blob | Ναι                | Ναι                                   |
| Λήψη Blob μετα-δεδομένων   | Ναι                | Ναι                                   |
| Ορισμός Blob μετα-δεδομένων   | Ναι                | Ναι                                   |
| Εκμίσθωση Blob (*)      | Ναι                | Ναι                                   |
| Στιγμιότυπο Blob       | Ναι                | Ναι                                   |
| Αντιγραφή αντικειμένων Blob           | Ναι                | Ναι (για προέλευσης και προορισμού blob) |
| Ματαίωση αντιγραφή αντικειμένων Blob     | Όχι                 | Όχι                                    |
| Διαγραφή αντικειμένων Blob         | Όχι                 | Ναι                                   |
| Τοποθέτηση μπλοκ           | Όχι                 | Όχι                                    |
| Τοποθέτηση λίστα μπλοκ      | Ναι                | Ναι                                   |
| Λήψη λίστα μπλοκ      | Ναι                | Όχι                                    |
| Τοποθέτηση σελίδας            | Ναι                | Ναι                                   |
| Λήψη περιοχές σελίδων     | Ναι                | Ναι                                   |


(*) Εκμίσθωση Blob δεν αλλάζει η ETag σε ένα αντικείμενο blob.  

### <a name="pessimistic-concurrency-for-blobs"></a>Απαισιόδοξη ταυτόχρονης εκτέλεσης για αντικείμενα BLOB
Για να κλειδώσετε ένα blob για αποκλειστική χρήση, μπορείτε να αποκτήσετε μια [μίσθωση](http://msdn.microsoft.com/library/azure/ee691972.aspx) σε αυτό. Όταν αποκτήσετε μια μίσθωση, καθορίζετε για πόσο χρόνο πρέπει η μίσθωση: Αυτό μπορεί να είναι για μεταξύ 15 έως 60 δευτερόλεπτα ή άπειρο που αντιστοιχεί σε ένα κλείδωμα για αποκλειστική χρήση. Μπορείτε να ανανεώσετε μια πεπερασμένο μίσθωση για να επεκτείνετε το και να αφήσετε οποιαδήποτε εκμίσθωση όταν τελειώσετε με αυτό. Η υπηρεσία blob εκδόσεις αυτόματα πεπερασμένο μισθώσεις όταν λήξης.  

Μισθώσεις Ενεργοποίηση στρατηγικές διαφορετικό συγχρονισμού υποστήριξη, συμπεριλαμβανομένων των για αποκλειστική χρήση εγγραφής / ανάγνωσης, για αποκλειστική χρήση εγγραφής σε κοινή χρήση / αποκλειστική ανάγνωση και εγγραφή σε κοινή χρήση / ανάγνωση εξαιρουμένων των ακραίων τιμών. Όταν υπάρχει μια μίσθωση την υπηρεσία αποθήκευσης επιβάλλει εξαιρουμένων των ακραίων τιμών εγγραφές (τοποθέτηση, ρύθμιση και διαγραφή λειτουργίες) ταυτόχρονα εξασφαλίζετε ότι οι αποκλειστικότητα για λειτουργίες ανάγνωσης απαιτεί ωστόσο ο προγραμματιστής για να βεβαιωθείτε ότι όλες οι εφαρμογές προγράμματος-πελάτη να χρησιμοποιήσετε ένα Αναγνωριστικό εκμίσθωση και που μόνο ένα πρόγραμμα-πελάτη κάθε φορά περιλαμβάνει ένα αναγνωριστικό έγκυροι μίσθωση. Λειτουργίες που περιλαμβάνει ένα αποτέλεσμα Αναγνωριστικό μίσθωσης στο κοινόχρηστο διαβάζει ανάγνωσης.  

Το παρακάτω C# τμήμα κώδικα παρουσιάζει ένα παράδειγμα κατά τη λήψη μια αποκλειστική μίσθωση για 30 δευτερόλεπτα σε ένα αντικείμενο blob, ενημέρωση του περιεχομένου από το αντικείμενο blob και, στη συνέχεια, να αφήσετε τη μίσθωση. Εάν υπάρχει ήδη μια έγκυρη μίσθωση στην το αντικείμενο blob όταν προσπαθείτε να αποκτήσετε μια νέα μίσθωση, η υπηρεσία blob επιστρέφει ένα αποτέλεσμα κατάστασης "Διένεξη HTTP (409)". Το παρακάτω τμήμα κώδικα χρησιμοποιεί ένα αντικείμενο **AccessCondition** για τη συμπύκνωση τις πληροφορίες εκμίσθωση όταν κάνει μια αίτηση για να ενημερώσετε το αντικείμενο blob στην υπηρεσία αποθήκευσης.  Μπορείτε να κάνετε λήψη του πλήρους δείγματος εδώ: [Διαχείριση συγχρονισμού με χώρο αποθήκευσης Azure](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    // Acquire lease for 15 seconds
    string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
    Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

    // Update blob using lease. This operation will succeed
    const string helloText = "Blob updated";
    var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
    blockBlob.UploadText(helloText, accessCondition: accessCondition);
    Console.WriteLine("Blob updated using an exclusive lease");

    //Simulate third party update to blob without lease
    try
    {
        // Below operation will fail as no valid lease provided
        Console.WriteLine("Trying to update blob without valid lease");
        blockBlob.UploadText("Update without lease, will fail");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
            Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
        else
            throw;
    }  

Εάν επιχειρήσετε μια λειτουργία εγγραφής σε ένα μίσθωσης blob χωρίς να διέρχονται το Αναγνωριστικό μίσθωση, η αίτηση αποτυγχάνει με 412 σφάλμα. Λάβετε υπόψη ότι εάν λήξει η μίσθωση πριν από την κλήση της μεθόδου **UploadText** , αλλά εξακολουθείτε να περάσετε το Αναγνωριστικό μίσθωση, η αίτηση επίσης αποτύχει με σφάλμα **412** . Για περισσότερες πληροφορίες σχετικά με τη διαχείριση μίσθωσης λήξη ώρες και τα αναγνωριστικά μίσθωσης, ανατρέξτε στην τεκμηρίωση ΥΠΌΛΟΙΠΑ [Εκμίσθωση Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx) .  

Τις ακόλουθες λειτουργίες blob να χρησιμοποιήσετε μισθώσεις διαχείρισης Απαισιόδοξη ταυτόχρονης εκτέλεσης:  


-   Τοποθέτηση αντικειμένων Blob
-   Λήψη Blob
-   Λήψη ιδιοτήτων Blob
-   Ορισμός ιδιοτήτων Blob
-   Λήψη Blob μετα-δεδομένων
-   Ορισμός Blob μετα-δεδομένων
-   Διαγραφή αντικειμένων Blob
-   Τοποθέτηση μπλοκ
-   Τοποθέτηση λίστα μπλοκ
-   Λήψη λίστα μπλοκ
-   Τοποθέτηση σελίδας
-   Λήψη περιοχές σελίδων
-   Στιγμιότυπο Blob - Αναγνωριστικό εκμίσθωση προαιρετικά εάν υπάρχει μια μίσθωση
-   Αντιγραφή αντικειμένων Blob - εκμίσθωση ID απαιτείται αν υπάρχει μια μίσθωση στο blob του προορισμού
-   Ματαίωση αντιγραφή αντικειμένων Blob - εκμίσθωση ID απαιτείται αν υπάρχει μια μίσθωση άπειρο στο blob του προορισμού
-   Εκμίσθωση Blob  

### <a name="pessimistic-concurrency-for-containers"></a>Απαισιόδοξη ταυτόχρονης εκτέλεσης για κοντέινερ
Μισθώσεις στο κοντέινερ Ενεργοποίηση την ίδια στρατηγικές συγχρονισμού υποστήριξη σε αντικείμενα BLOB (εξαιρουμένων των ακραίων τιμών εγγραφής / ανάγνωσης, για αποκλειστική χρήση εγγραφής σε κοινή χρήση / αποκλειστική ανάγνωση και εγγραφή σε κοινή χρήση / ανάγνωση εξαιρουμένων των ακραίων τιμών) ωστόσο Αντίθετα με αντικείμενα BLOB την υπηρεσία αποθήκευσης μόνο επιβάλλει αποκλειστικότητα στην λειτουργίες διαγραφής. Για να διαγράψετε ένα κοντέινερ με μια ενεργή μίσθωση, ένας υπολογιστής-πελάτης πρέπει να συμπεριλάβετε το Αναγνωριστικό ενεργή μίσθωση με την αίτηση delete. Όλες οι άλλες λειτουργίες κοντέινερ επιτύχει σε ένα κοντέινερ μίσθωσης χωρίς να συμπεριλάβετε το Αναγνωριστικό εκμίσθωση οπότε γίνεται κοινή χρήση των λειτουργιών. Εάν απαιτείται αποκλειστικότητα ενημερωμένης έκδοσης (τοποθέτηση ή σύνολο) ή λειτουργίες ανάγνωσης, στη συνέχεια, τους προγραμματιστές θα πρέπει να βεβαιωθείτε όλοι οι πελάτες να χρησιμοποιήσετε ένα Αναγνωριστικό εκμίσθωση και ότι μόνο ένα πρόγραμμα-πελάτη κάθε φορά περιλαμβάνει ένα αναγνωριστικό έγκυροι μίσθωση.  

Τις ακόλουθες λειτουργίες κοντέινερ να χρησιμοποιήσετε μισθώσεις διαχείρισης Απαισιόδοξη ταυτόχρονης εκτέλεσης:  

-   Διαγραφή κοντέινερ
-   Λήψη ιδιοτήτων κοντέινερ
-   Λήψη κοντέινερ μετα-δεδομένων
-   Ορισμός κοντέινερ μετα-δεδομένων
-   Λάβετε κοντέινερ ACL
-   Ορισμός κοντέινερ ACL
-   Εκμίσθωση κοντέινερ  

Για περισσότερες πληροφορίες ανατρέξτε στο θέμα:  

- [Καθορισμός κεφαλίδες υπό όρους για λειτουργίες υπηρεσίας Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx)
- [Εκμίσθωση κοντέινερ](http://msdn.microsoft.com/library/azure/jj159103.aspx)
- [Εκμίσθωση Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Διαχείριση συγχρονισμού στην υπηρεσία πίνακα
Η υπηρεσία πίνακα χρησιμοποιεί αισιόδοξη ταυτόχρονης εκτέλεσης ελέγχει ως την προεπιλεγμένη συμπεριφορά όταν εργάζεστε με οντοτήτων, σε αντίθεση με την υπηρεσία blob όπου πρέπει να επιλέξετε για την εκτέλεση ελέγχων βέλτιστου. Τα άλλα διαφορά μεταξύ των υπηρεσιών πίνακα και αντικειμένων blob είναι ότι μπορείτε να διαχειριστείτε τη συμπεριφορά ταυτόχρονης εκτέλεσης των οντοτήτων μόνο ότι με την υπηρεσία blob μπορείτε να διαχειριστείτε το ταυτόχρονης εκτέλεσης του κοντέινερ και αντικείμενα blob.  

Για να χρησιμοποιήσετε βέλτιστου και για να ελέγξετε εάν άλλη διεργασία τροποποιηθεί μια οντότητα, μετά από την ανάκτηση του από την υπηρεσία αποθήκευσης πίνακα, μπορείτε να χρησιμοποιήσετε την τιμή ETag λαμβάνετε όταν η υπηρεσία πίνακα επιστρέφει μια οντότητα. Το περίγραμμα της αυτή η διαδικασία είναι ως εξής:  

1.  Ανάκτηση μιας οντότητας από την υπηρεσία αποθήκευσης πίνακα, η απόκριση που περιλαμβάνει μια τιμή ETag που προσδιορίζει το τρέχον αναγνωριστικό που σχετίζεται με αυτήν την οντότητα στην υπηρεσία αποθήκευσης.
2.  Όταν ενημερώνετε την οντότητα, περιλαμβάνει την τιμή ETag που λάβατε στο βήμα 1 στην κεφαλίδα **If-Match** υποχρεωτικό της αίτησης που στέλνετε στην υπηρεσία.
3.  Η υπηρεσία συγκρίνει την τιμή ETag στην πρόσκληση σε με την τρέχουσα τιμή ETag της οντότητας.
4.  Εάν η τρέχουσα τιμή ETag της οντότητας είναι διαφορετικό από το ETag στην κεφαλίδα **If-Match** υποχρεωτικό στην πρόσκληση σε, η υπηρεσία επιστρέφει ένα σφάλμα 412 στον υπολογιστή-πελάτη. Αυτό υποδεικνύει στο πρόγραμμα-πελάτη που κάποια άλλη διαδικασία έχει ενημερωθεί η οντότητα, επειδή το πρόγραμμα-πελάτη ανάκτηση του.
5.  Εάν η τρέχουσα τιμή ETag της οντότητας είναι ίδια με την ETag στην κεφαλίδα **If-Match** υποχρεωτικό στην πρόσκληση σε ή την κεφαλίδα **If-Match** περιέχει το χαρακτήρα μπαλαντέρ (*), η υπηρεσία εκτελεί τη λειτουργία που ζητήθηκε και ενημερώνει την τρέχουσα τιμή ETag της οντότητας για την εμφάνιση που έχει ενημερωθεί.  

Σημειώστε ότι σε αντίθεση με την υπηρεσία blob, η υπηρεσία πίνακα απαιτεί το πρόγραμμα-πελάτη για να συμπεριλάβετε μια κεφαλίδα **If-Match** στο αιτήματα ενημέρωσης. Ωστόσο, είναι δυνατό να επιβάλετε μια άνευ όρων update (τελευταία στρατηγικής wins δημιουργίας) και να παρακάμψετε ελέγχους ταυτόχρονης εκτέλεσης, εάν ο υπολογιστής-πελάτης ορίζει την κεφαλίδα **If-Match** για να το χαρακτήρα μπαλαντέρ (*) στην πρόσκληση σε.  

Το παρακάτω C# τμήμα κώδικα εμφανίζει μια οντότητα πελατών που προηγουμένως δημιουργήθηκε ή ανάκτηση χρειάζεται η διεύθυνση ηλεκτρονικού ταχυδρομείου που έχει ενημερωθεί. Το αρχικό εισαγάγετε ή να ανακτήσετε τη λειτουργία αποθηκεύει την τιμή ETag στο αντικείμενο πελατών και, επειδή το δείγμα χρησιμοποιεί την ίδια παρουσία του αντικειμένου κατά την εκτέλεση της λειτουργίας αντικατάστασης, αυτόματα επιστρέφει την τιμή ETag στην υπηρεσία του πίνακα, η ενεργοποίηση της υπηρεσίας για να ελέγξετε για παραβιάσεις ταυτόχρονης εκτέλεσης. Εάν κάποια άλλη διαδικασία έχει ενημερωθεί η οντότητα στο χώρο αποθήκευσης πινάκων, η υπηρεσία επιστρέφει ένα μήνυμα κατάστασης HTTP 412 (τιμή απέτυχε).  Μπορείτε να κάνετε λήψη του πλήρους δείγματος εδώ: [Διαχείριση συγχρονισμού με χώρο αποθήκευσης Azure](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    try
    {
        customer.Email = "updatedEmail@contoso.org";
        TableOperation replaceCustomer = TableOperation.Replace(customer);
        customerTable.Execute(replaceCustomer);
        Console.WriteLine("Replace operation succeeded.");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == 412)
            Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
        else
            throw;
    }  

Ρητά απενεργοποιήσετε τον έλεγχο ταυτόχρονης εκτέλεσης, θα πρέπει να ορίσετε την ιδιότητα **ETag** του αντικειμένου **υπαλλήλου** "*" πριν από την εκτέλεση της λειτουργίας αντικατάστασης.  

    customer.ETag = "*";  

Ο παρακάτω πίνακας συνοψίζει τον τρόπο τις λειτουργίες πίνακα οντότητα Χρησιμοποιήστε ETag τιμές:

| Η λειτουργία                | Επιστρέφει την τιμή ETag | Απαιτεί κεφαλίδα αίτησης If-Match |
|:-------------------------|:-------------------|:---------------------------------|
| Ερώτημα οντοτήτων           | Ναι                | Όχι                               |
| Εισαγωγή οντότητα            | Ναι                | Όχι                               |
| Ενημέρωση οντότητα            | Ναι                | Ναι                              |
| Συγχώνευση οντότητα             | Ναι                | Ναι                              |
| Διαγραφή οντότητας            | Όχι                 | Ναι                              |
| Εισαγωγή ή αντικατάσταση οντότητα | Ναι                | Όχι                               |
| Εισαγωγή ή οντότητα συγχώνευσης   | Ναι                | Όχι                               |

Σημειώστε ότι οι λειτουργίες **Insert ή αντικατάσταση οντότητα** και **Εισαγωγή ή συγχώνευση οντότητα** κάντε *δεν* εκτελέστε οποιαδήποτε ελέγχων ταυτόχρονης εκτέλεσης επειδή δεν στείλετε μια τιμή ETag με την υπηρεσία του πίνακα.  

Γενικά προγραμματιστές χρησιμοποιώντας πίνακες πρέπει να βασίζεστε στη βέλτιστου κατά την ανάπτυξη εφαρμογών μεταβλητού μεγέθους. Εάν είναι απαραίτητο το κλείδωμα Απαισιόδοξη, μία προσέγγιση προγραμματιστές μπορεί να διαρκέσει κατά την πρόσβαση σε πίνακες είναι να αντιστοιχίσετε ένα καθορισμένο blob για κάθε πίνακα και προσπαθήστε να για να χρησιμοποιήσετε μια μίσθωση το αντικείμενο blob πριν λειτουργικό του πίνακα. Αυτή η προσέγγιση απαιτεί την εφαρμογή για να βεβαιωθείτε ότι όλες οι διαδρομές πρόσβασης δεδομένων αποκτήσει η μίσθωση πριν από το λειτουργικό του πίνακα. Επίσης, πρέπει να σημειώσετε που ο χρόνος ελάχιστη μίσθωση είναι 15 δευτερολέπτων που απαιτεί προσεκτική εξέταση για κλιμάκωση.  

Για περισσότερες πληροφορίες ανατρέξτε στο θέμα:  

- [Λειτουργίες σε οντοτήτων](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Διαχείριση συγχρονισμού στην υπηρεσία Ουράς
Ένα σενάριο σε ποια ταυτόχρονης εκτέλεσης είναι ένα πρόβλημα στην υπηρεσία ουράς είναι όπου πολλά προγράμματα-πελάτες ανακτάτε μηνύματα από μια ουρά. Όταν ένα μήνυμα ανακτηθεί από την ουρά, η απόκριση περιλαμβάνει το μήνυμα και μια τιμή pop παραλαβή, που είναι απαραίτητη για να διαγράψετε το μήνυμα. Το μήνυμα δεν διαγράφονται αυτόματα από την ουρά, αλλά αφού έχει ανακτηθεί, δεν είναι ορατές σε άλλα προγράμματα-πελάτες για το χρονικό διάστημα που καθορίζεται από την παράμετρο visibilitytimeout. Για να διαγράψετε το μήνυμα μετά την επεξεργασία και πριν από την ώρα που καθορίζεται από το TimeNextVisible στοιχείο της απόκρισης, η οποία υπολογίζεται με βάση την τιμή της παραμέτρου visibilitytimeout αναμένεται προγράμματος-πελάτη που ανακτά το μήνυμα. Η τιμή του visibilitytimeout προστίθεται την ώρα κατά την οποία την ανάκτηση του μηνύματος για να καθορίσετε την τιμή της TimeNextVisible.  

Την υπηρεσία ουράς δεν διαθέτει υποστήριξη για είτε αισιόδοξη ή απαισιόδοξη ταυτόχρονης εκτέλεσης και για αυτό λόγο προγράμματα-πελάτες επεξεργασίας μηνύματα που ανακτώνται από μια ουρά θα πρέπει να βεβαιωθείτε ότι τα μηνύματα υποβάλλονται σε επεξεργασία με τον τρόπο idempotent. Τελευταία στρατηγικής wins writer χρησιμοποιείται για λειτουργίες update όπως SetQueueServiceProperties, SetQueueMetaData, SetQueueACL και UpdateMessage.  

Για περισσότερες πληροφορίες ανατρέξτε στο θέμα:  

- [Υπηρεσία ουράς REST API](http://msdn.microsoft.com/library/azure/dd179363.aspx)
- [Λήψη μηνυμάτων](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Διαχείριση συγχρονισμού στην υπηρεσία αρχείου
Η υπηρεσία αρχείων είναι δυνατή η πρόσβαση με χρήση δύο τελικά σημεία διαφορετικό πρωτόκολλο – Ερωτήσεων και ΥΠΌΛΟΙΠΟ. Η υπηρεσία ΥΠΌΛΟΙΠΟ δεν διαθέτει υποστήριξη για το κλείδωμα αισιόδοξες ή απαισιόδοξη κλείδωμα και όλες τις ενημερώσεις θα ακολουθήστε τελευταίο στρατηγικής wins writer. Προγράμματα-πελάτες Ερωτήσεων που ενεργοποίησης κοινόχρηστα στοιχεία αρχείων να αξιοποιήσετε αρχείο συστήματος locking μηχανισμούς για να διαχειριστείτε την πρόσβαση σε κοινόχρηστα αρχεία – συμπεριλαμβανομένης της δυνατότητας εκτελέσετε Απαισιόδοξη κλείδωμα. Όταν ένα πρόγραμμα-πελάτη Ερωτήσεων ανοίγει ένα αρχείο, καθορίζει τόσο η πρόσβαση στο αρχείο και κοινή χρήση λειτουργίας. Ορίζοντας μια επιλογή πρόσβαση στο αρχείο "Σύνταξη" ή "Μόνο για ανάγνωση" μαζί με μια λειτουργία κοινή χρήση αρχείου "Κανένα" θα έχει ως αποτέλεσμα το αρχείο είναι κλειδωμένο από ένα πρόγραμμα-πελάτη Ερωτήσεων μέχρι να κλείσετε το αρχείο. Εάν επιχειρείται η ΥΠΌΛΟΙΠΗ εργασία σε ένα αρχείο όπου ένα πρόγραμμα-πελάτη Ερωτήσεων έχει κλειδώσει το αρχείο της υπηρεσίας ΥΠΌΛΟΙΠΟ θα επιστρέψει ο κωδικός κατάστασης 409 (διένεξη) με κωδικό σφάλματος SharingViolation.  

Όταν ένα πρόγραμμα-πελάτη Ερωτήσεων ανοίγει ένα αρχείο για διαγραφή, το επισημαίνει το αρχείο ως εκκρεμεί διαγραφή μέχρι να όλες οι άλλες-πελάτη Ερωτήσεων είναι κλειστές Άνοιγμα λαβές σε αυτό το αρχείο. Όταν ένα αρχείο έχει σημανθεί ως εκκρεμεί διαγραφή, οποιαδήποτε ΥΠΌΛΟΙΠΑ πράξη σε αυτό το αρχείο θα επιστρέψει ο κωδικός κατάστασης 409 (διένεξη) με κωδικό σφάλματος SMBDeletePending. Κωδικός κατάστασης 404 (δεν βρέθηκε) δεν επιστρέφεται, εφόσον είναι δυνατή για το πρόγραμμα-πελάτη Ερωτήσεων για να καταργήσετε τη σημαία εκκρεμεί Διαγραφή πριν κλείσετε το αρχείο. Με άλλα λόγια, ο κωδικός κατάστασης 404 (δεν βρέθηκε) αναμένεται μόνο όταν το αρχείο έχει καταργηθεί. Σημειώστε ότι όταν είναι ένα αρχείο σε μια Ερωτήσεων εκκρεμεί διαγραφή κατάστασης, αυτό δεν θα συμπεριληφθούν στα αποτελέσματα της λίστας αρχείων. Επίσης, σημειώστε ότι οι λειτουργίες ΥΠΌΛΟΙΠΑ διαγραφή αρχείων και καταλόγων διαγραφή ΥΠΌΛΟΙΠΟ είναι atomically και δεν έχει ως αποτέλεσμα εκκρεμεί διαγραφή κατάσταση.  

Για περισσότερες πληροφορίες ανατρέξτε στο θέμα:  

- [Διαχείριση αρχείων κλειδώνει](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Σύνοψη και επόμενα βήματα
Η υπηρεσία Microsoft Azure αποθήκευσης έχει σχεδιαστεί ώστε να ανταποκρίνεται στις ανάγκες των εφαρμογών του πιο σύνθετες online χωρίς να επιβάλετε τους προγραμματιστές να υπονομεύσει ή ενδείκνυται υποθέσεις κλειδιού σχεδίασης όπως συνέπειας ταυτόχρονης εκτέλεσης και τα δεδομένα που προέρχονται για να κρατήσετε παρέχεται για.  

Για την εφαρμογή ολοκληρωμένο δείγμα που αναφέρονται σε αυτό το ιστολόγιο:  

- [Διαχείριση συγχρονισμού με χώρο αποθήκευσης Azure - δείγμα εφαρμογής](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Για περισσότερες πληροφορίες σχετικά με αποθήκευσης Azure ανατρέξτε στο θέμα:  

- [Microsoft Azure αποθήκευσης αρχική σελίδα](https://azure.microsoft.com/services/storage/)
- [Εισαγωγή στις Azure χώρου αποθήκευσης](storage-introduction.md)
- Γρήγορα αποτελέσματα για [Blob](storage-dotnet-how-to-use-blobs.md), [πίνακα](storage-dotnet-how-to-use-tables.md), [ουρές](storage-dotnet-how-to-use-queues.md)και [αρχεία](storage-dotnet-how-to-use-files.md) χώρου αποθήκευσης
- Αρχιτεκτονική αποθήκευσης – [Azure χώρου αποθήκευσης: υπηρεσία αποθήκευσης στο Cloud ιδιαίτερα διαθέσιμο με ισχυρό συνέπειας](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
