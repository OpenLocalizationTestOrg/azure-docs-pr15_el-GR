<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε τα θέματα υπηρεσίας Bus με Java | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε θέματα Bus υπηρεσίας και συνδρομές στο Azure. Δείγματα κώδικα έχουν εγγραφεί για εφαρμογές Java."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Πώς μπορείτε να χρησιμοποιήσετε θέματα Bus υπηρεσίας και συνδρομών

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Αυτός ο οδηγός περιγράφει τον τρόπο χρήσης της υπηρεσίας Bus θέματα και συνδρομών. Τα δείγματα που είναι γραμμένες σε Java και χρησιμοποιήστε το [Azure SDK για Java][]. Τα σενάρια καλύπτεται περιλαμβάνουν **τη δημιουργία θέματα και συνδρομές**, **τη δημιουργία συνδρομής φίλτρα**, **Αποστολή μηνυμάτων σε ένα θέμα**, **λαμβάνετε μηνύματα από μια συνδρομή**και **Διαγραφή θέματα και συνδρομών**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Τι είναι τα θέματα Bus υπηρεσίας και συνδρομές;

Θέματα υπηρεσίας Bus και συνδρομές υποστηρίζει μια *δημοσίευση/εγγραφή* ανταλλαγής μηνυμάτων μοντέλο επικοινωνίας. Όταν χρησιμοποιείτε τα θέματα και οι συνδρομές, στοιχεία μια κατανεμημένη εφαρμογή δεν επικοινωνούν απευθείας μεταξύ τους; αντί για αυτό ανταλλαγή μηνυμάτων μέσω ενός θέματος, η οποία λειτουργεί ως ενδιάμεσο.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Σε αντίθεση με αυτό ουρές Bus υπηρεσίας, όπου κάθε μήνυμα υποβάλλεται σε επεξεργασία από ένα μεμονωμένο καταναλωτή, θέματα και οι συνδρομές παρέχουν "ένα-προς-πολλά" φόρμα επικοινωνίας, χρησιμοποιώντας ένα μοτίβο δημοσίευση/εγγραφή. Είναι δυνατή η καταχώρηση πολλές συνδρομές σε ένα θέμα. Κατά την αποστολή ενός μηνύματος σε ένα θέμα, στη συνέχεια, γίνεται διαθέσιμη σε κάθε συνδρομής λαβή/στη διαδικασία ανεξάρτητα.

Μια συνδρομή σε ένα θέμα μοιάζει με μια εικονική ουρά που λαμβάνει αντίγραφα των μηνυμάτων που έχουν αποσταλεί στο θέμα. Προαιρετικά, μπορείτε να καταχωρήσετε κανόνες φιλτραρίσματος για ένα θέμα σε βάση ανά συνδρομή, η οποία σας επιτρέπει να φίλτρου/περιορισμός των μηνυμάτων σε ένα θέμα λαμβάνονται από ποιες συνδρομές το θέμα.

Θέματα υπηρεσίας Bus και συνδρομές σάς επιτρέπουν να κλιμακωθεί για την επεξεργασία πολύ μεγάλο αριθμό των μηνυμάτων σε πολύ μεγάλο αριθμό των χρηστών και των εφαρμογών.

## <a name="create-a-service-namespace"></a>Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας

Για να αρχίσετε να χρησιμοποιείτε τα θέματα Bus υπηρεσίας και συνδρομές στο Azure, πρέπει πρώτα να δημιουργήσετε ένα χώρο ονομάτων υπηρεσίας. Ένα χώρο ονομάτων παρέχει ένα κοντέινερ πεδίου για την προσθήκη διεύθυνσης σε πόρους Bus υπηρεσίας στην εφαρμογή σας.

Για να δημιουργήσετε ένα χώρο ονομάτων:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για να χρησιμοποιήσετε Bus υπηρεσίας

Βεβαιωθείτε ότι έχετε εγκαταστήσει το [Azure SDK για Java][] πριν από τη δημιουργία αυτού του δείγματος. Εάν χρησιμοποιείτε Έκλειψη, μπορείτε να εγκαταστήσετε το [Κιτ εργαλείων Azure για Έκλειψη][] που περιλαμβάνει το Azure SDK για Java. Στη συνέχεια, μπορείτε να προσθέσετε το **Microsoft Azure βιβλιοθηκών για Java** στο έργο σας:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Προσθέστε τις ακόλουθες δηλώσεις εισαγωγής στο επάνω μέρος του αρχείου Java:

```
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Προσθέστε τις βιβλιοθήκες Azure για Java σας διαδρομή Δόμηση και συμπεριλάβετε στο συγκρότησης ανάπτυξη του έργου.

## <a name="create-a-topic"></a>Δημιουργία θέματος

Λειτουργίες διαχείρισης για θέματα υπηρεσίας Bus μπορεί να πραγματοποιηθεί μέσω της κλάσης **ServiceBusContract** . Ένα αντικείμενο **ServiceBusContract** έχει συνταχθεί με την κατάλληλη ρύθμιση παραμέτρων που ενσωματώνει το διακριτικό συσχετισμών Ασφαλείας με δικαιώματα για τη διαχείριση και την κλάση **ServiceBusContract** είναι το μοναδικό σημείο επικοινωνίας με Azure.

Η κλάση **ServiceBusService** παρέχει μεθόδους για να δημιουργήσετε, απαρίθμηση και διαγραφή θέματα. Το παρακάτω παράδειγμα εμφανίζει τον τρόπο μπορεί να χρησιμοποιηθεί ένα αντικείμενο **ServiceBusService** για να δημιουργήσετε ένα θέμα με το όνομα `TestTopic`, με ένα χώρο ονομάτων που ονομάζεται `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Υπάρχουν μέθοδοι σε **TopicInfo** με δυνατότητα ιδιότητες του θέματος θα οριστεί (για παράδειγμα: για να ορίσετε την προεπιλεγμένη time to live (TTL) τιμή για να εφαρμοστεί σε μηνύματα που αποστέλλονται στο θέμα). Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε ένα θέμα με το όνομα `TestTopic` με μέγιστο μέγεθος του 5 GB:

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

Σημειώστε ότι μπορείτε να χρησιμοποιήσετε τη μέθοδο **listTopics** σε αντικείμενα **ServiceBusContract** για να ελέγξετε εάν υπάρχει ήδη ένα θέμα με ένα καθορισμένο όνομα μέσα σε ένα χώρο ονομάτων υπηρεσίας.

## <a name="create-subscriptions"></a>Δημιουργία εγγραφών

Συνδρομές σε θέματα δημιουργούνται επίσης με την κλάση **ServiceBusService** . Συνδρομές ονομάζονται και μπορούν να έχουν ένα προαιρετικό φίλτρο που περιορίζει το σύνολο των μηνυμάτων που του μεταβιβάστηκε εικονικού ουρά τη συνδρομή.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Δημιουργήστε μια συνδρομή με το φίλτρο προεπιλεγμένη (MatchAll)

Το φίλτρο **MatchAll** είναι το προεπιλεγμένο φίλτρο που χρησιμοποιείται, αν υπάρχει φίλτρο έχει καθοριστεί όταν δημιουργείται μια νέα συνδρομή. Όταν χρησιμοποιείται το φίλτρο **MatchAll** , όλα τα μηνύματα που δημοσιεύονται στο θέμα τοποθετούνται στην ουρά εικονικού τη συνδρομή. Το παρακάτω παράδειγμα δημιουργεί μια εγγραφή με όνομα "AllMessages" και χρησιμοποιεί το προεπιλεγμένο φίλτρο **MatchAll** .

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### <a name="create-subscriptions-with-filters"></a>Δημιουργία συνδρομές με τα φίλτρα

Μπορείτε επίσης να δημιουργήσετε τα φίλτρα που σας επιτρέπουν να εμβέλειας που θα πρέπει να εμφανίζονται τα μηνύματα που αποστέλλονται σε ένα θέμα μέσα σε μια συνδρομή στο συγκεκριμένο θέμα.

Η πιο ευέλικτη τύπο φίλτρου που υποστηρίζονται από συνδρομές είναι το [SqlFilter][], που εφαρμόζει ένα υποσύνολο SQL92. Φίλτρα SQL λειτουργούν σχετικά με τις ιδιότητες των μηνυμάτων που δημοσιεύονται στο θέμα. Για περισσότερες λεπτομέρειες σχετικά με τις παραστάσεις που μπορούν να χρησιμοποιηθούν με φίλτρο SQL, ελέγξτε τη σύνταξη [SqlFilter.SqlExpression][] .

Το ακόλουθο παράδειγμα δημιουργεί μια εγγραφή με όνομα `HighMessages` σε ένα αντικείμενο [SqlFilter][] που επιλέγει μόνο τα μηνύματα που έχουν μια προσαρμοσμένη ιδιότητα **αριθμού μηνύματος** που είναι μεγαλύτερη από 3:

```
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Ομοίως, το ακόλουθο παράδειγμα δημιουργεί μια εγγραφή με όνομα `LowMessages` σε ένα αντικείμενο [SqlFilter][] που επιλέγει μόνο τα μηνύματα που έχουν μια **αριθμού μηνύματος** ιδιότητα μικρότερη ή ίση με 3:

```
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Όταν ένα μήνυμα αποστέλλεται τώρα για να `TestTopic`, παραδίδονται πάντα σε μια συνδρομή στο δέκτες το `AllMessages` συνδρομή, και να παραδοθεί επιλεκτικής δέκτες έχετε εγγραφεί στο το `HighMessages` και `LowMessages` συνδρομές (ανάλογα με το περιεχόμενο του μηνύματος).

## <a name="send-messages-to-a-topic"></a>Αποστολή μηνυμάτων σε ένα θέμα

Για να στείλετε ένα μήνυμα σε ένα θέμα Bus υπηρεσίας, η εφαρμογή σας λαμβάνει ένα αντικείμενο **ServiceBusContract** . Ο ακόλουθος κώδικας περιγράφει πώς μπορείτε να στείλετε ένα μήνυμα το `TestTopic` θέμα δημιουργηθεί προηγουμένως το `HowToSample` χώρος ονομάτων:

```
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Τα μηνύματα που αποστέλλονται σε θέματα Bus υπηρεσίας είναι παρουσίες της κλάσης [BrokeredMessage][] . [BrokeredMessage][] *τα αντικείμενα έχουν ένα σύνολο τυπικές μεθόδους (όπως είναι οι * *setLabel* * και * *το TimeToLive**), ένα λεξικό που χρησιμοποιείται για τη διατήρηση προσαρμοσμένες ιδιότητες συγκεκριμένη εφαρμογή, και ένα κύριο σώμα των δεδομένων αυθαίρετο εφαρμογής. Μια εφαρμογή του, να ορίσετε το σώμα του μηνύματος, μεταφέροντας οποιοδήποτε αντικείμενο σειριοποιηθεί στην κατασκευή του τα [BrokeredMessage][]και τις κατάλληλες * *DataContractSerializer* * , στη συνέχεια, θα χρησιμοποιείται για σειριοποίηση του αντικειμένου. Εναλλακτικά, μια * *java.io.InputStream** μπορεί να παρέχεται.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να στείλετε πέντε δοκιμαστικών μηνυμάτων για να το `TestTopic` **MessageSender** μας που λαμβάνονται στο παραπάνω τμήμα κώδικα.
Παρατηρήστε πώς η τιμή της ιδιότητας **αριθμού μηνύματος** για κάθε μήνυμα ποικίλλει σε την επανάληψη του βρόχου (αυτό θα καθορίσει ποιες συνδρομές λαμβάνουν την):

```
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Θέματα υπηρεσίας Bus υποστηρίζει ένα μέγιστο μέγεθος μηνύματος 256 KB η [τυπική σειρά](service-bus-premium-messaging.md) και 1 MB στο [επίπεδο Premium](service-bus-premium-messaging.md). Την επικεφαλίδα, η οποία περιλαμβάνει τις ιδιότητες τυπικών και προσαρμοσμένων εφαρμογών, μπορεί να έχει ένα μέγιστο μέγεθος των 64 KB. Υπάρχει όριο στον αριθμό των μηνυμάτων που θα διατηρούνται σε ένα θέμα, αλλά υπάρχει μια καπέλο το συνολικό μέγεθος των μηνυμάτων που διαθέτει ένα θέμα. Σε αυτό το θέμα μέγεθος ορίζεται κατά τη δημιουργία, με ένα ανώτερο όριο των 5 GB.

## <a name="how-to-receive-messages-from-a-subscription"></a>Πώς μπορείτε να λαμβάνετε μηνύματα από μια συνδρομή

Για να λαμβάνετε μηνύματα από μια συνδρομή, χρησιμοποιήστε ένα αντικείμενο **ServiceBusContract** . Μηνύματα που λαμβάνετε να εργαστείτε σε δύο διαφορετικές καταστάσεις λειτουργίας: **ReceiveAndDelete** και **PeekLock**.

Όταν λαμβάνετε χρησιμοποιώντας τη λειτουργία **ReceiveAndDelete** , είναι μια λειτουργία μίας στιγμιότυπο - δηλαδή, όταν η υπηρεσία Bus λάβει ένα αποδεικτικό ανάγνωσης για ένα μήνυμα, το επισημαίνει το μήνυμα ως που καταναλώνεται και επιστρέφει στην εφαρμογή. Η λειτουργία **ReceiveAndDelete** είναι το μοντέλο απλούστερος και λειτουργεί καλύτερα για σενάρια στις οποίες μια εφαρμογή μπορεί να αποδεχτεί τις δεν επεξεργασία ενός μηνύματος σε περίπτωση αποτυχίας. Για να κατανοήσετε αυτήν, εξετάστε το ενδεχόμενο να ένα σενάριο που ζητήματα της αίτησης λήψης του καταναλωτή και, στη συνέχεια, παρουσιάζει σφάλμα πριν από την επεξεργασία του. Επειδή η υπηρεσία Bus θα έχουν σήμανση του μηνύματος ως που καταναλώνεται, στη συνέχεια, όταν η εφαρμογή επανεκκίνηση του και αρχίζει κατανάλωση ξανά τα μηνύματα, το θα έχετε παραβλέψει το μήνυμα που καταναλώθηκε πριν από το σφάλμα.

Στη λειτουργία **PeekLock** , λαμβάνετε γίνεται μια λειτουργία δύο στάδιο, γεγονός που επιτρέπει σε εφαρμογές υποστήριξης που δεν θέλετε να γίνει μηνύματα που λείπουν. Όταν Bus υπηρεσία λαμβάνει μια αίτηση, βρίσκει στο επόμενο μήνυμα να καταναλωθεί, κλειδώνει για να αποτρέψετε άλλους καταναλωτές πραγματοποιεί λήψη του και, στη συνέχεια, επιστρέφει το στην εφαρμογή. Μετά την εφαρμογή ολοκληρώσει την επεξεργασία του μηνύματος (ή αποθηκεύει αξιόπιστα για μελλοντική επεξεργασία), ολοκληρώνει δεύτερου σταδίου της διαδικασίας παραλαβής καλώντας την **Διαγραφή** του ληφθέντος μηνύματος. Κατά την υπηρεσία Bus βλέπει την κλήση **Διαγραφή** , θα επισημάνετε το μήνυμα ως που καταναλώνεται και καταργήστε την από το θέμα.

Το παρακάτω παράδειγμα παρουσιάζει πώς μπορούν να ληφθούν τα μηνύματα και να υποβάλλονται σε επεξεργασία με χρήση της κατάστασης λειτουργίας **PeekLock** (όχι η προεπιλεγμένη κατάσταση λειτουργίας). Το παρακάτω παράδειγμα, εκτελεί ένα βρόχο και επεξεργάζεται μηνύματα στην συνδρομής "HighMessages" και, στη συνέχεια, κλείνει όταν υπάρχουν περισσότερα μηνύματα (Εναλλακτικά, αυτό θα μπορούσε να έχει την τιμή να περιμένετε για νέα μηνύματα).

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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

Υπηρεσία Bus παρέχει λειτουργικότητα που θα σας βοηθήσουν να ανακτήσετε ομαλά από σφάλματα στο εφαρμογής ή δυσκολίες επεξεργασία ενός μηνύματος. Εάν μια εφαρμογή ακουστικό είναι δυνατή η επεξεργασία του μηνύματος για κάποιο λόγο, στη συνέχεια, το να καλέσετε τη μέθοδο **unlockMessage** του ληφθέντος μηνύματος (αντί για τη μέθοδο **deleteMessage** ). Αυτό θα προκαλέσει Bus υπηρεσίας για να ξεκλειδώσετε το μήνυμα μέσα στο θέμα και να την κάνετε διαθέσιμη θα ληφθεί ξανά, με την ίδια εφαρμογή καταναλώνει ή από άλλη εφαρμογή καταναλώνει.

Υπάρχει επίσης ένα χρονικό όριο που σχετίζεται με ένα μήνυμα κλειδωμένο μέσα στο θέμα και, εάν η εφαρμογή δεν μπορεί να επεξεργαστεί το μήνυμα πριν από την κλείδωμα λήξει το χρονικό όριο (για παράδειγμα, εάν η εφαρμογή παρουσιάζει σφάλμα), στη συνέχεια, υπηρεσία Bus θα ξεκλειδώσετε αυτόματα το μήνυμα και να την κάνετε διαθέσιμη θα ληφθεί ξανά.

Σε περίπτωση που η εφαρμογή παρουσιάζει σφάλμα μετά την επεξεργασία του μηνύματος, αλλά πριν από την έκδοση της αίτησης **deleteMessage** , στη συνέχεια, το μήνυμα θα είναι redelivered στην εφαρμογή κατά την επανεκκίνηση. Συχνά αυτό ονομάζεται **Τουλάχιστον αφού επεξεργασίας**, αυτό σημαίνει ότι θα υποβληθούν τουλάχιστον μία φορά σε κάθε μήνυμα αλλά σε ορισμένες περιπτώσεις ίσως redelivered το ίδιο μήνυμα. Εάν το σενάριο δεν θέλετε να γίνει διπλότυπων επεξεργασίας, στη συνέχεια, προγραμματιστές εφαρμογών πρέπει να Προσθήκη επιπλέον λογικής σε εφαρμογή τους χειρισμού διπλότυπων μήνυμα παράδοσης. Συχνά, αυτό είναι δυνατό με τη μέθοδο **getMessageId** του μηνύματος, η οποία θα παραμένει σταθερή κατά μήκος προσπαθειών παράδοσης.

## <a name="delete-topics-and-subscriptions"></a>Διαγράψτε τα θέματα και συνδρομών

Ο κύριος τρόπος για να διαγράψετε τα θέματα και συνδρομές είναι να χρησιμοποιήσετε ένα αντικείμενο **ServiceBusContract** . Διαγραφή ενός θέματος, θα διαγραφούν επίσης τις συνδρομές που έχουν καταχωρηθεί με το θέμα. Συνδρομές μπορεί επίσης να διαγραφεί ανεξάρτητα.

```
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του ουρές Bus υπηρεσίας, ανατρέξτε στο θέμα [υπηρεσία Bus ουρές, θέματα, και συνδρομές][] για περισσότερες πληροφορίες.

  [Azure SDK για Java]: http://azure.microsoft.com/develop/java/
  [Azure Κιτ εργαλείων για Έκλειψη]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Υπηρεσία Bus ουρές, θέματα και συνδρομών]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  
  [0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
