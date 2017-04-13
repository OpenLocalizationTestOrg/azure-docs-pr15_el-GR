<properties 
   pageTitle="Ελέγχετε τη δρομολόγηση και χρήση εικονικού συσκευές χρησιμοποιώντας το Azure CLI στο μοντέλο κλασική ανάπτυξης | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να ελέγξετε τη δρομολόγηση σε VNets χρησιμοποιώντας το Azure CLI στο μοντέλο κλασική ανάπτυξης"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Στοιχείο ελέγχου δρομολόγηση και χρήση εικονικού συσκευές (κλασικό) χρησιμοποιώντας το CLI Azure

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο κλασική ανάπτυξης. Μπορείτε επίσης να [ελέγχου δρομολόγηση και χρήση εικονικού συσκευές στο μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-network-create-udr-arm-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Οι εντολές Azure CLI δείγμα παρακάτω θεωρείτε ότι ένα απλό περιβάλλον ήδη δημιουργηθεί με βάση το παραπάνω σενάριο. Εάν θέλετε να εκτελέσετε τις εντολές που εμφανίζονται σε αυτό το έγγραφο, δημιουργήστε το περιβάλλον εμφανίζεται στη [Δημιουργία ενός VNet (κλασική) χρησιμοποιώντας το Azure CLI](virtual-networks-create-vnet-classic-cli.md).

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Δημιουργία του UDR για το υποδίκτυο προσκηνίου
Για να δημιουργήσετε τον πίνακα δρομολόγηση και δρομολόγηση που απαιτείται για το υποδίκτυο προσκηνίου με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε το **`azure config mode`** για να μεταβείτε σε κλασική λειτουργία.

        azure config mode asm

    Αποτέλεσμα:

        info:    New mode is asm

3. Εκτελέστε το **`azure network route-table create`** εντολή για να δημιουργήσετε έναν πίνακα διαδρομών για το υποδίκτυο προσκηνίου.

        azure network route-table create -n UDR-FrontEnd -l uswest

    Αποτέλεσμα:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    Παράμετροι:
    - **-l (ή--θέση)**. Azure περιοχής όπου θα δημιουργηθεί το νέο NSG. Για το σενάριο, *westus*.
    - **-n (ή--όνομα)**. Όνομα για το νέο NSG. Για το σενάριο, *NSG FrontEnd*.

4. Εκτελέστε το **`azure network route-table route set`** εντολή για να δημιουργήσετε μια διαδρομή στον πίνακα δρομολόγηση δημιουργήθηκε παραπάνω για να στείλετε όλη την κυκλοφορία που προορίζονται για το υποδίκτυο παρασκηνίου (192.168.2.0/24) για να το **FW1** Εικονική (192.168.0.4).

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    Αποτέλεσμα:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    Παράμετροι:
    - **-r (ή--όνομα πίνακα δρομολόγηση)**. Το όνομα του πίνακα δρομολόγηση όπου θα προστεθούν τη διαδρομή. Για το σενάριο, *UDR FrontEnd*.
    - **-(ή--πρόθεμα διεύθυνσης)**. Πρόθεμα διεύθυνσης για το υποδίκτυο όπου πακέτα προορίζονται για. Για το σενάριο, *192.168.2.0/24*.
    - **-t (--Επόμενο αναπήδηση-τύπου ή)**. Τύπος αντικειμένου κυκλοφορίας θα σταλεί σε. Πιθανές τιμές είναι *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*ή *κανένα*.
    - **-p (ή--Επόμενο-μεταπήδηση--διεύθυνση ip**). Η διεύθυνση IP για το επόμενο μεταπήδησης. Για το σενάριο, *192.168.0.4*.

5. Εκτελέστε το **`azure network vnet subnet route-table add`** εντολή για να συσχετίσετε τον πίνακα δρομολόγηση δημιουργήσατε παραπάνω με το υποδίκτυο **FrontEnd** .

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    Αποτέλεσμα:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    Παράμετροι:
    - **-t (ή--vnet-όνομα)**. Όνομα το VNet όπου βρίσκεται το υποδίκτυο. Για το σενάριο, *TestVNet*.
    - **- n (ή--υποδικτύου όνομα**. Το όνομα του πίνακα διαδρομών θα προστεθούν στο υποδικτύου. Για το σενάριο, *FrontEnd*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Δημιουργία του UDR για το υποδίκτυο παρασκηνίου
Για να δημιουργήσετε τον πίνακα δρομολόγηση και δρομολόγηση που απαιτείται για το υποδίκτυο παρασκηνίου με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

3. Εκτελέστε το **`azure network route-table create`** εντολή για να δημιουργήσετε έναν πίνακα διαδρομών για το υποδίκτυο παρασκηνίου.

        azure network route-table create -n UDR-BackEnd -l uswest

4. Εκτελέστε το **`azure network route-table route set`** εντολή για να δημιουργήσετε μια διαδρομή στον πίνακα δρομολόγηση δημιουργήθηκε παραπάνω για να στείλετε όλη την κυκλοφορία που προορίζονται για το υποδίκτυο προσκηνίου (192.168.1.0/24) για να το **FW1** Εικονική (192.168.0.4).

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. Εκτελέστε το **`azure network vnet subnet route-table add`** εντολή για να συσχετίσετε τον πίνακα δρομολόγηση δημιουργήσατε παραπάνω με το υποδίκτυο **παρασκηνίου** .

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd

