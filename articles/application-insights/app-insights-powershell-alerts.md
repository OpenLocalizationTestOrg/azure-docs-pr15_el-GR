<properties
    pageTitle="Χρήση του Powershell για να ορίσετε ειδοποιήσεις της εφαρμογής ιδέες"
    description="Αυτοματοποίηση ρύθμιση παραμέτρων για τις ιδέες εφαρμογής για να λάβετε μηνύματα ηλεκτρονικού ταχυδρομείου σχετικά με τις αλλαγές μετρικό."
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
    ms.date="02/19/2016"
    ms.author="awills"/>

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Χρήση του PowerShell για να ορίσετε ειδοποιήσεις της εφαρμογής ιδέες

Μπορείτε να αυτοματοποιήσετε τη ρύθμιση παραμέτρων για τις [ειδοποιήσεις](app-insights-alerts.md) στο [Visual Studio εφαρμογή ιδέες](app-insights-overview.md).

Επιπλέον, μπορείτε να [ορίσετε webhooks για την αυτοματοποίηση την απάντησή σας σε μια ειδοποίηση](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="one-time-setup"></a>Μοναδική εγκατάσταση

Εάν δεν έχετε χρησιμοποιήσει PowerShell με πριν από τη συνδρομή σας Azure:

Εγκαταστήστε τη λειτουργική μονάδα Azure Powershell στον υπολογιστή όπου θέλετε να εκτελέσετε τις δέσμες ενεργειών.

 * Εγκατάσταση [Microsoft Web πλατφόρμα Installer (v5 ή νεότερη έκδοση)](http://www.microsoft.com/web/downloads/platform.aspx).
 * Χρησιμοποιήστε το για να εγκαταστήσετε το Microsoft Azure Powershell


## <a name="connect-to-azure"></a>Σύνδεση με Azure

Ξεκινήστε Azure PowerShell και να [συνδεθείτε με τη συνδρομή σας](../powershell-install-configure.md):

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>Λήψη ειδοποιήσεων

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Προσθήκη ειδοποίησης


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>Παράδειγμα 1

Ηλεκτρονικού ταχυδρομείου μου, εάν ο διακομιστής απόκριση σε προσκλήσεις HTTP, μέσος όρος πάνω από 5 λεπτά, είναι μικρότερη από 1 δευτερόλεπτο. Μου πόρων ιδέες εφαρμογή ονομάζεται IceCreamWebApp και βρίσκεται στην ομάδα πόρων Fabrikam. Είμαι ο κάτοχος της συνδρομής Azure.

Το GUID είναι το Αναγνωριστικό συνδρομής (μην το κλειδί οργάνων της εφαρμογής).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Παράδειγμα 2

Έχω μια εφαρμογή στην οποία να χρησιμοποιήσετε [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) για την αναφορά ένα μετρικό με το όνομα "salesPerHour." Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου τους συναδέλφους μου εάν μειωθεί "salesPerHour" κάτω από 100, μέσος όρος υπερβαίνει τις 24 ώρες.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Τον ίδιο κανόνα μπορεί να χρησιμοποιηθεί για τη μέτρηση αναφερθεί, χρησιμοποιώντας την [παράμετρο μέτρησης](app-insights-api-custom-events-metrics.md#properties) της κλήσης άλλο παρακολούθησης όπως TrackEvent ή trackPageView.

## <a name="metric-names"></a>Μετρικό ονόματα

Μετρικό όνομα | Όνομα οθόνης | Περιγραφή
---|---|---
`basicExceptionBrowser.count`|Εξαιρέσεις προγράμματος περιήγησης|Απαρίθμηση των μη καταγεγραμμένο εξαιρέσεις που ανακύπτουν στο πρόγραμμα περιήγησης.
`basicExceptionServer.count`|Εξαιρέσεις διακομιστή|Πλήθος ανεπίλυτη εξαιρέσεις που ανακύπτουν από την εφαρμογή
`clientPerformance.clientProcess.value`|Χρόνος επεξεργασίας προγράμματος-πελάτη|Χρόνος μεταξύ λαμβάνει το τελευταίο byte ενός εγγράφου, μέχρι να φορτωθεί DOM. Ασύγχρονων αιτήσεων μπορεί να επεξεργάζεται ακόμη.
`clientPerformance.networkConnection.value`|Χρόνος σύνδεση δικτύου φόρτωση σελίδας| Χρόνο που χρειάζεται το πρόγραμμα περιήγησης για να συνδεθείτε στο δίκτυο. Μπορεί να είναι 0, εάν στο cache.
`clientPerformance.receiveRequest.value`|Λήψη χρόνος απόκρισης| Χρόνου μεταξύ του προγράμματος περιήγησης Αποστολή αίτησης για να αρχίσουν να λαμβάνετε απάντηση.
`clientPerformance.sendRequest.value`|Αποστολή διάρκεια αίτησης| Ο χρόνος που λαμβάνονται από πρόγραμμα περιήγησης για να στείλετε την πρόσκληση.
`clientPerformance.total.value`|Ο χρόνος φόρτωσης περιήγησης σελίδας|Το χρονικό διάστημα από αίτηση χρήστη μέχρι να γίνεται φόρτωση DOM, φύλλα στυλ, δέσμες ενεργειών και εικόνες.
`performanceCounter.available_bytes.value`|Διαθέσιμη μνήμη|Φυσική μνήμη άμεσα διαθέσιμη για μια διεργασία ή για χρήση του συστήματος.
`performanceCounter.io_data_bytes_per_sec.value`|Ρυθμός διαδικασία εισόδου/ΕΞΌΔΟΥ|Σύνολο byte ανά δευτερόλεπτο ανάγνωση και εγγραφή σε αρχεία, δικτύου και τις συσκευές.
`performanceCounter.number_of_exceps_thrown_per_sec`|εξαίρεση επιτόκιο|Εξαιρέσεις που ανακύπτουν ανά δευτερόλεπτο.
`performanceCounter.percentage_processor_time.value`|Διαδικασία CPU|Το ποσοστό του χρόνου που πέρασε όλα τα νήματα διεργασιών που χρησιμοποιείται από τον επεξεργαστή για την εκτέλεση οδηγιών για τη διαδικασία εφαρμογές.
`performanceCounter.percentage_processor_total.value`|Χρόνος επεξεργασίας|Το ποσοστό του χρόνου που περνά ο επεξεργαστής στην νήματα μη αδράνειας.
`performanceCounter.process_private_bytes.value`|Διαδικασία ιδιωτικών byte|Μνήμη αποκλειστικά στους οποίους έχουν ανατεθεί διαδικασίες της εφαρμογής εποπτευόμενη.
`performanceCounter.request_execution_time.value`|Χρόνος εκτέλεσης αίτησης ASP.NET|Χρόνος εκτέλεσης της πιο πρόσφατης αίτησης.
`performanceCounter.requests_in_application_queue.value`|ASP.NET αιτήσεων στην ουρά εκτέλεσης|Το μήκος της ουράς αίτησης εφαρμογής.
`performanceCounter.requests_per_sec`|Ρυθμός αιτήσεων ASP.NET|Ρυθμός όλων των αιτήσεων για την εφαρμογή ανά δευτερόλεπτο από το ASP.NET.
`remoteDependencyFailed.durationMetric.count`|Αποτυχίες εξάρτησης|Απαρίθμηση των αποτυχημένων κλήσεις που έγιναν από την εφαρμογή διακομιστή για εξωτερικούς πόρους.
`request.duration`|Χρόνος απόκρισης του διακομιστή|Χρόνου μεταξύ του λαμβάνετε μια αίτηση HTTP και τελικής επεξεργασίας αποστολή της απάντησης.
`request.rate`|Αίτηση επιτόκιο|Ρυθμός όλες τις αιτήσεις για την εφαρμογή ανά δευτερόλεπτο.
`requestFailed.count`|Αποτυχημένων αιτήσεων|Αιτήσεις καταμέτρηση των HTTP που οδήγησε σε έναν κωδικό απόκριση > = 400
`view.count`|Προβολές σελίδας|Καταμέτρηση των αιτήσεων χρήστη του προγράμματος-πελάτη για μια ιστοσελίδα. Κίνηση σύνθετων έχει φιλτραριστεί.
{το προσαρμοσμένο όνομα μετρικό}|{Σας μετρικό όνομα}|Το μετρικό τιμή που αναφέρθηκε από [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) ή στην [παράμετρο μετρήσεις μιας κλήσης παρακολούθησης](app-insights-api-custom-events-metrics.md#properties).

Τα μετρικά αποστέλλονται από διαφορετικές τηλεμετρίας λειτουργικές μονάδες:

Μετρικό ομάδας | Λειτουργική μονάδα συλλογής
---|---
basicExceptionBrowser,<br/>clientPerformance,<br/>Προβολή | [Πρόγραμμα περιήγησης JavaScript](app-insights-javascript.md)
performanceCounter | [Επιδόσεων](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [Εξάρτηση](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
αίτηση,<br/>requestFailed|[Αίτηση διακομιστή](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

Μπορείτε να [αυτοματοποιήσετε την απάντησή σας σε μια ειδοποίηση](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure θα καλέσετε μια διεύθυνση web της επιλογής σας όταν ενεργοποιείται μια ειδοποίηση.

## <a name="see-also"></a>Δείτε επίσης


* [Δέσμη ενεργειών για να ρυθμίσετε τις παραμέτρους ιδέες εφαρμογής](app-insights-powershell-script-create-resource.md)
* [Δημιουργία εφαρμογής ιδέες και πόρους δοκιμής web από πρότυπα](app-insights-powershell.md)
* [Αυτοματοποίηση σύζευξη τα Διαγνωστικά του Microsoft Azure για ιδέες εφαρμογής](app-insights-powershell-azure-diagnostics.md)
* [Αυτοματοποίηση την απάντησή σας σε μια ειδοποίηση](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
