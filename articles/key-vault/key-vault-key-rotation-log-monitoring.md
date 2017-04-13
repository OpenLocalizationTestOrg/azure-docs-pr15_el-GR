<properties
    pageTitle="Τρόπος εγκατάστασης του αριθμού-κλειδιού θάλαμο με τον έλεγχο και τελικών κλειδιού περιστροφής | Microsoft Azure"
    description="Χρησιμοποιήστε αυτό οδηγίες θα σας βοηθήσουν να ρύθμισης με βασικές περιστροφής και παρακολούθηση κλειδιού θάλαμο αρχείων καταγραφής"
    services="key-vault"
    documentationCenter=""
    authors="swgriffith"
    manager="mbaldwin"
    tags=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="jodehavi;stgriffi"/>
#<a name="how-to-setup-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Τρόπος εγκατάστασης του αριθμού-κλειδιού θάλαμο με τον έλεγχο και τελικών κλειδιού περιστροφής

##<a name="introduction"></a>Εισαγωγή

Αφού δημιουργήσετε το θάλαμο κλειδί Azure, θα μπορούν να ξεκινούν αξιοποίηση που θάλαμο για την αποθήκευση του πλήκτρα και απορρήτου. Τις εφαρμογές σας δεν είναι πλέον πρέπει να παραμένει σας πλήκτρα ή απόρρητο, αλλά τα θα ζητήσει τους από το πλήκτρο θάλαμο σύμφωνα με τις ανάγκες. Αυτό σας επιτρέπει να ενημερώσετε κλειδιά και απορρήτου χωρίς να επηρεάζονται η συμπεριφορά της εφαρμογής σας, η οποία ανοίγει ένα πλήθος δυνατοτήτων γύρω από τον αριθμό-κλειδί και τη συμπεριφορά μυστικού διαχείρισης.

Σε αυτό το άρθρο παρουσιάζει ένα παράδειγμα αξιοποίηση Azure θάλαμο αριθμού-κλειδιού για να αποθηκεύσετε ένα μυστικό, σε αυτήν την περίπτωση ένα κλειδί λογαριασμού χώρου αποθήκευσης Azure που είναι προσβάσιμη από μια εφαρμογή του. Αυτό θα δείχνουν επίσης υλοποίηση μια προγραμματισμένη περιστροφής του πλήκτρου λογαριασμού χώρου αποθήκευσης. Τέλος, αυτό θα καθοδηγήσει μια επίδειξη σχετικά με τον τρόπο για την παρακολούθηση των αρχείων καταγραφής ελέγχου κλειδιού θάλαμο και να βελτιώσετε τις ειδοποιήσεις όταν οι αιτήσεις μη αναμενόμενο.

> \[AZURE. ΣΗΜΕΊΩΣΗ\] αυτό το πρόγραμμα εκμάθησης δεν προορίζεται για να εξηγήσετε με λεπτομέρειες το αρχικό σύνολο επάνω από το Azure θάλαμο αριθμού-κλειδιού. Για αυτές τις πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure κλειδί θάλαμο](key-vault-get-started.md). Ή, για το περιβάλλον γραμμής εντολών πλατφόρμες οδηγίες, ανατρέξτε στο θέμα [αυτό το πρόγραμμα εκμάθησης ισοδύναμο](key-vault-manage-with-cli.md).

##<a name="setting-up-keyvault"></a>Ρύθμιση KeyVault

Για να ενεργοποιήσετε μια εφαρμογή για να ανακτήσετε ένα μυστικό από θάλαμο κλειδί Azure, πρέπει πρώτα να δημιουργήσετε το μυστικό και στείλτε το στο θάλαμο σας. Αυτό μπορεί να πραγματοποιηθεί εύκολα μέσω του PowerShell όπως φαίνεται παρακάτω.

Ξεκινήστε μια περίοδο λειτουργίας Azure PowerShell και πραγματοποιήστε είσοδο στο λογαριασμό σας Azure με την ακόλουθη εντολή:

```powershell
Login-AzureRmAccount
```

Στο παράθυρο περιήγησης αναδυόμενο, εισαγάγετε το όνομα χρήστη λογαριασμός Azure και τον κωδικό πρόσβασης. Θα σας βοηθήσουν όλες τις συνδρομές που σχετίζονται με αυτόν το λογαριασμό και από προεπιλογή, χρησιμοποιεί το πρώτο Azure PowerShell.

Εάν έχετε πολλές συνδρομές, ίσως χρειαστεί να καθορίσετε κάποια συγκεκριμένη που χρησιμοποιήθηκε για τη δημιουργία του Azure θάλαμο αριθμού-κλειδιού. Πληκτρολογήστε τα εξής για να δείτε τις συνδρομές για το λογαριασμό σας:

```powershell
Get-AzureRmSubscription
```

Στη συνέχεια, για να καθορίσετε τη συνδρομή που είναι συσχετισμένη με το πλήκτρο θάλαμο που θα η καταγραφή, πληκτρολογήστε:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID> 
```

Καθώς αυτό το άρθρο παρουσιάζει την αποθήκευση ενός κλειδιού λογαριασμού χώρου αποθήκευσης ως ένα μυστικό, θα πρέπει να λάβετε αυτό το κλειδί λογαριασμού χώρου αποθήκευσης.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Μετά από την ανάκτηση το μυστικό, σε αυτήν την περίπτωση το κλειδί λογαριασμού χώρου αποθήκευσης, θα πρέπει να μετατρέψετε που ασφαλούς συμβολοσειρά και, στη συνέχεια, δημιουργήστε ένα μυστικό με συγκεκριμένη τιμή σε σας κλειδιού θάλαμο.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Επόμενο που θα θέλετε να λάβετε το URI για το μυστικό που μόλις δημιουργήσατε. Αυτό θα χρησιμοποιηθεί αργότερα όταν καλείτε το θάλαμο αριθμού-κλειδιού για να ανακτήσετε το μυστικό. Εκτελέστε την ακόλουθη εντολή PowerShell και σημειώστε την τιμή 'Id', η οποία είναι το μυστικό URI.

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

##<a name="setting-up-application"></a>Εγκατάσταση εφαρμογής

Τώρα που έχετε έναν μυστικό είναι αποθηκευμένα που θα θέλετε να ανακτήσετε που μυστικό και χρησιμοποιήστε το από τον κώδικα. Υπάρχουν μερικά βήματα που απαιτούνται για να επιτύχετε αυτό, την πρώτη και πιο σημαντικές που καταχώρηση εφαρμογής σας με το Azure Active Directory και, στη συνέχεια, που σας ενημερώνει θάλαμο κλειδί Azure τις πληροφορίες εφαρμογής, έτσι ώστε να σας επιτρέψουν αιτήσεις από την εφαρμογή σας.

> \[AZURE. ΣΗΜΕΊΩΣΗ\] εφαρμογή σας πρέπει να δημιουργηθεί στο ίδιο μισθωτή Azure Active Directory ως το κλειδί θάλαμο. 

Πρώτα να ανοίξετε την καρτέλα εφαρμογές του Azure Active Directory

![Εφαρμογές που είναι ανοιχτές στο Azure AD](./media/keyvault-keyrotation/AzureAD_Header.png)

Επιλέξτε "Προσθήκη" για να προσθέσετε μια νέα εφαρμογή σας Azure AD

![Επιλέξτε Προσθήκη εφαρμογής](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Αποχώρηση από τον τύπο εφαρμογής ως 'API WEB ή/και εφαρμογή Web' και δώστε ένα όνομα στο την εφαρμογή σας.

![Όνομα της εφαρμογής](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Δώστε την εφαρμογή σας 'Καθολικής σύνδεσης διεύθυνσης URL' και ένα 'URI Αναγνωριστικό εφαρμογής'. Αυτές μπορεί να είναι οτιδήποτε που θέλετε για αυτήν την επίδειξη και μπορεί να αλλάξει αργότερα εάν είναι απαραίτητο.

![Παροχή απαιτείται URIs](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Μόλις η εφαρμογή προστίθεται σε Azure AD, που θα να εισάγονται στο τη σελίδα της εφαρμογής. Από εκείνη τη στιγμή κάντε κλικ στην καρτέλα 'Παράμετροι' και, στη συνέχεια, βρείτε και αντιγράψτε την τιμή 'Αναγνωριστικό υπολογιστή-πελάτη'. Σημειώστε το Αναγνωριστικό υπολογιστή-πελάτη για επόμενα βήματα.

Στη συνέχεια, θα πρέπει να δημιουργήσετε ένα κλειδί για την εφαρμογή σας να μπορούν να αλληλεπιδρούν με το Azure AD. Μπορείτε να το δημιουργήσετε κάτω από την ενότητα 'Πλήκτρα' στην καρτέλα "Ρύθμιση παραμέτρων". Σημειώστε την το κλειδί που δημιουργήθηκε πρόσφατα από την εφαρμογή του Azure AD για χρήση σε ένα βήμα νεότερη έκδοση.

![Πλήκτρα για εφαρμογή Azure AD](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Πριν από τη δημιουργία τυχόν κλήσεις από την εφαρμογή σας σε θάλαμο κλειδί θα πρέπει να ενημερώνουν το κλειδί θάλαμο σχετικά με την εφαρμογή σας και το ' δικαιώματα. Την ακόλουθη εντολή λαμβάνει το όνομα θάλαμο και το Αναγνωριστικό υπολογιστή-πελάτη από την εφαρμογή Azure AD και εκχωρεί 'Get' πρόσβαση του κλειδιού θάλαμο για την εφαρμογή.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Σε αυτό το σημείο θα είστε έτοιμοι να ξεκινήσετε να δημιουργείτε τις κλήσεις σας εφαρμογής. Στην εφαρμογή σας θα πρέπει πρώτα να εγκαταστήσετε τα πακέτα NuGet που απαιτούνται για να αλληλεπιδράσετε με θάλαμο κλειδί Azure και Azure Active Directory. Από την Κονσόλα διαχείρισης πακέτου του Visual Studio, πληκτρολογήστε τις παρακάτω εντολές. Σημειώστε ότι κατά την εγγραφή αυτού του άρθρου την τρέχουσα έκδοση του πακέτου για το στοιχείο είναι 3.10.305231913, ώστε να μπορεί να θέλετε να επιβεβαιώσετε την πιο πρόσφατη έκδοση και να ενημερώσετε αντίστοιχα.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Στο κώδικα της εφαρμογής σας, να δημιουργήσετε ένα εκπαιδευτικό για τη διατήρηση της μεθόδου για τον έλεγχο ταυτότητας της υπηρεσίας καταλόγου Active Directory. Σε αυτό το παράδειγμα που κλάση ονομάζεται 'Βοηθητικά προγράμματα'. Στη συνέχεια, θα πρέπει να προσθέσετε τη χρήση του παρακάτω.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Στη συνέχεια, προσθέστε την ακόλουθη μέθοδο για να ανακτήσετε το διακριτικό JWT από το Azure AD. Για δυνατότητα συντήρησης που μπορεί να θέλετε να μετακινήσετε τη σκληρή κωδικοποιημένο τιμές συμβολοσειρών σε της ρύθμισης παραμέτρων του web ή την εφαρμογή.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Τέλος, μπορείτε να προσθέσετε τον απαραίτητο κώδικα για να καλέσετε θάλαμο κλειδί και να ανακτήσετε το μυστικό τιμή. Πρώτα πρέπει να προσθέσετε τα ακόλουθα χρησιμοποιώντας πρόταση.

```csharp
using Microsoft.Azure.KeyVault;
```

Στη συνέχεια θα προσθέσετε οι κλήσεις μέθοδο για την κλήση αριθμού-κλειδιού θάλαμο και να ανακτήσετε το μυστικό. Σε αυτήν τη μέθοδο που παρέχει το μυστικό URI που έχετε αποθηκεύσει σε ένα προηγούμενο βήμα. Σημείωση η χρήση της μεθόδου GetToken από την κλάση βοηθητικά προγράμματα που δημιουργήσατε παραπάνω.
    
```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Όταν εκτελείτε την εφαρμογή σας, θα πρέπει τώρα να τον έλεγχο ταυτότητας για να Azure Active Directory και, στη συνέχεια, ανακτώντας σας κρυφής τιμής από το Azure θάλαμο αριθμού-κλειδιού.

##<a name="key-rotation-using-azure-automation"></a>Πλήκτρο περιστροφής με την αυτοματοποίηση Azure

Υπάρχουν διάφορες επιλογές για την εφαρμογή στρατηγικής περιστροφής για τιμές που αποθηκεύετε ως απόρρητο Azure κλειδί θάλαμο. Απόρρητο να περιστρέφονται ως μέρος μιας μη αυτόματων διαδικασιών, ενδέχεται να περιστραφεί ορθογώνιο με αξιοποίηση κλήσεις API ή μπορεί να περιστρέφονται κατά μια δέσμη ενεργειών αυτοματισμού. Για τους σκοπούς της σε αυτό το άρθρο θα σας θα αξιοποίηση Azure PowerShell συνδυάζονται με αυτοματοποίηση Azure για να αλλάξετε ένα πλήκτρο πρόσβασης λογαριασμού χώρου αποθήκευσης Azure και, στη συνέχεια, θα ενημερώσουμε ενός κλειδιού θάλαμο μυστικό με αυτό το νέο κλειδί. 

Προκειμένου να επιτρέπει αυτοματοποίησης Azure για να ορίσετε μυστικού τιμές σας κλειδιού θάλαμο θα πρέπει να λάβετε το Αναγνωριστικό υπολογιστή-πελάτη για τη σύνδεση με το όνομα 'AzureRunAsConnection' που δημιουργήθηκε όταν δημιουργηθεί παρουσία σας Azure αυτοματισμού. Μπορείτε να λάβετε σε αυτό το Αναγνωριστικό, επιλέγοντας 'Στοιχεία' από την παρουσία Azure αυτοματισμού. Από εκεί που επιλέξτε 'Συνδέσεις' και, στη συνέχεια, επιλέξτε την αρχή υπηρεσία 'AzureRunAsConnection'. Που θα θέλετε να λάβετε υπόψη το "Αναγνωριστικό εφαρμογής'. 

![Αναγνωριστικό υπολογιστή-πελάτη αυτοματισμού Azure](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Ενώ βρίσκεστε ακόμη στο παράθυρο περιουσιακών στοιχείων θα θέλετε επίσης να επιλέξετε "Λειτουργικές μονάδες". Από λειτουργικές μονάδες που θα επιλέξτε 'Συλλογή' και, στη συνέχεια, αναζητήστε το και 'Εισαγωγή' ενημερωμένες εκδόσεις των κάθε μία από τις ακόλουθες ενότητες.

    Azure
    Azure.Storage   
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage
    
> \[AZURE. ΣΗΜΕΊΩΣΗ\] κατά την εγγραφή αυτού του άρθρου σημειωθεί λειτουργικές μονάδες που πρέπει να ενημερωθούν για τη δέσμη ενεργειών σε κοινή χρήση κάτω από το στοιχείο η μόνο τα παραπάνω. Εάν διαπιστώσετε ότι η εργασία σας αυτοματισμού έχει αποτύχει, θα πρέπει να επιβεβαιώσετε ότι έχετε όλες τις απαραίτητες λειτουργικές μονάδες και τις εξαρτήσεις τους εισαχθεί.

Μετά την ανάκτηση το Αναγνωριστικό εφαρμογής για τη σύνδεσή σας Azure αυτοματισμού, θα πρέπει να σας ενημερώσει το θάλαμο κλειδί Azure ότι αυτή η εφαρμογή έχει πρόσβαση για να ενημερώσετε απόρρητο στο θάλαμο σας. Αυτό μπορεί να πραγματοποιηθεί με την ακόλουθη εντολή PowerShell.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Στη συνέχεια θα μπορείτε να επιλέξετε τον πόρο 'Runbooks' στην περιοχή σας παρουσία αυτοματισμού Azure και επιλέξτε "Προσθήκη ενός Runbook'. Επιλέξτε 'Γρήγορη δημιουργία'. Όνομα του runbook και επιλέξτε 'PowerShell' ως τύπο runbook. Προαιρετικά να προσθέσετε μια περιγραφή. Τέλος, κάντε κλικ στο κουμπί 'Δημιουργία'.

![Δημιουργία Runbook](./media/keyvault-keyrotation/Create_Runbook.png)

Στο παράθυρο του προγράμματος επεξεργασίας για το νέο runbook θα επικολλήσετε την ακόλουθη δέσμη ενεργειών PowerShell.

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -StorageAccountName $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Από το παράθυρο του προγράμματος επεξεργασίας, μπορείτε να επιλέξετε 'Έλεγχος παράθυρο' για να ελέγξετε τη δέσμη ενεργειών σας. Όταν η δέσμη ενεργειών εκτελείται χωρίς σφάλμα, μπορείτε να επιλέξετε την επιλογή "Δημοσίευση" και, στη συνέχεια, μπορείτε να εφαρμόσετε ένα χρονοδιάγραμμα για runbook πίσω στο παράθυρο Ρύθμιση παραμέτρων runbook.

##<a name="key-vault-auditing-pipeline"></a>Έλεγχος του αριθμού-κλειδιού θάλαμο διοχέτευσης

Κατά τη ρύθμιση μιας θάλαμο κλειδί Azure μπορείτε να ενεργοποιήσετε τον έλεγχο για τη συλλογή αρχείων καταγραφής σε αιτήσεις πρόσβασης σε θάλαμο το κλειδί. Αυτά τα αρχεία καταγραφής είναι αποθηκευμένες σε ένα λογαριασμό που έχει οριστεί ως αποθήκευσης Azure και να, στη συνέχεια, να τα μηνύματα μεταφέρονται, παρακολουθούνται και ανάλυση. Κάτω από το στοιχείο καθοδηγεί σε ένα σενάριο που αξιοποιεί Azure συναρτήσεις, Azure λογικής εφαρμογών και αριθμού-κλειδιού θάλαμο ελέγχου αρχεία καταγραφής για να δημιουργήσετε μια διαδικασία για να στείλετε ένα μήνυμα ηλεκτρονικού ταχυδρομείου όταν απόρρητο από το θάλαμο ανακτώνται από μια εφαρμογή που ταιριάζουν με το αναγνωριστικό εφαρμογής του web app.

Πρώτα, θα πρέπει να ενεργοποιήσετε την καταγραφή σε θάλαμο τον αριθμό-κλειδί. Αυτό μπορεί να γίνει μέσω του τις παρακάτω εντολές του PowerShell (πλήρεις λεπτομέρειες μπορούν να προβληθούν [εδώ](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>' 
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Όταν είναι ενεργοποιημένη, στη συνέχεια, αρχεία καταγραφής ελέγχου θα ξεκινήσει τη συλλογή στο λογαριασμό που έχει οριστεί ως χώρου αποθήκευσης. Αυτά τα αρχεία καταγραφής θα περιέχει συμβάντα σχετικά με τον τρόπο και όταν το κλειδί χώροι φύλαξης είναι προσβάσιμες και από ποιον. 

> \[AZURE. ΣΗΜΕΊΩΣΗ\] μπορείτε να αποκτήσετε πρόσβαση τις πληροφορίες των αρχείων καταγραφής πολύ, 10 λεπτά μετά από τον αριθμό-κλειδί φύλαξης λειτουργία. Στις περισσότερες περιπτώσεις, θα είναι ταχύτερα από αυτό.

Το επόμενο βήμα είναι να [δημιουργήσετε μια ουρά Bus υπηρεσίας Azure](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Αυτό θα όπου μετακινούνται τα αρχεία καταγραφής ελέγχου κλειδιού θάλαμο. Μία φορά στην ουρά, η εφαρμογή λογικής θα σηκώστε τους και να λειτουργούν σε αυτά. Για να δημιουργήσετε μια υπηρεσία Bus είναι σχετικά απλή και ακολουθούν τα βήματα υψηλού επιπέδου:

1. Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσία (Εάν έχετε ήδη ένα που θέλετε να χρησιμοποιήσετε για αυτό και, στη συνέχεια, προχωρήστε στο βήμα 2).
2. Αναζητήστε το Bus υπηρεσίας στην πύλη του και επιλέξτε το πεδίο ονόματος που θέλετε να δημιουργήσετε ουρά στο.
3. Επιλέξτε Δημιουργία και επιλέξτε υπηρεσία Bus -> ουρά και καταχωρήστε τις απαιτούμενες λεπτομέρειες.
4. Πιάσετε τις πληροφορίες σύνδεσης υπηρεσίας Bus, επιλέγοντας το χώρο ονομάτων και κάνοντας κλικ στην επιλογή _Πληροφορίες σύνδεσης_. Θα χρειαστείτε αυτές τις πληροφορίες για το επόμενο τμήμα.

Στη συνέχεια, θα [δημιουργήσετε μια συνάρτηση Azure](../azure-functions/functions-create-first-azure-function.md) ψηφοφορία με τα αρχεία καταγραφής του αριθμού-κλειδιού θάλαμο εντός του λογαριασμού χώρου αποθήκευσης και επιλέξτε τα νέα συμβάντα. Αυτό θα είναι μια συνάρτηση που ενεργοποιείται σε ένα χρονοδιάγραμμα.

Δημιουργία μιας συνάρτησης Azure (επιλέξτε Δημιουργία -> συνάρτηση εφαρμογής στην πύλη του). Κατά τη δημιουργία, μπορείτε να χρησιμοποιήσετε ένα υπάρχον σχέδιο φιλοξενίας ή να δημιουργήσετε ένα νέο. Μπορεί επίσης να επιλέξετε για τη φιλοξενία δυναμικές. Μπορείτε να βρείτε περισσότερες λεπτομέρειες για τη συνάρτηση φιλοξενίας επιλογές [εδώ](../azure-functions/functions-scale.md).

Όταν δημιουργείται η συνάρτηση Azure, μεταβείτε σε αυτήν και επιλέξτε ένα χρονόμετρο συνάρτηση και C\# , στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία** από την οθόνη έναρξης.

![Συναρτήσεις Azure Έναρξη Blade](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Στην καρτέλα " _Ανάπτυξη_ ", αντικαταστήστε τον κωδικό run.csx με τα εξής:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging; 
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log) 
{ 
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob) 
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```
> \[AZURE. ΣΗΜΕΊΩΣΗ\] βεβαιωθείτε ότι για να αντικαταστήσετε τις μεταβλητές στον κώδικα παραπάνω ώστε να οδηγούν στο λογαριασμό σας χώρο αποθήκευσης όπου εγγράφονται τα αρχεία καταγραφής θάλαμο αριθμού-κλειδιού, για να την υπηρεσία Bus που δημιουργήσατε νωρίτερα και να τη συγκεκριμένη διαδρομή προς τα αρχεία καταγραφής αποθήκευσης κλειδιού θάλαμο.

Η συνάρτηση παίρνει την πιο πρόσφατη αρχείο καταγραφής από το λογαριασμό χώρου αποθήκευσης όπου εγγράφονται τα αρχεία καταγραφής του αριθμού-κλειδιού θάλαμο, Αποτυπώνει την πιο πρόσφατη συμβάντα από αυτό το αρχείο και προωθεί τους σε μια υπηρεσία Bus ουρά. Επειδή ένα αρχείο θα μπορούσε να έχει πολλά συμβάντα, π.χ. μέσω Πλήρης ώρα, στη συνέχεια, δημιουργούμε ένα αρχείο _sync.txt_ που η συνάρτηση επίσης εξετάζει για να καθορίσετε τη χρονική σήμανση του τελευταίου συμβάντος που έχει επιλέξει προς τα επάνω. Αυτό θα εξασφαλίσει ότι θα σας δεν push το ίδιο συμβάν πολλές φορές. Αυτό το αρχείο _sync.txt_ περιέχει απλώς μια χρονική σήμανση για το τελευταίο αντιμετώπισε συμβάν. Τα αρχεία καταγραφής, κατά τη φόρτωση, πρέπει να ταξινομούνται με βάση τη χρονική σήμανση για να βεβαιωθείτε ότι αυτές ταξινομούνται σωστά.

Για αυτήν τη συνάρτηση, θα σας αναφοράς μερικά επιπλέον βιβλιοθήκες που δεν είναι διαθέσιμες από το πλαίσιο σε συναρτήσεις Azure. Για να συμπεριλάβετε τα εξής, χρειαζόμαστε Azure συναρτήσεις για να αποσπάσετε τους χρησιμοποιώντας nuget. Ενεργοποιήστε την επιλογή _Προβολή αρχείων_ 

![Επιλογή προβολής αρχείων](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

και να προσθέσετε ένα νέο αρχείο που ονομάζεται _project.json_ με το παρακάτω περιεχόμενο:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Κατά την _Αποθήκευση_ αυτό θα ενεργοποιήσει Azure συναρτήσεις για να κάνετε λήψη τα απαραίτητα δυαδικά δεδομένα. 

Μεταβείτε στην καρτέλα **ενσωματώσουν** και δώστε την παράμετρο χρονόμετρο ένα χαρακτηριστικό όνομα για να χρησιμοποιήσετε μέσα από τη συνάρτηση. Στο παραπάνω κώδικα, που θεωρεί ότι το χρονόμετρο κλήση _myTimer_. Καθορίστε μια [παράσταση CRON](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) ως εξής: 0 \* \* \* \* \* για το χρονόμετρο που θα προκαλέσει τη συνάρτηση για να εκτελέσετε μόνο μία φορά σε λεπτά. 

Στην καρτέλα **ενσωματώσουν το** ίδιο, προσθέστε μια εισαγωγής που θα είναι του τύπου _Χώρο αποθήκευσης Blob του Azure_. Αυτό θα οδηγεί στο αρχείο _sync.txt_ που περιέχει τη χρονική σήμανση του τελευταίου συμβάντος βλέπατε από τη συνάρτηση. Αυτό θα γίνουν διαθέσιμες εντός της συνάρτησης με το όνομα της παραμέτρου. Στο παραπάνω κώδικα, το χώρο αποθήκευσης Blob του Azure εισόδου αναμένει το όνομα της παραμέτρου για να _inputBlob_. Επιλέξτε το λογαριασμό χώρου αποθήκευσης όπου θα βρίσκονται στο αρχείο _sync.txt_ (θα μπορούσε να είναι η ίδια ή ένα λογαριασμό διαφορετικό χώρο αποθήκευσης) και στο πεδίο "διαδρομή", δώστε τη διαδρομή όπου βρίσκεται το αρχείο, στο πλαίσιο μορφή {container-name}/path/to/sync.txt.

Προσθέστε το αποτέλεσμα που θα είναι του τύπου _Χώρο αποθήκευσης Blob του Azure_ εξόδου. Αυτό θα οδηγεί επίσης το αρχείο _sync.txt_ ορίσατε στην είσοδο. Αυτό θα χρησιμοποιηθεί από τη συνάρτηση για να γράψετε τη χρονική σήμανση του τελευταίου συμβάντος βλέπατε. Ο κώδικας παραπάνω αναμένει αυτό παράμετρος κλήση _outputBlob_.

Σε αυτό το σημείο, η συνάρτηση είναι έτοιμοι. Βεβαιωθείτε ότι για να επιστρέψετε στην καρτέλα **Ανάπτυξη** και _αποθηκεύστε_ τον κώδικα. Ελέγξτε το παράθυρο εξόδου για τυχόν σφάλματα μεταγλώττισης και διορθώστε αυτά αντίστοιχα. Εάν συγκεντρώνει, στη συνέχεια, τώρα θα πρέπει να εκτελεί τον κώδικα και κάθε λεπτό θα Ελέγξτε τα αρχεία καταγραφής θάλαμο αριθμό-κλειδί και push οποιαδήποτε νέα συμβάντα σε καθορισμένο ουρά Bus υπηρεσίας. Θα πρέπει να βλέπετε πληροφορίες σύνδεσης που έχουν εγγραφεί για το αρχείο καταγραφής κάθε φορά παραθύρου ενεργοποιείται η συνάρτηση.

###<a name="azure-logic-app"></a>Εφαρμογή Azure λογικής

Στη συνέχεια θα πρέπει να δημιουργήσετε μια εφαρμογή λογικής Azure που θα σηκώστε τα συμβάντα που η συνάρτηση ώθηση στην ουρά Bus υπηρεσίας, η ανάλυση του περιεχομένου και στείλετε ένα μήνυμα ηλεκτρονικού ταχυδρομείου με βάση μια συνθήκη που ταιριάζουν.

[Δημιουργία μιας εφαρμογής λογικής](../app-service-logic/app-service-logic-create-a-logic-app.md) , μεταβαίνοντας στις επιλογές Δημιουργία -> λογική εφαρμογής. 

Όταν δημιουργηθεί η εφαρμογή λογικής, μεταβείτε σε αυτήν και επιλέξτε _Επεξεργασία_. Το πρόγραμμα επεξεργασίας του λογική εφαρμογής, επιλέξτε την _Υπηρεσία ουράς Bus_ -διαχειριζόμενο api και εισαγάγετε τα διαπιστευτήριά σας Bus υπηρεσία για να συνδεθεί στην ουρά.

![Azure λογικής Bus εφαρμογής υπηρεσίας](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Στη συνέχεια, επιλέξτε Προσθήκη _συνθήκης_. Στη συνθήκη, μεταβείτε στο πρόγραμμα _επεξεργασίας για προχωρημένους_ και καταχωρήστε τις παρακάτω, αντικαθιστώντας τα APP_ID με την πραγματική APP_ID της εφαρμογής web:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Αυτή η παράσταση ουσιαστικά θα επιστρέψει την **τιμή false** εάν η ιδιότητα **αναγνωριστικό εφαρμογής** από το συμβάν εισερχόμενες (που είναι το σώμα του μηνύματος υπηρεσίας Bus) δεν είναι το **αναγνωριστικό εφαρμογής** της εφαρμογής. 

Τώρα, δημιουργήστε μια ενέργεια κάτω από την επιλογή _Εάν όχι, μην κάνετε τίποτα..._ .

![Azure λογικής εφαρμογών, επιλέξτε ενέργεια](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Για την ενέργεια, επιλέξτε _Office 365 - αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου_. Συμπληρώστε τα πεδία για να δημιουργήσετε ένα μήνυμα ηλεκτρονικού ταχυδρομείου για να στείλετε όταν η καθορισμένη συνθήκη επιστρέφει false. Εάν δεν έχετε Office 365 που μπορεί να δείτε εναλλακτικές λύσεις για να επιτύχετε το ίδιο.

Σε αυτό το σημείο έχετε μια ολοκληρωμένη διοχέτευσης που, μία φορά σε λεπτά, θα φαίνεται για νέα αρχεία καταγραφής ελέγχου θάλαμο αριθμού-κλειδιού. Οποιαδήποτε νέα αρχεία καταγραφής εντοπίζει, αυτό θα push τους σε μια υπηρεσία ουρά Bus. Η εφαρμογή λογικής θα ενεργοποιηθεί μόλις καταλήξει σε νέο μήνυμα στην ουρά και εάν το αναγνωριστικό εφαρμογής μέσα στο συμβάν δεν συμφωνεί με το αναγνωριστικό εφαρμογής από την εφαρμογή κλήσης, στη συνέχεια, στείλτε ένα μήνυμα ηλεκτρονικού ταχυδρομείου. 
