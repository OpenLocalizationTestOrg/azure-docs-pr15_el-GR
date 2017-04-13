<properties
   pageTitle="Αντιγράψτε δεδομένα από WASB και στο χώρο αποθήκευσης λίμνης δεδομένων με χρήση Distcp | Microsoft Azure"
   description="Χρησιμοποιήστε το εργαλείο Distcp για να αντιγράψετε δεδομένα από αντικείμενα blob του Azure χώρου αποθήκευσης και στο χώρο αποθήκευσης δεδομένων λίμνης"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Χρήση Distcp για να αντιγράψετε δεδομένα μεταξύ των αντικειμένων blob του Azure χώρου αποθήκευσης και χώρος αποθήκευσης δεδομένων λίμνης

> [AZURE.SELECTOR]
- [Χρήση DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Χρήση AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)


Αφού δημιουργήσετε ένα σύμπλεγμα HDInsight που έχει πρόσβαση σε ένα λογαριασμό του χώρου αποθήκευσης λίμνης δεδομένων, μπορείτε να χρησιμοποιήσετε εργαλεία προσαρμογής Hadoop όπως Distcp για να αντιγράψετε τα δεδομένα **προς και από το** χώρο αποθήκευσης σύμπλεγμα HDInsight (WASB) σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης. Σε αυτό το άρθρο παρέχει οδηγίες σχετικά με τον τρόπο για να επιτύχετε το εξής.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).
- **Ενεργοποίηση της συνδρομής σας Azure** για το χώρο αποθήκευσης δεδομένων λίμνης δημόσια preview. Ανατρέξτε στο θέμα [οδηγίες](data-lake-store-get-started-portal.md#signup).
- **Σύμπλεγμα azure HDInsight** με πρόσβαση σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης. Ανατρέξτε στο θέμα [Δημιουργία ένα σύμπλεγμα HDInsight με το χώρο αποθήκευσης λίμνης δεδομένων](data-lake-store-hdinsight-hadoop-use-portal.md). Βεβαιωθείτε ότι η ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας για το σύμπλεγμα.

## <a name="do-you-learn-fast-with-videos"></a>Θα μάθετε γρήγορα με βίντεο;

[Παρακολουθήστε αυτό το βίντεο](https://mix.office.com/watch/1liuojvdx6sie) σχετικά με τον τρόπο για να αντιγράψετε δεδομένα μεταξύ των αντικειμένων blob του Azure χώρου αποθήκευσης και χώρου αποθήκευσης λίμνης δεδομένων με χρήση DistCp.

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>Χρήση Distcp από απομακρυσμένης επιφάνειας εργασίας (Windows σύμπλεγμα) ή SSH (Linux σύμπλεγμα)

Ένα σύμπλεγμα HDInsight διατίθεται με το βοηθητικό πρόγραμμα Distcp, το οποίο μπορεί να χρησιμοποιηθεί για να αντιγράψετε δεδομένα από διαφορετικές προελεύσεις σε ένα σύμπλεγμα HDInsight. Εάν έχετε ρυθμίσει τις παραμέτρους του συμπλέγματος HDInsight για να χρησιμοποιήσετε το χώρο αποθήκευσης λίμνης δεδομένων ως μια επιπλέον χώρο αποθήκευσης, το βοηθητικό πρόγραμμα Distcp μπορεί να είναι χρησιμοποιούνται εκτός του-οι έτοιμες για να αντιγράψετε δεδομένα από ένα λογαριασμό του χώρου αποθήκευσης λίμνης δεδομένων καθώς και και. Σε αυτήν την ενότητα εξετάσουμε πώς μπορείτε να χρησιμοποιήσετε το βοηθητικό πρόγραμμα Distcp.

1. Εάν έχετε ένα σύμπλεγμα Windows, απομακρυσμένο σε ένα σύμπλεγμα HDInsight που έχει πρόσβαση σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης. Για οδηγίες, ανατρέξτε στο θέμα [σύνδεση με χρήση RDP συμπλεγμάτων](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp). Από το σύμπλεγμα επιφάνειας εργασίας, ανοίξτε τη γραμμή εντολών Hadoop.

    Εάν έχετε ένα σύμπλεγμα Linux, χρησιμοποιήστε SSH για να συνδεθείτε με το σύμπλεγμα. Ανατρέξτε στο θέμα [σύνδεση σε ένα σύμπλεγμα βάσει Linux HDInsight](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster). Εκτελέστε τις εντολές από τη γραμμή εντολών SSH.

3. Επιβεβαιώστε εάν μπορείτε να αποκτήσετε πρόσβαση το χώρο αποθήκευσης αντικειμένων blob Azure (WASB). Εκτελέστε την ακόλουθη εντολή:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Αυτό θα πρέπει να παρέχουν μια λίστα περιεχομένων σε το χώρο αποθήκευσης αντικειμένων blob.

4. Ομοίως, επιβεβαιώστε εάν μπορείτε να αποκτήσετε πρόσβαση στο λογαριασμό του χώρου αποθήκευσης λίμνης δεδομένων από το σύμπλεγμα. Εκτελέστε την ακόλουθη εντολή:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    Αυτό θα πρέπει να παρέχουν μια λίστα των αρχείων/φακέλων στο λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων.

5. Χρησιμοποιήστε Distcp για να αντιγράψετε δεδομένα από WASB σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Αυτό θα αντιγράψετε τα περιεχόμενα του **** φακέλου/παράδειγμα/δεδομένων/gutenberg/στο WASB να **/myfolder** στο λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων.

6. Ομοίως, χρησιμοποιήστε Distcp για να αντιγράψετε δεδομένα από το λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης WASB.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Αυτό θα αντιγραφή των περιεχομένων **/myfolder** στο λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων σε **** φάκελο/παράδειγμα/δεδομένων/gutenberg/WASB.

## <a name="see-also"></a>Δείτε επίσης

- [Αντιγράψτε δεδομένα από αντικείμενα blob του Azure χώρου αποθήκευσης στο χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-copy-data-azure-storage-blob.md)
- [Διασφάλιση των δεδομένων στο χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-secure-data.md)
- [Χρήση ανάλυσης λίμνης Azure δεδομένων με το χώρο αποθήκευσης δεδομένων λίμνης](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Χρήση Azure HDInsight με το χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-hdinsight-hadoop-use-portal.md)
