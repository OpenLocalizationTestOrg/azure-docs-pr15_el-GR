<properties
    pageTitle="B2C καταλόγου Azure Active Directory | Microsoft Azure"
    description="Διαχείριση προφίλ με χρήση του Azure Active Directory B2C και πώς μπορείτε να δημιουργήσετε μια εφαρμογή επιφάνειας εργασίας των Windows που περιλαμβάνει εισόδου, εγγραφής."
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

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Δημιουργήστε μια εφαρμογή επιφάνειας εργασίας των Windows

Με τη χρήση B2C Azure Active Directory (Azure AD), μπορείτε να προσθέσετε δυνατότητες διαχείρισης ισχυρή ταυτότητας από το χρήστη σας εφαρμογή υπολογιστή με μερικά βήματα σύντομο. Σε αυτό το άρθρο θα σας δείξουν πώς μπορείτε να δημιουργήσετε μια εφαρμογή "λίστα εκκρεμών εργασιών".NET Windows Presentation Foundation (WPF) που περιλαμβάνει εγγραφής, εισόδου, Διαχείριση χρηστών και προφίλ. Η εφαρμογή θα περιλαμβάνουν υποστήριξη για εγγραφής και είσοδος με χρήση του ηλεκτρονικού ταχυδρομείου ή ένα όνομα χρήστη. Αυτό θα περιλαμβάνει επίσης υποστήριξη εγγραφής και είσοδος χρησιμοποιώντας λογαριασμοί κοινωνικών όπως το Facebook και το Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Λήψη ενός καταλόγου Azure AD B2C

Μπορείτε να χρησιμοποιήσετε Azure AD B2C, πρέπει να δημιουργήσετε έναν κατάλογο, ή να μισθωτή.  Ένας κατάλογος είναι ένα κοντέινερ για όλους τους χρήστες σας, εφαρμογές, ομάδες και πολλά άλλα. Εάν δεν έχετε ήδη, [Δημιουργήστε έναν κατάλογο B2C](active-directory-b2c-get-started.md) πριν να συνεχίσετε με αυτόν τον οδηγό.

## <a name="create-an-application"></a>Δημιουργία μιας εφαρμογής

Στη συνέχεια, πρέπει να δημιουργήσετε μια εφαρμογή στον κατάλογό σας B2C. Αυτό σας δίνει Azure AD πληροφορίες που χρειάζονται για την ασφαλή επικοινωνία με την εφαρμογή σας. Για να δημιουργήσετε μια εφαρμογή, ακολουθήστε [αυτές τις οδηγίες](active-directory-b2c-app-registration.md).  Βεβαιωθείτε ότι:

- Συμπεριλάβετε ένα **εγγενές πρόγραμμα-πελάτη** στην εφαρμογή.
- Αντιγράψτε την **ανακατεύθυνση URI** `urn:ietf:wg:oauth:2.0:oob`. Είναι η προεπιλεγμένη διεύθυνση URL για αυτό το δείγμα κώδικα.
- Αντιγράψτε το **Αναγνωριστικό εφαρμογής** που έχει αντιστοιχιστεί σε εφαρμογή σας. Θα το χρειαστείτε αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Δημιουργία πολιτικών σας

Στο Azure AD B2C, κάθε εμπειρία χρήστη ορίζεται από μια [πολιτική](active-directory-b2c-reference-policies.md). Αυτό το δείγμα κώδικα περιέχει τρεις εμπειρίες ταυτότητας: εγγραφείτε, πραγματοποιήστε είσοδο και επεξεργασία του προφίλ. Πρέπει να δημιουργήσετε μια πολιτική για κάθε τύπο, όπως περιγράφεται στο [άρθρο αναφοράς πολιτικής](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Όταν δημιουργείτε τις τρεις πολιτικές, βεβαιωθείτε ότι:

- Επιλέξτε **ηλεκτρονικού ταχυδρομείου εγγραφής** ή **Αναγνωριστικό χρήστη εγγραφής** στο το blade υπηρεσίες παροχής ταυτότητας.
- Επιλέξτε **εμφανιζόμενο όνομα** και άλλα χαρακτηριστικά εγγραφής από την πολιτική της εγγραφής σας.
- Επιλέξτε αξιώσεων **εμφανιζόμενο όνομα** και το **Αναγνωριστικό αντικειμένου** ως δηλώσεις εφαρμογής για κάθε πολιτική. Μπορείτε να επιλέξετε καθώς και άλλες αξιώσεων.
- Αντιγράψτε το **όνομα** κάθε πολιτική μετά τη δημιουργία του. Θα πρέπει να έχει το πρόθεμα `b2c_1_`.  Θα χρειαστείτε αυτά τα ονόματα πολιτικής αργότερα.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Αφού δημιουργήσετε με επιτυχία τις τρεις πολιτικές, είστε έτοιμοι να δημιουργήσετε την εφαρμογή σας.

## <a name="download-the-code"></a>Κάντε λήψη του κώδικα

Ο κωδικός για αυτό προγραμμάτων εκμάθησης [διατηρούνται με GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Για να δημιουργήσετε το δείγμα καθώς εργάζεστε, μπορείτε να [κάνετε λήψη ενός έργου σκελετό ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Μπορείτε επίσης να αντιγράψετε το σκελετό:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

Η εφαρμογή ολοκληρωμένων είναι επίσης [διαθέσιμο ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) ή σε το `complete` υποκατάστημα στο ίδιο αποθετήριο.

Μετά τη λήψη του δείγματος κώδικα, ανοίξτε το αρχείο .sln Visual Studio για να ξεκινήσετε. Το `TaskClient` την εφαρμογή υπολογιστή WPF που ο χρήστης αλληλεπιδρά με το έργο είναι. Για τους σκοπούς αυτής της εκμάθησης, καλεί web εργασίας παρασκηνίου API, φιλοξενούνται στο Azure που αποθηκεύει του χρήστη κάθε λίστα εκκρεμών εργασιών.  Δεν χρειάζεται να δημιουργείτε το API web, έχουμε ήδη να εκτελείται για εσάς.

Για να μάθετε πώς ένα API web ασφαλή πραγματοποιεί έλεγχο ταυτότητας αιτήσεις χρησιμοποιώντας Azure AD B2C, δείτε το [web API γρήγορα αποτελέσματα το άρθρο](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Εκτέλεση πολιτικές
Εφαρμογή σας επικοινωνεί με Azure AD B2C με την αποστολή μηνυμάτων ελέγχου ταυτότητας που καθορίζουν την πολιτική που θέλουν να εκτελέσει ως μέρος της αίτησης HTTP. Για εφαρμογές υπολογιστή του .NET, μπορείτε να χρησιμοποιήσετε την προεπισκόπηση βιβλιοθήκη ελέγχου ταυτότητας Microsoft (MSAL) για να στείλετε μηνύματα ελέγχου ταυτότητας διακριτικό 2.0, εκτέλεση πολιτικές, και να λάβετε τα διακριτικά που καλούν web APIs.

### <a name="install-msal"></a>Εγκατάσταση MSAL
Προσθήκη MSAL για να το `TaskClient` έργου χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου του Visual Studio.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Πληκτρολογήστε τις λεπτομέρειες του B2C σας
Ανοίξτε το αρχείο `Globals.cs` και να αντικαταστήσετε κάθε μία από τις τιμές ιδιοτήτων με το δικό σας. Αυτή η κλάση χρησιμοποιείται σε όλο το `TaskClient` αναφοράς που χρησιμοποιούνται ευρέως τιμές.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>Δημιουργία του PublicClientApplication
Η κύρια κλάση της MSAL είναι `PublicClientApplication`. Αυτή η κλάση αντιπροσωπεύει την εφαρμογή στο σύστημα του Azure AD B2C. Όταν το initalizes εφαρμογή, δημιουργήστε μια παρουσία του `PublicClientApplication` σε `MainWindow.xaml.cs`. Αυτό μπορεί να χρησιμοποιηθεί σε όλο το παράθυρο.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>Έναρξη μιας ροής εγγραφής
Όταν ένας χρήστης επιλέξει να πρόσημα προς τα επάνω, που θέλετε να ξεκινήσετε μια ροή εγγραφής που χρησιμοποιεί την πολιτική εγγραφής που δημιουργήσατε. Με τη χρήση MSAL, απλώς καλείτε `pca.AcquireTokenAsync(...)`. Οι παράμετροι που μεταβιβάζουν για `AcquireTokenAsync(...)` καθορίζουν ποιες διακριτικό λαμβάνετε, η πολιτική που χρησιμοποιείται στο αίτησης για έλεγχο ταυτότητας και πολλά άλλα.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
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

### <a name="initiate-a-sign-in-flow"></a>Έναρξη μιας ροής εισόδου
Μπορείτε να ξεκινήσετε μια ροή εισόδου με τον ίδιο τρόπο που πραγματοποιείτε μια ροή εγγραφής. Όταν ένας χρήστης πραγματοποιεί είσοδο, κάνετε την ίδια κλήση MSAL, αυτήν τη φορά, χρησιμοποιώντας την πολιτική εισόδου:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Έναρξη μιας ροής Επεξεργασία προφίλ
Και πάλι, μπορείτε να εκτελέσετε μια πολιτική Επεξεργασία προφίλ με τον ίδιο τρόπο:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

Σε όλες αυτές τις περιπτώσεις, MSAL είτε επιστρέφει ένα διακριτικό στο `AuthenticationResult` ή δημιουργεί μια εξαίρεση. Κάθε φορά που λαμβάνετε ένα διακριτικό από MSAL, μπορείτε να χρησιμοποιήσετε το `AuthenticationResult.User` αντικειμένου για να ενημερώσετε τα δεδομένα χρήστη στην εφαρμογή, όπως το περιβάλλον εργασίας Χρήστη. ADAL αποθηκεύει επίσης προσωρινά το διακριτικό για χρήση σε άλλα τμήματα της εφαρμογής.


### <a name="check-for-tokens-on-app-start"></a>Έλεγχος για τα διακριτικά από το μενού Έναρξη εφαρμογής
Μπορείτε επίσης να χρησιμοποιήσετε MSAL για να παρακολουθείτε του χρήστη εισόδου κατάσταση.  Σε αυτήν την εφαρμογή, θέλουμε το χρήστη για να παραμείνετε συνδεδεμένοι στο ακόμα και αν αυτά Κλείσιμο της εφαρμογής και ανοίξτε την ξανά.  Επιστροφή μέσα το `OnInitialized` παράκαμψη, χρησιμοποιήστε του MSAL `AcquireTokenSilent` μέθοδο για να ελέγξετε για στο cache τα διακριτικά:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
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
```

## <a name="call-the-task-api"></a>Κλήση εργασίας API
Τώρα που έχετε χρησιμοποιήσει MSAL για να εκτελέσετε πολιτικές και να λάβετε τα διακριτικά.  Όταν θέλετε να χρησιμοποιήσετε ένα αυτά τα διακριτικά για να καλέσετε την εργασία API, μπορείτε να χρησιμοποιήσετε ξανά του MSAL `AcquireTokenSilent` μέθοδο για να ελέγξετε για στο cache τα διακριτικά:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
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
    ...
```

Κατά την κλήση σε `AcquireTokenSilentAsync(...)` με επιτυχία και ενός διακριτικού βρίσκεται στη μνήμη cache, μπορείτε να προσθέσετε το διακριτικό για το `Authorization` κεφαλίδα της αίτησης HTTP. Το API web εργασίας θα χρησιμοποιήσει αυτήν την κεφαλίδα για τον έλεγχο ταυτότητας της αίτησης για να διαβάσετε λίστα εκκρεμών εργασιών του χρήστη:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>Αποσύνδεση χρήστη
Τέλος, μπορείτε να χρησιμοποιήσετε MSAL για να τερματίσετε την περίοδο λειτουργίας ενός χρήστη με την εφαρμογή, όταν ο χρήστης επιλέγει **Αποσύνδεση**.  Όταν χρησιμοποιείτε MSAL, αυτή η ενέργεια πραγματοποιείται καταργώντας όλα τα διακριτικά από το cache διακριτικού:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>Εκτελέστε την εφαρμογή δείγματος

Τέλος, δημιουργία και εκτέλεση του δείγματος.  Εγγραφή για την εφαρμογή, χρησιμοποιώντας ένα μήνυμα ηλεκτρονικού ταχυδρομείου διεύθυνση ή το όνομα χρήστη. Αποσυνδεθείτε και συνδεθείτε ξανά με το ίδιο χρήστη. Επεξεργασία του προφίλ του χρήστη. Πραγματοποιήστε έξοδο και να εγγραφούν χρησιμοποιώντας έναν άλλο χρήστη.

## <a name="add-social-idps"></a>Προσθήκη κοινωνικού IDPs

Προς το παρόν, η εφαρμογή υποστηρίζει μόνο χρήστη εγγραφής και εισόδου που χρησιμοποιούν **οι τοπικοί λογαριασμοί**. Αυτές είναι οι λογαριασμοί που είναι αποθηκευμένα στον κατάλογό σας B2C που χρησιμοποιούν το όνομα χρήστη και τον κωδικό πρόσβασης. Με τη χρήση Azure AD B2C, μπορείτε να προσθέσετε υποστήριξη για άλλες υπηρεσίες παροχής ταυτότητας (IDPs) χωρίς να αλλάξετε οποιαδήποτε από τον κωδικό.

Για να προσθέσετε κοινωνικών IDPs την εφαρμογή σας, ξεκινήστε ακολουθώντας τις αναλυτικές οδηγίες σε αυτά τα άρθρα. Για κάθε IDP που θέλετε για την υποστήριξη, πρέπει να καταχωρήσετε μια εφαρμογή στο σύστημα που και να αποκτήσετε ένα αναγνωριστικό υπολογιστή-πελάτη.

- [Ρύθμιση του Facebook ως μια IDP](active-directory-b2c-setup-fb-app.md)
- [Ρύθμιση του Google ως μια IDP](active-directory-b2c-setup-goog-app.md)
- [Ρύθμιση του Amazon ως μια IDP](active-directory-b2c-setup-amzn-app.md)
- [Ρύθμιση του LinkedIn ως μια IDP](active-directory-b2c-setup-li-app.md)

Αφού προσθέσετε τις υπηρεσίες παροχής ταυτότητας στον κατάλογό σας B2C, πρέπει να επεξεργαστείτε κάθε μία από τις τρεις πολιτικές για να συμπεριλάβετε τα νέα IDPs, όπως περιγράφεται στο [άρθρο αναφοράς πολιτικής](active-directory-b2c-reference-policies.md). Αφού αποθηκεύσετε τις πολιτικές σας, εκτελέστε ξανά την εφαρμογή. Θα πρέπει να δείτε το νέο IDPs προστεθεί ως είσοδο και να αντιμετωπίσει επιλογές εγγραφής σε κάθε έναν από την ταυτότητά σας.

Μπορείτε να πειραματιστείτε με τις πολιτικές και να παρακολουθήσετε τις συνέπειες σε εφαρμογή της δείγμα. Προσθήκη ή κατάργηση IDPs, χειριστείτε αξιώσεων εφαρμογή ή αλλαγή χαρακτηριστικών εγγραφής. Πειραματιστείτε μέχρι να μπορείτε να δείτε πώς πολιτικών, αιτήσεις για έλεγχο ταυτότητας και MSAL να συνδέσετε.

Για αναφορά, το ολοκληρωμένο δείγμα που [παρέχεται ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Μπορείτε, επίσης, να την αντιγράψετε από GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
