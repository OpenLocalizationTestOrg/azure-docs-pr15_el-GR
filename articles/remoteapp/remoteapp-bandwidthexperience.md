<properties 
    pageTitle="Azure RemoteApp - πώς εύρος ζώνης δικτύου και την ποιότητα της εμπειρία εργασίας μαζί; | Microsoft Azure"
    description="Μάθετε πώς το εύρος ζώνης δικτύου στο Azure RemoteApp μπορεί να επηρεάσει την του χρήστη την ποιότητα της εμπειρίας."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp - πώς εύρος ζώνης δικτύου και την ποιότητα της εμπειρία εργασίας μαζί;

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Όταν κοιτάζετε του [συνολικού εύρους ζώνης δικτύου](remoteapp-bandwidth.md) που απαιτούνται για το Azure RemoteApp, λάβετε υπόψη τους παρακάτω παράγοντες - αυτές αποτελούν μέρος όλα δυναμικό σύστημα που επηρεάζει τη συνολική εμπειρία χρήστη. 

- **Διαθέσιμο εύρος ζώνης δικτύου και τρέχουσες συνθήκες του δικτύου** - ένα σύνολο παραμέτρων (λανθάνων χρόνος, απώλεια jitter) στο ίδιο δίκτυο σε μια δεδομένη στιγμή μπορεί να επηρεάσει την εφαρμογή εμπειρία ροής, γεγονός που σημαίνει ότι χαμηλωμένο συνολική εμπειρία χρήστη. Το εύρος ζώνης που είναι διαθέσιμες στο δίκτυό σας είναι μια συνάρτηση συμφόρηση, τυχαία απώλεια, λανθάνων χρόνος επειδή όλες αυτές οι παράμετροι επηρεάζουν το μηχανισμό ελέγχου συμφόρηση, οποίο με τη σειρά στοιχεία ελέγχου ταχύτητας μετάδοσης για να αποφύγετε διενέξεις.  Για παράδειγμα, ένα δίκτυο με απώλειες ή σε δίκτυο με μεγάλο λανθάνοντα χρόνο θα κάνουν την εμπειρία του χρήστη εσφαλμένες ακόμη και σε ένα δίκτυο με το εύρος ζώνης 1000 MB. Η απώλεια και λανθάνων χρόνος ποικίλλουν ανάλογα με τον αριθμό των χρηστών που βρίσκονται στο ίδιο δίκτυο και τι κάνουν οι χρήστες (για παράδειγμα, παρακολουθώντας βίντεο, τη λήψη ή την αποστολή μεγάλων αρχείων, εκτύπωση).
- **Σενάριο χρήσης** - την εμπειρία εξαρτάται από το τι οι χρήστες κάνουν ως άτομα και ως ομάδα στο ίδιο δίκτυο. Για παράδειγμα, μία διαφάνεια ανάγνωσης απαιτεί ένα μόνο καρέ πρόκειται να ενημερωθεί. Εάν ο χρήστης skims και κύλιση προς τα επάνω από το περιεχόμενο του εγγράφου κειμένου, που χρειάζονται υψηλότερο αριθμό των πλαισίων για να ενημερωθούν ανά δευτερόλεπτο. Επικοινωνία και πίσω με το διακομιστή σε αυτό το σενάριο θα εμφανίσει εκμετάλλευση περισσότερες εύρους ζώνης δικτύου. Επίσης να λάβετε υπόψη ακραίες παράδειγμα: πολλών χρηστών παρακολουθώντας βίντεο υψηλής ευκρίνειας (όπως ανάλυση 4K), κρατώντας πατημένο το HD κλήσεις διάσκεψης, παιχνιδιών 3Δ βίντεο ή εργασία σε συστήματα CAD. Όλα αυτά μπορούν να κάνουν σχεδόν χωρίς δυνατότητα χρήσης ακόμα πολύ υψηλή εύρους ζώνης δικτύου.
- **Ανάλυση οθόνης και του αριθμού των οθονών** - περισσότερες εύρος ζώνης δικτύου απαιτείται για την πλήρη ενημέρωση οθόνες μεγαλύτερο από μικρότερες οθόνες. Η υποκείμενη τεχνολογία κάνει μια εργασία αρκετά καλή κωδικοποίηση και μετάδοση μόνο τις περιοχές οθόνες που έχουν ενημερωθεί, αλλά κάθε τόσο, ολόκληρη την οθόνη πρέπει να ενημερωθεί. Όταν ο χρήστης έχει μια υψηλότερη ανάλυση οθόνης (για παράδειγμα ανάλυση 4K), αυτήν την ενημερωμένη έκδοση απαιτεί μεγαλύτερο εύρος ζώνης δικτύου από μια οθόνη με ανάλυση κάτω (όπως 1024x768px). Αυτή η ίδια λογική ισχύουν εάν χρησιμοποιείτε περισσότερες από μία οθόνη για την ανακατεύθυνση. Το εύρος ζώνης πρέπει να αυξήσετε με τον αριθμό των οθονών.
- **Ανακατεύθυνση Πρόχειρο και συσκευή** - αυτό είναι ένα ζήτημα δεν πολύ εμφανή, αλλά σε πολλές περιπτώσεις εάν ένας χρήστης αποθηκεύει ένα μεγάλο τμήμα των δεδομένων στο Πρόχειρο, χρειάζονται λίγο χρόνο για αυτές τις πληροφορίες για τη μεταφορά από τον υπολογιστή-πελάτη απομακρυσμένης επιφάνειας εργασίας με το διακομιστή. Η μετάδοση εμπειρία μπορεί να αντιμετωπίσουν προβλήματα εξαιτίας την εμπειρία αποστολής πριν το περιεχόμενο του Προχείρου. Το ίδιο ισχύει για ανακατεύθυνση συσκευής - εάν σαρωτή ή κάμερα web παράγει πολλά δεδομένα που πρέπει να σταλεί νέα στο διακομιστή, ή έναν εκτυπωτή πρέπει να λάβετε ένα μεγάλο έγγραφο, ή τοπικού χώρου αποθήκευσης πρέπει να είναι διαθέσιμες σε μια εφαρμογή που εκτελείται στο cloud για να αντιγράψετε ένα μεγάλο αρχείο, οι χρήστες ενδέχεται να παρατηρήσετε καρέ ή προσωρινά βίντεο "σταθεροποιημένη" επειδή τα δεδομένα που χρειάζονται για την ανακατεύθυνση της συσκευής όλο το εύρος ζώνης δικτύου χρειάζεται. 

Κατά την αξιολόγηση ανάγκες σας εύρους ζώνης δικτύου, βεβαιωθείτε ότι πρέπει να λάβετε υπόψη όλους τους παρακάτω παράγοντες που λειτουργούν σαν ένα σύστημα.

Τώρα, επιστρέψτε στο [άρθρο εύρους ζώνης δικτύου κύριο](remoteapp-bandwidth.md)ή μετακινήστε δοκιμές το [εύρος ζώνης δικτύου](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Μάθε περισσότερα
- [Εκτίμηση της χρήσης του εύρους ζώνης δικτύου Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp - δοκιμές σας χρήσης του εύρους ζώνης δικτύου με ορισμένα κοινά σενάρια](remoteapp-bandwidthtests.md)

- [Azure RemoteApp εύρος ζώνης δικτύου - γενικές οδηγίες (Εάν δεν μπορείτε να ελέγξετε τη δική σας)](remoteapp-bandwidthguidelines.md)