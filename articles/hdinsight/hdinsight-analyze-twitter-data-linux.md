<properties
    pageTitle="Ανάλυση δεδομένων Twitter με Apache Hive στην HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Python για την αποθήκευση Tweets που περιέχουν συγκεκριμένες λέξεις-κλειδιά και, στη συνέχεια, χρησιμοποιήστε Hive και Hadoop σε HDInsight για να μετατρέψετε τα ανεπεξέργαστα δεδομένα TWitter σε έναν πίνακα με δυνατότητα αναζήτησης ομάδας."
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
    ms.date="09/27/2016"
    ms.author="larryfr"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Ανάλυση των δεδομένων Twitter με ομάδα στο HDInsight

Σε αυτό το έγγραφο, θα λάβετε tweets χρησιμοποιώντας μια ροή API Twitter και, στη συνέχεια, χρήση Apache Hive σε ένα σύμπλεγμα βάσει Linux HDInsight στη διαδικασία JSON τα δεδομένα που έχουν μορφοποιηθεί. Το αποτέλεσμα θα είναι μια λίστα των χρηστών Twitter που σας έστειλε την περισσότερες tweets που περιέχονται σε μια συγκεκριμένη λέξη.

> [AZURE.NOTE] Ενώ μεμονωμένα τμήματα αυτού του εγγράφου μπορεί να χρησιμοποιηθεί με το HDInsight που βασίζεται σε Windows συμπλεγμάτων (Python για παράδειγμα), πολλά βήματα βάση χρησιμοποιώντας ένα σύμπλεγμα βάσει Linux HDInsight. Για συγκεκριμένα βήματα σε ένα σύμπλεγμα που βασίζεται στα Windows, ανατρέξτε στο θέμα [ανάλυση του Twitter δεδομένων με χρήση της ομάδας στο HDInsight](hdinsight-analyze-twitter-data.md).

###<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- Ένα __σύμπλεγμα βάσει Linux Azure HDInsight__. Για πληροφορίες σχετικά με τη δημιουργία ένα σύμπλεγμα, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το HDInsight βάσει Linux](hdinsight-hadoop-linux-tutorial-get-started.md) για οδηγίες σχετικά με τη δημιουργία ενός συμπλέγματος.

- Ένα __πρόγραμμα-πελάτη SSH__. Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με βάσει Linux HDInsight, ανατρέξτε στα ακόλουθα άρθρα:

    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, Unix ή λειτουργικό σύστημα OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

- __Python__ και [pip](https://pypi.python.org/pypi/pip)

##<a name="get-a-twitter-feed"></a>Λάβετε ένα Twitter τροφοδοσίας

Twitter σάς επιτρέπει να ανακτήσετε τα [δεδομένα για κάθε tweet](https://dev.twitter.com/docs/platform-objects/tweets) ως έγγραφο σημειογραφίας αντικειμένων JavaScript (JSON) μέσω μιας REST API. [Διακριτικό](http://oauth.net) είναι απαραίτητη για το API του ελέγχου ταυτότητας. Μπορείτε, επίσης, πρέπει να δημιουργήσετε μια _Εφαρμογή του Twitter_ που περιέχει τις ρυθμίσεις που χρησιμοποιούνται για να αποκτήσετε πρόσβαση το API.

###<a name="create-a-twitter-application"></a>Δημιουργία μιας εφαρμογής Twitter

1. Από ένα πρόγραμμα περιήγησης web, πραγματοποιήστε είσοδο στο [https://apps.twitter.com/](https://apps.twitter.com/). Εάν δεν έχετε ένα λογαριασμό του Twitter, κάντε κλικ στη σύνδεση **εγγραφή τώρα** .
2. Κάντε κλικ στην επιλογή **Δημιουργία νέας εφαρμογής**.
3. Πληκτρολογήστε **το όνομα**, **Περιγραφή**, **τοποθεσία Web**. Μπορείτε να ορίσετε μια διεύθυνση URL για το πεδίο **τοποθεσία Web** . Ο παρακάτω πίνακας εμφανίζει ορισμένα δείγματα τιμών για να χρησιμοποιήσετε:

  	| Πεδίο | Τιμή |
  	|:----- |:----- |
  	| Όνομα  | MyHDInsightApp |
  	| Περιγραφή | MyHDInsightApp |
  	| Τοποθεσία Web | http://www.myhdinsightapp.com |
    
4. Επιλέξτε **Ναι, συμφωνείτε**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία την εφαρμογή του Twitter**.
5. Κάντε κλικ στην καρτέλα " **δικαιώματα** ". Τα προεπιλεγμένα δικαιώματα είναι **μόνο για ανάγνωση**. Αυτό είναι αρκετό για αυτό το πρόγραμμα εκμάθησης.
6. Κάντε κλικ στην καρτέλα **πλήκτρα και οι κωδικοί πρόσβασης** .
7. Κάντε κλικ στην επιλογή **Δημιουργία μου διακριτικό πρόσβασης**.
8. Κάντε κλικ στην επιλογή **Δοκιμή διακριτικό** στην επάνω δεξιά γωνία της σελίδας.
9. Καταγράψτε **κλειδί καταναλωτή**, **μυστικό καταναλωτή**, **διακριτικό πρόσβασης**και **μυστικό διακριτικό πρόσβασης**. Θα χρειαστείτε αργότερα τις τιμές.

>[AZURE.NOTE] Όταν χρησιμοποιείτε την εντολή καμπύλη στα Windows, χρησιμοποιήστε διπλά εισαγωγικά αντί για μονά εισαγωγικά για τις τιμές επιλογής.

###<a name="download-tweets"></a>Λήψη tweets

Ο ακόλουθος κώδικας Python θα λήψη 10.000 tweets από Twitter και να τα αποθηκεύσετε σε ένα αρχείο με το όνομα __tweets.txt__.

> [AZURE.NOTE] Ακολουθήστε τα παρακάτω βήματα εκτελούνται στο σύμπλεγμα HDInsight, επειδή το Python είναι ήδη εγκατεστημένο.

1. Συνδεθείτε με το σύμπλεγμα HDInsight χρησιμοποιώντας SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Εάν χρησιμοποιούσατε έναν κωδικό πρόσβασης για την ασφάλιση λογαριασμού χρήστη SSH, θα σας ζητηθεί για να το εισαγάγετε. Εάν χρησιμοποιείτε ένα δημόσιο κλειδί, ίσως χρειαστεί να χρησιμοποιήσετε το `-i` παραμέτρου για να καθορίσετε το αντίστοιχο ιδιωτικό κλειδί. Για παράδειγμα, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με βάσει Linux HDInsight, ανατρέξτε στα ακόλουθα άρθρα:
    
    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, Unix ή λειτουργικό σύστημα OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows)
    
2. Από προεπιλογή, βοηθητικού προγράμματος __pip__ δεν είναι εγκατεστημένη στον κόμβο κεφαλής HDInsight. Χρησιμοποιήστε τα ακόλουθα για να εγκαταστήσετε και, στη συνέχεια, ενημερώστε αυτό το βοηθητικό πρόγραμμα:

        sudo apt-get install python-pip
        sudo pip install --upgrade pip

3. Ο κωδικός για να κάνετε λήψη tweets βασίζεται σε [Tweepy](http://www.tweepy.org/) και [Progressbar](https://pypi.python.org/pypi/progressbar/2.2). Για να εγκαταστήσετε αυτές τις, χρησιμοποιήστε την ακόλουθη εντολή:

        sudo apt-get install python-dev libffi-dev libssl-dev
        sudo apt-get remove python-openssl
        sudo pip install tweepy progressbar pyOpenSSL requests[security]
        
    > [AZURE.NOTE] Είναι τα bit σχετικά με την κατάργηση python-openssl, την εγκατάσταση του python αποκλίσεις, libffi αποκλίσεις, την libssl, pyOpenSSL και αιτήσεις [ασφάλεια] για να αποφύγετε μια προειδοποίηση InsecurePlatform κατά τη σύνδεση στο Twitter μέσω SSL από Python.
    >
    > Tweepy v3.2.0 χρησιμοποιείται για να αποφύγετε [ένα μήνυμα σφάλματος](https://github.com/tweepy/tweepy/issues/576) που μπορεί να προκύψουν κατά την επεξεργασία tweets.

4. Χρησιμοποιήστε την παρακάτω εντολή για να δημιουργήσετε ένα νέο αρχείο με το όνομα __gettweets.py__:

        nano gettweets.py

5. Χρησιμοποιήστε τα ακόλουθα με τα περιεχόμενα του αρχείου __gettweets.py__ . Αντικατάσταση των στοιχείων κράτησης θέσης για το __καταναλωτή\_μυστικό__, __καταναλωτή\_πλήκτρο__, __πρόσβασης /\_διακριτικού__, και __access\_διακριτικού\_μυστικό__ με τις πληροφορίες από την εφαρμογή του Twitter.

        #!/usr/bin/python

        from tweepy import Stream, OAuthHandler
        from tweepy.streaming import StreamListener
        from progressbar import ProgressBar, Percentage, Bar
        import json
        import sys

        #Twitter app information
        consumer_secret='Your consumer secret'
        consumer_key='Your consumer key'
        access_token='Your access token'
        access_token_secret='Your access token secret'

        #The number of tweets we want to get
        max_tweets=10000

        #Create the listener class that will receive and save tweets
        class listener(StreamListener):
            #On init, set the counter to zero and create a progress bar
            def __init__(self, api=None):
                self.num_tweets = 0
                self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

            #When data is received, do this
            def on_data(self, data):
                #Append the tweet to the 'tweets.txt' file
                with open('tweets.txt', 'a') as tweet_file:
                    tweet_file.write(data)
                    #Increment the number of tweets
                    self.num_tweets += 1
                    #Check to see if we have hit max_tweets and exit if so
                    if self.num_tweets >= max_tweets:
                        self.pbar.finish()
                        sys.exit(0)
                    else:
                        #increment the progress bar
                        self.pbar.update(self.num_tweets)
                return True

            #Handle any errors that may occur
            def on_error(self, status):
                print status

        #Get the OAuth token
        auth = OAuthHandler(consumer_key, consumer_secret)
        auth.set_access_token(access_token, access_token_secret)
        #Use the listener class for stream processing
        twitterStream = Stream(auth, listener())
        #Filter for these topics
        twitterStream.filter(track=["azure","cloud","hdinsight"])

6. Χρησιμοποιήστε το __συνδυασμό πλήκτρων Ctrl + X__και κατόπιν __Y__ για να αποθηκεύσετε το αρχείο.

7. Χρησιμοποιήστε την παρακάτω εντολή για να εκτελέσετε το αρχείο και να κάνετε λήψη tweets:

        python gettweets.py

    Ένδειξη προόδου θα πρέπει να εμφανίζονται και μέτρηση έως 100%, όπως το tweets λήψης και αποθηκεύσατε το αρχείο.

    > [AZURE.NOTE] Εάν διαρκεί πολύ χρόνο για τη γραμμή προόδου για να προχωρήσετε, θα πρέπει να αλλάξετε το φίλτρο για να παρακολουθείτε τάσης θέματα. Όταν υπάρχουν πολλά tweets σχετικά με το θέμα που το φιλτράρισμα, μπορείτε να λάβετε πολύ γρήγορα το 10000 tweets είναι απαραίτητο.

###<a name="upload-the-data"></a>Κάντε αποστολή των δεδομένων

Για να αποστείλετε τα δεδομένα στο WASB (το κατανεμημένο σύστημα αρχείων χρησιμοποιείται από το HDInsight), χρησιμοποιήστε τις παρακάτω εντολές:

    hdfs dfs -mkdir -p /tutorials/twitter/data
    hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt

Αυτό αποθηκεύει τα δεδομένα σε μια θέση όπου έχουν πρόσβαση όλοι οι κόμβοι του συμπλέγματος.

##<a name="run-the-hiveql-job"></a>Εκτέλεση της εργασίας HiveQL


1. Χρησιμοποιήστε την ακόλουθη εντολή για να δημιουργήσετε ένα αρχείο που περιέχει HiveQL προτάσεις:

        nano twitter.hql
    
    Χρησιμοποιήστε τα παρακάτω ανάλογα με τα περιεχόμενα του αρχείου:

        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        -- Drop table, if it exists
        DROP TABLE tweets_raw;
        -- Create it, pointing toward the tweets logged from Twitter
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
        -- Drop and recreate the destination table
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
        -- Select tweets from the imported data, parse the JSON,
        -- and insert into the tweets table
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
        
        
3. Πατήστε το __συνδυασμό πλήκτρων Ctrl + X__και, στη συνέχεια, πατήστε το πλήκτρο __Y__ για να αποθηκεύσετε το αρχείο.

4. Χρησιμοποιήστε την ακόλουθη εντολή για να εκτελέσετε το HiveQL που περιέχονται στο αρχείο:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i twitter.hql
        
    Αυτό θα φορτώσετε το κέλυφος Hive, εκτελέστε το HiveQL στο αρχείο __twitter.hql__ και τέλος να επιστρέψει μια `jdbc:hive2//localhost:10001/>` ερώτηση.
    
5. Από τη γραμμή εντολών beeline, χρησιμοποιήστε τα ακόλουθα για να επιβεβαιώσετε ότι μπορείτε να επιλέξετε δεδομένα από τον πίνακα __tweets__ που δημιουργήθηκε από το HiveQL στο αρχείο __twitter.hql__ :
        
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;

    Αυτό θα επιστρέψει έως 10 tweets που περιέχουν τη λέξη __Azure__ στο κείμενο του μηνύματος.

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης μας είδατε πώς μπορείτε να μετατρέψετε ένα μη δομημένα συνόλου δεδομένων JSON σε δομημένες Hive πίνακα σε ερώτημα, Εξερεύνηση και ανάλυση των δεδομένων από το Twitter χρησιμοποιώντας Azure HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Γρήγορα αποτελέσματα με το HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Ανάλυση των δεδομένων καθυστέρηση πτήσεων χρησιμοποιώντας HDInsight](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
