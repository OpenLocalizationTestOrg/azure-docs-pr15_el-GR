<properties 
   pageTitle="Ρύθμιση παραμέτρων HBase αλληλεπίδραση μεταξύ των δύο κέντρα δεδομένων | Microsoft Azure" 
   description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους HBase αναπαραγωγής σε κέντρα δεδομένων δύο και σχετικά με τις περιπτώσεις χρήσης για την αναπαραγωγή σύμπλεγμα." 
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
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>Ρύθμιση παραμέτρων HBase παν-αναπαραγωγής HDInsight

> [AZURE.SELECTOR]
- [Ρύθμιση παραμέτρων συνδεσιμότητας VPN](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [Ρύθμιση παραμέτρων του DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [Ρύθμιση παραμέτρων HBase αναπαραγωγής](hdinsight-hbase-geo-replication.md) 
 
Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους HBase αναπαραγωγής σε κέντρα δεδομένων δύο. Ορισμένες χρησιμοποιούν περιπτώσεις για αναπαραγωγή σύμπλεγμα περιλαμβάνουν:

- Δημιουργία αντιγράφων ασφαλείας και καταστροφή ανάκτησης
- Συγκέντρωση δεδομένων
- Κατανομή γεωγραφικά δεδομένα
- Κατάποσης δεδομένα Online σε συνδυασμό με ανάλυση δεδομένων χωρίς σύνδεση

Αναπαραγωγή σύμπλεγμα χρησιμοποιεί μεθοδολογία push προέλευσης. Ένα σύμπλεγμα HBase μπορεί να είναι ένα αρχείο προέλευσης, προορισμού, είτε να πραγματοποιήσει τους ρόλους και τα δύο ταυτόχρονα. Αναπαραγωγή είναι ασύγχρονης και ενδεχόμενες συνέπειας είναι ο στόχος της αναπαραγωγής. Όταν το αρχείο προέλευσης λαμβάνει επεξεργασίας σε μια οικογένεια στήλης με αλληλεπίδραση με δυνατότητα, αυτό επεξεργασία αντιγράφεται όλα συμπλεγμάτων προορισμού. Όταν γίνεται αναπαραγωγή δεδομένων από ένα σύμπλεγμα σε μια άλλη, παρακολουθούνται του συμπλέγματος προέλευσης και όλα συμπλεγμάτων που έχετε ήδη που καταναλώθηκε τα δεδομένα για να αποτρέψετε την επαναλήψεις αναπαραγωγής. Για περισσότερες πληροφορίες, σε αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να ρυθμίσετε μια αναπαραγωγή προέλευσης προορισμού.  Για άλλες τοπολογίες σύμπλεγμα, ανατρέξτε στο θέμα [Οδηγός αναφοράς HBase Apache](http://hbase.apache.org/book.html#_cluster_replication).

Αυτό είναι το τρίτο τμήμα της σειράς:

- [Ρύθμιση παραμέτρων μια σύνδεση VPN μεταξύ δύο εικονικού δικτύων][hdinsight-hbase-replication-vnet]
- [Ρύθμιση παραμέτρων DNS για το εικονικό δίκτυα][hdinsight-hbase-replication-dns]
- Ρύθμιση παραμέτρων αναπαραγωγής παν HBase (αυτό το πρόγραμμα εκμάθησης)

Το παρακάτω διάγραμμα παρουσιάζει τα δύο εικονικού δίκτυα και τη συνδεσιμότητα του δικτύου που δημιουργήσατε στο θέμα [Ρύθμιση παραμέτρων μια σύνδεση VPN μεταξύ δύο εικονικού δικτύων] [ hdinsight-hbase-geo-replication-vnet] και [Ρύθμιση παραμέτρων του DNS για το εικονικό δίκτυα][hdinsight-hbase-replication-dns]: 

![Διάγραμμα δικτύου εικονικού αναπαραγωγής HDInsight HBase][img-vnet-diagram]

## <a id="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Μια σταθμούς εργασίας με το Azure PowerShell**.

    Για την εκτέλεση δεσμών ενεργειών του PowerShell, πρέπει να εκτελέσετε Azure PowerShell ως διαχειριστής και ορίστε την πολιτική εκτέλεσης σε *RemoteSigned*. Ανατρέξτε στο θέμα χρησιμοποιώντας το cmdlet Set-της πολιτικής εκτέλεσης του.
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Δύο Azure εικονικού δικτύου με σύνδεση VPN και ρύθμιση των παραμέτρων DNS**.  Για οδηγίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μια σύνδεση VPN μεταξύ δύο δικτύων Azure εικονικό][hdinsight-hbase-replication-vnet], και [Ρύθμιση παραμέτρων DNS μεταξύ δύο δικτύων Azure εικονικό][hdinsight-hbase-replication-dns].


    Πριν από την εκτέλεση δεσμών ενεργειών του PowerShell, βεβαιωθείτε ότι είστε συνδεδεμένοι στη συνδρομή σας Azure χρησιμοποιώντας το ακόλουθο cmdlet:

        Add-AzureAccount

    Εάν έχετε πολλές συνδρομές Azure, χρησιμοποιήστε το ακόλουθο cmdlet για να ορίσετε την τρέχουσα συνδρομή σας:

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>Προμήθεια συμπλεγμάτων HBase στο HDInsight

Στο θέμα [Ρύθμιση παραμέτρων μια σύνδεση VPN μεταξύ δύο δικτύων Azure εικονικού][hdinsight-hbase-replication-vnet], έχετε δημιουργήσει ένα εικονικό δίκτυο στην Ευρώπη ένα κέντρο δεδομένων και, ένα εικονικό δίκτυο σε ένα κέντρο δεδομένων η.π.α.. Τα δύο εικονικού δικτύου είναι συνδεδεμένο μέσω VPN. Σε αυτήν την περίοδο λειτουργίας, θα προμήθεια ένα σύμπλεγμα HBase σε κάθε ένα από τα δίκτυα εικονικού. Παρακάτω σε αυτό το πρόγραμμα εκμάθησης, θα κάνετε ένα από το συστοιχίες HBase για να αναπαραγάγετε το άλλο σύμπλεγμα HBase.

Η πύλη κλασική Azure δεν υποστηρίζει παροχής συμπλεγμάτων HDInsight με προσαρμοσμένη ρύθμιση παραμέτρων επιλογών. Για παράδειγμα, μπορείτε να ορίσετε *hbase.replication* στην *τιμή true*. Εάν ορίσετε την τιμή στο αρχείο παραμέτρων μετά την παροχή της υπηρεσίας ένα σύμπλεγμα, θα χάσετε τη ρύθμιση μετά το σύμπλεγμα είναι που reimaged. Για περισσότερες πληροφορίες, ανατρέξτε στην [παροχή Hadoop συμπλεγμάτων στο HDInsight][hdinsight-provision]. Μία από τις επιλογές για την προμήθεια σύμπλεγμα HDInsight με προσαρμοσμένες επιλογές χρησιμοποιεί Azure PowerShell.


**Για την παροχή των ένα σύμπλεγμα HBase Contoso-VNet-ΕΕ** 

1. Από σταθμούς εργασίας σας, ανοίξτε το Windows PowerShell ISE.
2. Ορίστε τις μεταβλητές κατά την έναρξη της δέσμης ενεργειών και, στη συνέχεια, εκτελέστε τη δέσμη ενεργειών.

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**Για την παροχή των ένα σύμπλεγμα HBase στις Contoso-VNet-η.π.α.** 

- Χρησιμοποιήστε την ίδια δέσμη ενεργειών με τις εξής τιμές:

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    Επειδή έχετε ήδη συνδεδεμένοι στο λογαριασμό σας Azure, δεν χρειάζεται να εκτελέσετε τα παρακάτω comlets πλέον:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>Ρύθμιση παραμέτρων DNS προώθησης υπό όρους

Στη [Ρύθμιση παραμέτρων του DNS για το εικονικό δίκτυα][hdinsight-hbase-replication-dns], έχετε ρυθμίσει τις παραμέτρους των διακομιστών DNS για τα δύο δίκτυα. Των συμπλεγμάτων HBase έχουν προθέματα διαφορετικό τομέα. Πρέπει να ρυθμίσετε τις παραμέτρους επιπλέον υπό όρους προωθήσεις DNS.

Για να ρυθμίσετε τις παραμέτρους προώθησης υπό όρους, πρέπει να γνωρίζετε τα δύο συμπλεγμάτων HBase προθέματα τομέα. 

**Για να βρείτε προθέματα τομέα του των δύο HBase συμπλεγμάτων**

1. RDP σε **Contoso-HBase-ΕΕ**.  Για οδηγίες, ανατρέξτε στο θέμα [Διαχείριση Hadoop συμπλεγμάτων στο HDInsight, χρησιμοποιώντας την πύλη κλασική Azure][hdinsight-manage-portal]. Είναι στην πραγματικότητα headnode0 του συμπλέγματος.
2. Ανοίξτε μια κονσόλα του Windows PowerShell ή μια γραμμή εντολών.
3. Εκτελέστε **ipconfig**και σημειώστε **επίθημα DNS συγκεκριμένης σύνδεσης**.
4. Δεν κλείστε την περίοδο λειτουργίας RDP.  Θα το χρειαστείτε αργότερα για να ελέγξετε ανάλυση ονόματος τομέα.
5. Επαναλάβετε τα ίδια βήματα για να βρείτε το **επίθημα DNS συγκεκριμένης σύνδεσης** του **Contoso-HBase-ΗΠΑ**.


**Για να ρυθμίσετε τις παραμέτρους προώθησης DNS**
 
1.  RDP σε **Contoso-DNS-ΕΕ**. 
2.  Κάντε κλικ στο πλήκτρο των Windows στην κάτω αριστερή γωνία.
2.  Κάντε κλικ στην επιλογή **Εργαλεία διαχείρισης**.
3.  Κάντε κλικ στην επιλογή **DNS**.
4.  Στο αριστερό παράθυρο, αναπτύξτε **DSN**, **Contoso-DNS-ΕΕ**.
5.  Κάντε δεξί κλικ **Προωθήσεων υπό όρους**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέα προώθησης υπό όρους**. 
5.  Εισαγάγετε τις παρακάτω πληροφορίες:
    - **Τομέα DNS**: Εισαγάγετε το επίθημα DNS του Contoso-HBase-Ηνωμένες Πολιτείες. Για παράδειγμα: Contoso-HBase-US.f5.internal.cloudapp.net.
    - **Διευθύνσεις IP των κύριων διακομιστών**: Εισαγάγετε 10.2.0.4, που είναι η διεύθυνση IP Contoso-DNS-Ηνωμένες Πολιτείες του. Βεβαιωθείτε ότι το IP. Διακομιστή DNS μπορεί να έχει διαφορετική διεύθυνση IP.
6.  Πατήστε το πλήκτρο **ENTER**και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.  Τώρα θα μπορεί να επιλύσει Contoso-DNS-Ηνωμένες Πολιτείες της διεύθυνσης IP από Contoso-DNS-ΕΕ.
7.  Επαναλάβετε τα βήματα για να προσθέσετε υπό όρους προώθηση DNS στην υπηρεσία DNS στον υπολογιστή εικονικές Contoso-DNS-η.π.α., με τις ακόλουθες τιμές:
    - **Τομέα DNS**: Εισαγάγετε το επίθημα DNS της Contoso-HBase-ΕΕ. 
    - **Διευθύνσεις IP των κύριων διακομιστών**: Εισαγάγετε 10.2.0.4, που είναι η διεύθυνση IP Contoso-DNS-της Ευρωπαϊκής Ένωσης.

**Για να ελέγξετε ανάλυση ονόματος τομέα**

1. Μετάβαση στο παράθυρο RDP Contoso-HBase-ΕΕ.
2. Ανοίξτε μια γραμμή εντολών.
3. Εκτελέστε την εντολή ping:

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    Το πρωτόκολλο Επιλογή είναι ενεργοποιημένη από τους κόμβους εργαζόμενου το HBase συμπλεγμάτων

4. Μην κλείσετε την περίοδο λειτουργίας RDP. Θα εξακολουθούν να το χρειαστείτε αργότερα στην εκμάθηση.
5. Επαναλάβετε τα ίδια βήματα για να κάνετε ping το headnode0 της Contoso-HBase-ΕΕ από τις ΗΠΑ HBase Contoso.

>[AZURE.IMPORTANT] DNS πρέπει να εργαστείτε πριν να συνεχίσετε με τα επόμενα βήματα.

## <a name="enable-replication-between-hbase-tables"></a>Ενεργοποίηση της αναπαραγωγής μεταξύ HBase πινάκων

Τώρα, μπορείτε να δημιουργήσετε ένα δείγμα πίνακα HBase, να ενεργοποιήσετε την αναπαραγωγή και, στη συνέχεια, να το δοκιμάσετε με ορισμένα δεδομένα. Το δείγμα πίνακα που θα χρησιμοποιήσετε έχει δύο οικογένειες στήλης: προσωπικό και το Office. 

Σε αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να κάνετε το σύμπλεγμα Europe HBase ως του συμπλέγματος προέλευσης και του συμπλέγματος ΗΠΑ HBase ως το σύμπλεγμα προορισμού.

Δημιουργία πινάκων HBase με τα ίδια ονόματα και οι οικογένειες στήλης στην συμπλεγμάτων προέλευσης και προορισμού, ώστε να γνωρίζει το σύμπλεγμα προορισμού θέση για την αποθήκευση δεδομένων, αυτό θα λάβουν. Για περισσότερες πληροφορίες σχετικά με το κέλυφος HBase, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Apache HBase στο HDInsight][hdinsight-hbase-get-started].

**Για να δημιουργήσετε έναν πίνακα HBase στην Contoso-HBase-ΕΕ**

1. Μετάβαση στο παράθυρο RDP **Contoso-HBase-ΕΕ** .
2. Από την επιφάνεια εργασίας, κάντε κλικ στην επιλογή **γραμμή εντολών Hadoop**.
2. Αλλαγή του φακέλου στον κεντρικό κατάλογο HBase:

        cd %HBASE_HOME%\bin
3. Ανοίξτε το κέλυφος HBase:

        hbase shell
4. Δημιουργία ενός πίνακα HBase:

        create 'Contacts', 'Personal', 'Office'
5. Μην κλείσετε την περίοδο λειτουργίας RDP ούτε το παράθυρο γραμμής εντολών Hadoop. Εξακολουθείτε να χρειάζεστε τους αργότερα στο πρόγραμμα εκμάθησης.
    
**Για να δημιουργήσετε έναν πίνακα HBase στην Contoso-HBase-η.π.α.**

- Επαναλάβετε τα ίδια βήματα για να δημιουργήσετε τον ίδιο πίνακα στην Contoso-HBase-ΗΠΑ.


**Για να προσθέσετε Contoso-HBase-ΗΠΑ ως ένα ομότιμων αναπαραγωγής**

1. Μετάβαση στο παράθυρο RDP **Contso HBase_EU** .
2. Από το παράθυρο κελύφους HBase, προσθέστε το σύμπλεγμα προορισμού (Contoso-HBase-η.π.α.) ως ένα ομότιμης σύνδεσης, για παράδειγμα:

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    Στο δείγμα, το επίθημα τομέα είναι *contoso-hbase-us.d4.internal.cloudapp.net*. Πρέπει να ενημερώσετε ώστε να ταιριάζει με το επίθημα τομέα του συμπλέγματος HBase ΜΑΣ. Βεβαιωθείτε ότι υπάρχει χωρίς κενά διαστήματα μεταξύ τα ονόματα.

**Για να ρυθμίσετε την οικογένειά κάθε στήλης να αναπαραχθεί στο σύμπλεγμα προέλευσης**

1. Από το παράθυρο κελύφους HBase από την περίοδο λειτουργίας RDP **Contso-HBase-ΕΕ** , ρυθμίστε τις παραμέτρους κάθε στήλη οικογένεια να αναπαραχθεί:

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**Για τη μαζική αποστολή δεδομένων στον πίνακα HBase**

Ένα δείγμα αρχείου δεδομένων έχει αποσταλεί σε δημόσια κοντέινερ αντικειμένων Blob του Azure με την παρακάτω διεύθυνση URL:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

Το περιεχόμενο του αρχείου:

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

Μπορείτε να αποστείλετε το αρχείο δεδομένων του ίδιου σε το σύμπλεγμά σας HBase και να εισαγάγετε τα δεδομένα από εκεί.

1. Μετάβαση στο παράθυρο RDP **Contoso-HBase-ΕΕ** .
2. Από την επιφάνεια εργασίας, κάντε κλικ στην επιλογή **γραμμή εντολών Hadoop**.
3. Αλλαγή του φακέλου στον κεντρικό κατάλογο HBase:

        cd %HBASE_HOME%\bin

4. αποστολή των δεδομένων:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>Βεβαιωθείτε ότι η αναπαραγωγή δεδομένων θα πραγματοποιηθεί

Μπορείτε να επαληθεύσετε ότι η αναπαραγωγή γίνεται με τους πίνακες από δύο συμπλεγμάτων με τις παρακάτω εντολές κελύφους HBase σάρωση:

        Scan 'Contacts'


## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς να ρυθμίσετε τις παραμέτρους HBase αναπαραγωγής σε δύο κέντρα δεδομένων. Για να μάθετε περισσότερα σχετικά με το HDInsight και HBase, ανατρέξτε στα θέματα:

- [Σελίδα υπηρεσίας HDInsight](https://azure.microsoft.com/services/hdinsight/)
- [Τεκμηρίωση HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)
- [Γρήγορα αποτελέσματα με το HBase Apache στο HDInsight][hdinsight-hbase-get-started]
- [Επισκόπηση HDInsight HBase][hdinsight-hbase-overview]
- [Προμήθεια HBase συμπλεγμάτων σε Azure εικονικού δικτύου][hdinsight-hbase-provision-vnet]
- [Ανάλυση σε πραγματικό χρόνο Twitter άποψη με HBase][hdinsight-hbase-twitter-sentiment]
- [Την ανάλυση δεδομένων αισθητήρα με καταιγίδας και HBase στο HDInsight (Hadoop)][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
