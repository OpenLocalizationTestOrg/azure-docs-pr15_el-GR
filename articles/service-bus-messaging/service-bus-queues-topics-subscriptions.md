<properties 
    pageTitle="Υπηρεσία Bus ουρές, θέματα και συνδρομές | Microsoft Azure"
    description="Επισκόπηση της υπηρεσίας Bus μηνυμάτων οντοτήτων."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm" />

# <a name="service-bus-queues-topics-and-subscriptions"></a>Υπηρεσία Bus ουρές, θέματα και συνδρομών

Microsoft Azure Service Bus υποστηρίζει ένα σύνολο τεχνολογίες βασίζεται στο cloud, ενδιάμεσο προσανατολισμένα σε μήνυμα συμπεριλαμβανομένων ουράς αξιόπιστη μηνυμάτων και διαρκή δημοσίευση/εγγραφή ανταλλαγής μηνυμάτων. Αυτές οι δυνατότητες brokered μηνυμάτων μπορεί να θεωρηθεί ως οι αποσυνδεδεμένοι μηνυμάτων δυνατότητες που υποστήριξη δημοσίευσης εγγραφής, χρονικό αποσύνδεση και σεναρίων με χρήση του Bus υπηρεσία ανταλλαγής μηνυμάτων ύφασμα εξισορρόπησης φόρτου. Οι αποσυνδεδεμένοι επικοινωνίας έχει πολλά πλεονεκτήματα; Για παράδειγμα, προγράμματα-πελάτες και διακομιστές μπορεί να συνδεθείτε σύμφωνα με τις ανάγκες και να εκτελέσετε λειτουργίες τους με τον τρόπο ασύγχρονης.

Των μηνυμάτων οντοτήτων που σχηματίζουν τις βασικές τις brokered δυνατότητες ανταλλαγής μηνυμάτων στην υπηρεσία Bus είναι ουρές, θέματα/συνδρομές και κανόνες/ενέργειες.

## <a name="queues"></a>Ουρές

Ουρές προσφέρουν πρώτη στο, πρώτη ανάληψη FIFO παράδοση του μηνύματος σε μία ή περισσότερες ανταγωνιστικών καταναλωτές. Αυτό σημαίνει ότι τα μηνύματα συνήθως αναμένεται να λάβει και να υποβάλλονται σε επεξεργασία από το δέκτες με τη σειρά που προστέθηκαν στην ουρά και κάθε μήνυμα είναι λάβει και να υποβάλλονται σε επεξεργασία από ένα μόνο μήνυμα καταναλωτή. Ένα βασικό πλεονέκτημα της χρήσης ουρές είναι να επιτύχετε "χρονικό αποσύνδεση" εφαρμογή στοιχείων. Με άλλα λόγια, τους παραγωγούς (αποστολείς) και οι καταναλωτές (δέκτες) δεν χρειάζεται να είναι αποστολή και λήψη μηνυμάτων την ίδια στιγμή, επειδή τα μηνύματα αποθηκεύονται κατάταξή στην ουρά. Επιπλέον, το πρόγραμμα παραγωγής των δεν πρέπει να περιμένουν για μια απάντηση από τον καταναλωτή για να συνεχίσετε να επεξεργάζονται και αποστολή μηνυμάτων.

Μια σχετική πλεονέκτημα είναι "Φόρτωση ισοστάθμιση," η οποία σας επιτρέπει παραγωγών και καταναλωτών για αποστολή και λήψη μηνυμάτων διαφορετικά επιτόκια. Σε πολλές εφαρμογές, η φόρτωση συστήματος ποικίλλει διάρκεια του χρόνου; Ωστόσο, ο χρόνος επεξεργασίας που απαιτούνται για κάθε μονάδα εργασίας είναι συνήθως σταθερές. Intermediating μήνυμα παραγωγών και καταναλωτών με μια ουρά σημαίνει ότι η εφαρμογή καταναλώνει έχει μόνο προμήθεια να έχετε τη δυνατότητα για το χειρισμό average φόρτου αντί για φόρτωση κορύφωσης. Το βάθος της ουράς αναπτύσσεται και τις συμβάσεις όπως η φόρτωση εισερχόμενες ποικίλλει. Αυτό αποθηκεύει απευθείας χρήματα όσον αφορά το ποσό της υποδομής που απαιτείται για την εξυπηρέτηση η φόρτωση της εφαρμογής. Καθώς αυξάνεται η φόρτωση περισσότερων διαδικασιών εργασίας μπορούν να προστεθούν σε ανάγνωση από την ουρά. Κάθε μήνυμα υποβάλλεται σε επεξεργασία από έναν μόνο από οι διαδικασίες εργασίας. Επιπλέον, αυτό εξισορρόπησης φόρτου βάσει ελκυστική επιτρέπει για βέλτιστη χρήση των υπολογιστών εργασίας ακόμα και αν οι υπολογιστές εργασίας διαφέρουν όσον αφορά την ισχύ επεξεργασίας, όπως αυτά θα ελκυστική μηνύματα με το δικό τους μέγιστο χρέωση. Αυτό το μοτίβο είναι συχνά ονομάζονται το μοτίβο "κάθε καταναλωτή".

Χρήση ουρές για ενδιάμεσου μεταξύ παραγωγών μήνυμα και οι καταναλωτές παρέχει μια εγγενή ελεύθερη ζεύξης μεταξύ των στοιχείων. Επειδή παραγωγών και καταναλωτών δεν γνωρίζουν μεταξύ τους, μπορεί να αναβαθμιστεί πρόγραμμα-πελάτη χωρίς να χρειάζεται καμία επίδραση στην το πρόγραμμα παραγωγής των.

Δημιουργία μιας ουράς είναι μια διαδικασία πολλών βημάτων. Μπορείτε να εκτελέσετε λειτουργίες διαχείρισης για την υπηρεσία Bus μηνυμάτων οντοτήτων (ουρές και θέματα) μέσω την κλάση [Microsoft.ServiceBus.NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , η οποία έχει συνταχθεί με την παροχή της διεύθυνσης βάσης από το χώρο ονομάτων Bus υπηρεσία και τα διαπιστευτήρια του χρήστη. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) παρέχει μεθόδους για δημιουργία, απαρίθμηση και διαγραφή μηνυμάτων οντοτήτων. Μετά τη δημιουργία ενός αντικειμένου [Microsoft.ServiceBus.TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) από το όνομα συσχετισμών Ασφαλείας και κλειδί και ένα αντικείμενο διαχείρισης χώρος ονομάτων υπηρεσίας, μπορείτε να χρησιμοποιήσετε τη μέθοδο [Microsoft.ServiceBus.NamespaceManager.CreateQueue](https://msdn.microsoft.com/library/azure/hh293157.aspx) για να δημιουργήσετε ουρά. Για παράδειγμα:

```
// Create management credentials
TokenProvider credentials = TokenProvider. CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Μπορείτε να δημιουργήσετε, στη συνέχεια, ένα αντικείμενο ουράς και ενός εργοστασίου ανταλλαγής μηνυμάτων με την υπηρεσία URI Bus ως όρισμα. Για παράδειγμα:

```
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Στη συνέχεια, μπορείτε να στείλετε μηνύματα στην ουρά. Για παράδειγμα, εάν έχετε μια λίστα των μηνυμάτων όπου μεσολαβούν που ονομάζεται `MessageList`, ο κωδικός εμφανίζεται με την εξής:

```
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Μπορείτε να, στη συνέχεια, λαμβάνετε μηνύματα από την ουρά, ως εξής:

```
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Στη λειτουργία [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) , η λειτουργία λήψης είναι μίας-στιγμιότυπο; Αυτό σημαίνει ότι, όταν Bus υπηρεσία λαμβάνει την αίτηση, επισημαίνει το μήνυμα ως που καταναλώνεται και επιστρέφει στην εφαρμογή. Η λειτουργία [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) είναι το μοντέλο απλούστερος και λειτουργεί καλύτερα για σενάρια στο οποίο η εφαρμογή μπορεί να αποδεχτεί τις δεν επεξεργασία ενός μηνύματος σε περίπτωση αποτυχίας. Για να κατανοήσετε αυτήν, εξετάστε το ενδεχόμενο να ένα σενάριο που ζητήματα της αίτησης λήψης του καταναλωτή και, στη συνέχεια, παρουσιάζει σφάλμα πριν από την επεξεργασία του. Επειδή η υπηρεσία Bus επισημαίνει το μήνυμα ως που καταναλώνεται, όταν η εφαρμογή επανεκκίνηση του και αρχίζει κατανάλωση ξανά τα μηνύματα, αυτό θα έχω παραβλέψει το μήνυμα που καταναλώθηκε πριν από το σφάλμα.

Στη λειτουργία [PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) , η λειτουργία λήψης γίνεται δύο σταδίων, που επιτρέπει σε εφαρμογές υποστήριξης που δεν θέλετε να γίνει μηνύματα που λείπουν. Όταν Bus υπηρεσία λαμβάνει την αίτηση, βρίσκει στο επόμενο μήνυμα να καταναλωθεί, κλειδώνει για να αποτρέψετε άλλες τους καταναλωτές να πραγματοποιεί λήψη του και, στη συνέχεια, επιστρέφει το στην εφαρμογή. Μετά την εφαρμογή ολοκληρώσει την επεξεργασία του μηνύματος (ή αποθηκεύει αξιόπιστα για μελλοντική επεξεργασία), ολοκληρώνει δεύτερου σταδίου της διαδικασίας παραλαβής καλώντας την [Ολοκλήρωση](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) του ληφθέντος μηνύματος. Κατά την υπηρεσία Bus βλέπει [ολοκλήρωσης](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) κλήσεων, επισημαίνει το μήνυμα ως που καταναλώνεται.

Εάν η εφαρμογή είναι δυνατό να επεξεργαστεί το μήνυμα για κάποιο λόγο, το να καλέσετε τη μέθοδο [εγκαταλείψετε](https://msdn.microsoft.com/library/azure/hh181837.aspx) του ληφθέντος μηνύματος (αντί για [ολοκλήρωσης](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). Αυτή η δυνατότητα επιτρέπει Bus υπηρεσίας για να ξεκλειδώσετε το μήνυμα και να την κάνετε διαθέσιμη θα ληφθεί ξανά, με τον ίδιο καταναλωτή ή από άλλη ανταγωνιστικών καταναλωτή. Δεύτερον, υπάρχει ένα χρονικό όριο που σχετίζεται με το κλείδωμα και εάν η εφαρμογή δεν μπορεί να επεξεργαστεί το μήνυμα πριν το κλείδωμα λήξη χρονικού ορίου (για παράδειγμα, εάν η εφαρμογή παρουσιάζει σφάλμα), στη συνέχεια, υπηρεσία Bus ξεκλειδώνονται διαθέτει το μήνυμα και κάνει διαθέσιμο θα ληφθεί ξανά (ουσιαστικά εκτέλεση μιας λειτουργίας [εγκαταλείψετε](https://msdn.microsoft.com/library/azure/hh181837.aspx) από προεπιλογή).

Σημειώστε ότι σε περίπτωση που η εφαρμογή παρουσιάζει σφάλμα μετά την επεξεργασία του μηνύματος, αλλά πριν από την έκδοση της αίτησης [ολοκλήρωσης](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) , το μήνυμα είναι redelivered με την εφαρμογή κατά την επανεκκίνηση. Συχνά αυτό ονομάζεται *Τουλάχιστον μία φορά *επεξεργασία; Αυτό σημαίνει ότι κάθε μήνυμα γίνεται τουλάχιστον μία φορά. Ωστόσο, σε ορισμένες περιπτώσεις ίσως να redelivered το ίδιο μήνυμα. Εάν το σενάριο δεν θέλετε να γίνει διπλότυπων επεξεργασία, στη συνέχεια, πρόσθετες λογικής απαιτείται στην εφαρμογή για να εντοπίσετε διπλότυπες τιμές, η οποία μπορεί να είναι επίτευξη ανάλογα με την ιδιότητα **αναγνωριστικού μηνύματος** του μηνύματος, η οποία παραμένει σταθερή κατά μήκος προσπαθειών παράδοσης. Αυτό είναι γνωστό ως *Ακριβώς μία φορά* επεξεργασίας.

Για περισσότερες πληροφορίες και ένα παράδειγμα στην πράξη σχετικά με τη δημιουργία και αποστολή μηνυμάτων για να και από ουρές, ανατρέξτε στο θέμα η [υπηρεσία Bus όπου μεσολαβούν μηνυμάτων πρόγραμμα εκμάθησης .NET](service-bus-brokered-tutorial-dotnet.md).

## <a name="topics-and-subscriptions"></a>Θέματα και συνδρομών

Σε αντίθεση με το ουρές, όπου κάθε μήνυμα υποβάλλεται σε επεξεργασία από ένα μεμονωμένο καταναλωτή, *θέματα* και οι *συνδρομές* παρέχουν ένα-προς-πολλά φόρμας επικοινωνίας, σε ένα μοτίβο *για δημοσίευση/εγγραφή* . Χρήσιμη για κλιμάκωση σε πολύ μεγάλο αριθμό παραληπτών, κάθε δημοσιευμένο μήνυμα διατίθενται σε κάθε συνδρομή που έχουν καταχωρηθεί στο θέμα. Τα μηνύματα αποστέλλονται σε ένα θέμα και παραδίδονται σε μία ή περισσότερες σχετικές εγγραφές, ανάλογα με το φίλτρο κανόνες που μπορεί να οριστεί με βάση ανά συνδρομή. Οι συνδρομές να χρησιμοποιήσετε πρόσθετα φίλτρα για να περιορίσετε τα μηνύματα που θέλουν να λαμβάνετε. Τα μηνύματα αποστέλλονται σε ένα θέμα με τον ίδιο τρόπο αποστέλλονται σε μια ουρά, αλλά δεν λαμβάνονται μηνύματα από το θέμα απευθείας. Αντί για αυτό, λαμβάνονται από συνδρομές. Μια συνδρομή θέμα μοιάζει με μια εικονική ουρά που λαμβάνει αντίγραφα των μηνυμάτων που αποστέλλονται στο θέμα. Τα μηνύματα λαμβάνονται από μια συνδρομή τον ίδιο τρόπο με τον τρόπο που λαμβάνονται από μια ουρά.

Συγκριτικά, τη λειτουργικότητα-αποστολή μηνύματος από μια ουρά χάρτες απευθείας σε ένα θέμα και τη λειτουργικότητα λήψη μηνύματος που αντιστοιχεί σε μια συνδρομή. Μεταξύ άλλων, αυτό σημαίνει ότι συνδρομές υποστηρίζει τα ίδια μοτίβα που περιγράφεται παραπάνω σε αυτήν την ενότητα όσον αφορά τις ουρές: ανταγωνιστικών καταναλωτή, χρονικό αποσύνδεση, ισοστάθμιση φόρτωση και εξισορρόπηση φόρτου.

Δημιουργία ενός θέματος είναι παρόμοια με τη δημιουργία μιας ουράς, όπως φαίνεται στο παράδειγμα στην προηγούμενη ενότητα. Δημιουργία της υπηρεσίας URI και, στη συνέχεια, χρησιμοποιήστε την κλάση [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) για να δημιουργήσετε το πρόγραμμα-πελάτη χώρο ονομάτων. Στη συνέχεια, μπορείτε να δημιουργήσετε ένα θέμα χρησιμοποιώντας τη μέθοδο [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) . Για παράδειγμα:

```
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Στη συνέχεια, προσθέστε συνδρομές που θέλετε:

```
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Στη συνέχεια, μπορείτε να δημιουργήσετε ένα πρόγραμμα-πελάτη το θέμα. Για παράδειγμα:

```
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Χρησιμοποιώντας τον αποστολέα του μηνύματος, μπορείτε να στείλετε και να λαμβάνετε μηνύματα από και προς το θέμα, όπως φαίνεται στην προηγούμενη ενότητα. Για παράδειγμα:

```
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Παρόμοια με ουρές, μηνύματα λαμβάνονται από μια συνδρομή χρησιμοποιώντας ένα αντικείμενο [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) αντί για ένα αντικείμενο [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) . Δημιουργήστε το πρόγραμμα-πελάτη συνδρομή, μεταβίβαση το όνομα του θέματος, το όνομα της συνδρομής και (προαιρετικά) στη λειτουργία παραλαβής ως παραμέτρους. Για παράδειγμα, με τη συνδρομή **απογραφής** :

```
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Κανόνες και ενέργειες

Σε πολλά σενάρια, τα μηνύματα που έχουν συγκεκριμένα χαρακτηριστικά πρέπει να επεξεργαστούν με διαφορετικούς τρόπους. Για να ενεργοποιήσετε αυτήν, μπορείτε να ρυθμίσετε συνδρομές για να βρείτε μηνύματα που έχουν επιθυμητοί ιδιότητες και, στη συνέχεια, να εκτελέσετε ορισμένες τροποποιήσεις σε αυτές τις ιδιότητες. Κατά την υπηρεσία Bus συνδρομές δείτε όλα τα μηνύματα που αποστέλλονται στο θέμα, μπορείτε να αντιγράψετε μόνο ένα υποσύνολο αυτά τα μηνύματα στην ουρά εικονικού συνδρομής. Αυτή η ενέργεια πραγματοποιείται με τη συνδρομή φίλτρα. Τροποποιήσεις αυτές ονομάζονται *Ενέργειες φίλτρου*. Όταν δημιουργείται μια συνδρομή, μπορείτε να πληκτρολογήσετε μια παράσταση φίλτρου που λειτουργεί για τις ιδιότητες του μηνύματος, τόσο τις ιδιότητες συστήματος (για παράδειγμα, η **ετικέτα**) και τις ιδιότητες προσαρμοσμένη εφαρμογή (για παράδειγμα, **StoreName**.) Η παράσταση φίλτρου SQL είναι προαιρετικό σε αυτήν την περίπτωση; χωρίς μια παράσταση φίλτρου SQL, καμία ενέργεια φίλτρου που ορίζονται από το σε μια συνδρομή θα εκτελεστεί σε όλα τα μηνύματα για αυτήν τη συνδρομή.

Χρησιμοποιώντας το προηγούμενο παράδειγμα, για να φιλτράρει μηνύματα που προέρχονται μόνο από **Store1**, να δημιουργείτε τη συνδρομή του πίνακα εργαλείων ως εξής:

```
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Με αυτό το φίλτρο συνδρομή στη θέση, μόνο τα μηνύματα που έχουν το `StoreName` ιδιότητα οριστεί σε `Store1` αντιγράφονται στην ουρά εικονικού για το `Dashboard` συνδρομής.

Για περισσότερες πληροφορίες σχετικά με πιθανές φίλτρο τιμών, ανατρέξτε στην τεκμηρίωση για τις κλάσεις [SqlFilter](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx) και [SqlRuleAction](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlruleaction.aspx) . Δείτε επίσης το [όπου μεσολαβούν μηνυμάτων: σύνθετων φίλτρων](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) και δείγματα [Φίλτρα το θέμα](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) .

## <a name="next-steps"></a>Επόμενα βήματα

Δείτε τα ακόλουθα θέματα για περισσότερες πληροφορίες και παραδείγματα χρήσης υπηρεσίας Bus όπου μεσολαβούν οντοτήτων ανταλλαγής μηνυμάτων για προχωρημένους.

- [Επισκόπηση μηνυμάτων Bus υπηρεσίας](service-bus-messaging-overview.md)
- [Πρόγραμμα εκμάθησης όπου μεσολαβούν Bus υπηρεσία ανταλλαγής μηνυμάτων .NET](service-bus-brokered-tutorial-dotnet.md)
- [Πρόγραμμα εκμάθησης όπου μεσολαβούν Bus υπηρεσία ανταλλαγής μηνυμάτων ΥΠΌΛΟΙΠΟ](service-bus-brokered-tutorial-rest.md)
- [Δείγμα φίλτρα θέμα](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters)
- [Όπου μεσολαβούν μηνυμάτων: Δείγμα φίλτρων για προχωρημένους](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

