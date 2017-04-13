<properties
   pageTitle="Πώς μπορείτε να δημιουργήσετε NSGs σε κλασική λειτουργία χρησιμοποιώντας το Azure CLI | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε και να αναπτύξετε το NSGs Κλασική λειτουργία χρησιμοποιώντας το CLI Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Πώς μπορείτε να δημιουργήσετε NSGs (κλασικό) στο το CLI Azure

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο κλασική ανάπτυξης. Μπορείτε επίσης να [δημιουργήσετε NSGs στο μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-networks-create-nsg-arm-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Οι εντολές Azure CLI δείγμα παρακάτω θεωρείτε ότι ένα απλό περιβάλλον ήδη δημιουργηθεί με βάση το παραπάνω σενάριο. Εάν θέλετε να εκτελέσετε τις εντολές που εμφανίζονται σε αυτό το έγγραφο, δημιουργήστε πρώτα το περιβάλλον δοκιμής, [δημιουργώντας μια VNet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Πώς μπορείτε να δημιουργήσετε το NSG για το υποδίκτυο προσκηνίου
Για να δημιουργήσετε μια NSG με όνομα επώνυμο **NSG FrontEnd** με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.

2. Εκτελέστε το **`azure config mode`** εντολή για να μεταβείτε σε κλασική λειτουργία, όπως φαίνεται παρακάτω.

        azure config mode asm

    Αναμενόμενο αποτέλεσμα:

        info:    New mode is asm

3. Εκτελέστε το **`azure network nsg create`** εντολή για να δημιουργήσετε μια NSG.

        azure network nsg create -l uswest -n NSG-FrontEnd

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Παράμετροι:

    - **-l (ή--θέση)**. Azure περιοχής όπου θα δημιουργηθεί το νέο NSG. Για το σενάριο, *westus*.
    - **-n (ή--όνομα)**. Όνομα για το νέο NSG. Για το σενάριο, *NSG FrontEnd*.

4. Εκτελέστε το **`azure network nsg rule create`** εντολή για να δημιουργήσετε έναν κανόνα που επιτρέπει την πρόσβαση στη θύρα 3389 (RDP) από το Internet.

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Παράμετροι:

    - **-(ή--nsg-όνομα)**. Όνομα της το NSG στο οποίο θα δημιουργηθεί ο κανόνας. Για το σενάριο, *NSG FrontEnd*.
    - **-n (ή--όνομα)**. Όνομα για τον νέο κανόνα. Για το σενάριο, *rdp κανόνα*.
    - **-c (ή--ενέργεια)**. Επίπεδο πρόσβασης για τον κανόνα (αποδοχή ή απόρριψη).
    - **-p (ή--protocol)**. Protocol (Tcp, Udp, ή *) για τον κανόνα.
    - **-r (ή--τύπος)**. Κατεύθυνση της σύνδεσης (εισερχομένων ή εξερχομένων).
    - **-y (ή--προτεραιότητα)**. Προτεραιότητα για τον κανόνα.
    - **-f (ή--πρόθεμα διεύθυνσης προέλευσης)**. Πρόθεμα διεύθυνσης προέλευσης σε CIDR ή χρησιμοποιώντας προεπιλεγμένες ετικέτες.
    - **-o (ή--περιοχής θύρας προέλευσης)**. Θύρα προέλευσης ή περιοχή θύρας.
    - **-e (ή--πρόθεμα διεύθυνσης προορισμού)**. Πρόθεμα διεύθυνσης προορισμού σε CIDR ή χρησιμοποιώντας προεπιλεγμένες ετικέτες.
    - **-u (ή--περιοχή θύρα προορισμού)**. Θύρα προορισμού ή περιοχή θύρας.

5. Εκτελέστε το **`azure network nsg rule create`** εντολή για να δημιουργήσετε έναν κανόνα που επιτρέπει την πρόσβαση στη θύρα 80 (HTTP) από το Internet.

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Αναμενόμενη putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Εκτελέστε το **`azure network nsg subnet add`** εντολή για να συνδέσετε το NSG στο υποδίκτυο προσκηνίου.

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Πώς μπορείτε να δημιουργήσετε το NSG για το υποδίκτυο παρασκηνίου
Για να δημιουργήσετε μια NSG με όνομα επώνυμο *NSG παρασκηνίου* με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

3. Εκτελέστε το **`azure network nsg create`** εντολή για να δημιουργήσετε μια NSG.

        azure network nsg create -l uswest -n NSG-BackEnd

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Παράμετροι:

    - **-l (ή--θέση)**. Azure περιοχής όπου θα δημιουργηθεί το νέο NSG. Για το σενάριο, *westus*.
    - **-n (ή--όνομα)**. Όνομα για το νέο NSG. Για το σενάριο, *NSG FrontEnd*.

4. Εκτελέστε το **`azure network nsg rule create`** εντολή για να δημιουργήσετε έναν κανόνα που επιτρέπει την πρόσβαση στη θύρα 1433 (SQL) από το υποδίκτυο προσκηνίου.

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. Εκτελέστε το **`azure network nsg rule create`** εντολή για να δημιουργήσετε έναν κανόνα που αποτρέπει την πρόσβαση στο Internet.

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    Αναμενόμενη putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Εκτελέστε το **`azure network nsg subnet add`** εντολή για να συνδέσετε το NSG στο υποδίκτυο παρασκηνίου.

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK
