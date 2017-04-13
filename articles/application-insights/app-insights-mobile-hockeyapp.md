<properties
    pageTitle="Παρακολούθηση των επιδόσεων για κινητές συσκευές web apps με ανάλυση προγραμματιστής | Microsoft Azure"
    description="Εφαρμογή επιδόσεις και την παρακολούθηση χρήσης για τους προγραμματιστές εφαρμογής για κινητές συσκευές. , επιφάνειας εργασίας, υπηρεσία web και εφαρμογές παρασκηνίου με HockeyApp και τις ιδέες εφαρμογής."
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Κινητές συσκευές ανάλυσης με HockeyApp και τις ιδέες εφαρμογής

Παρακολουθείτε τις επιδόσεις και χρήση των βήτα έλεγχος και ανάπτυξη εφαρμογών φορητούς και επιτραπέζιους με [HockeyApp](https://hockeyapp.net/). Παρακολούθηση του υποστήριξης web υπηρεσία και παρασκηνίου εφαρμογών με το [Visual Studio εφαρμογή ιδέες](app-insights-overview.md). Ανάλυση των δεδομένων από τον υπολογιστή-πελάτη και το διακομιστή εφαρμογές στην ανάλυση και να εμφανίσετε τα γραφήματα μαζί με μεταξύ τους σε έναν πίνακα εργαλείων Azure.

Ανάλυση προγραμματιστής Microsoft προσφέρει δύο λύσεις για την Εποπτεία επιδόσεων εφαρμογής:

* **HockeyApp για κινητές συσκευές** και τις εφαρμογές υπολογιστή.
 * Διανομή δοκιμαστικές εκδόσεις σε επιλεγμένους χρήστες.
 * Σφάλμα ανάλυσης.
 * Προσαρμοσμένη τηλεμετρίας για ανάλυση χρήσης.
* **Εφαρμογή ιδέες για τοποθεσίες web** και τις υπηρεσίες και παρασκηνίου εφαρμογές.
 * Μετρικά απόδοσης και της χρήσης και ειδοποιήσεις.
 * Εξαίρεση αναφορών και ανίχνευση διαγνωστικών.
 * Διαγνωστικών αναζήτησης με φιλτράρισμα και τις σχετικές συνδέσεις τηλεμετρίας.

Δύο λύσεων προσφέρουν:

 * Ισχυρές **[γλώσσας ερωτήματος ανάλυσης](app-insights-analytics.md)** για εργαλεία διαγνωστικών και ανάλυση.
 * **[Εξαγωγή δεδομένων](app-insights-export-telemetry.md)** στο δικό του χώρο αποθήκευσης.
 * Εμφάνιση **ολοκληρωμένου πίνακα εργαλείων** αναλυτικών γραφημάτων και πινάκων.

## <a name="monitor-your-app-components"></a>Εποπτεία των στοιχείων σας εφαρμογής

Ακολουθήστε τις οδηγίες σε αυτές τις σελίδες για να εγκαταστήσετε το SDK στον κώδικά σας και να ξεκινήσετε την παρακολούθηση της εφαρμογής σας.

### <a name="web-apps---application-insights"></a>Εφαρμογές Web - ιδέες εφαρμογής

* [Εφαρμογή web ASP.NET](app-insights-asp-net.md) 
* [Εφαρμογή web Java](app-insights-java-get-started.md)
* [Node.js web app](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Υπηρεσίες Azure Cloud](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Εφαρμογές για κινητές συσκευές - HockeyApp

* [εφαρμογή iOS](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X εφαρμογής](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Εφαρμογή Android](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Εφαρμογή καθολικής Windows](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [Εφαρμογή Windows Phone 8 και 8.1](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Παρουσίαση Windows Foundation εφαρμογής](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

Για τις εφαρμογές στην επιφάνεια εργασίας των Windows, συνιστάται να HockeyApp. Αλλά μπορείτε επίσης να [στείλετε τηλεμετρίας από μια εφαρμογή για Windows σε εφαρμογή ιδέες](app-insights-windows-desktop.md). Μπορείτε να το κάνετε για να πειραματιστείτε με εφαρμογή ιδέες.


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Ανάλυση και εξαγωγή για HockeyApp τηλεμετρίας

Για να διερευνήσουμε προσαρμοσμένη HockeyApp και καταγραφή τηλεμετρίας χρησιμοποιώντας τις ανάλυση και συνεχής εξαγωγή δυνατότητες της εφαρμογής ιδέες κατά [τη ρύθμιση του γέφυρα](app-insights-hockeyapp-bridge-app.md).




