<properties
   pageTitle="Εφαρμογή μια αρχιτεκτονική δικτύου ιδιαίτερα διαθέσιμο υβριδικό | Microsoft Azure"
   description="Πώς μπορείτε να υλοποιήσετε μια αρχιτεκτονική ασφαλούς τοποθεσίας σε τοποθεσία δικτύου που εκτείνεται σε μια Azure εικονικού δικτύου και ένα δίκτυο εσωτερικής συνδεδεμένη με τη χρήση ExpressRoute με VPN πύλης ανακατεύθυνσης."
   services="guidance,virtual-network,vpn-gateway,expressroute"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>Εφαρμογή μια αρχιτεκτονική δικτύου ιδιαίτερα διαθέσιμο υβριδικό

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Σε αυτό το άρθρο περιγράφει τις βέλτιστες πρακτικές για τη σύνδεση ένα δίκτυο εσωτερικής εγκατάστασης με εικονικών δικτύων σε Azure χρησιμοποιώντας ExpressRoute, με μια τοποθεσία σε τοποθεσία εικονικού ιδιωτικού δικτύου (VPN) ως σύνδεση ανακατεύθυνσης. Η κίνηση ρέει ανάμεσα στο δίκτυο εσωτερικής εγκατάστασης και ένα Azure εικονικού δικτύου (VNet) μέσω μιας σύνδεσης ExpressRoute.  Εάν υπάρχει μια απώλεια σύνδεσης στο κύκλωμα ExpressRoute, κίνηση θα δρομολογούνται μέσω μιας διοχέτευσης VPN ασφαλείας IP.

> [AZURE.NOTE] Azure έχει δύο διαφορετικές ανάπτυξης μοντέλα: [Διαχείριση πόρων] [ resource-manager-overview] και κλασική. Σε αυτό το σχέδιο χρησιμοποιεί διαχείριση πόρων, η οποία η Microsoft συνιστά για νέες αναπτύξεις.

Τυπική χρήση υποθέσεις για αυτήν την αρχιτεκτονική περιλαμβάνουν τα εξής:

- Υβριδική εφαρμογές όπου είναι κατανεμημένες φόρτους εργασίας ανάμεσα σε ένα δίκτυο εσωτερικής εγκατάστασης και Azure.

- Εφαρμογές που εκτελούνται ευρείας κλίμακας, κρίσιμης σημασίας φόρτους εργασίας που απαιτούν μεγάλο βαθμό κλιμάκωση.

- Εγκαταστάσεις μεγάλης κλίμακας δημιουργίας αντιγράφων ασφαλείας και επαναφοράς για τα δεδομένα που πρέπει να είναι αποθηκευμένο εκτός τοποθεσίας.

- Χειρισμός Big Data φόρτους εργασίας.

- Χρήση Azure ως τοποθεσία αποκατάστασης.

Λάβετε υπόψη ότι εάν το κύκλωμα ExpressRoute δεν είναι διαθέσιμη, τη διαδρομή VPN θα μόνο χειρίζεται ιδιωτικές διεισδύουν συνδέσεις. Δημόσια διεισδύουν και Microsoft διεισδύουν συνδέσεις θα περάσουν μέσω του Internet.

## <a name="architecture-diagram"></a>Διάγραμμα αρχιτεκτονικής

>[AZURE.NOTE] Η [Υπηρεσία πύλης VPN Azure] [ azure-vpn-gateway] υλοποιεί δύο τύπους πυλών εικονικού δικτύου; Πύλες εικονικού δικτύου VPN και των πυλών ExpressRoute εικονικού δικτύου. Σε όλο το έγγραφο, τον όρο *Πύλης VPN* αναφέρεται στην υπηρεσία του Azure, ενώ οι φράσεις *πύλη εικονικού δικτύου VPN* και *ExpressRoute πύλης εικονικού δικτύου* που χρησιμοποιούνται για να ανατρέξετε στις υλοποιήσεις του VPN και ExpressRoute της πύλης, αντίστοιχα.

Το παρακάτω διάγραμμα επισημαίνει τα σημαντικά στοιχεία σε αυτήν την αρχιτεκτονική:

> Ένα έγγραφο του Visio που περιλαμβάνει αυτό το διάγραμμα αρχιτεκτονική είναι διαθέσιμο για λήψη στο [Κέντρο λήψης αρχείων της Microsoft]το[visio-download]. Σε αυτό το διάγραμμα είναι στο "υβριδική δίκτυο - ER VPN" σελίδα.

![[0]][0]

- **Azure εικονικών δικτύων (VNets).** Κάθε VNet βρίσκεται σε μία μόνο περιοχή Azure και να φιλοξενήσετε πολλά επίπεδα εφαρμογής. Εφαρμογή βαθμίδες μπορεί να φέρουν κατά διαστήματα χρησιμοποιώντας δευτερεύοντα δίκτυα σε κάθε VNet ή/και δικτύου ομάδες ασφαλείας (NSGs).

- **Εταιρικό δίκτυο εσωτερικής εγκατάστασης.** Αυτό είναι ένα δίκτυο υπολογιστές και συσκευές, συνδεδεμένοι μέσω ενός ιδιωτικού δικτύου τοπικό εκτελούνται μέσα σε μια εταιρεία.

- **Συσκευή VPN.** Αυτή είναι μια συσκευή ή μια υπηρεσία που παρέχει εξωτερική σύνδεση με το δίκτυο εσωτερικής εγκατάστασης. Η συσκευή VPN μπορεί να είναι μια συσκευή υλικού ή μπορεί να είναι μια λύση λογισμικού όπως το δρομολόγησης και απομακρυσμένης πρόσβασης υπηρεσίας (RRAS) σε Windows Server 2012.

> [AZURE.NOTE] Για μια λίστα με τις υποστηριζόμενες συσκευές VPN και πληροφορίες σχετικά με τη ρύθμιση παραμέτρων των επιλεγμένων VPN συσκευές για τη σύνδεση με μια πύλη VPN Azure, ανατρέξτε στις οδηγίες για την κατάλληλη συσκευή από τη [λίστα των συσκευών VPN που υποστηρίζονται από το Azure][vpn-appliance].

- **Πύλη εικονικού δικτύου VPN.** Η πύλη εικονικού δικτύου VPN επιτρέπει την VNet για να συνδεθείτε στη συσκευή VPN στο δίκτυο εσωτερικής εγκατάστασης. Η πύλη εικονικού δικτύου VPN έχει ρυθμιστεί ώστε να δέχεται αιτήσεις από το δίκτυο εσωτερικής εγκατάστασης μόνο μέσω της συσκευής VPN. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [σύνδεση ένα δίκτυο εσωτερικής εγκατάστασης σε δίκτυο εικονικού Microsoft Azure][connect-to-an-Azure-vnet].

- **ExpressRoute πύλη εικονικού δικτύου.** Η πύλη εικονικού δικτύου ExpressRoute επιτρέπει την VNet για να συνδεθείτε με το κύκλωμα ExpressRoute χρησιμοποιείται για τη σύνδεση με το δίκτυο εσωτερικής εγκατάστασης.

- **Η πύλη υποδικτύου.** Οι πύλες εικονικού δικτύου πραγματοποιούνται στο ίδιο υποδίκτυο.

- **Σύνδεση VPN.** Η σύνδεση έχει ιδιότητες που καθορίζουν τον τύπο σύνδεσης (ασφαλείας IP) και τον αριθμό-κλειδί από κοινού με τη συσκευή VPN εσωτερικής εγκατάστασης για να κρυπτογραφήσετε την κυκλοφορία.

- **Κύκλωμα ExpressRoute.** Αυτό είναι ένα επίπεδο 2 ή κυκλώματος επιπέδου 3 που παρέχεται από την υπηρεσία παροχής συνδεσιμότητας αυτό το δίκτυο σύνδεσμοι το εσωτερικής εγκατάστασης με Azure μέσω του άκρου δρομολογητών. Το κύκλωμα χρησιμοποιεί την υποδομή υλικού που ελέγχονται από την υπηρεσία παροχής σύνδεσης.

- **Ν σειρών cloud της εφαρμογής.** Αυτή είναι η εφαρμογή που φιλοξενούνται στο Azure. Αυτό μπορεί να περιλαμβάνει πολλά επίπεδα, με πολλά δευτερεύοντα δίκτυα συνδεδεμένοι μέσω balancers Azure φόρτωση. Η κίνηση σε κάθε υποδίκτυο ενδέχεται να υπόκεινται σε κανόνες που έχουν οριστεί με τη χρήση [Ομάδων ασφαλείας δικτύου Azure][azure-network-security-group](NSGs). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Microsoft Azure ασφαλείας][getting-started-with-azure-security].

## <a name="recommendations"></a>Συστάσεις

Azure προσφέρει πολλές διαφορετικές πόρους και τύπους πόρων, ώστε να μπορεί να είναι αυτή η αναφορά αρχιτεκτονική παρασχεθεί πολλούς διαφορετικούς τρόπους. Παρέχουμε ένα πρότυπο από διαχειριστή πόρων Azure για να εγκαταστήσετε την αρχιτεκτονική αναφοράς που ακολουθεί αυτές τις συστάσεις. Εάν επιλέξετε να δημιουργήσετε το δικό σας αρχιτεκτονική αναφορά θα πρέπει να ακολουθήσετε αυτές τις συστάσεις, εκτός αν έχετε συγκεκριμένες απαιτήσεις που σύσταση υπερβαίνει το μέγεθος.

### <a name="vnet-and-gatewaysubnet"></a>VNet και GatewaySubnet

Δημιουργία της πύλης εικονικού δικτύου ExpressRoute και η πύλη εικονικού δικτύου VPN στο το ίδιο VNet. Αυτό σημαίνει ότι θα πρέπει να μπορούν να μοιράζονται το ίδιο με το όνομα **GatewaySubnet** υποδίκτυο

Εάν το VNet περιλαμβάνει ήδη ένα δευτερεύον δίκτυο με το όνομα **GatewaySubnet**, βεβαιωθείτε ότι έχει μια /27 ή μεγαλύτερο χώρο διευθύνσεων. Εάν το υπάρχον υποδίκτυο είναι πολύ μικρό, στη συνέχεια, καταργήστε την ως εξής και δημιουργήστε ένα νέο όπως φαίνεται στην εικόνα στην επόμενη κουκκίδα:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Εάν το VNet δεν περιέχει ένα δευτερεύον δίκτυο με το όνομα **GatewaySubnet**, στη συνέχεια, δημιουργήστε ένα νέο ως εξής:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>VPN και ExpressRoute πυλών

Βεβαιωθείτε ότι η εταιρεία σας πληροί τις [απαιτήσεις προαπαιτούμενες ExpressRoute] [ expressroute-prereq] για τη σύνδεση με Azure.

Εάν έχετε ήδη μια πύλη εικονικού δικτύου VPN στο σας VNet Azure, καταργήστε την, όπως φαίνεται παρακάτω.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Ακολουθήστε τις οδηγίες στο θέμα [εφαρμογή μια υβριδική αρχιτεκτονική δικτύου με Azure ExpressRoute] [ implementing-expressroute] για να δημιουργήσετε τη σύνδεση ExpressRoute.

Ακολουθήστε τις οδηγίες στο θέμα [εφαρμογή μια υβριδική αρχιτεκτονική δικτύου με Azure και VPN εσωτερικής] [ implementing-vpn] για να δημιουργήσετε τη σύνδεση VPN εικονικού δικτύου πύλης.

Αφού έχετε δημιουργήσει τις συνδέσεις πύλης εικονικού δικτύου, δοκιμάστε το περιβάλλον ως εξής:

1. Βεβαιωθείτε ότι μπορείτε να συνδεθείτε από το δίκτυο εσωτερικής εγκατάστασης με το Azure VNet.

2. Επικοινωνήστε με την υπηρεσία παροχής για να διακόψετε τη σύνδεση ExpressRoute για σκοπούς δοκιμής.

3. Βεβαιωθείτε ότι το μπορείτε να συνδέσετε από το δίκτυο εσωτερικής εγκατάστασης για να σας VNet Azure χρησιμοποιώντας τη σύνδεση VPN εικονικού δικτύου πύλης.

4. Επικοινωνήστε με την υπηρεσία παροχής για να reestabilish ExpressRoute συνδεσιμότητας.

## <a name="considerations"></a>Ζητήματα

Για ζητήματα ExpressRoute, ανατρέξτε στο θέμα [εφαρμογή μια υβριδική αρχιτεκτονική δικτύου με Azure ExpressRoute] [ guidance-expressroute] οδηγίες.

Για ζητήματα VPN-τοποθεσίας, ανατρέξτε την [υλοποίηση μια υβριδική αρχιτεκτονική δικτύου με Azure και VPN εσωτερικής] [ guidance-vpn] οδηγίες.

Για ζητήματα γενικά Azure ασφαλείας, ανατρέξτε στο θέμα [υπηρεσίες cloud της Microsoft και ασφάλεια δικτύου][best-practices-security].

## <a name="solution-deployment"></a>Ανάπτυξη της λύσης

Εάν έχετε μια υπάρχουσα υποδομή εσωτερικής εγκατάστασης που ήδη έχει ρυθμιστεί με μια συσκευή κατάλληλο δίκτυο, μπορείτε να αναπτύξετε την αρχιτεκτονική αναφοράς, ακολουθώντας τα παρακάτω βήματα:

1. Κάντε δεξί κλικ στο κουμπί παρακάτω και επιλέξτε είτε "Άνοιγμα σύνδεσης σε νέα καρτέλα" ή "Άνοιγμα σύνδεσης σε νέο παράθυρο":  
[![Ανάπτυξη Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. Αναμονή για τη σύνδεση για να ανοίξετε στην πύλη του Azure και, στη συνέχεια, ακολουθήστε τα παρακάτω βήματα: 
  - Το όνομα της **ομάδας πόρων** ορίζεται ήδη στο αρχείο παραμέτρων, επομένως, επιλέξτε **Δημιουργία νέου** και πληκτρολογήστε `ra-hybrid-vpn-er-rg` στο πλαίσιο κειμένου.
  - Επιλέξτε την περιοχή από τη **θέση** αναπτυσσόμενο πλαίσιο.
  - Επεξεργαστείτε το **Πρότυπο ριζικό Uri** ή πλαίσια κειμένου **Uri ριζικού παραμέτρων** .
  - Εξετάστε τους όρους και τις προϋποθέσεις και, στη συνέχεια, επιλέξτε το πλαίσιο ελέγχου **αποδέχομαι τους όρους και προϋποθέσεις που αναφέρονται παραπάνω** .
  - Κάντε κλικ στο κουμπί **αγοράς** .

3. Περιμένετε για την ανάπτυξη για να ολοκληρωθεί.

4. Κάντε δεξί κλικ στο κουμπί παρακάτω και επιλέξτε είτε "Άνοιγμα σύνδεσης σε νέα καρτέλα" ή "Άνοιγμα σύνδεσης σε νέο παράθυρο":  
[![Ανάπτυξη Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. Αναμονή για τη σύνδεση για να ανοίξετε στην πύλη του Azure, στη συνέχεια, πληκτρολογήστε, στη συνέχεια, ακολουθήστε τα παρακάτω βήματα: 
  - Επιλέξτε **Χρήση υπάρχοντος** στην ενότητα **ομάδα πόρων** και εισαγάγετε `ra-hybrid-vpn-er-rg` στο πλαίσιο κειμένου.
  - Επιλέξτε την περιοχή από τη **θέση** αναπτυσσόμενο πλαίσιο.
  - Επεξεργαστείτε το **Πρότυπο ριζικό Uri** ή πλαίσια κειμένου **Uri ριζικού παραμέτρων** .
  - Εξετάστε τους όρους και τις προϋποθέσεις και, στη συνέχεια, επιλέξτε το πλαίσιο ελέγχου **αποδέχομαι τους όρους και προϋποθέσεις που αναφέρονται παραπάνω** .
  - Κάντε κλικ στο κουμπί **αγοράς** .

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "Αρχιτεκτονική της μια αρχιτεκτονική δικτύου ιδιαίτερα διαθέσιμο υβριδικό χρησιμοποιώντας ExpressRoute και VPN πύλης"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
