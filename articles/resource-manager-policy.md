<properties
    pageTitle="Azure πολιτικής διαχείρισης πόρων | Microsoft Azure"
    description="Περιγράφει τον τρόπο χρήσης πολιτικής διαχείρισης πόρων Azure ώστε να μην παραβιάσεις σε διαφορετικό εύρος όπως συνδρομή, ομάδες πόρων ή μεμονωμένους πόρους."
    services="azure-resource-manager"
    documentationCenter="na"
    authors="ravbhatnagar"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="gauravbh;tomfitz"/>

# <a name="use-policy-to-manage-resources-and-control-access"></a>Χρησιμοποιήσετε την πολιτική για να διαχειριστείτε τους πόρους και τον έλεγχο της πρόσβασης

Azure διαχείριση πόρων τώρα σάς επιτρέπει να ελέγχετε την πρόσβαση μέσω προσαρμοσμένες πολιτικές. Με τις πολιτικές, μπορείτε να αποτρέψετε τους χρήστες στην εταιρεία σας από διακοπή συμβάσεις που απαιτούνται για τη διαχείριση πόρων της εταιρείας σας. 

Μπορείτε να δημιουργήσετε ορισμούς πολιτικής που περιγράφουν τις ενέργειες ή τους πόρους που δεν επιτρέπονται ρητά. Μπορείτε να αντιστοιχίσετε αυτές τις ορισμούς πολιτικής από την επιθυμητή εμβέλεια, όπως τη συνδρομή, ομάδα πόρων ή ένας μεμονωμένος πόρος. 

Σε αυτό το άρθρο θα σας εξηγεί τη βασική δομή της γλώσσας ορισμού πολιτικής που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε πολιτικές. Στη συνέχεια, θα σας περιγράφουν πώς μπορείτε να εφαρμόσετε αυτές τις πολιτικές σε διαφορετικό εύρος.

## <a name="how-is-it-different-from-rbac"></a>Πώς είναι διαφορετικό από RBAC;

Υπάρχουν μερικές βασικές διαφορές μεταξύ της πολιτικής και έλεγχος πρόσβασης βάσει ρόλων, αλλά το πρώτο πράγμα που πρέπει να κατανοήσετε είναι ότι οι πολιτικές και RBAC συνεργασία. Για να χρησιμοποιήσετε πολιτικές, πρέπει να έχουν πιστοποιηθεί μέσω RBAC. Σε αντίθεση με RBAC, πολιτικής είναι μια προεπιλεγμένη επιτρέπουν και ρητή απόρριψη συστήματος. 

RBAC εστιάζει σε τις ενέργειες που ένας **χρήστης** μπορεί να εκτελέσει σε διαφορετικό εύρος. Για παράδειγμα, ένας συγκεκριμένος χρήστης προστίθεται το ρόλο του συνεργάτη για μια ομάδα πόρων στο επιθυμητό εύρος, ώστε ο χρήστης να κάνουν αλλαγές σε αυτήν την ομάδα πόρων. 

Πολιτική εστιάζει σε ενέργειες **πόρων** σε διάφορες εύρους. Για παράδειγμα, μέσω των πολιτικών, μπορείτε να ελέγχετε τους τύπους των πόρων που να μπορεί να παρασχεθεί ή να περιορίσετε τις θέσεις όπου μπορεί να παρασχεθεί τους πόρους.

## <a name="common-scenarios"></a>Κοινά σενάρια

Ένα συνηθισμένο σενάριο είναι να απαιτείται τμημάτων ετικετών για ετοιμάζετε σκοπό. Μια εταιρεία μπορεί να θέλετε να επιτρέψετε λειτουργίες μόνο όταν το Κέντρο κατάλληλο κόστους είναι συσχετισμένη; Διαφορετικά, απόρριψη της αίτησης. Αυτή η πολιτική βοηθά τους σας χρεώνει το Κέντρο κατάλληλο κόστους για τις εργασίες που εκτελούνται.

Άλλο ένα κοινό σενάριο είναι ότι η εταιρεία μπορεί να θέλετε να ελέγξετε τις θέσεις όπου δημιουργούνται πόρους. Ή μπορεί να θέλουν να ελέγχετε την πρόσβαση στους πόρους, επιτρέποντας μόνο συγκεκριμένους τύπους πόρων για να παρασχεθεί.

Ομοίως, μια εταιρεία να ελέγχετε την υπηρεσία καταλόγου ή να επιβάλετε τις επιθυμητές συνθήκες ονοματοθεσίας για τους πόρους.

Χρήση των πολιτικών, εύκολα μπορείτε να επιτύχετε αυτά τα σενάρια.

## <a name="policy-definition-structure"></a>Δομή ορισμού πολιτικής

Ορισμός πολιτικής δημιουργείται JSON. Αποτελείται από μία ή περισσότερες συνθήκες/λογικούς τελεστές που ορίζουν τις ενέργειες και ένα εφέ που σας ενημερώνει για το τι συμβαίνει όταν πληρούνται οι όροι. Το σχήμα είναι Δημοσιευμένος στην [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

Βασικά, μια πολιτική περιέχει τα ακόλουθα στοιχεία:

**Συνθήκη/λογικούς τελεστές:** ένα σύνολο όρων που μπορεί να χειριστεί σε ένα σύνολο λογικούς τελεστές.

**Εφέ:** τι συμβαίνει όταν ικανοποιείται η συνθήκη – είτε απόρριψη ή ελέγχου. Εφέ ελέγχου εκπέμπει ένα αρχείο καταγραφής της υπηρεσίας συμβάντων προειδοποίησης. Για παράδειγμα, ένας διαχειριστής να δημιουργήσει μια πολιτική η οποία έχει ως αποτέλεσμα ένα συμβάν ελέγχου εάν οποιοσδήποτε δημιουργεί μια μεγάλη Εικονική. Ο διαχειριστής μπορεί να εξετάσετε τα αρχεία καταγραφής αργότερα.

    {
      "if" : {
          <condition> | <logical operator>
      },
      "then" : {
          "effect" : "deny | audit | append"
      }
    }
    
## <a name="policy-evaluation"></a>Αξιολόγηση πολιτικής

Πολιτικές αξιολογούνται όταν δημιουργούνται πόρους. Σε περίπτωση ανάπτυξη προτύπου, πολιτικές αξιολογούνται κατά τη δημιουργία της κάθε πόρο στο πρότυπο. 

> [AZURE.NOTE] Προς το παρόν, η πολιτική δεν έχει ως αποτέλεσμα τύπους πόρων που δεν υποστηρίζουν ετικέτες, είδος και θέση, όπως ο τύπος πόρου Microsoft.Resources/deployments. Αυτή η υποστήριξη θα προστεθεί στο μέλλον. Για να αποφύγετε προβλήματα συμβατότητας με παλαιότερες εκδόσεις, πρέπει να ρητά καθορίζετε τον τύπο όταν συντάσσετε πολιτικές. Για παράδειγμα, μια πολιτική ετικέτας που δεν καθορίσετε τύπους εφαρμόζεται για όλους τους τύπους. Σε αυτή την περίπτωση, μια ανάπτυξη προτύπου ενδέχεται να αποτύχει, εάν υπάρχει μια ένθετη πόρων που δεν υποστηρίζει ετικέτες και ο τύπος πόρου ανάπτυξης έχει προστεθεί αξιολόγησης πολιτικής. 

## <a name="logical-operators"></a>Λογικοί τελεστές

Τα υποστηριζόμενα λογικούς τελεστές μαζί με τη σύνταξη είναι οι εξής:

| Όνομα τελεστή     | Σύνταξη         |
| :------------- | :------------- |
| Δεν            | "not": {&lt;συνθήκη ή τελεστή &gt;}             |
| Και           | "allOf": [{&lt;συνθήκη ή τελεστή &gt;}, {&lt;συνθήκη ή τελεστή &gt;}] |
| Ή                         | "anyOf": [{&lt;συνθήκη ή τελεστή &gt;}, {&lt;συνθήκη ή τελεστή &gt;}] |

Διαχείριση πόρων σας επιτρέπει να καθορίσετε περίπλοκη λογική στην πολιτική σας μέσω του ένθετου τελεστές. Για παράδειγμα, για να απορρίψετε τη δημιουργία πόρων σε μια συγκεκριμένη θέση για έναν τύπο συγκεκριμένο πόρο. Παράδειγμα ένθετου τελεστές εμφανίζεται παρακάτω.

## <a name="conditions"></a>Συνθήκες

Μια συνθήκη είναι είτε ένα **πεδίο** ή **προέλευσης** ικανοποιούν ορισμένα κριτήρια. Τα ονόματα των συνθήκης υποστηριζόμενες και σύνταξη είναι:

| Το όνομα της συνθήκης | Σύνταξη                |
| :------------- | :------------- |
| Ισούται με             | "ισούται με": "&lt;τιμή&gt;"               |
| Όπως                  | "μου αρέσει": "&lt;τιμή&gt;"                   |
| Περιέχει          | "περιέχει": "&lt;τιμή&gt;"|
| Στο                        | "σε": ["&lt;τιμή1&gt;","&lt;τιμή2&gt;"]|
| ContainsKey    | "containsKey": "&lt;όνομα κλειδιού&gt;" |
| Υπάρχει     | "υπάρχει": "&lt;bool&gt;" |

### <a name="fields"></a>Πεδία

Συνθήκες είναι μορφοποιημένη χρησιμοποιώντας πεδία και προελεύσεις. Ένα πεδίο αναπαριστά ιδιότητες στο φορτίο αίτηση πόρου που χρησιμοποιείται για να περιγράψετε την κατάσταση του πόρου. Προέλευση αντιπροσωπεύει χαρακτηριστικά της αίτησης ίδια. 

Υποστηρίζονται τα παρακάτω πεδία και πηγές:

Πεδία: **όνομα**, **είδος**, **Τύπος**, **θέση**, **ετικετών**, **ετικέτες.** *, and * *ιδιότητα ψευδώνυμο**. 

### <a name="property-aliases"></a>Ιδιότητα ψευδώνυμα 
Ιδιότητα ψευδώνυμο είναι ένα όνομα που μπορεί να χρησιμοποιηθεί σε έναν ορισμό πολιτικής για να αποκτήσετε πρόσβαση στις πόρων τύπου συγκεκριμένες ιδιότητες, όπως οι ρυθμίσεις και τις SKU. Λειτουργεί σε όλες τις εκδόσεις API όπου υπάρχει η ιδιότητα. Ψευδώνυμα μπορεί να ανακτηθεί, χρησιμοποιώντας το REST API φαίνεται παρακάτω (Powershell υποστήριξη θα προστεθεί στο μέλλον):

    GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
    
Εμφανίζεται κάτω από τον ορισμό των ένα ψευδώνυμο. Όπως μπορείτε να δείτε, ένα ψευδώνυμο καθορίζει διαδρομές σε διαφορετικές εκδόσεις API, ακόμα και όταν υπάρχει μια αλλαγή όνομα ιδιότητας. 

    "aliases": [
        {
          "name": "Microsoft.Storage/storageAccounts/sku.name",
          "paths": [
            {
              "path": "properties.accountType",
              "apiVersions": [
                "2015-06-15",
                "2015-05-01-preview"
              ]
            },
            {
              "path": "sku.name",
              "apiVersions": [
                "2016-01-01"
              ]
            }
          ]
        }
    ]

Προς το παρόν, τα υποστηριζόμενα ψευδώνυμα είναι:

| Ψευδώνυμο | Περιγραφή |
| ---------- | ----------- |
| {resourceType}/sku.name | Οι τύποι υποστηρίζονται πόρων είναι: Microsoft.Compute/virtualMachines,<br />Microsoft.Storage/storageAccounts,<br />Microsoft.Web/serverFarms,<br /> Microsoft.Scheduler/jobcollections,<br />Microsoft.DocumentDB/databaseAccounts,<br />Microsoft.Cache/Redis,<br />Microsoft.CDN/profiles |
| {resourceType}/sku.family | Τύπος πόρου υποστηριζόμενες είναι Microsoft.Cache/Redis |
| {resourceType}/sku.capacity | Τύπος πόρου υποστηριζόμενες είναι Microsoft.Cache/Redis |
| Microsoft.Compute/virtualMachines/imagePublisher |  |
| Microsoft.Compute/virtualMachines/imageOffer  |  |
| Microsoft.Compute/virtualMachines/imageSku  |  |
| Microsoft.Compute/virtualMachines/imageVersion  |  |
| Microsoft.Cache/Redis/enableNonSslPort |  |
| Microsoft.Cache/Redis/shardCount |  |
| Microsoft.SQL/servers/version |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveId |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveName |  |
| Microsoft.SQL/servers/databases/edition |  |
| Microsoft.SQL/servers/databases/elasticPoolName |  |
| Microsoft.SQL/servers/elasticPools/dtu |  |
| Microsoft.SQL/servers/elasticPools/edition |  |

Προς το παρόν, η πολιτική λειτουργεί μόνο σε αιτήσεις ΤΟΠΟΘΈΤΗΣΗ. 

## <a name="effect"></a>Εφέ
Η πολιτική υποστηρίζει τρεις τύποι εφέ - **Απόρριψη**, **Έλεγχος**και **Προσάρτηση**. 

- Απόρριψη δημιουργεί ένα συμβάν στο αρχείο καταγραφής ελέγχου και αποτύχει η αίτηση
- Έλεγχος δημιουργεί ένα συμβάν στο αρχείο καταγραφής ελέγχου, αλλά δεν αποτυγχάνει στην πρόσκληση
- Προσάρτηση προσθέτει το καθορισμένο σύνολο πεδίων στην πρόσκληση σε 

Για να **προσαρτήσετε**, πρέπει να δώσετε τις ακόλουθες λεπτομέρειες:

    ....
    "effect": "append",
    "details": [
      {
        "field": "field name",
        "value": "value of the field"
      }
    ]

Η τιμή μπορεί να είναι μια συμβολοσειρά ή ένα αντικείμενο μορφή JSON. 

## <a name="policy-definition-examples"></a>Παραδείγματα ορισμού πολιτικής

Τώρα ας ρίξουμε μια ματιά Τρόπος ορισμού της πολιτικής για να επιτύχετε τα προηγούμενα σενάρια.

### <a name="chargeback-require-departmental-tags"></a>Ετοιμάζετε: Απαιτείται τμημάτων ετικετών

Η ακόλουθη πολιτική απορρίπτει αιτήσεις που δεν διαθέτουν μια ετικέτα που περιέχει το κλειδί "costCenter".

    {
      "if": {
        "not" : {
          "field" : "tags",
          "containsKey" : "costCenter"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

Η ακόλουθη πολιτική προσαρτά ετικέτα costCenter με μια προκαθορισμένη τιμή όταν υπάρχουν χωρίς ετικέτες. 

    {
      "if": {
        "field": "tags",
        "exists": "false"
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags",
            "value": {"costCenter":"myDepartment" }
          }
        ]
      }
    }
    
Η ακόλουθη πολιτική προσαρτά ετικέτα costCenter με μια προκαθορισμένη τιμή όταν δεν υπάρχει η ετικέτα costCenter, αλλά υπάρχουν άλλες ετικέτες. 

    {
      "if": {
        "allOf": [
          {
            "field": "tags",
            "exists": "true"
          },
          {
            "field": "tags.costCenter",
            "exists": "false"
          }
        ]
    
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags.costCenter",
            "value": "myDepartment"
          }
        ]
      }
    }


### <a name="geo-compliance-ensure-resource-locations"></a>Συμβατότητα παν: Βεβαιωθείτε ότι θέσεις πόρων

Το παρακάτω παράδειγμα εμφανίζει μια πολιτική που απορρίπτει αιτήσεις όπου η θέση δεν είναι Βόρειας Ευρώπη ή Δυτική Ευρώπη.

    {
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="service-curation-select-the-service-catalog"></a>Υπηρεσία Curation: Επιλέξτε την υπηρεσία καταλόγου

Το παρακάτω παράδειγμα εμφανίζει μια πολιτική που επιτρέπει ενέργειες μόνο σχετικά με τις υπηρεσίες του τύπου Microsoft.Resources/\*, Microsoft.Compute/\*, Microsoft.Storage/\*, Microsoft.Network/\* επιτρέπονται. Οτιδήποτε άλλο δεν επιτρέπεται.

    {
      "if" : {
        "not" : {
          "anyOf" : [
            {
              "field" : "type",
              "like" : "Microsoft.Resources/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Compute/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Storage/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Network/*"
            }
          ]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="use-approved-skus"></a>Χρήση εγκρίθηκε SKU

Το παρακάτω παράδειγμα εμφανίζει τη χρήση των ιδιοτήτων ψευδώνυμο για να περιορίσετε τα SKU. Στο παράδειγμα, μόνο Standard_LRS και Standard_GRS έχουν εγκριθεί για λογαριασμούς του χώρου αποθήκευσης.

    {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          },
          {
            "not": {
              "allof": [
                {
                  "field": "Microsoft.Storage/storageAccounts/sku.name",
                  "in": ["Standard_LRS", "Standard_GRS"]
                }
              ]
            }
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
    

### <a name="naming-convention"></a>Κανόνες ονοματοθεσίας

Το παρακάτω παράδειγμα εμφανίζει τη χρήση του χαρακτήρα μπαλαντέρ, που υποστηρίζεται από τη συνθήκη "μου αρέσει". Η συνθήκη δηλώνει ότι, εάν το όνομα συμφωνεί με το μοτίβο αναφερόμενης (namePrefix\*nameSuffix), στη συνέχεια, να απορρίψετε την αίτηση.

    {
      "if" : {
        "not" : {
          "field" : "name",
          "like" : "namePrefix*nameSuffix"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }
    
### <a name="tag-requirement-just-for-storage-resources"></a>Απαίτηση ετικέτα μόνο για τους πόρους αποθήκευσης

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να κάνετε ένθεση λογικών τελεστών ώστε να απαιτεί μια ετικέτα εφαρμογής για μόνο τους πόρους του χώρου αποθήκευσης.

    {
        "if": {
            "allOf": [
              {
                "not": {
                  "field": "tags",
                  "containsKey": "application"
                }
              },
              {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
              }
            ]
        },
        "then": {
            "effect": "audit"
        }
    }

## <a name="policy-assignment"></a>Αντιστοίχιση πολιτικής

Οι πολιτικές εφαρμόζονται σε διαφορετικό εύρος όπως συνδρομή, ομάδων πόρων και μεμονωμένους πόρους. Πολιτικές μεταβιβάζονται από όλους τους πόρους θυγατρικό. Επομένως, αν μια πολιτική εφαρμόζεται σε μια ομάδα πόρων, είναι ισχύουν για όλους τους πόρους στη συγκεκριμένη ομάδα πόρων.

## <a name="creating-a-policy"></a>Δημιουργία πολιτικής

Αυτή η ενότητα παρέχει λεπτομέρειες σχετικά με πώς μια πολιτική μπορούν να δημιουργηθούν με REST API.

### <a name="create-policy-definition-with-rest-api"></a>Δημιουργία πολιτικής ορισμού με REST API

Μπορείτε να δημιουργήσετε μια πολιτική με το [REST API για ορισμούς πολιτικής](https://msdn.microsoft.com/library/azure/mt588471.aspx). Το REST API σάς επιτρέπει να δημιουργήσετε και διαγραφή ορισμοί πολιτικής και λήψη πληροφοριών σχετικά με την υπάρχουσα ορισμών.

Για να δημιουργήσετε μια πολιτική, εκτελέστε:

    PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

Με μια αίτηση κυρίως κείμενο παρόμοιο με το ακόλουθο παράδειγμα:

    {
      "properties":{
        "policyType":"Custom",
        "description":"Test Policy",
        "policyRule":{
          "if" : {
            "not" : {
              "field" : "tags",
              "containsKey" : "costCenter"
            }
          },
          "then" : {
            "effect" : "deny"
          }
        }
      },
      "name":"testdefinition"
    }


Για την έκδοση api Χρησιμοποιήστε *2016-04-01*. Για παραδείγματα και περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [REST API για ορισμούς πολιτικής](https://msdn.microsoft.com/library/azure/mt588471.aspx).

### <a name="create-policy-definition-using-powershell"></a>Δημιουργία πολιτικής ορισμού χρήση του PowerShell

Μπορείτε να δημιουργήσετε έναν ορισμό πολιτικής χρησιμοποιώντας το cmdlet New-AzureRmPolicyDefinition. Το παρακάτω παράδειγμα δημιουργεί μια πολιτική επιτρέποντας σε πόρους μόνο στην Ευρώπη Βόρειας και Δυτική Ευρώπη.

    $policy = New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain regions" -Policy '{  
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'          

Το αποτέλεσμα της εκτέλεσης είναι αποθηκευμένο στο αντικείμενο $policy και μπορούν να χρησιμοποιηθούν αργότερα κατά την αντιστοίχιση πολιτικής. Για την παράμετρο πολιτικής, τη διαδρομή προς ένα αρχείο .json που περιέχει την πολιτική μπορεί επίσης να παρέχεται αντί για που καθορίζει τα ενσωματωμένα πολιτικής.

    New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain    regions" -Policy "path-to-policy-json-on-disk"

### <a name="create-policy-definition-using-azure-cli"></a>Δημιουργία πολιτικής ορισμού χρησιμοποιώντας Azure CLI

Μπορείτε να δημιουργήσετε έναν ορισμό πολιτικής χρησιμοποιώντας το azure CLI με την εντολή Ορισμός πολιτικής, όπως φαίνεται παρακάτω. Το παρακάτω παράδειγμα δημιουργεί μια πολιτική επιτρέποντας σε πόρους μόνο στην Ευρώπη Βόρειας και Δυτική Ευρώπη.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy-string '{   
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'    
    

Είναι δυνατό να καθορίσετε τη διαδρομή προς ένα αρχείο .json που περιέχει την πολιτική αντί για που καθορίζει τα ενσωματωμένα πολιτικής, όπως φαίνεται παρακάτω.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy "path-to-policy-json-on-disk"


## <a name="applying-a-policy"></a>Εφαρμογή της πολιτικής

### <a name="policy-assignment-with-rest-api"></a>Αντιστοίχιση πολιτικής με REST API

Μπορείτε να εφαρμόσετε τον ορισμό της πολιτικής στο επιθυμητό εύρος μέσω του [REST API για αναθέσεις πολιτικής](https://msdn.microsoft.com/library/azure/mt588466.aspx). Το REST API σάς επιτρέπει να δημιουργήσετε και διαγραφή πολιτικής αναθέσεις και λήψη πληροφοριών σχετικά με τις υπάρχουσες εκχωρήσεις.

Για να δημιουργήσετε μια ανάθεση πολιτικής, εκτελέστε:

    PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}

{Πολιτικής-ανάθεσης} είναι το όνομα της πολιτικής ανάθεσης. Για την έκδοση api Χρησιμοποιήστε *2016-04-01*. 

Με μια αίτηση κυρίως κείμενο παρόμοιο με το ακόλουθο παράδειγμα:

    {
      "properties":{
        "displayName":"VM_Policy_Assignment",
        "policyDefinitionId":"/subscriptions/########/providers/Microsoft.Authorization/policyDefinitions/testdefinition",
        "scope":"/subscriptions/########-####-####-####-############"
      },
      "name":"VMPolicyAssignment"
    }

Για παραδείγματα και περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [REST API για αναθέσεις πολιτικής](https://msdn.microsoft.com/library/azure/mt588466.aspx).

### <a name="policy-assignment-using-powershell"></a>Χρήση του PowerShell αντιστοίχιση πολιτικής

Μπορείτε να εφαρμόσετε την πολιτική δημιουργήθηκε παραπάνω μέσω του PowerShell για να την επιθυμητή εμβέλεια, χρησιμοποιώντας το cmdlet New-AzureRmPolicyAssignment:

    New-AzureRmPolicyAssignment -Name regionPolicyAssignment -PolicyDefinition $policy -Scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
$Policy ακολουθεί το αντικείμενο πολιτικής που επιστράφηκε ως αποτέλεσμα η εκτέλεση τα cmdlet New-AzureRmPolicyDefinition όπως φαίνεται παραπάνω. Δείτε το πεδίο εφαρμογής είναι το όνομα της ομάδας πόρων που καθορίζετε.

Εάν θέλετε να καταργήσετε την αντιστοίχιση πολιτικής παραπάνω, μπορείτε να το κάνετε ως εξής:

    Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Μπορείτε να λάβετε, να αλλάξετε ή να καταργήσετε ορισμοί πολιτικής μέσω Get-AzureRmPolicyDefinition, ορισμός AzureRmPolicyDefinition και cmdlet για κατάργηση AzureRmPolicyDefinition αντίστοιχα.

Ομοίως, μπορείτε να γρήγορα, να αλλάξετε, ή να καταργήσετε αναθέσεις πολιτικής μέσω Get-AzureRmPolicyAssignment, ορισμός AzureRmPolicyAssignment και κατάργηση AzureRmPolicyAssignment cmdlet για αντίστοιχα.

### <a name="policy-assignment-using-azure-cli"></a>Αντιστοίχιση πολιτικής χρήσης Azure CLI

Μπορείτε να εφαρμόσετε την πολιτική δημιουργήθηκε παραπάνω μέσω Azure CLI στο επιθυμητό εύρος, χρησιμοποιώντας την εντολή ανάθεση πολιτικής:

    azure policy assignment create --name regionPolicyAssignment --policy-definition-id /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/<policy-name> --scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
Δείτε το πεδίο εφαρμογής είναι το όνομα της ομάδας πόρων που καθορίζετε. Εάν η τιμή της παραμέτρου πολιτικής-ορισμού-ταυτότητας είναι άγνωστη, είναι δυνατό να αποκτήσετε μέσω του Azure CLI, όπως φαίνεται παρακάτω: 

    azure policy definition show <policy-name>

Εάν θέλετε να καταργήσετε την αντιστοίχιση πολιτικής παραπάνω, μπορείτε να το κάνετε ως εξής:

    azure policy assignment delete --name regionPolicyAssignment --scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Μπορείτε να λάβετε, να αλλάξετε ή να καταργήσετε πολιτική ορισμών μέσω της πολιτικής definition εμφάνιση, ορισμός, εντολές και διαγραφής αντίστοιχα.

Ομοίως, μπορείτε να λάβετε, να αλλάξετε ή να καταργήσετε πολιτική αναθέσεις έως την αντιστοίχιση πολιτικής εμφάνιση και να διαγράψετε εντολές αντίστοιχα.

##<a name="policy-audit-events"></a>Πολιτική ελέγχου συμβάντων

Αφού έχετε εφαρμόσει την πολιτική, θα ξεκινήσει για να δείτε τα συμβάντα που σχετίζονται με την πολιτική. Μπορείτε να κάνετε είτε μεταβείτε στην πύλη, χρησιμοποιήστε PowerShell ή το Azure CLI λήψη αυτών των δεδομένων. 

### <a name="policy-audit-events-using-powershell"></a>Χρήση του PowerShell συμβάντων ελέγχου πολιτικής

Για να δείτε όλα τα συμβάντα που σχετίζονται με εφέ "Απόρριψη", μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή PowerShell:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/deny/action"} 

Για να δείτε όλα τα συμβάντα που σχετίζονται με ελέγχου εφέ, μπορείτε να χρησιμοποιήσετε την ακόλουθη εντολή:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/audit/action"} 

### <a name="policy-audit-events-using-azure-cli"></a>Πολιτική συμβάντων ελέγχου με χρήση Azure CLI

Για να προβάλετε όλες συμβάντα από έναν πόρο που σχετίζονται με το εφέ "Απόρριψη" Ομαδοποίηση, μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή CLI:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/deny/action\")"

Για να δείτε όλα τα συμβάντα που σχετίζονται με ελέγχου εφέ, μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή CLI:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/audit/action\")"
