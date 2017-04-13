<properties
    pageTitle="Δημιουργία Hadoop, HBase ή καταιγίδας συμπλεγμάτων σε Linux στο χρησιμοποιώντας το πλατφόρμες CLI Azure HDInsight | Microsoft Azure"
    description="Μάθετε πώς να δημιουργείτε χρησιμοποιώντας το CLI Azure πλατφόρμες, πρότυπα διαχείρισης πόρων Azure και το Azure REST API συμπλεγμάτων βάσει Linux HDInsight. Μπορείτε να καθορίσετε τον τύπο συμπλέγματος (Hadoop, HBase ή καταιγίδας) ή να χρησιμοποιήσετε δέσμες ενεργειών για την εγκατάσταση προσαρμοσμένα στοιχεία..."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>Δημιουργία βάσει Linux συμπλεγμάτων στο χρησιμοποιώντας το CLI Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Το Azure CLI είναι μια πλατφόρμα βοηθητικό πρόγραμμα γραμμής εντολών που σας επιτρέπει να διαχειριστείτε τις υπηρεσίες Azure. Μπορεί να χρησιμοποιηθεί, μαζί με τα πρότυπα διαχείρισης πόρων Azure, για να δημιουργήσετε ένα σύμπλεγμα HDInsight, μαζί με λογαριασμούς συσχετισμένη χώρου αποθήκευσης και άλλες υπηρεσίες.

Azure πρότυπα διαχείρισης πόρων είναι JSON έγγραφα που περιγράφουν μια __ομάδα πόρων__ και όλους τους πόρους του (όπως το HDInsight.) Αυτή η προσέγγιση με βάση το πρότυπο σάς επιτρέπει να ορίσετε όλους τους πόρους που χρειάζεστε για HDInsight σε ένα πρότυπο. Αυτό σας επιτρέπει επίσης να διαχειριστείτε τις αλλαγές στην ομάδα ως σύνολο μέσω __αναπτύξεις__, που έχουν εφαρμογή αλλαγών σε ολόκληρη την ομάδα.

Τα βήματα σε αυτό το έγγραφο θα καθοδηγήσουν στη διαδικασία δημιουργίας ενός νέου συμπλέγματος HDInsight χρησιμοποιώντας το CLI Azure και ένα πρότυπο.

> [AZURE.IMPORTANT] Τα βήματα σε αυτό το έγγραφο χρησιμοποιούν ο προεπιλεγμένος αριθμός εργαζόμενου κόμβους (4) για ένα σύμπλεγμα HDInsight. Εάν σχεδιάζετε σε περισσότερους από 32 κόμβους εργαζόμενου (κατά τη δημιουργία συμπλέγματος ή την κλίμακα του συμπλέγματος), στη συνέχεια, πρέπει να επιλέξετε ένα μέγεθος κεφαλής κόμβο με τουλάχιστον 8 πυρήνων και 14 GB ram.
>
> Για περισσότερες πληροφορίες σχετικά με μεγέθη κόμβο και σχετικές κόστος, ανατρέξτε στο θέμα [HDInsight τις τιμές](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Azure CLI__. Τα βήματα σε αυτό το έγγραφο έχουν τελευταία δοκιμή με Azure CLI έκδοση 0.10.1.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>Απαιτήσεις για στοιχείο ελέγχου πρόσβασης

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>Συνδεθείτε με τη συνδρομή σας στο Azure

Ακολουθήστε τα βήματα που τεκμηριώνονται σε [σύνδεση με μια συνδρομή του Azure από το περιβάλλον γραμμής εντολών Azure (Azure CLI)](../xplat-cli-connect.md) και να συνδεθείτε με τη συνδρομή σας χρησιμοποιώντας τη μέθοδο __σύνδεσης__ .

##<a name="create-a-cluster"></a>Δημιουργήστε ένα σύμπλεγμα

Ακολουθήστε τα παρακάτω βήματα πρέπει να εκτελεστούν από μια γραμμή εντολών, κελύφους ή περιόδου λειτουργίας τερματικού μετά την εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI.

1. Χρησιμοποιήστε την ακόλουθη εντολή για τον έλεγχο ταυτότητας Azure τη συνδρομή σας:

        azure login

    Θα σας ζητηθεί να δώσετε το όνομα και τον κωδικό πρόσβασης. Εάν έχετε πολλές συνδρομές Azure, χρησιμοποιήστε `azure account set <subscriptionname>` για να ορίσετε τη συνδρομή που χρησιμοποιούν τις εντολές Azure CLI.

3. Μεταβείτε σε λειτουργία διαχείρισης πόρων Azure χρησιμοποιώντας την ακόλουθη εντολή:

        azure config mode arm

4. Δημιουργία μιας ομάδας πόρων. Αυτήν την ομάδα πόρων θα περιέχει το HDInsight σύμπλεγμα και σχετικές λογαριασμού χώρου αποθήκευσης.

        azure group create groupname location
        
    * Αντικαταστήστε το __όνομα ομάδας__ με ένα μοναδικό όνομα για την ομάδα. 
    * Αντικαταστήστε __θέση__ με τη γεωγραφική περιοχή που θέλετε να δημιουργήσετε την ομάδα στην. 
    
        Για μια λίστα με έγκυρες θέσεις, χρησιμοποιήστε το `azure location list` εντολή και, στη συνέχεια, χρησιμοποιήστε μία από τις θέσεις από τη στήλη " __όνομα__ ".

5. Δημιουργία λογαριασμού χώρου αποθήκευσης. Αυτόν το λογαριασμό χώρου αποθήκευσης θα χρησιμοποιηθεί ως το προεπιλεγμένο αποθήκευσης για το σύμπλεγμα HDInsight.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * Αντικαταστήστε το __όνομα ομάδας__ με το όνομα της ομάδας που δημιουργήσατε στο προηγούμενο βήμα.
     * Αντικαταστήστε __θέση__ με την ίδια θέση που χρησιμοποιούνται στο προηγούμενο βήμα. 
     * Αντικαταστήστε __storagename__ με ένα μοναδικό όνομα για το λογαριασμό χώρου αποθήκευσης.
     
     > [AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με τις παραμέτρους που χρησιμοποιούνται σε αυτήν την εντολή, χρησιμοποιήστε το `azure storage account create -h` για να προβάλετε τη Βοήθεια για αυτήν την εντολή.

5. Ανακτήστε το κλειδί που χρησιμοποιείται για να αποκτήσετε πρόσβαση στο λογαριασμό του χώρου αποθήκευσης.

        azure storage account keys list -g groupname storagename
        
    * Αντικαταστήστε το __όνομα ομάδας__ με το όνομα της ομάδας πόρων.
    * Αντικατάσταση __storagename__ με το όνομα του λογαριασμού χώρου αποθήκευσης.
    
    Στα δεδομένα που επιστρέφονται, αποθηκεύστε την τιμή __αριθμού-κλειδιού__ για __κλειδί1__.

6. Δημιουργήστε ένα σύμπλεγμα HDInsight.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Αντικαταστήστε το __όνομα ομάδας__ με το όνομα της ομάδας πόρων.

    * Αντικαταστήστε __Hadoop__ με τον τύπο σύμπλεγμα που θέλετε να δημιουργήσετε. Για παράδειγμα, `Hadoop`, `HBase`, `Storm` ή `Spark`.

        > [AZURE.IMPORTANT] HDInsight συμπλεγμάτων παραδίδεται με διάφορους τύπους, οι οποίοι αντιστοιχούν σε το φόρτο εργασίας ή την τεχνολογία που ρυθμίζεται για το σύμπλεγμα. Δεν υπάρχει υποστηριζόμενη μέθοδος για να δημιουργήσετε ένα σύμπλεγμα που συνδυάζει πολλούς τύπους, όπως καταιγίδας και HBase σε ένα σύμπλεγμα. 

    * Αντικαταστήστε __θέση__ με την ίδια θέση που χρησιμοποιείται σε προηγούμενα βήματα.

    * Αντικαταστήστε __storagename__ με το όνομα λογαριασμού του χώρου αποθήκευσης.

    * Αντικαταστήστε __ιδιότητα storagekey__ με τον αριθμό-κλειδί που λαμβάνονται στο προηγούμενο βήμα. 

    * Για το `--defaultStorageContainer` παράμετρο, χρησιμοποιήστε το ίδιο όνομα με που χρησιμοποιείτε για το σύμπλεγμα.

    * Αντικατάσταση __διαχείρισης__ και __httppassword__ με το όνομα και τον κωδικό πρόσβασης που θέλετε να χρησιμοποιήσετε κατά την πρόσβαση στο σύμπλεγμα μέσω HTTPS.

    * Αντικατάσταση __sshuser__ και __sshuserpassword__ με το όνομα χρήστη και τον κωδικό πρόσβασης που θέλετε να χρησιμοποιήσετε κατά την πρόσβαση στο σύμπλεγμα χρησιμοποιώντας SSH

    Ενδέχεται να χρειαστούν αρκετά λεπτά για να ολοκληρωθεί η διαδικασία δημιουργίας σύμπλεγμα. Συνήθως είναι περίπου 15.

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει με επιτυχία ένα σύμπλεγμα HDInsight με το Azure CLI, χρησιμοποιήστε τα ακόλουθα για να μάθετε πώς μπορείτε να εργαστείτε με το σύμπλεγμά σας:

###<a name="hadoop-clusters"></a>Hadoop συμπλεγμάτων

* [Χρήση της ομάδας με το HDInsight](hdinsight-use-hive.md)
* [Χρήση γουρούνι με HDInsight](hdinsight-use-pig.md)
* [Χρήση MapReduce με HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase συμπλεγμάτων

* [Γρήγορα αποτελέσματα με το HBase σε HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Ανάπτυξη εφαρμογών Java για HBase σε HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Καταιγίδας συμπλεγμάτων

* [Ανάπτυξη τοπολογίες Java για καταιγίδας στην HDInsight](hdinsight-storm-develop-java-topology.md)
* [Χρησιμοποιήστε τα στοιχεία Python στο καταιγίδας στην HDInsight](hdinsight-storm-develop-python-topology.md)
* [Ανάπτυξη και εποπτεία τοπολογίες με καταιγίδας στην HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
