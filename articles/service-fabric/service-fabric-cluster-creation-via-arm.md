
<properties
   pageTitle="Δημιουργήστε ένα ασφαλές σύμπλεγμα ύφασμα υπηρεσία χρησιμοποιώντας τη διαχείριση πόρων Azure | Microsoft Azure"
   description="Σε αυτό το άρθρο περιγράφει πώς μπορείτε να ρυθμίσετε ένα σύμπλεγμα ασφαλούς ύφασμα υπηρεσίας στο Azure χρησιμοποιώντας Azure διαχείριση πόρων, θάλαμο κλειδί Azure και Azure Active Directory (AAD) για τον έλεγχο ταυτότητας του προγράμματος-πελάτη."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Δημιουργήστε ένα σύμπλεγμα ύφασμα υπηρεσίας στο Azure χρησιμοποιώντας τη διαχείριση πόρων Azure

> [AZURE.SELECTOR]
- [Azure από διαχειριστή πόρων](service-fabric-cluster-creation-via-arm.md)
- [Πύλη του Azure](service-fabric-cluster-creation-via-portal.md)

Πρόκειται για αναλυτικές οδηγίες που σας καθοδηγεί σε τα βήματα ρύθμισης σύμπλεγμα Azure ύφασμα υπηρεσίας ασφαλούς στο Azure χρησιμοποιώντας τη διαχείριση πόρων Azure. Αυτός ο οδηγός σάς καθοδηγεί σε τα παρακάτω βήματα:

 - Ρύθμιση του αριθμού-κλειδιού θάλαμο για τη Διαχείριση πλήκτρα για την ασφάλεια στο σύμπλεγμα και εφαρμογή.
 - Δημιουργήστε ένα σύμπλεγμα ασφαλή στο Azure με τη διαχείριση πόρων Azure.
 - Έλεγχος ταυτότητας χρηστών με Azure Active Directory (AAD) για τη Διαχείριση συμπλέγματος.

Μια ασφαλής σύμπλεγμα είναι ένα σύμπλεγμα που αποτρέπει την μη εξουσιοδοτημένη πρόσβαση σε λειτουργίες διαχείρισης, που περιλαμβάνει την ανάπτυξη, την αναβάθμιση και διαγραφή εφαρμογές, τις υπηρεσίες και τα δεδομένα που περιέχουν. Ένα μη ασφαλές σύμπλεγμα είναι ένα σύμπλεγμα ότι οποιοσδήποτε μπορεί να συνδεθούν στο οποιαδήποτε στιγμή και να εκτελέσετε λειτουργίες διαχείρισης. Παρόλο που είναι δυνατό να δημιουργήσετε ένα μη ασφαλές σύμπλεγμα, είναι **συνιστάται ιδιαίτερα να δημιουργήσετε ένα σύμπλεγμα ασφαλή**. Ένα μη ασφαλές σύμπλεγμα, **δεν είναι δυνατή η ασφάλεια αργότερα** - πρέπει να δημιουργηθεί ένα νέο σύμπλεγμα.

Οι έννοιες είναι το ίδιο για τη δημιουργία ασφαλούς συμπλεγμάτων, εάν οι των συμπλεγμάτων είναι συμπλεγμάτων Linux ή συμπλεγμάτων Windows. Για περισσότερες πληροφορίες και βοήθεια δέσμες ενεργειών για τη δημιουργία ασφαλούς Linux συμπλεγμάτων, ανατρέξτε στο θέμα [Δημιουργία ασφαλούς συμπλεγμάτων σε Linux](#secure-linux-clusters)

## <a name="log-in-to-azure"></a>Συνδεθείτε στο Azure
Αυτός ο οδηγός χρησιμοποιεί [Azure PowerShell][azure-powershell]. Κατά την εκκίνηση μιας νέας περιόδου λειτουργίας PowerShell, συνδεθείτε στο λογαριασμό σας στο Azure και επιλέξτε τη συνδρομή σας πριν από την εκτέλεση Azure εντολές.

Συνδεθείτε στο λογαριασμό σας στο azure:

```powershell
Login-AzureRmAccount
```

Επιλέξτε τη συνδρομή σας:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Ρύθμιση του αριθμού-κλειδιού θάλαμο

Αυτή η ενότητα παρουσιάζει τη δημιουργία ενός αριθμού-κλειδιού θάλαμο για ένα σύμπλεγμα ύφασμα υπηρεσίας στο Azure και για τις εφαρμογές υπηρεσίας ύφασμα. Για οδηγίες ολοκλήρωσης σε θάλαμο αριθμού-κλειδιού, που αναφέρονται σε το [κλειδί θάλαμο γρήγορα αποτελέσματα Οδηγός][key-vault-get-started].

Υπηρεσία ύφασμα χρησιμοποιεί πιστοποιητικά X.509 για να προστατεύσετε ένα σύμπλεγμα και παρέχει δυνατότητες ασφαλείας της εφαρμογής. Azure θάλαμο αριθμού-κλειδιού χρησιμοποιείται για τη διαχείριση πιστοποιητικών για συμπλεγμάτων ύφασμα υπηρεσίας στο Azure. Όταν ένα σύμπλεγμα έχει αναπτυχθεί στο Azure, η υπηρεσία παροχής Azure πόρων που είναι υπεύθυνη για τη δημιουργία συμπλεγμάτων ύφασμα υπηρεσίας χρησιμοποιεί τα πιστοποιητικά από θάλαμο κλειδί και εγκαθιστά τους στο σύμπλεγμα ΣΠΣ.

Το παρακάτω διάγραμμα παρουσιάζει τη σχέση μεταξύ του αριθμού-κλειδιού θάλαμο, ένα σύμπλεγμα ύφασμα υπηρεσίας και η υπηρεσία παροχής Azure πόρων που χρησιμοποιεί πιστοποιητικά που είναι αποθηκευμένα στο πλήκτρο θάλαμο όταν δημιουργεί ένα σύμπλεγμα:

![Εγκατάσταση πιστοποιητικού][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Δημιουργήστε μια ομάδα πόρων

Το πρώτο βήμα είναι να δημιουργήσετε μια ομάδα πόρων ειδικά για το κλειδί θάλαμο. Συνιστάται η θέση αριθμού-κλειδιού θάλαμο σε ξεχωριστή ομάδα πόρων. Αυτό σας επιτρέπει να καταργήσετε το υπολογισμού και την αποθήκευση πόρων ομάδες, καθώς και την ομάδα πόρου που έχει το σύμπλεγμά σας ύφασμα υπηρεσίας χωρίς να χάσετε το πλήκτρα και απορρήτου. Ομάδα πόρων που έχει τον αριθμό-κλειδί θάλαμο πρέπει να είναι στην ίδια περιοχή ως το σύμπλεγμα που χρησιμοποιεί.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Δημιουργία βασικών θάλαμο 

Δημιουργήστε ένα πλήκτρο θάλαμο στη νέα ομάδα πόρων. Το κλειδί θάλαμο **πρέπει να είναι ενεργοποιημένη για ανάπτυξη** για να επιτρέψετε την υπηρεσία παροχής υπηρεσίας ύφασμα πόρων για να λάβετε τα πιστοποιητικά από αυτό και εγκαταστήστε το στοιχείο σε κόμβους συμπλέγματος:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Εάν έχετε μια υπάρχουσα θάλαμο αριθμό-κλειδί, μπορείτε να την ενεργοποιήσετε για ανάπτυξη χρησιμοποιώντας Azure CLI:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>
## <a name="add-certificates-to-key-vault"></a>Προσθήκη πιστοποιητικών θάλαμο αριθμού-κλειδιού

Τα πιστοποιητικά χρησιμοποιούνται σε ύφασμα υπηρεσίας για την παροχή ελέγχου ταυτότητας και κρυπτογράφησης για την ασφάλιση διάφορες πτυχές των ένα σύμπλεγμα και τις εφαρμογές. Για περισσότερες πληροφορίες σχετικά με τον τρόπο χρήσης των πιστοποιητικών σε ύφασμα υπηρεσίας, ανατρέξτε στο θέμα [σενάρια ασφαλείας συμπλέγματος υπηρεσίας ύφασμα][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Πιστοποιητικό σύμπλεγμα και server (απαιτείται) 

Αυτό το πιστοποιητικό απαιτείται για να προστατεύσετε ένα σύμπλεγμα και να αποτρέψετε τη μη εξουσιοδοτημένη πρόσβαση σε αυτό. Παρέχει ασφαλείας συμπλέγματος με μερικά τρόπους:
 
 - **Σύμπλεγμα ελέγχου ταυτότητας:** Πραγματοποιεί έλεγχο ταυτότητας κόμβου προς κόμβο επικοινωνίας για την Ομοσπονδία σύμπλεγμα. Μόνο οι κόμβοι που μπορεί να αποδεικνύετε την ταυτότητά τους με αυτό το πιστοποιητικό μπορούν να συμμετάσχουν στο σύμπλεγμα.
 - **Έλεγχος ταυτότητας διακομιστή:** Πραγματοποιεί έλεγχο ταυτότητας τα τελικά σημεία διαχείρισης συμπλέγματος σε ένα πρόγραμμα-πελάτης διαχείρισης, ώστε ο υπολογιστής-πελάτης διαχείρισης να γνωρίζει το μιλάει στο πραγματικό σύμπλεγμα. Αυτό το πιστοποιητικό παρέχει επίσης SSL για το API διαχείρισης HTTPS και για την υπηρεσία ύφασμα Explorer μέσω HTTPS.

Για να εξυπηρετήσει αυτούς τους σκοπούς, το πιστοποιητικό πρέπει να ικανοποιεί τις ακόλουθες απαιτήσεις:

 - Το πιστοποιητικό πρέπει να περιέχει ένα ιδιωτικό κλειδί.
 - Το πιστοποιητικό πρέπει να δημιουργηθούν για ανταλλαγή κλειδιών, δυνατότητα εξαγωγής σε ένα αρχείο ανταλλαγής προσωπικών πληροφοριών (.pfx).
 - Όνομα θέματος το πιστοποιητικό πρέπει να συμφωνεί με τον τομέα που χρησιμοποιείται για την πρόσβαση στο σύμπλεγμα ύφασμα υπηρεσίας. Σε αυτό το matchng απαιτείται για την παροχή SSL τελικά σημεία διαχείρισης HTTPS και της Εξερεύνησης ύφασμα υπηρεσίας του συμπλέγματος. Δεν μπορείτε να αποκτήσετε ένα πιστοποιητικό SSL από μια αρχή έκδοσης πιστοποιητικών (CA) για το `.cloudapp.azure.com` τομέα. Θα πρέπει να αποκτήσετε ένα προσαρμοσμένο όνομα τομέα για το σύμπλεγμά σας. Όταν κάνετε αίτηση για ένα πιστοποιητικό από μια αρχή έκδοσης Πιστοποιητικών, όνομα θέματος το πιστοποιητικό πρέπει να συμφωνεί με το όνομα του προσαρμοσμένου τομέα που χρησιμοποιείται για το σύμπλεγμά σας.

### <a name="application-certificates-optional"></a>Εφαρμογή πιστοποιητικά (προαιρετικό)

Οποιονδήποτε αριθμό πρόσθετα πιστοποιητικά μπορεί να εγκατασταθεί σε ένα σύμπλεγμα για λόγους ασφαλείας εφαρμογής. Πριν να δημιουργήσετε το σύμπλεγμά σας, εξετάστε τα σενάρια ασφαλείας εφαρμογής που απαιτούν ένα πιστοποιητικό για να είναι εγκατεστημένο στον τους κόμβους, όπως:

 - Κρυπτογράφηση και αποκρυπτογράφηση των τιμών ρύθμισης παραμέτρων εφαρμογής
 - Κρυπτογράφηση δεδομένων σε κόμβους κατά την αναπαραγωγή 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Μορφοποίηση πιστοποιητικών για χρήση της υπηρεσίας παροχής Azure πόρων

Ιδιωτικό κλειδί αρχείων (.pfx) μπορεί να προστεθεί και να χρησιμοποιηθεί απευθείας μέσω του αριθμού-κλειδιού θάλαμο. Ωστόσο, η υπηρεσία παροχής Azure πόρων απαιτεί κλειδιά να αποθηκευτούν σε μια ειδική μορφή JSON που περιλαμβάνει το .pfx ως ένα βάσης 64 κωδικοποιημένη συμβολοσειρά και τον κωδικό πρόσβασης ιδιωτικό κλειδί. Για να συμπεριλάβετε αυτές τις απαιτήσεις, πλήκτρα πρέπει να τοποθετηθεί σε μια συμβολοσειρά JSON και, στη συνέχεια, έχουν αποθηκευτεί ως *απόρρητο* στο πλήκτρο θάλαμο.

Για να διευκολύνετε τη διαδικασία, μια λειτουργική μονάδα PowerShell είναι [διαθέσιμη στο GitHub][service-fabric-rp-helpers]. Ακολουθήστε τα παρακάτω βήματα για να χρησιμοποιήσετε τη λειτουργική μονάδα:

  1. Κάντε λήψη του όλα τα περιεχόμενα του repo σε έναν τοπικό κατάλογο. 
  2. Εισαγωγή της λειτουργικής μονάδας στο παράθυρο του PowerShell:

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
Το `Invoke-AddCertToKeyVault` εντολή σε αυτήν την ενότητα PowerShell αυτόματα μορφοποιεί ένα ιδιωτικό κλειδί πιστοποιητικού σε μια συμβολοσειρά JSON και αποστέλλει θάλαμο αριθμού-κλειδιού. Χρησιμοποιήστε το για να προσθέσετε το πιστοποιητικό σύμπλεγμα και τυχόν πρόσθετα εφαρμογής πιστοποιητικά θάλαμο αριθμού-κλειδιού. Επαναλάβετε αυτό το βήμα για τυχόν πρόσθετα πιστοποιητικά που θέλετε να εγκαταστήσετε το σύμπλεγμά σας.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Η προηγούμενη συμβολοσειρές είναι όλες τις προϋποθέσεις θάλαμο αριθμού-κλειδιού για ρύθμιση παραμέτρων του προτύπου από διαχειριστή πόρων συμπλέγματος ύφασμα υπηρεσίας που εγκαθίσταται πιστοποιητικά για κόμβο, Διαχείριση ασφαλείας τελικού σημείου και του ελέγχου ταυτότητας και οποιαδήποτε πρόσθετη ασφάλεια τις δυνατότητες της εφαρμογής που χρησιμοποιούν πιστοποιητικά X.509. Σε αυτό το σημείο, πρέπει τώρα να έχετε τα ακόλουθα ρύθμισης στο Azure:

 - Ομάδα πόρων θάλαμο αριθμού-κλειδιού
   - Πλήκτρο θάλαμο
     - Πιστοποιητικό ελέγχου ταυτότητας διακομιστή συμπλέγματος
     - Εφαρμογή πιστοποιητικών

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Ρύθμιση του Azure Active Directory για έλεγχο ταυτότητας του προγράμματος-πελάτη

AAD επιτρέπει εταιρείες (γνωστό ως μισθωτές) για τη διαχείριση πρόσβασης χρήστη σε εφαρμογές, οι οποίες είναι χωρισμένα σε εφαρμογές με μια σύνδεση με βάση το web περιβάλλοντος εργασίας Χρήστη και τις εφαρμογές με μια εμπειρία εγγενές πρόγραμμα-πελάτη. Σε αυτό το έγγραφο, θα σας λαμβάνεται ως δεδομένο ότι έχετε ήδη δημιουργήσει ένα μισθωτή. Εάν δεν είναι, Αρχίστε διαβάζοντας το [πώς μπορείτε να λάβετε ένα μισθωτή του Azure Active Directory][active-directory-howto-tenant].

Ένα σύμπλεγμα υπηρεσίας ύφασμα προσφέρει πολλά σημεία εισόδου στις λειτουργίες του διαχείρισης, συμπεριλαμβανομένης της [Υπηρεσίας ύφασμα Explorer] βασίζεται στο web[ service-fabric-visualizing-your-cluster] και του [Visual Studio][service-fabric-manage-application-in-visual-studio]. Ως αποτέλεσμα, μπορείτε να δημιουργήσετε δύο AAD εφαρμογών για να ελέγχετε την πρόσβαση στο σύμπλεγμα, μια εφαρμογή web και μία εγγενή εφαρμογή.

Για να απλοποιήσετε ορισμένα βήματα που περιλαμβάνονται στη ρύθμιση παραμέτρων AAD με ένα σύμπλεγμα ύφασμα υπηρεσίας, έχουμε δημιουργήσει ένα σύνολο των δεσμών ενεργειών του Windows PowerShell.

>[AZURE.NOTE] Πρέπει να εκτελέσετε αυτά τα βήματα *πριν από* τη δημιουργία του συμπλέγματος αυτό στις περιπτώσεις όπου οι δέσμες ενεργειών αναμένετε σύμπλεγμα ονόματα και τα τελικά σημεία, αυτά θα πρέπει να είναι προγραμματισμένες τιμές, δεν αυτά που έχετε ήδη δημιουργήσει.

1. [Κάντε λήψη των δεσμών ενεργειών] [ sf-aad-ps-script-download] στον υπολογιστή σας.

2. Κάντε δεξί κλικ στο αρχείο zip, επιλέξτε **Ιδιότητες**, στη συνέχεια, επιλέξτε το πλαίσιο ελέγχου **Κατάργηση αποκλεισμού** και εφαρμογή.

3. Εξαγάγετε το αρχείο zip.

4. Εκτέλεση `SetupApplications.ps1`, παρέχοντας την TenantId ClusterName και WebApplicationReplyUrl ως παράμετροι. Για παράδειγμα:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Μπορείτε να βρείτε το **TenantId** κατά την εκτέλεση της εντολής PowerShell '' Get-AzureSubscription'' '. Αυτό θα εμφανίσει το **TenantId** για κάθε εγγραφή.

    Το **ClusterName** χρησιμοποιείται για πρόθεμα τις εφαρμογές AAD που δημιουργήθηκε από τη δέσμη ενεργειών. Δεν χρειάζεται να ταιριάζει με το όνομα του πραγματικού συμπλέγματος ακριβώς όπως προορίζεται μόνο για να σας διευκολύνει να αντιστοιχίζετε AAD αντικείμενα σε ένα σύμπλεγμα ύφασμα υπηρεσίας που που χρησιμοποιούνται με.

    Το **WebApplicationReplyUrl** είναι το προεπιλεγμένο τελικό σημείο που AAD επιστρέφει στους χρήστες σας αφού ολοκληρώσετε τη διαδικασία εισόδου. Θα πρέπει να ορίσετε το τελικό σημείο Explorer ύφασμα υπηρεσίας για το σύμπλεγμα, η οποία από προεπιλογή είναι:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    Θα σας ζητηθεί να συνδεθείτε στο λογαριασμό που έχει δικαιώματα διαχειριστή για το μισθωτή AAD. Όταν το κάνετε συνεχίζεται η δέσμη ενεργειών για να δημιουργήσετε το web και εγγενείς εφαρμογές για να αντιπροσωπεύει το σύμπλεγμά σας ύφασμα υπηρεσίας. Αν κοιτάξετε του μισθωτή εφαρμογές στην [πύλη του Azure κλασική][azure-classic-portal], θα πρέπει να βλέπετε δύο νέες εγγραφές:

    - *ClusterName*\_συμπλέγματος
    - *ClusterName*\_προγράμματος-πελάτη

    Το δέσμη ενεργειών εκτυπώνει το Json απαιτείται από το πρότυπο διαχείρισης πόρων Azure κατά τη δημιουργία του συμπλέγματος στην επόμενη ενότητα, επομένως διατηρήστε το PowerShell ανοίξει το παράθυρο.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Δημιουργία προτύπου από διαχειριστή πόρων ύφασμα υπηρεσίας συμπλέγματος

Σε αυτήν την ενότητα, το αποτέλεσμα των προηγούμενων εντολών του PowerShell που χρησιμοποιούνται σε ένα πρότυπο ύφασμα υπηρεσιών συμπλέγματος διαχείρισης πόρων.

Διαχείριση πόρων δείγμα πρότυπα είναι διαθέσιμα στη [συλλογή προτύπων Azure γρήγορης εκκίνησης στο GitHub][azure-quickstart-templates]. Αυτά τα πρότυπα μπορεί να χρησιμοποιηθεί ως σημείο εκκίνησης για το πρότυπο σύμπλεγμα. 

### <a name="create-the-resource-manager-template"></a>Δημιουργία του προτύπου για τη διαχείριση πόρων

Αυτός ο οδηγός χρησιμοποιεί το [5 κόμβου ασφαλούς σύμπλεγμα] [ service-fabric-secure-cluster-5-node-1-nodetype-wad] δείγμα προτύπου και τις παραμέτρους του προτύπου. Λήψη `azuredeploy.json` και `azuredeploy.parameters.json` στον υπολογιστή σας και ανοίξτε και τα δύο αρχεία στο πρόγραμμα επεξεργασίας κειμένου Αγαπημένα.

### <a name="add-certificates"></a>Προσθήκη πιστοποιητικών

Τα πιστοποιητικά προστίθεται σε ένα πρότυπο από διαχειριστή πόρων συμπλέγματος με την αναφορά του αριθμού-κλειδιού θάλαμο που περιέχει τα πλήκτρα πιστοποιητικού. Συνιστάται να ότι αυτές οι τιμές του αριθμού-κλειδιού θάλαμο τοποθετούνται σε ένα αρχείο παραμέτρων προτύπου για τη διαχείριση πόρων για να διατηρήσετε τη διαχείριση πόρων με δυνατότητα επανάληψης χρήσης πρότυπο αρχείου και δωρεάν τιμών ειδικά για μια ανάπτυξη.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>Προσθήκη όλων των πιστοποιητικών για την osProfile VMSS

Κάθε πιστοποιητικό που πρέπει να έχει εγκατασταθεί στο σύμπλεγμα πρέπει να ρυθμιστεί στην ενότητα osProfile του πόρου VMSS (Microsoft.Compute/virtualMachineScaleSets). Αυτό καθοδηγεί την υπηρεσία παροχής πόρων για να εγκαταστήσετε το πιστοποιητικό σε του ΣΠΣ. Αυτό περιλαμβάνει το πιστοποιητικό σύμπλεγμα, καθώς και τυχόν πιστοποιητικών ασφαλείας εφαρμογής που σκοπεύετε να χρησιμοποιήσετε για τις εφαρμογές σας:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-service-fabric-cluster-certificate"></a>Ρύθμιση παραμέτρων υφάσματος υπηρεσιών συμπλέγματος πιστοποιητικού

Το πιστοποιητικό ελέγχου ταυτότητας σύμπλεγμα πρέπει επίσης να ρυθμιστεί στο τον πόρο συμπλέγματος ύφασμα υπηρεσίας (Microsoft.ServiceFabric/clusters) και την επέκταση ύφασμα υπηρεσίας για VMSS στον πόρο VMSS. Αυτό σας επιτρέπει την υπηρεσία παροχής υπηρεσίας ύφασμα πόρων για να ρυθμίσετε τις παραμέτρους της για χρήση για το σύμπλεγμα και του ελέγχου ταυτότητας διακομιστή για τα τελικά σημεία διαχείρισης.

##### <a name="vmss-resource"></a>VMSS πόρων:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Υπηρεσία ύφασμα πόρων:

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-aad-config"></a>Εισαγωγή ρύθμισης παραμέτρων AAD

Η ρύθμιση παραμέτρων AAD που δημιουργήσατε νωρίτερα μπορεί να εισαχθεί απευθείας στο δικό σας πρότυπο διαχείρισης πόρων, ωστόσο συνιστάται να εξαγάγετε τις τιμές σε παραμέτρους πρώτα σε ένα αρχείο παραμέτρων για να διατηρήσετε το πρότυπο διαχείρισης πόρων με δυνατότητα επανάληψης χρήσης και δωρεάν τιμών συγκεκριμένα σε μια ανάπτυξη.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < μια "Ρύθμιση παραμέτρων-arm" ></a>παραμέτρους προτύπου ρύθμιση παραμέτρων διαχείρισης πόρων

Τέλος, χρησιμοποιήστε τις τιμές εξόδου από τις εντολές θάλαμο κλειδί και AAD PowerShell για τη συμπλήρωση του αρχείου παράμετροι:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Σε αυτό το σημείο, θα πρέπει τώρα να έχετε τα εξής:

 - Ομάδα πόρων θάλαμο αριθμού-κλειδιού
    - Πλήκτρο θάλαμο
    - Πιστοποιητικό ελέγχου ταυτότητας διακομιστή συμπλέγματος
    - Πιστοποιητικό κρυπτογράφηση δεδομένων
 - Azure Active Directory μισθωτή 
    - Εφαρμογή AAD για διαχείριση βασίζεται στο web και Εξερεύνηση ύφασμα υπηρεσίας
    - Εφαρμογή AAD για τη Διαχείριση εγγενές πρόγραμμα-πελάτη
    - Οι χρήστες με τους ρόλους που έχουν εκχωρηθεί 
 - Το πρότυπο διαχείρισης πόρων συμπλέγματος ύφασμα υπηρεσίας
    - Ρύθμιση των παραμέτρων μέσω του αριθμού-κλειδιού θάλαμο πιστοποιητικών
    - Azure Active Directory που έχει ρυθμιστεί 

Το παρακάτω διάγραμμα παρουσιάζει όπου ρύθμισης παραμέτρων θάλαμο κλειδί και AAD χωράει στο πρότυπό σας από διαχειριστή πόρων.

![Διαχείριση πόρων εξάρτηση χάρτη][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Δημιουργία του συμπλέγματος

Τώρα είστε έτοιμοι να δημιουργήσετε στο σύμπλεγμα χρησιμοποιώντας [ανάπτυξης ARM][resource-group-template-deploy].

#### <a name="test-it"></a>Δοκιμάστε αυτό

Χρησιμοποιήστε την ακόλουθη εντολή PowerShell για να ελέγξετε το πρότυπό σας από διαχειριστή πόρων με ένα αρχείο παραμέτρων:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Ανάπτυξη του

Εάν η δοκιμή προτύπου για τη διαχείριση πόρων είναι επιτυχής, χρησιμοποιήστε την ακόλουθη εντολή PowerShell για να αναπτύξετε το πρότυπό σας από διαχειριστή πόρων με ένα αρχείο παραμέτρων:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>
## <a name="assign-users-to-roles"></a>Αντιστοίχιση χρηστών σε ρόλους

Αφού έχετε δημιουργήσει τις εφαρμογές για να αντιπροσωπεύει το σύμπλεγμά σας, πρέπει να εκχωρήσετε τους χρήστες σας για να τους ρόλους που υποστηρίζονται από το ύφασμα υπηρεσίας: μόνο για ανάγνωση και επιλέξτε Διαχείριση. Μπορείτε να το κάνετε με την [πύλη του Azure κλασική][azure-classic-portal].

1. Μεταβείτε στο μισθωτή σας και επιλέξτε εφαρμογές.
2. Επιλέξτε την εφαρμογή web, η οποία έχει ένα όνομα όπως `myTestCluster_Cluster`.
3. Κάντε κλικ στην καρτέλα χρήστες.
4. Επιλέξτε το χρήστη για να αντιστοιχίσετε και κάντε κλικ στο κουμπί **Εκχώρηση** στο κάτω μέρος της οθόνης.

    ![Αντιστοίχιση χρηστών σε ρόλους κουμπί][assign-users-to-roles-button]

5. Επιλέξτε το ρόλο για να εκχωρήσετε στο χρήστη.

    ![Αντιστοίχιση χρηστών σε ρόλους][assign-users-to-roles-dialog]

>[AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με τους ρόλους στο ύφασμα υπηρεσίας, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης βάσει ρόλων για προγράμματα-πελάτες ύφασμα υπηρεσίας](service-fabric-cluster-security-roles.md).

 <a name="secure-linux-cluster"></a> 
##  <a name="create-secure-clusters-on-linux"></a>Δημιουργία ασφαλούς συμπλεγμάτων σε Linux

Για να διευκολύνετε τη διαδικασία, έχει παραχωρηθεί μια δέσμη ενεργειών Βοήθειας [εδώ](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Για τη χρήση αυτής της δέσμης ενεργειών Βοήθειας, θεωρείται ότι έχετε ήδη εγκαταστήσει CLI Azure και βρίσκεται στη διαδρομή σας. Βεβαιωθείτε ότι η δέσμη ενεργειών έχει δικαιώματα για την εκτέλεση, εκτελώντας `chmod +x cert_helper.py` μετά τη λήψη της. Το πρώτο βήμα είναι να συνδεθείτε στο λογαριασμό σας Azure χρησιμοποιώντας το CLI με το `azure login` εντολή. Μετά τη σύνδεση στο λογαριασμό σας στο Azure, χρησιμοποιήστε το στοιχείο βοήθειας με το πιστοποιητικό υπογραφής CA, όπως φαίνεται στο παρακάτω εντολή:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

Αυτή η εντολή επιστρέφει τις παρακάτω τρεις συμβολοσειρές ως το αποτέλεσμα: 

1. Μια SourceVaultID, η οποία είναι το Αναγνωριστικό για τη νέα ομάδα πόρων KeyVault το δημιουργήθηκε για εσάς. 

2. Μια CertificateUrl για να αποκτήσετε πρόσβαση στο πιστοποιητικό.

3. Μια CertificateThumbprint, η οποία χρησιμοποιείται για τον έλεγχο ταυτότητας.


Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε την εντολή:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Εκτέλεση της προηγούμενης εντολής σάς παρέχει τις τρεις συμβολοσειρές ως εξής:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

 Όνομα θέματος το πιστοποιητικό πρέπει να συμφωνεί με τον τομέα που χρησιμοποιείται για την πρόσβαση στο σύμπλεγμα ύφασμα υπηρεσίας. Αυτό είναι απαραίτητο για την παροχή SSL τελικά σημεία διαχείρισης HTTPS το σύμπλεγμα και της Εξερεύνησης ύφασμα υπηρεσίας. Δεν μπορείτε να αποκτήσετε ένα πιστοποιητικό SSL από μια αρχή έκδοσης πιστοποιητικών (CA) για το `.cloudapp.azure.com` τομέα. Θα πρέπει να αποκτήσετε ένα προσαρμοσμένο όνομα τομέα για το σύμπλεγμά σας. Όταν κάνετε αίτηση για ένα πιστοποιητικό από μια αρχή έκδοσης Πιστοποιητικών όνομα θέματος το πιστοποιητικό πρέπει να συμφωνεί με το όνομα του προσαρμοσμένου τομέα που χρησιμοποιείται για το σύμπλεγμά σας.

Αυτές είναι οι εγγραφές που απαιτείται για τη δημιουργία ένα σύμπλεγμα ύφασμα υπηρεσία ασφαλούς (χωρίς AAD), όπως περιγράφεται στην [παραμέτρους προτύπου ρύθμιση παραμέτρων διαχείρισης πόρων](#configure-arm). Μπορείτε να συνδεθείτε με το σύμπλεγμα ασφαλούς μέσω οδηγίες κατά [τον έλεγχο ταυτότητας πρόσβασης υπολογιστή-πελάτη σε ένα σύμπλεγμα](service-fabric-connect-to-secure-cluster.md). Linux προεπισκόπηση συμπλεγμάτων δεν υποστηρίζουν AAD ελέγχου ταυτότητας. Μπορείτε να εκχωρήσετε ρόλους διαχειριστή και προγράμματος-πελάτη που περιγράφεται στην ενότητα [αναθέσετε ρόλους στους χρήστες](#assign-roles). Κατά τον καθορισμό ρόλους διαχειριστή και προγράμματος-πελάτη για ένα σύμπλεγμα προεπισκόπηση Linux, έχετε για την παροχή thumbprints πιστοποιητικό ελέγχου ταυτότητας (σε αντίθεση με όνομα θέματος, επειδή δεν υπάρχει επικύρωση αλληλουχίας ανάκλησης πραγματοποιείται είτε σε αυτήν την έκδοση preview).


Εάν θέλετε να χρησιμοποιήσετε ένα αυτο-υπογεγραμμένο πιστοποιητικό για σκοπούς δοκιμής, μπορείτε να χρησιμοποιήσετε την ίδια δέσμη ενεργειών για να δημιουργήσετε ένα αυτο-υπογεγραμμένο πιστοποιητικό και στείλτε το στο KeyVault, με την παροχή της σημαίας `ss` όχι να δίνει τη διαδρομή πιστοποιητικό και το όνομα του πιστοποιητικού. Για παράδειγμα, δείτε την ακόλουθη εντολή για τη δημιουργία και αποστολή ένα αυτο-υπογεγραμμένο πιστοποιητικό:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

Αυτή η εντολή επιστρέφει το ίδιο τρεις συμβολοσειρές, SourceVault, CertificateUrl και CertificateThumbprint, η οποία χρησιμοποιείται για τη δημιουργία ασφαλούς Linux σύμπλεγμα, μαζί με τη θέση όπου έχει τοποθετηθεί το πιστοποιητικό αυτόματης υπογραφής. Θα χρειαστεί το αυτο-υπογεγραμμένο πιστοποιητικό για να συνδεθείτε με το σύμπλεγμα.  Μπορείτε να συνδεθείτε με το σύμπλεγμα ασφαλούς μέσω οδηγίες κατά [τον έλεγχο ταυτότητας πρόσβασης υπολογιστή-πελάτη σε ένα σύμπλεγμα](service-fabric-connect-to-secure-cluster.md). Όνομα θέματος το πιστοποιητικό πρέπει να συμφωνεί με τον τομέα που χρησιμοποιείται για την πρόσβαση στο σύμπλεγμα ύφασμα υπηρεσίας. Αυτό είναι απαραίτητο για την παροχή SSL τελικά σημεία διαχείρισης HTTPS το σύμπλεγμα και της Εξερεύνησης ύφασμα υπηρεσίας. Δεν μπορείτε να αποκτήσετε ένα πιστοποιητικό SSL από μια αρχή έκδοσης πιστοποιητικών (CA) για το `.cloudapp.azure.com` τομέα. Θα πρέπει να αποκτήσετε ένα προσαρμοσμένο όνομα τομέα για το σύμπλεγμά σας. Όταν κάνετε αίτηση για ένα πιστοποιητικό από μια αρχή έκδοσης Πιστοποιητικών όνομα θέματος το πιστοποιητικό πρέπει να συμφωνεί με το όνομα του προσαρμοσμένου τομέα που χρησιμοποιείται για το σύμπλεγμά σας.

Οι παράμετροι που παρέχεται από τη δέσμη ενεργειών βοηθητικού προγράμματος μπορεί να συμπληρωθεί στην πύλη, όπως περιγράφεται στην ενότητα [Δημιουργία ένα σύμπλεγμα στην πύλη του Azure](service-fabric-cluster-creation-via-portal.md#create-cluster-portal).

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το σημείο, μπορείτε να έχετε ένα σύμπλεγμα ασφαλούς με έλεγχο ταυτότητας παρέχουν διαχείρισης Azure Active Directory. Επόμενο, [συνδεθείτε με το σύμπλεγμα](service-fabric-connect-to-secure-cluster.md) και για να μάθετε πώς μπορείτε να [διαχειριστείτε απόρρητο εφαρμογής](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Αντιμετώπιση προβλημάτων με τη ρύθμιση του Azure Active Directory για έλεγχο ταυτότητας του προγράμματος-πελάτη

Εάν αντιμετωπίσετε κάποιο πρόβλημα κατά τη ρύθμιση του Azure Active Directory για έλεγχο ταυτότητας του προγράμματος-πελάτη, διαβάστε το παρακάτω suggestings για πιθανές λύσεις.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Υπηρεσία ύφασμα Explorer οδηγίες για την επιλογή πιστοποιητικού

#### <a name="problem"></a>Πρόβλημα

Μετά τη σύνδεση με επιτυχία στη σελίδα login AAD στην υπηρεσία ύφασμα Explorer, το πρόγραμμα περιήγησης επιστρέφει στην αρχική σελίδα αλλά ζητά από ένα παράθυρο διαλόγου για την επιλογή ενός πιστοποιητικού.

![Παράθυρο διαλόγου Επιλογή πιστοποιητικού SFX][sfx-select-certificate-dialog]

#### <a name="reason"></a>Αιτία

Ο χρήστης δεν έχει εκχωρηθεί σε ένα ρόλο στην εφαρμογή σύμπλεγμα AAD. Έτσι ο έλεγχος ταυτότητας AAD αποτυγχάνει σε σύμπλεγμα ύφασμα υπηρεσίας. Υπηρεσία ύφασμα Explorer επιστρέψουν στο πιστοποιητικό ελέγχου ταυτότητας.

#### <a name="solution"></a>Λύση

Ακολουθήστε τις οδηγίες από τη ρύθμιση του AAD και να αναθέσετε ρόλους χρήστη. Επίσης, συνιστάται να "ΧΡΉΣΤΗ ΑΝΆΘΕΣΗΣ ΑΠΑΙΤΕΊΤΑΙ σε Εφαρμογή της ACCESS" για να ενεργοποιηθεί ως `SetupApplications.ps1` κάνει.

### <a name="connect-with-powershell-fails-with-error-the-specified-credentials-are-invalid"></a>Σύνδεση με το PowerShell αποτύχει με το σφάλμα: οι καθορισμένες πιστοποιήσεις δεν είναι έγκυρες

#### <a name="problem"></a>Πρόβλημα

Όταν χρήση του PowerShell για να συνδεθείτε με χρήση της κατάστασης λειτουργίας ασφαλείας "AzureActiveDirectory" σύμπλεγμα, μετά τη σύνδεση με επιτυχία στη σελίδα login AAD, σύνδεση αποτυγχάνει με το σφάλμα: οι καθορισμένες πιστοποιήσεις δεν είναι έγκυρες φαίνεται.

#### <a name="solution"></a>Λύση

Όπως περιγράφεται παραπάνω.

### <a name="service-fabric-explorer-signing-in-return-failure-aadsts50011"></a>Υπηρεσία ύφασμα Explorer είσοδο σε επιστροφές αποτυχία: AADSTS50011

#### <a name="problem"></a>Πρόβλημα

Μετά τη σύνδεση στη σελίδα στην υπηρεσία ύφασμα Εξερεύνηση AAD εισόδου, σελίδα επιστρέφει εισόδου αποτυχία - AADSTS50011: τη διεύθυνση απάντησης &lt;διεύθυνση url&gt; δεν ταιριάζουν με τις διευθύνσεις απάντησης που έχει ρυθμιστεί για την εφαρμογή: &lt;guid&gt;. 

![Διεύθυνση απάντησης SFX δεν ταιριάζουν με][sfx-reply-address-not-match]

#### <a name="reason"></a>Αιτία

Η εφαρμογή cluster(web) που αντιπροσωπεύει προσπάθειες Explorer ύφασμα Service για τον έλεγχο ταυτότητας σε σχέση με AAD, ως μέρος της πρόσκλησης σε παρέχει τη διεύθυνση URL επιστροφής redirect. Αλλά δεν παρατίθεται στη λίστα 'ΑΠΆΝΤΗΣΗ URL' εφαρμογή AAD.

#### <a name="solution"></a>Λύση

Προσθέστε διεύθυνση url της υπηρεσίας ύφασμα Explorer 'ΑΠΆΝΤΗΣΗ διεύθυνση URL' στην καρτέλα ρύθμιση παραμέτρων της εφαρμογής cluster(web) ή αντικαταστήστε ένα από τα στοιχεία στη λίστα. Στη συνέχεια, αποθηκεύστε.

![Διεύθυνση url απάντηση εφαρμογής Web][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters"></a>Να να χρησιμοποιήσετε ξανά το ίδιο μισθωτή AAD για πολλές συμπλεγμάτων;

#### <a name="answer"></a>Απάντηση

Ναι. Αλλά μην ξεχάσετε να προσθέσετε τη διεύθυνση URL της υπηρεσίας ύφασμα Εξερεύνησης στην εφαρμογή σας cluster(web) δεν θα λειτουργούν διαφορετικά Explorer ύφασμα υπηρεσίας.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled"></a>Γιατί εξακολουθεί να πρέπει πιστοποιητικό διακομιστή ενώ με δυνατότητα AAD;

#### <a name="answer"></a>Απάντηση

FabricClient και FabricGateway εκτέλεση κοινό έλεγχο ταυτότητας. Σε περίπτωση AAD τον έλεγχο ταυτότητας, ενοποίηση AAD παρέχει ταυτότητα προγράμματος-πελάτη στο διακομιστή και πιστοποιητικό διακομιστή χρησιμοποιείται για την επαλήθευση ταυτότητας διακομιστή. Για περισσότερες πληροφορίες σχετικά με το πώς λειτουργεί το πιστοποιητικό σε ύφασμα υπηρεσίας, επιλέξτε [πιστοποιητικά X.509 και ύφασμα υπηρεσίας][x509-certificates-and-service-fabric]

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png