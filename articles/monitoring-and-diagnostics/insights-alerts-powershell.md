<properties
    pageTitle="Χρήση του PowerShell για τη δημιουργία ειδοποιήσεων για τις υπηρεσίες του Azure | Microsoft Azure"
    description="Χρήση του PowerShell για τη δημιουργία Azure ειδοποιήσεις, η οποία μπορεί να ενεργοποιήσει τις ειδοποιήσεις και αυτοματισμού όταν πληρούνται οι συνθήκες που καθορίζετε."
    authors="rboucher"
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
    ms.author="robb"/>

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Χρήση του PowerShell για τη δημιουργία ειδοποιήσεων για τις υπηρεσίες του Azure

> [AZURE.SELECTOR]
- [Πύλη](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Επισκόπηση

Αυτό το άρθρο σάς δείχνει πώς μπορείτε να ρυθμίσετε Azure ειδοποιήσεων με χρήση του PowerShell.  

Μπορείτε να λάβετε μια ειδοποίηση που βασίζονται σε παρακολούθησης μετρήσεις για ή συμβάντα σε Azure των υπηρεσιών σας.

- **Μετρικό τιμές** - ειδοποίησης εναύσματα όταν η τιμή του ένα συγκεκριμένο μετρικό τέμνει μια οριακή τιμή αντιστοιχίζετε προς οποιαδήποτε κατεύθυνση. Αυτό σημαίνει ότι και τα δύο ενεργοποιεί κατά την πρώτη συνθήκη και, στη συνέχεια, στη συνέχεια όταν συνθήκη που είναι δεν πληρούνται πλέον.    
- **Δραστηριότητα καταγραφής συμβάντων** - μπορούν να ενεργοποιήσουν μια ειδοποίηση σε *κάθε* συμβάν ή, μόνο όταν υπάρχουν ορισμένα συμβάντα.

Μπορείτε να ρυθμίσετε μια ειδοποίηση ώστε να όταν ενεργοποιεί κάντε τα εξής:

- Αποστολή ειδοποιήσεων ηλεκτρονικού ταχυδρομείου στο διαχειριστή της υπηρεσίας και συνδιαχειριστών
- Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου σε επιπλέον μηνύματα ηλεκτρονικού ταχυδρομείου που καθορίζετε.
- κλήση μιας webhook
- Έναρξη εκτέλεσης της μια Azure runbook (μόνο από την πύλη του Azure)

Μπορείτε να ρυθμίσετε και να λάβετε πληροφορίες σχετικά με την ειδοποίηση κανόνων χρησιμοποιώντας

- [Πύλη του Azure](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [περιβάλλον γραμμής εντολών (CLI)](insights-alerts-command-line-interface.md)
- [Οθόνη Azure REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Για περισσότερες πληροφορίες, μπορείτε πάντα να πληκτρολογήσετε ```get-help``` και, στη συνέχεια, την εντολή PowerShell που θέλετε βοήθεια.

## <a name="create-alert-rules-in-powershell"></a>Δημιουργία ειδοποίησης κανόνων σε PowerShell

1. Συνδεθείτε στο Azure.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Λάβετε μια λίστα από τις συνδρομές που είναι διαθέσιμες. Βεβαιωθείτε ότι εργάζεστε με τη σωστή συνδρομή. Εάν δεν είναι, ρυθμίστε την σε τον σωστό τομέα χρησιμοποιώντας το αποτέλεσμα από `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  Για να παραθέσετε υπάρχοντες κανόνες σε μια ομάδα πόρων, χρησιμοποιήστε την ακόλουθη εντολή:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. Για να δημιουργήσετε έναν κανόνα, πρέπει να έχετε πολλές σημαντικές πληροφορίες πρώτα. 
    - Το **Αναγνωριστικό πόρου** για τον πόρο που θέλετε να ορίσετε μια ειδοποίηση για
    - Οι **Ορισμοί μετρικό** διαθέσιμη για το συγκεκριμένο πόρο

    Ένας τρόπος για να λάβετε το Αναγνωριστικό πόρου είναι να χρησιμοποιήσουν την πύλη Azure. Υπό την προϋπόθεση ότι ο πόρος έχει ήδη δημιουργηθεί, επιλέξτε το από την πύλη. Στη συνέχεια, στην την επόμενη blade, επιλέξτε *Ιδιότητες* κάτω από την ενότητα *Ρυθμίσεις* . Το Αναγνωριστικό ΠΌΡΟΥ είναι ένα πεδίο σε την επόμενη blade. Ένας άλλος τρόπος είναι να χρησιμοποιήσετε την [Εξερεύνηση των πόρων Azure](https://resources.azure.com/).

    Είναι ένα παράδειγμα αναγνωριστικό πόρου για μια εφαρμογή web

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Μπορείτε να χρησιμοποιήσετε `Get-AzureRmMetricDefinition` για να προβάλετε τη λίστα όλων των ορισμών μετρικό για ένα συγκεκριμένο πόρο.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    Το παρακάτω παράδειγμα δημιουργεί έναν πίνακα με τη μέτρηση ονόματος και της μονάδας για το συγκεκριμένο μετρικό σύστημα.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    Μια πλήρη λίστα των διαθέσιμων επιλογών για λήψη AzureRmMetricDefinition είναι διαθέσιμη, εκτελώντας Get-MetricDefinitions.


5. Το παρακάτω παράδειγμα ρυθμίζει μια ειδοποίηση σε έναν πόρο τοποθεσίας web. Η ειδοποίηση εναύσματα κάθε φορά που λαμβάνει με συνέπεια οποιαδήποτε κυκλοφορία για 5 λεπτά και ξανά όταν λάβει χωρίς κίνηση για 5 λεπτά.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. Για να δημιουργήσετε webhook ή αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου όταν ενεργοποιεί μια ειδοποίηση, δημιουργήστε πρώτα το μήνυμα ηλεκτρονικού ταχυδρομείου ή/και webhooks. Στη συνέχεια, αμέσως δημιουργήστε τον κανόνα αργότερα με την - ετικέτα ενέργειες και όπως φαίνεται στο παρακάτω παράδειγμα. Δεν μπορείτε να συσχετίσετε webhook ή μηνύματα ηλεκτρονικού ταχυδρομείου με ήδη δημιουργήσει κανόνες μέσω του PowerShell.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. Για να δημιουργήσετε μια ειδοποίηση που αποτελεί έναυσμα για μια συγκεκριμένη συνθήκη στο αρχείο καταγραφής της δραστηριότητας, χρησιμοποιήστε τις εντολές της παρακάτω φόρμας

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    Το OperationName - αντιστοιχεί σε έναν τύπο συμβάντος στο αρχείο καταγραφής της δραστηριότητας. Παραδείγματα *Microsoft.Compute/virtualMachines/delete* και *microsoft.insights/diagnosticSettings/write*.

    Μπορείτε να χρησιμοποιήσετε την εντολή PowerShell [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) για να αποκτήσετε μια λίστα με πιθανές operationNames. Εναλλακτικά, μπορείτε να χρησιμοποιήσετε την πύλη του Azure για το αρχείο καταγραφής της δραστηριότητας ερωτήματος και να βρείτε συγκεκριμένες πέρα από τις εργασίες που θέλετε να δημιουργήσετε μια ειδοποίηση για. Οι λειτουργίες που εμφανίζονται στην προβολή γραφικού καταγραφής φιλικό ονόματα. Διερεύνηση σε το JSON για την καταχώρηση και να αποσπάσετε την τιμή OperationName.   

8. Βεβαιωθείτε ότι τις ειδοποιήσεις σας έχουν δημιουργηθεί σωστά, εξετάζοντας τους κανόνες μεμονωμένα.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Διαγραφή των ειδοποιήσεων. Αυτές οι εντολές διαγράψτε τους κανόνες που δημιουργήσατε προηγουμένως σε αυτό το άρθρο.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Επόμενα βήματα

* [Επισκόπηση του Azure παρακολούθηση](monitoring-overview.md) συμπεριλαμβανομένων τους τύπους των πληροφοριών που μπορεί να συλλέξετε και να παρακολουθήσετε.
* Μάθετε περισσότερα σχετικά με [τη ρύθμιση των παραμέτρων webhooks σε ειδοποιήσεις](insights-webhooks-alerts.md).
* Μάθετε περισσότερα σχετικά με το [Azure αυτοματισμού Runbooks](..\automation\automation-starting-a-runbook.md).
* Δείτε μια [Επισκόπηση της συλλογής αρχείων καταγραφής διαγνωστικών](monitoring-overview-of-diagnostic-logs.md) για τη συλλογή λεπτομερείς μετρικά υψηλής συχνότητας την υπηρεσία.
* Δείτε μια [Επισκόπηση της συλλογής μετρικά](insights-how-to-customize-monitoring.md) για να βεβαιωθείτε ότι η υπηρεσία σας είναι διαθέσιμο και αποκρίνεται.
