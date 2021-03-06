<properties
    pageTitle="Προσθέστε τη γραμμή σύνδεσης τους χρήστες του Office 365 σε εφαρμογές της λογικής | Microsoft Azure"
    description="Επισκόπηση της σύνδεσης τους χρήστες του Office 365 με παραμέτρους REST API"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>Γρήγορα αποτελέσματα με τη γραμμή σύνδεσης τους χρήστες του Office 365

Σύνδεση με τους χρήστες του Office 365 για να λάβετε τα προφίλ, οι χρήστες αναζήτησης και άλλα. 

>[AZURE.NOTE] Αυτήν την έκδοση του άρθρου ισχύει για έκδοση σχήματος 2015-08-01-προεπισκόπηση της λογικής εφαρμογών.

Με τους χρήστες του Office 365, μπορείτε να:

- Δημιουργήστε την επιχειρηματική ροή με βάση τα δεδομένα που λαμβάνετε από τους χρήστες του Office 365. 
- Χρήση ενεργειών που να τους άμεσους υφισταμένους, λάβετε προφίλ του διευθυντή ενός χρήστη και πολλά άλλα. Αυτές οι ενέργειες αποσπάσετε μια απάντηση και, στη συνέχεια, κάντε το αποτέλεσμα είναι διαθέσιμες για άλλες ενέργειες. Για παράδειγμα, λήψη ενός ατόμου τους άμεσους υφισταμένους, και, στη συνέχεια, να αυτές τις πληροφορίες και ενημέρωση μιας βάσης δεδομένων SQL Azure. 

Για να προσθέσετε μια πράξη σε εφαρμογές της λογικής, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής λογική](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Εναύσματα και ενέργειες

Η γραμμή σύνδεσης τους χρήστες του Office 365 περιλαμβάνει τις ακόλουθες ενέργειες που είναι διαθέσιμες. Υπάρχουν εναύσματα.

| Εναυσμάτων | Ενέργειες|
| --- | --- |
|Κανένας | <ul><li>Λήψη διαχειριστή</li><li>Λάβετε το προφίλ μου</li><li>Λήψη άμεσους υφισταμένους</li><li>Λήψη προφίλ χρήστη</li><li>Αναζήτηση για τους χρήστες</li></ul>|

Όλες τις γραμμές σύνδεσης υποστηρίζει δεδομένα σε μορφές JSON και XML. 


## <a name="create-a-connection-to-office-365-users"></a>Δημιουργία σύνδεσης με τους χρήστες του Office 365

Όταν προσθέτετε αυτή η γραμμή σύνδεσης για τις εφαρμογές σας λογικής, πρέπει να είσοδος στο λογαριασμό σας τους χρήστες του Office 365 και λογική εφαρμογών για να συνδεθείτε στο λογαριασμό σας.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

Αφού δημιουργήσετε τη σύνδεση, εισαγάγετε τις ιδιότητες τους χρήστες του Office 365, όπως το αναγνωριστικό χρήστη. Η **αναφορά REST API** σε αυτό το θέμα περιγράφει αυτές τις ιδιότητες.

>[AZURE.TIP] Μπορείτε να χρησιμοποιήσετε αυτήν τη σύνδεση ίδια τους χρήστες του Office 365 σε άλλες εφαρμογές της λογικής.


## <a name="office-365-users-rest-api-reference"></a>Αναφορά REST API χρήστες του Office 365
Ισχύει για την έκδοση: 1.0.

### <a name="get-my-profile"></a>Λάβετε το προφίλ μου 
Ανακτά το προφίλ για τον τρέχοντα χρήστη.  
```GET: /users/me``` 

Δεν υπάρχουν παράμετροι για αυτήν την κλήση.

#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|202|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|400|BadRequest|
|401|Μη εξουσιοδοτημένο|
|403|Δεν επιτρέπεται|
|500|Εσωτερικό σφάλμα διακομιστή|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|


### <a name="get-user-profile"></a>Λήψη προφίλ χρήστη 
Ανακτά ένα συγκεκριμένο προφίλ χρήστη.  
```GET: /users/{userId}``` 

| Όνομα| Τύπος δεδομένων|Απαιτείται|Που βρίσκεται στο|Προεπιλεγμένη τιμή|Περιγραφή|
| ---|---|---|---|---|---|
|αναγνωριστικό χρήστη|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Αναγνωριστικό χρήστη κύριο όνομα ή ηλεκτρονικού ταχυδρομείου|

#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|202|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|400|BadRequest|
|401|Μη εξουσιοδοτημένο|
|403|Δεν επιτρέπεται|
|500|Εσωτερικό σφάλμα διακομιστή|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|


### <a name="get-manager"></a>Λήψη διαχειριστή 
Ανακτά προφίλ χρήστη για τη διαχείριση των ο καθορισμένος χρήστης.  
```GET: /users/{userId}/manager``` 

| Όνομα| Τύπος δεδομένων|Απαιτείται|Που βρίσκεται στο|Προεπιλεγμένη τιμή|Περιγραφή|
| ---|---|---|---|---|---|
|αναγνωριστικό χρήστη|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Αναγνωριστικό χρήστη κύριο όνομα ή ηλεκτρονικού ταχυδρομείου|

#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|202|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|400|BadRequest|
|401|Μη εξουσιοδοτημένο|
|403|Δεν επιτρέπεται|
|500|Εσωτερικό σφάλμα διακομιστή|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|



### <a name="get-direct-reports"></a>Λήψη άμεσους υφισταμένους 
Λάβετε άμεσους υφισταμένους.  
```GET: /users/{userId}/directReports``` 

| Όνομα| Τύπος δεδομένων|Απαιτείται|Που βρίσκεται στο|Προεπιλεγμένη τιμή|Περιγραφή|
| ---|---|---|---|---|---|
|αναγνωριστικό χρήστη|συμβολοσειρά|Ναι|διαδρομή|Κανένας|Αναγνωριστικό χρήστη κύριο όνομα ή ηλεκτρονικού ταχυδρομείου|

#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|202|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|400|BadRequest|
|401|Μη εξουσιοδοτημένο|
|403|Δεν επιτρέπεται|
|500|Εσωτερικό σφάλμα διακομιστή|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|



### <a name="search-for-users"></a>Αναζήτηση για τους χρήστες 
Ανακτά τα αποτελέσματα των προφίλ χρήστη αναζήτησης.  
```GET: /users``` 

| Όνομα| Τύπος δεδομένων|Απαιτείται|Που βρίσκεται στο|Προεπιλεγμένη τιμή|Περιγραφή|
| ---|---|---|---|---|---|
|searchTerm|συμβολοσειρά|Όχι|ερώτημα|Κανένας|Συμβολοσειράς αναζήτησης (ισχύει για: Εμφάνιση όνομα, όνομα, επώνυμο, αλληλογραφία, αλληλογραφία ψευδώνυμο και κύριο όνομα χρήστη)|

#### <a name="response"></a>Απόκριση

|Όνομα|Περιγραφή|
|---|---|
|200|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|202|Η λειτουργία ολοκληρώθηκε με επιτυχία|
|400|BadRequest|
|401|Μη εξουσιοδοτημένο|
|403|Δεν επιτρέπεται|
|500|Εσωτερικό σφάλμα διακομιστή|
|προεπιλεγμένη|Απέτυχε η λειτουργία.|



## <a name="object-definitions"></a>Ορισμούς αντικειμένων

#### <a name="user-user-model-class"></a>Χρήστη: Κλάση μοντέλο χρήστη

|Όνομα ιδιότητας | Τύπος δεδομένων |Απαιτείται
|---|---|---|
|Εμφανιζόμενο όνομα|συμβολοσειρά|Όχι|
|GivenName|συμβολοσειρά|Όχι|
|Επώνυμο|συμβολοσειρά|Όχι|
|Αλληλογραφία|συμβολοσειρά|Όχι|
|MailNickname|συμβολοσειρά|Όχι|
|TelephoneNumber|συμβολοσειρά|Όχι|
|AccountEnabled|δυαδική τιμή|Όχι|
|Αναγνωριστικό|συμβολοσειρά|Ναι
|UserPrincipalName|συμβολοσειρά|Όχι|
|Τμήμα|συμβολοσειρά|Όχι|
|Τίτλος εργασίας|συμβολοσειρά|Όχι|
|mobilePhone|συμβολοσειρά|Όχι|


## <a name="next-steps"></a>Επόμενα βήματα

[Δημιουργία μιας εφαρμογής λογικής](../app-service-logic/app-service-logic-create-a-logic-app.md).

Επιστροφή στη [λίστα APIs](apis-list.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
