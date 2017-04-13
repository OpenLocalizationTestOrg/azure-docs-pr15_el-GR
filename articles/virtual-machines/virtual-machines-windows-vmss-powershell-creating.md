<properties
    pageTitle="Δημιουργία συνόλων κλίμακα εικονική μηχανή με χρήση των cmdlet του PowerShell | Microsoft Azure"
    description="Να ξεκινήσετε τη δημιουργία και διαχείριση πρώτη Azure εικονική μηχανή κλίμακα σύνολα σας με χρήση των cmdlet του Azure PowerShell"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="danielsollondon"/>

# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a>Δημιουργία συνόλων κλίμακα εικονική μηχανή με χρήση των cmdlet του PowerShell

Αυτό είναι ένα παράδειγμα του τρόπου για να δημιουργήσετε μια εικονική μηχανή Set(VMSS) κλίμακα, δημιουργεί μια VMSS του 3 κόμβους, με όλες τις σχετικές δικτύωση και χώρου αποθήκευσης.

## <a name="first-steps"></a>Πρώτα βήματα
Βεβαιωθείτε ότι έχετε εγκατεστημένη την πιο πρόσφατη λειτουργική μονάδα Azure PowerShell, αυτή θα περιέχει το PowerShell commandlets χρειάζεται να διατηρήσετε και να δημιουργήσετε VMSS.
Επιλέξτε διαδοχικά τα εργαλεία εντολών [εδώ](http://aka.ms/webpi-azps) για τις πιο πρόσφατες διαθέσιμες Azure λειτουργικές μονάδες.

Για να βρείτε VMSS που σχετίζονται με commandlets, χρησιμοποιήστε τη συμβολοσειρά αναζήτησης \*VMSS\*.

## <a name="creating-a-vmss"></a>Δημιουργία μιας VMSS

##### <a name="create-resource-group"></a>Δημιουργία ομάδας πόρων

```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

##### <a name="create-storage-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης

Ορισμός τύπου λογαριασμού χώρου αποθήκευσης / όνομα.

```
$stoname = 'sto' + $rgname;
$stotype = 'Standard_LRS';
 New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname -Location $loc -SkuName $stotype -Kind "Storage";

$stoaccount = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname;
```

#### <a name="create-networking-vnet--subnet"></a>Δημιουργία δικτύωση (VNET / υποδίκτυο)

##### <a name="subnet-specification"></a>Προδιαγραφή υποδίκτυο

```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

##### <a name="vnet-specification"></a>Προδιαγραφή VNET

```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

#In this case below we assume the new subnet is the only one, note difference if you have one already or have adjusted this code to more than one subnet.
$subnetId = $vnet.Subnets[0].Id;
```

##### <a name="create-public-ip-resource-to-allow-external-access"></a>Δημιουργία δημόσιας IP πόρων για να επιτρέψετε την πρόσβαση εξωτερικών

Αυτό θα είναι συνδεδεμένο με το για να τη μονάδα εξισορρόπησης φόρτου.

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

##### <a name="create-and-configure-load-balancer"></a>Δημιουργία και ρύθμιση παραμέτρων εξισορρόπηση φόρτου

```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

#Bind Public IP to Load Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

##### <a name="configure-load-balancer"></a>Ρύθμιση παραμέτρων εξισορρόπηση φόρτου
Δημιουργία ρύθμισης παραμέτρων χώρου συγκέντρωσης διεύθυνση παρασκηνίου, αυτό θα είναι κοινόχρηστη, το NIC του του ΣΠΣ στο VMSS.

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

Ορισμός φόρτωσης ισορροπημένες θύρας δοκιμή του, αλλάξτε τις ρυθμίσεις που είναι κατάλληλη για την εφαρμογή σας.

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

Δημιουργία κανόνων NAT για άμεση συνδεσιμότητα δρομολόγησης (όχι φόρτωση εξισορρόπηση) για να του ΣΠΣ στο το VMSS μέσω το πρόγραμμα εξισορρόπησης φόρτου, σημείωση αυτό παρουσιάζει τη χρήση RDP, αυτή είναι μόνο για επίδειξη και εσωτερικό μεθόδους VNET πρέπει να χρησιμοποιούνται για RDP'ing σε αυτούς τους διακομιστές.

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

Δημιουργήστε τη φόρτωση εξισορρόπηση κανόνα, αυτό το παράδειγμα εμφανίζει φόρτωση εξισορρόπησης θύρα 80 αιτήσεις, χρησιμοποιώντας τις ρυθμίσεις από τα προηγούμενα βήματα.

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

Δημιουργία εξισορρόπηση φόρτου με τη ρύθμιση παραμέτρων.

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

Ελέγξτε τις ρυθμίσεις Λίβρες, επιλέξτε διαμορφώσεων θύρα εξισορρόπησης φόρτου, σημείωση, δεν θα βλέπετε τους κανόνες εισερχομένων NAT μέχρι η Εικονική του στα το VMSS δημιουργούνται.

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-vmss"></a>Ρύθμιση παραμέτρων και δημιουργία VMSS

Σημείωση, αυτό το παράδειγμα υποδομή εμφανίζει τον τρόπο ρύθμισης διανομή και κλιμάκωση κυκλοφορία web κατά μήκος του VMSS, αλλά οι εικόνες ΣΠΣ που καθορίζεται εδώ δεν έχετε εγκατεστημένες οποιοδήποτε τις υπηρεσίες web.

```
#specify VMSS Name
$vmssName = 'vmss' + $rgname;

##specify VMSS specific details
$adminUsername = 'azadmin';
$adminPassword = "Password1234!";

$PublisherName = 'MicrosoftWindowsServer'
$Offer         = 'WindowsServer'
$Sku          = '2012-R2-Datacenter'
$Version       = 'latest'
$vmNamePrefix = 'winvmss'

$vhdContainer = "https://" + $stoname + ".blob.core.windows.net/" + $vmssName;

###add an extension
$extname = 'BGInfo';
$publisher = 'Microsoft.Compute';
$exttype = 'BGInfo';
$extver = '2.1';
```

Συνδέσετε NIC με εξισορρόπηση φόρτου και υποδίκτυο

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

Δημιουργία VMSS ρύθμισης παραμέτρων

```
#Specify number of nodes
$numberofnodes = 3

$vmss = New-AzureRmVmssConfig -Location $loc -SkuCapacity $numberofnodes -SkuName 'Standard_A2' -UpgradePolicyMode 'automatic' `
  	| Add-AzureRmVmssNetworkInterfaceConfiguration -Name $subnetName -Primary $true -IPConfiguration $ipCfg `
  	| Set-AzureRmVmssOSProfile -ComputerNamePrefix $vmNamePrefix -AdminUsername $adminUsername -AdminPassword $adminPassword `
  	| Set-AzureRmVmssStorageProfile -Name 'test' -OsDiskCreateOption 'FromImage' -OsDiskCaching 'None' `
    -ImageReferenceOffer $Offer -ImageReferenceSku $Sku -ImageReferenceVersion $Version `
    -ImageReferencePublisher $PublisherName -VhdContainer $vhdContainer `
  	| Add-AzureRmVmssExtension -Name $extname -Publisher $publisher -Type $exttype -TypeHandlerVersion $extver -AutoUpgradeMinorVersion $true
```

Δημιουργία VMSS ρύθμισης παραμέτρων

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

Τώρα που έχετε δημιουργήσει το VMSS. Μπορείτε να ελέγξετε τη σύνδεση με τα μεμονωμένα Εικονική χρησιμοποιώντας RDP σε αυτό το παράδειγμα:

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
