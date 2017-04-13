<properties
    pageTitle="Επεξεργασία των μηνυμάτων συσκευή-cloud IoT διανομέα (Java) | Microsoft Azure"
    description="Παρακολουθήστε αυτήν την εκμάθηση Java για να μάθετε χρήσιμες μοτίβα για την επεξεργασία μηνυμάτων συσκευή-cloud IoT διανομέα."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/01/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-java"></a>Πρόγραμμα εκμάθησης: Πώς να επεξεργαστεί διανομέα IoT μηνύματα συσκευής στο cloud χρησιμοποιώντας Java

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Εισαγωγή

Azure IoT διανομέα είναι μια πλήρως διαχειριζόμενη υπηρεσία που επιτρέπει την αξιόπιστη και ασφαλής διπλής κατεύθυνσης επικοινωνιών μεταξύ εκατομμύρια IoT συσκευές και μια εφαρμογή, δημιουργήστε αντίγραφα τέλος. Άλλα προγράμματα εκμάθησης ([Γρήγορα αποτελέσματα με το Κέντρο IoT] και [Αποστολή μηνυμάτων cloud-σε-συσκευή με το Κέντρο IoT][lnk-c2d]) σας δείξουν πώς μπορείτε να χρησιμοποιήσετε τη βασική συσκευή στο cloud και cloud-σε-συσκευή ανταλλαγής μηνυμάτων λειτουργικότητα IoT διανομέα.

Αυτό το πρόγραμμα εκμάθησης δημιουργεί στην τον κώδικα που εμφανίζεται στην εκμάθηση [Γρήγορα αποτελέσματα με το Κέντρο IoT] και που εμφανίζει δύο μεταβλητού μεγέθους μοτίβα που μπορείτε να χρησιμοποιήσετε για την επεξεργασία μηνυμάτων συσκευής στο cloud:

- Αξιόπιστη χώρου αποθήκευσης των μηνυμάτων συσκευής στο cloud στο [χώρο αποθήκευσης αντικειμένων blob του Azure]. Ένα συνηθισμένο σενάριο είναι *ψυχρές διαδρομή* ανάλυσης, στην οποία μπορείτε να αποθηκεύσετε δεδομένα τηλεμετρίας σε αντικείμενα BLOB για να χρησιμοποιήσετε ως είσοδο σε διεργασίες ανάλυσης. Αυτές οι διεργασίες μπορεί να ελέγχεται με εργαλεία όπως το [Azure εργοστασίου δεδομένων] ή στη στοίβα [HDInsight (Hadoop)] .

- Αξιόπιστη επεξεργασία *αλληλεπιδραστικών* μηνύματα συσκευής στο cloud. Συσκευή-cloud μηνυμάτων είναι αλληλεπιδραστικές όταν είναι άμεση εναύσματα για ένα σύνολο ενεργειών σε εφαρμογή παρασκηνίου. Για παράδειγμα, μια συσκευή ενδέχεται να σας στείλει ένα μήνυμα προειδοποίησης που αποτελεί έναυσμα για εισαγωγή δελτίου σε ένα σύστημα CRM. Αντίθετα, τα μηνύματα *σημείο δεδομένων* απλώς τροφοδοσίας σε μια μηχανισμός ανάλυσης που βρίσκεται. Για παράδειγμα, τηλεμετρίας θερμοκρασίας από μια συσκευή που πρέπει να αποθηκευτεί για μελλοντική ανάλυση είναι ένα μήνυμα σημείο δεδομένων.

Επειδή διανομέα IoT εκθέτει ένα [Συμβάν διανομέα][lnk-event-hubs]-συμβατά τελικό σημείο για να λάβετε μηνύματα συσκευής στο cloud, αυτό χρησιμοποιεί προγραμμάτων εκμάθησης μια παρουσία [EventProcessorHost] . Αυτή η παρουσία:

* Αξιόπιστα αποθηκεύει μηνύματα *σημείο δεδομένων* στο χώρο αποθήκευσης αντικειμένων blob του Azure.
* Προωθεί *αλληλεπιδραστικών* συσκευή-cloud μηνύματα σε μια Azure [Bus υπηρεσίας ουράς] για άμεση επεξεργασία.

Υπηρεσία Bus βοηθά να εξασφαλίσετε αξιόπιστη επεξεργασία των αλληλεπιδραστικών μηνυμάτων, όπως παρέχει σημεία ανά μήνυμα ελέγχου και ώρα που βασίζεται στο παράθυρο άρσεως αναπαραγωγής.

> [AZURE.NOTE] Μια παρουσία **EventProcessorHost** είναι μόνο ένας τρόπος για την επεξεργασία αλληλεπιδραστικών μηνυμάτων. Άλλες επιλογές περιλαμβάνουν [Azure Service ύφασμα] [ lnk-service-fabric] και τις [Αναλύσεις ροή Azure][lnk-stream-analytics].

Στο τέλος αυτού του προγράμματος εκμάθησης, μπορείτε να εκτελέσετε τρεις εφαρμογές κονσόλας Java:

* **προσομοιωμένη συσκευής**, μια τροποποιημένη έκδοση της εφαρμογής που έχουν δημιουργηθεί με το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με το Κέντρο IoT] , στέλνει μηνύματα συσκευή-cloud σημείο δεδομένων κάθε δεύτερη και αλληλεπιδραστικών συσκευής στο cloud μηνύματα κάθε 10 δευτερόλεπτα. Αυτή η εφαρμογή χρησιμοποιεί το πρωτόκολλο AMQP για να επικοινωνήσετε με το Κέντρο IoT.
* **διαδικασία-d2c-μηνύματα** χρησιμοποιεί την κλάση [EventProcessorHost] για να ανακτήσετε μηνύματα από το τελικό σημείο διανομέα συμβατό με το συμβάν. Το, στη συνέχεια, αξιόπιστα αποθηκεύει μηνύματα σημείο δεδομένων στο χώρο αποθήκευσης αντικειμένων blob του Azure και προωθεί αλληλεπιδραστικών μηνύματα σε μια ουρά Bus υπηρεσίας.
* **διαδικασία αλληλεπιδραστικών μηνύματα** ουρές καταργήστε την το διαδραστικό μηνυμάτων από την υπηρεσία Bus ουρά.

> [AZURE.NOTE] Ο διανομέας IoT διαθέτει SDK υποστήριξη για πολλές πλατφόρμες συσκευών και γλώσσες, συμπεριλαμβανομένων των C, Java και JavaScript. Για οδηγίες σχετικά με τον τρόπο αντικατάστασης προσομοιωμένη συσκευή σε αυτό το πρόγραμμα εκμάθησης με μια φυσική συσκευή και πώς μπορείτε να συνδέσετε συσκευές με ένα διανομέα IoT, ανατρέξτε στο [Κέντρο για προγραμματιστές του Azure IoT].

Αυτό το πρόγραμμα εκμάθησης εφαρμόζεται απευθείας σε άλλους τρόπους για την εκμετάλλευση μηνυμάτων συμβατό με το Κέντρο συμβάν, όπως το [HDInsight (Hadoop)] έργα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές διανομέα IoT Azure - συσκευή στο cloud].

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

+ Μια ολοκληρωμένη έκδοση εργασίας του προγράμματος εκμάθησης [Γρήγορα αποτελέσματα με το Κέντρο IoT] .

+ Java SE 8. <br/> [Προετοιμασία περιβάλλον ανάπτυξής σας] [ lnk-dev-setup] περιγράφει τον τρόπο εγκατάστασης Java για αυτό το πρόγραμμα εκμάθησης στα Windows ή Linux.

+ Maven 3.  <br/> [Προετοιμασία περιβάλλον ανάπτυξής σας] [ lnk-dev-setup] περιγράφει τον τρόπο εγκατάστασης Maven για αυτό το πρόγραμμα εκμάθησης στα Windows ή Linux.

+ Λογαριασμού Azure active. <br/>Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/free/) σε λίγα λεπτά.

Θα πρέπει να έχετε ορισμένες βασικές γνώσεις [Αποθήκευσης Azure] και [Bus υπηρεσίας Azure].


## <a name="send-interactive-messages-from-a-simulated-device"></a>Αποστολή μηνυμάτων αλληλεπιδραστικών από μια πρόχειρη συσκευή

Σε αυτήν την ενότητα, μπορείτε να τροποποιήσετε την εφαρμογή προσομοιωμένη συσκευή που δημιουργήσατε στην εκμάθηση [Γρήγορα αποτελέσματα με το Κέντρο IoT] για την αποστολή μηνυμάτων αλληλεπιδραστικών συσκευής στο cloud στο διανομέα IoT.

1. Χρησιμοποιήστε ένα πρόγραμμα επεξεργασίας κειμένου για να ανοίξετε το αρχείο simulated-device\src\main\java\com\mycompany\app\App.java. Αυτό το αρχείο περιέχει τον κωδικό για την εφαρμογή **προσομοιωμένη συσκευή** που δημιουργήσατε στην εκμάθηση [Γρήγορα αποτελέσματα με το Κέντρο IoT] .

2. Προσθέστε την ακόλουθη κλάση ένθετη στην κλάση **εφαρμογής** :

    ```
    private static class InteractiveMessageSender implements Runnable {
      public void run() {
        try {
          while (true) {
            String msgStr = "Alert message!";
            Message msg = new Message(msgStr);
            msg.setMessageId(java.util.UUID.randomUUID().toString());
            msg.setProperty("messageType", "interactive");
            System.out.println("Sending interactive message: " + msgStr);

            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);

            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(10000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished sending interactive messages.");
        }
      }
    }
    ```

    Αυτή η κατηγορία είναι παρόμοια με την κλάση **MessageSender** του έργου **προσομοιωμένη συσκευή** . Οι διαφορές μόνο είναι ότι μπορείτε πλέον να ορίσετε την ιδιότητα συστήματος **αναγνωριστικού μηνύματος** και μια προσαρμοσμένη ιδιότητα ονομάζεται **Τύπος μηνύματος**.
    Ο κώδικας αντιστοιχίζει ένα μοναδικό αναγνωριστικό (UUID) στην ιδιότητα **αναγνωριστικού μηνύματος** . Η υπηρεσία Bus να χρησιμοποιήσετε αυτό το αναγνωριστικό για να καταργήστε την αναπαραγωγή τα μηνύματα που λαμβάνει. Το δείγμα χρησιμοποιεί την ιδιότητα **Τύπος μηνύματος** για να διακρίνετε αλληλεπιδραστικών από το σημείο δεδομένων μηνύματα. Η εφαρμογή μεταβιβάζει αυτές τις πληροφορίες στις Ιδιότητες μηνύματος, αντί στο σώμα του μηνύματος, ώστε ο επεξεργαστής συμβάντων, δεν χρειάζεται να αποσειριοποίησης του μηνύματος για να εκτελέσετε δρομολόγηση μηνυμάτων.

    > [AZURE.NOTE] Είναι σημαντικό για τη δημιουργία του **αναγνωριστικού μηνύματος** που χρησιμοποιείται για την αναπαραγωγή καταργήστε την αλληλεπιδραστική μηνύματα στον κώδικα συσκευή. Επικοινωνίες περιστασιακό δικτύου ή άλλες αποτυχίες, θα μπορούσε να έχει ως αποτέλεσμα πολλές αναμετάδοση το ίδιο μήνυμα από αυτήν τη συσκευή. Μπορείτε επίσης να χρησιμοποιήσετε ένα Αναγνωριστικό σημασιολογίας μηνύματος, όπως ο κατακερματισμός των πεδίων δεδομένων σχετικό μήνυμα, στη θέση της ένα UUID.

3. Τροποποιήστε το **κύριο** μέθοδο για να στείλετε και τα δύο αλληλεπιδραστικών μηνύματα και σημείο δεδομένων μηνύματα όπως φαίνεται στην παρακάτω τμήμα κώδικα:

    ````
    MessageSender sender = new MessageSender();
    InteractiveMessageSender interactiveSender = new InteractiveMessageSender();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(sender);
    executor.execute(interactiveSender);
    ````

4. Αποθηκεύστε και κλείστε το αρχείο simulated-device\src\main\java\com\mycompany\app\App.java.

    > [AZURE.NOTE] Για λόγους απλούστευσης, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να εφαρμόζουν μια πολιτική "Επανάληψη" όπως εκθετική διπλασιασμών, όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων].

5. Για να δημιουργήσετε την εφαρμογή **προσομοιωμένη συσκευής** χρησιμοποιώντας Maven, εκτελέστε την ακόλουθη εντολή στη γραμμή εντολών στο φάκελο προσομοιωμένη συσκευή:

    ```
    mvn clean package -DskipTests
    ```

## <a name="process-device-to-cloud-messages"></a>Διεργασία μηνύματα συσκευής στο cloud

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Java που επεξεργάζεται τη συσκευή για να cloud μηνύματα από διανομέα IoT. Διανομέα IOT εκθέτει ένα [διανομέα συμβάν]-συμβατά τελικό σημείο για να ενεργοποιήσετε μια εφαρμογή για την ανάγνωση μηνυμάτων συσκευής στο cloud. Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί την κλάση [EventProcessorHost] για την επεξεργασία αυτών των μηνυμάτων σε μια εφαρμογή κονσόλας. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να επεξεργαστείτε μηνύματα από διανομείς συμβάντος, ανατρέξτε στο θέμα το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με το συμβάν διανομείς] .

Το κύριο δύσκολο κατά την υλοποίηση της αξιόπιστης αποθήκευσης των μηνυμάτων σημείο δεδομένων ή προώθηση μηνυμάτων διαδραστικό, είναι ότι Επεξεργασία συμβάντος εξαρτάται από τον καταναλωτή μήνυμα για την παροχή σημεία ελέγχου για την πρόοδο του έργου. Επιπλέον, για να επιτύχετε μια υψηλής απόδοσης και κατά την ανάγνωση από το συμβάν διανομείς θα πρέπει να παρέχετε σημεία ελέγχου σε δέσμες με μεγάλο. Αυτή η προσέγγιση δημιουργεί την πιθανότητα των διπλότυπων επεξεργασίας για μεγάλο αριθμό των μηνυμάτων, εάν υπάρχει ένα σφάλμα και να επαναφέρετε το προηγούμενο σημείο ελέγχου. Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δείτε πώς μπορείτε να συγχρονίσετε αποθήκευσης Azure εγγραφές και windows άρσεως διπλότυπου Bus υπηρεσίας με σημεία ελέγχου **EventProcessorHost** .

Για να συντάξετε μηνύματα με το χώρο αποθήκευσης Azure αξιόπιστα, το δείγμα χρησιμοποιεί τη δυνατότητα ολοκλήρωσης μεμονωμένα μπλοκ με [αντικείμενα BLOB μπλοκ][Azure Block Blobs]. Ο επεξεργαστής συμβάντων συγκεντρώνει μηνύματα στη μνήμη μέχρι να είναι καιρός να παρέχουν ένα σημείο ελέγχου. Για παράδειγμα, αφού το αθροιστικό buffer μηνυμάτων φτάσει το μέγιστο μέγεθος μπλοκ 4 MB ή μετά την υπηρεσία Bus άρσεως αναπαραγωγής που περνά χρονικού διαστήματος. Στη συνέχεια, πριν από το σημείο ελέγχου, τον κωδικό δεσμεύεται ενός νέου μπλοκ για το αντικείμενο blob.

Ο επεξεργαστής συμβάντων χρησιμοποιεί διανομείς συμβάν Μετατοπίζει το μήνυμα ως μπλοκ αναγνωριστικά. Αυτός ο μηχανισμός επιτρέπει στον επεξεργαστή συμβάντος για να εκτελέσετε ένα σημάδι ελέγχου αναπαραγωγής άρσεως πριν από την δεσμεύεται το νέο μπλοκ με το χώρο αποθήκευσης, φροντίζοντας πιθανές σφάλμα μεταξύ την οριστικοποίηση των ένα μπλοκ και το σημείο ελέγχου.

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί ένα λογαριασμό αποθήκευσης Azure για να γράψετε όλα τα μηνύματα που ανακτήθηκαν από διανομέα IoT. Για να αποφασίσετε εάν πρέπει να χρησιμοποιήσετε πολλούς λογαριασμούς αποθήκευσης Azure στη λύση σας, ανατρέξτε στο θέμα [αποθήκευσης Azure κλιμάκωση κατευθυντήριες γραμμές].

Η εφαρμογή χρησιμοποιεί τη λειτουργία αναπαραγωγής άρσεως Bus υπηρεσίας για να αποφύγετε διπλότυπες τιμές όταν επεξεργάζεται αλληλεπιδραστικών μηνύματα. Η συσκευή προσομοιωμένη Αποτυπώνει κάθε αλληλεπιδραστικών μήνυμα με ένα μοναδικό **αναγνωριστικού μηνύματος**. Αυτό το αναγνωριστικό Bus υπηρεσίας για να βεβαιωθείτε ότι, στο παράθυρο ώρας καθορισμένες άρσεως αναπαραγωγής, χωρίς δύο μηνύματα με το ίδιο **αναγνωριστικού μηνύματος** θα παραδίδονται στο δέκτη. Αυτό άρσεως αναπαραγωγή, μαζί με τη σημασιολογία ολοκλήρωσης ανά μήνυμα που παρέχεται από την υπηρεσία Bus ουρές, διευκολύνει την υλοποίηση την αξιόπιστη επεξεργασία των αλληλεπιδραστικών μηνυμάτων.

Για να βεβαιωθείτε ότι κανένα μήνυμα έχει υποβληθεί εκ νέου εκτός του παραθύρου άρσεως αναπαραγωγής, τον κωδικό συγχρονίζει το μηχανισμό σημείο ελέγχου **EventProcessorHost** με το παράθυρο Bus υπηρεσίας ουράς άρσεως αναπαραγωγής. Αυτός ο συγχρονισμός γίνεται από να επιβάλετε ένα σημείο ελέγχου τουλάχιστον μία φορά κάθε φορά που το παράθυρο άρσεως αναπαραγωγής που περνά (σε αυτό το πρόγραμμα εκμάθησης, το παράθυρο είναι μία ώρα).

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί μια μοναδική διαμερίσματα ουρά Bus υπηρεσίας όλα τα μηνύματα αλληλεπιδραστικών ανακτήθηκαν από διανομέα IoT στη διαδικασία. Για περισσότερες πληροφορίες σχετικά με τη χρήση υπηρεσιών Bus ουρές να πληροί τις απαιτήσεις κλιμάκωση της λύσης σας, ανατρέξτε στην τεκμηρίωση [Bus υπηρεσίας Azure] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Προμήθεια λογαριασμό αποθήκευσης Azure και μια ουρά Bus υπηρεσίας

Για να χρησιμοποιήσετε την κλάση [EventProcessorHost] , πρέπει να έχετε ένα λογαριασμό αποθήκευσης Azure για να ενεργοποιήσετε το **EventProcessorHost** για την καταγραφή πληροφοριών σημείο ελέγχου. Μπορείτε να χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό αποθήκευσης Azure ή, ακολουθήστε τις οδηγίες στο θέμα [Σχετικά με το χώρο αποθήκευσης Azure] για να δημιουργήσετε ένα νέο. Σημειώστε τη συμβολοσειρά σύνδεσης λογαριασμός Azure χώρου αποθήκευσης.

> [AZURE.NOTE] Όταν κάνετε αντιγραφή και επικόλληση τη συμβολοσειρά σύνδεσης λογαριασμός Azure χώρου αποθήκευσης, βεβαιωθείτε ότι υπάρχουν κενά διαστήματα περιλαμβάνονται.

Επίσης, χρειάζεστε μια ουρά Bus υπηρεσίας για να ενεργοποιήσετε την αξιόπιστη επεξεργασία των αλληλεπιδραστικών μηνυμάτων. Μπορείτε να δημιουργήσετε μια ουρά μέσω προγραμματισμού, με ένα παράθυρο άρσεως αναπαραγωγής μία ώρα, όπως εξηγείται στο θέμα [Πώς να χρησιμοποιείτε ουρές Bus υπηρεσίας][ουράς Bus υπηρεσίας]. Εναλλακτικά, μπορείτε να χρησιμοποιήσετε την [πύλη του Azure κλασική][lnk-classic-portal], ακολουθώντας τα παρακάτω βήματα:

1. Στην κάτω αριστερή γωνία, κάντε κλικ στην επιλογή **Δημιουργία** . Στη συνέχεια, κάντε κλικ στην επιλογή **Εφαρμογή υπηρεσιών** > **Bus υπηρεσίας** > **ουρά** > **Δημιουργία προσαρμοσμένης**. Πληκτρολογήστε το όνομα **d2ctutorial**, επιλέξτε μια περιοχή, και χρησιμοποιήστε έναν υπάρχοντα χώρο ονομάτων ή δημιουργήστε ένα νέο. Σημειώστε το όνομα χώρου ονομάτων, το χρειαστείτε αργότερα σε αυτό το πρόγραμμα εκμάθησης. Στην επόμενη σελίδα, επιλέξτε **Ενεργοποίηση εντοπισμού διπλοτύπων**και ορίστε την **αναπαραγωγή εντοπισμού ιστορικού χρονικού διαστήματος** σε μία ώρα. Στη συνέχεια, κάντε κλικ στο σημάδι ελέγχου στην κάτω δεξιά γωνία για να αποθηκεύσετε τη ρύθμιση των παραμέτρων σας ουρά.

    ![Δημιουργία ουράς στην πύλη του Azure][30]

2. Στη λίστα των ουρές Bus υπηρεσίας, κάντε κλικ στην επιλογή **d2ctutorial**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**. Δημιουργήστε δύο πολιτικές κοινόχρηστη πρόσβαση, μία που ονομάζεται **Αποστολή** με δικαιώματα **αποστολής** και έναν που ονομάζεται **ακρόαση** με **ακρόαση** δικαιώματα. Σημειώστε τις τιμές του **πρωτεύοντος κλειδιού** για τις δύο πολιτικές, χρειάζεστε τα παρακάτω σε αυτό το πρόγραμμα εκμάθησης. Όταν ολοκληρώσετε, κάντε κλικ στην επιλογή " **Αποθήκευση** " στο κάτω μέρος.

    ![Ρύθμιση παραμέτρων μιας ουράς στην πύλη του Azure][31]

### <a name="create-the-event-processor"></a>Δημιουργία ο επεξεργαστής συμβάντων

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή Java για να επεξεργαστείτε μηνύματα από το τελικό σημείο διανομέα συμβατό με το συμβάν.

Η πρώτη εργασία είναι να προσθέσετε ένα έργο Maven που ονομάζεται **διαδικασία-d2c-μηνύματα** που λαμβάνει μηνύματα συσκευής στο cloud από το τελικό σημείο συμβατό διανομέα IoT διανομέα συμβάντων και δρομολογεί αυτά τα μηνύματα με άλλες υπηρεσίες υποστήριξης.

1. Στο iot-java-get-αποτελέσματα φάκελο που δημιουργήσατε στην εκμάθηση [Γρήγορα αποτελέσματα με το Κέντρο IoT] , δημιουργήστε ένα έργο Maven που ονομάζεται **διαδικασία-d2c-μηνύματα** χρησιμοποιώντας την παρακάτω εντολή στη σας εντολών. Σημειώστε ότι αυτό είναι μια εντολή μεγάλου:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Από την γραμμή εντολών, μεταβείτε στο νέο φάκελο διαδικασία-d2c-μηνύματα.

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο pom.xml στο φάκελο διαδικασία-d2c-μηνύματα και προσθέστε τις ακόλουθες εξαρτήσεις στον κόμβο **εξαρτήσεις** . Αυτές οι εξαρτήσεις δίνουν τη δυνατότητα να χρησιμοποιήσετε το azure eventhubs azure-eventhubs-eph και πακέτα azure servicebus στην εφαρμογή σας για να αλληλεπιδράσετε με το διανομέα IoT και Bus υπηρεσίας ουράς:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs-eph</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Αποθηκεύστε και κλείστε το αρχείο pom.xml.

Η επόμενη εργασία είναι να προσθέσετε μια κλάση **ErrorNotificationHandler** για το έργο.

1. Χρησιμοποιήστε ένα πρόγραμμα επεξεργασίας κειμένου για να δημιουργήσετε ένα αρχείο process-d2c-messages\src\main\java\com\mycompany\app\ErrorNotificationHandler.java. Προσθέστε τον παρακάτω κώδικα στο αρχείο για να εμφανίσετε μηνύματα σφάλματος από την παρουσία **EventProcesssorHost** :

    ```
    package com.mycompany.app;

    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements
        Consumer<ExceptionReceivedEventArgs> {
      @Override
      public void accept(ExceptionReceivedEventArgs t) {
        System.out.println("EventProcessorHost: Host " + t.getHostname()
            + " received general error notification during " + t.getAction() + ": "
            + t.getException().toString());
      }
    }
    ```

2. Αποθηκεύστε και κλείστε το αρχείο ErrorNotificationHandler.java.

Τώρα μπορείτε να προσθέσετε μια κλάση που υλοποιεί το περιβάλλον εργασίας **IEventProcessor** . Η κλάση **EventProcessorHost** κλήσεις αυτής της κλάσης για την επεξεργασία μηνυμάτων συσκευής στο cloud που έχουν ληφθεί από διανομέα IoT. Ο κώδικας σε αυτήν την κλάση υλοποιεί της λογικής για την αποθήκευση αξιόπιστα μηνύματα σε ένα κοντέινερ αντικειμένων blob και προώθηση μηνυμάτων αλληλεπιδραστικών στην ουρά Bus υπηρεσίας.

Η μέθοδος **onEvents** ορίζει τη μεταβλητή **latestEventData** που παρακολουθεί τον αριθμό offset και την ακολουθία των το πιο πρόσφατο μήνυμα διαβάζονται από αυτόν τον επεξεργαστή συμβάν. Να θυμάστε ότι κάθε επεξεργαστή είναι υπεύθυνος για μια μεμονωμένη διαμερίσματα. Η μέθοδος **onEvents** , στη συνέχεια, λαμβάνει μια δέσμη μηνυμάτων από το Κέντρο IoT και τα επεξεργάζεται, ως εξής: στέλνει μηνύματα αλληλεπιδραστικών στην ουρά Bus υπηρεσίας και τοποθετεί μηνυμάτων σημείο δεδομένων στο buffer μνήμης **toAppend** . Εάν το buffer μνήμης φτάσει το όριο μπλοκ 4 MB ή τα windows ώρα άρσεως αναπαραγωγής που περνά (μία ώρα μετά το τελευταίο σημείο ελέγχου σε αυτό το πρόγραμμα εκμάθησης), η μέθοδος ενεργοποιεί ένα σημείο ελέγχου.

Η μέθοδος **AppendAndCheckPoint** δημιουργεί πρώτα μια **blockId** για το μπλοκ για να προσαρτήσετε το αντικείμενο blob. Azure αποθήκευσης απαιτεί όλα αποκλεισμός αναγνωριστικά να έχουν το ίδιο μήκος, ώστε η μέθοδος pads τη μετατόπιση με μηδενικά στην αρχή. Στη συνέχεια, εάν ένα μπλοκ με αυτό το Αναγνωριστικό είναι ήδη στο αντικείμενο blob, τη μέθοδο αντικαθιστά το με το τρέχον περιεχόμενο buffer.

> [AZURE.NOTE] Για να απλοποιήσετε τον κώδικα, αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί ένα μεμονωμένο blob ανά διαμερίσματα για την αποθήκευση των μηνυμάτων. Μια λύση πραγματικό να υλοποιήσετε σάρωση με τη δημιουργία πρόσθετων αρχείων μετά από ένα συγκεκριμένο χρονικό διάστημα ή μετά από ένα συγκεκριμένο μέγεθος αρχείου. Να θυμάστε ότι ένα αντικειμένων blob του Azure μπλοκ μπορεί να περιέχει πολύ 195 GB δεδομένων.

Η επόμενη εργασία είναι για να υλοποιήσετε το περιβάλλον εργασίας **IEventProcessor** :

1. Χρησιμοποιήστε ένα πρόγραμμα επεξεργασίας κειμένου για να δημιουργήσετε ένα αρχείο process-d2c-messages\src\main\java\com\mycompany\app\EventProcessor.java.

2. Προσθέστε το παρακάτω εισαγωγές και τον ορισμό κλάσης στο αρχείο EventProcessor.java. Η κλάση **EventProcessor** υλοποιεί το περιβάλλον εργασίας **IEventProcessor** που ορίζει τη συμπεριφορά του προγράμματος-πελάτη διανομείς συμβάν:

    ```
    package com.mycompany.app;

    import java.io.ByteArrayInputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.nio.charset.StandardCharsets;
    import java.time.Duration;
    import java.time.Instant;
    import java.util.ArrayList;
    import java.util.Base64;
    import java.util.concurrent.ExecutionException;

    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.BrokeredMessage;

    public class EventProcessor implements IEventProcessor {

    }
    ```

3. Προσθέστε τις ακόλουθες μεθόδους για την τάξη **EventProcessor** για να υλοποιήσετε το περιβάλλον εργασίας **IEventProcessor** :

    ```
    @Override
    public void onOpen(PartitionContext context) throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is opening");
    }

    @Override
    public void onClose(PartitionContext context, CloseReason reason)
        throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is closing for reason "
          + reason.toString());
    }

    @Override
    public void onError(PartitionContext context, Throwable error) {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " onError: " + error.toString());
    }

    @Override
    public void onEvents(PartitionContext context, Iterable<EventData> messages)
        throws Exception {
    }
    ```

4. Προσθέστε τις ακόλουθες μεταβλητές επίπεδο κλάσης στην κλάση **EventProcessor** :

    ```
    public static CloudBlobContainer blobContainer;
    public static ServiceBusContract serviceBusContract;

    // Use a smaller MAX_BLOCK_SIZE value to test.
    final private int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
    final private Duration MAX_CHECKPOINT_TIME = Duration.ofHours(1);

    private ByteArrayOutputStream toAppend = new ByteArrayOutputStream(
        MAX_BLOCK_SIZE);
    private Instant start = Instant.now();
    private EventData latestEventData;
    ```

5. Προσθέστε μια μέθοδο **AppendAndCheckPoint** με την εξής υπογραφή στην κλάση **EventProcessor** :

    ```
    private void AppendAndCheckPoint(PartitionContext context)
      throws URISyntaxException, StorageException, IOException,
      IllegalArgumentException, InterruptedException, ExecutionException {
    }
    ```

6. Προσθέστε τον ακόλουθο κώδικα στη μέθοδο **AppendAndCheckPoint** για να ανακτήσετε το μήνυμα offset και την ακολουθία αριθμό της τρέχουσας στο τα διαμερίσματα:

    ```
    String currentOffset = latestEventData.getSystemProperties().getOffset();
    Long currentSequence = latestEventData.getSystemProperties().getSequenceNumber();
    System.out
        .printf(
            "\nAppendAndCheckPoint using partition: %s, offset: %s, sequence: %s\n",
            context.getPartitionId(), currentOffset, currentSequence);
    ```

7. Στη μέθοδο **AppendAndCheckPoint** , χρησιμοποιήστε την τρέχουσα τιμή offset για να δημιουργήσετε μια παρουσία **BlockEntry** για το επόμενο μπλοκ για να αποθηκεύσετε το αντικείμενο blob:

    ```
    Long blockId = Long.parseLong(currentOffset);
    String blockIdString = String.format("startSeq:%1$025d", blockId);
    String encodedBlockId = Base64.getEncoder().encodeToString(
        blockIdString.getBytes(StandardCharsets.US_ASCII));
    BlockEntry block = new BlockEntry(encodedBlockId);
    ```

8. Στη μέθοδο **AppendAndCheckPoint** , αποστείλετε το πιο πρόσφατο σύνολο των μηνυμάτων για να το blob μπλοκ και να ανακτήσετε την τρέχουσα λίστα μπλοκ:

    ```
    String blobName = String.format("iothubd2c_%s", context.getPartitionId());
    CloudBlockBlob currentBlob = blobContainer.getBlockBlobReference(blobName);

    currentBlob.uploadBlock(block.getId(),
        new ByteArrayInputStream(toAppend.toByteArray()), toAppend.size());
    ArrayList<BlockEntry> blockList = currentBlob.downloadBlockList();
    ```

9. Στη μέθοδο **AppendAndCheckPoint** , μπορείτε είτε να δημιουργήσετε το αρχικό μπλοκ σε ένα νέο blob είτε να προσαρτήσετε το μπλοκ την υπάρχουσα blob:

    ```
    if (currentBlob.exists()) {
      // Check if we should append new block or overwrite existing block
      BlockEntry last = blockList.get(blockList.size() - 1);
      if (blockList.size() > 0 && !last.getId().equals(block.getId())) {
        System.out.printf("Appending block %s to blob %s\n", blockId, blobName);
        blockList.add(block);
      } else {
        System.out.printf("Overwriting block %s in blob %s\n", blockId,
            blobName);
      }
    } else {
      System.out.printf("Creating initial block %s in new blob: %s\n", blockId,
          blobName);
      blockList.add(block);
    }
    currentBlob.commitBlockList(blockList);
    ```

10. Τέλος στη μέθοδο **AppendAndCheckPoint** , δημιουργήστε ένα σημείο ελέγχου σε τα διαμερίσματα και προετοιμασία για την αποθήκευση του επόμενου μπλοκ μηνυμάτων:

    ```
    context.checkpoint(latestEventData);

    // Reset everything after the checkpoint.
    toAppend.reset();
    start = Instant.now();
    System.out.printf("Checkpointed on partition id: %s\n",
        context.getPartitionId());
    ```

11. Στη μέθοδο **onEvents** , προσθέστε τον ακόλουθο κώδικα για να λάβετε μηνύματα από το τελικό σημείο IoT διανομέας και προώθηση μηνυμάτων αλληλεπιδραστικών στην ουρά Bus υπηρεσίας. Στη συνέχεια, η κλήση της μεθόδου **AppendAndCheckPoint** όταν ο αποκλεισμός είναι πλήρης ή λήξει το χρονικό όριο:

    ```
    if (messages != null) {
      for (EventData eventData : messages) {
        latestEventData = eventData;
        byte[] data = eventData.getBody();
        if (eventData.getProperties().containsKey("messageType")
            && eventData.getProperties().get("messageType")
                .equals("interactive")) {
          String messageId = (String) eventData.getSystemProperties().get(
              "message-id");
          BrokeredMessage message = new BrokeredMessage(data);
          message.setMessageId(messageId);
          serviceBusContract.sendQueueMessage("d2ctutorial", message);
          continue;
        }
        if (toAppend.size() + data.length > MAX_BLOCK_SIZE
            || Duration.between(start, Instant.now()).compareTo(
                MAX_CHECKPOINT_TIME) > 0) {
          AppendAndCheckPoint(context);
        }
        toAppend.write(data);
      }
    }
    ```

12. Τέλος στη μέθοδο **onEvents** , προσθέστε έναν όρο 'else if' για να καλέσετε το **AppendAndCheckPoint** εάν λήξει το χρονικό όριο όταν υπάρχουν μηνύματα που προέρχονται από διανομέα IoT:

    ```
    else if ((toAppend.size() > 0)
        && Duration.between(start, Instant.now())
            .compareTo(MAX_CHECKPOINT_TIME) > 0) {
      AppendAndCheckPoint(context);
    }
    ```

13. Αποθηκεύστε τις αλλαγές στο αρχείο EventProcessor.java.

Η τελευταία εργασία στο έργο **διαδικασία-d2c-μηνυμάτων** είναι για να προσθέσετε κώδικα στην **κύρια** μέθοδο που εμφανίζει μια παρουσία **EventProcessorHost** .

1. Χρησιμοποιήστε ένα πρόγραμμα επεξεργασίας κειμένου για να ανοίξετε το αρχείο process-d2c-messages\src\main\java\com\mycompany\app\App.java.

2. Προσθέστε τις παρακάτω προτάσεις για την **Εισαγωγή** του αρχείου:

    ```
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.storage.CloudStorageAccount;
    import com.microsoft.azure.storage.StorageException;
    import com.microsoft.azure.storage.blob.CloudBlobClient;
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusConfiguration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusService;

    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.concurrent.*;
    ```

3. Προσθέστε την ακόλουθη μεταβλητή επίπεδο κλάσης την κλάση **εφαρμογής** . Αντικαταστήστε το **{yourstorageaccountconnectionstring}** με τη συμβολοσειρά σύνδεσης λογαριασμού αποθήκευσης Azure που κάνατε σε σημείωση της προηγουμένως στην ενότητα [προμήθεια λογαριασμό αποθήκευσης Azure και μια ουρά Bus υπηρεσίας](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String storageConnectionString = "{yourstorageaccountconnectionstring}";
    ```

4. Προσθέστε τις ακόλουθες μεταβλητές επίπεδο κλάσης στην **εφαρμογή** κλάση και αντικαταστήστε **{yourservicebusnamespace}** με το χώρο ονομάτων Bus υπηρεσίας και **{yourservicebussendkey}** με αριθμό-κλειδί **Αποστολή** της ουράς σας. Κάνατε προηγουμένως μια σημείωση με χώρο ονομάτων και να **ακούσετε** κύριες τιμές στην ενότητα [προμήθεια λογαριασμό αποθήκευσης Azure και μια ουρά Bus υπηρεσίας](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "send";
    private final static String serviceBusSASKey = "{yourservicebussendkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    ```

5. Προσθέστε τις ακόλουθες μεταβλητές επίπεδο κλάσης την κλάση **εφαρμογής** . Αντικαταστήστε **{youreventhubcompatibleendpoint}** με την τιμή του τελικού σημείου διανομέα συμβατό με το συμβάν. Η τιμή του τελικού σημείου μοιάζει με **του... χώρο ονομάτων** έτσι θα πρέπει να καταργήσετε το **μικρές: / /** πρόθεμα και το επίθημα **.servicebus.windows.net/** . Αντικαταστήστε **{youreventhubcompatiblename}** με το όνομα διανομέας συμβατό με το συμβάν. Αντικαταστήστε **{youriothubkey}** με τον αριθμό-κλειδί **iothubowner** . Κάνατε μια σημείωση από αυτές τις τιμές στο παράθυρο [Δημιουργία ένα διανομέα IoT] [ lnk-create-an-iot-hub] ενότητας στην εκμάθηση *Γρήγορα αποτελέσματα με το Azure IoT διανομέα για Java* :

    ```
    private final static String consumerGroupName = "$Default";
    private final static String namespaceName = "{youreventhubcompatibleendpoint}";
    private final static String eventHubName = "{youreventhubcompatiblename}";
    private final static String sasKeyName = "iothubowner";
    private final static String sasKey = "{youriothubkey}";
    ```

6. Τροποποιήστε την υπογραφή τη μέθοδο του **κύριου** ως εξής:

    ```
    public static void main(String args[]) throws InvalidKeyException,
      URISyntaxException, StorageException {
    }
    ```

7. Προσθέστε τον ακόλουθο κώδικα στην **κύρια** μέθοδο για να λάβετε μια αναφορά στο κοντέινερ αντικειμένων blob όπου αποθηκεύονται τα μηνύματα:

    ```
    System.out.println("Process D2C messages using EventProcessorHost");
    CloudStorageAccount account = CloudStorageAccount
        .parse(storageConnectionString);
    CloudBlobClient client = account.createCloudBlobClient();
    EventProcessor.blobContainer = client
        .getContainerReference("d2cjavatutorial");
    EventProcessor.blobContainer.createIfNotExists();
    ```

8. Προσθέστε τον ακόλουθο κώδικα στην **κύρια** μέθοδο για να λάβετε μια αναφορά για την υπηρεσία Bus υπηρεσίας:

    ```
    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    EventProcessor.serviceBusContract = ServiceBusService.create(config);
    ```

9. Στην **κύρια** μέθοδο, ρύθμιση παραμέτρων και δημιουργήστε μια παρουσία **EventProcessorHost** . Η επιλογή **setInvokeProcessorAfterReceiveTimeout** εξασφαλίζει ότι η παρουσία **EventProcessorHost** καλεί τη μέθοδο **onEvents** στο περιβάλλον εργασίας **IEventProcessor** , ακόμα και όταν δεν υπάρχουν μηνύματα για την επεξεργασία. Η μέθοδος **onEvents** , στη συνέχεια, πάντα καλεί τη μέθοδο **AppendAndCheckPoint** όταν λήγει το χρονικό όριο.

    ```
    ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(
        namespaceName, eventHubName, sasKeyName, sasKey);
    EventProcessorHost host = new EventProcessorHost(eventHubName,
        consumerGroupName, eventHubConnectionString.toString(),
        storageConnectionString);
    EventProcessorOptions options = new EventProcessorOptions();
    options.setExceptionNotification(new ErrorNotificationHandler());
    options.setInvokeProcessorAfterReceiveTimeout(true);
    ```

10. Στην **κύρια** μέθοδο, καταχωρήστε την εφαρμογή **IEventProcessor** με την παρουσία **EventProcessorHost** :

    ```
    try {
      System.out.println("Registering host named " + host.getHostName());
      host.registerEventProcessor(EventProcessor.class, options).get();
    } catch (Exception e) {
      System.out.print("Failure while registering: ");
      if (e instanceof ExecutionException) {
        Throwable inner = e.getCause();
        System.out.println(inner.toString());
      } else {
        System.out.println(e.toString());
      }
      System.out.println(e.toString());
    }
    ```

11. Τέλος, μπορείτε να προσθέσετε λογική στην **κύρια** μέθοδο για να τερματίσετε την παρουσία **EventProcessorHost** :

    ```
    System.out.println("Press enter to stop");
    try {
      System.in.read();
      host.unregisterEventProcessor();

      System.out.println("Calling forceExecutorShutdown");
      EventProcessorHost.forceExecutorShutdown(120);
    } catch (Exception e) {
      System.out.println(e.toString());
      e.printStackTrace();
    }

    System.out.println("End of sample");
    ```

12. Αποθηκεύστε και κλείστε το αρχείο process-d2c-messages\src\main\java\com\mycompany\app\App.java.

13. Για να δημιουργήσετε την εφαρμογή **διαδικασία-d2c-μηνύματα** χρησιμοποιώντας Maven, εκτελέστε την ακόλουθη εντολή στη γραμμή εντολών στο φάκελο διαδικασία-d2c-μηνυμάτων:

    ```
    mvn clean package -DskipTests
    ```

## <a name="receive-interactive-messages"></a>Λήψη αλληλεπιδραστικών μηνυμάτων

Σε αυτήν την ενότητα, μπορείτε να συντάξετε μια εφαρμογή κονσόλας Java που λαμβάνει το διαδραστικό μηνύματα από την υπηρεσία Bus ουρά.

Η πρώτη εργασία είναι να προσθέσετε ένα έργο Maven που ονομάζεται **διαδικασία αλληλεπιδραστικών μηνύματα** που λαμβάνει μηνύματα που στέλνονται στην ουρά Bus υπηρεσίας από τις παρουσίες **EventProcessor** .

1. Στο iot-java-get-αποτελέσματα φάκελο που δημιουργήσατε στην εκμάθηση [Γρήγορα αποτελέσματα με το Κέντρο IoT] , δημιουργήστε ένα έργο Maven που ονομάζεται **διαδικασία αλληλεπιδραστικών μηνύματα** χρησιμοποιώντας την ακόλουθη εντολή στη σας εντολών. Σημειώστε ότι αυτό είναι μια εντολή μεγάλου:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-interactive-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Σε σας γραμμή εντολών, μεταβείτε στο νέο φάκελο διαδικασία αλληλεπιδραστικών μηνύματα.

3. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου, ανοίξτε το αρχείο pom.xml στο φάκελο διαδικασία αλληλεπιδραστικών μηνύματα και προσθέστε την ακόλουθη εξάρτηση στον κόμβο **εξαρτήσεις** . Αυτή η εξάρτηση σάς επιτρέπει να χρησιμοποιήσετε το πακέτο azure servicebus στην εφαρμογή σας για να αλληλεπιδράσετε με ουρά σας Bus υπηρεσίας:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Αποθηκεύστε και κλείστε το αρχείο pom.xml.

Η επόμενη εργασία είναι να προσθέσετε κώδικα για να ανακτήσετε μηνύματα από την υπηρεσία Bus ουρά.

1. Χρησιμοποιήστε ένα πρόγραμμα επεξεργασίας κειμένου για να ανοίξετε το αρχείο process-interactive-messages\src\main\java\com\mycompany\app\App.java.

2. Προσθέστε τα ακόλουθα `import` προτάσεων για το αρχείο:

    ```
    import java.io.IOException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;

    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.exception.ServiceException;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    ```

3. Προσθέστε τις ακόλουθες μεταβλητές επίπεδο κλάσης στην **εφαρμογή** κλάση και αντικαταστήστε **{yourservicebusnamespace}** με το χώρο ονομάτων Bus υπηρεσίας και **{yourservicebuslistenkey}** με αριθμό-κλειδί **ακρόαση** σας ουρά. Κάνατε προηγουμένως μια σημείωση με χώρο ονομάτων και να **ακούσετε** κύριες τιμές στην ενότητα [προμήθεια λογαριασμό αποθήκευσης Azure και μια ουρά Bus υπηρεσίας](#provision-an-azure-storage-account-and-a-service-bus-queue) :

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "listen";
    private final static String serviceBusSASKey = "{yourservicebuslistenkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    private final static String queueName = "d2ctutorial";
    private static ServiceBusContract service = null;
    ```

4. Προσθέστε την ακόλουθη κλάση ένθετη στην κλάση **εφαρμογής** για να λάβετε μηνύματα από την ουρά:

    ```
    private static class MessageReceiver implements Runnable {
      public void run() {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        try {
          while (true) {
            ReceiveQueueMessageResult resultQM = service.receiveQueueMessage(
                queueName, opts);
            BrokeredMessage message = resultQM.getValue();
            if (message != null && message.getMessageId() != null) {
              System.out.println("MessageID: " + message.getMessageId());
              System.out.print("From queue: ");
              byte[] b = new byte[200];
              String s = null;
              int numRead = message.getBody().read(b);
              while (-1 != numRead) {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
              }
              System.out.println();
            } else {
              Thread.sleep(1000);
            }
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        } catch (ServiceException e) {
          System.out.println("ServiceException: " + e.getMessage());
        } catch (IOException e) {
          System.out.println("IOException: " + e.getMessage());
        }
      }
    }
    ```

5. Τροποποιήστε την υπογραφή τη μέθοδο του **κύριου** ως εξής:

    ```
    public static void main(String args[]) throws ServiceException, IOException {
    }
    ```

6. Στην **κύρια** μέθοδο, προσθέστε τον ακόλουθο κώδικα για να ξεκινήσετε ακρόαση για νέα μηνύματα:

    ```
    System.out.println("Process interactive messages");

    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    service = ServiceBusService.create(config);

    MessageReceiver receiver = new MessageReceiver();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(receiver);

    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    ```

7. Αποθηκεύστε και κλείστε το αρχείο process-interactive-messages\src\main\java\com\mycompany\app\App.java.

8. Για να δημιουργήσετε την εφαρμογή **διαδικασία αλληλεπιδραστικών μηνύματα** χρησιμοποιώντας Maven, εκτελέστε την ακόλουθη εντολή στη γραμμή εντολών στο φάκελο διαδικασία αλληλεπιδραστικών μηνυμάτων:

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις τρεις εφαρμογές.

1.  Για να εκτελέσετε την εφαρμογή **διαδικασία αλληλεπιδραστικών μηνύματα** , σε μια γραμμή εντολών ή κελύφους μεταβείτε στο φάκελο διαδικασία αλληλεπιδραστικών μηνύματα και εκτελέστε την ακόλουθη εντολή:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Εκτέλεση της διαδικασίας αλληλεπιδραστικών μηνύματα][processinteractive]

2.  Για να εκτελέσετε την εφαρμογή **διαδικασία-d2c-μηνύματα** , σε μια γραμμή εντολών ή κελύφους μεταβείτε στο φάκελο διαδικασία-d2c-μηνύματα και εκτελέστε την ακόλουθη εντολή:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Εκτέλεση της διαδικασίας-d2c-μηνύματα][processd2c]

3.  Για να εκτελέσετε την εφαρμογή **προσομοιωμένη συσκευή** , σε μια γραμμή εντολών ή κελύφους μεταβείτε στο φάκελο προσομοιωμένη συσκευή και εκτελέστε την ακόλουθη εντολή:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Εκτέλεση προσομοιωμένη συσκευή][simulateddevice]

> [AZURE.NOTE] Για να δείτε ενημερώσεις στο blob σας, ίσως χρειαστεί να μειώσετε τη σταθερά **MAX_BLOCK_SIZE** στην τάξη **StoreEventProcessor** μικρότερη τιμή, όπως **1024**. Αυτή η αλλαγή είναι χρήσιμη, επειδή θα χρειαστεί κάποιος χρόνος για την επίτευξη το όριο μεγέθους μπλοκ με τα δεδομένα που αποστέλλονται από τη συσκευή προσομοιωμένη. Με μικρότερο μέγεθος μπλοκ, δεν χρειάζεται να περιμένετε τόσο πολύ για να δείτε το αντικείμενο blob που δημιουργούνται και ενημερώνονται. Ωστόσο, χρησιμοποιώντας ένα μεγαλύτερο μέγεθος μπλοκ κάνει την εφαρμογή πιο μεταβλητού μεγέθους.

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς να αξιόπιστα επεξεργαστείτε σημείου δεδομένων και αλληλεπιδραστικών μηνύματα συσκευής στο cloud χρησιμοποιώντας την κλάση [EventProcessorHost] .

[Πώς μπορείτε να στέλνετε μηνύματα cloud-σε-συσκευή με το Κέντρο IoT] [ lnk-c2d] σας δείχνει πώς μπορείτε να στείλετε μηνύματα στις συσκευές σας από το παρασκηνίου.

Για να δείτε παραδείγματα ολοκλήρωσης για ολοκληρωμένες λύσεις που χρησιμοποιούν IoT διανομέα, ανατρέξτε στο θέμα [Οικογένεια IoT Azure][lnk-suite].

Για να μάθετε περισσότερα σχετικά με την ανάπτυξη λύσεων με IoT διανομέα, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές IoT διανομέα].

<!-- Images. -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[processinteractive]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[processd2c]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/createqueue2.png
[31]: ./media/iot-hub-java-java-process-d2c/createqueue3.png

<!-- Links -->

[Χώρος αποθήκευσης αντικειμένων blob του Azure]: ../storage/storage-dotnet-how-to-use-blobs.md
[Εργοστασιακές Azure δεδομένων]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Bus υπηρεσίας Ουράς]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Οδηγός Azure προγραμματιστής διανομέα IoT - συσκευή με το cloud]: iot-hub-devguide-messaging.md

[Azure χώρου αποθήκευσης]: https://azure.microsoft.com/documentation/services/storage/
[Bus Azure υπηρεσίας]: https://azure.microsoft.com/documentation/services/service-bus/

[Οδηγός για προγραμματιστές IoT διανομέα]: iot-hub-devguide.md
[Γρήγορα αποτελέσματα με το Κέντρο IoT]: iot-hub-java-java-getstarted.md
[Κέντρο για προγραμματιστές IoT Azure]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Χειρισμός μεταβατικές σφαλμάτων]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Σχετικά με το χώρο αποθήκευσης στο Azure]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Γρήγορα αποτελέσματα με το συμβάν διανομείς]: ../event-hubs/event-hubs-java-ephjava-getstarted.md
[Azure αποθήκευσης κλιμάκωση κατευθυντήριες γραμμές]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: https://github.com/Azure/azure-event-hubs/tree/master/java/azure-eventhubs-eph
[Χειρισμός μεταβατικές σφαλμάτων]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-java-java-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-create-an-iot-hub]: iot-hub-java-java-getstarted.md#create-an-iot-hub