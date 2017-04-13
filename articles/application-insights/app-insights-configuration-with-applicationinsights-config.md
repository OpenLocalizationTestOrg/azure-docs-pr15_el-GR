<properties 
    pageTitle="Ρύθμιση παραμέτρων εφαρμογής ιδέες SDK με ApplicationInsights.config ή .xml" 
    description="Ενεργοποίηση ή απενεργοποίηση λειτουργικών μονάδων συλλογής δεδομένων και προσθήκη μετρητών επιδόσεων και άλλες παραμέτρους" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Ρύθμιση παραμέτρων του ιδέες εφαρμογή SDK με ApplicationInsights.config ή .xml

Η εφαρμογή ιδέες .NET SDK αποτελείται από έναν αριθμό NuGet πακέτων. Το [βασικό πακέτο](http://www.nuget.org/packages/Microsoft.ApplicationInsights) παρέχει το API για την αποστολή τηλεμετρίας για την εφαρμογή ιδέες. [Πρόσθετα πακέτα](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) παρέχουν τηλεμετρίας _λειτουργικές μονάδες_ και _προετοιμασίες_ για αυτόματη παρακολούθηση τηλεμετρίας από την εφαρμογή σας και του περιβάλλοντος του. Προσαρμόζοντας το αρχείο ρύθμισης παραμέτρων, μπορείτε να ενεργοποιήσετε ή να απενεργοποιήσετε τηλεμετρίας λειτουργικές μονάδες και προετοιμασίες και ρύθμιση παραμέτρων για ορισμένα από τα.

Το αρχείο ρύθμισης παραμέτρων ονομάζεται `ApplicationInsights.config` ή `ApplicationInsights.xml`, ανάλογα με τον τύπο της εφαρμογής σας. Προστίθεται αυτόματα στο έργο σας κατά την [εγκατάσταση του περισσότερες εκδόσεις του SDK][start]. Επίσης προστίθεται σε μια εφαρμογή web από [Την οθόνη κατάσταση διακομιστή των υπηρεσιών IIS][redfield], ή όταν επιλέγετε την ιδέες Appplication [επέκταση για μια τοποθεσία Web του Azure ή Εικονική](app-insights-azure-web-apps.md).

Δεν υπάρχει ένα ισοδύναμο αρχείο για να ελέγξετε το [SDK σε μια ιστοσελίδα][client].

Αυτό το έγγραφο περιγράφει τις ενότητες που βλέπετε στη ρύθμιση παραμέτρων του αρχείου, πώς μπορούν να ελέγχουν τα στοιχεία του SDK, και ποια πακέτα NuGet φόρτωση αυτά τα στοιχεία.

## <a name="telemetry-modules-aspnet"></a>Λειτουργικές μονάδες τηλεμετρίας (ASP.NET)

Κάθε λειτουργική μονάδα τηλεμετρίας συλλέγει ένα συγκεκριμένο τύπο δεδομένων και χρησιμοποιεί το μέγεθος των κύριων API για να στείλετε τα δεδομένα. Οι λειτουργικές μονάδες εγκαθίστανται από διαφορετικές NuGet πακέτων, τα οποία επίσης να προσθέσετε τις απαιτούμενες γραμμές στο αρχείο .config.

Υπάρχει ένας κόμβος στο αρχείο ρύθμισης παραμέτρων για κάθε λειτουργική μονάδα. Για να απενεργοποιήσετε μια λειτουργική μονάδα, διαγράψτε τον κόμβο ή σχόλιο την προβολή.



### <a name="dependency-tracking"></a>Εξάρτηση παρακολούθησης

[Εξάρτηση παρακολούθησης](app-insights-dependencies.md) συλλέγει τηλεμετρίας σχετικά με τις κλήσεις που κάνει την εφαρμογή σας σε βάσεις δεδομένων και εξωτερικές υπηρεσίες και βάσεων δεδομένων. Για να επιτρέπεται η ενότητα αυτή για να εργαστείτε σε ένα διακομιστή των υπηρεσιών IIS, πρέπει να [εγκαταστήσετε την οθόνη κατάσταση][redfield]. Για να το χρησιμοποιήσετε σε εφαρμογές Azure web ή ΣΠΣ, [Επιλέξτε την επέκταση εφαρμογής ιδέες](app-insights-azure-web-apps.md).

Μπορείτε επίσης να συντάξετε το δικό σας εξάρτηση παρακολούθησης κώδικα με χρήση του [TrackDependency API](app-insights-api-custom-events-metrics.md#track-dependency).


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* Το πακέτο NuGet [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) .

### <a name="performance-collector"></a>Απόδοση συλλογής

[Συλλέγει μετρητές επιδόσεων συστήματος](app-insights-performance-counters.md) όπως φορτίο CPU, μνήμης και δικτύου από εγκαταστάσεις των υπηρεσιών IIS. Μπορείτε να καθορίσετε ποια μετρητές για τη συλλογή, συμπεριλαμβανομένων των μετρητών επιδόσεων που έχετε ορίσει στον εαυτό σας.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* Το πακέτο NuGet [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) .


### <a name="application-insights-diagnostics-telemetry"></a>Εφαρμογή ιδέες Διαγνωστικά Τηλεμετρίας

Το `DiagnosticsTelemetryModule` αναφέρει σφάλματα στον κώδικα οργάνων ιδέες εφαρμογή ίδια. Για παράδειγμα, εάν ο κώδικας είναι δυνατή η πρόσβαση μετρητές επιδόσεων ή εάν μια `ITelemetryInitializer` δημιουργεί μια εξαίρεση. Ανίχνευση τηλεμετρίας παρακολουθείται από αυτήν τη λειτουργική μονάδα εμφανίζεται στο [Διαγνωστικών αναζήτησης][diagnostic]. Αποστέλλει διαγνωστικών δεδομένων dc.services.vsallin.net.
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* Το πακέτο NuGet [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) . Εάν εγκαθιστάτε μόνο αυτό το πακέτο, το αρχείο ApplicationInsights.config δεν δημιουργείται αυτόματα. 

### <a name="developer-mode"></a>Κατάσταση λειτουργίας προγραμματιστή

`DeveloperModeWithDebuggerAttachedTelemetryModule`επιβάλλει την εφαρμογή ιδέες `TelemetryChannel` για την αποστολή δεδομένων αμέσως, τηλεμετρίας ένα στοιχείο κάθε φορά, όταν ένα πρόγραμμα εντοπισμού σφαλμάτων είναι συνδεδεμένη με τη διαδικασία εφαρμογής. Αυτό μειώνει το χρονικό διάστημα από τη στιγμή που, όταν η εφαρμογή σας παρακολουθεί τις τηλεμετρίας και όταν εμφανιστεί στην πύλη του ιδέες εφαρμογής. Προκαλεί σημαντική επιβάρυνσης στο εύρος ζώνης δικτύου και CPU.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Εφαρμογή ιδέες Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) Το πακέτο NuGet

### <a name="web-request-tracking"></a>Αίτηση Web παρακολούθησης

Αναφορές του [χρόνου και αποτέλεσμα κωδικός απόκρισης](app-insights-asp-net.md) των αιτήσεων HTTP. 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* Το πακέτο NuGet [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)

### <a name="exception-tracking"></a>Εξαίρεση παρακολούθησης

`ExceptionTrackingTelemetryModule`κομμάτια ανεπίλυτη εξαιρέσεις στην εφαρμογή web. Ανατρέξτε στο θέμα [αποτυχίες και εξαιρέσεις][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* Το πακέτο NuGet [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web)


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-παρακολουθεί τις [Εξαιρέσεις μη τηρηθείσας εργασίας](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-παρακολουθεί ανεπίλυτη εξαιρέσεις για ρόλους εργασίας, τις υπηρεσίες windows και κονσόλας εφαρμογές.
* [Εφαρμογή ιδέες Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) Το πακέτο NuGet.

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

Το πακέτο Microsoft.ApplicationInsights παρέχει [πυρήνα API](https://msdn.microsoft.com/library/mt420197.aspx) του SDK. Οι άλλες λειτουργικές μονάδες τηλεμετρίας Χρησιμοποιήστε αυτήν την επιλογή και μπορείτε επίσης να [το χρησιμοποιήσετε για να ορίσετε τις δικές σας τηλεμετρίας](app-insights-api-custom-events-metrics.md).

* Δεν υπάρχει καταχώρηση στο ApplicationInsights.config.
* Το πακέτο NuGet [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) . Εάν εγκαταστήσετε ακριβώς αυτό NuGet, δημιουργείται χωρίς αρχείο .config.

## <a name="telemetry-channel"></a>Τηλεμετρίας καναλιού

Το κανάλι τηλεμετρίας διαχειρίζεται προσωρινής και μετάδοση των τηλεμετρίας για την υπηρεσία εφαρμογής ιδέες. 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`είναι το προεπιλεγμένο κανάλι για τις υπηρεσίες. Το buffer για δεδομένα στη μνήμη.
* `Microsoft.ApplicationInsights.PersistenceChannel`είναι μια εναλλακτική λύση για εφαρμογές κονσόλας. Το να αποθηκεύσετε unflushed δεδομένα με το χώρο αποθήκευσης μόνιμη όταν η εφαρμογή κλείνει προς τα κάτω και θα στείλετε όταν ξεκινά ξανά την εφαρμογή.


## <a name="telemetry-initializers-aspnet"></a>Προετοιμασίες τηλεμετρίας (ASP.NET)

Προετοιμασίες τηλεμετρίας Ορισμός ιδιοτήτων περιβάλλοντος που αποστέλλονται μαζί με κάθε στοιχείο της τηλεμετρίας. 

Μπορείτε να [συντάξετε το δικό σας προετοιμασίες](app-insights-api-filtering-sampling.md#add-properties) για να ορίσετε ιδιότητες περιβάλλοντος.

Η τυπική προετοιμασίες ρυθμίζονται είτε με τα πακέτα Web ή WindowsServer NuGet:


* `AccountIdTelemetryInitializer`Ορίζει την ιδιότητα AccountId.
* `AuthenticatedUserIdTelemetryInitializer`Ορίζει την ιδιότητα AuthenticatedUserId όπως έχει οριστεί από το SDK JavaScript.
* `AzureRoleEnvironmentTelemetryInitializer`ενημερώσεις το `RoleName` και `RoleInstance` ιδιοτήτων του `Device` περιβάλλοντος για όλα τα στοιχεία τηλεμετρίας με τις πληροφορίες που έχουν εξαχθεί από το περιβάλλον Azure χρόνου εκτέλεσης.
* `BuildInfoConfigComponentVersionTelemetryInitializer`ενημερώσεις το `Version` ιδιότητα το `Component` περιβάλλοντος για όλα τα στοιχεία τηλεμετρίας με την τιμή που έχουν εξαχθεί από το `BuildInfo.config` αρχείο παράγονται από Δόμηση MS.
* `ClientIpHeaderTelemetryInitializer`ενημερώσεις `Ip` ιδιότητα του το `Location` περιβάλλοντος όλων των στοιχείων τηλεμετρίας με βάση το `X-Forwarded-For` κεφαλίδα HTTP της αίτησης.
* `DeviceTelemetryInitializer`ενημερώνει τις ακόλουθες ιδιότητες του `Device` περιβάλλοντος για όλα τα στοιχεία τηλεμετρίας.
 - `Type`έχει οριστεί σε "PC"
 - `Id`έχει οριστεί για το όνομα τομέα του υπολογιστή όπου εκτελείται η εφαρμογή web.
 - `OemName`έχει οριστεί στην τιμή που έχουν εξαχθεί από το `Win32_ComputerSystem.Manufacturer` πεδίων με χρήση WMI.
 - `Model`έχει οριστεί στην τιμή που έχουν εξαχθεί από το `Win32_ComputerSystem.Model` πεδίων με χρήση WMI.
 - `NetworkType`έχει οριστεί στην τιμή που έχουν εξαχθεί από το `NetworkInterface`.
 - `Language`έχει οριστεί στο όνομα του `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`ενημερώσεις του `RoleInstance` ιδιότητα το `Device` περιβάλλοντος για όλα τα στοιχεία τηλεμετρίας με το όνομα τομέα του υπολογιστή όπου εκτελείται η εφαρμογή web.
* `OperationNameTelemetryInitializer`ενημερώσεις του `Name` ιδιότητα το `RequestTelemetry` και το `Name` ιδιότητα του το `Operation` περιβάλλοντος όλων των στοιχείων τηλεμετρίας με βάση τη μέθοδο HTTP, καθώς και ονόματα ελεγκτή ASP.NET MVC και ενέργεια καλείται για να επεξεργαστεί την αίτηση.
* `OperationIdTelemetryInitializer`ή `OperationCorrelationTelemetryInitializer` ενημερώσεις του `Operation.Id` ιδιότητα περιβάλλοντος όλων των στοιχείων τηλεμετρίας παρακολουθούνται κατά το χειρισμό μια αίτηση με την αυτόματη δημιουργία `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`ενημερώσεις του `Id` ιδιότητα το `Session` περιβάλλοντος για όλα τα στοιχεία τηλεμετρίας με τιμή που έχουν εξαχθεί από το `ai_session` cookie που δημιουργούνται από τον κώδικα οργάνων ApplicationInsights JavaScript που εκτελείται στο πρόγραμμα περιήγησης του χρήστη. 
* `SyntheticTelemetryInitializer`ή `SyntheticUserAgentTelemetryInitializer` ενημερώσεις του `User`, `Session` και `Operation` ιδιότητες περιβάλλοντα όλων των στοιχείων τηλεμετρίας παρακολουθούνται κατά την επεξεργασία της αίτησης από μια προέλευση σύνθετων, όπως διαθεσιμότητα δοκιμή ή αναζήτηση bot μηχανισμός. Από προεπιλογή, [Μετρικά Explorer](app-insights-metrics-explorer.md) δεν εμφανίζει σύνθετων τηλεμετρίας. 

    Το `<Filters>` ορίστε τον εντοπισμό ιδιότητες των αιτήσεων.
* `UserAgentTelemetryInitializer`ενημερώσεις το `UserAgent` ιδιότητα το `User` περιβάλλοντος όλων των στοιχείων τηλεμετρίας με βάση το `User-Agent` κεφαλίδα HTTP της πρόσκλησης σε.
* `UserTelemetryInitializer`ενημερώσεις του `Id` και `AcquisitionDate` ιδιότητες του `User` περιβάλλοντος για όλα τα στοιχεία τηλεμετρίας με τιμές που έχουν εξαχθεί από το `ai_user` cookie που δημιουργούνται από τον κώδικα οργάνων εφαρμογή ιδέες JavaScript που εκτελείται στο πρόγραμμα περιήγησης του χρήστη.
* `WebTestTelemetryInitializer`Ορίζει το αναγνωριστικό χρήστη, αναγνωριστικό περιόδου λειτουργίας και ιδιότητες σύνθετων προέλευσης για αιτήσεις HTTP που προέρχονται από [δοκιμές διαθεσιμότητα](app-insights-monitor-web-app-availability.md).
Το `<Filters>` ορίστε τον εντοπισμό ιδιότητες των αιτήσεων.

## <a name="telemetry-processors-aspnet"></a>Επεξεργαστές τηλεμετρίας (ASP.NET)

Επεξεργαστές τηλεμετρίας να φιλτράρετε και να τροποποιήσετε κάθε στοιχείο τηλεμετρίας ακριβώς πριν την αποστολή από το SDK στην πύλη του.

Μπορείτε να [συντάξετε το δικό σας επεξεργαστές τηλεμετρίας](app-insights-api-filtering-sampling.md#filtering).


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Επεξεργαστής τηλεμετρίας προσαρμόσιμη δειγματοληψία (από 2.0.0-beta3)

Αυτή είναι ενεργοποιημένη από προεπιλογή. Αν την εφαρμογή σας στείλει πολλές τηλεμετρίας, καταργεί τον επεξεργαστή κάποιο από αυτό.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Η παράμετρος παρέχει ο προορισμός που ο αλγόριθμος προσπαθεί να επιτύχετε. Κάθε παρουσία του SDK λειτουργεί ανεξάρτητα από αυτό, ώστε να εάν ο διακομιστής είναι ένα σύμπλεγμα πολλά μηχανήματα, τον πραγματικό όγκο τηλεμετρίας θα πολλαπλασιάζεται αντίστοιχα.

[Μάθετε περισσότερα σχετικά με τη δειγματοληψία](app-insights-sampling.md).



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Επεξεργαστής τηλεμετρίας δειγματοληψία σταθερού επιτοκίου (από 2.0.0-beta1)

Υπάρχει επίσης μια τυπική [δειγματοληψία τηλεμετρίας επεξεργαστής](app-insights-api-filtering-sampling.md#sampling) (από 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Παράμετροι καναλιού (Java)

Αυτές οι παράμετροι επηρεάζουν τον τρόπο στο SDK Java θα πρέπει να αποθηκεύσετε Εκκαθαρίστε τα δεδομένα τηλεμετρίας που που συλλέγει.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

Ο αριθμός των στοιχείων τηλεμετρίας που μπορούν να αποθηκευτούν στο χώρο αποθήκευσης στη μνήμη του SDK. Όταν φτάσετε σε αυτό τον αριθμό, εκκαθάριση του buffer τηλεμετρίας - δηλαδή, στην οποία αποστέλλονται τα στοιχεία τηλεμετρίας στο διακομιστή εφαρμογής ιδέες.

-   Ελάχιστο: 1
-   Max: 1000
-   Προεπιλεγμένη: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

Καθορίζει πόσο συχνά τα δεδομένα που είναι αποθηκευμένα στο το χώρο αποθήκευσης στη μνήμη θα πρέπει η εκκαθάριση (απεσταλμένων σε εφαρμογή ιδέες).

-   Ελάχιστο: 1
-   Max: 300
-   Προεπιλεγμένη: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

Καθορίζει το μέγιστο μέγεθος σε MB που εκχωρείται με τη μόνιμη αποθήκευση στον τοπικό δίσκο. Αυτός ο χώρος αποθήκευσης χρησιμοποιείται για μόνιμη στοιχεία τηλεμετρίας που απέτυχε να μεταδοθούν στο τελικό σημείο εφαρμογής ιδέες. Όταν ικανοποιείται το μέγεθος του χώρου αποθήκευσης, θα απορρίπτονται νέα στοιχεία τηλεμετρίας.

-   Ελάχιστο: 1
-   Max: 100
-   Προεπιλεγμένη: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

Καθορίζει τον πόρο ιδέες εφαρμογής στην οποία εμφανίζεται τα δεδομένα σας. Συνήθως δημιουργείτε έναν νέο πόρο, με ένα νέο αριθμό-κλειδί, για κάθε μία από τις εφαρμογές σας.

Εάν θέλετε να ορίσετε τον αριθμό-κλειδί δυναμικά - για παράδειγμα, εάν θέλετε να στείλετε τα αποτελέσματα από την εφαρμογή σας σε διαφορετικούς πόρους - μπορείτε να παραλείψετε το κλειδί από το αρχείο ρύθμισης παραμέτρων και ορισμός στον κώδικα.

Για να ορίσετε τον αριθμό-κλειδί για όλες τις εμφανίσεις της TelemetryClient, όπως τυπική τηλεμετρίας λειτουργικές μονάδες, ορίστε τον αριθμό-κλειδί στο TelemetryConfiguration.Active. Κάντε τα εξής σε μια μέθοδο προετοιμασίας, όπως global.aspx.cs σε μια υπηρεσία ASP.NET:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Εάν θέλετε απλώς να στείλετε ένα συγκεκριμένο σύνολο συμβάντα σε έναν άλλο πόρο, μπορείτε να ορίσετε τον αριθμό-κλειδί για ένα συγκεκριμένο TelemetryClient:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[Μάθετε περισσότερα σχετικά με το API][api].

Για να λάβετε νέας κλειδιού, [Δημιουργήστε ένα νέο πόρο στην πύλη εφαρμογής ιδέες][new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

