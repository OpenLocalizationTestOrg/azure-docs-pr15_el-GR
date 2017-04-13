<properties 
   pageTitle="Οδηγός αντιμετώπιση προβλημάτων ExpressRoute - γρήγορα ARP πίνακες | Microsoft Azure"
   description="Αυτή η σελίδα παρέχει οδηγίες σχετικά με γρήγορα το ARP πίνακες για ένα κύκλωμα ExpressRoute"
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="ganesr"/>

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>Αντιμετώπιση προβλημάτων ExpressRoute Οδηγό - γρήγορα ARP πίνακες στο μοντέλο ανάπτυξης για τη διαχείριση πόρων

> [AZURE.SELECTOR]
[PowerShell - διαχείριση πόρων](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - κλασικό](expressroute-troubleshooting-arp-classic.md)

Σε αυτό το άρθρο σάς καθοδηγεί σε τα βήματα για να μάθετε τους πίνακες ARP για το κύκλωμα ExpressRoute. 

>[AZURE.IMPORTANT] Αυτό το έγγραφο προορίζεται για να σας βοηθήσει να διάγνωση και διόρθωση προβλημάτων απλό. Δεν προορίζεται για να αντικαθιστά υποστήριξης της Microsoft. Πρέπει να ανοίξετε ένα δελτίο υποστήριξης με [υποστήριξης της Microsoft,](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Εάν δεν μπορείτε να επιλύσετε το πρόβλημα χρησιμοποιώντας τις οδηγίες που περιγράφεται παρακάτω.

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Διεύθυνση επίλυσης Protocol (ARP) και ARP πίνακες
Πρωτόκολλο επίλυσης διευθύνσεων (ARP) είναι ένα πρωτόκολλο επιπέδου 2 που ορίζεται στο [RFC 826](https://tools.ietf.org/html/rfc826). ARP χρησιμοποιείται για να αντιστοιχίσετε τη διεύθυνση Ethernet (διεύθυνση MAC) με μια διεύθυνση ip.

Ο πίνακας ARP παρέχει μια αντιστοίχιση της διεύθυνσης ipv4 και διεύθυνση MAC για ένα συγκεκριμένο διεισδύουν. Στον πίνακα ARP για μια διεισδύουν κυκλώματος ExpressRoute παρέχει τις ακόλουθες πληροφορίες για κάθε διασύνδεση (κύριας και δευτερεύουσας)

1. Αντιστοίχιση της διεύθυνσης ip του εσωτερικής δρομολογητή περιβάλλοντος εργασίας στη διεύθυνση MAC
2. Αντιστοίχιση της διεύθυνσης ip του ExpressRoute δρομολογητή περιβάλλοντος εργασίας στη διεύθυνση MAC
3. Ηλικία της αντιστοίχισης

ARP πίνακες μπορούν να σας βοηθήσουν να επικυρώσετε ρύθμιση παραμέτρων επιπέδου 2 και αντιμετώπιση προβλημάτων βασικές επιπέδου 2 ζητήματα συνδεσιμότητας. 

Παράδειγμα πίνακα ARP: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Στην παρακάτω ενότητα παρέχει πληροφορίες για το πώς μπορείτε να προβάλετε τους πίνακες ARP ορατό από τους δρομολογητές άκρη ExpressRoute. 

## <a name="prerequisites-for-learning-arp-tables"></a>Προαπαιτούμενα στοιχεία για την εκμάθηση ARP πίνακες

Βεβαιωθείτε ότι έχετε τα ακόλουθα πριν από την περαιτέρω προόδου

 - Ένα έγκυρο ExpressRoute κύκλωμα έχουν ρυθμιστεί με τουλάχιστον μία διεισδύουν. Το κύκλωμα πρέπει να ρυθμιστεί πλήρως από την υπηρεσία παροχής σύνδεσης. Εσείς (ή την υπηρεσία παροχής συνδεσιμότητας) πρέπει να έχετε ρυθμίσει τουλάχιστον ένα από τα peerings (Azure ιδιωτικές, Azure δημόσιο και Microsoft) σε αυτό το κύκλωμα.
 - Περιοχές διευθύνσεων IP που χρησιμοποιούνται για τη ρύθμιση παραμέτρων του peerings (Azure ιδιωτικές, Azure δημόσιο και Microsoft). Αναθεωρήστε τα παραδείγματα ανάθεσης διεύθυνση ip στη [σελίδα δρομολόγησης απαιτήσεις ExpressRoute](expressroute-routing.md) για να κατανοήσετε τον τρόπο περιβάλλοντα εργασίας στη δική σας πλευρά και στην πλευρά ExpressRoute αντιστοίχισης διευθύνσεων ip. Μπορείτε να λάβετε πληροφορίες σχετικά με τη ρύθμιση παραμέτρων peering ανατρέχοντας στη [σελίδα Ρύθμιση παραμέτρων διεισδύουν ExpressRoute](expressroute-howto-routing-arm.md).
 - Πληροφορίες από την ομάδα σας κοινωνικής δικτύωσης / παροχής συνδεσιμότητας στις διευθύνσεις MAC των διασυνδέσεων χρησιμοποιείται με αυτές τις διευθύνσεις IP.
 - Πρέπει να έχετε την πιο πρόσφατη λειτουργικής μονάδας PowerShell για Azure (1,50 ή νεότερη έκδοση).

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Γρήγορα το ARP πίνακες για το κύκλωμα ExpressRoute
Αυτή η ενότητα παρέχει οδηγίες για το πώς μπορείτε να προβάλετε τους πίνακες ARP ανά διεισδύουν χρήση του PowerShell. Μπορείτε ή την υπηρεσία παροχής σύνδεσης πρέπει να έχετε ρυθμίσει τις παραμέτρους του διεισδύουν πριν από την πρόοδο περαιτέρω. Κάθε κύκλωμα έχει δύο διαδρομές (κύριας και δευτερεύουσας). Μπορείτε να ελέγξετε τον πίνακα ARP για κάθε διαδρομή ανεξάρτητα.

### <a name="arp-tables-for-azure-private-peering"></a>Πίνακες ARP για Azure ιδιωτικό διεισδύουν
Το ακόλουθο cmdlet παρέχει το ARP πίνακες για Azure ιδιωτικό διεισδύουν

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Δείγμα εξόδου είναι εμφανίζεται παρακάτω για μία από τις διαδρομές

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Πίνακες ARP για Azure δημόσια διεισδύουν
Το ακόλουθο cmdlet παρέχει το ARP πίνακες για Azure δημόσια διεισδύουν

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Δείγμα εξόδου είναι εμφανίζεται παρακάτω για μία από τις διαδρομές

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Πίνακες ARP για διεισδύουν Microsoft
Το ακόλουθο cmdlet παρέχει το ARP πίνακες για διεισδύουν Microsoft

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Δείγμα εξόδου είναι εμφανίζεται παρακάτω για μία από τις διαδρομές

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Πώς μπορείτε να χρησιμοποιήσετε αυτές τις πληροφορίες
Μια διεισδύουν ARP πίνακα μπορεί να χρησιμοποιηθεί για τον καθορισμό επικύρωση ρύθμιση παραμέτρων επιπέδου 2 και συνδεσιμότητα. Αυτή η ενότητα παρέχει μια επισκόπηση του πώς θα φαίνονται τα ARP πίνακες στην περιοχή διαφορετικά σενάρια.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>Πίνακα ARP όταν ένα κύκλωμα βρίσκεται σε κατάσταση λειτουργίας (αναμενόμενη κατάσταση)

 - Στον πίνακα ARP θα έχει μια εγγραφή για την πλευρά εσωτερικής εγκατάστασης με μια έγκυρη διεύθυνση IP και τη διεύθυνση MAC και μια παρόμοια καταχώρηση για το στο πλάι της Microsoft. 
 - Η τελευταία οκτάδα της διεύθυνσης ip εσωτερικής εγκατάστασης θα είναι πάντα μονό αριθμό.
 - Η τελευταία οκτάδα της διεύθυνσης ip της Microsoft θα είναι πάντα άρτιος αριθμός.
 - Θα εμφανιστεί η ίδια διεύθυνση MAC στην πλευρά της Microsoft για όλα τα 3 peerings (κύριο / δευτερεύοντα). 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP πίνακα μόλις εσωτερικής εγκατάστασης / συνδεσιμότητα παροχής πλευρά έχει προβλήματα

 - Μόνο μία καταχώρηση θα εμφανίζονται στον πίνακα ARP. Αυτό θα εμφανίσει την αντιστοίχιση μεταξύ της διεύθυνσης MAC και διεύθυνση IP που χρησιμοποιείται στο πλάι της Microsoft. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Ανοίξτε μια αίτηση υποστήριξης με την υπηρεσία παροχής σύνδεσης για τον εντοπισμό σφαλμάτων εν λόγω ζητημάτων. 


### <a name="arp-table-when-microsoft-side-has-problems"></a>Πίνακα ARP κατά την πλευρά του προγράμματος Microsoft έχει προβλήματα

 - Δεν θα βλέπετε ένα πίνακα ARP που εμφανίζεται για μια διεισδύουν εάν υπάρχουν ζητήματα στην πλευρά της Microsoft. 
 -  Ανοίξτε ένα δελτίο υποστήριξης με την [υποστήριξη της Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Καθορίστε ότι έχετε κάποιο πρόβλημα με τη σύνδεση επιπέδου 2. 

## <a name="next-steps"></a>Επόμενα βήματα

 - Επικυρώστε τις ρυθμίσεις παραμέτρων επιπέδου 3 για το κύκλωμα ExpressRoute
     - Λήψη δρομολόγηση σύνοψης για να προσδιορίσετε την κατάσταση των περιόδων λειτουργίας το πρωτόκολλο BGP 
     - Λήψη δρομολόγηση πίνακα για να προσδιορίσετε ποια προθέματα έχουν κοινοποιηθεί σε ExpressRoute
 - Επικυρώστε μεταφορά δεδομένων με την αναθεώρηση των byte που μεταβίβασης / ανάληψης ελέγχου
 - Εάν εξακολουθείτε να αντιμετωπίζετε προβλήματα, ανοίξτε ένα δελτίο υποστήριξης με την [υποστήριξη της Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
