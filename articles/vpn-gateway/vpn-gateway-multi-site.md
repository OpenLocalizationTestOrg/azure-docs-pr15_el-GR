<properties 
   pageTitle="Σύνδεση σε δίκτυο εικονικού με πολλές τοποθεσίες χρησιμοποιώντας πύλης VPN και PowerShell | Microsoft Azure"
   description="Σε αυτό το άρθρο θα σας καθοδηγήσει τη σύνδεση πολλές τοποθεσίες του τοπικού εσωτερικής εγκατάστασης σε ένα εικονικό δίκτυο με χρήση μιας πύλης VPN για το μοντέλο κλασική ανάπτυξης."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="yushwang" />

# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Προσθήκη σύνδεσης τοποθεσίας σε τοποθεσία για μια VNet με μια υπάρχουσα σύνδεση VPN πύλης

> [AZURE.SELECTOR]
- [Διαχείριση πόρων - πύλη](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Κλασικό - PowerShell](vpn-gateway-multi-site.md)

Σε αυτό το άρθρο σάς καθοδηγεί σε χρήση του PowerShell για να προσθέσετε συνδέσεις-τοποθεσίας (S2S) σε μια πύλη VPN που έχει μια υπάρχουσα σύνδεση. Αυτός ο τύπος σύνδεσης αναφέρεται συχνά ως "πολλών τοποθεσία" Ρύθμιση παραμέτρων. 

Σε αυτό το άρθρο ισχύει για το εικονικό δίκτυα που δημιουργούνται με χρήση του μοντέλου κλασική ανάπτυξης (γνωστό και ως υπηρεσία διαχείρισης). Αυτά τα βήματα δεν ισχύουν για ExpressRoute /--τοποθεσίας συνυπάρχουσες ρυθμίσεις παραμέτρων σύνδεσης. Για πληροφορίες σχετικά με τις συνδέσεις συνυπάρχουσες, ανατρέξτε στο θέμα [ExpressRoute/S2S συνυπάρχουσες συνδέσεις](../expressroute/expressroute-howto-coexist-classic.md) .

### <a name="deployment-models-and-methods"></a>Μοντέλα ανάπτυξης και οι μέθοδοι

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Ενημερώνουμε αυτόν τον πίνακα ως νέα άρθρα και πρόσθετα εργαλεία γίνονται διαθέσιμοι για αυτήν τη ρύθμιση παραμέτρων. Όταν ένα άρθρο είναι διαθέσιμη, θα σας σύνδεση απευθείας σε αυτήν από αυτόν τον πίνακα.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 



## <a name="about-connecting"></a>Σχετικά με τη σύνδεση

Μπορείτε να συνδεθείτε πολλές τοποθεσίες εσωτερικής εγκατάστασης σε ένα μεμονωμένο εικονικού δικτύου. Αυτό είναι ιδιαίτερα ελκυστική για τη δημιουργία υβριδικού λύσεων cloud. Δημιουργία σύνδεσης πολλών τοποθεσιών με την πύλη Azure εικονικού δικτύου είναι παρόμοια με τη δημιουργία άλλων συνδέσεων τοποθεσίας σε τοποθεσία. Στην πραγματικότητα, μπορείτε να χρησιμοποιήσετε μια υπάρχουσα πύλη Azure VPN, με την προϋπόθεση ότι η πύλη είναι σε δυναμικό (δρομολόγηση βάσει).

Εάν έχετε ήδη μια στατική πύλη συνδεδεμένοι στο δίκτυό σας εικονικού, μπορείτε να αλλάξετε τον τύπο πύλη σε δυναμικό χωρίς να χρειάζεται να ξαναδημιουργήσετε το εικονικό δίκτυο για να χωρέσει πολλών τοποθεσίας. Πριν να αλλάξετε τον τύπο δρομολόγησης, βεβαιωθείτε ότι την πύλη VPN εσωτερικής υποστηρίζει ρυθμίσεις παραμέτρων βάσει δρομολόγηση VPN. 

![διάγραμμα πολλών τοποθεσίας] (./media/vpn-gateway-multi-site/multisite.png "πολλές τοποθεσίες")

## <a name="points-to-consider"></a>Σημεία πρέπει να λάβετε υπόψη

**Δεν θα μπορούν να χρησιμοποιήσουν την πύλη κλασική Azure για να κάνετε αλλαγές σε αυτό το εικονικό δίκτυο.** Για αυτήν την έκδοση, θα χρειαστεί να κάνετε αλλαγές σε αρχείο ρύθμισης παραμέτρων του δικτύου αντί να χρησιμοποιήσετε την πύλη κλασική Azure. Εάν κάνετε αλλαγές στην πύλη του κλασική Azure, θα αντικαθιστούν τις ρυθμίσεις αναφοράς πολλών τοποθεσιών για αυτό το εικονικό δίκτυο. 

Θα πρέπει να καταλάβετε αρκετά χρησιμοποιώντας το αρχείο ρύθμισης παραμέτρων δικτύου, την ώρα που έχετε ολοκληρώσει τη διαδικασία πολλών τοποθεσίας. Ωστόσο, εάν έχετε πολλά άτομα που εργάζονται σε διαμόρφωση του δικτύου σας, θα πρέπει να βεβαιωθείτε ότι όλοι οι χρήστες να γνωρίζει για αυτόν τον περιορισμό. Αυτό δεν σημαίνει ότι δεν μπορείτε να χρησιμοποιήσετε την πύλη κλασική Azure καθόλου. Μπορείτε να το χρησιμοποιήσετε για όλα τα υπόλοιπα, εκτός από τις αλλαγές ρύθμισης παραμέτρων σε αυτό το συγκεκριμένο εικονικό δίκτυο.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Πριν ξεκινήσετε τη ρύθμιση των παραμέτρων, βεβαιωθείτε ότι έχετε τα εξής:

- Μια συνδρομή του Azure. Εάν δεν έχετε ήδη μια συνδρομή του Azure, μπορείτε να ενεργοποιήσετε το [MSDN συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ή το σύμβολο του για έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/).

- Συμβατό υλικό VPN για κάθε εσωτερική θέση. Ελέγξτε [Για συσκευές VPN για εικονικού η συνδεσιμότητα του δικτύου](vpn-gateway-about-vpn-devices.md) για να επαληθεύσετε εάν η συσκευή που θέλετε να χρησιμοποιήσετε είναι κάτι που είναι γνωστό ώστε να είναι συμβατός.

- Μια εξωτερική αντικριστές δημόσια IPv4 διεύθυνση IP για κάθε συσκευή VPN. Η διεύθυνση IP δεν μπορεί να βρίσκεται πίσω από μια συσκευή NAT. Αυτή είναι η απαίτηση.

- Θα πρέπει να εγκαταστήσετε την πιο πρόσφατη έκδοση του τα cmdlet του Azure PowerShell. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .

- Κάποιο άτομο που είναι ειδικές γνώσεις στη ρύθμιση παραμέτρων του υλικού VPN. Δεν θα μπορείτε να χρησιμοποιήσετε τις αυτόματης δημιουργίας VPN δέσμες ενεργειών από την πύλη κλασική Azure για να ρυθμίσετε τις συσκευές σας VPN. Αυτό σημαίνει ότι θα πρέπει να έχετε κατανοήσει πώς μπορείτε να ρυθμίσετε τη συσκευή σας VPN ή αν εργάζεστε με κάποιο άτομο που κάνει ισχυρό.

- Το περιοχές διευθύνσεων IP που θέλετε να χρησιμοποιήσετε για το εικονικό δίκτυο (Εάν δεν έχετε ήδη δημιουργήσει ένα). 

- Περιοχές διευθύνσεων IP για κάθε μία από τις τοποθεσίες του τοπικού δικτύου που θα τη σύνδεση με. Θα πρέπει να βεβαιωθείτε ότι η διεύθυνση IP περιοχές για κάθε μία από τις τοποθεσίες του τοπικού δικτύου που θέλετε να συνδεθείτε για να μην επικαλύπτονται. Διαφορετικά, η πύλη κλασική Azure ή το REST API θα απορρίψει τη ρύθμιση παραμέτρων που αποστέλλεται. 

    Για παράδειγμα, εάν έχετε δύο τοποθεσίες τοπικού δικτύου που περιέχουν και οι δύο το 10.2.3.0/24 περιοχή διεύθυνση IP και έχετε ένα πακέτο με μια διεύθυνση προορισμού 10.2.3.3, Azure δεν θα γνωρίζετε ποια τοποθεσία που θέλετε να αποστείλετε το πακέτο επειδή επικαλύπτει τις περιοχές διευθύνσεων. Για να αποτρέψετε δρομολόγησης θέματα, Azure δεν σας επιτρέπει να αποστείλετε ένα αρχείο ρύθμισης παραμέτρων που έχει επικαλυπτόμενες περιοχές.



## <a name="1-create-a-site-to-site-vpn"></a>1. να δημιουργήσετε ένα VPN τοποθεσίας σε τοποθεσία

Εάν έχετε ήδη ένα VPN τοποθεσίας σε τοποθεσία με μια δυναμική πύλη δρομολόγησης, εξαιρετικός! Μπορείτε να συνεχίσετε να [εξαγάγετε τις ρυθμίσεις παραμέτρων εικονικού δικτύου](#export). Εάν δεν είναι, κάντε τα εξής:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Εάν έχετε ήδη ένα εικονικό δίκτυο-τοποθεσίας, αλλά έχει μια στατική πύλη δρομολόγησης (πολιτικής βάσει):

1. Αλλάξτε τον τύπο πύλης δυναμική δρομολόγηση. Ένα VPN πολλών τοποθεσίας απαιτεί μια δυναμική πύλη δρομολόγησης (γνωστό και ως δρομολόγηση βάσει). Για να αλλάξετε τον τύπο πύλης σας, θα πρέπει να πρώτα διαγράψετε την υπάρχουσα πύλη, στη συνέχεια, δημιουργήστε ένα νέο. Για οδηγίες, ανατρέξτε στο θέμα [πώς μπορείτε να αλλάξετε τον τύπο δρομολόγησης VPN για την πύλη](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway).  

2. Ρύθμιση των παραμέτρων σας νέα πύλη και δημιουργήστε σας διοχέτευσης VPN. Για οδηγίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μιας πύλης VPN στην πύλη κλασική Azure](vpn-gateway-configure-vpn-gateway-mp.md). Πρώτα, αλλάξτε τον τύπο πύλης σε δυναμική δρομολόγηση. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Εάν δεν έχετε ένα εικονικό δίκτυο-τοποθεσίας:

1. Δημιουργία τοποθεσίας σε τοποθεσία εικονικού δικτύου με αυτές τις οδηγίες: [Δημιουργία εικονικό δίκτυο με μια σύνδεση VPN--τοποθεσίας στην πύλη κλασική Azure](vpn-gateway-site-to-site-create.md).  

2. Ρύθμιση παραμέτρων μιας δυναμικής δρομολόγησης πύλης χρησιμοποιώντας τις παρακάτω οδηγίες: [ρύθμιση των παραμέτρων μιας πύλης VPN](vpn-gateway-configure-vpn-gateway-mp.md). Φροντίστε να επιλέξετε **δυναμική δρομολόγηση** για τον τύπο της πύλης.

## <a name="export"></a>2. εξαγωγή του αρχείου ρύθμισης παραμέτρων δικτύου 

Εξαγάγετε το αρχείο ρύθμισης παραμέτρων δικτύου. Το αρχείο που εξάγετε θα χρησιμοποιηθεί για τη ρύθμιση παραμέτρων οι νέες ρυθμίσεις πολλών τοποθεσίας. Εάν χρειάζεστε οδηγίες σχετικά με τον τρόπο για να εξαγάγετε ένα αρχείο, ανατρέξτε στην ενότητα στο άρθρο: [πώς μπορείτε να δημιουργήσετε ένα VNet χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων δικτύου στην πύλη του Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3. Ανοίξτε το αρχείο ρύθμισης παραμέτρων δικτύου

Ανοίξτε το αρχείο ρύθμισης παραμέτρων δικτύου που έχετε λάβει στο τελευταίο βήμα. Χρησιμοποιήστε οποιοδήποτε πρόγραμμα επεξεργασίας xml που θέλετε. Το αρχείο θα πρέπει να μοιάζει με το εξής:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. Προσθήκη πολλών αναφορών σε τοποθεσίες

Όταν προσθέτετε ή καταργείτε πληροφορίες αναφοράς τοποθεσίας, θα μπορείτε να κάνετε αλλαγές στη ρύθμιση παραμέτρων για το ConnectionsToLocalNetwork/LocalNetworkSiteRef. Προσθήκη μιας νέας ενεργοποιεί αναφορά τοπική τοποθεσία Azure για να δημιουργήσετε μια νέα διοχέτευσης. Στο παρακάτω παράδειγμα, η ρύθμιση παραμέτρων δικτύου είναι για μια σύνδεση μίας τοποθεσίας. Αφού ολοκληρώσετε τις αλλαγές σας, αποθηκεύστε το αρχείο.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. Εισαγάγετε το αρχείο ρύθμισης παραμέτρων δικτύου

Εισαγάγετε το αρχείο ρύθμισης παραμέτρων δικτύου. Όταν εισάγετε αυτό το αρχείο με τις αλλαγές, το νέο σήραγγες θα προστεθούν. Το σήραγγες θα χρησιμοποιήσει η δυναμική πύλη που δημιουργήσατε νωρίτερα. Εάν χρειάζεστε οδηγίες σχετικά με τον τρόπο για να εισαγάγετε το αρχείο, ανατρέξτε στην ενότητα στο άρθρο: [πώς μπορείτε να δημιουργήσετε ένα VNet χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων δικτύου στην πύλη του Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. Κάντε λήψη πλήκτρα

Μόλις προστεθεί το νέο σήραγγες, χρησιμοποιήστε το cmdlet του PowerShell `Get-AzureVNetGatewayKey` για να λάβετε τα ασφαλείας IP/IKE προ-κοινόχρηστο κλειδιά για κάθε διοχέτευση.

Για παράδειγμα:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Εάν προτιμάτε, μπορείτε επίσης να χρησιμοποιήσετε το REST API *Γρήγορα εικονικού δικτύου πύλης σε κοινή χρήση αριθμού-κλειδιού* για να λάβετε τα πλήκτρα ήδη κοινόχρηστο.

## <a name="7-verify-your-connections"></a>7. Επαληθεύστε τις συνδέσεις σας

Ελέγξτε την κατάσταση πολλών τοποθεσία διοχέτευσης. Μετά τη λήψη τα πλήκτρα για κάθε διοχέτευση, θα θέλετε να επαληθεύσετε συνδέσεις. Χρήση `Get-AzureVnetConnection` να λάβετε μια λίστα με σήραγγες εικονικού δικτύου, όπως φαίνεται στο παρακάτω παράδειγμα. VNet1 είναι το όνομα του VNet.

    Get-AzureVnetConnection -VNetName VNET1
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με τις πύλες VPN, ανατρέξτε στο θέμα [Σχετικά με τις πύλες VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).

