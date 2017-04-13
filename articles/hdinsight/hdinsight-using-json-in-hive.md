<properties
   pageTitle="Ανάλυση και διαδικασία JSON έγγραφα με ομάδα στο HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε JSON έγγραφα και να αναλύσετε τις με ομάδα στο HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Επεξεργασία και να αναλύσετε JSON εγγράφων με ομάδα στο HDInsight

Μάθετε πώς να επεξεργαστεί και να αναλύσετε JSON αρχείων με την ομάδα του HDInsight. Το ακόλουθο έγγραφο JSON θα χρησιμοποιηθεί στην εκμάθηση

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Μπορείτε να βρείτε το αρχείο στο wasbs://processjson@hditutorialdata.blob.core.windows.net/. Για περισσότερες πληροφορίες σχετικά με τη χρήση χώρος αποθήκευσης αντικειμένων Blob του Azure με το HDInsight, ανατρέξτε στο θέμα [χώρο αποθήκευσης αντικειμένων Blob Azure HDFS συμβατό με χρήση με Hadoop στο HDInsight](hdinsight-hadoop-use-blob-storage.md). Εάν θέλετε, μπορείτε να αντιγράψετε το αρχείο για να το προεπιλεγμένο κοντέινερ του συμπλέγματος.

Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσετε την κονσόλα ομάδας.  Για οδηγίες του ανοίγει κονσόλα Hive, δείτε [Χρήση Hive με Hadoop σε HDInsight με σύνδεση απομακρυσμένης επιφάνειας εργασίας](hdinsight-hadoop-use-hive-remote-desktop.md).

##<a name="flatten-json-documents"></a>Ισοπέδωση JSON εγγράφων

Οι μέθοδοι που αναφέρονται στην επόμενη ενότητα απαιτούν το έγγραφο JSON σε μία γραμμή. Επομένως, πρέπει να ισοπεδώσετε JSON έγγραφο σε μια συμβολοσειρά. Εάν το έγγραφό σας JSON είναι ήδη τίθενται, μπορείτε να παραλείψετε αυτό το βήμα και να μεταβείτε κατευθείαν στην επόμενη ενότητα σε JSON την ανάλυση δεδομένων.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Το αρχείο ανεπεξέργαστα JSON βρίσκεται στην **wasbs://processjson@hditutorialdata.blob.core.windows.net/**. Ο πίνακας *StudentsRaw* Hive οδηγεί σε ανεπεξέργαστα κατάργησης επίπεδη JSON έγγραφο.

Ο πίνακας *StudentsOneLine* Hive θα αποθηκεύσει τα δεδομένα σε το προεπιλεγμένο σύστημα αρχείων HDInsight, κάτω από τη διαδρομή */json/σπουδαστές /* .

Η πρόταση INSERT τη συμπλήρωση του πίνακα StudentOneLine με τα δεδομένα του επίπεδη JSON.

Η πρόταση SELECT επιστρέφει μόνο 1 γραμμή.

Εδώ είναι το αποτέλεσμα της πρότασης SELECT:

![Ισοπέδωση του εγγράφου JSON.][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>Ανάλυση JSON έγγραφα σε ομάδα

Ομάδα παρέχει τρεις διαφορετικές μηχανισμούς για την εκτέλεση ερωτημάτων σε JSON έγγραφα:

- χρήση ΓΡΉΓΟΡΑ\_JSON\_αντικείμενο UDF (συνάρτηση που ορίζονται από το χρήστη)
- Χρησιμοποιήστε το UDF JSON_TUPLE
- χρήση προσαρμοσμένου SerDe
- Γράψτε είστε ο κάτοχος UDF χρησιμοποιώντας Python ή άλλες γλώσσες. Ανατρέξτε στο [άρθρο] [ hdinsight-python] σχετικά με την εκτέλεση τον δικό σας κωδικό Python με ομάδα.

### <a name="use-the-getjsonobject-udf"></a>Χρήση ΓΡΉΓΟΡΑ\_JSON_OBJECT UDF
Ομάδα παρέχει μια ενσωματωμένη UDF που ονομάζεται [λήψη json αντικειμένου](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) που μπορεί να εκτελέσει JSON υποβολή ερωτημάτων κατά το χρόνο εκτέλεσης. Αυτή η μέθοδος δέχεται δύο ορίσματα – το όνομα του πίνακα και το όνομα της μεθόδου που έχει το έγγραφο JSON επίπεδη και το πεδίο JSON που πρέπει να αναλυθούν. Ας δούμε ένα παράδειγμα για να δείτε πώς λειτουργεί αυτό το UDF.

Λάβετε το όνομα και το επώνυμο για κάθε μαθητή

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Παρακάτω θα δείτε το αποτέλεσμα κατά την εκτέλεση αυτού του ερωτήματος στο παράθυρο κονσόλας.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Υπάρχουν ορισμένοι περιορισμοί της το UDF get-json_object.

- Επειδή κάθε πεδίο στο ερώτημα απαιτεί εκ νέου κατά την ανάλυση του ερωτήματος, επηρεάζει τις επιδόσεις.
- ΛΉΨΗ\_JSON_OBJECT() επιστρέφει την αναπαράσταση συμβολοσειράς ενός πίνακα. Για να μετατρέψετε έναν πίνακα Hive αυτό, θα πρέπει να χρησιμοποιήσετε κανονικές εκφράσεις για να αντικαταστήσετε τις αγκύλες ' [' και ']' και, στη συνέχεια, καλέστε επίσης διαίρεσης για να λάβετε έναν πίνακα.


Αυτός είναι ο λόγος Hive wiki συνιστά τη χρήση του json_tuple.  

### <a name="use-the-jsontuple-udf"></a>Χρησιμοποιήστε το UDF JSON_TUPLE

Μια άλλη UDF που παρέχεται από την ομάδα ονομάζεται [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple) που εκτελεί καλύτερα από [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Αυτή η μέθοδος λαμβάνει ένα σύνολο πλήκτρα και μια συμβολοσειρά JSON και επιστρέφει μιας πλειάδας τιμών χρησιμοποιώντας μια λειτουργία. Το παρακάτω ερώτημα επιστρέφει το αναγνωριστικό μαθητές και το βαθμό από το έγγραφο JSON:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

Το αποτέλεσμα της αυτήν τη δέσμη ενεργειών στην κονσόλα ομάδα:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_ΠΛΕΙΆΔΑΣ χρησιμοποιεί τη σύνταξη [πλευρική προβολή](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) στην ομάδα που σας επιτρέπει json\_πλειάδας για να δημιουργήσετε μια εικονική πίνακα εφαρμόζοντας τη συνάρτηση UDT σε κάθε γραμμή του αρχικού πίνακα.  Σύνθετη JSONs γίνονται πολύ δύσχρηστες λόγω τη χρήση επαναλαμβανόμενων της ΠΛΕΥΡΙΚΉ ΠΡΟΒΟΛΉΣ. Επιπλέον, JSON_TUPLE δεν μπορεί να χειριστεί ένθετων JSONs.


###<a name="use-custom-serde"></a>Χρήση προσαρμοσμένου SerDe

SerDe είναι η καλύτερη επιλογή για την ανάλυση ένθετη JSON έγγραφα, σας επιτρέπει να ορίσετε το σχήμα JSON, και να χρησιμοποιήσετε το σχήμα για να αναλύσετε τα έγγραφα. Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσετε ένα από τα πιο δημοφιλή SerDe που έχει αναπτυχθεί από [rcongiu](https://github.com/rcongiu).

**Για να χρησιμοποιήσετε το προσαρμοσμένο SerDe:**

1. Εγκαταστήστε το [Java SE ανάπτυξης κιτ 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Επιλογή της έκδοσης Windows X64 του JDK το, εάν πρόκειται να χρησιμοποιείτε την ανάπτυξη των Windows από το HDInsight

    >[AZURE.WARNING] JDK 1.8 δεν λειτουργεί με αυτό SerDe.

    Μετά την ολοκλήρωση της εγκατάστασης, προσθέστε μια νέα μεταβλητή περιβάλλοντος χρήστη:

    1. Ανοίξτε την **προβολή συστήματος ρυθμίσεις για προχωρημένους** από την οθόνη του Windows.
    2. Κάντε κλικ στο κουμπί **μεταβλητές περιβάλλοντος**.  
    3. Προσθέστε μια νέα μεταβλητή περιβάλλοντος **JAVA_HOME** δείκτη ποντικιού **C:\Program Files\Java\jdk1.7.0_55** ή όπου είναι εγκατεστημένο το JDK.

    ![Ρύθμιση config σωστές τιμές για το JDK][image-hdi-hivejson-jdk]

2. Εγκατάσταση [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)

    Προσθήκη φάκελο bin διαδρομή σας μεταβαίνοντας στο στοιχείο ελέγχου Panel--> Επεξεργασία τις μεταβλητές συστήματος για το λογαριασμό μεταβλητές Environment. Το παρακάτω στιγμιότυπο οθόνης που δείχνει πώς μπορείτε να το κάνετε αυτό.

    ![Ρύθμιση Maven][image-hdi-hivejson-maven]

3. Δημιουργία διπλότυπου στο έργο από [Ομάδα-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github τοποθεσίας. Μπορείτε να το κάνετε αυτό, κάνοντας κλικ στο κουμπί "Λήψη Zip", όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης.

    ![Κλωνοποίηση του έργου][image-hdi-hivejson-serde]

4: μεταβείτε στο φάκελο όπου έχετε λάβει αυτό το πακέτο και πληκτρολογήστε "πακέτο mvn". Αυτό πρέπει να δημιουργήσετε τα αρχεία είναι απαραίτητο βάζο που μπορείτε να αντιγράψετε, στη συνέχεια, επάνω από το σύμπλεγμα.

5: μεταβείτε στο φάκελο προορισμού, κάτω από το ριζικό φάκελο όπου έχετε κάνει λήψη του πακέτου. Αποστολή του αρχείου json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar κεφαλή κόμβου του συμπλέγματος. Να συνήθως τοποθετήσετε κάτω από το φάκελο δυαδικό hive: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin ή κάτι παρόμοιο.

6: η ομάδα σας ζητηθεί, πληκτρολογήστε "Προσθήκη /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar βάζο". Εφόσον σε περίπτωση μου, το βάζο βρίσκεται στο φάκελο C:\apps\dist\hive-0.13.x\bin, να μπορώ να προσθέσω απευθείας το βάζο με το όνομα όπως φαίνεται παρακάτω:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

Τώρα, είστε έτοιμοι να χρησιμοποιήσετε το SerDe για την εκτέλεση ερωτημάτων σε σχέση με το έγγραφο JSON.

Η ακόλουθη πρόταση Δημιουργήστε έναν πίνακα με ένα καθορισμένο σχήμα

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Για να εμφανίσετε το όνομα και το επώνυμο του μαθητή

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Αυτό είναι το αποτέλεσμα από την κονσόλα ομάδας.

![Ερώτημα SerDe 1][image-hdi-hivejson-serde_query1]

Για να υπολογίσετε το άθροισμα των βαθμών που είναι μέρος του εγγράφου JSON

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

Το παραπάνω ερώτημα χρησιμοποιεί [Μεγέθυνση πλευρική προβολή](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF για να αναπτύξετε τον πίνακα βαθμολογίας, έτσι ώστε να είναι η αθροίζονται.

Παρακάτω θα δείτε την έξοδο από την κονσόλα ομάδας.

![Ερώτημα SerDe 2][image-hdi-hivejson-serde_query2]

Για να βρείτε την ΕΠΙΛΟΓΉ των θεμάτων που δίνεται μαθητής έχει έχει βαθμολογηθεί με περισσότερες από 80 σημεία  
      jt. StudentClassCollection.ClassId από json_table jt πλευρική προβολή μεγέθυνση (jt. Συλλογή StudentClassCollection.Score) ως βαθμολογία όπου βαθμολογία > 80.

Το παραπάνω ερώτημα επιστρέφει έναν πίνακα με ομάδα σε αντίθεση με λήψη\_json\_αντικείμενο το οποίο επιστρέφει μια συμβολοσειρά.

![Ερώτημα SerDe 3][image-hdi-hivejson-serde_query3]

Εάν θέλετε να skil ακατάλληλη JSON, στη συνέχεια, όπως εξηγείται στη [σελίδα wiki](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) του SerDe αυτό, μπορείτε να επιτύχετε που, πληκτρολογώντας τον παρακάτω κώδικα:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>Σύνοψη
Εν κατακλείδι, τον τύπο του τελεστή JSON στην ομάδα που επιλέγετε εξαρτάται από το σενάριό σας. Εάν έχετε ένα απλό έγγραφο JSON και έχετε μόνο ένα πεδίο για να κάνετε αναζήτηση σε – μπορείτε να επιλέξετε για να χρησιμοποιήσετε γρήγορα Hive UDF\_json\_αντικειμένου. Εάν έχετε περισσότερες από μία πλήκτρα για να κάνετε αναζήτηση, στη συνέχεια, μπορείτε να χρησιμοποιήσετε json_tuple. Εάν έχετε ένα ένθετων έγγραφο, θα πρέπει να μπορείτε να χρησιμοποιήσετε το SerDe JSON.

Για άλλα σχετικά άρθρα, ανατρέξτε στο θέμα

- [Χρήση της ομάδας και HiveQL με Hadoop στο HDInsight για να αναλύσετε ένα δείγμα αρχείου log4j Apache](hdinsight-use-hive.md)
- [Ανάλυση πτήσεων καθυστέρηση δεδομένων με χρήση της ομάδας στο HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Ανάλυση των δεδομένων Twitter με ομάδα στο HDInsight](hdinsight-analyze-twitter-data.md)
- [Εκτελέστε μια εργασία Hadoop χρησιμοποιώντας DocumentDB και HDInsight](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
