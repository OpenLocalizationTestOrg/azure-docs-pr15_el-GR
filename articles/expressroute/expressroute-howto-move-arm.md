<properties
   pageTitle="Μετακίνηση κυκλώματα ExpressRoute από κλασική διαχείριση πόρων για να | Microsoft Azure"
   description="Αυτή η σελίδα περιγράφει πώς μπορείτε να μετακινήσετε ένα κλασική κύκλωμα στο μοντέλο ανάπτυξης διαχείρισης πόρων."
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


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>Μετακίνηση κυκλώματα ExpressRoute από το κλασικό στο μοντέλο ανάπτυξης για τη διαχείριση πόρων

## <a name="configuration-prerequisites"></a>Προαπαιτούμενα στοιχεία ρύθμισης παραμέτρων

- Χρειάζεστε την πιο πρόσφατη έκδοση του PowerShell Azure λειτουργικές μονάδες (τουλάχιστον έκδοση 1.0).
- Βεβαιωθείτε ότι ελέγξετε τα [προαπαιτούμενα στοιχεία για τις](expressroute-prerequisites.md) [απαιτήσεις για τη δρομολόγηση](expressroute-routing.md)και [ροές εργασίας](expressroute-workflows.md) πριν να ξεκινήσετε τη ρύθμιση των παραμέτρων.
- Πριν από την προηγούμενη περαιτέρω, διαβάστε πληροφορίες που παρέχεται στην ενότητα [Μετακίνηση ένα κύκλωμα ExpressRoute από κλασική για τη διαχείριση πόρων](expressroute-move.md). Βεβαιωθείτε ότι έχετε έχουν πλήρως κατανοητή τα όρια και περιορισμοί του τι είναι δυνατή.
- Εάν θέλετε να μετακινήσετε ένα κύκλωμα Azure ExpressRoute από το μοντέλο κλασική ανάπτυξης στο μοντέλο ανάπτυξης διαχείρισης πόρων Azure, πρέπει να έχετε το κύκλωμα πλήρως ρυθμισμένη και λειτουργικές στο μοντέλο κλασική ανάπτυξης.
- Βεβαιωθείτε ότι έχετε μια ομάδα πόρων που δημιουργήθηκε στο μοντέλο ανάπτυξης διαχείρισης πόρων.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Μετακινήστε το κύκλωμα ExpressRoute στο μοντέλο ανάπτυξης για τη διαχείριση πόρων

Πρέπει να μετακινήσετε ένα κύκλωμα ExpressRoute στο μοντέλο ανάπτυξης για τη διαχείριση πόρων, έτσι ώστε να μπορείτε να τη χρησιμοποιήσετε σε το κλασικό και τα μοντέλα ανάπτυξης διαχείρισης πόρων. Μπορείτε να το κάνετε αυτό, εκτελώντας τις παρακάτω εντολές του PowerShell.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Βήμα 1: Συλλογή κυκλώματος λεπτομέρειες από το μοντέλο κλασική ανάπτυξης

Πρέπει να πρώτα να συγκεντρώσετε πληροφορίες σχετικά με το κύκλωμα ExpressRoute.

Είσοδος στο περιβάλλον Azure κλασική και συγκέντρωση το κλειδί υπηρεσίας. Μπορείτε να χρησιμοποιήσετε το παρακάτω τμήμα κώδικα του PowerShell για τη συγκέντρωση των πληροφοριών:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

Αντιγράψτε το **κλειδί της υπηρεσίας** του κυκλώματος που θέλετε να μετακινήσετε επάνω από το μοντέλο ανάπτυξης διαχείρισης πόρων.

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Βήμα 2: Πραγματοποιήστε είσοδο στο περιβάλλον διαχείρισης πόρων και δημιουργήστε μια νέα ομάδα πόρων

Μπορείτε να δημιουργήσετε μια νέα ομάδα πόρων, χρησιμοποιώντας το παρακάτω τμήμα κώδικα:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

Μπορείτε επίσης να χρησιμοποιήσετε μια υπάρχουσα ομάδα πόρων, εάν έχετε ήδη ένα.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Βήμα 3: Μετακινήστε το κύκλωμα ExpressRoute στο μοντέλο ανάπτυξης για τη διαχείριση πόρων

Τώρα είστε έτοιμοι να μετακινήσετε επάνω από το κύκλωμα ExpressRoute από το κλασικό στο μοντέλο ανάπτυξης διαχείρισης πόρων. Ελέγξτε τις πληροφορίες που παρέχεται στην ενότητα [Μετακίνηση ένα κύκλωμα ExpressRoute από το κλασικό στο μοντέλο ανάπτυξης διαχείρισης πόρων](expressroute-move.md) πριν να προχωρήσετε.

Μπορείτε να το κάνετε αυτό, εκτελώντας το παρακάτω τμήμα κώδικα:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Αφού ολοκληρωθεί η μετακίνηση, το νέο όνομα που εμφανίζεται στο προηγούμενο το cmdlet θα χρησιμοποιηθεί για την αντιμετώπιση του πόρου. Το κύκλωμα ουσιαστικά θα μετονομαστεί.

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>Ενεργοποίηση ενός κυκλώματος ExpressRoute για δύο μοντέλα ανάπτυξης

Πρέπει να μετακινήσετε το κύκλωμα ExpressRoute στο μοντέλο ανάπτυξης διαχείρισης πόρων πριν από τον έλεγχο της πρόσβασης στο μοντέλο ανάπτυξης.

Εκτελέστε το ακόλουθο cmdlet για να ενεργοποιήσετε την πρόσβαση σε δύο μοντέλα ανάπτυξη:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Αφού αυτή η λειτουργία ολοκληρώθηκε με επιτυχία, θα μπορούν να δουν το κύκλωμα στο μοντέλο κλασική ανάπτυξης.

Εκτελέστε τα εξής για να λάβετε τις λεπτομέρειες του κυκλώματος ExpressRoute:

    get-azurededicatedcircuit

Πρέπει να μπορούν να δουν το κλειδί υπηρεσίας που παρατίθενται. Τώρα, μπορείτε να διαχειριστείτε συνδέσεις προς το κύκλωμα ExpressRoute χρησιμοποιώντας την τυπική κλασική ανάπτυξης μοντέλο για κλασική VNets και σας τυπική ARM εντολές για ARM VNETs. Τα ακόλουθα άρθρα θα σας καθοδηγήσει πώς μπορείτε να διαχειριστείτε συνδέσεις προς το κύκλωμα ExpressRoute:

- [Σύνδεση εικονικού δικτύου σας για να σας κυκλώματος ExpressRoute στο μοντέλο ανάπτυξης για τη διαχείριση πόρων](expressroute-howto-linkvnet-arm.md)
- [Σύνδεση εικονικού δικτύου σας για να σας κυκλώματος ExpressRoute στο μοντέλο κλασική ανάπτυξης](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>Απενεργοποίηση του κυκλώματος ExpressRoute στο μοντέλο κλασική ανάπτυξης

Εκτελέστε το ακόλουθο cmdlet για να απενεργοποιήσετε την πρόσβαση στο μοντέλο κλασική ανάπτυξη:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Αφού αυτή η λειτουργία ολοκληρώθηκε με επιτυχία, δεν θα μπορούν να δουν το κύκλωμα στο μοντέλο κλασική ανάπτυξης.

## <a name="next-steps"></a>Επόμενα βήματα

Αφού δημιουργήσετε το κύκλωμα, βεβαιωθείτε ότι κάνετε τα εξής:

- [Δημιουργία και τροποποίηση δρομολόγησης για το κύκλωμα ExpressRoute](expressroute-howto-routing-arm.md)
- [Σύνδεση εικονικού δικτύου σας για να σας κυκλώματος ExpressRoute](expressroute-howto-linkvnet-arm.md)
