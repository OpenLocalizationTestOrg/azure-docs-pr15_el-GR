<properties
    pageTitle="Κατάσταση χειρισμού στα πρότυπα διαχείρισης πόρων | Microsoft Azure"
    description="Εμφανίζει προτεινόμενα προσεγγίσεις για τη χρήση σύνθετων αντικειμένων για κοινή χρήση δεδομένων κατάστασης με πρότυπα διαχείρισης πόρων Azure και συνδεδεμένων πρότυπα"
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>Κατάσταση στα πρότυπα διαχείρισης πόρων Azure κοινής χρήσης

Αυτό το θέμα δείχνει βέλτιστες πρακτικές για τη διαχείριση και την κατάσταση μέσα σε πρότυπα κοινής χρήσης. Οι παράμετροι και μεταβλητές που εμφανίζονται σε αυτό το θέμα είναι παραδείγματα του τύπου των αντικειμένων που μπορείτε να ορίσετε για την οργάνωση εύκολα τις απαιτήσεις σας ανάπτυξης. Από αυτά τα παραδείγματα, μπορείτε να εφαρμόσετε τις δικές σας αντικείμενα με τιμές ιδιοτήτων που έχει νόημα για το περιβάλλον σας.

Αυτό το θέμα αποτελεί τμήμα ενός μεγαλύτερου λευκή βίβλος. Για να διαβάσετε το πλήρες χαρτιού, κάντε λήψη του [World κλάσης πόρου Διαχείριση προτύπων ζητήματα και δοκιμασμένη πρακτικές](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).


## <a name="provide-standard-configuration-settings"></a>Παροχή τυπική ρύθμισης παραμέτρων

Αντί να προσφέρουν ένα πρότυπο που παρέχει ευελιξία συνολικό και αμέτρητες παραλλαγές, ένα κοινό μοτίβο είναι να δώσετε μια επιλογή γνωστές παραμέτρους. Στην πραγματικότητα, οι χρήστες μπορούν να επιλέξουν βασικά μεγέθη t-shirt όπως φίλτρου, μικρό, μεσαίο και μεγάλο. Άλλα παραδείγματα μεγέθη t-shirt είναι προσφορές προϊόντων, όπως η Κοινότητα edition ή έκδοση enterprise. Σε άλλες περιπτώσεις, ίσως ρυθμίσεις παραμέτρων φόρτο εργασίας από μια τεχνολογία – όπως μείωση χάρτη ή χωρίς sql.

Με τα σύνθετα αντικείμενα, μπορείτε να δημιουργήσετε μεταβλητές που περιέχουν συλλογές των δεδομένων, ορισμένες φορές γνωστή ως "ομάδες ιδιοτήτων" και να χρησιμοποιήσετε αυτά τα δεδομένα για να καθοδηγήσετε τη δήλωση πόρων στο πρότυπό σας. Αυτή η προσέγγιση παρέχει καλή, γνωστή ρυθμίσεις παραμέτρων διαφορετικού μεγέθους που είναι προ-διαμορφωμένες για τους πελάτες σας. Χωρίς γνωστά διαμορφώσεις, οι χρήστες του προτύπου πρέπει να προσδιορίσετε σύμπλεγμα αλλαγής μεγέθους στη δική τους συντελεστής σε περιορισμούς πόρων πλατφόρμα και μαθηματική πράξη για τον προσδιορισμό του διαμερισμάτων που προκύπτει από λογαριασμούς χώρου αποθήκευσης και άλλοι πόροι (λόγω σύμπλεγμα ορίων μεγέθους και πόρων). Εκτός από την πραγματοποίηση μια καλύτερη εμπειρία για τον πελάτη, είναι πιο εύκολο να μερικά γνωστά ρυθμίσεις παραμέτρων υποστήριξης και να σας βοηθήσει να κάνουν υψηλότερο επίπεδο πυκνότητας.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να ορίσετε μεταβλητές που περιέχουν σύνθετα αντικείμενα για την εμφάνιση των συλλογών δεδομένων. Οι συλλογές Ορισμός τιμών που χρησιμοποιούνται για εικονική μηχανή μέγεθος, οι ρυθμίσεις δικτύου, ρυθμίσεις του λειτουργικού συστήματος και ρυθμίσεις διαθεσιμότητας.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Παρατηρήστε ότι η μεταβλητή **tshirtSize** συνδέει το μέγεθος t-shirt που παρέχεται μέσω μιας παραμέτρου (**μικρό**, **Μεσαίο**, **μεγάλο**) για να το κείμενο **tshirtSize**. Μπορείτε να χρησιμοποιήσετε αυτήν τη μεταβλητή για να ανακτήσετε τη μεταβλητή συσχετισμένο αντικείμενο σύνθετη για αυτό το μέγεθος t-shirt.

Στη συνέχεια, μπορείτε να αναφέρετε αυτές τις μεταβλητές αργότερα στο πρότυπο. Η δυνατότητα αναφοράς με την ονομασία μεταβλητές και τις ιδιότητές τους απλοποιεί τη σύνταξη του προτύπου και διευκολύνει την κατανόηση περιβάλλοντος. Το παρακάτω παράδειγμα καθορίζει έναν πόρο για την ανάπτυξη, χρησιμοποιώντας τα αντικείμενα εμφανίζονται προηγουμένως για τον ορισμό τιμών. Για παράδειγμα, το μέγεθος Εικονική ορίζεται από την ανάκτηση της τιμής για `variables('tshirtSize').vmSize` κατά την τιμή για το μέγεθος του δίσκου ανάκτηση από `variables('tshirtSize').diskSize`. Επιπλέον, το URI για ένα συνδεδεμένο πρότυπο έχει οριστεί η ρύθμιση της τιμής για `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>Κατάσταση πέρασμα σε ένα πρότυπο

Μπορείτε να κάνετε κοινή χρήση κατάσταση σε ένα πρότυπο στις παραμέτρους που παρέχετε απευθείας κατά την ανάπτυξη.

Ο παρακάτω πίνακας παραθέτει που χρησιμοποιείται συχνά παραμέτρους σε πρότυπα.

Όνομα | Τιμή | Περιγραφή
---- | ----- | -----------
θέση    | Συμβολοσειρά από μια λίστα περιορισμένη Azure περιοχών   | Η θέση όπου αναπτύσσονται τους πόρους.
storageAccountNamePrefix    | Συμβολοσειρά    | Μοναδικό όνομα DNS για το λογαριασμό χώρου αποθήκευσης όπου τοποθετούνται δίσκων την εικονική Μηχανή
όνομα_τομέα  | Συμβολοσειρά    | Το όνομα τομέα από τη δυνατότητα δημόσιας πρόσβασης jumpbox Εικονική με τη μορφή: **{όνομα_τομέα}. { θέση}.cloudapp.com** για παράδειγμα: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | Συμβολοσειρά    | Όνομα χρήστη για το ΣΠΣ
adminPassword   | Συμβολοσειρά    | Κωδικός πρόσβασης για το ΣΠΣ
tshirtSize  | Συμβολοσειρά από μια λίστα με περιορισμένη προσφέρεται μεγέθη t-shirt   | Το μέγεθος της μονάδας καθορισμένη κλίμακα προμήθειας. Για παράδειγμα, "Μικρό", "Μεσαία", "Μεγάλο"
virtualNetworkName  | Συμβολοσειρά    | Το όνομα του εικονικού δικτύου στο οποίο ο καταναλωτής θέλει να χρησιμοποιήσετε.
enableJumpbox   | Συμβολοσειρά από μια λίστα περιορισμένη (ενεργοποίηση/απενεργοποίηση) | Η παράμετρος που προσδιορίζει εάν θέλετε να ενεργοποιήσετε ένα jumpbox για το περιβάλλον. Τιμές: "έχει ενεργοποιηθεί", "απενεργοποιημένη"

Η παράμετρος **tshirtSize** χρησιμοποιείται στην προηγούμενη ενότητα ορίζεται ως εξής:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>Μεταβίβαση κατάσταση σε συνδεδεμένα προτύπων

Κατά τη σύνδεση σε συνδεδεμένα πρότυπα, μπορείτε συχνά Χρησιμοποιήστε συνδυασμό στατική και που δημιουργούνται μεταβλητές.

### <a name="static-variables"></a>Στατική μεταβλητές

Στατική μεταβλητές συχνά χρησιμοποιούνται για την παροχή βάσης τιμές, όπως διευθύνσεις URL που χρησιμοποιούνται σε όλο το πρότυπο.

Στο το παρακάτω απόσπασμα πρότυπο, `templateBaseUrl` Καθορίζει τη θέση ρίζας για το πρότυπο σε GitHub. Στην επόμενη γραμμή δημιουργεί μια νέα μεταβλητή `sharedTemplateUrl` που συνδέει τη βασική διεύθυνση URL με το γνωστό όνομα του προτύπου κοινόχρηστους πόρους. Κάτω από αυτήν τη γραμμή, μια μεταβλητή αντικειμένου σύνθετες χρησιμοποιείται για να αποθηκεύσετε ένα μέγεθος t-shirt, όπου τη βασική διεύθυνση URL είναι συνδέονται στη θέση του προτύπου γνωστές ρυθμίσεις και να είναι αποθηκευμένα σε το `vmTemplate` την ιδιότητα.

Το πλεονέκτημα αυτής της προσέγγισης είναι ότι εάν αλλάξει τη θέση του προτύπου, μόνο πρέπει να αλλάξετε τη στατική μεταβλητή από ένα σημείο, η οποία μεταβιβάζει σε όλο το συνδεδεμένο πρότυπα.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Μεταβλητές που δημιουργήθηκε

Εκτός από στατική μεταβλητές, δημιουργούνται δυναμικά πολλές μεταβλητές. Αυτή η ενότητα προσδιορίζει ορισμένοι από τους συνήθεις τύπους που δημιουργήθηκε μεταβλητών.

#### <a name="tshirtsize"></a>tshirtSize

Είστε εξοικειωμένοι με αυτήν τη μεταβλητή που δημιουργήθηκε από το παραπάνω παράδειγμα.

#### <a name="networksettings"></a>networkSettings

Σε δυναμικότητα, δυνατότητα ή Τέλος-περιορισμένο λύση πρότυπο, τα πρότυπα που είναι συνδεδεμένες δημιουργήσει συνήθως τους πόρους που υπάρχουν σε ένα δίκτυο. Ένα απλό προσέγγιση είναι να χρησιμοποιήσετε μια σύνθετη αντικειμένου για την αποθήκευση ρυθμίσεων δικτύου και αυτά μεταβιβάζουν συνδεδεμένων πρότυπα.

Παράδειγμα ενημέρωση σχετικά με τις ρυθμίσεις του δικτύου μπορεί να εμφανίζεται κάτω από.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

Πόροι που δημιουργήθηκαν στο συνδεδεμένο πρότυπα τοποθετούνται συχνά σε ένα σύνολο διαθεσιμότητα. Στο παρακάτω παράδειγμα, το όνομα του συνόλου διαθεσιμότητα έχει καθοριστεί και επίσης καταμετρήσετε τον τομέα σφαλμάτων και τομέα update για να χρησιμοποιήσετε.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Εάν χρειάζεστε πολλές διαθεσιμότητα σύνολα (για παράδειγμα, μία για κύρια κόμβοι) και ένα άλλο για τους κόμβους δεδομένων, μπορείτε να χρησιμοποιήσετε ένα όνομα ως πρόθεμα, καθορίστε πολλά σύνολα διαθεσιμότητα ή ακολουθήστε το μοντέλο φαίνεται παραπάνω για τη δημιουργία μιας μεταβλητής για ένα συγκεκριμένο μέγεθος t-shirt.

#### <a name="storagesettings"></a>storageSettings

Λεπτομέρειες χώρου αποθήκευσης είναι συχνά κοινόχρηστα με συνδεδεμένα πρότυπα. Στο παρακάτω παράδειγμα, ένα αντικείμενο *storageSettings* παρέχει λεπτομέρειες σχετικά με τα ονόματα λογαριασμού και κοντέινερ χώρου αποθήκευσης.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

Με το συνδεδεμένο πρότυπα, ίσως χρειαστεί να μεταβιβάζουν ρυθμίσεις του λειτουργικού συστήματος για διάφορους τύπους κόμβους σε διαφορετική γνωστές ρυθμίσεις τύπους. Μια σύνθετη αντικείμενο είναι ένας εύκολος τρόπος για την αποθήκευση και κοινή χρήση πληροφοριών λειτουργικό σύστημα και διευκολύνει επίσης να υποστηρίζει πολλές επιλογές λειτουργικό σύστημα για ανάπτυξη.

Το παρακάτω παράδειγμα εμφανίζει ένα αντικείμενο για *osSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

Μια μεταβλητή που έχει δημιουργηθεί, *machineSettings* είναι μια σύνθετη αντικείμενο που περιέχουν ένα συνδυασμό των μεταβλητών πυρήνα για να δημιουργήσετε μια Εικονική. Οι μεταβλητές περιλαμβάνουν διαχειριστή όνομα χρήστη και τον κωδικό πρόσβασης, ένα πρόθεμα για τα ονόματα Εικονική και το λειτουργικό σύστημα αναφοράς εικόνας.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Σημείωση που *osImageReference* ανακτά τις τιμές από τη μεταβλητή *osSettings* οριστεί στο κύριο πρότυπο. Αυτό σημαίνει ότι μπορείτε εύκολα να αλλάξετε το λειτουργικό σύστημα για μια Εικονική — εντελώς ή με βάση την προτίμηση ένα πρότυπο προγράμματος-πελάτη.

#### <a name="vmscripts"></a>vmScripts

Το αντικείμενο *vmScripts* περιέχει λεπτομέρειες σχετικά με τις δέσμες ενεργειών για να κάνετε λήψη και να εκτελέσετε σε μια Εικονική παρουσία, συμπεριλαμβανομένων των εκτός και μέσα σε αναφορές. Εκτός οι αναφορές περιλαμβάνουν την υποδομή. Οι αναφορές εσωτερικό περιλαμβάνουν την εγκατεστημένο λογισμικό εγκαταστήσει και τις παραμέτρους.

Μπορείτε να χρησιμοποιήσετε την ιδιότητα *scriptsToDownload* για μια λίστα των δεσμών ενεργειών για να κάνετε λήψη για να την εικονική Μηχανή. Αυτό το αντικείμενο περιέχει επίσης οι αναφορές σε ορίσματα γραμμής εντολών για διαφορετικούς τύπους ενεργειών. Αυτές οι ενέργειες περιλαμβάνουν εκτέλεση η προεπιλεγμένη εγκατάσταση για κάθε μεμονωμένο κόμβο, μια εγκατάσταση που εκτελείται μετά από όλους τους κόμβους αναπτύσσονται και οποιεσδήποτε πρόσθετες δέσμες ενεργειών που μπορεί να είναι συγκεκριμένες σε ένα πρότυπο που δίνεται.

Αυτό το παράδειγμα είναι από ένα πρότυπο που χρησιμοποιείται για την ανάπτυξη MongoDB, που απαιτεί μια κριτής για παράδοση υψηλής διαθεσιμότητας. Το *arbiterNodeInstallCommand* έχει προστεθεί *vmScripts* για να εγκαταστήσετε το κριτής.

Η ενότητα μεταβλητές είναι όπου μπορείτε να βρείτε τις μεταβλητές που ορίζουν το συγκεκριμένο κείμενο για την εκτέλεση της δέσμης ενεργειών με το πρώτο γράμμα κάθε λέξης τιμές.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Επιστροφή κατάστασης από ένα πρότυπο

Όχι μόνο μπορείτε να μεταβιβάσετε δεδομένα σε ένα πρότυπο, μπορείτε επίσης να μοιραστείτε δεδομένων στο πρότυπο κλήσης. Στην ενότητα **εξόδους** προτύπου συνδεδεμένων, μπορείτε να παράσχετε ζεύγη κλειδιού/τιμής που μπορεί να χρησιμοποιηθεί από το πρότυπο προέλευσης.

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο για τη μεταβίβαση στην ιδιωτική διεύθυνση IP που δημιουργούνται σε ένα συνδεδεμένο πρότυπο.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

Μέσα στο κύριο πρότυπο, μπορείτε να χρησιμοποιήσετε αυτά τα δεδομένα με την ακόλουθη σύνταξη:

    "[reference('master-node').outputs.masterip.value]"

Μπορείτε να χρησιμοποιήσετε αυτή την παράσταση στην ενότητα εξόδους ή στην ενότητα πόρους από το κύριο πρότυπο. Δεν μπορείτε να χρησιμοποιήσετε την παράσταση στην ενότητα μεταβλητές, διότι το εξαρτάται από την κατάσταση χρόνου εκτέλεσης. Για να λάβετε αυτήν την τιμή από το κύριο πρότυπο, χρησιμοποιήστε:

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
Για ένα παράδειγμα της χρήσης στην ενότητα εξόδους ενός συνδεδεμένου προτύπου για να επιστρέψετε δίσκων δεδομένων για μια εικονική μηχανή, ανατρέξτε στο θέμα [Δημιουργία πολλών δίσκων δεδομένων για μια εικονική μηχανή](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine).

## <a name="define-authentication-settings-for-virtual-machine"></a>Ορισμός ρυθμίσεις ελέγχου ταυτότητας για εικονική μηχανή

Μπορείτε να χρησιμοποιήσετε το ίδιο μοτίβο εμφανίζεται παραπάνω για ρυθμίσεις παραμέτρων για να καθορίσετε τις ρυθμίσεις ελέγχου ταυτότητας για μια εικονική μηχανή. Μπορείτε να δημιουργήσετε μια παράμετρο για διαβίβαση στον τύπο ελέγχου ταυτότητας.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Μπορείτε να προσθέσετε μεταβλητές για τους τύπους ελέγχου ταυτότητας και μια μεταβλητή για να αποθηκεύσετε τον τύπο που χρησιμοποιείται για αυτήν την ανάπτυξη με βάση την τιμή της παραμέτρου.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Όταν ορίζετε την εικονική μηχανή, μπορείτε να ρυθμίσετε το **osProfile** στη μεταβλητή που δημιουργήσατε.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Επόμενα βήματα
- Για να μάθετε περισσότερα σχετικά με τις ενότητες του προτύπου, ανατρέξτε στο θέμα [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure](resource-group-authoring-templates.md)
- Για να δείτε τις συναρτήσεις που είναι διαθέσιμες μέσα σε ένα πρότυπο, ανατρέξτε στο θέμα [Συναρτήσεις προτύπου για τη διαχείριση πόρων Azure](resource-group-template-functions.md)

