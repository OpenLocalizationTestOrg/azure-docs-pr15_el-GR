<properties
    pageTitle="Γρήγορα αποτελέσματα .NET Azure AD | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή υπολογιστή με Windows .NET που ενοποιείται με το Azure AD για είσοδο στο και καλεί Azure AD προστατευμένο APIs χρησιμοποιώντας OAuth."
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


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Ενοποίηση του Azure AD σε μια εφαρμογή WPF επιφάνειας εργασίας των Windows

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Εάν αναπτύσσετε μια εφαρμογή υπολογιστή, Azure AD καθιστά απλή και άμεση για να τον έλεγχο ταυτότητας τους χρήστες σας με τους λογαριασμούς υπηρεσίας καταλόγου Active Directory.  Ενεργοποιεί επίσης την εφαρμογή σας για την ασφαλή εκμετάλλευση οποιαδήποτε API που προστατεύονται από Azure AD, όπως τα API Office 365 ή το API Azure web.

Για .NET εγγενή προγράμματα-πελάτες που πρέπει να αποκτήσετε πρόσβαση σε προστατευμένο πόρους, Azure AD παρέχει τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory ή ADAL.  Σκοπός του ADAL κατά τη διάρκεια ζωής της είναι ώστε να μπορείτε εύκολα για την εφαρμογή σας για να λάβετε διακριτικά πρόσβασης.  Για μια επίδειξη απλώς πόσο εύκολο είναι, εδώ θα σας θα δημιουργήσετε μια λίστα εκκρεμών εργασιών WPF .NET εφαρμογή που:

-   Λαμβάνει πρόσβαση διακριτικά για την κλήση του API Azure AD Graph χρησιμοποιεί το [πρωτόκολλο ελέγχου ταυτότητας διακριτικό 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Πραγματοποιεί αναζήτηση σε έναν κατάλογο για τους χρήστες με ένα καθορισμένο ψευδώνυμο.
-   Οι χρήστες πρόσημα ανάληψη.

Για να δημιουργήσετε την πλήρη εφαρμογή εργασία, θα πρέπει να:

2. Καταχωρήστε την εφαρμογή του Azure AD.
3. Εγκατάσταση και ρύθμιση παραμέτρων του ADAL.
5. Χρησιμοποιήστε ADAL για να λάβετε τα διακριτικά από το Azure AD.

Για να ξεκινήσετε, [κάντε λήψη του σκελετό εφαρμογής](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) ή [λήψη ολοκληρωμένο δείγμα](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Θα χρειαστεί επίσης ένα μισθωτή του Azure AD με την οποία μπορείτε να δημιουργήσετε χρήστες και να καταχωρήσετε μια εφαρμογή του.  Εάν δεν έχετε ήδη ένα μισθωτή, [Μάθετε πώς μπορείτε να αποκτήσετε](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. καταχώρηση της εφαρμογής DirectorySearcher*
Για να ενεργοποιήσετε την εφαρμογή σας για να λάβετε τα διακριτικά, θα πρέπει πρώτα να καταχωρήσετε στο μισθωτή σας Azure AD και να εκχωρήσετε το δικαίωμα πρόσβασης το API Azure AD Graph:

-   Πραγματοποιήστε είσοδο στο την πύλη διαχείρισης Azure
-   Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην **Υπηρεσία καταλόγου Active Directory**
-   Επιλέξτε έναν μισθωτή στην οποία θέλετε να καταχωρήσετε την εφαρμογή.
-   Κάντε κλικ στην καρτέλα **εφαρμογές** και κάντε κλικ στην επιλογή **Προσθήκη** στο το σχέδιο κάτω.
-   Ακολουθήστε τις οδηγίες και να δημιουργήσετε μια νέα **Εφαρμογή εγγενής προγράμματος-πελάτη**.
    -   Το **όνομα** της εφαρμογής θα περιγράφει την εφαρμογή σας στους τελικούς χρήστες
    -   Η **Ανακατεύθυνση Uri** είναι ένας συνδυασμός συνδυασμό και συμβολοσειρά που Azure AD θα χρησιμοποιήσετε για να λάβετε απαντήσεις διακριτικού.  Πληκτρολογήστε μια συγκεκριμένη τιμή για την εφαρμογή σας, π.χ. `http://DirectorySearcher`.
-   Όταν ολοκληρώσετε την καταχώρηση, AAD θα εκχωρήσετε την εφαρμογή σας ένα αναγνωριστικό μοναδικό προγράμματος-πελάτη.  Θα χρειαστείτε αυτή την τιμή στις επόμενες ενότητες, επομένως, αντιγράψτε την από την καρτέλα **Ρύθμιση παραμέτρων** .
- Επίσης στην καρτέλα **Ρύθμιση παραμέτρων** , εντοπίστε την ενότητα "Δικαιώματα σε άλλες εφαρμογές".  Για την εφαρμογή "Azure Active Directory", προσθέστε το δικαίωμα **πρόσβασης σας εταιρείας καταλόγου** στην περιοχή **Δικαιώματα με ανάθεση**.  Αυτό θα ενεργοποιήσει την εφαρμογή σας ερώτημα για το API Graph για τους χρήστες.

## <a name="2-install--configure-adal"></a>*2. εγκατάσταση και ρύθμιση παραμέτρων ADAL*
Τώρα που έχετε μια εφαρμογή στο Azure AD, μπορείτε να εγκαταστήσετε ADAL και να γράψετε τον κωδικό που σχετίζονται με την ταυτότητα.  Με τη σειρά για ADAL να μπορούν να επικοινωνούν με Azure AD, πρέπει να παρέχει ορισμένες πληροφορίες σχετικά με την εγγραφή σας εφαρμογή.
-   Ξεκινήστε προσθέτοντας ADAL στο έργο DirectorySearcher χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Στο έργο DirectorySearcher, ανοίξτε `app.config`.  Αντικαταστήστε τις τιμές των στοιχείων στο το `<appSettings>` ενότητα ώστε να αντικατοπτρίζει τις τιμές που εισαγάγει στην πύλη του Azure.  Ο κώδικας θα αναφοράς αυτές τις τιμές κάθε φορά που χρησιμοποιεί ADAL.
    -   Η `ida:Tenant` είναι ο τομέας του μισθωτή του Azure AD, π.χ. contoso.onmicrosoft.com
    -   Η `ida:ClientId` είναι η clientId της εφαρμογής σας που αντιγράψατε από την πύλη.
    -   Η `ida:RedirectUri` είναι η ανακατεύθυνση διεύθυνσης url που έχουν καταχωρηθεί στην πύλη.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Χρησιμοποιήστε ADAL για να λάβετε τα διακριτικά από AAD*
Η βασική αρχή πίσω από ADAL είναι ότι κάθε φορά που την εφαρμογή σας χρειάζεται ένα διακριτικό πρόσβασης, απλώς καλεί `authContext.AcquireTokenAsync(...)`, και ADAL κάνει τα υπόλοιπα.  

-   Στο το `DirectorySearcher` το έργο, Άνοιγμα `MainWindow.xaml.cs` και εντοπίστε το `MainWindow()` μέθοδο.  Το πρώτο βήμα είναι να προετοιμάσει την εφαρμογή `AuthenticationContext` -ADAL του πρωτεύοντος τάξης.  Αυτό είναι όπου περάσετε ADAL τις συντεταγμένες που χρειάζονται για την επικοινωνία με το Azure AD και ενημερώστε το cache τα διακριτικά με τον τρόπο.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- Τώρα εντοπίστε το `Search(...)` μέθοδο, η οποία θα είναι δυνατή όταν το χρήστη cliks την "Αναζήτηση", κουμπί στο περιβάλλον εργασίας Χρήστη της εφαρμογής.  Αυτή η μέθοδος κάνει ένα αίτημα GET για το API Azure AD Graph ερώτημα για τους χρήστες των οποίων UPN αρχίζει με τον όρο αναζήτησης που δίνεται.  Αλλά για να υποβάλετε ένα ερώτημα το API Graph, πρέπει να συμπεριλάβετε μια access_token στο το `Authorization` κεφαλίδα της αίτησης - αυτό είναι ADAL μπορεί να βοηθήσει.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- Όταν η εφαρμογή σας ζητάει ενός διακριτικού καλώντας `AcquireTokenAsync(...)`, ADAL θα επιχειρήσει να επιστρέψει ένα διακριτικό χωρίς να ζητήσετε από το χρήστη για τα διαπιστευτήρια.  Εάν ADAL Καθορίζει ότι ο χρήστης πρέπει να εισέλθετε για να λάβετε ένα διακριτικό, θα εμφανιστεί ένα παράθυρο διαλόγου σύνδεση, συλλογής τα διαπιστευτήρια του χρήστη και επιστροφή ενός διακριτικού κατά την επιτυχή έλεγχο ταυτότητας.  Εάν το ADAL είναι δυνατό να επιστρέψει ενός διακριτικού για οποιονδήποτε λόγο, θα εμφανίσουν μια `AdalException`.
- Σημειώστε ότι το `AuthenticationResult` αντικείμενο περιέχει ένα `UserInfo` αντικείμενο το οποίο μπορεί να χρησιμοποιηθεί για τη συλλογή πληροφοριών της εφαρμογής σας μπορεί να χρειαστεί.  Στο το DirectorySearcher, `UserInfo` χρησιμοποιείται για την προσαρμογή περιβάλλοντος εργασίας Χρήστη της εφαρμογής με το αναγνωριστικό του χρήστη.

- Όταν ο χρήστης κάνει κλικ στο κουμπί "Έξοδος", θέλουμε να βεβαιωθείτε ότι η επόμενη κλήση `AcquireTokenAsync(...)` θα ζητήσετε από το χρήστη για να πραγματοποιήσετε είσοδο.  Με ADAL, αυτό είναι τόσο εύκολη όσο η εκκαθάριση του cache του διακριτικού:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- Ωστόσο, εάν ο χρήστης δεν κάνει κλικ στο κουμπί "Έξοδος", θα θέλετε να διατηρήσετε την περίοδο λειτουργίας του χρήστη για την επόμενη φορά που εκτελούν το DirectorySearcher.  Όταν ξεκινά η εφαρμογή, μπορείτε να ελέγξετε διακριτικού cache του ADAL για μια υπάρχουσα διακριτικό και να ενημερώσετε το περιβάλλον εργασίας Χρήστη ανάλογα.  Στο το `CheckForCachedToken()` μέθοδο, κάνετε μια κλήση για να `AcquireTokenAsync(...)`, που περνά μέσα σε αυτήν τη στιγμή το `PromptBehavior.Never` παραμέτρου.  `PromptBehavior.Never`θα σας ενημερώσει ADAL ότι ο χρήστης δεν θα πρέπει να σας ζητηθεί για είσοδο στο και ADAL αντί για αυτό θα πρέπει να εμφανίσουν εξαίρεση, εάν είναι δυνατό να επιστρέψει μια διακριτικού.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Συγχαρητήρια! Τώρα έχετε μια εργασία εφαρμογή .NET WPF που έχει τη δυνατότητα να ελέγχουν την ταυτότητα χρηστών, καλέστε με ασφάλεια APIs Web χρησιμοποιώντας διακριτικό 2.0 και λήψη βασικών πληροφοριών σχετικά με το χρήστη.  Εάν δεν το έχετε κάνει ήδη, τώρα είναι η ώρα για τη συμπλήρωση του μισθωτή με ορισμένων χρηστών.  Εκτελέστε την εφαρμογή DirectorySearcher και συνδεθείτε με έναν από αυτούς τους χρήστες.  Αναζήτηση για άλλους χρήστες με βάση τους UPN.  Κλείστε την εφαρμογή και εκτελέστε ξανά.  Παρατηρήστε πώς παραμένει ανέπαφος περιόδου λειτουργίας του χρήστη.  Πραγματοποιήστε έξοδο και συνδεθείτε ξανά με έναν άλλο χρήστη.

ADAL διευκολύνει να ενσωματώσετε όλες αυτές τις συνήθεις δυνατότητες ταυτότητας στην εφαρμογή σας.  Το αναλαμβάνει όλες τις εργασίες dirty για εσάς: Διαχείριση cache, η υποστήριξη πρωτοκόλλου διακριτικό, παρουσίαση στο χρήστη με μια σύνδεση περιβάλλοντος εργασίας Χρήστη, η ανανέωση έχει λήξει διακριτικά και πολλά άλλα.  Όλα όσα χρειάζεται να γνωρίζετε είναι μια μεμονωμένη κλήση API, `authContext.AcquireTokenAsync(...)`.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) παρέχεται [εδώ](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Τώρα, μπορείτε να μετακινήσετε επιπλέον σενάρια.  Εάν θέλετε, μπορείτε να δοκιμάσετε:

[Ασφαλούς Web .NET API με Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
