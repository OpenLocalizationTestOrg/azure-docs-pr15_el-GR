<properties 
    pageTitle="Διαμερίσματα ουρές και θέματα | Microsoft Azure"
    description="Περιγράφει τον τρόπο δημιουργίας διαμερισμάτων ουρές Bus υπηρεσία και τα θέματα με τη χρήση πολλών μεσίτες μήνυμα."
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
    ms.date="09/02/2016"
    ms.author="sethm;hillaryc" />

# <a name="partitioned-queues-and-topics"></a>Διαμερίσματα ουρές και θέματα

Azure Bus υπηρεσία χρησιμοποιεί πολλά μεσίτες μηνύματος για την επεξεργασία μηνυμάτων και πολλών μηνυμάτων αποθηκεύει για την αποθήκευση μηνυμάτων. Συμβατικός ουρά ή το θέμα είναι χειρίζεται ενός μηνύματος μόνο μεσίτη και είναι αποθηκευμένο σε ένα χώρο αποθήκευσης μηνυμάτων. Υπηρεσία Bus καθίσταται ουρές ή θέματα που θα είναι διαμερίσματα σε πολλές μεσίτες μήνυμα και αποθηκεύει ανταλλαγής μηνυμάτων. Αυτό σημαίνει ότι η συνολική μεταγωγή διαμερίσματα ουρά ή το θέμα δεν είναι πλέον περιορίζεται από τις επιδόσεις broker μήνυμα ή ανταλλαγής μηνυμάτων χώρου αποθήκευσης. Επιπλέον, ένα προσωρινό μη διαθεσιμότητα του χώρου αποθήκευσης μηνυμάτων δεν αποδίδει διαμερίσματα ουρά ή το θέμα διαθέσιμες. Διαμερίσματα ουρές και θέματα μπορούν να περιέχουν όλες τις προηγμένες δυνατότητες Bus υπηρεσία, όπως η υποστήριξη για συναλλαγές και οι περίοδοι λειτουργίας.

Για περισσότερες λεπτομέρειες σχετικά με την υπηρεσία Bus εσωτερικά στοιχεία, ανατρέξτε στο θέμα [αρχιτεκτονική Bus υπηρεσίας][] .

## <a name="how-it-works"></a>Πώς λειτουργεί

Κάθε διαμερίσματα ουρά ή το θέμα αποτελείται από πολλά τμήματα. Κάθε τμήμα αποθηκεύονται σε ένα διαφορετικό χώρο αποθήκευσης ανταλλαγής μηνυμάτων και χειρισμός από ένα διαφορετικό μήνυμα broker. Κατά την αποστολή ενός μηνύματος σε διαμερίσματα ουρά ή το θέμα, υπηρεσία Bus εκχωρεί το μήνυμα σε ένα από τα τμήματα. Η επιλογή μπορεί να γίνει τυχαία, υπηρεσία Bus ή χρησιμοποιώντας έναν αριθμό-κλειδί διαμερισμάτων που μπορεί να καθορίσει τον αποστολέα.

Όταν ένας υπολογιστής-πελάτης θέλει να λάβετε ένα μήνυμα από ένα διαμερίσματα ουρά ή από μια συνδρομή σε ένα διαμερίσματα θέμα, τα ερωτήματα Bus υπηρεσίας όλα τα τμήματα για τα μηνύματα, στη συνέχεια, επιστρέφει το πρώτο μήνυμα που λαμβάνεται από οποιονδήποτε από τους χώρους αποθήκευσης μηνυμάτων στον παραλήπτη. Οι cache Bus υπηρεσίας στο άλλο μηνύματα και επιστρέφει τους όταν λαμβάνει επιπλέον λαμβάνει αιτήσεις. Πρόγραμμα-πελάτη και λήψη δεν γνωρίζει το διαμερισμάτων; η συμπεριφορά άμεσα προσβάσιμη από το πρόγραμμα-πελάτη διαμερίσματα ουρά ή το θέμα (για παράδειγμα, ανάγνωση, ολοκλήρωση, αναβολή, αδρανούς αλληλογραφίας, prefetching) είναι παρόμοια με τη συμπεριφορά κανονική οντότητας.

Δεν υπάρχει χωρίς πρόσθετο κόστος κατά την αποστολή ενός μηνύματος σε ή λαμβάνετε ένα μήνυμα από ένα διαμερίσματα ουρά ή το θέμα.

## <a name="enable-partitioning"></a>Ενεργοποίηση διαμερισμάτων

Για να χρησιμοποιήσετε διαμερίσματα ουρές και θέματα με το Azure Service Bus, χρησιμοποιήστε το SDK Azure έκδοση 2,2 ή νεότερη έκδοση, ή να καθορίσετε `api-version=2013-10` στο HTTP σας ζητά.

Μπορείτε να δημιουργήσετε ουρές Bus υπηρεσίας και θέματα σε 1, 2, 3, 4 ή 5 GB μεγέθη (η προεπιλογή είναι 1 GB). Με διαμερισμάτων με δυνατότητα, υπηρεσία Bus δημιουργεί 16 διαμερίσματα για κάθε GB που καθορίζετε. Ως εκ τούτου, εάν δημιουργήσετε μια ουρά που είναι 5 GB σε μέγεθος, με τα διαμερίσματα 16 το μέγεθος μέγιστο ουράς γίνεται (5 \* 16) = 80 GB. Μπορείτε να δείτε το μέγιστο μέγεθος του διαμερίσματα ουρά ή το θέμα, εξετάζοντας την καταχώρηση στην [πύλη του Azure][].

Υπάρχουν πολλοί τρόποι για να δημιουργήσετε ένα διαμερίσματα ουρά ή το θέμα. Όταν δημιουργείτε στην ουρά ή το θέμα από την εφαρμογή σας, μπορείτε να ενεργοποιήσετε διαμερισμάτων για την ουρά ή το θέμα, αντίστοιχα ορίζοντας την ιδιότητα [QueueDescription.EnablePartitioning][] ή [TopicDescription.EnablePartitioning][] στην **τιμή true**. Αυτές οι ιδιότητες πρέπει να οριστούν τη στιγμή στην ουρά ή δημιουργείται το θέμα. Δεν είναι δυνατή η αλλαγή αυτών των ιδιοτήτων σε υπάρχουσα ουρά ή το θέμα. Για παράδειγμα:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Εναλλακτικά, μπορείτε να δημιουργήσετε μια ουρά διαμερίσματα ή ένα θέμα στο Visual Studio ή στην [πύλη του Azure][]. Όταν δημιουργείτε μια νέα ουρά ή ένα θέμα στην πύλη, ορίστε την επιλογή **Ενεργοποίηση διαμερισμάτων** στο το blade **Γενικές ρυθμίσεις** της ουράς ή το θέμα παράθυρο **ρυθμίσεων** στην **τιμή true**. Στο Visual Studio, επιλέξτε το πλαίσιο ελέγχου **Ενεργοποίηση διαμερισμάτων** στο παράθυρο διαλόγου **Νέα ουρά** ή ένα **Νέο θέμα** .

## <a name="use-of-partition-keys"></a>Χρήση των πλήκτρων partition

Όταν ένα μήνυμα που τοποθετούνται στην ουρά σε διαμερίσματα ουρά ή το θέμα, υπηρεσία Bus ελέγχει για την παρουσία ενός κλειδιού διαμερίσματα. Εάν δεν εντοπίσει, επιλέγει το τμήμα που βασίζονται σε αυτό το κλειδί. Εάν δεν εντοπίσει έναν αριθμό-κλειδί διαμερίσματα, επιλέγει το τμήμα που βασίζεται σε έναν εσωτερικό αλγόριθμο.

### <a name="using-a-partition-key"></a>Χρησιμοποιώντας έναν αριθμό-κλειδί partition

Ορισμένα σενάρια, όπως οι περίοδοι λειτουργίας ή συναλλαγές, απαιτούν μηνύματα να αποθηκευτούν σε ένα συγκεκριμένο τμήμα. Όλα αυτά τα σενάρια απαιτούν τη χρήση ενός κλειδιού διαμερίσματα. Όλα τα μηνύματα που χρησιμοποιούν το ίδιο κλειδί διαμερίσματα έχουν ανατεθεί στο ίδιο τμήμα. Εάν το τμήμα είναι προσωρινά μη διαθέσιμος, υπηρεσία Bus επιστρέφει ένα σφάλμα.

Ανάλογα με το σενάριο, οι ιδιότητες διαφορετικό μήνυμα χρησιμοποιούνται ως έναν αριθμό-κλειδί διαμερίσματα:

**Αναγνωριστικό περιόδου λειτουργίας**: Εάν ένα μήνυμα με την ιδιότητα [BrokeredMessage.SessionId][] έχει οριστεί και, στη συνέχεια, υπηρεσία Bus χρησιμοποιεί αυτήν την ιδιότητα ως το κλειδί διαμερίσματα. Με αυτόν τον τρόπο, όλα τα μηνύματα που ανήκει στην ίδια περίοδο λειτουργίας χειρίζεται το ίδιο broker μήνυμα. Αυτή η δυνατότητα επιτρέπει Bus υπηρεσίας για να εξασφαλίσετε μήνυμα ταξινόμηση καθώς και τη συνέπεια των μελών της περιόδου λειτουργίας.

**PartitionKey**: Εάν έχει ένα μήνυμα, αλλά δεν την ιδιότητα [BrokeredMessage.SessionId][] έχει οριστεί η ιδιότητα [BrokeredMessage.PartitionKey][] και, στη συνέχεια, υπηρεσία Bus χρησιμοποιεί την ιδιότητα [PartitionKey][] ως το κλειδί διαμερίσματα. Εάν το μήνυμα έχει το [αναγνωριστικό περιόδου λειτουργίας][] και το σύνολο ιδιοτήτων [PartitionKey][] , και οι δύο ιδιότητες πρέπει να είναι ίδιες. Εάν η ιδιότητα [PartitionKey][] έχει οριστεί σε μια διαφορετική τιμή από την ιδιότητα [αναγνωριστικό περιόδου λειτουργίας][] , η υπηρεσία Bus επιστρέφει μια εξαίρεση **InvalidOperationException** . Η ιδιότητα [PartitionKey][] πρέπει να χρησιμοποιείται εάν έναν αποστολέα στέλνει μηνύματα συναλλαγών υπόψη μη περιόδου λειτουργίας. Το κλειδί partition εξασφαλίζει ότι όλα τα μηνύματα που αποστέλλονται σε μια συναλλαγή αντιμετωπίζονται από το ίδιο broker ανταλλαγής μηνυμάτων.

**Αναγνωριστικού μηνύματος**: Εάν στην ουρά ή το θέμα έχει την ιδιότητα [QueueDescription.RequiresDuplicateDetection][] έχει οριστεί στην **τιμή true** και τις ιδιότητες [BrokeredMessage.SessionId][] ή [BrokeredMessage.PartitionKey][] δεν έχουν οριστεί, τότε η ιδιότητα [BrokeredMessage.MessageId][] λειτουργεί ως το κλειδί διαμερίσματα. (Σημειώστε ότι οι βιβλιοθήκες Microsoft .NET και AMQP αναθέτει αυτόματα ένα Αναγνωριστικό μηνύματος εάν η εφαρμογή αποστολής δεν.) Σε αυτήν την περίπτωση, όλα τα αντίγραφα του ίδιου μηνύματος χειρίζεται το ίδιο broker μήνυμα. Αυτό σας επιτρέπει Bus υπηρεσίας για τον εντοπισμό και εξάλειψη διπλότυπων μηνύματα. Εάν η ιδιότητα [QueueDescription.RequiresDuplicateDetection][] δεν έχει οριστεί στην **τιμή true**, υπηρεσία Bus δεν μπορείτε να την ιδιότητα [αναγνωριστικού μηνύματος][] ως έναν αριθμό-κλειδί διαμερίσματα.

### <a name="not-using-a-partition-key"></a>Δεν χρησιμοποιείτε έναν αριθμό-κλειδί partition

Κατά την απουσία έναν αριθμό-κλειδί διαμερίσματα, υπηρεσία Bus κατανέμει μηνύματα με τρόπο round robin για όλα τα τμήματα του διαμερίσματα ουρά ή το θέμα. Εάν το επιλεγμένο τμήμα δεν είναι διαθέσιμη, υπηρεσία Bus αναθέτει το μήνυμα σε ένα διαφορετικό τμήμα. Με αυτόν τον τρόπο, η λειτουργία αποστολής ολοκληρωθεί με επιτυχία παρά προσωρινά μη διαθεσιμότητα του χώρου αποθήκευσης μηνυμάτων.

Για να εκχωρήσετε στην υπηρεσία Bus αρκετό χρόνο για να enqueue το μήνυμα σε ένα διαφορετικό τμήμα, την τιμή [MessagingFactorySettings.OperationTimeout][] που καθορίζεται από το πρόγραμμα-πελάτη που στέλνει το μήνυμα πρέπει να είναι μεγαλύτερο από 15 δευτερολέπτων. Συνιστάται να ορίσετε την ιδιότητα [OperationTimeout][] στην προεπιλεγμένη τιμή του 60 δευτερόλεπτα.

Σημειώστε ότι ένα κλειδί partition "κωδικοί PIN" ένα μήνυμα για ένα συγκεκριμένο τμήμα. Εάν ο χώρος αποθήκευσης μηνυμάτων που περιέχει αυτό το τμήμα δεν είναι διαθέσιμη, υπηρεσία Bus επιστρέφει ένα σφάλμα. Κατά την απουσία έναν αριθμό-κλειδί διαμερίσματα, Bus υπηρεσίας να επιλέξετε ένα διαφορετικό τμήμα και τη λειτουργία ολοκληρωθεί με επιτυχία. Επομένως, συνιστάται να ότι δεν θέλετε να καθορίσετε έναν αριθμό-κλειδί διαμερίσματα, εκτός εάν είναι απαραίτητο.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Θέματα για προχωρημένους: χρήση συναλλαγές με διαμερίσματα οντοτήτων

Μηνύματα που αποστέλλονται ως μέρος μιας συναλλαγής πρέπει να καθορίσετε έναν αριθμό-κλειδί διαμερίσματα. Αυτό μπορεί να είναι μία από τις ακόλουθες ιδιότητες: [BrokeredMessage.SessionId][], [BrokeredMessage.PartitionKey][]ή [BrokeredMessage.MessageId][]. Όλα τα μηνύματα που αποστέλλονται ως μέρος της ίδιας πράξης πρέπει να καθορίσετε το ίδιο κλειδί διαμερίσματα. Εάν προσπαθήσετε να στείλετε ένα μήνυμα χωρίς έναν αριθμό-κλειδί partition μέσα σε μια συναλλαγή, υπηρεσία Bus επιστρέφει μια εξαίρεση **InvalidOperationException** . Εάν προσπαθείτε να αποστείλετε πολλά μηνύματα μέσα από την ίδια εντολή που έχουν αριθμούς-κλειδιά διαμερισμάτων διαφορετικά, υπηρεσία Bus επιστρέφει μια εξαίρεση **InvalidOperationException** . Για παράδειγμα:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Εάν οποιαδήποτε από τις ιδιότητες που θα χρησιμοποιηθεί ως έναν αριθμό-κλειδί διαμερίσματα έχουν ρυθμιστεί, υπηρεσία Bus κωδικοί PIN του μηνύματος σε ένα συγκεκριμένο τμήμα. Αυτή η συμπεριφορά παρουσιάζεται μια συναλλαγή χρησιμοποιείται ή όχι. Συνιστάται να ότι δεν καθορίσετε έναν αριθμό-κλειδί διαμερίσματα Εάν δεν είναι απαραίτητο.

## <a name="using-sessions-with-partitioned-entities"></a>Χρήση περιόδους λειτουργίας με διαμερίσματα οντοτήτων

Για να στείλετε ένα μήνυμα συναλλαγών σε μια περίοδο λειτουργίας γνωρίζετε το θέμα ή ουρά, η ιδιότητα [BrokeredMessage.SessionId][] πρέπει να έχει το μήνυμα. Εάν η ιδιότητα [BrokeredMessage.PartitionKey][] έχει οριστεί επίσης, πρέπει να είναι ίδια με την ιδιότητα [αναγνωριστικό περιόδου λειτουργίας][] . Εάν διαφέρουν, υπηρεσία Bus επιστρέφει μια εξαίρεση **InvalidOperationException** .

Σε αντίθεση με κανονική ουρές (δεν έχουν δημιουργηθεί διαμερίσματα) ή τα θέματα, δεν είναι δυνατό να χρησιμοποιήσετε μία συναλλαγή για αποστολή πολλών μηνυμάτων σε διαφορετικές περίοδοι λειτουργίας. Εάν Επιχειρήθηκε, υπηρεσία Bus επιστρέφει μια εξαίρεση **InvalidOperationException **. Για παράδειγμα:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Προώθηση αυτόματης μηνύματος με διαμερίσματα οντοτήτων

Azure Bus υπηρεσία υποστηρίζει προώθησης αυτόματων μηνυμάτων από, για να ή μεταξύ διαμερίσματα οντοτήτων. Για να ενεργοποιήσετε την αυτόματη μηνυμάτων προώθηση κλήσεων, ορίστε την ιδιότητα [QueueDescription.ForwardTo][] στην ουρά προέλευσης ή τη συνδρομή. Εάν το μήνυμα καθορίζει έναν αριθμό-κλειδί διαμερίσματα ([αναγνωριστικό περιόδου λειτουργίας][], [PartitionKey][]ή [αναγνωριστικού μηνύματος][]), αυτό το πλήκτρο partition χρησιμοποιείται για την οντότητα προορισμού.

## <a name="considerations-and-guidelines"></a>Ζητήματα και τις κατευθυντήριες γραμμές

- **Δυνατότητες υψηλής συνέπειας**: Εάν μια οντότητα χρησιμοποιεί δυνατότητες όπως οι περίοδοι λειτουργίας, διπλοτύπων ή ρητό έλεγχο των διαμερισμάτων κλειδί και, στη συνέχεια, οι λειτουργίες ανταλλαγής μηνυμάτων δρομολογούνται πάντα σε συγκεκριμένα τμήματα. Εάν οποιοδήποτε από τα τμήματα εμπειρία μεγάλη κίνηση ή τα υποκείμενα χώρου αποθήκευσης είναι κατεστραμμένος, αυτές οι λειτουργίες αποτύχουν και διαθεσιμότητα μειώνεται. Γενικά, τη συνέπεια είναι ακόμη πολύ μεγαλύτερος από πρόσωπα που δεν έχουν δημιουργηθεί διαμερίσματα, μόνο ένα υποσύνολο κυκλοφορίας αντιμετωπίζει προβλήματα, αντί για όλη την κυκλοφορία.
- **Διαχείριση**: λειτουργίες όπως δημιουργία, ενημέρωση και διαγραφή πρέπει να εκτελεστούν σε όλα τα τμήματα της οντότητας. Εάν οποιοδήποτε τμήμα είναι κατεστραμμένη αυτό θα μπορούσε να έχει ως αποτέλεσμα αποτυχίες για αυτές τις λειτουργίες. Για τη λειτουργία Get, πληροφορίες, όπως καταμετρά μήνυμα πρέπει να είναι συναθροιστεί από όλα τα τμήματα. Εάν οποιοδήποτε τμήμα είναι κατεστραμμένη, η κατάσταση διαθεσιμότητας οντότητα αναφέρεται ως περιορίζεται.
- **Σενάρια μικρού μεγέθους μήνυμα**: για αυτές τις περιπτώσεις, ιδιαίτερα κατά τη χρήση του πρωτοκόλλου HTTP, ίσως χρειαστεί να εκτελούν πολλές λειτουργίες λήψης για να λάβετε όλα τα μηνύματα. Για αιτήσεις παραλαβής, προσκηνίου της εκτελεί μια παραλαβή σε όλα τα τμήματα και τα αποθηκεύει όλες τις απαντήσεις που λάβατε. Μια αίτηση οι επόμενες λήψης στην ίδια σύνδεση να επωφεληθείτε από αυτό σε cache και λήψη των αδρανειών θα κάτω. Ωστόσο, εάν έχετε πολλές συνδέσεις ή χρήση HTTP, που δημιουργεί μια νέα σύνδεση για κάθε αίτηση. Ως εκ τούτου, δεν υπάρχει εγγύηση ότι αυτό θα ξεκινάτε στον ίδιο κόμβο. Εάν όλα τα υπάρχοντα μηνύματα είναι κλειδωμένη και στο cache σε άλλο υπολογιστή-πελάτη, η λειτουργία λήψης επιστρέφει την **τιμή null**. Λήξει τελικά μηνύματα και να λαμβάνετε τις ξανά. Συνιστάται να ενεργών HTTP.
- **Αναζήτηση/Σύνοψη μηνυμάτων**: PeekBatch δεν επιστρέφει πάντα τον αριθμό των μηνυμάτων που καθορίζεται στην [ιδιότητα MessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx). Υπάρχουν δύο συνήθεις αιτίες για αυτό. Μια αιτία είναι ότι το μέγεθος του συγκεντρωτική της συλλογής των μηνυμάτων υπερβαίνει το μέγιστο μέγεθος των 256KB. Μια άλλη αιτία είναι ότι εάν στην ουρά ή το θέμα έχει την [ιδιότητα EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) έχει οριστεί στην **τιμή true**, ένα διαμερίσματα μπορεί να μην έχετε αρκετό μηνυμάτων για να ολοκληρώσετε τον απαιτούμενο αριθμό των μηνυμάτων. Σε γενικές γραμμές, εάν μια εφαρμογή θέλει να λαμβάνετε ένα συγκεκριμένο πλήθος των μηνυμάτων, την πρέπει να καλέσετε [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) επανειλημμένα μέχρι να λαμβάνει αυτόν τον αριθμό των μηνυμάτων ή υπάρχουν περισσότερα μηνύματα για να Ρίξτε μια ματιά. Για περισσότερες πληροφορίες, συμπεριλαμβανομένων των δείγματα κώδικα, ανατρέξτε στο θέμα [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) ή [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx).

## <a name="latest-added-features"></a>Τελευταία έκδοση πρόσθετες δυνατότητες

- Προσθήκη ή κατάργηση κανόνα υποστηρίζεται τώρα με διαμερίσματα οντοτήτων. Διαφορετικό από μη διαμερίσματα οντοτήτων, αυτές οι λειτουργίες δεν υποστηρίζονται στο πλαίσιο συναλλαγών. 
- AMQP υποστηρίζεται τώρα για αποστολή και λήψη μηνυμάτων για να και από ένα διαμερίσματα οντότητα.
- AMQP υποστηρίζεται τώρα για τις ακόλουθες λειτουργίες: [Μαζική αποστολή](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [Λήψη δέσμη](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx), [παραλαβής κατά αριθμό ακολουθίας](https://msdn.microsoft.com/library/azure/hh330765.aspx), [Ρίξτε μια ματιά](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), [Ανανέωση κλειδώματος](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), [Μήνυμα χρονοδιάγραμμα](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [Ακύρωση μηνύματος έχει προγραμματιστεί](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [Προσθήκη κανόνα](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Κατάργηση κανόνα](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Κλείδωμα ανανέωση της περιόδου λειτουργίας](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [Ορισμός κατάστασης λειτουργίας](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [Λήψη κατάσταση περιόδου λειτουργίας](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [Ρίξτε μια ματιά περιόδου λειτουργίας μηνύματα](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx)και [Απαρίθμηση περιόδους λειτουργίας](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Περιορισμοί διαμερίσματα οντοτήτων

Προς το παρόν Bus υπηρεσίας επιβάλλει τους ακόλουθους περιορισμούς διαμερίσματα ουρές και τα θέματα:

-   Διαμερίσματα ουρές και θέματα δεν υποστηρίζει την αποστολή μηνυμάτων που ανήκουν σε διαφορετικές περίοδοι λειτουργίας σε μία συναλλαγή.
-   Υπηρεσία Bus επιτρέπει αυτήν τη στιγμή έως 100 διαμερίσματα ουρές ή τα θέματα ανά χώρο ονομάτων. Κάθε διαμερίσματα ουρά ή το θέμα καταμετρά προς το όριο των 10.000 οντοτήτων ανά χώρο ονομάτων (δεν ισχύει για το επίπεδο Premium).

## <a name="next-steps"></a>Επόμενα βήματα

Ανατρέξτε στο θέμα συζήτησης του [AMQP 1.0 υποστήριξη για την υπηρεσία Bus διαμερίσματα ουρές και θέματα][] για να μάθετε περισσότερα σχετικά με τη δημιουργία διαμερισμάτων ανταλλαγής μηνυμάτων οντοτήτων. 

  [Αρχιτεκτονική Bus υπηρεσίας]: service-bus-architecture.md
  [Πύλη του Azure]: https://portal.azure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [Αναγνωριστικό περιόδου λειτουργίας]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [Αναγνωριστικού μηνύματος]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [Υποστήριξη AMQP 1.0 Bus υπηρεσίας διαμερίσματα ουρές και θέματα]: service-bus-partitioned-queues-and-topics-amqp-overview.md
