<properties 
    pageTitle="Εισαγωγή στο επίπεδο Azure Redis Cache Premium | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε Redis διατήρησης, Redis σύμπλεγμα και VNET υποστήριξη για τις παρουσίες Premium επίπεδο Azure Redis Cache" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Εισαγωγή στο επίπεδο Azure Redis Cache Premium
Azure Redis Cache είναι μια κατανεμημένη, διαχειριζόμενων cache που σας βοηθά να δημιουργήσετε εφαρμογές ιδιαίτερα μεταβλητού μεγέθους και αποκρίνεται, παρέχοντας εξαιρετικά γρήγορη πρόσβαση στα δεδομένα σας. 

Η νέα σειρά Premium είναι μια σειρά είστε έτοιμοι για μεγάλες επιχειρήσεις, που περιλαμβάνει όλες τις δυνατότητες τυπική επιπέδων και πολλά άλλα, όπως καλύτερες επιδόσεις, μεγαλύτερο φόρτους εργασίας, αποκατάσταση, εισαγωγή/εξαγωγή και βελτιωμένη ασφάλεια. Συνεχίστε να διαβάζετε για να μάθετε περισσότερα σχετικά με τις πρόσθετες δυνατότητες από την κλίμακα cache Premium.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Καλύτερη απόδοση σε σύγκριση με το επίπεδο τυπική ή Basic
**Καλύτερα επιδόσεων επάνω σε τυπική ή βασικές επίπεδο.** Οι cache στο επίπεδο Premium έχουν αναπτυχθεί σε υλικού, το οποίο έχει ταχύτερους επεξεργαστές και σας προσφέρει καλύτερη απόδοση σε σύγκριση στη βασική ή τυπική σειρά. Οι cache επίπεδο Premium έχουν μεγαλύτερη ταχύτητα μετάδοσης και των αδρανειών κάτω. 

**Απόδοση για το ίδιο μέγεθος Cache είναι μεγαλύτερη στο Premium σε σύγκριση με τυπική σειρά.** Για παράδειγμα, η ταχύτητα μεταγωγής μια 53 GB P4 μνήμης cache (Premium) είναι 250K αιτήσεις ανά δευτερόλεπτο σε σύγκριση με 150 K για C6 (τυπική).

Για περισσότερες πληροφορίες σχετικά με το μέγεθος, μετάδοσης και εύρους ζώνης με μνήμης cache premium, ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις Cache Redis Azure](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis διατήρησης δεδομένων
Η σειρά Premium σάς επιτρέπει να διατηρούνται τα δεδομένα cache στο χώρο αποθήκευσης Azure λογαριασμού. Σε μια βασική/τυπική μνήμη cache όλα τα δεδομένα αποθηκεύονται μόνο στη μνήμη. Σε περίπτωση υποκείμενα υποδομή θέματα μπορεί να είναι ενδεχόμενη απώλεια δεδομένων. Συνιστάται να χρησιμοποιείτε τη δυνατότητα διατήρησης δεδομένων Redis στο επίπεδο Premium για να αυξήσετε υποστηρίζεται από την απώλεια δεδομένων. Azure Redis Cache προσφέρει RDB και AOF (σύντομα διαθέσιμο) επιλογές στο [Redis διατήρησης](http://redis.io/topics/persistence). 

Για οδηγίες σχετικά με τη ρύθμιση των παραμέτρων διατήρησης, δείτε [πώς μπορείτε να ρυθμίσετε τις παραμέτρους διατήρησης για Premium Azure Redis μνήμης Cache](cache-how-to-premium-persistence.md).

##<a name="redis-cluster"></a>Redis συμπλέγματος
Εάν θέλετε να δημιουργήσετε μεγαλύτερη από 53 GB μνήμης cache ή θέλετε να shard δεδομένων σε πολλούς κόμβους Redis, μπορείτε να χρησιμοποιήσετε Redis σύμπλεγμα το οποίο είναι διαθέσιμο στο επίπεδο Premium. Κάθε κόμβο αποτελείται από ένα ζεύγος cache κύρια/ρεπλίκα ελέγχονται από Azure για υψηλή διαθεσιμότητα. 

**Σύμπλεγμα redis παρέχει μέγιστη κλίμακα και απόδοση.** Μετάδοσης γραμμικά αυξάνεται καθώς αυξάνετε τον αριθμό των shards (κόμβοι) στο σύμπλεγμα. Π.χ. Εάν δημιουργήσετε ένα σύμπλεγμα P4 10 shards και, στη συνέχεια, η διαθέσιμη ταχύτητα μετάδοσης είναι 250K * 10 = 2,5 εκατομμύρια αιτήσεις ανά δευτερόλεπτο. Ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις Cache Redis Azure](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) για περισσότερες λεπτομέρειες σχετικά με το μέγεθος, μετάδοσης και εύρους ζώνης με premium μνήμης cache.

Για να ξεκινήσετε με το σύμπλεγμα, δείτε [πώς μπορείτε να ρυθμίσετε τις παραμέτρους του συμπλέγματος για Premium Azure Redis μνήμης Cache](cache-how-to-premium-clustering.md).

##<a name="enhanced-security-and-isolation"></a>Βελτιωμένη ασφάλεια και την απομόνωση

Οι cache που έχουν δημιουργηθεί με τη σειρά Basic ή τυπική είναι δυνατή η πρόσβαση στο internet, δημόσια. Πρόσβαση στο cache είναι περιορισμένη με βάση το κλειδί πρόσβασης. Με τη σειρά Premium μπορείτε περαιτέρω να εξασφαλίσετε ότι μόνο τα προγράμματα-πελάτες μέσα σε ένα συγκεκριμένο δίκτυο πρόσβαση του Cache. Μπορείτε να αναπτύξετε Redis Cache σε ένα [Azure εικονικού δικτύου (VNet)](https://azure.microsoft.com/services/virtual-network/). Μπορείτε να χρησιμοποιήσετε όλες τις δυνατότητες της VNet όπως δευτερεύοντα δίκτυα, πολιτικών του ελέγχου πρόσβασης, και άλλες δυνατότητες για να περιορίσετε την πρόσβαση σε Redis.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να ρυθμίσετε τις παραμέτρους εικονικού δικτύου υποστήριξης για Premium Azure Redis μνήμης Cache](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Εισαγωγή/εξαγωγή

Εισαγωγή/εξαγωγή είναι μια λειτουργία διαχείρισης δεδομένων Azure Redis Cache που σας επιτρέπει να εισαγάγετε δεδομένα στο Azure Redis Cache ή εξαγωγή δεδομένων από το Azure Redis Cache κατά την εισαγωγή και εξαγωγή ένα στιγμιότυπο της βάσης δεδομένων Redis Cache (RDB) από ένα cache premium σε μια σελίδα blob σε ένα λογαριασμό του χώρου αποθήκευσης Azure. Αυτό σας επιτρέπει να μετεγκατάσταση μεταξύ διαφορετικό παρουσιών Azure Redis Cache ή τη συμπλήρωση του cache με δεδομένα πριν από τη χρήση.

Εισαγωγή μπορεί να χρησιμοποιηθεί για να μεταφέρετε αρχεία συμβατά RDB Redis από οποιονδήποτε διακομιστή Redis λειτουργεί σε οποιοδήποτε cloud ή το περιβάλλον, συμπεριλαμβανομένων των Redis εκτελείται στο Linux, των Windows ή σε οποιαδήποτε υπηρεσία παροχής cloud όπως υπηρεσίες Web του Amazon και οι άλλοι χρήστες. Εισαγωγή δεδομένων είναι ένας εύκολος τρόπος για να δημιουργήσετε ένα cache με εκ των προτέρων συμπληρωμένες δεδομένων. Κατά τη διαδικασία εισαγωγής, Azure Redis Cache φορτώνει τα αρχεία RDB από Azure χώρο αποθήκευσης στη μνήμη και, στη συνέχεια, εισάγει τα πλήκτρα σε το cache.

Εξαγωγή σάς επιτρέπει να εξαγάγετε τα δεδομένα που είναι αποθηκευμένα στο Cache Redis Azure να Redis συμβατά αρχεία RDB. Μπορείτε να χρησιμοποιήσετε αυτήν τη δυνατότητα για τη μετακίνηση δεδομένων από μία παρουσία Azure Redis Cache σε ένα άλλο ή σε άλλον διακομιστή Redis. Κατά τη διαδικασία εξαγωγής, δημιουργείται ένα προσωρινό αρχείο στο η Εικονική που φιλοξενεί την παρουσία server Azure Redis Cache και το αρχείο αποστέλλεται στο λογαριασμό που έχει οριστεί ως χώρου αποθήκευσης. Όταν ολοκληρωθεί η λειτουργία εξαγωγής με είτε μια κατάσταση επιτυχίας ή αποτυχίας, διαγράφεται το προσωρινό αρχείο.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να εισαγάγετε τα δεδομένα στο και εξαγωγή δεδομένων από το Azure Redis Cache](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Επανεκκινήστε τον υπολογιστή

Η σειρά premium σάς επιτρέπει να επανεκκίνηση της έναν ή περισσότερους κόμβους από το cache στην-ζήτηση. Αυτό σας επιτρέπει να δοκιμάσετε την εφαρμογή σας για υποστηρίζεται σε περίπτωση αποτυχίας. Μπορείτε να πραγματοποιήσετε επανεκκίνηση τους ακόλουθους κόμβους.

-   Κύριο κόμβο από το cache
-   Εξαρτώμενη κόμβο από το cache
-   Υπόδειγμα και εξαρτώμενη κόμβους από το cache
-   Όταν χρησιμοποιείτε ένα cache premium με σύμπλεγμα, μπορείτε να πραγματοποιήσετε επανεκκίνηση του υποδείγματος, εξαρτώμενη ή και τα δύο κόμβους για μεμονωμένα shards στο cache

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [επανεκκινήστε](cache-administration.md#reboot) και [Επανεκκίνηση της συνήθεις Ερωτήσεις](cache-administration.md#reboot-faq).

## <a name="schedule-updates"></a>Προγραμματισμός ενημερώσεων

Η δυνατότητα προγραμματισμένες ενημερώσεις σάς επιτρέπει να καθορίσετε ένα παράθυρο συντήρησης για το cache. Όταν το παράθυρο Συντήρηση καθορίζεται, οι ενημερώσεις διακομιστή Redis γίνονται στη διάρκεια αυτού του παραθύρου. Για να καθορίσετε ένα παράθυρο συντήρησης, επιλέξτε την επιθυμητή ημέρες και καθορίστε τη διατήρηση παραθύρου Έναρξη ώρα για κάθε ημέρα. Σημειώστε ότι η ώρα παράθυρο συντήρησης είναι στο UTC. 

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Προγραμματισμός ενημερώσεις](cache-administration.md#schedule-updates) και [Χρονοδιάγραμμα συνήθεις Ερωτήσεις](cache-administration.md#schedule-updates-faq).

>[AZURE.NOTE] Μόνο Redis διακομιστή ενημερώσεις πραγματοποιούνται κατά τη διάρκεια του παραθύρου προγραμματισμένη συντήρηση. Το παράθυρο Συντήρηση δεν ισχύει για ενημερώσεις του Azure ή ενημερώσεις για το λειτουργικό σύστημα Εικονική.

## <a name="to-scale-to-the-premium-tier"></a>Για να κλιμακωθεί στο επίπεδο premium

Για να κλιμακωθεί στο επίπεδο premium, επιλέξτε μία από τις σειρές premium απλώς στο το blade **Αλλαγή επιπέδων τις τιμές** . Μπορείτε επίσης να κλιμάκωση το cache στο επίπεδο premium με χρήση του PowerShell και CLI. Για οδηγίες βήμα προς βήμα, ανατρέξτε στο θέμα [Πώς να Cache Redis Azure κλίμακα](cache-how-to-scale.md) και [τον τρόπο για την αυτοματοποίηση μιας λειτουργίας κλίμακας](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Επόμενα βήματα

Δημιουργήστε ένα cache και Εξερευνήστε τις νέες δυνατότητες επίπεδο premium.

-   [Τρόπος ρύθμισης των παραμέτρων διατήρησης για Premium Azure Redis μνήμης Cache](cache-how-to-premium-persistence.md)
-   [Πώς μπορείτε να ρυθμίσετε τις παραμέτρους εικονικού δικτύου υποστήριξης για Premium Azure Redis μνήμης Cache](cache-how-to-premium-vnet.md)
-   [Τρόπος ρύθμισης των παραμέτρων του συμπλέγματος για Premium Azure Redis μνήμης Cache](cache-how-to-premium-clustering.md)
-   [Πώς να εισαγωγή δεδομένων σε και εξαγωγή δεδομένων από το Azure Redis Cache](cache-how-to-import-export-data.md)
-   [Πώς να διαχειριστείτε Azure Redis Cache](cache-administration.md)
  

