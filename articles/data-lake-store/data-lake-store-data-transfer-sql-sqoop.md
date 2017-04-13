<properties 
   pageTitle="Αντιγραφή δεδομένων μεταξύ του χώρου αποθήκευσης δεδομένων λίμνης και βάση δεδομένων Azure SQL με χρήση Sqoop | Microsoft Azure"
   description="Χρήση Sqoop για να αντιγράψετε δεδομένα μεταξύ βάση δεδομένων SQL Azure και του χώρου αποθήκευσης δεδομένων λίμνης" 
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

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Αντιγραφή δεδομένων μεταξύ του χώρου αποθήκευσης δεδομένων λίμνης και βάση δεδομένων Azure SQL με χρήση Sqoop

Μάθετε πώς μπορείτε να χρησιμοποιήσετε Apache Sqoop για την εισαγωγή και εξαγωγή δεδομένων μεταξύ της βάσης δεδομένων SQL Azure και αποθήκευσης λίμνης δεδομένων.
 

## <a name="what-is-sqoop"></a>Τι είναι το Sqoop;

Οι εφαρμογές μεγάλο δεδομένων είναι μια φυσική επιλογή για την επεξεργασία μη δομημένα και ημι-δομημένα δεδομένα, όπως τα αρχεία καταγραφής και αρχεία. Ωστόσο, μπορεί επίσης να υπάρχει ανάγκη για την επεξεργασία δομημένα δεδομένα που είναι αποθηκευμένα σε σχεσιακές βάσεις δεδομένων.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) είναι ένα εργαλείο που έχει σχεδιαστεί για τη μεταφορά δεδομένων μεταξύ σχεσιακές βάσεις δεδομένων και ένα αρχείο φύλαξης μεγάλο δεδομένων, όπως το χώρο αποθήκευσης λίμνης δεδομένων. Μπορείτε να το χρησιμοποιήσετε για να εισαγάγετε δεδομένα από ένα σύστημα διαχείρισης σχεσιακή βάση δεδομένων (RDBMS) όπως η βάση δεδομένων SQL Azure στο χώρο αποθήκευσης λίμνης δεδομένων. Μπορείτε να, στη συνέχεια, μετασχηματισμός και ανάλυση των δεδομένων με χρήση φόρτους εργασίας μεγάλο δεδομένων και, στη συνέχεια, να εξαγάγετε τα δεδομένα σε ένα RDBMS. Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσετε μια βάση δεδομένων SQL Azure ως σας σχεσιακή βάση δεδομένων για εισαγωγή/εξαγωγή από.
 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).
- **Ενεργοποίηση της συνδρομής σας Azure** για το χώρο αποθήκευσης δεδομένων λίμνης δημόσια preview. Ανατρέξτε στο θέμα [οδηγίες](data-lake-store-get-started-portal.md#signup). 
- **Σύμπλεγμα azure HDInsight** με πρόσβαση σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης. Ανατρέξτε στο θέμα [Δημιουργία ένα σύμπλεγμα HDInsight με το χώρο αποθήκευσης λίμνης δεδομένων](data-lake-store-hdinsight-hadoop-use-portal.md). Σε αυτό το άρθρο προϋποθέτει ότι έχετε ένα σύμπλεγμα HDInsight Linux με το χώρο αποθήκευσης λίμνης δεδομένων access.
- **Βάση δεδομένων SQL azure**. Για οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα, ανατρέξτε στο θέμα [Δημιουργία μια βάση δεδομένων Azure SQL](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Θα μάθετε γρήγορα με βίντεο;

[Παρακολουθήστε αυτό το βίντεο](https://mix.office.com/watch/1butcdjxmu114) σχετικά με τον τρόπο για να αντιγράψετε δεδομένα μεταξύ των αντικειμένων blob του Azure χώρου αποθήκευσης και χώρου αποθήκευσης λίμνης δεδομένων με χρήση DistCp.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Δημιουργία πινάκων δείγμα στη βάση δεδομένων SQL Azure

1. Για να ξεκινήσετε με, δημιουργήστε δύο δειγμάτων πίνακες στη βάση δεδομένων SQL Azure. Χρήση του [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) ή Visual Studio για να συνδεθείτε στη βάση δεδομένων SQL Azure και στη συνέχεια, εκτελέστε τα ακόλουθα ερωτήματα.

    **Δημιουργία Πίνακας1**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **Δημιουργία πίνακας2**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. Στο **πίνακας1**, προσθέστε μερικά δείγματα δεδομένων. Αφήστε κενό **πίνακας2** . Θα σας θα εισαγάγετε δεδομένα από **τον Table1** στο χώρο αποθήκευσης λίμνης δεδομένων. Στη συνέχεια, θα σας θα εξαγωγή δεδομένων από το χώρο αποθήκευσης λίμνης δεδομένων σε **πίνακας2**. Εκτελέστε το ακόλουθο τμήμα κώδικα.

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Χρήση Sqoop από ένα σύμπλεγμα HDInsight με την πρόσβαση στο χώρο αποθήκευσης δεδομένων λίμνης

Ένα σύμπλεγμα HDInsight διαθέτει ήδη τα πακέτα Sqoop διαθέσιμη. Εάν έχετε ρυθμίσει τις παραμέτρους του συμπλέγματος HDInsight για να χρησιμοποιήσετε το χώρο αποθήκευσης λίμνης δεδομένων ως μια επιπλέον χώρο αποθήκευσης, μπορείτε να χρησιμοποιήσετε Sqoop (χωρίς αλλαγές ρύθμισης παραμέτρων) για εισαγωγή/εξαγωγή δεδομένων μεταξύ σχεσιακή βάση δεδομένων (σε αυτό το παράδειγμα, βάση δεδομένων SQL Azure) και ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης. 

1. Για αυτό το πρόγραμμα εκμάθησης, θα σας θεωρείται ότι έχετε δημιουργήσει ένα σύμπλεγμα Linux, ώστε να πρέπει να χρησιμοποιήσετε SSH για να συνδεθείτε με το σύμπλεγμα. Ανατρέξτε στο θέμα [σύνδεση σε ένα σύμπλεγμα βάσει Linux HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).

2. Επιβεβαιώστε εάν μπορείτε να αποκτήσετε πρόσβαση στο λογαριασμό του χώρου αποθήκευσης λίμνης δεδομένων από το σύμπλεγμα. Εκτελέστε την ακόλουθη εντολή από τη γραμμή εντολών SSH:

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    Αυτό θα πρέπει να παρέχουν μια λίστα των αρχείων/φακέλων στο λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Εισαγωγή δεδομένων από βάση δεδομένων SQL Azure στο χώρο αποθήκευσης δεδομένων λίμνης

3. Μεταβείτε στον κατάλογο όπου Sqoop πακέτα είναι διαθέσιμα. Συνήθως, αυτό θα είναι στο `/usr/hdp/<version>/sqoop/bin`. 

4. Εισαγάγετε τα δεδομένα από **τον Table1** στο λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων. Χρησιμοποιήστε την ακόλουθη σύνταξη:

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Σημείωση που **sql--όνομα διακομιστή βάσης δεδομένων** αντιπροσωπεύει το όνομα του διακομιστή όπου εκτελείται η βάση δεδομένων Azure SQL. **όνομα βάσης δεδομένων SQL** αντιπροσωπεύει το όνομα πραγματική βάση δεδομένων.

    Για παράδειγμα,

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. Βεβαιωθείτε ότι τα δεδομένα μεταφερθεί με το λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων. Εκτελέστε την ακόλουθη εντολή:

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Θα πρέπει να βλέπετε το παρακάτω αποτέλεσμα.

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Κάθε **τμήμα-m -** * αρχείο αντιστοιχεί σε μια γραμμή στον πίνακα προέλευσης, * *πίνακας1**. Μπορείτε να προβάλετε τα περιεχόμενα του τμήματος-m -* αρχείων για να επαληθεύσετε.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Εξαγωγή δεδομένων από το χώρο αποθήκευσης λίμνης δεδομένων σε βάση δεδομένων SQL Azure

6. Εξαγάγετε τα δεδομένα από το λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης στον κενό πίνακα, **πίνακας2**, της βάσης δεδομένων SQL Azure. Χρησιμοποιήστε την ακόλουθη σύνταξη.

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Για παράδειγμα,

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. Βεβαιωθείτε ότι τα δεδομένα έχει αποσταλεί στον πίνακα βάσης δεδομένων SQL. Χρήση του [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) ή Visual Studio για να συνδεθείτε στη βάση δεδομένων SQL Azure και στη συνέχεια, εκτελέστε το ακόλουθο ερώτημα.

        
        SELECT * FROM TABLE2

    Αυτό θα πρέπει να έχετε το παρακάτω αποτέλεσμα.

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>Δείτε επίσης

- [Αντιγράψτε δεδομένα από αντικείμενα blob του Azure χώρου αποθήκευσης στο χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-copy-data-azure-storage-blob.md)
- [Διασφάλιση των δεδομένων στο χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-secure-data.md)
- [Χρήση ανάλυσης λίμνης Azure δεδομένων με το χώρο αποθήκευσης δεδομένων λίμνης](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Χρήση Azure HDInsight με το χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-hdinsight-hadoop-use-portal.md)
