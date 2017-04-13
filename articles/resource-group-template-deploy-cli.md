<properties
   pageTitle="Ανάπτυξη των πόρων με Azure CLI και πρότυπο | Microsoft Azure"
   description="Χρήση της διαχείρισης πόρων Azure και Azure CLI για να αναπτύξετε ένα πόρους σε Azure. Τους πόρους που ορίζονται σε ένα πρότυπο από διαχειριστή πόρων."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Ανάπτυξη των πόρων με πρότυπα διαχείρισης πόρων και Azure CLI

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Πύλη](resource-group-template-deploy-portal.md)
- [REST API](resource-group-template-deploy-rest.md)

Αυτό το θέμα εξηγεί τον τρόπο χρήσης του Azure CLI με τα πρότυπα για τη διαχείριση πόρων για να αναπτύξετε το παράθυρο των πόρων σε Azure.  

> [AZURE.TIP] Για βοήθεια σχετικά με τον εντοπισμό σφαλμάτων σε ένα μήνυμα σφάλματος κατά την ανάπτυξη, ανατρέξτε στο θέμα:
>
> - [Λειτουργίες ανάπτυξης προβολή με Azure CLI](resource-manager-troubleshoot-deployments-cli.md) , για να μάθετε περισσότερα σχετικά με τη λήψη πληροφοριών που σας βοηθά να αντιμετωπίσετε το σφάλμα
> - [Αντιμετώπιση προβλημάτων συνηθισμένων σφαλμάτων κατά την ανάπτυξη πόρων για Azure με τη Διαχείριση Azure πόρων](resource-manager-common-deployment-errors.md) για να μάθετε τον τρόπο επίλυσης κοινών σφαλμάτων ανάπτυξης

Το πρότυπο μπορεί να είναι είτε ένα τοπικό αρχείο ή ένα εξωτερικό αρχείο που είναι διαθέσιμα μέσω ενός URI. Όταν το πρότυπό σας βρίσκεται σε ένα λογαριασμό του χώρου αποθήκευσης, μπορείτε να περιορίσετε την πρόσβαση στο πρότυπο και να παρέχουν ένα διακριτικό υπογραφής (συσχετισμών Ασφαλείας) κοινόχρηστο πρόσβασης κατά την ανάπτυξη.

## <a name="quick-steps-to-deployment"></a>Γρήγορα βήματα για να ανάπτυξης

Σε αυτό το άρθρο περιγράφει όλες τις διαφορετικές επιλογές που διαθέσιμες σε εσάς κατά την ανάπτυξη. Ωστόσο, συχνά χρειάζεται μόνο δύο απλές εντολές. Γρήγορα αποτελέσματα με ανάπτυξη, χρησιμοποιήστε τις παρακάτω εντολές:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Για να μάθετε περισσότερα σχετικά με τις επιλογές για την ανάπτυξη που μπορεί να είναι πιο κατάλληλα για το σενάριό σας, συνεχίστε να διαβάζετε αυτό το άρθρο.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-azure-cli"></a>Ανάπτυξη με το Azure CLI

Εάν δεν έχετε ήδη χρησιμοποιήσει Azure CLI με τη διαχείριση πόρων, ανατρέξτε στο θέμα [χρήση του Azure CLI για Mac, Linux, και το Windows Azure διαχείρισης πόρων](xplat-cli-azure-resource-manager.md).

1. Συνδεθείτε στο λογαριασμό σας στο Azure. Μετά την παροχή τα διαπιστευτήριά σας, η εντολή επιστρέφει το αποτέλεσμα της σύνδεσής σας.

        azure login
  
        ...
        info:    login command OK

2. Εάν έχετε πολλές συνδρομές, δώστε το αναγνωριστικό συνδρομής που θέλετε να χρησιμοποιήσετε για ανάπτυξη.

        azure account set <YourSubscriptionNameOrId>

3. Μετάβαση σε λειτουργική μονάδα Azure διαχείριση πόρων. Λάβετε την επιβεβαίωση της λειτουργίας στην νέα.

        azure config mode arm
   
        info:     New mode is arm

4. Εάν δεν έχετε μια υπάρχουσα ομάδα πόρων, δημιουργήστε μια ομάδα πόρων. Εισαγάγετε το όνομα της ομάδας πόρων και θέση που χρειάζεστε για τη λύση. Πρέπει να παρέχει μια θέση για την ομάδα των πόρων, επειδή η ομάδα πόρων αποθηκεύει μετα-δεδομένα σχετικά με τους πόρους. Για λόγους συμβατότητας, ενδέχεται να θέλετε να καθορίσετε τη θέση όπου είναι αποθηκευμένο το συγκεκριμένο μετα-δεδομένων. Σε γενικές γραμμές, συνιστάται να καθορίσετε μια θέση όπου θα βρίσκονται πιο τους πόρους σας. Χρήση του στην ίδια θέση να απλοποιήσετε το πρότυπό σας.

        azure group create -n ExampleResourceGroup -l "West US"

     Επιστρέφεται μια σύνοψη της νέας ομάδας πόρων.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Επικυρώστε την ανάπτυξη πριν από την εκτέλεση του, εκτελέστε την εντολή **επικύρωση πρότυπο azure ομάδας** . Κατά τη δοκιμή ανάπτυξης, δώστε τις παραμέτρους ακριβώς όπως θα κάνατε κατά την εκτέλεση της ανάπτυξης (απεικονίζεται στο επόμενο βήμα).

        azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup

5. Για να αναπτύξετε τους πόρους σας ομάδα πόρων, εκτελέστε την ακόλουθη εντολή και δώστε τις απαραίτητες παραμέτρους. Οι παράμετροι περιλαμβάνουν ένα όνομα για την ανάπτυξη, το όνομα του σας ομάδα πόρων, η διαδρομή ή διεύθυνση URL για το πρότυπο και τυχόν άλλες παραμέτρους που απαιτούνται για το σενάριό σας. 
   
     Έχετε τις εξής τρεις επιλογές για την παροχή τιμές παραμέτρων: 

     1. Χρήση παραμέτρων ενσωματωμένη και ένα τοπικό πρότυπο. Κάθε παράμετρο έχει τη μορφή: `"ParameterName": { "value": "ParameterValue" }`. Το παρακάτω παράδειγμα εμφανίζει τις παραμέτρους με χαρακτήρες διαφυγής.

            azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Χρήση παραμέτρων ενσωματωμένη και μια σύνδεση σε ένα πρότυπο.

            azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Χρησιμοποιήστε ένα αρχείο παραμέτρων. Για πληροφορίες σχετικά με το αρχείο προτύπου, ανατρέξτε [στο αρχείο παραμέτρων](#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Αφού τους πόρους που έχουν αναπτυχθεί σε μία από τις τρεις παραπάνω μεθόδους, θα δείτε μια σύνοψη των ανάπτυξης.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Για να εκτελέσετε μια πλήρη ανάπτυξη, ορίστε **λειτουργία** **ολοκληρώθηκε**. Να είστε προσεκτικοί όταν χρησιμοποιείτε αυτήν την κατάσταση λειτουργίας, όπως κατά λάθος, μπορείτε να διαγράψετε τους πόρους που δεν βρίσκονται στο πρότυπό σας.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Εάν θέλετε να συνδεθείτε πρόσθετες πληροφορίες σχετικά με την ανάπτυξη που μπορεί να σας βοηθήσει να αντιμετωπίσετε τυχόν σφάλματα ανάπτυξης, χρησιμοποιήστε την παράμετρο **εντοπισμού σφαλμάτων με τη ρύθμιση** . Μπορείτε να καθορίσετε ότι θα καταγράφονται με τη λειτουργία ανάπτυξης περιεχομένου αίτησης, απόκριση περιεχομένου ή και τα δύο.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## <a name="deploy-template-from-storage-with-sas-token"></a>Ανάπτυξη προτύπου από το χώρο αποθήκευσης με διακριτικό συσχετισμών Ασφαλείας

Μπορείτε να προσθέσετε τα πρότυπά σας λογαριασμό χώρου αποθήκευσης και σύνδεση σε αυτά κατά τη διάρκεια της ανάπτυξης με διακριτικό συσχετισμών Ασφαλείας.

> [AZURE.IMPORTANT] Ακολουθώντας τα παρακάτω βήματα, το αντικείμενο blob που περιέχει το πρότυπο είναι προσβάσιμος σε μόνο ο κάτοχος του λογαριασμού. Ωστόσο, κατά τη δημιουργία ενός διακριτικού συσχετισμών Ασφαλείας για το αντικείμενο blob, το αντικείμενο blob είναι προσβάσιμος σε οποιονδήποτε με συγκεκριμένο URI. Εάν κάποιος άλλος χρήστης διακόπτει την URI, αυτός ο χρήστης είναι μπορούν να έχουν πρόσβαση στο πρότυπο. Χρήση ενός διακριτικού συσχετισμών Ασφαλείας είναι ένας καλός τρόπος με τον περιορισμό της πρόσβασης των προτύπων, αλλά δεν πρέπει να συμπεριλάβετε ευαίσθητα δεδομένα, όπως κωδικούς πρόσβασης απευθείας στο πρότυπο.

### <a name="add-private-template-to-storage-account"></a>Προσθήκη ιδιωτικό προτύπου με το λογαριασμό χώρου αποθήκευσης

Ακολουθήστε τα παρακάτω βήματα ρύθμιση ενός λογαριασμού χώρου αποθήκευσης για πρότυπα:

1. Δημιουργία μιας ομάδας πόρων.

        azure group create -n "ManageGroup" -l "westus"

2. Δημιουργία λογαριασμού χώρου αποθήκευσης. Το όνομα του λογαριασμού χώρου αποθήκευσης πρέπει να είναι μοναδικό σε Azure, ώστε να παρέχετε το δικό σας όνομα για το λογαριασμό.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Ορισμός μεταβλητές για το λογαριασμό χώρου αποθήκευσης και το κλειδί.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Δημιουργία κοντέινερ. Το δικαίωμα έχει οριστεί σε **Απενεργοποίηση** γεγονός που σημαίνει ότι το κοντέινερ είναι διαθέσιμο μόνο για τον κάτοχο της.

        azure storage container create --container templates -p Off 
        
4. Προσθήκη του προτύπου στο κοντέινερ.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Παροχή διακριτικό συσχετισμών Ασφαλείας κατά τη διάρκεια της ανάπτυξης

Για να αναπτύξετε ένα ιδιωτικό πρότυπο σε ένα λογαριασμό του χώρου αποθήκευσης, ανάκτηση ενός διακριτικού συσχετισμών Ασφαλείας και συμπεριλαμβάνει στο URI για το πρότυπο.

1. Δημιουργία ενός διακριτικού συσχετισμών Ασφαλείας με δικαιώματα ανάγνωσης και την ώρα λήξης για να περιορίσετε την πρόσβαση. Ορίστε το χρόνο λήξης για να επιτρέψετε αρκετό χρόνο για να ολοκληρωθεί η ανάπτυξη. Ανακτήστε το πλήρες URI του προτύπου όπως το διακριτικό συσχετισμών Ασφαλείας.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Ανάπτυξη του προτύπου, παρέχοντας το URI που περιλαμβάνει το διακριτικό συσχετισμών Ασφαλείας.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Για ένα παράδειγμα της χρήσης ενός διακριτικού συσχετισμών Ασφαλείας με συνδεδεμένα πρότυπα, ανατρέξτε στο θέμα [χρήση συνδεδεμένων προτύπων με τη διαχείριση πόρων Azure](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Επόμενα βήματα
- Για ένα παράδειγμα ανάπτυξης πόρους μέσω της βιβλιοθήκης του προγράμματος-πελάτη .NET, ανατρέξτε στο θέμα [Χρήση πόρους ανάπτυξης των βιβλιοθηκών .NET και ένα πρότυπο](virtual-machines/virtual-machines-windows-csharp-template.md).
- Για να ορίσετε τις παραμέτρους στο πρότυπο, ανατρέξτε στο θέμα [πρότυπα σύνταξη από κοινού](resource-group-authoring-templates.md#parameters).
- Για οδηγίες σχετικά με την ανάπτυξη της λύσης σε διαφορετικά περιβάλλοντα, ανατρέξτε στο θέμα [ανάπτυξη και περιβάλλοντα δοκιμής στο Microsoft Azure](solution-dev-test-environments.md).
- Για λεπτομέρειες σχετικά με τη χρήση μιας αναφοράς KeyVault για τη μεταβίβαση ασφαλούς τιμών, ανατρέξτε στο θέμα [μεταβιβάζουν ασφαλούς τιμές κατά την ανάπτυξη](resource-manager-keyvault-parameter.md).

