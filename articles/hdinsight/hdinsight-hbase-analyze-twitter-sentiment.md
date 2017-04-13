<properties 
    pageTitle="Ανάλυση σε πραγματικό χρόνο Twitter άποψη με HBase | Microsoft Azure" 
    description="Μάθετε πώς να κάνετε ανάλυση σε πραγματικό χρόνο άποψη μεγάλο δεδομένων από το Twitter χρησιμοποιώντας HBase σε ένα σύμπλεγμα HDInsight (Hadoop)." 
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
    ms.date="09/09/2016" 
    ms.author="jgao"/>

# <a name="analyze-real-time-twitter-sentiment-with-hbase-in-hdinsight"></a>Ανάλυση σε πραγματικό χρόνο Twitter άποψη με HBase σε HDInsight

Μάθετε πώς μπορείτε να κάνετε σε πραγματικό χρόνο [ανάλυση άποψη](http://en.wikipedia.org/wiki/Sentiment_analysis) μεγάλο δεδομένων από το Twitter, χρησιμοποιώντας HBase σε ένα σύμπλεγμα HDInsight (Hadoop).


Τοποθεσίες Web κοινωνικής είναι μία από τις κύριες οδήγησης δυνάμεις για υιοθέτησης μεγάλο δεδομένων. Δημόσιων API που παρέχεται από τοποθεσίες όπως Twitter είναι χρήσιμη προέλευση δεδομένων για την ανάλυση και την κατανόηση δημοφιλείς τάσεις. Σε αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να αναπτύξετε μια κονσόλα ροής εφαρμογής υπηρεσίας και μια εφαρμογή web ASP.NET για να εκτελέσετε τις ακόλουθες ενέργειες:

![Άποψη HDInsight HBase ανάλυση Twitter][img-app-arch]

- Η εφαρμογή ροής
    - λήψη ετικέτα παν tweets σε πραγματικό χρόνο με τη χρήση του Twitter ροής API
    - αξιολόγηση του άποψη από αυτές τις tweets
    - Αποθήκευση των πληροφοριών άποψη στο HBase, χρησιμοποιώντας το Microsoft SDK HBase
- Η εφαρμογή Azure τοποθεσίες Web
    - Σχεδιάστε τα αποτελέσματα σε πραγματικό χρόνο στατιστικές στους χάρτες Bing χρησιμοποιώντας μια εφαρμογή web ASP.NET. Μια απεικόνιση της το tweets θα μοιάζει κάπως έτσι:

    ![hdinsight.hbase.Twitter.sentiment.Bing.Map][img-bing-map]
    
    Θα μπορούν να tweets ερωτήματος με συγκεκριμένες λέξεις-κλειδιά για να πάρετε μια ιδέα του εάν τη γνώμη εκφράζεται σε το tweets είναι θετικές, αρνητικές ή ουδέτερο.

Ένα ολοκληρωμένο δείγμα λύση Visual Studio μπορεί να βρεθεί στην GitHub: [εφαρμογή ανάλυσης κοινωνικών άποψη πραγματικού χρόνου](https://github.com/maxluk/tweet-sentiment).































### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Σύμπλεγμα του HBase στο HDInsight**. Για οδηγίες σχετικά με τη δημιουργία συμπλεγμάτων, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με τη χρήση HBase με Hadoop σε HDInsight][hbase-get-started]. Θα χρειαστείτε τα εξής δεδομένα για να μεταβείτε στο πρόγραμμα εκμάθησης:


    <table border="1">
    <tr><th>Ιδιότητα συμπλέγματος</th><th>Περιγραφή</th></tr>
    <tr><td>Όνομα HBase συμπλέγματος</td><td>Το όνομα συμπλέγματος HDInsight HBase. Για παράδειγμα: https://myhbase.azurehdinsight.net/</td></tr>
    <tr><td>Το όνομα χρήστη συμπλέγματος</td><td>Το όνομα λογαριασμού χρήστη Hadoop. Το προεπιλεγμένο όνομα χρήστη Hadoop είναι <strong>διαχειριστής</strong>.</td></tr>
    <tr><td>Σύμπλεγμα κωδικού πρόσβασης χρήστη</td><td>Κωδικός πρόσβασης χρήστη σύμπλεγμα Hadoop.</td></tr>
    </table>

- **Μια σταθμούς εργασίας** με το Visual Studio 2013 εγκατεστημένο. Για οδηγίες, ανατρέξτε στο θέμα [Την εγκατάσταση του Visual Studio](http://msdn.microsoft.com/library/e2h7fzkw.aspx).





## <a name="create-a-twitter-application-id-and-secrets"></a>Δημιουργήστε ένα Αναγνωριστικό εφαρμογής Twitter και απορρήτου

Το [διακριτικό](http://oauth.net/) χρησιμοποιούν ροής API για να εξουσιοδοτήσετε Twitter αιτήσεις. Το πρώτο βήμα για να χρησιμοποιήσετε διακριτικό είναι να δημιουργήσετε μια νέα εφαρμογή στην τοποθεσία για προγραμματιστές του Twitter.

**Για να δημιουργήσετε το Αναγνωριστικό εφαρμογής Twitter και απορρήτου**

1. Πραγματοποιήστε είσοδο στο [Twitter εφαρμογές](https://apps.twitter.com/). Εάν δεν έχετε ένα λογαριασμό του Twitter, κάντε κλικ στη σύνδεση **εγγραφή τώρα** .
2. Κάντε κλικ στην επιλογή **Δημιουργία νέας εφαρμογής**.
3. Πληκτρολογήστε ένα **όνομα**, **Περιγραφή**και **τοποθεσία Web**. Το όνομα της εφαρμογής Twitter πρέπει να είναι ένα μοναδικό όνομα. Το πεδίο τοποθεσία Web δεν χρησιμοποιείται πραγματικά. Δεν είναι απαραίτητο να είναι μια έγκυρη διεύθυνση URL. 
4. Επιλέξτε **Ναι, συμφωνείτε**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία την εφαρμογή του Twitter**.
5. Κάντε κλικ στην καρτέλα " **δικαιώματα** ". Τα προεπιλεγμένα δικαιώματα είναι **μόνο για ανάγνωση**. Αυτό είναι αρκετό για αυτό το πρόγραμμα εκμάθησης. 
6. Κάντε κλικ στην καρτέλα **πλήκτρα και οι κωδικοί πρόσβασης** .
7. Κάντε κλικ στην επιλογή **Δημιουργία μου διακριτικό πρόσβασης**.
8. Κάντε κλικ στην επιλογή **Δοκιμή διακριτικό** στην επάνω δεξιά γωνία της σελίδας.
9. Αντιγράψτε τις τιμές του **αριθμού-κλειδιού καταναλωτή**, **μυστικό καταναλωτή**, **διακριτικό πρόσβασης**και **μυστικό διακριτικό πρόσβασης** . Θα χρειαστείτε αυτές τις τιμές αργότερα στην εκμάθηση.

    ![hdi.hbase.Twitter.sentiment.Twitter.App][img-twitter-app]






























## <a name="create-twitter-streaming-service"></a>Δημιουργία Twitter ροή υπηρεσίας

Που χρειάζεστε για να δημιουργήσετε μια εφαρμογή για να λάβετε tweets, τον υπολογισμό tweet άποψη βαθμολογία και αποστολή των λέξεων επεξεργασία tweet HBase.

**Για να δημιουργήσετε την εφαρμογή ροής**

1. Ανοίξτε το **Visual Studio**και δημιουργία οπτικών C# κονσόλα εφαρμογής που ονομάζεται **TweetSentimentStreaming**. 
2. Από την **Κονσόλα διαχείρισης πακέτου**, εκτελέστε τις εξής εντολές:

        Install-Package Microsoft.HBase.Client -version 0.4.2.0
        Install-Package TweetinviAPI -version 1.0.0.0

    Αυτές οι εντολές εγκαταστήστε το πακέτο [HBase.NET SDK](https://www.nuget.org/packages/Microsoft.HBase.Client/) , το οποίο είναι η βιβλιοθήκη προγράμματος-πελάτη για να αποκτήσετε πρόσβαση στο σύμπλεγμα HBase, και το πακέτο [Tweetinvi API](https://www.nuget.org/packages/TweetinviAPI/) , η οποία χρησιμοποιείται για να αποκτήσετε πρόσβαση το API του Twitter.

    > [AZURE.NOTE] Το δείγμα που χρησιμοποιούνται σε αυτό το άρθρο έχει ελεγχθεί χρησιμοποιώντας την έκδοση που αναφέρονται παραπάνω.  Μπορείτε να καταργήσετε το έκδοση διακόπτη - για να εγκαταστήσετε την πιο πρόσφατη έκδοση.

3. Από την **Εξερεύνηση λύσεων**, προσθέστε **System.Configuration** στην αναφορά.
4. Προσθέστε ένα νέο αρχείο κλάσης στο έργο που ονομάζεται **HBaseWriter.cs**και, στη συνέχεια, αντικαταστήστε τον κώδικα με τα εξής:

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling;
        using org.apache.hadoop.hbase.rest.protobuf.generated;
        using Microsoft.HBase.Client;
        using Tweetinvi.Models;

        namespace TweetSentimentStreaming
        {
            class HBaseWriter
            {
                // HDinsight HBase cluster and HBase table information
                const string CLUSTERNAME = "https://<Enter Your Cluster Name>.azurehdinsight.net/";
                const string HADOOPUSERNAME = "admin"; //the default name is "admin"
                const string HADOOPUSERPASSWORD = "<Enter the Hadoop User Password>";

                const string HBASETABLENAME = "tweets_by_words";
                const string COUNT_ROW_KEY = "~ROWCOUNT";
                const string COUNT_COLUMN_NAME = "d:COUNT";
                
                long rowCount = 0;

                // Sentiment dictionary file and the punctuation characters
                const string DICTIONARYFILENAME = @"..\..\dictionary.tsv";
                private static char[] _punctuationChars = new[] {
            ' ', '!', '\"', '#', '$', '%', '&', '\'', '(', ')', '*', '+', ',', '-', '.', '/',   //ascii 23--47
            ':', ';', '<', '=', '>', '?', '@', '[', ']', '^', '_', '`', '{', '|', '}', '~' };   //ascii 58--64 + misc.

                // For writting to HBase
                HBaseClient client;

                // a sentiment dictionary for estimate sentiment. It is loaded from a physical file.
                Dictionary<string, DictionaryItem> dictionary;

                // use multithread write
                Thread writerThread;
                Queue<ITweet> queue = new Queue<ITweet>();
                bool threadRunning = true;

                // This function connects to HBase, loads the sentiment dictionary, and starts the thread for writting.
                public HBaseWriter()
                {
                    ClusterCredentials credentials = new ClusterCredentials(new Uri(CLUSTERNAME), HADOOPUSERNAME, HADOOPUSERPASSWORD);
                    client = new HBaseClient(credentials);

                    // create the HBase table if it doesn't exist
                    if (!client.ListTablesAsync().Result.name.Contains(HBASETABLENAME))
                    {
                        TableSchema tableSchema = new TableSchema();
                        tableSchema.name = HBASETABLENAME;
                        tableSchema.columns.Add(new ColumnSchema { name = "d" });
                        client.CreateTableAsync(tableSchema).Wait();
                        Console.WriteLine("Table \"{0}\" is created.", HBASETABLENAME);
                    }

                    // Read current row count cell
                    rowCount = GetRowCount();

                    // Load sentiment dictionary from a file
                    LoadDictionary();

                    // Start a thread for writting to HBase
                    writerThread = new Thread(new ThreadStart(WriterThreadFunction));
                    writerThread.Start();
                }

                ~HBaseWriter()
                {
                    threadRunning = false;
                }

                private long GetRowCount()
                {
                    try
                    {
                        RequestOptions options = RequestOptions.GetDefaultOptions();
                        options.RetryPolicy = RetryPolicy.NoRetry;
                        var cellSet = client.GetCellsAsync(HBASETABLENAME, COUNT_ROW_KEY, null, null, options).Result;
                        if (cellSet.rows.Count != 0)
                        {
                            var countCol = cellSet.rows[0].values.Find(cell => Encoding.UTF8.GetString(cell.column) == COUNT_COLUMN_NAME);
                            if (countCol != null)
                            {
                                return Convert.ToInt64(Encoding.UTF8.GetString(countCol.data));
                            }
                        }
                    }
                    catch(Exception ex)
                    {
                        if (ex.InnerException.Message.Equals("The remote server returned an error: (404) Not Found.", StringComparison.OrdinalIgnoreCase))
                        {
                            return 0;
                        }
                        else
                        {
                            throw ex;
                        }
                        
                    }

                    return 0;
                }

                // Enqueue the Tweets received
                public void WriteTweet(ITweet tweet)
                {
                    lock (queue)
                    {
                        queue.Enqueue(tweet);
                    }
                }

                // Load sentiment dictionary from a file
                private void LoadDictionary()
                {
                    List<string> lines = File.ReadAllLines(DICTIONARYFILENAME).ToList();
                    var items = lines.Select(line =>
                    {
                        var fields = line.Split('\t');
                        var pos = 0;
                        return new DictionaryItem
                        {
                            Type = fields[pos++],
                            Length = Convert.ToInt32(fields[pos++]),
                            Word = fields[pos++],
                            Pos = fields[pos++],
                            Stemmed = fields[pos++],
                            Polarity = fields[pos++]
                        };
                    });

                    dictionary = new Dictionary<string, DictionaryItem>();
                    foreach (var item in items)
                    {
                        if (!dictionary.Keys.Contains(item.Word))
                        {
                            dictionary.Add(item.Word, item);
                        }
                    }
                }

                // Calculate sentiment score
                private int CalcSentimentScore(string[] words)
                {
                    Int32 total = 0;
                    foreach (string word in words)
                    {
                        if (dictionary.Keys.Contains(word))
                        {
                            switch (dictionary[word].Polarity)
                            {
                                case "negative": total -= 1; break;
                                case "positive": total += 1; break;
                            }
                        }
                    }
                    if (total > 0)
                    {
                        return 1;
                    }
                    else if (total < 0)
                    {
                        return -1;
                    }
                    else
                    {
                        return 0;
                    }
                }

                // Popular a CellSet object to be written into HBase
                private void CreateTweetByWordsCells(CellSet set, ITweet tweet)
                {
                    // Split the Tweet into words
                    string[] words = tweet.Text.ToLower().Split(_punctuationChars);

                    // Calculate sentiment score base on the words
                    int sentimentScore = CalcSentimentScore(words);
                    var word_pairs = words.Take(words.Length - 1)
                                        .Select((word, idx) => string.Format("{0} {1}", word, words[idx + 1]));
                    var all_words = words.Concat(word_pairs).ToList();

                    // For each word in the Tweet add a row to the HBase table
                    foreach (string word in all_words)
                    {
                        string time_index = (ulong.MaxValue - (ulong)tweet.CreatedAt.ToBinary()).ToString().PadLeft(20) + tweet.IdStr;
                        string key = word + "_" + time_index;

                        // Create a row
                        var row = new CellSet.Row { key = Encoding.UTF8.GetBytes(key) };

                        // Add columns to the row, including Tweet identifier, language, coordinator(if available), and sentiment 
                        var value = new Cell { column = Encoding.UTF8.GetBytes("d:id_str"), data = Encoding.UTF8.GetBytes(tweet.IdStr) };
                        row.values.Add(value);

                        value = new Cell { column = Encoding.UTF8.GetBytes("d:lang"), data = Encoding.UTF8.GetBytes(tweet.Language.ToString()) };
                        row.values.Add(value);

                        if (tweet.Coordinates != null)
                        {
                            var str = tweet.Coordinates.Longitude.ToString() + "," + tweet.Coordinates.Latitude.ToString();
                            value = new Cell { column = Encoding.UTF8.GetBytes("d:coor"), data = Encoding.UTF8.GetBytes(str) };
                            row.values.Add(value);
                        }

                        value = new Cell { column = Encoding.UTF8.GetBytes("d:sentiment"), data = Encoding.UTF8.GetBytes(sentimentScore.ToString()) };
                        row.values.Add(value);

                        set.rows.Add(row);
                    }
                }

                // Write a Tweet (CellSet) to HBase
                public void WriterThreadFunction()
                {
                    try
                    {
                        while (threadRunning)
                        {
                            if (queue.Count > 0)
                            {
                                CellSet set = new CellSet();
                                lock (queue)
                                {
                                    do
                                    {
                                        ITweet tweet = queue.Dequeue();
                                        CreateTweetByWordsCells(set, tweet);
                                    } while (queue.Count > 0);
                                }

                                // Write the Tweet by words cell set to the HBase table
                                client.StoreCellsAsync(HBASETABLENAME, set).Wait();
                                Console.WriteLine("\tRows written: {0}", set.rows.Count);
                            }
                            Thread.Sleep(100);
                        }
                    }
                    catch (Exception ex)
                    {
                        Console.WriteLine("Exception: " + ex.Message);
                    }
                }
            }
            public class DictionaryItem
            {
                public string Type { get; set; }
                public int Length { get; set; }
                public string Word { get; set; }
                public string Pos { get; set; }
                public string Stemmed { get; set; }
                public string Polarity { get; set; }
            }
        }

6. Ορίστε τις σταθερές στον προηγούμενο κώδικα, συμπεριλαμβανομένων των **CLUSTERNAME**, **HADOOPUSERNAME**, **HADOOPUSERPASSWORD**και DICTIONARYFILENAME. Το DICTIONARYFILENAME είναι το όνομα του αρχείου και τη θέση του το direction.tsv.  Το αρχείο μπορούν να ληφθούν από **https://hditutorialdata.blob.core.windows.net/twittersentiment/dictionary.tsv**. Εάν θέλετε να αλλάξετε το όνομα του πίνακα HBase, πρέπει να αλλάξετε το όνομα του πίνακα στην εφαρμογή web ανάλογα.

7. Άνοιγμα **Program.cs**και να αντικαταστήσετε τον κωδικό με τα εξής:

        using System;
        using System.Diagnostics;
        using Tweetinvi;
        using Tweetinvi.Models;

        namespace TweetSentimentStreaming
        {
            class Program
            {
                const string TWITTERAPPACCESSTOKEN = "<Enter Twitter App Access Token>";
                const string TWITTERAPPACCESSTOKENSECRET = "<Enter Twitter Access Token Secret>";
                const string TWITTERAPPAPIKEY = "<Enter Twitter App API Key>";
                const string TWITTERAPPAPISECRET = "<Enter Twitter App API Secret>";

                static void Main(string[] args)
                {
                    Auth.SetUserCredentials(TWITTERAPPAPIKEY, TWITTERAPPAPISECRET, TWITTERAPPACCESSTOKEN, TWITTERAPPACCESSTOKENSECRET);

                    Stream_FilteredStreamExample();
                }

                private static void Stream_FilteredStreamExample()
                {
                    for (;;)
                    {
                        try
                        {
                            HBaseWriter hbase = new HBaseWriter();
                            var stream = Stream.CreateFilteredStream();
                            stream.AddLocation(new Coordinates(-180, -90), new Coordinates(180, 90)); 

                            var tweetCount = 0;
                            var timer = Stopwatch.StartNew();

                            stream.MatchingTweetReceived += (sender, args) =>
                            {
                                tweetCount++;
                                var tweet = args.Tweet;

                                // Write Tweets to HBase
                                hbase.WriteTweet(tweet);

                                if (timer.ElapsedMilliseconds > 1000)
                                {
                                    if (tweet.Coordinates != null)
                                    {
                                        Console.ForegroundColor = ConsoleColor.Green;
                                        Console.WriteLine("\n{0}: {1} {2}", tweet.Id, tweet.Language.ToString(), tweet.Text);
                                        Console.ForegroundColor = ConsoleColor.White;
                                        Console.WriteLine("\tLocation: {0}, {1}", tweet.Coordinates.Longitude, tweet.Coordinates.Latitude);
                                    }

                                    timer.Restart();
                                    Console.WriteLine("\tTweets/sec: {0}", tweetCount);
                                    tweetCount = 0;
                                }
                            };

                            stream.StartStreamMatchingAllConditions();
                        }
                        catch (Exception ex)
                        {
                            Console.WriteLine("Exception: {0}", ex.Message);
                        }
                    }
                }

            }
        }

8. Ορίστε τις σταθερές, συμπεριλαμβανομένων των **TWITTERAPPACCESSTOKEN**, **TWITTERAPPACCESSTOKENSECRET**, **TWITTERAPPAPIKEY** και **TWITTERAPPAPISECRET**. 

Για να εκτελέσετε την υπηρεσία ροής, πατήστε το πλήκτρο **F5**. Ακολουθεί ένα στιγμιότυπο οθόνης της εφαρμογής κονσόλας:

![hdinsight.hbase.Twitter.sentiment.Streaming.Service][img-streaming-service]
    
Διατηρήστε τη ροή κονσόλας εφαρμογή εκτελείται κατά την ανάπτυξη της εφαρμογής web, ώστε να έχετε περισσότερα δεδομένα για να χρησιμοποιήσετε. Για να εξετάσετε τα δεδομένα που έχουν εισαχθεί σε πίνακα, μπορείτε να χρησιμοποιήσετε HBase κελύφους. Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το HBase στο HDInsight](hdinsight-hbase-tutorial-get-started.md#create-tables-and-insert-data).


## <a name="visualize-real-time-sentiment"></a>Απεικόνιση άποψη σε πραγματικό χρόνο

Σε αυτήν την ενότητα, θα δημιουργήσετε μια εφαρμογή web ASP.NET MVC να διαβάζει τα δεδομένα σε πραγματικό χρόνο άποψη HBase και να σχεδιάσετε τα δεδομένα στους χάρτες Bing.

**Για να δημιουργήσετε μια εφαρμογή ASP.NET MVC Web**

1. Ανοίξτε το Visual Studio.
2. Κάντε κλικ στο **αρχείο**, κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **έργο**.
3. Εισαγάγετε τις παρακάτω πληροφορίες:

    - Κατηγορία προτύπου: **Visual C# / Web**
    - Πρότυπο: **εφαρμογή ASP.NET Web**
    - Όνομα: **TweetSentimentWeb**
    - Θέση: **C:\Tutorials** 
4. Κάντε κλικ στο **κουμπί OK**.
5. Στην περιοχή **Επιλογή προτύπου**, κάντε κλικ στην επιλογή **MVC**. 
6. Στο **Microsoft Azure**, κάντε κλικ στην επιλογή **Διαχείριση εγγραφών**.
7. **Διαχείριση του Microsoft Azure συνδρομές**, κάντε κλικ στην επιλογή **Είσοδος**.
8. Εισαγάγετε τα διαπιστευτήριά σας Azure. Πληροφορίες Azure τη συνδρομή σας θα εμφανίζεται στην καρτέλα **Λογαριασμοί** .
9. Κάντε κλικ στο κουμπί **Κλείσιμο** για να κλείσετε το παράθυρο " **Διαχείριση Microsoft Azure συνδρομές** ".
10. Από το **Νέο έργο ASP.NET - TweetSentimentWeb**, κάντε κλικ στο **κουμπί OK**.
11. Από τη **Ρύθμιση παραμέτρων του Microsoft Azure ρυθμίσεις τοποθεσίας**, επιλέξτε την **περιοχή** που βρίσκεται πιο κοντά σε εσάς. Δεν χρειάζεται να ορίσετε έναν διακομιστή βάσης δεδομένων. 
12. Κάντε κλικ στο **κουμπί OK**.

**Για να εγκαταστήσετε τα πακέτα NuGet**

1. Από το μενού **Εργαλεία** , κάντε κλικ στην επιλογή **Διαχείριση πακέτου Nuget**και, στη συνέχεια, κάντε κλικ στην επιλογή **Κονσόλα διαχείρισης πακέτου**. Ο πίνακας κονσόλας ανοίγει στο κάτω μέρος της σελίδας.
2. Χρησιμοποιήστε την ακόλουθη εντολή για να εγκαταστήσετε το πακέτο [HBase.NET SDK](https://www.nuget.org/packages/Microsoft.HBase.Client/) , το οποίο είναι η βιβλιοθήκη προγράμματος-πελάτη για πρόσβαση στο σύμπλεγμα HBase:

        Install-Package Microsoft.HBase.Client 

**Για να προσθέσετε κλάση HBaseReader**

1. Από την **Εξερεύνηση λύσεων**, αναπτύξτε το στοιχείο **TweetSentiment**.
2. Κάντε δεξί κλικ σε **μοντέλα**, κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **τάξης**.
3. Στο πεδίο **όνομα** , πληκτρολογήστε **HBaseReader.cs**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.
4. Αντικαταστήστε τον κώδικα με τα εξής:

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Web;
        
        using System.Configuration;
        using System.Threading.Tasks;
        using System.Text;
        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;
        
        namespace TweetSentimentWeb.Models
        {
            public class HBaseReader
            {
                // For reading Tweet sentiment data from HDInsight HBase
                HBaseClient client;
        
                // HDinsight HBase cluster and HBase table information
                const string CLUSTERNAME = "<HBaseClusterName>";
                const string HADOOPUSERNAME = "<HBaseClusterHadoopUserName>"
                const string HADOOPUSERPASSWORD = "<HBaseCluserUserPassword>";
                const string HBASETABLENAME = "tweets_by_words";
        
                // The constructor
                public HBaseReader()
                {
                    ClusterCredentials creds = new ClusterCredentials(
                                    new Uri(CLUSTERNAME),
                                    HADOOPUSERNAME,
                                    HADOOPUSERPASSWORD);
                    client = new HBaseClient(creds);
                }
        
                // Query Tweets sentiment data from the HBase table asynchronously 
                public async Task<IEnumerable<Tweet>> QueryTweetsByKeywordAsync(string keyword)
                {
                    List<Tweet> list = new List<Tweet>();
        
                    // Demonstrate Filtering the data from the past 6 hours the row key
                    string timeIndex = (ulong.MaxValue -
                        (ulong)DateTime.UtcNow.Subtract(new TimeSpan(6, 0, 0)).ToBinary()).ToString().PadLeft(20);
                    string startRow = keyword + "_" + timeIndex;
                    string endRow = keyword + "|";
                    Scanner scanSettings = new Scanner
                    {
                        batch = 100000,
                        startRow = Encoding.UTF8.GetBytes(startRow),
                        endRow = Encoding.UTF8.GetBytes(endRow)
                    };
        
                    // Make async scan call
                    ScannerInformation scannerInfo =
                        await client.CreateScannerAsync(HBASETABLENAME, scanSettings);
        
                    CellSet next;
        
                    while ((next = await client.ScannerGetNextAsync(scannerInfo)) != null)
                    {
                        foreach (CellSet.Row row in next.rows)
                        {
                            // find the cell with string pattern "d:coor" 
                            var coordinates =
                                row.values.Find(c => Encoding.UTF8.GetString(c.column) == "d:coor");
        
                            if (coordinates != null)
                            {
                                string[] lonlat = Encoding.UTF8.GetString(coordinates.data).Split(',');
        
                                var sentimentField =
                                    row.values.Find(c => Encoding.UTF8.GetString(c.column) == "d:sentiment");
                                Int32 sentiment = 0;
                                if (sentimentField != null)
                                {
                                    sentiment = Convert.ToInt32(Encoding.UTF8.GetString(sentimentField.data));
                                }
        
                                list.Add(new Tweet
                                {
                                    Longtitude = Convert.ToDouble(lonlat[0]),
                                    Latitude = Convert.ToDouble(lonlat[1]),
                                    Sentiment = sentiment
                                });
                            }
        
                            if (coordinates != null)
                            {
                                string[] lonlat = Encoding.UTF8.GetString(coordinates.data).Split(',');
                            }
                        }
                    }
        
                    return list;
                }
            }
        
            public class Tweet
            {
                public string IdStr { get; set; }
                public string Text { get; set; }
                public string Lang { get; set; }
                public double Longtitude { get; set; }
                public double Latitude { get; set; }
                public int Sentiment { get; set; }
            }
        }

4. Μέσα στην κλάση **HBaseReader** , αλλάξτε τις σταθερές τιμές ως εξής:

    - **CLUSTERNAME**: το HBase σύμπλεγμα όνομα, για παράδειγμα, *https://<HBaseClusterName>.azurehdinsight.net/*. 
    - **HADOOPUSERNAME**: το HBase σύμπλεγμα Hadoop όνομα χρήστη. Το προεπιλεγμένο όνομα είναι *διαχειριστής*.
    - **HADOOPUSERPASSWORD**: το HBase σύμπλεγμα Hadoop χρήστη, τον κωδικό πρόσβασης.
    - **HBASETABLENAME** = "tweets_by_words";

    Το όνομα του πίνακα HBase είναι **"tweets_by_words"**. Οι τιμές πρέπει να συμφωνούν με τις τιμές που στείλατε στην υπηρεσία ροής, έτσι ώστε η εφαρμογή web διαβάζει τα δεδομένα από τον ίδιο πίνακα HBase.




**Για να προσθέσετε ελεγκτή TweetsController**

1. Από την **Εξερεύνηση λύσεων**, αναπτύξτε το στοιχείο **TweetSentimentWeb**.
2. Κάντε δεξί κλικ **ελεγκτές**, κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **ελεγκτή**.
3. Κάντε κλικ στην επιλογή **Ελεγκτή 2 API Web - είναι κενή**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.
4. Στο πεδίο **Όνομα ελεγκτή** , πληκτρολογήστε **TweetsController**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.
5. Από την **Εξερεύνηση λύσεων**, κάντε διπλό κλικ TweetsController.cs για να ανοίξετε το αρχείο.
5. Τροποποιήστε το αρχείο, ώστε να μοιάζει με τα εξής:

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Http;
        
        using System.Threading.Tasks;
        using TweetSentimentWeb.Models;
        
        namespace TweetSentimentWeb.Controllers
        {
            public class TweetsController : ApiController
            {
                HBaseReader hbase = new HBaseReader();
        
                public async Task<IEnumerable<Tweet>> GetTweetsByQuery(string query)
                {
                    return await hbase.QueryTweetsByKeywordAsync(query);
                }
            }
        }

**Για να προσθέσετε heatmap.js**

1. Από την **Εξερεύνηση λύσεων**, αναπτύξτε το στοιχείο **TweetSentimentWeb**.
2. Κάντε δεξί κλικ σε **δέσμες ενεργειών**, κάντε κλικ στην επιλογή **Προσθήκη**, κάντε κλικ στην επιλογή **Αρχείο JavaScript**.
3. Στο πεδίο **όνομα στοιχείου** , πληκτρολογήστε **heatmap.js**.
4. Επικολλήστε τον ακόλουθο κώδικα στο αρχείο. Ο κώδικας έχει συνταχθεί από Alastair Aitchison. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [βιβλιοθήκη HeatMap v7 AJAX χάρτες Bing](http://alastaira.wordpress.com/2011/04/15/bing-maps-ajax-v7-heatmap-library/).

        /*******************************************************************************
        * Author: Alastair Aitchison
        * Website: http://alastaira.wordpress.com
        * Date: 15th April 2011
        * 
        * Description: 
        * This JavaScript file provides an algorithm that can be used to add a heatmap
        * overlay on a Bing Maps v7 control. The intensity and temperature palette
        * of the heatmap are designed to be easily customisable.
        *
        * Requirements:
        * The heatmap layer itself is created dynamically on the client-side using
        * the HTML5 <canvas> element, and therefore requires a browser that supports
        * this element. It has been tested on IE9, Firefox 3.6/4 and 
        * Chrome 10 browsers. If you can confirm whether it works on other browsers or
        * not, I'd love to hear from you!
        
        * Usage:
        * The HeatMapLayer constructor requires:
        * - A reference to a map object
        * - An array or Microsoft.Maps.Location items
        * - Optional parameters to customise the appearance of the layer
        *  (Radius,, Unit, Intensity, and ColourGradient), and a callback function
        *
        */
        
        var HeatMapLayer = function (map, locations, options) {
        
            /* Private Properties */
            var _map = map,
              _canvas,
              _temperaturemap,
              _locations = [],
              _viewchangestarthandler,
              _viewchangeendhandler;
        
            // Set default options
            var _options = {
                // Opacity at the centre of each heat point
                intensity: 0.5,
        
                // Affected radius of each heat point
                radius: 1000,
        
                // Whether the radius is an absolute pixel value or meters
                unit: 'meters',
        
                // Colour temperature gradient of the map
                colourgradient: {
                    "0.00": 'rgba(255,0,255,20)',  // Magenta
                    "0.25": 'rgba(0,0,255,40)',    // Blue
                    "0.50": 'rgba(0,255,0,80)',    // Green
                    "0.75": 'rgba(255,255,0,120)', // Yellow
                    "1.00": 'rgba(255,0,0,150)'    // Red
                },
        
                // Callback function to be fired after heatmap layer has been redrawn 
                callback: null
            };
        
            /* Private Methods */
            function _init() {
                var _mapDiv = _map.getRootElement();
        
                if (_mapDiv.childNodes.length >= 3 && _mapDiv.childNodes[2].childNodes.length >= 2) {
                    // Create the canvas element
                    _canvas = document.createElement('canvas');
                    _canvas.style.position = 'relative';
        
                    var container = document.createElement('div');
                    container.style.position = 'absolute';
                    container.style.left = '0px';
                    container.style.top = '0px';
                    container.appendChild(_canvas);
        
                    _mapDiv.childNodes[2].childNodes[1].appendChild(container);
        
                    // Override defaults with any options passed in the constructor
                    _setOptions(options);
        
                    // Load array of location data
                    _setPoints(locations);
        
                    // Create a colour gradient from the suppied colourstops
                    _temperaturemap = _createColourGradient(_options.colourgradient);
        
                    // Wire up the event handler to redraw heatmap canvas
                    _viewchangestarthandler = Microsoft.Maps.Events.addHandler(_map, 'viewchangestart', _clearHeatMap);
                    _viewchangeendhandler = Microsoft.Maps.Events.addHandler(_map, 'viewchangeend', _createHeatMap);
        
                    _createHeatMap();
        
                    delete _init;
                } else {
                    setTimeout(_init, 100);
                }
            }
        
            // Resets the heat map
            function _clearHeatMap() {
                var ctx = _canvas.getContext("2d");
                ctx.clearRect(0, 0, _canvas.width, _canvas.height);
            }
        
            // Creates a colour gradient from supplied colour stops on initialisation
            function _createColourGradient(colourstops) {
                var ctx = document.createElement('canvas').getContext('2d');
                var grd = ctx.createLinearGradient(0, 0, 256, 0);
                for (var c in colourstops) {
                    grd.addColorStop(c, colourstops[c]);
                }
                ctx.fillStyle = grd;
                ctx.fillRect(0, 0, 256, 1);
                return ctx.getImageData(0, 0, 256, 1).data;
            }
        
            // Applies a colour gradient to the intensity map
            function _colouriseHeatMap() {
                var ctx = _canvas.getContext("2d");
                var dat = ctx.getImageData(0, 0, _canvas.width, _canvas.height);
                var pix = dat.data; // pix is a CanvasPixelArray containing height x width x 4 bytes of data (RGBA)
                for (var p = 0, len = pix.length; p < len;) {
                    var a = pix[p + 3] * 4; // get the alpha of this pixel
                    if (a != 0) { // If there is any data to plot
                        pix[p] = _temperaturemap[a]; // set the red value of the gradient that corresponds to this alpha
                        pix[p + 1] = _temperaturemap[a + 1]; //set the green value based on alpha
                        pix[p + 2] = _temperaturemap[a + 2]; //set the blue value based on alpha
                    }
                    p += 4; // Move on to the next pixel
                }
                ctx.putImageData(dat, 0, 0);
            }
        
            // Sets any options passed in
            function _setOptions(options) {
                for (attrname in options) {
                    _options[attrname] = options[attrname];
                }
            }
        
            // Sets the heatmap points from an array of Microsoft.Maps.Locations  
            function _setPoints(locations) {
                _locations = locations;
            }
        
            // Main method to draw the heatmap
            function _createHeatMap() {
                // Ensure the canvas matches the current dimensions of the map
                // This also has the effect of resetting the canvas
                _canvas.height = _map.getHeight();
                _canvas.width = _map.getWidth();
        
                _canvas.style.top = -_canvas.height / 2 + 'px';
                _canvas.style.left = -_canvas.width / 2 + 'px';
        
                // Calculate the pixel radius of each heatpoint at the current map zoom
                if (_options.unit == "pixels") {
                    radiusInPixel = _options.radius;
                } else {
                    radiusInPixel = _options.radius / _map.getMetersPerPixel();
                }
        
                var ctx = _canvas.getContext("2d");
        
                // Convert lat/long to pixel location
                var pixlocs = _map.tryLocationToPixel(_locations, Microsoft.Maps.PixelReference.control);
                var shadow = 'rgba(0, 0, 0, ' + _options.intensity + ')';
                var mapWidth = 256 * Math.pow(2, _map.getZoom());
        
                // Create the Intensity Map by looping through each location
                for (var i = 0, len = pixlocs.length; i < len; i++) {
                    var x = pixlocs[i].x;
                    var y = pixlocs[i].y;
        
                    if (x < 0) {
                        x += mapWidth * Math.ceil(Math.abs(x / mapWidth));
                    }
        
                    // Create radial gradient centred on this point
                    var grd = ctx.createRadialGradient(x, y, 0, x, y, radiusInPixel);
                    grd.addColorStop(0.0, shadow);
                    grd.addColorStop(1.0, 'transparent');
        
                    // Draw the heatpoint onto the canvas
                    ctx.fillStyle = grd;
                    ctx.fillRect(x - radiusInPixel, y - radiusInPixel, 2 * radiusInPixel, 2 * radiusInPixel);
                }
        
                // Apply the specified colour gradient to the intensity map
                _colouriseHeatMap();
        
                // Call the callback function, if specified
                if (_options.callback) {
                    _options.callback();
                }
            }
        
            /* Public Methods */
        
            this.Show = function () {
                if (_canvas) {
                    _canvas.style.display = '';
                }
            };
        
            this.Hide = function () {
                if (_canvas) {
                    _canvas.style.display = 'none';
                }
            };
        
            // Sets options for intensity, radius, colourgradient etc.
            this.SetOptions = function (options) {
                _setOptions(options);
            }
        
            // Sets an array of Microsoft.Maps.Locations from which the heatmap is created
            this.SetPoints = function (locations) {
                // Reset the existing heatmap layer
                _clearHeatMap();
                // Pass in the new set of locations
                _setPoints(locations);
                // Recreate the layer
                _createHeatMap();
            }
        
            // Removes the heatmap layer from the DOM
            this.Remove = function () {
                _canvas.parentNode.parentNode.removeChild(_canvas.parentNode);
        
                if (_viewchangestarthandler) { Microsoft.Maps.Events.removeHandler(_viewchangestarthandler); }
                if (_viewchangeendhandler) { Microsoft.Maps.Events.removeHandler(_viewchangeendhandler); }
        
                _locations = null;
                _temperaturemap = null;
                _canvas = null;
                _options = null;
                _viewchangestarthandler = null;
                _viewchangeendhandler = null;
            }
        
            // Call the initialisation routine
            _init();
        };
        
        // Call the Module Loaded method
        Microsoft.Maps.moduleLoaded('HeatMapModule');


**Για να προσθέσετε twitterStream.js**

1. Από την **Εξερεύνηση λύσεων**, αναπτύξτε το στοιχείο **TweetSentimentWeb**.
2. Κάντε δεξί κλικ σε **δέσμες ενεργειών**, κάντε κλικ στην επιλογή **Προσθήκη**, κάντε κλικ στην επιλογή **Αρχείο JavaScript**.
3. Στο πεδίο **όνομα στοιχείου** , πληκτρολογήστε**twitterStream.js**.
4. Αντιγράψτε και επικολλήστε τον παρακάτω κώδικα στο αρχείο:

        var liveTweetsPos = [];
        var liveTweets = [];
        var liveTweetsNeg = [];
        var map;
        var heatmap;
        var heatmapNeg;
        var heatmapPos;
        
        function initialize() {
            // Initialize the map
            var options = {
                credentials: "AvFJTZPZv8l3gF8VC3Y7BPBd0r7LKo8dqKG02EAlqg9WAi0M7la6zSIT-HwkMQbx",
                center: new Microsoft.Maps.Location(23.0, 8.0),
                mapTypeId: Microsoft.Maps.MapTypeId.ordnanceSurvey,
                labelOverlay: Microsoft.Maps.LabelOverlay.hidden,
                zoom: 2.5
            };
            var map = new Microsoft.Maps.Map(document.getElementById('map_canvas'), options);
        
            // Heatmap options for positive, neutral and negative layers
        
            var heatmapOptions = {
                // Opacity at the centre of each heat point
                intensity: 0.5,
        
                // Affected radius of each heat point
                radius: 15,
        
                // Whether the radius is an absolute pixel value or meters
                unit: 'pixels'
            };
        
            var heatmapPosOptions = {
                // Opacity at the centre of each heat point
                intensity: 0.5,
        
                // Affected radius of each heat point
                radius: 15,
        
                // Whether the radius is an absolute pixel value or meters
                unit: 'pixels',
        
                colourgradient: {
                    0.0: 'rgba(0, 255, 255, 0)',
                    0.1: 'rgba(0, 255, 255, 1)',
                    0.2: 'rgba(0, 255, 191, 1)',
                    0.3: 'rgba(0, 255, 127, 1)',
                    0.4: 'rgba(0, 255, 63, 1)',
                    0.5: 'rgba(0, 127, 0, 1)',
                    0.7: 'rgba(0, 159, 0, 1)',
                    0.8: 'rgba(0, 191, 0, 1)',
                    0.9: 'rgba(0, 223, 0, 1)',
                    1.0: 'rgba(0, 255, 0, 1)'
                }
            };
        
            var heatmapNegOptions = {
                // Opacity at the centre of each heat point
                intensity: 0.5,
        
                // Affected radius of each heat point
                radius: 15,
        
                // Whether the radius is an absolute pixel value or meters
                unit: 'pixels',
        
                colourgradient: {
                    0.0: 'rgba(0, 255, 255, 0)',
                    0.1: 'rgba(0, 255, 255, 1)',
                    0.2: 'rgba(0, 191, 255, 1)',
                    0.3: 'rgba(0, 127, 255, 1)',
                    0.4: 'rgba(0, 63, 255, 1)',
                    0.5: 'rgba(0, 0, 127, 1)',
                    0.7: 'rgba(0, 0, 159, 1)',
                    0.8: 'rgba(0, 0, 191, 1)',
                    0.9: 'rgba(0, 0, 223, 1)',
                    1.0: 'rgba(0, 0, 255, 1)'
                }
            };
        
            // Register and load the Client Side HeatMap Module
            Microsoft.Maps.registerModule("HeatMapModule", "scripts/heatmap.js");
            Microsoft.Maps.loadModule("HeatMapModule", {
                callback: function () {
                    // Create heatmap layers for positive, neutral and negative tweets
                    heatmapPos = new HeatMapLayer(map, liveTweetsPos, heatmapPosOptions);
                    heatmap = new HeatMapLayer(map, liveTweets, heatmapOptions);
                    heatmapNeg = new HeatMapLayer(map, liveTweetsNeg, heatmapNegOptions);
                }
            });
        
            $("#searchbox").val("xbox");
            $("#searchBtn").click(onsearch);
            $("#positiveBtn").click(onPositiveBtn);
            $("#negativeBtn").click(onNegativeBtn);
            $("#neutralBtn").click(onNeutralBtn);
            $("#neutralBtn").button("toggle");
        }
        
        function onsearch() {
            var uri = 'api/tweets?query=';
            var query = $('#searchbox').val();
            $.getJSON(uri + query)
                .done(function (data) {
                    liveTweetsPos = [];
                    liveTweets = [];
                    liveTweetsNeg = [];
        
                    // On success, 'data' contains a list of tweets.
                    $.each(data, function (key, item) {
                        addTweet(item);
                    });
        
                    if (!$("#neutralBtn").hasClass('active')) {
                        $("#neutralBtn").button("toggle");
                    }
                    onNeutralBtn();
                })
                .fail(function (jqXHR, textStatus, err) {
                    $('#statustext').text('Error: ' + err);
                });
        }
        
        function addTweet(item) {
            //Add tweet to the heat map arrays.
            var tweetLocation = new Microsoft.Maps.Location(item.Latitude, item.Longtitude);
            if (item.Sentiment > 0) {
                liveTweetsPos.push(tweetLocation);
            } else if (item.Sentiment < 0) {
                liveTweetsNeg.push(tweetLocation);
            } else {
                liveTweets.push(tweetLocation);
            }
        }
        
        function onPositiveBtn() {
            if ($("#neutralBtn").hasClass('active')) {
                $("#neutralBtn").button("toggle");
            }
            if ($("#negativeBtn").hasClass('active')) {
                $("#negativeBtn").button("toggle");
            }
        
            heatmapPos.SetPoints(liveTweetsPos);
            heatmapPos.Show();
            heatmapNeg.Hide();
            heatmap.Hide();
        
            $('#statustext').text('Tweets: ' + liveTweetsPos.length + "   " + getPosNegRatio());
        }
        
        function onNeutralBtn() {
            if ($("#positiveBtn").hasClass('active')) {
                $("#positiveBtn").button("toggle");
            }
            if ($("#negativeBtn").hasClass('active')) {
                $("#negativeBtn").button("toggle");
            }
        
            heatmap.SetPoints(liveTweets);
            heatmap.Show();
            heatmapNeg.Hide();
            heatmapPos.Hide();
        
            $('#statustext').text('Tweets: ' + liveTweets.length + "   " + getPosNegRatio());
        }
        
        function onNegativeBtn() {
            if ($("#positiveBtn").hasClass('active')) {
                $("#positiveBtn").button("toggle");
            }
            if ($("#neutralBtn").hasClass('active')) {
                $("#neutralBtn").button("toggle");
            }
        
            heatmapNeg.SetPoints(liveTweetsNeg);
            heatmapNeg.Show();
            heatmap.Hide();;
            heatmapPos.Hide();;
        
            $('#statustext').text('Tweets: ' + liveTweetsNeg.length + "\t" + getPosNegRatio());
        }
        
        function getPosNegRatio() {
            if (liveTweetsNeg.length == 0) {
                return "";
            }
            else {
                var ratio = liveTweetsPos.length / liveTweetsNeg.length;
                var str = parseFloat(Math.round(ratio * 10) / 10).toFixed(1);
                return "Positive/Negative Ratio: " + str;
            }
        }


**Για να τροποποιήσετε το layout.cshtml**

1. Από την **Εξερεύνηση λύσεων**, αναπτύξτε το στοιχείο **TweetSentimentWeb**, αναπτύξτε **προβολές**, αναπτύξτε το στοιχείο **κοινή χρήση**και, στη συνέχεια, κάντε διπλό κλικ _**Layout.cshtml**.
2. Αντικαταστήστε το περιεχόμενο με τα εξής:

        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="utf-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>@ViewBag.Title</title>
            @Styles.Render("~/Content/css")
            @Scripts.Render("~/bundles/modernizr")
            <!-- Bing Maps -->
            <script type="text/javascript" src="http://ecn.dev.virtualearth.net/mapcontrol/mapcontrol.ashx?v=7.0&mkt=en-gb"></script>
            <!-- Spatial Dashboard JavaScript -->
            <script src="~/Scripts/twitterStream.js" type="text/javascript"></script>
        </head>
        <body onload="initialize()">
            <div class="navbar navbar-inverse navbar-fixed-top">
                <div class="container">
                    <div class="navbar-header">
                        <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                            <span class="icon-bar"></span>
                            <span class="icon-bar"></span>
                            <span class="icon-bar"></span>
                        </button>
                    </div>
                    <div class="navbar-collapse collapse">
                        <div class="row">
                            <ul class="nav navbar-nav col-lg-5">
                                <li class="col-lg-12">
                                    <div class="navbar-form">
                                        <input id="searchbox" type="search" class="form-control">
                                        <button type="button" id="searchBtn" class="btn btn-primary">Go</button>
                                    </div>
                                </li>
                            </ul>
                            <ul class="nav navbar-nav col-lg-7">
                                <li>
                                    <div class="navbar-form">
                                        <div class="btn-group" data-toggle="buttons-radio">
                                            <button type="button" id="positiveBtn" class="btn btn-primary">Positive</button>
                                            <button type="button" id="neutralBtn" class="btn btn-primary">Neutral</button>
                                            <button type="button" id="negativeBtn" class="btn btn-primary">Negative</button>
                                        </div>
                                    </div>
                                </li>
                                <li><span id="statustext" class="navbar-text"></span></li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
            <div class="map_container">
                @RenderBody()
            </div>
            @Scripts.Render("~/bundles/jquery")
            @Scripts.Render("~/bundles/bootstrap")
            @RenderSection("scripts", required: false)
        </body>
        </html>



**Για να τροποποιήσετε το Index.cshtml**

1. Από την **Εξερεύνηση λύσεων**, αναπτύξτε το στοιχείο **TweetSentimentWeb**, ανάπτυξη **προβολές**, ανάπτυξη **για οικιακή χρήση**και, στη συνέχεια, κάντε διπλό κλικ στο **Index.cshtml**.
2. Αντικαταστήστε το περιεχόμενο με τα εξής:

        @{
            ViewBag.Title = "Tweet Sentiment";
        }
        
        <div class="map_container">
            <div id="map_canvas"/>
        </div>

**Για να τροποποιήσετε το αρχείο site.css**

1. Από την **Εξερεύνηση λύσεων**, αναπτύξτε το στοιχείο **TweetSentimentWeb**, ανάπτυξη **περιεχομένου**και, στη συνέχεια, κάντε διπλό κλικ στο **Site.css**.
2. Προσάρτηση τον παρακάτω κώδικα στο αρχείο:
        
        /* make container, and thus map, 100% width */
        .map_container {
            width: 100%;
            height: 100%;
        }
        
        #map_canvas{
          height:100%;
        }
        
        #tweets{
          position: absolute;
          top: 60px;
          left: 75px;
          z-index:1000;
          font-size: 30px;
        }

**Για να τροποποιήσετε το αρχείο global.asax**

1. Από την **Εξερεύνηση λύσεων**, αναπτύξτε **TweetSentimentWeb**και, στη συνέχεια, κάντε διπλό κλικ στο **Global.asax**.
2. Προσθέστε την ακόλουθη πρόταση **χρησιμοποιώντας** :

        using System.Web.Http;

2. Προσθέστε τις ακόλουθες γραμμές μέσα στη συνάρτηση **Application_Start()** :

        // Register API routes
        GlobalConfiguration.Configure(WebApiConfig.Register);
  
    Τροποποιήστε την καταχώρηση από τις διαδρομές API για να κάνετε το Web API ελεγκτή εργασία μέσα στην εφαρμογή MVC.

**Για να εκτελέσετε την εφαρμογή web**

1. Βεβαιωθείτε ότι εφαρμογή κονσόλας της υπηρεσίας ροής εξακολουθεί να εκτελείται, ώστε να μπορείτε να δείτε τις αλλαγές σε πραγματικό χρόνο.
2. Πατήστε το πλήκτρο **F5** για να εκτελέσετε την εφαρμογή web:

    ![hdinsight.hbase.Twitter.sentiment.Bing.Map][img-bing-map]
2. Στο πλαίσιο κειμένου, πληκτρολογήστε μια λέξη-κλειδί και, στη συνέχεια, κάντε κλικ στο κουμπί **Μετάβαση**.  Ανάλογα με τα δεδομένα που συλλέγονται στον πίνακα HBase, ορισμένες λέξεις-κλειδιά δεν μπορεί να βρεθεί. Δοκιμάστε ορισμένες κοινές λέξεις-κλειδιά, όπως "αγαπάτε," "xbox" και "playstation." 
3. Εναλλαγή μεταξύ των **θετικό** **ουδέτερο**και **αρνητικός αριθμός** για να συγκρίνετε άποψη το θέμα.
4. Ενημερώστε την υπηρεσία ροής εκτέλεση για άλλη μία ώρα, και, στη συνέχεια, αναζητήστε τις ίδιες λέξεις-κλειδιά και συγκρίνουν τα αποτελέσματα.

 
Προαιρετικά, μπορείτε να αναπτύξετε την εφαρμογή στις τοποθεσίες Web Azure. Για οδηγίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure τοποθεσίες Web και ASP.NET][website-get-started].
 
## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς μπορείτε να λάβετε tweets, την ανάλυση του άποψη του tweets, αποθηκεύσετε τα δεδομένα άποψη σε HBase και παρουσιάσετε τα δεδομένα σε πραγματικό χρόνο άποψη Twitter στους χάρτες Bing. Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Γρήγορα αποτελέσματα με το HDInsight][hdinsight-get-started]
- [Ρύθμιση παραμέτρων αναπαραγωγής HBase HDInsight](hdinsight-hbase-geo-replication.md) 
- [Ανάλυση δεδομένων Twitter με Hadoop σε HDInsight][hdinsight-analyze-twitter-data]
- [Ανάλυση των δεδομένων καθυστέρηση πτήσεων χρησιμοποιώντας HDInsight][hdinsight-analyze-flight-delay-data]
- [Ανάπτυξη προγραμμάτων Java MapReduce για HDInsight][hdinsight-develop-mapreduce]


[hbase-get-started]: hdinsight-hbase-tutorial-get-started-linux.md
[website-get-started]: ../app-service-web/web-sites-dotnet-get-started.md



[img-app-arch]: ./media/hdinsight-hbase-analyze-twitter-sentiment/AppArchitecture.png
[img-twitter-app]: ./media/hdinsight-hbase-analyze-twitter-sentiment/TwitterApp.png
[img-streaming-service]: ./media/hdinsight-hbase-analyze-twitter-sentiment/StreamingService.png
[img-bing-map]: ./media/hdinsight-hbase-analyze-twitter-sentiment/TwitterSentimentBingMap.png



[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-analyze-twitter-data]: hdinsight-analyze-twitter-data.md




[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
 
