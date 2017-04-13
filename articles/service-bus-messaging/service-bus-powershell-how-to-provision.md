<properties
    pageTitle="Διαχείριση υπηρεσίας Bus με το PowerShell | Microsoft Azure"
    description="Διαχείριση υπηρεσίας Bus με δεσμών ενεργειών του PowerShell"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="sethm"/>

# <a name="manage-service-bus-with-powershell"></a>Διαχείριση υπηρεσίας Bus με το PowerShell

## <a name="overview"></a>Επισκόπηση

Microsoft Azure PowerShell είναι ένα περιβάλλον δέσμης ενεργειών που μπορείτε να χρησιμοποιήσετε για να ελέγξετε και να αυτοματοποιήσετε την ανάπτυξη και τη διαχείριση της σας φόρτους εργασίας στο Azure. Σε αυτό το άρθρο περιγράφει τον τρόπο χρήσης του PowerShell για παροχή και διαχείριση υπηρεσίας Bus οντοτήτων όπως οι χώροι ονομάτων, ουρές και διανομείς συμβάντος με χρήση μιας τοπικής κονσόλας Azure PowerShell.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τις εξής προϋποθέσεις:

- Μια συνδρομή του Azure. Azure είναι μια πλατφόρμα που βασίζεται σε συνδρομή. Για περισσότερες πληροφορίες σχετικά με την απόκτηση μια συνδρομή, ανατρέξτε στο θέμα [Επιλογές αγοράς][], [Προσφέρει μέλος][]ή [Δωρεάν δοκιμαστικής έκδοσης][].

- Ένας υπολογιστής με το Azure PowerShell. Για οδηγίες, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure][].

- Μια γενική Κατανόηση των δεσμών ενεργειών του PowerShell, των πακέτων NuGet και το .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Όπως μια αναφορά σε συγκρότησης .NET για Bus υπηρεσίας

Υπάρχουν διαθέσιμες για τη Διαχείριση υπηρεσίας Bus περιορισμένο αριθμό cmdlet του PowerShell. Να παρέχετε οντοτήτων που δεν είναι εκτεθειμένη έως τα cmdlet του υπάρχοντος, μπορείτε να χρησιμοποιήσετε το πρόγραμμα-πελάτη .NET για Bus υπηρεσίας στο [πακέτο NuGet Bus υπηρεσίας][].

Πρώτα, βεβαιωθείτε ότι η δέσμη ενεργειών να εντοπίσετε συγκρότησης **Microsoft.ServiceBus.dll** , στην οποία έχει εγκατασταθεί με το πακέτο NuGet. Προκειμένου να είναι ευέλικτη, τη δέσμη ενεργειών πραγματοποιεί τα εξής βήματα:

1. Καθορίζει τη διαδρομή στο οποίο έγινε κλήση.
2. Διέλευσης ο τη διαδρομή, μέχρι να εντοπίσει ένα φάκελο που ονομάζεται `packages`. Αυτός ο φάκελος δημιουργείται κατά την εγκατάσταση του NuGet πακέτων.
3. Αναζητήσεις σταδιακά το `packages` φακέλου για μιας συγκρότησης με το όνομα **Microsoft.ServiceBus.dll**.
4. Αναφορές συγκρότησης, έτσι ώστε οι τύποι είναι διαθέσιμοι για μελλοντική χρήση.

Δείτε τώρα πώς αυτά τα βήματα σχετικά με την εφαρμογή σε μια δέσμη ενεργειών του PowerShell:

```
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>Παροχή ένα χώρο ονομάτων Bus υπηρεσίας

Δύο cmdlet του PowerShell υποστηρίζει λειτουργίες χώρος ονομάτων Bus υπηρεσίας. Αντί για τα API SDK .NET, μπορείτε να χρησιμοποιήσετε [Get-AzureSBNamespace][] και [Δημιουργία AzureSBNamespace][].

Αυτό το παράδειγμα δημιουργεί μερικά τοπικές μεταβλητές στη δέσμη ενεργειών; `$Namespace` and `$Location`.

- `$Namespace`είναι το όνομα του πεδίου υπηρεσίας Bus ονομάτων με τον οποίο θέλετε να εργαστείτε.
- `$Location`Προσδιορίζει το κέντρο δεδομένων με την οποία η δέσμη ενεργειών διατάξεις χώρου ονομάτων.
- `$CurrentNamespace`αποθηκεύει το πεδίο ονόματος αναφοράς που τη δέσμη ενεργειών ανακτά (ή δημιουργεί).

Σε μια πραγματική δέσμη ενεργειών, `$Namespace` και `$Location` μπορούν να περάσουν ως παράμετροι.

Αυτό το τμήμα της δέσμης ενεργειών εκτελεί τις ακόλουθες εργασίες:

1. Προσπαθεί να ανακτήσετε ένα χώρο ονομάτων Bus υπηρεσίας με το όνομα που πληκτρολογήθηκε.
2. Εάν βρεθεί ο χώρος ονομάτων, αναφορές ό, τι βρέθηκε.
3. Εάν δεν εντοπιστεί το χώρο ονομάτων, δημιουργεί το χώρο ονομάτων και κατόπιν ανακτά το χώρο ονομάτων που μόλις δημιουργήθηκε.

    ```
    $Namespace = "MyServiceBusNS"
    $Location = "West US"
    
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
    
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Για να προμηθεύσουν άλλων οντοτήτων Bus υπηρεσίας, δημιουργήστε μια παρουσία της κλάσης [NamespaceManager][] από το SDK.
Μπορείτε να χρησιμοποιήσετε το cmdlet [Get-AzureSBAuthorizationRule][] για να ανακτήσετε ένας κανόνας εξουσιοδότησης που χρησιμοποιείται για την παροχή μια συμβολοσειρά σύνδεσης. Θα αποθηκεύει μια αναφορά σε το `NamespaceManager` παρουσίας στο το `$NamespaceManager` μεταβλητή. Θα χρησιμοποιήσουμε `$NamespaceManager` αργότερα στη δέσμη ενεργειών για την προμήθεια άλλων οντοτήτων.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Προμήθεια άλλων οντοτήτων Bus υπηρεσίας

Προκειμένου να προμηθεύσουν άλλων οντοτήτων, όπως ουρές, θέματα και διανομείς συμβάντος, χρησιμοποιήστε το [.NET API για Bus υπηρεσίας][]. Σε αυτό το άρθρο εστιάζει μόνο σε διανομείς συμβάντος, αλλά τα βήματα για άλλα οντοτήτων είναι παρόμοια. Επιπλέον, πιο λεπτομερή παραδείγματα, συμπεριλαμβανομένων των άλλων οντοτήτων, αναφέρονται στο τέλος αυτού του άρθρου.

Αυτό το τμήμα της δέσμης ενεργειών δημιουργεί τέσσερις περισσότερες τοπικές μεταβλητές. Αυτές οι μεταβλητές χρησιμοποιούνται για τη δημιουργία μιας `EventHubDescription` αντικειμένου. Η δέσμη ενεργειών εκτελεί τις ακόλουθες εργασίες:

1. Χρήση του `NamespaceManager` αντικείμενο, για να ελέγξετε εάν το συμβάν διανομέα που προσδιορίζονται με `$Path` υπάρχει.
2. Εάν δεν υπάρχει, δημιουργήστε μια `EventHubDescription` και μεταβιβάζουν που για να το `NamespaceManager` κλάσης `CreateEventHubIfNotExists` μέθοδο.
3. Μετά τον καθορισμό ότι υπάρχει ενότητα συμβάντων, δημιουργήστε μια ομάδα καταναλωτή με χρήση `ConsumerGroupDescription` και `NamespaceManager`.

    ```
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }
        
    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>Μετεγκατάσταση ένα χώρο ονομάτων με μια άλλη συνδρομή Azure

Το παρακάτω ακολουθίας εντολών μετακινείται ένα χώρο ονομάτων από μία συνδρομές στο Azure στον άλλο. Για να εκτελέσετε αυτήν τη λειτουργία, ο χώρος ονομάτων πρέπει να είναι ήδη ενεργή και ο χρήστης που εκτελεί τις εντολές του PowerShell πρέπει να είστε διαχειριστής σε συνδρομές προέλευσης και προορισμού.

```
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο παρέχεται μια βασική διάρθρωση για παροχή υπηρεσίας Bus οντοτήτων με χρήση του PowerShell. Οτιδήποτε άλλο μπορείτε να το κάνετε με τις βιβλιοθήκες προγράμματος-πελάτη .NET, μπορείτε επίσης να κάνετε σε μια δέσμη ενεργειών PowerShell.

Πιο λεπτομερή παραδείγματα είναι διαθέσιμες σε αυτές τις καταχωρήσεις ιστολογίου:

- [Πώς μπορείτε να δημιουργήσετε ουρές Bus υπηρεσίας, θέματα και συνδρομές χρησιμοποιώντας μια δέσμη ενεργειών PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Πώς μπορείτε να δημιουργήσετε μια υπηρεσία Namespace Bus και ένα συμβάν διανομέα, χρησιμοποιώντας μια δέσμη ενεργειών PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Ορισμένες έτοιμων δέσμες ενεργειών είναι επίσης διαθέσιμο για λήψη:
- [Υπηρεσία Bus δεσμών ενεργειών του PowerShell](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Επιλογές αγοράς]: http://azure.microsoft.com/pricing/purchase-options/
[Μέλος προσφορών]: http://azure.microsoft.com/pricing/member-offers/
[Δωρεάν δοκιμαστική έκδοση]: http://azure.microsoft.com/pricing/free-trial/
[Εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure]: ../powershell-install-configure.md
[Υπηρεσία Bus NuGet πακέτου]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Νέα AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[API .NET για Bus υπηρεσίας]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.aspx
[NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
