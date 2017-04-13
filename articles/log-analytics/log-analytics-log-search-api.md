<properties
    pageTitle="Ανάλυση καταγραφής συνδεθείτε search REST API | Microsoft Azure"
    description="Αυτός ο οδηγός παρέχει ένα βασικό πρόγραμμα εκμάθησης που περιγράφει πώς μπορείτε να χρησιμοποιήσετε την αναζήτηση αρχείου καταγραφής ανάλυσης REST API στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS) και παρέχει παραδείγματα που σας δείχνουν πώς μπορείτε να χρησιμοποιήσετε τις εντολές."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="log-analytics-log-search-rest-api"></a>Αρχείο καταγραφής αναλυτικών στοιχείων καταγραφής search REST API

Αυτός ο οδηγός παρέχει ένα βασικό πρόγραμμα εκμάθησης που περιγράφει πώς μπορείτε να χρησιμοποιήσετε το αρχείο καταγραφής αναλυτικών στοιχείων Search REST API στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS) και παρέχει παραδείγματα που σας δείχνουν πώς μπορείτε να χρησιμοποιήσετε τις εντολές. Ορισμένα από τα παραδείγματα σε αυτό το άρθρο αναφοράς λειτουργικές ιδέες, ποιο είναι το όνομα του την προηγούμενη έκδοση του αρχείου καταγραφής ανάλυσης.

## <a name="overview-of-the-log-search-rest-api"></a>Επισκόπηση της αναζήτησης καταγραφής REST API

Το αρχείο καταγραφής αναλυτικών στοιχείων Search REST API είναι RESTful και είναι δυνατή η πρόσβαση μέσω του Azure API διαχείρισης πόρων. Σε αυτό το έγγραφο θα βρείτε παραδείγματα όπου το API γίνεται μέσω του [ARMClient](https://github.com/projectkudu/ARMClient), ένα εργαλείο γραμμής εντολών Άνοιγμα αρχείου προέλευσης που απλοποιεί την κλήση του Azure API διαχείρισης πόρων. Η χρήση των ARMClient και του PowerShell είναι μία από πολλές επιλογές για να αποκτήσετε πρόσβαση το API καταγραφής αναλυτικών στοιχείων αναζήτησης. Μια άλλη επιλογή είναι να χρησιμοποιήσετε τη λειτουργική μονάδα Azure PowerShell για OperationalInsights που περιλαμβάνει cmdlet του για να αποκτήσετε πρόσβαση αναζήτησης. Με αυτά τα εργαλεία που μπορούν να χρησιμοποιήσουν το RESTful API διαχείρισης πόρων Azure για να κάνετε κλήσεις σε χώρους εργασίας OMS και εκτέλεση εντολών αναζήτησης μέσα σε αυτά. Το API θα εξόδου αποτελεσμάτων αναζήτησης σε εσάς σε μορφή JSON, επιτρέποντάς σας να χρησιμοποιήσετε τα αποτελέσματα αναζήτησης με πολλούς διαφορετικούς τρόπους μέσω προγραμματισμού.

Μπορεί να χρησιμοποιηθεί η διαχείριση πόρων Azure μέσω μιας [βιβλιοθήκης για .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) , καθώς και μια [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx). Εξετάστε τις συνδεδεμένες σελίδες web για να μάθετε περισσότερα.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Πρόγραμμα εκμάθησης βασικές καταγραφής ανάλυσης Search REST API

### <a name="to-use-the-arm-client"></a>Για να χρησιμοποιήσετε το πρόγραμμα-πελάτη ARM

1. Εγκαταστήστε [Chocolatey](https://chocolatey.org/), το οποίο είναι ένα άνοιγμα της διαχείρισης πακέτου προέλευσης για Windows. Ανοίξτε ένα παράθυρο γραμμής εντολών ως διαχειριστής και, στη συνέχεια, εκτελέστε την ακόλουθη εντολή:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

2. Εγκαταστήστε το ARMClient, εκτελώντας την ακόλουθη εντολή:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Για να εκτελέσετε μια απλή αναζήτηση χρησιμοποιώντας το ARMClient

1. Συνδεθείτε στο λογαριασμό σας Microsoft ή OrgID:

    ```
    armclient login
    ```

    Μια επιτυχημένη σύνδεση παραθέτει όλες τις συνδρομές συνδεδεμένη με το λογαριασμό που δίνεται:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. Λάβετε τους χώρους εργασίας οικογένεια προγραμμάτων διαχείρισης λειτουργίες:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Επιτυχής λήψη κλήσης θα εξόδου όλους τους χώρους εργασίας που συνδέονται με τη συνδρομή:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Δημιουργήστε τη μεταβλητή αναζήτησης:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Αναζήτηση χρησιμοποιώντας τη νέα μεταβλητή αναζήτησης:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Συνδεθείτε παραδείγματα αναφορά ανάλυσης Search REST API
Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να χρησιμοποιήσετε το API αναζήτησης.

### <a name="search---actionread"></a>Αναζήτηση - ενέργεια/μη αναγνωσμένα

**Δείγμα Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Αίτηση:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
Ο παρακάτω πίνακας περιγράφει τις ιδιότητες που είναι διαθέσιμες.

|**Ιδιότητα**|**Περιγραφή**|
|---|---|
|αρχή|Ο μέγιστος αριθμός των αποτελεσμάτων για να επιστρέψει.|
|επισήμανση|Περιέχει παραμέτρους προ και μετά, χρησιμοποιείται συνήθως για την επισήμανση αντίστοιχα πεδία|
|πριν από|Προθέματα τη δεδομένη συμβολοσειρά για να σας αντιστοιχισμένα πεδία.|
|δημοσίευση|Τοποθετεί τη δεδομένη συμβολοσειρά σας αντιστοιχισμένα πεδία.|
|ερώτημα|Το ερώτημα αναζήτησης που χρησιμοποιείται για να συλλέξετε και να επιστρέφει αποτελέσματα.|
|Έναρξη|Αρχή του χρονικού διαστήματος που θέλετε να να βρεθεί από τα αποτελέσματα.|
|Τέλος|Τέλος του χρονικού διαστήματος που θέλετε να να βρεθεί από τα αποτελέσματα.|


**Απόκριση:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Αναζήτηση / {ΑΝΑΓΝΩΡΙΣΤΙΚΌ} - ενέργεια/μη αναγνωσμένα

**Αίτηση για τα περιεχόμενα της αποθηκευμένης αναζήτησης:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

>[AZURE.NOTE] Εάν η αναζήτηση επιστρέφει με 'Σε εκκρεμότητα' κατάσταση, σταθμοσκόπησης, στη συνέχεια, τα ενημερωμένα αποτελέσματα μπορεί να γίνει μέσω αυτό το API. Μετά από 6 min, το αποτέλεσμα της αναζήτησης θα καταργηθεί από το cache και υπερβεί HTTP θα επιστραφεί. Εάν η αίτηση αρχικό search επέστρεψε 'Επιτυχής' κατάσταση αμέσως, θα προστεθεί δεν στο cache προκαλούν αυτό το API για να επιστρέψετε υπερβεί HTTP εάν το ερώτημα. Τα περιεχόμενα του ένα αποτέλεσμα HTTP 200 θα είναι στην ίδια μορφή με την αίτηση αρχικό αναζήτησης μόνο με τις ενημερωμένες τιμές.

### <a name="saved-searches---rest-only"></a>Αποθηκευμένες αναζητήσεις - ΥΠΌΛΟΙΠΟ μόνο

**Αίτηση λίστα αποθηκευμένες αναζητήσεις:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Υποστηριζόμενες μεθόδους: ΤΟΠΟΘΈΤΗΣΗ ΔΙΑΓΡΑΦΉ ΛΉΨΗ

Υποστηρίζονται συλλογής μεθόδους: ΛΉΨΗ

Ο παρακάτω πίνακας περιγράφει τις ιδιότητες που είναι διαθέσιμες.

|Ιδιότητα|Περιγραφή|
|---|---|
|Αναγνωριστικό|Το μοναδικό αναγνωριστικό.|
|ETag|**Απαιτείται για την ενημέρωση κώδικα**. Ενημέρωση από το διακομιστή σε κάθε εγγραφή. Η τιμή πρέπει να ισούται με την τρέχουσα τιμή αποθηκευμένη ή ' *' για να ενημερώσετε. επιστρέφεται 409 για τιμές παλιούς/δεν είναι έγκυρη.|
|Properties.Query|**Απαιτείται**. Το ερώτημα αναζήτησης.|
|properties.displayName|**Απαιτείται**. Το όνομα χρήστη καθορισμένο εμφάνισης του ερωτήματος. Εάν διαμορφωθεί ως ένας πόρος Azure, αυτό θα ήταν μια ετικέτα.|
|Properties.Category|**Απαιτείται**. Κατηγορία του ερωτήματος που ορίζονται από το χρήστη. Εάν διαμορφωθεί ως ένας πόρος Azure αυτό θα ήταν μια ετικέτα.|

>[AZURE.NOTE] Το API αναζήτησης καταγραφής ανάλυσης αυτήν τη στιγμή επιστρέφει δημιουργούνται από το χρήστη αποθηκευμένες αναζητήσεις όταν απομακρυσμένα για αποθηκευμένες αναζητήσεις σε ένα χώρο εργασίας. Το API δεν θα επιστρέψει αποθηκευμένες αναζητήσεις που παρέχεται από λύσεις αυτήν τη στιγμή. Αυτή η λειτουργία θα προστεθεί σε μεταγενέστερη ημερομηνία.

### <a name="create-saved-searches"></a>Δημιουργία αποθηκευμένες αναζητήσεις

**Αίτηση:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Διαγραφή αποθηκευτεί αναζητήσεις

**Αίτηση:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Ενημέρωση αποθηκευτεί αναζητήσεις

 **Αίτηση:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Μετα-δεδομένων - JSON μόνο

Ακολουθεί ένας τρόπος για να δείτε τα πεδία για όλους τους τύπους καταγραφής για τα δεδομένα που συλλέγονται στο χώρο εργασίας σας. Για παράδειγμα, εάν θέλετε να γνωρίζετε εάν ο τύπος συμβάντος περιλαμβάνει ένα πεδίο με όνομα υπολογιστή, στη συνέχεια, αυτό είναι ένας τρόπος για να αναζητήσετε και να επιβεβαιώσετε.

**Αίτηση για τα πεδία:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Απόκριση:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

Ο παρακάτω πίνακας περιγράφει τις ιδιότητες που είναι διαθέσιμες.

|**Ιδιότητα**|**Περιγραφή**|
|---|---|
|Όνομα|Όνομα πεδίου.|
|Εμφανιζόμενο όνομα|Το εμφανιζόμενο όνομα του πεδίου.|
|Τύπος|Ο τύπος της τιμής του πεδίου.|
|facetable|Συνδυασμός των τρεχουσών 'ευρετήριο', ' αποθηκευμένο ' και 'όψη' Ιδιότητες.|
|Εμφάνιση|Τρέχουσα ιδιότητα 'Εμφάνιση'. TRUE, εάν το πεδίο είναι ορατό στην αναζήτηση.|
|ownerType|Μειωθεί σε μόνο οι τύποι που ανήκουν σε onboarded διευθύνσεων IP.|


## <a name="optional-parameters"></a>Προαιρετικές παράμετροι
Οι παρακάτω πληροφορίες περιγράφονται προαιρετικό παραμέτρους που είναι διαθέσιμες.

### <a name="highlighting"></a>Επισήμανση

Η παράμετρος "Επισήμανση" είναι μια προαιρετική παράμετρος που μπορείτε να χρησιμοποιήσετε για να ζητήσετε υποσυστήματος αναζήτησης περιλαμβάνει ένα σύνολο δείκτες τα ζητούμενα.

Αυτές οι δείκτες υποδεικνύουν στην αρχή και τέλος επισήμανση κειμένου που ταιριάζει με τους όρους που παρέχονται στο ερώτημα αναζήτησης.
Μπορείτε να καθορίσετε τους δείκτες έναρξης και λήξης που θα χρησιμοποιηθεί από την αναζήτηση για να αναδιπλώσετε το επισήμανση όρων.

**Παράδειγμα ερωτήματος αναζήτησης**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Δείγμα αποτέλεσμα:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Παρατηρήστε ότι το αποτέλεσμα του παραπάνω περιέχει ένα μήνυμα σφάλματος που περιλαμβάνει το πρόθεμα και προσαρτημένο.

## <a name="computer-groups"></a>Ομάδες υπολογιστή

Οι ομάδες υπολογιστή είναι ειδική αποθηκευμένες αναζητήσεις που επιστρέφουν ένα σύνολο από τους υπολογιστές.  Μπορείτε να χρησιμοποιήσετε μια ομάδα του υπολογιστή σας σε άλλα ερωτήματα για να περιορίσετε τα αποτελέσματα για τους υπολογιστές στην ομάδα.  Μια ομάδα υπολογιστή έχει υλοποιηθεί ως μια αποθηκευμένη αναζήτηση με ετικέτα ομάδας με μια τιμή του υπολογιστή.

Ακολουθεί μια απάντηση δείγμα για μια ομάδα υπολογιστή.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
        }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Ανάκτηση ομάδες υπολογιστή

Χρησιμοποιήστε τη μέθοδο Get με το Αναγνωριστικό ομάδας για να ανακτήσετε μια ομάδα υπολογιστή.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Δημιουργία ή ενημέρωση μιας ομάδας υπολογιστή

Χρησιμοποιήστε τη μέθοδο τοποθέτηση με ένα μοναδικό αποθηκευτεί ID αναζήτησης για να δημιουργήσετε μια νέα ομάδα υπολογιστή. Εάν χρησιμοποιείτε μια υπάρχουσα ομάδα το Αναγνωριστικό του υπολογιστή, στη συνέχεια, ότι ένας θα τροποποιηθούν. Όταν δημιουργείτε μια ομάδα υπολογιστή στην κονσόλα OMS, στη συνέχεια, το Αναγνωριστικό δημιουργείται από την ομάδα και το όνομα.

Το ερώτημα που χρησιμοποιείται για τον ορισμό της ομάδας πρέπει να επιστρέφει ένα σύνολο των υπολογιστών για την ομάδα για να λειτουργήσει σωστά.  Συνιστάται να τερματίσετε το ερώτημά σας με *| Διακριτό υπολογιστή* για να βεβαιωθείτε ότι τα σωστά δεδομένα που επιστρέφονται.

Τον ορισμό της αποθηκευμένης αναζήτησης πρέπει να συμπεριλάβετε μια ετικέτα με το όνομα ομάδας με μια τιμή του υπολογιστή για την αναζήτηση για να ταξινομηθούν ως ομάδα υπολογιστή.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Διαγραφή ομάδων υπολογιστή

Χρησιμοποιήστε τη μέθοδο Delete με το Αναγνωριστικό ομάδας για να διαγράψετε μια ομάδα υπολογιστή.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε σχετικά με το [αρχείο καταγραφής αναζητήσεις](log-analytics-log-searches.md) για τη δημιουργία ερωτημάτων με προσαρμοσμένα πεδία για τα κριτήρια.
