<properties
pageTitle="GitHub | Microsoft Azure"
description="Δημιουργήστε εφαρμογές λογικής με Azure εφαρμογής υπηρεσίας. GitHub είναι η υπηρεσία φιλοξενίας ένα αποθετήριο Git βασίζεται στο web. Προσφέρει όλες τις κατανέμεται αναθεώρησης στοιχείο ελέγχου και την πηγή κώδικα διαχείρισης (SCM) λειτουργίες Git, καθώς και προσθήκη δικό του δυνατότητες."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-github-connector"></a>Γρήγορα αποτελέσματα με τη γραμμή σύνδεσης GitHub

GitHub είναι η υπηρεσία φιλοξενίας ένα αποθετήριο Git βασίζεται στο web. Προσφέρει όλες τις κατανέμεται αναθεώρησης στοιχείο ελέγχου και την πηγή κώδικα διαχείρισης (SCM) λειτουργίες Git, καθώς και προσθήκη δικό του δυνατότητες.

>[AZURE.NOTE] Αυτήν την έκδοση του άρθρου ισχύει για έκδοση σχήματος 2015-08-01-προεπισκόπηση της λογικής εφαρμογών. 

Μπορείτε να ξεκινήσετε, δημιουργώντας μια εφαρμογή της λογικής τώρα, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής λογική](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Εναύσματα και ενέργειες

Η γραμμή σύνδεσης GitHub μπορεί να χρησιμοποιηθεί ως ενέργεια; έχει trigger(s). Όλες τις γραμμές σύνδεσης υποστηρίζει δεδομένα σε μορφές JSON και XML. 

 Η γραμμή σύνδεσης GitHub περιλαμβάνει το παρακάτω ενεργειών ή/και trigger(s) διαθέσιμες:

### <a name="github-actions"></a>Ενέργειες GitHub
Μπορείτε να εκτελέσετε τα παρακάτω ενεργειών:

|Ενέργεια|Περιγραφή|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Δημιουργεί ένα ζήτημα|
### <a name="github-triggers"></a>GitHub εναυσμάτων
Μπορείτε να ακούσετε για αυτά τα συμβάντα:

|Έναυσμα | Περιγραφή|
|--- | ---|
|Όταν ανοίγει ένα ζήτημα|Ανοίγει ένα ζήτημα|
|Όταν είναι κλειστό κάποιο πρόβλημα|Ένα ζήτημα είναι κλειστό|
|Όταν ένα θέμα έχει ανατεθεί|Ένα θέμα έχει ανατεθεί|


## <a name="create-a-connection-to-github"></a>Δημιουργία σύνδεσης με GitHub
Για να δημιουργήσετε εφαρμογές λογικής με GitHub, που πρέπει να πρώτα να δημιουργήσετε μια **σύνδεση** , στη συνέχεια, δώστε τις λεπτομέρειες για τις ακόλουθες ιδιότητες: 

|Ιδιότητα| Απαιτείται|Περιγραφή|
| ---|---|---|
|Διακριτικού|Ναι|Παρέχει GitHub διαπιστευτήρια|
Αφού δημιουργήσετε τη σύνδεση, μπορείτε να το χρησιμοποιήσετε για να εκτελέσετε τις ενέργειες και να ακούσετε για τα εναύσματα που περιγράφονται σε αυτό το άρθρο. 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] Μπορείτε να χρησιμοποιήσετε αυτήν τη σύνδεση σε άλλες εφαρμογές της λογικής.

## <a name="reference-for-github"></a>Αναφορά για GitHub
Ισχύει για την έκδοση: 1.0

## <a name="createissue"></a>CreateIssue
Δημιουργήστε ένα ζήτημα: δημιουργεί ένα ζήτημα 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| Όνομα| Τύπος δεδομένων|Απαιτείται|Που βρίσκεται στο|Προεπιλεγμένη τιμή|Περιγραφή|
| ---|---|---|---|---|---|
|repositoryOwner|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Κάτοχος αποθετήριο δεδομένων|
|repositoryName|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Όνομα αποθετήριο δεδομένων|
|issueBasicDetails| |Ναι|σώμα|Κανένας|Λεπτομέρειες θέματος|

#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Ok|
|400|Ακατάλληλη αίτηση|
|401|Μη εξουσιοδοτημένο|
|403|Δεν επιτρέπεται|
|404|Δεν βρέθηκε|
|500|Εσωτερικό σφάλμα διακομιστή. Άγνωστο σφάλμα|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|


## <a name="issueopened"></a>IssueOpened
Όταν ανοίγει ένα ζήτημα: ανοίγει ένα ζήτημα 

```GET: /trigger/issueOpened``` 

Δεν υπάρχουν παράμετροι για αυτήν την κλήση
#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Ok|
|400|Ακατάλληλη αίτηση|
|401|Μη εξουσιοδοτημένο|
|403|Δεν επιτρέπεται|
|404|Δεν βρέθηκε|
|500|Εσωτερικό σφάλμα διακομιστή. Άγνωστο σφάλμα|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|


## <a name="issueclosed"></a>IssueClosed
Όταν είναι κλειστό ένα ζήτημα: ένα ζήτημα είναι κλειστό 

```GET: /trigger/issueClosed``` 

Δεν υπάρχουν παράμετροι για αυτήν την κλήση
#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Ok|
|400|Ακατάλληλη αίτηση|
|401|Μη εξουσιοδοτημένο|
|403|Δεν επιτρέπεται|
|404|Δεν βρέθηκε|
|500|Εσωτερικό σφάλμα διακομιστή. Άγνωστο σφάλμα|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|


## <a name="issueassigned"></a>IssueAssigned
Όταν ένα θέμα έχει ανατεθεί: ένα θέμα έχει ανατεθεί 

```GET: /trigger/issueAssigned``` 

Δεν υπάρχουν παράμετροι για αυτήν την κλήση
#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Ok|
|400|Ακατάλληλη αίτηση|
|401|Μη εξουσιοδοτημένο|
|403|Δεν επιτρέπεται|
|404|Δεν βρέθηκε|
|500|Εσωτερικό σφάλμα διακομιστή. Άγνωστο σφάλμα|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|


## <a name="object-definitions"></a>Ορισμούς αντικειμένων 

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| Όνομα ιδιότητας | Τύπος δεδομένων | Απαιτείται |
|---|---|---|
|Τίτλος|συμβολοσειρά|Ναι |
|σώμα|συμβολοσειρά|Ναι |
|το άτομο για ανάθεση|συμβολοσειρά|Ναι |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| Όνομα ιδιότητας | Τύπος δεδομένων | Απαιτείται |
|---|---|---|
|Τίτλος|συμβολοσειρά|Ναι |
|σώμα|συμβολοσειρά|Ναι |
|το άτομο για ανάθεση|συμβολοσειρά|Ναι |
|αριθμός|συμβολοσειρά|Όχι |
|κατάσταση|συμβολοσειρά|Όχι |
|created_at|συμβολοσειρά|Όχι |
|repository_url|συμβολοσειρά|Όχι |


## <a name="next-steps"></a>Επόμενα βήματα
[Δημιουργία εφαρμογής λογικής](../app-service-logic/app-service-logic-create-a-logic-app.md)