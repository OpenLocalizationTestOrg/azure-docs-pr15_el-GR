<properties 
    pageTitle="Έναρξη και διακοπή εικονικές μηχανές με αυτοματοποίηση Azure - ροής εργασίας PowerShell | Microsoft Azure"
    description="Η έκδοση του Azure αυτοματισμού σενάριο, συμπεριλαμβανομένων των runbooks για να ξεκινήσετε και να διακόψετε την κλασική εικονικές μηχανές γραφικών."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Σενάριο αυτοματισμού Azure - Έναρξη και διακοπή εικονικές μηχανές

Αυτό το σενάριο αυτοματισμού Azure περιλαμβάνει runbooks για να ξεκινήσετε και να διακόψετε την κλασική εικονικές μηχανές.  Μπορείτε να χρησιμοποιήσετε αυτό το σενάριο για οποιοδήποτε από τα εξής:  

- Χρησιμοποιήστε το runbooks χωρίς να τροποποιηθεί στο δικό σας περιβάλλον. 
- Τροποποιήστε το runbooks για την εκτέλεση προσαρμοσμένων λειτουργικότητα.  
- Καλέστε το runbooks από μια άλλη runbook ως μέρος μια συνολική λύση. 
- Χρησιμοποιήστε το runbooks ως προγράμματα εκμάθησης για να μάθετε runbook σύνταξης έννοιες. 

> [AZURE.SELECTOR]
- [Γραφικά](automation-solution-startstopvm-graphical.md)
- [Ροή εργασίας του PowerShell](automation-solution-startstopvm-psworkflow.md)

Αυτή είναι η έκδοση runbook PowerShell ροής εργασίας από αυτό το σενάριο. Είναι επίσης διαθέσιμο με χρήση [γραφικών runbooks](automation-solution-startstopvm-graphical.md).

## <a name="getting-the-scenario"></a>Γρήγορα το σενάριο

Αυτό το σενάριο αποτελείται από δύο runbooks PowerShell ροής εργασίας που μπορείτε να κάνετε λήψη από τις παρακάτω συνδέσεις.  Δείτε την [έκδοση γραφικών](automation-solution-startstopvm-graphical.md) του αυτό το σενάριο για συνδέσεις για τα γραφικά runbooks.

| Runbook | Σύνδεση | Τύπος | Περιγραφή |
|:---|:---|:---|:---|
| Έναρξη-AzureVMs | [Έναρξη Azure κλασική ΣΠΣ](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | Ροή εργασίας του PowerShell | Ξεκινά όλες οι κλασική εικονικές μηχανές σε ένα Azure subscriptionor όλες οι εικονικές μηχανές με όνομα συγκεκριμένης υπηρεσίας. |
| Διακοπή AzureVMs | [Διακοπή Azure κλασική ΣΠΣ](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | Ροή εργασίας του PowerShell | Διακόπτει όλες οι εικονικές μηχανές σε ένα λογαριασμό αυτοματισμού ή όλες οι εικονικές μηχανές με όνομα συγκεκριμένης υπηρεσίας.  |


## <a name="installing-and-configuring-the-scenario"></a>Εγκατάσταση και ρύθμιση παραμέτρων του σεναρίου

### <a name="1-install-the-runbooks"></a>1. Εγκαταστήστε το runbooks

Μετά τη λήψη του runbooks, μπορείτε να εισαγάγετε τους χρησιμοποιώντας τη διαδικασία εισαγωγής [ενός Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).

### <a name="2-review-the-description-and-requirements"></a>2. Ελέγξτε τις απαιτήσεις και περιγραφή
Το runbooks περιλαμβάνει κείμενο σχόλια βοήθειας που περιλαμβάνει μια περιγραφή και απαιτούμενα στοιχεία.  Μπορείτε επίσης να λάβετε τις ίδιες πληροφορίες από αυτό το άρθρο. 

### <a name="3-configure-assets"></a>3. ρύθμιση παραμέτρων περιουσιακών στοιχείων
Το runbooks απαιτεί τα εξής στοιχεία που πρέπει να δημιουργήσετε και να συμπληρώσετε με τις κατάλληλες τιμές.

| Τύπος περιουσιακών στοιχείων | Όνομα περιουσιακών στοιχείων | Περιγραφή |
|:---|:---|:---|:---|
| Διαπιστευτήρια | AzureCredential | Περιέχει τα διαπιστευτήρια για ένα λογαριασμό που έχει αρχή για έναρξη και διακοπή εικονικές μηχανές στην Azure συνδρομής.  Εναλλακτικά, μπορείτε να καθορίσετε μια άλλη διαπιστευτηρίων περιουσιακών στοιχείων στην παράμετρο **διαπιστευτηρίων** της δραστηριότητας **Προσθήκη AzureAccount** . |
| Μεταβλητή | AzureSubscriptionId | Περιέχει το Αναγνωριστικό συνδρομής του Azure τη συνδρομή σας. |

## <a name="using-the-scenario"></a>Χρησιμοποιώντας το σενάριο

### <a name="parameters"></a>Παράμετροι

Το runbooks έχουν τις παρακάτω παραμέτρους.  Πρέπει να δώσετε τιμές για οποιαδήποτε υποχρεωτικές παράμετροι και προαιρετικά να παράσχετε τιμές για τις άλλες παραμέτρους ανάλογα με τις απαιτήσεις σας.

| Παράμετρος | Τύπος | Υποχρεωτική | Περιγραφή |
|:---|:---|:---|:---|
| Όνομα_υπηρεσίας | συμβολοσειρά | Όχι | Εάν παρέχεται μια τιμή, στη συνέχεια, όλες οι εικονικές μηχανές με αυτό το όνομα υπηρεσίας είναι αποτελέσματα ή να διακοπεί.  Εάν παρέχεται καμία τιμή, στη συνέχεια, όλες οι κλασική εικονικές μηχανές στην Azure συνδρομής είναι αποτελέσματα ή να διακοπεί. |
| AzureSubscriptionIdAssetName | συμβολοσειρά | Όχι | Περιέχει το όνομα της [μεταβλητής περιουσιακών στοιχείων](#installing-and-configuring-the-scenario) που περιέχει το Αναγνωριστικό συνδρομής του Azure τη συνδρομή σας.  Εάν δεν μπορείτε να καθορίσετε μια τιμή, χρησιμοποιείται *AzureSubscriptionId* .  |
| AzureCredentialAssetName | συμβολοσειρά | Όχι | Περιέχει το όνομα του [περιουσιακού στοιχείου διαπιστευτηρίων](#installing-and-configuring-the-scenario) που περιέχει τα διαπιστευτήρια για runbook για να χρησιμοποιήσετε.  Εάν δεν μπορείτε να καθορίσετε μια τιμή, χρησιμοποιείται *AzureCredential* .  |

### <a name="starting-the-runbooks"></a>Έναρξη του runbooks

Μπορείτε να χρησιμοποιήσετε οποιαδήποτε από τις μεθόδους κατά την [εκκίνηση ενός runbook στο Azure αυτοματισμού](automation-starting-a-runbook.md) για να ξεκινήσετε ένα από τα runbooks σε αυτό το σενάριο.

Τα ακόλουθα δείγματα εντολών χρησιμοποιεί Windows PowerShell για να εκτελέσετε **StartAzureVMs** για να ξεκινήσετε όλες οι εικονικές μηχανές με το όνομα της υπηρεσίας *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Εξόδου

Το runbooks θα [εξόδου ένα μήνυμα](automation-runbook-output-and-messages.md) για κάθε εικονικό μηχάνημα που υποδεικνύει την έναρξη ή διακοπή οδηγία υποβλήθηκε με επιτυχία ή όχι.  Μπορείτε να αναζητήσετε μια συγκεκριμένη συμβολοσειρά στο αποτέλεσμα για να προσδιορίσετε το αποτέλεσμα για κάθε runbook.  Οι πιθανές εξόδου συμβολοσειρές παρατίθενται στον παρακάτω πίνακα.

| Runbook | Η συνθήκη | Μήνυμα |
|:---|:---|:---|
| Έναρξη-AzureVMs | Εικονική μηχανή εκτελείται ήδη  | MyVM εκτελείται ήδη |
| Έναρξη-AzureVMs | Έναρξη της αίτησης για εικονική μηχανή υποβληθεί με επιτυχία | MyVM έχει ξεκινήσει η διαδικασία |
| Έναρξη-AzureVMs | Απέτυχε η αίτηση έναρξης για εικονική μηχανή  | Απέτυχε η MyVM για να ξεκινήσετε |
| Διακοπή AzureVMs | Εικονική μηχανή έχει ήδη διακοπεί  | MyVM ήδη έχει διακοπεί |
| Διακοπή AzureVMs | Διακοπή αίτησης για εικονική μηχανή υποβληθεί με επιτυχία | MyVM έχει διακοπεί |
| Διακοπή AzureVMs | Απέτυχε η αίτηση διακοπής για εικονική μηχανή  | Απέτυχε η MyVM για να διακόψετε την |

Για παράδειγμα, το παρακάτω τμήμα κώδικα από μια runbook επιχειρεί να ξεκινήσει όλες οι εικονικές μηχανές με το όνομα της υπηρεσίας *MyServiceName*.  Εάν κάποιο από τα ΑΠΟΤΥΧΙΑ αιτήσεις Έναρξη, στη συνέχεια, μπορούν να ληφθούν ενέργειες σφάλματος. 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Λεπτομερή ανάλυση

Ακολουθεί μια λεπτομερή ανάλυση της το runbooks σε αυτό το σενάριο.  Μπορείτε να χρησιμοποιήσετε αυτές τις πληροφορίες για να προσαρμόσετε το runbooks ή μόνο για να μάθετε από αυτά για σύνταξη από κοινού το δικό σας σενάρια αυτοματισμού.

### <a name="parameters"></a>Παράμετροι

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

Η ροή εργασίας ξεκινά από γρήγορα τις τιμές για τις [παραμέτρους εισόδου](#using-the-scenario).  Εάν δεν παρέχονται τα ονόματα των διαθέσιμων στοιχείων, στη συνέχεια, τα προεπιλεγμένα ονόματα που χρησιμοποιούνται.

### <a name="output"></a>Εξόδου

    # Returns strings with status messages
    [OutputType([String])]

Αυτή η γραμμή δηλώνει ότι το αποτέλεσμα του runbook θα είναι μια συμβολοσειρά.  Αυτό δεν απαιτείται αλλά είναι βέλτιστη πρακτική για όταν runbook χρησιμοποιείται ως ένα [θυγατρικό runbook](automation-child-runbooks.md) ώστε μια γονικής runbook θα γνωρίζουν ότι ο τύπος εξόδου πρέπει να περιμένουν.

### <a name="authentication"></a>Έλεγχος ταυτότητας

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

Τις επόμενες γραμμές ορίστε τα [διαπιστευτήρια](automation-configuring.md#configuring-authentication-to-azure-resources) και Azure συνδρομή που θα χρησιμοποιηθεί για τα υπόλοιπα του runbook.
Πρώτα χρησιμοποιούμε **Get-AutomationPSCredential** για να λάβετε το περιουσιακών στοιχείων που διατηρεί τα διαπιστευτήρια με πρόσβαση σε έναρξη και διακοπή εικονικές μηχανές στην Azure συνδρομής. Στη συνέχεια, **Προσθήκη AzureAccount** χρησιμοποιεί αυτού του περιουσιακού στοιχείου για να ορίσει τα διαπιστευτήρια.  Το αποτέλεσμα είναι που έχουν εκχωρηθεί σε μια εικονική μεταβλητή, έτσι ώστε το δεν περιλαμβάνεται στο αποτέλεσμα του runbook.  

Η μεταβλητή περιουσιακών στοιχείων με το Αναγνωριστικό συνδρομής, στη συνέχεια, ανάκτηση με **Get-AutomationVariable** και τη συνδρομή που έχουν οριστεί με **Επιλογή AzureSubscription**.

### <a name="get-vms"></a>Λήψη ΣΠΣ

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**Get-AzureVM** χρησιμοποιείται για να ανακτήσετε τις εικονικές μηχανές runbook θα λειτουργούν με το.  Εάν μια τιμή παρέχεται στη μεταβλητή εισαγωγής **όνομα_υπηρεσίας** , στη συνέχεια, ανακτώνται μόνο τις εικονικές μηχανές με το ίδιο όνομα υπηρεσίας.  Εάν **όνομα_υπηρεσίας** είναι κενό, όλες οι εικονικές μηχανές ανακτώνται.

### <a name="startstop-virtual-machines-and-send-output"></a>Έναρξη/διακοπή εικονικές μηχανές και αποστολή εξόδου

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

Το επόμενο βήμα γραμμές μέσω κάθε εικονική μηχανή.  Πρώτα το **PowerState** της η εικονική μηχανή είναι επιλεγμένο το στοιχείο για να δείτε εάν εκτελείται ήδη ή να διακοπεί, ανάλογα με το runbook.  Αν είναι ήδη σε κατάσταση προορισμού, στη συνέχεια, ένα μήνυμα αποστέλλεται εξόδου και τα άκρα του runbook.  Εάν όχι, τότε χρησιμοποιείται **AzureVM Έναρξη** ή **Διακοπή AzureVM** προσπαθεί να εκκινήσει ή να διακόψει την εικονική μηχανή με το αποτέλεσμα της αίτησης είναι αποθηκευμένα σε μια μεταβλητή.  Στη συνέχεια, αποστέλλεται ένα μήνυμα για να εξαγάγετε που καθορίζει αν η αίτηση για την έναρξη ή διακοπή υποβλήθηκε με επιτυχία.


## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε περισσότερα σχετικά με την εργασία με το παιδί runbooks, ανατρέξτε στο θέμα [runbooks θυγατρικό στο Azure Automation](automation-child-runbooks.md) 
- Για να μάθετε περισσότερα σχετικά με τα μηνύματα εξόδου κατά τη διάρκεια της εκτέλεσης runbook και να σας βοηθήσει να αντιμετωπίσετε καταγραφή, ανατρέξτε στο θέμα [Runbook εξόδου και μηνύματα σε αυτοματισμού Azure](automation-runbook-output-and-messages.md)
