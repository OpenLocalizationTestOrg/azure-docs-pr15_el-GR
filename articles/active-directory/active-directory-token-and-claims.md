 <properties
   pageTitle="Azure AD διακριτικό αναφοράς | Microsoft Azure"
   description="Οδηγός για την κατανόηση και την αξιολόγηση των απαιτήσεων στο τα διακριτικά SAML 2.0 και διακριτικά Web JSON (JWT) που εκδίδονται από Azure Active Directory (AAD)"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Αναφορά διακριτικού Azure AD

Azure Active Directory (Azure AD) εκπέμπει διάφορους τύπους διακριτικά ασφαλείας κατά την επεξεργασία του κάθε ροή ελέγχου ταυτότητας. Αυτό το έγγραφο περιγράφει τη μορφή, χαρακτηριστικά ασφαλείας και περιεχόμενα κάθε τύπο διακριτικού.

## <a name="types-of-tokens"></a>Τύποι διακριτικά

Azure AD υποστηρίζει το [πρωτόκολλο εξουσιοδότησης διακριτικό 2.0](active-directory-protocols-oauth-code.md), που χρησιμοποιεί το access_tokens και refresh_tokens.  Επίσης, υποστηρίζει τον έλεγχο ταυτότητας και είσοδος μέσω [Σύνδεση OpenID](active-directory-protocols-openid-connect-code.md), που παρουσιάζει ένα τρίτο τύπο διακριτικό, το id_token.  Κάθε ένα από αυτά τα διακριτικά αναπαρίσταται ως "διακριτικό φορέα".

Διακριτικό φορέα είναι διακριτικό ελαφριά ασφαλείας που εκχωρεί πρόσβαση "φορέα" σε ένα προστατευμένο πόρο. Με αυτήν την έννοια, "φορέα" είναι οποιοδήποτε μέρος που μπορεί να παρουσιάζουν το διακριτικό. Αν απαιτείται έλεγχος ταυτότητας με Azure AD προκειμένου να λαμβάνουν ένα διακριτικό φορέα, πρέπει να λαμβάνονται τα βήματα για την ασφάλιση το διακριτικό, ώστε να μην υποκλοπής από ένα μέρος ακούσια. Επειδή τα διακριτικά φορέα έχει ένα ενσωματωμένο μηχανισμό για να αποτρέψετε τη μη εξουσιοδοτημένη μέρη με τη χρήση τους, πρέπει να μεταφέρονται σε ένα κανάλι ασφαλούς όπως ασφάλεια επιπέδου μεταφοράς (HTTPS). Εάν ένα διακριτικό φορέα μεταδίδονται στο Απαλοιφή, μια Άντρας στο το μεσαίο επίθεση μπορεί να χρησιμοποιηθεί για να αποκτήσετε το διακριτικό και να αποκτήσει μη εξουσιοδοτημένη πρόσβαση σε έναν πόρο προστατευμένο. Τις ίδιες βασικές αρχές ασφαλείας εφαρμογή κατά την αποθήκευση ή την προσωρινή αποθήκευση διακριτικά φορέα για μελλοντική χρήση. Πάντα βεβαιωθείτε ότι η εφαρμογή σας μεταδίδει και αποθηκεύει τα διακριτικά φορέα με ασφαλή τρόπο. Για περισσότερες ζητήματα ασφαλείας στην διακριτικά φορέα, ανατρέξτε στο θέμα [RFC 6750 τμήμα 5](http://tools.ietf.org/html/rfc6750).

Πολλά από τα διακριτικά εκδοθεί από Azure AD υλοποιούνται ως διακριτικά Web JSON ή JWTs.  Ένα JWT είναι μια συμπαγή, διεύθυνση URL ασφαλή τρόπο για τη μεταφορά πληροφοριών ανάμεσα σε δύο μέρη.  Οι πληροφορίες που περιέχονται στο JWTs είναι γνωστό ως "αξιώσεων" ή διεκδικήσεων των πληροφοριών σχετικά με τον φορέα και το θέμα του διακριτικού.  Το αξιώσεων στο JWTs είναι αντικείμενα JSON κωδικοποιημένα και σειριοποιηθεί για τη μετάδοση.  Επειδή το JWTs εκδοθεί από Azure AD υπογραφή, αλλά δεν κρυπτογραφημένα, μπορείτε να ελέγξετε εύκολα τα περιεχόμενα ενός JWT για σκοπούς εντοπισμού σφαλμάτων.  Υπάρχουν διάφορα εργαλεία που είναι διαθέσιμο για αυτή την περίπτωση, όπως [jwt.calebb.net](http://jwt.calebb.net). Για περισσότερες πληροφορίες σχετικά με JWTs, μπορείτε να αναφερθείτε με την [προδιαγραφή JWT](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens είναι μια φόρμα του διακριτικού ασφαλείας εισόδου που λαμβάνει την εφαρμογή σας κατά την εκτέλεση του ελέγχου ταυτότητας με χρήση της [Σύνδεσης OpenID](active-directory-protocols-openid-connect-code.md).  Μπορούν παρουσιάζονται ως [JWTs](#types-of-tokens)και περιέχουν αξιώσεων που μπορείτε να χρησιμοποιήσετε για είσοδο στο χρήστη στην εφαρμογή σας.  Μπορείτε να χρησιμοποιήσετε το αξιώσεων σε μια id_token όπως βλέπετε ότι ταιριάζουν – συνήθως χρησιμοποιούνται για την εμφάνιση των πληροφοριών του λογαριασμού ή τη λήψη αποφάσεων ελέγχου πρόσβασης σε μια εφαρμογή.

Id_tokens υπογραφή, αλλά δεν κρυπτογραφημένα αυτήν τη στιγμή.  Όταν η εφαρμογή σας λαμβάνει μια id_token, πρέπει να [επικύρωση της υπογραφής](#validating-tokens) για να αποδείξετε το διακριτικό Αυθεντικότητας και επικύρωση μερικά αξιώσεων στο διακριτικό για να αποδείξετε ισχύος του πιστοποιητικού.  Το αξιώσεων επικυρώνονται από μια εφαρμογή ποικίλλουν ανάλογα με τις απαιτήσεις σενάριο, αλλά υπάρχουν ορισμένα [κοινά επικυρώσεων διεκδίκηση](#validating-tokens) που πρέπει να εκτελέσετε την εφαρμογή σας σε κάθε σενάριο.

Ανατρέξτε στην επόμενη ενότητα για πληροφορίες στην id_tokens αξιώσεων, καθώς και ένα δείγμα id_token.  Σημειώστε ότι η αξιώσεων στο id_tokens δεν επιστρέφονται με κάποια συγκεκριμένη σειρά.  Επιπλέον, νέα αξιώσεων μπορεί να εισαχθεί σε id_tokens σε οποιοδήποτε σημείο στο χρόνο - της εφαρμογής σας θα πρέπει να όχι αλλαγή ως νέα αξιώσεων εισάγονται.  Η παρακάτω λίστα περιλαμβάνει τις απαιτήσεις που την εφαρμογή σας μπορεί να ερμηνεύσει αξιόπιστα τη στιγμή της σύνταξης αυτού.  Εάν είναι απαραίτητο, μπορείτε να βρείτε ακόμα περισσότερες λεπτομέρειες στην [προδιαγραφή OpenID σύνδεση](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Δείγμα id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] Για την πρακτική εξάσκηση, δοκιμάστε να έλεγχο των απαιτήσεων στο το δείγμα id_token από την επικόλλησή τους σε [calebb.net](http://jwt.calebb.net).

#### <a name="claims-in-idtokens"></a>Αξιώσεις στο id_tokens

| Διεκδίκηση JWT | Όνομα | Περιγραφή |
|-----------|------|-------------|
| `appid`| Αναγνωριστικό εφαρμογής | Προσδιορίζει την εφαρμογή που χρησιμοποιεί το διακριτικό για την πρόσβαση σε έναν πόρο. Η εφαρμογή μπορεί να λειτουργήσει ως έχει ή εκ μέρους ενός χρήστη. Το Αναγνωριστικό εφαρμογής συνήθως αντιπροσωπεύει ένα αντικείμενο εφαρμογής, αλλά μπορεί επίσης να αντιπροσωπεύει ένα αντικείμενο κεφαλαίου υπηρεσίας στο Azure AD. <br><br> **Παράδειγμα JWT τιμή**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| Ακροατηρίου | Στον παραλήπτη που θέλετε από το διακριτικό. Η εφαρμογή που λαμβάνει το διακριτικό πρέπει να βεβαιωθείτε ότι η τιμή ακροατηρίου είναι σωστή και απορρίψετε οποιαδήποτε διακριτικά που προορίζονται για ένα διαφορετικό ακροατήριο. <br><br> **Παράδειγμα SAML τιμή**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Παράδειγμα JWT τιμή**: <br> `"aud":"https://contoso.com"` |
| `appidacr`| Αναφορά κλάσης περιβάλλον εφαρμογής ελέγχου ταυτότητας | Υποδεικνύει πώς έχει γίνει έλεγχος ταυτότητας του προγράμματος-πελάτη. Για έναν δημόσιο υπολογιστή-πελάτη, η τιμή είναι 0. Εάν χρησιμοποιούνται Αναγνωριστικό υπολογιστή-πελάτη και μυστικό προγράμματος-πελάτη, η τιμή είναι 1. <br><br> **Παράδειγμα JWT τιμή**: <br> `"appidacr": "0"`|
| `acr`| Αναφορά κλάσης περιβάλλον ελέγχου ταυτότητας | Υποδεικνύει πώς το θέμα έχει ελεγχθεί η ταυτότητά, σε αντίθεση με το πρόγραμμα-πελάτη στο το αίτημα αναφοράς κλάσης περιβάλλον εφαρμογής ελέγχου ταυτότητας. Η τιμή "0" δηλώνει ότι ο έλεγχος ταυτότητας του τελικού χρήστη δεν πληροί τις απαιτήσεις του ISO/IEC 29115. <br><br> **Παράδειγμα JWT τιμή**: <br> `"acr": "0"`|
| | Έλεγχος ταυτότητας άμεσων | Καταγράφει την ημερομηνία και την ώρα που παρουσιάστηκε ελέγχου ταυτότητας. <br><br> **Παράδειγμα SAML τιμή**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| Μέθοδος ελέγχου ταυτότητας | Προσδιορίζει τον τρόπο το θέμα του διακριτικού έγινε έλεγχος ταυτότητας. <br><br> **Παράδειγμα SAML τιμή**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Παράδειγμα JWT τιμή**:`“amr”: ["pwd"]` |
| `given_name`| Όνομα | Παρέχει το πρώτο ή "δεδομένο" όνομα του χρήστη, όπως έχουν οριστεί στο αντικείμενο χρήστη Azure AD. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Παράδειγμα JWT τιμή**: <br> `"given_name": "Frank"` |
| `groups`| Ομάδες | Παρέχει αναγνωριστικά αντικειμένων που αναπαριστούν το θέμα μελών ομάδας. Αυτές οι τιμές είναι μοναδικές (δείτε Αναγνωριστικό αντικειμένου) και μπορούν να χρησιμοποιηθούν με ασφάλεια για τη διαχείριση της πρόσβασης, όπως η ενεργοποίηση της εξουσιοδότησης για να αποκτήσετε πρόσβαση σε έναν πόρο. Οι ομάδες που περιλαμβάνονται στο το αίτημα ομάδες έχουν ρυθμιστεί με βάση ανά εφαρμογή, μέσω της ιδιότητας "groupMembershipClaims" το δηλωτικό εφαρμογής. Τιμή null θα αποκλείσετε όλες τις ομάδες, η τιμή "SecurityGroup" θα περιλαμβάνει μόνο τα μέλη ομάδας ασφαλείας Active Directory και η τιμή "Όλα" θα περιλαμβάνουν ομάδες ασφαλείας και λίστες διανομής του Office 365. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Παράδειγμα JWT τιμή**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | Υπηρεσία παροχής ταυτότητας | Καταγράφει την υπηρεσία παροχής ταυτότητας που ελεγχθεί η ταυτότητά το θέμα του διακριτικού. Αυτή η τιμή είναι ίδια με την τιμή της απαίτησης εκδότη, εκτός εάν ο λογαριασμός χρήστη είναι σε έναν διαφορετικό μισθωτή από τον εκδότη. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Παράδειγμα JWT τιμή**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | Αποθηκεύει την ώρα κατά την οποία έχει εκδοθεί το διακριτικό. Χρησιμοποιείται συχνά για τη μέτρηση διακριτικού ενημέρωση. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Παράδειγμα JWT τιμή**: <br> `"iat": 1390234181` |
| `iss` | Εκδότης | Προσδιορίζει την υπηρεσία διακριτικού ασφαλείας (STS), το που κατασκευάζει και επιστρέφει το διακριτικό. Στο τα διακριτικά που επιστρέφει Azure AD, τον εκδότη είναι sts.windows.net. Το GUID την τιμή διεκδίκηση εκδότη είναι το Αναγνωριστικό μισθωτή του καταλόγου Azure AD. Το Αναγνωριστικό μισθωτή είναι ένα αναγνωριστικό αμετάβλητες και αξιόπιστη του καταλόγου. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Παράδειγμα JWT τιμή**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | Επώνυμο | Παρέχει το τελευταίο όνομα, επώνυμο ή συγγενείς όνομα του χρήστη, όπως καθορίζονται στο αντικείμενο χρήστη Azure AD. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Παράδειγμα JWT τιμή**: <br> `"family_name": "Miller"` |
| `unique_name`| Όνομα | Παρέχει μια human αναγνώσιμο τιμή που προσδιορίζει το θέμα του διακριτικού. Αυτή η τιμή δεν εγγυάται για να είναι μοναδικό μέσα σε έναν μισθωτή και έχει σχεδιαστεί για να χρησιμοποιούνται μόνο για σκοπούς εμφάνισης. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Παράδειγμα JWT τιμή**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | Αναγνωριστικό αντικειμένου | Περιέχει ένα μοναδικό αναγνωριστικό ενός αντικειμένου σε Azure AD. Αυτή η τιμή είναι αμετάβλητες και δεν μπορούν να εκ νέου ή να τα χρησιμοποιήσετε πάλι. Χρησιμοποιήστε το Αναγνωριστικό αντικειμένου για τον προσδιορισμό ενός αντικειμένου σε ερωτήματα για να Azure AD. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Παράδειγμα JWT τιμή**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | Ρόλοι | Αντιπροσωπεύει όλους τους ρόλους εφαρμογής που το θέμα έχει παραχωρηθεί άμεσα και έμμεσα μέσω της συμμετοχής σε ομάδα και μπορεί να χρησιμοποιηθεί για την επιβολή έλεγχος πρόσβασης βάσει ρόλων. Ρόλοι της εφαρμογής έχουν οριστεί με βάση ανά εφαρμογή, έως το `appRoles` ιδιότητα τη δήλωση της εφαρμογής. Το `value` ιδιότητα κάθε εφαρμογή του ρόλου είναι η τιμή που εμφανίζεται στο το αίτημα ρόλους. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Παράδειγμα JWT τιμή**: <br> `“roles”: ["Admin", … ]` |
| `scp` | Πεδίο εφαρμογής | Υποδεικνύει τα δικαιώματα απομίμησης την εφαρμογή-πελάτη. Είναι τα προεπιλεγμένα δικαιώματα `user_impersonation`. Ο κάτοχος του πόρου ασφαλές να καταχωρήσετε πρόσθετες τιμές στο Azure AD. <br><br> **Παράδειγμα JWT τιμή**: <br> `"scp": "user_impersonation"`|
| `sub` |Θέμα| Προσδιορίζει το κεφάλαιο σχετικά με την οποία το διακριτικό διεκδικήσεις πληροφοριών, όπως το χρήστη για μια εφαρμογή του. Αυτή η τιμή είναι αμετάβλητες και δεν είναι δυνατό να εκχωρηθούν εκ νέου ή εκ νέου χρήση τους, επομένως, μπορεί να χρησιμοποιηθεί για την εκτέλεση ελέγχων εξουσιοδότησης με ασφάλεια. Επειδή το θέμα βρίσκεται πάντα στο τα διακριτικά τα προβλήματα Azure AD, συνιστάται η χρήση αυτής της τιμής σε ένα σύστημα εξουσιοδότησης γενικής χρήσης. <br> `SubjectConfirmation`δεν είναι αξίωση. Περιγράφει τον τρόπο το θέμα του διακριτικού έχει επαληθευτεί. `Bearer`υποδεικνύει ότι το θέμα έχει επιβεβαιωθεί από την κατοχή του διακριτικού. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Παράδειγμα JWT τιμή**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | Αναγνωριστικό μισθωτή | Ένα αναγνωριστικό αμετάβλητες, μη με δυνατότητα επανάληψης χρήσης που προσδιορίζει ο μισθωτής καταλόγου που εξέδωσε το διακριτικό. Μπορείτε να χρησιμοποιήσετε αυτή την τιμή για να αποκτήσετε πρόσβαση σε συγκεκριμένο μισθωτή καταλόγου πόρων σε μια εφαρμογή πολλών μισθωτή. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε αυτή την τιμή για τον προσδιορισμό του μισθωτή σε μια κλήση για το API Graph. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Παράδειγμα JWT τιμή**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|Διάρκεια ζωής διακριτικού | Καθορίζει το χρονικό διάστημα μέσα στην οποία διακριτικό είναι έγκυρη. Την υπηρεσία που επικυρώσει το διακριτικό θα πρέπει να βεβαιωθείτε ότι η τρέχουσα ημερομηνία είναι εντός της διακριτικού διάρκειας ζωής, άλλο αυτό θα πρέπει να απορρίψετε το διακριτικό. Η υπηρεσία ίσως επιτρέψουν για έως και πέντε λεπτά πέρα από την περιοχή διακριτικού διάρκεια ζωής με λογαριασμού για όλες τις διαφορές ρολόι στιγμή ("χρονικής απόκλισης") μεταξύ Azure AD και της υπηρεσίας. <br><br> **Παράδειγμα SAML τιμή**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Παράδειγμα JWT τιμή**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| Κύριο όνομα χρήστη | Αποθηκεύει το όνομα χρήστη του αρχικού χρήστη.<br><br> **Παράδειγμα JWT τιμή**: <br> `"upn": frankm@contoso.com`|
| `ver`| Έκδοση | Αποθηκεύει τον αριθμό έκδοσης του διακριτικού. <br><br> **Παράδειγμα JWT τιμή**: <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>Διακριτικά πρόσβασης

Διακριτικά πρόσβασης είναι μόνο ΑΝΑΛΩΣΙΜΩΝ από τις υπηρεσίες Microsoft σε αυτό το σημείο στο φορά.  Οι εφαρμογές σας δεν θα πρέπει να χρειάζεται να εκτελέσετε οποιαδήποτε επικύρωση ή έλεγχο των κωδικών πρόσβασης για οποιαδήποτε από τα σενάρια που υποστηρίζονται αυτήν τη στιγμή.  Να διαχειρίζεστε διακριτικά πρόσβασης ως αδιαφανές - είναι απλώς συμβολοσειρές που μπορούν να μεταβιβάζουν την εφαρμογή σας στη Microsoft στο αιτήσεις HTTP.

Όταν ζητήσετε ένα διακριτικό πρόσβασης, Azure AD επιστρέφει επίσης ορισμένα μετα-δεδομένα σχετικά με το διακριτικό πρόσβασης για την κατανάλωση της εφαρμογής σας.  Αυτές οι πληροφορίες περιλαμβάνουν το χρόνο λήξης της το διακριτικό πρόσβασης και το εύρος για την οποία είναι έγκυρη.  Αυτό σας επιτρέπει την εφαρμογή σας για να εκτελέσετε έξυπνη προσωρινή αποθήκευση διακριτικά πρόσβασης χωρίς να χρειάζεται να ανάλυση ανοιχτό το διακριτικό πρόσβασης ίδια.

## <a name="refresh-tokens"></a>Ανανεώστε τα διακριτικά

Ανανεώστε τα διακριτικά είναι διακριτικά ασφαλείας, που να χρησιμοποιήσετε την εφαρμογή σας για να αποκτήσετε νέα διακριτικά πρόσβασης σε μια ροή διακριτικό 2.0.  Σας επιτρέπει την εφαρμογή σας για να επιτύχετε μακροπρόθεσμες πρόσβασης σε πόρους εκ μέρους ενός χρήστη χωρίς να απαιτείται αλληλεπίδραση από το χρήστη.

Ανανέωση διακριτικά είναι πολλών πόρων, γεγονός που σημαίνει αυτά ενδέχεται να είναι λαμβάνεται κατά τη διάρκεια αίτησης διακριτικού για έναν πόρο, αλλά εξαργυρώσατε για διακριτικά πρόσβασης σε μια εντελώς διαφορετική πόρο. Για να καθορίσετε πολλών πόρων, ορίστε το `resource` παραμέτρου στην αίτηση στον πόρο προορισμού.

Ανανέωση διακριτικά είναι αδιαφανές για την εφαρμογή σας. Είναι μεγάλης διάρκειας, αλλά δεν πρέπει να γράφονται την εφαρμογή σας να περιμένει ότι ένα διακριτικό ανανέωση θα διαρκέσει οποιαδήποτε χρονική περίοδο.  Ανανέωση διακριτικά μπορεί να ακυρωθεί σε οποιαδήποτε χρονική στιγμή για διάφορους λόγους.  Ο μόνος τρόπος για να γνωρίζω αν ενός διακριτικού ανανέωση είναι έγκυρη εφαρμογή σας είναι να δοκιμάσετε να την εξαργύρωση, καθιστώντας αίτησης διακριτικού Azure AD διακριτικού τελικό σημείο.

Κατά την εξαργύρωση ενός διακριτικού ανανέωσης για έναν νέο κωδικό πρόσβασης, θα λάβετε έναν νέο κωδικό ανανέωση στην διακριτικού απάντηση.  Θα πρέπει να αποθηκεύσετε το διακριτικό που μόλις έχει εκδοθεί ανανέωσης, αντικαθιστώντας αυτό που χρησιμοποιείται στην αίτηση.  Αυτό θα εγγυάται ότι σας διακριτικά ανανέωση εξακολουθεί να ισχύει για το χρονικό διάστημα όσο το δυνατόν.

## <a name="validating-tokens"></a>Επικύρωση διακριτικά

Σε αυτό το σημείο στο φορά, την επικύρωση μόνο διακριτικού την εφαρμογή υπολογιστή-πελάτη σας θα πρέπει να πρέπει να εκτελέσετε την επικύρωση id_tokens.  Για να επικυρώσετε ένα id_token, την εφαρμογή σας θα πρέπει να επικύρωση υπογραφής του id_token και το αξιώσεων στο το id_token.

Παρέχουμε βιβλιοθήκες και δείγματα κώδικα που δείχνουν τον τρόπο χειρισμού εύκολα διακριτικού επικύρωσης, εάν θέλετε να κατανοήσετε το υποκείμενο διαδικασία.  Υπάρχουν επίσης πολλές βιβλιοθήκες Άνοιγμα αρχείου προέλευσης τρίτων κατασκευαστών διαθέσιμη για επικύρωση JWT, τουλάχιστον μία επιλογή για σχεδόν κάθε πλατφόρμα και τη γλώσσα. Για περισσότερες πληροφορίες σχετικά με τις βιβλιοθήκες Azure AD ελέγχου ταυτότητας και δείγματα κώδικα, ανατρέξτε στο θέμα [Azure AD βιβλιοθήκες ελέγχου ταυτότητας](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>Επικύρωση της υπογραφής

Μια JWT περιέχει τρία τμήματα, που διαχωρίζονται με το `.` χαρακτήρα.  Το πρώτο τμήμα είναι γνωστό ως **κεφαλίδα**, το δεύτερο ως **σώμα**και η τρίτη με την **υπογραφή**.  Στο τμήμα υπογραφής μπορεί να χρησιμοποιηθεί για την επικύρωση της αυθεντικότητας του id_token, έτσι ώστε να είναι αξιόπιστο από την εφαρμογή σας.

Id_Tokens έχετε εισέλθει με αλγόριθμους βασική ασύμμετρη κρυπτογράφηση κλάδο, όπως RSA 256. Στην κεφαλίδα της το id_token περιέχει πληροφορίες σχετικά με τη μέθοδο κλειδί και κρυπτογράφησης που χρησιμοποιείται για την είσοδο το διακριτικό:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

Το `alg` διεκδίκηση υποδεικνύει τον αλγόριθμο που χρησιμοποιήθηκε για να πραγματοποιήσετε το διακριτικό, χωρίς την `x5t` διεκδίκηση υποδεικνύει το συγκεκριμένο δημόσιο κλειδί που χρησιμοποιήθηκε για να πραγματοποιήσετε το διακριτικό.

Σε οποιαδήποτε δεδομένη χρονική στιγμή, Azure AD μπορεί να συνδεθείτε μια id_token χρησιμοποιώντας οποιαδήποτε από ένα ορισμένο σύνολο ζεύγη δημόσιων-ιδιωτικών κλειδιών. Azure AD Περιστρέφει το δυνατό σύνολο αριθμών-κλειδιών σε περιοδική βάση, ώστε να την εφαρμογή σας πρέπει να γράφονται χειρισμού αυτόματα αυτές τις βασικές αλλαγές.  Ένα λογικό συχνότητα για να ελέγξετε για ενημερώσεις για τη δημόσια κλειδιά που χρησιμοποιούνται από το Azure AD είναι κάθε 24 ώρες.

Μπορείτε να αποκτήσετε το υπογραφής κλειδιού δεδομένων που είναι απαραίτητα για την επικύρωση της υπογραφής με χρήση του εγγράφου μετα-δεδομένων OpenID σύνδεση βρίσκεται στο:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] Δοκιμάστε αυτήν τη διεύθυνση URL σε ένα πρόγραμμα περιήγησης!

Αυτό το έγγραφο μετα-δεδομένων είναι ένα αντικείμενο JSON που περιέχει πολλές χρήσιμες τμήματα πληροφοριών, όπως η θέση της τα διάφορα τελικά σημεία που απαιτείται για την εκτέλεση του ελέγχου ταυτότητας που OpenID σύνδεση.  

Περιλαμβάνει επίσης μια `jwks_uri`, που παρέχει τη θέση του συνόλου των δημόσιων κλειδιών που χρησιμοποιείται για την είσοδο διακριτικά.  Το έγγραφο JSON βρίσκεται στο το `jwks_uri` περιέχει όλες τις πληροφορίες του κλειδιού δημόσια χρησιμοποιείται σε συγκεκριμένη συγκεκριμένη χρονική στιγμή.  Να χρησιμοποιήσετε την εφαρμογή σας το `kid` διεκδίκηση στην κεφαλίδα JWT για να επιλέξετε ποια δημόσιο κλειδί σε αυτό το έγγραφο έχει χρησιμοποιηθεί για την υπογραφή ενός συγκεκριμένου διακριτικού.  Στη συνέχεια, να μπορεί να εκτελέσει χρησιμοποιώντας το σωστό δημόσιο κλειδί και αλγόριθμο καθορισμένου επικύρωση της υπογραφής.

Εκτέλεση επικύρωση της υπογραφής βρίσκεται εκτός του αντικειμένου αυτού του εγγράφου - υπάρχουν πολλές βιβλιοθήκες Άνοιγμα αρχείου προέλευσης για βοηθώντας σας να το κάνετε εάν είναι απαραίτητο.

#### <a name="validating-the-claims"></a>Επικύρωση των απαιτήσεων

Κατά την εφαρμογή σας λαμβάνει μια id_token κατά την είσοδο στο χρήστη, αυτό πρέπει να λειτουργεί επίσης μερικά ελέγχων προς τις αντίστοιχες απαιτήσεις στα το id_token.  Αυτά περιλαμβάνουν, αλλά δεν είστε υποχρεωμένοι να:

  - Το αίτημα **ακροατηρίου** - για να επιβεβαιώσετε ότι το id_token που περιμένατε να δοθεί για την εφαρμογή σας.
  - Το **Όχι πριν από** και την **Ώρα λήξης** αξιώσεων - για να επιβεβαιώσετε ότι το id_token δεν έχει λήξει.
  - Το αίτημα **εκδότη** - για να επιβεβαιώσετε ότι το διακριτικό στην πραγματικότητα έχει εκδοθεί για την εφαρμογή σας από Azure AD.
  - Το **Nonce** - να συμβάλει στην αντιμετώπιση επίθεση διακριτικού αναπαραγωγής.
  - και ακόμη περισσότερα...

Για μια πλήρη λίστα των επικυρώσεων διεκδίκηση θα πρέπει να εκτελέσετε την εφαρμογή σας, ανατρέξτε στις [προδιαγραφές OpenID σύνδεση](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Λεπτομέρειες τις αναμενόμενες τιμές για αυτές τις απαιτήσεις περιλαμβάνονται στην προηγούμενη ενότητα [id_token ενότητας](#id-tokens) .

## <a name="sample-tokens"></a>Δείγμα διακριτικά

Αυτή η ενότητα εμφανίζει δείγματα SAML και JWT διακριτικά που επιστρέφει Azure AD. Αυτά τα δείγματα σάς επιτρέπουν να δείτε το αξιώσεων στο περιβάλλον.
SAML διακριτικού

Αυτό είναι ένα δείγμα από ένα τυπικό διακριτικό SAML.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>Το διακριτικό JWT - απομίμησης χρήστη

Αυτό είναι ένα δείγμα από ένα τυπικό JSON web διακριτικό (JWT) χρησιμοποιείται σε μια ροή εκχώρηση κώδικα εξουσιοδότησης.
Εκτός από τις απαιτήσεις, το διακριτικό περιλαμβάνει αριθμό έκδοσης στην **έκδοση** και **appidacr**, η αναφορά κλάσης περιβάλλον ελέγχου ταυτότητας, η οποία υποδεικνύει πώς έχει γίνει έλεγχος ταυτότητας του προγράμματος-πελάτη. Για έναν δημόσιο υπολογιστή-πελάτη, η τιμή είναι 0. Εάν ένα Αναγνωριστικό υπολογιστή-πελάτη ή μυστικό προγράμματος-πελάτη που χρησιμοποιήθηκε, η τιμή είναι 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Σχετικό περιεχόμενο
- Ανατρέξτε στο θέμα το Azure AD Graph [λειτουργίες πολιτικής](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) και της [πολιτικής οντότητα](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity), για να μάθετε περισσότερα σχετικά με τη διαχείριση της πολιτικής διακριτικού διάρκεια ζωής μέσω του API Azure AD Graph.
- Για περισσότερες πληροφορίες και παραδείγματα σχετικά με τη διαχείριση των πολιτικών μέσω των cmdlet του PowerShell, συμπεριλαμβανομένων των δειγμάτων, ανατρέξτε στο θέμα [με δυνατότητα ρύθμισης παραμέτρων διακριτικού διάρκεια ζωής στο Azure AD](active-directory-configurable-token-lifetimes.md). 
