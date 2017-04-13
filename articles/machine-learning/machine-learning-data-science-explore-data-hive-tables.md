<properties
    pageTitle="Εξερεύνηση των δεδομένων σε πίνακες Hive με ερωτήματα Hive | Microsoft Azure"
    description="Εξερεύνηση των δεδομένων σε πίνακες Hive χρήση ερωτημάτων ομάδα."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Εξερεύνηση των δεδομένων σε πίνακες Hive με ερωτήματα ομάδας

Αυτό το έγγραφο παρέχει δείγμα Hive δεσμών ενεργειών που χρησιμοποιούνται για την Εξερεύνηση των δεδομένων σε πίνακες ομάδας σε ένα σύμπλεγμα HDInsight Hadoop.

Το παρακάτω **μενού** συνδέσεις σε θέματα τα οποία περιγράφουν πώς μπορείτε να χρησιμοποιήσετε εργαλεία για την Εξερεύνηση των δεδομένων από διάφορα περιβάλλοντα χώρο αποθήκευσης.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Σε αυτό το άρθρο προϋποθέτει ότι έχετε:

* Δημιουργία λογαριασμού Azure χώρου αποθήκευσης. Εάν χρειάζεστε οδηγίες, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού αποθήκευσης Azure](../storage/storage-create-storage-account.md#create-a-storage-account)
* Προμήθεια ένα προσαρμοσμένο σύμπλεγμα Hadoop με την υπηρεσία HDInsight. Εάν χρειάζεστε οδηγίες, ανατρέξτε στο θέμα [Προσαρμογή Azure HDInsight Hadoop συμπλεγμάτων για σύνθετη ανάλυση](machine-learning-data-science-customize-hadoop-cluster.md).
* Τα δεδομένα σε πίνακες ομάδα συμπλεγμάτων Azure HDInsight Hadoop έχει αποσταλεί. Εάν δεν έχει, ακολουθήστε τις οδηγίες στο θέμα [Δημιουργία και τη φόρτωση δεδομένων σε πίνακες της ομάδας](machine-learning-data-science-move-hive-tables.md) για την αποστολή δεδομένων σε πίνακες Hive πρώτα.
* Με δυνατότητα απομακρυσμένης πρόσβασης στο σύμπλεγμα. Εάν χρειάζεστε οδηγίες, ανατρέξτε στο θέμα [πρόσβαση κεφαλή κόμβο του Hadoop συμπλέγματος](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* Εάν χρειάζεστε οδηγίες σχετικά με τον τρόπο υποβολής ερωτημάτων Hive, δείτε [πώς μπορείτε να υποβάλετε Hive ερωτημάτων](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Παράδειγμα Hive ερωτήματος δέσμες ενεργειών για Εξερεύνηση δεδομένων

1. Καταμέτρηση των παρατηρήσεων ανά partition `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. Καταμέτρηση των παρατηρήσεων ανά ημέρα `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. Λάβετε τα επίπεδα σε μια στήλη κατηγοριών  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. Βρείτε τον αριθμό των επιπέδων σε συνδυασμό των δύο στήλες κατηγοριών `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. Λάβετε την κατανομή για αριθμητικές στήλες  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. Εξαγάγετε τις εγγραφές από τη συμμετοχή σε δύο πίνακες

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Οι δέσμες ενεργειών πρόσθετες ερωτήματος για σενάρια δεδομένων ταξιδιού ταξί

Παραδείγματα ερωτημάτων που αφορούν συγκεκριμένα σενάρια [Νέα ΥΌΡΚΗ ταξί ταξίδι δεδομένα](http://chriswhong.com/open-data/foil_nyc_taxi/) παρέχονται επίσης στο [αποθετήριο Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Αυτά τα ερωτήματα ήδη σχήμα δεδομένων που καθορίζεται και είστε έτοιμοι να υποβάλλονται για να εκτελέσετε.
