<properties
   pageTitle="Εξουσιοδότηση σε εφαρμογές multitenant | Microsoft Azure"
   description="Πώς να εκτελείτε εξουσιοδότησης σε μια εφαρμογή multitenant"
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
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>Βάσει ρόλων και πόρων βάσει εξουσιοδότησης σε multitenant εφαρμογές

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Σε αυτό το άρθρο αποτελεί [μέρος μιας σειράς]. Υπάρχει επίσης μια ολοκληρωμένη [δείγμα εφαρμογής] που συνοδεύει αυτήν τη σειρά.

Μας [υλοποίηση αναφορά] είναι μια εφαρμογή του ASP.NET πυρήνα 1.0. Σε αυτό το άρθρο θα εξετάσουμε δύο γενικές προσεγγίσεις για έγκριση, η χρήση της άδειας APIs που παρέχεται στην υπηρεσία ASP.NET 1.0 πυρήνα.

-   **Εξουσιοδότηση βάσει ρόλων**. Εξουσιοδότησης μια ενέργεια με βάση τους ρόλους που έχουν εκχωρηθεί σε ένα χρήστη. Για παράδειγμα, ορισμένες ενέργειες απαιτούν ρόλου διαχειριστή.
-   **Εξουσιοδότηση βάσει πόρων**. Εξουσιοδότησης μια ενέργεια που βασίζεται σε ένα συγκεκριμένο πόρο. Για παράδειγμα, κάθε πόρο έχει έναν κάτοχο. Ο κάτοχος μπορεί να διαγράψει τον πόρο. οι άλλοι χρήστες δεν είναι δυνατή.

Μια τυπική εφαρμογή θα χρησιμοποιούν συνδυασμό και των δύο. Για παράδειγμα, για να διαγράψετε έναν πόρο, ο χρήστης πρέπει να είναι ο πόρος κάτοχος _ή_ διαχειριστής.


## <a name="role-based-authorization"></a>Εξουσιοδότηση βάσει ρόλων

[Έρευνες Tailspin] [ Tailspin] εφαρμογή ορίζει τους εξής ρόλους:

- Διαχειριστής. Να εκτελέσετε όλες οι λειτουργίες CRUD σε οποιαδήποτε έρευνα που ανήκει σε αυτόν το μισθωτή.
- Εκπαιδευτικών. Να δημιουργήσετε νέες έρευνες
- Πρόγραμμα ανάγνωσης. Διαβάστε τις έρευνες που ανήκουν σε αυτόν το μισθωτή

Ρόλοι ισχύουν για _τους χρήστες_ της εφαρμογής. Στην εφαρμογή έρευνες, ένας χρήστης είναι ένα διαχειριστή, εκπαιδευτικών, ή ανάγνωσης.

Για πληροφορίες σχετικά με τον τρόπο για να ορίσετε και να διαχειριστείτε τους ρόλους, ανατρέξτε στο θέμα [εφαρμογή ρόλους].

Ανεξάρτητα από το πώς μπορείτε να διαχειριστείτε τους ρόλους, τον κωδικό εξουσιοδότησης θα είναι παρόμοιο. Πυρήνα ASP.NET 1.0 παρουσιάζει μια αφαίρεση που ονομάζεται [πολιτικές εξουσιοδότησης][policies]. Με αυτήν τη δυνατότητα, ορίζετε πολιτικές εξουσιοδότησης στον κώδικα και, στη συνέχεια, εφαρμόστε αυτές τις πολιτικές ελεγκτή ενέργειες. Η πολιτική αποσυνδέεται από τον ελεγκτή.

### <a name="create-policies"></a>Δημιουργία πολιτικών

Για να ορίσετε μια πολιτική, πρώτα να δημιουργήσετε ένα εκπαιδευτικό που υλοποιεί `IAuthorizationRequirement`. Είναι πιο εύκολο να προέρχεται από `AuthorizationHandler`. Στο το `Handle` μέθοδο, εξετάστε το σχετικό claim(s).

Ακολουθεί ένα παράδειγμα από την εφαρμογή Tailspin έρευνες:

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] Ανατρέξτε στο θέμα [SurveyCreatorRequirement.cs]

Αυτή η κλάση καθορίζει την απαίτηση για ένα χρήστη για να δημιουργήσετε μια νέα έρευνα. Ο χρήστης πρέπει να είναι ο ρόλος SurveyAdmin ή SurveyCreator.

Στην τάξη σας εκκίνησης, Ορισμός πολιτικής με όνομα που περιλαμβάνει ένα ή περισσότερα απαιτήσεις. Εάν υπάρχουν πολλά απαιτήσεις, ο χρήστης πρέπει να πληροί _κάθε_ απαίτηση για να επιτρέπεται. Ο ακόλουθος κώδικας ορίζει δύο πολιτικές:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] Ανατρέξτε στο θέμα [Startup.cs]

Αυτός ο κωδικός ορίζει επίσης το σχήμα ελέγχου ταυτότητας, που σας ενημερώνει για το ποια ενδιάμεσο έλεγχο ταυτότητας πρέπει να εκτελέσετε εάν αποτύχει η εξουσιοδότηση ASP.NET. Σε αυτήν την περίπτωση, θα σας Καθορίστε το ενδιάμεσο το cookie ελέγχου ταυτότητας, επειδή το ενδιάμεσο έλεγχο ταυτότητας cookie να ανακατευθύνετε το χρήστη σε μια σελίδα "Απαγορεύεται". Η θέση της σελίδας απαγορεύεται έχει οριστεί από την επιλογή AccessDeniedPath το ενδιάμεσο cookie; ανατρέξτε στο θέμα [ρύθμιση των παραμέτρων του ελέγχου ταυτότητας ενδιάμεσου λογισμικού].

### <a name="authorize-controller-actions"></a>Εγκρίνετε ελεγκτή ενέργειες

Τέλος, για να επικυρώσετε μια ενέργεια σε ένα ελεγκτή MVC, ορίστε την πολιτική το `Authorize` χαρακτηριστικό:

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

Σε παλαιότερες εκδόσεις του ASP.NET, μπορείτε να ορίσετε την ιδιότητα **τους ρόλους** στο χαρακτηριστικό:

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

Αυτό εξακολουθεί να υποστηρίζεται στο ASP.NET 1.0 πυρήνα, αλλά έχει ορισμένα μειονεκτήματα σε σύγκριση με τις πολιτικές εξουσιοδότησης:

-   Προϋποθέτει έναν τύπο συγκεκριμένο απαίτησης. Πολιτικές να ελέγξετε για οποιονδήποτε τύπο διεκδίκηση. Οι ρόλοι είναι μόνο ένας τύπος διεκδίκηση.
-   Το όνομα του ρόλου είναι σχεδιασμένο σε το χαρακτηριστικό. Με τις πολιτικές, η λογική εξουσιοδότησης είναι από ένα σημείο, καθιστώντας πιο εύκολη για ενημέρωση ή ακόμα και φόρτωση από τις ρυθμίσεις παραμέτρων.
-   Πολιτικές Ενεργοποίηση πιο σύνθετες αποφάσεις εξουσιοδότησης (π.χ., age > = 21) που δεν είναι δυνατό να εκφράζονται με ιδιότητα μέλους απλό ρόλο.

## <a name="resource-based-authorization"></a>Πόρων βάσει εξουσιοδότησης

_Πόρων βάσει εξουσιοδότησης_ συμβαίνει κάθε φορά που η άδεια εξαρτάται από έναν συγκεκριμένο πόρο που θα επηρεαστούν από μια λειτουργία. Στην εφαρμογή Tailspin ερευνών, κάθε έρευνα έχει έναν κάτοχο και οι συνεργάτες μηδέν-προς-πολλά.

-   Ο κάτοχος μπορεί να ανάγνωση, ενημέρωση, διαγραφή, δημοσίευση, και κατάργηση δημοσίευσης την έρευνα.
-   Ο κάτοχος μπορεί να εκχωρήσει συμβάλλοντες να την έρευνα.
-   Οι συνεργάτες να διαβάσετε και να ενημερώσετε την έρευνα.

Σημειώστε ότι "κάτοχος" και "Συνεργάτης" δεν είναι εφαρμογή ρόλοι; είναι αποθηκευμένα ανά έρευνας, σε βάση δεδομένων της εφαρμογής. Για να ελέγξετε εάν ένας χρήστης να διαγράψετε μια έρευνα, για παράδειγμα, η εφαρμογή ελέγχει εάν ο χρήστης είναι ο κάτοχος για αυτήν την έρευνα.

Στο ASP.NET 1.0 πυρήνα, υλοποιήστε πόρων βάσει εξουσιοδότησης, που προέρχονται από **AuthorizationHandler** και παράκαμψη τη μέθοδο **χειρισμού** .

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

Παρατηρήστε ότι αυτή η κλάση συνιστάται ιδιαίτερα πληκτρολογήσατε αντικείμενα έρευνας.  Εγγραφείτε τάξη DI κατά την εκκίνηση:

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

Για την εκτέλεση ελέγχων εξουσιοδότησης, χρησιμοποιήστε το περιβάλλον εργασίας **IAuthorizationService** , το οποίο μπορείτε να εισαγάγει στην ελεγκτές σας. Ο ακόλουθος κώδικας ελέγχει εάν ένας χρήστης να διαβάσετε μια έρευνα:

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

Επειδή θα σας μεταφέρουν του ένα `Survey` αντικείμενο, αυτή η κλήση θα ενεργοποιήσει το `SurveyAuthorizationHandler`.

Στον κώδικα εξουσιοδότησης, μια καλή προσέγγιση είναι να συγκεντρωτικών αποτελεσμάτων όλα τα δικαιώματα του χρήστη βάσει ρόλων και βάσει πόρων, στη συνέχεια, ορίστε τη συνάρτηση συγκεντρωτικών αποτελεσμάτων σε σχέση με την επιθυμητή λειτουργία ελέγχου.
Ακολουθεί ένα παράδειγμα από την εφαρμογή έρευνες. Η εφαρμογή ορίζει πολλοί τύποι δικαιωμάτων:

- Διαχειριστής
- Συμβολής
- Εκπαιδευτικών
- Κάτοχος
- Πρόγραμμα ανάγνωσης

Η εφαρμογή ορίζει επίσης ένα σύνολο πιθανές λειτουργίες σε έρευνες:

- Δημιουργία
- Ανάγνωση
- Ενημέρωση
- Διαγραφή
- Δημοσίευση
- Unpublsh

Ο ακόλουθος κώδικας δημιουργεί μια λίστα με δικαιώματα για ένα συγκεκριμένο χρήστη και έρευνα. Παρατηρήστε ότι αυτός ο κώδικας είναι τόσο σε ρόλους εφαρμογής του χρήστη, και τα πεδία κάτοχο/συμβολής της έρευνας.

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] Ανατρέξτε στο θέμα [SurveyAuthorizationHandler.cs].

Σε μια εφαρμογή πολλών μισθωτή, πρέπει να βεβαιωθείτε ότι δικαιώματα δεν "προκαλέσει απώλεια" με τα δεδομένα του μισθωτή. Κατά την εφαρμογή έρευνες, το δικαίωμα συμβολής επιτρέπεται σε μισθωτές &mdash; μπορείτε να αντιστοιχίσετε κάποιον από μια άλλη μισθωτή ως μια contriubutor. Τους άλλους τύπους δικαιωμάτων περιορίζονται στους πόρους που ανήκουν σε μισθωτή του χρήστη. Για να ενεργοποιήσετε αυτήν την απαίτηση, τον κωδικό ελέγχει το Αναγνωριστικό μισθωτή πριν από την εκχώρηση του δικαιώματος του. (Το `TenantId` πεδίων ως εκχωρημένη όταν δημιουργείται η έρευνα.)

Το επόμενο βήμα είναι να ελέγξετε τη λειτουργία (ανάγνωση, ενημέρωση, διαγραφή, κλπ) σε σχέση με τα δικαιώματα. Η εφαρμογή έρευνες υλοποιεί αυτό το βήμα, χρησιμοποιώντας έναν πίνακα αναζήτησης των συναρτήσεων:

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>Επόμενα βήματα

- Διαβάστε το επόμενο άρθρο σε αυτήν τη σειρά: [ασφάλιση ενός στοιχείου διακομιστή web API σε μια εφαρμογή multitenant][web-api]
- Για να μάθετε περισσότερα σχετικά με την άδεια πόρων που βασίζονται στο ASP.NET 1.0 πυρήνα, ανατρέξτε στο θέμα [Εξουσιοδότηση βάσει πόρων][rbac].

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[μέρος μιας σειράς]: guidance-multitenant-identity.md
[Ρόλοι της εφαρμογής]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[υλοποίηση αναφοράς]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.CS]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[Ρύθμιση των παραμέτρων του ελέγχου ταυτότητας ενδιάμεσου λογισμικού]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[δείγμα εφαρμογής]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
