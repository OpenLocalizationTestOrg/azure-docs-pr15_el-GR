<properties 
   pageTitle="Ελέγχετε τη δρομολόγηση και χρήση εικονικού συσκευές με χρήση του PowerShell στο μοντέλο κλασική ανάπτυξης | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να ελέγξετε τη δρομολόγηση σε VNets χρήση του PowerShell στο μοντέλο κλασική ανάπτυξης"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Στοιχείο ελέγχου δρομολόγηση και χρήση εικονικού συσκευές (κλασικό) με χρήση του PowerShell

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο κλασική ανάπτυξης.

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Το δείγμα Azure PowerShell παρακάτω εντολές περιμένετε ένα απλό περιβάλλον που έχετε ήδη δημιουργήσει με βάση το παραπάνω σενάριο. Εάν θέλετε να εκτελέσετε τις εντολές που εμφανίζονται σε αυτό το έγγραφο, δημιουργήστε το περιβάλλον εμφανίζεται στη [Δημιουργία ενός VNet (κλασική) με χρήση του PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Δημιουργία του UDR για το υποδίκτυο προσκηνίου
Για να δημιουργήσετε τον πίνακα δρομολόγηση και δρομολόγηση που απαιτείται για το υποδίκτυο προσκηνίου με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

3. Εκτελέστε το **`New-AzureRouteTable`** cmdlet για να δημιουργήσετε έναν πίνακα δρομολόγηση για το υποδίκτυο προσκηνίου.

        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"

    Αποτέλεσμα:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. Εκτελέστε το **`Set-AzureRoute`** cmdlet για να δημιουργήσετε μια διαδρομή στον πίνακα δρομολόγηση δημιουργήθηκε παραπάνω για να στείλετε όλη την κυκλοφορία που προορίζονται για το υποδίκτυο παρασκηνίου (192.168.2.0/24) για να το **FW1** Εικονική (192.168.0.4).
    
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

    Αποτέλεσμα:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. Εκτελέστε το **`Set-AzureSubnetRouteTable`** cmdlet για να συσχετίσετε τον πίνακα δρομολόγησης δημιουργήθηκε παραπάνω με το υποδίκτυο **FrontEnd** .

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Δημιουργία του UDR για το υποδίκτυο παρασκηνίου
Για να δημιουργήσετε τον πίνακα δρομολόγηση και δρομολόγηση που απαιτείται για το υποδίκτυο παρασκηνίου με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

3. Εκτελέστε το **`New-AzureRouteTable`** cmdlet για να δημιουργήσετε έναν πίνακα δρομολόγηση για το υποδίκτυο παρασκηνίου.

        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"

4. Εκτελέστε το **`Set-AzureRoute`** cmdlet για να δημιουργήσετε μια διαδρομή στον πίνακα δρομολόγηση δημιουργήθηκε παραπάνω για να στείλετε όλη την κυκλοφορία που προορίζονται για το υποδίκτυο προσκηνίου (192.168.1.0/24) για να το **FW1** Εικονική (192.168.0.4).

        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

5. Εκτελέστε το **`Set-AzureSubnetRouteTable`** cmdlet για να συσχετίσετε τον πίνακα δρομολόγησης δημιουργήθηκε παραπάνω με το υποδίκτυο **παρασκηνίου** .

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName BackEnd `
            -RouteTableName UDR-BackEnd

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Ενεργοποίηση της προώθησης IP σε η Εικονική FW1
Για να ενεργοποιήσετε την προώθηση κλήσεων σε η Εικονική FW1 IP, ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε το **`Get-AzureIPForwarding`** cmdlet για να έλεγχος της κατάστασης των προώθηση IP

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    Αποτέλεσμα:

        Disabled

2. Εκτελέστε το **`Set-AzureIPForwarding`** εντολή για να ενεργοποιήσετε την προώθηση IP για το *FW1* Εικονική.

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
