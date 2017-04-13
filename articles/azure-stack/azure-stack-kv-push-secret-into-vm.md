<properties
    pageTitle="Ανάπτυξη μια Εικονική με το πιστοποιητικό χρησιμοποιώντας Azure στοίβας κλειδί θάλαμο | Microsoft Azure"
    description="Μάθετε τον τρόπο ανάπτυξης μια Εικονική και εισαγωγής ενός πιστοποιητικού από θάλαμο κλειδί στοίβας Azure"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>Δημιουργία ΣΠΣ και συμπεριλάβετε τα πιστοποιητικά που ανακτήθηκαν από θάλαμο αριθμού-κλειδιού

Σε στοίβα Azure, ΣΠΣ αναπτύσσονται μέσω της διαχείρισης πόρων Azure και τώρα, μπορείτε να αποθηκεύσετε τα πιστοποιητικά σε Azure στοίβας κλειδί θάλαμο. Στη συνέχεια, Azure στοίβας (Microsoft.Compute υπηρεσίας παροχής πόρων για να είναι συγκεκριμένες) προωθεί τους σε ΣΠΣ σας όταν αναπτύσσονται του ΣΠΣ. Τα πιστοποιητικά μπορεί να χρησιμοποιηθεί σε πολλά σενάρια, συμπεριλαμβανομένων SSL, κρυπτογράφησης και τον έλεγχο ταυτότητας βάσει πιστοποιητικού.

Χρησιμοποιώντας αυτήν τη μέθοδο, μπορείτε να διατηρήσετε το πιστοποιητικό ασφαλή. Είναι τώρα δεν στην εικόνα Εικονική ή στα αρχεία ρύθμισης παραμέτρων της εφαρμογής ή σε κάποια άλλη μη ασφαλή θέση. Με τη ρύθμιση πολιτικής κατάλληλα δικαιώματα πρόσβασης για το πλήκτρο θάλαμο, μπορείτε επίσης να ελέγξετε ποιος αποκτά πρόσβαση σε πιστοποιητικού. Ένα άλλο πλεονέκτημα είναι ότι μπορείτε να διαχειρίζεστε όλα τα πιστοποιητικά σας σε ένα σημείο στο Azure στοίβας κλειδί θάλαμο.

Ακολουθεί μια γρήγορη επισκόπηση της διαδικασίας:

-   Χρειάζεστε ένα πιστοποιητικό στη μορφή .pfx.

-   Δημιουργία ενός κλειδιού θάλαμο (χρησιμοποιώντας ένα πρότυπο ή την ακόλουθη δέσμη ενεργειών).

-   Βεβαιωθείτε ότι έχετε ενεργοποιήσει το διακόπτη EnabledForDeployment.

-   Αποστείλετε το πιστοποιητικό ως ένα μυστικό.

## <a name="deploying-vms"></a>Ανάπτυξη ΣΠΣ

Αυτήν τη δέσμη ενεργειών δείγμα δημιουργεί ένα πλήκτρο θάλαμο και, στη συνέχεια, αποθηκεύει ένα πιστοποιητικό που είναι αποθηκευμένα στο αρχείο .pfx σε έναν τοπικό κατάλογο, για να το πλήκτρο θάλαμο ως ένα μυστικό.

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

Το πρώτο τμήμα της δέσμης ενεργειών διαβάζει το αρχείο .pfx και, στη συνέχεια, αποθηκεύει το ως JSON ένα αντικείμενο με την κωδικοποίηση base64 περιεχομένου αρχείου. Στη συνέχεια, το αντικείμενο JSON είναι επίσης κωδικοποίηση base64.

Στη συνέχεια, δημιουργεί μια νέα ομάδα πόρων και, στη συνέχεια, δημιουργεί ένα πλήκτρο θάλαμο. Σημείωση η τελευταία παράμετρος στην εντολή Δημιουργία AzureKeyVault, '-EnabledForDeployment', που παρέχει πρόσβαση σε Azure (συγκεκριμένα για την υπηρεσία παροχής πόρων Microsoft.Compute) για να διαβάσετε απορρήτου από τον αριθμό-κλειδί φύλαξης για αναπτύξεις.

Την τελευταία εντολή αποθηκεύει απλώς το αντικείμενο JSON κωδικοποίηση base64 στο του κλειδιού θάλαμο ως ένα μυστικό.

Ακολουθεί δείγμα εξόδου από την προηγούμενη δέσμη ενεργειών:

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

Τώρα θα σας είναι έτοιμοι να αναπτύξετε ένα πρότυπο Εικονική. Σημείωση το URI της το μυστικό από την έξοδο (ανάλογα με την επισήμανση στο προηγούμενο αποτέλεσμα με πράσινο χρώμα).

Χρειάζεστε ένα πρότυπο που βρίσκεται εδώ. Οι παράμετροι του ειδικό ενδιαφέρον (εκτός από τις παραμέτρους συνήθη Εικονική) είναι το όνομα θάλαμο, την ομάδα των πόρων θάλαμο και το μυστικό URI. Φυσικά, μπορείτε επίσης να κάντε λήψη του από GitHub και να τροποποιήσετε σύμφωνα με τις ανάγκες.

Όταν έχει αναπτυχθεί αυτό Εικονική, Azure εισάγει το πιστοποιητικό σε η Εικονική.
Στα Windows, πιστοποιητικά στο αρχείο .pfx προστίθενται με το χωρίς δυνατότητα εξαγωγής ιδιωτικό κλειδί. Το πιστοποιητικό προστίθεται στη θέση του πιστοποιητικού LocalMachine, με το χώρο αποθήκευσης πιστοποιητικού που παρέχονται από το χρήστη. Στην Linux, το αρχείο πιστοποιητικού τοποθετείται κάτω από τον κατάλογο /var/lib/waagent, με το όνομα του αρχείου &lt;UppercaseThumbprint&gt;.crt για το X509 αρχείο πιστοποιητικού, και &lt;UppercaseThumbprint&gt;.prv για το ιδιωτικό κλειδί.
Και τα δύο από αυτά τα αρχεία είναι μορφοποιημένο .pem.

Η εφαρμογή συνήθως εντοπίζει το πιστοποιητικό χρησιμοποιώντας την αποτύπωση και δεν χρειάζεται τροποποίηση.

## <a name="retiring-certificates"></a>Απόσυρση πιστοποιητικών


Στην προηγούμενη ενότητα, θα σας εμφάνιζε τρόπος για να προωθήσετε ένα νέο πιστοποιητικό για την υπάρχουσα ΣΠΣ. Αλλά το παλιό πιστοποιητικό είναι ακόμη σε η Εικονική και δεν είναι δυνατό να καταργηθούν. Για πρόσθετη ασφάλεια, μπορείτε να αλλάξετε το χαρακτηριστικό για παλιό μυστικό 'Απενεργοποιημένη', ώστε να ακόμα και εάν ένα παλιό πρότυπο προσπαθεί να δημιουργήσετε μια Εικονική με αυτήν την παλιά έκδοση του πιστοποιητικού, θα. Δείτε τώρα πώς μπορείτε να ορίσετε μια συγκεκριμένη έκδοση μυστικού να απενεργοποιηθεί:

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>Ολοκλήρωση


Με αυτήν τη νέα μέθοδο, το πιστοποιητικό μπορεί να διατηρούνται ξεχωριστά από την εικόνα εικονική Μηχανή ή το φορτίο εφαρμογής. Επομένως, θα σας έχουν καταργηθεί ένα σημείο της έκθεσης.

Το πιστοποιητικό μπορεί να είναι επίσης ανανεώνεται και αποστολή του σε θάλαμο κλειδί χωρίς να χρειάζεται να δημιουργήσετε ξανά την εικόνα Εικονική ή του πακέτου ανάπτυξης εφαρμογών. Η εφαρμογή εξακολουθεί να πρέπει να παρέχεται μέσω με το νέο URI για αυτήν την έκδοση νέου πιστοποιητικού.

Διαχωρίζοντας το πιστοποιητικό από την εικονική Μηχανή ή το φορτίο εφαρμογής, θα σας έχει περιορισμένες τώρα ο αριθμός του προσωπικού που θα έχει άμεση πρόσβαση στο πιστοποιητικό.

Ως ένα πρόσθετο πλεονέκτημα, τώρα έχετε ένα κατάλληλο σημείο στο θάλαμο αριθμού-κλειδιού για να διαχειριστείτε όλα τα πιστοποιητικά σας, συμπεριλαμβανομένου του όλες τις εκδόσεις που είχαν αναπτυχθεί μέσα στο χρόνο.

## <a name="next-steps"></a>Επόμενα βήματα

[Ανάπτυξη μια Εικονική με κωδικό πρόσβασης θάλαμο αριθμού-κλειδιού](azure-stack-kv-deploy-vm-with-secret.md)

[Να επιτρέπεται σε μια εφαρμογή για να αποκτήσετε πρόσβαση θάλαμο αριθμού-κλειδιού](azure-stack-kv-sample-app.md)