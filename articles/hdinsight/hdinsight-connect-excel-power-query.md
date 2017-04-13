<properties
    pageTitle="Σύνδεση του Excel με Hadoop με το Power Query | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να επωφεληθείτε από επιχειρηματικής ευφυΐας στοιχεία και να χρησιμοποιήσετε το Power Query για Excel για πρόσβαση σε δεδομένα αποθηκευμένα σε Hadoop σε HDInsight."
    services="hdinsight"
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
    ms.date="10/19/2016"
    ms.author="jgao"/>


#<a name="connect-excel-to-hadoop-by-using-power-query"></a>Συνδεθείτε Excel Hadoop, χρησιμοποιώντας το Power Query

Μία βασικών δυνατοτήτων της λύσης μεγάλο δεδομένων Microsoft είναι η ενοποίηση του Microsoft επιχειρηματικής ευφυΐας (BI) στοιχεία με Hadoop συμπλεγμάτων στο Azure HDInsight. Ένα παράδειγμα κύρια αυτή η ενσωμάτωση είναι η δυνατότητα σύνδεσης του Excel με το λογαριασμό χώρου αποθήκευσης Azure που περιέχει τα δεδομένα που σχετίζονται με το σύμπλεγμά σας Hadoop, χρησιμοποιώντας το Microsoft Power Query για το πρόσθετο του Excel. Σε αυτό το άρθρο σάς καθοδηγεί σε πώς μπορείτε να ρυθμίσετε και να χρησιμοποιήσετε το Power Query να ερωτήματος δεδομένων που σχετίζεται με ένα σύμπλεγμα Hadoop διαχείρισης με το HDInsight.

> [AZURE.NOTE] Ενώ τα βήματα σε αυτό το άρθρο μπορούν να χρησιμοποιηθούν με σύμπλεγμα μια Linux ή HDInsight που βασίζεται στα Windows, Windows είναι απαραίτητη για το πρόγραμμα-πελάτη σταθμούς εργασίας.

### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Σύμπλεγμα του HDInsight**. Για να ρυθμίσετε ένα, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure HDInsight][hdinsight-get-started].
- **A σταθμούς εργασίας** που εκτελεί Windows 7, Windows Server 2008 R2 ή νεότερο λειτουργικό σύστημα.
- **Office 2013 Professional Plus, Office 365 ProPlus, αυτόνομη έκδοση του Excel 2013, ή το Office 2010 Professional Plus**.


## <a name="install-power-query"></a>Εγκατάσταση του Power Query

Power Query μπορεί να χρησιμοποιηθεί για την εισαγωγή δεδομένων από διάφορες προελεύσεις στο Microsoft Excel, όπου μπορούν να το του power BI εργαλεία όπως το PowerPivot και το Power View. Συγκεκριμένα, Power Query να εισαγάγετε δεδομένα που έχει τεθεί εξόδου ή που έχει δημιουργηθεί από μια εργασία Hadoop εκτελείται σε ένα σύμπλεγμα HDInsight.

Κάντε λήψη του Microsoft Power Query για Excel από το [Κέντρο λήψης της Microsoft] [ powerquery-download] και να την εγκαταστήσετε.

## <a name="import-hdinsight-data-into-excel"></a>Εισαγωγή HDInsight δεδομένων στο Excel

Power Query στο πρόσθετο για το Excel διευκολύνει την εισαγωγή δεδομένων από το HDInsight σύμπλεγμά σας στο Excel, όπου εργαλεία Επιχειρηματικής ευφυΐας, όπως PowerPivot και το Power Map μπορεί να χρησιμοποιηθεί για να ελέγξετε, ανάλυση και παρουσίαση των δεδομένων.

**Για να εισαγάγετε δεδομένα από ένα σύμπλεγμα HDInsight**

1. Ανοίξτε το Excel.

2. Δημιουργήστε ένα νέο κενό βιβλίο εργασίας.

3. Κάντε κλικ στο μενού του **Power Query** , κάντε κλικ στην επιλογή **Από το Azure**και, στη συνέχεια, κάντε κλικ στην επιλογή **Από το Microsoft Azure HDInsight**.

    ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **Σημείωση:** Εάν δεν βλέπετε το μενού **Power Query** , μεταβείτε στο **αρχείο** > **Επιλογές** > **Πρόσθετα**και επιλέξτε **Πρόσθετα COM** από το αναπτυσσόμενο πλαίσιο **Διαχείριση** στο κάτω μέρος της σελίδας. Επιλέξτε το κουμπί **μεταβείτε...** και βεβαιωθείτε ότι το πλαίσιο για το Power Query για το πρόσθετο του Excel έχει ελεγχθεί.

    **Σημείωση:** Power Query σας επιτρέπει επίσης να εισαγάγετε δεδομένα από HDFS, κάνοντας κλικ στην επιλογή **Από άλλες προελεύσεις**.

3. Για **Το όνομα του λογαριασμού**, πληκτρολογήστε το όνομα του λογαριασμού χώρου αποθήκευσης αντικειμένων Blob του Azure που σχετίζεται με το σύμπλεγμά σας και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**. Αυτό είναι το [προεπιλεγμένο αποθήκευσης λογαριασμό](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) ή έναν λογαριασμό συνδεδεμένων χώρου αποθήκευσης.  Η μορφή είναι *https://<StorageAccountName>.blob.core.windows.net/*.

4. Για το **Κλειδί λογαριασμού**, πληκτρολογήστε το κλειδί για το λογαριασμό χώρου αποθήκευσης αντικειμένων Blob και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**. (Πρέπει να το κάνετε αυτό μόνο την πρώτη φορά που έχετε πρόσβαση σε αυτόν το χώρο αποθήκευσης.)

5. Στο παράθυρο **περιήγησης** στην αριστερή πλευρά του προγράμματος επεξεργασίας ερωτήματος, κάντε διπλό κλικ στο όνομα κοντέινερ χώρου αποθήκευσης αντικειμένων Blob. Από προεπιλογή, το όνομα του κοντέινερ είναι το ίδιο όνομα με το όνομα του συμπλέγματος.

6. Εντοπίστε **το αρχείο HiveSampleData.txt** στη στήλη **όνομα** (η διαδρομή του φακέλου είναι **... / hive/αποθήκης/hivesampletable/**), και, στη συνέχεια, κάντε κλικ στην επιλογή **δυαδικό** στα αριστερά της το αρχείο HiveSampleData.txt. Το αρχείο HiveSampleData.txt συνοδεύεται από όλα του συμπλέγματος. Προαιρετικά, μπορείτε να χρησιμοποιήσετε το δικό σας αρχείο.

    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. Εάν θέλετε, μπορείτε να μετονομάσετε τα ονόματα των στηλών. Όταν είστε έτοιμοι, κάντε κλικ στην επιλογή **Κλείσιμο και φόρτωση**.  Μετά τη φόρτωση των δεδομένων στο βιβλίο εργασίας σας:

    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο μάθατε πώς μπορείτε να χρησιμοποιήσετε το Power Query για να ανακτήσετε δεδομένα από το HDInsight στο Excel. Ομοίως, μπορείτε να ανακτήσετε δεδομένα από το HDInsight σε βάση δεδομένων SQL Azure. Επίσης, είναι δυνατή η αποστολή δεδομένων σε HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

* [Σύνδεση Excel με το HDInsight με το πρόγραμμα οδήγησης ODBC Microsoft Hive][hdinsight-ODBC]
* [Αποστολή δεδομένων σε HDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
