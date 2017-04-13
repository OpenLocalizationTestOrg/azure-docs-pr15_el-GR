<properties
    pageTitle="API Web .NET v2.0 Azure AD | Microsoft Azure"
    description="Εταιρικό ή σχολικό λογαριασμούς και πώς να δημιουργείτε ένα Api Web MVC .NET που δέχεται διακριτικά από δύο προσωπικό λογαριασμό Microsoft."
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
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>Ασφαλούς ένα API web MVC

Με Azure Active Directory το τελικό σημείο v2.0, μπορείτε να προστατεύσετε ένα API Web χρησιμοποιώντας διακριτικά πρόσβασης [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) , επιτρέπει στους χρήστες με προσωπικό λογαριασμό Microsoft που διαθέτετε και εργασίας ή σχολείου λογαριασμών στο ασφαλή πρόσβαση σας API Web.

> [AZURE.NOTE]
    Δεν όλα τα σενάρια Azure Active Directory και δυνατότητες που υποστηρίζονται από το τελικό σημείο v2.0.  Για να καθορίσετε εάν θα πρέπει να χρησιμοποιήσετε το τελικό σημείο v2.0, διαβάστε σχετικά με [τους περιορισμούς v2.0](active-directory-v2-limitations.md).

Στο ASP.NET web APIs, μπορείτε να εκτελέσετε αυτό με ενδιάμεσο OWIN της Microsoft που περιλαμβάνονται στο διαίρεσης 4,5 .NET Framework.  Εδώ θα χρησιμοποιήσουμε OWIN για να δημιουργήσετε μια "Λίστα εκκρεμών εργασιών" MVC το API Web που επιτρέπει στους υπολογιστές-πελάτες για να δημιουργήσετε και να διαβάσετε εργασίες από τη λίστα εκκρεμών εργασιών του χρήστη.  Το API web θα επαληθεύσετε ότι εισερχόμενες αιτήσεις περιέχει ένα διακριτικό πρόσβασης έγκυρη και απορρίψετε οποιαδήποτε αιτήσεων που δεν διέρχονται επικύρωσης σε ένα προστατευμένο δρομολόγηση.  Αυτό το δείγμα δημιουργήθηκε με τη χρήση του Visual Studio 2015.

## <a name="download"></a>Λήψη
Τον κώδικα για αυτό το πρόγραμμα εκμάθησης διατηρείται [σε GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).  Για να παρακολουθήσετε κατά μήκος, μπορείτε να [κάνετε λήψη της εφαρμογής σκελετό ως ένα .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) ή κλωνοποίηση το σκελετό:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

Η εφαρμογή σκελετό περιλαμβάνει όλος ο κώδικας στερεότυπο κείμενο για μια απλή API, αλλά δεν διαθέτει όλα τα τμήματα που σχετίζονται με την ταυτότητα. Εάν δεν θέλετε να παρακολουθήσετε κατά μήκος, μπορείτε να κλωνοποίηση αντί για αυτό ή να [κάνετε λήψη ολοκληρωμένο δείγμα](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Καταχώρηση εφαρμογής
Δημιουργία νέας εφαρμογής στο [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ή ακολουθήστε αυτά τα [λεπτομερή βήματα](active-directory-v2-app-registration.md).  Βεβαιωθείτε ότι:

- Αντιγραφή προς τα κάτω το **Αναγνωριστικό εφαρμογής** που έχουν εκχωρηθεί σε εφαρμογή σας, θα χρειαστεί το συντομότερο.

Αυτή η λύση visual studio περιέχει επίσης "TodoListClient", που είναι μια απλή εφαρμογή WPF.  Το TodoListClient χρησιμοποιείται για να δείχνουν πώς ένας χρήστης πρόσημα στο και πώς ένα πρόγραμμα-πελάτη μπορούν να εκδώσετε αιτήσεις για το API Web.  Σε αυτήν την περίπτωση, το TodoListClient και το TodoListService αντιπροσωπεύονται από την ίδια εφαρμογή.  Για να ρυθμίσετε τις παραμέτρους του TodoListClient, θα πρέπει επίσης να:

- Προσθέστε την πλατφόρμα **Mobile** για την εφαρμογή σας.


## <a name="install-owin"></a>Εγκατάσταση OWIN

Τώρα που έχετε καταχωρήσει μια εφαρμογή, πρέπει να ρυθμίσετε την εφαρμογή σας για να επικοινωνήσετε με το τελικό σημείο v2.0 προκειμένου να επικυρώσετε εισερχόμενες αιτήσεις και τα διακριτικά.

- Για να ξεκινήσετε, ανοίξτε τη λύση και προσθέστε τα πακέτα OWIN ενδιάμεσο NuGet στο έργο TodoListService χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>Ρύθμιση παραμέτρων ελέγχου ταυτότητας OAuth

- Προσθέστε μια κλάση OWIN εκκίνησης στο έργο TodoListService που ονομάζεται `Startup.cs`.  Δεξιό κλικ στο έργο--> **Προσθήκη** --> **Νέο στοιχείο** --> αναζήτηση για "OWIN".  Το ενδιάμεσο OWIN θα ενεργοποιήσει το `Configuration(…)` μέθοδο κατά την εκκίνηση της εφαρμογής σας.
- Αλλάξτε τη δήλωση κλάσης `public partial class Startup` -Έχουμε ήδη υλοποιήσει μέρος αυτής της κλάσης για εσάς σε ένα άλλο αρχείο.  Στο το `Configuration(…)` μέθοδο, κάνετε μια κλήση σε ConfgureAuth(...) για να ρυθμίσετε τον έλεγχο ταυτότητας για την εφαρμογή web.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- Ανοίξτε το αρχείο `App_Start\Startup.Auth.cs` και να εφαρμόζουν το `ConfigureAuth(…)` μέθοδο, η οποία θα ρυθμιστεί για το API Web για να αποδεχτείτε διακριτικά από το τελικό σημείο v2.0.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- Τώρα μπορείτε να χρησιμοποιήσετε `[Authorize]` χαρακτηριστικά για την προστασία σας ελεγκτές και ενέργειες με έλεγχο ταυτότητας φορέα OAuth 2.0.  Διακόσμηση το `Controllers\TodoListController.cs` τάξης με μια ετικέτα εξουσιοδότηση.  Αυτό θα επιβάλετε το χρήστη για να πραγματοποιήσετε είσοδο πριν από την πρόσβαση σε αυτήν τη σελίδα.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Όταν μια εξουσιοδοτημένοι καλούντος με επιτυχία καλεί ένα από τα `TodoListController` API, η ενέργεια ενδέχεται να πρέπει να έχετε πρόσβαση σε πληροφορίες σχετικά με τον καλούντα.  OWIN παρέχει πρόσβαση σε απαιτήσεις μέσα το διακριτικό φορέα μέσω του `ClaimsPrincpal` αντικειμένου.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   Τέλος, ανοίξτε το `web.config` αρχείων στον ριζικό κατάλογο του έργου TodoListService και πληκτρολογήστε τις τιμές παραμέτρων στην το `<appSettings>` ενότητας.
  - Σας `ida:Audience` είναι το **Αναγνωριστικό εφαρμογής** της εφαρμογής που καταχωρήσατε στην πύλη.

## <a name="configure-the-client-app"></a>Ρύθμιση παραμέτρων της εφαρμογής υπολογιστή-πελάτη
Μπορείτε να δείτε την υπηρεσία λίστα Todo στην πράξη, πρέπει να ρυθμίσετε τις παραμέτρους του προγράμματος-πελάτη λίστα Todo ώστε να λάβετε τα διακριτικά από το τελικό σημείο v2.0 και πραγματοποίηση κλήσεων για την υπηρεσία.

- Στο έργο TodoListClient, ανοίξτε το `App.config` και πληκτρολογήστε τις τιμές παραμέτρων στην το `<appSettings>` ενότητας.
  - Σας `ida:ClientId` αναγνωριστικό εφαρμογής που αντιγράψατε από την πύλη.

Τέλος, εκκαθάριση, δημιουργία και εκτέλεση κάθε έργο!  Τώρα έχετε ένα API Web MVC .NET που δέχεται εταιρικός ή Σχολικός λογαριασμούς και τα διακριτικά από τους δύο λογαριασμούς Microsoft προσωπικό.  Πραγματοποιήστε είσοδο στο το TodoListClient και κλήση σας στο web api για να προσθέσετε εργασίες στη λίστα εκκρεμών εργασιών του χρήστη.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) [παρέχεται ως ένα .zip εδώ](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), ή μπορείτε να αντιγράψει το το GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα μπορείτε να μετακινήσετε σε πρόσθετα θέματα.  Εάν θέλετε, μπορείτε να δοκιμάσετε:

[Κλήση Web API από μια εφαρμογή Web >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Για επιπλέον πόρους, ανατρέξτε στο θέμα:
- [Στον οδηγό για προγραμματιστές v2.0 >>](active-directory-appmodel-v2-overview.md)
- [Ετικέτα StackOverflow "azure active directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Λήψη ενημερώσεων ασφαλείας για τα προϊόντα μας

Συνιστάται να λαμβάνετε ειδοποιήσεις όταν προκύπτουν περιστατικά ασφαλείας, αν επισκεφθείτε την τοποθεσία [αυτήν τη σελίδα](https://technet.microsoft.com/security/dd252948) και εγγραφή συμβουλευτική ειδοποιήσεων ασφαλείας.
