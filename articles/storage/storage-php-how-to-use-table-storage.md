<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων από PHP | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία πίνακα από PHP για να δημιουργήσετε και να διαγράψετε έναν πίνακα, εισαγωγή, διαγραφή και υποβολή ερωτήματος στον πίνακα."
    services="storage"
    documentationCenter="php"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-php"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων από PHP

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός δείχνει πώς μπορείτε να εκτελέσετε συνηθισμένα σενάρια με την υπηρεσία πίνακα Azure. Τα δείγματα είναι γραμμένες σε PHP και χρησιμοποιήστε το [Azure SDK για PHP][download]. Τα σενάρια καλύπτεται περιλαμβάνουν **τη δημιουργία και τη διαγραφή ενός πίνακα, και εισαγωγή, τη διαγραφή, και υποβολή ερωτημάτων οντοτήτων σε έναν πίνακα**. Για περισσότερες πληροφορίες σχετικά με την υπηρεσία πίνακα Azure, ανατρέξτε στην ενότητα [επόμενα βήματα](#next-steps) .

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Δημιουργία μιας εφαρμογής PHP

Η μόνη απαίτηση για τη δημιουργία μιας εφαρμογής PHP που αποκτά πρόσβαση στην υπηρεσία πίνακα Azure είναι η αναφορά σε κλάσεων στο Azure SDK για PHP από τον κωδικό. Μπορείτε να χρησιμοποιήσετε οποιαδήποτε εργαλεία ανάπτυξης για να δημιουργήσετε την εφαρμογή σας, όπως το Σημειωματάριο.

Σε αυτόν τον οδηγό, μπορείτε να χρησιμοποιήσετε δυνατότητες υπηρεσίας πίνακα, το οποίο μπορεί να ονομάζεται από μέσα σε μια εφαρμογή PHP τοπικά ή σε κώδικα που εκτελείται μέσα σε ένα ρόλο Azure web, ρόλο εργαζόμενου ή τοποθεσία Web.

## <a name="get-the-azure-client-libraries"></a>Λάβετε τις βιβλιοθήκες Azure προγράμματος-πελάτη

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Ρύθμιση παραμέτρων της εφαρμογής σας για να αποκτήσετε πρόσβαση στην υπηρεσία πίνακα

Για να χρησιμοποιήσετε την υπηρεσία πίνακα Azure APIs, πρέπει να:

1. Αναφορά στο αρχείο συσκευή αυτόματης φόρτωσης χρησιμοποιώντας το [require_once] [ require_once] πρόταση, και
2. Αναφορά οποιαδήποτε κλάσεις που μπορείτε να χρησιμοποιήσετε.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να συμπεριλάβετε το αρχείο συσκευή αυτόματης φόρτωσης και αναφορά την κλάση **ServicesBuilder** .

> [AZURE.NOTE] Αυτό το παράδειγμα (και άλλα παραδείγματα σε αυτό το άρθρο) θεωρείται ότι έχετε εγκαταστήσει τις βιβλιοθήκες PHP προγράμματος-πελάτη για Azure μέσω σύνθεσης. Εάν έχετε εγκαταστήσει τις βιβλιοθήκες με μη αυτόματο τρόπο, πρέπει να αναφέρονται τα <code>WindowsAzure.php</code> συσκευή αυτόματης φόρτωσης αρχείου.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


Στα παρακάτω παραδείγματα, η `require_once` δήλωση εμφανίζεται πάντα, αλλά μόνο τις κατηγορίες που είναι απαραίτητο για να εκτελέσει το παράδειγμα αναφέρονται.

## <a name="set-up-an-azure-storage-connection"></a>Ρύθμιση σύνδεσης Azure χώρου αποθήκευσης

Για να ξεκινήσει ένα πρόγραμμα-πελάτης υπηρεσίας πίνακα Azure, πρέπει πρώτα να έχετε μια έγκυρη συμβολοσειρά σύνδεσης. Η μορφή για τη συμβολοσειρά σύνδεσης υπηρεσίας πίνακα είναι:

Για να αποκτήσετε πρόσβαση σε μια ζωντανή υπηρεσία:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Για να αποκτήσετε πρόσβαση του χώρου αποθήκευσης προσομοίωσης:

    UseDevelopmentStorage=true


Για να δημιουργήσετε οποιονδήποτε υπολογιστή-πελάτη Azure υπηρεσία, πρέπει να χρησιμοποιήσετε την κλάση **ServicesBuilder** . Μπορείς:

* μεταβίβαση τη συμβολοσειρά σύνδεσης απευθείας σε αυτήν ή
* Χρησιμοποιήστε το **CloudConfigurationManager (ΚΦΜ)** για να ελέγξετε πολλές εξωτερικές προελεύσεις για τη συμβολοσειρά σύνδεσης:
    * από προεπιλογή, διατίθεται με την υποστήριξη για ένα εξωτερικό αρχείο προέλευσης - μεταβλητές περιβάλλοντος
    * Μπορείτε να προσθέσετε νέες προελεύσεις επεκτείνοντας την κλάση **ConnectionStringSource**

Για παραδείγματα που περιγράφονται εδώ, τη συμβολοσειρά σύνδεσης θα περάσουν απευθείας.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## <a name="create-a-table"></a>Δημιουργία πίνακα

Ένα αντικείμενο **TableRestProxy** σάς επιτρέπει να δημιουργήσετε έναν πίνακα με τη μέθοδο **createTable** . Κατά τη δημιουργία ενός πίνακα, μπορείτε να ορίσετε το χρονικό όριο υπηρεσίας πίνακα. (Για περισσότερες πληροφορίες σχετικά με το χρονικό όριο υπηρεσίας πίνακα, ανατρέξτε στο θέμα [Ρύθμιση χρονικών ορίων για λειτουργίες υπηρεσίας πίνακα][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

Για πληροφορίες σχετικά με τους περιορισμούς σχετικά με τα ονόματα πινάκων, ανατρέξτε στο θέμα [Κατανόηση του μοντέλου δεδομένων υπηρεσίας του πίνακα][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Προσθέστε μια οντότητα σε έναν πίνακα

Για να προσθέσετε μια οντότητα σε έναν πίνακα, δημιουργήστε ένα νέο αντικείμενο **οντότητα** και μεταβιβάζουν το **TableRestProxy -> insertEntity**. Σημειώστε ότι όταν δημιουργείτε μια οντότητα, πρέπει να καθορίσετε μια `PartitionKey` και `RowKey`. Αυτά είναι τα μοναδικά αναγνωριστικά για μια οντότητα και είναι τιμές που μπορούν να αναζητηθούν πολύ ταχύτερα από τις άλλες ιδιότητες οντότητα. Το σύστημα χρησιμοποιεί `PartitionKey` για την αυτόματη διανομή οντοτήτων του πίνακα επάνω από πολλούς κόμβους χώρου αποθήκευσης. Οντοτήτων με το ίδιο `PartitionKey` είναι αποθηκευμένα στον ίδιο κόμβο. (Εκτελούν λειτουργίες σε πολλών οντοτήτων που είναι αποθηκευμένα στον ίδιο κόμβο καλύτερη από στην οντοτήτων που είναι αποθηκευμένα σε διαφορετικούς κόμβους.) Η `RowKey` είναι το μοναδικό Αναγνωριστικό μιας οντότητας μέσα σε ένα διαμερίσματα.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

Για πληροφορίες σχετικά με τις ιδιότητες πίνακα και τύπων, ανατρέξτε στο θέμα [Κατανόηση του μοντέλου δεδομένων υπηρεσίας πίνακα][table-data-model].

Η κλάση **TableRestProxy** προσφέρει δύο εναλλακτικές μεθόδους για την εισαγωγή οντοτήτων: **insertOrMergeEntity** και **insertOrReplaceEntity**. Για να χρησιμοποιήσετε αυτές τις μεθόδους, δημιουργήστε μια νέα **οντότητα** και μεταβιβάζουν ως παράμετρο, είτε μέθοδο. Κάθε μέθοδο θα εισαγάγει η οντότητα, εάν δεν υπάρχει. Εάν υπάρχει ήδη η οντότητα, **insertOrMergeEntity** ενημέρωσης τιμές ιδιοτήτων, εάν υπάρχει ήδη, τις ιδιότητες και προσθέτει νέες ιδιότητες, εάν δεν υπάρχουν, ενώ **insertOrReplaceEntity** αντικαθιστά πλήρως μια υπάρχουσα οντότητα. Το παρακάτω παράδειγμα εμφανίζει τον τρόπο χρήσης του **insertOrMergeEntity**. Εάν η οντότητα με `PartitionKey` "tasksSeattle" και `RowKey` "1" δεν υπάρχει ήδη, θα εισαχθεί. Ωστόσο, εάν αυτό έχει ήδη εισαχθεί (όπως φαίνεται στο παραπάνω παράδειγμα), το `DueDate` ιδιότητα θα ενημερωθεί, και το `Status` ιδιότητα που θα προστεθούν. Το `Description` και `Location` ενημερώνονται επίσης ιδιότητες, αλλά με τιμές που αποτελεσματική παραμένουν αμετάβλητες. Εάν αυτά τα τελευταία δύο ιδιότητες δεν έχουν προστεθεί όπως φαίνεται στο παράδειγμα, αλλά υπήρχαν στην οντότητα προορισμού, θα διατηρήσετε αναλλοίωτο υπάρχουσες τιμές τους.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="retrieve-a-single-entity"></a>Ανάκτηση μία οντότητα

Η μέθοδος **TableRestProxy -> getEntity** σάς επιτρέπει να ανακτήσετε μια μοναδική οντότητα κατά την υποβολή ερωτημάτων για τα `PartitionKey` και `RowKey`. Στο παρακάτω παράδειγμα, το πλήκτρο διαμερίσματα `tasksSeattle` και κλειδί γραμμής `1` μεταβιβάζονται τη μέθοδο **getEntity** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## <a name="retrieve-all-entities-in-a-partition"></a>Ανάκτηση όλα οντοτήτων ένα διαμερίσματα

Ερωτήματα οντότητα έχουν συνταχθεί με φίλτρα (για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Υποβολή ερωτημάτων πίνακες και οντοτήτων][filters]). Για να ανακτήσετε όλους οντοτήτων διαμερίσματα, χρησιμοποιήστε το φίλτρο "PartitionKey eq *όνομα_διαμερίσματος*". Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να ανακτήσετε όλους οντοτήτων το `tasksSeattle` διαμερίσματα, μεταφέροντας ένα φίλτρο για τη μέθοδο **queryEntities** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Ανακτήστε ένα υποσύνολο των οντοτήτων ένα διαμερίσματα

Το ίδιο μοτίβο που χρησιμοποιούνται στο προηγούμενο παράδειγμα μπορεί να χρησιμοποιηθεί για την ανάκτηση οποιαδήποτε υποσύνολο των οντοτήτων ένα διαμερίσματα. Το υποσύνολο των οντοτήτων που μπορείτε να ανακτήσετε καθορίζονται από το φίλτρο που χρησιμοποιείτε (για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Υποβολή ερωτημάτων πίνακες και οντοτήτων][filters]). Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε ένα φίλτρο για να ανακτήσετε όλους οντοτήτων με ένα συγκεκριμένο `Location` και μια `DueDate` λιγότερο από μια συγκεκριμένη ημερομηνία.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entity-properties"></a>Ανακτήστε ένα υποσύνολο των ιδιοτήτων οντότητα

Ένα ερώτημα να ανακτήσετε ένα υποσύνολο των ιδιοτήτων οντότητα. Αυτήν την τεχνική, που ονομάζεται *Προβολή*, μειώνει το εύρος ζώνης και να βελτιώσετε τις επιδόσεις του ερωτήματος, ειδικά για μεγάλη οντοτήτων. Για να καθορίσετε μια ιδιότητα που θα ανακτηθούν, μεταβιβάζουν το όνομα της ιδιότητας για τη μέθοδο του **ερωτήματος -> addSelectField** . Μπορείτε να καλέσετε αυτήν τη μέθοδο πολλές φορές για να προσθέσετε περισσότερες ιδιότητες. Μετά την εκτέλεση **TableRestProxy -> queryEntities**, το επιστρεφόμενο οντοτήτων θα έχετε μόνο τις επιλεγμένες ιδιότητες. (Εάν θέλετε να επαναφέρετε ένα υποσύνολο των οντοτήτων πίνακα, χρησιμοποιήστε ένα φίλτρο όπως φαίνεται στα παραπάνω ερωτήματα.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## <a name="update-an-entity"></a>Ενημέρωση μιας οντότητας

Μια υπάρχουσα οντότητα μπορούν να ενημερωθούν χρησιμοποιώντας τις μεθόδους **οντότητα -> Ορισμός ιδιότητας** και **οντότητα -> addProperty** στην οντότητα και, στη συνέχεια, κλήση **TableRestProxy -> updateEntity**. Το παρακάτω παράδειγμα ανακτά μια οντότητα, τροποποιεί μία ιδιότητα, καταργεί μια άλλη ιδιότητα και προσθέτει μια νέα ιδιότητα. Σημειώστε ότι μπορείτε να καταργήσετε μια ιδιότητα, ορίζοντας την τιμή σε **null**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-an-entity"></a>Διαγραφή οντότητας

Για να διαγράψετε μια οντότητα, μεταβιβάζουν το όνομα του πίνακα και η οντότητα `PartitionKey` και `RowKey` τη μέθοδο **TableRestProxy -> deleteEntity** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Σημειώστε ότι οι επιταγές ταυτόχρονης εκτέλεσης, μπορείτε να ορίσετε το Etag για μια οντότητα να διαγραφεί χρησιμοποιώντας τη μέθοδο **DeleteEntityOptions -> setEtag** και προώθηση του αντικειμένου **DeleteEntityOptions** στον **deleteEntity** ως ένα τέταρτο παραμέτρου.

## <a name="batch-table-operations"></a>Λειτουργίες πίνακα δέσμης

Η μέθοδος **TableRestProxy -> δέσμη** σάς επιτρέπει να εκτελεί λειτουργίες πολλών σε μία μόνο αίτηση. Δείτε το μοτίβο περιλαμβάνει την προσθήκη λειτουργίες **BatchRequest** αντικείμενο και, στη συνέχεια, που περνά μέσα στο αντικείμενο **BatchRequest** τη μέθοδο **TableRestProxy -> δέσμης** . Για να προσθέσετε μια πράξη σε ένα αντικείμενο **BatchRequest** , μπορείτε να καλέσετε οποιαδήποτε από τις παρακάτω μεθόδους πολλές φορές:

* **addInsertEntity** (προσθέτει μια λειτουργία insertEntity)
* **addUpdateEntity** (προσθέτει μια λειτουργία updateEntity)
* **addMergeEntity** (προσθέτει μια λειτουργία mergeEntity)
* **addInsertOrReplaceEntity** (προσθέτει μια λειτουργία insertOrReplaceEntity)
* **addInsertOrMergeEntity** (προσθέτει μια λειτουργία insertOrMergeEntity)
* **addDeleteEntity** (προσθέτει μια λειτουργία deleteEntity)

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να εκτελεί λειτουργίες **insertEntity** και **deleteEntity** σε μία μόνο αίτηση:

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;
    use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Για περισσότερες πληροφορίες σχετικά με τη δέσμη λειτουργίες πίνακα, ανατρέξτε στο θέμα [Εκτέλεση συναλλαγές ομάδα οντότητα][entity-group-transactions].

## <a name="delete-a-table"></a>Διαγραφή ενός πίνακα

Τέλος, για να διαγράψετε έναν πίνακα, μεταβιβάζουν το όνομα του πίνακα για τη μέθοδο **TableRestProxy -> Διαγραφή πίνακα** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία της υπηρεσίας πίνακα Azure, ακολουθήστε αυτές τις συνδέσεις για να μάθετε σχετικά με τις πιο σύνθετες εργασίες χώρου αποθήκευσης.

- Επισκεφθείτε το [Ιστολόγιο ομάδας αποθήκευσης Azure](http://blogs.msdn.com/b/windowsazurestorage/)

Για περισσότερες πληροφορίες, ανατρέξτε επίσης στο [Κέντρο για προγραμματιστές PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
