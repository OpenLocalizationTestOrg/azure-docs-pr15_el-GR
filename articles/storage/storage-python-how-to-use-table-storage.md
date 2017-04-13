<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων από Python | Microsoft Azure"
    description="Αποθηκεύστε δομημένων δεδομένων στο cloud χρησιμοποιώντας χώρος αποθήκευσης πινάκων του Azure, ένα χώρο αποθήκευσης δεδομένων NoSQL."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-python"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων από Python

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός δείχνει πώς μπορείτε να εκτελέσετε συνηθισμένα σενάρια, χρησιμοποιώντας την υπηρεσία αποθήκευσης Azure πίνακα. Τα δείγματα είναι γραμμένες σε Python και χρησιμοποιήστε το [Microsoft Azure SDK χώρου αποθήκευσης για Python]. Τα εμπλεκόμενα σενάρια περιλαμβάνουν τη δημιουργία και τη διαγραφή ενός πίνακα, εκτός από την εισαγωγή και την υποβολή ερωτημάτων οντοτήτων σε έναν πίνακα.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Δημιουργία πίνακα

Το αντικείμενο **TableService** σάς επιτρέπει να εργάζεστε με τις υπηρεσίες του πίνακα. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **TableService** . Προσθήκη του κώδικα κοντά στο επάνω μέρος οποιουδήποτε αρχείου Python στην οποία θέλετε να πρόσβαση μέσω προγραμματισμού αποθήκευσης Azure:

    from azure.storage.table import TableService, Entity

Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **TableService** , χρησιμοποιώντας το κλειδί χώρου αποθήκευσης λογαριασμού όνομα και το λογαριασμό.  Αντικατάσταση 'ο λογαριασμός μου' και 'mykey' με το όνομα του λογαριασμού και αριθμού-κλειδιού.

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## <a name="add-an-entity-to-a-table"></a>Προσθέστε μια οντότητα σε έναν πίνακα

Για να προσθέσετε μια οντότητα, δημιουργήστε πρώτα ένα λεξικό ή οντότητα που καθορίζει την οντότητα ιδιότητα ονόματα και τις τιμές. Σημειώστε ότι για κάθε οντότητα, πρέπει να καθορίσετε **PartitionKey** και **RowKey**. Αυτά είναι τα μοναδικά αναγνωριστικά των οντοτήτων σας. Μπορείτε να υποβάλετε ερώτημα με αυτές τις τιμές πολύ ταχύτερα από μπορείτε να υποβάλετε ερώτημα σας άλλες ιδιότητες. Το σύστημα χρησιμοποιεί **PartitionKey** για την αυτόματη διανομή των οντοτήτων πίνακα επάνω από πολλούς κόμβους χώρου αποθήκευσης.
Πρόσωπα που έχουν το ίδιο **PartitionKey** αποθηκεύονται στον ίδιο κόμβο. **RowKey** είναι το μοναδικό Αναγνωριστικό της οντότητας εντός τα διαμερίσματα που ανήκει σε.

Για να προσθέσετε μια οντότητα στον πίνακά σας, μεταβιβάζουν ένα αντικείμενο λεξικό για να το **Εισαγωγή\_οντότητα** μέθοδο.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

Μπορείτε επίσης να περάσετε μια παρουσία της κλάσης **οντότητα** για να την **Εισαγωγή\_οντότητα** μέθοδο.

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## <a name="update-an-entity"></a>Ενημέρωση μιας οντότητας

Αυτός ο κωδικός δείχνει πώς μπορείτε να αντικαταστήσετε την παλιά έκδοση μιας υπάρχουσας οντότητας με μια ενημερωμένη έκδοση.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

Εάν δεν υπάρχει η οντότητα που ενημερώνεται, στη συνέχεια, η λειτουργία update θα αποτύχει. Εάν θέλετε να αποθηκεύσετε μια οντότητα ανεξάρτητα από το εάν το υπήρχε πριν από, χρησιμοποιήστε **Εισαγωγή\_ή\_replace_entity**.
Στο παρακάτω παράδειγμα, η πρώτη κλήση θα αντικαταστήσει την υπάρχουσα οντότητα. Η δεύτερη κλήση θα εισαγάγετε μια νέα οντότητα, δεδομένου ότι δεν υπάρχει η οντότητα με το καθορισμένο **PartitionKey** και **RowKey** υπάρχει στον πίνακα.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

## <a name="change-a-group-of-entities"></a>Αλλάξτε μια ομάδα οντοτήτων

Μερικές φορές έχει νόημα να υποβάλετε πολλές λειτουργίες μαζί σε μια δέσμη για να βεβαιωθείτε ότι ατομικής επεξεργασία από το διακομιστή. Για να εκτελέσετε που, μπορείτε να χρησιμοποιήσετε την κλάση **TableBatch** . Όταν θέλετε να υποβάλετε τη δέσμη, καλείτε **Υποβολή\_δέσμης**. Σημειώστε ότι όλα οντοτήτων πρέπει να είναι η ίδια partition προκειμένου να τροποποιηθούν ως μια δέσμη. Το παρακάτω παράδειγμα προσθέτει δύο οντοτήτων μαζί σε μια δέσμη.

    from azure.storage.table import TableBatch
    batch = TableBatch()
    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    batch.insert_entity(task10)
    batch.insert_entity(task11)
    table_service.commit_batch('tasktable', batch)

Δέσμες μπορεί επίσης να χρησιμοποιηθεί με τη σύνταξη manager contex:

    task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
    task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

    with table_service.batch('tasktable') as batch:
        batch.insert_entity(task12)
        batch.insert_entity(task13)


## <a name="query-for-an-entity"></a>Ερώτημα για μια οντότητα

Για να το ερώτημα σε μια οντότητα σε έναν πίνακα, χρησιμοποιήστε το **γρήγορα\_οντότητα** μέθοδο, μεταφέροντας **PartitionKey** και **RowKey**.

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## <a name="query-a-set-of-entities"></a>Ένα σύνολο οντοτήτων ερωτήματος

Αυτό το παράδειγμα εντοπίζει όλες τις εργασίες στο Σιάτλ βάση **PartitionKey**.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## <a name="query-a-subset-of-entity-properties"></a>Ένα υποσύνολο οντότητα ιδιοτήτων ερωτήματος

Ένα ερώτημα σε έναν πίνακα μπορεί να ανακτήσει ιδιότητες μερικά μόνο από μια οντότητα.
Αυτήν την τεχνική, που ονομάζεται *Προβολή*, μειώνει το εύρος ζώνης και να βελτιώσετε τις επιδόσεις του ερωτήματος, ειδικά για μεγάλη οντοτήτων. Χρησιμοποιήστε την παράμετρο **Επιλέξτε** και μεταβιβάζουν τα ονόματα από τις ιδιότητες που θέλετε να εμφανιστεί επάνω από το πρόγραμμα-πελάτη.

Το ερώτημα στο ακόλουθο κώδικα επιστρέφει μόνο τις περιγραφές των οντοτήτων στον πίνακα.

[AZURE.NOTE] Το παρακάτω τμήμα κώδικα λειτουργεί μόνο σε σχέση με την υπηρεσία αποθήκευσης στο cloud. Αυτό δεν υποστηρίζεται από το χώρο αποθήκευσης προσομοίωσης.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
    for task in tasks:
        print(task.description)

## <a name="delete-an-entity"></a>Διαγραφή οντότητας

Μπορείτε να διαγράψετε μια οντότητα, χρησιμοποιώντας το κλειδί διαμερίσματα και της γραμμής.

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## <a name="delete-a-table"></a>Διαγραφή ενός πίνακα

Ο ακόλουθος κώδικας Διαγράφει έναν πίνακα από ένα λογαριασμό του χώρου αποθήκευσης.

    table_service.delete_table('tasktable')

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του πίνακα χώρου αποθήκευσης, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα.

- [Κέντρο για προγραμματιστές Python](/develop/python/)
- [Υπηρεσίες Azure αποθήκευσης REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης]
- [Χώρος αποθήκευσης Microsoft Azure SDK για Python]

[Azure Ιστολόγιο ομάδας χώρου αποθήκευσης]: http://blogs.msdn.com/b/windowsazurestorage/
[Χώρος αποθήκευσης Microsoft Azure SDK για Python]: https://github.com/Azure/azure-storage-python
