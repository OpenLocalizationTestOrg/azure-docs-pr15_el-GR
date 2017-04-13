<properties
pageTitle="Χρήση μιας συνάρτησης που ορίζονται από το χρήστη Java (UDF) με ομάδα στο HDInsight | Microsoft Azure"
description="Μάθετε πώς μπορείτε να δημιουργήσετε και να χρησιμοποιήσετε μια συνάρτηση που ορίζεται από το χρήστη Java (UDF) από ομάδα στο HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>Χρησιμοποιήστε ένα Java UDF με ομάδα στο HDInsight

Ομάδα είναι ιδανικά για την εργασία με δεδομένα σε HDInsight, αλλά μερικές φορές χρειάζεται μια πιο γενικού σκοπό γλώσσας. Η ομάδα σας επιτρέπει να δημιουργήσετε συναρτήσεις χρήστη (UDF) χρησιμοποιώντας μια ποικιλία γλώσσες προγραμματισμού. Σε αυτό το έγγραφο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε μια UDF Java από ομάδα.

## <a name="requirements"></a>Απαιτήσεις

* Μια συνδρομή του Azure

* Ένα σύμπλεγμα HDInsight (Windows ή Linux βάσει)

    > [AZURE.NOTE] Τα περισσότερα βήματα που περιγράφονται σε αυτό το έγγραφο θα λειτουργεί και στους δύο τύπους σύμπλεγμα; Ωστόσο, τα βήματα που χρησιμοποιούνται για να αποστείλετε το μεταγλωττισμένο UDF στο σύμπλεγμα και εκτελέστε το αφορούν ειδικά βάσει Linux συμπλεγμάτων. Παρέχονται συνδέσεις σε πληροφορίες που μπορούν να χρησιμοποιηθούν με συμπλεγμάτων που βασίζεται στα Windows.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 ή νεότερη έκδοση (ή μια αντίστοιχη, όπως OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* Ένα πρόγραμμα επεξεργασίας κειμένου ή Java IDE

    > [AZURE.IMPORTANT] Εάν χρησιμοποιείτε ένα διακομιστή βάσει Linux HDInsight, αλλά τη δημιουργία αρχείων Python σε ένα πρόγραμμα-πελάτη των Windows, πρέπει να χρησιμοποιείτε ένα πρόγραμμα επεξεργασίας που χρησιμοποιεί LF ως κατάληξη γραμμής. Εάν δεν είστε βέβαιοι αν το πρόγραμμα επεξεργασίας χρησιμοποιεί LF ή CRLF, ανατρέξτε στην ενότητα [Αντιμετώπιση προβλημάτων](#troubleshooting) για οδηγίες σχετικά με την κατάργηση του χαρακτήρα CR χρήση βοηθητικών προγραμμάτων στο σύμπλεγμα HDInsight.

## <a name="create-an-example-udf"></a>Δημιουργήστε ένα παράδειγμα UDF

1. Από τη γραμμή εντολών, χρησιμοποιήστε τα ακόλουθα για να δημιουργήσετε ένα νέο έργο Maven:

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] Εάν χρησιμοποιείτε το PowerShell, θα πρέπει να τοποθετήσετε εισαγωγικά γύρω από τις παραμέτρους. Για παράδειγμα, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Αυτό θα δημιουργήσει έναν νέο κατάλογο που ονομάζεται __exampleudf__, που θα περιέχει το έργο Maven.

2. Μετά τη δημιουργία του έργου, η διαγραφή του καταλόγου __exampleudf/src έλεγχο__ που δημιουργήθηκε ως μέρος του έργου. θα δεν χρησιμοποιηθεί για αυτό το παράδειγμα.

3. Ανοίξτε το __exampleudf/pom.xml__και αντικαταστήστε το υπάρχον `<dependencies>` εγγραφή με τα εξής:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Αυτές οι καταχωρήσεις Καθορίστε την έκδοση του Hadoop και Hive περιλαμβάνεται με το HDInsight 3.3 και 3.4 συμπλεγμάτων. Μπορείτε να βρείτε πληροφορίες σχετικά με τις εκδόσεις του Hadoop και ομάδας που παρέχεται με το HDInsight από το έγγραφο [διαχείρισης εκδόσεων στοιχείου HDInsight](hdinsight-component-versioning.md) .

    Προσθήκη μιας `<build>` ενότητα πριν από την `</project>` γραμμή στο τέλος του αρχείου. Αυτή η ενότητα θα πρέπει να περιλαμβάνουν τα εξής:

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
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
            </plugins>
        </build>
    
    Αυτές οι καταχωρήσεις ορίζουν τον τρόπο δημιουργίας του έργου. Συγκεκριμένα, η έκδοση του Java που χρησιμοποιεί το project και πώς να δημιουργείτε μια uberjar για ανάπτυξη στο σύμπλεγμα.

    Αποθηκεύστε το αρχείο όταν έχει γίνει οι αλλαγές.

4. Μετονομάστε __exampleudf/src/main/java/com/microsoft/examples/App.java__ σε __ExampleUDF.java__και, στη συνέχεια, ανοίξτε το αρχείο στο πρόγραμμα επεξεργασίας.

5. Αντικαταστήστε τα περιεχόμενα του αρχείου __ExampleUDF.java__ με τα εξής, στη συνέχεια, αποθηκεύστε το αρχείο.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    Αυτό υλοποιεί μια UDF που δέχεται μια τιμή συμβολοσειράς και επιστρέφει μια έκδοση της συμβολοσειράς με πεζά γράμματα.

## <a name="build-and-install-the-udf"></a>Δημιουργία και εγκαταστήστε το UDF

1. Χρησιμοποιήστε την παρακάτω εντολή για να μεταγλωττίσετε και να συσκευάσετε το UDF:

        mvn compile package

    Αυτό θα δημιουργήσετε, στη συνέχεια, πακέτο το UDF σε __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__.

2. Χρησιμοποιήστε το `scp` εντολή για να αντιγράψετε το αρχείο στο σύμπλεγμα HDInsight.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    Αντικαταστήστε __myuser__ με το λογαριασμό χρήστη SSH για το σύμπλεγμά σας. Αντικαταστήστε __mycluster__ με το όνομα του συμπλέγματος. Εάν χρησιμοποιούσατε έναν κωδικό πρόσβασης για την ασφάλιση του λογαριασμού SSH, θα σας ζητηθεί να εισαγάγετε τον κωδικό πρόσβασης. Εάν έχετε χρησιμοποιήσει ένα πιστοποιητικό, ίσως χρειαστεί να χρησιμοποιήσετε το `-i` παραμέτρου για να καθορίσετε το αρχείο ιδιωτικού κλειδιού.

3. Συνδεθείτε με το σύμπλεγμα χρησιμοποιώντας SSH. 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με το HDInsight, ανατρέξτε στο θέμα τα ακόλουθα έγγραφα.

    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, Unix ή λειτουργικό σύστημα OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

4. Από την περίοδο λειτουργίας SSH, αντιγράψτε το αρχείο βάζο με το χώρο αποθήκευσης HDInsight.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>Χρησιμοποιήστε το UDF από ομάδα

1. Χρησιμοποιήστε τα ακόλουθα για να ξεκινήσετε το πρόγραμμα-πελάτη Beeline από την περίοδο λειτουργίας SSH.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Αυτή η εντολή προϋποθέτει ότι χρησιμοποιήσατε την προεπιλεγμένη της __διαχείρισης__ για το λογαριασμό σύνδεσης για το σύμπλεγμά σας.

2. Μόλις φτάνετε στη το `jdbc:hive2://localhost:10001/>` ερώτηση, πληκτρολογήστε τα εξής για να προσθέσετε το UDF σε ομάδα και εκτίθεται ως μια συνάρτηση.

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. Χρησιμοποιήστε το UDF για να μετατρέψετε τις τιμές που ανακτήθηκαν από έναν πίνακα σε πεζούς συμβολοσειρές.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    Αυτό θα επιλέξτε την πλατφόρμα συσκευή (Android, Windows, iOS, κ.λπ.) από τον πίνακα, μετατροπή της συμβολοσειράς σε πεζά και, στη συνέχεια, να τις εμφανίσετε. Το αποτέλεσμα θα είναι παρόμοιο με τα εξής.

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Επόμενα βήματα

Για άλλους τρόπους για να εργαστείτε με ομάδα, ανατρέξτε στο θέμα [Χρήση Hive με HDInsight](hdinsight-use-hive.md).

Για περισσότερες πληροφορίες σχετικά με τις συναρτήσεις Hive User-Defined, ανατρέξτε [Hive τελεστές και συναρτήσεις, ορίζονται από το χρήστη](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) στην ενότητα του wiki Hive στο apache.org.
