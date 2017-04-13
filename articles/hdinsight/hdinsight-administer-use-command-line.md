<properties
    pageTitle="Διαχείριση συμπλεγμάτων Hadoop χρησιμοποιώντας Azure CLI | Microsoft Azure"
    description="Πώς μπορείτε να χρησιμοποιήσετε το CLI Azure για τη Διαχείριση συμπλεγμάτων Hadoop στο HDIsight"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Διαχείριση συμπλεγμάτων Hadoop στο χρησιμοποιώντας το CLI Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Μάθετε πώς μπορείτε να χρησιμοποιήσετε το [Azure περιβάλλον γραμμής εντολών](../xplat-cli-install.md) για τη Διαχείριση συμπλεγμάτων Hadoop στο Azure HDInsight. Το Azure CLI έχει υλοποιηθεί σε Node.js. Μπορεί να χρησιμοποιηθεί σε οποιαδήποτε πλατφόρμα που υποστηρίζει Node.js, συμπεριλαμβανομένων των Windows, Mac και Linux.

Σε αυτό το άρθρο καλύπτει μόνο χρησιμοποιούν το CLI Azure HDInsight. Για γενικές οδηγίες σχετικά με τη χρήση Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων Azure CLI][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI** - δείτε [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md) για πληροφορίες εγκατάστασης και ρύθμισης παραμέτρων.
- **Σύνδεση με το Azure**, χρησιμοποιώντας την ακόλουθη εντολή:

        azure login

    Για περισσότερες πληροφορίες σχετικά με τον έλεγχο ταυτότητας χρησιμοποιώντας έναν εταιρικό ή σχολικό λογαριασμό, ανατρέξτε στο θέμα [σύνδεση σε μια συνδρομή του Azure από το Azure CLI](xplat-cli-connect.md).
    
- **Μετάβαση στη λειτουργία διαχείρισης πόρων Azure**, χρησιμοποιώντας την ακόλουθη εντολή:

        azure config mode arm

Για να λάβετε βοήθεια, χρησιμοποιήστε το διακόπτη **-h** .  Για παράδειγμα:

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>Δημιουργία συμπλεγμάτων

Ανατρέξτε στο θέμα [Δημιουργία Linux βάσει συμπλεγμάτων στο χρησιμοποιώντας το CLI Azure HDInsight](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##<a name="list-and-show-cluster-details"></a>Λίστα και εμφάνιση λεπτομερειών συμπλέγματος
Χρησιμοποιήστε τις παρακάτω εντολές για την παράθεση και εμφάνιση λεπτομερειών συμπλέγματος:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI. CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>Διαγραφή συμπλεγμάτων

Χρησιμοποιήστε την παρακάτω εντολή για να διαγράψετε ένα σύμπλεγμα:

    azure hdinsight cluster delete <Cluster Name>

Μπορείτε επίσης να διαγράψετε ένα σύμπλεγμα, διαγράφοντας την ομάδα πόρων που περιέχει το σύμπλεγμα. Σημειώστε, αυτό θα διαγράψει όλους τους πόρους στην ομάδα, συμπεριλαμβανομένου του προεπιλεγμένου λογαριασμού χώρου αποθήκευσης.

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>Κλίμακα συμπλεγμάτων

Για να αλλάξετε το μέγεθος του συμπλέγματος Hadoop:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Ενεργοποίηση/απενεργοποίηση HTTP πρόσβασης για ένα σύμπλεγμα

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Ενεργοποίηση/απενεργοποίηση RDP πρόσβασης για ένα σύμπλεγμα

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το άρθρο μάθατε πώς μπορείτε να εκτελέσετε διάφορες εργασίες διαχείρισης συμπλέγματος HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

* [Διαχείριση HDInsight, χρησιμοποιώντας την πύλη του Azure] [hdinsight-admin-portal]
* [Διαχείριση HDInsight με χρήση του Azure PowerShell] [hdinsight-admin-powershell]
* [Γρήγορα αποτελέσματα με το Azure HDInsight] [hdinsight-get-started]
* [Πώς μπορείτε να χρησιμοποιήσετε το CLI Azure] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Λίστα "και" Εμφάνιση συμπλεγμάτων"
