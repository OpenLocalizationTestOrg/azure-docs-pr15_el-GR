<properties
   pageTitle="Αυτοματοποίηση πόρους στις λύσεις OMS | Microsoft Azure"
   description="Λύσεις σε OMS συνήθως θα περιλαμβάνουν runbooks στο Azure αυτοματισμού για την αυτοματοποίηση διαδικασιών, όπως τη συλλογή και επεξεργασία δεδομένων παρακολούθησης.  Σε αυτό το άρθρο περιγράφει τον τρόπο για να συμπεριλάβετε runbooks και τους σχετικούς πόρους σε μια λύση."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="bwren" />

# <a name="automation-resources-in-oms-solutions-preview"></a>Αυτοματοποίηση πόρων σε OMS λύσεις (έκδοση Preview)

>[AZURE.NOTE]Αυτό είναι προκαταρκτικού τεκμηρίωση για τη δημιουργία λύσεις διαχείρισης σε OMS που βρίσκονται τη συγκεκριμένη στιγμή σε προεπισκόπηση. Οποιαδήποτε διάταξη που περιγράφεται παρακάτω μπορεί να αλλάξουν.   

[Διαχείριση λύσεων στο OMS](operations-management-suite-solutions.md) συνήθως θα περιλαμβάνει runbooks στο Azure αυτοματισμού για την αυτοματοποίηση διαδικασιών, όπως τη συλλογή και επεξεργασία δεδομένων παρακολούθησης.  Εκτός από την runbooks, λογαριασμοί αυτοματισμού περιλαμβάνει πόρους όπως μεταβλητές και τα χρονοδιαγράμματα που υποστηρίζουν το runbooks χρησιμοποιείται στη λύση.  Σε αυτό το άρθρο περιγράφει τον τρόπο για να συμπεριλάβετε runbooks και τους σχετικούς πόρους σε μια λύση.

>[AZURE.NOTE]Χρησιμοποιήστε τα δείγματα σε αυτό το άρθρο παραμέτρους και μεταβλητές που απαιτείται ή κοινά και για λύσεις διαχείρισης και περιγράφεται στη [Δημιουργία λύσεις διαχείρισης στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Σε αυτό το άρθρο προϋποθέτει ότι είστε ήδη εξοικειωμένοι με τον τρόπο για να [δημιουργήσετε μια λύση διαχείρισης](operations-management-suite-solutions-creating.md) και τη δομή ενός αρχείου λύσης.

## <a name="samples"></a>Δείγματα
Μπορείτε να λάβετε δείγματα προτύπων για τη διαχείριση πόρων για πόρους αυτοματισμού από τα [πρότυπα γρήγορη έναρξη στο GitHub](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates).

## <a name="automation-account"></a>Αυτοματοποίηση λογαριασμού
Όλους τους πόρους αυτοματισμού Azure περιέχονται σε ένα [λογαριασμό αυτοματισμού](../automation/automation-security-overview.md#automation-account-overview).  Όπως περιγράφεται στο [χώρο εργασίας OMS και αυτοματισμού λογαριασμού](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) στο λογαριασμό αυτοματισμού δεν περιλαμβάνεται στο της λύσης διαχείρισης αλλά πρέπει να υπάρχει πριν από την εγκατάσταση της λύσης.  Εάν δεν είναι διαθέσιμη, στη συνέχεια, την εγκατάσταση της λύσης θα αποτύχει.

Είναι το όνομα του λογαριασμού τους αυτοματισμού στο όνομα κάθε πόρο αυτοματισμού.  Αυτό γίνεται στη λύση με την παράμετρο **όνομα λογαριασμού** , όπως στο ακόλουθο παράδειγμα ενός πόρου runbook.
    
    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
[Azure αυτοματισμού runbook](../automation/automation-runbook-types.md) πόροι έχουν ένα τύπο **Microsoft.Automation/automationAccounts/runbooks** και τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| runbookType | Καθορίζει τους τύπους του runbook. <br><br> Δέσμη ενεργειών - δέσμη ενεργειών του PowerShell <br>PowerShell - PowerShell ροής εργασίας <br> GraphPowerShell - runbook δέσμης ενεργειών του PowerShell γραφικών <br> GraphPowerShellWorkflow - runbook γραφικών PowerShell ροής εργασίας   |
| logProgress | Καθορίζει αν θα πρέπει να δημιουργηθεί [εγγραφές προόδου](../automation/automation-runbook-output-and-messages.md) για runbook. |
| logVerbose  | Καθορίζει αν θα πρέπει να δημιουργηθεί [λεπτομερές εγγραφές](../automation/automation-runbook-output-and-messages.md) για runbook. |
| Περιγραφή | Προαιρετική περιγραφή για το runbook. |
| publishContentLink | Καθορίζει το περιεχόμενο του runbook. <br><br>URI - Uri στο περιεχόμενο του runbook.  Αυτό θα είναι ένα αρχείο .ps1 για runbooks PowerShell και δέσμης ενεργειών και ένα αρχείο εξαγόμενα γραφικών runbook για ένα γράφημα runbook.  <br> έκδοση - έκδοση του runbook για τη δική σας παρακολούθηση. |

Παρακάτω είναι ένα παράδειγμα ενός πόρου runbook.  Σε αυτήν την περίπτωση, ανακτά runbook από τη [Συλλογή του PowerShell](https://www.powershellgallery.com).

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Ξεκινώντας μια runbook
Υπάρχουν δύο μέθοδοι για να ξεκινήσετε μια runbook σε μια λύση διαχείρισης.

- Για να ξεκινήσετε runbook όταν η λύση είναι εγκατεστημένο, δημιουργήστε έναν [πόρο εργασίας](#automation-jobs) όπως περιγράφεται παρακάτω.
- Για να ξεκινήσετε runbook σε ένα χρονοδιάγραμμα, δημιουργήστε έναν [πόρο χρονοδιάγραμμα](#schedules) όπως περιγράφεται παρακάτω. 


## <a name="automation-jobs"></a>Αυτοματοποίηση εργασιών
Για να ξεκινήσετε μια runbook κατά την εγκατάσταση της λύσης διαχείρισης, μπορείτε να δημιουργήσετε έναν πόρο **εργασίας** .  Πόροι εργασίας έχουν ένα τύπο **Microsoft.Automation/automationAccounts/jobs** και τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| runbook    | Μόνο **όνομα** οντότητα με το όνομα του runbook.  |
| παράμετροι | Οντότητα για κάθε παράμετρο που απαιτούνται από runbook. |


Η εργασία περιλαμβάνει το όνομα του runbook και τις τιμές παραμέτρων που θα σταλεί σε runbook.  Η εργασία πρέπει να [εξαρτώνται από](operations-management-suite-solutions-creating.md#resources) runbook που να ξεκινά από runbook πρέπει να δημιουργηθεί πριν από την εργασία.  Μπορείτε επίσης να δημιουργήσετε εξαρτήσεις σε άλλα έργα για runbooks που πρέπει να ολοκληρωθούν πριν από το τρέχον αρχείο.

Το όνομα του πόρου εργασίας πρέπει να περιέχει ένα GUID που εκχωρείται συνήθως από μια παράμετρο.  Μπορείτε να διαβάσετε περισσότερα σχετικά με τις παραμέτρους GUID στη [δημιουργία λύσεων στην οικογένεια προγραμμάτων διαχείρισης λειτουργίες (OMS)](operations-management-suite-solutions-creating.md#parameters).  

Ακολουθεί ένα παράδειγμα ενός πόρου εργασίας που ξεκινά μια runbook κατά την εγκατάσταση της λύσης διαχείρισης.  Ένα άλλο runbooks πρέπει να ολοκληρωθούν πριν από αυτό ένα ξεκινά, ώστε να έχει εξαρτήσεις στις εργασίες για το συγκεκριμένο runbook.  Τις λεπτομέρειες για το έργο παρέχονται μέσω παραμέτρους και μεταβλητές αντί να καθοριστεί απευθείας.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Πιστοποιητικά
[Αυτοματοποίηση Azure πιστοποιητικά](../automation/automation-certificates.md) έχουν ένα τύπο **Microsoft.Automation/automationAccounts/certificates** και τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| base64Value   | Τιμή βάσης 64 για το πιστοποιητικό. |
| Αποτύπωση    | Αποτύπωση για το πιστοποιητικό. |

Παρακάτω είναι ένα παράδειγμα ενός πόρου πιστοποιητικού.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Τα διαπιστευτήρια
[Αυτοματοποίηση Azure διαπιστευτήρια](../automation/automation-credentials.md) έχουν ένα τύπο **Microsoft.Automation/automationAccounts/credentials** και τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| όνομα χρήστη   | Το όνομα χρήστη για τα διαπιστευτήρια. |
| κωδικός πρόσβασης   | Ο κωδικός πρόσβασης για τα διαπιστευτήρια. |

Παρακάτω είναι ένα παράδειγμα ενός πόρου διαπιστευτηρίων.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Χρονοδιαγράμματα
[Αυτοματοποίηση Azure χρονοδιαγράμματα](../automation/automation-schedules.md) έχουν ένα τύπο **Microsoft.Automation/automationAccounts/schedules** και τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| Περιγραφή | Προαιρετική περιγραφή για το χρονοδιάγραμμα. |
| ώρα έναρξης   | Καθορίζει την ώρα έναρξης του χρονοδιαγράμματος ως αντικείμενο ημερομηνίας/ώρας. Μια συμβολοσειρά που μπορεί να παρέχεται εάν μπορεί να μετατραπεί σε μια έγκυρη ημερομηνία/ώρα. |
| isEnabled   | Καθορίζει εάν είναι ενεργοποιημένη στο χρονοδιάγραμμα. |
| χρονικό διάστημα    | Ο τύπος του χρονικού διαστήματος για το χρονοδιάγραμμα.<br><br>ημέρα<br>ώρα |
| συχνότητα   | Συχνότητα που το χρονοδιάγραμμα θα πρέπει να ενεργοποιούνται σε αριθμό των ημερών ή ωρών. |

Παρακάτω είναι ένα παράδειγμα ενός πόρου χρονοδιάγραμμα.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Μεταβλητές
[Μεταβλητές αυτοματισμού Azure](../automation/automation-variables.md) έχουν ένα τύπο **Microsoft.Automation/automationAccounts/variables** και τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| Περιγραφή | Προαιρετική περιγραφή για τη μεταβλητή. |
| isEncrypted | Καθορίζει εάν η μεταβλητή θα πρέπει να είναι κρυπτογραφημένο. |
| Τύπος        | Τύπος δεδομένων για τη μεταβλητή. |
| τιμή       | Τιμή για τη μεταβλητή. |

Παρακάτω είναι ένα παράδειγμα της μεταβλητής πόρου.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Λειτουργικές μονάδες
Τη λύση διαχείρισης δεν χρειάζεται να ορίσετε [καθολικού λειτουργικές μονάδες](../automation/automation-integration-modules.md) χρησιμοποιούνται από το runbooks, επειδή αυτά θα είναι πάντα διαθέσιμη.  Πρέπει να συμπεριλάβετε έναν πόρο για τυχόν άλλες λειτουργική μονάδα χρησιμοποιείται από το runbooks και runbook θα πρέπει να εξαρτώνται από τον πόρο λειτουργικής μονάδας για να βεβαιωθείτε ότι δημιουργηθεί πριν από την runbook.

[Ενοποίηση λειτουργικές μονάδες](../automation/automation-integration-modules.md) έχουν ένα τύπο **Microsoft.Automation/automationAccounts/modules** και τις ιδιότητες στον παρακάτω πίνακα.

| Ιδιότητα | Περιγραφή |
|:--|:--|
| contentLink | Καθορίζει το περιεχόμενο της λειτουργικής μονάδας. <br><br>URI - Uri στο περιεχόμενο του runbook.  Αυτό θα είναι ένα αρχείο .ps1 για runbooks PowerShell και δέσμης ενεργειών και ένα αρχείο εξαγόμενα γραφικών runbook για ένα γράφημα runbook.  <br> έκδοση - έκδοση του runbook για τη δική σας παρακολούθηση. |

Παρακάτω είναι ένα παράδειγμα ενός πόρου λειτουργική μονάδα.

    {       
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Ενημέρωση λειτουργικές μονάδες
Εάν μπορείτε να ενημερώσετε μια λύση διαχείρισης που περιλαμβάνει ένα runbook που χρησιμοποιεί ένα χρονοδιάγραμμα και τη νέα έκδοση της λύσης σας έχει μια νέα λειτουργική μονάδα που χρησιμοποιείται από το συγκεκριμένο runbook, στη συνέχεια, runbook μπορεί να χρησιμοποιεί την παλιά έκδοση της λειτουργικής μονάδας.  Πρέπει να περιλαμβάνουν τα εξής runbooks στη λύση σας και να δημιουργήσετε μια εργασία για να εκτελέσετε τις πριν από τις άλλες runbooks.  Αυτό θα εξασφαλίσει ότι τυχόν λειτουργικές μονάδες ενημερώνονται ως απαιτείται πριν από την runbooks γίνεται φόρτωση.

- [Ενημέρωση ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) εξασφαλίζει ότι όλες οι λειτουργικές μονάδες που χρησιμοποιούνται από runbooks στη λύση σας είναι η πιο πρόσφατη έκδοση.  
- [ReRegisterAutomationSchedule-MS-διαχείρισης](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) θα επαναλάβετε την καταχώρηση όλους τους πόρους χρονοδιάγραμμα για να βεβαιωθείτε ότι το runbooks συνδεθεί με χρήση τις πιο πρόσφατες λειτουργικές μονάδες.


Ακολουθεί ένα δείγμα τα απαιτούμενα στοιχεία μιας λύσης για την υποστήριξη της ενημερωμένης έκδοσης λειτουργική μονάδα.
    
    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Επόμενα βήματα

- [Προσθήκη προβολής στη λύση σας](operations-management-suite-solutions-resources-views.md) για την απεικόνιση δεδομένων που έχουν συλλεχθεί.