<properties
    pageTitle="Ανάλυση αισθητήρα δεδομένων με χρήση της ομάδας και Hadoop | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να αναλύσετε δεδομένα αισθητήρα, χρησιμοποιώντας την κονσόλα ερωτήματος Hive με HDInsight (Hadoop) και, στη συνέχεια, οπτικοποίηση των δεδομένων στο Microsoft Excel με το PowerView."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="larryfr"/>

#<a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Ανάλυση των δεδομένων αισθητήρα χρησιμοποιώντας την κονσόλα ερωτήματος Hive στην Hadoop στο HDInsight

Μάθετε πώς μπορείτε να αναλύσετε δεδομένα αισθητήρα, χρησιμοποιώντας την κονσόλα ερωτήματος Hive με HDInsight (Hadoop) και, στη συνέχεια, οπτικοποίηση των δεδομένων στο Microsoft Excel με το Power View.

> [AZURE.NOTE] Τα βήματα σε αυτό το έγγραφο λειτουργούν μόνο με συμπλεγμάτων HDInsight που βασίζεται στα Windows.

Σε αυτό το δείγμα, θα χρησιμοποιήσετε Hive σε διαδικασία παλαιότερα στοιχεία που δημιουργήθηκαν με συστήματα θέρμανσης, εξαερισμού και κλιματισμού (HVAC) συστήματα για τον προσδιορισμό συστήματα που δεν έχουν τη δυνατότητα να αξιόπιστα διατηρεί τη θερμοκρασία σύνολο. Θα μάθετε πώς μπορείτε να:

- Δημιουργία πινάκων HIVE ερωτήματα δεδομένων που έχουν αποθηκευτεί σε αρχεία τιμές διαχωρισμένες με κόμμα (CSV) τιμή.
- Δημιουργία ΟΜΆΔΑΣ ερωτημάτων για να αναλύσετε τα δεδομένα.
- Χρησιμοποιήστε το Microsoft Excel για να συνδεθείτε με το HDInsight (χρησιμοποιώντας τη σύνδεση ανοικτή βάση δεδομένων (ODBC) για την ανάκτηση ανάλυσης δεδομένων.
- Χρησιμοποιήστε το Power View για την απεικόνιση των δεδομένων.

![Ένα διάγραμμα η αρχιτεκτονική λύσης](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Ένα σύμπλεγμα HDInsight (Hadoop): ανατρέξτε στο θέμα [Hadoop παροχή συμπλεγμάτων στο HDInsight](hdinsight-provision-clusters.md) για πληροφορίες σχετικά με τη δημιουργία ενός συμπλέγματος.

* Microsoft Excel 2013

    > [AZURE.NOTE] Το Microsoft Excel χρησιμοποιείται για την απεικόνιση δεδομένων με το [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Ομάδα Microsoft πρόγραμμα οδήγησης ODBC](http://www.microsoft.com/download/details.aspx?id=40886)

##<a name="to-run-the-sample"></a>Για να εκτελέσετε το δείγμα

1. Από το πρόγραμμα περιήγησης web, μεταβείτε στην ακόλουθη διεύθυνση URL. Αντικατάσταση `<clustername>` με το όνομα του συμπλέγματος HDInsight.

        https://<clustername>.azurehdinsight.net

    Όταν σας ζητηθεί, τον έλεγχο ταυτότητας, χρησιμοποιώντας το όνομα χρήστη του διαχειριστή και τον κωδικό πρόσβασης που χρησιμοποιήσατε κατά την προμήθεια αυτό το σύμπλεγμα.

2. Από την ιστοσελίδα που ανοίγει, κάντε κλικ στην καρτέλα **Συλλογή γρήγορα αποτελέσματα** και, στη συνέχεια, στην κατηγορία **λύσεις με δείγμα δεδομένων** , κάντε κλικ στην επιλογή το δείγμα **Αισθητήρα ανάλυση δεδομένων** .

    ![Γρήγορα αποτελέσματα συλλογή εικόνων](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Ακολουθήστε τις οδηγίες που παρέχονται στη σελίδα web για να ολοκληρώσετε το δείγμα.
