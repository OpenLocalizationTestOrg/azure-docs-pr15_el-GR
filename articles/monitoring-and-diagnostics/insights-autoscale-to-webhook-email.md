<properties
    pageTitle="Χρησιμοποιήστε autoscale ενέργειες για να στείλετε μηνύματα ηλεκτρονικού ταχυδρομείου και webhook οι ειδοποιήσεις. | Microsoft Azure"
    description="Δείτε πώς μπορείτε να χρησιμοποιήσετε autoscale ενέργειες για να καλέσετε διευθύνσεις URL web ή αποστολή ειδοποιήσεων ηλεκτρονικού ταχυδρομείου σε οθόνη Azure. "
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
    ms.date="07/19/2016"
    ms.author="ashwink"/>

# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Χρησιμοποιήστε autoscale ενέργειες για να στείλετε μηνύματα ηλεκτρονικού ταχυδρομείου και webhook οι ειδοποιήσεις στην οθόνη Azure

Αυτό το άρθρο παρουσιάζει πώς ρυθμίζετε εναύσματα, έτσι ώστε να μπορείτε να καλέσετε συγκεκριμένες web διευθύνσεις URL ή να στέλνει μηνύματα ηλεκτρονικού ταχυδρομείου που βασίζεται σε ενέργειες autoscale στο Azure.  

## <a name="webhooks"></a>Webhooks
Webhooks σάς επιτρέπουν να δρομολογήσετε το Azure οι ειδοποιήσεις σε άλλα συστήματα για τις ειδοποιήσεις της επεξεργασίας εξόδου ή προσαρμοσμένο. Για παράδειγμα, δρομολόγηση την ειδοποίηση για υπηρεσίες που μπορεί να διαχειριστεί μια εισερχόμενη αίτηση web για την αποστολή SMS, αρχείο καταγραφής σφαλμάτων, ειδοποιήστε μιας ομάδας χρησιμοποιώντας τη συνομιλία ή μηνυμάτων των υπηρεσιών, κ.λπ. Το webhook URI πρέπει να είναι ένα έγκυρο τελικό σημείο HTTP ή HTTPS.

## <a name="email"></a>Μήνυμα ηλεκτρονικού ταχυδρομείου
Μήνυμα ηλεκτρονικού ταχυδρομείου μπορούν να σταλούν στον οποιαδήποτε έγκυρη διεύθυνση ηλεκτρονικού ταχυδρομείου. Οι διαχειριστές και διαχειριστές από κοινού της συνδρομής όπου εκτελείται ο κανόνας θα ειδοποιηθείτε επίσης.


## <a name="cloud-services-and-web-apps"></a>Υπηρεσίες cloud και τα Web Apps
Μπορείτε να επιλέξετε στο από την πύλη του Azure για τις υπηρεσίες Cloud και συστοιχίες διακομιστών (Web Apps).

- Επιλέξτε τη μέτρηση **κλίμακα από** .

![κλιμακωθεί με](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## <a name="virtual-machine-scale-sets"></a>Σύνολα κλίμακα εικονική μηχανή
Για νεότερη εικονικές μηχανές δημιουργήθηκε με τη διαχείριση πόρων (σύνολα κλίμακα εικονική μηχανή), μπορείτε να το ρυθμίσετε χρησιμοποιώντας REST API, πρότυπα για τη διαχείριση πόρων, PowerShell και CLI. Περιβάλλον εργασίας πύλης δεν είναι ακόμη διαθέσιμη.
Όταν χρησιμοποιείτε το πρότυπο REST API ή τη διαχείριση πόρων, συμπεριλάβετε το στοιχείο ειδοποιήσεις με τις ακόλουθες επιλογές.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
|Πεδίο                              |Υποχρεωτικές; |Περιγραφή|
|---                                |---        |---|
|η λειτουργία                          |Ναι        |η τιμή πρέπει να είναι "Κλίμακα"|
|sendToSubscriptionAdministrator    |Ναι        |η τιμή πρέπει να είναι "true" ή "false"|
|sendToSubscriptionCoAdministrators |Ναι        |η τιμή πρέπει να είναι "true" ή "false"|
|customEmails                       |Ναι        |τιμή μπορεί να είναι null [] ή έναν πίνακα συμβολοσειρά των μηνυμάτων ηλεκτρονικού ταχυδρομείου|
|webhooks                           |Ναι        |τιμή μπορεί να είναι null ή έγκυρη Uri|
|serviceUri                         |Ναι        |μια έγκυρη https Uri|
|Ιδιότητες                         |Ναι        |τιμή πρέπει να είναι κενή {} ή μπορεί να περιέχει ζεύγη κλειδιού-τιμής|


## <a name="authentication-in-webhooks"></a>Έλεγχος ταυτότητας στο webhooks
Υπάρχουν δύο μορφές URI ελέγχου ταυτότητας:

1. Έλεγχος ταυτότητας διακριτικού βάσης, όπου μπορείτε να αποθηκεύσετε το webhook URI με διακριτικού Αναγνωριστικό ως παραμέτρου ερωτήματος. Για παράδειγμα, https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Βασικός έλεγχος ταυτότητας, όπου μπορείτε να χρησιμοποιήσετε ένα Αναγνωριστικό χρήστη και κωδικό πρόσβασης. Για παράδειγμα,https://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## <a name="autoscale-notification-webhook-payload-schema"></a>Autoscale ειδοποίηση webhook φορτίο σχήματος
Όταν δημιουργείτε την ειδοποίηση autoscale, τα παρακάτω μετα-δεδομένα περιλαμβάνεται στο φορτίο webhook:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|Πεδίο  |Υποχρεωτικές;|    Περιγραφή|
|---|---|---|
|κατάσταση |Ναι    |Την κατάσταση που υποδεικνύει ότι έχει δημιουργηθεί μια ενέργεια autoscale|
|η λειτουργία| Ναι |Για μια αύξηση των παρουσιών, θα είναι "Κλίμακα ανάληψη" και για μείωση των παρουσιών, να είναι "κλίμακα"|
|περιβάλλον|   Ναι |Το περιβάλλον autoscale ενέργειας|
|χρονική σήμανση| Ναι |Σήμανση χρόνου κατά την ενεργοποίηση της ενέργειας autoscale|
|αναγνωριστικό |Ναι|   Διαχείριση πόρων Αναγνωριστικό της ρύθμισης autoscale|
|Όνομα   |Ναι|   Το όνομα της ρύθμισης autoscale|
|λεπτομέρειες|   Ναι |Εξήγηση των την ενέργεια που εκτελέσατε την υπηρεσία autoscale και η αλλαγή στην καταμέτρηση παρουσίας|
|subscriptionId|    Ναι |Αναγνωριστικό συνδρομής του πόρου προορισμού στην οποία είναι που κλιμάκωση|
|resourceGroupName| Ναι|    Ομάδα πόρων όνομα του πόρου προορισμού που είναι που κλιμάκωση|
|resourceName   |Ναι|   Όνομα του πόρου προορισμού που είναι που κλιμάκωση|
|τύπου πόρου   |Ναι|   Τα τρία υποστηριζόμενες τιμές: "microsoft.classiccompute/domainnames/slots/roles" - ρόλους υπηρεσία Cloud, "microsoft.compute/virtualmachinescalesets" - εικονική μηχανή κλίμακα σύνολα, και "Microsoft.Web/serverfarms" - Web App|
|resourceId |Ναι|Διαχείριση πόρων Αναγνωριστικό του πόρου προορισμού στην οποία είναι που κλιμάκωση|
|portalLink |Ναι    |Azure πύλης σύνδεσης στη σελίδα σύνοψη του πόρου προορισμού|
|oldCapacity|   Ναι |Το τρέχον (παλιά) παρουσία πλήθος όταν Autoscale εκτελέσατε μια ενέργεια κλίμακα|
|newCapacity|   Ναι |Το νέο πλήθος παρουσία που Autoscale κλιμάκωση τον πόρο|
|Ιδιότητες|    Όχι| Προαιρετικό. Σύνολο < αριθμό-κλειδί, τιμή > ζεύγη (για παράδειγμα, λεξικό < συμβολοσειρά, συμβολοσειρά >). Το πεδίο "Ιδιότητες" είναι προαιρετικό. Σε ένα προσαρμοσμένο περιβάλλον εργασίας χρήστη ή λογικής ροής εργασίας εφαρμογής με βάση, μπορείτε να εισαγάγετε αριθμούς-κλειδιά και τις τιμές που μπορούν να περάσουν χρησιμοποιώντας το φορτίο. Ένας εναλλακτικός τρόπος για να μεταβιβάσετε προσαρμοσμένες ιδιότητες την εξερχόμενη κλήση webhook είναι να χρησιμοποιήσετε το webhook URI ίδια (ως παράμετροι ερωτήματος)|
