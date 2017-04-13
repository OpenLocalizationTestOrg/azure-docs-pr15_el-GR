<properties 
    pageTitle="Νέο σχήμα έκδοση 2015-08-01-preview" 
    description="Μάθετε πώς να συντάξετε τον ορισμό JSON για την πιο πρόσφατη έκδοση της λογικής εφαρμογών" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>
    
# <a name="new-schema-version-2015-08-01-preview"></a>Νέο σχήμα έκδοση 2015-08-01-preview

Η νέα διάταξη και API έκδοση για τις εφαρμογές της λογικής έχει έναν αριθμό βελτιώσεις που τη βελτίωση της αξιοπιστίας και ευκολία στη χρήση των εφαρμογών που είναι λογική. Υπάρχουν 4 βασικές διαφορές:

1. Ο τύπος ενέργειας **APIApp** έχει ενημερωθεί σε νέο τύπο ενέργειας **APIConnection** .
2. **Επαναλάβετε** έχει μετονομαστεί σε **Foreach**.
3. Η εφαρμογή API **Ακρόασης HTTP** δεν απαιτείται πλέον.
4. Κλήση θυγατρικών ροών εργασίας χρησιμοποιεί μια νέα διάταξη.

## <a name="1-moving-to-api-connections"></a>1. Μετακίνηση στις συνδέσεις API

Η αλλαγή μεγαλύτερων είναι ότι δεν χρειάζεστε πλέον για την ανάπτυξη εφαρμογών API στη συνδρομή σας Azure για χρήση του API. Υπάρχουν 2 τρόποι χρήσης APIs:
* Διαχειριζόμενο API
* Το προσαρμοσμένο API Web

Κάθε μία από αυτές τις γίνεται λίγο διαφορετικά επειδή τους διαχείρισης και μοντέλα φιλοξενίας είναι διαφορετικές. Ένα πλεονέκτημα της αυτό το μοντέλο είναι που δεν είναι πλέον είστε περιορίζεται σε πόρους που έχουν αναπτυχθεί σε σας ομάδα πόρων. 

### <a name="managed-apis"></a>Διαχειριζόμενες APIs

Υπάρχουν αρκετά του API του που πραγματοποιείται από τη Microsoft για λογαριασμό σας, όπως το Office 365, Salesforce, Twitter, FTP κ.λπ... Ορισμένες από αυτές τις διαχειριζόμενες API μπορεί να χρησιμοποιηθεί ως-είναι, όπως το Bing μετάφραση, ενώ άλλοι χρειάζονται ρύθμισης παραμέτρων. Αυτή η ρύθμιση παραμέτρων ονομάζεται *σύνδεσης*.

Για παράδειγμα, κατά τη χρήση του Office 365, πρέπει να δημιουργήσετε μια σύνδεση που περιέχει το διακριτικό εισόδου του Office 365. Αυτό το διακριτικό αποθηκεύονται με ασφάλεια και ανανεωθεί, έτσι ώστε η εφαρμογή της λογικής πάντοτε να καλέσετε το API του Office 365. Εναλλακτικά, εάν θέλετε να συνδεθείτε με το διακομιστή SQL ή FTP, πρέπει να δημιουργήσετε μια σύνδεση που έχει τη συμβολοσειρά σύνδεσης. 

Μέσα σε τον ορισμό ονομάζονται αυτές οι ενέργειες `APIConnection`. Ακολουθεί ένα παράδειγμα μιας σύνδεσης που καλεί Office 365 για να στείλετε ένα μήνυμα ηλεκτρονικού ταχυδρομείου:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Το τμήμα της ισχύος που είναι μοναδικός για συνδέσεις API είναι το `host` αντικειμένου. Αυτό περιέχει δύο μέρη: `api` και `connection`.

Το `api` περιλαμβάνει το περιβάλλον εκτέλεσης φιλοξενείται διεύθυνση URL της όπου που διαχειριζόμενο API. Μπορείτε να δείτε καλώντας όλων των διαθέσιμων διαχειριζόμενο API για εσάς `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Κατά τη χρήση του API, ενδέχεται να ή μπορεί να μην έχετε οποιεσδήποτε **παραμέτρους σύνδεσης** που ορίζονται από το. Εάν δεν το, δεν απαιτείται καμία **σύνδεση** . Εάν υπάρχει, στη συνέχεια, θα πρέπει να δημιουργήσετε μια σύνδεση. Όταν δημιουργείτε αυτήν τη σύνδεση του θα έχουν το όνομα που επιλέξατε και, στη συνέχεια, μπορείτε να αναφέρετε που σε το `connection` αντικείμενο εντός του `host` αντικειμένου. Για να δημιουργήσετε μια σύνδεση σε μια ομάδα πόρων, καλέστε:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Με το εξής κυρίως κείμενο:


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>Ανάπτυξη διαχειριζόμενων APIs σε ένα πρότυπο διαχείρισης πόρων Azure

Μπορείτε να δημιουργήσετε μια πλήρη εφαρμογή σε ένα πρότυπο ARM εφόσον δεν απαιτεί αλληλεπιδραστικών εισόδου. Εάν το απαιτεί εισόδου, μπορείτε να ρυθμίσετε τα πάντα με το πρότυπο ARM, αλλά θα εξακολουθεί να πρέπει να επισκεφθείτε την πύλη για να εξουσιοδοτήσετε τις συνδέσεις. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Μπορείτε να δείτε σε αυτό το παράδειγμα ότι οι συνδέσεις είναι απλώς κανονική πόροι που βρίσκονται στο σας ομάδα πόρων. Αυτά αναφοράς του managedAPIs που είναι διαθέσιμες σε εσάς στη συνδρομή σας.

### <a name="your-custom-web-apis"></a>Το προσαρμοσμένο APIs Web

Εάν χρησιμοποιείτε το δικό σας API του (συγκεκριμένα, δεν Microsoft-Διαχείριση αυτά), πρέπει να χρησιμοποιήσετε την ενσωματωμένη ενέργεια **HTTP** για να καλέσετε τους. Για να έχετε μια ιδανική εμπειρία, που θα πρέπει να εμφανίζουν ένα τελικό σημείο swagger για το API. Αυτό θα ενεργοποιήσει τη σχεδίαση εφαρμογών λογική για την απόδοση τις εισροές και εκροές για το API. Χωρίς μια swagger, το εργαλείο σχεδίασης μόνο θα μπορείτε να εμφανίσετε τις εισροές και εκροές ως αδιαφανές JSON αντικείμενα.

Ακολουθεί ένα παράδειγμα που εμφανίζει το νέο `metadata.apiDefinitionUrl` ιδιότητα:
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

Εάν φιλοξενείτε το API Web στην **Υπηρεσία εφαρμογών** , στη συνέχεια, θα αυτόματα εμφανίζεται στη λίστα των ενεργειών που είναι διαθέσιμες στη σχεδίαση. Εάν όχι, θα πρέπει να επικολλήστε τη διεύθυνση URL απευθείας. Το τελικό σημείο swagger πρέπει να είναι χωρίς έλεγχο ταυτότητας προκειμένου να μπορεί να χρησιμοποιηθεί μέσα σε τη σχεδίαση εφαρμογών λογικής (παρόλο που μπορεί να ασφαλής το API ίδιο με όποιο μεθόδους υποστηρίζονται στο το Swagger).

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>Χρήση των εφαρμογών σας ήδη ανεπτυγμένος API με 2015-08-01-preview

Εάν ήδη αναπτύξει το API εφαρμογής, μπορείτε να καλέσετε το μέσω της ενέργειας **HTTP** .

Για παράδειγμα, εάν χρησιμοποιείτε το Dropbox σε λίστα αρχεία, ενδέχεται να έχετε κάπως έτσι σε ορισμού έκδοση του σχήματος **2014 12-01-προεπισκόπηση** :

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Μπορείτε να δημιουργήσετε την ισοδύναμη ενέργεια HTTP όπως κάτω από το (η ενότητα παραμέτρους από το παραμένει ορισμού εφαρμογής λογικής αμετάβλητη):

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Παρουσίαση μερικών μέσω αυτές τις ιδιότητες ένα-προς-ένα:

| Ιδιότητα ενέργειας |  Περιγραφή |
| --------------- | -----------  |
| `type` | `Http`Αντί για`APIapp` |
| `metadata.apiDefinitionUrl` | Εάν θέλετε να χρησιμοποιήσετε αυτήν την ενέργεια στη σχεδίαση εφαρμογών λογικής, που θα θέλετε να συμπεριλάβετε το τελικό σημείο μετα-δεδομένων. Δομείται από:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Δομείται από:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Πάντα`POST` |
| `inputs.body` | Ίδιο με τις παραμέτρους της εφαρμογής api | 
| `inputs.authentication` | Ίδιο με τον έλεγχο ταυτότητας εφαρμογής api |

Αυτή η προσέγγιση θα πρέπει να λειτουργήσει για όλες τις ενέργειες εφαρμογής API. Ωστόσο, πρέπει να θυμάστε ότι αυτές τις προηγούμενες εφαρμογές API δεν υποστηρίζονται πλέον και θα πρέπει να μετακινήσετε σε μία από τις δύο άλλες επιλογές παραπάνω (ένα διαχειριζόμενο API ή φιλοξενίας σας προσαρμοσμένη API Web).

## <a name="2-repeat-renamed-to-foreach"></a>2. μετονομαστεί σε Foreach επανάληψη

Για την προηγούμενη έκδοση του σχήματος που θα σας λάβατε πολλά σχόλια των πελατών που **επαναλάβετε** ήταν προκαλεί σύγχυση και δεν σωστά καταγραφή ότι ήταν πραγματικά έναν για κάθε βρόχο. Ως αποτέλεσμα, θα σας έχουν μετονομαστεί το σε **Foreach**. Για παράδειγμα:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Τώρα θα να γράφονται με:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Προηγουμένως, η συνάρτηση `@repeatItem()` που χρησιμοποιήθηκε για την αναφορά του τρέχοντος στοιχείου που iterated πάνω από. Αυτό έχει απλοποιηθεί απλώς `@item()`. 

### <a name="referencing-the-outputs-of-the-foreach"></a>Αναφορά εξόδους του από το Foreach
Για να απλοποιήσετε περαιτέρω, το εξόδους **Foreach** ενέργειες δεν θα να περικλείεται σε ένα αντικείμενο που ονομάζεται **repeatItems**. Αυτό σημαίνει ότι, ότι έχουν εξόδους του από το παραπάνω Επανάληψη:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Τώρα θα είναι:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Κατά την αναφορά αυτές τις εξόδους, για να μεταβείτε στο κυρίως σώμα του την ενέργεια που θα πρέπει να κάνετε:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

Τώρα μπορείτε να κάνετε αντί για αυτό:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

Με αυτές τις αλλαγές, οι συναρτήσεις `@repeatItem()`, `@repeatBody()` και `@repeatOutputs()` καταργούνται.

## <a name="3-native-http-listener"></a>3. εγγενούς ακρόασης HTTP 
Οι δυνατότητες ακρόασης HTTP τώρα είναι ενσωματωμένο, επομένως δεν χρειάζεται πλέον να αναπτύξετε μια εφαρμογή API ακρόασης HTTP. Διαβάστε σχετικά με [όλες τις λεπτομέρειες για το πώς μπορείτε να κάνετε εδώ σας λογικής εφαρμογή τελικού σημείου δυνατή η κλήση](app-service-logic-http-endpoint.md). 

Με αυτές τις αλλαγές, η συνάρτηση `@accessKeys()` καταργείται και έχει αντικατασταθεί με την `@listCallbackURL()` συνάρτηση για τους σκοπούς της γρήγορα το τελικό σημείο (όταν είναι απαραίτητο). Επιπλέον, τώρα πρέπει να ορίσετε τουλάχιστον ένα έναυσμα σε εφαρμογή της λογικής τώρα. Εάν θέλετε να `/run` τη ροή εργασίας, θα πρέπει να έχετε μία από μια `manual`, `apiConnectionWebhook` ή `httpWebhook` εναύσματα. 

## <a name="4-calling-child-workflows"></a>4. ροές εργασίας θυγατρικό καλείτε

Κλήση θυγατρικών ροών εργασίας απαιτείται στο παρελθόν, θα σε αυτήν τη ροή εργασίας, γρήγορα το διακριτικό πρόσβασης και, στη συνέχεια, την επικόλληση που κατά τον ορισμό της λογικής εφαρμογής που θέλετε να καλέσετε αυτό το παιδί. Με τη νέα έκδοση σχήματος, ο μηχανισμός εφαρμογές λογικής δημιουργεί αυτόματα μια συσχετισμών Ασφαλείας κατά το χρόνο εκτέλεσης για τη θυγατρική ροή εργασιών, γεγονός που σημαίνει ότι δεν χρειάζεται να επικολλήσετε τον ορισμό οποιαδήποτε απορρήτου.  Ακολουθεί ένα παράδειγμα:

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

Ένα δεύτερο βελτίωσης είναι θα σας θα παρέχοντας τις ροές εργασίας θυγατρικό πλήρη πρόσβαση στην πρόσκληση σε εισερχόμενα. Αυτό σημαίνει ότι μπορείτε να περάσετε παραμέτρους στην ενότητα *ερωτήματα* και στο αντικείμενο *κεφαλίδες* και ότι μπορείτε να ορίσετε πλήρως όλο το σώμα.

Τέλος, υπάρχουν απαιτούμενες αλλαγές για τη θυγατρική ροή εργασιών. Ενώ το πριν από θα μπορούσε να μόνο την κλήση θυγατρική ροή εργασιών απευθείας. Τώρα, θα πρέπει να ορίσετε ένα τελικό σημείο έναυσμα εντός της ροής εργασίας για το γονικό στοιχείο για να καλέσετε. Γενικά, αυτό σημαίνει ότι θα προσθέσετε ένα έναυσμα του τύπου **μη αυτόματη** και, στη συνέχεια, χρησιμοποιήστε που στον ορισμό γονικό στοιχείο. Λάβετε υπόψη ότι η `host` ιδιότητα συγκεκριμένα έχει μια `triggerName`, επειδή πρέπει πάντα να καθορίζετε ποια έναυσμα που χρησιμοποιείτε.

## <a name="other-changes"></a>Άλλες αλλαγές

### <a name="new-queries-property"></a>Νέα ιδιότητα ερωτημάτων
Όλοι οι τύποι ενέργεια υποστηρίζει τώρα μια νέα εισόδου που ονομάζεται **ερωτήματα**. Αυτό μπορεί να είναι ένα αντικείμενο δομημένα και όχι να χρειάζεται να συγκεντρώσετε με μη αυτόματο τρόπο τη συμβολοσειρά.

### <a name="parse-function-renamed"></a>συνάρτηση Parse() μετονομαστεί
Όπως θα σας θα σύντομα προσθέσετε περισσότερους τύπους περιεχομένου, το `parse()` συνάρτηση έχει μετονομαστεί σε `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Σύντομα διαθέσιμο: APIs ενοποίησης για μεγάλες επιχειρήσεις
Σε αυτό το σημείο στο φορά, θα σας μην κάνετε ακόμη έχει διαχειριζόμενο εκδόσεις των API ενοποίησης για μεγάλες επιχειρήσεις διαθέσιμη (όπως AS2). Αυτά θα σύντομα διαθέσιμο καλύπτεται στο ο [Χάρτης](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). Στο μεταξύ, μπορείτε να χρησιμοποιήσετε τις υπάρχουσες ανεπτυγμένος API BizTalk μέσω της ενέργειας HTTP, όπως καλύπτεται επάνω από "Χρησιμοποιώντας τις εφαρμογές σας ήδη ανεπτυγμένος API."
