<properties
    pageTitle="Ρύθμιση παραμέτρων webhooks στο Azure μετρικό ειδοποιήσεις | Microsoft Azure"
    description="Αναδρομολόγηση Azure ειδοποιήσεων για άλλα συστήματα μη Azure."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Ρύθμιση παραμέτρων ενός webhook στο Azure μετρικό ειδοποίησης

Webhooks σάς επιτρέπουν να δρομολογήσετε Azure μια ειδοποίηση σε άλλα συστήματα για επεξεργασίας εξόδου ή προσαρμοσμένες ενέργειες. Μπορείτε να χρησιμοποιήσετε ένα webhook σε μια ειδοποίηση για να δρομολογήσετε σε υπηρεσίες που Αποστολή SMS, καταγραφή σφαλμάτων, ειδοποιήστε μια ομάδα μέσω ανταλλαγής μηνυμάτων συνομιλίας/υπηρεσιών ή κάντε όσες άλλες ενέργειες. Σε αυτό το άρθρο περιγράφει πώς μπορείτε να ορίσετε μια webhook Azure μετρικό ειδοποίησης και το φορτίο για τη ΔΗΜΟΣΊΕΥΣΗ HTTP σε μια webhook μοιάζει. Για πληροφορίες σχετικά με την εγκατάσταση και τη διάταξη για ένα αρχείο καταγραφής της δραστηριότητας Azure λαμβάνω (ειδοποίηση σχετικά με τα συμβάντα), [ανατρέξτε στο θέμα αυτήν τη σελίδα αντί για αυτό](./insights-auditlog-to-webhook-email.md).

Μορφοποιήστε τα περιεχόμενα ειδοποίησης στο JSON Azure ειδοποιήσεις HTTP POST, σχήματος που ορίζονται από το παρακάτω, σε ένα webhook URI που παρέχετε κατά τη δημιουργία της ειδοποίησης. Σε αυτό το URI πρέπει να είναι ένα έγκυρο τελικό σημείο HTTP ή HTTPS. Azure καταχωρεί μία εγγραφή ανά αίτηση όταν είναι ενεργοποιημένη μια ειδοποίηση.

## <a name="configuring-webhooks-via-the-portal"></a>Ρύθμιση παραμέτρων webhooks μέσω της πύλης

Μπορείτε να προσθέσετε ή να ενημερώσετε το webhook URI στην οθόνη δημιουργία/ενημέρωση ειδοποιήσεις στην [πύλη](https://portal.azure.com/).

![Προσθήκη κανόνα ειδοποίησης](./media/insights-webhooks-alerts/Alertwebhook.png)

Μπορείτε επίσης να ρυθμίσετε μια ειδοποίηση για να δημοσιεύσετε μια webhook URI χρησιμοποιώντας τα [Cmdlet του PowerShell Azure](./insights-powershell-samples.md#create-alert-rules), [Πλατφόρμες CLI](./insights-cli-samples.md#work-with-alerts)ή [Azure οθόνη REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Έλεγχος ταυτότητας του webhook

Το webhook μπορούν να ελέγχουν την ταυτότητα χρησιμοποιώντας μία από τις παρακάτω μεθόδους:

1. **Διακριτικό βάσει εξουσιοδότησης** - το webhook URI αποθηκεύεται με ένα Αναγνωριστικό διακριτικού, π.χ. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Βασικές εξουσιοδότησης** - το webhook URI αποθηκεύεται με το όνομα χρήστη και τον κωδικό πρόσβασης, π.χ. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Φορτίο σχήματος

Η λειτουργία ΔΗΜΟΣΊΕΥΣΗ περιλαμβάνει την ακόλουθη φορτίου JSON και διάταξη για όλες τις ειδοποιήσεις βάσει μετρικό σύστημα.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Πεδίο | Υποχρεωτική | Σταθερό σύνολο τιμών | Σημειώσεις |
| :-------------| :-------------   | :-------------   | :-------------   |
|κατάσταση|Y|"Ενεργοποιημένο", "Επίλυση"|Κατάσταση για την ειδοποίηση βάσει από τις συνθήκες που έχετε ορίσει.|
|περιβάλλον| Y | | Το περιβάλλον ειδοποίησης.|
|χρονική σήμανση| Y | | Η ώρα κατά την οποία ενεργοποιήθηκε την ειδοποίηση.|
|αναγνωριστικό | Y | | Κάθε κανόνας ειδοποίησης έχει ένα μοναδικό αναγνωριστικό.|
|Όνομα               |Y                  |                   | Το όνομα ειδοποίησης.|
|Περιγραφή        |Y                  |                           |Περιγραφή της ειδοποίησης.|
|conditionType      |Y                  |"Μετρικό", "Συμβάν"          |Υποστηρίζονται δύο τύπους ειδοποιήσεων. Ένα με βάση μια συνθήκη μετρικό και το άλλο με βάση ένα συμβάν στο αρχείο καταγραφής της δραστηριότητας. Χρησιμοποιήστε αυτήν την τιμή για να ελέγξετε εάν την ειδοποίηση βασίζεται σε μετρικό σύστημα ή το συμβάν.|
|η συνθήκη          |Y                  |                           | Για να ελέγξετε για τα συγκεκριμένα πεδία με βάση το conditionType.|
|metricName         |για τις ειδοποιήσεις μετρικό  |                           |Το όνομα του τη μέτρηση που καθορίζει τι παρακολουθεί τον κανόνα.|
|metricUnit         |για τις ειδοποιήσεις μετρικό  |"Byte που", "BytesPerSecond", "Καταμέτρηση", "CountPerSecond", "Ποσοστό", "Δευτερόλεπτα"|     Η μονάδα που επιτρέπονται στον τη μέτρηση. [Τιμές επιτρεπόμενων παρατίθενται εδώ](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx).|
|metricValue        |για τις ειδοποιήσεις μετρικό  |                           |Η πραγματική τιμή τη μέτρηση που προκάλεσαν την ειδοποίηση.|
|όριο          |για τις ειδοποιήσεις μετρικό  |                           |Η τιμή ορίου στο οποίο ενεργοποιείται η ειδοποίηση.|
|windowSize         |για τις ειδοποιήσεις μετρικό  |                           |Το χρονικό διάστημα που χρησιμοποιείται για την παρακολούθηση της ειδοποίησης δραστηριότητας με βάση το όριο. Πρέπει να είναι μεταξύ 5 λεπτά και 1 ημέρα. Μορφή ISO 8601 διάρκεια.|
|timeAggregation    |για τις ειδοποιήσεις μετρικό  |"Average", "Επώνυμο", "Έως", "Ελάχιστη", "Κανένα", "σύνολο" | Πώς θα πρέπει να συνδυάσετε τα δεδομένα που συλλέγονται μέσα στο χρόνο. Η προεπιλεγμένη τιμή είναι μέσο όρο. [Τιμές επιτρεπόμενων παρατίθενται εδώ](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx).|
|Τελεστής           |για τις ειδοποιήσεις μετρικό  |                           |Τον τελεστή που χρησιμοποιείται για να συγκρίνετε τα τρέχοντα δεδομένα μετρικό για το όριο του συνόλου.|
|subscriptionId     |Y                  |                           |Αναγνωριστικό συνδρομής Azure.|
|resourceGroupName  |Y                  |                           |Όνομα της ομάδας πόρων για τον πόρο επηρεαζόμενη.|
|resourceName       |Y                  |                           |Όνομα πόρου του επηρεαζόμενη πόρου.|
|τύπου πόρου       |Y                  |                           |Τύπος πόρου του επηρεαζόμενη πόρου.|
|resourceId         |Y                  |                           |Αναγνωριστικό πόρου του επηρεαζόμενη πόρου.|
|resourceRegion     |Y                  |                           |Περιοχή ή θέση του επηρεαζόμενη πόρου.|
|portalLink         |Y                  |                           |Απευθείας σύνδεση στη σελίδα σύνοψη πύλης πόρων.|
|Ιδιότητες         |N                  |Προαιρετικό                   |Ορισμός της `<Key, Value>` ζεύγη (δηλαδή `Dictionary<String, String>`) που περιλαμβάνει λεπτομέρειες σχετικά με το συμβάν. Το πεδίο "Ιδιότητες" είναι προαιρετικό. Σε ένα προσαρμοσμένο περιβάλλον εργασίας Χρήστη ή τη λογική βασίζεται σε εφαρμογή ροή εργασίας, οι χρήστες να εισαγάγετε το κλειδί/τιμές που μπορούν να περάσουν μέσω το φορτίο. Το εναλλακτικό τρόπος για να μεταβιβάσετε προσαρμοσμένες ιδιότητες του webhook είναι μέσω του uri webhook (ως παράμετροι ερωτήματος)|


>[AZURE.NOTE] Το πεδίο "Ιδιότητες" μπορεί να οριστεί μόνο με χρήση του [Azure οθόνη REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε περισσότερα σχετικά με το Azure ειδοποιήσεις και webhooks στην [Ενοποίηση ειδοποιήσεις Azure με PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080) βίντεο
- [Εκτέλεση δεσμών ενεργειών αυτοματοποίησης Azure (Runbooks) στο Azure ειδοποιήσεων](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Χρήση λογικής εφαρμογής για να στείλετε μια SMS μέσω Twilio από το Azure ειδοποίησης](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
- [Χρήση λογικής εφαρμογής για να στείλετε ένα μήνυμα αδράνειας από το Azure ειδοποίησης](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
- [Χρήση λογικής εφαρμογής για να στείλετε ένα μήνυμα σε μια ουρά Azure από το Azure ειδοποίησης](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
