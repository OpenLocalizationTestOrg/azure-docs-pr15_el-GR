<properties
   pageTitle="Ανάπτυξη εφαρμογών υπηρεσίας ύφασμα | Microsoft Azure"
   description="Πώς μπορείτε να αναπτύξετε και να καταργήσετε εφαρμογές σε ύφασμα υπηρεσίας"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="deploy-and-remove-applications-using-powershell"></a>Ανάπτυξη και κατάργηση εφαρμογών με χρήση του PowerShell

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Μία φορά ένα [τύπο εφαρμογής μετατράπηκε σε μορφή πακέτου][10], είναι έτοιμο για ανάπτυξη σε ένα σύμπλεγμα ύφασμα υπηρεσίας Azure. Ανάπτυξη περιλαμβάνει τα ακόλουθα τρία βήματα:

1. Αποστείλετε το πακέτο εφαρμογών
2. Καταχωρήστε τον τύπο εφαρμογής
3. Δημιουργήστε την παρουσία της εφαρμογής

>[AZURE.NOTE] Εάν χρησιμοποιείτε το Visual Studio για την ανάπτυξη και τον εντοπισμό σφαλμάτων εφαρμογών σε το σύμπλεγμά σας τοπικής ανάπτυξης, όλα τα ακόλουθα βήματα αντιμετωπίζονται αυτόματα μέσω μιας δέσμης ενεργειών του PowerShell βρίσκεται στο φάκελο δεσμών ενεργειών του έργου εφαρμογής. Σε αυτό το άρθρο παρέχει φόντου σε τι αυτές τις δέσμες ενεργειών κάνουν έτσι ώστε να μπορείτε να εκτελέσετε τις ίδιες λειτουργίες εκτός του Visual Studio.

## <a name="upload-the-application-package"></a>Αποστείλετε το πακέτο εφαρμογών

Αποστολή του πακέτου εφαρμογών το τοποθετεί σε μια θέση που είναι προσβάσιμο από εσωτερικά στοιχεία ύφασμα υπηρεσίας. Μπορείτε να χρησιμοποιήσετε PowerShell για να εκτελέσετε την αποστολή. Πριν να εκτελέσετε τις εντολές του PowerShell σε αυτό το άρθρο, ξεκινούν πάντα με τη χρήση [ServiceFabricCluster σύνδεση](https://msdn.microsoft.com/library/mt125938.aspx) για να συνδεθείτε με το σύμπλεγμα ύφασμα υπηρεσίας.

Ας υποθέσουμε ότι έχετε ένα φάκελο που ονομάζεται *MyApplicationType* που περιέχει τη δήλωση εφαρμογής είναι απαραίτητο, υπηρεσία δηλώσεων και πακέτα κώδικα / / περιεχομένου δεδομένων ρύθμισης. Η εντολή [Αντιγραφή ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) αποστέλλει το πακέτο στο σύμπλεγμα εικόνα αποθήκευσης. Το cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** , που είναι μέρος της λειτουργικής μονάδας υπηρεσίας ύφασμα SDK PowerShell, χρησιμοποιείται για να λάβετε τη συμβολοσειρά σύνδεσης store εικόνα.  Για να εισαγάγετε τη λειτουργική μονάδα SDK, εκτελέστε:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Μπορείτε να αντιγράψετε ένα πακέτο εφαρμογών από *C:\users\ryanwi\Documents\Visual Studio 2015\Projects\MyApplication\myapplication\pkg\debug* *c:\temp\MyApplicationType* (rename τον κατάλογο "Εντοπισμός σφαλμάτων" σε "MyApplicationType"). Το παρακάτω παράδειγμα αποστέλλει το πακέτο:

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
  	|   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
  	|         Microsoft.ServiceFabric.Internal.Strings.resources.dll
  	|         System.Fabric.Strings.resources.dll
  	|
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## <a name="register-the-application-package"></a>Καταχώρηση του πακέτου εφαρμογών

Εγγραφή για το πακέτο εφαρμογών κάνει τον τύπο εφαρμογής και την έκδοση που έχουν δηλωθεί στο δηλωτικό εφαρμογή διαθέσιμη για χρήση. Το σύστημα διαβάζει το πακέτο που έχουν αποσταλεί στο προηγούμενο βήμα, επιβεβαιώστε το πακέτο (που είναι ισοδύναμη με την εκτέλεση [Δοκιμής ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) τοπικά), η επεξεργασία των περιεχομένων του πακέτου και αντιγράψτε το πακέτο επεξεργασία σε μια θέση εσωτερικής συστήματος.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

Η εντολή [Register-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) επιστρέφει μόνο αφού το σύστημα έχει αντιγραφεί με επιτυχία το πακέτο εφαρμογών. Πόσος χρόνος ενέργεια αυτή σας μεταφέρει εξαρτάται από τα περιεχόμενα του πακέτου εφαρμογών. Εάν είναι απαραίτητο, την παράμετρο **- TimeoutSec** μπορεί να χρησιμοποιηθεί για την παροχή ενός μεγαλύτερου χρονικού ορίου. (Το προεπιλεγμένο χρονικό όριο είναι 60 δευτερόλεπτα).

Η εντολή [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) παραθέτει όλες τις εκδόσεις τύπου με επιτυχία καταχωρημένη εφαρμογή.

## <a name="create-the-application"></a>Δημιουργία της εφαρμογής

Που μπορεί να εμφανίσει μια εφαρμογή, χρησιμοποιώντας οποιαδήποτε έκδοση τύπο εφαρμογής που έχει καταχωρηθεί με επιτυχία με την εντολή [Δημιουργία ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) . Το όνομα της εφαρμογής κάθε πρέπει να ξεκινά με το *ύφασμα:* συνδυασμού και είναι μοναδικός για κάθε παρουσία της εφαρμογής. Οποιαδήποτε προεπιλεγμένες υπηρεσίες που ορίζονται από το στο δηλωτικό εφαρμογής από τον τύπο εφαρμογής-στόχου δημιουργούνται αυτήν τη στιγμή.

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

Η εντολή [Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) παραθέτει όλες τις εμφανίσεις εφαρμογής που δημιουργήθηκαν με επιτυχία, μαζί με τις συνολικές κατάστασης.

Η εντολή [Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) παραθέτει όλες τις εμφανίσεις υπηρεσίας που δημιουργήθηκαν με επιτυχία μέσα σε μια παρουσία της δεδομένης εφαρμογής. Προεπιλεγμένες υπηρεσίες (εάν υπάρχουν) παρατίθενται εδώ.

Πολλών παρουσιών της εφαρμογής μπορούν να δημιουργηθούν για οποιαδήποτε δεδομένη έκδοση ενός τύπου καταχωρημένη εφαρμογή. Κάθε παρουσία της εφαρμογής εκτελείται μεμονωμένα, με τη δική του καταλόγου εργασίας και διαδικασία.

## <a name="remove-an-application"></a>Κατάργηση μιας εφαρμογής

Όταν μια παρουσία της εφαρμογής δεν είναι πλέον απαραίτητη, μπορείτε να την καταργήσετε μόνιμα, χρησιμοποιώντας την εντολή [Κατάργηση ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) . Αυτή η εντολή καταργεί αυτόματα όλες τις υπηρεσίες που ανήκουν σε καθώς και την εφαρμογή οριστική κατάργηση όλη την κατάσταση υπηρεσίας. Αυτή η λειτουργία δεν μπορεί να αντιστραφεί και είναι δυνατή η ανάκτησή κατάσταση εφαρμογής.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Όταν μια συγκεκριμένη έκδοση του ένας τύπος εφαρμογής δεν είναι πλέον απαραίτητη, που θα πρέπει να το unregister χρησιμοποιώντας την εντολή [Unregister ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) . Κατάργηση καταχώρησης τύπους που δεν χρησιμοποιούνται εκδόσεις χώρου αποθήκευσης που χρησιμοποιείται από τα περιεχόμενα του πακέτου εφαρμογής αυτού του τύπου στο χώρο αποθήκευσης της εικόνας. Ένας τύπος εφαρμογής μπορεί να είναι μη καταχωρημένες με την προϋπόθεση ότι δεν υπάρχουν εφαρμογές δημιουργούνται σε σχέση με το και όχι σε εκκρεμότητα εφαρμογή αναβαθμίσεις αναφέρονται σε αυτήν.

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Αντιγραφή ServiceFabricApplicationPackage ζητά ένα ImageStoreConnectionString

Το περιβάλλον υπηρεσίας ύφασμα SDK πρέπει να έχετε ήδη τις προεπιλογές σωστή ρύθμιση. Αλλά, εάν είναι απαραίτητο, το ImageStoreConnectionString για όλες τις εντολές θα πρέπει να συμφωνεί με την τιμή που χρησιμοποιεί το σύμπλεγμα ύφασμα υπηρεσίας. Μπορείτε να το βρείτε στο σύμπλεγμα δηλωτικό ανακτηθούν έως την εντολή [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) :

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## <a name="next-steps"></a>Επόμενα βήματα

[Αναβάθμιση εφαρμογής υπηρεσίας ύφασμα](service-fabric-application-upgrade.md)

[Εισαγωγή εύρυθμης λειτουργίας υπηρεσίας ύφασμα](service-fabric-health-introduction.md)

[Διάγνωση και αντιμετώπιση προβλημάτων με μια υπηρεσία ύφασμα υπηρεσίας](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Μια εφαρμογή στην υπηρεσία ύφασμα του μοντέλου](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
