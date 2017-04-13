<properties
   pageTitle="Επισκόπηση του χώρου αποθήκευσης λίμνης δεδομένων Azure | Microsoft Azure"
   description="Κατανόηση των τι είναι το χώρο αποθήκευσης λίμνης δεδομένων Azure και την τιμή παρέχει επάνω από άλλα χώροι αποθήκευσης δεδομένων"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Επισκόπηση του χώρου αποθήκευσης λίμνης δεδομένων Azure

Χώρος αποθήκευσης λίμνης Azure δεδομένων είναι ένα αποθετήριο hyper κλίμακας εταιρικές για μεγάλο δεδομένων αναλυτικό φόρτους εργασίας. Azure λίμνης δεδομένων σάς επιτρέπει να καταγράψετε δεδομένα από οποιαδήποτε ταχύτητα μέγεθος, τον τύπο και κατάποσης σε ένα μόνο σημείο για λειτουργικές και διερευνητικές ανάλυση.

> [AZURE.TIP] Χρήση του [χώρου αποθήκευσης δεδομένων λίμνης διαδρομή εκμάθησης](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) για να ξεκινήσετε την Εξερεύνηση της υπηρεσίας Azure δεδομένων λίμνης αποθήκευσης.

Χώρος αποθήκευσης λίμνης Azure δεδομένων είναι δυνατή η πρόσβαση από Hadoop (διαθέσιμη με σύμπλεγμα HDInsight) χρησιμοποιώντας τα API ΥΠΌΛΟΙΠΑ συμβατό με WebHDFS. Έχει σχεδιαστεί ειδικά για να ενεργοποιήσετε την ανάλυση σε τα δεδομένα που έχουν αποθηκευτεί μαζί με ρυθμίζεται για επιδόσεων για σενάρια ανάλυση δεδομένων. Έξοδος από το πλαίσιο, περιλαμβάνει όλες τις δυνατότητες επίπεδου μεγάλης εταιρείας — ασφαλείας, διαχειρισιμότητα, κλιμάκωση, αξιοπιστία και διαθεσιμότητα — απαραίτητες για μεγάλες επιχειρήσεις ρεαλιστικό περιπτώσεις χρήσης.


![Azure δεδομένων λίμνης](./media/data-lake-store-overview/data-lake-store-concept.png)

Ορισμένες από τις βασικές δυνατότητες του το Azure λίμνης δεδομένων περιλαμβάνουν τα εξής.

### <a name="built-for-hadoop"></a>Έχει σχεδιαστεί για Hadoop

Ο χώρος αποθήκευσης Azure λίμνης δεδομένων είναι ένα σύστημα αρχείων Apache Hadoop συμβατά με το σύστημα αρχείων κατανεμημένο Hadoop (HDFS) και λειτουργεί με το περιβάλλον εμπορικής προσαρμογής Hadoop.  Τα υπάρχοντα HDInsight εφαρμογές ή τις υπηρεσίες που χρησιμοποιούν το API WebHDFS εύκολα να ενσωματώσετε με το χώρο αποθήκευσης λίμνης δεδομένων. Χώρος αποθήκευσης δεδομένων λίμνης εκθέτει επίσης ένα περιβάλλον εργασίας συμβατό με WebHDFS ΥΠΌΛΟΙΠΟ για εφαρμογές

Δεδομένα που είναι αποθηκευμένα στο χώρο αποθήκευσης λίμνης δεδομένων μπορείτε να αναλύσετε εύκολα χρησιμοποιώντας Hadoop αναλυτικό πλαίσια όπως MapReduce ή η ομάδα. Microsoft Azure HDInsight συμπλεγμάτων μπορεί να παρασχεθεί και να ρυθμίσει τις παραμέτρους για απευθείας πρόσβαση σε δεδομένα που είναι αποθηκευμένα στο χώρο αποθήκευσης λίμνης δεδομένων.

### <a name="unlimited-storage-petabyte-files"></a>Απεριόριστος χώρος αποθήκευσης, petabyte αρχείων

Χώρος αποθήκευσης λίμνης δεδομένων Azure παρέχει απεριόριστο χώρο αποθήκευσης και είναι κατάλληλη για την αποθήκευση μια ποικιλία δεδομένων για ανάλυση. Δεν επιβάλλει οποιαδήποτε όρια μεγέθους λογαριασμού, μεγέθη αρχείων ή την ποσότητα των δεδομένων που μπορούν να αποθηκευτούν σε ένα λίμνης δεδομένων. Μεμονωμένων αρχείων μπορεί να είναι από kilobyte έως petabytes μέγεθος καθιστώντας ιδανική επιλογή για να αποθηκεύσετε οποιονδήποτε τύπο δεδομένων. Τα δεδομένα αποθηκεύονται κατάταξή, καθιστώντας πολλών αντιγράφων και δεν υπάρχει περιορισμός στη διάρκεια του χρόνου για την οποία μπορούν να αποθηκευτούν τα δεδομένα σε λίμνης τα δεδομένα.

### <a name="performance-tuned-for-big-data-analytics"></a>Απόδοση ρυθμισμένη για ανάλυση δεδομένων μεγάλο

Χώρος αποθήκευσης λίμνης Azure δεδομένων είναι ενσωματωμένη για την εκτέλεση μεγάλης κλίμακας αναλυτικό συστήματα που απαιτούν μαζικών μετάδοσης στο ερώτημα και να αναλύσετε μεγάλες ποσότητες δεδομένων. Τα δεδομένα λίμνης δισέλιδα τμήματα ενός αρχείου πάνω από έναν αριθμό των διακομιστών μεμονωμένα χώρου αποθήκευσης. Αυτό βελτιώνει την ταχύτητα μεταγωγής ανάγνωσης κατά την ανάγνωση του αρχείου παράλληλα για την εκτέλεση ανάλυσης δεδομένων.


### <a name="enterprise-ready-highly-available-and-secure"></a>Ετοιμότητα για μεγάλες επιχειρήσεις: Ιδιαίτερα-διαθέσιμα και ασφαλή

Χώρος αποθήκευσης λίμνης δεδομένων Azure παρέχει βιομηχανικών προτύπων διαθεσιμότητα και την αξιοπιστία. Τα δεδομένα της εταιρείας σας αποθηκεύονται κατάταξή δημιουργώντας πλεοναζόντων αντίγραφα για να προστατευτείτε από τυχόν μη αναμενόμενο αποτυχίες. Επιχειρήσεις να χρησιμοποιήσετε λίμνης δεδομένων Azure στο λύσεις τους ως ένα σημαντικό μέρος της πλατφόρμας τα υπάρχοντα δεδομένα.

Χώρος αποθήκευσης δεδομένων λίμνης παρέχει επίσης επίπεδου μεγάλης εταιρείας ασφαλείας για τα δεδομένα που έχουν αποθηκευτεί. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ασφαλή δεδομένα στο χώρο αποθήκευσης λίμνης Azure δεδομένων](#DataLakeStoreSecurity).


### <a name="all-data"></a>Όλα τα δεδομένα

Χώρος αποθήκευσης λίμνης δεδομένων Azure μπορούν να αποθηκεύουν δεδομένα στην αρχική τους μορφή, όπως είναι, χωρίς να απαιτείται οποιαδήποτε μετασχηματισμοί εκ των προτέρων. Χώρος αποθήκευσης δεδομένων λίμνης δεν απαιτεί ένα σχήμα που καθορίζονται πριν από την φόρτωση των δεδομένων, διατηρώντας το έως και μεμονωμένες αναλυτικό πλαίσιο να ερμηνεύσετε τα δεδομένα και να ορίσετε ένα σχήμα τη στιγμή της ανάλυσης. Είναι δυνατή η αποθήκευση αρχείων αυθαίρετο μεγεθών και μορφές δίνει τη δυνατότητα για το χώρο αποθήκευσης λίμνης δεδομένων για το χειρισμό δομημένα, ημι-δομημένα και μη δομημένα δεδομένα.

Τα κοντέινερ χώρου αποθήκευσης λίμνης Azure δεδομένων για τα δεδομένα είναι ουσιαστικά φακέλων και αρχείων. Λειτουργία σε την αποθηκευμένη δεδομένα χρησιμοποιώντας το SDK, πύλης Azure και Azure Powershell. Με την προϋπόθεση ότι μπορείτε να τοποθετήσετε τα δεδομένα σας στο κατάστημα χρήσης αυτών των διασυνδέσεων και χρησιμοποιώντας το κατάλληλο κοντέινερ, μπορείτε να αποθηκεύσετε οποιονδήποτε τύπο δεδομένων. Χώρος αποθήκευσης δεδομένων λίμνης δεν εκτελεί οποιαδήποτε ειδικό χειρισμό των δεδομένων με βάση τον τύπο των δεδομένων που αποθηκεύει.


## <a name="DataLakeStoreSecurity"></a>Ασφάλεια των δεδομένων στο χώρο αποθήκευσης λίμνης δεδομένων Azure

Χώρος αποθήκευσης λίμνης δεδομένων Azure χρησιμοποιεί Azure Active Directory για τον έλεγχο ταυτότητας και λίστες ελέγχου πρόσβασης (ACL) για να διαχειριστείτε την πρόσβαση στα δεδομένα σας.

| Η δυνατότητα                                 | Περιγραφή                              |
|-----------------------------------------|------------------------------------------|
| Έλεγχος ταυτότητας | Χώρος αποθήκευσης λίμνης δεδομένων Azure ενοποιείται με το Azure Active Directory (AAD) για τη Διαχείριση ταυτότητας και την πρόσβαση για όλα τα δεδομένα που είναι αποθηκευμένα στο χώρο αποθήκευσης λίμνης Azure δεδομένων. Ως αποτέλεσμα την ενοποίηση, λίμνης δεδομένων Azure πλεονεκτήματα από όλες τις δυνατότητες AAD όπως έλεγχο ταυτότητας πολλών παραγόντων, υπό όρους πρόσβασης, έλεγχος πρόσβασης βάσει ρόλων, παρακολούθηση χρήσης εφαρμογής, ασφαλείας, παρακολούθηση και την ειδοποίηση, κ.λπ. Χώρος αποθήκευσης λίμνης δεδομένων Azure υποστηρίζει το πρωτόκολλο OAuth 2.0 για έλεγχο ταυτότητας με στο περιβάλλον εργασίας ΥΠΌΛΟΙΠΟ. |
| Έλεγχος πρόσβασης                          | Χώρος αποθήκευσης λίμνης δεδομένων Azure παρέχει έλεγχο πρόσβασης με την υποστήριξη POSIX στυλ δικαιώματα που εκτίθεται από το πρωτόκολλο WebHDFS. Στην τρέχουσα έκδοση, μπορεί να ενεργοποιηθεί ACL σε το ριζικό φάκελο, των υποφακέλων, καθώς και μεμονωμένων αρχείων. Οι ACL που ισχύουν για τον ριζικό φάκελο θα ισχύουν επίσης για όλα τα θυγατρικών φακέλων/αρχεία καθώς και.|

Θέλετε να μάθετε περισσότερα σχετικά με την προστασία των δεδομένων στο χώρο αποθήκευσης λίμνης δεδομένων. Ακολουθήστε τις παρακάτω συνδέσεις.

* Για οδηγίες σχετικά με τον τρόπο ασφαλή δεδομένα στο χώρο αποθήκευσης λίμνης δεδομένων, ανατρέξτε στο θέμα [ασφαλή δεδομένα στο χώρο αποθήκευσης λίμνης Azure δεδομένων](data-lake-store-secure-data.md).
* Προτιμάτε βίντεο; [Παρακολουθήστε αυτό το βίντεο](https://mix.office.com/watch/1q2mgzh9nn5lx) σχετικά με τον τρόπο για την ασφάλιση δεδομένα που είναι αποθηκευμένα στο χώρο αποθήκευσης λίμνης δεδομένων.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Εφαρμογές που είναι συμβατά με το χώρο αποθήκευσης λίμνης δεδομένων Azure

Χώρος αποθήκευσης λίμνης δεδομένων Azure είναι συμβατά με πιο ανοιχτή στοιχεία προέλευσης στο στο περιβάλλον εμπορικής προσαρμογής Hadoop. Επίσης Ενοποιείται όμορφα με άλλες υπηρεσίες του Azure. Με αυτόν τον τρόπο αποθήκευσης δεδομένων λίμνης μια ιδανική επιλογή για τις ανάγκες σας χώρου αποθήκευσης δεδομένων. Ακολουθήστε τις παρακάτω για να μάθετε περισσότερα σχετικά με τον τρόπο χώρου αποθήκευσης λίμνης δεδομένων μπορεί να χρησιμοποιηθεί με στοιχεία Άνοιγμα αρχείου προέλευσης, καθώς και άλλες υπηρεσίες του Azure τόσο συνδέσεις.

* Ανατρέξτε στο θέμα [εφαρμογές και υπηρεσίες που είναι συμβατά με το χώρο αποθήκευσης λίμνης Azure δεδομένων](data-lake-store-compatible-oss-other-applications.md) για μια λίστα με τις εφαρμογές Άνοιγμα αρχείου προέλευσης διαλειτουργικό με χώρου αποθήκευσης δεδομένων λίμνης.
* Ανατρέξτε στο θέμα [ολοκλήρωση με άλλες υπηρεσίες του Azure](data-lake-store-integrate-with-other-services.md) για να κατανοήσετε τον τρόπο αποθήκευσης λίμνης δεδομένων μπορεί να χρησιμοποιηθεί με άλλες υπηρεσίες του Azure για να ενεργοποιήσετε ένα ευρύτερο πλήθος σενάρια.
* Ανατρέξτε στο θέμα [σενάρια για τη χρήση του χώρου αποθήκευσης δεδομένων λίμνης](data-lake-store-data-scenarios.md) για να μάθετε τον τρόπο χρήσης του χώρου αποθήκευσης δεδομένων λίμνης σε σενάρια όπως ingesting δεδομένων, επεξεργασία δεδομένων, τη λήψη δεδομένων και οπτικοποίηση δεδομένων.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Τι είναι το σύστημα αρχείων χώρου αποθήκευσης λίμνης δεδομένων Azure (adl: / /);

Χώρος αποθήκευσης δεδομένων λίμνης είναι δυνατή η πρόσβαση μέσω του νέου συστήματος αρχείων, το AzureDataLakeFilesystem (adl: / /), στα περιβάλλοντα Hadoop (διαθέσιμη με σύμπλεγμα HDInsight). Εφαρμογές και υπηρεσίες που χρησιμοποιούν adl: / / μπορούν να επωφεληθείτε από περαιτέρω βελτιστοποίηση των επιδόσεων που δεν είναι ακόμη διαθέσιμες στο WebHDFS. Ως αποτέλεσμα, χώρος αποθήκευσης δεδομένων λίμνης σάς παρέχει την ευελιξία να κάνει είτε τις καλύτερες επιδόσεις με την προτεινόμενη επιλογή χρήσης adl: / / ή τη διατήρηση υπάρχοντα κωδικό Συνεχίζοντας να χρησιμοποιούν το API WebHDFS απευθείας. Azure HDInsight αξιοποιεί πλήρως το AzureDataLakeFilesystem να παρέχετε τις καλύτερες επιδόσεις στην χώρου αποθήκευσης λίμνης δεδομένων.

Μπορείτε να αποκτήσετε πρόσβαση σε δεδομένα σας με το χώρο αποθήκευσης λίμνης δεδομένων με χρήση `adl://<data_lake_store_name>.azuredatalakestore.net`. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να αποκτήσετε πρόσβαση στα δεδομένα στο χώρο αποθήκευσης λίμνης δεδομένων, ανατρέξτε στο θέμα [Προβολή ιδιοτήτων των αποθηκευμένων δεδομένων](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Πώς μπορώ να ξεκινήσω με χρήση του χώρου αποθήκευσης λίμνης δεδομένων Azure;

Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης λίμνης δεδομένων με την πύλη Azure](data-lake-store-get-started-portal.md), σχετικά με την παροχή ενός χώρου αποθήκευσης λίμνης δεδομένων με την πύλη Azure. Αφού έχουν παρασχεθεί Azure λίμνης δεδομένων, μπορείτε να μάθετε πώς μπορείτε να χρησιμοποιήσετε προσφορές μεγάλο δεδομένων όπως ανάλυση λίμνης δεδομένων Azure ή Azure HDInsight με το χώρο αποθήκευσης λίμνης δεδομένων. Μπορείτε επίσης να δημιουργήσετε μια εφαρμογή .NET για τη δημιουργία λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων Azure και εκτελούν λειτουργίες όπως η αποστολή δεδομένων, κάντε λήψη δεδομένων, κ.λπ.

- [Γρήγορα αποτελέσματα με το Azure δεδομένων λίμνης ανάλυσης](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Χρήση Azure HDInsight με το χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Γρήγορα αποτελέσματα με χρήση του .NET SDK χώρο αποθήκευσης λίμνης δεδομένων Azure](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>Χώρος αποθήκευσης δεδομένων λίμνης βίντεο

Εάν προτιμάτε παρακολουθώντας βίντεο για να μάθετε, χώρος αποθήκευσης δεδομένων λίμνης παρέχει βίντεο σε μια περιοχή δυνατότητες.

* [Δημιουργία λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων Azure](https://mix.office.com/watch/1k1cycy4l4gen)
* [Χρησιμοποιήστε την Εξερεύνηση δεδομένων για τη διαχείριση δεδομένων στο χώρο αποθήκευσης λίμνης δεδομένων Azure](https://mix.office.com/watch/icletrxrh6pc)
* [Σύνδεση δεδομένων Azure λίμνης ανάλυσης στο χώρο αποθήκευσης λίμνης δεδομένων Azure](https://mix.office.com/watch/qwji0dc9rx9k)
* [Πρόσβαση Azure λίμνης χώρου αποθήκευσης δεδομένων μέσω ανάλυσης δεδομένων λίμνης](https://mix.office.com/watch/1n0s45up381a8)
* [Σύνδεση Azure HDInsight στο χώρο αποθήκευσης λίμνης δεδομένων Azure](https://mix.office.com/watch/l93xri2yhtp2)
* [Πρόσβαση Azure λίμνης χώρου αποθήκευσης δεδομένων μέσω της ομάδας και γουρούνι](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Χρήση DistCp (Hadoop κατανεμημένης αντίγραφο) για να αντιγράψετε δεδομένα προς και από το χώρο αποθήκευσης λίμνης δεδομένων Azure](https://mix.office.com/watch/1liuojvdx6sie)
* [Χρήση Apache Sqoop για μετακίνηση δεδομένων μεταξύ σχεσιακές προελεύσεις και χώρου αποθήκευσης λίμνης δεδομένων Azure](https://mix.office.com/watch/1butcdjxmu114)
* [Ενορχήστρωσης δεδομένων με χρήση εργοστασίου δεδομένων Azure για το χώρο αποθήκευσης λίμνης δεδομένων Azure](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Ασφάλεια των δεδομένων στο χώρο αποθήκευσης λίμνης δεδομένων Azure](https://mix.office.com/watch/1q2mgzh9nn5lx)


