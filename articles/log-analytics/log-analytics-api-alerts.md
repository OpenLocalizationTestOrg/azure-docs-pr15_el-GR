<properties
   pageTitle="Αρχείο καταγραφής αναλυτικών στοιχείων ειδοποίησης REST API"
   description="Το αρχείο καταγραφής ανάλυσης ειδοποίησης REST API σάς επιτρέπει να δημιουργήσετε και να διαχειριστείτε τις ειδοποιήσεις στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS).  Σε αυτό το άρθρο παρέχει λεπτομέρειες σχετικά με το API και διάφορα παραδείγματα για την εκτέλεση διαφορετικές λειτουργίες."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="log-analytics-alert-rest-api"></a>Αρχείο καταγραφής αναλυτικών στοιχείων ειδοποίησης REST API

Το αρχείο καταγραφής ανάλυσης ειδοποίησης REST API σάς επιτρέπει να δημιουργήσετε και να διαχειριστείτε τις ειδοποιήσεις στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS).  Σε αυτό το άρθρο παρέχει λεπτομέρειες σχετικά με το API και διάφορα παραδείγματα για την εκτέλεση διαφορετικές λειτουργίες.

Το αρχείο καταγραφής αναλυτικών στοιχείων Search REST API είναι RESTful και είναι δυνατή η πρόσβαση μέσω του Azure διαχείριση πόρων REST API. Σε αυτό το έγγραφο θα βρείτε παραδείγματα όπου το API είναι προσβάσιμη από τη γραμμή εντολών του PowerShell χρησιμοποιώντας [ARMClient](https://github.com/projectkudu/ARMClient), ένα εργαλείο γραμμής εντολών Άνοιγμα αρχείου προέλευσης που απλοποιεί την κλήση του Azure API διαχείρισης πόρων. Η χρήση των ARMClient και του PowerShell είναι μία από πολλές επιλογές για να αποκτήσετε πρόσβαση το API καταγραφής αναλυτικών στοιχείων αναζήτησης. Με αυτά τα εργαλεία που μπορούν να χρησιμοποιήσουν το RESTful API διαχείρισης πόρων Azure για να κάνετε κλήσεις σε χώρους εργασίας OMS και εκτέλεση εντολών αναζήτησης μέσα σε αυτά. Το API θα εξόδου αποτελεσμάτων αναζήτησης σε εσάς σε μορφή JSON, επιτρέποντάς σας να χρησιμοποιήσετε τα αποτελέσματα αναζήτησης με πολλούς διαφορετικούς τρόπους μέσω προγραμματισμού.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Προς το παρόν, οι ειδοποιήσεις μπορούν να δημιουργηθούν μόνο με μια αποθηκευμένη αναζήτηση στο αρχείο καταγραφής ανάλυσης.  Μπορείτε να ανατρέξετε στο [Αρχείο καταγραφής Search REST API](log-analytics-log-search-api.md) για περισσότερες πληροφορίες.

## <a name="schedules"></a>Χρονοδιαγράμματα
Μια αποθηκευμένη αναζήτηση μπορεί να έχει ένα ή περισσότερα προγράμματα. Το χρονοδιάγραμμα Καθορίζει πόσο συχνά αναζήτηση είναι εκτέλεση και το χρονικό διάστημα, βάσει του οποίου έχει προσδιοριστεί τα κριτήρια.
Χρονοδιαγράμματα έχουν τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα  | Περιγραφή |
|:--|:--|
| Χρονικό διάστημα | Πόσο συχνά εκτελείται η αναζήτηση. Μετράται σε λεπτά. |
| QueryTimeSpan | Το χρονικό διάστημα βάσει του οποίου αξιολογείται τα κριτήρια. Πρέπει να είναι ίση ή μεγαλύτερη από διάστημα. Μετράται σε λεπτά. |
| Έκδοση | Η έκδοση API που χρησιμοποιείται.  Προς το παρόν, αυτό θα πρέπει πάντα να οριστεί σε 1. |

Για παράδειγμα, εξετάστε το ενδεχόμενο να ένα ερώτημα συμβάντος με ένα χρονικό διάστημα από 15 λεπτά και ένα χρονικό διάστημα από 30 λεπτά. Σε αυτήν την περίπτωση, το ερώτημα θα εκτελείται κάθε 15 λεπτά, και μια ειδοποίηση μπορεί να ενεργοποιείται εάν συνεχίζεται τα κριτήρια για την επίλυση σε true σε ένα διάστημα 30 λεπτά.

### <a name="retrieving-schedules"></a>Ανάκτηση χρονοδιαγράμματα
Χρησιμοποιήστε τη μέθοδο Get για την ανάκτηση όλων των χρονοδιαγραμμάτων για μια αποθηκευμένη αναζήτηση.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Χρησιμοποιήστε τη μέθοδο Get με μια Ταυτότητα χρονοδιάγραμμα για την ανάκτηση ενός συγκεκριμένου χρονοδιαγράμματος για μια αποθηκευμένη αναζήτηση.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Ακολουθεί μια απάντηση δείγμα για ένα χρονοδιάγραμμα.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>Δημιουργία ενός χρονοδιαγράμματος
Χρησιμοποιήστε τη μέθοδο τοποθέτηση με χρονοδιάγραμμα μοναδικό Αναγνωριστικό για να δημιουργήσετε ένα νέο χρονοδιάγραμμα.  Σημειώστε ότι δύο χρονοδιαγράμματα δεν είναι δυνατό να έχουν το ίδιο Αναγνωριστικό ακόμα και αν είναι συσχετισμένες με διαφορετικό αποθηκευμένες αναζητήσεις.  Όταν δημιουργείτε ένα χρονοδιάγραμμα στην κονσόλα OMS, δημιουργείται ένα GUID για το αναγνωριστικό του χρονοδιαγράμματος.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Επεξεργασία χρονοδιαγράμματος
Χρησιμοποιήστε τη μέθοδο τοποθέτηση με ένα υπάρχον Αναγνωριστικό χρονοδιάγραμμα για την ίδια αποθηκευμένη αναζήτηση για να τροποποιήσετε που χρονοδιάγραμμα.  Στο σώμα της πρόσκλησης σε πρέπει να περιλαμβάνει το etag του χρονοδιαγράμματος.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Διαγραφή χρονοδιαγράμματα
Χρησιμοποιήστε τη μέθοδο Delete με μια Ταυτότητα χρονοδιάγραμμα για να διαγράψετε ένα χρονοδιάγραμμα.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Ενέργειες
Ένα χρονοδιάγραμμα μπορεί να έχει πολλές ενέργειες. Μια ενέργεια μπορεί να καθορίσετε μία ή περισσότερες διαδικασίες για να εκτελέσετε όπως αποστολή αλληλογραφίας ή ξεκινώντας μια runbook ή μπορεί να καθορίζει μια οριακή τιμή που καθορίζει πότε τα αποτελέσματα μιας αναζήτησης ικανοποιούν ορισμένα κριτήρια.  Ορισμένες ενέργειες θα καθορίσει και τα δύο, έτσι ώστε οι διεργασίες που εκτελούνται όταν ικανοποιείται το όριο.

Όλες οι ενέργειες έχουν τις ιδιότητες στον παρακάτω πίνακα.  Διαφορετικούς τύπους ειδοποιήσεων έχουν διαφορετικές πρόσθετες ιδιότητες που περιγράφονται παρακάτω.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| Τύπος | Τύπος της ενέργειας.  Προς το παρόν, οι πιθανές τιμές είναι ειδοποίησης και Webhook. |
| Όνομα | Εμφανιζόμενο όνομα για την ειδοποίηση. |
| Έκδοση | Η έκδοση API που χρησιμοποιείται.  Προς το παρόν, αυτό θα πρέπει πάντα να οριστεί σε 1. |

### <a name="retrieving-actions"></a>Ανάκτηση ενέργειες
Χρησιμοποιήστε τη μέθοδο Get για την ανάκτηση όλων των ενεργειών για ένα χρονοδιάγραμμα.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Χρησιμοποιήστε τη μέθοδο Get με το Αναγνωριστικό ενέργεια για να ανακτήσετε μια συγκεκριμένη ενέργεια για ένα χρονοδιάγραμμα.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Δημιουργία ή επεξεργασία ενέργειες
Χρησιμοποιήστε τη μέθοδο τοποθέτηση με Αναγνωριστικό ενέργειας που είναι μοναδικές στο χρονοδιάγραμμα για να δημιουργήσετε μια νέα ενέργεια.  Όταν δημιουργείτε μια ενέργεια στην κονσόλα OMS, είναι ένα GUID για το αναγνωριστικό ενέργειας.

Χρησιμοποιήστε τη μέθοδο τοποθέτηση με ένα υπάρχον Αναγνωριστικό ενέργεια για την ίδια αποθηκευμένη αναζήτηση για να τροποποιήσετε που χρονοδιάγραμμα.  Στο σώμα της πρόσκλησης σε πρέπει να περιλαμβάνει το etag του χρονοδιαγράμματος.

Η μορφή της αίτησης για να δημιουργήσετε μια νέα ενέργεια διαφέρει κατά τύπο ενέργειας, ώστε να αυτά τα παραδείγματα παρέχονται στις παρακάτω ενότητες.

### <a name="deleting-actions"></a>Διαγραφή ενέργειες
Χρησιμοποιήστε τη μέθοδο Delete με το Αναγνωριστικό ενέργεια για να διαγράψετε μια ενέργεια.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Ενέργειες ειδοποίησης
Ένα χρονοδιάγραμμα, θα πρέπει να έχετε μία και μόνο μία ενέργεια ειδοποίησης.  Ενέργειες ειδοποίησης έχουν μία ή περισσότερες από τις ενότητες στον παρακάτω πίνακα.  Κάθε περιγράφεται λεπτομερώς περαιτέρω παρακάτω.

| Ενότητα | Περιγραφή |
|:--|:--|
| Όριο | Κριτήρια για κατά την εκτέλεση της ενέργειας. |  
| EmailNotification | Αποστολή αλληλογραφίας σε πολλούς παραλήπτες. |
| Αποκατάσταση εύρυθμης λειτουργίας | Ξεκινήστε μια runbook Αυτοματισμός Azure προσπαθεί να διορθώσετε το πρόβλημα αναγνωρίσιμα. |

#### <a name="thresholds"></a>Όρια
Μια ενέργεια ειδοποίησης θα πρέπει να έχετε μία και μόνο όριο.  Όταν τα αποτελέσματα της μια αποθηκευμένη αναζήτηση ταιριάζουν με το όριο σε μια ενέργεια που σχετίζεται με αυτήν την αναζήτηση, στη συνέχεια, τυχόν άλλες διεργασίες σε αυτήν την ενέργεια που εκτελούνται.  Μια ενέργεια μπορεί επίσης να περιέχει μόνο ένα όριο, έτσι ώστε να μπορεί να χρησιμοποιηθεί με ενέργειες από τους άλλους τύπους που δεν περιέχουν όρια.

Όρια έχουν τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| Τελεστής | Τελεστής για τη σύγκριση όριο. <br> gt = μεγαλύτερο από <br> lt = μικρότερο από |
| Τιμή | Τιμή για το όριο. |

Για παράδειγμα, εξετάστε το ενδεχόμενο ένα συμβάν ερώτημα με ένα χρονικό διάστημα 15 λεπτά, ένα χρονικό διάστημα από 30 λεπτά και μια οριακή τιμή μεγαλύτερη από 10. Σε αυτήν την περίπτωση, το ερώτημα θα εκτελείται κάθε 15 λεπτά, και μια ειδοποίηση μπορεί να ενεργοποιείται εάν επέστρεψε 10 συμβάντα που έχουν δημιουργηθεί πάνω από ένα διάστημα 30 λεπτά.

Ακολουθεί μια απάντηση δείγμα για μια ενέργεια με μόνο ένα όριο.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Χρησιμοποιήστε τη μέθοδο τοποθέτηση με ενέργεια μοναδικό Αναγνωριστικό για να δημιουργήσετε μια νέα ενέργεια όριο για ένα χρονοδιάγραμμα.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Χρησιμοποιήστε τη μέθοδο τοποθέτηση με υπάρχουσα Αναγνωριστικό ενέργεια για να τροποποιήσετε μια ενέργεια όριο για ένα χρονοδιάγραμμα.  Στο σώμα της πρόσκλησης σε πρέπει να περιλαμβάνει το etag της ενέργειας.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>Ειδοποίηση μέσω ηλεκτρονικού ταχυδρομείου
Ειδοποιήσεις ηλεκτρονικού ταχυδρομείου αποστολή αλληλογραφίας σε έναν ή περισσότερους παραλήπτες.  Περιλαμβάνουν τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| Οι παραλήπτες | Λίστα με διευθύνσεις ηλεκτρονικού ταχυδρομείου. |
| Θέμα | Το θέμα της αλληλογραφίας. |
| Συνημμένο | Συνημμένα δεν αυτήν τη στιγμή υποστηρίζονται, ώστε να αυτό θα έχουν πάντα την τιμή "Καμία". |

Ακολουθεί μια απάντηση δείγμα για μια ενέργεια ειδοποίησης ηλεκτρονικού ταχυδρομείου με μια οριακή τιμή.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Χρησιμοποιήστε τη μέθοδο τοποθέτηση με ενέργεια μοναδικό Αναγνωριστικό για να δημιουργήσετε μια νέα ενέργεια ηλεκτρονικού ταχυδρομείου για ένα χρονοδιάγραμμα.  Το παρακάτω παράδειγμα δημιουργεί μια ειδοποίηση ηλεκτρονικού ταχυδρομείου με μια οριακή τιμή, ώστε η αλληλογραφία αποστέλλεται όταν τα αποτελέσματα της αναζήτησης υπερβαίνουν το όριο.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Χρησιμοποιήστε τη μέθοδο τοποθέτηση με υπάρχουσα Αναγνωριστικό ενέργεια για να τροποποιήσετε μια ενέργεια ηλεκτρονικού ταχυδρομείου για ένα χρονοδιάγραμμα.  Στο σώμα της πρόσκλησης σε πρέπει να περιλαμβάνει το etag της ενέργειας.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Ενέργειες αποκατάσταση εύρυθμης λειτουργίας
Remediations ξεκινήστε μια runbook αυτοματισμού Azure που προσπαθεί να διορθώσετε το πρόβλημα που προσδιορίζονται με την ειδοποίηση.  Πρέπει να δημιουργήσετε ένα webhook για runbook χρησιμοποιείται σε μια ενέργεια αποκατάσταση εύρυθμης λειτουργίας και, στη συνέχεια, καθορίστε το URI στην ιδιότητα WebhookUri.  Όταν δημιουργείτε αυτήν την ενέργεια χρησιμοποιώντας την κονσόλα OMS, μια νέα webhook δημιουργείται αυτόματα για runbook.

Remediations περιλαμβάνουν τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| RunbookName | Το όνομα του runbook. Αυτό πρέπει να συμφωνεί με ένα δημοσιευμένο runbook στο λογαριασμό αυτοματισμού έχει ρυθμιστεί στη λύση αυτοματισμού στο χώρο εργασίας σας OMS. |
| WebhookUri | URI από το webhook.
| Λήξη | Η ημερομηνία και ώρα λήξης από την webhook.  Εάν το webhook δεν έχει μια λήξης, αυτό μπορεί να είναι οποιαδήποτε έγκυρη μελλοντική ημερομηνία. |

Ακολουθεί μια απάντηση δείγμα για μια ενέργεια αποκατάσταση εύρυθμης λειτουργίας με μια οριακή τιμή.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Χρησιμοποιήστε τη μέθοδο τοποθέτηση με ενέργεια μοναδικό Αναγνωριστικό για να δημιουργήσετε μια νέα ενέργεια αποκατάσταση εύρυθμης λειτουργίας για ένα χρονοδιάγραμμα.  Το παρακάτω παράδειγμα δημιουργεί μια επιδιόρθωση με μια οριακή τιμή, ώστε να ξεκινά runbook όταν τα αποτελέσματα της αναζήτησης υπερβαίνουν το όριο.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Χρησιμοποιήστε τη μέθοδο τοποθέτηση με υπάρχουσα Αναγνωριστικό ενέργεια για να τροποποιήσετε μια ενέργεια αποκατάσταση εύρυθμης λειτουργίας για ένα χρονοδιάγραμμα.  Στο σώμα της πρόσκλησης σε πρέπει να περιλαμβάνει το etag της ενέργειας.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Παράδειγμα
Ακολουθεί ένα παράδειγμα ολοκλήρωσης για να δημιουργήσετε μια νέα ειδοποίηση μέσω ηλεκτρονικού ταχυδρομείου.  Αυτό δημιουργεί ένα νέο χρονοδιάγραμμα μαζί με μια ενέργεια που περιέχει ένα όριο και το ηλεκτρονικό ταχυδρομείο.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Ενέργειες Webhook
Ενέργειες Webhook ξεκινήστε μια διεργασία καλεί μια διεύθυνση URL και προαιρετικά παρέχοντας ένα φορτίο να αποστέλλονται.  Είναι παρόμοια με την αποκατάσταση εύρυθμης λειτουργίας ενέργειες διαφορά προορίζονται για webhooks που μπορεί να ενεργοποιήσει διεργασίες εκτός από το Azure αυτοματισμού runbooks.  Επίσης, παρέχουν πρόσθετες επιλογή παρέχοντας ένα φορτίο να παραδοθεί της απομακρυσμένης διαδικασίας.

Ενέργειες Webhook δεν έχουν ένα όριο αλλά αντί για αυτό, πρέπει να προστεθεί σε ένα χρονοδιάγραμμα που περιλαμβάνει μια ενέργεια ειδοποίησης με μια οριακή τιμή.  Μπορείτε να προσθέσετε πολλές ενέργειες Webhook που θα όλα ενεργοποιείται όταν ικανοποιείται το όριο.

Ενέργειες Webhook περιλαμβάνουν τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| WebhookUri | Το θέμα της αλληλογραφίας. |
| CustomPayload | Προσαρμοσμένη φορτίο να αποστέλλονται τα webhook.  Η μορφοποίηση θα εξαρτώνται από τι αναμένει το webhook. |

Ακολουθεί μια απάντηση δείγμα για την ενέργεια webhook και μια ενέργεια ειδοποίησης σχετίζεται με ένα όριο.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Δημιουργία ή επεξεργασία μιας ενέργειας webhook
Χρησιμοποιήστε τη μέθοδο τοποθέτηση με ενέργεια μοναδικό Αναγνωριστικό για να δημιουργήσετε μια νέα ενέργεια webhook για ένα χρονοδιάγραμμα.  Το παρακάτω παράδειγμα δημιουργεί μια ενέργεια Webhook και μια ενέργεια ειδοποίησης με ένα όριο, έτσι ώστε το webhook θα ενεργοποιηθεί όταν τα αποτελέσματα της αναζήτησης υπερβαίνουν το όριο.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Χρησιμοποιήστε τη μέθοδο τοποθέτηση με υπάρχουσα Αναγνωριστικό ενέργεια για να τροποποιήσετε μια ενέργεια webhook για ένα χρονοδιάγραμμα.  Στο σώμα της πρόσκλησης σε πρέπει να περιλαμβάνει το etag της ενέργειας.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Επόμενα βήματα

- Χρησιμοποιήστε το [REST API για την εκτέλεση αναζητήσεων καταγραφής](log-analytics-log-search-api.md) στο αρχείο καταγραφής ανάλυσης.
