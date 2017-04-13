## <a name="receive-messages-with-apache-storm"></a>Λήψη μηνυμάτων με Apache καταιγίδας

[**Καταιγίδας Apache**](https://storm.incubator.apache.org) είναι ένα σύστημα κατανέμεται σε πραγματικό χρόνο κατά τον υπολογισμό που απλοποιεί αξιόπιστη επεξεργασίας μη δεσμευμένες ροής δεδομένων. Αυτή η ενότητα δείχνει πώς μπορείτε να χρησιμοποιήσετε ένα συμβάν διανομείς καταιγίδας στομίου για λήψη συμβάντων από συμβάν διανομείς. Χρήση καταιγίδας Apache, μπορείτε να διαιρέσετε συμβάντα σε πολλές διεργασιών που φιλοξενούνται σε διαφορετικούς κόμβους. Η ενοποίηση διανομείς συμβάν με καταιγίδας απλοποιεί κατανάλωση συμβάντος, με διαφάνεια σημείων ελέγχου την πρόοδο του έργου με χρήση του καταιγίδας Zookeeper εγκατάστασης, τη Διαχείριση μόνιμη σημεία ελέγχου και παράλληλα λαμβάνει από συμβάν διανομείς.

Για περισσότερες πληροφορίες σχετικά με το συμβάν διανομείς λήψη μοτίβα, δείτε την [Επισκόπηση συμβάν διανομείς][].

Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί μια εγκατάσταση [Καταιγίδας HDInsight][] , η οποία παρέχεται με το συμβάν διανομείς στομίου ήδη διαθέσιμα.

1. Ακολουθήστε τη διαδικασία [Καταιγίδας HDInsight - γρήγορα αποτελέσματα](../articles/hdinsight/hdinsight-storm-overview.md) για να δημιουργήσετε ένα νέο σύμπλεγμα HDInsight, και συνδεθείτε μέσω απομακρυσμένης επιφάνειας εργασίας.

2. Αντιγραφή του `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` αρχείου για το περιβάλλον τοπικής ανάπτυξης. Περιέχει το στομίου καταιγίδας συμβάντα.

3. Χρησιμοποιήστε την παρακάτω εντολή για να εγκαταστήσετε το πακέτο στον τοπικό χώρο αποθήκευσης Maven. Αυτό σας επιτρέπει να προσθέσετε ως μια αναφορά του έργου καταιγίδας αργότερα.

        mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar

4. Στο Έκλειψη, δημιουργήστε ένα νέο έργο Maven (κάντε κλικ στην επιλογή **αρχείο**, στη συνέχεια, **Δημιουργία**, στη συνέχεια, **έργου**).

    ![][12]

5. Επιλέξτε **Χρήση προεπιλεγμένη θέση χώρου εργασίας**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**

6. Επιλέξτε το archetype **maven-archetype-γρήγορη έναρξη** και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**

7. Εισαγάγετε ένα **αναγνωριστικό ομάδας** και **ArtifactId**και, στη συνέχεια, κάντε κλικ στο κουμπί **Τέλος**

8. Στο **pom.xml**, προσθέστε τις ακόλουθες εξαρτήσεις στο το `<dependency>` κόμβο.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-core</artifactId>
            <version>0.9.2-incubating</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.microsoft.eventhubs</groupId>
            <artifactId>eventhubs-storm-spout</artifactId>
            <version>0.9</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>1.3.3</version>
            <exclusions>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>

9. Στο φάκελο **src** , δημιουργήστε ένα αρχείο που ονομάζεται **Config.properties** και αντιγράψτε το παρακάτω περιεχόμενο, αντικαθιστώντας τις ακόλουθες τιμές:

        eventhubspout.username = ReceiveRule

        eventhubspout.password = {receive rule key}

        eventhubspout.namespace = ioteventhub-ns

        eventhubspout.entitypath = {event hub name}

        eventhubspout.partitions.count = 16

        # if not provided, will use storm's zookeeper settings
        # zookeeper.connectionstring=localhost:2181

        eventhubspout.checkpoint.interval = 10

        eventhub.receiver.credits = 10

    Η τιμή για το **eventhub.receiver.credits** καθορίζει πόσες συμβάντα είναι μαζικής πριν από την κυκλοφορία τους της διοχέτευσης καταιγίδας. Για λόγους απλούστευσης, αυτό το παράδειγμα ορίζει αυτήν την τιμή 10. Παραγωγή, πρέπει να συνήθως είναι τιμή μεγαλύτερες τιμές. Για παράδειγμα, 1024.

10. Δημιουργία μιας νέας κλάσης που ονομάζεται **LoggerBolt** με τον ακόλουθο κώδικα:

        import java.util.Map;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import backtype.storm.task.OutputCollector;
        import backtype.storm.task.TopologyContext;
        import backtype.storm.topology.OutputFieldsDeclarer;
        import backtype.storm.topology.base.BaseRichBolt;
        import backtype.storm.tuple.Tuple;

        public class LoggerBolt extends BaseRichBolt {
            private OutputCollector collector;
            private static final Logger logger = LoggerFactory
                      .getLogger(LoggerBolt.class);

            @Override
            public void execute(Tuple tuple) {
                String value = tuple.getString(0);
                logger.info("Tuple value: " + value);

                collector.ack(tuple);
            }

            @Override
            public void prepare(Map map, TopologyContext context, OutputCollector collector) {
                this.collector = collector;
                this.count = 0;
            }

            @Override
            public void declareOutputFields(OutputFieldsDeclarer declarer) {
                // no output fields
            }

        }

    Αυτό κεραυνό καταιγίδας καταγράφει το περιεχόμενο των συμβάντων που λάβατε. Αυτό μπορεί εύκολα να επεκταθεί για να αποθηκεύσετε πλειάδων σε μια υπηρεσία αποθήκευσης. Το [πρόγραμμα εκμάθησης ανάλυσης αισθητήρα HDInsight] χρησιμοποιεί αυτήν την ίδια προσέγγιση για την αποθήκευση δεδομένων σε HBase.

11. Δημιουργία μιας κλάσης που ονομάζεται **LogTopology** με τον ακόλουθο κώδικα:

        import java.io.FileReader;
        import java.util.Properties;
        import backtype.storm.Config;
        import backtype.storm.LocalCluster;
        import backtype.storm.StormSubmitter;
        import backtype.storm.generated.StormTopology;
        import backtype.storm.topology.TopologyBuilder;
        import com.microsoft.eventhubs.samples.EventCount;
        import com.microsoft.eventhubs.spout.EventHubSpout;
        import com.microsoft.eventhubs.spout.EventHubSpoutConfig;

        public class LogTopology {
            protected EventHubSpoutConfig spoutConfig;
            protected int numWorkers;

            protected void readEHConfig(String[] args) throws Exception {
                Properties properties = new Properties();
                if (args.length > 1) {
                    properties.load(new FileReader(args[1]));
                } else {
                    properties.load(EventCount.class.getClassLoader()
                            .getResourceAsStream("Config.properties"));
                }

                String username = properties.getProperty("eventhubspout.username");
                String password = properties.getProperty("eventhubspout.password");
                String namespaceName = properties
                        .getProperty("eventhubspout.namespace");
                String entityPath = properties.getProperty("eventhubspout.entitypath");
                String zkEndpointAddress = properties
                        .getProperty("zookeeper.connectionstring"); // opt
                int partitionCount = Integer.parseInt(properties
                        .getProperty("eventhubspout.partitions.count"));
                int checkpointIntervalInSeconds = Integer.parseInt(properties
                        .getProperty("eventhubspout.checkpoint.interval"));
                int receiverCredits = Integer.parseInt(properties
                        .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                    // (opt)
                System.out.println("Eventhub spout config: ");
                System.out.println("  partition count: " + partitionCount);
                System.out.println("  checkpoint interval: "
                        + checkpointIntervalInSeconds);
                System.out.println("  receiver credits: " + receiverCredits);

                spoutConfig = new EventHubSpoutConfig(username, password,
                        namespaceName, entityPath, partitionCount, zkEndpointAddress,
                        checkpointIntervalInSeconds, receiverCredits);

                // set the number of workers to be the same as partition number.
                // the idea is to have a spout and a logger bolt co-exist in one
                // worker to avoid shuffling messages across workers in storm cluster.
                numWorkers = spoutConfig.getPartitionCount();

                if (args.length > 0) {
                    // set topology name so that sample Trident topology can use it as
                    // stream name.
                    spoutConfig.setTopologyName(args[0]);
                }
            }

            protected StormTopology buildTopology() {
                TopologyBuilder topologyBuilder = new TopologyBuilder();

                EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
                topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                        spoutConfig.getPartitionCount()).setNumTasks(
                        spoutConfig.getPartitionCount());
                topologyBuilder
                        .setBolt("LoggerBolt", new LoggerBolt(),
                                spoutConfig.getPartitionCount())
                        .localOrShuffleGrouping("EventHubsSpout")
                        .setNumTasks(spoutConfig.getPartitionCount());
                return topologyBuilder.createTopology();
            }

            protected void runScenario(String[] args) throws Exception {
                boolean runLocal = true;
                readEHConfig(args);
                StormTopology topology = buildTopology();
                Config config = new Config();
                config.setDebug(false);

                if (runLocal) {
                    config.setMaxTaskParallelism(2);
                    LocalCluster localCluster = new LocalCluster();
                    localCluster.submitTopology("test", config, topology);
                    Thread.sleep(5000000);
                    localCluster.shutdown();
                } else {
                    config.setNumWorkers(numWorkers);
                    StormSubmitter.submitTopology(args[0], config, topology);
                }
            }

            public static void main(String[] args) throws Exception {
                LogTopology topology = new LogTopology();
                topology.runScenario(args);
            }
        }


    Αυτή η κλάση δημιουργεί ένα νέο συμβάν διανομείς στομίου, χρησιμοποιώντας τις ιδιότητες στη ρύθμιση παραμέτρων αρχειοθετήστε instatiate το. Είναι σημαντικό να λάβετε υπόψη ότι αυτό το παράδειγμα δημιουργεί όσο το δυνατόν περισσότερες εργασίες spouts ως ο αριθμός των διαμερισμάτων στην ενότητα συμβάντων, για να χρησιμοποιήσετε τον μέγιστο παραλληλισμό επιτρέπεται από αυτόν το συμβάν διανομέα.

<!-- Links -->
[Επισκόπηση διανομείς συμβάντων]: ../articles/event-hubs/event-hubs-overview.md
[HDInsight καταιγίδας]: ../articles/hdinsight/hdinsight-storm-overview.md
[Πρόγραμμα εκμάθησης ανάλυσης αισθητήρα HDInsight]: ../articles/hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/service-bus-event-hubs-get-started-receive-storm/create-storm1.png