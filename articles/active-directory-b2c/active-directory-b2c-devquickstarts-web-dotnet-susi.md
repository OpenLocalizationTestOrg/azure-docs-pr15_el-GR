<properties
    pageTitle="B2C καταλόγου Azure Active Directory | Microsoft Azure"
    description="Πώς να δημιουργείτε μια εφαρμογή web που έχει εγγραφής, είσοδος και επαναφορά χρησιμοποιώντας Azure Active Directory B2C κωδικού πρόσβασης."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-sign-up--sign-in-in-a-aspnet-web-app"></a>Azure AD B2C: Εγγραφής & είσοδο σε μια εφαρμογή ASP.NET Web

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Με τη χρήση B2C Azure Active Directory (Azure AD), μπορείτε να προσθέσετε δυνατότητες διαχείρισης ισχυρή ταυτότητας από το χρήστη σε εφαρμογή web με λίγα βήματα σύντομο. Σε αυτό το άρθρο θα ασχολείται με τον τρόπο για να δημιουργήσετε μια εφαρμογή web ASP.NET που περιλαμβάνει το χρήστη να εγγραφής, είσοδος και επαναφορά του κωδικού πρόσβασης. Η εφαρμογή θα περιλαμβάνει υποστήριξη εγγραφής και είσοδος με χρήση του ηλεκτρονικού ταχυδρομείου ή ένα όνομα χρήστη, καθώς και με τη χρήση λογαριασμοί κοινωνικών όπως το Facebook και το Google.

Αυτό το πρόγραμμα εκμάθησης διαφέρει από [την άλλη εκμάθηση web .NET](active-directory-b2c-devquickstarts-web-dotnet.md) που χρησιμοποιεί μια [εγγραφή ή πραγματοποιήστε είσοδο στο πολιτικής](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy) για την παροχή δήλωση χρηστών και είσοδος με χρήση ενός κουμπιού, αντί για δύο (μία για εγγραφής και μία για είσοδο).  Σε ένα nutshell, εγγραφή ή εισόδου στην πολιτική επιτρέπει στους χρήστες να πραγματοποιούν είσοδο με έναν υπάρχοντα λογαριασμό εάν έχουν ένα ή δημιουργήστε ένα νέο, εάν πρόκειται για την πρώτη φορά χρησιμοποιώντας την εφαρμογή.

## <a name="get-an-azure-ad-b2c-directory"></a>Λήψη ενός καταλόγου Azure AD B2C

Μπορείτε να χρησιμοποιήσετε Azure AD B2C, πρέπει να δημιουργήσετε έναν κατάλογο, ή να μισθωτή. Ένας κατάλογος είναι ένα κοντέινερ για όλους τους χρήστες σας, εφαρμογές, ομάδες και πολλά άλλα.  Εάν δεν έχετε ήδη, [Δημιουργήστε έναν κατάλογο B2C](active-directory-b2c-get-started.md) πριν να συνεχίσετε με αυτόν τον οδηγό.

## <a name="create-an-application"></a>Δημιουργία μιας εφαρμογής

Στη συνέχεια, πρέπει να δημιουργήσετε μια εφαρμογή στον κατάλογό σας B2C. Αυτό σας δίνει Azure AD πληροφορίες που χρειάζονται για την ασφαλή επικοινωνία με την εφαρμογή σας. Για να δημιουργήσετε μια εφαρμογή, ακολουθήστε [αυτές τις οδηγίες](active-directory-b2c-app-registration.md).  Βεβαιωθείτε ότι:

- Συμπεριλάβετε μια **εφαρμογή web/web API** στην εφαρμογή.
- Πληκτρολογήστε `https://localhost:44316/` ως μια **ανακατεύθυνση URI**. Είναι η προεπιλεγμένη διεύθυνση URL για αυτό το δείγμα κώδικα.
- Αντιγραφή προς τα κάτω το **Αναγνωριστικό εφαρμογής** που έχει αντιστοιχιστεί σε εφαρμογή σας.  Θα το χρειαστείτε αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Δημιουργία πολιτικών σας

Στο Azure AD B2C, κάθε εμπειρία χρήστη ορίζεται από μια [πολιτική](active-directory-b2c-reference-policies.md). Αυτό το δείγμα κώδικα περιέχει δύο εμπειρίες ταυτότητας: **εγγραφής / εισόδου**και να **επαναφέρετε τον κωδικό πρόσβασης**.  Πρέπει να δημιουργήσετε μία πολιτική κάθε τύπο, όπως περιγράφεται στο [άρθρο αναφοράς πολιτικής](active-directory-b2c-reference-policies.md). Όταν δημιουργείτε τις δύο πολιτικές, βεβαιωθείτε ότι:

- Επιλέξτε **Αναγνωριστικό χρήστη εγγραφής** ή **ηλεκτρονικού ταχυδρομείου εγγραφής** σε το blade υπηρεσίες παροχής ταυτότητας.
- Επιλέξτε το **εμφανιζόμενο όνομα** και άλλα χαρακτηριστικά εγγραφής στην πολιτική σας εγγραφής & εισόδου.
- Επιλέξτε το αίτημα **εμφανιζόμενο όνομα** ως εφαρμογή διεκδίκηση σε κάθε πολιτική. Μπορείτε να επιλέξετε καθώς και άλλες αξιώσεων.
- Αντιγράψτε το **όνομα** κάθε πολιτική μετά τη δημιουργία του. Θα χρειαστείτε αυτά τα ονόματα πολιτικής αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Αφού δημιουργήσετε δύο πολιτικές σας, είστε έτοιμοι να δημιουργήσετε την εφαρμογή σας.

## <a name="download-the-code-and-configure-authentication"></a>Κάντε λήψη του κώδικα και ρύθμιση παραμέτρων ελέγχου ταυτότητας

Ο κωδικός για αυτό δείγμα [διατηρείται σε GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI). Για να δημιουργήσετε το δείγμα καθώς εργάζεστε, μπορείτε να [κάνετε λήψη του σκελετό έργου ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip). Μπορείτε επίσης να αντιγράψετε το σκελετό:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

Το ολοκληρωμένο δείγμα είναι επίσης [διαθέσιμο ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) ή σε το `complete` κλάδο της ίδιο αποθετήριο.

Μετά τη λήψη του δείγματος κώδικα, ανοίξτε το αρχείο .sln Visual Studio για να ξεκινήσετε.

Εφαρμογή σας επικοινωνεί με Azure AD B2C στέλνοντας αιτήσεις ελέγχου ταυτότητας HTTP που καθορίζουν την πολιτική θέλει να εκτελέσει ως μέρος της αίτησης. Για τις εφαρμογές web .NET, μπορείτε να χρησιμοποιήσετε βιβλιοθήκη OWIN της Microsoft για να στείλετε σύνδεση OpenID αιτήσεις ελέγχου ταυτότητας, εκτέλεση πολιτικές, Διαχείριση περιόδων λειτουργίας χρηστών και πολλά άλλα.

Για να ξεκινήσετε, προσθέστε τα πακέτα OWIN ενδιάμεσο NuGet στο έργο χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου του Visual Studio.

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Install-Package System.IdentityModel.Tokens.Jwt
```

Στη συνέχεια, ανοίξτε το `web.config` αρχείο στον ριζικό κατάλογο του έργου και πληκτρολογήστε τις τιμές παραμέτρων της εφαρμογής σας σε το `<appSettings>` ενότητα, αντικαθιστώντας τις τιμές κάτω από το στοιχείο με το δικό σας.  Μπορείτε να αφήσετε το `ida:RedirectUri` και το `ida:AadInstance` τιμές όπως, που δεν έχουν αλλάξει.

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Στη συνέχεια, προσθέστε μια OWIN κλάση εκκίνησης για το έργο που ονομάζεται `Startup.cs`. Κάντε δεξί κλικ στο έργο, επιλέξτε **Προσθήκη** και το **Νέο στοιχείο**και, στη συνέχεια, αναζητήστε το "OWIN." Αλλάξτε τη δήλωση κλάσης `public partial class Startup`. Θα σας υλοποιηθεί τμήμα αυτής της κλάσης για εσάς σε ένα άλλο αρχείο. Το ενδιάμεσο OWIN θα ενεργοποιήσει το `Configuration(...)` μέθοδο κατά την εκκίνηση της εφαρμογής σας. Σε αυτήν τη μέθοδο, πραγματοποίηση κλήσης για να `ConfigureAuth(...)`, όπου ρυθμίζετε τον έλεγχο ταυτότητας για την εφαρμογή σας.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Ανοίξτε το αρχείο `App_Start\Startup.Auth.cs` και να εφαρμόζουν το `ConfigureAuth(...)` μέθοδο.  Οι παράμετροι που παρέχετε στο `OpenIdConnectAuthenticationOptions` χρησιμοποιηθεί ως συντεταγμένες για την εφαρμογή σας για να επικοινωνήσετε με το Azure AD. Πρέπει επίσης να ρυθμίσετε τον έλεγχο ταυτότητας cookie. Η σύνδεση OpenID ενδιάμεσο χρησιμοποιεί cookies για να διατηρήσετε τις περιόδους λειτουργίας χρήστη, μεταξύ άλλων.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(PasswordResetPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SusiPolicyId));

    }

    private Task OnSecurityTokenValidated(SecurityTokenValidatedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        // If you wanted to keep some local state in the app (like a db of signed up users),
        // you could use this notification to create the user record if it does not already
        // exist.

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
        return new OpenIdConnectAuthenticationOptions
        {
            // For each policy, give OWIN the policy-specific metadata address, and
            // set the authentication type to the id of the policy
            MetadataAddress = String.Format(aadInstance, tenant, policy),
            AuthenticationType = policy,

            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = AuthenticationFailed,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
...
```

## <a name="send-authentication-requests-to-azure-ad"></a>Αποστολή προσκλήσεων ελέγχου ταυτότητας σε Azure AD
Εφαρμογή σας είναι τώρα έχει ρυθμιστεί σωστά για να επικοινωνήσετε με Azure AD B2C, χρησιμοποιώντας το πρωτόκολλο ελέγχου ταυτότητας OpenID σύνδεση.  OWIN έχει ληφθεί φροντίσει όλων των λεπτομερειών δημιουργία μηνύματα ελέγχου ταυτότητας, επικύρωση διακριτικά από Azure AD και τη διατήρηση περίοδο λειτουργίας χρήστη.  Όλες τις δυνατότητες που είναι παραμένει για να ξεκινήσετε ροής κάθε χρήστη.

Όταν ο χρήστης επιλέγει **σύνδεσης** ή **Ξεχάσατε τον κωδικό πρόσβασής σας;** στην εφαρμογή web, ενεργοποιείται η συσχετισμένη ενέργεια σε `Controllers\AccountController.cs`. Σε κάθε περίπτωση, μπορείτε να χρησιμοποιήσετε ενσωματωμένα OWIN μεθόδους για να ενεργοποιήσετε το σωστό πολιτικής:

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SusiPolicyId);
    }
}

public void ResetPassword()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties() { RedirectUri = "/" }, Startup.PasswordResetPolicyId);
    }
}
```

Κατά την εκτέλεση της διαδικασίας Πραγματοποιήστε είσοδο προς τα επάνω ή πραγματοποιήστε είσοδο στην πολιτική, ο χρήστης έχει τη δυνατότητα να κάνετε κλικ σε μια **Ξεχάσατε τον κωδικό πρόσβασής σας;** σύνδεση.  Σε αυτό το συμβάν, Azure AD B2C θα σας στείλει ένα συγκεκριμένο μήνυμα λάθους που δηλώνει ότι αυτό θα πρέπει να εκτελούνται έναν κωδικό πρόσβασης της εφαρμογής σας Επαναφορά πολιτικής.  Μπορείτε να καταγράψετε αυτό το σφάλμα στην `Startup.Auth.cs` χρησιμοποιώντας το `AuthenticationFailed` ειδοποίησης:

```C#
// Used for avoiding yellow-screen-of-death TODO
private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        // If the user canceled the sign in, redirect back to the home page
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```


Εκτός από την κλήση ρητά μια πολιτική, μπορείτε να χρησιμοποιήσετε ένα `[Authorize]` ετικέτα στο σας ελεγκτές που θα εκτελεστεί μια πολιτική, εάν ο χρήστης δεν είναι συνδεδεμένος στο. Άνοιγμα `Controllers\HomeController.cs` και προσθέστε το `[Authorize]` ετικέτα στον ελεγκτή αξιώσεων.  OWIN θα επιλέξετε το τελευταίο πολιτική έχει ρυθμιστεί όταν το `[Authorize]` της αντίστοιχης ετικέτα.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

Μπορείτε επίσης να χρησιμοποιήσετε OWIN να αποσυνδεθείτε από το χρήστη από την εφαρμογή. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Εμφάνιση πληροφοριών χρήστη
Όταν που ελέγχουν την ταυτότητα χρηστών, χρησιμοποιώντας σύνδεση OpenID, Azure AD επιστρέφει ένα Αναγνωριστικό διακριτικό στην εφαρμογή που περιέχει **αξιώσεων**. Αυτά είναι διεκδικήσεων σχετικά με το χρήστη. Μπορείτε να χρησιμοποιήσετε αξιώσεων για να προσαρμόσετε την εφαρμογή σας.  

Άνοιγμα του `Controllers\HomeController.cs` αρχείου. Μπορείτε να αποκτήσετε πρόσβαση χρήστη αξιώσεων στο σας ελεγκτές μέσω του `ClaimsPrincipal.Current` αντικείμενο αρχής ασφαλείας.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Μπορείτε να αποκτήσετε πρόσβαση σε κάθε απαίτηση που λαμβάνει την εφαρμογή σας με τον ίδιο τρόπο.  Μια λίστα με όλα τα αξιώσεων λαμβάνει την εφαρμογή είναι διαθέσιμη για εσάς στη σελίδα **αξιώσεων** .

## <a name="run-the-sample-app"></a>Εκτελέστε την εφαρμογή δείγματος

Τέλος, μπορείτε να δημιουργήσετε και να εκτελέσετε την εφαρμογή σας. Εγγραφή για την εφαρμογή, χρησιμοποιώντας ένα μήνυμα ηλεκτρονικού ταχυδρομείου διεύθυνση ή το όνομα χρήστη. Αποσυνδεθείτε και συνδεθείτε ξανά με το ίδιο χρήστη. Επεξεργασία του προφίλ του χρήστη. Πραγματοποιήστε έξοδο και να εγγραφούν ως έναν άλλο χρήστη. Σημειώστε ότι οι πληροφορίες που εμφανίζονται στην καρτέλα **αξιώσεων** αντιστοιχεί με τις πληροφορίες που ρυθμίσατε στο πολιτικές σας.

## <a name="add-social-idps"></a>Προσθήκη κοινωνικού IDPs

Προς το παρόν, η εφαρμογή υποστηρίζει μόνο χρήστη εγγραφής και είσοδος χρησιμοποιώντας **Τοπικοί λογαριασμοί**. Αυτές είναι οι λογαριασμοί που είναι αποθηκευμένα στον κατάλογό σας B2C που χρησιμοποιούν το όνομα χρήστη και τον κωδικό πρόσβασης. Με τη χρήση Azure AD B2C, μπορείτε να προσθέσετε υποστήριξη για άλλες **υπηρεσίες παροχής ταυτότητας** (IDPs) χωρίς να αλλάξετε οποιαδήποτε από τον κωδικό.

Για να προσθέσετε κοινωνικών IDPs την εφαρμογή σας, ξεκινήστε ακολουθώντας τις αναλυτικές οδηγίες σε αυτά τα άρθρα. Για κάθε IDP που θέλετε για την υποστήριξη, πρέπει να καταχωρήσετε μια εφαρμογή στο σύστημα που και να αποκτήσετε ένα αναγνωριστικό υπολογιστή-πελάτη.

- [Ρύθμιση του Facebook ως μια IDP](active-directory-b2c-setup-fb-app.md)
- [Ρύθμιση του Google ως μια IDP](active-directory-b2c-setup-goog-app.md)
- [Ρύθμιση του Amazon ως μια IDP](active-directory-b2c-setup-amzn-app.md)
- [Ρύθμιση του LinkedIn ως μια IDP](active-directory-b2c-setup-li-app.md)

Αφού προσθέσετε τις υπηρεσίες παροχής ταυτότητας στον κατάλογό σας B2C, πρέπει να επεξεργαστείτε κάθε μία από τις τρεις πολιτικές για να συμπεριλάβετε τα νέα IDPs, όπως περιγράφεται στο [άρθρο αναφοράς πολιτικής](active-directory-b2c-reference-policies.md). Αφού αποθηκεύσετε τις πολιτικές σας, εκτελέστε ξανά την εφαρμογή.  Θα πρέπει να δείτε το νέο IDPs προστεθεί ως είσοδο και να αντιμετωπίσει επιλογές εγγραφής σε κάθε έναν από την ταυτότητά σας.

Μπορείτε να πειραματιστείτε με τις πολιτικές και να παρατηρήσετε το αποτέλεσμα στην εφαρμογή σας δείγμα. Προσθήκη ή κατάργηση IDPs, χειριστείτε αξιώσεων εφαρμογή ή αλλαγή χαρακτηριστικών εγγραφής. Πειραματιστείτε μέχρι να μπορείτε να δείτε πώς πολιτικών, αιτήσεις για έλεγχο ταυτότητας και OWIN να συνδέσετε.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) που [παρέχεται ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip). Μπορείτε, επίσης, να την αντιγράψετε από GitHub:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
