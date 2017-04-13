<properties
   pageTitle="Αναβάθμιση ένα σύμπλεγμα μεμονωμένη υπηρεσία ύφασμα σε Windows Server | Microsoft Azure"
   description="Αναβάθμιση το ύφασμα υπηρεσία κώδικα ή/και τη ρύθμιση παραμέτρων που εκτελείται ένα σύμπλεγμα υπηρεσίας ύφασμα μεμονωμένη, συμπεριλαμβανομένης της ρύθμισης κατάσταση ενημέρωσης συμπλέγματος"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>Αναβάθμιση συμπλέγματος υπηρεσίας ύφασμα μεμονωμένη σε Windows Server

> [AZURE.SELECTOR]
- [Azure συμπλέγματος](service-fabric-cluster-upgrade.md)
- [Μεμονωμένη συμπλέγματος](service-fabric-cluster-upgrade-windows-server.md)

Για οποιαδήποτε σύγχρονο σύστημα, σχεδίαση για βελτίωσης είναι το κλειδί για την επίτευξη μακροπρόθεσμες επιτυχίας του προϊόντος σας. Ένα σύμπλεγμα ύφασμα υπηρεσίας είναι ένας πόρος που διαθέτετε. Σε αυτό το άρθρο περιγράφει πώς μπορείτε να βεβαιωθείτε ότι το σύμπλεγμα εκτελείται πάντα υποστηριζόμενες εκδόσεις του υφάσματος υπηρεσία κώδικα και ρυθμίσεις παραμέτρων.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Έλεγχος της έκδοσης ύφασμα που εκτελείται στον συμπλέγματος

Μπορείτε να ορίσετε το σύμπλεγμά σας να λήψη των ενημερώσεων ύφασμα υπηρεσίας, όταν η Microsoft δημοσιεύει μια νέα έκδοση ή να επιλέξετε για να επιλέξετε μια έκδοση υποστηριζόμενο υφάσματος θέλετε το σύμπλεγμά σας να είναι ενεργοποιημένος. 

Μπορείτε να το κάνετε αυτό, ορίζοντας τη ρύθμιση παραμέτρων του συμπλέγματος "fabricClusterAutoupgradeEnabled" σε true ή false.


>[AZURE.NOTE] Βεβαιωθείτε ότι για να διατηρήσετε το σύμπλεγμά σας να εκτελείτε πάντα μια υποστηριζόμενη έκδοση ύφασμα υπηρεσίας. Καθώς και όταν θα σας την αναγγελία την κυκλοφορία του μια νέα έκδοση της υπηρεσίας ύφασμα, την προηγούμενη έκδοση έχει επισημανθεί για τέλος της υποστήριξης μετά τουλάχιστον 60 ημερών από την ημερομηνία. Το νέο ενημερωμένες εκδόσεις είναι ανακοινώθηκε [στο ιστολόγιο ομάδας του ύφασμα υπηρεσίας](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Νέα έκδοση είναι διαθέσιμη σε, στη συνέχεια, επιλέξτε. 


Μπορείτε να αναβαθμίσετε το σύμπλεγμά σας στη νέα έκδοση μόνο εάν χρησιμοποιείτε μια ρύθμιση παραμέτρων κόμβου παραγωγής στυλ, όπου κάθε κόμβο ύφασμα υπηρεσία που έχει εκχωρηθεί σε ένα ξεχωριστό φυσικά ή εικονική μηχανή. Εάν έχετε ένα σύμπλεγμα ανάπτυξης, όταν υπάρχουν περισσότερες από μία υπηρεσία ύφασμα κόμβους σε μία μόνο φυσική ή εικονική μηχανή, πρέπει να καταργήσεις το σύμπλεγμά σας και να δημιουργήσετε ξανά με τη νέα έκδοση.


Υπάρχουν δύο ξεχωριστές ροές εργασίας για την αναβάθμιση του συμπλέγματος για τις πιο πρόσφατες ή μια έκδοση ύφασμα υποστηριζόμενη υπηρεσία. Μία για συμπλεγμάτων που έχουν σύνδεση με την αυτόματη λήψη της πιο πρόσφατης έκδοσης και στο δεύτερο παράδειγμα για συμπλεγμάτων που είναι χωρίς σύνδεση για να κάνετε λήψη της πιο πρόσφατης έκδοσης ύφασμα υπηρεσίας.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Αναβάθμιση των συμπλεγμάτων με τη σύνδεση για να κάνετε λήψη τις πιο πρόσφατες κώδικα και ρύθμισης παραμέτρων 

Ακολουθήστε τα παρακάτω βήματα για να αναβαθμίσετε το σύμπλεγμά σας σε μια υποστηριζόμενη έκδοση, εάν έχετε σύνδεση στο internet για να [http://download.microsoft.com](http://download.microsoft.com) το σας κόμβοι συμπλέγματος 

Για συμπλεγμάτων που έχουν σύνδεση με [http://download.microsoft.com](http://download.microsoft.com), θα σας δείτε τακτά χρονικά διαστήματα για τη διαθεσιμότητα των νέων εκδόσεων ύφασμα υπηρεσίας.


Όταν είναι διαθέσιμη μια νέα έκδοση ύφασμα υπηρεσία, το πακέτο τοπικά στο σύμπλεγμα και παρέχεται για αναβάθμιση. Επιπλέον για να ενημερώσετε τον πελάτη αυτής της νέας έκδοσης, το σύστημα τοποθετεί μια προειδοποίηση εύρυθμης λειτουργίας ρητή σύμπλεγμα παρόμοιο με το εξής:

"Η τρέχουσα σύμπλεγμα έκδοση [έκδοση #] η υποστήριξη λήγει [ημερομηνία].", όταν το σύμπλεγμα εκτελεί την πιο πρόσφατη έκδοση, η προειδοποίηση φεύγει.


#### <a name="cluster-upgrade-workflow"></a>Σύμπλεγμα αναβάθμισης ροής εργασίας.
 
Όταν εμφανιστεί η προειδοποίηση εύρυθμης λειτουργίας σύμπλεγμα, πρέπει να κάνετε τα εξής:

1. Συνδεθείτε με το σύμπλεγμα από οποιονδήποτε υπολογιστή που διαθέτει δικαιώματα πρόσβασης διαχειριστή για όλους τους υπολογιστές που παρατίθενται ως κόμβους του συμπλέγματος. Υπολογιστή που εκτελείται σε αυτήν τη δέσμη ενεργειών δεν χρειάζεται να είναι μέρος του συμπλέγματος

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Λήψη της λίστας των υπηρεσιών ύφασμα εκδόσεις που μπορείτε να κάνετε αναβάθμιση

    ```powershell

    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    θα πρέπει να λάβετε το αποτέλεσμα παρόμοιο με αυτό:

    ![λήψη ύφασμα εκδόσεις][getfabversions]

3. Ξεκινήσετε μια αναβάθμιση συμπλέγματος σε μία από τις εκδόσεις που είναι διαθέσιμες με χρήση του [PowerShell Έναρξη ServiceFabricClusterUpgrade cmd](https://msdn.microsoft.com/library/mt125872.aspx)

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Μπορείτε να παρακολουθείτε την πρόοδο της αναβάθμισης υπηρεσίας ύφασμα explorer ή, εκτελέστε την παρακάτω εντολή κελύφους power

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the Start-ServiceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Αφού έχετε σταθερής τα προβλήματα που οδήγησε σε της επαναφοράς, πρέπει να ξεκινήσετε την αναβάθμιση ξανά, ακολουθώντας τα ίδια βήματα ως πριν από.


### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Αναβάθμιση των συμπλεγμάτων με <U>χωρίς συνδεσιμότητας</u> για να κάνετε λήψη τις πιο πρόσφατες κώδικα και ρύθμισης παραμέτρων

Ακολουθήστε τα παρακάτω βήματα για να αναβαθμίσετε το σύμπλεγμά σας σε μια υποστηριζόμενη έκδοση, εάν σας σύμπλεγμα κόμβοι **δεν έχουν** δυνατότητα σύνδεσης στο internet για να [http://download.microsoft.com](http://download.microsoft.com) 


>[AZURE.NOTE]Εάν χρησιμοποιείτε ένα σύμπλεγμα που δεν είναι συνδεδεμένοι στο internet, θα πρέπει να παρακολουθείτε το Ιστολόγιο ομάδας ύφασμα υπηρεσίας για να ενημερωθείτε για μια νέα έκδοση. Το σύστημα **δεν** τοποθετήστε καμία προειδοποίηση σύμπλεγμα εύρυθμης λειτουργίας να σας ειδοποιεί του.  

1. Τροποποίηση της ρύθμισης παραμέτρων του συμπλέγματος για να ορίσετε την ιδιότητα παρακάτω σε false.

        "fabricClusterAutoupgradeEnabled": false,

και να εκκινήσουν μια αναβάθμιση ρύθμισης παραμέτρων. ανατρέξτε στην [Έναρξη ServiceFabricClusterUpgrade PS cmd](https://msdn.microsoft.com/library/mt125872.aspx) για χρήση λεπτομέρειες. Την έκδοση δήλωσης σύμπλεγμα είναι η έκδοση που έχετε στο το clusterConfig.JSON. Βεβαιωθείτε ότι για να την ενημερώσετε πριν από την έναρξη απενεργοποίηση της ρύθμισης παραμέτρων αναβάθμισης.

```powershell

    Start-ServiceFabricClusterUpgrade [-Config] [-ClusterConfigVersion] -FailureAction Rollback -Monitored 

```

#### <a name="cluster-upgrade-workflow"></a>Σύμπλεγμα αναβάθμισης ροής εργασίας.
 


1. Κάντε λήψη του την πιο πρόσφατη έκδοση του πακέτου από έγγραφο [δημιουργία συμπλέγματος ύφασμα service για τον windows server](service-fabric-cluster-creation-for-windows-server.md) 


1. Συνδεθείτε με το σύμπλεγμα από οποιονδήποτε υπολογιστή που διαθέτει δικαιώματα πρόσβασης διαχειριστή για όλους τους υπολογιστές που παρατίθενται ως κόμβους του συμπλέγματος. Υπολογιστή που εκτελείται σε αυτήν τη δέσμη ενεργειών δεν χρειάζεται να είναι μέρος του συμπλέγματος 

    ```powershell

    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Αντιγράψτε το πακέτο που έχετε λάβει το χώρο αποθήκευσης εικόνα σύμπλεγμα.

    ```powershell

    ###### Get the list of available service fabric versions 
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"


    ```

2. Καταχώρηση το αντιγραμμένο πακέτο 

    ```powershell

    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 

    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```


3. Ξεκινήσετε μια αναβάθμιση συμπλέγματος σε μία από τις εκδόσεις που είναι διαθέσιμες. 

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Μπορείτε να παρακολουθείτε την πρόοδο της αναβάθμισης υπηρεσίας ύφασμα explorer ή, εκτελέστε την παρακάτω εντολή κελύφους power

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the start-serviceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Αφού έχετε σταθερής τα προβλήματα που οδήγησε σε της επαναφοράς, πρέπει να ξεκινήσετε την αναβάθμιση ξανά, ακολουθώντας τα ίδια βήματα ως πριν από.



## <a name="next-steps"></a>Επόμενα βήματα
- Μάθετε πώς μπορείτε να προσαρμόσετε μερικές από τις [Ρυθμίσεις ύφασμα σύμπλεγμα ύφασμα υπηρεσίας](service-fabric-cluster-fabric-settings.md)
- Μάθετε πώς μπορείτε να [κλιμακωθεί το σύμπλεγμά σας και σμίκρυνση](service-fabric-cluster-scale-up-down.md)
- Μάθετε περισσότερα σχετικά με την [εφαρμογή αναβαθμίσεις](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
