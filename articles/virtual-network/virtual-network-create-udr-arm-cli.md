<properties 
   pageTitle="Ελέγχετε τη δρομολόγηση και χρήση εικονικού συσκευές στη Διαχείριση πόρων χρησιμοποιώντας το Azure CLI | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να ελέγξετε τη δρομολόγηση και να χρησιμοποιήσετε εικονικού συσκευές χρησιμοποιώντας το CLI Azure"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-the-azure-cli"></a>Δημιουργία ορίζονται από το χρήστη δρομολογεί (UDR) στο το Azure CLI

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε επίσης να [δημιουργήσετε UDRs στο μοντέλο κλασική ανάπτυξης](virtual-network-create-udr-classic-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Οι εντολές Azure CLI δείγμα παρακάτω θεωρείτε ότι ένα απλό περιβάλλον ήδη δημιουργηθεί με βάση το παραπάνω σενάριο. Εάν θέλετε να εκτελέσετε τις εντολές που εμφανίζονται σε αυτό το έγγραφο, πρώτα δημιουργία δοκιμαστικό περιβάλλον με [αυτό το πρότυπο](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before)για την ανάπτυξη, κάντε κλικ στην επιλογή **Ανάπτυξη να Azure**, αντικαταστήστε τις προεπιλεγμένες τιμές παραμέτρων Εάν είναι απαραίτητο, και ακολουθήστε τις οδηγίες στην πύλη.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Δημιουργία του UDR για το υποδίκτυο προσκηνίου
Για να δημιουργήσετε τον πίνακα δρομολόγηση και δρομολόγηση που απαιτείται για το υποδίκτυο προσκηνίου με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

3. Εκτελέστε το **`azure network route-table create`** εντολή για να δημιουργήσετε έναν πίνακα διαδρομών για το υποδίκτυο προσκηνίου.

        azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest

    Αποτέλεσμα:

        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK

    Παράμετροι:
    - **-g (ή--ομάδα πόρων)**. Το όνομα της ομάδας πόρων όπου θα δημιουργηθεί η UDR. Για το σενάριο, *TestRG*.
    - **-l (ή--θέση)**. Azure περιοχής όπου θα δημιουργηθεί το νέο UDR. Για το σενάριο, *westus*.
    - **-n (ή--όνομα)**. Όνομα για τη νέα UDR. Για το σενάριο, *UDR FrontEnd*.

4. Εκτελέστε το **`azure network route-table route create`** εντολή για να δημιουργήσετε μια διαδρομή στον πίνακα δρομολόγηση δημιουργήθηκε παραπάνω για να στείλετε όλη την κυκλοφορία που προορίζονται για το υποδίκτυο παρασκηνίου (192.168.2.0/24) για να το **FW1** Εικονική (192.168.0.4).

        azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4

    Αποτέλεσμα:

        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK

    Παράμετροι:
    - **-r (ή--όνομα πίνακα δρομολόγηση)**. Το όνομα του πίνακα δρομολόγηση όπου θα προστεθούν τη διαδρομή. Για το σενάριο, *UDR FrontEnd*.
    - **-(ή--πρόθεμα διεύθυνσης)**. Πρόθεμα διεύθυνσης για το υποδίκτυο όπου πακέτα προορίζονται για. Για το σενάριο, *192.168.2.0/24*.
    - **-y (--Επόμενο μεταπήδηση-τύπου ή)**. Τύπος αντικειμένου κυκλοφορίας θα σταλεί σε. Πιθανές τιμές είναι *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*ή *κανένα*.
    - **-p (ή--Επόμενο-μεταπήδηση--διεύθυνση ip**). Η διεύθυνση IP για το επόμενο μεταπήδησης. Για το σενάριο, *192.168.0.4*.

5. Εκτελέστε το **`azure network vnet subnet set`** εντολή για να συσχετίσετε τον πίνακα δρομολόγηση δημιουργήσατε παραπάνω με το υποδίκτυο **FrontEnd** .

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd

    Αποτέλεσμα:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

    Παράμετροι:
    - **-e (ή--vnet-όνομα)**. Όνομα το VNet όπου βρίσκεται το υποδίκτυο. Για το σενάριο, *TestVNet*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Δημιουργία του UDR για το υποδίκτυο παρασκηνίου
Για να δημιουργήσετε τον πίνακα δρομολόγηση και δρομολόγηση που απαιτείται για το υποδίκτυο παρασκηνίου με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε το **`azure network route-table create`** εντολή για να δημιουργήσετε έναν πίνακα διαδρομών για το υποδίκτυο παρασκηνίου.

        azure network route-table create -g TestRG -n UDR-BackEnd -l westus

4. Εκτελέστε το **`azure network route-table route create`** εντολή για να δημιουργήσετε μια διαδρομή στον πίνακα δρομολόγηση δημιουργήθηκε παραπάνω για να στείλετε όλη την κυκλοφορία που προορίζονται για το υποδίκτυο προσκηνίου (192.168.1.0/24) για να το **FW1** Εικονική (192.168.0.4).

        azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4

5. Εκτελέστε το **`azure network vnet subnet set`** εντολή για να συσχετίσετε τον πίνακα δρομολόγηση δημιουργήσατε παραπάνω με το υποδίκτυο **παρασκηνίου** .

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd

## <a name="enable-ip-forwarding-on-fw1"></a>Ενεργοποίηση της προώθησης IP σε FW1
Για να ενεργοποιήσετε την προώθηση στο NIC που χρησιμοποιούνται από **FW1**IP, ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε το **`azure network nic show`** εντολή και, παρατηρήστε την τιμή για την **Προώθηση Ενεργοποίηση IP**. Αυτό πρέπει να οριστεί στην *τιμή false*.

        azure network nic show -g TestRG -n NICFW1

    Αποτέλεσμα:
        
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK

2. Εκτελέστε το **`azure network nic set`** εντολή για να ενεργοποιήσετε την προώθηση IP.

        azure network nic set -g TestRG -n NICFW1 -f true

    Αποτέλεσμα:

        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK

    Παράμετροι:

    - **-f (ή--προώθηση Ενεργοποίηση ip)**. *True* ή *false*.
