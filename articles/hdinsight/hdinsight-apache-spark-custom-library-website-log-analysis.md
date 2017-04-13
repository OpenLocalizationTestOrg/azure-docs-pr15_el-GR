<properties 
    pageTitle="Χρησιμοποιήστε προσαρμοσμένες βιβλιοθήκες με ένα σύμπλεγμα HDInsight τους για να αναλύσετε αρχεία καταγραφής από την τοποθεσία Web του | Microsoft Azure" 
    description="Χρήση προσαρμοσμένης βιβλιοθηκών με ένα σύμπλεγμα HDInsight τους για να αναλύσετε αρχεία καταγραφής από την τοποθεσία Web" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>

# <a name="analyze-website-logs-using-a-custom-library-with-apache-spark-cluster-on-hdinsight-linux"></a>Ανάλυση αρχεία καταγραφής τοποθεσίας Web, χρησιμοποιώντας μια προσαρμοσμένη βιβλιοθήκη με σύμπλεγμα Apache τους σε HDInsight Linux

Αυτό το Σημειωματάριο παρουσιάζει πώς μπορείτε να αναλύσετε δεδομένα του αρχείου καταγραφής χρησιμοποιώντας μια προσαρμοσμένη βιβλιοθήκη με τους σε HDInsight. Η προσαρμοσμένη βιβλιοθήκη χρησιμοποιούμε είναι μια βιβλιοθήκη Python που ονομάζεται **iislogparser.py**.

> [AZURE.TIP] Αυτό το πρόγραμμα εκμάθησης είναι επίσης διαθέσιμη ως Jupyter σημειωματαρίου σε ένα σύμπλεγμα τους (Linux) που δημιουργείτε στο HDInsight. Η εμπειρία σημειωματαρίου σας επιτρέπει να εκτελέσετε τα τμήματα κώδικα Python από το Σημειωματάριο ίδια. Για να εκτελέσετε το πρόγραμμα εκμάθησης από μέσα σε ένα σημειωματάριο, δημιουργήστε ένα σύμπλεγμα τους, εκκίνηση ενός σημειωματαρίου Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), και, στη συνέχεια, εκτελέστε το Σημειωματάριο **ανάλυση συνδέεται με τους χρησιμοποιώντας ένα προσαρμοσμένο library.ipynb** κάτω από το φάκελο **PySpark** .

**Προαπαιτούμενα στοιχεία:**

Πρέπει να έχετε τα εξής:

- Μια συνδρομή του Azure. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Ένα σύμπλεγμα Apache τους σε HDInsight Linux. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία τους Apache συμπλεγμάτων στο Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Αποθήκευση ανεπεξέργαστα δεδομένα ως μια RDD

Σε αυτήν την ενότητα, μπορούμε να χρησιμοποιήσουμε το Σημειωματάριο [Jupyter](https://jupyter.org) που σχετίζεται με ένα σύμπλεγμα Apache τους στο HDInsight για να εκτελέσετε εργασίες που επεξεργάζονται το ανεπεξέργαστο δείγμα δεδομένων και αποθηκεύστε το ως ομάδα πίνακα. Το δείγμα δεδομένων είναι ένα αρχείο .csv (hvac.csv) διαθέσιμη σε όλα συμπλεγμάτων από προεπιλογή.

Όταν τα δεδομένα σας αποθηκεύεται ως ομάδα πίνακα, στην επόμενη ενότητα θα σας θα συνδεθείτε στον πίνακα Hive χρησιμοποιώντας εργαλεία Επιχειρηματικής ευφυΐας, όπως το Power BI και Tableau.

1. Από την [Πύλη Azure](https://portal.azure.com/), από την startboard, κάντε κλικ στο πλακίδιο για το σύμπλεγμα τους (εάν καρφιτσωμένα αυτό για να το startboard). Μπορείτε επίσης να μεταβείτε σε το σύμπλεγμά σας στην περιοχή **Αναζήτηση όλων** > **Συμπλεγμάτων HDInsight**.   

2. Από το σύμπλεγμα blade τους, κάντε κλικ στην επιλογή **Πίνακας εργαλείων σύμπλεγμα**και, στη συνέχεια, κάντε κλικ στην επιλογή **Jupyter σημειωματαρίου**. Εάν σας ζητηθεί, πληκτρολογήστε τα διαπιστευτήρια διαχειριστή για το σύμπλεγμα.

    > [AZURE.NOTE] Μπορείτε επίσης μπορεί να φτάσει στο Σημειωματάριο Jupyter για το σύμπλεγμά σας ανοίγοντας την παρακάτω διεύθυνση URL στο πρόγραμμα περιήγησης. Αντικαταστήστε το __CLUSTERNAME__ με το όνομα του συμπλέγματος:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Δημιουργήστε ένα νέο σημειωματάριο. Κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **PySpark**.

    ![Δημιουργία νέου σημειωματαρίου Jupyter] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.createnotebook.png "Δημιουργία νέου σημειωματαρίου Jupyter")

3. Δημιουργείται ένα νέο σημειωματάριο και ανοίγει με το όνομα Untitled.pynb. Κάντε κλικ στο όνομα του σημειωματαρίου στο επάνω μέρος και πληκτρολογήστε ένα φιλικό όνομα.

    ![Παροχή ένα όνομα για το Σημειωματάριο] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.notebook.name.png "Παροχή ένα όνομα για το Σημειωματάριο")

4. Επειδή έχετε δημιουργήσει ένα σημειωματάριο χρησιμοποιώντας το PySpark πυρήνα, δεν χρειάζεται να δημιουργήσετε οποιαδήποτε περιβάλλοντα ρητά. Τα περιβάλλοντα τους και η ομάδα θα δημιουργηθεί αυτόματα για εσάς όταν εκτελείτε το πρώτο κελί κώδικα. Μπορείτε να ξεκινήσετε με την εισαγωγή τους τύπους που απαιτούνται για αυτό το σενάριο. Επικολλήστε το εξής τμήμα κώδικα σε ένα κενό κελί και, στη συνέχεια, πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**.


        from pyspark.sql import Row
        from pyspark.sql.types import *


5. Δημιουργήστε μια RDD χρησιμοποιώντας το δείγμα δεδομένων καταγραφής ήδη διαθέσιμα στο σύμπλεγμα. Μπορείτε να αποκτήσετε πρόσβαση τα δεδομένα του προεπιλεγμένου λογαριασμού χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμα στο **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.


        logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


6. Ανακτήστε ένα αρχείο καταγραφής δείγμα ρύθμιση για να βεβαιωθείτε ότι το προηγούμενο βήμα ολοκληρώθηκε με επιτυχία.

        logs.take(5)

    Θα πρέπει να δείτε το αποτέλεσμα παρόμοιο με το εξής:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Ανάλυση των δεδομένων αρχείου καταγραφής χρησιμοποιώντας μια προσαρμοσμένη βιβλιοθήκη Python

7. Στο αποτέλεσμα του παραπάνω, οι πρώτες δύο γραμμές περιλαμβάνουν τις πληροφορίες κεφαλίδας και κάθε υπόλοιπη γραμμή συμφωνεί με το σχήμα που περιγράφονται σε αυτήν την κεφαλίδα. Κατά την ανάλυση όπως αρχεία καταγραφής μπορεί να είναι περίπλοκη. Επομένως, χρησιμοποιούμε μια προσαρμοσμένη βιβλιοθήκη Python (**iislogparser.py**) που το κάνει κατά την ανάλυση όπως αρχεία καταγραφής πολύ πιο εύκολη. Από προεπιλογή, αυτή η βιβλιοθήκη περιλαμβάνεται με το σύμπλεγμά σας τους σε HDInsight στο **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Ωστόσο, δεν είναι σε αυτήν τη βιβλιοθήκη του `PYTHONPATH` , ώστε να δεν είναι δυνατό να χρησιμοποιήσουμε το χρησιμοποιώντας μια εντολή εισαγωγή όπως `import iislogparser`. Για να χρησιμοποιήσετε αυτήν τη βιβλιοθήκη, θα σας πρέπει να τη διανείμετε σε όλους τους κόμβους εργασίας. Εκτελέστε το ακόλουθο τμήμα κώδικα.


        sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


9. `iislogparser`παρέχει μια συνάρτηση `parse_log_line` που επιστρέφει `None` εάν μια γραμμή του αρχείου καταγραφής είναι μια γραμμή κεφαλίδων και επιστρέφει μια παρουσία του `LogLine` κλάση εάν συναντά μια γραμμή του αρχείου καταγραφής. Χρησιμοποιήστε το `LogLine` τάξης για να εξαγάγετε μόνο τις γραμμές του αρχείου καταγραφής από την RDD:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()


10. Ανακτήστε ορισμένες γραμμές που έχουν εξαχθεί αρχείου καταγραφής για να βεβαιωθείτε ότι το βήμα ολοκληρώθηκε με επιτυχία.

        logLines.take(2)

    Το αποτέλεσμα θα πρέπει να είναι παρόμοια με τα εξής:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]


11. Το `LogLine` κλάση, με τη σειρά περιλαμβάνει ορισμένες χρήσιμες μεθόδων, όπως `is_error()`, που επιστρέφει εάν μια καταχώρηση αρχείου καταγραφής έχει έναν κωδικό σφάλματος. Χρησιμοποιήστε αυτήν την επιλογή για να υπολογίσετε τον αριθμό των σφαλμάτων στις γραμμές που έχουν εξαχθεί καταγραφής και, στη συνέχεια, συνδεθείτε όλα τα σφάλματα σε ένα διαφορετικό αρχείο.

        errors = logLines.filter(lambda p: p.is_error())
        numLines = logLines.count()
        numErrors = errors.count()
        print 'There are', numErrors, 'errors and', numLines, 'log entries'
        errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

    Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There are 30 errors and 646 log entries

12. Μπορείτε επίσης να χρησιμοποιήσετε **Matplotlib** για να δημιουργήσετε μια απεικόνιση των δεδομένων. Για παράδειγμα, εάν θέλετε να απομονώσετε την αιτία των αιτήσεων που εκτελούνται για μεγάλο χρονικό διάστημα, μπορεί να θέλετε να βρείτε τα αρχεία που χρειαστεί περισσότερο χρόνο για να λειτουργήσει κατά μέσο όρο.
Το παρακάτω τμήμα κώδικα ανακτά την επάνω 25 πόρους που χρειάστηκαν οι περισσότερες χρόνο για να λειτουργήσει μια αίτηση.

        def avgTimeTakenByKey(rdd):
            return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                    lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                    lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                      .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))
            
        avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

    Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [(u'/blogposts/mvc4/step13.png', 197.5),
         (u'/blogposts/mvc2/step10.jpg', 179.5),
         (u'/blogposts/extractusercontrol/step5.png', 170.0),
         (u'/blogposts/mvc4/step8.png', 159.0),
         (u'/blogposts/mvcrouting/step22.jpg', 155.0),
         (u'/blogposts/mvcrouting/step3.jpg', 152.0),
         (u'/blogposts/linqsproc1/step16.jpg', 138.75),
         (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
         (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
         (u'/blogposts/nested/step2.jpg', 126.0),
         (u'/blogposts/adminpack/step1.png', 124.0),
         (u'/BlogPosts/datalistpaging/step2.png', 118.0),
         (u'/blogposts/mvc4/step35.png', 117.0),
         (u'/blogposts/mvcrouting/step2.jpg', 116.5),
         (u'/blogposts/aboutme/basketball.jpg', 109.0),
         (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
         (u'/blogposts/mvc4/step12.png', 106.0),
         (u'/blogposts/linq8/step0.jpg', 105.5),
         (u'/blogposts/mvc2/step18.jpg', 104.0),
         (u'/blogposts/mvc2/step11.jpg', 104.0),
         (u'/blogposts/mvcrouting/step1.jpg', 104.0),
         (u'/blogposts/extractusercontrol/step1.png', 103.0),
         (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
         (u'/blogposts/mvcrouting/step21.jpg', 101.0),
         (u'/blogposts/mvc4/step1.png', 98.0)]


13. Μπορείτε, επίσης, να εμφανίσετε αυτές τις πληροφορίες στη φόρμα της σχεδίασης. Ως πρώτο βήμα για τη δημιουργία μιας σχεδίασης, ενημερώστε μας πρώτα να δημιουργήσετε ένα προσωρινό πίνακα **AverageTime**. Στον πίνακα των ομάδων τα αρχεία καταγραφής από χρόνο για να δείτε εάν υπάρχουν οποιαδήποτε αιχμές ασυνήθιστο λανθάνων χρόνος σε οποιαδήποτε συγκεκριμένη χρονική στιγμή.

        avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
        schema = StructType([StructField('Minutes', IntegerType(), True),
                             StructField('Time', FloatType(), True)])
                             
        avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
        avgTimeTakenByMinuteDF.registerTempTable('AverageTime')

14. Στη συνέχεια, μπορείτε να εκτελέσετε το ακόλουθο ερώτημα SQL για να λάβετε όλες τις εγγραφές στον πίνακα **AverageTime** .

        %%sql -o averagetime
        SELECT * FROM AverageTime

    Το `%%sql` μαγικός ακολουθούμενο από `-o averagetime` εξασφαλίζει ότι το αποτέλεσμα του ερωτήματος διατηρηθεί τοπικά στο διακομιστή Jupyter (συνήθως το headnode του συμπλέγματος). Το αποτέλεσμα είναι σταθερές ως μια dataframe [Pandas](http://pandas.pydata.org/) με το καθορισμένο όνομα **averagetime**.

    Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:

    ![Αποτέλεσμα του ερωτήματος SQL] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/sql.output.png "Αποτέλεσμα του ερωτήματος SQL")

    Για περισσότερες πληροφορίες σχετικά με το `%%sql` μαγικός, καθώς και άλλες magics είναι διαθέσιμη με το PySpark πυρήνα, ανατρέξτε στο θέμα [πυρήνων διαθέσιμη Jupyter σημειωματάρια με συμπλεγμάτων HDInsight τους](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

15. Τώρα, μπορείτε να χρησιμοποιήσετε Matplotlib, μια βιβλιοθήκη που χρησιμοποιούνται για τη δημιουργία απεικόνισης δεδομένων, για να δημιουργήσετε μια σχεδίασης. Επειδή η σχεδίαση πρέπει να δημιουργηθεί από το dataframe τοπικά μόνιμων **averagetime** , το τμήμα κώδικα πρέπει να ξεκινούν με το `%%local` μαγικός. Αυτό εξασφαλίζει ότι ο κώδικας εκτελείται τοπικά στο διακομιστή Jupyter.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
        plt.xlabel('Time (min)')
        plt.ylabel('Average time taken for request (ms)')

    Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:

    ![Matplotlib εξόδου] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdi-apache-spark-web-log-analysis-plot.png "Matplotlib εξόδου")

16. Αφού ολοκληρώσετε την εκτέλεση της εφαρμογής, θα πρέπει να τερματισμού στο Σημειωματάριο για να αφήσετε τους πόρους. Για να το κάνετε αυτό, από το μενού **αρχείο** στο Σημειωματάριο, κάντε κλικ στην επιλογή **Κλείσιμο και διακοπή**. Αυτό θα τερματισμού και κλείσιμο του σημειωματαρίου.
    

## <a name="seealso"></a>Δείτε επίσης


* [Επισκόπηση: Apache τους σε Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Σενάρια

* [Τους με το BI: Εκτέλεση ανάλυσης αλληλεπιδραστικών δεδομένων με χρήση τους σε HDInsight με εργαλεία Επιχειρηματικής ευφυΐας](hdinsight-apache-spark-use-bi-tools.md)

* [Τους με μηχανικής εκμάθησης: χρήση τους σε HDInsight για την ανάλυση δόμησης θερμοκρασίας με τη χρήση δεδομένων HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Τους με μηχανικής εκμάθησης: χρήση τους σε HDInsight πρόβλεψη της εστίασης στα αποτελέσματα ελέγχου](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Τους ροής: Χρήση τους σε HDInsight για τη δημιουργία εφαρμογών σε πραγματικό χρόνο ροής](hdinsight-apache-spark-eventhub-streaming.md)

### <a name="create-and-run-applications"></a>Δημιουργία και εκτέλεση εφαρμογών

* [Δημιουργήστε μια μεμονωμένη εφαρμογή χρησιμοποιώντας Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Απομακρυσμένη εκτέλεση εργασιών σε ένα σύμπλεγμα τους χρησιμοποιώντας Λίβιος](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Εργαλεία και επεκτάσεις

* [Χρησιμοποιήστε HDInsight εργαλεία προσθήκης για IntelliJ ΙΔΈΑ για να δημιουργήσετε και να υποβάλετε τους Scala εφαρμογών](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Χρησιμοποιήστε HDInsight εργαλεία προσθήκης για IntelliJ ΙΔΈΑ για τον εντοπισμό σφαλμάτων εφαρμογών τους από απόσταση](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Χρήση Zeppelin σημειωματάρια με ένα σύμπλεγμα τους σε HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Διαθέσιμο για Jupyter σημειωματαρίου στο σύμπλεγμα τους για HDInsight πυρήνων](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Χρήση εξωτερικών πακέτων με σημειωματάρια Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Εγκατάσταση Jupyter στον υπολογιστή σας και να συνδεθείτε με ένα σύμπλεγμα HDInsight τους](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Διαχείριση πόρων

* [Διαχείριση πόρων για το σύμπλεγμα Apache τους στο Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Παρακολούθηση και ο εντοπισμός σφαλμάτων εργασίες που εκτελείται σε ένα σύμπλεγμα Apache τους στο HDInsight](hdinsight-apache-spark-job-debugging.md)
