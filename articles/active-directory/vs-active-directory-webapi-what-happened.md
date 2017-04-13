<properties
    pageTitle="Τι απέγινε το έργο μου WebApi (υπηρεσία συνδεδεμένοι Visual Studio Azure Active Directory) | Microsoft Azure "
    description="Περιγράφει τι συμβαίνει στο έργο σας MVC WebApi συνδέεστε με Azure AD με χρήση του Visual Studio"
  services="active-directory"
    documentationCenter=""
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

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Τι απέγινε το έργο μου WebApi (υπηρεσία συνδεδεμένοι Visual Studio Azure Active Directory)

> [AZURE.SELECTOR]
> - [Γρήγορα αποτελέσματα](vs-active-directory-webapi-getting-started.md)
> - [Τι έγινε](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>Έχουν προστεθεί αναφορές

###<a name="nuget-package-references"></a>Αναφορές πακέτου NuGet

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>Αναφορές .NET

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>Αλλαγές κώδικα

###<a name="code-files-were-added-to-your-project"></a>Αρχεία κώδικα έχουν προστεθεί στο έργο σας

Μια κλάση εκκίνησης ελέγχου ταυτότητας, **App_Start/Startup.Auth.cs** προστέθηκε στο έργο σας που περιέχουν λογικής εκκίνησης για τον έλεγχο ταυτότητας Azure AD.

###<a name="startup-code-was-added-to-your-project"></a>Κωδικός εκκίνησης προστέθηκε στο έργο σας

Εάν είχατε ήδη μια κλάση εκκίνησης στο έργο σας, η μέθοδος **ρύθμισης παραμέτρων** ενημερώθηκε για να συμπεριλάβετε μια κλήση σε `ConfigureAuth(app)`. Διαφορετικά, κλάση της εκκίνησης προστέθηκε στο έργο σας.


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Το αρχείο app.config ή web.config έχει νέες τιμές παραμέτρων.

Έχουν προστεθεί οι ακόλουθες καταχωρήσεις ρύθμισης παραμέτρων.
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Azure AD εφαρμογής που δημιουργήθηκε

Εφαρμογή του Azure AD δημιουργήθηκε στον κατάλογο που έχετε επιλέξει στον οδηγό.

[Μάθετε περισσότερα σχετικά με το Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Εάν να γίνει μεταβίβαση ελέγχου *Απενεργοποίηση του ελέγχου ταυτότητας των μεμονωμένων λογαριασμών χρήστη*, τις πρόσθετες αλλαγές που έγιναν για το έργο μου;
Αναφορές πακέτου NuGet έχουν καταργηθεί και έχουν καταργηθεί και αντίγραφα ασφαλείας για τα αρχεία. Ανάλογα με την κατάσταση του έργου σας, ίσως χρειαστεί να καταργήσετε αρχεία ή πρόσθετες αναφορές με μη αυτόματο τρόπο ή να τροποποιήσετε κώδικα ανάλογα με την περίπτωση.

###<a name="nuget-package-references-removed-for-those-present"></a>Αναφορές πακέτου NuGet καταργηθεί (για αυτά παρουσίαση)

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>Αρχεία κώδικα αντίγραφα ασφαλείας και καταργηθεί (για αυτά παρουσίαση)

Κάθε ένα από τα ακόλουθα αρχεία ήταν αντίγραφα ασφαλείας και να έχει καταργηθεί από το έργο. Τα αρχεία αντιγράφων ασφαλείας βρίσκονται σε έναν φάκελο 'Δημιουργία αντιγράφων ασφαλείας' στον ριζικό κατάλογο του καταλόγου του έργου.

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>Κωδικός αντίγραφα ασφαλείας (για αυτά παρουσίαση)

Κάθε ένα από τα ακόλουθα αρχεία δημιουργήθηκε αντίγραφο ασφαλείας πριν να αντικατασταθούν. Τα αρχεία αντιγράφων ασφαλείας βρίσκονται σε έναν φάκελο 'Δημιουργία αντιγράφων ασφαλείας' στον ριζικό κατάλογο του καταλόγου του έργου.

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Εάν να γίνει μεταβίβαση ελέγχου *Ανάγνωση καταλόγου δεδομένων*, τις πρόσθετες αλλαγές που έγιναν για το έργο μου;

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Πρόσθετες αλλαγές που έχουν πραγματοποιηθεί στην app.config ή web.config

Έχουν προστεθεί οι ακόλουθες καταχωρήσεις πρόσθετες ρυθμίσεις παραμέτρων.

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Ενημερώθηκε την εφαρμογή Azure Active Directory
Την εφαρμογή Azure Active Directory ενημερώθηκε για να συμπεριλάβετε το δικαίωμα *ανάγνωσης καταλόγου δεδομένων* και ένα επιπλέον κλειδί δημιουργήθηκε που χρησιμοποιήθηκε, στη συνέχεια, το *Ida: τον κωδικό πρόσβασης* στο το `web.config` αρχείου.

[Μάθετε περισσότερα σχετικά με το Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
