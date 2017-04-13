<properties
    pageTitle="Γρήγορα αποτελέσματα με τη βιβλιοθήκη CDN Azure για .NET | Microsoft Azure"
    description="Μάθετε πώς να συντάξετε τις εφαρμογές για να διαχειριστείτε CDN Azure χρήση του Visual Studio .NET."
    services="cdn"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Γρήγορα αποτελέσματα με το Azure CDN ανάπτυξης

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

Μπορείτε να χρησιμοποιήσετε τη [Βιβλιοθήκη CDN Azure για .NET](https://msdn.microsoft.com/library/mt657769.aspx) για να αυτοματοποιήσετε τη δημιουργία και διαχείριση των προφίλ CDN και τα τελικά σημεία.  Αυτό το πρόγραμμα εκμάθησης παρουσιάζει τη δημιουργία μιας απλής εφαρμογής κονσόλας .NET που δείχνει πολλές από τις διαθέσιμες λειτουργίες.  Αυτό το πρόγραμμα εκμάθησης δεν προορίζεται για να περιγράψετε όλα τα στοιχεία της βιβλιοθήκης CDN Azure για το .NET με λεπτομέρειες.

Χρειάζεστε Visual Studio 2015, για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης.  [Visual Studio Κοινότητας 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) είναι διαθέσιμα για δωρεάν λήψη.

> [AZURE.TIP] Η [Ολοκλήρωση έργου από αυτό το πρόγραμμα εκμάθησης](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) είναι διαθέσιμο για λήψη στο MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Δημιουργήστε το έργο σας και προσθέστε πακέτα Nuget

Τώρα που θα σας έχετε δημιουργήσει μια ομάδα πόρων για το προφίλ CDN και να δώσει μας Azure AD δικαιώματα εφαρμογής για τη Διαχείριση προφίλ CDN και τελικά σημεία μέσα σε αυτήν την ομάδα, θα σας να ξεκινήσετε τη δημιουργία μας εφαρμογής.

Από το Visual Studio 2015, κάντε κλικ στην επιλογή **αρχείο**, **Δημιουργία**, **... έργου** για να ανοίξετε το παράθυρο διαλόγου νέου έργου.  Αναπτύξτε το στοιχείο **Visual C#**και, στη συνέχεια, επιλέξτε **Windows** στο παράθυρο στα αριστερά.  Στο κεντρικό παράθυρο, κάντε κλικ στην επιλογή **Εφαρμογή κονσόλας** .  Ονομάστε το έργο σας και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.  

![Νέο έργο](./media/cdn-app-dev-net/cdn-new-project.png)

Το project πρόκειται να χρησιμοποιήσετε ορισμένες βιβλιοθήκες Azure που περιέχονται σε Nuget πακέτων.  Ας προσθέσουμε εκείνες στο έργο.

1. Κάντε κλικ στο κουμπί του μενού **Εργαλεία** , **Διαχείριση πακέτου Nuget**και στη συνέχεια **Κονσόλα διαχείρισης πακέτου**.

    ![Διαχείριση πακέτων Nuget](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. Στην κονσόλα διαχείρισης πακέτου, εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε τη **Βιβλιοθήκη ελέγχου ταυτότητας Active Directory (ADAL)**:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. Εκτελέστε τα ακόλουθα για να εγκαταστήσετε τη **Βιβλιοθήκη διαχείρισης CDN Azure**:

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Οδηγίες, σταθερές, κύρια μέθοδος και οι μέθοδοι Βοήθειας

Η βασική δομή του το πρόγραμμα που έχει συνταχθεί, ας ξεκινήσουμε.

1. Επιστρέψτε στην καρτέλα Program.cs, αντικαταστήστε το `using` οδηγιών στο επάνω μέρος με τα εξής:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

2. Πρέπει να ορίσετε ορισμένες σταθερές μας μεθόδους θα χρησιμοποιήσετε.  Στο το `Program` κλάση, αλλά πριν την `Main` μέθοδο, προσθέστε τα εξής.  Φροντίστε να αντικαταστήσετε τους χαρακτήρες κράτησης θέσης, όπως το ** &lt;αγκύλες&gt;**, με τις δικές σας τιμές όπως απαιτείται.

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Επίσης στο επίπεδο τάξης, να ορίσετε αυτές τις δύο μεταβλητές.  Θα χρησιμοποιήσουμε αυτά αργότερα για να προσδιορίσετε αν το προφίλ και το τελικό σημείο υπάρχει ήδη.

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```

4.  Αντικαταστήστε το `Main` μέθοδο ως εξής:

    ```csharp
    static void Main(string[] args)
    {
        //Get a token
        AuthenticationResult authResult = GetAccessToken();

        // Create CDN client
        CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
            { SubscriptionId = subscriptionId };

        ListProfilesAndEndpoints(cdn);

        // Create CDN Profile
        CreateCdnProfile(cdn);

        // Create CDN Endpoint
        CreateCdnEndpoint(cdn);
        
        Console.WriteLine();

        // Purge CDN Endpoint
        PromptPurgeCdnEndpoint(cdn);

        // Delete CDN Endpoint
        PromptDeleteCdnEndpoint(cdn);

        // Delete CDN Profile
        PromptDeleteCdnProfile(cdn);

        Console.WriteLine("Press Enter to end program.");
        Console.ReadLine();
    }
    ```

5. Ορισμένα από τα άλλες μεθόδους πρόκειται να γίνεται ερώτηση στο χρήστη με ερωτήσεις "Ναι/Όχι".  Προσθέστε την ακόλουθη μέθοδο για να διευκολύνετε την που λίγο:

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Τώρα που η βασική δομή του στο πρόγραμμα είναι γραμμένο, θα πρέπει να δημιουργήσουμε τις μεθόδους που ονομάζεται το `Main` μέθοδο.

## <a name="authentication"></a>Έλεγχος ταυτότητας

Πριν να χρησιμοποιήσουμε τη βιβλιοθήκη διαχείρισης CDN Azure, πρέπει να ελέγχουν την ταυτότητα μας κεφάλαιο υπηρεσίας και να αποκτήσετε ένα διακριτικό έλεγχο ταυτότητας.  Αυτή η μέθοδος χρησιμοποιεί ADAL για να ανακτήσετε το διακριτικό.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Εάν χρησιμοποιείτε τον έλεγχο ταυτότητας μεμονωμένο χρήστη, η `GetAccessToken` μέθοδο θα είναι λίγο διαφορετικά.

>[AZURE.IMPORTANT] Χρησιμοποιήστε αυτό το δείγμα κώδικα μόνο εάν επιλέξετε να έχουν τον έλεγχο ταυτότητας χρήστη μεμονωμένα αντί για μια υπηρεσία κεφαλαίου.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Φροντίστε να αντικαταστήσετε `<redirect URI>` με την ανακατεύθυνση URI που καταχωρήσατε κατά την καταχώρηση της εφαρμογής στο Azure AD.

## <a name="list-cdn-profiles-and-endpoints"></a>Λίστα CDN προφίλ και τα τελικά σημεία

Τώρα είστε έτοιμοι να εκτελέσετε λειτουργίες CDN.  Το πρώτο πράγμα που κάνει μας μέθοδος είναι λίστα όλων των προφίλ και τελικών σημείων στο μας ομάδα πόρων και, εάν εντοπίζει μια αντιστοιχία για τα ονόματα των προφίλ και τελικού σημείου που καθορίζεται στο μας σταθερές, δημιουργεί μια σημείωση που για αργότερα, ώστε να μην Προσπαθούμε να δημιουργήσετε διπλότυπα δεδομένα.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Δημιουργία προφίλ CDN και τα τελικά σημεία

Στη συνέχεια, θα δημιουργήσουμε ένα προφίλ.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Αφού δημιουργηθεί το προφίλ, θα δημιουργήσουμε ένα τελικό σημείο.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

>[AZURE.NOTE] Το παραπάνω παράδειγμα εκχωρεί το τελικό σημείο μιας προέλευσης που ονομάζεται *Contoso* με ένα όνομα κεντρικού υπολογιστή `www.contoso.com`.  Θα πρέπει να αλλάξετε έτσι ώστε να οδηγούν στο origin το δικό σας όνομα κεντρικού υπολογιστή.

## <a name="purge-an-endpoint"></a>Εκκαθάριση τελικού σημείου

Υπό την προϋπόθεση ότι έχει δημιουργηθεί το τελικό σημείο, μια συνηθισμένη εργασία που μπορεί να θέλετε να εκτελέσετε με το πρόγραμμα Εκκαθάριση του περιεχομένου στο μας τελικού σημείου.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

>[AZURE.NOTE] Στο παραπάνω παράδειγμα, η συμβολοσειρά `/*` υποδηλώνει ότι θέλετε να εκκαθαρίσετε όλα τα στοιχεία στον ριζικό κατάλογο της διαδρομής τελικού σημείου.  Αυτό είναι παρόμοιο με τον έλεγχο **Εκκαθάριση όλων** στο παράθυρο διαλόγου "Εκκαθάριση" την Azure πύλη. Στο το `CreateCdnProfile` μέθοδο, να δημιουργηθεί το προφίλ ως ένα προφίλ **CDN Azure από Verizon** , χρησιμοποιώντας τον κωδικό `Sku = new Sku(SkuName.StandardVerizon)`, έτσι ώστε αυτή θα ολοκληρωθεί με επιτυχία.  Ωστόσο, **CDN Azure από Akamai** προφίλ δεν υποστηρίζουν **Εκκαθάριση όλων**, ώστε να εάν να χρησιμοποιώντας ένα προφίλ Akamai για αυτό το πρόγραμμα εκμάθησης, θα πρέπει να συμπεριλάβετε συγκεκριμένες διαδρομές για να γίνει εκκαθάριση.

## <a name="delete-cdn-profiles-and-endpoints"></a>Διαγραφή προφίλ CDN και τα τελικά σημεία

Τις τελευταίες μεθόδους θα διαγράψει του τελικού σημείου και των προφίλ.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Εκτέλεση του προγράμματος

Τώρα να μεταγλωττίσετε και να εκτελέσετε το πρόγραμμα κάνοντας κλικ στο κουμπί **Έναρξη** στο Visual Studio.

![Πρόγραμμα που εκτελείται](./media/cdn-app-dev-net/cdn-program-running-1.png)

Όταν το πρόγραμμα φτάσει στην παραπάνω ερώτηση, θα πρέπει να μπορείτε να επιστρέψετε στη ομάδα πόρων στην πύλη του Azure και να δείτε ότι έχει δημιουργηθεί το προφίλ.

![Επιτυχίας!](./media/cdn-app-dev-net/cdn-success.png)

Στη συνέχεια, να επιβεβαιώνουμε τις οδηγίες για να εκτελέσετε το υπόλοιπο του προγράμματος.

![Ολοκλήρωση προγράμματος](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Επόμενα βήματα

Για να δείτε το ολοκληρωμένο έργο από αυτόν τον οδηγό, [κάντε λήψη του δείγματος](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

Για να βρείτε πρόσθετες τεκμηρίωση σχετικά με τη βιβλιοθήκη Azure CDN διαχείρισης για το .NET, προβάλετε την [αναφορά στο MSDN](https://msdn.microsoft.com/library/mt657769.aspx).

Διαχειριστείτε τους πόρους σας CDN με το [PowerShell](./cdn-manage-powershell.md).


