<properties
   pageTitle="Εργασία με βάσει αξιώσεων ταυτότητες σε εφαρμογές multitenant | Microsoft Azure"
   description="Πώς μια χρήση αξιώσεις για επικύρωση εκδότη και εξουσιοδότησης"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>Εργασία με ταυτότητες βάσει αξιώσεων στο multitenant εφαρμογές

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Σε αυτό το άρθρο αποτελεί [μέρος μιας σειράς]. Υπάρχει επίσης μια ολοκληρωμένη [δείγμα εφαρμογής] που συνοδεύει αυτήν τη σειρά.

## <a name="claims-in-azure-ad"></a>Αξιώσεις στο Azure AD

Όταν ένας χρήστης πραγματοποιεί είσοδο, Azure AD στέλνει ένα διακριτικό Αναγνωριστικό που περιέχει ένα σύνολο αξιώσεων σχετικά με το χρήστη. Απαίτηση είναι απλώς καταδείξετε κάποια πληροφορία, εκφρασμένη ως ένα ζεύγος κλειδιού/τιμής. Για παράδειγμα, `email` = `bob@contoso.com`.  Αξιώσεις έχετε έναν εκδότη &mdash; σε αυτήν την περίπτωση, Azure AD &mdash; που είναι η οντότητα που πραγματοποιεί έλεγχο ταυτότητας χρήστη και δημιουργεί το αξιώσεων. Μπορείτε να εμπιστευτείτε το αξιώσεων επειδή εμπιστεύεστε τον εκδότη. (Αντίθετα, εάν δεν εμπιστεύεστε τον εκδότη, δεν εμπιστεύεστε την αξιώσεων!)

Σε υψηλό επίπεδο:

1.  Ο χρήστης πραγματοποιεί έλεγχο ταυτότητας.
2.  Το IDP στέλνει ένα σύνολο αξιώσεων.
3.  Η εφαρμογή κανονικοποιεί ή αυξάνει την αξιώσεων (προαιρετικό).
4.  Η εφαρμογή χρησιμοποιεί το αξιώσεων για να αποφάσεις εξουσιοδότησης.

Στη σύνδεση OpenID, το σύνολο των αξιώσεων που μπορείτε να πάρετε ελέγχεται από την [παράμετρο εμβέλεια] της αίτησης για έλεγχο ταυτότητας. Ωστόσο, Azure AD ζητήματα ένα περιορισμένο σύνολο αξιώσεων μέσω OpenID σύνδεση; ανατρέξτε στο θέμα [τύποι διεκδίκηση και υποστηρίζονται διακριτικού]. Εάν θέλετε περισσότερες πληροφορίες σχετικά με το χρήστη, θα πρέπει να χρησιμοποιήσετε το API Azure AD Graph.

Ακολουθούν ορισμένα από τα αξιώσεων από AAD που εφαρμογής συνήθως να φροντίσει σχετικά με:

Τύπος στο διακριτικό Αναγνωριστικό δήλωσης |    Περιγραφή
-----------------------|--------------
πληροφορίες | Ποιος το διακριτικό έχει εκδοθεί για. Αυτό θα είναι το αναγνωριστικό της εφαρμογής υπολογιστή-πελάτη. Γενικά, δεν θα πρέπει πρέπει να ανησυχείτε σχετικά με το αίτημα αυτό, επειδή το ενδιάμεσο την επικυρώσει αυτόματα. Παράδειγμα:`"91464657-d17a-4327-91f3-2ed99386406f"`
ομάδες   | Μια λίστα των ομάδων AAD από τις οποίες ο χρήστης είναι μέλος. Παράδειγμα:`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
ISS  | Τον [εκδότη] του διακριτικού OIDC. Παράδειγμα:`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
Όνομα    | Το εμφανιζόμενο όνομα του χρήστη. Παράδειγμα:`"Alice A."`
αναγνωριστικό αντικειμένου | Το αναγνωριστικό αντικειμένου για το χρήστη στο AAD. Αυτή η τιμή είναι το αναγνωριστικό αμετάβλητες και μη με δυνατότητα επανάληψης χρήσης του χρήστη. Χρησιμοποιήστε αυτήν την τιμή, όχι ηλεκτρονικού ταχυδρομείου, ως ένα μοναδικό αναγνωριστικό για τους χρήστες; να αλλάξετε τις διευθύνσεις ηλεκτρονικού ταχυδρομείου. Εάν χρησιμοποιείτε το API Azure AD γράφημα στην εφαρμογή σας, το Αναγνωριστικό αντικειμένου είναι συγκεκριμένη τιμή που χρησιμοποιούνται για τις πληροφορίες του ερωτήματος προφίλ. Παράδειγμα:`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
ρόλοι   | Μια λίστα με τους ρόλους εφαρμογής για το χρήστη. Παράδειγμα:`["SurveyCreator"]`
TID | Αναγνωριστικό μισθωτή. Αυτή η τιμή είναι ένα μοναδικό αναγνωριστικό για το μισθωτή στο Azure AD. Παράδειγμα:`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | Ένα human αναγνώσιμο εμφανιζόμενο όνομα του χρήστη. Παράδειγμα:`"alice@contoso.com"`
UPN | Κύριο όνομα χρήστη. Παράδειγμα:`"alice@contoso.com"`

Αυτός ο πίνακας παραθέτει τους τύπους διεκδίκηση όπως εμφανίζονται στο διακριτικό Αναγνωριστικό. ASP.NET 1.0 πυρήνα, η σύνδεση OpenID ενδιάμεσο μετατρέπει ορισμένοι από τους τύπους διεκδίκηση όταν το συμπληρώνει τη συλλογή αξιώσεων για το κύριο χρήστη:

-   αναγνωριστικό αντικειμένου >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   TID >`http://schemas.microsoft.com/identity/claims/tenantid`
-   unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   UPN >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>Αξιώσεις μετασχηματισμοί

Κατά τη διάρκεια της ροής του ελέγχου ταυτότητας, ίσως θέλετε να τροποποιήσετε το αξιώσεων που λαμβάνετε από το IDP. Στο ASP.NET 1.0 πυρήνα, μπορείτε να εκτελέσετε μετασχηματισμό αξιώσεων μέσα σε το συμβάν **AuthenticationValidated** από το ενδιάμεσο OpenID σύνδεση. (Ανατρέξτε στο θέμα [συμβάντα ελέγχου ταυτότητας]).

Οποιαδήποτε αξιώσεων που προσθέτετε κατά τη διάρκεια **AuthenticationValidated** είναι αποθηκευμένα σε cookie της περιόδου λειτουργίας ελέγχου ταυτότητας. Αυτές δεν να προωθηθεί ξανά για να Azure AD.

Ακολουθούν ορισμένα παραδείγματα του μετασχηματισμού αξιώσεων:

-   **Κανονικοποίηση αξιώσεις**ή τις αξιώσεων συνεπείς σε χρήστες. Αυτό είναι ιδιαίτερα χρήσιμο εάν λαμβάνετε αξιώσεων από πολλές IDPs, η οποία μπορεί να χρησιμοποιήσει διεκδίκηση διαφορετικούς τύπους για παρόμοιες πληροφορίες.
Για παράδειγμα, Azure AD στέλνει μια αίτηση "upn" που περιέχει ηλεκτρονικού ταχυδρομείου του χρήστη. Άλλες IDPs ενδέχεται να σας στείλει μια απαίτηση "ηλεκτρονικού ταχυδρομείου". Ο ακόλουθος κώδικας μετατρέπει τη διεκδίκηση "upn" σε ένα αίτημα "Ηλεκτρονικό ταχυδρομείο":

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- Προσθήκη **προεπιλεγμένης διεκδίκηση τιμές** για αξιώσεων που δεν υπάρχουν &mdash; για παράδειγμα, η ανάθεση σε ένα ρόλο προεπιλογή. Σε ορισμένες περιπτώσεις, αυτό μπορεί να απλοποιήσει λογικής εξουσιοδότησης.
- Προσθήκη **προσαρμοσμένου διεκδίκηση τύπους** με συγκεκριμένη εφαρμογή πληροφορίες σχετικά με το χρήστη. Για παράδειγμα, ενδέχεται να μπορείτε να αποθηκεύσετε ορισμένες πληροφορίες σχετικά με το χρήστη σε μια βάση δεδομένων. Θα μπορούσατε να προσθέσετε ένα προσαρμοσμένο διεκδίκηση με αυτές τις πληροφορίες του δελτίου ελέγχου ταυτότητας. Το αίτημα αποθηκεύονται σε ένα cookie, έτσι ώστε μόνο πρέπει να το λάβετε από τη βάση δεδομένων μία φορά σε κάθε περίοδο λειτουργίας σύνδεσης. Από την άλλη πλευρά, μπορείτε επίσης να θέλετε να αποφύγετε τη δημιουργία υπερβολικά μεγάλη cookies, επομένως πρέπει να λάβετε υπόψη το συσχετισμό μεταξύ μέγεθος cookie έναντι αναζητήσεις βάσης δεδομένων.   

Μετά την ολοκλήρωση της ροής ελέγχου ταυτότητας, το αξιώσεων είναι διαθέσιμες στο `HttpContext.User`. Σε αυτό το σημείο, θα πρέπει να διαχειρίζεστε τους ως μόνο για ανάγνωση συλλογή &mdash; π.χ., να τις χρησιμοποιήσουν για λήψη αποφάσεων εξουσιοδότησης.

## <a name="issuer-validation"></a>Εκδότης επικύρωσης
Στη σύνδεση OpenID, το αίτημα εκδότη ("iss") προσδιορίζει το IDP που εξέδωσε το διακριτικό Αναγνωριστικό. Το τμήμα της ροής της OIDC ελέγχου ταυτότητας είναι για να επιβεβαιώσετε ότι το αίτημα εκδότη συμφωνεί με τον πραγματικό εκδότη. Το ενδιάμεσο OIDC χειρίζεται αυτό για εσάς.

Στο Azure AD, η τιμή εκδότη είναι μοναδικό ανά μισθωτή AD (`https://sts.windows.net/<tenantID>`). Γι ' αυτό, μια εφαρμογή θα πρέπει να κάνετε μια επιπλέον ελέγχου, για να βεβαιωθείτε ότι ο εκδότης αντιπροσωπεύει έναν μισθωτή που επιτρέπεται για να πραγματοποιήσετε είσοδο στην εφαρμογή.

Για μια εφαρμογή μίας μισθωτή, μπορείτε να ελέγξετε μόνο ότι ο εκδότης είναι το δικό σας μισθωτή. Στην πραγματικότητα, το ενδιάμεσο OIDC κάνει αυτόματα από προεπιλογή. Σε μια εφαρμογή πολλών μισθωτή, πρέπει να επιτρέψετε για πολλές εκδότες, αντιστοιχεί με το διαφορετικό μισθωτές. Ακολουθεί μια γενική προσέγγιση για να χρησιμοποιήσετε:

-   Στις επιλογές ενδιάμεσο OIDC, ορίστε **ValidateIssuer** FALSE (ψευδής). Αυτό απενεργοποιεί τον αυτόματο έλεγχο.
-   Όταν ένας μισθωτής εγγράφεται, αποθήκευση στο μισθωτή και τον εκδότη στο χρήστη σας DB.
-   Κάθε φορά που ένας χρήστης πραγματοποιεί είσοδο, αναζητήστε τον εκδότη στη βάση δεδομένων. Εάν δεν βρεθεί τον εκδότη, αυτό σημαίνει ότι δεν έχει εγγραφεί αυτόν το μισθωτή. Μπορείτε να ανακατευθύνετε τους σε μια εγγραφή σελίδας.
-  Μπορείτε επίσης μπορεί να blacklist ορισμένες μισθωτές; Για παράδειγμα, για τους πελάτες που δεν πληρώνετε της συνδρομής.

Για πιο λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [εγγραφής και μισθωτή προσθήκης λογαριασμών σε μια εφαρμογή multitenant][signup].

## <a name="using-claims-for-authorization"></a>Αξιώσεις για την άδεια χρήσης

Με αξιώσεων, ταυτότητα του χρήστη δεν είναι πλέον ένα μονολιθικό οντότητα. Για παράδειγμα, ένας χρήστης μπορεί να έχει μια διεύθυνση ηλεκτρονικού ταχυδρομείου, τον αριθμό τηλεφώνου, γενέθλια, gender, κ.λπ. Μήπως IDP του χρήστη αποθηκεύει όλες αυτές τις πληροφορίες. Αλλά κατά τον έλεγχο ταυτότητας χρήστη, θα λάβετε συνήθως ένα υποσύνολο από αυτές τις ως αξιώσεων. Σε αυτό το μοντέλο, την ταυτότητα του χρήστη είναι απλώς μια δέσμη αξιώσεων. Όταν κάνετε αποφάσεις εξουσιοδότησης σχετικά με ένα χρήστη, θα αναζητήσει συγκεκριμένο σύνολα αξιώσεων. Με άλλα λόγια, η ερώτηση "Μπορεί να χρήστη X εκτέλεση ενέργειας Y" γίνεται ο τελικός "έχουν X δήλωσης χρήστη από το Ω".

Ακολουθούν ορισμένα βασικά μοτίβα για τον έλεγχο αξιώσεων.

-  Για να ελέγξετε ότι ο χρήστης έχει μια συγκεκριμένη διεκδίκηση με μια συγκεκριμένη τιμή:

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    Αυτός ο κωδικός ελέγχει εάν ο χρήστης διαθέτει ένα ρόλο διεκδίκηση με την τιμή "Διαχειριστής". Χειρίζεται σωστά την περίπτωση όπου ο χρήστης έχει χωρίς απαίτηση ρόλο ή πολλών αξιώσεων ρόλο.

    Η κλάση **ClaimTypes** ορίζει σταθερές για τους τύπους που χρησιμοποιούνται ευρέως διεκδίκηση. Ωστόσο, μπορείτε να χρησιμοποιήσετε οποιαδήποτε τιμή συμβολοσειράς για τον τύπο διεκδίκηση.

-   Για να λάβετε μια μοναδική τιμή για έναν τύπο απαίτησης, ενώ αναμένετε εκεί για να το πολύ μία τιμή:
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   Για να δείτε όλες τις τιμές για έναν τύπο απαίτησης:

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [βάσει ρόλων και πόρων βάσει εξουσιοδότησης σε εφαρμογές multitenant][authorization].

## <a name="next-steps"></a>Επόμενα βήματα

- Διαβάστε το επόμενο άρθρο σε αυτήν τη σειρά: [εγγραφής και μισθωτή προσθήκης λογαριασμών σε μια εφαρμογή multitenant][signup]
- Μάθετε περισσότερα σχετικά με [βάσει αξιώσεων εξουσιοδότησης] στην τεκμηρίωση 1.0 πυρήνα ASP.NET

<!-- Links -->
[μέρος μιας σειράς]: guidance-multitenant-identity.md
[η παράμετρος εύρους]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[Τύποι διεκδίκηση και υποστηριζόμενες διακριτικού]: ../active-directory/active-directory-token-and-claims.md
[εκδότης]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[Έλεγχος ταυτότητας συμβάντα]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[Βάσει αξιώσεων εξουσιοδότησης]: https://docs.asp.net/en/latest/security/authorization/claims.html
[δείγμα εφαρμογής]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md