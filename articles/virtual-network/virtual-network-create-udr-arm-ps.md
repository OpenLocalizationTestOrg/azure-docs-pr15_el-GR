<properties 
   pageTitle="Έλεγχος δρομολόγηση και χρησιμοποιήστε εικονικού συσκευές στη Διαχείριση πόρων με χρήση του PowerShell | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να ελέγξετε τη δρομολόγηση και να χρησιμοποιήσετε εικονικού συσκευές στη Διαχείριση πόρων με χρήση του PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-resource-manager-by-using-powershell"></a>Δημιουργία δρομολογεί που ορίζονται από το χρήστη (UDR) στη Διαχείριση πόρων με χρήση του PowerShell

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε επίσης να [δημιουργήσετε UDRs στο μοντέλο κλασική ανάπτυξης](virtual-network-create-udr-classic-ps.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Το δείγμα PowerShell παρακάτω εντολές περιμένετε ένα απλό περιβάλλον που έχετε ήδη δημιουργήσει με βάση το παραπάνω σενάριο. Εάν θέλετε να εκτελέσετε τις εντολές που εμφανίζονται σε αυτό το έγγραφο, πρώτα δημιουργία δοκιμαστικό περιβάλλον με [αυτό το πρότυπο](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before)για την ανάπτυξη, κάντε κλικ στην επιλογή **Ανάπτυξη να Azure**, αντικαταστήστε τις προεπιλεγμένες τιμές παραμέτρων Εάν είναι απαραίτητο, και ακολουθήστε τις οδηγίες στην πύλη.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Δημιουργία του UDR για το υποδίκτυο προσκηνίου
Για να δημιουργήσετε τον πίνακα δρομολόγηση και δρομολόγηση που απαιτείται για το υποδίκτυο προσκηνίου με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Δημιουργήστε μια διαδρομή που χρησιμοποιείται για την αποστολή όλη την κυκλοφορία που προορίζονται για το υποδίκτυο παρασκηνίου (192.168.2.0/24) για να δρομολογούνται στη συσκευή εικονικού **FW1** (192.168.0.4).

        $route = New-AzureRmRouteConfig -Name RouteToBackEnd `
            -AddressPrefix 192.168.2.0/24 -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

4. Δημιουργήστε έναν πίνακα δρομολόγηση με το όνομα **UDR FrontEnd** στην περιοχή **westus** που περιέχει τη διαδρομή που δημιουργήθηκε παραπάνω.

        $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
            -Name UDR-FrontEnd -Route $route

5. Δημιουργήστε μια μεταβλητή που περιέχει το VNet πού βρίσκεται το υποδίκτυο. Σε σενάριο μας, το VNet ονομάζεται **TestVNet**.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet

6. Ο συσχετισμός δρομολόγηση δημιουργήθηκε παραπάνω στο υποδίκτυο **FrontEnd** .
        
        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            -AddressPrefix 192.168.1.0/24 -RouteTable $routeTable

>[AZURE.WARNING] Το αποτέλεσμα για την παραπάνω εντολή εμφανίζει το περιεχόμενο για το αντικείμενο ρύθμισης παραμέτρων εικονικού δικτύου, το οποίο υπάρχει μόνο στον υπολογιστή όπου εκτελείτε PowerShell. Πρέπει να εκτελέσετε το cmdlet **Set-AzureVirtualNetwork** για να αποθηκεύσετε αυτές τις ρυθμίσεις στο Azure.

7. Αποθηκεύστε τη νέα ρύθμιση παραμέτρων υποδικτύου στο Azure.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Αναμενόμενο αποτέλεσμα:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
                            
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                                ...,
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              },
                                ...
                            ]   

## <a name="create-the-udr-for-the-back-end-subnet"></a>Δημιουργία του UDR για το υποδίκτυο παρασκηνίου
Για να δημιουργήσετε τον πίνακα δρομολόγηση και δρομολόγηση που απαιτείται για το υποδίκτυο παρασκηνίου με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

1. Δημιουργήστε μια διαδρομή που χρησιμοποιείται για την αποστολή όλη την κυκλοφορία που προορίζονται για το υποδίκτυο προσκηνίου (192.168.1.0/24) για να δρομολογούνται στη συσκευή εικονικού **FW1** (192.168.0.4).

        $route = New-AzureRmRouteConfig -Name RouteToFrontEnd `
            -AddressPrefix 192.168.1.0/24 -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

4. Δημιουργήστε έναν πίνακα δρομολόγηση με το όνομα **UDR παρασκηνίου** στην περιοχή **uswest** που περιέχει τη διαδρομή που δημιουργήθηκε παραπάνω.

        $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
            -Name UDR-BackEnd -Route $route

5. Ο συσχετισμός δρομολόγηση δημιουργήθηκε παραπάνω στο υποδίκτυο **παρασκηνίου** .

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd `
            -AddressPrefix 192.168.2.0/24 -RouteTable $routeTable

7. Αποθηκεύστε τη νέα ρύθμιση παραμέτρων υποδικτύου στο Azure.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Αναμενόμενο αποτέλεσμα:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
                            
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              ...,
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BacEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="enable-ip-forwarding-on-fw1"></a>Ενεργοποίηση της προώθησης IP σε FW1
Για να ενεργοποιήσετε την προώθηση στο NIC που χρησιμοποιούνται από **FW1**IP, ακολουθήστε τα παρακάτω βήματα.

1. Δημιουργήστε μια μεταβλητή που περιέχει τις ρυθμίσεις για το NIC που χρησιμοποιείται από το FW1. Σε σενάριο μας, το NIC ονομάζεται **NICFW1**.

        $nicfw1 = Get-AzureRmNetworkInterface -ResourceGroupName TestRG -Name NICFW1

2. Ενεργοποίηση της προώθησης IP και αποθηκεύστε τις ρυθμίσεις NIC.

        $nicfw1.EnableIPForwarding = 1
        Set-AzureRmNetworkInterface -NetworkInterface $nicfw1

    Αναμενόμενο αποτέλεσμα:

        Name                 : NICFW1
        ResourceGroupName    : TestRG
        Location             : westus
        Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1
        Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState    : Succeeded
        Tags                 : 
                               Name         Value                  
                               ===========  =======================
                               displayName  NetworkInterfaces - DMZ
                               
        VirtualMachine       : {
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/FW1"
                               }
        IpConfigurations     : [
                                 {
                                   "Name": "ipconfig1",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1/ipConfigurations/ipconfig1",
                                   "PrivateIpAddress": "192.168.0.4",
                                   "PrivateIpAllocationMethod": "Static",
                                   "Subnet": {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/DMZ"
                                   },
                                   "PublicIpAddress": {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPFW1"
                                   },
                                   "LoadBalancerBackendAddressPools": [],
                                   "LoadBalancerInboundNatRules": [],
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
        DnsSettings          : {
                                 "DnsServers": [],
                                 "AppliedDnsServers": [],
                                 "InternalDnsNameLabel": null,
                                 "InternalFqdn": null
                               }
        EnableIPForwarding   : True
        NetworkSecurityGroup : null
        Primary              : True

