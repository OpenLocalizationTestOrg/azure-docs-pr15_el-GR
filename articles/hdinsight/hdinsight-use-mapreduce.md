<properties
   pageTitle="MapReduce με Hadoop σε HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να εκτελέσετε εργασίες MapReduce σε Hadoop στο συμπλεγμάτων HDInsight. Θα εκτελείτε μια λειτουργία count βασικές word υλοποιηθεί ως εργασία Java MapReduce."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Χρήση MapReduce σε Hadoop σε HDInsight

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να εκτελέσετε εργασίες MapReduce σε Hadoop σε συμπλεγμάτων HDInsight. Μπορούμε να εκτελέσουμε μια λειτουργία count βασικές word υλοποιηθεί ως εργασία Java MapReduce.

##<a id="whatis"></a>Τι είναι το MapReduce;

Hadoop MapReduce είναι ένα πλαίσιο λογισμικού για τη σύνταξη εργασίες που επεξεργάζονται τεράστιες ποσότητες δεδομένων. Δεδομένα εισόδου χωρίζεται σε ανεξάρτητους μπλοκ, η οποία, στη συνέχεια, υποβάλλονται σε επεξεργασία παράλληλα κατά μήκος τους κόμβους του συμπλέγματος. Μια εργασία MapReduce αποτελείται από δύο λειτουργίες:

* **Πρόγραμμα αντιστοίχισης**: χρησιμοποιεί δεδομένα εισόδου και αναλύει το (συνήθως με το φιλτράρισμα και ταξινόμηση λειτουργίες) εκπέμπει πλειάδων (ζεύγη κλειδιού-τιμής)
* **Reducer**: καταναλώνει πλειάδων εκπέμπει το πρόγραμμα αντιστοίχισης και εκτελεί μια λειτουργία σύνοψης που δημιουργεί ένα αποτέλεσμα μικρότερο, συνδυασμένα από τα δεδομένα αντιστοίχισης

Ένα παράδειγμα βασικές word count MapReduce εργασία απεικονίζεται στο ακόλουθο διάγραμμα:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

Το αποτέλεσμα αυτής της εργασίας είναι μια καταμέτρηση του πόσες φορές κάθε λέξη προέκυψε το κείμενο που έχει αναλυθεί.

* Το πρόγραμμα αντιστοίχισης λαμβάνει κάθε γραμμή από το κείμενο εισαγωγής ως εισαγωγή και χωρίζει το σε λέξεις. Το εκπέμπει ένα κλειδιού/τιμής ζεύγος κάθε φορά που παρουσιάζεται μια λέξη της λέξης ακολουθείται από τον αριθμό 1. Το αποτέλεσμα έχει ταξινομηθεί πριν από την αποστολή σε reducer.

* Το reducer αθροίζει αυτές τις μεμονωμένες υπολογίζεται για κάθε λέξη και εκπέμπει ένα ζεύγος μία κλειδιού/τιμής που περιέχει τη λέξη ακολουθούμενο από το άθροισμα των εμφανίσεων του.

MapReduce μπορεί να υλοποιηθεί σε διάφορες γλώσσες. Java είναι τα πιο συνηθισμένα υλοποίηση και χρησιμοποιείται για σκοπούς επίδειξης σε αυτό το έγγραφο.

### <a name="hadoop-streaming"></a>Hadoop ροής

Γλώσσες ή πλαισίων που βασίζονται σε Java και η εικονική μηχανή Java (για παράδειγμα, Scalding ή επικάλυψη,) μπορεί να είναι εκτελέσατε απευθείας ως MapReduce εργασία, παρόμοια με μια εφαρμογή Java. Άλλα άτομα, όπως C# ή Python ή εκτελέσιμα μεμονωμένη, πρέπει να χρησιμοποιήσετε Hadoop ροής.

Ροή Hadoop επικοινωνεί με το πρόγραμμα αντιστοίχισης και reducer μέσω STDIN και STDOUT - το αντιστοίχισης reducer ανάγνωση δεδομένων μια γραμμή τη φορά από το STDIN και γράψτε το αποτέλεσμα στο STDOUT. Κάθε γραμμή ανάγνωση ή που εκπέμπει από το πρόγραμμα αντιστοίχισης και reducer πρέπει να είναι στη μορφή από ένα ζεύγος κλειδιού/τιμής, διαχωρισμένες με μια καρτέλα charaacter:

    [key]/t[value]

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ροή Hadoop](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Για παραδείγματα χρήσης Hadoop ροής με το HDInsight, ανατρέξτε στα εξής:

* [Ανάπτυξη Python MapReduce εργασίες](hdinsight-hadoop-streaming-python.md)

##<a id="data"></a>Σχετικά με το δείγμα δεδομένων

Σε αυτό το παράδειγμα, για το δείγμα δεδομένων, θα μπορείτε να χρησιμοποιήσετε τα σημειωματάρια του Leonardo Da Vinci, που παρέχονται ως έγγραφο κειμένου στο σύμπλεγμά σας HDInsight.

Το δείγμα δεδομένων είναι αποθηκευμένη στο χώρο αποθήκευσης αντικειμένων Blob του Azure, που χρησιμοποιεί HDInsight ως το προεπιλεγμένο σύστημα αρχείων Hadoop συμπλεγμάτων. HDInsight να αποκτήσετε πρόσβαση σε αρχεία που είναι αποθηκευμένα στο χώρο αποθήκευσης αντικειμένων Blob, χρησιμοποιώντας το πρόθεμα **wasb** . Για παράδειγμα, για να αποκτήσετε πρόσβαση στο αρχείο sample.log, πρέπει να χρησιμοποιήσετε την παρακάτω σύνταξη:

    wasbs:///example/data/gutenberg/davinci.txt

Επειδή το χώρο αποθήκευσης αντικειμένων Blob του Azure είναι το προεπιλεγμένο αποθήκευσης για HDInsight, μπορείτε επίσης να αποκτήσετε πρόσβαση στο αρχείο με τη χρήση **/example/data/gutenberg/davinci.txt**.

> [AZURE.NOTE] Στην προηγούμενη σύνταξη, **wasbs: / / /** χρησιμοποιείται για να αποκτήσετε πρόσβαση σε αρχεία που είναι αποθηκευμένα στο προεπιλεγμένο κοντέινερ χώρου αποθήκευσης για το σύμπλεγμα HDInsight. Αν έχετε καθορίσει τους λογαριασμούς επιπλέον χώρου αποθήκευσης όταν παρασχεθεί το σύμπλεγμά σας και θέλετε να αποκτήσετε πρόσβαση σε αρχεία που είναι αποθηκευμένα σε αυτούς τους λογαριασμούς, μπορείτε να αποκτήσετε πρόσβαση στα δεδομένα, καθορίζοντας τη διεύθυνση λογαριασμού κοντέινερ όνομα και χώρου αποθήκευσης. Για παράδειγμα, **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**.

##<a id="job"></a>Πληροφορίες σχετικά με το παράδειγμα MapReduce

Η εργασία MapReduce που χρησιμοποιείται σε αυτό το παράδειγμα βρίσκεται στην **wasbs://example/jars/hadoop-mapreduce-examples.jar**και παρέχεται με το σύμπλεγμά σας HDInsight. Αυτή η ενότητα περιέχει ένα παράδειγμα count word που που θα εκτελεστεί σε **davinci.txt**.

> [AZURE.NOTE] Στην συμπλεγμάτων HDInsight 2.1, τη θέση του αρχείου είναι **wasbs:///example/jars/hadoop-examples.jar**.

Για αναφορά, ακολουθεί τον κώδικα Java για το word count MapReduce έργο:

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

Για οδηγίες για να γράψετε τη δική σας εργασία MapReduce, ανατρέξτε στο θέμα [Ανάπτυξη MapReduce Java προγράμματα για HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md).

##<a id="run"></a>Εκτελέστε το MapReduce

HDInsight να εκτελέσετε εργασίες HiveQL, χρησιμοποιώντας διάφορες μεθόδους. Χρησιμοποιήστε τον παρακάτω πίνακα για να αποφασίσετε ποια μέθοδο είναι κατάλληλη για εσάς και, στη συνέχεια, ακολουθήστε τη σύνδεση για αναλυτικές οδηγίες.

| **Χρησιμοποιήστε αυτήν την επιλογή**...                                                    | **.. .για να κάνετε το εξής**                                       | .. .with αυτό το **σύμπλεγμα λειτουργικό σύστημα** | .. .from αυτό το **πρόγραμμα-πελάτη λειτουργικό σύστημα** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | Χρησιμοποιήστε την εντολή Hadoop μέσω **SSH**                  | Linux                                     | Linux, Unix, Mac OS X ή των Windows        |
| [Καμπύλη](hdinsight-hadoop-use-mapreduce-curl.md)                     | Υποβάλετε την εργασία από απόσταση χρησιμοποιώντας το **ΥΠΌΛΟΙΠΟ**               | Linux ή στα Windows                          | Linux, Unix, Mac OS X ή των Windows        |
| [Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) | Υποβάλετε την εργασία από απόσταση με χρήση του **Windows PowerShell** | Linux ή στα Windows                          | Windows                                  |
| [Σύνδεση απομακρυσμένης επιφάνειας εργασίας](hdinsight-hadoop-use-mapreduce-remote-desktop)    | Χρησιμοποιήστε την εντολή Hadoop **Απομακρυσμένης** επιφάνειας εργασίας       | Windows                                   | Windows                                  |

##<a id="nextsteps"></a>Επόμενα βήματα

Παρόλο που MapReduce παρέχει ισχυρές δυνατότητες διαγνωστικών, μπορεί να είναι λίγο ενδιαφέρουσα υπόδειγμα. Υπάρχουν πολλά πλαίσια βάσει Java που σας διευκολύνουν να ορίσετε MapReduce εφαρμογές, καθώς και τεχνολογίες, όπως γουρούνι και ομάδα, που παρέχουν μια πιο εύκολο τρόπο για να εργαστείτε με δεδομένα σε HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

* [Ανάπτυξη προγραμμάτων Java MapReduce για HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Ανάπτυξη Python ροής προγράμματα MapReduce για HDInsight](hdinsight-hadoop-streaming-python.md)

* [Ανάπτυξη εγκαύματα MapReduce έργα με Apache Hadoop σε HDInsight](hdinsight-hadoop-mapreduce-scalding.md)

* [Χρήση της ομάδας με το HDInsight][hdinsight-use-hive]

* [Χρήση γουρούνι με HDInsight][hdinsight-use-pig]

* [Εκτελέστε τα δείγματα HDInsight][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
