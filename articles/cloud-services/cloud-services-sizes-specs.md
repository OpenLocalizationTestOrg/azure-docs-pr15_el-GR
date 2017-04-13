<properties
 pageTitle="Μεγέθη για τις υπηρεσίες cloud | Microsoft Azure"
 description="Παραθέτει τα μεγέθη διαφορετικό εικονική μηχανή (και αναγνωριστικά) για Azure cloud υπηρεσίας web και εργαζόμενου ρόλους."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>Μεγέθη για τις υπηρεσίες Cloud

Αυτό το θέμα περιγράφει τα διαθέσιμα μεγέθη και επιλογές για την υπηρεσία Cloud ρόλο παρουσίες (ρόλους web και εργαζόμενου). Επίσης, παρέχει θέματα ανάπτυξης για να λάβετε υπόψη κατά το σχεδιασμό για να χρησιμοποιήσετε αυτούς τους πόρους. Κάθε μεγέθους έχει ένα Αναγνωριστικό που θα τοποθετήσετε στο [αρχείο ορισμού υπηρεσίας](cloud-services-model-and-package.md#csdef).

Υπηρεσίες cloud είναι ένας από τους τύπους των πόρων υπολογισμού που παρέχεται από Azure. Κάντε κλικ [εδώ](cloud-services-choose-me.md) για περισσότερες πληροφορίες σχετικά με τις υπηρεσίες Cloud.

> [AZURE.NOTE]Για να δείτε τις σχετικές Azure όρια, ανατρέξτε στο θέμα [συνδρομή Azure και όρια υπηρεσίας, όρια, και τους περιορισμούς](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>Μεγέθη για το web και εργαζόμενου παρουσίες ρόλων

Υπάρχουν πολλά βασικά μεγέθη για να επιλέξετε στο Azure. Ζητήματα σχετικά με ορισμένα από αυτά τα μεγέθη περιλαμβάνουν τα εξής:

* Σειρά D ΣΠΣ έχουν σχεδιαστεί για να εκτελέσετε εφαρμογές που απαιτούν μεγαλύτερη power υπολογισμού και επιδόσεων προσωρινό δίσκο. D σειρά ΣΠΣ παρέχουν ταχύτερους επεξεργαστές, αναλογία υψηλότερη μνήμης-πυρήνα και μια μονάδα δίσκου οπτικοί (SSD) για τον προσωρινό δίσκο. Για λεπτομέρειες, ανατρέξτε στην ανακοίνωση στο ιστολόγιο του Azure, [Νέα μεγέθη εικονική μηχανή D σειρά](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

* Dv2 σειρά, ένα στις τροφές δεύτερης βρεφικής για την αρχική D-σειρά, δυνατότητες μια πιο ισχυρή CPU. Της CPU Dv2 σειρά είναι περίπου 35% ταχύτερα από της CPU D σειρά. Βασίζεται σε την τελευταία γενιά 2,4 GHz Intel Xeon® E5-2673 v3 Επεξεργαστής (Haswell), και με το 2.0 τεχνολογία Intel των ενίσχυση, να μεταβείτε προς τα επάνω στο 3.1 GHz. Η σειρά Dv2 έχει τις ίδιες ρυθμίσεις παραμέτρων μνήμης και δίσκου ως η σειρά D.

*   Σειρά G ΣΠΣ προσφέρουν την περισσότερη μνήμη και να εκτελέσετε σε κεντρικούς υπολογιστές οι οποίοι έχουν οικογένειας επεξεργαστές Intel Xeon E5 V3.

*   Το ΣΠΣ μια σειρά μπορεί να αναπτυχθεί σε μια ποικιλία τύπους υλικού και επεξεργαστές. Το μέγεθος είναι επιβραδύνει, ανάλογα με το υλικό, για την απόδοση συνεπή επεξεργαστή για την παρουσία που εκτελείται, ανεξάρτητα από το υλικό που είναι αναπτυγμένο στην προσφορά. Για να προσδιορίσετε το φυσικό υλικό στην οποία έχει αναπτυχθεί αυτό το μέγεθος, ερώτημα το εικονικό υλικό από μέσα σε η εικονική μηχανή.

*   Το μέγεθος A0 είναι υπερ-εγγεγραμμένο του φυσικού υλικού. Για το συγκεκριμένο μέγεθος μόνο, άλλες αναπτύξεις πελάτη μπορεί να επηρεάσουν την απόδοση του σας εκτελείται φόρτο εργασίας. Οι επιδόσεις σχετική περιγράφεται παρακάτω με τα αναμενόμενα γραμμής βάσης, υπόκεινται σε μια κατά προσέγγιση τη μεταβλητότητα των 15 τοις εκατό.


Το μέγεθος του η εικονική μηχανή επηρεάζει την τιμολόγηση. Το μέγεθος επηρεάζει επίσης η δυναμικότητα επεξεργασίας, μνήμης και αποθήκευσης των η εικονική μηχανή. Έξοδα αποθήκευσης υπολογίζονται με βάση ξεχωριστά σε σελίδες που χρησιμοποιήθηκαν από το λογαριασμό χώρου αποθήκευσης. Για λεπτομέρειες, ανατρέξτε στο θέμα [Εικονικές μηχανές τιμολόγησης λεπτομέρειες](https://azure.microsoft.com/pricing/details/virtual-machines/) και [Τις τιμές του Azure χώρου αποθήκευσης](https://azure.microsoft.com/pricing/details/storage/). 


Τα παρακάτω ζητήματα ενδέχεται να σας βοηθήσουν να αποφασίσετε σε μέγεθος:


* Τα μεγέθη A8 A11 και H σειρά είναι επίσης γνωστή ως *παρουσίες στενής υπολογισμού*. Έχει σχεδιαστεί και βελτιστοποιηθεί για υπολογισμού στενής το υλικό που εκτελεί αυτά τα μεγέθη και τις εφαρμογές του δικτύου στενής, συμπεριλαμβανομένων των υπολογιστική υψηλών επιδόσεων (HPC) συμπλέγματος εφαρμογές, μοντελοποίηση και προσομοιώσεις. Η σειρά A8 A11 χρησιμοποιεί Intel Xeon E5-2670 @ 2.6 GHZ και η σειρά H χρησιμοποιεί v3 Intel Xeon E5-2667 @ 3.2 GHz. Για λεπτομερείς πληροφορίες και ζητήματα σχετικά με τη χρήση αυτών των μεγεθών, ανατρέξτε στο θέμα [σχετικά με το H-σειρά και υπολογισμού στενής ΣΠΣ μια σειρά](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 

* Dv2-σειρά, Δ-σειρά, G-σειρά, είναι ιδανική για εφαρμογές που απαιτούν ταχύτερη CPU, καλύτερη τοπικό δίσκο επιδόσεων ή εάν έχετε υψηλότερες απαιτήσεις μνήμης.  Σας παρέχουν ένα ισχυρό συνδυασμό για πολλές εφαρμογές επίπεδου μεγάλης εταιρείας.

*   Ορισμένα των φυσικών κεντρικών υπολογιστών στα κέντρα δεδομένων Azure ενδέχεται να μην υποστηρίζει μεγαλύτερα μεγέθη εικονική μηχανή, όπως A5 – A11. Ως αποτέλεσμα, μπορείτε να δείτε το μήνυμα σφάλματος **απέτυχε να ρυθμίσετε τις παραμέτρους εικονική μηχανή {όνομα υπολογιστή}** ή **απέτυχε η δημιουργία εικονική μηχανή {όνομα υπολογιστή}** , κατά την αλλαγή μεγέθους ενός υπάρχοντος εικονική μηχανή για ένα νέο μέγεθος; Δημιουργία ενός νέου εικονική μηχανή στο ένα εικονικό δίκτυο που δημιουργήθηκαν πριν από 16 Απριλίου 2013; ή προσθέτοντας μια νέα εικονική μηχανή σε μια υπάρχουσα υπηρεσία cloud. Ανατρέξτε στο θέμα [σφάλμα: "Απέτυχε να ρυθμίσετε τις παραμέτρους εικονική μηχανή"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) στο φόρουμ υποστήριξης για λύσεις για κάθε σενάριο ανάπτυξης.  

* Τη συνδρομή σας μπορεί επίσης να περιορίσετε τον αριθμό πυρήνων μπορείτε να αναπτύξετε σε ορισμένες οικογένειες μέγεθος. Για να αυξήσετε ένα όριο, επικοινωνήστε με την υποστήριξη Azure.


## <a name="performance-considerations"></a>Ζητήματα επιδόσεων

Έχουμε δημιουργήσει την έννοια της το Azure τον υπολογισμό μονάδας (ACU) για την παροχή έναν τρόπο για τη σύγκριση απόδοσης υπολογιστή (CPU) σε SKU Azure. Αυτό θα σας βοηθήσει να εντοπίζετε εύκολα που είναι πιο πιθανό να ικανοποιεί τις ανάγκες σας επιδόσεων SKU.  ACU είναι τυποποιημένες αυτήν τη στιγμή σε ένα μικρό (Standard_A1) Εικονική, στη συνέχεια, να 100 και όλες τις άλλες SKU αντιπροσωπεύουν περίπου πόσο πιο γρήγορα ότι SKU μπορεί να εκτελεστεί μια τυπική συγκριτικών. 

>[AZURE.IMPORTANT] Το ACU είναι μόνο κατευθυντήριες γραμμές.  Τα αποτελέσματα για το φόρτο εργασίας ενδέχεται να διαφέρουν. 

<br>

|Οικογένεια SKU |ACU/πυρήνα |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4](#a-series) |100 |
|[Standard_A5-7](#a-series) |100 |
|[A8 A11](#a-series)    |225 *|
|[D1-14](#d-series) |160 |
|[D1 15v2](#dv2-series) |210 - 250 *|
|[G1-5](#g-series)  |180 - 240 *|
|[H](#h-series) |290 - 300 *|

ACUs έχει επισημανθεί με ένα * Χρησιμοποιήστε τεχνολογία Intel® των για να αυξήσετε συχνότητα CPU και δώστε μια ενίσχυση επιδόσεων.  Το ποσό της την ενίσχυση μπορεί να ποικίλλει ανάλογα με το μέγεθος Εικονική φόρτο εργασίας και άλλων φόρτους εργασίας που εκτελούνται στον ίδιο κεντρικό υπολογιστή.

## <a name="size-tables"></a>Προσαρμογή μεγέθους πινάκων

Οι παρακάτω πίνακες δείχνουν τα μεγέθη και των δυνατοτήτων που παρέχουν.

* Χωρητικότητα αποθήκευσης εμφανίζεται στις μονάδες απομείωση ή 1024 ^ 3 byte που. Κατά τη σύγκριση δίσκων υπολογίζεται σε GB (1000 ^ 3 byte που) για να δίσκων υπολογίζεται σε απομείωση (1024 ^ 3) να θυμάστε ότι δυναμικότητας αριθμούς που δίνονται σε απομείωση μπορεί να εμφανίζονται μικρότερο μέγεθος. Για παράδειγμα, 1023 απομείωση = 1098.4 GB

* Ταχύτητα μεταγωγής του δίσκου μετράται σε λειτουργίες εισαγωγής/εξαγωγής ανά δευτερόλεπτο (IOP Προέλευσης) και MBps όπου MBps = 10 ^ 6 byte/sec.

* Μπορεί να λειτουργήσει δίσκων δεδομένων στο cache ή χωρίς λειτουργίες. Για τη λειτουργία δίσκου δεδομένα στο cache, η λειτουργία cache κεντρικού υπολογιστή έχει οριστεί **μόνο για ανάγνωση** ή **ReadWrite**.  Για τη λειτουργία δίσκου χωρίς δεδομένα, η λειτουργία cache κεντρικού υπολογιστή έχει οριστεί σε **κανένα**.

* Εύρος ζώνης μέγιστο δικτύου είναι το μέγιστο συγκεντρωτική εύρος ζώνης έχει εκχωρηθεί και έχουν εκχωρηθεί ανά τύπο Εικονική. Το μέγιστο εύρος ζώνης παρέχει οδηγίες για την επιλογή τον σωστό τύπο Εικονική για να βεβαιωθείτε ότι χωρητικότητα επαρκής δικτύου είναι διαθέσιμη. Όταν μετακινείτε μεταξύ χαμηλό, Μέτριο, υψηλό και πολύ υψηλή, τη μετάδοση θα αυξήσει αντίστοιχα. Επιδόσεων δικτύου πραγματική θα εξαρτώνται από πολλούς παράγοντες, συμπεριλαμβανομένου του δικτύου και εφαρμογή φορτία και εφαρμογή ρυθμίσεων δικτύου.


## <a name="a-series"></a>Μια σειρά

| Μέγεθος        | CPU πυρήνων | Μνήμη: απομείωση | Τοπικό σκληρό ΔΊΣΚΟ: απομείωση | Max δίσκων δεδομένων | Max μεταγωγή δίσκου δεδομένων: IOP Προέλευσης | Max NIC / εύρος ζώνης δικτύου |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 / χαμηλή                   |
| Standard_A1 | 1         | 1,75         | 70                    | 2              | 2 x 500              | 1 / μέτρια              |
| Standard_A2 | 2         | 3,5 GB       | 135                   | 4              | 4 x 500              | 1 / Μέτριος              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / υψηλή                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 x 500             | 4 / υψηλή                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4 X 500              | 1 / μέτρια              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / υψηλή                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 x 500             | 4 / υψηλή                  |

## <a name="a-series---compute-intensive-instances"></a>A-σειρά - παρουσίες στενής υπολογισμού

Για πληροφορίες και ζητήματα σχετικά με τη χρήση αυτών των μεγεθών, ανατρέξτε στο θέμα [σχετικά με το H-σειρά και υπολογισμού στενής ΣΠΣ μια σειρά](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


| Μέγεθος         | CPU πυρήνων | Μνήμη: απομείωση | Τοπικό σκληρό ΔΊΣΚΟ: απομείωση | Max δίσκων δεδομένων | Max μεταγωγή δίσκου δεδομένων: IOP Προέλευσης | Max NIC / εύρος ζώνης δικτύου |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / υψηλή                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / πολύ υψηλή             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / υψηλή                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / πολύ υψηλή             |

* RDMA με δυνατότητα

## <a name="d-series"></a>Σειρά D


| Μέγεθος         | CPU πυρήνων | Μνήμη: απομείωση | Τοπική SSD: απομείωση | Max δίσκων δεδομένων | Max μεταγωγή δίσκου δεδομένων: IOP Προέλευσης | Max NIC / εύρος ζώνης δικτύου |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3,5          | 50                   | 2              | 2 x 500              | 1 / μέτρια              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / υψηλή                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / υψηλή                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / υψηλή                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / υψηλή                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / υψηλή                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / υψηλή                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / πολύ υψηλή             |

## <a name="dv2-series"></a>Dv2 σειρά

| Μέγεθος            | CPU πυρήνων | Μνήμη: απομείωση | Τοπική SSD: απομείωση | Max δίσκων δεδομένων | Max μεταγωγή δίσκου δεδομένων: IOP Προέλευσης | Max NIC / εύρος ζώνης δικτύου |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3,5          | 50                   | 2              | 2 x 500              | 1 / μέτρια              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / υψηλή                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / υψηλή                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / υψηλή                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32 x 500             | 8 / πολύ υψηλή        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / υψηλή                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / υψηλή                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / υψηλή                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / πολύ υψηλή        |
| Standard_D15_v2 | 20        | 140          | 1.000                | 40             | 40 x 500             | 8 / πολύ υψηλή        |

## <a name="g-series"></a>Σειρά G

| Μέγεθος        | CPU πυρήνων | Μνήμη: απομείωση  | Τοπική SSD: απομείωση  | Max δίσκων δεδομένων | Max δίσκου μετάδοσης: IOP Προέλευσης | Max NIC / εύρος ζώνης δικτύου |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4 x 500            | 1 / υψηλή                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / υψηλή                  |
| Standard_G3 | 8         | 112          | 1,536                | 16             | 16 x 500           | 4 / πολύ υψηλή             |
| Standard_G4 | 16        | 224          | 3,072                | 32             | 32 x 500           | 8 / πολύ υψηλή        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64 x 500           | 8 / πολύ υψηλή        |


## <a name="h-series"></a>H σειρά

Azure H σειρά εικονικές μηχανές είναι η επόμενη γενιάς υψηλές επιδόσεις υπολογιστική ΣΠΣ με στόχο την ανώτερης υπολογιστική τις ανάγκες, όπως μοριακή μοντελοποίηση και υπολογιστική dynamics δυναμικού. Αυτά τα 8 και 16 πυρήνα ΣΠΣ δημιουργούνται στην τεχνολογία επεξεργαστής Intel Haswell E5 2667 V3 που διαθέτει DDR4 μνήμη και τοπικού χώρου αποθήκευσης που βασίζεται SSD. 

Εκτός από τις ουσιαστικά ενέργειας CPU, η σειρά H προσφέρει διάφορων τύπων επιλογών χαμηλής λανθάνων χρόνος RDMA δικτύωσης χρησιμοποιώντας FDR InfiniBand και πολλές ρυθμίσεις παραμέτρων μνήμης για την υποστήριξη εντατική υπολογιστική απαιτήσεις μνήμης.


| Μέγεθος           | CPU πυρήνων | Μνήμη: απομείωση | Τοπική SSD: απομείωση | Max δίσκων δεδομένων | Max δίσκου μετάδοσης: IOP Προέλευσης | Max NIC / εύρος ζώνης δικτύου |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1000                     | 16             | 16 x 500                    | 8 / υψηλή                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / πολύ υψηλή                  |
| Standard_H8m   | 8         | 112         | 1000                     | 16             | 16 x 500                    | 8 / υψηλή                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / πολύ υψηλή                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / πολύ υψηλή                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / πολύ υψηλή                  |


* RDMA με δυνατότητα

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Σημειώσεις: Τυπική A0 - A4 χρησιμοποιώντας CLI και PowerShell 

Στο μοντέλο κλασική ανάπτυξης, ορισμένα ονόματα μέγεθος Εικονική είναι λίγο διαφορετικά στην CLI και PowerShell:

* Standard_A0 είναι ExtraSmall 
* Standard_A1 έχει μικρό μέγεθος
* Standard_A2 είναι το μεσαίο
* Standard_A3 είναι μεγάλο
* Standard_A4 είναι ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>Ρύθμιση παραμέτρων μεγεθών για τις υπηρεσίες Cloud

Μπορείτε να καθορίσετε το μέγεθος εικονική μηχανή μιας παρουσίας ρόλο ως μέρος του μοντέλου υπηρεσίας που περιγράφεται από το [αρχείο ορισμού υπηρεσίας](cloud-services-model-and-package.md#csdef). Το μέγεθος του ρόλου καθορίζει τον αριθμό των CPU πυρήνων χωρητικότητα μνήμης και το μέγεθος του συστήματος τοπικού αρχείου που έχει εκχωρηθεί σε μια παρουσία που εκτελείται. Επιλέξτε το ρόλο μέγεθος με βάση την απαίτηση πόρων της εφαρμογής σας.

Ακολουθεί ένα παράδειγμα για τη ρύθμιση του μεγέθους ρόλο για να [Standard_D2](#general-purpose-d) για μια παρουσία ρόλου Web:

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε για [τη συνδρομή azure και όρια υπηρεσίας, ορίων, και τους περιορισμούς](../azure-subscription-service-limits.md).
- Μάθετε περισσότερα [σχετικά με το H-σειρά και υπολογισμού στενής ΣΠΣ μια σειρά](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) για φόρτους εργασίας όπως υψηλών επιδόσεων υπολογιστική (HPC).
