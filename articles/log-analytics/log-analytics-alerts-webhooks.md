<properties
   pageTitle="Δείγμα ειδοποίησης webhook ανάλυσης αρχείου καταγραφής"
   description="Μία από τις ενέργειες που μπορείτε να εκτελέσετε απόκριση σε μια ειδοποίηση καταγραφής ανάλυσης είναι μια *webhook*, που σας επιτρέπει να καλέσετε μια εξωτερική διαδικασία μέσω μιας μεμονωμένης αίτησης HTTP. Σε αυτό το άρθρο παρουσιάζει ένα παράδειγμα για τη δημιουργία μιας ενέργειας webhook σε μια ειδοποίηση καταγραφής αναλυτικών στοιχείων χρήσης αδράνεια."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="webhooks-in-log-analytics-alerts"></a>Webhooks στην ανάλυση καταγραφής ειδοποιήσεων

Μία από τις ενέργειες που μπορείτε να εκτελέσετε απόκριση σε μια [ειδοποίηση καταγραφής ανάλυσης](log-analytics-alerts.md) είναι μια *webhook*, που σας επιτρέπει να καλέσετε μια εξωτερική διαδικασία μέσω μιας μεμονωμένης αίτησης HTTP.  Μπορείτε να διαβάσετε σχετικά με τις λεπτομέρειες των ειδοποιήσεων και webhooks σε [ειδοποιήσεις στο αρχείο καταγραφής ανάλυσης](log-analytics-alerts.md)

Σε αυτό το άρθρο, θα δούμε ένα παράδειγμα για τη δημιουργία μιας ενέργειας webhook σε μια ειδοποίηση καταγραφής αναλυτικών στοιχείων χρήσης αδράνεια που είναι μια υπηρεσία ανταλλαγής μηνυμάτων.

>[AZURE.NOTE] Πρέπει να έχετε ένα λογαριασμό αδράνειας για να ολοκληρώσετε αυτό το δείγμα.  Μπορείτε να εγγραφείτε για έναν δωρεάν λογαριασμό στο [slack.com](http://slack.com).

## <a name="step-1---enable-webhooks-in-slack"></a>Βήμα 1 - ενεργοποίηση webhooks σε αδράνεια
2.  Πραγματοποιήστε είσοδο στο αδράνειας στο [slack.com](http://slack.com).
3.  Επιλέξτε ένα κανάλι στην ενότητα **κανάλια** στο αριστερό τμήμα του παραθύρου.  Αυτό είναι το κανάλι που το μήνυμα θα σταλεί σε.  Μπορείτε να επιλέξετε ένα από τα προεπιλεγμένα κανάλια όπως **γενικό** ή **τυχαία**.  Σε ένα σενάριο παραγωγής, πρέπει να δημιουργήσετε πιθανώς ένα ειδικό κανάλι όπως **criticalservicealerts**. <br>

    ![Αδράνειας καναλιών](media/log-analytics-alerts-webhooks/oms-webhooks01.png)

3. Κάντε κλικ στην επιλογή **Προσθήκη εφαρμογής ή προσαρμοσμένο ενοποίησης** για να ανοίξετε τον κατάλογο εφαρμογών.
3.  Πληκτρολογήστε *webhooks* στο πλαίσιο αναζήτησης και, στη συνέχεια, επιλέξτε **Εισερχόμενες WebHooks**. <br>

    ![Αδράνειας καναλιών](media/log-analytics-alerts-webhooks/oms-webhooks02.png)

4.  Δίπλα στο όνομα της ομάδας σας, κάντε κλικ στην επιλογή **εγκατάσταση** .
5.  Κάντε κλικ στην επιλογή **Προσθήκη ρύθμισης παραμέτρων**.
6.  Επιλέξτε το το κανάλι που πρόκειται να χρησιμοποιήσετε για αυτό το παράδειγμα, και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη εισερχόμενες WebHooks ενοποίησης**.  
6. Αντιγράψτε τη **διεύθυνση URL Webhook**.  Θα γίνει επικόλληση αυτό σε τη ρύθμιση της ειδοποίησης. <br>

    ![Αδράνειας καναλιών](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Βήμα 2 - Δημιουργία ειδοποίησης κανόνα στο αρχείο καταγραφής ανάλυσης
1.  [Δημιουργία κανόνα ειδοποίησης](log-analytics-alerts.md) με τις παρακάτω ρυθμίσεις.
    - Ερώτημα:```    Type=Event EventLevelName=error ```
    - Έλεγχος για αυτήν την ειδοποίηση κάθε: 5 λεπτά
    - Ο αριθμός των αποτελεσμάτων είναι: μεγαλύτερη από 10
    - Σε αυτό το παράθυρο ώρα: 60 λεπτά
    - Επιλέξτε **Ναι** για **Webhook** και **όχι** για τις άλλες ενέργειες.
7. Επικολλήστε τη διεύθυνση URL αδράνεια στο πεδίο **Διεύθυνση URL Webhook** .
8. Ενεργοποιήστε την επιλογή για να **συμπεριλάβετε ένα προσαρμοσμένο φορτίου JSON**.
9. Slack αναμένει ένα φορτίο μορφοποιημένη στο JSON με μια παράμετρο που ονομάζεται *κειμένου*.  Αυτό είναι το κείμενο που θα εμφανίζεται στο μήνυμα που δημιουργεί.  Μπορείτε να χρησιμοποιήσετε μία ή περισσότερες από τις παραμέτρους ειδοποίησης χρησιμοποιώντας το *#* σύμβολο όπως όπως στο παρακάτω παράδειγμα.

    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```

    ![παράδειγμα JSON φορτίο](media/log-analytics-alerts-webhooks/oms-webhooks07.png)

9.  Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε τον κανόνα ειδοποίησης.

10. Περιμένετε αρκετό χρόνο για μια ειδοποίηση ώστε να δημιουργηθεί και, στη συνέχεια, επιλέξτε αδράνεια για ένα μήνυμα που θα είναι παρόμοιο με το ακόλουθο.

    ![παράδειγμα webhook σε αδράνεια](media/log-analytics-alerts-webhooks/oms-webhooks08.png)


### <a name="advanced-webhook-payload-for-slack"></a>Για προχωρημένους φορτίο webhook για αδράνεια

Μπορείτε να προσαρμόσετε ευρέως εισερχόμενων μηνυμάτων με αδράνεια. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Εισερχόμενες Webhooks](https://api.slack.com/incoming-webhooks) στην τοποθεσία Web του αδράνειας. Ακολουθεί μια πιο σύνθετη φορτίου για να δημιουργήσετε ένα εμπλουτισμένο μήνυμα με τη μορφοποίηση:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


Αυτό θα δημιουργήσει ένα μήνυμα σε αδράνεια παρόμοιο με το ακόλουθο.

![παράδειγμα μηνύματος σε αδράνεια](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Σύνοψη

Με αυτόν τον ειδοποίησης κανόνα στο σημείο, θα έχετε ένα μήνυμα που αποστέλλεται σε αδράνεια κάθε φορά που πληρούνται τα κριτήρια.  

Αυτή είναι μια ενέργεια που μπορείτε να δημιουργήσετε απόκριση σε μια ειδοποίηση μόνο ένα παράδειγμα.  Μπορείτε να δημιουργήσετε μια ενέργεια webhook που καλεί μια άλλη εξωτερική υπηρεσία, μια ενέργεια runbook για να ξεκινήσετε μια runbook σε αυτοματισμού Azure ή μια ενέργεια ηλεκτρονικού ταχυδρομείου για την αποστολή αλληλογραφίας στον εαυτό σας ή άλλους παραλήπτες.   

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε περισσότερα σχετικά με τις [ειδοποιήσεις στο αρχείο καταγραφής αναλυτικών στοιχείων](log-analytics-alerts.md) , συμπεριλαμβανομένων των άλλων ενεργειών.
- [Δημιουργία runbooks στο Azure Automation](../automation/automation-webhooks.md) που μπορεί να ονομάζεται από μια webhook.