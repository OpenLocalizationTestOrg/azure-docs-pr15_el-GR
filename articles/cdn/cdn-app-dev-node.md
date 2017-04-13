<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure SDK CDN για Node.js | Microsoft Azure"
    description="Μάθετε πώς να συντάξετε Node.js εφαρμογών για τη Διαχείριση Azure CDN."
    services="cdn"
    documentationCenter="nodejs"
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

Μπορείτε να χρησιμοποιήσετε το [Azure SDK CDN για Node.js](https://www.npmjs.com/package/azure-arm-cdn) για να αυτοματοποιήσετε τη δημιουργία και διαχείριση των προφίλ CDN και τα τελικά σημεία.  Αυτό το πρόγραμμα εκμάθησης παρουσιάζει τη δημιουργία μιας απλής εφαρμογής κονσόλας Node.js που δείχνει πολλές από τις διαθέσιμες λειτουργίες.  Αυτό το πρόγραμμα εκμάθησης δεν προορίζεται για να περιγράψετε όλα τα στοιχεία του Azure CDN SDK για Node.js με λεπτομέρειες.

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, που θα πρέπει να έχετε ήδη [Node.js](http://www.nodejs.org) **4.x.x** ή υψηλότερη εγκατασταθεί και ρυθμιστεί.  Μπορείτε να χρησιμοποιήσετε οποιοδήποτε πρόγραμμα επεξεργασίας κειμένου που θέλετε να δημιουργήσετε την εφαρμογή σας Node.js.  Για να γράψετε αυτό το πρόγραμμα εκμάθησης, να χρησιμοποιηθεί [Κώδικα Visual Studio](https://code.visualstudio.com).  

> [AZURE.TIP] Η [Ολοκλήρωση έργου από αυτό το πρόγραμμα εκμάθησης](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) είναι διαθέσιμο για λήψη στο MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Δημιουργήστε το έργο σας και προσθέστε εξαρτήσεις NPM

Τώρα που θα σας έχετε δημιουργήσει μια ομάδα πόρων για το προφίλ CDN και να δώσει μας Azure AD δικαιώματα εφαρμογής για τη Διαχείριση προφίλ CDN και τελικά σημεία μέσα σε αυτήν την ομάδα, θα σας να ξεκινήσετε τη δημιουργία μας εφαρμογής.

Δημιουργήστε ένα φάκελο για να αποθηκεύσετε την εφαρμογή σας.  Από την κονσόλα με τα εργαλεία Node.js στην τρέχουσα διαδρομή σας, ορίστε την τρέχουσα θέση σας σε αυτόν το νέο φάκελο και προετοιμασία του έργου σας με την εκτέλεση:
    
    npm init
    
Που θα εμφανιστεί μια σειρά από ερωτήσεις για να προετοιμάσετε το έργο σας.  Για το **σημείο εισόδου**, αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί *app.js*.  Μπορείτε να δείτε μου άλλες επιλογές στο παρακάτω παράδειγμα.

![NPM προετοιμασία εξόδου](./media/cdn-app-dev-node/cdn-npm-init.png)

Τώρα είναι η προετοιμασία μας έργου με ένα αρχείο *packages.json* .  Το project πρόκειται να χρησιμοποιήσετε ορισμένες βιβλιοθήκες Azure που περιέχονται σε NPM πακέτων.  Θα χρησιμοποιήσουμε το περιβάλλον εκτέλεσης προγράμματος-πελάτη Azure για Node.js (ms-υπόλοιπο-azure) και τη βιβλιοθήκη Azure CDN προγράμματος-πελάτη για Node.js (azure-arm-cd).  Ας προσθέσουμε εκείνες στο έργο ως εξαρτήσεις.
 
    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Αφού έχετε ολοκληρώσει τα πακέτα την εγκατάσταση, το *package.json* αρχείου θα πρέπει να μοιάζει με αυτό το παράδειγμα (έκδοση αριθμούς ενδέχεται να διαφέρουν):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Τέλος, χρησιμοποιώντας το πρόγραμμα επεξεργασίας κειμένου, δημιουργήστε ένα αρχείο κειμένου κενό και αποθηκεύστε το στη ρίζα της μας φάκελο έργου ως *app.js*.  Τώρα είστε έτοιμοι να ξεκινήσετε τη σύνταξη κώδικα.

## <a name="requires-constants-authentication-and-structure"></a>Απαιτεί, σταθερές, τον έλεγχο ταυτότητας και δομή

Με *app.js* ανοιχτό στο μας επεξεργασίας, ας ξεκινήσουμε τη βασική δομή το πρόγραμμα που έχει συνταχθεί.

1. Προσθέστε το "απαιτεί" για τα πακέτα NPM στο επάνω μέρος με τα εξής:

    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```

2. Πρέπει να ορίσετε ορισμένες σταθερές μας μεθόδους θα χρησιμοποιήσετε.  Προσθέστε τα εξής.  Φροντίστε να αντικαταστήσετε τους χαρακτήρες κράτησης θέσης, όπως το ** &lt;αγκύλες&gt;**, με τις δικές σας τιμές όπως απαιτείται.

    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";

    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Στη συνέχεια, θα σας θα ξεκινήσει το πρόγραμμα-πελάτης διαχείρισης CDN και να της δώσετε μας διαπιστευτήρια.

    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
    
    Εάν χρησιμοποιείτε τον έλεγχο ταυτότητας χρήστη μεμονωμένα, αυτά τα δύο γραμμές θα είναι λίγο διαφορετικά.

    >[AZURE.IMPORTANT] Χρησιμοποιήστε αυτό το δείγμα κώδικα μόνο εάν επιλέξετε να έχουν τον έλεγχο ταυτότητας χρήστη μεμονωμένα αντί για μια υπηρεσία κεφαλαίου.  Να είστε προσεκτικοί για προστασία τα διαπιστευτήριά σας μεμονωμένος χρήστης και να τα διατηρήσετε μυστικό.

    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```

    Φροντίστε να αντικαταστήσετε τα στοιχεία σε ** &lt;αγκύλες&gt; ** με τις σωστές πληροφορίες.  Για `<redirect URI>`, χρησιμοποιήστε την ανακατεύθυνση URI που καταχωρήσατε κατά την καταχώρηση της εφαρμογής στο Azure AD.
    

4.  Μας εφαρμογής κονσόλας Node.js πρόκειται να διαρκέσει ορισμένες παραμέτρους γραμμής εντολών.  Ας επικύρωση ότι τουλάχιστον μία παράμετρος μεταβιβάστηκε.

    ```javascript
    //Collect command-line parameters
    var parms = process.argv.slice(2);

    //Do we have parameters?
    if(parms == null || parms.length == 0)
    {
        console.log("Not enough parameters!");
        console.log("Valid commands are list, delete, create, and purge.");
        process.exit(1);
    }
    ```

5. Που εμφανίζει μας για να το κύριο μέρος του προγράμματος μας, όπου θα σας δημιουργήσετε διακλάδωση άλλες συναρτήσεις που βασίζονται σε ποιες παράμετροι έχουν περάσει.

    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;

        case "create":
            cdnCreate();
            break;
        
        case "delete":
            cdnDelete();
            break;

        case "purge":
            cdnPurge();
            break;

        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```

6.  Σε πολλές θέσεις στο πρόγραμμα μας, θα πρέπει να βεβαιωθείτε ότι το σωστό αριθμό παράμετροι έχουν περάσει στο και εμφανίζει βοήθεια, εάν δεν εμφανίζονται σωστά.  Ας δημιουργήσουμε συναρτήσεις για να το κάνετε.

    ```javascript
    function requireParms(parmCount) {
        if(parms.length < parmCount) {
            usageHelp(parms[0].toLowerCase());
            process.exit(1);
        }
    }

    function usageHelp(cmd) {
        console.log("Usage for " + cmd + ":");
        switch(cmd)
        {
            case "list":
                console.log("list profiles");
                console.log("list endpoints <profile name>");
                break;

            case "create":
                console.log("create profile <profile name>");
                console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
                break;
            
            case "delete":
                console.log("delete profile <profile name>");
                console.log("delete endpoint <profile name> <endpoint name>");
                break;

            case "purge":
                console.log("purge <profile name> <endpoint name> <path>");
                break;

            default:
                console.log("Invalid command.");
        }
    }
    ```

7. Τέλος, οι συναρτήσεις θα χρησιμοποιούμε στο πρόγραμμα-πελάτη διαχείρισης CDN είναι ασύγχρονης, ώστε να χρειάζεται μια μέθοδο για να καλέσετε ξανά όταν ολοκληρώσετε την εργασία τους.  Ας κάνουμε ένα που να εμφανίζουν την έξοδο από το πρόγραμμα-πελάτη διαχείρισης CDN (εάν υπάρχουν) και κλείστε το πρόγραμμα ομαλά.

    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Τώρα που η βασική δομή του στο πρόγραμμα είναι γραμμένο, θα πρέπει να δημιουργήσουμε τις συναρτήσεις που ονομάζεται με βάση τις παραμέτρους μας.

## <a name="list-cdn-profiles-and-endpoints"></a>Λίστα CDN προφίλ και τα τελικά σημεία

Ας ξεκινήσουμε με κώδικα για να παραθέσετε τα υπάρχοντα προφίλ και τα τελικά σημεία.  Σχολίων κώδικα παρέχουν την αναμενόμενη σύνταξη, ώστε να γνωρίζουμε πού μεταβαίνει κάθε παράμετρο.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Δημιουργία προφίλ CDN και τα τελικά σημεία

Στη συνέχεια, θα σας θα σύνταξη των συναρτήσεων για τη δημιουργία προφίλ και τα τελικά σημεία.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Εκκαθάριση τελικού σημείου

Υπό την προϋπόθεση ότι έχει δημιουργηθεί το τελικό σημείο, μια συνηθισμένη εργασία που μπορεί να θέλετε να εκτελέσετε με το πρόγραμμα Εκκαθάριση περιεχομένου στο μας τελικού σημείου.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>Διαγραφή προφίλ CDN και τα τελικά σημεία

Η τελευταία συνάρτηση θα περιλαμβάνονται διαγράφει τα τελικά σημεία και προφίλ.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Εκτέλεση του προγράμματος

Θα σας τώρα μπορεί να εκτελέσει το πρόγραμμα Node.js χρησιμοποιώντας μας αγαπημένες εντοπισμού σφαλμάτων ή στην κονσόλα.

> [AZURE.TIP] Εάν χρησιμοποιείτε το Visual Studio κώδικα εντοπισμού σφαλμάτων, θα χρειαστεί να ρυθμίσετε το περιβάλλον σας για τη μεταβίβαση στο τις παραμέτρους γραμμής εντολών.  Κώδικα Visual Studio το κάνει αυτό στο αρχείο **lanuch.json** .  Αναζητήστε μια ιδιότητα με το όνομα **ορισμάτων** και να προσθέσετε έναν πίνακα τιμών συμβολοσειράς για τις παραμέτρους, ώστε να μοιάζει με αυτήν: `"args": ["list", "profiles"]`.

Ας ξεκινήσουμε με την καταχώρηση μας προφίλ.

![Λίστα προφίλ](./media/cdn-app-dev-node/cdn-list-profiles.png)

Θα σας ξανά έχετε έναν κενό πίνακα.  Επειδή δεν έχουμε τυχόν προφίλ στο μας ομάδα πόρων, που αναμένεται.  Ας δημιουργήσουμε τώρα ένα προφίλ.

![Δημιουργία προφίλ](./media/cdn-app-dev-node/cdn-create-profile.png)

Τώρα, ας προσθέσουμε τελικού σημείου.

![Δημιουργία τελικού σημείου](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Τέλος, ας διαγραφή μας προφίλ.

![Διαγραφή προφίλ](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Επόμενα βήματα

Για να δείτε το ολοκληρωμένο έργο από αυτόν τον οδηγό, [κάντε λήψη του δείγματος](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

Για να δείτε την αναφορά για το SDK CDN Azure για Node.js, προβάλετε την [αναφορά](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

Για να βρείτε πρόσθετες τεκμηρίωση του SDK Azure για Node.js, προβάλετε την [πλήρη αναφορά](http://azure.github.io/azure-sdk-for-node/).

Διαχειριστείτε τους πόρους σας CDN με το [PowerShell](./cdn-manage-powershell.md).

