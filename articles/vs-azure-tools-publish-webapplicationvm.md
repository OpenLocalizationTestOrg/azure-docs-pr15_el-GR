<properties
   pageTitle="Δημοσίευση WebApplicationVM | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αναπτύξετε μια εφαρμογή web για μια εικονική μηχανή. Αυτή η δέσμη ενεργειών δημιουργεί τους απαιτούμενους πόρους Azure τη συνδρομή σας Εάν δεν υπάρχουν."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-webapplicationvm-windows-powershell-script"></a>Δημοσίευση-WebApplicationVM (δέσμη ενεργειών του Windows PowerShell)

Ανάπτυξη μιας εφαρμογής web με ένα εικονικό μηχάνημα. Η δέσμη ενεργειών δημιουργεί τους απαιτούμενους πόρους στο Azure τη συνδρομή σας Εάν δεν υπάρχουν.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Ρύθμιση παραμέτρων

Η διαδρομή προς το αρχείο ρύθμισης παραμέτρων JSON που περιγράφει τις λεπτομέρειες της ανάπτυξης.

|Ψευδώνυμα|Κανένας|
|---|---|
|Απαιτείται;|TRUE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|Κανένας|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

### <a name="subscriptionname"></a>SubscriptionName

Το όνομα της συνδρομής Azure στην οποία θέλετε να δημιουργήσετε την εικονική μηχανή.

|Ψευδώνυμα|Κανένας|
|---|---|
|Απαιτείται;|FALSE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|Χρησιμοποιεί την πρώτη εγγραφή στο αρχείο συνδρομή|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

### <a name="webdeploypackage"></a>WebDeployPackage

Η διαδρομή προς το πακέτο ανάπτυξης web να δημοσιεύσετε την εικονική μηχανή. Μπορείτε να δημιουργήσετε αυτό το πακέτο χρησιμοποιώντας τον Οδηγό δημοσίευσης Web στο Visual Studio. Ανατρέξτε στο θέμα [ΔΙΑΔΙΚΑΣΙΕΣ: Δημιουργία πακέτου ανάπτυξης Web στο Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

|Ψευδώνυμα|Κανένας|
|---|---|
|Απαιτείται;|FALSE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|Κανένας|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

### <a name="allowuntrusted"></a>AllowUntrusted

Εάν είναι true, επιτρέπει τη χρήση των πιστοποιητικών που δεν έχετε συνδεθεί με μια αξιόπιστη αρχή ρίζας.

|Ψευδώνυμα|Κανένας|
|---|---|
|Απαιτείται;|FALSE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|FALSE|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

### <a name="vmpassword"></a>VMPassword

Τα διαπιστευτήρια για το λογαριασμό εικονική μηχανή. Παράδειγμα: - VMPassword @{Name = "Διαχειριστής" Κωδικός πρόσβασης = "κωδικός πρόσβασης"}

|Ψευδώνυμα|Κανένας|
|---|---|
|Απαιτείται;|FALSE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|Κανένας|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

### <a name="databaseserverpassword"></a>DatabaseServerPassword

Τα διαπιστευτήρια για τη βάση δεδομένων SQL στο Azure. Παράδειγμα: - DatabaseServerPassword @{Name = "Διαχειριστής" Κωδικός πρόσβασης = "κωδικός πρόσβασης"}

|Ψευδώνυμα|Κανένας|
|---|---|
|Απαιτείται;|FALSE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|Κανένας|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Εάν είναι αληθές, εκτύπωση των μηνυμάτων από τη δέσμη ενεργειών στη ροή εξόδου.

|Ψευδώνυμα|Κανένας|
|---|---|
|Απαιτείται;|FALSE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|FALSE|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

## <a name="remarks"></a>Παρατηρήσεις

Για μια πλήρη επεξήγηση του πώς μπορείτε να χρησιμοποιήσετε τη δέσμη ενεργειών για τη δημιουργία περιβάλλοντα προγραμματιστές και τις επιλογές, ανατρέξτε στο θέμα [Χρήση Windows δεσμών ενεργειών του PowerShell για δημοσίευση για να αποκλίσεις και περιβάλλον δοκιμής](vs-azure-tools-publishing-using-powershell-scripts.md).

Το αρχείο ρύθμισης παραμέτρων JSON καθορίζει τις λεπτομέρειες των στοιχείων που υπάρχουν για ανάπτυξη. Περιλαμβάνει τις πληροφορίες που καθορίσατε όταν δημιουργήσατε το έργο, όπως το όνομα, ομάδα συσχέτισης, ειδώλου VHD και μεγέθους η εικονική μηχανή. Επίσης περιλαμβάνει τα τελικά σημεία στον υπολογιστή εικονικές, τις βάσεις δεδομένων με την παροχή, εάν υπάρχει, και τις παραμέτρους ανάπτυξης στο web. Ο ακόλουθος κώδικας εμφανίζει ένα παράδειγμα αρχείου ρύθμισης παραμέτρων JSON:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Μπορείτε να επεξεργαστείτε το αρχείο ρύθμισης παραμέτρων JSON για να αλλάξετε ποια παρέχεται. Απαιτούνται μια εικονική μηχανή και μια υπηρεσία cloud, αλλά είναι προαιρετική ενότητα βάση δεδομένων.
