<properties 
    pageTitle="Δημιουργία εφαρμογών που χρησιμοποιούν τα θέματα Bus υπηρεσίας και συνδρομές | Microsoft Azure"
    description="Εισαγωγή στο παράθυρο δημοσίευση-εγγραφή δυνατότητες που παρέχονται από υπηρεσία Bus θέματα και συνδρομών."
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Δημιουργία εφαρμογών που χρησιμοποιούν τα θέματα Bus υπηρεσίας και συνδρομών

Azure Bus υπηρεσία υποστηρίζει ένα σύνολο τεχνολογιών το ενδιάμεσο βασίζεται στο cloud, προσανατολισμένος σε μήνυμα συμπεριλαμβανομένων ουράς αξιόπιστη μηνυμάτων και διαρκή δημοσίευση/εγγραφή ανταλλαγής μηνυμάτων. Σε αυτό το άρθρο δημιουργεί τις πληροφορίες που παρέχονται στη [Δημιουργία εφαρμογές που χρησιμοποιούν ουρές Bus υπηρεσίας](service-bus-create-queues.md) και προσφέρει μια εισαγωγή σχετικά με τις δυνατότητες δημοσίευση/εγγραφή που παρέχεται από την υπηρεσία Bus θέματα.

## <a name="evolving-retail-scenario"></a>Σε εξέλιξη σενάριο λιανικής πώλησης

Σε αυτό το άρθρο συνεχίζει το σενάριο λιανικής πώλησης που χρησιμοποιούνται σε [Δημιουργία εφαρμογές που χρησιμοποιείτε ουρές Bus υπηρεσίας](service-bus-create-queues.md). Ανάκληση αυτών των δεδομένων πωλήσεων από μεμονωμένες σημείο της πώλησης (ΘΈΣΗ) σταθμούς πρέπει να δρομολογείται σε ένα σύστημα διαχείρισης αποθέματος που χρησιμοποιεί αυτά τα δεδομένα για να καθορίσετε πότε έχει μετοχών να αναπληρωθούν. Κάθε ΘΈΣΗ terminal αναφέρει τα δεδομένα πωλήσεων με την αποστολή μηνυμάτων στην ουρά **DataCollectionQueue** , όπου παραμένουν μέχρι λαμβάνονται από το σύστημα διαχείρισης αποθέματος, όπως φαίνεται εδώ:

![Υπηρεσία Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Να εξελίσσονται αυτό το σενάριο, μια νέα απαίτηση έχει προστεθεί στο σύστημα: ο κάτοχος store θέλει να είναι σε θέση να παρακολουθείτε πώς αποδίδει το χώρο αποθήκευσης σε πραγματικό χρόνο.

Για να αντιμετωπίσετε αυτήν την απαίτηση, το σύστημα πρέπει να "πατήστε" η ροή δεδομένων πωλήσεων. Θέλουμε κάθε μήνυμα που αποστέλλεται από τη ΘΈΣΗ σταθμούς να αποστέλλονται στο σύστημα διαχείρισης αποθέματος ως πριν, αλλά θέλουμε ένα άλλο αντίγραφο κάθε μηνύματος που χρησιμοποιούμε για να παρουσιάσετε την προβολή πίνακα εργαλείων στον κάτοχο του χώρου αποθήκευσης.

Σε οποιαδήποτε κατάσταση όπως αυτή, στις οποίες απαιτείται κάθε μήνυμα για να χρησιμοποιηθεί από πολλά μέρη, μπορείτε να χρησιμοποιήσετε την υπηρεσία Bus *θέματα*. Θέματα παρέχουν ένα μοτίβο δημοσίευση/εγγραφή με την οποία γίνεται διαθέσιμη σε μία ή περισσότερες εγγραφές που έχουν καταχωρηθεί στο θέμα κάθε δημοσιευμένο μήνυμα. Αντίθετα, με ουρές κάθε μήνυμα παραλήφθηκε από ένα μεμονωμένο καταναλωτή.

Τα μηνύματα αποστέλλονται σε ένα θέμα με τον ίδιο τρόπο που αποστέλλονται σε μια ουρά. Ωστόσο, τα μηνύματα δεν λαμβάνονται από το θέμα απευθείας. λαμβάνονται από συνδρομές. Μπορείτε να θεωρήσετε μια συνδρομή σε ένα θέμα ως εικονικού ουρά που λαμβάνει αντίγραφα των μηνυμάτων που αποστέλλονται σε αυτό το θέμα. Λήψη των μηνυμάτων από μια συνδρομή τον ίδιο τρόπο όπως λαμβάνονται από μια ουρά.

Ξεκινήστε το σενάριο λιανικής πώλησης, ουρά αντικαθίσταται από ένα θέμα και προστίθεται η συνδρομή, που μπορούν να χρησιμοποιήσετε το στοιχείο σύστημα διαχείρισης αποθέματος. Το σύστημα εμφανίζεται πλέον ως εξής:

![Υπηρεσία Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Δείτε τη ρύθμιση παραμέτρων εκτελεί τον ίδιο τρόπο στη σχεδίαση προηγούμενη βασίζεται σε ουρά. Αυτό σημαίνει ότι τα μηνύματα που αποστέλλονται στο θέμα δρομολογούνται τη συνδρομή **απόθεμα** , από την οποία το **Σύστημα διαχείρισης αποθέματος** καταναλώνει τους.

Για να υποστηρίζουν τον πίνακα εργαλείων διαχείρισης, δημιουργούμε μια δεύτερη συνδρομή σχετικά με το θέμα, όπως φαίνεται εδώ:

![Υπηρεσία Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Με αυτήν τη ρύθμιση, κάθε μήνυμα από τη ΘΈΣΗ σταθμούς γίνεται διαθέσιμη σε συνδρομές του **πίνακα εργαλείων** και το **απόθεμα** .

## <a name="show-me-the-code"></a>Εμφάνιση του κώδικα

Στο άρθρο [Δημιουργία εφαρμογές που χρησιμοποιούν ουρές Bus υπηρεσίας](service-bus-create-queues.md) περιγράφει πώς μπορείτε να εγγραφείτε για ένα λογαριασμό Azure και να δημιουργήσετε ένα χώρο ονομάτων υπηρεσίας. Για να χρησιμοποιήσετε ένα χώρο ονομάτων Bus υπηρεσίας, μια εφαρμογή πρέπει να αναφέρεται συγκρότησης Bus υπηρεσίας, ειδικά Microsoft.ServiceBus.dll. Ο ευκολότερος τρόπος για την αναφορά εξαρτήσεων υπηρεσίας Bus είναι να εγκαταστήσετε το Bus υπηρεσίας [Nuget πακέτου](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). Μπορείτε επίσης να βρείτε συγκρότησης ως μέρος του Azure SDK. Η λήψη είναι διαθέσιμη από τη [σελίδα λήψης Azure SDK](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Δημιουργήστε το θέμα και συνδρομών

Λειτουργίες διαχείρισης για την υπηρεσία Bus μηνυμάτων οντοτήτων (ουρές και δημοσίευση/εγγραφή θέματα) εκτελούνται μέσω της κλάσης [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Κατάλληλα διαπιστευτήρια απαιτούνται για να δημιουργήσετε μια παρουσία [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) για ένα συγκεκριμένο πεδίο ονομάτων. Υπηρεσία Bus χρησιμοποιεί ένα μοντέλο ασφάλειας που βασίζεται σε [Θέσει σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας)](service-bus-sas-overview.md) . Η κλάση [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) αντιπροσωπεύει μια υπηρεσία παροχής διακριτικού ασφαλείας με ενσωματωμένα εργοστασίου μεθόδους επιστρέφοντας ορισμένες υπηρεσίες παροχής γνωστές διακριτικού. Θα χρησιμοποιήσουμε μια μέθοδο [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) για τη διατήρηση τα διαπιστευτήρια συσχετισμών Ασφαλείας. Η παρουσία [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , στη συνέχεια, έχει συνταχθεί με τη βασική διεύθυνση της υπηρεσίας Bus χώρου ονομάτων και η υπηρεσία παροχής διακριτικών.

Η κλάση [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) παρέχει μεθόδους για να δημιουργήσετε, απαρίθμηση και διαγραφή μηνυμάτων οντοτήτων. Ο κώδικας που εμφανίζεται εδώ εμφανίζει πώς η παρουσία [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) δημιουργείται και χρησιμοποιείται για να δημιουργήσετε το θέμα **DataCollectionTopic** .

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
     
TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);
 
namespaceManager.CreateTopic("DataCollectionTopic");
```

Σημειώστε ότι δεν υπάρχουν τύποι τη μέθοδο [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) που σας επιτρέπουν να ορίσετε ιδιότητες του θέματος. Για παράδειγμα, μπορείτε να ορίσετε την προεπιλεγμένη time to live (TTL) τιμή για τα μηνύματα που αποστέλλονται στο θέμα. Στη συνέχεια, προσθέστε τις συνδρομές **απόθεμα** και **πίνακα εργαλείων** .

```
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Αποστολή μηνυμάτων στο θέμα

Για λειτουργίες χρόνου εκτέλεσης στην υπηρεσία Bus πρόσωπα, Για παράδειγμα, αποστολή και λήψη μηνυμάτων, μια εφαρμογή πρέπει πρώτα να δημιουργήσετε ένα αντικείμενο [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) . Παρόμοια με την κλάση [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , την παρουσία [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) έχει δημιουργηθεί από τη βασική διεύθυνση της υπηρεσίας χώρου ονομάτων και η υπηρεσία παροχής διακριτικών.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Αποστολή μηνυμάτων και λάβει από την υπηρεσία Bus θέματα, είναι παρουσίες της κλάσης [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Αυτήν την κλάση αποτελείται από ένα σύνολο τυπικές ιδιότητες (όπως [ετικέτα](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) και [το TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), λεξικού που χρησιμοποιείται για τη διατήρηση ιδιότητες της εφαρμογής και, ένας οργανισμός των δεδομένων αυθαίρετο εφαρμογής. Μια εφαρμογή του, να ορίσετε τον οργανισμό περνώντας σε οποιοδήποτε αντικείμενο σειριοποιηθεί (το παρακάτω παράδειγμα μεταβιβάζει σε ένα αντικείμενο **SalesData** που αντιπροσωπεύει τα δεδομένα πωλήσεων από το terminal ΘΈΣΗ), που θα χρησιμοποιήσετε το [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) για σειριοποίηση του αντικειμένου. Εναλλακτικά, μπορεί να παρέχεται ένα αντικείμενο [ροής](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) .

```
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Ο ευκολότερος τρόπος για την αποστολή μηνυμάτων στο θέμα είναι να χρησιμοποιήσετε [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) για να δημιουργήσετε ένα αντικείμενο [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) απευθείας από την παρουσία [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) .

```
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Λήψη μηνυμάτων από μια συνδρομή

Παρόμοια με τη χρήση ουρές, για να λάβετε μηνύματα από μια συνδρομή που μπορείτε να χρησιμοποιήσετε ένα αντικείμενο [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) το οποίο μπορείτε να δημιουργήσετε απευθείας από το [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) χρησιμοποιώντας [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx). Μπορείτε να χρησιμοποιήσετε μία από τις δύο διαφορετικές λαμβάνουν λειτουργίες (**ReceiveAndDelete** και **PeekLock**), όπως περιγράφεται στο θέμα [Δημιουργία εφαρμογές που χρησιμοποιούν ουρές Bus υπηρεσίας](service-bus-create-queues.md).

Λάβετε υπόψη ότι, όταν δημιουργείτε μια [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) για τις συνδρομές, η παράμετρος *entityPath* είναι μέρος της φόρμας `topicPath/subscriptions/subscriptionName`. Επομένως, για να δημιουργήσετε ένα [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) για τη συνδρομή **απογραφής** του θέματος **DataCollectionTopic** , *entityPath* πρέπει να οριστεί σε `DataCollectionTopic/subscriptions/Inventory`. Ο κώδικας εμφανίζεται ως εξής:

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

## <a name="subscription-filters"></a>Φίλτρα συνδρομή

Μέχρι στιγμής, σε αυτό το σενάριο όλα τα μηνύματα που αποστέλλονται στο θέμα γίνονται διαθέσιμες σε όλες τις συνδρομές καταχωρημένες. Η φράση κλειδιού εδώ "γίνεται διαθέσιμη." Κατά την υπηρεσία Bus συνδρομές δείτε όλα τα μηνύματα που αποστέλλονται στο θέμα, μπορείτε να αντιγράψετε μόνο ένα υποσύνολο των αυτά τα μηνύματα στην ουρά εικονικού συνδρομής. Αυτό πραγματοποιείται χρησιμοποιώντας συνδρομή *φίλτρα*. Όταν δημιουργείτε μια συνδρομή, μπορείτε να πληκτρολογήσετε μια παράσταση φίλτρου με τη μορφή κατηγόρημα στυλ SQL92 που λειτουργεί επάνω από τις ιδιότητες του μηνύματος, τις ιδιότητες του συστήματος (για παράδειγμα, η [ετικέτα](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)) και τις ιδιότητες της εφαρμογής, όπως **StoreName** στο προηγούμενο παράδειγμα.

Σε εξέλιξη το σενάριο να παρουσιάζουν αυτό το παράδειγμα, μια δεύτερη χώρου αποθήκευσης είναι να προστεθούν μας σενάριο λιανικής πώλησης. Δεδομένα πωλήσεων από όλους τους τερματικούς σταθμούς ΘΈΣΗ από δύο καταστήματα εξακολουθούν να έχουν να δρομολογούνται στο σύστημα διαχείρισης αποθέματος κεντρική, αλλά η Διαχείριση χώρου αποθήκευσης με χρήση του εργαλείου πίνακα εργαλείων ενδιαφέρεται μόνο τις επιδόσεις του αυτόν το χώρο αποθήκευσης. Μπορείτε να χρησιμοποιήσετε το φιλτράρισμα για να επιτύχετε το εξής τη συνδρομή. Σημειώστε ότι όταν η ΘΈΣΗ σταθμούς δημοσιεύσετε μηνύματα, τους ορίσετε την ιδιότητα εφαρμογής **StoreName** στο μήνυμα. Δεδομένο δύο χώρους αποθήκευσης, για παράδειγμα **Redmond** και **Σιάτλ**, τη ΘΈΣΗ σταθμούς στο το Redmond αποθηκεύουν σφραγίδα τους μηνύματα δεδομένα πωλήσεων με μια **StoreName** ίσο με **Redmond**, ενώ το σταθμούς ΘΈΣΗ αποθήκευσης Σιάτλ χρησιμοποιούν ένα **StoreName** ισούται με **Σιάτλ**. Η διαχείριση του χώρου αποθήκευσης του χώρου αποθήκευσης Redmond θέλει μόνο για να δείτε τα δεδομένα από τη ΘΈΣΗ σταθμούς. Το σύστημα εμφανίζεται ως εξής:

![Υπηρεσία Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Για να ρυθμίσετε τη δρομολόγηση σε αυτό, δημιουργείτε τη συνδρομή του **πίνακα εργαλείων** ως εξής:

```
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Με αυτό το φίλτρο συνδρομή, μόνο τα μηνύματα που έχουν την ιδιότητα **StoreName** έχει οριστεί στο **Ρέντμοντ** θα αντιγραφούν στην ουρά εικονικού για τη συνδρομή του **πίνακα εργαλείων** . Υπάρχει πολύ περισσότερες με το φιλτράρισμα συνδρομή, ωστόσο. Εφαρμογές μπορεί να έχει πολλούς κανόνες φίλτρου ανά συνδρομή εκτός από τη δυνατότητα να τροποποιήσετε τις ιδιότητες ενός μηνύματος ως περάσει εικονικού ουρά μια συνδρομή.

## <a name="summary"></a>Σύνοψη

Όλους τους λόγους για να χρησιμοποιήσετε την υπηρεσία ουράς περιγράφεται στη [Δημιουργία εφαρμογών που χρησιμοποιούν ουρές Bus υπηρεσίας](service-bus-create-queues.md) επίσης ισχύουν για θέματα, συγκεκριμένα:

- Αποσύνδεση χρονικό – μήνυμα παραγωγών και καταναλωτών δεν χρειάζεται να είναι σε σύνδεση την ίδια στιγμή.

- Φόρτωση ισοστάθμιση-κορυφών φόρτωσης είναι εξομάλυνση από στο θέμα Ενεργοποίηση καταναλώνει εφαρμογές προμήθεια για average φόρτωση αντί για φόρτωση κορύφωσης.

- Εξισορρόπηση φόρτου-παρόμοια με μια ουρά, μπορείτε να έχετε πολλούς ανταγωνιστικών καταναλωτές ακρόαση σε μια μεμονωμένη συνδρομή, σε κάθε μήνυμα παραδοθεί σε ένα μόνο από τους καταναλωτές, συνεπώς εξισορρόπηση φόρτου.

- Ελεύθερη ζεύξης – μπορείτε να εξελίσσονται στο δίκτυο ανταλλαγής μηνυμάτων χωρίς να επηρεαστούν τα υπάρχοντα τελικά σημεία; Για παράδειγμα, προσθέτοντας συνδρομές ή αλλάζοντας φίλτρα σε ένα θέμα για να επιτρέψετε για τους νέους καταναλωτές.

## <a name="next-steps"></a>Επόμενα βήματα

Για πληροφορίες σχετικά με τη χρήση ουρές στο σενάριο λιανικής πώλησης ΘΈΣΗ, ανατρέξτε στο θέμα [Δημιουργία εφαρμογών που χρησιμοποιούν ουρές Bus υπηρεσίας](service-bus-create-queues.md) .