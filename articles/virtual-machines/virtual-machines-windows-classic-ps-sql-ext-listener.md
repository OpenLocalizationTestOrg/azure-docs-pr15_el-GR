<properties
    pageTitle="Ρύθμιση παραμέτρων ενός εξωτερικού ακρόασης για πάντα σε ομάδες διαθεσιμότητα | Microsoft Azure"
    description="Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί σε βήματα για τη δημιουργία μιας πάντα στη διαθεσιμότητα ομάδα ακρόασης στο Azure που είναι προσβάσιμο εξωτερικά με τη χρήση στη δημόσια διεύθυνση εικονικό IP της υπηρεσίας συσχετισμένη cloud."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Ρύθμιση παραμέτρων ενός εξωτερικού ακρόασης για πάντα σε ομάδες διαθεσιμότητα στο Azure

> [AZURE.SELECTOR]
- [Εσωτερική ακρόασης](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Εξωτερική ακρόασης](virtual-machines-windows-classic-ps-sql-ext-listener.md)

Αυτό το θέμα δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους μιας υπηρεσίας ακρόασης για μια πάντα στη διαθεσιμότητα ομάδα που είναι εξωτερική πρόσβαση στο internet. Αυτό γίνεται πιθανές, συσχετίζοντας διεύθυνση **Δημόσιας εικονικό IP (VIP)** την υπηρεσία cloud με το ακροατήριο.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Ομάδα διαθεσιμότητα σας μπορούν να περιέχουν αντίγραφα που βρίσκονται στην εσωτερική εγκατάσταση μόνο, Azure μόνο, ή να καλύπτουν εσωτερικής εγκατάστασης και Azure για υβριδικές διαμορφώσεις. Azure αντίγραφα μπορεί να βρίσκεται μέσα στην ίδια περιοχή ή σε πολλές περιοχές με χρήση πολλών δικτύων εικονικού (VNets). Τα παρακάτω βήματα θεωρείται ότι έχετε ήδη [ρυθμίσει τις παραμέτρους μιας ομάδας διαθεσιμότητας](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , αλλά δεν έχετε ρυθμίσει ένα ακροατήριο.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Οδηγίες και περιορισμοί για εξωτερική ακροατών

Λάβετε υπόψη τις ακόλουθες οδηγίες σχετικά με τη διαθεσιμότητα ακρόασης ομάδα στο Azure όταν πραγματοποιείτε ανάπτυξη χρησιμοποιώντας τη διεύθυνση ηβική VIP cloud υπηρεσίας:

- Η διαθεσιμότητα ακρόαση ομάδα υποστηρίζεται σε Windows Server 2008 R2, Windows Server 2012 και Windows Server 2012 R2.

- Την εφαρμογή-πελάτη πρέπει να βρίσκονται σε μια υπηρεσία cloud διαφορετικό από αυτόν που περιέχει την ομάδα σας διαθεσιμότητα ΣΠΣ. Azure δεν υποστηρίζει απόδοση διακομιστή απευθείας με το πρόγραμμα-πελάτη και διακομιστή στην ίδια υπηρεσία cloud.

- Από προεπιλογή, τα βήματα σε αυτό το άρθρο σας δείχνουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους ενός ακρόασης για να χρησιμοποιήσετε τη διεύθυνση IP εικονικού (VIP) υπηρεσία cloud. Ωστόσο, είναι δυνατό να δεσμεύσετε και να δημιουργήσετε πολλές διευθύνσεις VIP της υπηρεσίας cloud. Αυτό σας επιτρέπει να χρησιμοποιήσετε τα βήματα που περιγράφονται σε αυτό το άρθρο για να δημιουργήσετε πολλές ακροατών που κάθε συσχετίζονται με μια διαφορετική VIP. Για πληροφορίες σχετικά με τον τρόπο για να δημιουργήσετε πολλές διευθύνσεις VIP, ανατρέξτε στο θέμα [Πολλών VIPs ανά υπηρεσία στο cloud](../load-balancer/load-balancer-multivip.md).

- Εάν θέλετε να δημιουργήσετε μια υπηρεσία ακρόασης για ένα υβριδικό περιβάλλον, το δίκτυο εσωτερικής εγκατάστασης πρέπει να έχετε συνδεσιμότητας με δημόσια Internet εκτός από το VPN τοποθεσίας σε τοποθεσία με το Azure εικονικού δικτύου. Όταν στο δευτερεύον Azure, το ακροατήριο ομάδα διαθεσιμότητα είναι δυνατή η πρόσβαση μόνο από τη δημόσια διεύθυνση IP του την υπηρεσία cloud αντίστοιχα.

- Δεν υποστηρίζεται για να δημιουργήσετε μια εξωτερική ακρόασης στην ίδια υπηρεσία cloud όπου έχετε επίσης μια εσωτερική ακρόασης χρησιμοποιώντας το εσωτερικό εξισορρόπησης φόρτου (ILB).

## <a name="determine-the-accessibility-of-the-listener"></a>Προσδιορίστε την προσβασιμότητα της λειτουργίας ακρόασης

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Σε αυτό το άρθρο εστιάζει σχετικά με τη δημιουργία μιας υπηρεσίας ακρόασης που χρησιμοποιεί **εξωτερικά εξισορρόπησης φόρτου**. Εάν θέλετε μια υπηρεσία ακρόασης που είναι ιδιωτική στο δίκτυο του εικονικού σας, δείτε την έκδοση αυτού του άρθρου που παρέχει τα βήματα για τη ρύθμιση ενός [ακρόασης με ILB](virtual-machines-windows-classic-ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Δημιουργία εξισορρόπηση φόρτου τα τελικά σημεία Εικονική με απευθείας διακομιστή επιστροφής

Εξισορρόπηση φόρτου εξωτερικών χρησιμοποιεί η εικονική στη δημόσια διεύθυνση IP εικονικού από την υπηρεσία cloud που φιλοξενεί το ΣΠΣ. Επομένως, δεν χρειάζεται να δημιουργήσετε ή να ρυθμίσετε τη μονάδα εξισορρόπησης φόρτου σε αυτήν την περίπτωση.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Αντιγράψτε τη δέσμη ενεργειών PowerShell κάτω από ένα πρόγραμμα επεξεργασίας κειμένου και ορίστε τις τιμές μεταβλητών για να ταιριάζει στο περιβάλλον σας (προεπιλογές έχει παραχωρηθεί για ορισμένες παράμετροι). Λάβετε υπόψη ότι εάν την ομάδα σας διαθεσιμότητα εκτείνεται σε Azure περιοχές, πρέπει να εκτελέσετε τη δέσμη ενεργειών μία φορά σε κάθε κέντρα δεδομένων για την υπηρεσία στο cloud και κόμβους που βρίσκονται στο συγκεκριμένο κέντρο δεδομένων.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas

        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

1. Αφού έχετε ορίσει τις μεταβλητές, αντιγράψτε τη δέσμη ενεργειών από το πρόγραμμα επεξεργασίας κειμένου σε περίοδο λειτουργίας σας Azure PowerShell για να το εκτελέσετε. Εάν το μήνυμα εξακολουθεί να εμφανίζει >>, πληκτρολογήστε ENTER ξανά για να βεβαιωθείτε ότι η δέσμη ενεργειών αρχίζει να λειτουργεί.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Βεβαιωθείτε ότι έχει εγκατασταθεί KB2854082 εάν είναι απαραίτητο

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Ανοίξτε τις θύρες τείχους προστασίας στον κόμβο ομάδας διαθεσιμότητας

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Δημιουργήστε το ακροατήριο ομάδας διαθεσιμότητας

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Για την εξισορρόπηση φόρτου εξωτερικών, πρέπει να αποκτήσετε στη δημόσια διεύθυνση IP εικονικού από την υπηρεσία cloud που περιέχει το αντίγραφα. Συνδεθείτε στην πύλη του Azure κλασική. Μεταβείτε στην υπηρεσία cloud που περιέχει την ομάδα σας διαθεσιμότητα Εικονική. Ανοίξτε την προβολή του **πίνακα εργαλείων** .

3. Σημείωση η διεύθυνση που εμφανίζεται στην περιοχή **διεύθυνση δημόσιας εικονικό IP (VIP)**. Εάν η λύση εκτείνεται σε VNets, επαναλάβετε αυτό το βήμα για κάθε υπηρεσία cloud που περιέχει μια Εικονική που φιλοξενεί μια ρεπλίκα.

4. Σε ένα από τα ΣΠΣ, αντιγράψτε τη δέσμη ενεργειών PowerShell κάτω από ένα πρόγραμμα επεξεργασίας κειμένου και ορίστε τις μεταβλητές στις τιμές που σημειώσατε προηγουμένως.

        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service

        Import-Module FailoverClusters

        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.

        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255


1. Μία φορά που έχετε ορίσει τις μεταβλητές, ανοίξτε ένα παράθυρο του Windows PowerShell αναβαθμισμένα δικαιώματα, στη συνέχεια, αντιγράψτε τη δέσμη ενεργειών από το πρόγραμμα επεξεργασίας κειμένου και επικολλήστε σε περίοδο λειτουργίας σας Azure PowerShell για να το εκτελέσετε. Εάν το μήνυμα εξακολουθεί να εμφανίζει >>, πληκτρολογήστε ENTER ξανά για να βεβαιωθείτε ότι η δέσμη ενεργειών αρχίζει να λειτουργεί.

1. Επαναλάβετε αυτήν τη διαδικασία σε κάθε Εικονική. Αυτή η δέσμη ενεργειών ρυθμίζει τις παραμέτρους του πόρου διεύθυνση IP με τη διεύθυνση IP του την υπηρεσία cloud και ορίζει άλλες παραμέτρους όπως τη θύρα δοκιμή του. Όταν ο πόρος διεύθυνση IP είναι έφερε online, να μπορεί να ανταποκρίνεται, στη συνέχεια, να το σταθμοσκόπησης στη θύρα δοκιμή του από το τελικό σημείο εξισορρόπηση φόρτου δημιουργήσατε νωρίτερα σε αυτό το πρόγραμμα εκμάθησης.

## <a name="bring-the-listener-online"></a>Μεταφέρετε το ακροατήριο online

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Παρακολούθηση στοιχείων

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Δοκιμάστε το ακροατήριο ομάδας διαθεσιμότητας (εντός του ίδιου VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Δοκιμάστε το ακροατήριο ομάδας διαθεσιμότητας (επάνω από το internet)

Για να αποκτήσετε πρόσβαση στο πρόγραμμα ακρόασης από εκτός του εικονικού δικτύου, πρέπει να χρησιμοποιείτε το εξισορρόπηση φόρτου εξωτερικού/δημόσια (που περιγράφεται σε αυτό το θέμα) και όχι ILB, που είναι μόνο accesible εντός του ίδιου VNet. Στη συμβολοσειρά σύνδεσης, μπορείτε να καθορίσετε το όνομα της υπηρεσίας cloud. Για παράδειγμα, εάν είχατε μια υπηρεσία στο cloud με το όνομα *mycloudservice*, την πρόταση sqlcmd θα είναι ως εξής:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Σε αντίθεση με το προηγούμενο παράδειγμα, πρέπει να χρησιμοποιείται τον έλεγχο ταυτότητας SQL, επειδή ο καλών δεν μπορείτε να χρησιμοποιήσετε έλεγχο ταυτότητας των windows μέσω του internet. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πάντα στη διαθεσιμότητα ομάδας σε Εικονική Azure: σενάρια συνδεσιμότητα υπολογιστή-πελάτη](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Όταν χρησιμοποιείτε τον έλεγχο ταυτότητας SQL, βεβαιωθείτε ότι μπορείτε να δημιουργήσετε την ίδια σύνδεση σε δύο αντίγραφα. Για περισσότερες πληροφορίες σχετικά με την αντιμετώπιση συνδέσεις με ομάδες διαθεσιμότητας, ανατρέξτε στο θέμα [πώς μπορείτε να αντιστοιχίσετε συνδέσεις ή να χρησιμοποιήσετε περιέχονται χρήστες της βάσης δεδομένων SQL για να συνδεθείτε σε άλλα αντίγραφα και αντιστοίχιση με βάσεις δεδομένων διαθεσιμότητας](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Εάν τα αντίγραφα πάντα σε είναι σε διαφορετικά δευτερεύοντα δίκτυα, πρέπει να καθορίσετε προγράμματα-πελάτες **MultisubnetFailover = True** στη συμβολοσειρά σύνδεσης. Το αποτέλεσμα προσπάθειες παράλληλες σύνδεσης για να αντίγραφα του διαφορετικά δευτερεύοντα δίκτυα. Σημειώστε ότι αυτό το σενάριο περιλαμβάνει μια ανάπτυξη πάντα στην ομάδα διαθεσιμότητα σταυρό περιοχής.

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
