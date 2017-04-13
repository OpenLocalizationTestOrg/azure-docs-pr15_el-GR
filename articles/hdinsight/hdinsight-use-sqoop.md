<properties
    pageTitle="Χρήση Hadoop Sqoop σε HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure PowerShell από μια σταθμούς εργασίας για να εκτελέσετε Sqoop εισαγωγή και εξαγωγή ανάμεσα σε ένα σύμπλεγμα Hadoop και μια βάση δεδομένων Azure SQL."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight"></a>Χρήση Sqoop με Hadoop σε HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Μάθετε πώς μπορείτε να χρησιμοποιήσετε Sqoop HDInsight για εισαγωγή και εξαγωγή μεταξύ συμπλεγμάτων HDInsight και βάση δεδομένων Azure SQL ή βάση δεδομένων SQL Server.

Παρόλο που Hadoop είναι φυσική επιλογή για την επεξεργασία μη δομημένα και semistructured δεδομένα, όπως τα αρχεία καταγραφής και αρχεία, μπορεί επίσης να υπάρχουν ανάγκη για την επεξεργασία δομημένα δεδομένα που είναι αποθηκευμένα σε σχεσιακές βάσεις δεδομένων.

[Sqoop] [ sqoop-user-guide-1.4.4] είναι ένα εργαλείο που έχει σχεδιαστεί για τη μεταφορά δεδομένων μεταξύ συμπλεγμάτων Hadoop και σχεσιακές βάσεις δεδομένων. Μπορείτε να το χρησιμοποιήσετε για να εισαγάγετε δεδομένα από ένα σύστημα διαχείρισης σχεσιακή βάση δεδομένων (RDBMS), όπως SQL Server, MySQL ή Oracle σε σύστημα αρχείων Hadoop κατανεμημένο (HDFS), μετασχηματισμός των δεδομένων στο Hadoop με MapReduce ή η ομάδα και, στη συνέχεια, να εξαγάγετε τα δεδομένα σε μια RDBMS. Σε αυτό το πρόγραμμα εκμάθησης, χρησιμοποιείτε μια βάση δεδομένων SQL Server για τη σχεσιακή βάση δεδομένων.

Για τις εκδόσεις Sqoop που υποστηρίζονται σε συμπλεγμάτων HDInsight, ανατρέξτε στο θέμα [Τι νέο υπάρχει στο τις εκδόσεις σύμπλεγμα που παρέχεται από το HDInsight;] [hdinsight-versions].

##<a name="understand-the-scenario"></a>Κατανόηση του σεναρίου

Σύμπλεγμα HDInsight διατίθεται με ορισμένα δείγματα δεδομένων. Θα μπορείτε να χρησιμοποιήσετε τα ακόλουθα δύο δείγματα:

- Ένα log4j αρχείο καταγραφής, που βρίσκεται στην */example/data/sample.log*. Τα παρακάτω αρχεία καταγραφής είναι που έχουν εξαχθεί από το αρχείο:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

- Ένας πίνακας ομάδα με το όνομα *hivesampletable*, που παραπέμπει στο αρχείο δεδομένων που βρίσκεται στο */hive/warehouse/hivesampletable*. Ο πίνακας περιέχει ορισμένα δεδομένα κινητής συσκευής. 

  	| Πεδίο | Τύπος δεδομένων |
  	| ----- | --------- |
  	| clientid | συμβολοσειρά |
  	| querytime | συμβολοσειρά |
  	| αγορά | συμβολοσειρά |
  	| deviceplatform | συμβολοσειρά |
  	| devicemake | συμβολοσειρά |
  	| devicemodel | συμβολοσειρά |
  	| κατάσταση | συμβολοσειρά |
  	| χώρα | συμβολοσειρά |
  	| querydwelltime | διπλά |
  	| αναγνωριστικό περιόδου λειτουργίας | bigint |
  	| sessionpagevieworder | bigint |

Θα πρώτα να εξάγετε *sample.log* και *hivesampletable* στη βάση δεδομένων Azure SQL ή με τον SQL Server και, στη συνέχεια, εισαγάγετε τον πίνακα που περιέχει τα δεδομένα κινητής συσκευής πίσω στην HDInsight, χρησιμοποιώντας την ακόλουθη διαδρομή:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Δημιουργία σύμπλεγμα και βάση δεδομένων SQL

Αυτή η ενότητα σας δείχνει πώς μπορείτε να δημιουργήσετε ένα σύμπλεγμα και τα σχήματα βάσης δεδομένων SQL για την εκτέλεση του προγράμματος εκμάθησης χρησιμοποιώντας την πύλη του Azure και ενός προτύπου για τη διαχείριση πόρων Azure.  Εάν προτιμάτε να χρησιμοποιήσετε Azure PowerShell, ανατρέξτε στο θέμα [προσάρτημα Α](#appendix-a---a-powershell-sample).

1. Κάντε κλικ στην παρακάτω εικόνα για να ανοίξετε ένα πρότυπο διαχείρισης πόρων στην πύλη του Azure.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fusesqoop%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Το πρότυπο διαχείρισης πόρων βρίσκεται σε δημόσια blob κοντέινερ, *https://hditutorialdata.blob.core.windows.net/usesqoop/create-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json*. 
    
    Το πρότυπο διαχείρισης πόρων κλήσεις ένα πακέτο bacpac για να αναπτύξετε τα σχήματα πίνακα σε βάση δεδομένων SQL.  Το πακέτο bacpac επίσης βρίσκεται σε δημόσια blob κοντέινερ, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Εάν θέλετε να χρησιμοποιήσετε ένα ιδιωτικό κοντέινερ για τα αρχεία bacpac, χρησιμοποιήστε τις παρακάτω τιμές στο πρότυπο:
    
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",
    
2. Από το blade παραμέτρους, εισαγάγετε τα εξής:

    - **ClusterName**: Πληκτρολογήστε ένα όνομα για το σύμπλεγμα Hadoop που θα δημιουργήσετε.
    - **Σύμπλεγμα όνομα σύνδεσης και τον κωδικό πρόσβασης**: το προεπιλεγμένο όνομα σύνδεσης είναι διαχειριστής.
    - **SSH όνομα χρήστη και τον κωδικό πρόσβασης**.
    - **Όνομα σύνδεσης σε βάση δεδομένων SQL server και τον κωδικό πρόσβασης**.

    Οι τιμές που ακολουθούν οποίος καθορίζεται στην ενότητα μεταβλητές:
    
  	|Προεπιλεγμένο όνομα λογαριασμού χώρου αποθήκευσης|<CluterName>χώρος αποθήκευσης|
  	|----------------------------|-----------------|
  	|Azure όνομα διακομιστή βάσης δεδομένων SQL|<ClusterName>dbserver|
  	|Το όνομα βάσης δεδομένων SQL Azure|<ClusterName>DB|
    
    Γράψτε αυτές τις τιμές.  Θα χρειαστείτε τα αργότερα στην εκμάθηση.
    
3, κάντε κλικ στο κουμπί **OK** για να αποθηκεύσετε τις παραμέτρους.

4 από την **Ανάπτυξη προσαρμοσμένης** blade, κάντε κλικ στο πλαίσιο αναπτυσσόμενης λίστας **ομάδα πόρων** και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία** για να δημιουργήσετε μια νέα ομάδα πόρων. Η ομάδα πόρων είναι ένα κοντέινερ που ομαδοποιεί το σύμπλεγμα, ο λογαριασμός εξαρτώμενα χώρου αποθήκευσης και άλλων πόρων συνδεδεμένων.

Διαχειριστής, κάντε κλικ στην επιλογή **νομική τους όρους**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

6 κάντε κλικ στην επιλογή **Δημιουργία**. Θα δείτε ένα νέο πλακίδιο με τίτλο Submitting ανάπτυξης για ανάπτυξη προτύπου. Διαρκεί περίπου περίπου 20 λεπτά για να δημιουργήσετε το σύμπλεγμα και βάση δεδομένων SQL.

Εάν επιλέξετε να χρησιμοποιήσετε υπάρχουσα βάση δεδομένων Azure SQL ή του Microsoft SQL Server

- **Βάση δεδομένων Azure SQL**: πρέπει να ρυθμίσετε έναν κανόνα τείχους προστασίας για το διακομιστή βάσης δεδομένων Azure SQL για να επιτρέψετε την πρόσβαση από το σταθμούς εργασίας. Για οδηγίες σχετικά με τη δημιουργία μιας SQL Azure βάσης δεδομένων και τη ρύθμιση των παραμέτρων του τείχους προστασίας, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με βάση δεδομένων Azure SQL][sqldatabase-get-started]. 

    > [AZURE.NOTE] Από προεπιλογή, μια βάση δεδομένων Azure SQL επιτρέπουν συνδέσεις από το Azure υπηρεσίες, όπως Azure HDInsight. Εάν αυτή η ρύθμιση τείχος προστασίας είναι απενεργοποιημένη, πρέπει έχει ενεργοποιηθεί από την πύλη του Azure. Για οδηγίες σχετικά με τη δημιουργία μιας βάσης δεδομένων Azure SQL και τη ρύθμιση των παραμέτρων κανόνες τείχους προστασίας, ανατρέξτε στο θέμα [Δημιουργία και ρύθμιση παραμέτρων της βάσης δεδομένων SQL][sqldatabase-create-configue].

- **SQL Server**: Εάν το σύμπλεγμά σας HDInsight είναι στο ίδιο δίκτυο εικονικού στο Azure με τον SQL Server, μπορείτε να χρησιμοποιήσετε τα βήματα που περιγράφονται σε αυτό το άρθρο για την εισαγωγή και εξαγωγή δεδομένων σε μια βάση δεδομένων SQL Server.

    > [AZURE.NOTE] HDInsight υποστηρίζει μόνο βάσει θέση εικονικού δίκτυα και δεν αυτήν τη στιγμή λειτουργεί με το εικονικό δίκτυα που βασίζονται σε ομάδα συσχέτισης.

    * Για να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους ενός εικονικού δικτύου, ανατρέξτε στο θέμα [Δημιουργία ένα εικονικό δίκτυο με την πύλη Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

        * Όταν χρησιμοποιείτε το SQL Server στο κέντρο δεδομένων σας, πρέπει να ρυθμίσετε το εικονικό δίκτυο ως *--τοποθεσίας* ή *σημείου σε τοποθεσία*.

            > [AZURE.NOTE] Για το **σημείο σε τοποθεσία** εικονικού δίκτυα, SQL Server πρέπει να εκτελεί το πρόγραμμα-πελάτη VPN ρύθμισης παραμέτρων εφαρμογής, που είναι διαθέσιμη από την **πίνακα εργαλείων** των παραμέτρων Azure εικονικού δικτύου.

        * Όταν χρησιμοποιείτε το SQL Server σε μια εικονική μηχανή Azure, οποιαδήποτε ρύθμιση παραμέτρων εικονικού δικτύου μπορεί να χρησιμοποιηθεί εάν η εικονική μηχανή φιλοξενίας SQL Server είναι μέλος του ίδιου εικονικού δικτύου ως HDInsight.

    * Για να δημιουργήσετε ένα σύμπλεγμα HDInsight εικονικού δικτύου, ανατρέξτε στο θέμα [Δημιουργία Hadoop συμπλεγμάτων στο HDInsight χρησιμοποιώντας προσαρμοσμένες επιλογές](hdinsight-provision-clusters.md)

    > [AZURE.NOTE] SQL Server πρέπει επίσης να επιτρέπει έλεγχο ταυτότητας. Πρέπει να χρησιμοποιήσετε μια σύνδεση SQL Server για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο.


## <a name="run-sqoop-jobs"></a>Εκτέλεση Sqoop εργασιών

HDInsight να εκτελέσετε εργασίες Sqoop, χρησιμοποιώντας διάφορες μεθόδους. Χρησιμοποιήστε τον παρακάτω πίνακα για να αποφασίσετε ποια μέθοδο είναι κατάλληλη για εσάς και, στη συνέχεια, ακολουθήστε τη σύνδεση για αναλυτικές οδηγίες.

| **Χρησιμοποιήστε αυτήν την επιλογή** εάν θέλετε...                                   | .. .an **αλληλεπιδραστικών** κελύφους | ... Επεξεργασία **δέσμης** | .. .with αυτό το **σύμπλεγμα λειτουργικό σύστημα** | .. .from αυτό το **πρόγραμμα-πελάτη λειτουργικό σύστημα** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-use-sqoop-mac-linux.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X ή των Windows        |
| [.NET SDK για Hadoop](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux ή στα Windows                          | Windows (προς το παρόν)                        |
| [Azure PowerShell](hdinsight-hadoop-use-sqoop-powershell.md)  |           &nbsp;            |            ✔            | Linux ή στα Windows                          | Windows                                  |

##<a name="limitations"></a>Περιορισμοί

* Μαζική export - βάσει με Linux HDInsight, Sqoop σύνδεσης που χρησιμοποιείται για την εξαγωγή δεδομένων Microsoft SQL Server ή βάση δεδομένων SQL Azure δεν υποστηρίζει τη συγκεκριμένη στιγμή μαζικής εισάγεται.

* Δέσμης - με βάσει Linux HDInsight, όταν χρησιμοποιείτε το `-batch` εναλλαγή κατά την εκτέλεση εισάγει, Sqoop θα εκτελέσει πολλών εισάγει αντί για δέσμης για τις λειτουργίες εισαγωγή.

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα έχετε μάθει πώς μπορείτε να χρησιμοποιήσετε Sqoop. Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Χρήση της ομάδας με το HDInsight](hdinsight-use-hive.md)
- [Χρήση γουρούνι με HDInsight](hdinsight-use-pig.md)
- [Χρήση Oozie με HDInsight][hdinsight-use-oozie]: χρήση Sqoop ενεργειών σε μια ροή εργασίας Oozie.
- [Ανάλυση των δεδομένων καθυστέρηση πτήσεων χρησιμοποιώντας HDInsight][hdinsight-analyze-flight-data]: Χρησιμοποιήστε την ομάδα για την ανάλυση πτήσεων καθυστέρηση δεδομένων και κατόπιν χρησιμοποιήστε Sqoop να εξαγάγετε τα δεδομένα σε μια βάση δεδομένων Azure SQL.
- [Αποστολή δεδομένων στο HDInsight][hdinsight-upload-data]: βρείτε άλλες μεθόδους για την αποστολή δεδομένων με το χώρο αποθήκευσης αντικειμένων Blob HDInsight/Azure.


## <a name="appendix-a---a-powershell-sample"></a>Παράρτημα A - ένα δείγμα του PowerShell

Το δείγμα PowerShell εκτελεί τα ακόλουθα βήματα:

1. Σύνδεση με Azure.
2. Δημιουργία ομάδας του Azure πόρων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure](../powershell-azure-resource-manager.md)
3. Δημιουργήστε ένα διακομιστή βάσης δεδομένων SQL Azure, μια βάση δεδομένων Azure SQL και τους δύο πίνακες. 

    Εάν χρησιμοποιείτε το SQL Server αντί για αυτό, χρησιμοποιήστε τις παρακάτω προτάσεις για τη δημιουργία πινάκων:
    
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))

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
         [sessionpagevieworder][bigint])

    Ο ευκολότερος τρόπος για να εξετάσετε τη βάση δεδομένων και τους πίνακες είναι να χρησιμοποιήσετε το Visual Studio. Ο διακομιστής βάσης δεδομένων και της βάσης δεδομένων μπορεί να εξετάζονται με την πύλη Azure.

4. Δημιουργήστε ένα σύμπλεγμα HDInsight.

    Για να εξετάσετε το σύμπλεγμα, μπορείτε να χρησιμοποιήσετε το Azure πύλη ή Azure PowerShell.

5. Προ-επεξεργασία αρχείου προέλευσης δεδομένων.

    Σε αυτό το πρόγραμμα εκμάθησης, που θα εξαγάγετε ένα αρχείο καταγραφής log4j (οριοθετημένο αρχείο) και ενός πίνακα ομάδα σε μια βάση δεδομένων Azure SQL. Το οριοθετημένο αρχείο ονομάζεται */example/data/sample.log*. Νωρίτερα σε το πρόγραμμα εκμάθησης, είδατε μερικά δείγματα αρχείων καταγραφής log4j. Στο αρχείο καταγραφής, υπάρχουν ορισμένες κενές γραμμές και ορισμένες γραμμές παρόμοια με τα εξής:
    
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
    
    Αυτό είναι λεπτομερές για άλλα παραδείγματα που χρησιμοποιούν αυτά τα δεδομένα, αλλά θα σας πρέπει να καταργήσετε τις εξής εξαιρέσεις πριν θα σας να εισαγάγετε στη βάση δεδομένων Azure SQL ή SQL Server. Εξαγωγή Sqoop θα αποτύχει, εάν υπάρχει μια κενή συμβολοσειρά ή μια γραμμή με σε μικρότερο αριθμό στοιχείων από τον αριθμό των πεδίων που ορίζονται στον πίνακα βάσης δεδομένων Azure SQL. Ο πίνακας log4jlogs έχει 7 πεδία τύπου συμβολοσειράς.

    Αυτή η διαδικασία δημιουργεί ένα νέο αρχείο στο σύμπλεγμα: tutorials/usesqoop/data/sample.log. Για να εξετάσετε το αρχείο δεδομένων που έχουν τροποποιηθεί, μπορείτε να χρησιμοποιήσετε την πύλη του Azure, ένα εργαλείο explorer αποθήκευσης Azure ή Azure PowerShell. [Γρήγορα αποτελέσματα με το HDInsight] [ hdinsight-get-started] περιλαμβάνει ένα δείγμα κώδικα για τη χρήση του PowerShell Azure για λήψη ενός αρχείου και να εμφανίσετε το περιεχόμενο του αρχείου.

6. Εξαγωγή ενός αρχείου δεδομένων στη βάση δεδομένων Azure SQL.

    Το αρχείο προέλευσης είναι tutorials/usesqoop/data/sample.log. Στον πίνακα όπου την εξαγωγή των δεδομένων για να ονομάζεται log4jlogs.
    
    > [AZURE.NOTE] Εκτός από τις πληροφορίες συμβολοσειράς σύνδεσης, τα βήματα σε αυτήν την ενότητα πρέπει να λειτουργεί για μια βάση δεδομένων Azure SQL ή για τον SQL Server. Αυτά τα βήματα έχουν ελεγχθεί χρησιμοποιώντας τις παρακάτω ρυθμίσεις:
    >
    > * **Ρύθμιση παραμέτρων σημείου σε τοποθεσία Azure εικονικού δικτύου**: ένα εικονικό δίκτυο συνδεδεμένο στο σύμπλεγμα HDInsight με SQL Server σε μια ιδιωτική κέντρα δεδομένων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ενός σημείου σε τοποθεσία VPN στην πύλη διαχείρισης](../vpn-gateway/vpn-gateway-point-to-site-create.md) .
    > * **Azure HDInsight 3.1**: ανατρέξτε στο θέμα [Δημιουργία Hadoop συμπλεγμάτων σε HDInsight χρησιμοποιώντας προσαρμοσμένες επιλογές](hdinsight-provision-clusters.md) για πληροφορίες σχετικά με τη δημιουργία ενός συμπλέγματος εικονικού δικτύου.
    > * **SQL Server 2014**: έχει ρυθμιστεί ώστε να επιτρέπει έλεγχο ταυτότητας και την εκτέλεση του προγράμματος-πελάτη VPN ρύθμισης παραμέτρων πακέτου για να συνδεθείτε με ασφάλεια με το εικονικό δίκτυο.

7. Εξαγωγή πίνακα Hive στη βάση δεδομένων Azure SQL.

8. Εισαγάγετε τον πίνακα mobiledata στο σύμπλεγμα HDInsight.

    Για να εξετάσετε το αρχείο δεδομένων που έχουν τροποποιηθεί, μπορείτε να χρησιμοποιήσετε την πύλη του Azure, ένα εργαλείο explorer αποθήκευσης Azure ή Azure PowerShell.  [Γρήγορα αποτελέσματα με το HDInsight] [ hdinsight-get-started] περιλαμβάνει ένα δείγμα κώδικα σχετικά με τη χρήση του PowerShell Azure για λήψη ενός αρχείου και να εμφανίσετε το περιεχόμενο του αρχείου.


### <a name="the-powershell-sample"></a>Το δείγμα του PowerShell

    # Prepare an Azure SQL database to be used by the Sqoop tutorial
    
    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure Subscription ID>"
    
    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"
    
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    
    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"
    
    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"
    
    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
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
    [sessionpagevieworder][bigint])"
    
    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    
    #endregion
    
    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()
    
    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    
    #endregion
    
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - pre-process the source file
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
    
    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
    
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
    
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
    
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
    
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
    
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
    
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
    
    #endregion
    
    #region - export a log file from the cluster to the SQL database
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    $tableName_log4j = "log4jlogs"
    
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $exportDir_log4j = "/tutorials/usesqoop/data"
    
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - export a Hive table
    
    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - import a database
    
    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"
    
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
