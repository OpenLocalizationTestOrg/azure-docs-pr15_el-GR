<properties 
   pageTitle="Σύνδεση σε δίκτυο εικονικού με ένα κύκλωμα ExpressRoute με χρήση του PowerShell | Microsoft Azure"
   description="Αυτό το έγγραφο παρέχει μια επισκόπηση των τη σύνδεση εικονικού δίκτυα (VNets) για να ExpressRoute κυκλώματα χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων και PowerShell."
   services="expressroute"
   documentationCenter="na"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr" />

# <a name="link-a-virtual-network-to-an-expressroute-circuit"></a>Σύνδεση σε δίκτυο εικονικού με ένα κύκλωμα ExpressRoute

> [AZURE.SELECTOR]
- [Azure πύλης - διαχείριση πόρων](expressroute-howto-linkvnet-portal-resource-manager.md)
- [PowerShell - διαχείριση πόρων](expressroute-howto-linkvnet-arm.md)
- [PowerShell - κλασικό](expressroute-howto-linkvnet-classic.md)


Σε αυτό το άρθρο θα σας βοηθήσει να συνδέσετε εικονικών δικτύων (VNets) κυκλώματα Azure ExpressRoute, χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων και PowerShell. Εικονικό δίκτυα μπορεί να είναι στην ίδια συνδρομή ή μέρος μια άλλη συνδρομή.

**Σχετικά με τα μοντέλα Azure ανάπτυξης**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Προαπαιτούμενα στοιχεία ρύθμισης παραμέτρων

- Χρειάζεστε την πιο πρόσφατη έκδοση του PowerShell Azure λειτουργικές μονάδες (τουλάχιστον έκδοση 1.0). Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .
- Πρέπει να εξετάσετε το [προαπαιτούμενα στοιχεία για τις](expressroute-prerequisites.md) [απαιτήσεις δρομολόγηση](expressroute-routing.md)και [ροές εργασίας](expressroute-workflows.md) πριν να ξεκινήσετε τη ρύθμιση των παραμέτρων.
- Πρέπει να έχετε ένα ενεργό κύκλωμα ExpressRoute. 
    - Ακολουθήστε τις οδηγίες για να [δημιουργήσετε ένα κύκλωμα ExpressRoute](expressroute-howto-circuit-arm.md) και έχετε το κύκλωμα ενεργοποιημένη από την υπηρεσία παροχής σύνδεσης. 
    - Βεβαιωθείτε ότι έχετε Azure ιδιωτικό διεισδύουν έχει ρυθμιστεί για το κύκλωμα. Ανατρέξτε στο άρθρο [Ρύθμιση παραμέτρων δρομολόγησης](expressroute-howto-routing-arm.md) για δρομολόγηση οδηγίες. 
    - Βεβαιωθείτε ότι Azure ιδιωτικό διεισδύουν έχει ρυθμιστεί και το πρωτόκολλο BGP διεισδύουν μεταξύ του δικτύου και της Microsoft είναι προς τα επάνω, ώστε να έχετε τη δυνατότητα να ολοκληρωμένες συνδεσιμότητας.
    - Βεβαιωθείτε ότι έχετε ένα εικονικό δίκτυο και μια πύλη εικονικού δικτύου δημιουργούνται και πλήρως παροχή της υπηρεσίας. Ακολουθήστε τις οδηγίες για να δημιουργήσετε μια [πύλη VPN](../articles/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), αλλά μην ξεχάσετε να χρησιμοποιήσετε `-GatewayType ExpressRoute`.

Μπορείτε να συνδέσετε έως και 10 εικονικών δικτύων σε ένα τυπικό κύκλωμα ExpressRoute. Όλα τα δίκτυα εικονικού πρέπει να είναι στην ίδια περιοχή γεωπολιτική όταν χρησιμοποιείτε ένα τυπικό κύκλωμα ExpressRoute. 

Μπορείτε να συνδέσετε μια εικονική δίκτυα εκτός της περιοχής γεωπολιτική του κυκλώματος ExpressRoute ή να συνδεθείτε μεγαλύτερο αριθμό εικονικών δικτύων σας κυκλώματος ExpressRoute εάν έχετε ενεργοποιήσει το πρόσθετο premium ExpressRoute. Ανατρέξτε [στις συνήθεις Ερωτήσεις](expressroute-faqs.md) για περισσότερες λεπτομέρειες σχετικά με το πρόσθετο premium.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Σύνδεση ενός εικονικού δικτύου στην ίδια συνδρομή σε ένα κύκλωμα

Μπορείτε να συνδέσετε μια πύλη εικονικού δικτύου σε ένα κύκλωμα ExpressRoute χρησιμοποιώντας το ακόλουθο cmdlet. Βεβαιωθείτε ότι η πύλη εικονικού δικτύου δημιουργείται και είστε έτοιμοι για σύνδεση πριν να εκτελέσετε το cmdlet:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Σύνδεση ενός εικονικού δικτύου σε μια διαφορετική συνδρομή σε ένα κύκλωμα

Μπορείτε να μοιραστείτε ένα κύκλωμα ExpressRoute σε πολλές συνδρομές. Η παρακάτω εικόνα εμφανίζει μια απλή σχηματική της λειτουργίας πως κοινής χρήσης για κυκλώματα ExpressRoute σε πολλές συνδρομές.

Κάθε μία από το μικρότερο σύννεφων μέσα στο μεγάλο cloud χρησιμοποιείται για την αναπαράσταση συνδρομές που ανήκουν σε διαφορετικά τμήματα ενός οργανισμού. Κάθε ένα από τα τμήματα της εταιρείας μπορεί να χρησιμοποιήσει τις δικές τους συνδρομή για την ανάπτυξη, αλλά αυτές τις υπηρεσίες τους--να κάνετε κοινή χρήση ένα μεμονωμένο κύκλωμα ExpressRoute για να συνδεθείτε ξανά με το δίκτυο εσωτερικής εγκατάστασης. Ένα τμήμα (σε αυτό το παράδειγμα: IT) μπορούν να είναι κάτοχοι το κύκλωμα ExpressRoute. Άλλες συνδρομές εντός της εταιρείας να χρησιμοποιήσετε το κύκλωμα ExpressRoute.

>[AZURE.NOTE] Συνδεσιμότητα και το εύρος ζώνης χρεώσεων για το αποκλειστικό κύκλωμα θα εφαρμοστεί στον κάτοχο του κυκλώματος ExpressRoute. Όλα τα δίκτυα εικονικού μοιραστείτε το ίδιο εύρος ζώνης.

![Συνδεσιμότητα σταυρό συνδρομή](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Διαχείριση

Ο *κάτοχος κυκλώματος* είναι ο χρήστης εξουσιοδοτημένοι power του πόρου κυκλώματος ExpressRoute. Ο κάτοχος κυκλώματος να δημιουργήσετε άδειες που μπορεί να είναι εξαργυρώσατε από *τους χρήστες κυκλώματος*. *Κύκλωμα οι χρήστες* είναι οι κάτοχοι των πυλών εικονικού δικτύου (που δεν είναι μέσα σε την ίδια συνδρομή ως το κύκλωμα ExpressRoute). *Κύκλωμα οι χρήστες* μπορούν να αποκτήσουν άδειες (άδειας ανά εικονικού δικτύου).

Ο *κάτοχος κυκλώματος* έχει για να τροποποιήσετε και ανάκληση αδειών οποιαδήποτε στιγμή. Ανάκληση μια αποτελέσματα εξουσιοδότησης σε όλες τις συνδέσεις σύνδεσης που έχουν διαγραφεί από τη συνδρομή στην πρόσβαση των οποίων έχει ανακληθεί.

### <a name="circuit-owner-operations"></a>Λειτουργίες κάτοχο κυκλώματος 

#### <a name="creating-an-authorization"></a>Δημιουργία άδειας
    
Ο κάτοχος κυκλώματος δημιουργεί μια άδεια. Το αποτέλεσμα της δημιουργίας ενός κλειδιού εξουσιοδότησης που μπορούν να χρησιμοποιηθούν από το χρήστη κυκλώματος για να συνδέσετε τους πύλες εικονικού δικτύου για το κύκλωμα ExpressRoute. Η άδεια ισχύει για μόνο μία σύνδεση.

Το παρακάτω τμήμα κώδικα cmdlet δείχνει πώς μπορείτε να δημιουργήσετε μια άδεια:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

        $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
        

Απάντηση σε αυτή θα περιέχει το κλειδί εξουσιοδότησης και την κατάσταση:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded

        

#### <a name="reviewing-authorizations"></a>Εξέταση των εγκρίσεων

Ο κάτοχος κυκλώματος να αναθεωρήσετε όλες τις άδειες που εκδίδονται σε συγκεκριμένο κύκλωμα, εκτελώντας το ακόλουθο cmdlet:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
    

#### <a name="adding-authorizations"></a>Προσθήκη αδειών

Ο κάτοχος κυκλώματος να προσθέσετε άδειες, χρησιμοποιώντας το ακόλουθο cmdlet:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
    
    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit

    
#### <a name="deleting-authorizations"></a>Διαγραφή αδειών

Ο κάτοχος κυκλώματος να revoke/διαγραφή αδειών στο χρήστη, εκτελώντας το ακόλουθο cmdlet:

    Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit    

### <a name="circuit-user-operations"></a>Κύκλωμα χρήστη λειτουργιών

Ο χρήστης κυκλώματος πρέπει το Αναγνωριστικό ομότιμης σύνδεσης και ένα κλειδί εξουσιοδότησης από τον κάτοχο κυκλώματος. Το κλειδί εξουσιοδότησης είναι ένα GUID.

Είναι το Αναγνωριστικό ομότιμης σύνδεσης, μπορεί να ελεγχθεί από την ακόλουθη εντολή.

    Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"

#### <a name="redeeming-connection-authorizations"></a>Εξαργύρωση άδειες σύνδεσης

Ο χρήστης κυκλώματος να εκτελέσετε το ακόλουθο cmdlet για την εξαγορά αδείας σύνδεση:

    $id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"  
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"

#### <a name="releasing-connection-authorizations"></a>Αποδέσμευση άδειες σύνδεσης

Μπορείτε να αποδεσμεύσετε μια άδεια διαγράφοντας τη σύνδεση που συνδέεται το κύκλωμα ExpressRoute για να το εικονικό δίκτυο.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με το ExpressRoute, ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις ExpressRoute](expressroute-faqs.md).
