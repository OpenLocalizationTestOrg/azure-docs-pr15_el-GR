<properties
    pageTitle="Δημιουργία και χρήση ενός συσχετισμών Ασφαλείας με το χώρο αποθήκευσης αντικειμένων Blob | Microsoft Azure"
    description="Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να δημιουργήσετε υπογραφές κοινόχρηστη πρόσβαση για χρήση με το χώρο αποθήκευσης αντικειμένων Blob και τον τρόπο για την εκμετάλλευση τους από τις εφαρμογές προγράμματος-πελάτη σας."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Κοινή χρήση υπογραφών Access, μέρος 2: Δημιουργία και χρήση ενός συσχετισμών Ασφαλείας με το χώρο αποθήκευσης αντικειμένων Blob

## <a name="overview"></a>Επισκόπηση

[Μέρος 1](storage-dotnet-shared-access-signature-part-1.md) αυτού του προγράμματος εκμάθησης εξερευνήσει θέσει σε κοινή χρήση υπογραφών access (συσχετισμών Ασφαλείας) και επεξήγηση βέλτιστες πρακτικές για τη χρήση τους. Μέρος 2 δείχνει πώς μπορείτε να δημιουργήσετε και, στη συνέχεια, χρήση υπογραφών κοινόχρηστη πρόσβαση με το χώρο αποθήκευσης αντικειμένων Blob. Τα παραδείγματα είναι γραμμένες σε C# και χρησιμοποιήστε τη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για .NET. Τα σενάρια καλύπτεται περιλαμβάνουν αυτές τις πτυχές της εργασίας με τις υπογραφές κοινόχρηστη πρόσβαση:

- Δημιουργία υπογραφής κοινόχρηστη πρόσβαση σε ένα κοντέινερ
- Δημιουργία υπογραφής κοινόχρηστη πρόσβαση σε ένα αντικείμενο blob
- Δημιουργία πολιτικής αποθηκευμένες access για να διαχειριστείτε υπογραφές σε ένα κοντέινερ πόροι
- Δοκιμές τις υπογραφές κοινόχρηστη πρόσβαση από μια εφαρμογή προγράμματος-πελάτη

## <a name="about-this-tutorial"></a>Σχετικά με αυτό το πρόγραμμα εκμάθησης

Σε αυτό το πρόγραμμα εκμάθησης, θα σας θα εστίαση σχετικά με τη δημιουργία υπογραφών κοινόχρηστη πρόσβαση για κοντέινερ και αντικείμενα blob, δημιουργώντας δύο κονσόλας εφαρμογές. Την πρώτη εφαρμογή κονσόλας δημιουργεί υπογραφές κοινόχρηστη πρόσβαση σε ένα κοντέινερ και σε ένα αντικείμενο blob. Αυτή η εφαρμογή έχει γνώσεις για τα πλήκτρα για το λογαριασμό χώρου αποθήκευσης. Δεύτερη εφαρμογή κονσόλας και οι οποίες θα λειτουργεί ως μια εφαρμογή προγράμματος-πελάτη, αποκτά πρόσβαση σε κοντέινερ και αντικειμένων blob πόρων χρησιμοποιώντας τις υπογραφές κοινόχρηστης πρόσβασης που δημιουργήθηκαν με την πρώτη εφαρμογή. Αυτή η εφαρμογή χρησιμοποιεί τις υπογραφές κοινόχρηστη πρόσβαση μόνο για τον έλεγχο ταυτότητας την πρόσβαση στο κοντέινερ και αντικειμένων blob πόροι - δεν έχει γνώσεις για τα πλήκτρα για το λογαριασμό.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>Μέρος 1: Δημιουργία μιας εφαρμογής κονσόλας για τη δημιουργία υπογραφών κοινόχρηστη πρόσβαση

Πρώτα, βεβαιωθείτε ότι έχετε στη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για .NET εγκατεστημένο. Μπορείτε να εγκαταστήσετε το [πακέτο NuGet](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet πακέτο") που περιέχει τις πιο πρόσφατες συγκροτήσεις για τη βιβλιοθήκη προγράμματος-πελάτη; Αυτή είναι η προτεινόμενη μέθοδος για την εξασφάλιση ότι έχετε τις πιο πρόσφατες ενημερώσεις κώδικα. Μπορείτε επίσης να κάνετε λήψη στη βιβλιοθήκη προγράμματος-πελάτη ως μέρος της πιο πρόσφατης έκδοσης του [SDK Azure για το .NET](https://azure.microsoft.com/downloads/).

Στο Visual Studio, δημιουργήστε μια νέα εφαρμογή κονσόλας Windows και ονομάστε τον **GenerateSharedAccessSignatures**. Προσθέστε αναφορές σε **Microsoft.WindowsAzure.Configuration.dll** και **Microsoft.WindowsAzure.Storage.dll**, χρησιμοποιώντας μία από τις παρακάτω μεθόδους:

-   Εάν θέλετε να εγκαταστήσετε το πακέτο NuGet, πρώτα να εγκαταστήσετε το [Πρόγραμμα-πελάτη NuGet](https://docs.nuget.org/consume/installing-nuget). Στο Visual Studio, επιλέξτε **έργου | Διαχείριση πακέτων NuGet**, αναζήτηση στο Internet για το **Χώρο αποθήκευσης Azure**και ακολουθήστε τις οδηγίες για την εγκατάσταση.
-   Εναλλακτικά, εντοπίστε το συγκροτήσεις της εγκατάστασης του Azure SDK και προσθέστε αναφορές σε αυτά.

Στο επάνω μέρος του αρχείου Program.cs, προσθέστε τις ακόλουθες δηλώσεις **χρησιμοποιώντας** :

    using System.IO;    
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Επεξεργαστείτε το αρχείο app.config, ώστε να περιέχει μια ρύθμιση παραμέτρων με μια συμβολοσειρά σύνδεσης που οδηγεί στο λογαριασμό σας χώρου αποθήκευσης. Το αρχείο app.config θα πρέπει να μοιάζει με αυτό:

    <configuration>
      <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
      </appSettings>
    </configuration>

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Δημιουργία υπογραφής κοινόχρηστη πρόσβαση URI για ένα κοντέινερ

Για να με αρχήν, θα προσθέσουμε μια μέθοδο για να δημιουργήσετε μια υπογραφή κοινόχρηστη πρόσβαση σε ένα νέο κοντέινερ. Σε αυτήν την περίπτωση η υπογραφή δεν συσχετίζεται με μια πολιτική αποθηκευμένες πρόσβασης, ώστε να μεταφέρει στο URI τις πληροφορίες που υποδεικνύει την ώρα λήξης και τα δικαιώματα που χορηγεί.

Πρώτα, προσθέστε κώδικα στη μέθοδο **Main()** να ελέγχουν την ταυτότητα πρόσβαση στο λογαριασμό σας χώρου αποθήκευσης και να δημιουργήσετε ένα νέο κοντέινερ:

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Insert calls to the methods created below here...

        //Require user input before closing the console window.
        Console.ReadLine();
    }

Στη συνέχεια, προσθέστε μια νέα μέθοδο που δημιουργεί την υπογραφή κοινόχρηστη πρόσβαση για το κοντέινερ και επιστρέφει την υπογραφή URI:

    static string GetContainerSasUri(CloudBlobContainer container)
    {
        //Set the expiry time and permissions for the container.
        //In this case no start time is specified, so the shared access signature becomes valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List;

        //Generate the shared access signature on the container, setting the constraints directly on the signature.
        string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Προσθέστε τις ακόλουθες γραμμές στο κάτω μέρος της μεθόδου **Main()** , πριν από την κλήση σε **Console.ReadLine()**, για να καλέσετε **GetContainerSasUri()** και Γράψτε την υπογραφή URI στο παράθυρο κονσόλας:

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

Μεταγλώττιση και εκτέλεση για να εξαγάγετε την υπογραφή κοινόχρηστη πρόσβαση URI για το νέο κοντέινερ. Το URI θα είναι παρόμοιο με το ακόλουθο URI:       

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D

Αφού εκτελέσετε τον κώδικα, η κοινόχρηστη πρόσβαση υπογραφή που δημιουργήσατε στο κοντέινερ θα ισχύει για το επόμενο 24 ωρών. Η υπογραφή εκχωρεί ένα πρόγραμμα-πελάτη δικαίωμα λίστα αντικείμενα blob στο κοντέινερ και να συντάξετε ένα νέο blob στο κοντέινερ.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Δημιουργία υπογραφής κοινόχρηστη πρόσβαση URI για ένα Blob

Στη συνέχεια, θα σας θα παρόμοια κώδικας σύνταξης για να δημιουργήσετε ένα νέο blob μέσα στο κοντέινερ και να δημιουργήσει μια υπογραφή κοινόχρηστη πρόσβαση για αυτήν. Αυτή η υπογραφή κοινόχρηστη πρόσβαση δεν είναι συσχετισμένη με μια πολιτική πρόσβασης αποθηκευμένες, ώστε να περιλαμβάνει την ώρα έναρξης, η ώρα λήξης και δικαιωμάτων πληροφοριών στο URI.

Προσθέστε μια νέα μέθοδος που δημιουργεί ένα νέο blob και γράψτε κάποιο κείμενο σε αυτήν, στη συνέχεια, δημιουργεί μια υπογραφή κοινόχρηστη πρόσβαση και επιστρέφει την υπογραφή URI:

    static string GetBlobSasUri(CloudBlobContainer container)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a Shared Access Signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Set the expiry time and permissions for the blob.
        //In this case the start time is specified as a few minutes in the past, to mitigate clock skew.
        //The shared access signature will be valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

        //Generate the shared access signature on the blob, setting the constraints directly on the signature.
        string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Στο κάτω μέρος της μεθόδου **Main()** , προσθέστε τις ακόλουθες γραμμές για να καλέσετε **GetBlobSasUri()**, πριν από την κλήση σε **Console.ReadLine()**, και εγγραφής στην κοινόχρηστη πρόσβαση υπογραφή URI στο παράθυρο κονσόλας:    

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();


Μεταγλώττιση και εκτέλεση για να εξαγάγετε την υπογραφή κοινόχρηστη πρόσβαση URI για το νέο blob. Το URI θα είναι παρόμοιο με το ακόλουθο URI:

    https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D

### <a name="create-a-stored-access-policy-on-the-container"></a>Δημιουργήστε μια πολιτική πρόσβασης αποθηκευμένες στο κοντέινερ

Τώρα ας δημιουργήσουμε μια πολιτική πρόσβασης αποθηκευμένες στο κοντέινερ, που θα καθορίσει τους περιορισμούς για τις υπογραφές κοινόχρηστης πρόσβασης που συσχετίζονται με το.

Στα προηγούμενα παραδείγματα, θα σας συγκεκριμένο της ώρας έναρξης (σιωπηρά ή ρητά), το χρόνο λήξης, καθώς και τα δικαιώματα της υπογραφής κοινόχρηστη πρόσβαση URI ίδια. Στα παρακάτω παραδείγματα, θα σας θα Καθορίστε τα εξής στην πολιτική πρόσβασης αποθηκευμένες και όχι από την υπογραφή κοινόχρηστη πρόσβαση. Αυτή η ενέργεια επιτρέπει μας για να αλλάξετε αυτούς τους περιορισμούς χωρίς επανέκδοση την υπογραφή κοινόχρηστη πρόσβαση.

Είναι πιθανό να έχουν ένα ή περισσότερα από τους περιορισμούς της υπογραφής κοινόχρηστη πρόσβαση και το υπόλοιπο στην πολιτική αποθηκευμένες πρόσβασης. Ωστόσο, μπορείτε να καθορίσετε μόνο την ώρα έναρξης, η ώρα λήξης και τα δικαιώματα σε ένα σημείο ή το άλλο. Για παράδειγμα, δεν μπορείτε να καθορίσετε δικαιώματα στην υπογραφή κοινόχρηστη πρόσβαση και επίσης τα καθορίσετε στην πολιτική αποθηκευμένες πρόσβασης.

Σημείωση που όταν προσθέτετε μια πολιτική πρόσβασης σε ένα κοντέινερ, πρέπει να λάβετε υπάρχοντα δικαιώματα στο κοντέινερ, προσθέστε τη νέα πολιτική πρόσβασης και, στη συνέχεια, ορισμός δικαιωμάτων στο κοντέινερ.

Προσθέστε μια νέα μέθοδο που δημιουργεί μια νέα πολιτική πρόσβασης αποθηκευμένες σε ένα κοντέινερ και επιστρέφει το όνομα της πολιτικής:

    static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
        string policyName)
    {
        //Create a new shared access policy and define its constraints.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
        };

        //Get the container's existing permissions.
        BlobContainerPermissions permissions = container.GetPermissions();

        //Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        container.SetPermissions(permissions);
    }

Στο κάτω μέρος της μεθόδου **Main()** , πριν από την κλήση σε **Console.ReadLine()**, προσθέστε τις ακόλουθες γραμμές στο πρώτο Απαλοιφή τυχόν υπάρχουσες πολιτικές πρόσβασης και, στη συνέχεια, η κλήση της μεθόδου **CreateSharedAccessPolicy()** :    

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

Σημειώστε ότι όταν καταργείτε τις πολιτικές πρόσβασης σε ένα κοντέινερ, που πρέπει να πρώτα λήψη υπάρχοντα δικαιώματα στο κοντέινερ, στη συνέχεια, καταργήστε την επιλογή από τα δικαιώματα, στη συνέχεια, ορίστε τα δικαιώματα ξανά.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Δημιουργία υπογραφής κοινόχρηστη πρόσβαση URI στο κοντέινερ που χρησιμοποιεί μια πολιτική πρόσβασης

Στη συνέχεια, θα δημιουργήσουμε μια άλλη κοινόχρηστη πρόσβαση υπογραφής στο κοντέινερ που δημιουργήσαμε προηγουμένως, αλλά αυτήν τη φορά θα σας θα συσχετίσετε την υπογραφή με την πολιτική πρόσβασης που δημιουργήσαμε στο προηγούμενο παράδειγμα.

Προσθέστε μια νέα μέθοδο για να δημιουργήσει μια άλλη κοινόχρηστη πρόσβαση υπογραφή στο κοντέινερ:

    static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Generate the shared access signature on the container. In this case, all of the constraints for the
        //shared access signature are specified on the stored access policy.
        string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Στο κάτω μέρος της μεθόδου **Main()** , πριν από την κλήση σε **Console.ReadLine()**, προσθέστε τις ακόλουθες γραμμές για να καλέσετε τη μέθοδο **GetContainerSasUriWithPolicy** :

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>Δημιουργία υπογραφής κοινόχρηστη πρόσβαση URI στην το αντικείμενο Blob που χρησιμοποιεί μια πολιτική πρόσβασης

Τέλος, θα προσθέσουμε μια παρόμοια μέθοδο για να δημιουργήσετε μια άλλη blob και να δημιουργήσει μια υπογραφή κοινόχρηστη πρόσβαση που είναι συσχετισμένη με μια πολιτική πρόσβασης.

Προσθέστε μια νέα μέθοδο για να δημιουργήσετε ένα blob και να δημιουργήσει μια υπογραφή κοινόχρηστη πρόσβαση:

    static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a shared access signature. " +
        "A stored access policy defines the constraints for the signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Generate the shared access signature on the blob.
        string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Στο κάτω μέρος της μεθόδου **Main()** , πριν από την κλήση σε **Console.ReadLine()**, προσθέστε τις ακόλουθες γραμμές για να καλέσετε τη μέθοδο **GetBlobSasUriWithPolicy** :    

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

Η μέθοδος **Main()** πρέπει τώρα να μοιάζει ως εξής στο σύνολό. Εκτελέστε το για να γράψετε την υπογραφή κοινόχρηστη πρόσβαση URIs στο παράθυρο της κονσόλας, στη συνέχεια, αντιγράψτε και επικολλήστε τα σε ένα αρχείο κειμένου για χρήση στο δεύτερο τμήμα αυτού του προγράμματος εκμάθησης.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Generate a SAS URI for the container, without a stored access policy.
        Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, without a stored access policy.
        Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
        Console.WriteLine();

        //Clear any existing access policies on container.
        BlobContainerPermissions perms = container.GetPermissions();
        perms.SharedAccessPolicies.Clear();
        container.SetPermissions(perms);

        //Create a new access policy on the container, which may be optionally used to provide constraints for
        //shared access signatures on the container and the blob.
        string sharedAccessPolicyName = "tutorialpolicy";
        CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

        //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        Console.ReadLine();
    }

Όταν εκτελείτε την εφαρμογή κονσόλας GenerateSharedAccessSignatures, θα δείτε εξόδου παρόμοια με τα εξής στο παράθυρο της κονσόλας. Αυτές είναι οι υπογραφές κοινόχρηστης πρόσβασης που θα χρησιμοποιήσετε στο μέρος 2 του προγράμματος εκμάθησης.

![συσχετίσεις ασφαλείας-console-εξόδου-1][sas-console-output-1]

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>Μέρος 2: Δημιουργία μιας εφαρμογής κονσόλας για να ελέγξετε τις υπογραφές κοινόχρηστη πρόσβαση

Για να ελέγξετε τις υπογραφές κοινόχρηστη πρόσβαση που δημιουργήσατε στα προηγούμενα παραδείγματα, θα δημιουργήσουμε μια δεύτερη εφαρμογή κονσόλας που χρησιμοποιεί τις υπογραφές για να εκτελέσετε λειτουργίες στο κοντέινερ και σε ένα αντικείμενο blob.

> [AZURE.NOTE] Εάν έχουν περάσει περισσότερο από 24 ώρες, εφόσον έχετε ολοκληρώσει το πρώτο μέρος του προγράμματος εκμάθησης, οι υπογραφές που δημιουργήσατε δεν θα είναι πλέον έγκυρες. Σε αυτήν την περίπτωση, πρέπει να μπορείτε να εκτελέσετε τον κώδικα στην πρώτη εφαρμογή κονσόλας για τη δημιουργία υπογραφών Φρέσκο κοινόχρηστη πρόσβαση για χρήση στο δεύτερο τμήμα του προγράμματος εκμάθησης.

Στο Visual Studio, δημιουργήστε μια νέα εφαρμογή κονσόλας Windows και ονομάστε τον **ConsumeSharedAccessSignatures**. Προσθέστε αναφορές σε **Microsoft.WindowsAzure.Configuration.dll** και **Microsoft.WindowsAzure.Storage.dll**, όπως κάνατε προηγουμένως.

Στο επάνω μέρος του αρχείου Program.cs, προσθέστε τις ακόλουθες δηλώσεις **χρησιμοποιώντας** :

    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Στο σώμα της μεθόδου **Main()** , προσθέστε τις ακόλουθες σταθερές και ενημερώστε τις τιμές για τις υπογραφές κοινόχρηστη πρόσβαση που δημιουργήσατε στο μέρος 1 του προγράμματος εκμάθησης.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
    }

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Προσθέστε μια μέθοδο για να δοκιμάσετε λειτουργίες κοντέινερ, χρησιμοποιώντας μια υπογραφή κοινόχρηστη πρόσβαση

Στη συνέχεια, θα προσθέσουμε μια μέθοδο που ελέγχει ορισμένες λειτουργίες αντιπρόσωπος κοντέινερ, χρησιμοποιώντας μια υπογραφή κοινόχρηστη πρόσβαση στο κοντέινερ. Σημειώστε ότι η υπογραφή κοινόχρηστη πρόσβαση χρησιμοποιείται για να επιστρέψει μια αναφορά στο κοντέινερ, τον έλεγχο ταυτότητας πρόσβασης στο κοντέινερ με βάση την υπογραφή από μόνο του.

Προσθέστε την ακόλουθη μέθοδο Program.cs:

    static void UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

        //Create a list to store blob URIs returned by a listing operation on the container.
        List<ICloudBlob> blobList = new List<ICloudBlob>();

        //Write operation: write a new blob to the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //List operation: List the blobs in the container.
        try
        {
            foreach (ICloudBlob blob in container.ListBlobs())
            {
                blobList.Add(blob);
            }
            Console.WriteLine("List operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("List operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Get a reference to one of the blobs in the container and read it.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            MemoryStream msRead = new MemoryStream();
            msRead.Position = 0;
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                Console.WriteLine(msRead.Length);
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        Console.WriteLine();

        //Delete operation: Delete a blob in the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Ενημερώστε τη μέθοδο **Main()** για να καλέσετε **UseContainerSAS()** με και τα δύο από τις υπογραφές κοινόχρηστης πρόσβασης που δημιουργήσατε στο κοντέινερ:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        Console.ReadLine();
    }


### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Προσθέστε μια μέθοδο για να δοκιμάσετε Blob λειτουργίες χρησιμοποιώντας μια υπογραφή κοινόχρηστη πρόσβαση

Τέλος, θα προσθέσουμε μια μέθοδο που ελέγχει ορισμένες λειτουργίες αντιπρόσωπος blob χρησιμοποιώντας μια υπογραφή κοινόχρηστη πρόσβαση στην το αντικείμενο blob. Σε αυτήν την περίπτωση χρησιμοποιούμε την κατασκευή **CloudBlockBlob(String)**, που περνά μέσα στην υπογραφή κοινόχρηστη πρόσβαση, για να επιστρέψει μια αναφορά σε το αντικείμενο blob. Χωρίς έλεγχο ταυτότητας είναι απαραίτητη. είναι το με βάση την υπογραφή από μόνο του.

Προσθέστε την ακόλουθη μέθοδο Program.cs:

    static void UseBlobSAS(string sas)
    {
        //Try performing blob operations using the SAS provided.

        //Return a reference to the blob using the SAS URI.
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

        //Write operation: Write a new blob to the container.
        try
        {
            string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Read the contents of the blob.
        try
        {
            MemoryStream msRead = new MemoryStream();
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                msRead.Position = 0;
                using (StreamReader reader = new StreamReader(msRead, true))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                    }
                }
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Delete operation: Delete the blob.
        try
        {
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Ενημερώστε τη μέθοδο **Main()** για να καλέσετε **UseBlobSAS()** με και τα δύο από τις υπογραφές κοινόχρηστης πρόσβασης που δημιουργήσατε σε το αντικείμενο blob:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
        UseBlobSAS(blobSAS);
        UseBlobSAS(blobSASWithAccessPolicy);

        Console.ReadLine();
    }

Εκτελέστε την εφαρμογή κονσόλας και να παρακολουθήσετε την έξοδο για να δείτε τις λειτουργίες που επιτρέπονται για ποια υπογραφές. Το αποτέλεσμα στο παράθυρο της κονσόλας θα είναι παρόμοιο με το εξής:

![συσχετίσεις ασφαλείας-console-εξόδου-2][sas-console-output-2]

## <a name="next-steps"></a>Επόμενα βήματα

[Κοινή χρήση υπογραφών Access, μέρος 1: Κατανόηση του μοντέλου συσχετισμών Ασφαλείας](storage-dotnet-shared-access-signature-part-1.md)

[Διαχείριση ανώνυμη πρόσβαση για ανάγνωση σε κοντέινερ και αντικείμενα BLOB](storage-manage-access-to-resources.md)

[Εκχώρηση πρόσβασης με μια κοινόχρηστη πρόσβαση υπογραφή (REST API)](http://msdn.microsoft.com/library/azure/ee395415.aspx)

[Εισαγωγή πίνακα και ουρά συσχετισμών Ασφαλείας](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-console-output-1]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-1.PNG
[sas-console-output-2]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-2.PNG
