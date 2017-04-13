<properties
   pageTitle="Δημοσίευση-WebApplicationWebSite (δέσμη ενεργειών του Windows PowerShell) | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημοσιεύσετε ένα έργο web σε μια τοποθεσία Web του Azure. Αυτή η δέσμη ενεργειών δημιουργεί τους απαιτούμενους πόρους Azure τη συνδρομή σας Εάν δεν υπάρχουν."
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

# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Δημοσίευση-WebApplicationWebSite (δέσμη ενεργειών του Windows PowerShell)

##<a name="syntax"></a>Σύνταξη

Δημοσιεύει ένα έργο web σε μια τοποθεσία Web του Azure. Η δέσμη ενεργειών δημιουργεί τους απαιτούμενους πόρους στο Azure τη συνδρομή σας Εάν δεν υπάρχουν.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Ρύθμιση παραμέτρων

Η διαδρομή προς το αρχείο ρύθμισης παραμέτρων JSON που περιγράφει τις λεπτομέρειες της ανάπτυξης.

|Παράμετρος|Προεπιλεγμένη τιμή|
|---|---|
|Ψευδώνυμα|Κανένας|
|Απαιτείται;|TRUE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|Κανένας|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

## <a name="subscriptionname"></a>SubscriptionName

Το όνομα της συνδρομής Azure που θέλετε να δημιουργήσετε την τοποθεσία Web στο.

|Παράμετρος|Προεπιλεγμένη τιμή|
|---|---|
|Ψευδώνυμα|Κανένας|
|Απαιτείται;|FALSE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|Κανένας|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

## <a name="webdeploypackage"></a>WebDeployPackage

Η διαδρομή προς το πακέτο ανάπτυξης web για να δημοσιεύσετε την τοποθεσία Web. Μπορείτε να δημιουργήσετε αυτό το πακέτο χρησιμοποιώντας τον Οδηγό δημοσίευσης Web στο Visual Studio. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με τις υπηρεσίες Cloud Azure και ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

|Παράμετρος|Προεπιλεγμένη τιμή|
|---|---|
|Ψευδώνυμα|Κανένας|
|Απαιτείται;|FALSE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|Κανένας|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

## <a name="databaseserverpassword"></a>DatabaseServerPassword

Το όνομα χρήστη και τον κωδικό πρόσβασης για τη βάση δεδομένων SQL στο Azure.

|Παράμετρος|Προεπιλεγμένη τιμή|
|---|---|
|Ψευδώνυμα|Κανένας|
|Απαιτείται;|FALSE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|Κανένας|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Εάν είναι αληθές, εκτύπωση των μηνυμάτων από τη δέσμη ενεργειών στη ροή εξόδου.

|Παράμετρος|Προεπιλεγμένη τιμή|
|---|---|
|Ψευδώνυμα|Κανένας|
|Απαιτείται;|FALSE|
|Θέση|με το όνομα|
|Προεπιλεγμένη τιμή|FALSE|
|Αποδοχή διοχέτευσης εισόδου;|FALSE|
|Αποδοχή χαρακτήρες μπαλαντέρ;|FALSE|

## <a name="remarks"></a>Παρατηρήσεις

Για μια πλήρη επεξήγηση του πώς μπορείτε να χρησιμοποιήσετε τη δέσμη ενεργειών για τη δημιουργία περιβάλλοντα προγραμματιστές και τις επιλογές, ανατρέξτε στο θέμα [Χρήση Windows δεσμών ενεργειών του PowerShell για δημοσίευση για να αποκλίσεις και περιβάλλον δοκιμής](vs-azure-tools-publishing-using-powershell-scripts.md).

Το αρχείο ρύθμισης παραμέτρων JSON καθορίζει τις λεπτομέρειες των στοιχείων που υπάρχουν για ανάπτυξη. Περιλαμβάνει τις πληροφορίες που καθορίσατε όταν δημιουργήσατε το έργο, όπως το όνομα και το όνομα χρήστη για την τοποθεσία Web. Περιλαμβάνει επίσης τη βάση δεδομένων για την παροχή των, εάν υπάρχει. Ο ακόλουθος κώδικας εμφανίζει ένα παράδειγμα αρχείου ρύθμισης παραμέτρων JSON:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Μπορείτε να επεξεργαστείτε το αρχείο ρύθμισης παραμέτρων JSON για να αλλάξετε τι έχει αναπτυχθεί. Απαιτείται μια ενότητα τοποθεσία Web, αλλά είναι προαιρετική ενότητα βάση δεδομένων.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημοσίευση-WebApplicationVM (δέσμη ενεργειών του Windows PowerShell)](vs-azure-tools-publish-webapplicationvm.md)
