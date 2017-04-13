<properties
   pageTitle="Δημιουργία VNet διεισδύουν με χρήση των cmdlet του Powershell | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα εικονικό δίκτυο με την πύλη Azure στη Διαχείριση πόρων."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai; annahar"/>

# <a name="create-vnet-peering-using-powershell-cmdlets"></a>Δημιουργία VNet διεισδύουν με χρήση των cmdlet του Powershell

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Για να δημιουργήσετε ένα VNet διεισδύουν με χρήση του PowerShell, ακολουθήστε τα παρακάτω βήματα:

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../powershell-install-configure.md) και ακολουθήστε τις οδηγίες προς το τέλος για να συνδεθείτε στο Azure και επιλέξτε τη συνδρομή σας.

> [AZURE.NOTE] Cmdlet του PowerShell για τη Διαχείριση VNet διεισδύουν αποστέλλεται με [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Ανάγνωση αντικειμένων εικονικού δικτύου:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. Για να δημιουργήσετε VNet διεισδύουν, πρέπει να δημιουργήσετε δύο συνδέσεις, έναν για κάθε κατεύθυνση. Το παρακάτω βήμα θα δημιουργήσετε μια σύνδεση peering VNet για VNet1 να VNet2 πρώτα:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

    Εμφανίζει το αποτέλεσμα:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Αυτό το βήμα θα δημιουργήσετε μια σύνδεση peering VNet για VNet2 να VNet1:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

    Εμφανίζει το αποτέλεσμα:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Όταν δημιουργηθεί η σύνδεση peering VNet, μπορείτε να δείτε την κατάσταση σύνδεσης παρακάτω:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Εμφανίζει το αποτέλεσμα:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Υπάρχουν μερικές ιδιότητες με δυνατότητα ρύθμισης παραμέτρων για διεισδύουν VNet:

  	|Επιλογή|Περιγραφή|Προεπιλεγμένη|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Εάν διευθύνσεων χώρο του VNet ομότιμης σύνδεσης που θα συμπεριληφθούν ως τμήμα της ετικέτας Virtual_network|Ναι|
  	|AllowForwardedTraffic|Εάν η κίνηση δεν που προέρχονται από μια peered VNet αποδοχή ή αποτεθεί|Όχι|
  	|AllowGatewayTransit|Επιτρέπει την ομότιμης σύνδεσης VNet για να χρησιμοποιήσετε την πύλη VNet|Όχι|
  	|UseRemoteGateways|Χρήση πύλης VNet σας ομότιμης σύνδεσης. Το ομότιμης σύνδεσης VNet πρέπει να έχετε μια πύλη έχει ρυθμιστεί και AllowGatewayTransit επιλεγμένο. Δεν μπορείτε να χρησιμοποιήσετε αυτήν την επιλογή, εάν έχετε μια πύλη έχει ρυθμιστεί|Όχι|

    Κάθε σύνδεση στο VNet διεισδύουν περιλαμβάνει το σύνολο των ιδιοτήτων παραπάνω. Για παράδειγμα, μπορείτε να ορίσετε AllowVirtualNetworkAccess σε True για σύνδεση διεισδύουν VNet VNet1 για να VNet2 και να τη ρυθμίσετε σε False για τη σύνδεση peering VNet προς την άλλη κατεύθυνση.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    Μπορείτε να εκτελέσετε Get-AzureRmVirtualNetworkPeering σε διπλά ελέγχου την τιμή της ιδιότητας μετά την αλλαγή. Από την έξοδο, μπορείτε να δείτε AllowForwardedTraffic αλλάζει σύνολο στην τιμή True μετά την εκτέλεση τα cmdlet του παραπάνω.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Αφού δημιουργηθεί διεισδύουν σε αυτό το σενάριο, θα πρέπει να μπορείτε να ξεκινήσετε το συνδέσεις από οποιαδήποτε εικονική μηχανή οποιαδήποτε εικονική μηχανή του VNets και τα δύο. Από προεπιλογή, AllowVirtualNetworkAccess είναι True και VNet διεισδύουν θα προμήθεια το πρώτο γράμμα κάθε λέξης ACL για να επιτρέψετε την επικοινωνία μεταξύ του VNets. Εξακολουθείτε να μπορείτε να εφαρμόσετε κανόνες ομάδας (NSG) ασφαλείας δικτύου για να αποκλείσετε τη σύνδεση μεταξύ συγκεκριμένων δευτερεύοντα δίκτυα ή εικονικές μηχανές να αποκτήσει λεπτομερές ως έλεγχο πρόσβασης μεταξύ δύο εικονικού δικτύων.  Για περισσότερες πληροφορίες σχετικά με τη δημιουργία κανόνων NSG, ανατρέξτε σε αυτό το [άρθρο](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Για να δημιουργήσετε VNet διεισδύουν στις συνδρομές χρησιμοποιώντας το PowerShell, ακολουθήστε τα παρακάτω βήματα:

1. Πραγματοποιήστε είσοδο στο Azure με δικαιώματα χρήστη-A του λογαριασμού για μια συνδρομή και εκτέλεση το ακόλουθο cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Δεν είναι η απαίτηση, διεισδύουν μπορεί να πραγματοποιηθεί ακόμα και εάν οι χρήστες μεμονωμένα αυξήσετε διεισδύουν αιτήσεις τους αντίστοιχους VNets με την προϋπόθεση ότι ταιριάζουν με τις αιτήσεις. Προσθήκη ενός χρήστη με δικαιώματα από τα άλλα VNet ως χρήστης στο το τοπικό VNet διευκολύνει να κάνετε την εγκατάσταση.

2. Πραγματοποιήστε είσοδο στο Azure με δικαιώματα χρήστη-B του λογαριασμού για τη συνδρομή-B και εκτελέστε το ακόλουθο cmdlet:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. Στο χρήστη-A της περιόδου λειτουργίας σύνδεσης, εκτελέστε το cmdlet παρακάτω:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. Σε μια περίοδο λειτουργίας σύνδεσης χρήστη B, εκτελέστε το cmdlet παρακάτω:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Αφού διεισδύουν είναι εγκατεστημένος, οποιαδήποτε εικονική μηχανή στο VNet3 πρέπει να μπορούν να επικοινωνούν με οποιαδήποτε εικονική μηχανή στο VNet5.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Σε αυτό το σενάριο, μπορείτε να εκτελέσετε τα παρακάτω για να δημιουργήσετε το διεισδύουν VNet cmdlet του PowerShell.  Πρέπει να ορίσετε την ιδιότητα AllowForwardedTraffic στην τιμή True και να συνδέσετε VNET1 HubVNet, το οποίο επιτρέπει την εισερχόμενη κυκλοφορία από εκτός του χώρου διευθύνσεων του peering VNet.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id

2. Αφού δημιουργηθεί διεισδύουν, μπορείτε να αναφέρονται σε αυτό το [άρθρο](virtual-network-create-udr-arm-ps.md) και να ορίσετε μια διαδρομή που ορίζονται από το χρήστη (UDR) για να ανακατευθύνετε την κίνηση VNet1 μέσω μια εικονική συσκευή για να χρησιμοποιήσετε τις δυνατότητές του. Όταν ορίζετε την επόμενη διεύθυνση μεταπήδησης στο τη διαδρομή, μπορείτε να ορίσετε τη διεύθυνση IP του εικονικού συσκευής στο το ομότιμης σύνδεσης VNet HubVNet. Ακολουθεί ένα δείγμα:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Για να δημιουργήσετε ένα VNet διεισδύουν μεταξύ ενός κλασική εικονικού δικτύου και ένα διαχειριστή πόρων Azure εικονικού δικτύου στο PowerShell, ακολουθήστε τα παρακάτω βήματα:

1. Ανάγνωση αντικείμενο εικονικού δικτύου για το **VNET1**, η διαχείριση πόρων Azure εικονικού δικτύου ως εξής:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. Για να δημιουργήσετε VNet διεισδύουν σε αυτό το σενάριο, μόνο μία σύνδεση είναι απαραίτητο, ειδικά μια σύνδεση από **VNET1** σε **VNET2**. Αυτό το βήμα απαιτεί γνωρίζετε το αναγνωριστικό σας κλασική VNet πόρου. Το Αναγνωριστικό ομάδας πόρου-μορφή έχει την παρακάτω:

        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Φροντίστε να αντικαταστήσετε SubscriptionID, ResourceGroupName και VirtualNetworkName με τα κατάλληλα ονόματα.

    Αυτό μπορεί να πραγματοποιηθεί από τα εξής:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Μία φορά την VNet δημιουργείται διεισδύουν σύνδεση, μπορείτε να δείτε την κατάσταση σύνδεσης όπως φαίνεται στο παρακάτω αποτέλεσμα:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>Κατάργηση διεισδύουν VNet

1.  Για να καταργήσετε το VNet διεισδύουν, πρέπει να εκτελέσετε το ακόλουθο cmdlet:

        Remove-AzureRmVirtualNetworkPeering  

        Remove both links, using the following commands:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Αφού καταργήσετε μια σύνδεση σε ένα διεισδύουν VNET, την κατάσταση σύνδεσης ομότιμης σύνδεσης θα μεταβείτε αποσύνδεσης. Σε αυτήν την κατάσταση, δεν μπορείτε να δημιουργήσετε εκ νέου τη σύνδεση μέχρι να αλλάξει η κατάσταση σύνδεσης ομότιμης σύνδεσης σε ξεκίνησε. Συνιστάται να καταργήσετε και τα δύο συνδέσεις πριν να δημιουργήσετε ξανά το διεισδύουν VNet.
