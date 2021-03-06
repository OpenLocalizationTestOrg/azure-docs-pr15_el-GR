
<properties
    pageTitle="Επαναφορά τοποθεσίας Azure: Αναπαραγωγή Hyper-V εικονικές μηχανές σε ένα διακομιστή VMM | Microsoft Azure"
    description="Σε αυτό το άρθρο περιγράφει πώς μπορείτε να αναπαραγάγετε το Hyper-V εικονικές μηχανές όταν έχετε μόνο μία VMM διακομιστή."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/24/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-on-a-single-vmm-server"></a>Αναπαραγωγή Hyper-V εικονικές μηχανές σε ένα διακομιστή VMM

Διαβάστε αυτό το άρθρο για να μάθετε πώς μπορείτε να αναπαραγάγετε το Hyper-V εικονικές μηχανές βρίσκεται σε ένα διακομιστή host Hyper-V σε ένα σύννεφο VMM όταν έχετε μόνο ένα διακομιστή VMM στην ανάπτυξή σας.

Azure περιλαμβάνει δύο διαφορετικά [μοντέλα ανάπτυξης](../resource-manager-deployment-model.md) για τη δημιουργία και εργασία με πόρους: Διαχείριση πόρων Azure και κλασική. Azure έχει επίσης δύο πύλες – Azure κλασική πύλης που υποστηρίζει το μοντέλο κλασική ανάπτυξης και το Azure πύλης με την υποστήριξη για δύο μοντέλα ανάπτυξης. Σε αυτό το άρθρο περιέχει οδηγίες για τη ρύθμιση αναπαραγωγής στην πύλη του Azure.


Εάν έχετε ερωτήσεις και μετά την ανάγνωση αυτό το άρθρο, δημοσιεύστε τα στα σχόλια Disqus στο κάτω μέρος αυτού του άρθρου ή στο [φόρουμ υπηρεσίες ανάκτησης Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Επισκόπηση

Εάν θέλετε να αναπαραγάγετε το Hyper-V ΣΠΣ που βρίσκεται στο Hyper-V κεντρικούς υπολογιστές VMM και έχετε μόνο ένα διακομιστή VMM, μπορείτε να κάνετε [Αναπαραγωγή σε Azure](site-recovery-vmm-to-azure.md), ή μεταξύ σύννεφων στο μεμονωμένο διακομιστή VMM.

Συνιστάται να κάνετε αναπαραγωγή για να Azure επειδή ανακατεύθυνσης και αποκατάστασης δεν είναι απρόσκοπτη κατά την αναπαραγωγή ανάμεσα σε σύννεφων και έναν αριθμό των μη αυτόματα βήματα είναι απαραίτητες. Εάν θέλετε να αναπαραγάγετε χρησιμοποιώντας μόνο το διακομιστή VMM, μπορείτε να κάνετε τα εξής:

- Αναπαραγωγή με μία μεμονωμένη VMM διακομιστή
- Αναπαραγωγή με ένα διακομιστή VMM αναπτυχθεί σε ένα σύμπλεγμα Windows παραμόρφωσης


## <a name="replicate-across-sites-with-a-single-standalone-vmm-server"></a>Αναπαραγωγή σε τοποθεσίες με μία μεμονωμένη VMM διακομιστή

![Εικονικός διακομιστής VMM μεμονωμένη](./media/site-recovery-single-vmm/single-vmm-standalone.png)

Σε αυτό το σενάριο ανάπτυξη μεμονωμένο διακομιστή VMM ως μια εικονική μηχανή στην κύρια τοποθεσία και αναπαραγάγετε αυτό Εικονική σε μια δευτερεύουσα τοποθεσία μέσω Επαναφορά τοποθεσίας και ρεπλίκα Hyper-V.

1. Ρύθμιση του VMM σε μια Εικονική Hyper-V. Προτείνουμε να εγκαταστήσετε την παρουσία του SQL Server που χρησιμοποιείται από VMM στην την ίδια εικονική Μηχανή για να εξοικονομήσετε χρόνο. Εάν θέλετε να χρησιμοποιήσετε μια απομακρυσμένη παρουσία του SQL Server και μιας μη διαθεσιμότητα πραγματοποιείται, πρέπει να ανακτήσετε την παρουσία πριν μπορείτε να ανακτήσετε VMM.
2. Βεβαιωθείτε ότι ο διακομιστής VMM έχει τουλάχιστον δύο σύννεφων έχει ρυθμιστεί. Ένα σύννεφο περιέχει του ΣΠΣ που θέλετε να αναπαραγάγετε και άλλες cloud χρησιμοποιείται ως δευτερεύουσα θέση. Θα πρέπει να έχετε στο cloud που περιέχει το ΣΠΣ που επιθυμείτε να προστατεύσετε:

    - Ένα ή περισσότερα VMM ομάδες κεντρικών υπολογιστών που περιέχει έναν ή περισσότερους διακομιστές host Hyper-V σε κάθε ομάδα κεντρικού υπολογιστή.
    - Τουλάχιστον μία Hyper-V εικονική μηχανή σε κάθε διακομιστή Hyper-V κεντρικού υπολογιστή.

3. Δημιουργήστε ένα θάλαμο υπηρεσίες ανάκτησης, δημιουργία και λήψη ένα κλειδί εγγραφής θάλαμο και καταχώρηση στο διακομιστή VMM στο το θάλαμο. Κατά την εγγραφή, μπορείτε να εγκαταστήσετε το Azure τοποθεσίας αποκατάστασης υπηρεσία παροχής που χρησιμοποιείτε στο διακομιστή VMM.
4. Ρύθμιση ενός ή περισσότερων σύννεφων στην η Εικονική VMM και προσθέστε τους κεντρικούς υπολογιστές Hyper-V σε αυτές τις σύννεφων.
3. Ρύθμιση παραμέτρων προστασίας για την σύννεφων. Μπορείτε να καθορίσετε το όνομα του διακομιστή VMM μόνο με τις τοποθεσίες προέλευσης και προορισμού. Για να ρυθμίσετε τις παραμέτρους αντιστοίχιση δικτύου, μπορείτε να αντιστοιχίσετε στο δίκτυο Εικονική για το cloud με το ΣΠΣ που θέλετε να προστατεύσετε, στο δίκτυο Εικονική για το cloud αναπαραγωγής.
4. Ενεργοποίηση αρχικό αναπαραγωγής για ΣΠΣ που επιθυμείτε να προστατεύσετε μέσω του δικτύου, καθώς και τα δύο σύννεφων βρίσκονται στον ίδιο διακομιστή.
4. Στην κονσόλα διαχείρισης Hyper-V, ενεργοποίηση ρεπλίκα Hyper-V στον κεντρικό υπολογιστή Hyper-V που περιέχει την Εικονική VMM και ενεργοποιήσετε την αναπαραγωγή σε η Εικονική. Βεβαιωθείτε ότι δεν μπορείτε να προσθέσετε την Εικονική VMM σε οποιαδήποτε σύννεφων που προστατεύονται από Επαναφορά τοποθεσίας. Αυτό εξασφαλίζει ότι δεν είναι παρακαμφθούν ρυθμίσεις ρεπλίκα Hyper-V από Επαναφορά τοποθεσίας.
5. Εάν θέλετε να δημιουργήσετε σχέδια ανάκτησης, μπορείτε να καθορίσετε το ίδιο διακομιστή VMM για προέλευσης και προορισμού.

Ακολουθήστε τις οδηγίες σε [αυτό το άρθρο](site-recovery-vmm-to-vmm.md) για να δημιουργήσετε ένα θάλαμο, εγγραφείτε στο διακομιστή και ρύθμιση προστασίας.

### <a name="what-to-do-in-an-outage"></a>Τι πρέπει να κάνετε σε μια μη διαθεσιμότητα

Εάν εμφανιστεί μια ολοκληρωμένη μη διαθεσιμότητα και πρέπει να λειτουργούν από τη δευτερεύουσα τοποθεσία, κάντε τα εξής:

1.  Στην κονσόλα διαχείρισης Hyper-V στη δευτερεύουσα τοποθεσία, εκτελέστε μια μη προγραμματισμένη ανακατεύθυνσης να ανακατευθύνει την εικονική Μηχανή VMM από την κύρια δευτερεύουσα τοποθεσία.
2.  Βεβαιωθείτε ότι η Εικονική VMM είναι εγκατάσταση και λειτουργία στη δευτερεύουσα τοποθεσία.
3.  Το θάλαμο υπηρεσίες ανάκτησης, εκτελέστε μια μη προγραμματισμένη ανακατεύθυνσης αποτυχία πάνω από το φόρτο εργασίας ΣΠΣ από την κύρια σε δευτερεύοντα σύννεφων. Για να ολοκληρώσετε τη μη προγραμματισμένη ανακατεύθυνσης των του ΣΠΣ, ολοκλήρωση του εφεδρικού ή επιλέξτε ένα διαφορετικό αποκατάστασης σημείο όπως απαιτείται.
4.  Μετά την ολοκλήρωση του προγραμματισμένου εφεδρικού, οι χρήστες πρόσβαση φόρτο εργασίας πόρων σε δευτερεύουσα τοποθεσία.

Όταν η κύρια τοποθεσία λειτουργεί κανονικά ξανά, κάντε τα εξής:

1.  Στην κονσόλα διαχείρισης Hyper-V, ενεργοποίηση αντίστροφη αναπαραγωγή για την εικονική Μηχανή VMM, για να ξεκινήσετε την αναπαραγωγή από το δευτερεύον σε κύρια.
2.  Στην κονσόλα διαχείρισης Hyper-V, εκτελέστε μια προγραμματισμένη ανακατεύθυνσης για να επιστρέψετε αποτύχει η Εικονική VMM στην κύρια τοποθεσία. Ολοκλήρωση του εφεδρικού για να ολοκληρώσετε το. Στη συνέχεια, ενεργοποίηση αντίστροφη αναπαραγωγή για να ξεκινήσει η αναπαραγωγή του VMM από το κύριο σε δευτερεύοντα.
3.  Το θάλαμο υπηρεσίες ανάκτησης, ενεργοποίηση αντίστροφη αναπαραγωγή για το φόρτο εργασίας ΣΠΣ, για να ξεκινήσετε την αναπαραγωγή τους από το δευτερεύον σε κύρια.
4.  Το θάλαμο υπηρεσίες ανάκτησης, εκτελέστε μια προγραμματισμένη ανακατεύθυνσης αποτυχία ξανά το φόρτο εργασίας ΣΠΣ στην κύρια τοποθεσία. Ολοκλήρωση του εφεδρικού για να ολοκληρώσετε το. Στη συνέχεια, ενεργοποίηση αντίστροφη αναπαραγωγή για να ξεκινήσει η αναπαραγωγή του φόρτου εργασίας ΣΠΣ από το κύριο σε δευτερεύοντα.



## <a name="replicate-across-sites-with-a-single-vmm-server-in-a-stretched-cluster"></a>Αναπαραγωγή σε τοποθεσίες με ένα διακομιστή VMM σε ένα σύμπλεγμα παραμόρφωσης

![Ομαδοποιημένη εικονικού διακομιστή VMM](./media/site-recovery-single-vmm/single-vmm-cluster.png)

Αντί να αναπτύξετε ένα αυτόνομο διακομιστή VMM ως μια Εικονική που αναπαράγεται σε μια δευτερεύουσα τοποθεσία, μπορείτε να κάνετε VMM ιδιαίτερα διαθέσιμη από την ανάπτυξή του ως μια Εικονική στα ένα σύμπλεγμα ανακατεύθυνσης των Windows. Αυτό παρέχει ανθεκτικότητα φόρτο εργασίας και της προστασίας από αποτυχία υλικού. Για να αναπτύξετε με Επαναφορά τοποθεσίας η Εικονική VMM θα πρέπει να αναπτυχθούν σε ένα σύμπλεγμα παραμόρφωσης σε γεωγραφικά ξεχωριστές τοποθεσίες. Για να το κάνετε αυτό:

1. Εγκατάσταση VMM σε μια εικονική μηχανή σε ένα σύμπλεγμα ανακατεύθυνσης των Windows και ενεργοποιήστε την επιλογή για να εκτελέσετε το διακομιστή ως ιδιαίτερα διαθέσιμες κατά την εγκατάσταση.
2. Παρουσίας του SQL Server που χρησιμοποιείται από VMM πρέπει να αναπαραχθούν με ομάδες διαθεσιμότητας SQL Server AlwaysOn, έτσι ώστε να υπάρχει μια ρεπλίκα της βάσης δεδομένων στην δευτερεύουσα τοποθεσία.
3. Ακολουθήστε τις οδηγίες σε [αυτό το άρθρο](site-recovery-vmm-to-vmm.md) για να δημιουργήσετε ένα θάλαμο, εγγραφείτε στο διακομιστή και ρύθμιση προστασίας. Πρέπει να καταχωρήσετε κάθε διακομιστή VMM του συμπλέγματος στο το θάλαμο υπηρεσίες ανάκτησης. Για να το κάνετε αυτό, εγκατάσταση της υπηρεσίας παροχής σε ένα ενεργό κόμβο και καταχώρηση VMM διακομιστή. Στη συνέχεια, μπορείτε να εγκαταστήσετε την υπηρεσία παροχής σε άλλους κόμβους.

### <a name="what-to-do-in-an-outage"></a>Τι πρέπει να κάνετε σε μια μη διαθεσιμότητα

Όταν προκύπτει μια μη διαθεσιμότητα, ο διακομιστής VMM και το αντίστοιχο βάση δεδομένων SQL Server απέτυχε μέσω και έχετε πρόσβαση σε αυτές από τη δευτερεύουσα τοποθεσία.


## <a name="next-steps"></a>Επόμενα βήματα

[Μάθετε περισσότερα](site-recovery-vmm-to-vmm.md) σχετικά με τη λεπτομερή Επαναφορά τοποθεσίας για ανάπτυξη VMM VMM αναπαραγωγή.
