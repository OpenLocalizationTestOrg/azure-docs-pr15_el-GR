<properties
    pageTitle="Παρακολούθηση συμπλεγμάτων Hadoop στο HDInsight με χρήση του API Ambari | Microsoft Azure"
    description="Χρησιμοποιήστε τα API Ambari Apache για τη δημιουργία, τη διαχείριση και παρακολούθηση Hadoop συμπλεγμάτων. Εργαλεία διαισθητική τελεστή και API του Yammer αποκρύψτε την πολυπλοκότητα του Hadoop."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Οθόνη Hadoop συμπλεγμάτων στο HDInsight με χρήση του API Ambari

Μάθετε πώς μπορείτε να παρακολουθείτε συμπλεγμάτων HDInsight με τη χρήση Ambari APIs.

> [AZURE.NOTE] Οι πληροφορίες σε αυτό το άρθρο είναι κυρίως για συμπλεγμάτων HDInsight που βασίζεται στα Windows, τα οποία παρέχουν μια έκδοση του το REST API Ambari μόνο για ανάγνωση. Για Linux βάσει συμπλεγμάτων, ανατρέξτε στο θέμα [Διαχείριση Hadoop συμπλεγμάτων χρησιμοποιώντας Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Τι είναι το Ambari;

[Apache Ambari] [ ambari-home] χρησιμοποιείται για την προμήθεια, τη διαχείριση και παρακολούθηση Apache Hadoop συμπλεγμάτων. Περιλαμβάνει μια διαισθητική συλλογή εργαλείων τελεστή και ένα αξιόπιστο σύνολο API που απόκρυψη την πολυπλοκότητα του Hadoop, απλοποίηση της λειτουργίας συμπλεγμάτων. Για περισσότερες πληροφορίες σχετικά με τα API, ανατρέξτε στο θέμα [Αναφορά API Ambari][ambari-api-reference]. 

HDInsight υποστηρίζει επί του παρόντος μόνο τη Ambari παρακολούθησης δυνατότητα. Ambari API 1.0 υποστηρίζεται από το HDInsight έκδοση 3.0 και 2.1 συμπλεγμάτων. Σε αυτό το άρθρο καλύπτει κατά την πρόσβαση σε APIs Ambari στην συμπλεγμάτων έκδοση 3.1 και 2.1 HDInsight. Η βασική διαφορά μεταξύ των δύο είναι ότι ορισμένα από τα στοιχεία έχουν αλλάξει με την εισαγωγή του νέες δυνατότητες (όπως ο διακομιστής ιστορικού εργασίας). 

**Προαπαιτούμενα στοιχεία**

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Μια σταθμούς εργασίας με το Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- (Προαιρετικό) [cURL] [curl]. Για να το εγκαταστήσετε, ανατρέξτε στο θέμα [cURL εκδόσεις και στοιχεία λήψης][curl-download].

    >[AZURE.NOTE] Όταν χρησιμοποιήσετε την εντολή καμπύλη στα Windows, χρησιμοποιήστε διπλά εισαγωγικά αντί για μονό εισαγωγικά για τις τιμές επιλογής.

- **Σύμπλεγμα του Azure HDInsight**. Για οδηγίες σχετικά με την προμήθεια του συμπλέγματος, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το HDInsight] [ hdinsight-get-started] ή [HDInsight παροχή συμπλεγμάτων][hdinsight-provision]. Θα χρειαστείτε τα εξής δεδομένα για να μεταβείτε στο πρόγραμμα εκμάθησης:

    Ιδιότητα συμπλέγματος|Azure PowerShell το όνομα της μεταβλητής|Τιμή|Περιγραφή
    ---|---|---|---
    Όνομα HDInsight συμπλέγματος|$clusterName||Το όνομα του συμπλέγματος HDInsight.
    Όνομα χρήστη συμπλέγματος|$clusterUsername||Όνομα χρήστη σύμπλεγμα που καθορίζεται κατά την οποία δημιουργήθηκε το σύμπλεγμα.
    Σύμπλεγμα τον κωδικό πρόσβασης|$clusterPassword||Κωδικός πρόσβασης χρήστη σύμπλεγμα.

    >[AZURE.NOTE] Συμπλήρωση τις τιμές στον πίνακα. Αυτό θα είναι χρήσιμες για διέρχονται από αυτό το πρόγραμμα εκμάθησης.

## <a name="jump-start"></a>Έναρξη μεταπήδηση

Υπάρχουν πολλοί τρόποι για να χρησιμοποιήσετε Ambari για την παρακολούθηση της συμπλεγμάτων HDInsight.

**Χρήση του Azure PowerShell**

Ακολουθεί μια δέσμη ενεργειών του Azure PowerShell για να λάβετε πληροφορίες για την παρακολούθηση εργασία MapReduce *σε ένα σύμπλεγμα HDInsight 3.1.*  Η βασική διαφορά είναι μας ελκυστική αυτές τις λεπτομέρειες από την υπηρεσία ΝΉΜΑΤΑ (αντί για MapReduce).

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Ακολουθεί μια δέσμη ενεργειών του Azure PowerShell για τη λήψη του MapReduce εργασία παρακολούθηση πληροφοριών *σε ένα σύμπλεγμα HDInsight 2.1*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Το αποτέλεσμα είναι:

![Jobtracker εξόδου][img-jobtracker-output]

**Χρησιμοποιήστε καμπύλη**

Ακολουθεί ένα παράδειγμα γρήγορα πληροφορίες σύμπλεγμα χρησιμοποιώντας καμπύλη:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Το αποτέλεσμα είναι:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**10/8/2014 έκδοση**:

Όταν χρησιμοποιείτε το τελικό σημείο Ambari, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", το πεδίο *όνομα_κεντρικού_υπολογιστή* επιστρέφει το πλήρως προσδιορισμένο όνομα τομέα (FQDN) του κόμβου αντί για το όνομα κεντρικού υπολογιστή. Πριν από την κυκλοφορία 8/10/2014, αυτό το παράδειγμα επιστρέφονται απλώς "**headnode0**". Μετά την κυκλοφορία 8/10/2014, λαμβάνετε το FQDN "**headnode0. { .Azurehdinsight ClusterDNS} .net**", όπως φαίνεται στο προηγούμενο παράδειγμα. Αυτή η αλλαγή έχει απαιτούνται για να διευκολυνθεί σενάρια όπου μπορούν να αναπτυχθούν πολλούς τύπους σύμπλεγμα (όπως HBase και Hadoop) σε μία εικονικού δικτύου (VNET). Αυτό συμβαίνει, για παράδειγμα, κατά τη χρήση HBase ως πλατφόρμα παρασκηνίου για Hadoop.

## <a name="ambari-monitoring-apis"></a>Παρακολούθηση APIs Ambari

Ο παρακάτω πίνακας παραθέτει ορισμένα από τα πιο συνηθισμένα Ambari παρακολούθηση κλήσεων API. Για περισσότερες πληροφορίες σχετικά με το API, ανατρέξτε στο θέμα [Αναφορά API Ambari][ambari-api-reference].

Οθόνη API κλήσης|URI|Περιγραφή
---|---|---
Λήψη συμπλεγμάτων|`/api/v1/clusters`|
Λήψη πληροφοριών του συμπλέγματος.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|συμπλεγμάτων, υπηρεσίες, τους κεντρικούς υπολογιστές
Λήψη υπηρεσιών|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|Οι υπηρεσίες περιλαμβάνουν: hdfs, mapreduce
Λήψη πληροφοριών υπηρεσιών.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
Λάβετε στοιχεία της υπηρεσίας|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|HDFS: namenode, datanode<br/>MapReduce: jobtracker; tasktracker
Λήψη πληροφοριών του στοιχείου.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo, κεντρικός υπολογιστής στοιχεία, μετρικά
Λήψη κεντρικών υπολογιστών|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0, workernode0
Λήψη πληροφοριών του κεντρικού υπολογιστή.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
Λήψη στοιχείων κεντρικού υπολογιστή|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|namenode, resourcemanager
Λήψη host πληροφοριών του στοιχείου.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles, το στοιχείο, κεντρικός υπολογιστής, μετρικά
Λήψη ρυθμίσεις παραμέτρων|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|Τύποι config: πυρήνα τοποθεσίας, hdfs τοποθεσίας, mapred τοποθεσίας, τοποθεσία ομάδας
Λάβετε πληροφορίες ρύθμισης παραμέτρων.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|Τύποι config: πυρήνα τοποθεσίας, hdfs τοποθεσίας, mapred τοποθεσίας, τοποθεσία ομάδας


##<a name="next-steps"></a>Επόμενα βήματα

Τώρα έχετε μάθει πώς μπορείτε να χρησιμοποιήσετε Ambari παρακολούθηση κλήσεων API. Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Διαχείριση συμπλεγμάτων HDInsight με την πύλη Azure][hdinsight-admin-portal]
- [Διαχείριση συμπλεγμάτων HDInsight με χρήση του PowerShell Azure][hdinsight-admin-powershell]
- [Διαχείριση συμπλεγμάτων HDInsight χρησιμοποιώντας περιβάλλον γραμμής εντολών][hdinsight-admin-cli]
- [Τεκμηρίωση HDInsight][hdinsight-documentation]
- [Γρήγορα αποτελέσματα με το HDInsight][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
