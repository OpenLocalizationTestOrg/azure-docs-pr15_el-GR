<properties 
    pageTitle="Παρακολούθηση χρήσης και επιδόσεων για τις εφαρμογές στην επιφάνεια εργασίας των Windows" 
    description="Ανάλυση χρήσης και επιδόσεων της εφαρμογής υπολογιστή Windows με HockeyApp και τις ιδέες εφαρμογής." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Παρακολούθηση χρήσης και επιδόσεων σε εφαρμογές της επιφάνειας εργασίας των Windows

*Εφαρμογή ιδέες είναι σε προεπισκόπηση.*

[Ιδέες εφαρμογή του Visual Studio](app-insights-overview.md) και [HockeyApp](https://hockeyapp.net) σάς επιτρέπουν να παρακολουθείτε ανεπτυγμένος εφαρμογή σας για χρήση και τις επιδόσεις.

> [AZURE.IMPORTANT] Συνιστάται να [HockeyApp](https://hockeyapp.net) για τη διανομή και παρακολούθηση εφαρμογών επιφάνειας εργασίας και των συσκευών. Με HockeyApp, που μπορούν να διαχειρίζονται διανομής, ζωντανή δοκιμές και σχολίων χρήστη, καθώς και να παρακολούθηση αναφορών χρήσης και σφάλμα. Μπορείτε επίσης να [εξαγάγετε και ερωτήματος σας τηλεμετρίας με ανάλυση](app-insights-hockeyapp-bridge-app.md).

> Παρόλο που τηλεμετρίας μπορούν να σταλούν στον ιδέες εφαρμογής από μια εφαρμογή υπολογιστή, αυτή είναι κυρίως χρήσιμη για σκοπούς εντοπισμού σφαλμάτων και πειραματικής.


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Για να στείλετε τηλεμετρίας σε ιδέες εφαρμογής από μια εφαρμογή των Windows

1. Στην [πύλη του Azure](https://portal.azure.com), [Δημιουργήστε έναν πόρο εφαρμογής ιδέες](app-insights-create-new-resource.md). Για τον τύπο εφαρμογής, επιλέξτε εφαρμογή ASP.NET.
2. Λήψη αντιγράφου του αριθμού-κλειδιού οργάνων. Βρείτε τον αριθμό-κλειδί στο Essentials αναπτυσσόμενο μενού του νέου πόρου που μόλις δημιουργήσατε. 
3. Στο Visual Studio, επεξεργαστείτε τα πακέτα NuGet του έργου σας εφαρμογή και προσθέστε Microsoft.ApplicationInsights.WindowsServer. (Ή επιλέξτε Microsoft.ApplicationInsights εάν θέλετε απλώς τα κενά API, χωρίς να τις λειτουργικές μονάδες συλλογής τυπική τηλεμετρίας.)
4. Ορίστε τον αριθμό-κλειδί οργάνων είτε στον κώδικά σας:

    `TelemetryConfiguration.Active.InstrumentationKey = "`*τον αριθμό-κλειδί*`";` 

    ή σε ApplicationInsights.config (Εάν έχετε εγκαταστήσει ένα από τα πακέτα τυπική τηλεμετρίας):
 
    `<InstrumentationKey>`*τον αριθμό-κλειδί*`</InstrumentationKey>` 

    Εάν χρησιμοποιείτε ApplicationInsights.config, βεβαιωθείτε ότι τις ιδιότητές του Εξερεύνηση λύσεων που έχουν οριστεί **Δόμηση ενέργειας = περιεχόμενο, αντιγραφή κατάλογο εξόδου = αντίγραφο**.
5. [Χρήση του API](app-insights-api-custom-events-metrics.md) για να στείλετε τηλεμετρίας.
6. Εκτελέστε την εφαρμογή σας και δείτε τα τηλεμετρίας στον πόρο που δημιουργήσατε στην πύλη του Azure.

## <a name="telemetry"></a>Παράδειγμα κώδικα

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Επόμενα βήματα

* [Δημιουργία ενός πίνακα εργαλείων](app-insights-dashboards.md)
* [Διαγνωστικών αναζήτησης](app-insights-diagnostic-search.md)
* [Εξερεύνηση μετρικά](app-insights-metrics-explorer.md)
* [Γράψτε ανάλυσης ερωτήματα](app-insights-analytics.md)
 
