<properties
    pageTitle="Αποστολή δεδομένων για τις εργασίες Hadoop σε HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να στείλετε και να έχουν πρόσβαση σε δεδομένα για τα έργα Hadoop στο HDInsight χρησιμοποιώντας το CLI Azure, Azure Εξερεύνηση χώρου αποθήκευσης, Azure PowerShell, τη γραμμή εντολών Hadoop ή Sqoop."
    services="hdinsight,storage"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Αποστολή δεδομένων για τις εργασίες Hadoop στο HDInsight

Azure HDInsight παρέχει μια ολοκληρωμένη Κατανεμημένο σύστημα αρχείων Hadoop (HDFS) επάνω από το χώρο αποθήκευσης αντικειμένων Blob του Azure. Έχει σχεδιαστεί ως επέκταση HDFS να παρέχει μια απρόσκοπτη εμπειρία στους πελάτες. Ενεργοποιεί το πλήρες σύνολο των στοιχείων στην στο περιβάλλον εμπορικής προσαρμογής Hadoop για τη λειτουργία απευθείας στα δεδομένα που διαχειρίζεται. Azure χώρος αποθήκευσης αντικειμένων Blob και HDFS είναι συστήματα διακριτές αρχείων που είναι βελτιστοποιημένες για το χώρο αποθήκευσης των δεδομένων και υπολογισμούς σε αυτά τα δεδομένα. Για πληροφορίες σχετικά με τα οφέλη της χρήσης χώρος αποθήκευσης αντικειμένων Blob του Azure, ανατρέξτε στο θέμα [χώρο αποθήκευσης αντικειμένων Blob του Azure χρήση με το HDInsight][hdinsight-storage].

**Προαπαιτούμενα στοιχεία**

Πριν ξεκινήσετε, λάβετε υπόψη τις ακόλουθες απαιτήσεις:

* Ένα σύμπλεγμα Azure HDInsight. Για οδηγίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure HDInsight] [ hdinsight-get-started] ή [HDInsight παροχή συμπλεγμάτων][hdinsight-provision].

##<a name="why-blob-storage"></a>Γιατί αντικειμένων blob χώρο αποθήκευσης;

Azure HDInsight συμπλεγμάτων αναπτύσσονται συνήθως για να εκτελέσετε εργασίες MapReduce και των συμπλεγμάτων χάνονται μετά την ολοκλήρωση αυτών των εργασιών. Διατήρηση των δεδομένων στην το HDFS συμπλεγμάτων μετά την ολοκλήρωση υπολογισμούς θα ήταν μια ακριβή τρόπος για να αποθηκεύσετε αυτά τα δεδομένα. Χώρο αποθήκευσης Blob του Azure είναι μια ιδιαίτερα διαθέσιμη, ιδιαίτερα μεταβλητού μεγέθους, υψηλή χωρητικότητα, επιλογή χαμηλό κόστος και κοινόχρηστο χώρο αποθήκευσης για τα δεδομένα που είναι προς επεξεργασία χρησιμοποιώντας HDInsight. Αποθήκευση δεδομένων σε ένα αντικείμενο blob επιτρέπει των συμπλεγμάτων HDInsight που χρησιμοποιούνται για υπολογισμό να κυκλοφορούν με ασφάλεια χωρίς να χάσετε δεδομένα.

###<a name="directories"></a>Σε καταλόγους

Azure χώρους αποθήκευσης αντικειμένων Blob αποθήκευση δεδομένων ως ζεύγη κλειδιού/τιμής και δεν υπάρχει καμία ιεραρχία καταλόγου. Ωστόσο μπορεί να χρησιμοποιηθεί ο χαρακτήρας "/" μέσα το όνομα του κλειδιού ώστε να φαίνεται σαν να είναι αποθηκευμένο ένα αρχείο μέσα σε μια δομή καταλόγου. HDInsight βλέπει αυτά σαν να είναι πραγματικές καταλόγων.

Για παράδειγμα, ένα blob κλειδί ενδέχεται να είναι *input/log1.txt*. Δεν υπάρχει πραγματική "εισαγωγής" κατάλογος υπάρχει, αλλά λόγω της παρουσίας του χαρακτήρα "/" στο όνομα του κλειδιού, που έχει την εμφάνιση της διαδρομής αρχείου.

Αυτόν το λόγο, εάν χρησιμοποιείτε τα εργαλεία Azure Explorer ενδέχεται να παρατηρήσετε ορισμένα αρχεία 0 byte. Αυτά τα αρχεία εξυπηρετούν δύο σκοπούς:

- Εάν υπάρχουν κενούς φακέλους, επισημάνετε την ύπαρξη του φακέλου. Χώρο αποθήκευσης Blob του Azure είναι αρκετά Έξυπνες για να γνωρίζετε ότι εάν υπάρχει ένα blob που ονομάζεται foo/γραμμή, υπάρχει ένα φάκελο που ονομάζεται **foo**. Αλλά ο μόνος τρόπος για να δείχνει έναν κενό φάκελο που ονομάζεται **foo** με από αυτό το αρχείο ειδική 0 byte στη θέση.

- Διαθέτουν ειδική μετα-δεδομένων που απαιτείται από το σύστημα αρχείων Hadoop, ιδίως τα δικαιώματα και οι κάτοχοι για τους φακέλους.

##<a name="command-line-utilities"></a>Βοηθητικά προγράμματα γραμμής εντολών

Η Microsoft παρέχει τα ακόλουθα βοηθητικά προγράμματα για να εργαστείτε με το χώρο αποθήκευσης αντικειμένων Blob του Azure:

| Εργαλείο | Linux | ΛΕΙΤΟΥΡΓΙΚΌ ΣΎΣΤΗΜΑ OS X | Windows |
| ---- |:-----:|:----:|:-------:|
| [Azure περιβάλλον γραμμής εντολών][azurecli] | ✔ | ✔ | ✔ |
| [Azure PowerShell][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Εντολή Hadoop](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] Ενώ το Azure CLI Azure PowerShell και AzCopy να όλα να χρησιμοποιηθούν από το Azure έξω, το Hadoop, η εντολή είναι διαθέσιμη μόνο από το HDInsight σύμπλεγμα και μόνο επιτρέπει τη φόρτωση των δεδομένων από το τοπικό σύστημα αρχείων στο χώρο αποθήκευσης αντικειμένων Blob του Azure.

###<a id="xplatcli"></a>Azure CLI

Το Azure CLI είναι ένα εργαλείο πλατφόρμες που σας επιτρέπει να διαχειριστείτε τις υπηρεσίες Azure. Χρησιμοποιήστε τα ακόλουθα βήματα για την αποστολή δεδομένων στο χώρο αποθήκευσης αντικειμένων Blob του Azure:

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI για Mac, Linux και Windows](../xplat-cli-install.md).

2. Ανοίξτε μια γραμμή εντολών, πάρτι ή άλλες κελύφους και χρησιμοποιήστε τα ακόλουθα για τον έλεγχο ταυτότητας στη συνδρομή σας στο Azure.

        azure login

    Όταν σας ζητηθεί, πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης για τη συνδρομή σας.

3. Πληκτρολογήστε την παρακάτω εντολή για να παραθέσετε τους λογαριασμούς χώρου αποθήκευσης για τη συνδρομή σας:

        azure storage account list

4. Επιλέξτε το λογαριασμό χώρου αποθήκευσης που περιέχει το αντικείμενο blob που θέλετε να εργαστείτε και, στη συνέχεια, χρησιμοποιήστε την ακόλουθη εντολή για να ανακτήσετε το κλειδί για αυτόν το λογαριασμό:

        azure storage account keys list <storage-account-name>

    Αυτό πρέπει να επιστρέψει **πρωτεύοντα** και **δευτερεύοντα** κλειδιά. Αντιγράψτε την τιμή **πρωτεύοντος** κλειδιού, επειδή θα χρησιμοποιηθεί στα επόμενα βήματα.

5. Χρησιμοποιήστε την ακόλουθη εντολή για να ανακτήσετε μια λίστα των αντικειμένων blob κοντέινερ εντός του λογαριασμού χώρου αποθήκευσης:

        azure storage container list -a <storage-account-name> -k <primary-key>

6. Χρησιμοποιήστε τις παρακάτω εντολές για την αποστολή και λήψη αρχείων για το αντικείμενο blob:

    * Για να αποστείλετε ένα αρχείο:

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * Για να κάνετε λήψη ενός αρχείου:

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Εάν πάντα θα λειτουργεί με τον ίδιο λογαριασμό χώρου αποθήκευσης, μπορείτε να ορίσετε τις παρακάτω μεταβλητές περιβάλλοντος αντί για τον καθορισμό του λογαριασμού και κλειδί για κάθε εντολή:
>
> * **AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ΛΟΓΑΡΙΑΣΜΟΎ**: το όνομα του λογαριασμού χώρου αποθήκευσης
>
> * **AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ACCESS\_ΚΛΕΙΔΊ**: το κλειδί λογαριασμού χώρου αποθήκευσης

###<a id="powershell"></a>Azure PowerShell

Azure PowerShell είναι ένα περιβάλλον δέσμης ενεργειών που μπορείτε να χρησιμοποιήσετε για να ελέγξετε και να αυτοματοποιήσετε την ανάπτυξη και τη διαχείριση της σας φόρτους εργασίας στο Azure. Για πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων σας σταθμούς εργασίας για να εκτελέσετε Azure PowerShell, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../powershell-install-configure.md).

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Για να αποστείλετε ένα τοπικό αρχείο στο χώρο αποθήκευσης αντικειμένων Blob του Azure**

1. Ανοίξτε την κονσόλα Azure PowerShell, σύμφωνα με τις οδηγίες στο [εγκατάσταση και ρύθμιση παραμέτρων Azure PowerShell](../powershell-install-configure.md).
2. Ορίστε τις τιμές από τις πέντε πρώτες μεταβλητές σε την ακόλουθη δέσμη ενεργειών:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. Επικολλήστε τη δέσμη ενεργειών στην κονσόλα Azure PowerShell για να εκτελέσετε για να αντιγράψετε το αρχείο.

Για παράδειγμα δεσμών ενεργειών του PowerShell που δημιουργήσατε για να εργαστείτε με το HDInsight, ανατρέξτε στο θέμα [Εργαλεία HDInsight](https://github.com/blackmist/hdinsight-tools).

###<a id="azcopy"></a>AzCopy

AzCopy είναι ένα εργαλείο γραμμής εντολών που έχει σχεδιαστεί για να απλοποιήσετε την εργασία τη μεταφορά δεδομένων σε και έξοδος από το χώρο αποθήκευσης Azure λογαριασμού. Μπορείτε να χρησιμοποιήσετε ως ένα εργαλείο αυτόνομης υπηρεσίας ή να ενσωματώσετε αυτό το εργαλείο σε μια υπάρχουσα εφαρμογή. [Λήψη AzCopy][azure-azcopy-download].

Η σύνταξη AzCopy είναι:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [AzCopy - αποστολή και λήψη αρχείων για αντικείμενα BLOB Azure][azure-azcopy].


###<a id="commandline"></a>Γραμμή εντολών Hadoop

Γραμμή εντολών Hadoop είναι χρήσιμο για την αποθήκευση δεδομένων στο χώρο αποθήκευσης αντικειμένων blob όταν τα δεδομένα υπάρχει ήδη στον κόμβο κεφαλής συμπλέγματος μόνο.

Για να χρησιμοποιήσετε την εντολή Hadoop, πρέπει πρώτα να συνδεθείτε για να το headnode χρησιμοποιώντας μία από τις παρακάτω μεθόδους:

* **HDInsight που βασίζεται σε Windows**: [σύνδεση με χρήση απομακρυσμένης επιφάνειας εργασίας](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **Βασίζεται στο Linux HDInsight**: συνδεθείτε χρησιμοποιώντας SSH ([η εντολή SSH](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster) ή [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster))

Μετά τη σύνδεση, μπορείτε να χρησιμοποιήσετε την παρακάτω σύνταξη για να αποστείλετε ένα αρχείο με το χώρο αποθήκευσης.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Για παράδειγμα,`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Επειδή το προεπιλεγμένο σύστημα αρχείων για το HDInsight βρίσκεται στο χώρο αποθήκευσης αντικειμένων Blob του Azure, /example/data.txt είναι στην πραγματικότητα στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Μπορείτε επίσης να ανατρέξετε στο αρχείο ως:

    wasbs:///example/data/data.txt

ή

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Για μια λίστα των άλλων Hadoop εντολές που λειτουργούν με τα αρχεία, ανατρέξτε στο θέμα [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [AZURE.WARNING] Στην HBase συμπλεγμάτων, η προεπιλεγμένη αποκλεισμός μέγεθος που χρησιμοποιήθηκε κατά την εγγραφή δεδομένων είναι 256KB. Ενώ αυτό λειτουργεί καλά, κατά τη χρήση HBase APIs ή REST API, χρησιμοποιώντας το `hadoop` ή `hdfs dfs` εντολές για να γράψετε μεγαλύτερο από ~ 12GB δεδομένων έχει ως αποτέλεσμα ένα σφάλμα. Ανατρέξτε στην ενότητα [εξαίρεση χώρου αποθήκευσης για εγγραφή στην blob](#storageexception) παρακάτω για περισσότερες πληροφορίες.

##<a name="graphical-clients"></a>Προγράμματα-πελάτες του γραφικού

Υπάρχουν επίσης πολλές εφαρμογές που παρέχουν ένα γραφικό περιβάλλον εργασίας για την εργασία με το χώρο αποθήκευσης Azure. Ακολουθεί μια λίστα με κάποιες από αυτές τις εφαρμογές:

| Πρόγραμμα-πελάτη | Linux | ΛΕΙΤΟΥΡΓΙΚΌ ΣΎΣΤΗΜΑ OS X | Windows |
| ------ |:-----:|:----:|:-------:|
| [Εργαλεία του Microsoft Visual Studio για HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Εξερεύνηση Azure χώρου αποθήκευσης](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [Studio χώρο αποθήκευσης στο cloud 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Εξερεύνηση Azure](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>Εργαλεία του Visual Studio για HDInsight

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Περιήγηση συνδεδεμένων πόρων](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

###<a id="storageexplorer"></a>Εξερεύνηση Azure χώρου αποθήκευσης

*Εξερεύνηση χώρου αποθήκευσης Azure* είναι χρήσιμο εργαλείο για έλεγχο και την τροποποίηση των δεδομένων σε αντικείμενα blob. Είναι ένα εργαλείο δωρεάν, ανοίξτε προέλευσης που μπορούν να ληφθούν από [http://storageexplorer.com/](http://storageexplorer.com/). Ο κωδικός προέλευσης είναι διαθέσιμη καθώς και αυτήν τη σύνδεση.

Πριν να χρησιμοποιήσετε το εργαλείο, πρέπει να γνωρίζετε Azure χώρου αποθήκευσης και το όνομα του λογαριασμού το κλειδί λογαριασμού σας. Για οδηγίες σχετικά με τη λήψη αυτές τις πληροφορίες, ανατρέξτε στο θέμα το "Πώς να: προβολή", "Αντιγραφή" και "regenerate χώρου αποθήκευσης, πρόσβαση πλήκτρα" ενότητα [Δημιουργία, διαχείριση, ή διαγραφή ενός λογαριασμού χώρου αποθήκευσης][azure-create-storage-account].  

1. Εκτελέστε Explorer Azure χώρου αποθήκευσης. Εάν αυτή είναι η πρώτη φορά που έχετε εκτελέσατε την Εξερεύνηση χώρου αποθήκευσης, θα σας ζητηθεί για το ___όνομα λογαριασμού χώρου αποθήκευσης__ και __κλειδί λογαριασμού χώρου αποθήκευσης__. Εάν έχετε το εκτελέσατε πριν, χρησιμοποιήστε το κουμπί __Προσθήκη__ για να προσθέσετε ένα νέο όνομα λογαριασμού χώρου αποθήκευσης και κλειδί.

    Πληκτρολογήστε το όνομα και αριθμό-κλειδί για το λογαριασμό χώρου αποθήκευσης χρησιμοποιούνται από το HDinsight σύμπλεγμά σας και, στη συνέχεια, επιλέξτε __ΑΠΟΘΉΚΕΥΣΗ και ΆΝΟΙΓΜΑ__.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]

5. Στη λίστα των κοντέινερ στα αριστερά του περιβάλλοντος εργασίας, κάντε κλικ στο όνομα του κοντέινερ που σχετίζεται με το σύμπλεγμά σας HDInsight. Από προεπιλογή, αυτό είναι το όνομα του συμπλέγματος HDInsight, αλλά ενδέχεται να είναι διαφορετικές αν έχετε εισαγάγει ένα συγκεκριμένο όνομα κατά τη δημιουργία του συμπλέγματος.

6. Από τη γραμμή εργαλείων, επιλέξτε το εικονίδιο αποστολής.

    ![Γραμμή εργαλείων με επισήμανση στο εικονίδιο αποστολής](./media/hdinsight-upload-data/toolbar.png)

7. Καθορίστε ένα αρχείο για να αποστείλετε και, στη συνέχεια, κάντε κλικ στην επιλογή **Άνοιγμα**. Όταν σας ζητηθεί, επιλέξτε __Αποστολή__ για να αποστείλετε το αρχείο στη ρίζα του κοντέινερ χώρου αποθήκευσης. Εάν θέλετε να αποστείλετε το αρχείο σε μια συγκεκριμένη διαδρομή, πληκτρολογήστε τη διαδρομή του πεδίου __προορισμού__ και, στη συνέχεια, επιλέξτε __Αποστολή__.

    ![Παράθυρο διαλόγου Αποστολή αρχείου](./media/hdinsight-upload-data/fileupload.png)
    
    Μόλις ολοκληρωθεί η αποστολή του αρχείου, μπορείτε να το χρησιμοποιήσετε από εργασίες στο σύμπλεγμα HDInsight.

##<a name="mount-azure-blob-storage-as-local-drive"></a>Χώρος αποθήκευσης αντικειμένων Blob Azure ενεργοποίησης ως τοπική μονάδα δίσκου

Ανατρέξτε στο θέμα [αποθήκευσης αντικειμένων Blob του Azure ενεργοποίησης ως τοπική μονάδα δίσκου](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

##<a name="services"></a>Υπηρεσίες

###<a name="azure-data-factory"></a>Εργοστασιακές Azure δεδομένων

Η υπηρεσία Azure εργοστασίου δεδομένων είναι μια πλήρως διαχειριζόμενη υπηρεσία για τη σύνταξη υπηρεσίες δεδομένων χώρου αποθήκευσης, επεξεργασίας δεδομένων και δεδομένα κίνηση σε αγωγούς παραγωγής βελτιωμένο μεταβλητού μεγέθους και αξιόπιστη δεδομένων.

Azure εργοστασίου δεδομένων μπορεί να χρησιμοποιηθεί για τη μετακίνηση δεδομένων στο χώρο αποθήκευσης αντικειμένων Blob του Azure ή για να δημιουργήσετε αγωγούς δεδομένων που χρησιμοποιούν απευθείας HDInsight δυνατότητες όπως η ομάδα και γουρούνι.

Για περισσότερες πληροφορίες, ανατρέξτε στην [τεκμηρίωση Azure εργοστασίου δεδομένων](https://azure.microsoft.com/documentation/services/data-factory/).

###<a id="sqoop"></a>Apache Sqoop

Sqoop είναι ένα εργαλείο που έχει σχεδιαστεί για τη μεταφορά δεδομένων μεταξύ Hadoop και σχεσιακές βάσεις δεδομένων. Μπορείτε να το χρησιμοποιήσετε για να εισαγάγετε δεδομένα από ένα σύστημα διαχείρισης σχεσιακή βάση δεδομένων (RDBMS), όπως SQL Server, MySQL ή Oracle στο σύστημα αρχείων Hadoop κατανεμημένο (HDFS), μετασχηματισμός των δεδομένων σε Hadoop με MapReduce ή η ομάδα και, στη συνέχεια, να εξαγάγετε τα δεδομένα σε ένα RDBMS.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση Sqoop με HDInsight][hdinsight-use-sqoop].

##<a name="development-sdks"></a>Ανάπτυξη SDK

Χώρο αποθήκευσης Blob του Azure επίσης είναι δυνατή η πρόσβαση χρησιμοποιώντας μια SDK Azure από τις παρακάτω γλώσσες προγραμματισμού:

* .NET
* Java
* Node.js
* PHP
* Python
* Κείμενο φωνητικής γραφής

Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση του SDK Azure, ανατρέξτε στο θέμα [λήψη του Azure](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

### <a id="storageexception"></a>Εξαίρεση χώρου αποθήκευσης για εγγραφή στο blob

__Συμπτώματα__: όταν χρησιμοποιείτε το `hadoop` ή `hdfs dfs` εντολές για να γράψετε αρχεία που βρίσκονται ~ 12 GB ή μεγαλύτερο σε ένα σύμπλεγμα HBase, που μπορεί να αντιμετωπίσετε το ακόλουθο σφάλμα: 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__Αιτία__: HBase σε HDInsight συμπλεγμάτων προεπιλεγμένη σε μέγεθος μπλοκ των 256 KB κατά την εγγραφή Azure χώρου αποθήκευσης. Ενώ λειτουργεί για HBase APIs ή REST API, αυτό θα έχει ως αποτέλεσμα σφάλμα κατά τη χρήση του `hadoop` ή `hdfs dfs` βοηθητικά προγράμματα γραμμής εντολών.

__Ανάλυση__: χρήση `fs.azure.write.request.size` για να καθορίσετε ένα μεγαλύτερο μέγεθος μπλοκ. Αυτό είναι εφικτό με βάση ανά χρήση, χρησιμοποιώντας το `-D` παραμέτρου. Ακολουθεί ένα παράδειγμα, χρησιμοποιώντας αυτήν την παράμετρο με το `hadoop` εντολή:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Μπορείτε, επίσης, να αυξήσετε την τιμή του `fs.azure.write.request.size` καθολικά χρησιμοποιώντας Ambari. Ακολουθήστε τα παρακάτω βήματα μπορεί να χρησιμοποιηθεί για να αλλάξετε την τιμή στο περιβάλλον εργασίας Χρήστη του Ambari Web:

1. Στο πρόγραμμα περιήγησης, μεταβείτε στο περιβάλλον εργασίας Χρήστη Web Ambari για το σύμπλεγμά σας. Αυτό είναι https://CLUSTERNAME.azurehdinsight.net, όπου __CLUSTERNAME__ είναι το όνομα του συμπλέγματος.

    Όταν σας ζητηθεί, πληκτρολογήστε το όνομα διαχειριστή και τον κωδικό πρόσβασης για το σύμπλεγμα.

2. Από την αριστερή πλευρά της οθόνης, επιλέξτε __HDFS__και, στη συνέχεια, επιλέξτε την καρτέλα __διαμορφώσεων__ .

3. Στο πεδίο " __φιλτράρισμα...__ ", πληκτρολογήστε `fs.azure.write.request.size`. Αυτό θα εμφανίσει το πεδίο και την τρέχουσα τιμή στο μέσο της σελίδας.

4. Αλλαγή της τιμής από 262144 (256KB) με τη νέα τιμή. Για παράδειγμα, 4194304 (4MB).

![Εικόνα του αλλάζοντας την τιμή μέσω του περιβάλλοντος εργασίας Χρήστη Web Ambari](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Για περισσότερες πληροφορίες σχετικά με τη χρήση Ambari, ανατρέξτε στο θέμα [Διαχείριση HDInsight συμπλεγμάτων χρησιμοποιώντας το περιβάλλον εργασίας Χρήστη Web Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που γνωρίζετε τον τρόπο για να κατανοήσετε δεδομένων HDInsight, διαβάστε στα ακόλουθα άρθρα για να μάθετε πώς μπορείτε να εκτελέσετε ανάλυση:

* [Γρήγορα αποτελέσματα με το Azure HDInsight][hdinsight-get-started]
* [Υποβολή εργασιών Hadoop μέσω προγραμματισμού][hdinsight-submit-jobs]
* [Χρήση της ομάδας με το HDInsight][hdinsight-use-hive]
* [Χρήση γουρούνι με HDInsight][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
