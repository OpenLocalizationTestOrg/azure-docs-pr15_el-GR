<properties
   pageTitle="Διαχείριση πόρων προτύπου για τη σύνδεση πόρους | Microsoft Azure"
   description="Εμφανίζει το σχήμα από διαχειριστή πόρων για την ανάπτυξη συνδέσεων μεταξύ σχετικών πόρων σε ένα πρότυπο."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="resource-links-template-schema"></a>Σχήμα πρότυπο συνδέσεις πόρων

Δημιουργεί μια σύνδεση ανάμεσα σε δύο πόρους. Η σύνδεση έχει εφαρμοστεί σε έναν πόρο γνωστό ως ο πόρος προέλευσης. Ο δεύτερος πόρος στο πλαίσιο σύνδεση είναι γνωστό ως τον πόρο προορισμού.

## <a name="schema-format"></a>Μορφοποίηση σχήματος

Για να δημιουργήσετε μια σύνδεση, προσθέστε την ακόλουθη διάταξη στην ενότητα πόροι του προτύπου σας.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## <a name="values"></a>Τιμές

Στους πίνακες που ακολουθούν περιγράφουν τις τιμές πρέπει να ορίσετε στο σχήμα.

| Όνομα | Τιμή |
| ---- | ---- |
| Τύπος | Απαρίθμηση<br />Απαιτείται<br />**{χώρο ονομάτων} / {πληκτρολογήστε} / υπηρεσίες παροχής/συνδέσεις**<br /><br />Ο τύπος πόρου για να δημιουργήσετε. Το πεδίο {ονομάτων} και {τύπος} τιμές αναφέρονται στον τύπο χώρο ονομάτων και πόρων υπηρεσία παροχής του πόρου προέλευσης. |
| apiVersion | Απαρίθμηση<br />Απαιτείται<br />**2015 01--01**<br /><br />Η έκδοση API για να χρησιμοποιήσετε για τη δημιουργία του πόρου. |  
| Όνομα | Συμβολοσειρά<br />Απαιτείται<br />**{resouce}/Microsoft.Resources/{linkname}**<br /> έως και 64 χαρακτήρες και δεν μπορούν να περιέχουν <>, %, &,, ή οποιαδήποτε χαρακτήρες ελέγχου.<br /><br />Μια τιμή που καθορίζει τόσο το όνομα του πόρου προέλευσης και ένα όνομα για τη σύνδεση. |
| dependsOn | Πίνακα<br />Προαιρετικό<br />Μια λίστα διαχωρισμένες με κόμματα ενός πόρου ονομάτων ή των μοναδικών αναγνωριστικών πόρων.<br /><br />Η συλλογή των πόρων εξαρτάται από αυτήν τη σύνδεση. Εάν οι πόροι που πραγματοποιείτε σύνδεση αναπτύσσονται στο ίδιο πρότυπο, περιλαμβάνει αυτά τα ονόματα των πόρων σε αυτό το στοιχείο για να βεβαιωθείτε ότι έχουν αναπτυχθεί πρώτα. | 
| Ιδιότητες | Αντικείμενο<br />Απαιτείται<br />[Ιδιότητες αντικειμένου](#properties)<br /><br />Ένα αντικείμενο το οποίο προσδιορίζει τον πόρο να δημιουργήσετε τη σύνδεση και σημειώσεις σχετικά με τη σύνδεση. |  

<a id="properties" />
### <a name="properties-object"></a>Ιδιότητες αντικειμένου

| Όνομα | Τιμή |
| ------- | ---- |
| targetId | Συμβολοσειρά<br />Απαιτείται<br />**{αναγνωριστικό πόρου}**<br /><br />Το αναγνωριστικό του πόρου προορισμού για σύνδεση. |
| σημειώσεις | Συμβολοσειρά<br />Προαιρετικό<br />έως 512 χαρακτήρες<br /><br />Περιγραφή το κλείδωμα. |


## <a name="how-to-use-the-link-resource"></a>Πώς μπορείτε να χρησιμοποιήσετε τον πόρο σύνδεσης

Μπορείτε να εφαρμόσετε μια σύνδεση ανάμεσα σε δύο πόρων όταν οι πόροι έχουν εξάρτηση που συνεχίζει μετά την ανάπτυξη. Για παράδειγμα, μια εφαρμογή μπορεί να συνδεθεί με μια βάση δεδομένων σε μια ομάδα διαφορετικό πόρο. Μπορείτε να ορίσετε αυτήν την εξάρτηση, δημιουργώντας μια σύνδεση από την εφαρμογή στη βάση δεδομένων. Συνδέσεις σάς επιτρέπουν να εγγράφων τη σχέση μεταξύ των δύο πόρους. Αργότερα, εσείς ή κάποιος άλλος στον οργανισμό σας να υποβάλετε ερώτημα έναν πόρο για τις συνδέσεις για να ανακαλύψετε πώς λειτουργεί ο πόρος με άλλους πόρους.

Όλοι οι πόροι συνδεδεμένων πρέπει να ανήκουν σε την ίδια συνδρομή. Κάθε πόρο μπορούν να συνδεθούν με 50 άλλους πόρους. Εάν οποιοδήποτε από τους πόρους συνδεδεμένων έχουν διαγράψει ή μετακινήσει, τον κάτοχο της σύνδεσης πρέπει να εκκαθαρίσετε το υπόλοιπο σύνδεσης.

Για να εργαστείτε με συνδέσεις στις ΥΠΌΛΟΙΠΕΣ, ανατρέξτε στο θέμα [Συνδεδεμένων πόρους](https://msdn.microsoft.com/library/azure/mt238499.aspx).

Χρησιμοποιήστε την ακόλουθη εντολή Azure PowerShell για να δείτε όλες τις συνδέσεις σε τη συνδρομή σας. Μπορείτε να παράσχετε άλλες παραμέτρους για να περιορίσετε τα αποτελέσματα.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## <a name="examples"></a>Παραδείγματα

Το παρακάτω παράδειγμα ισχύει μόνο για ανάγνωση κλείδωμα για μια εφαρμογή web.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Γρήγορη έναρξη προτύπων

Τα παρακάτω πρότυπα γρήγορη έναρξη ανάπτυξη πόρους με μια σύνδεση.

- [Ειδοποίηση ουρά με την εφαρμογή λογικής](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Ειδοποίησης για αδράνεια με την εφαρμογή λογικής](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [Παροχή εφαρμογής API με μια υπάρχουσα πύλης](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Παροχή εφαρμογής API με μια νέα πύλη](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Δημιουργία εφαρμογής λογικής εφαρμογή μαζί με API χρησιμοποιώντας ένα πρότυπο](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Εφαρμογή λογικής που στέλνει ένα μήνυμα κειμένου όταν ενεργοποιείται μια ειδοποίηση](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## <a name="next-steps"></a>Επόμενα βήματα

- Για πληροφορίες σχετικά με τη δομή του προτύπου, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](resource-group-authoring-templates.md).
