<properties
   pageTitle="Ανάπτυξη βάσει Java τοπολογίες για Apache καταιγίδας | Microsoft Azure"
   description="Μάθετε πώς να δημιουργείτε τοπολογίες καταιγίδας σε Java, δημιουργώντας μια απλή word τοπολογία count."
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
   ms.date="09/14/2016"
   ms.author="larryfr"/>

#<a name="develop-java-based-topologies-for-a-basic-word-count-application-with-apache-storm-and-maven-on-hdinsight"></a>Ανάπτυξη βάσει Java τοπολογίες για μια εφαρμογή του βασικού καταμέτρηση λέξεων με καταιγίδας Apache και Maven σε HDInsight

Μάθετε πώς μπορείτε να δημιουργήσετε μια τοπολογία βάσει Java για Apache καταιγίδας σε HDInsight με τη χρήση Maven. Θα ακολουθήσετε τη διαδικασία για τη δημιουργία μιας εφαρμογής βασικές καταμέτρηση λέξεων χρησιμοποιώντας Maven και Java, όπου η τοπολογία ορίζεται σε Java. Στη συνέχεια, θα μάθετε πώς μπορείτε να ορίσετε την τοπολογία χρησιμοποιώντας το πλαίσιο ροής.

> [AZURE.NOTE] Το πλαίσιο ροή είναι διαθέσιμες στο καταιγίδας 0.10.0 ή νεότερη έκδοση. Καταιγίδας 0.10.0 είναι διαθέσιμη με το HDInsight 3.3 και 3.4.

Αφού ολοκληρώσετε τα βήματα σε αυτό το έγγραφο, θα έχετε μια βασική τοπολογία που μπορείτε να αναπτύξετε σε καταιγίδας Apache στην HDInsight.

> [AZURE.NOTE] Μια ολοκληρωμένη έκδοση από το τοπολογίες που δημιουργήσατε σε αυτό το έγγραφο είναι διαθέσιμη στο [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* <a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" target="_blank">Κιτ προγραμματιστής Java (JDK) έκδοση 7</a>

* <a href="https://maven.apache.org/download.cgi" target="_blank">Maven</a>: Maven είναι ένα σύστημα Δόμηση έργου για τα έργα Java.

* Ένα πρόγραμμα επεξεργασίας κειμένου, όπως το Σημειωματάριο, <a href="http://www.gnu.org/software/emacs/" target="_blank">Emacs<a>, <a href="http://www.sublimetext.com/" target="_blank">Sublime κειμένου</a>, <a href="https://atom.io/" target="_blank">Atom.io</a>, <a href="http://brackets.io/" target="_blank">Brackets.io</a>. Ή μπορείτε να χρησιμοποιήσετε ένα ενιαίο περιβάλλον ανάπτυξης (IDE) όπως <a href="https://eclipse.org/" target="_blank">Έκλειψη</a> (έκδοση Σελήνη ή νεότερη έκδοση).

    > [AZURE.NOTE] Το πρόγραμμα επεξεργασίας ή IDE ενδέχεται να έχει συγκεκριμένες λειτουργίες για την εργασία με Maven που δεν απευθύνεται σε αυτό το έγγραφο. Για πληροφορίες σχετικά με τις δυνατότητες επεξεργασίας περιβάλλον σας, ανατρέξτε στην τεκμηρίωση του προϊόντος που χρησιμοποιείτε.

##<a name="configure-environment-variables"></a>Ρύθμιση παραμέτρων μεταβλητές περιβάλλοντος

Οι παρακάτω μεταβλητές περιβάλλοντος μπορεί να οριστεί κατά την εγκατάσταση του Java και το JDK. Ωστόσο, θα πρέπει να ελέγξετε ότι υπάρχει και ότι περιέχουν τις σωστές τιμές για το σύστημά σας.

* **JAVA_HOME** - πρέπει να κατευθύνουν στον κατάλογο όπου είναι εγκατεστημένο το περιβάλλον χρόνου εκτέλεσης Java (JRE). Για παράδειγμα, σε μια κατανομή Unix ή Linux, θα πρέπει να έχει μια τιμή που είναι παρόμοια με `/usr/lib/jvm/java-7-oracle`. Στα Windows, αυτό θα έχει μια τιμή που είναι παρόμοια με`c:\Program Files (x86)\Java\jre1.7`

* **ΔΙΑΔΡΟΜΉ** - πρέπει να περιλαμβάνει τις ακόλουθες διαδρομές:

    * **JAVA_HOME** (ή η ισοδύναμη διαδρομή)

    * **JAVA_HOME\bin** (ή η ισοδύναμη διαδρομή)

    * Στον κατάλογο όπου είναι εγκατεστημένο το Maven

##<a name="create-a-new-maven-project"></a>Δημιουργήστε ένα νέο έργο Maven

Από τη γραμμή εντολών, χρησιμοποιήστε τον ακόλουθο κώδικα για να δημιουργήσετε ένα νέο έργο Maven με το όνομα **WordCount**:

    mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

Αυτό θα δημιουργήσει έναν νέο κατάλογο που ονομάζεται **WordCount** στην τρέχουσα θέση, που περιλαμβάνει μια βασική Maven έργου.

Ο κατάλογος **WordCount** θα περιέχει τα ακόλουθα στοιχεία:

* **pom.XML**: περιέχει τις ρυθμίσεις για το έργο Maven.

* **src\main\java\com\microsoft\example**: περιέχει κώδικα της εφαρμογής σας.

* **src\test\java\com\microsoft\example**: περιέχει δοκιμές για την εφαρμογή σας. Για αυτό το παράδειγμα, θα σας θα δεν δημιουργούν δοκιμές.

###<a name="remove-the-example-code"></a>Κατάργηση του κώδικα παράδειγμα

Επειδή θα σας θα δημιουργούν μας εφαρμογής, διαγράψτε τη δοκιμή που έχει δημιουργηθεί και τα αρχεία της εφαρμογής:

*  **src\test\java\com\microsoft\example\AppTest.Java**

*  **src\main\java\com\microsoft\example\App.Java**

##<a name="add-properties"></a>Προσθήκη ιδιοτήτων

Maven σάς επιτρέπει να καθορίσετε τιμές σε επίπεδο έργου που ονομάζεται Ιδιότητες. Προσθέστε τα ακόλουθα μετά το `<url>http://maven.apache.org</url>` γραμμής:

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--
        Storm 0.10.0 is for HDInsight 3.3 and 3.4.
        To find the version information for earlier HDInsight cluster
        versions, see https://azure.microsoft.com/en-us/documentation/articles/hdinsight-component-versioning/
        -->
        <storm.version>0.10.0</storm.version>
    </properties>

Τώρα μπορούμε να χρησιμοποιήσουμε αυτές τις τιμές σε άλλες ενότητες. Για παράδειγμα, κατά τον καθορισμό της έκδοσης του καταιγίδας στοιχεία, μπορούμε να χρησιμοποιήσουμε `${storm.version}` αντί για σκληρό κωδικοποίησης μια τιμή.

##<a name="add-dependencies"></a>Προσθήκη εξαρτήσεων

Επειδή αυτή είναι μια τοπολογία καταιγίδας, πρέπει να προσθέσετε μια εξάρτηση για στοιχεία καταιγίδας. Ανοίξτε το αρχείο **pom.xml** και προσθέστε τον ακόλουθο κώδικα στο το ** &lt;εξαρτήσεις >** την ενότητα:

    <dependency>
      <groupId>org.apache.storm</groupId>
      <artifactId>storm-core</artifactId>
      <version>${storm.version}</version>
      <!-- keep storm out of the jar-with-dependencies -->
      <scope>provided</scope>
    </dependency>

Κατά τη μεταγλώττιση, Maven χρησιμοποιεί αυτές τις πληροφορίες για να αναζητήσετε **καταιγίδας πυρήνα** στο αποθετήριο Maven. Πραγματοποιεί αναζήτηση πρώτα στο αποθετήριο στον τοπικό σας υπολογιστή. Εάν δεν υπάρχουν τα αρχεία, θα κάντε λήψη των αρχείων από το δημόσιο χώρο αποθήκευσης Maven και αποθηκεύει στο τοπικό αποθετήριο.

> [AZURE.NOTE] Ειδοποίηση του `<scope>provided</scope>` γραμμής στην ενότητα έχουμε προσθέσει. Αυτό σας ενημερώνει για Maven για να εξαιρέσετε **καταιγίδας πυρήνα** από οποιαδήποτε αρχεία ΒΆΖΟ δημιουργούμε, επειδή θα παρέχονται από το σύστημα. Αυτό σας επιτρέπει τα πακέτα που δημιουργείτε να είναι λίγο μικρότερες και εξασφαλίζει ότι θα μπορούν να χρησιμοποιήσουν τα bit **καταιγίδας πυρήνα** που περιλαμβάνονται στο το καταιγίδας σε σύμπλεγμα HDInsight.

##<a name="build-configuration"></a>Δημιουργία ρύθμισης παραμέτρων

Προσθήκες maven σάς επιτρέπουν να προσαρμόσετε τα στάδια δόμηση του έργου, όπως το πώς έχει μεταγλωττιστεί το έργο ή πώς μπορείτε να συσκευάσετε σε ένα αρχείο ΒΆΖΟ. Ανοίξτε το αρχείο **pom.xml** και προσθέστε τον ακόλουθο κώδικα απευθείας από πάνω του `</project>` γραμμής.

    <build>
      <plugins>
      </plugins>
      <resources>
      </resources>
    </build>

Αυτή η ενότητα θα χρησιμοποιηθεί για την προσθήκη προσθήκες, τους πόρους και άλλες επιλογές ρύθμισης παραμέτρων Δόμηση. Για μια πλήρη αναφορά του αρχείου __pom.xml__ , ανατρέξτε στο θέμα [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

###<a name="add-plug-ins"></a>Προσθήκη προσθήκες

Για τοπολογίες καταιγίδας, την <a href="http://mojo.codehaus.org/exec-maven-plugin/" target="_blank">Προσθήκη Maven εκτέλεσης</a> είναι χρήσιμη επειδή σάς επιτρέπει να εύκολα εκτέλεση της τοπολογίας τοπικά στο περιβάλλον ανάπτυξής σας. Προσθέστε τα εξής για να το `<plugins>` ενότητα του αρχείου **pom.xml** για να συμπεριλάβετε την προσθήκη Maven εκτέλεσης:

    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>1.4.0</version>
      <executions>
        <execution>
        <goals>
          <goal>exec</goal>
        </goals>
        </execution>
      </executions>
      <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
      </configuration>
    </plugin>

> [AZURE.NOTE] Λάβετε υπόψη ότι η `<mainClass>` χρησιμοποιεί καταχώρησης `${storm.topology}`. Δεν έχετε ορισμού αυτό νωρίτερα στην ενότητα Ιδιότητες (αλλά μπορεί να έχουμε.) Αντί για αυτό, θα μπορούμε να εγκαταστήσουμε αυτήν την τιμή από τη γραμμή εντολών, όταν εκτελείται η τοπολογία σε περιβάλλον ανάπτυξής σας σε ένα επόμενο βήμα.

Ένα άλλο χρήσιμο είναι η προσθήκη της <a href="http://maven.apache.org/plugins/maven-compiler-plugin/" target="_blank">Προσθήκης προγράμματος μεταγλώττισης Maven Apache</a>, που χρησιμοποιείται για να αλλάξετε τις επιλογές μεταγλώττισης. Ο κύριος λόγος χρειαζόμαστε αυτό είναι να αλλάξετε την έκδοση Java που χρησιμοποιεί το Maven για προέλευσης και προορισμού για την εφαρμογή σας. Θέλουμε έκδοση 1.7.

Προσθέστε τα εξής στο το `<plugins>` ενότητα του αρχείου **pom.xml** για να συμπεριλάβετε την προσθήκη προγράμματος μεταγλώττισης Maven Apache και ορίστε τις εκδόσεις προέλευσης και προορισμού σε 1.7.

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

###<a name="configure-resources"></a>Ρύθμιση παραμέτρων πόροι

Η ενότητα "Πόροι" σάς επιτρέπει να περιλαμβάνουν μη κώδικα πόρους, όπως αρχεία ρύθμισης παραμέτρων που απαιτούνται από τα στοιχεία της τοπολογίας. Για αυτό το παράδειγμα, προσθέστε τα εξής στο το `<resources>` ενότητα του αρχείου **pom.xml** .

    <resource>
        <directory>${basedir}/resources</directory>
        <filtering>false</filtering>
        <includes>
          <include>log4j2.xml</include>
        </includes>
    </resource>

Αυτό προσθέτει τον κατάλογο πόρους στον ριζικό κατάλογο του έργου (`${basedir}`) ως θέση που περιέχει τους πόρους και περιλαμβάνει το αρχείο με το όνομα __log4j2.xml__. Αυτό το αρχείο χρησιμοποιείται για τη ρύθμιση παραμέτρων ποιες πληροφορίες είναι συνδεδεμένος με την τοπολογία.

##<a name="create-the-topology"></a>Δημιουργία της τοπολογίας

Μια τοπολογία βάσει Java καταιγίδας αποτελείται από τρία στοιχεία που πρέπει να συντάσσετε (ή αναφορά) ως εξάρτηση.

* **Spouts**: διαβάζει δεδομένα από εξωτερικές προελεύσεις και εκπέμπει ροών δεδομένων σε της τοπολογίας.

* **Μπουλονιών**: πραγματοποιεί επεξεργασία σε ροές που εκπέμπει spouts ή άλλα στοιχεία και εκπέμπει ένα ή περισσότερα ροών.

* **Τοπολογία**: Καθορίζει τον τρόπο το spouts στοιχεία έχουν τακτοποιηθεί και παρέχει το σημείο εισόδου για την τοπολογία.

###<a name="create-the-spout"></a>Δημιουργία του στομίου

Για να μειώσετε τις απαιτήσεις για τη ρύθμιση των εξωτερικών προελεύσεων δεδομένων, η παρακάτω στομίου εκπέμπει απλώς τυχαία προτάσεων. Είναι μια τροποποιημένη έκδοση του ένα στομίου που παρέχεται με τα [παραδείγματα καταιγίδας Starter](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).

> [AZURE.NOTE] Για παράδειγμα ένα στομίου που διαβάζει από μια εξωτερική προέλευση δεδομένων, ανατρέξτε σε ένα από τα παρακάτω παραδείγματα:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): ένα παράδειγμα στομίου που διαβάζει από Twitter
>
> * [Καταιγίδας Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): μια στομίου που διαβάζει από Kafka

Για το στομίου, δημιουργήστε ένα νέο αρχείο με το όνομα **RandomSentenceSpout.java** στον κατάλογο **src\main\java\com\microsoft\example** και χρησιμοποιήστε τα ακόλουθα με τα περιεχόμενα:

    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     * http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

     /**
      * Original is available at https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/storm/starter/spout/RandomSentenceSpout.java
      */

    package com.microsoft.example;

    import backtype.storm.spout.SpoutOutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichSpout;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Values;
    import backtype.storm.utils.Utils;

    import java.util.Map;
    import java.util.Random;

    //This spout randomly emits sentences
    public class RandomSentenceSpout extends BaseRichSpout {
      //Collector used to emit output
      SpoutOutputCollector _collector;
      //Used to generate a random number
      Random _rand;

      //Open is called when an instance of the class is created
      @Override
      public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
      //Set the instance collector to the one passed in
        _collector = collector;
        //For randomness
        _rand = new Random();
      }

      //Emit data to the stream
      @Override
      public void nextTuple() {
      //Sleep for a bit
        Utils.sleep(100);
        //The sentences that will be randomly emitted
        String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
            "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
        //Randomly pick a sentence
        String sentence = sentences[_rand.nextInt(sentences.length)];
        //Emit the sentence
        _collector.emit(new Values(sentence));
      }

      //Ack is not implemented since this is a basic example
      @Override
      public void ack(Object id) {
      }

      //Fail is not implemented since this is a basic example
      @Override
      public void fail(Object id) {
      }

      //Declare the output fields. In this case, an sentence
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("sentence"));
      }
    }

Αφιερώστε λίγο χρόνο για να διαβάσετε τα σχόλια κώδικα για να κατανοήσετε τον τρόπο λειτουργίας αυτό στομίου.

> [AZURE.NOTE] Παρόλο που αυτή η τοπολογία χρησιμοποιεί μόνο μία στομίου, άλλα άτομα μπορεί να έχει πολλές που τροφοδοσίας δεδομένων από διαφορετικές προελεύσεις σε της τοπολογίας.

###<a name="create-the-bolts"></a>Δημιουργήστε τα στοιχεία

Στοιχεία χειρισμού της επεξεργασίας δεδομένων. Για αυτό τοπολογία, έχουμε δύο στοιχεία:

* **SplitSentence**: διαιρεί τις προτάσεις που εκπέμπει **RandomSentenceSpout** σε μεμονωμένες λέξεις.

* **WordCount**: καταμετρά πόσες φορές Παρουσιάστηκε κάθε λέξη.

> [AZURE.NOTE] Στοιχεία μπορεί να γίνει ως έχουν οτιδήποτε άλλο, για παράδειγμα, κατά τον υπολογισμό, διατήρησης ή μιλά σε εξωτερικά στοιχεία.

Δημιουργήστε δύο νέα αρχεία, **SplitSentence.java** και **WordCount.Java** στον κατάλογο **src\main\java\com\microsoft\example** . Χρησιμοποιήστε τα παρακάτω ανάλογα με το περιεχόμενο για τα αρχεία:

**SplitSentence**

    package com.microsoft.example;

    import java.text.BreakIterator;

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class SplitSentence extends BaseBasicBolt {

      //Execute is called to process tuples
      @Override
      public void execute(Tuple tuple, BasicOutputCollector collector) {
        //Get the sentence content from the tuple
        String sentence = tuple.getString(0);
        //An iterator to get each word
        BreakIterator boundary=BreakIterator.getWordInstance();
        //Give the iterator the sentence
        boundary.setText(sentence);
        //Find the beginning first word
        int start=boundary.first();
        //Iterate over each word and emit it to the output stream
        for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
          //get the word
          String word=sentence.substring(start,end);
          //If a word is whitespace characters, replace it with empty
          word=word.replaceAll("\\s+","");
          //if it's an actual word, emit it
          if (!word.equals("")) {
            collector.emit(new Values(word));
          }
        }
      }

      //Declare that emitted tuples will contain a word field
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("word"));
      }
    }

**WordCount**

    package com.microsoft.example;

    import java.util.HashMap;
    import java.util.Map;
    import java.util.Iterator;

    import backtype.storm.Constants;
    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;
    import backtype.storm.Config;

    // For logging
    import org.apache.logging.log4j.Logger;
    import org.apache.logging.log4j.LogManager;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class WordCount extends BaseBasicBolt {
        //Create logger for this class
        private static final Logger logger = LogManager.getLogger(WordCount.class);
        //For holding words and counts
        Map<String, Integer> counts = new HashMap<String, Integer>();
        //How often we emit a count of words
        private Integer emitFrequency;

        // Default constructor
        public WordCount() {
            emitFrequency=5; // Default to 60 seconds
        }

        // Constructor that sets emit frequency
        public WordCount(Integer frequency) {
            emitFrequency=frequency;
        }

        //Configure frequency of tick tuples for this bolt
        //This delivers a 'tick' tuple on a specific interval,
        //which is used to trigger certain actions
        @Override
        public Map<String, Object> getComponentConfiguration() {
            Config conf = new Config();
            conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
            return conf;
        }

        //execute is called to process tuples
        @Override
        public void execute(Tuple tuple, BasicOutputCollector collector) {
            //If it's a tick tuple, emit all words and counts
            if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
                    && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
                for(String word : counts.keySet()) {
                    Integer count = counts.get(word);
                    collector.emit(new Values(word, count));
                    logger.info("Emitting a count of " + count + " for word " + word);
                }
            } else {
                //Get the word contents from the tuple
                String word = tuple.getString(0);
                //Have we counted any already?
                Integer count = counts.get(word);
                if (count == null)
                    count = 0;
                //Increment the count and store it
                count++;
                counts.put(word, count);
            }
        }

        //Declare that we will emit a tuple containing two fields; word and count
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            declarer.declare(new Fields("word", "count"));
        }
    }

Αφιερώστε λίγο χρόνο για να διαβάσετε τα σχόλια κώδικα για να κατανοήσετε τον τρόπο λειτουργίας κάθε κεραυνό.

###<a name="define-the-topology"></a>Ορισμός της τοπολογίας

Η τοπολογία συνδέει το spouts και μπουλονιών μαζί σε ένα γράφημα, η οποία καθορίζει τον τρόπο ροής δεδομένων μεταξύ των στοιχείων. Επίσης, παρέχει συμβουλές παραλληλισμό που χρησιμοποιεί καταιγίδας κατά τη δημιουργία των εμφανίσεων τα στοιχεία μέσα στο σύμπλεγμα.

Ακολουθεί ένα βασικό διάγραμμα του γραφήματος των στοιχείων για αυτό τοπολογίας.

![διάγραμμα που δείχνει τη διάταξη spouts και στοιχεία](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

Για την υλοποίηση της τοπολογίας, δημιουργήστε ένα νέο αρχείο με το όνομα **WordCountTopology.java** στον κατάλογο **src\main\java\com\microsoft\example** . Χρησιμοποιήστε τα παρακάτω ανάλογα με το περιεχόμενο για το αρχείο:

    package com.microsoft.example;

    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.topology.TopologyBuilder;
    import backtype.storm.tuple.Fields;

    import com.microsoft.example.RandomSentenceSpout;

    public class WordCountTopology {

      //Entry point for the topology
      public static void main(String[] args) throws Exception {
      //Used to build the topology
        TopologyBuilder builder = new TopologyBuilder();
        //Add the spout, with a name of 'spout'
        //and parallelism hint of 5 executors
        builder.setSpout("spout", new RandomSentenceSpout(), 5);
        //Add the SplitSentence bolt, with a name of 'split'
        //and parallelism hint of 8 executors
        //shufflegrouping subscribes to the spout, and equally distributes
        //tuples (sentences) across instances of the SplitSentence bolt
        builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
        //Add the counter, with a name of 'count'
        //and parallelism hint of 12 executors
        //fieldsgrouping subscribes to the split bolt, and
        //ensures that the same word is sent to the same instance (group by field 'word')
        builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

        //new configuration
        Config conf = new Config();
        //Set to false to disable debug information
        // when running in production mode.
        conf.setDebug(false);

        //If there are arguments, we are running on a cluster
        if (args != null && args.length > 0) {
          //parallelism hint to set the number of workers
          conf.setNumWorkers(3);
          //submit the topology
          StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
        }
        //Otherwise, we are running locally
        else {
          //Cap the maximum number of executors that can be spawned
          //for a component to 3
          conf.setMaxTaskParallelism(3);
          //LocalCluster is used to run locally
          LocalCluster cluster = new LocalCluster();
          //submit the topology
          cluster.submitTopology("word-count", conf, builder.createTopology());
          //sleep
          Thread.sleep(10000);
          //shut down the cluster
          cluster.shutdown();
        }
      }
    }

Αφιερώστε λίγο χρόνο για να διαβάσετε τα σχόλια κώδικα για να κατανοήσετε τον τρόπο της τοπολογίας είναι που ορίζονται από το και, στη συνέχεια, που υποβάλλεται στο σύμπλεγμα.

###<a name="configure-logging"></a>Ρύθμιση παραμέτρων καταγραφής

Καταιγίδας χρησιμοποιεί Apache Log4j την καταγραφή πληροφοριών. Εάν ρυθμίσετε καταγραφής, της τοπολογίας θα εκπέμπει πολλές πληροφορίες διαγνωστικών, οι οποίες μπορεί να είναι δύσκολο να διαβάσετε. Για να ελέγξετε τι έχει συνδεθεί, δημιουργήστε ένα αρχείο με το όνομα __log4j2.xml__ στον κατάλογο __πόρους__ . Χρησιμοποιήστε τα ακόλουθα με τα περιεχόμενα του αρχείου.

    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration>
    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.microsoft.example" level="trace" additivity="false">
            <AppenderRef ref="STDOUT"/>
        </Logger>
        <Root level="error">
            <Appender-Ref ref="STDOUT"/>
        </Root>
    </Loggers>
    </Configuration>

Αυτό ρυθμίζει τις παραμέτρους μιας νέας καταγραφής για την κλάση __com.microsoft.example__ , η οποία περιλαμβάνει τα στοιχεία σε αυτό το παράδειγμα τοπολογίας. Το επίπεδο έχει οριστεί σε ανίχνευση για αυτό καταγραφής, που θα καταγραφή πληροφοριών καταγραφής εκπέμπει στοιχείων στην τοπολογία αυτό. Εάν κάνετε αναζήτηση ξανά μέσω του κώδικα για αυτό το έργο, θα παρατηρήσετε ότι μόνο το αρχείο WordCount.java υλοποιεί τη σύνδεση; θα καταγράψει το πλήθος των λέξεων.

Το `<Root level="error">` ενότητα ρυθμίζει τις παραμέτρους ριζικό επίπεδο καταγραφής (όλα τα στοιχεία δεν σε __com.microsoft.example__,) για να συνδεθείτε μόνο πληροφοριών σφάλματος.

> [AZURE.IMPORTANT] Ενώ αυτό μειώνει σημαντικά τις πληροφορίες που έχετε συνδεθεί κατά τη δοκιμή μια τοπολογία στο περιβάλλον ανάπτυξής σας, αυτό δεν καταργεί όλες τις πληροφορίες εντοπισμού σφαλμάτων που παράγεται όταν εκτελείται σε ένα σύμπλεγμα στο παραγωγής. Για να μειώσετε αυτές τις πληροφορίες, πρέπει επίσης να ορίσετε τον εντοπισμό σφαλμάτων σε false στη ρύθμιση παραμέτρων που υποβάλλεται στο σύμπλεγμα. Δείτε τον κωδικό WordCountTopology.java σε αυτό το έγγραφο για ένα παράδειγμα. 

Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων καταγραφής για Log4j, ανατρέξτε στο θέμα [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [AZURE.NOTE] Έκδοση καταιγίδας 0.10.0 χρησιμοποιεί Log4j 2.x. Παλαιότερες εκδόσεις του καταιγίδας χρησιμοποιούνται Log4j 1.x, που χρησιμοποιούνται σε διαφορετική μορφή για τη ρύθμιση παραμέτρων καταγραφής. Για πληροφορίες σχετικά με τη ρύθμιση παραμέτρων παλαιότερων, ανατρέξτε στο θέμα [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

##<a name="test-the-topology-locally"></a>Δοκιμή της τοπολογίας τοπικά

Μετά την αποθήκευση των αρχείων, χρησιμοποιήστε την ακόλουθη εντολή για να ελέγξετε την τοπολογία τοπικά.

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

Κατά την εκτέλεσή της τοπολογίας θα εμφανίζουν πληροφορίες εκκίνησης. Στη συνέχεια, ξεκινά για να εμφανίσετε γραμμές με την εξής όπως προτάσεις είναι που εκπέμπει από το στομίου και υποβάλλονται σε επεξεργασία από τα στοιχεία.

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Κοιτώντας την καταγραφή εκπέμπει η ράβδος WordCount, θα σας μπορεί να δει που ' και ' έχει που εκπέμπει 113 ώρες. Το πλήθος θα συνεχίσει να μετακινηθείτε προς τα επάνω, με την προϋπόθεση ότι η τοπολογία εκτελείται επειδή το στομίου εκπέμπει συνεχώς το ίδιο προτάσεων.

Θα υπάρχει επίσης ένα 5 δεύτερο διάστημα μεταξύ εκπομπής των λέξεων και το πλήθος. Αυτό συμβαίνει επειδή το στοιχείο __WordCount__ έχει ρυθμιστεί να εκπέμπει μόνο πληροφορίες κατά την άφιξη μιας πλειάδας υποδιαίρεσης και απαιτεί ότι οι πλειάδων παραδίδονται μόνο κάθε 5 δευτερόλεπτα από προεπιλογή.

## <a name="convert-the-topology-to-flux"></a>Μετατροπή της τοπολογίας ροής

Ροή είναι ένα νέο πλαίσιο είναι διαθέσιμη με καταιγίδας 0.10.0, η οποία σας επιτρέπει να διαχωρίσετε ρύθμισης παραμέτρων από την εφαρμογή. Τα στοιχεία σας (παξιμάδια και spouts,) εξακολουθεί να ορίζονται σε Java, αλλά η τοπολογία ορίζεται χρησιμοποιώντας ένα αρχείο YAML.

Το αρχείο YAML ορίζει τα στοιχεία για να χρησιμοποιήσετε για την τοπολογία, τον τρόπο ροής δεδομένων μεταξύ τους και ποιες τιμές για χρήση κατά την προετοιμασία των στοιχείων. Μπορείτε να συμπεριλάβετε ένα αρχείο YAML ως μέρος του αρχείου βάζο που περιέχει το έργο σας κατά την αναπτύξετε, ή μπορείτε να χρησιμοποιήσετε ένα εξωτερικό αρχείο YAML κατά την εκκίνηση της τοπολογίας.

1. Μετακινήστε το αρχείο __WordCountTopology.java__ εκτός του έργου. Στο παρελθόν, αυτό που ορίζονται από την τοπολογία, αλλά θα σας δεν θα χρησιμοποιείτε το για ροή.

2. Στον κατάλογο __πόρων__ , δημιουργήστε ένα νέο αρχείο με το όνομα __topology.yaml__. Χρησιμοποιήστε τα ακόλουθα με τα περιεχόμενα αυτού του αρχείου.

        # topology definition

        # name to be used when submitting. This is what shows up...
        # in the Storm UI/storm command-line tool as the topology name
        # when submitted to Storm
        name: "wordcount"

        # Topology configuration
        config:
        # Hint for the number of workers to create
        topology.workers: 1

        # Spout definitions
        spouts:
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            # parallelism hint
            parallelism: 1

        # Bolt definitions
        bolts:
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1

        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 10
            parallelism: 1

        # Stream definitions
        streams:
        - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            # The stream emitter
            from: "sentence-spout"
            # The stream consumer
            to: "splitter-bolt"
            # Grouping type
            grouping:
            type: SHUFFLE

        - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
            # field(s) to group on
            args: ["word"]

    Αφιερώστε λίγο χρόνο για να διαβάσετε και να κατανοήσετε τι σημαίνει κάθε ενότητα και πώς σχετίζεται με τον ορισμό βάσει Java στο αρχείο __WordCountTopology.java__ .

3. Κάνετε τις ακόλουθες αλλαγές στο αρχείο __pom.xml__ .

    * Προσθέστε τα παρακάτω νέα εξάρτηση σε το `<dependencies>` την ενότητα:

            <!-- Add a dependency on the Flux framework -->
            <dependency>
                <groupId>org.apache.storm</groupId>
                <artifactId>flux-core</artifactId>
                <version>${storm.version}</version>
            </dependency>

    * Προσθέστε την ακόλουθη προσθήκη για να το `<plugins>` ενότητας. Αυτή η προσθήκη χειρισμού της δημιουργίας ενός πακέτου (αρχείο βάζο) για το έργο και ισχύει κάποια συγκεκριμένη μετασχηματισμοί για ροή κατά τη δημιουργία του πακέτου.

            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                        <!-- Keep us from getting a "can't overwrite file error" -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                        <!-- We're using Flux, so refer to it as main -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>org.apache.storm.flux.Flux</mainClass>
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
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

    * Στην __εκτέλεσης-maven-Προσθήκη__ `<configuration>` ενότητας, αλλάξτε την τιμή για `<mainClass>` να `org.apache.storm.flux.Flux`. Αυτό σας επιτρέπει ροή χειρισμού εκτελείται η τοπολογία όταν μπορούμε να εκτελέσουμε το τοπικά σε ανάπτυξη.

    * Στο το `<resources>` ενότητα, προσθέστε τα εξής για να το `<includes>`. Αυτό περιλαμβάνει το αρχείο YAML που καθορίζει την τοπολογία ως μέρος του έργου.
    
            <include>topology.yaml</include>

## <a name="test-the-flux-topology-locally"></a>Δοκιμή της τοπολογίας ροή τοπικά

1. Χρησιμοποιήστε τα ακόλουθα για να μεταγλωττίσετε και εκτέλεση της τοπολογίας ροή χρησιμοποιώντας Maven.

        mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    
    Εάν χρησιμοποιείτε το PowerShell, χρησιμοποιήστε τα εξής:
    
        mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"

    Εάν είστε σε ένα σύστημα Unix/Linux/OS X και έχετε [εγκαταστήσει καταιγίδας στο περιβάλλον ανάπτυξής σας](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), μπορείτε να χρησιμοποιήσετε τις παρακάτω εντολές:

        mvn compile package
        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml

    Το `--local` παραμέτρου εκτελείται της τοπολογίας σε τοπική λειτουργία στο περιβάλλον ανάπτυξής σας. Το `-R /topology.yaml` παράμετρος χρησιμοποιεί το `topology.yaml` αρχείο πόρων από το αρχείο βάζο για τον ορισμό της τοπολογίας.

    Κατά την εκτέλεσή της τοπολογίας θα εμφανίζουν πληροφορίες εκκίνησης. Στη συνέχεια, ξεκινά για να εμφανίσετε γραμμές με την εξής όπως προτάσεις είναι που εκπέμπει από το στομίου και υποβάλλονται σε επεξεργασία από τα στοιχεία.

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    
    Θα υπάρξει 10 δεύτερο καθυστέρηση ανάμεσα σε δέσμες με πληροφορίες που καταγράφονται, ως το `topology.yaml` αρχείο μεταβιβάζει μια τιμή του `10` όταν δημιουργείται το στοιχείο WordCount. Αυτό ορίζει το διάστημα καθυστέρηση για στοιχεία της πλειάδας δεν υποδιαίρεσης σε 10 δευτερόλεπτα.

2.  Δημιουργήστε ένα αντίγραφο του το `topology.yaml` αρχείο από το έργο. Ονομάστε την `newtopology.yaml`. Στο αρχείο, βρείτε την παρακάτω ενότητα και αλλάξτε την τιμή του `10` σε `5`. Αυτό αλλάζει το διάστημα μεταξύ εκπομπής δέσμες καταμέτρηση λέξεων από 10 δευτερόλεπτα έως 5.

          - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 5
            parallelism: 1

3. Για να εκτελέσετε την τοπολογία, χρησιμοποιήστε την ακόλουθη εντολή:

        mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"

    Εναλλακτικά, εάν έχετε καταιγίδας στο περιβάλλον ανάπτυξής σας Unix/Linux/OS X:

        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml

    Αλλαγή του `/path/to/newtopology.yaml` για τη διαδρομή προς το αρχείο newtopology.yaml που δημιουργήσατε στο προηγούμενο βήμα. Αυτή η εντολή θα χρησιμοποιήσει το newtopology.yaml ως τον ορισμό της τοπολογίας. Εφόσον δεν περιλαμβάνονται τα `compile` παράμετρο, Maven θα εκ νέου χρήση της έκδοσης του έργου ενσωματωμένη προηγούμενα βήματα.

    Αφού ξεκινήσει η τοπολογία, θα πρέπει να παρατηρήσετε ότι έχει αλλάξει το χρονικό διάστημα μεταξύ εκπεμπομένου δέσμες ώστε να αντικατοπτρίζει την τιμή στο newtopology.yaml. Ώστε να μπορείτε να δείτε ότι μπορείτε να αλλάξετε ρύθμιση των παραμέτρων σας μέσω ενός αρχείου YAML χωρίς να χρειάζεται να μεταγλωττίσετε της τοπολογίας.

Υπάρχουν πολλές άλλες δυνατότητες που ροή παρέχει που δεν αναφέρεται εδώ, όπως υποκατάσταση μεταβλητής στο το αρχείο YAML με βάση τις παραμέτρους που εισήχθησαν κατά το χρόνο εκτέλεσης ή από μεταβλητές περιβάλλοντος. Για περισσότερες πληροφορίες σχετικά με αυτές και άλλες δυνατότητες του πλαισίου ροής, ανατρέξτε στο θέμα [ροή (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

##<a name="trident"></a>Trident

Trident είναι μια υψηλού επιπέδου αφαίρεσης που παρέχεται από καταιγίδας. Υποστηρίζει με κατάσταση επεξεργασίας. Το κύριο πλεονέκτημα της Trident είναι ότι το να εξασφαλίσετε ότι κάθε μήνυμα που εισάγει την τοπολογία γίνεται μόνο μία φορά. Αυτό είναι δύσκολο να επιτύχετε μια ανεπεξέργαστα τοπολογίας Java, ποια εγγύησης του ότι τα μηνύματα θα γίνει επεξεργασία τουλάχιστον μία φορά. Υπάρχουν επίσης άλλες διαφορές, όπως ενσωματωμένων στοιχείων που μπορούν να χρησιμοποιηθούν αντί να δημιουργήσετε στοιχεία. Στην πραγματικότητα, βίδες εντελώς αντικαθίστανται από γενική λιγότερο στοιχεία, όπως τα φίλτρα, προβλέψεις και συναρτήσεις.

Trident εφαρμογές μπορούν να δημιουργηθούν με τη χρήση Maven έργα. Μπορείτε να χρησιμοποιήσετε τα ίδια βασικά βήματα που παρουσιάζονται νωρίτερα σε αυτό το άρθρο, μόνο ο κώδικας είναι διαφορετικές. Trident δεν είναι δυνατό να (αυτήν τη στιγμή) να χρησιμοποιηθούν με το πλαίσιο ροής.

Για περισσότερες πληροφορίες σχετικά με την Trident, ανατρέξτε στο θέμα η <a href="http://storm.apache.org/documentation/Trident-API-Overview.html" target="_blank">Επισκόπηση API Trident</a>.

Για ένα παράδειγμα μιας εφαρμογής Trident, ανατρέξτε στο θέμα [Twitter τάσης θέματα με καταιγίδας Apache στην HDInsight](hdinsight-storm-twitter-trending.md).

##<a name="next-steps"></a>Επόμενα βήματα

Μάθατε πώς μπορείτε να δημιουργήσετε μια τοπολογία καταιγίδας χρησιμοποιώντας Java. Τώρα μάθετε πώς μπορείτε να:

* [Ανάπτυξη και διαχείριση τοπολογίες καταιγίδας Apache στην HDInsight](hdinsight-storm-deploy-monitor-topology.md)

* [Ανάπτυξη C# τοπολογίες για Apache καταιγίδας στη χρήση του Visual Studio HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Μπορείτε να βρείτε περισσότερες παράδειγμα τοπολογίες καταιγίδας μεταβαίνοντας [τοπολογίες παράδειγμα για καταιγίδας στην HDInsight](hdinsight-storm-example-topology.md).
