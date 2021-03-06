<properties
    pageTitle="Περιορισμοί τελικού σημείου v2.0 & περιορισμούς | Microsoft Azure"
    description="Μια λίστα με τους περιορισμούς και τους περιορισμούς με το τελικό σημείο v2.0 Azure AD."
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

# <a name="should-i-use-the-v20-endpoint"></a>Πρέπει να χρησιμοποιήσω το τελικό σημείο v2.0;

Κατά τη δημιουργία εφαρμογών που ενοποιούνται με το Azure Active Directory, θα πρέπει να αποφασίσετε αν τα πρωτόκολλα τελικό σημείο και τον έλεγχο ταυτότητας v2.0 καλύπτει τις ανάγκες σας.  Το αρχικό τελικό σημείο Azure AD υποστηρίζεται πλήρως ακόμη και σε ορισμένα σημεία, είναι περισσότερες πλούσια από v2.0.  Ωστόσο, το v2.0 τελικού σημείου [παρουσιάζει σημαντικά οφέλη](active-directory-v2-compare.md) για τους προγραμματιστές που μπορεί να σας δελεάσουν για να χρησιμοποιήσετε το νέο μοντέλο προγραμματισμού.

Σε αυτό το σημείο στο φορά, μας προτάσεων σχετικά με χρήση του τελικού σημείου v2.0 είναι ως εξής:

- Εάν θέλετε να υποστηρίζει προσωπικοί λογαριασμοί Microsoft στην εφαρμογή σας, θα πρέπει να χρησιμοποιείτε το τελικό σημείο v2.0.  Αλλά βεβαιωθείτε ότι έχετε κατανοήσει τους περιορισμούς που αναφέρονται σε αυτό το άρθρο, ιδιαίτερα εκείνων που αφορούν ειδικά εργασία & σχολικών λογαριασμών.
- Εάν η εφαρμογή σας απαιτεί μόνο σχολικοί λογαριασμοί & υποστήριξης εργασίας, θα πρέπει να χρησιμοποιείτε [το αρχικό Azure AD τα τελικά σημεία](active-directory-developers-guide.md).

Διάρκεια του χρόνου, το τελικό σημείο v2.0 θα αυξηθεί για να αποφύγετε τους περιορισμούς που εμφανίζεται εδώ, έτσι ώστε μόνο ποτέ θα χρειαστεί να χρησιμοποιήσετε το τελικό σημείο v2.0.  Στο μεταξύ, αυτό το άρθρο προορίζονται για να καθορίσετε εάν είναι το τελικό σημείο v2.0 για εσάς.  Θα συνεχίσουμε να ενημερώσετε αυτό το άρθρο με τον καιρό ώστε να απεικονίζουν την τρέχουσα κατάσταση του τελικού σημείου v2.0, επομένως, ελέγξτε ξανά για να ελέγξετε τις απαιτήσεις σας σε σχέση με τις δυνατότητες v2.0.

Εάν έχετε μια υπάρχουσα εφαρμογή με το Azure AD που δεν χρησιμοποιεί το τελικό σημείο v2.0, δεν χρειάζεται να ξεκινήσετε από την αρχή.  Στο μέλλον, θα σας θα παρέχοντας ένας τρόπος για να ενεργοποιήσετε τις υπάρχουσες εφαρμογές Azure AD για χρήση με το τελικό σημείο v2.0.

## <a name="restrictions-on-apps"></a>Περιορισμοί για εφαρμογές
Τους ακόλουθους τύπους εφαρμογών δεν υποστηρίζονται αυτήν τη στιγμή από το τελικό σημείο v2.0.  Για μια περιγραφή των τύπων υποστηριζόμενες εφαρμογές, ανατρέξτε [σε αυτό το άρθρο](active-directory-v2-flows.md).

##### <a name="standalone-web-apis"></a>APIs αυτόνομης υπηρεσίας Web
Με το τελικό σημείο v2.0, έχετε τη δυνατότητα να [δημιουργήσετε ένα API Web που προστατεύεται με διακριτικό 2.0](active-directory-v2-flows.md#web-apis).  Ωστόσο, αυτό το API Web μόνο θα μπορούν να λαμβάνουν τα διακριτικά από μια εφαρμογή που κάνει κοινή χρήση το ίδιο αναγνωριστικό εφαρμογής.  Δημιουργία μιας API που είναι προσβάσιμες από ένα πρόγραμμα-πελάτη με διαφορετικό αναγνωριστικό εφαρμογής web δεν υποστηρίζεται.  Αυτό το πρόγραμμα-πελάτης δεν θα μπορείτε να ζητήσετε ή να αποκτήσετε δικαιώματα για το API web.

Για να δείτε πώς μπορείτε να δημιουργήσετε ένα API Web που δέχεται διακριτικά από ένα πρόγραμμα-πελάτη με το ίδιο αναγνωριστικό εφαρμογής, ανατρέξτε στο θέμα τα δείγματα v2.0 τελικού σημείου Web API [Γρήγορα](active-directory-appmodel-v2-overview.md#getting-started)αποτελέσματα.

##### <a name="web-api-on-behalf-of-flow"></a>API Web στο λογαριασμό-του ροής
Πολλές αρχιτεκτονικές περιλαμβάνουν ένα API Web που πρέπει να καλέσετε μια άλλη ροής API Web, και τα δύο ασφαλές από το τελικό σημείο v2.0.  Αυτό το σενάριο είναι συνηθισμένο σε εγγενή προγράμματα-πελάτες που διαθέτουν ένα παρασκηνίου Web API, το οποίο με τη σειρά καλεί μια ηλεκτρονική υπηρεσία της Microsoft ή άλλη ενσωματωμένη προσαρμοσμένη web API που υποστηρίζει το Azure AD.

Αυτό το σενάριο είναι δυνατή η υποστήριξη χρησιμοποιώντας τη χορήγηση διακριτικό 2.0 Jwt φορέα διαπιστευτηρίων, αλλιώς γνωστές ως το On-Behalf-Of ροής.  Ωστόσο, η ροή On μέρους-του δεν υποστηρίζεται αυτήν τη στιγμή για το τελικό σημείο v2.0.  Για να δείτε πώς λειτουργεί η αυτήν τη ροή στο τα γενικά διαθέσιμο Azure AD εξυπηρέτησης πελατών, ανατρέξτε στο θέμα το [δείγμα στο λογαριασμό-του κώδικα στην GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).

## <a name="restrictions-on-app-registrations"></a>Περιορισμοί σχετικά με την εφαρμογή καταχωρήσεων
Σε αυτό το σημείο στο φορά, όλες οι εφαρμογές που θέλετε να ενσωματώσετε με το τελικό σημείο v2.0 πρέπει να δημιουργήσετε μια νέα καταχώρηση εφαρμογής στο [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Οποιαδήποτε υπάρχοντα Azure AD ή εφαρμογές λογαριασμό Microsoft που διαθέτετε δεν θα είναι συμβατά με το τελικό σημείο v2.0, ούτε θα εφαρμογές που έχουν καταχωρηθεί στην πύλη οποιαδήποτε εκτός από τη νέα πύλη καταχώρηση εφαρμογής.  Στόχος μας είναι να παρέχει κάποιο τρόπο για να ενεργοποιήσετε την υπάρχουσα εφαρμογές που χρησιμοποιούνται ως v2.0 εφαρμογής. Αυτήν τη στιγμή όμως, δεν υπάρχει καμία διαδρομή μετεγκατάστασης για μια εφαρμογή για το τελικό σημείο v2.0.

Ομοίως, οι εφαρμογές που έχουν καταχωρηθεί στην πύλη του νέα καταχώρηση εφαρμογής δεν θα λειτουργούν σε σχέση με το αρχικό τελικό σημείο ελέγχου ταυτότητας Azure AD.  Ωστόσο, χρησιμοποιείτε εφαρμογές που έχουν δημιουργηθεί με την πύλη καταχώρηση εφαρμογής για να ενοποιήσετε με επιτυχία με το τελικό σημείο ελέγχου ταυτότητας λογαριασμού Microsoft, `https://login.live.com`.

Επιπλέον, καταχωρήσεων εφαρμογής που δημιουργήθηκε στο [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) έχει την παρακάτω προειδοποιήσεις:

- Η ιδιότητα **κεντρική σελίδα** , γνωστές και ως το **σύμβολο στη διεύθυνση URL** δεν υποστηρίζεται.  Χωρίς αρχικής σελίδας, αυτές οι εφαρμογές δεν θα εμφανίζεται στον πίνακα "Office στοιχείο οι εφαρμογές μου".
- Μόνο δύο απόρρητο εφαρμογή επιτρέπονται ανά εφαρμογή Id αυτήν τη στιγμή.
- Μια καταχώρηση εφαρμογής δυνατό μόνο να προβάλετε και να ελέγχονται από ένα λογαριασμό μόνο για προγραμματιστές.  Αυτό δεν μπορεί να είναι κοινόχρηστη μεταξύ πολλών προγραμματιστές.
- Υπάρχουν αρκετές περιορισμοί σχετικά με τη μορφή του redirect_uri επιτρέπεται.  Ανατρέξτε στην παρακάτω ενότητα για περισσότερες λεπτομέρειες.

## <a name="restrictions-on-redirect-uris"></a>Περιορισμοί σχετικά με τα URI ανακατεύθυνσης
Εφαρμογές που έχουν καταχωρηθεί στην πύλη του νέα καταχώρηση εφαρμογής είναι περιορισμένα αυτήν τη στιγμή για ένα περιορισμένο σύνολο τιμών redirect_uri.  Το redirect_uri για εφαρμογές web και υπηρεσίες πρέπει να ξεκινούν με το συνδυασμό `https`, και όλες τις τιμές redirect_uri πρέπει να κάνετε κοινή χρήση μόνο τομέα DNS.  Για παράδειγμα, δεν μπορείτε να καταχωρήσετε μια εφαρμογή web που έχει redirect_uris:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Το σύστημα καταχώρησης συγκρίνει το ολόκληρη όνομα DNS από το υπάρχον redirect_uri με το όνομα DNS από το redirect_uri που προσθέτετε. Εάν οποιαδήποτε από τις ακόλουθες συνθήκες πληρούνται θα αποτύχει την αίτηση για να προσθέσετε το όνομα DNS:  

- Εάν το όνομα ολόκληρη DNS από το νέο redirect_uri δεν ταιριάζουν με το όνομα DNS από το υπάρχον redirect_uri
- Εάν το όνομα ολόκληρη DNS από το νέο redirect_uri δεν είναι έναν υποτομέα από το υπάρχον redirect_uri

Για παράδειγμα, εάν η εφαρμογή έχει αυτήν τη στιγμή redirect_uri:

`https://login.contoso.com`

Στη συνέχεια, είναι δυνατό να προσθέσετε:

`https://login.contoso.com/new`

που ταιριάζει απόλυτα με το όνομα DNS, ή:

`https://new.login.contoso.com`

που είναι έναν υποτομέα DNS του login.contoso.com.  Εάν θέλετε να έχετε μια εφαρμογή που έχει login east.contoso.com και login-west.contoso.com ως redirect_uris, στη συνέχεια, που πρέπει να προσθέσετε την παρακάτω redirect_uris με τη σειρά:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Τα τελευταία δύο μπορεί να προστεθεί, επειδή είναι δευτερεύοντες τομείς από την πρώτη redirect_uri, contoso.com. Αυτός ο περιορισμός θα καταργηθεί σε μια προσεχή έκδοση.

Για να μάθετε πώς μπορείτε να καταχωρήσετε μια εφαρμογή στην πύλη του νέα καταχώρηση εφαρμογής, ανατρέξτε [σε αυτό το άρθρο](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services--apis"></a>Περιορισμοί για τις υπηρεσίες & APIs
Το τελικό σημείο v2.0 προς το παρόν υποστηρίζει είσοδος για οποιαδήποτε εφαρμογή καταχωρηθεί στην νέα εφαρμογή δήλωσης πύλη, υπό την προϋπόθεση ότι εμπίπτουν σε μια λίστα ροών [υποστηριζόμενες ελέγχου ταυτότητας](active-directory-v2-flows.md).  Ωστόσο, αυτές οι εφαρμογές μόνο να μπορούν να αποκτούν οι κωδικοί πρόσβασης OAuth 2.0 για ένα περιορισμένο σύνολο πόρων.  Το τελικό σημείο v2.0 μόνο θα ζητήματος access_tokens για:

- Η εφαρμογή που ζητήθηκε το διακριτικό.  Μια εφαρμογή μπορεί να αποκτήσει ένα access_token για τον εαυτό σας, εάν η λογική εφαρμογή αποτελείται από πολλές διαφορετικές στοιχεία ή σειρές.  Για να δείτε αυτό το σενάριο στην πράξη, δείτε τα προγράμματα εκμάθησης [Γρήγορα αποτελέσματα](active-directory-appmodel-v2-overview.md#getting-started) .
- Η αλληλογραφία του Outlook, το ημερολόγιο και επαφές REST API, οι οποίες βρίσκονται στο https://outlook.office.com.  Για να μάθετε πώς να συντάξετε μια εφαρμογή που έχει πρόσβαση σε αυτά τα API, ανατρέξτε σε αυτά τα προγράμματα εκμάθησης για το [Office γρήγορα αποτελέσματα](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) .
- Τα API Microsoft Graph.  Για να μάθετε περισσότερα σχετικά με το Microsoft Graph και όλα τα δεδομένα που είναι διαθέσιμες, επισκεφθείτε [https://graph.microsoft.io](https://graph.microsoft.io).

Δεν υπάρχουν άλλες υπηρεσίες υποστηρίζονται αυτήν τη στιγμή.  Περισσότερες υπηρεσίες Microsoft Online θα προστεθεί στο μέλλον, καθώς και υποστήριξη για το δικό σας προσαρμοσμένο δομημένες API Web και τις υπηρεσίες.

## <a name="restrictions-on-libraries--sdks"></a>Περιορισμοί για βιβλιοθήκες & SDK
Βιβλιοθήκη υποστήριξης για το τελικό σημείο v2.0 είναι αρκετά περιορισμένα αυτήν τη στιγμή.  Εάν θέλετε να χρησιμοποιήσετε το τελικό σημείο v2.0 σε μια εφαρμογή παραγωγής, έχετε τις εξής επιλογές:

- Εάν δημιουργείτε μια εφαρμογή web, μπορείτε να χρησιμοποιήσετε με ασφάλεια μας γενικά διαθέσιμο το ενδιάμεσο πλευρά του διακομιστή για να πραγματοποιήσετε είσοδο στο και διακριτικό επικύρωσης.  Αυτά περιλαμβάνουν το ενδιάμεσο OWIN Άνοιγμα Αναγνωριστικό σύνδεση για την προσθήκη NodeJS Passport και ASP.NET.  Δείγματα κώδικα χρησιμοποιώντας το ενδιάμεσο μας είναι διαθέσιμες σε μας καθώς και ενότητα [Γρήγορα αποτελέσματα](active-directory-appmodel-v2-overview.md#getting-started) .
- Για άλλες πλατφόρμες και εφαρμογές native & κινητές συσκευές, μπορείτε να ενοποιήσετε επίσης με το τελικό σημείο v2.0 με απευθείας αποστολή & παραλαβή μηνυμάτων πρωτόκολλο στον κώδικα της εφαρμογής σας.  Η σύνδεση OpenID και OAuth πρωτόκολλα που [έχουν ρητά τεκμηριώνονται](active-directory-v2-protocols.md) θα σας βοηθήσουν να εκτελέσετε μια τέτοια ενσωμάτωση v2.0.
- Τέλος, μπορείτε να χρησιμοποιήσετε βιβλιοθήκες άνοιγμα σύνδεση Αναγνωριστικό και διακριτικό Άνοιγμα αρχείου προέλευσης για την ενοποίηση με το τελικό σημείο v2.0.  Το πρωτόκολλο v2.0 πρέπει να είναι συμβατά με πολλές βιβλιοθήκες πρωτόκολλο Άνοιγμα αρχείου προέλευσης χωρίς σημαντικών αλλαγών.  Η διαθεσιμότητα όπως βιβλιοθήκες ποικίλλει ανά γλώσσα και πλατφόρμα και τις τοποθεσίες Web [Άνοιγμα σύνδεση Αναγνωριστικό](http://openid.net/connect/) και [διακριτικό 2.0](http://oauth.net/2/) διατηρεί μια λίστα με τις δημοφιλείς υλοποιήσεις. Για περισσότερες λεπτομέρειες και τη λίστα των δειγμάτων που έχουν ελεγχθεί με το τελικό σημείο v2.0 και άνοιγμα αρχείου προέλευσης προγράμματος-πελάτη βιβλιοθήκες, ανατρέξτε στο θέμα [βιβλιοθήκες v2.0 και τον έλεγχο ταυτότητας Azure Active Directory (AD)](active-directory-v2-libraries.md) .

Θα σας έχουν επίσης κυκλοφορήσει μια αρχική προεπισκόπηση από τη [Βιβλιοθήκη ελέγχου ταυτότητας Microsoft (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) για το .NET μόνο.  Είστε υποδοχής για να δοκιμάσετε αυτήν τη βιβλιοθήκη σε εφαρμογές προγράμματος-πελάτη και διακομιστή .NET, αλλά ως προεπισκόπηση βιβλιοθήκη αυτή δεν συνοδεύεται από GA ποιότητας υποστήριξης.

## <a name="restrictions-on-protocols"></a>Περιορισμοί για τα πρωτόκολλα
Το τελικό σημείο v2.0 υποστηρίζει μόνο άνοιγμα σύνδεση Αναγνωριστικό & OAuth 2.0.  Ωστόσο, όχι σε όλες τις δυνατότητες και τις δυνατότητες του κάθε πρωτοκόλλου έχουν ενσωματωθεί στην το τελικό σημείο v2.0, όπως:

- Η σύνδεση OpenID `end_session_endpoint`, γεγονός που επιτρέπει μια εφαρμογή για να τερματίσετε την περίοδο λειτουργίας του χρήστη με το τελικό σημείο v2.0.
- id_tokens εκδοθεί από το τελικό σημείο v2.0 περιέχει μόνο ένα ζεύγους αναγνωριστικό για το χρήστη.  Αυτό σημαίνει ότι δύο διαφορετικές εφαρμογές θα λαμβάνετε διαφορετικά αναγνωριστικά για τον ίδιο χρήστη.  Σημειώστε ότι κατά την υποβολή ερωτημάτων στο Microsoft Graph `/me` τελικό σημείο, θα μπορείτε να λάβετε ένα correlatable ID για το χρήστη που μπορούν να χρησιμοποιηθούν σε εφαρμογές.
- id_tokens εκδοθεί από το τελικό σημείο v2.0 δεν περιέχουν μια `email` διεκδίκηση για το χρήστη αυτήν τη στιγμή, ακόμα και αν αποκτάτε δικαιώματα από το χρήστη για να δείτε το μήνυμα ηλεκτρονικού ταχυδρομείου.
- Το τελικό σημείο OpenID σύνδεση πληροφορίες χρήστη. Το τελικό σημείο πληροφορίες χρήστη δεν έχει υλοποιηθεί σε το τελικό σημείο v2.0 αυτήν τη στιγμή.  Ωστόσο, όλα τα δεδομένα προφίλ χρήστη πιθανώς θα εμφανιστεί αυτό το τελικό σημείο είναι διαθέσιμη από τη Microsoft Graph `/me` τελικού σημείου.
- Ρόλος & δηλώσεις ομάδας.  Προς το παρόν, το τελικό σημείο v2.0 δεν υποστηρίζει έκδοσης αξιώσεων ρόλο ή μια ομάδα σε id_tokens.

Για να κατανοήσετε καλύτερα το εύρος του πρωτοκόλλου λειτουργίες που υποστηρίζονται από το τελικό σημείο v2.0, διαβάστε τη [σύνδεση OpenID & OAuth 2.0 πρωτόκολλο αναφοράς](active-directory-v2-protocols.md).

## <a name="restrictions-for-work--school-accounts"></a>Περιορισμοί για λογαριασμούς του σχολείου & εργασία
Υπάρχουν μερικές δυνατότητες ειδικά για Microsoft εταιρεία/επαγγελματίες χρήστες που δεν υποστηρίζονται ακόμη από το τελικό σημείο v2.0.

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>Πρόσβασης υπό όρους που βασίζεται σε συσκευή, εφαρμογές εγγενής και κινητές συσκευές και το Microsoft Graph
Το τελικό σημείο v2.0 δεν υποστηρίζει ακόμη συσκευή ελέγχου ταυτότητας για κινητές συσκευές και εγγενούς εφαρμογές, όπως εγγενή εφαρμογές που εκτελείται σε iOS ή Android.  Αυτό θα μπορούσε να αποκλείσετε την εγγενή εφαρμογή από την κλήση του Microsoft Graph για ορισμένες εταιρείες.  Απαιτείται έλεγχος ταυτότητας συσκευή, όταν ο διαχειριστής Ρυθμίζει μια πολιτική πρόσβασης υπό όρους που βασίζεται σε συσκευή σε μια εφαρμογή.  Για το τελικό σημείο v2.0, το πιο πιθανό σενάριο για πρόσβασης υπό όρους που βασίζεται σε συσκευή είναι διαχειριστής τη ρύθμιση μιας πολιτικής σε έναν πόρο στο Microsoft Graph, όπως το API του Outlook.  Εάν ο διαχειριστής ρυθμίζει αυτή την πολιτική και εγγενή εφαρμογή σας ζητά ένα διακριτικό στο Microsoft Graph, την αίτηση τελικά θα αποτύχει επειδή η συσκευή δεν υποστηρίζεται έλεγχος ταυτότητας ακόμη.  Εφαρμογές Web που αίτηση διακριτικά στο Microsoft Graph, ωστόσο, υποστηρίζονται όταν έχουν ρυθμιστεί οι παράμετροι πολιτικές που βασίζονται σε συσκευή.  Στην τοποθεσία web app σενάριο συσκευή ο έλεγχος ταυτότητας πραγματοποιείται μέσω προγράμματος περιήγησης web του χρήστη.

Προγραμματιστής, πιθανότατα έχουν κανένα στοιχείο ελέγχου όταν πολιτικές έχουν οριστεί σε πόρους Microsoft Graph, ή ακόμα και υπόψη όταν συμβαίνει αυτό.  Εάν δημιουργείτε μια εφαρμογή για χρήστες εργασίας και του σχολείου, πρέπει να χρησιμοποιήσετε [το αρχικό τελικό σημείο Azure AD](active-directory-developers-guide.md) μέχρι το τελικό σημείο v2.0 υποστηρίζει έλεγχο ταυτότητας συσκευή.  Για περισσότερες πληροφορίες σχετικά με την πρόσβαση υπό όρους που βασίζεται σε συσκευή, ανατρέξτε στο θέμα [σε αυτό το άρθρο](active-directory-conditional-access.md#device-based-conditional-access).

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>Ενσωματωμένη ελέγχου ταυτότητας των Windows για εξωτερική μισθωτές
Εάν έχετε χρησιμοποιήσει ADAL (και επομένως το αρχικό τελικό σημείο Azure AD) σε εφαρμογές των Windows, ενδέχεται να έχουν τεθεί εκμεταλλευτείτε που είναι γνωστό ως η εκχώρηση SAML διεκδίκηση.  Εκχώρηση αυτή επιτρέπει στους χρήστες της ομόσπονδη Azure AD μισθωτές σιωπηρά ο έλεγχος ταυτότητας με παρουσία υπηρεσίας καταλόγου Active Directory εσωτερικής εγκατάστασης χωρίς να χρειάζεται να εισαγάγετε τα διαπιστευτήριά τους.  Η εκχώρηση SAML διεκδίκηση αυτήν τη στιγμή δεν υποστηρίζεται σε το τελικό σημείο v2.0.
