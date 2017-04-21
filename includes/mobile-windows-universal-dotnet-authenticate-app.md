
1. Ανοίξτε το αρχείο κοινόχρηστο έργο MainPage.cs και προσθέστε το παρακάτω τμήμα κώδικα για την τάξη MainPage:
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    Αυτός ο κωδικός πραγματοποιεί έλεγχο ταυτότητας του χρήστη με μια σύνδεση Facebook. Εάν χρησιμοποιείτε μια υπηρεσία παροχής ταυτότητας εκτός από το Facebook, αλλάξτε την τιμή του **MobileServiceAuthenticationProvider** επάνω από την τιμή για την υπηρεσία παροχής.

3. Σχόλιο ανάληψης ή διαγράψτε την κλήση στη μέθοδο **RefreshTodoItems** σε την υπάρχουσα παράκαμψη μέθοδο **OnNavigatedTo** .

    Αυτό αποτρέπει τα δεδομένα από τη φόρτωσή πριν από τον έλεγχο ταυτότητας του χρήστη. Στη συνέχεια, θα μπορείτε να προσθέσετε ένα κουμπί **Είσοδος** στην εφαρμογή που ενεργοποιεί τον έλεγχο ταυτότητας.

4. Προσθέστε το παρακάτω τμήμα κώδικα για την τάξη MainPage:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Στο έργο εφαρμογή Windows Store, ανοίξτε το αρχείο έργου MainPage.xaml και προσθέστε το ακόλουθο στοιχείο **κουμπί** ακριβώς πριν από το στοιχείο που ορίζει το κουμπί " **Αποθήκευση** ":

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. Στο χώρο αποθήκευσης του Windows Phone app έργο, προσθέστε το ακόλουθο στοιχείο **κουμπί** στο **ContentPanel**, μετά το στοιχείο **πλαίσιο κειμένου** :

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. Ανοίξτε το κοινόχρηστο αρχείο έργου App.xaml.cs και προσθέστε τον ακόλουθο κώδικα:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    Εάν υπάρχει ήδη τη μέθοδο **OnActivated** , απλώς προσθέστε το `#if...#endif` μπλοκ κώδικα.

9. Πατήστε το πλήκτρο F5 για να εκτελέσετε την εφαρμογή Windows Store, κάντε κλικ στο κουμπί **Είσοδος** και εισέλθω στην εφαρμογή σας στην υπηρεσία παροχής ταυτότητας επιλογής. 

    Όταν είστε με επιτυχία συνδεδεμένοι στο, η εφαρμογή θα πρέπει να εκτελούνται χωρίς σφάλματα και θα πρέπει να ερωτήματος σας υποστήριξης και να κάνετε ενημερώσεις στα δεδομένα.

10. Κάντε δεξί κλικ στο έργο χώρου αποθήκευσης του Windows Phone app, κάντε κλικ στην επιλογή **Ορισμός ως εκκίνησης έργου**και στη συνέχεια επαναλάβετε το προηγούμενο βήμα για να βεβαιωθείτε ότι ο χώρος αποθήκευσης Windows Phone app επίσης εκτελείται σωστά.  

 