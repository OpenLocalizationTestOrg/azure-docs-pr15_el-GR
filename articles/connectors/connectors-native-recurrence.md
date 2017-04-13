<properties
    pageTitle="Προσθέστε το έναυσμα περιοδικότητας σε εφαρμογές της λογικής | Microsoft Azure"
    description="Επισκόπηση των το έναυσμα περιοδικότητα και πώς μπορείτε να το χρησιμοποιήσετε με μια εφαρμογή της λογικής Azure."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-recurrence-trigger"></a>Γρήγορα αποτελέσματα με το έναυσμα περιοδικότητας

Χρησιμοποιώντας το έναυσμα Περιοδικότητα, μπορείτε να δημιουργήσετε ισχυρές ροές εργασίας στο cloud.

Για παράδειγμα, μπορείτε να κάνετε:

- Προγραμματισμός μιας ροής εργασίας για να εκτελέσετε μια αποθηκευμένη διαδικασία SQL καθημερινά.
- Μια σύνοψη όλων tweets ηλεκτρονικού ταχυδρομείου από την τελευταία εβδομάδα σχετικά με ένα συγκεκριμένο hashtag.

Για να ξεκινήσετε να χρησιμοποιείτε το έναυσμα περιοδικότητας σε μια εφαρμογή λογικής, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής λογικής](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Χρησιμοποιήστε ένα έναυσμα περιοδικότητας

Ένα έναυσμα είναι ένα συμβάν που μπορούν να χρησιμοποιηθούν για να ξεκινήσετε τη ροή εργασίας που έχει οριστεί σε μια εφαρμογή για λογική. [Μάθετε περισσότερα σχετικά με τα εναύσματα](connectors-overview.md).

Ακολουθεί ένα παράδειγμα ακολουθία πώς μπορείτε να ρυθμίσετε ένα έναυσμα περιοδικότητας σε μια εφαρμογή λογικής:

1. Προσθέστε το έναυσμα **Περιοδικότητα** ως το πρώτο βήμα σε μια εφαρμογή για λογική.
2. Συμπληρώστε τις παραμέτρους για το χρονικό διάστημα περιοδικότητας.

Η εφαρμογή λογική ξεκινά τώρα μια εκτέλεση μετά από κάθε χρονικό διάστημα.

![Έναυσμα HTTP](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Λεπτομέρειες εναυσμάτων

Το έναυσμα Περιοδικότητα περιλαμβάνει τις ακόλουθες ιδιότητες που μπορείτε να ρυθμίσετε.

Το ενεργοποιείται μια εφαρμογή λογικής μετά από ένα καθορισμένο χρονικό διάστημα.
A * σημαίνει ότι είναι απαιτούμενο πεδίο.

|Εμφανιζόμενο όνομα|Όνομα ιδιότητας|Περιγραφή|
|---|---|---|
|Συχνότητα *|συχνότητα|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|Διάστημα *|χρονικό διάστημα|Το χρονικό διάστημα της δεδομένης συχνότητας για την περιοδικότητα.|
|Ζώνη ώρας|ζώνη ώρας|Εάν παρέχεται μια ώρα έναρξης χωρίς μια απόκλιση UTC, θα χρησιμοποιηθεί αυτήν τη ζώνη ώρας.|
|Ώρα έναρξης|ώρα έναρξης|Την ώρα έναρξης σε [μορφή ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).|
<br>


## <a name="next-steps"></a>Επόμενα βήματα

Τώρα, δοκιμάστε την πλατφόρμα και να [δημιουργήσετε μια εφαρμογή λογικής](../app-service-logic/app-service-logic-create-a-logic-app.md). Μπορείτε να εξερευνήσετε τις άλλες διαθέσιμες συνδέσεις σε εφαρμογές της λογικής, εξετάζοντας τη [λίστα APIs](apis-list.md).