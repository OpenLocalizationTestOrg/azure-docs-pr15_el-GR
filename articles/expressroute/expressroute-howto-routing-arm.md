<properties
   pageTitle="Πώς μπορείτε να ρυθμίσετε τις παραμέτρους δρομολόγησης για ένα κύκλωμα ExpressRoute | Microsoft Azure"
   description="Σε αυτό το άρθρο σάς καθοδηγεί στα βήματα για τη δημιουργία και την προμήθεια στο ιδιωτικό, δημόσιο και Microsoft διεισδύουν από ένα κύκλωμα ExpressRoute. Σε αυτό το άρθρο σας δείχνει επίσης πώς μπορείτε να ελέγξετε την κατάσταση, ενημέρωση ή διαγραφή peerings για το κύκλωμα."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Δημιουργία και τροποποίηση δρομολόγησης για ένα κύκλωμα ExpressRoute


> [AZURE.SELECTOR]
[Azure πύλης - διαχείριση πόρων](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - διαχείριση πόρων](expressroute-howto-routing-arm.md)
[PowerShell - κλασικό](expressroute-howto-routing-classic.md)



Σε αυτό το άρθρο σάς καθοδηγεί σε τα βήματα για να δημιουργήσετε και να διαχειριστείτε τις παραμέτρους δρομολόγησης για ένα κύκλωμα ExpressRoute με χρήση του PowerShell και το μοντέλο ανάπτυξης Azure διαχείριση πόρων.  Τα παρακάτω βήματα θα εμφανίσει επίσης πώς μπορείτε να ελέγξετε την κατάσταση, ενημέρωση ή διαγραφή και deprovision peerings για ένα κύκλωμα ExpressRoute. 


**Σχετικά με τα μοντέλα Azure ανάπτυξης**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Προαπαιτούμενα στοιχεία ρύθμισης παραμέτρων

- Θα χρειαστείτε την πιο πρόσφατη έκδοση των ενοτήτων Azure PowerShell, έκδοση 1.0 ή νεότερη έκδοση. 
- Βεβαιωθείτε ότι ελέγξετε στη σελίδα [τις προϋποθέσεις](expressroute-prerequisites.md) , τη σελίδα [δρομολόγηση απαιτήσεις](expressroute-routing.md) και τη σελίδα [ροές εργασίας](expressroute-workflows.md) πριν να ξεκινήσετε τη ρύθμιση των παραμέτρων.
- Πρέπει να έχετε ένα ενεργό κύκλωμα ExpressRoute. Ακολουθήστε τις οδηγίες στο θέμα [Δημιουργία μιας κυκλώματος ExpressRoute](expressroute-howto-circuit-arm.md) και έχετε το κύκλωμα ενεργοποιημένη από την υπηρεσία παροχής συνδεσιμότητας πριν να συνεχίσετε. Το κύκλωμα ExpressRoute πρέπει να είναι σε κατάσταση προμήθεια του φακέλου και να έχει ενεργοποιηθεί για να μπορέσετε να εκτελέσετε τα cmdlet που περιγράφεται παρακάτω.

Αυτές οι οδηγίες ισχύουν μόνο για κυκλώματα που δημιουργήθηκε με την υπηρεσία παροχής σας δίνει τη δυνατότητα υπηρεσιών συνδεσιμότητας επιπέδου 2. Εάν χρησιμοποιείτε μια υπηρεσία παροχής που παρέχουν υπηρεσίες διαχειριζόμενων Layer 3 (συνήθως ένα IPVPN, όπως MPLS), την υπηρεσία παροχής σύνδεσης θα ρύθμιση παραμέτρων και Διαχείριση δρομολόγησης για εσάς.

>[AZURE.IMPORTANT] Θα σας αυτήν τη στιγμή δεν κοινοποίηση peerings έχει ρυθμιστεί από υπηρεσίες παροχής μέσω της πύλης διαχείρισης υπηρεσίας. Εργαζόμαστε για την ενεργοποίηση σύντομα αυτήν τη δυνατότητα. Επικοινωνήστε με την υπηρεσία παροχής πριν από τη ρύθμιση των παραμέτρων peerings το πρωτόκολλο BGP.

Μπορείτε να ρυθμίσετε μία, δύο ή όλα τρεις peerings (Azure ιδιωτικές, Azure δημόσιο και Microsoft) για ένα κύκλωμα ExpressRoute. Μπορείτε να ρυθμίσετε τις παραμέτρους peerings με οποιαδήποτε σειρά που επιλέγετε. Ωστόσο, πρέπει να βεβαιωθείτε ότι ολοκληρώσετε τη ρύθμιση παραμέτρων του κάθε peering μία κάθε φορά. 

## <a name="azure-private-peering"></a>Azure ιδιωτικό διεισδύουν

Αυτή η ενότητα παρέχει οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε, να, ενημέρωση και διαγραφή την Azure ιδιωτικό peering ρύθμιση των παραμέτρων για ένα κύκλωμα ExpressRoute. 

### <a name="to-create-azure-private-peering"></a>Για να δημιουργήσετε Azure ιδιωτικό διεισδύουν

1. Εισαγωγή της λειτουργικής μονάδας PowerShell για ExpressRoute.
    
    Πρέπει να εγκαταστήσετε την πιο πρόσφατη installer PowerShell από τη [Συλλογή PowerShell](http://www.powershellgallery.com/) και εισαγάγετε τις λειτουργικές μονάδες από διαχειριστή πόρων Azure στην περίοδο λειτουργίας PowerShell για να ξεκινήσετε να χρησιμοποιείτε τα cmdlet ExpressRoute. Θα πρέπει να εκτελέσετε PowerShell ως διαχειριστής.

        Install-Module AzureRM

        Install-AzureRM

    Εισαγωγή όλων των ενοτήτων AzureRM.* μέσα στην περιοχή γνωστά σημασιολογίας έκδοση

        Import-AzureRM

    Μπορείτε να εισαγάγετε επίσης απλώς επιλέξτε λειτουργικής μονάδας μέσα στην περιοχή γνωστή σημασιολογίας έκδοση 
        
        Import-Module AzureRM.Network 

    Σύνδεση στο λογαριασμό σας

        Login-AzureRmAccount

    Επιλέξτε τη συνδρομή που θέλετε να δημιουργήσετε ExpressRoute κυκλώματος
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Δημιουργήστε ένα κύκλωμα ExpressRoute.
    
    Ακολουθήστε τις οδηγίες για να δημιουργήσετε ένα [κύκλωμα ExpressRoute](expressroute-howto-circuit-arm.md) και έχουν παρασχεθεί από την υπηρεσία παροχής σύνδεσης. 

    Εάν η υπηρεσία παροχής συνδεσιμότητας προσφέρει υπηρεσίες διαχειριζόμενων επιπέδου 3, μπορείτε να ζητήσετε την υπηρεσία παροχής σύνδεσης για να ενεργοποιήσετε την Azure ιδιωτικό διεισδύουν για εσάς. Σε αυτή την περίπτωση, δεν θα χρειαστεί να ακολουθήσετε τις οδηγίες που παρατίθενται στις επόμενες ενότητες. Ωστόσο, εάν η υπηρεσία παροχής συνδεσιμότητας δεν διαχειρίζεται δρομολόγησης για εσάς, μετά τη δημιουργία του κυκλώματος, ακολουθήστε τις παρακάτω οδηγίες. 

3. Ελέγξτε το κύκλωμα ExpressRoute για να βεβαιωθείτε ότι είναι παροχή της υπηρεσίας.

    Θα πρέπει να πρώτα να ελέγξετε εάν το κύκλωμα ExpressRoute είναι παροχή της υπηρεσίας και επίσης με δυνατότητα. Δείτε το παρακάτω παράδειγμα.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Η απόκριση θα είναι κάτι παρόμοιο με το παρακάτω παράδειγμα:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []


4. Ρύθμιση παραμέτρων Azure ιδιωτικό διεισδύουν για το κύκλωμα.

    Βεβαιωθείτε ότι έχετε τα ακόλουθα στοιχεία πριν να συνεχίσετε με τα επόμενα βήματα:

    - Μια /30 υποδικτύου για τη σύνδεση κύρια. Αυτό δεν πρέπει να είναι μέρος του οποιαδήποτε προορίζεται για δίκτυα εικονικό χώρο διευθύνσεων.
    - Μια /30 υποδικτύου για τη δευτερεύουσα σύνδεση. Αυτό δεν πρέπει να είναι μέρος του οποιαδήποτε προορίζεται για δίκτυα εικονικό χώρο διευθύνσεων.
    - Ένα έγκυρο Αναγνωριστικό VLAN για να δημιουργήσετε αυτό διεισδύουν σε. Βεβαιωθείτε ότι δεν υπάρχει άλλες διεισδύουν στο κύκλωμα χρησιμοποιεί το ίδιο αναγνωριστικό VLAN.
    - ΩΣ αριθμός για διεισδύουν. Μπορείτε να χρησιμοποιήσετε 2 byte και 4 byte με τη ΜΟΡΦΉ αριθμών. Μπορείτε να χρησιμοποιήσετε μια ιδιωτική ΩΣ αριθμός για αυτό διεισδύουν. Βεβαιωθείτε ότι δεν χρησιμοποιείτε 65515.
    - Μια κατακερματισμός MD5 Εάν επιλέξετε να χρησιμοποιήσετε ένα. **Αυτό είναι προαιρετικό**.
    
    Μπορείτε να εκτελέσετε το ακόλουθο cmdlet για ρύθμιση παραμέτρων του Azure ιδιωτικό διεισδύουν για το κύκλωμα.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Μπορείτε να χρησιμοποιήσετε το παρακάτω cmdlet Εάν επιλέξετε να χρησιμοποιήσετε ένα κατακερματισμός MD5.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Βεβαιωθείτε ότι καθορίζετε τον αριθμό AS ως διεισδύουν ASN πελατών ASN.

### <a name="to-view-azure-private-peering-details"></a>Για να προβάλετε Azure ιδιωτικό λεπτομέρειες διεισδύουν

Μπορείτε να λάβετε λεπτομέρειες ρύθμισης παραμέτρων χρησιμοποιώντας το ακόλουθο cmdlet

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### <a name="to-update-azure-private-peering-configuration"></a>Για να ενημερώσετε Azure προσωπική ρύθμιση παραμέτρων διεισδύουν

Μπορείτε να ενημερώσετε οποιοδήποτε τμήμα της ρύθμισης παραμέτρων χρησιμοποιώντας το ακόλουθο cmdlet. Στο παρακάτω παράδειγμα, το Αναγνωριστικό εικονικού ΔΙΚΤΎΟΥ του κυκλώματος ενημερώνεται από 100 σε 500.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-azure-private-peering"></a>Για να διαγράψετε Azure ιδιωτικό διεισδύουν

Μπορείτε να καταργήσετε τη ρύθμιση των παραμέτρων σας peering, εκτελώντας το ακόλουθο cmdlet.

>[AZURE.WARNING] Πρέπει να βεβαιωθείτε ότι όλα τα δίκτυα εικονικού είναι χωρίς σύνδεση από το κύκλωμα ExpressRoute πριν από την εκτέλεση αυτού του cmdlet. 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## <a name="azure-public-peering"></a>Azure δημόσια διεισδύουν

Αυτή η ενότητα παρέχει οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε, να, ενημέρωση και διαγραφή την Azure δημόσια peering ρύθμιση των παραμέτρων για ένα κύκλωμα ExpressRoute.

### <a name="to-create-azure-public-peering"></a>Για να δημιουργήσετε Azure δημόσια διεισδύουν

1. Εισαγωγή της λειτουργικής μονάδας PowerShell για ExpressRoute.
    
    Πρέπει να εγκαταστήσετε την πιο πρόσφατη installer PowerShell από τη [Συλλογή PowerShell](http://www.powershellgallery.com/) και εισαγάγετε τις λειτουργικές μονάδες από διαχειριστή πόρων Azure στην περίοδο λειτουργίας PowerShell για να ξεκινήσετε να χρησιμοποιείτε τα cmdlet ExpressRoute. Θα πρέπει να εκτελέσετε PowerShell ως διαχειριστής.

        Install-Module AzureRM

        Install-AzureRM

    Εισαγωγή όλων των ενοτήτων AzureRM.* μέσα στην περιοχή γνωστή σημασιολογίας έκδοση

        Import-AzureRM

    Μπορείτε να εισαγάγετε επίσης απλώς επιλέξτε λειτουργικής μονάδας μέσα στην περιοχή γνωστή σημασιολογίας έκδοση 
        
        Import-Module AzureRM.Network 

    Σύνδεση στο λογαριασμό σας

        Login-AzureRmAccount

    Επιλέξτε τη συνδρομή που θέλετε να δημιουργήσετε ExpressRoute κυκλώματος
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Δημιουργήστε ένα κύκλωμα ExpressRoute.
    
    Ακολουθήστε τις οδηγίες για να δημιουργήσετε ένα [κύκλωμα ExpressRoute](expressroute-howto-circuit-arm.md) και έχουν παρασχεθεί από την υπηρεσία παροχής σύνδεσης. 

    Εάν η υπηρεσία παροχής συνδεσιμότητας προσφέρει υπηρεσίες διαχειριζόμενων επιπέδου 3, μπορείτε να ζητήσετε την υπηρεσία παροχής σύνδεσης για να ενεργοποιήσετε την Azure δημόσια διεισδύουν για εσάς. Σε αυτή την περίπτωση, δεν θα χρειαστεί να ακολουθήσετε τις οδηγίες που παρατίθενται στις επόμενες ενότητες. Ωστόσο, εάν η υπηρεσία παροχής συνδεσιμότητας δεν διαχειρίζεται δρομολόγησης για εσάς, μετά τη δημιουργία του κυκλώματος, ακολουθήστε τις παρακάτω οδηγίες.

3. Επιλέξτε κυκλώματος ExpressRoute για να βεβαιωθείτε ότι είναι παροχή της υπηρεσίας.

    Θα πρέπει να πρώτα να ελέγξετε εάν το κύκλωμα ExpressRoute είναι παροχή της υπηρεσίας και επίσης με δυνατότητα. Δείτε το παρακάτω παράδειγμα.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Η απόκριση θα είναι κάτι παρόμοιο με το παρακάτω παράδειγμα:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   

4. Ρύθμιση παραμέτρων Azure δημόσια διεισδύουν για το κύκλωμα.

    Βεβαιωθείτε ότι έχετε τις ακόλουθες πληροφορίες για να προχωρήσετε περαιτέρω.

    - Μια /30 υποδικτύου για τη σύνδεση κύρια. Πρέπει να είναι ένα έγκυρο πρόθεμα IPv4 δημόσια.
    - Μια /30 υποδικτύου για τη δευτερεύουσα σύνδεση. Πρέπει να είναι ένα έγκυρο πρόθεμα IPv4 δημόσια.
    - Ένα έγκυρο Αναγνωριστικό VLAN για να δημιουργήσετε αυτό διεισδύουν σε. Βεβαιωθείτε ότι δεν υπάρχει άλλες διεισδύουν στο κύκλωμα χρησιμοποιεί το ίδιο αναγνωριστικό VLAN.
    - ΩΣ αριθμός για διεισδύουν. Μπορείτε να χρησιμοποιήσετε 2 byte και 4 byte με τη ΜΟΡΦΉ αριθμών.
    - Μια κατακερματισμός MD5 Εάν επιλέξετε να χρησιμοποιήσετε ένα. **Αυτό είναι προαιρετικό**.
    
    Μπορείτε να εκτελέσετε το ακόλουθο cmdlet για ρύθμιση παραμέτρων του Azure δημόσια διεισδύουν για το κύκλωμα

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Μπορείτε να χρησιμοποιήσετε το παρακάτω cmdlet Εάν επιλέξετε να χρησιμοποιήσετε ένα κατακερματισμός MD5

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


    >[AZURE.IMPORTANT] Βεβαιωθείτε ότι καθορίζετε τον αριθμό ΩΣ ως διεισδύουν ASN και πελατών ASN.

### <a name="to-view-azure-public-peering-details"></a>Για να προβάλετε Azure δημόσια λεπτομέρειες διεισδύουν

Μπορείτε να λάβετε λεπτομέρειες ρύθμισης παραμέτρων χρησιμοποιώντας το ακόλουθο cmdlet

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### <a name="to-update-azure-public-peering-configuration"></a>Για να ενημερώσετε ρύθμισης παραμέτρων διεισδύουν Azure δημόσια

Μπορείτε να ενημερώσετε οποιοδήποτε τμήμα της ρύθμισης παραμέτρων χρησιμοποιώντας το ακόλουθο cmdlet

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Το Αναγνωριστικό εικονικού ΔΙΚΤΎΟΥ του κυκλώματος ενημερώνεται από 200 600 στο παραπάνω παράδειγμα.

### <a name="to-delete-azure-public-peering"></a>Για να διαγράψετε Azure δημόσια διεισδύουν

Μπορείτε να καταργήσετε τη ρύθμιση των παραμέτρων σας peering, εκτελώντας το ακόλουθο cmdlet

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="microsoft-peering"></a>Microsoft διεισδύουν

Αυτή η ενότητα παρέχει οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε, να, ενημέρωση και διαγραφή τις παραμέτρους peering της Microsoft για ένα κύκλωμα ExpressRoute. 

### <a name="to-create-microsoft-peering"></a>Για να δημιουργήσετε διεισδύουν Microsoft

1. Εισαγωγή της λειτουργικής μονάδας PowerShell για ExpressRoute.
    
    Πρέπει να εγκαταστήσετε την πιο πρόσφατη installer PowerShell από τη [Συλλογή PowerShell](http://www.powershellgallery.com/) και εισαγάγετε τις λειτουργικές μονάδες από διαχειριστή πόρων Azure στην περίοδο λειτουργίας PowerShell για να ξεκινήσετε να χρησιμοποιείτε τα cmdlet ExpressRoute. Θα πρέπει να εκτελέσετε PowerShell ως διαχειριστής.

        Install-Module AzureRM

        Install-AzureRM

    Εισαγωγή όλων των ενοτήτων AzureRM.* μέσα στην περιοχή γνωστά σημασιολογίας έκδοση

        Import-AzureRM

    Μπορείτε να εισαγάγετε επίσης απλώς επιλέξτε λειτουργικής μονάδας μέσα στην περιοχή γνωστή σημασιολογίας έκδοση 
        
        Import-Module AzureRM.Network 

    Σύνδεση στο λογαριασμό σας

        Login-AzureRmAccount

    Επιλέξτε τη συνδρομή που θέλετε να δημιουργήσετε ExpressRoute κυκλώματος
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Δημιουργήστε ένα κύκλωμα ExpressRoute.
    
    Ακολουθήστε τις οδηγίες για να δημιουργήσετε ένα [κύκλωμα ExpressRoute](expressroute-howto-circuit-arm.md) και έχουν παρασχεθεί από την υπηρεσία παροχής σύνδεσης. 

    Εάν η υπηρεσία παροχής συνδεσιμότητας προσφέρει υπηρεσίες διαχειριζόμενων επιπέδου 3, μπορείτε να ζητήσετε την υπηρεσία παροχής σύνδεσης για να ενεργοποιήσετε την Azure ιδιωτικό διεισδύουν για εσάς. Σε αυτή την περίπτωση, δεν θα χρειαστεί να ακολουθήσετε τις οδηγίες που παρατίθενται στις επόμενες ενότητες. Ωστόσο, εάν η υπηρεσία παροχής συνδεσιμότητας δεν διαχειρίζεται δρομολόγησης για εσάς, μετά τη δημιουργία του κυκλώματος, ακολουθήστε τις παρακάτω οδηγίες.

3. Επιλέξτε κυκλώματος ExpressRoute για να βεβαιωθείτε ότι είναι παροχή της υπηρεσίας.

    Θα πρέπει να πρώτα να ελέγξετε εάν το κύκλωμα ExpressRoute είναι παροχή της υπηρεσίας και επίσης με δυνατότητα. Δείτε το παρακάτω παράδειγμα.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Η απόκριση θα είναι κάτι παρόμοιο με το παρακάτω παράδειγμα:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. Ρύθμιση παραμέτρων του Microsoft διεισδύουν για το κύκλωμα.

    Βεβαιωθείτε ότι έχετε τις ακόλουθες πληροφορίες πριν να συνεχίσετε.

    - Μια /30 υποδικτύου για τη σύνδεση κύρια. Πρέπει να είναι έγκυρο δημόσια IPv4 πρόθεμα ανήκει σε εσάς και καταγραφεί σε ένα RIR / IRR.
    - Μια /30 υποδικτύου για τη δευτερεύουσα σύνδεση. Πρέπει να είναι έγκυρο δημόσια IPv4 πρόθεμα ανήκει σε εσάς και καταγραφεί σε ένα RIR / IRR.
    - Ένα έγκυρο Αναγνωριστικό VLAN για να δημιουργήσετε αυτό διεισδύουν σε. Βεβαιωθείτε ότι δεν υπάρχει άλλες διεισδύουν στο κύκλωμα χρησιμοποιεί το ίδιο αναγνωριστικό VLAN.
    - ΩΣ αριθμός για διεισδύουν. Μπορείτε να χρησιμοποιήσετε 2 byte και 4 byte με τη ΜΟΡΦΉ αριθμών.
    - Κοινοποιείται προθέματα: πρέπει να δώσετε μια λίστα με όλα τα προθέματα σκοπεύετε να κοινοποιήσετε πάνω από την περίοδο λειτουργίας το πρωτόκολλο BGP. Γίνονται δεκτές μόνο από τα δημόσια προθέματα διευθύνσεων IP. Μπορείτε να στείλετε μια λίστα με τιμές διαχωρισμένες με κόμμα, εάν σκοπεύετε να στείλετε ένα σύνολο προθέματα. Αυτά τα προθέματα πρέπει να έχει εγγραφεί σε εσάς σε μια RIR / IRR.
    - Πελατών ASN: Εάν είστε προθέματα διαφημίσεις που δεν έχουν καταχωρηθεί για την διεισδύουν ΩΣ αριθμό, μπορείτε να καθορίσετε τον αριθμό AS στην οποία έχετε καταχωρήσει. **Αυτό είναι προαιρετικό**.
    - Δρομολόγηση όνομα μητρώου: Μπορείτε να καθορίσετε το RIR / IRR βάσει των οποίων τον αριθμό και στα προθέματα έχετε καταχωρήσει.
    - Μια κατακερματισμός MD5, εάν επιλέξετε να χρησιμοποιήσετε ένα. **Αυτό είναι προαιρετικό.**
    
    Μπορείτε να εκτελέσετε το ακόλουθο cmdlet για ρύθμιση παραμέτρων του Microsoft διεισδύουν για το κύκλωμα

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-get-microsoft-peering-details"></a>Για να δείτε λεπτομέρειες διεισδύουν Microsoft

Μπορείτε να βρείτε λεπτομέρειες ρύθμισης παραμέτρων χρησιμοποιώντας το ακόλουθο cmdlet.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### <a name="to-update-microsoft-peering-configuration"></a>Για να ενημερώσετε ρύθμισης παραμέτρων διεισδύουν Microsoft

Μπορείτε να ενημερώσετε οποιοδήποτε τμήμα της ρύθμισης παραμέτρων χρησιμοποιώντας το ακόλουθο cmdlet.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
        

### <a name="to-delete-microsoft-peering"></a>Για να διαγράψετε διεισδύουν Microsoft

Μπορείτε να καταργήσετε τη ρύθμιση των παραμέτρων σας peering, εκτελώντας το ακόλουθο cmdlet.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>Επόμενα βήματα

Επόμενο βήμα, [σύνδεση ένα VNet σε ένα κύκλωμα ExpressRoute](expressroute-howto-linkvnet-arm.md).

-  Για περισσότερες πληροφορίες σχετικά με τις ροές εργασίας ExpressRoute, ανατρέξτε στο θέμα [ExpressRoute ροών εργασίας](expressroute-workflows.md).

-  Για περισσότερες πληροφορίες σχετικά με το κύκλωμα διεισδύουν, ανατρέξτε στο θέμα [ExpressRoute κυκλώματα και δρομολόγηση τομείς](expressroute-circuit-peerings.md).

-  Για περισσότερες πληροφορίες σχετικά με την εργασία με virtual δίκτυα, ανατρέξτε στο θέμα [Επισκόπηση εικονικές δικτύου](../virtual-network/virtual-networks-overview.md).

