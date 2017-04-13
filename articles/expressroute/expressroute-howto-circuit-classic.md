<properties
   pageTitle="Δημιουργία και τροποποίηση ενός κυκλώματος ExpressRoute χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης και PowerShell | Microsoft Azure"
   description="Σε αυτό το άρθρο σάς καθοδηγεί στα βήματα για τη δημιουργία και την προμήθεια ένα κύκλωμα ExpressRoute. Αυτό το άρθρο σας δείχνει πώς μπορείτε να ελέγξετε την κατάσταση, ενημέρωση ή διαγραφή και deprovision κυκλώματος σας."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>Δημιουργία και τροποποίηση ενός κυκλώματος ExpressRoute

> [AZURE.SELECTOR]
[Azure πύλης - διαχείριση πόρων](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShell - διαχείριση πόρων](expressroute-howto-circuit-arm.md)
[PowerShell - κλασικό](expressroute-howto-circuit-classic.md)

Σε αυτό το άρθρο σάς καθοδηγεί σε τα βήματα για να δημιουργήσετε ένα κύκλωμα Azure ExpressRoute με τη χρήση των cmdlet του PowerShell και το μοντέλο κλασική ανάπτυξης. Σε αυτό το άρθρο θα εμφανιστούν και πώς μπορείτε να ελέγξετε την κατάσταση, ενημέρωση ή διαγραφή και deprovision ένα κύκλωμα ExpressRoute.

**Σχετικά με τα μοντέλα Azure ανάπτυξης**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>Πριν ξεκινήσετε

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1. εξετάστε τις προϋποθέσεις και άρθρα ροής εργασίας

Βεβαιωθείτε ότι ελέγξετε οι [προϋποθέσεις](expressroute-prerequisites.md) και οι [ροές εργασίας](expressroute-workflows.md) πριν να ξεκινήσετε τη ρύθμιση των παραμέτρων.  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2. Εγκαταστήστε τις πιο πρόσφατες εκδόσεις των λειτουργικών μονάδων Azure PowerShell 

Ακολουθήστε τις οδηγίες στο [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για αναλυτικές οδηγίες σχετικά με τον τρόπο για να ρυθμίσετε τον υπολογιστή σας για να χρησιμοποιήσετε τις λειτουργικές μονάδες Azure PowerShell.

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. συνδεθείτε στο λογαριασμό σας στο Azure και επιλέξτε μια συνδρομή

1. Εκτελέστε το ακόλουθο cmdlet χρησιμοποιώντας μια αναβαθμισμένη εντολών του Windows PowerShell:

        Add-AzureAccount
2. Το σύμβολο στην οθόνη που εμφανίζεται, πραγματοποιήστε είσοδο στο λογαριασμό σας.

3. Λάβετε μια λίστα με όλες τις συνδρομές σας.

        Get-AzureSubscription
4. Επιλέξτε τη συνδρομή στην οποία θέλετε να χρησιμοποιήσετε.
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>Δημιουργία και παροχή ένα κύκλωμα ExpressRoute

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1. εισαγάγετε τις λειτουργικές μονάδες του PowerShell για ExpressRoute

 Εάν δεν το έχετε κάνει ήδη, πρέπει να εισαγάγετε τις λειτουργικές μονάδες Azure και ExpressRoute στην περίοδο λειτουργίας PowerShell για να ξεκινήσετε να χρησιμοποιείτε τα cmdlet ExpressRoute. Μπορείτε να εισαγάγετε τις λειτουργικές μονάδες από τη θέση στην οποία εγκαταστάθηκαν να στον τοπικό σας υπολογιστή. Ανάλογα με τη μέθοδο που χρησιμοποιήσατε για να εγκαταστήσετε τις λειτουργικές μονάδες, ενδέχεται να διαφέρουν από το παρακάτω παράδειγμα εμφανίζει τη θέση. Τροποποιήστε το παράδειγμα, εάν είναι απαραίτητο.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. γρήγορα τη λίστα των υποστηριζόμενες υπηρεσίες παροχής, θέσεις και εύρος ζώνης

Πριν να δημιουργήσετε ένα κύκλωμα ExpressRoute, χρειάζεστε τη λίστα των υπηρεσιών παροχής υποστηριζόμενες συνδεσιμότητας, θέσεις και επιλογές εύρους ζώνης.

Το cmdlet του PowerShell `Get-AzureDedicatedCircuitServiceProvider` επιστρέφει οι πληροφορίες που θα χρησιμοποιήσετε στα επόμενα βήματα:

    Get-AzureDedicatedCircuitServiceProvider

Ελέγξτε εάν η υπηρεσία παροχής συνδεσιμότητας αναγράφεται εκεί. Σημειώστε τις ακόλουθες πληροφορίες επειδή που θα το χρειαστείτε αργότερα όταν δημιουργείτε ένα κύκλωμα:

- Όνομα

- PeeringLocations

- BandwidthsOffered

Τώρα είστε έτοιμοι να δημιουργήσετε ένα κύκλωμα ExpressRoute.         

### <a name="3-create-an-expressroute-circuit"></a>3. Δημιουργήστε ένα κύκλωμα ExpressRoute

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο δημιουργίας ενός Mbps 200 ExpressRoute κύκλωμα μέσω Equinix στο κοίλο πυριτίου. Εάν χρησιμοποιείτε μια διαφορετική υπηρεσία παροχής και διαφορετικές ρυθμίσεις, αντικαταστήστε αυτές τις πληροφορίες όταν κάνετε την αίτησή σας.

>[AZURE.IMPORTANT] Το κύκλωμα ExpressRoute θα χρεώνεστε από τη στιγμή που έχει εκδοθεί από έναν αριθμό-κλειδί υπηρεσίας. Βεβαιωθείτε ότι μπορείτε να εκτελέσετε αυτήν τη λειτουργία όταν είστε έτοιμοι να παρέχετε το κύκλωμα στην υπηρεσία παροχής σύνδεσης.


Ακολουθεί ένα παράδειγμα αίτηση για ένα νέο αριθμό-κλειδί υπηρεσίας:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Εναλλακτικά, εάν θέλετε να δημιουργήσετε ένα κύκλωμα ExpressRoute με το πρόσθετο premium, χρησιμοποιήστε το παρακάτω παράδειγμα. Ανατρέξτε [Στις συνήθεις Ερωτήσεις ExpressRoute](expressroute-faqs.md) για περισσότερες λεπτομέρειες σχετικά με το πρόσθετο premium.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Η απόκριση θα περιέχει το κλειδί υπηρεσίας. Μπορείτε να λάβετε λεπτομερείς περιγραφές των όλες τις παραμέτρους, εκτελώντας τα εξής:

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4. λίστα όλα τα κυκλώματα ExpressRoute

Μπορείτε να εκτελέσετε το `Get-AzureDedicatedCircuit` εντολή για να λάβετε μια λίστα με όλα τα κυκλώματα ExpressRoute που δημιουργήσατε:


    Get-AzureDedicatedCircuit

Η απόκριση θα είναι κάτι παρόμοιο με το ακόλουθο παράδειγμα:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Μπορείτε να ανακτήσετε αυτές τις πληροφορίες ανά πάσα στιγμή, χρησιμοποιώντας το `Get-AzureDedicatedCircuit` cmdlet. Πραγματοποίηση της κλήσης χωρίς παραμέτρους παραθέτει όλα τα κυκλώματα. Τον αριθμό-κλειδί υπηρεσία θα εμφανίζεται στο πεδίο *ServiceKey* .

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Μπορείτε να λάβετε λεπτομερείς περιγραφές των όλες τις παραμέτρους, εκτελώντας τα εξής:

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. στείλτε το κλειδί υπηρεσίας στην υπηρεσία παροχής σύνδεσης για παροχή


*ServiceProviderProvisioningState* παρέχει πληροφορίες σχετικά με την τρέχουσα κατάσταση της προμήθειας στην υπηρεσία παροχής πλευρά. *Κατάσταση* παρέχει την κατάσταση στην πλευρά της Microsoft. Για περισσότερες πληροφορίες σχετικά με την προμήθεια Πολιτείες κυκλώματος, ανατρέξτε στο άρθρο [ροές εργασίας](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Όταν δημιουργείτε ένα νέο κύκλωμα ExpressRoute, το κύκλωμα θα είναι σε κατάσταση παρακάτω:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Το κύκλωμα θα μεταβείτε στην ακόλουθη κατάσταση όταν η υπηρεσία παροχής σύνδεσης είναι σε διαδικασία ενεργοποίησή του για να:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Ένα κύκλωμα ExpressRoute πρέπει να είναι σε κατάσταση για να μπορέσετε να χρησιμοποιήσετε την παρακάτω:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. περιοδικά Ελέγξτε την κατάσταση και την κατάσταση του αριθμού-κλειδιού κυκλώματος

Αυτό σας επιτρέπει να γνωρίζετε κατά την υπηρεσία παροχής έχει ενεργοποιήσει το κύκλωμα. Αφού το κύκλωμα έχει ρυθμιστεί, *ServiceProviderProvisioningState* θα εμφανίζονται ως *Provisioned* , όπως φαίνεται στο ακόλουθο παράδειγμα:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7. Δημιουργήστε τις παραμέτρους δρομολόγησης

Αναφορά σε το [ExpressRoute κυκλώματος παραμέτρους δρομολόγησης (δημιουργία και τροποποίηση peerings κυκλώματος)](expressroute-howto-routing-classic.md) το άρθρο για οδηγίες βήμα προς βήμα.

>[AZURE.IMPORTANT] Αυτές οι οδηγίες ισχύουν μόνο για κυκλώματα που έχουν δημιουργηθεί με υπηρεσίες παροχής που προσφέρουν υπηρεσίες επιπέδου 2 συνδεσιμότητας. Εάν χρησιμοποιείτε μια υπηρεσία παροχής που προσφέρει διαχειριζόμενων επιπέδου 3 υπηρεσιών (συνήθως ένα IP VPN, όπως MPLS), την υπηρεσία παροχής σύνδεσης θα ρυθμίσετε και να διαχειριστείτε τη δρομολόγηση για εσάς.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. σύνδεση ένα εικονικό δίκτυο με ένα κύκλωμα ExpressRoute

Σύνδεση στη συνέχεια, ένα εικονικό δίκτυο με το κύκλωμα ExpressRoute. Ανατρέξτε [κυκλώματα σύνδεση ExpressRoute για εικονικών δικτύων](expressroute-howto-linkvnet-classic.md) για οδηγίες βήμα προς βήμα. Εάν χρειάζεστε για να δημιουργήσετε ένα εικονικό δίκτυο με τη χρήση του μοντέλου κλασική ανάπτυξης για ExpressRoute, ανατρέξτε στο θέμα [Δημιουργία εικονικού δικτύου για ExpressRoute](expressroute-howto-vnet-portal-classic.md) για οδηγίες.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Λήψη της κατάστασης μιας κυκλώματος ExpressRoute

Μπορείτε να ανακτήσετε αυτές τις πληροφορίες ανά πάσα στιγμή, χρησιμοποιώντας το `Get-AzureCircuit` cmdlet. Πραγματοποίηση της κλήσης χωρίς παραμέτρους παραθέτει όλα τα κυκλώματα.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Μπορείτε να λάβετε πληροφορίες σχετικά με ένα συγκεκριμένο κύκλωμα ExpressRoute, μεταφέροντας το κλειδί υπηρεσίας ως παράμετρο στην κλήση:

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Μπορείτε να λάβετε λεπτομερείς περιγραφές των όλες τις παραμέτρους, εκτελώντας τα εξής:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Τροποποίηση ενός κυκλώματος ExpressRoute

Μπορείτε να τροποποιήσετε ορισμένες ιδιότητες ενός κυκλώματος ExpressRoute χωρίς να επηρεάζονται συνδεσιμότητας.

Μπορείτε να κάνετε τα εξής με χωρίς χρόνου εκτός λειτουργίας:

- Ενεργοποίηση ή απενεργοποίηση ενός πρόσθετου premium ExpressRoute για το κύκλωμα ExpressRoute.
- Αυξήστε το εύρος ζώνης του σας κυκλώματος ExpressRoute. Σημειώστε ότι το εύρος ζώνης του ένα κύκλωμα υποβάθμιση δεν υποστηρίζεται.
- Αλλάξτε το πρόγραμμα μέτρησης από τα δεδομένα με βάση τη χρήση απεριόριστες δεδομένα. Σημειώστε ότι μέτρησης στο πρόγραμμα από απεριόριστες δεδομένα με τα δεδομένα με βάση τη χρήση δεν υποστηρίζεται η αλλαγή.
- Μπορείτε να ενεργοποιήσετε και να απενεργοποιήσετε *Επιτρέπουν κλασική λειτουργίες*.

Ανατρέξτε [Στις συνήθεις Ερωτήσεις ExpressRoute](expressroute-faqs.md) για περισσότερες πληροφορίες σχετικά με όρια και περιορισμοί.

### <a name="to-enable-the-expressroute-premium-add-on"></a>Για να ενεργοποιήσετε το πρόσθετο premium ExpressRoute

Μπορείτε να ενεργοποιήσετε το πρόσθετο premium ExpressRoute για τις υπάρχουσες κύκλωμα χρησιμοποιώντας το ακόλουθο cmdlet του PowerShell:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Κύκλωμα σας θα έχουν τώρα ενεργοποιήσει το ExpressRoute premium πρόσθετες δυνατότητες. Σημειώστε ότι θα ξεκινήσουμε χρέωσης για τη δυνατότητα πρόσθετο premium με την επιτυχή εκτέλεση της εντολής.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Για να απενεργοποιήσετε το πρόσθετο premium ExpressRoute

>[AZURE.IMPORTANT] Αυτή η λειτουργία μπορεί να αποτύχει εάν χρησιμοποιείτε τους πόρους που είναι μεγαλύτερα από τι επιτρέπεται για το τυπικό κύκλωμα.

Λάβετε υπόψη τα εξής:

- Πρέπει να βεβαιωθείτε ότι ο αριθμός των εικονικών δικτύων συνδεδεμένο με το κύκλωμα είναι μικρότερο από 10 πριν να υποβιβάσετε από premium σε τυπική. Εάν δεν μπορείτε να το κάνετε αυτό, θα αποτύχει η αίτησή σας ενημερωμένη έκδοση και θα χρεωθεί τις χρεώσεις premium.

- Πρέπει να καταργήσετε τη σύνδεση όλα τα δίκτυα virtual σε άλλες περιοχές γεωπολιτική. Εάν δεν μπορείτε να το κάνετε αυτό, θα αποτύχει η αίτησή σας ενημερωμένη έκδοση και θα χρεωθεί τις χρεώσεις premium.

- Ο πίνακας διαδρομών πρέπει να είναι μικρότερη από 4.000 διαδρομές για διεισδύουν ιδιωτικό. Εάν το μέγεθος του πίνακα δρομολόγηση είναι μεγαλύτερο από 4.000 διαδρομές, η περίοδος λειτουργίας το πρωτόκολλο BGP θα αποθέστε και δεν θα ενεργοποιηθεί εκ νέου μέχρι τον αριθμό των κοινοποιημένη προθέματα μεταβαίνει μικρότερη από 4.000.

Μπορείτε να απενεργοποιήσετε το πρόσθετο premium ExpressRoute για τις υπάρχουσες κύκλωμα χρησιμοποιώντας το ακόλουθο cmdlet του PowerShell:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Για να ενημερώσετε το εύρος ζώνης κυκλώματος ExpressRoute

Ελέγξτε το [ExpressRoute συνήθεις Ερωτήσεις](expressroute-faqs.md) για επιλογές υποστηριζόμενες εύρους ζώνης για την υπηρεσία παροχής. Μπορείτε να επιλέξετε οποιοδήποτε μέγεθος που είναι μεγαλύτερη από το μέγεθος του υπάρχοντος κυκλώματος εφόσον επιτρέπει η φυσική θύρα (κατά την οποία δημιουργήθηκε το κύκλωμα).

>[AZURE.IMPORTANT] Δεν μπορείτε να μειώσετε το εύρος ζώνης του ένα κύκλωμα ExpressRoute χωρίς διακοπή. Υποβάθμιση εύρους ζώνης θα απαιτούν να deprovision το κύκλωμα ExpressRoute και, στη συνέχεια, reprovision ένα νέο κύκλωμα ExpressRoute.

Αφού αποφασίσετε τι μέγεθος που χρειάζεστε, μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή για να αλλάξετε το μέγεθος του κυκλώματος:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Κύκλωμα σας θα έχουν έχουν μεγέθους προς τα επάνω στην πλευρά της Microsoft. Θα πρέπει να επικοινωνήσετε με την υπηρεσία παροχής σύνδεσης για να ενημερώσετε τις ρυθμίσεις παραμέτρων στην πλευρά τους ώστε να ταιριάζει με αυτήν την αλλαγή. Σημειώστε ότι θα ξεκινήσουμε χρεώσεις για την επιλογή ενημερωμένων εύρους ζώνης από αυτό το σημείο στο.

Εάν δείτε το ακόλουθο σφάλμα κατά την αυξάνοντας το εύρος ζώνης κυκλώματος, αυτό σημαίνει ότι υπάρχουν στη θύρα φυσικής όπου έχει δημιουργηθεί το υπάρχον κυκλώματος απομένει δεν υπάρχει επαρκές εύρος ζώνης. Πρέπει να διαγράψετε αυτό το κύκλωμα και να δημιουργήσετε ένα νέο κύκλωμα το μέγεθος που θέλετε. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deprovisioning και τη διαγραφή ενός κυκλώματος ExpressRoute

Λάβετε υπόψη τα εξής:

- Πρέπει να καταργήσετε τη σύνδεση όλα τα δίκτυα εικονικού από το κύκλωμα ExpressRoute για αυτήν τη λειτουργία με επιτυχία. Ελέγξτε για να δείτε εάν έχετε οποιονδήποτε εικονικό δίκτυα που συνδέονται με το κύκλωμα εάν αποτύχει αυτή η λειτουργία.

- Εάν η κατάσταση παροχής ExpressRoute κυκλώματος υπηρεσία παροχής που χρησιμοποιείτε είναι **Provisioning** ή **Provisioned** πρέπει να εργαστείτε με την υπηρεσία παροχής για να deprovision το κύκλωμα στην πλευρά τους. Θα συνεχίσουμε να δεσμεύσετε πόρους και γραμματίου που μέχρι να ολοκληρωθεί deprovisioning το κύκλωμα την υπηρεσία παροχής και να ειδοποιεί μας.

- Εάν η υπηρεσία παροχής έχει κατάργηση του κυκλώματος (η κατάσταση παροχής υπηρεσίας παροχής έχει οριστεί σε **μην παρασχεθεί**), στη συνέχεια, μπορείτε να διαγράψετε το κύκλωμα. Αυτό θα σταματήσει χρέωση για το κύκλωμα.

Μπορείτε να διαγράψετε το κύκλωμα ExpressRoute, εκτελώντας την ακόλουθη εντολή:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Επόμενα βήματα

Αφού δημιουργήσετε το κύκλωμα, βεβαιωθείτε ότι κάνετε τα εξής:

- [Δημιουργία και τροποποίηση δρομολόγησης για το κύκλωμα ExpressRoute](expressroute-howto-routing-classic.md)
- [Σύνδεση εικονικού δικτύου σας για να σας κυκλώματος ExpressRoute](expressroute-howto-linkvnet-classic.md)
