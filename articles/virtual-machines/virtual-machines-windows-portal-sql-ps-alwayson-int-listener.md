<properties
   pageTitle="Ρύθμιση παραμέτρων πάντα στο ακροατών ομάδας διαθεσιμότητας – Microsoft Azure"
   description="Ρύθμιση παραμέτρων ακροατών ομάδα διαθεσιμότητα στο μοντέλο Azure διαχείριση πόρων, χρησιμοποιώντας μια εσωτερική εξισορρόπηση φόρτου με μία ή περισσότερες διευθύνσεις IP."
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="10/20/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Ρύθμιση παραμέτρων ενός ή περισσότερων πάντα στη διαθεσιμότητα ομάδα ακροατών - διαχείριση πόρων 

Αυτό το θέμα δείχνει πώς μπορείτε να κάνετε δύο πράγματα:

- Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου για τις ομάδες διαθεσιμότητα SQL Server με χρήση των cmdlet του PowerShell.

- Προσθήκη επιπλέον διευθύνσεων IP σε μια μονάδα εξισορρόπησης φόρτου για την υποστήριξη περισσότερες από μία ομάδες διαθεσιμότητα του SQL Server. 

Στο SQL Server μια ακρόασης ομάδα διαθεσιμότητα είναι ένα όνομα εικονικού δικτύου που προγράμματα-πελάτες συνδέεστε για πρόσβαση σε μια βάση δεδομένων στη ρεπλίκα πρωτεύον ή δευτερεύον. Σε εικονικές μηχανές Azure, μια μονάδα εξισορρόπησης φόρτου διατηρεί τη διεύθυνση IP για το ακροατήριο. Η εξισορρόπηση φόρτου δρομολογεί την κίνηση για να την παρουσία του SQL Server που ακρόαση στη θύρα δοκιμή του. Στις περισσότερες περιπτώσεις, μια ομάδα διαθεσιμότητα χρησιμοποιεί μια εσωτερική εξισορρόπηση φόρτου. Μια Azure εσωτερικό εξισορρόπηση φόρτου να φιλοξενήσετε μία ή πολλές διευθύνσεις IP. Κάθε διεύθυνση IP που χρησιμοποιεί μια θύρα συγκεκριμένες δοκιμή του. Αυτό το έγγραφο εμφανίζει τον τρόπο χρήσης του PowerShell για να δημιουργήσετε μια νέα εξισορρόπηση φόρτου, ή προσθέστε διευθύνσεις IP σε μια υπάρχουσα εξισορρόπηση φόρτου για τις ομάδες διαθεσιμότητα SQL Server. 

Η δυνατότητα για να αντιστοιχίσετε πολλές διευθύνσεις IP σε μια εσωτερική εξισορρόπηση φόρτου είναι μια νέα Azure και είναι διαθέσιμη μόνο σε μοντέλο διαχείρισης πόρων. Για να ολοκληρώσετε αυτήν την εργασία, πρέπει να έχετε μια ομάδα διαθεσιμότητα SQL Server αναπτυχθεί σε Azure εικονικές μηχανές στο μοντέλο διαχείρισης πόρων. Και οι δύο εικονικές μηχανές του SQL Server πρέπει να ανήκουν σε το ίδιο σύνολο διαθεσιμότητα. Μπορείτε να χρησιμοποιήσετε το [πρότυπο της Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) για να δημιουργήσετε αυτόματα την ομάδα διαθεσιμότητα στη Διαχείριση Azure πόρων. Αυτό το πρότυπο δημιουργεί αυτόματα ομάδα διαθεσιμότητας, όπως το εσωτερικό εξισορρόπηση φόρτου για εσάς. Εάν προτιμάτε, μπορείτε να [ρυθμίσετε με μη αυτόματο τρόπο μια ομάδας διαθεσιμότητας AlwaysOn](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Αυτό το θέμα απαιτεί ότι έχουν ήδη ρυθμιστεί τις ομάδες σας διαθεσιμότητα.  

Σχετικά θέματα περιλαμβάνουν τα εξής:

- [Ρύθμιση παραμέτρων ομάδες διαθεσιμότητας AlwaysOn σε Azure Εικονική (Γραφικών)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   

- [Ρύθμιση παραμέτρων σύνδεσης VNet-VNet, χρησιμοποιώντας τη διαχείριση πόρων Azure και PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Ρύθμιση παραμέτρων του τείχους προστασίας των Windows

Ρύθμιση παραμέτρων του τείχους προστασίας των Windows για να επιτρέψετε την πρόσβαση του SQL Server. Θα πρέπει να ρυθμίσετε τις παραμέτρους του τείχους προστασίας για να επιτρέψετε συνδέσεις TCP με τη χρήση θύρες από την παρουσία του SQL Server, καθώς και τη θύρα που χρησιμοποιείται κατά τη δοκιμή του ακρόασης. Για λεπτομερείς οδηγίες ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ένα τείχος προστασίας των Windows για πρόσβαση σε μηχανή βάσης δεδομένων](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Δημιουργήστε έναν κανόνα εισερχομένων για τη θύρα SQL Server και για τη θύρα δοκιμή του.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Παράδειγμα δέσμης ενεργειών: Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου με το PowerShell

> [AZURE.NOTE] Εάν έχετε δημιουργήσει τη διαθεσιμότητα ομάδα με το [πρότυπο της Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) η φόρτωση δεν πρέπει να ολοκληρώσετε αυτό το βήμα. 

Η ακόλουθη δέσμη ενεργειών PowerShell δημιουργεί μια εσωτερική εξισορρόπηση φόρτου, ρυθμίζει το κανόνες εξισορρόπησης φόρτου και σύνολα μια διεύθυνση IP για την εξισορρόπηση φόρτου. Για να εκτελέσετε τη δέσμη ενεργειών, ανοίξτε το Windows PowerShell ISE, επικολλήστε τη δέσμη ενεργειών στο τμήμα παραθύρου δέσμης ενεργειών. Χρήση `Login-AzureRMAccount` για να συνδεθείτε με το PowerShell. Εάν έχετε πολλές συνδρομές Azure, χρησιμοποιήστε `Select-AzureRmSubscription ` για να ορίσετε τη συνδρομή. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Παράδειγμα δέσμης ενεργειών: Προσθήκη μια διεύθυνση IP σε μια υπάρχουσα εξισορρόπηση φόρτου με το PowerShell

Για να χρησιμοποιήσετε περισσότερες από μία ομάδες διαθεσιμότητας, χρήση του PowerShell για να προσθέσετε μια πρόσθετη διεύθυνση IP σε μια υπάρχουσα εξισορρόπηση φόρτου. Κάθε διεύθυνση IP που απαιτεί το δικό της φόρτωσης εξισορρόπηση κανόνα, δοκιμή του θύρας και εμπρός θύρα.

Η θύρα προσκηνίου είναι η θύρα που χρησιμοποιούν οι εφαρμογές για να συνδεθείτε με την παρουσία του SQL Server. Διευθύνσεις IP για διαθεσιμότητα διαφορετικές ομάδες μπορούν να χρησιμοποιήσουν την ίδια θύρα προσκηνίου.

>[AZURE.NOTE] Για τις ομάδες διαθεσιμότητα SQL Server, κάθε διεύθυνση IP απαιτεί μια θύρα συγκεκριμένες δοκιμή του. Για παράδειγμα, εάν μια διεύθυνση IP σε μια μονάδα εξισορρόπησης φόρτου χρησιμοποιεί θύρα δοκιμή του 59999, άλλες διευθύνσεις IP στη συγκεκριμένη εξισορρόπηση φόρτου να χρησιμοποιήσετε θύρα δοκιμή του 59999. 

- Για πληροφορίες σχετικά με την εξισορρόπηση φόρτου όρια ανατρέξτε στο θέμα **ιδιωτική εμπρός Τερματισμός IP ανά μονάδα εξισορρόπησης φόρτου** στην περιοχή [Δίκτυο όρια - διαχείριση πόρων Azure](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

- Για πληροφορίες σχετικά με τη διαθεσιμότητα των ορίων ομάδα ανατρέξτε στο θέμα [περιορισμοί (διαθεσιμότητα ομάδες)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Η ακόλουθη δέσμη ενεργειών προσθέτει μια νέα διεύθυνση IP σε μια υπάρχουσα εξισορρόπηση φόρτου. Ενημερώστε τις μεταβλητές για το περιβάλλον σας. Το ILB χρησιμοποιεί τη θύρα ακρόασης για τη θύρα φόρτωσης εξισορρόπησης προσκηνίου. Αυτή η θύρα μπορεί να είναι θύρας στην οποία ακρόαση SQL Server σε. Για το προεπιλεγμένο παρουσίες του SQL Server, είναι θύρα 1433. Το κανόνα για μια ομάδα διαθεσιμότητα εξισορρόπησης φόρτου απαιτεί μια αιωρούμενη IP (επιστροφής απευθείας διακομιστή), ώστε να τη θύρα παρασκηνίου είναι ίδια με τη θύρα προσκηνίου.

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Ρύθμιση παραμέτρων του συμπλέγματος για να χρησιμοποιήσετε τη διεύθυνση IP εξισορρόπησης φόρτου 

Το επόμενο βήμα είναι να ρυθμίσετε τις παραμέτρους της ακρόασης στο σύμπλεγμα και μεταφέρετε το ακροατήριο online. Για να γίνει αυτό, κάντε τα εξής: 

1. Δημιουργήστε το ακροατήριο ομάδα διαθεσιμότητα στο σύμπλεγμα ανακατεύθυνσης  

2. Μεταφέρετε το ακροατήριο online

## <a name="1-create-the-availability-group-listener-on-the-failover-cluster"></a>1. Δημιουργήστε το ακροατήριο ομάδα διαθεσιμότητα στο σύμπλεγμα ανακατεύθυνσης

Σε αυτό το βήμα, προσθέτετε ένα σημείο πρόσβασης υπολογιστή-πελάτη για το σύμπλεγμα ανακατεύθυνσης με τη Διαχείριση συμπλέγματος ανακατεύθυνσης και, στη συνέχεια, χρήση του PowerShell για τη ρύθμιση παραμέτρων του πόρου συμπλέγματος για να ακούσετε στη θύρα δοκιμή του. 

1. Χρησιμοποιήστε RDP για να συνδεθείτε με το Azure εικονική μηχανή που φιλοξενεί την κύρια ρεπλίκα. 

2. Ανοίξτε τη Διαχείριση συμπλέγματος ανακατεύθυνσης.

3. Επιλέξτε τον κόμβο **δίκτυα** και σημειώστε το όνομα του δικτύου συμπλέγματος. Χρησιμοποιήστε αυτό το όνομα στο το `$ClusterNetworkName` μεταβλητής στη δέσμη ενεργειών PowerShell.

4. Αναπτύξτε το όνομα του συμπλέγματος και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρόλοι**.

5. Στο παράθυρο **ρόλων** , κάντε δεξί κλικ στο όνομα της ομάδας διαθεσιμότητας και, στη συνέχεια, επιλέξτε **Προσθήκη πόρων** > **Σημείο πρόσβασης υπολογιστή-πελάτη**.

6. Στο πλαίσιο **όνομα** , δημιουργήστε ένα όνομα για το νέο πρόγραμμα ακρόασης, στη συνέχεια, κάντε κλικ δύο φορές στο κουμπί **Επόμενο** και, στη συνέχεια, κάντε κλικ στο κουμπί **Τέλος**. Μην επαναφέρετε το ακροατήριο ή ο πόρος online σε αυτό το σημείο.
 
    Το όνομα για το νέο πρόγραμμα ακρόασης είναι το όνομα του δικτύου που θα χρησιμοποιήσει τις εφαρμογές για να συνδεθείτε με βάσεις δεδομένων στην ομάδα διαθεσιμότητα SQL Server.

7. Κάντε κλικ στην καρτέλα **πόροι** και, στη συνέχεια, αναπτύξτε το σημείο πρόσβασης υπολογιστή-πελάτη που μόλις δημιουργήσατε. Κάντε δεξί κλικ στον πόρο IP και κάντε κλικ στην επιλογή Ιδιότητες. Σημειώστε το όνομα της διεύθυνσης IP. Θα χρησιμοποιήσετε αυτό το όνομα στο το `$IPResourceName` μεταβλητής στη δέσμη ενεργειών PowerShell.

8. Στην περιοχή **Διεύθυνση IP** , κάντε κλικ στην επιλογή **Στατική διεύθυνση IP** και ορίστε τη στατική διεύθυνση IP στην ίδια διεύθυνση που χρησιμοποιήσατε κατά τη ρύθμιση της διεύθυνσης IP εξισορρόπησης φόρτου στην πύλη του Azure. 

9. Απενεργοποίηση NetBIOS για αυτήν τη διεύθυνση και κάντε κλικ στο κουμπί **OK**. Επαναλάβετε αυτό το βήμα για κάθε πόρο IP, εάν η λύση εκτείνεται σε πολλές VNets Azure. 

10. Κάνετε εξαρτάται από τη διεύθυνση IP του πόρου ομάδας διαθεσιμότητας του SQL Server. Δεξί κλικ στον πόρο στη Διαχείριση συμπλέγματος, αυτό είναι στην καρτέλα **πόροι** , στην περιοχή **Άλλες πόρους**. 

11. Στον κόμβο συμπλέγματος που φιλοξενεί αυτήν τη στιγμή η κύρια ρεπλίκα, ανοίξτε μια αναβαθμισμένη PowerShell ISE και επικολλήστε τις παρακάτω εντολές σε μια νέα δέσμη ενεργειών. Στην καρτέλα **εξαρτήσεις** , κάντε κλικ στο όνομα της λειτουργίας ακρόασης.
 
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>

    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
 
6. Ενημερώστε τις μεταβλητές και εκτελέστε τη δέσμη ενεργειών του PowerShell για να ρυθμίσετε τις παραμέτρους της διεύθυνσης IP και θύρες για το νέο πρόγραμμα ακρόασης.

 >[AZURE.NOTE] Εάν οι διακομιστές SQL σας είναι σε ξεχωριστές περιοχές, πρέπει να εκτελέσετε τη δέσμη ενεργειών PowerShell δύο φορές. Την πρώτη φορά που χρησιμοποιούν το όνομα δικτύου σύμπλεγμα, όνομα πόρου συμπλέγματος IP, και φορτώστε διεύθυνση IP εξισορρόπησης από την πρώτη ομάδα πόρων. Τη δεύτερη φορά που χρησιμοποιούν το όνομα δικτύου σύμπλεγμα, όνομα πόρου συμπλέγματος IP, και φορτώστε διεύθυνση IP εξισορρόπησης από τη δεύτερη ομάδα πόρων.

Τώρα το σύμπλεγμα διαθέτει έναν πόρο διαθεσιμότητα ομάδα ακρόασης.

## <a name="2-bring-the-listener-online"></a>2. μεταφέρετε το ακροατήριο online

Με τη διαθεσιμότητα ομάδα ακρόασης πόρο έχει ρυθμιστεί, μπορείτε να μεταφέρετε το ακροατήριο online ώστε να μπορούν να συνδεθούν εφαρμογές σε βάσεις δεδομένων με την ομάδα διαθεσιμότητας με την παρακολούθηση.

1. Μεταβείτε ξανά για Διαχείριση ανακατεύθυνσης συμπλεγμάτων. Αναπτύξτε **τους ρόλους** και, στη συνέχεια, επισημάνετε τη διαθεσιμότητα ομάδα. Στην καρτέλα **πόροι** , κάντε δεξί κλικ στο όνομα ακρόασης και κάντε κλικ στην επιλογή **Ιδιότητες**.

1. Κάντε κλικ στην καρτέλα **εξαρτήσεις** . Εάν υπάρχουν πολλά πόρων που εμφανίζονται, επιβεβαιώστε ότι οι διευθύνσεις IP έχουν ή όχι AND, εξαρτήσεις. Κάντε κλικ στο **κουμπί OK**.

1. Κάντε δεξί κλικ στο όνομα ακρόασης και κάντε κλικ στην επιλογή **Σύνδεση**.

1. Όταν το ακροατήριο βρίσκεται στο Internet, στην καρτέλα **πόροι** , κάντε δεξί κλικ στην ομάδα διαθεσιμότητα και κάντε κλικ στην επιλογή **Ιδιότητες**.

1. Δημιουργία εξάρτησης στον πόρο όνομα ακρόασης (όχι η IP διεύθυνση πόρους όνομα). Κάντε κλικ στο **κουμπί OK**.

1. Εκκίνηση του SQL Server Management Studio και να συνδεθείτε με την κύρια ρεπλίκα.

1. Μεταβείτε στη **διαθεσιμότητα AlwaysOn υψηλής** | **ομάδες διαθεσιμότητας** | **ακροατών ομάδας διαθεσιμότητας**. 

1. Τώρα θα πρέπει να βλέπετε το όνομα ακρόασης που δημιουργήσατε στη Διαχείριση ανακατεύθυνσης συμπλεγμάτων. Κάντε δεξί κλικ στο όνομα ακρόασης και κάντε κλικ στην επιλογή **Ιδιότητες**.

1. Στο πλαίσιο **θύρα** , καθορίστε τον αριθμό θύρας για την παρακολούθηση ομάδα διαθεσιμότητα, χρησιμοποιώντας το $EndpointPort που χρησιμοποιήσατε νωρίτερα (1433 ήταν η προεπιλογή), στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

Τώρα έχετε μια ομάδα διαθεσιμότητα SQL Server στο Azure εικονικές μηχανές εκτελούνται σε λειτουργία διαχείρισης πόρων. 

## <a name="test-the-connection-to-the-listener"></a>Ελέγξτε τη σύνδεση για το ακροατήριο

Για να ελέγξετε τη σύνδεση:

1. RDP για SQL Server που είναι στο ίδιο δίκτυο εικονικού, αλλά δεν ανήκει στη ρεπλίκα. Αυτό μπορεί να τα άλλα SQL Server στο σύμπλεγμα.

2. Χρησιμοποιήστε πρόγραμμα **sqlcmd** για να ελέγξετε τη σύνδεση. Για παράδειγμα, η ακόλουθη δέσμη ενεργειών δημιουργεί μια σύνδεση **sqlcmd** στην κύρια ρεπλίκα έως το ακροατήριο με έλεγχο ταυτότητας των Windows:

    ```
    sqlmd -S <listenerName> -E
    ```

    Εάν το πρόγραμμα ακρόασης χρησιμοποιεί μια θύρα εκτός από την προεπιλεγμένη θύρα (1433), καθορίστε τη θύρα στη συμβολοσειρά σύνδεσης. Για παράδειγμα, την παρακάτω εντολή sqlcmd συνδέεται με μια υπηρεσία ακρόασης στη θύρα 1435: 
    
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

Η σύνδεση SQLCMD συνδέεται αυτόματα με οποιαδήποτε παρουσία του SQL Server φιλοξενεί πρωτεύον αντίγραφο. 

>[AZURE.NOTE] Βεβαιωθείτε ότι είναι ανοιχτό το τείχος προστασίας των δύο διακομιστών SQL τη θύρα που καθορίζετε. Οι διακομιστές και οι δύο απαιτούν έναν κανόνα εισερχομένων για τη θύρα TCP που χρησιμοποιείτε. Ανατρέξτε στο θέμα [Προσθήκη ή επεξεργασία κανόνα τείχους προστασίας](http://technet.microsoft.com/library/cc753558.aspx) για περισσότερες πληροφορίες. 

## <a name="guidelines-and-limitations"></a>Γενικές οδηγίες και περιορισμοί

Λάβετε υπόψη τις ακόλουθες οδηγίες σε ακρόασης ομάδα διαθεσιμότητα στο Azure χρησιμοποιώντας εσωτερική μονάδα εξισορρόπησης φόρτου:

- Με μια εσωτερική έχετε πρόσβαση μόνο το ακροατήριο από μέσα στο ίδιο δίκτυο εικονικού μονάδα εξισορρόπησης φόρτου.

## <a name="for-more-information"></a>Για περισσότερες πληροφορίες

Για περισσότερες πληροφορίες ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων πάντα στη διαθεσιμότητα ομαδοποίηση σε Εικονική Azure με μη αυτόματο τρόπο](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

### <a name="powershell-cmdlets"></a>Cmdlet του PowerShell

Χρησιμοποιήστε τα παρακάτω cmdlet του PowerShell για να δημιουργήσετε μια εσωτερική εξισορρόπηση φόρτου για Azure εικονικές μηχανές.

- `New-AzureRmLoadBalancer`δημιουργεί μια μονάδα εξισορρόπησης φόρτου. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) . 

- `New-AzureRMLoadBalancerFrontendIpConfig`δημιουργεί μια προσκηνίου ρύθμιση παραμέτρων διευθύνσεων IP για μια μονάδα εξισορρόπησης φόρτου. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) .

- `New-AzureRmLoadBalancerRuleConfig`δημιουργεί μια ρύθμιση παραμέτρων κανόνα για μια μονάδα εξισορρόπησης φόρτου. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) . 

- `New-AzureRMLoadBalancerBackendAddressPoolConfig`δημιουργεί μια ρύθμιση χώρου συγκέντρωσης διεύθυνσης παρασκηνίου για μια μονάδα εξισορρόπησης φόρτου. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) . 

- `New-AzureRmLoadBalancerProbeConfig`δημιουργεί μια ρύθμιση παραμέτρων δοκιμή του για μια μονάδα εξισορρόπησης φόρτου. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) .

Εάν θέλετε να καταργήσετε μια μονάδα εξισορρόπησης φόρτου από μια ομάδα Azure πόρων, χρησιμοποιήστε `Remove-AzureRmLoadBalancer`. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Κατάργηση AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx).
