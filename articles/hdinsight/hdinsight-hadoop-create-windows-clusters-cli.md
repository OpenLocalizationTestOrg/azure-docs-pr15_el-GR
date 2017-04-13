<properties
   pageTitle="Δημιουργία συμπλεγμάτων που βασίζεται σε Windows Hadoop στο χρησιμοποιώντας Azure CLI HDInsight"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε συμπλεγμάτων για Azure HDInsight με τη χρήση Azure CLI."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Δημιουργία συμπλεγμάτων που βασίζεται σε Windows Hadoop στο χρησιμοποιώντας Azure CLI HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Μάθετε πώς να δημιουργείτε χρησιμοποιώντας Azure CLI συμπλεγμάτων HDInsight. Για άλλες δημιουργία συμπλέγματος και εργαλεία που διαθέτει κάντε κλικ στην επιλογή καρτέλα, επιλέξτε στο επάνω μέρος αυτής της σελίδας ή [μεθόδους δημιουργίας σύμπλεγμα](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Πριν να ξεκινήσετε τις οδηγίες σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Azure συνδρομής**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI**.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Απαιτήσεις για στοιχείο ελέγχου πρόσβασης

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>Σύνδεση με Azure

Χρησιμοποιήστε την ακόλουθη εντολή για να συνδεθείτε με Azure:

    azure login

Για περισσότερες πληροφορίες σχετικά με τον έλεγχο ταυτότητας χρησιμοποιώντας έναν εταιρικό ή σχολικό λογαριασμό, ανατρέξτε στο θέμα [σύνδεση σε μια συνδρομή του Azure από το Azure CLI](../xplat-cli-connect.md).

Χρησιμοποιήστε την παρακάτω εντολή για να μεταβείτε στη λειτουργία ARM:

    azure config mode arm

Για να λάβετε βοήθεια, χρησιμοποιήστε το διακόπτη **-h** .  Για παράδειγμα:

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>Δημιουργία συμπλεγμάτων

Πρέπει να έχετε μια διαχείρισης πόρων Azure (ARM) και ένα λογαριασμό του χώρου αποθήκευσης αντικειμένων Blob του Azure πριν μπορέσετε να δημιουργήσετε ένα σύμπλεγμα HDInsight. Για να δημιουργήσετε ένα σύμπλεγμα HDInsight, πρέπει να καθορίσετε τα εξής:

- **Ομάδα πόρων Azure**: ανάλυση λίμνης δεδομένων A λογαριασμό πρέπει να δημιουργηθεί μέσα σε μια ομάδα πόρων Azure. Azure διαχείριση πόρων σάς επιτρέπει να εργαστείτε με τους πόρους στην εφαρμογή σας ως ομάδα. Να αναπτύξετε, ενημέρωση ή να διαγράψετε όλους τους πόρους για την εφαρμογή σας σε μια ενιαία, συντονισμένη λειτουργία.

    Για μια λίστα των ομάδων πόρων στη συνδρομή σας:

        azure group list

    Για να δημιουργήσετε μια νέα ομάδα πόρων:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Όνομα HDInsight συμπλέγματος**

- **Θέση**: ένα από τα κέντρα δεδομένων Azure που υποστηρίζει συμπλεγμάτων HDInsight. Για μια λίστα με τις υποστηριζόμενες τοποθεσίες, ανατρέξτε στο θέμα [HDInsight τις τιμές](https://azure.microsoft.com/pricing/details/hdinsight/).

- **Προεπιλεγμένο αποθήκευσης λογαριασμό**: HDInsight χρησιμοποιεί ένα κοντέινερ χώρου αποθήκευσης αντικειμένων Blob του Azure ως το προεπιλεγμένο σύστημα αρχείων. Ένας λογαριασμός Azure χώρου αποθήκευσης είναι απαιτείται να μπορέσετε να δημιουργήσετε ένα σύμπλεγμα HDInsight.

    Για να δημιουργήσετε ένα νέο λογαριασμό Azure χώρου αποθήκευσης:

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]Το λογαριασμό χώρου αποθήκευσης πρέπει να είναι collocated με HDInsight στο κέντρο δεδομένων.
    > Στο πεδίο Τύπος λογαριασμού χώρου αποθήκευσης δεν είναι ZRS, επειδή το ZRS δεν υποστηρίζει πίνακα.

    Για πληροφορίες σχετικά με τη δημιουργία λογαριασμού αποθήκευσης Azure χρησιμοποιώντας την πύλη Azure, ανατρέξτε στο θέμα [δημιουργία, διαχείριση ή διαγραφή ενός λογαριασμού χώρου αποθήκευσης] [azure-δημιουργία-storageaccount].

    Εάν έχετε ήδη ένα λογαριασμό του χώρου αποθήκευσης αλλά δεν γνωρίζετε το όνομα του λογαριασμού και το κλειδί λογαριασμού, μπορείτε να χρησιμοποιήσετε τις παρακάτω εντολές για την ανάκτηση των πληροφοριών:

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    Για λεπτομέρειες σχετικά με γρήγορα τις πληροφορίες χρησιμοποιώντας την πύλη Azure, ανατρέξτε στην ενότητα "Διαχείριση λογαριασμού χώρου αποθήκευσης" [Azure σχετικά με το χώρο αποθήκευσης](../storage/storage-create-storage-account#manage-your-storage-account)λογαριασμών.

- **(Προαιρετικά) προεπιλεγμένη Blob κοντέινερ**: Η εντολή **δημιουργία συμπλέγματος azure hdinsight** δημιουργεί το κοντέινερ, εάν δεν υπάρχει. Εάν επιλέξετε να δημιουργήσετε το κοντέινερ εκ των προτέρων, μπορείτε να χρησιμοποιήσετε την ακόλουθη εντολή:

    Δημιουργία κοντέινερ χώρου αποθήκευσης Azure--όνομα λογαριασμού "<Storage Account Name>"--κλειδί λογαριασμού <Storage Account Key> [ContainerName]

Όταν έχετε το λογαριασμό χώρου αποθήκευσης προετοιμασμένοι, είστε έτοιμοι να δημιουργήσετε ένα σύμπλεγμα:


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>Δημιουργία συμπλεγμάτων χρησιμοποιώντας αρχεία ρύθμισης παραμέτρων
Συνήθως, δημιουργείτε ένα σύμπλεγμα HDInsight, εκτελέσετε εργασίες σε αυτό, και στη συνέχεια, διαγράψτε το σύμπλεγμα για να περιορίσω το κόστος. Το περιβάλλον γραμμής εντολών σάς δίνει την επιλογή για να αποθηκεύσετε τις ρυθμίσεις παραμέτρων σε ένα αρχείο, ώστε να μπορείτε να τη χρησιμοποιήσετε ξανά κάθε φορά που δημιουργείτε ένα σύμπλεγμα.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Παράδειγμα: Δημιουργήστε ένα αρχείο ρύθμισης παραμέτρων που περιέχει μια ενέργεια δέσμη ενεργειών για να εκτελέσετε κατά τη δημιουργία ενός συμπλέγματος.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>Δημιουργία συμπλεγμάτων με ενέργεια δέσμη ενεργειών

Δημιουργήστε ένα αρχείο ρύθμισης παραμέτρων που περιέχει μια ενέργεια δέσμη ενεργειών για να εκτελέσετε κατά τη δημιουργία ενός συμπλέγματος.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Δημιουργήστε ένα σύμπλεγμα με ενέργεια δέσμης ενεργειών

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Για γενικές δέσμης ενεργειών ενέργεια πληροφορίες, ανατρέξτε [συμπλεγμάτων HDInsight προσαρμογή με χρήση δέσμης ενεργειών (Linux)](hdinsight-hadoop-customize-cluster.md).


## <a name="create-clusters-using-arm-templates"></a>Δημιουργία συμπλεγμάτων με τη χρήση προτύπων ARM

Μπορείτε να χρησιμοποιήσετε CLI για να δημιουργήσετε συμπλεγμάτων καλώντας ARM πρότυπα. Ανατρέξτε στο θέμα [Ανάπτυξη με το Azure CLI](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Δείτε επίσης

- [Γρήγορα αποτελέσματα με το Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) - μάθετε πώς μπορείτε να αρχίσετε να εργάζεστε με το σύμπλεγμά σας HDInsight
- [Υποβολή Hadoop έργα μέσω προγραμματισμού](hdinsight-submit-hadoop-jobs-programmatically.md) - μάθετε πώς μπορείτε να υποβάλετε μέσω προγραμματισμού εργασιών με το HDInsight
- [Διαχείριση συμπλεγμάτων Hadoop στο χρησιμοποιώντας το CLI Azure HDInsight](hdinsight-administer-use-command-line.md)
- [Με το Azure CLI για Mac, Linux και Windows Azure υπηρεσία διαχείρισης](../virtual-machines-command-line-tools.md)
