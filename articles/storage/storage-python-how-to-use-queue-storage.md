<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης ουρά από Python | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία ουράς Azure από Python για να δημιουργήσετε και να διαγράψετε ουρές, εισαγωγή, λήψη και να διαγράψετε μηνύματα."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-python"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης ουρά από Python

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός δείχνει πώς μπορείτε να εκτελέσετε συνηθισμένα σενάρια με την υπηρεσία ουράς Azure χώρου αποθήκευσης. Τα δείγματα είναι γραμμένες σε Python και χρησιμοποιήστε το [Microsoft Azure SDK χώρου αποθήκευσης για Python]. Τα σενάρια καλύπτεται περιλαμβάνουν **εισαγωγής**, **παρατήρηση**, **λήψη**και **Διαγραφή** ουρά μηνυμάτων, καθώς και **τη δημιουργία και διαγραφή ουρές**. Για περισσότερες πληροφορίες σχετικά με ουρές, ανατρέξτε στην ενότητα [επόμενα βήματα].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Τρόπος: Δημιουργία ουράς

Το αντικείμενο **QueueService** σάς επιτρέπει να εργάζεστε με ουρές. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **QueueService** . Προσθέστε τα ακόλουθα κοντά στο επάνω μέρος οποιουδήποτε αρχείου Python στην οποία θέλετε να πρόσβαση μέσω προγραμματισμού αποθήκευσης Azure:

    from azure.storage.queue import QueueService

Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **QueueService** χρησιμοποιώντας το κλειδί αποθήκευσης λογαριασμού όνομα και το λογαριασμό. Αντικατάσταση 'ο λογαριασμός μου' και 'mykey' με το όνομα του λογαριασμού και αριθμού-κλειδιού.

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## <a name="how-to-insert-a-message-into-a-queue"></a>Τρόπος: Εισαγάγετε ένα μήνυμα σε μια ουρά

Για να εισαγάγετε ένα μήνυμα σε μια ουρά, χρησιμοποιήστε το **Τοποθέτηση\_μήνυμα** μέθοδο για να δημιουργήσετε ένα νέο μήνυμα και να την προσθέσετε στην ουρά.

    queue_service.put_message('taskqueue', u'Hello World')


## <a name="how-to-peek-at-the-next-message"></a>Τρόπος: Ρίξτε μια ματιά σε στο επόμενο μήνυμα

Μπορείτε να Ρίξτε μια ματιά σε το μήνυμα στο μπροστινό μέρος μια ουρά χωρίς να την καταργήσετε από την ουρά καλώντας την **Σύνοψη\_μηνύματα** μέθοδο. Από προεπιλογή, **Σύνοψη\_μηνύματα** peeks σε ένα μήνυμα.

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.content)


## <a name="how-to-dequeue-messages"></a>Τρόπος: Απομάκρυνση από την ουρά μηνυμάτων

Τον κωδικό καταργεί ένα μήνυμα από μια ουρά σε δύο βήματα. Όταν καλείτε **Λήψη\_μηνύματα**, μεταβείτε στο επόμενο μήνυμα σε μια ουρά από προεπιλογή. Ένα μήνυμα που επιστρέφονται από **Λήψη\_μηνύματα** δεν είναι ορατό σε οποιονδήποτε άλλο κωδικό ανάγνωση μηνυμάτων από αυτήν την ουρά. Από προεπιλογή, αυτό το μήνυμα παραμένει αόρατο για 30 δευτερόλεπτα. Για να ολοκληρώσετε την κατάργηση του μηνύματος από την ουρά, πρέπει να καλέσετε επίσης **Διαγραφή\_μήνυμα**. Αυτή η διαδικασία δύο βημάτων από την κατάργηση ενός μηνύματος διασφαλίζει ότι όταν αποτύχει η επεξεργασία ενός μηνύματος λόγω αποτυχία υλικού ή λογισμικού σας κώδικα, μια άλλη παρουσία του κωδικού σας μπορεί να λάβετε το ίδιο μήνυμα και να προσπαθήστε ξανά. Οι κλήσεις σας κώδικα **Διαγραφή\_μήνυμα** αμέσως μόλις το μήνυμα έχει υποστεί επεξεργασία.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)

Υπάρχουν δύο τρόποι για να προσαρμόσετε ανάκτηση μηνύματος από μια ουρά.
Πρώτα, μπορείτε να λάβετε μια δέσμη μηνυμάτων (έως 32). Δεύτερον, μπορείτε να ορίσετε ένα χρονικό όριο invisibility μεγαλύτερη ή μικρότερη, επιτρέποντάς σας κώδικα περισσότερες ή λιγότερο χρόνο για την επεξεργασία πλήρως κάθε μήνυμα. Τον ακόλουθο κώδικα, το παράδειγμα χρησιμοποιεί τη **Λήψη\_μηνύματα** μέθοδο για να λάβετε μηνύματα 16 σε μία κλήση. Στη συνέχεια, επεξεργάζεται κάθε μηνύματος ηλεκτρονικού ταχυδρομείου χρησιμοποιώντας μια για επανάληψη. Επίσης, ορίζει το χρονικό όριο invisibility έως πέντε λεπτά για κάθε μήνυμα.

    messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)      


## <a name="how-to-change-the-contents-of-a-queued-message"></a>Τρόπος: Αλλαγή των περιεχομένων ένα μήνυμα σε ουρά

Μπορείτε να αλλάξετε τα περιεχόμενα ενός μηνύματος στην ίδια θέση στην ουρά. Εάν το μήνυμα αντιπροσωπεύει μια εργασία, μπορείτε να χρησιμοποιήσετε αυτήν τη δυνατότητα να ενημερώσετε την κατάσταση της εργασίας εργασίας. Ο κώδικας κάτω από το χρησιμοποιεί το **Ενημέρωση\_μήνυμα** μέθοδο για να ενημερώσετε ένα μήνυμα. Το χρονικό όριο ορατότητα έχει οριστεί σε 0, γεγονός που σημαίνει ότι το μήνυμα εμφανίζεται αμέσως και το περιεχόμενο είναι ενημερωθεί.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')

## <a name="how-to-get-the-queue-length"></a>Τον τρόπο: Το μήκος ουράς γρήγορα

Μπορείτε να λάβετε μια εκτίμηση της τον αριθμό των μηνυμάτων σε μια ουρά. Το **Λήψη\_ουρά\_μετα-δεδομένων** μέθοδο ζητά από την υπηρεσία ουράς για να επιστρέψετε μετα-δεδομένα σχετικά με την ουρά και το **approximate_message_count**. Το αποτέλεσμα είναι μόνο κατά προσέγγιση, επειδή τα μηνύματα μπορούν να προστεθούν ή να καταργηθούν μετά την υπηρεσία ουράς αποκρίνεται σε την αίτησή σας.

    metadata = queue_service.get_queue_metadata('taskqueue')
    count = metadata.approximate_message_count

## <a name="how-to-delete-a-queue"></a>Τρόπος: Διαγραφή ουράς

Για να διαγράψετε μια ουρά και όλα τα μηνύματα που περιέχονται σε αυτό, καλέστε την **Διαγραφή\_ουρά** μέθοδο.

    queue_service.delete_queue('taskqueue')

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του χώρου αποθήκευσης ουρά, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα.

- [Κέντρο για προγραμματιστές Python](/develop/python/)
- [Υπηρεσίες Azure αποθήκευσης REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης]
- [Χώρος αποθήκευσης Microsoft Azure SDK για Python]

[Ιστολόγιο ομάδας Azure χώρου αποθήκευσης]: http://blogs.msdn.com/b/windowsazurestorage/
[Χώρος αποθήκευσης Microsoft Azure SDK για Python]: https://github.com/Azure/azure-storage-python
