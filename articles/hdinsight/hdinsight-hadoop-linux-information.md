<properties
   pageTitle="Συμβουλές για τη χρήση Hadoop σε βάσει Linux HDInsight | Microsoft Azure"
   description="Δείτε εφαρμογή συμβουλές για τη χρήση συμπλεγμάτων βάσει Linux HDInsight (Hadoop) σε ένα οικείο περιβάλλον Linux εκτελείται στο Azure cloud."
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
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>Πληροφορίες σχετικά με τη χρήση HDInsight στην Linux

Βάσει Linux συμπλεγμάτων Azure HDInsight παρέχει Hadoop ένα οικείο περιβάλλον Linux, εκτελείται στο Azure cloud. Για περισσότερα από αυτά, θα πρέπει να λειτουργεί ακριβώς όπως οποιαδήποτε άλλη εγκατάσταση Hadoop-σε-Linux. Αυτό το έγγραφο κλήσεις εκτός συγκεκριμένες διαφορές που πρέπει να γνωρίζετε.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Πολλά από τα βήματα σε αυτό το έγγραφο Χρησιμοποιήστε τα ακόλουθα βοηθητικά προγράμματα, η οποία μπορεί να χρειάζεται να έχουν εγκατασταθεί στο σύστημά σας.

* [cURL](https://curl.haxx.se/) - χρησιμοποιείται για την επικοινωνία με τις υπηρεσίες που βασίζονται στο web
* [jq](https://stedolan.github.io/jq/) - χρησιμοποιείται για την ανάλυση JSON εγγράφων
* [Azure CLI](../xplat-cli-install.md) - χρησιμοποιείται για την απομακρυσμένη διαχείριση των υπηρεσιών Azure

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>Ονόματα τομέων

Το πλήρως προσδιορισμένο όνομα τομέα (FQDN) για να χρησιμοποιήσετε κατά τη σύνδεση στο σύμπλεγμα από το internet είναι ** &lt;clustername >. azurehdinsight.net** ή (για SSH μόνο) ** &lt;clustername-ssh >. azurehdinsight.net**.

Εσωτερικά, κάθε κόμβο του συμπλέγματος έχει ένα όνομα που έχει αντιστοιχιστεί κατά τη ρύθμιση παραμέτρων του συμπλέγματος. Για να βρείτε τα ονόματα σύμπλεγμα, μπορείτε να επισκεφθείτε τη σελίδα __Hosts__ στο περιβάλλον εργασίας Χρήστη του Ambari Web ή χρησιμοποιήστε τα ακόλουθα για να λάβετε μια λίστα κεντρικών υπολογιστών από το REST API Ambari:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Αντικαταστήστε __τον κωδικό ΠΡΌΣΒΑΣΗΣ__ με τον κωδικό πρόσβασης του λογαριασμού διαχειριστή και __CLUSTERNAME__ με το όνομα του συμπλέγματος. Αυτό θα επιστρέψει ένα έγγραφο JSON που περιέχει μια λίστα με τους κεντρικούς υπολογιστές του συμπλέγματος και, στη συνέχεια, jq χρησιμοποιεί τα του `host_name` στοιχείο τιμή για κάθε κεντρικό υπολογιστή του συμπλέγματος.

Εάν πρέπει να βρείτε το όνομα του κόμβου για μια συγκεκριμένη υπηρεσία, μπορείτε να υποβάλετε ερώτημα Ambari για το συγκεκριμένο στοιχείο. Για παράδειγμα, για να βρείτε τους κεντρικούς υπολογιστές για τον κόμβο όνομα HDFS, χρησιμοποιήστε τα ακόλουθα.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Αυτή η συνάρτηση επιστρέφει ένα έγγραφο JSON που περιγράφει την υπηρεσία και, στη συνέχεια, jq χρησιμοποιεί τα μόνο το `host_name` τιμή για τους κεντρικούς υπολογιστές.

## <a name="remote-access-to-services"></a>Απομακρυσμένη πρόσβαση σε υπηρεσίες

* **Ambari (web)** - https://&lt;clustername >. azurehdinsight.net

    Τον έλεγχο ταυτότητας με τη χρήση του διαχειριστή συμπλέγματος χρήστη και τον κωδικό πρόσβασης και, στη συνέχεια, συνδεθείτε στο Ambari. Αυτό χρησιμοποιεί επίσης το σύμπλεγμα διαχειριστή χρήστη και κωδικό πρόσβασης.

    Ο έλεγχος ταυτότητας είναι απλού κειμένου - χρησιμοποιείται πάντα HTTPS για να διασφαλίσετε ότι η σύνδεση είναι ασφαλής.

    > [AZURE.IMPORTANT] Ενώ Ambari για το σύμπλεγμά σας είναι προσβάσιμος απευθείας μέσω του Internet, ορισμένες λειτουργίες του εξαρτάται από την πρόσβαση σε κόμβους με το όνομα εσωτερικού τομέα που χρησιμοποιείται από το σύμπλεγμα. Δεδομένου ότι αυτό είναι ένα όνομα εσωτερικού τομέα και δεν είναι δημόσια, θα λάβετε σφάλματα "ο διακομιστής δεν βρέθηκε" όταν προσπαθείτε να αποκτήσετε πρόσβαση σε ορισμένες δυνατότητες μέσω του Internet.
    >
    > Για να χρησιμοποιήσετε την πλήρη λειτουργικότητα του web Ambari περιβάλλοντος εργασίας Χρήστη, χρησιμοποιήστε ένα σωλήνα SSH την κυκλοφορία web διακομιστή μεσολάβησης στον κόμβο κεφαλής συμπλέγματος. Ανατρέξτε στο θέμα [Χρήση SSH διοχέτευση για πρόσβαση στο web Ambari περιβάλλοντος εργασίας Χρήστη, ResourceManager, JobHistory, NameNode, Oozie, και άλλη τοποθεσία web του περιβάλλοντος εργασίας Χρήστη](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** - https://&lt;clustername >.azurehdinsight.net/ambari

    > [AZURE.NOTE] Έλεγχος ταυτότητας με τη χρήση του διαχειριστή συμπλέγματος χρήστη και τον κωδικό πρόσβασης.
    >
    > Ο έλεγχος ταυτότητας είναι απλού κειμένου - χρησιμοποιείται πάντα HTTPS για να διασφαλίσετε ότι η σύνδεση είναι ασφαλής.

* **WebHCat (Templeton)** - https://&lt;clustername >.azurehdinsight.net/templeton

    > [AZURE.NOTE] Έλεγχος ταυτότητας με τη χρήση του διαχειριστή συμπλέγματος χρήστη και τον κωδικό πρόσβασης.
    >
    > Ο έλεγχος ταυτότητας είναι απλού κειμένου - χρησιμοποιείται πάντα HTTPS για να διασφαλίσετε ότι η σύνδεση είναι ασφαλής.

* **SSH** - &lt;clustername >-ssh.azurehdinsight.net στη θύρα 22 ή 23. Θύρα 22 χρησιμοποιείται για να συνδεθείτε με το πρωτεύον headnode, ενώ 23 χρησιμοποιείται για να συνδεθείτε με τη δευτερεύουσα. Για περισσότερες πληροφορίες σχετικά με τους κόμβους κεφαλής, ανατρέξτε στο θέμα [διαθεσιμότητα και την αξιοπιστία των συμπλεγμάτων Hadoop στο HDInsight](hdinsight-high-availability-linux.md).

    > [AZURE.NOTE] Μπορείτε μόνο να πρόσβαση τους κόμβους κεφαλής σύμπλεγμα μέσω SSH από έναν υπολογιστή-πελάτη. Μετά τη σύνδεση, έχετε τη δυνατότητα πρόσβασης τους κόμβους εργαζόμενου χρησιμοποιώντας SSH από μια headnode.

## <a name="file-locations"></a>Θέσεις αρχείων

Τα αρχεία που σχετίζονται με το Hadoop μπορεί να βρεθεί στην τους κόμβους συμπλέγματος στο `/usr/hdp`. Αυτός ο κατάλογος περιλαμβάνει τα παρακάτω υποκατάλογοι:

* __2.2.4.9-1__: ονομάζεται αυτόν τον κατάλογο για την έκδοση της πλατφόρμας Hortonworks δεδομένων χρησιμοποιούνται από το HDInsight, ώστε ο αριθμός σε το σύμπλεγμά σας μπορεί να είναι διαφορετικά από εκείνο που παρατίθεται εδώ.
* __τρέχουσα__: αυτόν τον κατάλογο περιέχει συνδέσεις σε καταλόγους κάτω από τον κατάλογο __2.2.4.9-1__ και υπάρχει, έτσι ώστε να μην χρειάζεται να πληκτρολογήσετε έναν αριθμό έκδοσης (που μπορεί να αλλάξει,) κάθε φορά που θέλετε να έχετε πρόσβαση σε ένα αρχείο.

Μπορείτε να βρείτε δεδομένα του παραδείγματος και ΒΆΖΟ αρχεία στο σύστημα αρχείων κατανεμημένο Hadoop (HDFS) ή αντικειμένων Blob του Azure χώρου αποθήκευσης στο ' / παράδειγμα ' ή ' wasbs: / / / παράδειγμα '.

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>HDFS, χώρος αποθήκευσης αντικειμένων Blob του Azure και βέλτιστες πρακτικές χώρου αποθήκευσης

Στα περισσότερα κατανομές Hadoop, HDFS είναι αντίγραφα ασφαλείας τοπικού χώρου αποθήκευσης σε υπολογιστές του συμπλέγματος. Ενώ αυτή είναι αποτελεσματική, μπορεί να είναι κοστίζουν για μια λύση που βασίζεται στο cloud όπου που θα χρεωθείτε ανά ώρα ή κατά λεπτό για τους πόρους υπολογισμού.

HDInsight χρησιμοποιεί χώρο αποθήκευσης αντικειμένων Blob του Azure ως τον προεπιλεγμένο χώρο αποθήκευσης, η οποία παρέχει τα ακόλουθα πλεονεκτήματα:

* Φτηνά μακροπρόθεσμες χώρου αποθήκευσης

* Άτομα με ειδικές ανάγκες από εξωτερικές υπηρεσίες, όπως τοποθεσίες Web, βοηθητικά προγράμματα αποστολή/λήψη αρχείου, διάφορες SDK γλώσσας και προγράμματα περιήγησης στο web

Εφόσον είναι το προεπιλεγμένο χώρο αποθήκευσης για το HDInsight, κανονικά δεν χρειάζεται να κάνετε τίποτα για να το χρησιμοποιήσετε. Για παράδειγμα, την παρακάτω εντολή θα εμφανιστούν τα αρχεία στο φάκελο **/example/data** , το οποίο είναι αποθηκευμένο στο χώρο αποθήκευσης αντικειμένων Blob του Azure:

    hdfs dfs -ls /example/data

Ορισμένες εντολές ενδέχεται να απαιτούν να καθορίσετε ότι χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob. Για αυτές τις, μπορείτε να πρόθεμα την εντολή με **wasb: / /**, ή **wasbs: / /**.

HDInsight σας επιτρέπει επίσης να συσχετίσετε πολλούς λογαριασμούς χώρο αποθήκευσης αντικειμένων Blob με ένα σύμπλεγμα. Για να αποκτήσετε πρόσβαση σε δεδομένα σε ένα λογαριασμό χώρο αποθήκευσης αντικειμένων Blob μη προεπιλεγμένες, μπορείτε να χρησιμοποιήσετε τη μορφή **wasbs: / /&lt;container-name>@&lt;όνομα λογαριασμού >.blob.core.windows.net/**. Για παράδειγμα, το ακόλουθο κείμενο θα εμφανιστούν τα περιεχόμενα του καταλόγου **/example/data** για το καθορισμένο κοντέινερ και το λογαριασμό χώρου αποθήκευσης αντικειμένων Blob:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>Τι χώρος αποθήκευσης αντικειμένων Blob χρησιμοποιεί το σύμπλεγμα;

Κατά τη δημιουργία συμπλέγματος, έχετε επιλέξει να είτε να χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό αποθήκευσης Azure και κοντέινερ ή δημιουργήστε ένα νέο. Στη συνέχεια, πιθανότατα ξεχάσατε για αυτό. Μπορείτε να βρείτε το προεπιλεγμένο λογαριασμό χώρου αποθήκευσης και κοντέινερ, χρησιμοποιώντας το REST API Ambari.

1. Χρησιμοποιήστε την ακόλουθη εντολή για την ανάκτηση πληροφοριών ρύθμισης παραμέτρων HDFS με καμπύλη και το φιλτράρισμα με χρήση [jq](https://stedolan.github.io/jq/):

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] Αυτό θα επιστρέψει την πρώτη ρύθμιση των παραμέτρων εφαρμόζονται στο διακομιστή (`service_config_version=1`,) που θα περιέχει αυτές τις πληροφορίες. Αν κάνετε ανάκτηση μια τιμή που έχει τροποποιηθεί μετά τη δημιουργία σύμπλεγμα, ίσως χρειαστεί να λίστα τις εκδόσεις ρύθμισης παραμέτρων και να ανακτήσετε την πιο πρόσφατη μία.

    Αυτό θα επιστρέψει μια τιμή με την εξής, όπου __το ΚΟΝΤΈΙΝΕΡ__ είναι το προεπιλεγμένο κοντέινερ και __όνομα ΛΟΓΑΡΙΑΣΜΟΎ__ είναι το όνομα του λογαριασμού χώρου αποθήκευσης Azure:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. Λάβετε την ομάδα των πόρων για το λογαριασμό χώρου αποθήκευσης, χρησιμοποιήστε το [Azure CLI](../xplat-cli-install.md). Στην παρακάτω εντολή, αντικαταστήστε το __όνομα ΛΟΓΑΡΙΑΣΜΟΎ__ με το όνομα λογαριασμού χώρου αποθήκευσης που ανακτήθηκαν από Ambari:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Αυτό θα επιστρέψει το όνομα της ομάδας πόρων για το λογαριασμό.
    
    > [AZURE.NOTE] Εάν έχει κανένα αποτέλεσμα επιστρέφεται από αυτή την εντολή, ίσως χρειαστεί να αλλάξετε το Azure CLI λειτουργία διαχείρισης πόρων Azure και εκτελέστε ξανά την εντολή. Για να μεταβείτε σε λειτουργία διαχείρισης πόρων Azure, χρησιμοποιήστε την ακόλουθη εντολή.
    >
    > `azure config mode arm`
    
2. Λήψη του αριθμού-κλειδιού για το λογαριασμό χώρου αποθήκευσης. Αντικαταστήστε το __όνομα ΟΜΆΔΑΣ__ με την ομάδα των πόρων από το προηγούμενο βήμα. Αντικαταστήστε το __όνομα ΛΟΓΑΡΙΑΣΜΟΎ__ με το όνομα του λογαριασμού χώρου αποθήκευσης:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    Αυτό θα επιστρέψει το πρωτεύον κλειδί για το λογαριασμό.

Μπορείτε επίσης να βρείτε τις πληροφορίες του χώρου αποθήκευσης με την πύλη Azure:

1. Στην [Πύλη του Azure](https://portal.azure.com/), επιλέξτε το σύμπλεγμά σας HDInsight.

2. Από την ενότητα __βασικά στοιχεία__ , επιλέξτε __όλες τις ρυθμίσεις__.

3. Από τις __Ρυθμίσεις__, επιλέξτε __Azure κλειδιά χώρου αποθήκευσης__.

4. Από __Azure πλήκτρα χώρου αποθήκευσης__, επιλέξτε έναν από τους λογαριασμούς χώρου αποθήκευσης που παρατίθενται. Αυτό θα εμφανίσει πληροφορίες σχετικά με το λογαριασμό χώρου αποθήκευσης.

5. Επιλέξτε το εικονίδιο κλειδιού. Αυτό θα εμφανίσει πλήκτρα για αυτόν το λογαριασμό χώρου αποθήκευσης.

### <a name="how-do-i-access-blob-storage"></a>Πώς μπορώ να αποκτήσω πρόσβαση χώρος αποθήκευσης αντικειμένων Blob;

Εκτός από μέσω της εντολής Hadoop από το σύμπλεγμα, υπάρχουν διάφορους τρόπους για να αποκτήσετε πρόσβαση σε αντικείμενα BLOB:

* [Azure CLI για Mac, Linux και Windows](../xplat-cli-install.md): περιβάλλον γραμμής εντολών εντολές για την εργασία με Azure. Μετά την εγκατάσταση, χρησιμοποιήστε το `azure storage` εντολής για βοήθεια σχετικά με τη χρήση του χώρου αποθήκευσης, ή `azure blob` για συγκεκριμένη blob εντολές.

* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): python μια δέσμη ενεργειών για την εργασία με αντικείμενα blob στο χώρο αποθήκευσης Azure.

* Μια ποικιλία SDK:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Κείμενο φωνητικής γραφής](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [Χώρος αποθήκευσης REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>Κλιμάκωση συμπλέγματος

Το σύμπλεγμα κλίμακας δυνατότητα σάς επιτρέπει να αλλάξετε τον αριθμό των κόμβους δεδομένων που χρησιμοποιούνται από ένα σύμπλεγμα που εκτελείται στο Azure HDInsight χωρίς να χρειάζεται να διαγράψετε και να δημιουργήσετε ξανά το σύμπλεγμα.

Μπορείτε να εκτελέσετε λειτουργίες κλίμακας κατά άλλες εργασίες ή οι διεργασίες σε ένα σύμπλεγμα.

Οι τύποι διαφορετικό σύμπλεγμα επηρεάζονται από κλίμακας ως εξής:

* __Hadoop__: κατά την κλιμάκωση μετρά τον αριθμό των κόμβους σε ένα σύμπλεγμα, ορισμένες από τις υπηρεσίες του συμπλέγματος είναι επανεκκίνηση του. Αυτό μπορεί να προκαλέσει εργασίες χρησιμοποιείτε ή σε εκκρεμότητα αποτυχία κατά την ολοκλήρωση της λειτουργίας κλίμακας. Μπορείτε να υποβάλετε ξανά τις εργασίες μόλις ολοκληρωθεί η λειτουργία.

* __HBase__: οι τοπικοί διακομιστές είναι αυτόματα εξισορρόπηση μέσα σε λίγα λεπτά μετά την ολοκλήρωση της λειτουργίας κλίμακας. Για να εξισορροπήσετε με μη αυτόματο τρόπο οι τοπικοί διακομιστές, χρησιμοποιήστε τα ακόλουθα βήματα:

    1. Συνδεθείτε με το σύμπλεγμα HDInsight χρησιμοποιώντας SSH. Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με το HDInsight, ανατρέξτε σε ένα από τα ακόλουθα έγγραφα:

        * [Χρήση SSH με HDInsight από Linux, Unix και Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [Χρήση SSH με HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. Χρησιμοποιήστε τα ακόλουθα για να ξεκινήσετε το κέλυφος HBase:

            hbase shell

    2. Μόλις το κέλυφος HBase έχει φορτώσει, χρησιμοποιήστε τα ακόλουθα για να εξισορροπήσετε με μη αυτόματο τρόπο τους διακομιστές τοπικές ρυθμίσεις:

            balancer

* __Καταιγίδας__: μετά την εκτέλεση μιας λειτουργίας κλίμακας, πρέπει να νέα εξισορρόπηση οποιαδήποτε εκτελείται τοπολογίες καταιγίδας. Αυτό σας επιτρέπει την τοπολογία να ξαναρυθμίσετε παραλληλισμό ρυθμίσεις με βάση τον αριθμό του νέου από τους κόμβους του συμπλέγματος. Για νέα εξισορρόπηση εκτελείται τοπολογίες, χρησιμοποιήστε μία από τις ακόλουθες επιλογές:

    * __SSH__: σύνδεση με το διακομιστή και χρησιμοποιήστε την παρακάτω εντολή για να νέα εξισορρόπηση μια τοπολογία:

            storm rebalance TOPOLOGYNAME

        Μπορείτε επίσης να καθορίσετε τις παραμέτρους για να παρακάμψετε τη υποδείξεις παραλληλισμό αρχικά που παρέχεται από την τοπολογία. Για παράδειγμα, `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` θα ρυθμίσει ξανά τις παραμέτρους της τοπολογίας 5 διαδικασιών εργασίας, 3 executors για το στοιχείο μπλε-στομίου και 10 executors για το στοιχείο κίτρινο κεραυνό.

    * __Περιβάλλον εργασίας Χρήστη καταιγίδας__: ακολουθήστε τα παρακάτω βήματα για να νέα εξισορρόπηση μια τοπολογία χρησιμοποιώντας το περιβάλλον εργασίας Χρήστη καταιγίδας.

        1. Άνοιγμα __https://CLUSTERNAME.azurehdinsight.net/stormui__ στο πρόγραμμα περιήγησης web, όπου CLUSTERNAME είναι το όνομα του συμπλέγματος καταιγίδας. Εάν σας ζητηθεί, πληκτρολογήστε το όνομα του διαχειριστή (διαχειριστές) σύμπλεγμα HDInsight και τον κωδικό πρόσβασης που καθορίσατε κατά τη δημιουργία του συμπλέγματος.

        3. Επιλέξτε την τοπολογία που θέλετε να νέα εξισορρόπηση και, στη συνέχεια, κάντε κλικ στο κουμπί __νέα εξισορρόπηση__ . Πληκτρολογήστε την καθυστέρηση πριν από την εκτέλεση της λειτουργίας νέα εξισορρόπηση.

Για συγκεκριμένες πληροφορίες σχετικά με την κλιμάκωση το σύμπλεγμά σας HDInsight, ανατρέξτε στα θέματα:

* [Διαχείριση συμπλεγμάτων Hadoop στο HDInsight, χρησιμοποιώντας την πύλη του Azure](hdinsight-administer-use-portal-linux.md#scaling)

* [Διαχείριση συμπλεγμάτων Hadoop στο HDinsight με τη χρήση του PowerShell Azure](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Πώς μπορώ να εγκαταστήσω απόχρωση (ή άλλο στοιχείο Hadoop);

HDInsight είναι μια διαχειριζόμενη υπηρεσία, που σημαίνει ότι μπορεί να είναι καταστρέφονται και reprovisioned αυτόματα από Azure εάν εντοπίζεται ένα πρόβλημα κόμβους σε ένα σύμπλεγμα. Αυτόν το λόγο, δεν συνιστάται να εγκαταστήσετε με μη αυτόματο τρόπο στοιχεία απευθείας σε κόμβους συμπλέγματος. Εναλλακτικά, χρησιμοποιήστε [HDInsight δέσμης ενεργειών](hdinsight-hadoop-customize-cluster.md) όταν πρέπει να εγκαταστήσετε τα εξής:

* Μια υπηρεσία ή στην τοποθεσία web, όπως τους ή απόχρωση.
* Ένα στοιχείο που απαιτεί αλλαγές στη ρύθμιση παραμέτρων σε πολλούς κόμβους του συμπλέγματος. Για παράδειγμα, μια μεταβλητή περιβάλλοντος απαιτείται, τη δημιουργία ενός καταλόγου καταγραφής, ή τη δημιουργία ενός αρχείου ρύθμισης παραμέτρων.

Ενέργειες δέσμης ενεργειών είναι πάρτι δέσμες ενεργειών που είναι εκτελέσατε κατά την προμήθεια του συμπλέγματος και μπορεί να χρησιμοποιηθεί για την εγκατάσταση και ρύθμιση παραμέτρων πρόσθετα στοιχεία στο σύμπλεγμα. Παράδειγμα δέσμες ενεργειών παρέχονται για την εγκατάσταση των ακόλουθων στοιχείων:

* [Απόχρωση](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Για πληροφορίες σχετικά με τις δικές σας ενέργειες δέσμης ενεργειών ανάπτυξη, ανατρέξτε στο θέμα [Ανάπτυξη δέσμης ενεργειών με το HDInsight](hdinsight-hadoop-script-actions-linux.md).

###<a name="jar-files"></a>Βάζο αρχείων

Ορισμένες τεχνολογίες Hadoop παρέχονται στα αρχεία αυτόνομη βάζο που είναι περιέχουν συναρτήσεις που χρησιμοποιούνται ως μέρος μιας εργασίας MapReduce ή από μέσα σε γουρούνι ή η ομάδα. Ενώ αυτές μπορεί να εγκατασταθεί με χρήση δέσμης ενεργειών, συχνά δεν απαιτούν οποιαδήποτε ρύθμιση και να απλώς να που έχουν αποσταλεί στο σύμπλεγμα μετά την προμήθεια και χρησιμοποιούνται απευθείας. Εάν θέλετε να makle ότι το στοιχείο επιβιώνει reimaging του συμπλέγματος, μπορείτε να αποθηκεύσετε το αρχείο βάζο στο WASB.

Για παράδειγμα, εάν θέλετε να χρησιμοποιήσετε την πιο πρόσφατη έκδοση του [DataFu](http://datafu.incubator.apache.org/), να κάνετε λήψη μιας βάζο που περιέχει το έργο και στείλτε το στο σύμπλεγμα HDInsight. Στη συνέχεια, ακολουθήστε την τεκμηρίωση DataFu σχετικά με τον τρόπο για να το χρησιμοποιήσει από το γουρούνι ή η ομάδα.

> [AZURE.IMPORTANT] Ορισμένα στοιχεία που είναι μεμονωμένη βάζο αρχεία παρέχονται με το HDInsight, αλλά δεν είναι στη διαδρομή. Εάν αναζητάτε ένα συγκεκριμένο στοιχείο, μπορείτε να χρησιμοποιήσετε την παρακολούθηση για να το αναζητήσετε σε το σύμπλεγμά σας:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Αυτό θα επιστρέψει τη διαδρομή του τα αντίστοιχα αρχεία που βάζο.

Εάν το σύμπλεγμα παρέχει ήδη μια έκδοση ενός στοιχείου ως αρχείο βάζο μεμονωμένη, αλλά θέλετε να χρησιμοποιήσετε μια διαφορετική έκδοση, μπορείτε να στείλετε μια νέα έκδοση του στοιχείου στο σύμπλεγμα και προσπαθήστε να χρησιμοποιήσετε σε εργασίες σας.

> [AZURE.WARNING] Στοιχεία που παρέχονται με το σύμπλεγμα HDInsight υποστηρίζονται πλήρως και υποστήριξη της Microsoft θα σας βοηθήσει να απομονώσετε και να επιλύσετε θέματα που σχετίζονται με αυτά τα στοιχεία.
>
> Προσαρμοσμένα στοιχεία λαμβάνουν εμπορικά λογικές υποστήριξη για να σας βοηθήσει να αντιμετωπίσετε περαιτέρω το ζήτημα. Αυτό μπορεί να έχει ως αποτέλεσμα το ζήτημα επίλυση ή που σας ζητά να συμμετάσχουν διαθέσιμα κανάλια για τις τεχνολογίες Άνοιγμα αρχείου προέλευσης όπου βρίσκονται πολλά επίπεδα εξειδίκευση για συγκεκριμένη τεχνολογία. Για παράδειγμα, υπάρχουν πολλές τοποθεσίες Κοινότητας που μπορούν να χρησιμοποιηθούν, όπως: [φόρουμ MSDN για το HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Επίσης, Apache έργα έχουν τοποθεσίες έργου στο [http://apache.org](http://apache.org), για παράδειγμα: [Hadoop](http://hadoop.apache.org/), [τους](http://spark.apache.org/).

## <a name="next-steps"></a>Επόμενα βήματα

* [Μετεγκατάσταση από το HDInsight που βασίζονται σε Windows στο βάσει Linux](hdinsight-migrate-from-windows-to-linux.md)
* [Χρήση της ομάδας με το HDInsight](hdinsight-use-hive.md)
* [Χρήση γουρούνι με HDInsight](hdinsight-use-pig.md)
* [Χρήση MapReduce εργασίες με το HDInsight](hdinsight-use-mapreduce.md)
