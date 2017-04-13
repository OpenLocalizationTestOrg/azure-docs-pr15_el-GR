<properties
    pageTitle="Χρήση του PowerShell για τη διαχείριση πόρων Bus υπηρεσίας και διανομείς συμβάν | Microsoft Azure"
    description="Χρήση του PowerShell για να δημιουργήσετε και να διαχειριστείτε πόρους Bus υπηρεσίας και διανομείς συμβάντος"
    services="service-bus,event-hubs"
    documentationCenter=".NET"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="use-powershell-to-manage-service-bus-and-event-hubs-resources"></a>Χρήση του PowerShell για τη Διαχείριση υπηρεσίας Bus και διανομείς συμβάν πόρων

Microsoft Azure PowerShell είναι ένα περιβάλλον δέσμης ενεργειών που μπορείτε να χρησιμοποιήσετε για να ελέγξετε και να αυτοματοποιήσετε την ανάπτυξη και τη διαχείριση των υπηρεσιών Azure. Σε αυτό το άρθρο περιγράφει τον τρόπο χρήσης του PowerShell για παροχή και διαχείριση υπηρεσίας Bus οντοτήτων όπως οι χώροι ονομάτων, ουρές και διανομείς συμβάντος με χρήση μιας τοπικής κονσόλας Azure PowerShell.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε, θα χρειαστείτε τα εξής:

- Μια συνδρομή του Azure. Azure είναι μια πλατφόρμα που βασίζεται σε συνδρομή. Για περισσότερες πληροφορίες σχετικά με την απόκτηση μια συνδρομή, ανατρέξτε στο θέμα [Επιλογές αγοράς][], [προσφέρει μέλος][]ή [δωρεάν λογαριασμό][].

- Ένας υπολογιστής με το Azure PowerShell. Για οδηγίες, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure][].

- Μια γενική Κατανόηση των δεσμών ενεργειών του PowerShell, των πακέτων NuGet και το .NET Framework.

## <a name="include-a-reference-to-the-net-assembly-for-service-bus"></a>Συμπεριλάβετε μια αναφορά σε συγκρότησης .NET για Bus υπηρεσίας

Υπάρχουν διαθέσιμες για τη Διαχείριση υπηρεσίας Bus περιορισμένο αριθμό cmdlet του PowerShell. Η παροχή οντοτήτων που δεν εκτίθενται έως τα υπάρχοντα cmdlet, μπορείτε να χρησιμοποιήσετε το πρόγραμμα-πελάτη .NET για Bus υπηρεσίας από μέσα του PowerShell με την αναφορά του [πακέτου NuGet Bus υπηρεσίας].

Πρώτα, βεβαιωθείτε ότι η δέσμη ενεργειών να εντοπίσετε συγκρότησης **Microsoft.ServiceBus.dll** , στην οποία έχει εγκατασταθεί με το πακέτο NuGet. Προκειμένου να είναι ευέλικτη, η δέσμη ενεργειών πραγματοποιεί τα εξής βήματα:

1. Καθορίζει τη διαδρομή από το οποίο να έχει ενεργοποιηθεί.
2. Διέλευσης ο τη διαδρομή, μέχρι να εντοπίσει ένα φάκελο που ονομάζεται `packages`. Αυτός ο φάκελος δημιουργείται κατά την εγκατάσταση του NuGet πακέτων.
3. Αναζητήσεις σταδιακά το `packages` φακέλου για μιας συγκρότησης με το όνομα **Microsoft.ServiceBus.dll**.
4. Αναφορές συγκρότησης, έτσι ώστε οι τύποι είναι διαθέσιμοι για μελλοντική χρήση.

Δείτε τώρα πώς αυτά τα βήματα σχετικά με την εφαρμογή σε μια δέσμη ενεργειών του PowerShell:

```powershell

try
{
    # IMPORTANT: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}

```

## <a name="provision-a-service-bus-namespace"></a>Παροχή ένα χώρο ονομάτων Bus υπηρεσίας

Όταν εργάζεστε με πεδία ονομάτων Bus υπηρεσίας, υπάρχουν δύο cmdlet του μπορείτε να χρησιμοποιήσετε αντί για το .NET SDK: [Get-AzureSBNamespace][] και [Δημιουργία AzureSBNamespace][].

Αυτό το παράδειγμα δημιουργεί μερικά τοπικές μεταβλητές στη δέσμη ενεργειών; `$Namespace` and `$Location`.

- `$Namespace`είναι το όνομα του του χώρου ονομάτων Bus υπηρεσίας με την οποία θέλετε να εργαστείτε.
- `$Location`Προσδιορίζει το κέντρο δεδομένων στην οποία θα σας θα παροχή του χώρου ονομάτων.
- `$CurrentNamespace`αποθηκεύει το πεδίο ονόματος αναφοράς που θα σας ανάκτηση (ή δημιουργία).

Σε μια πραγματική δέσμη ενεργειών, `$Namespace` και `$Location` μπορούν να περάσουν ως παράμετροι.

Αυτό το τμήμα της δέσμης ενεργειών κάνει τα εξής:

1. Προσπαθεί να ανακτήσετε ένα χώρο ονομάτων Bus υπηρεσίας με το όνομα που πληκτρολογήθηκε.
2. Εάν βρεθεί ο χώρος ονομάτων, αναφορές ό, τι βρέθηκε.
3. Εάν δεν εντοπιστεί το χώρο ονομάτων, δημιουργεί το χώρο ονομάτων και κατόπιν ανακτά το χώρο ονομάτων που μόλις δημιουργήθηκε.

    ``` powershell

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
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```
Για να προμηθεύσουν άλλων οντοτήτων Bus υπηρεσίας, δημιουργήστε μια παρουσία του το `NamespaceManager` αντικείμενο από το SDK. Μπορείτε να χρησιμοποιήσετε το cmdlet [Get-AzureSBAuthorizationRule][] για να ανακτήσετε ένας κανόνας εξουσιοδότησης που χρησιμοποιείται για την παροχή μια συμβολοσειρά σύνδεσης. Αυτό το παράδειγμα αποθηκεύει μια αναφορά σε το `NamespaceManager` παρουσίας στο το `$NamespaceManager` μεταβλητή. Η δέσμη ενεργειών χρησιμοποιεί αργότερα `$NamespaceManager` για την προμήθεια άλλων οντοτήτων.

    ``` powershell
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    # Create the NamespaceManager object to create the Event Hub
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
    ```

## <a name="provisioning-other-service-bus-entities"></a>Προμήθεια άλλων οντοτήτων Bus υπηρεσίας

Η παροχή άλλων οντοτήτων, όπως ουρές, θέματα και διανομείς συμβάντος, μπορείτε να χρησιμοποιήσετε το [.NET API για Bus υπηρεσίας][]. Πιο λεπτομερή παραδείγματα, συμπεριλαμβανομένων των άλλων οντοτήτων, αναφέρονται στο τέλος αυτού του άρθρου.

### <a name="create-an-event-hub"></a>Δημιουργήστε ένα διανομέα συμβάντος

Αυτό το τμήμα της δέσμης ενεργειών δημιουργεί τέσσερις περισσότερες τοπικές μεταβλητές. Αυτές οι μεταβλητές χρησιμοποιούνται για τη δημιουργία μιας `EventHubDescription` αντικειμένου. Η δέσμη ενεργειών κάνει τα εξής:

1. Χρήση του `NamespaceManager` αντικείμενο, για να ελέγξετε εάν το συμβάν διανομέα που προσδιορίζονται με `$Path` υπάρχει.
2. Εάν δεν υπάρχει, δημιουργήστε μια `EventHubDescription` και μεταβιβάζουν που για να το `NamespaceManager` κλάσης `CreateEventHubIfNotExists` μέθοδο.
3. Μετά τον καθορισμό ότι υπάρχει ενότητα συμβάντων, δημιουργήστε μια ομάδα καταναλωτή με χρήση `ConsumerGroupDescription` και `NamespaceManager`.

    ``` powershell

    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"

    # Check if the Event Hub already exists
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

### <a name="create-a-queue"></a>Δημιουργία ουράς

Για να δημιουργήσετε μια ουρά ή το θέμα, εκτελέστε ένα χώρο ονομάτων ελέγχου όπως στην προηγούμενη ενότητα.

    if ($NamespaceManager.QueueExists($Path))
    {
        Write-Output "The [$Path] queue already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] queue in the [$Namespace] namespace..."
        $QueueDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.QueueDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $QueueDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $QueueDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $QueueDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $QueueDescription.EnableBatchedOperations = $EnableBatchedOperations
        $QueueDescription.EnableDeadLetteringOnMessageExpiration = $EnableDeadLetteringOnMessageExpiration
        $QueueDescription.EnableExpress = $EnableExpress
        $QueueDescription.EnablePartitioning = $EnablePartitioning
        $QueueDescription.ForwardDeadLetteredMessagesTo = $ForwardDeadLetteredMessagesTo
        $QueueDescription.ForwardTo = $ForwardTo
        $QueueDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        if ($LockDuration -gt 0)
        {
            $QueueDescription.LockDuration = [System.TimeSpan]::FromSeconds($LockDuration)
        }
        $QueueDescription.MaxDeliveryCount = $MaxDeliveryCount
        $QueueDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $QueueDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        $QueueDescription.RequiresSession = $RequiresSession
        if ($EnablePartitioning)
        {
            $QueueDescription.SupportOrdering = $False
        }
        else
        {
            $QueueDescription.SupportOrdering = $SupportOrdering
        }
        $QueueDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateQueue($QueueDescription);
    }

### <a name="create-a-topic"></a>Δημιουργία θέματος

    if ($NamespaceManager.TopicExists($Path))
    {
        Write-Output "The [$Path] topic already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] topic in the [$Namespace] namespace..."
        $TopicDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.TopicDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $TopicDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $TopicDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $TopicDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $TopicDescription.EnableBatchedOperations = $EnableBatchedOperations
        $TopicDescription.EnableExpress = $EnableExpress
        $TopicDescription.EnableFilteringMessagesBeforePublishing = $EnableFilteringMessagesBeforePublishing
        $TopicDescription.EnablePartitioning = $EnablePartitioning
        $TopicDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        $TopicDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $TopicDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        if ($EnablePartitioning)
        {
            $TopicDescription.SupportOrdering = $False
        }
        else
        {
            $TopicDescription.SupportOrdering = $SupportOrdering
        }
        $TopicDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateTopic($TopicDescription);
    }

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο που διαθέτουν μια βασική ροή εργασίας για χρήση του PowerShell οντοτήτων Bus υπηρεσίας παροχής. Παρόλο που υπάρχουν περιορισμένο αριθμό cmdlet του PowerShell για τη Διαχείριση υπηρεσίας Bus μηνυμάτων οντοτήτων, με την αναφορά της συγκρότησης Microsoft.ServiceBus.dll, σχεδόν οποιαδήποτε εργασία μπορείτε να εκτελέσετε χρησιμοποιώντας τις βιβλιοθήκες του προγράμματος-πελάτη .NET μπορείτε επίσης να πραγματοποιήσετε σε μια δέσμη ενεργειών PowerShell.

Είναι διαθέσιμες σε αυτές τις καταχωρήσεις ιστολογίου πιο λεπτομερή παραδείγματα:

- [Πώς μπορείτε να δημιουργήσετε ουρές Bus υπηρεσίας, θέματα και συνδρομές χρησιμοποιώντας μια δέσμη ενεργειών PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Πώς μπορείτε να δημιουργήσετε μια υπηρεσία Namespace Bus και ένα συμβάν διανομέα, χρησιμοποιώντας μια δέσμη ενεργειών PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Ορισμένες έτοιμων δέσμες ενεργειών είναι επίσης διαθέσιμο για λήψη:

- [Υπηρεσία Bus δεσμών ενεργειών του PowerShell](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[επιλογές αγοράς]: http://azure.microsoft.com/pricing/purchase-options/
[μέλος προσφέρει]: http://azure.microsoft.com/pricing/member-offers/
[δωρεάν λογαριασμό]: http://azure.microsoft.com/pricing/free-trial/
[Υπηρεσία Bus NuGet πακέτου]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Νέα AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[API .NET για Bus υπηρεσίας]: https://msdn.microsoft.com/en-us/library/azure/mt419900.aspx
[Εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure]: ../powershell-install-configure.md
