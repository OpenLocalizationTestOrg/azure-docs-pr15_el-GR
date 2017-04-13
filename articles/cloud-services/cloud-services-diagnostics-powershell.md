<properties
    pageTitle="Ενεργοποίηση Διαγνωστικά στις υπηρεσίες Cloud Azure χρησιμοποιώντας το PowerShell | Microsoft Azure"
    description="Μάθετε πώς να ενεργοποιείτε Διαγνωστικά για τις υπηρεσίες cloud χρησιμοποιώντας το PowerShell"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Ενεργοποίηση Διαγνωστικά στις υπηρεσίες Cloud Azure χρησιμοποιώντας το PowerShell

Μπορείτε να συλλέξετε διαγνωστικών δεδομένα όπως αρχεία καταγραφής εφαρμογών, μετρητής επιδόσεων κ.λπ., από μια υπηρεσία στο Cloud χρησιμοποιώντας την επέκταση Διαγνωστικά Azure. Σε αυτό το άρθρο περιγράφει τον τρόπο για να ενεργοποιήσετε την επέκταση Διαγνωστικά του Azure για μια υπηρεσία στο Cloud χρησιμοποιώντας το PowerShell.  Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για τις προϋποθέσεις που χρειάζονται για αυτό το άρθρο.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Ενεργοποίηση της επέκτασης Διαγνωστικά ως μέρος του για την ανάπτυξη μια υπηρεσία στο Cloud

Αυτή η προσέγγιση του καλή για τον τύπο συνεχής ενοποίησης σενάρια όπου μπορεί να ενεργοποιηθεί η επέκταση Διαγνωστικά ως μέρος του για την ανάπτυξη της υπηρεσίας cloud. Κατά τη δημιουργία μιας νέας ανάπτυξης υπηρεσία Cloud, μπορείτε να ενεργοποιήσετε την επέκταση Διαγνωστικά περνώντας στην παράμετρο *ExtensionConfiguration* στο cmdlet [New-AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) . Η παράμετρος *ExtensionConfiguration* χρειάζονται ένας πίνακας με ρυθμίσεις παραμέτρων διαγνωστικά που μπορούν να δημιουργηθούν με τα cmdlet [New-AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) .

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να ενεργοποιήσετε την Διαγνωστικά για μια υπηρεσία στο cloud με WebRole και WorkerRole κάθε χρειάζεται μια ρύθμιση παραμέτρων διαφορετική Διαγνωστικά.

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

Εάν το αρχείο ρύθμισης παραμέτρων Διαγνωστικά καθορίζει ένα στοιχείο StorageAccount με ένα όνομα λογαριασμού χώρου αποθήκευσης, στη συνέχεια, το cmdlet New-AzureServiceDiagnosticsExtensionConfig θα χρησιμοποιεί αυτόματα αυτόν το λογαριασμό χώρου αποθήκευσης. Για να γίνει αυτό, το λογαριασμό χώρου αποθήκευσης πρέπει να είναι στην ίδια συνδρομή του ως την υπηρεσία Cloud αναπτύσσεται.

Από το Azure SDK 2.6 και μετά τα αρχεία ρύθμισης παραμέτρων επέκτασης που δημιουργούνται από το MSBuild δημοσίευση προορισμός εξόδου θα περιλαμβάνει το όνομα του λογαριασμού χώρου αποθήκευσης που βασίζεται σε διαγνωστικά ρύθμισης παραμέτρων συμβολοσειράς που καθορίζεται στο αρχείο ρύθμισης παραμέτρων της υπηρεσίας (.cscfg). Την παρακάτω δέσμη ενεργειών που δείχνει πώς μπορείτε να τα αρχεία ρύθμισης παραμέτρων επέκταση από τη δημοσίευση προορισμός εξόδου ανάλυση και ρύθμιση παραμέτρων επέκταση Διαγνωστικά για κάθε ρόλο κατά την ανάπτυξη την υπηρεσία cloud.

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

Visual Studio Online χρησιμοποιεί μια παρόμοια προσέγγιση για την αυτοματοποιημένη deploymnts των υπηρεσιών Cloud με την επέκταση Διαγνωστικά. Ανατρέξτε στο θέμα [Δημοσίευση AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) για ένα παράδειγμα ολοκλήρωσης.

Εάν δεν υπάρχει StorageAccount έχει καθοριστεί στη ρύθμιση παραμέτρων Διαγνωστικά, στη συνέχεια, πρέπει να μεταβιβάζουν στην παράμετρο StorageAccountName για το cmdlet. Εάν η παράμετρος StorageAccountName έχει οριστεί, στη συνέχεια, το cmdlet θα χρησιμοποιεί πάντα το λογαριασμό χώρου αποθήκευσης που καθορίζεται από την παράμετρο και δεν εκείνο που καθορίζεται στο αρχείο παραμέτρων Διαγνωστικά.

Εάν ο λογαριασμός Διαγνωστικά του χώρου αποθήκευσης είναι σε μια διαφορετική συνδρομή από την υπηρεσία Cloud, στη συνέχεια, πρέπει να μεταβιβάζουν ρητά στις παραμέτρους StorageAccountName και StorageAccountKey το cmdlet. Η παράμετρος StorageAccountKey δεν είναι απαραίτητος κατά το λογαριασμό χώρου αποθήκευσης Διαγνωστικά είναι στην ίδια συνδρομή του, όπως το cmdlet αυτόματα να ερωτήματος και να ορίσετε την τιμή του κλειδιού κατά την ενεργοποίηση του την επέκταση Διαγνωστικά. Ωστόσο, εάν το λογαριασμό χώρου αποθήκευσης Διαγνωστικά βρίσκεται σε μια διαφορετική συνδρομή, στη συνέχεια, το cmdlet ίσως να μην μπορείτε να πραγματοποιήσετε αυτόματη λήψη τον αριθμό-κλειδί και πρέπει να καθορίσετε τον αριθμό-κλειδί μέσω της παραμέτρου StorageAccountKey ρητά.

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Ενεργοποίηση της επέκτασης Διαγνωστικά σε μια υπάρχουσα υπηρεσία Cloud

Μπορείτε να χρησιμοποιήσετε το cmdlet [Set-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) για να ενεργοποιήσετε ή να ενημερώσετε Διαγνωστικά ρύθμισης παραμέτρων σε μια υπηρεσία Cloud που εκτελείται ήδη.


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>Λήψη τρέχουσα ρύθμιση παραμέτρων επέκταση Διαγνωστικά
Χρησιμοποιήστε το cmdlet [Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) για να λάβετε την τρέχουσα ρύθμιση παραμέτρων διαγνωστικών για μια υπηρεσία στο cloud.

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>Κατάργηση Διαγνωστικά επέκτασης
Για να απενεργοποιήσετε τα Διαγνωστικά σε μια υπηρεσία cloud, μπορείτε να χρησιμοποιήσετε το cmdlet [Κατάργηση AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589183.aspx) .

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Εάν έχετε ενεργοποιήσει την επέκταση Διαγνωστικά χρησιμοποιώντας το *Σύνολο AzureServiceDiagnosticsExtension* ή τη *Δημιουργία AzureServiceDiagnosticsExtensionConfig* χωρίς την παράμετρο *ρόλο* , στη συνέχεια, μπορείτε να καταργήσετε την επέκταση με *Κατάργηση AzureServiceDiagnosticsExtension* χωρίς την παράμετρο *ρόλο* . Εάν η παράμετρος *ρόλο* που χρησιμοποιήθηκε κατά την ενεργοποίηση την επέκταση, στη συνέχεια, πρέπει να επίσης να χρησιμοποιηθεί κατά την κατάργηση της επέκτασης.

Για να καταργήσετε την επέκταση Διαγνωστικά από κάθε μεμονωμένα ρόλο:

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>Επόμενα βήματα

- Για πρόσθετες οδηγίες σχετικά με τη χρήση Azure Διαγνωστικά και άλλες τεχνικές για την αντιμετώπιση προβλημάτων, ανατρέξτε στο θέμα [Ενεργοποίηση Διαγνωστικά στις υπηρεσίες Cloud Azure και εικονικές μηχανές](cloud-services-dotnet-diagnostics.md).
- Το [Σχήμα της ομάδας παραμέτρων Διαγνωστικά](https://msdn.microsoft.com/library/azure/dn782207.aspx) εξηγεί τις διάφορες επιλογές ρυθμίσεις παραμέτρων xml για την επέκταση Διαγνωστικά.
- Για να μάθετε πώς μπορείτε να ενεργοποιήσετε την επέκταση Διαγνωστικά για εικονικές μηχανές, ανατρέξτε στο θέμα [Δημιουργία εικονικών Windows μηχάνημα με παρακολούθηση και τα Διαγνωστικά χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  
