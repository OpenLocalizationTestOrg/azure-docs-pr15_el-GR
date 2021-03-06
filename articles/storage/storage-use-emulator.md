<properties 
    pageTitle="Χρησιμοποιήστε το προσομοίωσης Azure χώρου αποθήκευσης για ανάπτυξη και δοκιμές | Microsoft Azure" 
    description="Το προσομοίωσης Azure αποθήκευσης παρέχει ένα περιβάλλον δωρεάν τοπικής ανάπτυξης για ανάπτυξη και τον έλεγχο σε σχέση με το χώρο αποθήκευσης Azure. Μάθετε περισσότερα σχετικά με το χώρο αποθήκευσης προσομοίωσης, συμπεριλαμβανομένων πώς ελέγχονται αιτήσεις, τον τρόπο σύνδεσης με το προσομοίωσης από την εφαρμογή σας και πώς μπορείτε να χρησιμοποιήσετε το εργαλείο γραμμής εντολών." 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="tamram"/>

# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Χρησιμοποιήστε το προσομοίωσης Azure χώρου αποθήκευσης για ανάπτυξη και δοκιμές

## <a name="overview"></a>Επισκόπηση

Το Microsoft Azure προσομοίωσης αποθήκευσης παρέχει ένα τοπικό περιβάλλον που προσομοιώνει των υπηρεσιών Azure Blob, ουρά και πίνακα για σκοπούς ανάπτυξης. Χρησιμοποιώντας το προσομοίωσης χώρου αποθήκευσης, μπορείτε να ελέγξετε την εφαρμογή του σε σχέση με τις υπηρεσίες του χώρου αποθήκευσης τοπικά, χωρίς τη δημιουργία συνδρομής Azure ή προκαλώντας οποιοδήποτε κόστος. Όταν είστε ικανοποιημένοι με το πώς λειτουργεί η εφαρμογή σας στην το προσομοίωσης, μπορείτε να πραγματοποιήσετε εναλλαγή χρησιμοποιώντας ένα λογαριασμό του Azure χώρου αποθήκευσης στο cloud.

> [AZURE.NOTE] Το προσομοίωσης χώρου αποθήκευσης είναι διαθέσιμη ως μέρος του [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). Μπορείτε επίσης να εγκαταστήσετε το χώρο αποθήκευσης προσομοίωσης χρησιμοποιώντας το [μεμονωμένο πρόγραμμα εγκατάστασης](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409). Για να ρυθμίσετε τις παραμέτρους του προσομοίωσης χώρου αποθήκευσης, πρέπει να έχετε δικαιώματα διαχειριστή στον υπολογιστή.
> 
> Το χώρο αποθήκευσης προσομοίωσης εκτελείται τη συγκεκριμένη στιγμή μόνο στα Windows.
>  
> Σημειώστε ότι δεδομένων που έχουν δημιουργηθεί σε μια έκδοση από το χώρο αποθήκευσης προσομοίωσης ίσως να μην είναι δυνατή η πρόσβαση όταν χρησιμοποιείτε μια διαφορετική έκδοση. Εάν θέλετε να διατηρούνται τα δεδομένα σας για την μακροπρόθεσμη, συνιστάται να αποθηκεύετε τα δεδομένα σε ένα λογαριασμό Azure χώρου αποθήκευσης, αντί για το χώρο αποθήκευσης προσομοίωσης.

## <a name="how-the-storage-emulator-works"></a>Πώς λειτουργεί η προσομοίωσης χώρου αποθήκευσης
 
Το χώρο αποθήκευσης προσομοίωσης χρησιμοποιεί ένα τοπικό Microsoft SQL Server και το τοπικό σύστημα αρχείων για να προσομοιώσετε των υπηρεσιών Azure χώρου αποθήκευσης. Από προεπιλογή, το χώρο αποθήκευσης προσομοίωσης χρησιμοποιεί μια βάση δεδομένων Microsoft SQL Server 2012 Express LocalDB.  Μπορείτε να επιλέξετε να ρυθμίσετε τις παραμέτρους του χώρου αποθήκευσης προσομοίωσης για να αποκτήσετε πρόσβαση σε μια τοπική παρουσία του SQL Server αντί για την παρουσία LocalDB. Για περισσότερες πληροφορίες, ανατρέξτε [έναρξης και προετοιμασία του χώρου αποθήκευσης προσομοίωσης](#start-and-initialize-the-storage-emulator) κάτω από.

Μπορείτε να εγκαταστήσετε το SQL Server Management Studio Express για να διαχειριστείτε την εγκατάσταση του LocalDB. Το χώρο αποθήκευσης προσομοίωσης συνδέεται με SQL Server ή LocalDB με χρήση ελέγχου ταυτότητας των Windows. 

Υπάρχουν ορισμένες διαφορές στις λειτουργίες μεταξύ του χώρου αποθήκευσης προσομοίωσης και των υπηρεσιών Azure χώρου αποθήκευσης. Για περισσότερες πληροφορίες σχετικά με αυτές τις διαφορές, ανατρέξτε στο θέμα [διαφορές μεταξύ της προσομοίωσης χώρου αποθήκευσης και το χώρο αποθήκευσης Azure](#differences-between-the-storage-emulator-and-azure-storage).

## <a name="authenticating-requests-against-the-storage-emulator"></a>Πραγματοποίηση ελέγχου ταυτότητας προσκλήσεις σε σχέση με το χώρο αποθήκευσης προσομοίωσης

Ακριβώς όπως και με Azure χώρο αποθήκευσης στο cloud, κάθε αίτηση που κάνετε σε σχέση με το χώρο αποθήκευσης προσομοίωσης πρέπει να έχουν πιστοποιηθεί, εκτός εάν πρόκειται για ανώνυμη αίτηση. Μπορείτε να τον έλεγχο ταυτότητας προσκλήσεις σε σχέση με το χώρο αποθήκευσης προσομοίωσης με χρήση ελέγχου ταυτότητας κοινόχρηστου κλειδιού ή με μια κοινόχρηστη πρόσβαση υπογραφή (συσχετισμών Ασφαλείας).

### <a name="authentication-with-shared-key-credentials"></a>Έλεγχος ταυτότητας με διαπιστευτήρια θέσει σε κοινή χρήση αριθμού-κλειδιού

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Για περισσότερες λεπτομέρειες σε συμβολοσειρές σύνδεσης, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων Azure συμβολοσειρές σύνδεσης χώρου αποθήκευσης](storage-configure-connection-string.md). 

### <a name="authentication-with-a-shared-access-signature"></a>Έλεγχος ταυτότητας με μια κοινόχρηστη πρόσβαση υπογραφή 

Ορισμένες βιβλιοθήκες προγράμματος-πελάτη Azure χώρου αποθήκευσης, όπως η βιβλιοθήκη Xamarin, υποστηρίζει μόνο έλεγχο ταυτότητας με διακριτικό πρόσβασης κοινόχρηστο υπογραφή (συσχετισμών Ασφαλείας). Θα πρέπει να δημιουργήσετε αυτό το διακριτικό συσχετισμών Ασφαλείας χρησιμοποιώντας ένα εργαλείο ή μια εφαρμογή που υποστηρίζει έλεγχο ταυτότητας κοινόχρηστου κλειδιού. Ένας εύκολος τρόπος για να δημιουργήσετε το διακριτικό συσχετισμών Ασφαλείας είναι μέσω του Azure PowerShell:

1. Εάν δεν το έχετε κάνει ήδη, εγκαταστήστε Azure PowerShell. Συνιστάται να χρησιμοποιείτε την πιο πρόσφατη έκδοση του τα cmdlet του Azure PowerShell. Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md#Install) για οδηγίες εγκατάστασης.

2. Ανοίξτε το Azure PowerShell και εκτελέστε τις ακόλουθες εντολές. Μην ξεχάσετε να αντικαταστήσετε *όνομα_λογαριασμού* και *ACCOUNT_KEY ==* με τα δικά σας διαπιστευτήρια. Αντικατάσταση *CONTAINER_NAME* με όνομα της επιλογής σας.

        $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
        
        New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
        
        $now = Get-Date 
        
        New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

Κοινόχρηστη πρόσβαση υπογραφής που προκύπτει URI για το νέο κοντέινερ πρέπει να είναι παρόμοια με τα εξής:

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

Η υπογραφή κοινόχρηστης πρόσβασης που δημιουργήθηκαν με αυτό το παράδειγμα είναι έγκυρη για μία ημέρα. Η υπογραφή εκχωρεί πλήρη πρόσβαση (δηλαδή ανάγνωση, εγγραφή, διαγραφή, λίστα) σε αντικείμενα BLOB μέσα στο κοντέινερ.

Για περισσότερες πληροφορίες σχετικά με την κοινόχρηστη πρόσβαση υπογραφές, ανατρέξτε στο θέμα [Χρήση κοινόχρηστων Access υπογραφές (συσχετισμών Ασφαλείας)](storage-dotnet-shared-access-signature-part-1.md).


## <a name="start-and-initialize-the-storage-emulator"></a>Εκκίνηση και προετοιμασία του χώρου αποθήκευσης προσομοίωσης

Για να ξεκινήσετε το προσομοίωσης Azure χώρου αποθήκευσης, επιλέξτε το κουμπί "Έναρξη" ή πατήστε το πλήκτρο των Windows. Αρχίστε να πληκτρολογείτε **Azure προσομοίωσης χώρου αποθήκευσης**και επιλέξτε το προσομοίωσης από τη λίστα των εφαρμογών. 

Όταν εκτελείται το προσομοίωσης, θα δείτε ένα εικονίδιο στην περιοχή ειδοποιήσεων της γραμμής εργασιών των Windows.

Κατά την εκκίνηση του χώρου αποθήκευσης προσομοίωσης, θα εμφανιστεί ένα παράθυρο γραμμής εντολών. Μπορείτε να χρησιμοποιήσετε αυτό το παράθυρο γραμμής εντολών για την έναρξη και διακοπή προσομοίωσης του χώρου αποθήκευσης, καθώς και καταργήστε την επιλογή δεδομένων, λάβετε τρέχουσα κατάσταση και προετοιμασία του προσομοίωσης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αναφορά εργαλείο γραμμής εντολών προσομοίωσης χώρου αποθήκευσης](#storage-emulator-command-line-tool-reference).

Όταν είναι κλειστό το παράθυρο γραμμής εντολών, το χώρο αποθήκευσης προσομοίωσης συνεχίζει να λειτουργεί. Για να εμφανίσετε ξανά τη γραμμή εντολών, ακολουθήστε τα παραπάνω βήματα σαν ξεκινώντας το προσομοίωσης χώρου αποθήκευσης.

Την πρώτη φορά που εκτελείτε το προσομοίωσης χώρου αποθήκευσης, το περιβάλλον του τοπικού χώρου αποθήκευσης έχει προετοιμαστεί για εσάς. Η διαδικασία προετοιμασίας δημιουργεί μια βάση δεδομένων σε LocalDB και αποθεματικά HTTP θύρες για κάθε υπηρεσία του τοπικού χώρου αποθήκευσης. 

Το προσομοίωσης χώρου αποθήκευσης είναι εγκατεστημένο από προεπιλογή στον κατάλογο C:\Program Files (x86) \Microsoft SDKs\Azure\Storage προσομοίωσης. 

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Προετοιμασία του χώρου αποθήκευσης προσομοίωσης για να χρησιμοποιήσετε μια διαφορετική βάση δεδομένων SQL

Μπορείτε να χρησιμοποιήσετε το εργαλείο γραμμής εντολών προσομοίωσης χώρου αποθήκευσης για την προετοιμασία του χώρου αποθήκευσης προσομοίωσης ώστε να οδηγούν σε μια παρουσία της βάσης δεδομένων SQL εκτός από την προεπιλεγμένη παρουσία LocalDB. Σημειώστε ότι πρέπει να εκτελείτε το εργαλείο γραμμής εντολών με δικαιώματα διαχειριστή για την προετοιμασία της βάσης δεδομένων παρασκηνίου για το χώρο αποθήκευσης προσομοίωσης:

1. Κάντε κλικ στο κουμπί **Έναρξη** ή πατήστε το πλήκτρο των **Windows** . Αρχίστε να πληκτρολογείτε `Azure Storage Emulator` και επιλέξτε την όταν εμφανίζεται για να εμφανίσετε το χώρο αποθήκευσης στο εργαλείο γραμμής εντολών προσομοίωσης.
2. Στο παράθυρο γραμμή εντολών, πληκτρολογήστε την ακόλουθη εντολή, όπου `<SQLServerInstance>` είναι το όνομα της παρουσίας του SQL Server. Για να χρησιμοποιήσετε LocalDb, καθορίστε `(localdb)\v11.0` ως την παρουσία του SQL Server.

        AzureStorageEmulator init /server <SQLServerInstance> 
    
    Μπορείτε επίσης να χρησιμοποιήσετε την ακόλουθη εντολή, η οποία σας οδηγεί το προσομοίωσης για να χρησιμοποιήσετε την προεπιλεγμένη παρουσία του SQL Server:

        AzureStorageEmulator init /server .\\ 

    Εναλλακτικά, μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή, η οποία προετοιμάζει εκ νέου τη βάση δεδομένων με την προεπιλεγμένη εμφάνιση LocalDB:

        AzureStorageEmulator init /forceCreate 

Για περισσότερες πληροφορίες σχετικά με αυτές τις εντολές, ανατρέξτε στο θέμα [Αναφορά εργαλείο γραμμής εντολών προσομοίωσης χώρου αποθήκευσης](#storage-emulator-command-line-tool-reference).

## <a name="addressing-resources-in-the-storage-emulator"></a>Προσθήκη διεύθυνσης σε πόρους στο προσομοίωσης του χώρου αποθήκευσης

Τα τελικά σημεία υπηρεσίας για το χώρο αποθήκευσης προσομοίωσης είναι διαφορετικές από εκείνες ενός λογαριασμού Azure χώρου αποθήκευσης. Η διαφορά είναι οφείλεται στο γεγονός ότι στον τοπικό υπολογιστή δεν εκτελεί ανάλυση ονόματος τομέα, ώστε να απαιτούν τα τελικά σημεία προσομοίωσης χώρου αποθήκευσης μιας τοπικής διεύθυνσης αντί για ένα όνομα τομέα.

Όταν διεύθυνση ενός πόρου σε ένα λογαριασμό Azure χώρου αποθήκευσης, μπορείτε να χρησιμοποιήσετε το συνδυασμό παρακάτω, όπου το όνομα του λογαριασμού είναι μέρος του ονόματος κεντρικού υπολογιστή URI και ο πόρος που απευθύνεται είναι μέρος της διαδρομής URI:

    <http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>

Για παράδειγμα, το ακόλουθο URI είναι μια έγκυρη διεύθυνση για ένα blob σε ένα λογαριασμό Azure χώρου αποθήκευσης:

    https://myaccount.blob.core.windows.net/mycontainer/myblob.txt

Στο προσομοίωσης το χώρο αποθήκευσης, επειδή το τοπικό υπολογιστή δεν εκτελεί ανάλυση ονόματος τομέα, το όνομα του λογαριασμού είναι μέρος της διαδρομής URI αντί για το όνομα κεντρικού υπολογιστή. Μπορείτε να χρησιμοποιήσετε τον παρακάτω συνδυασμό για έναν πόρο που εκτελούνται σε προσομοίωσης του χώρου αποθήκευσης:

    http://<local-machine-address>:<port>/<account-name>/<resource-path>

Για παράδειγμα, η ακόλουθη διεύθυνση μπορεί να χρησιμοποιηθεί για πρόσβαση σε ένα αντικείμενο blob στο προσομοίωσης του χώρου αποθήκευσης:

    http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt

Τα τελικά σημεία υπηρεσίας για το χώρο αποθήκευσης προσομοίωσης είναι οι εξής:

    Blob Service: http://127.0.0.1:10000/<account-name>/<resource-path>
    Queue Service: http://127.0.0.1:10001/<account-name>/<resource-path>
    Table Service: http://127.0.0.1:10002/<account-name>/<resource-path>

### <a name="addressing-the-account-secondary-with-ra-grs"></a>Προσθήκη διεύθυνσης σε δευτερεύοντα με RA Εξοπλισμό του λογαριασμού

Ξεκινώντας με την έκδοση 3.1, ο λογαριασμός προσομοίωσης χώρου αποθήκευσης υποστηρίζει πρόσβαση για ανάγνωση παν πλεονάζοντα αναπαραγωγής (RA-Εξοπλισμό). Για πόρους χώρου αποθήκευσης στο cloud και την τοπική προσομοίωσης, μπορείτε να αποκτήσετε πρόσβαση στη δευτερεύουσα θέση με προσάρτηση - δευτερεύουσες σε σχέση με το όνομα του λογαριασμού. Για παράδειγμα, η ακόλουθη διεύθυνση μπορεί να χρησιμοποιηθεί για πρόσβαση σε ένα χρησιμοποιώντας τη δευτερεύουσα μόνο για ανάγνωση στο προσομοίωσης το χώρο αποθήκευσης blob:

    http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt 

> [AZURE.NOTE] Για πρόσβαση μέσω προγραμματισμού στο δευτερεύον με το χώρο αποθήκευσης προσομοίωσης, χρησιμοποιήστε τη βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για .NET έκδοση 3.2 ή νεότερη έκδοση. Ανατρέξτε στο θέμα στη [Βιβλιοθήκη προγράμματος-πελάτη του Microsoft Azure χώρου αποθήκευσης για .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) για λεπτομέρειες.

## <a name="storage-emulator-command-line-tool-reference"></a>Αναφορά εργαλείο γραμμής εντολών προσομοίωσης χώρου αποθήκευσης

Ξεκινώντας από την έκδοση 3.0, όταν εκκινείτε το προσομοίωσης χώρο αποθήκευσης, θα δείτε ένα παράθυρο γραμμής εντολών αναδυόμενης. Χρησιμοποιήστε το παράθυρο γραμμής εντολών για να ξεκινήσετε και να διακόψετε την προσομοίωσης καθώς και ως, για να υποβάλετε ένα ερώτημα για την κατάσταση και να εκτελέσετε άλλες λειτουργίες.

> [AZURE.NOTE] Εάν έχετε το Microsoft Azure τον υπολογισμό προσομοίωσης εγκατασταθεί, θα εμφανιστεί ένα εικονίδιο στην περιοχή ειδοποιήσεων όταν εκκινείτε το προσομοίωσης χώρου αποθήκευσης. Κάντε δεξί κλικ στο εικονίδιο για να εμφανίσετε ένα μενού, το οποίο παρέχει μια γραφικών τρόπος για να ξεκινήσετε και να διακόψετε την προσομοίωσης χώρου αποθήκευσης.

### <a name="command-line-syntax"></a>Σύνταξη γραμμής εντολών

    AzureStorageEmulator [start] [stop] [status] [clear] [init] [help]

### <a name="options"></a>Επιλογές

Για να προβάλετε τη λίστα επιλογών, πληκτρολογήστε `/help` στη γραμμή εντολών.

| Επιλογή | Περιγραφή                                                    | Εντολή                                                                                                 | Ορίσματα                                                                                                         |
|--------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Έναρξη**  | Ξεκινά το προσομοίωσης χώρου αποθήκευσης.                                | `AzureStorageEmulator start [-inprocess]`                                                                    | *-inprocess*: ξεκινήστε τη προσομοίωσης στην τρέχουσα διεργασία αντί να δημιουργήσετε μια νέα διαδικασία.                          |
| **Σταμάτα**   | Διακόπτει την προσομοίωσης χώρου αποθήκευσης.                                    | `AzureStorageEmulator stop`                                                                                  |                                                                                                                   |
| **Κατάσταση** | Εκτυπώνει την κατάσταση της προσομοίωσης του χώρου αποθήκευσης.                     | `AzureStorageEmulator status`                                                                                |                                                                                                                   |
| **Απαλοιφή**  | Καταργεί τα δεδομένα σε όλες τις υπηρεσίες που καθορίζεται στη γραμμή εντολών. | `AzureStorageEmulator clear [blob] [table] [queue] [all]                                                    `| *BLOB*: απαλείφει blob δεδομένων. <br/>*ουρά*: Καταργεί ουρά δεδομένων. <br/>*Πίνακας*: Καταργεί τα δεδομένα του πίνακα. <br/>*όλες*: Καταργεί όλα τα δεδομένα σε όλες τις υπηρεσίες. |
| **Προετοιμασία**   | Εκτελεί εφάπαξ προετοιμασίας για να ρυθμίσετε το προσομοίωσης.       | `AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate] [-inprocess]` | *-Όνομα_διακομιστή\όνομα_περιόδου_λειτουργίας server*: Καθορίζει το διακομιστή που φιλοξενεί την παρουσία του SQL. <br/>*όνομα_παρουσίας - sqlinstance*: Καθορίζει το όνομα της παρουσίας του SQL για να χρησιμοποιηθεί με την προεπιλεγμένη παρουσία διακομιστή. <br/>*-forcecreate*: επιβάλει δημιουργίας της βάσης δεδομένων SQL, ακόμα και αν υπάρχει ήδη. <br/>*-inprocess*: εκτελεί προετοιμασίας στην τρέχουσα διεργασία αντί για μια νέα διεργασία παραγωγής. Που θα πρέπει να ξεκινήσετε την τρέχουσα διαδικασία με αναβαθμισμένα δικαιώματα για να εκτελέσετε προετοιμασίας.          |
                                                                                                                  
## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Διαφορές μεταξύ της προσομοίωσης χώρου αποθήκευσης και το χώρο αποθήκευσης Azure

Επειδή το χώρο αποθήκευσης προσομοίωσης είναι ένα περιβάλλον εξομοιωμένου εκτελείται σε μια τοπική παρουσία SQL, υπάρχουν ορισμένες διαφορές στις λειτουργίες μεταξύ του προσομοίωσης και ένα λογαριασμό Azure χώρο αποθήκευσης στο cloud:

- Το χώρο αποθήκευσης προσομοίωσης υποστηρίζει μόνο ένα σταθερό λογαριασμό και έναν αριθμό-κλειδί γνωστές ελέγχου ταυτότητας.

- Το προσομοίωσης χώρου αποθήκευσης δεν είναι μια υπηρεσία αποθήκευσης μεταβλητού μεγέθους και δεν θα υποστηρίζει μεγάλου αριθμού ταυτόχρονες προγράμματα-πελάτες.

- Όπως περιγράφεται στους [πόρους διευθύνσεις στον προσομοίωσης του χώρου αποθήκευσης](#addressing-resources-in-the-storage-emulator), τους πόρους εξετάζονται διαφορετικά στις το χώρο αποθήκευσης προσομοίωσης έναντι λογαριασμού Azure χώρου αποθήκευσης. Αυτή η διαφορά είναι οφείλεται στο γεγονός ότι ανάλυση ονόματος τομέα είναι διαθέσιμο στο cloud, αλλά όχι στον τοπικό υπολογιστή.

- Ξεκινώντας με την έκδοση 3.1, ο λογαριασμός προσομοίωσης χώρου αποθήκευσης υποστηρίζει πρόσβαση για ανάγνωση παν πλεονάζοντα αναπαραγωγής (RA-Εξοπλισμό). Στο το προσομοίωσης, όλοι οι λογαριασμοί έχουν ενεργοποιηθεί RA-Εξοπλισμό και ποτέ υπάρχει τυχόν καθυστέρηση μεταξύ του κύριας και δευτερεύουσας αντίγραφα. Τις λειτουργίες γρήγορα στατιστικές υπηρεσίας Blob, λάβετε στατιστικές υπηρεσίας ουράς και λάβετε στατιστικές υπηρεσία πίνακα υποστηρίζονται στον δευτερεύοντα λογαριασμό και πάντα θα επιστρέψει την τιμή του `LastSyncTime` στοιχείο απόκρισης ως την τρέχουσα ώρα σύμφωνα με την υποκείμενη βάση δεδομένων SQL.

    Για πρόσβαση μέσω προγραμματισμού στο δευτερεύον με το χώρο αποθήκευσης προσομοίωσης, χρησιμοποιήστε τη βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για .NET έκδοση 3.2 ή νεότερη έκδοση. Ανατρέξτε στο θέμα στη [Βιβλιοθήκη προγράμματος-πελάτη του Microsoft Azure χώρου αποθήκευσης για .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) για λεπτομέρειες.

- Η υπηρεσία αρχείων και τελικά σημεία υπηρεσίας πρωτόκολλο Ερωτήσεων δεν υποστηρίζονται αυτήν τη στιγμή στο προσομοίωσης του χώρου αποθήκευσης.

- Το χώρο αποθήκευσης προσομοίωσης επιστρέφει ένα σφάλμα VersionNotSupportedByEmulator (κωδικός κατάστασης 400 - Ακατάλληλη αίτηση HTTP), εάν χρησιμοποιείτε μια έκδοση των υπηρεσιών χώρου αποθήκευσης που δεν υποστηρίζεται ακόμη από την έκδοση του το προσομοίωσης που χρησιμοποιείτε.

### <a name="differences-for-blob-storage"></a>Διαφορές για το χώρο αποθήκευσης αντικειμένων Blob 

Τις εξής διαφορές ισχύουν για χώρο αποθήκευσης αντικειμένων Blob στο το προσομοίωσης:

- Το μέγεθος του χώρου αποθήκευσης προσομοίωσης μόνο υποστηρίζει αντικειμένων blob έως 2 GB.

- Μια λειτουργία τοποθέτηση αντικειμένων Blob ενδέχεται να επιτύχει σύμφωνα με ένα blob που υπάρχει στο προσομοίωσης του χώρου αποθήκευσης και έχει μια ενεργή μίσθωση, ακόμα και αν το Αναγνωριστικό μίσθωσης δεν έχει καθοριστεί ως τμήμα της αίτησης. 

- Προσάρτηση Blob λειτουργίες δεν υποστηρίζονται από το προσομοίωσης. Προσπάθεια μια πράξη σε ένα blob προσάρτησης επιστρέφει ένα σφάλμα FeatureNotSupportedByEmulator (κωδικός κατάστασης 400 - Ακατάλληλη αίτηση HTTP).

### <a name="differences-for-table-storage"></a>Διαφορές για το χώρο αποθήκευσης πινάκων 

Τις εξής διαφορές ισχύουν για χώρο αποθήκευσης πινάκων στο το προσομοίωσης:

- Ιδιότητες ημερομηνίας στην υπηρεσία πίνακα στο το χώρο αποθήκευσης προσομοίωσης υποστηρίζει μόνο την περιοχή που υποστηρίζονται από το SQL Server 2005 (*π.χ.*, είναι απαραίτητο να είναι μεταγενέστερη από την 1η Ιανουαρίου 1753). Όλες οι ημερομηνίες πριν από την 1η Ιανουαρίου 1753 αλλάζουν σε αυτή την τιμή. Την ακρίβεια των ημερομηνιών περιορίζεται στα την ακρίβεια του SQL Server 2005, γεγονός που σημαίνει ότι οι ημερομηνίες είναι ακριβείς σε 1/300th του δευτερολέπτου.

- Το χώρο αποθήκευσης προσομοίωσης υποστηρίζει διαμερίσματα κλειδί γραμμής ιδιότητα κλειδιού τιμές και μικρότερη από 512 byte το κάθε. Επιπλέον, το συνολικό μέγεθος του το όνομα του λογαριασμού, όνομα πίνακα και ονόματα ιδιότητα κλειδιού μαζί δεν μπορεί να υπερβαίνει τα 900 byte.

- Το συνολικό μέγεθος του μια γραμμή σε έναν πίνακα στο το χώρο αποθήκευσης προσομοίωσης περιορίζεται σε λιγότερο από 1 MB.

- Στο προσομοίωσης το χώρο αποθήκευσης, πληκτρολογήστε ιδιότητες δεδομένων `Edm.Guid` ή `Edm.Binary` υποστηρίζει μόνο το `Equal (eq)` και `NotEqual (ne)` τελεστές σύγκρισης σε ερώτημα φιλτράρισμα συμβολοσειρές.

### <a name="differences-for-queue-storage"></a>Διαφορές για αποθήκευση ουράς

Δεν υπάρχουν διαφορές ειδικά για ουρά χώρου αποθήκευσης στο το προσομοίωσης.

## <a name="storage-emulator-release-notes"></a>Σημειώσεις έκδοσης προσομοίωσης χώρου αποθήκευσης

### <a name="version-45"></a>Έκδοση διαίρεσης 4,5

- Σταθερή ένα σφάλμα που προκάλεσαν προετοιμασίας και η εγκατάσταση του χώρου αποθήκευσης προσομοίωσης αποτυχία κατά τη δημιουργία αντιγράφων ασφαλείας βάσης δεδομένων μετονομάστηκε.

### <a name="version-44"></a>Έκδοση 4.4

- Το χώρο αποθήκευσης προσομοίωσης υποστηρίζει πλέον έκδοση 2015-12-11 των υπηρεσιών αποθήκευσης σε τελικά σημεία υπηρεσίας Blob, ουρά και πίνακα.

- Συλλογή των απορριμμάτων της προσομοίωσης το χώρο αποθήκευσης αντικειμένων blob δεδομένων είναι πιο αποτελεσματική τώρα όσον αφορά μεγάλου αριθμού αντικείμενα blob.

- Σταθερή ένα σφάλμα που προκάλεσαν ACL XML να επικυρώνονται λίγο διαφορετικά από τον τρόπο την υπηρεσία αποθήκευσης σημαίνει το κοντέινερ.

- Σταθερή ένα σφάλμα το οποίο μερικές φορές παρουσιάζεται max και min τιμών ημερομηνίας/ώρας για να αναφερθεί στην εσφαλμένη ζώνη ώρας.

### <a name="version-43"></a>Έκδοση 4.3

- Το χώρο αποθήκευσης προσομοίωσης υποστηρίζει πλέον έκδοση 2015-07-08 των υπηρεσιών αποθήκευσης σε τελικά σημεία υπηρεσίας Blob, ουρά και πίνακα.

### <a name="version-42"></a>Έκδοση 4.2

- Το χώρο αποθήκευσης προσομοίωσης υποστηρίζει πλέον έκδοση 2015-04-05 των υπηρεσιών αποθήκευσης σε τελικά σημεία υπηρεσίας Blob, ουρά και πίνακα.

### <a name="version-41"></a>Έκδοση 4.1

- Το χώρο αποθήκευσης προσομοίωσης υποστηρίζει πλέον έκδοση 2015-02-21 από τις υπηρεσίες χώρου αποθήκευσης σε Blob, ουρά και πίνακα τελικά σημεία υπηρεσίας, με εξαίρεση τις νέες δυνατότητες Blob προσάρτηση. 

- Το χώρο αποθήκευσης προσομοίωσης επιστρέφει τώρα ένα μήνυμα σφάλματος χαρακτηριστικό Εάν χρησιμοποιείτε μια έκδοση των υπηρεσιών χώρου αποθήκευσης που δεν υποστηρίζεται ακόμη από αυτήν την έκδοση του το προσομοίωσης. Συνιστάται να χρησιμοποιείτε την πιο πρόσφατη έκδοση του προσομοίωσης. Εάν αντιμετωπίσετε ένα σφάλμα VersionNotSupportedByEmulator (κωδικός κατάστασης 400 - Ακατάλληλη αίτηση HTTP), κάντε λήψη την πιο πρόσφατη έκδοση της προσομοίωσης του χώρου αποθήκευσης.

- Σταθερή ένα σφάλμα όπου αγωνιστικά συνθήκη που προκαλούνται πίνακα οντότητα δεδομένων να είναι εσφαλμένες κατά τη διάρκεια λειτουργιών ταυτόχρονες συγχώνευσης.

### <a name="version-40"></a>Έκδοση 4.0

- Το εκτελέσιμο αποθήκευσης προσομοίωσης έχει μετονομαστεί σε *AzureStorageEmulator.exe*.

### <a name="version-32"></a>Έκδοση 3.2

- Το χώρο αποθήκευσης προσομοίωσης υποστηρίζει πλέον έκδοση 2014-02-14 των υπηρεσιών αποθήκευσης σε τελικά σημεία υπηρεσίας Blob, ουρά και πίνακα. Σημειώστε ότι τα τελικά σημεία υπηρεσίας αρχείο προς το παρόν δεν υποστηρίζονται στο προσομοίωσης του χώρου αποθήκευσης. Για λεπτομέρειες σχετικά με την έκδοση 2014-02-14, ανατρέξτε στο θέμα [Διαχείριση εκδόσεων για τις υπηρεσίες του χώρου αποθήκευσης Azure](https://msdn.microsoft.com/library/azure/dd894041.aspx) .

### <a name="version-31"></a>Έκδοση 3.1

- Πρόσβαση για ανάγνωση παν πλεονάζοντα αποθήκευσης (RA-Εξοπλισμό) υποστηρίζεται τώρα στο προσομοίωσης του χώρου αποθήκευσης. Το λάβετε στατιστικές υπηρεσίας Blob, λάβετε στατιστικές υπηρεσίας ουράς και λάβετε APIs στατιστικές υπηρεσία πίνακα που υποστηρίζονται για το λογαριασμό δευτερεύοντα και πάντα θα επιστρέψει την τιμή του στοιχείου απόκριση LastSyncTime ως την τρέχουσα ώρα σύμφωνα με την υποκείμενη βάση δεδομένων SQL. Για πρόσβαση μέσω προγραμματισμού στο δευτερεύον με το χώρο αποθήκευσης προσομοίωσης, χρησιμοποιήστε τη βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για .NET έκδοση 3.2 ή νεότερη έκδοση. Ανατρέξτε στη βιβλιοθήκη Microsoft Azure αποθήκευσης προγράμματος-πελάτη για αναφορά .NET για λεπτομέρειες.

### <a name="version-30"></a>Έκδοση 3.0

- Το προσομοίωσης Azure χώρου αποθήκευσης δεν είναι πλέον αποστέλλεται στο ίδιο πακέτο ως το προσομοίωσης υπολογισμού.

- Χώρος αποθήκευσης προσομοίωσης περιβάλλον εργασίας χρήστη έχει καταργηθεί και θα προτιμήσει μια δέσμη ενεργειών περιβάλλον γραμμής εντολών. Για λεπτομέρειες σχετικά με το περιβάλλον γραμμής εντολών, ανατρέξτε στο θέμα αναφορά εργαλείο γραμμής εντολών προσομοίωσης χώρου αποθήκευσης. Το γραφικό περιβάλλον εργασίας θα συνεχίσουν να είναι παρόντες σε έκδοση 3.0, αλλά αυτό είναι δυνατή μόνο όταν είναι εγκατεστημένο το προσομοίωσης τον υπολογισμό κάνοντας δεξί κλικ στο εικονίδιο του δίσκου συστήματος και επιλέγοντας εμφάνιση αποθήκευσης προσομοίωσης περιβάλλοντος εργασίας Χρήστη.

- Έκδοση 2013-08-15 των υπηρεσιών Azure αποθήκευσης τώρα υποστηρίζεται πλήρως. (Προηγουμένως ήταν υποστηρίζονται μόνο αυτήν την έκδοση από χώρο αποθήκευσης προσομοίωσης έκδοση 2.2.1 Preview.)
