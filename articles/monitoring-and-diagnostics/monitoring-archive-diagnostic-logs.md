<properties
    pageTitle="Αρχειοθέτηση αρχείων καταγραφής διαγνωστικών Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να αρχειοθετήσετε σας Azure αρχεία καταγραφής διαγνωστικών για μακροπρόθεσμες διατήρησης σε ένα λογαριασμό του χώρου αποθήκευσης."
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
    ms.date="08/26/2016"
    ms.author="johnkem"/>

# <a name="archive-azure-diagnostic-logs"></a>Αρχειοθέτηση αρχείων καταγραφής διαγνωστικών Azure
Σε αυτό το άρθρο, θα σας δείξουμε πώς μπορείτε να χρησιμοποιήσετε την πύλη Azure, cmdlet του PowerShell, CLI ή REST API για την αρχειοθέτηση του [Azure αρχεία καταγραφής διαγνωστικών](monitoring-overview-of-diagnostic-logs.md) σε ένα λογαριασμό του χώρου αποθήκευσης. Αυτή η επιλογή είναι χρήσιμη αν θέλετε να διατηρήσετε τα αρχεία καταγραφής διαγνωστικών με μια πολιτική διατήρησης προαιρετική για ελέγχου, στατική ανάλυση ή δημιουργία αντιγράφων ασφαλείας.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Προτού ξεκινήσετε, πρέπει να [δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης](../storage/storage-create-storage-account.md#create-a-storage-account) στο οποίο μπορείτε να αρχειοθετείτε τα αρχεία καταγραφής διαγνωστικών. Προτείνουμε να μην χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης που περιλαμβάνει άλλες, μη παρακολούθηση δεδομένα που είναι αποθηκευμένα σε αυτό, ώστε να μπορείτε να ελέγχετε καλύτερα πρόσβαση σε δεδομένα παρακολούθησης. Ωστόσο, εάν επίσης αρχειοθετείτε το αρχείο καταγραφής δραστηριοτήτων και διαγνωστικών μετρήσεις σε ένα λογαριασμό του χώρου αποθήκευσης, μπορεί να νόημα να χρησιμοποιήσετε αυτόν το λογαριασμό χώρου αποθήκευσης για το αρχεία καταγραφής διαγνωστικών καθώς και για να διατηρήσετε όλα τα δεδομένα παρακολούθησης σε μια κεντρική θέση. Ο λογαριασμός χώρου αποθήκευσης που χρησιμοποιείτε πρέπει να είναι ένα λογαριασμό χώρου αποθήκευσης γενικής χρήσης, δεν ένα λογαριασμό χώρο αποθήκευσης αντικειμένων blob.

## <a name="diagnostic-settings"></a>Ρυθμίσεις διαγνωστικών
Για να αρχειοθετήσετε σας αρχεία καταγραφής διαγνωστικών χρησιμοποιώντας οποιαδήποτε από τις παρακάτω μεθόδους, μπορείτε να ορίσετε μια **Ρύθμιση διαγνωστικών** για ένα συγκεκριμένο πόρο. Μια διαγνωστικών ρύθμιση για έναν πόρο ορίζει τις κατηγορίες καταγράφει βρίσκονται που είναι αποθηκευμένα ή ροής και τα εξόδους — διανομέα λογαριασμό ή/και το συμβάν χώρου αποθήκευσης. Καθορίζει επίσης την πολιτική διατήρησης (αριθμός των ημερών που θέλετε να διατηρούνται) για συμβάντα κάθε κατηγορίας καταγραφής που είναι αποθηκευμένα σε ένα λογαριασμό του χώρου αποθήκευσης. Εάν μια πολιτική διατήρησης έχει οριστεί στην τιμή μηδέν, συμβάντα για αυτήν την κατηγορία καταγραφής αποθηκεύονται απεριόριστο χρονικό διάστημα (δηλαδή, δηλαδή για πάντα). Μια πολιτική διατήρησης αλλιώς μπορεί να είναι οποιοσδήποτε αριθμός των ημερών μεταξύ 1 και 2147483647. [Μπορείτε να διαβάσετε περισσότερα σχετικά με τις ρυθμίσεις διαγνωστικών εδώ](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).

## <a name="archive-diagnostic-logs-using-the-portal"></a>Αρχειοθέτηση αρχείων καταγραφής διαγνωστικών με την πύλη

1. Στην πύλη, κάντε κλικ στην επιλογή σε το blade πόρων για τον πόρο στην οποία θέλετε να ενεργοποιήσετε την αρχειοθέτησης των αρχείων καταγραφής διαγνωστικών.
2. Στην ενότητα **Παρακολούθηση** του μενού ρυθμίσεων πόρων, επιλέξτε **Διαγνωστικά**.

    ![Παρακολούθηση μέρος του μενού πόρων](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-sec.png)
3. Επιλέξτε το πλαίσιο ελέγχου για την **Εξαγωγή λογαριασμό χώρου αποθήκευσης**και, στη συνέχεια, επιλέξτε ένα λογαριασμό του χώρου αποθήκευσης. Προαιρετικά, ορίστε έναν αριθμό ημερών για να διατηρήσετε αυτά τα αρχεία καταγραφής χρησιμοποιώντας το **διατήρησης (ημέρες)** ρυθμιστικά. Μια διατήρηση του μηδέν ημέρες αποθηκεύει τα αρχεία καταγραφής απεριόριστο χρονικό διάστημα.

    ![Διαγνωστικών blade αρχείων καταγραφής](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-blade.png)
4. Κάντε κλικ στην επιλογή **Αποθήκευση**.

Αρχεία καταγραφής διαγνωστικών αρχειοθετούνται σε αυτόν το λογαριασμό χώρου αποθήκευσης, μόλις δημιουργείται νέα δεδομένα συμβάντος.

## <a name="archive-diagnostic-logs-via-the-powershell-cmdlets"></a>Αρχειοθέτηση αρχείων καταγραφής διαγνωστικών μέσω τα cmdlet του PowerShell

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Ιδιότητα         | Απαιτείται | Περιγραφή                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| ResourceId       | Ναι      | Αναγνωριστικό πόρου του πόρου στον οποίο θέλετε να ορίσετε μια ρύθμιση διαγνωστικών.                            |
| StorageAccountId | Όχι       | Αναγνωριστικό πόρου του λογαριασμού χώρου αποθήκευσης στην οποία θα πρέπει να αποθηκεύονται τα αρχεία καταγραφής διαγνωστικών.                          |
| Κατηγορίες       | Όχι       | Διαχωρισμένες με κόμματα λίστα κατηγοριών καταγραφής για την ενεργοποίηση.                                                     |
| Με δυνατότητα          | Ναι      | Τιμή Boolean που υποδεικνύει εάν Διαγνωστικά είναι ενεργοποιημένη ή απενεργοποιημένη σε αυτόν τον πόρο.                  |
| RetentionEnabled | Όχι       | Τιμή Boolean που υποδεικνύει εάν μια πολιτική διατήρησης είναι ενεργοποιημένη σε αυτόν τον πόρο.                            |
| RetentionInDays  | Όχι       | Αριθμός των ημερών για την οποία θα πρέπει να διατηρηθούν συμβάντα μεταξύ 1 και 2147483647. Η τιμή μηδέν αποθηκεύει τα αρχεία καταγραφής απεριόριστο χρονικό διάστημα. |

## <a name="archive-the-activity-log-via-the-cross-platform-cli"></a>Αρχειοθέτηση το αρχείο καταγραφής της δραστηριότητας μέσω του CLI πλατφόρμες

```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Ιδιότητα         | Απαιτείται | Περιγραφή                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| resourceId       | Ναι      | Αναγνωριστικό πόρου του πόρου στον οποίο θέλετε να ορίσετε μια ρύθμιση διαγνωστικών.                            |
| storageId        | Όχι       | Αναγνωριστικό πόρου του λογαριασμού χώρου αποθήκευσης στην οποία θα πρέπει να αποθηκεύονται τα αρχεία καταγραφής διαγνωστικών.                          |
| κατηγορίες       | Όχι       | Διαχωρισμένες με κόμματα λίστα κατηγοριών καταγραφής για την ενεργοποίηση.                                                     |
| με δυνατότητα          | Ναι      | Τιμή Boolean που υποδεικνύει εάν Διαγνωστικά είναι ενεργοποιημένη ή απενεργοποιημένη σε αυτόν τον πόρο.                  |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Αρχειοθέτηση αρχείων καταγραφής διαγνωστικών μέσω το REST API
[Δείτε αυτό το έγγραφο](https://msdn.microsoft.com/library/azure/dn931931.aspx) για πληροφορίες σχετικά με το πώς μπορείτε να ρυθμίσετε μια ρύθμιση διαγνωστικών χρησιμοποιώντας το Azure οθόνη REST API.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Σχήμα των αρχείων καταγραφής διαγνωστικών στο λογαριασμό χώρου αποθήκευσης
Αφού έχετε ρυθμίσει αρχειοθέτησης, ένα κοντέινερ χώρου αποθήκευσης δημιουργείται στο λογαριασμό χώρου αποθήκευσης με ένα συμβάν σε μία από τις κατηγορίες καταγραφής που έχετε ενεργοποιήσει. Τα αντικείμενα BLOB μέσα στο κοντέινερ, ακολουθήστε την ίδια μορφοποίηση σε αρχεία καταγραφής διαγνωστικών και το αρχείο καταγραφής της δραστηριότητας. Η δομή του αυτά τα αντικείμενα BLOB είναι:

> ιδέες - αρχεία καταγραφής-{όνομα κατηγορίας καταγραφής} / resourceId = / ΣΥΝΔΡΟΜΈΣ / {Αναγνωριστικό συνδρομής} /RESOURCEGROUPS/ {όνομα ομάδας πόρων} /PROVIDERS/ {όνομα πόρου παροχής} / {type πόρου} / {όνομα πόρου} / y = {τετραψήφια αριθμητική έτος} / m = {διψήφια αριθμητική μήνας} / d = {διψήφια αριθμητική ημέρα} / h = {hour}/m=00/PT1H.json διψήφιο 24-ωρης μορφής ώρας

Ή, πιο απλά,

> ιδέες - αρχεία καταγραφής-{όνομα κατηγορίας καταγραφής} / resourceId = / {πόρων αναγνωριστικό} / y = {τετραψήφια αριθμητική έτος} / m = {διψήφια αριθμητική μήνας} / d = {διψήφια αριθμητική ημέρα} / h = {hour}/m=00/PT1H.json διψήφιο 24-ωρης μορφής ώρας

Για παράδειγμα, ένα όνομα blob μπορεί να είναι:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json

Κάθε blob PT1H.json περιέχει ένα blob JSON συμβάντων που έγιναν κατά την ώρα που καθορίζεται στη διεύθυνση URL blob (για παράδειγμα, h = 12). Κατά την παρουσίαση ώρα, θα προσαρτηθούν συμβάντα στο αρχείο PT1H.json, καθώς προκύπτουν. Η τιμή minute (m = 00) είναι πάντα 00, επειδή το αρχείο καταγραφής διαγνωστικών συμβάντα είναι εσφαλμένος σε μεμονωμένα αντικείμενα BLOB ανά ώρα.

Μέσα στο αρχείο PT1H.json, κάθε συμβάντος αποθηκεύεται στον πίνακα "εγγραφές", ακολουθώντας αυτήν τη μορφή:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Όνομα στοιχείου  | Περιγραφή                                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------|
| ώρα          | Χρονική σήμανση όταν δημιουργήθηκε το συμβάν από την υπηρεσία Azure επεξεργασία της αίτησης που αντιστοιχεί στο συμβάν. |
| resourceId    | Αναγνωριστικό πόρου του επηρεαζόμενη πόρου.                                                                       |
| operationName | Όνομα της λειτουργίας.                                                                                      |
| κατηγορία      | Κατηγορία καταγραφής του συμβάντος.                                                                                  |
| Ιδιότητες    | Ορισμός της `<Key, Value>` ζεύγη (δηλαδή λεξικό) που περιγράφει τις λεπτομέρειες του συμβάντος.                            |

> [AZURE.NOTE] Η ιδιότητες και η χρήση της αυτές τις ιδιότητες μπορεί να διαφέρουν ανάλογα με τον πόρο.

## <a name="next-steps"></a>Επόμενα βήματα
- [Λήψη αντικείμενα BLOB για την ανάλυση](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Ροή αρχεία καταγραφής διαγνωστικών με διανομείς συμβάντος](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Διαβάστε περισσότερα σχετικά με τα αρχεία καταγραφής διαγνωστικών](monitoring-overview-of-diagnostic-logs.md)
