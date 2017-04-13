<properties
    pageTitle="Χρήση του Azure PowerShell με Azure αποθήκευσης | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε το cmdlet του Azure PowerShell για το χώρο αποθήκευσης Azure για να δημιουργήσετε και να διαχειριστείτε τους λογαριασμούς χώρο αποθήκευσης; εργασία με αντικείμενα blob, πίνακες, ουρές και αρχεία. ρύθμιση παραμέτρων και ερώτημα αποθήκευσης ανάλυση και Δημιουργία υπογραφών κοινόχρηστη πρόσβαση."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="using-azure-powershell-with-azure-storage"></a>Χρήση του Azure PowerShell με Azure αποθήκευσης

## <a name="overview"></a>Επισκόπηση

Azure PowerShell είναι μια λειτουργική μονάδα που παρέχει το cmdlet για τη Διαχείριση Azure μέσω του Windows PowerShell. Είναι ένα βάσει εργασίας κέλυφος γραμμής εντολών και γλώσσας δέσμης ενεργειών που έχει σχεδιαστεί ειδικά για τη διαχείριση του συστήματος. Με το PowerShell, μπορείτε εύκολα να ελέγξετε και να αυτοματοποιήσετε τη διαχείριση των υπηρεσιών Azure και εφαρμογές. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε τα cmdlet για να εκτελέσετε τις ίδιες εργασίες που μπορείτε να εκτελέσετε μέσω της [Πύλης Azure](https://portal.azure.com).

Σε αυτόν τον οδηγό, θα εξερευνήσουμε πώς μπορείτε να χρησιμοποιήσετε τα [Cmdlet του χώρου αποθήκευσης Azure](https://msdn.microsoft.com/library/azure/mt269418.aspx) για να εκτελέσετε μια ποικιλία ανάπτυξης και διαχείρισης εργασιών με το χώρο αποθήκευσης Azure.

Αυτός ο οδηγός προϋποθέτει ότι έχετε εκ των προτέρων εμπειρία με χρήση του [Χώρου αποθήκευσης Azure](https://azure.microsoft.com/documentation/services/storage/) και [Του Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx). Ο οδηγός παρέχει έναν αριθμό καταδεικνύουν τη χρήση του PowerShell με το χώρο αποθήκευσης Azure των δεσμών ενεργειών. Θα πρέπει να ενημερώσετε τις μεταβλητές δέσμης ενεργειών βάσει της ρύθμισης παραμέτρων πριν από την εκτέλεση κάθε δέσμη ενεργειών.

Η πρώτη ενότητα σε αυτόν τον Οδηγό παρέχει μια γρήγορη ματιά στο χώρο αποθήκευσης Azure και PowerShell. Για λεπτομερείς πληροφορίες και οδηγίες, ξεκινήστε από τις [προϋποθέσεις για τη χρήση του PowerShell Azure με το χώρο αποθήκευσης Azure](#prerequisites-for-using-azure-powershell-with-azure-storage).


## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Γρήγορα αποτελέσματα με το χώρο αποθήκευσης Azure και PowerShell σε 5 λεπτά

Αυτή η ενότητα σας δείχνει πώς να αποκτήσετε πρόσβαση αποθήκευσης Azure μέσω του PowerShell σε 5 λεπτά.

**Νέος χρήστης του Azure:** Λάβετε μια συνδρομή στο Microsoft Azure και ένα λογαριασμό Microsoft που συσχετίζεται με αυτήν τη συνδρομή. Για πληροφορίες σχετικά με τις επιλογές Azure αγοράς, ανατρέξτε στο θέμα [Δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/), [Επιλογές αγοράς](https://azure.microsoft.com/pricing/purchase-options/)και [Προσφέρει μέλος](https://azure.microsoft.com/pricing/member-offers/) (για τα μέλη του MSDN, Microsoft Partner Network, και BizSpark και άλλα προγράμματα της Microsoft).

Για περισσότερες πληροφορίες σχετικά με τις συνδρομές του Azure, ανατρέξτε στο θέμα [Εκχώρηση ρόλων διαχειριστή στο Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Αφού δημιουργήσετε μια συνδρομή στο Microsoft Azure και του λογαριασμού:**

1.  Κάντε λήψη και εγκατάσταση του [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409).
2.  Έναρξη των Windows PowerShell ενσωματωμένο δημιουργία δέσμης ενεργειών περιβάλλον (ISE): Στον τοπικό υπολογιστή σας, μεταβείτε στο μενού **Έναρξη** . Πληκτρολογήστε **Τα εργαλεία διαχείρισης** και κάντε κλικ στην επιλογή για να το εκτελέσετε. Στο παράθυρο " **Εργαλεία διαχείρισης** ", κάντε δεξιό κλικ στο **Windows PowerShell ISE**, κάντε κλικ στην επιλογή **Εκτέλεση ως διαχειριστής**.
3.  Στο **Windows PowerShell ISE**, κάντε κλικ στο **αρχείο** > **Δημιουργία** για να δημιουργήσετε ένα νέο αρχείο δέσμης ενεργειών.
4.  Τώρα, τα θα σας θα σας δώσει μια απλή δέσμη ενεργειών που εμφανίζει τη βασική τις εντολές του PowerShell για πρόσβαση στο χώρο αποθήκευσης Azure. Η δέσμη ενεργειών θα πρώτα ζητήστε από το Azure διαπιστευτήρια λογαριασμού για να προσθέσετε το Azure λογαριασμού για το τοπικό περιβάλλον PowerShell. Στη συνέχεια, η δέσμη ενεργειών θα Ορισμός της προεπιλεγμένης Azure συνδρομής και δημιουργήστε έναν νέο λογαριασμό του χώρου αποθήκευσης στο Azure. Στη συνέχεια, η δέσμη ενεργειών θα δημιουργήσετε ένα νέο κοντέινερ στο αυτού του νέου λογαριασμού χώρου αποθήκευσης και να αποστείλετε ένα υπάρχον αρχείο εικόνας (blob) σε αυτό το κοντέινερ. Μετά τη δέσμη ενεργειών εμφανίζει σε λίστα όλων των BLOB σε αυτό το κοντέινερ, θα δημιουργήσετε έναν νέο κατάλογο προορισμού στον τοπικό υπολογιστή σας και κάντε λήψη του αρχείου εικόνας.
5.  Στην παρακάτω ενότητα κώδικα, επιλέξτε τη δέσμη ενεργειών μεταξύ των παρατηρήσεων **#begin** και **#end**. Πατήστε το συνδυασμό πλήκτρων CTRL + C για να το αντιγράψετε στο Πρόχειρο.

        #begin
        # Update with the name of your subscription.
        $SubscriptionName = "YourSubscriptionName"

        # Give a name to your new storage account. It must be lowercase!
        $StorageAccountName = "yourstorageaccountname"

        # Choose "West US" as an example.
        $Location = "West US"

        # Give a name to your new container.
        $ContainerName = "imagecontainer"

        # Have an image file and a source directory in your local computer.
        $ImageToUpload = "C:\Images\HelloWorld.png"

        # A destination directory in your local computer.
        $DestinationFolder = "C:\DownloadImages"

        # Add your Azure account to the local PowerShell environment.
        Add-AzureAccount

        # Set a default Azure subscription.
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

        # Create a new storage account.
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location

        # Set a default storage account.
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

        # Create a new container.
        New-AzureStorageContainer -Name $ContainerName -Permission Off

        # Upload a blob into a container.
        Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

        # List all blobs in a container.
        Get-AzureStorageBlob -Container $ContainerName

        # Download blobs from the container:
        # Get a reference to a list of all blobs in a container.
        $blobs = Get-AzureStorageBlob -Container $ContainerName

        # Create the destination directory.
        New-Item -Path $DestinationFolder -ItemType Directory -Force  

        # Download blobs into the local destination directory.
        $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
        #end

5.  Στο **Windows PowerShell ISE**, πατήστε το συνδυασμό πλήκτρων CTRL + V για να αντιγράψετε τη δέσμη ενεργειών. Κάντε κλικ στο **αρχείο** > **Αποθήκευση**. Στο παράθυρο διαλόγου " **Αποθήκευση ως** ", πληκτρολογήστε το όνομα του αρχείου δέσμης ενεργειών, όπως "mystoragescript". Κάντε κλικ στην επιλογή **Αποθήκευση**.

6.  Τώρα, πρέπει να ενημερώσετε τις μεταβλητές δέσμης ενεργειών με βάση τις ρυθμίσεις παραμέτρων. Πρέπει να ενημερώσετε τη μεταβλητή **$SubscriptionName** με το δικό σας συνδρομή. Μπορείτε να διατηρήσετε τις υπόλοιπες μεταβλητές όπως καθορίζεται στη δέσμη ενεργειών ή να ενημερώσετε τους, όπως εσείς θέλετε.

    - **$SubscriptionName:** Πρέπει να ενημερώσετε αυτήν τη μεταβλητή με το δικό σας όνομα συνδρομής. Κάντε κλικ σε μία από τους ακόλουθους τρεις τρόπους για να εντοπίσετε το όνομα της συνδρομής σας:

        μια. Στο **Windows PowerShell ISE**, κάντε κλικ στο **αρχείο** > **Δημιουργία** για να δημιουργήσετε ένα νέο αρχείο δέσμης ενεργειών. Αντιγράψτε την ακόλουθη δέσμη ενεργειών στο νέο αρχείο δέσμης ενεργειών και κάντε κλικ στην επιλογή **Εντοπισμός σφαλμάτων** > **Εκτέλεση**. Η ακόλουθη δέσμη ενεργειών θα πρώτα ζητήστε από το Azure διαπιστευτήρια λογαριασμού για να προσθέσετε το Azure λογαριασμού για το τοπικό περιβάλλον PowerShell και, στη συνέχεια, να εμφανίσετε όλες τις συνδρομές που συνδέονται με την τοπική περίοδο λειτουργίας PowerShell. Σημειώστε το όνομα της συνδρομής που θέλετε να χρησιμοποιήσετε, ενώ μετά από αυτό το πρόγραμμα εκμάθησης:

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        β. Για να εντοπίσετε και να αντιγράψετε το όνομα της συνδρομής σας στην [Πύλη του Azure](https://portal.azure.com), στο κεντρικό μενού στα αριστερά, κάντε κλικ στην επιλογή **συνδρομές**. Αντιγράψτε το όνομα της συνδρομής που θέλετε να χρησιμοποιήσετε κατά την εκτέλεση των δεσμών ενεργειών σε αυτόν τον οδηγό.

        ![Πύλη του Azure][Image2]

        c. Για να εντοπίσετε και να αντιγράψετε το όνομα της συνδρομής σας στην [Πύλη κλασική Azure](https://manage.windowsazure.com/), κάντε κύλιση προς τα κάτω και κάντε κλικ στην επιλογή **Ρυθμίσεις** στην αριστερή πλευρά της πύλης. Κάντε κλικ στην επιλογή **συνδρομές** για να δείτε μια λίστα των συνδρομών σας. Αντιγράψτε το όνομα της συνδρομής που θέλετε να χρησιμοποιήσετε κατά την εκτέλεση των δεσμών ενεργειών που δίνεται σε αυτόν τον οδηγό.

        ![Azure κλασική πύλη][Image1]

    - **$StorageAccountName:** Χρησιμοποιήστε το όνομα που δόθηκε στη δέσμη ενεργειών ή πληκτρολογήστε ένα νέο όνομα για το λογαριασμό χώρου αποθήκευσης. **Σημαντικό:** Το όνομα του λογαριασμού χώρου αποθήκευσης πρέπει να είναι μοναδικό στο Azure. Πρέπει να είναι πεζών-κεφαλαίων, πολύ!

    - **$Location:** Χρησιμοποιήστε τη δεδομένη "Δυτική η.π.α." στη δέσμη ενεργειών ή επιλέξτε άλλες Azure θέσεις, όπως Ανατολικής η.π.α., Βόρειας Ευρώπης, και ούτω καθεξής.

    - **$ContainerName:** Χρησιμοποιήστε το όνομα που δόθηκε στη δέσμη ενεργειών ή πληκτρολογήστε ένα νέο όνομα για το κοντέινερ.

    - **$ImageToUpload:** Πληκτρολογήστε μια διαδρομή προς μια εικόνα στον τοπικό υπολογιστή σας, όπως: "C:\Images\HelloWorld.png".

    - **$DestinationFolder:** Πληκτρολογήστε μια διαδρομή σε έναν τοπικό κατάλογο για την αποθήκευση αρχείων που έχουν ληφθεί από χώρο αποθήκευσης Azure, όπως: "C:\DownloadImages".

7.  Μετά την ενημέρωση των μεταβλητών δέσμης ενεργειών στο αρχείο "mystoragescript.ps1", κάντε κλικ στο **αρχείο** > **Αποθήκευση**. Στη συνέχεια, κάντε κλικ στην επιλογή **Εντοπισμός σφαλμάτων** > **Εκτέλεση** ή πατήστε το πλήκτρο **F5** για να εκτελέσετε τη δέσμη ενεργειών.  

Αφού εκτελεστεί η δέσμη ενεργειών, θα πρέπει να έχετε ένα φάκελο τοπικού προορισμού που περιλαμβάνει το αρχείο εικόνας που έχετε λάβει. Το παρακάτω στιγμιότυπο οθόνης εμφανίζει το αποτέλεσμα παράδειγμα:

![Λήψη αντικείμενα BLOB][Image3]


> [AZURE.NOTE] Στην ενότητα "Γρήγορα αποτελέσματα με το Azure χώρου αποθήκευσης και το PowerShell σε 5 λεπτά" που παρέχεται μια σύντομη εισαγωγή σχετικά με τον τρόπο χρήσης του Azure PowerShell με το χώρο αποθήκευσης Azure. Για λεπτομερείς πληροφορίες και οδηγίες, συνιστάται να διαβάσετε στις παρακάτω ενότητες.

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Προαπαιτούμενα στοιχεία για τη χρήση του PowerShell Azure με το χώρο αποθήκευσης Azure
Χρειάζεστε μια συνδρομή Azure και το λογαριασμό για να εκτελέσετε τα cmdlet του PowerShell που σε αυτόν τον οδηγό, όπως περιγράφεται παραπάνω.

Azure PowerShell είναι μια λειτουργική μονάδα που παρέχει το cmdlet για τη Διαχείριση Azure μέσω του Windows PowerShell. Για πληροφορίες σχετικά με την εγκατάσταση και τη ρύθμιση του Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md). Συνιστάται να ότι κάνετε λήψη και να εγκαταστήσετε ή να αναβαθμίσετε την πιο πρόσφατη λειτουργική μονάδα Azure PowerShell πριν από τη χρήση αυτού του οδηγού.

Μπορείτε να εκτελέσετε τα cmdlet στο της τυπικής κονσόλας Windows PowerShell ή το Windows PowerShell ενσωματωμένη δέσμες ενεργειών περιβάλλον (ISE). Για παράδειγμα, για να ανοίξετε το **Windows PowerShell ISE**, μεταβείτε στο μενού "Έναρξη", πληκτρολογήστε τα εργαλεία διαχείρισης και κάντε κλικ στο κουμπί για να το εκτελέσετε. Στο παράθυρο "Εργαλεία διαχείρισης", κάντε δεξιό κλικ στο Windows PowerShell ISE, κάντε κλικ στην επιλογή Εκτέλεση ως διαχειριστής.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Πώς μπορείτε να διαχειριστείτε τους λογαριασμούς χώρου αποθήκευσης στο Azure

### <a name="how-to-set-a-default-azure-subscription"></a>Πώς μπορείτε να ορίσετε μια προεπιλεγμένη Azure συνδρομή
Για να διαχειριστείτε αποθήκευσης Azure μέσω του Azure PowerShell, πρέπει να τον έλεγχο ταυτότητας του περιβάλλοντος του προγράμματος-πελάτη με το Azure μέσω Azure Active Directory ελέγχου ταυτότητας ή έλεγχο ταυτότητας βάσει πιστοποιητικού. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) πρόγραμμα εκμάθησης. Αυτός ο οδηγός χρησιμοποιεί τον έλεγχο ταυτότητας Azure Active Directory.

1.  Στο Windows PowerShell ISE, πληκτρολογήστε την παρακάτω εντολή για να προσθέσετε το Azure λογαριασμού του τοπικού περιβάλλοντος του PowerShell:

    `Add-AzureAccount`

2.  Στο παράθυρο "Εισόδου στο στο Microsoft Azure", πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου και τον κωδικό πρόσβασης που σχετίζεται με το λογαριασμό σας. Azure πραγματοποιεί έλεγχο ταυτότητας και αποθηκεύει τις πληροφορίες διαπιστευτηρίων και, στη συνέχεια, κλείνει το παράθυρο.

3.  Στη συνέχεια, εκτελέστε την ακόλουθη εντολή για να προβάλετε το Azure λογαριασμούς στο περιβάλλον του τοπικού PowerShell, και βεβαιωθείτε ότι ο λογαριασμός σας εμφανίζεται:

    `Get-AzureAccount`

4.  Στη συνέχεια, εκτελέστε το ακόλουθο cmdlet για να προβάλετε όλες τις συνδρομές που συνδέονται με την τοπική περίοδο λειτουργίας PowerShell και βεβαιωθείτε ότι εμφανίζεται η συνδρομή σας:

    `Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`

5.  Για να ορίσετε μια προεπιλεγμένη Azure συνδρομή, εκτελέστε το cmdlet AzureSubscription επιλέξτε:

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Επιβεβαιώστε το όνομα της συνδρομής την προεπιλεγμένη, εκτελώντας το cmdlet Get-AzureSubscription:

    `Get-AzureSubscription -Default`

7.  Για να δείτε όλα τα διαθέσιμα cmdlet του PowerShell για το χώρο αποθήκευσης Azure, εκτελέστε:

    `Get-Command -Module Azure -Noun *Storage*`

### <a name="how-to-create-a-new-azure-storage-account"></a>Πώς μπορείτε να δημιουργήσετε ένα νέο λογαριασμό Azure χώρου αποθήκευσης
Για να χρησιμοποιήσετε Azure χώρου αποθήκευσης, χρειάζεστε ένα λογαριασμό του χώρου αποθήκευσης. Αφού ρυθμίσετε τον υπολογιστή σας για να συνδεθείτε με τη συνδρομή σας, μπορείτε να δημιουργήσετε ένα νέο λογαριασμό Azure χώρου αποθήκευσης.

1.  Εκτελέστε το cmdlet Get-AzureLocation για να βρείτε όλες τις διαθέσιμες κέντρο δεδομένων θέσεις:

    `Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes`

2.  Στη συνέχεια, εκτελέστε το cmdlet New-AzureStorageAccount για να δημιουργήσετε ένα νέο λογαριασμό του χώρου αποθήκευσης. Το παρακάτω παράδειγμα δημιουργεί ένα νέο λογαριασμό του χώρου αποθήκευσης στο το "Δυτική" Κέντρο δεδομένων η.π.α..

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location

> [AZURE.IMPORTANT] Το όνομα του λογαριασμού σας χώρο αποθήκευσης πρέπει να είναι μοναδικό μέσα σε Azure και πρέπει να είναι πεζά. Για συμβάσεις ονοματοδοσίας και τους περιορισμούς, ανατρέξτε στο θέμα [Σχετικά με τους λογαριασμούς Azure χώρου αποθήκευσης](storage-create-storage-account.md) και [ονομάτων και αναφορά σε κοντέινερ, αντικείμενα blob, και μετα-δεδομένων](http://msdn.microsoft.com/library/azure/dd135715.aspx).

### <a name="how-to-set-a-default-azure-storage-account"></a>Πώς μπορείτε να ορίσετε έναν προεπιλεγμένο λογαριασμό Azure χώρου αποθήκευσης
Μπορείτε να έχετε πολλούς λογαριασμούς χώρου αποθήκευσης στη συνδρομή σας. Μπορείτε να επιλέξετε μία από αυτές και να τη ρυθμίσετε ως του προεπιλεγμένου λογαριασμού χώρου αποθήκευσης για όλες τις εντολές του χώρου αποθήκευσης στην ίδια περίοδο λειτουργίας PowerShell. Αυτό σας επιτρέπει να εκτελέσετε τις εντολές αποθήκευσης Azure PowerShell χωρίς να καθορίσετε ρητά το περιβάλλον του χώρου αποθήκευσης.

1.  Για να ορίσετε έναν προεπιλεγμένο λογαριασμό χώρου αποθήκευσης για τη συνδρομή σας, μπορείτε να εκτελέσετε το cmdlet Set-AzureSubscription.

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  Στη συνέχεια, εκτελέστε το cmdlet Get-AzureSubscription για να επαληθεύσετε ότι ο λογαριασμός χώρου αποθήκευσης είναι συσχετισμένες με τον προεπιλεγμένο λογαριασμό συνδρομής. Αυτή η εντολή επιστρέφει τις ιδιότητες εγγραφής της τρέχουσας συνδρομής, συμπεριλαμβανομένης της τρέχουσας λογαριασμό χώρου αποθήκευσης.

        Get-AzureSubscription –Current

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Πώς μπορείτε να τοποθετήσετε στη λίστα όλοι οι λογαριασμοί Azure χώρο αποθήκευσης στη συνδρομή
Κάθε Azure συνδρομή μπορεί να έχει έως 100 λογαριασμούς χώρου αποθήκευσης. Για τις πιο πρόσφατες πληροφορίες σχετικά με τα όρια, ανατρέξτε στο θέμα [συνδρομή Azure και όρια υπηρεσίας, όρια, και τους περιορισμούς](../azure-subscription-service-limits.md).

Εκτελέστε το ακόλουθο cmdlet για να βρείτε το όνομα και την κατάσταση των λογαριασμών χώρου αποθήκευσης στην τρέχουσα εγγραφή:

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### <a name="how-to-create-an-azure-storage-context"></a>Πώς μπορείτε να δημιουργήσετε ένα περιβάλλον Azure χώρου αποθήκευσης
Περιβάλλον Azure χώρου αποθήκευσης είναι ένα αντικείμενο σε PowerShell για τη συμπύκνωση το χώρο αποθήκευσης διαπιστευτηρίων. Χρησιμοποιώντας ένα περιβάλλον του χώρου αποθήκευσης, ενώ εκτελείται οποιαδήποτε οι επόμενες cmdlet σάς επιτρέπει να ελέγχουν την ταυτότητα της αίτησης χωρίς να καθορίσετε το λογαριασμό χώρου αποθήκευσης και το πλήκτρο πρόσβασης ρητά. Μπορείτε να δημιουργήσετε ένα περιβάλλον του χώρου αποθήκευσης με πολλούς τρόπους, όπως χρησιμοποιώντας κλειδί πρόσβαση και το όνομα του λογαριασμού χώρου αποθήκευσης, διακριτικό υπογραφής (συσχετισμών Ασφαλείας) κοινόχρηστη πρόσβαση, συμβολοσειρά σύνδεσης, ή ανώνυμη. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx).  

Χρησιμοποιήστε έναν από τους ακόλουθους τρεις τρόπους για να δημιουργήσετε ένα περιβάλλον του χώρου αποθήκευσης:

- Εκτελέστε το cmdlet [Get-AzureStorageKey](http://msdn.microsoft.com/library/azure/dn495235.aspx) για να βρείτε το πλήκτρο πρόσβασης πρωτεύοντος χώρου αποθήκευσης για το λογαριασμό σας Azure χώρου αποθήκευσης. Στη συνέχεια, καλέστε το cmdlet [New-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx) για να δημιουργήσετε ένα περιβάλλον του χώρου αποθήκευσης:

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary


- Δημιουργία ενός διακριτικού υπογραφή κοινόχρηστη πρόσβαση για ένα κοντέινερ Azure χώρου αποθήκευσης και χρησιμοποιήστε το για να δημιουργήσετε ένα περιβάλλον του χώρου αποθήκευσης:

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία AzureStorageContainerSASToken](http://msdn.microsoft.com/library/azure/dn806416.aspx) και [Χρήση κοινόχρηστων Access υπογραφές (συσχετισμών Ασφαλείας)](storage-dotnet-shared-access-signature-part-1.md).

- Σε ορισμένες περιπτώσεις, ίσως θέλετε να καθορίσετε τα τελικά σημεία υπηρεσίας όταν δημιουργείτε ένα νέο περιβάλλον χώρου αποθήκευσης. Αυτό μπορεί να είναι απαραίτητο, όταν που έχετε καταχωρήσει ένα προσαρμοσμένο όνομα τομέα για το λογαριασμό χώρου αποθήκευσης με την υπηρεσία Blob ή που θέλετε να χρησιμοποιήσετε μια υπογραφή κοινόχρηστη πρόσβαση για πρόσβαση σε πόρους χώρου αποθήκευσης. Ορίστε τα τελικά σημεία υπηρεσίας σε μια συμβολοσειρά σύνδεσης και να το χρησιμοποιήσετε για να δημιουργήσετε ένα νέο περιβάλλον χώρου αποθήκευσης, όπως φαίνεται παρακάτω:

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString

Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να ρυθμίσετε μια συμβολοσειρά σύνδεσης του χώρου αποθήκευσης, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων συμβολοσειρές σύνδεσης](storage-configure-connection-string.md).

Τώρα που έχετε ορίσει τον υπολογιστή σας και μάθατε πώς μπορείτε να διαχειριστείτε συνδρομές και χρήση του Azure PowerShell λογαριασμούς χώρου αποθήκευσης, μεταβείτε στην επόμενη ενότητα για να μάθετε πώς μπορείτε να διαχειριστείτε Azure αντικείμενα BLOB και αντικειμένων blob στιγμιότυπα.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Πώς να ανακτάτε και να αναδημιουργήσετε πλήκτρα Azure χώρου αποθήκευσης

Ένα λογαριασμό αποθήκευσης Azure συνοδεύεται από δύο κλειδιά λογαριασμού. Μπορείτε να χρησιμοποιήσετε το ακόλουθο cmdlet για την ανάκτηση των αριθμών-κλειδιών.

    Get-AzureStorageKey -StorageAccountName "yourstorageaccount"

Χρησιμοποιήστε το ακόλουθο cmdlet για να ανακτήσετε ένα συγκεκριμένο κλειδί. Έγκυρες τιμές είναι κύριας και δευτερεύουσας.  

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary

Εάν θέλετε να αναδημιουργήσετε των αριθμών-κλειδιών, χρησιμοποιήστε το ακόλουθο cmdlet. Έγκυρες τιμές για τύπο-κλειδιού είναι "Κύρια" και "Δευτερεύουσα"

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Primary”

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Secondary”

## <a name="how-to-manage-azure-blobs"></a>Πώς μπορείτε να διαχειριστείτε Azure αντικείμενα BLOB
Χώρο αποθήκευσης Blob του Azure είναι μια υπηρεσία για την αποθήκευση μεγάλου όγκου μη δομημένα δεδομένα, όπως κείμενο ή δυαδικά δεδομένα, τα οποία είναι δυνατή η πρόσβαση από οπουδήποτε στον κόσμο μέσω HTTP ή HTTPS. Αυτή η ενότητα προϋποθέτει ότι είστε ήδη εξοικειωμένοι με τις έννοιες υπηρεσία αποθήκευσης αντικειμένων Blob του Azure. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob χρησιμοποιώντας .NET](storage-dotnet-how-to-use-blobs.md) και [Έννοιες εξυπηρέτησης Blob](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-to-create-a-container"></a>Πώς μπορείτε να δημιουργήσετε ένα κοντέινερ
Κάθε blob στο χώρο αποθήκευσης Azure πρέπει να είναι σε ένα κοντέινερ. Μπορείτε να δημιουργήσετε ένα κοντέινερ ιδιωτικού χρησιμοποιώντας το cmdlet New-AzureStorageContainer:

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] Υπάρχουν τρία επίπεδα ανώνυμη πρόσβαση για ανάγνωση: **Απενεργοποίηση** **Blob**και **κοντέινερ**. Για να αποτρέψετε ανώνυμη πρόσβαση σε αντικείμενα blob, ορίστε την παράμετρο δικαιωμάτων σε **Απενεργοποίηση**. Από προεπιλογή, το νέο κοντέινερ είναι ιδιωτική και είναι δυνατή η πρόσβαση μόνο από τον κάτοχο του λογαριασμού. Για να επιτρέψετε την ανώνυμη δημόσια πρόσβαση για ανάγνωση για τους πόρους blob, αλλά όχι σε κοντέινερ μετα-δεδομένων ή στη λίστα των αντικειμένων blob στο κοντέινερ, ορίστε την παράμετρο δικαιωμάτων σε **Blob**. Για να επιτρέψετε την πλήρη δημόσια πρόσβαση για ανάγνωση σε πόρους blob, κοντέινερ μετα-δεδομένων και τη λίστα των αντικειμένων blob στο κοντέινερ, ορίστε την παράμετρο δικαιωμάτων **κοντέινερ**. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση ανώνυμη πρόσβαση για ανάγνωση σε κοντέινερ και αντικείμενα BLOB](storage-manage-access-to-resources.md).

### <a name="how-to-upload-a-blob-into-a-container"></a>Πώς μπορείτε να αποστείλετε ένα blob σε ένα κοντέινερ
Χώρο αποθήκευσης Blob του Azure υποστηρίζει αντικείμενα BLOB μπλοκ και αντικείμενα BLOB σελίδας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Κατανόηση των μπλοκ blob, αντικείμενα BLOB Προσάρτηση, και αντικείμενα BLOB σελίδας](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Για να αποστείλετε αντικείμενα BLOB σε ένα κοντέινερ, μπορείτε να χρησιμοποιήσετε το cmdlet [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) . Από προεπιλογή, αυτή η εντολή μεταφέρει τα τοπικά αρχεία σε ένα μπλοκ blob. Για να καθορίσετε τον τύπο για το αντικείμενο blob, μπορείτε να χρησιμοποιήσετε την παράμετρο - BlobType.

Το παρακάτω παράδειγμα εκτελείται το cmdlet [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) για να λάβετε όλα τα αρχεία στον καθορισμένο φάκελο και, στη συνέχεια, μεταφέρει τους στο επόμενο cmdlet χρησιμοποιώντας τον τελεστή διοχέτευσης. Το cmdlet [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) μεταφέρει τα αρχεία τοπικά σας κοντέινερ:

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### <a name="how-to-download-blobs-from-a-container"></a>Πώς μπορείτε να κάνετε λήψη αντικείμενα BLOB από ένα κοντέινερ
Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να κάνετε λήψη αντικείμενα BLOB από ένα κοντέινερ. Το παράδειγμα δημιουργεί πρώτα μια σύνδεση με το χώρο αποθήκευσης Azure χρησιμοποιώντας το περιβάλλον λογαριασμού χώρου αποθήκευσης, το οποίο περιλαμβάνει το όνομα του λογαριασμού χώρου αποθήκευσης και του access πρωτεύοντος κλειδιού. Στη συνέχεια, το παράδειγμα ανακτά μια αναφορά blob χρησιμοποιώντας το cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) . Στη συνέχεια, το παράδειγμα χρησιμοποιεί το cmdlet [Get-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806418.aspx) για να κάνετε λήψη αντικείμενα blob στο φάκελο τοπικού προορισμού.

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>Τρόπος αντιγραφής αντικείμενα BLOB από ένα κοντέινερ χώρου αποθήκευσης σε μια άλλη
Μπορείτε να αντιγράψετε αντικείμενα BLOB σε λογαριασμούς χώρου αποθήκευσης και περιοχών ασύγχρονα. Το παρακάτω παράδειγμα παρουσιάζει τον τρόπο για να αντιγράψετε αντικείμενα BLOB από ένα κοντέινερ χώρου αποθήκευσης σε ένα άλλο σε δύο λογαριασμούς διαφορετικό χώρο αποθήκευσης. Το παράδειγμα πρώτα ορίζει τις μεταβλητές για λογαριασμούς του χώρου αποθήκευσης προέλευσης και προορισμού και, στη συνέχεια, δημιουργεί ένα περιβάλλον του χώρου αποθήκευσης για κάθε λογαριασμό. Στη συνέχεια, το παράδειγμα αντιγράφει αντικείμενα BLOB από το κοντέινερ προέλευσης στο κοντέινερ προορισμού χρησιμοποιώντας το cmdlet [AzureStorageBlobCopy Έναρξη](http://msdn.microsoft.com/library/azure/dn806394.aspx) . Το παράδειγμα προϋποθέτει ότι τους λογαριασμούς αποθήκευσης προέλευσης και προορισμού και το κοντέινερ υπάρχει ήδη.

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

Σημειώστε ότι αυτό το παράδειγμα πραγματοποιεί ασύγχρονης ένα αντίγραφο. Μπορείτε να παρακολουθείτε την κατάσταση κάθε αντίγραφο, εκτελώντας το cmdlet [Get-AzureStorageBlobCopyState](http://msdn.microsoft.com/library/azure/dn806406.aspx) .

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>Τρόπος αντιγραφής αντικείμενα BLOB από μια δευτερεύουσα τοποθεσία
Μπορείτε να αντιγράψετε αντικείμενα BLOB από τη δευτερεύουσα θέση ενός λογαριασμού RA Εξοπλισμό-με δυνατότητα.

    #define secondary storage context using a connection string constructed from secondary endpoints.
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### <a name="how-to-delete-a-blob"></a>Πώς μπορείτε να διαγράψετε ένα blob
Για να διαγράψετε ένα blob, πρώτα λήψη μιας αναφοράς blob και, στη συνέχεια, καλέστε το cmdlet κατάργηση AzureStorageBlob σε αυτό. Το παρακάτω παράδειγμα διαγράφει όλα τα αντικείμενα BLOB σε μια δεδομένη κοντέινερ. Το παράδειγμα πρώτα ορίζει τις μεταβλητές για ένα λογαριασμό του χώρου αποθήκευσης και, στη συνέχεια, δημιουργεί ένα περιβάλλον του χώρου αποθήκευσης. Στη συνέχεια, το παράδειγμα ανακτά μια αναφορά blob χρησιμοποιώντας το cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) και εκτελείται το cmdlet [AzureStorageBlob κατάργηση](http://msdn.microsoft.com/library/azure/dn806399.aspx) για να καταργήσετε αντικείμενα BLOB από ένα κοντέινερ στο Azure χώρο αποθήκευσης.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## <a name="how-to-manage-azure-blob-snapshots"></a>Πώς μπορείτε να διαχειριστείτε στιγμιότυπα αντικειμένων blob του Azure
Azure σάς επιτρέπει να δημιουργήσετε ένα στιγμιότυπο του ένα blob. Ένα στιγμιότυπο είναι μια έκδοση μόνο για ανάγνωση του ένα blob που λαμβάνεται από ένα σημείο στο χρόνο. Μόλις δημιουργηθεί ένα στιγμιότυπο, το μπορούν να διαβάσετε, αντιγραφή, ή διαγραφεί, αλλά δεν τροποποιηθεί. Στιγμιότυπα παρέχει κάποιο τρόπο για να δημιουργήσετε αντίγραφα ασφαλείας ένα blob, όπως εμφανίζεται σε μια συγκεκριμένη χρονική στιγμή. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία ένα στιγμιότυπο του ένα Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-to-create-a-blob-snapshot"></a>Πώς μπορείτε να δημιουργήσετε ένα στιγμιότυπο blob
Για να δημιουργήσετε μια snaphot από ένα blob, πρώτα λήψη μιας αναφοράς blob και, στη συνέχεια, καλέστε την `ICloudBlob.CreateSnapshot` σε αυτήν τη μέθοδο. Το παρακάτω παράδειγμα πρώτα ορίζει τις μεταβλητές για ένα λογαριασμό του χώρου αποθήκευσης και, στη συνέχεια, δημιουργεί ένα περιβάλλον του χώρου αποθήκευσης. Στη συνέχεια, το παράδειγμα ανακτά μια αναφορά blob χρησιμοποιώντας το cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) και εκτελεί τη μέθοδο [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) για να δημιουργήσετε ένα στιγμιότυπο.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### <a name="how-to-list-a-blobs-snapshots"></a>Πώς να λίστα στιγμιότυπων ένα blob
Μπορείτε να δημιουργήσετε όσες στιγμιότυπα θέλετε για ένα blob. Μπορείτε να παραθέσετε τα στιγμιότυπα που σχετίζεται με το blob για να παρακολουθείτε την τρέχουσα στιγμιότυπα. Το παρακάτω παράδειγμα χρησιμοποιεί μια προκαθορισμένη blob και καλεί το cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) για να παραθέσετε τα στιγμιότυπα από το αντικείμενο blob.  

    #Define the blob name.
    $BlobName = "yourblobname"

    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Πώς μπορείτε να αντιγράψετε ένα στιγμιότυπο του ένα blob
Μπορείτε να αντιγράψετε ένα στιγμιότυπο του ένα blob για να επαναφέρετε το στιγμιότυπο. Για λεπτομερείς πληροφορίες και τους περιορισμούς, ανατρέξτε στο θέμα [Δημιουργία ένα στιγμιότυπο του ένα Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). Το παρακάτω παράδειγμα πρώτα ορίζει τις μεταβλητές για ένα λογαριασμό του χώρου αποθήκευσης και, στη συνέχεια, δημιουργεί ένα περιβάλλον του χώρου αποθήκευσης. Στη συνέχεια, το παράδειγμα καθορίζει τις μεταβλητές όνομα κοντέινερ και αντικειμένων blob. Το παράδειγμα ανακτά μια αναφορά blob χρησιμοποιώντας το cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) και εκτελεί τη μέθοδο [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) για να δημιουργήσετε ένα στιγμιότυπο. Στη συνέχεια, το παράδειγμα εκτελείται το cmdlet [AzureStorageBlobCopy έναρξης](http://msdn.microsoft.com/library/azure/dn806394.aspx) για να αντιγράψετε το στιγμιότυπο της ένα blob χρησιμοποιώντας το αντικείμενο ICloudBlob για το αντικείμενο blob προέλευσης. Φροντίστε να ενημερώνετε τις μεταβλητές βάσει της ρύθμισης παραμέτρων πριν από την εκτέλεση του παραδείγματος. Σημειώστε ότι το παρακάτω παράδειγμα προϋποθέτει ότι το αρχείο προέλευσης και προορισμού κοντέινερ και το αντικείμενο προέλευσης blob υπάρχει ήδη.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

Τώρα που μάθατε πώς μπορείτε να διαχειριστείτε Azure αντικείμενα BLOB και αντικειμένων blob στιγμιότυπα με το Azure PowerShell, μεταβείτε στην επόμενη ενότητα για να μάθετε πώς μπορείτε να διαχειριστείτε τους πίνακες, ουρές και αρχείων.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Πώς μπορείτε να διαχειριστείτε Azure πίνακες και οντοτήτων πίνακα
Υπηρεσία αποθήκευσης στο Azure πίνακα είναι μια αποθήκευσης δεδομένων NoSQL, το οποίο μπορείτε να χρησιμοποιήσετε για την αποθήκευση και υποβολή ερωτήματος τεράστιες σύνολα δομημένες, μη σχεσιακών δεδομένων. Τα κύρια στοιχεία της υπηρεσίας είναι πίνακες, οντοτήτων και ιδιότητες. Ένας πίνακας είναι μια συλλογή οντοτήτων. Μια οντότητα είναι ένα σύνολο ιδιοτήτων. Κάθε οντότητα μπορεί να έχει έως 252 ιδιότητες, οι οποίες είναι όλα τα ζεύγη ονόματος-τιμής. Αυτή η ενότητα προϋποθέτει ότι είστε ήδη εξοικειωμένοι με τις έννοιες υπηρεσία αποθήκευσης στο Azure πίνακα. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Κατανόηση του μοντέλου δεδομένων υπηρεσίας του πίνακα](http://msdn.microsoft.com/library/azure/dd179338.aspx) και [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης πινάκων του Azure μέσω .NET](storage-dotnet-how-to-use-tables.md).

Στις ακόλουθες ενότητες, θα μάθετε πώς μπορείτε να διαχειριστείτε την υπηρεσία αποθήκευσης Azure πίνακα με χρήση του PowerShell Azure. Τα σενάρια καλύπτεται περιλαμβάνουν **τη δημιουργία**, **Διαγραφή**και **την ανάκτηση** **πίνακες**, καθώς και **Προσθήκη**, **Υποβολή ερωτημάτων**και **Διαγραφή πίνακα οντοτήτων**.

### <a name="how-to-create-a-table"></a>Πώς μπορείτε να δημιουργήσετε έναν πίνακα
Κάθε πίνακας πρέπει να βρίσκονται σε ένα λογαριασμό Azure χώρου αποθήκευσης. Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να δημιουργήσετε έναν πίνακα στο χώρο αποθήκευσης Azure. Το παράδειγμα δημιουργεί πρώτα μια σύνδεση με το χώρο αποθήκευσης Azure χρησιμοποιώντας το περιβάλλον λογαριασμού χώρου αποθήκευσης, το οποίο περιλαμβάνει το όνομα του λογαριασμού χώρου αποθήκευσης και το πλήκτρο πρόσβασης. Στη συνέχεια, χρησιμοποιεί το cmdlet [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) για να δημιουργήσετε έναν πίνακα στο χώρο αποθήκευσης Azure.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-retrieve-a-table"></a>Πώς μπορείτε να ανακτήσετε έναν πίνακα
Μπορείτε να ερωτήματος και να ανακτήσετε έναν ή όλους τους πίνακες σε ένα λογαριασμό του χώρου αποθήκευσης. Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να ανακτήσετε ένα δεδομένο πίνακα χρησιμοποιώντας το cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) .

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

Αν καλέσετε το cmdlet Get-AzureStorageTable χωρίς παραμέτρους, λαμβάνει όλους τους πίνακες του χώρου αποθήκευσης για ένα λογαριασμό του χώρου αποθήκευσης.

### <a name="how-to-delete-a-table"></a>Πώς μπορείτε να διαγράψετε έναν πίνακα
Μπορείτε να διαγράψετε έναν πίνακα από ένα λογαριασμό του χώρου αποθήκευσης, χρησιμοποιώντας το cmdlet [Κατάργηση AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806393.aspx) .  

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-manage-table-entities"></a>Πώς μπορείτε να διαχειριστείτε οντοτήτων πίνακα
Προς το παρόν, Azure PowerShell δεν παρέχει cmdlet για τη Διαχείριση οντοτήτων πίνακα απευθείας. Για να εκτελέσετε λειτουργίες σε οντοτήτων πίνακα, μπορείτε να χρησιμοποιήσετε τις κατηγορίες που δίνεται στη [Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης για .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-to-add-table-entities"></a>Πώς μπορείτε να προσθέσετε οντοτήτων πίνακα
Για να προσθέσετε μια οντότητα σε έναν πίνακα, πρέπει πρώτα να δημιουργήσετε ένα αντικείμενο το οποίο ορίζει ιδιότητες σας οντότητα. Μια οντότητα μπορεί να έχει έως 255 ιδιότητες, συμπεριλαμβανομένων των 3 Ιδιότητες συστήματος: **PartitionKey**, **RowKey**και **χρονική σήμανση**. Είστε υπεύθυνοι για την εισαγωγή και ενημέρωση των τιμών **PartitionKey** και **RowKey**. Ο διακομιστής διαχειρίζεται την τιμή της **χρονικής σήμανσης**, που δεν μπορεί να τροποποιηθεί. Μαζί του **PartitionKey** και **RowKey** προσδιορίζει με μοναδικό τρόπο κάθε οντότητα μέσα σε έναν πίνακα.

-   **PartitionKey**: Καθορίζει τα διαμερίσματα που είναι αποθηκευμένη η οντότητα στο.
-   **RowKey**: προσδιορίζει με μοναδικό τρόπο η οντότητα εντός τα διαμερίσματα.

Ενδέχεται να μπορείτε να ορίσετε έως 252 προσαρμοσμένων ιδιοτήτων για μια οντότητα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Κατανόηση του μοντέλου δεδομένων υπηρεσίας του πίνακα](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να προσθέσετε οντοτήτων σε έναν πίνακα. Το παράδειγμα δείχνει πώς μπορείτε να ανακτήσετε τον πίνακα υπαλλήλων και να προσθέσετε διάφορες οντοτήτων σε αυτήν. Πρώτα, δημιουργεί μια σύνδεση στο χώρο αποθήκευσης Azure χρησιμοποιώντας το περιβάλλον λογαριασμού χώρου αποθήκευσης, το οποίο περιλαμβάνει το όνομα του λογαριασμού χώρου αποθήκευσης και το πλήκτρο πρόσβασης. Στη συνέχεια, ανακτά την δεδομένο πίνακα χρησιμοποιώντας το cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . Εάν δεν υπάρχει στον πίνακα, το cmdlet [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) χρησιμοποιείται για τη δημιουργία ενός πίνακα στο χώρο αποθήκευσης Azure. Στη συνέχεια, το παράδειγμα καθορίζει μια προσαρμοσμένη συνάρτηση οντότητα Προσθήκη για να προσθέσετε οντοτήτων στον πίνακα, καθορίζοντας partition κάθε οντότητα και κλειδί γραμμής. Η συνάρτηση Προσθήκη οντότητα κλήσεις το cmdlet [New-αντικείμενο](http://technet.microsoft.com/library/hh849885.aspx) στην τάξη [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) για να δημιουργήσετε ένα αντικείμενο οντότητα. Αργότερα, το παράδειγμα καλεί τη μέθοδο [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) σε αυτό το αντικείμενο οντότητα για να το προσθέσετε σε έναν πίνακα.

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )

      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)

      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }

    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4

#### <a name="how-to-query-table-entities"></a>Τρόπος υποβολής ερωτημάτων οντοτήτων πίνακα
Για να το ερώτημα σε έναν πίνακα, χρησιμοποιήστε την κλάση [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) . Το παρακάτω παράδειγμα προϋποθέτει ότι έχετε εκτελέσει ήδη τη δέσμη ενεργειών που δίνεται του άρθρου πώς να προσθέσετε οντοτήτων ενότητα αυτού του οδηγού. Το παράδειγμα δημιουργεί πρώτα μια σύνδεση με το χώρο αποθήκευσης Azure χρησιμοποιώντας το περιβάλλον χώρου αποθήκευσης, η οποία περιλαμβάνει το όνομα του λογαριασμού χώρου αποθήκευσης και το πλήκτρο πρόσβασης. Στη συνέχεια, προσπαθεί να ανακτήσετε τα που δημιουργήσατε προηγουμένως πίνακα "Υπάλληλοι" με το cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . Καλεί το cmdlet [New-αντικείμενο](http://technet.microsoft.com/library/hh849885.aspx) στην τάξη Microsoft.WindowsAzure.Storage.Table.TableQuery δημιουργεί ένα νέο ερώτημα αντικείμενο. Το παράδειγμα πραγματοποιεί αναζήτηση για τα πρόσωπα που έχουν μια στήλη 'ID' του οποίου η τιμή είναι 1 όπως καθορίζονται στο φίλτρο συμβολοσειρά. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Υποβολή ερωτημάτων πίνακες και οντοτήτων](http://msdn.microsoft.com/library/azure/dd894031.aspx). Όταν εκτελείτε το ερώτημα, η συνάρτηση επιστρέφει όλα οντοτήτων που ικανοποιούν τα κριτήρια φίλτρου.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx

    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")

    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20

    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)

    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize

#### <a name="how-to-delete-table-entities"></a>Πώς μπορείτε να διαγράψετε οντοτήτων πίνακα
Μπορείτε να διαγράψετε μια οντότητα χρησιμοποιώντας τα πλήκτρα διαμερίσματα και της γραμμής. Το παρακάτω παράδειγμα προϋποθέτει ότι έχετε εκτελέσει ήδη τη δέσμη ενεργειών που δίνεται του άρθρου πώς να προσθέσετε οντοτήτων ενότητα αυτού του οδηγού. Το παράδειγμα δημιουργεί πρώτα μια σύνδεση με το χώρο αποθήκευσης Azure χρησιμοποιώντας το περιβάλλον χώρου αποθήκευσης, η οποία περιλαμβάνει το όνομα του λογαριασμού χώρου αποθήκευσης και το πλήκτρο πρόσβασης. Στη συνέχεια, προσπαθεί να ανακτήσετε τα που δημιουργήσατε προηγουμένως πίνακα "Υπάλληλοι" με το cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . Εάν ο πίνακας υπάρχει, το παράδειγμα καλεί τη μέθοδο [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) για να ανακτήσετε μια οντότητα με βάση τις τιμές κλειδιού διαμερίσματα και της γραμμής. Στη συνέχεια, μεταβιβάζουν η οντότητα για τη μέθοδο [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) για να διαγράψετε.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
       $entity = $tableResult.Result
    if ($entity -ne $null)
    {
       #Delete the entity.
       $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Πώς μπορείτε να διαχειριστείτε Azure ουρές και μηνύματα ουράς
Ο χώρος αποθήκευσης Azure ουρά είναι μια υπηρεσία για την αποθήκευση μεγάλου αριθμού των μηνυμάτων που μπορεί να είναι προσβάσιμα από οπουδήποτε στον κόσμο μέσω με έλεγχο ταυτότητας κλήσεων με HTTP ή HTTPS. Αυτή η ενότητα προϋποθέτει ότι είστε ήδη εξοικειωμένοι με τις έννοιες υπηρεσία αποθήκευσης στο Azure ουρά. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης ουρά Azure μέσω του .NET](storage-dotnet-how-to-use-queues.md).

Αυτή η ενότητα θα σας δείξουν πώς μπορείτε να διαχειριστείτε την υπηρεσία αποθήκευσης Azure ουρά με χρήση του PowerShell Azure. Τα σενάρια καλύπτεται περιλαμβάνουν **Εισαγωγή** και **τη διαγραφή** μηνυμάτων ουρά, καθώς και **τη δημιουργία**, **Διαγραφή**και **την ανάκτηση ουρές**.

### <a name="how-to-create-a-queue"></a>Πώς μπορείτε να δημιουργήσετε μια ουρά
Το παρακάτω παράδειγμα δημιουργεί πρώτα μια σύνδεση με το χώρο αποθήκευσης Azure χρησιμοποιώντας το περιβάλλον λογαριασμού χώρου αποθήκευσης, το οποίο περιλαμβάνει το όνομα του λογαριασμού χώρου αποθήκευσης και το πλήκτρο πρόσβασης. Στη συνέχεια, κλήσεις [AzureStorageQueue νέα](http://msdn.microsoft.com/library/azure/dn806382.aspx) cmdlet για να δημιουργήσετε μια ουρά με το όνομα 'Όνομα_ουράς'.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Για πληροφορίες σχετικά με συνθήκες ονοματοθεσίας για την υπηρεσία ουράς Azure, ανατρέξτε στο θέμα [Ονομασία ουρές και μετα-δεδομένων](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Πώς μπορείτε να ανακτήσετε μια ουρά
Μπορείτε να ερωτήματος και να ανακτήσετε μια συγκεκριμένη ουρά ή μια λίστα με όλες τις ουρές σε ένα λογαριασμό του χώρου αποθήκευσης. Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να ανακτήσετε μια συγκεκριμένη ουρά χρησιμοποιώντας το cmdlet [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) .

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

Αν καλέσετε το cmdlet [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) χωρίς παραμέτρους, το λαμβάνει μια λίστα με όλες τις ουρές.

### <a name="how-to-delete-a-queue"></a>Πώς μπορείτε να διαγράψετε μια ουρά
Για να διαγράψετε μια ουρά και όλα τα μηνύματα που περιέχονται σε αυτό, καλέστε το cmdlet κατάργηση AzureStorageQueue. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να διαγράψετε μια συγκεκριμένη ουρά χρησιμοποιώντας το cmdlet κατάργηση AzureStorageQueue.

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### <a name="how-to-insert-a-message-into-a-queue"></a>Πώς να εισαγάγετε ένα μήνυμα σε μια ουρά
Για να εισαγάγετε ένα μήνυμα σε μια ήδη υπάρχουσα ουρά, πρέπει πρώτα να δημιουργήσετε μια νέα παρουσία της κλάσης [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . Στη συνέχεια, καλέστε τη μέθοδο [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) . Μια CloudQueueMessage μπορεί να δημιουργηθεί από μια συμβολοσειρά (σε μορφή UTF-8) ή έναν πίνακα byte.

Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να προσθέσετε το μήνυμα σε μια ουρά. Το παράδειγμα δημιουργεί πρώτα μια σύνδεση με το χώρο αποθήκευσης Azure χρησιμοποιώντας το περιβάλλον λογαριασμού χώρου αποθήκευσης, το οποίο περιλαμβάνει το όνομα του λογαριασμού χώρου αποθήκευσης και το πλήκτρο πρόσβασης. Στη συνέχεια, ανακτά την καθορισμένη ουρά χρησιμοποιώντας το cmdlet [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) . Εάν υπάρχει η ουρά, χρησιμοποιείται το cmdlet [New-αντικειμένου](http://technet.microsoft.com/library/hh849885.aspx) για να δημιουργήσετε μια παρουσία της κλάσης [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . Αργότερα, το παράδειγμα καλεί τη μέθοδο [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) σε αυτό το αντικείμενο μηνύματος για να το προσθέσετε σε μια ουρά. Ακολουθεί κώδικα το οποίο ανακτά μια ουρά και εισάγει το μήνυμα 'MessageInfo':

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }


#### <a name="how-to-de-queue-at-the-next-message"></a>Πώς μπορείτε να καταργήστε την ουρά του επόμενου μηνύματος
Καταργήστε την τον κωδικό ουρές σε ένα μήνυμα από μια ουρά σε δύο βήματα. Όταν καλείτε τη μέθοδο [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) , λαμβάνετε στο επόμενο μήνυμα σε μια ουρά. Ένα μήνυμα που επιστρέφονται από **GetMessage** δεν είναι ορατό σε οποιονδήποτε άλλο κωδικό ανάγνωση μηνυμάτων από αυτήν την ουρά. Για να ολοκληρώσετε την κατάργηση του μηνύματος από την ουρά, μπορείτε, επίσης, πρέπει να καλέσετε τη μέθοδο [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) . Αυτή η διαδικασία δύο βημάτων από την κατάργηση ενός μηνύματος διασφαλίζει ότι εάν αποτύχει η επεξεργασία ενός μηνύματος λόγω αποτυχία υλικού ή λογισμικού σας κώδικα, μια άλλη παρουσία του κωδικού σας μπορεί να λάβετε το ίδιο μήνυμα και να προσπαθήστε ξανά. Τον κωδικό κλήσεις **DeleteMessage** δεξιά μετά το μήνυμα έχει υποστεί επεξεργασία.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## <a name="how-to-manage-azure-file-shares-and-files"></a>Διαχείριση κοινόχρηστων αρχείων Azure και αρχείων
Azure χώρο αποθήκευσης αρχείων προσφέρει κοινόχρηστου χώρου αποθήκευσης για τις εφαρμογές που χρησιμοποιούν το τυπικό πρωτόκολλο Ερωτήσεων. Εικονικές μηχανές Windows Azure και τις υπηρεσίες cloud να κάνετε κοινή χρήση αρχείων δεδομένων σε όλα τα στοιχεία της εφαρμογής μέσω μονταρισμένο κοινόχρηστα στοιχεία και εφαρμογές στην εσωτερική εγκατάσταση να αποκτήσετε πρόσβαση σε αρχείο δεδομένων σε ένα κοινόχρηστο στοιχείο μέσω του API χώρου αποθήκευσης του αρχείου ή Azure PowerShell.

Για περισσότερες πληροφορίες σχετικά με την αποθήκευση αρχείων Azure, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αρχείων Azure στα Windows](storage-dotnet-how-to-use-files.md) και [Αρχείο υπηρεσίας REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Πώς μπορείτε να ορίσετε και τις αναλύσεις χώρου αποθήκευσης ερωτήματος
Μπορείτε να χρησιμοποιήσετε [Ανάλυση Azure χώρου αποθήκευσης](storage-analytics.md) για τη συλλογή μετρήσεις για τους λογαριασμούς Azure χώρου αποθήκευσης και δεδομένα σχετικά με τις προσκλήσεις που αποστέλλονται στο λογαριασμό σας χώρο αποθήκευσης του αρχείου καταγραφής. Μπορείτε να χρησιμοποιήσετε μετρικά αποθήκευσης για την παρακολούθηση της εύρυθμης λειτουργίας του ένα λογαριασμό του χώρου αποθήκευσης και καταγραφή χώρου αποθήκευσης για τη διάγνωση και αντιμετώπιση προβλημάτων με το λογαριασμό χώρου αποθήκευσης.
Από προεπιλογή, μετρικών αποθήκευσης που δεν είναι ενεργοποιημένη για τις υπηρεσίες σας χώρου αποθήκευσης. Μπορείτε να ενεργοποιήσετε την παρακολούθηση χρησιμοποιώντας την πύλη Azure ή το Windows PowerShell ή μέσω προγραμματισμού, χρησιμοποιώντας τη βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης. Καταγραφή αποθήκευσης συμβαίνει πλευρά του διακομιστή και σας δίνει τη δυνατότητα να λεπτομέρειες εγγραφής για επιτυχή και αποτυχημένων αιτήσεων στο λογαριασμό σας στο χώρο αποθήκευσης. Αυτά τα αρχεία καταγραφής σάς επιτρέπουν να δείτε τις λεπτομέρειες ανάγνωση, εγγραφή και διαγραφή λειτουργίες σε σχέση με τους πίνακές σας, ουρές, και αντικείμενα BLOB καθώς και τους λόγους για αιτήσεις αποτυχίας.

Για να μάθετε πώς μπορείτε να ενεργοποιήσετε και να προβάλετε μετρικά αποθήκευσης δεδομένων με χρήση του PowerShell, ανατρέξτε στο θέμα [πώς μπορείτε να ενεργοποιήσετε μετρικά αποθήκευσης με χρήση του PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Για να μάθετε πώς μπορείτε να ενεργοποιήσετε και να ανακτήσετε χώρο αποθήκευσης καταγραφή δεδομένων με χρήση του PowerShell, ανατρέξτε στο θέμα [πώς μπορείτε να ενεργοποιήσετε την καταγραφή χώρου αποθήκευσης με χρήση του PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) και [μπορείτε να βρείτε τα δεδομένα σας καταγραφής αποθήκευσης καταγραφής](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Για λεπτομερείς πληροφορίες σχετικά με τη χρήση μετρικών αποθήκευσης και καταγραφή χώρου αποθήκευσης για την αντιμετώπιση προβλημάτων του χώρου αποθήκευσης, ανατρέξτε στο θέμα [παρακολούθησης, διάγνωση, και αντιμετώπιση προβλημάτων του Microsoft Azure αποθήκευσης](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Πώς μπορείτε να διαχειριστείτε κοινόχρηστα Access υπογραφή (συσχετισμών Ασφαλείας) και είναι αποθηκευμένο πολιτική πρόσβασης
Κοινόχρηστη πρόσβαση υπογραφές είναι ένα σημαντικό τμήμα του μοντέλου ασφαλείας για οποιαδήποτε εφαρμογή χρησιμοποιεί χώρο αποθήκευσης Azure. Είναι χρήσιμες για την παροχή περιορισμένα δικαιώματα για το λογαριασμό χώρου αποθήκευσης στους υπολογιστές-πελάτες που δεν θα πρέπει να έχετε τον αριθμό-κλειδί λογαριασμού. Από προεπιλογή, μόνο ο κάτοχος του λογαριασμού χώρου αποθήκευσης μπορεί να αποκτήσετε πρόσβαση αντικείμενα blob, πίνακες και ουρές μέσα σε αυτόν το λογαριασμό. Εάν η υπηρεσία ή την εφαρμογή σας πρέπει να κάνετε αυτούς τους πόρους διαθέσιμη σε άλλα προγράμματα-πελάτες χωρίς κοινής χρήσης κλειδιού πρόσβασης, έχετε τρεις επιλογές:

- Ορισμός δικαιωμάτων ενός κοντέινερ για να επιτρέψετε ανώνυμη πρόσβαση για ανάγνωση για το κοντέινερ και τα αντικείμενα blob. Αυτό δεν επιτρέπεται για πίνακες ή ουρές.
- Χρησιμοποιήστε μια υπογραφή κοινόχρηστη πρόσβαση ότι επιχορηγήσεις περιορισμένα δικαιώματα πρόσβασης σε κοντέινερ, αντικείμενα blob, ουρές και πίνακες για ένα συγκεκριμένο χρονικό διάστημα.
- Χρησιμοποιήστε μια πολιτική αποθηκευμένες πρόσβασης για να αποκτήσετε ένα πρόσθετο επίπεδο έλεγχο υπογραφές κοινόχρηστη πρόσβαση για ένα κοντέινερ ή τα αντικείμενα blob, για μια ουρά ή για έναν πίνακα. Η πολιτική αποθηκευμένες πρόσβασης σας επιτρέπει να αλλάξετε την ώρα έναρξης, η ώρα λήξης ή δικαιωμάτων για μια υπογραφή, ή για να ανακαλέσετε την μετά από αυτό έχει εκδοθεί.

Μια υπογραφή κοινόχρηστη πρόσβαση μπορεί να είναι σε μία από δύο μορφές:

- **Ad hoc συσχετίσεις Ασφαλείας**: Όταν δημιουργείτε μια ad hoc συσχετίσεις Ασφαλείας, την ώρα έναρξης, ώρα λήξης και δικαιώματα για τους συσχετισμούς Ασφαλείας καθορίζονται στο URI συσχετισμών Ασφαλείας. Αυτός ο τύπος των συσχετισμών Ασφαλείας που μπορεί να δημιουργηθεί ένα κοντέινερ, blob, πίνακας ή ουρά και δεν είναι μη revokable.
- **Συσχετίσεις Ασφαλείας με πολιτική αποθηκευμένες πρόσβασης**: μια πολιτική αποθηκευμένες πρόσβασης έχει οριστεί σε ένα κοντέινερ πόρων ένα κοντέινερ αντικειμένων blob, πίνακας ή ουρά - και μπορείτε να το χρησιμοποιήσετε για να διαχειριστείτε τους περιορισμούς για μία ή περισσότερες υπογραφές κοινόχρηστη πρόσβαση. Όταν μπορείτε να συσχετίσετε ένα συσχετισμών Ασφαλείας με μια πολιτική αποθηκευμένες πρόσβασης, τις συσχετίσεις Ασφαλείας μεταβιβάζονται τους περιορισμούς - την ώρα έναρξης, η ώρα λήξης και - ορισμός δικαιωμάτων για την πολιτική αποθηκευμένες πρόσβασης. Αυτός ο τύπος των συσχετισμών Ασφαλείας είναι revokable.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση κοινόχρηστων Access υπογραφές (συσχετισμών Ασφαλείας)](storage-dotnet-shared-access-signature-part-1.md) και [Διαχείριση ανώνυμη πρόσβαση για ανάγνωση σε κοντέινερ και αντικείμενα BLOB](storage-manage-access-to-resources.md).

Στις επόμενες ενότητες, θα μάθετε πώς μπορείτε να δημιουργήσετε μια πολιτική κοινόχρηστη πρόσβαση υπογραφής διακριτικού και αποθηκευμένες πρόσβασης για Azure πίνακες. Azure PowerShell παρέχει παρόμοια cmdlet του για κοντέινερ, αντικείμενα BLOB και καθώς και ουρές. Για να εκτελέσετε τις δέσμες ενεργειών σε αυτήν την ενότητα, κάντε λήψη του [PowerShell Azure έκδοση 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) ή νεότερη έκδοση.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>Πώς μπορείτε να δημιουργήσετε ένα διακριτικό υπογραφής πρόσβασης σε κοινή χρήση βάσει πολιτικής
Χρησιμοποιήστε το cmdlet New-AzureStorageTableStoredAccessPolicy για να δημιουργήσετε μια νέα πολιτική αποθηκευμένες πρόσβασης. Στη συνέχεια, καλέστε το cmdlet [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) για να δημιουργήσετε έναν νέο κωδικό υπογραφή βασίζεται σε πολιτική κοινόχρηστη πρόσβαση για έναν πίνακα του Azure χώρου αποθήκευσης.

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### <a name="how-to-create-an-ad-hoc-non-revokable-shared-access-signature-token"></a>Πώς μπορείτε να δημιουργήσετε ένα ad hoc διακριτικό υπογραφής πρόσβασης σε κοινή χρήση (μη revokable)
Χρησιμοποιήστε το cmdlet [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) για να δημιουργήσετε μια νέα ad hoc (μη revokable) σε κοινή χρήση Access διακριτικό υπογραφής για έναν πίνακα αποθήκευσης Azure:

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### <a name="how-to-create-a-stored-access-policy"></a>Πώς μπορείτε να δημιουργήσετε μια πολιτική αποθηκευμένες πρόσβασης
Χρησιμοποιήστε το cmdlet New-AzureStorageTableStoredAccessPolicy για να δημιουργήσετε μια νέα πολιτική αποθηκευμένες πρόσβασης για ένα χώρο αποθήκευσης Azure πίνακα:

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### <a name="how-to-update-a-stored-access-policy"></a>Πώς μπορείτε να ενημερώσετε μια πολιτική αποθηκευμένες πρόσβασης
Χρησιμοποιήστε το cmdlet Set-AzureStorageTableStoredAccessPolicy για να ενημερώσετε μια υπάρχουσα πολιτική αποθηκευμένες πρόσβασης για ένα χώρο αποθήκευσης Azure πίνακα:

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### <a name="how-to-delete-a-stored-access-policy"></a>Πώς μπορείτε να διαγράψετε μια πολιτική αποθηκευμένες πρόσβασης
Χρησιμοποιήστε το cmdlet AzureStorageTableStoredAccessPolicy Κατάργηση για να διαγράψετε μια πολιτική πρόσβασης αποθηκευμένες σε ένα χώρο αποθήκευσης Azure πίνακα:

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx


## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης Azure για δημόσιους οργανισμούς ΗΠΑ και Κίνα Azure
Περιβάλλον Azure είναι μια ανεξάρτητη ανάπτυξη του Windows Azure, όπως [Azure για δημόσιους οργανισμούς για δημόσιους οργανισμούς η.π.α.](https://azure.microsoft.com/features/gov/), [AzureCloud για καθολική Azure](https://portal.azure.com)και [AzureChinaCloud για το οποίο διαχειρίζεται η 21vianet στην Κίνα Azure](http://www.windowsazure.cn/). Μπορείτε να αναπτύξετε νέο Azure περιβάλλοντα για δημόσιους οργανισμούς αμερικανικό και Azure Κίνα.

Για να χρησιμοποιήσετε το χώρο αποθήκευσης Azure με AzureChinaCloud, πρέπει να δημιουργήσετε ένα περιβάλλον αποθήκευσης που είναι συσχετισμένη με AzureChinaCloud. Ακολουθήστε τα παρακάτω βήματα για να ξεκινήσετε:

1.  Εκτελέστε το cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) για να δείτε τα διαθέσιμα Azure περιβάλλοντα:

    `Get-AzureEnvironment`

2.  Προσθήκη λογαριασμού Κίνα Azure με το Windows PowerShell:

    `Add-AzureAccount –Environment AzureChinaCloud`

3.  Δημιουργήστε ένα περιβάλλον του χώρου αποθήκευσης για ένα λογαριασμό AzureChinaCloud:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud

Για να χρησιμοποιήσετε το χώρο αποθήκευσης Azure με [Κυβέρνηση Azure των η.π.α.](https://azure.microsoft.com/features/gov/), πρέπει να ορίσετε ένα νέο περιβάλλον και, στη συνέχεια, να δημιουργήσετε ένα νέο χώρο αποθήκευσης στο περιβάλλον με αυτό το περιβάλλον:

1.  Εκτελέστε το cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) για να δείτε τα διαθέσιμα Azure περιβάλλοντα:

    `Get-AzureEnvironment`

2.  Προσθήκη λογαριασμού Azure ΜΑΣ για δημόσιους οργανισμούς με το Windows PowerShell:

    `Add-AzureAccount –Environment AzureUSGovernment`

3.  Δημιουργήστε ένα περιβάλλον του χώρου αποθήκευσης για ένα λογαριασμό AzureUSGovernment:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment

Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Οδηγός για προγραμματιστές του Microsoft Azure για δημόσιους οργανισμούς](../azure-government-developer-guide.md).
- [Επισκόπηση των διαφορών κατά τη δημιουργία μιας εφαρμογής στην Κίνα υπηρεσίας](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτόν τον οδηγό, έχετε μάθατε πώς μπορείτε να διαχειριστείτε το χώρο αποθήκευσης Azure με το Azure PowerShell. Παρακάτω δίνονται κάποια σχετικά άρθρα και πόρους για να μάθετε περισσότερα σχετικά με τους.

- [Τεκμηρίωση Azure χώρου αποθήκευσης](https://azure.microsoft.com/documentation/services/storage/)
- [Cmdlet του PowerShell Azure χώρου αποθήκευσης](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [Αναφορά του Windows PowerShell](https://msdn.microsoft.com/library/ms714469.aspx)

[Image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png
[Image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png
[Image3]: ./media/storage-powershell-guide-full/Blobdownload.png


[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
