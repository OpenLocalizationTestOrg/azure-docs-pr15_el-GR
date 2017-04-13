<properties 
    pageTitle="Υπηρεσία Bus συναλλαγές | Microsoft Azure" 
    description="Επισκόπηση των Azure Service Bus μεμονωμένες συναλλαγές και αποστολή μέσω" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="10/04/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-transaction-processing"></a>Επισκόπηση της επεξεργασίας συναλλαγών Bus υπηρεσίας

Σε αυτό το άρθρο περιγράφει τις δυνατότητες κίνησης του Azure Bus υπηρεσίας. Ιδιαίτερα της συζήτησης απεικονίζεται από τις [Μεμονωμένες συναλλαγές με δείγμα Bus υπηρεσίας](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions). Σε αυτό το άρθρο περιορίζεται σε μια επισκόπηση των επεξεργασία συναλλαγών και τη δυνατότητα *Αποστολή μέσω* στην υπηρεσία Bus, ενώ το δείγμα μεμονωμένες συναλλαγές είναι ευρύτερης και πιο σύνθετες στην εμβέλεια.

## <a name="transactions-in-service-bus"></a>Οι συναλλαγές Bus υπηρεσίας

Μια [συναλλαγή](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) ομάδων δύο ή περισσότερες λειτουργίες μαζί σε ένα *εύρος εκτέλεσης*. Λόγω του χαρακτήρα, μια τέτοια συναλλαγή πρέπει να βεβαιωθείτε ότι όλες οι λειτουργίες που ανήκουν σε μια δεδομένη ομάδα λειτουργιών επιτύχει ή αποτύχει από κοινού. Σχετικά με αυτό συναλλαγές λειτουργεί ως μία μονάδα, η οποία αναφέρεται συχνά ως *ατομικότητα*. 

Υπηρεσία Bus είναι ένα μήνυμα συναλλαγών broker και εξασφαλίζει συναλλαγών ακεραιότητα για όλες τις εσωτερικές εργασίες σε σχέση με το μήνυμα αποθηκεύει. Μεταφορές όλων των μηνυμάτων μέσα σε Bus υπηρεσία, όπως η μετακίνηση μηνυμάτων σε [ουρά νεκρού γράμματα](service-bus-dead-letter-queues.md) ή [Αυτόματη προώθηση](service-bus-auto-forwarding.md) των μηνυμάτων μεταξύ οντοτήτων, είναι συναλλαγών. Ως εκ τούτου, εάν Bus υπηρεσίας αποδέχεται ένα μήνυμα, αυτό έχει ήδη αποθηκευμένο και με την ετικέτα με έναν αριθμό ακολουθίας. Οποιοδήποτε μήνυμα μεταφορές μέσα σε υπηρεσία Bus είναι συντονισμένη λειτουργίες κατά μήκος οντοτήτων από και, στη συνέχεια, στο, και το κανένα αποτέλεσμα απώλεια (με επιτυχία προέλευσης και προορισμού αποτυγχάνει) ή για αναπαραγωγή (αποτυγχάνει προέλευσης και προορισμού ολοκληρωθεί με επιτυχία) του μηνύματος.

Υπηρεσία Bus υποστηρίζει ενεργειών ομαδοποίηση κατά μία ανταλλαγής μηνυμάτων οντότητα (ουρά, το θέμα, συνδρομή) στο πεδίο εφαρμογής της συναλλαγή. Για παράδειγμα, μπορείτε να στείλετε πολλά μηνύματα σε μία ουρά από μέσα σε ένα πεδίο συναλλαγών και τα μηνύματα θα είναι μόνο δεσμευμένη καταγραφής στην ουρά όταν ολοκληρωθεί με επιτυχία η συναλλαγή.

## <a name="operations-within-a-transaction-scope"></a>Λειτουργίες μέσα σε ένα πεδίο συναλλαγής 

Οι λειτουργίες που μπορούν να εκτελεστούν μέσα σε ένα πεδίο συναλλαγής είναι ως εξής:

- ** [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: Αποστολή, SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: ολοκληρωθεί, CompleteAsync, Abandon, AbandonAsync, αδρανούς αλληλογραφίας, DeadletterAsync, αναβάλετε, DeferAsync, RenewLock, RenewLockAsync 

Λήψη δεν περιλαμβάνονται οι λειτουργίες, επειδή θεωρείται ότι η εφαρμογή θα αποκτήσει ο μηνύματα χρησιμοποιώντας τη λειτουργία [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) , μέσα σε ορισμένα λαμβάνουν βρόχο ή με μια [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx) επιστροφής κλήσης και μόνο, στη συνέχεια, ανοίγει ένα εύρος κίνησης για την επεξεργασία του μηνύματος.

Διάθεσης του μηνύματος (πλήρες, abandon, νεκρού γράμματα, αναβάλετε) παρουσιάζεται, στη συνέχεια, στο πεδίο εφαρμογής του, και εξαρτώμενα στην το συνολικό αποτέλεσμα της κίνησης.

## <a name="transfers-and-send-via"></a>Μεταφορές και "Αποστολή μέσω"

Για να ενεργοποιήσετε συναλλαγών handover δεδομένων από μια ουρά σε έναν επεξεργαστή και, στη συνέχεια, σε άλλη ουρά, υπηρεσία Bus υποστηρίζει *μεταφορές*. Σε μια λειτουργία μεταφοράς, έναν αποστολέα πρώτα στέλνει ένα μήνυμα σε μια "μεταβίβαση ουρά" και η μεταβίβαση ουρά αμέσως μετακινεί το μήνυμα στην ουρά προβλεπόμενο προορισμό χρησιμοποιώντας την ίδια εφαρμογή ισχυρό μεταφοράς που τη δυνατότητα αυτόματης προώθησης που βασίζεται σε. Το μήνυμα δεν είναι ποτέ δεσμευμένη καταγραφής της ουράς μεταφοράς με τον τρόπο που γίνεται ορατό για καταναλωτές της ουράς μεταφορά.

Στη δύναμη του αυτήν τη δυνατότητα συναλλαγών εκδηλώνεται όταν μεταβίβαση ουρά μόνη της προέλευσης εισαγωγής μηνυμάτων του αποστολέα. Με άλλα λόγια, υπηρεσία Bus να μεταφέρετε το μήνυμα στην ουρά προορισμού "μέσω" ουρά μεταφοράς, κατά την εκτέλεση μια ολοκληρωμένη (ή να αναβάλετε, ή νεκρού γραμμάτων) λειτουργία το μήνυμα εισαγωγής, όλα σε μία ατομική λειτουργία. 

### <a name="see-it-in-code"></a>Δείτε στο κώδικα

Για να ορίσετε η μεταβίβαση, μπορείτε να δημιουργήσετε έναν αποστολέα του μηνύματος που πρόκειται να εφαρμοστεί η ουρά προορισμού μέσω ουρά μεταφορά. Μπορείτε, επίσης, θα έχετε ένα δέκτη που χρησιμοποιεί τα μηνύματα από την ίδια ουρά. Για παράδειγμα:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Στη συνέχεια, μια απλή συναλλαγή χρησιμοποιεί αυτά τα στοιχεία, όπως στο ακόλουθο παράδειγμα:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Επόμενα βήματα

Ανατρέξτε στα ακόλουθα άρθρα για περισσότερες πληροφορίες σχετικά με την υπηρεσία Bus ουρές:

- [Αυτόματη προώθηση αλληλουχίας οντοτήτων Bus υπηρεσίας](service-bus-auto-forwarding.md)
- [Αυτόματη προώθηση δείγματος](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward)
- [Μεμονωμένες συναλλαγές με δείγμα Bus υπηρεσίας](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)
- [Azure ουρές και υπηρεσία Bus ουρές σε σύγκριση](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας](service-bus-dotnet-get-started-with-queues.md)