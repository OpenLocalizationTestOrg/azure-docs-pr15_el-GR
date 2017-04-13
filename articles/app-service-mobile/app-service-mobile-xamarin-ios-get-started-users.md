<properties
    pageTitle="Γρήγορα αποτελέσματα με τον έλεγχο ταυτότητας για τις εφαρμογές του Mobile στο Xamarin iOS"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε εφαρμογές του Mobile για τον έλεγχο ταυτότητας χρηστών από την εφαρμογή iOS Xamarin στις διάφορες υπηρεσίες παροχής ταυτότητας, συμπεριλαμβανομένων των AAD, Google, Facebook, Twitter και της Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>Προσθήκη ελέγχου ταυτότητας για την εφαρμογή Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Αυτό το θέμα δείχνει πώς να ελέγχουν την ταυτότητα χρήστες της εφαρμογής υπηρεσίας Mobile εφαρμογής από την εφαρμογή-πελάτη. Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να προσθέσετε τον έλεγχο ταυτότητας στο έργο γρήγορη έναρξη Xamarin.iOS χρησιμοποιώντας μια υπηρεσία παροχής ταυτότητας που υποστηρίζεται από την εφαρμογή υπηρεσίας. Αφού που έχει τεθεί με επιτυχία με έλεγχο ταυτότητας και επιτρέπεται από την εφαρμογή Mobile, εμφανίζεται η τιμή του Αναγνωριστικού χρήστη και θα μπορούν να έχουν πρόσβαση σε δεδομένα του πίνακα περιορισμένα.

Πρέπει να ολοκληρώσετε πρώτα το πρόγραμμα εκμάθησης, [Δημιουργία εφαρμογής Xamarin.iOS]. Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, πρέπει να προσθέσετε το πακέτο επέκταση του ελέγχου ταυτότητας για το έργο σας. Για περισσότερες πληροφορίες σχετικά με τα πακέτα επέκτασης server, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Καταχώρηση της εφαρμογής για τον έλεγχο ταυτότητας και ρύθμιση παραμέτρων των υπηρεσιών εφαρμογής

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Περιορισμός δικαιωμάτων σε χρήστες με έλεγχο ταυτότητας

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. στο Visual Studio ή Xamarin Studio, εκτελέστε το έργο προγράμματος-πελάτη σε μια συσκευή ή προσομοίωσης. Βεβαιωθείτε ότι είναι υψωμένο μια ανεπίλυτη εξαίρεση με κωδικό κατάστασης της 401 (εξουσιοδότηση) μετά την εκκίνηση της εφαρμογής. Η αποτυχία καταγράφεται στην κονσόλα του εργαλείου εντοπισμού σφαλμάτων. Επομένως, στο Visual Studio, θα πρέπει να βλέπετε την αποτυχία στο παράθυρο εξόδου.

&nbsp;&nbsp;Αυτό το σφάλμα μη εξουσιοδοτημένη συμβαίνει επειδή η εφαρμογή προσπαθεί να αποκτήσει πρόσβαση σας παρασκηνίου εφαρμογή Mobile ως χωρίς έλεγχο ταυτότητας χρήστη. Ο πίνακας *TodoItem* τώρα απαιτεί έλεγχο ταυτότητας.

Στη συνέχεια, θα ενημερώνετε την εφαρμογή υπολογιστή-πελάτη με πρόσκληση σε πόρους από την εφαρμογή Mobile παρασκηνίου με ένα χρήστη με έλεγχο ταυτότητας.

##<a name="add-authentication-to-the-app"></a>Προσθήκη ελέγχου ταυτότητας στην εφαρμογή

Σε αυτήν την ενότητα, θα μπορείτε να τροποποιήσετε την εφαρμογή για να εμφανιστεί μια οθόνη login πριν από την εμφάνιση δεδομένων. Κατά την εκκίνηση της εφαρμογής, δεν θα δεν σύνδεση με την υπηρεσία εφαρμογής και δεν θα εμφανίζονται όλα τα δεδομένα. Μετά την πρώτη φορά που ο χρήστης εκτελεί την κίνηση ανανέωσης, εμφανίζεται η οθόνη σύνδεσης. μετά την επιτυχή σύνδεση στη λίστα των στοιχείων todo θα εμφανίζονται.

1. Στο πρόγραμμα-πελάτη έργο, ανοίξτε το αρχείο **QSTodoService.cs** και προσθέστε το εξής χρησιμοποιώντας πρόταση και `MobileServiceUser` με το στοιχείο πρόσβασης στην κλάση QSTodoService:

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. Προσθέστε νέα μέθοδος που ονομάζεται **Έλεγχος ταυτότητας** για να **QSTodoService** με τον ορισμό της παρακάτω:


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Εάν χρησιμοποιείτε μια υπηρεσία παροχής ταυτότητας εκτός από ένα Facebook, αλλάξτε την τιμή που του μεταβιβάστηκε **LoginAsync** επάνω σε ένα από τα εξής: _διαθέσιμος MicrosoftAccount_, _Twitter_, _Google_ή _WindowsAzureActiveDirectory_.

3. Άνοιγμα **QSTodoListViewController.cs**. Τροποποίηση του ορισμού μέθοδο **ViewDidLoad** καταργώντας την κλήση σε **RefreshAsync()** κοντά στο τέλος:

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. Τροποποιήστε τη μέθοδο **RefreshAsync** για τον έλεγχο ταυτότητας εάν η ιδιότητα **χρήστη** είναι null. Προσθέστε τον ακόλουθο κώδικα στο επάνω μέρος του ορισμού της μεθόδου:

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. Στο Visual Studio ή Xamarin Studio συνδεδεμένοι παροχής φιλοξενίας δημιουργία Xamarin στο Mac σας, εκτελέστε το έργο προγράμματος-πελάτη στόχευσης μια συσκευή ή προσομοίωσης. Βεβαιωθείτε ότι η εφαρμογή εμφανίζει χωρίς δεδομένα.

    Εκτελέστε την κίνηση ανανέωσης με έλξη προς τα κάτω στη λίστα των στοιχείων που θα προκαλέσει την οθόνη σύνδεσης για να εμφανίζονται. Αφού έχετε εισαγάγει έγκυρα διαπιστευτήρια με επιτυχία, η εφαρμογή θα εμφανίσει τη λίστα των στοιχείων todo και μπορείτε να κάνετε ενημερώσεις στα δεδομένα.


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Δημιουργία εφαρμογής Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started.md
