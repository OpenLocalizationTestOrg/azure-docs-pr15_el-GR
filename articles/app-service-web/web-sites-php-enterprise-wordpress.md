<properties
    pageTitle="WordPress επιχειρηματικής κατηγορίας στην Azure εφαρμογής υπηρεσίας | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να φιλοξενήσετε μια τοποθεσία WordPress επιχειρηματικής κατηγορίας στην Azure εφαρμογής υπηρεσίας"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>WordPress επιχειρηματικής κατηγορίας στην Azure εφαρμογής υπηρεσίας

Azure εφαρμογής υπηρεσίας παρέχει ένα περιβάλλον με ασφαλή και εύκολο για χρήση για αποστολή κρίσιμες, μεγάλη κλίμακα [WordPress] [ wordpress] τοποθεσίες. Microsoft ίδια εκτελείται τοποθεσίες επιχειρηματικής κλάσης όπως το [Office] [ officeblog] και [Bing] [ bingblog] ιστολόγια. Αυτό το έγγραφο που δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure εφαρμογής υπηρεσίας Web Apps για να δημιουργήσετε και να διατηρήσετε μια επιχειρηματικής κατηγορίας, τοποθεσία WordPress βασίζεται στο cloud που μπορεί να χειρίζεται μεγάλο όγκο επισκέπτες.

## <a name="architecture-and-planning"></a>Αρχιτεκτονική και σχεδιασμός

Μια βασική εγκατάσταση WordPress έχει μόνο δύο απαιτήσεις.

* **Βάση δεδομένων MySQL** - μέσω της [ClearDB από το Azure Marketplace][cdbnstore], ή μπορείτε να διαχειριστείτε τις δικές σας εγκατάστασης MySQL σε εικονικές μηχανές Windows Azure χρησιμοποιώντας είτε [Windows] [ mysqlwindows] ή [Linux][mysqllinux].

  > [AZURE.NOTE] ClearDB παρέχει πολλές ρυθμίσεις παραμέτρων MySQL, με διαφορετικό επιδόσεων χαρακτηριστικά για κάθε ρύθμιση παραμέτρων. Δείτε το [Χώρο αποθήκευσης Azure] [ cdbnstore] για πληροφορίες σχετικά με τις προσφορές που παρέχεται μέσω του Azure χώρου αποθήκευσης ή απευθείας, όπως φαίνεται στην [τοποθεσία Web ClearDB](http://www.cleardb.com/pricing.view).

* **PHP 5.2.4 ή μεγαλύτερη** -Azure εφαρμογής υπηρεσίας παρέχουν τη συγκεκριμένη στιγμή [, εκδόσεις PHP 5.4, 5,5 και 5.6][phpwebsite].

    > [AZURE.NOTE] Συνιστάται να εκτελείτε πάντα την πιο πρόσφατη έκδοση της PHP για να βεβαιωθείτε ότι έχετε τις πιο πρόσφατες ενημερώσεις κώδικα ασφαλείας.

### <a name="basic-deployment"></a>Βασική ανάπτυξης

Χρήση μόνο οι βασικές απαιτήσεις, μπορείτε να δημιουργήσετε μια βασική λύση μέσα σε μια περιοχή Azure.

![μια εφαρμογή Azure web και βάση δεδομένων MySQL φιλοξενούνται σε μία μόνο περιοχή Azure][basic-diagram]

Ενώ αυτό θα σας επιτρέψει να κλίμακα ανάληψης την εφαρμογή σας με τη δημιουργία πολλών παρουσιών εφαρμογές Web της τοποθεσίας, όλα τα στοιχεία φιλοξενείται μέσα στα κέντρα δεδομένων σε μια συγκεκριμένη γεωγραφική περιοχή. Οι επισκέπτες από έξω από αυτήν την περιοχή μπορεί να δείτε καθυστέρηση απόκρισης κατά τη χρήση της τοποθεσίας και, εάν μεταβείτε στα κέντρα δεδομένων σε αυτήν την περιοχή προς τα κάτω, επομένως η εφαρμογή σας.


### <a name="multi-region-deployment"></a>Ανάπτυξη πολλών περιοχών

Με χρήση του Azure [Manager κίνηση][trafficmanager], είναι δυνατό να κλιμακωθεί τοποθεσία WordPress σε πολλές γεωγραφικές περιοχές ενώ παρέχει μόνο μία διεύθυνση URL για τους επισκέπτες. Όλοι οι επισκέπτες προέρχονται από μέσω της διαχείρισης κίνηση και, στη συνέχεια, δρομολογούνται σε μια περιοχή με βάση τη ρύθμιση παραμέτρων εξισορρόπησης φόρτου.

![μια εφαρμογή Azure web, φιλοξενούνται σε πολλές περιοχές, χρήση δρομολογητή CDBR υψηλής διαθεσιμότητας για να δρομολογήσετε MySQL σε περιοχές][multi-region-diagram]

Μέσα σε κάθε περιοχή, στην τοποθεσία WordPress θα εξακολουθούν να κλιμακωθεί σε πολλές παρουσίες εφαρμογές Web, αλλά αυτή η κλιμάκωση είναι περιοχή συγκεκριμένες; μεγάλη κίνηση περιοχές μπορεί να έχει διαφορετικά χαμηλής κίνησης από αυτές.

Αναπαραγωγή και δρομολόγηση σε πολλές βάσεις δεδομένων MySQL μπορεί να γίνει με χρήση του ClearDB [CDBR υψηλή διαθεσιμότητα δρομολογητή] [ cleardbscale] (απεικονίζεται αριστερή) ή [MySQL σύμπλεγμα CGE][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Ανάπτυξη πολλών περιοχών με το χώρο αποθήκευσης πολυμέσων και σε cache
Εάν η τοποθεσία αποδέχεται αποστολές ή αρχεία πολυμέσων κεντρικού υπολογιστή, χρησιμοποιήστε χώρο αποθήκευσης αντικειμένων Blob του Azure. Εάν χρειάζεστε σε cache, εξετάστε το ενδεχόμενο να [Redis cache][rediscache], [Memcache Cloud](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)ή ένα από τα άλλα προσφορές σε cache στο [Χώρο αποθήκευσης Azure](http://azure.microsoft.com/gallery/store/).

![μια εφαρμογή Azure web, φιλοξενούνται σε πολλές περιοχές, με χρήση του δρομολογητή CDBR υψηλής διαθεσιμότητας για MySQL, με διαχειριζόμενο Cache, χώρος αποθήκευσης αντικειμένων Blob και CDN][performance-diagram]

Χώρος αποθήκευσης αντικειμένων blob είναι παν-κατανέμεται περιοχές από προεπιλογή, ώστε να μην χρειάζεται να ανησυχείτε για την αναπαραγωγή αρχείων σε όλες τις τοποθεσίες. Μπορείτε επίσης να ενεργοποιήσετε το Azure [Περιεχομένου δικτύου διανομής (CDN)] [ cdn] για το χώρο αποθήκευσης αντικειμένων Blob, που κατανέμει αρχείων για να τερματίσετε κόμβους πιο κοντά στους επισκέπτες σας.

### <a name="planning"></a>Σχεδιασμός

#### <a name="additional-requirements"></a>Πρόσθετες απαιτήσεις

Για να κάνετε το εξής... | Χρησιμοποιήστε αυτήν την επιλογή...
------------------------|-----------
**Αποστολή ή να αποθηκεύσετε μεγάλα αρχεία** | [Προσθήκη WordPress για χρήση του χώρου αποθήκευσης αντικειμένων Blob][storageplugin]
**Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου** | [SendGrid] [ storesendgrid] και της [προσθήκης WordPress για τη χρήση SendGrid][sendgridplugin]
**Προσαρμοσμένα ονόματα τομέα** | [Ρύθμιση των παραμέτρων ενός προσαρμοσμένου ονόματος τομέα στο Azure εφαρμογής υπηρεσίας][customdomain]
**HTTPS** | [Ενεργοποίηση HTTPS για μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας][httpscustomdomain]
**Πριν από την παραγωγή επικύρωσης** | [Ρύθμιση του ενδιάμεσου περιβάλλοντα για τις εφαρμογές web στο Azure εφαρμογής υπηρεσίας][staging] <p>Αλλαγή μιας εφαρμογής web από ενδιάμεσου παραγωγή επίσης μετακινείται στη ρύθμιση παραμέτρων WordPress. Θα πρέπει να βεβαιωθείτε ότι όλες οι ρυθμίσεις έχουν ενημερωθεί με τις απαιτήσεις για την εφαρμογή σας παραγωγής πριν από την αλλαγή της σταδιακής εφαρμογής σε παραγωγής.</p>
**Παρακολούθηση και αντιμετώπιση προβλημάτων** | [Ενεργοποίηση της καταγραφής διαγνωστικών για τις εφαρμογές web στην υπηρεσία εφαρμογής Azure] [ log] και [Εποπτεία εφαρμογών Web στο Azure εφαρμογής υπηρεσίας][monitor]
**Ανάπτυξη της τοποθεσίας σας** | [Ανάπτυξη εφαρμογής web στην υπηρεσία εφαρμογής Azure][deploy]

#### <a name="availability-and-disaster-recovery"></a>Διαθεσιμότητα και την καταστροφή ανάκτησης

Για να κάνετε το εξής... | Χρησιμοποιήστε αυτήν την επιλογή...
------------------------|-----------
**Τοποθεσίες εξισορρόπηση φόρτου** ή **παν-διανομή τοποθεσίες** | [Δρομολόγηση κίνηση με τη Διαχείριση κίνηση Azure][trafficmanager]
**Δημιουργία αντιγράφων ασφαλείας και επαναφοράς** | [Δημιουργία αντιγράφων ασφαλείας μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας] [ backup] και να [επαναφέρετε μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας][restore]

#### <a name="performance"></a>Επιδόσεων

Επιδόσεις στο cloud είναι δυνατό κυρίως μέσω σε cache και κλίμακας ελέγχου; Ωστόσο πρέπει να θεωρείται η μνήμη, εύρους ζώνης και άλλα χαρακτηριστικά Web Apps φιλοξενίας.

Για να κάνετε το εξής... | Χρησιμοποιήστε αυτήν την επιλογή...
------------------------|-----------
**Κατανόηση των δυνατοτήτων παρουσία εφαρμογής υπηρεσίας** | [Πληροφορίες για την τιμολόγηση λεπτομέρειες, συμπεριλαμβανομένων των δυνατοτήτων του επίπεδα εφαρμογής υπηρεσίας][websitepricing]
**Πόροι cache** | [Redis cache][rediscache], [Memcache Cloud](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/)ή ένα από τα άλλα προσφορές σε cache στο [Χώρο αποθήκευσης Azure](/gallery/store/)
**Κλιμάκωση την εφαρμογή σας** | [Κλίμακα μια εφαρμογή web Azure εφαρμογής υπηρεσίας] [ websitescale] και [ClearDB υψηλή διαθεσιμότητα δρομολόγησης][cleardbscale]. Εάν επιλέξετε να φιλοξενήσετε και να διαχειριστείτε τις δικές σας εγκατάστασης MySQL, πρέπει να εξετάσετε [MySQL σύμπλεγμα CGE] [ cge] για ανάληψη κλίμακας

#### <a name="migration"></a>Μετεγκατάσταση

Υπάρχουν δύο μέθοδοι για τη μετεγκατάσταση μια υπάρχουσα τοποθεσία WordPress στο Azure εφαρμογής υπηρεσίας.

* ** [Εξαγωγή WordPress] [ export] ** -αυτό εξάγει το περιεχόμενο του ιστολόγιό σας, το οποίο, στη συνέχεια, μπορεί να εισαχθεί σε μια νέα τοποθεσία WordPress Azure εφαρμογής υπηρεσίας με χρήση της [προσθήκης πρόγραμμα εισαγωγής WordPress][import].

    > [AZURE.NOTE] Ενώ αυτή η διαδικασία σάς επιτρέπει να μετεγκαταστήσετε το περιεχόμενό σας, δεν μετεγκαθιστά τυχόν προσθήκες, θέματα ή άλλες προσαρμογές. Αυτά πρέπει να εγκαταστήσετε ξανά με μη αυτόματο τρόπο.

* **Μη αυτόματη μετεγκατάσταση** - [Δημιουργία αντιγράφου ασφαλείας της τοποθεσίας σας] [ wordpressbackup] και [βάσεων δεδομένων][wordpressdbbackup], στη συνέχεια, με μη αυτόματο τρόπο τον επαναφέρετε μια εφαρμογή web με Azure εφαρμογής υπηρεσίας και σχετικές βάση δεδομένων MySQL για μετεγκατάσταση ιδιαίτερα προσαρμοσμένες τοποθεσίες και να αποφύγετε την tedium με μη αυτόματο τρόπο την εγκατάσταση προσθηκών, θέματα και άλλες προσαρμογές.

## <a name="step-by-step-instructions"></a>Οδηγίες βήμα προς βήμα

### <a name="create-a-wordpress-site"></a>Δημιουργία τοποθεσίας WordPress

1. Χρησιμοποιήστε το [Azure Marketplace] [ cdbnstore] για να δημιουργήσετε μια βάση δεδομένων MySQL το μέγεθος που προσδιορίσατε στην ενότητα [αρχιτεκτονική και προγραμματισμού](#planning) , στο το περιφερειών ότι θα μπορείτε να φιλοξενήσετε την τοποθεσία σας.

2. Ακολουθήστε τα βήματα στο θέμα [Δημιουργία μια εφαρμογή web WordPress στο Azure εφαρμογής υπηρεσίας] [ createwordpress] για να δημιουργήσετε μια εφαρμογή web WordPress. Κατά τη δημιουργία της εφαρμογής web, επιλέξτε **Χρησιμοποιήστε μια υπάρχουσα βάση δεδομένων MySQL** και επιλέξτε τη βάση δεδομένων που δημιουργήσατε στο βήμα 1.

Εάν κάνετε μετεγκατάσταση μια υπάρχουσα τοποθεσία WordPress, ανατρέξτε στο θέμα [μετεγκατάσταση μια υπάρχουσα τοποθεσία WordPress να Azure](#Migrate-an-existing-WordPress-site-to-Azure) μετά τη δημιουργία νέας εφαρμογής web.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Μετεγκατάσταση μια υπάρχουσα τοποθεσία WordPress στο Azure

Όπως αναφέρθηκε στην ενότητα [αρχιτεκτονική και προγραμματισμό](#planning) , υπάρχουν δύο τρόποι για τη μετεγκατάσταση μιας τοποθεσίας WordPress.

* **Εξαγωγή και εισαγωγή** - για τοποθεσίες χωρίς όγκο προσαρμογής, ή όπου θέλετε απλώς να μετακινήσετε το περιεχόμενο.

* **Δημιουργία αντιγράφων ασφαλείας και επαναφοράς** - για τις τοποθεσίες με πολλά προσαρμογής όπου θέλετε να μετακινήσετε όλα τα στοιχεία.

Χρησιμοποιήστε μία από τις ακόλουθες ενότητες για να μετεγκαταστήσετε την τοποθεσία σας.

#### <a name="the-export-and-import-method"></a>Η μέθοδος εισαγωγή και εξαγωγή

1. Χρήση [Εξαγωγή WordPress] [ export] για να εξαγάγετε υπάρχουσα τοποθεσία σας.

2. Δημιουργία εφαρμογής web χρησιμοποιώντας τα βήματα που περιγράφονται στην ενότητα [Δημιουργία τοποθεσίας WordPress](#Create-a-new-WordPress-site) .

3. Συνδεθείτε στην τοποθεσία σας WordPress σε εφαρμογές Web και κάντε κλικ στην επιλογή **προσθήκες** -> **Add New**. Αναζητήστε και εγκαταστήστε, την προσθήκη **WordPress πρόγραμμα εισαγωγής** .

4. Μετά την εγκατάσταση της προσθήκης πρόγραμμα εισαγωγής, κάντε κλικ στην επιλογή **Εργαλεία** -> **Εισαγωγή**και, στη συνέχεια, επιλέξτε **WordPress** για να χρησιμοποιήσετε το πρόσθετο πρόγραμμα εισαγωγής WordPress.

5. Στη σελίδα **WordPress εισαγωγής** , κάντε κλικ στην επιλογή " **Επιλογή αρχείου**". Αναζητήστε το αρχείο WXR που έχει εξαχθεί από υπάρχουσα τοποθεσία WordPress και, στη συνέχεια, επιλέξτε **Αποστολή αρχείου και να εισαγάγετε**.

6. Κάντε κλικ στην επιλογή **Υποβολή**. Θα σας ζητηθεί ότι η εισαγωγή ολοκληρώθηκε με επιτυχία.

8. Αφού ολοκληρώσετε όλα αυτά τα βήματα, επανεκκινήστε στην τοποθεσία σας από το blade εφαρμογής web στην [πύλη του Azure][mgmtportal].

Μετά την εισαγωγή της τοποθεσίας, ίσως χρειαστεί να εκτελέσετε τα παρακάτω βήματα για να ενεργοποιήσετε τις ρυθμίσεις δεν που περιέχονται στο αρχείο εισαγωγής.

Εάν χρησιμοποιούσατε αυτό... | Κάντε το εξής...
------------------ | ----------
**Μόνιμων συνδέσεων** | Από τον πίνακα εργαλείων WordPress της νέας τοποθεσίας, κάντε κλικ στην επιλογή **Ρυθμίσεις** -> **μόνιμων** και, στη συνέχεια, η ενημερωμένη έκδοση της δομής μόνιμων συνδέσεων
**εικόνα/πολυμέσων συνδέσεις** | Για να ενημερώσετε τις συνδέσεις με τη νέα θέση, χρησιμοποιήστε την [Προσθήκη URL ενημέρωση μπλε Velvet][velvet], ένα εργαλείο αναζήτησης και αντικατάστασης, ή με μη αυτόματο τρόπο στη βάση δεδομένων σας
**Θέματα** | Μεταβείτε στις επιλογές **Εμφάνιση** -> **θέμα** και ενημερωμένη έκδοση του θέματος τοποθεσίας, όπως απαιτείται
**Μενού** | Εάν το θέμα σας υποστηρίζει μενού, συνδέσεις στην αρχική σελίδα σας ίσως χρειαστεί το παλιό δευτερεύουσες καταλόγου ενσωματωμένο σε αυτά. Μεταβείτε στις επιλογές **Εμφάνιση** -> **μενού** και να ενημερώσετε τους

#### <a name="the-backup-and-restore-method"></a>Η μέθοδος δημιουργίας αντιγράφων ασφαλείας και επαναφοράς

1. Δημιουργία αντιγράφων ασφαλείας του υπάρχοντος WordPress τοποθεσίας χρησιμοποιώντας τις πληροφορίες από [αντίγραφα ασφαλείας WordPress][wordpressbackup].

2. Δημιουργία αντιγράφων ασφαλείας την υπάρχουσα βάση δεδομένων, χρησιμοποιώντας τις πληροφορίες κατά [τη δημιουργία αντιγράφων ασφαλείας της βάσης δεδομένων][wordpressdbbackup].

3. Δημιουργία βάσης δεδομένων και να επαναφέρετε το αντίγραφο ασφαλείας.

    1. Αγοράστε μια νέα βάση δεδομένων από το [Azure Marketplace][cdbnstore], ή μια βάση δεδομένων MySQL σε [Windows] εγκατάστασης[ mysqlwindows] ή [Linux] [ mysqllinux] Εικονική.

    2. Χρησιμοποιώντας ένα πρόγραμμα-πελάτη MySQL όπως [Διάθεσή σας MySQL][workbench], συνδεθείτε με τη νέα βάση δεδομένων και να εισαγάγετε τη βάση δεδομένων WordPress.

    3. Ενημέρωση της βάσης δεδομένων για να αλλάξετε τις καταχωρήσεις τομέα για τον νέο σας τομέα Azure εφαρμογής υπηρεσίας. Για παράδειγμα, mywordpress.azurewebsites.net. Χρησιμοποιήστε την [Αναζήτηση και αντικατάσταση για δέσμη ενεργειών βάσεις δεδομένων WordPress] [ searchandreplace] για να αλλάξετε με ασφάλεια όλων των εμφανίσεων.

4. Δημιουργία εφαρμογής web στην πύλη του Azure και δημοσιεύστε το αντίγραφο ασφαλείας WordPress.

    1. Δημιουργία εφαρμογής web στην [πύλη του Azure] [ mgmtportal] με μια βάση δεδομένων χρησιμοποιώντας το **νέο** -> **Web + Mobile** -> **Azure Marketplace** -> **Web Apps** -> **εφαρμογής Web + SQL** (ή **Web app + MySQL**) -> **Δημιουργία**. Ρύθμιση παραμέτρων όλες τις απαιτούμενες ρυθμίσεις για να δημιουργήσετε μια εφαρμογή web κενό.

    2. Στο αντίγραφο ασφαλείας WordPress, εντοπίστε το αρχείο **Τμήματος Web config.php** και ανοίξτε το σε ένα πρόγραμμα επεξεργασίας. Αντικαταστήστε τις ακόλουθες καταχωρήσεις με τις πληροφορίες για τη νέα βάση δεδομένων MySQL.

        * **DB_NAME** - το όνομα χρήστη της βάσης δεδομένων

        * **DB_USER** - το όνομα χρήστη που χρησιμοποιείται για την πρόσβαση στη βάση δεδομένων

        * **DB_PASSWORD** - τον κωδικό πρόσβασης χρήστη

        Μετά την αλλαγή αυτές τις καταχωρήσεις, αποθηκεύστε και κλείστε το αρχείο **config.php Τμήματος Web** .

    3. Χρησιμοποιήστε την [ανάπτυξη μιας εφαρμογής web στο Azure εφαρμογής υπηρεσίας] [ deploy] πληροφορίες για να ενεργοποιήσετε τη μέθοδο ανάπτυξης που θέλετε να χρησιμοποιήσετε και, στη συνέχεια, αναπτύξτε το αντίγραφο ασφαλείας WordPress σε εφαρμογή web στο Azure εφαρμογής υπηρεσίας.

5. Όταν έχει αναπτυχθεί στην τοποθεσία WordPress, θα πρέπει να μπορούν να έχουν πρόσβαση στο νέο τοποθεσίας (όπως μια εφαρμογή web της εφαρμογής υπηρεσίας) χρησιμοποιώντας το *. azurewebsite.net διεύθυνση URL της τοποθεσίας.

### <a name="configure-your-site"></a>Ρύθμιση παραμέτρων της τοποθεσίας σας

Αφού έχει δημιουργηθεί ή μετεγκατασταθεί στην τοποθεσία WordPress, χρησιμοποιήστε τις ακόλουθες πληροφορίες για να βελτιώσετε την απόδοση ή να ενεργοποιήσετε την πρόσθετη λειτουργικότητα.

Για να κάνετε το εξής... | Χρησιμοποιήστε αυτήν την επιλογή...
------------- | -----------
**Ορισμός λειτουργίας πρόγραμμα εφαρμογής υπηρεσίας, το μέγεθος και ενεργοποίηση κλιμάκωση** | [Κλίμακα μια εφαρμογή web Azure εφαρμογής υπηρεσίας][websitescale]
**Ενεργοποίηση των συνδέσεων persistent βάσης δεδομένων** | Από προεπιλογή, WordPress δεν χρησιμοποιεί συνδέσεις μόνιμη βάσης δεδομένων, οι οποίες μπορεί να προκαλέσει τη σύνδεση με τη βάση δεδομένων για να γίνετε επιβραδύνει μετά από πολλές συνδέσεις. Για να ενεργοποιήσετε συνδέσεων persistent, εγκαταστήστε την (προσθήκη προσαρμογέα συνδέσεων persistent) [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]. 
**Βελτίωση της απόδοσης** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">Απενεργοποίηση του cookie ΈΧΟΥΝ</a> - να βελτιώσετε τις επιδόσεις όταν εκτελείται WordPress σε πολλές παρουσίες Web Apps</p></li><li><p>Ενεργοποίηση του cache. <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis cache</a> (έκδοση preview) μπορεί να χρησιμοποιηθεί με το <a href="https://wordpress.org/plugins/redis-object-cache/">Redis προσθήκης WordPress cache αντικειμένων</a>ή να χρησιμοποιήσετε ένα από τα άλλα σε cache προσφορές από το <a href="/gallery/store/">Χώρο αποθήκευσης Azure</a></p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">Πώς να κάνετε πιο γρήγορα WordPress με Wincache</a> - Wincache είναι ενεργοποιημένη από προεπιλογή για τις εφαρμογές Web</p></li><li><p><a href="../web-sites-scale/">Κλίμακα μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας</a> και χρησιμοποιήστε <a href="http://www.cleardb.com/developers/cdbr/introduction">Τη δρομολόγηση ClearDB υψηλή διαθεσιμότητα</a> ή <a href="http://www.mysql.com/products/cluster/">MySQL CGE συμπλέγματος</a></p></li></ul>
**Χρήση αντικειμένων blob για χώρο αποθήκευσης** | <ol><li><p><a href="../storage-create-storage-account/">Δημιουργήστε ένα λογαριασμό αποθήκευσης Azure</a></p></li><li><p>Μάθετε πώς μπορείτε να <a href="../cdn-how-to-use/">Χρησιμοποιήστε το δίκτυο περιεχομένου διανομής (CDN)</a> για να παν-διανομή δεδομένα αποθηκευμένα σε αντικείμενα blob.</p></li><li><p>Εγκατάσταση και ρύθμιση παραμέτρων της <a href="https://wordpress.org/plugins/windows-azure-storage/">Αποθήκευσης Azure για προσθήκη WordPress</a>.</p><p>Για λεπτομερή ρύθμιση και πληροφορίες ρύθμισης παραμέτρων για την προσθήκη, ανατρέξτε στο θέμα <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">Οδηγός</a>.</p> </li></ol>
**Ενεργοποίηση ηλεκτρονικού ταχυδρομείου** | Ενεργοποίηση <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> χρήση του χώρου αποθήκευσης Azure. Εγκατάσταση της <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">προσθήκης SendGrid</a> για WordPress.
**Ρύθμιση παραμέτρων ενός προσαρμοσμένου ονόματος τομέα** | [Ρύθμιση των παραμέτρων ενός προσαρμοσμένου ονόματος τομέα στο Azure εφαρμογής υπηρεσίας][customdomain]
**Ενεργοποίηση HTTPS για ένα προσαρμοσμένο όνομα τομέα** | [Ενεργοποίηση HTTPS για μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας][httpscustomdomain]
**Φόρτωση Υπόλοιπο "ή" παν-διανομή την τοποθεσία σας** | [Δρομολογείτε την κίνηση με τη Διαχείριση κίνηση Azure][trafficmanager]. Εάν χρησιμοποιείτε έναν προσαρμοσμένο τομέα, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ενός προσαρμοσμένου ονόματος τομέα στο Azure εφαρμογής υπηρεσίας] [ customdomain] για πληροφορίες σχετικά με την κυκλοφορία Manager με προσαρμοσμένα ονόματα τομέα
**Ενεργοποίηση αυτόματης δημιουργίας αντιγράφων ασφαλείας** | [Δημιουργία αντιγράφου ασφαλείας μια εφαρμογή web στο Azure εφαρμογής υπηρεσίας][backup]
**Ενεργοποιήσετε την καταγραφή διαγνωστικών** | [Ενεργοποίηση της καταγραφής διαγνωστικών για τις εφαρμογές web στο Azure εφαρμογής υπηρεσίας][log]

## <a name="next-steps"></a>Επόμενα βήματα

* [Βελτιστοποίηση WordPress](http://codex.wordpress.org/WordPress_Optimization)

* [Μετατροπή WordPress Multisite στο Azure εφαρμογής υπηρεσίας](web-sites-php-convert-wordpress-multisite.md)

* [ClearDB αναβάθμιση του οδηγού για Azure](http://www.cleardb.com/store/azure/upgrade)

* [Φιλοξενίας WordPress σε έναν υποφάκελο της εφαρμογής web στην υπηρεσία εφαρμογής Azure](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Βήμα προς βήμα: Δημιουργία τοποθεσίας WordPress χρησιμοποιώντας Azure](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [Φιλοξενείτε το ιστολόγιό σας WordPress υπάρχουσες στο Azure](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [Ενεργοποίηση Όμορφος μόνιμων στο WordPress](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [Πώς μπορείτε να μετεγκαταστήσετε και να εκτελέσετε το ιστολόγιο WordPress σε Azure εφαρμογής υπηρεσίας](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [Πώς μπορείτε να εκτελέσετε δωρεάν WordPress σε Azure εφαρμογής υπηρεσίας](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [WordPress σε Azure σε 2 λεπτά ή λιγότερο](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [Μετακίνηση ενός ιστολογίου WordPress στο Azure - μέρος 1: δημιουργία ιστολογίου WordPress σε Azure](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [Μετακίνηση ενός ιστολογίου WordPress στο Azure - μέρος 2: μεταφορά του περιεχομένου](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [Μετακίνηση ενός ιστολογίου WordPress στο Azure - μέρος 3: ρύθμιση του προσαρμοσμένου τομέα σας](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [Μετακίνηση ενός ιστολογίου WordPress στο Azure - μέρος 4: Όμορφος μόνιμων και κανόνες επανεγγραφή διεύθυνσης URL](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [Μετακίνηση ενός ιστολογίου WordPress στο Azure - τμήμα 5: μετακίνηση από έναν υποφάκελο στον ριζικό κατάλογο](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Πώς μπορείτε να ρυθμίσετε μια εφαρμογή web WordPress στο λογαριασμό σας στο Azure](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [Propping του WordPress στο Azure](http://www.johnpapa.net/wordpress-on-azure/)

* [Συμβουλές για WordPress σε Azure](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν από την εγγραφή για λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](http://go.microsoft.com/fwlink/?LinkId=523751), όπου μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή web μικρής διάρκειας starter στην εφαρμογή υπηρεσίας. Δεν υπάρχει πιστωτικές κάρτες υποχρεωτικό, χωρίς δεσμεύσεις.

## <a name="whats-changed"></a>Τι έχει αλλάξει
* Για οδηγίες για την αλλαγή από τοποθεσίες Web App υπηρεσία ανατρέξτε στο θέμα: [Azure εφαρμογής υπηρεσίας και τον αντίκτυπο σχετικά με τις υπάρχουσες υπηρεσίες Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
