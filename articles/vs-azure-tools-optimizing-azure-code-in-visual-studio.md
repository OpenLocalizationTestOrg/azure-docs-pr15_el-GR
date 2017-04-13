<properties
   pageTitle="Βελτιστοποίηση του Azure κώδικα στο Visual Studio | Microsoft Azure"
   description="Μάθετε σχετικά με τον τρόπο Azure κωδικό βελτιστοποίηση εργαλεία στο Visual Studio βοηθήσει να τον κωδικό πιο ισχυρό και εργασίας με καλύτερη απόδοση."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="optimizing-your-azure-code"></a>Βελτιστοποίηση του Azure κώδικα

Όταν προγραμματισμού εφαρμογών που χρησιμοποιούν το Microsoft Azure, υπάρχουν ορισμένες πρακτικές κωδικοποίησης θα πρέπει να ακολουθήσετε για να αποφύγετε προβλήματα με την εφαρμογή κλιμάκωση, συμπεριφορά και τις επιδόσεις σε ένα περιβάλλον cloud. Η Microsoft παρέχει ένα εργαλείο ανάλυσης κώδικα Azure που αναγνωρίζει και προσδιορίζει πολλά από αυτά τα προβλήματα συχνά συναντήσατε και σας βοηθά να επιλύσετε. Μπορείτε να κάνετε λήψη του εργαλείου στο Visual Studio μέσω NuGet.

## <a name="azure-code-analysis-rules"></a>Azure κανόνων ανάλυσης κώδικα

Το εργαλείο ανάλυσης κώδικα Azure χρησιμοποιεί τους παρακάτω κανόνες για να επισημάνει αυτόματα τον κωδικό Azure όταν εντοπίζει γνωστά θέματα επηρεάζουν επιδόσεων. Εντοπίστηκε θέματα εμφανίζονται ως ένα προειδοποιήσεις ή σφάλματα προγράμματος μεταγλώττισης. Επιδιορθώσεις κώδικα ή προτάσεις για την επίλυση στην προειδοποίηση ή σφάλμα συχνά παρέχονται μέσω ένα εικονίδιο λάμπας.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Αποφύγετε τη χρήση προεπιλεγμένη κατάσταση κατάσταση λειτουργίας (σε εξέλιξη)

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP0000

### <a name="description"></a>Περιγραφή

Εάν χρησιμοποιείτε την προεπιλεγμένη κατάσταση λειτουργίας κατάσταση λειτουργίας (σε εξέλιξη) για τις εφαρμογές του cloud, μπορεί να χάσετε κατάσταση περιόδου λειτουργίας.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

Από προεπιλογή, η λειτουργία κατάσταση λειτουργίας που καθορίζεται στο αρχείο web.config είναι σε εξέλιξη. Επίσης, εάν δεν υπάρχει καταχώρηση που καθορίζεται στο αρχείο ρύθμισης παραμέτρων, τη λειτουργία κατάσταση περιόδου λειτουργίας από προεπιλογή σε εξέλιξη. Στη λειτουργία σε εξέλιξη αποθηκεύει κατάσταση περιόδου λειτουργίας στη μνήμη στο διακομιστή web. Όταν γίνει επανεκκίνηση του παρουσίας ή μιας νέας περιόδου λειτουργίας χρησιμοποιείται για την υποστήριξη ανακατεύθυνσης ή εξισορρόπησης φόρτου, δεν αποθηκεύεται η κατάσταση λειτουργίας αποθηκεύονται στη μνήμη στο διακομιστή web. Αυτή η κατάσταση αποτρέπει την εφαρμογή από το να είναι μεταβλητού μεγέθους στο cloud.

Κατάσταση περιόδου λειτουργίας ASP.NET υποστηρίζει αρκετές επιλογές διαφορετικό χώρο αποθήκευσης για τα δεδομένα κατάστασης περιόδου λειτουργίας: InProc, StateServer, της, προσαρμογή, και απενεργοποίηση. Συνιστάται να χρησιμοποιήσετε προσαρμοσμένη λειτουργία host δεδομένα σε ένα εξωτερικό χώρο αποθήκευσης κατάσταση περιόδου λειτουργίας, όπως η [υπηρεσία παροχής κατάσταση περιόδου λειτουργίας Azure για το Redis](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Λύση

Μία προτεινόμενη λύση είναι να αποθηκεύσετε κατάσταση περιόδου λειτουργίας σε μια υπηρεσία διαχειριζόμενων cache. Μάθετε πώς μπορείτε να χρησιμοποιήσετε [κατάσταση περιόδου λειτουργίας Azure παροχής για Redis](http://go.microsoft.com/fwlink/?LinkId=401521) για να αποθηκεύσετε την κατάσταση λειτουργίας. Μπορείτε επίσης να αποθηκεύσετε την κατάσταση της περιόδου λειτουργίας σε άλλες θέσεις για να βεβαιωθείτε ότι η εφαρμογή σας είναι μεταβλητού μεγέθους στο cloud. Για να μάθετε περισσότερα σχετικά με την εναλλακτική λύση λύσεις Διαβάστε [Λειτουργίες κατάσταση περιόδου λειτουργίας](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Εκτέλεση μεθόδου δεν πρέπει να ασύγχρονης

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP1000

### <a name="description"></a>Περιγραφή

Δημιουργία ασύγχρονης μεθόδους (όπως [αναμένει](https://msdn.microsoft.com/library/hh156528.aspx)) εκτός της μεθόδου [Εκτέλεση ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) και, στη συνέχεια, να καλέσετε τις μεθόδους ασύγχρονης από το [εκτεταμένο](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Δήλωση τη μέθοδο [[εκτεταμένο](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ως ασύγχρονης έχει ως αποτέλεσμα το ρόλο εργασίας για να εισαγάγετε ένα βρόχο επανεκκίνηση.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

Κλήση μεθόδους ασύγχρονης μέσα στη μέθοδο [εκτεταμένο](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) έχει ως αποτέλεσμα ο χρόνος εκτέλεσης υπηρεσίας cloud για να Κάδος Ανακύκλωσης το ρόλο εργασίας. Κατά την εκκίνηση του ρόλου εργαζόμενου, όλα εκτέλεση του προγράμματος τίθεται σε ισχύ μέσα στη μέθοδο [Εκτέλεση ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Έξοδος από τη μέθοδο [εκτεταμένο](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) έχει ως αποτέλεσμα το ρόλο εργασίας για να ξεκινήσετε ξανά. Όταν ο χρόνος εκτέλεσης ρόλο εργαζόμενου επισκέψεις την ασύγχρονη μέθοδο, αποστέλλει όλες οι λειτουργίες μετά την ασύγχρονη μέθοδο και, στη συνέχεια, επιστρέφει. Αυτό θα κάνει το ρόλο εργαζόμενου για έξοδο από τη μέθοδο [[[[εκτεταμένο](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) και επανεκκίνηση. Στην επόμενη διαδοχικών προσεγγίσεων της εκτέλεσης, το ρόλο εργαζόμενου επισκέψεις ξανά την ασύγχρονη μέθοδο και επανεκκίνηση του, προκαλεί Κάδος Ανακύκλωσης ξανά καθώς και το ρόλο εργασίας.

### <a name="solution"></a>Λύση

Τοποθετήστε όλες οι λειτουργίες ασύγχρονης εκτός της μεθόδου [Εκτέλεση ()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Στη συνέχεια, καλέστε τη μέθοδο refactored ασύγχρονης από μέσα τη μέθοδο [[εκτεταμένο](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) , όπως RunAsync .wait (). Το εργαλείο ανάλυσης κώδικα Azure μπορεί να σας βοηθήσει να διορθώσετε αυτό το πρόβλημα.

Το παρακάτω τμήμα κώδικα δείχνει την επιδιόρθωση κώδικα για αυτό το πρόβλημα:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Χρήση υπογραφής πρόσβασης σε κοινή χρήση Bus υπηρεσία ελέγχου ταυτότητας

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP2000

### <a name="description"></a>Περιγραφή

Χρήση κοινόχρηστων Access υπογραφή (συσχετισμών Ασφαλείας) για τον έλεγχο ταυτότητας. Υπηρεσία ελέγχου πρόσβασης (ACS) που χρησιμοποιείται για τον έλεγχο ταυτότητας bus υπηρεσίας.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

Βελτιωμένη ασφάλεια, Azure Active Directory αντικαθιστά ACS τον έλεγχο ταυτότητας με έλεγχο ταυτότητας συσχετισμών Ασφαλείας. Για πληροφορίες σχετικά με το πρόγραμμα μετάβασης, ανατρέξτε στο θέμα [Azure Active Directory είναι το μέλλον της ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) .

### <a name="solution"></a>Λύση

Χρήση ελέγχου ταυτότητας των συσχετισμών Ασφαλείας σε εφαρμογές σας. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε μια υπάρχουσα διακριτικό συσχετισμών Ασφαλείας για να αποκτήσετε πρόσβαση σε ένα χώρο ονομάτων bus υπηρεσίας ή οντότητα.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Ανατρέξτε στα παρακάτω θέματα για περισσότερες πληροφορίες.

- Για μια επισκόπηση, ανατρέξτε στο θέμα [Θέσει σε κοινή χρήση Access υπογραφή τον έλεγχο ταυτότητας με Bus υπηρεσίας](https://msdn.microsoft.com/library/dn170477.aspx)

- [Πώς μπορείτε να χρησιμοποιήσετε κοινόχρηστα Access υπογραφή τον έλεγχο ταυτότητας με Bus υπηρεσίας](https://msdn.microsoft.com/library/dn205161.aspx)

- Για ένα δείγμα έργου, ανατρέξτε στο θέμα [Χρήση κοινόχρηστων Access υπογραφή (συσχετισμών Ασφαλείας) τον έλεγχο ταυτότητας με συνδρομές Bus υπηρεσίας](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Μπορείτε να χρησιμοποιήσετε τη μέθοδο OnMessage για να αποφύγετε την "Λήψη βρόχος"

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP2002

### <a name="description"></a>Περιγραφή

Για να αποφύγετε μεταβαίνουν σε μια "Λήψη βρόχος", η κλήση της μεθόδου **OnMessage** είναι καλύτερη λύση για τη λήψη μηνυμάτων από την κλήση της μεθόδου **παραλαβής** . Ωστόσο, εάν πρέπει να χρησιμοποιήσετε τη μέθοδο **παραλαβής** και μπορείτε να καθορίσετε ένα διακομιστή μη προεπιλεγμένες χρόνος αναμονής, βεβαιωθείτε ότι ο χρόνος αναμονής server είναι περισσότερα από ένα λεπτό.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

Κατά την κλήση **OnMessage**, ο υπολογιστής-πελάτης ξεκινά μια αντλία εσωτερικού μηνύματος που σταθμοσκοπεί συνεχώς το ουρά ή τη συνδρομή. Σε αυτό το μήνυμα αντλία περιέχει ένα συνεχούς επανάληψης που εκτελεί μια κλήση για να λάβετε μηνύματα. Εάν η κλήση λήγει το χρονικό όριο, θέματα νέας κλήσης. Το χρονικό διάστημα καθορίζεται από την τιμή της ιδιότητας [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)που χρησιμοποιείται.

Το πλεονέκτημα της χρήσης **OnMessage** σε σύγκριση με **παραλαβής** είναι ότι οι χρήστες δεν χρειάζεται να με μη αυτόματο τρόπο τις ψηφοφορίας για τα μηνύματα, χειριστείτε εξαιρέσεις, επεξεργασία πολλών μηνυμάτων παράλληλα και ολοκληρώστε τα μηνύματα.

Αν καλέσετε **παραλαβής** χωρίς να χρησιμοποιήσετε την προεπιλεγμένη τιμή, βεβαιωθείτε ότι η τιμή *ServerWaitTime* είναι περισσότερα από ένα λεπτό. Ρύθμιση *ServerWaitTime* σε περισσότερα από ένα λεπτό εμποδίζει το διακομιστή έληξε το χρονικό όριο πριν από το μήνυμα λαμβάνεται πλήρως.

### <a name="solution"></a>Λύση

Ανατρέξτε στα παρακάτω παραδείγματα κώδικα για προτεινόμενες χρήσεις. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [QueueClient.OnMessage μέθοδο (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)και [QueueClient.Receive μέθοδο (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Για να βελτιώσετε την απόδοση της υποδομής του Azure ανταλλαγής μηνυμάτων, ανατρέξτε στο θέμα το μοτίβο σχεδίαση [Ασύγχρονης μηνυμάτων κύριο βήμα](https://msdn.microsoft.com/library/dn589781.aspx).

Ακολουθεί ένα παράδειγμα της χρήσης **OnMessage** να λαμβάνετε μηνύματα.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Ακολουθεί ένα παράδειγμα της χρήσης **παραλαβής** με την προεπιλεγμένη ώρα αναμονής διακομιστή.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Ακολουθεί ένα παράδειγμα της χρήσης **παραλαβής** με το χρόνο αναμονής μη προεπιλεγμένες διακομιστή.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Εξετάστε το ενδεχόμενο χρήσης ασύγχρονης μεθόδους Bus υπηρεσίας

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP2003

### <a name="description"></a>Περιγραφή

Χρησιμοποιήστε ασύγχρονης υπηρεσίας Bus μεθόδους για βελτίωση της απόδοσης με όπου μεσολαβούν μηνυμάτων.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

Χρήση ασύγχρονης μεθόδων σας δίνει τη δυνατότητα ταυτόχρονης εκτέλεσης προγράμματος εφαρμογής επειδή η εκτέλεση κάθε κλήση δεν αποκλεισμός κύριο νήμα. Κατά τη χρήση υπηρεσιών Bus μεθόδους για την ανταλλαγή μηνυμάτων, την εκτέλεση μιας λειτουργίας (αποστολή, λήψη, να διαγράψετε, κ.λπ.) διαρκεί. Αυτήν τη στιγμή περιλαμβάνει την επεξεργασία της λειτουργίας από την υπηρεσία Bus υπηρεσία εκτός από το λανθάνων χρόνος της αίτησης και η απάντηση. Για να αυξήσετε τον αριθμό των λειτουργιών ανά ώρα, λειτουργίες πρέπει να εκτελεί ταυτόχρονα. Για περισσότερες πληροφορίες, ανατρέξτε σε [Βέλτιστες πρακτικές για επιδόσεων βελτιώσεις χρησιμοποιώντας υπηρεσία Bus όπου μεσολαβούν μηνυμάτων](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Λύση

Για πληροφορίες σχετικά με το πώς μπορείτε να χρησιμοποιήσετε την προτεινόμενη μέθοδος ασύγχρονης, ανατρέξτε στο θέμα [QueueClient κλάσης (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) .

Για να βελτιώσετε την απόδοση της υποδομής του Azure ανταλλαγής μηνυμάτων, ανατρέξτε στο θέμα το μοτίβο σχεδίαση [Ασύγχρονης μηνυμάτων κύριο βήμα](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Εξετάστε το ενδεχόμενο δημιουργίας διαμερισμάτων ουρές Bus υπηρεσία και τα θέματα

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP2004

### <a name="description"></a>Περιγραφή

Διαμερίσματα υπηρεσίας Bus ουρές και θέματα για καλύτερη απόδοση με υπηρεσία Bus μηνυμάτων.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

Διαμερισμάτων ουρές Bus υπηρεσία και τα θέματα αυξάνεται επιδόσεων μετάδοσης και υπηρεσία διαθεσιμότητα επειδή η συνολική μεταγωγή διαμερίσματα ουρά ή το θέμα δεν είναι πλέον περιορίζεται από τις επιδόσεις του ένα μεμονωμένο μήνυμα μεσολαβητή ή ανταλλαγής μηνυμάτων χώρου αποθήκευσης. Επιπλέον, ένα προσωρινό μη διαθεσιμότητα του χώρου αποθήκευσης μηνυμάτων δεν κάνετε ένα διαμερίσματα ουρά ή το θέμα διαθέσιμες. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία διαμερισμάτων μηνυμάτων οντοτήτων](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Λύση

Το παρακάτω τμήμα κώδικα δείχνει τον τρόπο δημιουργίας διαμερισμάτων ανταλλαγής μηνυμάτων οντοτήτων.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [διαμερίσματα υπηρεσίας Bus ουρές και θέματα | Ιστολόγιο του Microsoft Azure](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) και ανατρέξτε στο άρθρο του [Microsoft Azure Bus διαμερίσματα απεσταλμένων](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) δείγματος.

## <a name="do-not-set-sharedaccessstarttime"></a>Δεν έχει οριστεί SharedAccessStartTime

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP3001

### <a name="description"></a>Περιγραφή

Θα πρέπει να αποφύγετε τη χρήση SharedAccessStartTimeset για την τρέχουσα ώρα για να ξεκινήσετε αμέσως την πολιτική πρόσβασης σε κοινή χρήση. Μόνο πρέπει να ορίσετε αυτήν την ιδιότητα, εάν θέλετε να ξεκινήσετε την πολιτική πρόσβασης σε κοινή χρήση αργότερα.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

Ρολόι έχει ως αποτέλεσμα μια μικρή χρονική διαφορά μεταξύ των κέντρα δεδομένων. Για παράδειγμα, θα λογικά πιστεύετε ότι η ρύθμιση της ώρας έναρξης ενός χώρου αποθήκευσης πολιτικής συσχετισμών Ασφαλείας ως την τρέχουσα ώρα χρησιμοποιώντας DateTime.Now ή μια παρόμοια μέθοδο θα προκαλέσει την πολιτική συσχετισμών Ασφαλείας για να τεθούν σε ισχύ αμέσως. Ωστόσο, το χρόνο μικρές διαφορές μεταξύ της κέντρα δεδομένων μπορούν να προκαλέσουν προβλήματα με αυτό επειδή ορισμένες φορές κέντρο δεδομένων μπορεί να είναι λίγο αργότερα από την ώρα έναρξης, ενώ άλλοι βρίσκεστε μπροστά από το. Ως αποτέλεσμα, μπορεί να λήξει η πολιτική συσχετισμών Ασφαλείας γρήγορα (ή ακόμη και αμέσως) Εάν η διάρκεια ζωής πολιτικής έχει οριστεί πολύ σύντομο.

Για περισσότερες οδηγίες σχετικά με τη χρήση κοινόχρηστων υπογραφή πρόσβαση σε Azure χώρου αποθήκευσης, ανατρέξτε στο θέμα [Εισαγωγή πίνακα συσχετισμών Ασφαλείας (θέσει σε κοινή χρήση Access υπογραφή), ουρά συσχετισμών Ασφαλείας και ενημέρωση συσχετισμών Ασφαλείας Blob - Ιστολόγιο ομάδας αποθήκευσης Microsoft Azure - κεντρική τοποθεσία - MSDN ιστολόγια](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Λύση

Καταργήστε τη δήλωση που ορίζει την ώρα έναρξης της πολιτικής κοινόχρηστη πρόσβαση. Το εργαλείο ανάλυσης κώδικα Azure παρέχει μια επιδιόρθωση για το συγκεκριμένο θέμα. Για περισσότερες πληροφορίες σχετικά με τη Διαχείριση ασφαλείας, ανατρέξτε στο θέμα το μοτίβο σχεδίαση [Valet κλειδί μοτίβο](https://msdn.microsoft.com/library/dn568102.aspx).

Το παρακάτω τμήμα κώδικα παρουσιάζει την επιδιόρθωση κώδικα για αυτό το ζήτημα.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Η ώρα λήξης πρέπει να είναι περισσότερο από πέντε λεπτά πολιτική πρόσβασης σε κοινή χρήση

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP3002

### <a name="description"></a>Περιγραφή

Μπορεί να υπάρχει όσο περισσότερα σε πέντε λεπτά διαφορά στο ρολογιού μεταξύ των κέντρα δεδομένων σε διαφορετικές θέσεις λόγω μια συνθήκη γνωστό ως "ώρα skew." Για να αποτρέψετε τις συσχετίσεις Ασφαλείας διακριτικού πολιτικής από λήξη ορίστε νωρίτερα από την προγραμματισμένη, το χρόνο λήξης να είναι περισσότερο από πέντε λεπτά.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

Κέντρα δεδομένων σε διαφορετικές θέσεις του κόσμου συγχρονίσετε με ένα σήμα χρονισμού. Επειδή ο χρόνος για το σήμα χρονισμού να μεταβαίνουν σε διαφορετικές θέσεις, μπορεί να υπάρχει μια απόκλιση χρόνου μεταξύ κέντρα δεδομένων σε διαφορετικές γεωγραφικές θέσεις Παρόλο που τα πάντα συγχρονίζεται δήθεν. Αυτή η διαφορά ώρας μπορεί να επηρεάσουν το πρόσβασης σε κοινή χρήση πολιτικής Έναρξη χρόνου και λήξης χρονικού διαστήματος. Επομένως, για να βεβαιωθείτε ότι η πολιτική πρόσβασης σε κοινή χρήση τίθεται σε ισχύ αμέσως, μην ορίσετε την ώρα έναρξης. Επιπλέον, βεβαιωθείτε ότι η ώρα λήξης έχει περισσότερους από 5 λεπτά για να αποτρέψετε την πρώιμη λήξη χρονικού ορίου.

Για περισσότερες πληροφορίες σχετικά με τη χρήση κοινόχρηστων υπογραφή πρόσβαση σε Azure χώρου αποθήκευσης, ανατρέξτε στο θέμα [Εισαγωγή πίνακα συσχετισμών Ασφαλείας (θέσει σε κοινή χρήση Access υπογραφή), ουρά συσχετισμών Ασφαλείας και ενημέρωση συσχετισμών Ασφαλείας Blob - Ιστολόγιο ομάδας αποθήκευσης Microsoft Azure - κεντρική τοποθεσία - MSDN ιστολόγια](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Λύση

Για περισσότερες πληροφορίες σχετικά με τη Διαχείριση ασφαλείας, ανατρέξτε στο θέμα το μοτίβο σχεδίαση [Valet κλειδί μοτίβο](https://msdn.microsoft.com/library/dn568102.aspx).

Ακολουθεί ένα παράδειγμα δεν εμφανίζει την ώρα έναρξης πολιτική πρόσβασης σε κοινή χρήση.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Ακολουθεί ένα παράδειγμα της ακρίβειας που καθορίζει μια ώρα έναρξης πολιτική πρόσβασης σε κοινή χρήση με μεγαλύτερες από πέντε λεπτά πολιτική λήξης διάρκεια.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία και χρήση υπογραφής πρόσβασης σε κοινή χρήση](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Χρήση CloudConfigurationManager

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP4000

### <a name="description"></a>Περιγραφή

Χρησιμοποιώντας την κλάση [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) για έργα όπως Azure τοποθεσιών Web και υπηρεσίες Azure κινητές συσκευές δεν θα Παρουσιάστε χρόνου εκτέλεσης θέματα. Ως βέλτιστη πρακτική, ωστόσο, είναι καλή ιδέα να χρησιμοποιήσετε Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) ενιαίου έτσι ώστε να Διαχείριση παραμέτρων για όλες τις εφαρμογές Azure Cloud.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

CloudConfigurationManager διαβάζει το αρχείο ρύθμισης παραμέτρων που είναι κατάλληλη για το περιβάλλον εφαρμογής.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Λύση

Refactor τον κωδικό για να χρησιμοποιήσετε την [Κλάση CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Επιδιόρθωση κώδικα για αυτό το ζήτημα παρέχεται από το εργαλείο ανάλυσης κώδικα Azure.

Το παρακάτω τμήμα κώδικα παρουσιάζει την επιδιόρθωση κώδικα για αυτό το ζήτημα. Αντικατάσταση

`var settings = ConfigurationManager.AppSettings["mySettings"];`

με το

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Ακολουθεί ένα παράδειγμα του τρόπου για να αποθηκεύσετε τη ρύθμιση παραμέτρων σε ένα αρχείο App.config ή Web.config. Προσθέστε τις ρυθμίσεις στην ενότητα appSettings από το αρχείο ρύθμισης παραμέτρων. Ακολουθεί το αρχείο Web.config για το προηγούμενο παράδειγμα κώδικα.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Αποφύγετε τη χρήση συμβολοσειρές σχεδιασμένου σύνδεσης

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP4001

### <a name="description"></a>Περιγραφή

Εάν χρησιμοποιείτε συμβολοσειρές σχεδιασμένου σύνδεσης και πρέπει να ενημερώσετε τους αργότερα, θα πρέπει να κάνετε αλλαγές στο δικό σας πηγαίο κώδικα και μεταγλωττίσετε ξανά την εφαρμογή. Ωστόσο, εάν αποθηκεύετε τις συμβολοσειρές σύνδεσης σε ένα αρχείο ρύθμισης παραμέτρων, μπορείτε να αλλάξετε τις νεότερες εκδόσεις, ενημερώνοντας απλώς το αρχείο ρύθμισης παραμέτρων.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

Σκληρό κωδικοποίησης συμβολοσειρές σύνδεσης είναι μια καλή πρακτική επειδή αυτό παρουσιάζει προβλήματα όταν συμβολοσειρές σύνδεσης πρέπει να αλλάξουν γρήγορα. Επιπλέον, εάν το έργο πρέπει να γίνει μεταβίβαση ελέγχου του στοιχείου ελέγχου προέλευσης, συμβολοσειρές σύνδεσης σχεδιασμένου Παρουσιάστε ευπάθειες ασφαλείας επειδή οι συμβολοσειρές μπορούν να προβληθούν στον πηγαίο κώδικα.

### <a name="solution"></a>Λύση

Αποθήκευση συμβολοσειρές σύνδεσης στη ρύθμιση παραμέτρων αρχείων ή Azure περιβάλλοντα.

- Για μεμονωμένη εφαρμογές, χρησιμοποιήστε app.config για να αποθηκεύσετε ρυθμίσεις συμβολοσειρά σύνδεσης.

- Για τις εφαρμογές web που φιλοξενείται στο των υπηρεσιών IIS, χρησιμοποιήστε web.config για να αποθηκεύσετε συμβολοσειρές σύνδεσης.

- Για εφαρμογές vNext ASP.NET, χρησιμοποιήστε configuration.json για την αποθήκευση συμβολοσειρών σύνδεσης.

Για πληροφορίες σχετικά με τη χρήση αρχεία ρυθμίσεις παραμέτρων όπως web.config ή app.config, ανατρέξτε στο θέμα [Οδηγίες ρύθμισης παραμέτρων Web ASP.NET](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx). Για πληροφορίες σχετικά με τον τρόπο Azure εργασίας μεταβλητές περιβάλλοντος, ανατρέξτε στο θέμα [Azure τοποθεσίες Web: πώς συμβολοσειρές εφαρμογή και να εργαστείτε συμβολοσειρές σύνδεσης](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Για πληροφορίες σχετικά με την αποθήκευση συμβολοσειρά σύνδεσης σε προέλευση στοιχείου ελέγχου, ανατρέξτε στο θέμα [αποφύγετε την τοποθέτηση ευαίσθητες πληροφορίες, όπως συμβολοσειρές σύνδεσης σε αρχεία που είναι αποθηκευμένα στο αρχείο φύλαξης κώδικα προέλευσης](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Χρησιμοποιήστε Διαγνωστικά αρχείο ρύθμισης παραμέτρων

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP5000

### <a name="description"></a>Περιγραφή

Αντί για τη ρύθμιση των παραμέτρων των ρυθμίσεων των διαγνωστικών του κώδικα όπως χρησιμοποιώντας το Microsoft.WindowsAzure.Diagnostics προγραμματισμού API, θα πρέπει να ρυθμίζετε τις παραμέτρους των ρυθμίσεων των διαγνωστικών του αρχείου diagnostics.wadcfg. (Ή, diagnostics.wadcfgx Εάν χρησιμοποιείτε το 2,5 SDK Azure). Με αυτόν τον τρόπο, μπορείτε να αλλάξετε ρυθμίσεων των διαγνωστικών του χωρίς να χρειάζεται να μεταγλωττίσετε ξανά τον κωδικό.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

Πριν από την 2,5 SDK Azure (το οποίο χρησιμοποιεί Azure Διαγνωστικά 1.3), Διαγνωστικά Azure (WAD) ήταν δυνατό να ρυθμιστεί, χρησιμοποιώντας διάφορες μεθόδους: Προσθήκη για τη ρύθμιση των παραμέτρων blob στο χώρο αποθήκευσης, με τη χρήση απαραίτητο κώδικα, δηλωτικό ρύθμισης παραμέτρων ή την προεπιλεγμένη ρύθμιση παραμέτρων. Ωστόσο, ο προτιμώμενη τρόπος για να ρυθμίσετε τις παραμέτρους Διαγνωστικά είναι να χρησιμοποιήσετε ένα αρχείο ρύθμισης παραμέτρων XML (diagnostics.wadcfg ή diagnositcs.wadcfgx για το 2,5 SDK και νεότερες εκδόσεις) στο έργο εφαρμογής. Σε αυτήν την προσέγγιση, το αρχείο diagnostics.wadcfg εντελώς ορίζει τις παραμέτρους και να ενημερώνεται και στο θα επαναληφθεί η ανάπτυξη. Ανάμιξη τη χρήση του αρχείου ρύθμισης παραμέτρων diagnostics.wadcfg με το μεθόδους προγραμματισμού από τη ρύθμιση παραμέτρων, χρησιμοποιώντας τις κατηγορίες [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)ή [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)μπορεί να οδηγήσει σε σύγχυση. Ανατρέξτε στο θέμα [προετοιμασία ή αλλαγή Azure Διαγνωστικά ρύθμισης παραμέτρων](https://msdn.microsoft.com/library/azure/hh411537.aspx) για περισσότερες πληροφορίες.

Ξεκινώντας με WAD 1.3 (περιλαμβάνεται 2,5 SDK Azure), δεν είναι πλέον δυνατή η χρήση κωδικού ρύθμισης παραμέτρων Διαγνωστικά. Ως αποτέλεσμα, μπορείτε να παρέχετε τις παραμέτρους κατά την εφαρμογή ή ενημέρωση την επέκταση Διαγνωστικά.

### <a name="solution"></a>Λύση

Χρησιμοποιήστε το εργαλείο σχεδίασης Διαγνωστικά ρύθμισης παραμέτρων για να μετακινήσετε διαγνωστικών ρυθμίσεων στο αρχείο παραμέτρων Διαγνωστικά (diagnositcs.wadcfg ή diagnositcs.wadcfgx για το 2,5 SDK και νεότερες εκδόσεις). Επίσης, συνιστάται να εγκαταστήσετε το [2,5 SDK Azure](http://go.microsoft.com/fwlink/?LinkId=513188) και να χρησιμοποιήσετε την πιο πρόσφατη δυνατότητα Διαγνωστικά.

1. Στο μενού συντόμευσης για το ρόλο που θέλετε να ρυθμίσετε τις παραμέτρους, επιλέξτε Ιδιότητες και, στη συνέχεια, επιλέξτε την καρτέλα ρύθμισης παραμέτρων.

1. Στην ενότητα **Διαγνωστικά** , βεβαιωθείτε ότι είναι επιλεγμένο το πλαίσιο ελέγχου **Ενεργοποίηση Διαγνωστικά** .

1. Επιλέξτε το κουμπί **Ρύθμιση παραμέτρων** .

  ![Πρόσβαση στην επιλογή Ενεργοποίηση Διαγνωστικά](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων διαγνωστικών για τις υπηρεσίες Cloud Azure και εικονικές μηχανές](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) .


## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Αποφύγετε τη δήλωση DbContext αντικείμενα ως στατικό

### <a name="id"></a>ΑΝΑΓΝΩΡΙΣΤΙΚΌ

AP6000

### <a name="description"></a>Περιγραφή

Για να αποθηκεύσετε μνήμης, αποφύγετε τη δήλωση DBContext αντικείμενα ως στατική.

Κάντε κοινή χρήση ιδεών και σχολίων στο [Azure κώδικα ανάλυσης σχολίων](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Αιτία

DBContext αντικείμενα, κρατήστε πατημένο τα αποτελέσματα του ερωτήματος από κάθε κλήση. Στατικά αντικείμενα DBContext δεν διατίθενται μέχρι τον τομέα εφαρμογή καταργείται. Επομένως, σε στατικό αντικείμενο DBContext να εκμετάλλευση μεγάλες ποσότητες μνήμης.

### <a name="solution"></a>Λύση

Δήλωση DBContext ως μια τοπική μεταβλητή ή πεδίο παρουσίας μη στατικό, το χρησιμοποιήσετε για μια εργασία και, στη συνέχεια, αφήστε να γίνει διατίθενται μετά τη χρήση.

Το παρακάτω παράδειγμα κλάσης ελεγκτή MVC δείχνει πώς μπορείτε να χρησιμοποιήσετε το αντικείμενο DBContext.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με το optimzing και αντιμετώπιση προβλημάτων Azure εφαρμογών, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων με μια εφαρμογή web στην υπηρεσία εφαρμογής Azure χρήση του Visual Studio](./app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
