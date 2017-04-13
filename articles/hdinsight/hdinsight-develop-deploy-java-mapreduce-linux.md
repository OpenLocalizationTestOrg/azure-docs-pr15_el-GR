<properties
    pageTitle="Ανάπτυξη προγραμμάτων Java MapReduce για βάσει Linux HDInsight | Microsoft Azure"
    description="Μάθετε τον τρόπο για την ανάπτυξη προγραμμάτων Java MapReduce και ανάπτυξη βάσει Linux HDInsight."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>Ανάπτυξη προγραμμάτων Java MapReduce για Hadoop σε HDInsight Linux

Αυτή η έγγραφα σάς καθοδηγεί σε χρήση Apache Maven για τη δημιουργία εφαρμογής MapReduce, στη συνέχεια, να αναπτύξετε και να εκτελέσετε το σε Hadoop Linux βασίζεται σε σύμπλεγμα HDInsight.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 ή νεότερη έκδοση (ή μια αντίστοιχη, όπως OpenJDK)

- [Apache Maven](http://maven.apache.org/)

- **Μια συνδρομή του Azure**

- **Azure CLI**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>Ρύθμιση παραμέτρων μεταβλητές περιβάλλοντος

Οι παρακάτω μεταβλητές περιβάλλοντος μπορεί να οριστεί κατά την εγκατάσταση του Java και το JDK. Ωστόσο, θα πρέπει να ελέγξετε ότι υπάρχει και ότι περιέχουν τις σωστές τιμές για το σύστημά σας.

* **JAVA_HOME** - πρέπει να κατευθύνουν στον κατάλογο όπου είναι εγκατεστημένο το περιβάλλον χρόνου εκτέλεσης Java (JRE). Για παράδειγμα, σε ένα σύστημα OS X, Unix ή Linux, θα πρέπει να έχει μια τιμή που είναι παρόμοια με `/usr/lib/jvm/java-7-oracle`. Στα Windows, αυτό θα έχει μια τιμή που είναι παρόμοια με`c:\Program Files (x86)\Java\jre1.7`

* **ΔΙΑΔΡΟΜΉ** - πρέπει να περιλαμβάνει τις ακόλουθες διαδρομές:

    * **JAVA_HOME** (ή η ισοδύναμη διαδρομή)

    * **JAVA_HOME\bin** (ή η ισοδύναμη διαδρομή)

    * Στον κατάλογο όπου είναι εγκατεστημένο το Maven

##<a name="create-a-new-maven-project"></a>Δημιουργήστε ένα νέο έργο Maven

1. Από μια περίοδο λειτουργίας τερματικού ή γραμμή εντολών στο περιβάλλον ανάπτυξής σας, αλλάξτε σε καταλόγους στη θέση που θέλετε να αποθηκεύσετε αυτό το έργο.

3. Χρησιμοποιήστε την εντολή __mvn__ , η οποία έχει εγκατασταθεί με το Maven, για να δημιουργήσετε το ικρίωμα για το έργο.

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Αυτό θα δημιουργήσει έναν νέο κατάλογο του τρέχοντος καταλόγου, με το όνομα που καθορίζεται από την παράμετρο __artifactID__ (**wordcountjava** σε αυτό το παράδειγμα). Αυτός ο κατάλογος θα περιέχει τα ακόλουθα στοιχεία:

    * __pom.xml__ - το [Μοντέλο αντικειμένου έργου (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) που περιέχει πληροφορίες και τη ρύθμιση παραμέτρων λεπτομέρειες που χρησιμοποιούνται για τη δημιουργία του έργου.

    * __src__ - τον κατάλογο που περιέχει τον κατάλογο __java/κύριες/οργανισμού/hadoop/apache/παραδείγματα__ , όπου θα συντάσσετε την εφαρμογή.

3. Διαγράψτε το αρχείο __src/test/java/org/apache/hadoop/examples/apptest.java__ , καθώς αυτό δεν θα χρησιμοποιηθεί σε αυτό το παράδειγμα.

##<a name="add-dependencies"></a>Προσθήκη εξαρτήσεων

1. Επεξεργαστείτε το αρχείο __pom.xml__ και προσθέστε τα ακόλουθα εντός του `<dependencies>` την ενότητα:

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    Αυτό σας ενημερώνει για Maven ότι το έργο απαιτεί τις βιβλιοθήκες (που παρατίθενται σε &lt;artifactId\>) με μια συγκεκριμένη έκδοση (που παρατίθενται σε &lt;έκδοση\>). Κατά τη μεταγλώττιση, αυτό θα λαμβάνονται από το χώρο αποθήκευσης Maven προεπιλογή. Μπορείτε να χρησιμοποιήσετε την [Αναζήτηση Maven αποθετήριο δεδομένων](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) για να προβάλετε περισσότερα.

    Το `<scope>provided</scope>` σας ενημερώνει για Maven ότι δεν θα πρέπει να συσκευαστούν αυτές τις εξαρτήσεις με την εφαρμογή, όπως θα παρέχονται από το HDInsight σύμπλεγμα κατά το χρόνο εκτέλεσης.

2. Προσθέστε τα εξής στο αρχείο __pom.xml__ . Πρέπει να είναι εντός του `<project>...</project>` ετικέτες στο αρχείο. Για παράδειγμα, μεταξύ `</dependencies>` και `</project>`.

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                    <goals>
                      <goal>shade</goal>
                    </goals>
                </execution>
              </executions>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    Η πρώτη Προσθήκη ρυθμίζει τις παραμέτρους της [Προσθήκης σκίαση Maven](http://maven.apache.org/plugins/maven-shade-plugin/), που χρησιμοποιείται για τη δημιουργία ενός uberjar (μερικές φορές ονομάζεται μια fatjar) που περιέχει εξαρτήσεις που απαιτούνται από την εφαρμογή. Το επίσης αποτρέπει την αναπαραγωγή των αδειών χρήσης μέσα σε το πακέτο βάζο, η οποία μπορεί να προκαλέσει προβλήματα σε ορισμένα συστήματα.

    Η δεύτερη προσθήκη ρυθμίζει το πρόγραμμα μεταγλώττισης Maven, η οποία χρησιμοποιείται για να ορίσετε την έκδοση του Java που απαιτούνται από την έκδοση που χρησιμοποιείται στο σύμπλεγμα HDInsight αυτής της εφαρμογής.

3. Αποθηκεύστε το αρχείο __pom.xml__ .

##<a name="create-the-mapreduce-application"></a>Δημιουργία της εφαρμογής MapReduce

1. Μεταβείτε στον κατάλογο __wordcountjava/src/κύριες/java/οργανισμού/apache/hadoop/παραδείγματα__ και να μετονομάσετε το αρχείο __App.java__ σε __WordCount.java__.

2. Ανοίξτε το αρχείο __WordCount.java__ σε έναν επεξεργαστή κειμένου και αντικαταστήστε τα περιεχόμενα με τα εξής:

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

    Παρατηρήστε το όνομα του πακέτου είναι **org.apache.hadoop.examples** και το όνομα της κλάσης **WordCount**. Θα μπορείτε να χρησιμοποιήσετε αυτά τα ονόματα κατά την υποβολή της εργασίας MapReduce.

3. Αποθηκεύστε το αρχείο.

##<a name="build-the-application"></a>Δημιουργία της εφαρμογής

1. Μεταβείτε στον κατάλογο __wordcountjava__ , εάν δεν είστε ήδη εκεί.

2. Χρησιμοποιήστε την ακόλουθη εντολή για να δημιουργήσετε ένα αρχείο ΒΆΖΟ που περιέχει την εφαρμογή:

        mvn clean package

    Αυτό θα εκκαθαρίσετε οποιαδήποτε προηγούμενη Δόμηση αντικείμενα, κάντε λήψη τυχόν εξαρτήσεις που δεν έχετε ήδη εγκαταστήσει, και, στη συνέχεια, δημιουργία και συσκευασία της εφαρμογής.

3. Μόλις ολοκληρωθεί η εντολή, ο κατάλογος __wordcountjava/προορισμού__ θα περιέχει ένα αρχείο που ονομάζεται __wordcountjava-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] Το αρχείο __wordcountjava-1.0-SNAPSHOT.jar__ είναι μια uberjar, η οποία περιέχει όχι μόνο την εργασία WordCount, αλλά επίσης εξαρτήσεις που απαιτεί η εργασία κατά το χρόνο εκτέλεσης.


##<a id="upload"></a>Αποστείλετε το βάζο

Χρησιμοποιήστε την παρακάτω εντολή για να αποστείλετε το αρχείο βάζο στο το HDInsight headnode:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

Αυτό αντιγράφει τα αρχεία από το τοπικό σύστημα στον κόμβο κεφαλίδας.

> [AZURE.NOTE] Εάν χρησιμοποιούσατε έναν κωδικό πρόσβασης για την ασφάλιση SSH το λογαριασμό σας, θα σας ζητηθεί για τον κωδικό πρόσβασης. Εάν χρησιμοποιείτε ένα πλήκτρο SSH, ίσως χρειαστεί να χρησιμοποιήσετε το `-i` παράμετρο και τη διαδρομή προς το ιδιωτικό κλειδί. Για παράδειγμα, `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

##<a name="run"></a>Εκτέλεση της εργασίας MapReduce

1. Σύνδεση με το HDInsight χρησιμοποιώντας SSH, όπως περιγράφεται στα ακόλουθα άρθρα:

    - [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, Unix ή λειτουργικό σύστημα OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Από την περίοδο λειτουργίας SSH, χρησιμοποιήστε την παρακάτω εντολή για να εκτελέσετε την εφαρμογή MapReduce:

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    Αυτό θα χρησιμοποιήσετε την εφαρμογή WordCount MapReduce για την καταμέτρηση λέξεων στο αρχείο davinci.txt και να αποθηκεύσετε τα αποτελέσματα σε __wasbs: / / / παράδειγμα/δεδομένων/wordcountout__. Το αρχείο εισόδου και το αποτέλεσμα είναι αποθηκευμένα στο χώρο αποθήκευσης προεπιλογή για το σύμπλεγμα.

3. Μόλις ολοκληρωθεί η εργασία, χρησιμοποιήστε τα παρακάτω για να προβάλετε τα αποτελέσματα:

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    Θα πρέπει να λάβετε μια λίστα των λέξεων και μετρήσεις, με τιμές που είναι παρόμοιο με το εξής:

        zeal    1
        zelus   1
        zenith  2

##<a id="nextsteps"></a>Επόμενα βήματα

Σε αυτό το έγγραφο, μάθατε πώς μπορείτε να αναπτύξετε μια εργασία Java MapReduce. Δείτε τα ακόλουθα έγγραφα για άλλους τρόπους για να εργαστείτε με HDInsight.

- [Χρήση της ομάδας με το HDInsight][hdinsight-use-hive]
- [Χρήση γουρούνι με HDInsight][hdinsight-use-pig]
- [Χρήση MapReduce με HDInsight](hdinsight-use-mapreduce.md)

Για περισσότερες πληροφορίες, ανατρέξτε επίσης στο [Κέντρο για προγραμματιστές Java](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

