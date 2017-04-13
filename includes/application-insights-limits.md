Υπάρχουν κάποια όρια στον αριθμό των μετρήσεων και συμβάντα ανά εφαρμογή (δηλαδή, ανά κλειδί οργάνων). 

Όρια εξαρτώνται από την [Τιμολόγηση επιπέδων](https://azure.microsoft.com/pricing/details/application-insights/) που επιλέγετε.

**Πόρων** | **Προεπιλεγμένο όριο** | **Μέγιστο όριο**
-------- | ------------- | -------------
Σημεία δεδομένων λειτουργίας<sup>1, 2</sup> ανά μήνα | απεριόριστος χώρος | 
Σημεία σύνολο δεδομένων ανά μήνα για την αίτηση συμβάντος, εξάρτηση, ανίχνευση, εξαίρεσης και προβολή σελίδας | 5 εκατομμύρια | 50 εκατομμύρια<sup>3</sup>
[Ανίχνευση και καταγραφής](../articles/application-insights/app-insights-search-diagnostic-logs.md) την ταχύτητα δεδομένων | 200 dp/s | 500 dp/s
Ρυθμός δεδομένων [εξαίρεσης](../articles/application-insights/app-insights-asp-net-exceptions.md) | 50 dp/s | 50 dp/s
Σύνολο δεδομένων επιτόκιο για την αίτηση, συμβάν, εξάρτηση και τηλεμετρίας προβολής σελίδας | 200 dp/s | 500 dp/s
Διατήρηση ανεπεξέργαστα δεδομένα για την [Αναζήτηση](../articles/application-insights/app-insights-diagnostic-search.md) και [ανάλυση](../articles/application-insights/app-insights-analytics.md) | 7 ημέρες
Συγκεντρωτικά δεδομένα διατήρησης για [μετρικά explorer](../articles/application-insights/app-insights-metrics-explorer.md) | 90 ημέρες
Το πλήθος όνομα [ιδιότητας](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Μήκος ονόματος ιδιότητας | 150 | 
Το μήκος της τιμής ιδιότητας | 8192 | 
Ανίχνευση και εξαίρεση μήκος μηνύματος | 10000 |
Καταμέτρηση όνομα [μετρικό σύστημα](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Μήκος ονόματος μετρικό |  150 | 
[Διαθεσιμότητα δοκιμές](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> ένα σημείο δεδομένων είναι μια μεμονωμένη τιμή μετρικό ή ένα συμβάν, με συνημμένο ιδιότητες και τις μετρήσεις.

Περίοδος λειτουργίας A <sup>2</sup> σημείο δεδομένων καταγράφει την έναρξη ή η λήξη μιας περιόδου λειτουργίας και καταγράφει ταυτότητα χρήστη.

<sup>3</sup> μπορείτε να αγοράσετε επιπλέον χωρητικότητα πέρα από 50 εκατομμύρια.
 
[Σχετικά με τις πληροφορίες τιμολόγησης και ορίων στη ιδέες εφαρμογής](../articles/application-insights/app-insights-pricing.md)
