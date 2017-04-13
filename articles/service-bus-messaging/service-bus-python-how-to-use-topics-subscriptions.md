<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε τα θέματα υπηρεσίας Bus με Python | Microsoft Azure" 
    description="Μάθετε πώς να χρησιμοποιείτε θέματα Bus υπηρεσίας Azure και συνδρομές από Python." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Πώς μπορείτε να χρησιμοποιήσετε θέματα Bus υπηρεσίας και συνδρομών

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Σε αυτό το άρθρο περιγράφει τον τρόπο χρήσης της υπηρεσίας Bus θέματα και συνδρομών. Τα δείγματα είναι γραμμένες σε Python και χρησιμοποιήστε το [πακέτο Python Azure][]. Τα σενάρια καλύπτεται περιλαμβάνουν **τη δημιουργία θέματα και συνδρομές**, **τη δημιουργία συνδρομής φίλτρα**, **Αποστολή μηνυμάτων σε ένα θέμα**, **λαμβάνετε μηνύματα από μια συνδρομή**και **Διαγραφή θέματα και συνδρομών**. Για περισσότερες πληροφορίες σχετικά με τα θέματα και οι συνδρομές, ανατρέξτε στην ενότητα [Επόμενα βήματα](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Σημείωση:** Εάν πρέπει να εγκαταστήσετε το [πακέτο Python Azure][]ή Python, ανατρέξτε στον [Οδηγό εγκατάστασης Python](../python-how-to-install.md).

## <a name="create-a-topic"></a>Δημιουργία θέματος

Το αντικείμενο **ServiceBusService** σάς επιτρέπει να εργαστείτε με τα θέματα. Προσθέστε τα ακόλουθα κοντά στο επάνω μέρος οποιουδήποτε αρχείου Python στην οποία θέλετε να πρόσβαση μέσω προγραμματισμού Bus υπηρεσίας:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **ServiceBusService** . Αντικατάσταση `mynamespace`, `sharedaccesskeyname`, και `sharedaccesskey` με το πραγματικό χώρο ονομάτων, κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας) βασικές τιμή όνομα και κλειδιού.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Μπορείτε να αποκτήσετε τις τιμές για το όνομα του κλειδιού συσχετισμών Ασφαλείας και τιμή από την [πύλη του Azure][].

```
bus_service.create_topic('mytopic')
```

**Δημιουργία\_θέμα** υποστηρίζει επίσης πρόσθετες επιλογές, οι οποίες σας επιτρέπουν να Παράκαμψη προεπιλεγμένων ρυθμίσεων θέμα όπως μήνυμα διάρκεια ζωής ή το θέμα μέγιστο μέγεθος. Το παρακάτω παράδειγμα ορίζει το θέμα μέγιστο μέγεθος 5 GB και μια ώρα με live (TTL) τιμή του 1 λεπτό:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Δημιουργία εγγραφών

Συνδρομές σε θέματα δημιουργούνται επίσης με το αντικείμενο **ServiceBusService** . Συνδρομές ονομάζονται και μπορούν να έχουν ένα προαιρετικό φίλτρο που περιορίζει το σύνολο των μηνυμάτων που παραδίδονται σε ουρά εικονικού τη συνδρομή.

> [AZURE.NOTE] Συνδρομές είναι μόνιμη και θα εξακολουθούν να υπάρχουν μέχρι αυτά, είτε το θέμα στο οποίο έχετε εγγραφεί, διαγράφονται.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Δημιουργήστε μια συνδρομή με το φίλτρο προεπιλεγμένη (MatchAll)

Το φίλτρο **MatchAll** είναι το προεπιλεγμένο φίλτρο που χρησιμοποιείται, αν υπάρχει φίλτρο έχει καθοριστεί όταν δημιουργείται μια νέα συνδρομή. Όταν χρησιμοποιείται το φίλτρο **MatchAll** , όλα τα μηνύματα που δημοσιεύονται στο θέμα τοποθετούνται στην ουρά εικονικού τη συνδρομή. Το παρακάτω παράδειγμα δημιουργεί μια συνδρομή με το όνομα 'AllMessages' και χρησιμοποιεί το προεπιλεγμένο φίλτρο **MatchAll** .

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Δημιουργία συνδρομές με τα φίλτρα

Μπορείτε επίσης να ορίσετε φίλτρα που σας επιτρέπουν να καθορίσετε ποιο θα πρέπει να εμφανίζονται τα μηνύματα που αποστέλλονται σε ένα θέμα μέσα σε μια συνδρομή στο συγκεκριμένο θέμα.

Η πιο ευέλικτη τύπο φίλτρου που υποστηρίζονται από συνδρομές είναι μια **SqlFilter**, που εφαρμόζει ένα υποσύνολο SQL92. Φίλτρα SQL λειτουργούν σχετικά με τις ιδιότητες των μηνυμάτων που δημοσιεύονται στο θέμα. Για περισσότερες πληροφορίες σχετικά με τις παραστάσεις που μπορούν να χρησιμοποιηθούν με φίλτρο SQL, δείτε τη σύνταξη [SqlFilter.SqlExpression][] .

Μπορείτε να προσθέσετε φίλτρα σε μια συνδρομή με τη χρήση του **Δημιουργία\_κανόνα** μέθοδο του αντικειμένου **ServiceBusService** . Αυτή η μέθοδος σάς επιτρέπει να προσθέσετε νέα φίλτρα σε μια υπάρχουσα συνδρομή.

> [AZURE.NOTE] Επειδή το προεπιλεγμένο φίλτρο εφαρμόζεται αυτόματα σε όλες τις νέες εγγραφές, πρέπει πρώτα να καταργήσετε το προεπιλεγμένο φίλτρο ή το **MatchAll** θα αντικαταστήσει τα άλλα φίλτρα μπορείτε να καθορίσετε. Μπορείτε να καταργήσετε τον προεπιλεγμένο κανόνα, χρησιμοποιώντας το **Διαγραφή\_κανόνα** μέθοδο του αντικειμένου **ServiceBusService** .

Το ακόλουθο παράδειγμα δημιουργεί μια εγγραφή με όνομα `HighMessages` με **SqlFilter** που επιλέγει μόνο τα μηνύματα που έχουν μια ιδιότητα προσαρμοσμένου **αριθμού μηνύματος** που είναι μεγαλύτερη από 3:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Ομοίως, το ακόλουθο παράδειγμα δημιουργεί μια εγγραφή με όνομα `LowMessages` με **SqlFilter** που επιλέγει μόνο τα μηνύματα που έχουν μια **αριθμού μηνύματος** ιδιότητα μικρότερη ή ίση με 3:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Τώρα, όταν ένα μήνυμα αποστέλλεται `mytopic` παραδίδεται πάντα να δέκτες μια συνδρομή με τη συνδρομή θέμα **AllMessages** και επιλεκτική παραδίδονται δέκτες μια συνδρομή για τις συνδρομές θέμα **HighMessages** και **LowMessages** (ανάλογα με το περιεχόμενο του μηνύματος).

## <a name="send-messages-to-a-topic"></a>Αποστολή μηνυμάτων σε ένα θέμα

Για να στείλετε ένα μήνυμα σε ένα θέμα Bus υπηρεσία, πρέπει να χρησιμοποιήσετε την εφαρμογή σας το **Αποστολή\_θέμα\_μήνυμα** μέθοδο του αντικειμένου **ServiceBusService** .

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να στείλετε πέντε δοκιμαστικών μηνυμάτων για να `mytopic`. Σημειώστε ότι η τιμή της ιδιότητας **αριθμού μηνύματος** για κάθε μήνυμα ποικίλλει σε την επανάληψη του βρόχου (αυτή καθορίζει ποιες συνδρομές λαμβάνουν την):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Θέματα υπηρεσίας Bus υποστηρίζει ένα μέγιστο μέγεθος μηνύματος 256 KB η [τυπική σειρά](service-bus-premium-messaging.md) και 1 MB στο [επίπεδο Premium](service-bus-premium-messaging.md). Την επικεφαλίδα, η οποία περιλαμβάνει τις ιδιότητες τυπικών και προσαρμοσμένων εφαρμογών, μπορεί να έχει ένα μέγιστο μέγεθος των 64 KB. Υπάρχει όριο στον αριθμό των μηνυμάτων που θα διατηρούνται σε ένα θέμα, αλλά υπάρχει μια καπέλο το συνολικό μέγεθος των μηνυμάτων που διαθέτει ένα θέμα. Σε αυτό το θέμα μέγεθος ορίζεται κατά τη δημιουργία, με ένα ανώτερο όριο των 5 GB. Για περισσότερες πληροφορίες σχετικά με τα όρια, ανατρέξτε στο θέμα [όρια Bus υπηρεσίας][].

## <a name="receive-messages-from-a-subscription"></a>Λήψη μηνυμάτων από μια συνδρομή

Γίνεται λήψη των μηνυμάτων από μια συνδρομή με τη χρήση του **Λήψη\_συνδρομή\_μήνυμα** μέθοδος στο αντικείμενο **ServiceBusService** :

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Μηνύματα διαγράφονται από τη συνδρομή, όπως διαβάζονται όταν η παράμετρος **Σύνοψη\_κλείδωμα** έχει οριστεί στην **τιμή False**. Μπορείτε να διαβάσετε (σύνοψη) και να κλειδώσετε το μήνυμα χωρίς να το διαγράψετε από την ουρά, ορίζοντας την παράμετρο **Σύνοψη\_κλείδωμα** στην **τιμή True**.

Η συμπεριφορά της ανάγνωσης και τη διαγραφή του μηνύματος ως μέρος του η λειτουργία λήψης είναι απλούστερο μοντέλο και λειτουργεί καλύτερα για σενάρια στις οποίες μια εφαρμογή μπορεί να αποδεχτεί τις δεν επεξεργασία ενός μηνύματος σε περίπτωση αποτυχίας. Για να κατανοήσετε αυτήν, εξετάστε το ενδεχόμενο να ένα σενάριο που ζητήματα της αίτησης λήψης του καταναλωτή και, στη συνέχεια, παρουσιάζει σφάλμα πριν από την επεξεργασία του. Επειδή η υπηρεσία Bus θα έχουν σήμανση του μηνύματος ως που καταναλώνεται, στη συνέχεια, όταν η εφαρμογή επανεκκίνηση του και αρχίζει κατανάλωση ξανά τα μηνύματα, το θα έχετε παραβλέψει το μήνυμα που καταναλώθηκε πριν από το σφάλμα.

Εάν το **Σύνοψη\_κλείδωμα** η παράμετρος έχει οριστεί στην **τιμή True**, η λήψη γίνεται μια λειτουργία δύο στάδιο, γεγονός που επιτρέπει σε εφαρμογές υποστήριξης που δεν θέλετε να γίνει μηνύματα που λείπουν. Όταν Bus υπηρεσία λαμβάνει μια αίτηση, βρίσκει στο επόμενο μήνυμα να καταναλωθεί, κλειδώνει για να αποτρέψετε άλλους καταναλωτές πραγματοποιεί λήψη του και, στη συνέχεια, επιστρέφει το στην εφαρμογή. Μετά την εφαρμογή ολοκληρώσει την επεξεργασία του μηνύματος (ή αποθηκεύει αξιόπιστα για μελλοντική επεξεργασία), ολοκληρώνει δεύτερου σταδίου της διαδικασίας παραλαβής καλώντας μέθοδο **Διαγραφή** στο **μήνυμα** αντικείμενο. Η μέθοδος **Διαγραφή** επισημαίνει το μήνυμα ως που καταναλώνεται και καταργεί από τη συνδρομή.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Πώς να χειριστείτε σφάλματα εφαρμογών και αναγνώσιμα μηνύματα

Υπηρεσία Bus παρέχει λειτουργικότητα που θα σας βοηθήσουν να ανακτήσετε ομαλά από σφάλματα στο εφαρμογής ή δυσκολίες επεξεργασία ενός μηνύματος. Εάν μια εφαρμογή ακουστικό είναι δυνατή η επεξεργασία του μηνύματος για κάποιο λόγο, στη συνέχεια, το να καλέσετε τη μέθοδο **ξεκλειδώσετε** στο αντικείμενο **μηνύματος** . Αυτό θα προκαλέσει Bus υπηρεσίας για να ξεκλειδώσετε το μήνυμα εντός της συνδρομής και να την κάνετε διαθέσιμη θα ληφθεί ξανά, με την ίδια εφαρμογή καταναλώνει ή από άλλη εφαρμογή καταναλώνει.

Υπάρχει επίσης ένα χρονικό όριο που σχετίζεται με ένα μήνυμα κλειδωμένο εντός της συνδρομής και, εάν η εφαρμογή δεν μπορεί να επεξεργαστεί το μήνυμα πριν από την κλείδωμα λήξει το χρονικό όριο (για παράδειγμα, εάν η εφαρμογή παρουσιάζει σφάλμα), στη συνέχεια, υπηρεσία Bus καταργεί αυτόματα το μήνυμα και κάνει διαθέσιμο θα ληφθεί ξανά.

Σε περίπτωση που η εφαρμογή παρουσιάζει σφάλμα μετά την επεξεργασία του μηνύματος, αλλά πριν από την κλήση της μεθόδου **Διαγραφή** , στη συνέχεια, το μήνυμα θα είναι redelivered στην εφαρμογή κατά την επανεκκίνηση. Συχνά αυτό ονομάζεται **Τουλάχιστον αφού επεξεργασίας**, αυτό σημαίνει ότι θα υποβληθούν τουλάχιστον μία φορά σε κάθε μήνυμα αλλά σε ορισμένες περιπτώσεις ίσως redelivered το ίδιο μήνυμα. Εάν το σενάριο δεν θέλετε να γίνει διπλότυπων επεξεργασίας, στη συνέχεια, προγραμματιστές εφαρμογών πρέπει να Προσθήκη επιπλέον λογικής σε εφαρμογή τους χειρισμού διπλότυπων μήνυμα παράδοσης. Συχνά, αυτό είναι δυνατό χρησιμοποιώντας την ιδιότητα **αναγνωριστικού μηνύματος** του μηνύματος, η οποία θα παραμένει σταθερή κατά μήκος προσπαθειών παράδοσης.

## <a name="delete-topics-and-subscriptions"></a>Διαγράψτε τα θέματα και συνδρομών

Θέματα και συνδρομές είναι μόνιμες και πρέπει να διαγραφούν ρητά είτε μέσω του [Azure πύλη][] ή μέσω προγραμματισμού. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να διαγράψετε το θέμα με το όνομα `mytopic`:

```
bus_service.delete_topic('mytopic')
```

Διαγραφή ενός θέματος διαγράφει επίσης τις συνδρομές που έχουν καταχωρηθεί με το θέμα. Συνδρομές μπορεί επίσης να διαγραφεί ανεξάρτητα. Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να διαγράψετε μια εγγραφή με όνομα `HighMessages` από το `mytopic` το θέμα:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία για τα θέματα Bus υπηρεσίας, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα.

-   Ανατρέξτε στο θέμα [ουρές, θέματα, και συνδρομών][].
-   Αναφορά για [SqlFilter.SqlExpression][].

[Πύλη του Azure]: https://portal.azure.com
[Πακέτο Python Azure]: https://pypi.python.org/pypi/azure  
[Ουρές, θέματα και συνδρομών]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Υπηρεσία Bus ορίων]: service-bus-quotas.md 