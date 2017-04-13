<properties 
   pageTitle="Πώς μπορείτε να ορίσετε μια στατική ιδιωτικό IP σε κατάσταση λειτουργίας ARM χρησιμοποιώντας το CLI | Microsoft Azure"
   description="Κατανόηση των στατική διευθύνσεις IP (πτώσεις) και πώς μπορείτε να διαχειριστείτε τους σε κατάσταση λειτουργίας ARM χρησιμοποιώντας το CLI"
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

# <a name="how-to-set-a-static-private-ip-address-in-azure-cli"></a>Πώς μπορείτε να ορίσετε μια στατική διεύθυνση IP ιδιωτικό στο Azure CLI

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε επίσης να [διαχειριστείτε στατική διεύθυνση IP ιδιωτικό στο μοντέλο κλασική ανάπτυξης](virtual-networks-static-private-ip-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Οι εντολές Azure CLI δείγμα παρακάτω θεωρείτε ότι ένα απλό περιβάλλον που έχετε ήδη δημιουργήσει. Εάν θέλετε να εκτελέσετε τις εντολές που εμφανίζονται σε αυτό το έγγραφο, δημιουργήστε πρώτα το περιβάλλον δοκιμής περιγράφεται στη [Δημιουργία ενός vnet](virtual-networks-create-vnet-arm-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Πώς μπορείτε να καθορίσετε μια στατική διεύθυνση IP ιδιωτικά, κατά τη δημιουργία μια εικονική Μηχανή
Για να δημιουργήσετε μια Εικονική με το όνομα *DNS01* στο δευτερεύον *FrontEnd* από ένα VNet με το όνομα *TestVNet* με στατική ιδιωτικό IP του *192.168.1.101*, ακολουθήστε τα παρακάτω βήματα:

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.

2. Εκτελέστε την εντολή **λειτουργία azure ρύθμισης παραμέτρων** για να μεταβείτε σε λειτουργία διαχείρισης πόρων, όπως φαίνεται παρακάτω.

        azure config mode arm

    Αναμενόμενο αποτέλεσμα:

        info:    New mode is arm

3. Εκτελέστε το **azure δικτύου δημόσιας-ip δημιουργία** για να δημιουργήσετε μια δημόσια IP για την εικονική Μηχανή. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται.

        azure network public-ip create -g TestRG -n TestPIP -l centralus
    
    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network public-ip create
        + Looking up the public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up the public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK

    - **-g (ή--ομάδα πόρων)**. Το όνομα της ομάδας πόρων τη δημόσια IP θα δημιουργηθεί στο.
    - **-n (ή--όνομα)**. Όνομα από τη δημόσια διευθύνσεων IP.
    - **-l (ή--θέση)**. Azure περιοχής όπου θα δημιουργηθεί η δημόσια διευθύνσεων IP. Για το σενάριο, *centralus*.

3. Εκτελέστε την εντολή **Δημιουργία azure δικτύου nic** για να δημιουργήσετε ένα NIC με στατική IP ιδιωτικό. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται.

        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd

    Αναμενόμενο αποτέλεσμα:

        + Looking up the network interface "TestNIC"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up the network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

    - **-(ή--ιδιωτική διεύθυνση ip)**. Στατική ιδιωτική διεύθυνση IP για το NIC.
    - **-m (ή--υποδικτύου-vnet-name)**. Όνομα από το σημείο όπου θα δημιουργηθεί το NIC VNet.
    - **-k (ή--υποδικτύου όνομα)**. Το όνομα του υποδικτύου όπου θα δημιουργηθεί το NIC.

4. Εκτελέστε την εντολή **Δημιουργία εικονική azure** για να δημιουργήσετε την εικονική Μηχανή χρησιμοποιώντας η δημόσια διεύθυνση IP και NIC δημιουργήθηκε παραπάνω. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται.

        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command vm create
        + Looking up the VM "DNS01"
        info:    Using the VM Size "Standard_A1"
        warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account vnetstorage
        + Looking up the NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
        info:    Found public ip parameters, trying to setup PublicIP profile
        + Looking up the public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up the NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK

    - **-y (ή τύπου--os)**. Τύπος του λειτουργικού συστήματος για το Εικονική *Windows* ή *Linux*.
    - **-f (ή όνομα--nic)**. Το όνομα της σύνδεσης NIC, θα χρησιμοποιήσει την εικονική Μηχανή.
    - **-i (ή--όνομα δημόσια-ip)**. Όνομα από τη δημόσια IP θα χρησιμοποιήσει την εικονική Μηχανή.
    - **-F (ή--vnet-όνομα)**. Όνομα το VNet όπου θα δημιουργηθεί η Εικονική.
    - **-j (ή--vnet-υποδικτύου-name)**. Το όνομα του υποδικτύου όπου θα δημιουργηθεί η Εικονική.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Πώς μπορείτε να ανακτήσετε ιδιωτικό πληροφορίες στατικής διεύθυνσης IP για μια Εικονική

Για να προβάλετε τα ιδιωτικά πληροφορίες στατικής διεύθυνσης IP για την εικονική Μηχανή που δημιουργήθηκαν με τη δέσμη ενεργειών παραπάνω, εκτελέστε την ακόλουθη εντολή Azure CLI και παρατηρήστε τις τιμές για το *IP ιδιωτική Επιμ-μέθοδο* και *τη διεύθυνση IP ιδιωτικό*:

    azure vm show -g TestRG -n DNS01

Αναμενόμενο αποτέλεσμα:

    info:    Executing command vm show
    + Looking up the VM "DNS01"
    + Looking up the NIC "TestNIC"
    + Looking up the public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Πώς να καταργήσετε μια στατική διεύθυνση IP ιδιωτικό από μια εικονική Μηχανή
Δεν μπορείτε να καταργήσετε μια στατική διεύθυνση IP ιδιωτικό από NIC στο Azure CLI για διαχείριση πόρων. Που πρέπει να δημιουργήσετε μια νέα NIC που χρησιμοποιεί μια δυναμική IP κατάργηση προηγούμενης NIC από την εικονική Μηχανή και στη συνέχεια να προσθέσετε το νέο NIC η Εικονική. Για να αλλάξετε το NIC για την Εικονική χρησιμοποιούνται int eh εντολές παραπάνω, ακολουθήστε τα παρακάτω βήματα.
    
1. Εκτελέστε την εντολή **Δημιουργία azure δικτύου nic** για να δημιουργήσετε μια νέα NIC χρησιμοποιώντας δυναμική εκχώρηση διευθύνσεων IP. Παρατηρήστε πώς δεν χρειάζεται να καθορίσετε τη διεύθυνση IP αυτήν τη στιγμή.

        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nic create
        + Looking up the network interface "TestNIC2"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up the network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

2. Εκτελέστε την εντολή **Ορισμός azure εικονική** για να αλλάξετε το NIC που χρησιμοποιούνται από την εικονική Μηχανή.

        azure vm set -g TestRG -n DNS01 -N TestNIC2

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command vm set
        + Looking up the VM "DNS01"
        + Looking up the NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK

3. Εάν θέλατε, εκτελέστε την εντολή **Διαγραφή azure δικτύου nic** για να διαγράψετε το παλιό NIC.

        azure network nic delete -g TestRG -n TestNIC --quiet

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nic delete
        + Looking up the network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Πώς να προσθέσετε μια στατική διεύθυνση IP ιδιωτικό σε υπάρχουσα Εικονική μηχανή
Για να προσθέσετε μια στατική διεύθυνση IP ιδιωτικό NIC που χρησιμοποιούνται από την εικονική Μηχανή που δημιουργήθηκε με τη δέσμη ενεργειών παραπάνω, εκτελέστε την ακόλουθη εντολή:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Αναμενόμενο αποτέλεσμα:

    info:    Executing command network nic set
    + Looking up the network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up the network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε σχετικά με τις διευθύνσεις [δεσμευμένη δημόσια διεύθυνση IP](virtual-networks-reserved-public-ip.md) .
- Μάθετε σχετικά με τις διευθύνσεις [επιπέδου παρουσίας δημόσια IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Συμβουλευθείτε τα [APIs ΥΠΌΛΟΙΠΑ δεσμευμένη διεύθυνση IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).
