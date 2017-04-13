<properties 
   pageTitle="Διαχείριση NSGs χρησιμοποιώντας το Azure CLI στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να διαχειριστείτε τις υπάρχουσες NSGs χρησιμοποιώντας το Azure CLI στη Διαχείριση πόρων"
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

# <a name="manage-nsgs-using-the-azure-cli"></a>Διαχείριση NSGs χρησιμοποιώντας το CLI Azure

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="retrieve-information"></a>Ανάκτηση πληροφοριών

Μπορείτε να προβάλετε τις υπάρχουσες NSGs, να ανακτήσετε κανόνες για μια υπάρχουσα NSG και να μάθετε ποια πόρους μια NSG είναι συσχετισμένη με.

### <a name="view-existing-nsgs"></a>NSGs υπάρχουσα προβολή

Για να προβάλετε τη λίστα των NSGs μιας συγκεκριμένης ομάδας πόρων, εκτελέστε το `azure network nsg list` εντολή όπως φαίνεται παρακάτω. 

    azure network nsg list --resource-group RG-NSG

Αναμενόμενο αποτέλεσμα:

    info:    Executing command network nsg list
    + Getting the network security groups
    data:    Name          Location
    data:    ------------  --------
    data:    NSG-BackEnd   westus
    data:    NSG-FrontEnd  westus
    info:    network nsg list command OK
         
### <a name="list-all-rules-for-an-nsg"></a>Λίστα όλοι οι κανόνες για μια NSG

Για να προβάλετε τους κανόνες της μια NSG ονομάζεται **Προσκήνιο NSG**, εκτελέστε το `azure network nsg show` εντολή όπως φαίνεται παρακάτω. 

    azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd

Αναμενόμενο αποτέλεσμα:
    
    info:    Executing command network nsg show
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Name                            : NSG-FrontEnd
    data:    Type                            : Microsoft.Network/networkSecurityGroups
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    Tags                            : displayName=NSG - Front End
    data:    Security group rules:
    data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
    data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
    data:    rdp-rule                       Internet           *            *               3389              Tcp       Inbound    Allow   100
    data:    web-rule                       Internet           *            *               80                Tcp       Inbound    Allow   101
    data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000
    data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001
    data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500
    data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000
    data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001
    data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500
    info:    network nsg show command OK

>[AZURE.NOTE] Μπορείτε επίσης να χρησιμοποιήσετε `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` στη λίστα των κανόνων από το **NSG FrontEnd** NSG.

### <a name="view-nsg-associations"></a>Προβολή NSG συσχετίσεων

Για να δείτε τι είναι το NSG **NSG FrontEnd** πόρους συσχετίσετε με, εκτελέστε το `azure network nsg show` εντολή όπως φαίνεται παρακάτω. Παρατηρήστε ότι η μόνη διαφορά είναι η χρήση της παραμέτρου **--json** .

    azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json

Αναζητήστε τις ιδιότητες **networkInterfaces** και **δευτερεύοντα δίκτυα** , όπως φαίνεται παρακάτω:

    "networkInterfaces": [],
    ...
    "subnets": [
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
        }
    ],
    ...

Στο παραπάνω παράδειγμα, το NSG δεν είναι συσχετισμένη με οποιαδήποτε διασυνδέσεις δικτύου (NIC) και έχει συσχετιστεί με ένα δευτερεύον που ονομάζεται **προσκήνιο**.

## <a name="manage-rules"></a>Διαχείριση κανόνων

Μπορείτε να προσθέσετε κανόνες για μια υπάρχουσα NSG, να επεξεργαστείτε υπάρχοντα τους κανόνες και να καταργήσετε κανόνες.

### <a name="add-a-rule"></a>Προσθήκη κανόνα

Για να προσθέσετε έναν κανόνα που υπάρχει δυνατότητα **εισερχόμενη** κυκλοφορία θύρα **443** από οποιονδήποτε υπολογιστή για να το **NSG FrontEnd** NSG, εκτελέστε το `azure network nsg rule create` εντολή όπως φαίνεται παρακάτω.

    azure network nsg rule create --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --description "Allow access to port 443 for HTTPS" \
        --protocol Tcp \
        --source-address-prefix * \
        --source-port-range * \
        --destination-address-prefix * \
        --destination-port-range 443 \
        --access Allow \
        --priority 102 \
        --direction Inbound     

Αναμενόμενο αποτέλεσμα:

    info:    Executing command network nsg rule create
    + Looking up the network security rule "allow-https"
    + Creating a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : *
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule create command OK

### <a name="change-a-rule"></a>Αλλαγή ενός κανόνα

Για να αλλάξετε τον κανόνα που δημιουργήσατε παραπάνω για να επιτρέψετε την εισερχόμενη κυκλοφορία από το **Internet** μόνο, εκτελέστε το `azure network nsg rule set` εντολή όπως φαίνεται παρακάτω.

    azure network nsg rule set --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --source-address-prefix Internet

Αναμενόμενο αποτέλεσμα:

    info:    Executing command network nsg rule set
    + Looking up the network security group "NSG-FrontEnd"
    + Setting a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : Internet
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule set command OK

### <a name="delete-a-rule"></a>Διαγραφή κανόνα

Για να διαγράψετε τον κανόνα που δημιουργήσατε παραπάνω, εκτελέστε το `azure network nsg rule delete` εντολή όπως φαίνεται παρακάτω.

    azure network nsg rule delete --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --quiet

>[AZURE.NOTE] Το **--χωρίς μηνύματα** παραμέτρου εξασφαλίζει ότι δεν χρειάζεται να επιβεβαιώσετε τη διαγραφή.

Αναμενόμενο αποτέλεσμα:

    info:    Executing command network nsg rule delete
    + Looking up the network security group "NSG-FrontEnd"
    + Deleting network security rule "allow-https"
    info:    network nsg rule delete command OK

## <a name="manage-associations"></a>Διαχείριση συσχετίσεων

Μπορείτε να συσχετίσετε ένα NSG σε δευτερεύοντα δίκτυα και NIC. Μπορείτε επίσης να διαχωρίσετε μια NSG από οποιονδήποτε πόρο που συσχετίζεται με.

### <a name="associate-an-nsg-to-a-nic"></a>Συσχετισμός ενός NSG στο NIC

Για να συσχετίσετε το **NSG FrontEnd** NSG να **TestNICWeb1** NIC, εκτελέστε το `azure network nic set` εντολή όπως φαίνεται παρακάτω.

    azure network nic set --resource-group RG-NSG \
        --name TestNICWeb1 \
        --network-security-group-name NSG-FrontEnd

Αναμενόμενο αποτέλεσμα:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Looking up the network security group "NSG-FrontEnd"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-nic"></a>Καταργούν ένα NSG από NIC

Για να διαχωρίσετε το **NSG FrontEnd** NSG από το **TestNICWeb1** NIC, εκτελέστε το `azure network nic set` εντολή όπως φαίνεται παρακάτω.

    azure network nic set --resource-group RG-NSG --name TestNICWeb1 --network-security-group-id ""

>[AZURE.NOTE] Ειδοποίηση του "" (κενή) τιμή για την παράμετρο **δικτύου-ασφάλεια--αναγνωριστικό ομάδας** . Που είναι πώς μπορείτε να καταργήσετε μια συσχέτιση για μια NSG. Δεν μπορείτε να κάνετε το ίδιο με την παράμετρο **ασφάλεια-ομάδα-όνομα δικτύου** .

Αναμενόμενο αποτέλεσμα:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-subnet"></a>Καταργούν ένα NSG από ένα δευτερεύον δίκτυο

Για να διαχωρίσετε το **NSG FrontEnd** NSG από το υποδίκτυο **FrontEnd** , εκτελέστε το `azure network vnet subnet set` εντολή όπως φαίνεται παρακάτω.

    azure network vnet subnet set --resource-group RG-NSG \
        --vnet-name TestVNet \
        --name FrontEnd \
        --network-security-group-id ""

Αναμενόμενο αποτέλεσμα:

    info:    Executing command network vnet subnet set
    + Looking up the subnet "FrontEnd"
    + Setting subnet "FrontEnd"
    + Looking up the subnet "FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:    Type                            : Microsoft.Network/virtualNetworks/subnets
    data:    ProvisioningState               : Succeeded
    data:    Name                            : FrontEnd
    data:    Address prefix                  : 192.168.1.0/24
    data:    IP configurations:
    data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
    data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
    data:
    info:    network vnet subnet set command OK

### <a name="associate-an-nsg-to-a-subnet"></a>Συσχετισμός ενός NSG με ένα δευτερεύον

Για να συσχετίσετε το **NSG FrontEnd** NSG στο υποδίκτυο **FronEnd** ξανά, εκτελέστε το `azure network vnet subnet set` εντολή όπως φαίνεται παρακάτω.

    azure network vnet subnet set --resource-group RG-NSG \
        --vnet-name TestVNet \
        --name FrontEnd \
        --network-security-group-name NSG-FronEnd

>[AZURE.NOTE] Η εντολή παραπάνω λειτουργεί μόνο επειδή το NSG **NSG FrontEnd** είναι στην ίδια ομάδα πόρων ως το εικονικό δίκτυο **TestVNet**. Εάν το NSG βρίσκεται σε μια ομάδα διαφορετικό πόρο, πρέπει να χρησιμοποιήσετε το **--δικτύου-ασφάλεια--αναγνωριστικό ομάδας** παράμετρο αντί για αυτό, και δώστε το πλήρες αναγνωριστικό για το NSG. Μπορείτε να ανακτήσετε το αναγνωριστικό εκτελώντας **Εμφάνιση nsg azure δικτύου--ομάδα πόρων RG-NSG--όνομα NSG FrontEnd--json** και αναζητάτε την ιδιότητα " **αναγνωριστικό** ". 

Αναμενόμενο αποτέλεσμα:

        info:    Executing command network vnet subnet set
        + Looking up the subnet "FrontEnd"
        + Looking up the network security group "NSG-FrontEnd"
        + Setting subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
        data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
        data:
        info:    network vnet subnet set command OK

## <a name="delete-an-nsg"></a>Διαγραφή ενός NSG

Μπορείτε να διαγράψετε μια NSG μόνο εάν δεν έχει συνδεθεί σε κάθε πόρο. Για να διαγράψετε μια NSG, ακολουθήστε τα παρακάτω βήματα.

1. Για να ελέγξετε τους πόρους που σχετίζονται με μια NSG, εκτελέστε το `azure network nsg show` όπως φαίνεται στην [Προβολή NSGs συσχετίσεων](#View-NSGs-associations).
2. Εάν το NSG είναι συσχετισμένη με οποιαδήποτε NIC, εκτελέστε το `azure network nic set` όπως φαίνεται στο [Dissociate ένα NSG από NIC](#Dissociate-an-NSG-from-a-NIC) για κάθε NIC. 
3. Εάν το NSG είναι συσχετισμένη με οποιαδήποτε υποδίκτυο, εκτελέστε το `azure network vnet subnet set` όπως φαίνεται στο [Dissociate ένα NSG από ένα δευτερεύον δίκτυο](#Dissociate-an-NSG-from-a-subnet) για κάθε υποδίκτυο.
4. Για να διαγράψετε το NSG, εκτελέστε το `azure network nsg delete` εντολή όπως φαίνεται παρακάτω.

        azure network nsg delete --resource-group RG-NSG --name NSG-FrontEnd --quiet

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nsg delete
        + Looking up the network security group "NSG-FrontEnd"
        + Deleting network security group "NSG-FrontEnd"
        info:    network nsg delete command OK

## <a name="next-steps"></a>Επόμενα βήματα

- [Ενεργοποίηση καταγραφής](virtual-network-nsg-manage-log.md) για NSGs.