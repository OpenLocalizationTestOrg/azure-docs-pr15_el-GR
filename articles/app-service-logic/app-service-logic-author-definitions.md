<properties 
    pageTitle="Συντάκτης ορισμών λογική εφαρμογής | Microsoft Azure" 
    description="Μάθετε πώς να συντάξετε τον ορισμό JSON για τις εφαρμογές της λογικής" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="author-logic-app-definitions"></a>Συντάκτης ορισμών λογική εφαρμογής
Αυτό το θέμα περιγράφει τον τρόπο χρήσης ορισμούς [Azure λογικής εφαρμογών](app-service-logic-what-are-logic-apps.md) , που είναι μια απλή, δηλωτικό γλώσσα JSON. Εάν δεν το έχετε κάνει ακόμα, δείτε [πώς μπορείτε να δημιουργήσετε μια νέα εφαρμογή λογικής](app-service-logic-create-a-logic-app.md) πρώτα. Μπορείτε επίσης να διαβάσετε το [υλικό αναφοράς πλήρους της γλώσσας ορισμού στο MSDN](http://aka.ms/logicappsdocs).

## <a name="several-steps-that-repeat-over-a-list"></a>Ορισμένα βήματα που επαναλαμβάνονται σε μια λίστα

Μπορείτε να αξιοποιήσετε τον [τύπο foreach](app-service-logic-loops-and-scopes.md) για να επαναλάβετε πάνω από έναν πίνακα έως 10 k στοιχείων και εκτελεί μια ενέργεια για κάθε μία.

## <a name="a-failure-handling-step-if-something-goes-wrong"></a>Ένα βήμα Αποτυχία χειρισμού εάν προκύψει κάποιο πρόβλημα

Θέλετε συχνά να έχουν τη δυνατότητα να συντάξετε ένα *βήμα αποκατάσταση εύρυθμης λειτουργίας* — ορισμένες λογικής που εκτελεί, εάν, απέτυχε **και μόνο αν**, μία ή περισσότερες από τις κλήσεις σας. Σε αυτό το παράδειγμα, θα σας λήψη δεδομένων από διάφορες θέσεις, αλλά εάν αποτύχει η κλήση, να θέλετε να ΔΗΜΟΣΙΕΎΣΕΤΕ ένα μήνυμα σε ένα σημείο, ώστε να μπορούν να εντοπίσετε αυτόν αποτυχία αργότερα:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

Μπορείτε να κάνετε χρήση των το `runAfter` ιδιοτήτων για να καθορίσετε το `postToErrorMessageQueue` θα πρέπει να εκτελεστεί μόνο `readData` είναι **απέτυχε**.  Αυτό μπορεί επίσης να μια λίστα με πιθανές τιμές, επομένως `runAfter` θα μπορούσε να είναι `["Succeeded", "Failed"]`.

Τέλος, επειδή μπορείτε τώρα ο χειρισμός έχει το σφάλμα, θα σας δεν είναι πλέον επισημάνετε την εκτέλεση ως **απέτυχε**. Όπως μπορείτε να δείτε εδώ, το αυτής της εκτέλεσης είναι **ολοκληρώθηκε με** , παρόλο που ένα βήμα απέτυχε, επειδή που συντάξατε το βήμα για να χειρίζεται αυτό το σφάλμα.

## <a name="two-or-more-steps-that-execute-in-parallel"></a>Δύο (ή περισσότερα) τα βήματα που εκτελεί παράλληλα

Για να έχετε πολλούς εκτέλεση ενεργειών παράλληλα, η `runAfter` ιδιότητα πρέπει να είναι ισοδύναμη κατά το χρόνο εκτέλεσης. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

Όπως μπορείτε να δείτε στο παραπάνω παράδειγμα, και τα δύο `branch1` και `branch2` έχουν ρυθμιστεί να εκτελούνται μετά την `readData`. Ως αποτέλεσμα, και τα δύο από τα παρακάτω διακλαδώσεις θα εκτελείται παράλληλα:

![Παράλληλη](./media/app-service-logic-author-definitions/parallel.png)

Μπορείτε να δείτε τη σήμανση χρόνου για δύο διακλαδώσεις είναι πανομοιότυπες. 

## <a name="join-two-parallel-branches"></a>Συμμετοχή σε δύο παράλληλες διακλαδώσεις

Μπορείτε να συμμετάσχετε σε δύο ενέργειες που έχουν οριστεί να εκτελέσει παράλληλα με την προσθήκη στοιχείων για την `runAfter` ιδιότητα παρόμοια με την παραπάνω.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![Παράλληλη](./media/app-service-logic-author-definitions/join.png)

## <a name="mapping-items-in-a-list-to-some-different-configuration"></a>Αντιστοίχιση στοιχείων σε μια λίστα σε ορισμένες διαφορετικές ρύθμισης παραμέτρων

Επόμενο, ας υποθέσουμε ότι θέλουμε να λαμβάνετε εντελώς διαφορετικό περιεχόμενο ανάλογα με την τιμή της ιδιότητας. Μπορούμε να δημιουργήσουμε ένα χάρτη των τιμών σε προορισμούς ως παράμετρο:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

Σε αυτήν την περίπτωση, λαμβάνουμε πρώτα μια λίστα με άρθρα και, στη συνέχεια, το δεύτερο βήμα που αναζητά σε ένα χάρτη, με βάση την κατηγορία που έχει οριστεί ως παράμετρο, ποια διεύθυνση URL για να λάβετε το περιεχόμενο από. 

Δύο στοιχεία για να δώσετε προσοχή εδώ: το [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) συνάρτηση χρησιμοποιείται για να ελέγξετε για να δείτε αν η κατηγορία ταιριάζει με μία από τις κατηγορίες γνωστά που ορίζονται από το. Δεύτερον, αφού λαμβάνουμε την κατηγορία, θα σας μπορεί να αποσπάσετε το στοιχείο του χάρτη με αγκύλες: `parameters[...]`. 

## <a name="working-with-strings"></a>Εργασία με συμβολοσειρές

Υπάρχουν διάφορες λειτουργίες που μπορούν να χρησιμοποιηθούν για να χειριστείτε συμβολοσειρά. Ας ρίξουμε ένα παράδειγμα όπου έχουμε μια συμβολοσειρά που θέλουμε να περάσει ένα σύστημα, αλλά θα σας δεν είστε βέβαιοι ότι κωδικοποίηση χαρακτήρων θα γίνεται ο χειρισμός σωστά. Μία επιλογή είναι να base64 κωδικοποιείτε αυτήν τη συμβολοσειρά. Ωστόσο, για να αποφύγετε Απαλλαχτείτε από μια διεύθυνση URL, πρόκειται να αντικαταστήσετε μερικούς χαρακτήρες. 

Θέλουμε επίσης μια δευτερεύουσα το τη σειρά ονομασία επειδή δεν χρησιμοποιούνται, τους πρώτους 5 χαρακτήρες.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

Εργασία από μέσα προς τα έξω:

1. Λήψη του [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) του ονόματος του orderer, αυτό επιστρέφει πάλι τον συνολικό αριθμό χαρακτήρων

2. Αφαίρεση 5 (επειδή θα θέλουμε συμβολοσειράς μικρότερη)

3. Στην πραγματικότητα λαμβάνει το [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring) . Ξεκινήσουμε στο ευρετήριο `5` και επιλέξτε το υπόλοιπο της συμβολοσειράς.

4. Αυτό δευτερεύουσα συμβολοσειρά για να μετατρέψετε ένα [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) συμβολοσειράς

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)όλα τα `+` χαρακτήρες με`-`

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)όλα τα `/` χαρακτήρες με`_`

## <a name="working-with-date-times"></a>Εργασία με ημερομηνίες και οι ώρες

Ημερομηνία φορές μπορεί να είναι χρήσιμο, ιδιαίτερα όταν προσπαθείτε να εξαγάγετε δεδομένα από μια προέλευση δεδομένων που δεν υποστηρίζει ομαλά **εναύσματα**.  Μπορείτε επίσης να χρησιμοποιήσετε ημερομηνίες και οι ώρες για να υπολογίσετε διαρκεί πόσο χρόνο διάφορα βήματα. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{parameters('order').id}"
            }
        },
        "timingWarning": {
            "actions" {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
                },
                "runAfter": {}
            }
            "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
        }
    },
    "outputs": {}
}
```

Σε αυτό το παράδειγμα, θα σας την εξαγωγή του `startTime` από το προηγούμενο βήμα. Στη συνέχεια, θα σας γρήγορα την τρέχουσα ώρα και αφαίρεση ένα δευτερόλεπτο:[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (θα μπορούσατε να χρησιμοποιήσετε άλλες μονάδες χρόνου όπως `minutes` ή `hours`). Τέλος, θα σας να συγκρίνετε αυτές τις δύο τιμές. Εάν η πρώτη είναι μικρότερη από το δεύτερο, στη συνέχεια, αυτό σημαίνει ότι περισσότερο από ένα δευτερόλεπτο πέρασε από τη σειρά ήταν πρώτη τοποθετείται. 

Επίσης, σημειώστε ότι μπορούμε να χρησιμοποιήσουμε συμβολοσειρά formatters μορφοποίηση ημερομηνιών: στη συμβολοσειρά ερωτήματος να χρησιμοποιήσω το [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow) για να λάβετε το RFC1123. Όλα ημερομηνίας [που τεκμηριώνονται στο MSDN](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow)μορφοποίησης. 

## <a name="using-deployment-time-parameters-for-different-environments"></a>Χρήση παραμέτρων χρόνο ανάπτυξης για διαφορετικά περιβάλλοντα

Είναι συνηθισμένο να έχει έναν κύκλο ζωής ανάπτυξης όπου έχετε ένα περιβάλλον ανάπτυξης, ένα περιβάλλον δημιουργίας σταδίων και, στη συνέχεια, ένα περιβάλλον παραγωγής. Σε όλα αυτά μπορεί να θέλετε ο ίδιος ορισμός, αλλά χρησιμοποιούν διαφορετική βάσεις δεδομένων, για παράδειγμα. Παρομοίως, ενδέχεται να θέλετε να χρησιμοποιήσετε τον ίδιο ορισμό σε πολλές διαφορετικές περιοχές για υψηλή διαθεσιμότητα, αλλά θέλετε κάθε παρουσία λογική εφαρμογής για να συνομιλήσετε με συγκεκριμένη περιοχή της βάσης δεδομένων. 

Σημειώστε ότι αυτό είναι διαφορετικό από λαμβάνοντας διαφορετικές παραμέτρους στο *περιβάλλον εκτέλεσης*, για ότι θα πρέπει να χρησιμοποιήσετε το `trigger()` λειτουργούν όπως καλούνται παραπάνω. 

Μπορείτε να ξεκινήσετε με τον ορισμό υπεραπλουστευμένο όπως το εξής:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

Κατόπιν, στην περιοχή η πραγματική `PUT` αίτηση για την εφαρμογή λογικής μπορείτε να παρέχετε την παράμετρο `uri`. Σημειώστε, καθώς δεν υπάρχει πλέον μια προεπιλεγμένη τιμή αυτή η παράμετρος είναι απαραίτητη στο φορτίο εφαρμογή λογικής:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

Σε κάθε περιβάλλον, στη συνέχεια, μπορείτε να δώσετε μια διαφορετική τιμή για το `connection` παραμέτρου. 

Ανατρέξτε στην [τεκμηρίωση REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) για όλες τις επιλογές που έχετε για τη δημιουργία και διαχείριση εφαρμογών λογικής. 
