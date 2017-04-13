<properties
   pageTitle="Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου, χρησιμοποιώντας το Azure CLI στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε μια εσωτερική εξισορρόπηση φόρτου, χρησιμοποιώντας το Azure CLI στη Διαχείριση πόρων"
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

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου, χρησιμοποιώντας το CLI Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][μοντέλο κλασική ανάπτυξης](load-balancer-get-started-ilb-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Ανάπτυξη της λύσης, χρησιμοποιώντας το CLI Azure

Τα ακόλουθα βήματα δείχνουν πώς μπορείτε να δημιουργήσετε μια μονάδα εξισορρόπησης φόρτου μέσω Internet, χρησιμοποιώντας τη διαχείριση πόρων Azure με CLI. Με το Azure διαχείριση πόρων, κάθε πόρο δημιουργείται και έχει ρυθμιστεί ξεχωριστά, και στη συνέχεια, τοποθετήστε για να δημιουργήσετε έναν πόρο.

Πρέπει να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους για να αναπτύξετε μια μονάδα εξισορρόπησης φόρτου τα ακόλουθα αντικείμενα:

- **Ρύθμιση παραμέτρων IP περιβάλλοντος χρήστη**: περιέχει δημόσιες διευθύνσεις IP για την εισερχόμενη κίνηση δικτύου
- **Χώρος συγκέντρωσης διεύθυνση παρασκηνίου**: περιέχει διασυνδέσεις δικτύου (NIC) που επιτρέπουν την εικονικές μηχανές λήψης κίνηση του δικτύου από τη μονάδα εξισορρόπησης φόρτου
- **Εξισορρόπηση φόρτου κανόνες**: περιέχει κανόνες που αντιστοίχισης μιας δημόσιας θύρας στην τη μονάδα εξισορρόπησης φόρτου στη θύρα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου
- **Κανόνες εισερχομένων NAT**: περιέχει κανόνες που να αντιστοιχούν μια δημόσια θύρα τη μονάδα εξισορρόπησης φόρτου σε μια θύρα για ένα συγκεκριμένο εικονικό μηχάνημα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου
- **Probes**: περιέχει καθετήρες εύρυθμης λειτουργίας που χρησιμοποιούνται για να ελέγξετε τη διαθεσιμότητα των παρουσιών εικονικές μηχανές στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση πόρων Azure υποστήριξης για εξισορρόπηση φόρτου](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Ρύθμιση CLI για χρήση από διαχειριστή πόρων

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../../articles/xplat-cli-install.md). Ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.

2. Εκτελέστε την εντολή **λειτουργία azure ρύθμισης παραμέτρων** για να μεταβείτε σε λειτουργία διαχείρισης πόρων, ως εξής:

        azure config mode arm

    Αναμενόμενο αποτέλεσμα:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου βήμα προς βήμα

1. Πραγματοποιήστε είσοδο στο Azure.

        azure login

    Όταν σας ζητηθεί, εισαγάγετε τα διαπιστευτήριά σας Azure.

2. Αλλάξτε τα εργαλεία εντολή σε λειτουργία Azure διαχείριση πόρων.

        azure config mode arm

## <a name="create-a-resource-group"></a>Δημιουργήστε μια ομάδα πόρων

Όλοι οι πόροι στη Διαχείριση πόρων Azure συσχετίζονται με μια ομάδα πόρων. Εάν δεν το έχετε κάνει ακόμα, δημιουργήστε μια ομάδα πόρων.

    azure group create <resource group name> <location>

## <a name="create-an-internal-load-balancer-set"></a>Δημιουργήστε ένα σύνολο εξισορρόπησης εσωτερικό φόρτωσης

1. Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου

    Στο παρακάτω σενάριο, μια ομάδα πόρων με το όνομα nrprg δημιουργείται στο περιοχή Ανατολικής ΗΠΑ.

        azure network lb create --name nrprg --location eastus

    >[AZURE.NOTE] Όλοι οι πόροι για μια εσωτερική φόρτωσης balancers, όπως εικονικών δικτύων και δευτερεύοντα δίκτυα εικονικού δικτύου, πρέπει να είναι στην ίδια ομάδα πόρων και στην ίδια περιοχή.

2. Δημιουργήστε μια προσκηνίου διεύθυνση IP για το εσωτερικό εξισορρόπηση φόρτου.

    Η διεύθυνση IP που χρησιμοποιείτε πρέπει να είναι μέσα στην περιοχή υποδικτύου του εικονικού δικτύου σας.

        azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet

3. Δημιουργήστε το χώρο συγκέντρωσης διευθύνσεων παρασκηνίου.

        azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb

    Αφού ορίσετε μια προσκηνίου διεύθυνση IP και ένα χώρο συγκέντρωσης διεύθυνση παρασκηνίου, μπορείτε να δημιουργήσετε κανόνες εξισορρόπησης φόρτου, κανόνες εισερχόμενων NAT, και probes προσαρμοσμένων εύρυθμης λειτουργίας.

4. Δημιουργήστε έναν κανόνα εξισορρόπησης φόρτου για το εσωτερικό εξισορρόπηση φόρτου.

    Όταν ακολουθείτε τα προηγούμενα βήματα, η εντολή δημιουργεί έναν κανόνα μονάδα εξισορρόπησης φόρτου για ακρόαση στη θύρα 1433 στο χώρο συγκέντρωσης προσκηνίου και αποστολή εξισορρόπηση φόρτου κίνηση του δικτύου για το χώρο συγκέντρωσης διευθύνσεων παρασκηνίου, χρησιμοποιεί επίσης θύρα 1433.

        azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb

5. Δημιουργήστε κανόνες εισερχομένων NAT.

    Κανόνες εισερχόμενων NAT που χρησιμοποιούνται για να δημιουργήσετε τελικά σημεία σε μια μονάδα εξισορρόπησης φόρτου που οδηγούν σε μια συγκεκριμένη εικονική μηχανή παρουσία. Τα προηγούμενα βήματα δημιουργηθεί δύο κανόνες NAT απομακρυσμένης επιφάνειας εργασίας.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389

6. Δημιουργία καθετήρες εύρυθμης λειτουργίας για τη μονάδα εξισορρόπησης φόρτου.

    Μια δοκιμή εύρυθμης λειτουργίας του ελέγχει όλες τις εμφανίσεις εικονική μηχανή για να βεβαιωθείτε ότι μπορούν να στείλουν κίνηση του δικτύου. Η παρουσία εικονική μηχανή με έλεγχοι αποτυχίας δοκιμή του καταργείται από τη μονάδα εξισορρόπησης φόρτου μέχρι να μεταβαίνετε πάλι σε σύνδεση και ένα σημάδι ελέγχου δοκιμή του Καθορίζει ότι είναι σε καλή κατάσταση.

        azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4

    >[AZURE.NOTE]Της πλατφόρμας Windows Azure χρησιμοποιεί μια στατική, δυνατότητα δρομολόγησης δημόσια διεύθυνση IPv4 για μια ποικιλία διαχείρισης σενάρια. Η διεύθυνση IP είναι 168.63.129.16. Αυτή η διεύθυνση IP δεν πρέπει να αποκλειστεί από οποιαδήποτε τείχη προστασίας, επειδή αυτό μπορεί να προκαλέσει μη αναμενόμενη συμπεριφορά.
    >Σε σχέση με εξισορρόπησης Azure εσωτερικό φόρτου, χρησιμοποιείται αυτήν τη διεύθυνση IP, παρακολουθώντας καθετήρες από τη μονάδα εξισορρόπησης φόρτου για να προσδιορίσετε την κατάσταση εύρυθμης λειτουργίας για εικονικές μηχανές σε ένα σύνολο εξισορρόπηση φόρτου. Εάν μια ομάδα ασφαλείας δικτύου χρησιμοποιείται για να περιορίσετε την κίνηση Azure εικονικές μηχανές σε ένα σύνολο εσωτερικά εξισορρόπηση φόρτου ή έχει εφαρμοστεί σε ένα υποδίκτυο εικονικού δικτύου, βεβαιωθείτε ότι έχει προστεθεί ένας κανόνας ασφαλείας δικτύου για να επιτρέψετε την κυκλοφορία από 168.63.129.16.

## <a name="create-nics"></a>Δημιουργία NIC

Χρειάζεται να δημιουργήσετε NIC (ή να τροποποιήσετε υπάρχουσες) και συνδέετε σε κανόνες NAT, κανόνες εξισορρόπησης φόρτου και καθετήρες.

1. Δημιουργία μιας NIC με το όνομα *είναι nic1 λίβρες*, και, στη συνέχεια, να τη συσχετίσετε με τον κανόνα NAT *rdp1* και το σύνολο των διευθύνσεων παρασκηνίου *beilb* .

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus

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

2. Δημιουργία μιας NIC με το όνομα *είναι nic2 λίβρες*, και, στη συνέχεια, να τη συσχετίσετε με τον κανόνα NAT *rdp2* και το σύνολο των διευθύνσεων παρασκηνίου *beilb* .

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus

3. Δημιουργήστε μια εικονική μηχανή *Φθίνουσα 1*με το όνομα και, στη συνέχεια, να τη συσχετίσετε με το NIC με το όνομα *είναι nic1 λίβρες*. Πριν από την παρακάτω εντολή Εκτέλεση, δημιουργείται ένας λογαριασμός χώρου αποθήκευσης που ονομάζεται *web1nrp* :

        azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] ΣΠΣ σε μια μονάδα εξισορρόπησης φόρτου πρέπει να είναι στο ίδιο σύνολο διαθεσιμότητα. Χρήση `azure availset create` για να δημιουργήσετε μια διαθεσιμότητα σύνολο.

4. Δημιουργήστε μια εικονική μηχανή (Εικονική) που ονομάζεται *DB2*και, στη συνέχεια, να τη συσχετίσετε με το NIC με το όνομα *είναι nic2 λίβρες*. Ένα λογαριασμό χώρου αποθήκευσης που ονομάζεται *web1nrp* δημιουργήθηκε πριν από την εκτέλεση την ακόλουθη εντολή.

        azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="delete-a-load-balancer"></a>Διαγράψτε μια μονάδα εξισορρόπησης φόρτου

Για να καταργήσετε μια μονάδα εξισορρόπησης φόρτου, χρησιμοποιήστε την ακόλουθη εντολή:

    azure network lb delete --resource-group nrprg --name ilbset

## <a name="next-steps"></a>Επόμενα βήματα

[Ρύθμιση παραμέτρων μια λειτουργία διανομής εξισορρόπησης φόρτου με χρήση συσχέτισης IP προέλευσης](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)
