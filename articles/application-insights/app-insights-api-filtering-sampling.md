<properties 
    pageTitle="Φιλτράρισμα και προ-επεξεργασία αρχείου στο SDK ιδέες για την εφαρμογή | Microsoft Azure" 
    description="Γράψτε επεξεργαστές Τηλεμετρίας και Τηλεμετρίας προετοιμασίες για το SDK για το φιλτράρισμα ή προσθήκη ιδιοτήτων για τα δεδομένα πριν από την τηλεμετρίας αποστολή στην πύλη εφαρμογής ιδέες." 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Φιλτράρισμα και προ-επεξεργασία αρχείου τηλεμετρίας στο SDK ιδέες για την εφαρμογή

*Εφαρμογή ιδέες είναι σε προεπισκόπηση.*

Για να γράψετε και ρύθμιση παραμέτρων προσθήκες για την εφαρμογή SDK ιδέες για να προσαρμόσετε τον τρόπο τηλεμετρίας είναι και υποβάλλονται σε επεξεργασία πριν από την αποστολή στην υπηρεσία εφαρμογής ιδέες. 

Προς το παρόν αυτές οι δυνατότητες είναι διαθέσιμες για το SDK ASP.NET.

* [Δειγματοληψία](app-insights-sampling.md) μειώνει τον όγκο των τηλεμετρίας χωρίς να επηρεαστούν τα στατιστικά στοιχεία. Διατηρεί μαζί που σχετίζονται με σημεία δεδομένων, ώστε να μπορείτε να περιηγηθείτε μεταξύ τους κατά τη διάγνωση πρόβλημα. Στην πύλη, το συνολικό πλήθος πολλαπλασιάζεται να αποζημιώσει για τη δειγματοληψία.
* [Φιλτράρισμα με επεξεργαστές Τηλεμετρίας](#filtering) σάς επιτρέπει να επιλέξετε ή να τροποποιήσετε τηλεμετρίας στο SDK πριν από την αποστολή στο διακομιστή. Για παράδειγμα, ενδέχεται να μπορείτε να μειώσετε τον όγκο των τηλεμετρίας εξαιρώντας αιτήσεις από τα ρομπότ. Αλλά φιλτράρισμα είναι μια πιο βασικές προσέγγιση για τη μείωση της κίνησης από δειγματοληψία. Σας επιτρέπει μεγαλύτερο έλεγχο τι μεταδίδεται, αλλά θα πρέπει να έχετε υπόψη ότι επηρεάζει τα στατιστικά στοιχεία - για παράδειγμα, εάν αποκλείσετε όλες τις αιτήσεις επιτυχής.
* [Τηλεμετρίας προετοιμασίες Προσθήκη ιδιοτήτων](#add-properties) σε οποιαδήποτε τηλεμετρίας αποστέλλεται από την εφαρμογή σας, συμπεριλαμβανομένων των τηλεμετρίας από τις βασικές λειτουργικές μονάδες. Για παράδειγμα, θα μπορούσατε να προσθέσετε υπολογιζόμενες τιμές. ή αριθμούς έκδοσης από την οποία θέλετε να φιλτράρετε τα δεδομένα στην πύλη του.
* [Το API SDK](app-insights-api-custom-events-metrics.md) χρησιμοποιείται για την αποστολή προσαρμοσμένων συμβάντων και μετρήσεις.


Πριν να ξεκινήσετε:

* Εγκαταστήστε την [εφαρμογή ιδέες SDK για ASP.NET v2](app-insights-asp-net.md) στην εφαρμογή. 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>Φιλτράρισμα: ITelemetryProcessor

Αυτή η τεχνική παρέχει πιο άμεση έλεγχο τι είναι συμπεριλαμβάνονται ή εξαιρούνται από τη ροή τηλεμετρίας. Μπορείτε να το χρησιμοποιήσετε σε συνδυασμό με δειγματοληψία, ή ξεχωριστά.

Για να φιλτράρετε τηλεμετρίας, μπορείτε να συντάξετε ένα πρόγραμμα επεξεργασίας τηλεμετρίας και καταχωρήσετε με το SDK. Όλα τηλεμετρίας, έχετε επεξεργαστή σας και μπορείτε να επιλέξετε να αποθέστε το από τη ροή ή να προσθέσετε ιδιότητες. Περιλαμβάνονται και οι τηλεμετρίας από τις βασικές λειτουργικές μονάδες όπως της συλλογής αίτηση HTTP και την εξάρτηση συλλογής, καθώς και τηλεμετρίας που έχετε γράψει μόνοι σας. Για παράδειγμα, μπορείτε να, φιλτράρετε ανάληψη τηλεμετρίας σχετικά με τις αιτήσεις από τα ρομπότ ή εξαρτήσεων επιτυχής κλήσεις. 

> [AZURE.WARNING] Φιλτράρισμα του τηλεμετρίας αποστέλλονται από το SDK χρησιμοποιώντας επεξεργαστές να skew τα στατιστικά στοιχεία που βλέπετε στην πύλη και να είναι δύσκολο να παρακολουθήσετε σχετικά στοιχεία.
> 
> Αντί για αυτό, μπορείτε να χρησιμοποιήσετε [δειγματοληψία](app-insights-sampling.md).

### <a name="create-a-telemetry-processor"></a>Δημιουργήστε ένα πρόγραμμα επεξεργασίας τηλεμετρίας

1. Βεβαιωθείτε ότι η εφαρμογή SDK ιδέες στο έργο σας είναι έκδοση 2.0.0 ή νεότερη έκδοση. Κάντε δεξιό κλικ στο έργο σας στην Εξερεύνηση λύσεων Visual Studio και επιλέξτε Διαχείριση πακέτων NuGet. Στη Διαχείριση πακέτου NuGet, επιλέξτε Microsoft.ApplicationInsights.Web.

1. Για να δημιουργήσετε ένα φίλτρο, υλοποιήστε ITelemetryProcessor. Αυτό είναι ένα άλλο σημείο επεκτασιμότητα του όπως λειτουργική μονάδα τηλεμετρίας προετοιμασία τηλεμετρίας και τηλεμετρίας καναλιού. 

    Παρατηρήστε ότι Τηλεμετρίας επεξεργαστών δημιουργήσετε μια αλυσίδα επεξεργασίας. Όταν δημιουργείτε ένα πρόγραμμα επεξεργασίας τηλεμετρίας, μπορείτε να μεταβιβάσετε μια σύνδεση στον επόμενο επεξεργαστή στην αλυσίδα. Όταν ένα σημείο δεδομένων τηλεμετρίας μεταβιβάζεται στη μέθοδο της διαδικασίας, κάνει τις εργασίες και, στη συνέχεια, καλεί το επόμενο επεξεργαστής Τηλεμετρίας στην αλυσίδα.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. Εισαγάγετε αυτό στο ApplicationInsights.config: 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Αυτό είναι το ίδιο τμήμα όπου θα προετοιμάσετε ένα φίλτρο δειγματοληψία.)

Μπορείτε να μεταβιβάσετε τιμές συμβολοσειρών από το αρχείο .config, παρέχοντας δημόσιες ιδιότητες επώνυμο στην τάξη σας. 

> [AZURE.WARNING] Να χειριστείτε ώστε να ταιριάζει με το όνομα του τύπου και οποιαδήποτε ονόματα ιδιοτήτων στο αρχείο .config στα ονόματα κλάσης και ιδιοτήτων στον κώδικα. Εάν το αρχείο .config αναφέρεται σε έναν τύπο δεν υπάρχει ή την ιδιότητα, το SDK σιωπηρά ενδέχεται να αποτύχει για να στείλετε οποιαδήποτε τηλεμετρίας.

 
**Εναλλακτικά,** μπορέσετε να προετοιμάσετε το φίλτρο στον κώδικα. Σε μια κατάλληλη προετοιμασίας κλάση - για παράδειγμα AppStart στο Global.asax.cs - εισαγωγή επεξεργαστή σας σε η αλυσίδα:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients που έχουν δημιουργηθεί μετά από αυτό το σημείο θα χρησιμοποιήσει επεξεργαστών σας.

### <a name="example-filters"></a>Παράδειγμα φίλτρα

#### <a name="synthetic-requests"></a>Σύνθετων αιτήσεις

Φιλτράρετε τα BOT και web δοκιμές. Παρόλο που μετρικά Explorer σάς δίνει την επιλογή για να φιλτράρεται σύνθετων προελεύσεις, αυτή η επιλογή μειώνει την κυκλοφορία φιλτράροντας τους στο SDK.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Αποτυχία ελέγχου ταυτότητας

Φιλτράρετε τα αιτήματα με μια απάντηση "401". 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Φιλτράρει τις κλήσεις γρήγορα απομακρυσμένο εξάρτησης

Εάν θέλετε μόνο να διάγνωση κλήσεις που είναι αργή, να φιλτράρετε γρήγορα αυτά. 

> [AZURE.NOTE] Αυτό θα skew τα στατιστικά στοιχεία που βλέπετε στην πύλη του. Το γράφημα εξάρτησης θα φαίνεται σαν οι κλήσεις εξάρτηση είναι όλες τις αποτυχίες.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Διάγνωση θεμάτων εξάρτησης

[Αυτό το ιστολόγιο](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) περιγράφει ένα έργο για τη διάγνωση θεμάτων εξάρτηση με την αυτόματη αποστολή ping που κανονική με τις εξαρτήσεις.


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>Προσθήκη ιδιοτήτων: ITelemetryInitializer

Χρησιμοποιήστε προετοιμασίες τηλεμετρίας για να ορίσετε καθολικές ιδιότητες που αποστέλλονται με όλους τηλεμετρίας; και για να παρακάμψετε επιλεγμένο συμπεριφορά τυπική τηλεμετρίας λειτουργικές μονάδες. 

Για παράδειγμα, η εφαρμογή ιδέες για το πακέτο Web συλλέγει τηλεμετρίας σχετικά με τις αιτήσεις HTTP. Από προεπιλογή, το επισημαίνει καθώς απέτυχε οποιαδήποτε αίτηση με έναν κωδικό απόκριση > = 400. Αλλά αν θέλετε να χειριστείτε 400 ως επιτυχίας, μπορείτε να δώσετε ένα σύνολο αρχικών τιμών τηλεμετρίας που ορίζει την ιδιότητα επιτυχίας.

Εάν παρέχεται ένα σύνολο αρχικών τιμών τηλεμετρίας, ονομάζεται κάθε φορά που ονομάζεται οποιαδήποτε από τις μεθόδους Track*(). Περιλαμβάνονται και οι μέθοδοι που ονομάζεται από την τυπική τηλεμετρίας λειτουργικές μονάδες. Κατά συνθήκη, αυτές οι λειτουργικές μονάδες δεν ορίσετε οποιαδήποτε ιδιότητα που έχει ήδη ρυθμιστεί από ένα σύνολο αρχικών τιμών. 

**Ορισμός του προετοιμασίας**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Φόρτωση του προετοιμασίας**

Στο ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Εναλλακτικά,* μπορεί να εμφανίσει την προετοιμασία στον κώδικα, για παράδειγμα στο Global.aspx.cs:


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Δείτε περισσότερα στοιχεία αυτού του δείγματος.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>Προετοιμασίες τηλεμετρίας JavaScript

*JavaScript*

Εισαγάγετε ένα σύνολο αρχικών τιμών τηλεμετρίας αμέσως μετά τον κωδικό προετοιμασίας που λάβατε από την πύλη: 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Για μια σύνοψη των τα διαθέσιμα σε το telemetryItem μη προσαρμοσμένες ιδιότητες, ανατρέξτε στο θέμα το [μοντέλο δεδομένων](app-insights-export-data-model.md#lttelemetrytypegt).

Μπορείτε να προσθέσετε όσες προετοιμασίες όπως θέλετε. 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor και ITelemetryInitializer

Ποια είναι η διαφορά μεταξύ των επεξεργαστών τηλεμετρίας και τηλεμετρίας προετοιμασίες;

* Υπάρχουν ορισμένα επικαλύψεις σε τι μπορείτε να κάνετε με αυτά: και τα δύο μπορούν να χρησιμοποιηθούν για την προσθήκη ιδιοτήτων σε τηλεμετρίας.
* Να εκτελείτε πάντα πριν από την TelemetryProcessors TelemetryInitializers.
* TelemetryProcessors σάς επιτρέπουν να αντικαταστήσουν ή να απορρίψετε ένα στοιχείο τηλεμετρίας πλήρως.
* TelemetryProcessors δεν επεξεργασία τηλεμετρίας μετρητή επιδόσεων.



## <a name="persistence-channel"></a>Διατήρηση καναλιού 

Εάν την εφαρμογή σας εκτελείται όπου η σύνδεση στο internet δεν είναι πάντα διαθέσιμη ή αργή, μπορείτε να χρησιμοποιήσετε το κανάλι διατήρησης αντί για το κανάλι στη μνήμη προεπιλογή. 

Το προεπιλεγμένο κανάλι στη μνήμη, χάνονται οποιαδήποτε τηλεμετρίας που δεν έχει σταλεί από το χρόνο κλείνει την εφαρμογή. Παρόλο που μπορείτε να χρησιμοποιήσετε `Flush()` να προσπαθούν να στείλουν δεδομένα υπόλοιπη στο buffer, εξακολουθεί να χάνει δεδομένων εάν υπάρχει σύνδεση στο internet ή εάν η εφαρμογή τερματιστεί πριν από την μετάδοση έχει ολοκληρωθεί.

Αντίθετα, το κανάλι διατήρησης buffer για τους τηλεμετρίας σε ένα αρχείο, πριν να το στείλετε στην πύλη του. `Flush()`εξασφαλίζει ότι τα δεδομένα αποθηκεύονται στο αρχείο. Εάν οποιαδήποτε δεδομένα δεν αποστέλλονται από το χρόνο κλείνει την εφαρμογή, παραμένει στο αρχείο. Όταν γίνει επανεκκίνηση της εφαρμογής, τα δεδομένα θα αποσταλούν, στη συνέχεια, εάν υπάρχει σύνδεση στο internet. Συγκεντρώνει δεδομένα στο αρχείο για το χρονικό διάστημα που απαιτείται μέχρι να είναι διαθέσιμη μια σύνδεση. 

### <a name="to-use-the-persistence-channel"></a>Για να χρησιμοποιήσετε το κανάλι διατήρησης

1. Εισαγωγή του πακέτου NuGet [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3).
2. Συμπεριλάβετε αυτόν τον κωδικό στην εφαρμογή σας, σε μια θέση κατάλληλη προετοιμασίας:
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Χρήση `telemetryClient.Flush()` πριν από το κλείνει εφαρμογής, για να βεβαιωθείτε ότι δεδομένα είτε αποστέλλεται στην πύλη ή αποθηκεύσατε το αρχείο.

    Σημειώστε ότι Flush() είναι σύγχρονη για το κανάλι διατήρησης, αλλά ασύγχρονης για άλλα κανάλια.

 
Το κανάλι διατήρησης είναι βελτιστοποιημένη για συσκευές σενάρια, όπου ο αριθμός των συμβάντων που παράγονται από εφαρμογή είναι σχετικά μικρό και η σύνδεση είναι συχνά αναξιόπιστες. Αυτό το κανάλι θα γράψετε συμβάντα στο δίσκο σε αξιόπιστη αποθήκευσης πρώτα και, στη συνέχεια, προσπαθήστε να το στείλετε. 

#### <a name="example"></a>Παράδειγμα

Ας υποθέσουμε ότι θέλετε να παρακολουθήσετε ανεπίλυτη εξαιρέσεις. Έχετε εγγραφεί το `UnhandledException` συμβάν. Στην επιστροφή κλήσης, μπορείτε να συμπεριλάβετε μια κλήση σε στοίχιση για να βεβαιωθείτε ότι το τηλεμετρίας έχει διατηρηθεί.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Όταν η εφαρμογή τερματίζεται, θα δείτε ένα αρχείο σε `%LocalAppData%\Microsoft\ApplicationInsights\`, που περιέχει τα συμπιεσμένα συμβάντα. 
 
Επόμενη φορά που θα ξεκινήσετε αυτής της εφαρμογής, θα σηκώστε αυτού του αρχείου και παράδοση τηλεμετρίας για την εφαρμογή ιδέες, εάν είναι δυνατό το κανάλι.

#### <a name="test-example"></a>Παράδειγμα δοκιμαστική

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


Ο κωδικός του καναλιού διατήρησης είναι σε [github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel). 


## <a name="reference-docs"></a>Αναφορά έγγραφα

* [Επισκόπηση API](app-insights-api-custom-events-metrics.md)

* [Αναφορά ASP.NET](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>Κωδικός SDK

* [ASP.NET πυρήνα SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [5 ASP.NET](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="next"></a>Επόμενα βήματα


* [Αναζήτηση συμβάντων και αρχείων καταγραφής][diagnostic]
* [Δειγματοληψία](app-insights-sampling.md)
* [Αντιμετώπιση προβλημάτων][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 
