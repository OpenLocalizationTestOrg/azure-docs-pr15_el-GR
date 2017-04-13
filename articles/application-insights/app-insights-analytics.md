<properties 
    pageTitle="Ανάλυση - το εργαλείο ισχυρή αναζήτησης της εφαρμογής ιδέες | Microsoft Azure" 
    description="Επισκόπηση της ανάλυσης, το εργαλείο ισχυρή διαγνωστικών αναζήτησης της εφαρμογής ιδέες. " 
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
    ms.date="07/26/2016" 
    ms.author="awills"/>


# <a name="analytics-in-application-insights"></a>Ανάλυση σε ιδέες εφαρμογής


[Ανάλυση](app-insights-analytics.md) είναι η δυνατότητα ισχυρή αναζήτησης της [Εφαρμογής ιδέες](app-insights-overview.md). Αυτές οι σελίδες περιγράφουν τη lanquage ερωτήματος ανάλυσης. 

* **[Παρακολουθήστε το εισαγωγικό βίντεο](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Μονάδα δίσκου δοκιμή ανάλυσης μας προσομοιωμένη δεδομένα](https://analytics.applicationinsights.io/demo)** κατά την εφαρμογή σας δεν είναι αποστολή δεδομένων σε εφαρμογή ιδέες ακόμη.

## <a name="queries-in-analytics"></a>Ερωτήματα στην ανάλυση
 
Ένα τυπικό ερώτημα είναι ένας πίνακας *προέλευσης* ακολουθούμενο από μια σειρά *τελεστών* , διαχωρισμένα με `|`. 

Για παράδειγμα, ας Μάθετε τι ώρα της ημέρας του πολίτες της Hyderabad δοκιμή μας εφαρμογής web. Και ενώ είστε εκεί, ας δούμε τι κωδικών αποτελέσματος επιστρέφετε σε αιτήσεις τους HTTP. 

```AIQL

    requests      // Table of events that log HTTP requests.
  	| where timestamp > ago(7d) and client_City == "Hyderabad"
  	| summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
  	| extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

Θα σας καταμέτρηση διευθύνσεις IP διακριτές προγράμματος-πελάτη, ομαδοποιώντας τις κατά την ώρα της ημέρας επάνω από τις τελευταίες 7 ημέρες. 

Ας εμφανίζουν τα αποτελέσματα με την παρουσίαση γράφημα ράβδων, επιλέγοντας να φιλτράρετε τα αποτελέσματα από διαφορετικούς απόκριση κωδικούς:

![Επιλέξτε το γράφημα ράβδων, x και y αξόνων και στη συνέχεια αγοράς](./media/app-insights-analytics/020.png)

Φαίνεται μας εφαρμογή είναι πιο δημοφιλή μεσημέρι και πέταλα-ώρα στο Hyderabad. (Και θα πρέπει να εξετάσουμε αυτές τις 500 κώδικες.)


Υπάρχουν επίσης ισχυρή στατιστικές λειτουργίες:

![](./media/app-insights-analytics/025.png)


Η γλώσσα περιλαμβάνει πολλές δυνατότητες ελκυστική:

* [Φίλτρο](app-insights-analytics-reference.md#where-operator) σας τηλεμετρίας ανεπεξέργαστα εφαρμογής από τα πεδία, όπως τις προσαρμοσμένες ιδιότητες και μετρήσεις.
* [Συμμετοχή σε](app-insights-analytics-reference.md#join-operator) πολλούς πίνακες – correlate αιτήσεις με προβολές σελίδας, οι κλήσεις εξάρτηση, εξαιρέσεις και καταγραφής παρακολουθεί.
* Ισχυρές στατιστικές [συναθροίσεων](app-insights-analytics-reference.md#aggregations).
* Μόλις ισχυρή ως SQL, αλλά πολύ πιο εύκολο για σύνθετα ερωτήματα: αντί για ένθεσης καταστάσεις, διοχέτευση τα δεδομένα από μια λειτουργία δημοτικού στην επόμενη.
* Άμεση και ισχυρές απεικονίσεις.







## <a name="connect-to-your-application-insights-data"></a>Σύνδεση με τα δεδομένα σας ιδέες εφαρμογής


Άνοιγμα Analytics από την εφαρμογή [Επισκόπηση blade](app-insights-dashboards.md) στην εφαρμογή ιδέες: 

![Άνοιγμα portal.azure.com, άνοιγμα του πόρου ιδέες εφαρμογή και κάντε κλικ στην επιλογή ανάλυσης.](./media/app-insights-analytics/001.png)


## <a name="limits"></a>Όρια

Προς το παρόν, αποτελέσματα ερωτήματος περιορίζονται μόνο πάνω από μία εβδομάδα προηγούμενες δεδομένων.



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>Επόμενα βήματα


* Συνιστάται να ξεκινάτε με την [Περιήγηση γλώσσας](app-insights-analytics-tour.md).