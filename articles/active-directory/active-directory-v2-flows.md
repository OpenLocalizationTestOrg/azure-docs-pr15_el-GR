<properties
    pageTitle="Τύποι του τελικού σημείου v2.0 | Microsoft Azure"
    description="Οι τύποι των εφαρμογών και σενάρια που υποστηρίζονται από το τελικό σημείο v2.0 Azure AD."
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
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="types-of-apps-for-the-v20-endpoint"></a>Τύπους εφαρμογών για το τελικό σημείο v2.0
Το τελικό σημείο v2.0 υποστηρίζει έλεγχο ταυτότητας για μια ποικιλία αρχιτεκτονικές σύγχρονο εφαρμογής, τα οποία βασίζονται σε τα τυπικά πρωτόκολλα κλάδο [διακριτικό 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) ή/και τη [Σύνδεση OpenID](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  Σε αυτό το έγγραφο περιγράφει συνοπτικά τους τύπους των εφαρμογών που μπορείτε να δημιουργήσετε, ανεξάρτητα από τη γλώσσα ή πλατφόρμα που προτιμάτε.  Θα σας βοηθήσει να κατανοήσετε τα σενάρια υψηλού επιπέδου πριν να [ξεκινήσετε κατευθείαν στον κώδικα](active-directory-appmodel-v2-overview.md#getting-started).

> [AZURE.NOTE]
    Δεν όλα τα σενάρια Azure Active Directory και δυνατότητες που υποστηρίζονται από το τελικό σημείο v2.0.  Για να καθορίσετε εάν θα πρέπει να χρησιμοποιήσετε το τελικό σημείο v2.0, διαβάστε σχετικά με [τους περιορισμούς v2.0](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Τα βασικά στοιχεία
Κάθε εφαρμογή που χρησιμοποιεί το τελικό σημείο v2.0 θα πρέπει να είναι καταχωρημένος στο [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Η διαδικασία καταχώρησης εφαρμογής θα συλλογής & αντιστοίχιση μερικές τιμές σε εφαρμογή σας:

- **Αναγνωριστικό εφαρμογής** που προσδιορίζει με μοναδικό τρόπο την εφαρμογή σας
- **Ανακατεύθυνση URI** που μπορούν να χρησιμοποιηθούν για να κατευθύνετε απαντήσεων Επιστροφή στην εφαρμογή σας
- Μερικές άλλες τιμές αφορούν συγκεκριμένα το σενάριο.  Για περισσότερες λεπτομέρειες, μάθετε πώς μπορείτε να [καταχωρήσετε μια εφαρμογή](active-directory-v2-app-registration.md).

Αφού καταχωρηθεί, η εφαρμογή επικοινωνεί με Azure AD με την αποστολή προσκλήσεων για το τελικό σημείο v2.0 Azure Active Directory.  Παρέχουμε πλαίσια Άνοιγμα αρχείου προέλευσης & βιβλιοθήκες που να χειριστείτε τις λεπτομέρειες αυτών των αιτήσεων ή, μπορείτε να εφαρμόσετε τη λογική του ελέγχου ταυτότητας στον εαυτό σας δημιουργώντας αιτήσεων σε αυτά τα τελικά σημεία:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Εφαρμογές Web
Εφαρμογές web (.NET PHP, Java, φωνητικής γραφής, Python, κόμβου, κλπ) που είναι προσβάσιμες μέσω ενός προγράμματος περιήγησης, μπορείτε να εκτελέσετε χρήστη είσοδος με χρήση της [Σύνδεσης OpenID](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  Στη σύνδεση OpenID λαμβάνει την εφαρμογή web μια `id_token`, έναν κωδικό ασφαλείας που επαληθεύει την ταυτότητα του χρήστη και παρέχει πληροφορίες σχετικά με το χρήστη με τη μορφή αξιώσεων:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Μπορείτε να μάθετε σχετικά με όλους τους τύπους των διακριτικά και αξιώσεων διαθέσιμη σε μια εφαρμογή στην [αναφορά διακριτικού v2.0](active-directory-v2-tokens.md).

Στις εφαρμογές διακομιστή web, τη ροή εισόδου ελέγχου ταυτότητας λαμβάνει αυτά τα βήματα υψηλού επιπέδου:

![Εικόνα κλειστές λωρίδες εφαρμογής Web](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Η επικύρωση της το id_token χρησιμοποιώντας ένα δημόσιο κλειδί υπογραφής που λάβατε από το τελικό σημείο v2.0 αρκεί να διασφαλίσετε την ταυτότητα του χρήστη και να ορίσετε μια περίοδο λειτουργίας cookie που μπορεί να χρησιμοποιηθεί για τον προσδιορισμό του χρήστη στην επόμενη σελίδα αιτήσεις.

Για να δείτε αυτό το σενάριο στην πράξη, δοκιμάστε ένα από τα δείγματα εισόδου κώδικα εφαρμογής web μας ενότητας [Γρήγορα αποτελέσματα](active-directory-appmodel-v2-overview.md#getting-started) .

Εκτός από απλό εισόδου, μια εφαρμογή διακομιστή web ίσως χρειαστεί να αποκτήσετε πρόσβαση σε κάποια άλλη υπηρεσία web όπως μια REST API.  Σε αυτήν την περίπτωση το διακομιστή web app μπορούν να περιορίζονται σε ροή συνδυασμένη σύνδεση OpenID & OAuth 2.0, χρησιμοποιώντας το [διακριτικό κωδικό εξουσιοδότησης 2.0 ροής](active-directory-v2-protocols.md#oauth2-authorization-code-flow). Αυτό το σενάριο καλύπτεται κάτω από το στοιχείο σε μας [θέμα WebApp WebAPI γρήγορα αποτελέσματα](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Web APIs
Μπορείτε να χρησιμοποιήσετε το τελικό σημείο v2.0 για την ασφάλιση καθώς και τις υπηρεσίες web όπως RESTful API Web της εφαρμογής σας.  Αντί για id_tokens και την περίοδο λειτουργίας cookies, Web APIs Χρησιμοποιήστε access_tokens διακριτικό 2.0 για την ασφάλεια των δεδομένων και τον έλεγχο ταυτότητας εισερχόμενες αιτήσεις.  Ο καλών από ένα API Web τοποθετεί μια access_token στην κεφαλίδα της εξουσιοδότησης από μια αίτηση HTTP:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Το API Web, στη συνέχεια, να χρησιμοποιήσετε το access_token για να επαληθεύσετε την API ταυτότητα του καλούντα και εξαγάγετε πληροφορίες σχετικά με τον καλούντα από αξιώσεων που είναι κωδικοποιημένα το access_token.  Μπορείτε να μάθετε σχετικά με όλους τους τύπους των διακριτικά και αξιώσεων διαθέσιμη σε μια εφαρμογή στην [αναφορά διακριτικού v2.0](active-directory-v2-tokens.md).

Ένα API Web να δώσετε στους χρήστες για να επιλέξετε-στο/εξαίρεση ορισμένων λειτουργίες ή δεδομένων, εκθέσετε δικαιώματα, αλλιώς γνωστές ως [εύρους](active-directory-v2-scopes.md).  Για μια εφαρμογή κλήσης για να αποκτήσετε δικαιώματα για ένα εύρος, ο χρήστης πρέπει να τη συγκατάθεσή σας για το εύρος κατά τη διάρκεια μιας ροής.  Το τελικό σημείο v2.0 θα εκτελέσει το χρήστη που ζητά δικαιωμάτων και την καταγραφή αυτών των δικαιωμάτων σε όλα τα access_tokens που λαμβάνει το API Web.  Ολόκληρο το API Web χρειάζεται να ανησυχείτε για επικύρωση το access_tokens που λαμβάνει σε κάθε κλήση και εκτέλεση των ελέγχων κατάλληλη εξουσιοδότηση.

Ένα API Web μπορούν να λαμβάνουν access_tokens από όλους τους τύπους των εφαρμογών, όπως εφαρμογές διακομιστή web, επιφάνειας εργασίας και εφαρμογών για κινητές συσκευές, εφαρμογές μία μόνο σελίδα, στοιχεία daemon πλευρά του διακομιστή και ακόμη και άλλα APIs Web.  Η ροή υψηλού επιπέδου για έλεγχο ταυτότητας api web είναι ως εξής:

![Web API κλειστές λωρίδες εικόνας](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Για να μάθετε περισσότερα σχετικά με το authorization_codes, refresh_tokens και λεπτομερείς οδηγίες επιτύχετε access_tokens, διαβάστε σχετικά με το [πρωτόκολλο OAuth 2.0](active-directory-v2-protocols-oauth-code.md).

Για να μάθετε πώς να ασφαλίσετε μια web api με OAuth2 access_tokens, ανατρέξτε στο θέμα τα δείγματα κώδικα api web μας [ενότητα γρήγορα αποτελέσματα](active-directory-appmodel-v2-overview.md#getting-started).


## <a name="mobile-and-native-apps"></a>Mobile και του εγγενούς εφαρμογές
Εφαρμογές που έχουν εγκατασταθεί σε μια συσκευή, όπως φορητούς και επιτραπέζιους εφαρμογές, συχνά χρειάζεται για να αποκτήσετε πρόσβαση σε υπηρεσίες υποστήριξης ή API Web που αποθηκεύουν δεδομένα και να εκτελέσετε διάφορες λειτουργίες εκ μέρους ενός χρήστη.  Αυτές οι εφαρμογές να προσθέσετε εισόδου και εξουσιοδότηση υπηρεσίες παρασκηνίου χρησιμοποιώντας τη [ροή κωδικό εξουσιοδότησης 2.0 OAuth](active-directory-v2-protocols-oauth-code.md).  

Σε αυτήν τη ροή, μια την εφαρμογή λαμβάνει μια authorization_code από το τελικό σημείο v2.0 κατά είσοδος χρήστη, που αντιπροσωπεύει την εφαρμογή δικαιωμάτων για να καλέσετε τις υπηρεσίες υποστήριξης εκ μέρους του χρήστη αυτήν τη στιγμή συνδεδεμένοι στο.  Η εφαρμογή, στη συνέχεια, να ανταλλάξετε το authoriztion_code στο παρασκήνιο για ένα access_token OAuth 2.0 και ένα refresh_token.  Η εφαρμογή να χρησιμοποιήσετε το access_token για τον έλεγχο ταυτότητας προς API Web HTTP αιτήσεις και να χρησιμοποιήσετε το refresh_token για να λαμβάνετε νέο access_tokens όταν λήξει παλαιότερα.

![Εικόνα κλειστές λωρίδες εγγενούς εφαρμογής](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Μεμονωμένη σελίδα εφαρμογών (javascript)
Πολλά σύγχρονα εφαρμογές έχουν μια μεμονωμένη σελίδα εφαρμογής (SPA) προσκηνίου κυρίως χειρόγραφων javascript και χρησιμοποιείτε συχνά πλαίσια όπως AngularJS, Ember.js, Durandal, κ.λπ.  Το τελικό σημείο v2.0 Azure AD υποστηρίζει αυτές τις εφαρμογές χρησιμοποιώντας το [Ροής έμμεσα 2.0 OAuth](active-directory-v2-protocols-implicit.md).

Σε αυτήν τη ροή, η εφαρμογή λαμβάνει τα διακριτικά από το v2.0 εγκρίνετε τελικού σημείου απευθείας, χωρίς να εκτελέσει οποιαδήποτε ανταλλαγές διακομιστή προς διακομιστή παρασκηνίου.  Αυτό σας επιτρέπει όλων των λογικής ελέγχου ταυτότητας και χειρισμού περίοδο λειτουργίας για να τοποθετήσετε εντελώς στο πρόγραμμα-πελάτη javascript, χωρίς να εκτελέσει ανακατευθύνσεων επιπλέον σελίδας.

![Μη ρητή ροής κλειστές λωρίδες εικόνας](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Για να δείτε αυτό το σενάριο στην πράξη, δοκιμάστε ένα από τα δείγματα κώδικα εφαρμογής μεμονωμένη σελίδα μας ενότητας [Γρήγορα αποτελέσματα](active-directory-appmodel-v2-overview.md#getting-started) .

### <a name="daemonsserver-side-apps"></a>Τα στοιχεία daemon/διακομιστή πλευρά εφαρμογές
Εφαρμογές που περιέχουν μεγάλης διάρκειας διεργασίες ή που λειτουργούν χωρίς την παρουσία ενός χρήστη πρέπει επίσης ένας τρόπος για να αποκτήσετε πρόσβαση σε ασφαλή πόρους, όπως Web APIs.  Αυτές οι εφαρμογές μπορούν να τον έλεγχο ταυτότητας και να λάβετε κώδικες, χρησιμοποιώντας την ταυτότητα της εφαρμογής (αντί για ανάθεση ταυτότητα χρήστη) χρησιμοποιώντας το πρόγραμμα-πελάτη διακριτικό 2.0 ροής διαπιστευτήρια.

Σε αυτήν τη ροή, η εφαρμογή λαμβάνει τα διακριτικά από απευθείας αλληλεπίδραση με το `/token` τελικού σημείου:

![Μονού εφαρμογή κλειστές λωρίδες εικόνας](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Για να δημιουργήσετε μια εφαρμογή υπηρεσίας, δείτε το πρόγραμμα-πελάτη documeenation διαπιστευτήρια μας ενότητας [Γρήγορα αποτελέσματα](active-directory-appmodel-v2-overview.md#getting-started) ή αναφέρονται σε [αυτήν την εφαρμογή δείγμα .NET](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

## <a name="current-limitations"></a>Τρέχουσα περιορισμοί
Αυτοί οι τύποι των εφαρμογών δεν υποστηρίζονται αυτήν τη στιγμή από το τελικό σημείο v2.0, αλλά βρίσκονται σε ο χάρτης.  Επιπλέον περιορισμούς και περιορισμούς για το τελικό σημείο v2.0 που περιγράφονται στο [άρθρο περιορισμοί v2.0](active-directory-v2-limitations.md).

### <a name="chained-web-apis-on-behalf-of"></a>Συνδεδεμένες web APIs (στο λογαριασμό-του)
Πολλές αρχιτεκτονικές περιλαμβάνουν ένα API Web που πρέπει να καλέσετε μια άλλη ροής API Web, και τα δύο ασφαλές από το τελικό σημείο v2.0.  Αυτό το σενάριο είναι συνηθισμένο σε εγγενή προγράμματα-πελάτες που διαθέτουν ένα παρασκηνίου Web API, το οποίο με τη σειρά καλεί μια υπηρεσία Microsoft Online όπως το Office 365 ή το API Graph.

Αυτό το σενάριο συνδεδεμένων Web API μπορεί να υποστηρίζονται χρησιμοποιώντας τη χορήγηση διακριτικό 2.0 Jwt φορέα διαπιστευτηρίων, αλλιώς γνωστές ως το [On-Behalf-Of ροής](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow).  Ωστόσο, η ροή On μέρους-του δεν έχει αυτήν τη στιγμή υλοποιηθεί σε το τελικό σημείο v2.0.  Για να δείτε πώς λειτουργεί η αυτήν τη ροή στο τα γενικά διαθέσιμο Azure AD εξυπηρέτησης πελατών, ανατρέξτε στο θέμα το [δείγμα στο λογαριασμό-του κώδικα στην GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).
