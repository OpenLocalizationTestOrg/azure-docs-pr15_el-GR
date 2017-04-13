<properties
   pageTitle="Προσαρμοσμένες δέσμες ενεργειών σε VM Linux | Microsoft Azure"
   description="Αυτοματοποίηση εργασιών ρύθμισης παραμέτρων Εικονική Linux χρησιμοποιώντας την επέκταση προσαρμοσμένη δέσμη ενεργειών"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="nepeters"/>

# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Χρησιμοποιώντας την επέκταση Azure προσαρμοσμένη δέσμη ενεργειών με Linux εικονικές μηχανές

Η επέκταση προσαρμοσμένη δέσμη ενεργειών λήψεις και εκτελεί δέσμες ενεργειών σε Azure εικονικές μηχανές. Αυτή η επέκταση είναι χρήσιμη για ρύθμιση παραμέτρων ανάπτυξης καταχώρησης, εγκατάσταση λογισμικού ή άλλες ρύθμισης παραμέτρων / διαχείρισης εργασιών. Δέσμες ενεργειών μπορεί να κάνει λήψη του από το Azure χώρου αποθήκευσης ή σε άλλη προσβάσιμη θέση στο internet ή να που παρέχεται για την επέκταση χρόνος εκτέλεσης. Την επέκταση προσαρμοσμένης δέσμης ενεργειών ενοποιείται με το διαχειριστή πόρων Azure πρότυπα και μπορεί να εκτελεστεί επίσης με το Azure CLI, PowerShell, Azure πύλη ή το Azure εικονική μηχανή REST API.

Αυτό το έγγραφο "Λεπτομέρειες" πώς μπορείτε να χρησιμοποιήσετε την επέκταση προσαρμοσμένη δέσμη ενεργειών από το Azure CLI και ενός προτύπου για τη διαχείριση πόρων Azure, και επίσης λεπτομέρειες βήματα αντιμετώπισης προβλημάτων στα συστήματα Linux.

## <a name="extension-configuration"></a>Ρύθμιση παραμέτρων επέκτασης

Η ρύθμιση παραμέτρων προσαρμοσμένη δέσμη ενεργειών επέκταση Καθορίζει στοιχεία, όπως θέση δέσμης ενεργειών και την εντολή για να εκτελεστεί. Αυτή η ρύθμιση παραμέτρων μπορούν να αποθηκευτούν σε αρχεία ρύθμισης παραμέτρων, που καθορίζεται στη γραμμή εντολών ή σε ένα πρότυπο από διαχειριστή πόρων Azure. Ευαίσθητα δεδομένα μπορούν να αποθηκευτούν σε ένα προστατευμένο παραμέτρους, οι οποίες είναι κρυπτογραφημένες και αποκρυπτογράφηση μόνο μέσα η εικονική μηχανή. Η ρύθμιση παραμέτρων προστατευμένο είναι χρήσιμη όταν η εντολή εκτέλεσης περιλαμβάνει απόρρητο όπως έναν κωδικό πρόσβασης.

### <a name="public-configuration"></a>Δημόσια ρύθμισης παραμέτρων

Σχήμα:

- **commandToExecute**: (υποχρεωτικό, συμβολοσειράς) της δέσμης ενεργειών σημείο καταχώρησης για την εκτέλεση
- **fileUris**: (προαιρετικό, συμβολοσειρά πίνακας) τις διευθύνσεις URL για τα αρχεία για λήψη.
- **χρονική σήμανση** (προαιρετικά, ακέραιος) Χρησιμοποιήστε αυτό το πεδίο μόνο για να ενεργοποιήσετε μια επανεκτέλεση της δέσμης ενεργειών, αλλάζοντας την τιμή αυτού του πεδίου.

```none
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Προστατευμένο ρύθμισης παραμέτρων

Σχήμα:

- **commandToExecute**: (προαιρετικό, συμβολοσειρά) η δέσμη ενεργειών σημείο καταχώρησης για την εκτέλεση. Χρησιμοποιήστε αυτό το πεδίο εάν η εντολή σας περιέχει απόρρητο όπως κωδικούς πρόσβασης.
- **storageAccountName**: (προαιρετικό, συμβολοσειρά) το όνομα του λογαριασμού χώρου αποθήκευσης. Εάν καθορίσετε χώρο αποθήκευσης διαπιστευτηρίων, όλα fileUris πρέπει να είναι διευθύνσεις URL για αντικείμενα BLOB Azure.
- **storageAccountKey**: (προαιρετικό, συμβολοσειρά) το πλήκτρο πρόσβασης του λογαριασμού χώρου αποθήκευσης.


```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI

Όταν χρησιμοποιείτε το CLI Azure για να εκτελέσετε την επέκταση προσαρμοσμένη δέσμη ενεργειών, δημιουργήστε ένα αρχείο ρύθμισης παραμέτρων ή αρχεία τα οποία περιέχουν τουλάχιστον το uri αρχείου και την εντολή εκτέλεσης δέσμης ενεργειών.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /scirpt-config.json
```

Προαιρετικά, η εντολή μπορεί να εκτελεστεί χρησιμοποιώντας το `--public-config` και `--private-config` επιλογή, η οποία επιτρέπει τη ρύθμιση παραμέτρων να έχει καθοριστεί κατά την εκτέλεση και χωρίς ένα αρχείο ρύθμισης παραμέτρων ξεχωριστά.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

### <a name="azure-cli-examples"></a>Παραδείγματα Azure CLI

**Παράδειγμα 1** - δημόσια ρύθμισης παραμέτρων με το αρχείο δέσμης ενεργειών.

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
  "commandToExecute": "./hello.sh"
}
```

Azure CLI εντολή:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Παράδειγμα 2** - δημόσια ρύθμισης παραμέτρων με κανένα αρχείο δέσμης ενεργειών.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI εντολή:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Παράδειγμα 3** - αρχείο ρύθμισης παραμέτρων δημόσια χρησιμοποιείται για να καθορίσετε το αρχείο δέσμης ενεργειών URI και ένα αρχείο ρύθμισης παραμέτρων προστατευμένο χρησιμοποιείται για να καθορίσετε την εντολή για να εκτελεστεί.

Αρχείο ρύθμισης παραμέτρων δημόσια:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
}
```

Αρχείο ρύθμισης παραμέτρων προστατευμένο:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI εντολή:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path ./public.json --private-config-path ./protected.json
```

## <a name="resource-manager-template"></a>Το πρότυπο διαχείρισης πόρων

Η επέκταση Azure προσαρμοσμένη δέσμη ενεργειών μπορούν να εκτελεστούν κατά το χρόνο ανάπτυξης εικονική μηχανή χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων. Για να το κάνετε αυτό, προσθέστε κανονικά μορφοποιημένο JSON στο πρότυπο ανάπτυξης.

### <a name="resource-manager-examples"></a>Παραδείγματα από διαχειριστή πόρων

**Παράδειγμα 1** - δημόσια ρύθμισης παραμέτρων.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Παράδειγμα 2** - εντολή Εκτέλεση στο προστατευμένο ρύθμισης παραμέτρων.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Ανατρέξτε στο θέμα το .net πυρήνα μουσικής επίδειξη χώρου αποθήκευσης για μια πλήρη παράδειγμα - [Επίδειξη Store μουσικής](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

Όταν εκτελείται την επέκταση προσαρμοσμένη δέσμη ενεργειών, η δέσμη ενεργειών δημιουργείται ή λήψη σε έναν κατάλογο παρόμοιο με το παρακάτω παράδειγμα. Η εντολή έξοδος αποθηκεύεται επίσης σε αυτόν τον κατάλογο στο `stdout` και `stderr` αρχείου.

```none
/var/lib/azure/custom-script/download/0/
```

Η επέκταση δέσμης ενεργειών Azure δημιουργεί ένα αρχείο καταγραφής, το οποίο μπορείτε να βρείτε εδώ.

```none
/var/log/azure/customscript/handler.log
```

Η κατάσταση εκτέλεσης της επέκτασης προσαρμοσμένη δέσμη ενεργειών μπορεί να ανακτηθεί επίσης με το Azure CLI.

```none
azure vm extension get <resource-group> <vm-name>
```

Το αποτέλεσμα μοιάζει με το ακόλουθο κείμενο:

```none
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Επόμενα βήματα

Για πληροφορίες σχετικά με άλλες επεκτάσεις Εικονική δέσμης ενεργειών, ανατρέξτε στο θέμα [Επισκόπηση Azure επέκταση δέσμη ενεργειών για Linux](./virtual-machines-linux-extensions-features.md).