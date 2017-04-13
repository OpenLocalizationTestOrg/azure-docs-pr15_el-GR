<properties
    pageTitle="Κατακόρυφη κλιμάκωση σύνολα κλίμακα Azure εικονική μηχανή | Microsoft Azure"
    description="Τρόπος για να κλιμακωθεί κατακόρυφα μια εικονική μηχανή απάντηση παρακολούθηση ειδοποιήσεις με αυτοματοποίηση Azure"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="madhana"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="guybo"/>

# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Κατακόρυφη autoscale με σύνολα κλίμακα εικονική μηχανή

Σε αυτό το άρθρο περιγράφει τον τρόπο για να κλιμακωθεί κατακόρυφα Azure [Σύνολα κλίμακα εικονική μηχανή](https://azure.microsoft.com/services/virtual-machine-scale-sets/) με ή χωρίς reprovisioning. Για κατακόρυφη κλιμάκωση της ΣΠΣ που δεν βρίσκονται σε κλίμακα σύνολα, ανατρέξτε στην [κατακόρυφη κλιμάκωση Azure εικονική μηχανή με Azure αυτοματισμού](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md).

Κατακόρυφη κλιμάκωση, γνωστές και ως _κλιμάκωσης_ και _Κλιμάκωση προς τα κάτω_, σημαίνει ότι αύξουσες ή φθίνουσες μεγέθη εικονική μηχανή (Εικονική) ως απόκριση σε ένα φόρτο εργασίας. Συγκρίνετε αυτή με [οριζόντια κλίμακα](./virtual-machine-scale-sets-autoscale-overview.md), επίσης γνωστή ως _κλίμακα ανάληψη_ και _κλίμακας_, όπου ο αριθμός των ΣΠΣ έχει τροποποιηθεί ανάλογα με το φόρτο εργασίας.

Reprovisioning σημαίνει ότι η κατάργηση ενός υπάρχοντος Εικονική και να την αντικαταστήσετε με μια νέα. Όταν αύξηση ή μείωση του μεγέθους του ΣΠΣ σε ένα σύνολο κλίμακα Εικονική, σε ορισμένες περιπτώσεις που θέλετε να αλλαγή μεγέθους υπάρχοντα ΣΠΣ και να διατηρήσετε τα δεδομένα σας, ενώ σε άλλες περιπτώσεις πρέπει να αναπτύξετε το νέο ΣΠΣ από το νέο μέγεθος. Αυτό το έγγραφο καλύπτει δύο περιπτώσεις.

Κατακόρυφη κλιμάκωση μπορεί να είναι χρήσιμη όταν:

- Η υπηρεσία ενσωματωμένη σε εικονικές μηχανές είναι στην περιοχή χρησιμοποιούμενη (για παράδειγμα, κατά τα Σαββατοκύριακα). Μείωση του μεγέθους Εικονική να μειώσετε μηνιαίο κόστος.
- Αύξηση του μεγέθους Εικονική ώστε να αντιμετωπίσουν μεγαλύτερη ζήτηση χωρίς τη δημιουργία επιπλέον ΣΠΣ.

Μπορείτε να ρυθμίσετε το κατακόρυφη κλιμάκωση να που ενεργοποιούνται από βάσει μετρικό με βάση τις ειδοποιήσεις από την κλίμακα Εικονική το σύνολο. Όταν είναι ενεργοποιημένη η ειδοποίηση το ενεργοποιείται webhook μια συγκεκριμένη εναύσματα ένα runbook που μπορεί να κλιμακωθεί σας κλίμακα ρύθμιση προς τα επάνω ή προς τα κάτω. Κατακόρυφη κλιμάκωση μπορεί να ρυθμιστεί, ακολουθώντας τα παρακάτω βήματα:

1. Δημιουργήστε ένα λογαριασμό Azure αυτοματισμού με εκτέλεση ως δυνατότητα.
2. Εισαγωγή runbooks Azure αυτοματισμού κατακόρυφη κλίμακα για σύνολα κλίμακα Εικονική σε τη συνδρομή σας.
3. Προσθέστε μια webhook του runbook.
4. Προσθέστε μια ειδοποίηση για να την ορίσετε κλίμακα Εικονική χρησιμοποιώντας μια ειδοποίηση webhook.

> [AZURE.NOTE] Κατακόρυφη autoscaling μόνο μπορεί να γίνει μέσα σε ορισμένες περιοχές μεγέθη Εικονική. Συγκρίνετε τις προδιαγραφές του κάθε μεγέθους πριν αποφασίσετε για να κλιμακωθεί από μία σε ένα άλλο (μεγαλύτερο αριθμό μην υποδεικνύει πάντοτε μεγαλύτερο μέγεθος Εικονική). Μπορείτε να επιλέξετε για να κλιμακωθεί μεταξύ των παρακάτω ζευγών μεγέθη:

>| Εικονική μεγέθη κλίμακας ζεύγος |   |
|---|---|
|  Standard_A0 | Standard_A11 |
|  Standard_D1 |  Standard_D14 |
|  Standard_DS1 |  Standard_DS14 |
|  Standard_D1v2 |  Standard_D15v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Δημιουργήστε ένα λογαριασμό αυτοματισμού Azure με δυνατότητα εκτέλεσης ως

Το πρώτο πράγμα που πρέπει να κάνετε είναι να δημιουργήσει ένα λογαριασμό αυτοματισμού Azure που θα φιλοξενήσει το runbooks χρησιμοποιείται για να κλιμακωθεί τις παρουσίες Εικονική ρύθμιση κλίμακα. Πρόσφατα [Αυτοματισμού Azure](https://azure.microsoft.com/services/automation/) Παρουσιάστηκε η δυνατότητα "Εκτέλεση ως λογαριασμός", η οποία δημιουργεί τη ρύθμιση ασφαλείας του αρχικού υπηρεσία για την αυτόματη εκτέλεση του runbooks εκ μέρους ενός χρήστη πολύ εύκολη. Μπορείτε να διαβάσετε περισσότερα σχετικά με αυτό στο άρθρο παρακάτω:

* [Ο έλεγχος ταυτότητας Runbooks με Azure εκτελείται ως λογαριασμός](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Εισαγωγή runbooks Azure αυτοματισμού κατακόρυφη κλίμακα σε τη συνδρομή σας

Το runbooks που απαιτείται για την κατακόρυφη κλιμάκωση σας σύνολα κλίμακα Εικονική δημοσιεύονται ήδη στη συλλογή Runbook αυτοματισμού Azure. Για να εισαγάγετε τους στη συνδρομή σας, ακολουθήστε τα βήματα σε αυτό το άρθρο:

* [Συλλογές Runbook και λειτουργική μονάδα για την αυτοματοποίηση Azure](../automation/automation-runbook-gallery.md)

Ορίστε την επιλογή Αναζήτηση συλλογή από το μενού Runbooks:

![Runbooks προς εισαγωγή][runbooks]

Εμφανίζονται τα runbooks που πρέπει να εισαχθούν. Επιλέξτε runbook βάση εάν θέλετε κατακόρυφη κλιμάκωση με ή χωρίς reprovisioning:

![Συλλογή Runbooks][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Προσθήκη μιας webhook του runbook

Αφού εισαγάγετε το runbooks θα πρέπει να προσθέσετε μια webhook σε runbook, ώστε να μπορεί να ενεργοποιηθεί από μια ειδοποίηση από ένα σύνολο κλίμακα Εικονική. Σε αυτό το άρθρο περιγράφονται τις λεπτομέρειες της δημιουργίας ενός webhook για Runbook σας:

* [Azure webhooks αυτοματισμού](../automation/automation-webhooks.md)

> [AZURE.NOTE] Βεβαιωθείτε ότι μπορείτε να αντιγράψετε το webhook URI πριν να κλείσετε το παράθυρο διαλόγου webhook καθώς θα πρέπει στην επόμενη ενότητα.

## <a name="add-an-alert-to-your-vm-scale-set"></a>Προσθέστε μια ειδοποίηση για να την ορίσετε κλίμακα Εικονική

Ακολουθεί μια δέσμη ενεργειών του PowerShell που δείχνει πώς μπορείτε να προσθέσετε μια ειδοποίηση σε ένα σύνολο κλίμακα Εικονική. Ανατρέξτε στο παρακάτω άρθρο για να λάβετε το όνομα του τη μέτρηση για να ξεκινήσετε την ειδοποίηση στο: [κοινές μετρικά autoscaling Azure οθόνη](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] Συνιστάται να ρυθμίσετε τις παραμέτρους ενός παραθύρου λογικό ώρα της ειδοποίησης για να αποτρέψετε την ενεργοποίηση κατακόρυφη κλιμάκωση και διακοπή υπηρεσίας σχετίζονται, πολύ συχνά. Εξετάστε το ενδεχόμενο ένα παράθυρο του τουλάχιστον 20-30 λεπτά ή περισσότερο. Εξετάστε το ενδεχόμενο οριζόντια κλιμάκωση, αν πρέπει να αποφύγετε τις διακοπές.

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία ειδοποιήσεων, ανατρέξτε στα ακόλουθα άρθρα:

* [Azure δείγματα γρήγορης εκκίνησης του PowerShell οθόνη](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Δείγματα γρήγορης εκκίνησης Azure CLI πλατφόρμες οθόνη](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Σύνοψη

Σε αυτό το άρθρο εμφάνιζε απλή κατακόρυφη κλίμακας παραδείγματα. Με αυτά τα μπλοκ δόμησης - λογαριασμός αυτοματισμού, runbooks, webhooks, ειδοποιήσεις - μπορείτε να συνδεθείτε εμπλουτισμένη διάφορα συμβάντα με ένα σύνολο προσαρμοσμένων ενεργειών.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
