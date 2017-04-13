<properties 
   pageTitle="Διαχείριση NSGs χρήση του PowerShell στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς να διαχειρίζεστε exising NSGs χρήση του PowerShell στη Διαχείριση πόρων"
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
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-powershell"></a>Διαχείριση NSGs χρήση του PowerShell

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Ανάκτηση πληροφοριών

Μπορείτε να προβάλετε τις υπάρχουσες NSGs, να ανακτήσετε κανόνες για μια υπάρχουσα NSG και να μάθετε ποια πόρους μια NSG είναι συσχετισμένη με.

### <a name="view-existing-nsgs"></a>NSGs υπάρχουσα προβολή
Για να προβάλετε όλες τις υπάρχουσες NSGs μια συνδρομή, εκτελέστε το `Get-AzureRmNetworkSecurityGroup` cmdlet όπως φαίνεται παρακάτω.

    Get-AzureRmNetworkSecurityGroup

Αναμενόμενο αποτέλεσμα:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
                            
    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Για να προβάλετε τη λίστα των NSGs μιας συγκεκριμένης ομάδας πόρων, εκτελέστε το `Get-AzureRmNetworkSecurityGroup` cmdlet όπως φαίνεται παρακάτω. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG

Αναμενόμενο αποτέλεσμα:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
         
### <a name="list-all-rules-for-an-nsg"></a>Λίστα όλοι οι κανόνες για μια NSG

Για να προβάλετε τους κανόνες της μια NSG ονομάζεται **Προσκήνιο NSG**, εκτελέστε το `Get-AzureRmNetworkSecurityGroup` cmdlet όπως φαίνεται παρακάτω. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules

Αναμενόμενο αποτέλεσμα:
    
    Name                     : rdp-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound
    
    Name                     : web-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

>[AZURE.NOTE] Μπορείτε επίσης να χρησιμοποιήσετε `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` για να εμφανίσετε τους προεπιλεγμένους κανόνες από την **NSG FrontEnd** NSG.

### <a name="view-nsgs-associations"></a>Προβολή NSGs συσχετίσεων

Για να δείτε τι είναι το NSG **NSG FrontEnd** πόρους συσχετίσετε με, εκτελέστε το `Get-AzureRmNetworkSecurityGroup` cmdlet όπως φαίνεται παρακάτω.

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd

Αναζητήστε τις ιδιότητες **NetworkInterfaces** και **δευτερεύοντα δίκτυα** όπως φαίνεται παρακάτω:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

Στο παραπάνω παράδειγμα, το NSG δεν είναι συσχετισμένη με οποιαδήποτε διασυνδέσεις δικτύου (NIC) και έχει συσχετιστεί με ένα δευτερεύον που ονομάζεται **προσκήνιο**.

## <a name="manage-rules"></a>Διαχείριση κανόνων

Μπορείτε να προσθέσετε κανόνες για μια υπάρχουσα NSG, να επεξεργαστείτε υπάρχοντα τους κανόνες και να καταργήσετε κανόνες.

### <a name="add-a-rule"></a>Προσθήκη κανόνα

Για να προσθέσετε έναν κανόνα που υπάρχει δυνατότητα **εισερχόμενη** κυκλοφορία θύρα **443** από οποιονδήποτε υπολογιστή για να το **NSG FrontEnd** NSG, ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε το `Get-AzureRmNetworkSecurityGroup` cmdlet για να ανακτήσετε τα υπάρχοντα NSG και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Εκτελέστε το `Add-AzureRmNetworkSecurityRuleConfig` cmdlet, όπως φαίνεται παρακάτω.

        Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange * `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Για να αποθηκεύσετε τις αλλαγές που κάνατε το NSG, εκτελέστε το `Set-AzureRmNetworkSecurityGroup` cmdlet όπως φαίνεται παρακάτω.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Αναμενόμενο αποτέλεσμα που εμφανίζει μόνο τους κανόνες ασφαλείας:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Αλλαγή ενός κανόνα

Για να αλλάξετε τον κανόνα που δημιουργήσατε παραπάνω για να επιτρέψετε την εισερχόμενη κυκλοφορία από το **Internet** μόνο, ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε το `Get-AzureRmNetworkSecurityGroup` cmdlet για να ανακτήσετε τα υπάρχοντα NSG και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Εκτελέστε το `Set-AzureRmNetworkSecurityRuleConfig` cmdlet, όπως φαίνεται παρακάτω.

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange Internet `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Για να αποθηκεύσετε τις αλλαγές που κάνατε το NSG, εκτελέστε το `Set-AzureRmNetworkSecurityGroup` cmdlet όπως φαίνεται παρακάτω.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Αναμενόμενο αποτέλεσμα που εμφανίζει μόνο τους κανόνες ασφαλείας:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Διαγραφή κανόνα

1. Εκτελέστε το `Get-AzureRmNetworkSecurityGroup` cmdlet για να ανακτήσετε τα υπάρχοντα NSG και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Εκτελέστε το `Remove-AzureRmNetworkSecurityRuleConfig` cmdlet, όπως φαίνεται παρακάτω.

        Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule

3. Για να αποθηκεύσετε τις αλλαγές που κάνατε το NSG, εκτελέστε το `Set-AzureRmNetworkSecurityGroup` cmdlet όπως φαίνεται παρακάτω.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Αναμενόμενο αποτέλεσμα που εμφανίζει μόνο τους κανόνες ασφαλείας, ειδοποίηση στον **κανόνα https** δεν εμφανίζεται πλέον:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Διαχείριση συσχετίσεων

Μπορείτε να συσχετίσετε ένα NSG σε δευτερεύοντα δίκτυα και NIC. Μπορείτε επίσης να διαχωρίσετε μια NSG από οποιονδήποτε πόρο που συσχετίζεται με.

### <a name="associate-an-nsg-to-a-nic"></a>Συσχετισμός ενός NSG στο NIC

Για να συσχετίσετε το **NSG FrontEnd** NSG να **TestNICWeb1** NIC, ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε το `Get-AzureRmNetworkSecurityGroup` cmdlet για να ανακτήσετε τα υπάρχοντα NSG και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Εκτελέστε το `Get-AzureRmNetworkInterface` cmdlet για να ανακτήσετε τα υπάρχοντα NIC και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Ορίστε την ιδιότητα **NetworkSecurityGroup** της μεταβλητής **NIC** στην τιμή της μεταβλητής **NSG** , όπως φαίνεται παρακάτω.

        $nic.NetworkSecurityGroup = $nsg

4. Για να αποθηκεύσετε τις αλλαγές που έγιναν στο NIC, εκτελέστε το `Set-AzureRmNetworkInterface` cmdlet όπως φαίνεται παρακάτω.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Αναμενόμενο αποτέλεσμα που εμφανίζει μόνο την ιδιότητα **NetworkSecurityGroup** :

        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Καταργούν ένα NSG από NIC

Για να διαχωρίσετε το **NSG FrontEnd** NSG από το **TestNICWeb1** NIC, ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε το `Get-AzureRmNetworkSecurityGroup` cmdlet για να ανακτήσετε τα υπάρχοντα NSG και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Εκτελέστε το `Get-AzureRmNetworkInterface` cmdlet για να ανακτήσετε τα υπάρχοντα NIC και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Ορίστε την ιδιότητα **NetworkSecurityGroup** της μεταβλητής **NIC** σε **$null**, όπως φαίνεται παρακάτω.

        $nic.NetworkSecurityGroup = $null

4. Για να αποθηκεύσετε τις αλλαγές που έγιναν στο NIC, εκτελέστε το `Set-AzureRmNetworkInterface` cmdlet όπως φαίνεται παρακάτω.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Αναμενόμενο αποτέλεσμα που εμφανίζει μόνο την ιδιότητα **NetworkSecurityGroup** :

        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Καταργούν ένα NSG από ένα δευτερεύον δίκτυο

Για να διαχωρίσετε το **NSG FrontEnd** NSG από το υποδίκτυο **FrontEnd** , ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε το `Get-AzureRmVirtualNetwork` cmdlet για να ανακτήσετε τα υπάρχοντα VNet και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Εκτελέστε το `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet για να ανακτήσετε το υποδίκτυο **FrontEnd** και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

3. Ορίστε την ιδιότητα **NetworkSecurityGroup** της μεταβλητής **υποδικτύου** σε **$null**, όπως φαίνεται παρακάτω.

        $subnet.NetworkSecurityGroup = $null

4. Για να αποθηκεύσετε τις αλλαγές που έγιναν στο υποδίκτυο, εκτελέστε το `Set-AzureRmVirtualNetwork` cmdlet όπως φαίνεται παρακάτω.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Αναμενόμενο αποτέλεσμα που εμφανίζει μόνο τις ιδιότητες του υποδικτύου **FrontEnd** . Παρατηρήστε δεν υπάρχει μια ιδιότητα για **NetworkSecurityGroup**:

            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Συσχετισμός ενός NSG με ένα δευτερεύον

Για να συσχετίσετε το **NSG FrontEnd** NSG στο υποδίκτυο **FronEnd** ξανά, ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε το `Get-AzureRmVirtualNetwork` cmdlet για να ανακτήσετε τα υπάρχοντα VNet και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Εκτελέστε το `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet για να ανακτήσετε το υποδίκτυο **FrontEnd** και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

1. Εκτελέστε το `Get-AzureRmNetworkSecurityGroup` cmdlet για να ανακτήσετε τα υπάρχοντα NSG και αποθηκεύστε το σε μια μεταβλητή, όπως φαίνεται παρακάτω.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

3. Ορίστε την ιδιότητα **NetworkSecurityGroup** της μεταβλητής **υποδικτύου** σε **$null**, όπως φαίνεται παρακάτω.

        $subnet.NetworkSecurityGroup = $nsg

4. Για να αποθηκεύσετε τις αλλαγές που έγιναν στο υποδίκτυο, εκτελέστε το `Set-AzureRmVirtualNetwork` cmdlet όπως φαίνεται παρακάτω.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Αναμένεται εξόδου που εμφανίζει μόνο την ιδιότητα **NetworkSecurityGroup** του υποδικτύου **FrontEnd** :

        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Διαγραφή ενός NSG

Μπορείτε να διαγράψετε μια NSG μόνο εάν δεν έχει συνδεθεί σε κάθε πόρο. Για να διαγράψετε μια NSG, ακολουθήστε τα παρακάτω βήματα.

1. Για να ελέγξετε τους πόρους που σχετίζονται με μια NSG, εκτελέστε το `azure network nsg show` όπως φαίνεται στην [Προβολή NSGs συσχετίσεων](#View-NSGs-associations).
2. Εάν το NSG είναι συσχετισμένη με οποιαδήποτε NIC, εκτελέστε το `azure network nic set` όπως φαίνεται στο [Dissociate ένα NSG από NIC](#Dissociate-an-NSG-from-a-NIC) για κάθε NIC. 
3. Εάν το NSG είναι συσχετισμένη με οποιαδήποτε υποδίκτυο, εκτελέστε το `azure network vnet subnet set` όπως φαίνεται στο [Dissociate ένα NSG από ένα δευτερεύον δίκτυο](#Dissociate-an-NSG-from-a-subnet) για κάθε υποδίκτυο.
4. Για να διαγράψετε το NSG, εκτελέστε το `Remove-AzureRmNetworkSecurityGroup` cmdlet όπως φαίνεται παρακάτω.

        Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force

    >[AZURE.NOTE] Το **-ισχύ** παραμέτρου εξασφαλίζει ότι δεν χρειάζεται να επιβεβαιώσετε τη διαγραφή.

## <a name="next-steps"></a>Επόμενα βήματα

- [Ενεργοποίηση καταγραφής](virtual-network-nsg-manage-log.md) για NSGs.