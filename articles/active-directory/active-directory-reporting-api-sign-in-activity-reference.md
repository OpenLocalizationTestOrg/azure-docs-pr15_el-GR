<properties
    pageTitle="Αναφορά εισόδου δραστηριότητας Azure Active Directory API αναφοράς | Microsoft Azure"
    description="Αναφορά για το API αναφορά δραστηριότητας εισόδου Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Αναφορά εισόδου δραστηριότητας Azure Active Directory API αναφοράς


Αυτό το θέμα είναι μέρος μιας συλλογής θεμάτων σχετικά με το Azure Active Directory αναφοράς API.  
Azure AD αναφοράς σάς παρέχει ένα API που σας επιτρέπει να αποκτήσετε πρόσβαση εισόδου δραστηριότητας δεδομένων αναφοράς με χρήση κώδικα ή σχετικά εργαλεία.
Η εμβέλεια αυτού του θέματος είναι να σας παρέχει πληροφορίες αναφοράς σχετικά με την **είσοδο API αναφορά δραστηριότητας**.

Ανατρέξτε στα θέματα:

- Για περισσότερες πληροφορίες εννοιολογική [εισόδου δραστηριοτήτων](active-directory-reporting-azure-portal.md#sign-in-activities)
- [Γρήγορα αποτελέσματα με το Azure Active Directory αναφοράς API](active-directory-reporting-api-getting-started.md) για περισσότερες πληροφορίες σχετικά με το API αναφοράς.

Για ερωτήσεις, θέματα ή σχόλια, επικοινωνήστε με την [Αναφορά συμβάλλει AAD](mailto:aadreportinghelp@microsoft.com).



## <a name="who-can-access-the-api-data"></a>Ποιος έχει πρόσβαση στα δεδομένα API;

- Οι χρήστες το ρόλο διαχειριστή ασφαλείας ή ανάγνωσης ασφαλείας

- Οι καθολικοί διαχειριστές

- Οποιαδήποτε εφαρμογή που διαθέτει εξουσιοδότηση πρόσβασης το API (εξουσιοδότησης εφαρμογή μπορεί να είναι το πρόγραμμα εγκατάστασης μόνο βάσει δικαιωμάτων καθολικός διαχειριστής)



## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να αποκτήσετε πρόσβαση σε αυτήν την αναφορά μέσω του API αναφοράς, πρέπει να έχετε:

- Μια [Azure Active Directory Premium P1 ή P2 edition](active-directory-editions.md)

- Ολοκλήρωση τις [προϋποθέσεις για την πρόσβαση του Azure AD αναφοράς API](active-directory-reporting-api-prerequisites.md). 


##<a name="accessing-the-api"></a>Πρόσβαση σε το API

Μπορείτε είτε να αποκτήσετε πρόσβαση σε αυτό το API μέσω της [Εξερεύνησης Graph](https://graphexplorer2.cloudapp.net) ή μέσω προγραμματισμού χρησιμοποιώντας, για παράδειγμα, το PowerShell. Με τη σειρά για το PowerShell για να ερμηνεύσετε σωστά τη σύνταξη του φίλτρου OData χρησιμοποιούνται στο ΥΠΌΛΟΙΠΟ γράφημα AAD κλήσεις, πρέπει να χρησιμοποιήσετε το backtick (ή: βαρεία) χαρακτήρα "escape" το χαρακτήρα $. Ο χαρακτήρας backtick λειτουργεί ως [χαρακτήρα διαφυγής του PowerShell](https://technet.microsoft.com/library/hh847755.aspx), επιτρέποντάς του PowerShell για να κάνετε ένα ακριβές ερμηνεία του χαρακτήρα $, και να αποφευχθεί η σύγχυση του ως όνομα μεταβλητής PowerShell (ie: $filter).

Αυτό το θέμα εστιάζει στην Εξερεύνηση του γραφήματος. Για ένα παράδειγμα του PowerShell, ανατρέξτε στο θέμα αυτό [δέσμη ενεργειών του PowerShell](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).


## <a name="api-endpoint"></a>API τελικού σημείου

Μπορείτε να αποκτήσετε πρόσβαση σε αυτό το API χρησιμοποιώντας την παρακάτω URI βάσης:  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Λόγω του όγκου των δεδομένων, αυτό το API έχει όριο ένα εκατομμύριο επιστρέφονται εγγραφές. 

Αυτή η κλήση επιστρέφει τα δεδομένα σε δέσμες. Κάθε δέσμη έχει έως 1000 εγγραφές.  
Για να λάβετε την επόμενη δέσμη εγγραφών, χρησιμοποιήστε την επόμενη σύνδεση. Λάβετε τις πληροφορίες [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) από το πρώτο σύνολο εγγραφών, επιστρέφεται. Το διακριτικό παράλειψη ρύθμιση θα γίνει στο τέλος του αποτελέσματος.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Υποστηριζόμενες φίλτρα

Μπορείτε να περιορίσετε τον αριθμό των εγγραφών που επιστρέφονται από ένα API κλήσεων στη φόρμα ενός φίλτρου.  
Για το API εισόδου που σχετίζονται με τα δεδομένα, τα ακόλουθα φίλτρα που υποστηρίζονται:

- **$top =\<αριθμού εγγραφών που θα επιστραφούν\> ** - για να περιορίσετε τον αριθμό των επιστρεφόμενων εγγραφών. Αυτή είναι μια ακριβή λειτουργία. Δεν πρέπει να χρησιμοποιείτε αυτό το φίλτρο, εάν θέλετε να επιστρέψετε χιλιάδες αντικείμενα.  
- **$filter =\<την κατάσταση φίλτρο\> ** - για να καθορίσετε, με βάση τα πεδία υποστηριζόμενες φίλτρου, τον τύπο των καρτελών που σας ενδιαφέρουν



## <a name="supported-filter-fields-and-operators"></a>Πεδία υποστηριζόμενες φίλτρου και τελεστών

Για να καθορίσετε τον τύπο των καρτελών που σας ενδιαφέρουν, μπορείτε να δημιουργήσετε μια πρόταση φίλτρο που μπορεί να περιέχει μία ή ένα συνδυασμό των ακόλουθων πεδίων φίλτρου:

- [signinDateTime](#signindatetime) - καθορίζει μια ημερομηνία ή περιοχή ημερομηνιών

- [αναγνωριστικό χρήστη](#userid) - ορίζει ένα συγκεκριμένο χρήστη με βάση το αναγνωριστικό του χρήστη.

- [userPrincipalName](#userprincipalname) - ορίζει ένα συγκεκριμένο χρήστη βάσει κύριο όνομα χρήστη του χρήστη (UPN)

- [αναγνωριστικό εφαρμογής](#appid) - καθορίζει μια συγκεκριμένη εφαρμογή, με βάση το Αναγνωριστικό της εφαρμογής

- [appDisplayName](#appdisplayname) - καθορίζει μια συγκεκριμένη εφαρμογή, ανάλογα με την εφαρμογή εμφανιζόμενο όνομα

- [loginStatus](#loginStatus) - ορίζει την κατάσταση των συνδέσεων στις (επιτυχίας / αποτυχία)


> [AZURE.NOTE] Όταν χρησιμοποιείτε Graph Explorer, που χρειάζεστε για να χρησιμοποιήσετε τη συμφωνία πεζών-κεφαλαίων για κάθε γράμμα είναι τα πεδία φίλτρου.


Για να περιορίσετε το εύρος της τα δεδομένα που επιστρέφονται, μπορείτε να δημιουργήσετε συνδυασμούς τα υποστηριζόμενα φίλτρα και πεδία φίλτρου. Για παράδειγμα, η ακόλουθη πρόταση επιστρέφει τις επάνω 10 εγγραφές μεταξύ 2016 1η Ιουλίου και 2016 6ης Ιουλίου:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**Υποστηριζόμενα τελεστές**: eq, σελίδας, le, gt, lt

**Παράδειγμα**:

Με μια συγκεκριμένη ημερομηνία

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



Χρησιμοποιείτε μια περιοχή ημερομηνιών    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Σημειώσεις**:

Η παράμετρος datetime πρέπει να είναι σε μορφή UTC 


----------

### <a name="userid"></a>αναγνωριστικό χρήστη

**Υποστηριζόμενα τελεστές**: eq

**Παράδειγμα**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Σημειώσεις**:

Η τιμή του αναγνωριστικό χρήστη είναι μια τιμή συμβολοσειράς



----------

### <a name="userprincipalname"></a>userPrincipalName

**Υποστηριζόμενα τελεστές**: eq

**Παράδειγμα**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Σημειώσεις**:

Η τιμή του χαρακτηριστικού userPrincipalName είναι μια τιμή συμβολοσειράς

----------

### <a name="appid"></a>αναγνωριστικό εφαρμογής

**Υποστηριζόμενα τελεστές**: eq

**Παράδειγμα**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Σημειώσεις**:

Η τιμή του αναγνωριστικό εφαρμογής είναι μια τιμή συμβολοσειράς

----------


### <a name="appdisplayname"></a>appDisplayName

**Υποστηριζόμενα τελεστές**: eq

**Παράδειγμα**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Σημειώσεις**:

Η τιμή του appDisplayName είναι μια τιμή συμβολοσειράς

----------

### <a name="loginstatus"></a>loginStatus

**Υποστηριζόμενα τελεστές**: eq

**Παράδειγμα**:

    $filter=loginStatus+eq+'1'  


**Σημειώσεις**:

Υπάρχουν δύο επιλογές για την loginStatus: 0 - επιτυχία, 1 - Αποτυχία

----------



## <a name="next-steps"></a>Επόμενα βήματα

- Θέλετε να δείτε παραδείγματα για δραστηριότητες φιλτραρισμένη εισόδου; Δείτε τα [δείγματα αναφορών API εισόδου δραστηριότητας Azure Active Directory](active-directory-reporting-api-sign-in-activity-samples.md).

- Θέλετε να μάθετε περισσότερα σχετικά με το Azure AD αναφοράς API; Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure Active Directory αναφοράς API](active-directory-reporting-api-getting-started.md).