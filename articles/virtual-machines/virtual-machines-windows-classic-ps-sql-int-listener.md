<properties
    pageTitle="Ρύθμιση παραμέτρων μιας υπηρεσίας ακρόασης ILB για πάντα ομάδες διαθεσιμότητα | Microsoft Azure"
    description="Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί πόρους που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης και δημιουργεί μια πάντα στη διαθεσιμότητα ομάδα ακρόασης στο Azure χρησιμοποιώντας μια εσωτερική εξισορρόπησης φόρτου (ILB)."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Ρύθμιση παραμέτρων ενός ακρόασης ILB για πάντα σε ομάδες διαθεσιμότητα στο Azure

> [AZURE.SELECTOR]
- [Εσωτερική ακρόασης](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Εξωτερική ακρόασης](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="overview"></a>Επισκόπηση

Αυτό το θέμα δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους μιας υπηρεσίας ακρόασης για μια πάντα στη διαθεσιμότητα ομάδα, χρησιμοποιώντας μια **Εσωτερική εξισορρόπησης φόρτου (ILB)**.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Για να ρυθμίσετε μια ακρόασης ILB για μια ομάδα διαθεσιμότητα πάντα σε στο μοντέλο διαχείριση πόρων, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μιας εσωτερικής εξισορρόπηση φόρτου για μια πάντα στη διαθεσιμότητα ομάδα στο Azure](virtual-machines-windows-portal-sql-alwayson-int-listener.md).


Ομάδα διαθεσιμότητα σας μπορούν να περιέχουν αντίγραφα που βρίσκονται στην εσωτερική εγκατάσταση μόνο, Azure μόνο, ή να καλύπτουν εσωτερικής εγκατάστασης και Azure για υβριδικές διαμορφώσεις. Azure αντίγραφα μπορεί να βρίσκεται μέσα στην ίδια περιοχή ή σε πολλές περιοχές με χρήση πολλών δικτύων εικονικού (VNets). Τα παρακάτω βήματα θεωρείται ότι έχετε ήδη [ρυθμίσει τις παραμέτρους μιας ομάδας διαθεσιμότητας](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , αλλά δεν έχετε ρυθμίσει ένα ακροατήριο.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Γενικές οδηγίες και περιορισμοί για το εσωτερικό ακροατών
Λάβετε υπόψη τις ακόλουθες οδηγίες στη λειτουργία ακρόασης ομάδα διαθεσιμότητα στο Azure χρησιμοποιώντας ILB:

- Η διαθεσιμότητα ακρόαση ομάδα υποστηρίζεται σε Windows Server 2008 R2, Windows Server 2012 και Windows Server 2012 R2.

- Μόνο ένα εσωτερικό διαθεσιμότητα ομάδα ακρόασης υποστηρίζεται ανά υπηρεσία cloud, επειδή το ακροατήριο έχει ρυθμιστεί ώστε να τα ILB και δεν υπάρχει μόνο μία ILB ανά υπηρεσία στο cloud. Ωστόσο, είναι δυνατό να δημιουργήσετε πολλές εξωτερικές ακροατών. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ενός εξωτερικού ακρόασης για πάντα στη διαθεσιμότητα ομάδες στο Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

- Δεν υποστηρίζεται για να δημιουργήσετε μια εσωτερική ακρόασης στην ίδια υπηρεσία cloud όπου έχετε επίσης ένα εξωτερικό ακρόασης χρησιμοποιώντας την υπηρεσία cloud δημόσια VIP.

## <a name="determine-the-accessibility-of-the-listener"></a>Προσδιορίστε την προσβασιμότητα της λειτουργίας ακρόασης

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Σε αυτό το άρθρο εστιάζει σχετικά με τη δημιουργία μιας υπηρεσίας ακρόασης που χρησιμοποιεί μια **Εσωτερική εξισορρόπησης φόρτου (ILB)**. Εάν χρειάζεστε μια δημόσια/εξωτερικού ακρόασης, δείτε την έκδοση αυτού του άρθρου που παρέχει τα βήματα για τη ρύθμιση ενός [εξωτερικού ακρόασης](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Δημιουργία εξισορρόπηση φόρτου τα τελικά σημεία Εικονική με απευθείας διακομιστή επιστροφής

Για ILB, πρέπει πρώτα να δημιουργήσετε το εσωτερικό εξισορρόπηση φόρτου. Αυτό γίνεται στη δέσμη ενεργειών παρακάτω.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Για **ILB**, θα πρέπει να μπορείτε να εκχωρήσετε μια στατική διεύθυνση IP. Πρώτα, εξετάστε την τρέχουσα ρύθμιση παραμέτρων VNet, εκτελώντας την ακόλουθη εντολή:

        (Get-AzureVNetConfig).XMLConfiguration

1. Σημειώστε το όνομα του **δευτερεύοντος δικτύου** για το υποδίκτυο που περιέχει το ΣΠΣ που φιλοξενούν τα αντίγραφα. Αυτό θα χρησιμοποιηθεί στην παράμετρο **$SubnetName** στη δέσμη ενεργειών.

1. Στη συνέχεια, σημειώστε το όνομα του **VirtualNetworkSite** και το αρχικό **AddressPrefix** για το υποδίκτυο που περιέχει το ΣΠΣ που φιλοξενούν τα αντίγραφα. Αναζητήστε μια διαθέσιμη διεύθυνση IP, που περνά μέσα και οι δύο τιμές στην εντολή **Έλεγχος AzureStaticVNetIP** και εξέταση του **AvailableAddresses**. Για παράδειγμα, εάν η VNet έχει το όνομα *MyVNet* και είχατε μια διεύθυνση υποδικτύου περιοχής που αποτελέσματα στο *172.16.0.128*, την παρακάτω εντολή θα διευθύνσεις διαθέσιμες:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. Επιλέξτε μία από τις διαθέσιμες διευθύνσεις και χρησιμοποιήστε το στην παράμετρο **$ILBStaticIP** της την ακόλουθη δέσμη ενεργειών.

3. Αντιγράψτε τη δέσμη ενεργειών PowerShell κάτω από ένα πρόγραμμα επεξεργασίας κειμένου και ορίστε τις τιμές μεταβλητών για να ταιριάζει στο περιβάλλον σας (Σημειώστε ότι έχει παραχωρηθεί προεπιλογών για ορισμένες παράμετροι). Σημειώστε ότι οι υπάρχουσες αναπτύξεις που χρησιμοποιούν ομάδες συσχέτισης δεν είναι δυνατό να προσθέσετε ILB. Για περισσότερες πληροφορίες σχετικά με τις απαιτήσεις ILB, ανατρέξτε στο θέμα [Εσωτερική εξισορρόπηση φόρτου](../load-balancer/load-balancer-internal-overview.md). Επίσης, εάν σας ομάδας διαθεσιμότητας εκτείνεται σε Azure περιοχές, πρέπει να εκτελέσετε τη δέσμη ενεργειών μία φορά σε κάθε κέντρα δεδομένων για την υπηρεσία στο cloud και κόμβους που βρίσκονται στο συγκεκριμένο κέντρο δεδομένων.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

1. Αφού έχετε ορίσει τις μεταβλητές, αντιγράψτε τη δέσμη ενεργειών από το πρόγραμμα επεξεργασίας κειμένου σε περίοδο λειτουργίας σας Azure PowerShell για να το εκτελέσετε. Εάν το μήνυμα εξακολουθεί να εμφανίζει >>, πληκτρολογήστε ENTER ξανά για να βεβαιωθείτε ότι η δέσμη ενεργειών αρχίζει να λειτουργεί. Σημείωση

>[AZURE.NOTE] Πύλη του Azure κλασική δεν υποστηρίζει το εσωτερικό εξισορρόπηση φόρτου αυτήν τη στιγμή, έτσι δεν θα βλέπετε τα ILB ή τα τελικά σημεία στην πύλη του Azure κλασική. Ωστόσο, **Get-AzureEndpoint** επιστρέφει μια εσωτερική διεύθυνση IP, εάν η μονάδα εξισορρόπησης φόρτου εκτελείται σε αυτό. Διαφορετικά, επιστρέφει την τιμή null.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Βεβαιωθείτε ότι έχει εγκατασταθεί KB2854082 εάν είναι απαραίτητο

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Ανοίξτε τις θύρες τείχους προστασίας στον κόμβο ομάδας διαθεσιμότητας

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Δημιουργήστε το ακροατήριο ομάδας διαθεσιμότητας

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Για ILB, πρέπει να χρησιμοποιήσετε τη διεύθυνση IP του το εσωτερικό φόρτωσης εξισορρόπησης (ILB) δημιουργήσατε νωρίτερα. Χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών για να λάβετε αυτήν τη διεύθυνση IP σε PowerShell.

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. Σε ένα από τα ΣΠΣ, αντιγράψτε τη δέσμη ενεργειών του PowerShell για το λειτουργικό σας σύστημα σε ένα πρόγραμμα επεξεργασίας κειμένου και ορίστε τις μεταβλητές στις τιμές που σημειώσατε προηγουμένως.

    Για τον Windows Server 2012 ή νεότερη έκδοση, χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
        
    Για Windows Server 2008 R2, χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
    

1. Μία φορά που έχετε ορίσει τις μεταβλητές, ανοίξτε ένα παράθυρο του Windows PowerShell αναβαθμισμένα δικαιώματα, στη συνέχεια, αντιγράψτε τη δέσμη ενεργειών από το πρόγραμμα επεξεργασίας κειμένου και επικολλήστε σε περίοδο λειτουργίας σας Azure PowerShell για να το εκτελέσετε. Εάν το μήνυμα εξακολουθεί να εμφανίζει >>, πληκτρολογήστε ENTER ξανά για να βεβαιωθείτε ότι η δέσμη ενεργειών αρχίζει να λειτουργεί.

2. Επαναλάβετε αυτήν τη διαδικασία σε κάθε Εικονική. Αυτή η δέσμη ενεργειών ρυθμίζει τις παραμέτρους του πόρου διεύθυνση IP με τη διεύθυνση IP του την υπηρεσία cloud και ορίζει άλλες παραμέτρους όπως τη θύρα δοκιμή του. Όταν ο πόρος διεύθυνση IP είναι έφερε online, να μπορεί να ανταποκρίνεται, στη συνέχεια, να το σταθμοσκόπησης στη θύρα δοκιμή του από το τελικό σημείο εξισορρόπηση φόρτου δημιουργήσατε νωρίτερα σε αυτό το πρόγραμμα εκμάθησης.

## <a name="bring-the-listener-online"></a>Μεταφέρετε το ακροατήριο online

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Παρακολούθηση στοιχείων

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Δοκιμάστε το ακροατήριο ομάδας διαθεσιμότητας (εντός του ίδιου VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
