
<properties
   pageTitle="Azure καθοδήγηση | μοτίβα & πρακτικές | Microsoft Azure"
   description="Βέλτιστες πρακτικές και οδηγίες για Azure"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="christb"/>

# <a name="azure-guidance"></a>Καθοδήγηση Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ομάδα μοτίβα & πρακτικές της Microsoft αποτελεί τμήμα της ομάδας συμβουλευτική πελατών Azure. Σκοπός μας είναι να βοηθήσει τους προγραμματιστές, αρχιτέκτονες και οι επαγγελματίες τεχνολογιών πληροφορικής ήταν επιτυχής σε της πλατφόρμας Windows Azure. Αναπτύσσουμε οδηγίες που εμφανίζει τις βέλτιστες πρακτικές για τη δημιουργία λύσεων cloud στην Azure.

## <a name="checklists"></a>Λίστες ελέγχου

Αυτές οι λίστες είναι μια γρήγορη αναφορά για την εξέταση τα θεμελιώδη στοιχεία του διαθεσιμότητα και κλιμάκωση. 

- [Λίστα ελέγχου διαθεσιμότητας][AvailabilityChecklist] 

    Μια σύνοψη των προτεινόμενων πρακτικές για τη διασφάλιση της διαθεσιμότητας.

- [Λίστα ελέγχου κλιμάκωση][ScalabilityChecklist]

    Μια σύνοψη των προτεινόμενων πρακτικές για τη σχεδίαση και υλοποίηση με τις υπηρεσίες και χειρισμός διαχείρισης δεδομένων.

## <a name="best-practices-articles"></a>Βέλτιστες πρακτικές για άρθρα

Τα άρθρα αυτά παρέχουν μια λεπτομερής ανάλυση της σημαντικές έννοιες συνήθως που σχετίζεται με το cloud υπολογιστική. 

- [Σχεδίαση API][APIDesign] 

    Μια συζήτηση σχεδίαση θέματα που πρέπει να λάβετε υπόψη κατά τη σχεδίαση μιας API web.

- [Υλοποίηση API][APIImplementation] 

    Ένα σύνολο προτεινόμενες πρακτικές για την υλοποίηση και τη δημοσίευση ενός API web.

- [Οδηγίες ασφαλείας API](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 

    Μια συζήτηση του ελέγχου ταυτότητας και εξουσιοδότησης ανησυχίες (για παράδειγμα, τύποι διακριτικών, πρωτόκολλα εξουσιοδότησης, ροών εξουσιοδότησης και μετριασμού απειλή).

- [Καθοδήγηση Autoscaling][AutoscalingGuidance] 

    Μια σύνοψη των ζητήματα για κλιμάκωση λύσεις χωρίς την ανάγκη για μη αυτόματη παρέμβαση.

- [Καθοδήγηση εργασίες φόντου][BackgroundJobsGuidance] 

    Μια περιγραφή των διαθέσιμων επιλογών και προτεινόμενες πρακτικές για την υλοποίηση εργασίες που θα πρέπει να εκτελείται στο παρασκήνιο, ανεξάρτητα από οποιοδήποτε αλληλεπιδραστικό λειτουργίες ή πρώτου πλάνου.

- [Οδηγίες περιεχομένου παράδοσης δικτύου (CDN)][CDNGuidance] 

    Γενικές οδηγίες και πρακτική που συνιστάται για τη χρήση του CDN για να ελαχιστοποιήσετε το φόρτο τις εφαρμογές σας, και μεγιστοποίηση διαθεσιμότητα και της απόδοσης.

- [Προσωρινή αποθήκευση καθοδήγηση][CachingGuidance] 

    Μια σύνοψη των πώς μπορείτε να χρησιμοποιήσετε σε cache για να βελτιώσετε την απόδοση και κλιμάκωση ενός συστήματος.

- [Καθοδήγηση διαμέριση δεδομένων][DataPartitioningGuidance]

    Στρατηγικές που μπορείτε να χρησιμοποιήσετε για διαμερίσματα δεδομένων για να βελτιώσω την κλιμάκωση, μειώστε ασυμφωνίας και βελτιστοποίηση των επιδόσεων.

- [Καθοδήγηση παρακολούθησης και διαγνωστικών][MonitoringandDiagnosticsGuidance] 

    Καθοδήγηση σχετικά με την παρακολούθηση πώς οι χρήστες χρησιμοποιούν το σύστημά σας, να ανιχνεύσετε χρήσης πόρων και, γενικά παρακολουθεί την εύρυθμη λειτουργία και την απόδοση του συστήματος.

- [Προτεινόμενα συμβάσεις ονομασίας][naming-conventions] 

    Συνιστάται η συνθήκες ονοματοθεσίας για Azure πόρους.

- [Γενικές οδηγίες για τη επανάληψης][RetryGeneralGuidance] 

    Συζήτηση για τις γενικές έννοιες για το χειρισμό μεταβατικές σφάλματα.

- [Επανάληψη καθοδήγηση συγκεκριμένης υπηρεσίας][RetryServiceSpecificGuidance]

    Μια σύνοψη των δυνατοτήτων "Επανάληψη" για πολλές Azure υπηρεσιών, συμπεριλαμβανομένων των πληροφοριών που θα σας βοηθήσουν να χρησιμοποιήσετε, να προσαρμοστεί ή να επεκτείνετε το μηχανισμό "Επανάληψη" για αυτήν την υπηρεσία.

## <a name="scenario-guides"></a>Σενάριο οδηγοί

- [Εκτέλεση Elasticsearch σε Azure][elasticsearch] 
    
    Elasticsearch είναι μια μηχανή αναζήτησης ιδιαίτερα μεταβλητού μεγέθους ανοιχτού κώδικα και βάση δεδομένων. Είναι κατάλληλη για περιπτώσεις που απαιτούν γρήγορη ανάλυση και εντοπισμού των πληροφοριών που θα διατηρούνται στα μεγάλων συνόλων δεδομένων. Αυτές οι οδηγίες εξετάζει ορισμένες βασικές πτυχές πρέπει να λάβετε υπόψη κατά τη σχεδίαση ένα σύμπλεγμα Elasticsearch.

- [Η Διαχείριση ταυτοτήτων για multitenant εφαρμογές][identity-multitenant] 
    
    Multitenancy είναι μια αρχιτεκτονική όπου πολλούς μισθωτές κοινή χρήση μιας εφαρμογής αλλά είναι απομονωμένες από το ένα το άλλο. Αυτές οι οδηγίες δείχνει πώς μπορείτε να διαχειριστείτε τις ταυτότητες χρήστη σε μια εφαρμογή του multitenant, με χρήση [Azure Active Directory] [ AzureAD] χειρισμού εισόδου και τον έλεγχο ταυτότητας.
    
- [Ανάπτυξη λύσεων μεγάλο δεδομένων](https://msdn.microsoft.com/library/dn749874.aspx)

    Αυτός ο οδηγός εξετάζει τη χρήση HDInsight για σενάρια όπως επαναληπτικού Εξερεύνηση, ως αποθήκη δεδομένων, για διεργασίες ETL και ενσωμάτωση στην υπάρχουσα συστήματα BI. Περιλαμβάνει επίσης οδηγίες σχετικά με την κατανόηση τις έννοιες μεγάλο δεδομένων, σχεδιασμό και λύσεις μεγάλο δεδομένων σχεδίαση και υλοποίηση αυτές τις λύσεις.
    
## <a name="patterns"></a>Μοτίβα

- [Σύννεφο σχεδίαση μοτίβα: Καθιερωμένα αρχιτεκτονική οδηγίες για τις εφαρμογές του Cloud](https://msdn.microsoft.com/library/dn568099.aspx)

    Μοτίβα σχεδίαση cloud είναι η βιβλιοθήκη μοτίβα σχεδιασμού και καθοδήγηση σχετικά θέματα. Το articulates το όφελος από την εφαρμογή μοτίβα, που δείχνει τον τρόπο κάθε τμήμα μπορεί να χωρέσει στο cloud αρχιτεκτονικές εφαρμογής.
    
- [Βελτιστοποίηση των επιδόσεων για τις εφαρμογές του Cloud](https://github.com/mspnp/performance-optimization)

    Αυτές οι οδηγίες είναι μια Εξερεύνηση μοτίβων κοινές καταπολέμησης που εμποδίζουν εφαρμογές από κλίμακας φόρτος. Περιλαμβάνει δείγματα που καταδεικνύουν οκτώ καταπολέμησης μοτίβα και ένα [κύριο βήμα Ανάλυση απόδοσης](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) και έναν οδηγό για [την εκτίμηση της απόδοσης κατά μετρικά κλειδιού](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md).

## <a name="reference-architectures"></a>Αρχιτεκτονικών αναφοράς

Μας αρχιτεκτονικών αναφοράς έχουν τακτοποιηθεί με το σενάριο.
Κάθε μεμονωμένα αρχιτεκτονική προσφέρει προτεινόμενες πρακτικές και καθιερωμένα βήματα και ένα στοιχείο εκτελέσιμο που ενσωματώνει τις συστάσεις.

Η τρέχουσα βιβλιοθήκη του αρχιτεκτονικών αναφοράς είναι διαθέσιμη από την [http://aka.ms/architecture](http://aka.ms/architecture).

## <a name="resiliency-guidance"></a>Καθοδήγηση υποστηρίζεται

Αυτά τα θέματα περιγράφουν τον τρόπο για να σχεδιάσετε εφαρμογές που είναι είναι ανθεκτικά στις αποτυχία σε ένα περιβάλλον κατανέμεται cloud.   

- [Επισκόπηση υποστηρίζεται][ResiliencyOvervew]

     Μάθετε πώς μπορείτε να δημιουργήσετε εφαρμογές στην Azure πλατφόρμα που μπορούν να ανακτήσετε από αποτυχίες και να συνεχίσει να λειτουργεί. Περιγράφει μια δομημένη προσέγγιση για την επίτευξη υποστηρίζεται από σχεδίαση για να την εφαρμογή, ανάπτυξης και λειτουργίες.

- [Λίστα ελέγχου υποστηρίζεται][resiliency-checklist]

    Μια λίστα ελέγχου συστάσεις που θα σας βοηθήσουν να Σχεδιασμός για μια ποικιλία αποτυχία λειτουργιών που θα μπορούσε να συμβεί.

- [Αποτυχία λειτουργία ανάλυσης][resiliency-fma] 

    Αποτυχία λειτουργία ανάλυσης (FMA) είναι μια διαδικασία για τη δημιουργία υποστηρίζεται σε ένα σύστημα, προσδιορίζοντας πιθανές αποτυχία σημεία. Ως σημείο εκκίνησης για τη διαδικασία FMA, αυτό το άρθρο περιέχει έναν κατάλογο πιθανές λειτουργίες αποτυχία και τους μετριασμούς. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md

