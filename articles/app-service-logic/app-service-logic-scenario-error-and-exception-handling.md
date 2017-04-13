<properties
    pageTitle="Καταγραφή και χειρισμού σε εφαρμογές της λογικής σφαλμάτων | Microsoft Azure"
    description="Προβολή μιας υπόθεσης χρήσης πραγματικό για προχωρημένους σφάλματος χειρισμό και την καταγραφή με λογική εφαρμογών"
    keywords=""
    services="logic-apps"
    authors="hedidin"
    manager="anneta"
    editor=""
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="b-hoedid"/>

# <a name="logging-and-error-handling-in-logic-apps"></a>Καταγραφή και σε εφαρμογές της λογικής χειρισμού σφαλμάτων

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να επεκτείνετε μια εφαρμογή λογική για την καλύτερη υποστήριξη χειρισμού εξαιρέσεων. Πρόκειται για μια υπόθεση χρήσης πραγματικό και μας απάντηση στην ερώτηση του, "Εφαρμογές λογικής υποστηρίζει εξαίρεσης και χειρισμού σφαλμάτων;"

>[AZURE.NOTE]Η τρέχουσα έκδοση του τη δυνατότητα λογικής εφαρμογές του Microsoft Azure εφαρμογής υπηρεσίας παρέχει ένα τυπικό πρότυπο για απαντήσεις ενέργεια.
>Αυτό περιλαμβάνει εσωτερικές επικύρωσης και απαντήσεις σφάλματος που επιστρέφονται από μια εφαρμογή API.

## <a name="overview-of-the-use-case-and-scenario"></a>Επισκόπηση των περίπτωσης χρήσης και σεναρίου

Το παρακάτω κείμενο είναι το περίπτωσης χρήσης για αυτό το άρθρο.
Μια γνωστή υγείας εταιρεία που εκτελούν μας για να αναπτύξετε μια λύση Azure που θα δημιουργήσετε μια πύλη ασθενών χρησιμοποιώντας το Microsoft Dynamics CRM Online. Αυτά που απαιτούνται για να στείλετε συνάντηση τις εγγραφές μεταξύ του Dynamics CRM Online ασθενών πύλη και Salesforce.  Θα σας έχουν σας ζητηθεί να χρησιμοποιούν το πρότυπο [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) για όλες τις εγγραφές ασθενών.

Το έργο έχει δύο κύριες απαιτήσεις:  

 -  Μια μέθοδο για να συνδεθείτε εγγραφές που αποστέλλονται από την πύλη του Dynamics CRM Online
 -  Ένας τρόπος για να προβάλετε σφάλματα που έγιναν εντός της ροής εργασίας


## <a name="how-we-solved-the-problem"></a>Πώς θα σας επιλυθεί το πρόβλημα

>[AZURE.TIP] Μπορείτε να δείτε ένα βίντεο υψηλού επιπέδου του έργου με την [Ομάδα χρηστών ενοποίησης](http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "Ενοποίησης χρήστη ομάδας").

Επιλέγουμε [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/ "Azure DocumentDB") ως ένα αποθετήριο δεδομένων για τις εγγραφές καταγραφής και σφαλμάτων (DocumentDB αναφέρεται σε εγγραφές με τα έγγραφα). Επειδή οι εφαρμογές λογικής έχει ένα τυπικό πρότυπο για όλες τις απαντήσεις, θα δεν έχουμε για να δημιουργήσετε μια προσαρμοσμένη διάταξη. Θα μπορούσε να δημιουργήσουμε μια εφαρμογή API για να **εισαγάγετε** και **ερώτημα** για τις εγγραφές σφάλματος και συνδεθείτε. Θα σας μπορεί επίσης να ορίσετε ένα σχήμα για κάθε μέσα στην εφαρμογή API.  

Μια άλλη απαίτηση ήταν εκκαθάρισης εγγραφών μετά από μια συγκεκριμένη ημερομηνία. DocumentDB έχει μια ιδιότητα που ονομάζεται [διάρκεια ζωής](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "διάρκεια ζωής") (TTL), που επιτρέπεται να ορίσετε μια **διάρκεια ζωής** τιμή για κάθε εγγραφή ή τη συλλογή. Αυτό καταργηθεί την ανάγκη για τη μη αυτόματη διαγραφή εγγραφών στο DocumentDB.

### <a name="creation-of-the-logic-app"></a>Δημιουργία της εφαρμογής λογικής

Το πρώτο βήμα είναι να δημιουργήσετε την εφαρμογή λογικής και να φορτώσετε στη σχεδίαση. Σε αυτό το παράδειγμα, χρησιμοποιούμε γονικού-θυγατρικού λογικής εφαρμογών. Ας υποθέσουμε ότι θα σας έχετε ήδη δημιουργήσει το γονικό στοιχείο και πρόκειται να δημιουργήσετε ένα θυγατρικό λογική εφαρμογής.

Επειδή πρόκειται να η καταγραφή της εγγραφής που προέρχονται από το Dynamics CRM Online, ας ξεκινήσουμε στο επάνω μέρος. Πρέπει να χρησιμοποιήσετε ένα έναυσμα αίτηση, επειδή η εφαρμογή λογικής γονικό ενεργοποιεί αυτό το παιδί.

> [AZURE.IMPORTANT] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, θα πρέπει να δημιουργήσετε μια βάση δεδομένων DocumentDB και δύο συλλογές (καταγραφή και σφάλματα).

### <a name="logic-app-trigger"></a>Λογική εφαρμογής εναυσμάτων

Χρησιμοποιούμε ένα έναυσμα αίτηση όπως φαίνεται στο παρακάτω παράδειγμα.

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### <a name="steps"></a>Τα βήματα

Πρέπει να συνδεθείτε στην προέλευση (αίτηση) της ασθενών εγγραφής από την πύλη του Dynamics CRM Online.

1. Πρέπει να λάβετε μια νέα συνάντηση εγγραφή από το Dynamics CRM Online.
    Το έναυσμα που προέρχονται από CRM μας παρέχει το **CRM PatentId**, **τύπο εγγραφής**, **Δημιουργία ή ενημέρωση εγγραφής** (νέου ή να ενημερώσετε τιμή Boolean), και **SalesforceId**. Το **SalesforceId** μπορεί να είναι null, επειδή χρησιμοποιείται μόνο για μια ενημερωμένη έκδοση.
    Θα σας θα λάβετε την εγγραφή CRM, χρησιμοποιώντας το CRM **PatientID** και τον **Τύπο εγγραφής**.
1. Στη συνέχεια, θα χρειαστεί να προσθέσετε μας εφαρμογής DocumentDB API λειτουργία **InsertLogEntry** όπως φαίνεται στα παρακάτω σχήματα.


#### <a name="insert-log-entry-designer-view"></a>Εισαγωγή αρχείου καταγραφής καταχώρησης προβολή σχεδίασης

![Εισαγωγή αρχείου καταγραφής καταχώρησης](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### <a name="insert-error-entry-designer-view"></a>Εισαγωγή σφάλματος καταχώρησης προβολή σχεδίασης
![Εισαγωγή αρχείου καταγραφής καταχώρησης](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### <a name="check-for-create-record-failure"></a>Έλεγχος για δημιουργία αποτυχία εγγραφής

![Η συνθήκη](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## <a name="logic-app-source-code"></a>Λογική εφαρμογής πηγαίου κώδικα

>[AZURE.NOTE]  Ακολουθούν παραδείγματα μόνο. Επειδή αυτό το πρόγραμμα εκμάθησης βασίζεται σε μια εφαρμογή τη συγκεκριμένη στιγμή στο παραγωγής, την τιμή ενός **Κόμβου προέλευσης** μπορεί να μην εμφανίζονται οι ιδιότητες που σχετίζονται με τον προγραμματισμό μιας συνάντησης.

### <a name="logging"></a>Καταγραφή
Το παρακάτω δείγμα κώδικα εφαρμογής λογικής εμφανίζει τον τρόπο χειρισμού των καταγραφής.

#### <a name="log-entry"></a>Καταχώρηση του αρχείου καταγραφής
Αυτός είναι ο κωδικός λογική εφαρμογής προέλευσης για την εισαγωγή μια καταχώρηση αρχείου καταγραφής.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Αίτηση καταγραφής

Αυτό είναι το μήνυμα αίτησης καταγραφής δημοσιευτεί στην εφαρμογή API.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}"
        }
    }

```


#### <a name="log-response"></a>Απόκριση αρχείου καταγραφής

Αυτό είναι το μήνυμα απάντησης καταγραφής από την εφαρμογή API.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "\"0400fc2f-0000-0000-0000-575b3ff00000\"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Τώρα ας ρίξουμε μια ματιά το σφάλμα χειρισμός βήματα.


### <a name="error-handling"></a>Χειρισμού σφαλμάτων

Το παρακάτω δείγμα κώδικα λογικής εφαρμογές δείχνει πώς μπορείτε να υλοποιήσετε χειρισμού σφαλμάτων.

#### <a name="create-error-record"></a>Δημιουργία εγγραφής σφάλματος

Αυτός είναι ο κωδικός προέλευσης λογικής εφαρμογών για να δημιουργήσετε μια εγγραφή σφάλματος.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}          
```

#### <a name="insert-error-into-documentdb--request"></a>Σφάλμα εισαγωγής σε DocumentDB--αίτηση

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MasterID_mp__c\":\"\",\"C_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"ONY_ID__c\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-documentdb--response"></a>Εισαγωγή σφάλματος σε DocumentDB--απόκρισης


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "\"0c00eaac-0000-0000-0000-575b3fdc0000\"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\":\"DO - Degree level is DO\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MterID_mp__c\":\"\",\"Medicis_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"XXXXXXX\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Απόκριση σφάλμα Salesforce

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="returning-the-response-back-to-the-parent-logic-app"></a>Επιστρέφει την απάντηση στην εφαρμογή λογικής γονικό στοιχείο

Αφού έχετε την απάντηση, μπορείτε να το περάσετε ξανά στην εφαρμογή του γονικού λογικής.

#### <a name="return-success-response-to-the-parent-logic-app"></a>Επιστροφή επιτυχίας απάντηση στην εφαρμογή λογικής γονικό στοιχείο

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "   Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-the-parent-logic-app"></a>Επιστροφή απόκριση σφάλμα στην εφαρμογή λογικής γονικό στοιχείο

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="documentdb-repository-and-portal"></a>Αποθετήριο DocumentDB και πύλης

Οι λύσεις μας προστεθεί επιπλέον δυνατότητες με [DocumentDB](https://azure.microsoft.com/services/documentdb).

### <a name="error-management-portal"></a>Πύλη διαχείρισης του σφάλματος

Για να δείτε τα σφάλματα, μπορείτε να δημιουργήσετε μια εφαρμογή web της MVC για να εμφανίσετε τις εγγραφές σφάλματος από DocumentDB. **Λίστα**, **Λεπτομέρειες**, **Επεξεργασία**και **Διαγραφή** λειτουργίες περιλαμβάνονται στην τρέχουσα έκδοση.

> [AZURE.NOTE]Επεξεργαστείτε λειτουργία: DocumentDB κάνει μια αντικατάσταση ολόκληρου του εγγράφου.
> Οι εγγραφές που εμφανίζονται στη **λίστα** και τις προβολές **λεπτομερειών** είναι δείγματα μόνο. Δεν είναι πραγματική ασθενών συνάντηση εγγραφές.

Ακολουθούν παραδείγματα μας MVC λεπτομέρειες εφαρμογής που δημιουργήθηκε με την προηγούμενη περιγραφή προσέγγιση.

#### <a name="error-management-list"></a>Λίστα διαχείρισης σφαλμάτων

![Λίστα σφαλμάτων](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Προβολή λεπτομερειών σφάλματος διαχείρισης

![Λεπτομέρειες του σφάλματος](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Πύλη διαχείρισης του αρχείου καταγραφής

Για να προβάλετε τα αρχεία καταγραφής, έχουμε επίσης να δημιουργήσει μια εφαρμογή web της MVC.  Ακολουθούν παραδείγματα μας MVC λεπτομέρειες εφαρμογής που δημιουργήθηκε με την προηγούμενη περιγραφή προσέγγιση.

#### <a name="sample-log-detail-view"></a>Προβολή λεπτομερειών δείγματος αρχείου καταγραφής

![Προβολή λεπτομερειών αρχείου καταγραφής](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>Λεπτομέρειες εφαρμογής API

#### <a name="logic-apps-exception-management-api"></a>API διαχείρισης εξαίρεση εφαρμογές λογικής

Μας ανοιχτού κώδικα λογικής εφαρμογές εξαίρεσης διαχείρισης API εφαρμογής παρέχει τις ακόλουθες λειτουργίες.

Υπάρχουν δύο ελεγκτές:

- **ErrorController** εισάγει μια εγγραφή σφάλματος (έγγραφο) σε μια συλλογή DocumentDB.
- **LogController** Εισάγει μια εγγραφή αρχείου καταγραφής (έγγραφο) σε μια συλλογή DocumentDB.

> [AZURE.TIP] Χρησιμοποιήστε και τα δύο ελεγκτές `async Task<dynamic>` λειτουργίες. Αυτό σας επιτρέπει λειτουργιών, για να επιλυθούν κατά το χρόνο εκτέλεσης, ώστε να μπορούμε να δημιουργήσουμε το σχήμα DocumentDB στο σώμα της λειτουργίας.

Κάθε έγγραφο στο DocumentDB πρέπει να έχει ένα μοναδικό αναγνωριστικό. Χρησιμοποιούμε `PatientId` και να προσθέσετε μια χρονική σήμανση που έχει μετατραπεί σε μια τιμή χρονικής σήμανσης Unix (διπλό). Θα σας περικόψει για να καταργήσετε το κλασματικό τιμή.

Μπορείτε να προβάλετε τον πηγαίο κώδικα της μας ελεγκτή σφάλματος API [από GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).

Ονομάζουμε το API από μια εφαρμογή λογικής, χρησιμοποιώντας την ακόλουθη σύνταξη.

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Η παράσταση στο προηγούμενο δείγμα κώδικα έλεγχος της κατάστασης *Create_NewPatientRecord* **απέτυχε**.

## <a name="summary"></a>Σύνοψη

- Μπορείτε εύκολα να υλοποιήσετε καταγραφής και χειρισμού σε μια εφαρμογή λογικής σφαλμάτων.
- Μπορείτε να χρησιμοποιήσετε DocumentDB ως αποθετήριο δεδομένων για τις εγγραφές καταγραφής και σφαλμάτων (έγγραφα).
- Μπορείτε να χρησιμοποιήσετε MVC για να δημιουργήσετε μια πύλη για να εμφανίσετε τις εγγραφές καταγραφής και σφαλμάτων.

### <a name="source-code"></a>Πηγαίος κώδικας
Τον πηγαίο κώδικα για τη Διαχείριση εξαίρεση λογικής εφαρμογών API εφαρμογής είναι διαθέσιμη σε αυτό το [αποθετήριο GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "API διαχείρισης εξαίρεσης εφαρμογής λογικής").


## <a name="next-steps"></a>Επόμενα βήματα
- [Δείτε περισσότερα παραδείγματα λογικής εφαρμογών και σενάρια](app-service-logic-examples-and-scenarios.md)
- [Μάθετε σχετικά με τις εφαρμογές λογικής εργαλεία παρακολούθησης](app-service-logic-monitor-your-logic-apps.md)
- [Δημιουργία προτύπου εφαρμογής λογικής αυτοματοποιημένης ανάπτυξης](app-service-logic-create-deploy-template.md)
