## <a name="obtain-an-azure-resource-manager-token"></a>Αποκτήστε ένα διακριτικό Azure διαχείρισης πόρων

Azure υπηρεσίας καταλόγου Active Directory πρέπει να πραγματοποιήσει έλεγχο ταυτότητας όλες τις εργασίες που μπορείτε να εκτελέσετε σε πόρους, χρησιμοποιώντας τη διαχείριση πόρων Azure. Το παράδειγμα που ακολουθεί χρησιμοποιεί έλεγχο ταυτότητας μέσω κωδικού, για τις άλλες οδούς, δείτε: [αιτήσεις ελέγχου ταυτότητας διαχείριση πόρων Azure][lnk-authenticate-arm].

1. Προσθέστε τον ακόλουθο κώδικα στη μέθοδο **Main** στο Program.cs για να ανακτήσετε ένα διακριτικό από AD Azure χρησιμοποιώντας το αναγνωριστικό εφαρμογής και κωδικό πρόσβασης.

    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.windows.net/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
    
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. Δημιουργήστε ένα αντικείμενο **ResourceManagementClient** που χρησιμοποιεί το διακριτικό, προσθέτοντας τον παρακάτω κώδικα στο τέλος της η **κύρια** μέθοδος:

    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. Δημιουργήσετε ή να αποκτήσετε μια αναφορά, την ομάδα πόρων που χρησιμοποιείτε:

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx