<properties
    pageTitle="Ανάλυση τα πολυμέσα με την πύλη Azure | Microsoft Azure"
    description="Αυτό το θέμα περιγράφει πώς μπορείτε να επεξεργαστείτε τα πολυμέσα με αναλυτικά στοιχεία πολυμέσων πολυμέσων επεξεργαστές (πλάνο) με την πύλη Azure."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="analyze-your-media-using-the-azure-portal"></a>Ανάλυση τα πολυμέσα με την πύλη Azure

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Azure. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="overview"></a>Επισκόπηση

Azure Media υπηρεσιών ανάλυσης είναι μια συλλογή ομιλίας και προβλήματα όρασης τα στοιχεία (στο κλίμακα για μεγάλες επιχειρήσεις, συμμόρφωσης, ασφάλεια και καθολική reach) που είναι ευκολότερο για εταιρείες και επιχειρήσεις να εντοπίζουμε δυνατότητα επεξεργασίας ιδέες από τα αρχεία του βίντεο. Για πιο λεπτομερείς Επισκόπηση της ανάλυσης υπηρεσίες πολυμέσων Azure δείτε [αυτό](media-services-analytics-overview.md) το θέμα. 

Αυτό το θέμα περιγράφει πώς μπορείτε να επεξεργαστείτε τα πολυμέσα με αναλυτικά στοιχεία πολυμέσων πολυμέσων επεξεργαστές (πλάνο) με την πύλη Azure. Πλάνο αναλυτικών στοιχείων πολυμέσων παράγουν MP4 ή JSON αρχείων. Εάν ένα πρόγραμμα επεξεργασίας πολυμέσων που παράγεται ένα αρχείο MP4, μπορείτε να κάνετε λήψη σταδιακά το αρχείο. Εάν ένα πρόγραμμα επεξεργασίας πολυμέσων που παράγεται ένα αρχείο JSON, μπορείτε να κάνετε λήψη του αρχείου από το χώρο αποθήκευσης αντικειμένων blob του Azure. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Επιλογή ενός περιουσιακού στοιχείου που θέλετε να αναλύσετε 
 
1. Στην [πύλη του Azure](https://portal.azure.com/), επιλέξτε το λογαριασμό σας υπηρεσιών Azure Media Services.
2. Στο παράθυρο " **Ρυθμίσεις** ", επιλέξτε **στοιχεία**.  
.
    ![Ανάλυση βίντεο](./media/media-services-portal-analyze/media-services-portal-analyze001.png)

2. Επιλέξτε το πάγιο που θέλετε να αναλύσετε και πατήστε το κουμπί **ανάλυση** .
        
    ![Ανάλυση βίντεο](./media/media-services-portal-analyze/media-services-portal-analyze002.png)

3. Στο παράθυρο **διαθέσιμων στοιχείων πολυμέσων διαδικασία με αναλυτικά στοιχεία πολυμέσων** , επιλέξτε τον επεξεργαστή. 

    Τα υπόλοιπα το άρθρο εξηγεί γιατί και πώς μπορείτε να χρησιμοποιήσετε κάθε επεξεργαστή. 
   
4. Πατήστε **Δημιουργία** για να την έναρξη μιας εργασίας.

## <a name="azure-media-indexer"></a>Ευρετήριο πολυμέσων Azure

Ο επεξεργαστής πολυμέσων **Azure Media ευρετηρίου** σάς επιτρέπει να κάνετε αρχεία πολυμέσων και το περιεχόμενο με δυνατότητα αναζήτησης, καθώς και να δημιουργήσετε κλειστές λεζάντες κομμάτια. Αυτό ενότητες παρέχουν ορισμένες λεπτομέρειες σχετικά με τις επιλογές που μπορείτε να καθορίσετε για αυτό πολλών Επεξεργαστών.

![Ανάλυση βίντεο](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Γλώσσα

Η φυσική γλώσσα να αναγνωρίζονται του αρχείου πολυμέσων. Για παράδειγμα, Αγγλικά ή Ισπανικά. 

### <a name="captions"></a>Λεζάντες

Μπορείτε να επιλέξετε μια μορφή λεζάντα που δημιουργείται από το περιεχόμενό σας. Μια εργασία δημιουργίας ευρετηρίου μπορεί να δημιουργήσει αρχεία κλειστές λεζάντες με τις παρακάτω μορφές:  

- **Σάμι**
- **TTML**
- **WebVTT**

Κλειστό λεζάντα (Κοιν.) αρχεία σε αυτές τις μορφές μπορούν να χρησιμοποιηθούν για να κάνετε προσβάσιμες για άτομα με ειδικές ανάγκες ακοή αρχείων ήχου και βίντεο.

### <a name="aib-file"></a>Αρχείο AIB

Ορίστε αυτήν την επιλογή εάν θέλετε να δημιουργήσετε το αρχείο ήχου ευρετηρίου Blob για χρήση με το προσαρμοσμένο IFilter SQL Server. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αυτό](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) το ιστολόγιο.

### <a name="keywords"></a>Λέξεις-κλειδιά

Ορίστε αυτήν την επιλογή εάν θέλετε να δημιουργήσετε ένα αρχείο XML λέξεις-κλειδιά. Αυτό το αρχείο περιέχει λέξεις-κλειδιά που έχουν εξαχθεί από το περιεχόμενο ομιλίας, με συχνότητα και offset πληροφορίες.

### <a name="job-name"></a>Το όνομα της εργασίας

Ένα φιλικό όνομα που σας επιτρέπει να προσδιορίσετε την εργασία. [Σε αυτό](media-services-portal-check-job-progress.md) το άρθρο περιγράφει πώς μπορείτε να παρακολουθείτε την πρόοδο της εργασίας. 

### <a name="output-file"></a>Αρχείο εξόδου

Ένα φιλικό όνομα που σας επιτρέπει να προσδιορίσετε το περιεχόμενο εξόδου. 

## <a name="azure-media-hyperlapse"></a>Hyperlapse πολυμέσων Azure

Azure Media Hyperlapse είναι μια πολλών Επεξεργαστών που δημιουργεί ομαλές παραγραφεί ώρα βίντεο από το πρώτο άτομο ή ενέργεια κάμερας περιεχόμενο.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αυτό](media-services-hyperlapse-content.md) το θέμα. Αυτό ενότητες παρέχουν ορισμένες λεπτομέρειες σχετικά με τις επιλογές που μπορείτε να καθορίσετε για αυτό πολλών Επεξεργαστών.

![Ανάλυση βίντεο](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Ταχύτητα 

Καθορίστε την ταχύτητα με την οποία για να επιταχύνετε το βίντεο εισαγωγής. Το αποτέλεσμα είναι μια σταθεροποιημένη και παραγραφεί χρόνου εκτέλεσης της οθόνης εισόδου.

### <a name="job-name"></a>Το όνομα της εργασίας

Ένα φιλικό όνομα που σας επιτρέπει να προσδιορίσετε την εργασία. [Σε αυτό](media-services-portal-check-job-progress.md) το άρθρο περιγράφει πώς μπορείτε να παρακολουθείτε την πρόοδο της εργασίας. 

### <a name="output-file"></a>Αρχείο εξόδου

Ένα φιλικό όνομα που σας επιτρέπει να προσδιορίσετε το περιεχόμενο εξόδου. 

## <a name="azure-media-face-detector"></a>Πρόγραμμα ανίχνευσης πρόσωπο πολυμέσων Azure

Ο επεξεργαστής πολυμέσων **Azure Media πρόσωπο ανίχνευσης** (πολλών Επεξεργαστών) σάς επιτρέπει να μέτρηση, να παρακολουθείτε τις κινήσεις και ακόμα και να μετρήσετε συμμετοχής του ακροατηρίου και αντίδρασης μέσω εκφράσεις του προσώπου. Αυτή η υπηρεσία περιέχει δύο δυνατότητες: 

- **Εντοπισμός πρόσωπο**

    Εντοπισμός πρόσωπο που εντοπίζει και παρακολουθεί human όψεις μέσα σε ένα βίντεο. Πολλαπλές όψεις να εντοπιστεί και στη συνέχεια να παρακολουθούνται όπως μετακινήσεων, με ώρα και την τοποθεσία μετα-δεδομένων επιστρέφονται σε ένα αρχείο JSON. Κατά την παρακολούθηση, θα επιχειρήσει να δώσετε μια συνεπή Ταυτότητα για να το ίδιο πρόσωπο, ενώ το άτομο μετακίνηση μέσα στην οθόνη, ακόμα και αν είναι διακόπτεται εμπόδια ή σύντομα αποχώρηση από το πλαίσιο.

    >[AZURE.NOTE]Υπηρεσίες αυτό δεν εκτελεί την αναγνώριση του προσώπου. Ένα άτομο που αφήνει το πλαίσιο ή γίνεται διακόπτεται εμπόδια για μεγάλο χρονικό διάστημα θα έχουν ένα νέο Αναγνωριστικό όταν μπορούν επιστρέψουν.

- **Εντοπισμός συναισθήματα**
    
    Εντοπισμός συναισθήματα είναι ένα προαιρετικό στοιχείο του επεξεργαστή πρόσωπο εντοπισμού πολυμέσων που επιστρέφει ανάλυση σε πολλαπλά συναισθηματικούς χαρακτηριστικά από τις όψεις εντοπιστεί, συμπεριλαμβανομένων των ευτυχία, sadness, φόβου, anger και πολλά άλλα. 

![Ανάλυση βίντεο](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Λειτουργία εντοπισμού

Μία από τις ακόλουθες λειτουργίες μπορούν να χρησιμοποιηθούν από τον επεξεργαστή:

- εντοπισμός πρόσωπο
- ανά εντοπισμού συναισθήματα πρόσωπο
- εντοπισμός συναισθήματα συγκεντρωτικών αποτελεσμάτων

### <a name="job-name"></a>Το όνομα της εργασίας

Ένα φιλικό όνομα που σας επιτρέπει να προσδιορίσετε την εργασία. [Σε αυτό](media-services-portal-check-job-progress.md) το άρθρο περιγράφει πώς μπορείτε να παρακολουθείτε την πρόοδο της εργασίας. 

### <a name="output-file"></a>Αρχείο εξόδου

Ένα φιλικό όνομα που σας επιτρέπει να προσδιορίσετε το περιεχόμενο εξόδου. 

## <a name="azure-media-motion-detector"></a>Πρόγραμμα ανίχνευσης κίνησης πολυμέσων Azure

Ο επεξεργαστής πολυμέσων **Azure Media κίνησης ανίχνευσης** (πολλών Επεξεργαστών) σάς επιτρέπει να αποτελεσματικά αναγνώριση ενότητες που σας ενδιαφέρουν μέσα σε ένα βίντεο διαφορετικά μεγάλη και uneventful. Εντοπισμός κίνησης μπορεί να χρησιμοποιηθεί σε στατικό κάμερας αποσπάσματος για τον προσδιορισμό ενότητες του βίντεο όπου παρουσιάζεται κίνησης. Δημιουργεί ένα αρχείο JSON που περιέχει ένα μετα-δεδομένων με χρονικές σημάνσεις και του πλαισίου περιοχής όπου παρουσιάστηκε το συμβάν.

Στοχεύουν σε τροφοδοσίες βίντεο ασφαλείας, αυτή η τεχνολογία είναι σε θέση να κατηγοριοποιήσετε κίνησης σε σχετικό συμβάντων και false θετικά όπως σκιές και αλλαγές φωτισμού. Αυτό σας επιτρέπει να δημιουργούν ειδοποιήσεων ασφαλείας από τροφοδοσίες κάμερας χωρίς την ανεπιθύμητη αλληλογραφία με απεριόριστες σημασία συμβάντα, κατά τη διαδικασία μπορείτε να εξαγάγετε λεπτά που σας ενδιαφέρουν από πολύ μεγάλο εποπτείας βίντεο.

![Ανάλυση βίντεο](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Μικρογραφίες βίντεο πολυμέσων Azure

Αυτού του επεξεργαστή μπορεί να σας βοηθήσει να δημιουργήσετε συνόψεις μεγάλη βίντεο επιλέγοντας αυτόματα ενδιαφέρον τμήματα κώδικα από το βίντεο προέλευσης. Αυτό είναι χρήσιμο όταν θέλετε να δώσετε μια γρήγορη επισκόπηση των τι να περιμένει στο μεγάλο βίντεο. Για λεπτομερείς πληροφορίες και παραδείγματα, ανατρέξτε στο θέμα [Χρήση Azure Media μικρογραφίες βίντεο για να δημιουργήσετε μια σύνοψη βίντεο](media-services-video-summarization.md)

![Ανάλυση βίντεο](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Το όνομα της εργασίας

Ένα φιλικό όνομα που σας επιτρέπει να προσδιορίσετε την εργασία. [Σε αυτό](media-services-portal-check-job-progress.md) το άρθρο περιγράφει πώς μπορείτε να παρακολουθείτε την πρόοδο της εργασίας. 

### <a name="output-file"></a>Αρχείο εξόδου

Ένα φιλικό όνομα που σας επιτρέπει να προσδιορίσετε το περιεχόμενο εξόδου. 


##<a name="next-steps"></a>Επόμενα βήματα

Εμφανίστε τις υπηρεσίες πολυμέσων διαδικασίες εκμάθησης.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


