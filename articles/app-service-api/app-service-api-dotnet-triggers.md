<properties 
    pageTitle="Εφαρμογή υπηρεσίας API εφαρμογή εναυσμάτων | Microsoft Azure" 
    description="Πώς μπορείτε να υλοποιήσετε εναύσματα σε μια εφαρμογή API στο Azure εφαρμογής υπηρεσίας" 
    services="logic-apps" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="rachelap"/>

# <a name="azure-app-service-api-app-triggers"></a>Azure εναύσματα εφαρμογής API της εφαρμογής υπηρεσίας

>[AZURE.NOTE] Αυτήν την έκδοση του άρθρου ισχύει για το API εφαρμογές 2014-12-01-σχήματος προέκδοση.


## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο εξηγεί τον τρόπο για να υλοποιήσετε API εφαρμογή εναύσματα και κατανάλωση τους από μια εφαρμογή λογικής.

Όλα τα τμήματα κώδικα σε αυτό το θέμα αντιγράφονται από το [δείγμα κώδικα εφαρμογής API FileWatcher](http://go.microsoft.com/fwlink/?LinkId=534802). 

Σημειώστε ότι θα πρέπει να κάνετε λήψη του πακέτου παρακάτω nuget για τον κωδικό σε αυτό το άρθρο για να δημιουργήσετε και να εκτελέσετε: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Τι είναι τα API εναύσματα εφαρμογή;

Είναι ένα συνηθισμένο σενάριο για μια εφαρμογή API για να ξεκινήσετε ένα συμβάν, έτσι ώστε τα προγράμματα-πελάτες της εφαρμογής API μπορεί να λάβει την κατάλληλη ενέργεια σε απόκριση του συμβάντος. Ο μηχανισμός REST API βάσει που υποστηρίζει αυτό το σενάριο ονομάζεται ένα έναυσμα εφαρμογής API. 

Για παράδειγμα, ας υποθέσουμε ότι σας κώδικα του προγράμματος-πελάτη είναι με την [εφαρμογή Twitter API σύνδεσης](../app-service-logic/app-service-logic-connector-twitter.md) και τον κωδικό πρέπει να εκτελεί μια ενέργεια που βασίζονται σε νέα tweets που περιέχουν συγκεκριμένες λέξεις. Σε αυτήν την περίπτωση, μπορείτε να ορίσετε μια ψηφοφορία ή push έναυσμα για τη διευκόλυνση αυτή την ανάγκη.

## <a name="poll-trigger-versus-push-trigger"></a>Έναυσμα ψηφοφορίας έναντι έναυσμα push

Προς το παρόν, υποστηρίζονται δύο τύποι εναυσμάτων:

- Έναυσμα ψηφοφορίας - πρόγραμμα-πελάτη σταθμοσκοπεί της εφαρμογής API για ειδοποίηση του συμβάντος αντιμετωπίζετε έχει ενεργοποιηθεί 
- Έναυσμα Push - πρόγραμμα-πελάτη ειδοποιείται από την εφαρμογή API όταν ενεργοποιείται ένα συμβάν 

### <a name="poll-trigger"></a>Έναυσμα ψηφοφορίας

Ένα έναυσμα ψηφοφορίας εφαρμόζεται ως μια κανονική REST API και αναμένει υπολογιστές-πελάτες του (όπως μια εφαρμογή λογικής) για να την ψηφοφορία για να λάβετε ειδοποίηση. Ενώ το πρόγραμμα-πελάτη μπορούν να διατηρούν κατάσταση, το έναυσμα ψηφοφορίας ίδια είναι χωρίς κατάσταση. 

Οι ακόλουθες πληροφορίες σχετικά με τα πακέτα αίτησης και απόκρισης απεικονίζουν ορισμένες βασικές πτυχές της σύμβασης έναυσμα ψηφοφορίας:

- Αίτηση
    - Μέθοδος HTTP: ΛΉΨΗ
    - Παράμετροι
        - triggerState - αυτή η προαιρετική παράμετρος επιτρέπει στους υπολογιστές-πελάτες για να καθορίσετε τους κατάσταση, έτσι ώστε το έναυσμα ψηφοφορίας σωστά να αποφασίσετε εάν θα λάβετε ειδοποίηση ή όχι με βάση την καθορισμένη κατάσταση.
        - API ειδικές παραμέτρους
- Απόκριση
    - Ο κωδικός κατάστασης **200** - αίτηση είναι έγκυρη και υπάρχει μια ειδοποίηση από το έναυσμα. Το περιεχόμενο της ειδοποίησης θα είναι ο οργανισμός απόκρισης. Κεφαλίδα "" Επανάληψη "μετά" στην απάντηση υποδεικνύει ότι πρέπει να ανακτηθεί επιπλέον ειδοποίηση δεδομένων με μια κλήση οι επόμενες αίτηση.
    - Κωδικός κατάστασης **202** - αίτηση είναι έγκυρο, αλλά δεν υπάρχει καμία νέα ειδοποίηση από το έναυσμα.
    - Ο κωδικός κατάστασης **4xx** - αίτηση δεν είναι έγκυρη. Ο υπολογιστής-πελάτης δεν θα πρέπει να επαναλάβει την αίτηση.
    - Ο κωδικός κατάστασης **5xx** - αίτηση έχει ως αποτέλεσμα ένα εσωτερικό σφάλμα διακομιστή ή/και τον προσωρινό ζήτημα. Ο υπολογιστής-πελάτης πρέπει να επαναλάβει την αίτηση.

Το παρακάτω τμήμα κώδικα είναι ένα παράδειγμα του τρόπου για να υλοποιήσετε ένα έναυσμα ψηφοφορίας.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

Για να δοκιμάσετε αυτό το έναυσμα ψηφοφορίας, ακολουθήστε τα παρακάτω βήματα:

1. Ανάπτυξη της εφαρμογής API με τη ρύθμιση ελέγχου ταυτότητας **ανώνυμου**κοινό.
2. Καλέστε τη λειτουργία **αφής** σε ένα αρχείο αφής. Η παρακάτω εικόνα εμφανίζει ένα δείγμα αίτησης μέσω Postman.
   ![Η λειτουργία αφής κλήση μέσω Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Καλέστε το έναυσμα ψηφοφορία με την παράμετρο **triggerState** οριστεί σε μια χρονική σήμανση πριν από το βήμα #2. Η παρακάτω εικόνα εμφανίζει το δείγμα αίτησης μέσω Postman.
   ![Έναυσμα ψηφοφορίας κλήση μέσω Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Έναυσμα Push

Ένα έναυσμα push έχει υλοποιηθεί ως μια κανονική REST API που προωθεί τις ειδοποιήσεις στους υπολογιστές-πελάτες που έχετε καταχωρήσει για να ενημερώνεστε όταν fire συγκεκριμένα συμβάντα.

Οι ακόλουθες πληροφορίες σχετικά με τα πακέτα αίτησης και απόκρισης απεικόνιση ορισμένες βασικές πτυχές της σύμβασης έναυσμα push.

- Αίτηση
    - Μέθοδος HTTP: ΤΟΠΟΘΈΤΗΣΗ
    - Παράμετροι
        - triggerId: απαιτείται - αδιαφανές συμβολοσειράς (όπως ένα GUID) που αναπαριστά την καταχώρηση του ένα έναυσμα push.
        - callbackUrl: απαιτείται - διεύθυνση URL της κλήσης για να καλέσετε όταν ενεργοποιείται το συμβάν. Η κλήση είναι μια απλή κλήση ΔΗΜΟΣΊΕΥΣΗ HTTP.
        - API ειδικές παραμέτρους
- Απόκριση
    - Κατάσταση κώδικα **200** - αίτηση για την καταχώρηση επιτυχής προγράμματος-πελάτη.
    - Ο κωδικός κατάστασης **4xx** - αίτηση δεν είναι έγκυρη. Ο υπολογιστής-πελάτης δεν θα πρέπει να επαναλάβει την αίτηση.
    - Ο κωδικός κατάστασης **5xx** - αίτηση έχει ως αποτέλεσμα ένα εσωτερικό σφάλμα διακομιστή ή/και τον προσωρινό ζήτημα. Ο υπολογιστής-πελάτης πρέπει να επαναλάβει την αίτηση.
- Επιστροφή κλήσης
    - Μέθοδος HTTP: ΔΗΜΟΣΊΕΥΣΗ
    - Σώμα αίτησης: ειδοποίηση περιεχόμενο.

Το παρακάτω τμήμα κώδικα είναι ένα παράδειγμα του τρόπου για να υλοποιήσετε ένα έναυσμα push:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

Για να δοκιμάσετε αυτό το έναυσμα ψηφοφορίας, ακολουθήστε τα παρακάτω βήματα:

1. Ανάπτυξη της εφαρμογής API με τη ρύθμιση ελέγχου ταυτότητας **ανώνυμου**κοινό.
2. Αναζητήστε [http://requestb.in/](http://requestb.in/) για να δημιουργήσετε ένα RequestBin που θα χρησιμοποιηθεί ως διεύθυνση URL της κλήσης σας.
3. Καλέστε το έναυσμα push με GUID ως **triggerId** και τη διεύθυνση URL RequestBin ως **callbackUrl**.
   ![Έναυσμα Push κλήση μέσω Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Καλέστε τη λειτουργία **αφής** σε ένα αρχείο αφής. Η παρακάτω εικόνα εμφανίζει ένα δείγμα αίτησης μέσω Postman.
   ![Η λειτουργία αφής κλήση μέσω Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Ελέγξτε το RequestBin για να επιβεβαιώσετε ότι η επιστροφή κλήσης έναυσμα push ενεργοποιείται με ιδιότητα εξόδου.
   ![Έναυσμα ψηφοφορίας κλήση μέσω Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Περιγράψτε εναύσματα στον ορισμό API

Μετά την υλοποίηση εναύσματα και την ανάπτυξη της εφαρμογής σας API σε Azure, μεταβείτε στο το **API ορισμού** blade στην πύλη του Azure προεπισκόπηση και θα δείτε ότι εναύσματα αναγνωρίζονται αυτόματα το περιβάλλον εργασίας χρήστη, η οποία είναι βάσει από τον ορισμό Swagger 2.0 API της εφαρμογής API.

![API ορισμού Blade](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Εάν κάνετε κλικ στο κουμπί **Λήψη Swagger** και ανοίξτε το αρχείο JSON, θα δείτε αποτελέσματα παρόμοια με τα εξής:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

Η ιδιότητα επέκταση **x-ms-schedular-έναυσμα** είναι πώς εναύσματα περιγράφονται στον ορισμό API και προστίθεται αυτόματα από την πύλη εφαρμογής API όταν κάνετε αίτηση για τον ορισμό API μέσω της πύλης εάν η πρόσκληση σε σε ένα από τα ακόλουθα κριτήρια. (Μπορείτε επίσης να προσθέσετε αυτήν την ιδιότητα με μη αυτόματο τρόπο.)

- Έναυσμα ψηφοφορίας
    - Εάν η μέθοδος HTTP είναι **ΓΡΉΓΟΡΑ**.
    - Εάν η ιδιότητα **operationId** περιέχει τη συμβολοσειρά **έναυσμα**.
    - Εάν η ιδιότητα **parameters** περιλαμβάνει μια παράμετρο με μια ιδιότητα **name** οριστεί **triggerState**.
- Έναυσμα Push
    - Εάν η μέθοδος HTTP είναι **ΤΟΠΟΘΈΤΗΣΗ**.
    - Εάν η ιδιότητα **operationId** περιέχει τη συμβολοσειρά **έναυσμα**.
    - Εάν η ιδιότητα **parameters** περιλαμβάνει μια παράμετρο με μια ιδιότητα **name** οριστεί **triggerId**.

## <a name="use-api-app-triggers-in-logic-apps"></a>Χρήση εναυσμάτων API εφαρμογής στις εφαρμογές της λογικής

### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Λίστα και ρύθμιση παραμέτρων εναύσματα εφαρμογής API στη σχεδίαση εφαρμογών λογικής

Εάν δημιουργήσετε μια εφαρμογή της λογικής της ίδιας ομάδας πόρων ως εφαρμογή API, θα μπορείτε να τα προσθέσετε στον καμβά σχεδίασης κάνοντας απλώς κλικ σε αυτό. Οι παρακάτω εικόνες απεικονίζουν αυτό:

![Εναύσματα στη σχεδίαση εφαρμογής λογικής](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Ρύθμιση παραμέτρων έναυσμα ψηφοφορίας στη σχεδίαση εφαρμογής λογικής](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Ρύθμιση παραμέτρων Push έναυσμα στη σχεδίαση εφαρμογής λογικής](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>Βελτιστοποίηση εναύσματα εφαρμογής API για εφαρμογές λογικής

Αφού προσθέσετε εναύσματα σε μια εφαρμογή API, υπάρχουν μερικά πράγματα που μπορείτε να κάνετε για να βελτιώσετε την εμπειρία κατά τη χρήση του API εφαρμογής σε μια εφαρμογή για λογική.

Για παράδειγμα, η παράμετρος **triggerState** για εναύσματα ψηφοφορίας πρέπει να οριστεί με την εξής παράσταση στην εφαρμογή λογικής. Αυτή η παράσταση πρέπει να υπολογιστεί το τελευταίο την ενεργοποίηση του εναύσματος από την εφαρμογή λογικής και επιστροφής αυτήν την τιμή.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

ΣΗΜΕΊΩΣΗ: Για επεξηγήσεις σχετικά με τις συναρτήσεις που χρησιμοποιούνται στην παράσταση παραπάνω, ανατρέξτε στην τεκμηρίωση στη [Γλώσσα ορισμού λογική εφαρμογής ροής εργασίας](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Θα πρέπει να παρέχετε την παράσταση παραπάνω για την παράμετρο **triggerState** κατά τη χρήση του εναύσματος λογική εφαρμογής χρήστες. Είναι πιθανό να έχουν προκαθορισμένες από το σχεδιαστή της εφαρμογής λογικής έως την ιδιότητα επέκταση **x-ms-scheduler-σύσταση**αυτή την τιμή.  Η ιδιότητα επέκταση **x-ms-ορατότητας** μπορεί να οριστεί στην τιμή της *εσωτερικής* , έτσι ώστε η παράμετρος ίδια δεν εμφανίζεται στην στη σχεδίαση.  Το παρακάτω τμήμα κώδικα απεικονίζει που.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

Για εναύσματα push, η παράμετρος **triggerId** πρέπει να προσδιορίσει μοναδικά την εφαρμογή λογικής. Συνιστώμενη βέλτιστη πρακτική είναι να ορίσετε αυτήν την ιδιότητα στο όνομα της ροής εργασίας, χρησιμοποιώντας την ακόλουθη παράσταση:

    @workflow().name

Η εφαρμογή API χρησιμοποιώντας τις ιδιότητες επέκταση **x-ms-scheduler-προτάσεων** και **x-ms-ορατότητα** του ορισμού API, να μετάδοση στη σχεδίαση εφαρμογής λογικής για να ρυθμίσει αυτόματα αυτή η παράσταση για το χρήστη.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Προσθήκη ιδιοτήτων επέκτασης στο καθορισμού API

Πληροφορίες επιπλέον μετα-δεδομένων - όπως η επέκταση ιδιότητες **x-ms-scheduler-προτάσεων** και **x-ms-ορατότητα** - μπορούν να προστεθούν σε του καθορισμού API με έναν από τους δύο τρόπους: στατική ή δυναμική.

Για στατική μετα-δεδομένα, μπορείτε απευθείας να επεξεργαστείτε το αρχείο */metadata/apiDefinition.swagger.json* στο έργο σας και να προσθέσετε με μη αυτόματο τρόπο τις ιδιότητες.

Για τις εφαρμογές API χρησιμοποιώντας δυναμική μετα-δεδομένων, μπορείτε να επεξεργαστείτε το αρχείο SwaggerConfig.cs για να προσθέσετε ένα φίλτρο λειτουργίας που να προσθέσετε αυτές τις επεκτάσεις.

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Ακολουθεί ένα παράδειγμα του πώς μπορεί να εφαρμοστεί αυτή η κλάση για τη διευκόλυνση το σενάριο δυναμικής μετα-δεδομένων.

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 
