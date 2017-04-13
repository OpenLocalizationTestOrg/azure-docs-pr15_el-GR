<properties
    pageTitle="Χρήση Hadoop Sqoop σε βάσει Linux HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να εκτελέσετε Sqoop εισαγωγή και εξαγωγή μεταξύ Hadoop Linux βασίζεται σε σύμπλεγμα HDInsight και μια βάση δεδομένων Azure SQL."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>Χρήση Sqoop με Hadoop στο HDInsight (SSH)

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Μάθετε πώς μπορείτε να χρησιμοποιήσετε Sqoop για την εισαγωγή και εξαγωγή μεταξύ βάσει Linux HDInsight σύμπλεγμα και βάση δεδομένων της βάσης δεδομένων SQL Azure ή SQL Server.

> [AZURE.NOTE] Τα βήματα σε αυτό το άρθρο χρήση SSH για σύνδεση σε ένα σύμπλεγμα βάσει Linux HDInsight. Προγράμματα-πελάτες του Windows επίσης να χρησιμοποιήσετε Azure PowerShell και HDInsight .NET SDK να εργαστείτε με Sqoop βάσει Linux συμπλεγμάτων. Χρησιμοποιήστε στον επιλογέα στηλοθετών για να ανοίξετε αυτά τα άρθρα.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Σύμπλεγμα A Hadoop στο HDInsight** και βάση __Βάση δεδομένων SQL Azure__: τα βήματα σε αυτό το έγγραφο είναι με βάση το σύμπλεγμα και βάση δεδομένων που δημιουργήθηκε με χρήση του εγγράφου [Δημιουργία σύμπλεγμα και βάση δεδομένων SQL](hdinsight-use-sqoop.md#create-cluster-and-sql-database) . Εάν έχετε ήδη ένα σύμπλεγμα HDInsight και βάση δεδομένων SQL, μπορείτε να αντικαταστήσετε αυτά για τις τιμές που χρησιμοποιούνται σε αυτό το έγγραφο.
- **Σταθμούς εργασίας**: ένας υπολογιστής με ένα πρόγραμμα-πελάτη SSH.

##<a name="install-freetds"></a>Εγκατάσταση FreeTDS

1. Χρησιμοποιήστε SSH για να συνδεθείτε με το σύμπλεγμα βάσει Linux HDInsight. Η διεύθυνση για χρήση κατά τη σύνδεση είναι `CLUSTERNAME-ssh.azurehdinsight.net` και τη θύρα είναι `22`.

    Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH για να συνδεθείτε με το HDInsight, ανατρέξτε στο θέμα τα ακόλουθα έγγραφα:

    * **Προγράμματα-πελάτες του Linux, Unix ή OS X**: ανατρέξτε στο θέμα [σύνδεση σε ένα σύμπλεγμα βάσει Linux HDInsight από Linux, OS X ή Unix](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Προγράμματα-πελάτες των Windows**: ανατρέξτε στο θέμα [σύνδεση σε ένα σύμπλεγμα βάσει Linux HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

3. Χρησιμοποιήστε την ακόλουθη εντολή για να εγκαταστήσετε το FreeTDS:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    FreeTDS θα χρησιμοποιηθεί σε πολλά βήματα για να συνδεθείτε με βάση δεδομένων SQL.

##<a name="create-the-table-in-sql-database"></a>Δημιουργία πίνακα σε βάση δεδομένων SQL

> [AZURE.IMPORTANT] Εάν χρησιμοποιείτε ένα σύμπλεγμα HDInsight και βάση δεδομένων SQL που δημιουργήθηκε χρησιμοποιώντας τα βήματα στο [σύμπλεγμα δημιουργία και βάση δεδομένων SQL](hdinsight-use-sqoop.md), παραβλέψτε τα βήματα σε αυτήν την ενότητα που έχουν δημιουργηθεί από τη βάση δεδομένων και πίνακα ως μέρος του τα βήματα σε αυτό το έγγραφο.

1. Από τη σύνδεση SSH με το HDInsight, χρησιμοποιήστε την παρακάτω εντολή για να συνδεθείτε στη βάση δεδομένων SQL server και Κρήτη τον πίνακα που θα χρησιμοποιηθεί με το υπόλοιπο από αυτά τα βήματα:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Θα λάβετε εξόδου παρόμοια με τα εξής:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Κατά την `1>` ερώτηση, πληκτρολογήστε τις ακόλουθες γραμμές:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    Όταν το `GO` καταχώρηση δήλωση, θα αξιολογηθεί τις προηγούμενες καταστάσεις. Πρώτα, ο πίνακας **mobiledata** δημιουργείται, στη συνέχεια, προστίθεται ένα συγκεντρωτικό ευρετήριο σε αυτό (απαιτείται από βάση δεδομένων SQL).

    Χρησιμοποιήστε τα ακόλουθα για να επαληθεύσετε ότι έχει δημιουργηθεί στον πίνακα:

        SELECT * FROM information_schema.tables
        GO

    Θα πρέπει να βλέπετε εξόδου παρόμοια με τα εξής:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. Πληκτρολογήστε `exit` στο το `1>` μηνύματος για να εξέλθετε από το βοηθητικό πρόγραμμα tsql.

##<a name="sqoop-export"></a>Εξαγωγή Sqoop

3. Από τη σύνδεση SSH με το HDInsight, se την παρακάτω εντολή για να επαληθεύσετε ότι Sqoop μπορεί να δει τη βάση δεδομένων SQL:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Αυτό θα πρέπει να επιστρέψει μια λίστα των βάσεων δεδομένων, συμπεριλαμβανομένης της βάσης δεδομένων **sqooptest** που δημιουργήσατε νωρίτερα.

4. Χρησιμοποιήστε την ακόλουθη εντολή για να εξαγάγετε δεδομένα από **hivesampletable** στον πίνακα **mobiledata** :

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    Αυτό καθοδηγεί Sqoop για να συνδεθείτε με βάση δεδομένων SQL, με τη βάση δεδομένων **sqooptest** , και εξαγωγή δεδομένων από το **wasbs: / / / hive/αποθήκης/hivesampletable** (φυσική αρχεία για το *hivesampletable*,) στον πίνακα **mobiledata** .

5. Μετά την ολοκλήρωση της εντολής, χρησιμοποιήστε τα ακόλουθα για να συνδεθείτε στη βάση δεδομένων με χρήση TSQL:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Μόλις συνδεθεί, χρησιμοποιήστε τις παρακάτω προτάσεις για να επαληθεύσετε ότι έχει εξαχθεί τα δεδομένα στον πίνακα **mobiledata** :

        SELECT * FROM mobiledata
        GO

    Θα πρέπει να μπορείτε να δείτε μια λίστα με δεδομένα του πίνακα. Τύπος `exit` για να εξέλθετε από το βοηθητικό πρόγραμμα tsql.

##<a name="sqoop-import"></a>Εισαγωγή Sqoop

1. Χρησιμοποιήστε τα ακόλουθα για να εισαγάγετε δεδομένα από τον πίνακα **mobiledata** στη βάση δεδομένων SQL, μέχρι την **wasbs: / / / προγράμματα εκμάθησης/usesqoop/importeddata** καταλόγου στην HDInsight:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    Τα δεδομένα που έχουν εισαχθεί θα έχουν τα πεδία που διαχωρίζονται με το χαρακτήρα tab και τις γραμμές θα τερματιστεί κατά ένα χαρακτήρα νέας γραμμής.

2. Μόλις ολοκληρωθεί η εισαγωγή, χρησιμοποιήστε την ακόλουθη εντολή στη λίστα τα δεδομένα του νέου καταλόγου:

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>Χρήση του SQL Server

Μπορείτε επίσης να χρησιμοποιήσετε Sqoop για την εισαγωγή και εξαγωγή δεδομένων από το SQL Server, είτε στο κέντρο δεδομένων σας ή σε ένα εικονικό μηχάνημα που βρίσκεται στο Azure. Οι διαφορές μεταξύ της χρήσης της βάσης δεδομένων SQL και του SQL Server είναι:

* HDInsight και του SQL Server πρέπει να είναι στο ίδιο δίκτυο εικονικού Azure

    > [AZURE.NOTE] HDInsight υποστηρίζει μόνο βάσει θέση εικονικού δίκτυα και δεν αυτήν τη στιγμή λειτουργεί με το εικονικό δίκτυα που βασίζονται σε ομάδα συσχέτισης.

    Όταν χρησιμοποιείτε το SQL Server στο κέντρο δεδομένων σας, πρέπει να ρυθμίσετε το εικονικό δίκτυο ως *--τοποθεσίας* ή *σημείου σε τοποθεσία*.

    > [AZURE.NOTE] Για το **σημείο σε τοποθεσία** εικονικού δίκτυα, SQL Server πρέπει να εκτελεί το πρόγραμμα-πελάτη VPN ρύθμισης παραμέτρων εφαρμογής, που είναι διαθέσιμη από την **πίνακα εργαλείων** των παραμέτρων Azure εικονικού δικτύου.

    Για περισσότερες πληροφορίες Azure εικονικού δικτύου, ανατρέξτε στο θέμα [Επισκόπηση εικονικού δικτύου](../virtual-network/virtual-networks-overview.md).

* SQL Server πρέπει να ρυθμιστεί ώστε να επιτρέπει έλεγχο ταυτότητας SQL. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [επιλογή μια λειτουργία ελέγχου ταυτότητας](https://msdn.microsoft.com/ms144284.aspx)

* Ίσως χρειαστεί να ρυθμίσετε τις παραμέτρους του SQL Server για αποδοχή απομακρυσμένων συνδέσεων. Για περισσότερες πληροφορίες, δείτε [πώς μπορείτε να αντιμετωπίσετε τη σύνδεση με το μηχανισμό βάσεων δεδομένων SQL Server](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)

* Πρέπει να δημιουργήσετε τη βάση δεδομένων **sqooptest** στο SQL Server, χρησιμοποιώντας ένα βοηθητικό πρόγραμμα όπως **SQL Server Management Studio** ή **tsql** - τα βήματα για τη χρήση του Azure CLI λειτουργούν μόνο για βάση δεδομένων SQL Azure

    Οι καταστάσεις TSQL για να δημιουργήσετε τον πίνακα **mobiledata** είναι παρόμοια αυτές που χρησιμοποιούνται για τη βάση δεδομένων SQL, με εξαίρεση τη δημιουργία ενός clusterd ευρετήριο - αυτό δεν είναι απαραίτητο για τον SQL Server:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Κατά τη σύνδεση με τον SQL Server από το HDInsight, ίσως χρειαστεί να χρησιμοποιήσετε τη διεύθυνση IP του SQL Server, εκτός και εάν έχετε ρυθμίσει τις παραμέτρους ενός συστήματος ονομάτων τομέα (DNS) για την επίλυση ονομάτων στο δίκτυο εικονικού Azure. Για παράδειγμα:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>Περιορισμοί

* Μαζική export - βάσει με Linux HDInsight, Sqoop σύνδεσης που χρησιμοποιείται για την εξαγωγή δεδομένων Microsoft SQL Server ή βάση δεδομένων SQL Azure δεν υποστηρίζει τη συγκεκριμένη στιγμή μαζικής εισάγεται.

* Δέσμης - με βάσει Linux HDInsight, όταν χρησιμοποιείτε το `-batch` εναλλαγή κατά την εκτέλεση εισάγει, Sqoop θα εκτελέσει πολλών εισάγει αντί για δέσμης για τις λειτουργίες εισαγωγή.

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα έχετε μάθει πώς μπορείτε να χρησιμοποιήσετε Sqoop. Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Χρήση Oozie με HDInsight][hdinsight-use-oozie]: χρήση Sqoop ενεργειών σε μια ροή εργασίας Oozie.
- [Ανάλυση των δεδομένων καθυστέρηση πτήσεων χρησιμοποιώντας HDInsight][hdinsight-analyze-flight-data]: Χρησιμοποιήστε την ομάδα για την ανάλυση πτήσεων καθυστέρηση δεδομένων και κατόπιν χρησιμοποιήστε Sqoop να εξαγάγετε τα δεδομένα σε μια βάση δεδομένων Azure SQL.
- [Αποστολή δεδομένων στο HDInsight][hdinsight-upload-data]: βρείτε άλλες μεθόδους για την αποστολή δεδομένων με το χώρο αποθήκευσης αντικειμένων Blob HDInsight/Azure.



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
