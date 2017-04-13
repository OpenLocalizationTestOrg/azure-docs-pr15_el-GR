<properties
    pageTitle="Βελτίωση της απόδοσης με τη συμπίεση αρχείων στο Azure CDN | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να βελτιώσετε την ταχύτητα μεταφοράς αρχείων και αυξάνουν απόδοσης φόρτωση των σελίδων συμπιέζοντας τα αρχεία σας σε Azure CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="improve-performance-by-compressing-files"></a>Βελτίωση της απόδοσης με τη συμπίεση αρχείων

Συμπίεση είναι μια απλή και αποτελεσματική μέθοδο για βελτίωση της ταχύτητας μεταφοράς αρχείων και αύξηση απόδοσης φόρτωση των σελίδων, μειώνοντας το μέγεθος αρχείου πριν από την αποστολή από το διακομιστή. Το μειώνει το εύρος ζώνης κόστος και παρέχει μια πιο ευέλικτη εμπειρία για τους χρήστες σας.

Υπάρχουν δύο τρόποι για να ενεργοποιήσετε τη συμπίεση:

- Μπορείτε να ενεργοποιήσετε τη συμπίεση στο διακομιστή προέλευσης, στο οποίο πεζών-κεφαλαίων το CDN θα διέρχονται από τα συμπιεσμένα αρχεία και να παρουσιάζετε συμπιεσμένα αρχεία σε υπολογιστές-πελάτες που αίτηση τους.
- Μπορείτε να ενεργοποιήσετε τη συμπίεση απευθείας σε διακομιστές edge CDN, στο οποίο θα Συμπιέστε τα αρχεία το CDN πεζών-κεφαλαίων και να χρησιμοποιηθεί στους τελικούς χρήστες, ακόμα και αν δεν συμπιέζονται από το διακομιστή προέλευσης.

> [AZURE.IMPORTANT] Αλλαγές στη ρύθμιση παραμέτρων CDN χρειαστεί κάποιος χρόνος για τη μετάδοση μέσω του δικτύου.  Για τα προφίλ <b>CDN Azure από Akamai</b> , η μετάδοση ολοκληρώνεται συνήθως σε ένα λεπτό στην περιοχή.  Για τα προφίλ <b>CDN Azure από Verizon</b> , συνήθως βλέπετε τις αλλαγές σας ισχύουν 90 λεπτά.  Εάν αυτή είναι η πρώτη φορά που έχετε ρυθμίσει τη συμπίεση για το τελικό σημείο CDN, πρέπει να εξετάσετε αναμονή 1-2 ώρες για να βεβαιωθείτε ότι η συμπίεση έχει έχουν μεταδοθεί ρυθμίσεις για να το εμφανίζεται ένα πριν από την αντιμετώπιση προβλημάτων

## <a name="enabling-compression"></a>Ενεργοποίηση συμπίεσης

> [AZURE.NOTE] Τις τυπικές και Premium CDN βαθμίδες παρέχουν την ίδια λειτουργικότητα συμπίεσης, αλλά διαφέρει το περιβάλλον εργασίας χρήστη.  Για περισσότερες πληροφορίες σχετικά με τις διαφορές μεταξύ κλίμακες τυπικές και Premium CDN, ανατρέξτε στο θέμα [Επισκόπηση CDN Azure](cdn-overview.md).

### <a name="standard-tier"></a>Τυπική σειρά

> [AZURE.NOTE] Αυτή η ενότητα ισχύει για το **Azure CDN τυπική από Verizon** και **Azure CDN τυπική από Akamai** προφίλ.

1. Από το προφίλ blade CDN, κάντε κλικ στην επιλογή το τελικό σημείο CDN που θέλετε να διαχειριστείτε.

    ![CDN τα τελικά σημεία blade προφίλ](./media/cdn-file-compression/cdn-endpoints.png)

    Ανοίγει το blade CDN τελικού σημείου.

2. Κάντε κλικ στο κουμπί **Ρύθμιση παραμέτρων** .

    ![Κουμπί "Διαχείριση CDN blade προφίλ"](./media/cdn-file-compression/cdn-config-btn.png)

    Ανοίγει το blade CDN ρύθμισης παραμέτρων.

3. Ενεργοποιήστε **τη συμπίεση**.

    ![Επιλογές συμπίεσης CDN](./media/cdn-file-compression/cdn-compress-standard.png)

4. Χρησιμοποιήστε τους προεπιλεγμένους τύπους, ή να τροποποιήσετε τη λίστα, καταργώντας ή προσθήκη τύπων αρχείων.
    
    > [AZURE.TIP] Χρόνος είναι δυνατόν, δεν συνιστάται να εφαρμόσετε τη συμπίεση σε συμπιεσμένο μορφές, όπως ZIP, MP3, MP4, JPG, κ.λπ.
    
5. Αφού κάνετε τις αλλαγές σας, κάντε κλικ στο κουμπί **Αποθήκευση** .

### <a name="premium-tier"></a>Επίπεδο Premium

> [AZURE.NOTE] Αυτή η ενότητα ισχύει για το προφίλ **Premium CDN Azure από Verizon** .

1. Από το προφίλ blade CDN, κάντε κλικ στο κουμπί **Διαχείριση** .

    ![Κουμπί "Διαχείριση CDN blade προφίλ"](./media/cdn-file-compression/cdn-manage-btn.png)

    Ανοίγει η πύλη διαχείρισης του CDN.

2. Hover πάνω από την καρτέλα **Μεγάλο HTTP** , στη συνέχεια, hover πάνω από το αναδυόμενο **Ρυθμίσεις του Cache** .  Κάντε κλικ στην επιλογή **συμπίεση**.

    Εμφανίζονται οι επιλογές συμπίεσης.

    ![Συμπίεση αρχείων](./media/cdn-file-compression/cdn-compress-files.png)

3. Ενεργοποίηση συμπίεσης κάνοντας κλικ στο κουμπί επιλογής **Συμπίεση ενεργοποιημένη** .  Εισαγάγετε τους τύπους MIME που θέλετε να συμπιέσετε ως λίστα διαχωρισμένο με κόμματα (χωρίς κενά διαστήματα) στο πλαίσιο κειμένου **Τύπους αρχείων** .
        
    > [AZURE.TIP] Χρόνος είναι δυνατόν, δεν συνιστάται να εφαρμόσετε τη συμπίεση σε συμπιεσμένο μορφές, όπως ZIP, MP3, MP4, JPG, κ.λπ. 

4. Αφού κάνετε τις αλλαγές σας, κάντε κλικ στο κουμπί **Ενημέρωση** .


## <a name="compression-rules"></a>Κανόνες συμπίεσης

Σε αυτούς τους πίνακες περιγράφουν τη συμπεριφορά συμπίεσης Azure CDN για κάθε σενάριο.

> [AZURE.IMPORTANT] Για **CDN Azure από Verizon** (τυπική και Premium), μόνο επιλέξιμο αρχεία συμπιέζονται.  Για να είναι επιλέξιμη για συμπίεσης, ένα αρχείο πρέπει να:
>
> - Είναι μεγαλύτερο από 128 byte.
> - Είναι μικρότερο του 1 MB.
> 
> Για **CDN Azure από Akamai**, όλα τα αρχεία είναι επιλέξιμα από τη συμπίεση.
>
> Για όλα τα προϊόντα Azure CDN, ένα αρχείο πρέπει να είναι ένας τύπος MIME που έχει [ρυθμιστεί για τη συμπίεση](#enabling-compression).
>
> Προφίλ **CDN Azure από Verizon** (τυπική και Premium) υποστηρίζει **gzip**, **κοίλωμα**ή κωδικοποίηση **bzip2** .  Προφίλ **CDN Azure από Akamai** υποστηρίζει μόνο κωδικοποίηση **gzip** .
>
> Τα τελικά σημεία **CDN Azure από Akamai** αίτηση πάντα αρχεία **gzip** κωδικοποιημένη από την προέλευση, ανεξάρτητα από την αίτηση προγράμματος-πελάτη.

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Συμπίεση απενεργοποιηθεί ή το αρχείο είναι μη επιλέξιμη για τη συμπίεση

|Μορφή που ζητήθηκε προγράμματος-πελάτη (μέσω κωδικοποίηση αποδοχή κεφαλίδα)|Μορφή αρχείων στο cache|Απόκριση CDN στον υπολογιστή-πελάτη|Σημειώσεις|
|----------------|-----------|------------|-----|
|Συμπίεση|Συμπίεση|Συμπίεση|   |
|Συμπίεση|Αρχεία μη συμπιεσμένων|Αρχεία μη συμπιεσμένων|    | 
|Συμπίεση|Δεν στο cache|Συμπιεσμένη ή χωρίς συμπίεση|Εξαρτάται από το origin απόκρισης|
|Αρχεία μη συμπιεσμένων|Συμπίεση|Αρχεία μη συμπιεσμένων|    |
|Αρχεία μη συμπιεσμένων|Αρχεία μη συμπιεσμένων|Αρχεία μη συμπιεσμένων|    |   
|Αρχεία μη συμπιεσμένων|Δεν στο cache|Αρχεία μη συμπιεσμένων|     |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Ενεργοποιημένη συμπίεση και αρχείο είναι επιλέξιμο για τη συμπίεση

|Μορφή που ζητήθηκε προγράμματος-πελάτη (μέσω κωδικοποίηση αποδοχή κεφαλίδα)|Μορφή αρχείων στο cache|Απόκριση CDN στον υπολογιστή-πελάτη|Σημειώσεις|
|----------------|-----------|------------|-----|
|Συμπίεση|Συμπίεση|Συμπίεση|CDN transcodes μεταξύ υποστηριζόμενες μορφές|
|Συμπίεση|Αρχεία μη συμπιεσμένων|Συμπίεση|CDN εκτελεί τη συμπίεση|
|Συμπίεση|Δεν στο cache|Συμπίεση|CDN εκτελεί τη συμπίεση εάν origin επιστρέφει αρχεία μη συμπιεσμένων.  **Azure CDN από Verizon** περάσει τα αρχεία μη συμπιεσμένων αρχείο στην πρώτη αίτηση και, στη συνέχεια, συμπίεση και προσωρινή αποθήκευση του αρχείου για επόμενες αιτήσεις.  Αρχεία με `Cache-Control: no-cache` κεφαλίδα ποτέ θα συμπιέζονται. 
|Αρχεία μη συμπιεσμένων|Συμπίεση|Αρχεία μη συμπιεσμένων|CDN εκτελεί αποσυμπίεση|
|Αρχεία μη συμπιεσμένων|Αρχεία μη συμπιεσμένων|Αρχεία μη συμπιεσμένων|     |  
|Αρχεία μη συμπιεσμένων|Δεν στο cache|Αρχεία μη συμπιεσμένων|     |

## <a name="media-services-cdn-compression"></a>Υπηρεσίες CDN Συμπίεση πολυμέσων

Για CDN υπηρεσίες πολυμέσων με δυνατότητα ροής τελικά σημεία, συμπίεση είναι ενεργοποιημένη από προεπιλογή για τους ακόλουθους τύπους περιεχομένου: εφαρμογή/vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, εφαρμογή/f4m + xml. Που δεν είναι δυνατό να ενεργοποιούν/απενεργοποιούν συμπίεσης για τους προαναφερθέντες τύπους με την πύλη Azure.  

## <a name="see-also"></a>Δείτε επίσης
- [Αντιμετώπιση προβλημάτων CDN συμπίεση αρχείων](cdn-troubleshoot-compression.md)    