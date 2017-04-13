<properties
    pageTitle="Δημιουργία συστάσεις χρησιμοποιώντας Mahout και HDInsight που βασίζεται σε WIndows | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε το Apache Mahout μηχανικής εκμάθησης βιβλιοθήκης για τη δημιουργία ταινίας συστάσεις με HDInsight που βασίζεται σε Windows (Hadoop)."
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

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>Δημιουργία ταινίας συστάσεις, χρησιμοποιώντας το Apache Mahout με Hadoop σε HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Μάθετε πώς μπορείτε να χρησιμοποιήσετε το [Apache Mahout](http://mahout.apache.org) μηχανικής εκμάθησης βιβλιοθήκη με Azure HDInsight για τη δημιουργία ταινίας συστάσεις.

> [AZURE.NOTE] Τα βήματα σε αυτό το έγγραφο απαιτεί ένα πρόγραμμα-πελάτη των Windows και ένα σύμπλεγμα HDInsight που βασίζεται στα Windows. Για πληροφορίες σχετικά με τη χρήση Mahout από ένα Linux, OS X ή το πρόγραμμα-πελάτη Unix, με ένα σύμπλεγμα βάσει Linux HDInsight, ανατρέξτε στο θέμα [δημιουργία ταινίας συστάσεις με τη χρήση Mahout Apache με βάσει Linux Hadoop στο HDInsight](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>Τι θα μάθετε

Mahout είναι μια [μηχανικής εκμάθησης] [ ml] βιβλιοθήκη για Apache Hadoop. Mahout περιέχει αλγόριθμους για την επεξεργασία δεδομένων, όπως το φιλτράρισμα, ταξινόμηση, και σύμπλεγμα. Σε αυτό το άρθρο, θα χρησιμοποιήσετε μια μηχανή προτάσεων για τη δημιουργία ταινίας συστάσεις που βασίζονται σε ταινίες είδατε τους φίλους σας. Επίσης θα μάθετε πώς να εκτελείτε ταξινομήσεις με ένα σύμπλεγμα απόφασης. Αυτό θα μάθετε τα εξής:

* Πώς μπορείτε να εκτελέσετε εργασίες Mahout με τη χρήση του Windows PowerShell

* Πώς μπορείτε να εκτελέσετε εργασίες Mahout από τη γραμμή εντολών Hadoop

* Πώς να εγκαταστήσετε Mahout σε HDInsight 3.0 και HDInsight 2.0 συμπλεγμάτων

    > [AZURE.NOTE] Mahout παρέχονται με την έκδοση HDInsight 3.1 το συμπλεγμάτων. Εάν χρησιμοποιείτε μια παλαιότερη έκδοση του HDInsight, ανατρέξτε στο θέμα [Εγκατάσταση του Mahout](#install) πριν να συνεχίσετε.

##<a name="prerequisites"></a>προαπαιτούμενα στοιχεία

- **Σύμπλεγμα βασίζεται σε ένα Windows Hadoop στο HDInsight**. Για πληροφορίες σχετικά με τη δημιουργία μία, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Hadoop στο HDInsight][getstarted]
- **Μια σταθμούς εργασίας με το Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="recommendations"></a>Δημιουργία συστάσεις με χρήση του Windows PowerShell

> [AZURE.NOTE] Παρόλο που η εργασία χρησιμοποιείται σε αυτήν την ενότητα λειτουργεί με χρήση του Windows PowerShell, πολλές από τις κατηγορίες που παρέχονται με το Mahout δεν λειτουργούν αυτήν τη στιγμή με το Windows PowerShell και πρέπει να εκτελεστούν, χρησιμοποιώντας τη γραμμή εντολών Hadoop. Για μια λίστα κατηγοριών που δεν λειτουργούν με το Windows PowerShell, ανατρέξτε στην ενότητα [Αντιμετώπιση προβλημάτων](#troubleshooting) .
>
> Για παράδειγμα χρησιμοποιώντας τη γραμμή εντολών Hadoop για να εκτελέσετε εργασίες Mahout, ανατρέξτε στο θέμα [Ταξινόμηση δεδομένων με χρήση της γραμμής εντολών Hadoop](#classify).

Μία από τις συναρτήσεις που παρέχεται από Mahout είναι ένας μηχανισμός σύσταση. Αυτός ο μηχανισμός αποδέχεται δεδομένα στο πλαίσιο μορφή `userID`, `itemId`, και `prefValue` (τις προτιμήσεις τους χρήστες για το στοιχείο). Mahout, στη συνέχεια, να εκτελέσετε ανάλυση εμφάνιση από κοινού για να προσδιορίσετε: _οι χρήστες που διαθέτουν την προτίμηση για ένα στοιχείο έχουν επίσης μια προτίμηση για αυτά τα άλλα στοιχεία_. Mahout Καθορίζει, στη συνέχεια, οι χρήστες με παρόμοια στοιχείο προτιμήσεις, οι οποίες μπορούν να χρησιμοποιηθούν για να κάνετε συστάσεις.

Ακολουθεί μια πολύ απλό παράδειγμα που χρησιμοποιεί ταινίες:

* __Εμφάνιση από κοινού__: Αλέξανδρος, η Άννα και ο Μπάμπης όλα δηλώσει ότι σας αρέσει _πόλεμοι αστέρι_, _Το αυτοκρατορία απεργίες πίσω_και _επιστροφής από το Jedi_. Mahout Καθορίζει ότι οι χρήστες που όπως επίσης ένα από αυτά τα ταινίες όπως τις άλλες δύο.

* __Εμφάνιση από κοινού__: ο Μπάμπης και η Άννα επίσης δηλώσει ότι σας αρέσει _Η απειλή ανύπαρκτο αντικείμενο_, _επίθεση από τα αντίγραφα_και _Revenge από το Sith_. Mahout Καθορίζει ότι οι χρήστες που τους άρεσε επίσης το προηγούμενο τρεις ταινίες όπως αυτά τα τρία.

* __Ομοιότητα σύσταση__: επειδή Αλέξανδρος αρέσουν τα πρώτα τρία ταινίες, Mahout εξετάζει ταινίες που άλλα άτομα με παρόμοια προτιμήσεις δηλώσει ότι σας αρέσει, αλλά δεν έχει παρακολούθηση Αλέξανδρος (δηλώσει ότι σας αρέσει/χαρακτηρισμός). Σε αυτήν την περίπτωση, Mahout συνιστά _Το απειλή ανύπαρκτο αντικείμενο_, _επίθεση από τα αντίγραφα_και _Revenge από το Sith_.

###<a name="understanding-the-data"></a>Κατανόηση των δεδομένων

Εύκολη, [Έρευνα GroupLens] [ movielens] παρέχει δεδομένα χαρακτηρισμού για ταινίες σε μια μορφή που είναι συμβατή με Mahout. Αυτά τα δεδομένα είναι διαθέσιμη στο το σύμπλεγμά προεπιλεγμένο αποθήκευσης στο `/HdiSamples/MahoutMovieData`.

Υπάρχουν δύο αρχεία, `moviedb.txt` (πληροφορίες σχετικά με τις ταινίες,) και `user-ratings.txt`. Το αρχείο ratings.txt χρήστη χρησιμοποιείται κατά την ανάλυση, ενώ moviedb.txt χρησιμοποιείται για την παροχή πληροφοριών φιλική προς το χρήστη κειμένου κατά την εμφάνιση των αποτελεσμάτων της ανάλυσης.

Τα δεδομένα που περιέχονται στο χρήστη ratings.txt έχει δομή του `userID`, `movieID`, `userRating`, και `timestamp`, που σας ενημερώνει μας πώς ιδιαίτερα κάθε χρήστης με χαρακτηρισμό μια ταινία. Ακολουθεί ένα παράδειγμα των δεδομένων:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>Εκτέλεση της εργασίας

Χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών του Windows PowerShell για να εκτελέσετε μια εργασία που χρησιμοποιεί το μηχανισμό σύσταση Mahout με τα δεδομένα της ταινίας:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
            
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Εργασίες mahout δεν καταργήσει προσωρινά δεδομένα που δημιουργείται κατά την επεξεργασία της εργασίας. Το `--tempDir` παράμετρος καθορίζεται η εργασία παράδειγμα για την απομόνωση τα προσωρινά αρχεία σε έναν συγκεκριμένο κατάλογο.

Η εργασία Mahout δεν επιστρέφει το αποτέλεσμα στο STDOUT. Αντί για αυτό, το αποθηκεύει στον κατάλογο καθορισμένο εξόδου ως __τμήμα-r-00000__. Η δέσμη ενεργειών κάνει λήψη αυτού του αρχείου για να __output.txt__ στον τρέχοντα κατάλογο στην σας σταθμούς εργασίας.

Ακολουθεί ένα παράδειγμα του περιεχομένου αυτού του αρχείου:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

Η πρώτη στήλη είναι η `userID`. Οι τιμές που περιέχονται στο ' [' και ']' είναι `movieId`:`recommendationScore`.

###<a name="view-the-output"></a>Προβάλετε την έξοδο

Παρόλο που το αποτέλεσμα που δημιουργήθηκε μπορεί να είναι OK για χρήση σε μια εφαρμογή, δεν είναι πολύ ευανάγνωστη. Το `moviedb.txt` από το διακομιστή μπορούν να χρησιμοποιηθούν για να επιλύσετε το `movieId` σε ταινία όνομα, αλλά πρέπει να πρώτα να κάνετε λήψη του και το αρχείο χαρακτηρισμών από το διακομιστή, χρησιμοποιώντας την ακόλουθη δέσμη ενεργειών:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

Αφού έχετε κάνει λήψη των αρχείων, χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών του PowerShell για να εμφανίσετε συστάσεις με ονόματα ταινίας:

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

Ακολουθεί ένα παράδειγμα για την εκτέλεση της δέσμης ενεργειών:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

Το αποτέλεσμα θα πρέπει να είναι παρόμοιο με τα εξής:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Ταξινόμηση δεδομένων με χρήση της γραμμής εντολών Hadoop

Μία από τις μεθόδους ταξινόμησης είναι διαθέσιμη με Mahout είναι να δημιουργήσετε μια [τυχαία δάσος][forest]. Αυτή είναι μια πολλών διεργασία βήμα που περιλαμβάνει τη χρήση δεδομένων εκπαίδευση για τη δημιουργία απόφασης δέντρα, που χρησιμοποιούνται στη συνέχεια, για να ταξινομήσετε τα δεδομένα. Αυτό χρησιμοποιεί την κλάση __org.apache.mahout.classifier.df.tools.Describe__ που παρέχεται από Mahout. Προς το παρόν πρέπει να εκτελεστεί χρησιμοποιώντας τη γραμμή εντολών Hadoop.

###<a name="load-the-data"></a>Η φόρτωση των δεδομένων

1. Κάντε λήψη τα ακόλουθα αρχεία από [το σύνολο δεδομένων NSL KDD](http://nsl.cs.unb.ca/NSL-KDD/).

  * [KDDTrain +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): το αρχείο εκπαίδευσης

  * [KDDTest +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): τα δεδομένα δοκιμής

2. Ανοίξετε κάθε αρχείο και να καταργήσετε τις γραμμές στο επάνω μέρος που ξεκινούν με '@', και, στη συνέχεια, αποθηκεύστε τα αρχεία. Εάν δεν καταργούνται αυτές, θα λαμβάνετε μηνύματα σφάλματος κατά τη χρήση αυτών των δεδομένων με Mahout.

2. Στείλτε τα αρχεία για __να/δεδομένα του παραδείγματος__. Μπορείτε να το κάνετε αυτό, χρησιμοποιώντας την ακόλουθη δέσμη ενεργειών. Αντικατάσταση __CLUSTERNAME__ με το όνομα του συμπλέγματος HDInsight. Αντικαταστήστε το όνομα ΑΡΧΕΊΟΥ με το όνομα του αρχείου της να αποσταλεί.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###<a name="run-the-job"></a>Εκτέλεση της εργασίας

1. Αυτή η εργασία απαιτεί τη γραμμή εντολών Hadoop. Ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας για το σύμπλεγμα HDInsight και, στη συνέχεια, συνδεθείτε, ακολουθώντας τις οδηγίες στην ενότητα [σύνδεση με χρήση RDP συμπλεγμάτων HDInsight](hdinsight-administer-use-management-portal.md#rdp).

3. Μετά τη σύνδεση, χρησιμοποιήστε το εικονίδιο __Hadoop γραμμής εντολών__ για να ανοίξετε τη γραμμή εντολών Hadoop:

    ![hadoop cli][hadoopcli]

3. Χρησιμοποιήστε την παρακάτω εντολή για τη δημιουργία της περιγραφής αρχείου (__KDDTrain + .info__), που χρησιμοποιεί Mahout.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    Το `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` περιγράφει τα χαρακτηριστικά των δεδομένων στο αρχείο. Για παράδειγμα, L υποδεικνύει μια ετικέτα.

4. Δημιουργήστε ένα σύμπλεγμα δέντρα αποφάσεων, χρησιμοποιώντας την ακόλουθη εντολή:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    Το αποτέλεσμα αυτής της λειτουργίας είναι αποθηκευμένο στον κατάλογο __nsl δάσος__ , που βρίσκεται στο χώρο αποθήκευσης της για το σύμπλεγμα HDInsight στο __ wasbs://user/&lt;username > / nsl-δάσος/nsl-forest.seq. Το &lt;όνομα χρήστη > είναι το όνομα χρήστη που χρησιμοποιήσατε για την περίοδο λειτουργίας απομακρυσμένης επιφάνειας εργασίας σας. Αυτό το αρχείο δεν είναι αναγνώσιμη από τους ανθρώπους.

5. Δοκιμάστε το δάσος με την ταξινόμηση των του συνόλου δεδομένων __KDDTest + .arff__ . Χρησιμοποιήστε την ακόλουθη εντολή:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    Αυτή η εντολή επιστρέφει συνοπτικές πληροφορίες σχετικά με τη διαδικασία ταξινόμησης παρόμοιο με το εξής:

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  Αυτή η εργασία δημιουργεί επίσης ένα αρχείο που βρίσκεται στο __wasbs:///example/data/predictions/KDDTest+.arff.out__. Ωστόσο, αυτό το αρχείο δεν είναι δυνατή σε ανθρώπους.

> [AZURE.NOTE] Εργασίες mahout χωρίς αντικατάσταση των αρχείων. Εάν θέλετε να εκτελέσετε ξανά αυτές τις εργασίες, πρέπει να διαγράψετε τα αρχεία που έχουν δημιουργηθεί με προηγούμενες εργασίες.

##<a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

###<a name="install"></a>Εγκατάσταση Mahout

Mahout είναι εγκατεστημένη στον HDInsight 3.1 συμπλεγμάτων και αυτό μπορεί να εγκατασταθεί με μη αυτόματο τρόπο σε HDInsight 3.0 ή HDInsight 2.1 συμπλεγμάτων, χρησιμοποιώντας τα ακόλουθα βήματα:

1. Η έκδοση του Mahout για να χρησιμοποιήσετε εξαρτάται από την έκδοση HDInsight του συμπλέγματος. Μπορείτε να βρείτε την έκδοση σύμπλεγμα προβάλλοντας τις ιδιότητες για το σύμπλεγμα στην πύλη του Azure.

  * __Για 2.1 HDInsight__, μπορείτε να κάνετε λήψη ενός αρχείου αρχειοθέτησης Java (ΒΆΖΟ) που περιέχει [Mahout 0.9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar).

  * __Για 3.0 HDInsight__, πρέπει να [δημιουργήσετε Mahout από την προέλευση] [ build] και να καθορίσετε την έκδοση Hadoop που παρέχεται από το HDInsight. Εγκαταστήστε τις προϋποθέσεις που αναφέρονται στη σελίδα Δημιουργία, κάντε λήψη της προέλευσης και, στη συνέχεια, χρησιμοποιήστε την ακόλουθη εντολή για να δημιουργήσετε τα αρχεία βάζο Mahout:

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Όταν κυκλοφορήσει Mahout 1.0, θα πρέπει να μπορείτε να χρησιμοποιήσετε τα προ-δημιουργημένα πακέτα με HDInsight 3.0.

2. Αποστείλετε το αρχείο βάζο στο __παράδειγμα/πλατύστομα__ στο προεπιλεγμένο αποθήκευσης για το σύμπλεγμά σας. Αντικαταστήστε CLUSTERNAME στο την ακόλουθη δέσμη ενεργειών με το όνομα του συμπλέγματος HDInsight, και όνομα ΑΡΧΕΊΟΥ με τη διαδρομή προς το αρχείο __mahout-coure-0.9-job.jar__ ...

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>Δεν είναι δυνατό να αντικαταστήσετε αρχεία

Κάντε εργασίες mahout δεν εκκαθάριση προσωρινά αρχεία που έχουν δημιουργηθεί στη διάρκεια της επεξεργασίας. Επιπλέον, οι εργασίες δεν θα αντικαταστήσει ένα υπάρχον αρχείο εξόδου.

Για να αποφύγετε σφάλματα κατά την εκτέλεση εργασιών Mahout, διαγράψτε τα αρχεία προσωρινό και εξόδου μεταξύ εκτελείται ή χρησιμοποιήστε ονόματα μοναδικό προσωρινό και εξόδου καταλόγου.

###<a name="cannot-find-the-jar-file"></a>Δεν μπορείτε να βρείτε το αρχείο ΒΆΖΟ

HDInsight 3.1 συμπλεγμάτων περιλαμβάνουν Mahout. Η διαδρομή και το όνομα αρχείου περιλαμβάνουν τον αριθμό έκδοσης του Mahout που είναι εγκατεστημένη στο σύμπλεγμα. Το παράδειγμα δέσμης ενεργειών του Windows PowerShell σε αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί μια διαδρομή που είναι έγκυρες από Νοεμβρίου 2015, αλλά ο αριθμός έκδοσης θα αλλάξει στο μέλλον ενημερώσεις με το HDInsight. Για να προσδιορίσετε την τρέχουσα διαδρομή προς το αρχείο ΒΆΖΩΝ Mahout για το σύμπλεγμά σας, χρησιμοποιήστε την ακόλουθη εντολή του Windows PowerShell και, στη συνέχεια, να τροποποιήσετε τη δέσμη ενεργειών για τη διαδρομή του αρχείου που επιστρέφεται αναφορά:

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>Κατηγορίες που δεν λειτουργούν με το Windows PowerShell

Εργασίες mahout που χρησιμοποιούν τις ακόλουθες κλάσεις επιστρέφουν μια ποικιλία μηνύματα σφάλματος που χρησιμοποιούνται από το Windows PowerShell:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Για να εκτελέσετε εργασίες που χρησιμοποιούν αυτές τις κλάσεις, συνδεθείτε με το σύμπλεγμα HDInsight και εκτελέστε τις εργασίες, χρησιμοποιώντας τη γραμμή εντολών Hadoop. Για παράδειγμα, ανατρέξτε στο θέμα [Ταξινόμηση δεδομένων με χρήση της γραμμής εντολών Hadoop](#classify) .

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε πώς μπορείτε να χρησιμοποιήσετε Mahout, Ανακαλύψτε άλλοι τρόποι για εργασία με δεδομένα σε HDInsight:

* [Hive με HDInsight](hdinsight-use-hive.md)
* [Γουρούνι με HDInsight](hdinsight-use-pig.md)
* [MapReduce με HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
