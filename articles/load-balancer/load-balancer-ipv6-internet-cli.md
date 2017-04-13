<properties
    pageTitle="Δημιουργία Internet αντικριστές εξισορρόπηση φόρτου με IPv6 στο Azure διαχείριση πόρων χρησιμοποιώντας το Azure CLI | Microsoft Azure"
    description="Μάθετε πώς να δημιουργείτε Internet αντικριστές εξισορρόπηση φόρτου με IPv6 στο Azure διαχείριση πόρων χρησιμοποιώντας το CLI Azure"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="το IPv6, azure εξισορρόπηση φόρτου, διπλή στοίβα, δημόσια ip, εγγενούς ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a>Δημιουργία Internet αντικριστές εξισορρόπηση φόρτου με IPv6 στο Azure διαχείριση πόρων χρησιμοποιώντας το CLI Azure

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Πρότυπο](./load-balancer-ipv6-internet-template.md)

Μια μονάδα εξισορρόπησης φόρτου Azure είναι μια μονάδα εξισορρόπησης φόρτου επιπέδου-4 (TCP, UDP). Η μονάδα εξισορρόπησης φόρτου παρέχει υψηλή διαθεσιμότητα από τη διανομή εισερχόμενη κίνηση μεταξύ των παρουσιών σε καλή κατάσταση υπηρεσίας στις υπηρεσίες cloud ή εικονικές μηχανές σε ένα σύνολο εξισορρόπησης φόρτου. Azure εξισορρόπηση φόρτου επίσης να εμφανίσετε αυτές τις υπηρεσίες σε πολλαπλές θύρες, πολλές διευθύνσεις IP ή και τα δύο.

## <a name="example-deployment-scenario"></a>Παράδειγμα ανάπτυξης σεναρίου

Το παρακάτω διάγραμμα παρουσιάζει το λύση εξισορρόπησης φόρτου αναπτύσσεται με χρήση του προτύπου παράδειγμα που περιγράφονται σε αυτό το άρθρο.

![Σενάριο εξισορρόπησης φόρτου](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

Σε αυτό το σενάριο θα δημιουργήσετε στους παρακάτω πόρους Azure:

- δύο εικονικές μηχανές (ΣΠΣ)
- περιβάλλον εικονικού δικτύου για κάθε Εικονική με διευθύνσεις IPv4 και IPv6 στους οποίους έχουν ανατεθεί
- μια μονάδα εξισορρόπησης φόρτου μέσω Internet με ένα IPv4 και μια IPv6 δημόσια διεύθυνση IP
- μια ρύθμιση διαθεσιμότητα που περιέχει τα δύο ΣΠΣ
- δύο φόρτωση εξισορρόπησης κανόνες για να αντιστοιχίσετε τη δημόσια VIPs για τα τελικά σημεία ιδιωτική

## <a name="deploying-the-solution-using-the-azure-cli"></a>Ανάπτυξη της λύσης χρησιμοποιώντας το CLI Azure

Τα ακόλουθα βήματα δείχνουν πώς μπορείτε να δημιουργήσετε ένα Internet αντικριστές εξισορρόπηση φόρτου χρήση της διαχείρισης πόρων Azure με CLI. Με το Azure διαχείριση πόρων, κάθε πόρο δημιουργείται και έχει ρυθμιστεί ξεχωριστά, και στη συνέχεια, τοποθετήστε για να δημιουργήσετε έναν πόρο.

Για να αναπτύξετε μια μονάδα εξισορρόπησης φόρτου, δημιουργία και ρύθμιση παραμέτρων τα ακόλουθα αντικείμενα:

- Ρύθμιση παραμέτρων προσκηνίου IP - περιέχει δημόσιες διευθύνσεις IP για την εισερχόμενη κίνηση δικτύου.
- Χώρος συγκέντρωσης παρασκηνίου διεύθυνση - περιέχει διασυνδέσεις δικτύου (NIC) για τις εικονικές μηχανές για να λαμβάνετε κίνηση του δικτύου από τη μονάδα εξισορρόπησης φόρτου.
- Εξισορρόπηση φόρτου κανόνες - περιέχει κανόνες αντιστοίχισης μια δημόσια θύρα τη μονάδα εξισορρόπησης φόρτου στη θύρα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
- NAT κανόνες εισερχομένων - περιέχει κανόνες αντιστοίχιση μια δημόσια θύρα τη μονάδα εξισορρόπησης φόρτου σε μια θύρα για ένα συγκεκριμένο εικονικό μηχάνημα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
- Probes - περιέχει καθετήρες εύρυθμης λειτουργίας που χρησιμοποιείται για να ελέγξετε τη διαθεσιμότητα των παρουσιών εικονικές μηχανές στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση πόρων Azure υποστήριξης για εξισορρόπηση φόρτου](load-balancer-arm.md).

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a>Ρύθμιση του περιβάλλοντος του CLI για χρήση από διαχειριστή πόρων Azure

Για αυτό το παράδειγμα, θα σας εκτελούνται τα εργαλεία CLI σε ένα παράθυρο εντολών του PowerShell. Δεν χρησιμοποιούμε τα cmdlet του Azure PowerShell αλλά χρησιμοποιούμε δυνατοτήτων δέσμης ενεργειών του PowerShell για να βελτιώσετε την αναγνωσιμότητα και εκ νέου χρήση.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../../articles/xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.

2. Εκτελέστε την εντολή **λειτουργία azure ρύθμισης παραμέτρων** για να μεταβείτε σε λειτουργία διαχείρισης πόρων.

        azure config mode arm

    Αναμενόμενο αποτέλεσμα:

        info:    New mode is arm

3. Πραγματοποιήστε είσοδο στο Azure και να λάβετε μια λίστα των συνδρομών.

        azure login

    Εισαγάγετε Azure τα διαπιστευτήριά σας όταν σας ζητηθεί.

        azure account list

    Επιλέξτε τη συνδρομή που θέλετε να χρησιμοποιήσετε. Σημειώστε το αναγνωριστικό εγγραφής για το επόμενο βήμα.

4. Ρύθμιση του PowerShell μεταβλητές για χρήση με τις εντολές CLI.

        ```
        $subscriptionid = "########-####-####-####-############"  # enter subscription id
        $location = "southcentralus"
        $rgName = "pscontosorg1southctrlus09152016"
        $vnetName = "contosoIPv4Vnet"
        $vnetPrefix = "10.0.0.0/16"
        $subnet1Name = "clicontosoIPv4Subnet1"
        $subnet1Prefix = "10.0.0.0/24"
        $subnet2Name = "clicontosoIPv4Subnet2"
        $subnet2Prefix = "10.0.1.0/24"
        $dnsLabel = "contoso09152016"
        $lbName = "myIPv4IPv6Lb"
        ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Δημιουργία μιας ομάδας πόρων, μια μονάδα εξισορρόπησης φόρτου, ένα εικονικό δίκτυο και δευτερεύοντα δίκτυα

1. Δημιουργήστε μια ομάδα πόρων

        azure group create $rgName $location

2. Δημιουργήστε μια μονάδα εξισορρόπησης φόρτου

        $lb = azure network lb create --resource-group $rgname --location $location --name $lbName

3. Δημιουργήστε ένα εικονικό δίκτυο (VNet).

        $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix

    Δημιουργήστε δύο δευτερευόντων δικτύων στο αυτό VNet.

        $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
        $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Δημιουργία δημόσιας διευθύνσεις IP για το χώρο συγκέντρωσης προσκηνίου

1. Ρυθμίστε τις μεταβλητές PowerShell

        $publicIpv4Name = "myIPv4Vip"
        $publicIpv6Name = "myIPv6Vip"

2. Δημιουργήστε μια δημόσια διεύθυνση IP του προσκηνίου χώρου συγκέντρωσης διευθύνσεων IP.

        $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
        $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel

    >[AZURE.IMPORTANT]Η μονάδα εξισορρόπησης φόρτου χρησιμοποιεί την ετικέτα τομέα από τη δημόσια IP ως το FQDN. Αυτό μια αλλαγή από κλασική ανάπτυξη, το οποίο χρησιμοποιεί την υπηρεσία cloud όνομα ως τη μονάδα εξισορρόπησης φόρτου FQDN.
    >Σε αυτό το παράδειγμα, το FQDN είναι *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Δημιουργήστε σύνολα προσκηνίου και παρασκηνίου

Αυτό το παράδειγμα δημιουργεί προσκηνίου χώρου συγκέντρωσης IP που λαμβάνει την εισερχόμενη κίνηση δικτύου στον την εξισορρόπηση φόρτου και το χώρο συγκέντρωσης IP παρασκηνίου, όπου το χώρο συγκέντρωσης προσκηνίου στέλνει την κίνηση δικτύου εξισορρόπησης φόρτου.

1. Ρυθμίστε τις μεταβλητές PowerShell

        $frontendV4Name = "FrontendVipIPv4"
        $frontendV6Name = "FrontendVipIPv6"
        $backendAddressPoolV4Name = "BackendPoolIPv4"
        $backendAddressPoolV6Name = "BackendPoolIPv6"

2. Δημιουργία συνόλου προσκηνίου IP Συσχετίζοντας τη δημόσια IP που δημιουργήσατε στο προηγούμενο βήμα και τη μονάδα εξισορρόπησης φόρτου.

        $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
        $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
        $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
        $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName

## <a name="create-the-probe-nat-rules-and-lb-rules"></a>Δημιουργία τη δοκιμή του, οι κανόνες NAT και Λίβρες κανόνων

Αυτό το παράδειγμα δημιουργεί τα ακόλουθα στοιχεία:

- δοκιμή του κανόνα για να ελέγξετε για σύνδεση με τη θύρα TCP 80
- Ένας κανόνας NAT για να μεταφράσετε όλη την εισερχόμενη κυκλοφορία στη θύρα 3389 στη θύρα 3389 για RDP<sup>1</sup>
- Ένας κανόνας NAT για να μεταφράσετε όλη την εισερχόμενη κυκλοφορία στη θύρα 3391 στη θύρα 3389 για RDP<sup>1</sup>
- Ένας κανόνας εξισορρόπησης φόρτου να εξισορροπήσετε όλη την εισερχόμενη κυκλοφορία στη θύρα 80 στη θύρα 80 στις διευθύνσεις στο χώρο συγκέντρωσης παρασκηνίου.

<sup>1</sup> οι κανόνες NAT είναι συσχετισμένη με μια συγκεκριμένη εικονική μηχανή παρουσία πίσω από τη μονάδα εξισορρόπησης φόρτου. Η κυκλοφορία του δικτύου φτάνει στη θύρα 3389 αποστέλλεται το συγκεκριμένο εικονική μηχανή και τη θύρα που σχετίζεται με τον κανόνα NAT. Πρέπει να καθορίσετε ένα πρωτόκολλο (UDP ή TCP) για έναν κανόνα NAT. Και τα δύο πρωτόκολλα δεν μπορεί να αντιστοιχιστεί στην ίδια θύρα.

1. Ρυθμίστε τις μεταβλητές PowerShell

        $probeV4V6Name = "ProbeForIPv4AndIPv6"
        $natRule1V4Name = "NatRule-For-Rdp-VM1"
        $natRule2V4Name = "NatRule-For-Rdp-VM2"
        $lbRule1V4Name = "LBRuleForIPv4-Port80"
        $lbRule1V6Name = "LBRuleForIPv6-Port80"

2. Δημιουργήστε τη διερεύνηση

    Το παρακάτω παράδειγμα δημιουργεί μια TCP δοκιμή του που ελέγχει για συνδεσιμότητα με τη θύρα TCP 80 παρασκηνίου κάθε 15 δευτερόλεπτα. Αυτό θα επισημάνετε τον πόρο παρασκηνίου μη διαθέσιμες μετά από δύο διαδοχικές αποτυχίες.

        $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName

3. Δημιουργήστε κανόνες εισερχόμενων NAT που επιτρέπουν συνδέσεις RDP στους πόρους υποστήριξης

        $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
        $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName

4. Δημιουργήστε μια μονάδα εξισορρόπησης φόρτου κανόνες που στέλνει την κυκλοφορία σε διαφορετικές παρασκηνίου θύρες, ανάλογα με ποια προσκηνίου έλαβε την αίτηση

        $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
        $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName

5. Ελέγξτε τις ρυθμίσεις σας

        azure network lb show --resource-group $rgName --name $lbName

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show


## <a name="create-nics"></a>Δημιουργία NIC

Δημιουργία NIC και να συνδέετε σε κανόνες NAT, κανόνες εξισορρόπησης φόρτου και καθετήρες.

1. Ρυθμίστε τις μεταβλητές PowerShell

        $nic1Name = "myIPv4IPv6Nic1"
        $nic2Name = "myIPv4IPv6Nic2"
        $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
        $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
        $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
        $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
        $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
        $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"

2. Δημιουργήστε ένα NIC για κάθε παρασκηνίου και προσθέστε μια ρύθμιση παραμέτρων IPv6.

        $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

        $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Δημιουργήστε τους πόρους Εικονική παρασκηνίου και επισυνάψτε κάθε NIC

Για να δημιουργήσετε ΣΠΣ, πρέπει να έχετε ένα λογαριασμό του χώρου αποθήκευσης. Για την εξισορρόπηση φόρτου, του ΣΠΣ πρέπει να είναι μέλη ενός συνόλου διαθεσιμότητα. Για περισσότερες πληροφορίες σχετικά με τη δημιουργία ΣΠΣ, ανατρέξτε στο θέμα [Δημιουργία Εικονική μηχανή Azure χρησιμοποιώντας το PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md)

1. Ρυθμίστε τις μεταβλητές PowerShell

        $storageAccountName = "ps08092016v6sa0"
        $availabilitySetName = "myIPv4IPv6AvailabilitySet"
        $vm1Name = "myIPv4IPv6VM1"
        $vm2Name = "myIPv4IPv6VM2"
        $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
        $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
        $disk1Name = "WindowsVMosDisk1"
        $disk2Name = "WindowsVMosDisk2"
        $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
        $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
        $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
        $vmUserName = "vmUser"
        $mySecurePassword = "PlainTextPassword*1"

    >[AZURE.WARNING] Αυτό το παράδειγμα χρησιμοποιεί το όνομα χρήστη και τον κωδικό πρόσβασης για το ΣΠΣ σε απλό κείμενο. Κατάλληλη περίθαλψη πρέπει να λαμβάνονται όταν με χρήση διαπιστευτηρίων στο Απαλοιφή. Για μια πιο ασφαλή μέθοδο χειρισμού διαπιστευτήρια σε PowerShell, ανατρέξτε στο θέμα το cmdlet [Get-διαπιστευτηρίων](https://technet.microsoft.com/library/hh849815.aspx) .

2. Δημιουργία συνόλου λογαριασμού και τη διαθεσιμότητα του χώρου αποθήκευσης

    Μπορείτε να χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης κατά τη δημιουργία του ΣΠΣ. Η παρακάτω εντολή δημιουργεί ένα νέο λογαριασμό του χώρου αποθήκευσης.

        $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"

    Στη συνέχεια, δημιουργήστε το σύνολο διαθεσιμότητα.

        $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location

3. Δημιουργήστε τις εικονικές μηχανές με το συσχετισμένο NIC

        $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

        $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn  --storage-account-name $storageAccountName --disable-bginfo-extension

## <a name="next-steps"></a>Επόμενα βήματα

[Γρήγορα αποτελέσματα με τη ρύθμιση των παραμέτρων μιας εσωτερικής εξισορρόπηση φόρτου](load-balancer-get-started-ilb-arm-cli.md)

[Ρύθμιση παραμέτρων μιας λειτουργίας διανομής εξισορρόπησης φόρτου](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)
