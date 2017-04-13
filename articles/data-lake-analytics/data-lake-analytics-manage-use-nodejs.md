<properties
   pageTitle="Διαχείριση Azure δεδομένων λίμνης αναλυτικών στοιχείων χρήσης Azure SDK για Node.js | Azure"
   description="Μάθετε πώς να διαχειρίζεστε ανάλυσης δεδομένων λίμνης λογαριασμούς, προελεύσεις δεδομένων, εργασίες και οι χρήστες που χρησιμοποιούν Azure SDK για Node.js"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Διαχείριση Azure δεδομένων λίμνης αναλυτικών στοιχείων χρήσης Azure SDK για Node.js


[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Το SDK Azure για Node.js μπορεί να χρησιμοποιηθεί για τη Διαχείριση ανάλυσης λίμνης δεδομένων Azure λογαριασμούς, εργασίες και κατάλογοι. Για να δείτε το θέμα διαχείρισης με χρήση άλλων εργαλείων, κάντε κλικ στην επιλογή καρτέλα, επιλέξτε το παραπάνω.

Δεξιά τώρα υποστηρίζει:

  *  **Έκδοση node.js: 0.10.0 ή νεότερη έκδοση**
  *  **Έκδοση REST API για το λογαριασμό: 2015-10-01-preview**
  *  **Έκδοση REST API για τον κατάλογο: 2015-10-01-preview**
  *  **Έκδοση REST API για το έργο: 2016-03-20-preview**

## <a name="features"></a>Δυνατότητες

- Διαχείρισης λογαριασμού: δημιουργία, λήψη, λίστα, ενημέρωση και διαγραφή.
- Διαχείριση έργων: υποβολή, λήψη, λίστα, ακύρωση.
- Διαχείριση καταλόγου: λήψη, λίστα, Δημιουργία (απόρρητο), (απόρρητο) ενημέρωση, Διαγραφή (απόρρητο).

## <a name="how-to-install"></a>Πώς να εγκαταστήσετε το

```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Ο έλεγχος ταυτότητας με χρήση Azure Active Directory

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>Δημιουργία του προγράμματος-πελάτη ανάλυσης δεδομένων λίμνης

```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Δημιουργία λογαριασμού ανάλυσης δεδομένων λίμνης

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
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

## <a name="get-a-list-of-jobs"></a>Λάβετε μια λίστα των εργασιών

```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Λάβετε μια λίστα με βάσεις δεδομένων στον κατάλογο δεδομένων λίμνης ανάλυσης
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Δείτε επίσης

- [Microsoft Azure SDK για Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK για Node.js - Διαχείριση χώρου αποθήκευσης δεδομένων λίμνης](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)
