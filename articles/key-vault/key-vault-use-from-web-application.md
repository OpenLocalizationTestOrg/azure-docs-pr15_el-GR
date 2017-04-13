<properties
    pageTitle="Χρήση του Azure κλειδιού θάλαμο από μια εφαρμογή Web | Microsoft Azure"
    description="Χρησιμοποιήστε αυτό το πρόγραμμα εκμάθησης για να σας βοηθήσει να μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure θάλαμο κλειδί από μια εφαρμογή web."
    services="key-vault"
    documentationCenter=""
    authors="adhurwit"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-azure-key-vault-from-a-web-application"></a>Χρήση του Azure κλειδιού θάλαμο από μια εφαρμογή Web #

## <a name="introduction"></a>Εισαγωγή  
Χρησιμοποιήστε αυτό το πρόγραμμα εκμάθησης για να σας βοηθήσει να μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure θάλαμο κλειδί από μια εφαρμογή web στο Azure. Σας καθοδηγεί κατά τη διαδικασία πρόσβαση σε έναν μυστικό από ένα θάλαμο κλειδί Azure ώστε να μπορεί να χρησιμοποιηθεί στην εφαρμογή web σας.

**Εκτιμώμενη χρόνο για να ολοκληρωθεί:** 15 λεπτά


Για επισκόπηση πληροφορίες σχετικά με το Azure κλειδί θάλαμο, ανατρέξτε στο θέμα [Τι είναι το Azure κλειδί θάλαμο;](key-vault-whatis.md)

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- Ένα URI για ένα μυστικό σε ένα θάλαμο κλειδί Azure
- Ένα Αναγνωριστικό υπολογιστή-πελάτη και ένα μυστικό προγράμματος-πελάτη για μια εφαρμογή web που έχουν καταχωρηθεί Azure Active Directory που έχει πρόσβαση για τον αριθμό-κλειδί θάλαμο
- Μια εφαρμογή web. Θα σας θα που δείχνει τα βήματα για μια εφαρμογή ASP.NET MVC αναπτυχθεί στο Azure ως μια εφαρμογή Web.

> [AZURE.NOTE]  Είναι απαραίτητο ότι έχετε ολοκληρώσει τα βήματα που αναφέρονται σε [Γρήγορα αποτελέσματα με το Azure θάλαμο αριθμού-κλειδιού](key-vault-get-started.md) για αυτό το πρόγραμμα εκμάθησης, ώστε να έχετε το URI για έναν μυστικό και το Αναγνωριστικό υπολογιστή-πελάτη και μυστικό προγράμματος-πελάτη για μια εφαρμογή web.

Η εφαρμογή web που θα έχουν πρόσβαση σε θάλαμο του αριθμού-κλειδιού είναι εκείνη που έχει καταχωρηθεί στο Azure Active Directory και έχει καθοριστεί πρόσβασης για τον αριθμό-κλειδί θάλαμο. Εάν αυτό δεν είναι πεζών και κεφαλαίων γραμμάτων, επιστρέψτε στο Register μια εφαρμογή στην εκμάθηση γρήγορα αποτελέσματα και επαναλάβετε τα βήματα που αναφέρονται.

Αυτό το πρόγραμμα εκμάθησης έχει σχεδιαστεί για τους προγραμματιστές web που κατανοήσετε τα βασικά στοιχεία για τη δημιουργία εφαρμογών web στο Azure. Για περισσότερες πληροφορίες σχετικά με τις εφαρμογές Web Azure, ανατρέξτε στο θέμα [Επισκόπηση Web Apps](../app-service-web/app-service-web-overview.md).



## <a id="packages"></a>Προσθήκη πακέτων Nuget ##
Υπάρχουν δύο πακέτων που η εφαρμογή web σας πρέπει να έχετε εγκαταστήσει.

- Βιβλιοθήκη ελέγχου ταυτότητας Active Directory - περιέχει μεθόδους για αλληλεπίδραση με Azure Active Directory και τη Διαχείριση ταυτότητα χρήστη
- Βιβλιοθήκη θάλαμο Azure κλειδί - περιέχει μεθόδους για αλληλεπίδραση με θάλαμο κλειδί Azure


Και τα δύο από αυτά τα πακέτα μπορεί να εγκατασταθεί χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου χρησιμοποιώντας την εντολή του πακέτου εγκατάστασης.

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Τροποποίηση Web.Config ##
Υπάρχουν τρεις ρυθμίσεις εφαρμογής που πρέπει να προστεθεί στο αρχείο web.config ως εξής.

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Εάν δεν σκοπεύετε να φιλοξενήσετε την εφαρμογή σας ως μια εφαρμογή Web της Azure, στη συνέχεια, θα πρέπει να προσθέτετε τις πραγματικές τιμές ClientId μυστικό προγράμματος-πελάτη και μυστικό URI στο Web.config. Διαφορετικά αφήστε εικονική αυτές τις τιμές, επειδή θα σας θα προσθέσετε τις πραγματικές τιμές στην πύλη του Azure για το πρόσθετο επίπεδο ασφάλειας.


## <a id="gettoken"></a>Προσθήκη μέθοδο για να λάβετε μια πρόσβαση διακριτικού ##
Για να χρησιμοποιήσετε το κλειδί API θάλαμο χρειάζεστε ένα διακριτικό πρόσβασης. Το πρόγραμμα-πελάτη θάλαμο κλειδί χειρισμού κλήσεων για το API θάλαμο αριθμού-κλειδιού αλλά πρέπει να παρέχετε την με μια συνάρτηση που λαμβάνει το διακριτικό πρόσβασης.  

Ακολουθεί ο κώδικας για να λάβετε μια πρόσβαση διακριτικού από το Azure Active Directory. Αυτός ο κωδικός να μεταβείτε σε οποιοδήποτε σημείο στην εφαρμογή σας. Να, όπως για να προσθέσετε μια κατηγορία βοηθητικά προγράμματα ή EncryptionHelper.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [AZURE.NOTE] 
> Χρησιμοποιώντας το Αναγνωριστικό υπολογιστή-πελάτη και μυστικό προγράμματος-πελάτη είναι ο ευκολότερος τρόπος για να ελέγχουν την ταυτότητα μιας εφαρμογής Azure AD. Και χρησιμοποιείτε στην εφαρμογή web σας επιτρέπει για το διαχωρισμό των καθηκόντων και ελέγχετε περισσότερο τη διαχείριση του κλειδιού σας. Αλλά το βασίζονται σχετικά με την τοποθέτηση το μυστικό προγράμματος-πελάτη στις ρυθμίσεις των παραμέτρων που για ορισμένες μπορεί να είναι ως επικίνδυνη ως τοποθέτηση το μυστικό που θέλετε να προστατεύσετε στο ρυθμίσεων παραμέτρων. Δείτε παρακάτω μια συζήτηση σχετικά με τον τρόπο για να χρησιμοποιήσετε ένα Αναγνωριστικό υπολογιστή-πελάτη και το πιστοποιητικό αντί για το Αναγνωριστικό υπολογιστή-πελάτη και μυστικό προγράμματος-πελάτη για τον έλεγχο ταυτότητας της εφαρμογής Azure AD.



## <a id="appstart"></a>Ανάκτηση το μυστικό στην εκκίνηση της εφαρμογής ##
Τώρα χρειαζόμαστε κώδικα για να καλέσετε το API θάλαμο κλειδί και να ανακτήσετε το μυστικό. Ο ακόλουθος κώδικας να τοποθετήσετε σε οποιοδήποτε σημείο εφόσον ονομάζεται πριν να χρειαστεί να το χρησιμοποιείτε. Να έχετε τοποθετήσετε αυτόν τον κώδικα στο συμβάν εκκίνηση της εφαρμογής στο το Global.asax έτσι ώστε να εκτελείται μία φορά κατά την έναρξη και είναι διαθέσιμες για την εφαρμογή το μυστικό.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec;



## <a id="portalsettings"></a>Προσθήκη των ρυθμίσεων της εφαρμογής στην πύλη του Azure (προαιρετικά) ##
Εάν έχετε μια εφαρμογή Web της Azure τώρα μπορείτε να προσθέσετε τις πραγματικές τιμές για το AppSettings στην πύλη του Azure. Με αυτόν τον τρόπο, τις πραγματικές τιμές δεν θα είναι το αρχείο web.config αλλά προστατεύονται μέσω της πύλης όπου έχετε δυνατότητες ελέγχου ξεχωριστή πρόσβαση. Αυτές οι τιμές θα αντικαταστήσει τις τιμές που έχετε εισαγάγει στο web.config σας. Βεβαιωθείτε ότι τα ονόματα είναι η ίδια.

![Ρυθμίσεις εφαρμογής που εμφανίζεται στην πύλη Azure][1]


## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Ο έλεγχος ταυτότητας με ένα πιστοποιητικό αντί για ένα μυστικό προγράμματος-πελάτη
Ένας άλλος τρόπος για να ελέγχουν την ταυτότητα μιας εφαρμογής Azure AD είναι χρησιμοποιώντας ένα Αναγνωριστικό υπολογιστή-πελάτη και ένα πιστοποιητικό αντί για ένα Αναγνωριστικό υπολογιστή-πελάτη και μυστικό προγράμματος-πελάτη. Ακολουθούν τα βήματα για να χρησιμοποιήσετε ένα πιστοποιητικό σε μια εφαρμογή Web Azure:

1. Αποκτήστε ή δημιουργήστε ένα πιστοποιητικό
2. Συσχέτιση του πιστοποιητικού με μια εφαρμογή του Azure AD
3. Προσθήκη κώδικα σε εφαρμογή Web για να χρησιμοποιήσετε το πιστοποιητικό
4. Προσθήκη ενός πιστοποιητικού σε εφαρμογή Web


**Αποκτήστε ή δημιουργήστε ένα πιστοποιητικό** Για τους σκοπούς μας θα κάνουμε ένα δοκιμαστικό πιστοποιητικό. Εδώ θα βρείτε ορισμένες εντολές που μπορείτε να χρησιμοποιήσετε σε μια προγραμματιστής γραμμή εντολών για να δημιουργήσετε ένα πιστοποιητικό. Αλλαγή καταλόγου στο σημείο όπου θέλετε να τα αρχεία πιστοποιητικού που θα δημιουργηθεί.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Σημειώστε την ημερομηνία λήξης και τον κωδικό πρόσβασης για το .pfx (σε αυτό το παράδειγμα: 31/07/2016 και test123). Θα πρέπει τα παρακάτω.

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία ένα δοκιμαστικό πιστοποιητικό, ανατρέξτε στο θέμα [ΔΙΑΔΙΚΑΣΙΕΣ: δημιουργία των δικών δοκιμή πιστοποιητικού](https://msdn.microsoft.com/library/ff699202.aspx)


**Συσχέτιση του πιστοποιητικού με μια εφαρμογή του Azure AD** Τώρα που έχετε ένα πιστοποιητικό, πρέπει να συσχετίσετε με μια εφαρμογή του Azure AD. Αλλά την πύλη διαχείρισης του Azure δεν υποστηρίζονται από αυτήν τη στιγμή. Αντί για αυτό, πρέπει να χρησιμοποιήσετε Powershell. Ακολουθούν οι εντολές που χρειάζεστε για να εκτελέσετε:

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2

    PS C:\> $x509.Import("C:\data\KVWebApp.cer")

    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    PS C:\> $now = [System.DateTime]::Now

    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31")

    PS C:\> $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow

    PS C:\> $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    PS C:\>$x509.Thumbprint

Αφού εκτελέσετε αυτές οι εντολές, μπορείτε να δείτε την εφαρμογή σε Azure AD. Εάν δεν βλέπετε την εφαρμογή στην πρώτη, αναζήτηση για "Εφαρμογές ανήκει η εταιρεία μου" αντί για "Εφαρμογές χρησιμοποιεί η εταιρεία μου".

Για να μάθετε περισσότερα σχετικά με το Azure AD εφαρμογής και ServicePrincipal αντικειμένων, ανατρέξτε στο θέμα [εφαρμογή και των αντικειμένων κεφάλαιο υπηρεσίας](../active-directory/active-directory-application-objects.md)



**Προσθήκη κώδικα για την εφαρμογή Web της χρήσης του πιστοποιητικού** Τώρα θα προσθέσουμε κώδικα για την εφαρμογή Web της πρόσβασης του πιστοποιητικού και να το χρησιμοποιήσετε για τον έλεγχο ταυτότητας.

Πρώτα υπάρχει κωδικός πρόσβασης του πιστοποιητικού.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Σημειώστε ότι το StoreLocation είναι CurrentUser αντί για LocalMachine. Και ότι θα σας παρέχετε 'false' για τη μέθοδο Find επειδή χρησιμοποιούμε ενός πιστοποιητικού δοκιμής.


Επόμενο είναι κώδικα που χρησιμοποιεί το CertificateHelper και δημιουργεί μια ClientAssertionCertificate που είναι απαραίτητη για τον έλεγχο ταυτότητας.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Παρακάτω θα δείτε το νέο κωδικό για να λάβετε το διακριτικό πρόσβασης. Αντικαθιστά την παραπάνω μέθοδο GetToken. Να έχετε δώσει το ένα διαφορετικό όνομα για τη διευκόλυνσή σας.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

Πρέπει να τα βάλετε όλα αυτόν τον κωδικό σε κλάση βοηθητικά προγράμματα μου του project Web App για μεγαλύτερη ευκολία στη χρήση.

Η τελευταία αλλαγή κώδικα είναι στη μέθοδο Application_Start. Πρώτα πρέπει να καλέσετε τη μέθοδο GetCert() για να φορτώσετε το ClientAssertionCertificate. Και, στη συνέχεια, αλλάξουμε τη μέθοδο επιστροφής κλήσης που θα σας παράσχει κατά τη δημιουργία ενός νέου KeyVaultClient. Σημειώστε ότι αυτό αντικαθιστά τον κώδικα που έπρεπε παραπάνω.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Προσθέστε ένα πιστοποιητικό για την εφαρμογή Web της μέσω της πύλης Azure** Προσθήκη ενός πιστοποιητικού σε εφαρμογή Web είναι μια διαδικασία δύο βημάτων απλό. Αρχικά, μεταβείτε στην πύλη του Azure και μεταβείτε σε εφαρμογή Web. Στην το blade ρυθμίσεις για την εφαρμογή Web σας, κάντε κλικ στην εγγραφή για "προσαρμοσμένων τομέων και SSL". Κατά το blade που ανοίγει που θα μπορεί να αποστείλετε το πιστοποιητικό που δημιουργήσατε παραπάνω, KVWebApp.pfx, βεβαιωθείτε ότι μπορείτε να θυμηθείτε τον κωδικό πρόσβασης για το pfx.

![Προσθήκη ενός πιστοποιητικού σε μια εφαρμογή Web στην πύλη του Azure][2]


Το τελευταίο πράγμα που πρέπει να κάνετε είναι να προσθέσετε μια ρύθμιση εφαρμογής σε εφαρμογή Web που περιλαμβάνει το όνομα τοποθεσίας Web\_ΦΌΡΤΩΣΗΣ\_ΠΙΣΤΟΠΟΙΗΤΙΚΆ και η τιμή *. Αυτό θα εξασφαλίσει ότι έχουν φορτωθεί όλα τα πιστοποιητικά. Εάν θέλετε να φορτώσετε μόνο τα πιστοποιητικά που έχετε αποστείλει, στη συνέχεια, μπορείτε να εισαγάγετε μια λίστα με τους thumbprints διαχωρισμένες με κόμματα.

Για να μάθετε περισσότερα σχετικά με την προσθήκη ενός πιστοποιητικού σε μια εφαρμογή Web, ανατρέξτε στο θέμα [Χρήση πιστοποιητικά σε εφαρμογές Azure τοποθεσίες Web](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)


**Προσθήκη ενός πιστοποιητικού στο πλήκτρο θάλαμο ως ένα μυστικό** Αντί να αποστείλετε το πιστοποιητικό σας απευθείας στην υπηρεσία Web App, μπορείτε να αποθηκεύσετε σε θάλαμο κλειδί ως έναν μυστικό και ανάπτυξή του από εκεί. Αυτή είναι μια διαδικασία δύο βημάτων που περιγράφεται στην παρακάτω καταχώρηση ιστολογίου, [Την ανάπτυξη Azure Web App πιστοποιητικό μέσω του αριθμού-κλειδιού θάλαμο](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)



## <a id="next"></a>Επόμενα βήματα ##


Για τον προγραμματισμό αναφορών, ανατρέξτε στο θέμα [Azure κλειδί θάλαμο C# προγράμματος-πελάτη API αναφορά](https://msdn.microsoft.com/library/azure/dn903628.aspx).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
