<properties
    pageTitle="Ροή Azure αρχεία καταγραφής διαγνωστικών με διανομείς συμβάν | Microsoft Azure"
    description="Μάθετε πώς να ροή Azure αρχεία καταγραφής διαγνωστικών με διανομείς συμβάν."
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
    ms.date="08/08/2016"
    ms.author="johnkem"/>

# <a name="stream-azure-diagnostic-logs-to-event-hubs"></a>Ροή Azure αρχεία καταγραφής διαγνωστικών με διανομείς συμβάντος

**[Αρχεία καταγραφής διαγνωστικών Azure](monitoring-overview-of-diagnostic-logs.md)** είναι δυνατό να διοχετεύονται σε άμεσο πραγματικό χρόνο σε οποιαδήποτε εφαρμογή χρησιμοποιώντας την επιλογή "Εξαγωγή σε συμβάν διανομείς" ενσωματωμένο στην πύλη ή ενεργοποιώντας το αναγνωριστικό υπηρεσίας Bus κανόνα σε μια ρύθμιση διαγνωστικών μέσω των cmdlet του PowerShell Azure ή Azure CLI.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Τι μπορείτε να κάνετε με τα αρχεία καταγραφής διαγνωστικών και διανομείς συμβάντος
Εδώ θα βρείτε ορισμένους τρόπους που μπορείτε να χρησιμοποιήσετε τη δυνατότητα ροής για αρχεία καταγραφής διαγνωστικών:

- **Αρχεία καταγραφής ροής άλλων κατασκευαστών καταγραφή και τα συστήματα τηλεμετρίας** – διάρκεια του χρόνου, διανομείς συμβάν ροής θα μετατραπεί σε το μηχανισμό να διοχέτευση σας αρχεία καταγραφής διαγνωστικών σε SIEMs τρίτων και να συνδεθείτε ανάλυση λύσεις.

- **Προβολή εύρυθμης λειτουργίας υπηρεσιών με ροή δεδομένων "συντόμευσης διαδρομή" για να PowerBI** – χρήση διανομείς συμβάντος, ροή ανάλυση και PowerBI, μπορείτε να μετατρέψετε εύκολα σας δεδομένα Διαγνωστικά σε κοντά σε πραγματικό χρόνο ιδέες σχετικά με τις υπηρεσίες Azure. [Τεκμηρίωση σε αυτό το άρθρο παρέχει μια εξαιρετική επισκόπηση των πώς μπορείτε να ρυθμίσετε ένα συμβάν διανομείς, να επεξεργάζονται δεδομένα με ροή ανάλυση, και να χρησιμοποιήσετε PowerBI ως το αποτέλεσμα](../stream-analytics/stream-analytics-power-bi-dashboard.md). Εδώ θα βρείτε μερικές συμβουλές για τις ρυθμίσεις με αρχεία καταγραφής διαγνωστικών:
    - Οι διανομείς συμβάντων για μια κατηγορία των αρχείων καταγραφής διαγνωστικών δημιουργείται αυτόματα όταν ενεργοποιήσετε την επιλογή στην πύλη ή ενεργοποίηση μέσω του PowerShell, ώστε να θέλετε να επιλέξετε το συμβάν διανομείς στο χώρο ονομάτων Bus υπηρεσίας με το όνομα που ξεκινά με "ιδέες-"
    - Ακολουθεί δείγμα ερωτήματος ανάλυσης ροή μπορείτε να χρησιμοποιήσετε για την ανάλυση απλώς όλα τα καταγραφής δεδομένα σε έναν πίνακα PowerBI:

```
SELECT
records.ArrayValue.[Properties you want to track]
INTO
[OutputSourceName – the PowerBI source]
FROM
[InputSourceName] AS e
CROSS APPLY GetArrayElements(e.records) AS records
```

- **Δημιουργήστε μια προσαρμοσμένη τηλεμετρίας και καταγραφή από την πλατφόρμα** – εάν έχετε ήδη μια πλατφόρμα συγκεκριμένου τηλεμετρίας ή είναι απλώς υπόψη σχετικά με τη δημιουργία μία, ιδιαίτερα με δημοσίευσης εγγραφής φύση του συμβάντος διανομείς σάς επιτρέπει να ευελιξία ingest αρχεία καταγραφής διαγνωστικών. [Ανατρέξτε στο θέμα Dan Rosanova του οδηγού για χρήση διανομείς συμβάν σε μια πλατφόρμα τηλεμετρίας παγκόσμια κλίμακα εδώ](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Ενεργοποίηση της ροής των αρχείων καταγραφής διαγνωστικών
Μπορείτε να ενεργοποιήσετε ροής των αρχείων καταγραφής διαγνωστικών μέσω προγραμματισμού, μέσω της πύλης, ή με χρήση του [Azure οθόνη REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx). Σε κάθε περίπτωση, μπορείτε να επιλέξετε μια υπηρεσία Namespace Bus και ένα συμβάν διανομείς δημιουργείται στο χώρο ονομάτων για κάθε κατηγορία καταγραφής ενεργοποιήσετε. **Κατηγορία καταγραφής** διαγνωστικών είναι ένας τύπος αρχείου καταγραφής που ενδέχεται να συλλέγει έναν πόρο. Μπορείτε να επιλέξετε ποιες κατηγορίες καταγραφής που θέλετε να συλλέξετε για ένα συγκεκριμένο πόρο στην πύλη του Azure στην περιοχή τα Διαγνωστικά blade.

![Αρχείο καταγραφής κατηγορίες στην πύλη](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [AZURE.WARNING] Ενεργοποίηση και ροή αρχεία καταγραφής διαγνωστικών από τους πόρους (για παράδειγμα, ΣΠΣ ή υπηρεσία ύφασμα) υπολογισμού και [απαιτεί ένα διαφορετικό σύνολο βημάτων](../event-hubs/event-hubs-streaming-azure-diags-data.md).

### <a name="via-powershell-cmdlets"></a>Μέσω των cmdlet του PowerShell
Για να ενεργοποιηθεί η ροή μέσω τα [Cmdlet του PowerShell Azure](insights-powershell-samples.md), μπορείτε να χρησιμοποιήσετε το `Set-AzureRmDiagnosticSetting` cmdlet με αυτές τις παραμέτρους:

```
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

Το Αναγνωριστικό υπηρεσίας Bus κανόνας είναι μια συμβολοσειρά με αυτήν τη μορφή: `{service bus resource ID}/authorizationrules/{key name}`, για παράδειγμα, `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{service bus namespace}/authorizationrules/RootManageSharedAccessKey`.


### <a name="via-azure-cli"></a>Μέσω Azure CLI
Για να ενεργοποιηθεί η ροή μέσω του [Azure CLI](insights-cli-samples.md), μπορείτε να χρησιμοποιήσετε το `insights diagnostic set` εντολής ως εξής:

```
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Χρησιμοποιήστε την ίδια μορφοποίηση για το Αναγνωριστικό κανόνα Bus υπηρεσία, όπως εξηγείται για το Cmdlet του PowerShell.

###<a name="via-azure-portal"></a>Μέσω Azure πύλη
Για να ενεργοποιηθεί η ροή μέσω της πύλης Azure, μεταβείτε στις ρυθμίσεις Διαγνωστικά του πόρου και επιλέξτε 'Εξαγωγή σε συμβάν διανομέα'.

![Εξαγωγή σε διανομείς συμβάντος στην πύλη](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Για να ρυθμίσετε τις παραμέτρους της, επιλέξτε μια υπάρχουσα Namespace Bus υπηρεσίας. Ο χώρος ονομάτων επιλεγμένο θα όπου δημιουργείται (Εάν πρόκειται για την πρώτη φορά ροής αρχεία καταγραφής διαγνωστικών) ή ροής προς το συμβάν διανομείς (εάν υπάρχουν ήδη πόρους που η ροή αυτή την κατηγορία καταγραφής σε αυτόν το χώρο ονομάτων), και η πολιτική καθορίζει τα δικαιώματα που έχει το μηχανισμό ροής. Σήμερα, η ροή με ένα συμβάν διανομείς απαιτεί δικαιώματα διαχείριση, την ανάγνωση και αποστολή. Μπορείτε να δημιουργήσετε ή να τροποποιήσετε πολιτικές πρόσβασης υπηρεσίας Bus Namespace θέσει σε κοινή χρήση στην πύλη του κλασική κάτω από την καρτέλα "Ρύθμιση παραμέτρων" για την υπηρεσία Namespace Bus. Για να ενημερώσετε μία από αυτές τις ρυθμίσεις διαγνωστικών, ο υπολογιστής-πελάτης πρέπει να έχετε το δικαίωμα ListKey του κανόνα εξουσιοδότησης Bus υπηρεσίας.

##<a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Πώς μπορώ να εκμετάλλευση τα δεδομένα του αρχείου καταγραφής από το συμβάν διανομείς;
Ακολουθεί δείγμα δεδομένων εξόδου από το συμβάν διανομείς:

```
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Όνομα στοιχείου | Περιγραφή                                            |
|--------------|--------------------------------------------------------|
|εγγραφές       | Ένας πίνακας με όλα τα συμβάντα καταγραφής σε αυτό το φορτίο.            |
|ώρα          | Ώρα κατά την οποία παρουσιάστηκε το συμβάν.                      |
|κατηγορία      | Κατηγορία καταγραφής για αυτό το συμβάν.                           |
|resourceId    | Αναγνωριστικό πόρου του πόρου που δημιούργησε αυτό το συμβάν. |
|operationName | Όνομα της λειτουργίας.                                 |
|επίπεδο         | Προαιρετικό. Υποδεικνύει το επίπεδο καταγραφής συμβάντων.               |
|Ιδιότητες    | Ιδιότητες του συμβάντος.                               |


Μπορείτε να προβάλετε μια λίστα με όλες τις υπηρεσίες παροχής πόρων που υποστηρίζουν ροή διανομέα συμβάν [εδώ](monitoring-overview-of-diagnostic-logs.md).

##<a name="next-steps"></a>Επόμενα βήματα
- [Διαβάστε περισσότερα σχετικά με τα αρχεία καταγραφής διαγνωστικών Azure](monitoring-overview-of-diagnostic-logs.md)
- [Γρήγορα αποτελέσματα με το συμβάν διανομείς](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
