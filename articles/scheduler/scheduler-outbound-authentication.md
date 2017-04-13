<properties
 pageTitle="Έλεγχος ταυτότητας εξερχομένων χρονοδιαγράμματος"
 description="Έλεγχος ταυτότητας εξερχομένων χρονοδιαγράμματος"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/15/2016"
 ms.author="deli"/>

# <a name="scheduler-outbound-authentication"></a>Έλεγχος ταυτότητας εξερχομένων χρονοδιαγράμματος

Χρονοδιάγραμμα εργασιών ενδέχεται να χρειαστεί να καλέσετε με τις υπηρεσίες που απαιτούν έλεγχο ταυτότητας. Με αυτόν τον τρόπο, μια υπηρεσία που ονομάζεται να προσδιορίσετε εάν η εργασία Scheduler να αποκτήσετε πρόσβαση τους πόρους. Ορισμένες από αυτές τις υπηρεσίες περιλαμβάνει άλλες υπηρεσίες του Azure, Salesforce.com, Facebook και ασφαλείς προσαρμοσμένες τοποθεσίες Web.

## <a name="adding-and-removing-authentication"></a>Προσθήκη και κατάργηση ελέγχου ταυτότητας

Προσθήκη ελέγχου ταυτότητας για ένα χρονοδιάγραμμα έργου είναι απλή-Προσθήκη JSON θυγατρικό στοιχείο `authentication` για να το `request` στοιχείο κατά τη δημιουργία ή ενημέρωση μιας εργασίας. Απορρήτου που εισήχθησαν στην υπηρεσία του χρονοδιαγράμματος σε μια ΘΈΣΗ, την ενημέρωση ΚΏΔΙΚΑ ή ΔΗΜΟΣΊΕΥΣΗ αίτηση – ως μέρος του `authentication` αντικείμενο – επιστρέφονται ποτέ σε απαντήσεις. Στις απαντήσεις, μυστικού πληροφορίες έχει οριστεί σε τιμή null ή μπορεί να έχετε μια δημόσια διακριτικού που αντιπροσωπεύει την οντότητα με έλεγχο ταυτότητας.

Για να καταργήσετε τον έλεγχο ταυτότητας, ΤΟΠΟΘΕΤΉΣΤΕ ή ΚΏΔΙΚΑ της εργασίας ρητά, τη ρύθμιση του `authentication` αντικειμένου σε null. Δεν θα βλέπετε οποιεσδήποτε ιδιότητες ελέγχου ταυτότητας πίσω στην απάντηση.

Προς το παρόν, είναι η μόνη υποστηριζόμενους τύπους ελέγχου ταυτότητας του `ClientCertificate` μοντέλο (για χρησιμοποιώντας τα πιστοποιητικά SSL/TLS προγράμματος-πελάτη), το `Basic` μοντέλο (για το βασικό έλεγχο ταυτότητας), και το `ActiveDirectoryOAuth` μοντέλο (για το διακριτικό Active Directory έλεγχο ταυτότητας.)

## <a name="request-body-for-clientcertificate-authentication"></a>Σώμα αίτησης για έλεγχο ταυτότητας ClientCertificate

Κατά την προσθήκη χρησιμοποιώντας τον έλεγχο ταυτότητας του `ClientCertificate` του μοντέλου, καθορίστε τα ακόλουθα πρόσθετα στοιχεία στο σώμα της αίτησης.  

|Στοιχείο|Περιγραφή|
|:---|:---|
|_έλεγχος ταυτότητας (γονικό στοιχείο)_|Αντικείμενο ελέγχου ταυτότητας για τη χρήση του πιστοποιητικού SSL προγράμματος-πελάτη.|
|_Τύπος_|Απαιτείται. Τύπος ελέγχου ταυτότητας. Για τα πιστοποιητικά SSL προγράμματος-πελάτη, η τιμή πρέπει να `ClientCertificate`.|
|_PFX_|Απαιτείται. Κωδικοποίηση Base64 περιεχόμενα του αρχείου PFX.|
|_κωδικός πρόσβασης_|Απαιτείται. Ο κωδικός πρόσβασης για να αποκτήσετε πρόσβαση στο αρχείο PFX.|


## <a name="response-body-for-clientcertificate-authentication"></a>Σώμα απόκρισης για έλεγχο ταυτότητας ClientCertificate

Όταν μια αίτηση αποστέλλεται με πληροφορίες ελέγχου ταυτότητας, η απόκριση περιέχει τα ακόλουθα στοιχεία σχετικά με τον έλεγχο ταυτότητας.

|Στοιχείο |Περιγραφή |
|:--|:--|
|_έλεγχος ταυτότητας (γονικό στοιχείο)_ |Αντικείμενο ελέγχου ταυτότητας για τη χρήση του πιστοποιητικού SSL προγράμματος-πελάτη.|
|_Τύπος_ |Τύπος ελέγχου ταυτότητας. Για τα πιστοποιητικά SSL προγράμματος-πελάτη, η τιμή είναι `ClientCertificate`.|
|_certificateThumbprint_ |Την αποτύπωση του πιστοποιητικού.|
|_certificateSubjectName_ |Το αποκλειστικό όνομα θέματος του πιστοποιητικού.|
|_certificateExpiration_ |Η ημερομηνία λήξης του πιστοποιητικού.|

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>Δείγμα ΥΠΌΛΟΙΠΑ αίτηση για έλεγχο ταυτότητας ClientCertificate

```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>Δείγμα ΥΠΌΛΟΙΠΑ απόκρισης για έλεγχο ταυτότητας ClientCertificate

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>Σώμα αίτησης για βασικό έλεγχο ταυτότητας

Κατά την προσθήκη χρησιμοποιώντας τον έλεγχο ταυτότητας του `Basic` του μοντέλου, καθορίστε τα ακόλουθα πρόσθετα στοιχεία στο σώμα της αίτησης.

|Στοιχείο|Περιγραφή|
|:--|:--|
|_έλεγχος ταυτότητας (γονικό στοιχείο)_ |Αντικείμενο ελέγχου ταυτότητας για τη χρήση βασικό έλεγχο ταυτότητας.|
|_Τύπος_ |Απαιτείται. Τύπος ελέγχου ταυτότητας. Για το βασικό έλεγχο ταυτότητας, η τιμή πρέπει να `Basic`.|
|_όνομα χρήστη_ |Απαιτείται. Το όνομα χρήστη για τον έλεγχο ταυτότητας.|
|_κωδικός πρόσβασης_ |Απαιτείται. Ο κωδικός πρόσβασης για τον έλεγχο ταυτότητας.|

## <a name="response-body-for-basic-authentication"></a>Σώμα απόκρισης για βασικό έλεγχο ταυτότητας

Όταν μια αίτηση αποστέλλεται με πληροφορίες ελέγχου ταυτότητας, η απόκριση περιέχει τα ακόλουθα στοιχεία σχετικά με τον έλεγχο ταυτότητας.

|Στοιχείο|Περιγραφή|
|:--|:--|
|_έλεγχος ταυτότητας (γονικό στοιχείο)_ |Αντικείμενο ελέγχου ταυτότητας για τη χρήση βασικό έλεγχο ταυτότητας.|
|_Τύπος_ |Τύπος ελέγχου ταυτότητας. Για βασικό έλεγχο ταυτότητας, η τιμή είναι `Basic`.|
|_όνομα χρήστη_ |Το όνομα χρήστη με έλεγχο ταυτότητας.|

## <a name="sample-rest-request-for-basic-authentication"></a>Δείγμα ΥΠΌΛΟΙΠΑ αίτησης για βασικό έλεγχο ταυτότητας

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>Δείγμα ΥΠΌΛΟΙΠΑ απόκρισης για βασικό έλεγχο ταυτότητας

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>Σώμα αίτησης για έλεγχο ταυτότητας ActiveDirectoryOAuth

Κατά την προσθήκη χρησιμοποιώντας τον έλεγχο ταυτότητας του `ActiveDirectoryOAuth` του μοντέλου, καθορίστε τα ακόλουθα πρόσθετα στοιχεία στο σώμα της αίτησης.

|Στοιχείο |Περιγραφή |
|:--|:--|
|_έλεγχος ταυτότητας (γονικό στοιχείο)_ |Αντικείμενο ελέγχου ταυτότητας για τη χρήση του ελέγχου ταυτότητας ActiveDirectoryOAuth.|
|_Τύπος_ |Απαιτείται. Τύπος ελέγχου ταυτότητας. Για τον έλεγχο ταυτότητας ActiveDirectoryOAuth, η τιμή πρέπει να `ActiveDirectoryOAuth`.|
|_μισθωτή_ |Απαιτείται. Το αναγνωριστικό μισθωτή για το μισθωτή Azure AD.|
|_ακροατηρίου_ |Απαιτείται. Είναι ενεργοποιημένη η επιλογή https://management.core.windows.net/.|
|_clientId_ |Απαιτείται. Δώστε το αναγνωριστικό υπολογιστή-πελάτη για την εφαρμογή Azure AD.|
|_μυστικό_ |Απαιτείται. Μυστικό του προγράμματος-πελάτη που ζητά το διακριτικό.|

### <a name="determining-your-tenant-identifier"></a>Καθορισμός του αναγνωριστικού μισθωτή

Μπορείτε να βρείτε το αναγνωριστικό μισθωτή για το μισθωτή Azure AD εκτελώντας το `Get-AzureAccount` στο Azure PowerShell.

## <a name="response-body-for-activedirectoryoauth-authentication"></a>Σώμα απόκρισης για έλεγχο ταυτότητας ActiveDirectoryOAuth

Όταν μια αίτηση αποστέλλεται με πληροφορίες ελέγχου ταυτότητας, η απόκριση περιέχει τα ακόλουθα στοιχεία σχετικά με τον έλεγχο ταυτότητας.

|Στοιχείο |Περιγραφή |
|:--|:--|
|_έλεγχος ταυτότητας (γονικό στοιχείο)_ |Αντικείμενο ελέγχου ταυτότητας για τη χρήση του ελέγχου ταυτότητας ActiveDirectoryOAuth.|
|_Τύπος_ |Τύπος ελέγχου ταυτότητας. Για τον έλεγχο ταυτότητας ActiveDirectoryOAuth, η τιμή είναι `ActiveDirectoryOAuth`.|
|_μισθωτή_ |Το αναγνωριστικό μισθωτή για το μισθωτή Azure AD. |
|_ακροατηρίου_ |Είναι ενεργοποιημένη η επιλογή https://management.core.windows.net/.|
|_clientId_ |Το αναγνωριστικό υπολογιστή-πελάτη για την εφαρμογή Azure AD.|

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>Δείγμα ΥΠΌΛΟΙΠΑ αίτηση για έλεγχο ταυτότητας ActiveDirectoryOAuth

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>Δείγμα ΥΠΌΛΟΙΠΑ απόκρισης για έλεγχο ταυτότητας ActiveDirectoryOAuth

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>Δείτε επίσης


 [Τι είναι το χρονοδιάγραμμα;](scheduler-intro.md)

 [Azure έννοιες χρονοδιάγραμμα, ορολογία και οντότητα ιεραρχία](scheduler-concepts-terms.md)

 [Γρήγορα αποτελέσματα με το χρονοδιάγραμμα στην πύλη του Azure](scheduler-get-started-portal.md)

 [Προγράμματα και Τιμολόγηση στο χρονοδιάγραμμα Azure](scheduler-plans-billing.md)

 [Αναφορά Azure Scheduler REST API](https://msdn.microsoft.com/library/mt629143)

 [Αναφορά Azure cmdlet του PowerShell χρονοδιαγράμματος](scheduler-powershell-reference.md)

 [Azure Scheduler υψηλή διαθεσιμότητα και την αξιοπιστία](scheduler-high-availability-reliability.md)

 [Azure όρια χρονοδιάγραμμα, προεπιλεγμένες τιμές και τους κωδικούς σφάλματος](scheduler-limits-defaults-errors.md)
