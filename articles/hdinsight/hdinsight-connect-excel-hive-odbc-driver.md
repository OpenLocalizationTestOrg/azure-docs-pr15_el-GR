<properties
   pageTitle="Σύνδεση του Excel με Hadoop με το πρόγραμμα οδήγησης ODBC Hive | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να ρυθμίσετε και να χρησιμοποιήσετε το πρόγραμμα οδήγησης Microsoft Hive ODBC για το Excel με δεδομένα ερωτήματος σε ένα σύμπλεγμα HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>Σύνδεση του Excel σε Hadoop με το πρόγραμμα οδήγησης Microsoft Hive ODBC

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Λύση μεγάλο δεδομένων της Microsoft Ενοποιείται στοιχεία Microsoft επιχειρηματικής ευφυΐας (BI) με συμπλεγμάτων Apache Hadoop που έχουν αναπτυχθεί από το Azure HDInsight. Παράδειγμα ενοποίησης αυτή είναι η δυνατότητα σύνδεσης του Excel στην ομάδα αποθήκη δεδομένων από ένα σύμπλεγμα Hadoop στο HDInsight χρησιμοποιώντας το πρόγραμμα οδήγησης Microsoft Hive Open Database Connectivity (ODBC).

Επίσης, είναι δυνατή η σύνδεση δεδομένων που σχετίζεται με ένα σύμπλεγμα HDInsight και άλλες προελεύσεις δεδομένων, συμπεριλαμβανομένων άλλων συμπλεγμάτων Hadoop (μη HDInsight), από το Excel με χρήση του πρόσθετου του Microsoft Power Query για Excel. Για πληροφορίες σχετικά με την εγκατάσταση και χρήση του Power Query, ανατρέξτε στο θέμα [Σύνδεση του Excel στο HDInsight με το Power Query][hdinsight-power-query].

> [AZURE.NOTE] Ενώ τα βήματα σε αυτό το άρθρο μπορούν να χρησιμοποιηθούν με σύμπλεγμα μια Linux ή HDInsight που βασίζεται στα Windows, Windows είναι απαραίτητη για το πρόγραμμα-πελάτη σταθμούς εργασίας.

**Προαπαιτούμενα στοιχεία**:

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Σύμπλεγμα του HDInsight**. Για να δημιουργήσετε μια λίστα, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure HDInsight][hdinsight-get-started].
- **A σταθμούς εργασίας** με το Office 2013 Professional Plus, Office 365 Pro Plus, αυτόνομη έκδοση του Excel 2013 ή Office 2010 Professional Plus.


##<a name="install-microsoft-hive-odbc-driver"></a>Εγκατάσταση του προγράμματος οδήγησης Microsoft Hive ODBC

Λήψη και εγκατάσταση του Microsoft Hive το πρόγραμμα οδήγησης ODBC από το [Κέντρο λήψης αρχείων της][hive-odbc-driver-download].

Αυτό το πρόγραμμα οδήγησης μπορεί να εγκατασταθεί σε εκδόσεις 32 bit ή 64 bit των Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 και Windows Server 2012 και θα σας επιτρέψει σύνδεσης Azure HDInsight (έκδοση 1,6 και νεότερες εκδόσεις) και Azure HDInsight προσομοίωσης (v.1.0.0.0 και νεότερες εκδόσεις). Θα πρέπει να εγκαταστήσετε την έκδοση που αντιστοιχεί στην έκδοση της εφαρμογής όπου πρόκειται να χρησιμοποιήσετε το πρόγραμμα οδήγησης ODBC. Για αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιηθεί από το πρόγραμμα οδήγησης από το Office Excel.

##<a name="create-hive-odbc-data-source"></a>Δημιουργία προέλευσης δεδομένων Hive ODBC

Ακολουθήστε τα παρακάτω βήματα που δείχνουν τον τρόπο για να δημιουργήσετε μια προέλευση δεδομένων ODBC Hive.

1. Από το Windows 8 ή Windows 10, πατήστε το πλήκτρο των Windows για να ανοίξετε την οθόνη Έναρξη και, στη συνέχεια, πληκτρολογήστε **προελεύσεις δεδομένων**.
2. Κάντε κλικ στην επιλογή **Ρύθμιση δεδομένων ODBC προέλευσης (32-bit)** ή να **ρυθμίσετε αρχεία προέλευσης δεδομένων ODBC (64 bit)** , ανάλογα με την έκδοση του Office. Εάν χρησιμοποιείτε Windows 7, επιλέξτε **Αρχεία προέλευσης δεδομένων ODBC (32 bit)** ή **Αρχεία προέλευσης δεδομένων ODBC (64 bit)** από **Τα εργαλεία διαχείρισης**. Αυτό θα ξεκινήσει το παράθυρο διαλόγου **Διαχείριση προέλευσης δεδομένων ODBC** .

    ![Το στοιχείο OBDC διαχείρισης προέλευσης δεδομένων][img-hdi-simbahiveodbc-datasource-admin]

3. Από το χρήστη DNS, κάντε κλικ στην επιλογή **Προσθήκη** για να ανοίξετε τον οδηγό **Δημιουργία νέου αρχείου προέλευσης δεδομένων** .
4. Επιλέξτε **Πρόγραμμα οδήγησης ODBC Hive Microsoft**και, στη συνέχεια, κάντε κλικ στο κουμπί **Τέλος**. Αυτό θα ξεκινήσει το παράθυρο διαλόγου **Microsoft Hive ODBC εγκατάσταση προγράμματος οδήγησης DNS** .

5. Πληκτρολογήστε ή επιλέξτε τις ακόλουθες τιμές:

    Ιδιότητα|Περιγραφή
    ---|---
    Όνομα της προέλευσης δεδομένων|Δώστε ένα όνομα στο αρχείο προέλευσης δεδομένων
    Κεντρικός υπολογιστής|Πληκτρολογήστε &lt;HDInsightClusterName >. azurehdinsight.net. Για παράδειγμα, myHDICluster.azurehdinsight.net
    Θύρα|Χρησιμοποιήστε <strong>443</strong>. (Αυτή η θύρα έχει αλλάξει από 563 σε 443.)
    Βάση δεδομένων|Χρησιμοποιήστε την <strong>προεπιλεγμένη</strong>.
    Τύπος διακομιστή ομάδας|Επιλέξτε <strong>την ομάδα διακομιστή 2</strong>
    Μηχανισμός|Επιλέξτε την <strong>Υπηρεσία Azure HDInsight</strong>
    Διαδρομή HTTP|Αφήστε κενό.
    Όνομα χρήστη|Πληκτρολογήστε το όνομα χρήστη χρήστη σύμπλεγμα HDInsight. Αυτό είναι το όνομα χρήστη που δημιουργήσατε κατά τη διαδικασία παροχή σύμπλεγμα. Εάν χρησιμοποιήσατε τη δυνατότητα Γρήγορη δημιουργία, το προεπιλεγμένο όνομα χρήστη είναι <strong>διαχειριστής</strong>.
    Κωδικός πρόσβασης|Πληκτρολογήστε τον κωδικό πρόσβασης χρήστη σύμπλεγμα HDInsight.
    </table>

    Υπάρχουν ορισμένες παράμετροι σημαντικό να έχετε υπόψη όταν κάνετε κλικ στο κουμπί **Επιλογές για προχωρημένους**:

    Παράμετρος|Περιγραφή
    ---|---
    Χρήση εγγενούς ερωτήματος|Όταν είναι επιλεγμένο, το πρόγραμμα οδήγησης ODBC δεν θα προσπαθήσει να μετατρέψετε TSQL σε HiveQL. Θα χρησιμοποιείτε μόνο αν είστε βέβαιοι υποβάλετε αμιγώς δηλώσεις HiveQL 100%. Κατά τη σύνδεση με τον SQL Server ή βάση δεδομένων SQL Azure, πρέπει να αφήσετε απενεργοποιημένο.
    Λήψη ανά μπλοκ γραμμών|Κατά τη λήψη σε μεγάλο όγκο εγγραφές, αυτή η παράμετρος της ρύθμισης ενδέχεται να απαιτείται για να βεβαιωθείτε ότι βέλτιστη επιδόσεις.
    Προεπιλεγμένη μήκος συμβολοσειράς στήλης, μήκος δυαδική στήλη, κλίμακα δεκαδικό στήλης|Ο τύπος δεδομένων μήκη και ένδειξη μπορεί να επηρεάσει την επιστροφή των δεδομένων. Αυτά θα προκαλέσει εσφαλμένες πληροφορίες που θα επιστραφούν λόγω απώλεια ακρίβεια ή/και περικοπή.


    ![Επιλογές για προχωρημένους][img-HiveOdbc-DataSource-AdvancedOptions]

6. Κάντε κλικ στην επιλογή **Δοκιμή** για να ελέγξετε την προέλευση δεδομένων. Όταν το αρχείο προέλευσης δεδομένων έχει ρυθμιστεί σωστά, που εμφανίζει *ΔΟΚΙΜΈΣ ΟΛΟΚΛΗΡΏΘΗΚΕ με ΕΠΙΤΥΧΊΑ!*.
7. Κάντε κλικ στο **κουμπί OK** για να κλείσετε το παράθυρο διαλόγου έλεγχος. Τώρα θα πρέπει να παρατίθενται τη νέα προέλευση δεδομένων στον **Προέλευσης δεδομένων ODBC**.
8. Κάντε κλικ στο **κουμπί OK** για να κλείσετε τον οδηγό.

##<a name="import-data-into-excel-from-hdinsight"></a>Εισαγωγή δεδομένων στο Excel από το HDInsight

Τα παρακάτω βήματα περιγράφουν τον τρόπο εισαγωγής δεδομένων από έναν πίνακα hive σε βιβλίο εργασίας του Excel χρησιμοποιώντας το αρχείο προέλευσης δεδομένων ODBC που δημιουργήσατε στα παραπάνω βήματα.

1. Ανοίξτε ένα νέο ή υπάρχον βιβλίο εργασίας στο Excel.
2. Από την καρτέλα **δεδομένα** , κάντε κλικ στην επιλογή **Από άλλες προελεύσεις δεδομένων**και, στη συνέχεια, κάντε κλικ στην επιλογή **Από Οδηγό σύνδεσης δεδομένων** για να ξεκινήσετε τον **Οδηγό σύνδεσης δεδομένων**.

    ![Οδηγός σύνδεσης δεδομένων Open][img-hdi-simbahiveodbc.excel.dataconnection]

3. Επιλέξτε **ODBC DSN** ως προέλευση δεδομένων και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
4. Από προελεύσεις δεδομένων ODBC, επιλέξτε το όνομα της προέλευσης δεδομένων που δημιουργήσατε στο προηγούμενο βήμα και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
5. Πληκτρολογήστε ξανά τον κωδικό πρόσβασης για το σύμπλεγμα στον οδηγό και, στη συνέχεια, κάντε κλικ στην επιλογή **Δοκιμή** για να επαληθεύσετε τις παραμέτρους και πάλι, εάν είναι απαραίτητο.
6. Κάντε κλικ στο **κουμπί OK** για να κλείσετε το παράθυρο διαλόγου έλεγχος.
7. Κάντε κλικ στο **κουμπί OK**. Περιμένετε για το παράθυρο διαλόγου **Επιλογή βάσης δεδομένων και πίνακα** για να το ανοίξετε. Αυτό μπορεί να χρειαστούν μερικά δευτερόλεπτα.
8. Επιλέξτε τον πίνακα που θέλετε να εισαγάγετε και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. Το *hivesampletable* είναι ένας πίνακας hive δείγματος που παρέχεται με το HDInsight συμπλεγμάτων.  Μπορείτε να το επιλέξετε εάν δεν έχετε δημιουργήσει ένα. Για περισσότερες πληροφορίες σχετικά με την εκτέλεση Hive ερωτήματα και δημιουργία πινάκων Hive, ανατρέξτε στο θέμα [Χρήση Hive με HDInsight][hdinsight-use-hive].
8. Κάντε κλικ στο κουμπί **Τέλος**.
9. Στο παράθυρο διαλόγου **Εισαγωγή δεδομένων** , μπορείτε να αλλάξετε ή να καθορίσετε το ερώτημα. Για να το κάνετε αυτό, κάντε κλικ στην επιλογή **Ιδιότητες**. Αυτό μπορεί να χρειαστούν μερικά δευτερόλεπτα.
10. Κάντε κλικ στην καρτέλα **Ορισμός** και, στη συνέχεια, να προσαρτήσετε **ΌΡΙΟ 200** σε πρόταση select ομάδας στο πλαίσιο κειμένου **κείμενο εντολής** . Η τροποποίηση θα περιορίσει την εγγραφή που επιστρέφεται σύνολο σε 200.

    ![Ιδιότητες σύνδεσης][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Κάντε κλικ στο **κουμπί OK** για να κλείσετε το παράθυρο διαλόγου Ιδιότητες σύνδεσης.
12. Κάντε κλικ στο **κουμπί OK** για να κλείσετε το παράθυρο διαλόγου **Εισαγωγή δεδομένων** .  
13. Πληκτρολογήστε ξανά τον κωδικό πρόσβασης και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**. Χρειάζονται μερικά δευτερόλεπτα πριν λαμβάνει εισαγωγή δεδομένων στο Excel.

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο μάθατε πώς μπορείτε να χρησιμοποιήσετε το πρόγραμμα οδήγησης ODBC Hive της Microsoft για να ανακτήσετε δεδομένα από την υπηρεσία HDInsight στο Excel. Ομοίως, μπορείτε να ανακτήσετε δεδομένα από την υπηρεσία HDInsight σε βάση δεδομένων SQL. Επίσης, είναι δυνατή η αποστολή δεδομένων σε μια υπηρεσία HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Ανάλυση των δεδομένων καθυστέρηση πτήσεων χρησιμοποιώντας HDInsight][hdinsight-analyze-flight-data]
- [Αποστολή δεδομένων σε HDInsight][hdinsight-upload-data]
- [Χρήση Sqoop με HDInsight] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png
