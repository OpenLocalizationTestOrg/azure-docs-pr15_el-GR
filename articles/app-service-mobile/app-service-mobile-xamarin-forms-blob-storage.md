<properties
    pageTitle="Σύνδεση στο χώρο αποθήκευσης Azure στην εφαρμογή Xamarin.Forms"
    description="Προσθήκη εικόνων για την εφαρμογή για κινητές συσκευές todo λίστα Xamarin.Forms κατά τη σύνδεση με το χώρο αποθήκευσης αντικειμένων blob του Azure"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>Σύνδεση στο χώρο αποθήκευσης Azure στην εφαρμογή Xamarin.Forms

## <a name="overview"></a>Επισκόπηση

Εφαρμογές του Mobile Azure πελάτη και διακομιστή SDK υποστήριξη συγχρονισμού χωρίς σύνδεση δομημένων δεδομένων με τις λειτουργίες CRUD σε σχέση με το τελικό σημείο /tables. Γενικά αυτά τα δεδομένα αποθηκεύονται σε μια βάση δεδομένων ή παρόμοια store και, γενικά αυτές οι χώροι αποθήκευσης δεδομένων δεν μπορούν να αποθηκευτούν μεγάλα δυαδικά δεδομένα αποτελεσματικά. Επίσης, ορισμένες εφαρμογές έχουν που σχετίζονται με δεδομένα που είναι αποθηκευμένα σε άλλη θέση (π.χ., χώρος αποθήκευσης αντικειμένων blob, κοινόχρηστα στοιχεία αρχείων) και, είναι χρήσιμο να έχετε τη δυνατότητα να δημιουργείτε συσχετίσεις μεταξύ εγγραφών με το τελικό σημείο /tables και άλλων δεδομένων.

Αυτό το θέμα δείχνει πώς μπορείτε να προσθέσετε υποστήριξη για εικόνες για τη γρήγορη έναρξη εφαρμογές του Mobile todo λίστα. Πρέπει να ολοκληρώσετε πρώτα το πρόγραμμα εκμάθησης, [Δημιουργία εφαρμογής Xamarin.Forms].

Σε αυτό το πρόγραμμα εκμάθησης, θα δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης και προσθέστε μια συμβολοσειρά σύνδεσης για την εφαρμογή Mobile παρασκηνίου. Στη συνέχεια, μπορείτε να προσθέσετε μια νέα μεταβίβαση από τον νέο τύπο εφαρμογές του Mobile `StorageController<T>` για τον project server.

>[AZURE.TIP] Αυτό το πρόγραμμα εκμάθησης περιλαμβάνει ένα [δείγμα συνοδευτικό](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/) διαθέσιμη, που μπορούν να αναπτυχθούν στο δικό σας λογαριασμό Azure. 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Ολοκληρώστε το πρόγραμμα εκμάθησης [Δημιουργία μιας εφαρμογής Xamarin.Forms] , το οποίο παραθέτει άλλες προϋποθέσεις. Σε αυτό το άρθρο χρησιμοποιεί την ολοκληρωμένη εφαρμογή από αυτό το πρόγραμμα εκμάθησης.

>[AZURE.NOTE] Εάν θέλετε να γρήγορα αποτελέσματα με το Azure εφαρμογής υπηρεσίας πριν εγγραφείτε για ένα λογαριασμό Azure, μεταβείτε στο [Δοκιμάστε εφαρμογής υπηρεσίας](https://tryappservice.azure.com/?appServiceName=mobile). Εκεί, μπορείτε να αμέσως δημιουργήσετε μια εφαρμογή για κινητές συσκευές μικρής διάρκειας starter στην εφαρμογή υπηρεσίας — απαιτείται πιστωτική κάρτα και χωρίς δεσμεύσεις.

## <a name="create-a-storage-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης

1. Δημιουργία λογαριασμού χώρου αποθήκευσης, ακολουθώντας το πρόγραμμα εκμάθησης, [Δημιουργήστε ένα λογαριασμό Azure χώρου αποθήκευσης]. 

2. Στην πύλη του Azure, μεταβείτε στο λογαριασμό σας που έχουν δημιουργηθεί πρόσφατα χώρου αποθήκευσης και κάντε κλικ στο εικονίδιο **πλήκτρα** . Αντιγράψτε τη **συμβολοσειρά σύνδεσης πρωτεύοντος**.

3. Μεταβείτε σας παρασκηνίου εφαρμογής για κινητές συσκευές. Σε **Όλες τις ρυθμίσεις** -> **Ρυθμίσεις εφαρμογής** -> **Συμβολοσειρές σύνδεσης**, δημιουργήστε ένα νέο κλειδί με το όνομα `MS_AzureStorageAccountConnectionString` και χρησιμοποιήστε την τιμή που αντιγράψατε από το λογαριασμό χώρου αποθήκευσης. Χρήση **προσαρμοσμένης** ως ο τύπος κλειδιού.

## <a name="add-a-storage-controller-to-the-server"></a>Προσθέστε έναν ελεγκτή αποθήκευσης στο διακομιστή

Πρέπει να προσθέσετε ένα νέο ελεγκτή στο έργο σας διακομιστή που θα ανταποκριθεί στις αιτήσεις για ενός διακριτικού συσχετισμών Ασφαλείας για το χώρο αποθήκευσης Azure, καθώς και επιστρέφει μια λίστα των αρχείων που αντιστοιχούν σε μια εγγραφή:

- [Προσθέστε έναν ελεγκτή αποθήκευσης στο έργο σας διακομιστή](#add-controller-code)
- [Δρομολογεί καταχωρηθεί από τον ελεγκτή χώρου αποθήκευσης](#routes-registered)
- [Πρόγραμμα-πελάτη και διακομιστή επικοινωνίας](#client-communication)

###<a name="add-controller-code"></a>Προσθέστε έναν ελεγκτή αποθήκευσης στο έργο σας διακομιστή

1. Στο Visual Studio, ανοίξτε το έργο σας .NET server. Προσθέστε το πακέτο Nuget [Microsoft.Azure.Mobile.Server.Files]. Φροντίστε να επιλέξετε **περιλαμβάνουν προέκδοση**.

2. Στο Visual Studio, ανοίξτε το έργο σας .NET server. Κάντε δεξί κλικ στο φάκελο **ελεγκτές** και επιλέξτε **Προσθήκη** -> **ελεγκτή** -> **Ελεγκτή 2 API Web - είναι κενή**. Δώστε ένα όνομα στον ελεγκτή `TodoItemStorageController`.

3. Προσθέστε τα ακόλουθα χρήση προτάσεων:

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. Αλλαγή της βασικής κλάσης να `StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. Προσθέστε τις ακόλουθες μεθόδους για να την κλάση:

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. Ενημερώστε τις ρυθμίσεις παραμέτρων Web API για να ρυθμίσετε τη δρομολόγηση χαρακτηριστικό. Στο **Startup.MobileApp.cs**, προσθέστε την ακόλουθη γραμμή για να το `ConfigureMobileApp()` μέθοδο, μετά από τον ορισμό των το `config` μεταβλητή:

        config.MapHttpAttributeRoutes();

7. Δημοσίευση του project server για να σας παρασκηνίου εφαρμογής για κινητές συσκευές.

###<a name="routes-registered"></a>Δρομολογεί καταχωρηθεί από τον ελεγκτή χώρου αποθήκευσης

Η νέα `TodoItemStorageController` εκθέτει δύο δευτερεύουσες πόρων σύμφωνα με την εγγραφή που διαχειρίζεται:

- StorageToken

    + ΔΗΜΟΣΊΕΥΣΗ HTTP: Δημιουργεί μια διακριτικό χώρου αποθήκευσης
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP GET: Ανακτά μια λίστα των αρχείων που σχετίζονται με την εγγραφή
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + ΔΙΑΓΡΑΦΉ HTTP: Διαγράφει το αρχείο που καθορίζεται στο αρχείο αναγνωριστικό πόρου
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="client-communication"></a>Πρόγραμμα-πελάτη και διακομιστή επικοινωνίας

Λάβετε υπόψη ότι `TodoItemStorageController` κάνει *δεν* έχουν δρομολόγηση για αποστολή ή λήψη ενός blob. Αυτό συμβαίνει επειδή ένα πρόγραμμα-πελάτη κινητών αλληλεπιδρά με blob αποθήκευσης *απευθείας* για να εκτελέσετε αυτές τις εργασίες, μετά την πρώτη ενός διακριτικού συσχετισμών Ασφαλείας (κοινόχρηστα υπογραφή πρόσβασης) για ασφαλή πρόσβαση σε συγκεκριμένη blob ή κοντέινερ. Αυτό είναι ένα σημαντικό αρχιτεκτονική σχεδίαση, όπως διαφορετικά πρόσβαση στο χώρο αποθήκευσης θα περιορίζεται από την κλιμάκωση και τη διαθεσιμότητα των φορητό υπολογιστή στο παρασκήνιο. Αντί για αυτό, με τη σύνδεση απευθείας στο χώρο αποθήκευσης Azure, το πρόγραμμα-πελάτη κινητών να επωφεληθείτε από τις δυνατότητες όπως η αυτόματη διαμερισμάτων και παν κατανομής.

Μια υπογραφή κοινόχρηστη πρόσβαση παρέχει πρόσβαση ανάθεση στους πόρους στο λογαριασμό σας στο χώρο αποθήκευσης. Αυτό σημαίνει ότι μπορείτε να εκχωρήσετε ένα πρόγραμμα-πελάτη περιορισμένα δικαιώματα σε αντικείμενα στο λογαριασμό σας στο χώρο αποθήκευσης για ένα καθορισμένο χρονικό διάστημα και με ένα συγκεκριμένο σύνολο δικαιωμάτων, χωρίς να χρειάζεται να κάνετε κοινή χρήση τα πλήκτρα πρόσβασης του λογαριασμού. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Κατανόηση των κοινόχρηστων Access υπογραφές].

Το παρακάτω διάγραμμα παρουσιάζει τις αλληλεπιδράσεις υπολογιστή-πελάτη και διακομιστή. Πριν από την αποστολή ενός αρχείου, ο υπολογιστής-πελάτης ζητά ενός διακριτικού συσχετισμών Ασφαλείας από την υπηρεσία. Η υπηρεσία χρησιμοποιεί τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης για να δημιουργήσετε μια νέα συσχετισμών Ασφαλείας που, στη συνέχεια, επιστρέφει στον υπολογιστή-πελάτη. Τις συσχετίσεις Ασφαλείας είναι περιορισμένης και περιορίζει δικαιώματα μόνο σε συγκεκριμένο αρχείο ή κοντέινερ. Το πρόγραμμα-πελάτη κινητών χρησιμοποιεί, στη συνέχεια, αυτό συσχετισμών Ασφαλείας και ο υπολογιστής-πελάτης αποθήκευσης Azure SDK για να αποστείλετε το αρχείο στο χώρο αποθήκευσης αντικειμένων blob.

![Αίτηση ενός διακριτικού συσχετισμών Ασφαλείας](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>Ενημερώστε την εφαρμογή υπολογιστή-πελάτη σας για να προσθέσετε υποστήριξη εικόνων

Ανοίξτε το έργο γρήγορη έναρξη Xamarin.Forms στο Visual Studio ή Xamarin Studio. Θα εγκαταστήσετε Nuget πακέτων και ενημερώστε το έργο portable βιβλιοθήκης και του iOS, Android και Windows έργα προγράμματος-πελάτη:

- [Προσθήκη πακέτων Nuget](#add-nuget)
- [Προσθήκη IPlatform περιβάλλοντος εργασίας](#add-iplatform)
- [Προσθήκη FileHelper τάξης](#add-filehelper)
- [Προσθέστε ένα πρόγραμμα χειρισμού Συγχρονισμός αρχείων](#file-sync-handler)
- [Ενημέρωση TodoItemManager](#update-todoitemmanager)
- [Προσθέστε μια προβολή λεπτομερειών](#add-details-view)
- [Ενημερώστε την κύρια προβολή](#update-main-view)
- [Ενημέρωση του Android έργου](#update-android), [iOS έργου](#update-ios), [Windows έργου](#update-windows)

>[AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης περιέχει μόνο οδηγίες για την Android, iOS και πλατφόρμες Windows Store, δεν Windows Phone.

###<a name="add-nuget"></a>Προσθήκη πακέτων Nuget

Κάντε δεξί κλικ της λύσης και επιλέξτε **Διαχείριση Nuget πακέτα για τη λύση**. Προσθέστε τα ακόλουθα πακέτα Nuget σε **όλα** τα έργα από τη λύση. Φροντίστε να ελέγξετε **περιλαμβάνουν προέκδοση**.

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

Για τη διευκόλυνσή σας, αυτό το δείγμα χρησιμοποιεί τη βιβλιοθήκη [PCLStorage] , αλλά δεν απαιτείται από το πρόγραμμα-πελάτη εφαρμογές του Mobile Azure SDK.

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="add-iplatform"></a>Προσθήκη IPlatform περιβάλλοντος εργασίας

Δημιουργήστε ένα νέο περιβάλλον `IPlatform` του έργου κύριο portable βιβλιοθήκη. Αυτό ακολουθεί το μοτίβο [Xamarin.Forms DependencyService] για να φορτώσετε την κλάση συγκεκριμένης πλατφόρμας προς τα δεξιά κατά το χρόνο εκτέλεσης. Θα προσθέσετε αργότερα υλοποιήσεις συγκεκριμένης πλατφόρμας σε κάθε ένα από τα έργα προγράμματος-πελάτη.

1. Προσθέστε τα ακόλουθα χρήση προτάσεων:

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. Αντικαταστήστε την εφαρμογή με τα εξής:

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="add-filehelper"></a>Προσθήκη FileHelper τάξης

1. Δημιουργία μιας νέας κλάσης `FileHelper` του έργου κύριο portable βιβλιοθήκη. Προσθέστε τα ακόλουθα χρήση προτάσεων:

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. Προσθέστε τον ορισμό κλάσης:

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="file-sync-handler"></a>Προσθέστε ένα πρόγραμμα χειρισμού Συγχρονισμός αρχείων

Δημιουργία μιας νέας κλάσης `TodoItemFileSyncHandler` του έργου κύριο portable βιβλιοθήκη. Αυτή η κλάση περιέχει επιστροφές κλήσης από το Azure SDK να ειδοποιήσετε τον κωδικό, όταν ένα αρχείο έχει προστεθεί ή καταργηθεί.

Το SDK προγράμματος-πελάτη κινητών Azure δεν αποθηκεύει πραγματικά δεδομένα αρχείου: το πρόγραμμα-πελάτης SDK καλεί υλοποίηση του `IFileSyncHandler` που με τη σειρά της καθορίζει τον τρόπο αρχεία αποθηκεύονται στην τοπική συσκευή.

1. Προσθέστε τα ακόλουθα χρήση προτάσεων:

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. Αντικαταστήστε τον ορισμό της τάξης με τα εξής: 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="update-todoitemmanager"></a>Ενημέρωση TodoItemManager

1. Στο **TodoItemManager.cs**, καταργήστε τα σχόλια από τη γραμμή `#define OFFLINE_SYNC_ENABLED`.

2. Στο **TodoItemManager.cs**, προσθέστε τα ακόλουθα χρήση προτάσεων:

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. Στην κατασκευή της `TodoItemManager`, προσθέστε τα ακόλουθα μετά την κλήση σε `DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. Στην κατασκευή, αντικαταστήστε την κλήση σε `InitializeAsync` με την εξής. Αυτό θα εξασφαλίσει ότι δεν υπάρχουν επιστροφές κλήσης όταν τροποποιούνται εγγραφές στον τοπικό χώρο αποθήκευσης. Η δυνατότητα συγχρονισμού αρχείο χρησιμοποιεί αυτές τις επιστροφές κλήσης για να ενεργοποιήσετε το πρόγραμμα χειρισμού Συγχρονισμός αρχείων.

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. Στο `SyncAsync()`, προσθέστε τα ακόλουθα μετά την κλήση σε `PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. Προσθέστε τις ακόλουθες μεθόδους για να `TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="add-details-view"></a>Προσθέστε μια προβολή λεπτομερειών

Σε αυτήν την ενότητα, θα προσθέσετε μια νέα προβολή λεπτομερειών για ένα στοιχείο todo. Όταν ο χρήστης επιλέγει ένα στοιχείο todo και επιτρέπει σε νέα εικόνες θα προστεθούν σε ένα στοιχείο, δημιουργείται στην προβολή.

1. Προσθέστε μια νέα κλάση **TodoItemImage** στο έργο portable βιβλιοθήκης με την εφαρμογή παρακάτω:

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. Επεξεργασία **App.cs**. Αντικατάσταση της προετοιμασίας των `MainPage` με τα εξής:
    
        MainPage = new NavigationPage(new TodoList());

3. Στο **App.cs**, προσθέστε την ακόλουθη ιδιότητα:

        public static object UIContext { get; set; }

4. Κάντε δεξί κλικ στο έργο portable βιβλιοθήκη και επιλέξτε **Προσθήκη** -> **Νέο στοιχείο** -> **πλατφόρμες** -> **Φόρμες Xaml σελίδας**. Δώστε όνομα στην προβολή `TodoItemDetailsView`.

5. Ανοίξτε το **TodoItemDetailsView.xaml** και αντικαταστήστε το σώμα του ContentPage με τα εξής:

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. Επεξεργασία **TodoItemDetailsView.xaml.cs** και προσθέστε το εξής χρήση προτάσεων:

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. Αντικαταστήστε την εφαρμογή της `TodoItemDetailsView` με τα εξής:

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="update-main-view"></a>Ενημερώστε την κύρια προβολή 

Ενημερώστε την κύρια προβολή για να ανοίξετε την προβολή λεπτομερειών όταν είναι επιλεγμένο ένα στοιχείο todo.

Στο **TodoList.xaml.cs**, αντικαταστήστε την εφαρμογή της `OnSelected` με τα εξής:

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="update-android"></a>Ενημέρωση του Android έργου

Προσθέστε κώδικα της συγκεκριμένης πλατφόρμας στο Android έργο, συμπεριλαμβανομένου του κώδικα για τη λήψη ενός αρχείου και τη χρήση της κάμερας για να καταγράψετε μια νέα εικόνα. 

Αυτός ο κωδικός χρησιμοποιεί το Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/) για να φορτώσετε την κλάση συγκεκριμένης πλατφόρμας προς τα δεξιά κατά το χρόνο εκτέλεσης.

1. Προσθέστε το στοιχείο **Xamarin.Mobile** στο Android έργο.

2. Προσθέστε μια νέα κλάση `DroidPlatform` με την παρακάτω εφαρμογή. Αντικατάσταση "YourNamespace" με τον κύριο χώρο ονομάτων του έργου σας.

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Επεξεργασία **MainActivity.cs**. Στο `OnCreate`, προσθέστε τα ακόλουθα πριν από την κλήση σε `LoadApplication()`:

        App.UIContext = this;

###<a name="update-ios"></a>Ενημέρωση του έργου iOS

Προσθέστε κώδικα της συγκεκριμένης πλατφόρμας στο έργο iOS.

1. Προσθέστε το στοιχείο **Xamarin.Mobile** στο έργο iOS.

2. Προσθέστε μια νέα κλάση `TouchPlatform` με την παρακάτω εφαρμογή. Αντικατάσταση "YourNamespace" με τον κύριο χώρο ονομάτων του έργου σας.

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Επεξεργασία **AppDelegate.cs** και καταργήστε τα σχόλια από την κλήση σε `SQLitePCL.CurrentPlatform.Init()`.

###<a name="update-windows"></a>Ενημέρωση του έργου των Windows

1. Εγκαταστήστε την επέκταση Visual Studio [SQLite για Windows 8.1](http://go.microsoft.com/fwlink/?LinkID=716919). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα το πρόγραμμα εκμάθησης [Ενεργοποίηση συγχρονισμού χωρίς σύνδεση για την εφαρμογή σας των Windows](app-service-mobile-windows-store-dotnet-get-started-offline-data.md). 

2. Επεξεργασία **Package.appxmanifest** και έλεγχος των δυνατοτήτων την **κάμερα Web** .

3. Προσθέστε μια νέα κλάση `WindowsStorePlatform` με την παρακάτω εφαρμογή. Αντικατάσταση "YourNamespace" με τον κύριο χώρο ονομάτων του έργου σας.

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>Σύνοψη

Σε αυτό το άρθρο περιγράφεται πώς μπορείτε να χρησιμοποιήσετε τη νέα υποστήριξη αρχείου στο κινητό Azure προγράμματος-πελάτη και διακομιστή SDK για να εργαστείτε με το χώρο αποθήκευσης Azure. 

- Δημιουργία λογαριασμού χώρου αποθήκευσης και προσθέστε τη συμβολοσειρά σύνδεσης του στοιχείου διακομιστή εφαρμογής για κινητές συσκευές. Μόνο για υπολογιστή στο παρασκήνιο περιλαμβάνει το πλήκτρο με το χώρο αποθήκευσης Azure: φορητός υπολογιστής-πελάτης ζητά ενός διακριτικού συσχετισμών Ασφαλείας (κοινόχρηστα υπογραφή Access) κάθε φορά που χρειάζονται για την πρόσβαση στο χώρο αποθήκευσης Azure. Για να μάθετε περισσότερα σχετικά με τα διακριτικά συσχετισμών Ασφαλείας στο χώρο αποθήκευσης Azure, ανατρέξτε στο θέμα [Κατανόηση των κοινόχρηστων Access υπογραφές].

- Δημιουργία ενός ελεγκτή που υποκατηγορίες `StorageController` προκειμένου να χειρίζεται τις αιτήσεις διακριτικού συσχετισμών Ασφαλείας και να λάβετε τα αρχεία που σχετίζονται με μια εγγραφή. Από προεπιλογή, τα αρχεία συσχετίζονται με μια εγγραφή χρησιμοποιώντας το Αναγνωριστικό εγγραφής ως μέρος του το όνομα του κοντέινερ. η συμπεριφορά μπορεί να προσαρμοστεί, καθορίζοντας μια εφαρμογή της `IContainerNameResolver`. Μπορείτε επίσης να προσαρμόσετε την πολιτική διακριτικού συσχετισμών Ασφαλείας.

- Το SDK προγράμματος-πελάτη κινητών Azure δεν αποθηκεύει στην πραγματικότητα αποθηκεύει τα δεδομένα του αρχείου. Αντίθετα, το πρόγραμμα-πελάτης SDK καλεί το `IFileSyncHandler`, καθορίζεται ποια, στη συνέχεια, πώς (και if) αποθηκεύονται τα αρχεία στην τοπική συσκευή. Το πρόγραμμα χειρισμού συγχρονισμού έχει καταχωρηθεί ως εξής:

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`ονομάζεται όταν το SDK προγράμματος-πελάτη κινητών Azure πρέπει τα δεδομένα του αρχείου (π.χ., ως μέρος της διαδικασίας αποστολής). Αυτό σας δίνει τη δυνατότητα διαχείριση πώς (και if) αρχεία είναι αποθηκευμένη στην τοπική συσκευή και να επιστρέψετε αυτές τις πληροφορίες όταν χρειάζεται.

      + `IFileSyncHandler.ProcessFileSynchronizationAction`καλείται ως μέρος της ροής συγχρονισμός των αρχείων. Μια αναφορά αρχείου και μια τιμή απαρίθμησης FileSynchronizationAction παρέχονται, ώστε να μπορείτε να αποφασίσετε πώς η εφαρμογή σας θα πρέπει να χειρίζεται αυτό το συμβάν (π.χ. αυτόματα τη λήψη ενός αρχείου όταν δημιουργείται ή ενημερώνεται, τη διαγραφή ενός αρχείου από την τοπική συσκευή όταν αυτό το αρχείο έχει διαγραφεί από το διακομιστή).

- A `MobileServiceFile` μπορεί να χρησιμοποιηθεί είτε σε λειτουργία σύνδεση ή εκτός σύνδεσης, χρησιμοποιώντας ένα `IMobileServiceTable` ή `IMobileServiceSyncTable`, αντίστοιχα. Στο σενάριο για εργασία χωρίς σύνδεση, η αποστολή θα συμβεί όταν η εφαρμογή καλεί `PushFileChangesAsync`. Αυτό θα κάνει την ουρά λειτουργία εκτός σύνδεσης για επεξεργασία; για κάθε εργασία αρχείο, το πρόγραμμα-πελάτη κινητών Azure SDK θα ενεργοποιήσει το `GetDataSource` μέθοδος σε το `IFileSyncHandler` παρουσία για να ανακτήσετε τα περιεχόμενα του αρχείου για την αποστολή.

- Για να ανακτήσετε αρχεία ενός στοιχείου, καλέστε την '' GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>` παρουσία. Αυτή η μέθοδος επιστρέφει μια λίστα των αρχείων που σχετίζονται με το στοιχείο δεδομένων που παρέχονται. (Σημείωση: Αυτή είναι μια *τοπική* λειτουργία και θα επιστρέψει τα αρχεία με βάση την κατάσταση του αντικειμένου όταν συγχρονίστηκε τελευταία. Για να λάβετε μια ενημερωμένη λίστα των αρχείων από το διακομιστή, θα πρέπει να πραγματοποιείτε μια λειτουργία συγχρονισμού πρώτα.)

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- Τη δυνατότητα συγχρονισμού αρχείων χρησιμοποιεί ειδοποιήσεις εγγραφής αλλαγής του τοπικού χώρου αποθήκευσης για να ανακτήσετε τις εγγραφές που λάβατε το πρόγραμμα-πελάτη ως μέρος μιας λειτουργίας push ή ελκυστική. Αυτό είναι δυνατό, ενεργοποιώντας ειδοποιήσεις τοπική και διακομιστή για το περιβάλλον συγχρονισμού χρησιμοποιώντας το `StoreTrackingOptions` παραμέτρου. 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + Άλλες επιλογές παρακολούθησης χώρου αποθήκευσης είναι διαθέσιμη, όπως ειδοποιήσεις μόνο για τοπική ή μόνο για διακομιστή. Μπορείτε να προσθέσετε ή να ο κάτοχος του προσαρμοσμένου επιστροφής κλήσης με χρήση του `EventManager` ιδιότητα `IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- Είναι δυνατή η προσθήκη ή κατάργηση αρχείων από μια εγγραφή τροποποιώντας χώρος αποθήκευσης αντικειμένων blob απευθείας, επειδή η συσχέτιση είναι δυνατό μέσω μια συνθήκη ονοματοθεσίας. Ωστόσο, σε αυτήν την περίπτωση θα πρέπει πάντα **Ενημέρωση της χρονικής σήμανσης εγγραφή όταν το σχετικό αντικείμενα BLOB τροποποιούνται**. Το πρόγραμμα-πελάτη κινητών Azure SDK ενημερώνει πάντα μια εγγραφή κατά την προσθήκη ή κατάργηση ενός αρχείου. 

    Ο λόγος για αυτήν την απαίτηση είναι ότι ορισμένες κινητές συσκευές θα έχουν ήδη την εγγραφή στον τοπικό χώρο αποθήκευσης. Όταν αυτά τα προγράμματα-πελάτες εκτελέσετε μια ελκυστική αυξανόμενες, δεν θα επιστραφούν αυτήν την εγγραφή και ο υπολογιστής-πελάτης δεν θα ερώτημα για το νέο συσχετισμένα αρχεία. Για να αποφύγετε αυτό το πρόβλημα, συνιστάται να κάνετε ενημέρωση της εγγραφής χρονικής σήμανσης κατά την εκτέλεση οποιαδήποτε αλλαγή χώρου αποθήκευσης αντικειμένων blob που δεν χρησιμοποιεί το πρόγραμμα-πελάτη κινητών Azure SDK.

- Το πρόγραμμα-πελάτη project χρησιμοποιεί το μοτίβο [Xamarin.Forms DependencyService] για να φορτώσετε την κλάση συγκεκριμένης πλατφόρμας προς τα δεξιά κατά το χρόνο εκτέλεσης. Σε αυτό το δείγμα, ορίσαμε ένα περιβάλλον εργασίας `IPlatform` με υλοποιήσεις σε κάθε ένα από τα έργα συγκεκριμένης πλατφόρμας.

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[Δημιουργία εφαρμογής Xamarin.Forms]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[Κατανόηση των κοινόχρηστων Access υπογραφών]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[Δημιουργήστε ένα λογαριασμό Azure χώρου αποθήκευσης]:  ../storage/storage-create-storage-account.md#create-a-storage-account
