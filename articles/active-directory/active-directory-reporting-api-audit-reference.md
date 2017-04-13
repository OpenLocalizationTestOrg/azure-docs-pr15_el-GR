<properties
    pageTitle="Azure Active Directory ελέγχου API αναφοράς | Microsoft Azure"
    description="Πώς μπορείτε να ξεκινήσετε με το API ελέγχου Azure Active Directory"
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
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory ελέγχου API αναφοράς

Αυτό το θέμα είναι μέρος μιας συλλογής θεμάτων σχετικά με το Azure Active Directory αναφοράς API.  
Azure AD αναφοράς σάς παρέχει ένα API που σας επιτρέπει να αποκτήσετε πρόσβαση σε δεδομένα ελέγχου με χρήση κώδικα ή σχετικά εργαλεία.
Η εμβέλεια αυτού του θέματος είναι να σας παρέχει πληροφορίες αναφοράς σχετικά με το **API ελέγχου**.

Ανατρέξτε στα θέματα:

- Για περισσότερες πληροφορίες εννοιολογική [αρχείων καταγραφής ελέγχου](active-directory-reporting-azure-portal.md#audit-logs)
- [Γρήγορα αποτελέσματα με το Azure Active Directory αναφοράς API](active-directory-reporting-api-getting-started.md) για περισσότερες πληροφορίες σχετικά με το API αναφοράς.

Για ερωτήσεις, θέματα ή σχόλια, επικοινωνήστε με την [Αναφορά συμβάλλει AAD](mailto:aadreportinghelp@microsoft.com).


## <a name="who-can-access-the-data"></a>Ποιος έχει πρόσβαση στα δεδομένα;

- Οι χρήστες το ρόλο διαχειριστή ασφαλείας ή ανάγνωσης ασφαλείας

- Οι καθολικοί διαχειριστές

- Οποιαδήποτε εφαρμογή που διαθέτει εξουσιοδότηση πρόσβασης το API (εξουσιοδότησης εφαρμογή μπορεί να είναι το πρόγραμμα εγκατάστασης μόνο βάσει δικαιωμάτων καθολικός διαχειριστής)



## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να αποκτήσετε πρόσβαση σε αυτήν την αναφορά μέσω του API αναφοράς, πρέπει να έχετε:

- Μια [Azure Active Directory δωρεάν ή καλύτερη edition](active-directory-editions.md)

- Ολοκλήρωση τις [προϋποθέσεις για την πρόσβαση του Azure AD αναφοράς API](active-directory-reporting-api-prerequisites.md). 
 

##<a name="accessing-the-api"></a>Πρόσβαση σε το API

Μπορείτε είτε να αποκτήσετε πρόσβαση σε αυτό το API μέσω της [Εξερεύνησης Graph](https://graphexplorer2.cloudapp.net) ή μέσω προγραμματισμού χρησιμοποιώντας, για παράδειγμα, το PowerShell. Με τη σειρά για το PowerShell για να ερμηνεύσετε σωστά τη σύνταξη του φίλτρου OData χρησιμοποιούνται στο ΥΠΌΛΟΙΠΟ γράφημα AAD κλήσεις, πρέπει να χρησιμοποιήσετε το backtick (ή: βαρεία) χαρακτήρα "escape" το χαρακτήρα $. Ο χαρακτήρας backtick λειτουργεί ως [χαρακτήρα διαφυγής του PowerShell](https://technet.microsoft.com/library/hh847755.aspx), επιτρέποντάς του PowerShell για να κάνετε ένα ακριβές ερμηνεία του χαρακτήρα $, και να αποφευχθεί η σύγχυση του ως όνομα μεταβλητής PowerShell (ie: $filter).

Αυτό το θέμα εστιάζει στην Εξερεύνηση του γραφήματος. Για ένα παράδειγμα PowerShell, ανατρέξτε στο θέμα αυτό [δέσμη ενεργειών του PowerShell](active-directory-reporting-api-audit-samples.md#powershell-script).

 
 

## <a name="api-endpoint"></a>API τελικού σημείου


Μπορείτε να αποκτήσετε πρόσβαση σε αυτό το API χρησιμοποιώντας το παρακάτω URI:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Υπάρχει όριο στον αριθμό των εγγραφών που επιστρέφονται από το API ελέγχου Azure AD (χρησιμοποιώντας το OData σελιδοποίηση).
Για διατήρησης όρια για δεδομένα αναφοράς, ανατρέξτε στο θέμα [Δημιουργία αναφορών πολιτικές διατήρησης](active-directory-reporting-retention.md).

Αυτή η κλήση επιστρέφει τα δεδομένα σε δέσμες. Κάθε δέσμη έχει έως 1000 εγγραφές.  
Για να λάβετε την επόμενη δέσμη εγγραφών, χρησιμοποιήστε την επόμενη σύνδεση. Λάβετε τις πληροφορίες skiptoken από το πρώτο σύνολο των επιστρεφόμενων εγγραφών. Το διακριτικό παράλειψη ρύθμιση θα γίνει στο τέλος του αποτελέσματος.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Υποστηριζόμενες φίλτρα

Μπορείτε να περιορίσετε τον αριθμό των εγγραφών που επιστρέφονται από ένα API κλήσεων στη φόρμα ενός φίλτρου.  
Για το API εισόδου που σχετίζονται με τα δεδομένα, τα ακόλουθα φίλτρα που υποστηρίζονται:

- **$top =\<αριθμού εγγραφών που θα επιστραφούν\> ** - για να περιορίσετε τον αριθμό των επιστρεφόμενων εγγραφών. Αυτή είναι μια ακριβή λειτουργία. Δεν πρέπει να χρησιμοποιείτε αυτό το φίλτρο, εάν θέλετε να επιστρέψετε χιλιάδες αντικείμενα.     
- **$filter =\<σας δήλωση φίλτρο\> ** - για να καθορίσετε, με βάση τα πεδία υποστηριζόμενες φίλτρου, τον τύπο των καρτελών που σας ενδιαφέρουν



## <a name="supported-filter-fields-and-operators"></a>Υποστηριζόμενες φίλτρο πεδία και τελεστών

Για να καθορίσετε τον τύπο των καρτελών που σας ενδιαφέρουν, μπορείτε να δημιουργήσετε μια πρόταση φίλτρου που μπορεί να περιέχει μία ή ένα συνδυασμό των ακόλουθων πεδίων φίλτρου:

- [η ημερομηνία δραστηριότητας](#activitydate) - καθορίζει μια ημερομηνία ή περιοχή ημερομηνιών
- [Τύπος δραστηριότητας](#activitytype) - καθορίζει τον τύπο μιας δραστηριότητας
- [δραστηριότητα](#activity) - ορίζει τη δραστηριότητα ως συμβολοσειρά  
- [Όνομα/ηθοποιού](#actorname) - ορίζει το παράγοντα μορφή ονόματος του παράγοντα
- [παράγοντα/objectid](#actorobjectid) - ορίζει το παράγοντα στη φόρμα της Ταυτότητας του παράγοντα   
- [παράγοντα/upn](#actorupn) - ορίζει το παράγοντα στη φόρμα από το κύριο όνομα χρήστη (UPN) του παράγοντα 
- [όνομα/προορισμού](#targetname) - ορίζει προορισμού στη φόρμα το όνομα του παράγοντα
- [προορισμού/objectid](#targetobjectid) - ορίζει προορισμού στη φόρμα από το Αναγνωριστικό του προορισμού  
- [προορισμού/upn](#targetupn) - ορίζει το παράγοντα στη φόρμα από το κύριο όνομα χρήστη (UPN) του παράγοντα   




----------

### <a name="activitydate"></a>η ημερομηνία δραστηριότητας

**Υποστηριζόμενα τελεστές**: eq, σελίδας, le, gt, lt

**Παράδειγμα**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**Σημειώσεις**:

DateTime πρέπει να είναι σε μορφή UTC

----------

### <a name="activitytype"></a>Τύπος δραστηριότητας

**Υποστηριζόμενα τελεστές**: eq

**Παράδειγμα**:

    $filter=activityType eq 'User'  

**Σημειώσεις**:

διάκριση πεζών-κεφαλαίων

----------

### <a name="activity"></a>δραστηριότητα

**Υποστηριζόμενα τελεστές**: eq, περιέχει, startsWith

**Παράδειγμα**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**Σημειώσεις**:

διάκριση πεζών-κεφαλαίων

----------

### <a name="actorname"></a>Όνομα/ηθοποιού

**Υποστηριζόμενα τελεστές**: eq, περιέχει, startsWith

**Παράδειγμα**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**Σημειώσεις**:

διάκριση πεζών-κεφαλαίων

    

----------
### <a name="actorobjectid"></a>παράγοντα/objectId

**Υποστηριζόμενα τελεστές**: eq

**Παράδειγμα**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>όνομα/προορισμού

**Υποστηριζόμενα τελεστές**: eq, περιέχει, startsWith

**Παράδειγμα**:

    $filter=targets/any(t: t/name eq 'some name')   

**Σημειώσεις**:

Διάκριση πεζών-κεφαλαίων

----------

### <a name="targetupn"></a>προορισμού/upn

**Υποστηριζόμενα τελεστές**: eq, startsWith

**Παράδειγμα**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**Σημειώσεις**:

- Διάκριση πεζών-κεφαλαίων
- Πρέπει να προσθέσετε τον πλήρη χώρο ονομάτων κατά την υποβολή ερωτήματος Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

----------

### <a name="targetobjectid"></a>προορισμού/objectId

**Υποστηριζόμενα τελεστές**: eq

**Παράδειγμα**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>παράγοντα/upn

**Υποστηριζόμενα τελεστές**: eq, startsWith

**Παράδειγμα**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**Σημειώσεις**:

- Διάκριση πεζών-κεφαλαίων 
- Πρέπει να προσθέσετε τον πλήρη χώρο ονομάτων κατά την υποβολή ερωτήματος Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

----------




## <a name="next-steps"></a>Επόμενα βήματα

- Θέλετε να δείτε παραδείγματα για δραστηριότητες φιλτραρισμένη συστήματος; Δείτε τα [δείγματα API ελέγχου Azure Active Directory](active-directory-reporting-api-audit-samples.md).

- Θέλετε να μάθετε περισσότερα σχετικά με το Azure AD αναφοράς API; Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure Active Directory αναφοράς API](active-directory-reporting-api-getting-started.md).