<properties
   pageTitle="Διαχείριση πόρων ΥΠΌΛΟΙΠΑ APIs | Microsoft Azure"
   description="Επισκόπηση τα παραδείγματα ελέγχου ταυτότητας και η χρήση APIs ΥΠΌΛΟΙΠΑ από διαχειριστή πόρων"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="navale;tomfitz;"/>
   
# <a name="resource-manager-rest-apis"></a>Διαχείριση πόρων ΥΠΌΛΟΙΠΑ APIs

> [AZURE.SELECTOR]
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Πύλη](./azure-portal/resource-group-portal.md) 
- [REST API](resource-manager-rest-api.md)

Πίσω από κάθε κλήση για να Azure διαχείριση πόρων, πίσω από κάθε πρότυπο ανεπτυγμένος, πίσω από κάθε λογαριασμό χώρου αποθήκευσης που έχετε διαμορφώσει υπάρχει ενός ή πολλών κλήσεων στο RESTful API του Azure διαχείριση πόρων. Αυτό το θέμα αφορά καθέναν από αυτά τα API και πώς μπορείτε να καλέσετε τους χωρίς να χρησιμοποιήσετε οποιαδήποτε SDK καθόλου. Αυτό μπορεί να είναι πολύ χρήσιμο όταν θέλετε πλήρη έλεγχο σε όλες τις αιτήσεις Azure ή εάν το SDK για τη γλώσσα που προτιμάτε δεν είναι διαθέσιμη ή δεν υποστηρίζει τις λειτουργίες που θέλετε να εκτελέσετε.

Σε αυτό το άρθρο θα περνούν από κάθε API που εκτίθεται στο Azure, αλλά θα χρησιμοποιήσετε προτιμάτε κάποια ως παράδειγμα πώς μπορείτε να προχωρήσετε και να συνδεθείτε με τους. Εάν γνωρίζετε τα βασικά στοιχεία, στη συνέχεια, μπορείτε να προχωρήσετε και να διαβάσετε το [Azure διαχείριση πόρων REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn790568.aspx) για να βρείτε λεπτομερείς πληροφορίες σχετικά με τον τρόπο για να χρησιμοποιήσετε το υπόλοιπο των API.

## <a name="authentication"></a>Έλεγχος ταυτότητας
Έλεγχος ταυτότητας για ARM γίνεται από Azure Active Directory (AD). Για να συνδεθείτε με οποιαδήποτε API πρέπει πρώτα να τον έλεγχο ταυτότητας με Azure AD για τη λήψη ενός διακριτικού ελέγχου ταυτότητας που μπορεί να περάσει σε κάθε αίτηση. Θα σας περιγράφει αμιγώς κλήσης απευθείας με τα ΥΠΌΛΟΙΠΑ API, θα σας θα επίσης λαμβάνεται ως δεδομένο ότι δεν θέλετε για τον έλεγχο ταυτότητας με κωδικό πρόσβασης Κανονικό όνομα χρήστη όπου μια pop-up οθόνης μπορεί να σας ζητήσει για το όνομα χρήστη και τον κωδικό πρόσβασης και ίσως ακόμα και άλλους μηχανισμούς ελέγχου ταυτότητας που χρησιμοποιείται σε σενάρια ελέγχου ταυτότητας δύο παραγόντων. Γι ' αυτό, θα δημιουργήσουμε αυτό που ονομάζεται μια εφαρμογή του Azure AD και αρχής υπηρεσιών που θα χρησιμοποιηθεί για τη σύνδεση με. Αλλά να θυμάστε ότι Azure AD υποστηρίζει αρκετές διαδικασίες ελέγχου ταυτότητας και όλες τις θα μπορούσαν να χρησιμοποιηθούν για την ανάκτηση που διακριτικού ελέγχου ταυτότητας που χρειαζόμαστε για οι επακόλουθες απαιτήσεις API.
Ακολουθήστε [Δημιουργία Azure AD εφαρμογών και υπηρεσιών αρχή](./resource-group-create-service-principal-portal.md) για οδηγίες βήμα προς βήμα.

### <a name="generating-an-access-token"></a>Δημιουργία ενός διακριτικού πρόσβασης 
Έλεγχος ταυτότητας σε σχέση με Azure AD γίνεται με κλήση σε Azure AD, που βρίσκεται στο login.microsoftonline.com. Προκειμένου να ελέγξει την ταυτότητά σας πρέπει να έχετε τις ακόλουθες πληροφορίες:

* Αναγνωριστικό μισθωτή Azure AD (το όνομα που Azure AD που χρησιμοποιείτε για να συνδεθείτε, συχνά η ίδια με την εταιρεία σας, αλλά δεν είναι απαραίτητο)
* Αναγνωριστικό εφαρμογής (ληφθούν κατά το βήμα Azure AD εφαρμογή δημιουργίας)
* Τον κωδικό πρόσβασης (που επιλέξατε κατά τη δημιουργία της εφαρμογής Azure AD)

Στο το κάτω από την αίτηση HTTP, βεβαιωθείτε ότι για να αντικαταστήσετε "Azure AD μισθωτή Αναγνωριστικό", "Αναγνωριστικό εφαρμογής" και "Κωδικός πρόσβασης" με τις σωστές τιμές.

**Αίτηση HTTP γενικής χρήσης:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... θα (Εάν ο έλεγχος ταυτότητας με επιτυχία) έχει ως αποτέλεσμα μια απάντηση παρόμοια με αυτήν:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(Το access_token στην παραπάνω απάντηση έχουν περικοπεί για να αυξήσετε την αναγνωσιμότητα)

**Δημιουργία διακριτικό πρόσβασης χρησιμοποιώντας πάρτι:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Δημιουργία διακριτικό πρόσβασης με χρήση του PowerShell:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Η απόκριση περιέχει ένα διακριτικό πρόσβασης, πληροφορίες σχετικά με το χρονικό διάστημα που το διακριτικό είναι έγκυρη και πληροφορίες σχετικά με το τι πόρων, μπορείτε να χρησιμοποιήσετε αυτό το διακριτικό για.
Το διακριτικό πρόσβασης που έχετε λάβει στην προηγούμενη κλήση HTTP πρέπει να περάσετε στο για την αίτηση όλων για το API ARM ως κεφαλίδα με το όνομα "Έγκριση" με την τιμή "Φορέα YOUR_ACCESS_TOKEN". Παρατηρήστε το διάστημα μεταξύ "Φορέα" και το διακριτικό πρόσβασης.

Όπως μπορείτε να δείτε από το παραπάνω αποτέλεσμα HTTP, το διακριτικό είναι έγκυρες για ένα συγκεκριμένο χρονικό διάστημα κατά την οποία πρέπει να προσωρινή αποθήκευση και εκ νέου χρήση που ίδια διακριτικού. Ακόμα και αν είναι δυνατό να πραγματοποιήσουν έλεγχο ταυτότητας σε Azure AD για κάθε κλήση API, θα ήταν ιδιαίτερα αποτελεσματική.

## <a name="calling-arm-rest-apis"></a>Κλήση APIs ΥΠΌΛΟΙΠΟ ARM

[Azure διαχείριση πόρων ΥΠΌΛΟΙΠΑ APIs τεκμηριώνονται εδώ](https://msdn.microsoft.com/library/azure/dn790568.aspx) και το του εκτός εμβέλειας για αυτό το πρόγραμμα εκμάθησης για τη χρήση της κάθε και κάθε έγγραφο. Παρούσα τεκμηρίωση θα χρησιμοποιήσουν μόνο σε λίγα API για να εξηγήσετε τη βασική χρήση των API και μετά από αυτό που αναφέρονται στην τεκμηρίωση του επίσημη.

### <a name="list-all-subscriptions"></a>Λίστα όλων των συνδρομών

Μία από τις πιο απλός λειτουργίες που μπορείτε να κάνετε είναι να παραθέσετε τα διαθέσιμα συνδρομών που μπορείτε να αποκτήσετε πρόσβαση. Στο το κάτω από την αίτηση, μπορείτε να δείτε πώς το διακριτικό πρόσβασης μεταβιβάζεται στο ως κεφαλίδα.

(Αντικαταστήστε YOUR_ACCESS_TOKEN με την πραγματική διακριτικό πρόσβασης).

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... ως αποτέλεσμα, θα εμφανιστεί μια λίστα με τις συνδρομές που επιτρέπεται να έχει πρόσβαση σε αυτό το κεφάλαιο υπηρεσίας

(Συνδρομή αναγνωριστικά παρακάτω έχουν περικοπεί για αναγνωσιμότητα)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Λίστα όλων των ομάδων πόρων σε μια συγκεκριμένη συνδρομή

Όλοι οι πόροι είναι διαθέσιμη με τα API ARM είναι ένθετο μέσα σε μια ομάδα πόρων. Πρόκειται να ερωτήματος ARM για υπάρχουσες ομάδες πόρων μας χρησιμοποιώντας τη συνδρομή του κάτω από την αίτηση HTTP GET. Παρατηρήστε πώς το Αναγνωριστικό συνδρομής μεταβιβάζεται στο ως τμήμα της διεύθυνσης URL αυτήν τη στιγμή.

(Αντικατάσταση YOUR_ACCESS_TOKEN και SUBSCRIPTION_ID με το πραγματικό διακριτικό πρόσβασης και Αναγνωριστικό συνδρομής)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Εξαρτάται από την απάντηση που λαμβάνετε εάν έχετε ομάδες πόρων που ορίζονται από το και εάν Ναι, τον αριθμό.

(Συνδρομή αναγνωριστικά παρακάτω έχουν περικοπεί για αναγνωσιμότητα)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Δημιουργήστε μια ομάδα πόρων

Μέχρι στιγμής μας έχετε μόνο υποβολή ερωτημάτων τα API ARM για πληροφορίες, ώρα εργαζόμαστε δημιουργία ορισμένους πόρους αντί για αυτό και Ας ξεκινήσουμε με απλούστερο από αυτές όλων, μια ομάδα πόρων. Η παρακάτω αίτηση HTTP δημιουργεί μια νέα ομάδα πόρων σε μια περιοχή/θέση της επιλογής σας και προσθέτει μία ή περισσότερες ετικέτες σε αυτό (προσθέτει το δείγμα κάτω από το στοιχείο στην πραγματικότητα μόνο μία ετικέτα).

(Αντικαταστήστε YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME με η πραγματική διακριτικό πρόσβασης, Αναγνωριστικό συνδρομής και το όνομα της ομάδας πόρων που θέλετε να δημιουργήσετε)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Εάν είναι επιτυχής, θα λάβετε μια παρόμοια απόκριση σε αυτό

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Έχετε δημιουργήσει με επιτυχία μια ομάδα πόρων στο Azure. Συγχαρητήρια!

### <a name="deploy-resources-to-a-resource-group-using-an-arm-template"></a>Ανάπτυξη των πόρων σε μια ομάδα πόρων χρησιμοποιώντας ένα πρότυπο ARM

Με ARM, μπορείτε να αναπτύξετε τους πόρους σας με τη χρήση προτύπων ARM. Ένα πρότυπο ARM ορίζει πολλούς πόρους και τις εξαρτήσεις τους. Για αυτήν την ενότητα θα απλώς υποθέσουμε ότι είστε εξοικειωμένοι με τα πρότυπα ARM και θα απλώς σας δείξουμε πώς μπορείτε να πραγματοποιήσετε την κλήση API για να ξεκινήσετε την ανάπτυξη ενός. Εδώ, μπορείτε να βρείτε μια αναλυτική τεκμηρίωση ARM προτύπων.

Ανάπτυξη ενός προτύπου ARM δεν διαφέρουν πολύ για το πώς μπορείτε να καλέσετε άλλους APIs. Ένα σημαντικό αναλογίες είναι ότι ανάπτυξης ενός προτύπου μπορεί να διαρκέσει αρκετά μεγάλο χρονικό διάστημα, ανάλογα με το τι είναι το εσωτερικό του προτύπου, και η κλήση API απλώς θα επιστρέψει και είναι προς τα επάνω σε εσάς ως προγραμματιστής ερώτημα για την κατάσταση της ανάπτυξης για να μάθετε περισσότερα, όταν ολοκληρωθεί η ανάπτυξη.

Για αυτό το παράδειγμα, ας χρησιμοποιήσουμε δημόσια εκτεθειμένων προτύπου ARM διαθέσιμη σε [GitHub](https://github.com/Azure/azure-quickstart-templates). Το πρότυπο που θα χρησιμοποιήσετε θα αναπτύξετε μια Εικονική Linux στην περιοχή Δυτική ΗΠΑ. Ακόμα κι αν αυτό το πρότυπο θα έχουν το πρότυπο που είναι διαθέσιμες σε ένα αποθετήριο δημόσια όπως GitHub, μπορείτε επίσης να επιλέξετε για τη μεταβίβαση το πλήρες πρότυπο ως μέρος της αίτησης. Σημειώστε ότι παρέχουμε τιμές παραμέτρων ως μέρος της αίτησης που θα χρησιμοποιηθεί μέσα στο πρότυπο που χρησιμοποιήθηκαν.

(Αντικαταστήστε SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD και DNS_NAME_FOR_PUBLIC_IP στις τιμές που είναι κατάλληλη για την αίτησή σας)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Για να βελτιώσετε την αναγνωσιμότητα αυτής της τεκμηρίωσης έχει παραλειφθεί η αρκετά μεγάλη απόκριση JSON για αυτήν την αίτηση. Η απόκριση θα περιέχει πληροφορίες σχετικά με την ανάπτυξη του προτύπου που μόλις δημιουργήσατε.

