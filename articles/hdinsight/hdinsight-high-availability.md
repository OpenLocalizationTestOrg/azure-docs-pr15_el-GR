<properties
    pageTitle="Διαθεσιμότητα Hadoop συμπλεγμάτων στο HDInsight | Microsoft Azure"
    description="HDInsight αναπτύσσει ιδιαίτερα διαθέσιμες και αξιόπιστη συμπλεγμάτων με ένα πρόσθετο κεφαλής κόμβο."
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Διαθεσιμότητα και την αξιοπιστία συμπλεγμάτων που βασίζεται σε Windows Hadoop στο HDInsight


>[AZURE.NOTE] Τα βήματα που χρησιμοποιούνται σε αυτό το έγγραφο είναι συγκεκριμένες για συμπλεγμάτων HDInsight που βασίζεται στα Windows. Εάν χρησιμοποιείτε ένα σύμπλεγμα βάσει Linux, ανατρέξτε στο θέμα [διαθεσιμότητα και την αξιοπιστία των συμπλεγμάτων βάσει Linux Hadoop στο HDInsight](hdinsight-high-availability-linux.md) για πληροφορίες σχετικά με Linux.

HDInsight επιτρέπει στους πελάτες να αναπτύξετε μια ποικιλία τύπων σύμπλεγμα, για διαφορετικά δεδομένα φόρτους εργασίας ανάλυσης. Τύποι σύμπλεγμα που παρέχεται σήμερα είναι Hadoop συμπλεγμάτων για ερωτήματος και ανάλυση φόρτους εργασίας, HBase συμπλεγμάτων για NoSQL φόρτους εργασίας και καταιγίδας συμπλεγμάτων για επεξεργασία συμβάντος πραγματικό χρόνο φόρτους εργασίας. Μέσα σε έναν τύπο που δίνεται σύμπλεγμα, υπάρχουν διαφορετικούς ρόλους για τους διάφορους κόμβους. Για παράδειγμα:



- Hadoop συμπλεγμάτων για HDInsight αναπτύσσονται με δύο ρόλους:
    - Προϊστάμενος κόμβου (2 κόμβοι)
    - Δεδομένα κόμβο (τουλάχιστον 1 κόμβο)

- HBase συμπλεγμάτων για HDInsight αναπτύσσονται με τρεις ρόλους:
    - Οι διακομιστές κεφαλής (2 κόμβοι)
    - Περιοχή διακομιστές (τουλάχιστον 1 κόμβο)
    - Υπόδειγμα/Zookeeper κόμβους (3 κόμβοι)

- Καταιγίδας συμπλεγμάτων για HDInsight αναπτύσσονται με τρεις ρόλους:
    - Nimbus κόμβους (2 κόμβοι)
    - Οι διακομιστές επόπτη (τουλάχιστον 1 κόμβο)
    - Zookeeper κόμβους (3 κόμβοι)

Τυπική υλοποιήσεις συμπλεγμάτων Hadoop συνήθως έχουν έναν μεμονωμένο κεφαλής κόμβο. HDInsight καταργεί το μοναδικό σημείο αποτυχίας με την προσθήκη ενός/head δευτερεύοντα κόμβο κεφαλής κόμβο διακομιστή/Nimbus για να αυξήσετε τη διαθεσιμότητα και την αξιοπιστία των την υπηρεσία που απαιτείται για τη Διαχείριση φόρτους εργασίας. Αυτές οι κόμβοι διακομιστές/Nimbus κεφαλής κόμβοι/κεφαλή έχουν σχεδιαστεί για να διαχειριστείτε την αποτυχία της εργασίας κόμβοι ομαλά, αλλά οποιαδήποτε ΡΕΥΜΑΤΟΣ της κύριας υπηρεσίες που εκτελούνται στον κόμβο κεφαλής θα μπορούσε να προκαλέσει το σύμπλεγμα παύει να λειτουργεί.


[ZooKeeper](http://zookeeper.apache.org/ ) κόμβους (ZKs) που έχουν προστεθεί και χρησιμοποιούνται για τις εκλογές επικεφαλής του κεφαλής κόμβους και προς ασφάλιση τον εργαζόμενο κόμβους και των πυλών (GWs) πότε πρέπει να ανακατευθύνει στον δευτερεύοντα κόμβο κεφαλής (κεφαλής Node1) όταν γίνεται ανενεργός τον ενεργό κόμβο κεφαλής (κεφαλής Node0).

![Διάγραμμα ιδιαίτερα αξιόπιστη κεφαλής κόμβο κατά την εφαρμογή HDInsight Hadoop.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>Ελέγξτε την κατάσταση υπηρεσίας ενεργό κόμβο κεφαλής
Για να προσδιορίσετε ποια κεφαλής κόμβου είναι ενεργή και να ελέγξετε την κατάσταση από τις υπηρεσίες που εκτελούνται σε αυτόν τον κόμβο κεφαλής, πρέπει να συνδεθείτε με το σύμπλεγμα Hadoop, χρησιμοποιώντας το πρωτόκολλο απομακρυσμένης επιφάνειας εργασίας (RDP). Για τις RDP οδηγίες, ανατρέξτε στο θέμα [Διαχείριση Hadoop συμπλεγμάτων στο HDInsight, χρησιμοποιώντας την πύλη του Azure](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp). Όταν έχετε συνδέσει στο σύμπλεγμα, κάντε διπλό κλικ στο εικονίδιο **Διαθέσιμη υπηρεσία Hadoop** που βρίσκεται στην επιφάνεια εργασίας για να αποκτήσετε κατάστασης σχετικά με ποια κεφαλής κόμβο του Namenode, Jobtracker, Templeton, Oozieservice, Metastore και Hiveserver2 υπηρεσίες είναι χρησιμοποιείτε, ή για τις υπηρεσίες HDI 3.0, Namenode, διαχείριση πόρων, ιστορικό διακομιστή, Templeton, Oozieservice, Metastore και Hiveserver2.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

Στο στιγμιότυπο οθόνης, το ενεργό κόμβο κεφαλής είναι *headnode0*.

## <a name="access-log-files-on-the-secondary-head-node"></a>Πρόσβαση σε αρχεία καταγραφής στον δευτερεύοντα κόμβο κεφαλής

Για να αποκτήσετε πρόσβαση σε εργασία αρχεία καταγραφής στον δευτερεύοντα κόμβο κεφαλίδας σε περίπτωση που έχει καταστεί τον ενεργό κόμβο κεφαλής, περιήγηση στο περιβάλλον εργασίας Χρήστη του JobTracker εξακολουθεί να λειτουργεί όπως και για τον κύριο ενεργό κόμβο. Για να αποκτήσετε πρόσβαση JobTracker, πρέπει να συνδεθείτε στο σύμπλεγμα Hadoop, χρησιμοποιώντας RDP, όπως περιγράφεται στην προηγούμενη ενότητα. Όταν έχετε απομακρυσμένη πρόσβαση στο σύμπλεγμα, κάντε διπλό κλικ στο εικονίδιο της **Κατάστασης του κόμβου όνομα Hadoop** που βρίσκεται στην επιφάνεια εργασίας και, στη συνέχεια, κάντε κλικ στην επιλογή τα **αρχεία καταγραφής NameNode** για να μεταβείτε στον κατάλογο των αρχείων καταγραφής στον δευτερεύοντα κεφαλής κόμβο.

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>Ρύθμιση παραμέτρων μεγέθους κεφαλής κόμβου
Οι κόμβοι κεφαλής εκχωρούνται ως μεγάλο εικονικές μηχανές (ΣΠΣ) από προεπιλογή. Αυτό το μέγεθος είναι κατάλληλες για τη διαχείριση των περισσότερες εργασίες Hadoop εκτέλεση στο σύμπλεγμα. Αλλά υπάρχουν σενάρια που μπορεί να απαιτεί πολύ μεγάλες ΣΠΣ για τους κόμβους κεφαλίδας. Ένα παράδειγμα είναι όταν το σύμπλεγμα έχει για τη διαχείριση μεγάλου αριθμού μικρές Oozie εργασίες.

Πολύ μεγάλες ΣΠΣ μπορούν να ρυθμιστούν με τη χρήση των cmdlet του Azure PowerShell ή το SDK HDInsight.

Τη δημιουργία και την προμήθεια του συμπλέγματος με χρήση του Azure PowerShell που τεκμηριώνονται σε [Διαχείριση HDInsight χρήση του PowerShell](hdinsight-administer-use-powershell.md). Η ρύθμιση παραμέτρων από έναν κόμβο πολύ μεγάλες κεφαλής απαιτεί την προσθήκη του `-HeadNodeVMSize ExtraLarge` παράμετρο, για να το `New-AzureRmHDInsightcluster` cmdlet που χρησιμοποιούνται σε αυτόν τον κωδικό.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

Για το SDK, το κείμενο είναι παρόμοια. Τη δημιουργία και την προμήθεια του ένα σύμπλεγμα, χρησιμοποιώντας το SDK που τεκμηριώνονται σε [Χρήση SDK .NET HDInsight](hdinsight-provision-clusters.md#sdk). Η ρύθμιση παραμέτρων από έναν κόμβο πολύ μεγάλες κεφαλής απαιτεί την προσθήκη του `HeadNodeSize = NodeVMSize.ExtraLarge` παράμετρο, για να το `ClusterCreateParameters()` μέθοδο που χρησιμοποιείται σε αυτόν τον κωδικό.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Επόμενα βήματα

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [Σύνδεση με χρήση RDP συμπλεγμάτων HDInsight](hdinsight-administer-use-management-portal.md#rdp)
- [Χρήση HDInsight .NET SDK](hdinsight-provision-clusters.md#sdk)
