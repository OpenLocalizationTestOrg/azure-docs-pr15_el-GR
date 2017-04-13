
1. Στο αρχείο έργου MainPage.xaml.cs, προσθέστε τις ακόλουθες δηλώσεις **χρησιμοποιώντας** :

        using System.Linq;      
        using Windows.Security.Credentials;

2. Αντικατάσταση της μεθόδου **AuthenticateAsync** με τον ακόλουθο κώδικα:

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    Σε αυτήν την έκδοση του **AuthenticateAsync**, η εφαρμογή προσπαθεί να χρησιμοποιήσει διαπιστευτήρια που είναι αποθηκευμένα στο το **PasswordVault** για πρόσβαση στην υπηρεσία. Ένα κανονικό εισόδου επίσης πραγματοποιείται όταν δεν υπάρχει καμία αποθηκευμένων διαπιστευτηρίων.

    >[AZURE.NOTE]Ενός διακριτικού της μνήμης cache ενδέχεται να έχει λήξει και λήξη διακριτικού μπορεί επίσης να εμφανιστεί μετά τον έλεγχο ταυτότητας, όταν χρησιμοποιείτε την εφαρμογή. Για να μάθετε πώς να καθορίσετε εάν ένα διακριτικό έχει λήξει, ανατρέξτε στο θέμα [Έλεγχος για τα διακριτικά έλεγχος ταυτότητας έχει λήξει](http://aka.ms/jww5vp). Για μια λύση για να χειρισμός εξουσιοδότησης σφαλμάτων που σχετίζονται με διακριτικά λήξης, ανατρέξτε στην καταχώρηση [προσωρινή αποθήκευση και χειρισμός έχει λήξει διακριτικά στις υπηρεσίες Mobile Azure διαχειριζόμενων SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 

3. Επανεκκινήστε την εφαρμογή δύο φορές.

    Παρατηρήστε ότι σε την πρώτη εκκίνηση, πραγματοποιήστε είσοδο με την υπηρεσία παροχής απαιτείται ξανά. Ωστόσο, κατά το δεύτερο επανεκκίνηση χρησιμοποιούνται τα διαπιστευτήρια στο cache και εισόδου είναι θα παραλειφθούν. 
