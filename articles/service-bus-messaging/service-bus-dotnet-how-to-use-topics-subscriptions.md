<properties
    pageTitle="Χρήση υπηρεσίας Bus θέματα με το .NET | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Bus υπηρεσίας θέματα και οι συνδρομές με .NET στο Azure. Δείγματα κώδικα έχουν εγγραφεί για εφαρμογές .NET."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Πώς μπορείτε να χρησιμοποιήσετε θέματα Bus υπηρεσίας και συνδρομών

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Σε αυτό το άρθρο περιγράφει τον τρόπο χρήσης της υπηρεσίας Bus θέματα και συνδρομών. Τα δείγματα που είναι γραμμένες σε C# και χρησιμοποιήστε τα API .NET. Τα σενάρια καλύπτεται περιλαμβάνουν τη δημιουργία συνδρομών, τη δημιουργία συνδρομής φίλτρα, αποστολή μηνυμάτων σε ένα θέμα, λαμβάνετε μηνύματα από μια συνδρομή και διαγραφή συνδρομών και θέματα και θέματα. Για περισσότερες πληροφορίες σχετικά με τα θέματα και οι συνδρομές, ανατρέξτε στην ενότητα [επόμενα βήματα](#next-steps) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Ρύθμιση παραμέτρων της εφαρμογής για να χρησιμοποιήσετε Bus υπηρεσίας

Όταν δημιουργείτε μια εφαρμογή που χρησιμοποιεί Bus υπηρεσία, πρέπει να προσθέσετε μια αναφορά σε συγκρότησης Bus υπηρεσίας και περιλαμβάνουν τα αντίστοιχα πεδία ονομάτων. Ο ευκολότερος τρόπος για να το κάνετε αυτό είναι το κατάλληλο πακέτο [NuGet](https://www.nuget.org) λήψης.

## <a name="get-the-service-bus-nuget-package"></a>Λήψη του πακέτου NuGet Bus υπηρεσίας

Το [πακέτο NuGet Bus υπηρεσίας](https://www.nuget.org/packages/WindowsAzure.ServiceBus) είναι ο ευκολότερος τρόπος για να λάβετε το API Bus υπηρεσίας και να ρυθμίσετε την εφαρμογή σας με όλες τις απαραίτητες εξαρτήσεις Bus υπηρεσίας. Για να εγκαταστήσετε το πακέτο NuGet Bus υπηρεσίας στο έργο σας, κάντε τα εξής:

1.  Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ σε **αναφορές**και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.
2.  Αναζήτηση για "Υπηρεσία Bus" και επιλέξτε το στοιχείο **Microsoft Azure Service Bus** . Κάντε κλικ στην επιλογή **εγκατάσταση** για να ολοκληρώσετε την εγκατάσταση και, στη συνέχεια, κλείστε το παράθυρο διαλόγου παρακάτω:

    ![][7]

Τώρα είστε έτοιμοι να γράψετε κώδικα για Bus υπηρεσίας.

## <a name="create-a-service-bus-connection-string"></a>Δημιουργία συμβολοσειράς σύνδεσης Bus υπηρεσίας

Υπηρεσία Bus χρησιμοποιεί μια συμβολοσειρά σύνδεσης για να αποθηκεύσετε τα τελικά σημεία και τα διαπιστευτήρια. Μπορείτε να συμπεριλάβετε συμβολοσειρά σύνδεσης σε ένα αρχείο ρύθμισης παραμέτρων, αντί να σκληρό κωδικοποίηση αυτό:

- Όταν χρησιμοποιείτε τις υπηρεσίες Azure, συνιστάται να αποθηκεύσετε τη συμβολοσειρά σύνδεσης με χρήση του συστήματος ρύθμισης παραμέτρων Azure service (αρχεία .csdef και .cscfg).
- Όταν χρησιμοποιείτε το Azure τοποθεσίες Web ή εικονικές μηχανές Windows Azure, συνιστάται να αποθηκεύσετε τη συμβολοσειρά σύνδεσης με χρήση του .NET ρύθμισης παραμέτρων συστήματος (για παράδειγμα, το αρχείο Web.config).

Και στις δύο περιπτώσεις, μπορείτε να ανακτήσετε σας χρησιμοποιώντας συμβολοσειρά σύνδεσης του `CloudConfigurationManager.GetSetting` μέθοδο, όπως φαίνεται παρακάτω σε αυτό το άρθρο.

### <a name="configure-your-connection-string"></a>Ρύθμιση των παραμέτρων σας συμβολοσειρά σύνδεσης

Ο μηχανισμός ρύθμισης παραμέτρων υπηρεσίας σάς επιτρέπει να δυναμικά Αλλαγή ρυθμίσεων παραμέτρων από την [πύλη του Azure][] χωρίς επανάληψη ανάπτυξης την εφαρμογή σας. Για παράδειγμα, προσθέστε μια `Setting` ετικέτα με το αρχείο ορισμού (**.csdef**) υπηρεσία, όπως φαίνεται στο επόμενο παράδειγμα.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Στη συνέχεια, μπορείτε να καθορίσετε τιμές στο αρχείο ρύθμισης παραμέτρων (.cscfg) της υπηρεσίας.

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Χρησιμοποιήστε το όνομα κλειδιού σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας) και βασικές τιμές που έχουν ανακτηθεί από την πύλη, όπως περιγράφεται προηγουμένως.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Ρύθμιση των παραμέτρων σας συμβολοσειρά σύνδεσης κατά τη χρήση Azure τοποθεσίες Web ή εικονικές μηχανές Windows Azure

Όταν χρησιμοποιείτε τις τοποθεσίες Web ή εικονικές μηχανές, συνιστάται να χρησιμοποιήσετε το σύστημα παραμέτρων .NET (για παράδειγμα, Web.config). Μπορείτε να αποθηκεύσετε τη συμβολοσειρά σύνδεσης χρησιμοποιώντας την `<appSettings>` στοιχείο.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Χρησιμοποιήστε το όνομα συσχετισμών Ασφαλείας και τιμές κλειδιού που θα ανακτηθούν από την [πύλη του Azure][], όπως περιγράφεται προηγουμένως.

## <a name="create-a-topic"></a>Δημιουργία θέματος

Μπορείτε να εκτελέσετε λειτουργίες διαχείρισης για την υπηρεσία Bus θέματα και συνδρομές χρησιμοποιώντας την κλάση [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Αυτή η κλάση παρέχει μεθόδους για να δημιουργήσετε, απαρίθμηση και διαγραφή θέματα.

Το παρακάτω παράδειγμα δημιουργεί μια `NamespaceManager` αντικείμενο χρησιμοποιώντας το Azure `CloudConfigurationManager` τάξης με μια συμβολοσειρά σύνδεσης που αποτελείται από τη βασική διεύθυνση της υπηρεσίας Bus χώρο ονομάτων και τις κατάλληλες συσχετίσεις Ασφαλείας διαπιστευτήρια με δικαιώματα για τη διαχείριση του. Αυτή η συμβολοσειρά σύνδεσης έχει την εξής μορφή:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Χρησιμοποιήστε το παρακάτω παράδειγμα, δίνεται τις ρυθμίσεις παραμέτρων στην προηγούμενη ενότητα.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Υπάρχουν τύποι τη μέθοδο [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) που σας επιτρέπουν να ορίσετε ιδιότητες του θέματος; Για παράδειγμα, για να ορίσετε την προεπιλεγμένη τιμή time to live (TTL) για να εφαρμοστεί σε μηνύματα που αποστέλλονται στο θέμα. Αυτές οι ρυθμίσεις εφαρμόζονται χρησιμοποιώντας την κλάση [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) . Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε ένα θέμα με το όνομα **TestTopic** με μέγιστο μέγεθος του 5 GB και ένα προεπιλεγμένο μήνυμα TTL 1 λεπτού.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε τη μέθοδο [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) σε αντικείμενα [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) για να ελέγξετε εάν υπάρχει ήδη ένα θέμα με ένα καθορισμένο όνομα μέσα σε ένα χώρο ονομάτων.

## <a name="create-a-subscription"></a>Δημιουργία εγγραφής

Μπορείτε επίσης να δημιουργήσετε το θέμα συνδρομές χρησιμοποιώντας την κλάση [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Συνδρομές ονομάζονται και μπορούν να έχουν ένα προαιρετικό φίλτρο που περιορίζει το σύνολο των μηνυμάτων που του μεταβιβάστηκε εικονικού ουρά τη συνδρομή.

> [AZURE.IMPORTANT] Με τη σειρά για τα μηνύματα που θα ληφθεί από μια συνδρομή, πρέπει να δημιουργήσετε αυτήν τη συνδρομή πριν από την αποστολή των μηνυμάτων στο θέμα. Εάν υπάρχουν χωρίς συνδρομές σε ένα θέμα, το θέμα απορρίπτει αυτά τα μηνύματα.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Δημιουργήστε μια συνδρομή με το φίλτρο προεπιλεγμένη (MatchAll)

Εάν υπάρχει φίλτρο έχει καθοριστεί όταν δημιουργείται μια νέα συνδρομή, το φίλτρο **MatchAll** είναι το προεπιλεγμένο φίλτρο που χρησιμοποιείται. Όταν χρησιμοποιείτε το φίλτρο **MatchAll** , όλα τα μηνύματα που δημοσιεύονται στο θέμα τοποθετούνται στην ουρά εικονικού τη συνδρομή. Το παρακάτω παράδειγμα δημιουργεί μια εγγραφή με όνομα "AllMessages" και χρησιμοποιεί το προεπιλεγμένο φίλτρο **MatchAll** .

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Δημιουργία συνδρομές με τα φίλτρα

Μπορείτε επίσης να ορίσετε φίλτρα που σας επιτρέπουν να καθορίσετε ποια μηνύματα που αποστέλλονται σε ένα θέμα, θα πρέπει να εμφανίζονται μέσα σε μια συνδρομή στο συγκεκριμένο θέμα.

Η πιο ευέλικτη τύπο φίλτρου που υποστηρίζονται από συνδρομές είναι την κλάση [SqlFilter][] , η οποία υλοποιεί ένα υποσύνολο SQL92. Φίλτρα SQL λειτουργούν σχετικά με τις ιδιότητες των μηνυμάτων που δημοσιεύονται στο θέμα. Για περισσότερες πληροφορίες σχετικά με τις παραστάσεις που μπορούν να χρησιμοποιηθούν με φίλτρο SQL, δείτε τη σύνταξη [SqlFilter.SqlExpression][] .

Το παρακάτω παράδειγμα δημιουργεί μια συνδρομή με το όνομα **HighMessages** σε ένα αντικείμενο [SqlFilter][] που επιλέγει μόνο τα μηνύματα που έχουν μια προσαρμοσμένη ιδιότητα **αριθμού μηνύματος** που είναι μεγαλύτερη από 3.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Ομοίως, το ακόλουθο παράδειγμα δημιουργεί μια εγγραφή με όνομα **LowMessages** με [SqlFilter][] που επιλέγει μόνο τα μηνύματα που έχουν μια **αριθμού μηνύματος** ιδιότητα μικρότερη ή ίση με 3.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Τώρα όταν ένα μήνυμα αποστέλλεται `TestTopic`, πάντα παραδοθεί σε δέκτες μια συνδρομή με τη συνδρομή θέμα **AllMessages** και επιλεκτική παραδίδονται δέκτες μια συνδρομή για τις συνδρομές θέμα **HighMessages** και **LowMessages** (ανάλογα με το περιεχόμενο του μηνύματος).

## <a name="send-messages-to-a-topic"></a>Αποστολή μηνυμάτων σε ένα θέμα

Για να στείλετε ένα μήνυμα σε ένα θέμα Bus υπηρεσίας, η εφαρμογή σας δημιουργεί ένα αντικείμενο [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) χρησιμοποιώντας τη συμβολοσειρά σύνδεσης.

Ο ακόλουθος κώδικας περιγράφει πώς μπορείτε να δημιουργήσετε ένα αντικείμενο [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) για το θέμα **TestTopic** που δημιουργήθηκε με προηγούμενη έκδοση του [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) κλήση API.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Τα μηνύματα που αποστέλλονται σε θέματα υπηρεσίας Bus είναι παρουσίες της κλάσης [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Τα αντικείμενα [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) έχουν ένα σύνολο τυπικές ιδιότητες (όπως [ετικέτα](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) και [το TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), ένα λεξικό που χρησιμοποιείται για τη διατήρηση προσαρμοσμένες ιδιότητες συγκεκριμένη εφαρμογή, και ένα κύριο σώμα των δεδομένων αυθαίρετο εφαρμογής. Μια εφαρμογή του, να ορίσετε το σώμα του μηνύματος, μεταφέροντας οποιοδήποτε αντικείμενο σειριοποιηθεί στην κατασκευή του αντικειμένου [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) και το κατάλληλο **DataContractSerializer** χρησιμοποιείται για να σειριοποίηση του αντικειμένου. Εναλλακτικά, μπορεί να παρέχεται μια **System.IO.Stream** .

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να στείλετε μηνύματα πέντε δοκιμής στο αντικείμενο [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) **TestTopic** που λαμβάνονται στο προηγούμενο παράδειγμα κώδικα. Σημειώστε ότι η τιμή της ιδιότητας [αριθμού μηνύματος](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) για κάθε μήνυμα ποικίλλει ανάλογα με την επανάληψη του βρόχου (αυτή καθορίζει ποιες συνδρομές λαμβάνετε αυτό).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Θέματα υπηρεσίας Bus υποστηρίζει ένα μέγιστο μέγεθος μηνύματος 256 KB η [τυπική σειρά](service-bus-premium-messaging.md) και 1 MB στο [επίπεδο Premium](service-bus-premium-messaging.md). Την επικεφαλίδα, η οποία περιλαμβάνει τις ιδιότητες τυπικών και προσαρμοσμένων εφαρμογών, μπορεί να έχει ένα μέγιστο μέγεθος των 64 KB. Υπάρχει όριο στον αριθμό των μηνυμάτων που θα διατηρούνται σε ένα θέμα, αλλά υπάρχει μια καπέλο το συνολικό μέγεθος των μηνυμάτων που διαθέτει ένα θέμα. Σε αυτό το θέμα μέγεθος ορίζεται κατά τη δημιουργία, με ένα ανώτερο όριο των 5 GB. Αν είναι ενεργοποιημένη η δημιουργία διαμερισμάτων, το ανώτατο όριο είναι μεγαλύτερη. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Partitioned μηνυμάτων οντοτήτων](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Πώς μπορείτε να λαμβάνετε μηνύματα από μια συνδρομή

Το συνιστώμενο τρόπο να λαμβάνετε μηνύματα από μια συνδρομή είναι να χρησιμοποιήσετε ένα αντικείμενο [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) . Αντικείμενα [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) να εργαστείτε σε δύο διαφορετικές καταστάσεις λειτουργίας: [ *ReceiveAndDelete* και *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Όταν λαμβάνετε χρησιμοποιώντας τη λειτουργία **ReceiveAndDelete** , είναι μια λειτουργία μίας στιγμιότυπο. Αυτό σημαίνει ότι όταν Bus υπηρεσία λαμβάνει ένα αποδεικτικό ανάγνωσης για ένα μήνυμα σε μια συνδρομή, το επισημαίνει το μήνυμα ως που καταναλώνεται και επιστρέφει στην εφαρμογή. Η λειτουργία **ReceiveAndDelete** είναι το μοντέλο απλούστερος και λειτουργεί καλύτερα για σενάρια στις οποίες μια εφαρμογή μπορεί να αποδεχτεί τις δεν επεξεργασία ενός μηνύματος σε περίπτωση αποτυχίας. Για να κατανοήσετε αυτήν, εξετάστε το ενδεχόμενο να ένα σενάριο που ζητήματα της αίτησης λήψης του καταναλωτή και, στη συνέχεια, παρουσιάζει σφάλμα πριν από την επεξεργασία του. Επειδή η υπηρεσία Bus επισήμανε το μήνυμα ως που καταναλώθηκε, όταν η εφαρμογή επανεκκίνηση του και αρχίζει κατανάλωση ξανά τα μηνύματα, αυτό θα έχω παραβλέψει το μήνυμα που καταναλώθηκε πριν από το σφάλμα.

Στη λειτουργία **PeekLock** (που είναι η προεπιλεγμένη κατάσταση λειτουργίας), η διαδικασία παραλαβής γίνεται μια λειτουργία δύο σταδίων, η οποία δίνει τη δυνατότητα σε εφαρμογές υποστήριξης που δεν θέλετε να γίνει μηνύματα που λείπουν. Όταν Bus υπηρεσία λαμβάνει μια αίτηση, βρίσκει στο επόμενο μήνυμα να καταναλωθεί, κλειδώνει για να αποτρέψετε άλλους καταναλωτές πραγματοποιεί λήψη του και, στη συνέχεια, επιστρέφει το στην εφαρμογή. Μετά την εφαρμογή ολοκληρώσει την επεξεργασία του μηνύματος (ή αποθηκεύει αξιόπιστα για μελλοντική επεξεργασία), ολοκληρώνει δεύτερου σταδίου της διαδικασίας παραλαβής καλώντας την [Ολοκλήρωση](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) του ληφθέντος μηνύματος. Όταν η υπηρεσία Bus βλέπει **ολοκλήρωσης** κλήσεων, επισημαίνει το μήνυμα ως που καταναλώνεται και καταργεί από τη συνδρομή.

Το παρακάτω παράδειγμα παρουσιάζει πώς μπορούν να ληφθούν τα μηνύματα και να υποβάλλονται σε επεξεργασία με την προεπιλεγμένη κατάσταση λειτουργίας **PeekLock** . Για να καθορίσετε μια διαφορετική τιμή [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) , μπορείτε να χρησιμοποιήσετε μια άλλη υπερφόρτωσης για [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx). Αυτό το παράδειγμα χρησιμοποιεί την επιστροφή κλήσης [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) για να επεξεργαστείτε μηνύματα κατά την άφιξή σε τη συνδρομή **HighMessages** .

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

Αυτό το παράδειγμα ρυθμίζει τις παραμέτρους της επιστροφής κλήσης [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) χρησιμοποιώντας ένα αντικείμενο [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) . [Αυτόματη καταχώρηση](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) έχει οριστεί στην **τιμή false** για να ενεργοποιήσετε μη αυτόματη ρύθμιση πότε πρέπει να καλέσετε [ολοκλήρωσης](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) του ληφθέντος μηνύματος. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) έχει οριστεί σε 1 λεπτό, κάτι που έχει ως αποτέλεσμα το πρόγραμμα-πελάτη να περιμένετε έως και ένα λεπτό πριν από τον τερματισμό της δυνατότητας αυτόματης ανανέωσης και ο υπολογιστής-πελάτης κάνει νέας κλήσης για να ελέγξετε για μηνύματα. Αυτή η τιμή ιδιότητας μειώνει του αριθμού των φορών που ο υπολογιστής-πελάτης πραγματοποιεί χρεώνονται κλήσεις που δεν ανάκτηση μηνυμάτων.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Πώς να χειριστείτε σφάλματα εφαρμογών και αναγνώσιμα μηνύματα

Υπηρεσία Bus παρέχει λειτουργικότητα που θα σας βοηθήσουν να ανακτήσετε ομαλά από σφάλματα στο εφαρμογής ή δυσκολίες επεξεργασία ενός μηνύματος. Εάν μια εφαρμογή λήψης είναι δυνατή η επεξεργασία του μηνύματος για κάποιο λόγο, στη συνέχεια, το να καλέσετε τη μέθοδο [εγκαταλείψετε](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) του ληφθέντος μηνύματος (αντί για τη μέθοδο [ολοκλήρωσης](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ). Αυτό προκαλεί Bus υπηρεσίας για να ξεκλειδώσετε το μήνυμα εντός της συνδρομής και να την κάνετε διαθέσιμη θα ληφθεί ξανά, με την ίδια εφαρμογή καταναλώνει ή από άλλη εφαρμογή καταναλώνει.

Υπάρχει επίσης ένα χρονικό όριο που σχετίζεται με ένα μήνυμα κλειδωμένο εντός της συνδρομής και, εάν η εφαρμογή δεν μπορεί να επεξεργαστεί το μήνυμα πριν από την κλείδωμα λήξει το χρονικό όριο (για παράδειγμα, εάν η εφαρμογή παρουσιάζει σφάλμα), στη συνέχεια, υπηρεσία Bus καταργεί αυτόματα το μήνυμα και κάνει διαθέσιμο θα ληφθεί ξανά.

Σε περίπτωση που η εφαρμογή παρουσιάζει σφάλμα μετά την επεξεργασία του μηνύματος, αλλά πριν από την έκδοση της αίτησης [ολοκλήρωσης](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) , το μήνυμα θα redelivered στην εφαρμογή κατά την επανεκκίνηση. Συχνά αυτό ονομάζεται *τουλάχιστον μία φορά επεξεργασία*; Αυτό σημαίνει ότι κάθε μήνυμα υποβάλλεται σε επεξεργασία τουλάχιστον μία φορά, αλλά σε ορισμένες περιπτώσεις ίσως redelivered το ίδιο μήνυμα. Εάν το σενάριο δεν θέλετε να γίνει διπλότυπων επεξεργασίας, στη συνέχεια, προγραμματιστές εφαρμογών πρέπει να Προσθήκη επιπλέον λογικής σε εφαρμογή τους χειρισμού διπλότυπων μήνυμα παράδοσης. Συχνά, αυτό είναι δυνατό χρησιμοποιώντας την ιδιότητα [αναγνωριστικού μηνύματος](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) του μηνύματος, η οποία παραμένει σταθερή κατά μήκος προσπαθειών παράδοσης.

## <a name="delete-topics-and-subscriptions"></a>Διαγράψτε τα θέματα και συνδρομών

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να διαγράψετε το θέμα **TestTopic** από το χώρο ονομάτων υπηρεσίας **HowToSample** .

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Διαγραφή ενός θέματος διαγράφει επίσης τις συνδρομές που έχουν καταχωρηθεί με το θέμα. Συνδρομές μπορεί επίσης να διαγραφεί ανεξάρτητα. Ο ακόλουθος κώδικας περιγράφει πώς μπορείτε να διαγράψετε μια εγγραφή με όνομα **HighMessages** από το θέμα **TestTopic** .

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία των συνδρομών και θέματα Bus υπηρεσίας, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα.

-   [Ουρές, θέματα, και συνδρομών][].
-   [Δείγμα φίλτρα θέμα][]
-   Αναφορά API για [SqlFilter][].
-   Δημιουργία μιας εφαρμογής εργασία που στέλνει και λαμβάνει μηνύματα από μια υπηρεσία Bus ουρά και: [υπηρεσία Bus όπου μεσολαβούν ανταλλαγής μηνυμάτων πρόγραμμα εκμάθησης .NET][].
-   Δείγματα Bus υπηρεσίας: λήψη από το [Azure δείγματα][] ή δείτε την [Επισκόπηση](service-bus-samples.md).

  [Πύλη του Azure]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Ουρές, θέματα και συνδρομών]: service-bus-queues-topics-subscriptions.md
  [Δείγμα φίλτρα θέμα]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Πρόγραμμα εκμάθησης όπου μεσολαβούν Bus υπηρεσία ανταλλαγής μηνυμάτων .NET]: service-bus-brokered-tutorial-dotnet.md
  [Δείγματα Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
