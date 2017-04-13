<properties
    pageTitle="B2C καταλόγου Azure Active Directory | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή web που καλεί μια API web με χρήση του Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure AD B2C: Καλέσετε ένα API web από μια εφαρμογή web .NET

Με τη χρήση B2C Azure Active Directory (Azure AD), μπορείτε να προσθέσετε δυνατότητες διαχείρισης ισχυρή ταυτότητας από το χρήστη για τις εφαρμογές web και web APIs με λίγα βήματα σύντομο. Σε αυτό το άρθρο θα ασχολείται με τον τρόπο για να δημιουργήσετε μια εφαρμογή web .NET μοντέλο-προβολή-ελεγκτή (MVC) "λίστα εκκρεμών εργασιών" που καλεί μια API web χρησιμοποιώντας τα διακριτικά φορέα

Σε αυτό το άρθρο δεν καλύπτει Διαχείριση προφίλ με Azure AD B2C και πώς μπορείτε να υλοποιήσετε εισόδου, εγγραφής. Εστιάζει στην κλήση web APIs μετά το ήδη τον έλεγχο ταυτότητας χρήστη. Εάν δεν το έχετε κάνει ήδη, θα πρέπει να μπορείτε να ξεκινήσετε με το [.NET web app ξεκινήσατε το πρόγραμμα εκμάθησης](active-directory-b2c-devquickstarts-web-dotnet.md) για να μάθετε περισσότερα σχετικά με τα βασικά στοιχεία του Azure AD B2C.

## <a name="get-an-azure-ad-b2c-directory"></a>Λήψη ενός καταλόγου Azure AD B2C

Μπορείτε να χρησιμοποιήσετε Azure AD B2C, πρέπει να δημιουργήσετε έναν κατάλογο, ή να μισθωτή.  Ένας κατάλογος είναι ένα κοντέινερ για όλους τους χρήστες σας, οι εφαρμογές, ομάδες και πολλά άλλα.  Εάν δεν έχετε ήδη, [Δημιουργήστε έναν κατάλογο B2C](active-directory-b2c-get-started.md) πριν να συνεχίσετε με αυτόν τον οδηγό.

## <a name="create-an-application"></a>Δημιουργία μιας εφαρμογής

Στη συνέχεια, πρέπει να δημιουργήσετε μια εφαρμογή στον κατάλογό σας B2C. Αυτό σας δίνει Azure AD πληροφορίες που χρειάζονται για την ασφαλή επικοινωνία με την εφαρμογή σας. Σε αυτήν την περίπτωση, το web app και το web API θα αναπαρίσταται από ένα μεμονωμένο **Αναγνωριστικό εφαρμογής**, επειδή αυτά περιλαμβάνουν μία λογική εφαρμογής. Για να δημιουργήσετε μια εφαρμογή, ακολουθήστε [αυτές τις οδηγίες](active-directory-b2c-app-registration.md). Βεβαιωθείτε ότι:

- Συμπεριλάβετε μια **εφαρμογή web/web API** στην εφαρμογή.
- Πληκτρολογήστε `https://localhost:44316/` ως **διεύθυνση URL απάντηση**. Είναι η προεπιλεγμένη διεύθυνση URL για αυτό το δείγμα κώδικα.
- Αντιγράψτε το **Αναγνωριστικό εφαρμογής** που έχει αντιστοιχιστεί σε εφαρμογή σας. Θα χρειαστεί επίσης αυτό αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Δημιουργία πολιτικών σας

Στο Azure AD B2C, κάθε εμπειρία χρήστη ορίζεται από μια [πολιτική](active-directory-b2c-reference-policies.md). Αυτή η εφαρμογή web περιέχει τρία εμπειρίες ταυτότητας: εγγραφείτε, πραγματοποιήστε είσοδο και επεξεργασία του προφίλ. Πρέπει να δημιουργήσετε μία πολιτική κάθε τύπο, όπως περιγράφεται στο [άρθρο αναφοράς πολιτικής](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Όταν δημιουργείτε τις τρεις πολιτικές, βεβαιωθείτε ότι:

- Επιλέξτε το **εμφανιζόμενο όνομα** και άλλα χαρακτηριστικά εγγραφής από την πολιτική της εγγραφής σας.
- Επιλέξτε το **εμφανιζόμενο όνομα** και το **Αναγνωριστικό αντικειμένου** δηλώσεις εφαρμογής σε κάθε πολιτική. Μπορείτε να επιλέξετε καθώς και άλλες αξιώσεων.
- Αντιγράψτε το **όνομα** κάθε πολιτική μετά τη δημιουργία του. Θα πρέπει να έχει το πρόθεμα `b2c_1_`. Θα χρειαστείτε αυτά τα ονόματα πολιτικής αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Αφού δημιουργήσετε τρεις πολιτικές σας, είστε έτοιμοι να δημιουργήσετε την εφαρμογή σας.

Σημειώστε ότι αυτό το άρθρο δεν καλύπτει πώς μπορείτε να χρησιμοποιήσετε τις πολιτικές που μόλις δημιουργήσατε. Για να μάθετε πώς λειτουργούν οι πολιτικές στο Azure AD B2C, ξεκινήστε με το [.NET web app ξεκινήσατε το πρόγραμμα εκμάθησης](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Κάντε λήψη του κώδικα

Ο κωδικός για αυτό προγραμμάτων εκμάθησης [διατηρούνται με GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet). Για να δημιουργήσετε το δείγμα καθώς εργάζεστε, μπορείτε να [κάνετε λήψη του σκελετό έργου ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). Μπορείτε επίσης να αντιγράψετε το σκελετό:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

Η εφαρμογή ολοκληρωμένων είναι επίσης [διαθέσιμο ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) ή σε το `complete` υποκατάστημα στο ίδιο αποθετήριο.

Μετά τη λήψη του δείγματος κώδικα, ανοίξτε το αρχείο .sln Visual Studio για να ξεκινήσετε.

## <a name="configure-the-task-web-app"></a>Ρύθμιση παραμέτρων της εφαρμογής web εργασίας

Για να λάβετε `TaskWebApp` για να επικοινωνήσετε με Azure AD B2C, πρέπει να παρέχει ορισμένα κοινά παραμέτρους. Στο το `TaskWebApp` έργου, ανοίξτε το `web.config` αρχείων στον ριζικό κατάλογο του έργου και να αντικαταστήσετε τις τιμές το `<appSettings>` ενότητας. Μπορείτε να αφήσετε το `AadInstance`, `RedirectUri`, και `TaskServiceUrl` τις τιμές ως έχουν.

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>Λήψη διακριτικά πρόσβασης και καλέστε την εργασία API

Αυτή η ενότητα θα περιγράφουν πώς μπορείτε να χρησιμοποιήσετε το διακριτικό που λαμβάνεται κατά τη σύνδεση με το Azure AD B2C για να αποκτήσετε πρόσβαση σε μια τοποθεσία web API που προστατεύεται επίσης με το Azure AD B2C.

Σε αυτό το άρθρο δεν καλύπτει τις λεπτομέρειες σχετικά με τον τρόπο για την ασφάλιση του API. Για να μάθετε πώς ένα API web ασφαλή πραγματοποιεί έλεγχο ταυτότητας αιτήσεις χρησιμοποιώντας Azure AD B2C, δείτε το [web API γρήγορα αποτελέσματα το άρθρο](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="save-the-sign-in-token"></a>Αποθήκευση στο σύμβολο στο διακριτικού

Πρώτα, έλεγχο ταυτότητας του χρήστη (χρησιμοποιώντας μία από τις πολιτικές σας) και λήψη ενός διακριτικού εισόδου από το Azure AD B2C.  Εάν δεν είστε βέβαιοι πώς να εκτελέσει πολιτικές, επιστρέψτε και δοκιμάστε το [.NET web app ξεκινήσατε το πρόγραμμα εκμάθησης](active-directory-b2c-devquickstarts-web-dotnet.md) για να μάθετε περισσότερα σχετικά με τα βασικά στοιχεία του Azure AD B2C.

Ανοίξτε το αρχείο `App_Start\Startup.Auth.cs`.  Υπάρχει ένα σημαντικό αλλαγή πρέπει να κάνετε για να το `OpenIdConnectAuthenticationOptions` -πρέπει να ορίσετε `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

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
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>Λήψη ενός διακριτικού τους ελεγκτές

Η `TasksController` είναι υπεύθυνος για την επικοινωνία με το web API, αποστολή αιτήσεων HTTP για το API για ανάγνωση, δημιουργία και διαγραφή εργασιών.  Επειδή το API προστατεύεται από Azure AD B2C, πρέπει να πρώτα να ανακτήσετε το διακριτικό που αποθηκεύσατε στο βήμα παραπάνω.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

Το `BootstrapContext` περιέχει είσοδος διακριτικό που αποκτήσατε εκτελώντας μία από τις πολιτικές B2C.

### <a name="read-tasks-from-the-web-api"></a>Διαβάστε εργασίες από το web API

Όταν έχετε ενός διακριτικού, μπορείτε να το επισυνάψετε σε HTTP `GET` αίτηση στο το `Authorization` κεφαλίδα για την ασφαλή κλήση `TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Δημιουργία και διαγραφή εργασιών στο web API

Ακολουθήστε το ίδιο μοτίβο όταν στέλνετε `POST` και `DELETE` αιτήσεις για το API, στο web χρησιμοποιώντας το `BootstrapContext` για να ανακτήσετε είσοδος διακριτικού. Θα σας υλοποιηθεί την ενέργεια "Δημιουργία" για εσάς. Μπορείτε να δοκιμάσετε τελικής επεξεργασίας την ενέργεια διαγραφής σε `TasksController.cs`.

## <a name="run-the-sample-app"></a>Εκτελέστε την εφαρμογή δείγματος

Τέλος, δημιουργία και εκτέλεση της εφαρμογής. Εγγραφή και είσοδος και δημιουργία εργασιών για το χρήστη συνδεδεμένοι στο. Πραγματοποιήστε έξοδο και συνδεθείτε με ένα διαφορετικό χρήστη. Δημιουργία εργασιών για τον συγκεκριμένο χρήστη. Παρατηρήστε πώς οι εργασίες είναι αποθηκευμένες ανά χρήστη στην το API, επειδή το API εξάγει την ταυτότητα του χρήστη από το διακριτικό λάβει.

Για αναφορά, το ολοκληρωμένο δείγμα που [παρέχεται ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip). Μπορείτε, επίσης, να την αντιγράψετε από GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
