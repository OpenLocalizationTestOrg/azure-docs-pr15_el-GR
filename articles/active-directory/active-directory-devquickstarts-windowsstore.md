<properties
    pageTitle="Ο χώρος αποθήκευσης του Windows Azure AD γρήγορα αποτελέσματα | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή του Windows Store που ενοποιείται με το Azure AD για είσοδο στο και καλεί Azure AD προστατευμένο APIs χρησιμοποιώντας OAuth."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>Ενοποίηση του Azure AD με μια εφαρμογή του Windows Store

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Εάν αναπτύσσετε μια εφαρμογή για το Windows Store, Azure AD καθιστά απλή και άμεση για να τον έλεγχο ταυτότητας τους χρήστες σας με τους λογαριασμούς υπηρεσίας καταλόγου Active Directory.  Ενεργοποιεί επίσης την εφαρμογή σας για την ασφαλή εκμετάλλευση οποιαδήποτε API που προστατεύονται από Azure AD, όπως τα API Office 365 ή το API Azure web.

Για τις εφαρμογές υπολογιστή του Windows Store που πρέπει να αποκτήσετε πρόσβαση σε προστατευμένο πόρους, Azure AD παρέχει τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory ή ADAL.  Σκοπός του ADAL κατά τη διάρκεια ζωής της είναι ώστε να μπορείτε εύκολα για την εφαρμογή σας για να λάβετε διακριτικά πρόσβασης.  Για μια επίδειξη απλώς πόσο εύκολο είναι, εδώ θα σας θα δημιουργήσετε μια εφαρμογή του Windows Store "Καταλόγου Searcher" που:

-   Λαμβάνει πρόσβαση διακριτικά για την κλήση του API Azure AD Graph χρησιμοποιεί το [πρωτόκολλο ελέγχου ταυτότητας διακριτικό 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Πραγματοποιεί αναζήτηση σε έναν κατάλογο για τους χρήστες με μια δεδομένη UPN.
-   Οι χρήστες πρόσημα ανάληψη.

Για να δημιουργήσετε την πλήρη εφαρμογή εργασία, θα πρέπει να:

2. Καταχωρήστε την εφαρμογή του Azure AD.
3. Εγκατάσταση και ρύθμιση παραμέτρων του ADAL.
5. Χρησιμοποιήστε ADAL για να λάβετε τα διακριτικά από το Azure AD.

Για να ξεκινήσετε, [κάντε λήψη ενός έργου σκελετό](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) ή [λήψη ολοκληρωμένο δείγμα](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Κάθε είναι μια λύση Visual Studio 2015.  Θα χρειαστεί επίσης ένα μισθωτή του Azure AD με την οποία μπορείτε να δημιουργήσετε χρήστες και να καταχωρήσετε μια εφαρμογή του.  Εάν δεν έχετε ήδη ένα μισθωτή, [Μάθετε πώς μπορείτε να αποκτήσετε](active-directory-howto-tenant.md).

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
- Επίσης στην καρτέλα **Ρύθμιση παραμέτρων** , εντοπίστε την ενότητα "Δικαιώματα σε άλλες εφαρμογές".  Για την εφαρμογή "Azure Active Directory", προσθέστε τα δικαιώματα **πρόσβασης στον κατάλογο ως ο χρήστης πραγματοποιήσει στο** στην περιοχή **Δικαιώματα με ανάθεση**.  Αυτό θα ενεργοποιήσει την εφαρμογή σας ερώτημα για το API Graph για τους χρήστες.

## <a name="2-install--configure-adal"></a>*2. εγκατάσταση και ρύθμιση παραμέτρων ADAL*
Τώρα που έχετε μια εφαρμογή στο Azure AD, μπορείτε να εγκαταστήσετε ADAL και να γράψετε τον κωδικό που σχετίζονται με την ταυτότητα.  Με τη σειρά για ADAL να μπορούν να επικοινωνούν με Azure AD, πρέπει να παρέχει ορισμένες πληροφορίες σχετικά με την εγγραφή σας εφαρμογή.
-   Ξεκινήστε προσθέτοντας ADAL στο έργο DirectorySearcher χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Στο έργο DirectorySearcher, ανοίξτε `MainPage.xaml.cs`.  Αντικαταστήστε τις τιμές του `Config Values` περιοχή ώστε να αντικατοπτρίζει τις τιμές που εισαγάγει στην πύλη του Azure.  Ο κώδικας θα αναφοράς αυτές τις τιμές κάθε φορά που χρησιμοποιεί ADAL.
    -   Η `tenant` είναι ο τομέας του μισθωτή του Azure AD, π.χ. contoso.onmicrosoft.com
    -   Η `clientId` είναι η clientId της εφαρμογής σας που αντιγράψατε από την πύλη.
-   Θα πρέπει τώρα να Ανακαλύψτε το uri επιστροφής κλήσης για την εφαρμογή Windows Store.  Ρύθμιση ένα σημείο διακοπής σε αυτήν τη γραμμή στο το `MainPage` μέθοδο:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Δημιουργήστε τη λύση, και βεβαιωθείτε ότι όλες οι αναφορές πακέτου επαναφέρονται.  Εάν λείπουν πακέτων, άνοιγμα του της διαχείρισης πακέτου Nuget και επαναφέρετε τα πακέτα.
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

    authContext = new AuthenticationContext(authority);
}
```

- Τώρα εντοπίστε το `Search(...)` μέθοδο, η οποία θα είναι δυνατή όταν ο χρήστης κάνει κλικ στο κουμπί "Αναζήτηση" στο περιβάλλον εργασίας Χρήστη της εφαρμογής.  Αυτή η μέθοδος κάνει ένα αίτημα GET για το API Azure AD Graph ερώτημα για τους χρήστες των οποίων UPN αρχίζει με τον όρο αναζήτησης που δίνεται.  Αλλά για να υποβάλετε ένα ερώτημα το API Graph, πρέπει να συμπεριλάβετε μια access_token στο το `Authorization` κεφαλίδα της αίτησης - αυτό είναι ADAL μπορεί να βοηθήσει.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- Όταν η εφαρμογή σας ζητάει ενός διακριτικού καλώντας `AcquireTokenAsync(...)`, ADAL θα επιχειρήσει να επιστρέψει ενός διακριτικού χωρίς να ζητήσετε από το χρήστη για τα διαπιστευτήρια.  Εάν ADAL Καθορίζει ότι ο χρήστης πρέπει να εισέλθετε για να λάβετε ένα διακριτικό, θα εμφανιστεί ένα παράθυρο διαλόγου σύνδεση, συλλογής τα διαπιστευτήρια του χρήστη και επιστροφή ενός διακριτικού κατά την επιτυχή έλεγχο ταυτότητας.  Εάν δεν μπορεί να επιστρέψει ενός διακριτικού για οποιονδήποτε λόγο, ADAL το `AuthenticationResult` κατάσταση θα είναι ένα σφάλμα.

- Τώρα είναι ώρα να χρησιμοποιήσετε το το access_token που μόλις αποκτήσατε.  Επίσης με το `Search(...)` μέθοδο, επισυνάψτε το διακριτικό στην πρόσκληση σε γράφημα API ΓΡΉΓΟΡΑ στην κεφαλίδα της εξουσιοδότησης:

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- Μπορείτε επίσης να χρησιμοποιήσετε το `AuthenticationResult` αντικειμένου για να εμφανίσετε πληροφορίες σχετικά με το χρήστη στην εφαρμογή σας, όπως το αναγνωριστικό του χρήστη:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Τέλος, μπορείτε να χρησιμοποιήσετε ADAL για είσοδο στο χρήστη εκτός καθώς και την εφαρμογή.  Όταν ο χρήστης κάνει κλικ στο κουμπί "Έξοδος", θέλουμε να βεβαιωθείτε ότι η επόμενη κλήση `AcquireTokenAsync(...)` θα εμφανιστεί ένα σύμβολο στην προβολή.  Με ADAL, αυτό είναι τόσο εύκολη όσο η εκκαθάριση του cache του διακριτικού:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Συγχαρητήρια! Τώρα έχετε μια εργασία Εφαρμογή Windows Store που έχει τη δυνατότητα να ελέγχουν την ταυτότητα χρηστών, καλέστε με ασφάλεια APIs Web χρησιμοποιώντας διακριτικό 2.0 και λήψη βασικών πληροφοριών σχετικά με το χρήστη.  Εάν δεν το έχετε κάνει ήδη, τώρα είναι η ώρα για τη συμπλήρωση του μισθωτή με ορισμένων χρηστών.  Εκτελέστε την εφαρμογή DirectorySearcher και συνδεθείτε με έναν από αυτούς τους χρήστες.  Αναζήτηση για άλλους χρήστες με βάση τους UPN.  Κλείστε την εφαρμογή και εκτελέστε ξανά.  Παρατηρήστε πώς παραμένει ανέπαφος περιόδου λειτουργίας του χρήστη.  Πραγματοποιήστε έξοδο (κάνοντας δεξί κλικ για να εμφανίσετε τη γραμμή κάτω) και συνδεθείτε ξανά με έναν άλλο χρήστη.

ADAL διευκολύνει να ενσωματώσετε όλες αυτές τις συνήθεις δυνατότητες ταυτότητας στην εφαρμογή σας.  Το αναλαμβάνει όλες τις εργασίες dirty για εσάς: Διαχείριση cache, η υποστήριξη πρωτοκόλλου διακριτικό, παρουσίαση στο χρήστη με μια σύνδεση περιβάλλοντος εργασίας Χρήστη, η ανανέωση έχει λήξει διακριτικά και πολλά άλλα.  Όλα όσα χρειάζεται να γνωρίζετε είναι μια μεμονωμένη κλήση API, `authContext.AcquireToken*(…)`.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) παρέχεται [εδώ](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Τώρα, μπορείτε να μετακινήσετε σενάρια επιπλέον ταυτότητα.  Εάν θέλετε, μπορείτε να δοκιμάσετε:

[Ασφαλούς Web .NET API με Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
