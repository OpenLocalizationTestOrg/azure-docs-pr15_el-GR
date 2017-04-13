<properties
    pageTitle="Κλιμάκωση ανάλυση ροής εργασιών για να αυξήσετε την ταχύτητα | Microsoft Azure"
    description="Μάθετε πώς να κλιμακωθεί ανάλυση ροής εργασιών με τη ρύθμιση των παραμέτρων εισαγωγής διαμερίσματα, της ρύθμισης τον ορισμό του ερωτήματος και τη ρύθμιση μονάδες η ροή εργασίας."
    keywords="δεδομένα ροής, η ροή επεξεργασίας δεδομένων, με ακρίβεια αναλυτικών στοιχείων"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Κλιμάκωση Azure ανάλυση ροής εργασιών για να αυξήσετε την ταχύτητα επεξεργασίας δεδομένων ροής

Μάθετε πώς να με ακρίβεια τις εργασίες ανάλυσης και Υπολογισμός *μονάδες ροής* για ροή ανάλυση, τον τρόπο για να κλιμακωθεί ανάλυση ροής εργασιών με τη ρύθμιση των παραμέτρων εισαγωγής διαμερίσματα, της ρύθμισης τον ορισμό του ερωτήματος ανάλυσης και τη ρύθμιση μονάδες η ροή εργασίας. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Τι είναι τα τμήματα μιας ανάλυσης ροή εργασίας;
Έναν ορισμό ανάλυση ροή εργασίας περιλαμβάνει εισόδων, ένα ερώτημα και εξόδου. Στοιχεία που έχετε εισαγάγει από όπου η εργασία διαβάζει τη ροή δεδομένων, το ερώτημα χρησιμοποιείται για τη μετατροπή της ροής δεδομένων εισόδου και το αποτέλεσμα είναι όπου η εργασία αποστέλλει τα αποτελέσματα εργασία για να.  

Μια εργασία απαιτεί τουλάχιστον μία προέλευση εισόδου για ροή δεδομένων. Τα δεδομένα προέλευσης εισαγωγής ροή μπορούν να αποθηκευτούν σε ένα διανομέα Azure Service Bus συμβάν ή στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Εισαγωγή στην ανάλυση ροή Azure](stream-analytics-introduction.md) και [να ξεκινήσετε να χρησιμοποιείτε Azure ροή ανάλυσης](stream-analytics-get-started.md).

## <a name="configuring-streaming-units"></a>Ρύθμιση παραμέτρων ροής μονάδες
Ροή μονάδες (SUs) αντιπροσωπεύουν τους πόρους και υπολογιστική ισχύ που απαιτείται για την εκτέλεση μιας εργασίας Azure ροή ανάλυσης. SUs παρέχει κάποιο τρόπο για να περιγράψετε το συμβάν σχετική επεξεργασίας χωρητικότητας που βασίζονται σε ανάμειξη μέτρηση της CPU, μνήμη, ανάγνωση και εγγραφή χρεώσεων. Κάθε ροή μονάδα αντιστοιχεί περίπου 1MB/δευτερόλεπτα ταχύτητα μεταγωγής. 

Επιλογή πόσες SUs είναι απαραίτητα για μια συγκεκριμένη εργασία εξαρτάται από τη ρύθμιση παραμέτρων διαμερίσματα για τα δεδομένα εισόδου και το ερώτημα που ορίζεται για την εργασία. Μπορείτε να επιλέξετε μέχρι το όριο σε ροή μονάδες για ένα έργο χρησιμοποιώντας την πύλη κλασική Azure. Κάθε Azure συνδρομή από προεπιλογή έχει ένα όριο των έως και 50 ροής μονάδες για όλες τις εργασίες αναλυτικών στοιχείων σε μια συγκεκριμένη περιοχή. Για να αυξήσετε ροής μονάδων για τις συνδρομές σας, επικοινωνήστε με την [Υποστήριξη της Microsoft](http://support.microsoft.com).

Ο αριθμός των ροής μονάδων που μπορούν να χρησιμοποιούν μια εργασία εξαρτάται από τη ρύθμιση παραμέτρων διαμερίσματα για τα δεδομένα εισόδου και το ερώτημα που ορίζεται για την εργασία. Σημειώστε επίσης ότι πρέπει να χρησιμοποιείται μια έγκυρη τιμή για τις μονάδες ροής. Οι έγκυρες τιμές έναρξης 1, 3, 6 και, στη συνέχεια, προς τα επάνω σε διαστήματα από 6, όπως φαίνεται παρακάτω.

![Ανάλυση Azure ροή ροή μονάδες κλίμακας][img.stream.analytics.streaming.units.scale]

Σε αυτό το άρθρο θα σας δείξουν πώς μπορείτε να υπολογίσετε και με ακρίβεια το ερώτημα για να αυξήσετε μετάδοσης για εργασίες ανάλυσης.

## <a name="embarrassingly-parallel-job"></a>Παράλληλη embarrassingly εργασία
Η εργασία embarrassingly παράλληλες είναι το πιο μεταβλητού μεγέθους σενάριο που έχετε στο Azure ροή ανάλυσης. Συνδέεται ένα διαμερίσματα της εισόδου σε μία παρουσία του ερωτήματος σε ένα διαμερίσματα του αποτελέσματος. Την επίτευξη αυτό παραλληλισμό απαιτεί μερικά πράγματα:

1.  Αν σας λογικής ερωτήματος εξαρτάται από το ίδιο κλειδί που υποβάλλεται σε επεξεργασία από την ίδια παρουσία ερωτήματος, στη συνέχεια, πρέπει να βεβαιωθείτε ότι τα συμβάντα μεταβείτε το ίδιο διαμερίσματα της εισαγωγής σας. Στην περίπτωση διανομείς συμβάντος, αυτό σημαίνει ότι τα δεδομένα συμβάντων πρέπει να έχει οριστεί **PartitionKey** ή μπορείτε να χρησιμοποιήσετε διαμερίσματα αποστολείς. Για Blob, αυτό σημαίνει ότι τα συμβάντα στέλνονται στον ίδιο φάκελο διαμερίσματα. Εάν το ερώτημα λογικής δεν απαιτεί να επεξεργαστούν το ίδιο κλειδί από την ίδια παρουσία ερωτήματος, στη συνέχεια, μπορείτε να αγνοήσετε αυτήν την απαίτηση. Ένα παράδειγμα θα ήταν ενός απλού ερωτήματος επιλέξτε/έργου φίλτρου.  
2.  Όταν τα δεδομένα είναι διάταξη όπως αυτό πρέπει να βρίσκεται στην πλευρά εισαγωγής, πρέπει να βεβαιωθείτε ότι το ερώτημά σας έχει διαμερίσματα. Απαιτεί να χρησιμοποιήσετε **Διαμερίσματα με** σε όλα τα βήματα. Επιτρέπονται πολλά βήματα αλλά όλες πρέπει να είναι διαμερίσματα με τον ίδιο αριθμό-κλειδί. Κάτι άλλο για να λάβετε υπόψη είναι ότι, προς το παρόν, το πλήκτρο διαμερισμού πρέπει να οριστεί να **ΑναγνωριστικόΔιαμερίσματος** να έχετε μια πλήρως παράλληλες εργασία.  
3.  Υποστηρίζονται προς το παρόν μόνο συμβάν διανομείς και αντικειμένων Blob διαμερίσματα εξόδου. Για το συμβάν διανομείς εξόδου, πρέπει να ρυθμίσετε το πεδίο **PartitionKey** **ΑναγνωριστικόΔιαμερίσματος**. Για Blob, δεν χρειάζεται να κάνετε τίποτα.  
4.  Κάτι άλλο να σημειώσετε, ο αριθμός των διαμερισμάτων εισαγωγής πρέπει να ισούται με τον αριθμό των διαμερισμάτων εξόδου. BLOB εξόδου αυτήν τη στιγμή δεν υποστηρίζει τα διαμερίσματα, αλλά αυτό οφείλεται πειράζει θα μεταβιβαστεί του συνδυασμού διαμερισμάτων την επόμενη ερωτήματος. Παραδείγματα των τιμών διαμερισμάτων που θα επιτρέπουν πλήρως παράλληλες εργασίας:  
    1.  8 διανομείς συμβάν εισόδου διαμερίσματα και 8 διανομείς συμβάν εξόδου διαμερίσματα
    2.  8 διανομείς συμβάν εισαγωγής διαμερίσματα και αντικειμένων Blob εξόδου  
    3.  8 blob διαμερίσματα εισόδου και εξόδου Blob  
    4.  8 blob εισαγωγής διαμερίσματα και 8 διανομείς συμβάν εξόδου διαμερίσματα  

Ακολουθούν ορισμένα παραδείγματα σεναρίων που είναι embarrassingly παράλληλες.

### <a name="simple-query"></a>Απλό ερώτημα
Διανομέα συμβάν εισαγωγής – συμβάν διανομείς με 8 διαμερίσματα εξόδου – με 8 διαμερίσματα

**Ερώτημα:**

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Αυτό το ερώτημα είναι ένα απλό φίλτρο και ως εκ τούτου, θα σας, δεν χρειάζεται να ανησυχείτε για τη δημιουργία διαμερισμάτων τα δεδομένα εισόδου σας στείλουμε με διανομείς συμβάν. Θα παρατηρήσετε ότι το ερώτημα έχει **Διαμερίσματα μέσω** της **ΑναγνωριστικόΔιαμερίσματος**, έτσι θα σας ικανοποιεί απαίτηση #2 από επάνω. Για την έξοδο, πρέπει να ρυθμίσετε τις παραμέτρους το αποτέλεσμα του συμβάντος διανομείς η εργασία να έχει το πεδίο **PartitionKey** οριστεί σε **ΑναγνωριστικόΔιαμερίσματος**. Μία τελευταία ελέγχου, τα διαμερίσματα εισαγωγής == διαμερίσματα εξόδου. Αυτή η τοπολογία είναι embarrassingly παράλληλες.

### <a name="query-with-grouping-key"></a>Ερώτημα με αριθμό-κλειδί ομαδοποίησης
Εισαγωγής – συμβάν διανομείς με 8 διαμερίσματα εξόδου – Blob

**Ερώτημα:**

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Αυτό το ερώτημα έχει έναν αριθμό-κλειδί ομαδοποίησης και ως εκ τούτου, τον ίδιο αριθμό-κλειδί πρέπει να υποβληθεί σε επεξεργασία από την ίδια παρουσία ερωτήματος. Αυτό σημαίνει ότι πρέπει να στείλετε τα συμβάντα σε διανομείς συμβάντα σε διαμερίσματα. Ποια κλειδί θα σας ενδιαφέρουν; **ΑναγνωριστικόΔιαμερίσματος** είναι μια έννοια λογικής εργασία, το πραγματικό κλειδί που σας ενδιαφέρουν είναι **TollBoothId**. Αυτό σημαίνει ότι θα πρέπει να μπορούμε να εγκαταστήσουμε το **PartitionKey** από τα δεδομένα του συμβάντος στείλουμε με διανομείς συμβάντος για να το **TollBoothId** του συμβάντος. Το ερώτημα έχει **Διαμερίσματα μέσω** της **ΑναγνωριστικόΔιαμερίσματος**, ώστε να είμαστε καλή εκεί. Για την έξοδο, εφόσον είναι Blob, θα σας δεν χρειάζεται να ανησυχείτε για τη ρύθμιση των παραμέτρων **PartitionKey**. Για την απαίτηση #4, ξανά, αυτή είναι Blob, έτσι θα σας δεν χρειάζεται να ανησυχείτε. Αυτή η τοπολογία είναι embarrassingly παράλληλες.

### <a name="multi-step-query-with-grouping-key"></a>Πολλούς βήμα ερωτήματος με αριθμό-κλειδί ομαδοποίησης ###
Διανομέα συμβάν εισαγωγής – συμβάν διανομέα με 8 διαμερίσματα εξόδου – με 8 διαμερίσματα

**Ερώτημα:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Αυτό το ερώτημα έχει έναν αριθμό-κλειδί ομαδοποίησης και ως εκ τούτου, τον ίδιο αριθμό-κλειδί πρέπει να υποβληθεί σε επεξεργασία από την ίδια παρουσία ερωτήματος. Μπορούμε να χρησιμοποιήσουμε την ίδια στρατηγική ως το προηγούμενο ερώτημα. Το ερώτημα έχει πολλά βήματα. Διαθέτει κάθε βήμα **Partition μέσω** της **ΑναγνωριστικόΔιαμερίσματος**; Ναι, ώστε να είμαστε καλή. Για την έξοδο, πρέπει να ρυθμίσετε το **PartitionKey** **ΑναγνωριστικόΔιαμερίσματος** όπως περιγράφεται παραπάνω και μπορούμε να δούμε επίσης που έχει τον ίδιο αριθμό των διαμερισμάτων με την είσοδο. Αυτή η τοπολογία είναι embarrassingly παράλληλες.


## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Παραδείγματα σεναρίων που δεν είναι embarrassingly παράλληλες

### <a name="mismatched-partition-count"></a>Ασυμφωνία πλήθος διαμερισμάτων ###
Διανομέα συμβάν εισαγωγής – συμβάν διανομείς με 8 διαμερίσματα εξόδου – με 32 διαμερίσματα

Δεν έχει σημασία ποια το ερώτημα είναι σε αυτήν την περίπτωση επειδή η είσοδος διαμερισμάτων count! = πλήθος διαμερισμάτων εξόδου.

### <a name="not-using-event-hubs-or-blobs-as-output"></a>Δεν χρησιμοποιείτε το συμβάν διανομείς ή αντικείμενα BLOB ως αποτέλεσμα
PowerBI εισαγωγής – συμβάν διανομείς με 8 διαμερίσματα εξόδου –

PowerBI εξόδου δεν υποστηρίζεται επί του παρόντος διαμερισμάτων.

### <a name="multi-step-query-with-different-partition-by-values"></a>Πολλούς βήμα ερωτήματος με διαφορετικές διαμερίσματα με τιμές
Διανομέα συμβάν εισαγωγής – συμβάν διανομέα με 8 διαμερίσματα εξόδου – με 8 διαμερίσματα

**Ερώτημα:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
Όπως μπορείτε να δείτε, το δεύτερο βήμα χρησιμοποιεί **TollBoothId** ως διαμερισμού κλειδί. Αυτό δεν είναι το ίδιο με το πρώτο βήμα και, επομένως, θα απαιτούν να κάνετε μια τυχαία σειρά. 

Αυτές είναι ορισμένα παραδείγματα και counterexamples ανάλυση ροής εργασιών που θα έχουν τη δυνατότητα να επιτύχετε μια τοπολογία embarrassingly παράλληλες και με αυτήν τη δυνατότητα μέγιστη κλίμακα. Για τις εργασίες που δεν εμπίπτουν σε ένα από αυτά τα προφίλ, θα γίνει μελλοντικές ενημερώσεις που περιγράφει με λεπτομέρεια τον τρόπο για να κλιμακωθεί πολύ ορισμένα άλλα κανονικής σενάρια ροή ανάλυσης.

Για κάνετε τώρα, χρησιμοποιήστε τις παρακάτω γενικές οδηγίες:

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Υπολογισμός ο μέγιστος αριθμός μονάδων μιας εργασίας ροής
Ο συνολικός αριθμός των ροής μονάδες που μπορούν να χρησιμοποιηθούν από μια εργασία ροής ανάλυση εξαρτάται από τον αριθμό των βημάτων του ερωτήματος που ορίζονται για την εργασία και ο αριθμός των διαμερισμάτων για κάθε βήμα.

### <a name="steps-in-a-query"></a>Τα βήματα σε ένα ερώτημα
Ένα ερώτημα μπορεί να έχει ένα ή πολλά βήματα. Κάθε βήμα είναι ένα δευτερεύον ερώτημα που ορίζονται από τη λέξη-κλειδί **με** χρήση. Το ερώτημα μόνο που είναι εκτός του τη λέξη-κλειδί **με** επίσης υπολογίζεται ως ένα βήμα, **για παράδειγμα, την πρόταση SELECT στο ακόλουθο ερώτημα** :

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Το προηγούμενο ερώτημα έχει δύο βήματα.

> [AZURE.NOTE] Αυτό το δείγμα ερώτημα θα αναλυθεί αργότερα στο άρθρο.

### <a name="partition-a-step"></a>Δημιουργία διαμερισμάτων ένα βήμα

Δημιουργία διαμερισμάτων ένα βήμα απαιτεί τις ακόλουθες συνθήκες:

- Η προέλευση εισόδου πρέπει να είναι διαμερίσματα. Για περισσότερες πληροφορίες, ανατρέξτε στον [Οδηγό προγραμματισμού συμβάντων διανομείς](../event-hubs/event-hubs-programming-guide.md).
- **Την πρόταση SELECT του ερωτήματος** πρέπει να διαβάσετε από ένα διαμερίσματα προέλευσης εισαγωγής.
- Το ερώτημα μέσα στο βήμα πρέπει να έχετε τα **Διαμερίσματα με** τη λέξη-κλειδί

Όταν ένα ερώτημα έχει διαμερίσματα, τα συμβάντα εισαγωγής θα επεξεργασία και αθροιστικά σε ξεχωριστή partition ομάδες και συμβάντα εξόδους δημιουργούνται για κάθε μία από τις ομάδες. Εάν μια συγκεντρωτική τιμή συνδυασμένο είναι επιθυμητό, πρέπει να δημιουργήσετε ένα δεύτερο βήμα δεν έχουν δημιουργηθεί διαμερίσματα να συναθροίσετε.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Υπολογισμός του μέγιστες μονάδες για μια εργασία ροής

Όλα τα βήματα που δεν έχουν δημιουργηθεί διαμερίσματα μαζί να κλίμακα έως έξι μονάδες ροής για μια εργασία ροής ανάλυσης. Για να προσθέσετε επιπλέον ροή μονάδες, ένα βήμα πρέπει να είναι διαμερίσματα. Κάθε διαμερίσματα μπορούν να έχουν έξι ροής μονάδες.

<table border="1">
<tr><th>Ερώτημα μιας εργασίας</th><th>Μέγιστες μονάδες για την εργασία ροής</th></td>

<tr><td>
<ul>
<li>Το ερώτημα περιέχει ένα βήμα.</li>
<li>Το βήμα δεν έχει διαμερίσματα.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Η ροή δεδομένων εισόδου είναι διαμερίσματα με 3.</li>
<li>Το ερώτημα περιέχει ένα βήμα.</li>
<li>Στο βήμα έχει διαμερίσματα.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>Το ερώτημα περιέχει δύο βήματα.</li>
<li>Κανένα από τα βήματα είναι διαμερίσματα.</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>Η εισαγωγή ροής δεδομένων είναι διαμερίσματα με 3.</li>
<li>Το ερώτημα περιέχει δύο βήματα. Στο βήμα εισαγωγής έχει διαμερίσματα και δεν είναι το δεύτερο βήμα.</li>
<li>Η πρόταση SELECT διαβάζει από τα διαμερίσματα εισόδου.</li>
</ul>
</td>
<td>24 (18 για διαμερίσματα βήματα) + 6 για τα βήματα που δεν έχουν δημιουργηθεί διαμερίσματα</td></tr>
</table>

### <a name="examples-of-scale"></a>Παραδείγματα της κλίμακας
Το παρακάτω ερώτημα υπολογίζει τον αριθμό των αυτοκινήτων διέρχονται από μια θέση με χρέωση με τρεις tollbooths μέσα σε ένα παράθυρο τριών λεπτών. Αυτό το ερώτημα μπορεί να έχει έως και έξι ροής μονάδες.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Για να χρησιμοποιήσετε περισσότερες μονάδες ροής για το ερώτημα, τόσο η ροή δεδομένων εισόδου και πρέπει να είναι διαμερίσματα του ερωτήματος. Δεδομένου ότι τα διαμερίσματα ροή δεδομένων έχει οριστεί σε 3, το ακόλουθο ερώτημα έχουν τροποποιηθεί μπορεί να γίνει κλιμάκωση έως 18 ροής μονάδες:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Όταν ένα ερώτημα έχει διαμερίσματα, τα συμβάντα εισαγωγής υποβάλλονται σε επεξεργασία και συναθροίζονται σε ξεχωριστή partition ομάδες. Συμβάντα εξόδου δημιουργούνται επίσης για κάθε μία από τις ομάδες. Δημιουργία διαμερισμάτων μπορεί να προκαλέσει ορισμένα μη αναμενόμενα αποτελέσματα όταν το πεδίο " **Ομαδοποίηση κατά** " δεν είναι το κλειδί Partition την εισαγωγή ροής δεδομένων. Για παράδειγμα, στο πεδίο **TollBoothId** το προηγούμενο ερώτημα δείγμα δεν είναι το κλειδί Input1 διαμερίσματα. Τα δεδομένα από την #1 υπόστεγο μπορούν να προβληθούν σε περισσότερα από ένα διαμερίσματα.

Κάθε ένα από τα διαμερίσματα Input1 θα υποβληθούν σε ξεχωριστά από ροή ανάλυση και πολλές εγγραφές από το πλήθος αυτοκίνητο πέρασμα-εμφάνισης για την ίδια υπόστεγο στο ίδιο παράθυρο tumbling θα δημιουργηθεί. Εάν δεν μπορούν να αλλάξουν τον αριθμό-κλειδί εισαγωγής διαμερίσματα, αυτό το πρόβλημα μπορεί να διορθωθεί, προσθέτοντας ένα επιπλέον βήμα μη διαμερισμάτων, για παράδειγμα:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Αυτό το ερώτημα μπορεί να έχει 24 ροής μονάδες.

>[AZURE.NOTE] Εάν συνδέετε δύο ροών, βεβαιωθείτε ότι οι ροές δημιουργούνται διαμερίσματα με το πλήκτρο διαμερίσματα της στήλης που μπορείτε να κάνετε οι σύνδεσμοι και έχουν τον ίδιο αριθμό διαμερίσματα σε δύο ροές.


## <a name="configure-stream-analytics-job-partition"></a>Ρύθμιση παραμέτρων partition ανάλυση ροής εργασίας

**Για να προσαρμόσετε μια μονάδα ροής για ένα έργο**

1. Πραγματοποιήστε είσοδο [πύλη διαχείρισης](https://manage.windowsazure.com).
2. Στο αριστερό παράθυρο, κάντε κλικ στην επιλογή **Ανάλυσης ροής** .
3. Κάντε κλικ στην επιλογή η εργασία ροής ανάλυσης που θέλετε να περιορίσετε το μέγεθος.
4. Κάντε κλικ στην επιλογή **ΚΛΊΜΑΚΑΣ** στο επάνω μέρος της σελίδας.

![Ανάλυση Azure ροή ροή μονάδες κλίμακας][img.stream.analytics.streaming.units.scale]

Στην πύλη του Azure, ρυθμίσεις κλίμακας είναι δυνατή η πρόσβαση στην περιοχή ρυθμίσεις:

![Azure παραμέτρων εργασίας πύλη ροή ανάλυσης][img.stream.analytics.preview.portal.settings.scale]

## <a name="monitor-job-performance"></a>Παρακολούθηση εργασίας απόδοσης

Χρησιμοποιώντας την πύλη διαχείρισης, μπορείτε να παρακολουθήσετε την απόδοση ενός έργου σε συμβάντα/δευτερόλεπτο:

![Ανάλυση Azure ροή παρακολουθείτε τις εργασίες][img.stream.analytics.monitor.job]

Υπολογίστε τα αναμενόμενα μετάδοσης από το φόρτο εργασίας σε συμβάντα/δευτερόλεπτα. Εάν η απόδοση είναι μικρότερο από το αναμενόμενο, με ακρίβεια τα διαμερίσματα εισαγωγής, με ακρίβεια το ερώτημα και προσθέστε επιπλέον ροή μονάδες με το έργο σας.

## <a name="stream-analytics-throughput-at-scale---raspberry-pi-scenario"></a>Ροή μετάδοσης ανάλυσης με κλίμακα - π Raspberry σεναρίου


Για να κατανοήσετε πώς η ροή εργασιών ανάλυσης κλιμάκωση σε ένα τυπικό σενάριο όσον αφορά την ταχύτητα επεξεργασίας σε πολλές μονάδες ροής, θα δείτε μια έρευνα που στέλνει αισθητήρα δεδομένα (προγράμματα-πελάτες) σε συμβάν διανομέα, επεξεργάζεται και στέλνει ειδοποίησης ή στατιστικά στοιχεία ως το αποτέλεσμα με ένα άλλο διανομέα συμβάν.

Ο υπολογιστής-πελάτης στέλνει συνθετική αισθητήρα δεδομένα με διανομείς συμβάντος σε μορφή JSON για ανάλυση ροή και εξόδου δεδομένων είναι επίσης σε μορφή JSON.  Ακολουθεί πώς θα φαίνεται το δείγμα δεδομένων μου αρέσει –  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Ερώτημα: "Αποστολή ειδοποίησης όταν έχει διακοπεί η ένδειξη"  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

Μέτρηση μετάδοσης: Μετάδοση σε αυτό το πλαίσιο είναι το ποσό της εισαγωγής δεδομένων με ροή ανάλυση υποβάλλονται σε επεξεργασία με ένα σταθερό χρονικό διάστημα (10 λεπτά). Για να επιτύχετε καλύτερη απόδοση επεξεργασίας για τα δεδομένα εισόδου, τόσο η ροή δεδομένων εισόδου και πρέπει να είναι διαμερίσματα του ερωτήματος. Επίσης **COUNT()**περιλαμβάνεται στο ερώτημα για να μετρήσετε πόσες εισαγωγής συμβάντα που έχουν υποβληθεί σε επεξεργασία. Για να βεβαιωθείτε ότι η εργασία δεν απλώς αναμονή για εισαγωγής συμβάντα αναμένεται, κάθε διαμερίσματα του εισαγωγής διανομέα συμβάν ήταν είναι προεγκατεστημένα με επαρκή δεδομένα εισόδου (περίπου 300MB).  

Ακολουθούν τα αποτελέσματα με αυξανόμενη αριθμό μονάδων ροής και αντίστοιχες Partition καταμέτρηση στο συμβάν διανομείς.  

<table border="1">
<tr><th>Τα διαμερίσματα εισαγωγής</th><th>Τα διαμερίσματα εξόδου</th><th>Ροή μονάδες</th><th>Σταθερή μετάδοσης
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

![img.stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Λήψη Βοήθειας
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ Azure ροή ανάλυσης](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Επόμενα βήματα

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings.png

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 