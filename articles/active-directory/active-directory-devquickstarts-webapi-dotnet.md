<properties
    pageTitle="Γρήγορα αποτελέσματα .NET Azure AD | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε ένα API Web MVC .NET που ενοποιείται με το Azure AD για τον έλεγχο ταυτότητας και εξουσιοδότηση."
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


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>Προστασία μιας API Web με χρήση φορέα διακριτικά από Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Εάν δημιουργείτε μια εφαρμογή η οποία παρέχει πρόσβαση σε προστατευμένο πόρους που θα πρέπει να γνωρίζετε πώς να προστατεύσετε αυτούς τους πόρους από την access αδικαιολόγητες.
Azure AD καθιστά απλή και απλή για την προστασία web API χρησιμοποιώντας τα διακριτικά Access 2.0 φορέα OAuth με λίγες μόνο γραμμές του κώδικα.

Στις εφαρμογές web Asp.NET, μπορείτε να εκτελέσετε αυτό με εφαρμογή του Microsoft από την Κοινότητα βάσει OWIN το ενδιάμεσο περιλαμβάνονται στο διαίρεσης 4,5 .NET Framework.  Εδώ θα χρησιμοποιήσουμε OWIN για να δημιουργήσετε μια τοποθεσία web "Λίστα εκκρεμών εργασιών" API που:
-   Ορίζει ποια API δεν είναι προστατευμένο.
-   Επαληθεύει ότι οι κλήσεις Web API περιέχει ένα έγκυρο διακριτικό πρόσβασης.

Για να το κάνετε αυτό, θα πρέπει να:

1. Καταχωρήστε μια εφαρμογή του Azure AD
2. Ρύθμιση της εφαρμογής σας για να χρησιμοποιήσετε τη διαδικασία ελέγχου ταυτότητας OWIN.
3. Ρύθμιση παραμέτρων μιας εφαρμογής προγράμματος-πελάτη για να καλέσετε το για να κάνετε λίστα API Web

Για να ξεκινήσετε, [κάντε λήψη του σκελετό εφαρμογής](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) ή [λήψη ολοκληρωμένο δείγμα](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Κάθε είναι μια λύση Visual Studio 2013.  Θα χρειαστεί επίσης ένα μισθωτή του Azure AD στην οποία θέλετε να καταχωρήσετε την εφαρμογή σας.  Εάν δεν έχετε ήδη, [Μάθετε πώς μπορείτε να αποκτήσετε](active-directory-howto-tenant.md).


## <a name="1--register-an-application-with-azure-ad"></a>*1. καταχώρηση μιας εφαρμογής με το Azure AD*
Για να προστατεύσετε την εφαρμογή σας, θα πρέπει πρώτα να δημιουργήσετε μια εφαρμογή στο μισθωτή σας και δώστε Azure AD μερικές σημαντικές πληροφορίες.

-   Πραγματοποιήστε είσοδο στο στην [πύλη διαχείρισης Azure](https://manage.windowsazure.com)
-   Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην **Υπηρεσία καταλόγου Active Directory**
-   Επιλέξτε έναν μισθωτή στην οποία θέλετε να καταχωρήσετε την εφαρμογή.
-   Κάντε κλικ στην καρτέλα **εφαρμογές** και κάντε κλικ στην επιλογή **Προσθήκη** στο το σχέδιο κάτω.
-   Ακολουθήστε τις οδηγίες και δημιουργήστε μια νέα **εφαρμογή Web ή/και WebAPI**.
    -   Το **όνομα** της εφαρμογής θα περιγράφουν την εφαρμογή σας στους τελικούς χρήστες.  Πληκτρολογήστε "λίστα εκκρεμών εργασιών υπηρεσίας".
    -   Η **Ανακατεύθυνση Uri** είναι ένας συνδυασμός συνδυασμό και συμβολοσειρά που Azure AD θα χρησιμοποιήσετε για να επιστρέψετε οποιαδήποτε διακριτικά της εφαρμογής σας ζητηθεί. Πληκτρολογήστε `https://localhost:44321/` για αυτήν την τιμή.
-   Όταν ολοκληρώσετε την καταχώρηση, μεταβείτε στην καρτέλα **Ρύθμιση παραμέτρων** και εντοπίστε το πεδίο **URI Αναγνωριστικό εφαρμογής** .  Εισαγάγετε ένα συγκεκριμένο μισθωτή αναγνωριστικό για αυτήν την τιμή, π.χ.`https://contoso.onmicrosoft.com/TodoListService`
- Αποθηκεύστε τη ρύθμιση παραμέτρων.  Αφήστε ανοιχτό το πύλη - επίσης θα χρειαστεί να καταχωρήσετε την εφαρμογή-πελάτη λίγο.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. ρύθμιση της εφαρμογής σας για να χρησιμοποιήσετε τη διαδικασία ελέγχου ταυτότητας OWIN*

Τώρα που καταχωρήσατε μια εφαρμογή με το Azure AD, πρέπει να ρυθμίσετε την εφαρμογή σας για να επικοινωνήσετε με το Azure AD προκειμένου να επικυρώσετε εισερχόμενες αιτήσεις και τα διακριτικά.

-   Για να ξεκινήσετε, ανοίξτε τη λύση και προσθέστε τα πακέτα OWIN ενδιάμεσο NuGet στο έργο TodoListService χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   Προσθέστε μια κλάση OWIN εκκίνησης στο έργο TodoListService που ονομάζεται `Startup.cs`.  Δεξιό κλικ στο έργο--> **Προσθήκη** --> **Νέο στοιχείο** --> αναζήτηση για "OWIN".  Το ενδιάμεσο OWIN θα ενεργοποιήσει το `Configuration(…)` μέθοδο κατά την εκκίνηση της εφαρμογής σας.
-   Αλλάξτε τη δήλωση κλάσης `public partial class Startup` -Έχουμε ήδη υλοποιήσει μέρος αυτής της κλάσης για εσάς σε ένα άλλο αρχείο.  Στο το `Configuration(…)` μέθοδο, κάνετε μια κλήση σε ConfgureAuth(...) για να ρυθμίσετε τον έλεγχο ταυτότητας για την εφαρμογή web.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Ανοίξτε το αρχείο `App_Start\Startup.Auth.cs` και να εφαρμόζουν το `ConfigureAuth(…)` μέθοδο.  Οι παράμετροι που παρέχετε στο `WindowsAzureActiveDirectoryBearerAuthenticationOptions` θα χρησιμοποιηθεί ως συντεταγμένες για την εφαρμογή σας για να επικοινωνήσετε με το Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   Τώρα μπορείτε να χρησιμοποιήσετε `[Authorize]` χαρακτηριστικά για την προστασία σας ελεγκτές και ενέργειες με έλεγχο ταυτότητας φορέα JWT.  Διακόσμηση το `Controllers\TodoListController.cs` τάξης με μια ετικέτα εξουσιοδότηση.  Αυτό θα επιβάλετε το χρήστη για να πραγματοποιήσετε είσοδο πριν από την πρόσβαση σε αυτήν τη σελίδα.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Όταν μια εξουσιοδοτημένοι καλούντος με επιτυχία καλεί ένα από τα `TodoListController` API, η ενέργεια ενδέχεται να πρέπει να έχετε πρόσβαση σε πληροφορίες σχετικά με τον καλούντα.  OWIN παρέχει πρόσβαση σε απαιτήσεις μέσα το διακριτικό φορέα μέσω του `ClaimsPrincpal` αντικειμένου.  
- Είναι μια συνηθισμένη απαίτηση για web APIs για να επικυρώσει το "εύρος" παρουσίαση στο διακριτικό - αυτόν τον τρόπο εξασφαλίζεται ότι ο τελικός χρήστης έχει δεχθεί να τα δικαιώματα που απαιτούνται για να αποκτήσετε πρόσβαση στην υπηρεσία λίστα Todo:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   Τέλος, ανοίξτε το `web.config` αρχείων στον ριζικό κατάλογο του έργου TodoListService και πληκτρολογήστε τις τιμές παραμέτρων στην το `<appSettings>` ενότητας.
  - Η `ida:Tenant` είναι το όνομα του μισθωτή του Azure AD, π.χ. "contoso.onmicrosoft.com".
  - Σας `ida:Audience` είναι το URI Αναγνωριστικό εφαρμογής της εφαρμογής που έχετε εισαγάγει στην πύλη του Azure.

## <a name="3--configure-a-client-application--run-the-service"></a>*3. ρύθμιση παραμέτρων μιας εφαρμογής του προγράμματος-πελάτη και εκτελείται η υπηρεσία*
Μπορείτε να δείτε την υπηρεσία λίστα Todo στην πράξη, πρέπει να ρυθμίσετε τις παραμέτρους του προγράμματος-πελάτη λίστα Todo ώστε να λάβετε τα διακριτικά από AAD και πραγματοποίηση κλήσεων για την υπηρεσία.

- Μεταβείτε στην [Πύλη διαχείρισης του Azure](https://manage.windowsazure.com)
- Δημιουργία νέας εφαρμογής στο μισθωτή του Azure AD και επιλέξτε **Εγγενή εφαρμογή προγράμματος-πελάτη** στην προτροπή που προκύπτει.
    -   Το **όνομα** της εφαρμογής θα περιγράφει την εφαρμογή σας στους τελικούς χρήστες
    -   Πληκτρολογήστε `http://TodoListClient/` για την τιμή **Ανακατεύθυνση Uri** .
- Όταν ολοκληρώσετε την καταχώρηση, AAD θα εκχωρήσετε την εφαρμογή σας ένα μοναδικό **Αναγνωριστικό υπολογιστή-πελάτη**. Θα χρειαστείτε αυτή την τιμή στα επόμενα βήματα, επομένως, αντιγράψτε την από την καρτέλα ρύθμιση παραμέτρων.
- Επίσης στην καρτέλα **Ρύθμιση παραμέτρων** , εντοπίστε την ενότητα "Δικαιώματα σε άλλες εφαρμογές". Κάντε κλικ στην επιλογή "Προσθήκη εφαρμογής". Επιλέξτε "Όλες οι εφαρμογές", στην αναπτυσσόμενη λίστα "Εμφάνιση" και κάντε κλικ στο επάνω σημάδι ελέγχου. Εντοπίστε και κάντε κλικ στο για να κάνετε λίστα την υπηρεσία σας και κάντε κλικ στο σημάδι ελέγχου κάτω για να προσθέσετε την εφαρμογή. Επιλέξτε "Access για να κάνετε λίστα υπηρεσίας" από την αναπτυσσόμενη λίστα "Ανάθεση δικαιωμάτων" και αποθηκεύστε τη ρύθμιση παραμέτρων.


- Στο Visual Studio, ανοίξτε `App.config` στο TodoListClient το project και εισαγάγετε τις τιμές παραμέτρων στην το `<appSettings>` ενότητας.
  - Η `ida:Tenant` είναι το όνομα του μισθωτή του Azure AD, π.χ. "contoso.onmicrosoft.com".
  - Σας `ida:ClientId` Αναγνωριστικό εφαρμογής που αντιγράψατε από την πύλη του Azure.
  - Σας `todo:TodoListResourceId` είναι το URI Αναγνωριστικό εφαρμογής της εφαρμογής για να την κάνετε υπηρεσίας σε λίστα που καταχωρήσατε στην πύλη του Azure.

Τέλος, εκκαθάριση, δημιουργία και εκτέλεση κάθε έργο!  Εάν δεν το έχετε κάνει ήδη, τώρα είναι η ώρα για να δημιουργήσετε έναν νέο χρήστη στο μισθωτή σας με μια *. τομέα onmicrosoft.com.  Πραγματοποιήστε είσοδο στο πρόγραμμα-πελάτη λίστα εκκρεμών εργασιών με τον συγκεκριμένο χρήστη και προσθήκη ορισμένες εργασίες σε λίστα του χρήστη για να κάνετε.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) παρέχεται [εδώ](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Τώρα, μπορείτε να μετακινήσετε περισσότερες πρόσθετες ταυτότητας σενάρια.

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
