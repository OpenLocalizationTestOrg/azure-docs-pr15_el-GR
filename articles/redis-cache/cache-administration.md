<properties 
    pageTitle="Πώς να διαχειριστείτε Azure Redis Cache | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να εκτελέσετε εργασίες διαχείρισης όπως επανεκκινήστε τον υπολογιστή και χρονοδιάγραμμα ενημερώσεις για το Azure Redis Cache"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="how-to-administer-azure-redis-cache"></a>Πώς να διαχειριστείτε Azure Redis Cache

Αυτό το θέμα περιγράφει τον τρόπο για να εκτελέσετε εργασίες διαχείρισης όπως προγραμματισμό ενημερώσεις για τις παρουσίες Azure Redis Cache και την επανεκκίνηση.

>[AZURE.IMPORTANT] Τις ρυθμίσεις και τις δυνατότητες που περιγράφονται σε αυτό το άρθρο είναι διαθέσιμες μόνο για μνήμης cache επίπεδο Premium.


## <a name="administration-settings"></a>Ρυθμίσεις διαχείρισης

Οι ρυθμίσεις του Azure Redis Cache **διαχείρισης** σάς επιτρέπουν να εκτελέσετε τις ακόλουθες εργασίες διαχείρισης για το cache premium. Για να έχετε πρόσβαση στις ρυθμίσεις διαχείρισης, κάντε κλικ στην επιλογή **Ρυθμίσεις** ή **όλες τις ρυθμίσεις** από το Redis Cache blade και μεταβείτε με κύλιση στην ενότητα **Διαχείριση** σε το blade **Ρυθμίσεις** .

![Διαχείριση](./media/cache-administration/redis-cache-administration.png)

-   [Επανεκκινήστε τον υπολογιστή](#reboot)
-   [Προγραμματισμός ενημερώσεων](#schedule-updates)

## <a name="reboot"></a>Επανεκκινήστε τον υπολογιστή

Η **επανεκκίνηση της** blade σάς επιτρέπει να επανεκκίνηση της έναν ή περισσότερους κόμβους από το cache. Αυτό σας επιτρέπει να δοκιμάσετε την εφαρμογή σας για υποστηρίζεται σε περίπτωση αποτυχίας.

![Επανεκκινήστε τον υπολογιστή](./media/cache-administration/redis-cache-reboot.png)

Εάν έχετε ένα cache premium με σύμπλεγμα με δυνατότητα, μπορείτε να επιλέξετε ποια shards της μνήμης cache για να κάνετε επανεκκίνηση.

![Επανεκκινήστε τον υπολογιστή](./media/cache-administration/redis-cache-reboot-cluster.png)

Για να επανεκκίνηση έναν ή περισσότερους κόμβους από το cache, επιλέξτε την επιθυμητή κόμβους και κάντε κλικ στο κουμπί **επανεκκίνηση**. Εάν έχετε ένα cache premium με σύμπλεγμα με δυνατότητα, επιλέξτε το shard(s) να επανεκκίνηση της και, στη συνέχεια, κάντε κλικ στο κουμπί **επανεκκίνηση**. Μετά από μερικά λεπτά, το επιλεγμένο node(s) επανεκκινήστε τον υπολογιστή και είναι πίσω online λίγα λεπτά αργότερα.

Την επίδραση σε εφαρμογές προγράμματος-πελάτη διαφέρει ανάλογα με το node(s) που κάνετε επανεκκίνηση.

-   **Υπόδειγμα** - μετά από την επανεκκίνηση το κύριο κόμβο, Azure Redis Cache αποτυγχάνει επάνω στον κόμβο ρεπλίκα και Προβιβάστε το υπόδειγμα. Κατά την ανακατεύθυνση αυτό, μπορεί να υπάρχει ένα σύντομο διάστημα στις οποίες ενδέχεται να αποτύχει συνδέσεις στο cache.
-   **Εξαρτώμενη** - μετά από την επανεκκίνηση του κόμβου εξαρτώμενη, συνήθως δεν υπάρχει καμία επίδραση στα προγράμματα-πελάτες cache.
-   **Και τα δύο κύριες και εξαρτώμενη** - όταν οι κόμβοι cache είναι επανεκκίνηση, όλα τα δεδομένα που θα χαθεί στο cache και συνδέσεις προς την αποτυχία cache μέχρι τον κύριο κόμβο παρέχεται πάλι σε σύνδεση. Εάν έχετε ρυθμίσει τις παραμέτρους [διατήρησης δεδομένων](cache-how-to-premium-persistence.md), το πιο πρόσφατο αντίγραφο ασφαλείας επανέρχονται όταν συνδεθείτε ξανά το cache. Σημειώστε ότι όλες οι εγγραφές cache που έχουν πραγματοποιηθεί μετά από το πιο πρόσφατο αντίγραφο ασφαλείας θα χαθούν.
-   **Node(s) μιας cache premium με σύμπλεγμα με δυνατότητα** - όταν κάνετε επανεκκίνηση του node(s) μιας cache premium με σύμπλεγμα με δυνατότητα, η συμπεριφορά είναι ίδια με όταν κάνετε επανεκκίνηση της node(s) μιας μη ομαδοποιημένη cache.


>[AZURE.IMPORTANT] Επανεκκινήστε τον υπολογιστή είναι διαθέσιμη μόνο για μνήμης cache επίπεδο Premium.

## <a name="reboot-faq"></a>Επανεκκινήστε συνήθεις Ερωτήσεις

-   [Ποια κόμβος πρέπει να επανεκκίνηση της για να ελέγξετε την εφαρμογή μου;](#which-node-should-i-reboot-to-test-my-application)
-   [Να να επανεκκινήσετε το cache για να καταργήσετε συνδέσεις υπολογιστή-πελάτη;](#can-i-reboot-the-cache-to-clear-client-connections)
-   [Θα να χάσετε δεδομένα από το cache εάν να κάνετε επανεκκίνηση;](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
-   [Μπορώ να επανεκκίνηση της μου cache με τη χρήση του PowerShell, CLI ή άλλα εργαλεία διαχείρισης;](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
-   [Ποιες τιμές σειρές να χρησιμοποιήσετε τη λειτουργικότητα επανεκκινήστε τον υπολογιστή;](#what-pricing-tiers-can-use-the-reboot-functionality)


### <a name="which-node-should-i-reboot-to-test-my-application"></a>Ποια κόμβου να πρέπει να επανεκκίνηση για να ελέγξετε την εφαρμογή μου;

Για να ελέγξετε την ανοχή της εφαρμογής σας σε σχέση με την αποτυχία του πρωτεύοντος κόμβου από το cache, κάντε επανεκκίνηση του κόμβου **υποδείγματος** . Για να ελέγξετε την ανοχή της εφαρμογής σας σε σχέση με την αποτυχία του δευτερεύοντος κόμβου, επανεκκινήστε τον κόμβο **εξαρτώμενη** . Για να ελέγξετε την ανοχή της εφαρμογής σας σε σχέση με το συνολικό την αποτυχία της μνήμης cache, κάντε επανεκκίνηση και στους **δύο** κόμβους.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Να να επανεκκινήσετε το cache για να καταργήσετε συνδέσεις υπολογιστή-πελάτη;

Ναι, εάν κάνετε επανεκκίνηση του cache καταργούνται όλες οι συνδέσεις προγράμματος-πελάτη. Αυτό μπορεί να χρήσιμη σε περίπτωση όπου όλες οι συνδέσεις προγράμματος-πελάτη που χρησιμοποιούνται προς τα επάνω, για παράδειγμα λόγω ένα σφάλμα λογικής ή ένα σφάλμα στην εφαρμογή υπολογιστή-πελάτη. Κάθε επίπεδο τιμολόγησης έχει διαφορετικό [ορίων σύνδεσης προγράμματος-πελάτη](cache-configure.md#default-redis-server-configuration) για τα διάφορα μεγέθη και όταν αυτά τα όρια έχουν φτάσει, δεν υπάρχουν περισσότερες συνδέσεις προγράμματος-πελάτη γίνονται δεκτές. Την επανεκκίνηση του cache παρέχει έναν τρόπο για να καταργήσετε όλες τις συνδέσεις προγράμματος-πελάτη.

>[AZURE.IMPORTANT] Εάν χρησιμοποιούνται συνδέσεις σας προγράμματος-πελάτη οφείλεται σε ένα σφάλμα λογικής ή ένα σφάλμα στον κώδικα του προγράμματος-πελάτη, σημειώστε ότι StackExchange.Redis θα συνδέσετε αυτόματα όταν ο κόμβος Redis είναι πάλι σε σύνδεση. Εάν το υποκείμενο πρόβλημα δεν επιλυθεί, οι συνδέσεις προγράμματος-πελάτη θα εξακολουθήσει να χρησιμοποιηθεί προς τα επάνω.

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Θα να χάσετε δεδομένα από το cache εάν να κάνετε επανεκκίνηση;

Εάν κάνετε επανεκκίνηση τόσο στην **κύρια** και **εξαρτώμενη** κόμβους όλα τα δεδομένα στο cache (ή σε συγκεκριμένη shard Εάν χρησιμοποιείτε ένα cache premium με σύμπλεγμα με δυνατότητα) θα χαθεί. Εάν έχετε ρυθμίσει τις παραμέτρους [διατήρησης δεδομένων](cache-how-to-premium-persistence.md), θα γίνει επαναφορά το πιο πρόσφατο αντίγραφο ασφαλείας, όταν συνδεθείτε ξανά το cache. Σημειώστε ότι όλες οι εγγραφές cache που έχουν προκύψει αφού δημιουργήθηκε το αντίγραφο ασφαλείας θα χαθούν.

Εάν κάνετε επανεκκίνηση της απλώς έναν από τους κόμβους, δεδομένων δεν είναι συνήθως διατηρούνται, αλλά ενδέχεται να είναι ακόμη. Για παράδειγμα, εάν το κύριο κόμβο γίνεται επανεκκίνηση και μια εγγραφή cache είναι σε εξέλιξη, τα δεδομένα από το cache εγγραφής θα χαθεί. Ένα άλλο σενάριο για απώλεια δεδομένων θα ήταν Εάν κάνετε επανεκκίνηση έναν κόμβο και άλλου κόμβου συμβαίνει για να μεταβείτε προς τα κάτω εξαιτίας μιας αποτυχίας την ίδια στιγμή. Για περισσότερες πληροφορίες σχετικά με τις πιθανές αιτίες για απώλεια δεδομένων, ανατρέξτε στο θέμα [Τι συνέβη με τα δεδομένα μου στο Redis;](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md).

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Μπορώ να επανεκκίνηση της μου cache με τη χρήση του PowerShell, CLI ή άλλα εργαλεία διαχείρισης;

Ναι, για το PowerShell οδηγίες ανατρέξτε στο θέμα [επανεκκίνηση της Redis cache](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Ποιες τιμές σειρές να χρησιμοποιήσετε τη λειτουργικότητα επανεκκινήστε τον υπολογιστή;

Επανεκκινήστε τον υπολογιστή είναι διαθέσιμη μόνο σε το premium τις τιμές σε επίπεδο.

## <a name="schedule-updates"></a>Προγραμματισμός ενημερώσεων

Το **Χρονοδιάγραμμα ενημερώσεις** blade σάς επιτρέπει να καθορίσετε ένα παράθυρο συντήρησης για το cache. Όταν το παράθυρο Συντήρηση καθορίζεται, οι ενημερώσεις διακομιστή Redis γίνονται στη διάρκεια αυτού του παραθύρου. Σημειώστε ότι το παράθυρο Συντήρηση ισχύει μόνο για Redis ενημερώσεις διακομιστή, και όχι σε οποιαδήποτε Azure ενημερώνει ή ενημερώσεις για το λειτουργικό σύστημα του του ΣΠΣ που φιλοξενούν το cache.

![Προγραμματισμός ενημερώσεων](./media/cache-administration/redis-schedule-updates.png)

Για να καθορίσετε ένα παράθυρο συντήρησης, επιλέξτε την επιθυμητή ημέρες και καθορίστε την ώρα έναρξης του παραθύρου συντήρησης για κάθε ημέρα και κάντε κλικ στο κουμπί **OK**. Σημειώστε ότι η ώρα παράθυρο συντήρησης είναι στο UTC. 

>[AZURE.NOTE] Η προεπιλεγμένη συντήρησης για ενημερώσεις είναι 5 ωρών. Αυτή η τιμή δεν είναι δυνατό να ρυθμιστούν από την πύλη του Azure, αλλά μπορείτε να ρυθμίσετε το PowerShell χρησιμοποιώντας το `MaintenanceWindow` παράμετρος της το cmdlet [New-AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx) . Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [να μπορεί να Διαχείριση προγραμματισμένες ενημερώσεις με τη χρήση του PowerShell, CLI ή άλλα εργαλεία διαχείρισης;](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)

## <a name="schedule-updates-faq"></a>Προγραμματισμός ενημερώσεων συνήθεις Ερωτήσεις

-   [Όταν γίνονται ενημερώσεις εάν να μην χρησιμοποιείτε τη δυνατότητα ενημερώσεις χρονοδιάγραμμα;](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
-   [Τι τύπο ενημερώσεις πραγματοποιούνται κατά τη διάρκεια του παραθύρου προγραμματισμένη συντήρηση;](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
-   [Μπορούν να διαχειριζόμενων προγραμματισμένες ενημερώσεις με τη χρήση του PowerShell, CLI ή άλλα εργαλεία διαχείρισης;](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
-   [Ποιες τιμές σειρές να χρησιμοποιήσετε τη λειτουργία ενημερώσεις το χρονοδιάγραμμα;](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Όταν γίνονται ενημερώσεις εάν να μην χρησιμοποιείτε τη δυνατότητα ενημερώσεις χρονοδιάγραμμα;

Εάν δεν μπορείτε να καθορίσετε ένα παράθυρο συντήρησης, μπορούν να γίνουν ενημερώσεις οποιαδήποτε στιγμή.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Τι τύπο ενημερώσεις πραγματοποιούνται κατά τη διάρκεια του παραθύρου προγραμματισμένη συντήρηση;

Μόνο Redis διακομιστή ενημερώσεις πραγματοποιούνται κατά τη διάρκεια του παραθύρου προγραμματισμένη συντήρηση. Το παράθυρο Συντήρηση δεν ισχύει για ενημερώσεις του Azure ή ενημερώσεις για το λειτουργικό σύστημα Εικονική.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Μπορούν να διαχειριζόμενων προγραμματισμένες ενημερώσεις με τη χρήση του PowerShell, CLI ή άλλα εργαλεία διαχείρισης;

Ναι, μπορείτε να διαχειριστείτε τις προγραμματισμένες ενημερώσεις χρησιμοποιώντας τα παρακάτω cmdlet του PowerShell.

-   [Get-AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763835.aspx)
-   [Νέα AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763834.aspx)
-   [Νέα AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx)
-   [Κατάργηση AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763837.aspx)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Ποιες τιμές σειρές να χρησιμοποιήσετε τη λειτουργία ενημερώσεις το χρονοδιάγραμμα;

Προγραμματισμός ενημερώσεων είναι διαθέσιμη μόνο σε το premium τις τιμές σε επίπεδο.

## <a name="next-steps"></a>Επόμενα βήματα

-   Εξερευνήστε ακόμη περισσότερες δυνατότητες [Azure Redis Cache premium επίπεδο](cache-premium-tier-intro.md) .




