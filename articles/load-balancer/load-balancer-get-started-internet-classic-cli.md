<properties
   pageTitle="Να ξεκινήσετε τη δημιουργία Internet αντικριστές εξισορρόπηση φόρτου στο μοντέλο κλασική ανάπτυξης χρησιμοποιώντας το Azure CLI | Microsoft Azure"
   description="Μάθετε πώς να δημιουργείτε Internet αντικριστές εξισορρόπηση φόρτου στο μοντέλο κλασική ανάπτυξης χρησιμοποιώντας το CLI Azure"
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

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Να ξεκινήσετε τη δημιουργία Internet αντικριστές εξισορρόπηση φόρτου (κλασικό) στο το CLI Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο κλασική ανάπτυξης. Μπορείτε επίσης να [Μάθετε πώς να δημιουργείτε Internet αντικριστές εξισορρόπηση φόρτου χρησιμοποιώντας τη διαχείριση πόρων Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Βήμα προς βήμα τη δημιουργία αντικριστές εξισορρόπηση φόρτου χρησιμοποιώντας CLI Internet

Αυτός ο οδηγός δείχνει πώς μπορείτε να δημιουργήσετε μια μονάδα εξισορρόπησης φόρτου Internet με βάση το παραπάνω σενάριο.

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../../articles/xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.

2. Εκτελέστε την εντολή **λειτουργία azure ρύθμισης παραμέτρων** για να μεταβείτε σε κλασική λειτουργία, όπως φαίνεται παρακάτω.

        azure config mode asm

    Αναμενόμενο αποτέλεσμα:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Δημιουργία τελικού σημείου και φόρτωση εξισορρόπησης σύνολο

Το σενάριο προϋποθέτει τις εικονικές μηχανές "web1" και "web2" έχουν δημιουργηθεί.
Αυτός ο οδηγός θα δημιουργήσει ένα σύνολο εξισορρόπησης φόρτου χρησιμοποιώντας θύρας 80 ως δημόσια θύρας και τη θύρα 80 ως τοπική θύρα. Μια θύρα δοκιμή του είναι επίσης έχει ρυθμιστεί στη θύρα 80 και με το όνομα του συνόλου εξισορρόπησης φόρτου "lbset".


### <a name="step-1"></a>Βήμα 1

Δημιουργία του πρώτου τελικού σημείου και φόρτωση εξισορρόπησης συνόλου με χρήση `azure network vm endpoint create` για εικονική μηχανή "web1".

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

Παράμετροι χρησιμοποιούνται:

**-k** - τοπικό υπολογιστή εικονική θύρα<br>
**-o** - πρωτοκόλλου<BR>
**-t** - δοκιμή του θύρα<BR>
**-b** - όνομα εξισορρόπησης φόρτου<BR>

## <a name="step-2"></a>Βήμα 2

Προσθέστε μια δεύτερη εικονική μηχανή "web2" στο σύνολο εξισορρόπησης φόρτου.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>Βήμα 3

Επαλήθευση του χρησιμοποιεί ρύθμισης παραμέτρων εξισορρόπησης φόρτου `azure vm show` .

    azure vm show web1

Το αποτέλεσμα θα είναι:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Δημιουργήστε έναν απομακρυσμένο υπολογιστή τελικό σημείο για μια εικονική μηχανή

Μπορείτε να δημιουργήσετε ένα απομακρυσμένο υπολογιστή τελικό σημείο για να προωθήσετε την κίνηση του δικτύου από μια δημόσια θύρα σε μια τοπική θύρα για μια συγκεκριμένη εικονική μηχανή χρησιμοποιώντας `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Κατάργηση εικονική μηχανή από εξισορρόπηση φόρτου

Πρέπει να διαγράψετε το τελικό σημείο που σχετίζονται με τη μονάδα εξισορρόπησης φόρτου Ορισμός από την εικονική μηχανή. Αφού καταργηθεί το τελικό σημείο, η εικονική μηχανή δεν ανήκει τη μονάδα εξισορρόπησης φόρτου Ορισμός πλέον.

 Χρησιμοποιώντας το παραπάνω παράδειγμα, μπορείτε να καταργήσετε το τελικό σημείο που δημιουργήσατε για εικονική μηχανή "web1" από εξισορρόπηση φόρτου "lbset" με την εντολή `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] Μπορείτε να εξερευνήσετε περισσότερες επιλογές για τη Διαχείριση χρησιμοποιώντας την εντολή τα τελικά σημεία`azure vm endpoint --help`


## <a name="next-steps"></a>Επόμενα βήματα

[Γρήγορα αποτελέσματα με τη ρύθμιση των παραμέτρων μιας εσωτερικής εξισορρόπηση φόρτου](load-balancer-get-started-ilb-arm-ps.md)

[Ρύθμιση παραμέτρων μιας λειτουργίας διανομής εξισορρόπησης φόρτου](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)

