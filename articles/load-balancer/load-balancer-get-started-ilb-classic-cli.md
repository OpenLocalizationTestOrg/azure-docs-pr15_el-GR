<properties
   pageTitle="Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου χρησιμοποιώντας το Azure CLI στο μοντέλο κλασική ανάπτυξης | Microsoft Azure"
   description="Μάθετε πώς να δημιουργείτε μια εσωτερική εξισορρόπηση φόρτου χρησιμοποιώντας το Azure CLI στο μοντέλο κλασική ανάπτυξης"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Να ξεκινήσετε τη δημιουργία μιας εσωτερικής μονάδα εξισορρόπησης φόρτου (κλασικό) χρησιμοποιώντας το CLI Azure

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας το μοντέλο από διαχειριστή πόρων](load-balancer-get-started-ilb-arm-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Για να δημιουργήσετε μια εσωτερική εξισορρόπηση φόρτου για εικονικές μηχανές

Για να δημιουργήσετε ένα σύνολο εξισορρόπησης εσωτερικό φόρτωση και τους διακομιστές που θα σας στέλνει την κυκλοφορία τους σε αυτό, πρέπει να κάνετε τα εξής:

1. Δημιουργήστε μια παρουσία του εσωτερικού εξισορρόπηση φόρτου που θα είναι το τελικό σημείο εισερχόμενης κυκλοφορίας για να γίνει με εξισορρόπηση σε τους διακομιστές ενός συνόλου εξισορρόπηση φόρτου.

1. Προσθήκη τελικά σημεία αντιστοιχεί τις εικονικές μηχανές που θα να λαμβάνουν τα εισερχόμενα δεδομένα.

1. Ρύθμιση παραμέτρων τους διακομιστές που θα στέλνετε η κίνηση για να γίνει με εξισορρόπηση για να στείλετε την κυκλοφορία τους για να την εικονική διεύθυνση IP (VIP) της παρουσίας του εσωτερικού εξισορρόπηση φόρτου.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Βήμα προς βήμα τη δημιουργία μιας εσωτερικής εξισορρόπηση φόρτου χρησιμοποιώντας CLI

Αυτός ο οδηγός δείχνει πώς μπορείτε να δημιουργήσετε μια εσωτερική εξισορρόπηση φόρτου με βάση το παραπάνω σενάριο.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../../articles/xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.

2. Εκτελέστε την εντολή **λειτουργία azure ρύθμισης παραμέτρων** για να μεταβείτε σε κλασική λειτουργία, όπως φαίνεται παρακάτω.

        azure config mode asm

    Αναμενόμενο αποτέλεσμα:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Δημιουργία τελικού σημείου και φόρτωση εξισορρόπησης σύνολο

Το σενάριο προϋποθέτει εικονικές μηχανές "Φθίνουσα 1" και "DB2" σε μια υπηρεσία cloud που ονομάζεται "mytestcloud". Και οι δύο εικονικές μηχανές χρησιμοποιούν ένα εικονικό δίκτυο που ονομάζεται μου "testvnet" με υποδικτύου "υποδικτύου-1".

Αυτός ο οδηγός θα δημιουργήσει ένα σύνολο εξισορρόπησης εσωτερικό φόρτωσης χρησιμοποιώντας θύρα 1433 ως ιδιωτικό θύρας και 1433 ως τοπική θύρα.

Αυτό είναι ένα συνηθισμένο σενάριο όπου έχετε SQL εικονικές μηχανές σε παρασκηνίου χρησιμοποιώντας μια εσωτερική εξισορρόπηση φόρτου για να εξασφαλίσετε τους διακομιστές βάσης δεδομένων δεν είναι δυνατή η έκθεση απευθείας με μια δημόσια διεύθυνση IP.


### <a name="step-1"></a>Βήμα 1

Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου συνόλου με χρήση `azure network service internal-load-balancer add`.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

Παράμετροι χρησιμοποιούνται:

**-r** - όνομα υπηρεσίας cloud<BR>
**-n** - όνομα εξισορρόπησης εσωτερικό φόρτωσης<BR>
**-t** - υποδικτύου όνομα (ίδιο υποδίκτυο από τις εικονικές μηχανές θα προσθέσετε για το εσωτερικό εξισορρόπηση φόρτου)<BR>
**-a** - (προαιρετικά) προσθέστε μια στατική διεύθυνση IP ιδιωτική<BR>

Ανάληψη ελέγχου `azure service internal-load-balancer --help` για περισσότερες πληροφορίες.

Μπορείτε να ελέγξετε τις ιδιότητες εξισορρόπησης εσωτερικό φόρτωσης χρησιμοποιώντας την εντολή `azure service internal-load-balancer list` *όνομα υπηρεσίας cloud*.

Δείτε εδώ ακολουθεί ένα παράδειγμα του αποτελέσματος:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>Βήμα 2

Μπορείτε να ρυθμίσετε το σύνολο εξισορρόπησης εσωτερικό φόρτωσης όταν προσθέτετε το πρώτο τελικό σημείο. Θα μπορείτε να συσχετίσετε το τελικό σημείο, εικονική μηχανή και δοκιμή του θύρα στο σύνολο εξισορρόπησης εσωτερικό φόρτωση σε αυτό το βήμα.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

Παράμετροι χρησιμοποιούνται:

**-k** - τοπικό υπολογιστή εικονική θύρα<BR>
**-t** - δοκιμή του θύρα<BR>
**-r** - δοκιμή του πρωτοκόλλου<BR>
**-e** - δοκιμή του χρονικού διαστήματος σε δευτερόλεπτα<BR>
**-f** - διάστημα χρονικού ορίου σε δευτερόλεπτα <BR>
**-i** - εσωτερικό φόρτωσης εξισορρόπησης όνομα <BR>


## <a name="step-3"></a>Βήμα 3

Επαλήθευση του χρησιμοποιεί ρύθμισης παραμέτρων εξισορρόπησης φόρτου `azure vm show` *όνομα εικονική μηχανή*

    azure vm show DB1

Το αποτέλεσμα θα είναι:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Δημιουργήστε έναν απομακρυσμένο υπολογιστή τελικό σημείο για μια εικονική μηχανή

Μπορείτε να δημιουργήσετε ένα απομακρυσμένο υπολογιστή τελικό σημείο για να προωθήσετε την κίνηση του δικτύου από μια δημόσια θύρα σε μια τοπική θύρα για μια συγκεκριμένη εικονική μηχανή χρησιμοποιώντας `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Κατάργηση εικονική μηχανή από εξισορρόπηση φόρτου

Μπορείτε να καταργήσετε μια εικονική μηχανή από μια εσωτερική εξισορρόπηση φόρτου Ορισμός διαγράφοντας το σχετικό τελικό σημείο. Αφού καταργηθεί το τελικό σημείο, η εικονική μηχανή δεν θα συμπεριληφθεί η τη μονάδα εξισορρόπησης φόρτου Ορισμός πλέον.

 Χρησιμοποιώντας το παραπάνω παράδειγμα, μπορείτε να καταργήσετε το τελικό σημείο που δημιουργήσατε για εικονική μηχανή "Φθίνουσα 1" από το εσωτερικό εξισορρόπηση φόρτου "ilbset", χρησιμοποιώντας την εντολή `azure vm endpoint delete`.

    azure vm endpoint delete DB1 tcp-1433-1433


Ανάληψη ελέγχου `azure vm endpoint --help` για περισσότερες πληροφορίες.


## <a name="next-steps"></a>Επόμενα βήματα

[Ρύθμιση παραμέτρων μια λειτουργία διανομής εξισορρόπησης φόρτου χρησιμοποιώντας συσχέτισης IP προέλευσης](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)