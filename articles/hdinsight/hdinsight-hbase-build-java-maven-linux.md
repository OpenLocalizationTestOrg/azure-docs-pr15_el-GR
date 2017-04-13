<properties
    pageTitle="Δημιουργία μιας εφαρμογής HBase χρησιμοποιώντας Maven και Java και, στη συνέχεια, ανάπτυξη βάσει Linux HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Apache Maven για να δημιουργήσετε μια εφαρμογή βασισμένη σε Java HBase Apache και, στη συνέχεια, τον αναπτύσσετε σε HDInsight Linux βασίζεται στο Azure cloud."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Χρησιμοποιήστε Maven για να δημιουργήσετε εφαρμογές Java που χρησιμοποιούν HBase με HDInsight που βασίζεται στο Linux (Hadoop)

Μάθετε πώς μπορείτε να δημιουργήσετε και να δημιουργήσετε μια εφαρμογή [Apache HBase](http://hbase.apache.org/) σε Java με τη χρήση Apache Maven. Στη συνέχεια, χρησιμοποιήστε την εφαρμογή με ένα σύμπλεγμα βάσει Linux HDInsight.

[Maven](http://maven.apache.org/) είναι ένα λογισμικό διαχείρισης έργων και την κατανόηση εργαλείο που σας επιτρέπει να δομήσετε λογισμικό, τεκμηρίωση και τις αναφορές για τα έργα Java. Σε αυτό το άρθρο, θα μάθετε πώς να το χρησιμοποιήσετε για να δημιουργήσετε μια βασική εφαρμογή Java που που δημιουργεί, ερωτήματα, και διαγράφει ένα πίνακα HBase σε ένα σύμπλεγμα βάσει Linux HDInsight.

> [AZURE.NOTE] Τα βήματα σε αυτό το έγγραφο που λαμβάνεται ως δεδομένο ότι χρησιμοποιείτε ένα σύμπλεγμα βάσει Linux HDInsight. Για πληροφορίες σχετικά με ένα σύμπλεγμα HDInsight που βασίζεται στα Windows, ανατρέξτε στο θέμα [Χρήση Maven για να δημιουργήσετε εφαρμογές Java που χρησιμοποιούν HBase με HDInsight που βασίζεται σε Windows](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>Απαιτήσεις

* [Πλατφόρμα Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 ή νεότερη έκδοση

* [Maven](http://maven.apache.org/)

* [Ένα σύμπλεγμα βάσει Linux Azure HDInsight με HBase](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] Τα βήματα σε αυτό το έγγραφο έχει ελεγχθεί με εκδόσεις σύμπλεγμα HDInsight 3.2, 3.3 και 3.4. Οι προεπιλεγμένες τιμές που παρέχονται σε παραδείγματα είναι για ένα σύμπλεγμα HDInsight 3.4.

* **Εξοικείωση με SSH και SCP**. Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH και SCP με το HDInsight, ανατρέξτε στα παρακάτω:

    * **Προγράμματα-πελάτες του Linux, Unix ή OS X**: ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, OS X ή Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Προγράμματα-πελάτες των Windows**: ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

##<a name="create-the-project"></a>Δημιουργία έργου

1. Από τη γραμμή εντολών στο περιβάλλον ανάπτυξής σας, αλλάξτε τον κατάλογο στη θέση όπου θέλετε να δημιουργήσετε το έργο, για παράδειγμα, `cd code/hdinsight`.

2. Χρησιμοποιήστε την εντολή __mvn__ , η οποία έχει εγκατασταθεί με το Maven, για να δημιουργήσετε το ικρίωμα για το έργο.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Αυτό δημιουργεί ένα νέο κατάλογο του τρέχοντος καταλόγου, με το όνομα που καθορίζεται από την παράμετρο __artifactID__ (**hbaseapp** σε αυτό το παράδειγμα). Αυτός ο κατάλογος θα περιέχει τα ακόλουθα στοιχεία:

    * __pom.XML__: το μοντέλο αντικειμένου έργου ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) περιέχει πληροφορίες και τη ρύθμιση παραμέτρων λεπτομέρειες που χρησιμοποιούνται για τη δημιουργία του έργου.

    * __src__: Ο κατάλογος που περιέχει τον κατάλογο __κύριες/java/com/microsoft/παραδείγματα__ , όπου θα συντάσσετε την εφαρμογή.

3. Διαγράψτε το αρχείο __src/test/java/com/microsoft/examples/apptest.java__ , επειδή δεν θα να χρησιμοποιηθεί σε αυτό το παράδειγμα.

##<a name="update-the-project-object-model"></a>Ενημέρωση στο μοντέλο αντικειμένου έργου

1. Επεξεργαστείτε το αρχείο __pom.xml__ και προσθέστε τον ακόλουθο κώδικα στο εσωτερικό του `<dependencies>` την ενότητα:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Αυτό σας ενημερώνει για Maven ότι το έργο απαιτεί την έκδοση __προγράμματος-πελάτη το hbase__ __1.1.2__. Κατά τη μεταγλώττιση, αυτό θα λαμβάνονται από το χώρο αποθήκευσης Maven προεπιλογή. Μπορείτε να χρησιμοποιήσετε την [Maven κεντρικό αποθετήριο αναζήτησης](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) για να μάθετε περισσότερα σχετικά με αυτήν την εξάρτηση.

    > [AZURE.IMPORTANT] Ο αριθμός έκδοσης πρέπει να συμφωνεί με την έκδοση του HBase που παρέχεται με το σύμπλεγμά σας HDInsight. Χρησιμοποιήστε τον παρακάτω πίνακα για να βρείτε τον αριθμό έκδοσης σωστά.

  	| Έκδοση HDInsight συμπλέγματος | Για να χρησιμοποιήσετε την έκδοση HBase |
  	| ----- | ----- |
  	| 3.2 | 0.98.4-hadoop2 |
  	| 3.3 και 3.4 | 1.1.2 |

    Για περισσότερες πληροφορίες σχετικά με HDInsight εκδόσεις και των στοιχείων, ανατρέξτε στο θέμα [Τι είναι τα διάφορα στοιχεία Hadoop διαθέσιμη με το HDInsight](hdinsight-component-versioning.md).

2. Εάν χρησιμοποιείτε μια 3.3 HDInsight ή 3.4 σύμπλεγμα, πρέπει επίσης να προσθέσετε τα εξής για να το `<dependencies>` την ενότητα:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Αυτό θα φορτώσετε τα θήβα-βασικά στοιχεία, τα οποία απαιτούνται με την έκδοση Hbase 1.1.x.

2. Προσθέστε τον ακόλουθο κώδικα στο αρχείο __pom.xml__ . Πρέπει να είναι εντός του `<project>...</project>` ετικέτες στο αρχείο, για παράδειγμα, μεταξύ `</dependencies>` και `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
            <resource>
              <directory>${basedir}/conf</directory>
              <filtering>false</filtering>
              <includes>
                <include>hbase-site.xml</include>
              </includes>
            </resource>
          </resources>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
                        <version>3.3</version>
              <configuration>
                <source>1.7</source>
                <target>1.7</target>
              </configuration>
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

    Αυτό ρυθμίζει τις παραμέτρους ενός πόρου (__για διάσκεψη/hbase-site.xml__), που περιέχει πληροφορίες ρύθμισης παραμέτρων για HBase.

    > [AZURE.NOTE] Μπορείτε επίσης να ορίσετε τιμές παραμέτρων μέσω κώδικα. Δείτε τα σχόλια στο παράδειγμα __CreateTable__ που ακολουθεί για το πώς μπορείτε να το κάνετε αυτό.

    Αυτό ρυθμίζει επίσης την [Προσθήκη σκίαση Maven](http://maven.apache.org/plugins/maven-shade-plugin/)και [Προσθήκη προγράμματος μεταγλώττισης Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) . Το πρόγραμμα μεταγλώττισης προσθήκης χρησιμοποιείται για να μεταγλωττίσετε της τοπολογίας. Η προσθήκη σκίαση χρησιμοποιείται για να αποφευχθεί η αναπαραγωγή άδειας χρήσης στο πακέτο ΒΆΖΟ που δημιουργείται από Maven. Η αιτία είναι η επιλογή είναι ότι τα αρχεία διπλότυπων άδειας χρήσης προκαλούν σφάλμα κατά το χρόνο εκτέλεσης στο σύμπλεγμα HDInsight. Χρήση maven-σκιά-Προσθήκη με το `ApacheLicenseResourceTransformer` υλοποίηση αποτρέπει αυτό το σφάλμα.

    Η προσθήκη σκιάς maven παράγει επίσης ένα uber βάζο (ή fat βάζο,) που περιέχει όλες τις εξαρτήσεις που απαιτούνται από την εφαρμογή.

3. Αποθηκεύστε το αρχείο __pom.xml__ .

4. Δημιουργήστε έναν νέο κατάλογο που ονομάζεται __για διάσκεψη__ στον κατάλογο __hbaseapp__ . Αυτό θα χρησιμοποιηθεί για τη διατήρηση πληροφοριών ρύθμισης παραμέτρων για τη σύνδεση στο HBase.

5. Χρησιμοποιήστε την παρακάτω εντολή για να αντιγράψετε τη ρύθμιση παραμέτρων HBase από το διακομιστή HDInsight στον κατάλογο __για διάσκεψη__ . Αντικαταστήστε το **όνομα ΧΡΉΣΤΗ** του το όνομα της σύνδεσής σας SSH. Αντικαταστήστε το **CLUSTERNAME** με το όνομα συμπλέγματος HDInsight σας:

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] Εάν χρησιμοποιούσατε έναν κωδικό πρόσβασης για το λογαριασμό σας SSH, θα σας ζητηθεί να εισαγάγετε τον κωδικό πρόσβασης. Εάν χρησιμοποιείτε ένα πλήκτρο SSH με το λογαριασμό, ίσως χρειαστεί να χρησιμοποιήσετε το `-i` παραμέτρου για να καθορίσετε τη διαδρομή προς το αρχείο κλειδιού. Το παρακάτω παράδειγμα θα φορτώσετε το ιδιωτικό κλειδί από `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>Δημιουργία της εφαρμογής

1. Μεταβείτε στον κατάλογο __hbaseapp/src/κύριες/java/com/microsoft/παραδείγματα__ και να μετονομάσετε το αρχείο app.java σε __CreateTable.java__.

2. Ανοίξτε το αρχείο __CreateTable.java__ και να αντικαταστήσετε το υπάρχον περιεχόμενο με τα εξής:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    Αυτή είναι η κλάση __CreateTable__ , το οποίο θα δημιουργήσετε έναν πίνακα που ονομάζεται __άτομα__ και συμπληρώστε τον με ορισμένες προκαθορισμένες χρήστες.

3. Αποθηκεύστε το αρχείο __CreateTable.java__ .

4. Στον κατάλογο __hbaseapp/src/κύριες/java/com/microsoft/παραδείγματα__ , δημιουργήστε ένα νέο αρχείο με το όνομα __SearchByEmail.java__. Χρησιμοποιήστε τα παρακάτω ανάλογα με τα περιεχόμενα αυτού του αρχείου:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    Η κλάση __SearchByEmail__ μπορεί να χρησιμοποιηθεί σε ερώτημα για γραμμές με τη διεύθυνση ηλεκτρονικού ταχυδρομείου. Επειδή χρησιμοποιεί μια κανονική παράσταση φίλτρου, μπορείτε να δώσετε μια συμβολοσειρά ή μια κανονική έκφραση όταν χρησιμοποιείτε την κλάση.

5. Αποθηκεύστε το αρχείο __SearchByEmail.java__ .

6. Στον κατάλογο __hbaseapp/src/κύριες/hava/com/microsoft/παραδείγματα__ , δημιουργήστε ένα νέο αρχείο με το όνομα __DeleteTable.java__. Χρησιμοποιήστε τα παρακάτω ανάλογα με τα περιεχόμενα αυτού του αρχείου:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    Αυτή η κατηγορία είναι για την εκκαθάριση αυτό το παράδειγμα, απενεργοποίηση και απόθεση τον πίνακα που δημιουργήθηκε από την κλάση __CreateTable__ .

7. Αποθηκεύστε το αρχείο __DeleteTable.java__ .

##<a name="build-and-package-the-application"></a>Δημιουργία και συσκευασία της εφαρμογής

2. Από τον κατάλογο __hbaseapp__ , χρησιμοποιήστε την ακόλουθη εντολή για να δημιουργήσετε ένα αρχείο ΒΆΖΟ που περιέχει την εφαρμογή:

        mvn clean package

    Αυτό εκκαθαρίζει οποιαδήποτε προηγούμενη αντικείμενα Δόμηση, λήψεις τυχόν εξαρτήσεις που δεν έχετε ήδη εγκαταστήσει, στη συνέχεια, δημιουργεί και πακέτα της εφαρμογής.

3. Όταν ολοκληρωθεί η εντολή, ο κατάλογος __hbaseapp/προορισμού__ θα περιέχει ένα αρχείο που ονομάζεται __hbaseapp-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] Το αρχείο __hbaseapp-1.0-SNAPSHOT.jar__ είναι μια uber βάζο (μερικές φορές ονομάζεται ένα fat βάζο,) που περιέχει όλες τις εξαρτήσεις που απαιτούνται για την εκτέλεση της εφαρμογής.

##<a name="upload-the-jar-file-and-run-jobs"></a>Αποστείλετε το αρχείο ΒΆΖΟ και να εκτελέσετε εργασίες

1. Χρησιμοποιήστε τα ακόλουθα για να αποστείλετε το βάζο στο σύμπλεγμα HDInsight. Αντικαταστήστε το **όνομα ΧΡΉΣΤΗ** του το όνομα της σύνδεσής σας SSH. Αντικαταστήστε το **CLUSTERNAME** με το όνομα συμπλέγματος HDInsight σας:

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Αυτό θα αποστείλετε το αρχείο στον κεντρικό κατάλογο για το λογαριασμό χρήστη σας SSH.

    > [AZURE.NOTE] Εάν χρησιμοποιούσατε έναν κωδικό πρόσβασης για το λογαριασμό σας SSH, θα σας ζητηθεί να εισαγάγετε τον κωδικό πρόσβασης. Εάν χρησιμοποιείτε ένα πλήκτρο SSH με το λογαριασμό, ίσως χρειαστεί να χρησιμοποιήσετε το `-i` παραμέτρου για να καθορίσετε τη διαδρομή προς το αρχείο κλειδιού. Το παρακάτω παράδειγμα θα φορτώσετε το ιδιωτικό κλειδί από `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. Χρησιμοποιήστε SSH για να συνδεθείτε με το σύμπλεγμα HDInsight. Αντικαταστήστε το **όνομα ΧΡΉΣΤΗ** του το όνομα της σύνδεσής σας SSH. Αντικαταστήστε το **CLUSTERNAME** με το όνομα συμπλέγματος HDInsight σας:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] Εάν χρησιμοποιούσατε έναν κωδικό πρόσβασης για το λογαριασμό σας SSH, θα σας ζητηθεί να εισαγάγετε τον κωδικό πρόσβασης. Εάν χρησιμοποιείτε ένα πλήκτρο SSH με το λογαριασμό, ίσως χρειαστεί να χρησιμοποιήσετε το `-i` παραμέτρου για να καθορίσετε τη διαδρομή προς το αρχείο κλειδιού. Το παρακάτω παράδειγμα θα φορτώσετε το ιδιωτικό κλειδί από `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Μόλις συνδεθεί, χρησιμοποιήστε τα ακόλουθα για να δημιουργήσετε έναν νέο πίνακα HBase χρησιμοποιώντας την εφαρμογή Java:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    Αυτό θα δημιουργήσετε έναν νέο πίνακα HBase που ονομάζεται __άτομα__και συμπληρώστε τον με τα δεδομένα.

4. Στη συνέχεια, χρησιμοποιήστε τα ακόλουθα για να πραγματοποιήσετε αναζήτηση για διευθύνσεις ηλεκτρονικού ταχυδρομείου που είναι αποθηκευμένα στον πίνακα:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    Πρέπει να λάβετε τα ακόλουθα αποτελέσματα:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>Διαγραφή του πίνακα

Όταν τελειώσετε με το παράδειγμα, χρησιμοποιήστε την ακόλουθη εντολή από την περίοδο λειτουργίας Azure PowerShell για να διαγράψετε τον πίνακα __άτομα__ που χρησιμοποιούνται σε αυτό το παράδειγμα:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

