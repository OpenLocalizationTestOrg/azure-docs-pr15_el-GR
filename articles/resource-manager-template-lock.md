<properties
   pageTitle="Πρότυπο διαχείρισης πόρων για πόρο κλειδωμάτων | Microsoft Azure"
   description="Εμφανίζει το σχήμα από διαχειριστή πόρων για την ανάπτυξη κλειδωμάτων πόρων σε ένα πρότυπο."
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="resource-locks-template-schema"></a>Σχήμα πρότυπο κλειδωμάτων πόρων

Δημιουργεί μια κλειδαριά σε έναν πόρο και τους θυγατρικούς πόρους.

## <a name="schema-format"></a>Μορφοποίηση σχήματος

Για να δημιουργήσετε ένα κλείδωμα, προσθέστε την ακόλουθη διάταξη στην ενότητα πόροι του προτύπου σας.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## <a name="values"></a>Τιμές

Στους πίνακες που ακολουθούν περιγράφουν τις τιμές πρέπει να ορίσετε στο σχήμα.

| Όνομα | Απαιτείται | Περιγραφή |
| ---- | -------- | ----------- |
| Τύπος | Ναι | Ο τύπος πόρου για να δημιουργήσετε.<br /><br />Για τους πόρους:<br />**{χώρο ονομάτων} / {πληκτρολογήστε} / υπηρεσίες παροχής/κλειδωμάτων**<br /><br/>Για τις ομάδες πόρων:<br />**Microsoft.Authorization/locks** |
| apiVersion | Ναι | Η έκδοση API για να χρησιμοποιήσετε για τη δημιουργία του πόρου.<br /><br />Χρήση:<br />**2015 01--01**<br /><br /> |
| Όνομα | Ναι | Μια τιμή που καθορίζει τον πόρο για να κλειδώσετε και ένα όνομα για το κλείδωμα. Μπορεί να είναι έως και 64 χαρακτήρες και δεν μπορούν να περιέχουν <>, %, &,, ή οποιαδήποτε χαρακτήρες ελέγχου.<br /><br />Για τους πόρους:<br />**{resource}/Microsoft.Authorization/{lockname}**<br /><br />Για τις ομάδες πόρων:<br />**{lockname}** |
| dependsOn | Όχι | Μια λίστα διαχωρισμένες με κόμματα ενός πόρου ονομάτων ή των μοναδικών αναγνωριστικών πόρων.<br /><br />Η συλλογή των πόρων που εξαρτάται από αυτό το κλείδωμα. Εάν ο πόρος κλειδώματος που έχει αναπτυχθεί στο ίδιο πρότυπο, συμπεριλάβετε το όνομα του πόρου σε αυτό το στοιχείο για να βεβαιωθείτε ότι ο πόρος έχει αναπτυχθεί πρώτα. | 
| Ιδιότητες | Ναι | Αντικείμενο που προσδιορίζει τον τύπο του κλειδώματος και σημειώσεις σχετικά με το κλείδωμα.<br /><br />Ανατρέξτε στην ενότητα [Ιδιότητες αντικειμένου](#properties-object). |  

### <a name="properties-object"></a>Ιδιότητες αντικειμένου

| Όνομα | Απαιτείται | Περιγραφή |
| ---- | -------- | ----------- |
| επίπεδο   | Ναι | Ο τύπος του κλειδώματος για να εφαρμόσετε το εύρος.<br /><br />**CannotDelete** - οι χρήστες μπορούν να τροποποιήσετε πόρων αλλά δεν διαγράψτε το.<br />**Μόνο για ανάγνωση** - οι χρήστες μπορούν να διαβάσουν από έναν πόρο, αλλά δεν μπορούν να το διαγράψετε ή να εκτελέστε τις ενέργειες σε αυτήν. |
| σημειώσεις   | Όχι | Περιγραφή το κλείδωμα. Μπορεί να είναι έως 512 χαρακτήρες. |


## <a name="how-to-use-the-lock-resource"></a>Πώς μπορείτε να χρησιμοποιήσετε τον πόρο κλειδώματος

Μπορείτε να προσθέσετε αυτόν τον πόρο στο πρότυπό σας, για να αποτρέψετε την καθορισμένη ενέργειες σε έναν πόρο. Το κλείδωμα ισχύει για όλους τους χρήστες και ομάδες.

Για να δημιουργήσετε ή να διαγράψετε κλειδωμάτων διαχείρισης, πρέπει να έχετε πρόσβαση σε **Microsoft.Authorization/** * ή * *Microsoft.Authorization/locks/* ** Ενέργειες. Ενσωματωμένη ρόλων, μόνο **κάτοχο** και **χρήστη πρόσβαση διαχειριστή ** εκχωρούνται αυτές τις ενέργειες. Για πληροφορίες σχετικά με τον έλεγχο πρόσβασης βάσει ρόλων, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης βάσει ρόλων Azure](./active-directory/role-based-access-control-configure.md).

Το κλείδωμα εφαρμόζεται για τον συγκεκριμένο πόρο και πόροι θυγατρικό.

Μπορείτε να καταργήσετε μια κλειδαριά με την εντολή PowerShell **Κατάργηση AzureRmResourceLock** ή με τη [λειτουργία διαγραφής](https://msdn.microsoft.com/library/azure/mt204562.aspx) από το REST API.

## <a name="examples"></a>Παραδείγματα

Το παρακάτω παράδειγμα ισχύει ένα κλείδωμα δεν είναι δυνατό να διαγραφή για μια εφαρμογή web.

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
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

Το επόμενο παράδειγμα ισχύει κλείδωμα δεν είναι δυνατό να διαγραφή για την ομάδα των πόρων.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## <a name="next-steps"></a>Επόμενα βήματα

- Για πληροφορίες σχετικά με τη δομή του προτύπου, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](resource-group-authoring-templates.md).
- Για περισσότερες πληροφορίες σχετικά με το κλείδωμα, ανατρέξτε στο θέμα [πόροι κλείδωμα με τη διαχείριση πόρων Azure](resource-group-lock-resources.md).
