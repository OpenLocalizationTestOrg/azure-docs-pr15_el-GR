<properties
   pageTitle="Τοπολογίες καταιγίδας Apache με το Visual Studio και C# | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε τοπολογίες καταιγίδας στο C#, δημιουργώντας μια τοπολογία count απλό word στο Visual Studio χρησιμοποιώντας τα εργαλεία HDInsight για το Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/27/2016"
   ms.author="larryfr"/>

# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>Ανάπτυξη C# τοπολογίες για Apache καταιγίδας σε HDInsight χρήση Hadoop εργαλείων για το Visual Studio

Μάθετε πώς μπορείτε να δημιουργήσετε μια τοπολογία C# καταιγίδας, χρησιμοποιώντας τα εργαλεία HDInsight για το Visual Studio. Αυτό το πρόγραμμα εκμάθησης σας καθοδηγεί κατά τη διαδικασία δημιουργίας ενός νέου έργου καταιγίδας στο Visual Studio, σκοπούς δοκιμής τοπικά και την ανάπτυξή του σε μια καταιγίδας Apache σε σύμπλεγμα HDInsight.

Επίσης θα μάθετε πώς μπορείτε να δημιουργήσετε τοπολογίες υβριδική που χρησιμοποιούν C# και των στοιχείων Java.

> [AZURE.IMPORTANT] Ενώ τα βήματα σε αυτό το έγγραφο που βασίζονται σε ένα περιβάλλον ανάπτυξης Windows με το Visual Studio, μεταγλωττισμένο έργου μπορούν να υποβληθούν σε σύμπλεγμα μια Linux ή HDInsight που βασίζεται στα Windows. Μόνο βάσει Linux συμπλεγμάτων δημιουργηθεί μετά την υποστήριξη 28/10/2016 τοπολογίες SCP.NET.
>
> Για να χρησιμοποιήσετε μια τοπολογία C# με ένα σύμπλεγμα βάσει Linux, πρέπει να ενημερώσετε το πακέτο Microsoft.SCP.Net.SDK NuGet χρησιμοποιείται από το έργο σας στην έκδοση 0.10.0.6 ή νεότερη έκδοση. Την έκδοση του πακέτου επίσης πρέπει να συμφωνεί με την κύρια έκδοση καταιγίδας εγκατεστημένο στον HDInsight. Για παράδειγμα, καταιγίδας σε εκδόσεις HDInsight 3.3 και 3.4 Χρησιμοποιήστε καταιγίδας έκδοση 0.10.x, ενώ HDInsight 3.5 χρησιμοποιεί καταιγίδας 1.0.x.
> 
> C# τοπολογίες στην βάσει Linux συμπλεγμάτων πρέπει να χρησιμοποιήστε διαίρεσης 4,5 .NET, και μονοφωνικό για να εκτελέσετε στο σύμπλεγμα HDInsight. Τα περισσότερα από όσα θα λειτουργούν, ωστόσο θα πρέπει να ελέγξετε το έγγραφο [Μονοφωνικό συμβατότητας](http://www.mono-project.com/docs/about-mono/compatibility/) για πιθανές ασυμβατότητα.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- Μία από τις παρακάτω εκδόσεις του Visual Studio

    - Visual Studio 2012, με [4 ενημέρωσης](http://www.microsoft.com/download/details.aspx?id=39305)

    - Visual Studio 2013 με [Ενημέρωση 4](http://www.microsoft.com/download/details.aspx?id=44921) ή [Visual Studio 2013 Κοινότητας](http://go.microsoft.com/fwlink/?LinkId=517284)

    - Visual Studio 2015 ή του [Visual Studio 2015 Κοινότητας](https://go.microsoft.com/fwlink/?LinkId=532606)

- Azure SDK 2.9.5 ή νεότερη έκδοση

- Εργαλεία HDInsight για το Visual Studio: ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το HDInsight εργαλεία για το Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) για να εγκαταστήσετε και να ρυθμίσετε τα εργαλεία HDInsight για το Visual Studio.

    > [AZURE.NOTE] Εργαλεία HDInsight για το Visual Studio δεν υποστηρίζονται στο Visual Studio Express

-   Καταιγίδας Apache σε σύμπλεγμα HDInsight: ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το καταιγίδας Apache στην HDInsight](hdinsight-apache-storm-tutorial-get-started.md) για τα βήματα για να δημιουργήσετε ένα σύμπλεγμα.

## <a name="templates"></a>Πρότυπα

Τα εργαλεία HDInsight για το Visual Studio παρέχουν τα παρακάτω πρότυπα::

| Τύπος έργου | Παρουσιάζει |
| ------------ | ------------- |
| Εφαρμογή καταιγίδας | Ένα κενό έργο τοπολογία καταιγίδας |
| Δείγμα Writer καταιγίδας Azure SQL | Πώς να συντάξετε με βάση δεδομένων SQL Azure |
| Δείγμα ανάγνωσης DocumentDB καταιγίδας | Μάθετε πώς να διαβάζετε από Azure DocumentDB |
| Δείγμα DocumentDB Writer καταιγίδας | Πώς να συντάξετε να Azure DocumentDB |
| Δείγμα ανάγνωσης EventHub καταιγίδας | Μάθετε πώς να διαβάζετε από διανομείς συμβάν Azure |
| Δείγμα EventHub Writer καταιγίδας | Πώς να συντάξετε με διανομείς συμβάν Azure |
| Δείγμα ανάγνωσης HBase καταιγίδας | Μάθετε πώς να διαβάζετε από HBase στην HDInsight συμπλεγμάτων |
| Δείγμα HBase Writer καταιγίδας | Πώς να συντάξετε να HBase στην HDInsight συμπλεγμάτων |
| Δείγμα υβριδική καταιγίδας | Πώς μπορείτε να χρησιμοποιήσετε ένα στοιχείο Java |
| Δείγμα καταιγίδας | Μια βασική word τοπολογία count |

> [AZURE.NOTE] Τα δείγματα ανάγνωσης και εγγραφής HBase Χρησιμοποιήστε το REST API HBase για να επικοινωνήσετε με μια HBase στο σύμπλεγμα HDInsight, όχι το API Java HBase.

Στα βήματα σε αυτό το έγγραφο, θα χρησιμοποιήσετε το βασικό τύπο έργου καταιγίδας εφαρμογής για να δημιουργήσετε μια νέα τοπολογία.

## <a name="create-a-c-topology"></a>Δημιουργήσετε μια τοπολογία C#

1. Εάν δεν έχετε ήδη εγκαταστήσει την πιο πρόσφατη έκδοση των εργαλείων HDInsight για το Visual Studio, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το HDInsight εργαλεία για το Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Ανοίξτε το Visual Studio, επιλέξτε **αρχείο** > **Δημιουργία**και, στη συνέχεια, το **Project**.

3. Από την οθόνη **Νέο έργο** , αναπτύξτε το στοιχείο **εγκατεστημένες** > **πρότυπα**και επιλέξτε **HDInsight**. Από τη λίστα των προτύπων, επιλέξτε **Εφαρμογή καταιγίδας**. Στο κάτω μέρος της οθόνης, πληκτρολογήστε **WordCount** ως το όνομα της εφαρμογής.

    ![εικόνα](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. Μετά τη δημιουργία του έργου, θα πρέπει να έχετε τα ακόλουθα αρχεία:

    - **Program.CS**: αυτή καθορίζει την τοπολογία για το έργο σας. Σημειώστε ότι μια προεπιλεγμένη τοπολογία που αποτελείται από ένα στομίου και μία κεραυνό δημιουργείται από προεπιλογή.

    - **Spout.CS**: ένα παράδειγμα στομίου που εκπέμπει τυχαίους αριθμούς.

    - **Bolt.CS**: ένα παράδειγμα κεραυνό που διατηρεί μια Καταμέτρηση αριθμών που εκπέμπει το στομίου.

    Ως μέρος της δημιουργίας έργου, η πιο πρόσφατη [SCP.NET πακέτων](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/) θα λαμβάνονται από NuGet.

    [AZURE.INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

Στις επόμενες ενότητες, θα μπορείτε να τροποποιήσετε αυτό το έργο σε μια βασική εφαρμογή WordCount.

### <a name="implement-the-spout"></a>Υλοποίηση του στομίου

1. Άνοιγμα **Spout.cs**. Spouts χρησιμοποιούνται για την ανάγνωση δεδομένων σε μια τοπολογία από μια εξωτερική προέλευση. Τα κύρια στοιχεία για μια στομίου είναι:

    - **NextTuple**: ονομάζεται από καταιγίδας όταν το στομίου επιτρέπεται να εκπέμπει νέα πλειάδων.

    - **ACK** (μόνο για συναλλαγών τοπολογία): χειρίζεται επιβεβαίωση ξεκίνησε από άλλα στοιχεία στην τοπολογία για πλειάδων αποστέλλεται από αυτό στομίου. Αναγνωρίζοντας μιας πλειάδας επιτρέπει την στομίου γνωρίζετε ότι έγινε επεξεργασία με επιτυχία από ροής στοιχεία.

    - **Αποτυχία** (μόνο για συναλλαγών τοπολογία): χειρίζεται πλειάδων που ΑΠΟΤΥΧΙΑ επεξεργασίας άλλων στοιχείων της τοπολογίας. Αυτό σας παρέχει τη δυνατότητα να εκπέμπει εκ νέου στοιχεία της πλειάδας δεν, έτσι ώστε να μπορεί να είναι επεξεργασία ξανά.

2. Αντικαταστήστε τα περιεχόμενα της κλάσης **Spout** με τα εξής. Αυτό δημιουργεί μια στομίου που τυχαία εκπέμπει μια πρόταση στο της τοπολογίας.

        private Context ctx;
        private Random r = new Random();
        string[] sentences = new string[] {
            "the cow jumped over the moon",
            "an apple a day keeps the doctor away",
            "four score and seven years ago",
            "snow white and the seven dwarfs",
            "i am at two with nature"
        };

        public Spout(Context ctx)
        {
            // Set the instance context
            this.ctx = ctx;

            Context.Logger.Info("Generator constructor called");

            // Declare Output schema
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // The schema for the default output stream is
            // a tuple that contains a string field
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        }

        // Get an instance of the spout
        public static Spout Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Spout(ctx);
        }

        public void NextTuple(Dictionary<string, Object> parms)
        {
            Context.Logger.Info("NextTuple enter");
            // The sentence to be emitted
            string sentence;

            // Get a random sentence
            sentence = sentences[r.Next(0, sentences.Length - 1)];
            Context.Logger.Info("Emit: {0}", sentence);
            // Emit it
            this.ctx.Emit(new Values(sentence));

            Context.Logger.Info("NextTuple exit");
        }

        public void Ack(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }

        public void Fail(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }
    
    Αφιερώστε λίγο χρόνο για να διαβάσετε τα σχόλια για να κατανοήσετε τι σημαίνει αυτό κώδικα.

### <a name="implement-the-bolts"></a>Υλοποίηση τα στοιχεία

1. Διαγράψτε το υπάρχον αρχείο **Bolt.cs** από το έργο.

2. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο και επιλέξτε **Προσθήκη** > **νέο στοιχείο**. Από τη λίστα, επιλέξτε **Καταιγίδας κεραυνό**και πληκτρολογήστε **Splitter.cs** ως το όνομα. Επαναλάβετε αυτήν τη διαδικασία για να δημιουργήσετε μια δεύτερη με καθορισμένη **Counter.cs**.

    - **Splitter.CS**: εφαρμόζει έναν κεραυνό που διαιρείται προτάσεων σε μεμονωμένες λέξεις και εκπέμπει μια νέα ροή των λέξεων.

    - **Counter.CS**: εφαρμόζει έναν κεραυνό που μετράει κάθε λέξη και εκπέμπει μια νέα ροή των λέξεων και το πλήθος για κάθε λέξη.

    > [AZURE.NOTE] Αυτά τα στοιχεία απλώς ανάγνωσης και εγγραφής σε ροές, αλλά μπορείτε επίσης να χρησιμοποιήσετε μια κεραυνό για να επικοινωνήσετε με προελεύσεις, όπως μια βάση δεδομένων ή μια υπηρεσία.

3. Άνοιγμα **Splitter.cs**. Σημειώστε ότι έχει μόνο μία μέθοδο από προεπιλογή: **Εκτέλεση**. Αυτό ονομάζεται όταν η ράβδος λαμβάνει μιας πλειάδας για επεξεργασία. Εδώ, μπορείτε να διαβάσετε και να επεξεργαστεί εισερχόμενης πλειάδων και εκπέμπει εξερχομένων πλειάδων.

4. Αντικαταστήστε τα περιεχόμενα της **διαίρεσης** κλάσης με τον ακόλουθο κώδικα:

        private Context ctx;

        // Constructor
        public Splitter(Context ctx)
        {
            Context.Logger.Info("Splitter constructor called");
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // Input contains a tuple with a string field (the sentence)
            inputSchema.Add("default", new List<Type>() { typeof(string) });
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // Outbound contains a tuple with a string field (the word)
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance of the bolt
        public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Splitter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the sentence from the tuple
            string sentence = tuple.GetString(0);
            // Split at space characters
            foreach (string word in sentence.Split(' '))
            {
                Context.Logger.Info("Emit: {0}", word);
                //Emit each word
                this.ctx.Emit(new Values(word));
            }

            Context.Logger.Info("Execute exit");
        }

    Αφιερώστε λίγο χρόνο για να διαβάσετε τα σχόλια για να κατανοήσετε τι σημαίνει αυτό κώδικα.

5. Ανοίξτε το **Counter.cs** και αντικαταστήστε τα περιεχόμενα τάξης με τα εξής:

        private Context ctx;

        // Dictionary for holding words and counts
        private Dictionary<string, int> counts = new Dictionary<string, int>();

        // Constructor
        public Counter(Context ctx)
        {
            Context.Logger.Info("Counter constructor called");
            // Set instance context
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string field - the word
            inputSchema.Add("default", new List<Type>() { typeof(string) });

            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string and integer field - the word and the word count
            outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance
        public static Counter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Counter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the word from the tuple
            string word = tuple.GetString(0);
            // Do we already have an entry for the word in the dictionary?
            // If no, create one with a count of 0
            int count = counts.ContainsKey(word) ? counts[word] : 0;
            // Increment the count
            count++;
            // Update the count in the dictionary
            counts[word] = count;

            Context.Logger.Info("Emit: {0}, count: {1}", word, count);
            // Emit the word and count information
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
            Context.Logger.Info("Execute exit");
        }

    Αφιερώστε λίγο χρόνο για να διαβάσετε τα σχόλια για να κατανοήσετε τι σημαίνει αυτό κώδικα.

### <a name="define-the-topology"></a>Ορισμός της τοπολογίας

Spouts και βίδες τοποθετούνται σε ένα γράφημα, η οποία καθορίζει τον τρόπο ροής δεδομένων μεταξύ των στοιχείων. Για αυτό τοπολογία, το γράφημα είναι ως εξής:

![Εικόνα του πώς έχουν τακτοποιηθεί στοιχεία](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Προτάσεις έχουν αποσταλεί από το στομίου, που είναι κατανεμημένες παρουσίες η ράβδος διαίρεσης. Η ράβδος διαίρεσης χωρίζει τις προτάσεις σε λέξεις, που είναι κατανεμημένες στο η ράβδος μετρητή.

Επειδή η καταμέτρηση λέξεων παραμένει τοπικά στην παρουσία μετρητή, θέλουμε να βεβαιωθείτε ότι συγκεκριμένες λέξεις ροής για να την ίδια παρουσία κεραυνό μετρητή, ώστε να έχουμε μόνο μία παρουσία παρακολούθηση των μια συγκεκριμένη λέξη. Αλλά για η ράβδος διαίρεσης, πραγματικά δεν έχει σημασία ποια κεραυνό λαμβάνει ποια πρότασης, ώστε να θέλουμε απλώς να φορτώσει προτάσεων υπολοίπου σε αυτές τις περιπτώσεις.

Άνοιγμα **Program.cs**. Η μέθοδος σημαντικό είναι **GetTopologyBuilder**, που χρησιμοποιείται για τον ορισμό της τοπολογίας που υποβάλλεται σε καταιγίδας. Αντικαταστήστε τα περιεχόμενα του **GetTopologyBuilder** με τον ακόλουθο κώδικα για την υλοποίηση της τοπολογίας που περιγράφονται προηγουμένως:

        // Create a new topology named 'WordCount'
        TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

        // Add the spout to the topology.
        // Name the component 'sentences'
        // Name the field that is emitted as 'sentence'
        topologyBuilder.SetSpout(
            "sentences",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
            },
            1);
        // Add the splitter bolt to the topology.
        // Name the component 'splitter'
        // Name the field that is emitted 'word'
        // Use suffleGrouping to distribute incoming tuples
        //   from the 'sentences' spout across instances
        //   of the splitter
        topologyBuilder.SetBolt(
            "splitter",
            Splitter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
            },
            1).shuffleGrouping("sentences");

        // Add the counter bolt to the topology.
        // Name the component 'counter'
        // Name the fields that are emitted 'word' and 'count'
        // Use fieldsGrouping to ensure that tuples are routed
        //   to counter instances based on the contents of field
        //   position 0 (the word). This could also have been
        //   List<string>(){"word"}.
        //   This ensures that the word 'jumped', for example, will always
        //   go to the same instance
        topologyBuilder.SetBolt(
            "counter",
            Counter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
            },
            1).fieldsGrouping("splitter", new List<int>() { 0 });

        // Add topology config
        topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
        {
            {"topology.kryo.register","[\"[B\"]"}
        });

        return topologyBuilder;

Αφιερώστε λίγο χρόνο για να διαβάσετε τα σχόλια για να κατανοήσετε τι σημαίνει αυτό κώδικα.

## <a name="submit-the-topology"></a>Υποβολή της τοπολογίας

1. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο και επιλέξτε **Υποβολή για να καταιγίδας στην HDInsight**.

    > [AZURE.NOTE] Εάν σας ζητηθεί, πληκτρολογήστε τα διαπιστευτήρια σύνδεσης για τη συνδρομή σας στο Azure. Εάν έχετε περισσότερες από μία συνδρομές, συνδεθείτε με αυτό που περιέχει το καταιγίδας σε σύμπλεγμα HDInsight.

2. Επιλέξτε το καταιγίδας σε σύμπλεγμα HDInsight από την αναπτυσσόμενη λίστα **Καταιγίδας σύμπλεγμα** και, στη συνέχεια, επιλέξτε **Υποβολή**. Μπορείτε να παρακολουθείτε εάν την υποβολή είναι επιτυχής, χρησιμοποιώντας το παράθυρο **εξόδου** .

3. Όταν η τοπολογία υποβλήθηκε με επιτυχία, θα πρέπει να εμφανίζεται το **Τοπολογίες καταιγίδας** για το σύμπλεγμα. Επιλέξτε την τοπολογία **WordCount** από τη λίστα για να προβάλετε πληροφορίες σχετικά με την τοπολογία εκτελείται.

    > [AZURE.NOTE] Μπορείτε επίσης να προβάλετε **Τοπολογίες καταιγίδας** από την **Εξερεύνηση διακομιστών**: ανάπτυξη **Azure** > **HDInsight**, κάντε δεξί κλικ σε μια καταιγίδας σε σύμπλεγμα HDInsight και, στη συνέχεια, επιλέξτε **Προβολή καταιγίδας τοπολογίες**.

    Χρησιμοποιήστε τις συνδέσεις για την spouts ή στοιχεία για να προβάλετε πληροφορίες σχετικά με αυτά τα στοιχεία. Θα ανοίξει ένα νέο παράθυρο για κάθε επιλεγμένου στοιχείου.

4. Από την προβολή **Τοπολογία σύνοψη** , κάντε κλικ στο κουμπί **Τερματισμός** για να διακόψετε την τοπολογία.

    > [AZURE.NOTE] Τοπολογίες καταιγίδας συνεχίσει να εκτελείται μέχρι αυτά είναι απενεργοποιημένες, ή να διαγραφεί το σύμπλεγμα.

## <a name="transactional-topology"></a>Συναλλαγές τοπολογίας

Η προηγούμενη τοπολογία είναι μη συναλλαγών. Τα στοιχεία εντός της τοπολογίας υλοποιεί οποιαδήποτε λειτουργικότητα για αναπαραγωγή μηνύματα, εάν η επεξεργασία αποτύχει από ένα στοιχείο της τοπολογίας. Για ένα παράδειγμα συναλλαγών τοπολογία, δημιουργήστε ένα νέο έργο και επιλέξτε **Δείγμα καταιγίδας** ως τον τύπο του έργου.

Συναλλαγές τοπολογίες υλοποίηση τα εξής για να υποστηρίζει την αναπαραγωγή δεδομένων:

- **Προσωρινή αποθήκευση μετα-δεδομένων**: το στομίου πρέπει να αποθηκεύουν μετα-δεδομένα σχετικά με τα δεδομένα που εκπέμπει, έτσι ώστε τα δεδομένα του μπορούν να ανακτηθούν και που εκπέμπει ξανά Εάν προκύψει σφάλμα. Επειδή τα δεδομένα που εκπέμπει το δείγμα είναι μικρό, τα ανεπεξέργαστα δεδομένα για κάθε πλειάδας αποθηκεύονται σε ένα λεξικό για αναπαραγωγή.

- **ACK**: κάθε κεραυνό στην τοπολογία μπορούν να καλέσουν `this.ctx.Ack(tuple)` για επιβεβαίωση ότι έχει με επιτυχία υποβάλλονται σε επεξεργασία μιας πλειάδας. Όταν όλα τα στοιχεία έχουν οπισμένες στοιχεία της πλειάδας δεν, το `Ack` καλείται η μέθοδος από το στομίου. Αυτό σας επιτρέπει την στομίου για να καταργήσετε δεδομένα στο cache για αναπαραγωγή επειδή εντελώς έγινε επεξεργασία των δεδομένων.

- **Αποτυχία**: κάθε κεραυνό μπορούν να καλέσουν `this.ctx.Fail(tuple)` για να υποδείξει ότι η επεξεργασία έχει απέτυχε για μιας πλειάδας. Μεταδίδει την αποτυχία για να το `Fail` μέθοδο το στομίου, όπου στοιχεία της πλειάδας δεν μπορούν να αναπαραχθούν με τη χρήση μετα-δεδομένων στο cache.

- **Αναγνωριστικό ακολουθίας**: όταν εκπομπής μιας πλειάδας, μπορείτε να καθορίσετε ένα Αναγνωριστικό ακολουθίας. Αυτό πρέπει να είναι μια τιμή που προσδιορίζει στοιχεία της πλειάδας δεν για επεξεργασία της αναπαραγωγής (Ack και ΑΠΟΤΥΧΙΑ). Για παράδειγμα, το στομίου του έργου **Καταιγίδας δείγμα** χρησιμοποιεί τα ακόλουθα όταν εκπομπής δεδομένων:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Αυτό εκπέμπει ένα νέο πλειάδας που περιέχει μια πρόταση στη ροή προεπιλεγμένη, με την τιμή Αναγνωριστικό ακολουθίας που περιέχονται σε **lastSeqId**. Για αυτό το παράδειγμα, **lastSeqId** απλώς αυξάνεται για κάθε πλειάδας που εκπέμπει.

Όπως φαίνεται στο **Δείγμα καταιγίδας** έργο, εάν ένα στοιχείο είναι συναλλαγών μπορούν να οριστούν κατά το χρόνο εκτέλεσης, βάσει ρύθμισης παραμέτρων.

## <a name="hybrid-topology"></a>Υβριδική τοπολογίας

Εργαλεία HDInsight για το Visual Studio επίσης μπορούν να χρησιμοποιηθούν για τη δημιουργία υβριδικού τοπολογίες, όπου ορισμένα στοιχεία είναι C# και άλλους είναι Java.

Για μια υβριδική τοπολογία το παράδειγμα, δημιουργήστε ένα νέο έργο και επιλέξτε **Καταιγίδας υβριδική δείγμα**. Με τον τρόπο αυτό δημιουργείται ένα δείγμα πλήρως σχόλια που περιέχει πολλές τοπολογίες που δείχνουν τα εξής:

- **Java spout** και **C# κεραυνό**: ορίζεται στο **HybridTopology_javaSpout_csharpBolt**

    - Μια έκδοση συναλλαγών έχει οριστεί στο **HybridTopologyTx_javaSpout_csharpBolt**

- **C# spout** και **κεραυνό Java**: ορίζεται στο **HybridTopology_csharpSpout_javaBolt**

    - Μια έκδοση συναλλαγών έχει οριστεί στο **HybridTopologyTx_csharpSpout_javaBolt**

        > [AZURE.NOTE] Αυτήν την έκδοση επίσης παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε κωδικό Clojure από ένα αρχείο κειμένου ως ένα στοιχείο Java.

Για να κάνετε εναλλαγή μεταξύ της τοπολογίας που χρησιμοποιείται κατά την υποβολή του έργου, απλώς μετακινήστε το `[Active(true)]` δήλωση στην τοπολογία που θέλετε να χρησιμοποιήσετε πριν να την υποβάλετε για το σύμπλεγμα.

> [AZURE.NOTE] Όλα τα αρχεία Java που απαιτούνται παρέχονται ως μέρος αυτού του έργου στο φάκελο **JavaDependency** .

Λάβετε υπόψη τα παρακάτω κατά τη δημιουργία και υποβολή μιας τοπολογίας υβριδική:

- **JavaComponentConstructor** πρέπει να χρησιμοποιείται για να δημιουργήσετε μια νέα παρουσία της κλάσης Java για στομίου ή κεραυνό.

- πρέπει να χρησιμοποιείται **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** σειριοποίηση δεδομένων σε και έξοδος από το Java στοιχεία από Java αντικειμένων σε JSON.

- Κατά την υποβολή της τοπολογίας στο διακομιστή, πρέπει να χρησιμοποιείτε την επιλογή **πρόσθετες ρυθμίσεις παραμέτρων** για να καθορίσετε τις **διαδρομές Java αρχείων**. Η διαδρομή που καθορίστηκε πρέπει να είναι στον κατάλογο που περιέχει τα αρχεία ΒΆΖΟ που περιέχουν τις σχολικές τάξεις Java.

### <a name="azure-event-hubs"></a>Διανομείς Azure συμβάντος

Έκδοση SCP.Net 0.9.4.203 παρουσιάζει μια νέα κλάση και μέθοδος ειδικά για την εργασία με το συμβάν διανομέα Spout (μια Java στομίου που διαβάζει από συμβάν διανομέα.) Όταν δημιουργείτε μια τοπολογία που χρησιμοποιεί στομίου αυτό, χρησιμοποιήστε τις ακόλουθες μεθόδους:

- Κλάση **EventHubSpoutConfig** : δημιουργεί ένα αντικείμενο που περιλαμβάνει τις παραμέτρους για το στοιχείο στομίου

- Μέθοδος **TopologyBuilder.SetEventHubSpout** : Προσθέτει το στοιχείο Spout διανομέα συμβάντος στην τοπολογία

> [AZURE.NOTE] Ενώ αυτά καθιστούν ευκολότερη την εργασία με το συμβάν Spout διανομέα από άλλα στοιχεία Java, πρέπει να χρησιμοποιήσετε εξακολουθεί να το CustomizedInteropJSONSerializer σειριοποίηση δεδομένων που παράγονται από το στομίου.

## <a id="configurationmanager"></a>Χρήση ConfigurationManager

Μην χρησιμοποιήσετε ConfigurationManager τιμές παραμέτρων ανάκτηση από κεραυνό και spout στοιχεία. Αυτό θα οδηγήσει σε μια εξαίρεση null δείκτη. Αντί για αυτό, η ρύθμιση παραμέτρων για το έργο σας μεταβιβάζεται σε της τοπολογίας καταιγίδας ως ένα ζεύγος κλειδιού/τιμής στο περιβάλλον τοπολογίας. Κάθε στοιχείο που βασίζεται σε τιμές παραμέτρων πρέπει να τις ανακτήσετε από το περιβάλλον κατά την προετοιμασία.

Ο ακόλουθος κώδικας δείχνει τον τρόπο για να ανακτήσετε αυτές τις τιμές:

    public class MyComponent : ISCPBolt
    {
        // To hold configuration information loaded from context
        Configuration configuration;
        ...
        public MyComponent(Context ctx, Dictionary<string, Object> parms)
        {
            // Save a copy of the context for this component instance
            this.ctx = ctx;
            // If it exists, load the configuration for the component
            if(parms.ContainsKey(Constants.USER_CONFIG))
            {
                this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
            }
            // Retrieve the value of "Foo" from configuration
            var foo = this.configuration.AppSettings.Settings["Foo"].Value;
        }
        ...
    }

Εάν χρησιμοποιείτε μια `Get` μέθοδο για να επιστρέψει μια περίοδο λειτουργίας του στοιχείου, θα πρέπει να βεβαιωθείτε ότι και τα δύο περάσει το `Context` και `Dictionary<string, Object>` παράμετροι στην κατασκευή. Το ακόλουθο παράδειγμα είναι μια βασική `Get` μέθοδο που μεταφέρει σωστά αυτές τις τιμές:

    public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new MyComponent(ctx, parms);
    }

## <a name="how-to-update-scpnet"></a>Πώς μπορείτε να ενημερώσετε SCP.NET

Πρόσφατες εκδόσεις των SCP.NET υποστήριξης του πακέτου αναβάθμισης μέσω NuGet. Όταν μια νέα ενημερωμένη έκδοση είναι διαθέσιμη, θα λάβετε μια ειδοποίηση αναβάθμισης. Για να ελέγξετε με μη αυτόματο τρόπο μια αναβάθμιση, εκτελέστε τα εξής βήματα:

1. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο και επιλέξτε **Διαχείριση πακέτων NuGet**.

2. Από τη Διαχείριση πακέτου, επιλέξτε **ενημερώσεις**. Εάν μια ενημερωμένη έκδοση είναι διαθέσιμη, θα εμφανίζεται. Κάντε κλικ στο κουμπί **Ενημέρωση** για το πακέτο για να την εγκαταστήσετε.

> [AZURE.IMPORTANT] Εάν το έργο σας έχει δημιουργηθεί με μία από τις προηγούμενες εκδόσεις του SCP.NET που δεν χρησιμοποιήσατε NuGet για ενημερώσεις του πακέτου, πρέπει να εκτελέσετε τα παρακάτω βήματα για να ενημερώσετε τη νέα έκδοση:
>
> 1. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο και επιλέξτε **Διαχείριση πακέτων NuGet**.
> 2. Χρήση του πεδίου **αναζήτησης** , αναζήτηση και, στη συνέχεια, να προσθέσετε, **Microsoft.SCP.Net.SDK** στο έργο.

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

### <a name="null-pointer-exceptions"></a>Εξαιρέσεις null δείκτη

Όταν χρησιμοποιείτε μια τοπολογία C# με ένα σύμπλεγμα βάσει Linux HDInsight, βίδας και spout στοιχεία που χρησιμοποιούν ConfigurationManager για να διαβάσετε ρυθμίσεις παραμέτρων κατά το χρόνο εκτέλεσης ενδέχεται να επιστρέψει null δείκτη εξαιρέσεις. Αυτό συμβαίνει επειδή δεν είναι η ρύθμιση παραμέτρων για τη φόρτωση τομέα από το σύνολο που περιέχει το έργο σας.

Η ρύθμιση παραμέτρων για το έργο σας μεταβιβάζεται σε της τοπολογίας καταιγίδας ως ένα ζεύγος κλειδιού/τιμής στο περιβάλλον τοπολογία και μπορεί να ανακτηθεί από το αντικείμενο λεξικό που μεταβιβάζεται στα στοιχεία σας όταν έχουν αρχικές τιμές.

Το παρακάτω παράδειγμα δείχνει τη φόρτωση τις τιμές παραμέτρων από το περιβάλλον τοπολογία, ανατρέξτε στην ενότητα [ConfigurationManager](#configurationmanager) αυτού του εγγράφου.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Όταν χρησιμοποιείτε μια τοπολογία C# με ένα σύμπλεγμα βάσει Linux HDInsight, που μπορεί να αντιμετωπίσετε το ακόλουθο σφάλμα:

    System.TypeLoadException: Failure has occurred while loading a type.

Αυτό συνήθως occurrs όταν χρησιμοποιείτε ένα δυαδικό αρχείο που δεν είναι συμβατή με την έκδοση του .NET που υποστηρίζει μονοφωνικό.

Για συμπλεγμάτων βάσει Linux HDInsight, που θα πρέπει να βεβαιωθείτε ότι το έργο σας χρησιμοποιεί στατιστικές για .NET διαίρεσης 4,5 δυαδικά δεδομένα.


### <a name="test-a-topology-locally"></a>Δοκιμάστε μια τοπολογία τοπικά

Παρόλο που είναι εύκολο να αναπτύξετε μια τοπολογία σε ένα σύμπλεγμα, σε ορισμένες περιπτώσεις, ίσως χρειαστεί να δοκιμάσετε μια τοπολογία τοπικά. Χρησιμοποιήστε τα ακόλουθα βήματα για να εκτελέσετε και δοκιμή της τοπολογίας παράδειγμα σε αυτό το πρόγραμμα εκμάθησης τοπικά στο περιβάλλον ανάπτυξής σας.

> [AZURE.WARNING] Τοπική δοκιμές λειτουργεί μόνο για βασικές, μόνο τοπολογίες C#. Δεν πρέπει να χρησιμοποιείτε τοπικό έλεγχο για υβριδική τοπολογίες ή τοπολογίες που χρησιμοποιούν πολλών ροών, όπως θα εμφανιστούν σφάλματα.

1. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο και επιλέξτε **Ιδιότητες**. Στις ιδιότητες του έργου, αλλάξτε τον **τύπο εξόδου** σε **Εφαρμογή κονσόλας**.

    ![Τύπος εξόδου](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] Να θυμάστε ότι για να αλλάξετε τον **τύπο εξόδου** **Βιβλιοθήκη κλάσεων** πριν από την ανάπτυξη της τοπολογίας σε ένα σύμπλεγμα.

2. Στην **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο, στη συνέχεια, επιλέξτε **Προσθήκη** > **Νέο στοιχείο**. Επιλέξτε **τάξης** και πληκτρολογήστε **LocalTest.cs** ως το όνομα της κλάσης. Τέλος, κάντε κλικ στην επιλογή **Προσθήκη**.

3. Ανοίξτε **LocalTest.cs** και προσθέστε την ακόλουθη πρόταση **Χρήση** στο επάνω μέρος:

        using Microsoft.SCP;

4. Χρησιμοποιήστε τα παρακάτω ανάλογα με τα περιεχόμενα της κλάσης **LocalTest** :

        // Drives the topology components
        public void RunTestCase()
        {
            // An empty dictionary for use when creating components
            Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

            #region Test the spout
            {
                Console.WriteLine("Starting spout");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext spoutCtx = LocalContext.Get();
                // Get a new instance of the spout, using the local context
                Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

                // Emit 10 tuples
                for (int i = 0; i < 10; i++)
                {
                    sentences.NextTuple(emptyDictionary);
                }
                // Use LocalContext to persist the data stream to file
                spoutCtx.WriteMsgQueueToFile("sentences.txt");
                Console.WriteLine("Spout finished");
            }
            #endregion

            #region Test the splitter bolt
            {
                Console.WriteLine("Starting splitter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext splitterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


                // Set the data stream to the data created by the spout
                splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    splitter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                splitterCtx.WriteMsgQueueToFile("splitter.txt");
                Console.WriteLine("Splitter bolt finished");
            }
            #endregion

            #region Test the counter bolt
            {
                Console.WriteLine("Starting counter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext counterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Counter counter = Counter.Get(counterCtx, emptyDictionary);

                // Set the data stream to the data created by splitter bolt
                counterCtx.ReadFromFileToMsgQueue("splitter.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    counter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                counterCtx.WriteMsgQueueToFile("counter.txt");
                Console.WriteLine("Counter bolt finished");
            }
            #endregion
        }

    Αφιερώστε λίγο χρόνο για να διαβάσετε τα σχόλια κώδικα. Αυτός ο κωδικός χρησιμοποιεί **LocalContext** για να εκτελέσετε τα στοιχεία στο περιβάλλον ανάπτυξης και παραμένει στη ροή δεδομένων μεταξύ των στοιχείων σε αρχεία κειμένου στον τοπικό δίσκο.

5. Ανοίξτε **Program.cs** και προσθέστε τα εξής στην **κύρια** μέθοδο:

        Console.WriteLine("Starting tests");
        System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
        // Initialize the runtime
        SCPRuntime.Initialize();

        //If we are not running under the local context, throw an error
        if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
        {
            throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
        }
        // Create test instance
        LocalTest tests = new LocalTest();
        // Run tests
        tests.RunTestCase();
        Console.WriteLine("Tests finished");
        Console.ReadKey();

6. Αποθηκεύστε τις αλλαγές, στη συνέχεια, κάντε κλικ στην επιλογή **F5** ή επιλέξτε **Εντοπισμός σφαλμάτων** > **Ξεκινήστε τον εντοπισμό σφαλμάτων** για να ξεκινήσετε το έργο. Ένα παράθυρο κονσόλας θα πρέπει να εμφανίζονται και κατάσταση καταγραφής ως την πρόοδο δοκιμές. Όταν **ολοκληρωθεί δοκιμών που** εμφανίζεται, πατήστε οποιοδήποτε πλήκτρο για να κλείσετε το παράθυρο.

7. Χρησιμοποιήστε την **Εξερεύνηση των Windows** για να εντοπίσετε τον κατάλογο που περιέχει το έργο σας, για παράδειγμα, **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. Σε αυτόν τον κατάλογο, ανοίξτε **Ανακύκλωσης**και, στη συνέχεια, κάντε κλικ στην επιλογή **Εντοπισμός σφαλμάτων**. Θα πρέπει να δείτε τα αρχεία κειμένου που έχουν παραχθεί κατά την εκτέλεση των δοκιμών: sentences.txt, counter.txt και splitter.txt. Ανοίξτε κάθε αρχείο κειμένου και να ελέγξετε τα δεδομένα.

    > [AZURE.NOTE] Δεδομένα συμβολοσειράς είναι σταθερές ως πίνακας δεκαδικό τιμών σε αυτά τα αρχεία. Για παράδειγμα, \[[97,103,111]] στο το **splitter.txt** αρχείου είναι η λέξη "και".

Παρόλο που δοκιμές μια βασική λέξη μέτρηση τοπικά η εφαρμογή είναι αρκετά trivial, η πραγματική τιμή προέρχεται όταν έχετε μια σύνθετη τοπολογία που επικοινωνεί με εξωτερικές προελεύσεις δεδομένων ή εκτελεί σύνθετης ανάλυσης δεδομένων. Όταν εργάζεστε σε όπως ένα έργο, ίσως χρειαστεί να ορίσετε σημεία διακοπής και βήμα μέσω του κώδικα στο τα στοιχεία σας να απομονώσετε ζητήματα.

> [AZURE.NOTE] Φροντίστε να ορίσετε τον **τύπο του έργου** πίσω στη **Βιβλιοθήκη κλάσης** πριν από την ανάπτυξη στον μιας καταιγίδας σε σύμπλεγμα HDInsight.

### <a name="log-information"></a>Πληροφορίες αρχείου καταγραφής

Μπορείτε εύκολα να καταγράψετε πληροφορίες από τα στοιχεία τοπολογία σας με τη χρήση `Context.Logger`. Για παράδειγμα, το ακόλουθο θα δημιουργήσει μια καταχώρηση ενημερωτικό αρχείο καταγραφής:

    Context.Logger.Info("Component started");

Πληροφορίες που καταγράφονται μπορούν να προβληθούν από το **Αρχείο καταγραφής της υπηρεσίας Hadoop**, που βρίσκεται στην **Εξερεύνηση Server**. Αναπτύξτε την καταχώρηση για το καταιγίδας σε σύμπλεγμα HDInsight και, στη συνέχεια, αναπτύξτε το στοιχείο **Αρχείο καταγραφής της υπηρεσίας Hadoop**. Τέλος, επιλέξτε το αρχείο καταγραφής για την προβολή.

> [AZURE.NOTE] Τα αρχεία καταγραφής αποθηκεύονται στο χώρο αποθήκευσης Azure λογαριασμό που χρησιμοποιείται από το σύμπλεγμά σας. Εάν αυτή είναι μια διαφορετική συνδρομή από την είστε συνδεδεμένοι με το Visual Studio, πρέπει να συνδεθείτε με τη συνδρομή που περιέχει το λογαριασμό χώρου αποθήκευσης για να δείτε αυτές τις πληροφορίες.

### <a name="view-error-information"></a>Προβολή πληροφοριών σφάλματος

Για να δείτε τα σφάλματα που έχουν προκύψει σε μια τοπολογία εκτελείται, χρησιμοποιήστε τα παρακάτω βήματα:

1. Από την **Εξερεύνηση Server**, κάντε δεξί κλικ το καταιγίδας στο σύμπλεγμα HDInsight και επιλέξτε **Προβολή καταιγίδας τοπολογίες**.

2. Το **Spout** και **μπουλονιών**, η στήλη **Τελευταία σφάλματος** θα έχει πληροφορίες σχετικά με το τελευταίο σφάλμα που παρουσιάστηκε.

3. Επιλέξτε το **Αναγνωριστικό Spout** ή το **Αναγνωριστικό κεραυνό** για το στοιχείο που έχει σφάλμα που παρατίθενται. Στη σελίδα λεπτομέρειες που εμφανίζεται, θα εμφανίζονται πρόσθετες πληροφορίες σφάλματος στην ενότητα " **σφάλματα** " στο κάτω μέρος της σελίδας.

4. Για να λάβετε περισσότερες πληροφορίες, επιλέξτε μια **θύρα** από την ενότητα **Executors** της σελίδας για να δείτε το αρχείο καταγραφής εργαζόμενου καταιγίδας για τα τελευταία λίγα λεπτά.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε πώς μπορείτε να αναπτύξετε και ανάπτυξη τοπολογίες καταιγίδας από τα εργαλεία HDInsight για το Visual Studio, θα μάθετε πώς να [διαδικασία συμβάντα από διανομέα συμβάν Azure με καταιγίδας στην HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).

Για παράδειγμα μια τοπολογία C# που διαχωρίζει ροή δεδομένων σε πολλών ροών, ανατρέξτε στο θέμα [παράδειγμα καταιγίδας C#](https://github.com/Blackmist/csharp-storm-example).

Για να ανακαλύψετε περισσότερες πληροφορίες σχετικά με τη δημιουργία C# τοπολογίες, επισκεφθείτε [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Για περισσότερους τρόπους για να εργαστείτε με HDInsight και περισσότερες καταιγίδας σε δείγματα HDinsight, ανατρέξτε στα παρακάτω:

**Microsoft SCP.NET**

* [Οδηγός προγραμματισμού SCP](hdinsight-storm-scp-programming-guide.md)

**Καταιγίδας Apache στην HDInsight**

- [Ανάπτυξη και εποπτεία τοπολογίες με καταιγίδας Apache στην HDInsight](hdinsight-storm-deploy-monitor-topology.md)

- [Παράδειγμα τοπολογίες για καταιγίδας στην HDInsight](hdinsight-storm-example-topology.md)

**Apache Hadoop σε HDInsight**

- [Χρήση της ομάδας με Hadoop σε HDInsight](hdinsight-use-hive.md)

- [Χρήση γουρούνι με Hadoop σε HDInsight](hdinsight-use-pig.md)

- [Χρήση MapReduce με Hadoop σε HDInsight](hdinsight-use-mapreduce.md)

**Apache HBase σε HDInsight**

- [Γρήγορα αποτελέσματα με το HBase στην HDInsight](hdinsight-hbase-tutorial-get-started.md)
