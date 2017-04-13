<properties
   pageTitle="Δημιουργία και τροποποίηση ένα κύκλωμα ExpressRoute με χρήση της διαχείρισης πόρων και PowerShell | Microsoft Azure"
   description="Σε αυτό το άρθρο περιγράφει πώς μπορείτε να δημιουργήσετε, προμήθεια, επαλήθευση, ενημέρωση, διαγραφή και deprovision ένα κύκλωμα ExpressRoute."
   documentationCenter="na"
   services="expressroute"
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
   ms.author="ganesr"/>


# <a name="create-and-modify-an-expressroute-circuit"></a>Δημιουργία και τροποποίηση ενός κυκλώματος ExpressRoute

> [AZURE.SELECTOR]
[Azure πύλης - διαχείριση πόρων](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - διαχείριση πόρων](expressroute-howto-circuit-arm.md)
[PowerShell - κλασικό](expressroute-howto-circuit-classic.md)


Σε αυτό το άρθρο περιγράφει τον τρόπο για να δημιουργήσετε ένα κύκλωμα Azure ExpressRoute με τη χρήση των cmdlet του Windows PowerShell και το μοντέλο ανάπτυξης διαχείρισης πόρων Azure. Σε αυτό το άρθρο θα εμφανιστούν και πώς να ελέγξετε την κατάσταση του κυκλώματος, ενημέρωση, ή διαγραφή και deprovision του.

**Σχετικά με τα μοντέλα Azure ανάπτυξης**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Πριν ξεκινήσετε


- Αποκτήστε την πιο πρόσφατη έκδοση του PowerShell Azure λειτουργικές μονάδες (τουλάχιστον έκδοση 1.0). Για αναλυτικές οδηγίες σχετικά με τον τρόπο για να ρυθμίσετε τον υπολογιστή σας για να χρησιμοποιήσετε τις ενότητες του PowerShell, ακολουθήστε τις οδηγίες στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

- Εξετάστε [τις προϋποθέσεις](expressroute-prerequisites.md) και [ροές εργασίας](expressroute-workflows.md) , πριν να ξεκινήσετε τη ρύθμιση των παραμέτρων.

## <a name="create-and-provision-an-expressroute-circuit"></a>Δημιουργία και παροχή ένα κύκλωμα ExpressRoute

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. συνδεθείτε στο λογαριασμό σας Azure και επιλέξτε τη συνδρομή σας

Για να ξεκινήσετε τη ρύθμιση των παραμέτρων σας, πραγματοποιήστε είσοδο στο λογαριασμό σας Azure. Για περισσότερες πληροφορίες σχετικά με το PowerShell, ανατρέξτε στο θέμα [Χρήση του Windows PowerShell με το διαχειριστή πόρων](../powershell-azure-resource-manager.md). Χρησιμοποιήστε τα παρακάτω παραδείγματα για να σας βοηθήσει να συνδεθείτε:

    Login-AzureRmAccount

Ελέγξτε τις συνδρομές για το λογαριασμό:

    Get-AzureRmSubscription

Επιλέξτε τη συνδρομή στην οποία θέλετε να δημιουργήσετε ένα κύκλωμα ExpressRoute για:

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. γρήγορα τη λίστα των υποστηριζόμενες υπηρεσίες παροχής, θέσεις και εύρος ζώνης

Πριν να δημιουργήσετε ένα κύκλωμα ExpressRoute, χρειάζεστε τη λίστα των υπηρεσιών παροχής υποστηριζόμενες συνδεσιμότητας, θέσεις και επιλογές εύρους ζώνης.

Το cmdlet του PowerShell `Get-AzureRmExpressRouteServiceProvider` επιστρέφει οι πληροφορίες που θα χρησιμοποιήσετε στα επόμενα βήματα:

    Get-AzureRmExpressRouteServiceProvider

Ελέγξτε εάν η υπηρεσία παροχής συνδεσιμότητας αναγράφεται εκεί. Σημειώστε τις ακόλουθες πληροφορίες επειδή που θα το χρειαστείτε αργότερα όταν δημιουργείτε ένα κύκλωμα:

- Όνομα

- PeeringLocations

- BandwidthsOffered

Τώρα είστε έτοιμοι να δημιουργήσετε ένα κύκλωμα ExpressRoute.   

### <a name="3-create-an-expressroute-circuit"></a>3. Δημιουργήστε ένα κύκλωμα ExpressRoute

Εάν δεν έχετε ήδη μια ομάδα πόρων, πρέπει να δημιουργήσετε ένα πριν να δημιουργήσετε το κύκλωμα ExpressRoute. Μπορείτε να το κάνετε, εκτελώντας την ακόλουθη εντολή:


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


Το παρακάτω παράδειγμα εμφανίζει τον τρόπο δημιουργίας ενός Mbps 200 ExpressRoute κύκλωμα μέσω Equinix στο κοίλο πυριτίου. Εάν χρησιμοποιείτε μια διαφορετική υπηρεσία παροχής και διαφορετικές ρυθμίσεις, αντικαταστήστε αυτές τις πληροφορίες όταν κάνετε την αίτησή σας. Ακολουθεί ένα παράδειγμα αίτηση για ένα νέο αριθμό-κλειδί υπηρεσίας:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Βεβαιωθείτε ότι καθορίζετε τη σωστή σειρά SKU και συγγενείς SKU:

- Επίπεδο SKU Καθορίζει εάν είναι ενεργοποιημένη μια τυπική ExpressRoute ή ένα πρόσθετο premium ExpressRoute. Μπορείτε να καθορίσετε *Τυπική* για να λάβετε την τυπική SKU ή *Premium* για το πρόσθετο premium.

- Οικογένεια SKU καθορίζει τον τύπο χρέωσης. Μπορείτε να καθορίσετε *Metereddata* για το πρόγραμμα με βάση τη χρήση δεδομένων και *Unlimiteddata* για απεριόριστο πρόγραμμα δεδομένων. Σημειώστε ότι μπορείτε να αλλάξετε τον τύπο χρεώσεων από *Metereddata* σε *Unlimiteddata*, αλλά δεν μπορείτε να αλλάξετε τον τύπο από *Unlimiteddata* σε *Metereddata*.


>[AZURE.IMPORTANT] Το κύκλωμα ExpressRoute θα χρεώνεστε από τη στιγμή που έχει εκδοθεί από έναν αριθμό-κλειδί υπηρεσίας. Βεβαιωθείτε ότι μπορείτε να εκτελέσετε αυτήν τη λειτουργία όταν είστε έτοιμοι να παρέχετε το κύκλωμα στην υπηρεσία παροχής σύνδεσης.

Η απόκριση περιέχει το κλειδί υπηρεσίας. Μπορείτε να λάβετε λεπτομερείς περιγραφές των όλες τις παραμέτρους, εκτελώντας τα εξής:


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. λίστα όλα τα κυκλώματα ExpressRoute

Για να λάβετε μια λίστα με όλα τα κυκλώματα ExpressRoute που δημιουργήσατε, εκτελέστε το `Get-AzureRmExpressRouteCircuit` εντολή:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Η απόκριση θα είναι παρόμοιο με το ακόλουθο παράδειγμα:


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
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Μπορείτε να ανακτήσετε αυτές τις πληροφορίες ανά πάσα στιγμή, χρησιμοποιώντας το `Get-AzureRmExpressRouteCircuit` cmdlet. Πραγματοποίηση της κλήσης χωρίς παραμέτρους παραθέτει όλα τα κυκλώματα. Τον αριθμό-κλειδί υπηρεσία θα εμφανίζεται στο πεδίο *ServiceKey* :


    Get-AzureRmExpressRouteCircuit


Η απόκριση θα είναι παρόμοιο με το ακόλουθο παράδειγμα:


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
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Μπορείτε να λάβετε λεπτομερείς περιγραφές των όλες τις παραμέτρους, εκτελώντας τα εξής:


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. στείλτε το κλειδί υπηρεσίας στην υπηρεσία παροχής σύνδεσης για παροχή

*ServiceProviderProvisioningState* παρέχει πληροφορίες σχετικά με την τρέχουσα κατάσταση της προμήθειας στην υπηρεσία παροχής πλευρά. Κατάσταση παρέχει την κατάσταση στην πλευρά της Microsoft. Για περισσότερες πληροφορίες σχετικά με την προμήθεια Πολιτείες κυκλώματος, ανατρέξτε στο άρθρο [ροές εργασίας](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Όταν δημιουργείτε ένα νέο κύκλωμα ExpressRoute, το κύκλωμα θα είναι σε κατάσταση παρακάτω:


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Το κύκλωμα θα αλλάξει στην παρακάτω κατάσταση όταν η υπηρεσία παροχής σύνδεσης είναι σε διαδικασία ενεργοποίησή του για να:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Για να μπορέσετε να χρησιμοποιήσετε ένα κύκλωμα ExpressRoute, πρέπει να είναι σε κατάσταση παρακάτω:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. περιοδικά Ελέγξτε την κατάσταση και την κατάσταση του αριθμού-κλειδιού κυκλώματος

Έλεγχος της κατάστασης και η κατάσταση του αριθμού-κλειδιού κυκλώματος σάς επιτρέπει να γνωρίζετε κατά την υπηρεσία παροχής έχει ενεργοποιήσει το κύκλωμα. Μετά το κύκλωμα έχει ρυθμιστεί, *ServiceProviderProvisioningState* εμφανίζεται ως *Provisioned*, όπως φαίνεται στο ακόλουθο παράδειγμα:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Η απόκριση θα είναι παρόμοιο με το ακόλουθο παράδειγμα:


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

### <a name="7-create-your-routing-configuration"></a>7. Δημιουργήστε τις παραμέτρους δρομολόγησης

Για οδηγίες βήμα προς βήμα, ανατρέξτε στο άρθρο [Ρύθμιση παραμέτρων δρομολόγησης κυκλώματος ExpressRoute](expressroute-howto-routing-arm.md) για να δημιουργήσετε και να τροποποιήσετε peerings κυκλώματος.


>[AZURE.IMPORTANT] Αυτές οι οδηγίες ισχύουν μόνο για κυκλώματα που έχουν δημιουργηθεί με υπηρεσίες παροχής που προσφέρουν υπηρεσίες επιπέδου 2 συνδεσιμότητας. Εάν χρησιμοποιείτε μια υπηρεσία παροχής που προσφέρει διαχειριζόμενων επιπέδου 3 υπηρεσιών (συνήθως ένα IP VPN, όπως MPLS), την υπηρεσία παροχής σύνδεσης θα ρυθμίσετε και να διαχειριστείτε τη δρομολόγηση για εσάς.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. σύνδεση ένα εικονικό δίκτυο με ένα κύκλωμα ExpressRoute

Σύνδεση στη συνέχεια, ένα εικονικό δίκτυο με το κύκλωμα ExpressRoute. Χρησιμοποιήστε το άρθρο [Linking εικονικών δικτύων για κυκλώματα ExpressRoute](expressroute-howto-linkvnet-arm.md) κατά την εργασία με το μοντέλο ανάπτυξης διαχείρισης πόρων.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Λήψη της κατάστασης μιας κυκλώματος ExpressRoute

Μπορείτε να ανακτήσετε αυτές τις πληροφορίες ανά πάσα στιγμή, χρησιμοποιώντας το `Get-AzureRmExpressRouteCircuit` cmdlet. Πραγματοποίηση της κλήσης χωρίς παραμέτρους παραθέτει όλα τα κυκλώματα.

    Get-AzureRmExpressRouteCircuit


Η απόκριση θα είναι παρόμοιο με το ακόλουθο παράδειγμα:


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


Μπορείτε να λάβετε πληροφορίες σχετικά με ένα συγκεκριμένο κύκλωμα ExpressRoute, μεταφέροντας το όνομα της ομάδας πόρων και όνομα κυκλώματος ως παράμετρο στην κλήση:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Η απόκριση θα είναι παρόμοιο με το ακόλουθο παράδειγμα:


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


Μπορείτε να λάβετε λεπτομερείς περιγραφές των όλες τις παραμέτρους, εκτελώντας τα εξής:

    get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>Τροποποίηση ενός κυκλώματος ExpressRoute

Μπορείτε να τροποποιήσετε ορισμένες ιδιότητες ενός κυκλώματος ExpressRoute χωρίς να επηρεάζονται συνδεσιμότητας.

Μπορείτε να κάνετε τα εξής με χωρίς χρόνου εκτός λειτουργίας:

- Ενεργοποίηση ή απενεργοποίηση ενός πρόσθετου premium ExpressRoute για το κύκλωμα ExpressRoute.
- Αυξήστε το εύρος ζώνης του σας κυκλώματος ExpressRoute. Σημειώστε ότι το εύρος ζώνης του ένα κύκλωμα υποβάθμιση δεν υποστηρίζεται.
- Αλλάξτε το πρόγραμμα μέτρησης από τα δεδομένα με βάση τη χρήση απεριόριστες δεδομένα. Σημειώστε ότι μέτρησης στο πρόγραμμα από απεριόριστες δεδομένα με τα δεδομένα με βάση τη χρήση δεν υποστηρίζεται η αλλαγή.
-  Μπορείτε να ενεργοποιήσετε και να απενεργοποιήσετε *Επιτρέπουν κλασική λειτουργίες*.

Για περισσότερες πληροφορίες σχετικά με όρια και περιορισμούς, ανατρέξτε στο [ExpressRoute συνήθεις Ερωτήσεις](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>Για να ενεργοποιήσετε το πρόσθετο premium ExpressRoute

Μπορείτε να ενεργοποιήσετε το πρόσθετο premium ExpressRoute για τις υπάρχουσες κύκλωμα χρησιμοποιώντας το παρακάτω τμήμα κώδικα του PowerShell:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Το κύκλωμα θα έχετε τώρα ενεργοποιήσει το ExpressRoute premium πρόσθετες δυνατότητες. Σημειώστε ότι θα σας θα ξεκινήσει χρέωσης για τη δυνατότητα πρόσθετο premium με την επιτυχή εκτέλεση της εντολής.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Για να απενεργοποιήσετε το πρόσθετο premium ExpressRoute

>[AZURE.IMPORTANT] Αυτή η λειτουργία μπορεί να αποτύχει εάν χρησιμοποιείτε τους πόρους που είναι μεγαλύτερα από τι επιτρέπεται για το τυπικό κύκλωμα.

Λάβετε υπόψη τα εξής:

- Πριν να υποβιβάσετε από premium σε τυπική, πρέπει να βεβαιωθείτε ότι ο αριθμός των εικονικού δικτύων που είναι συνδεδεμένες με το κύκλωμα είναι μικρότερο από 10. Εάν δεν μπορείτε να το κάνετε αυτό, αποτυγχάνει αίτησης ενημέρωσης και θα σας χρεώσουμε επιτόκια premium.

- Πρέπει να καταργήσετε τη σύνδεση όλα τα δίκτυα virtual σε άλλες περιοχές γεωπολιτική. Εάν δεν μπορείτε να το κάνετε αυτό, θα αποτύχει η αίτησή σας ενημερωμένη έκδοση και θα σας χρεώσουμε επιτόκια premium.

- Ο πίνακας διαδρομών πρέπει να είναι μικρότερη από 4.000 διαδρομές για διεισδύουν ιδιωτικό. Εάν το μέγεθος του πίνακα δρομολόγηση είναι μεγαλύτερο από 4.000 διαδρομές, η περίοδος λειτουργίας το πρωτόκολλο BGP αποθέτει και δεν θα ενεργοποιηθεί εκ νέου μέχρι τον αριθμό των κοινοποιημένη προθέματα μεταβαίνει μικρότερη από 4.000.

Μπορείτε να απενεργοποιήσετε το πρόσθετο premium ExpressRoute για το υπάρχον κύκλωμα χρησιμοποιώντας το ακόλουθο cmdlet του PowerShell:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Για να ενημερώσετε το εύρος ζώνης κυκλώματος ExpressRoute

Για επιλογές υποστηριζόμενες εύρους ζώνης για την υπηρεσία παροχής, επιλέξτε [ExpressRoute συνήθεις Ερωτήσεις](expressroute-faqs.md). Μπορείτε να επιλέξετε οποιοδήποτε μέγεθος μεγαλύτερο από το μέγεθος του υπάρχοντος κυκλώματος.

>[AZURE.IMPORTANT] Δεν μπορείτε να μειώσετε το εύρος ζώνης του ένα κύκλωμα ExpressRoute χωρίς διακοπή. Υποβάθμιση εύρους ζώνης απαιτεί να deprovision το κύκλωμα ExpressRoute και, στη συνέχεια, reprovision ένα νέο κύκλωμα ExpressRoute.

Αφού αποφασίσετε τι μέγεθος που χρειάζεστε, χρησιμοποιήστε την ακόλουθη εντολή για να αλλάξετε το μέγεθος του κυκλώματος:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Κύκλωμα σας θα είναι μεγέθους προς τα επάνω στην πλευρά της Microsoft. Στη συνέχεια, θα πρέπει να επικοινωνήσετε με την υπηρεσία παροχής σύνδεσης για να ενημερώσετε τις ρυθμίσεις παραμέτρων στην πλευρά τους ώστε να ταιριάζει με αυτήν την αλλαγή. Αφού κάνετε αυτήν την ειδοποίηση, θα σας θα ξεκινήσει που χρεώσεις για την επιλογή ενημερωμένων εύρους ζώνης.


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Για να μετακινήσετε την SKU από με βάση τη χρήση για απεριόριστο

Μπορείτε να αλλάξετε την SKU του ένα κύκλωμα ExpressRoute, χρησιμοποιώντας το παρακάτω τμήμα κώδικα του PowerShell:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Για να ελέγχετε την πρόσβαση σε περιβάλλοντα διαχείρισης πόρων και το κλασικό  

Διαβάστε τις οδηγίες στο θέμα [Μετακίνηση ExpressRoute κυκλώματα από το κλασικό στο μοντέλο ανάπτυξης διαχείρισης πόρων](expressroute-howto-move-arm.md).  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deprovisioning και τη διαγραφή ενός κυκλώματος ExpressRoute

Λάβετε υπόψη τα εξής:

- Πρέπει να καταργήσετε τη σύνδεση όλα τα δίκτυα virtual από το κύκλωμα ExpressRoute. Εάν αυτή η λειτουργία αποτύχει, ελέγξτε εάν οποιοδήποτε εικονικού δίκτυα είναι συνδεδεμένες με το κύκλωμα.

- Εάν η κατάσταση παροχής ExpressRoute κυκλώματος υπηρεσία παροχής που χρησιμοποιείτε είναι **Provisioning** ή **Provisioned** πρέπει να εργαστείτε με την υπηρεσία παροχής για να deprovision το κύκλωμα στην πλευρά τους. Θα συνεχίσουμε να δεσμεύσετε πόρους και γραμματίου που μέχρι να ολοκληρωθεί deprovisioning το κύκλωμα την υπηρεσία παροχής και να ειδοποιεί μας.

- Εάν η υπηρεσία παροχής έχει κατάργηση του κυκλώματος (η κατάσταση παροχής υπηρεσίας παροχής έχει οριστεί σε **μην παρασχεθεί**), στη συνέχεια, μπορείτε να διαγράψετε το κύκλωμα. Αυτό θα σταματήσει χρέωση για το κύκλωμα

Μπορείτε να διαγράψετε το κύκλωμα ExpressRoute, εκτελώντας την ακόλουθη εντολή:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Επόμενα βήματα

Αφού δημιουργήσετε το κύκλωμα, βεβαιωθείτε ότι κάνετε τα εξής:

- [Δημιουργία και τροποποίηση δρομολόγησης για το κύκλωμα ExpressRoute](expressroute-howto-routing-arm.md)
- [Σύνδεση εικονικού δικτύου σας για να σας κυκλώματος ExpressRoute](expressroute-howto-linkvnet-arm.md)
