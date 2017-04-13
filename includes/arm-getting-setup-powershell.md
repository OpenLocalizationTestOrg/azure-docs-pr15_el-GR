## <a name="setting-up-powershell-for-resource-manager-templates"></a>Τη ρύθμιση του PowerShell για πρότυπα διαχείρισης πόρων

Για να χρησιμοποιήσετε Azure PowerShell με το διαχειριστή πόρων, θα πρέπει να έχετε το δικαίωμα του Windows PowerShell και εκδόσεις Azure PowerShell.

### <a name="verify-powershell-versions"></a>Επαλήθευση του PowerShell εκδόσεις

Επαλήθευση έχετε την έκδοση 3.0 ή 4.0 του Windows PowerShell. Για να βρείτε την έκδοση του Windows PowerShell, πληκτρολογήστε αυτήν την εντολή στη γραμμή εντολών του Windows PowerShell.

    $PSVersionTable

Θα εμφανιστεί ο παρακάτω τύπος των πληροφοριών:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Βεβαιωθείτε ότι η τιμή του **PSVersion** είναι 3.0 ή 4.0. Εάν όχι, ανατρέξτε στο θέμα [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) ή [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Ορίσετε λογαριασμός Azure και συνδρομή

Εάν δεν έχετε ήδη μια συνδρομή του Azure, μπορείτε να ενεργοποιήσετε το [MSDN συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ή το σύμβολο προς τα επάνω για μια [δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/).

Ανοίξτε μια γραμμή εντολών του PowerShell Azure και συνδεθείτε στο Azure με αυτήν την εντολή.

    Login-AzureRmAccount

Εάν έχετε πολλές συνδρομές Azure, μπορείτε να εμφανίσετε τις συνδρομές σας Azure με αυτήν την εντολή.

    Get-AzureRmSubscription

Θα εμφανιστεί ο παρακάτω τύπος των πληροφοριών:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Μπορείτε να ορίσετε την τρέχουσα συνδρομή σας Azure, εκτελώντας αυτές οι εντολές στη γραμμή εντολών του PowerShell Azure. Αντικαταστήστε τα πάντα μέσα σε τις προσφορές, συμπεριλαμβανομένων του < και > χαρακτήρες, με το σωστό όνομα.

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Για περισσότερες πληροφορίες σχετικά με τις συνδρομές του Azure και λογαριασμούς, ανατρέξτε στο θέμα [ΔΙΑΔΙΚΑΣΙΕΣ: σύνδεση με τη συνδρομή σας](powershell-install-configure.md#Connect).
