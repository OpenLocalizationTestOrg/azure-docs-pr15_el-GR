<properties 
    pageTitle="Επισκόπηση του μοντέλου διανομείς συμβάντων ελέγχου ταυτότητας και ασφάλεια | Microsoft Azure"
    description="Διανομείς τον έλεγχο ταυτότητας και την ασφάλεια μοντέλο Επισκόπηση συμβάντων."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Διανομείς τον έλεγχο ταυτότητας και την ασφάλεια μοντέλο Επισκόπηση συμβάντων

Το μοντέλο ασφαλείας διανομείς συμβάν ικανοποιεί τις ακόλουθες απαιτήσεις:

- Μόνο τις συσκευές που παρουσιάζουν έγκυρα διαπιστευτήρια μπορεί να στείλει δεδομένα σε ένα συμβάν διανομέα.
- Μια συσκευή δεν μπορεί να κάνει απομίμηση κάποια άλλη συσκευή.
- Μια συσκευή επικίνδυνο μπορεί να έχει αποκλειστεί από την αποστολή δεδομένων σε ένα συμβάν διανομέα.

## <a name="device-authentication"></a>Έλεγχος ταυτότητας συσκευή

Το μοντέλο ασφαλείας διανομείς συμβάν βασίζεται σε ένα συνδυασμό διακριτικά [Θέσει σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) και *εκδότες συμβάν*. Ένα συμβάν publisher ορίζει ένα εικονικό τελικό σημείο για ένα συμβάν διανομέα. Το publisher μπορεί να χρησιμοποιηθεί μόνο για την αποστολή μηνυμάτων με ένα διανομέα συμβάν. Δεν είναι δυνατή η λήψη μηνυμάτων από εκδότη.

Συνήθως, ένα συμβάν διανομέα απασχολεί ενός εκδότη ανά συσκευή. Όλα τα μηνύματα που αποστέλλονται σε οποιοδήποτε από τα εκδότες με ένα διανομέα συμβάν είναι που τοποθετούνται στην ουρά μέσα σε αυτόν το διανομέα συμβάν. Εκδότες Ενεργοποίηση έλεγχο πρόσβασης σε λεπτομερή και περιορισμού.

Κάθε συσκευή έχει εκχωρηθεί ένα μοναδικό διακριτικό, το οποίο έχει αποσταλεί στη συσκευή. Τα διακριτικά παράγονται τέτοια ώστε κάθε μοναδικό διακριτικό που εκχωρεί πρόσβαση σε διαφορετικό μοναδικό εκδότη. Μια συσκευή που διαθέτει ένα διακριτικό να στείλετε μόνο μία publisher, αλλά δεν τον publisher. Εάν πολλές συσκευές από κοινού το ίδιο διακριτικό, κάθε μία από αυτές τις συσκευές μοιράζεται εκδότη.

Παρόλο που δεν συνιστάται, είναι δυνατό να εξοπλισμός συσκευές με διακριτικά που εκχωρούν άμεση πρόσβαση με ένα διανομέα συμβάν. Οποιαδήποτε συσκευή όπου διατηρείται διακριτικού μπορεί να στείλει μηνύματα απευθείας σε αυτόν το διανομέα συμβάν. Η συσκευή δεν θα υπόκεινται περιορισμού. Επιπλέον, η συσκευή δεν μπορεί να είναι τηλεφώνου χωρίς αξιοπιστία από την αποστολή σε αυτόν το διανομέα συμβάν.

Όλα τα διακριτικά είναι συνδεδεμένοι με έναν αριθμό-κλειδί συσχετισμών Ασφαλείας. Συνήθως, όλων των διακριτικών είναι συνδεδεμένοι με το ίδιο κλειδί. Συσκευές δεν είναι γνωρίζετε το κλειδί. Αυτό αποτρέπει τις συσκευές από κατασκευαστικές διακριτικά.

### <a name="create-the-sas-key"></a>Δημιουργήστε το κλειδί συσχετισμών Ασφαλείας

Κατά τη δημιουργία ενός χώρου ονομάτων διανομείς συμβάντος, διανομείς συμβάν Azure δημιουργεί ένα κλειδί συσχετισμών Ασφαλείας 256-bit με το όνομα **RootManageSharedAccessKey**. Αυτό κλειδιού επιχορηγήσεις αποστολή, ακρόαση και διαχείριση δικαιωμάτων στο χώρο ονομάτων. Μπορείτε να δημιουργήσετε επιπλέον πλήκτρα. Συνιστάται να που παράγει έναν αριθμό-κλειδί που επιχορηγήσεις αποστολή δικαιωμάτων στο συγκεκριμένο διανομέα συμβάν. Για το υπόλοιπο μέρος αυτού του θέματος, θεωρείται ότι μπορείτε με το όνομα αυτού του πλήκτρου `EventHubSendKey`.

Το παρακάτω παράδειγμα δημιουργεί ένα κλειδί αποστολή μόνο κατά τη δημιουργία την ενότητα συμβάν:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Δημιουργία διακριτικά

Μπορείτε να δημιουργήσετε κώδικες, χρησιμοποιώντας το πλήκτρο συσχετισμών Ασφαλείας. Πρέπει να μπορείτε να δημιουργήσετε μόνο μία διακριτικό ανά συσκευή. Στη συνέχεια, μπορεί να παραχθεί διακριτικά χρησιμοποιώντας την ακόλουθη μέθοδο. Όλα τα διακριτικά δημιουργούνται χρησιμοποιώντας το πλήκτρο **EventHubSendKey** . Κάθε διακριτικό αντιστοιχίζεται μια μοναδική διεύθυνση URI.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Όταν καλέσετε αυτή τη μέθοδο, θα πρέπει να έχει καθοριστεί το URI ως `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Για όλα τα διακριτικά, το URI δεν είναι πανομοιότυπες, με εξαίρεση των `PUBLISHER_NAME`, που πρέπει να είναι διαφορετικές για κάθε διακριτικό. Ιδανικά, `PUBLISHER_NAME` αντιπροσωπεύει το Αναγνωριστικό της συσκευής που λαμβάνει το διακριτικό αυτό.

Αυτή η μέθοδος δημιουργεί ένα διακριτικό με την ακόλουθη δομή:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

Την ώρα λήξης διακριτικού έχει καθοριστεί σε δευτερόλεπτα από την 1η Ιανουαρίου 1970. Ακολουθεί ένα παράδειγμα ενός διακριτικού:

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Συνήθως, τα διακριτικά έχουν μια διάρκεια ζωής που μοιάζει με ή υπερβαίνει τη διάρκεια ζωής της συσκευής. Εάν η συσκευή έχει τη δυνατότητα να προμηθευτείτε έναν νέο κωδικό, μπορεί να χρησιμοποιηθεί διακριτικά με μια μικρότερη διάρκεια ζωής.

### <a name="devices-sending-data"></a>Συσκευές αποστολή δεδομένων

Αφού δημιουργηθούν τα διακριτικά, κάθε συσκευή παρέχεται με το δικό της μοναδικό διακριτικό.

Όταν η συσκευή σας στείλει δεδομένων σε ένα συμβάν διανομέα, η συσκευή ετικέτες το διακριτικό με η αίτηση αποστολής. Για να αποτρέψετε ένας εισβολέας από μη εξουσιοδοτημένη ανάγνωση μηνυμάτων και την κλοπή το διακριτικό, την επικοινωνία μεταξύ της συσκευής και την ενότητα συμβάντων πρέπει να είναι πάνω από ένα κρυπτογραφημένο κανάλι.

### <a name="blacklisting-devices"></a>Blacklisting συσκευές

Εάν ο εισβολέας κλοπής ενός διακριτικού, ο εισβολέας μπορεί να μίμηση στη συσκευή διακριτικό του οποίου έχει γίνει κλοπής. Μια συσκευή blacklisting αποδίδει τη συσκευή χωρίς δυνατότητα χρήσης μέχρι τη συσκευή δίνεται έναν νέο κωδικό που χρησιμοποιεί διαφορετικό εκδότη.

## <a name="authentication-of-back-end-applications"></a>Έλεγχος ταυτότητας με τις εφαρμογές παρασκηνίου

Για τον έλεγχο ταυτότητας παρασκηνίου εφαρμογές που εκμετάλλευση των δεδομένων που δημιουργούνται από συσκευές, διανομείς συμβάν χρησιμοποιεί ένα μοντέλο ασφάλειας που είναι παρόμοιο με το μοντέλο που χρησιμοποιείται για την υπηρεσία Bus θέματα. Μια ομάδα καταναλωτή διανομείς συμβάν είναι ισοδύναμο με μια συνδρομή σε ένα θέμα Bus υπηρεσίας. Πρόγραμμα-πελάτη και να δημιουργήσετε μια ομάδα καταναλωτή, αν η αίτηση για να δημιουργήσετε την ομάδα καταναλωτή συνοδεύεται από ενός διακριτικού ότι επιχορηγήσεις Διαχείριση δικαιωμάτων για την ενότητα συμβάν ή για το χώρο ονομάτων στην οποία ανήκει ο διανομέας συμβάν. Επιτρέπεται ένα πρόγραμμα-πελάτη για την εκμετάλλευση δεδομένα από μια ομάδα καταναλωτή αν της αίτησης λήψης συνοδεύεται από ένα διακριτικό που εκχωρεί δικαιώματα παραλαβής σε αυτήν την ομάδα καταναλωτή, την ενότητα συμβάν ή το χώρο ονομάτων στην οποία ανήκει ο διανομέας συμβάν.

Στην τρέχουσα έκδοση της υπηρεσίας Bus δεν υποστηρίζει κανόνες συσχετισμών Ασφαλείας για μεμονωμένες συνδρομές. Το ίδιο ισχύει για τις ομάδες καταναλωτών συμβάν διανομείς. Υποστήριξη συσχετισμών Ασφαλείας θα προστεθούν στο μέλλον για τις δυνατότητες και τις δύο.

Κατά την απουσία ελέγχου ταυτότητας συσχετισμών Ασφαλείας για καταναλωτές μεμονωμένες ομάδες, μπορείτε να χρησιμοποιήσετε πλήκτρα συσχετισμών Ασφαλείας για την ασφάλιση όλες τις ομάδες καταναλωτών με κοινές αριθμό-κλειδί. Αυτή η προσέγγιση επιτρέπει σε μια εφαρμογή για την εκμετάλλευση δεδομένα από οποιαδήποτε από τις ομάδες καταναλωτών με ένα διανομέα συμβάν.

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με το συμβάν διανομείς, επισκεφθείτε τα ακόλουθα θέματα:

- [Επισκόπηση διανομείς συμβάντων]
- Μια [στην ουρά μηνυμάτων λύση] χρησιμοποιώντας ουρές Bus υπηρεσίας.
- Μια ολοκληρωμένη [δείγμα εφαρμογής που χρησιμοποιεί το συμβάν διανομείς].

[Επισκόπηση διανομείς συμβάντων]: event-hubs-overview.md
[δείγμα εφαρμογής που χρησιμοποιεί το συμβάν διανομείς]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[στην ουρά μηνυμάτων λύσης]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
