<properties
    pageTitle="Χρησιμοποιήστε το περιβάλλον γραμμής εντολών πλατφόρμες (CLI) για να δημιουργήσετε ειδοποιήσεις για τις υπηρεσίες του Azure | Microsoft Azure"
    description="Χρησιμοποιήστε το περιβάλλον γραμμής εντολών για να δημιουργήσετε Azure ειδοποιήσεις, η οποία μπορεί να ενεργοποιήσει τις ειδοποιήσεις και αυτοματισμού όταν πληρούνται οι συνθήκες που καθορίζετε."
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
    ms.date="10/24/2016"
    ms.author="robb"/>

# <a name="use-the-cross-platform-command-line-interface-cli-to-create-alerts-for-azure-services"></a>Χρησιμοποιήστε το περιβάλλον γραμμής εντολών πλατφόρμες (CLI) για να δημιουργήσετε ειδοποιήσεις για τις υπηρεσίες του Azure

> [AZURE.SELECTOR]
- [Πύλη](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο σάς δείχνει πώς μπορείτε να ρυθμίσετε τις ειδοποιήσεις Azure χρησιμοποιώντας το περιβάλλον γραμμής εντολών (CLI).

>[AZURE.NOTE] Azure οθόνη είναι το νέο όνομα για το τι ονομαζόταν "Ιδέες Azure" μέχρι 25ο Σεπτεμβρίου 2016. Ωστόσο, τα πεδία ονομάτων και, επομένως, τις παρακάτω εντολές εξακολουθεί να περιέχουν "ιδέες".

Μπορείτε να λάβετε μια ειδοποίηση που βασίζονται σε παρακολούθησης μετρήσεις για ή συμβάντα σε Azure των υπηρεσιών σας.

- **Μετρικό τιμές** - ειδοποίησης εναύσματα όταν η τιμή του ένα συγκεκριμένο μετρικό τέμνει μια οριακή τιμή αντιστοιχίζετε προς οποιαδήποτε κατεύθυνση. Αυτό σημαίνει ότι και τα δύο ενεργοποιεί κατά την πρώτη συνθήκη και, στη συνέχεια, στη συνέχεια όταν συνθήκη που είναι δεν πληρούνται πλέον.    
- **Δραστηριότητα καταγραφής συμβάντων** - μπορούν να ενεργοποιήσουν μια ειδοποίηση σε *κάθε* συμβάν ή, μόνο όταν υπάρχουν ορισμένα συμβάντα.

Μπορείτε να ρυθμίσετε μια ειδοποίηση ώστε να όταν ενεργοποιεί κάντε τα εξής:

- Αποστολή ειδοποιήσεων ηλεκτρονικού ταχυδρομείου στο διαχειριστή της υπηρεσίας και συνδιαχειριστών
- Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου σε επιπλέον μηνύματα ηλεκτρονικού ταχυδρομείου που καθορίζετε.
- κλήση μιας webhook
- Έναρξη εκτέλεσης της μια Azure runbook (μόνο από την πύλη του Azure αυτήν τη στιγμή)

Μπορείτε να ρυθμίσετε και να λάβετε πληροφορίες σχετικά με την ειδοποίηση κανόνων χρησιμοποιώντας

- [Πύλη του Azure](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [περιβάλλον γραμμής εντολών (CLI)](insights-alerts-command-line-interface.md)
- [Οθόνη Azure REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Μπορείτε πάντα να λάβετε Βοήθεια για τις εντολές, πληκτρολογώντας μια εντολή και τη θέση - Βοήθεια στο τέλος. Για παράδειγμα:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Δημιουργία ειδοποίησης κανόνων χρησιμοποιώντας το CLI

1. Εκτελέστε το τις προϋποθέσεις και να συνδεθείτε στη Azure. Ανατρέξτε στο θέμα [δείγματα CLI οθόνη Azure](insights-cli-samples.md). Με λίγα λόγια, εγκαταστήστε το CLI και εκτέλεση αυτών των εντολών. Αυτό να έχετε πραγματοποιήσει είσοδο, εμφάνιση συνδρομή που χρησιμοποιείτε και προετοιμασία να εκτελούν εντολές του Azure οθόνη.


    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2.  Για να παραθέσετε υπάρχοντες κανόνες σε μια ομάδα πόρων, χρησιμοποιήστε την παρακάτω φόρμα **azure ιδέες ειδοποιήσεις κανόνα λίστας** *[επιλογές] &lt;ομάδα πόρων&gt; *

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. Για να δημιουργήσετε έναν κανόνα, πρέπει να έχετε πολλές σημαντικές πληροφορίες πρώτα.
    - Το **Αναγνωριστικό πόρου** για τον πόρο που θέλετε να ορίσετε μια ειδοποίηση για
    - Οι **Ορισμοί μετρικό** διαθέσιμη για το συγκεκριμένο πόρο

    Ένας τρόπος για να λάβετε το Αναγνωριστικό πόρου είναι να χρησιμοποιήσουν την πύλη Azure. Υπό την προϋπόθεση ότι ο πόρος έχει ήδη δημιουργηθεί, επιλέξτε το από την πύλη. Στη συνέχεια, στην την επόμενη blade, επιλέξτε *Ιδιότητες* κάτω από την ενότητα *Ρυθμίσεις* . Το *Αναγνωριστικό ΠΌΡΟΥ* είναι ένα πεδίο σε την επόμενη blade. Ένας άλλος τρόπος είναι να χρησιμοποιήσετε την [Εξερεύνηση των πόρων Azure](https://resources.azure.com/).

    Είναι ένα παράδειγμα αναγνωριστικό πόρου για μια εφαρμογή web

    ```console
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Για να λάβετε μια λίστα με τις διαθέσιμες μετρήσεις και τις μονάδες για αυτές τις μετρήσεις για το προηγούμενο παράδειγμα πόρων, χρησιμοποιήστε την ακόλουθη εντολή CLI:  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
    ```

    _PT1M_ είναι το επίπεδο λεπτομερειών της διαθέσιμη μέτρησης (1-λεπτό διαστήματα). Χρησιμοποιώντας διαφορετική υποδιαιρέσεων παρέχει διάφορες επιλογές μετρικό.


4. Για να δημιουργήσετε έναν κανόνα ειδοποίησης που βασίζεται στο μετρικό σύστημα, χρησιμοποιήστε μια εντολή την εξής μορφή:

    **Ορισμός μετρικό σύστημα κανόνα ειδοποιήσεις Azure ιδέες** *[επιλογές] &lt;Όνομα_κανόνα&gt; &lt;θέση&gt; &lt;ομάδα πόρων&gt; &lt;windowSize&gt; &lt;τελεστή&gt; &lt;όριο&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt; *

    Το παρακάτω παράδειγμα ρυθμίζει μια ειδοποίηση σε έναν πόρο τοποθεσίας web. Η ειδοποίηση εναύσματα κάθε φορά που λαμβάνει με συνέπεια οποιαδήποτε κυκλοφορία για 5 λεπτά και ξανά όταν λάβει χωρίς κίνηση για 5 λεπτά.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```

5. Για να δημιουργήσετε webhook ή αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου όταν ενεργοποιείται μια ειδοποίηση, δημιουργήστε πρώτα το μήνυμα ηλεκτρονικού ταχυδρομείου ή/και webhooks. Στη συνέχεια, να δημιουργήσετε τον κανόνα αμέσως αργότερα. Δεν μπορείτε να συσχετίσετε webhook ή μηνύματα ηλεκτρονικού ταχυδρομείου με ήδη δημιουργήσει κανόνων χρησιμοποιώντας το CLI.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```


6. Για να δημιουργήσετε μια ειδοποίηση που ενεργοποιείται σε μια συγκεκριμένη συνθήκη στο αρχείο καταγραφής της δραστηριότητας, χρησιμοποιήστε τη μορφή:

    **κανόνας ειδοποιήσεις ιδέες καταγράψει συνόλου** *[επιλογές] &lt;Όνομα_κανόνα&gt; &lt;θέση&gt; &lt;ομάδα πόρων&gt; &lt;operationName&gt; *

    Για παράδειγμα

    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```

    Το operationName αντιστοιχεί σε έναν τύπο συμβάντων για μια καταχώρηση στο αρχείο καταγραφής της δραστηριότητας. Παραδείγματα *Microsoft.Compute/virtualMachines/delete* και *microsoft.insights/diagnosticSettings/write*.

    Μπορείτε να χρησιμοποιήσετε την εντολή PowerShell [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) για να αποκτήσετε μια λίστα με πιθανές operationNames. Εναλλακτικά, μπορείτε να χρησιμοποιήσετε την πύλη του Azure για το αρχείο καταγραφής της δραστηριότητας ερωτήματος και να βρείτε συγκεκριμένες πέρα από τις εργασίες που θέλετε να δημιουργήσετε μια ειδοποίηση για. Οι λειτουργίες που εμφανίζονται στην προβολή γραφικού καταγραφής φιλικό ονόματα. Διερεύνηση σε το JSON για την καταχώρηση και να αποσπάσετε την τιμή OperationName.   

7. Μπορείτε να επαληθεύσετε ότι τις ειδοποιήσεις σας έχουν δημιουργηθεί σωστά, εξετάζοντας έναν μεμονωμένο κανόνα.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```

8. Για να διαγράψετε τους κανόνες, χρησιμοποιήστε μια εντολή της φόρμας:

    **Διαγραφή κανόνα ειδοποιήσεις ιδέες** [επιλογές] &lt;ομάδα πόρων&gt; &lt;Όνομα_κανόνα&gt;

    Αυτές οι εντολές διαγράψτε τους κανόνες που δημιουργήσατε προηγουμένως σε αυτό το άρθρο.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```



## <a name="next-steps"></a>Επόμενα βήματα

* [Επισκόπηση του Azure παρακολούθηση](monitoring-overview.md) συμπεριλαμβανομένων τους τύπους των πληροφοριών που μπορεί να συλλέξετε και να παρακολουθήσετε.
* Μάθετε περισσότερα σχετικά με [τη ρύθμιση των παραμέτρων webhooks σε ειδοποιήσεις](insights-webhooks-alerts.md).
* Μάθετε περισσότερα σχετικά με το [Azure αυτοματισμού Runbooks](..\automation\automation-starting-a-runbook.md).
* Δείτε μια [Επισκόπηση της συλλογής αρχείων καταγραφής διαγνωστικών](monitoring-overview-of-diagnostic-logs.md) για τη συλλογή λεπτομερείς μετρικά υψηλής συχνότητας την υπηρεσία.
* Δείτε μια [Επισκόπηση της συλλογής μετρικά](insights-how-to-customize-monitoring.md) για να βεβαιωθείτε ότι η υπηρεσία σας είναι διαθέσιμο και αποκρίνεται.
