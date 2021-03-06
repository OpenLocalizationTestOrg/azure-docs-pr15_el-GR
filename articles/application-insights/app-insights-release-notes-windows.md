<properties 
    pageTitle="Σημειώσεις έκδοσης για εφαρμογή ιδέες για Windows" 
    description="Τις πιο πρόσφατες ενημερώσεις για το Windows Store SDK." 
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
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Σημειώσεις έκδοσης για εφαρμογή ιδέες SDK για Windows Phone και αποθήκευση

SDK ιδέες για την εφαρμογή στέλνει τηλεμετρίας σχετικά με την εφαρμογή του live σε [Εφαρμογή ιδέες](https://azure.microsoft.com/services/application-insights/), όπου μπορείτε να αναλύσετε τη χρήση και τις επιδόσεις.


## <a name="version-111"></a>Έκδοση 1.1.1

### <a name="windows-sdk"></a>Windows SDK

- Διόρθωση να ανταποκρίνεται στη διάρκεια σφάλμα κατά τη χρήση του Windows Phone το Silverlight SDK. Μετά την αλλαγή, θα είναι σταθερές οποιαδήποτε σφάλμα που συμβαίνει αργότερα από ~ 2 δευτερόλεπτα μετά την κλήση σε WindowsAppInitialier.InitializeAsync(...) στο δίσκο και θα σταλεί την επόμενη φορά που γίνεται εκκίνηση της εφαρμογής. Εάν δεν συμβεί σφάλμα πριν ~ 2 δευτερόλεπτα μετά την κλήση, θα αγνοηθεί.  
- Ορίστε το NuGet εξαρτήσεις σε μια συγκεκριμένη έκδοση των πυρήνα και Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3).   

### <a name="core-sdk"></a>Πυρήνα SDK

- Μέγεθος των κύριων γίνεται στο github. Μπορείτε να βρείτε τις σημειώσεις μελλοντική έκδοση του SDK πυρήνα [στο github](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)

## <a name="version-11"></a>Έκδοση 1.1

### <a name="core-sdk"></a>Πυρήνα SDK

- SDK παρουσιάζει τώρα νέος τύπος τηλεμετρίας ```DependencyTelemetry``` που περιέχει πληροφορίες σχετικά με την κλήση εξάρτηση από την εφαρμογή
- Νέα μέθοδος ```TelemetryClient.TrackDependency``` σας επιτρέπει να στείλετε πληροφορίες σχετικά με τις κλήσεις εξάρτηση από την εφαρμογή

## <a name="version-100"></a>Έκδοση 1.0.0

### <a name="windows-app-sdk"></a>SDK εφαρμογής των Windows

- Νέα προετοιμασίας για τις εφαρμογές των Windows. Νέα `WindowsAppInitializer` τάξης με `InitializeAsync()` επιτρέπει τη μέθοδο για εκκίνηση η προετοιμασία του SDK συλλογής. Αυτή η αλλαγή επιτρέπουν πιο ακριβή έλεγχο και βελτιώσεις απόδοσης προετοιμασίας σημαντική εφαρμογή επάνω από την προηγούμενη τεχνική ApplicationInsights.config.
- Δεν είναι πλέον αυτόματα την τιμή DeveloperMode. Για να αλλάξετε τη συμπεριφορά DeveloperMode πρέπει να καθορίσετε στον κώδικα.
- Το πακέτο NuGet εισάγει δεν είναι πλέον ApplicationInsights.config. Συνιστάται να χρησιμοποιήσετε τη νέα WindowsAppInitializer κατά την προσθήκη με μη αυτόματο τρόπο το πακέτο NuGet.
- Διαβάζει μόνο ApplicationInsights.config `<InstrumentationKey>`, όλες οι άλλες ρυθμίσεις παραβλέπονται στις προτιμήσεις για τις ρυθμίσεις WindowsAppInitializer.
- Αγορά χώρου αποθήκευσης θα αυτόματης που συλλέγονται από SDK.
- Πολλές διορθώσεις σφαλμάτων, σταθερότητα και βελτιώσεις επιδόσεων.

### <a name="core-sdk"></a>Πυρήνα SDK

- Αρχείο ApplicationInsights.config δεν είναι πλέον requiered. Και δεν προστέθηκε από το πακέτο NuGet. Ρύθμιση παραμέτρων μπορεί να καθοριστεί πλήρως στον κώδικα.
- Πακέτο NuGet δεν είναι πλέον θα προσθέσει ένα αρχείο των προορισμών τη λύση σας. Αυτό καταργεί την αυτόματη ρύθμιση της DeveloperMode κατά τη διάρκεια μιας Δόμηση εντοπισμού σφαλμάτων. DeveloperMode πρέπει να οριστεί με μη αυτόματο τρόπο στον κώδικα.

## <a name="version-017"></a>Έκδοση 0.17

### <a name="windows-app-sdk"></a>SDK εφαρμογής των Windows

- SDK εφαρμογής Windows υποστηρίζει πλέον καθολικής Windows εφαρμογές που δημιουργήθηκαν σε σχέση με Windows 10 technical preview και με ΣΎΓΚΡΙΣΗ RC 2015.

### <a name="core-sdk"></a>Πυρήνα SDK

- TelemetryClient τις προεπιλεγμένες ρυθμίσεις για την προετοιμασία με το InMemoryChannel.
- Νέα API προσθέσει, `TelemetryClient.Flush()`. Αυτή η μέθοδος στοίχιση θα ενεργοποιήσει μια άμεση αποστολή αποκλεισμού της όλα τηλεμετρίας είστε συνδεδεμένοι σε αυτόν τον υπολογιστή-πελάτη. Αυτή η δυνατότητα επιτρέπει μη αυτόματη ενεργοποίηση αποστολής πριν τερματισμός της διεργασίας.
- Το πακέτο NuGet προσθέσει ο στόχος .net διαίρεσης 4,5. Αυτός ο προορισμός δεν έχει εξωτερικές εξαρτήσεις (καταργημένα εξαρτήσεις BCL και Προέλευση_συμβάντος).

## <a name="version-016"></a>Έκδοση 0,16 

Προεπισκόπηση 2015-04-28

- SDK υποστηρίζει πλέον DNX πλατφόρμα προορισμού για να ενεργοποιήσετε την παρακολούθηση των [πυρήνα .NET framework](http://www.dotnetfoundation.org/NETCore5) εφαρμογές.
- Παρουσίες ```TelemetryClient``` cache πλήκτρο οργάνων πλέον. Τώρα, αν κλειδί οργάνων δεν οριστεί ```TelemetryClient``` ρητά ```InstrumentationKey``` θα επιστρέψει τιμή null. Διορθώνει ένα ζήτημα κατά τη ρύθμιση ```TelemetryConfiguration.Active.InstrumentationKey``` αφού ορισμένες τηλεμετρίας ήδη έχουν συλλεχθεί, λειτουργικές μονάδες τηλεμετρίας όπως συλλογής εξάρτηση, web αιτήσεις συλλογής και επιδόσεων μετρητές συλλογής δεδομένων θα χρησιμοποιήσει νέο αριθμό-κλειδί οργάνων.

## <a name="version-015"></a>Έκδοση 0,15

- Νέα ιδιότητα ```Operation.SyntheticSource``` τώρα διαθέσιμα σε ```TelemetryContext```. Τώρα μπορείτε να επισημάνετε τα στοιχεία του τηλεμετρίας ως "δεν κυκλοφορία πραγματικού χρήστη" και καθορίστε πώς έχει δημιουργηθεί με αυτήν την κυκλοφορία. Ως παράδειγμα, ορίζοντας αυτήν την ιδιότητα, μπορείτε να διακρίνετε κίνηση από σας αυτοματισμού δοκιμής από την κυκλοφορία δοκιμής φόρτωση.
- Λογική καναλιού μετακινήθηκε για την ξεχωριστή NuGet που ονομάζεται Microsoft.ApplicationInsights.PersistenceChannel. Προεπιλεγμένο κανάλι τώρα ονομάζεται InMemoryChannel
- Νέα μέθοδος ```TelemetryClient.Flush``` επιτρέπει για να κάνετε εκκαθάριση τηλεμετρίας στοιχεία από το buffer σύγχρονη

## <a name="version-013"></a>Έκδοση 0,13

Δεν υπάρχει σημειώσεις έκδοσης για παλαιότερες εκδόσεις που είναι διαθέσιμες. 
