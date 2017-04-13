<properties
   pageTitle="Δημιουργία Internet αντικριστές εξισορρόπηση φόρτου στη Διαχείριση πόρων χρησιμοποιώντας το Azure CLI | Microsoft Azure"
   description="Μάθετε πώς να δημιουργείτε Internet αντικριστές εξισορρόπηση φόρτου στη Διαχείριση πόρων χρησιμοποιώντας το CLI Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="creating-an-internal-load-balancer-using-the-azure-cli"></a>Δημιουργία μιας εσωτερικής εξισορρόπηση φόρτου χρησιμοποιώντας το CLI Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε επίσης να [Μάθετε πώς να δημιουργείτε Internet αντικριστές εξισορρόπηση φόρτου χρησιμοποιώντας κλασική ανάπτυξης](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Ανάπτυξη της λύσης χρησιμοποιώντας το CLI Azure

Τα ακόλουθα βήματα δείχνουν πώς μπορείτε να δημιουργήσετε ένα Internet αντικριστές εξισορρόπηση φόρτου χρήση της διαχείρισης πόρων Azure με CLI. Με τη διαχείριση πόρων Azure κάθε πόρο έχει δημιουργηθεί και έχει ρυθμιστεί ξεχωριστά, στη συνέχεια, τοποθέτηση για να δημιουργήσετε έναν πόρο.

Πρέπει να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους των παρακάτω αντικειμένων για να αναπτύξετε μια μονάδα εξισορρόπησης φόρτου:

- Ρύθμιση παραμέτρων προσκηνίου IP - περιέχει δημόσιες διευθύνσεις IP για την εισερχόμενη κίνηση δικτύου.
- Χώρος συγκέντρωσης παρασκηνίου διεύθυνση - περιέχει διασυνδέσεις δικτύου (NIC) για τις εικονικές μηχανές για να λαμβάνετε κίνηση του δικτύου από τη μονάδα εξισορρόπησης φόρτου.
- Εξισορρόπηση φόρτου κανόνες - περιέχει κανόνες αντιστοίχισης μια δημόσια θύρα τη μονάδα εξισορρόπησης φόρτου στη θύρα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
- NAT κανόνες εισερχομένων - περιέχει κανόνες αντιστοίχιση μια δημόσια θύρα τη μονάδα εξισορρόπησης φόρτου σε μια θύρα για ένα συγκεκριμένο εικονικό μηχάνημα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
- Probes - περιέχει καθετήρες εύρυθμης λειτουργίας που χρησιμοποιείται για να ελέγξετε τη διαθεσιμότητα των παρουσιών εικονικές μηχανές στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.

Για περισσότερες πληροφορίες ανατρέξτε στο θέμα [Διαχείριση πόρων Azure υποστήριξης για εξισορρόπηση φόρτου](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Ρύθμιση CLI για χρήση από διαχειριστή πόρων

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../../articles/xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.

2. Εκτελέστε την εντολή **λειτουργία azure ρύθμισης παραμέτρων** για να μεταβείτε σε λειτουργία διαχείρισης πόρων, όπως φαίνεται παρακάτω.

        azure config mode arm

    Αναμενόμενο αποτέλεσμα:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Δημιουργήστε ένα εικονικό δίκτυο και μια δημόσια διεύθυνση IP για το χώρο συγκέντρωσης προσκηνίου IP

1. Δημιουργήστε ένα εικονικό δίκτυο (VNet) με το όνομα *NRPVnet* στη θέση Ανατολικής ΗΠΑ χρησιμοποιώντας μια ομάδα πόρων με το όνομα *NRPRG*.

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    Δημιουργήστε ένα δευτερεύον δίκτυο με το όνομα *NRPVnetSubnet* με ένα μπλοκ CIDR 10.0.0.0/24 στο *NRPVnet*.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Δημιουργήστε μια δημόσια διεύθυνση IP με το όνομα που θα χρησιμοποιηθεί από ένα σύνολο προσκηνίου IP με το όνομα DNS *loadbalancernrp.eastus.cloudapp.azure.com* *NRPPublicIP* . Η παρακάτω εντολή χρησιμοποιεί τον τύπο στατική εκχώρηση και το χρονικό όριο αδράνειας 4 λεπτά.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT]Η μονάδα εξισορρόπησης φόρτου θα χρησιμοποιήσει την ετικέτα τομέα από τη δημόσια IP ως το FQDN. Αυτό μια αλλαγή από κλασική ανάπτυξη, που χρησιμοποιεί το cloud υπηρεσίας ως τη μονάδα εξισορρόπησης φόρτου πλήρως προσδιορισμένο όνομα τομέα (FQDN).
    >Σε αυτό το παράδειγμα, το FQDN είναι *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Δημιουργήστε μια μονάδα εξισορρόπησης φόρτου

Η παρακάτω εντολή δημιουργεί μια μονάδα εξισορρόπησης φόρτου που ονομάζεται *NRPlb* στην ομάδα πόρων *NRPRG* στις *ΗΠΑ Ανατολικής* Azure θέση.

    azure network lb create NRPRG NRPlb eastus

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Δημιουργία χώρου συγκέντρωσης προσκηνίου IP και ένα χώρο συγκέντρωσης διεύθυνση παρασκηνίου

Αυτό το παράδειγμα δείχνει τον τρόπο δημιουργίας του προσκηνίου χώρου συγκέντρωσης διευθύνσεων IP που λαμβάνει την εισερχόμενη κίνηση δικτύου στον την εξισορρόπηση φόρτου και το χώρο συγκέντρωσης IP υπολογιστή στο παρασκήνιο, όπου το χώρο συγκέντρωσης προσκηνίου στέλνει την κίνηση δικτύου εξισορρόπησης φόρτου.

1. Δημιουργία συνόλου προσκηνίου IP Συσχετίζοντας τη δημόσια IP που δημιουργήσατε στο προηγούμενο βήμα και τη μονάδα εξισορρόπησης φόρτου.

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Ρύθμιση του χώρου συγκέντρωσης διεύθυνση παρασκηνίου που χρησιμοποιείται για τη λήψη εισερχόμενα δεδομένα από το χώρο συγκέντρωσης προσκηνίου διευθύνσεων IP.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## <a name="create-lb-rules-nat-rules-and-probe"></a>Δημιουργία κανόνων Λίβρες, κανόνες NAT και δοκιμή του

Αυτό το παράδειγμα δημιουργεί τα ακόλουθα στοιχεία.

- Ένας κανόνας NAT για να μεταφράσετε όλη την εισερχόμενη κυκλοφορία στη θύρα 21 σε θύρα 22<sup>1</sup>
- Ένας κανόνας NAT για να μεταφράσετε όλη την εισερχόμενη κυκλοφορία στη θύρα 23 στη θύρα 22
- Ένας κανόνας εξισορρόπησης φόρτου να εξισορροπήσετε όλη την εισερχόμενη κυκλοφορία στη θύρα 80 στη θύρα 80 στις διευθύνσεις στο χώρο συγκέντρωσης παρασκηνίου.
- δοκιμή του κανόνα για να ελέγξετε την κατάσταση εύρυθμης λειτουργίας σε μια σελίδα με το όνομα *HealthProbe.aspx*.

<sup>1</sup> οι κανόνες NAT είναι συσχετισμένη με μια συγκεκριμένη εικονική μηχανή παρουσία πίσω από τη μονάδα εξισορρόπησης φόρτου. Η κυκλοφορία του δικτύου φτάνει στη θύρα 21 αποστέλλεται μια συγκεκριμένη εικονική μηχανή στη θύρα 22 που σχετίζονται με αυτόν τον κανόνα NAT. Πρέπει να καθορίσετε ένα πρωτόκολλο (UDP ή TCP) για έναν κανόνα NAT. Και τα δύο πρωτόκολλα δεν μπορεί να αντιστοιχιστεί στην ίδια θύρα.

1. Δημιουργήστε τους κανόνες NAT.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22

2. Δημιουργήστε έναν κανόνα εξισορρόπησης φόρτου.

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Δημιουργήστε μια δοκιμή εύρυθμης λειτουργίας του.

        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4

4. Ελέγξτε τις ρυθμίσεις σας.

        azure network lb show nrprg nrplb

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Δημιουργία NIC

Χρειάζεται να δημιουργήσετε NIC (ή να τροποποιήσετε υπάρχουσες) και συνδέετε σε κανόνες NAT, κανόνες εξισορρόπησης φόρτου και καθετήρες.

1. Δημιουργία ενός NIC με το όνομα *είναι nic1 λίβρες*, και να συσχετίσετε με τον κανόνα NAT *rdp1* και το σύνολο των διευθύνσεων παρασκηνίου *NRPbackendpool* .

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Δημιουργία ενός NIC με το όνομα *είναι nic2 λίβρες*, και να συσχετίσετε με τον κανόνα NAT *rdp2* και το σύνολο των διευθύνσεων παρασκηνίου *NRPbackendpool* .

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Δημιουργήστε μια εικονική μηχανή (Εικονική) που ονομάζεται *web1*και συσχετίσετε με το NIC με το όνομα *είναι nic1 λίβρες*. Ένα λογαριασμό χώρου αποθήκευσης που ονομάζεται *web1nrp* δημιουργήθηκε πριν από την εκτέλεση της εντολής παρακάτω.

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] ΣΠΣ σε μια μονάδα εξισορρόπησης φόρτου πρέπει να είναι στο ίδιο σύνολο διαθεσιμότητα. Χρήση `azure availset create` για να δημιουργήσετε μια διαθεσιμότητα σύνολο.

    Το αποτέλεσμα θα πρέπει να είναι παρόμοια με τα εξής:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] Αναμένεται το ενημερωτικό μήνυμα **αυτή είναι μια NIC χωρίς publicIP έχει ρυθμιστεί** από το NIC που έχει δημιουργηθεί για τη μονάδα εξισορρόπησης φόρτου τη σύνδεση στο Internet, χρησιμοποιώντας τη φόρτωση εξισορρόπησης δημόσια διεύθυνση IP.

    Επειδή το *να nic1 λίβρες* NIC σχετίζεται με τον κανόνα NAT *rdp1* , μπορείτε να συνδεθείτε χρησιμοποιώντας RDP μέσω θύρας 3441 επί τη μονάδα εξισορρόπησης φόρτου *web1* .

4. Δημιουργήστε μια εικονική μηχανή (Εικονική) που ονομάζεται *web2*και συσχετίσετε με το NIC με το όνομα *είναι nic2 λίβρες*. Ένα λογαριασμό χώρου αποθήκευσης που ονομάζεται *web1nrp* δημιουργήθηκε πριν από την εκτέλεση της εντολής παρακάτω.

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="update-an-existing-load-balancer"></a>Ενημέρωση μιας υπάρχουσας εξισορρόπηση φόρτου

Μπορείτε να προσθέσετε κανόνες αναφορά σε μια υπάρχουσα εξισορρόπηση φόρτου. Στο επόμενο παράδειγμα, έναν νέο κανόνα εξισορρόπησης φόρτου προστίθεται σε μια υπάρχουσα εξισορρόπηση φόρτου **NRPlb**

    azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool

## <a name="delete-a-load-balancer"></a>Διαγράψτε μια μονάδα εξισορρόπησης φόρτου

Χρησιμοποιήστε την παρακάτω εντολή για να καταργήσετε μια μονάδα εξισορρόπησης φόρτου:

    azure network lb delete --resource-group nrprg --name nrplb

## <a name="next-steps"></a>Επόμενα βήματα

[Γρήγορα αποτελέσματα με τη ρύθμιση των παραμέτρων μιας εσωτερικής εξισορρόπηση φόρτου](load-balancer-get-started-ilb-arm-cli.md)

[Ρύθμιση παραμέτρων μιας λειτουργίας διανομής εξισορρόπησης φόρτου](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)
