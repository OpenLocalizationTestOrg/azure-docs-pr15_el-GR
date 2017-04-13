<properties
    pageTitle="Πρόγραμμα εκμάθησης: Κρυπτογράφηση και αποκρυπτογράφηση αντικείμενα blob στο χώρο αποθήκευσης Microsoft Azure χρησιμοποιώντας θάλαμο κλειδί Azure | Microsoft Azure"
    description="Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί σε πώς μπορείτε να κρυπτογραφήσετε και να αποκρυπτογραφήσετε μια blob χρησιμοποιεί κρυπτογράφηση πλευρά του προγράμματος-πελάτη για το Microsoft Azure χώρου αποθήκευσης με θάλαμο κλειδί Azure."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="required"
    ms.date="10/18/2016"
    ms.author="lakasa;robinsh"/>

# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Πρόγραμμα εκμάθησης: Κρυπτογράφηση και αποκρυπτογράφηση αντικείμενα blob στο χώρο αποθήκευσης Microsoft Azure χρησιμοποιώντας θάλαμο κλειδί Azure

## <a name="introduction"></a>Εισαγωγή

Αυτό το πρόγραμμα εκμάθησης περιγράφει πώς μπορείτε να κάνετε χρήση κρυπτογράφησης αποθήκευσης πλευρά του προγράμματος-πελάτη με το Azure κλειδί θάλαμο. Σας καθοδηγεί με τη διαδικασία για να κρυπτογραφήσετε και να αποκρυπτογραφήσετε μια blob σε μια εφαρμογή κονσόλας χρησιμοποιώντας αυτές τις τεχνολογίες.

**Εκτιμώμενη χρόνο για να ολοκληρωθεί:** 20 λεπτά την

Για επισκόπηση πληροφορίες σχετικά με το Azure κλειδί θάλαμο, ανατρέξτε στο θέμα [Τι είναι το Azure κλειδί θάλαμο;](../key-vault/key-vault-whatis.md).

Για επισκόπηση πληροφορίες σχετικά με την πλευρά του προγράμματος-πελάτη κρυπτογράφησης για το χώρο αποθήκευσης Azure, ανατρέξτε στο θέμα [κρυπτογράφηση πλευρά του προγράμματος-πελάτη και θάλαμο κλειδί Azure για το χώρο αποθήκευσης στο Microsoft Azure](storage-client-side-encryption.md).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- Ένα λογαριασμό αποθήκευσης Azure
- Visual Studio 2013 ή νεότερη έκδοση
- Azure PowerShell


## <a name="overview-of-client-side-encryption"></a>Επισκόπηση της κρυπτογράφησης πλευρά του προγράμματος-πελάτη

Για μια επισκόπηση της κρυπτογράφησης πλευρά του προγράμματος-πελάτη για το χώρο αποθήκευσης Azure, ανατρέξτε στο θέμα [κρυπτογράφηση πλευρά του προγράμματος-πελάτη και θάλαμο κλειδί Azure για το χώρο αποθήκευσης στο Microsoft Azure](storage-client-side-encryption.md)

Ακολουθεί μια σύντομη περιγραφή του τρόπου λειτουργίας του κρυπτογράφησης πλευρά του προγράμματος-πελάτη:

1. Το πρόγραμμα-πελάτη αποθήκευσης Azure SDK δημιουργεί ένα κλειδί περιεχομένου κρυπτογράφησης (CEK), το οποίο είναι ένα πλήκτρο συμμετρική μία φορά χρήση.
2. Δεδομένα πελατών είναι κρυπτογραφημένη χρησιμοποιώντας αυτό CEK.
3. Το CEK, στη συνέχεια, αναδιπλώνεται (κρυπτογραφημένα) χρησιμοποιώντας το κλειδί κλειδιού κρυπτογράφησης (KEK). Το KEK προσδιορίζεται από ένα πλήκτρο αναγνωριστικό και μπορεί να είναι ένα ζεύγος κλειδιών ασύμμετρη ή έναν αριθμό-κλειδί συμμετρική και μπορεί να είναι διαχειριζόμενο τοπικά ή είναι αποθηκευμένα σε θάλαμο κλειδί Azure. Το πρόγραμμα-πελάτη χώρο αποθήκευσης στο ίδιο ποτέ αποκτά πρόσβαση το KEK. Το καλεί μόνο ο αλγόριθμος αναδίπλωσης κλειδιού που παρέχεται από θάλαμο αριθμού-κλειδιού. Οι πελάτες να επιλέξετε να χρησιμοποιήσετε προσαρμοσμένες υπηρεσίες παροχής για κλειδί αναδίπλωσης/κατάργηση αναδίπλωσης εάν θέλουν.
4. Το κρυπτογραφημένων δεδομένων είναι στη συνέχεια, που έχουν αποσταλεί με την υπηρεσία αποθήκευσης Azure.


## <a name="set-up-your-azure-key-vault"></a>Ρύθμιση του Azure θάλαμο αριθμού-κλειδιού
Για να συνεχίσετε με αυτό το πρόγραμμα εκμάθησης, πρέπει να κάνετε τα παρακάτω βήματα που περιγράφονται στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure θάλαμο αριθμού-κλειδιού](../key-vault/key-vault-get-started.md):

- Δημιουργία ενός κλειδιού θάλαμο.
- Προσθήκη αριθμού-κλειδιού ή μυστικό του κλειδιού θάλαμο.
- Καταχωρήστε μια εφαρμογή του Azure Active Directory.
- Εγκρίνετε την εφαρμογή για να χρησιμοποιήσετε το κλειδί ή το μυστικό.

Σημειώστε την ClientID και ClientSecret που δημιουργήθηκαν κατά την καταχώρηση μιας εφαρμογής με το Azure Active Directory.

Δημιουργήστε δύο αριθμών-κλειδιών στο του κλειδιού θάλαμο. Υποθέσουμε ότι για τα υπόλοιπα του προγράμματος εκμάθησης ότι έχετε χρησιμοποιήσει τα ονόματα των εξής: ContosoKeyVault και TestRSAKey1.


## <a name="create-a-console-application-with-packages-and-appsettings"></a>Δημιουργία μιας εφαρμογής κονσόλας με πακέτων και AppSettings

Στο Visual Studio, δημιουργήστε μια νέα εφαρμογή κονσόλας.

Προσθήκη πακέτων απαραίτητες nuget στην κονσόλα διαχείρισης πακέτου.

    Install-Package WindowsAzure.Storage

    // This is the latest stable release for ADAL.
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault
    Install-Package Microsoft.Azure.KeyVault.Extensions


Προσθέστε AppSettings το App.Config.

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

Προσθέστε τα ακόλουθα `using` προτάσεις και βεβαιωθείτε ότι για να προσθέσετε μια αναφορά σε System.Configuration στο έργο.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Προσθέστε μια μέθοδο για να λάβετε ένα διακριτικό στην εφαρμογή σας κονσόλας

Η ακόλουθη μέθοδος χρησιμοποιείται από κλάσεις θάλαμο αριθμό-κλειδί που πρέπει να πραγματοποιεί έλεγχο ταυτότητας για πρόσβαση σε σας κλειδιού θάλαμο.

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"],
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

## <a name="access-storage-and-key-vault-in-your-program"></a>Πρόσβαση στο χώρο αποθήκευσης και τον αριθμό-κλειδί θάλαμο στο πρόγραμμα

Στη συνάρτηση κύριες, προσθέστε τον ακόλουθο κώδικα.

    // This is standard code to interact with Blob storage.
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage.
    // This is where the GetToken method from above is used.
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] Πλήκτρο θάλαμο μοντέλα αντικειμένου
>
>Είναι σημαντικό να κατανοήσετε ότι δεν υπάρχουν στην πραγματικότητα δύο θάλαμο κλειδί μοντέλα αντικειμένου πρέπει να γνωρίζετε: μία βασίζεται σε το REST API (KeyVault χώρος ονομάτων) και το άλλο είναι μια επέκταση κρυπτογράφησης πλευρά του προγράμματος-πελάτη.

> Το πρόγραμμα-πελάτη θάλαμο κλειδί αλληλεπιδρά με το REST API και αναγνωρίζει πλήκτρα Web JSON και απορρήτου για τα δύο είδη στοιχεία που περιέχονται σε θάλαμο αριθμού-κλειδιού.

> Οι επεκτάσεις θάλαμο κλειδί είναι κλάσεις που φαίνεται να δημιουργηθεί ειδικά για την κρυπτογράφηση πλευρά του προγράμματος-πελάτη στο χώρο αποθήκευσης Azure. Που περιέχουν ένα περιβάλλον εργασίας για κλειδιά (IKey) και κατηγορίες με βάση την έννοια της επίλυσης έναν αριθμό-κλειδί. Υπάρχουν δύο υλοποιήσεις του IKey που πρέπει να γνωρίζετε: RSAKey και SymmetricKey. Τώρα τύχει να συμπίπτουν με τα στοιχεία που περιέχονται σε έναν αριθμό-κλειδί θάλαμο, αλλά σε αυτό το σημείο βρίσκονται ανεξάρτητη κλάσεις (, ώστε το κλειδί και μυστικό ανακτώνται από το πρόγραμμα-πελάτη θάλαμο κλειδί υλοποιεί IKey).


## <a name="encrypt-blob-and-upload"></a>Κρυπτογράφηση blob και αποστολή
Προσθέστε τον ακόλουθο κώδικα για να κρυπτογραφήσετε μια blob και στείλτε το λογαριασμό σας στο Azure χώρου αποθήκευσης. Η μέθοδος **ResolveKeyAsync** που χρησιμοποιείται επιστρέφει ένα IKey.


    // Retrieve the key that you created previously.
    // The IKey that is returned here is an RsaKey.
    // Remember that we used the names contosokeyvault and testrsakey1.
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob.
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method.
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


Ακολουθεί ένα στιγμιότυπο οθόνης από την [Πύλη κλασική Azure](https://manage.windowsazure.com) για ένα blob που έχει τεθεί κρυπτογραφημένα με τη χρήση κρυπτογράφησης πλευρά του προγράμματος-πελάτη με έναν αριθμό-κλειδί είναι αποθηκευμένα σε θάλαμο αριθμού-κλειδιού. Η ιδιότητα **αναγνωριστικό κλειδιού** είναι το URI για το κλειδί στο θάλαμο αριθμό-κλειδί που λειτουργεί ως το KEK. Η ιδιότητα **κρυπτογραφημένο κλειδί** περιέχει την κρυπτογραφημένη έκδοση του το CEK.

![Στιγμιότυπο οθόνης που εμφανίζει μετα-δεδομένα Blob που περιλαμβάνει κρυπτογράφηση μετα-δεδομένων][1]

> [AZURE.NOTE] Αν κοιτάξετε το κατασκευή BlobEncryptionPolicy, θα δείτε ότι μπορεί να δεχθεί έναν αριθμό-κλειδί ή/και μια επίλυση. Πρέπει να γνωρίζετε που δεξιά τώρα, δεν μπορείτε να χρησιμοποιήσετε μια επίλυση για την κρυπτογράφηση, επειδή κάνει τη δεδομένη στιγμή δεν υποστηρίζει ένα προεπιλεγμένο κλειδί.



## <a name="decrypt-blob-and-download"></a>Αποκρυπτογράφηση blob και λήψης
Αποκρυπτογράφηση είναι στην πραγματικότητα όταν χρησιμοποιώντας τις κατηγορίες επίλυσης νόημα. Το Αναγνωριστικό του κλειδιού που χρησιμοποιείται για την κρυπτογράφηση είναι συσχετισμένη με το αντικείμενο blob στο τα μετα-δεδομένα, ώστε να υπάρχει λόγος για να ανακτήσετε τον αριθμό-κλειδί και να θυμάστε ότι η συσχέτιση μεταξύ του αριθμού-κλειδιού και αντικειμένων blob. Έχετε απλώς για να βεβαιωθείτε ότι το κλειδί παραμένει στο πλήκτρο θάλαμο.   

Το ιδιωτικό κλειδί ενός κλειδιού RSA παραμένει στο πλήκτρο θάλαμο, ώστε να για αποκρυπτογράφηση να παρουσιάζεται, το πλήκτρο κρυπτογραφημένα από τα μετα-δεδομένα blob που περιέχει το CEK αποστέλλεται θάλαμο αριθμού-κλειδιού για αποκρυπτογράφηση.

Προσθέστε τα εξής για να αποκρυπτογραφήσετε τη blob που έχετε αποστείλει.

    // In this case, we will not pass a key and only pass the resolver because
    // this policy will only be used for downloading / decrypting.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] Υπάρχουν μερικά άλλα είδη επίλυσης για να διευκολύνετε την διαχείρισης κλειδιών, συμπεριλαμβανομένων των: AggregateKeyResolver και CachingKeyResolver.


## <a name="use-key-vault-secrets"></a>Χρήση του αριθμού-κλειδιού θάλαμο απορρήτου
Ο τρόπος για να χρησιμοποιήσετε ένα μυστικό με κρυπτογράφηση πλευρά του προγράμματος-πελάτη είναι μέσω της κλάσης SymmetricKey επειδή ένα μυστικό είναι ουσιαστικά έναν αριθμό-κλειδί συμμετρική. Ωστόσο, όπως αναφέρεται παραπάνω, ένα μυστικό στο πλήκτρο θάλαμο δεν αντιστοιχεί ακριβώς σε μια SymmetricKey. Υπάρχουν μερικά πράγματα που πρέπει να κατανοήσετε:


- Το κλειδί σε ένα SymmetricKey πρέπει να είναι μια σταθερή διάρκεια: 128, 192, 256, 384 ή 512 bit.
- Το κλειδί σε ένα SymmetricKey πρέπει να είναι κωδικοποίηση Base64.
- Ένα μυστικό θάλαμο αριθμό-κλειδί που θα χρησιμοποιηθεί ως ένα SymmetricKey πρέπει να έχει έναν τύπο περιεχομένου "εφαρμογή/οκτάδα-ροής" στο πλήκτρο θάλαμο.

Ακολουθεί ένα παράδειγμα στο PowerShell της δημιουργίας ενός μυστικό στο θάλαμο αριθμό-κλειδί που μπορούν να χρησιμοποιηθούν ως μια SymmetricKey.
ΣΗΜΕΊΩΣΗ: Η μόνιμη τιμή, $key, είναι για επίδειξη σκοπό μόνο. Το δικό σας κώδικα που θα θέλετε να δημιουργήσετε αυτό το κλειδί.

    // Here we are making a 128-bit key so we have 16 characters.
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128.
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // Substitute the VaultName and Name in this command.
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

Στην εφαρμογή σας console, μπορείτε να χρησιμοποιήσετε την ίδια κλήση ως πριν από την ανάκτηση αυτό μυστικό ως μια SymmetricKey.

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
        CancellationToken.None).GetAwaiter().GetResult();

Που είναι. Απολαύστε!

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με τη χρήση χώρος αποθήκευσης Microsoft Azure με C#, ανατρέξτε στο θέμα [Βιβλιοθήκης προγράμματος-πελάτη του Microsoft Azure χώρου αποθήκευσης για .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Για περισσότερες πληροφορίες σχετικά με το REST API Blob, ανατρέξτε στο θέμα [Blob υπηρεσίας REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Για τις πιο πρόσφατες πληροφορίες σχετικά με την Microsoft Azure αποθήκευσης, μεταβείτε στο [Ιστολόγιο ομάδας του Microsoft Azure χώρου αποθήκευσης](http://blogs.msdn.com/b/windowsazurestorage/).


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png
