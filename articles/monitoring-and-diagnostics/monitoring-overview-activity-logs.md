<properties
    pageTitle="Επισκόπηση του αρχείου καταγραφής της δραστηριότητας των Azure | Microsoft Azure"
    description="Μάθετε τι είναι το αρχείο καταγραφής της δραστηριότητας Azure και πώς μπορείτε να το χρησιμοποιήσετε για να κατανοήσετε γεγονότα εντός Azure τη συνδρομή σας."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="johnkem"/>

# <a name="overview-of-the-azure-activity-log"></a>Επισκόπηση του αρχείου καταγραφής της δραστηριότητας Azure
**Αρχείο καταγραφής δραστηριοτήτων Azure** είναι ένα αρχείο καταγραφής που παρέχει πληροφορίες για τις εργασίες που έχουν πραγματοποιηθεί σε πόρους στη συνδρομή σας. Το αρχείο καταγραφής της δραστηριότητας ήταν προηγουμένως γνωστό ως "Αρχεία καταγραφής ελέγχου" ή "Λειτουργικές αρχείων καταγραφής", επειδή το να αναφέρει συμβάντα επιπέδου στοιχείου ελέγχου για τις συνδρομές σας. Χρησιμοποιώντας το αρχείο καταγραφής της δραστηριότητας, μπορείτε να προσδιορίσετε το ' Τι, ποιος, και πότε ' για οποιαδήποτε λειτουργίες (ΤΟΠΟΘΈΤΗΣΗ, ΔΗΜΟΣΊΕΥΣΗ, ΔΙΑΓΡΑΦΉ) που λαμβάνονται σχετικά με τους πόρους στη συνδρομή σας εγγραφής. Μπορείτε επίσης να κατανοήσετε την κατάσταση της λειτουργίας και άλλες σχετικές ιδιότητες. Το αρχείο καταγραφής της δραστηριότητας δεν περιλαμβάνει λειτουργίες ανάγνωσης (ΛΉΨΗ).

Το αρχείο καταγραφής της δραστηριότητας διαφέρει από [Αρχεία καταγραφής διαγνωστικών](./monitoring-overview-of-diagnostic-logs.md), που είναι όλα τα αρχεία καταγραφής που εκπέμπει από έναν πόρο. Αυτά τα αρχεία καταγραφής παρέχουν δεδομένα σχετικά με τη λειτουργία αυτού του πόρου, αντί για λειτουργίες σε αυτόν τον πόρο.

Μπορείτε να ανακτήσετε συμβάντα από το αρχείο καταγραφής δραστηριοτήτων με την πύλη Azure, CLI, cmdlet του PowerShell, και Azure οθόνη REST API.

Δείτε το [βίντεο εισαγωγή στο αρχείο καταγραφής της δραστηριότητας](https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz).  

## <a name="what-you-can-do-with-the-activity-log"></a>Τι μπορείτε να κάνετε με το αρχείο καταγραφής της δραστηριότητας
Ακολουθούν ορισμένες από τις ενέργειες που μπορείτε να κάνετε με το αρχείο καταγραφής της δραστηριότητας:

- Υποβολή ερωτήματος και προβάλετε στην **πύλη του Azure**.
- Ερώτημα το μέσω REST API, Cmdlet του PowerShell ή CLI.
- [Δημιουργία ειδοποίησης ηλεκτρονικού ταχυδρομείου ή webhook που αποτελεί έναυσμα για απενεργοποίηση ένα συμβάν καταγραφής δραστηριότητας.](./insights-auditlog-to-webhook-email.md)
- [Αποθηκεύστε το σε ένα **Λογαριασμό του χώρου αποθήκευσης** για αρχειοθέτηση ή μη αυτόματη επιθεώρηση](./monitoring-archive-activity-log.md). Μπορείτε να καθορίσετε το χρόνο διατήρησης (σε ημέρες) χρησιμοποιώντας **Προφίλ καταγραφής**.
- Αναλύσετε με χρήση του [**περιεχομένου πακέτου PowerBI**](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/)PowerBI.
- [Ροή την σε ένα **Συμβάν διανομέα** ](./monitoring-stream-activity-logs-event-hubs.md) για κατάποση από μια υπηρεσία άλλου κατασκευαστή ή μια λύση προσαρμοσμένης αναλυτικών στοιχείων όπως PowerBI.

## <a name="export-the-activity-log-with-log-profiles"></a>Εξαγωγή του αρχείου καταγραφής δραστηριότητας με προφίλ αρχείου καταγραφής
Ένα **Προφίλ αρχείου καταγραφής** ελέγχου τον τρόπο εξαγωγής των το αρχείο καταγραφής δραστηριοτήτων. Χρησιμοποιώντας ένα αρχείο καταγραφής προφίλ, μπορείτε να ρυθμίσετε:

- Όπου το αρχείο καταγραφής της δραστηριότητας πρέπει να σταλεί (λογαριασμού χώρου αποθήκευσης ή διανομείς συμβάντων)
- Πρέπει να σταλεί ποιες κατηγορίες συμβάντων (εγγραφή, διαγραφή, ενέργεια)
- Θα πρέπει να εξαχθούν ποιες περιοχές (θέσεις)
- Πόσο χρόνο πρέπει να διατηρηθεί το αρχείο καταγραφής της δραστηριότητας σε λογαριασμό χώρου αποθήκευσης – ένα διατήρηση του μηδέν ημέρες σημαίνει ότι θα τηρούνται για πάντα αρχεία καταγραφής. Διαφορετικά, η τιμή μπορεί να είναι οποιοσδήποτε αριθμός των ημερών μεταξύ 1 και 2147483647. Εάν έχουν οριστεί πολιτικές διατήρησης, αλλά την αποθήκευση αρχείων καταγραφής σε ένα λογαριασμό του χώρου αποθήκευσης είναι απενεργοποιημένη (για παράδειγμα, εάν μόνο διανομείς συμβάν ή OMS επιλογές είναι επιλεγμένο το στοιχείο), οι πολιτικές διατήρησης έχει κανένα αποτέλεσμα.

Αυτές οι ρυθμίσεις μπορούν να ρυθμιστούν μέσω την επιλογή "Εξαγωγή" στο το αρχείο καταγραφής δραστηριοτήτων blade στην πύλη. Μπορούν επίσης να έχει ρυθμιστεί μέσω προγραμματισμού, [χρησιμοποιώντας το Azure οθόνη REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx), cmdlet του PowerShell ή CLI. Μια συνδρομή μπορεί να έχει μόνο μία σύνδεση προφίλ.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Ρύθμιση παραμέτρων καταγραφής προφίλ με την πύλη Azure
Μπορείτε να ροή το αρχείο καταγραφής δραστηριοτήτων με ένα διανομέα συμβάν ή να αποθηκεύσετε σε ένα λογαριασμό του χώρου αποθήκευσης, χρησιμοποιώντας την επιλογή "Εξαγωγή" στην πύλη του Azure.

1. Μεταβείτε το **Αρχείο καταγραφής δραστηριοτήτων** blade χρησιμοποιώντας το μενού στην αριστερή πλευρά της πύλης.

    ![Μεταβείτε στο αρχείο καταγραφής της δραστηριότητας στην πύλη](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Κάντε κλικ στο κουμπί **Εξαγωγή** στο επάνω μέρος του blade.

    ![Κουμπί "Εξαγωγή" στην πύλη](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Στο το blade που εμφανίζεται, μπορείτε να επιλέξετε:  
    - περιοχές για την οποία θέλετε να εξαγάγετε συμβάντα
    - το λογαριασμό χώρου αποθήκευσης στο οποίο θέλετε να αποθηκεύσετε τα συμβάντα
    - ο αριθμός των ημερών που θέλετε να διατηρήσετε τα συμβάντα στο χώρο αποθήκευσης. Η ρύθμιση 0 ημέρες διατηρεί τα αρχεία καταγραφής για πάντα.
    - το Namespace Bus υπηρεσία στην οποία θα θέλατε ένα συμβάν διανομέα που θα δημιουργηθεί για ροή αυτά τα συμβάντα.

    ![Εξαγωγή blade αρχείο καταγραφής δραστηριοτήτων](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε αυτές τις ρυθμίσεις. Οι ρυθμίσεις εφαρμόζονται αμέσως στη συνδρομή σας.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Ρύθμιση παραμέτρων καταγραφής προφίλ που χρησιμοποιούν τα cmdlet του PowerShell Azure
#### <a name="get-existing-log-profile"></a>Λήψη υπάρχον αρχείο καταγραφής προφίλ
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Προσθέστε ένα προφίλ του αρχείου καταγραφής
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Ιδιότητα         | Απαιτείται | Περιγραφή   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Όνομα             | Ναι      | Όνομα του προφίλ σας στο αρχείο καταγραφής.                                 |
| StorageAccountId | Όχι       | Αναγνωριστικό πόρου του λογαριασμού χώρου αποθήκευσης στην οποία θα πρέπει να αποθηκευτεί το αρχείο καταγραφής της δραστηριότητας.                         |
| serviceBusRuleId | Όχι       | Αναγνωριστικό κανόνα Bus υπηρεσίας για το χώρο ονομάτων Bus υπηρεσία που θέλετε να έχετε διανομείς συμβάντος που δημιουργήθηκε στο. Είναι μια συμβολοσειρά με αυτήν τη μορφή: `{service bus resource ID}/authorizationrules/{key name}`. |
| Θέσεις        | Ναι      | Διαχωρισμένες με κόμματα λίστα περιοχές για την οποία θέλετε να συλλογή δραστηριότητα καταγραφής συμβάντων.              |
| RetentionInDays  | Ναι      | Αριθμός των ημερών για τα συμβάντα που θα πρέπει να διατηρηθούν, μεταξύ 1 και 2147483647. Η τιμή μηδέν αποθηκεύει τα αρχεία καταγραφής απεριόριστο χρονικό διάστημα (αιώνες). |
| Κατηγορίες       | Όχι       | Διαχωρισμένες με κόμματα λίστα κατηγοριών συμβάντων που θα πρέπει να συγκεντρώσει. Πιθανές τιμές είναι εγγραφής, διαγραφή και ενέργεια.                                 |

#### <a name="remove-a-log-profile"></a>Κατάργηση ενός προφίλ του αρχείου καταγραφής
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Ρύθμιση παραμέτρων προφίλ καταγραφής χρησιμοποιώντας το CLI πλατφόρμες Azure
#### <a name="get-existing-log-profile"></a>Λήψη υπάρχον αρχείο καταγραφής προφίλ
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
Το `name` ιδιότητα πρέπει να είναι το όνομα του προφίλ σας στο αρχείο καταγραφής.

#### <a name="add-a-log-profile"></a>Προσθέστε ένα προφίλ του αρχείου καταγραφής
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Ιδιότητα         | Απαιτείται | Περιγραφή   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Όνομα             | Ναι      | Όνομα του προφίλ σας στο αρχείο καταγραφής.                                 |
| storageId        | Όχι       | Αναγνωριστικό πόρου του λογαριασμού χώρου αποθήκευσης στην οποία θα πρέπει να αποθηκευτεί το αρχείο καταγραφής της δραστηριότητας.                         |
| serviceBusRuleId | Όχι       | Αναγνωριστικό κανόνα Bus υπηρεσίας για το χώρο ονομάτων Bus υπηρεσία που θέλετε να έχετε διανομείς συμβάντος που δημιουργήθηκε στο. Είναι μια συμβολοσειρά με αυτήν τη μορφή: `{service bus resource ID}/authorizationrules/{key name}`. |
| θέσεις        | Ναι      | Διαχωρισμένες με κόμματα λίστα περιοχές για την οποία θέλετε να συλλογή δραστηριότητα καταγραφής συμβάντων.              |
| retentionInDays  | Ναι      | Αριθμός των ημερών για τα συμβάντα που θα πρέπει να διατηρηθούν, μεταξύ 1 και 2147483647. Η τιμή μηδέν αποθηκεύει τα αρχεία καταγραφής απεριόριστο χρονικό διάστημα (αιώνες).     |
| κατηγορίες       | Όχι       | Διαχωρισμένες με κόμματα λίστα κατηγοριών συμβάντων που θα πρέπει να συγκεντρώσει. Πιθανές τιμές είναι εγγραφής, διαγραφή και ενέργεια.                                 |

#### <a name="remove-a-log-profile"></a>Κατάργηση ενός προφίλ του αρχείου καταγραφής
```
azure insights logprofile delete --name my_log_profile
```

## <a name="event-schema"></a>Σχήμα συμβάν
Κάθε συμβάντος στο αρχείο καταγραφής της δραστηριότητας περιλαμβάνει ένα blob JSON παρόμοιο με αυτό το παράδειγμα:

```
{
  "value": [ {
    "authorization": {
      "action": "microsoft.support/supporttickets/write",
      "role": "Subscription Admin",
      "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
    },
    "caller": "admin@contoso.com",
    "channels": "Operation",
    "claims": {
      "aud": "https://management.core.windows.net/",
      "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
      "iat": "1421876371",
      "nbf": "1421876371",
      "exp": "1421880271",
      "ver": "1.0",
      "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
      "puid": "20030000801A118C",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
      "name": "John Smith",
      "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
      "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.microsoft.com/claims/authnclassreference": "1"
    },
    "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "description": "",
    "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
    "eventName": {
      "value": "EndRequest",
      "localizedValue": "End request"
    },
    "eventSource": {
      "value": "Microsoft.Resources",
      "localizedValue": "Microsoft Resources"
    },
    "httpRequest": {
      "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
      "clientIpAddress": "192.168.35.115",
      "method": "PUT"
    },
    "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
    "level": "Informational",
    "resourceGroupName": "MSSupportGroup",
    "resourceProviderName": {
      "value": "microsoft.support",
      "localizedValue": "microsoft.support"
    },
    "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
    "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "operationName": {
      "value": "microsoft.support/supporttickets/write",
      "localizedValue": "microsoft.support/supporttickets/write"
    },
    "properties": {
      "statusCode": "Created"
    },
    "status": {
      "value": "Succeeded",
      "localizedValue": "Succeeded"
    },
    "subStatus": {
      "value": "Created",
      "localizedValue": "Created (HTTP Status Code: 201)"
    },
    "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
    "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
    "subscriptionId": "s1"
  } ],
"nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```

| Όνομα στοιχείου         | Περιγραφή             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| εξουσιοδότηση        | BLOB RBAC ιδιοτήτων του συμβάντος. Συνήθως περιλαμβάνει τις ιδιότητες "ενέργεια", "ρόλος" και "εύρος".|
| καλούντος               | Διεύθυνση ηλεκτρονικού ταχυδρομείου του χρήστη που έχει εκτελεστεί η λειτουργία, UPN διεκδίκηση ή με βάση τη διαθεσιμότητα διεκδίκηση κύριο όνομα Υπηρεσίας.|
| καναλιών             | Μία από τις παρακάτω τιμές: "Διαχειριστής", "Η λειτουργία"|
| correlationId        | Συνήθως ένα GUID σε μορφή συμβολοσειράς. Τα συμβάντα που μοιράζονται ένα correlationId ανήκει σε την ίδια ενέργεια uber.   |
| Περιγραφή          | Στατικό κείμενο περιγραφή του συμβάντος.                              |
| eventDataId          | Μοναδικό αναγνωριστικό του συμβάντος.    |
| Προέλευση_συμβάντος          | Το όνομα της υπηρεσίας Azure ή υποδομή που δημιούργησε αυτό το συμβάν.    |
| httpRequest          | BLOB που περιγράφει την αίτηση Http. Συνήθως περιλαμβάνει το "clientRequestId", "clientIpAddress" και "μέθοδος" (μέθοδος HTTP. ΤΟΠΟΘΈΤΗΣΗ for example,).                            |
| επίπεδο                | Επίπεδο του συμβάντος. Μία από τις παρακάτω τιμές: "Κρίσιμη", "Σφάλμα", "Προειδοποίηση", "Ενημερωτικό" και "Λεπτομερές"  |
| resourceGroupName    | Όνομα της ομάδας πόρων για τον πόρο επηρεαζόμενη.               |
| resourceProviderName | Όνομα της υπηρεσίας παροχής πόρων για τον πόρο επηρεαζόμενη             |
| resourceUri          | Αναγνωριστικό πόρου του επηρεαζόμενη πόρου.                               |
| operationId          | GUID κοινού από τα συμβάντα που αντιστοιχούν σε μία μόνο λειτουργία.         |
| operationName        | Όνομα της λειτουργίας.  |
| Ιδιότητες           | Ορισμός των `<Key, Value>` ζεύγη (δηλαδή, ένα λεξικό) που περιγράφει τις λεπτομέρειες του συμβάντος.                                |
| κατάσταση               | Η συμβολοσειρά που περιγράφει την κατάσταση της λειτουργίας. Ορισμένες κοινές τιμές είναι: Έναρξη, σε εξέλιξη, ολοκληρώθηκε με, απέτυχε, ενεργές, επιλύθηκε.                                |
| δευτερεύουσα κατάσταση            | Συνήθως ο κωδικός κατάστασης HTTP από τα ΥΠΌΛΟΙΠΑ αντίστοιχη κλήση, αλλά μπορεί επίσης να περιλαμβάνει άλλες συμβολοσειρές που περιγράφει μια δευτερεύουσα κατάσταση, όπως αυτές τις συνήθεις τιμές: OK (κωδικό κατάστασης HTTP: 200), που έχουν δημιουργηθεί (κωδικό κατάστασης HTTP: 201), αποδεκτή (κωδικό κατάστασης HTTP: 202), δεν περιεχομένου (κωδικό κατάστασης HTTP: 204), ακατάλληλη αίτηση (κωδικός κατάστασης HTTP: 400), δεν βρέθηκε (κωδικό κατάστασης HTTP: 404), διένεξης (κωδικό κατάστασης HTTP : 409), εσωτερικό σφάλμα διακομιστή (κωδικού κατάστασης HTTP: 500), υπηρεσία διαθέσιμη (κωδικού κατάστασης HTTP: 503), το χρονικό όριο πύλης (κωδικού κατάστασης HTTP: 504). |
| eventTimestamp       | Χρονική σήμανση όταν δημιουργήθηκε το συμβάν από την υπηρεσία Azure επεξεργασία της αίτησης που αντιστοιχεί στο συμβάν.     |
| submissionTimestamp  | Χρονική σήμανση κατά το συμβάν γίνεται διαθέσιμο για την υποβολή ερωτημάτων.             |
| subscriptionId       | Αναγνωριστικό συνδρομής Azure.  |
| nextLink             | Το διακριτικό συνέχισης για τη λήψη στο επόμενο σύνολο αποτελεσμάτων όταν αυτά θα διαιρεθεί σε πολλές απαντήσεις. Συνήθως απαραίτητη, όταν υπάρχουν περισσότερα από 200 εγγραφές.     |

## <a name="next-steps"></a>Επόμενα βήματα
- [Μάθετε περισσότερα σχετικά με το αρχείο καταγραφής της δραστηριότητας (παλαιότερα αρχεία καταγραφής ελέγχου)](../resource-group-audit.md)
- [Ροή το αρχείο καταγραφής Azure δραστηριοτήτων με διανομείς συμβάντος](./monitoring-stream-activity-logs-event-hubs.md)
