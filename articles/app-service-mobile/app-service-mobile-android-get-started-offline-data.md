<properties
    pageTitle="Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή Mobile Azure (Android)"
    description="Μάθετε πώς να χρησιμοποιείτε εφαρμογών υπηρεσίας Mobile σε cache και συγχρονισμός δεδομένων χωρίς σύνδεση στην εφαρμογή σας Android"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή για κινητές συσκευές Android

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης καλύπτει τη δυνατότητα συγχρονισμού χωρίς σύνδεση του Azure εφαρμογές του Mobile για Android. Συγχρονισμού χωρίς σύνδεση επιτρέπει στους τελικούς χρήστες να αλληλεπιδράσετε με μια εφαρμογή για κινητές συσκευές&mdash;προβολή, προσθήκη ή τροποποίηση δεδομένων&mdash;ακόμα και όταν δεν υπάρχει σύνδεση δικτύου. Οι αλλαγές αποθηκεύονται σε μια τοπική βάση δεδομένων. Όταν η συσκευή είναι πάλι σε σύνδεση, αυτές οι αλλαγές συγχρονίζονται με το απομακρυσμένο υπολογιστή στο παρασκήνιο.

Εάν πρόκειται για την πρώτη εμπειρία με εφαρμογές του Mobile Azure, θα πρέπει να ολοκληρώσετε πρώτα το πρόγραμμα εκμάθησης, [Δημιουργία εφαρμογής Android]. Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, πρέπει να προσθέσετε τα πακέτα επέκτασης πρόσβασης δεδομένων στο έργο σας. Για περισσότερες πληροφορίες σχετικά με τα πακέτα επέκτασης server, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Για να μάθετε περισσότερα σχετικά με τη δυνατότητα συγχρονισμού χωρίς σύνδεση, ανατρέξτε στο θέμα [Συγχρονισμού χωρίς σύνδεση δεδομένων σε εφαρμογές του Mobile Azure].

## <a name="update-the-app-to-support-offline-sync"></a>Ενημέρωση της εφαρμογής για την υποστήριξη συγχρονισμού χωρίς σύνδεση

Με το συγχρονισμό εκτός σύνδεσης, διαβάστε και εγγραφή από έναν *πίνακα συγχρονισμού* (χρησιμοποιώντας το περιβάλλον εργασίας *IMobileServiceSyncTable* ), που είναι μέρος μιας βάσης δεδομένων **SQLite** στη συσκευή σας.

Για να push και να αποσπάσετε αλλαγών μεταξύ της συσκευής και των υπηρεσιών Azure Mobile Services, μπορείτε να χρησιμοποιήσετε ένα *περιβάλλον συγχρονισμού* (*MobileServiceClient.SyncContext*), που θα προετοιμάσετε με την τοπική βάση δεδομένων για να αποθηκεύσετε τοπικά δεδομένα.

1. Στο `TodoActivity.java`, σχολιάζετε τον υπάρχοντα ορισμό των `mToDoTable` και καταργήστε τα σχόλια από την έκδοση πίνακα συγχρονισμού:

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. Στο το `onCreate` μέθοδος σχολιάζετε της υπάρχουσας προετοιμασίας των `mToDoTable` και καταργήστε τα σχόλια από τον ορισμό αυτό:

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. Στο `refreshItemsFromTable` σχολιάζετε τον ορισμό των `results` και καταργήστε τα σχόλια από τον ορισμό αυτό:

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. Σχόλιο από τον ορισμό των `refreshItemsFromMobileServiceTable`.

5. Καταργήστε τα σχόλια από τον ορισμό των `refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. Καταργήστε τα σχόλια από τον ορισμό των `sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Δοκιμή της εφαρμογής

Σε αυτήν την ενότητα, ελέγξτε τη συμπεριφορά με WiFi σε και, στη συνέχεια, απενεργοποιήστε την επιλογή μέσω Wi-Fi για να δημιουργήσετε ένα σενάριο χωρίς σύνδεση.

Όταν προσθέτετε στοιχεία δεδομένων, αυτά που θα διατηρούνται στα του τοπικού χώρου αποθήκευσης SQLite, αλλά δεν είναι συγχρονισμένο με το την υπηρεσία κινητών τηλεφώνων, μέχρι να πατήσετε το κουμπί **Ανανέωση** . Άλλες εφαρμογές ενδέχεται να έχουν διαφορετικές απαιτήσεις όσον αφορά το πότε πρέπει να συγχρονιστούν τα δεδομένα, αλλά για σκοπούς επίδειξης αυτό το πρόγραμμα εκμάθησης έχει ο χρήστης ζητήσει ρητά.

Όταν πατήσετε το κουμπί, ξεκινά μια νέα εργασία στο παρασκήνιο. Το προωθεί πρώτα όλες τις αλλαγές που έγιναν στον τοπικό χώρο αποθήκευσης χρησιμοποιείτε περιβάλλον συγχρονισμού, στη συνέχεια, χρησιμοποιεί τα όλα αλλάξει δεδομένων από το Azure στον τοπικό πίνακα.

### <a name="offline-testing"></a>Έλεγχος για εργασία χωρίς σύνδεση

1. Τοποθετήστε τη συσκευή ή simulator σε *Κατάσταση λειτουργίας αεροπλάνου*. Αυτό δημιουργεί ένα σενάριο χωρίς σύνδεση.

2. Προσθέστε ορισμένα στοιχεία *ToDo* ή να επισημάνετε ορισμένα στοιχεία ως ολοκληρωμένη. Έξοδος από τη συσκευή ή simulator (ή υποχρεωτικά κλείσετε την εφαρμογή) και κάντε επανεκκίνηση. Βεβαιωθείτε ότι έχετε έχει διατηρηθεί τις αλλαγές σας στη συσκευή επειδή διατηρούνται στο χώρο αποθήκευσης του τοπικού SQLite.

3. Προβάλετε τα περιεχόμενα του πίνακα Azure *TodoItem* είτε με ένα SQL εργαλείο όπως *SQL Server Management Studio*ή ένα πρόγραμμα-πελάτη ΥΠΌΛΟΙΠΑ όπως *Fiddler* ή *Postman*. Βεβαιωθείτε ότι τα νέα στοιχεία _δεν_ έχουν συγχρονιστεί με το διακομιστή

    + Για έναν υπολογιστή στο παρασκήνιο Node.js, μεταβείτε στην [πύλη του Azure](https://portal.azure.com/)και στην εφαρμογή Mobile παρασκηνίου κάντε κλικ στην επιλογή **Εύκολο πίνακες** > **TodoItem** για να προβάλετε τα περιεχόμενα του `TodoItem` πίνακα.
    + Για έναν υπολογιστή στο παρασκήνιο .NET, προβάλετε τα περιεχόμενα του πίνακα είτε με ένα εργαλείο SQL όπως *SQL Server Management Studio*, ή με ένα πρόγραμμα-πελάτη ΥΠΌΛΟΙΠΑ όπως *Fiddler* ή *Postman*.

4. Ενεργοποίηση μέσω Wi-Fi στη συσκευή ή simulator. Στη συνέχεια, πατήστε το κουμπί **Ανανέωση** .

5. Προβάλετε τα δεδομένα TodoItem ξανά στην πύλη του Azure. Η νέα και τροποποιημένα TodoItems πρέπει τώρα να εμφανίζεται.

## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Συγχρονισμός δεδομένων χωρίς σύνδεση στο Azure εφαρμογές για κινητές συσκευές]

* [Εξώφυλλο στο cloud: συγχρονισμού χωρίς σύνδεση στις υπηρεσίες του Azure κινητές] \(Σημείωση: το βίντεο σχετικά με τις υπηρεσίες Mobile, αλλά συγχρονισμού χωρίς σύνδεση λειτουργεί με παρόμοιο τρόπο στις εφαρμογές του Mobile Azure\)


<!-- URLs. -->

[Συγχρονισμός δεδομένων χωρίς σύνδεση στο Azure εφαρμογές για κινητές συσκευές]: app-service-mobile-offline-data-sync.md

[Δημιουργία εφαρμογής Android]: app-service-mobile-android-get-started.md

[Εξώφυλλο του cloud: Συγχρονισμού χωρίς σύνδεση στις υπηρεσίες του Azure κινητές συσκευές]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

