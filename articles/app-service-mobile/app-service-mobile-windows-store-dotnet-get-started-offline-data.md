<properties
    pageTitle="Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή σας καθολικής πλατφόρμας των Windows (UWP) με εφαρμογές του Mobile | Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε μια εφαρμογή Mobile Azure σε cache και συγχρονισμός δεδομένων χωρίς σύνδεση στην εφαρμογή καθολικής πλατφόρμας των Windows (UWP)."
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-windows-app"></a>Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή σας των Windows

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να προσθέσετε υποστήριξη χωρίς σύνδεση σε μια εφαρμογή καθολικής πλατφόρμας των Windows (UWP) χρησιμοποιώντας μια εφαρμογή Mobile Azure παρασκηνίου. Συγχρονισμού χωρίς σύνδεση επιτρέπει στους τελικούς χρήστες να αλληλεπιδράσετε με μια εφαρμογή για κινητές συσκευές--προβολή, προσθήκη ή τροποποίηση δεδομένων - ακόμα και όταν δεν υπάρχει σύνδεση δικτύου. Οι αλλαγές αποθηκεύονται σε μια τοπική βάση δεδομένων. Όταν η συσκευή είναι πάλι σε σύνδεση, αυτές οι αλλαγές συγχρονίζονται με το απομακρυσμένο υπολογιστή στο παρασκήνιο.

Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να ενημερώσετε το έργο εφαρμογής UWP από την εκμάθηση [Δημιουργία μια εφαρμογή για Windows] για να υποστηρίζει τις δυνατότητες για εργασία χωρίς σύνδεση του Azure εφαρμογές του Mobile. Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, πρέπει να προσθέσετε τα πακέτα επέκτασης πρόσβασης δεδομένων στο έργο σας. Για περισσότερες πληροφορίες σχετικά με τα πακέτα επέκτασης server, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Για να μάθετε περισσότερα σχετικά με τη δυνατότητα συγχρονισμού χωρίς σύνδεση, ανατρέξτε στο θέμα [Συγχρονισμού χωρίς σύνδεση δεδομένων σε εφαρμογές του Mobile Azure].

## <a name="requirements"></a>Απαιτήσεις

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής προαπαιτούμενα:

* Visual Studio 2013 εκτελούνται στα Windows 8.1 ή νεότερη έκδοση.
* Ολοκλήρωση της [Δημιουργία μια εφαρμογή για Windows],[Δημιουργήστε μια εφαρμογή για windows].
* [Χώρος αποθήκευσης SQLite υπηρεσιών Azure Mobile][sqlite store nuget]
* [SQLite για ανάπτυξη καθολικής πλατφόρμας των Windows](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>Ενημέρωση της εφαρμογής του προγράμματος-πελάτη για την υποστήριξη δυνατοτήτων για εργασία χωρίς σύνδεση

Azure δυνατότητες χωρίς σύνδεση εφαρμογή Mobile σάς επιτρέπουν να αλληλεπιδράσετε με μια τοπική βάση δεδομένων όταν βρίσκεστε σε ένα σενάριο χωρίς σύνδεση. Για να χρησιμοποιήσετε αυτές τις δυνατότητες της εφαρμογής σας, θα προετοιμάσετε ένα [SyncContext] [ synccontext] σε έναν τοπικό χώρο αποθήκευσης. Στη συνέχεια, αναφοράς πίνακα μέσω του περιβάλλοντος εργασίας[IMobileServiceSyncTable] [IMobileServiceSyncTable]. SQLite χρησιμοποιείται ως τον τοπικό χώρο αποθήκευσης στη συσκευή.

1. Εγκαταστήστε το [περιβάλλον εκτέλεσης SQLite στην καθολική πλατφόρμα των Windows](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).

2. Στο Visual Studio, ανοίξτε τη Διαχείριση πακέτου NuGet για το έργο εφαρμογής UWP που έχετε ολοκληρώσει στην εκμάθηση [Δημιουργία μια εφαρμογή για Windows] .
    Αναζητήστε και εγκαταστήστε το πακέτο NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** .

4. Στην Εξερεύνηση λύσεων, κάντε δεξί κλικ σε **αναφορές** > **Προσθήκη αναφοράς...**  >  **Καθολικής Windows** > **επεκτάσεις**, στη συνέχεια, ενεργοποίηση **SQLite για καθολική πλατφόρμα των Windows** και **2015 χρόνου εκτέλεσης Visual C++ για τις εφαρμογές καθολικής πλατφόρμας των Windows**.

    ![Προσθήκη αναφοράς SQLite UWP][1]

5. Ανοίξτε το αρχείο MainPage.xaml.cs και καταργήστε τα σχόλια από το `#define OFFLINE_SYNC_ENABLED` ορισμού.

6. Στο Visual Studio, πατήστε το πλήκτρο **F5** για να δημιουργήσετε ξανά και εκτελέστε την εφαρμογή υπολογιστή-πελάτη. Η εφαρμογή λειτουργεί με τον ίδιο τρόπο όπως και πριν από την έχετε ενεργοποιήσει συγχρονισμού χωρίς σύνδεση. Ωστόσο, της τοπικής βάσης δεδομένων τώρα συμπληρώνεται με δεδομένα που μπορεί να χρησιμοποιηθεί σε ένα σενάριο χωρίς σύνδεση.

## <a name="update-sync"></a>Ενημέρωση της εφαρμογής για να αποσυνδεθείτε από υπολογιστή στο παρασκήνιο

Σε αυτήν την ενότητα, μπορείτε να διακόψετε τη σύνδεση για την εφαρμογή Mobile παρασκηνίου να προσομοιώνουν μια κατάσταση χωρίς σύνδεση. Όταν προσθέτετε στοιχεία δεδομένων, το πρόγραμμα χειρισμού εξαίρεσης σάς ενημερώνει ότι η εφαρμογή είναι σε λειτουργία χωρίς σύνδεση. Σε αυτήν την κατάσταση, νέα στοιχεία προστέθηκαν στο του τοπικού χώρου αποθήκευσης και θα συγχρονίζονται υπόβαθρο εφαρμογής για κινητές συσκευές όταν push Επόμενο εκτελείται σε κατάσταση σύνδεσης.

1. Επεξεργασία App.xaml.cs στο κοινόχρηστο έργο. Σχόλιο εκτός της προετοιμασίας των το **MobileServiceClient** και προσθέστε την ακόλουθη γραμμή, η οποία χρησιμοποιεί μια διεύθυνση URL δεν είναι έγκυρη εφαρμογή για κινητές συσκευές:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Μπορείτε να δείχνουν συμπεριφοράς χωρίς σύνδεση με την απενεργοποίηση μέσω Wi-Fi και δίκτυα κινητής τηλεφωνίας στη συσκευή ή να χρησιμοποιήσετε την κατάσταση λειτουργίας αεροπλάνου.

2. Πατήστε το πλήκτρο **F5** για να δημιουργήσετε και να εκτελέσετε την εφαρμογή. Ειδοποίηση συγχρονισμού σας απέτυχε κατά την ανανέωση κατά την εκκίνηση της εφαρμογής.

3. Πληκτρολογήστε νέα στοιχεία και παρατηρήστε ότι push αποτυγχάνει με κατάσταση [CancelledByNetworkError] κάθε φορά που κάνετε κλικ στο κουμπί **Αποθήκευση**. Ωστόσο, τα νέα στοιχεία todo υπάρχει στον τοπικό χώρο αποθήκευσης μέχρι να είναι δυνατή η προώθηση τους στον υπολογιστή στο παρασκήνιο εφαρμογής για κινητές συσκευές.  Σε μια εφαρμογή παραγωγής, εάν μπορείτε να αποκρύψετε τις εξής εξαιρέσεις στην εφαρμογή υπολογιστή-πελάτη συμπεριφέρεται σαν να εξακολουθεί να είναι συνδεδεμένος στον υπολογιστή στο παρασκήνιο εφαρμογής για κινητές συσκευές.

4. Κλείστε την εφαρμογή και κάντε επανεκκίνηση, για να βεβαιωθείτε ότι τα νέα στοιχεία που δημιουργήσατε διατηρούνται στον τοπικό χώρο αποθήκευσης.

5. (Προαιρετικό) Στο Visual Studio, ανοίξτε την **Εξερεύνηση διακομιστή**. Περιήγηση στη βάση δεδομένων σας στο **Azure**->**Βάσεις δεδομένων SQL**. Κάντε δεξί κλικ σε βάση δεδομένων σας και επιλέξτε **Άνοιγμα στην Εξερεύνηση αντικειμένου SQL Server**. Τώρα μπορείτε να περιηγηθείτε σε πίνακα της βάσης δεδομένων SQL και τα περιεχόμενά της. Βεβαιωθείτε ότι τα δεδομένα στη βάση δεδομένων παρασκηνίου δεν έχει αλλάξει.

6. (Προαιρετικό) Χρησιμοποιήστε ένα εργαλείο ΥΠΌΛΟΙΠΑ όπως Fiddler ή Postman ερώτημα για τις κινητές συσκευές παρασκηνίου, με χρήση ενός ερωτήματος ΛΉΨΗ στη φόρμα `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Ενημέρωση της εφαρμογής για να συνδεθείτε ξανά την εφαρμογή Mobile παρασκηνίου

Σε αυτήν την ενότητα, μπορείτε να συνδεθείτε ξανά την εφαρμογή στον υπολογιστή στο παρασκήνιο εφαρμογής για κινητές συσκευές. Αυτές οι αλλαγές προσομοίωση επανασύνδεση δικτύου στην εφαρμογή του.

Πρώτη φορά που εκτελείτε την εφαρμογή, η `OnNavigatedTo` κλήσεων χειρισμού συμβάντων `InitLocalStoreAsync`. Αυτή η μέθοδος κλήσεις με τη σειρά `SyncAsync` για συγχρονισμό του τοπικού χώρου αποθήκευσης με βάση δεδομένων παρασκηνίου. Η εφαρμογή προσπαθεί να συγχρονίσετε κατά την εκκίνηση.

1. Ανοίξτε το App.xaml.cs στο κοινόχρηστο έργο και καταργήστε τα σχόλια από το προηγούμενο η προετοιμασία του `MobileServiceClient` για να χρησιμοποιήσετε το σωστό τη διεύθυνση URL της εφαρμογής για κινητές συσκευές.

2. Πατήστε το πλήκτρο **F5** για εκ νέου δημιουργία και την εκτέλεση της εφαρμογής. Η εφαρμογή συγχρονίζει τις τοπικές αλλαγές σας με την εφαρμογή Mobile Azure παρασκηνίου χρησιμοποιώντας λειτουργίες push και ελκυστική όταν το `OnNavigatedTo` πρόγραμμα χειρισμού συμβάντων εκτελείται.

3. (Προαιρετικό) Προβάλετε τα ενημερωμένα δεδομένα χρησιμοποιώντας Εξερεύνηση αντικειμένου SQL Server ή ένα εργαλείο ΥΠΌΛΟΙΠΑ όπως Fiddler. Ανακοίνωση των δεδομένων έχει συγχρονιστεί μεταξύ της βάσης δεδομένων παρασκηνίου Azure εφαρμογή Mobile και του τοπικού χώρου αποθήκευσης.

4. Στην εφαρμογή, επιλέξτε το πλαίσιο ελέγχου δίπλα στο στοιχείο μερικά στοιχεία για να ολοκληρώσετε τις στον τοπικό χώρο αποθήκευσης.

  `UpdateCheckedTodoItem`κλήσεις `SyncAsync` ο συγχρονισμός κάθε ολοκληρώθηκε στοιχείο με την εφαρμογή Mobile παρασκηνίου. `SyncAsync`κλήσεις Ώθηση και έλξη. Ωστόσο, **κάθε φορά που εκτελείτε μια ελκυστική σε σχέση με έναν πίνακα που ο υπολογιστής-πελάτης έχει κάνει αλλαγές, πάτημα πάντα εκτελείται αυτόματα**. Αυτή η συμπεριφορά εξασφαλίζει όλους τους πίνακες του τοπικού χώρου αποθήκευσης μαζί με τις σχέσεις παραμένουν συνεπή. Αυτή η συμπεριφορά ενδέχεται να οδηγήσει σε μια μη αναμενόμενο push.  Για περισσότερες πληροφορίες σχετικά με αυτήν τη συμπεριφορά, ανατρέξτε στο θέμα [Δεδομένων συγχρονισμού χωρίς σύνδεση σε εφαρμογές του Mobile Azure].


##<a name="api-summary"></a>Σύνοψη API

Για να υποστηρίζει τις δυνατότητες για εργασία χωρίς σύνδεση κινητές συσκευές των υπηρεσιών, θα σας χρησιμοποιείται το περιβάλλον εργασίας [IMobileServiceSyncTable] και προετοιμασία [MobileServiceClient.SyncContext] [ synccontext] με μια τοπική βάση δεδομένων SQLite. Όταν είστε εκτός σύνδεσης, τις κανονικές λειτουργίες CRUD για τις εφαρμογές του Mobile λειτουργούν σαν να εξακολουθεί να είναι συνδεδεμένος στην εφαρμογή, ενώ οι λειτουργίες προκύψουν έναντι του τοπικού χώρου αποθήκευσης. Τις παρακάτω μεθόδους που χρησιμοποιούνται για το συγχρονισμό του τοπικού χώρου αποθήκευσης με το διακομιστή:

*  **[PushAsync]** Επειδή αυτή η μέθοδος είναι μέλος της [IMobileServicesSyncContext], τις αλλαγές σε όλους τους πίνακες προωθούνται στον υπολογιστή στο παρασκήνιο. Μόνο οι εγγραφές με τις τοπικές αλλαγές αποστέλλονται στο διακομιστή.

* **[PullAsync] ** 
   ξεκινήσει με μια ελκυστική από μια [IMobileServiceSyncTable]. Όταν υπάρχουν εντοπισμένες αλλαγές στον πίνακα, ένα μη ρητή push εκτελείται για να βεβαιωθείτε ότι διατηρούνται όλους τους πίνακες του τοπικού χώρου αποθήκευσης μαζί με τις σχέσεις συνεπή. Η παράμετρος *pushOtherTables* ελέγχει αν άλλους πίνακες στο περιβάλλον μετακινούνται σε μια μη ρητή push. Η παράμετρος *ερωτήματος* λαμβάνει μια [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] 
   ή OData συμβολοσειρά ερωτήματος για να φιλτράρετε τα δεδομένα που επιστρέφονται. Η παράμετρος *queryId* χρησιμοποιείται για να ορίσετε μια επαυξητική συγχρονισμού. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα  [Δεδομένων συγχρονισμού χωρίς σύνδεση σε εφαρμογές του Mobile Azure](app-service-mobile-offline-data-sync.md#how-sync-works).

* **[PurgeAsync]** Εφαρμογή σας πρέπει να καλέσετε περιοδικά αυτήν τη μέθοδο για να γίνει εκκαθάριση παλιά δεδομένα από τον τοπικό χώρο αποθήκευσης. Χρησιμοποιήστε την παράμετρο *επιβάλετε* όταν πρέπει να εκκαθάριση τυχόν αλλαγές που δεν έχουν συγχρονιστεί ακόμη.

Για περισσότερες πληροφορίες σχετικά με αυτά τα θέματα, ανατρέξτε στο θέμα [Δεδομένων συγχρονισμού χωρίς σύνδεση σε εφαρμογές του Mobile Azure](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Περισσότερες πληροφορίες

Τα παρακάτω θέματα παρέχουν πληροφορίες φόντου επιπλέον τη δυνατότητα συγχρονισμού χωρίς σύνδεση από εφαρμογές του Mobile:

* [Συγχρονισμός δεδομένων χωρίς σύνδεση στο Azure εφαρμογές για κινητές συσκευές]
* [ΔΙΑΔΙΚΑΣΙΕΣ SDK .NET Azure εφαρμογές για κινητές συσκευές][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Συγχρονισμός δεδομένων χωρίς σύνδεση στο Azure εφαρμογές για κινητές συσκευές]: app-service-mobile-offline-data-sync.md
[Δημιουργήστε μια εφαρμογή για windows]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
