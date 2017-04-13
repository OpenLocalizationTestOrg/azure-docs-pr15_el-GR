<properties 
   pageTitle="Πώς μπορείτε να δημιουργήσετε NSGs σε κατάσταση λειτουργίας ARM χρησιμοποιώντας το Azure CLI | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε και να αναπτύξετε NSGs στο ARM χρησιμοποιώντας το CLI Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
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

# <a name="how-to-create-nsgs-in-the-azure-cli"></a>Πώς μπορείτε να δημιουργήσετε NSGs στο το CLI Azure

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε επίσης να [δημιουργήσετε NSGs στο μοντέλο κλασική ανάπτυξης](virtual-networks-create-nsg-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Οι εντολές Azure CLI δείγμα παρακάτω θεωρείτε ότι ένα απλό περιβάλλον ήδη δημιουργηθεί με βάση το παραπάνω σενάριο. Εάν θέλετε να εκτελέσετε τις εντολές που εμφανίζονται σε αυτό το έγγραφο, δημιουργία πρώτα το δοκιμαστικό περιβάλλον με [αυτό το πρότυπο](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd)για την ανάπτυξη, κάντε κλικ **Ανάπτυξη να Azure**, αντικαταστήστε τις προεπιλεγμένες τιμές παραμέτρων Εάν είναι απαραίτητο, και, στη συνέχεια, ακολουθήστε τις οδηγίες στην πύλη.

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Πώς μπορείτε να δημιουργήσετε το NSG για το υποδίκτυο προσκηνίου
Για να δημιουργήσετε μια NSG με όνομα επώνυμο *NSG FrontEnd* με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.

2. Εκτελέστε την εντολή **λειτουργία azure ρύθμισης παραμέτρων** για να μεταβείτε σε λειτουργία διαχείρισης πόρων, όπως φαίνεται παρακάτω.

        azure config mode arm

    Αναμενόμενο αποτέλεσμα:

        info:    New mode is arm

3. Εκτελέστε την εντολή **Δημιουργία nsg azure δικτύου** για να δημιουργήσετε μια NSG.

        azure network nsg create -g TestRG -l westus -n NSG-FrontEnd

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
        data:    Name                            : NSG-FrontEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK

    Παράμετροι:
    - **-g (ή--ομάδα πόρων)**. Το όνομα της ομάδας πόρων όπου θα δημιουργηθεί η NSG. Για το σενάριο, *TestRG*.
    - **-l (ή--θέση)**. Azure περιοχής όπου θα δημιουργηθεί το νέο NSG. Για το σενάριο, *westus*.
    - **-n (ή--όνομα)**. Όνομα για το νέο NSG. Για το σενάριο, *NSG FrontEnd*.

4. Εκτελέστε την εντολή **Δημιουργία κανόνα nsg azure δικτύου** για να δημιουργήσετε έναν κανόνα που επιτρέπει την πρόσβαση στη θύρα 3389 (RDP) από το Internet.

        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nsg rule create
        warn:    Using default direction: Inbound
        info:    Looking up the network security rule "rdp-rule"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp
        -rule
        data:    Name                            : rdp-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 3389
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Παράμετροι:

    - **-(ή--nsg-όνομα)**. Όνομα της το NSG στο οποίο θα δημιουργηθεί ο κανόνας. Για το σενάριο, *NSG FrontEnd*.
    - **-n (ή--όνομα)**. Όνομα για τον νέο κανόνα. Για το σενάριο, *rdp κανόνα*.
    - **-c (ή--πρόσβαση)**. Επίπεδο πρόσβασης για τον κανόνα (αποδοχή ή απόρριψη).
    - **-p (ή--protocol)**. Protocol (Tcp, Udp, ή *) για τον κανόνα.
    - **-r (ή--κατεύθυνση)**. Κατεύθυνση της σύνδεσης (εισερχομένων ή εξερχομένων).
    - **-y (ή--προτεραιότητα)**. Προτεραιότητα για τον κανόνα.
    - **-f (ή--πρόθεμα διεύθυνσης προέλευσης)**. Πρόθεμα διεύθυνσης προέλευσης σε CIDR ή χρησιμοποιώντας προεπιλεγμένες ετικέτες.
    - **-o (ή--περιοχής θύρας προέλευσης)**. Θύρα προέλευσης ή περιοχή θύρας.
    - **-e (ή--πρόθεμα διεύθυνσης προορισμού)**. Πρόθεμα διεύθυνσης προορισμού σε CIDR ή χρησιμοποιώντας προεπιλεγμένες ετικέτες.
    - **-u (ή--περιοχή θύρα προορισμού)**. Θύρα προορισμού ή περιοχή θύρας.    

5. Εκτελέστε την εντολή **Δημιουργία κανόνα nsg azure δικτύου** για να δημιουργήσετε έναν κανόνα που επιτρέπει την πρόσβαση στη θύρα 80 (HTTP) από το Internet.

        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Αναμενόμενη putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 80
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Εκτελέστε την εντολή **Ορισμός υποδίκτυο vnet azure δικτύου** για να συνδέσετε το NSG στο υποδίκτυο προσκηνίου.

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -o NSG-FrontEnd

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Πώς μπορείτε να δημιουργήσετε το NSG για το υποδίκτυο παρασκηνίου
Για να δημιουργήσετε μια NSG με όνομα επώνυμο *NSG παρασκηνίου* με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

3. Εκτελέστε την εντολή **Δημιουργία nsg azure δικτύου** για να δημιουργήσετε μια NSG.

        azure network nsg create -g TestRG -l westus -n NSG-BackEnd

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd
        data:    Name                            : NSG-BackEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK

4. Εκτελέστε την εντολή **Δημιουργία κανόνα nsg azure δικτύου** για να δημιουργήσετε έναν κανόνα που επιτρέπει την πρόσβαση στη θύρα 1433 (SQL) από το υποδίκτυο προσκηνίου.

        azure network nsg rule create -g TestRG -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "sql-rule"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule
        data:    Name                            : sql-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 1433
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

5. Εκτελέστε την εντολή **Δημιουργία κανόνα nsg azure δικτύου** για να δημιουργήσετε έναν κανόνα που αποτρέπει την πρόσβαση στο Internet από.

        azure network nsg rule create -g TestRG -a NSG-BackEnd -n web-rule -c Deny -p * -r Outbound -y 200 -f * -o * -e Internet -u *

    Αναμενόμενη putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : *
        data:    Source Port                     : *
        data:    Destination IP                  : Internet
        data:    Destination Port                : *
        data:    Protocol                        : *
        data:    Direction                       : Outbound
        data:    Access                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Εκτελέστε την εντολή **Ορισμός υποδίκτυο vnet azure δικτύου** για να συνδέσετε το NSG στο υποδίκτυο παρασκηνίου.

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -o NSG-BackEnd

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Setting subnet "BackEnd"
        info:    Looking up the subnet "BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/BackEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : BackEnd
        data:    Address prefix                  : 192.168.2.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL1/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL2/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
