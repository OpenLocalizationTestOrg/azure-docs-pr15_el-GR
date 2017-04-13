<properties
 pageTitle="Ανάπτυξη εγκαύματα MapReduce έργα με Maven | Microsoft Azure"
 description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Maven για να δημιουργήσετε μια εργασία εγκαύματα MapReduce, στη συνέχεια, να αναπτύξετε και να εκτελέσετε την εργασία στην Hadoop σε σύμπλεγμα HDInsight."
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
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>Ανάπτυξη εγκαύματα MapReduce έργα με Apache Hadoop σε HDInsight

Εγκαύματα είναι μια βιβλιοθήκη Scala που σας διευκολύνει να δημιουργήσετε Hadoop MapReduce εργασίες. Παρέχει μια συνοπτική σύνταξη, καθώς και Ερμητική ενοποίηση με το Scala.

Σε αυτό το έγγραφο, μάθετε πώς μπορείτε να χρησιμοποιήσετε Maven για να δημιουργήσετε μια εργασία MapReduce πλήθος βασικές word γραμμένο σε Scalding. Στη συνέχεια, θα μάθετε πώς μπορείτε να αναπτύξετε και να εκτελέσετε αυτήν την εργασία σε ένα σύμπλεγμα HDInsight.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **A Windows ή Linux βάσει Hadoop σε σύμπλεγμα HDInsight**. Για περισσότερες πληροφορίες, ανατρέξτε [βάσει Linux παροχή Hadoop σε HDInsight](hdinsight-hadoop-provision-linux-clusters.md) ή [Hadoop σε HDInsight που βασίζεται σε Windows διάταξη](hdinsight-provision-clusters.md) .

* **[Maven](http://maven.apache.org/)**

* **[Πλατφόρμα Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 ή νεότερη έκδοση**

## <a name="create-and-build-the-project"></a>Δημιουργήσετε και να δομήσετε το έργο

1. Χρησιμοποιήστε την παρακάτω εντολή για να δημιουργήσετε ένα νέο έργο Maven:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    Αυτή η εντολή θα δημιουργήσετε έναν νέο κατάλογο με όνομα **scaldingwordcount**και δημιουργήστε το ικρίωμα για μια εφαρμογή Scala.

2. Στον κατάλογο **scaldingwordcount** , ανοίξτε το αρχείο **pom.xml** και να αντικαταστήσετε το περιεχόμενο με τα εξής:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
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
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Αυτό το αρχείο περιγράφει το έργο, εξαρτήσεις και προσθήκες. Εδώ θα βρείτε τις σημαντικές καταχωρήσεις:

    * **maven.Compiler.Source** και **maven.compiler.target**: Ορίζει την έκδοση Java για αυτό το έργο

    * **αποθετήρια**: το αποθετήρια που περιέχουν τα αρχεία εξάρτηση που χρησιμοποιούνται από αυτό το έργο

    * **εγκαύματα core_2.11** και **hadoop πυρήνα**: αυτό το έργο εξαρτάται από το Scalding και Hadoop πακέτων πυρήνα

    * **maven-scala-Προσθήκη**: Προσθήκη για να μεταγλωττίσετε scala εφαρμογές

    * **maven-σκιά-Προσθήκη**: Προσθήκη για να δημιουργήσετε με σκίαση πλατύστομα (fat). Αυτή η προσθήκη ισχύει φίλτρα και μετασχηματισμοί; specificially:

        * **φίλτρα**: τα φίλτρα που έχουν εφαρμοστεί να τροποποιήσετε τις πληροφορίες μετα περιλαμβάνεται στο αρχείο βάζο. Για να αποτρέψετε την είσοδο εξαιρέσεις κατά το χρόνο εκτέλεσης, εξαιρούνται τα διάφορα αρχεία υπογραφής που μπορούν να περιληφθούν με τις εξαρτήσεις.

        * **εκτελέσεις**: Η ρύθμιση παραμέτρων πακέτου φάση εκτέλεσης Καθορίζει την κλάση **com.twitter.scalding.Tool** ως η κύρια κλάση για το πακέτο. Χωρίς αυτό, θα πρέπει να καθορίσετε com.twitter.scalding.Tool, καθώς και την κλάση που περιέχει τη λογική εφαρμογής, κατά την εκτέλεση της εργασίας με την εντολή hadoop.

3. Διαγραφή του καταλόγου **src/έλεγχο** , όπως που θα δεν δημιουργούν δοκιμές με αυτό το παράδειγμα.

4. Ανοίξτε το αρχείο **src/main/scala/com/microsoft/example/App.scala** και να αντικαταστήσετε το περιεχόμενο με τα εξής:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    Αυτό υλοποιεί μια εργασία count βασικές word.

5. Αποθηκεύστε και κλείστε τα αρχεία.

6. Χρησιμοποιήστε την ακόλουθη εντολή από τον κατάλογο **scaldingwordcount** για να δημιουργήσετε και να συσκευασία της εφαρμογής:

        mvn package

    Μόλις ολοκληρωθεί αυτή η εργασία, μπορείτε να βρείτε το πακέτο που περιέχει την εφαρμογή WordCount στο **target/scaldingwordcount-1.0-SNAPSHOT.jar**.

## <a name="run-the-job-on-a-linux-based-cluster"></a>Εκτέλεση της εργασίας σε ένα σύμπλεγμα βάσει Linux

> [AZURE.NOTE] Τα παρακάτω βήματα χρησιμοποιούν SSH και την εντολή Hadoop. Για άλλες μεθόδους MapReduce εργασίες που εκτελούνται, ανατρέξτε στο θέμα [Χρήση MapReduce στο Hadoop σε HDInsight](hdinsight-use-mapreduce.md).

1. Χρησιμοποιήστε την παρακάτω εντολή για να φορτώσετε το πακέτο το σύμπλεγμά σας HDInsight:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    Αυτό αντιγράφει τα αρχεία από το τοπικό σύστημα στον κόμβο κεφαλίδας.

    > [AZURE.NOTE] Εάν χρησιμοποιούσατε έναν κωδικό πρόσβασης για την ασφάλιση SSH το λογαριασμό σας, θα σας ζητηθεί για τον κωδικό πρόσβασης. Εάν χρησιμοποιείτε ένα πλήκτρο SSH, ίσως χρειαστεί να χρησιμοποιήσετε το `-i` παράμετρο και τη διαδρομή προς το ιδιωτικό κλειδί. Για παράδειγμα,`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. Χρησιμοποιήστε την ακόλουθη εντολή για να συνδεθείτε με τον κόμβο κεφαλής συμπλέγματος:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Εάν χρησιμοποιούσατε έναν κωδικό πρόσβασης για την ασφάλιση SSH το λογαριασμό σας, θα σας ζητηθεί για τον κωδικό πρόσβασης. Εάν χρησιμοποιείτε ένα πλήκτρο SSH, ίσως χρειαστεί να χρησιμοποιήσετε το `-i` παράμετρο και τη διαδρομή προς το ιδιωτικό κλειδί. Για παράδειγμα,`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. Μόλις συνδεθεί στον κόμβο κεφαλίδας, χρησιμοποιήστε την παρακάτω εντολή για να εκτελέσετε την εργασία count word

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    Αυτή η ενέργεια εκτελεί την κλάση WordCount υλοποίηση νωρίτερα. `--hdfs`καθοδηγεί το έργο για να χρησιμοποιήσετε HDFS. `--input`Καθορίζει το αρχείο εισαγωγής κειμένου, ενώ `--output` Καθορίζει τη θέση εξόδου.

4. Αφού ολοκληρωθεί η εργασία, χρησιμοποιήστε τα παρακάτω για να προβάλετε την έξοδο.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Αυτό θα εμφανίζουν πληροφορίες που είναι παρόμοιο με το εξής:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>Εκτέλεση της εργασίας σε ένα σύμπλεγμα που βασίζεται σε Windows

Τα παρακάτω βήματα χρησιμοποιούν Windows PowerShell. Για άλλες μεθόδους MapReduce εργασίες που εκτελούνται, ανατρέξτε στο θέμα [Χρήση MapReduce στο Hadoop σε HDInsight](hdinsight-use-mapreduce.md).

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Ξεκινήστε Azure PowerShell και συνδεθείτε στο λογαριασμό σας στο Azure. Μετά την παροχή τα διαπιστευτήριά σας, η εντολή επιστρέφει πληροφορίες σχετικά με το λογαριασμό σας.

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. Εάν έχετε πολλές συνδρομές, δώστε το αναγνωριστικό συνδρομής που θέλετε να χρησιμοποιήσετε για ανάπτυξη.

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε `Get-AzureRMSubscription` για να λάβετε μια λίστα με όλες τις συνδρομές που σχετίζεται με το λογαριασμό σας, η οποία περιλαμβάνει τη συνδρομή αναγνωριστικό για κάθε μία.

4. Χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών για την αποστολή και να εκτελέσετε την εργασία WordCount. Αντικατάσταση `CLUSTERNAME` με το όνομα του σας HDInsight συμπλέγματος και βεβαιωθείτε ότι έχετε `$fileToUpload` είναι η σωστή διαδρομή προς το αρχείο __scaldingwordcount-1.0-SNAPSHOT.jar__ .

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
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
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     Όταν εκτελείτε τη δέσμη ενεργειών, θα σας ζητηθεί να εισαγάγετε το διαχειριστή όνομα χρήστη και κωδικό πρόσβασης για το σύμπλεγμά σας HDInsight. Τυχόν σφάλματα που προκύπτουν κατά την εκτέλεση του έργου θα καταγραφούν στην κονσόλα.
     
6. Μόλις ολοκληρωθεί η εργασία, το αποτέλεσμα θα λαμβάνονται για το αρχείο __output.txt__ στον τρέχοντα κατάλογο. Χρησιμοποιήστε την παρακάτω εντολή για να εμφανίσετε τα αποτελέσματα.

        cat output.txt

    Το αρχείο πρέπει να περιέχει τιμές που είναι παρόμοιο με το εξής:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε πώς να χρησιμοποιείτε το Scalding για να δημιουργήσετε MapReduce εργασίες για το HDInsight, χρησιμοποιήστε τις παρακάτω συνδέσεις για να εξερευνήσετε άλλους τρόπους για να εργαστείτε με Azure HDInsight.

* [Χρήση της ομάδας με το HDInsight](hdinsight-use-hive.md)

* [Χρήση γουρούνι με HDInsight](hdinsight-use-pig.md)

* [Χρήση MapReduce εργασίες με το HDInsight](hdinsight-use-mapreduce.md)
