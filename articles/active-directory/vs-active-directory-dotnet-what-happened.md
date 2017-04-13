<properties
    pageTitle="Τι απέγινε το έργο μου MVC (υπηρεσία συνδεδεμένοι Visual Studio Azure Active Directory) | Microsoft Azure "
    description="Περιγράφει τι συμβαίνει στο έργο σας MVC όταν συνδέεστε με Azure AD με χρήση του Visual Studio συνδεδεμένες υπηρεσίες"
    services="active-directory"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Τι απέγινε το έργο μου MVC (υπηρεσία συνδεδεμένοι Visual Studio Azure Active Directory);

> [AZURE.SELECTOR]
> - [Γρήγορα αποτελέσματα](vs-active-directory-dotnet-getting-started.md)
> - [Τι έγινε](vs-active-directory-dotnet-what-happened.md)



## <a name="references-have-been-added"></a>Έχουν προστεθεί αναφορές

### <a name="nuget-package-references"></a>Αναφορές πακέτου NuGet

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>Αναφορές .NET

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Κωδικός έχει προστεθεί

### <a name="code-files-were-added-to-your-project"></a>Αρχεία κώδικα έχουν προστεθεί στο έργο σας

Μια κλάση εκκίνησης ελέγχου ταυτότητας, **App_Start/Startup.Auth.cs** προστέθηκε στο έργο σας που περιέχουν λογικής εκκίνησης για τον έλεγχο ταυτότητας Azure AD. Επίσης, μια κλάση ελεγκτή, Controllers/AccountController.cs προστέθηκε που περιέχει μεθόδους **SignIn()** και **SignOut()** . Τέλος, μερική προβολή, **Views/Shared/_LoginPartial.cshtml** προστέθηκε που περιέχει μια σύνδεση στην ενέργεια για εισόδου/SignOut.

### <a name="startup-code-was-added-to-your-project"></a>Κωδικός εκκίνησης προστέθηκε στο έργο σας

Εάν είχατε ήδη μια κλάση εκκίνησης στο έργο σας, η μέθοδος **ρύθμισης παραμέτρων** ενημερώθηκε για να συμπεριλάβετε μια κλήση σε **ConfigureAuth(app)**. Διαφορετικά, κλάση της εκκίνησης προστέθηκε στο έργο σας.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>Ο app.config ή web.config διαθέτει νέες τιμές ρύθμισης παραμέτρων

Έχουν προστεθεί οι ακόλουθες καταχωρήσεις ρύθμισης παραμέτρων.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Δημιουργήθηκε μια εφαρμογή Azure Active Directory (AD)
Εφαρμογή του Azure AD δημιουργήθηκε στον κατάλογο που έχετε επιλέξει στον οδηγό.

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Εάν να γίνει μεταβίβαση ελέγχου *Απενεργοποίηση του ελέγχου ταυτότητας των μεμονωμένων λογαριασμών χρήστη*, τις πρόσθετες αλλαγές που έγιναν για το έργο μου;
Αναφορές πακέτου NuGet έχουν καταργηθεί και έχουν καταργηθεί και αντίγραφα ασφαλείας για τα αρχεία. Ανάλογα με την κατάσταση του έργου σας, ίσως χρειαστεί να καταργήσετε αρχεία ή πρόσθετες αναφορές με μη αυτόματο τρόπο ή να τροποποιήσετε κώδικα ανάλογα με την περίπτωση.

### <a name="nuget-package-references-removed-for-those-present"></a>Αναφορές πακέτου NuGet καταργηθεί (για αυτά παρουσίαση)

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Αρχεία κώδικα αντίγραφα ασφαλείας και καταργηθεί (για αυτά παρουσίαση)

Κάθε ένα από τα ακόλουθα αρχεία ήταν αντίγραφα ασφαλείας και να έχει καταργηθεί από το έργο. Τα αρχεία αντιγράφων ασφαλείας βρίσκονται σε έναν φάκελο 'Δημιουργία αντιγράφων ασφαλείας' στον ριζικό κατάλογο του καταλόγου του έργου.

- **App_Start\IdentityConfig.CS**
- **Controllers\ManageController.CS**
- **Models\IdentityModels.CS**
- **Models\ManageViewModels.CS**

### <a name="code-files-backed-up-for-those-present"></a>Κωδικός αντίγραφα ασφαλείας (για αυτά παρουσίαση)

Κάθε ένα από τα ακόλουθα αρχεία δημιουργήθηκε αντίγραφο ασφαλείας πριν να αντικατασταθούν. Τα αρχεία αντιγράφων ασφαλείας βρίσκονται σε έναν φάκελο 'Δημιουργία αντιγράφων ασφαλείας' στον ριζικό κατάλογο του καταλόγου του έργου.

- **Startup.CS**
- **App_Start\Startup.auth.CS**
- **Controllers\AccountController.CS**
- **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Εάν να γίνει μεταβίβαση ελέγχου *Ανάγνωση καταλόγου δεδομένων*, τις πρόσθετες αλλαγές που έγιναν για το έργο μου;

Έχουν προστεθεί επιπλέον αναφορές.

###<a name="additional-nuget-package-references"></a>Επιπλέον αναφορές του πακέτου NuGet

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###<a name="additional-net-references"></a>Επιπλέον αναφορές .NET

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###<a name="additional-code-files-were-added-to-your-project"></a>Έχουν προστεθεί επιπλέον κώδικα αρχεία στο έργο σας

Δύο αρχεία έχουν προστεθεί για την υποστήριξη διακριτικού σε cache: **Models\ADALTokenCache.cs** και **Models\ApplicationDbContext.cs**.  Μια επιπλέον ελεγκτή και προβολή έχουν προστεθεί για την απεικόνιση χρησιμοποιώντας το Azure graph APIs κατά την πρόσβαση σε πληροφορίες προφίλ χρήστη.  Αυτά τα αρχεία είναι **Controllers\UserProfileController.cs** και **Views\UserProfile\Index.cshtml**.

###<a name="additional-startup-code-was-added-to-your-project"></a>Επιπλέον κωδικό εκκίνησης προστέθηκε στο έργο σας

Στο αρχείο **startup.auth.cs** , ενός νέου αντικειμένου **OpenIdConnectAuthenticationNotifications** προστέθηκε στο μέλος **τις ειδοποιήσεις** της το **OpenIdConnectAuthenticationOptions**.  Αυτή είναι η ενεργοποίηση λαμβάνει τον κωδικό OAuth και του exchange για ένα διακριτικό πρόσβασης.

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Πρόσθετες αλλαγές που έχουν πραγματοποιηθεί στην app.config ή web.config

Έχουν προστεθεί οι ακόλουθες καταχωρήσεις πρόσθετες ρυθμίσεις παραμέτρων.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

Έχουν προστεθεί οι ακόλουθες ενότητες ρύθμισης παραμέτρων και τη συμβολοσειρά σύνδεσης.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###<a name="your-azure-active-directory-app-was-updated"></a>Ενημερώθηκε την εφαρμογή Azure Active Directory
Την εφαρμογή Azure Active Directory ενημερώθηκε για να συμπεριλάβετε το δικαίωμα *ανάγνωσης καταλόγου δεδομένων* και ένα επιπλέον κλειδί που δημιουργήθηκε το οποίο, στη συνέχεια, χρησιμοποιείται ως το *ida: ClientSecret* στο αρχείο **web.config** .

[Μάθετε περισσότερα σχετικά με το Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
