<properties 
    pageTitle="Διαχείριση αναζήτησης Azure με δεσμών ενεργειών του Powershell | Microsoft Azure | Υπηρεσία αναζήτησης φιλοξενούμενη cloud" 
    description="Διαχείριση της υπηρεσίας αναζήτησης Azure με δεσμών ενεργειών του PowerShell. Δημιουργία ή ενημέρωση μιας υπηρεσίας αναζήτησης Azure και διαχείριση αναζήτησης Azure διαχείρισης κλειδιών" 
    services="search" 
    documentationCenter="" 
    authors="seansaleh" 
    manager="mblythe" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="search" 
    ms.devlang="na" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="powershell" 
    ms.date="08/15/2016" 
    ms.author="seasa"/>

# <a name="manage-your-azure-search-service-with-powershell"></a>Διαχείριση της υπηρεσίας αναζήτησης Azure με το PowerShell
> [AZURE.SELECTOR]
- [Πύλη](search-manage.md)
- [PowerShell](search-manage-powershell.md)
- [REST API](search-get-started-management-api.md)

Αυτό το θέμα περιγράφει τις εντολές του PowerShell για να εκτελέσετε πολλές από τις εργασίες διαχείρισης για τις υπηρεσίες του Azure αναζήτησης. Θα θα καθοδηγήσουμε έως τη δημιουργία μιας υπηρεσίας αναζήτησης, την κλίμακα του και τη Διαχείριση τα κλειδιά API.
Αυτές οι εντολές παράλληλο τις επιλογές διαχείρισης που είναι διαθέσιμες στο [Azure αναζήτησης διαχείρισης REST API](http://msdn.microsoft.com/library/dn832684.aspx).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
 
- Πρέπει να έχετε Azure PowerShell 1.0 ή μεγαλύτερη. Για οδηγίες, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../powershell-install-configure.md).
- Πρέπει να είστε συνδεδεμένοι Azure τη συνδρομή σας στο PowerShell όπως περιγράφεται παρακάτω.

Πρώτα, πρέπει να συνδεθείτε στο Azure με αυτήν την εντολή:

    Login-AzureRmAccount

Καθορίστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου του Azure το λογαριασμό σας και ο κωδικός πρόσβασης στο παράθυρο διαλόγου login Microsoft Azure.

Εναλλακτικά, μπορείτε να [login μη αλληλεπιδραστικά με μια υπηρεσία κεφαλαίου](../resource-group-authenticate-service-principal.md).

Εάν έχετε πολλές συνδρομές Azure, πρέπει να ορίσετε τη συνδρομή σας στο Azure. Για να δείτε μια λίστα με τις συνδρομές σας στο τρέχον, εκτέλεση αυτής της εντολής.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Για να καθορίσετε τη συνδρομή, εκτελέστε την ακόλουθη εντολή. Στο παρακάτω παράδειγμα, είναι το όνομα της συνδρομής `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Εντολές για να σας βοηθήσουν να ξεκινήσετε

    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search" -Force

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1
    
    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
    
    # View your resource
    $resource
    
    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key
    
    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
        
    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource
    
    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource
    
## <a name="next-steps"></a>Επόμενα βήματα
    
Τώρα που δημιουργείται της υπηρεσίας, που μπορεί να διαρκέσει τα επόμενα βήματα: Δημιουργήστε ένα [ευρετήριο](search-what-is-an-index.md), [ερωτήματος ένα ευρετήριο](search-query-overview.md), και τέλος να δημιουργήσετε και να διαχειριστείτε τις δικές σας εφαρμογής αναζήτησης που χρησιμοποιεί το Azure αναζήτησης.

- [Δημιουργία ευρετηρίου αναζήτησης Azure στην πύλη του Azure](search-create-index-portal.md)

- [Ερώτημα ευρετηρίου αναζήτησης Azure χρησιμοποιώντας την Εξερεύνηση αναζήτησης στην πύλη του Azure](search-explorer.md)

- [Ρύθμιση ένα ευρετήριο για να φορτώσετε δεδομένων από άλλες υπηρεσίες](search-indexer-overview.md)

- [Πώς μπορείτε να χρησιμοποιήσετε την αναζήτηση Azure στο .NET](search-howto-dotnet-sdk.md)

- [Ανάλυση κυκλοφορία σας Azure αναζήτησης](search-traffic-analytics.md)
