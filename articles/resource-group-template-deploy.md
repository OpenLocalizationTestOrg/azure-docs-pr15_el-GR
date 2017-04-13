<properties
   pageTitle="Ανάπτυξη των πόρων με το PowerShell και το πρότυπο | Microsoft Azure"
   description="Χρήση της διαχείρισης πόρων Azure και Azure PowerShell για να αναπτύξετε ένα πόρους σε Azure. Τους πόρους που ορίζονται σε ένα πρότυπο από διαχειριστή πόρων."
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

# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Ανάπτυξη των πόρων με πρότυπα διαχείρισης πόρων και Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Πύλη](resource-group-template-deploy-portal.md)
- [REST API](resource-group-template-deploy-rest.md)

Αυτό το θέμα εξηγεί τον τρόπο χρήσης του Azure PowerShell με τα πρότυπα για τη διαχείριση πόρων για να αναπτύξετε το παράθυρο των πόρων σε Azure.  

> [AZURE.TIP] Για βοήθεια σχετικά με τον εντοπισμό σφαλμάτων σε ένα μήνυμα σφάλματος κατά την ανάπτυξη, ανατρέξτε στο θέμα:
>
> - [Λειτουργίες ανάπτυξης προβολή με το Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md) , για να μάθετε περισσότερα σχετικά με τη λήψη πληροφοριών που σας βοηθά να αντιμετωπίσετε το σφάλμα
> - [Αντιμετώπιση προβλημάτων συνηθισμένων σφαλμάτων κατά την ανάπτυξη πόρων για Azure με τη Διαχείριση Azure πόρων](resource-manager-common-deployment-errors.md) για να μάθετε τον τρόπο επίλυσης κοινών σφαλμάτων ανάπτυξης

Το πρότυπο μπορεί να είναι είτε ένα τοπικό αρχείο ή ένα εξωτερικό αρχείο που είναι διαθέσιμα μέσω ενός URI. Όταν το πρότυπό σας βρίσκεται σε ένα λογαριασμό του χώρου αποθήκευσης, μπορείτε να περιορίσετε την πρόσβαση στο πρότυπο και να παρέχουν ένα διακριτικό υπογραφής (συσχετισμών Ασφαλείας) κοινόχρηστο πρόσβασης κατά την ανάπτυξη.

## <a name="quick-steps-to-deployment"></a>Γρήγορα βήματα για να ανάπτυξης

Σε αυτό το άρθρο περιγράφει όλες τις διαφορετικές επιλογές που διαθέσιμες σε εσάς κατά την ανάπτυξη. Ωστόσο, συχνά χρειάζεται μόνο δύο απλές εντολές. Γρήγορα αποτελέσματα με ανάπτυξη, χρησιμοποιήστε τις παρακάτω εντολές:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Για να μάθετε περισσότερα σχετικά με τις επιλογές για την ανάπτυξη που μπορεί να είναι πιο κατάλληλα για το σενάριό σας, συνεχίστε να διαβάζετε αυτό το άρθρο.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-powershell"></a>Ανάπτυξη με το PowerShell

1. Συνδεθείτε στο λογαριασμό σας στο Azure.

        Add-AzureRmAccount

     Επιστρέφεται μια σύνοψη του λογαριασμού σας.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Εάν έχετε πολλές συνδρομές, δώστε το Αναγνωριστικό συνδρομής που θέλετε να χρησιμοποιήσετε για την ανάπτυξη με την εντολή **Ορισμός AzureRmContext** . 

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. Συνήθως, κατά την ανάπτυξη νέου προτύπου, που θέλετε να δημιουργήσετε μια ομάδα πόρων για τους πόρους. Εάν έχετε μια υπάρχουσα ομάδα πόρων που θέλετε να αναπτύξετε, μπορείτε να παραλείψετε αυτό το βήμα και να χρησιμοποιήσετε αυτήν την ομάδα πόρων. 

     Για να δημιουργήσετε μια ομάδα πόρων, δώστε ένα όνομα και μια θέση για την ομάδα πόρων. Πρέπει να παρέχει μια θέση για την ομάδα των πόρων, επειδή η ομάδα πόρων αποθηκεύει μετα-δεδομένα σχετικά με τους πόρους. Για λόγους συμβατότητας, ενδέχεται να θέλετε να καθορίσετε τη θέση όπου είναι αποθηκευμένο το συγκεκριμένο μετα-δεδομένων. Σε γενικές γραμμές, συνιστάται να καθορίσετε μια θέση όπου θα βρίσκονται πιο τους πόρους σας. Χρήση του στην ίδια θέση να απλοποιήσετε το πρότυπό σας.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Επιστρέφεται μια σύνοψη της νέας ομάδας πόρων.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. Πριν από την εκτέλεση του ανάπτυξη, μπορείτε να επικυρώσετε τις ρυθμίσεις ανάπτυξης. Το cmdlet **Δοκιμής AzureRmResourceGroupDeployment** σάς επιτρέπει να βρείτε προβλήματα πριν από τη δημιουργία πραγματικούς πόρους. Το παρακάτω παράδειγμα εμφανίζει τον τρόπο για την επικύρωση μιας ανάπτυξης.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Για να αναπτύξετε τους πόρους σας ομάδα πόρων, εκτελέστε την εντολή **Δημιουργία AzureRmResourceGroupDeployment** και δώστε τις απαραίτητες παραμέτρους. Οι παράμετροι περιλαμβάνουν ένα όνομα για την ανάπτυξη, το όνομα του σας ομάδα πόρων, η διαδρομή ή διεύθυνση URL στο πρότυπο που δημιουργήσατε και τυχόν άλλες παραμέτρους που απαιτούνται για το σενάριό σας. Εάν η παράμετρος **λειτουργία** δεν έχει καθοριστεί, χρησιμοποιείται η προεπιλεγμένη τιμή **προσαύξηση** . Για να εκτελέσετε μια πλήρη ανάπτυξη, ορίστε **λειτουργία** **ολοκληρώθηκε**. Να είστε προσεκτικοί όταν χρησιμοποιείτε την πλήρη λειτουργία, όπως κατά λάθος, μπορείτε να διαγράψετε τους πόρους που δεν βρίσκονται στο πρότυπό σας.

     Για να αναπτύξετε ένα τοπικό πρότυπο, χρησιμοποιήστε την παράμετρο **TemplateFile** :

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Για να αναπτύξετε ένα εξωτερικό πρότυπο, χρήση παραμέτρου **TemplateUri** :

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Έχετε τις ακόλουθες επιλογές για την παροχή τιμές παραμέτρων: 
   
     1. Χρησιμοποιήστε το ενσωματωμένο παραμέτρους.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Χρησιμοποιήστε ένα αντικείμενο παραμέτρου.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Χρήση ενός αρχείου τοπικής παραμέτρου. Για πληροφορίες σχετικά με το αρχείο προτύπου, ανατρέξτε [στο αρχείο παραμέτρων](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Χρήση ενός αρχείου εξωτερικών παραμέτρου. Για πληροφορίες σχετικά με το αρχείο προτύπου, ανατρέξτε [στο αρχείο παραμέτρων](#parameter-file). 

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Όταν χρησιμοποιείτε ένα αρχείο εξωτερικών παραμέτρου, δεν είναι δυνατό να περάσετε άλλες τιμές είτε ενσωματωμένο ή από ένα τοπικό αρχείο. Για περισσότερες πληροφορίες, ανατρέξτε στο άρθρο [παραμέτρου προτεραιότητα](#parameter-precendence).

     Αφού τους πόρους που έχουν αναπτυχθεί, θα δείτε μια σύνοψη των ανάπτυξης.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Εάν το πρότυπό σας περιλαμβάνει μια παράμετρο με το ίδιο όνομα με μία από τις παραμέτρους στην εντολή PowerShell, που θα σας ζητηθεί να δώσετε μια τιμή για αυτήν την παράμετρο. Η παράμετρος από το πρότυπο θα περιλαμβάνει το επιθεματική **FromTemplate**. Για παράδειγμα, μια παράμετρο που ονομάζεται **ResourceGroupName** στο σας πρότυπο βρίσκεται σε διένεξη με την παράμετρο **ResourceGroupName** στο το cmdlet [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) . Θα σας ζητηθεί να δώσετε μια τιμή για το **ResourceGroupNameFromTemplate**. Σε γενικές γραμμές, θα πρέπει να αποφύγετε αυτό σύγχυση δίνοντάς δεν παραμέτρους με το ίδιο όνομα με παραμέτρους που χρησιμοποιούνται για λειτουργίες ανάπτυξης.

6. Εάν θέλετε να συνδεθείτε πρόσθετες πληροφορίες σχετικά με την ανάπτυξη που μπορεί να σας βοηθήσει να αντιμετωπίσετε τυχόν σφάλματα ανάπτυξης, χρησιμοποιήστε την παράμετρο **DeploymentDebugLogLevel** . Μπορείτε να καθορίσετε ότι θα καταγράφονται με τη λειτουργία ανάπτυξης περιεχομένου αίτησης, απόκριση περιεχομένου ή και τα δύο.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Για περισσότερες πληροφορίες σχετικά με τη χρήση αυτού του περιεχόμενου εντοπισμού σφαλμάτων για την αντιμετώπιση προβλημάτων αναπτύξεις, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων αναπτύξεις ομάδα πόρων με το Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md).

## <a name="deploy-template-from-storage-with-sas-token"></a>Ανάπτυξη προτύπου από το χώρο αποθήκευσης με διακριτικό συσχετισμών Ασφαλείας

Μπορείτε να προσθέσετε τα πρότυπά σας λογαριασμό χώρου αποθήκευσης και σύνδεση σε αυτά κατά τη διάρκεια της ανάπτυξης με διακριτικό συσχετισμών Ασφαλείας.

> [AZURE.IMPORTANT] Ακολουθώντας τα παρακάτω βήματα, το αντικείμενο blob που περιέχει το πρότυπο είναι προσβάσιμος σε μόνο ο κάτοχος του λογαριασμού. Ωστόσο, κατά τη δημιουργία ενός διακριτικού συσχετισμών Ασφαλείας για το αντικείμενο blob, το αντικείμενο blob είναι προσβάσιμος σε οποιονδήποτε με συγκεκριμένο URI. Εάν κάποιος άλλος χρήστης διακόπτει την URI, αυτός ο χρήστης είναι μπορούν να έχουν πρόσβαση στο πρότυπο. Χρήση ενός διακριτικού συσχετισμών Ασφαλείας είναι ένας καλός τρόπος με τον περιορισμό της πρόσβασης των προτύπων, αλλά δεν πρέπει να συμπεριλάβετε ευαίσθητα δεδομένα, όπως κωδικούς πρόσβασης απευθείας στο πρότυπο.

### <a name="add-private-template-to-storage-account"></a>Προσθήκη ιδιωτικό προτύπου με το λογαριασμό χώρου αποθήκευσης

Ακολουθήστε τα παρακάτω βήματα ρύθμιση ενός λογαριασμού χώρου αποθήκευσης για πρότυπα:

1. Δημιουργία μιας ομάδας πόρων.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Δημιουργία λογαριασμού χώρου αποθήκευσης. Το όνομα του λογαριασμού χώρου αποθήκευσης πρέπει να είναι μοναδικό σε Azure, ώστε να παρέχετε το δικό σας όνομα για το λογαριασμό.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Ορίστε τον τρέχοντα λογαριασμό χώρου αποθήκευσης.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Δημιουργία κοντέινερ. Το δικαίωμα έχει οριστεί σε **Απενεργοποίηση** γεγονός που σημαίνει ότι το κοντέινερ είναι διαθέσιμο μόνο για τον κάτοχο της.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. Προσθήκη του προτύπου στο κοντέινερ.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Παροχή διακριτικό συσχετισμών Ασφαλείας κατά τη διάρκεια της ανάπτυξης

Για να αναπτύξετε ένα ιδιωτικό πρότυπο σε ένα λογαριασμό του χώρου αποθήκευσης, ανάκτηση ενός διακριτικού συσχετισμών Ασφαλείας και συμπεριλαμβάνει στο URI για το πρότυπο.

1. Εάν έχετε αλλάξει τον τρέχοντα λογαριασμό χώρου αποθήκευσης, ορίστε τον τρέχοντα λογαριασμό χώρου αποθήκευσης σε εκείνο που περιέχει τα πρότυπά σας.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Δημιουργία ενός διακριτικού συσχετισμών Ασφαλείας με δικαιώματα ανάγνωσης και την ώρα λήξης για να περιορίσετε την πρόσβαση. Ανακτήστε το πλήρες URI του προτύπου όπως το διακριτικό συσχετισμών Ασφαλείας.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. Ανάπτυξη του προτύπου, παρέχοντας το URI που περιλαμβάνει το διακριτικό συσχετισμών Ασφαλείας.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Για ένα παράδειγμα της χρήσης ενός διακριτικού συσχετισμών Ασφαλείας με συνδεδεμένα πρότυπα, ανατρέξτε στο θέμα [χρήση συνδεδεμένων προτύπων με τη διαχείριση πόρων Azure](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="parameter-precedence"></a>Παράμετρος προτεραιότητας

Μπορείτε να χρησιμοποιήσετε ενσωματωμένα παραμέτρους και ένα αρχείο τοπικής παραμέτρου με την ίδια λειτουργία ανάπτυξη. Για παράδειγμα, μπορείτε να καθορίσετε ορισμένες τιμές στο αρχείο τοπικής παραμέτρου και να προσθέσετε άλλα ενσωματωμένες τιμές κατά την ανάπτυξη. Εάν μπορείτε να παράσχετε τιμές για μια παράμετρο στην παράμετρο τοπικό αρχείο και ενσωμάτωση, η τιμή ενσωματωμένη έχει προτεραιότητα.

Ωστόσο, δεν μπορείτε να χρησιμοποιήσετε τις παραμέτρους ενσωματωμένη με ένα αρχείο εξωτερικών παραμέτρου. Όταν ορίζετε ένα αρχείο παραμέτρων στην παράμετρο **TemplateParameterUri** , παραβλέπονται όλες τις παραμέτρους ενσωματωμένη. Πρέπει να παρέχετε όλες τις τιμές παραμέτρων στο εξωτερικό αρχείο. Εάν το πρότυπό σας περιλαμβάνει μια τιμή διάκριση πεζών-κεφαλαίων που δεν μπορείτε να συμπεριλάβετε στο αρχείο παραμέτρων, προσθέστε αυτήν την τιμή ενός κλειδιού θάλαμο και αναφορά του κλειδιού θάλαμο στο αρχείο σας εξωτερικών παράμετρο, ή δυναμικά παρέχουν όλες τις ενσωματωμένες τιμές παραμέτρων.

Για λεπτομέρειες σχετικά με τη χρήση μιας αναφοράς KeyVault για τη μεταβίβαση ασφαλούς τιμών, ανατρέξτε στο θέμα [μεταβιβάζουν ασφαλούς τιμές κατά την ανάπτυξη](resource-manager-keyvault-parameter.md).

## <a name="next-steps"></a>Επόμενα βήματα
- Για ένα παράδειγμα ανάπτυξης πόρους μέσω της βιβλιοθήκης του προγράμματος-πελάτη .NET, ανατρέξτε στο θέμα [Χρήση πόρους ανάπτυξης των βιβλιοθηκών .NET και ένα πρότυπο](virtual-machines/virtual-machines-windows-csharp-template.md).
- Για να ορίσετε τις παραμέτρους στο πρότυπο, ανατρέξτε στο θέμα [πρότυπα σύνταξη από κοινού](resource-group-authoring-templates.md#parameters).
- Για οδηγίες σχετικά με την ανάπτυξη της λύσης σε διαφορετικά περιβάλλοντα, ανατρέξτε στο θέμα [ανάπτυξη και περιβάλλοντα δοκιμής στο Microsoft Azure](solution-dev-test-environments.md).

