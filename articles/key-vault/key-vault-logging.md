<properties
    pageTitle="Καταγραφή Azure κλειδιού θάλαμο | Microsoft Azure"
    description="Χρησιμοποιήστε αυτό το πρόγραμμα εκμάθησης για γρήγορα αποτελέσματα με το Azure κλειδί θάλαμο καταγραφή."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/31/2016"
    ms.author="cabailey"/>

# <a name="azure-key-vault-logging"></a>Καταγραφή Azure θάλαμο κλειδιού #
Azure θάλαμο κλειδί είναι διαθέσιμη στις περισσότερες περιοχές. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα το [κλειδί θάλαμο τιμολόγησης σελίδας](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Εισαγωγή  
Αφού δημιουργήσετε ένα ή περισσότερα κλειδιού χώροι φύλαξης, μπορεί να θέλετε να παρακολουθείτε πώς και πότε το κλειδί χώροι φύλαξης έχουν πρόσβαση, και από ποιον. Μπορείτε να το κάνετε ενεργοποιώντας την καταγραφή θάλαμο αριθμού-κλειδιού, η οποία αποθηκεύει τις πληροφορίες σε ένα λογαριασμό Azure χώρου αποθήκευσης που παρέχετε. Ένα νέο κοντέινερ που ονομάζεται **ιδέες-αρχεία καταγραφής-auditevent** δημιουργείται αυτόματα για το λογαριασμό σας καθορισμένο χώρου αποθήκευσης και μπορείτε να χρησιμοποιήσετε το ίδιο λογαριασμό χώρου αποθήκευσης για τη συλλογή αρχείων καταγραφής για πολλές βασικές χώροι φύλαξης.

Μπορείτε να αποκτήσετε πρόσβαση τις πληροφορίες των αρχείων καταγραφής πολύ, 10 λεπτά μετά από τον αριθμό-κλειδί φύλαξης λειτουργία. Στις περισσότερες περιπτώσεις, θα είναι ταχύτερα από αυτό.  Είναι προς τα επάνω για να διαχειριστείτε τα αρχεία καταγραφής στο λογαριασμό σας στο χώρο αποθήκευσης:

- Χρησιμοποιήστε μεθόδους ελέγχου τυπική Azure πρόσβασης για την ασφάλιση τα αρχεία καταγραφής ο περιορισμός ποιος μπορεί να αποκτήσετε πρόσβαση σε αυτά.
- Διαγραφή αρχείων καταγραφής που δεν θέλετε πλέον να διατηρήσετε στο λογαριασμό σας στο χώρο αποθήκευσης.

Χρησιμοποιήστε αυτό το πρόγραμμα εκμάθησης για να σας βοηθήσει να ξεκινήσετε με το Azure κλειδί θάλαμο καταγραφή, για να δημιουργήσετε το λογαριασμό χώρου αποθήκευσης και ενεργοποιήσετε την καταγραφή ερμηνεύσετε τις πληροφορίες σύνδεσης που έχουν συλλεχθεί.  


>[AZURE.NOTE]  Αυτό το πρόγραμμα εκμάθησης δεν περιλαμβάνει οδηγίες για τον τρόπο δημιουργίας βασικών χώροι φύλαξης, πλήκτρα ή απορρήτου. Για αυτές τις πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure κλειδί θάλαμο](key-vault-get-started.md). Ή, για το περιβάλλον γραμμής εντολών πλατφόρμες οδηγίες, ανατρέξτε στο θέμα [αυτό το πρόγραμμα εκμάθησης ισοδύναμο](key-vault-manage-with-cli.md).
>
>Προς το παρόν, δεν μπορείτε να ρυθμίσετε θάλαμο κλειδί Azure στην πύλη του Azure. Εναλλακτικά, χρησιμοποιήστε αυτές τις οδηγίες Azure PowerShell.

Τα αρχεία καταγραφής που μπορείτε να συλλέξετε μπορούν να απεικονιστούν χρησιμοποιώντας ανάλυση καταγραφής από την οικογένεια προγραμμάτων διαχείρισης λειτουργίες. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [λύσης Azure θάλαμο αριθμού-κλειδιού (έκδοση Preview) στο αρχείο καταγραφής αναλυτικών στοιχείων](../log-analytics/log-analytics-azure-key-vault.md).

Για επισκόπηση πληροφορίες σχετικά με το Azure κλειδί θάλαμο, ανατρέξτε στο θέμα [Τι είναι το Azure κλειδί θάλαμο;](key-vault-whatis.md)

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- Μια υπάρχουσα κλειδιού θάλαμο που χρησιμοποιείτε.  
- Azure PowerShell, την **Ελάχιστη έκδοση του 1.0.1**. Για να εγκαταστήσετε Azure PowerShell και να συσχετίσετε με τη συνδρομή σας στο Azure, δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md). Εάν έχετε ήδη εγκαταστήσει το Azure PowerShell και δεν γνωρίζετε την έκδοση, από την κονσόλα του PowerShell Azure, πληκτρολογήστε `(Get-Module azure -ListAvailable).Version`.  
- Τα αρχεία καταγραφής αρκετό χώρο αποθήκευσης στη Azure για το πλήκτρο θάλαμο.


## <a id="connect"></a>Σύνδεση με τις συνδρομές σας ##

Ξεκινήστε μια περίοδο λειτουργίας Azure PowerShell και πραγματοποιήστε είσοδο στο λογαριασμό σας Azure με την ακόλουθη εντολή:  

    Login-AzureRmAccount

Στο παράθυρο περιήγησης αναδυόμενο, εισαγάγετε το όνομα χρήστη λογαριασμός Azure και τον κωδικό πρόσβασης. Θα σας βοηθήσουν όλες τις συνδρομές που σχετίζονται με αυτόν το λογαριασμό και από προεπιλογή, χρησιμοποιεί το πρώτο Azure PowerShell.

Εάν έχετε πολλές συνδρομές, ίσως χρειαστεί να καθορίσετε κάποια συγκεκριμένη που χρησιμοποιήθηκε για τη δημιουργία του Azure θάλαμο αριθμού-κλειδιού. Πληκτρολογήστε τα εξής για να δείτε τις συνδρομές για το λογαριασμό σας:

    Get-AzureRmSubscription

Στη συνέχεια, για να καθορίσετε τη συνδρομή που είναι συσχετισμένη με το πλήκτρο θάλαμο που θα η καταγραφή, πληκτρολογήστε:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).


## <a id="storage"></a>Δημιουργία νέου λογαριασμού χώρου αποθήκευσης για τα αρχεία καταγραφής ##

Παρόλο που μπορείτε να χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης για τα αρχεία καταγραφής, θα δημιουργήσουμε ένα νέο λογαριασμό του χώρου αποθήκευσης που θα είναι αφοσιωμένη στο να θάλαμο κλειδί αρχεία καταγραφής. Για τη διευκόλυνσή σας για το πότε πρέπει να καθορίσετε αργότερα, θα αποθηκεύει τις λεπτομέρειες σε μεταβλητή που ονομάζεται **σα**.

Για πρόσθετη ευκολία στη διαχείριση, θα επίσης χρησιμοποιήσουμε την ίδια ομάδα των πόρων που περιέχει το πλήκτρο θάλαμο. Από το παράθυρο [Γρήγορα αποτελέσματα πρόγραμμα εκμάθησης](key-vault-get-started.md), αυτήν την ομάδα πόρων ονομάζεται **ContosoResourceGroup** και θα συνεχίσουμε να χρησιμοποιήσετε τη θέση Ανατολικής Ασίας. Αντικαταστήστε αυτές τις τιμές για το δικό σας, ανάλογα με την περίπτωση:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name ContosoKeyVaultLogs -Type Standard_LRS -Location 'East Asia'


>[AZURE.NOTE]  Εάν αποφασίσετε να χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης, πρέπει να χρησιμοποιήσει την ίδια συνδρομή ως σας κλειδιού θάλαμο και πρέπει να χρησιμοποιεί το μοντέλο ανάπτυξης διαχείρισης πόρων και όχι το μοντέλο ανάπτυξης κλασική.

## <a id="identify"></a>Προσδιορισμός του κλειδιού θάλαμο για τα αρχεία καταγραφής ##

Σε μας γρήγορα αποτελέσματα πρόγραμμα εκμάθησης, το όνομα κλειδιού θάλαμο ήταν **ContosoKeyVault**, έτσι θα συνεχίσουμε να χρησιμοποιήσετε αυτό το όνομα και να αποθηκεύσετε τις λεπτομέρειες σε μεταβλητή που ονομάζεται **kv**:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Ενεργοποίηση καταγραφής ##

Για να ενεργοποιήσετε την καταγραφή για θάλαμο αριθμού-κλειδιού, θα χρησιμοποιήσουμε το cmdlet Set-AzureRmDiagnosticSetting, μαζί με τις μεταβλητές που δημιουργήσαμε για το νέο λογαριασμό του χώρου αποθήκευσης και μας κλειδιού θάλαμο. Επίσης, θα σας θα ορίσετε το **-με δυνατότητα** σημαία για να **$true** και ορίστε την κατηγορία σε AuditEvent (το μόνο κατηγορία για καταγραφή θάλαμο κλειδί),:


    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

Το αποτέλεσμα για αυτό περιλαμβάνει τα εξής:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


Αυτό επιβεβαιώνει ότι καταγραφής τώρα είναι ενεργοποιημένη για τις βασικές θάλαμο, αποθήκευση πληροφοριών για το λογαριασμό χώρου αποθήκευσης.

Προαιρετικά μπορείτε επίσης να ορίσετε την πολιτική διατήρησης για τα αρχεία καταγραφής ώστε να παλαιότερα αρχεία καταγραφής θα διαγραφούν αυτόματα. Για παράδειγμα, Ορισμός πολιτικής διατήρησης με σημαία **- RetentionEnabled** **$true** και ρυθμίσετε την παράμετρο **- RetentionInDays** **90** , έτσι ώστε τα αρχεία καταγραφής που είναι παλαιότερα από 90 ημερών θα διαγραφούν αυτόματα.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Τι είναι συνδεδεμένος:

- Είστε συνδεδεμένοι όλες τις αιτήσεις REST API με έλεγχο ταυτότητας, που περιλαμβάνει αποτυχημένων αιτήσεων ως αποτέλεσμα τα δικαιώματα πρόσβασης, σφάλματα συστήματος ή εσφαλμένες αιτήσεις.
- Λειτουργίες του κλειδιού φύλαξης μόνη της, που περιλαμβάνει τη δημιουργία, διαγραφή, πολιτικές πρόσβασης κλειδιού θάλαμο ρύθμιση, και ενημέρωση κλειδιού θάλαμο χαρακτηριστικά όπως είναι οι ετικέτες.
- Λειτουργίες σε πλήκτρα και απόρρητο στο του κλειδιού θάλαμο, που περιλαμβάνει τη δημιουργία, τροποποίηση ή διαγραφή αυτά τα πλήκτρα ή απόρρητο; λειτουργίες όπως εισόδου, επαληθεύστε, κρυπτογράφηση, αποκρυπτογράφηση, αναδίπλωση και χωρίς αναδίπλωση πλήκτρα, λάβετε απόρρητο, λίστα πλήκτρα και απορρήτου και τις εκδόσεις τους.
- Χωρίς έλεγχο ταυτότητας αιτήσεων που έχει ως αποτέλεσμα 401 απάντηση. Για παράδειγμα, αιτήσεων που δεν έχουν ένα διακριτικό φορέα, ή είναι ακατάλληλη ή έχει λήξει ή έχει ένα μη έγκυρο διακριτικό.  


## <a id="access"></a>Πρόσβαση σε σας αρχείων καταγραφής ##

Αρχεία καταγραφής κλειδιού θάλαμο αποθηκεύονται στο κοντέινερ **ιδέες-αρχεία καταγραφής-auditevent** στο λογαριασμό χώρου αποθήκευσης που παρέχονται. Για να δημιουργήσετε μια λίστα με όλα τα αντικείμενα BLOB σε αυτό το κοντέινερ, πληκτρολογήστε:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

Το αποτέλεσμα θα είναι κάτι παρόμοιο με αυτό:

**Uri κοντέινερ: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**


**Όνομα**

**----**

**resourceId = / ΣΥΝΔΡΟΜΈΣ/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/υπηρεσίες ΠΑΡΟΧΉΣ/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId = / ΣΥΝΔΡΟΜΈΣ/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/υπηρεσίες ΠΑΡΟΧΉΣ/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

** resourceId = / ΣΥΝΔΡΟΜΈΣ/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/υπηρεσίες ΠΑΡΟΧΉΣ/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json***


Όπως μπορείτε να δείτε από αυτό το αποτέλεσμα, τα αντικείμενα BLOB ακολουθήστε μια συνθήκη ονοματοθεσίας: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

Οι τιμές ημερομηνίας και ώρας Χρησιμοποιήστε UTC.

Επειδή ο ίδιος λογαριασμός χώρου αποθήκευσης μπορούν να χρησιμοποιηθούν για τη συλλογή αρχείων καταγραφής για πολλούς πόρους, το Αναγνωριστικό πόρου πλήρους στο όνομα του blob είναι πολύ χρήσιμο για την πρόσβαση ή λήψη μόνο τα αντικείμενα BLOB που χρειάζεστε. Αλλά πριν το κάνουμε, παρουσιάζεται πρώτα πώς μπορείτε να κάνετε λήψη όλων των blob του.

Πρώτα, δημιουργήστε ένα φάκελο για να κάνετε λήψη του αντικείμενα blob. Για παράδειγμα:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Στη συνέχεια, θα λάβετε μια λίστα όλων των BLOB:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Διοχέτευση αυτήν τη λίστα μέσω 'Get-AzureStorageBlobContent' για να κάνετε λήψη του αντικείμενα BLOB σε μας φάκελο προορισμού:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Κατά την εκτέλεση αυτής της εντολής δεύτερη, το **/** οριοθέτη στα ονόματα αντικειμένων blob, δημιουργήστε μια δομή πλήρους φακέλου κάτω από το φάκελο προορισμού και τη δομή αυτή θα χρησιμοποιηθεί για να κάνετε λήψη και να αποθηκεύσετε τα αντικείμενα BLOB ως αρχεία.

Για να λάβετε επιλεκτικής αντικείμενα blob, χρησιμοποιήστε χαρακτήρες μπαλαντέρ. Για παράδειγμα:

- Εάν έχετε πολλές βασικές χώροι φύλαξης και θέλετε να κάνετε λήψη αρχείων καταγραφής για ένα μόνο θάλαμο κλειδιού, με το όνομα CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3

- Εάν έχετε πολλές ομάδες πόρων και θέλετε να κάνετε λήψη αρχείων καταγραφής για μία μόνο ομάδα πόρων, χρησιμοποιήστε `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'

- Εάν θέλετε να κάνετε λήψη όλων των αρχείων καταγραφής για το μήνα Ιανουαρίου 2016, χρησιμοποιήστε `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Τώρα είστε έτοιμοι να ξεκινήσετε την αναζήτηση στο τι υπάρχει στα αρχεία καταγραφής. Αλλά πριν περάσετε στην οποία, δύο περισσότερες παραμέτρους για λήψη AzureRmDiagnosticSetting που ενδέχεται να πρέπει να γνωρίζετε:

- Ερώτημα για την κατάσταση των διαγνωστικών ρυθμίσεων για τον πόρο θάλαμο κλειδιού:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`

- Απενεργοποίηση της καταγραφής για τον πόρο θάλαμο κλειδιού:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`


## <a id="interpret"></a>Ερμηνεία τα αρχεία καταγραφής θάλαμο αριθμού-κλειδιού ##

Μεμονωμένα αντικείμενα BLOB αποθηκεύονται ως κείμενο, που έχει μορφοποιηθεί ως JSON blob. Αυτή είναι μια καταχώρηση αρχείου καταγραφής παράδειγμα εκτέλεση `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


Ο παρακάτω πίνακας παραθέτει τα ονόματα πεδίων και τις περιγραφές.


| Όνομα πεδίου        | Περιγραφή |
| ------------- |-------------|
| ώρα      | Ημερομηνία και ώρα (UTC).|
| resourceId      | Αναγνωριστικό Azure πόρου από διαχειριστή πόρων. Για τα αρχεία καταγραφής θάλαμο αριθμού-κλειδιού, αυτή είναι πάντα το αναγνωριστικό του αριθμού-κλειδιού θάλαμο πόρων.|
| operationName      | Όνομα της λειτουργίας, όπως περιγράφεται στον επόμενο πίνακα.|
| operationVersion      | Αυτή είναι η έκδοση REST API ζητήθηκε από το πρόγραμμα-πελάτη.|
| κατηγορία      | Για τα αρχεία καταγραφής θάλαμο αριθμού-κλειδιού, AuditEvent είναι η τιμή μεμονωμένο, διαθέσιμη.|
| resultType      | Αποτέλεσμα της αίτησης REST API.|
| resultSignature      | Κατάσταση HTTP.|
| resultDescription     | Επιπλέον περιγραφή σχετικά με το αποτέλεσμα, όταν είναι διαθέσιμες.|
| durationMs      | Χρόνος που χρειάστηκαν για την εξυπηρέτηση της αίτησης REST API, σε χιλιοστά του δευτερολέπτου. Αυτό δεν περιλαμβάνει την αδράνεια δικτύου, ώστε ο χρόνος που μετρούν στην πλευρά του προγράμματος-πελάτη δεν μπορεί να συμφωνούν με αυτήν τη στιγμή.|
| callerIpAddress      | Διεύθυνση IP του προγράμματος-πελάτη που έκανε την αίτηση.|
| correlationId      | Ένα προαιρετικό GUID που μπορεί να περάσει το πρόγραμμα-πελάτη για να συσχετίσετε αρχεία καταγραφής από την πλευρά του προγράμματος-πελάτη με αρχεία καταγραφής από την πλευρά του υπηρεσίας (πλήκτρο θάλαμο).|
| ταυτότητα      | Ταυτότητα από το διακριτικό που παρουσιάστηκε κατά την υποβολή της αίτησης REST API. Αυτό είναι συνήθως "χρήστης", "κεφάλαιο υπηρεσίας" ή ένα συνδυασμό "χρήστη + αναγνωριστικό εφαρμογής" όπως πεζών / κεφαλαίων σε μια αίτηση που προκύπτει από ένα cmdlet του Azure PowerShell.|
| Ιδιότητες      | Αυτό το πεδίο θα περιέχει διαφορετικές πληροφορίες με βάση τη λειτουργία (operationName). Στις περισσότερες περιπτώσεις, περιέχει πληροφορίες προγράμματος-πελάτη (συμβολοσειρά παράγοντα χρήστη που μεταβιβάζεται από το πρόγραμμα-πελάτη), την ακριβή REST API πρόσκληση URI και ο κωδικός κατάστασης HTTP. Επιπλέον, όταν ένα αντικείμενο επιστρέφεται ως αποτέλεσμα μια αίτηση (για παράδειγμα, KeyCreate ή VaultGet) το θα επίσης περιέχουν το κλειδί URI (όπως "αναγνωριστικό"), URI θάλαμο ή μυστικό URI.|




Οι τιμές πεδίου **operationName** είναι σε μορφή ObjectVerb. Για παράδειγμα:

- Όλες οι λειτουργίες κλειδιού θάλαμο έχουν το ' θάλαμο`<action>`' μορφοποίηση, όπως είναι οι `VaultGet` και `VaultCreate`.

- Όλες οι βασικές λειτουργίες έχουν το ' κλειδί`<action>`' μορφοποίηση, όπως είναι οι `KeySign` και `KeyList`.

- Όλες οι λειτουργίες μυστικού έχουν το ' μυστικό`<action>`' μορφοποίηση, όπως είναι οι `SecretGet` και `SecretListVersions`.

Ο παρακάτω πίνακας παραθέτει τις operationName και αντίστοιχη εντολή REST API.

| operationName        | Εντολή REST API |
| ------------- |-------------|
| Έλεγχος ταυτότητας      | Μέσω του τελικού σημείου Azure Active Directory|
| VaultGet      | [Λήψη πληροφοριών σχετικά με ένα πλήκτρο θάλαμο](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx)|
| VaultPut      | [Δημιουργία ή ενημέρωση ενός κλειδιού θάλαμο](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx)|
| VaultDelete      | [Διαγραφή ενός κλειδιού θάλαμο](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx)|
| VaultPatch      | [Ενημέρωση ενός κλειδιού θάλαμο](https://msdn.microsoft.com/library/azure/mt620025.aspx)|
| VaultList      | [Λίστα όλα χώροι φύλαξης κλειδιού σε μια ομάδα πόρων](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx)|
| KeyCreate      | [Δημιουργήστε έναν αριθμό-κλειδί](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx)|
| KeyGet      | [Λήψη πληροφοριών σχετικά με έναν αριθμό-κλειδί](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx)|
| KeyImport      | [Εισαγάγετε έναν αριθμό-κλειδί στο ένα θάλαμο](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx)|
| KeyBackup      | [Δημιουργία αντιγράφων ασφαλείας έναν αριθμό-κλειδί](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).|
| KeyDelete      | [Διαγράψτε έναν αριθμό-κλειδί](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx)|
| KeyRestore      | [Επαναφορά ενός κλειδιού](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx)|
| KeySign      | [Πραγματοποιήστε είσοδο με έναν αριθμό-κλειδί](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx)|
| KeyVerify      | [Επαλήθευση με αριθμό-κλειδί](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx)|
| KeyWrap      | [Αναδίπλωση έναν αριθμό-κλειδί](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx)|
| KeyUnwrap      | [Χωρίς αναδίπλωση έναν αριθμό-κλειδί](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx)|
| KeyEncrypt      | [Κρυπτογράφηση με έναν αριθμό-κλειδί](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx)|
| KeyDecrypt      | [Αποκρυπτογράφηση με αριθμό-κλειδί](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx)|
| KeyUpdate      | [Ενημέρωση έναν αριθμό-κλειδί](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx)|
| KeyList      | [Λίστα με τα πλήκτρα σε ένα θάλαμο](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx)|
| KeyListVersions      | [Λίστα με τις εκδόσεις ενός κλειδιού](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx)|
| SecretSet      | [Δημιουργήστε ένα μυστικό](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx)|
| SecretGet      | [Λήψη μυστικό](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx)|
| SecretUpdate      | [Ενημέρωση μιας μυστικό](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx)|
| SecretDelete      | [Διαγραφή ενός μυστικό](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx)|
| SecretList      | [Λίστα απόρρητο σε ένα θάλαμο](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx)|
| SecretListVersions      | [Λίστα εκδόσεις του ένα μυστικό](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx)|




## <a id="next"></a>Επόμενα βήματα ##

Για ένα πρόγραμμα εκμάθησης που χρησιμοποιεί Azure θάλαμο αριθμού-κλειδιού σε μια εφαρμογή web, ανατρέξτε στο θέμα [Χρήση Azure θάλαμο κλειδί από μια εφαρμογή Web](key-vault-use-from-web-application.md).

Για τον προγραμματισμό αναφορών, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές του Azure θάλαμο αριθμού-κλειδιού](key-vault-developers-guide.md).

Για μια λίστα των cmdlet του Azure PowerShell 1.0 για θάλαμο κλειδί Azure, ανατρέξτε στο θέμα [Azure κλειδί θάλαμο cmdlet](https://msdn.microsoft.com/library/azure/dn868052.aspx).

Για ένα πρόγραμμα εκμάθησης κλειδιού περιστροφής και αρχείου καταγραφής ελέγχου με θάλαμο κλειδί Azure, δείτε [πώς μπορείτε να ρύθμισης θάλαμο κλειδί με τον έλεγχο και τελικών κλειδιού περιστροφής](key-vault-key-rotation-log-monitoring.md).
