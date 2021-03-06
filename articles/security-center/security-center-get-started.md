<properties
   pageTitle="Οδηγός γρήγορης εκκίνησης του Azure Κέντρο ασφάλειας | Microsoft Azure"
   description="Σε αυτό το άρθρο σάς βοηθά να γρήγορα αποτελέσματα με το Κέντρο ασφάλειας Azure που καθοδήγησης μέσω τα στοιχεία διαχείρισης ασφαλείας παρακολούθησης και πολιτικής και συνδέοντάς μπορείτε με επόμενα βήματα."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/28/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-quick-start-guide"></a>Οδηγός γρήγορης εκκίνησης του Κέντρο ασφάλειας Azure

Σε αυτό το άρθρο σάς βοηθά να γρήγορα γρήγορα αποτελέσματα με το Κέντρο ασφάλειας Azure, που καθοδήγησης μέσω τα στοιχεία ασφαλείας παρακολούθησης και πολιτικής διαχείρισης του Κέντρου ασφαλείας.

> [AZURE.NOTE] Σε αυτό το άρθρο παρουσιάζει την υπηρεσία, χρησιμοποιώντας μια ανάπτυξη παράδειγμα. Σε αυτό το άρθρο δεν είναι αναλυτικές οδηγίες.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ξεκινήσετε με το Κέντρο ασφάλειας, πρέπει να έχετε μια συνδρομή στο Microsoft Azure. Εάν δεν έχετε μια συνδρομή, μπορείτε να εγγραφείτε για έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/).

Το δωρεάν επίπεδο του Κέντρο ασφάλειας ενεργοποιείται αυτόματα με τη συνδρομή σας και παρέχει ορατότητα στην κατάσταση ασφαλείας Azure τους πόρους σας. Παρέχει διαχείριση πολιτικών ασφαλείας βασικές, συστάσεις ασφαλείας και ενσωμάτωση ασφαλείας προϊόντων και υπηρεσιών από το Azure συνεργάτες.

Μπορείτε να αποκτήσετε πρόσβαση Κέντρο ασφάλειας από την [πύλη του Azure](https://azure.microsoft.com/features/azure-portal/). Για να μάθετε περισσότερα σχετικά με την πύλη του Azure, ανατρέξτε στην [τεκμηρίωση πύλης](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="data-collection"></a>Συλλογή δεδομένων

Κέντρο ασφάλειας συγκεντρώνει δεδομένα από το εικονικές μηχανές (ΣΠΣ) για να αξιολογεί την κατάσταση ασφάλειας τους και παρέχει προτάσεις ασφάλειας σάς ειδοποιεί για απειλές. Όταν έχετε πρώτα πρόσβαση στο Κέντρο ασφάλειας, συλλογή δεδομένων είναι ενεργοποιημένη σε όλα ΣΠΣ στη συνδρομή σας. Συνιστάται η συλλογή δεδομένων, αλλά μπορείτε να επιλέξετε απενεργοποιώντας τη συλλογή δεδομένων στην πολιτική Κέντρο ασφάλειας.

Ακολουθήστε τα παρακάτω βήματα περιγράφουν πώς μπορείτε να αποκτήσετε πρόσβαση και να χρησιμοποιήσετε τα στοιχεία του Κέντρο ασφάλειας. Σε αυτά τα βήματα, θα σας δείξουμε πώς μπορείτε να απενεργοποιήσετε τη συλλογή δεδομένων, εάν επιλέξετε να επιλέξω να μην.

## <a name="access-security-center"></a>Κέντρο ασφάλειας πρόσβασης

Στην πύλη, ακολουθήστε τα παρακάτω βήματα για πρόσβαση στο Κέντρο ασφάλειας:

1. Στο μενού **Microsoft Azure** , επιλέξτε **Κέντρο ασφάλειας**.
![Μενού Azure][1]

2. Εάν έχετε πρόσβαση σε Κέντρο ασφάλειας για πρώτη φορά, ανοίγει το blade **Καλώς ορίσατε** . Επιλέξτε Ναι, **! Θέλω να Κέντρο ασφάλειας Azure εκκίνηση** για να ανοίξετε το **Κέντρο ασφάλειας** blade και για να ενεργοποιήσετε τη συλλογή δεδομένων.
![Οθόνη υποδοχής][10]

3. Μετά την εκκίνηση του Κέντρο ασφάλειας από την υποδοχή blade ή επιλέξτε Κέντρο ασφάλειας από το μενού Microsoft Azure, ανοίγει το **Κέντρο ασφάλειας** blade. Για εύκολη πρόσβαση σε blade το **Κέντρο ασφάλειας** στο μέλλον, ορίστε την επιλογή **Pin blade στον πίνακα εργαλείων** (επάνω δεξιά γωνία).
![Καρφίτσωμα blade επιλογή πίνακα εργαλείων][2]

## <a name="use-security-center"></a>Χρησιμοποιήστε το Κέντρο ασφαλείας

Μπορείτε να ρυθμίσετε πολιτικές ασφαλείας για τις συνδρομές του Azure και τις ομάδες πόρων. Ας ρυθμίσει μια πολιτική ασφαλείας για τη συνδρομή σας:

1. Στην το **Κέντρο ασφάλειας** blade, επιλέξτε το πλακίδιο **πολιτικής** .
![Πολιτική ασφαλείας][3]

2. Στην blade την **πολιτική ασφαλείας - Ορισμός της πολιτικής ανά συνδρομή ή την ομάδα πόρων** , επιλέξτε μια συνδρομή.
3. Η **πολιτική ασφαλείας** blade, **συλλογή δεδομένων** είναι ενεργοποιημένη για τη συλλογή αυτόματα αρχεία καταγραφής. Η επέκταση παρακολούθησης παρέχεται σε όλα τρέχον και το νέο VM στην συνδρομής. (Μπορείτε να επιλέξετε από τη συλλογή δεδομένων με τη ρύθμιση **συλλογής δεδομένων** για να **απενεργοποιήσετε**, αλλά αυτό εμποδίζει το Κέντρο ασφάλειας παρέχοντας ειδοποιήσεων ασφαλείας και συστάσεις.)
4. Στην blade την **πολιτική ασφαλείας** , επιλέξτε το στοιχείο **Επιλογή ενός λογαριασμού χώρου αποθήκευσης ανά περιοχή**. Για κάθε περιοχή στην οποία έχετε ΣΠΣ εκτελείται, μπορείτε να επιλέξετε το λογαριασμό χώρου αποθήκευσης όπου αποθηκεύονται τα δεδομένα που συλλέγονται από αυτά τα ΣΠΣ. Εάν δεν επιλέξετε ένα λογαριασμό χώρου αποθήκευσης για κάθε περιοχή, δημιουργείται για εσάς. Τα δεδομένα που συλλέγονται είναι λογικά απομόνωσης από τους άλλους πελάτες δεδομένα για λόγους ασφαλείας.

     > [AZURE.NOTE] Συνιστάται να που Ενεργοποίηση συλλογής δεδομένων και επιλέξτε πρώτα ένα λογαριασμό χώρου αποθήκευσης στο επίπεδο της συνδρομής. Πολιτικές ασφαλείας μπορεί να οριστεί στο επίπεδο Azure συνδρομής και επίπεδο ομάδας πόρων, αλλά ρύθμιση παραμέτρων για τη συλλογή δεδομένων και το λογαριασμό χώρου αποθήκευσης πραγματοποιείται μόνο σε επίπεδο συνδρομής.

5. Στην blade την **πολιτική ασφαλείας** , επιλέξτε **Αποτροπή πολιτικής**. Έτσι ανοίγει το blade **αποτροπής πολιτικής** .
![Αποτροπή πολιτικής][4]

6. Στην την **πολιτική αποτροπής** blade, ενεργοποιήστε τις συστάσεις που θέλετε να δείτε ως μέρος του πολιτική ασφαλείας σας. Παραδείγματα:

 - Ρύθμιση **ενημερώσεων συστήματος** **στην** σαρώνει όλες οι υποστηριζόμενες εικονικές μηχανές για ενημερώσεις του λειτουργικού Συστήματος που λείπουν.
 - Ρύθμιση **OS ευπάθειες** **στην** σαρώνει όλες οι υποστηριζόμενες εικονικές μηχανές για τον προσδιορισμό τυχόν ρυθμίσεις παραμέτρων λειτουργικό σύστημα που μπορεί να επηρεάσει την εικονική μηχανή περισσότερα σχετικά με επιθέσεις.

### <a name="view-recommendations"></a>Προβολή συστάσεων

1. Επιστρέψτε στο blade το **Κέντρο ασφάλειας** και επιλέξτε το πλακίδιο **συστάσεις** . Κέντρο ασφάλειας αναλύει περιοδικά την κατάσταση ασφαλείας του Azure τους πόρους σας. Όταν το Κέντρο ασφάλειας συναντά πιθανά θέματα ευπάθειας ασφαλείας, που εμφανίζει συστάσεις σε το blade **συστάσεις** .
![Συστάσεις στο Κέντρο ασφάλειας Azure][5]

2.  Επιλέξτε σύσταση σχετικά με το blade **συστάσεις** για να δείτε περισσότερες πληροφορίες ή/και να εκτελέσουν κάποια ενέργεια για να επιλύσετε το ζήτημα.

### <a name="view-the-health-and-security-state-of-your-resources"></a>Προβολή της κατάστασης εύρυθμης λειτουργίας και την ασφάλεια των πόρων σας

1.  Επιστροφή στο blade το **Κέντρο ασφάλειας** . Το πλακίδιο **εύρυθμης λειτουργίας ασφαλείας πόρους** περιέχει δείκτες της κατάστασης ασφαλείας για εικονικές μηχανές, τη δικτύωση, δεδομένων και εφαρμογές.
2.  Επιλέξτε **εικονικές μηχανές** για να δείτε περισσότερες πληροφορίες. Το blade **εικονικές μηχανές** ανοίγει και εμφανίζει μια σύνοψη την κατάσταση των προγραμμάτων λογισμικό κακόβουλης λειτουργίας, ενημερώσεις συστήματος, επανεκκίνηση του και ευπάθειες λειτουργικό σύστημα του ΣΠΣ σας.
![Το πλακίδιο εύρυθμης λειτουργίας πόρους στο Κέντρο ασφάλειας Azure][6]

3.  Επιλέξτε σύσταση στην περιοχή **ΕΙΚΟΝΙΚΉ ΜΗΧΑΝΉ ΣΥΣΤΆΣΕΙΣ** για να δείτε περισσότερες πληροφορίες ή/και εκτέλεση ενεργειών για τη ρύθμιση παραμέτρων απαραίτητα στοιχεία ελέγχου.
4.  Επιλέξτε μια Εικονική στην περιοχή **εικονικές μηχανές** για να δείτε περισσότερες λεπτομέρειες.

### <a name="view-security-alerts"></a>Προβολή ειδοποιήσεων ασφαλείας

1.  Επιστρέψτε στο blade το **Κέντρο ασφάλειας** και επιλέξτε το πλακίδιο **ειδοποιήσεων ασφαλείας** . Το blade **ειδοποιήσεων ασφαλείας** ανοίγει και εμφανίζει μια λίστα με τις ειδοποιήσεις. Το Κέντρο ασφάλειας ανάλυση των αρχείων καταγραφής ασφαλείας και δραστηριότητας δικτύου δημιουργεί αυτές τις ειδοποιήσεις. Περιλαμβάνονται οι ειδοποιήσεις από λύσεις συνεργατών ενσωματωμένη.
![Προειδοποιήσεις ασφαλείας στο Κέντρο ασφάλειας Azure][7]

    > [AZURE.NOTE] Προειδοποιήσεις ασφαλείας είναι διαθέσιμοι μόνο εάν είναι ενεργοποιημένη η τυπική σειρά του Κέντρου ασφαλείας. Μια δωρεάν δοκιμαστική έκδοση 90 ημερών από την τυπική σειρά είναι διαθέσιμη. Για πληροφορίες σχετικά με τον τρόπο για να λάβετε την τυπική σειρά, ανατρέξτε στην ενότητα [επόμενα βήματα](#next-steps) .

2.  Επιλέξτε μια ειδοποίηση για να προβάλετε πρόσθετες πληροφορίες. Σε αυτό το παράδειγμα, ας επιλέξουμε **εντόπισε Τροποποιήθηκε δυαδικό αρχείο συστήματος**. Έτσι ανοίγει λεπίδες που παρέχουν πρόσθετες λεπτομέρειες σχετικά με την ειδοποίηση.
![Λεπτομέρειες της ειδοποίησης ασφαλείας στο Κέντρο ασφάλειας Azure][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Προβάλετε την εύρυθμη λειτουργία της σας λύσεων για συνεργάτες

1. Επιστροφή στο blade το **Κέντρο ασφάλειας** . Το πλακίδιο **λύσεων για συνεργάτες** σάς επιτρέπει να παρακολουθείτε, με μια ματιά, την κατάσταση εύρυθμης λειτουργίας σας λύσεων για συνεργάτες ενσωματωμένο με το Azure τη συνδρομή σας.
2. Επιλέξτε το πλακίδιο **λύσεων για συνεργάτες** . Μια blade ανοίγει και εμφανίζει μια λίστα με τις λύσεις συνεργατών συνδεδεμένοι στο Κέντρο ασφάλειας.
![Λύσεις συνεργατών][9]

3. Επιλέξτε μια λύση συνεργάτη. Σε αυτό το παράδειγμα, ας επιλέξουμε της λύσης **F5 WAF** .  Μια blade ανοίγει και εμφανίζει την κατάσταση της λύσης συνεργάτες και τους συναφείς πόρους της λύσης. Επιλέξτε **κονσόλας λύση** για να ανοίξετε την εμπειρία διαχείρισης συνεργατών για αυτήν τη λύση.

## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το άρθρο έχει εισαχθεί σε τα στοιχεία ασφαλείας παρακολούθησης και πολιτικής διαχείρισης του Κέντρο ασφάλειας. Τώρα που είστε εξοικειωμένοι με το Κέντρο ασφάλειας, δοκιμάστε τα παρακάτω βήματα:

- Ρύθμιση παραμέτρων σε μια πολιτική ασφαλείας για τη συνδρομή σας στο Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ρύθμιση πολιτικών ασφάλειας στο Κέντρο ασφάλειας Azure](security-center-policies.md).
- Χρησιμοποιήστε τις συστάσεις στο Κέντρο ασφάλειας για να σας βοηθήσουν να προστατεύσετε τους πόρους σας Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση ασφαλείας συστάσεων στο Κέντρο ασφάλειας Azure](security-center-recommendations.md).
- Ελέγξτε και να διαχειριστείτε τις ειδοποιήσεις σας τρέχουσα ασφαλείας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση και απάντηση σε ειδοποιήσεις ασφαλείας στο Κέντρο ασφάλειας Azure](security-center-managing-and-responding-alerts.md).
- Μάθετε περισσότερα σχετικά με το [προηγμένες δυνατότητες εντοπισμού απειλή](security-center-detection-capabilities.md) που παρέχονται με το [τυπικό επίπεδο](security-center-pricing.md) του Κέντρου ασφαλείας. Μια δωρεάν δοκιμαστική έκδοση 90 ημερών από την τυπική σειρά είναι διαθέσιμη.
- Εάν έχετε ερωτήσεις σχετικά με τη χρήση του Κέντρου ασφαλείας, ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις για το Κέντρο ασφάλειας Azure](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
