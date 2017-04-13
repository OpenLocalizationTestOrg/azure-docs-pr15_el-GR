<properties
    pageTitle="B2C καταλόγου Azure Active Directory | Microsoft Azure"
    description="Οι τύποι των διακριτικά εκδοθεί στο το B2C Azure Active Directory."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>


# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Αναφορά διακριτικού

Azure Active Directory (Azure AD) B2C εκπέμπει διάφορους τύπους διακριτικά ασφαλείας καθώς επεξεργάζεται κάθε [ροή ελέγχου ταυτότητας](active-directory-b2c-apps.md). Αυτό το έγγραφο περιγράφει τη μορφή, χαρακτηριστικά ασφαλείας και περιεχόμενα του κάθε τύπο διακριτικού.

## <a name="types-of-tokens"></a>Τύποι διακριτικά

Azure AD B2C υποστηρίζει το [πρωτόκολλο εξουσιοδότησης διακριτικό 2.0](active-directory-b2c-reference-protocols.md), που χρησιμοποιεί το διακριτικά πρόσβασης και διακριτικά ανανέωσης. Επίσης, υποστηρίζει τον έλεγχο ταυτότητας και είσοδος μέσω [Σύνδεση OpenID](active-directory-b2c-reference-protocols.md), που παρουσιάζει ένα τρίτο τύπο διακριτικού: το διακριτικό Αναγνωριστικό. Κάθε ένα από αυτά τα διακριτικά αναπαρίσταται ως διακριτικό φορέα.

Διακριτικό φορέα είναι διακριτικό ελαφριά ασφαλείας που εκχωρεί πρόσβαση "φορέα" σε ένα προστατευμένο πόρο. Το φορέα είναι οποιοδήποτε μέρος που μπορεί να παρουσιάζουν το διακριτικό. Azure AD πρέπει να πρώτα τον έλεγχο ταυτότητας ένα πάρτι πριν να κάνει λήψη ένα διακριτικό φορέα. Αλλά, εάν δεν έχουν ληφθεί τα απαραίτητα βήματα για την ασφάλιση το διακριτικό στο μετάδοση και το χώρο αποθήκευσης, μπορεί να υποκλοπής και να χρησιμοποιούνται από ένα ανεπιθύμητα μέρη. Ορισμένες διακριτικά ασφαλείας έχουν ένα ενσωματωμένο μηχανισμό για την αποτροπή μη εξουσιοδοτημένα άτομα από τη χρήση τους, αλλά τα διακριτικά φορέα δεν χρειάζεται ο μηχανισμός. Πρέπει να μεταφέρονται σε ένα κανάλι ασφαλής, όπως η ασφάλεια επιπέδου μεταφοράς (HTTPS).

Εάν ένα διακριτικό φορέα μεταδίδονται εκτός της ασφαλούς καναλιού, ένα κακόβουλο άτομο να χρησιμοποιήσετε μια επίθεση στο μέσο για να αποκτήσετε το διακριτικό και να το χρησιμοποιήσετε για να αποκτήσει μη εξουσιοδοτημένη πρόσβαση σε έναν πόρο προστατευμένο. Τις ίδιες βασικές αρχές ασφαλείας ισχύουν όταν τα διακριτικά φορέα αποθήκευσης ή στο cache για μελλοντική χρήση. Πάντα βεβαιωθείτε ότι η εφαρμογή σας μεταδίδει και αποθηκεύει τα διακριτικά φορέα με ασφαλή τρόπο.

Για επιπλέον ζητήματα ασφαλείας στην διακριτικά φορέα, ανατρέξτε στο θέμα [RFC 6750 τμήμα 5](http://tools.ietf.org/html/rfc6750).

Πολλά από τα διακριτικά που ζητήματα Azure AD B2C υλοποιούνται ως JSON διακριτικά web (JWTs). Ένα JWT είναι μια συμπαγή, διεύθυνση URL ασφαλή τρόπο για τη μεταφορά πληροφοριών ανάμεσα σε δύο μέρη. JWTs περιέχουν πληροφορίες γνωστό ως αξιώσεων. Αυτά είναι διεκδικήσεων πληροφοριών σχετικά με το φορέα και το θέμα του διακριτικού. Το αξιώσεων στο JWTs είναι JSON αντικείμενα που κωδικοποιούνται και σειριοποιηθεί για τη μετάδοση. Επειδή το JWTs εκδοθεί από Azure AD B2C είστε συνδεδεμένοι, αλλά δεν κρυπτογραφημένα, μπορείτε να ελέγξετε εύκολα τα περιεχόμενα ενός JWT για τον εντοπισμό σφαλμάτων σε αυτό. Υπάρχουν διαθέσιμα πολλά εργαλεία που μπορούν να το κάνετε αυτό, συμπεριλαμβανομένων των [calebb.net](http://calebb.net). Για περισσότερες πληροφορίες σχετικά με το JWTs, ανατρέξτε στο [JWT προδιαγραφές](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Αναγνωριστικό διακριτικά

Ένα Αναγνωριστικό διακριτικό είναι μια φόρμα του διακριτικού ασφαλείας που λαμβάνει την εφαρμογή από το B2C Azure AD `authorize` και `token` τα τελικά σημεία. Αναγνωριστικό διακριτικά παρουσιάζονται ως [JWTs](#types-of-tokens)και περιέχουν αξιώσεων που μπορείτε να χρησιμοποιήσετε για να προσδιορίσετε τους χρήστες στην εφαρμογή. Όταν είναι αποκτήθηκε Αναγνωριστικό διακριτικά από το `authorize` τελικού σημείου, χρησιμοποιούνται συχνά για να πραγματοποιήσετε είσοδο χρήστες σε εφαρμογές web. Όταν είναι αποκτήθηκε διακριτικά Αναγνωριστικό από το `token` τελικού σημείου, μπορούν να σταλούν σε αιτήσεις HTTP κατά την επικοινωνία μεταξύ των δύο στοιχεία της ίδιας εφαρμογή ή υπηρεσία. Μπορείτε να χρησιμοποιήσετε το αξιώσεων σε ένα Αναγνωριστικό διακριτικό που βλέπετε ότι ταιριάζουν. Που χρησιμοποιούνται ευρέως για να εμφανίσετε πληροφορίες λογαριασμού ή για τη λήψη αποφάσεων ελέγχου πρόσβασης σε μια εφαρμογή.  

Αναγνωριστικό διακριτικά έχουν υπογραφή, αλλά δεν είναι κρυπτογραφημένα αυτήν τη στιγμή. Όταν σας εφαρμογή ή API λαμβάνει ένα Αναγνωριστικό διακριτικό, πρέπει να [επικύρωση της υπογραφής](#token-validation) για να αποδείξετε ότι το διακριτικό είναι αυθεντικό. Εφαρμογή ή API σας πρέπει να επικυρώσετε μερικά αξιώσεων στο διακριτικό για να αποδείξετε ότι είναι έγκυρο. Ανάλογα με τις απαιτήσεις σενάριο, μπορεί να διαφέρει το αξιώσεων επικυρώνονται από μια εφαρμογή, αλλά την εφαρμογή σας πρέπει να εκτελέσετε ορισμένες [κοινές διεκδίκηση επικυρώσεων](#token-validation) σε κάθε σενάριο.

#### <a name="sample-id-token"></a>Δείγμα Αναγνωριστικό διακριτικού
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Διακριτικά πρόσβασης

Ένα διακριτικό πρόσβασης είναι επίσης μια φόρμα του διακριτικού ασφαλείας που λαμβάνει την εφαρμογή από το B2C Azure AD `authorize` και `token` τα τελικά σημεία. Διακριτικά πρόσβασης παρουσιάζονται επίσης ως [JWTs](#types-of-tokens)και περιέχουν αξιώσεων που μπορείτε να χρησιμοποιήσετε για να προσδιορίσετε τους χρήστες σας υπηρεσίες web & APIs.

Διακριτικά πρόσβασης έχουν υπογραφή, αλλά δεν είναι κρυπτογραφημένο αυτήν τη στιγμή - και παρόμοια με τα διακριτικά αναγνωριστικό.  Διακριτικά πρόσβασης πρέπει να χρησιμοποιείται για να παρέχουν πρόσβαση σε υπηρεσίες web & APIs και για να προσδιορίσετε & έλεγχο ταυτότητας του χρήστη σε αυτές τις υπηρεσίες.  Ωστόσο, δεν παρέχουν οποιαδήποτε διεκδίκηση της άδειας σε αυτές τις υπηρεσίες.  Αυτό σημαίνει ότι που το `scp` διεκδίκηση στο διακριτικά πρόσβασης δεν περιορίζει ή αλλιώς αντιπροσωπεύει την πρόσβαση που έχει εκχωρηθεί το θέμα του διακριτικού.

Όταν το API λαμβάνει ένα διακριτικό πρόσβασης, πρέπει να [επικυρώσετε την υπογραφή](#token-validation) για να αποδείξετε ότι το διακριτικό είναι αυθεντικό. Το API πρέπει να επικυρώσετε τη μερικά αξιώσεων στο διακριτικό για να αποδείξετε ότι είναι έγκυρο. Ανάλογα με τις απαιτήσεις σενάριο, μπορεί να διαφέρει το αξιώσεων επικυρώνονται από μια εφαρμογή, αλλά την εφαρμογή σας πρέπει να εκτελέσετε ορισμένες [κοινές διεκδίκηση επικυρώσεων](#token-validation) σε κάθε σενάριο.

### <a name="claims-in-id--access-tokens"></a>Αξιώσεις σε Αναγνωριστικό & διακριτικά πρόσβασης

Όταν χρησιμοποιείτε Azure AD B2C, έχετε έλεγχο ακριβείας για το περιεχόμενο του διακριτικά σας. Μπορείτε να ρυθμίσετε [πολιτικές](active-directory-b2c-reference-policies.md) για να στείλετε ορισμένα σύνολα δεδομένων χρήστη σε αξιώσεων που απαιτεί την εφαρμογή σας για τις λειτουργίες. Αυτές οι αξιώσεις μπορούν να περιλαμβάνουν τυπικές ιδιότητες, όπως ο χρήστης `displayName` και `emailAddress`. Επίσης, να περιλαμβάνουν [χρήστη προσαρμοσμένα χαρακτηριστικά](active-directory-b2c-reference-custom-attr.md) που μπορείτε να ορίσετε στον κατάλογό σας B2C. Κάθε Αναγνωριστικό & access διακριτικό που λαμβάνετε θα περιέχει ένα συγκεκριμένο σύνολο αξιώσεων σχετικά με την ασφάλεια. Εφαρμογές σας να χρησιμοποιήσετε αυτές τις απαιτήσεις για ασφαλή έλεγχο ταυτότητας χρηστών και προσκλήσεις.

Σημειώστε ότι η αξιώσεων σε Αναγνωριστικό διακριτικά δεν επιστρέφονται με κάποια συγκεκριμένη σειρά. Επιπλέον, μπορεί να εισαχθεί νέα αξιώσεων σε Αναγνωριστικό διακριτικά οποιαδήποτε στιγμή. Την εφαρμογή σας δεν θα πρέπει να καταργήσουν ως νέα αξιώσεων εισάγονται. Ακολουθούν οι δηλώσεις που θα έπρεπε να υπάρχει στο Αναγνωριστικό & access διακριτικά εκδοθεί από Azure AD B2C. Τυχόν πρόσθετες απαιτήσεις θα προσδιοριστεί από τις πολιτικές. Για την πρακτική εξάσκηση, δοκιμάστε να έλεγχο των απαιτήσεων στο δείγμα Αναγνωριστικό διακριτικό, επικολλώντας τον στο [calebb.net](http://calebb.net). Μπορείτε να βρείτε περισσότερες λεπτομέρειες στην [προδιαγραφή OpenID σύνδεση](http://openid.net/specs/openid-connect-core-1_0.html).

| Όνομα | Διεκδίκηση | Παράδειγμα τιμής | Περιγραφή |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Ακροατηρίου | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Μια απαίτηση ακροατηρίου προσδιορίζει στον παραλήπτη που θέλετε από το διακριτικό. Azure AD B2C, το κοινό είναι Αναγνωριστικό εφαρμογής της εφαρμογής σας, όπως στους οποίους έχουν ανατεθεί την εφαρμογή σας στην πύλη καταχώρηση εφαρμογής. Την εφαρμογή σας θα πρέπει να Επικυρώστε αυτήν την τιμή και να απορρίψετε το διακριτικό, εάν δεν ταιριάζει. |
| Εκδότης | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Το αίτημα αυτό προσδιορίζει την υπηρεσία διακριτικού ασφαλείας (STS), το που κατασκευάζει και επιστρέφει το διακριτικό. Προσδιορίζει επίσης τον κατάλογο Azure AD με την οποία ο χρήστης έχει ελεγχθεί η ταυτότητα. Εφαρμογή σας θα πρέπει να επικυρώσετε τη διεκδίκηση εκδότη για να βεβαιωθείτε ότι το διακριτικό προήλθε από το τελικό σημείο v2.0. |
| Εκδοθεί στο | `iat` | `1438535543` | Το αίτημα αυτό είναι ο χρόνος στο οποίο έχει εκδοθεί το διακριτικό, που αντιπροσωπεύονται σε χρονική σήμανση χρόνου. |
| Ώρα λήξης | `exp` | `1438539443` | Η ώρα λήξης διεκδίκηση είναι η ώρα κατά την οποία το διακριτικό μετατρέπεται σε μη έγκυρη, που αντιπροσωπεύονται σε χρονική σήμανση χρόνου. Εφαρμογή σας πρέπει να χρησιμοποιήσετε το αίτημα για να επιβεβαιώσετε την εγκυρότητα της διάρκειας ζωής διακριτικού.  |
| Όχι πριν από | `nbf` | `1438535543` | Το αίτημα αυτό είναι ο χρόνος με τον οποίο το διακριτικό θα είναι έγκυρο, αναπαρίσταται σε χρονική σήμανση χρόνου. Αυτή είναι συνήθως η ίδια με την ώρα που έχει εκδοθεί από το διακριτικό. Εφαρμογή σας πρέπει να χρησιμοποιήσετε το αίτημα για να επιβεβαιώσετε την εγκυρότητα της διάρκειας ζωής διακριτικού.  |
| Έκδοση | `ver` | `1.0` | Αυτή είναι η έκδοση του διακριτικού Αναγνωριστικό, όπως ορίζεται από το Azure AD. |
| Κατακερματισμός κώδικα | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Ο κατακερματισμός κώδικα περιλαμβάνεται σε ένα Αναγνωριστικό διακριτικού μόνο όταν το διακριτικό εκδίδεται μαζί με έναν κωδικό εξουσιοδότησης διακριτικό 2.0. Ο κατακερματισμός κώδικας μπορεί να χρησιμοποιηθεί για την επικύρωση της αυθεντικότητας έναν κωδικό εξουσιοδότησης. Δείτε τη [σύνδεση OpenID προδιαγραφή](http://openid.net/specs/openid-connect-core-1_0.html) για περισσότερες λεπτομέρειες σχετικά με τον τρόπο για να εκτελέσετε αυτή η επικύρωση. |
| Κατακερματισμός διακριτικό πρόσβασης | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Μια κατακερματισμός διακριτικό πρόσβασης περιλαμβάνει ένα Αναγνωριστικό διακριτικό μόνο όταν το διακριτικό εκδίδεται μαζί με ένα διακριτικό πρόσβασης OAuth 2.0. Μια κατακερματισμός διακριτικού πρόσβασης μπορεί να χρησιμοποιηθεί για την επικύρωση της αυθεντικότητας ένα διακριτικό πρόσβασης. Δείτε τη [σύνδεση OpenID προδιαγραφή](http://openid.net/specs/openid-connect-core-1_0.html) για περισσότερες λεπτομέρειες σχετικά με τον τρόπο για να εκτελέσετε αυτή η επικύρωση. |
| Nonce | `nonce` | `12345` | Μια nonce είναι μια στρατηγική που χρησιμοποιείται για τον περιορισμό επιθέσεις διακριτικού αναπαραγωγής. Εφαρμογή σας να καθορίσετε μια nonce σε μια πρόσκληση σε εξουσιοδότησης χρησιμοποιώντας το `nonce` ερώτημα παραμέτρων. Η τιμή που παρέχετε στην πρόσκληση σε θα είναι που εκπέμπει χωρίς τροποποίηση στο το `nonce` διεκδίκηση μόνο ενός διακριτικού Αναγνωριστικό. Αυτό σας επιτρέπει την εφαρμογή σας για να επιβεβαιώσετε την τιμή έναντι της τιμής του που καθορίζεται στο την αίτηση, η οποία associates περίοδο λειτουργίας της εφαρμογής με δεδομένη διακριτικό Αναγνωριστικό. Εφαρμογή σας θα πρέπει να εκτελεί αυτό επικύρωση κατά τη διαδικασία διακριτικού επικύρωσης Αναγνωριστικό. |
| Θέμα | `sub` | `Not supported currently. Use oid claim.` | Αυτό είναι ένα κεφάλαιο σχετικά με την οποία το διακριτικό διεκδικήσεις πληροφορίες, όπως ο χρήστης της εφαρμογής. Αυτή η τιμή είναι αμετάβλητες και δεν μπορούν να εκ νέου ή να τα χρησιμοποιήσετε πάλι. Μπορεί να χρησιμοποιηθεί για την εκτέλεση ελέγχων εξουσιοδότησης ασφαλής, όπως όταν το διακριτικό χρησιμοποιείται για να αποκτήσετε πρόσβαση σε έναν πόρο. Ωστόσο, το θέμα αίτημα δεν έχει υλοποιηθεί ακόμη στο το B2C Azure AD. Θα πρέπει να ρυθμίσετε τις πολιτικές σας για να συμπεριλάβετε το Αναγνωριστικό αντικειμένου `oid` διεκδίκηση και χρησιμοποιήστε την τιμή για να Προσδιορίστε τους χρήστες, αντί να χρησιμοποιήσετε το θέμα αίτημα για έλεγχο ταυτότητας. |
| Αναφορά κλάσης περιβάλλον ελέγχου ταυτότητας | `acr` | `b2c_1_sign_in` | Αυτό είναι το όνομα της πολιτικής που χρησιμοποιήθηκε για να αποκτήσετε το διακριτικό Αναγνωριστικό.  |
| Έλεγχος ταυτότητας χρόνου | `auth_time` | `1438535543` | Το αίτημα αυτό είναι ο χρόνος με τον οποίο ένα χρήστη τελευταίας καταχώρησης τα διαπιστευτήρια, που αντιπροσωπεύονται σε χρονική σήμανση χρόνου. |


### <a name="refresh-tokens"></a>Ανανεώστε τα διακριτικά

Ανανεώστε τα διακριτικά είναι διακριτικά ασφάλειας που να χρησιμοποιήσετε την εφαρμογή σας για να αποκτήσετε νέο Αναγνωριστικό διακριτικά και πρόσβασης διακριτικά σε μια ροή διακριτικό 2.0. Παρέχουν την εφαρμογή σας με μακροπρόθεσμες πρόσβασης σε πόρους εκ μέρους χρήστες χωρίς να απαιτείται αλληλεπίδραση με αυτούς τους χρήστες.

Για να λάβετε μια ανανέωση διακριτικού στο διακριτικού απάντηση, σας πρέπει να αίτησης εφαρμογής του `offline_acesss` εύρος. Για να μάθετε περισσότερα σχετικά με το `offline_access` εμβέλεια, ανατρέξτε στην [αναφορά πρωτόκολλο Azure AD B2C](active-directory-b2c-reference-protocols.md).

Ανανεώστε τα διακριτικά είναι και θα είναι πάντα, αδιαφανές για την εφαρμογή σας. Αυτές εκδίδονται από Azure AD και μπορεί να είναι επιθεώρηση και ερμηνεύεται μόνο από Azure AD. Είναι μεγάλης διάρκειας, αλλά την εφαρμογή σας δεν πρέπει να γράφονται με την προοπτική που θα τελευταίος ενός διακριτικού ανανέωσης για μια συγκεκριμένη χρονική περίοδο. Ανανέωση διακριτικά μπορεί να ακυρωθεί ανά πάσα στιγμή, για διάφορους λόγους. Ο μόνος τρόπος για να γνωρίζω αν μια ανανέωση διακριτικού είναι έγκυρη εφαρμογή σας είναι να δοκιμάσετε να την εξαργύρωση, κάνοντας μια αίτηση διακριτικού Azure AD.

Κατά την εξαργύρωση ενός διακριτικού ανανέωσης για έναν νέο κωδικό (και εάν η εφαρμογή σας έχει παραχωρηθεί τα `offline_access` εύρος), θα λάβετε έναν νέο κωδικό ανανέωση στην διακριτικού απάντηση. Θα πρέπει να αποθηκεύσετε το διακριτικό που μόλις έχει εκδοθεί ανανέωσης. Αυτό πρέπει να αντικαταστήσετε το διακριτικό ανανέωση είχατε χρησιμοποιήσει προηγουμένως στην αίτηση. Αυτό σας βοηθά να εξασφαλίσετε ότι σας διακριτικά ανανέωση εξακολουθεί να ισχύει για το χρονικό διάστημα όσο το δυνατόν.

## <a name="token-validation"></a>Επικύρωση διακριτικού

Για να επικυρώσετε ενός διακριτικού, την εφαρμογή σας πρέπει να ελέγξετε η υπογραφή και αξιώσεων του διακριτικού.

Πολλές βιβλιοθήκες Άνοιγμα αρχείου προέλευσης είναι διαθέσιμες για την επικύρωση JWTs, ανάλογα με τη γλώσσα που προτιμάτε. Συνιστάται να εξερευνήσετε αυτές τις επιλογές αντί να υλοποιήσετε το δικό σας λογικής επικύρωσης. Οι πληροφορίες σε αυτόν τον Οδηγό μπορεί να σας βοηθήσει να μάθετε πώς να χρησιμοποιείτε σωστά αυτές τις βιβλιοθήκες.

### <a name="validate-the-signature"></a>Επικύρωση της υπογραφής
Μια JWT περιέχει τρία τμήματα, διαχωρισμένα με το `.` χαρακτήρα. Το πρώτο τμήμα είναι η **κεφαλίδα**, το δεύτερο είναι το **κύριο σώμα**και το τρίτο είναι η **υπογραφή**. Στο τμήμα υπογραφής μπορεί να χρησιμοποιηθεί για την επικύρωση της αυθεντικότητας το διακριτικό, έτσι ώστε να είναι αξιόπιστο από την εφαρμογή σας.

Azure AD B2C διακριτικά έχετε εισέλθει με τη χρήση αλγόριθμους κρυπτογράφησης ασύμμετρη βιομηχανικών προτύπων, όπως RSA 256. Η κεφαλίδα του διακριτικού περιέχει πληροφορίες σχετικά με τη μέθοδο κλειδί και κρυπτογράφησης που χρησιμοποιείται για την είσοδο το διακριτικό:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

Το `alg` διεκδίκηση υποδεικνύει τον αλγόριθμο που χρησιμοποιήθηκε για να πραγματοποιήσετε το διακριτικό. Το `kid` διεκδίκηση υποδεικνύει το συγκεκριμένο δημόσιο κλειδί που χρησιμοποιήθηκε για να πραγματοποιήσετε το διακριτικό.

Οποιαδήποτε στιγμή, Azure AD μπορεί να συνδεθείτε ενός διακριτικού, χρησιμοποιώντας ένα από ένα ορισμένο σύνολο ζεύγη δημόσιων-ιδιωτικών κλειδιών. Azure AD Περιστρέφει το δυνατό σύνολο πλήκτρα περιοδικά, ώστε να την εφαρμογή σας πρέπει να γράφονται χειρισμού αυτόματα αυτές τις βασικές αλλαγές. Ένα λογικό συχνότητα για να ελέγξετε για ενημερώσεις για τη δημόσια κλειδιά που χρησιμοποιούνται από το Azure AD είναι κάθε 24 ώρες.

Azure AD B2C έχει ένα τελικό σημείο σύνδεσης OpenID μετα-δεδομένων. Αυτό σας επιτρέπει εφαρμογών για τη λήψη πληροφοριών σχετικά με το Azure AD B2C κατά το χρόνο εκτέλεσης. Αυτές οι πληροφορίες περιλαμβάνουν τα τελικά σημεία διακριτικού περιεχομένων και διακριτικό υπογραφής πλήκτρα. Στον κατάλογο B2C περιέχει ένα έγγραφο μετα-δεδομένων JSON για κάθε πολιτική. Για παράδειγμα, το έγγραφο μετα-δεδομένων για το `b2c_1_sign_in` πολιτικής στην το `fabrikamb2c.onmicrosoft.com` βρίσκεται στο:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`είναι ο κατάλογος B2C που χρησιμοποιείται για τον έλεγχο ταυτότητας χρήστη, και `b2c_1_sign_in` είναι η πολιτική που χρησιμοποιείται για να αποκτήσετε το διακριτικό. Για να προσδιορίσετε την πολιτική που χρησιμοποιήθηκε για την υπογραφή ενός διακριτικού (και πού να απευθυνθείτε για τη λήψη τα μετα-δεδομένα), έχετε δύο επιλογές. Πρώτα, περιλαμβάνει το όνομα της πολιτικής του `acr` διεκδίκηση στο διακριτικό. Μπορείτε να ανάλυση αξιώσεων από το σώμα του JWT από βάσης 64 αποκωδικοποίηση σώμα και αποσειριοποίηση τη συμβολοσειρά JSON που προκύπτει. Το `acr` διεκδίκηση θα είναι το όνομα της πολιτικής που χρησιμοποιήθηκε για την έκδοση το διακριτικό.  Σας άλλη επιλογή είναι να κωδικοποιείτε την πολιτική στην τιμή του `state` παραμέτρου κατά την έκδοση της αίτησης και, στη συνέχεια, αποκωδικοποιείτε για να προσδιορίσετε την πολιτική που χρησιμοποιήθηκε. Είτε η μέθοδος είναι έγκυρη.

Το έγγραφο μετα-δεδομένων είναι ένα αντικείμενο JSON που περιέχει πολλά τμήματα χρήσιμες πληροφορίες. Αυτά περιλαμβάνουν τη θέση του τα τελικά σημεία που απαιτούνται για την εκτέλεση OpenID σύνδεση ελέγχου ταυτότητας. Επίσης, περιλαμβάνουν `jwks_uri`, που σας δίνει τη θέση του συνόλου των δημόσιων κλειδιών που χρησιμοποιούνται για να πραγματοποιήσετε διακριτικά. Ότι θέση παρέχεται εδώ, αλλά είναι καλύτερο για τη λήψη της θέσης δυναμικά χρησιμοποιώντας το έγγραφο μετα-δεδομένων και ανάλυση ανάληψη `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

Το έγγραφο JSON βρίσκεται σε αυτήν τη διεύθυνση URL περιέχει όλες τις δημόσιες βασικές πληροφορίες χρησιμοποιείται σε μια συγκεκριμένη χρονική. Να χρησιμοποιήσετε την εφαρμογή σας το `kid` διεκδίκηση της κεφαλίδας JWT για να επιλέξετε το δημόσιο κλειδί στο έγγραφο JSON που χρησιμοποιείται για την υπογραφή ενός συγκεκριμένου διακριτικού. Στη συνέχεια, να μπορεί να εκτελέσει επικύρωση της υπογραφής, χρησιμοποιώντας το σωστό δημόσιο κλειδί και ο υποδεικνυόμενος αλγόριθμος.

Μια περιγραφή του τρόπου εκτέλεσης επικύρωση της υπογραφής είναι εκτός του αντικειμένου αυτού του εγγράφου. Πολλές βιβλιοθήκες Άνοιγμα αρχείου προέλευσης είναι διαθέσιμες για να σας βοηθήσει με αυτό, εάν τον χρειάζεστε.

### <a name="validate-the-claims"></a>Επικυρώστε τις απαιτήσεις
Όταν σας εφαρμογή ή API λαμβάνει ένα Αναγνωριστικό διακριτικό, αυτό πρέπει να κάνετε επίσης πολλές ελέγχων προς τις αντίστοιχες απαιτήσεις στο διακριτικό Αναγνωριστικό. Αυτά περιλαμβάνουν, αλλά δεν είστε υποχρεωμένοι να:

- Το αίτημα **ακροατηρίου** : αυτό επαληθεύει ότι το διακριτικό Αναγνωριστικό που περιμένατε να δοθεί για την εφαρμογή σας.
- Οι δηλώσεις **όχι πριν από** και την **ώρα λήξης** : αυτά Επιβεβαιώστε ότι το διακριτικό Αναγνωριστικό δεν έχει λήξει.
- Το αίτημα **εκδότη** : αυτό επαληθεύει ότι το διακριτικό έχει εκδοθεί για την εφαρμογή σας από Azure AD.
- Το **nonce**: Αυτή είναι μια στρατηγική για μετριασμού επίθεση διακριτικού αναπαραγωγής.

Για μια πλήρη λίστα των επικυρώσεων θα πρέπει να εκτελέσετε την εφαρμογή σας, ανατρέξτε στην [προδιαγραφή OpenID σύνδεση](https://openid.net). Λεπτομέρειες τις αναμενόμενες τιμές για αυτές τις απαιτήσεις περιλαμβάνονται στην προηγούμενη [ενότητα διακριτικού](#types-of-tokens).  

## <a name="token-lifetimes"></a>Η διάρκεια ζωής διακριτικού

Το παρακάτω διακριτικού διάρκεια ζωής παρέχονται για περαιτέρω τις γνώσεις σας. Μπορούν να σας βοηθήσουν κατά την ανάπτυξη και εντοπισμός σφαλμάτων εφαρμογών. Σημειώστε ότι οι εφαρμογές σας δεν πρέπει να υπογράψει κάποιος πρέπει να περιμένουν οποιαδήποτε από αυτές τις διάρκεια ζωής για να παραμένουν σταθερές. Μπορούν να και θα αλλάξει.  Μπορείτε να διαβάσετε περισσότερα σχετικά με την προσαρμογή του διακριτικού διάρκεια ζωής στο Azure AD B2C [εδώ](active-directory-b2c-token-session-sso.md).

| Διακριτικού | Διάρκεια ζωής | Περιγραφή |
| ----------------------- | ------------------------------- | ------------ |
| Αναγνωριστικό διακριτικά | Μία ώρα | Αναγνωριστικό διακριτικά είναι συνήθως έγκυρες για κάθε ώρα. Εφαρμογή web της να χρησιμοποιήσετε αυτήν τη διάρκεια ζωής για να διατηρήσετε το δικό της περιόδους λειτουργίας με χρήστες (συνιστάται). Μπορείτε επίσης να επιλέξετε μια διαφορετική περίοδο λειτουργίας διάρκεια ζωής. Εάν η εφαρμογή σας πρέπει να λάβετε ένα νέο Αναγνωριστικό διακριτικού, πρέπει απλώς να κάνετε μια νέα αίτηση εισόδου Azure AD. Εάν ένας χρήστης έχει μια περίοδο λειτουργίας περιήγησης έγκυρη με Azure AD, αυτός ο χρήστης δεν μπορεί να απαιτείται για να εισαγάγετε ξανά τα διαπιστευτήρια. |
| Ανανεώστε τα διακριτικά | Έως και 14 ημέρες | Ένα μεμονωμένο ανανέωση διακριτικό ισχύει για έως και 14 ημέρες. Ωστόσο, ενός διακριτικού ανανέωσης μπορεί να καταστούν μη έγκυροι οποιαδήποτε στιγμή για διάφορους λόγους. Εφαρμογή σας θα πρέπει να συνεχίσετε να προσπαθήσει να χρησιμοποιήσει ενός διακριτικού ανανέωση μέχρι η αίτηση αποτυγχάνει ή μέχρι την εφαρμογή σας αντικαθιστά το διακριτικό Ανανέωση με ένα νέο.  Ένα διακριτικό ανανέωση επίσης να καταστούν μη έγκυροι εάν έχει περάσει 90 ημέρες από ο χρήστης εισαγάγει τελευταία διαπιστευτήρια. |
| Κωδικοί εξουσιοδότησης | Πέντε λεπτά | Οι κωδικοί εξουσιοδότησης είναι σκόπιμα μικρής διάρκειας. Αυτά θα πρέπει να εξαργυρώσατε αμέσως για διακριτικά πρόσβασης, τα διακριτικά Αναγνωριστικό ή ανανέωση διακριτικά κατά τη λήψη. |