<properties 
   pageTitle="Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με REST API | Microsoft Azure" 
   description="Χρήση WebHDFS REST API για να εκτελούν λειτουργίες σε ανάλυση λίμνης δεδομένων" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Γρήγορα αποτελέσματα με το Azure δεδομένων λίμνης ανάλυση με APIs ΥΠΌΛΟΙΠΟ

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Μάθετε πώς να χρησιμοποιείτε API ΥΠΌΛΟΙΠΟ WebHDFS και APIs ΥΠΌΛΟΙΠΑ ανάλυση λίμνης δεδομένων για να διαχειριστείτε τους λογαριασμούς, εργασίες και στον κατάλογο δεδομένων λίμνης ανάλυσης. 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

- **Δημιουργία μιας εφαρμογής υπηρεσίας καταλόγου Azure Active Directory**. Μπορείτε να χρησιμοποιήσετε την εφαρμογή Azure AD για τον έλεγχο ταυτότητας η εφαρμογή Analytics λίμνης δεδομένων με το Azure AD. Υπάρχουν διαφορετικές μεθόδους για τον έλεγχο ταυτότητας με Azure AD, που είναι **τελικού χρήστη ελέγχου ταυτότητας** ή **τον έλεγχο ταυτότητας υπηρεσίας εξυπηρέτησης**. Για οδηγίες και περισσότερες πληροφορίες σχετικά με τον τρόπο για τον έλεγχο ταυτότητας, ανατρέξτε στο θέμα [ο έλεγχος ταυτότητας με ανάλυση λίμνης δεδομένων με Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). Σε αυτό το άρθρο χρησιμοποιεί καμπύλη ώστε να δείχνουν τον τρόπο για την πραγματοποίηση κλήσεων REST API σε σχέση με ένα λογαριασμό ανάλυση λίμνης δεδομένων.

## <a name="authenticate-with-azure-active-directory"></a>Ο έλεγχος ταυτότητας με καταλόγου Azure Active Directory

Υπάρχουν δύο μέθοδοι για τον έλεγχο ταυτότητας με το Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Έλεγχος ταυτότητας του τελικού χρήστη (αλληλεπιδραστικά)

Χρησιμοποιώντας αυτήν τη μέθοδο, εφαρμογή ζητά από το χρήστη για να συνδεθείτε και όλες οι λειτουργίες που εκτελούνται στο περιβάλλον του χρήστη. 

Ακολουθήστε τα παρακάτω βήματα για τον έλεγχο ταυτότητας αλληλεπιδραστικών:

1. Από την εφαρμογή, ανακατευθύνετε το χρήστη στην ακόλουθη διεύθυνση URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT-URI > πρέπει να είναι κωδικοποιημένη για χρήση σε μια διεύθυνση URL. Επομένως, για https://localhost, χρησιμοποιήστε `https%3A%2F%2Flocalhost`)

    Για αυτό το πρόγραμμα εκμάθησης, μπορείτε να αντικαταστήστε τις τιμές κράτησης θέσης στη διεύθυνση URL παραπάνω και να επικολλήσετε στη γραμμή διευθύνσεων του προγράμματος περιήγησης. Θα ανακατευθυνθείτε για τον έλεγχο ταυτότητας με χρήση του Azure login. Μόλις συνδεθείτε μπορείτε με επιτυχία, η απόκριση εμφανίζεται στη γραμμή διευθύνσεων του προγράμματος περιήγησης. Η απόκριση θα είναι με την εξής μορφή:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Καταγράψτε τον κωδικό εξουσιοδότησης από την απάντηση. Για αυτό το πρόγραμμα εκμάθησης, μπορείτε να αντιγράψετε τον κώδικα εξουσιοδότησης από τη γραμμή διευθύνσεων του προγράμματος περιήγησης web και να μεταβιβάζουν το στην πρόσκληση σε ΚΑΤΑΧΏΡΗΣΗ για το τελικό σημείο διακριτικού, όπως φαίνεται παρακάτω:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] Σε αυτήν την περίπτωση, η \<REDIRECT-URI > δεν χρειάζεται να κωδικοποιηθεί.

3. Η απάντηση είναι ένα αντικείμενο JSON που περιέχει ένα διακριτικό πρόσβασης (π.χ., `"access_token": "<ACCESS_TOKEN>"`) και ενός διακριτικού ανανέωσης (π.χ., `"refresh_token": "<REFRESH_TOKEN>"`). Η εφαρμογή σας χρησιμοποιεί το διακριτικό πρόσβασης κατά την πρόσβαση στο χώρο αποθήκευσης λίμνης δεδομένων Azure και το διακριτικό Ανανέωση για να λάβετε έναν νέο κωδικό πρόσβασης όταν λήγει ένα διακριτικό πρόσβασης.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Όταν λήξει το διακριτικό πρόσβασης, μπορείτε να ζητήσετε έναν νέο κωδικό πρόσβασης, χρησιμοποιώντας το διακριτικό ανανέωσης, όπως φαίνεται παρακάτω:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Για περισσότερες πληροφορίες σχετικά με τον έλεγχο ταυτότητας χρήστη διαδραστικό, ανατρέξτε στο θέμα [Εκχώρηση ροής κωδικό εξουσιοδότησης](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Υπηρεσία εξυπηρέτησης ελέγχου ταυτότητας (μη αλληλεπιδραστική)

Χρησιμοποιώντας αυτήν τη μέθοδο, η εφαρμογή παρέχει το δικό της διαπιστευτήρια για να εκτελέσετε τις λειτουργίες. Για αυτό, πρέπει να εκδώσετε μια αίτηση POST όπως αυτό που φαίνεται παρακάτω: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Το αποτέλεσμα αυτής της αίτησης θα περιλαμβάνει ένα διακριτικό εξουσιοδότησης (δηλώνεται από `access-token` στο αποτέλεσμα του παρακάτω) που θα ανοίξει περάσετε με τις κλήσεις σας REST API. Αποθήκευση αυτό το διακριτικό ελέγχου ταυτότητας σε ένα αρχείο κειμένου. θα χρειαστείτε αργότερα σε αυτό το άρθρο.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Αυτό το άρθρο χρησιμοποιεί τη **μη αλληλεπιδραστική** προσέγγιση. Για περισσότερες πληροφορίες σχετικά με την μη αλληλεπιδραστική (υπηρεσία εξυπηρέτησης κλήσεις), ανατρέξτε στο θέμα [της υπηρεσίας εξυπηρέτησης κλήσεις χρησιμοποιώντας τα διαπιστευτήρια](https://msdn.microsoft.com/library/azure/dn645543.aspx).
## <a name="create-a-data-lake-analytics-account"></a>Δημιουργία λογαριασμού ανάλυσης δεδομένων λίμνης

Πρέπει να δημιουργήσετε μια ομάδα πόρων Azure, καθώς και ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης πριν μπορέσετε να δημιουργήσετε ένα λογαριασμό ανάλυση λίμνης δεδομένων.  Ανατρέξτε στο θέμα [Δημιουργία λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Η παρακάτω εντολή καμπύλη δείχνει πώς μπορείτε να δημιουργήσετε ένα λογαριασμό:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Αντικατάσταση \< `REDACTED` \> με το διακριτικό εξουσιοδότησης, \< `AzureSubscriptionID` \> με το Αναγνωριστικό συνδρομής, \< `AzureResourceGroupName` \> με το όνομα μιας υπάρχουσας ομάδας πόρων Azure, και \< `NewAzureDataLakeAnalyticsAccountName` \> με ένα νέο όνομα λογαριασμού ανάλυση λίμνης δεδομένων. Το φορτίο αίτηση για αυτή την εντολή περιέχονται στο αρχείο **CreateDatalakeAnalyticsAccountRequest.json** που παρέχεται για το `-d` παραμέτρου παραπάνω. Τα περιεχόμενα του αρχείου input.json μοιάζουν με τα εξής:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Λογαριασμοί ανάλυση λίμνης δεδομένων λίστας σε μια συνδρομή

Η παρακάτω εντολή καμπύλη δείχνει πώς μπορείτε να λίστα λογαριασμών στο συνδρομής:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Αντικατάσταση \< `REDACTED` \> με το διακριτικό εξουσιοδότησης, \< `AzureSubscriptionID` \> με το αναγνωριστικό συνδρομής. Το αποτέλεσμα είναι παρόμοια με:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Λήψη πληροφοριών σχετικά με ένα λογαριασμό ανάλυσης δεδομένων λίμνης

Την ακόλουθη εντολή καμπύλη δείχνει πώς μπορείτε να λάβετε μια πληροφορίες λογαριασμού:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Αντικατάσταση \< `REDACTED` \> με το διακριτικό εξουσιοδότησης, \< `AzureSubscriptionID` \> με το Αναγνωριστικό συνδρομής, \< `AzureResourceGroupName` \> με το όνομα μιας υπάρχουσας ομάδας πόρων Azure, και \< `DataLakeAnalyticsAccountName` \> με το όνομα του υπάρχοντος λογαριασμού λίμνης ανάλυση δεδομένων. Το αποτέλεσμα είναι παρόμοια με:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Λίστα δεδομένων λίμνης αποθηκεύει ενός λογαριασμού ανάλυσης δεδομένων λίμνης

Η παρακάτω εντολή καμπύλη δείχνει πώς μπορείτε να λίστα αποθηκεύει λίμνης δεδομένων από ένα λογαριασμό:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Αντικατάσταση \< `REDACTED` \> με το διακριτικό εξουσιοδότησης, \< `AzureSubscriptionID` \> με το Αναγνωριστικό συνδρομής, \< `AzureResourceGroupName` \> με το όνομα μιας υπάρχουσας ομάδας πόρων Azure, και \< `DataLakeAnalyticsAccountName` \> με το όνομα του υπάρχοντος λογαριασμού λίμνης ανάλυση δεδομένων. Το αποτέλεσμα είναι παρόμοια με:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>Υποβολή εργασιών U-SQL

Η παρακάτω εντολή καμπύλη δείχνει πώς μπορείτε να υποβάλετε μια εργασία U-SQL:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Αντικατάσταση \< `REDACTED` \> με το διακριτικό εξουσιοδότησης, \< `DataLakeAnalyticsAccountName` \> με το όνομα του υπάρχοντος λογαριασμού λίμνης ανάλυση δεδομένων. Το φορτίο αίτηση για αυτή την εντολή περιέχονται στο αρχείο **SubmitADLAJob.json** που παρέχεται για το `-d` παραμέτρου παραπάνω. Τα περιεχόμενα του αρχείου input.json μοιάζουν με τα εξής:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

Το αποτέλεσμα είναι παρόμοια με:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>Λίστα εργασιών U-SQL

Η παρακάτω εντολή καμπύλη δείχνει πώς μπορείτε να λίστα εργασιών U-SQL:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Αντικατάσταση \< `REDACTED` \> με το διακριτικό εξουσιοδότησης, και \< `DataLakeAnalyticsAccountName` \> με το όνομα του υπάρχοντος λογαριασμού λίμνης ανάλυση δεδομένων. 


Το αποτέλεσμα είναι παρόμοια με:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Λήψη στοιχείων καταλόγου

Την ακόλουθη εντολή καμπύλη δείχνει πώς μπορείτε να λάβετε τις βάσεις δεδομένων από τον κατάλογο:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

Το αποτέλεσμα είναι παρόμοια με:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Δείτε επίσης

- Για να δείτε ένα πιο σύνθετο ερώτημα, ανατρέξτε στο θέμα [ανάλυση τοποθεσίας Web αρχεία καταγραφής χρησιμοποιώντας Azure δεδομένων λίμνης ανάλυσης](data-lake-analytics-analyze-weblogs.md).
- Για να ξεκινήσετε την ανάπτυξη εφαρμογών U-SQL, ανατρέξτε στο θέμα [Ανάπτυξη U-SQL δέσμης ενεργειών με χρήση εργαλεία λίμνης δεδομένων για το Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Για να μάθετε U-SQL, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το δεδομένων λίμνης ανάλυση U-SQL Azure γλώσσας](data-lake-analytics-u-sql-get-started.md).
- Για τις εργασίες διαχείρισης, ανατρέξτε στο θέμα [Διαχείριση Azure δεδομένων λίμνης αναλυτικών στοιχείων χρήσης Azure πύλη](data-lake-analytics-manage-use-portal.md).
- Για να δείτε μια επισκόπηση της ανάλυσης λίμνης δεδομένων, ανατρέξτε στο θέμα [Επισκόπηση Azure δεδομένων λίμνης ανάλυσης](data-lake-analytics-overview.md).
- Για να δείτε το ίδιο πρόγραμμα εκμάθησης με χρήση άλλων εργαλείων, κάντε κλικ στην επιλογή των δεικτών επιλογής καρτέλα στο επάνω μέρος της σελίδας.
- Την καταγραφή διαγνωστικών πληροφοριών, ανατρέξτε στο θέμα [πρόσβαση σε αρχεία καταγραφής Διαγνωστικά για ανάλυση λίμνης δεδομένων Azure](data-lake-analytics-diagnostic-logs.md)
