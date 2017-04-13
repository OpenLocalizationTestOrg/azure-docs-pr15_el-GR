<properties
    pageTitle="Γρήγορα αποτελέσματα .NET Azure AD | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή Web MVC .NET που ενσωματώνεται στο Azure AD για είσοδος."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="aspnet-web-app-sign-in--sign-out-with-azure-ad"></a>Εισόδου στην εφαρμογή ASP.NET Web & έξοδος με Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD καθιστά απλή και άμεση για να μεταβιβάζουν διαχείρισης των ταυτοτήτων της εφαρμογής web σας, παρέχοντας μία είσοδος και sign-out με λίγες μόνο γραμμές του κώδικα.  Στις εφαρμογές web Asp.NET, μπορείτε να εκτελέσετε αυτό με εφαρμογή του Microsoft από την Κοινότητα βάσει OWIN ενδιάμεσου λογισμικού περιλαμβάνονται στο διαίρεσης 4,5 .NET Framework.  Εδώ θα χρησιμοποιήσουμε OWIN για να:
-   Πραγματοποιήστε είσοδο στο χρήστη στην εφαρμογή χρησιμοποιώντας Azure AD ως την υπηρεσία παροχής ταυτότητας.
-   Εμφάνιση ορισμένες πληροφορίες σχετικά με το χρήστη.
-   Πραγματοποιήστε είσοδο στο χρήστη από την εφαρμογή.

Για να το κάνετε αυτό, θα πρέπει να:

1. Καταχωρήστε μια εφαρμογή του Azure AD
2. Ρύθμιση της εφαρμογής σας για να χρησιμοποιήσετε τη διαδικασία ελέγχου ταυτότητας OWIN.
3. Χρησιμοποιήστε OWIN για την έκδοση αιτήσεις εισόδου και sign-out να Azure AD.
4. Εκτυπώστε δεδομένα σχετικά με το χρήστη.

Για να ξεκινήσετε, [κάντε λήψη του σκελετό εφαρμογής](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) ή [λήψη ολοκληρωμένο δείγμα](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  Θα χρειαστεί επίσης ένα μισθωτή του Azure AD στην οποία θέλετε να καταχωρήσετε την εφαρμογή σας.  Εάν δεν έχετε ήδη, [Μάθετε πώς μπορείτε να αποκτήσετε](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>*1. καταχώρηση μιας εφαρμογής με το Azure AD*
Για να ενεργοποιήσετε την εφαρμογή σας για τον έλεγχο ταυτότητας χρηστών, θα πρέπει πρώτα να καταχωρήσετε μια νέα εφαρμογή στο μισθωτή σας.

- Είσοδος στην πύλη του Azure διαχείρισης.
- Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην **Υπηρεσία καταλόγου Active Directory**.
- Επιλέξτε το μισθωτή όπου θέλετε να καταχωρήσετε την εφαρμογή.
- Κάντε κλικ στην καρτέλα **εφαρμογές** και κάντε κλικ στην επιλογή Προσθήκη στο το σχέδιο κάτω.
- Ακολουθήστε τις οδηγίες και δημιουργήστε μια νέα **εφαρμογή Web ή/και WebAPI**.
    - Το **όνομα** της εφαρμογής θα περιγράφει την εφαρμογή σας στους τελικούς χρήστες
    -   Το **Σύμβολο στη διεύθυνση URL** είναι τη βασική διεύθυνση URL της εφαρμογής.  Το σκελετό προεπιλογή είναι `https://localhost:44320/`.
    - Το **URI Αναγνωριστικό εφαρμογής** είναι ένα μοναδικό αναγνωριστικό για την εφαρμογή σας.  Η σύμβαση είναι να χρησιμοποιήσετε `https://<tenant-domain>/<app-name>`, π.χ.`https://contoso.onmicrosoft.com/my-first-aad-app`
- Όταν ολοκληρώσετε την καταχώρηση, AAD θα εκχωρήσετε την εφαρμογή σας ένα αναγνωριστικό μοναδικό προγράμματος-πελάτη.  Θα χρειαστείτε αυτή την τιμή στις επόμενες ενότητες, επομένως, αντιγράψτε την από την καρτέλα ρύθμιση παραμέτρων.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. ρύθμιση της εφαρμογής σας για να χρησιμοποιήσετε τη διαδικασία ελέγχου ταυτότητας OWIN*
Εδώ, θα σας θα ρυθμίσετε τις παραμέτρους του ενδιάμεσο OWIN για να χρησιμοποιήσετε το πρωτόκολλο ελέγχου ταυτότητας OpenID σύνδεση.  OWIN θα χρησιμοποιηθεί για αιτήσεις εισόδου και sign-out θεμάτων, να διαχειριστείτε την περίοδο λειτουργίας του χρήστη και λήψη πληροφοριών σχετικά με το χρήστη, μεταξύ άλλων.

-   Για να ξεκινήσετε, προσθέστε τα πακέτα OWIN ενδιάμεσο NuGet στο έργο χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Προσθέστε μια κλάση OWIN εκκίνησης στο έργο που ονομάζεται `Startup.cs` δεξιό κλικ στο έργο--> **Προσθήκη** --> **Νέο στοιχείο** --> αναζήτηση για "OWIN".  Το ενδιάμεσο OWIN θα ενεργοποιήσει το `Configuration(...)` μέθοδο κατά την εκκίνηση της εφαρμογής σας.
-   Αλλάξτε τη δήλωση κλάσης `public partial class Startup` -Έχουμε ήδη υλοποιήσει μέρος αυτής της κλάσης για εσάς σε ένα άλλο αρχείο.  Στο το `Configuration(...)` μέθοδο, κάνετε μια κλήση σε ConfgureAuth(...) για να ρυθμίσετε τον έλεγχο ταυτότητας για την εφαρμογή web  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Ανοίξτε το αρχείο `App_Start\Startup.Auth.cs` και να εφαρμόζουν το `ConfigureAuth(...)` μέθοδο.  Οι παράμετροι που παρέχετε στο `OpenIDConnectAuthenticationOptions` θα χρησιμοποιηθεί ως συντεταγμένες για την εφαρμογή σας για να επικοινωνήσετε με το Azure AD.  Θα χρειαστεί επίσης να ρυθμίσετε τον έλεγχο ταυτότητας Cookie - το ενδιάμεσο OpenID σύνδεση χρησιμοποιεί cookies στο παρασκήνιο.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ClientId = clientId,
            Authority = authority,
            PostLogoutRedirectUri = postLogoutRedirectUri,
        });
}
```

-   Τέλος, ανοίξτε το `web.config` αρχείων στον ριζικό κατάλογο του έργου και πληκτρολογήστε τις τιμές παραμέτρων στην το `<appSettings>` ενότητας.
    -   Την εφαρμογή `ida:ClientId` είναι το Guid που αντιγράψατε από την πύλη Azure στο βήμα 1.
    -   Η `ida:Tenant` είναι το όνομα του μισθωτή του Azure AD, π.χ. "contoso.onmicrosoft.com".
    -   Σας `ida:PostLogoutRedirectUri` υποδεικνύει στο Azure AD όπου θα πρέπει να ανακατευθυνθεί ενός χρήστη μετά την ολοκλήρωση της αίτησης sign-out με επιτυχία.

## <a name="3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>*3. Χρησιμοποιήστε OWIN για την έκδοση αιτήσεις εισόδου και sign-out να Azure AD*
Εφαρμογή σας είναι τώρα έχει ρυθμιστεί σωστά για να επικοινωνήσετε με το Azure AD χρησιμοποιεί το πρωτόκολλο ελέγχου ταυτότητας OpenID σύνδεση.  OWIN έχει ληφθεί φροντίσει όλες τις λεπτομέρειες καλή δημιουργία μηνύματα ελέγχου ταυτότητας, επικύρωση διακριτικά από Azure AD και τη διατήρηση περίοδο λειτουργίας χρήστη.  Όλα τα περιεχόμενα παραμένει για να δώσετε στους χρήστες έναν τρόπο είσοδος και έξοδος.

- Μπορείτε να χρησιμοποιήσετε εγκρίνετε ετικέτες στο σας ελεγκτές ώστε να απαιτεί αυτός ο χρήστης που πραγματοποιεί είσοδο πριν αποκτήσετε πρόσβαση σε συγκεκριμένη σελίδα.  Άνοιγμα `Controllers\HomeController.cs`, και προσθέστε το `[Authorize]` ετικέτα στον ελεγκτή για.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   Μπορείτε επίσης να χρησιμοποιήσετε OWIN για την έκδοση απευθείας αιτήσεις ελέγχου ταυτότητας από τον κωδικό.  Άνοιγμα `Controllers\AccountController.cs`.  Στο SignIn() και SignOut() ενέργειες, θεμάτων σύνδεση OpenID πρόκληση και sign-out αιτήσεις, αντίστοιχα.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(
        OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

-   Στη συνέχεια, ανοίξτε το `Views\Shared\_LoginPartial.cshtml`.  Αυτή είναι η θέση που θα εμφανίζεται στους χρήστες συνδέσεις εισόδου και sign-out της εφαρμογής σας και εκτυπώσετε το όνομα του χρήστη σε μια προβολή.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## <a name="4--display-user-information"></a>*4. πληροφορίες χρήστη εμφάνιση*
Κατά τον έλεγχο ταυτότητας χρηστών με σύνδεση OpenID, Azure AD επιστρέφει μια id_token στην εφαρμογή που περιέχει "αξιώσεων" ή διεκδικήσεων σχετικά με το χρήστη.  Μπορείτε να χρησιμοποιήσετε αυτές τις απαιτήσεις για την εξατομίκευση της εφαρμογής σας:

- Άνοιγμα του `Controllers\HomeController.cs` αρχείου.  Μπορείτε να αποκτήσετε πρόσβαση αξιώσεων του χρήστη στο σας ελεγκτές μέσω του `ClaimsPrincipal.Current` αντικείμενο αρχής ασφαλείας.

```C#
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
    ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
    ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
    ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

    return View();
}
```

Τέλος, δημιουργία και εκτέλεση της εφαρμογής σας!  Εάν δεν το έχετε κάνει ήδη, τώρα είναι η ώρα για να δημιουργήσετε έναν νέο χρήστη στο μισθωτή σας με μια *. τομέα onmicrosoft.com.  Πραγματοποιήστε είσοδο με αυτόν το χρήστη και Παρατηρήστε πώς απεικονίζεται την ταυτότητα του χρήστη στην επάνω γραμμή περιήγησης.  Πραγματοποιήστε έξοδο και συνδεθείτε ξανά με έναν άλλο χρήστη στο μισθωτή σας.  Εάν νιώθετε ιδιαίτερα φιλόδοξων, καταχώρηση και να εκτελέσετε μια άλλη παρουσία αυτού του εφαρμογής (με το δικό της clientId) και δείτε ανατρέξτε στο θέμα καθολικής σύνδεσης σε στην πράξη.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) που [παρέχεται εδώ](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  

Τώρα μπορείτε να μετακινήσετε σε θέματα για προχωρημένους.  Εάν θέλετε, μπορείτε να δοκιμάσετε:

[Ασφαλούς Web API με Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
