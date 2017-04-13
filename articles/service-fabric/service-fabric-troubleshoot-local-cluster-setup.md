<properties
   pageTitle="Αντιμετώπιση προβλημάτων του τοπικού ρύθμισης σύμπλεγμα υπηρεσίας ύφασμα | Microsoft Azure"
   description="Σε αυτό το άρθρο καλύπτει ένα σύνολο προτάσεις για την αντιμετώπιση προβλημάτων τοπικής ανάπτυξης συμπλέγματος"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="troubleshoot-your-local-development-cluster-setup"></a>Αντιμετώπιση προβλημάτων του προγράμματος εγκατάστασης συμπλέγματος τοπικής ανάπτυξης

Εάν αντιμετωπίσετε κάποιο πρόβλημα κατά την αλληλεπίδραση με το τοπικό σύμπλεγμα ανάπτυξης Azure Service ύφασμα, αναθεωρήστε τις παρακάτω προτάσεις για πιθανές λύσεις.

## <a name="cluster-setup-failures"></a>Αποτυχία εγκατάστασης συμπλέγματος

### <a name="cannot-clean-up-service-fabric-logs"></a>Δεν είναι δυνατό να εκκαθαρίσετε ύφασμα υπηρεσία αρχείων καταγραφής

#### <a name="problem"></a>Πρόβλημα

Κατά την εκτέλεση της δέσμης ενεργειών DevClusterSetup, θα δείτε ένα σφάλμα, όπως αυτό:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Λύση

Κλείστε το τρέχον παράθυρο PowerShell και ανοίξτε ένα νέο παράθυρο του PowerShell ως διαχειριστής. Τώρα θα πρέπει να μπορείτε να εκτελέσετε με επιτυχία η δέσμη ενεργειών.

## <a name="cluster-connection-failures"></a>Αποτυχίες σύνδεσης συμπλέγματος

### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Cmdlet του PowerShell ύφασμα υπηρεσίας δεν αναγνωρίζονται σε Azure PowerShell

#### <a name="problem"></a>Πρόβλημα

Εάν επιχειρήσετε να εκτελέσετε οποιαδήποτε από τα cmdlet του PowerShell ύφασμα υπηρεσία, όπως είναι οι `Connect-ServiceFabricCluster` σε ένα παράθυρο του PowerShell Azure, αποτύχει, τι λένε ότι το cmdlet δεν αναγνωρίζεται. Ο λόγος για αυτό είναι ότι Azure PowerShell χρησιμοποιεί την έκδοση 32 bit του Windows PowerShell (ακόμη και σε εκδόσεις λειτουργικό σύστημα 64 bit,) ότι τα cmdlet ύφασμα υπηρεσίας λειτουργούν μόνο στα περιβάλλοντα 64-bit.

#### <a name="solution"></a>Λύση

Να εκτελείτε πάντα cmdlet του ύφασμα υπηρεσίας απευθείας από το Windows PowerShell.

>[AZURE.NOTE] Η πιο πρόσφατη έκδοση του Azure PowerShell δεν δημιουργεί μια ειδική συντόμευση, ώστε αυτή δεν είναι πλέον πρέπει να πραγματοποιείται.

### <a name="type-initialization-exception"></a>Τύπος εξαίρεσης προετοιμασίας

#### <a name="problem"></a>Πρόβλημα

Όταν συνδέεστε με το σύμπλεγμα στο PowerShell, εμφανίζεται το σφάλμα TypeInitializationException για System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Λύση

Η μεταβλητή διαδρομής δεν έχει οριστεί σωστά κατά την εγκατάσταση. Ελέγξτε αποσυνδεθείτε από το Windows και συνδεθείτε ξανά. Αυτό θα ανανεωθεί πλήρως διαδρομή σας.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Σύνδεση σύμπλεγμα αποτυγχάνει με "Αντικείμενο είναι κλειστό"

#### <a name="problem"></a>Πρόβλημα

Μια κλήση σε σύνδεση ServiceFabricCluster αποτυγχάνει με το μήνυμα σφάλματος ως εξής:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Λύση

Κλείστε το τρέχον παράθυρο PowerShell και ανοίξτε ένα νέο παράθυρο του PowerShell ως διαχειριστής. Τώρα θα πρέπει να μπορείτε να συνδεθείτε με επιτυχία.

### <a name="fabric-connection-denied-exception"></a>Εξαίρεση δεν επιτρέπεται η σύνδεση ύφασμα

#### <a name="problem"></a>Πρόβλημα

Όταν εντοπισμού από το Visual Studio, θα λάβετε ένα σφάλμα FabricConnectionDeniedException.

#### <a name="solution"></a>Λύση

Αυτό το σφάλμα παρουσιάζεται συνήθως όταν προσπαθήστε να δοκιμή για να ξεκινήσετε μια υπηρεσία παροχής φιλοξενίας υπηρεσίας επεξεργασία με μη αυτόματο τρόπο, αντί να επιτρέπεται ο χρόνος εκτέλεσης υπηρεσίας ύφασμα για να το ξεκινήσετε για εσάς.

Βεβαιωθείτε ότι δεν έχετε τα έργα υπηρεσίας Ορισμός ως εκκίνησης έργα στη λύση σας. Μόνο τα έργα εφαρμογής υπηρεσίας ύφασμα θα πρέπει να οριστούν ως έργα εκκίνησης.

>[AZURE.TIP] Εάν, μετά την εγκατάσταση, το τοπικό σύμπλεγμα αρχίζει να συμπεριφέρεται ασυνήθιστα, μπορείτε να κάνετε επαναφορά χρησιμοποιώντας την εφαρμογή εργασιών τοπικό σύμπλεγμα διαχειριστή συστήματος. Αυτό θα καταργήσετε το υπάρχον σύμπλεγμα και να ρυθμίσετε ένα νέο. Σημειώστε ότι όλα αναπτύξει εφαρμογές και σχετικών δεδομένων θα καταργηθούν.

## <a name="next-steps"></a>Επόμενα βήματα

- [Κατανόηση και αντιμετώπιση προβλημάτων με το σύμπλεγμά σας με τις αναφορές εύρυθμης λειτουργίας συστήματος](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Απεικόνιση το σύμπλεγμά σας με την Εξερεύνηση ύφασμα υπηρεσίας](service-fabric-visualizing-your-cluster.md)
