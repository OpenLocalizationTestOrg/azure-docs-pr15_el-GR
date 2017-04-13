<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob του Azure από iOS | Microsoft Azure"
    description="Αποθήκευση μη δομημένα δεδομένα στο cloud με το χώρο αποθήκευσης αντικειμένων Blob του Azure (χώρος αποθήκευσης αντικειμένων)."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-ios"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από iOS

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο θα σας δείξουν πώς να εκτελείτε συνηθισμένα σενάρια χρησιμοποιώντας χώρο αποθήκευσης αντικειμένων Blob Microsoft Azure. Τα δείγματα είναι γραμμένες σε στόχο-C και να χρησιμοποιήσετε τη [Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης για iOS](https://github.com/Azure/azure-storage-ios). Τα σενάρια καλύπτεται περιλαμβάνουν **Αποστολή**, **καταχώρηση**, **τη λήψη**και **τη διαγραφή** αντικειμένων blob. Για περισσότερες πληροφορίες σχετικά με αντικείμενα blob, ανατρέξτε στην ενότητα [Επόμενα βήματα](#next-steps) . Μπορείτε επίσης να κάνετε λήψη της [εφαρμογής δείγμα](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) για να δείτε γρήγορα τη χρήση του χώρου αποθήκευσης Azure σε μια εφαρμογή του iOS.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Εισαγωγή στη βιβλιοθήκη iOS αποθήκευσης Azure στην εφαρμογή σας

Μπορείτε να εισαγάγετε τη βιβλιοθήκη iOS αποθήκευσης Azure στην εφαρμογή σας χρησιμοποιώντας το [Azure αποθήκευσης CocoaPod](https://cocoapods.org/pods/AZSClient) ή με την εισαγωγή του αρχείου **πλαισίου** .

## <a name="cocoapod"></a>CocoaPod

1. Εάν δεν το έχετε κάνει ήδη, [Εγκαταστήστε CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) στον υπολογιστή σας ανοίγοντας ένα παράθυρο τερματικού και εκτελέστε την παρακάτω εντολή

        sudo gem install cocoapods

2. Επόμενο, στον κατάλογο του έργου (τον κατάλογο που περιέχει το `.xcodeproj` αρχείου), δημιουργήστε ένα νέο αρχείο που ονομάζεται `Podfile`(χωρίς επέκταση αρχείου). Προσθέστε τα εξής για να `Podfile` και αποθήκευση

        pod 'AZSClient'

3. Στο παράθυρο τερματικού, μεταβείτε στον κατάλογο έργου και εκτελέστε την ακόλουθη εντολή

        pod install

4. Εάν σας `.xcodeproj` είναι ανοιχτό σε Xcode, κλείστε το. Στον κατάλογο του έργου σας, ανοίξτε το αρχείο που μόλις δημιουργήθηκε έργου που θα έχουν τα `.xcworkspace` επέκταση. Αυτό είναι το αρχείο που θα εκτελεστεί από για τώρα στην.

## <a name="framework"></a>Πλαίσιο
Για να χρησιμοποιήσετε τη βιβλιοθήκη iOS αποθήκευσης Azure, θα πρέπει πρώτα να δημιουργήσετε το αρχείο framework.

1. Πρώτα, κάντε λήψη ή κλωνοποίηση το [repo azure-χώρος αποθήκευσης-ios](https://github.com/azure/azure-storage-ios).

2. Μεταβείτε στο *azure-χώρος αποθήκευσης-ios* -> *βιβλιοθήκης* -> *Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης*και άνοιγμα `AZSClient.xcodeproj` στο Xcode.

3. Στο επάνω αριστερό τμήμα του Xcode, αλλαγή του συνδυασμού ενεργό από "Βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη" σε "Πλαίσιο".

4. Δημιουργήστε το έργο (⌘ + B). Αυτό θα δημιουργήσει μια `AZSClient.framework` αρχείο στον υπολογιστή σας.

Στη συνέχεια, μπορείτε να εισαγάγετε το αρχείο framework στην εφαρμογή σας, κάνοντας τα εξής:

1. Δημιουργήστε ένα νέο έργο ή ανοίξτε το υπάρχον έργο στο Xcode.

2. Κάντε κλικ στο έργο σας, στο αριστερό παράθυρο περιήγησης και κάντε κλικ στην καρτέλα " *Γενικά* " στο επάνω μέρος του προγράμματος επεξεργασίας του έργου.

3. Στην περιοχή *συνδεδεμένα πλαίσια και τις βιβλιοθήκες* , κάντε κλικ στο κουμπί Προσθήκη (+).

4. Κάντε κλικ στο κουμπί *Προσθήκη άλλου...*. Για να περιηγηθείτε και να προσθέσετε το `AZSClient.framework` αρχείο που μόλις δημιουργήσατε.

5. Στην περιοχή *συνδεδεμένα πλαίσια και τις βιβλιοθήκες* , κάντε ξανά κλικ στο κουμπί Προσθήκη (+).

6. Στη λίστα των βιβλιοθηκών που παρέχεται ήδη, κάντε αναζήτηση για `libxml2.2.dylib` και να την προσθέσετε στο έργο σας.

7. Κάντε κλικ στην καρτέλα *Δημιουργία ρυθμίσεις* στο επάνω μέρος του προγράμματος επεξεργασίας του έργου.

8. Στην περιοχή *Διαδρομές αναζήτησης* , κάντε διπλό κλικ στο *Πλαίσιο αναζήτησης διαδρομές* και να προσθέσετε τη διαδρομή προς το `AZSClient.framework` αρχείου.

## <a name="import-statement"></a>Δήλωση εισαγωγής
Θα πρέπει να συμπεριλάβετε την ακόλουθη πρόταση εισαγωγής στο σημείο όπου θέλετε να καλέσετε το API αποθήκευσης Azure αρχείο.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Ασύγχρονων λειτουργιών
> [AZURE.NOTE] Όλες οι μέθοδοι που εκτελούν μια αίτηση σε σχέση με την υπηρεσία είναι ασύγχρονων λειτουργιών. Στα δείγματα κώδικα, θα βρείτε ότι αυτές οι μέθοδοι έχουν ένα πρόγραμμα χειρισμού ολοκλήρωσης. Κώδικας μέσα τη λαβή συμπλήρωσης θα εκτελεστεί **μετά την** ολοκλήρωση της αίτησης. Πραγματοποιείται κώδικας μετά τη λαβή συμπλήρωσης θα εκτελείται **κατά** την αίτηση.

## <a name="create-a-container"></a>Δημιουργία κοντέινερ
Κάθε blob στο χώρο αποθήκευσης Azure πρέπει να βρίσκονται σε ένα κοντέινερ. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε ένα κοντέινερ, που ονομάζεται *newcontainer*, στο λογαριασμό σας χώρο αποθήκευσης, εάν δεν υπάρχει ήδη. Όταν επιλέγετε ένα όνομα για το κοντέινερ, να Επιθυμώντας τους κανόνες ονοματοθεσίας που αναφέρονται παραπάνω.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

Μπορείτε να επιβεβαιώσετε ότι λειτουργεί, εξετάζοντας την [Εξερεύνηση χώρου αποθήκευσης του Microsoft Azure](http://storageexplorer.com) και επαλήθευση που *newcontainer* είναι στη λίστα των κοντέινερ για το λογαριασμό χώρου αποθήκευσης.

## <a name="set-container-permissions"></a>Ορισμός δικαιωμάτων κοντέινερ
Δικαιώματα ενός κοντέινερ έχουν ρυθμιστεί για **ιδιωτική** πρόσβαση από προεπιλογή. Ωστόσο, κοντέινερ παρέχουν ορισμένες διαφορετικές επιλογές για πρόσβαση κοντέινερ:

- **Ιδιωτικό**: κοντέινερ και αντικειμένων blob δεδομένων μπορεί να διαβαστεί από τον κάτοχο της μόνο λογαριασμό.

- **BLOB**: Blob δεδομένων μέσα σε αυτό το κοντέινερ μπορεί να διαβαστεί μέσω ανώνυμη αίτηση, αλλά κοντέινερ δεδομένων δεν είναι διαθέσιμη. Προγράμματα-πελάτες δεν είναι δυνατό να απαρίθμηση αντικείμενα BLOB μέσα στο κοντέινερ μέσω ανώνυμη αίτηση.

- **Κοντέινερ**: κοντέινερ και αντικειμένων blob δεδομένων μπορεί να διαβαστεί μέσω ανώνυμη αίτηση. Προγράμματα-πελάτες να απαριθμήσετε αντικείμενα BLOB μέσα στο κοντέινερ μέσω ανώνυμη αίτηση, αλλά δεν είναι δυνατή η απαρίθμηση κοντέινερ εντός του λογαριασμού χώρου αποθήκευσης.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε ένα κοντέινερ με τα δικαιώματα πρόσβασης **κοντέινερ** που θα επιτρέψετε την πρόσβαση δημόσια, μόνο για ανάγνωση για όλους τους χρήστες στο Internet:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## <a name="upload-a-blob-into-a-container"></a>Αποστείλετε ένα blob σε ένα κοντέινερ
Όπως αναφέρθηκε στην ενότητα [έννοιες εξυπηρέτησης Blob](#blob-service-concepts) , χώρος αποθήκευσης αντικειμένων Blob προσφέρει τρεις διαφορετικοί τύποι αντικειμένων blob: Αποκλεισμός αντικείμενα blob, να προσαρτήσετε αντικείμενα BLOB και σελίδα αντικείμενα blob. Αυτήν τη στιγμή, η βιβλιοθήκη iOS αποθήκευσης Azure υποστηρίζει μόνο αντικείμενα BLOB μπλοκ. Στις περισσότερες περιπτώσεις, blob μπλοκ είναι ο τύπος συνιστάται να χρησιμοποιήσετε.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να αποστείλετε ένα μπλοκ blob από μια NSString. Εάν υπάρχει ήδη ένα blob με το ίδιο όνομα σε αυτό το κοντέινερ, θα αντικατασταθούν τα περιεχόμενα του αυτό blob.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

Μπορείτε να επιβεβαιώσετε ότι λειτουργεί, εξετάζοντας την [Εξερεύνηση χώρου αποθήκευσης του Microsoft Azure](http://storageexplorer.com) και επαλήθευση ότι το κοντέινερ, *containerpublic*, περιέχει το αντικείμενο blob, *sampleblob*. Σε αυτό το δείγμα, χρησιμοποιήσαμε ένα κοντέινερ δημόσια, ώστε να μπορείτε επίσης να επαληθεύσετε ότι αυτή ολοκληρώθηκε με επιτυχία, μεταβαίνοντας τα αντικείμενα BLOB URI:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Εκτός από την αποστολή ενός μπλοκ blob από μια NSString, παρόμοιες μεθόδους υπάρχουν για NSData, NSInputStream ή ένα τοπικό αρχείο.

## <a name="list-the-blobs-in-a-container"></a>Λίστα με τα αντικείμενα BLOB σε ένα κοντέινερ
Το παρακάτω παράδειγμα εμφανίζει τον τρόπο για μια λίστα όλων των BLOB σε ένα κοντέινερ. Κατά την εκτέλεση αυτής της λειτουργίας, να Επιθυμώντας από τις ακόλουθες παραμέτρους:     

- **continuationToken** - το διακριτικό συνέχισης αντιπροσωπεύει όπου θα πρέπει να ξεκινήσετε τη λειτουργία καταχώρηση. Εάν δεν υπάρχει το διακριτικό παρέχεται, αυτό θα εμφανιστούν αντικείμενα BLOB από την αρχή. Οποιονδήποτε αριθμό αντικείμενα BLOB μπορεί να αναφέρεται, από το μηδέν έως και ένα μέγιστο σύνολο. Ακόμα και αν αυτή η μέθοδος επιστρέφει μηδέν αποτελέσματα, εάν `results.continuationToken` δεν είναι nil, μπορεί να υπάρχουν περισσότερα αντικείμενα BLOB στην υπηρεσία που δεν έχουν καταχωρηθεί στη λίστα.
- **πρόθεμα** - μπορείτε να καθορίσετε το πρόθεμα για χρήση για τη λίστα αντικειμένων blob. Θα εμφανίζονται μόνο τα αντικείμενα BLOB που ξεκινούν με αυτό το πρόθεμα.
- **useFlatBlobListing** - όπως που αναφέρονται στην ενότητα [ονομάτων και αναφορά σε κοντέινερ και αντικείμενα BLOB](#naming-and-referencing-containers-and-blobs) , παρόλο που η υπηρεσία Blob είναι ένας συνδυασμός επίπεδο χώρου αποθήκευσης, μπορείτε να δημιουργήσετε μια ιεραρχία εικονικού δίνοντάς αντικείμενα blob με πληροφορίες διαδρομής. Ωστόσο, μη επιπέδων καταχώρηση αυτήν τη στιγμή δεν υποστηρίζεται. Αυτό σύντομα διαθέσιμο. Προς το παρόν, αυτή η τιμή πρέπει να είναι`YES`
- **blobListingDetails** - μπορείτε να καθορίσετε ποια στοιχεία θέλετε να συμπεριλάβετε κατά την καταχώρηση αντικείμενα BLOB
    - `AZSBlobListingDetailsNone`: Λίστας μόνο δεσμευμένη αντικείμενα BLOB και δεν επιστρέφουν blob μετα-δεδομένων.
    - `AZSBlobListingDetailsSnapshots`: Λίστα δεσμευμένου αντικείμενα BLOB και στιγμιότυπα blob.
    - `AZSBlobListingDetailsMetadata`: Ανάκτηση blob μετα-δεδομένα για κάθε blob επιστρέφονται στην καταχώρηση της.
    - `AZSBlobListingDetailsUncommittedBlobs`: Λίστα δεσμευμένο και μη ολοκληρωμένη αντικείμενα blob.
    - `AZSBlobListingDetailsCopy`: Περιλαμβάνουν αντιγραφή ιδιότητες στην καταχώρηση της.
    - `AZSBlobListingDetailsAll`: Λίστα όλες οι διαθέσιμες δεσμευμένη αντικείμενα blob, μη ολοκληρωμένη αντικείμενα BLOB και στιγμιότυπα και επιστροφής όλα μετα-δεδομένων και αντιγραφή κατάστασης για αυτά τα αντικείμενα blob.
- **maxResults** - ο μέγιστος αριθμός των αποτελεσμάτων για να επιστρέψετε για αυτήν τη λειτουργία. Χρησιμοποιήστε -1 για να ορίσετε ένα όριο.
- **completionHandler** - το τμήμα κώδικα για την εκτέλεση με τα αποτελέσματα της λειτουργίας καταχώρηση.

Σε αυτό το παράδειγμα, χρησιμοποιείται μια μέθοδο Βοήθειας για κλήση σταδιακά τη λίστα αντικείμενα BLOB μέθοδο κάθε φορά που επιστρέφεται η τιμή ενός διακριτικού συνέχισης.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## <a name="download-a-blob"></a>Λήψη ένα blob

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να κάνετε λήψη μιας blob σε ένα αντικείμενο NSString.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## <a name="delete-a-blob"></a>Διαγράψτε ένα blob

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να διαγράψετε ένα blob.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## <a name="delete-a-blob-container"></a>Διαγραφή ενός κοντέινερ αντικειμένων blob

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να διαγράψετε ένα κοντέινερ.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από iOS, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα σχετικά με τη βιβλιοθήκη iOS και την υπηρεσία αποθήκευσης.

- [Azure βιβλιοθήκη προγράμματος-πελάτη χώρου αποθήκευσης για iOS](https://github.com/azure/azure-storage-ios)
- [Azure αποθήκευσης iOS τεκμηρίωση αναφοράς](http://azure.github.io/azure-storage-ios/)
- [Υπηρεσίες Azure αποθήκευσης REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης](http://blogs.msdn.com/b/windowsazurestorage)

Εάν έχετε ερωτήσεις σχετικά με αυτήν τη βιβλιοθήκη, μπορείτε να δημοσιεύσετε για να μας [φόρουμ MSDN Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) ή [Υπερχείλισης στοίβας](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Εάν έχετε τη δυνατότητα προτάσεις για το χώρο αποθήκευσης Azure, καταχωρήστε στα [Σχόλιά Azure χώρου αποθήκευσης](https://feedback.azure.com/forums/217298-storage/).
