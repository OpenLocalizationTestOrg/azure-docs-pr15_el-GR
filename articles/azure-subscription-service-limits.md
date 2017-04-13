<properties
    pageTitle="Συνδρομή στο Microsoft Azure και όρια υπηρεσίας, ορίων και περιορισμών"
    description="Παρέχει μια λίστα με κοινές Azure συνδρομής και όρια υπηρεσίας, όρια και περιορισμούς. Αυτό περιλαμβάνει πληροφορίες σχετικά με τον τρόπο για να αυξήσετε όρια μαζί με μέγιστο τιμών."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure συνδρομής και όρια υπηρεσίας, ορίων και περιορισμών

## <a name="overview"></a>Επισκόπηση

Αυτό το έγγραφο καθορίζει ορισμένα από τα πιο συνηθισμένα όρια Microsoft Azure. Αυτό δεν αυτήν τη στιγμή καλύπτει όλων των υπηρεσιών Azure. Διάρκεια του χρόνου, αυτά τα όρια θα έχει επεκταθεί και θα ενημερωθούν για να καλύψετε περισσότερα από την πλατφόρμα.

Επισκεφθείτε την [Επισκόπηση τιμολόγησης Azure](https://azure.microsoft.com/pricing/) για να μάθετε περισσότερα σχετικά με τις πληροφορίες τιμολόγησης Azure. Εκεί, μπορείτε να υπολογίσετε τις δαπάνες σας χρησιμοποιώντας την [Τιμολόγηση Αριθμομηχανής](https://azure.microsoft.com/pricing/calculator/) ή επισκεφθείτε τη σελίδα λεπτομερειών τιμολόγησης για μια υπηρεσία (για παράδειγμα, [Windows ΣΠΣ](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).

> [AZURE.NOTE] Εάν θέλετε να αυξήσετε το όριο πάνω από το **Προεπιλεγμένο όριο**, μπορείτε να [ανοίξετε μια αίτηση υποστήριξης πελατών online χωρίς χρέωση](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Τα όρια δεν είναι δυνατό να αυξηθεί επάνω από το **Μέγιστο όριο** τιμή στους παρακάτω πίνακες. Εάν δεν υπάρχει στήλη **Μέγιστο όριο** , στη συνέχεια, τον καθορισμένο πόρο δεν έχει προσαρμόσιμα όρια.

## <a name="limits-and-the-azure-resource-manager"></a>Όρια και τη διαχείριση πόρων Azure

Τώρα είναι δυνατό να συνδυάσετε πολλές Azure τους πόρους σε μία ομάδα πόρων Azure. Όταν χρησιμοποιείτε ομάδες πόρων, όρια που έχουν μόνο μία φορά καθολικού γίνονται διαχείρισης σε επίπεδο τοπικές με τη διαχείριση πόρων Azure. Για περισσότερες πληροφορίες σχετικά με τις ομάδες πόρων Azure, ανατρέξτε στο θέμα [Επισκόπηση της διαχείρισης πόρων Azure](azure-resource-manager/resource-group-overview.md).

Στα παρακάτω όρια, έναν νέο πίνακα έχει προστεθεί για να απεικονίσει οποιεσδήποτε διαφορές σε όρια κατά τη χρήση της διαχείρισης πόρων Azure. Για παράδειγμα, υπάρχει μια **Συνδρομή όρια** πίνακα και ενός πίνακα **Όρια συνδρομή - διαχείριση πόρων Azure** . Όταν ένα όριο που ισχύει για δύο σενάρια, εμφανίζεται μόνο στον πρώτο πίνακα. Εκτός και αν αναφέρεται διαφορετικά, όρια είναι καθολικές σε όλες τις περιοχές.

> [AZURE.NOTE] Είναι σημαντικό να δώσετε έμφαση ότι ορίων για τους πόρους σε ομάδες πόρων Azure είναι ανά περιοχή προσβάσιμο από τη συνδρομή σας, και όχι ανά-συνδρομή, όπως είναι η υπηρεσία ορίων διαχείρισης. Ας χρησιμοποιήσουμε ορίων πυρήνα ως παράδειγμα. Εάν χρειάζεστε για να ζητήσετε μια αύξηση ορίου με την υποστήριξη για πυρήνων, πρέπει να αποφασίσετε πόσες πυρήνων που θέλετε να χρησιμοποιήσετε σε ποιες περιοχές και, στη συνέχεια, κάντε μια συγκεκριμένη αίτηση για ομάδα πόρων Azure πυρήνα ορίων για τις ποσότητες και τις περιοχές που θέλετε. Επομένως, εάν θέλετε να χρησιμοποιήσετε 30 πυρήνων σε Δυτική Ευρώπη για να εκτελέσετε την εφαρμογή σας εκεί; θα πρέπει να μπορείτε να υποβάλετε αίτηση 30 πυρήνων σε Δυτική Ευρώπη συγκεκριμένα. Αλλά δεν θα έχετε ένα όριο πυρήνα αυξήσετε σε οποιαδήποτε άλλη περιοχή--μόνο Δυτική Ευρώπη θα έχουν του ορίου πυρήνα 30.
<!-- -->
Ως αποτέλεσμα, που μπορεί να σας φανεί χρήσιμο να μπορείτε να αποφασίσετε τι σας ομάδα πόρων Azure ορίων πρέπει να είναι για το φόρτο εργασίας σε κάθε μία περιοχή και ζητήστε το ποσό σε κάθε περιοχή στο οποίο εξετάζετε το ενδεχόμενο να ανάπτυξης. Ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων σε θέματα ανάπτυξης](./resource-manager-common-deployment-errors.md) για περισσότερη βοήθεια εντοπισμός του τρέχοντος ορίων για συγκεκριμένες περιοχές.

## <a name="service-specific-limits"></a>Όρια συγκεκριμένης υπηρεσίας

- [Υπηρεσία καταλόγου Active Directory](#active-directory-limits)
- [API διαχείρισης](#api-management-limits)
- [Εφαρμογή υπηρεσίας](#app-service-limits)
- [Πύλη εφαρμογής](#application-gateway-limits)
- [Εφαρμογή ιδέες](#application-insights-limits)
- [Αυτοματοποίηση](#automation-limits)
- [Azure Redis Cache](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Δημιουργία αντιγράφων ασφαλείας](#backup-limits)
- [Μαζική](#batch-limits)
- [Υπηρεσίες BizTalk](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [Υπηρεσίες cloud](#cloud-services-limits)
- [Προέλευση δεδομένων](#data-factory-limits)
- [Ανάλυση δεδομένων λίμνης](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Διανομείς συμβάντος](#event-hubs-limits)
- [Ο διανομέας IoT](#iot-hub-limits)
- [Πλήκτρο θάλαμο](#key-vault-limits)
- [Υπηρεσίες πολυμέσων](#media-services-limits)
- [Δέσμευση κινητές συσκευές](#mobile-engagement-limits)
- [Υπηρεσίες κινητές συσκευές](#mobile-services-limits)
- [Παρακολούθηση](#monitoring-limits)
- [Έλεγχος ταυτότητας πολλών παραγόντων](#multi-factor-authentication)
- [Δικτύωση](#networking-limits)
- [Υπηρεσία ειδοποιήσεων διανομέα](#notification-hub-service-limits)
- [Λειτουργικές ιδέες](#operational-insights-limits)
- [Ομάδα πόρων](#resource-group-limits)
- [Χρονοδιάγραμμα](#scheduler-limits)
- [Αναζήτηση](#search-limits)
- [Υπηρεσία Bus](#service-bus-limits)
- [Επαναφορά τοποθεσίας](#site-recovery-limits)
- [Βάση δεδομένων SQL](#sql-database-limits)
- [Χώρος αποθήκευσης](#storage-limits)
- [StorSimple συστήματος](#storsimple-system-limits)
- [Ροή ανάλυσης](#stream-analytics-limits)
- [Συνδρομή](#subscription-limits)
- [Διαχείριση κίνηση](#traffic-manager-limits)
- [Εικονικές μηχανές](#virtual-machines-limits)
- [Σύνολα κλίμακα εικονική μηχανή](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Όρια συνδρομή
#### <a name="subscription-limits"></a>Όρια συνδρομή
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Όρια συνδρομή - διαχείριση πόρων Azure

Τα ακόλουθα όρια ισχύουν όταν χρησιμοποιείτε τη διαχείριση πόρων Azure και τις ομάδες πόρων Azure. Όρια που δεν έχουν αλλάξει με τη διαχείριση πόρων Azure δεν παρατίθενται πιο κάτω. Ανατρέξτε στον προηγούμενο πίνακα για αυτά τα όρια.

Για πληροφορίες σχετικά με το χειρισμό όρια για αιτήσεις για τη διαχείριση πόρων, ανατρέξτε στο θέμα [Διαχείριση περιορισμού πόρων αιτήσεις](resource-manager-request-limits.md).

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Όρια ομάδα πόρων

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Όρια εικονικές μηχανές
#### <a name="virtual-machine-limits"></a>Όρια εικονική μηχανή
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Όρια εικονικές μηχανές - διαχείριση πόρων Azure

Τα ακόλουθα όρια ισχύουν όταν χρησιμοποιείτε τη διαχείριση πόρων Azure και τις ομάδες πόρων Azure. Όρια που δεν έχουν αλλάξει με τη διαχείριση πόρων Azure δεν παρατίθενται πιο κάτω. Ανατρέξτε στον προηγούμενο πίνακα για αυτά τα όρια.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Όρια σύνολα κλίμακα εικονική μηχανή

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Όρια δικτύωση

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Όρια δικτύωση
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Τα όρια πύλη εφαρμογής

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>Όρια Manager κίνηση

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>Όρια DNS

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Όρια χώρου αποθήκευσης

Για περισσότερες λεπτομέρειες σχετικά με τα όρια χώρου αποθήκευσης λογαριασμού, ανατρέξτε στο θέμα [Azure κλιμάκωση χώρου αποθήκευσης και τους στόχους επιδόσεων](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Όρια χώρου αποθήκευσης υπηρεσίας

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>Όρια δίσκου εικονική μηχανή

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [μεγέθη εικονική μηχανή](../articles/virtual-machines/virtual-machines-linux-sizes.md) .

**Λογαριασμοί τυπική χώρου αποθήκευσης**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Λογαριασμοί Premium χώρου αποθήκευσης**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Ορίων χώρου αποθήκευσης της υπηρεσίας παροχής πόρων

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Όρια υπηρεσίες cloud

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>Όρια εφαρμογής υπηρεσίας
Τα ακόλουθα όρια εφαρμογής υπηρεσίας περιλαμβάνουν όρια για εφαρμογές Web, οι εφαρμογές του Mobile, API εφαρμογές και λογική εφαρμογών.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Όρια του χρονοδιαγράμματος

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Όρια δέσμης

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>Όρια BizTalk υπηρεσιών
Ο παρακάτω πίνακας εμφανίζει τα όρια για τις υπηρεσίες Biztalk Azure.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>Όρια DocumentDB

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Όρια που παρατίθενται με έναν αστερίσκο (*) [μπορούν να ρυθμιστούν με επικοινωνία με την υποστήριξη του Azure](./documentdb/documentdb-increase-limits.md).

### <a name="mobile-engagement-limits"></a>Όρια Mobile δέσμευση

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Όρια αναζήτησης

Πληροφορίες για την τιμολόγηση βαθμίδες καθορίζουν τα όρια της υπηρεσίας αναζήτησης και χωρητικότητα. Βαθμίδες περιλαμβάνουν τα εξής:

- *Δωρεάν* υπηρεσία πολλών μισθωτών, από κοινού με άλλους Azure συνδρομητές, προορίζεται για αξιολόγησης και ανάπτυξη μικρές έργα.
- *Βασική* παρέχει αποκλειστικό υπολογιστικών πόρων για παραγωγής φόρτους εργασίας σε ένα μικρότερο κλίμακα, με έως και τρεις αντίγραφα για ιδιαίτερα διαθέσιμη ερωτήματος φόρτους εργασίας.
- *Τυπική (S1, S2, S3, υψηλή πυκνότητα S3)* είναι μεγαλύτερα παραγωγής φόρτους εργασίας. Πολλά επίπεδα υπάρχουν εντός την τυπική σειρά, έτσι ώστε να μπορείτε να επιλέξετε μια ρύθμιση παραμέτρων πόρων για συγκεκριμένα σενάρια.

**Όρια ανά συνδρομή**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Όρια ανά υπηρεσίας αναζήτησης**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Για πιο λεπτομερείς πληροφορίες σχετικά με άλλα όρια, όπως το μέγεθος του εγγράφου, ερωτημάτων ανά δευτερόλεπτο, πλήκτρα, προσκλήσεων και απαντήσεων, ανατρέξτε στο θέμα [όρια υπηρεσίας στην αναζήτηση Azure](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Όρια Media Services

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>Όρια CDN

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Όρια υπηρεσίες Mobile

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Παρακολούθηση ορίων

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Υπηρεσία ειδοποιήσεων διανομέα τα όρια

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Όρια διανομείς συμβάντος

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Όρια Bus υπηρεσίας

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>Όρια IoT διανομέα

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Όρια εργοστασίου δεδομένων

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Όρια λίμνης ανάλυση δεδομένων
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Όρια ανάλυση ροής
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Τα όρια υπηρεσίας καταλόγου Active Directory

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Τα όρια Azure RemoteApp

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>Όρια του συστήματος StorSimple

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Όρια λειτουργίας ιδέες

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Όρια δημιουργίας αντιγράφων ασφαλείας

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Τα όρια Επαναφορά τοποθεσίας

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Όρια ιδέες εφαρμογής

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>Όρια API διαχείρισης

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure όρια Redis Cache

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Όρια θάλαμο αριθμού-κλειδιού

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Έλεγχος ταυτότητας πολλών παραγόντων
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Όρια αυτοματισμού
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>Τα όρια της βάσης δεδομένων SQL

Για όρια βάση δεδομένων SQL, ανατρέξτε στο θέμα [Όρια πόρων βάσης δεδομένων SQL](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Δείτε επίσης

[Κατανόηση των ορίων Azure και αυξάνεται](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Εικονική μηχανή και τα μεγέθη υπηρεσία Cloud για Azure](virtual-machines/virtual-machines-linux-sizes.md)

[Μεγέθη για τις υπηρεσίες Cloud](cloud-services/cloud-services-sizes-specs.md)
