<properties
    pageTitle="Πρόγραμμα εκμάθησης εργοστασίου δεδομένων: πρώτου διοχέτευσης δεδομένων | Microsoft Azure"
    description="Αυτό το πρόγραμμα εκμάθησης εργοστασίου δεδομένων Azure δείχνει πώς μπορείτε να δημιουργήσετε και να προγραμματίσετε μια προέλευση δεδομένων που επεξεργάζεται δεδομένα χρησιμοποιώντας Hive δέσμης ενεργειών σε ένα σύμπλεγμα Hadoop."
    services="data-factory"
    keywords="Azure δεδομένων εργοστασίου πρόγραμμα εκμάθησης, hadoop σύμπλεγμα, hadoop ομάδα"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-pipeline-to-process-data-using-hadoop-cluster"></a>Πρόγραμμα εκμάθησης: Δημιουργία του πρώτου διοχέτευσης την επεξεργασία δεδομένων με χρήση σύμπλεγμα Hadoop 
> [AZURE.SELECTOR]
- [Επισκόπηση και τις προϋποθέσεις](data-factory-build-your-first-pipeline.md)
- [Πύλη του Azure](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Το πρότυπο διαχείρισης πόρων](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε το πρώτο εργοστασίου Azure δεδομένων με μια διαδικασία δεδομένων που επεξεργάζεται δεδομένα, εκτελώντας Hive δέσμης ενεργειών σε ένα σύμπλεγμα Azure HDInsight (Hadoop). 

> [AZURE.NOTE] Σε αυτό το άρθρο παρέχει μια επισκόπηση των Azure εργοστασίου δεδομένων. Για μια επισκόπηση της υπηρεσίας, ανατρέξτε στο θέμα [Εισαγωγή στις εργοστασιακές Azure δεδομένων](data-factory-introduction.md). Ανατρέξτε στο θέμα [διαδρομή εκμάθησης εργοστασίου δεδομένων](https://azure.microsoft.com/documentation/learning-paths/data-factory/) για ένα προτεινόμενο τρόπο για να περιηγηθείτε στις μας περιεχομένου για να μάθετε σχετικά με την προέλευση δεδομένων.

## <a name="whats-covered-in-this-tutorial"></a>Τι καλύπτεται σε αυτό το πρόγραμμα εκμάθησης; 
**Azure εργοστασίου δεδομένων** σάς επιτρέπει να συντάσσετε δεδομένων **κίνηση** και εργασίες **επεξεργασίας** δεδομένων ως ροές εργασίας βασίζονται σε δεδομένα (ονομάζεται επίσης αγωγούς δεδομένων). Μάθετε πώς μπορείτε να δημιουργήσετε την πρώτη δραστηριότητα διοχέτευσης με επεξεργασία δεδομένων (ή μετασχηματισμού δεδομένων) δεδομένων. Αυτή η δραστηριότητα χρησιμοποιεί ένα σύμπλεγμα HDInsight Hadoop για να μετασχηματισμός και να αναλύσετε δείγματα αρχείων καταγραφής web.  

Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να εκτελέσετε τα παρακάτω βήματα:

1.  Δημιουργήστε μια **προέλευση δεδομένων**. Μια προέλευση δεδομένων μπορεί να περιέχει ένα ή περισσότερα δεδομένα διοχετεύσεων που μετακίνηση και την επεξεργασία δεδομένων. 
2.  Δημιουργία **συνδεδεμένων υπηρεσιών**. Μπορείτε να δημιουργήσετε ένα συνδεδεμένο υπηρεσίας για να συνδέσετε ένα χώρο αποθήκευσης δεδομένων ή μια υπηρεσία υπολογισμού για την προέλευση δεδομένων. Ένα χώρο αποθήκευσης δεδομένων, όπως το χώρο αποθήκευσης Azure περιέχει δεδομένα εισόδου/εξόδου δραστηριοτήτων στη διοχέτευση. Μια υπηρεσία υπολογισμού, όπως το HDInsight Hadoop σύμπλεγμα διεργασίες/μετασχηματισμούς δεδομένων.    
3.  Δημιουργία εισόδου και εξόδου **συνόλων δεδομένων**. Ένα σύνολο δεδομένων εισαγωγής αντιπροσωπεύει την είσοδο για μια δραστηριότητα στη διοχέτευση και ένα σύνολο δεδομένων εξόδου αντιπροσωπεύει την έξοδο για τη δραστηριότητα.
3.  Δημιουργία της **διοχέτευσης**. Μια διαδικασία μπορεί να έχει μία ή περισσότερες δραστηριότητες (παραδείγματα: αντιγραφή δραστηριότητα, δραστηριότητας Hive HDInsight). Σε αυτό το παράδειγμα χρησιμοποιεί τη δραστηριότητα HDInsight Hive που εκτελεί μια δέσμη ενεργειών της ομάδας σε ένα σύμπλεγμα HDInsight Hadoop. Η δέσμη ενεργειών πρώτα δημιουργεί έναν πίνακα που αναφέρεται στα δεδομένα του αρχείου καταγραφής ανεπεξέργαστα web αποθηκεύονται στο χώρο αποθήκευσης αντικειμένων blob του Azure και, στη συνέχεια, διαμερίσματα τα ανεπεξέργαστα δεδομένα κατά έτος και μήνα.

    Υπάρχουν δύο τύποι των δραστηριοτήτων που υποστηρίζονται από το Azure εργοστασίου δεδομένων. Είναι: [δεδομένων κίνηση δραστηριότητες](data-factory-data-movement-activities.md) και οι [δραστηριότητες μετασχηματισμού δεδομένων](data-factory-data-transformation-activities.md). Υπάρχει μόνο μία κίνηση η δραστηριότητα δεδομένων, που είναι η δραστηριότητα αντίγραφο. Σε αυτό το πρόγραμμα εκμάθησης, δεν χρησιμοποιείτε τη δραστηριότητα αντίγραφο. Για ένα πρόγραμμα εκμάθησης σχετικά με τη χρήση της δραστηριότητας αντίγραφο, ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: αντιγραφή δεδομένων από μια Azure αντικειμένων blob σε Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Ομάδα HDInsight δραστηριότητα χρησιμοποιείτε σε αυτό το πρόγραμμα εκμάθησης είναι μία από τις δραστηριότητες μετασχηματισμού δεδομένων που υποστηρίζονται από προέλευση δεδομένων.  
 
Παρακάτω θα δείτε την **Προβολή διαγράμματος** του εργοστασίου δείγμα δεδομένων που δημιουργείτε σε αυτό το πρόγραμμα εκμάθησης. 

![Προβολή διαγράμματος στο πρόγραμμα εκμάθησης εργοστασίου δεδομένων](./media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)

Σε αυτό το πρόγραμμα εκμάθησης, ο φάκελος **inputdata** του κοντέινερ αντικειμένων blob του Azure **adfgetstarted** περιέχει ένα αρχείο με όνομα input.log. Αυτό το αρχείο καταγραφής περιλαμβάνει καταχωρήσεις από τρεις μήνες: Ιανουαρίου, Φεβρουαρίου και Μαρτίου του 2016. Ακολουθούν οι γραμμές δείγματος για κάθε μήνα στο αρχείο εισαγωγής. 

    2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Κατά την επεξεργασία του αρχείου μέσω της διοχέτευσης με δραστηριότητα Hive HDInsight, τη δραστηριότητα εκτελεί μια δέσμη ενεργειών Hive στο σύμπλεγμα HDInsight ότι τα διαμερίσματα δεδομένων εισόδου κατά έτος και μήνα. Η δέσμη ενεργειών δημιουργεί τρεις φάκελοι εξόδου που περιέχουν ένα αρχείο με καταχωρήσεις από κάθε μήνα.  

    adfgetstarted/partitioneddata/year=2016/month=1/000000_0
    adfgetstarted/partitioneddata/year=2016/month=2/000000_0
    adfgetstarted/partitioneddata/year=2016/month=3/000000_0

Από τις γραμμές δείγματος που εμφανίζεται παραπάνω, το πρώτο (με 2016-01-01) έχει εγγραφεί στο αρχείο 000000_0 τον μήνα = 1 φάκελος. Ομοίως, στο δεύτερο παράδειγμα είναι γραμμένο με το αρχείο σε μήνα = 2 φακέλου και η τρίτη μία έχει εγγραφεί στο αρχείο τον μήνα = 3 φάκελος.  


## <a name="pre-requisites"></a>Προαπαιτούμενα
Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τις εξής προϋποθέσεις:

1.  **Azure συνδρομή** - Εάν δεν έχετε μια συνδρομή του Azure, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Ανατρέξτε στο άρθρο [Δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/) στον πώς μπορείτε να αποκτήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης.

2.  **Χώρος αποθήκευσης azure** – μπορείτε να χρησιμοποιήσετε ένα λογαριασμό Azure χώρου αποθήκευσης για την αποθήκευση των δεδομένων σε αυτό το πρόγραμμα εκμάθησης. Εάν δεν έχετε ένα λογαριασμό Azure χώρου αποθήκευσης, ανατρέξτε στο άρθρο [Δημιουργία λογαριασμού χώρου αποθήκευσης](../storage/storage-create-storage-account.md#create-a-storage-account) . Αφού δημιουργήσετε το λογαριασμό χώρου αποθήκευσης, σημειώστε προς τα κάτω το **όνομα λογαριασμού** και το **πλήκτρο πρόσβασης**. Ανατρέξτε στο θέμα [Προβολή "," Αντιγραφή "και" πλήκτρα πρόσβασης regenerate χώρου αποθήκευσης](../storage/storage-create-storage-account.md#view-and-copy-storage-access-keys). 

### <a name="upload-files-to-azure-storage-for-the-tutorial"></a>Αποστολή αρχείων στο χώρο αποθήκευσης Azure για το πρόγραμμα εκμάθησης
Πριν να ξεκινήσετε το πρόγραμμα εκμάθησης, πρέπει να προετοιμάσετε το λογαριασμό σας χώρο αποθήκευσης Azure με δείγματα αρχείων για το πρόγραμμα εκμάθησης.

1. Αποστολή αρχείο ερωτήματος ομάδας (HQL) στο φάκελο **δέσμης ενεργειών** του κοντέινερ αντικειμένων blob **adfgetstarted** .
2. Αποστείλετε το αρχείο εισαγωγής **inputdata** φάκελο του κοντέινερ αντικειμένων blob **adfgetstarted** . 

#### <a name="create-hql-script-file"></a>Δημιουργία HQL αρχείο δέσμης ενεργειών 

1. Εκκινήστε **το Σημειωματάριο** και επικολλήστε την ακόλουθη δέσμη ενεργειών HQL. Αυτή η δέσμη ενεργειών Hive δημιουργεί δύο πίνακες: **WebLogsRaw** και **WebLogsPartitioned**. Κάντε κλικ στην επιλογή " **αρχείο** " στο μενού και επιλέξτε **Αποθήκευση ως**. Μεταβείτε στο φάκελο **C:\adfgetstarted** στο σκληρό σας δίσκο. Επιλέξτε * *όλα τα αρχεία (*.*) **για το** Αποθήκευση ως τύπου** πεδίο. Πληκτρολογήστε **partitionweblogs.hql** για το **όνομα αρχείου**. Επιβεβαιώστε ότι το **κωδικοποίηση** πεδίο στο κάτω μέρος του παραθύρου διαλόγου έχει οριστεί **ANSI**. Εάν δεν είναι, ρυθμίστε την σε **ANSI **.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Κατά το χρόνο εκτέλεσης, τη δραστηριότητα Hive στη διοχέτευση εργοστασίου δεδομένων μεταφέρει τιμές τα **inputtable** και τις παραμέτρους **partitionedtable** όπως φαίνεται στην το παρακάτω τμήμα κώδικα:  

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

Το **storageaccountname** είναι το όνομα του λογαριασμού σας Azure χώρου αποθήκευσης.
 
#### <a name="create-a-sample-input-file"></a>Δημιουργία ενός δείγματος αρχείου εισαγωγής
Χρησιμοποιώντας το Σημειωματάριο, δημιουργήστε ένα αρχείο με το όνομα **input.log** στο το **c:\adfgetstarted** με το παρακάτω περιεχόμενο: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

#### <a name="upload-input-file-and-hql-file-to-your-azure-blob-storage"></a>Αποστολή αρχείου εισόδου και το αρχείο HQL στο χώρο αποθήκευσης αντικειμένων Blob του Azure

Αυτή η ενότητα παρέχει οδηγίες σχετικά με τη χρήση του εργαλείου **AzCopy** για να αντιγράψετε τα αρχεία input.log και partitionweblogs.hql στο χώρο αποθήκευσης Blob του Azure. Μπορείτε να χρησιμοποιήσετε οποιοδήποτε εργαλείο της επιλογής σας (για παράδειγμα: [Εξερεύνηση χώρου αποθήκευσης του Microsoft Azure](http://storageexplorer.com/), [CloudXPlorer από λογισμικό ClumsyLeaf](http://clumsyleaf.com/products/cloudxplorer) για να εκτελέσετε αυτήν την εργασία.   
     
1. Κάντε λήψη της [πιο πρόσφατης έκδοσης του **AzCopy**](http://aka.ms/downloadazcopy)ή την [πιο πρόσφατη έκδοση προεπισκόπησης](http://aka.ms/downloadazcopypr). Δείτε [πώς μπορείτε να χρησιμοποιήσετε AzCopy](../storage/storage-use-azcopy.md) άρθρο για οδηγίες σχετικά με τη χρήση βοηθητικού προγράμματος.
2. Μεταβείτε στο φάκελο c:\adfgetstarted και εκτελέστε την ακόλουθη εντολή: 

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy" /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storageaccesskey>  /Pattern:input.log

    Αυτή η εντολή αποστέλλει το αρχείο **input.log** με το λογαριασμό χώρου αποθήκευσης (**adfgetstarted** κοντέινερ και **inputdata** φακέλου). Αντικαταστήστε **storageaccountname** με το όνομα του λογαριασμού χώρου αποθήκευσης και **storageaccesskey** με το πλήκτρο πρόσβασης χώρου αποθήκευσης.

    > [AZURE.NOTE] Αυτή η εντολή δημιουργεί ένα κοντέινερ που ονομάζεται **adfgetstarted** στο χώρο αποθήκευσης αντικειμένων Blob του Azure του και αντιγράφει το αρχείο **input.log** από την τοπική μονάδα δίσκου στο φάκελο **inputdata** στο κοντέινερ. 
    
3. Όταν το αρχείο έχει αποσταλεί με επιτυχία, μπορείτε να δείτε το αποτέλεσμα είναι παρόμοιο με το εξής από AzCopy.
    
        Finished 1 of total 1 file(s).
        [2015/12/16 23:07:33] Transfer summary:
        -----------------
        Total files transferred: 1
        Transfer successfully:   1
        Transfer skipped:        0
        Transfer failed:         0
        Elapsed time:            00.00:00:01
5. Εκτελέστε την παρακάτω εντολή για να αποστείλετε το αρχείο **partitionweblogs.hql** στο φάκελο **δέσμης ενεργειών** του κοντέινερ **adfgetstarted** . Ακολουθεί η εντολή: 
    
        AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storageaccesskey>  /Pattern:partitionweblogs.hql

Έχετε ολοκληρώσει τις προϋποθέσεις. Μπορείτε να δημιουργήσετε μια προέλευση δεδομένων χρησιμοποιώντας έναν από τους εξής τρόπους. Κάντε κλικ σε μία από τις καρτέλες στο επάνω ή τις παρακάτω συνδέσεις για να εκτελέσετε το πρόγραμμα εκμάθησης. 

- [Πύλη του Azure](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Διαχείριση πόρων προτύπου](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)