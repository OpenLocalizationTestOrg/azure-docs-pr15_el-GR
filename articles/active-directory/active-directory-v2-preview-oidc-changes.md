<properties
    pageTitle="Αλλάζει σε το τελικό σημείο v2.0 Azure AD | Microsoft Azure"
    description="Μια περιγραφή των αλλαγών που πραγματοποιούνται σε εφαρμογή μοντέλο v2.0 δημόσια προεπισκόπηση πρωτόκολλα."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="important-updates-to-the-v20-authentication-protocols"></a>Σημαντικές ενημερώσεις για τα πρωτόκολλα ελέγχου ταυτότητας v2.0
Προγραμματιστές προσοχή! Επάνω από τις επόμενες δύο εβδομάδες, θα σας θα κάνει μερικά ενημερωμένες εκδόσεις για να μας πρωτόκολλα ελέγχου ταυτότητας v2.0 που μπορεί να σημαίνει διακοπή αλλαγές για κάποια από τις εφαρμογές που έχετε γράψει κατά τη διάρκεια μας περιόδου preview.  

## <a name="who-does-this-affect"></a>Ποιος επηρεάζει;
Καμία από τις εφαρμογές που έχουν δημιουργηθεί για να χρησιμοποιήσετε το v2.0 συγκλίνει τελικό σημείο ελέγχου ταυτότητας,

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με το τελικό σημείο v2.0 [εδώ](active-directory-appmodel-v2-overview.md).

Αν έχετε δημιουργήσει μια εφαρμογή χρησιμοποιώντας το τελικό σημείο v2.0 με κωδικοποίηση απευθείας με το πρωτόκολλο v2.0, χρησιμοποιώντας οποιαδήποτε από μας middlewares web σύνδεση OpenID ή OAuth ή τη χρήση άλλων βιβλιοθηκών τρίτου κατασκευαστή για να εκτελέσετε τον έλεγχο ταυτότητας, που θα πρέπει να προετοιμαστείτε για να ελέγξετε τα έργα σας και να κάνετε αλλαγές, εάν είναι απαραίτητο.

## <a name="who-doesnt-this-affect"></a>Άτομα που δεν αυτό επηρεάζει;
Καμία από τις εφαρμογές που έχουν δημιουργηθεί σε σχέση με το τελικό σημείο ελέγχου ταυτότητας παραγωγής Azure AD,

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Αυτό το πρωτόκολλο έχει οριστεί σε λίθο και δεν θα να αντιμετωπίζει τις αλλαγές.

Επιπλέον, εάν σας εφαρμογή **μόνο** χρησιμοποιεί μας ADAL βιβλιοθήκη για την πραγματοποίηση ελέγχου ταυτότητας, δεν θα πρέπει να αλλάξετε τίποτα.  ADAL έχει προστατεύεται την εφαρμογή σας από τις αλλαγές.  

## <a name="what-are-the-changes"></a>Ποιες είναι οι αλλαγές;
### <a name="removing-the-x5t-value-from-jwt-headers"></a>Κατάργηση της τιμής x5t από κεφαλίδες JWT
Το τελικό σημείο v2.0 χρησιμοποιεί τα διακριτικά JWT σε μεγάλο βαθμό, που περιέχουν μια ενότητα παράμετροι κεφαλίδας με σχετικές μετα-δεδομένα σχετικά με το διακριτικό.  Εάν αποκωδικοποιείτε την κεφαλίδα ενός μας τρέχουσα JWTs, μπορείτε να βρείτε κάτι τέτοιο:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Όπου το "x5t" και "παιδί" Ιδιότητες προσδιορίστε το δημόσιο κλειδί που πρέπει να χρησιμοποιούνται για την επικύρωση το διακριτικό υπογραφής, όπως η ανάκτηση από το τελικό σημείο σύνδεσης OpenID μετα-δεδομένων.

Η αλλαγή πραγματοποιούμε εδώ είναι να καταργήσετε την ιδιότητα "x5t".  Ενδέχεται να συνεχίσετε να χρησιμοποιείτε το ίδιο μηχανισμούς για την επικύρωση διακριτικά, αλλά πρέπει να βασίζεστε μόνο για την ιδιότητα "παιδιού" για να ανακτήσετε τη σωστή δημόσιο κλειδί, όπως καθορίζονται στο πρωτόκολλο OpenID σύνδεση. 

> [AZURE.IMPORTANT] **Εργαστείτε: Βεβαιωθείτε ότι η εφαρμογή σας δεν εξαρτάται από την ύπαρξη της τιμής x5t.**

### <a name="removing-profileinfo"></a>Κατάργηση profile_info
Στο παρελθόν, το τελικό σημείο v2.0 έχει έχουν επιστρέφει ένα αντικείμενο JSON κωδικοποίηση base64 σε διακριτικού απαντήσεων που ονομάζεται `profile_info`.  Κατά την αίτηση ένα διακριτικό πρόσβασης από το τελικό σημείο v2.0 στέλνοντας μια αίτηση για:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Απάντηση θα έχει ως εξής JSON αντικειμένου:
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Το `profile_info` τιμή που περιέχονται πληροφορίες σχετικά με το χρήστη που έχετε συνδεθεί στην εφαρμογή - τους εμφανιζόμενο όνομα, όνομα, επώνυμο, διεύθυνση ηλεκτρονικού ταχυδρομείου, αναγνωριστικό και ούτω καθεξής.  Κυρίως, το `profile_info` που χρησιμοποιήθηκε για προσωρινή αποθήκευση διακριτικού και να εμφανίσετε σκοπούς.

Θα σας τώρα η κατάργηση του `profile_info` τιμή –, αλλά μην ανησυχείτε, παρέχουμε αυτές τις πληροφορίες ακόμη για τους προγραμματιστές στο ελαφρώς διαφορετική θέση.  Αντί για `profile_info`, το τελικό σημείο v2.0 τώρα θα επιστρέψει μια `id_token` σε κάθε διακριτικού απόκριση:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Ενδέχεται να μπορείτε να αποκωδικοποιείτε και να ανάλυση του id_token για να ανακτήσετε τις ίδιες πληροφορίες που έχετε λάβει από profile_info.  Η id_token είναι μια JSON Web διακριτικού (JWT), με περιεχόμενο όπως καθορίζεται από το OpenID σύνδεση.  Ο κωδικός για αυτές τις ενέργειες επομένως, θα πρέπει να είναι παρόμοια-απλώς πρέπει να εξαχθούν στο μεσαίο τμήμα (σώμα) το id_token και base64 αποκωδικοποιείτε την πρόσβαση του αντικειμένου JSON μέσα σε.

Επάνω από τις επόμενες δύο εβδομάδες, θα πρέπει να κώδικα την εφαρμογή σας να ανακτήσει τις πληροφορίες χρήστη είτε από το `id_token` ή `profile_info`; υπάρχει όποιο από τα δύο.  Με αυτόν τον τρόπο όταν πραγματοποιείται η αλλαγή της εφαρμογής σας μπορεί να διαχειριστεί απρόσκοπτα τη μετάβαση από το `profile_info` να `id_token` χωρίς διακοπή.

> [AZURE.IMPORTANT] **Εργαστείτε: Βεβαιωθείτε ότι η εφαρμογή σας δεν εξαρτάται από την ύπαρξη του `profile_info` τιμή.**

### <a name="removing-idtokenexpiresin"></a>Κατάργηση id_token_expires_in
Παρόμοια με `profile_info`, θα σας επίσης η κατάργηση του `id_token_expires_in` παράμετρο από τις απαντήσεις.  Στο παρελθόν, θα πρέπει να επιστρέψει μια τιμή για το τελικό σημείο v2.0 `id_token_expires_in` μαζί με κάθε απόκριση id_token, για παράδειγμα σε μια απόκριση εξουσιοδότηση:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Ή σε μια απάντηση διακριτικού:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Το `id_token_expires_in` τιμή θα δηλώνει τον αριθμό των δευτερολέπτων που θα εξακολουθούν να ισχύουν για το id_token.  Τώρα, θα σας πρόκειται να καταργήσετε το `id_token_expires_in` τιμή πλήρως.  Αντί για αυτό, μπορείτε να χρησιμοποιήσετε την τυπική σύνδεση OpenID `nbf` και `exp` αξιώσεις για να εξετάσετε την εγκυρότητα της μια id_token.  Δείτε την [αναφορά διακριτικού v2.0](active-directory-v2-tokens.md) για περισσότερες πληροφορίες σχετικά με αυτές τις απαιτήσεις.

> [AZURE.IMPORTANT] **Εργαστείτε: Βεβαιωθείτε ότι η εφαρμογή σας δεν εξαρτάται από την ύπαρξη του `id_token_expires_in` τιμή.**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>Αλλαγή του αξιώσεων που επιστρέφονται από εύρος = openid
Αυτή η αλλαγή θα είναι η πιο σημαντική – στην πραγματικότητα, θα επηρεάσει σχεδόν κάθε εφαρμογή που χρησιμοποιεί το τελικό σημείο v2.0.  Πολλές εφαρμογές αποστολή προσκλήσεων στο τελικό σημείο v2.0 χρησιμοποιώντας το `openid` εμβέλεια, όπως:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Σήμερα, όταν ο χρήστης εκχωρεί συγκατάθεση για το `openid` εύρος, την εφαρμογή σας λαμβάνει πληθώρα πληροφοριών σχετικά με το χρήστη στο το id_token που προκύπτει.  Αυτές οι αξιώσεις μπορούν να περιλαμβάνουν το όνομα του ατόμου, προτιμώμενη όνομα χρήστη, τη διεύθυνση ηλεκτρονικού ταχυδρομείου, Αναγνωριστικό αντικειμένου και πολλά άλλα.

Σε αυτήν την ενημέρωση, θα σας πρόκειται να αλλάξετε τις πληροφορίες που το `openid` εύρος δίνει την πρόσβασή σας εφαρμογή σε, για καλύτερη comform με την προδιαγραφή OpenID σύνδεση.  Το `openid` εύρος θα μόνο επιτρέπουν την εφαρμογή σας για να συνδεθείτε στο χρήστη στο και λαμβάνετε ένα αναγνωριστικό εφαρμογής ειδικά για το χρήστη στο το `sub` διεκδίκηση από το id_token.  Το αξιώσεων σε μια id_token με μόνο το `openid` εύρος εκχωρηθεί θα devoid of προσωπικές πληροφορίες.  Παράδειγμα id_token αξιώσεων είναι οι εξής:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Εάν θέλετε να αποκτήσετε αναγνωρίσιμα προσωπικά στοιχεία (PII) σχετικά με το χρήστη στην εφαρμογή, την εφαρμογή σας θα πρέπει να ζητήσετε πρόσθετα δικαιώματα από το χρήστη.  Θα σας Εισαγωγή υποστήριξης για τα δύο νέα πεδία από την προδιαγραφή OpenID σύνδεση – το `email` και `profile` εύρους – που σας επιτρέπουν να το κάνετε.

- Το `email` εμβέλειας είναι πολύ απλές-να επιτρέπει την πρόσβαση εφαρμογής του χρήστη κύρια διεύθυνση ηλεκτρονικού ταχυδρομείου μέσω του `email` διεκδίκηση στο το id_token.  Λάβετε υπόψη ότι η `email` διεκδίκηση δεν θα είναι πάντα παρόντες σε id_tokens – μόνο θα συμπεριληφθούν εάν είναι διαθέσιμη στο προφίλ χρήστη.
- Το `profile` εύρος δίνει την πρόσβασή σας εφαρμογή σε όλες τις άλλες βασικές πληροφορίες σχετικά με το χρήστη – τους όνομα, το όνομα χρήστη προτιμώμενη, Αναγνωριστικό αντικειμένου και ούτω καθεξής.

Αυτό σας επιτρέπει να κώδικα την εφαρμογή σας με τον τρόπο ελάχιστους αποκάλυψης – μπορείτε να ζητήσετε από το χρήστη για απλώς το σύνολο των πληροφοριών που απαιτεί την εφαρμογή σας για να κάνετε την εργασία.  Εάν θέλετε να συνεχίσετε γρήγορα το πλήρες σύνολο των πληροφοριών χρήστη που λαμβάνει προς το παρόν την εφαρμογή σας, πρέπει να συμπεριλάβετε όλα τα τρία πεδία στις προσκλήσεις εξουσιοδότησης σε:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Εφαρμογή σας μπορεί να ξεκινήσει η αποστολή του `email` και `profile` εύρους αμέσως και το τελικό σημείο v2.0 θα αποδεχτείτε αυτές τις δύο εύρους και ξεκινήστε ζητά δικαιώματα από τους χρήστες ανάλογα με τις απαιτήσεις.  Ωστόσο, η αλλαγή στην ερμηνεία των το `openid` εμβέλεια δεν θα ισχύσουν για μερικές εβδομάδες.

> [AZURE.IMPORTANT] **Εργαστείτε: Προσθήκη του `profile` και `email` εμβελειών εάν η εφαρμογή σας απαιτεί πληροφορίες σχετικά με το χρήστη.**  Σημειώστε ότι ADAL θα περιλαμβάνουν και τα δύο από αυτά τα δικαιώματα σε αιτήσεις από προεπιλογή. 

### <a name="removing-the-issuer-trailing-slash"></a>Κατάργηση τον εκδότη τελική κάθετος.
Στο παρελθόν, η τιμή εκδότη που εμφανίζεται σε διακριτικά από το τελικό σημείο v2.0 εκτελέσατε τη φόρμα

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Όπου το guid ήταν το tenantId του μισθωτή του Azure AD που εξέδωσε το διακριτικό.  Με αυτές τις αλλαγές, γίνεται η τιμή εκδότη

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

στο τόσο τα διακριτικά και στο έγγραφο εντοπισμού OpenID σύνδεση.

> [AZURE.IMPORTANT] **Εργαστείτε: Βεβαιωθείτε ότι η εφαρμογή σας αποδέχεται την τιμή εκδότη τόσο με και χωρίς μια κάθετο κατά την επικύρωση εκδότη.**

## <a name="why-change"></a>Γιατί να αλλάξω;
Το κύριο κίνητρο για εισαγωγή αυτές οι αλλαγές θα πρέπει να συμβατή με την τυπική προδιαγραφή OpenID σύνδεση.  Κατά τη σύνδεση συμβατή OpenID, ελπίζετε για να ελαχιστοποιήσετε τις διαφορές μεταξύ της ενοποίηση με τις υπηρεσίες Microsoft ταυτότητας και με άλλες υπηρεσίες ταυτότητας του κλάδου.  Θέλουμε να επιτρέπει στους προγραμματιστές να χρησιμοποιείτε τους βιβλιοθήκες ελέγχου ταυτότητας αγαπημένη προέλευση άνοιγμα χωρίς να χρειάζεται να τροποποιήσουν τις βιβλιοθήκες για να αποθηκεύσουν τις διαφορές της Microsoft.

## <a name="what-can-you-do"></a>Τι μπορείτε να κάνετε;
Σήμερα, μπορείτε να αρχίσετε να πραγματοποιήσετε όλες τις αλλαγές που περιγράφονται παραπάνω.  Θα πρέπει να αμέσως:

1.  **Καταργήστε τυχόν εξαρτήσεις σε το `x5t` κεφαλίδα παραμέτρου.**
2.  **Χειρίζονται ομαλά τη μετάβαση από το `profile_info` να `id_token` σε διακριτικού απαντήσεις.**
3.  **Καταργήστε τυχόν εξαρτήσεις σε το `id_token_expires_in` παραμέτρου απόκρισης.**
3.  **Προσθέστε το `profile` και `email` εμβελειών για την εφαρμογή σας, εάν η εφαρμογή σας χρειάζεται πληροφορίες βασικού χρήστη.**
4.  **Αποδοχή εκδότη τιμών σε διακριτικά τόσο με και χωρίς μια κάθετο.**

Την [τεκμηρίωση πρωτόκολλο v2.0](active-directory-v2-protocols.md) έχει ήδη ενημερωθεί με αυτές τις αλλαγές, ώστε να μπορείτε να τον χρησιμοποιήσετε ως αναφορά στη Βοήθεια για την ενημέρωση του κώδικα.

Εάν έχετε περισσότερες ερωτήσεις σχετικά με το πεδίο των αλλαγών, επικοινωνήστε όποιους για επικοινωνία μας στο Twitter στο @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Πόσο συχνά θα γίνονται αλλαγές πρωτόκολλο;
Θα σας δεν αναμένεται οποιαδήποτε περαιτέρω Διακοπή αλλάζει για τα πρωτόκολλα ελέγχου ταυτότητας.  Θα σας δέσμης αυτών των αλλαγών στην τελική έκδοση ενός σκόπιμα ώστε δεν θα πρέπει να μεταβείτε σε αυτόν τον τύπο της διαδικασίας ενημέρωσης ξανά οποιαδήποτε στιγμή σύντομα.  Φυσικά, θα συνεχίσουμε να προσθέσετε δυνατότητες για την υπηρεσία ελέγχου ταυτότητας συγκλίνει v2.0 που μπορείτε να εκμεταλλευτείτε, αλλά πρέπει να είναι αυτές οι αλλαγές προσθετικά και όχι αλλαγή υπαρχόντων κώδικα.

Τέλος, θα σας θα θέλατε να πείτε ευχαριστούμε για δοκιμάζοντας στοιχεία κατά την περίοδο preview.  Η ιδέες και εμπειρίες του μας πρώτοι έχουν πολύτιμη μέχρι τώρα και θα σας ελπίζετε θα συνεχίσετε να κάνετε κοινή χρήση τις απόψεις και τις ιδέες.

Κωδικοποίηση ικανοποιημένοι!

Η διαίρεση ταυτότητα της Microsoft