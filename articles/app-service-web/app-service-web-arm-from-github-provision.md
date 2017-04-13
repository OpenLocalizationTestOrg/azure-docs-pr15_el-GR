<properties 
    pageTitle="Ανάπτυξη εφαρμογής web που είναι συνδεδεμένο με ένα αποθετήριο GitHub" 
    description="Χρήση ενός προτύπου για τη διαχείριση πόρων Azure για την ανάπτυξη μιας εφαρμογής web που περιέχει ένα έργο από ένα αποθετήριο GitHub." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/27/2016" 
    ms.author="cephalin"/>

# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Ανάπτυξη εφαρμογής web συνδεδεμένο με ένα αποθετήριο GitHub

Σε αυτό το θέμα, θα μάθετε πώς μπορείτε να δημιουργήσετε ένα πρότυπο από διαχειριστή πόρων Azure που αναπτύσσει μια εφαρμογή web που είναι συνδεδεμένο με ένα έργο σε ένα αποθετήριο GitHub. Θα μάθετε πώς να καθορίσετε τους πόρους που έχουν αναπτυχθεί και πώς μπορείτε να ορίσετε τις παραμέτρους που είναι καθοριστεί όταν εκτελείται το ανάπτυξης. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο για τη δική σας αναπτύξεις, ή να προσαρμόσετε ώστε να πληρούν τις απαιτήσεις σας.

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προτύπων, ανατρέξτε στο θέμα [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure](../resource-group-authoring-templates.md).

Για την πλήρη πρότυπο, ανατρέξτε στο θέμα [Web App είναι συνδεδεμένο με το πρότυπο GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-deploy"></a>Τι θα αναπτύξετε

Με αυτό το πρότυπο θα αναπτύξετε μια εφαρμογή web που περιέχει τον κωδικό από ένα έργο στο GitHub.

Για να εκτελέσετε την ανάπτυξη αυτόματα, κάντε κλικ στο κουμπί παρακάτω:

[![Ανάπτυξη Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Παράμετροι

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL

Η διεύθυνση URL για το αποθετήριο GitHub που περιέχει το έργο για την ανάπτυξη. Αυτή η παράμετρος περιέχει μια προεπιλεγμένη τιμή, αλλά αυτή η τιμή προορίζεται μόνο για να σας δείξουν πώς μπορείτε να καταχωρήσετε τη διεύθυνση URL για το αποθετήριο. Μπορείτε να χρησιμοποιήσετε αυτή την τιμή κατά τη δοκιμή του προτύπου, αλλά θα θέλετε να δώσετε τη διεύθυνση URL αποθετήριο τα δικά σας κατά την εργασία με το πρότυπο.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>διακλάδωση

Το υποκατάστημα του αποθετηρίου για χρήση κατά την ανάπτυξη της εφαρμογής. Η προεπιλεγμένη τιμή είναι κύρια, αλλά μπορείτε να παρέχετε το όνομα του κάθε κλάδο στο αποθετήριο δεδομένων που θέλετε να αναπτύξετε.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## <a name="resources-to-deploy"></a>Πόροι για την ανάπτυξη

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Εφαρμογή Web

Δημιουργεί την εφαρμογή web που είναι συνδεδεμένο με το έργο στο GitHub. 

Μπορείτε να καθορίσετε το όνομα της εφαρμογής web μέσω της παραμέτρου **όνομα τοποθεσίας** , καθώς και τη θέση του web app μέσω της παραμέτρου **siteLocation** . Στο στοιχείο **dependsOn** , το πρότυπο ορίζει την εφαρμογή web ως εξαρτάται από την υπηρεσία φιλοξενίας πρόγραμμα. Επειδή είναι εξαρτάται από το πρόγραμμα φιλοξενίας, η εφαρμογή web δεν έχει δημιουργηθεί μέχρι να ολοκληρωθεί το πρόγραμμα φιλοξενίας που δημιουργούνται. Το στοιχείο **dependsOn** χρησιμοποιείται μόνο για να καθορίσετε ανάπτυξης σειρά. Εάν δεν επισημάνετε την εφαρμογή web ως εξαρτάται από το πρόγραμμα φιλοξενίας, Mananger πόρων Azure θα επιχειρήσει να δημιουργήσετε δύο πόρους ταυτόχρονα και ενδέχεται να εμφανιστεί ένα σφάλμα εάν η εφαρμογή web έχει δημιουργηθεί πριν από το πρόγραμμα φιλοξενίας.

Η εφαρμογή web έχει επίσης έναν πόρο παιδί που καθορίζεται στο παρακάτω ενότητα **πόρους** . Αυτός ο πόρος θυγατρικό καθορίζει Προέλευση στοιχείου ελέγχου για το έργο που έχει αναπτυχθεί με την εφαρμογή web. Σε αυτό το πρότυπο, το στοιχείο ελέγχου προέλευσης είναι συνδεδεμένη με ένα συγκεκριμένο αποθετήριο GitHub. Το αποθετήριο GitHub ορίζεται με τον κωδικό **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** που ενδέχεται να καθορίσετε από τον προγραμματισμό τη διεύθυνση URL του αποθετηρίου όταν θέλετε να δημιουργήσετε ένα πρότυπο που αναπτύσσει επανειλημμένα ένα έργο ενώ απαιτεί τον ελάχιστο αριθμό των παραμέτρων.
Αντί για σκληρό κωδικοποίηση τη διεύθυνση URL αποθετήριο δεδομένων, μπορείτε να προσθέσετε μια παράμετρος για τη διεύθυνση URL αποθετήριο δεδομένων και να χρησιμοποιήσετε αυτήν την τιμή για την ιδιότητα **RepoUrl** .

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Εντολές για να εκτελέσετε ανάπτυξης

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 
