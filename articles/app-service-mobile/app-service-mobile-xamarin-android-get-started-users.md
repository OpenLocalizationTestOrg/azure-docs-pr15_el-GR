<properties
    pageTitle="Γρήγορα αποτελέσματα με τον έλεγχο ταυτότητας για εφαρμογές του Mobile στο Xamarin Android"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε εφαρμογές του Mobile για τον έλεγχο ταυτότητας τους χρήστες της εφαρμογής Xamarin Android στις διάφορες υπηρεσίες παροχής ταυτότητας, συμπεριλαμβανομένων των AAD, Google, Facebook, Twitter και της Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>Προσθήκη ελέγχου ταυτότητας για την εφαρμογή Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Αυτό το θέμα εξηγεί τον τρόπο ελέγχου ταυτότητας χρηστών από μια εφαρμογή Mobile από την εφαρμογή-πελάτη. Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να προσθέσετε τον έλεγχο ταυτότητας στο έργο γρήγορη έναρξη χρησιμοποιώντας μια υπηρεσία παροχής ταυτότητας που υποστηρίζεται από εφαρμογές του Mobile Azure. Μετά τη με επιτυχία ελεγχθεί η ταυτότητά και εξουσιοδοτημένοι στην εφαρμογή Mobile, εμφανίζεται η τιμή του Αναγνωριστικού χρήστη.

Αυτό το πρόγραμμα εκμάθησης είναι με βάση τη γρήγορη έναρξη εφαρμογή Mobile. Πρέπει να ολοκληρώσετε επίσης πρώτα το πρόγραμμα εκμάθησης, [Δημιουργία εφαρμογής Xamarin.Android]. Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, πρέπει να προσθέσετε το πακέτο επέκταση του ελέγχου ταυτότητας για το έργο σας. Για περισσότερες πληροφορίες σχετικά με τα πακέτα επέκτασης server, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register"></a>Καταχώρηση της εφαρμογής για τον έλεγχο ταυτότητας και ρύθμιση παραμέτρων των υπηρεσιών εφαρμογής

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Περιορισμός δικαιωμάτων σε χρήστες με έλεγχο ταυτότητας

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Στο Visual Studio ή Xamarin Studio, εκτελέστε το έργο προγράμματος-πελάτη σε μια συσκευή ή προσομοίωσης. Βεβαιωθείτε ότι είναι υψωμένο μια ανεπίλυτη εξαίρεση με κωδικό κατάστασης της 401 (εξουσιοδότηση) μετά την εκκίνηση της εφαρμογής. Αυτό συμβαίνει επειδή η εφαρμογή προσπαθεί να αποκτήσει πρόσβαση σας παρασκηνίου εφαρμογή Mobile ως χωρίς έλεγχο ταυτότητας χρήστη. Ο πίνακας *TodoItem* τώρα απαιτεί έλεγχο ταυτότητας.

Στη συνέχεια, θα ενημερώνετε την εφαρμογή υπολογιστή-πελάτη με πρόσκληση σε πόρους από την εφαρμογή Mobile παρασκηνίου με ένα χρήστη με έλεγχο ταυτότητας.

##<a name="add-authentication"></a>Προσθήκη ελέγχου ταυτότητας στην εφαρμογή

Η εφαρμογή ενημερώνεται για να απαιτείται από τους χρήστες να πατήστε το κουμπί " **Είσοδος** " και να υποβληθούν σε έλεγχο ταυτότητας εμφάνισης των δεδομένων.

1. Προσθέστε τον ακόλουθο κώδικα για την τάξη **TodoActivity** :

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    Αυτό δημιουργεί μια νέα μέθοδο για τον έλεγχο ταυτότητας χρήστη και ένα πρόγραμμα χειρισμού μέθοδο για ένα νέο κουμπί **Είσοδος** . Η στο παραπάνω παράδειγμα κώδικα τον έλεγχο ταυτότητας χρήστη, χρησιμοποιώντας μια σύνδεση Facebook. Ένα παράθυρο διαλόγου χρησιμοποιείται για να εμφανίσετε το Αναγνωριστικό χρήστη αφού ελεγχθεί η ταυτότητά.

    > [AZURE.NOTE] Εάν χρησιμοποιείτε μια υπηρεσία παροχής ταυτότητας εκτός από το Facebook, αλλάξτε την τιμή που του μεταβιβάστηκε **LoginAsync** επάνω σε ένα από τα εξής: _διαθέσιμος MicrosoftAccount_, _Twitter_, _Google_ή _WindowsAzureActiveDirectory_.

3. Στη μέθοδο **OnCreate** , διαγραφή ή σχόλιο από την ακόλουθη γραμμή κώδικα:

        OnRefreshItemsSelected ();

4. Στο αρχείο Activity_To_Do.axml, προσθέστε τον ακόλουθο ορισμό κουμπί *LoginUser* πριν από την υπάρχουσα κουμπί *των μεθόδων AddItem* :

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. Προσθέστε το ακόλουθο στοιχείο στο αρχείο Strings.xml πόρους:

        <string name="login_button_text">Sign in</string>

6. Στο Visual Studio ή Xamarin Studio, εκτέλεση του προγράμματος-πελάτη έργου σε μια συσκευή ή προσομοίωσης και συνδεθείτε με την υπηρεσία παροχής ταυτότητας επιλογής.

    Όταν είστε με επιτυχία συνδεθεί στο, η εφαρμογή θα εμφανίσει το Αναγνωριστικό σύνδεσης και τη λίστα των στοιχείων todo και μπορείτε να κάνετε ενημερώσεις στα δεδομένα.


<!-- URLs. -->
[Δημιουργία εφαρμογής Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md
