<properties
    pageTitle="Δημιουργία συμπλεγμάτων HBase σε ένα εικονικό δίκτυο | Microsoft Azure"
    description="Γρήγορα αποτελέσματα χρησιμοποιώντας HBase στο Azure HDInsight. Μάθετε πώς να δημιουργείτε HDInsight HBase συμπλεγμάτων Azure εικονικού δικτύου."
    keywords=""
    services="hdinsight,virtual-network"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>Δημιουργία συμπλεγμάτων HBase στο Azure εικονικού δικτύου 

Μάθετε πώς να δημιουργείτε συμπλεγμάτων Azure HDInsight HBase σε ένα [Δίκτυο εικονικού Azure][1].

Με ενσωμάτωση του εικονικού δικτύου, συμπλεγμάτων HBase μπορεί να αναπτυχθεί το ίδιο εικονικό δίκτυο με τις εφαρμογές σας, έτσι ώστε οι εφαρμογές μπορούν να επικοινωνούν με HBase απευθείας. Τα πλεονεκτήματα είναι:

- Απευθείας σύνδεση της εφαρμογής web για να τους κόμβους του συμπλέγματος HBase, που επιτρέπει την επικοινωνία μέσω HBase Java απομακρυσμένης διαδικασίας κλήσης APIs (RPC).
- Βελτιωμένες επιδόσεις, δεν χρειάζεται κυκλοφορία σας μεταβείτε πάνω από πολλές πύλες και φόρτωση balancers.
- Η δυνατότητα να επεξεργαστεί ευαίσθητων πληροφοριών με πιο ασφαλή τρόπο, χωρίς να εκθέσετε μια δημόσια τελικού σημείου.

###<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Μια σταθμούς εργασίας με το Azure PowerShell**. Ανατρέξτε στο θέμα [εγκατάσταση και χρήση του PowerShell Azure](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/). 

## <a name="create-hbase-cluster-into-virtual-network"></a>Δημιουργία HBase σύμπλεγμα σε εικονικού δικτύου

Σε αυτήν την ενότητα, θα δημιουργήσετε ένα σύμπλεγμα βάσει Linux HBase με το λογαριασμό εξαρτώμενα αποθήκευσης Azure σε μια Azure εικονικού δικτύου, χρησιμοποιώντας ένα [πρότυπο από διαχειριστή πόρων Azure](../resource-group-template-deploy.md). Για άλλες μεθόδους δημιουργίας σύμπλεγμα και κατανόηση τις ρυθμίσεις, ανατρέξτε στο θέμα [Δημιουργία HDInsight συμπλεγμάτων](hdinsight-hadoop-provision-linux-clusters.md). Για περισσότερες πληροφορίες σχετικά με τη χρήση ενός προτύπου για να δημιουργήσετε συμπλεγμάτων Hadoop σε HDInsight, ανατρέξτε στο θέμα [Δημιουργία Hadoop συμπλεγμάτων σε με τη χρήση προτύπων από διαχειριστή πόρων Azure HDInsight](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [AZURE.NOTE] Ορισμένες ιδιότητες έχουν σχεδιασμένου στο πρότυπο. Για παράδειγμα:
>
> * __Θέση__: 2 Ανατολικής η.π.α.
> * Έκδοση __Cluster: 3.4
> * __Καταμέτρηση κόμβο εργαζόμενου σύμπλεγμα__: 4
> * __Προεπιλεγμένος χώρος αποθήκευσης λογαριασμός__: &lt;όνομα συμπλέγματος > Αποθήκευση
> * __Εικονική όνομα δικτύου__: &lt;όνομα συμπλέγματος >-vnet
> * __Εικονική χώρου διευθύνσεων δικτύου__: 10.0.0.0/16
> * __Όνομα δευτερεύοντος δικτύου__: προεπιλογή
> * __Περιοχή υποδικτύου διευθύνσεων__: 10.0.0.0/24
>
> &lt;Σύμπλεγμα όνομα > θα αντικατασταθεί με το όνομα συμπλέγματος που παρέχετε κατά τη χρήση του προτύπου.

1. Κάντε κλικ στην παρακάτω εικόνα για να ανοίξετε το πρότυπο στην πύλη του Azure. Το πρότυπο βρίσκεται σε ένα κοντέινερ δημόσια blob. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Από την **Ανάπτυξη προσαρμοσμένης** blade, εισαγάγετε τα εξής:

    - **Συνδρομή**: Επιλέξτε μια συνδρομή Azure που χρησιμοποιούνται για τη δημιουργία του συμπλέγματος HDInsight, το εξαρτώμενα λογαριασμό χώρου αποθήκευσης και το Azure εικονικού δικτύου.
    - **Ομάδα πόρων**: Επιλέξτε **Δημιουργία νέου**και καθορίστε ένα νέο όνομα ομάδας πόρων.
    - **Θέση**: Επιλέξτε μια θέση για την ομάδα των πόρων.
    - **ClusterName**: Πληκτρολογήστε ένα όνομα για το σύμπλεγμα Hadoop που θα δημιουργήσετε.
    - **Σύμπλεγμα όνομα σύνδεσης και τον κωδικό πρόσβασης**: το προεπιλεγμένο όνομα σύνδεσης είναι **διαχειριστής**.
    - **SSH όνομα χρήστη και τον κωδικό πρόσβασης**: **sshuser**είναι το προεπιλεγμένο όνομα χρήστη.  Μπορείτε να τη μετονομάσετε. 
    - **Συμφωνώ με τους όρους και τις συνθήκες που αναφέρονται παραπάνω**: (επιλέξτε)
    

3. Κάντε κλικ στην επιλογή **αγορά**. Διαρκεί περίπου περίπου 20 λεπτά για να δημιουργήσετε ένα σύμπλεγμα. Αφού δημιουργηθεί το σύμπλεγμα, μπορείτε να επιλέξετε το σύμπλεγμα blade στην πύλη του για να το ανοίξετε.

Αφού ολοκληρώσετε το πρόγραμμα εκμάθησης, που μπορεί να θέλετε να διαγράψετε το σύμπλεγμα. Με το HDInsight, τα δεδομένα σας αποθηκεύονται στο χώρο αποθήκευσης Azure, ώστε να μπορείτε να διαγράψετε ένα σύμπλεγμα όταν δεν είναι σε χρήση. Επίσης, που θα χρεωθείτε για ένα σύμπλεγμα HDInsight, ακόμα και όταν δεν είναι σε χρήση. Επειδή οι χρεώσεις για το σύμπλεγμα είναι πολλές φορές περισσότερες από τις χρεώσεις για χώρο αποθήκευσης, έχει οικονομικών νόημα για να διαγράψετε συμπλεγμάτων όταν δεν είναι σε χρήση. Για τις οδηγίες της διαγραφής ένα σύμπλεγμα, ανατρέξτε στο θέμα [Διαχείριση Hadoop συμπλεγμάτων στο HDInsight, χρησιμοποιώντας την πύλη του Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

Για να ξεκινήσετε να εργάζεστε με το νέο σύμπλεγμα HBase, μπορείτε να χρησιμοποιήσετε τις διαδικασίες που βρέθηκε στο [Γρήγορα αποτελέσματα με τη χρήση HBase με Hadoop στο HDInsight](hdinsight-hbase-tutorial-get-started.md).

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Συνδεθείτε με το σύμπλεγμα HBase χρησιμοποιώντας HBase Java RPC APIs

1.  Δημιουργία μιας υποδομής ως μια εικονική μηχανή υπηρεσίας (IaaS) στο ίδιο δίκτυο εικονικού Azure και το ίδιο υποδίκτυο. Για οδηγίες σχετικά με τη δημιουργία ενός νέου εικονική μηχανή IaaS, ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή εκτελεί Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Όταν ακολουθείτε τα βήματα σε αυτό το έγγραφο, πρέπει να χρησιμοποιήσετε τα παρακάτω για τη ρύθμιση παραμέτρων δικτύου:

    - __Εικονική δικτύου__: &lt;όνομα συμπλέγματος >-vnet
    - __Υποδίκτυο__: προεπιλογή

    > [AZURE.IMPORTANT] Αντικατάσταση &lt;όνομα συμπλέγματος > με το όνομα που χρησιμοποιείται κατά τη δημιουργία του συμπλέγματος HDInsight στα προηγούμενα βήματα.

    Χρησιμοποιώντας αυτές τις τιμές θα ρυθμίσετε τις παραμέτρους η εικονική μηχανή για να χρησιμοποιήσετε την ίδια εικονικού δικτύου και το δευτερεύον ως το σύμπλεγμα HDInsight. Αυτό θα σας επιτρέψει να επικοινωνήσετε απευθείας με μεταξύ τους.

2.  Κατά τη χρήση μιας εφαρμογής Java για να συνδεθείτε με HBase απομακρυσμένα, πρέπει να χρησιμοποιήσετε το πλήρως προσδιορισμένο όνομα τομέα (FQDN). Για να προσδιορίσετε αυτό, πρέπει να λάβετε το επίθημα DNS συγκεκριμένης σύνδεσης του συμπλέγματος HBase. Για να το κάνετε αυτό, μπορείτε να χρησιμοποιήσετε μία από τις παρακάτω μεθόδους:

    * Χρησιμοποιήστε ένα πρόγραμμα περιήγησης Web για να πραγματοποιήσετε μια κλήση Ambari:
    
        Αναζήτηση για να https://&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / hosts?minimal_response = true. Να μετατραπεί σε ένα αρχείο JSON με τα προθέματα DNS.

    * Χρησιμοποιήστε την τοποθεσία Web Ambari:

        1. Αναζήτηση για να https://&lt;ClusterName >. azurehdinsight.net.
        2. Κάντε κλικ στην επιλογή **Hosts** από το επάνω μενού.

    * Χρήση καμπύλη για την πραγματοποίηση κλήσεων ΥΠΌΛΟΙΠΟ:

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        Στο παράθυρο δεδομένων σημειογραφίας αντικειμένων JavaScript (JSON) που επιστρέφονται, βρείτε την καταχώρηση "όνομα_κεντρικού_υπολογιστή". Αυτό θα περιέχει το FQDN για τους κόμβους του συμπλέγματος. Για παράδειγμα:

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        Το τμήμα της στην αρχή του ονόματος τομέα με το όνομα του συμπλέγματος είναι το επίθημα DNS. Για παράδειγμα, mycluster.b1.cloudapp.net.

    * Χρήση του Azure PowerShell
    
        Χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών Azure PowerShell για να καταχωρήσετε τη συνάρτηση **Get-ClusterDetail** , η οποία μπορεί να χρησιμοποιηθεί για να επιστρέψετε το επίθημα DNS:

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Μετά την εκτέλεση της δέσμης ενεργειών του PowerShell Azure, χρησιμοποιήστε την ακόλουθη εντολή για να επιστρέψετε το επίθημα DNS, χρησιμοποιώντας τη συνάρτηση **Get-ClusterDetail** . Καθορίστε το όνομα συμπλέγματος HDInsight HBase, όνομα διαχειριστή και κωδικό πρόσβασης διαχειριστή κατά τη χρήση αυτής της εντολής.

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        Αυτό θα επιστρέψει το επίθημα DNS. Για παράδειγμα, **yourclustername.b4.internal.cloudapp.net**.

    * Χρήση RDP
    
        Μπορείτε επίσης να χρησιμοποιήσετε απομακρυσμένης επιφάνειας εργασίας για να συνδεθείτε με το σύμπλεγμα HBase (θα συνδεθείτε στον κόμβο κεφαλής) και να εκτελέσετε **ipconfig** από μια γραμμή εντολών για να αποκτήσετε το επίθημα DNS. Για οδηγίες σχετικά με την ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας Protocol (RDP) και σύνδεση με το σύμπλεγμα χρησιμοποιώντας RDP, ανατρέξτε στο θέμα [Διαχείριση Hadoop συμπλεγμάτων στο με την πύλη Azure HDInsight][hdinsight-admin-portal].
        
        ![hdinsight.hbase.DNS.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Για να επαληθεύσετε ότι η εικονική μηχανή να επικοινωνήσετε με το σύμπλεγμα HBase, χρησιμοποιήστε την εντολή `ping headnode0.<dns suffix>` από την εικονική μηχανή. Για παράδειγμα, ping headnode0.mycluster.b1.cloudapp.net.

Για να χρησιμοποιήσετε αυτές τις πληροφορίες σε μια εφαρμογή Java, μπορείτε να ακολουθήσετε τα βήματα στο θέμα [Χρήση Maven για να δημιουργήσετε εφαρμογές Java που χρησιμοποιούν HBase με HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) για να δημιουργήσετε μια εφαρμογή του. Για να εγκαταστήσει την εφαρμογή σύνδεση σε έναν απομακρυσμένο διακομιστή HBase, τροποποιήστε το αρχείο **hbase-site.xml** σε αυτό το παράδειγμα για να χρησιμοποιήσετε το FQDN για Zookeeper. Για παράδειγμα:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με την επίλυση ονομάτων στο Azure εικονικού δίκτυα, συμπεριλαμβανομένων των πώς μπορείτε να χρησιμοποιήσετε το δικό σας διακομιστή DNS, ανατρέξτε στο θέμα [Επίλυση ονομάτων (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης μάθατε πώς μπορείτε να δημιουργήσετε ένα σύμπλεγμα HBase. Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Γρήγορα αποτελέσματα με το HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Ρύθμιση παραμέτρων αναπαραγωγής HBase HDInsight](hdinsight-hbase-geo-replication.md)
- [Δημιουργία συμπλεγμάτων Hadoop σε HDInsight](hdinsight-provision-clusters.md)
- [Γρήγορα αποτελέσματα με τη χρήση HBase με Hadoop σε HDInsight](hdinsight-hbase-tutorial-get-started.md)
- [Ανάλυση Twitter άποψη με HBase σε HDInsight](hdinsight-hbase-analyze-twitter-sentiment.md)
- [Επισκόπηση εικονικού δικτύου][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Παροχή λεπτομερειών για το νέο σύμπλεγμα HBase"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Χρησιμοποιήστε την ενέργεια δέσμη ενεργειών για να προσαρμόσετε ένα σύμπλεγμα HBase"

[azure-preview-portal]: https://portal.azure.com
