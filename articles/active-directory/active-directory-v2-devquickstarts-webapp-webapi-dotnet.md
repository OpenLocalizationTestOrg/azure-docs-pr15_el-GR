<properties
    pageTitle="Εφαρμογή Web .NET v2.0 Azure AD | Microsoft Azure"
    description="Πώς να δημιουργείτε μια εφαρμογή Web MVC .NET κλήσεις περιεχομένου web των υπηρεσιών με χρήση προσωπικών λογαριασμών της Microsoft και εργασίας ή σχολείου λογαριασμούς για την είσοδο."
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
    ms.date="10/27/2016"
    ms.author="dastrock"/>

# <a name="calling-a-web-api-from-a-net-web-app"></a>Κλήση web API από μια εφαρμογή web .NET

Με το τελικό σημείο v2.0, μπορείτε γρήγορα να προσθέσετε τον έλεγχο ταυτότητας στις εφαρμογές web και να web APIs με λογαριασμούς εργασίας ή του σχολείου και υποστήριξη για δύο προσωπικοί λογαριασμοί Microsoft.  Εδώ, θα μπορέσουμε να δημιουργήσουμε μια εφαρμογή web MVC που πραγματοποιεί χρήστες χρησιμοποιώντας τη σύνδεση OpenID, με βοήθεια από το ενδιάμεσο OWIN της Microsoft.  Στην εφαρμογή web θα λάβετε διακριτικά πρόσβασης OAuth 2.0 για δημιουργία api ασφαλές από το διακριτικό 2.0 που επιτρέπει σε περιεχόμενο web, να διαβάσετε και να διαγράψετε σε μια δεδομένη χρήστη "λίστα εκκρεμών εργασιών".

Αυτό το πρόγραμμα εκμάθησης εστιάζει κυρίως σχετικά με τη χρήση MSAL να αποκτήσετε και να χρησιμοποιήσετε διακριτικά πρόσβασης σε μια εφαρμογή web, που περιγράφονται σε πλήρη [εδώ](active-directory-v2-flows.md#web-apps).  Ως τις προϋποθέσεις, μπορεί να θέλετε να πρώτα να μάθετε πώς μπορείτε να [προσθέσετε βασικές εισόδου για μια εφαρμογή web](active-directory-v2-devquickstarts-dotnet-web.md) ή πώς να [προστατεύσετε κατάλληλα web API](active-directory-v2-devquickstarts-dotnet-api.md).

> [AZURE.NOTE]
    Δεν όλα τα σενάρια Azure Active Directory και δυνατότητες που υποστηρίζονται από το τελικό σημείο v2.0.  Για να καθορίσετε εάν θα πρέπει να χρησιμοποιήσετε το τελικό σημείο v2.0, διαβάστε σχετικά με [τους περιορισμούς v2.0](active-directory-v2-limitations.md).

## <a name="download-sample-code"></a>Λήψη δείγματος κώδικα

Τον κώδικα για αυτό το πρόγραμμα εκμάθησης διατηρείται [σε GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  Για να παρακολουθήσετε κατά μήκος, μπορείτε να [κάνετε λήψη της εφαρμογής σκελετό ως ένα .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) ή κλωνοποίηση το σκελετό:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Εναλλακτικά, μπορείτε να [κάνετε λήψη της εφαρμογής ολοκληρωμένη ως ένα .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) ή κλωνοποίηση η ολοκληρωμένη εφαρμογή:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Καταχώρηση εφαρμογής
Δημιουργία νέας εφαρμογής στο [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ή ακολουθήστε αυτά τα [λεπτομερή βήματα](active-directory-v2-app-registration.md).  Βεβαιωθείτε ότι:

- Αντιγραφή προς τα κάτω το **Αναγνωριστικό εφαρμογής** που έχουν εκχωρηθεί σε εφαρμογή σας, θα χρειαστεί το συντομότερο.
- Δημιουργήστε μια **Εφαρμογή μυστικό** του τύπου **τον κωδικό πρόσβασης** και αντιγράψτε προς τα κάτω την τιμή για αργότερα
- Προσθέστε την πλατφόρμα για την εφαρμογή σας στο **Web** .
- Εισαγάγετε το σωστό **Ανακατεύθυνση URI**. Το uri ανακατεύθυνσης υποδεικνύει στο Azure AD όπου θα πρέπει να κατευθύνεται απαντήσεων ελέγχου ταυτότητας - η προεπιλογή για αυτό το πρόγραμμα εκμάθησης είναι `https://localhost:44326/`.


## <a name="install-owin"></a>Εγκατάσταση OWIN
Προσθήκη των πακέτων OWIN το ενδιάμεσο NuGet για να το `TodoList-WebApp` έργο που χρησιμοποιεί την Κονσόλα διαχείρισης πακέτου.  Το ενδιάμεσο OWIN θα χρησιμοποιηθεί για αιτήσεις εισόδου και sign-out θεμάτων, Διαχείριση περιόδου λειτουργίας του χρήστη και λήψη πληροφοριών σχετικά με το χρήστη, μεταξύ άλλων.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Είσοδος στο χρήστη
Τώρα μπορείτε να ρυθμίσετε τις το ενδιάμεσο OWIN για να χρησιμοποιήσετε το [πρωτόκολλο OpenID σύνδεση ελέγχου ταυτότητας](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  

-   Άνοιγμα του `web.config` αρχείο στη ρίζα της το `TodoList-WebApp` του project και πληκτρολογήστε τις τιμές παραμέτρων της εφαρμογής σας σε το `<appSettings>` ενότητας.
    -   Η `ida:ClientId` είναι το **Αναγνωριστικό εφαρμογής** που έχουν εκχωρηθεί σε εφαρμογή σας στην πύλη εγγραφής.
    - Η `ida:ClientSecret` είναι το **Μυστικό εφαρμογής** που δημιουργήσατε στην πύλη εγγραφής.
    -   Η `ida:RedirectUri` είναι η **Ανακατεύθυνση Uri** που καταχωρήσατε στην πύλη.
- Άνοιγμα του `web.config` αρχείο στη ρίζα της το `TodoList-Service` του project και να αντικαταστήσετε το `ida:Audience` με το ίδιο **Αναγνωριστικό εφαρμογής** όπως παραπάνω.


- Ανοίξτε το αρχείο `App_Start\Startup.Auth.cs` και προσθήκη `using` προτάσεις για τις βιβλιοθήκες από επάνω.
- Στο ίδιο αρχείο, υλοποιήσετε το `ConfigureAuth(...)` μέθοδο.  Οι παράμετροι που παρέχετε στο `OpenIDConnectAuthenticationOptions` θα χρησιμοποιηθεί ως συντεταγμένες για την εφαρμογή σας για να επικοινωνήσετε με το Azure AD.

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
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Χρησιμοποιήστε MSAL για να λάβετε διακριτικά πρόσβασης
Στο το `AuthorizationCodeReceived` ειδοποίηση, θέλουμε να χρησιμοποιήσετε [διακριτικό 2.0 σε συνδυασμό με σύνδεση OpenID](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) για να εξαργυρώσετε το authorization_code για ένα διακριτικό πρόσβασης στην υπηρεσία λίστα εκκρεμών εργασιών.  MSAL να διευκολύνετε αυτήν τη διαδικασία για εσάς:

- Πρώτα, εγκαταστήστε την έκδοση preview του MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```
- Και να προσθέσετε ένα άλλο `using` πρόταση για να την `App_Start\Startup.Auth.cs` αρχείου για MSAL.
- Προσθέστε τώρα μια νέα μέθοδο, το `OnAuthorizationCodeReceived` πρόγραμμα χειρισμού συμβάντων.  Αυτό το πρόγραμμα χειρισμού θα χρησιμοποιήσει MSAL για να αποκτήσετε ένα διακριτικό πρόσβασης για το API λίστα εκκρεμών εργασιών και θα αποθηκεύσει το διακριτικό στο cache διακριτικού του MSAL για αργότερα:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

- Στις εφαρμογές web, MSAL έχει μια επεκτάσιμη διακριτικού cache που μπορούν να χρησιμοποιηθούν για την αποθήκευση διακριτικά.  Αυτό το δείγμα υλοποιεί το `NaiveSessionCache` που χρησιμοποιεί χώρο αποθήκευσης περιόδου λειτουργίας http για να τα διακριτικά cache.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Καλέστε το API Web
Τώρα είναι ώρα να χρησιμοποιήσετε πραγματικά το access_token αποκτήσατε στο βήμα 3.  Ανοίξτε το web app `Controllers\TodoListController.cs` αρχείο, το οποίο καθιστά όλες τις αιτήσεις CRUD για το API λίστα εκκρεμών εργασιών.

- Μπορείτε να χρησιμοποιήσετε MSAL ξανά εδώ για τη λήψη access_tokens από το cache MSAL.  Πρώτα, προσθέστε μια `using` δήλωση για MSAL σε αυτό το αρχείο.

    `using Microsoft.Identity.Client;`

- Στο το `Index` ενέργεια, χρησιμοποιήστε MSAL του `AcquireTokenSilentAsync` μέθοδο για να λάβετε μια access_token που μπορεί να χρησιμοποιηθεί για την ανάγνωση δεδομένων από την υπηρεσία λίστα εκκρεμών εργασιών:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

- Το δείγμα, στη συνέχεια, προσθέτει το διακριτικό που προκύπτει για την αίτηση HTTP GET ως το `Authorization` επικεφαλίδα, η οποία χρησιμοποιεί την υπηρεσία λίστα εκκρεμών εργασιών για τον έλεγχο ταυτότητας της αίτησης.
- Εάν η υπηρεσία λίστα εκκρεμών εργασιών επιστρέφει ένα `401 Unauthorized` απόκρισης, το access_tokens στο MSAL που δεν είναι έγκυρες για κάποιο λόγο.  Σε αυτήν την περίπτωση, πρέπει να απορρίψετε οποιαδήποτε access_tokens από το cache MSAL και εμφάνιση στο χρήστη ένα μήνυμα ότι ενδέχεται να πρέπει να εισέλθετε ξανά, που θα γίνει επανεκκίνηση της ροής διακριτικού acquisition.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

- Ομοίως, εάν MSAL είναι δυνατό να επιστρέψει μια access_token για οποιονδήποτε λόγο, θα πρέπει να πείτε στο χρήστη να συνδεθείτε ξανά.  Αυτό είναι τόσο απλή όσο η αλίευση οποιαδήποτε `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

- Το ίδιο ακριβής `AcquireTokenSilentAsync` κλήση είναι implementd στο το `Create` και `Delete` ενέργειες.  Στις εφαρμογές web, μπορείτε να χρησιμοποιήσετε αυτήν τη μέθοδο MSAL για να λάβετε access_tokens κάθε φορά που τις χρειάζεστε στην εφαρμογή.  MSAL θα εκτελέσει κατά τη λήψη, προσωρινή αποθήκευση και ανανέωση διακριτικά για εσάς.

Τέλος, δημιουργία και εκτέλεση της εφαρμογής σας!  Πραγματοποιήστε είσοδο με ένα λογαριασμό Microsoft ή λογαριασμού Azure AD και Παρατηρήστε πώς απεικονίζεται ταυτότητας του χρήστη στην επάνω γραμμή περιήγησης.  Προσθήκη και διαγραφή ορισμένα στοιχεία από τη λίστα εκκρεμών εργασιών του χρήστη για να δείτε το διακριτικό 2.0 ασφαλές κλήσεις API στην πράξη.  Τώρα έχετε μια εφαρμογή web & web API, και τα δύο ασφαλές χρησιμοποιώντας κλάδο τυπική τα πρωτόκολλα, που μπορούν να ελέγχουν την ταυτότητα χρηστών με τις προσωπικές και εταιρικό/σχολικό τους λογαριασμούς.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) που [παρέχεται εδώ](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Επόμενα βήματα

Για επιπλέον πόρους, ανατρέξτε στο θέμα:
- [Στον οδηγό για προγραμματιστές v2.0 >>](active-directory-appmodel-v2-overview.md)
- [Ετικέτα StackOverflow "azure active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Λήψη ενημερώσεων ασφαλείας για τα προϊόντα μας

Συνιστάται να λαμβάνετε ειδοποιήσεις όταν προκύπτουν περιστατικά ασφαλείας, αν επισκεφθείτε την τοποθεσία [αυτήν τη σελίδα](https://technet.microsoft.com/security/dd252948) και εγγραφή συμβουλευτική ειδοποιήσεων ασφαλείας.
