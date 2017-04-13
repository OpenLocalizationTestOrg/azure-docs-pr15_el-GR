<properties
   pageTitle="Παρακολούθηση και διαχείριση συμπλεγμάτων HDInsight χρησιμοποιώντας το Apache Ambari REST API | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Ambari για την παρακολούθηση και διαχείριση συμπλεγμάτων που βασίζεται στο Linux HDInsight. Σε αυτό το έγγραφο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε το REST API Ambari περιλαμβάνεται συμπλεγμάτων HDInsight."
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

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Διαχείριση συμπλεγμάτων HDInsight με τη χρήση του Ambari REST API

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari απλοποιεί τη διαχείριση και την παρακολούθηση ενός συμπλέγματος Hadoop, παρέχοντας ένα εύκολο να χρησιμοποιήσετε web περιβάλλοντος εργασίας Χρήστη και REST API. Ambari περιλαμβάνεται στην συμπλεγμάτων βάσει Linux HDInsight και χρησιμοποιείται για να παρακολουθήσετε το σύμπλεγμα και να κάνετε αλλαγές στη ρύθμιση παραμέτρων. Σε αυτό το έγγραφο, θα μάθετε τα βασικά στοιχεία για εργασία με το REST API Ambari εκτελώντας κοινές εργασίες χρησιμοποιώντας καμπύλη.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* [cURL](http://curl.haxx.se/): καμπύλη είναι ένα βοηθητικό πρόγραμμα πλατφόρμες που μπορούν να χρησιμοποιηθούν για να εργαστείτε με REST API από τη γραμμή εντολών. Σε αυτό το έγγραφο, χρησιμοποιείται για να επικοινωνήσετε με το REST API Ambari.
* [jq](https://stedolan.github.io/jq/): jq είναι μια πλατφόρμα βοηθητικό πρόγραμμα γραμμής εντολών για την εργασία με έγγραφα JSON. Σε αυτό το έγγραφο, χρησιμοποιούνται για την ανάλυση των εγγράφων JSON που επιστρέφονται από το REST API Ambari.
* [Azure CLI](../xplat-cli-install.md): μια πλατφόρμα βοηθητικό πρόγραμμα γραμμής εντολών για την εργασία με τις υπηρεσίες του Azure.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a id="whatis"></a>Τι είναι το Ambari;

[Apache Ambari](http://ambari.apache.org) κάνει πιο απλά διαχείρισης Hadoop, παρέχοντας ένα web εύκολο για χρήση περιβάλλοντος εργασίας Χρήστη που μπορεί να χρησιμοποιηθεί για την παροχή, διαχείριση και την παρακολούθηση Hadoop συμπλεγμάτων. Οι προγραμματιστές να ενοποιήσετε αυτές τις δυνατότητες σε εφαρμογές τους, χρησιμοποιώντας τα [Ambari ΥΠΌΛΟΙΠΟ APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari παρέχεται από προεπιλογή με συμπλεγμάτων βάσει Linux HDInsight.

##<a name="rest-api"></a>REST API

Το βασικό URI για το REST API Ambari στην HDInsight είναι https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, όπου __CLUSTERNAME__ είναι το όνομα του συμπλέγματος.

> [AZURE.IMPORTANT] Ενώ το όνομα του συμπλέγματος στο τμήμα τομέα πλήρως προσδιορισμένο όνομα (FQDN) του URI (CLUSTERNAME.azurehdinsight.net) είναι διάκριση πεζών-κεφαλαίων, άλλες εμφανίσεις στο URI διάκριση πεζών-κεφαλαίων. Για παράδειγμα, εάν το σύμπλεγμά σας ονομάζεται MyCluster, τα παρακάτω είναι έγκυρη URI:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Το παρακάτω URIs επιστρέψουν σφάλμα, επειδή η δεύτερη εμφάνιση του ονόματος δεν είναι σωστή πεζών και κεφαλαίων γραμμάτων.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Σύνδεση με Ambari σε HDInsight απαιτεί HTTPS. Κατά τον έλεγχο ταυτότητας της σύνδεσης, πρέπει να χρησιμοποιήσετε το όνομα του λογαριασμού διαχειριστή (η προεπιλογή είναι __διαχειριστής__), και τον κωδικό πρόσβασης που παρείχατε κατά τη δημιουργία του συμπλέγματος.

Το παρακάτω παράδειγμα χρησιμοποιεί καμπύλη για να κάνετε ένα αίτημα GET σε σχέση με το REST API. Αντικαταστήστε __τον κωδικό ΠΡΌΣΒΑΣΗΣ__ με τον κωδικό πρόσβασης διαχειριστή για το σύμπλεγμα. Αντικαταστήστε το __CLUSTERNAME__ με το όνομα του συμπλέγματος:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Η απόκριση δεν είναι ένα έγγραφο JSON που αρχίζει με πληροφορίες που είναι παρόμοιο με το εξής:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Επειδή πρόκειται για JSON, είναι πιο εύκολο να χρησιμοποιήσετε μια ανάλυσης JSON για να εργαστείτε με τα δεδομένα. Για παράδειγμα, το ακόλουθο παράδειγμα χρησιμοποιεί jq για να εμφανίσετε μόνο το `health_report` στοιχείο.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>Παράδειγμα: Λήψη το FQDN του κόμβοι συμπλέγματος

Όταν εργάζεστε με HDInsight, ενδέχεται να πρέπει να γνωρίζετε το πλήρως προσδιορισμένο όνομα τομέα (FQDN) της έναν κόμβο συμπλέγματος. Μπορείτε εύκολα να την ανακτήσετε το FQDN για τα διάφορα κόμβους του συμπλέγματος χρησιμοποιώντας τα εξής:

* __Προϊστάμενος κόμβους__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Οι κόμβοι εργασίας__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Οι κόμβοι zookeeper__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Σημείωση κάθε ένα από αυτά τα παραδείγματα ακολουθήστε το ίδιο μοτίβο:

1. Ερώτημα ένα στοιχείο που γνωρίζουμε εκτελείται σε αυτές τις κόμβους.
2. Ανάκτηση του `host_name` στοιχεία, τα οποία περιέχουν το FQDN για αυτές τις κόμβους.

Το `host_components` στοιχείο του αποστολέα εγγράφου περιέχει πολλά στοιχεία. Χρήση `.host_components[]`, και, στη συνέχεια, να καθορίσετε μια διαδρομή μέσα στο στοιχείο θα διέλευσης κάθε στοιχείο και να αποσπάσετε την τιμή από τη συγκεκριμένη διαδρομή. Εάν θέλετε μόνο μία τιμή, όπως την πρώτη εγγραφή FQDN, μπορείτε να επιστρέψετε τα στοιχεία ως συλλογή και να, στη συνέχεια, επιλέξτε μια συγκεκριμένη εγγραφή:

    jq '[.host_components[].HostRoles.host_name][0]'

Αυτή η διαδικασία επιστρέφει το πρώτο FQDN από τη συλλογή.

##<a name="example-get-the-default-storage-account-and-container"></a>Παράδειγμα: Λήψη του προεπιλεγμένου λογαριασμού χώρου αποθήκευσης και κοντέινερ

Όταν δημιουργείτε ένα σύμπλεγμα HDInsight, πρέπει να χρησιμοποιήσετε ένα λογαριασμό χώρου αποθήκευσης Azure και ένα κοντέινερ αντικειμένων blob ως το προεπιλεγμένο αποθήκευσης για το σύμπλεγμα. Μπορείτε να χρησιμοποιήσετε Ambari για να ανακτήσετε αυτές τις πληροφορίες, μετά τη δημιουργία του συμπλέγματος. Για παράδειγμα, εάν θέλετε να μέσω προγραμματισμού εγγραφή δεδομένων απευθείας στο κοντέινερ.

Το παρακάτω θα ανακτήσει το URI WASB το χώρο αποθήκευσης προεπιλεγμένη συμπλεγμάτων:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Αυτή η συνάρτηση επιστρέφει την πρώτη ρύθμιση των παραμέτρων εφαρμόζεται σε διακομιστή (`service_config_version=1`,) που περιέχει αυτές τις πληροφορίες. Εάν ανακτήσετε μια τιμή που έχει τροποποιηθεί μετά τη δημιουργία σύμπλεγμα, ίσως χρειαστεί να λίστα τις εκδόσεις ρύθμισης παραμέτρων και να ανακτήσετε την πιο πρόσφατη μία.

Αυτή η διαδικασία επιστρέφει μια τιμή παρόμοιο με το παρακάτω παράδειγμα, όπου __το ΚΟΝΤΈΙΝΕΡ__ είναι το προεπιλεγμένο κοντέινερ και __όνομα ΛΟΓΑΡΙΑΣΜΟΎ__ είναι το όνομα του λογαριασμού χώρου αποθήκευσης Azure:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Στη συνέχεια, μπορείτε να χρησιμοποιήσετε αυτές τις πληροφορίες με το [Azure CLI](../xplat-cli-install.md) αποστολή ή λήψη δεδομένων από το κοντέινερ.

1. Λάβετε την ομάδα των πόρων για το λογαριασμό χώρου αποθήκευσης. Αντικαταστήστε το __όνομα ΛΟΓΑΡΙΑΣΜΟΎ__ με το όνομα του λογαριασμού χώρου αποθήκευσης ανακτήθηκαν από Ambari:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Αυτή η διαδικασία επιστρέφει το όνομα της ομάδας πόρων για το λογαριασμό.
    
    > [AZURE.NOTE] Εάν έχει κανένα αποτέλεσμα επιστρέφεται από αυτή την εντολή, ίσως χρειαστεί να αλλάξετε το Azure CLI λειτουργία διαχείρισης πόρων Azure και εκτελέστε ξανά την εντολή. Για να μεταβείτε σε λειτουργία διαχείρισης πόρων Azure, χρησιμοποιήστε την ακόλουθη εντολή:
    >
    > `azure config mode arm`
    
2. Λήψη του αριθμού-κλειδιού για το λογαριασμό χώρου αποθήκευσης. Αντικαταστήστε το __όνομα ΟΜΆΔΑΣ__ με την ομάδα των πόρων από το προηγούμενο βήμα. Αντικαταστήστε το __όνομα ΛΟΓΑΡΙΑΣΜΟΎ__ με το όνομα του λογαριασμού χώρου αποθήκευσης:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    Αυτό το παράδειγμα επιστρέφει το πρωτεύον κλειδί για το λογαριασμό.
    
3. Χρησιμοποιήστε την εντολή "Αποστολή" για να αποθηκεύσετε ένα αρχείο στο κοντέινερ:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    Αντικαταστήστε το __όνομα ΛΟΓΑΡΙΑΣΜΟΎ__ με το όνομα του λογαριασμού χώρου αποθήκευσης. Αντικατάσταση __ACCOUNTKEY__ με τον αριθμό-κλειδί ανάκτηση προηγουμένως. __FILEPATH__ είναι η διαδρομή προς το αρχείο που θέλετε να αποστείλετε, ενώ __BLOBPATH__ είναι η διαδρομή του κοντέινερ.

    Για παράδειγμα, εάν θέλετε το αρχείο για να εμφανίζονται στο HDInsight στο wasbs://example/data/filename.txt, στη συνέχεια, __BLOBPATH__ θα ήταν `example/data/filename.txt`.

##<a name="example-update-ambari-configuration"></a>Παράδειγμα: Ενημέρωση Ambari ρύθμισης παραμέτρων

1. Εμφανίζεται η τρέχουσα ρύθμιση παραμέτρων, το οποίο αποθηκεύει Ambari ως η "επιθυμητή ρύθμιση παραμέτρων":

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    Αυτό το παράδειγμα επιστρέφει JSON εγγράφου που περιέχει την τρέχουσα ρύθμιση παραμέτρων (που αναγνωρίζονται από την _ετικέτα_ τιμή) για τα στοιχεία που έχουν εγκατασταθεί στο σύμπλεγμα. Το παράδειγμα που ακολουθεί είναι ένα απόσπασμα από τα δεδομένα που επιστρέφονται από έναν τύπο σύμπλεγμα τους.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    Από αυτήν τη λίστα, πρέπει να αντιγράψετε το όνομα του στοιχείου (για παράδειγμα, __τους\_thrift\_sparkconf__ και την τιμή της __ετικέτας__ .
    
2. Ανακτήστε τη ρύθμιση παραμέτρων για το στοιχείο και ετικέτας, χρησιμοποιώντας την ακόλουθη εντολή. Αντικαταστήστε __τους-thrift-sparkconf__ και την __ΑΡΧΙΚΉ__ με το στοιχείο και την ετικέτα που θέλετε να ανακτήσετε τη ρύθμιση παραμέτρων για.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Καμπύλη ανακτά το έγγραφο JSON και, στη συνέχεια, jq χρησιμοποιείται για να κάνετε τροποποιήσεις τα δεδομένα για να δημιουργήσετε ένα πρότυπο. Στη συνέχεια, χρησιμοποιείται το πρότυπο για να προσθέσετε/τροποποιήσετε τιμές παραμέτρων. Ειδικά κάνει τα εξής:
    
    * Δημιουργεί μια μοναδική τιμή που περιέχει τη συμβολοσειρά "έκδοση" και την ημερομηνία, η οποία είναι αποθηκευμένη στο __newtag__.
    * Δημιουργεί ένα έγγραφο ριζικό κατάλογο για τη νέα επιθυμητή ρύθμιση παραμέτρων.
    * Λαμβάνει τα περιεχόμενα του `.items[]` πίνακα και το προσθέτει το στοιχείο __desired_config__ .
    * Διαγράφει το __href__, την __έκδοση__και __ρύθμισης παραμέτρων__ στοιχεία, όπως αυτά τα στοιχεία δεν είναι απαραίτητα για να υποβάλετε μια νέα ρύθμιση παραμέτρων.
    * Προσθέτει ένα νέο στοιχείο __ετικέτας__ και ορίζει την τιμή σε __έκδοση ###__. Το αριθμητικό τμήμα είναι με βάση την τρέχουσα ημερομηνία. Κάθε ρύθμιση πρέπει να έχει μια μοναδική ετικέτα.
    
    Τέλος, τα δεδομένα αποθηκεύονται στο έγγραφο __newconfig.json__ . Τη δομή του εγγράφου θα πρέπει να είναι παρόμοιο με το ακόλουθο παράδειγμα:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Ανοίξτε τις τιμές για το έγγραφο και τροποποίηση/Προσθήκη __newconfig.json__ στο αντικείμενο __Ιδιότητες__ . Το παρακάτω παράδειγμα αλλάζει την τιμή __"spark.yarn.am.memory"__ από __"ζ 1"__ σε __"3 g"__ και, και προσθέτει ένα νέο στοιχείο για __"spark.kryoserializer.buffer.max"__ με την τιμή __"256 μ"__.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Αφού ολοκληρώσετε τις τροποποιήσεις, αποθηκεύστε το αρχείο.

4. Χρησιμοποιήστε την παρακάτω εντολή για να υποβάλετε τις ενημερωμένες παραμέτρους για να Ambari.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Αυτή η εντολή σωλήνες τα περιεχόμενα του αρχείου __newconfig.json__ στην πρόσκληση σε καμπύλη, που υποβάλλει στην το σύμπλεγμα ως το νέο επιθυμητή ρύθμιση παραμέτρων. Η πρόσκληση σε καμπύλη επιστρέφει ένα έγγραφο JSON. Το στοιχείο __versionTag__ σε αυτό το έγγραφο θα πρέπει να συμφωνεί με την έκδοση που έχετε υποβάλει και το αντικείμενο __διαμορφώσεων__ θα περιέχει τις αλλαγές ρύθμισης παραμέτρων που ζητήθηκε.

###<a name="example-restart-a-service-component"></a>Παράδειγμα: Επανεκκινήστε το στοιχείο παροχής υπηρεσιών

Σε αυτό το σημείο, αν κοιτάξετε το web Ambari περιβάλλοντος εργασίας Χρήστη, την υπηρεσία τους θα δηλώνει ότι πρέπει να γίνει επανεκκίνηση πριν από τη νέα ρύθμιση παραμέτρων μπορούν να τεθούν σε ισχύ. Χρησιμοποιήστε τα παρακάτω βήματα για να κάνετε επανεκκίνηση της υπηρεσίας.

1. Χρησιμοποιήστε τα ακόλουθα για να ενεργοποιήσετε την κατάσταση λειτουργίας συντήρησης για την υπηρεσία τους:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Αυτή η εντολή αποστέλλει ένα έγγραφο JSON στο διακομιστή (που περιέχονται σε το `echo` πρόταση,) που ενεργοποιεί λειτουργία συντήρησης.
    Μπορείτε να επαληθεύσετε ότι η υπηρεσία είναι τώρα σε κατάσταση λειτουργίας συντήρησης χρησιμοποιώντας την ακόλουθη αίτηση:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    Αυτό θα επιστρέψει μια τιμή `"ON"`.

3. Στη συνέχεια, χρησιμοποιήστε τα ακόλουθα για να απενεργοποιήσετε την υπηρεσία:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Αυτή η εντολή επιστρέφει μια απόκριση παρόμοια με την ακόλουθη.
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    Το `href` τιμή που επιστρέφεται από αυτό URI χρησιμοποιεί την εσωτερική διεύθυνση IP του κόμβου συμπλέγματος. Για να χρησιμοποιήσετε από έξω από το σύμπλεγμα, αντικαταστήστε το τμήμα '10.0.0.18:8080' με το FQDN του συμπλέγματος. Για παράδειγμα, την ακόλουθη εντολή ανακτά την κατάσταση της αίτησης.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Εάν αυτή η διαδικασία επιστρέφει μια τιμή `"COMPLETED"` , στη συνέχεια, την αίτηση έχει ολοκληρωθεί.

4. Μόλις ολοκληρωθεί η προηγούμενη αίτηση, χρησιμοποιήστε τα ακόλουθα για να ξεκινήσετε την υπηρεσία.

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Μετά την επανεκκίνηση της υπηρεσίας, είναι οι νέες ρυθμίσεις παραμέτρων.

5. Τέλος, χρησιμοποιήστε τα ακόλουθα για να απενεργοποιήσετε τη λειτουργία συντήρησης.

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>Επόμενα βήματα

Για μια ολοκληρωμένη αναφορά του REST API, ανατρέξτε στο θέμα [V1 αναφορά API Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Ορισμένες λειτουργίες Ambari είναι απενεργοποιημένη, όπως η διαχείρισή της γίνεται από την υπηρεσία cloud HDInsight. Για παράδειγμα, η προσθήκη ή κατάργηση hosts από το σύμπλεγμα ή προσθήκη νέων υπηρεσιών.
