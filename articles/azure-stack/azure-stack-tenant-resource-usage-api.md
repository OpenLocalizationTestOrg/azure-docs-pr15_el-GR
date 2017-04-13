<properties
    pageTitle="Χρήση πόρων API μισθωτή | Microsoft Azure"
    description="Αναφορά για τη χρήση πόρων API, η οποία ανακτήσετε πληροφορίες χρήσης Azure στοίβας."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="tenant-resource-usage-api"></a>Χρήση πόρων API μισθωτή

Ένας μισθωτής να χρησιμοποιήσετε το API μισθωτή για να προβάλετε τα δεδομένα χρήσης πόρων του μισθωτή. Αυτό το API είναι συνεπείς με το API χρήση Azure (προς το παρόν στο ιδιωτικό preview).

Μπορείτε να χρησιμοποιήσετε το Windows PowerShell cmdlet **Get-UsageAggregates** για να λάβετε δεδομένων χρήσης, όπως στο Azure.

## <a name="api-call"></a>Κλήση API

### <a name="request"></a>Αίτηση

Η αίτηση λαμβάνει κατανάλωση λεπτομέρειες για τις συνδρομές που ζητήθηκαν και για το χρονικό πλαίσιο που ζητήθηκαν. Δεν υπάρχει καμία σώμα της αίτησης.

| **Μέθοδος**  | **Αίτηση URI** |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ΛΉΨΗ         | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Ορίσματα

| **Το όρισμα**             | **Περιγραφή** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Armendpoint*             | Azure διαχείριση πόρων το τελικό σημείο του περιβάλλοντός σας Azure στοίβας. |
| *subId*                   | Αναγνωριστικό συνδρομής του χρήστη που πραγματοποιεί την κλήση. Μπορείτε να χρησιμοποιήσετε αυτό το API μόνο σε ερώτημα για μια μεμονωμένη συνδρομή χρήση. Υπηρεσίες παροχής να χρησιμοποιήσετε το API χρήση πόρων υπηρεσία παροχής που χρησιμοποιείτε για χρήση ερωτήματος για μισθωτές όλων. |
| *reportedStartTime*       | Ώρα έναρξης της το ερώτημα. Η τιμή *DateTime* πρέπει να είναι στο UTC και στην αρχή της ώρας, για παράδειγμα, 13:00. Για ημερήσια συγκέντρωση, ορίστε αυτήν την τιμή σε UTC τα μεσάνυχτα. Η μορφή είναι *διαφυγής* ISO 8601, για παράδειγμα, 2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z, όπου είναι διαφυγής άνω και κάτω τελεία για να 3α % και συν διαφυγής για 2β %, ώστε να είναι φιλική URI. |
| *reportedEndTime*         | Ώρα λήξης του ερωτήματος. Τους περιορισμούς που ισχύουν για *reportedStartTime* εφαρμόζονται επίσης σε αυτό το όρισμα. Η τιμή για το *reportedEndTime* δεν μπορεί να είναι στο μέλλον. |
| *aggregationGranularity*  | Προαιρετική παράμετρος που έχει δύο διακριτές τιμές πιθανά: καθημερινά και ανά ώρα. Καθώς προτείνετε τις τιμές, μία επιστρέφει τα δεδομένα σε ημερήσια υποδιαίρεση και το άλλο είναι μια ωριαία ανάλυση. Η επιλογή ημερήσιων είναι η προεπιλεγμένη. |
| *subscriberId*            | Αναγνωριστικό συνδρομής. Για να λάβετε τα φιλτραρισμένα δεδομένα, απαιτείται το Αναγνωριστικό συνδρομής του μισθωτή απευθείας από την υπηρεσία παροχής. Εάν καθοριστεί η παράμετρος Αναγνωριστικό χωρίς τη συνδρομή, η κλήση επιστρέφει δεδομένων χρήσης για μισθωτές απευθείας όλες της υπηρεσίας παροχής. |
| *έκδοση API*             | Η έκδοση του πρωτοκόλλου που χρησιμοποιείται για να κάνετε αυτήν την αίτηση. Πρέπει να χρησιμοποιήσετε 2015-06-01-preview. |
| *continuationToken*       | Ανάκτηση διακριτικού από την τελευταία κλήση στην υπηρεσία παροχής χρήση API. Αυτό είναι απαραίτητο όταν μια απάντηση είναι μεγαλύτερη από 1.000 γραμμές. Αυτό είναι το σελιδοδείκτη για την πρόοδο. Εάν δεν υπάρχει, ανάκτηση των δεδομένων από την αρχή της ημέρας ή ώρα, με βάση το επίπεδο λεπτομερειών που εισήχθησαν στο. |

### <a name="response"></a>Απόκριση

ΛΉΨΗ /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime = 2015-06-01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity = ημερήσια & έκδοση api = 1.0

{

"τιμή":\[

{

"αναγνωριστικό": "/ subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",

"όνομα": "sub1-meterID1"

"Τύπος": "Microsoft.Commerce/UsageAggregate",

"Ιδιότητες": {

"subscriptionId": "sub1",

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

"instanceData": "{\\" Microsoft.Resources\\": {\\" resourceUri\\":\\" resourceUri1\\",\\" θέση\\":\\" την Αλάσκα\\",\\" ετικέτες\\": null,\\" additionalInfo\\": null}}",

:2.4000000000 "Ποσότητα",

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Λεπτομέρειες απόκρισης

| **Το όρισμα**      | **Περιγραφή** |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *αναγνωριστικό*              | Μοναδικό Αναγνωριστικό του χρήση συγκεντρωτικής τιμής |
| *Όνομα*            | Όνομα της χρήσης συγκεντρωτικής τιμής |
| *Τύπος*            | Ορισμός πόρων |
| *subscriptionId*  | Αναγνωριστικό συνδρομής του Azure χρήστη |
| *usageStartTime*  | UTC ώρα έναρξης της το χρωματισμού χρήση στην οποία ανήκει αυτήν τη συγκέντρωση χρήση |
| *usageEndTime*    | Ώρα λήξης UTC της το χρωματισμού χρήση στην οποία ανήκει αυτήν τη συγκέντρωση χρήση |
| *instanceData*    | Ζεύγη κλειδιού-τιμής των λεπτομερειών παρουσία (σε μια νέα μορφή):<br>  *resourceUri*: πλήρως προσδιορισμένη Αναγνωριστικό πόρου, συμπεριλαμβανομένων των ομάδων πόρων και το όνομα της παρουσίας <br>  *θέση*: περιοχή στην οποία έχει εκτέλεση αυτής της υπηρεσίας <br>  *ετικέτες*: ετικέτες πόρων που καθορίζει το χρήστη <br>  *additionalInfo*: περισσότερες λεπτομέρειες σχετικά με τον πόρο που καταναλώνεται, για παράδειγμα, τύπος έκδοση ή την εικόνα του λειτουργικού Συστήματος |
| *ποσότητα*        | Ποσό των κατανάλωση πόρων που έγιναν σε αυτό το πλαίσιο χρόνου |
| *meterId*         | Μοναδικό Αναγνωριστικό για τον πόρο που ήταν που καταναλώθηκε (επίσης γνωστές ως *ResourceID*) |

## <a name="next-steps"></a>Επόμενα βήματα

[Συνήθεις Ερωτήσεις για που σχετίζονται με χρήση](azure-stack-usage-related-faq.md)