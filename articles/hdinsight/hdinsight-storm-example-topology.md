<properties
 pageTitle="Παράδειγμα καταιγίδας Apache τοπολογίες στην HDInsight | Microsoft Azure"
 description="Μια λίστα με παράδειγμα καταιγίδας τοπολογίες δημιουργούνται και ελεγχθεί με καταιγίδας Apache στην HDInsight συμπεριλαμβανομένων βασικές τοπολογίες C# και Java και εργασία με διανομείς συμβάν."
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

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Παράδειγμα καταιγίδας toplogies και των στοιχείων για Apache καταιγίδας στην HDInsight

Ακολουθεί μια λίστα με παραδείγματα δημιουργούνται και διατηρούνται από τη Microsoft για χρήση με καταιγίδας Apache στην HDInsight. Αυτά τα παραδείγματα καλύπτει μια ποικιλία θεμάτων, από τη δημιουργία βασικών C# και τοπολογίες Java με την εργασία με Azure υπηρεσίες όπως το συμβάν διανομείς, DocumentDB, Power BI, βάση δεδομένων SQL, HBase HDInsight και αποθήκευσης Azure. Ορισμένα παραδείγματα επίσης δείχνουν πώς μπορείτε να εργαστείτε με τις τεχνολογίες μη Azure ή ακόμα και δεν ανήκουν στη Microsoft, όπως SignalR και Socket.IO

| Περιγραφή                                                                                             | Παρουσιάζει                                         | Γλώσσα/Framework         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Εγγραφή στο χώρο αποθήκευσης λίμνης δεδομένων Azure από Apache καταιγίδας](hdinsight-storm-write-data-lake-store.md) | Εγγραφή στο χώρο αποθήκευσης λίμνης δεδομένων Azure | Java |
| [Προέλευση συμβάντος Spout διανομέας και κεραυνό](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Αρχείο προέλευσης για το συμβάν διανομέα στομίου και κεραυνό | Java |
| [Ανάπτυξη βάσει Java τοπολογίες για Apache καταιγίδας στην HDInsight][5797064f]                                 | Maven                                                | Java                       |
| [Ανάπτυξη C# τοπολογίες για Apache καταιγίδας στη χρήση του Visual Studio HDInsight][16fce2d1]                     | Εργαλεία HDInsight για το Visual Studio                    | C#, Java                   |
| [Δημιουργία πολλών ροών δεδομένων σε μια τοπολογία C# καταιγίδας][ec5a4064]                                         | Πολλών ροών                                     | C#                         |
| [Καθορισμός Twitter τάσης θέματα με καταιγίδας στην HDInsight][3c86c7c8]                                   | Trident                                              | Java, Trident              |
| [Διαδικασία συμβάντα από διανομείς συμβάν Azure με καταιγίδας στην HDInsight (C#)][844d1d81]                                | Διανομείς συμβάντος                                           | C# και Java                |
| [Διαδικασία συμβάντα από διανομείς συμβάν Azure με καταιγίδας στην HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) | Διανομείς συμβάντος | Java |
| [Χρήση του Power Bi για την απεικόνιση δεδομένων από μια τοπολογία καταιγίδας][94d15238]                              | Power BI                                             | C#                         |
| [Την ανάλυση δεδομένων αισθητήρα με καταιγίδας και HBase στο HDInsight][ab894747]                                     | Συμβάν διανομείς, HBase, Socket.IO, Web πίνακα εργαλείων          | C#, Java, JavaScript, HTML |
| [Επεξεργασία δεδομένων αισθητήρα οχήματος από διανομείς συμβάν χρησιμοποιώντας καταιγίδας στην HDInsight][246ee964]                        | Συμβάν διανομείς, DocumentDb, Azure χώρο αποθήκευσης αντικειμένων Blob (WASB)    | C#, Java                   |
| [Εξαγωγή, μετασχηματισμού και φόρτωση (ETL) από το Azure συμβάν διανομείς να HBase, χρησιμοποιώντας καταιγίδας στην HDInsight][b4b68194] | Συμβάν διανομείς, HBase                                    | C#                         |
| [Πρότυπο C# καταιγίδας τοπολογία έργου για την εργασία με τις υπηρεσίες του Azure από καταιγίδας στην HDInsight][ce0c02a2]  | Συμβάν διανομείς, DocumentDb, SQL βάσης δεδομένων, HBase, SignalR | C#, Java                   |
| [Κλιμάκωση σημεία αναφοράς για την ανάγνωση από το Azure διανομείς συμβάν χρησιμοποιώντας καταιγίδας στην HDInsight][d6c540e3]           | Μετάδοση μηνυμάτων, διανομείς συμβάντος, βάση δεδομένων SQL         | C#, Java                   |
| [Συσχετισμός συμβάντα χρήση καταιγίδας και HBase σε HDInsight](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [Χρήση Python με καταιγίδας στην HDInsight](hdinsight-storm-develop-python-topology.md) | Στοιχεία Python με Java και Clojure βάσει τοπολογίες καταιγίδας | Python |

## <a name="next-steps"></a>Επόμενα βήματα

* [Γρήγορα αποτελέσματα με το καταιγίδας Apache στην HDInsight][2b8c3488]

* [Μάθετε πώς μπορείτε να αναπτύξετε και να διαχειριστείτε τοπολογίες καταιγίδας με καταιγίδας στην HDInsight][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Μάθετε τον τρόπο δημιουργίας μιας καταιγίδας σε σύμπλεγμα HDInsight και χρήση του πίνακα εργαλείων καταιγίδας για την ανάπτυξη τοπολογίες παράδειγμα."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Μάθετε πώς μπορείτε να αναπτύξετε και να διαχειριστείτε τοπολογίες χρησιμοποιώντας τον πίνακα εργαλείων καταιγίδας που βασίζεται στο web και καταιγίδας περιβάλλοντος εργασίας Χρήστη ή τα εργαλεία HDInsight για το Visual Studio."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Μάθετε πώς μπορείτε να δημιουργήσετε τοπολογίες C# καταιγίδας, χρησιμοποιώντας τα εργαλεία HDInsight για το Visual Studio."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Μάθετε πώς να δημιουργείτε τοπολογίες καταιγίδας σε Java, χρησιμοποιώντας Maven, δημιουργώντας μια τοπολογία βασικές wordcount."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Παρουσιάζει πώς μπορείτε να εγγραφή δεδομένων στο Power BI από μια τοπολογία C# και, στη συνέχεια, δημιουργήστε ένα γράφημα και πίνακα εργαλείων από τα δεδομένα."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Παρουσιάζει μια βασική τοπολογία καταιγίδας που εκτελεί μια wordcount, υλοποιηθεί σε C#. Αυτό δείχνει επίσης πώς μπορείτε να δημιουργήσετε πολλές ροές δεδομένων μέσα σε μια τοπολογία C#."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Μάθετε πώς να ανάγνωση και εγγραφή δεδομένων από το Azure συμβάν διανομείς με καταιγίδας στην HDInsight."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Μάθετε πώς να χρησιμοποιείτε καταιγίδας Apache στην HDInsight να επεξεργάζονται αισθητήρα δεδομένα από το Azure συμβάν διανομείς, οπτικοποίηση χρησιμοποιώντας D3.js και (προαιρετικά), αποθηκεύστε το σε HBase."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "Μάθετε πώς μπορείτε να χρησιμοποιήσετε Trident για να δημιουργήσετε μια τοπολογία καταιγίδας που καθορίζει τάσης θέματα (βάσει hashtags,) στο Twitter."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Μάθετε πώς μπορείτε να χρησιμοποιήσετε μια τοπολογία καταιγίδας για την ανάγνωση μηνυμάτων από το Azure συμβάν διανομείς, ανάγνωση εγγράφων από το Azure DocumentDB για αναφορά σε δεδομένα και αποθήκευση δεδομένων στο χώρο αποθήκευσης Azure."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Διάφορες τοπολογίες για μια επίδειξη μετάδοσης κατά την ανάγνωση από διανομείς συμβάν Azure και την αποθήκευση στη βάση δεδομένων SQL χρησιμοποιώντας καταιγίδας Apache στην HDInsight."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Μάθετε πώς μπορείτε να την ανάγνωση δεδομένων από διανομείς συμβάν Azure, συγκεντρωτικών αποτελεσμάτων και Μετασχηματισμός των δεδομένων και, στη συνέχεια, αποθηκεύστε το σε HBase σε HDInsight."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Αυτό το έργο περιέχει πρότυπα για spouts, στοιχεία και τοπολογίες για να αλληλεπιδράσετε με διάφορες Azure υπηρεσίες όπως το συμβάν διανομείς, DocumentDB και βάση δεδομένων SQL."
 
