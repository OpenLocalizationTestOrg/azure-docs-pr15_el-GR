<properties
   pageTitle="Χρήση δεδομένων λίμνης Store .NET SDK για να αναπτύξετε εφαρμογές | Microsoft Azure"
   description="Χρήση του Azure δεδομένων λίμνης Store .NET SDK για να αναπτύξετε εφαρμογές"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Γρήγορα αποτελέσματα με χρήση του .NET SDK χώρο αποθήκευσης λίμνης δεδομένων Azure

> [AZURE.SELECTOR]
- [Πύλη](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Μάθετε πώς μπορείτε να χρησιμοποιήσετε το [Azure δεδομένων λίμνης Store .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) για να εκτελέσετε βασικές λειτουργίες όπως όπως δημιουργία φακέλων, αποστολή και λήψη αρχείων δεδομένων, κ.λπ. Για περισσότερες πληροφορίες σχετικά με την λίμνης δεδομένων, ανατρέξτε στο θέμα [Χώρου αποθήκευσης λίμνης Azure δεδομένων](data-lake-store-overview.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* **Visual Studio 2013 ή του 2015**. Χρησιμοποιήστε το Visual Studio 2015 τις παρακάτω οδηγίες.

* **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

* **Λογαριασμός azure δεδομένων λίμνης Store**. Για οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα λογαριασμό, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης λίμνης δεδομένων Azure](data-lake-store-get-started-portal.md)

* **Δημιουργία μιας εφαρμογής υπηρεσίας καταλόγου Azure Active Directory**. Μπορείτε να χρησιμοποιήσετε την εφαρμογή Azure AD για τον έλεγχο ταυτότητας της εφαρμογής αποθήκευσης λίμνης δεδομένων με το Azure AD. Υπάρχουν διαφορετικές μεθόδους για τον έλεγχο ταυτότητας με Azure AD, που είναι **τελικού χρήστη ελέγχου ταυτότητας** ή **τον έλεγχο ταυτότητας υπηρεσίας εξυπηρέτησης**. Για οδηγίες και περισσότερες πληροφορίες σχετικά με τον τρόπο για τον έλεγχο ταυτότητας, ανατρέξτε στο θέμα [Έλεγχος ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων χρησιμοποιώντας Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Δημιουργία μιας εφαρμογής .NET

1. Ανοίξτε το Visual Studio και δημιουργία μιας εφαρμογής κονσόλας.

2. Από το μενού **αρχείο** , κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **έργο**.

3. Από το **Νέο έργο**, πληκτρολογήστε ή επιλέξτε τις ακόλουθες τιμές:

  	| Ιδιότητα | Τιμή                       |
  	|----------|-----------------------------|
  	| Κατηγορία | Πρότυπα/Visual C# / Windows |
  	| Πρότυπο | Εφαρμογή κονσόλας         |
  	| Όνομα     | CreateADLApplication        |

4. Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο.

5. Προσθέστε τα πακέτα Nuget στο έργο σας.

    1. Κάντε δεξί κλικ στο όνομα του έργου στην Εξερεύνηση λύσεων και κάντε κλικ στην επιλογή **Διαχείριση πακέτων NuGet**.
    2. Στην καρτέλα **Διαχείριση πακέτου Nuget** , βεβαιωθείτε ότι **το πακέτο προέλευσης** έχει οριστεί σε **nuget.org** και ότι το πλαίσιο ελέγχου **Συμπερίληψη προέκδοση** είναι επιλεγμένο.
    3. Αναζητήστε και εγκαταστήστε τα ακόλουθα πακέτα NuGet:

        * `Microsoft.Azure.Management.DataLake.Store`-Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί v0.12.5 preview.
        * `Microsoft.Azure.Management.DataLake.StoreUploader`-Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί v0.10.6 preview.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`-Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί v2.2.8 preview.

        ![Προσθήκη μιας προέλευσης Nuget] (./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Δημιουργία ενός νέου λογαριασμού λίμνης δεδομένων Azure")

    4. Κλείστε τη **Διαχείριση πακέτου Nuget**.

6. Άνοιγμα **Program.cs**, διαγράψτε τον υπάρχοντα κωδικό και, στη συνέχεια, να συμπεριλάβετε τις παρακάτω προτάσεις για να προσθέσετε αναφορές σε πεδία ονομάτων.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. Δηλώσετε τις μεταβλητές, όπως φαίνεται παρακάτω και παρέχουν τις τιμές για το όνομα του χώρου αποθήκευσης λίμνης δεδομένων και το όνομα της ομάδας πόρων που υπάρχουν ήδη. Επίσης, βεβαιωθείτε ότι το τοπικό διαδρομή και το όνομα αρχείου που παρέχετε εδώ πρέπει να υπάρχει στον υπολογιστή. Προσθέστε το παρακάτω τμήμα κώδικα μετά το δηλώσεις χώρου ονομάτων.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

Στις υπόλοιπες ενότητες αυτού του άρθρου, μπορείτε να δείτε πώς μπορείτε να χρησιμοποιήσετε τις διαθέσιμες μεθόδους .NET για την εκτέλεση λειτουργιών, όπως τον έλεγχο ταυτότητας, Αποστολή αρχείου, κ.λπ.

## <a name="authentication"></a>Έλεγχος ταυτότητας

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Εάν χρησιμοποιείτε τελικού χρήστη ελέγχου ταυτότητας (συνιστάται για αυτό το πρόγραμμα εκμάθησης)

Χρησιμοποιήστε αυτήν την επιλογή με μια υπάρχουσα Azure AD "Native Client" εφαρμογή; μία παρέχεται για εσάς παρακάτω. Για να σας βοηθήσει να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης πιο γρήγορα, συνιστάται να χρησιμοποιήσετε αυτήν την προσέγγιση.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Μερικά πράγματα που πρέπει να γνωρίζετε σχετικά με αυτό παραπάνω τμήματος κώδικα.

* Για να σας βοηθήσει να ολοκληρώσετε το πρόγραμμα εκμάθησης πιο γρήγορα, αυτό απόκομμα χρησιμοποιεί μια μια Azure AD Αναγνωριστικό τομέα και προγράμματος-πελάτη που είναι διαθέσιμη από προεπιλογή για όλες τις συνδρομές Azure. Επομένως, μπορείτε να κάνετε **Χρησιμοποιήστε αυτό τμήματος κώδικα ως-είναι στην εφαρμογή σας**.
* Ωστόσο, εάν θέλετε να χρησιμοποιήσετε το δικό σας Αναγνωριστικό υπολογιστή-πελάτη Azure AD τομέα και των εφαρμογών, που πρέπει να δημιουργήσετε μια εφαρμογή εγγενούς Azure AD και, στη συνέχεια, χρησιμοποιήστε τον τομέα Azure AD, Αναγνωριστικό υπολογιστή-πελάτη, και ανακατεύθυνση URI για την εφαρμογή που δημιουργήσατε. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία μια εφαρμογή του Active Directory](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) .

>[AZURE.NOTE] Τις οδηγίες στο θέμα τις παραπάνω συνδέσεις είναι για μια εφαρμογή web Azure AD. Ωστόσο, τα βήματα είναι ακριβώς ίδια ακόμα και εάν επιλέξατε να δημιουργήσετε μια εφαρμογή προγράμματος-πελάτη εγγενούς αντί για αυτό. 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Εάν χρησιμοποιείτε τον έλεγχο ταυτότητας υπηρεσίας εξυπηρέτησης με το μυστικό προγράμματος-πελάτη 

Το παρακάτω τμήμα κώδικα μπορεί να χρησιμοποιηθεί για τον έλεγχο ταυτότητας την εφαρμογή σας μη-αλληλεπιδραστικά, χρησιμοποιώντας το πρόγραμμα-πελάτη μυστικού / κλειδιών για μια εφαρμογή / υπηρεσίας κεφάλαιο. Χρησιμοποιήστε αυτήν την επιλογή με μια υπάρχουσα [εφαρμογή "Web App" Azure AD](../resource-group-create-service-principal-portal.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Εάν χρησιμοποιείτε τον έλεγχο ταυτότητας υπηρεσίας εξυπηρέτησης με πιστοποιητικό

Όπως μια τρίτη επιλογή, το παρακάτω τμήμα κώδικα μπορεί να χρησιμοποιηθεί για τον έλεγχο ταυτότητας την εφαρμογή σας μη-αλληλεπιδραστικά, χρησιμοποιώντας το πιστοποιητικό για μια εφαρμογή / υπηρεσίας κεφάλαιο. Χρησιμοποιήστε αυτήν την επιλογή με μια υπάρχουσα [εφαρμογή "Web App" Azure AD](../resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Δημιουργία αντικείμενα προγράμματος-πελάτη

Το παρακάτω τμήμα κώδικα δημιουργεί το χώρο αποθήκευσης δεδομένων λίμνης το λογαριασμό συστήματος αρχείων προγράμματος-πελάτη και αντικείμενα, τα οποία χρησιμοποιούνται για την έκδοση αιτήσεις για την υπηρεσία.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Λίστα όλοι οι λογαριασμοί χώρου αποθήκευσης λίμνης δεδομένων μέσα σε μια συνδρομή

Το παρακάτω τμήμα κώδικα παραθέτει όλους τους λογαριασμούς χώρου αποθήκευσης λίμνης δεδομένων μέσα σε μια δεδομένη Azure συνδρομή.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>Δημιουργία καταλόγου

Στο ακόλουθο τμήματος κώδικα μια `CreateDirectory` μέθοδο που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε έναν κατάλογο μέσα σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Αποστολή αρχείου

Στο ακόλουθο τμήματος κώδικα μια `UploadFile` μέθοδο που μπορείτε να χρησιμοποιήσετε για την αποστολή αρχείων σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`υποστηρίζει επαναλαμβανόμενες αποστολής και λήψης ανάμεσα σε μια τοπική διαδρομή αρχείου και μια διαδρομή αρχείου χώρου αποθήκευσης λίμνης δεδομένων.    

## <a name="get-file-or-directory-info"></a>Λήψη πληροφοριών του αρχείου ή του καταλόγου

Στο ακόλουθο τμήματος κώδικα μια `GetItemInfo` μέθοδο που μπορείτε να χρησιμοποιήσετε για να ανακτήσετε πληροφορίες σχετικά με ένα αρχείο ή κατάλογο διαθέσιμη στο χώρο αποθήκευσης λίμνης δεδομένων. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Λίστα αρχείων ή σε καταλόγους

Στο ακόλουθο τμήματος κώδικα μια `ListItem` μέθοδο που μπορεί να χρησιμοποιήσετε για μια λίστα των αρχείων και καταλόγων σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>CONCATENATE αρχείων

Στο ακόλουθο τμήματος κώδικα μια `ConcatenateFiles` μέθοδος που χρησιμοποιείτε για τη συνένωση των αρχείων. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Προσάρτηση σε ένα αρχείο

Στο ακόλουθο τμήματος κώδικα μια `AppendToFile` μέθοδος που χρησιμοποιείτε προσάρτηση δεδομένων σε ένα αρχείο που είναι αποθηκευμένο σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Λήψη ενός αρχείου

Στο ακόλουθο τμήματος κώδικα μια `DownloadFile` τη μέθοδο που χρησιμοποιείτε για να κάνετε λήψη ενός αρχείου από ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>Επόμενα βήματα

- [Διασφάλιση των δεδομένων στο χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-secure-data.md)
- [Χρήση ανάλυσης λίμνης Azure δεδομένων με το χώρο αποθήκευσης δεδομένων λίμνης](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Χρήση Azure HDInsight με το χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Αναφορά SDK χώρου αποθήκευσης λίμνης δεδομένων .NET](https://msdn.microsoft.com/library/mt581387.aspx)
- [Χώρος αποθήκευσης δεδομένων λίμνης ΥΠΌΛΟΙΠΑ αναφοράς](https://msdn.microsoft.com/library/mt693424.aspx)
