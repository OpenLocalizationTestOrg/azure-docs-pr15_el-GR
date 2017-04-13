<properties 
    pageTitle="Γράψτε εφαρμογές που χρησιμοποιούν ουρές Bus υπηρεσίας | Microsoft Azure"
    description="Πώς να συντάξετε μια απλή εφαρμογή που βασίζεται σε ουρά που χρησιμοποιεί Bus υπηρεσίας."
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-queues"></a>Δημιουργία εφαρμογών που χρησιμοποιούν ουρές Bus υπηρεσίας

Αυτό το θέμα περιγράφει ουρές Bus υπηρεσίας και σας δείχνει πώς να συντάξετε μια απλή εφαρμογή που βασίζεται σε ουρά που χρησιμοποιεί Bus υπηρεσίας.

Εξετάστε το ενδεχόμενο να ένα σενάριο από τον κόσμο των στην οποία πρέπει να δρομολογείται δεδομένα πωλήσεων από μεμονωμένους σταθμούς Point-of-Sale (ΘΈΣΗ) σε ένα σύστημα διαχείρισης αποθέματος που χρησιμοποιεί τα δεδομένα για να καθορίσετε πότε έχει μετοχών να αναπληρωθούν λιανικής πώλησης. Αυτή η λύση χρησιμοποιεί Bus υπηρεσία ανταλλαγής μηνυμάτων για την επικοινωνία μεταξύ τους τερματικούς σταθμούς και το σύστημα διαχείρισης αποθέματος, όπως φαίνεται στην παρακάτω εικόνα:

![Εικόνα ουρές Bus υπηρεσίας 1](./media/service-bus-create-queues/IC657161.gif)

Κάθε ΘΈΣΗ terminal αναφέρει τα δεδομένα πωλήσεων με την αποστολή μηνυμάτων για να το **DataCollectionQueue**. Αυτά τα μηνύματα παραμένουν σε αυτήν την ουρά μέχρι την ανάκτησή τους από το σύστημα διαχείρισης αποθέματος. Αυτό το μοτίβο συχνά είναι αποκαλείται *ασύγχρονης μηνυμάτων*, επειδή δεν έχει το terminal ΘΈΣΗ σε αναμονή για απάντηση από το σύστημα διαχείρισης αποθέματος για να συνεχίσετε την επεξεργασία.

## <a name="why-queuing"></a>Γιατί ουράς;

Πριν εξετάσουμε τον κώδικα που απαιτείται για να ορίσετε αυτήν την εφαρμογή, εξετάστε το ενδεχόμενο να μιλήσετε απευθείας τα πλεονεκτήματα της χρήσης μιας ουράς σε αυτό το σενάριο αντί του σταθμούς ΘΈΣΗ (σύγχρονη) στο σύστημα διαχείρισης αποθέματος.

### <a name="temporal-decoupling"></a>Αποσύνδεση χρονικό

Με την ανταλλαγή μηνυμάτων ασύγχρονης μοτίβο, παραγωγών και καταναλωτών δεν χρειάζεται να είναι σε σύνδεση την ίδια στιγμή. Η υποδομή ανταλλαγής μηνυμάτων αποθηκεύει αξιόπιστα μηνύματα μέχρι το καταναλώνει μέρος είναι έτοιμη για λήψη τους. Αυτό σημαίνει ότι τα στοιχεία της κατανεμημένης εφαρμογής μπορεί να αποσυνδεθεί, είτε προαιρετικά; Για παράδειγμα, για συντήρηση ή λόγω στοιχείο σφάλμα, χωρίς που επηρεάζουν ολόκληρο το σύστημα. Επιπλέον, η εφαρμογή καταναλώνει μόνο ίσως χρειαστεί να είναι σε σύνδεση κατά τη διάρκεια ορισμένων ώρες της ημέρας. Για παράδειγμα, σε αυτό το σενάριο λιανικής πώλησης, το σύστημα διαχείρισης αποθέματος μπορεί να αρκεί να συνδεθεί μετά το τέλος της την εργάσιμη ημέρα.

### <a name="load-leveling"></a>Φόρτωση ισοστάθμισης

Σε πολλές εφαρμογές φόρτωση του συστήματος ποικίλλει ανάλογα με τον καιρό, ότι ο χρόνος επεξεργασίας που απαιτούνται για κάθε μονάδα εργασίας είναι συνήθως σταθερές. Intermediating μήνυμα παραγωγών και καταναλωτών με μια ουρά σημαίνει ότι η εφαρμογή καταναλώνει (ο εργαζόμενος) έχει μόνο προμήθεια για να εξυπηρετήσει την average φόρτωση αντί για μια φόρτωση κορύφωσης. Το βάθος της ουράς θα αυξηθεί και σύμβαση καθώς το εισερχόμενες φορτίο μεταβάλλεται. Αυτό αποθηκεύει απευθείας χρήματα όσον αφορά το ποσό της υποδομής που απαιτείται για την εξυπηρέτηση η φόρτωση της εφαρμογής.

![Υπηρεσία Bus ουρές εικόνα 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Εξισορρόπηση φόρτου

Καθώς αυξάνεται η φόρτωση περισσότερων διαδικασιών εργασίας μπορούν να προστεθούν σε ανάγνωση από την ουρά εργασίας. Κάθε μήνυμα υποβάλλεται σε επεξεργασία από έναν μόνο από οι διαδικασίες εργασίας. Επιπλέον, αυτό εξισορρόπησης φόρτου βάσει ελκυστική επιτρέπει για βέλτιστη χρήση των υπολογιστών εργασίας ακόμα και αν οι υπολογιστές εργασίας διαφέρουν όσον αφορά την ισχύ επεξεργασίας, όπως αυτά θα ελκυστική μηνύματα με το δικό τους μέγιστο χρέωση. Αυτό το μοτίβο είναι συχνά ονομάζονται το μοτίβο ανταγωνιστικών καταναλωτών.

![Εικόνα ουρές Bus υπηρεσίας 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Ελεύθερη ζεύξης

Χρήση μηνυμάτων στο ενδιάμεσο μεταξύ παραγωγών μήνυμα και οι καταναλωτές παρέχει μια εγγενή ελεύθερη ζεύξης μεταξύ των στοιχείων. Επειδή παραγωγών και καταναλωτών δεν γνωρίζουν μεταξύ τους, μπορεί να αναβαθμιστεί πρόγραμμα-πελάτη χωρίς να χρειάζεται καμία επίδραση στην το πρόγραμμα παραγωγής των. Επιπλέον, η τοπολογία ανταλλαγής μηνυμάτων μπορούν να εξελίσσονται χωρίς να επηρεαστούν τα υπάρχοντα τελικά σημεία. Θα αναλύονται τα εξής πιο όταν, αναφερθούμε δημοσίευση/εγγραφή.

## <a name="show-me-the-code"></a>Εμφάνιση του κώδικα

Στην παρακάτω ενότητα δείχνει πώς μπορείτε να χρησιμοποιήσετε Bus υπηρεσίας για να δημιουργήσετε αυτήν την εφαρμογή.

### <a name="sign-up-for-an-azure-account"></a>Εγγραφείτε για ένα λογαριασμό Azure

Θα χρειαστείτε Azure λογαριασμού για να αρχίσετε να εργάζεστε με Bus υπηρεσίας. Εάν δεν έχετε ήδη ένα, μπορείτε να εγγραφείτε για έναν δωρεάν λογαριασμό [εδώ](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Δημιουργία ενός χώρου ονομάτων

Όταν έχετε μια συνδρομή, μπορείτε να [δημιουργήσετε έναν νέο χώρο ονομάτων](service-bus-create-namespace-portal.md). Κάθε χώρος ονομάτων λειτουργεί ως πεδίου κοντέινερ για ένα σύνολο οντοτήτων Bus υπηρεσίας. Δώστε ένα μοναδικό όνομα το νέο χώρο ονομάτων σε όλους τους λογαριασμούς Bus υπηρεσίας. 

### <a name="install-the-nuget-package"></a>Εγκατάσταση του πακέτου NuGet

Για να χρησιμοποιήσετε το χώρο ονομάτων Bus υπηρεσίας, μια εφαρμογή πρέπει να αναφέρεται συγκρότησης Bus υπηρεσίας, ειδικά Microsoft.ServiceBus.dll. Μπορείτε να βρείτε αυτό συγκρότησης ως μέρος του Microsoft Azure SDK και η λήψη είναι διαθέσιμη από τη [σελίδα λήψης Azure SDK](https://azure.microsoft.com/downloads/). Ωστόσο, το [πακέτο NuGet Bus υπηρεσίας](https://www.nuget.org/packages/WindowsAzure.ServiceBus) είναι ο ευκολότερος τρόπος για να λάβετε το API Bus υπηρεσίας και για να ρυθμίσετε την εφαρμογή σας με όλες τις εξαρτήσεις Bus υπηρεσίας.

### <a name="create-the-queue"></a>Δημιουργία της Ουράς

Λειτουργίες διαχείρισης για την υπηρεσία Bus μηνυμάτων οντοτήτων (ουρές και δημοσίευση/εγγραφή θέματα) εκτελούνται μέσω της κλάσης [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Υπηρεσία Bus χρησιμοποιεί ένα μοντέλο ασφάλειας που βασίζεται σε [Θέσει σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας)](service-bus-sas-overview.md) . Η κλάση [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) αντιπροσωπεύει μια υπηρεσία παροχής διακριτικού ασφαλείας με ενσωματωμένα εργοστασίου μεθόδους επιστρέφοντας ορισμένες υπηρεσίες παροχής γνωστές διακριτικού. Θα χρησιμοποιήσουμε μια μέθοδο [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) για τη διατήρηση τα διαπιστευτήρια συσχετισμών Ασφαλείας. Η παρουσία [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , στη συνέχεια, έχει συνταχθεί με τη βασική διεύθυνση της υπηρεσίας Bus χώρου ονομάτων και η υπηρεσία παροχής διακριτικών.

Η κλάση [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) παρέχει μεθόδους για να δημιουργήσετε, απαρίθμηση και διαγραφή μηνυμάτων οντοτήτων. Ο κώδικας που εμφανίζεται εδώ εμφανίζει πώς δημιουργείται και χρησιμοποιείται για τη δημιουργία ουρά **DataCollectionQueue** την παρουσία [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) .

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
 
TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Σημειώστε ότι δεν υπάρχουν τύποι της μεθόδου [CreateQueue](https://msdn.microsoft.com/library/azure/hh322663.aspx) με δυνατότητα ιδιότητες της ουράς για να απενεργοποιήσει. Για παράδειγμα, μπορείτε να ορίσετε την προεπιλεγμένη time to live (TTL) τιμή για τα μηνύματα που αποστέλλονται στην ουρά.

### <a name="send-messages-to-the-queue"></a>Αποστολή μηνυμάτων στην ουρά

Για λειτουργίες χρόνου εκτέλεσης στην υπηρεσία Bus πρόσωπα, Για παράδειγμα, αποστολή και λήψη μηνυμάτων, μια εφαρμογή πρέπει πρώτα να δημιουργήσετε ένα αντικείμενο [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) . Παρόμοια με την κλάση [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , την παρουσία [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) έχει δημιουργηθεί από τη βασική διεύθυνση της υπηρεσίας χώρου ονομάτων και η υπηρεσία παροχής διακριτικών.

```
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Αποστολή μηνυμάτων και λάβατε από την υπηρεσία Bus ουρές είναι παρουσίες της κλάσης [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Αυτήν την κλάση αποτελείται από ένα σύνολο τυπικές ιδιότητες (όπως [ετικέτα](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) και [το TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), λεξικού που χρησιμοποιείται για τη διατήρηση ιδιότητες της εφαρμογής και, ένας οργανισμός των δεδομένων αυθαίρετο εφαρμογής. Μια εφαρμογή του, να ορίσετε τον οργανισμό περνώντας σε οποιοδήποτε αντικείμενο σειριοποιηθεί (το παρακάτω παράδειγμα μεταβιβάζει σε ένα αντικείμενο **SalesData** που αντιπροσωπεύει τα δεδομένα πωλήσεων από το terminal ΘΈΣΗ), που θα χρησιμοποιήσετε το [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) για σειριοποίηση του αντικειμένου. Εναλλακτικά, μπορεί να παρέχεται ένα αντικείμενο [ροής](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) .

Ο ευκολότερος τρόπος για την αποστολή μηνυμάτων σε μια δεδομένη ουρά, στην περίπτωσή μας **DataCollectionQueue**, είναι να χρησιμοποιήσετε [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) για να δημιουργήσετε ένα αντικείμενο [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) απευθείας από την παρουσία [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) .

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Λήψη μηνυμάτων από την ουρά

Για να λαμβάνετε μηνύματα από την ουρά, μπορείτε να χρησιμοποιήσετε ένα αντικείμενο [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) το οποίο μπορείτε να δημιουργήσετε απευθείας από το [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) χρησιμοποιώντας [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx). Μήνυμα δέκτες να εργαστείτε σε δύο διαφορετικές καταστάσεις λειτουργίας: **ReceiveAndDelete** και **PeekLock**. Το [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) έχει οριστεί όταν δημιουργείται το ακουστικό του μηνύματος, ως παράμετρο στην κλήση [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) .


Όταν χρησιμοποιείτε τη λειτουργία **ReceiveAndDelete** , η λήψη είναι μια λειτουργία μίας στιγμιότυπο. Αυτό σημαίνει ότι, όταν Bus υπηρεσία λαμβάνει την αίτηση, επισημαίνει το μήνυμα ως που καταναλώνεται και επιστρέφει στην εφαρμογή. Η λειτουργία **ReceiveAndDelete** είναι το μοντέλο απλούστερος και λειτουργεί καλύτερα για σενάρια όπου η εφαρμογή μπορεί να να αποδεχτεί τις δεν επεξεργασία ενός μηνύματος, εάν προκύψει σφάλμα. Για να κατανοήσετε αυτήν, εξετάστε το ενδεχόμενο να ένα σενάριο που ζητήματα της αίτησης λήψης του καταναλωτή και, στη συνέχεια, παρουσιάζει σφάλμα πριν από την επεξεργασία του. Επειδή το Bus υπηρεσίας σήμανση του μηνύματος ως που καταναλώνεται, όταν η επανεκκίνηση της εφαρμογής και ξεκινά κατανάλωση ξανά τα μηνύματα, αυτό θα έχω παραβλέψει το μήνυμα που καταναλώθηκε πριν από το σφάλμα.

Στη λειτουργία **PeekLock** , η λήψη γίνεται μια λειτουργία δύο σταδίων, το οποίο καθιστά δυνατή την υποστήριξη των εφαρμογών που δεν θέλετε να γίνει που λείπουν μηνύματα. Όταν Bus υπηρεσία λαμβάνει την αίτηση, βρίσκει στο επόμενο μήνυμα να καταναλωθεί, κλειδώνει για να αποτρέψετε άλλους καταναλωτές πραγματοποιεί λήψη του και, στη συνέχεια, επιστρέφει το στην εφαρμογή. Μετά την εφαρμογή ολοκληρώσει την επεξεργασία του μηνύματος (ή αποθηκεύει αξιόπιστα για μελλοντική επεξεργασία), ολοκληρώνει δεύτερου σταδίου της διαδικασίας παραλαβής καλώντας την [Ολοκλήρωση](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) του ληφθέντος μηνύματος. Κατά την υπηρεσία Bus βλέπει [ολοκλήρωσης](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) κλήσεων, επισημαίνει το μήνυμα ως που καταναλώνεται.

Δύο άλλες αποτελέσματα είναι δυνατό. Πρώτα, εάν η εφαρμογή είναι δυνατό να επεξεργαστεί το μήνυμα για κάποιο λόγο, το να καλέσετε [εγκαταλείψετε](https://msdn.microsoft.com/library/azure/hh181837.aspx) του ληφθέντος μηνύματος (αντί για [ολοκλήρωσης](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). Αυτό προκαλεί Bus υπηρεσίας για να ξεκλειδώσετε το μήνυμα και να την κάνετε διαθέσιμη θα ληφθεί ξανά, με τον ίδιο καταναλωτή ή από άλλη ολοκλήρωση καταναλωτή. Δεύτερον, υπάρχει ένα χρονικό όριο που σχετίζεται με το κλείδωμα και εάν η εφαρμογή δεν μπορεί να επεξεργαστεί το μήνυμα πριν το κλείδωμα λήξη χρονικού ορίου (για παράδειγμα, εάν η εφαρμογή παρουσιάζει σφάλμα), στη συνέχεια, υπηρεσία Bus θα ξεκλειδώσετε το μήνυμα και να την κάνετε διαθέσιμη θα ληφθεί ξανά (ουσιαστικά εκτέλεση μιας λειτουργίας [εγκαταλείψετε](https://msdn.microsoft.com/library/azure/hh181837.aspx) από προεπιλογή).

Λάβετε υπόψη ότι εάν η εφαρμογή παρουσιάζει σφάλμα μετά την επεξεργάζεται το μήνυμα, αλλά πριν από την [Ολοκλήρωση](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) έχει εκδοθεί αίτηση, το μήνυμα θα είναι redelivered στην εφαρμογή κατά την επανεκκίνηση. Αυτό είναι αποκαλείται συχνά επεξεργασίας *Τουλάχιστον μία φορά* . Αυτό σημαίνει ότι θα υποβληθούν τουλάχιστον μία φορά σε κάθε μήνυμα αλλά σε ορισμένες περιπτώσεις ίσως redelivered το ίδιο μήνυμα. Εάν το σενάριο δεν θέλετε να γίνει διπλότυπων επεξεργασίας, στη συνέχεια, πρόσθετες λογικής απαιτείται στην εφαρμογή για να εντοπίσετε διπλότυπες τιμές. Έτσι μπορείτε να επιτύχετε με βάση την ιδιότητα [αναγνωριστικού μηνύματος](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) του μηνύματος. Η τιμή αυτής της ιδιότητας παραμένει σταθερή κατά μήκος προσπαθειών παράδοσης. Αυτό είναι αποκαλείται επεξεργασίας *Ακριβώς μία φορά* .

Ο κώδικας που εμφανίζεται εδώ λαμβάνει και επεξεργάζεται ενός μηνύματος με τη λειτουργία **PeekLock** , που είναι η προεπιλεγμένη, εάν δεν υπάρχει τιμή [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) παρέχεται ρητά.

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
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

### <a name="use-the-queue-client"></a>Χρησιμοποιήστε το πρόγραμμα-πελάτη ουρά

Τα παραδείγματα νωρίτερα σε αυτήν την ενότητα δημιουργούνται [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) και [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) αντικείμενα απευθείας από το [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) για αποστολή και λήψη μηνυμάτων από την ουρά, αντίστοιχα. Εναλλακτική προσέγγιση είναι να χρησιμοποιήσετε την κλάση [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) , ποια υποστηρίζει τόσο την αποστολή και λήψη λειτουργίες εκτός από τις πιο σύνθετες δυνατότητες, όπως οι περίοδοι λειτουργίας.

```
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);
            
BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία για ουρές, ανατρέξτε στο θέμα [Δημιουργία εφαρμογές που χρησιμοποιούν θέματα Bus υπηρεσίας και συνδρομές](service-bus-create-topics-subscriptions.md) για να συνεχίσετε αυτήν τη συζήτηση χρησιμοποιώντας τις δυνατότητες δημοσίευση/εγγραφή των θέματα Bus υπηρεσίας και τις συνδρομές.