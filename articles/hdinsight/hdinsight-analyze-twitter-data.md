<properties
    pageTitle="Ανάλυση δεδομένων Twitter με Hadoop στο HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την ομάδα για την ανάλυση δεδομένων Twitter στο Hadoop στο HDInsight για να βρείτε τη συχνότητα χρήσης μιας συγκεκριμένης λέξης."
    services="hdinsight"
    documentationCenter=""
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

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Ανάλυση των δεδομένων Twitter με ομάδα στο HDInsight

Τοποθεσίες Web κοινωνικής είναι μία από τις κύριες οδήγησης δυνάμεις για υιοθέτησης μεγάλο δεδομένων. Δημόσιων API που παρέχεται από τοποθεσίες όπως Twitter είναι χρήσιμη προέλευση δεδομένων για την ανάλυση και την κατανόηση δημοφιλείς τάσεις. Σε αυτό το πρόγραμμα εκμάθησης, θα λάβετε tweets χρησιμοποιώντας μια ροή API Twitter και, στη συνέχεια, χρησιμοποιήστε Apache Hive σε Azure HDInsight για να λάβετε μια λίστα με τους χρήστες Twitter που σας έστειλε την περισσότερες tweets που περιέχονται σε μια συγκεκριμένη λέξη.

> [AZURE.NOTE] Τα βήματα σε αυτό το έγγραφο απαιτεί ένα σύμπλεγμα HDInsight που βασίζεται στα Windows. Για συγκεκριμένα βήματα σε ένα σύμπλεγμα βάσει Linux, ανατρέξτε στο θέμα [ανάλυση του Twitter δεδομένων με χρήση της ομάδας στο HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).


##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Μια σταθμούς εργασίας** με το Azure PowerShell εγκατασταθεί και ρυθμιστεί. 

    Για να εκτελέσετε τις δέσμες ενεργειών του Windows PowerShell, πρέπει να εκτελέσετε Azure PowerShell ως διαχειριστής και ορίστε την πολιτική εκτέλεσης σε *RemoteSigned*. Ανατρέξτε στο θέμα [Εκτέλεση του Windows PowerShell δέσμες ενεργειών][powershell-script].

    Πριν από την εκτέλεση δεσμών ενεργειών του Windows PowerShell, βεβαιωθείτε ότι είστε συνδεδεμένοι με τη συνδρομή σας στο Azure χρησιμοποιώντας το ακόλουθο cmdlet:

        Login-AzureRmAccount

    Εάν έχετε πολλές συνδρομές Azure, χρησιμοποιήστε το ακόλουθο cmdlet για να ορίσετε την τρέχουσα συνδρομή σας:

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Σύμπλεγμα του Azure HDInsight**. Για οδηγίες σχετικά με την προμήθεια του συμπλέγματος, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το HDInsight] [ hdinsight-get-started] ή [HDInsight παροχή συμπλεγμάτων] [hdinsight-provision]. Θα χρειαστεί το όνομα του συμπλέγματος αργότερα στην εκμάθηση.

Ο παρακάτω πίνακας παραθέτει τα αρχεία που χρησιμοποιούνται σε αυτό το πρόγραμμα εκμάθησης:

Αρχεία|Περιγραφή
---|---
/tutorials/Twitter/Data/tweets.txt|Τα δεδομένα προέλευσης για την ομάδα εργασίας.
/tutorials/Twitter/Output|Ο φάκελος εξόδου για το έργο της ομάδας. Το όνομα αρχείου προεπιλεγμένη ομάδα εργασίας εξόδου είναι **000000_0**.
tutorials/Twitter/Twitter.hql|Το αρχείο δέσμης ενεργειών HiveQL.
/tutorials/Twitter/JobStatus|Την κατάσταση της εργασίας Hadoop.


##<a name="get-twitter-feed"></a>Λήψη Twitter τροφοδοσίας

Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσετε το [Twitter ροής APIs][twitter-streaming-api]. Το συγκεκριμένο Twitter ροής API που θα χρησιμοποιήσετε είναι [καταστάσεις/φίλτρο][twitter-statuses-filter].

>[AZURE.NOTE] Ένα αρχείο που περιέχει 10.000 tweets και το αρχείο δέσμης ενεργειών ομάδας (καλύπτεται στην επόμενη ενότητα) έχουν αποσταλεί σε μια δημόσια κοντέινερ αντικειμένων Blob. Εάν θέλετε να χρησιμοποιήσετε τα αρχεία που έχουν αποσταλεί, μπορείτε να παραλείψετε αυτή την ενότητα.

Tweets τα [δεδομένα](https://dev.twitter.com/docs/platform-objects/tweets) αποθηκεύονται στη μορφή σημειογραφίας αντικειμένων JavaScript (JSON) που περιέχει μια ένθετη πολύπλοκη δομή. Αντί να γράφετε πολλές γραμμές κώδικα, χρησιμοποιώντας μια συμβατικός γλώσσα προγραμματισμού, μπορείτε να μετατρέψετε αυτή τη δομή ένθετων σε έναν πίνακα ομάδα, ώστε το μπορούν να αναζητηθούν με μια Structured Query Language (SQL)-όπως γλώσσας που ονομάζεται HiveQL.

Twitter χρησιμοποιεί διακριτικό για την παροχή τους χρήστες με δικαίωμα πρόσβασης για το API. Διακριτικό είναι ένα πρωτόκολλο ελέγχου ταυτότητας που επιτρέπει στους χρήστες να εγκρίνει τις εφαρμογές για εκτέλεση ενεργειών για λογαριασμό τους χωρίς κοινή χρήση τον κωδικό πρόσβασής τους. Περισσότερες πληροφορίες μπορείτε να βρείτε την [oauth.net](http://oauth.net/) ή με την εξαιρετική [Οδηγός αρχαρίων για OAuth](http://hueniverse.com/oauth/) από Hueniverse.

Το πρώτο βήμα για να χρησιμοποιήσετε διακριτικό είναι να δημιουργήσετε μια νέα εφαρμογή στην τοποθεσία του Twitter προγραμματιστής.

**Για να δημιουργήσετε μια εφαρμογή Twitter**

1. Πραγματοποιήστε είσοδο στο [https://apps.twitter.com/](https://apps.twitter.com/). Εάν δεν έχετε ένα λογαριασμό του Twitter, κάντε κλικ στη σύνδεση **εγγραφή τώρα** .
2. Κάντε κλικ στην επιλογή **Δημιουργία νέας εφαρμογής**.
3. Πληκτρολογήστε **το όνομα**, **Περιγραφή**, **τοποθεσία Web**. Μπορείτε να ορίσετε μια διεύθυνση URL για το πεδίο **τοποθεσία Web** . Ο παρακάτω πίνακας εμφανίζει ορισμένα δείγματα τιμών για να χρησιμοποιήσετε:

    Πεδίο|Τιμή
    ---|---
    Όνομα|MyHDInsightApp
    Περιγραφή|MyHDInsightApp
    Τοποθεσία Web|http://www.myhdinsightapp.com

4. Επιλέξτε **Ναι, συμφωνείτε**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία την εφαρμογή του Twitter**.
5. Κάντε κλικ στην καρτέλα " **δικαιώματα** ". Τα προεπιλεγμένα δικαιώματα είναι **μόνο για ανάγνωση**. Αυτό είναι αρκετό για αυτό το πρόγραμμα εκμάθησης.
6. Κάντε κλικ στην καρτέλα **πλήκτρα και οι κωδικοί πρόσβασης** .
7. Κάντε κλικ στην επιλογή **Δημιουργία μου διακριτικό πρόσβασης**.
8. Κάντε κλικ στην επιλογή **Δοκιμή διακριτικό** στην επάνω δεξιά γωνία της σελίδας.
9. Καταγράψτε **κλειδί καταναλωτή**, **μυστικό καταναλωτή**, **διακριτικό πρόσβασης**και **μυστικό διακριτικό πρόσβασης**. Θα χρειαστείτε τις τιμές αργότερα στην εκμάθηση.

Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσετε του Windows PowerShell για να κάνετε την υπηρεσία web κλήσεων. Για ένα .NET C# δείγμα, ανατρέξτε στο θέμα [ανάλυση σε πραγματικό χρόνο Twitter άποψη με HBase σε HDInsight][hdinsight-hbase-twitter-sentiment]. Το δημοφιλείς εργαλείο για την πραγματοποίηση κλήσεων υπηρεσίας web είναι [*Curl*][curl]. Καμπύλη μπορούν να ληφθούν από [εδώ][curl-download].

>[AZURE.NOTE] Όταν χρησιμοποιείτε την εντολή καμπύλη στα Windows, χρησιμοποιήστε διπλά εισαγωγικά αντί για μονά εισαγωγικά για τις τιμές επιλογής.

**Για να λάβετε tweets**

1. Ανοίξτε το Windows PowerShell ενσωματωμένη περιβάλλον δέσμης ενεργειών (ISE). (Στην οθόνη έναρξης των Windows 8, πληκτρολογήστε **PowerShell_ISE** και, στη συνέχεια, κάντε κλικ στην επιλογή **Windows PowerShell ISE**. Ανατρέξτε στο θέμα [Έναρξη του Windows PowerShell στα Windows 8 και Windows][powershell-start].)

2. Αντιγράψτε την ακόλουθη δέσμη ενεργειών του παραθύρου δέσμης ενεργειών:

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. Ορίστε τα πρώτα πέντε έως οκτώ μεταβλητές στη δέσμη ενεργειών:


    Μεταβλητή|Περιγραφή
    ---|---
    $clusterName|Αυτό είναι το όνομα του συμπλέγματος HDInsight, όπου θέλετε να εκτελέσετε την εφαρμογή.
    $oauth_consumer_key|Αυτό είναι το εφαρμογή Twitter **καταναλωτή αριθμό-κλειδί** που καταγράψατε νωρίτερα όταν δημιουργήσατε την εφαρμογή Twitter.
    $oauth_consumer_secret|Αυτή είναι η εφαρμογή Twitter **μυστικό καταναλωτή** καταγράψατε νωρίτερα.
    $oauth_token|Αυτή είναι η εφαρμογή του Twitter **διακριτικό πρόσβασης** καταγράψατε νωρίτερα.
    $oauth_token_secret|Αυτό είναι το Twitter εφαρμογής **access διακριτικού μυστικό** καταγράψατε νωρίτερα.
    $destBlobName|Αυτό είναι το όνομα blob εξόδου. Η προεπιλεγμένη τιμή είναι **tutorials/twitter/data/tweets.txt**. Εάν αλλάξετε την προεπιλεγμένη τιμή, θα πρέπει να ενημερώσετε τις δέσμες ενεργειών του Windows PowerShell, αντίστοιχα.
    $trackString|Η υπηρεσία web θα επιστρέψει tweets που σχετίζονται με αυτές τις λέξεις-κλειδιά. Η προεπιλεγμένη τιμή είναι **Azure, Cloud, HDInsight**. Εάν αλλάξετε την προεπιλεγμένη τιμή, θα ενημερώσει τις δέσμες ενεργειών του Windows PowerShell αντίστοιχα.
    $lineMax|Η τιμή καθορίζει πόσες tweets θα διαβάσει τη δέσμη ενεργειών. Χρειάζονται περίπου τρία λεπτά για να διαβάσετε 100 tweets. Μπορείτε να ορίσετε έναν μεγαλύτερο αριθμό, αλλά θα χρειαστεί περισσότερο χρόνο για να κάνετε λήψη.

5. Πατήστε το πλήκτρο **F5** για να εκτελέσετε τη δέσμη ενεργειών. Εάν αντιμετωπίσετε προβλήματα, ως λύση, επιλέξτε όλες τις γραμμές και, στη συνέχεια, πιέστε το πλήκτρο **F8**.
6. Βλέπετε "Ολοκλήρωση!" στο τέλος του αποτελέσματος. Μηνύματα σφάλματος θα εμφανίζονται με κόκκινο χρώμα.

Ως μια διαδικασία επικύρωσης, μπορείτε να ελέγξετε το αρχείο εξόδου, **/tutorials/twitter/data/tweets.txt**, στο χώρο αποθήκευσης αντικειμένων Blob Azure σας με χρήση της Εξερεύνησης χώρου αποθήκευσης Azure ή Azure PowerShell. Για ένα δείγμα του Windows PowerShell δέσμης ενεργειών για την καταχώρηση των αρχείων, ανατρέξτε στο θέμα [χώρος αποθήκευσης αντικειμένων Blob χρήση με το HDInsight][hdinsight-storage-powershell].



##<a name="create-hiveql-script"></a>Δημιουργία δέσμης ενεργειών HiveQL

Χρήση του Azure PowerShell, μπορείτε να εκτελέσετε πολλαπλές δηλώσεις HiveQL μία κάθε φορά ή να συσκευάσετε τη δήλωση HiveQL σε ένα αρχείο δέσμης ενεργειών. Σε αυτό το πρόγραμμα εκμάθησης, θα δημιουργήσετε μια δέσμη ενεργειών HiveQL. Το αρχείο δέσμης ενεργειών πρέπει να αποσταλεί σε χώρο αποθήκευσης αντικειμένων Blob του Azure. Στην επόμενη ενότητα, θα μπορείτε να εκτελέσετε το αρχείο δέσμης ενεργειών με χρήση του Azure PowerShell.

>[AZURE.NOTE] Το αρχείο δέσμης ενεργειών ομάδας και ένα αρχείο που περιέχει 10.000 tweets έχουν αποσταλεί σε μια δημόσια κοντέινερ αντικειμένων Blob. Εάν θέλετε να χρησιμοποιήσετε τα αρχεία που έχουν αποσταλεί, μπορείτε να παραλείψετε αυτή την ενότητα.

Η δέσμη ενεργειών HiveQL θα εκτελέσει τις εξής ενέργειες:

1. **Απόθεση του πίνακα tweets_raw** σε περίπτωση στον πίνακα υπάρχει ήδη.
2. Για να **δημιουργήσετε τον πίνακα ομάδα tweets_raw**. Αυτή η ομάδα προσωρινό δομημένες πίνακα διατηρεί τα δεδομένα για περαιτέρω εξαγάγετε μετασχηματισμός και φόρτωση επεξεργασίας (ETL). Για πληροφορίες σχετικά με τα διαμερίσματα, ανατρέξτε στο θέμα [Hive πρόγραμμα εκμάθησης][apache-hive-tutorial].  
3. **Φόρτωση δεδομένων** από το φάκελο προέλευσης, /tutorials/twitter/data. Το σύνολο δεδομένων μεγάλου tweets σε μορφή ένθετη JSON τώρα έχει μετατραπεί σε ένα προσωρινό δομή πίνακα ομάδα.
3. **Απόθεση του πίνακα tweets** σε περίπτωση στον πίνακα υπάρχει ήδη.
4. **Δημιουργία πίνακα tweets**. Μπορείτε να υποβάλετε ερώτημα σε σχέση με το σύνολο δεδομένων tweets με χρήση της ομάδας, πρέπει να εκτελέσετε κάποια άλλη διαδικασία ETL. Αυτή η διαδικασία ETL καθορίζει μια πιο λεπτομερή διάταξη πίνακα για τα δεδομένα που έχετε αποθηκεύσει στον πίνακα "twitter_raw".  
5. **Εισαγωγή πίνακα αντικατάσταση**. Αυτήν τη δέσμη ενεργειών σύνθετες Hive θα ξεκινήσετε ένα σύνολο μεγάλη MapReduce εργασίες από το σύμπλεγμα Hadoop. Ανάλογα με το σύνολο δεδομένων σας και το μέγεθος του συμπλέγματος, αυτό μπορεί να διαρκέσει περίπου 10 λεπτά.
6. **Εισαγωγή αντικατάσταση καταλόγου**. Εκτέλεση ενός ερωτήματος και την έξοδο του συνόλου δεδομένων σε ένα αρχείο. Αυτό το ερώτημα θα επιστρέψει μια λίστα χρηστών Twitter που σας έστειλε περισσότερες tweets που περιλαμβάνουν τη λέξη "Azure".

**Για να δημιουργήσετε μια δέσμη ενεργειών Hive και στείλτε το στο Azure**

1. Ανοίξτε το Windows PowerShell ISE.
2. Αντιγράψτε την ακόλουθη δέσμη ενεργειών του παραθύρου δέσμης ενεργειών:

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. Ορίστε τις δύο πρώτες μεταβλητές στη δέσμη ενεργειών:

    Μεταβλητή|Περιγραφή
    ---|---
    $clusterName|Πληκτρολογήστε το όνομα του συμπλέγματος HDInsight όπου θέλετε να εκτελέσετε την εφαρμογή.
    $subscriptionID|Πληκτρολογήστε το αναγνωριστικό Azure συνδρομής.
    $sourceDataPath|Η θέση αποθήκευσης αντικειμένων Blob του Azure όπου τα ερωτήματα Hive θα διαβάσει τα δεδομένα από. Δεν χρειάζεται να αλλάξετε αυτήν τη μεταβλητή.
    $outputPath|Η θέση αποθήκευσης αντικειμένων Blob του Azure όπου τα ερωτήματα Hive θα εξόδου τα αποτελέσματα. Δεν χρειάζεται να αλλάξετε αυτήν τη μεταβλητή.
    $hqlScriptFile|Τη θέση και το όνομα του αρχείου από το αρχείο δέσμης ενεργειών HiveQL. Δεν χρειάζεται να αλλάξετε αυτήν τη μεταβλητή.

5. Πατήστε το πλήκτρο **F5** για να εκτελέσετε τη δέσμη ενεργειών. Εάν αντιμετωπίσετε προβλήματα, ως λύση, επιλέξτε όλες τις γραμμές και, στη συνέχεια, πιέστε το πλήκτρο **F8**.
6. Βλέπετε "Ολοκλήρωση!" στο τέλος του αποτελέσματος. Μηνύματα σφάλματος θα εμφανίζονται με κόκκινο χρώμα.

Ως μια διαδικασία επικύρωσης, μπορείτε να ελέγξετε το αρχείο εξόδου, **/tutorials/twitter/twitter.hql**, στο χώρο αποθήκευσης αντικειμένων Blob Azure σας με χρήση της Εξερεύνησης χώρου αποθήκευσης Azure ή Azure PowerShell. Για ένα δείγμα του Windows PowerShell δέσμης ενεργειών για την καταχώρηση των αρχείων, ανατρέξτε στο θέμα [χώρος αποθήκευσης αντικειμένων Blob χρήση με το HDInsight][hdinsight-storage-powershell].  


##<a name="process-twitter-data-by-using-hive"></a>Διαδικασία Twitter δεδομένων με χρήση της ομάδας

Έχετε ολοκληρώσει όλες τις εργασίες προετοιμασίας. Τώρα, μπορείτε να ενεργοποιήσετε τη δέσμη ενεργειών της ομάδας και να ελέγξετε τα αποτελέσματα.

### <a name="submit-a-hive-job"></a>Υποβάλετε μια εργασία ομάδας

Χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών του Windows PowerShell για να εκτελέσετε τη δέσμη ενεργειών της ομάδας. Θα πρέπει να ορίσετε την πρώτη μεταβλητή.

>[AZURE.NOTE] Για να χρησιμοποιήσετε το tweets και τη δέσμη ενεργειών HiveQL που έχετε αποστείλει σε τα τελευταία δύο ενότητες, ρυθμίστε $hqlScriptFile "/ tutorials/twitter/twitter.hql". Για να χρησιμοποιήσετε αυτές που έχουν αποσταλεί σε μια δημόσια blob για εσάς, ρυθμίστε $hqlScriptFile "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>Ελέγξτε τα αποτελέσματα

Χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών του Windows PowerShell για να ελέγξετε το αποτέλεσμα της ομάδας εργασίας. Θα πρέπει να ορίσετε τις δύο πρώτες μεταβλητές.

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] Ο πίνακας Hive χρησιμοποιεί \001 ως οριοθέτης πεδίου. Ο οριοθέτης που δεν είναι ορατή στο αποτέλεσμα.

Αφού έχουν τεθεί τα αποτελέσματα της ανάλυσης στο χώρο αποθήκευσης αντικειμένων Blob του Azure, να εξαγάγετε τα δεδομένα σε ένα διακομιστή βάσης δεδομένων/SQL Azure SQL, εξαγωγή δεδομένων στο Excel με χρήση του Power Query ή συνδέστε την εφαρμογή με τα δεδομένα, χρησιμοποιώντας το πρόγραμμα οδήγησης ODBC Hive. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση Sqoop με HDInsight][hdinsight-use-sqoop], [ανάλυση πτήσεων καθυστέρηση δεδομένων με χρήση HDInsight][hdinsight-analyze-flight-delay-data], [Σύνδεσης του Excel με το HDInsight με το Power Query][hdinsight-power-query], και [Σύνδεσης του Excel με το HDInsight με το Microsoft Hive πρόγραμμα οδήγησης ODBC][hdinsight-hive-odbc].

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης μας είδατε πώς μπορείτε να μετατρέψετε ένα μη δομημένα συνόλου δεδομένων JSON σε δομημένες Hive πίνακα σε ερώτημα, Εξερεύνηση και ανάλυση των δεδομένων από το Twitter χρησιμοποιώντας Azure HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Γρήγορα αποτελέσματα με το HDInsight][hdinsight-get-started]
- [Ανάλυση σε πραγματικό χρόνο Twitter άποψη με HBase σε HDInsight][hdinsight-hbase-twitter-sentiment]
- [Ανάλυση των δεδομένων καθυστέρηση πτήσεων χρησιμοποιώντας HDInsight][hdinsight-analyze-flight-delay-data]
- [Σύνδεση του Excel με το HDInsight με το Power Query][hdinsight-power-query]
- [Σύνδεση Excel με το HDInsight με το πρόγραμμα οδήγησης ODBC Microsoft Hive][hdinsight-hive-odbc]
- [Χρήση Sqoop με HDInsight][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
