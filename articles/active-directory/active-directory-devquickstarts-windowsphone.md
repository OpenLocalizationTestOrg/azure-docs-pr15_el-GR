<properties
    pageTitle="Γρήγορα αποτελέσματα Azure AD Windows Phone | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή Windows Phone που ενοποιείται με το Azure AD για είσοδο στο και καλεί Azure AD προστατευμένο APIs χρησιμοποιώντας OAuth."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>



# <a name="integrate-azure-ad-with-a-windows-phone-app"></a>Ενοποίηση του Azure AD με μια εφαρμογή για Windows Phone

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Εάν αναπτύσσετε μια εφαρμογή για Windows Phone 8.1, Azure AD καθιστά απλή και άμεση για να τον έλεγχο ταυτότητας τους χρήστες σας με τους λογαριασμούς υπηρεσίας καταλόγου Active Directory.  Ενεργοποιεί επίσης την εφαρμογή σας για την ασφαλή εκμετάλλευση οποιαδήποτε API που προστατεύονται από Azure AD, όπως τα API Office 365 ή το API Azure web.

> [AZURE.NOTE] Αυτό το δείγμα κώδικα χρησιμοποιεί ADAL v2.0.  Για την πιο πρόσφατη τεχνολογία, ενδέχεται να θέλετε να δοκιμάσετε αντί για αυτό το [πρόγραμμα εκμάθησης Universal Windows χρησιμοποιώντας ADAL v3.0](active-directory-devquickstarts-windowsstore.md).  Εάν δημιουργείτε στην πραγματικότητα μια εφαρμογή για Windows Phone 8.1, αυτή είναι στο σωστό μέρος.  Υποστηρίζεται ακόμη πλήρως ADAL v2.0 και τον προτεινόμενο τρόπο ανάπτυξης εφαρμογών agianst Windows Phone 8.1 χρησιμοποιεί Azure AD.

Για .NET εγγενή προγράμματα-πελάτες που πρέπει να αποκτήσετε πρόσβαση σε προστατευμένο πόρους, Azure AD παρέχει τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory ή ADAL.  Σκοπός του ADAL κατά τη διάρκεια ζωής της είναι ώστε να μπορείτε εύκολα για την εφαρμογή σας για να λάβετε διακριτικά πρόσβασης.  Για μια επίδειξη απλώς πόσο εύκολο είναι, εδώ θα σας θα δημιουργήσετε μια εφαρμογή Windows Phone 8.1 "καταλόγου Searcher" που:

-   Λαμβάνει πρόσβαση διακριτικά για την κλήση του API Azure AD Graph χρησιμοποιεί το [πρωτόκολλο ελέγχου ταυτότητας διακριτικό 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Πραγματοποιεί αναζήτηση σε έναν κατάλογο για τους χρήστες με μια δεδομένη UPN.
-   Οι χρήστες πρόσημα ανάληψη.

Για να δημιουργήσετε την πλήρη εφαρμογή εργασία, θα πρέπει να:

2. Καταχωρήστε την εφαρμογή του Azure AD.
3. Εγκατάσταση και ρύθμιση παραμέτρων του ADAL.
5. Χρησιμοποιήστε ADAL για να λάβετε τα διακριτικά από το Azure AD.

Για να ξεκινήσετε, [κάντε λήψη ενός έργου σκελετό](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) ή [λήψη ολοκληρωμένο δείγμα](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Κάθε είναι μια λύση Visual Studio 2013.  Θα χρειαστεί επίσης ένα μισθωτή του Azure AD με την οποία μπορείτε να δημιουργήσετε χρήστες και να καταχωρήσετε μια εφαρμογή του.  Εάν δεν έχετε ήδη ένα μισθωτή, [Μάθετε πώς μπορείτε να αποκτήσετε](active-directory-howto-tenant.md).

## <a name="1-register-the-directory-searcher-application"></a>*1. καταχώρηση της εφαρμογής Searcher καταλόγου*
Για να ενεργοποιήσετε την εφαρμογή σας για να λάβετε τα διακριτικά, θα πρέπει πρώτα να καταχωρήσετε στο μισθωτή σας Azure AD και να εκχωρήσετε το δικαίωμα πρόσβασης το API Azure AD Graph:

-   Πραγματοποιήστε είσοδο στο στην [πύλη διαχείρισης Azure](https://manage.windowsazure.com)
-   Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην **Υπηρεσία καταλόγου Active Directory**
-   Επιλέξτε έναν μισθωτή στην οποία θέλετε να καταχωρήσετε την εφαρμογή.
-   Κάντε κλικ στην καρτέλα **εφαρμογές** και κάντε κλικ στην επιλογή **Προσθήκη** στο το σχέδιο κάτω.
-   Ακολουθήστε τις οδηγίες και να δημιουργήσετε μια νέα **Εφαρμογή εγγενούς προγράμματος-πελάτη**.
    -   Το **όνομα** της εφαρμογής θα περιγράφει την εφαρμογή σας στους τελικούς χρήστες
    -   Η **Ανακατεύθυνση Uri** είναι ένας συνδυασμός συνδυασμό και συμβολοσειρά που Azure AD θα χρησιμοποιήσετε για να λάβετε απαντήσεις διακριτικού.  Καταχωρήστε μια τιμή κράτησης θέσης για τώρα, π.χ. `http://DirectorySearcher`.  Θα σας θα αντικαταστήσει αυτήν την τιμή αργότερα.
-   Όταν ολοκληρώσετε την καταχώρηση, AAD θα εκχωρήσετε την εφαρμογή σας ένα αναγνωριστικό μοναδικό προγράμματος-πελάτη.  Θα χρειαστείτε αυτή την τιμή στις επόμενες ενότητες, επομένως, αντιγράψτε την από την καρτέλα **Ρύθμιση παραμέτρων** .
- Επίσης στην καρτέλα **Ρύθμιση παραμέτρων** , εντοπίστε την ενότητα "Δικαιώματα σε άλλες εφαρμογές".  Για την εφαρμογή "Azure Active Directory", προσθέστε το δικαίωμα **πρόσβασης σας εταιρείας καταλόγου** στην περιοχή **Δικαιώματα με ανάθεση**.  Αυτό θα ενεργοποιήσει την εφαρμογή σας ερώτημα για το API Graph για τους χρήστες.

## <a name="2-install--configure-adal"></a>*2. εγκατάσταση και ρύθμιση παραμέτρων ADAL*
Τώρα που έχετε μια εφαρμογή στο Azure AD, μπορείτε να εγκαταστήσετε ADAL και να γράψετε τον κωδικό που σχετίζονται με την ταυτότητα.  Με τη σειρά για ADAL να μπορούν να επικοινωνούν με Azure AD, πρέπει να παρέχει ορισμένες πληροφορίες σχετικά με την εγγραφή σας εφαρμογή.
-   Ξεκινήστε προσθέτοντας ADAL στο έργο DirectorySearcher χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Στο έργο DirectorySearcher, ανοίξτε `MainPage.xaml.cs`.  Αντικαταστήστε τις τιμές του `Config Values` περιοχή ώστε να αντικατοπτρίζει τις τιμές που εισαγάγει στην πύλη του Azure.  Ο κώδικας θα αναφοράς αυτές τις τιμές κάθε φορά που χρησιμοποιεί ADAL.
    -   Η `tenant` είναι ο τομέας του μισθωτή του Azure AD, π.χ. contoso.onmicrosoft.com
    -   Η `clientId` είναι η clientId της εφαρμογής σας που αντιγράψατε από την πύλη.
-   Θα πρέπει τώρα να Ανακαλύψτε το uri επιστροφής κλήσης για την εφαρμογή σας Windows Phone.  Ρύθμιση ένα σημείο διακοπής σε αυτήν τη γραμμή στο το `MainPage` μέθοδο:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Εκτελέστε την εφαρμογή και Εξοικονομήστε αντιγράψτε την τιμή του `redirectUri` όταν πατήσετε το σημείο διακοπής.  Αυτό θα πρέπει να μοιάζει με

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Επιστρέψτε στην καρτέλα **Ρύθμιση παραμέτρων** της εφαρμογής σας στην πύλη διαχείρισης του Azure, αντικαταστήστε την τιμή από το **RedirectUri** με αυτήν την τιμή.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Χρησιμοποιήστε ADAL για να λάβετε τα διακριτικά από AAD*
Η βασική αρχή πίσω από ADAL είναι ότι κάθε φορά που την εφαρμογή σας χρειάζεται ένα διακριτικό πρόσβασης, απλώς καλεί `authContext.AcquireToken(…)`, και ADAL κάνει τα υπόλοιπα.  

-   Το πρώτο βήμα είναι να προετοιμάσει την εφαρμογή `AuthenticationContext` -ADAL του πρωτεύοντος τάξης.  Αυτό είναι το σημείο όπου περάσετε ADAL τις συντεταγμένες που χρειάζονται για την επικοινωνία με το Azure AD και ενημερώστε το cache τα διακριτικά με τον τρόπο.

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

- Τώρα εντοπίστε το `Search(...)` μέθοδο, η οποία θα είναι δυνατή όταν το cliks χρήστη "Αναζήτηση", κουμπί στο περιβάλλον εργασίας Χρήστη της εφαρμογής.  Αυτή η μέθοδος κάνει ένα αίτημα GET για το API Azure AD Graph ερώτημα για τους χρήστες των οποίων UPN αρχίζει με τον όρο αναζήτησης που δίνεται.  Αλλά για να υποβάλετε ένα ερώτημα το API Graph, πρέπει να συμπεριλάβετε μια access_token στο το `Authorization` κεφαλίδα της αίτησης - αυτό είναι ADAL μπορεί να βοηθήσει.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
- Εάν είναι απαραίτητο αλληλεπιδραστικών ο έλεγχος ταυτότητας, ADAL θα χρησιμοποιήσει του Windows Phone Web ελέγχου ταυτότητας Broker (βιβλίου διευθύνσεων των Windows) και [μοντέλο συνέχισης](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) για να εμφανίσετε το Azure AD Πραγματοποιήστε είσοδο στη σελίδα.  Όταν ο χρήστης πραγματοποιεί είσοδο, την εφαρμογή σας πρέπει να μεταβιβάζουν ADAL τα αποτελέσματα της επικοινωνίας βιβλίου διευθύνσεων των Windows.  Αυτό είναι τόσο απλή όσο η εφαρμογή του `ContinueWebAuthentication` περιβάλλοντος εργασίας:

```C#
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

- Τώρα είναι ώρα να χρησιμοποιήσετε την `AuthenticationResult` που επιστρέφονται ADAL για την εφαρμογή σας.  Στο το `QueryGraph(...)` επιστροφή κλήσης, επισυνάψτε το access_token αποκτήσατε για την αίτηση GET στην κεφαλίδα της εξουσιοδότησης:

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
- Μπορείτε επίσης να χρησιμοποιήσετε το `AuthenticationResult` αντικειμένου για να εμφανίσετε πληροφορίες σχετικά με το χρήστη στην εφαρμογή. Στο το `QueryGraph(...)` μέθοδο, χρησιμοποιήστε το αποτέλεσμα για να εμφανίσετε το αναγνωριστικό του χρήστη στη σελίδα:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Τέλος, μπορείτε να χρησιμοποιήσετε ADAL για είσοδο στο χρήστη εκτός καθώς και την εφαρμογή.  Όταν ο χρήστης κάνει κλικ στο κουμπί "Έξοδος", θέλουμε να βεβαιωθείτε ότι η επόμενη κλήση `AcquireTokenSilentAsync(...)` θα αποτύχει.  Με ADAL, αυτό είναι τόσο εύκολη όσο η εκκαθάριση του cache του διακριτικού:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Συγχαρητήρια! Τώρα έχετε μια εργασία Εφαρμογή Windows Phone που έχει τη δυνατότητα να ελέγχουν την ταυτότητα χρηστών, καλέστε με ασφάλεια APIs Web χρησιμοποιώντας διακριτικό 2.0 και λήψη βασικών πληροφοριών σχετικά με το χρήστη.  Εάν δεν το έχετε κάνει ήδη, τώρα είναι η ώρα για τη συμπλήρωση του μισθωτή με ορισμένων χρηστών.  Εκτελέστε την εφαρμογή DirectorySearcher και συνδεθείτε με έναν από αυτούς τους χρήστες.  Αναζήτηση για άλλους χρήστες με βάση τους UPN.  Κλείστε την εφαρμογή και εκτελέστε ξανά.  Παρατηρήστε πώς παραμένει ανέπαφος περιόδου λειτουργίας του χρήστη.  Πραγματοποιήστε έξοδο και συνδεθείτε ξανά με έναν άλλο χρήστη.

ADAL διευκολύνει να ενσωματώσετε όλες αυτές τις συνήθεις δυνατότητες ταυτότητας στην εφαρμογή σας.  Το αναλαμβάνει όλες τις εργασίες dirty για εσάς: Διαχείριση cache, η υποστήριξη πρωτοκόλλου διακριτικό, παρουσίαση στο χρήστη με μια σύνδεση περιβάλλοντος εργασίας Χρήστη, η ανανέωση έχει λήξει διακριτικά και πολλά άλλα.  Όλα όσα χρειάζεται να γνωρίζετε είναι μια μεμονωμένη κλήση API, `authContext.AcquireToken*(…)`.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) παρέχεται [εδώ](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Τώρα, μπορείτε να μετακινήσετε σενάρια επιπλέον ταυτότητα.  Εάν θέλετε, μπορείτε να δοκιμάσετε:

[Ασφαλούς Web .NET API με Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]