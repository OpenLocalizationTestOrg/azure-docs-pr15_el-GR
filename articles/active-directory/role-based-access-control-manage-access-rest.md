<properties
    pageTitle="Διαχείριση έλεγχος πρόσβασης βάσει ρόλων με το REST API"
    description="Διαχείριση έλεγχος πρόσβασης βάσει ρόλων με το REST API"
    services="active-directory"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="multiple"
    ms.tgt_pltfrm="rest-api"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-role-based-access-control-with-the-rest-api"></a>Διαχείριση έλεγχος πρόσβασης βάσει ρόλων με το REST API

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API](role-based-access-control-manage-access-rest.md)

Βάσει ρόλων πρόσβασης ελέγχου (RBAC) στην πύλη του Azure και API διαχείρισης πόρων Azure σάς βοηθά να διαχειριστείτε την πρόσβαση για τη συνδρομή σας και τους πόρους σε λεπτομερή επίπεδο. Με αυτήν τη δυνατότητα, μπορείτε να εκχωρήσετε πρόσβαση για χρήστες, ομάδες ή αρχές υπηρεσίας καταλόγου Active Directory, εκχώρηση ρόλων ορισμένες τους σε μια ειδική εμβέλεια.

## <a name="list-all-role-assignments"></a>Λίστα με όλες τις αναθέσεις ρόλων

Παραθέτει σε λίστα όλες τις αναθέσεις ρόλων στην καθορισμένη εμβέλεια και subscopes.

Λίστα εκχωρήσεις ρόλων, πρέπει να έχετε πρόσβαση σε `Microsoft.Authorization/roleAssignments/read` λειτουργίας στο εύρος. Όλες οι ενσωματωμένες ρόλοι έχουν εκχωρηθεί πρόσβαση σε αυτήν τη λειτουργία. Για περισσότερες πληροφορίες σχετικά με τις αναθέσεις ρόλων και τη διαχείριση πρόσβασης για τους πόρους Azure, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Αίτηση

Χρησιμοποιήστε τη μέθοδο **ΓΡΉΓΟΡΑ** με το ακόλουθο URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

Εντός του URI, κάντε το εξής υποκατάστατα για να προσαρμόσετε την αίτησή σας:

1. Αντικαταστήστε *{εύρος}* με την εμβέλεια του οποίου θέλετε να παραθέσετε τις αναθέσεις ρόλων. Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να καθορίσετε την εμβέλεια για τα διάφορα επίπεδα:

  - Συνδρομή: /subscriptions/ {αναγνωριστικό συνδρομής}  
  - Ομάδα πόρων: /subscriptions/ {αναγνωριστικό συνδρομής} / resourceGroups/myresourcegroup1  
  - Πόρων: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Αντικαταστήστε *{έκδοση api}* με 2015-07-01.

3. Αντικαταστήστε το *{φίλτρο}* με τη συνθήκη που θέλετε να εφαρμόσετε για να φιλτράρετε τη λίστα ανάθεσης ρόλων:

  - Λίστα εκχώρησης ρόλου για μόνο το καθορισμένο πεδίο, χωρίς να συμπεριλαμβάνει τις εκχωρήσεις ρόλων στο subscopes:`atScope()`    
  - Λίστα εκχώρησης ρόλου για ένα συγκεκριμένο χρήστη, ομάδας ή εφαρμογή:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
  - Λίστα εκχώρησης ρόλου για έναν συγκεκριμένο χρήστη, συμπεριλαμβανομένων και αυτών που έχουν μεταβιβαστεί από ομάδες |`assignedTo('{objectId of user}')`

### <a name="response"></a>Απόκριση

Ο κωδικός κατάστασης: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Λήψη πληροφοριών σχετικά με μια εκχώρηση ρόλων

Λαμβάνει πληροφορίες σχετικά με μια ανάθεση μεμονωμένο ρόλο που καθορίζεται από το αναγνωριστικό ανάθεσης ρόλων.

Για πληροφορίες σχετικά με μια εκχώρηση ρόλων, πρέπει να έχετε πρόσβαση σε `Microsoft.Authorization/roleAssignments/read` λειτουργία. Όλες οι ενσωματωμένες ρόλοι έχουν εκχωρηθεί πρόσβαση σε αυτήν τη λειτουργία. Για περισσότερες πληροφορίες σχετικά με τις αναθέσεις ρόλων και τη διαχείριση πρόσβασης για τους πόρους Azure, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Αίτηση

Χρησιμοποιήστε τη μέθοδο **ΓΡΉΓΟΡΑ** με το ακόλουθο URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Εντός του URI, κάντε το εξής υποκατάστατα για να προσαρμόσετε την αίτησή σας:

1. Αντικαταστήστε *{εύρος}* με την εμβέλεια του οποίου θέλετε να παραθέσετε τις αναθέσεις ρόλων. Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να καθορίσετε την εμβέλεια για τα διάφορα επίπεδα:

  - Συνδρομή: /subscriptions/ {αναγνωριστικό συνδρομής}  
  - Ομάδα πόρων: /subscriptions/ {αναγνωριστικό συνδρομής} / resourceGroups/myresourcegroup1  
  - Πόρων: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Αντικαταστήστε *{ρόλο-ανάθεσης-αναγνωριστικό}* με το αναγνωριστικό GUID της εκχώρησης ρόλου.

3. Αντικαταστήστε *{έκδοση api}* με 2015-07-01.

### <a name="response"></a>Απόκριση

Ο κωδικός κατάστασης: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Δημιουργήστε μια εκχώρηση ρόλων

Δημιουργήστε μια εκχώρηση ρόλων στο καθορισμένο εύρος για το καθορισμένο κεφάλαιο εκχώρηση το συγκεκριμένο ρόλο.

Για να δημιουργήσετε μια εκχώρηση ρόλων, πρέπει να έχετε πρόσβαση σε `Microsoft.Authorization/roleAssignments/write` λειτουργία. Η ενσωματωμένη ρόλων, μόνο *κάτοχο* και *Διαχειριστή πρόσβασης χρήστη* έχουν εκχωρηθεί πρόσβαση σε αυτήν τη λειτουργία. Για περισσότερες πληροφορίες σχετικά με τις αναθέσεις ρόλων και τη διαχείριση πρόσβασης για τους πόρους Azure, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Αίτηση

Χρησιμοποιήστε τη μέθοδο **ΤΟΠΟΘΈΤΗΣΗ** με το ακόλουθο URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Εντός του URI, κάντε το εξής υποκατάστατα για να προσαρμόσετε την αίτησή σας:

1. Αντικαταστήστε *{εύρος}* με το πεδίο στο οποίο θέλετε να δημιουργήσετε τις αναθέσεις ρόλων. Όταν δημιουργείτε μια εκχώρηση ρόλων στο γονικό εμβέλεια, όλα τα πεδία που θυγατρικό μεταβιβάζονται στην ίδια εκχώρηση ρόλου. Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να καθορίσετε την εμβέλεια για τα διάφορα επίπεδα:

  - Συνδρομή: /subscriptions/ {αναγνωριστικό συνδρομής}  
  - Ομάδα πόρων: /subscriptions/ {αναγνωριστικό συνδρομής} / resourceGroups/myresourcegroup1   
  - Πόρων: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Αντικαταστήστε *{ρόλο-ανάθεσης-αναγνωριστικό}* με νέο GUID, τι είναι το αναγνωριστικό GUID της νέας ανάθεσης ρόλων.

3. Αντικαταστήστε *{έκδοση api}* με 2015-07-01.

Για το σώμα της αίτησης, δώστε τις τιμές με την εξής μορφή:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Όνομα στοιχείου     | Απαιτείται | Τύπος   | Περιγραφή |
|------------------|----------|--------|-------------|
| roleDefinitionId | Ναι      | Συμβολοσειρά | Το αναγνωριστικό του ρόλου. Η μορφή του αναγνωριστικού είναι:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId      | Ναι      | Συμβολοσειρά | objectId του Azure AD κύριου (χρήστη, ομάδας ή κεφάλαιο υπηρεσίας) στο οποίο έχει ανατεθεί στο ρόλο. |

### <a name="response"></a>Απόκριση

Ο κωδικός κατάστασης: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Διαγράψτε μια εκχώρηση ρόλων

Διαγράψτε μια εκχώρηση ρόλων στο καθορισμένο εύρος.

Για να διαγράψετε μια εκχώρηση ρόλων, πρέπει να έχετε πρόσβαση σε το `Microsoft.Authorization/roleAssignments/delete` λειτουργία. Η ενσωματωμένη ρόλων, μόνο *κάτοχο* και *Διαχειριστή πρόσβασης χρήστη* έχουν εκχωρηθεί πρόσβαση σε αυτήν τη λειτουργία. Για περισσότερες πληροφορίες σχετικά με τις αναθέσεις ρόλων και τη διαχείριση πρόσβασης για τους πόρους Azure, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Αίτηση

Χρησιμοποιήστε τη μέθοδο **DELETE** με το ακόλουθο URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Εντός του URI, κάντε το εξής υποκατάστατα για να προσαρμόσετε την αίτησή σας:

1. Αντικαταστήστε *{εύρος}* με το πεδίο στο οποίο θέλετε να δημιουργήσετε τις αναθέσεις ρόλων. Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να καθορίσετε την εμβέλεια για τα διάφορα επίπεδα:

  - Συνδρομή: /subscriptions/ {αναγνωριστικό συνδρομής}  
  - Ομάδα πόρων: /subscriptions/ {αναγνωριστικό συνδρομής} / resourceGroups/myresourcegroup1  
  - Πόρος: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Αντικαταστήστε *{ρόλο-ανάθεσης-αναγνωριστικό}* με το αναγνωριστικό ανάθεσης ρόλων GUID.

3. Αντικαταστήστε *{έκδοση api}* με 2015-07-01.

### <a name="response"></a>Απόκριση

Ο κωδικός κατάστασης: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Λίστα όλων των ρόλων

Παραθέτει όλους τους ρόλους που είναι διαθέσιμες για την ανάθεση στο καθορισμένο εύρος.

Σε λίστα ρόλους, πρέπει να έχετε πρόσβαση σε `Microsoft.Authorization/roleDefinitions/read` λειτουργίας στο εύρος. Όλες οι ενσωματωμένες ρόλοι έχουν εκχωρηθεί πρόσβαση σε αυτήν τη λειτουργία. Για περισσότερες πληροφορίες σχετικά με τις αναθέσεις ρόλων και τη διαχείριση πρόσβασης για τους πόρους Azure, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Αίτηση

Χρησιμοποιήστε τη μέθοδο **ΓΡΉΓΟΡΑ** με το ακόλουθο URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

Εντός του URI, κάντε το εξής υποκατάστατα για να προσαρμόσετε την αίτησή σας:

1. Αντικαταστήστε *{εύρος}* , με την εμβέλεια για την οποία θέλετε να παραθέσετε τους ρόλους. Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να καθορίσετε την εμβέλεια για τα διάφορα επίπεδα:

  - Συνδρομή: /subscriptions/ {αναγνωριστικό συνδρομής}  
  - Ομάδα πόρων: /subscriptions/ {αναγνωριστικό συνδρομής} / resourceGroups/myresourcegroup1  
  - /Subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 πόρων  

2. Αντικαταστήστε *{έκδοση api}* με 2015-07-01.

3. Αντικαταστήστε το *{φίλτρο}* με τη συνθήκη που θέλετε να εφαρμόσετε για να φιλτράρετε τη λίστα με τους ρόλους:

  - Λίστα ρόλους που είναι διαθέσιμοι για ανάθεση στο καθορισμένο εύρος και κάθε της τα εξαρτημένα πεδία:`atScopeAndBelow()`
  - Αναζήτηση για ένα ρόλο χρησιμοποιώντας ακριβή εμφανιζόμενο όνομα: `roleName%20eq%20'{role-display-name}'`. Χρησιμοποιήστε τη διεύθυνση URL κωδικοποιημένη μορφή το ακριβές εμφανιζόμενο όνομα του ρόλου. Για παράδειγμα,`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Απόκριση

Ο κωδικός κατάστασης: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Λήψη πληροφοριών σχετικά με ένα ρόλο

Λαμβάνει πληροφορίες σχετικά με ένα μεμονωμένο ρόλο που καθορίζεται από το αναγνωριστικό ορισμού ρόλο. Για πληροφορίες σχετικά με ένα μεμονωμένο ρόλο χρησιμοποιώντας το εμφανιζόμενο όνομα, ανατρέξτε στο θέμα [λίστα όλους τους ρόλους](role-based-access-control-manage-access-rest.md#list-all-roles).

Για πληροφορίες σχετικά με ένα ρόλο, πρέπει να έχετε πρόσβαση σε `Microsoft.Authorization/roleDefinitions/read` λειτουργία. Όλες οι ενσωματωμένες ρόλοι έχουν εκχωρηθεί πρόσβαση σε αυτήν τη λειτουργία. Για περισσότερες πληροφορίες σχετικά με τις αναθέσεις ρόλων και τη διαχείριση πρόσβασης για τους πόρους Azure, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Αίτηση

Χρησιμοποιήστε τη μέθοδο **ΓΡΉΓΟΡΑ** με το ακόλουθο URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Εντός του URI, κάντε το εξής υποκατάστατα για να προσαρμόσετε την αίτησή σας:

1. Αντικαταστήστε *{εύρος}* με την εμβέλεια του οποίου θέλετε να παραθέσετε τις αναθέσεις ρόλων. Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να καθορίσετε την εμβέλεια για τα διάφορα επίπεδα:

  - Συνδρομή: /subscriptions/ {αναγνωριστικό συνδρομής}  
  - Ομάδα πόρων: /subscriptions/ {αναγνωριστικό συνδρομής} / resourceGroups/myresourcegroup1  
  - Πόρων: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Αντικαταστήστε *{ρόλο ορισμού αναγνωριστικού}* με το αναγνωριστικό GUID του ορισμού ρόλου.

3. Αντικαταστήστε *{έκδοση api}* με 2015-07-01.

### <a name="response"></a>Απόκριση

Ο κωδικός κατάστασης: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Δημιουργήστε έναν προσαρμοσμένο ρόλο
Δημιουργήστε έναν προσαρμοσμένο ρόλο.

Για να δημιουργήσετε ένα προσαρμοσμένο ρόλο, πρέπει να έχετε πρόσβαση σε `Microsoft.Authorization/roleDefinitions/write` πράξη σε όλα τα `AssignableScopes`. Η ενσωματωμένη ρόλων, μόνο *κάτοχο* και *Διαχειριστή πρόσβασης χρήστη* έχουν εκχωρηθεί πρόσβαση σε αυτήν τη λειτουργία. Για περισσότερες πληροφορίες σχετικά με τις αναθέσεις ρόλων και τη διαχείριση πρόσβασης για τους πόρους Azure, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Αίτηση

Χρησιμοποιήστε τη μέθοδο **ΤΟΠΟΘΈΤΗΣΗ** με το ακόλουθο URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Εντός του URI, κάντε το εξής υποκατάστατα για να προσαρμόσετε την αίτησή σας:

1. Αντικαταστήστε *{εύρος}* με την πρώτη *AssignableScope* του προσαρμοσμένου ρόλου. Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να καθορίσετε την εμβέλεια για τα διάφορα επίπεδα.

  - Συνδρομή: /subscriptions/ {αναγνωριστικό συνδρομής}  
  - Ομάδα πόρων: /subscriptions/ {αναγνωριστικό συνδρομής} / resourceGroups/myresourcegroup1  
  - Πόρος: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Αντικαταστήστε *{ρόλο ορισμού αναγνωριστικού}* με νέο GUID, τι είναι το αναγνωριστικό GUID του νέου προσαρμοσμένου ρόλου.

3. Αντικαταστήστε *{έκδοση api}* με 2015-07-01.

Για το σώμα της αίτησης, δώστε τις τιμές με την εξής μορφή:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Όνομα στοιχείου | Απαιτείται | Τύπος | Περιγραφή |
|--------------|----------|------|-------------|
| Όνομα         | Ναι | Συμβολοσειρά   | Αναγνωριστικό GUID του προσαρμοσμένου ρόλου.    |
| properties.roleName               | Ναι | Συμβολοσειρά   | Εμφανιζόμενο όνομα του προσαρμοσμένου ρόλου. Μέγιστο μέγεθος 128 χαρακτήρες.                        |
| Properties.Description            | Όχι  | Συμβολοσειρά   | Περιγραφή του προσαρμοσμένου ρόλου. Μέγιστο μέγεθος 1024 χαρακτήρες.                                               |
| Properties.Type                   | Ναι | Συμβολοσειρά   | Ορισμός σε "CustomRole."                                         |
| Properties.Permissions.Actions    | Ναι | Συμβολοσειρά] | Ένας πίνακας με ενέργεια συμβολοσειρές που καθορίζει τις λειτουργίες που έχουν εκχωρηθεί από το προσαρμοσμένο ρόλο.             |
| properties.permissions.notActions | Όχι  | Συμβολοσειρά] | Ένας πίνακας με ενέργεια συμβολοσειρές που καθορίζει τις λειτουργίες για να εξαιρέσετε από τις λειτουργίες που έχουν εκχωρηθεί από το προσαρμοσμένο ρόλο. |
| properties.assignableScopes       | Ναι | Συμβολοσειρά] | Ένας πίνακας με την οποία μπορεί να χρησιμοποιηθεί ο ρόλος προσαρμοσμένου εύρους.   |

### <a name="response"></a>Απόκριση

Ο κωδικός κατάστασης: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Ενημέρωση έναν προσαρμοσμένο ρόλο

Τροποποιήστε ένα προσαρμοσμένο ρόλο.

Για να τροποποιήσετε έναν προσαρμοσμένο ρόλο, πρέπει να έχετε πρόσβαση σε `Microsoft.Authorization/roleDefinitions/write` πράξη σε όλα τα `AssignableScopes`. Η ενσωματωμένη ρόλων, μόνο *κάτοχο* και *Διαχειριστή πρόσβασης χρήστη* έχουν εκχωρηθεί πρόσβαση σε αυτήν τη λειτουργία. Για περισσότερες πληροφορίες σχετικά με τις αναθέσεις ρόλων και τη διαχείριση πρόσβασης για τους πόρους Azure, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Αίτηση

Χρησιμοποιήστε τη μέθοδο **ΤΟΠΟΘΈΤΗΣΗ** με το ακόλουθο URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Εντός του URI, κάντε το εξής υποκατάστατα για να προσαρμόσετε την αίτησή σας:

1. Αντικαταστήστε *{εύρος}* με την πρώτη *AssignableScope* του προσαρμοσμένου ρόλου. Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να καθορίσετε την εμβέλεια για τα διάφορα επίπεδα:

  - Συνδρομή: /subscriptions/ {αναγνωριστικό συνδρομής}  
  - Ομάδα πόρων: /subscriptions/ {αναγνωριστικό συνδρομής} / resourceGroups/myresourcegroup1  
  - Πόρων: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Αντικαταστήστε *{ρόλο ορισμού αναγνωριστικού}* με το αναγνωριστικό GUID του προσαρμοσμένου ρόλου.

3. Αντικαταστήστε *{έκδοση api}* με 2015-07-01.

Για το σώμα της αίτησης, δώστε τις τιμές με την εξής μορφή:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Όνομα στοιχείου | Απαιτείται | Τύπος | Περιγραφή |
|--------------|----------|------|-------------|
| Όνομα         | Ναι      | Συμβολοσειρά | Αναγνωριστικό GUID του προσαρμοσμένου ρόλου. |
| properties.roleName | Ναι | Συμβολοσειρά | Εμφανιζόμενο όνομα του ρόλου ενημερωμένη προσαρμοσμένη. |
| Properties.Description | Όχι | Συμβολοσειρά | Περιγραφή το ενημερωμένο προσαρμοσμένο ρόλο. |
| Properties.Type | Ναι | Συμβολοσειρά | Ορισμός σε "CustomRole." |
| Properties.Permissions.Actions | Ναι | Συμβολοσειρά] | Ένας πίνακας με ενέργεια συμβολοσειρές που καθορίζει τις λειτουργίες στην οποία το ενημερωμένο προσαρμοσμένο ρόλος εκχωρεί πρόσβαση. |
| properties.permissions.notActions | Όχι | Συμβολοσειρά] | Ένας πίνακας με ενέργεια συμβολοσειρές που καθορίζει τις λειτουργίες για να εξαιρέσετε από τις εργασίες που εκχωρεί το ενημερωμένο προσαρμοσμένο ρόλο. |
| properties.assignableScopes | Ναι | Συμβολοσειρά] | Ένας πίνακας με την οποία μπορεί να χρησιμοποιηθεί το ενημερωμένο ρόλο προσαρμοσμένου εύρους. |

### <a name="response"></a>Απόκριση

Ο κωδικός κατάστασης: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Διαγραφή προσαρμοσμένης ρόλου

Διαγράψτε έναν προσαρμοσμένο ρόλο.

Για να διαγράψετε έναν προσαρμοσμένο ρόλο, πρέπει να έχετε πρόσβαση σε `Microsoft.Authorization/roleDefinitions/delete` πράξη σε όλα τα `AssignableScopes`. Η ενσωματωμένη ρόλων, μόνο *κάτοχο* και *Διαχειριστή πρόσβασης χρήστη* έχουν εκχωρηθεί πρόσβαση σε αυτήν τη λειτουργία. Για περισσότερες πληροφορίες σχετικά με τις αναθέσεις ρόλων και τη διαχείριση πρόσβασης για τους πόρους Azure, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Αίτηση

Χρησιμοποιήστε τη μέθοδο **DELETE** με το ακόλουθο URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Εντός του URI, κάντε το εξής υποκατάστατα για να προσαρμόσετε την αίτησή σας:

1. Αντικαταστήστε *{εύρος}* με το πεδίο στο οποίο θέλετε να διαγράψετε τον ορισμό ρόλου. Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να καθορίσετε την εμβέλεια για τα διάφορα επίπεδα:

  - Συνδρομή: /subscriptions/ {αναγνωριστικό συνδρομής}  
  - Ομάδα πόρων: /subscriptions/ {αναγνωριστικό συνδρομής} / resourceGroups/myresourcegroup1  
  - Πόρων: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Αντικαταστήστε *{ρόλο ορισμού αναγνωριστικού}* με το αναγνωριστικό GUID ρόλο ορισμό του προσαρμοσμένου ρόλου.

3. Αντικαταστήστε *{έκδοση api}* με 2015-07-01.

### <a name="response"></a>Απόκριση

Ο κωδικός κατάστασης: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```


[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
