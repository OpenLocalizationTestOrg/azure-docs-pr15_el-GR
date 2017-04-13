<properties
    pageTitle="Γρήγορα αποτελέσματα με τον έλεγχο ταυτότητας για τις εφαρμογές του Mobile στην εφαρμογή Xamarin.Forms | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε εφαρμογές του Mobile για τον έλεγχο ταυτότητας τους χρήστες της εφαρμογής Xamarin φόρμες σε διάφορες υπηρεσίες παροχής ταυτότητας, συμπεριλαμβανομένων των AAD, Google, Facebook, Twitter και της Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>Προσθήκη ελέγχου ταυτότητας για την εφαρμογή Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>Επισκόπηση

Αυτό το θέμα δείχνει πώς να ελέγχουν την ταυτότητα χρήστες της εφαρμογής υπηρεσίας Mobile εφαρμογής από την εφαρμογή-πελάτη. Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να προσθέσετε τον έλεγχο ταυτότητας στο έργο γρήγορη έναρξη Xamarin.Forms χρησιμοποιώντας μια υπηρεσία παροχής ταυτότητας που υποστηρίζεται από την εφαρμογή υπηρεσίας. Αφού που έχει τεθεί με επιτυχία με έλεγχο ταυτότητας και επιτρέπεται από την εφαρμογή Mobile, εμφανίζεται η τιμή του Αναγνωριστικού χρήστη και θα μπορούν να έχουν πρόσβαση σε δεδομένα του πίνακα περιορισμένα.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για το αποτέλεσμα καλύτερα με αυτό το πρόγραμμα εκμάθησης, συνιστάται να ολοκληρώσετε πρώτα το πρόγραμμα εκμάθησης [Δημιουργία εφαρμογής Xamarin.Forms](app-service-mobile-xamarin-forms-get-started.md) . Αφού ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, θα έχετε ένα έργο Xamarin.Forms που είναι μια εφαρμογή TodoList πολλών πλατφόρμας.

Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, πρέπει να προσθέσετε το πακέτο επέκταση του ελέγχου ταυτότητας για το έργο σας. Για περισσότερες πληροφορίες σχετικά με τα πακέτα επέκτασης server, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Καταχώρηση της εφαρμογής για τον έλεγχο ταυτότητας και ρύθμιση παραμέτρων των υπηρεσιών εφαρμογής

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Περιορισμός δικαιωμάτων σε χρήστες με έλεγχο ταυτότητας

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>Προσθήκη ελέγχου ταυτότητας στη βιβλιοθήκη portable τάξης

Εφαρμογές του Mobile χρησιμοποιεί τη μέθοδο επέκτασης [LoginAsync] στην το [MobileServiceClient] να συνδεθείτε στο χρήστη με έλεγχο ταυτότητας εφαρμογής υπηρεσίας. Αυτό το δείγμα χρησιμοποιεί μια ροή Διαχείριση διακομιστή ελέγχου ταυτότητας που εμφανίζει την υπηρεσία παροχής εισόδου στο περιβάλλον εργασίας στην εφαρμογή. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση διακομιστή ελέγχου ταυτότητας](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Για να εξασφαλιστεί μια καλύτερη εμπειρία χρήστη στην εφαρμογή παραγωγής, ίσως θέλετε να αντί για αυτό με χρήση [προγράμματος-πελάτη Διαχείριση ελέγχου ταυτότητας](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow). 

Για τον έλεγχο ταυτότητας με ένα έργο Xamarin.Forms, μπορείτε να ορίσετε ένα περιβάλλον εργασίας **IAuthenticate** στη φορητή κλάσης βιβλιοθήκη για την εφαρμογή. Μπορείτε επίσης να ενημερώσετε το περιβάλλον εργασίας χρήστη που ορίζονται από το στη φορητή βιβλιοθήκη κλάσης για να προσθέσετε μια **εισόδου στο** κουμπί, το οποίο ο χρήστης κάνει κλικ για να ξεκινήσετε τον έλεγχο ταυτότητας. Μετά την επιτυχή έλεγχο ταυτότητας, φόρτωση των δεδομένων από την εφαρμογή για κινητές συσκευές παρασκηνίου.

Πρέπει να υλοποιήσετε το περιβάλλον εργασίας **IAuthenticate** για κάθε πλατφόρμα υποστηρίζεται από την εφαρμογή σας.


1. Στο Visual Studio ή Xamarin Studio, Άνοιγμα App.cs από το έργο με **φορητό** στο όνομά της, που είναι φορητό βιβλιοθήκη κλάσεων έργου, στη συνέχεια, προσθέστε τα ακόλουθα `using` δήλωση:

        using System.Threading.Tasks;

2. Στο App.cs, προσθέστε τα ακόλουθα `IAuthenticate` περιβάλλοντος εργασίας του ορισμού αμέσως πριν από την `App` κλάση ορισμού.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. Προσθέστε τα ακόλουθα μέλη στατικό στην κλάση **εφαρμογής** για να προετοιμάσετε το περιβάλλον εργασίας με μια συγκεκριμένη εφαρμογή πλατφόρμας.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. Άνοιγμα TodoList.xaml από το έργο Portable βιβλιοθήκη κλάσεων, προσθέστε το ακόλουθο στοιχείο **κουμπί** στο στοιχείο διάταξη *buttonsPanel* , μετά το υπάρχον κουμπί: 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    Αυτό το κουμπί ενεργοποιεί Διαχείριση διακομιστή τον έλεγχο ταυτότητας με τον υπολογιστή στο παρασκήνιο εφαρμογής για κινητές συσκευές.

5. Άνοιγμα TodoList.xaml.cs από το έργο Portable βιβλιοθήκη κλάσεων και, στη συνέχεια, προσθέστε το παρακάτω πεδίο για να το `TodoList` κλάσης:

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. Αντικατάσταση της μεθόδου **OnAppearing** με τον ακόλουθο κώδικα:

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Αυτό διασφαλίζει ότι μόνο ανανέωσης των δεδομένων από την υπηρεσία αφού ο χρήστης έχει ελεγχθεί η ταυτότητα.

7. Προσθέστε το ακόλουθο πρόγραμμα χειρισμού για το συμβάν **Clicked** στην κλάση **TodoList** :

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. Αποθηκεύστε τις αλλαγές σας και εκ νέου δημιουργία του έργου φορητό βιβλιοθήκη κλάσεων επαλήθευση χωρίς σφάλματα.


##<a name="add-authentication-to-the-android-app"></a>Προσθήκη ελέγχου ταυτότητας για την εφαρμογή στο Android

Αυτή η ενότητα δείχνει πώς μπορείτε να υλοποιήσετε το περιβάλλον εργασίας **IAuthenticate** του έργου εφαρμογή στο Android. Εάν δεν υποστηρίζετε συσκευές Android, παραλείψτε αυτήν την ενότητα.

1. Στο Visual Studio ή Xamarin Studio, κάντε δεξί κλικ στο έργο **droid** , στη συνέχεια, **ορίστε ως εκκίνησης έργου**.

2. Πατήστε το πλήκτρο F5 για να ξεκινήσετε το έργο στον εντοπισμό σφαλμάτων και, στη συνέχεια, επιβεβαιώστε ότι μια ανεπίλυτη εξαίρεση με κωδικό κατάστασης της 401 (εξουσιοδότηση) ενεργοποιείται μετά την εκκίνηση της εφαρμογής. Αυτό συμβαίνει επειδή η πρόσβαση στο υπόβαθρο είναι περιορισμένη σε εξουσιοδοτημένους χρήστες μόνο.

3. Άνοιγμα MainActivity.cs στο Android έργο και προσθέστε το εξής `using` προτάσεις:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Ενημερώστε την κλάση **MainActivity** στο υλοποιεί τη διασύνδεση **IAuthenticate** , ως εξής:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. Ενημερώστε την κλάση **MainActivity** , προσθέτοντας ένα πεδίο **MobileServiceUser** και μεθόδου **ελέγχου ταυτότητας** , το οποίο απαιτείται από το περιβάλλον εργασίας **IAuthenticate** , ως εξής:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    Εάν χρησιμοποιείτε μια υπηρεσία παροχής ταυτότητας εκτός από το Facebook, επιλέξτε μια διαφορετική τιμή για το [MobileServiceAuthenticationProvider].

6. Προσθέστε τον ακόλουθο κώδικα στη μέθοδο **OnCreate** της κλάσης **MainActivity** πριν από την κλήση σε `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Αυτό διασφαλίζει ότι η υπηρεσία ελέγχου ταυτότητας έχει προετοιμαστεί πριν από την εφαρμογή φορτία.

7. Εκ νέου δημιουργία της εφαρμογής, το εκτελείτε, στη συνέχεια, πραγματοποιήστε είσοδο με την υπηρεσία παροχής ελέγχου ταυτότητας που επιλέξατε και επαλήθευση, θα μπορείτε να αποκτήσετε πρόσβαση σε δεδομένα ως χρήστης με έλεγχο ταυτότητας.

##<a name="add-authentication-to-the-ios-app"></a>Προσθήκη ελέγχου ταυτότητας για την εφαρμογή του iOS

Αυτή η ενότητα δείχνει πώς μπορείτε να υλοποιήσετε το περιβάλλον εργασίας **IAuthenticate** του έργου εφαρμογή iOS. Εάν δεν υποστηρίζετε συσκευές iOS, παραλείψτε αυτήν την ενότητα.

1. Στο Visual Studio ή Xamarin Studio, κάντε δεξί κλικ στο έργο **iOS** , στη συνέχεια, **ορίστε ως εκκίνησης έργου**.

2. Πατήστε το πλήκτρο F5 για να ξεκινήσετε το έργο στον εντοπισμό σφαλμάτων και, στη συνέχεια, επιβεβαιώστε ότι μια ανεπίλυτη εξαίρεση με κωδικό κατάστασης της 401 (εξουσιοδότηση) ενεργοποιείται μετά την εκκίνηση της εφαρμογής. Αυτό συμβαίνει επειδή η πρόσβαση στο υπόβαθρο είναι περιορισμένη σε εξουσιοδοτημένους χρήστες μόνο.

4. Άνοιγμα AppDelegate.cs στο iOS έργο και προσθέστε το εξής `using` προτάσεις:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Ενημερώστε την κλάση **AppDelegate** στο υλοποιεί τη διασύνδεση **IAuthenticate** , ως εξής:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. Ενημερώστε την κλάση **AppDelegate** με την προσθήκη ενός πεδίου **MobileServiceUser** και μεθόδου **ελέγχου ταυτότητας** , το οποίο απαιτείται από το περιβάλλον εργασίας **IAuthenticate** , ως εξής:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    Εάν χρησιμοποιείτε μια υπηρεσία παροχής ταυτότητας εκτός από το Facebook, επιλέξτε μια διαφορετική τιμή για το [MobileServiceAuthenticationProvider].

6. Προσθέστε την ακόλουθη γραμμή του κώδικα στη μέθοδο **FinishedLaunching** πριν από την κλήση σε `LoadApplication()`: 

        App.Init(this);

    Αυτό διασφαλίζει ότι η υπηρεσία ελέγχου ταυτότητας έχει προετοιμαστεί πριν από την εφαρμογή φόρτωση.

7. Εκ νέου δημιουργία της εφαρμογής, το εκτελείτε, στη συνέχεια, πραγματοποιήστε είσοδο με την υπηρεσία παροχής ελέγχου ταυτότητας που επιλέξατε και επαλήθευση, θα μπορείτε να αποκτήσετε πρόσβαση σε δεδομένα ως χρήστης με έλεγχο ταυτότητας.


##<a name="add-authentication-to-windows-app-projects"></a>Προσθήκη ελέγχου ταυτότητας σε έργα εφαρμογής των Windows

Αυτή η ενότητα δείχνει πώς μπορείτε να υλοποιήσετε το περιβάλλον εργασίας **IAuthenticate** στα Windows 8.1 και Windows Phone 8.1 εφαρμογή έργα. Τα ίδια βήματα ισχύουν για τα έργα Universal πλατφόρμας των Windows (UWP). Εάν δεν υποστηρίζετε συσκευές με Windows, παραλείψτε αυτήν την ενότητα.

1. Στο Visual Studio, κάντε δεξί κλικ στο έργο **WinPhone81** , στη συνέχεια, **ορίστε ως έργο εκκίνησης**είτε το **WinApp** .

2. Πατήστε το πλήκτρο F5 για να ξεκινήσετε το έργο στον εντοπισμό σφαλμάτων και, στη συνέχεια, επιβεβαιώστε ότι μια ανεπίλυτη εξαίρεση με κωδικό κατάστασης της 401 (εξουσιοδότηση) ενεργοποιείται μετά την εκκίνηση της εφαρμογής. Αυτό συμβαίνει επειδή η πρόσβαση στο υπόβαθρο είναι περιορισμένη σε εξουσιοδοτημένους χρήστες μόνο.

3. Ανοίξτε το MainPage.xaml.cs για το έργο εφαρμογής των Windows και προσθέστε τα ακόλουθα `using` προτάσεις:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Αντικατάσταση `<your_Portable_Class_Library_namespace>` με το χώρο ονομάτων για τη βιβλιοθήκη σας φορητή τάξης.

4. Ενημερώστε την κλάση **MainPage** στο υλοποιεί τη διασύνδεση **IAuthenticate** , ως εξής:

        public sealed partial class MainPage : IAuthenticate


5. Ενημερώστε την κλάση **MainPage** με την προσθήκη ενός πεδίου **MobileServiceUser** και μεθόδου **ελέγχου ταυτότητας** , το οποίο απαιτείται από το περιβάλλον εργασίας **IAuthenticate** , ως εξής:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    Εάν χρησιμοποιείτε μια υπηρεσία παροχής ταυτότητας εκτός από το Facebook, επιλέξτε μια διαφορετική τιμή για το [MobileServiceAuthenticationProvider].

6. Προσθέστε την ακόλουθη γραμμή του κώδικα στην κατασκευή για την τάξη **MainPage** πριν από την κλήση σε `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    Αντικατάσταση `<your_Portable_Class_Library_namespace>` με το χώρο ονομάτων για τη βιβλιοθήκη σας φορητή τάξης.  
    Εάν πρόκειται για το έργο WinApp, μπορείτε να μεταβείτε προς τα κάτω, βήμα 8. Το επόμενο βήμα ισχύει μόνο για το έργο WinPhone81, όπου πρέπει να ολοκληρώσετε την επιστροφή κλήσης σύνδεσης.

7. (Προαιρετικό) Στο έργο εφαρμογής **WinPhone81** , ανοίξτε το App.xaml.cs και προσθέστε το εξής `using` προτάσεις:

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    Αντικατάσταση `<your_Portable_Class_Library_namespace>` με το χώρο ονομάτων για τη βιβλιοθήκη σας φορητή τάξης.

8.  Προσθέστε το παρακάτω παράκαμψη μέθοδο **OnActivated** στην κλάση **εφαρμογής** :

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    Όταν η παράκαμψη μέθοδο υπάρχει ήδη, απλώς προσθέστε τον κώδικα υπό όρους από το παραπάνω τμήμα κώδικα.

7. Εκ νέου δημιουργία της εφαρμογής, το εκτελείτε, στη συνέχεια, πραγματοποιήστε είσοδο με την υπηρεσία παροχής ελέγχου ταυτότητας που επιλέξατε και επαλήθευση, θα μπορείτε να αποκτήσετε πρόσβαση σε δεδομένα ως χρήστης με έλεγχο ταυτότητας.

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα που ολοκληρωθεί αυτό το πρόγραμμα εκμάθησης βασικό έλεγχο ταυτότητας, μπορείτε να συνεχίσετε με ένα από τα παρακάτω προγράμματα εκμάθησης:

+ [Προσθέστε τις ειδοποιήσεις push για την εφαρμογή σας](app-service-mobile-xamarin-forms-get-started-push.md)  
  Μάθετε πώς μπορείτε να προσθέσετε push ειδοποιήσεις υποστήριξη για να την εφαρμογή σας και ρύθμιση παραμέτρων σας εφαρμογή Mobile υποστήριξης για την αποστολή ειδοποιήσεων push Azure ειδοποίηση διανομείς.

+ [Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή σας](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Μάθετε πώς μπορείτε να προσθέσετε την εφαρμογή σας χρησιμοποιώντας μια εφαρμογή Mobile παρασκηνίου υποστήριξη χωρίς σύνδεση. Συγχρονισμού χωρίς σύνδεση επιτρέπει στους τελικούς χρήστες να αλληλεπιδράσετε με μια εφαρμογή για κινητές συσκευές&mdash;προβολή, προσθήκη ή τροποποίηση δεδομένων&mdash;ακόμα και όταν δεν υπάρχει σύνδεση δικτύου.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

