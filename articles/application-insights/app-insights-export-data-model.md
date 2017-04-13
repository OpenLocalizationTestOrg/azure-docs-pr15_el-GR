<properties 
    pageTitle="Μοντέλο δεδομένων ιδέες εφαρμογής" 
    description="Περιγράφει τις ιδιότητες που έχουν εξαχθεί από συνεχής εξαγωγής στο JSON και χρησιμοποιούνται ως φίλτρα." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>Μοντέλο δεδομένων εξαγωγή ιδέες εφαρμογής

Αυτός ο πίνακας παραθέτει τις ιδιότητες του τηλεμετρίας αποστέλλεται από την [Εφαρμογή ιδέες](app-insights-overview.md) SDK στην πύλη του. Θα δείτε αυτές τις ιδιότητες σε δεδομένων εξόδου από [Συνεχής εξαγωγή](app-insights-export-telemetry.md).
Επίσης, εμφανίζονται σε φίλτρα ιδιοτήτων στην [Εξερεύνηση μετρικό](app-insights-metrics-explorer.md) και [Διαγνωστικών αναζήτησης](app-insights-diagnostic-search.md).

Σημεία για να λάβετε υπόψη:

* `[0]`σε αυτούς τους πίνακες υποδηλώνει ένα σημείο στη διαδρομή όπου έχετε για να εισαγάγετε ένα ευρετήριο. αλλά πάντα δεν είναι 0.
* Είναι διάρκειες ώρας σε δέκατα microsecond, επομένως 10000000 == 1 δευτερόλεπτο.
* Ημερομηνίες και ώρες είναι UTC και δίνεται στη μορφή ISO`yyyy-MM-DDThh:mm:ss.sssZ`

Υπάρχουν αρκετές [παραδείγματα](app-insights-export-telemetry.md#code-samples) που δείχνουν τον τρόπο χρήσης τους.



## <a name="example"></a>Παράδειγμα

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>Περιβάλλον

Όλοι οι τύποι τηλεμετρίας συνοδεύεται από μια ενότητα περιβάλλοντος. Δεν είναι όλα αυτά τα πεδία έχουν μεταδοθεί με κάθε σημείο δεδομένων.



|Διαδρομή|Τύπος|Σημειώσεις|
|---|---|---|
| Context.Custom.Dimensions [0]  | αντικείμενο]  | Ορίστε ζεύγη κλειδιού-τιμής συμβολοσειράς, προσαρμοσμένες ιδιότητες παραμέτρου. Μέγιστο μήκος 100, τιμές μέγιστο μήκος κλειδιού 1024. Λιγότερα από 100 μοναδικές τιμές, η ιδιότητα μπορεί να γίνει αναζήτηση αλλά δεν μπορεί να χρησιμοποιηθεί για το τμήμα αγοράς. Πλήκτρα Max 200 ανά ikey.  |
| Context.Custom.metrics [0]  | αντικείμενο]  | Ορισμός με παράμετρο προσαρμοσμένες διαστάσεις και με TrackMetrics ζεύγη κλειδιού-τιμής. Μέγιστο μήκος κλειδιού 100, τιμές μπορεί να είναι αριθμητική. |
| context.data.eventTime | συμβολοσειρά | UTC |
| context.data.isSynthetic | δυαδική τιμή | Αίτηση φαίνεται να προέρχονται από μια δοκιμαστική bot ή web. |
| context.data.samplingRate | αριθμός | Ποσοστό τηλεμετρίας που δημιουργούνται από το SDK που έχει αποσταλεί στην πύλη. Περιοχή 0,0 100.0.|
| Context.Device | αντικείμενο | Συσκευή-πελάτη |
| Context.Device.Browser | συμβολοσειρά | IE, Chrome... |
| context.device.browserVersion | συμβολοσειρά | Chrome 48,0... |
| context.device.deviceModel | συμβολοσειρά | |
| context.device.deviceName | συμβολοσειρά | |
| Context.Device.ID | συμβολοσειρά | |
| Context.Device.Locale | συμβολοσειρά | EN-GB, de-DE... |
| Context.Device.Network | συμβολοσειρά | |
| context.device.oemName | συμβολοσειρά | |
| context.device.osVersion | συμβολοσειρά | Κεντρικός υπολογιστής λειτουργικό σύστημα |
| context.device.roleInstance | συμβολοσειρά | Αναγνωριστικό του κεντρικού υπολογιστή διακομιστή |
| context.device.roleName | συμβολοσειρά | |
| Context.Device.Type | συμβολοσειρά | PC, το πρόγραμμα περιήγησης... |
| Context.Location | αντικείμενο | Που προέρχονται από clientip. |
| Context.Location.City | συμβολοσειρά | Προέρχεται από clientip, εάν το γνωρίζετε  |
| Context.Location.clientip | συμβολοσειρά | Τελευταία οκτάγωνο είναι anonymized με το 0. |
| Context.Location.continent | συμβολοσειρά | |
| Context.Location.Country | συμβολοσειρά | |
| Context.Location.province | συμβολοσειρά | Νομός ή επαρχία |
| Context.Operation.ID | συμβολοσειρά | Στοιχεία που έχουν το ίδιο αναγνωριστικό λειτουργίας εμφανίζονται ως σχετικά στοιχεία στην πύλη. Συνήθως το αναγνωριστικό αίτησης. |
| Context.Operation.Name | συμβολοσειρά | διεύθυνση URL ή αίτηση όνομα |
| context.operation.parentId | συμβολοσειρά | Επιτρέπει ένθετη σχετικά στοιχεία. |
| Context.Session.ID | συμβολοσειρά | Το αναγνωριστικό ομάδας λειτουργιών από την ίδια προέλευση. Μια περίοδο 30 λεπτά χωρίς λειτουργία σήματα στο τέλος της περιόδου λειτουργίας. |
| context.session.isFirst | δυαδική τιμή | |
| context.user.accountAcquisitionDate | συμβολοσειρά | |
| context.user.anonAcquisitionDate | συμβολοσειρά | |
| context.user.anonId | συμβολοσειρά | |
| context.user.authAcquisitionDate | συμβολοσειρά | [Χρήστη με έλεγχο ταυτότητας](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | δυαδική τιμή | |
| internal.data.documentVersion | συμβολοσειρά | |
| Internal.Data.ID | συμβολοσειρά | |



## <a name="events"></a>Συμβάντα

Προσαρμοσμένα συμβάντα που δημιουργούνται με [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event). 


|Διαδρομή|Τύπος|Σημειώσεις|
|---|---|---|
| Πλήθος συμβάντων [0] | Ακέραιος αριθμός | 100 / (επιτόκιο[δειγματοληψία](app-insights-sampling.md) ). Για παράδειγμα 4 =&gt; 25%. |
| όνομα συμβάντος [0] | συμβολοσειρά | Όνομα του συμβάντος.  Μέγιστο μήκος 250. |
| διεύθυνση url συμβάν [0] | συμβολοσειρά | |
| urlData.base συμβάν [0] | συμβολοσειρά | |
| urlData.host συμβάν [0] | συμβολοσειρά | |

## <a name="exceptions"></a>Εξαιρέσεις

Αναφορές [Εξαιρέσεις](app-insights-asp-net-exceptions.md) στο διακομιστή και στο πρόγραμμα περιήγησης. 


|Διαδρομή|Τύπος|Σημειώσεις|
|---|---|---|
| συγκρότησης basicException [0] | συμβολοσειρά | |
| Πλήθος basicException [0] | Ακέραιος αριθμός | 100 / (επιτόκιο[δειγματοληψία](app-insights-sampling.md) ). Για παράδειγμα 4 =&gt; 25%. |
| exceptionGroup basicException [0] | συμβολοσειρά | |
| exceptionType basicException [0] | συμβολοσειρά | |συμβολοσειρά | |
| failedUserCodeMethod basicException [0] | συμβολοσειρά | |
| failedUserCodeAssembly basicException [0] | συμβολοσειρά | |
| handledAt basicException [0] | συμβολοσειρά | |
| hasFullStack basicException [0] | δυαδική τιμή | |
| αναγνωριστικό basicException [0] | συμβολοσειρά | |
| μέθοδος basicException [0] | συμβολοσειρά | |
| μήνυμα basicException [0] | συμβολοσειρά | Το μήνυμα εξαίρεσης. Μέγιστο μήκος 10k.|
| outerExceptionMessage basicException [0] | συμβολοσειρά | |
| outerExceptionThrownAtAssembly basicException [0] | συμβολοσειρά | |
| outerExceptionThrownAtMethod basicException [0] | συμβολοσειρά | |
| outerExceptionType basicException [0] | συμβολοσειρά | |
| outerId basicException [0] | συμβολοσειρά | |
| συγκρότησης parsedStack [0] basicException [0] | συμβολοσειρά | |
| όνομα αρχείου parsedStack [0] basicException [0] | συμβολοσειρά | |
| επίπεδο parsedStack [0] basicException [0] | Ακέραιος αριθμός | |
| γραμμή parsedStack [0] basicException [0] | Ακέραιος αριθμός | |
| μέθοδος parsedStack [0] basicException [0] | συμβολοσειρά | |
| στοίβα basicException [0] | συμβολοσειρά | Μέγιστο μήκος 10k|
| typeName basicException [0] | συμβολοσειρά | |



## <a name="trace-messages"></a>Μηνύματα ανίχνευσης

Αποστολή με [TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)και με τη δυνατότητα [καταγραφής προσαρμογέων](app-insights-asp-net-trace-logs.md).


|Διαδρομή|Τύπος|Σημειώσεις|
|---|---|---|
| loggerName μήνυμα [0] | συμβολοσειρά ||
| παράμετροι μήνυμα [0] | συμβολοσειρά ||
| μήνυμα [0] ανεπεξέργαστα | συμβολοσειρά | Το μήνυμα καταγραφής, μέγιστο μήκος 10k. |
| severityLevel μήνυμα [0] | συμβολοσειρά | |



## <a name="remote-dependency"></a>Απομακρυσμένη εξάρτησης

Αποστέλλονται από TrackDependency. Χρησιμοποιείται για την απόδοση της αναφοράς και η χρήση της [κλήσεις με τις εξαρτήσεις](app-insights-asp-net-dependencies.md) στο διακομιστή και AJAX κλήσεις στο πρόγραμμα περιήγησης.

|Διαδρομή|Τύπος|Σημειώσεις|
|---|---|---|
| ασύγχρονη remoteDependency [0] | δυαδική τιμή | |
| baseName remoteDependency [0] | συμβολοσειρά |  |
| Όνομα_εντολής remoteDependency [0] | συμβολοσειρά | Για παράδειγμα "αρχική/ευρετήριο" |
| Πλήθος remoteDependency [0] | Ακέραιος αριθμός | 100 / (επιτόκιο[δειγματοληψία](app-insights-sampling.md) ). Για παράδειγμα 4 =&gt; 25%. |
| dependencyTypeName remoteDependency [0] | συμβολοσειρά | HTTP, SQL... |
| durationMetric.value remoteDependency [0] | αριθμός | Το χρονικό διάστημα από κλήση ολοκλήρωσης της απόκρισης από εξάρτηση |
| αναγνωριστικό remoteDependency [0] | συμβολοσειρά | |
| όνομα remoteDependency [0] | συμβολοσειρά | Διεύθυνση URL. Μέγιστο μήκος 250.|
| resultCode remoteDependency [0] | συμβολοσειρά | από HTTP εξάρτησης |
| επιτυχίας remoteDependency [0] | δυαδική τιμή | |
| Τύπος remoteDependency [0] | συμβολοσειρά | HTTP, Sql... |
| διεύθυνση url remoteDependency [0] | συμβολοσειρά |  Μέγιστο μήκος 2000 |
| urlData.base remoteDependency [0] | συμβολοσειρά | Μέγιστο μήκος 2000 |
| urlData.hashTag remoteDependency [0] | συμβολοσειρά | |
| urlData.host remoteDependency [0] | συμβολοσειρά | Μέγιστο μήκος 200 |


## <a name="requests"></a>Προσκλήσεις

Αποστέλλονται από [TrackRequest](app-insights-api-custom-events-metrics.md#track-request). Βασικές λειτουργικές μονάδες Χρησιμοποιήστε αυτήν την επιλογή για να το χρόνο απόκρισης διακομιστή αναφορών, μετράται στο διακομιστή. 


|Διαδρομή|Τύπος|Σημειώσεις|
|---|---|---|
| μέτρηση αιτήσεων [0] | Ακέραιος αριθμός | 100 / (επιτόκιο[δειγματοληψία](app-insights-sampling.md) ). Για παράδειγμα: 4 =&gt; 25%. |
| durationMetric.value αίτηση [0] | αριθμός | Το χρονικό διάστημα από αίτηση φτάνει απάντηση. 1e7 == 1s |
| αναγνωριστικό αίτησης [0] | συμβολοσειρά | Αναγνωριστικό λειτουργίας |
| όνομα αίτησης [0] | συμβολοσειρά | ΛΉΨΗ/ΚΑΤΑΧΏΡΗΣΗ + βασική διεύθυνση url.  Μέγιστο μήκος 250 |
| responseCode αίτηση [0] | Ακέραιος αριθμός | Απόκριση HTTP που αποστέλλονται στο πρόγραμμα-πελάτη |
| αίτηση [0] επιτυχίας | δυαδική τιμή | Προεπιλεγμένη == (responseCode &lt; 400) |
| διεύθυνση url αίτηση [0] | συμβολοσειρά | Εκτός του κεντρικού υπολογιστή |
| urlData.base αίτηση [0] | συμβολοσειρά | |
| urlData.hashTag αίτηση [0] | συμβολοσειρά |  |
| urlData.host αίτηση [0] | συμβολοσειρά | |


## <a name="page-view-performance"></a>Προβολή σελίδας επιδόσεων

Αποστέλλονται από το πρόγραμμα περιήγησης. Μετρά το χρόνο για την επεξεργασία μιας σελίδας, από το χρήστη προετοιμασία την αίτηση για να εμφανίσετε ολοκλήρωσης (εξαιρουμένων των ασύγχρονων AJAX κλήσεις).

Περιβάλλον τιμές εμφανίζουν προγράμματος-πελάτη λειτουργικό σύστημα και έκδοση του προγράμματος περιήγησης. 


|Διαδρομή|Τύπος|Σημειώσεις|
|---|---|---|
| clientProcess.value clientPerformance [0] | Ακέραιος αριθμός | Το χρονικό διάστημα από το τέλος της λαμβάνει την HTML για την εμφάνιση της σελίδας. |
| όνομα clientPerformance [0] | συμβολοσειρά | |
| networkConnection.value clientPerformance [0] | Ακέραιος αριθμός | Ο χρόνος που λαμβάνονται για να δημιουργήσετε μια σύνδεση δικτύου. |
| receiveRequest.value clientPerformance [0] | Ακέραιος αριθμός | Το χρονικό διάστημα από τη λήξη της στέλνει την αίτηση για να λαμβάνετε την HTML σε απάντηση. |
| sendRequest.value clientPerformance [0] | Ακέραιος αριθμός | Ώρα που έχετε δημιουργήσει για να στείλετε μια αίτηση HTTP. |
| total.value clientPerformance [0] | Ακέραιος αριθμός | Το χρονικό διάστημα από έναρξη για να στείλετε την πρόσκληση για την εμφάνιση της σελίδας. |
| διεύθυνση url clientPerformance [0] | συμβολοσειρά | Διεύθυνση URL αυτής της αίτησης |
| urlData.base clientPerformance [0] | συμβολοσειρά | |
| urlData.hashTag clientPerformance [0] | συμβολοσειρά | |
| urlData.host clientPerformance [0] | συμβολοσειρά | |
| urlData.protocol clientPerformance [0] | συμβολοσειρά | |

## <a name="page-views"></a>Προβολές σελίδας

Αποστέλλονται από trackPageView() ή [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view)

|Διαδρομή|Τύπος|Σημειώσεις|
|---|---|---|
| Καταμέτρηση προβολή [0] | Ακέραιος αριθμός | 100 / (επιτόκιο[δειγματοληψία](app-insights-sampling.md) ). Για παράδειγμα 4 =&gt; 25%. |
| προβολή [0] durationMetric.value | Ακέραιος αριθμός | Η τιμή προαιρετικά στο trackPageView() ή με startTrackPage() - stopTrackPage(). Δεν είναι η ίδια ως clientPerformance τιμές. |
| όνομα προβολής [0] | συμβολοσειρά | Τίτλο σελίδας.  Μέγιστο μήκος 250 |
| διεύθυνση url προβολής [0] | συμβολοσειρά | |
| προβολή [0] urlData.base | συμβολοσειρά | |
| προβολή [0] urlData.hashTag | συμβολοσειρά | |
| προβολή [0] urlData.host | συμβολοσειρά | |



## <a name="availability"></a>Διαθεσιμότητα

Αναφορές [διαθεσιμότητα web δοκιμές](app-insights-monitor-web-app-availability.md).

|Διαδρομή|Τύπος|Σημειώσεις|
|---|---|---|
| διαθεσιμότητα [0] availabilityMetric.name | συμβολοσειρά | διαθεσιμότητα |
| διαθεσιμότητα [0] availabilityMetric.value | αριθμός |1.0 ή 0,0 |
| Καταμέτρηση διαθεσιμότητα [0] | Ακέραιος αριθμός | 100 / (επιτόκιο[δειγματοληψία](app-insights-sampling.md) ). Για παράδειγμα 4 =&gt; 25%. |
| διαθεσιμότητα [0] dataSizeMetric.name | συμβολοσειρά | |
| διαθεσιμότητα [0] dataSizeMetric.value | Ακέραιος αριθμός | |
| διαθεσιμότητα [0] durationMetric.name | συμβολοσειρά | |
| διαθεσιμότητα [0] durationMetric.value | αριθμός | Διάρκεια της δοκιμής. 1e7 == 1s |
| μήνυμα διαθεσιμότητα [0] | συμβολοσειρά | Αποτυχία διαγνωστικών |
| αποτέλεσμα διαθεσιμότητα [0] | συμβολοσειρά | Επιτυχίας/αποτυχίας |
| διαθεσιμότητα [0] runLocation | συμβολοσειρά | Προέλευση παν http req |
| διαθεσιμότητα [0] testName | συμβολοσειρά | |
| διαθεσιμότητα [0] testRunId | συμβολοσειρά | |
| διαθεσιμότητα [0] testTimestamp | συμβολοσειρά | |




## <a name="metrics"></a>Μετρήσεις

Που δημιουργείται από την TrackMetric().

Η τιμή μετρικό βρίσκεται στο context.custom.metrics[0]

Για παράδειγμα:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Σχετικά με τις τιμές μονάδων μέτρησης

Μετρικό τιμές, τόσο σε μετρικό αναφορές και αλλού, αναφέρονται με μια τυπική αντικειμένου δομή. Για παράδειγμα:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Αυτήν τη στιγμή - μέσω αυτό μπορεί να αλλάξει στο μέλλον - στο όλες τις τιμές που αναφέρθηκε από τις βασικές λειτουργικές μονάδες SDK, `count==1` και μόνο το `name` και `value` πεδία είναι χρήσιμη. Μόνο πεζών και κεφαλαίων γραμμάτων όπου θα είναι διαφορετικά θα ήταν εάν γράφετε το δικό σας κλήσεις TrackMetric στο οποίο ορίζετε τις άλλες παραμέτρους. 

Είναι ο σκοπός της τα άλλα πεδία για να επιτρέψετε μετρικά να συναθροιστεί στο SDK, για να μειώσετε την κυκλοφορία στην πύλη του. Για παράδειγμα, που θα μπορούσε να μέσες πολλές διαδοχικές ενδείξεις πριν από την αποστολή κάθε μετρικό αναφορά. Στη συνέχεια, θέλετε τον υπολογισμό του min, max, την τυπική απόκλιση και συγκεντρωτική τιμή (άθροισμα ή μέσο όρο) και ορίστε καταμέτρηση του αριθμού των μετρήσεων που αναπαρίσταται από την αναφορά. 

Στην παραπάνω πίνακες, θα σας έχουν παραλειφθεί τα πεδία που χρησιμοποιούνται σπάνια count, min, max, τυπική απόκλιση και sampledValue.

Αντί για προ-συγκέντρωση μετρήσεις, μπορείτε να χρησιμοποιήσετε [δειγματοληψία](app-insights-sampling.md) Εάν χρειάζεστε για να μειώσετε τον όγκο των τηλεμετρίας.


### <a name="durations"></a>Διάρκειες

Εκτός από το σημείο όπου αναφέρεται διαφορετικά, διάρκειες αντιπροσωπεύονται σε δέκατα ένα μικροδευτερόλεπτο, έτσι ώστε να 10000000.0 σημαίνει 1 δευτερόλεπτο.



## <a name="see-also"></a>Δείτε επίσης

* [Εφαρμογή ιδέες](app-insights-overview.md) 
* [Συνεχής εξαγωγής](app-insights-export-telemetry.md)
* [Δείγματα κώδικα](app-insights-export-telemetry.md#code-samples)


