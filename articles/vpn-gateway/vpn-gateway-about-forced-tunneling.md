<properties 
   pageTitle="Ρύθμιση παραμέτρων εξαναγκασμένη διοχέτευση για συνδέσεις τοποθεσίας σε τοποθεσία με χρήση του μοντέλου κλασική ανάπτυξης | Microsoft Azure"
   description="Πώς μπορείτε να κάνετε ανακατεύθυνση ή 'επιβάλετε' όλη την κυκλοφορία Internet συνδεδεμένων στην τοποθεσία σας εσωτερικής εγκατάστασης."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Ρύθμιση παραμέτρων εξαναγκασμένη διοχέτευση χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης

> [AZURE.SELECTOR]
- [PowerShell - κλασικό](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - διαχείριση πόρων](vpn-gateway-forced-tunneling-rm.md)

Εξαναγκασμένη διοχέτευση σάς επιτρέπει να ανακατεύθυνση ή "ισχύει" όλη την κυκλοφορία Internet συνδεδεμένων πίσω στην τοποθεσία σας στην εσωτερική εγκατάσταση μέσω διοχέτευσης VPN τοποθεσίας σε τοποθεσία για επιθεώρηση και έλεγχος. Αυτή είναι μια απαίτηση κρίσιμες ασφαλείας για τα περισσότερα για μεγάλες επιχειρήσεις IT πολιτικές. 

Χωρίς εξαναγκασμένη διοχέτευση κυκλοφορία Internet συνδεδεμένων από το ΣΠΣ στο Azure θα πάντα διέλευση από το Azure την υποδομή δικτύου απευθείας ανάληψη στο Internet, χωρίς την επιλογή για να σας επιτρέπουν να ελέγξετε ή να ελέγχετε την κυκλοφορία. Ενδέχεται να οδηγήσουν σε κοινοποίηση πληροφοριών ή άλλους τύπους παραβιάσεις ασφαλείας μη εξουσιοδοτημένη πρόσβαση στο Internet.

Σε αυτό το άρθρο θα σας καθοδηγήσει τη ρύθμιση των παραμέτρων υποχρεωτική διοχέτευση για εικονικού δίκτυα που δημιουργούνται με χρήση του μοντέλου κλασική ανάπτυξης. 

**Σχετικά με τα μοντέλα Azure ανάπτυξης**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Ανάπτυξη μοντέλων και εργαλεία για την εξαναγκασμένη διοχέτευση**

Εξαναγκασμένη διοχέτευσης σύνδεσης μπορεί να ρυθμιστεί για το μοντέλο κλασική ανάπτυξης και το μοντέλο ανάπτυξης διαχείρισης πόρων. Ανατρέξτε στον παρακάτω πίνακα για περισσότερες πληροφορίες. Ενημερώνουμε αυτόν τον πίνακα ως νέα άρθρα, νέα μοντέλα ανάπτυξης και πρόσθετα εργαλεία γίνονται διαθέσιμοι για αυτήν τη ρύθμιση παραμέτρων. Όταν ένα άρθρο είναι διαθέσιμη, θα σας σύνδεση απευθείας σε αυτήν από τον πίνακα.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="requirements-and-considerations"></a>Απαιτήσεις και τα θέματα

Εξαναγκασμένη διοχέτευση στο Azure έχει ρυθμιστεί μέσω δρομολογεί εικονικού δικτύου που ορίζονται από το χρήστη (UDR). Ανακατεύθυνση κίνηση σε μια εσωτερική τοποθεσία εκφράζεται ως μια προεπιλεγμένη δρομολόγηση για την πύλη Azure VPN. Ενότητα που ακολουθεί παραθέτει την τρέχουσα τον περιορισμό του πίνακα και δρομολογεί δρομολόγησης για ένα δίκτυο εικονικού Azure:


-  Κάθε υποδίκτυο εικονικού δικτύου έχει ένα ενσωματωμένο, πίνακας δρομολόγησης συστήματος. Ο πίνακας δρομολόγησης συστήματος περιλαμβάνει τις ακόλουθες τρεις ομάδες διαδρομών:

    - **Δρομολογεί την τοπική VNet:** Απευθείας στον προορισμό της ΣΠΣ στο ίδιο δίκτυο εικονικού
    
    - **Στην εσωτερική εγκατάσταση δρομολογεί:** Για την πύλη Azure VPN
    
    - **Προεπιλεγμένη δρομολόγηση:** Απευθείας με το Internet. Πακέτα που προορίζονται για να το ιδιωτικών διευθύνσεων IP δεν καλύπτεται από την προηγούμενη δύο δρομολογεί θα καταργηθεί.


-  Με την έκδοση του δρομολογεί ορίζονται από το χρήστη, μπορείτε να δημιουργήσετε έναν πίνακα δρομολόγησης για να προσθέσετε μια προεπιλεγμένη δρομολόγηση και, στη συνέχεια, να συσχετίσετε τον πίνακα δρομολόγησης για να σας subnet(s) VNet για να ενεργοποιήσετε την εξαναγκασμένη διοχέτευση σε αυτά τα δευτερεύοντα δίκτυα.

- Πρέπει να ορίσετε μια "προεπιλεγμένη τοποθεσία" μεταξύ των τοποθεσιών τοπικού σταυρό εσωτερικής εγκατάστασης συνδεδεμένοι με το εικονικό δίκτυο.

- Εξαναγκασμένη διοχέτευση πρέπει να σχετίζονται με ένα VNet που έχει μια δυναμική δρομολόγηση πύλη VPN (όχι μια στατική πύλη).
 
- ExpressRoute υποχρεωτική διοχέτευση δεν έχει ρυθμιστεί μέσω αυτού του μηχανισμού, αλλά αντί για αυτό, είναι ενεργοποιημένη από διαφήμιση μια προεπιλεγμένη δρομολόγηση μέσω του peering περιόδους λειτουργίας το πρωτόκολλο BGP ExpressRoute. Ανατρέξτε στην [Τεκμηρίωση ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) για περισσότερες πληροφορίες.



## <a name="configuration-overview"></a>Επισκόπηση της ρύθμισης παραμέτρων

Στο παρακάτω παράδειγμα, το Frontend υποδικτύου δεν είναι υποχρεωτική γίνει διοχέτευση. Το φόρτο εργασίας στο δευτερεύον Frontend να συνεχίσετε να αποδεχτείτε και απάντηση σε αιτήματα πελατών από το Internet απευθείας. Είστε υποχρεωμένοι των δευτερευόντων δικτύων ενδιάμεσο επίπεδο και παρασκηνίου διοχέτευσης. Οποιαδήποτε εξερχόμενες συνδέσεις από αυτές τις δύο δευτερευόντων δικτύων στο Internet θα υποχρεωτική ή θα ανακατευθύνεται ξανά με μια τοποθεσία εσωτερικής μέσω ένα από τα σήραγγες S2S VPN.

Αυτό σας επιτρέπει να περιορισμός και έλεγχος πρόσβασης στο Internet από το εικονικές μηχανές ή στο cloud services σε Azure, ενώ συνεχίζετε να ενεργοποιήσετε την αρχιτεκτονική πολλαπλών επιπέδων υπηρεσίας απαιτείται. Μπορείτε επίσης να εφαρμόσετε εξαναγκασμένη διοχέτευση με τα δίκτυα ολόκληρο εικονικό εάν υπάρχουν χωρίς φόρτους εργασίας μέσω Internet στο εικονικού δίκτυά σας.


![Υποχρεωτική διοχέτευση](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Βεβαιωθείτε ότι έχετε τα ακόλουθα στοιχεία πριν να ξεκινήσετε τη ρύθμιση των παραμέτρων.

- Μια συνδρομή του Azure. Εάν δεν έχετε ήδη μια συνδρομή του Azure, μπορείτε να ενεργοποιήσετε το [MSDN συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ή το σύμβολο του για έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/).

- Ρύθμιση παραμέτρων εικονικό δίκτυο. 

- Η πιο πρόσφατη έκδοση του τα cmdlet του Azure PowerShell. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .


## <a name="configure-forced-tunneling"></a>Ρύθμιση παραμέτρων εξαναγκασμένη διοχέτευση

Η παρακάτω διαδικασία θα σας βοηθήσει να καθορίσετε εξαναγκασμένη διοχέτευση για ένα εικονικό δίκτυο. Τα βήματα ρύθμισης παραμέτρων αντιστοιχούν στο αρχείο ρύθμισης παραμέτρων δικτύου VNet.



    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

Σε αυτό το παράδειγμα, το εικονικό δίκτυο "MultiTier-VNet" περιλαμβάνει τρία δευτερεύοντα δίκτυα: δευτερεύοντα δίκτυα *Frontend*, *Midtier*και *παρασκηνίου* , με συνδέσεις τέσσερις διασταύρωσης εσωτερικής εγκατάστασης: *DefaultSiteHQ*και τρεις *διακλαδώσεις*. 

Τα βήματα θα ορίσετε το *DefaultSiteHQ* ως την προεπιλεγμένη σύνδεση τοποθεσίας για υποχρεωτική διοχέτευση και ρυθμίσετε τις παραμέτρους του Midtier και δευτερεύοντα δίκτυα παρασκηνίου για να χρησιμοποιήσετε υποχρεωτική διοχέτευση.


1. Δημιουργήστε έναν πίνακα δρομολόγησης. Χρησιμοποιήστε το ακόλουθο cmdlet για να δημιουργήσετε τον πίνακα δρομολόγηση.

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Προσθέστε μια προεπιλεγμένη δρομολόγηση στον πίνακα δρομολόγησης. 

    Το παρακάτω παράδειγμα προσθέτει μια προεπιλεγμένη δρομολόγηση στον πίνακα δρομολόγησης που δημιουργήσατε στο βήμα 1. Σημείωση που υποστηρίζεται μόνο η διαδρομή είναι το πρόθεμα προορισμού "0.0.0.0/0" για να το NextHop "VPNGateway".
 
        Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. Συσχέτιση του πίνακα δρομολόγησης για να τα δευτερεύοντα δίκτυα. 

    Αφού δημιουργηθεί έναν πίνακα δρομολόγησης και προσθέσει μια διαδρομή, χρησιμοποιήστε το παρακάτω παράδειγμα για να προσθέσετε ή να συσχετίσετε τον πίνακα δρομολόγησης με ένα δευτερεύον VNet. Το παράδειγμα προσθέτει τον πίνακα δρομολόγησης "MyRouteTable" το Midtier και παρασκηνίου δευτερεύοντα δίκτυα του VNet MultiTier-VNet.

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Εκχωρήστε μια προεπιλεγμένη τοποθεσία για υποχρεωτική διοχέτευση. 

    Στο προηγούμενο βήμα, τα δείγματα δεσμών ενεργειών cmdlet δημιουργήσει τον πίνακα δρομολόγησης και σχετικές πίνακα διαδρομών σε δύο από τα δευτερεύοντα δίκτυα VNet. Το υπόλοιπο βήμα είναι να επιλέξετε μια τοπική τοποθεσία μεταξύ των συνδέσεων πολλές τοποθεσίες του εικονικού δικτύου ως την προεπιλεγμένη τοποθεσία ή σωλήνα.

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## <a name="additional-powershell-cmdlets"></a>Επιπλέον cmdlet του PowerShell

### <a name="to-delete-a-route-table"></a>Για να διαγράψετε έναν πίνακα διαδρομών

    Remove-AzureRouteTable -Name <routeTableName>

### <a name="to-list-a-route-table"></a>Για να εμφανίσετε έναν πίνακα διαδρομών

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### <a name="to-delete-a-route-from-a-route-table"></a>Για να διαγράψετε μια διαδρομή από έναν πίνακα διαδρομών

    Remove-AzureRouteTable –Name <routeTableName>

### <a name="to-remove-a-route-from-a-subnet"></a>Για να καταργήσετε μια διαδρομή από ένα υποδίκτυο

    Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Για να εμφανίσετε τον πίνακα δρομολόγησης που σχετίζεται με ένα υποδίκτυο
    
    Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Για να καταργήσετε μια προεπιλεγμένη τοποθεσία από μια πύλη VNet VPN

    Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>






