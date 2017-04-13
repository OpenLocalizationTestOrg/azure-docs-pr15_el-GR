<properties 
    pageTitle="Ανάπτυξη και διαχείριση ειδοποιήσεων διανομείς, με χρήση του PowerShell" 
    description="Πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε διανομείς ειδοποίηση με χρήση του PowerShell για αυτοματισμού" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Ανάπτυξη και διαχείριση ειδοποιήσεων διανομείς, με χρήση του PowerShell

##<a name="overview"></a>Επισκόπηση

Αυτό το άρθρο σας δείχνει πώς μπορείτε να χρησιμοποιήσετε δημιουργία και διαχείριση Azure ειδοποίηση διανομείς χρήση του PowerShell. Οι ακόλουθες κοινές εργασίες αυτοματισμού εμφανίζονται σε αυτό το θέμα.

+ Δημιουργήστε ένα διανομέα ειδοποίησης
+ Ορισμός διαπιστευτηρίων

Εάν χρειάζεστε για να δημιουργήσετε έναν νέο χώρο ονομάτων bus υπηρεσία για την ειδοποίηση διανομείς, ανατρέξτε στο θέμα [Διαχείριση υπηρεσίας Bus με το PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Διαχείριση ειδοποιήσεων διανομείς δεν υποστηρίζεται απευθείας από τα cmdlet περιλαμβάνεται με το Azure PowerShell. Είναι η καλύτερη προσέγγιση από το PowerShell για αναφορά συγκρότησης Microsoft.Azure.NotificationHubs.dll. Με το [Microsoft Azure ειδοποίηση διανομείς NuGet πακέτου](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)κατανέμεται συγκρότησης.


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- Μια συνδρομή του Azure. Azure είναι μια πλατφόρμα που βασίζεται σε συνδρομή. Για περισσότερες πληροφορίες σχετικά με την απόκτηση μια συνδρομή, ανατρέξτε στο θέμα [Επιλογές αγοράς], [Προσφέρει μέλος]ή [Δωρεάν δοκιμαστικής έκδοσης].

- Ένας υπολογιστής με το Azure PowerShell. Για οδηγίες, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure].

- Μια γενική Κατανόηση των δεσμών ενεργειών του PowerShell, των πακέτων NuGet και το .NET Framework.


## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Όπως μια αναφορά σε συγκρότησης .NET για Bus υπηρεσίας

Διαχείριση διανομείς ειδοποίηση Azure δεν είναι ακόμη περιλαμβάνεται με τα cmdlet του PowerShell στο Azure PowerShell. Να παρέχετε διανομείς ειδοποίηση, μπορείτε να χρησιμοποιήσετε το πρόγραμμα-πελάτη .NET που παρέχονται στο [Microsoft Azure ειδοποίηση διανομείς NuGet πακέτου](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Πρώτα, βεβαιωθείτε ότι η δέσμη ενεργειών σας μπορεί να εντοπίσει συγκρότησης **Microsoft.Azure.NotificationHubs.dll** , στην οποία έχει εγκατασταθεί ως ένα πακέτο NuGet σε ένα έργο Visual Studio. Προκειμένου να είναι ευέλικτη, τη δέσμη ενεργειών πραγματοποιεί τα εξής βήματα:

1. Καθορίζει τη διαδρομή στο οποίο έγινε κλήση.
2. Διέλευσης ο τη διαδρομή, μέχρι να εντοπίσει ένα φάκελο που ονομάζεται `packages`. Αυτός ο φάκελος δημιουργείται κατά την εγκατάσταση του NuGet πακέτων για τα έργα Visual Studio.
3. Αναζητήσεις σταδιακά το `packages` φακέλου για μιας συγκρότησης με το όνομα **Microsoft.Azure.NotificationHubs.dll**.
4. Αναφορές συγκρότησης, έτσι ώστε οι τύποι είναι διαθέσιμοι για μελλοντική χρήση.

Δείτε τώρα πώς αυτά τα βήματα σχετικά με την εφαρμογή σε μια δέσμη ενεργειών του PowerShell:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>Δημιουργήστε την κλάση NamespaceManager

Για να προμηθεύσουν διανομείς ειδοποίηση, δημιουργήστε μια παρουσία της κλάσης [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) από το SDK. 

Μπορείτε να χρησιμοποιήσετε το cmdlet [Get-AzureSBAuthorizationRule] περιλαμβάνεται με το Azure PowerShell για να ανακτήσετε ένας κανόνας εξουσιοδότησης που χρησιμοποιείται για την παροχή μια συμβολοσειρά σύνδεσης. Θα αποθηκεύει μια αναφορά σε το `NamespaceManager` παρουσίας στο το `$NamespaceManager` μεταβλητή. Θα χρησιμοποιήσουμε `$NamespaceManager` να παρέχετε διανομέα ειδοποίησης.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Προμήθεια διανομέα νέα ειδοποίηση 

Για να προμηθεύσουν διανομέα νέα ειδοποίηση, χρησιμοποιήστε το [.NET API για διανομείς ειδοποίησης].

Σε αυτό το τμήμα της δέσμης ενεργειών μπορείτε να ορίσετε τέσσερις τοπικές μεταβλητές. 

1. `$Namespace`: Ορίστε αυτό το όνομα του χώρου ονομάτων όπου θέλετε να δημιουργήσετε ένα διανομέα ειδοποίησης.
2. `$Path`: Ορίστε αυτή η διαδρομή ως το όνομα του νέου διανομέα ειδοποίηση.  Για παράδειγμα, "MyHub".    
3. `$WnsPackageSid`: Ορίστε αυτή στο πακέτο αναγνωριστικό ΑΣΦΑΛΕΊΑΣ για εφαρμογή Windows από το [Κέντρο ανάπτυξης των Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Ορίστε αυτή για να το μυστικό κλειδί για εφαρμογή Windows από το [Κέντρο ανάπτυξης των Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Αυτές οι μεταβλητές που χρησιμοποιούνται για να συνδεθείτε με το χώρο ονομάτων και δημιουργήστε ένα νέο διανομέα ειδοποίηση έχει ρυθμιστεί ώστε να χειριστείτε τις ειδοποιήσεις υπηρεσίες ειδοποιήσεων των Windows (WNS) με WNS διαπιστευτήρια για μια εφαρμογή για Windows. Για πληροφορίες σχετικά με τη λήψη του πακέτου αναγνωριστικό ΑΣΦΑΛΕΊΑΣ και μυστικό κλειδί ανατρέξτε στο θέμα, το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με ειδοποίηση διανομείς](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) . 

+ Το τμήμα κώδικα δέσμης ενεργειών χρησιμοποιεί το `NamespaceManager` αντικειμένου για να ελέγξετε για να δείτε εάν ο διανομέας ειδοποίησης που προσδιορίζονται με `$Path` υπάρχει.

+ Εάν δεν υπάρχει, θα δημιουργήσετε τη δέσμη ενεργειών μια `NotificationHubDescription` με WNS διαπιστευτήρια και μεταβιβάζουν που για να το `NamespaceManager` κλάσης `CreateNotificationHub` μέθοδο.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Διαχείριση υπηρεσίας Bus με το PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Πώς μπορείτε να δημιουργήσετε ουρές Bus υπηρεσίας, θέματα και συνδρομές χρησιμοποιώντας μια δέσμη ενεργειών PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Πώς μπορείτε να δημιουργήσετε μια υπηρεσία Namespace Bus και ένα συμβάν διανομέα, χρησιμοποιώντας μια δέσμη ενεργειών PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Ορισμένες έτοιμων δέσμες ενεργειών είναι επίσης διαθέσιμο για λήψη:
- [Υπηρεσία Bus δεσμών ενεργειών του PowerShell](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)
 

[Επιλογές αγοράς]: http://azure.microsoft.com/pricing/purchase-options/
[Μέλος προσφορών]: http://azure.microsoft.com/pricing/member-offers/
[Δωρεάν δοκιμαστική έκδοση]: http://azure.microsoft.com/pricing/free-trial/
[Εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure]: ../powershell-install-configure.md
[API .NET για διανομείς ειδοποίησης]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
 
