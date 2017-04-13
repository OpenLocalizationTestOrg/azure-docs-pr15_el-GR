<properties
    pageTitle="B2C καταλόγου Azure Active Directory: Χρησιμοποιήστε το γράφημα API | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να καλέσετε το API Graph για ένα μισθωτή B2C, χρησιμοποιώντας μια ταυτότητα της εφαρμογής για να αυτοματοποιήσετε τη διαδικασία."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure AD B2C: Χρήση του API του γραφήματος

Azure Active Directory (Azure AD) B2C μισθωτές την τάση να είναι πολύ μεγάλα. Αυτό σημαίνει ότι πολλές κοινές εργασίες διαχείρισης μισθωτή πρέπει να εκτελεστούν μέσω προγραμματισμού. Ένα παράδειγμα κύρια είναι διαχείριση χρηστών. Ίσως χρειαστεί να μετεγκαταστήσετε έναν υπάρχοντα χώρο αποθήκευσης του χρήστη στο μισθωτή B2C. Εάν θέλετε, μπορείτε να φιλοξενήσετε δήλωσης χρήστη στη δική σας σελίδα και δημιουργήστε λογαριασμούς χρηστών στο Azure AD τι συμβαίνει στο παρασκήνιο. Αυτοί οι τύποι εργασιών απαιτούν τη δυνατότητα για τη δημιουργία, ανάγνωση, ενημέρωση και διαγραφή λογαριασμών χρηστών. Μπορείτε να κάνετε αυτές τις εργασίες με τη χρήση του API Azure AD Graph.

Για μισθωτές B2C, υπάρχουν δύο κύρια καταστάσεις επικοινωνία με το API του γραφήματος.

- Για διαδραστική, μίας εκτέλεσης εργασίες που πρέπει να λειτουργεί ως ένα λογαριασμό διαχειριστή μισθωτή B2C κατά την εκτέλεση των εργασιών. Αυτή η κατάσταση λειτουργίας απαιτεί από ένα διαχειριστή για να συνδεθείτε με τα διαπιστευτήρια που διαχείρισης για να εκτελέσετε οποιαδήποτε κλήσεων για το API Graph.
- Για αυτόματη, λόγω των συνεχών εργασίες, θα πρέπει να χρησιμοποιείτε κάποιον τύπο του λογαριασμού υπηρεσίας που παρέχετε με τα απαραίτητα δικαιώματα για την εκτέλεση εργασιών διαχείρισης. Στο Azure AD, μπορείτε να το κάνετε καταχωρώντας μια εφαρμογή και τον έλεγχο ταυτότητας για να Azure AD. Αυτό γίνεται με το **Αναγνωριστικό εφαρμογής** που χρησιμοποιεί την [Εκχώρηση 2.0 διακριτικό διαπιστευτήρια προγράμματος-πελάτη](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). Σε αυτήν την περίπτωση, η εφαρμογή λειτουργεί ως μόνη της, όχι ως ένα χρήστη, για να καλέσετε το API του γραφήματος.

Σε αυτό το άρθρο, θα εξετάσουμε πώς να εκτελείτε την υπόθεση αυτοματοποιημένη χρήση. Για μια επίδειξη, θα μπορέσουμε να δημιουργήσουμε μια διαίρεσης 4,5 .NET `B2CGraphClient` που εκτελεί χρήστη δημιουργία, ανάγνωση, ενημέρωση και διαγραφή λειτουργίες (CRUD). Ο υπολογιστής-πελάτης θα έχει ένα περιβάλλον γραμμής εντολών των Windows (CLI) που σας επιτρέπει να καλέσετε διάφορες μεθόδους. Ωστόσο, ο κώδικας εγγράφεται συμπεριφέρονται με τον τρόπο μη αλληλεπιδραστικό, αυτοματοποιημένη.

## <a name="get-an-azure-ad-b2c-tenant"></a>Λάβετε ένα μισθωτή του Azure AD B2C

Πριν από την μπορείτε να δημιουργήσετε εφαρμογές ή τους χρήστες ή να αλληλεπιδράσετε με Azure AD καθόλου, θα χρειαστείτε ένα μισθωτή του Azure AD B2C και ένα λογαριασμό καθολικού διαχειριστή μισθωτή. Εάν δεν έχετε έναν μισθωτή ήδη, [Γρήγορα αποτελέσματα με το Azure AD B2C](active-directory-b2c-get-started.md).

## <a name="register-a-service-application-in-your-tenant"></a>Καταχώρηση μιας εφαρμογής υπηρεσίας στο μισθωτή σας

Αφού έχετε B2C μισθωτή, πρέπει να δημιουργήσετε την εφαρμογή υπηρεσίας με τη χρήση των cmdlet του Azure AD PowerShell.
Πρώτα, κάντε λήψη και εγκαταστήστε το [Βοηθό Microsoft Online Services εισόδου](http://go.microsoft.com/fwlink/?LinkID=286152). Στη συνέχεια, κάντε λήψη και εγκαταστήστε τη [λειτουργική μονάδα Azure Active Directory 64 bit για το Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

> [AZURE.IMPORTANT]
Για να χρησιμοποιήσετε το API του γραφήματος με το μισθωτή B2C, θα πρέπει να καταχωρήσετε μια αποκλειστική εφαρμογή με χρήση του PowerShell. Ακολουθήστε τις οδηγίες σε αυτό το άρθρο για να το κάνετε. Δεν μπορείτε να χρησιμοποιήσετε ξανά τις ήδη υπάρχουσες εφαρμογές B2C που καταχωρήσατε στην πύλη του Azure.

Μετά την εγκατάσταση της λειτουργικής μονάδας PowerShell, ανοίξτε το PowerShell και συνδεθείτε με το μισθωτή B2C. Αφού εκτελέσετε `Get-Credential`, θα σας ζητηθεί για ένα όνομα χρήστη και τον κωδικό πρόσβασης, πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης για το λογαριασμό διαχειριστή μισθωτή B2C.

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

Πριν να δημιουργήσετε την εφαρμογή σας, πρέπει να δημιουργήσετε μια νέα **μυστικό προγράμματος-πελάτη**.  Η εφαρμογή σας θα χρησιμοποιήσει το μυστικό προγράμματος-πελάτη για τον έλεγχο ταυτότητας Azure AD και να αποκτήσετε πρόσβαση διακριτικά. Μπορείτε να δημιουργήσετε έναν έγκυρο μυστικό της PowerShell:

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

Η εντολή τελική θα πρέπει να εκτυπώσετε το νέο πρόγραμμα-πελάτη μυστικό. Αντιγράψτε το σε ασφαλές. Που θα το χρειαστείτε αργότερα. Τώρα μπορείτε να δημιουργήσετε την εφαρμογή σας, παρέχοντας το νέο πρόγραμμα-πελάτη μυστικό ως διαπιστευτηρίου για την εφαρμογή:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

Εάν δημιουργήσετε με επιτυχία την εφαρμογή, αυτό θα πρέπει να εκτυπώσετε ιδιοτήτων της εφαρμογής, όπως τα παραπάνω. Θα χρειαστεί και τα δύο `ObjectId` και `AppPrincipalId`, επομένως, αντιγράψτε αυτές τις τιμές, πολύ.

Αφού δημιουργήσετε μια εφαρμογή στο μισθωτή σας B2C, πρέπει να εκχωρήσετε δικαιώματα που χρειάζεται για να εκτελέσετε λειτουργίες CRUD χρήστη. Εκχώρηση οι τρεις ρόλοι της εφαρμογής: οι αναγνώστες καταλόγου (για να διαβάσετε χρήστες), οι συντάκτες καταλόγου (για να δημιουργήσετε και να ενημερώσετε τους χρήστες), και ένα διαχειριστή λογαριασμού χρήστη (για να διαγράψετε χρήστες). Αυτούς τους ρόλους έχουν τις γνωστές αναγνωριστικά, ώστε να μπορείτε να αντικαταστήσετε το `-RoleMemberObjectId` παράμετρος με `ObjectId` από το παραπάνω και εκτελέστε τις ακόλουθες εντολές. Για να δείτε τη λίστα με όλους τους ρόλους καταλόγου, προσπαθήστε να εκτελέσετε `Get-MsolRole`.

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

Τώρα έχετε μια εφαρμογή που έχει δικαιώματα για δημιουργία, ανάγνωση, ενημέρωση και διαγραφή χρηστών από το μισθωτή B2C.

## <a name="download-configure-and-build-the-sample-code"></a>Λήψη, να ρυθμίσετε τις παραμέτρους και να δομήσετε το δείγμα κώδικα

Πρώτα, κάντε λήψη του δείγματος κώδικα και να εκτελείται. Στη συνέχεια, θα σας θα διαρκέσει μια πιο κοντινή ματιά στην αυτό.  Μπορείτε να [κάνετε λήψη του δείγματος κώδικα ως αρχείο .zip](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Μπορείτε επίσης να το αντιγράψετε σε έναν κατάλογο της επιλογής σας:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Άνοιγμα του `B2CGraphClient\B2CGraphClient.sln` λύσης Visual Studio στο Visual Studio. Στο το `B2CGraphClient` του project, ανοίξτε το αρχείο `App.config`. Αντικαταστήστε τις ρυθμίσεις εφαρμογής τρεις με τις δικές σας τιμές:

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Στη συνέχεια, κάντε δεξί κλικ στη το `B2CGraphClient` λύση και εκ νέου δημιουργία του δείγματος. Εάν είστε επιτυχής, θα πρέπει να έχετε τώρα μια `B2C.exe` εκτελέσιμο αρχείο που βρίσκεται σε `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Δημιουργία λειτουργίες CRUD χρήστη με τη χρήση του API του γραφήματος

Για να χρησιμοποιήσετε το B2CGraphClient, ανοίξτε μια `cmd` Windows εντολή ερώτηση και αλλάξτε τον κατάλογο για να το `Debug` καταλόγου. Στη συνέχεια, εκτελέστε το `B2C Help` εντολή.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Αυτό θα εμφανίσει μια σύντομη περιγραφή κάθε εντολή. Κάθε φορά που μπορείτε να ενεργοποιήσετε μία από αυτές τις εντολές, `B2CGraphClient` κάνει μια αίτηση για το API Azure AD Graph.

### <a name="get-an-access-token"></a>Λάβετε ένα διακριτικό πρόσβασης

Οποιαδήποτε αίτηση για το API Graph απαιτεί ένα διακριτικό πρόσβασης για τον έλεγχο ταυτότητας. `B2CGraphClient`χρησιμοποιεί το άνοιγμα προέλευσης Active Directory ελέγχου ταυτότητας βιβλιοθήκη (ADAL) για να αποκτήσετε πρόσβαση διακριτικά. ADAL διευκολύνει την διακριτικού acquisition, παρέχοντας ένα απλό API και φροντίζοντας ορισμένες σημαντικές λεπτομέρειες, όπως σε cache διακριτικά πρόσβασης. Δεν χρειάζεται να χρησιμοποιήσετε ADAL για να λάβετε τα διακριτικά, μέσω. Μπορείτε επίσης να λάβετε τα διακριτικά δημιουργώντας αιτήσεις HTTP.

> [AZURE.NOTE]
    Αυτό το δείγμα κώδικα χρησιμοποιεί ADAL v2 προκειμένου να επικοινωνήσετε με το API του γραφήματος.  Πρέπει να χρησιμοποιήσετε ADAL v2 ή v3 για να λάβετε διακριτικά πρόσβασης που μπορεί να χρησιμοποιηθεί με το API Azure AD Graph.

Όταν `B2CGraphClient` εκτελείται, που δημιουργεί μια παρουσία του `B2CGraphClient` τάξης. Η κατασκευή για αυτήν την κλάση ρυθμίζει ένα ικριώματος ADAL ελέγχου ταυτότητας:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Θα χρησιμοποιήσουμε το `B2C Get-User` εντολής ως παράδειγμα. Όταν `B2C Get-User` ενεργοποιείται χωρίς τυχόν πρόσθετες εισροές, οι κλήσεις CLI το `B2CGraphClient.GetAllUsers(...)` μέθοδο. Αυτή η μέθοδος κλήσεις `B2CGraphClient.SendGraphGetRequest(...)`, που υποβάλλει μια αίτηση HTTP GET για το API Graph. Πριν από την `B2CGraphClient.SendGraphGetRequest(...)` αποστέλλει αίτηση ΓΡΉΓΟΡΑ, αυτό πρώτα αποκτά πρόσβαση σε διακριτικού με χρήση του ADAL:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Μπορείτε να λάβετε μια πρόσβαση διακριτικού για το API Graph, καλώντας την ADAL `AuthenticationContext.AcquireToken(...)` μέθοδο. ADAL, στη συνέχεια, επιστρέφει ένα `access_token` που αντιπροσωπεύει την ταυτότητα της εφαρμογής.

### <a name="read-users"></a>Διαβάστε τους χρήστες

Όταν θέλετε να λάβετε μια λίστα με τους χρήστες ή να λάβετε ένα συγκεκριμένο χρήστη από το API του γραφήματος, μπορείτε να στείλετε ένα HTTP `GET` αίτηση για να το `/users` τελικού σημείου. Μια αίτηση για όλους τους χρήστες στο μισθωτή μοιάζει κάπως έτσι:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Για να δείτε αυτήν την πρόσκληση, εκτελέστε:

 ```
 > B2C Get-User
 ```

Υπάρχουν δύο σημαντικά πράγματα που πρέπει να λάβετε υπόψη:

- Προστίθεται το διακριτικό πρόσβασης που αποκτήθηκε μέσω ADAL το `Authorization` κεφαλίδας, χρησιμοποιώντας το `Bearer` συνδυασμού.
- Για μισθωτές B2C, πρέπει να χρησιμοποιήσετε την παράμετρο ερωτήματος `api-version=1.6`.

Και τα δύο από αυτές τις λεπτομέρειες γίνεται με το `B2CGraphClient.SendGraphGetRequest(...)` μέθοδο:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Δημιουργία λογαριασμών χρηστών καταναλωτή

Όταν δημιουργείτε λογαριασμούς χρηστών στο μισθωτή σας B2C, μπορείτε να στείλετε ένα HTTP `POST` αίτηση για να το `/users` τελικού σημείου:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Για να δημιουργήσετε χρήστες λιανικής πώλησης απαιτούνται περισσότερες από αυτές τις ιδιότητες σε αυτήν την αίτηση. Για να μάθετε περισσότερα, κάντε κλικ [εδώ](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Λάβετε υπόψη ότι η `//` σχόλια έχουν συμπεριληφθεί για απεικόνιση. Μην συμπεριλαμβάνετε τους σε μια πραγματική αίτηση.

Για να δείτε την αίτηση, εκτελέστε μία από τις παρακάτω εντολές:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

Το `Create-User` εντολή λαμβάνει ένα αρχείο .json ως μια παράμετρο εισόδου. Περιέχει μια αναπαράσταση JSON ενός αντικειμένου χρήστη. Υπάρχουν δύο δείγματα αρχείων .json στο δείγμα κώδικα: `usertemplate-email.json` και `usertemplate-username.json`. Μπορείτε να τροποποιήσετε αυτά τα αρχεία στις ανάγκες σας. Εκτός από τα απαιτούμενα πεδία παραπάνω, περιλαμβάνονται διάφορα προαιρετικά πεδία που μπορείτε να χρησιμοποιήσετε σε αυτά τα αρχεία. Μπορείτε να βρείτε λεπτομέρειες σχετικά με τα προαιρετικά πεδία στην [αναφορά οντότητας API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Μπορείτε να δείτε πώς η πρόσκληση σε ΔΗΜΟΣΊΕΥΣΗ έχει συνταχθεί σε `B2CGraphClient.SendGraphPostRequest(...)`.

- Που αποδίδει ένα διακριτικό πρόσβασης για το `Authorization` κεφαλίδα της αίτησης.
- Ορίζει `api-version=1.6`.
- Περιλαμβάνει το αντικείμενο χρήστη JSON στο κυρίως κείμενο της αίτησης.

> [AZURE.NOTE]
Εάν οι λογαριασμοί που θέλετε να κάνετε μετεγκατάσταση από έναν υπάρχοντα χώρο αποθήκευσης χρήστη διαθέτει κάτω ισχύος κωδικού πρόσβασης από το [επιβάλλονται από Azure AD B2C θραύσης ισχυρό κωδικό πρόσβασης](https://msdn.microsoft.com/library/azure/jj943764.aspx), μπορείτε να απενεργοποιήσετε την απαίτηση ισχυρού κωδικού πρόσβασης χρησιμοποιώντας το `DisableStrongPassword` τιμή του ορίσματος το `passwordPolicies` ιδιότητα. Για παράδειγμα, μπορείτε να τροποποιήσετε την αίτηση χρήστη δημιουργίας που παρέχονται παραπάνω ως εξής: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Ενημερώστε τους λογαριασμούς χρηστών καταναλωτή

Όταν ενημερώνετε αντικειμένων χρήστη, η διαδικασία είναι παρόμοια με αυτή που χρησιμοποιείτε για να δημιουργήσετε αντικείμενα χρήστη. Αλλά αυτή η διαδικασία χρησιμοποιεί το HTTP `PATCH` μέθοδο:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

Προσπαθήστε να ενημερώσετε ένα χρήστη με την ενημέρωση των αρχείων σας JSON με νέα δεδομένα. Μπορείτε να χρησιμοποιήσετε `B2CGraphClient` για να εκτελέσετε μία από αυτές τις εντολές:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Έλεγχος του `B2CGraphClient.SendGraphPatchRequest(...)` μέθοδο για λεπτομέρειες σχετικά με τον τρόπο για να στείλετε αυτήν την αίτηση.

### <a name="search-users"></a>Οι χρήστες αναζήτησης

Μπορείτε να αναζητήσετε για τους χρήστες στο μισθωτή σας B2C με δύο τρόπους. Ένα, με χρήση του χρήστη αντικείμενο Αναγνωριστικό ή δύο, με χρήση του χρήστη εισόδου αναγνωριστικό (π.χ., το `signInNames` ιδιότητα).

Εκτελέστε μία από τις παρακάτω εντολές για να αναζητήσετε έναν συγκεκριμένο χρήστη:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Ακολουθούν μερικά παραδείγματα:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Διαγραφή χρηστών

Η διαδικασία για τη διαγραφή ενός χρήστη είναι απλή. Χρησιμοποιήστε το HTTP `DELETE` μέθοδο και τη δομή της διεύθυνσης URL με το σωστό Αναγνωριστικό αντικειμένου:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Για να δείτε ένα παράδειγμα, πληκτρολογήστε την ακόλουθη εντολή και δείτε την αίτηση διαγραφή που εκτυπώνεται στην κονσόλα:

```
> B2C Delete-User <object-id-of-user>
```

Έλεγχος του `B2CGraphClient.SendGraphDeleteRequest(...)` μέθοδο για λεπτομέρειες σχετικά με τον τρόπο για να στείλετε αυτήν την αίτηση.

Μπορείτε να εκτελέσετε πολλές άλλες ενέργειες με το API Azure AD Graph εκτός από τη Διαχείριση χρηστών. Η [αναφορά API Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) παρέχει λεπτομέρειες σχετικά με κάθε ενέργεια, μαζί με δείγματα προσκλήσεων.

## <a name="use-custom-attributes"></a>Χρησιμοποιήστε τα προσαρμοσμένα χαρακτηριστικά

Περισσότερες εφαρμογές καταναλωτή πρέπει να αποθηκεύσετε ορισμένες πληροφορίες προφίλ χρήστη προσαρμοσμένο τύπο. Ένας τρόπος να κάνετε αυτό είναι για να ορίσετε ένα προσαρμοσμένο χαρακτηριστικό στο μισθωτή σας B2C. Μπορείτε, στη συνέχεια, να επιλογή θεωρήστε ότι το χαρακτηριστικό τον ίδιο τρόπο που διαχειρίζεστε οποιαδήποτε άλλη ιδιότητα σε ένα αντικείμενο χρήστη. Μπορείτε να ενημερώσετε το χαρακτηριστικό, να διαγράψετε το χαρακτηριστικό, να ερωτήματος από το χαρακτηριστικό, να στείλετε το χαρακτηριστικό ως αξίωση στο εισόδου διακριτικά και πολλά άλλα.

Για να ορίσετε ένα προσαρμοσμένο χαρακτηριστικό στο μισθωτή σας B2C, ανατρέξτε στο άρθρο [αναφορά προσαρμοσμένο χαρακτηριστικό B2C](active-directory-b2c-reference-custom-attr.md).

Μπορείτε να προβάλετε τα προσαρμοσμένα χαρακτηριστικά που ορίζονται στο μισθωτή σας B2C με τη χρήση `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Το αποτέλεσμα από αυτές τις συναρτήσεις αποκαλύπτει τις λεπτομέρειες της κάθε προσαρμοσμένο χαρακτηριστικό, όπως:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Μπορείτε να χρησιμοποιήσετε το πλήρες όνομα, όπως `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, ως μια ιδιότητα στο τα αντικείμενα χρήστη.  Ενημερώσετε το αρχείο .json με τη νέα ιδιότητα και μια τιμή για την ιδιότητα και, στη συνέχεια, εκτελέστε:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Με τη χρήση `B2CGraphClient`, έχετε μια εφαρμογή υπηρεσίας που μπορούν να διαχειρίζονται τους χρήστες σας B2C μισθωτή μέσω προγραμματισμού. `B2CGraphClient`χρησιμοποιεί το δικό της ταυτότητα της εφαρμογής για τον έλεγχο ταυτότητας για το API Azure AD Graph. Αποκτά επίσης τα διακριτικά, χρησιμοποιώντας ένα πρόγραμμα-πελάτη μυστικό. Όπως μπορείτε να ενσωματώσετε αυτήν τη λειτουργικότητα στην εφαρμογή σας, να θυμάστε ορισμένα βασικά σημεία για τις εφαρμογές B2C:

- Πρέπει να εκχωρήσετε στην εφαρμογή τα κατάλληλα δικαιώματα στο μισθωτή.
- Προς το παρόν, πρέπει να χρησιμοποιήσετε ADAL v2 για να λάβετε διακριτικά πρόσβασης. (Μπορείτε επίσης να στείλετε μηνύματα πρωτοκόλλων απευθείας, χωρίς να χρησιμοποιήσετε μια βιβλιοθήκη.)
- Όταν καλείτε το API του γραφήματος, χρησιμοποιήστε `api-version=1.6`.
- Όταν δημιουργείτε και ενημέρωση χρήστες λιανικής πώλησης, μερικές ιδιότητες απαιτούνται, όπως περιγράφεται παραπάνω.

Εάν έχετε ερωτήσεις ή αιτήσεις για τις ενέργειες που θέλετε να εκτελέσετε με τη χρήση του API Graph στο μισθωτή σας B2C, αφήστε το σχόλιό σας σε αυτό το άρθρο ή αρχείων κάποιο πρόβλημα στο αποθετήριο δείγματα κώδικα GitHub.
