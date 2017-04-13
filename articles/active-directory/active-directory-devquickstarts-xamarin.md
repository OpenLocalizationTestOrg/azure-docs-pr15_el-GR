<properties
    pageTitle="Azure AD Xamarin γρήγορα αποτελέσματα | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή Xamarin που ενοποιείται με το Azure AD για είσοδο στο και καλεί Azure AD προστατευμένο APIs χρησιμοποιώντας OAuth."
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Ενοποίηση του Azure AD σε μια εφαρμογή Xamarin

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin σάς επιτρέπει να γράφετε εφαρμογές για κινητές συσκευές σε C# που μπορεί να εκτελείται σε iOS, Android και Windows (κινητές συσκευές και υπολογιστές). Εάν δημιουργείτε μια εφαρμογή χρησιμοποιώντας Xamarin, Azure AD καθιστά απλή και άμεση για να τον έλεγχο ταυτότητας τους χρήστες σας με τους λογαριασμούς υπηρεσίας καταλόγου Active Directory. Ενεργοποιεί επίσης την εφαρμογή σας για την ασφαλή εκμετάλλευση οποιαδήποτε API που προστατεύονται από Azure AD, όπως τα API Office 365 ή το API Azure web.

Για τις εφαρμογές Xamarin που πρέπει να αποκτήσετε πρόσβαση σε προστατευμένο πόρους, Azure AD παρέχει τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory ή ADAL. Σκοπός του ADAL κατά τη διάρκεια ζωής της είναι ώστε να μπορείτε εύκολα για την εφαρμογή σας για να λάβετε διακριτικά πρόσβασης. Για μια επίδειξη απλώς πόσο εύκολο είναι, εδώ θα σας θα δημιουργήσετε μια εφαρμογή "Καταλόγου Searcher" που:

-   Εκτελείται σε iOS, Android, επιφάνεια εργασίας των Windows, Windows Phone και Windows Store.
- Χρησιμοποιεί τη βιβλιοθήκη μία φορητή κλάσης (PCL) για να ελέγχουν την ταυτότητα χρηστών και να λάβετε τα διακριτικά για το API Azure AD Graph
-   Πραγματοποιεί αναζήτηση σε έναν κατάλογο για τους χρήστες με μια δεδομένη UPN.

Για να δημιουργήσετε την πλήρη εφαρμογή εργασία, θα πρέπει να:

2. Ρυθμίστε το περιβάλλον ανάπτυξης Xamarin
2. Καταχωρήστε την εφαρμογή του Azure AD.
3. Εγκατάσταση και ρύθμιση παραμέτρων του ADAL.
5. Χρησιμοποιήστε ADAL για να λάβετε τα διακριτικά από το Azure AD.

Για να ξεκινήσετε, [κάντε λήψη ενός έργου σκελετό](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) ή [λήψη ολοκληρωμένο δείγμα](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Κάθε είναι μια λύση Visual Studio 2013. Θα χρειαστεί επίσης ένα μισθωτή του Azure AD με την οποία μπορείτε να δημιουργήσετε χρήστες και να καταχωρήσετε μια εφαρμογή του. Εάν δεν έχετε ήδη ένα μισθωτή, [Μάθετε πώς μπορείτε να αποκτήσετε](active-directory-howto-tenant.md).

## <a name="0-set-up-your-xamarin-development-environment"></a>*0. ρυθμίσετε το περιβάλλον ανάπτυξης Xamarin*
Επειδή αυτό το πρόγραμμα εκμάθησης περιλαμβάνει έργων για iOS, Android και Windows, θα χρειαστεί Visual Studio και Xamarin μαζί. Για να δημιουργήσετε το περιβάλλον είναι απαραίτητο, ακολουθήστε τις οδηγίες ολοκλήρωση [εγκατάστασης και εγκατάσταση για Visual Studio και Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) στο MSDN. Αυτές τις οδηγίες περιλαμβάνουν υλικό που μπορείτε να δείτε την για να μάθετε περισσότερα σχετικά με την Xamarin ενώ περιμένετε για τα προγράμματα εγκατάστασης για να ολοκληρωθεί. 

Αφού έχετε ολοκληρώσει τη διαμόρφωση είναι απαραίτητο, ανοίξτε τη λύση στο Visual Studio για να ξεκινήσετε. Θα βρείτε έξι έργα: πέντε συγκεκριμένης πλατφόρμας έργα και μία βιβλιοθήκη portable τάξης που θα είναι κοινόχρηστη σε όλες τις πλατφόρμες,`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1. καταχώρηση της εφαρμογής Searcher καταλόγου*
Για να ενεργοποιήσετε την εφαρμογή σας για να λάβετε τα διακριτικά, θα πρέπει πρώτα να καταχωρήσετε στο μισθωτή σας Azure AD και να εκχωρήσετε το δικαίωμα πρόσβασης το API Azure AD Graph:

-   Πραγματοποιήστε είσοδο στο την [πύλη διαχείρισης Azure](https://manage.windowsazure.com)
-   Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην **Υπηρεσία καταλόγου Active Directory**
-   Επιλέξτε έναν μισθωτή στην οποία θέλετε να καταχωρήσετε την εφαρμογή.
-   Κάντε κλικ στην καρτέλα **εφαρμογές** και κάντε κλικ στην επιλογή **Προσθήκη** στο το σχέδιο κάτω.
-   Ακολουθήστε τις οδηγίες και να δημιουργήσετε μια νέα **Εφαρμογή εγγενής προγράμματος-πελάτη**.
    -   Το **όνομα** της εφαρμογής θα περιγράφει την εφαρμογή σας στους τελικούς χρήστες
    -   Η **Ανακατεύθυνση Uri** είναι ένας συνδυασμός συνδυασμό και συμβολοσειρά που Azure AD θα χρησιμοποιήσετε για να λάβετε απαντήσεις διακριτικού. Καταχωρήστε μια τιμή, π.χ. `http://DirectorySearcher`.
-   Όταν ολοκληρώσετε την καταχώρηση, AAD θα εκχωρήσετε την εφαρμογή σας ένα αναγνωριστικό μοναδικό προγράμματος-πελάτη. Θα χρειαστείτε αυτή την τιμή στις επόμενες ενότητες, επομένως, αντιγράψτε την από την καρτέλα **Ρύθμιση παραμέτρων** .
- Επίσης στην καρτέλα **Ρύθμιση παραμέτρων** , εντοπίστε την ενότητα "Δικαιώματα σε άλλες εφαρμογές". Για την εφαρμογή "Azure Active Directory", προσθέστε το δικαίωμα **πρόσβασης σας εταιρείας καταλόγου** στην περιοχή **Δικαιώματα με ανάθεση**. Αυτό θα ενεργοποιήσει την εφαρμογή σας ερώτημα για το API Graph για τους χρήστες.

## <a name="2-install--configure-adal"></a>*2. εγκατάσταση και ρύθμιση παραμέτρων ADAL*
Τώρα που έχετε μια εφαρμογή στο Azure AD, μπορείτε να εγκαταστήσετε ADAL και να γράψετε τον κωδικό που σχετίζονται με την ταυτότητα. Με τη σειρά για ADAL να μπορούν να επικοινωνούν με Azure AD, πρέπει να παρέχει ορισμένες πληροφορίες σχετικά με την εγγραφή σας εφαρμογή.
-   Ξεκινήστε προσθέτοντας ADAL σε κάθε ένα από τα έργα στη λύση χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- Θα πρέπει να παρατηρήσετε ότι έχουν προστεθεί δύο βιβλιοθήκη αναφορών για κάθε έργο - τμήμα PCL της ADAL και ένα τμήμα συγκεκριμένης πλατφόρμας.

-   Στο έργο DirectorySearcherLib, ανοίξτε `DirectorySearcher.cs`. Αλλάξτε τις τιμές μέλος κλάσης ώστε να αντικατοπτρίζει τις τιμές που εισαγάγει στην πύλη του Azure. Ο κώδικας θα αναφοράς αυτές τις τιμές κάθε φορά που χρησιμοποιεί ADAL.
    -   Η `tenant` είναι ο τομέας του μισθωτή του Azure AD, π.χ. contoso.onmicrosoft.com
    -   Η `clientId` είναι η clientId της εφαρμογής σας που αντιγράψατε από την πύλη.
    - Η `returnUri` είναι η redirectUri που καταχωρήσατε στην πύλη, π.χ. `http://DirectorySearcher`.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Χρησιμοποιήστε ADAL για να λάβετε τα διακριτικά από AAD*
*Almost* σύνολο της λογικής ελέγχου ταυτότητας της εφαρμογής βρίσκεται στο `DirectorySearcher.SearchByAlias(...)`. Που απαιτείται για τα έργα συγκεκριμένης πλατφόρμας είναι να περάσει με βάση τα συμφραζόμενα παραμέτρου για να την `DirectorySearcher` PCL.

- Πρώτα, ανοίξτε `DirectorySearcher.cs` και να προσθέσετε μια νέα παράμετρο για να το `SearchByAlias(...)` μέθοδο. `IPlatformParameters`είναι η παράμετρος με βάση τα συμφραζόμενα που ενσωματώνει τα αντικείμενα συγκεκριμένης πλατφόρμας που ADAL πρέπει να εκτελέσετε τον έλεγχο ταυτότητας.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   Στη συνέχεια, η προετοιμασία του `AuthenticationContext` -ADAL του πρωτεύοντος τάξης. Αυτό είναι όπου περάσετε ADAL τις συντεταγμένες που χρειάζονται για την επικοινωνία με το Azure AD. Στη συνέχεια, καλέστε `AcquireTokenAsync(...)`, που αποδέχεται το `IPlatformParameters` αντικειμένου και θα ενεργοποιήσει τη ροή ελέγχου ταυτότητας είναι απαραίτητο για να λάβετε ένα διακριτικό στην εφαρμογή.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`θα επιχειρήσει πρώτα να επιστρέψετε ενός διακριτικού για τον πόρο που ζητήθηκε (το API Graph σε αυτήν την περίπτωση) χωρίς να ερωτηθεί ο χρήστης για να εισαγάγετε τα διαπιστευτήριά τους (μέσω σε cache ή ανανέωση παλιά διακριτικά). Μόνο εάν είναι απαραίτητο, θα εμφανίζεται το χρήστη στο σύμβολο Azure AD στη σελίδα πριν από το διακριτικό ζητήθηκε κατά τη λήψη.


- Μπορείτε, στη συνέχεια, να επισυνάψετε το διακριτικό πρόσβασης στην πρόσκληση σε γράφημα API στην κεφαλίδα της εξουσιοδότησης:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

Είναι μόνο για το `DirectorySearcher` PCL και την εφαρμογή του που αφορούν την ταυτότητα κώδικα.  Όλες τις δυνατότητες που είναι παραμένει για να καλέσετε το `SearchByAlias(...)` μέθοδο σε κάθε πλατφόρμα προβολές, και όπου είναι απαραίτητο να προσθέσετε κώδικα για το χειρισμό σωστά την κύκλου ζωής του περιβάλλοντος εργασίας Χρήστη.

####<a name="android"></a>Android:
- Στο `MainActivity.cs`, προσθέστε μια κλήση σε `SearchByAlias(...)` του κουμπιού, κάντε κλικ στην επιλογή πρόγραμμα χειρισμού:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- Πρέπει επίσης να παρακάμψετε τη `OnActivityResult` κύκλου ζωής μέθοδο για την προώθηση κάθε ελέγχου ταυτότητας ανακατευθύνει ξανά την κατάλληλη μέθοδο.  ADAL παρέχει μια μέθοδο Βοήθειας για αυτό στο Android:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Επιφάνεια εργασίας των Windows:
- Στο `MainWindow.xaml.cs`, απλώς να πραγματοποιήσετε μια κλήση `SearchByAlias(...)` διέρχεται μια `WindowInteropHelper` στην επιφάνεια εργασίας του `PlatformParameters` αντικειμένου:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- Στο `DirSearchClient_iOSViewController.cs`, του iOS `PlatformParameters` αντικείμενο λαμβάνει απλώς μια αναφορά στον ελεγκτή προβολή:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>Παγκόσμια Windows:
- Στο Universal των Windows, ανοίξτε `MainPage.xaml.cs` και να εφαρμόζουν το `Search` μέθοδο, η οποία χρησιμοποιεί μια μέθοδο Βοήθειας σε ένα κοινόχρηστο έργο για να ενημερώσετε το περιβάλλον εργασίας Χρήστη, όπως απαιτείται.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Συγχαρητήρια! Τώρα έχετε μια εργασία Xamarin εφαρμογών που έχει τη δυνατότητα να ελέγχουν την ταυτότητα χρηστών και με ασφάλεια κλήσεων APIs Web χρησιμοποιώντας διακριτικό 2.0 σε πέντε διαφορετικές πλατφόρμες. Εάν δεν το έχετε κάνει ήδη, τώρα είναι η ώρα για τη συμπλήρωση του μισθωτή με ορισμένων χρηστών. Εκτελέστε την εφαρμογή DirectorySearcher και συνδεθείτε με έναν από αυτούς τους χρήστες. Αναζήτηση για άλλους χρήστες με βάση τους UPN.

ADAL διευκολύνει την ενσωμάτωση κοινές δυνατότητες ταυτότητας σε εφαρμογή σας. Το αναλαμβάνει όλες τις εργασίες dirty για εσάς: Διαχείριση cache, η υποστήριξη πρωτοκόλλου διακριτικό, παρουσίαση στο χρήστη με μια σύνδεση περιβάλλοντος εργασίας Χρήστη, η ανανέωση έχει λήξει διακριτικά και πολλά άλλα. Όλα όσα χρειάζεται να γνωρίζετε είναι μια μεμονωμένη κλήση API, `authContext.AcquireToken*(…)`.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) παρέχεται [εδώ](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Τώρα, μπορείτε να μετακινήσετε σενάρια επιπλέον ταυτότητα. Εάν θέλετε, μπορείτε να δοκιμάσετε:

[Ασφαλούς Web .NET API με Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
