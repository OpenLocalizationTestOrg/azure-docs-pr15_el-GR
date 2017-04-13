<properties
    pageTitle="Ρύθμιση παραμέτρων ενός webhook στο αρχείο καταγραφής δραστηριοτήτων Azure ειδοποιήσεις | Microsoft Azure"
    description="Δείτε πώς μπορείτε να χρησιμοποιήσετε αρχείο καταγραφής δραστηριοτήτων ειδοποιήσεις για να καλέσετε webhooks. "
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
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-activity-log-alerts"></a>Ρύθμιση παραμέτρων ενός webhook στο ένα ειδοποιήσεις Azure αρχείο καταγραφής δραστηριοτήτων

Webhooks σάς επιτρέπουν να δρομολογήσετε Azure μια ειδοποίηση σε άλλα συστήματα για επεξεργασίας εξόδου ή προσαρμοσμένες ενέργειες. Μπορείτε να χρησιμοποιήσετε ένα webhook σε μια ειδοποίηση για να δρομολογήσετε σε υπηρεσίες που Αποστολή SMS, καταγραφή σφαλμάτων, ειδοποιήστε μια ομάδα μέσω ανταλλαγής μηνυμάτων συνομιλίας/υπηρεσιών ή κάντε όσες άλλες ενέργειες. Σε αυτό το άρθρο περιγράφει πώς μπορείτε να ορίσετε μια webhook ειδοποίησης Azure δραστηριότητα καταγραφής και το φορτίο για τη ΔΗΜΟΣΊΕΥΣΗ HTTP σε μια webhook μοιάζει. Για πληροφορίες σχετικά με την εγκατάσταση και τη διάταξη για Azure μετρικό ειδοποίησης, [ανατρέξτε στο θέμα αυτήν τη σελίδα αντί για αυτό](insights-webhooks-alerts.md). Επίσης, μπορείτε να ρυθμίσετε μια ειδοποίηση καταγραφής δραστηριότητας για να στείλετε μήνυμα ηλεκτρονικού ταχυδρομείου κατά την ενεργοποίηση.

>[AZURE.NOTE] Αυτή η δυνατότητα είναι αυτήν τη στιγμή στην προεπισκόπηση, επομένως, περιμένετε μεταβλητής ποιότητας και επιδόσεων.

Μπορείτε να ρυθμίσετε μια ειδοποίηση αρχείο καταγραφής δραστηριοτήτων με τα [Cmdlet του PowerShell Azure](insights-powershell-samples.md#create-alert-rules), [Πλατφόρμες CLI](insights-cli-samples.md#work-with-alerts)ή [Azure οθόνη REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Έλεγχος ταυτότητας του webhook
Η webhook μπορούν να ελέγχουν την ταυτότητα χρησιμοποιώντας μία από τις παρακάτω μεθόδους:

1. **Διακριτικό βάσει εξουσιοδότησης** - το webhook URI αποθηκεύεται με ένα Αναγνωριστικό διακριτικού, π.χ. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Βασικές εξουσιοδότησης** - το webhook URI αποθηκεύεται με το όνομα χρήστη και τον κωδικό πρόσβασης, π.χ. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Φορτίο σχήματος
Η λειτουργία ΔΗΜΟΣΊΕΥΣΗ περιλαμβάνει την ακόλουθη φορτίου JSON και διάταξη για όλες τις ειδοποιήσεις που βασίζεται στο αρχείο καταγραφής δραστηριοτήτων. Αυτό το σχήμα είναι παρόμοια με αυτή που χρησιμοποιούνται από τις ειδοποιήσεις που βασίζονται σε μετρικό σύστημα.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|Όνομα στοιχείου       |Περιγραφή|
|---                |---|
|κατάσταση             |Χρησιμοποιείται για μετρικό ειδοποιήσεις. Πάντα την τιμή "ενεργοποιηθεί" για το αρχείο καταγραφής δραστηριοτήτων ειδοποιήσεις.|
|περιβάλλον            |Το περιβάλλον του συμβάντος.|
|resourceProviderName|Η υπηρεσία παροχής πόρων του επηρεαζόμενη πόρου.|
|conditionType      |Πάντα "συμβάν".|
|Όνομα               |Το όνομα του κανόνα ειδοποίησης.|
|αναγνωριστικό                 |Αναγνωριστικό πόρου της ειδοποίησης.|
|Περιγραφή        |Περιγραφή ειδοποίησης ως σύνολο κατά τη δημιουργία της ειδοποίησης.|
|subscriptionId     |Αναγνωριστικό συνδρομής Azure.|
|χρονική σήμανση          |Ώρα κατά την οποία δημιουργήθηκε το συμβάν από την υπηρεσία Azure που υποβάλλονται σε επεξεργασία της αίτησης.|
|resourceId         |Αναγνωριστικό πόρου του επηρεαζόμενη πόρου.|
|resourceGroupName  |Όνομα της ομάδας πόρων για τον πόρο επηρεαζόμενη|
|Ιδιότητες         |Ορισμός της `<Key, Value>` ζεύγη (δηλαδή `Dictionary<String, String>`) που περιλαμβάνει λεπτομέρειες σχετικά με το συμβάν.|
|συμβάν              |Στοιχείο που περιέχει μετα-δεδομένα σχετικά με το συμβάν.|
|εξουσιοδότηση      |Οι ιδιότητες RBAC του συμβάντος. Αυτά περιλαμβάνουν συνήθως η "ενέργεια", "ρόλος" και "εύρος".|
|κατηγορία           |Κατηγορία του συμβάντος. Υποστηριζόμενες τιμές περιλαμβάνουν: διαχείρισης, ειδοποίηση, ασφαλείας, ServiceHealth, προτάσεων.|
|καλούντος             |Διεύθυνση ηλεκτρονικού ταχυδρομείου του χρήστη που έχει πραγματοποιηθεί η λειτουργία, UPN διεκδίκηση ή με βάση τη διαθεσιμότητα διεκδίκηση κύριο όνομα Υπηρεσίας. Μπορεί να είναι null για ορισμένες κλήσεις συστήματος.|
|correlationId      |Συνήθως ένα GUID σε μορφή συμβολοσειράς. Συμβάντα με correlationId ανήκουν την ίδια ενέργεια μεγαλύτερα και συνήθως κάνετε κοινή χρήση ενός correlationId.|
|eventDescription   |Στατικό κείμενο περιγραφή του συμβάντος.|
|eventDataId        |Μοναδικό αναγνωριστικό για το συμβάν.|
|Προέλευση_συμβάντος        |Το όνομα της υπηρεσίας Azure ή υποδομή που δημιούργησε το συμβάν.|
|httpRequest        |Συνήθως περιλαμβάνει το "clientRequestId", "clientIpAddress" και "μέθοδος" (μέθοδος HTTP π.χ. ΤΟΠΟΘΈΤΗΣΗ).|
|επίπεδο              |Μία από τις παρακάτω τιμές: "Κρίσιμη", "Σφάλμα", "Προειδοποίηση", "Ενημερωτικό" και "Λεπτομερές".|
|operationId        |Συνήθως GUID κοινού από τα συμβάντα που αντιστοιχεί σε μία λειτουργία.|
|operationName      |Όνομα της λειτουργίας.|
|Ιδιότητες         |Ιδιότητες του συμβάντος.|
|κατάσταση             |Συμβολοσειρά. Κατάσταση της λειτουργίας. Συνηθισμένες τιμές περιλαμβάνουν: "Αποτελέσματα", "Σε εξέλιξη", "Ολοκληρώθηκε με επιτυχία", "Απέτυχε", "Ενεργές", "Επίλυση".|
|δευτερεύουσα κατάσταση          |Συνήθως περιλαμβάνει ο κωδικός κατάστασης HTTP από την αντίστοιχη κλήση ΥΠΌΛΟΙΠΟ. Μπορεί επίσης να περιλαμβάνει άλλες συμβολοσειρές που περιγράφει μια δευτερεύουσα κατάσταση. Κοινές τιμές δευτερεύουσα κατάσταση περιλαμβάνουν: OK (κωδικό κατάστασης HTTP: 200), που έχουν δημιουργηθεί (κωδικό κατάστασης HTTP: 201), αποδεκτή (κωδικό κατάστασης HTTP: 202), δεν περιεχομένου (κωδικό κατάστασης HTTP: 204), ακατάλληλη αίτηση (κωδικό κατάστασης HTTP: 400), δεν βρέθηκε (κωδικό κατάστασης HTTP: 404), διένεξης (κωδικό κατάστασης HTTP: 409), εσωτερικό σφάλμα διακομιστή (κωδικό κατάστασης HTTP: 500), υπηρεσία δεν είναι διαθέσιμη (κωδικό κατάστασης HTTP: 503), χρονικό όριο πύλης (κωδικό κατάστασης HTTP: 504)|

## <a name="next-steps"></a>Επόμενα βήματα
- [Μάθετε περισσότερα σχετικά με το αρχείο καταγραφής της δραστηριότητας](monitoring-overview-activity-logs.md)
- [Εκτέλεση δεσμών ενεργειών αυτοματοποίησης Azure (Runbooks) στο Azure ειδοποιήσεων](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Χρήση λογικής App για να στείλετε μια SMS μέσω Twilio από το Azure ειδοποίησης](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Αυτό το παράδειγμα είναι για μετρικό ειδοποιήσεις, αλλά μπορεί να τροποποιηθεί ώστε να λειτουργεί με το αρχείο καταγραφής δραστηριοτήτων ειδοποίησης.
- [Χρήση λογικής App για να στείλετε ένα μήνυμα αδράνειας από Azure ειδοποίησης](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Αυτό το παράδειγμα είναι για μετρικό ειδοποιήσεις, αλλά μπορεί να τροποποιηθεί ώστε να λειτουργεί με το αρχείο καταγραφής δραστηριοτήτων ειδοποίησης.
- [Χρήση λογικής εφαρμογής για να στείλετε ένα μήνυμα σε μια ουρά Azure από το Azure ειδοποίησης](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Αυτό το παράδειγμα είναι για μετρικό ειδοποιήσεις, αλλά μπορεί να τροποποιηθεί ώστε να λειτουργεί με το αρχείο καταγραφής δραστηριοτήτων ειδοποίησης.
