<properties
    pageTitle="Εκτελέστε τα δείγματα Hadoop στο HDInsight | Microsoft Azure"
    description="Γρήγορα αποτελέσματα με την υπηρεσία Azure HDInsight με τα δείγματα που παρέχονται. Χρησιμοποιήστε δεσμών ενεργειών του PowerShell που εκτελούνται προγράμματα MapReduce σε συμπλεγμάτων δεδομένων."
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
    ms.date="10/21/2016"
    ms.author="jgao"/>

#<a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Εκτέλεση Hadoop MapReduce δείγματα στο HDInsight που βασίζεται σε Windows

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Ένα σύνολο των δειγμάτων παρατίθενται θα σας βοηθήσουν να αποτελέσματα εκτελείται MapReduce εργασίες σε Hadoop συμπλεγμάτων χρησιμοποιώντας Azure HDInsight. Αυτά τα δείγματα διατίθενται σε κάθε μία από το HDInsight διαχειριζόμενων συμπλεγμάτων που δημιουργείτε. Εκτέλεση αυτά τα δείγματα θα εξοικειωθείτε με τη χρήση των cmdlet του Azure PowerShell για να εκτελέσετε εργασίες σε Hadoop συμπλεγμάτων.

- [**Το Word count**][hdinsight-sample-wordcount]: καταμετρά τις εμφανίσεις του word σε ένα αρχείο κειμένου.
- [**C# ροής καταμέτρηση λέξεων**][hdinsight-sample-csharp-streaming]: καταμετρά τις εμφανίσεις του word σε ένα αρχείο κειμένου χρησιμοποιώντας το περιβάλλον εργασίας ροής Hadoop.
- [**Εφαρμογή εκτίμησης σπιτιών π**][hdinsight-sample-pi-estimator]: χρησιμοποιεί μια στατιστική (ημιπερίοδο Monte Carlo) μέθοδο για να εκτιμήσετε την τιμή του π.
- [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: Εκτελέστε μια γενικής χρήσης GraySort σε ένα αρχείο και 10 GB χρησιμοποιώντας HDInsight. Υπάρχουν τρεις εργασίες για να εκτελέσετε: Teragen για τη δημιουργία των δεδομένων, Terasort για να ταξινομήσετε τα δεδομένα και Teravalidate για να επιβεβαιώσετε ότι τα δεδομένα έχουν σωστά ταξινομηθεί.

>[AZURE.NOTE] Μπορείτε να βρείτε τον πηγαίο κώδικα στο προσάρτημα. 

Υπάρχει πολύ πρόσθετη τεκμηρίωση στο web για τις τεχνολογίες που σχετίζονται με το Hadoop, όπως προγραμματισμού βασίζεται σε Java MapReduce και η ροή και τεκμηρίωση σχετικά με τα cmdlet που χρησιμοποιούνται στο Windows PowerShell δέσμες ενεργειών. Για περισσότερες πληροφορίες σχετικά με αυτούς τους πόρους, ανατρέξτε στα θέματα:

- [Ανάπτυξη προγραμμάτων Java MapReduce για Hadoop σε HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
- [Υποβολή Hadoop εργασιών στο HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Εισαγωγή στις Azure HDInsight][hdinsight-introduction]

Σήμερα, σε πολλά άτομα Επιλέξτε ομάδα και γουρούνι επάνω MapReduce.  Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Χρήση ομάδας σε HDInsight](hdinsight-use-hive.md)
- [Χρήση γουρούνι σε HDInsight](hdinsight-use-pig.md)
 
**Προαπαιτούμενα στοιχεία**:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **ένα σύμπλεγμα HDInsight**. Για οδηγίες σχετικά με τους διάφορους τρόπους με την οποία μπορούν να δημιουργηθούν όπως συμπλεγμάτων, ανατρέξτε στο θέμα [Δημιουργία Hadoop συμπλεγμάτων στο HDInsight](hdinsight-provision-clusters.md).
- **Μια σταθμούς εργασίας με το Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="hdinsight-sample-wordcount"></a>Το Word μέτρηση - Java 

Για να υποβάλετε ένα έργο MapReduce, μπορείτε πρώτα να δημιουργήσετε έναν ορισμό εργασίας MapReduce. Στον ορισμό εργασίας, μπορείτε να καθορίσετε το αρχείο MapReduce προγράμματος βάζο και τη θέση του αρχείου βάζο, το οποίο είναι **wasbs:///example/jars/hadoop-mapreduce-examples.jar**, το όνομα κλάσης και τα ορίσματα.  Το πρόγραμμα MapReduce wordcount δέχεται δύο ορίσματα: το αρχείο προέλευσης που θα χρησιμοποιηθεί για την καταμέτρηση λέξεων και τη θέση για το αποτέλεσμα.

Μπορείτε να βρείτε τον πηγαίο κώδικα στο [παράρτημα A](#apendix-a---the-word-count-MapReduce-program-in-java).

Για τη διαδικασία ανάπτυξης ενός προγράμματος Java MapReduce, ανατρέξτε στο θέμα - [Ανάπτυξη MapReduce Java προγράμματα για Hadoop σε HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
 
**Για να υποβάλετε μια εργασία MapReduce καταμέτρηση του word**

1. Ανοίξτε το **Windows PowerShell ISE**. Για οδηγίες, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure][powershell-install-configure].
2. Επικολλήστε την ακόλουθη δέσμη ενεργειών PowerShell:

        $subscriptionName = "<Azure Subscription Name>"
        $resourceGroupName = "<Resource Group Name>"
        $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name
        
        Select-AzureRmSubscription -SubscriptionName $subscriptionName
        
        # Define the MapReduce job
        $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "wordcount" `
                                    -Arguments "wasbs:///example/data/gutenberg/davinci.txt", "wasbs:///example/data/WordCountOutput1"
        
        # Submit the job and wait for job completion
        $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:" 
        $mrJob = Start-AzureRmHDInsightJob `
                            -ResourceGroupName $resourceGroupName `
                            -ClusterName $clusterName `
                            -HttpCredential $cred `
                            -JobDefinition $mrJobDefinition 
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -JobId $mrJob.JobId 
        
        # Get the job output
        $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
        $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
        $defaultStorageContainer = $cluster.DefaultStorageContainer
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -DefaultStorageAccountName $defaultStorageAccount `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultContainer $defaultStorageContainer  `
            -JobId $mrJob.JobId `
            -DisplayOutputType StandardError

        # Download the job output to the workstation
        $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
        Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force
        
        # Display the output file
        cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"

    Η εργασία MapReduce δημιουργεί ένα αρχείο που ονομάζεται *τμήμα-r-00000*, που περιέχει λέξεις και τις μετρήσεις. Η δέσμη ενεργειών χρησιμοποιεί την εντολή **findstr** σε λίστα όλες τις λέξεις που περιέχει *"υπάρχει"*.

3. Ορίστε τις πρώτες 3 μεταβλητές και εκτελέστε τη δέσμη ενεργειών.

## <a name="hdinsight-sample-csharp-streaming"></a>Μέτρηση - C# ροής του Word

Hadoop παρέχει ένα API ροής για να MapReduce, που σας επιτρέπει να γράψετε χάρτη και να μειώσετε συναρτήσεις σε γλώσσα διαφορετική από Java.

> [AZURE.NOTE] Τα βήματα σε αυτό το πρόγραμμα εκμάθησης ισχύουν μόνο για συμπλεγμάτων HDInsight που βασίζεται στα Windows. Για ένα παράδειγμα της ροής δεδομένων για συμπλεγμάτων βάσει Linux HDInsight, ανατρέξτε στο θέμα [Ανάπτυξη Python ροής προγραμμάτων για HDInsight](hdinsight-hadoop-streaming-python.md).

Στο παράδειγμα, το πρόγραμμα αντιστοίχισης και το reducer είναι εκτελέσιμων αρχείων που διαβάστε την εισαγωγή δεδομένων από το [stdin] [ stdin-stdout-stderr] (γραμμή) και το αποτέλεσμα στο [stdout]εκπέμπει[stdin-stdout-stderr]. Το πρόγραμμα καταμετρά όλες τις λέξεις στο κείμενο.

Όταν ένα εκτελέσιμο αρχείο έχει οριστεί για **mappers**, κάθε εργασία αντιστοίχισης εκκινεί το εκτελέσιμο αρχείο ως ξεχωριστή διεργασία, όταν το πρόγραμμα αντιστοίχισης έχει προετοιμαστεί. Καθώς η εργασία αντιστοίχισης εκτελείται, μετατρέπει την εισαγωγή δεδομένων σε γραμμές και τροφοδοσιών τις γραμμές για το [stdin] [ stdin-stdout-stderr] της διαδικασίας.

Στο μεταξύ, το πρόγραμμα αντιστοίχισης συλλέγει το αποτέλεσμα μίας γραμμής από το stdout της διαδικασίας. Μετατρέπει κάθε γραμμή σε ένα ζεύγος κλειδιού/τιμής, η οποία συλλέγονται ως το αποτέλεσμα από το πρόγραμμα αντιστοίχισης. Από προεπιλογή, το πρόθεμα γραμμής μέχρι τον πρώτο χαρακτήρα Tab είναι το κλειδί και το υπόλοιπο της γραμμής (εκτός από το χαρακτήρα Tab) είναι η τιμή. Εάν δεν υπάρχει καμία χαρακτήρα Tab στη γραμμή, ολόκληρη τη γραμμή θεωρείται ως κλειδί και η τιμή είναι μηδέν.

Όταν ένα εκτελέσιμο αρχείο έχει οριστεί για **σμίκρυνσης**, κάθε εργασία reducer εκκινεί το εκτελέσιμο αρχείο ως ξεχωριστή διεργασία, όταν το reducer έχει προετοιμαστεί. Ενώ εκτελείται η εργασία reducer, μετατρέπει τα ζεύγη κλειδί/τιμές εισαγωγής σε γραμμές και το τροφοδοσιών τις γραμμές για το [stdin] [ stdin-stdout-stderr] της διαδικασίας.

Στο μεταξύ, η reducer συλλέγει το αποτέλεσμα προσανατολισμένα σε γραμμή από το [stdout] [ stdin-stdout-stderr] της διαδικασίας. Μετατρέπει κάθε γραμμή σε ένα ζεύγος κλειδιού/τιμής, το οποίο συλλέγονται ως το αποτέλεσμα του reducer. Από προεπιλογή, το πρόθεμα γραμμής μέχρι τον πρώτο χαρακτήρα Tab είναι το κλειδί και το υπόλοιπο της γραμμής (εκτός από το χαρακτήρα Tab) είναι η τιμή.

Για περισσότερες πληροφορίες σχετικά με το περιβάλλον Hadoop η ροή εργασίας, ανατρέξτε στο θέμα [Hadoop ροή] [hadoop ροή].

**Για να υποβάλετε μια C# η ροή εργασίας count word**

- Ακολουθήστε τη διαδικασία στην [Καταμέτρηση λέξεων - Java](#word-count-java)και να αντικαταστήσετε τον ορισμό εργασίας με τα εξής:

        $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                                -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                                -Mapper "cat.exe" `
                                -Reducer "wc.exe" `
                                -InputPath "/example/data/gutenberg/davinci.txt" `
                                -OutputPath "/example/data/StreamingOutput/wc.txt"  


    Το αρχείο εξόδου πρέπει να είναι:
    
        example/data/StreamingOutput/wc.txt/part-00000      
                                
## <a name="hdinsight-sample-pi-estimator"></a>Εφαρμογή εκτίμησης σπιτιών π

Η εφαρμογή εκτίμησης π σπιτιών χρησιμοποιεί μια στατιστική (ημιπερίοδο Monte Carlo) μέθοδο για να εκτιμήσετε την τιμή του π. Σημεία τοποθετείται τυχαία μέσα σε μια μονάδα τετράγωνη επίσης εμπίπτουν σε έναν κύκλο που περιλαμβάνονται σε αυτό το τετράγωνο με πιθανότητα ισούται με την περιοχή του κύκλου, π/4. Την τιμή του π μπορεί να υπολογιστεί από την τιμή του 4R, όπου R είναι η αναλογία της τον αριθμό των σημείων που βρίσκονται μέσα στον κύκλο με τον συνολικό αριθμό των σημείων που βρίσκονται μέσα στο τετράγωνο. Τόσο μεγαλύτερη δείγμα των σημείων χρησιμοποιούνται, τόσο καλύτερες την εκτίμηση είναι.

Η δέσμη ενεργειών που παρέχεται για αυτό το δείγμα υποβάλλει μια εργασία βάζο Hadoop και έχει οριστεί προς τα επάνω για να εκτελέσετε με μια τιμή 16 χάρτες, κάθε μία από τις οποίες απαιτείται από τις τιμές παραμέτρων για τον υπολογισμό σημεία δείγματος 10 εκατομμύρια. Αυτές οι τιμές παραμέτρων μπορεί να αλλάξει για να βελτιώσετε την εκτιμώμενη τιμή του π. Για αναφορά, τα πρώτα 10 δεκαδικά ψηφία του π είναι 3.1415926535.

**Για να υποβάλετε μια εργασία Εφαρμογή εκτίμησης σπιτιών π**

- Ακολουθήστε τη διαδικασία στην [Καταμέτρηση λέξεων - Java](#word-count-java)και να αντικαταστήσετε τον ορισμό εργασίας με τα εξής:

        $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "pi" `
                                    -Arguments "16", "10000000"

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB Graysort

Αυτό το δείγμα χρησιμοποιεί έναν μέτρια 10GB δεδομένων έτσι ώστε να εκτελείται σχετικά γρήγορα. Χρησιμοποιεί τις εφαρμογές MapReduce που αναπτύχθηκε από τη Owen O'Malley και Arun Murthy που το ετήσιο συγκριτικών ταξινόμηση terabyte γενικής χρήσης ("daytona") στο 2009 με το ρυθμό 0.578 TB/min (100 TB σε 173 λεπτά). Για περισσότερες πληροφορίες σε αυτό και σε άλλα σημεία αναφοράς ταξινόμησης, ανατρέξτε στην τοποθεσία [Sortbenchmark](http://sortbenchmark.org/) .

Αυτό το δείγμα χρησιμοποιεί τρεις σύνολα MapReduce προγράμματα:

1. **TeraGen** είναι ένα πρόγραμμα MapReduce που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε τις γραμμές δεδομένων για να ταξινομήσετε.
2. **TeraSort** δειγμάτων τα δεδομένα εισόδου και χρησιμοποιεί MapReduce για να ταξινομήσετε τα δεδομένα σε ένα σύνολο παραγγελίας. TeraSort είναι μια βασική ταξινόμηση των συναρτήσεων MapReduce, εκτός από ένα προσαρμοσμένο partitioner που χρησιμοποιεί μια ταξινομημένη λίστα με τα πλήκτρα N-1 δειγματοληψία που ορίζουν την περιοχή κλειδιού για κάθε μείωση. Συγκεκριμένα, όλα τα πλήκτρα αυτών που δείγμα [i-1] < = κλειδί < αποστέλλονται δείγμα [i] για να μειώσετε i. Αυτό εγγυήσεις ότι τα προϊόντα των μείωση i όλα είναι μικρότερη από την έξοδο μείωση i + 1.
3. **TeraValidate** είναι ένα πρόγραμμα MapReduce που επαληθεύει ότι το αποτέλεσμα έχει ταξινομηθεί καθολικά. Δημιουργεί ένα χάρτη ανά αρχείο στον κατάλογο εξόδου και κάθε αντιστοίχιση εξασφαλίζει ότι κάθε κλειδί είναι μικρότερη ή ίση στην προηγούμενη διαφάνεια. Η συνάρτηση χάρτη δημιουργεί επίσης τις εγγραφές από το πρώτο και το τελευταίο πλήκτρα κάθε αρχείου και, η συνάρτηση μείωση εξασφαλίζει ότι το πρώτο πλήκτρο του αρχείου i είναι μεγαλύτερο από το τελευταίο κλειδί του αρχείου i-1. Προβλήματα αναφέρονται ως αποτέλεσμα την μείωση με τα πλήκτρα που είναι εκτός λειτουργίας.

Η μορφή εισόδου και εξόδου, που χρησιμοποιείται από όλες τις τρεις εφαρμογές, διαβάζει και καταγράφει τα αρχεία κειμένου με τη σωστή μορφή. Το αποτέλεσμα της μειώνουν έχει αναπαραγωγής στην τιμή 1, αντί για την προεπιλεγμένη 3, επειδή το διαγωνισμό συγκριτικών δεν απαιτεί ότι τα δεδομένα εξόδου να αναπαραχθούν με πολλούς κόμβους.

Τρεις εργασίες απαιτούνται από το δείγμα, κάθε αντιστοιχεί σε ένα από τα προγράμματα MapReduce περιγράφεται στην εισαγωγή:

1. Δημιουργία των δεδομένων για την ταξινόμηση κατά την εκτέλεση της εργασίας MapReduce **TeraGen** .
2. Ταξινόμηση των δεδομένων κατά την εκτέλεση της εργασίας MapReduce **TeraSort** .
3. Επιβεβαιώστε ότι τα δεδομένα έχει ταξινομηθεί σωστά κατά την εκτέλεση της εργασίας **TeraValidate** MapReduce.

**Για να υποβάλετε τις εργασίες**

- Ακολουθήστε τη διαδικασία στην [Καταμέτρηση λέξεων - Java](#word-count-java)και χρησιμοποιήστε την εξής ορισμοί:

    $teragen = δημιουργία AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - όνομα κλάσης "teragen" '-ορίσματα "-Dmapred.map.tasks=50", "100000000", "/ παράδειγμα / / 10 GB-ταξινόμηση-εισαγωγή δεδομένων"
    
    $terasort = δημιουργία AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - όνομα κλάσης "terasort" '-ορίσματα "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/ παράδειγμα / / 10 GB-ταξινόμηση-εισαγωγή δεδομένων", "/ παράδειγμα / / 10 GB-ταξινόμηση-εξόδου δεδομένων"
    
    $teravalidate = δημιουργία AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - όνομα κλάσης "teravalidate" '-ορίσματα "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/ παράδειγμα / / 10 GB-ταξινόμηση-εξόδου δεδομένων", "/ παράδειγμα/δεδομένων/10 GB-ταξινόμηση-επικύρωση"


##<a name="next-steps"></a>Επόμενα βήματα 

Από αυτό το άρθρο και στα άρθρα σε κάθε ένα από τα δείγματα, μάθατε πώς μπορείτε να εκτελέσετε τα δείγματα περιλαμβάνεται με το HDInsight συμπλεγμάτων με χρήση του Azure PowerShell. Για προγράμματα εκμάθησης σχετικά με τη χρήση γουρούνι, ομάδας και MapReduce με το HDInsight, ανατρέξτε στα ακόλουθα θέματα:

* [Γρήγορα αποτελέσματα με το Hadoop με ομάδα στο HDInsight για να αναλύσετε Χρήση ακουστικού κινητές συσκευές][hdinsight-get-started]
* [Χρήση γουρούνι με Hadoop σε HDInsight][hdinsight-use-pig]
* [Χρήση της ομάδας με Hadoop σε HDInsight][hdinsight-use-hive]
* [Υποβολή Hadoop εργασιών στο HDInsight] [hdinsight-submit-jobs]
* [Τεκμηρίωση Azure HDInsight SDK][hdinsight-sdk-documentation]
* [Εντοπισμός σφαλμάτων Hadoop στο HDInsight: μηνύματα σφάλματος] [hdinsight-errors]


## <a name="appendix-a---the-word-count-source-code"></a>Παράρτημα A - το Word count πηγαίου κώδικα

    package org.apache.hadoop.examples;
    import java.io.IOException;
    import java.util.StringTokenizer;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

    public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
      }
    }

    public static class IntSumReducer
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
      }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
      System.err.println("Usage: wordcount <in> <out>");
      System.exit(2);
        }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
    }


## <a name="appendix-b---the-word-count-streaming-source-code"></a>Προσάρτημα Β - την καταμέτρηση λέξεων ροή πηγαίου κώδικα

Το πρόγραμμα MapReduce χρησιμοποιεί την εφαρμογή cat.exe ως ένα περιβάλλον εργασίας αντιστοίχισης για τη ροή κειμένου σε κονσόλας, καθώς και την εφαρμογή wc.exe, όπως το περιβάλλον εργασίας μείωση για να μετρήσετε τον αριθμό των λέξεων που διοχετεύονται από ένα έγγραφο. Τόσο το πρόγραμμα αντιστοίχισης reducer ανάγνωση χαρακτήρες, γραμμή προς γραμμή, από την τυπική ροή δεδομένων εισόδου (stdin) και εγγραφή στη ροή τυπική εξόδου (stdout).

    // The source code for the cat.exe (Mapper).

    using System;
    using System.IO;

    namespace cat
    {
        class cat
        {
            static void Main(string[] args)
            {
                if (args.Length > 0)
                {
                    Console.SetIn(new StreamReader(args[0]));
                }

                string line;
                while ((line = Console.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
    }



Ο κώδικας αντιστοίχισης το αρχείο cat.cs χρησιμοποιεί μια [StreamReader] [ streamreader] αντικειμένου για να διαβάσετε τους χαρακτήρες της εισερχόμενης ροής στην κονσόλα, η οποία, στη συνέχεια, εγγράφει η ροή στη ροή τυπική εξόδου με τη στατική [Console.Writeline] [ console-writeline] μέθοδο.


    // The source code for wc.exe (Reducer) is:

    using System;
    using System.IO;
    using System.Linq;

    namespace wc
    {
        class wc
        {
            static void Main(string[] args)
            {
                string line;
                var count = 0;

                if (args.Length > 0){
                    Console.SetIn(new StreamReader(args[0]));
                }

                while ((line = Console.ReadLine()) != null) {
                    count += line.Count(cr => (cr == ' ' || cr == '\n'));
                }
                Console.WriteLine(count);
            }
        }
    }


Ο κώδικας reducer στο αρχείο wc.cs χρησιμοποιεί μια [StreamReader] [ streamreader] αντικειμένου για να διαβάσετε χαρακτήρων από την τυπική ροή δεδομένων εισόδου που έχουν Έξοδος από το πρόγραμμα αντιστοίχισης cat.exe. Καθώς διαβάζει τους χαρακτήρες με το [Console.Writeline] [ console-writeline] μέθοδο, αυτό καταμετρά τις λέξεις μετρώντας κενά διαστήματα και χαρακτήρες τέλος της γραμμής στο τέλος κάθε λέξης. Συντάσσει, στη συνέχεια, το συνολικό στη ροή τυπική εξόδου με το [Console.Writeline] [ console-writeline] μέθοδο.





## <a name="appendix-c---the-pi-estimator-source-code"></a>Προσάρτημα C - το π εφαρμογή εκτίμησης σπιτιών πηγαίου κώδικα

Η εφαρμογή εκτίμησης π σπιτιών κώδικα Java που περιέχει τις συναρτήσεις αντιστοίχισης και reducer είναι διαθέσιμο για επιθεώρηση παρακάτω. Το πρόγραμμα αντιστοίχιση δημιουργεί έναν καθορισμένο αριθμό σημείων τοποθετείται τυχαία μέσα σε ένα τετράγωνο μονάδα και, στη συνέχεια, Καταμετρά τον αριθμό των αυτά τα σημεία που βρίσκονται μέσα στον κύκλο. Το πρόγραμμα reducer συγκεντρώνει σημεία μέτρηση από το mappers και, στη συνέχεια, υπολογίζει την τιμή του π από το τύπου 4R, όπου R είναι η αναλογία της τον αριθμό των σημείων μέτρηση μέσα στον κύκλο με τον συνολικό αριθμό των σημείων που βρίσκονται μέσα στο τετράγωνο.

    /**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements. See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership. The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License. You may obtain a copy of the License at
    *
    * http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or   implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.math.BigDecimal;
    import java.util.Iterator;

    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.BooleanWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Writable;
    import org.apache.hadoop.io.WritableComparable;
    import org.apache.hadoop.io.SequenceFile.CompressionType;
    import org.apache.hadoop.mapred.FileInputFormat;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.MapReduceBase;
    import org.apache.hadoop.mapred.Mapper;
    import org.apache.hadoop.mapred.OutputCollector;
    import org.apache.hadoop.mapred.Reducer;
    import org.apache.hadoop.mapred.Reporter;
    import org.apache.hadoop.mapred.SequenceFileInputFormat;
    import org.apache.hadoop.mapred.SequenceFileOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;


    //A Map-reduce program to estimate the value of Pi
    //using quasi-Monte Carlo method.
    //
    //Mapper:
    //Generate points in a unit square
    //and then count points inside/outside of the inscribed circle of the square.
    //
    //Reducer:
    //Accumulate points inside/outside results from the mappers.
    //Let numTotal = numInside + numOutside.
    //The fraction numInside/numTotal is a rational approximation of
    //the value (Area of the circle)/(Area of the square),
    //where the area of the inscribed circle is Pi/4
    //and the area of unit square is 1.
    //Then, Pi is estimated value to be 4(numInside/numTotal).
    //

    public class PiEstimator extends Configured implements Tool {
    //tmp directory for input/output
    static private final Path TMP_DIR = new Path(
    PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

    //2-dimensional Halton sequence {H(i)},
    //where H(i) is a 2-dimensional point and i >= 1 is the index.
    //Halton sequence is used to generate sample points for Pi estimation.
    private static class HaltonSequence {
    // Bases
    static final int[] P = {2, 3};
    //Maximum number of digits allowed
    static final int[] K = {63, 40};

    private long index;
    private double[] x;
    private double[][] q;
    private int[][] d;

    //Initialize to H(startindex),
    //so the sequence begins with H(startindex+1).
    HaltonSequence(long startindex) {
    index = startindex;
    x = new double[K.length];
    q = new double[K.length][];
    d = new int[K.length][];
    for(int i = 0; i < K.length; i++) {
    q[i] = new double[K[i]];
    d[i] = new int[K[i]];
    }

    for(int i = 0; i < K.length; i++) {
    long k = index;
    x[i] = 0;

    for(int j = 0; j < K[i]; j++) {
    q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
    d[i][j] = (int)(k % P[i]);
    k = (k - d[i][j])/P[i];
    x[i] += d[i][j] * q[i][j];
    }
    }
    }

    //Compute next point.
    //Assume the current point is H(index).
    //Compute H(index+1).
    //@return a 2-dimensional point with coordinates in [0,1)^2
    double[] nextPoint() {
    index++;
    for(int i = 0; i < K.length; i++) {
    for(int j = 0; j < K[i]; j++) {
    d[i][j]++;
    x[i] += q[i][j];
    if (d[i][j] < P[i]) {
    break;
    }
    d[i][j] = 0;
    x[i] -= (j == 0? 1.0: q[i][j-1]);
    }
    }
    return x;
    }
    }

    //Mapper class for Pi estimation.
    //Generate points in a unit square and then
    //count points inside/outside of the inscribed circle of the square.
    public static class PiMapper extends MapReduceBase
    implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

    //Map method.
    //@param offset samples starting from the (offset+1)th sample.
    //@param size the number of samples for this map
    //@param out output {ture->numInside, false->numOutside}
    //@param reporter
    public void map(LongWritable offset,
    LongWritable size,
    OutputCollector<BooleanWritable, LongWritable> out,
    Reporter reporter) throws IOException {

    final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
    long numInside = 0L;
    long numOutside = 0L;

    for(long i = 0; i < size.get(); ) {
    //generate points in a unit square
    final double[] point = haltonsequence.nextPoint();

    //count points inside/outside of the inscribed circle of the square
    final double x = point[0] - 0.5;
    final double y = point[1] - 0.5;
    if (x*x + y*y > 0.25) {
    numOutside++;
    } else {
    numInside++;
    }

    //report status
    i++;
    if (i % 1000 == 0) {
    reporter.setStatus("Generated " + i + " samples.");
    }
    }

    //output map results
    out.collect(new BooleanWritable(true), new LongWritable(numInside));
    out.collect(new BooleanWritable(false), new LongWritable(numOutside));
    }
    }


    //Reducer class for Pi estimation.
    //Accumulate points inside/outside results from the mappers.
    public static class PiReducer extends MapReduceBase
    implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

    private long numInside = 0;
    private long numOutside = 0;
    private JobConf conf; //configuration for accessing the file system

    //Store job configuration.
    @Override
    public void configure(JobConf job) {
    conf = job;
    }


    // Accumulate number of points inside/outside results from the mappers.
    // @param isInside Is the points inside?
    // @param values An iterator to a list of point counts
    // @param output dummy, not used here.
    // @param reporter

    public void reduce(BooleanWritable isInside,
    Iterator<LongWritable> values,
    OutputCollector<WritableComparable<?>, Writable> output,
    Reporter reporter) throws IOException {
    if (isInside.get()) {
    for(; values.hasNext(); numInside += values.next().get());
    } else {
    for(; values.hasNext(); numOutside += values.next().get());
    }
    }

    //Reduce task done, write output to a file.
    @Override
    public void close() throws IOException {
    //write output to a file
    Path outDir = new Path(TMP_DIR, "out");
    Path outFile = new Path(outDir, "reduce-out");
    FileSystem fileSys = FileSystem.get(conf);
    SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
    outFile, LongWritable.class, LongWritable.class,
    CompressionType.NONE);
    writer.append(new LongWritable(numInside), new LongWritable(numOutside));
    writer.close();
    }
    }

    //Run a map/reduce job for estimating Pi.
    //@return the estimated value of Pi.
    public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
    )
    throws IOException {
    //setup job conf
    jobConf.setJobName(PiEstimator.class.getSimpleName());

    jobConf.setInputFormat(SequenceFileInputFormat.class);

    jobConf.setOutputKeyClass(BooleanWritable.class);
    jobConf.setOutputValueClass(LongWritable.class);
    jobConf.setOutputFormat(SequenceFileOutputFormat.class);

    jobConf.setMapperClass(PiMapper.class);
    jobConf.setNumMapTasks(numMaps);

    jobConf.setReducerClass(PiReducer.class);
    jobConf.setNumReduceTasks(1);

    // turn off speculative execution, because DFS doesn't handle
    // multiple writers to the same file.
    jobConf.setSpeculativeExecution(false);

    //setup input/output directories
    final Path inDir = new Path(TMP_DIR, "in");
    final Path outDir = new Path(TMP_DIR, "out");
    FileInputFormat.setInputPaths(jobConf, inDir);
    FileOutputFormat.setOutputPath(jobConf, outDir);

    final FileSystem fs = FileSystem.get(jobConf);
    if (fs.exists(TMP_DIR)) {
     throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
     + " already exists. Please remove it first.");
     }
     if (!fs.mkdirs(inDir)) {
     throw new IOException("Cannot create input directory " + inDir);
     }

     //generate an input file for each map task
     try {
     for(int i=0; i < numMaps; ++i) {
     final Path file = new Path(inDir, "part"+i);
     final LongWritable offset = new LongWritable(i * numPoints);
     final LongWritable size = new LongWritable(numPoints);
     final SequenceFile.Writer writer = SequenceFile.createWriter(
     fs, jobConf, file,
     LongWritable.class, LongWritable.class, CompressionType.NONE);
     try {
     writer.append(offset, size);
     } finally {
     writer.close();
     }
     System.out.println("Wrote input for Map #"+i);
     }

     //start a map/reduce job
     System.out.println("Starting Job");
     final long startTime = System.currentTimeMillis();
     JobClient.runJob(jobConf);
     final double duration = (System.currentTimeMillis() - startTime)/1000.0;
     System.out.println("Job Finished in " + duration + " seconds");

     //read outputs
     Path inFile = new Path(outDir, "reduce-out");
     LongWritable numInside = new LongWritable();
     LongWritable numOutside = new LongWritable();
     SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
     try {
     reader.next(numInside, numOutside);
     } finally {
     reader.close();
     }

     //compute estimated value
     return BigDecimal.valueOf(4).setScale(20)
     .multiply(BigDecimal.valueOf(numInside.get()))
     .divide(BigDecimal.valueOf(numMaps))
     .divide(BigDecimal.valueOf(numPoints));
     } finally {
     fs.delete(TMP_DIR, true);
     }
     }

    //Parse arguments and then runs a map/reduce job.
    //Print output in standard out.
    //@return a non-zero if there is an error. Otherwise, return 0.
     public int run(String[] args) throws Exception {
     if (args.length != 2) {
     System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
     ToolRunner.printGenericCommandUsage(System.err);
     return -1;
     }

     final int nMaps = Integer.parseInt(args[0]);
     final long nSamples = Long.parseLong(args[1]);

     System.out.println("Number of Maps = " + nMaps);
     System.out.println("Samples per Map = " + nSamples);

     final JobConf jobConf = new JobConf(getConf(), getClass());
     System.out.println("Estimated value of Pi is "
     + estimate(nMaps, nSamples, jobConf));
     return 0;
     }

     //main method for running it as a stand alone command.
     public static void main(String[] argv) throws Exception {
     System.exit(ToolRunner.run(null, new PiEstimator(), argv));
     }
     }
     
## <a name="appendix-d---the-10gb-graysort-source-code"></a>Προσάρτημα D - το 10gb graysort πηγαίου κώδικα

Τον κωδικό για το πρόγραμμα TeraSort MapReduce παρέχονται για τον έλεγχο σε αυτήν την ενότητα.


    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     *     http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

    package org.apache.hadoop.examples.terasort;

    import java.io.IOException;
    import java.io.PrintStream;
    import java.net.URI;
    import java.util.ArrayList;
    import java.util.List;

    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.filecache.DistributedCache;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.Partitioner;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;

    /**
     * Generates the sampled split points, launches the job,
     * and waits for it to finish.
     * <p>
     * To run the program:
     * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
     */

    public class TeraSort extends Configured implements Tool {
      private static final Log LOG = LogFactory.getLog(TeraSort.class);

      /**
       * A partitioner that splits text keys into roughly equal
       * partitions in a global sorted order.
       */

      static class TotalOrderPartitioner implements Partitioner<Text,Text>{
        private TrieNode trie;
        private Text[] splitPoints;

        /**
         * A generic trie node
         */
        static abstract class TrieNode {
          private int level;
          TrieNode(int level) {
            this.level = level;
          }
          abstract int findPartition(Text key);
          abstract void print(PrintStream strm) throws IOException;
          int getLevel() {
            return level;
          }
        }

        /**
         * An inner trie node that contains 256 children based on the next
         * character.
         */
        static class InnerTrieNode extends TrieNode {
          private TrieNode[] child = new TrieNode[256];

          InnerTrieNode(int level) {
            super(level);
          }
          int findPartition(Text key) {
            int level = getLevel();
            if (key.getLength() <= level) {
              return child[0].findPartition(key);
            }
            return child[key.getBytes()[level]].findPartition(key);
          }
          void setChild(int idx, TrieNode child) {
            this.child[idx] = child;
          }
          void print(PrintStream strm) throws IOException {
            for(int ch=0; ch < 255; ++ch) {
              for(int i = 0; i < 2*getLevel(); ++i) {
                strm.print(' ');
              }
              strm.print(ch);
              strm.println(" ->");
              if (child[ch] != null) {
                child[ch].print(strm);
              }
            }
          }
        }

        /**
         * A leaf trie node that does string compares to figure out where the given
         * key belongs between lower..upper.
         */
        static class LeafTrieNode extends TrieNode {
          int lower;
          int upper;
          Text[] splitPoints;
          LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
            super(level);
            this.splitPoints = splitPoints;
            this.lower = lower;
            this.upper = upper;
          }
          int findPartition(Text key) {
            for(int i=lower; i<upper; ++i) {
              if (splitPoints[i].compareTo(key) >= 0) {
                return i;
              }
            }
            return upper;
          }
          void print(PrintStream strm) throws IOException {
            for(int i = 0; i < 2*getLevel(); ++i) {
              strm.print(' ');
            }
            strm.print(lower);
            strm.print(", ");
            strm.println(upper);
          }
        }


        /**
         * Read the cut points from the given sequence file.
         * @param fs the file system
         * @param p the path to read
         * @param job the job config
         * @return the strings to split the partitions on
         * @throws IOException
         */
        private static Text[] readPartitions(FileSystem fs, Path p,
                                             JobConf job) throws IOException {
          SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
          List<Text> parts = new ArrayList<Text>();
          Text key = new Text();
          NullWritable value = NullWritable.get();
          while (reader.next(key, value)) {
            parts.add(key);
            key = new Text();
          }
          reader.close();
          return parts.toArray(new Text[parts.size()]);  
        }

        /**
         * Given a sorted set of cut points, build a trie that will find the correct
         * partition quickly.
         * @param splits the list of cut points
         * @param lower the lower bound of partitions 0..numPartitions-1
         * @param upper the upper bound of partitions 0..numPartitions-1
         * @param prefix the prefix that we have already checked against
         * @param maxDepth the maximum depth we will build a trie for
         * @return the trie node that will divide the splits correctly
         */
        private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                          Text prefix, int maxDepth) {
          int depth = prefix.getLength();
          if (depth >= maxDepth || lower == upper) {
            return new LeafTrieNode(depth, splits, lower, upper);
          }
          InnerTrieNode result = new InnerTrieNode(depth);
          Text trial = new Text(prefix);
          // append an extra byte on to the prefix
          trial.append(new byte[1], 0, 1);
          int currentBound = lower;
          for(int ch = 0; ch < 255; ++ch) {
            trial.getBytes()[depth] = (byte) (ch + 1);
            lower = currentBound;
            while (currentBound < upper) {
              if (splits[currentBound].compareTo(trial) >= 0) {
                break;
              }
              currentBound += 1;
            }
            trial.getBytes()[depth] = (byte) ch;
            result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                         maxDepth);
          }
          // pick up the rest
          trial.getBytes()[depth] = 127;
          result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                        maxDepth);
          return result;
        }

        public void configure(JobConf job) {
          try {
            FileSystem fs = FileSystem.getLocal(job);
            Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
            splitPoints = readPartitions(fs, partFile, job);
            trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
          } catch (IOException ie) {
            throw new IllegalArgumentException("can't read paritions file", ie);
          }
        }

        public TotalOrderPartitioner() {
        }

        public int getPartition(Text key, Text value, int numPartitions) {
          return trie.findPartition(key);
        }

      }

      public int run(String[] args) throws Exception {
        LOG.info("starting");
        JobConf job = (JobConf) getConf();
        Path inputDir = new Path(args[0]);
        inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
        Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
        URI partitionUri = new URI(partitionFile.toString() +
                                   "#" + TeraInputFormat.PARTITION_FILENAME);
        TeraInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        job.setJobName("TeraSort");
        job.setJarByClass(TeraSort.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        job.setInputFormat(TeraInputFormat.class);
        job.setOutputFormat(TeraOutputFormat.class);
        job.setPartitionerClass(TotalOrderPartitioner.class);
        TeraInputFormat.writePartitionFile(job, partitionFile);
        DistributedCache.addCacheFile(partitionUri, job);
        DistributedCache.createSymlink(job);
        job.setInt("dfs.replication", 1);
        TeraOutputFormat.setFinalSync(job, true);
        JobClient.runJob(job);
        LOG.info("done");
        return 0;
      }

      /**
       * @param args
       */

      public static void main(String[] args) throws Exception {
        int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
        System.exit(res);
      }
    }














 




[hdinsight-errors]: hdinsight-debug-jobs.md

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md


[powershell-install-configure]: ../powershell-install-configure.md

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
