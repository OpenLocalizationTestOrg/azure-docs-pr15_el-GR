<properties
    pageTitle="Προσθήκη ελέγχου ταυτότητας σε εφαρμογή της γενικής χρήσης πλατφόρμας των Windows (UWP) | Azure εφαρμογές για κινητές συσκευές"
    description="Μάθετε πώς να χρησιμοποιείτε τις εφαρμογές του Mobile Azure εφαρμογής υπηρεσίας για τον έλεγχο ταυτότητας τους χρήστες της εφαρμογής καθολικής πλατφόρμας των Windows (UWP) χρησιμοποιώντας μια ποικιλία υπηρεσιών παροχής ταυτότητας, συμπεριλαμβανομένων των: AAD, Google, Facebook, Twitter και της Microsoft."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>Προσθήκη ελέγχου ταυτότητας για την εφαρμογή των Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Αυτό το θέμα δείχνει πώς μπορείτε να προσθέσετε τον έλεγχο ταυτότητας που βασίζεται στο cloud για την εφαρμογή για κινητές συσκευές. Σε αυτό το πρόγραμμα εκμάθησης, προσθέτετε τον έλεγχο ταυτότητας στο έργο γρήγορη έναρξη καθολικής πλατφόρμας των Windows (UWP) για εφαρμογές του Mobile χρησιμοποιώντας μια υπηρεσία παροχής ταυτότητας που υποστηρίζεται από το Azure εφαρμογής υπηρεσίας. Μετά την που έχει τεθεί με επιτυχία με έλεγχο ταυτότητας και επιτρέπεται από την εφαρμογή Mobile παρασκηνίου, εμφανίζεται η τιμή του Αναγνωριστικού χρήστη.

Αυτό το πρόγραμμα εκμάθησης είναι με βάση τη γρήγορη έναρξη εφαρμογές του Mobile. Πρέπει να ολοκληρώσετε πρώτα το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με εφαρμογές του Mobile](app-service-mobile-windows-store-dotnet-get-started.md).

##<a name="register"></a>Καταχώρηση της εφαρμογής για τον έλεγχο ταυτότητας και ρύθμιση παραμέτρων της εφαρμογής υπηρεσίας

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Περιορισμός δικαιωμάτων σε χρήστες με έλεγχο ταυτότητας

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Τώρα, μπορείτε να επαληθεύσετε ότι έχει απενεργοποιηθεί ανώνυμης πρόσβασης για τον υπολογιστή στο παρασκήνιο. Με το έργο εφαρμογής UWP Ορισμός ως το έργο εκκίνησης, ανάπτυξης και εκτελέστε την εφαρμογή; Βεβαιωθείτε ότι είναι υψωμένο μια ανεπίλυτη εξαίρεση με κωδικό κατάστασης της 401 (εξουσιοδότηση) μετά την εκκίνηση της εφαρμογής. Αυτό συμβαίνει επειδή η εφαρμογή προσπαθεί να αποκτήσει πρόσβαση τον κωδικό App Mobile ως χωρίς έλεγχο ταυτότητας χρήστη, αλλά ο πίνακας *TodoItem* τώρα απαιτεί έλεγχο ταυτότητας.

Στη συνέχεια, θα μπορείτε να ενημερώσετε την εφαρμογή για τον έλεγχο ταυτότητας χρηστών πριν από την αίτηση πόρων από την υπηρεσία εφαρμογής.

##<a name="add-authentication"></a>Προσθήκη ελέγχου ταυτότητας στην εφαρμογή

1. Σε UWP την εφαρμογή αρχείου MainPage.cs έργου και προσθέστε το παρακάτω τμήμα κώδικα στην κλάση MainPage:
    
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

3. Σχόλιο ανάληψης ή διαγράψτε την κλήση της μεθόδου **ButtonRefresh_Click** (ή τη μέθοδο **InitLocalStoreAsync** ) στο την υπάρχουσα παράκαμψη μέθοδο **OnNavigatedTo** . Αυτό αποτρέπει τα δεδομένα από τη φόρτωσή πριν από τον έλεγχο ταυτότητας του χρήστη. Στη συνέχεια, θα μπορείτε να προσθέσετε ένα κουμπί **Είσοδος** στην εφαρμογή που ενεργοποιεί τον έλεγχο ταυτότητας.

4. Προσθέστε το παρακάτω τμήμα κώδικα για την τάξη MainPage:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Ανοίξτε το αρχείο έργου MainPage.xaml, εντοπίστε το στοιχείο που ορίζει το κουμπί " **Αποθήκευση** " και να το αντικαταστήσετε με τον ακόλουθο κώδικα:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Πατήστε το πλήκτρο F5 για να εκτελέσετε την εφαρμογή και κάντε κλικ στο κουμπί **Είσοδος** εισέλθω στην εφαρμογή σας στην υπηρεσία παροχής ταυτότητας επιλεγμένο. Αφού σας εισόδου είναι επιτυχής, η εφαρμογή εκτελείται χωρίς σφάλματα και είστε σε θέση να ερωτήματος σας υποστήριξης και να κάνετε ενημερώσεις στα δεδομένα.


##<a name="tokens"></a>Αποθηκεύστε το διακριτικό έλεγχο ταυτότητας του υπολογιστή-πελάτη

Το προηγούμενο παράδειγμα εμφάνιζε μια τυπική εισόδου, γεγονός που απαιτεί το πρόγραμμα-πελάτη για να επικοινωνήσετε με την υπηρεσία παροχής ταυτότητας και της εφαρμογής υπηρεσίας κάθε φορά που ξεκινά η εφαρμογή. Όχι μόνο είναι αποτελεσματική, μπορείτε να εκτελέσετε αυτήν τη μέθοδο σε χρήση-συσχετίζει θέματα πρέπει να πολλοί πελάτες προσπαθήσετε να εκκινήσετε εφαρμογή την ίδια στιγμή. Μια καλύτερη προσέγγιση είναι να cache το διακριτικό εξουσιοδότησης που επιστρέφονται από την υπηρεσία εφαρμογής και προσπαθήστε να χρησιμοποιήσετε αυτό το πρώτο πριν από τη χρήση μιας υπηρεσίας παροχής βάσει εισόδου.

>[AZURE.NOTE]Μπορείτε να αποθηκεύσετε προσωρινά το διακριτικό εκδοθεί από εφαρμογή υπηρεσιών ανεξάρτητα από το εάν χρησιμοποιείτε τον έλεγχο ταυτότητας διαχειριζόμενο από το πρόγραμμα-πελάτη ή διαχείριση υπηρεσίας. Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί διαχείριση υπηρεσίας ελέγχου ταυτότητας.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα που ολοκληρωθεί αυτό το πρόγραμμα εκμάθησης βασικό έλεγχο ταυτότητας, μπορείτε να συνεχίσετε με ένα από τα παρακάτω προγράμματα εκμάθησης:

+ [Προσθέστε τις ειδοποιήσεις push για την εφαρμογή σας](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Μάθετε πώς μπορείτε να προσθέσετε push ειδοποιήσεις υποστήριξη για να την εφαρμογή σας και ρύθμιση παραμέτρων σας εφαρμογή Mobile υποστήριξης για την αποστολή ειδοποιήσεων push Azure ειδοποίηση διανομείς.

+ [Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή σας](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Μάθετε πώς μπορείτε να προσθέσετε την εφαρμογή σας χρησιμοποιώντας μια εφαρμογή Mobile παρασκηνίου υποστήριξη χωρίς σύνδεση. Συγχρονισμού χωρίς σύνδεση επιτρέπει στους τελικούς χρήστες να αλληλεπιδράσετε με μια εφαρμογή για κινητές συσκευές&mdash;προβολή, προσθήκη ή τροποποίηση δεδομένων&mdash;ακόμα και όταν δεν υπάρχει σύνδεση δικτύου.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

