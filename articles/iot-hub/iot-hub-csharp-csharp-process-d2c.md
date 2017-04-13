<properties
    pageTitle="Επεξεργασία των μηνυμάτων συσκευή-cloud IoT διανομέα (.Net) | Microsoft Azure"
    description="Ακολουθήστε αυτό το πρόγραμμα εκμάθησης για να μάθετε χρήσιμες μοτίβα για την επεξεργασία μηνυμάτων συσκευή-cloud IoT διανομέα."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Πρόγραμμα εκμάθησης: Πώς να επεξεργαστεί διανομέα IoT συσκευή-cloud μηνύματα χρησιμοποιώντας το .net

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Εισαγωγή

Azure IoT διανομέα είναι μια πλήρως διαχειριζόμενη υπηρεσία που επιτρέπει την αξιόπιστη και ασφαλής διπλής κατεύθυνσης επικοινωνιών μεταξύ εκατομμύρια IoT συσκευές και μια εφαρμογή, δημιουργήστε αντίγραφα τέλος. Άλλα προγράμματα εκμάθησης ([Γρήγορα αποτελέσματα με το Κέντρο IoT] και [Αποστολή μηνυμάτων cloud-σε-συσκευή με το Κέντρο IoT][lnk-c2d]) σας δείξουν πώς μπορείτε να χρησιμοποιήσετε τη βασική συσκευή στο cloud και cloud-σε-συσκευή ανταλλαγής μηνυμάτων λειτουργικότητα IoT διανομέα.

Αυτό το πρόγραμμα εκμάθησης δημιουργεί στην τον κώδικα που εμφανίζεται στην εκμάθηση [Γρήγορα αποτελέσματα με το Κέντρο IoT] και που εμφανίζει δύο μεταβλητού μεγέθους μοτίβα που μπορείτε να χρησιμοποιήσετε για την επεξεργασία μηνυμάτων συσκευής στο cloud:

- Αξιόπιστη χώρου αποθήκευσης των μηνυμάτων συσκευής στο cloud στο [χώρο αποθήκευσης αντικειμένων blob του Azure]. Ένα συνηθισμένο σενάριο είναι *ψυχρές διαδρομή* ανάλυσης, στο οποίο μπορείτε να αποθηκεύσετε δεδομένα τηλεμετρίας σε αντικείμενα BLOB για να χρησιμοποιήσετε ως είσοδο σε διεργασίες ανάλυσης. Αυτές οι διεργασίες μπορεί να ελέγχεται με εργαλεία όπως το [Azure εργοστασίου δεδομένων] ή στη στοίβα [HDInsight (Hadoop)] .

- Αξιόπιστη επεξεργασία *αλληλεπιδραστικών* μηνύματα συσκευής στο cloud. Συσκευή-cloud μηνυμάτων είναι αλληλεπιδραστικές όταν είναι άμεση εναύσματα για ένα σύνολο ενεργειών σε εφαρμογή παρασκηνίου. Για παράδειγμα, μια συσκευή ενδέχεται να σας στείλει ένα μήνυμα προειδοποίησης που αποτελεί έναυσμα για εισαγωγή δελτίου σε ένα σύστημα CRM. Αντίθετα, τα μηνύματα *σημείο δεδομένων* απλώς τροφοδοσίας σε μια μηχανισμός ανάλυσης που βρίσκεται. Για παράδειγμα, τηλεμετρίας θερμοκρασίας από μια συσκευή που πρέπει να αποθηκευτεί για μελλοντική ανάλυση είναι ένα μήνυμα σημείο δεδομένων.

Επειδή διανομέα IoT εκθέτει ένα [Συμβάν διανομέα][lnk-event-hubs]-συμβατά τελικό σημείο για να λάβετε μηνύματα συσκευής στο cloud, αυτό χρησιμοποιεί προγραμμάτων εκμάθησης μια παρουσία [EventProcessorHost] . Αυτή η παρουσία:

* Αξιόπιστα αποθηκεύει μηνύματα *σημείο δεδομένων* στο χώρο αποθήκευσης αντικειμένων blob του Azure.
* Προωθεί *αλληλεπιδραστικών* συσκευή-cloud μηνύματα σε μια Azure [Bus υπηρεσίας ουράς] για άμεση επεξεργασία.

Υπηρεσία Bus βοηθά να εξασφαλίσετε αξιόπιστη επεξεργασία των αλληλεπιδραστικών μηνυμάτων, όπως παρέχει σημεία ανά μήνυμα ελέγχου και ώρα που βασίζεται στο παράθυρο άρσεως αναπαραγωγής.

> [AZURE.NOTE] Μια παρουσία **EventProcessorHost** είναι μόνο ένας τρόπος για την επεξεργασία αλληλεπιδραστικών μηνυμάτων. Άλλες επιλογές περιλαμβάνουν [Azure Service ύφασμα] [ lnk-service-fabric] και τις [Αναλύσεις ροή Azure][lnk-stream-analytics].

Στο τέλος αυτού του προγράμματος εκμάθησης, μπορείτε να εκτελέσετε τρεις εφαρμογές κονσόλας Windows:

* **SimulatedDevice**, μια τροποποιημένη έκδοση της εφαρμογής δημιουργήσατε στο το [Γρήγορα αποτελέσματα με το Κέντρο IoT] πρόγραμμα εκμάθησης, δεδομένων στέλνει μηνύματα συσκευή-cloud σημείο κάθε δεύτερη και αλληλεπιδραστικών μηνύματα συσκευή-cloud κάθε 10 δευτερόλεπτα. Αυτή η εφαρμογή χρησιμοποιεί το πρωτόκολλο AMQP για να επικοινωνήσετε με το Κέντρο IoT.
* **ProcessDeviceToCloudMessages** χρησιμοποιεί την κλάση [EventProcessorHost] για να ανακτήσετε μηνύματα από το τελικό σημείο διανομέα συμβατό με το συμβάν. Το, στη συνέχεια, αξιόπιστα αποθηκεύει μηνύματα σημείο δεδομένων στο χώρο αποθήκευσης αντικειμένων blob του Azure και προωθεί αλληλεπιδραστικών μηνύματα σε μια ουρά Bus υπηρεσίας.
* **ProcessD2CInteractiveMessages** ουρές καταργήστε την το διαδραστικό μηνυμάτων από την υπηρεσία Bus ουρά.

> [AZURE.NOTE] Ο διανομέας IoT διαθέτει SDK υποστήριξη για πολλές πλατφόρμες συσκευών και γλώσσες, συμπεριλαμβανομένων των C, Java και JavaScript. Για να μάθετε πώς να αντικαταστήστε προσομοιωμένη συσκευή σε αυτό το πρόγραμμα εκμάθησης με μια φυσική συσκευή και πώς μπορείτε να συνδέσετε συσκευές με ένα διανομέα IoT, ανατρέξτε στο [Κέντρο για προγραμματιστές του Azure IoT].

Αυτό το πρόγραμμα εκμάθησης εφαρμόζεται απευθείας σε άλλους τρόπους για την εκμετάλλευση μηνυμάτων συμβατό με το Κέντρο συμβάν, όπως το [HDInsight (Hadoop)] έργα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές διανομέα IoT Azure - συσκευή στο cloud].

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε τα εξής:

+ Microsoft Visual Studio 2015.

+ Λογαριασμού Azure active. <br/>Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/free/) σε λίγα λεπτά.

Θα πρέπει να έχετε ορισμένες βασικές γνώσεις [Αποθήκευσης Azure] και [Bus υπηρεσίας Azure].


## <a name="send-interactive-messages-from-a-simulated-device"></a>Αποστολή μηνυμάτων αλληλεπιδραστικών από μια πρόχειρη συσκευή

Σε αυτήν την ενότητα, μπορείτε να τροποποιήσετε την εφαρμογή προσομοιωμένη συσκευή που δημιουργήσατε στην εκμάθηση [Γρήγορα αποτελέσματα με το Κέντρο IoT] για την αποστολή μηνυμάτων αλληλεπιδραστικών συσκευής στο cloud στο διανομέα IoT.

1. Στο Visual Studio, μέσα στο έργο **SimulatedDevice** , προσθέστε την ακόλουθη μέθοδο για την τάξη **πρόγραμμα** .

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    Αυτή η μέθοδος είναι παρόμοια με τη μέθοδο **SendDeviceToCloudMessagesAsync** του **SimulatedDevice** έργου. Οι διαφορές μόνο είναι ότι μπορείτε πλέον να ορίσετε την ιδιότητα συστήματος **αναγνωριστικού μηνύματος** και μια ιδιότητα χρήστη που ονομάζεται **Τύπος μηνύματος**.
    Ο κώδικας αντιστοιχίζει ένα καθολικά μοναδικό αναγνωριστικό (GUID) για την ιδιότητα **αναγνωριστικού μηνύματος** . Η υπηρεσία Bus να χρησιμοποιήσετε αυτό το αναγνωριστικό για να καταργήστε την αναπαραγωγή τα μηνύματα που λαμβάνει. Το δείγμα χρησιμοποιεί την ιδιότητα **Τύπος μηνύματος** για να διακρίνετε αλληλεπιδραστικών από μηνύματα σημείο δεδομένων. Η εφαρμογή μεταβιβάζει αυτές τις πληροφορίες στο παράθυρο διαλόγου Ιδιότητες μηνύματος αντί στο σώμα του μηνύματος, ώστε ο επεξεργαστής συμβάντων, δεν χρειάζεται να αποσειριοποίησης του μηνύματος για να εκτελέσετε δρομολόγηση μηνυμάτων.

    > [AZURE.NOTE] Είναι σημαντικό για τη δημιουργία του **αναγνωριστικού μηνύματος** που χρησιμοποιείται για την αναπαραγωγή καταργήστε την αλληλεπιδραστική μηνύματα στον κώδικα συσκευή. Επικοινωνίες περιστασιακό δικτύου ή άλλες αποτυχίες, θα μπορούσε να έχει ως αποτέλεσμα πολλές αναμετάδοση το ίδιο μήνυμα από αυτήν τη συσκευή. Μπορείτε επίσης να χρησιμοποιήσετε ένα Αναγνωριστικό σημασιολογίας μηνύματος, όπως ο κατακερματισμός των πεδίων δεδομένων σχετικό μήνυμα, στη θέση GUID.

2. Προσθέστε την ακόλουθη μέθοδο στη μέθοδο **κύριες** , αμέσως πριν την `Console.ReadLine()` γραμμής:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Για λόγους απλούστευσης, αυτό το πρόγραμμα εκμάθησης δεν υλοποιεί οποιαδήποτε πολιτική "Επανάληψη". Στον κώδικα παραγωγής, θα πρέπει να εφαρμόζουν μια πολιτική "Επανάληψη" όπως εκθετική διπλασιασμών, όπως προτείνεται στο άρθρο MSDN [Μεταβατικές χειρισμού σφαλμάτων].

## <a name="process-device-to-cloud-messages"></a>Διεργασία μηνύματα συσκευής στο cloud

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας Windows που επεξεργάζεται τη συσκευή για να cloud μηνύματα από διανομέα IoT. Διανομέα IOT εκθέτει ένα [διανομέα συμβάν]-συμβατά τελικό σημείο για να ενεργοποιήσετε μια εφαρμογή για την ανάγνωση μηνυμάτων συσκευής στο cloud. Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί την κλάση [EventProcessorHost] για την επεξεργασία αυτών των μηνυμάτων σε μια εφαρμογή κονσόλας. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να επεξεργαστείτε μηνύματα από διανομείς συμβάντος, ανατρέξτε στο θέμα το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με το συμβάν διανομείς] .

Το δύσκολο κατά την υλοποίηση της αξιόπιστης αποθήκευσης των μηνυμάτων σημείο δεδομένων ή την προώθηση των μηνυμάτων αλληλεπιδραστικών είναι η επεξεργασία συμβάντος εξαρτάται από τον καταναλωτή μήνυμα για την παροχή σημεία ελέγχου για την πρόοδο του έργου. Επιπλέον, για να επιτύχετε μια υψηλής απόδοσης και κατά την ανάγνωση από το συμβάν διανομείς θα πρέπει να παρέχετε σημεία ελέγχου σε δέσμες με μεγάλο. Αυτή η προσέγγιση δημιουργεί την πιθανότητα των διπλότυπων επεξεργασίας για μεγάλο αριθμό των μηνυμάτων εάν υπάρχει ένα σφάλμα και να επαναφέρετε το προηγούμενο σημείο ελέγχου. Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δείτε πώς μπορείτε να συγχρονίσετε αποθήκευσης Azure εγγραφές και υπηρεσία Bus άρσεως αναπαραγωγή windows με σημεία ελέγχου **EventProcessorHost** .

Για να συντάξετε μηνύματα με το χώρο αποθήκευσης Azure αξιόπιστα, το δείγμα χρησιμοποιεί τη δυνατότητα ολοκλήρωσης μεμονωμένα μπλοκ με [αντικείμενα BLOB μπλοκ][Azure Block Blobs]. Ο επεξεργαστής συμβάντων συγκεντρώνει μηνύματα στη μνήμη μέχρι να είναι καιρός να παρέχουν ένα σημείο ελέγχου. Για παράδειγμα, αφού το αθροιστικό buffer μηνυμάτων φτάσει το μέγιστο μέγεθος μπλοκ 4 MB ή μετά την υπηρεσία Bus άρσεως αναπαραγωγής που περνά χρονικού διαστήματος. Στη συνέχεια, πριν από το σημείο ελέγχου, τον κωδικό δεσμεύεται ενός νέου μπλοκ για το αντικείμενο blob.

Ο επεξεργαστής συμβάντων χρησιμοποιεί διανομείς συμβάν Μετατοπίζει το μήνυμα ως μπλοκ αναγνωριστικά. Αυτός ο μηχανισμός επιτρέπει στον επεξεργαστή συμβάντος για να εκτελέσετε ένα σημάδι ελέγχου αναπαραγωγής άρσεως πριν από την δεσμεύεται το νέο μπλοκ με το χώρο αποθήκευσης, φροντίζοντας πιθανές σφάλμα μεταξύ την οριστικοποίηση των ένα μπλοκ και το σημείο ελέγχου.

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί ένα λογαριασμό αποθήκευσης Azure για να γράψετε όλα τα μηνύματα που ανακτήθηκαν από διανομέα IoT. Για να αποφασίσετε εάν πρέπει να χρησιμοποιήσετε πολλούς λογαριασμούς αποθήκευσης Azure στη λύση σας, ανατρέξτε στο θέμα [αποθήκευσης Azure κλιμάκωση κατευθυντήριες γραμμές].

Η εφαρμογή χρησιμοποιεί τη λειτουργία αναπαραγωγής άρσεως Bus υπηρεσίας για να αποφύγετε διπλότυπες τιμές όταν επεξεργάζεται αλληλεπιδραστικών μηνύματα. Η συσκευή προσομοιωμένη Αποτυπώνει κάθε αλληλεπιδραστικών μήνυμα με ένα μοναδικό **αναγνωριστικού μηνύματος**. Αυτά τα αναγνωριστικά Ενεργοποίηση Bus υπηρεσίας για να βεβαιωθείτε ότι, στο παράθυρο ώρας καθορισμένες άρσεως αναπαραγωγής, χωρίς δύο μηνύματα με το ίδιο **αναγνωριστικού μηνύματος** θα παραδίδονται στο δέκτη. Αυτό άρσεως αναπαραγωγή, μαζί με τη σημασιολογία ολοκλήρωσης ανά μήνυμα που παρέχεται από την υπηρεσία Bus ουρές, διευκολύνει την υλοποίηση την αξιόπιστη επεξεργασία των αλληλεπιδραστικών μηνυμάτων.

Για να βεβαιωθείτε ότι κανένα μήνυμα έχει υποβληθεί εκ νέου εκτός του παραθύρου άρσεως αναπαραγωγής, τον κωδικό συγχρονίζει το μηχανισμό σημείο ελέγχου **EventProcessorHost** με το παράθυρο Bus υπηρεσίας ουράς άρσεως αναπαραγωγής. Αυτός ο συγχρονισμός γίνεται από να επιβάλετε ένα σημείο ελέγχου τουλάχιστον μία φορά κάθε φορά που το παράθυρο άρσεως αναπαραγωγής που περνά (σε αυτό το πρόγραμμα εκμάθησης, το παράθυρο είναι μία ώρα).

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί μια μοναδική διαμερίσματα ουρά Bus υπηρεσίας όλα τα μηνύματα αλληλεπιδραστικών ανακτήθηκαν από διανομέα IoT στη διαδικασία. Για περισσότερες πληροφορίες σχετικά με τη χρήση υπηρεσιών Bus ουρές να πληροί τις απαιτήσεις κλιμάκωση της λύσης σας, ανατρέξτε στην τεκμηρίωση [Bus υπηρεσίας Azure] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Προμήθεια λογαριασμό αποθήκευσης Azure και μια ουρά Bus υπηρεσίας
Για να χρησιμοποιήσετε την κλάση [EventProcessorHost] , πρέπει να έχετε ένα λογαριασμό αποθήκευσης Azure για να ενεργοποιήσετε το **EventProcessorHost** για την καταγραφή πληροφοριών σημείο ελέγχου. Μπορείτε να χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό αποθήκευσης Azure ή, ακολουθήστε τις οδηγίες στο θέμα [Σχετικά με το χώρο αποθήκευσης Azure] για να δημιουργήσετε ένα νέο. Σημειώστε τη συμβολοσειρά σύνδεσης λογαριασμός Azure χώρου αποθήκευσης.

> [AZURE.NOTE] Όταν κάνετε αντιγραφή και επικόλληση τη συμβολοσειρά σύνδεσης λογαριασμός Azure χώρου αποθήκευσης, βεβαιωθείτε ότι υπάρχουν κενά διαστήματα περιλαμβάνονται.

Επίσης, χρειάζεστε μια ουρά Bus υπηρεσίας για να ενεργοποιήσετε την αξιόπιστη επεξεργασία των αλληλεπιδραστικών μηνυμάτων. Μπορείτε να δημιουργήσετε μια ουρά μέσω προγραμματισμού, με ένα παράθυρο άρσεως αναπαραγωγής μία ώρα, όπως εξηγείται στο θέμα [Πώς να χρησιμοποιείτε ουρές Bus υπηρεσίας][ουράς Bus υπηρεσίας]. Εναλλακτικά, μπορείτε να χρησιμοποιήσετε την [πύλη του Azure κλασική][lnk-classic-portal], ακολουθώντας τα παρακάτω βήματα:

1. Στην κάτω αριστερή γωνία, κάντε κλικ στην επιλογή **Δημιουργία** . Στη συνέχεια, κάντε κλικ στην επιλογή **Εφαρμογή υπηρεσιών** > **Bus υπηρεσίας** > **ουρά** > **Δημιουργία προσαρμοσμένης**. Πληκτρολογήστε το όνομα **d2ctutorial**, επιλέξτε μια περιοχή, και χρησιμοποιήστε έναν υπάρχοντα χώρο ονομάτων ή δημιουργήστε ένα νέο. Στην επόμενη σελίδα, επιλέξτε **Ενεργοποίηση εντοπισμού διπλοτύπων**και ορίστε την **αναπαραγωγή εντοπισμού ιστορικού χρονικού διαστήματος** σε μία ώρα. Στη συνέχεια, κάντε κλικ στο σημάδι ελέγχου στην κάτω δεξιά γωνία για να αποθηκεύσετε τη ρύθμιση των παραμέτρων σας ουρά.

    ![Δημιουργία ουράς στην πύλη του Azure][30]

2. Στη λίστα των ουρές Bus υπηρεσίας, κάντε κλικ στην επιλογή **d2ctutorial**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**. Δημιουργήστε δύο πολιτικές κοινόχρηστη πρόσβαση, μία που ονομάζεται **Αποστολή** με δικαιώματα **αποστολής** και έναν που ονομάζεται **ακρόαση** με **ακρόαση** δικαιώματα. Όταν ολοκληρώσετε, κάντε κλικ στην επιλογή " **Αποθήκευση** " στο κάτω μέρος.

    ![Ρύθμιση παραμέτρων μιας ουράς στην πύλη του Azure][31]

3. Κάντε κλικ στην επιλογή **Πίνακας εργαλείων** στο επάνω και, στη συνέχεια, **πληροφορίες σύνδεσης** στο κάτω μέρος. Σημειώστε τη σύνδεση δύο συμβολοσειρών.

    ![Πίνακας εργαλείων ουρά στην πύλη του Azure][32]

### <a name="create-the-event-processor"></a>Δημιουργία ο επεξεργαστής συμβάντων

1. Στην τρέχουσα λύση Visual Studio, για να δημιουργήσετε ένα έργο Visual C# Windows χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** , κάντε κλικ στην επιλογή **αρχείο** > **Προσθήκη** > **Νέο έργο**. Βεβαιωθείτε ότι η έκδοση του .NET Framework είναι 4.5.1 ή νεότερη έκδοση. Όνομα του έργου **ProcessDeviceToCloudMessages**και κάντε κλικ στο κουμπί **OK**.

    ![Νέο έργο στο Visual Studio][10]

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **ProcessDeviceToCloudMessages** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**. Εμφανίζεται το παράθυρο διαλόγου **Διαχείριση πακέτου NuGet** .

3. Αναζήτηση για **WindowsAzure.ServiceBus**, κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης. Αυτή η λειτουργία στοιχεία λήψης, εγκαθιστά και προσθέτει μια αναφορά στο [πακέτο Azure Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus), με όλες τις εξαρτήσεις του.

4. Αναζήτηση για **Microsoft.Azure.ServiceBus.EventProcessorHost**, κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης. Αυτή η λειτουργία στοιχεία λήψης, εγκαθιστά και προσθέτει μια αναφορά σε [Διανομέα Bus συμβάν Azure Service - EventProcessorHost NuGet πακέτου](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), με όλες τις εξαρτήσεις του.

5. Κάντε δεξί κλικ στο έργο **ProcessDeviceToCloudMessages** , κάντε κλικ στην επιλογή **Προσθήκη**και, στη συνέχεια, κάντε κλικ στην επιλογή **τάξης**. Ονομάστε τη νέα κλάση **StoreEventProcessor**και, στη συνέχεια, κάντε κλικ στο κουμπί **OK** για να δημιουργήσετε την κλάση.

6. Προσθέστε τις παρακάτω προτάσεις στο επάνω μέρος του αρχείου StoreEventProcessor.cs:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. Αντικαταστήστε τον παρακάτω κώδικα για το σώμα της κλάσης:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    Η κλάση **EventProcessorHost** κλήσεις αυτής της κλάσης για την επεξεργασία μηνυμάτων συσκευής στο cloud που έχουν ληφθεί από διανομέα IoT. Ο κώδικας σε αυτήν την κλάση υλοποιεί της λογικής για την αποθήκευση αξιόπιστα μηνύματα σε ένα κοντέινερ αντικειμένων blob και προώθηση μηνυμάτων αλληλεπιδραστικών στην ουρά Bus υπηρεσίας.

    Η μέθοδος **OpenAsync** προετοιμάζει τη μεταβλητή **currentBlockInitOffset** , που παρακολουθεί την τρέχουσα μετατόπιση του πρώτου μηνύματος διαβάζονται από αυτόν τον επεξεργαστή συμβάν. Να θυμάστε ότι κάθε επεξεργαστή είναι υπεύθυνος για μια μεμονωμένη διαμερίσματα.

    Η μέθοδος **ProcessEventsAsync** λαμβάνει μια δέσμη μηνυμάτων από το Κέντρο IoT και τα επεξεργάζεται, ως εξής: στέλνει μηνύματα αλληλεπιδραστικών στην ουρά Bus υπηρεσίας και τοποθετεί μηνυμάτων σημείο δεδομένων στο buffer μνήμης που ονομάζεται **toAppend**. Εάν το buffer μνήμης φτάσει το όριο των 4 MB ή τα windows ώρα άρσεως αναπαραγωγής που περνά (μία ώρα μετά από ένα σημείο ελέγχου σε αυτό το πρόγραμμα εκμάθησης), στη συνέχεια, η εφαρμογή ενεργοποιεί ένα σημείο ελέγχου.

    Η μέθοδος **AppendAndCheckpoint** δημιουργεί πρώτα μια blockId για το μπλοκ για να προσαρτήσετε. Azure αποθήκευσης απαιτεί όλα αποκλεισμός αναγνωριστικά να έχουν το ίδιο μήκος, ώστε η μέθοδος pads τη μετατόπιση με αρχικά μηδενικά - `currentBlockInitOffset.ToString("0000000000000000000000000")`. Στη συνέχεια, εάν ένα μπλοκ με αυτό το Αναγνωριστικό είναι ήδη στο αντικείμενο blob, η μέθοδος αντικαθιστά το με τα τρέχοντα περιεχόμενα του buffer.

    > [AZURE.NOTE] Για να απλοποιήσετε τον κώδικα, αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί ένα μεμονωμένο blob ανά διαμερίσματα για την αποθήκευση των μηνυμάτων. Μια λύση πραγματικό να υλοποιήσετε σάρωση με τη δημιουργία πρόσθετων αρχείων μετά από ένα συγκεκριμένο χρονικό διάστημα ή μετά από ένα συγκεκριμένο μέγεθος αρχείου. Να θυμάστε ότι ένα αντικειμένων blob του Azure μπλοκ μπορεί να περιέχει πολύ 195 GB δεδομένων.

8. Στην τάξη **προγράμματος** , προσθέστε την ακόλουθη πρόταση **με χρήση** στο επάνω μέρος:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. Τροποποιήστε τη μέθοδο **κύριες** στην τάξη **πρόγραμμα** ως εξής. Αντικαταστήστε **{συμβολοσειρά σύνδεσης διανομέα iot}** με τη συμβολοσειρά σύνδεσης **iothubowner** από το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με το Κέντρο IoT] . Αντικαταστήστε τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης με τη συμβολοσειρά σύνδεσης που σημειώσατε κατά την έναρξη αυτής της ενότητας. Αντικαταστήστε τη συμβολοσειρά σύνδεσης υπηρεσίας Bus με δικαιώματα **αποστολής** για την ουρά με το όνομα **d2ctutorial** σημειώσατε κατά την έναρξη της αυτήν την ενότητα:

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] Για λόγους απλούστευσης, αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί μια μεμονωμένη παρουσία της κλάσης [EventProcessorHost] . Για περισσότερες πληροφορίες, ανατρέξτε στον [Οδηγό προγραμματισμού συμβάντων διανομείς].

## <a name="receive-interactive-messages"></a>Λήψη αλληλεπιδραστικών μηνυμάτων
Σε αυτήν την ενότητα, μπορείτε να συντάξετε μια εφαρμογή κονσόλας Windows που λαμβάνει το διαδραστικό μηνύματα από την υπηρεσία Bus ουρά. Για περισσότερες πληροφορίες σχετικά με τον τρόπο αρχιτεκτονική μια λύση χρησιμοποιώντας Bus υπηρεσίας, ανατρέξτε στο θέμα [Δημιουργία εφαρμογές πολλαπλών επιπέδων με Bus υπηρεσίας][].

1. Στην τρέχουσα λύση Visual Studio, δημιουργήστε ένα έργο Visual C# Windows χρησιμοποιώντας το πρότυπο έργου **Εφαρμογής κονσόλας** . Το όνομα του έργου **ProcessD2CInteractiveMessages**.

2. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο **ProcessD2CInteractiveMessages** και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**. Αυτή η λειτουργία εμφανίζει το παράθυρο **Διαχείριση πακέτου NuGet** .

3. Αναζήτηση για **WindowsAzure.ServiceBus**, κάντε κλικ στην επιλογή **εγκατάσταση**και αποδεχτείτε τους όρους χρήσης. Αυτή η λειτουργία στοιχεία λήψης, εγκαθιστά και προσθέτει μια αναφορά σε [Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus), με όλες τις εξαρτήσεις του.

4. Προσθέστε τις παρακάτω προτάσεις **Χρήση** στο επάνω μέρος του αρχείου **Program.cs** :

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Τέλος, προσθέστε τις ακόλουθες γραμμές στη μέθοδο **κύριες** . Αντικαταστήστε τη συμβολοσειρά σύνδεσης με **ακρόαση** δικαιώματα για την ουρά με το όνομα **d2ctutorial**:

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Εκτελέστε τις εφαρμογές

Τώρα είστε έτοιμοι να εκτελέσετε τις εφαρμογές.

1.  Στο Visual Studio, στην Εξερεύνηση λύσεων, κάντε δεξί κλικ τη λύση σας και επιλέξτε **Ορισμός εκκίνησης έργα**. Επιλέξτε **πολλά έργα εκκίνησης**και, στη συνέχεια, επιλέξτε **Έναρξη** με την ενέργεια για τα έργα **ProcessDeviceToCloudMessages**, **SimulatedDevice**και **ProcessD2CInteractiveMessages** .

2.  Πατήστε το πλήκτρο **F5** για να ξεκινήσετε τις εφαρμογές τρεις κονσόλας. Η εφαρμογή **ProcessD2CInteractiveMessages** πρέπει να επεξεργαστεί κάθε αλληλεπιδραστικών μήνυμα που αποστέλλεται από την εφαρμογή **SimulatedDevice** .

  ![Τρεις εφαρμογές κονσόλας][50]

> [AZURE.NOTE] Για να δείτε ενημερώσεις στο blob σας, ίσως χρειαστεί να μειώσετε τη σταθερά **MAX_BLOCK_SIZE** στην τάξη **StoreEventProcessor** μικρότερη τιμή, όπως **1024**. Αυτή η αλλαγή είναι χρήσιμη, επειδή θα χρειαστεί κάποιος χρόνος για την επίτευξη το όριο μεγέθους μπλοκ με τα δεδομένα που αποστέλλονται από τη συσκευή προσομοιωμένη. Με μικρότερο μέγεθος μπλοκ, δεν χρειάζεται να περιμένετε τόσο πολύ για να δείτε το αντικείμενο blob που δημιουργούνται και ενημερώνονται. Ωστόσο, χρησιμοποιώντας ένα μεγαλύτερο μέγεθος μπλοκ κάνει την εφαρμογή πιο μεταβλητού μεγέθους.

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς να αξιόπιστα επεξεργαστείτε σημείου δεδομένων και αλληλεπιδραστικών μηνύματα συσκευής στο cloud χρησιμοποιώντας την κλάση [EventProcessorHost] .

[Πώς μπορείτε να στέλνετε μηνύματα cloud-σε-συσκευή με το Κέντρο IoT] [ lnk-c2d] σας δείχνει πώς μπορείτε να στείλετε μηνύματα στις συσκευές σας από το παρασκηνίου.

Για να δείτε παραδείγματα ολοκλήρωσης για ολοκληρωμένες λύσεις που χρησιμοποιούν IoT διανομέα, ανατρέξτε στο θέμα [Οικογένεια IoT Azure][lnk-suite].

Για να μάθετε περισσότερα σχετικά με την ανάπτυξη λύσεων με IoT διανομέα, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές IoT διανομέα].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Χώρος αποθήκευσης αντικειμένων blob του Azure]: ../storage/storage-dotnet-how-to-use-blobs.md
[Εργοστασιακές Azure δεδομένων]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Bus υπηρεσίας Ουράς]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Οδηγός Azure προγραμματιστής διανομέα IoT - συσκευή με το cloud]: iot-hub-devguide-messaging.md

[Azure χώρου αποθήκευσης]: https://azure.microsoft.com/documentation/services/storage/
[Bus Azure υπηρεσίας]: https://azure.microsoft.com/documentation/services/service-bus/

[Οδηγός για προγραμματιστές IoT διανομέα]: iot-hub-devguide.md
[Γρήγορα αποτελέσματα με το Κέντρο IoT]: iot-hub-csharp-csharp-getstarted.md
[Κέντρο για προγραμματιστές IoT Azure]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Χειρισμός μεταβατικές σφαλμάτων]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Σχετικά με το χώρο αποθήκευσης στο Azure]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Γρήγορα αποτελέσματα με το συμβάν διανομείς]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure αποθήκευσης κλιμάκωση κατευθυντήριες γραμμές]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Οδηγός προγραμματισμού διανομείς συμβάντος]: ../event-hubs/event-hubs-programming-guide.md
[Χειρισμός μεταβατικές σφαλμάτων]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Δημιουργήστε εφαρμογές πολλαπλών επιπέδων με Bus υπηρεσίας]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
