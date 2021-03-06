<properties
    pageTitle="Εκκαθάριση τελικού σημείου Azure CDN | Microsoft Azure"
    description="Μάθετε πώς να εκκαθάρισης όλο το περιεχόμενο στο cache από ένα τελικό σημείο CDN."
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

# <a name="purge-an-azure-cdn-endpoint"></a>Εκκαθάριση τελικού σημείου Azure CDN

## <a name="overview"></a>Επισκόπηση

Azure κόμβους άκρη CDN θα cache παγίων μέχρι να λήξει του παγίου time to live (TTL).  Μετά τη λήξη του παγίου TTL, όταν ένας υπολογιστής-πελάτης ζητά παγίου από τον κόμβο ακμή, ο κόμβος άκρο θα ανακτήσει μια νέα ενημερωμένου αντιγράφου του περιουσιακού στοιχείου για να εξυπηρετήσει την αίτηση προγράμματος-πελάτη και αποθήκευσης ανανέωση του cache.

Μερικές φορές μπορεί να θέλετε να εκκαθάρισης στο cache περιεχόμενο από όλους τους κόμβους άκρου και αναγκάζετε όλων για να ανακτήσετε νέων ενημερωμένων περιουσιακών στοιχείων.  Αυτό μπορεί να οφείλεται σε ενημερώσεις στην εφαρμογή web σας, ή για να update γρήγορα τα στοιχεία που περιέχουν εσφαλμένες πληροφορίες.

> [AZURE.TIP] Σημειώστε ότι εκκαθάριση καταργεί μόνο το περιεχόμενο της cache στους διακομιστές άκρη CDN.  Οποιαδήποτε ροής μνήμης cache, όπως διακομιστές μεσολάβησης και οι cache τοπικό πρόγραμμα περιήγησης, μπορεί να εξακολουθεί να κρατήστε πατημένο το πλήκτρο προσωρινά αποθηκευμένο αντίγραφο του αρχείου.  Είναι σημαντικό να θυμάστε αυτό κατά τη ρύθμιση ενός αρχείου time to live.  Μπορείτε να επιβάλετε μια ροής προγράμματος-πελάτη για να ζητήσετε την πιο πρόσφατη έκδοση του αρχείου σας, παρέχοντας ένα μοναδικό όνομα κάθε φορά που μπορείτε να την ενημερώσετε ή με να εκμεταλλευτείτε [σε cache συμβολοσειρά ερωτήματος](cdn-query-string.md).  

Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί σε εκκαθάριση περιουσιακών στοιχείων από όλους τους κόμβους άκρο του τελικού σημείου.

## <a name="walkthrough"></a>Αναλυτικές οδηγίες

1. Στην [Πύλη του Azure](https://portal.azure.com), μεταβείτε στο προφίλ CDN που περιέχει το τελικό σημείο που θέλετε να εκκαθαρίσετε.

2. Από το προφίλ blade CDN, κάντε κλικ στο κουμπί Εκκαθάριση.

    ![CDN blade προφίλ](./media/cdn-purge-endpoint/cdn-profile-blade.png)

    Ανοίγει η εκκαθάριση blade.

    ![CDN εκκαθάριση blade](./media/cdn-purge-endpoint/cdn-purge-blade.png)

3. Στην την εκκαθάριση blade, επιλέξτε τη διεύθυνση που θέλετε να εκκαθαρίσετε από την αναπτυσσόμενη λίστα διεύθυνση URL της υπηρεσίας.

    ![Εκκαθάριση φόρμας](./media/cdn-purge-endpoint/cdn-purge-form.png)

    > [AZURE.NOTE] Μπορείτε επίσης να λάβετε το blade εκκαθάριση, κάνοντας κλικ στο κουμπί **Εκκαθάριση** του blade τελικού σημείου CDN.  Σε αυτή την περίπτωση, το πεδίο **διεύθυνση URL** θα εκ των προτέρων συμπληρωμένες με τη διεύθυνση της υπηρεσίας που συγκεκριμένες τελικού σημείου.

4. Επιλέξτε ποια στοιχεία που θέλετε να εκκαθάρισης από τους κόμβους άκρο.  Εάν θέλετε να απαλείψετε όλα τα περιουσιακά στοιχεία, επιλέξτε το πλαίσιο ελέγχου **Εκκαθάριση όλων** .  Διαφορετικά, πληκτρολογήστε την πλήρη διαδρομή κάθε περιουσιακού στοιχείου που θέλετε να εκκαθαρίσετε (π.χ., `/pictures/kitten.png`) στο πλαίσιο κειμένου **διαδρομή** .

    > [AZURE.TIP] Περισσότερα πλαίσια κειμένου **διαδρομή** θα εμφανίζονται μετά την εισαγωγή του κειμένου ώστε να μπορείτε να δημιουργήσετε μια λίστα με πολλά στοιχεία.  Μπορείτε να διαγράψετε στοιχεία από τη λίστα κάνοντας κλικ στο κουμπί με τα αποσιωπητικά (...).
    >
    > Διαδρομές πρέπει να είναι μια σχετική διεύθυνση URL που χωρά την ακόλουθη [κανονική έκφραση](https://msdn.microsoft.com/library/az24scfc.aspx): `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`.  Για **CDN Azure από Verizon** (τυπική και Premium), αστερίσκος (\*) μπορεί να χρησιμοποιηθεί ως χαρακτήρας μπαλαντέρ (π.χ., `/music/*`).  Με το **Azure CDN από Akamai**δεν επιτρέπεται χαρακτήρες μπαλαντέρ και **Εκκαθάριση όλων** .
    
5. Κάντε κλικ στο κουμπί **Εκκαθάριση** .

    ![Κουμπί Εκκαθάριση](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [AZURE.IMPORTANT] Εκκαθάριση αιτήσεις χρειαστεί περίπου 2-3 λεπτά για την επεξεργασία με **CDN Azure από Verizon** (τυπική και Premium) και περίπου 7 λεπτά με **CDN Azure από Akamai**.  Azure CDN έχει όριο 50 ταυτόχρονες εκκαθάριση αιτήσεις οποιαδήποτε στιγμή. 

## <a name="see-also"></a>Δείτε επίσης
- [Προ-φόρτωση περιουσιακών στοιχείων σε ένα τελικό σημείο Azure CDN](cdn-preload-endpoint.md)
- [Azure CDN REST API αναφορά - εκκαθάριση ή προ-φόρτωση ενός ορίου](https://msdn.microsoft.com/library/mt634451.aspx)
