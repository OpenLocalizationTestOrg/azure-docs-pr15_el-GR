<properties
pageTitle="Εφαρμογή εγγενούς .NET v2.0 καταλόγου Azure Active Directory | Microsoft Azure"
description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή εγγενούς .NET που πραγματοποιεί εταιρικό ή σχολικό λογαριασμοί και οι χρήστες με δύο τον προσωπικό λογαριασμό Microsoft."
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>Προσθήκη εισόδου σε μια εφαρμογή υπολογιστή με Windows

Με το το τελικό σημείο v2.0, μπορείτε να προσθέσετε γρήγορα ελέγχου ταυτότητας για τις εφαρμογές του υπολογιστή με την υποστήριξη για τους δύο λογαριασμούς Microsoft προσωπικές και των λογαριασμών εργασίας ή του σχολείου.  Ενεργοποιεί επίσης την εφαρμογή σας για την ασφαλή επικοινωνία με μια τοποθεσία web του υπολογιστή στο παρασκήνιο api, καθώς και [Το Microsoft Graph](https://graph.microsoft.io) και μερικά από τα [APIs ενοποιημένο σύστημα του Office 365](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [AZURE.NOTE] Δεν όλα τα σενάρια Azure Active Directory (AD) και οι δυνατότητες που υποστηρίζονται από το τελικό σημείο v2.0.  Για να καθορίσετε εάν θα πρέπει να χρησιμοποιήσετε το τελικό σημείο v2.0, διαβάστε σχετικά με [τους περιορισμούς v2.0](active-directory-v2-limitations.md).

Για [.NET εγγενούς εφαρμογές που εκτελούνται σε μια συσκευή](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD παρέχει τη Microsoft ταυτότητα ελέγχου ταυτότητας, ή τη βιβλιοθήκη MSAL.  Σκοπός του MSAL κατά τη διάρκεια ζωής της είναι ώστε να μπορείτε εύκολα για την εφαρμογή σας για να λάβετε τα διακριτικά για την κλήση υπηρεσιών web.  Για μια επίδειξη απλώς πόσο εύκολο είναι, εδώ θα σας θα δημιουργήσουμε μια λίστα εκκρεμών εργασιών WPF .NET εφαρμογή που:

- Πραγματοποιεί το χρήστη & λαμβάνει πρόσβαση κώδικες, χρησιμοποιώντας το [πρωτόκολλο ελέγχου ταυτότητας διακριτικό 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow).
- Κλήσεις με ασφάλεια μια υπηρεσία web λίστα εκκρεμών εργασιών, η οποία είναι επίσης ασφαλές από το διακριτικό 2.0 παρασκηνίου.
- Τοποθετείται στο χρήστη.

## <a name="download-sample-code"></a>Λήψη δείγματος κώδικα

Τον κώδικα για αυτό το πρόγραμμα εκμάθησης διατηρείται [σε GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).  Για να παρακολουθήσετε κατά μήκος, μπορείτε να [κάνετε λήψη της εφαρμογής σκελετό ως ένα .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) ή κλωνοποίηση το σκελετό:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

Η εφαρμογή ολοκληρωμένων παρέχονται στο τέλος αυτού του προγράμματος εκμάθησης καθώς και.

## <a name="register-an-app"></a>Καταχώρηση εφαρμογής
Δημιουργία νέας εφαρμογής στο [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ή ακολουθήστε αυτά τα [λεπτομερή βήματα](active-directory-v2-app-registration.md).  Βεβαιωθείτε ότι:

- Αντιγραφή προς τα κάτω το **Αναγνωριστικό εφαρμογής** που έχουν εκχωρηθεί σε εφαρμογή σας, θα χρειαστεί το συντομότερο.
- Προσθέστε την πλατφόρμα **Mobile** για την εφαρμογή σας.

## <a name="install--configure-msal"></a>Εγκατάσταση και ρύθμιση παραμέτρων MSAL
Τώρα που έχετε μια εφαρμογή που έχουν καταχωρηθεί με τη Microsoft, μπορείτε να εγκαταστήσετε MSAL και να γράψετε τον κωδικό που σχετίζονται με την ταυτότητα.  Με τη σειρά για MSAL να μπορούν να επικοινωνούν το τελικό σημείο v2.0, πρέπει να παρέχει ορισμένες πληροφορίες σχετικά με την εγγραφή σας εφαρμογή.

-   Ξεκινήστε προσθέτοντας MSAL στο έργο TodoListClient χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   Στο έργο TodoListClient, ανοίξτε `app.config`.  Αντικαταστήστε τις τιμές των στοιχείων στο το `<appSettings>` ενότητα ώστε να αντικατοπτρίζει τις τιμές που εισαγάγει στην πύλη του καταχώρηση εφαρμογής.  Ο κώδικας θα αναφοράς αυτές τις τιμές κάθε φορά που χρησιμοποιεί MSAL.
    -   Η `ida:ClientId` είναι το **Αναγνωριστικό εφαρμογής** της εφαρμογής που αντιγράψατε από την πύλη.

- Στο έργο TodoList υπηρεσία, ανοίξτε `web.config` στον ριζικό κατάλογο του έργου.  
    - Αντικαταστήστε το `ida:Audience` τιμή με το ίδιο **Αναγνωριστικό εφαρμογής** από την πύλη.

## <a name="use-msal-to-get-tokens"></a>Χρησιμοποιήστε MSAL για να λάβετε τα διακριτικά
Η βασική αρχή πίσω από MSAL είναι ότι κάθε φορά που την εφαρμογή σας χρειάζεται ένα διακριτικό πρόσβασης, απλώς καλείτε `app.AcquireToken(...)`, και MSAL κάνει τα υπόλοιπα.  

-   Στο το `TodoListClient` το έργο, Άνοιγμα `MainWindow.xaml.cs` και εντοπίστε το `OnInitialized(...)` μέθοδο.  Το πρώτο βήμα είναι να προετοιμάσει την εφαρμογή `PublicClientApplication` -MSAL του πρωτεύοντος κλάσης που αντιπροσωπεύει εγγενείς εφαρμογές.  Αυτό είναι όπου που μεταβιβάζουν MSAL τις συντεταγμένες που χρειάζονται για την επικοινωνία με το Azure AD και ενημερώστε το cache διακριτικά με τον τρόπο.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- Κατά την εκκίνηση της εφαρμογής, θέλουμε να ελέγξετε και να δείτε εάν ο χρήστης είναι συνδεδεμένος ήδη στην εφαρμογή.  Ωστόσο, δεν θέλουμε να σας καλέσει απλώς ακόμη ένα περιβάλλον εργασίας Χρήστη εισόδου - που θα κάνουμε στο χρήστη, κάντε κλικ στην επιλογή "Είσοδος" για να το κάνετε.  Επίσης με το `OnInitialized(...)` μέθοδο:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Εάν ο χρήστης δεν είναι συνδεδεμένος και κάνουν κλικ στο κουμπί "Είσοδος", θέλουμε να καλέσετε μια σύνδεση περιβάλλοντος εργασίας Χρήστη και να έχετε το χρήστη, εισαγάγετε τα διαπιστευτήριά τους.  Υλοποίηση τη λαβή κουμπί εισόδου:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

- Εάν ο χρήστης με επιτυχία πρόσημα στο MSAL θα λαμβάνει και προσωρινή αποθήκευση ενός διακριτικού για εσάς και μπορείτε να συνεχίσετε να καλέσετε το `GetTodoList()` μέθοδο αξιόπιστη.  Το μόνο που απομένει για να λάβετε τις εργασίες ενός χρήστη είναι για να υλοποιήσετε το `GetTodoList()` μέθοδο.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Εκτέλεση

Συγχαρητήρια! Τώρα έχετε μια εφαρμογή .NET WPF εργασία που έχει τη δυνατότητα να ελέγχουν την ταυτότητα χρηστών και με ασφάλεια κλήσεων APIs Web χρησιμοποιώντας διακριτικό 2.0.  Εκτελέστε σας και τα δύο έργα και συνδεθείτε με έναν προσωπικό λογαριασμό Microsoft ή έναν εταιρικό ή σχολικό λογαριασμό.  Προσθήκη εργασιών στη λίστα εκκρεμών εργασιών του χρήστη.  Πραγματοποιήστε έξοδο και συνδεθείτε ξανά με έναν άλλο χρήστη για να προβάλετε τη λίστα εκκρεμών εργασιών.  Κλείστε την εφαρμογή και εκτελέστε ξανά.  Παρατηρήστε πώς περιόδου λειτουργίας του χρήστη παραμένει ανέπαφος - αυτό συμβαίνει επειδή η εφαρμογή αποθηκεύει τα διακριτικά σε ένα τοπικό αρχείο.

MSAL διευκολύνει την ενσωμάτωση κοινές δυνατότητες ταυτότητας σε εφαρμογή της, χρησιμοποιώντας τους λογαριασμούς τόσο προσωπικές και εργασίας.  Το αναλαμβάνει όλες τις εργασίες dirty για εσάς: Διαχείριση cache, η υποστήριξη πρωτοκόλλου διακριτικό, παρουσίαση στο χρήστη με μια σύνδεση περιβάλλοντος εργασίας Χρήστη, η ανανέωση έχει λήξει διακριτικά και πολλά άλλα.  Όλα όσα χρειάζεται να γνωρίζετε είναι μια μεμονωμένη κλήση API, `app.AcquireTokenAsync(...)`.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) [παρέχεται ως ένα .zip εδώ](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), ή μπορείτε να αντιγράψει το το GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα μπορείτε να μετακινήσετε σε θέματα για προχωρημένους.  Εάν θέλετε, μπορείτε να δοκιμάσετε:

- [Ασφάλιση του API Web TodoListService με το τελικό σημείο v2.0](active-directory-v2-devquickstarts-dotnet-api.md)

Για επιπλέον πόρους, ανατρέξτε στο θέμα:  

- [Στον οδηγό για προγραμματιστές v2.0 >>](active-directory-appmodel-v2-overview.md)
- [Ετικέτα "msal" StackOverflow >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Λήψη ενημερώσεων ασφαλείας για τα προϊόντα μας

Συνιστάται να λαμβάνετε ειδοποιήσεις όταν προκύπτουν περιστατικά ασφαλείας, αν επισκεφθείτε την τοποθεσία [αυτήν τη σελίδα](https://technet.microsoft.com/security/dd252948) και εγγραφή συμβουλευτική ειδοποιήσεων ασφαλείας.
