<properties 
    pageTitle="Δημιουργία και διαχείριση εργασιών ελαστικότητας βάσης δεδομένων με χρήση του PowerShell" 
    description="PowerShell που χρησιμοποιούνται για τη Διαχείριση συνόλων βάση δεδομένων SQL Azure" 
    services="sql-database" documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="create-and-manage-a-sql-database-elastic-database-jobs-using-powershell-preview"></a>Δημιουργήστε και διαχειριστείτε τις εργασίες μια βάση δεδομένων SQL ελαστικότητας βάσης δεδομένων με χρήση του PowerShell (έκδοση preview)

> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)



Το API του PowerShell για **εργασίες ελαστικότητας βάσης δεδομένων** (στην έκδοση preview), σας επιτρέπουν να ορίσετε μια ομάδα βάσεων δεδομένων βάσει του οποίου θα εκτελεστεί δεσμών ενεργειών. Σε αυτό το άρθρο δείχνει πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε τις **εργασίες ελαστικότητας βάσης δεδομένων** με χρήση των cmdlet του PowerShell. Ανατρέξτε στο θέμα [Επισκόπηση ελαστικότητας εργασίες](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
* Μια συνδρομή του Azure. Για μια δωρεάν δοκιμαστική έκδοση, ανατρέξτε στο θέμα [δωρεάν δοκιμαστική έκδοση ενός μήνα](https://azure.microsoft.com/pricing/free-trial/).
* Ένα σύνολο βάσεις δεδομένων που δημιουργήθηκαν με τα εργαλεία ελαστικότητας βάσης δεδομένων. Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με εργαλεία ελαστικότητας βάσης δεδομένων](sql-database-elastic-scale-get-started.md).
* Azure PowerShell. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).
* **Εργασίες ελαστικότητας βάσης δεδομένων** Το πακέτο του PowerShell: ανατρέξτε στο θέμα [εργασίες κατά την εγκατάσταση ελαστικότητας βάσης δεδομένων](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Επιλέξτε τη συνδρομή σας στο Azure

Για να επιλέξετε τη συνδρομή που χρειάζεστε σας συνδρομή αναγνωριστικό (**-SubscriptionId**) ή συνδρομή όνομα (**-SubscriptionName**). Εάν έχετε πολλές συνδρομές, μπορείτε να εκτελέσετε το cmdlet **Get-AzureRmSubscription** και να αντιγράψετε πληροφορίες για τη συνδρομή που θέλετε από το σύνολο των αποτελεσμάτων. Όταν έχετε τις πληροφορίες τη συνδρομή σας, εκτελέστε την παρακάτω commandlet για να ορίσετε αυτήν την εγγραφή ως την προεπιλεγμένη, δηλαδή προορισμού για τη δημιουργία και διαχείριση εργασιών:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

Το [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) προτείνεται για χρήση για να αναπτύξετε και να εκτελέσει δέσμες ενεργειών PowerShell σε σχέση με τις εργασίες ελαστικότητας βάσης δεδομένων.

## <a name="elastic-database-jobs-objects"></a>Ελαστικά αντικείμενα εργασίες της βάσης δεδομένων

Ο παρακάτω πίνακας παραθέτει ανάληψη όλους τους αντικείμενο τύπους εργασιών **Ελαστικότητας βάσης δεδομένων** μαζί με την περιγραφή και το σχετικό API του Yammer PowerShell.

<table style="width:100%">
  <tr>
    <th>Τύπος αντικειμένου</th>
    <th>Περιγραφή</th>
    <th>Σχετικές APIs PowerShell</th>
  </tr>
  <tr>
    <td>Διαπιστευτήρια</td>
    <td>Όνομα χρήστη και τον κωδικό πρόσβασης για να χρησιμοποιήσετε κατά τη σύνδεση με βάσεις δεδομένων για την εκτέλεση των δεσμών ενεργειών ή την εφαρμογή της DACPACs. <p>Ο κωδικός πρόσβασης κρυπτογραφούνται πριν από την αποστολή και την αποθήκευση της βάσης δεδομένων ελαστικότητας εργασίες βάσης δεδομένων.  Ο κωδικός πρόσβασης είναι αποκρυπτογράφηση από την υπηρεσία ελαστικότητας εργασίες βάση δεδομένων μέσω των διαπιστευτηρίων δημιουργούνται και που έχουν αποσταλεί από τη δέσμη ενεργειών εγκατάστασης.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Νέα AzureSqlJobCredential</p><p>Ορισμός AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Δέσμη ενεργειών</td>
    <td>Δέσμη ενεργειών Transact-SQL που θα χρησιμοποιηθεί για εκτέλεση σε βάσεις δεδομένων.  Θα πρέπει να είναι η σύνταξη τη δέσμη ενεργειών για να idempotent, επειδή η υπηρεσία θα επανάληψη εκτέλεσης της δέσμης ενεργειών με αποτυχίες.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Νέα AzureSqlJobContent</p>
    <p>Ορισμός AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Εφαρμογή δεδομένων επιπέδων </a> πακέτου για να εφαρμοστεί σε βάσεις δεδομένων.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Νέα AzureSqlJobContent</p>
    <p>Ορισμός AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Βάση δεδομένων προορισμού</td>
    <td>Βάση δεδομένων και ένα όνομα διακομιστή που δείχνει σε μια βάση δεδομένων SQL Azure.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Νέα AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Shard χάρτη προορισμού</td>
    <td>Συνδυασμός μια βάση δεδομένων προορισμού και μια διαπιστευτηρίων που θα χρησιμοποιηθεί για τον προσδιορισμό πληροφοριών που είναι αποθηκευμένες σε ένα χάρτη shard ελαστικά βάσης δεδομένων.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Νέα AzureSqlJobTarget</p>
    <p>Ορισμός AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Προσαρμοσμένη συλλογή προορισμού</td>
    <td>Καθορισμένο ομάδα βάσεις δεδομένων για να χρησιμοποιήσετε αποτελούν συνολικά για εκτέλεση.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Νέα AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Προσαρμοσμένη συλλογή θυγατρικών προορισμού</td>
    <td>Βάση δεδομένων προορισμού στα οποία γίνεται αναφορά από μια προσαρμοσμένη συλλογή.</td>
    <td>
    <p>Προσθήκη AzureSqlJobChildTarget</p>
    <p>Κατάργηση AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Εργασία</td>
    <td>
    <p>Ορισμός των παραμέτρων για ένα έργο που μπορούν να χρησιμοποιηθούν για την ενεργοποίηση εκτέλεσης ή για να ικανοποιεί ένα χρονοδιάγραμμα.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Νέα AzureSqlJob</p>
    <p>Ορισμός AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Εκτέλεση εργασίας</td>
    <td>
    <p>Κοντέινερ των εργασιών που είναι απαραίτητες για την κάλυψη είτε εκτέλεση μια δέσμη ενεργειών ή η εφαρμογή μιας DACPAC σε έναν προορισμό χρησιμοποιώντας τα διαπιστευτήρια για τις συνδέσεις βάσης δεδομένων με αποτυχίες χειρισμού σύμφωνα με μια πολιτική εκτέλεσης.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Έναρξη-AzureSqlJobExecution</p>
    <p>Διακοπή AzureSqlJobExecution</p>
    <p>AzureSqlJobExecution αναμονής</p>
  </tr>

<tr>
    <td>Εκτέλεση εργασιών έργου</td>
    <td>
    <p>Απλή μονάδα εργασίας για την κάλυψη μιας εργασίας.</p>
    <p>Εάν μια εργασία δεν είναι δυνατό να με επιτυχία εκτέλεση, το μήνυμα εξαίρεσης που προκύπτει θα καταγραφούν και μια νέα εργασία που ταιριάζει θα δημιουργηθεί και θα εκτελεστεί σύμφωνα με την πολιτική καθορισμένο εκτέλεσης.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Έναρξη-AzureSqlJobExecution</p>
    <p>Διακοπή AzureSqlJobExecution</p>
    <p>AzureSqlJobExecution αναμονής</p>
  </tr>

<tr>
    <td>Πολιτική εκτέλεσης εργασίας</td>
    <td>
    <p>Στοιχεία ελέγχου εργασία χρονικών ορίων εκτέλεσης, "Επανάληψη" όρια και χρονικά διαστήματα μεταξύ των επαναλήψεων.</p>
    <p>Ελαστικά εργασίες βάσης δεδομένων περιλαμβάνει μια προεπιλεγμένη πολιτική εκτέλεσης εργασία που προκαλούν ουσιαστικά άπειρο επαναλήψεις αποτυχιών εργασίας έργου με εκθετική διπλασιασμών διαστημάτων μεταξύ κάθε "Επανάληψη".</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Νέα AzureSqlJobExecutionPolicy</p>
    <p>Ορισμός AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Χρονοδιάγραμμα</td>
    <td>
    <p>Ώρας που βασίζονται σε προδιαγραφές για εκτέλεση για να αναλάβει είτε σε μια επαναλαμβανόμενη εργασία διάστημα είτε τη μία φορά.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Νέα AzureSqlJobSchedule</p>
    <p>Ορισμός AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Εργασία εναυσμάτων</td>
    <td>
    <p>Αντιστοίχιση μεταξύ μιας εργασίας και ένα χρονοδιάγραμμα για να έναυσμα εκτέλεσης εργασία σύμφωνα με το χρονοδιάγραμμα.</p>
    </td>
    <td>
    <p>Νέα AzureSqlJobTrigger</p>
    <p>Κατάργηση AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Τύποι ομαδοποίηση υποστηριζόμενες εργασίες ελαστικότητας βάσης δεδομένων
Η εργασία εκτελεί δέσμες ενεργειών Transact-SQL (T-SQL) ή την εφαρμογή της DACPACs σε μια ομάδα βάσεων δεδομένων. Κατά την υποβολή μιας εργασίας που θα εκτελεστεί σε μια ομάδα βάσεων δεδομένων, η εργασία "ανάπτυξη" του στο θυγατρικό εργασιών όπου κάθε εκτελεί την εκτέλεση ζητήθηκε σε σχέση με μια βάση δεδομένων μία μόνο στην ομάδα. 
 
Υπάρχουν δύο τύποι ομάδων που μπορείτε να δημιουργήσετε: 

* Ομάδα [Shard χάρτης](sql-database-elastic-scale-shard-map-management.md) : κατά την υποβολή μιας εργασίας για να στοχεύετε σε ένα χάρτη shard, τα ερωτήματα του χάρτη shard για να προσδιορίσετε το τρέχον σύνολο της shards η εργασία και, στη συνέχεια, δημιουργεί θυγατρικό εργασιών για κάθε shard στο χάρτη shard.
* Προσαρμοσμένη ομάδα συλλογής: μια προσαρμοσμένη που ορίζονται από το σύνολο των βάσεων δεδομένων. Όταν μια εργασία ως προορισμό μια προσαρμοσμένη συλλογή, δημιουργεί θυγατρικό εργασίες για κάθε βάση δεδομένων αυτήν τη στιγμή της προσαρμοσμένης συλλογής.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Για να ορίσετε τη βάση δεδομένων ελαστικότητας των εργασιών που σύνδεσης

Πρέπει να οριστεί με τα έργα *βάση δεδομένων ελέγχου* πριν από τη χρήση των εργασιών APIs σύνδεσης. Εκτελεί αυτό το cmdlet ενεργοποιεί ένα παράθυρο διαπιστευτηρίων για αναδύεται κάθε αίτηση το όνομα χρήστη και κωδικό πρόσβασης που δημιουργήσατε κατά την εγκατάσταση του εργασίες ελαστικότητας βάσης δεδομένων. Όλα τα παραδείγματα που παρέχονται μέσα σε αυτό το θέμα, ας υποθέσουμε ότι αυτό το πρώτο βήμα έχει ήδη πραγματοποιηθεί.

Ανοίξτε μια σύνδεση για τα έργα ελαστικότητας βάσης δεδομένων:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Κρυπτογραφημένο διαπιστευτήρια εντός των εργασιών ελαστικότητας βάσης δεδομένων

Διαπιστευτήρια βάσης δεδομένων μπορεί να εισαχθεί σε το εργασιών *ελέγχου βάσης δεδομένων* με τον κωδικό πρόσβασης του κρυπτογραφημένα. Είναι απαραίτητο να αποθηκεύει τα διαπιστευτήρια για να ενεργοποιήσετε τις εργασίες που θα εκτελεστεί αργότερα, (χρησιμοποιώντας χρονοδιαγράμματα εργασιών).
 
Κρυπτογράφηση λειτουργεί μέσω ένα πιστοποιητικό που δημιουργήσατε ως μέρος της δέσμης ενεργειών εγκατάστασης. Η δέσμη ενεργειών εγκατάστασης δημιουργεί και αποστέλλει το πιστοποιητικό στην υπηρεσία Cloud Azure για αποκρυπτογράφηση την αποθηκευμένη κρυπτογραφημένων κωδικών πρόσβασης. Η υπηρεσία Cloud Azure αποθηκεύει αργότερα το δημόσιο κλειδί εντός του εργασιών *ελέγχου βάσης δεδομένων* που σας επιτρέπει το περιβάλλον API του PowerShell ή Azure κλασική πύλη για να κρυπτογραφήσετε έναν κωδικό πρόσβασης που παρέχεται χωρίς να απαιτείται το πιστοποιητικό για να έχει εγκατασταθεί τοπικά.
 
Οι κωδικοί πρόσβασης διαπιστευτηρίων είναι κρυπτογραφημένη και ασφαλής από τους χρήστες με πρόσβαση μόνο για ανάγνωση σε αντικείμενα εργασίες ελαστικότητας βάσης δεδομένων. Αλλά είναι δυνατή για κακόβουλο χρήστη με πρόσβαση ανάγνωσης / εγγραφής σε αντικείμενα ελαστικότητας εργασίες βάσης δεδομένων για να εξαγάγετε έναν κωδικό πρόσβασης. Τα διαπιστευτήρια είναι σχεδιασμένες για να χρησιμοποιηθεί ξανά σε εργασία εκτελέσεις. Διαπιστευτήρια μεταβιβάζονται βάσεις δεδομένων προορισμού κατά τη δημιουργία συνδέσεων. Αυτήν τη στιγμή υπάρχουν περιορισμοί για τις βάσεις δεδομένων προορισμού που χρησιμοποιούνται για κάθε διαπιστευτήρια, κακόβουλος χρήστης να προσθέσει μια βάση δεδομένων προορισμού για μια βάση δεδομένων στην περιοχή έλεγχος του κακόβουλο χρήστη. Ο χρήστης θα μπορούσε να αργότερα να ξεκινήσει μια εργασία στόχευσης αυτήν τη βάση δεδομένων για να αποκτήσουν τα διαπιστευτήρια τον κωδικό πρόσβασης.

Βέλτιστες πρακτικές ασφαλείας για τα έργα ελαστικότητας βάσης δεδομένων περιλαμβάνουν τα εξής:

* Περιορίστε τη χρήση των API σε αξιόπιστα άτομα.
* Τα διαπιστευτήρια πρέπει να έχει τα ελάχιστα δικαιώματα που είναι απαραίτητο να εκτελέσετε την εργασία.  Περισσότερες πληροφορίες μπορούν να προβληθούν μέσα σε αυτό άρθρο SQL Server MSDN [εξουσιοδότηση και δικαιώματα](https://msdn.microsoft.com/library/bb669084.aspx) .

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Για να δημιουργήσετε ένα κρυπτογραφημένο διαπιστευτήρια για εκτέλεση εργασίας σε βάσεις δεδομένων

Για να δημιουργήσετε ένα νέο κρυπτογραφημένο διαπιστευτηρίων, το [**cmdlet Get-διαπιστευτηρίων**](https://technet.microsoft.com/library/hh849815.aspx) σας ζητά ένα όνομα χρήστη και τον κωδικό πρόσβασης που μπορούν να περάσουν το [**cmdlet New-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346063.aspx).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Για να ενημερώσετε τα διαπιστευτήρια

Όταν αλλάξετε τους κωδικούς πρόσβασης, χρησιμοποιήστε το [**cmdlet Set-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346062.aspx) και ορίστε την παράμετρο **CredentialName** .

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Για να ορίσετε μια προορισμού χάρτη shard ελαστικά βάσης δεδομένων

Για να εκτελέσετε μια εργασία σε σχέση με όλες τις βάσεις δεδομένων σε ένα σύνολο shard (που δημιουργήθηκε χρησιμοποιώντας [βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη](sql-database-elastic-database-client-library.md)), χρησιμοποιήστε μια αντιστοίχιση shard ως βάση δεδομένων προορισμού. Αυτό το παράδειγμα απαιτεί μια εφαρμογή sharded που δημιουργήθηκε με τη βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη. Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το δείγμα εργαλεία ελαστικότητας βάσης δεδομένων](sql-database-elastic-scale-get-started.md).

Η βάση δεδομένων shard χάρτη manager πρέπει να οριστεί ως μια βάση δεδομένων προορισμού και, στη συνέχεια, το χάρτη συγκεκριμένες shard πρέπει να έχει καθοριστεί ως προορισμό.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Δημιουργία δέσμης ενεργειών T-SQL για εκτέλεση σε βάσεις δεδομένων

Κατά τη δημιουργία δεσμών ενεργειών T-SQL για εκτέλεση, είναι ιδιαίτερα προτεινόμενα δομούνται να είναι [idempotent](https://en.wikipedia.org/wiki/Idempotence) και είναι ανθεκτικά έναντι αποτυχιών. Ελαστικά εργασίες βάσης δεδομένων θα επανάληψη εκτέλεσης μιας δέσμης ενεργειών κάθε φορά που εκτέλεσης συναντά μια αποτυχία, ανεξάρτητα από την κατάταξη της αποτυχίας.

Χρησιμοποιήστε το [**cmdlet New-AzureSqlJobContent**](https://msdn.microsoft.com/library/mt346085.aspx) για να δημιουργήσετε και να αποθηκεύσετε μια δέσμη ενεργειών για εκτέλεση και να ορίσετε τις παραμέτρους **- ContentName** και **- Κείμενο-εντολής** .

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Δημιουργήστε μια νέα δέσμη ενεργειών από ένα αρχείο
Εάν έχει οριστεί η δέσμη ενεργειών T-SQL μέσα σε ένα αρχείο, χρησιμοποιήστε αυτήν την επιλογή για να εισαγάγετε τη δέσμη ενεργειών:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Για να ενημερώσετε μια δέσμη ενεργειών T-SQL για εκτέλεση σε βάσεις δεδομένων  

Αυτή η δέσμη ενεργειών PowerShell ενημερώνει το κείμενο της εντολής T-SQL για μια υπάρχουσα δέσμη ενεργειών.

Ορίστε τις παρακάτω μεταβλητές ώστε να αντικατοπτρίζει τον ορισμό επιθυμητή δέσμη ενεργειών για να ορίσετε:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>Για να ενημερώσετε τον ορισμό μια υπάρχουσα δέσμη ενεργειών

    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Για να δημιουργήσετε μια εργασία να εκτελεί μια δέσμη ενεργειών σε ένα χάρτη shard

Αυτή η δέσμη ενεργειών PowerShell ξεκινά μια εργασία για εκτέλεση ενεργειών σε κάθε shard σε ένα χάρτη shard ελαστικά κλίμακα.

Ορίστε τις παρακάτω μεταβλητές ώστε να αντικατοπτρίζει το επιθυμητό δέσμης ενεργειών και προορισμού:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Για να εκτελέσετε μια εργασία 

Αυτή η δέσμη ενεργειών PowerShell εκτελεί μια υπάρχουσα εργασία:

Ενημερώστε την ακόλουθη μεταβλητή ώστε να αντικατοπτρίζει το όνομα της εργασίας που θέλετε να εκτελείται:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>Για να ανακτήσετε την κατάσταση της εκτέλεσης μιας μόνο εργασία

Χρησιμοποιήστε το [**cmdlet Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) και ορίστε την παράμετρο **JobExecutionId** για να προβάλετε την κατάσταση της εκτέλεσης του έργου.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Χρησιμοποιήστε το cmdlet **Get-AzureSqlJobExecution** ίδια με την παράμετρο **IncludeChildren** για να προβάλετε την κατάσταση των θυγατρικών εργασία εκτελέσεις, συγκεκριμένα το συγκεκριμένο μέλος για κάθε εργασία εκτέλεσης κάθε βάση στοχευμένες από την εργασία.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Για να προβάλετε την κατάσταση σε πολλές εκτελέσεις εργασία

Το [**cmdlet Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) έχει πολλές προαιρετικές παράμετροι που μπορούν να χρησιμοποιηθούν για να εμφανίσετε πολλές εκτελέσεις εργασία, φιλτράρεται μέσα από τις παραμέτρους που παρέχεται. Το παρακάτω παρουσιάζει ορισμένα από τα τους πιθανούς τρόπους για να χρησιμοποιήσετε Get-AzureSqlJobExecution:

Ανάκτηση όλα εκτελέσεις ενεργού επάνω επιπέδου εργασίας:

    Get-AzureSqlJobExecution

Ανάκτηση όλα εκτελέσεις επάνω επιπέδου εργασίας, συμπεριλαμβανομένων των εκτελέσεις Ανενεργή εργασία:

    Get-AzureSqlJobExecution -IncludeInactive

Ανάκτηση όλα εκτελέσεις εργασία θυγατρικό ενός αναγνωριστικού εκτέλεσης παρεχόμενη εργασία, συμπεριλαμβανομένων των εκτελέσεις Ανενεργή εργασία:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Ανάκτηση όλα εργασία εκτελέσεις δημιουργήθηκε με ένα χρονοδιάγραμμα / συνδυασμό, συμπεριλαμβανομένων των εργασιών Ανενεργή εργασία:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Ανάκτηση όλων των εργασιών στόχευσης καθορισμένο shard χάρτη, συμπεριλαμβανομένων των ανενεργές εργασίες:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Ανάκτηση όλων των εργασιών στόχευσης μια καθορισμένη προσαρμοσμένη συλλογή, συμπεριλαμβανομένων ανενεργές εργασίες:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Ανάκτηση της λίστας των εκτελέσεις εργασίας εργασία μέσα σε μια συγκεκριμένη εργασία εκτέλεσης:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Ανάκτηση λεπτομέρειες εκτέλεσης της εργασίας έργου:

Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί για να προβάλετε τις λεπτομέρειες της εκτέλεσης εργασιών μια εργασία, η οποία είναι ιδιαίτερα χρήσιμη κατά τον εντοπισμό σφαλμάτων αποτυχίες εκτέλεσης.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a>Για να ανακτήσετε αποτυχίες εντός εκτελέσεις εργασιών έργου

Το **αντικείμενο JobTaskExecution** περιλαμβάνει μια ιδιότητα για τον κύκλο ζωής των της εργασίας μαζί με μια ιδιότητα μηνύματος. Εάν μια εργασία εκτέλεση εργασιών απέτυχε, θα οριστεί η ιδιότητα κύκλου ζωής *απέτυχε* και θα οριστεί η ιδιότητα μηνύματος για να το μήνυμα εξαίρεσης που προκύπτει και η στοίβα. Εάν μια εργασία δεν ολοκληρώθηκε με επιτυχία, είναι σημαντικό για να προβάλετε τις λεπτομέρειες των εργασιών έργου που δεν ολοκληρώθηκε με επιτυχία για μια συγκεκριμένη εργασία.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Για να περιμένετε για μια εργασία εκτέλεσης για την ολοκλήρωση

Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί να περιμένετε για μια εργασία για να ολοκληρωθεί:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Δημιουργία προσαρμοσμένου εκτέλεσης πολιτικής

Ελαστικά εργασίες βάσης δεδομένων υποστηρίζει τη δημιουργία προσαρμοσμένου εκτέλεσης πολιτικές που μπορούν να εφαρμοστούν κατά την έναρξη εργασιών.
  
Οι πολιτικές εκτέλεσης επιτρέπουν τη συγκεκριμένη στιγμή για τον ορισμό:

* Όνομα: Αναγνωριστικό για την πολιτική εκτέλεσης.
* Χρονικό όριο εκτύπωσης: Συνολικός χρόνος πριν από μια εργασία θα ακυρωθεί από ελαστικότητας εργασίες βάσης δεδομένων.
* Αρχικό χρονικό διάστημα επανάληψης: το χρονικό διάστημα αναμονής πριν από την πρώτη "Επανάληψη".
* Μέγιστο χρονικό διάστημα επανάληψης: Καπέλο διαστημάτων "Επανάληψη" για να χρησιμοποιήσετε.
* Επανάληψη συντελεστή διπλασιασμών διάστημα: Συντελεστή που χρησιμοποιούνται για τον υπολογισμό το επόμενο χρονικό διάστημα μεταξύ των επαναλήψεων.  Ο παρακάτω τύπος χρησιμοποιείται: (αρχικό χρονικό διάστημα επανάληψης) * Math.pow ((συντελεστής διπλασιασμών διάστημα), (αριθμός επαναλήψεων) - 2). 
* Μέγιστος αριθμός προσπαθειών: Ο μέγιστος αριθμός των επαναλήψεων προσπαθεί να εκτελέσετε μέσα σε μια εργασία.

Η προεπιλεγμένη πολιτική εκτέλεσης χρησιμοποιεί τις ακόλουθες τιμές:

* Όνομα: Προεπιλεγμένη πολιτική εκτέλεσης
* Χρονικό όριο εργασίας: 1 εβδομάδα
* Αρχικό χρονικό διάστημα επανάληψης: 100 χιλιοστά του δευτερολέπτου
* Μέγιστο χρονικό διάστημα επανάληψης: 30 λεπτά
* Επανάληψη συντελεστή διάστημα: 2
* Μέγιστος αριθμός προσπαθειών: 2.147.483.647

Δημιουργήστε την πολιτική επιθυμητή εκτέλεσης:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Ενημέρωση μιας πολιτικής προσαρμοσμένο εκτέλεσης

Ενημερώστε την πολιτική εκτέλεσης θέλετε να ενημερώσετε:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Ακύρωση μιας εργασίας

Ελαστικά εργασίες βάσης δεδομένων υποστηρίζει αιτήματα ακύρωσης των εργασιών.  Εάν ελαστικότητας εργασίες βάσης δεδομένων εντοπίσει μια αίτηση ακύρωσης για μια εργασία που εκτελείται τη συγκεκριμένη στιγμή, θα επιχειρήσει να διακόψετε την εργασία.

Υπάρχουν δύο τρόποι με τους ότι ελαστικότητας εργασίες βάσης δεδομένων μπορεί να εκτελέσει Ακύρωση:

1. Ακύρωση εργασίες που εκτελούνται αυτήν τη στιγμή: σε περίπτωση που εντοπιστεί Ακύρωση ενώ εκτελείται τη συγκεκριμένη στιγμή μια εργασία, θα εφαρμοστούν Ακύρωση εντός του εκτελείται πτυχή της εργασίας.  Για παράδειγμα: Εάν υπάρχει μεγάλης διάρκειας ερωτήματος που εκτελούνται τη συγκεκριμένη στιγμή όταν επιχειρείται ακύρωση, θα υπάρξει μιας προσπάθειας για να ακυρώσετε το ερώτημα.
2. Ακύρωση εργασίας επαναλήψεις: Εάν Ακύρωση εντοπιστεί από το στοιχείο ελέγχου νήμα πριν από μια εργασία γίνεται εκκίνηση για εκτέλεση, το στοιχείο ελέγχου νήμα θα αποφύγετε την εκκίνηση της εργασίας και δηλώνουν την αίτηση ως ακυρώθηκε.

Εάν ζητείται Ακύρωση εργασίας για ένα έργο γονικό, την αίτηση ακύρωσης θα εξακολουθήσουν να ισχύουν για τη γονική εργασία και για όλες τις εργασίες θυγατρικό.
 
Για να υποβάλετε μια αίτηση ακύρωσης, χρησιμοποιήστε το [**cmdlet διακοπή AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) και ορίστε την παράμετρο **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Για να διαγράψετε μια εργασία και ασύγχρονη εργασίας ιστορικού

Ελαστικά εργασίες βάσης δεδομένων υποστηρίζει ασύγχρονης διαγραφής των εργασιών. Μια εργασία να επισημανθεί για διαγραφή και το σύστημα θα διαγράψει η εργασία και όλο το ιστορικό εργασία αφού έχετε ολοκληρώσει όλες εκτελέσεις εργασία για την εργασία. Το σύστημα δεν θα ακυρώσει αυτόματα εκτελέσεις ενεργού εργασίας.  

Κλήση [**Διακοπή AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) για να ακυρώσετε εκτελέσεις ενεργού εργασίας.

Για να ενεργοποιήσετε τη διαγραφή εργασία, χρησιμοποιήστε το [**cmdlet κατάργηση AzureSqlJob**](https://msdn.microsoft.com/library/mt346083.aspx) και ορίστε την παράμετρο **όνομα_εργασίας** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="to-create-a-custom-database-target"></a>Για να δημιουργήσετε μια προσαρμοσμένη βάση δεδομένων προορισμού

Μπορείτε να ορίσετε προσαρμοσμένη βάση δεδομένων των προορισμών είτε απευθείας εκτέλεσης είτε για να συμπεριληφθούν μέσα σε μια ομάδα προσαρμοσμένης βάσης δεδομένων. Για παράδειγμα, επειδή **τα σύνολα ελαστικότητας βάσης δεδομένων** δεν ακόμη απευθείας υποστηρίζονται με χρήση του PowerShell APIs, μπορείτε να δημιουργήσετε μια προσαρμοσμένη βάση δεδομένων προορισμού και προορισμού συλλογής προσαρμοσμένη βάση δεδομένων που περιλαμβάνει όλες τις βάσεις δεδομένων στο χώρο συγκέντρωσης.

Ορίστε τις παρακάτω μεταβλητές ώστε να αντικατοπτρίζει τις πληροφορίες της βάσης δεδομένων που θέλετε:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Για να δημιουργήσετε μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής

Χρησιμοποιήστε το cmdlet [**New-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) για να ορίσετε μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής για να ενεργοποιήσετε την εκτέλεση σε πολλούς στόχους καθορισμένη βάση δεδομένων. Αφού δημιουργήσετε μια ομάδα βάσης δεδομένων, βάσεις δεδομένων μπορούν να συσχετιστούν με την προσαρμοσμένη συλλογή προορισμού.

Ορίστε τις παρακάτω μεταβλητές ώστε να αντικατοπτρίζει τις παραμέτρους θέλετε προσαρμοσμένο συλλογής προορισμού:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Για να προσθέσετε βάσεις δεδομένων σε μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής

Για να προσθέσετε μια βάση δεδομένων σε μια συγκεκριμένη συλλογή προσαρμοσμένο Χρησιμοποιήστε το cmdlet [**Προσθήκη AzureSqlJobChildTarget**](https://msdn.microsoft.comlibrary/mt346064.aspx) .

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Εξετάστε τις βάσεις δεδομένων μέσα σε μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής

Χρησιμοποιήστε το cmdlet [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) για την ανάκτηση των θυγατρικών βάσεων δεδομένων μέσα σε μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Δημιουργία μιας εργασίας για να εκτελέσετε μια δέσμη ενεργειών σε μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής

Χρησιμοποιήστε το cmdlet [**New-AzureSqlJob**](https://msdn.microsoft.com/library/mt346078.aspx) για να δημιουργήσετε μια εργασία σε σχέση με μια ομάδα βάσεις δεδομένων που ορίζονται από μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής. Ελαστικά εργασίες βάσης δεδομένων θα αναπτύξετε το έργο σε πολλές εργασίες θυγατρικό κάθε αντιστοιχεί σε μια βάση δεδομένων που σχετίζεται με το στόχο συλλογής προσαρμοσμένη βάση δεδομένων και βεβαιωθείτε ότι η δέσμη ενεργειών έχει εκτελεστεί σε κάθε βάση δεδομένων. Ξανά, είναι σημαντικό ότι οι δέσμες ενεργειών είναι idempotent να είναι ανθεκτικά στις επαναλήψεις.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Συλλογή δεδομένων σε βάσεις δεδομένων

Μπορείτε να χρησιμοποιήσετε μια εργασία για να εκτελέσετε ένα ερώτημα σε μια ομάδα βάσεων δεδομένων και να στείλετε τα αποτελέσματα σε ένα συγκεκριμένο πίνακα. Στον πίνακα μπορούν να αναζητηθούν μετά το γεγονός για να δείτε τα αποτελέσματα του ερωτήματος από κάθε βάση δεδομένων. Αυτό παρέχει ένα ασύγχρονης μέθοδο για την εκτέλεση ενός ερωτήματος σε πολλές βάσεις δεδομένων. Αποτυχημένων προσπαθειών αντιμετωπίζονται αυτόματα μέσω επαναλήψεις.

Ο πίνακας προορισμού καθορισμένο θα δημιουργηθεί αυτόματα αν δεν υπάρχει ακόμα. Το νέο πίνακα ταιριάζει με το σχήμα του συνόλου αποτελεσμάτων που επιστράφηκε. Εάν μια δέσμη ενεργειών επιστρέφει πολλαπλά σύνολα αποτελεσμάτων, εργασίες ελαστικότητας βάσης δεδομένων θα στέλνει μόνο την πρώτη στον πίνακα προορισμού.

Η ακόλουθη δέσμη ενεργειών PowerShell εκτελεί μια δέσμη ενεργειών και συλλέγει τα αποτελέσματά του σε έναν καθορισμένο πίνακα. Αυτή η δέσμη ενεργειών προϋποθέτει ότι έχει δημιουργηθεί μια δέσμη ενεργειών T-SQL που εξάγει ένα μονό σύνολο αποτελεσμάτων και ότι έχει δημιουργηθεί μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής.

Αυτή η δέσμη ενεργειών χρησιμοποιεί το cmdlet [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) . Ρυθμίστε τις παραμέτρους για δέσμης ενεργειών, τα διαπιστευτήρια και εκτέλεσης προορισμού:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Για να δημιουργήσετε και να ξεκινήσετε μια εργασία για σενάρια συλλογής δεδομένων

Αυτή η δέσμη ενεργειών χρησιμοποιεί το cmdlet [**AzureSqlJobExecution Έναρξη**](https://msdn.microsoft.com/library/mt346055.aspx) .
 
    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>Για να προγραμματίσετε ένα έναυσμα εκτέλεσης εργασίας

Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί για να δημιουργήσετε ένα επαναλαμβανόμενο χρονοδιάγραμμα. Αυτή η δέσμη ενεργειών χρησιμοποιεί ένα λεπτό διάστημα, αλλά [**Δημιουργία AzureSqlJobSchedule**](https://msdn.microsoft.com/library/mt346068.aspx) υποστηρίζει επίσης - DayInterval, - HourInterval, - MonthInterval, και - WeekInterval παραμέτρους. Μπορούν να δημιουργηθούν με χρονοδιαγράμματα που εκτελούνται μόνο μία φορά με διέρχεται - ενημερώσεις.

Δημιουργία νέου χρονοδιαγράμματος:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Για να ενεργοποιήσετε μια εργασία που εκτελείται στο χρονοδιάγραμμα

Ένα έναυσμα εργασίας μπορεί να οριστεί να έχουν εκτελεστεί σύμφωνα με το χρονοδιάγραμμα έργου. Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί για να δημιουργήσετε ένα έναυσμα εργασίας.

Χρησιμοποιήστε [Νέα AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346069.aspx) και να ορίσετε τις ακόλουθες μεταβλητές να αντιστοιχούν στις την επιθυμητή εργασία και χρονοδιάγραμμα:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    –JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Για να καταργήσετε μια προγραμματισμένη συσχέτιση για να διακόψετε την εργασία από την εκτέλεση στο χρονοδιάγραμμα

Για να διακόψετε την επαναλαμβανόμενη εργασία Εκτέλεση εργασίας μέσω ένα έναυσμα εργασία, το έναυσμα εργασίας μπορούν να καταργηθούν. Καταργήστε ένα έναυσμα εργασίας για να διακόψετε μια εργασία από την εκτέλεση σύμφωνα με ένα χρονοδιάγραμμα χρησιμοποιώντας το [**cmdlet κατάργηση AzureSqlJobTrigger**](https://msdn.microsoft.com/library/mt346070.aspx).

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Ανάκτηση εναύσματα εργασία δεσμευμένο στο χρονοδιάγραμμα

Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί για να αποκτήσετε και να εμφανίσετε τα εναύσματα εργασία έχουν καταχωρηθεί σε ένα χρονοδιάγραμμα συγκεκριμένης ώρας.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>Για να ανακτήσετε εναύσματα εργασία δεσμευμένη σε ένα έργο 

Χρησιμοποιήστε [Get-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346067.aspx) για να αποκτήσετε και να εμφανίσετε τα χρονοδιαγράμματα που περιέχει μια καταχωρημένες εργασία.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Για να δημιουργήσετε μια εφαρμογή επιπέδων δεδομένων (DACPAC) για εκτέλεση σε βάσεις δεδομένων

Για να δημιουργήσετε ένα DACPAC, ανατρέξτε στο θέμα [εφαρμογές επιπέδων δεδομένων](https://msdn.microsoft.com/library/ee210546.aspx). Για να αναπτύξετε ένα DACPAC, χρησιμοποιήστε το [cmdlet New-AzureSqlJobContent](https://msdn.microsoft.com/library/mt346085.aspx). Το DACPAC πρέπει να είναι δυνατή η πρόσβαση στην υπηρεσία. Συνιστάται να αποστείλετε ένα DACPAC που έχουν δημιουργηθεί στο χώρο αποθήκευσης Azure και δημιουργήστε μια [Υπογραφή πρόσβασης σε κοινή χρήση](../storage/storage-dotnet-shared-access-signature-part-1.md) για το DACPAC.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Για να ενημερώσετε μια εφαρμογή επιπέδων δεδομένων (DACPAC) για εκτέλεση σε βάσεις δεδομένων

Υπάρχουσα DACPACs καταχωρηθεί μέσα σε εργασίες ελαστικότητας βάσης δεδομένων μπορούν να ενημερωθούν ώστε να οδηγεί στη νέα URIs. Χρησιμοποιήστε το [**cmdlet Set-AzureSqlJobContentDefinition**](https://msdn.microsoft.com/library/mt346074.aspx) για να ενημερώσετε το URI DACPAC σε μια υπάρχουσα καταχωρημένων DACPAC:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Για να δημιουργήσετε μια εργασία για να εφαρμόσετε μια εφαρμογή επιπέδων δεδομένων (DACPAC) σε βάσεις δεδομένων

Μετά τη δημιουργία ενός DACPAC μέσα σε εργασίες ελαστικότητας βάσης δεδομένων, μπορεί να δημιουργηθεί μια εργασία για να εφαρμοστούν τα DACPAC σε μια ομάδα βάσεων δεδομένων. Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί για να δημιουργήσετε μια εργασία DACPAC σε μια προσαρμοσμένη συλλογή των βάσεων δεδομένων:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
