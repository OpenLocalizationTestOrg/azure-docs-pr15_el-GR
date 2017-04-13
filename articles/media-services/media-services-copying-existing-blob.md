<properties 
    pageTitle="Αντιγραφή μιας υπάρχουσας Blob στον πολυμέσων υπηρεσιών περιουσιακών στοιχείων | Microsoft Azure" 
    description="Αυτό το θέμα εξηγεί τον τρόπο για να αντιγράψετε μια υπάρχουσα blob σε ενός περιουσιακού στοιχείου υπηρεσίες πολυμέσων." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/13/2016" 
    ms.author="juliako"/>

#<a name="copying-an-existing-blob-into-a-media-services-asset"></a>Αντιγραφή μιας υπάρχουσας Blob στον ενός περιουσιακού στοιχείου υπηρεσίες πολυμέσων

Αυτό το θέμα δείχνει πώς μπορείτε να αντιγράψετε αντικείμενα BLOB από ένα λογαριασμό χώρου αποθήκευσης σε ένα νέο πάγιο Microsoft Azure Media Services.

Σας αντικείμενα BLOB μπορεί να υπάρχουν σε ένα λογαριασμό χώρου αποθήκευσης που είναι συσχετισμένες με το λογαριασμό Media Services ή το λογαριασμό χώρου αποθήκευσης που δεν σχετίζεται με το λογαριασμό Media Services. Αυτό το θέμα παρουσιάζει τον τρόπο για την αντιγραφή αντικειμένων blob από ένα λογαριασμό χώρου αποθήκευσης ενός περιουσιακού στοιχείου υπηρεσίες πολυμέσων. Σημειώστε ότι μπορείτε επίσης να αντιγράψετε σε κέντρα δεδομένων. Ωστόσο, μπορεί να υπάρχουν χρεώσεις με αυτόν τον τρόπο. Για περισσότερες πληροφορίες σχετικά με τις τιμές, ανατρέξτε στο θέμα [Μεταφορά δεδομένων](https://azure.microsoft.com/pricing/#header-11).

>[AZURE.NOTE] Δεν πρέπει να επιχειρήσετε να αλλάξετε τα περιεχόμενα του κοντέινερ αντικειμένων blob που δημιουργήθηκαν από το Media Services χωρίς τη χρήση APIs υπηρεσίας πολυμέσων.

##<a name="download-sample"></a>Λήψη δείγματος

Λήψη και να εκτελέσετε ένα δείγμα από [εδώ](https://azure.microsoft.com/documentation/samples/media-services-dotnet-copy-blob-into-asset/).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- Δύο λογαριασμούς Media Services σε μια νέα ή υπάρχουσα συνδρομή Azure. Ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε ένα λογαριασμό υπηρεσίες πολυμέσων](media-services-portal-create-account.md).
- Λειτουργικά συστήματα: Windows 10, Windows 7, Windows 2008 R2 ή Windows 8.
- .NET framework διαίρεσης 4,5.
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate ή Express) ή νεότερη έκδοση.

##<a name="set-up-your-project"></a>Ρύθμιση του έργου σας

Σε αυτήν την ενότητα που θα δημιουργήσετε και ρύθμιση του έργου εφαρμογής κονσόλας C#.

1. Χρησιμοποιήστε το Visual Studio για να δημιουργήσετε μια νέα λύση που περιέχει το έργο εφαρμογής κονσόλας C#. 
2. Πληκτρολογήστε CopyExistingBlobsIntoAsset για το όνομα και, στη συνέχεια, κάντε κλικ στο κουμπί OK.
1. Χρησιμοποιήστε Nuget για να προσθέσετε αναφορές για σχετικές βιβλιοθήκες DLL Media Services. Στο κύριο μενού Visual Studio, επιλέξτε ΕΡΓΑΛΕΊΑ, -> Διαχείριση πακέτου βιβλιοθήκη -> Κονσόλα διαχείρισης πακέτου. Στο παράθυρο πληκτρολογήστε κονσόλας εγκατάσταση πακέτου windowsazure.mediaservices και πιέστε το πλήκτρο enter.
1. Προσθέστε άλλες αναφορές που απαιτούνται για αυτό το έργο: System.Configuration.
1. Αντικατάσταση χρήση προτάσεων που έχουν προστεθεί στο αρχείο Programs.cs από προεπιλογή με τις εξής τιμές:
        
        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using System.Collections.Generic;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure;
        using System.Web;
        using Microsoft.WindowsAzure.Storage.Blob;
        using Microsoft.WindowsAzure.Storage.Auth;

1. Προσθέστε την ενότητα appSettings στο αρχείο .config και να ενημερώσετε τις τιμές με βάση τον αριθμό-κλειδί υπηρεσίες πολυμέσων και την αποθήκευση και τιμές για το όνομα. 

        <appSettings>
          <add key="MediaServicesAccountName" value="Media-Services-Account-Name"/>
          <add key="MediaServicesAccountKey" value="Media-Services-Account-Key"/>
          <add key="MediaServicesStorageAccountName" value="Media-Services-Storage-Account-Name"/>
          <add key="MediaServicesStorageAccountKey" value="Media-Services-Storage-Account-Key"/>
          <add key="ExternalStorageAccountName" value="External-Storage-Account-Name"/>
          <add key="ExternalStorageAccountKey" value="External-Storage-Account-Key"/>
        </appSettings>


##<a name="copy-blobs-from-a-storage-account-into-a-media-services-asset"></a>Αντιγραφή αντικειμένων blob από ένα λογαριασμό χώρου αποθήκευσης σε μια περιουσιακών στοιχείων υπηρεσίες πολυμέσων

Το παρακάτω παράδειγμα κώδικα εκτελεί τις ακόλουθες εργασίες:

1. Δημιουργεί την παρουσία CloudMediaContext. 
1. Δημιουργεί παρουσίες CloudStorageAccount: _sourceStorageAccount και _destinationStorageAccount.
1. Κάνει αποστολή των αρχείων ομαλή ροή δεδομένων από έναν τοπικό κατάλογο σε ένα κοντέινερ αντικειμένων blob που βρίσκεται στο _sourceStorageAccount. 
1. Δημιουργεί ένα νέο πάγιο. Το κοντέινερ αντικειμένων blob που που δημιουργείται για το συγκεκριμένο πάγιο βρίσκεται στο _destinationStorageAccount. 
1. Χρησιμοποιεί Azure SDK χώρου αποθήκευσης για να αντιγράψετε το καθορισμένο αντικείμενα BLOB σε κοντέινερ που σχετίζεται με το περιουσιακών στοιχείων.

    >[AZURE.NOTE]Η λειτουργία αντιγραφής δεν εμφανίσουν εξαίρεση εάν το εργαλείο εντοπισμού έχει λήξει.

1. Δεδομένου ότι, σε αυτό το παράδειγμα αντιγραφή ομαλή ροή αρχείων, το παράδειγμα εμφανίζει τον τρόπο ρύθμισης του αρχείου .ism να είναι το κύριο αρχείο. Εάν, για παράδειγμα, αντιγράφονται αρχείου .mp4, το αρχείο mp4 πρέπει να ρυθμιστεί για να το αρχικό αρχείο.
1. Δημιουργεί ομαλή ροή διεύθυνσης URL για το προσδιοριστικό OnDemandOrigin που σχετίζεται με τον πόρο. 
            
        class Program
        {
            // Read values from the App.config file. 
            static string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
            static string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
            static string _storageAccountName = ConfigurationManager.AppSettings["MediaServicesStorageAccountName"];
            static string _storageAccountKey = ConfigurationManager.AppSettings["MediaServicesStorageAccountKey"];
            static string _externalStorageAccountName = ConfigurationManager.AppSettings["ExternalStorageAccountName"];
            static string _externalStorageAccountKey = ConfigurationManager.AppSettings["ExternalStorageAccountKey"];

            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;

            private static CloudStorageAccount _sourceStorageAccount = null;
            private static CloudStorageAccount _destinationStorageAccount = null;

            static void Main(string[] args)
            {
            _cachedCredentials = new MediaServicesCredentials(
                    _accountName,
                    _accountKey);
            // Use the cached credentials to create CloudMediaContext.
            _context = new CloudMediaContext(_cachedCredentials);

            // In this example the storage account from which we copy blobs is not 
            // associated with the Media Services account into which we copy blobs.
            // But the same code will work for coping blobs from a storage account that is 
            // associated with the Media Services account.
            //
            // Get a reference to a storage account that is not associated with a Media Services account
            // (an external account).  
            StorageCredentials externalStorageCredentials =
                new StorageCredentials(_externalStorageAccountName, _externalStorageAccountKey);
            _sourceStorageAccount = new CloudStorageAccount(externalStorageCredentials, true);

            //Get a reference to the storage account that is associated with a Media Services account. 
            StorageCredentials mediaServicesStorageCredentials =
                new StorageCredentials(_storageAccountName, _storageAccountKey);
            _destinationStorageAccount = new CloudStorageAccount(mediaServicesStorageCredentials, false);

            // Upload Smooth Streaming files into a storage account.
            string localMediaDir = @"C:\supportFiles\streamingfiles";
            CloudBlobContainer blobContainer =
                UploadContentToStorageAccount(localMediaDir);

            // Create a new asset and copy the smooth streaming files into 
            // the container that is associated with the asset.
            IAsset asset = CreateAssetFromExistingBlobs(blobContainer);

            // Get the streaming URL for the smooth streaming files 
            // that were copied into the asset.   
            string urlForClientStreaming = CreateStreamingLocator(asset);
            Console.WriteLine("Smooth Streaming URL: " + urlForClientStreaming);

            Console.ReadLine();
            }

            /// <summary>
            /// Uploads content from a local directory into the specified storage account.
            /// In this example the storage account is not associated with the Media Services account.
            /// </summary>
            /// <param name="localPath">The path from which to upload the files.</param>
            /// <returns>The container that contains the uploaded files.</returns>
            static public CloudBlobContainer UploadContentToStorageAccount(string localPath)
            {
            CloudBlobClient externalCloudBlobClient = _sourceStorageAccount.CreateCloudBlobClient();

            CloudBlobContainer externalMediaBlobContainer = externalCloudBlobClient.GetContainerReference("streamingfiles");

            externalMediaBlobContainer.CreateIfNotExists();

            // Upload files to the blob container.  
            DirectoryInfo uploadDirectory = new DirectoryInfo(localPath);
            foreach (var file in uploadDirectory.EnumerateFiles())
            {
                CloudBlockBlob blob = externalMediaBlobContainer.GetBlockBlobReference(file.Name);

                blob.UploadFromFile(file.FullName, FileMode.Open);
            }

            return externalMediaBlobContainer;
            }

            /// <summary>
            /// Creates a new asset and copies blobs from the specifed storage account.
            /// </summary>
            /// <param name="mediaBlobContainer">The specified blob container.</param>
            /// <returns>The new asset.</returns>
            static public IAsset CreateAssetFromExistingBlobs(CloudBlobContainer mediaBlobContainer)
            {
            // Create a new asset. 
            IAsset asset = _context.Assets.Create("NewAsset_" + Guid.NewGuid(), AssetCreationOptions.None);

            IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
                TimeSpan.FromHours(24), AccessPermissions.Write);
            ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);

            CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

            // Get the asset container URI and Blob copy from mediaContainer to assetContainer. 
            string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];

            CloudBlobContainer assetContainer =
                destBlobStorage.GetContainerReference(destinationContainerName);

            if (assetContainer.CreateIfNotExists())
            {
                assetContainer.SetPermissions(new BlobContainerPermissions
                {
                PublicAccess = BlobContainerPublicAccessType.Blob
                });
            }

            var blobList = mediaBlobContainer.ListBlobs();
            foreach (var sourceBlob in blobList)
            {
                var assetFile = asset.AssetFiles.Create((sourceBlob as ICloudBlob).Name);
                CopyBlob(sourceBlob as ICloudBlob, assetContainer);
                assetFile.ContentFileSize = (sourceBlob as ICloudBlob).Properties.Length;
                assetFile.Update();
            }

            asset.Update();

            destinationLocator.Delete();
            writePolicy.Delete();

            // Since we copied a set of Smooth Streaming files, 
            // set the .ism file to be the primary file. 
            // If we, for example, copied an .mp4, then the mp4 would be the primary file. 
            SetISMFileAsPrimary(asset);

            return asset;
            }

            /// <summary>
            /// Creates the OnDemandOrigin locator in order to get the streaming URL.
            /// </summary>
            /// <param name="asset">The asset that contains the smooth streaming files.</param>
            /// <returns>The streaming URL.</returns>
            static public string CreateStreamingLocator(IAsset asset)
            {
            var ismAssetFile = asset.AssetFiles.ToList().
                Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).First();

            // Create a 30-day readonly access policy. 
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                TimeSpan.FromDays(30),
                AccessPermissions.Read);

            // Create a locator to the streaming content on an origin. 
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                policy,
                DateTime.UtcNow.AddMinutes(-5));

            return originLocator.Path + ismAssetFile.Name + "/manifest";
            }

            /// <summary>
            /// Copies the specified blob into the specified container.
            /// </summary>
            /// <param name="sourceBlob">The source container.</param>
            /// <param name="destinationContainer">The destination container.</param>
            static private void CopyBlob(ICloudBlob sourceBlob, CloudBlobContainer destinationContainer)
            {
            var signature = sourceBlob.GetSharedAccessSignature(new SharedAccessBlobPolicy
            {
                Permissions = SharedAccessBlobPermissions.Read,
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
            });

            ICloudBlob destinationBlob = destinationContainer.GetBlockBlobReference(sourceBlob.Name);

            if (destinationBlob.Exists())
            {
                Console.WriteLine(string.Format("Destination blob '{0}' already exists. Skipping.", destinationBlob.Uri));
            }
            else
            {

                // Display the size of the source blob.
                Console.WriteLine(sourceBlob.Properties.Length);

                Console.WriteLine(string.Format("Copy blob '{0}' to '{1}'", sourceBlob.Uri, destinationBlob.Uri));
                destinationBlob.StartCopyFromBlob(new Uri(sourceBlob.Uri.AbsoluteUri + signature));

                while (true)
                {
                // The StartCopyFromBlob is an async operation, 
                // so we want to check if the copy operation is completed before proceeding. 
                // To do that, we call FetchAttributes on the blob and check the CopyStatus. 
                destinationBlob.FetchAttributes();
                if (destinationBlob.CopyState.Status != CopyStatus.Pending)
                {
                    break;
                }
                //It's still not completed. So wait for some time.
                System.Threading.Thread.Sleep(1000);
                }


                // Display the size of the destination blob.
                Console.WriteLine(destinationBlob.Properties.Length);

            }
            }

            /// <summary>
            /// Sets a file with the .ism extension as a primary file.
            /// </summary>
            /// <param name="asset">The asset that contains the smooth streaming files.</param>
            static private void SetISMFileAsPrimary(IAsset asset)
            {

            //If you expect the asset to contain the .ism asset file, set the .ism file as the primary file.
            var ismAssetFiles = asset.AssetFiles.ToList().
                Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray();

            // The following code assigns the first .ism file as the primary file in the asset.
            // An asset should have one .ism file.  
            ismAssetFiles.First().IsPrimary = true;
            ismAssetFiles.First().Update();
            }
        }

 

##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
