<properties
    pageTitle="Την έξοδο σε μια Azure Redis Cache, χρησιμοποιώντας συναρτήσεις Azure, από τα αναλυτικά στοιχεία ροή Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε μια συνάρτηση Azure συνδεδεμένη μια ουρά Bus υπηρεσίας, για να συμπληρώσετε μια Cache Redis Azure από το αποτέλεσμα της ανάλυσης ροής εργασίας."
    keywords="ροή δεδομένων, redis cache, bus απεσταλμένων"
    services="stream-analytics"
    authors="ryancrawcour"
    manager="jhubbard"
    documentationCenter=""
    />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/26/2016"
    ms.author="ryancraw"/>

# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Πώς μπορείτε να αποθηκεύσετε δεδομένα από το Azure ροή ανάλυση σε μια Azure Redis μνήμη Cache χρήση συναρτήσεων Azure

Ανάλυση Azure ροή σάς επιτρέπει να γρήγορα ανάπτυξη και ανάπτυξη λύσεων χαμηλού κόστους για να αποκτήσετε ιδέες σε πραγματικό χρόνο από συσκευές, αισθητήρες, υποδομή, και εφαρμογές ή οποιαδήποτε ροή δεδομένων. Επιτρέπει σε διάφορες περιπτώσεις χρήσης, όπως σε πραγματικό χρόνο διαχείρισης και παρακολούθησης, εντολή και στοιχείο ελέγχου, απάτη εντοπισμού, συνδεδεμένο αυτοκίνητα και πολλά περισσότερα. Σε πολλά σενάρια, ενδέχεται να θέλετε να αποθηκεύσετε δεδομένα εκτυπωθεί από Azure ροή ανάλυσης σε ένα χώρο αποθήκευσης κατανεμημένων δεδομένων όπως μια cache Azure Redis.

Ας υποθέσουμε ότι είστε μέλος μιας εταιρείας τηλεπικοινωνιακό. Προσπαθείτε να εντοπίσει απάτη SIM όπου πολλαπλές κλήσεις που προέρχονται από την ίδια ταυτότητα, στο ίδιο ώρας, αλλά σε διαφορετική γεωγραφικά θέσεις. Έχουν ανατεθεί με την αποθήκευση όλων των πιθανών ως δόλια τηλεφωνικών κλήσεων στο ένα cache Azure Redis. Σε αυτό το ιστολόγιο, παρέχουμε οδηγίες σε πώς μπορείτε εύκολα να ολοκληρώσετε την εργασία σας. 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Ολοκληρώστε τον [Εντοπισμό απάτη σε πραγματικό χρόνο] [ fraud-detection] Γνωρίστε για ASA

## <a name="architecture-overview"></a>Επισκόπηση αρχιτεκτονικής
![Στιγμιότυπο οθόνης της αρχιτεκτονικής](./media/stream-analytics-functions-redis/architecture-overview.png)

Όπως φαίνεται στην προηγούμενη εικόνα, ανάλυση ροή επιτρέπει ροής δεδομένων εισαγωγής για το ερώτημα και να αποστέλλονται στο αποτέλεσμα. Με βάση το αποτέλεσμα, συναρτήσεις Azure να, στη συνέχεια, ενεργοποιείτε ορισμένες τύπος συμβάντος. 

Σε αυτό το ιστολόγιο, εστίαση σε τμήματος Azure συναρτήσεις αυτό αγωγών ή πιο συγκεκριμένα στην ενεργοποίηση του συμβάντος που αποθηκεύει ως δόλια δεδομένα στο cache.
Μετά την ολοκλήρωση της [Ανίχνευσης απάτη σε πραγματικό χρόνο] [ fraud-detection] το πρόγραμμα εκμάθησης, έχετε μια εισαγωγής (ένα συμβάν διανομέας), ένα ερώτημα και το αποτέλεσμα (χώρος αποθήκευσης αντικειμένων blob) ήδη ρυθμιστεί και την εκτέλεση. Σε αυτό το ιστολόγιο, αλλάξουμε το αποτέλεσμα για να χρησιμοποιήσετε μια υπηρεσία ουράς Bus αντί για αυτό. Μετά από αυτό, θα σας να συνδέσετε μια συνάρτηση Azure σε αυτήν την ουρά. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Δημιουργία και συνδέστε μια υπηρεσία ουράς Bus εξόδου
Για να δημιουργήσετε μια ουρά Bus υπηρεσίας, ακολουθήστε τα βήματα 1 και 2 από την ενότητα .NET στο [Γρήγορα αποτελέσματα με την υπηρεσία Bus ουρές][servicebus-getstarted].
Τώρα ας σύνδεση ουρά με την εργασία ροής ανάλυσης που δημιουργήθηκε με την προηγούμενη Γνωρίστε εντοπισμού απάτη.



1. Στην πύλη του Azure, μεταβείτε στο το blade **εξόδους** της εργασίας σας και επιλέξτε **Προσθήκη** στο επάνω μέρος της σελίδας.

    ![Προσθήκη εξόδους](./media/stream-analytics-functions-redis/adding-outputs.png)

2. Επιλέξτε **Υπηρεσία ουράς Bus** ως το **δέκτη** και ακολουθήστε τις οδηγίες στην οθόνη. Φροντίστε να επιλέξετε το χώρο ονομάτων της υπηρεσίας ουράς Bus που δημιουργήσατε στο [Γρήγορα αποτελέσματα με την υπηρεσία Bus ουρές][servicebus-getstarted]. Όταν τελειώσετε, κάντε κλικ στο κουμπί "δεξιά".
3. Καθορίστε τις ακόλουθες τιμές:
    - **Μορφή σειριοποιητής συμβάν**: JSON
    - **Κωδικοποίηση**: UTF8
    - **ΜΟΡΦΉ**: γραμμής διαχωρισμένα

4. Κάντε κλικ στο κουμπί **Δημιουργία** για να προσθέσετε αυτή την προέλευση και για να επαληθεύσετε ότι ανάλυση ροή μπορεί να συνδεθεί με επιτυχία με το λογαριασμό χώρου αποθήκευσης.

5. Στην καρτέλα **ερώτημα** , αντικαταστήστε το τρέχον ερώτημα με τα εξής. Αντικαταστήστε *[ΥΠΗΡΕΣΊΑΣ BUS το ΌΝΟΜΆ ΣΑΣ]* με το όνομα εξόδου που δημιουργήσατε στο βήμα 3. 

    ```    

        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2

        INTO [YOUR SERVICE BUS NAME]
    
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    
        WHERE CS1.SwitchNum != CS2.SwitchNum
    
    ```

## <a name="create-an-azure-redis-cache"></a>Δημιουργήστε ένα Cache Azure Redis
Δημιουργήστε ένα cache Azure Redis ακολουθώντας την ενότητα .NET με [τον τρόπο χρήσης Azure Redis Cache] [ use-rediscache] μέχρι την ενότητα που ονομάζεται ***Ρύθμιση παραμέτρων του cache, οι υπολογιστές-πελάτες***.
Μόλις ολοκληρωθεί η λήψη, έχετε ένα νέο Cache Redis. Στην περιοχή **όλες τις ρυθμίσεις**, επιλέξτε **τα πλήκτρα πρόσβασης** και Σημείωση προς τα κάτω τη ***συμβολοσειρά σύνδεσης πρωτεύοντος***.

![Στιγμιότυπο οθόνης της αρχιτεκτονικής](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Δημιουργία μιας συνάρτησης Azure
[Δημιουργία του πρώτου συνάρτηση Azure] ακολουθήστε[ functions-getstarted] πρόγραμμα εκμάθησης για γρήγορα αποτελέσματα με τις συναρτήσεις Azure. Εάν έχετε ήδη μια συνάρτηση Azure που θέλετε να χρησιμοποιήσετε, στη συνέχεια, προχωρήστε στο [συγγραφής Redis μνήμη cache](#Writing-to-Redis-Cache)

1. Στην πύλη του, επιλέξτε εφαρμογή υπηρεσιών από το αριστερό πλαίσιο περιήγησης και, στη συνέχεια, κάντε κλικ στο όνομα της εφαρμογής Azure συνάρτηση για να μεταβείτε στην τοποθεσία Web της συνάρτησης εφαρμογής.
    ![Στιγμιότυπο οθόνης της εφαρμογής υπηρεσιών συνάρτηση λίστας](./media/stream-analytics-functions-redis/app-services-function-list.png)

2. Κάντε κλικ στην επιλογή **νέα συνάρτηση > ServiceBusQueueTrigger-C#**. Για τα παρακάτω πεδία, ακολουθήστε αυτές τις οδηγίες:
    - **Όνομα ουράς**: το ίδιο όνομα με το όνομα που πληκτρολογήσατε όταν δημιουργήσατε ουρά [γρήγορα] αποτελέσματα με το ουρές Bus υπηρεσίας[ servicebus-getstarted] (όχι το όνομα της την υπηρεσία bus). Βεβαιωθείτε ότι χρησιμοποιείτε ουρά που είναι συνδεδεμένη με το αποτέλεσμα του ροή ανάλυσης.
    - **Σύνδεση Bus υπηρεσίας**: Επιλέξτε **Προσθήκη μια συμβολοσειρά σύνδεσης**. Για να βρείτε τη συμβολοσειρά σύνδεσης, μεταβείτε στην πύλη του κλασική, επιλέξτε **Bus υπηρεσίας**, η υπηρεσία bus που δημιουργήσατε και **ΠΛΗΡΟΦΟΡΊΕΣ ΣΎΝΔΕΣΗΣ** στο κάτω μέρος της οθόνης. Βεβαιωθείτε ότι βρίσκεστε στην κύρια οθόνη σε αυτήν τη σελίδα. Αντιγράψτε και επικολλήστε τη συμβολοσειρά σύνδεσης. Μην διστάσεις να εισαγάγετε οποιοδήποτε όνομα σύνδεσης.
    
        ![Στιγμιότυπο οθόνης της σύνδεσης bus υπηρεσίας](./media/stream-analytics-functions-redis/servicebus-connection.png)
    - **Δικαιώματα_προσπέλασης**: Επιλέξτε **Διαχείριση**


3. Κάντε κλικ στην επιλογή **Δημιουργία**

## <a name="writing-to-redis-cache"></a>Εγγραφή σε Redis Cache
Δημιουργήσαμε τώρα μια συνάρτηση Azure που διαβάζει από μια υπηρεσία ουρά Bus. Μόνο που απομένει να κάνετε είναι να χρησιμοποιήσετε τη συνάρτηση για να γράψετε αυτά τα δεδομένα στο cache Redis. 

1. Επιλέξτε σας που έχουν δημιουργηθεί πρόσφατα **ServiceBusQueueTrigger**και κάντε κλικ στην επιλογή **συνάρτησης των ρυθμίσεων της εφαρμογής** στην επάνω δεξιά γωνία. Επιλέξτε **ρυθμίσεων εφαρμογής υπηρεσίας > Ρυθμίσεις > Ρυθμίσεις εφαρμογής**

2. Στην ενότητα συμβολοσειρές σύνδεσης, δημιουργήστε ένα όνομα στην ενότητα **όνομα** . Επικολλήστε τη συμβολοσειρά σύνδεσης πρωτεύοντος που εντοπίσατε στο βήμα **Δημιουργία ένα Cache Redis** στην ενότητα **τιμή** . Επιλέξτε **προσαρμοσμένη** όπου εμφανίζεται η ένδειξη **Βάσης δεδομένων SQL**.

3. Στο επάνω μέρος, κάντε κλικ στην επιλογή **Αποθήκευση** .

    ![Στιγμιότυπο οθόνης της σύνδεσης bus υπηρεσίας](./media/stream-analytics-functions-redis/function-connection-string.png)

4. Τώρα, επιστρέψτε στις ρυθμίσεις εφαρμογής υπηρεσίας και επιλέξτε **Εργαλεία > εφαρμογή υπηρεσίας επεξεργασίας (έκδοση Preview) > στην > Μετάβαση**.

    ![Στιγμιότυπο οθόνης της σύνδεσης bus υπηρεσίας](./media/stream-analytics-functions-redis/app-service-editor.png)

5. Σε ένα πρόγραμμα επεξεργασίας της επιλογής σας, δημιουργήστε ένα αρχείο JSON που ονομάζεται **project.json** με τα εξής και αποθηκεύστε το στον τοπικό δίσκο.

        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }

6. Η αποστολή του αρχείου σε στον ριζικό κατάλογο της συνάρτησης σας (δεν WWWROOT). Θα πρέπει να βλέπετε ένα αρχείο που ονομάζεται **project.lock.json** αυτόματα εμφανίζονται, επιβεβαίωση ότι το Nuget πακέτων "StackExchange.Redis" και "Newtonsoft.Json" έχουν εισαχθεί.

7. Στο αρχείο **run.csx** , αντικαταστήστε την προ-δημιουργημένες κώδικα με τον ακόλουθο κώδικα. Στη συνάρτηση lazyConnection, αντικαταστήστε "ΣΤΉΛΗ ΌΝΟΜΑ" με το όνομα που δημιουργήσατε στο βήμα 2 του **χώρου αποθήκευσης δεδομένων στη μνήμη cache του Redis**.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;
    
    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");
    
        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-the-stream-analytics-job"></a>Εκκίνηση της ανάλυσης ροής εργασίας

1. Εκκινήστε την εφαρμογή telcodatagen.exe. Η χρήση της είναι ως εξής:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````

2. Από τη ροή εργασίας ανάλυση blade στην πύλη, κάντε κλικ στο κουμπί " **Έναρξη** " στο επάνω μέρος της σελίδας.

    ![Στιγμιότυπο οθόνης της Έναρξη εργασίας](./media/stream-analytics-functions-redis/starting-job.png)

3. Στο το blade **Έναρξη εργασίας** που εμφανίζεται, επιλέξτε **τώρα** και, στη συνέχεια, κάντε κλικ στο κουμπί **Έναρξη** , στο κάτω μέρος της οθόνης. Η εργασία κατάστασης αλλάζει σε έναρξη και μετά από ορισμένες αλλαγές για να εκτελείται.
 
    ![Στιγμιότυπο οθόνης της επιλογής ώρας έναρξης έργου](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Εκτέλεση λύσης και επιλέξτε αποτελεσμάτων
Επιστροφή στη σελίδα σας **ServiceBusQueueTrigger** , θα πρέπει να βλέπετε τώρα δηλώσεις καταγραφής. Αυτά τα αρχεία καταγραφής από την εμφάνιση που έχετε κάτι από την ουρά Bus υπηρεσίας, που έχει τεθεί σε βάση δεδομένων, και η λήψη του μέσω την ώρα ως το κλειδί!

Για να επαληθεύσετε ότι τα δεδομένα σας είναι σε το cache Redis, μεταβείτε στη σελίδα σας cache Redis στην νέα πύλη (όπως φαίνεται στο προηγούμενο βήμα [Δημιουργία ένα Cache Redis Azure](#Create-an-Azure-Redis-Cache) ) και επιλέξτε Console.

Τώρα μπορείτε να συντάξετε Redis εντολές για να επιβεβαιώσετε ότι τα δεδομένα είναι στην πραγματικότητα στο cache.

![Στιγμιότυπο οθόνης της κονσόλας Redis](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Επόμενα βήματα
Είμαστε πολύ σχετικά με τις νέες ενέργειες συναρτήσεις Azure και ανάλυση ροή να κάνετε μαζί και θα σας ελπίζετε έτσι ξεκλειδώνονται διαθέτει νέες δυνατότητες για εσάς. Εάν έχετε οποιαδήποτε σχόλια για το τι θέλετε στη συνέχεια, μην διστάσεις να χρησιμοποιήσετε την [τοποθεσία Azure UserVoice](https://feedback.azure.com/forums/270577-stream-analytics).

Εάν είστε νέο Microsoft Azure, σας προσκαλούμε να το δοκιμάσετε με την εγγραφή για μια [δωρεάν δοκιμαστική λογαριασμός Azure](https://azure.microsoft.com/pricing/free-trial/). Εάν είστε νέος χρήστης του ροή ανάλυση, στη συνέχεια, σας προσκαλούμε για να [δημιουργήσετε την πρώτη εργασία ανάλυση ροή](stream-analytics-create-a-job.md).

Εάν χρειάζεστε κάποια βοήθεια ή έχετε ερωτήσεις, καταχώρηση στα φόρουμ του [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) ή [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) . 

Μπορείτε επίσης να δείτε τους ακόλουθους πόρους:

- [Αναφορά προγραμματιστών Azure συναρτήσεις](../azure-functions/functions-reference.md)
- [Azure συναρτήσεις C# αναφορά προγραμματιστών](../azure-functions/functions-reference-csharp.md)
- [Azure συναρτήσεις F # αναφορά προγραμματιστών](../azure-functions/functions-reference-fsharp.md)
- [Αναφορά προγραμματιστών Azure NodeJS συναρτήσεις](../azure-functions/functions-reference.md)
- [Azure συναρτήσεις εναύσματα και συνδέσεις](../azure-functions/functions-triggers-bindings.md)
- [Πώς μπορείτε να παρακολουθείτε Azure Redis Cache](../redis-cache/cache-how-to-monitor.md)

Για να παραμείνετε ενημερωμένοι για όλες τις πιο πρόσφατες ειδήσεις και δυνατότητες, ακολουθήστε [@AzureStreaming](https://twitter.com/AzureStreaming) στο Twitter.


[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
