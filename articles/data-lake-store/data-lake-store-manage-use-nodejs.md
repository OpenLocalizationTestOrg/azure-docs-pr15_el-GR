<properties 
   pageTitle="Γρήγορα αποτελέσματα με το Azure αποθηκεύει λίμνης δεδομένων με χρήση του Azure SDK για Node.js | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Node.js για να εργαστείτε με λογαριασμούς χώρου αποθήκευσης λίμνης δεδομένων και το σύστημα αρχείων." 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>Γρήγορα αποτελέσματα με το Azure αποθήκευσης λίμνης δεδομένων με χρήση του Azure SDK για Node.js

> [AZURE.SELECTOR]
- [Πύλη](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)


Μάθετε πώς μπορείτε να χρησιμοποιήσετε στο SDK Azure για Node.js για να δημιουργήσετε ένα λογαριασμό χώρου αποθήκευσης λίμνης Azure δεδομένων και να εκτελέσετε βασικές λειτουργίες όπως όπως δημιουργία φακέλων, αποστολή και λήψη αρχείων δεδομένων, να διαγράψετε το λογαριασμό, κ.λπ. Για περισσότερες πληροφορίες σχετικά με το χώρο αποθήκευσης λίμνης δεδομένων, ανατρέξτε στο θέμα [Επισκόπηση της λίμνης χώρου αποθήκευσης δεδομένων](data-lake-store-overview.md). Προς το παρόν, το υποστηρίζει SDK

  *  **Έκδοση node.js: 0.10.0 ή νεότερη έκδοση**
  *  **Έκδοση REST API για το λογαριασμό: 2015-10-01-preview**
  *  **Έκδοση REST API για το σύστημα αρχείων: 2015-10-01-preview**

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

- **Δημιουργία μιας εφαρμογής υπηρεσίας καταλόγου Azure Active Directory**. Μπορείτε να χρησιμοποιήσετε την εφαρμογή Azure AD για τον έλεγχο ταυτότητας της εφαρμογής αποθήκευσης λίμνης δεδομένων με το Azure AD. Υπάρχουν διαφορετικές μεθόδους για τον έλεγχο ταυτότητας με Azure AD, που είναι **τελικού χρήστη ελέγχου ταυτότητας** ή **τον έλεγχο ταυτότητας υπηρεσίας εξυπηρέτησης**. Για οδηγίες και περισσότερες πληροφορίες σχετικά με τον τρόπο για τον έλεγχο ταυτότητας, ανατρέξτε στο θέμα [Έλεγχος ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων χρησιμοποιώντας Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="how-to-install"></a>Πώς να εγκαταστήσετε το

```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>Ο έλεγχος ταυτότητας με χρήση Azure Active Directory

Τα παρακάτω τμήματα εμφανίζονται δύο ξεχωριστές τρόποι για τον έλεγχο ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων με χρήση Azure AD. Για λεπτομερή συζήτηση σχετικά με τις διάφορες μεθόδους για να χρησιμοποιήσετε για τον έλεγχο ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων, ανατρέξτε στο θέμα [Έλεγχος ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων χρησιμοποιώντας Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

Το παρακάτω τμήμα κώδικα απαιτεί επίσης εισόδων όπως το όνομα τομέα Azure AD, Αναγνωριστικό υπολογιστή-πελάτη για μια εφαρμογή Azure AD, κ.λπ. Όλες αυτές τις λεπτομέρειες μπορεί να ανακτηθεί από μια Azure AD εφαρμογή που πρέπει να δημιουργήσει, του οποίου οι λεπτομέρειες περιλαμβάνονται επίσης στην παραπάνω σύνδεση.

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>Δημιουργήστε τα προγράμματα-πελάτες του χώρου αποθήκευσης δεδομένων λίμνης

```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης δεδομένων λίμνης

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="create-a-file-with-content"></a>Δημιουργήστε ένα αρχείο με το περιεχόμενο
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a>Λάβετε μια λίστα των αρχείων και φακέλων

```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Δείτε επίσης

- [Microsoft Azure SDK για Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK για Node.js - Διαχείριση λίμνης ανάλυση δεδομένων](https://www.npmjs.com/package/azure-arm-datalake-analytics)
