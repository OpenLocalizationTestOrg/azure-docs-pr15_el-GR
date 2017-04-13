<properties 
    pageTitle="Εισαγωγή και εξαγωγή δεδομένων στο Azure Redis Cache | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να εισαγωγή και εξαγωγή δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων blob με τις παρουσίες Azure Redis Cache premium" 
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

# <a name="import-and-export-data-in-azure-redis-cache"></a>Εισαγωγή και εξαγωγή δεδομένων στο Azure Redis Cache

Εισαγωγή/εξαγωγή είναι μια λειτουργία διαχείρισης δεδομένων Azure Redis Cache, που σας επιτρέπει να εισαγάγετε δεδομένα στο Azure Redis Cache ή εξαγωγή δεδομένων από το Azure Redis Cache κατά την εισαγωγή και εξαγωγή ένα στιγμιότυπο της βάσης δεδομένων Redis Cache (RDB) από ένα cache premium σε μια σελίδα blob σε ένα λογαριασμό του χώρου αποθήκευσης Azure. Εισαγωγή/εξαγωγή σάς επιτρέπει να μετεγκαταστήσετε μεταξύ παρουσιών διαφορετικό Azure Redis Cache ή τη συμπλήρωση του cache με δεδομένα πριν από τη χρήση.

Σε αυτό το άρθρο παρέχει έναν οδηγό για την εισαγωγή και εξαγωγή δεδομένων με το Azure Redis Cache και τις απαντήσεις σε συνήθεις ερωτήσεις.

>[AZURE.IMPORTANT] Εισαγωγή/εξαγωγή είναι σε προεπισκόπηση και είναι διαθέσιμη μόνο για το [επίπεδο premium](cache-premium-tier-intro.md) μνήμης cache.

## <a name="import"></a>Εισαγωγή

Εισαγωγή μπορεί να χρησιμοποιηθεί για να μεταφέρετε αρχεία συμβατά RDB Redis από οποιονδήποτε διακομιστή Redis λειτουργεί σε οποιοδήποτε cloud ή το περιβάλλον, συμπεριλαμβανομένων των Redis εκτελείται στο Linux, των Windows ή σε οποιαδήποτε υπηρεσία παροχής cloud όπως υπηρεσίες Web του Amazon και οι άλλοι χρήστες. Εισαγωγή δεδομένων είναι ένας εύκολος τρόπος για να δημιουργήσετε ένα cache με εκ των προτέρων συμπληρωμένες δεδομένων. Κατά τη διαδικασία εισαγωγής, Azure Redis Cache φορτώνει τα αρχεία RDB από Azure χώρο αποθήκευσης στη μνήμη και, στη συνέχεια, εισάγει τα πλήκτρα σε το cache.

>[AZURE.NOTE] Πριν να ξεκινήσετε τη λειτουργία εισαγωγής, βεβαιωθείτε ότι το αρχείο βάσης δεδομένων Redis (RDB) ή αρχεία αποστέλλονται σε αντικείμενα BLOB σελίδας στο Azure χώρου αποθήκευσης, στην ίδια περιοχή και συνδρομής ως σας παρουσία Azure Redis Cache. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob του Azure](../storage/storage-dotnet-how-to-use-blobs.md). Εάν εξαγάγατε το αρχείο RDB χρησιμοποιώντας τη δυνατότητα [Azure Redis εξαγωγή Cache](#export) , το αρχείο RDB είναι ήδη αποθηκευμένο σε μια σελίδα blob και είναι έτοιμα για εισαγωγή.

1. Για να εισαγάγετε μία ή περισσότερες εξαγόμενα cache αντικειμένων blob, [Αναζήτηση για να το cache](cache-configure.md#configure-redis-cache-settings) στην πύλη του Azure και κάντε κλικ στην επιλογή **Εισαγωγή δεδομένων** από το blade **Ρυθμίσεις** του cache παρουσίας.

    ![Εισαγωγή δεδομένων][cache-import-data]

2. Κάντε κλικ στο κουμπί **Επιλογή Blob(s)** και επιλέξτε το λογαριασμό χώρου αποθήκευσης που περιέχει τα δεδομένα για να εισαγάγετε.

    ![Επιλέξτε το λογαριασμό χώρου αποθήκευσης][cache-import-choose-storage-account]

3. Κάντε κλικ στο κοντέινερ που περιέχει τα δεδομένα για να εισαγάγετε.

    ![Επιλογή κοντέινερ][cache-import-choose-container]

4. Επιλέξτε ένα ή περισσότερα αντικείμενα BLOB για να εισαγάγετε κάνοντας κλικ στην περιοχή στα αριστερά του ονόματος blob και, στη συνέχεια, κάντε κλικ στην **επιλογή**.

    ![Επιλέξτε αντικείμενα BLOB][cache-import-choose-blobs]

5. Κάντε κλικ στην επιλογή **Εισαγωγή** για να ξεκινήσετε τη διαδικασία εισαγωγής.

    >[AZURE.IMPORTANT] Κατά τη διαδικασία εισαγωγής δεν είναι προσβάσιμη από προγράμματα-πελάτες του cache του cache και διαγράφεται οποιαδήποτε υπάρχοντα δεδομένα στο cache.

    ![Εισαγωγή][cache-import-blobs]

    Μπορείτε να παρακολουθήσετε την πρόοδο της λειτουργίας εισαγωγής, ακολουθώντας τις ειδοποιήσεις από την πύλη του Azure ή προβάλλοντας τα συμβάντα του [ελέγχου συνδεθείτε](cache-configure.md#support-amp-troubleshooting-settings).

    ![Πρόοδος εισαγωγής][cache-import-data-import-complete] 


## <a name="export"></a>Εξαγωγή

Εξαγωγή σάς επιτρέπει να εξαγάγετε τα δεδομένα που είναι αποθηκευμένα στο Cache Redis Azure να Redis συμβατά αρχεία RDB. Μπορείτε να χρησιμοποιήσετε αυτήν τη δυνατότητα για τη μετακίνηση δεδομένων από μία παρουσία Azure Redis Cache σε ένα άλλο ή σε άλλον διακομιστή Redis. Κατά τη διαδικασία εξαγωγής, δημιουργείται ένα προσωρινό αρχείο στο η Εικονική που φιλοξενεί την παρουσία server Azure Redis Cache και το αρχείο αποστέλλεται στο λογαριασμό που έχει οριστεί ως χώρου αποθήκευσης. Όταν ολοκληρωθεί η λειτουργία εξαγωγής με είτε μια κατάσταση επιτυχίας ή αποτυχίας, διαγράφεται το προσωρινό αρχείο.

1. Για να εξαγάγετε τα τρέχοντα περιεχόμενα του cache με το χώρο αποθήκευσης, [Αναζητήστε το cache](cache-configure.md#configure-redis-cache-settings) στην πύλη του Azure και κάντε κλικ στην επιλογή **Εξαγωγή δεδομένων** από το blade **Ρυθμίσεις** του cache παρουσίας.

    ![Επιλέξτε κοντέινερ χώρου αποθήκευσης][cache-export-data-choose-storage-container]

2. Κάντε κλικ στην επιλογή **Επιλέξτε κοντέινερ χώρου αποθήκευσης** και επιλέξτε το λογαριασμό του χώρου αποθήκευσης που θέλετε. Το λογαριασμό χώρου αποθήκευσης πρέπει να είναι στην ίδια συνδρομή και περιοχής ως το cache.

    >[AZURE.IMPORTANT] Εισαγωγή/εξαγωγή λειτουργεί με αντικείμενα BLOB σελίδας, που υποστηρίζονται από το κλασικό και λογαριασμούς ARM χώρου αποθήκευσης, αλλά δεν υποστηρίζονται από [λογαριασμούς χώρο αποθήκευσης Blob](../storage/storage-blob-storage-tiers.md#blob-storage-accounts) αυτήν τη στιγμή.

    ![Το λογαριασμό χώρου αποθήκευσης][cache-export-data-choose-account]

3. Επιλέξτε το επιθυμητό blob κοντέινερ και κάντε κλικ στην **επιλογή**. Για να χρησιμοποιήσετε νέες ένα κοντέινερ, κάντε κλικ στην επιλογή **Προσθήκη κοντέινερ** για να προσθέσετε πρώτα και, στη συνέχεια, επιλέξτε την από τη λίστα.

    ![Επιλέξτε κοντέινερ χώρου αποθήκευσης][cache-export-data-container]

4. Πληκτρολογήστε ένα **πρόθεμα ονόματος Blob** και κάντε κλικ στην επιλογή " **Εξαγωγή** " για να ξεκινήσετε τη διαδικασία εξαγωγής. Το πρόθεμα ονόματος blob χρησιμοποιείται για τα ονόματα των αρχείων που δημιουργούνται από αυτήν τη λειτουργία εξαγωγής πρόθεμα.

    ![Εξαγωγή][cache-export-data]

    Μπορείτε να παρακολουθήσετε την πρόοδο της λειτουργίας εξαγωγής, ακολουθώντας τις ειδοποιήσεις από την πύλη του Azure ή προβάλλοντας τα συμβάντα του [ελέγχου συνδεθείτε](cache-configure.md#support-amp-troubleshooting-settings).

    ![][cache-export-data-export-complete]

    Οι cache παραμένουν διαθέσιμες για χρήση κατά τη διαδικασία εξαγωγής.


## <a name="importexport-faq"></a>Εισαγωγή/εξαγωγή συνήθεις Ερωτήσεις

Αυτή η ενότητα περιέχει συνήθεις ερωτήσεις σχετικά με τη δυνατότητα εισαγωγής/εξαγωγής.

-   [Ποιες τιμές σειρές να χρησιμοποιήσετε εισαγωγή/εξαγωγή;](#what-pricing-tiers-can-use-importexport)
-   [Μπορώ να εισάγω δεδομένα από οποιαδήποτε Redis διακομιστή;](#can-i-import-data-from-any-redis-server)
-   [Θα μου cache είναι διαθέσιμο στη διάρκεια μιας λειτουργίας εισαγωγής/εξαγωγής;](#will-my-cache-be-available-during-an-importexport-operation)
-   [Μπορώ να χρησιμοποιήσω εισαγωγή/εξαγωγή με σύμπλεγμα Redis;](#can-i-use-importexport-with-redis-cluster)
-   [Πώς λειτουργεί η εισαγωγή/εξαγωγή με ένα προσαρμοσμένο βάσεις δεδομένων;](#how-does-importexport-work-with-a-custom-databases-setting)
-   [Σε τι διαφέρει εισαγωγή/εξαγωγή από διατήρηση Redis;](#how-is-importexport-different-from-redis-persistence)
-   [Μπορώ να αυτοματοποιήσω εισαγωγή/εξαγωγή με χρήση του PowerShell, CLI ή άλλα προγράμματα-πελάτες διαχείρισης;](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
-   [Έλαβα ένα σφάλμα χρονικού ορίου κατά τη διάρκεια της λειτουργίας μου εισαγωγή/εξαγωγή. Τι σημαίνει αυτό;](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
-   [Έλαβα ένα μήνυμα σφάλματος κατά την εξαγωγή δεδομένων μου για χώρο αποθήκευσης Blob του Azure. Τι έγινε?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)


### <a name="what-pricing-tiers-can-use-importexport"></a>Ποιες τιμές σειρές να χρησιμοποιήσετε εισαγωγή/εξαγωγή;

Εισαγωγή/εξαγωγή είναι διαθέσιμη μόνο σε το premium τις τιμές σε επίπεδο.

### <a name="can-i-import-data-from-any-redis-server"></a>Μπορώ να εισάγω δεδομένα από οποιαδήποτε Redis διακομιστή;

Ναι, εκτός από την εισαγωγή των δεδομένων που έχουν εξαχθεί από τις εμφανίσεις Azure Redis Cache, μπορείτε να εισαγάγετε αρχεία RDB από οποιονδήποτε διακομιστή Redis λειτουργεί σε οποιοδήποτε cloud ή περιβάλλον, όπως Linux, Windows, ή στο cloud υπηρεσίες παροχής όπως υπηρεσίες Web του Amazon. Για να το κάνετε αυτό, αποστείλετε το αρχείο RDB από τον επιθυμητό διακομιστή Redis σε μια σελίδα blob σε ένα λογαριασμό του χώρου αποθήκευσης Azure και, στη συνέχεια, εισαγάγετέ το στο σας παρουσία Azure Redis Cache premium. Για παράδειγμα, μπορεί να θέλετε να εξαγάγετε τα δεδομένα από το cache παραγωγής και εισαγάγετέ το στο cache χρησιμοποιείται ως τμήμα της περιβάλλον ενδιάμεσου σταδίου για σκοπούς δοκιμής ή μετεγκατάστασης. 

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>Θα μου cache είναι διαθέσιμο στη διάρκεια μιας λειτουργίας εισαγωγής/εξαγωγής;

-   **Εξαγωγή** - μνήμης cache παραμένουν διαθέσιμα και μπορείτε να συνεχίσετε να χρησιμοποιείτε το cache κατά τη διάρκεια μιας λειτουργίας εξαγωγής.
-   **Εισαγωγή** - μνήμης cache δεν είναι πλέον διαθέσιμες κατά την εκκίνηση μιας λειτουργίας εισαγωγής και είναι διαθέσιμες για χρήση όταν ολοκληρωθεί η λειτουργία εισαγωγής.


### <a name="can-i-use-importexport-with-redis-cluster"></a>Μπορώ να χρησιμοποιήσω εισαγωγή/εξαγωγή με σύμπλεγμα Redis;

Ναι, και μπορείτε να εισαγωγή/εξαγωγή ανάμεσα σε μια ομαδοποιημένη cache και ένα μη ομαδοποιημένη cache. Από την έναρξη της Redis σύμπλεγμα [υποστηρίζει μόνο βάσης δεδομένων 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), δεν θα εισαχθούν τα δεδομένα σε βάσεις δεδομένων εκτός από το 0. Όταν εισαχθούν ομαδοποιημένων cache δεδομένα, τα πλήκτρα είναι αναδιανομή μεταξύ του shards του συμπλέγματος. 

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Πώς λειτουργεί η εισαγωγή/εξαγωγή με ένα προσαρμοσμένο βάσεις δεδομένων;

Ορισμένες πληροφορίες για την τιμολόγηση βαθμίδες έχουν διαφορετικά [όρια βάσεις δεδομένων](cache-configure.md#databases), ώστε να υπάρχουν ορισμένα ζητήματα κατά την εισαγωγή εάν έχετε ρυθμίσει μια προσαρμοσμένη τιμή για το `databases` τη ρύθμιση κατά τη δημιουργία του cache.

-   Κατά την εισαγωγή σε μια σειρά τιμολόγησης με ένα κάτω `databases` όριο από το επίπεδο από το οποίο έχει εξαχθεί:
    -   Εάν χρησιμοποιείτε τον προεπιλεγμένο αριθμό `databases` που είναι 16 για όλες τις πληροφορίες τιμολόγησης βαθμίδες, δεν χάνονται δεδομένα.
    -   Εάν χρησιμοποιείτε έναν προσαρμοσμένο αριθμό `databases` που εμπίπτει εντός των ορίων για το επίπεδο στο οποίο πρόκειται να εισαγάγετε, χωρίς δεδομένα θα χαθεί.
    -   Εάν τα δεδομένα σας έχουν εξαχθεί περιείχε δεδομένα σε μια βάση δεδομένων που υπερβαίνει τα όρια της νέας σειράς, τα δεδομένα από αυτές τις υψηλότερες βάσεις δεδομένων δεν εισάγονται.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Σε τι διαφέρει εισαγωγή/εξαγωγή από διατήρηση Redis;

Διατήρηση Azure Redis Cache σάς επιτρέπει να παραμένει δεδομένα αποθηκευμένα σε Redis με το χώρο αποθήκευσης Azure. Κατά τη ρύθμιση παραμέτρων διατήρησης, Azure Redis Cache εξακολουθεί να εμφανίζεται ένα στιγμιότυπο της μνήμης cache Redis σε δυαδική μορφή Redis στο δίσκο με βάση μια συχνότητας δημιουργίας αντιγράφων ασφαλείας με δυνατότητα ρύθμισης παραμέτρων. Εάν καταστροφική που προκύπτει ένα συμβάν που απενεργοποιεί το πρωτεύων και ρεπλίκα cache, τα δεδομένα του cache επανέρχονται αυτόματα, χρησιμοποιώντας το πιο πρόσφατο στιγμιότυπο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να ρυθμίσετε τις παραμέτρους διατήρησης δεδομένων για Premium Azure Redis μνήμης Cache](cache-how-to-premium-persistence.md).

Εισαγωγή / εξαγωγή, μπορείτε να μεταφέρετε δεδομένα σε ή να εξαγάγετε από το Azure Redis Cache. Δεν ρύθμιση παραμέτρων δημιουργίας αντιγράφων ασφαλείας και επαναφορά χρησιμοποιώντας Redis διατήρησης.


### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Μπορώ να αυτοματοποιήσω εισαγωγή/εξαγωγή με χρήση του PowerShell, CLI ή άλλα προγράμματα-πελάτες διαχείρισης;

Ναι, για το PowerShell οδηγίες ανατρέξτε στο θέμα [για να εισαγάγετε ένα cache Redis](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) και [για να εξαγάγετε ένα cache Redis](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).



### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Έλαβα ένα σφάλμα χρονικού ορίου κατά τη διάρκεια της λειτουργίας μου εισαγωγή/εξαγωγή. Τι σημαίνει αυτό;

Εάν παραμένουν στην την **Εισαγωγή δεδομένων** ή να **εξαγάγετε δεδομένα** blade για περισσότερο από 15 λεπτά πριν από την προετοιμασία της λειτουργίας, θα λάβετε ένα σφάλμα παρόμοιο με το ακόλουθο.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Για να επιλύσετε αυτό το θέμα, να ξεκινήσετε την εισαγωγή ή εξαγωγή λειτουργίας πριν από 15 λεπτά έχει παρέλθει.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Έλαβα ένα μήνυμα σφάλματος κατά την εξαγωγή δεδομένων μου για χώρο αποθήκευσης Blob του Azure. Τι έγινε?

Εισαγωγή/εξαγωγή λειτουργεί μόνο με RDB αρχεία που έχουν αποθηκευτεί ως αντικείμενα BLOB σελίδας. Άλλοι τύποι αντικειμένων blob δεν υποστηρίζονται αυτήν τη στιγμή, συμπεριλαμβανομένων των λογαριασμών χώρο αποθήκευσης αντικειμένων blob με ζεστού και εντυπωσιακές βαθμίδες.


## <a name="next-steps"></a>Επόμενα βήματα
Μάθετε πώς να χρησιμοποιείτε περισσότερες δυνατότητες cache premium.

-   [Εισαγωγή στο επίπεδο Azure Redis Cache Premium](cache-premium-tier-intro.md)    

  
<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png








