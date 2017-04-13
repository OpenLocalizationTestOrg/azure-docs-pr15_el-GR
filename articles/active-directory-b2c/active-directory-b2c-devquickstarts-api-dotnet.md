<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε ένα .NET API Web με χρήση του Azure Active Directory B2C, ασφαλές χρησιμοποιώντας διακριτικά πρόσβασης OAuth 2.0 για τον έλεγχο ταυτότητας."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Δημιουργήστε ένα API web .NET

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Με B2C Azure Active Directory (Azure AD), μπορείτε να ασφαλίσετε μια API web με χρήση του διακριτικά πρόσβασης OAuth 2.0. Αυτά τα διακριτικά επιτρέπει τις εφαρμογές προγράμματος-πελάτη που χρησιμοποιούν Azure AD B2C για τον έλεγχο ταυτότητας για το API. Αυτό το άρθρο σας δείχνει πώς μπορείτε να δημιουργήσετε ένα API "λίστα εκκρεμών εργασιών" μοντέλο-προβολή-ελεγκτή .NET (MVC) που επιτρέπει στους χρήστες να CRUD για εργασίες. Το web API είναι ασφαλή χρήση Azure AD B2C και επιτρέπει μόνο εξουσιοδοτημένους χρήστες για να διαχειριστείτε τη λίστα εκκρεμών εργασιών.

## <a name="create-an-azure-ad-b2c-directory"></a>Δημιουργία ενός καταλόγου Azure AD B2C

Μπορείτε να χρησιμοποιήσετε Azure AD B2C, πρέπει να δημιουργήσετε έναν κατάλογο, ή να μισθωτή. Ένας κατάλογος είναι ένα κοντέινερ για όλους τους χρήστες σας, εφαρμογές, ομάδες και πολλά άλλα. Εάν δεν έχετε ήδη, [Δημιουργήστε έναν κατάλογο B2C](active-directory-b2c-get-started.md) πριν να συνεχίσετε με αυτόν τον οδηγό.

## <a name="create-an-application"></a>Δημιουργία μιας εφαρμογής

Στη συνέχεια, πρέπει να δημιουργήσετε μια εφαρμογή στον κατάλογό σας B2C. Αυτό σας δίνει Azure AD πληροφορίες που χρειάζονται για την ασφαλή επικοινωνία με την εφαρμογή σας. Για να δημιουργήσετε μια εφαρμογή, ακολουθήστε [αυτές τις οδηγίες](active-directory-b2c-app-registration.md). Βεβαιωθείτε ότι:

- Συμπεριλάβετε μια **εφαρμογή web** ή **web API** στην εφαρμογή.
- Χρησιμοποιήστε την **ανακατεύθυνση ομοιόμορφο αναγνωριστικό πόρου** `https://localhost:44316/` για την εφαρμογή web. Αυτή είναι η προεπιλεγμένη θέση του προγράμματος-πελάτη εφαρμογής web για αυτό το δείγμα κώδικα.
- Αντιγράψτε το **Αναγνωριστικό εφαρμογής** που έχει αντιστοιχιστεί σε εφαρμογή σας. Που θα το χρειαστείτε αργότερα.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Δημιουργία πολιτικών σας

Στο Azure AD B2C, κάθε εμπειρία χρήστη ορίζεται από μια [πολιτική](active-directory-b2c-reference-policies.md). Το πρόγραμμα-πελάτη σε αυτό το δείγμα κώδικα περιέχει τρία εμπειρίες ταυτότητας: εγγραφείτε, πραγματοποιήστε είσοδο και επεξεργασία του προφίλ. Θα πρέπει να δημιουργήσετε μια πολιτική για κάθε τύπο, όπως περιγράφεται στο [άρθρο αναφοράς πολιτικής](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Όταν δημιουργείτε τις πολιτικές τρεις σας, βεβαιωθείτε ότι:

- Επιλέξτε **ηλεκτρονικού ταχυδρομείου εγγραφής** ή **Αναγνωριστικό χρήστη εγγραφής** στο το blade υπηρεσίες παροχής ταυτότητας.
- Επιλέξτε **εμφανιζόμενο όνομα** και άλλα χαρακτηριστικά εγγραφής από την πολιτική της εγγραφής σας.
- Επιλέξτε αξιώσεων **εμφανιζόμενο όνομα** και το **Αναγνωριστικό αντικειμένου** ως δηλώσεις εφαρμογής για κάθε πολιτική. Μπορείτε να επιλέξετε καθώς και άλλες αξιώσεων.
- Αντιγράψτε το **όνομα** κάθε πολιτική μετά τη δημιουργία του. Θα χρειαστείτε αυτά τα ονόματα πολιτικής αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Αφού δημιουργήσετε με επιτυχία τις τρεις πολιτικές, είστε έτοιμοι να δημιουργήσετε την εφαρμογή σας.

## <a name="download-the-code"></a>Κάντε λήψη του κώδικα

Ο κωδικός για αυτό προγραμμάτων εκμάθησης [διατηρούνται με GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). Για να δημιουργήσετε το δείγμα καθώς εργάζεστε, μπορείτε να [κάνετε λήψη ενός έργου σκελετό ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Μπορείτε επίσης να αντιγράψετε το σκελετό:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Η εφαρμογή ολοκληρωμένων είναι επίσης [διαθέσιμο ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) ή σε το `complete` υποκατάστημα στο ίδιο αποθετήριο.

Μετά τη λήψη του δείγματος κώδικα, ανοίξτε το αρχείο .sln Visual Studio για να ξεκινήσετε. Το αρχείο λύσης περιέχει δύο έργα: `TaskWebApp` και `TaskService`. `TaskWebApp`είναι μια εφαρμογή web MVC που ο χρήστης αλληλεπιδρά με. `TaskService`είναι η εφαρμογή web παρασκηνίου API που αποθηκεύει λίστα εκκρεμών εργασιών κάθε χρήστη.

## <a name="configure-the-task-web-app"></a>Ρύθμιση παραμέτρων της εφαρμογής web εργασίας

Όταν ένας χρήστης αλληλεπιδρά με `TaskWebApp`, ο υπολογιστής-πελάτης στέλνει αιτήσεις Azure AD και λαμβάνει ξανά τα διακριτικά που μπορούν να χρησιμοποιηθούν για να καλέσετε το `TaskService` API στο web. Για να εισέλθετε στο χρήστη και λάβετε τα διακριτικά, πρέπει να παρέχετε `TaskWebApp` με ορισμένες πληροφορίες σχετικά με την εφαρμογή σας. Στο το `TaskWebApp` έργου, ανοίξτε το `web.config` αρχείων στον ριζικό κατάλογο του έργου και να αντικαταστήσετε τις τιμές σε το `<appSettings>` ενότητας.  Μπορείτε να αφήσετε το `AadInstance`, `RedirectUri`, και `TaskServiceUrl` τις τιμές ως-είναι.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

Σε αυτό το άρθρο δεν καλύπτει δόμησης το `TaskWebApp` προγράμματος-πελάτη.  Για να μάθετε πώς να δημιουργείτε μια εφαρμογή web χρησιμοποιώντας Azure AD B2C, ανατρέξτε στο θέμα [το πρόγραμμα εκμάθησης μας .NET web app](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>Ασφαλούς το API

Όταν έχετε ένα πρόγραμμα-πελάτη που καλεί το API εκ μέρους τους χρήστες, μπορείτε να ασφαλίσετε `TaskService` χρησιμοποιώντας τα διακριτικά φορέα OAuth 2.0. Το API μπορούν να αποδεχτούν και να επικυρώσει τα διακριτικά με χρήση του περιβάλλοντος εργασίας Άνοιγμα Web της Microsoft για τη βιβλιοθήκη .NET (OWIN).

### <a name="install-owin"></a>Εγκατάσταση του OWIN
Ξεκινήστε με την εγκατάσταση της διοχέτευσης ελέγχου ταυτότητας OWIN OAuth:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Πληκτρολογήστε τις λεπτομέρειες του B2C σας
Άνοιγμα του `web.config` αρχείο στη ρίζα της το `TaskService` του project και να αντικαταστήσετε τις τιμές σε το `<appSettings>` ενότητας. Αυτές οι τιμές θα χρησιμοποιηθεί σε ολόκληρη τη βιβλιοθήκη API και OWIN.  Μπορείτε να αφήσετε το `AadInstance` τιμή που δεν έχουν αλλάξει.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Προσθέστε μια κλάση εκκίνησης OWIN
Προσθέστε μια OWIN κλάση εκκίνησης για το `TaskService` έργου που ονομάζεται `Startup.cs`.  Κάντε δεξί κλικ στο έργο, επιλέξτε **Προσθήκη** και **Νέο στοιχείο**και, στη συνέχεια, αναζητήστε OWIN.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>Ρύθμιση παραμέτρων ελέγχου ταυτότητας διακριτικό 2.0
Ανοίξτε το αρχείο `App_Start\Startup.Auth.cs`, και να εφαρμόζουν το `ConfigureAuth(...)` μέθοδο:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Ασφαλούς του ελεγκτή εργασιών
Μετά την εφαρμογή έχει ρυθμιστεί ώστε να χρησιμοποιούν τον έλεγχο ταυτότητας διακριτικό 2.0, μπορείτε να ασφαλίσετε το API web προσθέτοντας ένα `[Authorize]` ετικέτα στον ελεγκτή εργασίας. Αυτό είναι το ελεγκτή όλα χειρισμό λίστα εκκρεμών εργασιών, όπου πραγματοποιείται, ώστε να πρέπει να διασφαλίσετε την ολόκληρο ελεγκτή στο επίπεδο της τάξης. Μπορείτε επίσης να προσθέσετε το `[Authorize]` ετικέτα σε μεμονωμένες ενέργειες για πιο αναλυτικό έλεγχο.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Λήψη πληροφοριών χρήστη από το διακριτικό
`TasksController`αποθηκεύει εργασίες σε μια βάση δεδομένων, όπου κάθε εργασία έχει ένα συσχετισμένο χρήστη στον οποίο "ανήκει" της εργασίας. Ο κάτοχος προσδιορίζεται από το χρήστη **Αναγνωριστικό αντικειμένου**. (Αυτός είναι ο λόγος που χρειάζεται για να προσθέσετε το Αναγνωριστικό αντικειμένου ως εφαρμογή διεκδίκηση σε όλες τις πολιτικές.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>Εκτελέστε την εφαρμογή δείγματος

Τέλος, δημιουργία και εκτέλεση και τα δύο `TaskWebApp` και `TaskService`. Εγγραφή για την εφαρμογή, χρησιμοποιώντας μια διεύθυνση ηλεκτρονικού ταχυδρομείου ή το όνομα χρήστη. Δημιουργία ορισμένες εργασίες από τη λίστα εκκρεμών εργασιών του χρήστη και Παρατηρήστε πώς αυτά διατηρούνται στο API ακόμη και μετά τη διακοπή και επανεκκίνηση του προγράμματος-πελάτη.

## <a name="edit-your-policies"></a>Επεξεργαστείτε τις πολιτικές

Όταν που έχουν ασφαλές API χρησιμοποιώντας Azure AD B2C, μπορείτε να πειραματιστείτε με τις πολιτικές της εφαρμογής σας και δείτε τα εφέ (ή δεν διαθέτουν τους) στο το API. Μπορείτε να χειριστείτε τα δηλώσεις εφαρμογής στις πολιτικές και να αλλάξετε τις πληροφορίες χρήστη που είναι διαθέσιμη στο web API. Θα είναι διαθέσιμη στην ιστοσελίδα σας .NET MVC API στο οποιαδήποτε αξιώσεων που θέλετε να προσθέσετε το `ClaimsPrincipal` αντικείμενο, όπως περιγράφεται παραπάνω σε αυτό το άρθρο.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
