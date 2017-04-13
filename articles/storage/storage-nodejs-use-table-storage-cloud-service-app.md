<properties 
    pageTitle="Εφαρμογή Web με το χώρο αποθήκευσης πινάκων (Node.js) | Microsoft Azure" 
    description="Ένα πρόγραμμα εκμάθησης που βασίζεται στην εφαρμογή Web με το πρόγραμμα εκμάθησης Express με προσθήκη υπηρεσιών Azure χώρου αποθήκευσης και τη λειτουργική μονάδα Azure." 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robmcm"/>

# <a name="nodejs-web-application-using-storage"></a>Node.js εφαρμογής Web με χρήση του χώρου αποθήκευσης

## <a name="overview"></a>Επισκόπηση

Σε αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να επεκτείνετε την εφαρμογή που έχει δημιουργηθεί με το πρόγραμμα εκμάθησης [Node.js εφαρμογής Web με χρήση Express] , χρησιμοποιώντας τις βιβλιοθήκες Microsoft Azure προγράμματος-πελάτη για Node.js για να εργαστείτε με τις υπηρεσίες διαχείρισης δεδομένων. Θα μπορείτε να επεκτείνετε την εφαρμογή σας για να δημιουργήσετε μια λίστα εργασιών που βασίζεται στο web εφαρμογή που μπορείτε να αναπτύξετε σε Azure. Η λίστα εργασιών επιτρέπει στο χρήστη να ανακτήσετε εργασίες, να προσθέσετε νέες εργασίες και επισημάνετε εργασίες ως ολοκληρωμένη.

Τα στοιχεία εργασίας είναι αποθηκευμένα στο χώρο αποθήκευσης Azure. Azure αποθήκευσης παρέχει χώρο αποθήκευσης μη δομημένα δεδομένα που είναι ανοχή και ιδιαίτερα διαθέσιμη. Azure αποθήκευσης περιλαμβάνει πολλές δομές δεδομένων, όπου μπορείτε να αποθηκεύσετε και πρόσβαση σε δεδομένα και μπορείτε να αξιοποιήσετε τις υπηρεσίες αποθήκευσης από τα περιλαμβάνονται στο Azure SDK για Node.js ή μέσω REST APIs API. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αποθήκευση και πρόσβαση σε δεδομένα στο Azure].

Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ολοκληρώσει την [Εφαρμογή Web Node.js] [Node.js χωρίς ρητή][Node.js εφαρμογής Web με χρήση Express] προγράμματα εκμάθησης και.

Θα μάθετε:

-   Πώς μπορείτε να εργαστείτε με το μηχανισμό Jade προτύπου
-   Πώς μπορείτε να εργαστείτε με τις υπηρεσίες διαχείρισης δεδομένων Azure

Στιγμιότυπο οθόνης της εφαρμογής ολοκληρωμένη είναι κάτω από:

![Το ολοκληρωμένο ιστοσελίδας στον internet explorer](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Ρύθμιση χώρου αποθήκευσης διαπιστευτηρίων στο Web.Config

Για να αποκτήσετε πρόσβαση στο χώρο αποθήκευσης Azure, πρέπει να μεταβιβάζουν στο χώρο αποθήκευσης διαπιστευτηρίων. Για να το κάνετε αυτό, μπορείτε να χρησιμοποιήσουν ρυθμίσεις εφαρμογής web.config.
Αυτές οι ρυθμίσεις θα περάσουν ως μεταβλητές περιβάλλοντος σε κόμβο, που είναι για ανάγνωση, στη συνέχεια, από το Azure SDK.

> [AZURE.NOTE] Χώρος αποθήκευσης διαπιστευτηρίων χρησιμοποιούνται μόνο όταν η εφαρμογή έχει αναπτυχθεί σε Azure. Όταν εκτελείται στο το προσομοίωσης, η εφαρμογή θα χρησιμοποιεί την προσομοίωσης χώρου αποθήκευσης.

Ακολουθήστε τα παρακάτω βήματα για να ανακτήσετε τα διαπιστευτήρια λογαριασμού χώρου αποθήκευσης και προσθέστε τους στις ρυθμίσεις web.config:

1.  Εάν δεν είναι ήδη άνοιγμα, ξεκινήστε το PowerShell Azure από το μενού **Έναρξη** , επεκτείνοντας **όλα τα προγράμματα, Azure**, κάντε δεξί κλικ **Azure PowerShell**και, στη συνέχεια, επιλέξτε **Εκτέλεση ως διαχειριστής**.

2.  Αλλαγή σε καταλόγους στο φάκελο που περιέχει την εφαρμογή σας. Για παράδειγμα, C:\\κόμβου\\tasklist\\WebRole1.

3.  Από το παράθυρο Azure Powershell, εισαγάγετε το ακόλουθο cmdlet για να ανακτήσετε τις πληροφορίες λογαριασμού χώρου αποθήκευσης:

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    Αυτό ανακτά τη λίστα λογαριασμών χώρου αποθήκευσης και λογαριασμό κλειδιά που σχετίζεται με την υπηρεσία.

    > [AZURE.NOTE] Επειδή το SDK Azure δημιουργεί ένα λογαριασμό του χώρου αποθήκευσης κατά την ανάπτυξη μιας υπηρεσίας, ένα λογαριασμό του χώρου αποθήκευσης θα πρέπει να υπάρχει ήδη, από την εφαρμογή σας με το προηγούμενο οδηγούς για την ανάπτυξη.

4.  Ανοίξτε το αρχείο **ServiceDefinition.csdef** που περιέχει τις ρυθμίσεις περιβάλλοντος που χρησιμοποιούνται όταν η εφαρμογή έχει αναπτυχθεί σε Azure:

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  Εισαγάγετε το ακόλουθο μπλοκ στην περιοχή στοιχείο **περιβάλλον** , αντικαθιστώντας τα {ΛΟΓΑΡΙΑΣΜΟΎ χώρου ΑΠΟΘΉΚΕΥΣΗΣ} και {ΠΛΉΚΤΡΟ ΠΡΌΣΒΑΣΗΣ ΑΠΟΘΉΚΕΥΣΗΣ} με το όνομα λογαριασμού και το πρωτεύον κλειδί για το λογαριασμό χώρου αποθήκευσης που θέλετε να χρησιμοποιήσετε για την ανάπτυξη:

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![Τα περιεχόμενα του αρχείου web.cloud.config](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  Αποθηκεύστε το αρχείο και κλείστε το Σημειωματάριο.

### <a name="install-additional-modules"></a>Εγκατάσταση πρόσθετες λειτουργικές μονάδες

2. Χρησιμοποιήστε την ακόλουθη εντολή για να εγκαταστήσετε το [azure], [κόμβου-uuid], [nconf] και [ασύγχρονης] λειτουργικές μονάδες τοπικά επίσης ως, για να αποθηκεύσετε μια εγγραφή για τους στο αρχείο **package.json** :

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    Το αποτέλεσμα αυτής της εντολής πρέπει να είναι παρόμοια με το εξής:

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##<a name="using-the-table-service-in-a-node-application"></a>Με την υπηρεσία πίνακα σε μια εφαρμογή κόμβου

Σε αυτήν την ενότητα θα μπορείτε να επεκτείνετε την βασική εφαρμογή δημιουργήθηκε από την εντολή **express** , προσθέτοντας ένα αρχείο **task.js** , το οποίο περιέχει το μοντέλο για τις εργασίες σας. Επίσης θα την υπάρχουσα **app.js** τροποποίηση και δημιουργήστε ένα νέο αρχείο **tasklist.js** που χρησιμοποιεί το μοντέλο.

### <a name="create-the-model"></a>Δημιουργία του μοντέλου

1. Στον κατάλογο **WebRole1** , δημιουργήστε έναν νέο κατάλογο που ονομάζεται **μοντέλα**.

2. Στον κατάλογο **μοντέλα** , δημιουργήστε ένα νέο αρχείο με το όνομα **task.js**. Αυτό το αρχείο θα περιέχει το μοντέλο για τις εργασίες που δημιουργήθηκε από την εφαρμογή σας.

3. Στην αρχή του αρχείου **task.js** , προσθέστε τον ακόλουθο κώδικα για αναφορά απαιτούμενες βιβλιοθήκες:

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Στη συνέχεια, θα μπορείτε να προσθέσετε κώδικα για να ορίσετε και να εξαγάγετε το αντικείμενο εργασίας. Αυτό το αντικείμενο είναι υπεύθυνη για τη σύνδεση με τον πίνακα.

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Στη συνέχεια, προσθέστε τον ακόλουθο κώδικα για να ορίσετε πρόσθετες μεθόδους στο αντικείμενο εργασίας, που επιτρέπουν αλληλεπιδράσεις με δεδομένα που είναι αποθηκευμένα στον πίνακα:

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };

            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. Αποθηκεύστε και κλείστε το αρχείο **task.js** .

### <a name="create-the-controller"></a>Δημιουργία του ελεγκτή

1. Στον κατάλογο **WebRole1/διαδρομές** , δημιουργήστε ένα νέο αρχείο με το όνομα **tasklist.js** και ανοίξτε το σε ένα πρόγραμμα επεξεργασίας κειμένου.

2. Προσθέστε τον παρακάτω κώδικα **tasklist.js**. Αυτό φορτώνει το azure και ασύγχρονης λειτουργικές μονάδες, που χρησιμοποιούνται από **tasklist.js**. Αυτό ορίζει, επίσης, η συνάρτηση **TaskList** , η οποία μεταβιβάζεται μια παρουσία του αντικειμένου **εργασίας** ορίσαμε παλαιότερη έκδοση:

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. Συνεχίστε να προσθέτετε στο αρχείο **tasklist.js** , προσθέτοντας τις μεθόδους που χρησιμοποιούνται για **showTasks**, **addTask**και **completeTasks**:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this      
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

3. Αποθηκεύστε το αρχείο **tasklist.js** .

### <a name="modify-appjs"></a>Τροποποίηση app.js

1. Στον κατάλογο **WebRole1** , ανοίξτε το αρχείο **app.js** σε έναν επεξεργαστή κειμένου. 

2. Στην αρχή του αρχείου, προσθέστε τα ακόλουθα για να φορτώσετε τη λειτουργική μονάδα azure και να ορίσετε το κλειδί όνομα και διαμερίσματα πίνακα:

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. Στο αρχείο app.js, μεταβείτε με κύλιση προς τα κάτω, όπου μπορείτε να δείτε την ακόλουθη γραμμή:

        app.use('/', routes);
        app.use('/users', users);

    Αντικαταστήστε τις παραπάνω γραμμές με τον κώδικα που φαίνεται παρακάτω. Αυτό θα προετοιμασία της παρουσίας της <strong>εργασίας</strong> με μια σύνδεση με το λογαριασμό χώρου αποθήκευσης. Αυτό που του μεταβιβάστηκε η <strong>TaskList</strong>, που θα χρησιμοποιήσετε για να επικοινωνήσετε με την υπηρεσία πίνακα:

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. Αποθηκεύστε το αρχείο **app.js** .

### <a name="modify-the-index-view"></a>Τροποποίηση της προβολής ευρετηρίου

1. Αλλαγή σε καταλόγους στον κατάλογο **προβολές** και ανοίξτε το αρχείο **index.jade** σε ένα πρόγραμμα επεξεργασίας κειμένου.

2. Αντικαταστήστε τα περιεχόμενα του αρχείου **index.jade** με τον παρακάτω κώδικα. Αυτό ορίζει την προβολή για την εμφάνιση υπάρχουσες εργασίες, καθώς και μια φόρμα για την προσθήκη νέων εργασιών και τη σήμανση υπάρχουσες ως ολοκληρωμένη.

        extends layout

        block content
          h1= title
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if tasks != []
                tr
                  td 
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name: 
            input(name="item[name]", type="textbox")
            label Item Category: 
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. Αποθηκεύστε και κλείστε το αρχείο **index.jade** .

### <a name="modify-the-global-layout"></a>Τροποποιήστε τη διάταξη καθολικού

Το αρχείο **layout.jade** στον κατάλογο **προβολές** χρησιμοποιείται ως καθολικό πρότυπο για άλλα αρχεία **.jade** . Σε αυτό το βήμα θα τροποποιήσετε ώστε να χρησιμοποιεί [Εκκίνησης του Twitter](https://github.com/twbs/bootstrap), που είναι ένα κιτ εργαλείων που διευκολύνει την σχεδίαση ωραία εμφάνιση τοποθεσίας Web.

1. Κάντε λήψη και να εξαγάγετε τα αρχεία για [Εκκίνηση του Twitter](http://getbootstrap.com/). Αντιγράψτε το αρχείο **bootstrap.min.css** από το **εκκίνησης\\dist\\css** φάκελο για να το **δημόσια\\φύλλα στυλ** καταλόγου της εφαρμογής σας tasklist.

2. Από το φάκελο **προβολές** , ανοίξτε το **layout.jade** στο πρόγραμμα επεξεργασίας κειμένου και αντικαταστήστε τα περιεχόμενα με τα εξής:

        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body.app
            nav.navbar.navbar-default
              div.navbar-header
                a.navbar-brand(href='/') My Tasks
            block content

3. Αποθηκεύστε το αρχείο **layout.jade** .

### <a name="running-the-application-in-the-emulator"></a>Εκτέλεση της εφαρμογής στο το προσομοίωσης

Χρησιμοποιήστε την παρακάτω εντολή για να ξεκινήσετε την εφαρμογή σε το προσομοίωσης.

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

Το πρόγραμμα περιήγησης θα ανοίξει και εμφανίζει τη σελίδα παρακάτω:

![Μια τοποθεσία web σελίδων με τίτλο μου λίστας εργασιών με έναν πίνακα που περιέχει εργασίες και τα πεδία για να προσθέσετε μια νέα εργασία.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

Χρησιμοποιήστε τη φόρμα για να προσθέσετε στοιχεία ή να καταργήσετε υπάρχοντα στοιχεία, επισημαίνοντάς τις ως ολοκληρωμένη.

## <a name="publishing-the-application-to-azure"></a>Δημοσίευση της εφαρμογής σε Azure


Στο παράθυρο του Windows PowerShell, καλέστε το ακόλουθο cmdlet για να αναπτύξετε εκ νέου την υπηρεσία για να Azure.

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

Αντικαταστήστε **myuniquename** με ένα μοναδικό όνομα για αυτήν την εφαρμογή. Αντικαταστήστε **datacentername** με το όνομα του ένα κέντρο δεδομένων Azure, όπως **Δυτική ΗΠΑ**.

Μετά την ολοκλήρωση της ανάπτυξης, θα πρέπει να βλέπετε απόκριση παρόμοια με τα εξής:

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

Όπως και πριν, επειδή έχετε καθορίσει τη **-Εκκίνηση** την επιλογή, το πρόγραμμα περιήγησης ανοίγει και εμφανίζει την εφαρμογή που εκτελείται στο Azure όταν ολοκληρωθεί η δημοσίευση.

![Ένα παράθυρο προγράμματος περιήγησης που εμφανίζει τη σελίδα μου στη λίστα εργασιών. Η διεύθυνση URL υποδεικνύει τη σελίδα είναι τώρα φιλοξενούνται στον Azure.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Διακοπή και διαγράφοντας την εφαρμογή σας

Μετά την ανάπτυξη της εφαρμογής σας, μπορεί να θέλετε να το απενεργοποιήσετε, ώστε να μπορείτε να αποφύγετε το κόστος ή να δημιουργήσετε και να αναπτύξετε άλλες εφαρμογές εντός της δωρεάν δοκιμαστικής περιόδου.

Λογαριασμοί Azure web ρόλο παρουσίες ανά ώρα διακομιστή χρόνο που καταναλώθηκε.
Χρόνος διακομιστή καταναλώνεται όταν έχει αναπτυχθεί την εφαρμογή σας, ακόμα και αν οι εμφανίσεις δεν λειτουργούν και βρίσκονται σε κατάσταση διακοπής.

Τα παρακάτω βήματα που δείχνουν τον τρόπο για να διακόψετε και να διαγράψετε την εφαρμογή σας.

1.  Στο παράθυρο του Windows PowerShell, διακόψετε την ανάπτυξη υπηρεσίας που δημιουργήσατε στην προηγούμενη ενότητα με το ακόλουθο cmdlet:

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    Διακοπή της υπηρεσίας ενδέχεται να χρειαστούν αρκετά λεπτά. Όταν η υπηρεσία έχει διακοπεί, λαμβάνετε ένα μήνυμα που δηλώνει ότι έχει διακοπεί.

3.  Για να διαγράψετε την υπηρεσία, καλέστε το ακόλουθο cmdlet:

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    Όταν σας ζητηθεί, πληκτρολογήστε **Y** για να διαγράψετε την υπηρεσία.

    Διαγραφή της υπηρεσίας ενδέχεται να χρειαστούν αρκετά λεπτά. Μετά τη διαγραφή της υπηρεσίας λαμβάνετε ένα μήνυμα που δηλώνει ότι η υπηρεσία έχει διαγραφεί.

  [Node.js εφαρμογής Web με χρήση Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [Αποθήκευση και την πρόσβαση σε δεδομένα στο Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Εφαρμογή node.js Web]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 
