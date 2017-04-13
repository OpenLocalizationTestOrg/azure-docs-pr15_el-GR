<properties
pageTitle="Να δημιουργήσετε μια εφαρμογή HBase χρησιμοποιώντας Maven και ανάπτυξη με το HDInsight που βασίζεται σε Windows | Microsoft Azure"
description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Apache Maven για να δημιουργήσετε μια εφαρμογή βασισμένη σε Java HBase Apache και, στη συνέχεια, τον αναπτύσσετε σε ένα σύμπλεγμα βασίζεται σε Windows Azure HDInsight."
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
ms.date="10/03/2016"
ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Χρησιμοποιήστε Maven για να δημιουργήσετε εφαρμογές Java που χρησιμοποιούν HBase με HDInsight που βασίζεται σε Windows (Hadoop)

Μάθετε πώς μπορείτε να δημιουργήσετε και να δημιουργήσετε μια εφαρμογή [Apache HBase](http://hbase.apache.org/) σε Java με τη χρήση Apache Maven. Στη συνέχεια, χρησιμοποιήστε την εφαρμογή με το Azure HDInsight (Hadoop).

[Maven](http://maven.apache.org/) είναι ένα λογισμικό διαχείρισης έργων και την κατανόηση εργαλείο που σας επιτρέπει να δημιουργήσετε λογισμικό, τεκμηρίωση και τις αναφορές για τα έργα Java. Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια βασική εφαρμογή Java που που δημιουργεί, ερωτήματα, και διαγράφει ένα HBase πίνακα σε ένα σύμπλεγμα Azure HDInsight.

> [AZURE.NOTE] Τα βήματα σε αυτό το έγγραφο που λαμβάνεται ως δεδομένο ότι χρησιμοποιείτε ένα σύμπλεγμα HDInsight που βασίζεται στα Windows. Για πληροφορίες σχετικά με ένα σύμπλεγμα βάσει Linux HDInsight, ανατρέξτε στο θέμα [Χρήση Maven για να δημιουργήσετε εφαρμογές Java που χρησιμοποιούν HBase με Linux βάσει HDInsight](hdinsight-hbase-build-java-maven-linux.md)

##<a name="requirements"></a>Απαιτήσεις

* [Πλατφόρμα Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 ή νεότερη έκδοση

* [Maven](http://maven.apache.org/)


* [Ένα σύμπλεγμα HDInsight που βασίζεται σε Windows με HBase](hdinsight-hbase-tutorial-get-started.md#create-hbase-cluster)


    > [AZURE.NOTE] Τα βήματα σε αυτό το έγγραφο έχει ελεγχθεί με εκδόσεις σύμπλεγμα HDInsight 3.2 και 3.3. Οι προεπιλεγμένες τιμές που παρέχονται σε παραδείγματα είναι για ένα σύμπλεγμα HDInsight 3.3.

##<a name="create-the-project"></a>Δημιουργία έργου

1. Από τη γραμμή εντολών στο περιβάλλον ανάπτυξής σας, αλλάξτε τον κατάλογο στη θέση όπου θέλετε να δημιουργήσετε το έργο, για παράδειγμα, `cd code\hdinsight`.

2. Χρησιμοποιήστε την εντολή __mvn__ , η οποία έχει εγκατασταθεί με το Maven, για να δημιουργήσετε το ικρίωμα για το έργο.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Αυτή η εντολή δημιουργεί έναν κατάλογο στην τρέχουσα θέση, με το όνομα που καθορίζεται από την παράμετρο __artifactID__ (**hbaseapp** σε αυτό το παράδειγμα). Αυτός ο κατάλογος περιλαμβάνει τα ακόλουθα στοιχεία:

    * __pom.XML__: το μοντέλο αντικειμένου έργου ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) περιέχει πληροφορίες και τη ρύθμιση παραμέτρων λεπτομέρειες που χρησιμοποιούνται για τη δημιουργία του έργου.

    * __src__: Ο κατάλογος που περιέχει τον κατάλογο __main\java\com\microsoft\examples__ , όπου θα συντάσσετε την εφαρμογή.

3. Διαγράψτε το αρχείο __src\test\java\com\microsoft\examples\apptest.java__ επειδή δεν χρησιμοποιείται σε αυτό το παράδειγμα.

##<a name="update-the-project-object-model"></a>Ενημέρωση στο μοντέλο αντικειμένου έργου

1. Επεξεργαστείτε το αρχείο __pom.xml__ και προσθέστε τον ακόλουθο κώδικα στο εσωτερικό του `<dependencies>` την ενότητα:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Αυτή η ενότητα σας ενημερώνει για Maven ότι το έργο απαιτεί την έκδοση __προγράμματος-πελάτη το hbase__ __1.1.2__. Κατά τη μεταγλώττιση, αυτή την εξάρτηση γίνεται λήψη από το χώρο αποθήκευσης Maven προεπιλογή. Μπορείτε να χρησιμοποιήσετε την [Maven κεντρικό αποθετήριο αναζήτησης](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) για να μάθετε περισσότερα σχετικά με αυτήν την εξάρτηση.

    > [AZURE.IMPORTANT] Ο αριθμός έκδοσης πρέπει να συμφωνεί με την έκδοση του HBase που παρέχεται με το σύμπλεγμά σας HDInsight. Χρησιμοποιήστε τον παρακάτω πίνακα για να βρείτε τον αριθμό έκδοσης σωστά.

  	| Έκδοση HDInsight συμπλέγματος | Για να χρησιμοποιήσετε την έκδοση HBase |
  	| ----- | ----- |
  	| 3.2 | 0.98.4-hadoop2 |
  	| 3.3 | 1.1.2 |

    Για περισσότερες πληροφορίες σχετικά με HDInsight εκδόσεις και των στοιχείων, ανατρέξτε στο θέμα [Τι είναι τα διάφορα στοιχεία Hadoop διαθέσιμη με το HDInsight](hdinsight-component-versioning.md).

2. Εάν χρησιμοποιείτε ένα σύμπλεγμα HDInsight 3.3, πρέπει επίσης να προσθέσετε τα εξής για να το `<dependencies>` την ενότητα:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Αυτή η εξάρτηση θα φορτώσετε θήβα-τα βασικά στοιχεία του, που χρησιμοποιούνται από την έκδοση Hbase 1.1.x.

2. Προσθέστε τον ακόλουθο κώδικα στο αρχείο __pom.xml__ . Αυτή η ενότητα πρέπει να είναι εντός του `<project>...</project>` ετικέτες στο αρχείο, για παράδειγμα, μεταξύ `</dependencies>` και `</project>`.

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

    Το `<resources>` ενότητα ρυθμίζει τις παραμέτρους ενός πόρου (__conf\hbase site.xml__) που περιέχει πληροφορίες ρύθμισης παραμέτρων για HBase.

    > [AZURE.NOTE] Μπορείτε επίσης να ορίσετε τιμές παραμέτρων μέσω κώδικα. Δείτε τα σχόλια στο παράδειγμα __CreateTable__ που ακολουθεί για το πώς μπορείτε να το κάνετε αυτό.

    Αυτό `<plugins>` ενότητας ρυθμίζει την [Προσθήκη σκίαση Maven](http://maven.apache.org/plugins/maven-shade-plugin/)και [Προσθήκη προγράμματος μεταγλώττισης Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) . Το πρόγραμμα μεταγλώττισης προσθήκης χρησιμοποιείται για να μεταγλωττίσετε της τοπολογίας. Η προσθήκη σκίαση χρησιμοποιείται για να αποφευχθεί η αναπαραγωγή άδειας χρήσης στο πακέτο ΒΆΖΟ που δημιουργείται από Maven. Η αιτία είναι η επιλογή είναι ότι τα αρχεία διπλότυπων άδειας χρήσης προκαλούν σφάλμα κατά το χρόνο εκτέλεσης στο σύμπλεγμα HDInsight. Χρήση maven-σκιά-Προσθήκη με το `ApacheLicenseResourceTransformer` υλοποίηση αποτρέπει αυτό το σφάλμα.

    Η προσθήκη σκιάς maven δημιουργεί επίσης ένα uber βάζο (ή fat βάζο) που περιέχει όλες τις εξαρτήσεις που απαιτούνται από την εφαρμογή.

3. Αποθηκεύστε το αρχείο __pom.xml__ .

4. Δημιουργήστε έναν νέο κατάλογο που ονομάζεται __για διάσκεψη__ στον κατάλογο __hbaseapp__ . Στον κατάλογο __για διάσκεψη__ , δημιουργήστε ένα αρχείο με το όνομα __hbase site.xml__. Χρησιμοποιήστε τα παρακάτω ανάλογα με τα περιεχόμενα του αρχείου:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     http://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Αυτό το αρχείο θα χρησιμοποιηθεί για τη φόρτωση τη ρύθμιση παραμέτρων HBase για ένα σύμπλεγμα HDInsight.

    > [AZURE.NOTE] Αυτό είναι ένα αρχείο ελάχιστους hbase-site.xml και περιέχει τις ελάχιστες απολύτως ρυθμίσεις για το σύμπλεγμα HDInsight.

3. Αποθηκεύστε το αρχείο __hbase-site.xml__ .

##<a name="create-the-application"></a>Δημιουργία της εφαρμογής

1. Μεταβείτε στον κατάλογο __hbaseapp\src\main\java\com\microsoft\examples__ και μετονομάστε το αρχείο app.java σε __CreateTable.java__.

2. Ανοίξτε το αρχείο __CreateTable.java__ και να αντικαταστήσετε το υπάρχον περιεχόμενο με τον ακόλουθο κώδικα:

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
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

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

    Πρόκειται για την κλάση __CreateTable__ , η οποία θα δημιουργήσετε έναν πίνακα που ονομάζεται __άτομα__ και συμπληρώστε τον με ορισμένες προκαθορισμένες χρήστες.

3. Αποθηκεύστε το αρχείο __CreateTable.java__ .

4. Στον κατάλογο __hbaseapp\src\main\java\com\microsoft\examples__ , δημιουργήστε ένα νέο αρχείο με το όνομα __SearchByEmail.java__. Χρησιμοποιήστε τον ακόλουθο κώδικα με τα περιεχόμενα αυτού του αρχείου:

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

6. Στον κατάλογο __hbaseapp\src\main\hava\com\microsoft\examples__ , δημιουργήστε ένα νέο αρχείο με το όνομα __DeleteTable.java__. Χρησιμοποιήστε τον ακόλουθο κώδικα με τα περιεχόμενα αυτού του αρχείου:

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

1. Ανοίξτε μια γραμμή εντολών και αλλάξτε σε καταλόγους στον κατάλογο __hbaseapp__ .

2. Χρησιμοποιήστε την ακόλουθη εντολή για να δημιουργήσετε ένα αρχείο ΒΆΖΟ που περιέχει την εφαρμογή:

        mvn clean package

    Αυτό εκκαθαρίζει οποιαδήποτε προηγούμενη αντικείμενα Δόμηση, λήψεις τυχόν εξαρτήσεις που δεν έχετε ήδη εγκαταστήσει, στη συνέχεια, δημιουργεί και πακέτα της εφαρμογής.

3. Όταν ολοκληρωθεί η εντολή, ο κατάλογος __hbaseapp\target__ περιέχει ένα αρχείο που ονομάζεται __hbaseapp-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] Το αρχείο __hbaseapp-1.0-SNAPSHOT.jar__ είναι μια uber βάζο (μερικές φορές ονομάζεται ένα fat βάζο,) που περιέχει όλες τις εξαρτήσεις που απαιτούνται για την εκτέλεση της εφαρμογής.

##<a name="upload-the-jar-file-and-start-a-job"></a>Αποστείλετε το αρχείο ΒΆΖΟ και να ξεκινήσει μια εργασία

Υπάρχουν πολλοί τρόποι για να αποστείλετε ένα αρχείο για να το σύμπλεγμά σας HDInsight, όπως περιγράφεται στην [Αποστολή δεδομένων για τις εργασίες Hadoop στο HDInsight](hdinsight-upload-data.md). Τα παρακάτω βήματα χρησιμοποιούν Azure PowerShell.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


1. Μετά την εγκατάσταση και τη ρύθμιση των παραμέτρων του PowerShell Azure, δημιουργήστε ένα νέο αρχείο με το όνομα __hbase runner.psm1__. Χρησιμοποιήστε τα παρακάτω ανάλογα με τα περιεχόμενα αυτού του αρχείου:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasbs:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    Αυτό το αρχείο περιέχει δύο λειτουργικές μονάδες:

    * __Προσθήκη HDInsightFile__ - χρησιμοποιείται για την αποστολή αρχείων με το HDInsight

    * __Έναρξη-HBaseExample__ - χρησιμοποιείται για να εκτελέσετε τις κατηγορίες που δημιουργήσατε νωρίτερα

2. Αποθηκεύστε το αρχείο __hbase-runner.psm1__ .

3. Άνοιγμα νέου παραθύρου Azure PowerShell, αλλαγή σε καταλόγους στον κατάλογο __hbaseapp__ και, στη συνέχεια, εκτελέστε την ακόλουθη εντολή.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Αλλάξτε τη διαδρομή στη θέση του αρχείου __hbase runner.psm1__ που δημιουργήσατε νωρίτερα. Με αυτόν τον τρόπο τη λειτουργική μονάδα για αυτήν την περίοδο λειτουργίας Azure PowerShell.

2. Χρησιμοποιήστε την παρακάτω εντολή για να φορτώσετε το __hbaseapp-1.0-SNAPSHOT.jar__ το σύμπλεγμά σας HDInsight.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Αντικαταστήστε __hdinsightclustername__ με το όνομα του συμπλέγματος HDInsight. Η εντολή αποστέλλει το __hbaseapp-1.0-SNAPSHOT.jar__ στη θέση του __παράδειγμα/πλατύστομα__ στην κύρια αποθήκευσης για το σύμπλεγμά σας HDInsight.

3. Μετά την αποστολή των αρχείων, χρησιμοποιήστε τον ακόλουθο κώδικα για να δημιουργήσετε έναν πίνακα χρησιμοποιώντας το __hbaseapp__:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Αντικαταστήστε __hdinsightclustername__ με το όνομα του συμπλέγματος HDInsight.

    Αυτή η εντολή δημιουργεί ένα νέο πίνακα με το όνομα __άτομα__ στο σύμπλεγμά σας HDInsight. Αυτή η εντολή δεν εμφανίζεται κανένα αποτέλεσμα στο παράθυρο της κονσόλας.

2. Για να αναζητήσετε εγγραφές στον πίνακα, χρησιμοποιήστε την ακόλουθη εντολή:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Αντικαταστήστε __hdinsightclustername__ με το όνομα του συμπλέγματος HDInsight.

    Αυτή η εντολή χρησιμοποιεί την κλάση **SearchByEmail** για να αναζητήσετε οποιεσδήποτε γραμμές όπου η οικογένεια στήλη __contactinformation__ και τη στήλη __ηλεκτρονικού ταχυδρομείου__ , περιέχει τη συμβολοσειρά __contoso.com__. Πρέπει να λάβετε τα ακόλουθα αποτελέσματα:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Χρήση __fabrikam.com__ για το `-emailRegex` τιμή επιστρέφει τους χρήστες που έχουν __fabrikam.com__ στο πεδίο ηλεκτρονικού ταχυδρομείου. Επειδή η αναζήτηση αυτή εφαρμόζεται χρησιμοποιώντας κανονική παράσταση βάσει φίλτρο, μπορείτε επίσης να εισαγάγετε κανονικές εκφράσεις, όπως είναι οι __^ r__, ποιες εγγραφές επιστρέφει όπου το μήνυμα ηλεκτρονικού ταχυδρομείου αρχίζει με το γράμμα "r".

##<a name="delete-the-table"></a>Διαγραφή του πίνακα

Όταν τελειώσετε με το παράδειγμα, χρησιμοποιήστε την ακόλουθη εντολή από την περίοδο λειτουργίας Azure PowerShell για να διαγράψετε τον πίνακα __άτομα__ που χρησιμοποιούνται σε αυτό το παράδειγμα:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Αντικαταστήστε __hdinsightclustername__ με το όνομα του συμπλέγματος HDInsight.

##<a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

###<a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Χωρίς αποτελέσματα ή μη αναμενόμενα αποτελέσματα κατά τη χρήση HBaseExample έναρξης

Χρησιμοποιήστε το `-showErr` παραμέτρου για να προβάλετε το τυπικό σφάλμα (STDERR) που παράγεται κατά την εκτέλεση της εργασίας.
