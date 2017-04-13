<properties
    pageTitle="Εφαρμογή Web .NET v2.0 Azure AD | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή Web MVC .NET που πραγματοποιεί εταιρικό ή σχολικό λογαριασμοί και οι χρήστες με δύο τον προσωπικό λογαριασμό Microsoft."
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

# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Προσθήκη εισόδου σε μια εφαρμογή web .NET MVC

Με το τελικό σημείο v2.0, μπορείτε να προσθέσετε γρήγορα τον έλεγχο ταυτότητας στις εφαρμογές web με την υποστήριξη για τους δύο λογαριασμούς Microsoft προσωπικές και λογαριασμούς εργασίας ή του σχολείου.  Στις εφαρμογές web ASP.NET, μπορείτε να εκτελέσετε αυτό με ενδιάμεσο OWIN της Microsoft που περιλαμβάνονται στο διαίρεσης 4,5 .NET Framework.

> [AZURE.NOTE]
    Δεν όλα τα σενάρια Azure Active Directory και δυνατότητες που υποστηρίζονται από το τελικό σημείο v2.0.  Για να καθορίσετε εάν θα πρέπει να χρησιμοποιήσετε το τελικό σημείο v2.0, διαβάστε σχετικά με [τους περιορισμούς v2.0](active-directory-v2-limitations.md).

 Εδώ θα μπορέσουμε να δημιουργήσουμε μια εφαρμογή web που χρησιμοποιεί OWIN για να εισέλθετε στο χρήστη, να εμφανίσετε ορισμένες πληροφορίες σχετικά με το χρήστη και πραγματοποιήστε είσοδο στο χρήστη από την εφαρμογή.
 
 ## <a name="download"></a>Λήψη
Τον κώδικα για αυτό το πρόγραμμα εκμάθησης διατηρείται [σε GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  Για να παρακολουθήσετε κατά μήκος, μπορείτε να [κάνετε λήψη της εφαρμογής σκελετό ως ένα .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) ή κλωνοποίηση το σκελετό:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

Η εφαρμογή ολοκληρωμένων παρέχονται στο τέλος αυτού του προγράμματος εκμάθησης καθώς και.

## <a name="register-an-app"></a>Καταχώρηση εφαρμογής
Δημιουργία νέας εφαρμογής στο [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ή ακολουθήστε αυτά τα [λεπτομερή βήματα](active-directory-v2-app-registration.md).  Βεβαιωθείτε ότι:

- Αντιγραφή προς τα κάτω το **Αναγνωριστικό εφαρμογής** που έχουν εκχωρηθεί σε εφαρμογή σας, θα χρειαστεί το συντομότερο.
- Προσθέστε την πλατφόρμα για την εφαρμογή σας στο **Web** .
- Εισαγάγετε το σωστό **Ανακατεύθυνση URI**. Το uri ανακατεύθυνσης υποδεικνύει στο Azure AD όπου θα πρέπει να κατευθύνεται απαντήσεων ελέγχου ταυτότητας - η προεπιλογή για αυτό το πρόγραμμα εκμάθησης είναι `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Εγκατάσταση και ρύθμιση παραμέτρων ελέγχου ταυτότητας OWIN
Εδώ, θα σας θα ρυθμίσετε τις παραμέτρους του ενδιάμεσο OWIN για να χρησιμοποιήσετε το πρωτόκολλο ελέγχου ταυτότητας OpenID σύνδεση.  OWIN θα χρησιμοποιηθεί για την έκδοση αιτήσεις εισόδου και sign-out, Διαχείριση περιόδου λειτουργίας του χρήστη, και λήψη πληροφοριών σχετικά με το χρήστη, μεταξύ άλλων.

-   Για να ξεκινήσετε, ανοίξτε το `web.config` file στον ριζικό κατάλογο του έργου και πληκτρολογήστε τις τιμές παραμέτρων της εφαρμογής σας σε το `<appSettings>` ενότητας.
    -   Η `ida:ClientId` είναι το **Αναγνωριστικό εφαρμογής** που έχουν εκχωρηθεί σε εφαρμογή σας στην πύλη εγγραφής.
    -   Η `ida:RedirectUri` είναι η **Ανακατεύθυνση Uri** που καταχωρήσατε στην πύλη.

-   Στη συνέχεια, προσθέστε τα πακέτα OWIN ενδιάμεσο NuGet στο έργο χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Προσθέστε ένα "OWIN κλάση εκκίνησης" στο έργο που ονομάζεται `Startup.cs` δεξιό κλικ στο έργο--> **Προσθήκη** --> **Νέο στοιχείο** --> αναζήτηση για "OWIN".  Το ενδιάμεσο OWIN θα ενεργοποιήσει το `Configuration(...)` μέθοδο κατά την εκκίνηση της εφαρμογής σας.
-   Αλλάξτε τη δήλωση κλάσης `public partial class Startup` -Έχουμε ήδη υλοποιήσει μέρος αυτής της κλάσης για εσάς σε ένα άλλο αρχείο.  Στο το `Configuration(...)` μέθοδο, κάνετε μια κλήση σε ConfigureAuth(...) για να ρυθμίσετε τον έλεγχο ταυτότητας για την εφαρμογή web  

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}
```

-   Ανοίξτε το αρχείο `App_Start\Startup.Auth.cs` και να εφαρμόζουν το `ConfigureAuth(...)` μέθοδο.  Οι παράμετροι που παρέχετε στο `OpenIdConnectAuthenticationOptions` θα χρησιμοποιηθεί ως συντεταγμένες για την εφαρμογή σας για να επικοινωνήσετε με το Azure AD.  Θα χρειαστεί επίσης να ρυθμίσετε τον έλεγχο ταυτότητας Cookie - το ενδιάμεσο OpenID σύνδεση χρησιμοποιεί cookies στο παρασκήνιο.

```C#
public void ConfigureAuth(IAppBuilder app)
             {
                     app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

                     app.UseCookieAuthentication(new CookieAuthenticationOptions());

                     app.UseOpenIdConnectAuthentication(
                             new OpenIdConnectAuthenticationOptions
                             {
                                     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                                     // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                     // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                                     ClientId = clientId,
                                     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                                     RedirectUri = redirectUri,
                                     Scope = "openid email profile",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## <a name="send-authentication-requests"></a>Αποστολή προσκλήσεων ελέγχου ταυτότητας
Εφαρμογή σας είναι τώρα έχει ρυθμιστεί σωστά για να επικοινωνήσετε με το τελικό σημείο v2.0 χρησιμοποιεί το πρωτόκολλο ελέγχου ταυτότητας OpenID σύνδεση.  OWIN έχει ληφθεί φροντίσει όλες τις λεπτομέρειες καλή δημιουργία μηνύματα ελέγχου ταυτότητας, επικύρωση διακριτικά από Azure AD και τη διατήρηση περίοδο λειτουργίας χρήστη.  Όλα τα περιεχόμενα παραμένει για να δώσετε στους χρήστες έναν τρόπο είσοδος και έξοδος.

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

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   Στη συνέχεια, ανοίξτε το `Views\Shared\_LoginPartial.cshtml`.  Αυτή είναι η θέση που θα εμφανίζεται στους χρήστες συνδέσεις εισόδου και sign-out της εφαρμογής σας και εκτυπώσετε το όνομα του χρήστη σε μια προβολή.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
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

## <a name="display-user-information"></a>Εμφάνιση πληροφοριών χρήστη
Κατά τον έλεγχο ταυτότητας χρηστών με σύνδεση OpenID, το τελικό σημείο v2.0 επιστρέφει μια id_token στην εφαρμογή που περιέχει [αξιώσεις](active-directory-v2-tokens.md#id_tokens)ή διεκδικήσεων σχετικά με το χρήστη.  Μπορείτε να χρησιμοποιήσετε αυτές τις απαιτήσεις για την εξατομίκευση της εφαρμογής σας:

- Άνοιγμα του `Controllers\HomeController.cs` αρχείου.  Μπορείτε να αποκτήσετε πρόσβαση αξιώσεων του χρήστη στο σας ελεγκτές μέσω του `ClaimsPrincipal.Current` αντικείμενο αρχής ασφαλείας.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## <a name="run"></a>Εκτέλεση

Τέλος, δημιουργία και εκτέλεση της εφαρμογής σας!   Συνδεθείτε με έναν προσωπικό λογαριασμό Microsoft ή έναν εταιρικό ή σχολικό λογαριασμό και Παρατηρήστε πώς ενημερώνεται την ταυτότητα του χρήστη στην επάνω γραμμή περιήγησης.  Τώρα έχετε μια εφαρμογή web ασφαλές χρησιμοποιώντας κλάδο τυπική πρωτόκολλα που μπορούν να ελέγχουν την ταυτότητα χρηστών με τις προσωπικές και εταιρικό/σχολικό τους λογαριασμούς.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) [παρέχεται ως ένα .zip εδώ](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), ή μπορείτε να αντιγράψει το το GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα μπορείτε να μετακινήσετε σε θέματα για προχωρημένους.  Εάν θέλετε, μπορείτε να δοκιμάσετε:

[Ασφαλούς ένα API Web με το το τελικό σημείο v2.0 >>](active-directory-devquickstarts-webapi-dotnet.md)

Για επιπλέον πόρους, ανατρέξτε στο θέμα:
- [Στον οδηγό για προγραμματιστές v2.0 >>](active-directory-appmodel-v2-overview.md)
- [Ετικέτα StackOverflow "azure active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Λήψη ενημερώσεων ασφαλείας για τα προϊόντα μας

Συνιστάται να λαμβάνετε ειδοποιήσεις όταν προκύπτουν περιστατικά ασφαλείας, αν επισκεφθείτε την τοποθεσία [αυτήν τη σελίδα](https://technet.microsoft.com/security/dd252948) και εγγραφή συμβουλευτική ειδοποιήσεων ασφαλείας.
