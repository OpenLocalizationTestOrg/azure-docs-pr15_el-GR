<properties 
    pageTitle="Ανάλυση δεδομένων καθυστέρηση πτήσεων με ομάδα στο βάσει Linux HDInsight | Microsoft Azure" 
    description="Μάθετε πώς να χρησιμοποιείτε την ομάδα για να αναλύσετε με δεδομένα πτήσεων σε βάσει Linux HDInsight και, στη συνέχεια, εξαγάγετε τα δεδομένα στη βάση δεδομένων SQL χρησιμοποιώντας Sqoop." 
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
    ms.date="10/11/2016" 
    ms.author="larryfr"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Ανάλυση πτήσεων καθυστέρηση δεδομένων με χρήση της ομάδας στο HDInsight

Μάθετε πώς μπορείτε να αναλύσετε δεδομένα καθυστέρηση πτήσεων χρήση ομάδας σε βάσει Linux HDInsight και, στη συνέχεια, εξαγωγή των δεδομένων σε βάση δεδομένων SQL Azure με χρήση Sqoop.

> [AZURE.NOTE] Ενώ μεμονωμένα τμήματα αυτού του εγγράφου μπορεί να χρησιμοποιηθεί με το HDInsight που βασίζεται σε Windows συμπλεγμάτων (Python και ομάδα, για παράδειγμα), είναι συγκεκριμένες για βάσει Linux συμπλεγμάτων πολλά βήματα. Για τα βήματα που θα λειτουργεί με ένα σύμπλεγμα που βασίζεται στα Windows, ανατρέξτε στο θέμα [ανάλυση πτήσεων καθυστέρηση δεδομένων με χρήση της ομάδας στο HDInsight](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Σύμπλεγμα του HDInsight__. Για οδηγίες σχετικά με τη δημιουργία ενός νέου συμπλέγματος βάσει Linux HDInsight, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με τη χρήση Hadoop με ομάδα στο HDInsight στην Linux](hdinsight-hadoop-linux-tutorial-get-started.md) .

- __Βάση δεδομένων SQL azure__. Θα χρησιμοποιήσετε μια βάση δεδομένων Azure SQL ως χώρος αποθήκευσης δεδομένων προορισμού. Εάν δεν έχετε ήδη μια βάση δεδομένων SQL, ανατρέξτε στο θέμα [βάση δεδομένων SQL πρόγραμμα εκμάθησης: Δημιουργία βάσης δεδομένων SQL σε λεπτά](../sql-database/sql-database-get-started.md).

- __Azure CLI__. Εάν δεν έχετε εγκαταστήσει το Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md) για περισσότερα βήματα.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>Πραγματοποιήστε λήψη των δεδομένων πτήσεων

1. Αναζήτηση για να την [έρευνα και διαχείριση πρωτοποριακή τεχνολογία, γραφείο του μεταφορών στατιστικά στοιχεία][rita-website].
2. Στη σελίδα, επιλέξτε τις ακόλουθες τιμές:

  	| Όνομα | Τιμή |
  	| ---- | ---- |
  	| Φίλτρο έτος | 2013 |
  	| Φιλτράρισμα περίοδο | Ιανουάριος |
  	| Πεδία | Το έτος, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, προορισμού, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Καταργήστε την επιλογή από όλα τα άλλα πεδία |

3. Κάντε κλικ στην επιλογή **λήψη**. 

##<a name="upload-the-data"></a>Κάντε αποστολή των δεδομένων

1. Χρησιμοποιήστε την ακόλουθη εντολή για να αποστείλετε το αρχείο zip στον κόμβο κεφαλής σύμπλεγμα HDInsight:

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Αντικαταστήστε το __όνομα ΑΡΧΕΊΟΥ__ με το όνομα του αρχείου zip. Αντικαταστήστε το __όνομα ΧΡΉΣΤΗ__ με login SSH για το σύμπλεγμα HDInsight. Αντικατάσταση CLUSTERNAME με το όνομα του συμπλέγματος HDInsight.
    
    > [AZURE.NOTE] Εάν χρησιμοποιείτε έναν κωδικό πρόσβασης για τον έλεγχο ταυτότητας του login SSH, θα σας ζητηθεί για τον κωδικό πρόσβασης. Εάν χρησιμοποιείτε ένα δημόσιο κλειδί, ίσως χρειαστεί να χρησιμοποιήσετε το `-i` παραμέτρου και καθορίστε τη διαδρομή προς το αντίστοιχο ιδιωτικό κλειδί. Για παράδειγμα `scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Μόλις ολοκληρωθεί η αποστολή, συνδεθείτε με το σύμπλεγμα χρησιμοποιώντας SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με βάσει Linux HDInsight, ανατρέξτε στα ακόλουθα άρθρα:
    
    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, Unix ή λειτουργικό σύστημα OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. Μόλις συνδεθεί, χρησιμοποιήστε τα ακόλουθα για να αποσυμπιέστε το αρχείο .zip:

        unzip FILENAME.zip
    
    Αυτό θα γίνει εξαγωγή ενός αρχείου .csv που έχει περίπου 60MB μέγεθος.
    
4. Χρησιμοποιήστε τα ακόλουθα για να δημιουργήσετε έναν νέο κατάλογο στην WASB (το store κατανεμημένων δεδομένων που χρησιμοποιούνται από το HDInsight) και αντιγράψτε το αρχείο:

    hdfs dfs - mkdir -p /tutorials/flightdelays/data hdfs dfs-τοποθέτηση FILENAME.csv/προγράμματα εκμάθησης/flightdelays/δεδομένων /
    
##<a name="create-and-run-the-hiveql"></a>Δημιουργία και εκτέλεση του HiveQL

Χρησιμοποιήστε τα παρακάτω βήματα για να εισαγάγετε δεδομένα από το αρχείο CSV σε έναν πίνακα ομάδα με το όνομα __των καθυστερήσεων__.

1. Χρησιμοποιήστε τα ακόλουθα για να δημιουργήσετε και να επεξεργαστείτε ένα νέο αρχείο με το όνομα __flightdelays.hql__:

        nano flightdelays.hql
        
    Χρησιμοποιήστε τα παρακάτω ανάλογα με τα περιεχόμενα αυτού του αρχείου:
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. Χρησιμοποιήστε το __συνδυασμό πλήκτρων Ctrl + X__και κατόπιν __Y__ για να αποθηκεύσετε το αρχείο.

3. Χρησιμοποιήστε τα ακόλουθα για να ξεκινήσετε την ομάδα και εκτελέστε το αρχείο __flightdelays.hql__ :

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] Σε αυτό το παράδειγμα, `localhost` χρησιμοποιείται εφόσον είστε συνδεδεμένοι με το κεφαλής κόμβο του συμπλέγματος HDInsight, δηλαδή όπου εκτελείται HiveServer2.

4. Χρησιμοποιήστε την παρακάτω εντολή για να ανοίξετε μια διαδραστική περίοδο λειτουργίας Beeline:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. Όταν εμφανιστεί το `jdbc:hive2://localhost:10001/>` ερώτηση, χρησιμοποιήστε τα ακόλουθα για να ανακτήσετε δεδομένα από τα δεδομένα που έχουν εισαχθεί πτήσεων καθυστέρηση.

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    Αυτό θα ανακτήσει μια λίστα των πόλεων που αντιμετωπίζουν καθυστερήσεις καιρού, καθώς και την ώρα μέση καθυστέρηση, και να το αποθηκεύσετε σε `/tutorials/flightdelays/output`. Αργότερα, Sqoop θα διαβάσει τα δεδομένα από αυτήν τη θέση και να το εξαγάγετε σε βάση δεδομένων SQL Azure.

6. Για να εξέλθετε από Beeline, εισαγάγετε `!quit` στη γραμμή εντολών.

## <a name="create-a-sql-database"></a>Δημιουργία βάσης δεδομένων SQL

Εάν έχετε ήδη μια βάση δεδομένων SQL, πρέπει να λάβετε το όνομα του διακομιστή. Μπορείτε να τη βρείτε στην [Πύλη του Azure](https://portal.azure.com) επιλέγοντας __Βάσεις δεδομένων SQL__και, στη συνέχεια, φιλτράροντας με βάση το όνομα της βάσης δεδομένων που θέλετε να χρησιμοποιήσετε. Το όνομα του διακομιστή εμφανίζεται στη στήλη __ΔΙΑΚΟΜΙΣΤΉ__ .

Εάν δεν έχετε ήδη μια βάση δεδομένων SQL, χρησιμοποιήστε τις πληροφορίες στο [βάση δεδομένων SQL του προγράμματος εκμάθησης: Δημιουργία βάσης δεδομένων SQL σε λεπτά](../sql-database/sql-database-get-started.md) για να δημιουργήσετε ένα. Θα πρέπει να αποθηκεύσετε το όνομα διακομιστή που χρησιμοποιείται για τη βάση δεδομένων.

##<a name="create-a-sql-database-table"></a>Δημιουργήστε έναν πίνακα βάσης δεδομένων SQL

> [AZURE.NOTE] Υπάρχουν πολλοί τρόποι για να συνδεθείτε με βάση δεδομένων SQL για να δημιουργήσετε έναν πίνακα. Τα παρακάτω βήματα χρησιμοποιούν [FreeTDS](http://www.freetds.org/) από το HDInsight σύμπλεγμα.

1. Χρησιμοποιήστε SSH για να συνδεθείτε με το σύμπλεγμα βάσει Linux HDInsight και εκτελέστε τα ακόλουθα βήματα από την περίοδο λειτουργίας SSH.

3. Χρησιμοποιήστε την ακόλουθη εντολή για να εγκαταστήσετε το FreeTDS:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Μόλις FreeTDS έχει εγκατασταθεί, χρησιμοποιήστε την παρακάτω εντολή για να συνδεθείτε με το διακομιστή βάσης δεδομένων SQL. Αντικαταστήστε __όνομα_διακομιστή__ με το όνομα του διακομιστή βάσης δεδομένων SQL. Αντικαταστήστε __adminLogin__ και __adminPassword__ με login για βάση δεδομένων SQL. Αντικαταστήστε το __όνομα βάσης δεδομένων__ με το όνομα της βάσης δεδομένων.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    Θα λάβετε εξόδου παρόμοια με τα εξής:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Κατά την `1>` ερώτηση, πληκτρολογήστε τις ακόλουθες γραμμές:

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    Όταν το `GO` καταχώρηση δήλωση, θα αξιολογηθεί τις προηγούμενες καταστάσεις. Αυτό θα δημιουργήσει έναν νέο πίνακα με το όνομα __των καθυστερήσεων__, με ένα συγκεντρωτικό ευρετήριο (απαιτείται από βάση δεδομένων SQL).

    Χρησιμοποιήστε τα ακόλουθα για να επαληθεύσετε ότι έχει δημιουργηθεί στον πίνακα:

        SELECT * FROM information_schema.tables
        GO

    Θα πρέπει να βλέπετε εξόδου παρόμοια με τα εξής:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. Πληκτρολογήστε `exit` στο το `1>` μηνύματος για να εξέλθετε από το βοηθητικό πρόγραμμα tsql.
    
##<a name="export-data-with-sqoop"></a>Εξαγωγή δεδομένων με Sqoop

2. Χρησιμοποιήστε την ακόλουθη εντολή για να επαληθεύσετε ότι Sqoop μπορεί να δει τη βάση δεδομένων SQL:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Αυτό θα πρέπει να επιστρέψει μια λίστα των βάσεων δεδομένων, όπως η βάση δεδομένων που δημιουργήσατε προηγουμένως τον πίνακα των καθυστερήσεων του.

3. Χρησιμοποιήστε την ακόλουθη εντολή για να εξαγάγετε δεδομένα από hivesampletable στον πίνακα mobiledata:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    Αυτό καθοδηγεί Sqoop για να συνδεθείτε με βάση δεδομένων SQL, με τη βάση δεδομένων που περιέχει τον πίνακα των καθυστερήσεων, και εξαγωγή δεδομένων από το wasbs: / / / προγράμματα εκμάθησης/flightdelays/εξόδου (όπου θα σας είναι αποθηκευμένο το αποτέλεσμα του ερωτήματος hive νωρίτερα,) στον πίνακα καθυστερήσεις.

4. Μετά την ολοκλήρωση της εντολής, χρησιμοποιήστε τα ακόλουθα για να συνδεθείτε στη βάση δεδομένων με χρήση TSQL:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Μόλις συνδεθεί, χρησιμοποιήστε τις παρακάτω προτάσεις για να επαληθεύσετε ότι έχει εξαχθεί τα δεδομένα στον πίνακα mobiledata:
    
        SELECT * FROM delays
        GO

    Θα πρέπει να μπορείτε να δείτε μια λίστα με δεδομένα του πίνακα. Τύπος `exit` για να εξέλθετε από το βοηθητικό πρόγραμμα tsql.

##<a id="nextsteps"></a>Επόμενα βήματα

Τώρα μπορείτε να κατανοήσετε τον τρόπο για να αποστείλετε ένα αρχείο στο χώρο αποθήκευσης αντικειμένων Blob του Azure, πώς μπορείτε να συμπληρώσετε έναν πίνακα Hive χρησιμοποιώντας τα δεδομένα από το χώρο αποθήκευσης αντικειμένων Blob του Azure, τον τρόπο εκτέλεσης ερωτημάτων Hive και πώς μπορείτε να χρησιμοποιήσετε Sqoop για να εξαγάγετε δεδομένα από HDFS σε μια βάση δεδομένων Azure SQL. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

* [Γρήγορα αποτελέσματα με το HDInsight][hdinsight-get-started]
* [Χρήση της ομάδας με το HDInsight][hdinsight-use-hive]
* [Χρήση Oozie με HDInsight][hdinsight-use-oozie]
* [Χρήση Sqoop με HDInsight][hdinsight-use-sqoop]
* [Χρήση γουρούνι με HDInsight][hdinsight-use-pig]
* [Ανάπτυξη προγραμμάτων Java MapReduce για HDInsight][hdinsight-develop-mapreduce]
* [Ανάπτυξη Hadoop Python streaming προγράμματα για HDInsight][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
