<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας με Java | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας στο Azure. Δείγματα κώδικα γραμμένο σε Java."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Σε αυτό το άρθρο περιγράφει τον τρόπο χρήσης της υπηρεσίας Bus ουρές. Τα δείγματα που είναι γραμμένες σε Java και χρησιμοποιήστε το [Azure SDK για Java][]. Τα σενάρια καλύπτεται περιλαμβάνουν **τη δημιουργία ουρές**, **Αποστολή και λήψη μηνυμάτων**και **τη διαγραφή ουρές**.

## <a name="what-are-service-bus-queues"></a>Τι είναι η υπηρεσία Bus ουρές;

Υπηρεσία Bus ουρές υποστηρίζουν μοντέλου επικοινωνίας **όπου μεσολαβούν μηνυμάτων** . Όταν χρησιμοποιείτε ουρές, στοιχεία μια κατανεμημένη εφαρμογή δεν επικοινωνούν απευθείας μεταξύ τους; αντί για αυτό ανταλλαγή μηνυμάτων μέσω μια ουρά, η οποία λειτουργεί ως ενδιάμεσο (broker). Πρόγραμμα παραγωγής των μηνυμάτων (αποστολέα) παραδίδει ένα μήνυμα στην ουρά και, στη συνέχεια, συνεχίζει την επεξεργασία.
Ασύγχρονη, καταναλωτή μήνυμα (ακουστικό) Μετακινεί το μήνυμα από την ουρά και το επεξεργάζεται. Το πρόγραμμα παραγωγής των δεν πρέπει να περιμένουν για μια απάντηση από τον καταναλωτή για να συνεχίσετε να επεξεργάζονται και περαιτέρω αποστολή μηνυμάτων. Ουρές προσφέρουν **πρώτη στο, πρώτη ανάληψη FIFO** παράδοση του μηνύματος σε μία ή περισσότερες ανταγωνιστικών καταναλωτές. Αυτό σημαίνει ότι τα μηνύματα συνήθως λάβει και υποβάλλονται σε επεξεργασία από το δέκτες με τη σειρά που προστέθηκαν στην ουρά και κάθε μήνυμα είναι λάβει και να υποβάλλονται σε επεξεργασία από ένα μόνο μήνυμα καταναλωτή.

![QueueConcepts](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Υπηρεσία Bus ουρές είναι μια γενικής χρήσης τεχνολογία που μπορούν να χρησιμοποιηθούν για μια μεγάλη ποικιλία σενάρια:

- Επικοινωνία μεταξύ ρόλους web και εργαζόμενου σε μια εφαρμογή του Azure πολλαπλών επιπέδων.
- Επικοινωνία μεταξύ εφαρμογών εσωτερικής εγκατάστασης και του Azure φιλοξενούνται εφαρμογές σε μια υβριδική λύση.
- Επικοινωνία μεταξύ των στοιχείων μιας εφαρμογής κατανέμεται εκτέλεση εσωτερικής εγκατάστασης σε διαφορετικές εταιρείες ή τμήματα ενός οργανισμού.

Χρήση ουρές σάς επιτρέπει να κλιμακωθεί ανάληψη τις εφαρμογές σας πιο εύκολα και να ενεργοποιήσετε την ανοχή στην αρχιτεκτονική.

## <a name="create-a-service-namespace"></a>Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας

Για να ξεκινήσετε να χρησιμοποιείτε ουρές Bus υπηρεσίας στο Azure, πρέπει πρώτα να δημιουργήσετε ένα χώρο ονομάτων. Ένα χώρο ονομάτων παρέχει ένα κοντέινερ πεδίου για την προσθήκη διεύθυνσης σε πόρους Bus υπηρεσίας στην εφαρμογή σας.

Για να δημιουργήσετε ένα χώρο ονομάτων:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για να χρησιμοποιήσετε Bus υπηρεσίας

Βεβαιωθείτε ότι έχετε εγκαταστήσει το [Azure SDK για Java][] πριν από τη δημιουργία αυτού του δείγματος. Εάν χρησιμοποιείτε Έκλειψη, μπορείτε να εγκαταστήσετε το [Κιτ εργαλείων Azure για Έκλειψη][] που περιλαμβάνει το Azure SDK για Java. Στη συνέχεια, μπορείτε να προσθέσετε το **Microsoft Azure βιβλιοθηκών για Java** στο έργο σας:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Προσθέστε τα ακόλουθα `import` δηλώσεις στο επάνω μέρος του αρχείου Java:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Δημιουργία ουράς

Λειτουργίες διαχείρισης για ουρές Bus υπηρεσίας μπορεί να πραγματοποιηθεί μέσω της κλάσης **ServiceBusContract** . Ένα αντικείμενο **ServiceBusContract** έχει συνταχθεί με την κατάλληλη ρύθμιση παραμέτρων που ενσωματώνει το διακριτικό συσχετισμών Ασφαλείας με δικαιώματα για τη διαχείριση και την κλάση **ServiceBusContract** είναι το μοναδικό σημείο επικοινωνίας με Azure.

Η κλάση **ServiceBusService** παρέχει μεθόδους για να δημιουργήσετε, απαρίθμηση και διαγραφή ουρές. Το παρακάτω παράδειγμα δείχνει πώς μπορεί να χρησιμοποιηθεί ένα αντικείμενο **ServiceBusService** για να δημιουργήσετε μια ουρά με το όνομα "TestQueue", με ένα χώρο ονομάτων με το όνομα "HowToSample":

```
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Υπάρχουν μέθοδοι σε **QueueInfo** που επιτρέπουν ιδιότητες της ουράς για να απενεργοποιήσει (για παράδειγμα: για να ορίσετε την προεπιλεγμένη time to live (TTL) τιμή για να εφαρμοστεί σε μηνύματα που αποστέλλονται σε ουρά). Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια ουρά με το όνομα `TestQueue` με μέγιστο μέγεθος του 5GB:

````
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Σημειώστε ότι μπορείτε να χρησιμοποιήσετε τη μέθοδο **listQueues** σε αντικείμενα **ServiceBusContract** για να ελέγξετε εάν υπάρχει ήδη μια ουρά με ένα καθορισμένο όνομα μέσα σε ένα χώρο ονομάτων υπηρεσίας.

## <a name="send-messages-to-a-queue"></a>Αποστολή μηνυμάτων σε μια ουρά

Για να στείλετε ένα μήνυμα σε ουρά Bus υπηρεσίας, η εφαρμογή σας λαμβάνει ένα αντικείμενο **ServiceBusContract** . Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να στείλετε ένα μήνυμα το `TestQueue` προηγουμένως δημιουργήσει στην ουρά του `HowToSample` χώρος ονομάτων:

```
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Αποστολή μηνυμάτων και λάβατε από την υπηρεσία Bus ουρές είναι παρουσίες της κλάσης [BrokeredMessage][] . Τα αντικείμενα [BrokeredMessage][] έχουν ένα σύνολο τυπικές ιδιότητες (όπως [ετικέτα](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) και [το TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), ένα λεξικό που χρησιμοποιείται για τη διατήρηση προσαρμοσμένη εφαρμογή συγκεκριμένες ιδιότητες, και ένα κύριο σώμα των δεδομένων αυθαίρετο εφαρμογής. Μια εφαρμογή του, να ορίσετε το σώμα του μηνύματος, μεταφέροντας οποιοδήποτε αντικείμενο σειριοποιηθεί στην κατασκευή του το [BrokeredMessage][]και το κατάλληλο πρόγραμμα σειριοποίησης θα χρησιμοποιηθεί, στη συνέχεια, να σειριοποίηση του αντικειμένου. Εναλλακτικά, μπορείτε να δώσετε μια java **. ΕΊΣΟΔΟΣ/ΈΞΟΔΟΣ. InputStream** αντικειμένου.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να στείλετε πέντε δοκιμαστικών μηνυμάτων για να το `TestQueue` **MessageSender** μας που λαμβάνονται στο προηγούμενο τμήμα κώδικα:

```
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Υπηρεσία Bus ουρές υποστηρίζουν μέγιστο μέγεθος αρχείου των 256 KB η [τυπική σειρά](service-bus-premium-messaging.md) και 1 MB στο [επίπεδο Premium](service-bus-premium-messaging.md). Την επικεφαλίδα, η οποία περιλαμβάνει τις ιδιότητες τυπικών και προσαρμοσμένων εφαρμογών, μπορεί να έχει ένα μέγιστο μέγεθος των 64 KB. Υπάρχει όριο στον αριθμό των μηνυμάτων που θα διατηρούνται σε ουρά, αλλά υπάρχει μια καπέλο το συνολικό μέγεθος των μηνυμάτων που διαθέτει μια ουρά. Αυτό το μέγεθος ουρά ορίζεται κατά τη δημιουργία, με ένα ανώτερο όριο των 5 GB.

## <a name="receive-messages-from-a-queue"></a>Λήψη μηνυμάτων από μια ουρά

Ο κύριος τρόπος για να λάβετε μηνύματα από μια ουρά είναι να χρησιμοποιήσετε ένα αντικείμενο **ServiceBusContract** . Μηνύματα που λαμβάνετε να εργαστείτε σε δύο διαφορετικές καταστάσεις λειτουργίας: **ReceiveAndDelete** και **PeekLock**.

Όταν λαμβάνετε χρησιμοποιώντας τη λειτουργία **ReceiveAndDelete** , είναι μια λειτουργία μίας στιγμιότυπο - δηλαδή, όταν Bus υπηρεσία λαμβάνει ένα αποδεικτικό ανάγνωσης για ένα μήνυμα σε μια ουρά, το επισημαίνει το μήνυμα ως που καταναλώνεται και επιστρέφει στην εφαρμογή. Η λειτουργία **ReceiveAndDelete** (που είναι η προεπιλεγμένη κατάσταση λειτουργίας) είναι το μοντέλο απλούστερος και λειτουργεί καλύτερα για σενάρια στις οποίες μια εφαρμογή μπορεί να αποδεχτεί τις δεν επεξεργασία ενός μηνύματος σε περίπτωση αποτυχίας. Για να κατανοήσετε αυτήν, εξετάστε το ενδεχόμενο να ένα σενάριο που ζητήματα της αίτησης λήψης του καταναλωτή και, στη συνέχεια, παρουσιάζει σφάλμα πριν από την επεξεργασία του.
Επειδή η υπηρεσία Bus θα έχουν σήμανση του μηνύματος ως που καταναλώνεται, στη συνέχεια, όταν η εφαρμογή επανεκκίνηση του και αρχίζει κατανάλωση ξανά τα μηνύματα, το θα έχετε παραβλέψει το μήνυμα που καταναλώθηκε πριν από το σφάλμα.

Στη λειτουργία **PeekLock** , λαμβάνετε γίνεται μια λειτουργία δύο στάδιο, γεγονός που επιτρέπει σε εφαρμογές υποστήριξης που δεν θέλετε να γίνει μηνύματα που λείπουν. Όταν Bus υπηρεσία λαμβάνει μια αίτηση, βρίσκει στο επόμενο μήνυμα να καταναλωθεί, κλειδώνει για να αποτρέψετε άλλους καταναλωτές πραγματοποιεί λήψη του και, στη συνέχεια, επιστρέφει το στην εφαρμογή. Μετά την εφαρμογή ολοκληρώσει την επεξεργασία του μηνύματος (ή αποθηκεύει αξιόπιστα για μελλοντική επεξεργασία), ολοκληρώνει δεύτερου σταδίου της διαδικασίας παραλαβής καλώντας την **Διαγραφή** του ληφθέντος μηνύματος. Όταν Bus υπηρεσίας βλέπει την κλήση **Διαγραφή** , θα επισημάνετε το μήνυμα ως που καταναλώνεται και να την καταργήσετε από την ουρά.

Το παρακάτω παράδειγμα παρουσιάζει πώς μπορούν να ληφθούν τα μηνύματα και να υποβάλλονται σε επεξεργασία με χρήση της κατάστασης λειτουργίας **PeekLock** (όχι η προεπιλεγμένη κατάσταση λειτουργίας). Το παρακάτω παράδειγμα η κατάσταση συνεχούς επανάληψης και επεξεργάζεται μηνύματα κατά την άφιξή σε μας "TestQueue":

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Πώς να χειριστείτε σφάλματα εφαρμογών και αναγνώσιμα μηνύματα

Υπηρεσία Bus παρέχει λειτουργικότητα που θα σας βοηθήσουν να ανακτήσετε ομαλά από σφάλματα στο εφαρμογής ή δυσκολίες επεξεργασία ενός μηνύματος. Εάν μια εφαρμογή ακουστικό είναι δυνατή η επεξεργασία του μηνύματος για κάποιο λόγο, στη συνέχεια, το να καλέσετε τη μέθοδο **unlockMessage** του ληφθέντος μηνύματος (αντί για τη μέθοδο **deleteMessage** ). Αυτό θα προκαλέσει Bus υπηρεσίας για να ξεκλειδώσετε το μήνυμα μέσα στην ουρά και να την κάνετε διαθέσιμη θα ληφθεί ξανά, με την ίδια εφαρμογή καταναλώνει ή από άλλη εφαρμογή καταναλώνει.

Υπάρχει επίσης ένα χρονικό όριο που σχετίζεται με ένα μήνυμα κλειδωμένο μέσα στην ουρά και, εάν η εφαρμογή δεν μπορεί να επεξεργαστεί το μήνυμα πριν από την κλείδωμα λήξει το χρονικό όριο (π.χ., εάν η εφαρμογή παρουσιάζει σφάλμα), στη συνέχεια, υπηρεσία Bus θα ξεκλειδώσετε αυτόματα το μήνυμα και να την κάνετε διαθέσιμη θα ληφθεί ξανά.

Σε περίπτωση που η εφαρμογή παρουσιάζει σφάλμα μετά την επεξεργασία του μηνύματος, αλλά πριν από την έκδοση της αίτησης **deleteMessage** , στη συνέχεια, το μήνυμα θα είναι redelivered στην εφαρμογή κατά την επανεκκίνηση. Συχνά αυτό ονομάζεται **Τουλάχιστον αφού επεξεργασίας**, αυτό σημαίνει ότι θα υποβληθούν τουλάχιστον μία φορά σε κάθε μήνυμα αλλά σε ορισμένες περιπτώσεις ίσως redelivered το ίδιο μήνυμα. Εάν το σενάριο δεν θέλετε να γίνει διπλότυπων επεξεργασίας, στη συνέχεια, προγραμματιστές εφαρμογών πρέπει να Προσθήκη επιπλέον λογικής σε εφαρμογή τους χειρισμού διπλότυπων μήνυμα παράδοσης. Συχνά, αυτό είναι δυνατό με τη μέθοδο **getMessageId** του μηνύματος, η οποία θα παραμένει σταθερή κατά μήκος προσπαθειών παράδοσης.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του ουρές Bus υπηρεσίας, ανατρέξτε στο θέμα [ουρές, θέματα, και συνδρομές][] για περισσότερες πληροφορίες.

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές Java](/develop/java/).


  [Azure SDK για Java]: http://azure.microsoft.com/develop/java/
  [Azure Κιτ εργαλείων για Έκλειψη]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Ουρές, θέματα και συνδρομών]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

