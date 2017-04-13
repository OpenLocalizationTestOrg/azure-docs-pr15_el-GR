<properties 
    pageTitle="Τρόπος ρύθμισης παραμέτρων του Azure Redis Cache | Microsoft Azure"
    description="Κατανόηση η προεπιλεγμένη Redis ρύθμιση παραμέτρων Azure Redis Cache και μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους του Azure Redis Cache παρουσίες"
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
    ms.date="08/25/2016"
    ms.author="sdanie" />

# <a name="how-to-configure-azure-redis-cache"></a>Τρόπος ρύθμισης παραμέτρων του Azure Redis Cache

Αυτό το θέμα περιγράφει πώς μπορείτε να ελέγξετε και να ενημερώσετε τη ρύθμιση παραμέτρων για τις παρουσίες Azure Redis Cache και καλύπτει την προεπιλεγμένη ρύθμιση παραμέτρων διακομιστή Redis για παρουσίες Azure Redis Cache.

>[AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση παραμέτρων και χρήση κορυφαίες δυνατότητες του cache, ανατρέξτε στο θέμα [πώς μπορείτε να ρυθμίσετε τις παραμέτρους διατήρησης για Premium Azure Redis μνήμης Cache](cache-how-to-premium-persistence.md), [τον τρόπο ρύθμισης παραμέτρων του συμπλέγματος για Premium Azure Redis μνήμης Cache](cache-how-to-premium-clustering.md)και [πώς μπορείτε να ρυθμίσετε τις παραμέτρους εικονικού δικτύου υποστήριξης για Premium Azure Redis μνήμης Cache](cache-how-to-premium-vnet.md).

## <a name="configure-redis-cache-settings"></a>Ρύθμιση παραμέτρων cache Redis

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis Cache παρέχει τις ακόλουθες ρυθμίσεις σχετικά με το blade **Ρυθμίσεις** .

![Redis ρυθμίσεις του Cache](./media/cache-configure/redis-cache-settings.png)



-   [Υποστήριξη & Ρυθμίσεις αντιμετώπισης προβλημάτων](#support-amp-troubleshooting-settings)
-   [Γενικές ρυθμίσεις](#general-settings)
    -   [Ιδιότητες](#properties)
    -   [Πλήκτρα πρόσβασης](#access-keys)
    -   [Ρυθμίσεις για προχωρημένους](#advanced-settings)
    -   [Redis Σύμβουλος Cache](#redis-cache-advisor)
-   [Ρυθμίσεις κλίμακας](#scale-settings)
    -   [Πληροφορίες για την τιμολόγηση επιπέδων](#pricing-tier)
    -   [Redis μέγεθος συμπλέγματος](#cluster-size)
-   [Ρυθμίσεις διαχείρισης δεδομένων](#data-management-settings)
    -   [Redis διατήρησης δεδομένων](#redis-data-persistence)
    -   [Εισαγωγή/εξαγωγή](#importexport)
-   [Ρυθμίσεις διαχείρισης](#administration-settings)
    -   [Επανεκκινήστε τον υπολογιστή](#reboot)
    -   [Προγραμματισμός ενημερώσεων](#schedule-updates)
-   [Των ρυθμίσεων των διαγνωστικών](#diagnostics-settings)
-   [Ρυθμίσεις δικτύου](#network-settings)
-   [Ρυθμίσεις διαχείρισης πόρων](#resource-management-settings)

## <a name="support--troubleshooting-settings"></a>Υποστήριξη & Ρυθμίσεις αντιμετώπισης προβλημάτων

Οι ρυθμίσεις στην ενότητα **υποστήριξης + αντιμετώπισης προβλημάτων** σας παρέχει επιλογές για την επίλυση προβλημάτων με το cache.

![Υποστήριξη + αντιμετώπισης προβλημάτων](./media/cache-configure/redis-cache-support-troubleshooting.png)

Κάντε κλικ στην επιλογή **διάγνωση και επίλυση προβλημάτων** που παρέχονται με συνήθη ζητήματα και στρατηγικές για την επίλυση τους.

Κάντε κλικ στο **αρχείο καταγραφής της δραστηριότητας** για να προβάλετε τις ενέργειες που εκτελούνται στην το cache. Μπορείτε επίσης να χρησιμοποιήσετε το φιλτράρισμα για να αναπτύξετε αυτήν την προβολή για να συμπεριλάβετε άλλους πόρους. Για περισσότερες πληροφορίες σχετικά με την εργασία με αρχεία καταγραφής ελέγχου, ανατρέξτε στο θέμα [Προβολή συμβάντων και αρχεία καταγραφής ελέγχου](../monitoring-and-diagnostics/insights-debugging-with-events.md) και [ελέγχου λειτουργιών με τη διαχείριση πόρων](../resource-group-audit.md). Για περισσότερες πληροφορίες σχετικά με την παρακολούθηση συμβάντων Azure Redis Cache, ανατρέξτε στο θέμα [Λειτουργίες και ειδοποιήσεις](cache-how-to-monitor.md#operations-and-alerts).

**Εύρυθμη λειτουργία των πόρων** σας παρακολουθεί τον πόρο και σάς ενημερώνει εάν λειτουργεί όπως αναμένεται. Για περισσότερες πληροφορίες σχετικά με την υπηρεσία υγείας Azure πόρων, ανατρέξτε στο θέμα [Επισκόπηση εύρυθμης λειτουργίας Azure πόρων](../resource-health/resource-health-overview.md).

>[AZURE.NOTE] Εύρυθμη λειτουργία των πόρων είναι αυτήν τη στιγμή δεν είναι δυνατή η δημιουργία αναφοράς για την εύρυθμη λειτουργία των παρουσιών Azure Redis Cache σε ένα εικονικό δίκτυο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [όλες τις δυνατότητες του cache λειτουργούν όταν φιλοξενίας ένα cache σε μια VNET;](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

Κάντε κλικ στην επιλογή **Δημιουργία υποστηρίζει την αίτηση** για να ανοίξετε μια αίτηση υποστήριξης για το cache.

## <a name="general-settings"></a>Γενικές ρυθμίσεις

Οι ρυθμίσεις στην ενότητα " **Γενικά** " σας επιτρέπουν να έχετε πρόσβαση και να ρυθμίσετε τις εξής παραμέτρους για το cache.

![Γενικές ρυθμίσεις](./media/cache-configure/redis-cache-general-settings.png)

-   [Ιδιότητες](#properties)
-   [Πλήκτρα πρόσβασης](#access-keys)
-   [Ρυθμίσεις για προχωρημένους](#advanced-settings)
-   [Redis Σύμβουλος Cache](#redis-cache-advisor)

### <a name="properties"></a>Ιδιότητες

Κάντε κλικ στην εντολή **Ιδιότητες** , για να προβάλετε πληροφορίες σχετικά με το cache, συμπεριλαμβάνοντας το τελικό σημείο cache και τις θύρες.

![Redis Cache ιδιοτήτων](./media/cache-configure/redis-cache-properties.png)

### <a name="access-keys"></a>Πλήκτρα πρόσβασης

Κάντε κλικ στην επιλογή **πλήκτρα πρόσβασης** για να προβάλετε ή να δημιουργήσετε ξανά το συνδυασμό πλήκτρων πρόσβασης για το cache. Τα πλήκτρα που χρησιμοποιούνται μαζί με το όνομα κεντρικού υπολογιστή και τις θύρες από το blade **Ιδιότητες** από τα προγράμματα-πελάτες τη σύνδεση με το cache.

![Redis Cache πλήκτρων πρόσβασης](./media/cache-configure/redis-cache-manage-keys.png)






### <a name="advanced-settings"></a>Ρυθμίσεις για προχωρημένους

Οι ακόλουθες ρυθμίσεις είναι ρυθμίσετε τις παραμέτρους του blade **ρυθμίσεις για προχωρημένους** .

-   [Θύρες πρόσβασης](#access-ports)
-   [Πολιτική Maxmemory και δεσμευμένες maxmemory](#maxmemory-policy-and-maxmemory-reserved)
-   [Ειδοποιήσεις Keyspace (ρυθμίσεις για προχωρημένους)](#keyspace-notifications-advanced-settings)


### <a name="access-ports"></a>Θύρες πρόσβασης

Από προεπιλογή, χωρίς SSL πρόσβασης είναι απενεργοποιημένη για νέα μνήμης cache. Για να ενεργοποιήσετε τη θύρα χωρίς SSL, κάντε κλικ στην επιλογή " **όχι** " για **να επιτρέπεται η πρόσβαση μόνο μέσω SSL** στην το **blade τις ρυθμίσεις για προχωρημένους** και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

![Θύρες πρόσβασης redis Cache](./media/cache-configure/redis-cache-access-ports.png)

### <a name="maxmemory-policy-and-maxmemory-reserved"></a>Πολιτική Maxmemory και δεσμευμένες maxmemory

Οι ρυθμίσεις **πολιτικής Maxmemory** και **δεσμευμένες maxmemory** στον το blade **ρυθμίσεις για προχωρημένους** , ρυθμίστε τις παραμέτρους των πολιτικών μνήμης για το cache. Η ρύθμιση **πολιτικής maxmemory** ρυθμίζει τις παραμέτρους της πολιτικής eviction για το cache και **δεσμευμένες maxmemory** ρυθμίζει τις παραμέτρους της μνήμης δεσμευμένες για διαδικασίες-cache.

![Redis πολιτική Maxmemory Cache](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Πολιτική Maxmemory** σάς επιτρέπει να επιλέξετε από τις παρακάτω πολιτικές eviction.

-   πτητικές-lru - αυτή είναι η προεπιλεγμένη.
-   allkeys lru
-   πτητικές τυχαία
-   allkeys τυχαία
-   ttl πτητικής
-   noeviction

Για περισσότερες πληροφορίες σχετικά με τις πολιτικές maxmemory, ανατρέξτε στο θέμα [Eviction πολιτικές](http://redis.io/topics/lru-cache#eviction-policies).

Η ρύθμιση **δεσμευμένες maxmemory** ρυθμίζει το μέγεθος της μνήμης σε MB που είναι δεσμευμένο για λειτουργίες-cache όπως αναπαραγωγής κατά την ανακατεύθυνση. Μπορεί επίσης να χρησιμοποιηθεί όταν έχετε μια αναλογία υψηλής κατακερματισμός. Ρύθμιση αυτής της τιμής σάς επιτρέπει να έχετε Redis διακομιστή συνεπείς όταν το φορτίο μεταβάλλεται. Αυτή η τιμή πρέπει να οριστεί νεότερη έκδοση για φόρτους εργασίας που είναι γράψετε χοντρό. Όταν μνήμης είναι δεσμευμένο για τέτοιες δραστηριότητες είναι διαθέσιμες για την αποθήκευση των δεδομένων στο cache.

>[AZURE.IMPORTANT] Η ρύθμιση **δεσμευμένες maxmemory** είναι διαθέσιμη μόνο για τυπική και τα αποθηκεύει Premium.

### <a name="keyspace-notifications-advanced-settings"></a>Ειδοποιήσεις Keyspace (ρυθμίσεις για προχωρημένους)

Redis keyspace ειδοποιήσεις έχουν ρυθμιστεί οι παράμετροι σε το blade **ρυθμίσεις για προχωρημένους** . Ειδοποιήσεις Keyspace να επιτρέπεται σε προγράμματα-πελάτες για να λαμβάνετε ειδοποιήσεις όταν παρουσιάζονται συγκεκριμένα συμβάντα.

![Redis Cache ρυθμίσεις για προχωρημένους](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Ειδοποιήσεις Keyspace και τη ρύθμιση **ειδοποιήστε-keyspace-συμβάντα** είναι διαθέσιμα μόνο για τυπική και τα αποθηκεύει Premium.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Redis Keyspace ειδοποιήσεις](http://redis.io/topics/notifications). Για το δείγμα κώδικα, ανατρέξτε στο αρχείο [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) στο δείγμα [Χαίρετε](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) .

<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis Σύμβουλος Cache

Το blade **συστάσεις** εμφανίζει συστάσεις για το cache. Κατά την κανονική λειτουργία, εμφανίζονται χωρίς συστάσεις. 

![Συστάσεις](./media/cache-configure/redis-cache-no-recommendations.png)

Εάν οποιαδήποτε συνθήκες προκύπτουν κατά τη διάρκεια της εκτέλεσης λειτουργιών το cache όπως μεγάλη χρήση μνήμης, το εύρος ζώνης δικτύου ή φόρτωση διακομιστή, εμφανίζεται μια ειδοποίηση στον το blade **Redis Cache** .

![Συστάσεις](./media/cache-configure/redis-cache-recommendations-alert.png)

Περαιτέρω πληροφορίες μπορεί να βρεθεί στην το blade **συστάσεις** .

![Συστάσεις](./media/cache-configure/redis-cache-recommendations.png)

Μπορείτε να παρακολουθείτε αυτές τις μετρήσεις στις ενότητες [γραφημάτων παρακολούθησης](cache-how-to-monitor.md#monitoring-charts) και [Χρήση γραφημάτων](cache-how-to-monitor.md#usage-charts) από το **Redis Cache** blade.

Κάθε επίπεδο τιμολόγησης έχει διαφορετικό όρια για συνδέσεις υπολογιστή-πελάτη, μνήμη και το εύρος ζώνης. Εάν το cache πλησιάζει μέγιστη χωρητικότητα για αυτές τις μετρήσεις σε μια σταθερή χρονική περίοδο, δημιουργείται σύσταση. Για περισσότερες πληροφορίες σχετικά με τις μετρήσεις και τα όρια για αναθεώρηση από το εργαλείο **συστάσεις** , ανατρέξτε στον παρακάτω πίνακα.

| Redis μετρικό Cache      | Για περισσότερες πληροφορίες, ανατρέξτε                                                  |
|-------------------------|---------------------------------------------------------------------------|
| Χρήση του εύρους ζώνης δικτύου | [Απόδοση cache - διαθέσιμο εύρος ζώνης](cache-faq.md#cache-performance) |
| Προγράμματα-πελάτες του συνδεδεμένου       | [Προεπιλεγμένη ρύθμιση παραμέτρων διακομιστή Redis - maxclients](#maxclients)            |
| Φόρτωση διακομιστή             | [Χρήση γραφημάτων - Redis φόρτωσης διακομιστή](cache-how-to-monitor.md#usage-charts)  |
| Χρήση μνήμης            | [Απόδοση cache - μέγεθος](cache-faq.md#cache-performance)                |

Για να αναβαθμίσετε το cache, κάντε κλικ στην επιλογή **αναβάθμιση τώρα** για να αλλάξετε την [Τιμολόγηση επίπεδο](#pricing-tier) και κλιμάκωση το cache. Για περισσότερες πληροφορίες σχετικά με την επιλογή τιμολόγησης επίπεδο, ανατρέξτε στο θέμα [Τι Redis Cache προσφοράς και το μέγεθος πρέπει να χρησιμοποιήσω;](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-settings"></a>Ρυθμίσεις κλίμακας

Οι ρυθμίσεις στην ενότητα **κλίμακα** σάς επιτρέπουν να έχετε πρόσβαση και να ρυθμίσετε τις εξής παραμέτρους για το cache.

![Δικτύου](./media/cache-configure/redis-cache-scale.png)

-   [Πληροφορίες για την τιμολόγηση επιπέδων](#pricing-tier)
-   [Redis μέγεθος συμπλέγματος](#cluster-size)

### <a name="pricing-tier"></a>Πληροφορίες για την τιμολόγηση επιπέδων

Κάντε κλικ στο **επίπεδο Τιμολόγηση** για να προβάλετε ή να αλλάξετε τη σειρά τιμολόγησης για το cache. Για περισσότερες πληροφορίες σχετικά με την κλιμάκωση, δείτε [πώς μπορείτε να Cache Redis Azure κλίμακα](cache-how-to-scale.md).

![Redis Cache τις τιμές επιπέδων](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### <a name="redis-cluster-size"></a>Redis μέγεθος συμπλέγματος

Κάντε κλικ στην επιλογή **Μέγεθος συμπλέγματος Redis (έκδοση PREVIEW)** για να αλλάξετε το μέγεθος του συμπλέγματος για το τρέχον cache premium με σύμπλεγμα με δυνατότητα.

>[AZURE.NOTE] Σημειώστε ότι ενώ η σειρά Azure Redis Cache Premium έχει εκδοθεί γενικής διαθεσιμότητας, η δυνατότητα Redis μέγεθος συμπλέγματος είναι αυτήν τη στιγμή στην προεπισκόπηση.

![Redis μέγεθος συμπλέγματος](./media/cache-configure/redis-cache-redis-cluster-size.png)

Για να αλλάξετε το μέγεθος του συμπλέγματος, χρησιμοποιήστε το ρυθμιστικό ή πληκτρολογήστε έναν αριθμό μεταξύ 1 και 10 στο πλαίσιο κειμένου **καταμέτρησης Shard** και κάντε κλικ στο **κουμπί OK** για να αποθηκεύσετε.

>[AZURE.IMPORTANT] Σύμπλεγμα redis είναι διαθέσιμη μόνο για Premium μνήμης cache. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος ρύθμισης των παραμέτρων του συμπλέγματος για Premium Azure Redis μνήμης Cache](cache-how-to-premium-clustering.md).


## <a name="data-management-settings"></a>Ρυθμίσεις διαχείρισης δεδομένων

Οι ρυθμίσεις στην ενότητα **Διαχείριση δεδομένων** σάς επιτρέπουν να έχετε πρόσβαση και να ρυθμίσετε τις εξής παραμέτρους για το cache.

![Διαχείριση δεδομένων](./media/cache-configure/redis-cache-data-management.png)

-   [Redis διατήρησης δεδομένων](#redis-data-persistence)
-   [Εισαγωγή/εξαγωγή](#importexport)

### <a name="redis-data-persistence"></a>Redis διατήρησης δεδομένων

Κάντε κλικ στην επιλογή **Redis διατήρησης δεδομένων** για ενεργοποίηση, απενεργοποίηση ή ρύθμιση παραμέτρων διατήρησης δεδομένων για το cache premium.

![Redis διατήρησης δεδομένων](./media/cache-configure/redis-cache-persistence-settings.png)

Για να ενεργοποιήσετε Redis διατήρησης, κάντε κλικ στην επιλογή **με δυνατότητα** για να ενεργοποιήσετε το αντίγραφο ασφαλείας RDB (Redis βάση δεδομένων). Για να απενεργοποιήσετε Redis διατήρησης, κάντε κλικ στην επιλογή **απενεργοποιημένα**.

Για να ρυθμίσετε το διάστημα δημιουργίας αντιγράφων ασφαλείας, επιλέξτε τη **Συχνότητα δημιουργίας αντιγράφων ασφαλείας** από την αναπτυσσόμενη λίστα. Επιλογές περιλαμβάνουν **15 λεπτά**, **30 λεπτά**, **60 λεπτά**, **6 ώρες**, **12 ώρες**και **24 ώρες**. Αυτό το χρονικό διάστημα θα ξεκινήσει μετρώντας προς τα κάτω, αφού ολοκληρωθεί με επιτυχία η προηγούμενη λειτουργία δημιουργίας αντιγράφων ασφαλείας και όταν το μεσολαβεί ξεκινά ένα νέο αντίγραφο ασφαλείας.

Κάντε κλικ στην επιλογή για να επιλέξετε το λογαριασμό χώρου αποθήκευσης για να χρησιμοποιήσετε **Το λογαριασμό χώρου αποθήκευσης** και επιλέξτε είτε το **πρωτεύον κλειδί** ή **δευτερεύον κλειδί** για να χρησιμοποιήσετε από την αναπτυσσόμενη λίστα **Κλειδί αποθήκευσης** . Πρέπει να επιλέξετε ένα λογαριασμό του χώρου αποθήκευσης στην ίδια περιοχή ως το cache και ένα λογαριασμό του **Χώρου αποθήκευσης Premium** συνιστάται επειδή premium αποθήκευσης έχει υψηλότερη μεταγωγή. Οποιαδήποτε στιγμή το κλειδί χώρου αποθήκευσης για το λογαριασμό σας διατήρησης αναδημιουργείται, πρέπει να επιλέξετε ξανά το πλήκτρο που επιθυμείτε από την αναπτυσσόμενη λίστα **Κλειδί αποθήκευσης** .

Κάντε κλικ στο **κουμπί OK** για να αποθηκεύσετε τη ρύθμιση παραμέτρων διατήρησης.

>[AZURE.IMPORTANT] Διατήρηση δεδομένων redis είναι διαθέσιμη μόνο για Premium μνήμης cache. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να ρυθμίσετε τις παραμέτρους διατήρησης για Premium Azure Redis μνήμης Cache](cache-how-to-premium-persistence.md).

### <a name="importexport"></a>Εισαγωγή/εξαγωγή

Εισαγωγή/εξαγωγή είναι μια λειτουργία διαχείρισης δεδομένων Azure Redis Cache που σας επιτρέπει να εισαγάγετε δεδομένα στο Azure Redis Cache ή εξαγωγή δεδομένων από το Azure Redis Cache κατά την εισαγωγή και εξαγωγή ένα στιγμιότυπο της βάσης δεδομένων Redis Cache (RDB) από ένα cache premium σε μια σελίδα blob σε ένα λογαριασμό του χώρου αποθήκευσης Azure. Αυτό σας επιτρέπει να μετεγκαταστήσετε μεταξύ παρουσιών διαφορετικό Azure Redis Cache ή τη συμπλήρωση του cache με δεδομένα πριν από τη χρήση.

Εισαγωγή μπορεί να χρησιμοποιηθεί για να μεταφέρετε αρχεία συμβατά RDB Redis από οποιονδήποτε διακομιστή Redis λειτουργεί σε οποιοδήποτε cloud ή το περιβάλλον, συμπεριλαμβανομένων των Redis εκτελείται στο Linux, των Windows ή σε οποιαδήποτε υπηρεσία παροχής cloud όπως υπηρεσίες Web του Amazon και οι άλλοι χρήστες. Εισαγωγή δεδομένων είναι ένας εύκολος τρόπος για να δημιουργήσετε ένα cache με εκ των προτέρων συμπληρωμένες δεδομένων. Κατά τη διαδικασία εισαγωγής Azure Redis Cache φορτώνει τα αρχεία RDB από Azure χώρο αποθήκευσης στη μνήμη και, στη συνέχεια, εισάγει τα πλήκτρα σε το cache.

Εξαγωγή σάς επιτρέπει να εξαγάγετε τα δεδομένα που είναι αποθηκευμένα στο Cache Redis Azure να Redis συμβατά αρχεία RDB. Μπορείτε να χρησιμοποιήσετε αυτήν τη δυνατότητα για τη μετακίνηση δεδομένων από μία παρουσία Azure Redis Cache σε ένα άλλο ή σε άλλον διακομιστή Redis. Κατά τη διαδικασία εξαγωγής δημιουργείται ένα προσωρινό αρχείο στην η Εικονική που φιλοξενεί την παρουσία server Azure Redis Cache και το αρχείο αποστέλλεται στο λογαριασμό που έχει οριστεί ως χώρου αποθήκευσης. Όταν ολοκληρωθεί η λειτουργία εξαγωγής με είτε μια κατάσταση επιτυχίας ή αποτυχίας, διαγράφεται το προσωρινό αρχείο.

>[AZURE.IMPORTANT] Εισαγωγή/εξαγωγή είναι διαθέσιμη μόνο για μνήμης cache επίπεδο Premium. Για περισσότερες πληροφορίες και οδηγίες, ανατρέξτε στο θέμα [Εισαγωγή και εξαγωγή δεδομένων στο Azure Redis Cache](cache-how-to-import-export-data.md).


## <a name="administration-settings"></a>Ρυθμίσεις διαχείρισης

Οι ρυθμίσεις στην ενότητα **Διαχείριση** σάς επιτρέπουν να εκτελέσετε τις ακόλουθες εργασίες διαχείρισης για το cache premium. 

![Διαχείριση](./media/cache-configure/redis-cache-administration.png)

-   [Επανεκκινήστε τον υπολογιστή](#reboot)
-   [Προγραμματισμός ενημερώσεων](#schedule-updates)

>[AZURE.IMPORTANT] Οι ρυθμίσεις σε αυτήν την ενότητα είναι διαθέσιμα μόνο για μνήμης cache επίπεδο Premium.

### <a name="reboot"></a>Επανεκκινήστε τον υπολογιστή

Η **επανεκκίνηση της** blade σάς επιτρέπει να επανεκκίνηση της έναν ή περισσότερους κόμβους από το cache. Αυτό σας επιτρέπει να δοκιμάσετε την εφαρμογή σας για υποστηρίζεται σε περίπτωση αποτυχίας.

![Επανεκκινήστε τον υπολογιστή](./media/cache-configure/redis-cache-reboot.png)

Εάν έχετε ένα cache premium με σύμπλεγμα με δυνατότητα, μπορείτε να επιλέξετε ποια shards της μνήμης cache για να κάνετε επανεκκίνηση.

![Επανεκκινήστε τον υπολογιστή](./media/cache-configure/redis-cache-reboot-cluster.png)

Για να κάνετε επανεκκίνηση της μία ή περισσότερες κόμβους από το cache, επιλέξτε την επιθυμητή κόμβοι και κάντε κλικ στο κουμπί **επανεκκίνηση**. Εάν έχετε ένα cache premium με σύμπλεγμα με δυνατότητα, επιλέξτε το shard(s) να επανεκκίνηση της και, στη συνέχεια, κάντε κλικ στο κουμπί **επανεκκίνηση**. Μετά από μερικά λεπτά, το επιλεγμένο node(s) επανεκκινήστε τον υπολογιστή και είναι πίσω online λίγα λεπτά αργότερα.

>[AZURE.IMPORTANT] Επανεκκινήστε τον υπολογιστή είναι διαθέσιμη μόνο για μνήμης cache επίπεδο Premium. Για περισσότερες πληροφορίες και οδηγίες, ανατρέξτε στο θέμα [Διαχείριση Azure Redis Cache - επανεκκίνηση](cache-administration.md#reboot).

### <a name="schedule-updates"></a>Προγραμματισμός ενημερώσεων

Το **Χρονοδιάγραμμα ενημερώσεις** blade σάς επιτρέπει να καθορίσετε ένα παράθυρο συντήρησης Redis διακομιστή ενημερώσεις για το cache. 

>[AZURE.IMPORTANT] Σημειώστε ότι το παράθυρο Συντήρηση ισχύει μόνο για Redis ενημερώσεις διακομιστή, και όχι σε οποιαδήποτε Azure ενημερώνει ή ενημερώσεις για το λειτουργικό σύστημα του του ΣΠΣ που φιλοξενούν το cache.

![Προγραμματισμός ενημερώσεων](./media/cache-configure/redis-schedule-updates.png)

Για να καθορίσετε ένα παράθυρο συντήρησης, επιλέξτε την επιθυμητή ημέρες και καθορίστε την ώρα έναρξης του παραθύρου συντήρησης για κάθε ημέρα και κάντε κλικ στο κουμπί **OK**. Σημειώστε ότι η ώρα παράθυρο συντήρησης είναι στο UTC. 

>[AZURE.IMPORTANT] Προγραμματισμός ενημερώσεων είναι διαθέσιμη μόνο για μνήμης cache επίπεδο Premium. Για περισσότερες πληροφορίες και οδηγίες, ανατρέξτε στο θέμα [Διαχείριση Azure Redis Cache - προγραμματισμός ενημερώσεων](cache-administration.md#schedule-updates).

## <a name="diagnostics-settings"></a>Των ρυθμίσεων των διαγνωστικών

Στην ενότητα **Διαγνωστικά** σάς επιτρέπει να ρυθμίσετε τις παραμέτρους διαγνωστικών για το Cache Redis.

![Εργαλεία διαγνωστικών](./media/cache-configure/redis-cache-diagnostics.png)

Για να [ρυθμίσετε το λογαριασμό χώρου αποθήκευσης](cache-how-to-monitor.md#enable-cache-diagnostics) χρησιμοποιείται για την αποθήκευση Διαγνωστικά του cache, κάντε κλικ στην επιλογή **Διαγνωστικά** .

![Redis Διαγνωστικά του Cache](./media/cache-configure/redis-cache-diagnostics-settings.png)

Κάντε κλικ στην επιλογή **Redis μετρήσεις** για να [προβάλετε μετρικά](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts) για cache και **ειδοποίησης κανόνες** για να [ορίσετε κανόνες ειδοποίησης](cache-how-to-monitor.md#operations-and-alerts).

Για περισσότερες πληροφορίες σχετικά με τα Διαγνωστικά Azure Redis Cache, δείτε [πώς μπορείτε να παρακολουθείτε Azure Redis Cache](cache-how-to-monitor.md).


## <a name="network-settings"></a>Ρυθμίσεις δικτύου

Οι ρυθμίσεις στην ενότητα **δίκτυο** σάς επιτρέπουν να έχετε πρόσβαση και να ρυθμίσετε τις εξής παραμέτρους για το cache.

![Δικτύου](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] Ρυθμίσεις εικονικού δικτύου είναι διαθέσιμες μόνο για premium μνήμης cache που έχουν ρυθμιστεί με την υποστήριξη VNET κατά τη δημιουργία του cache. Για πληροφορίες σχετικά με τη δημιουργία μιας cache premium με VNET υποστήριξη και ενημέρωση των ρυθμίσεων, δείτε [πώς μπορείτε να ρυθμίσετε τις παραμέτρους εικονικού υποστήριξη δικτύου για Premium Azure Redis μνήμης Cache](cache-how-to-premium-vnet.md).

## <a name="resource-management-settings"></a>Ρυθμίσεις διαχείρισης πόρων

![Διαχείριση πόρων](./media/cache-configure/redis-cache-resource-management.png)

Στην ενότητα **ετικέτες** σας βοηθά να οργανώνετε τους πόρους σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση ετικετών για να οργανώσετε το Azure πόρους](../resource-group-using-tags.md).

Στην ενότητα **κλειδώνει** σάς επιτρέπει να κλειδώσετε μια συνδρομή, ομάδα πόρων ή έναν πόρο για να αποτρέψετε άλλους χρήστες στην εταιρεία σας από τη διαγραφή ή την τροποποίηση κρίσιμες πόρους κατά λάθος. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πόροι κλείδωμα με τη διαχείριση πόρων Azure](../resource-group-lock-resources.md).

Στην ενότητα **χρήστες** παρέχει υποστήριξη για έλεγχο πρόσβασης βάσει ρόλων (RBAC) στην πύλη του Azure για να πληροί τις απαιτήσεις διαχείρισης access απλώς και επακριβώς εταιρείες. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης βάσει ρόλων στην πύλη του Azure](../active-directory/role-based-access-control-configure.md).

Κάντε κλικ στην επιλογή **Εξαγωγή προτύπου** για δημιουργία και την εξαγωγή ενός προτύπου ανεπτυγμένος τους πόρους σας για μελλοντικές αναπτύξεις. Για περισσότερες πληροφορίες σχετικά με την εργασία με τα πρότυπα, ανατρέξτε στο θέμα [Ανάπτυξη πόρους με τα πρότυπα για τη διαχείριση πόρων Azure](../resource-group-template-deploy.md).

## <a name="default-redis-server-configuration"></a>Προεπιλεγμένη ρύθμιση παραμέτρων του διακομιστή Redis

Νέες παρουσίες Azure Redis Cache έχουν ρυθμιστεί με τις παρακάτω προεπιλεγμένες τιμές παραμέτρων Redis.

>[AZURE.NOTE] Οι ρυθμίσεις σε αυτήν την ενότητα δεν μπορεί να αλλάξει με χρήση του `StackExchange.Redis.IServer.ConfigSet` μέθοδο. Εάν αυτή η μέθοδος ονομάζεται με μία από τις εντολές σε αυτήν την ενότητα, μια παρόμοια με την παρακάτω εξαίρεση:  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Τις τιμές που είναι δυνατό να ρυθμιστούν, όπως **max-μνήμης-πολιτική**, είναι δυνατό να ρυθμιστούν μέσω του Azure Εργαλεία διαχείρισης πύλη ή γραμμή εντολών όπως Azure CLI ή PowerShell.

|Ρύθμιση|Προεπιλεγμένη τιμή|Περιγραφή|
|---|---|---|
|βάσεις δεδομένων|16|Ο προεπιλεγμένος αριθμός των βάσεων δεδομένων είναι 16, αλλά μπορείτε να ρυθμίσετε ένα διαφορετικό αριθμό με βάση τις πληροφορίες τιμολόγησης σειράς. <sup>1</sup> η προεπιλεγμένη βάση δεδομένων είναι DB 0, μπορείτε να επιλέξετε ένα διαφορετικό σε μια βάση ανά σύνδεση με χρήση `connection.GetDatabase(dbid)` όπου dbid είναι ένας αριθμός μεταξύ `0` και `databases - 1`.|
|maxclients|Εξαρτάται από το επίπεδο τιμολόγησης<sup>2</sup>|Αυτό είναι ο μέγιστος αριθμός των συνδεδεμένων πελατών επιτρέπεται την ίδια στιγμή. Μόλις το όριο Redis θα κλείσει όλες τις συνδέσεις νέα αποστολή σφάλμα 'μέγιστου αριθμού των πελατών που καλούν'.|
|maxmemory πολιτικής|πτητικές lru|Πολιτική Maxmemory είναι η ρύθμιση για το πώς Redis θα επιλέξτε τι θέλετε να καταργήσετε όταν φτάσει maxmemory (το μέγεθος του cache σας δίνει τη δυνατότητα που επιλέξατε όταν δημιουργήσατε το cache). Με το Azure Redis Cache η προεπιλεγμένη ρύθμιση είναι πτητικής-lru, το οποίο καταργεί τα πλήκτρα με ένα λήξης συνόλου με χρήση του αλγόριθμου LRU. Αυτή η ρύθμιση μπορεί να ρυθμιστεί στην πύλη του Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Maxmemory πολιτικής και δεσμευμένες maxmemory](#maxmemory-policy-and-maxmemory-reserved).|
|δείγματα maxmemory|3|LRU και αλγορίθμων ελάχιστους TTL δεν είναι ακριβείς αλγόριθμους αλλά κατά προσέγγιση ανάπτυξη αλγόριθμους (για να εξοικονομήσετε μνήμη), ώστε να μπορείτε να επιλέξετε καθώς και για να ελέγξετε το μέγεθος του δείγματος. Για παράδειγμα για προεπιλεγμένη Redis θα ελέγξετε τρία κλειδιά και επιλέξτε αυτήν που χρησιμοποιήθηκε λιγότερο πρόσφατα.|
|LUA προθεσμία|5.000|Μέγιστος χρόνος εκτέλεσης μιας δέσμης ενεργειών Lua σε χιλιοστά του δευτερολέπτου. Εάν έχει φτάσει τον μέγιστο χρόνο εκτέλεσης Redis θα συνδεθείτε που μια δέσμη ενεργειών εξακολουθεί να παραμένει στον εκτέλεσης μετά το μέγιστο επιτρεπόμενο χρόνο και θα ξεκινήσει για να απαντήσετε σε ερωτήματα με το μήνυμα σφάλματος.|
|όριο LUA συμβάντος|500|Αυτό είναι το μέγιστο μέγεθος του ουρά συμβάντων δέσμης ενεργειών.|
|pubsub normalclient--buffer-όριο εξόδου προγράμματος-πελάτη--buffer-όριο εξόδου|0 0 032mb 8mb 60|Τα όρια buffer εξόδου του προγράμματος-πελάτη μπορεί να χρησιμοποιηθεί για να επιβάλετε αποσύνδεσης των υπολογιστών-πελατών που δεν την ανάγνωση δεδομένων από το διακομιστή αρκετά γρήγορα για κάποιο λόγο (μια κοινή αιτία είναι ότι ένα πρόγραμμα-πελάτη Pub/Sub δεν είναι δυνατό να εκμετάλλευση γρήγορα τον εκδότη μπορούν να δημιουργήσουν τα μηνύματα). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [http://redis.io/topics/clients](http://redis.io/topics/clients).|

<a name="databases"></a>
<sup>1</sup> Το όριο για `databases` είναι διαφορετικό για κάθε Azure Redis μνήμη Cache τις τιμές επίπεδο και μπορεί να οριστεί σε cache δημιουργίας. Εάν δεν υπάρχει `databases` ρύθμιση ορίζεται κατά τη δημιουργία του cache, η προεπιλογή είναι 16.

-   Βασική και τυπική μνήμης cache
    -   C0 cache (250 MB) - έως 16 βάσεις δεδομένων
    -   C1 cache (1 GB) - έως 16 βάσεις δεδομένων
    -   C2 cache (2,5 GB) - έως 16 βάσεις δεδομένων
    -   C3 cache (6 GB) - έως 16 βάσεις δεδομένων
    -   C4 cache (13 GB) - έως 32 βάσεις δεδομένων
    -   C5 cache (26 GB) - έως 48 βάσεις δεδομένων
    -   C6 cache (53 GB) - έως και 64 βάσεις δεδομένων
-   Αποθηκεύει προσωρινά Premium
    -   P1 (6 GB - 60 GB) - έως 16 βάσεις δεδομένων
    -   P2 (13 GB - 130 GB) - έως 32 βάσεις δεδομένων
    -   P3 (26 GB - 260 GB) - έως 48 βάσεις δεδομένων
    -   P4 (53 GB - 530 GB) - έως και 64 βάσεις δεδομένων
    -   Όλες οι cache premium με σύμπλεγμα Redis με δυνατότητα - σύμπλεγμα Redis υποστηρίζει μόνο χρήση της βάσης δεδομένων 0 έτσι ώστε το `databases` περιορισμός για οποιαδήποτε cache premium με σύμπλεγμα Redis με δυνατότητα αποτελεσματικά είναι 1 και η εντολή [Select](http://redis.io/commands/select) δεν επιτρέπεται. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πρέπει να κάνετε αλλαγές εφαρμογής του προγράμματος-πελάτη για να χρησιμοποιήσετε σύμπλεγμα;](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)


>[AZURE.NOTE] Το `databases` ρύθμιση μπορεί να είναι ρυθμισμένες μόνο κατά τη δημιουργία της cache και μόνο με τη χρήση του PowerShell, CLI ή άλλα προγράμματα-πελάτες διαχείρισης. Ένα παράδειγμα για τη ρύθμιση των παραμέτρων `databases` κατά τη δημιουργία cache χρησιμοποιώντας το PowerShell, ανατρέξτε στο θέμα [Δημιουργία AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a>
<sup>2</sup> `maxclients` είναι διαφορετικό για κάθε Azure Redis μνήμη Cache τις τιμές σε επίπεδο.

-   Βασική και τυπική μνήμης cache
    -   C0 cache (250 MB) - έως και 256 συνδέσεις
    -   C1 cache (1 GB) - έως 1.000 συνδέσεις
    -   C2 cache (2,5 GB) - έως και 2.000 συνδέσεις
    -   C3 cache (6 GB) - έως 5.000 συνδέσεις
    -   C4 cache (13 GB) - έως 10.000 συνδέσεις
    -   C5 cache (26 GB) - έως 15.000 συνδέσεις
    -   C6 cache (53 GB) - έως 20.000 συνδέσεις
-   Αποθηκεύει προσωρινά Premium
    -   P1 (6 GB - 60 GB) - έως και 7.500 συνδέσεις
    -   P2 (13 GB - 130 GB) - έως 15.000 συνδέσεις
    -   P3 (26 GB - 260 GB) - έως και 30.000 συνδέσεις
    -   P4 (53 GB - 530 GB) - έως 40.000 συνδέσεις

## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis εντολές που δεν υποστηρίζονται στο Azure Redis Cache

>[AZURE.IMPORTANT] Επειδή η ρύθμιση των παραμέτρων και τη διαχείριση των παρουσιών Azure Redis Cache γίνεται από τη Microsoft είναι απενεργοποιημένες τις παρακάτω εντολές. Εάν προσπαθήσετε να καλέσετε τους θα λάβετε ένα μήνυμα σφάλματος που είναι παρόμοια με `"(error) ERR unknown command"`.
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  ΡΎΘΜΙΣΗ ΠΑΡΑΜΈΤΡΩΝ
>-  ΕΝΤΟΠΙΣΜΌΣ ΣΦΑΛΜΆΤΩΝ
>-  ΜΕΤΕΓΚΑΤΆΣΤΑΣΗ
>-  ΑΠΟΘΉΚΕΥΣΗ
>-  ΤΕΡΜΑΤΙΣΜΌΣ
>-  SLAVEOF
>-  ΣΎΜΠΛΕΓΜΑ - συμπλέγματος εγγραφής που είναι απενεργοποιημένες, αλλά μόνο για ανάγνωση σύμπλεγμα επιτρέπεται εντολές.

Για περισσότερες πληροφορίες σχετικά με τις εντολές Redis, ανατρέξτε στο θέμα [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Redis κονσόλας

Μπορείτε να εκδώσετε ασφαλή εντολές για να σας παρουσιών Azure Redis Cache χρησιμοποιώντας την **Κονσόλα Redis**, η οποία είναι διαθέσιμη για το πρότυπο και τα αποθηκεύει Premium.

>[AZURE.IMPORTANT] Κονσόλα Redis δεν λειτουργεί με VNET, δημιουργία συμπλέγματος, και βάσεις δεδομένων εκτός από 0. 
>
>-  [VNET](cache-how-to-premium-vnet.md) - όταν το cache είναι μέρος μιας VNET, μόνο τα προγράμματα-πελάτες σε VNET την πρόσβαση του cache. Επειδή στην κονσόλα Redis χρησιμοποιεί το πρόγραμμα-πελάτη redis cli.exe φιλοξενούνται σε ΣΠΣ που δεν αποτελούν μέρος της VNET σας, δεν μπορεί να συνδεθεί το cache.
>-  [Δημιουργία συμπλεγμάτων](cache-how-to-premium-clustering.md) - το κονσόλας Redis χρησιμοποιεί πελάτη redis cli.exe, η οποία δεν υποστηρίζει σύμπλεγμα αυτήν τη στιγμή. Το βοηθητικό πρόγραμμα redis cli στον κλάδο [ασταθής](http://redis.io/download) του αποθετηρίου Redis στο GitHub υλοποιεί βασική υποστήριξη όταν αποτελέσματα με το `-c` εναλλαγή. Για περισσότερες πληροφορίες ανατρέξτε στο θέμα [Αναπαραγωγή με το σύμπλεγμα](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) [http://redis.io](http://redis.io) στο το [Redis σύμπλεγμα πρόγραμμα εκμάθησης](http://redis.io/topics/cluster-tutorial).
>-  Κονσόλα Redis δημιουργεί μια νέα σύνδεση με βάση δεδομένων 0 κάθε φορά που υποβάλετε μια εντολή. Δεν μπορείτε να χρησιμοποιήσετε το `SELECT` εντολή για να επιλέξετε μια διαφορετική βάση δεδομένων, επειδή γίνεται επαναφορά της βάσης δεδομένων με το 0 με κάθε εντολή. Για πληροφορίες σχετικά με την εκτέλεση εντολών Redis, συμπεριλαμβανομένης της αλλαγής σε διαφορετική βάση δεδομένων, ανατρέξτε στο θέμα [Πώς μπορώ να εκτελέσω εντολές Redis;](cache-faq.md#how-can-i-run-redis-commands)

Για να αποκτήσετε πρόσβαση στην κονσόλα Redis, κάντε κλικ στην επιλογή **Κονσόλα** από το **Redis Cache** blade.

![Redis κονσόλας](./media/cache-configure/redis-console-menu.png)

Για να δίνουν εντολές σε σχέση με την παρουσία του cache, απλώς πληκτρολογήστε στην επιθυμητή εντολή στην κονσόλα.

![Redis κονσόλας](./media/cache-configure/redis-console.png)

Για τη λίστα των εντολών Redis που είναι απενεργοποιημένα για Azure Redis Cache, ανατρέξτε στην προηγούμενη ενότητα [Redis εντολές που δεν υποστηρίζονται στο Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) . Για περισσότερες πληροφορίες σχετικά με τις εντολές Redis, ανατρέξτε στο θέμα [http://redis.io/commands](http://redis.io/commands). 

## <a name="move-your-cache-to-a-new-subscription"></a>Μετακινήστε το cache σε μια νέα συνδρομή

Μπορείτε να μετακινήσετε το cache σε μια νέα συνδρομή, κάνοντας κλικ στην επιλογή **Μετακίνηση**.

![Μετακίνηση Redis Cache](./media/cache-configure/redis-cache-move.png)

Για πληροφορίες σχετικά με τη μετακίνηση πόρους από μία ομάδα πόρων σε μια άλλη και, από μια συνδρομή σε μια άλλη, ανατρέξτε στο θέμα [Μετακίνηση πόρων για νέα ομάδα πόρων ή τη συνδρομή](../resource-group-move-resources.md).

## <a name="next-steps"></a>Επόμενα βήματα
-   Για περισσότερες πληροφορίες σχετικά με την εργασία με εντολές Redis, ανατρέξτε στο θέμα [Πώς μπορώ να εκτελέσω εντολές Redis;](cache-faq.md#how-can-i-run-redis-commands).