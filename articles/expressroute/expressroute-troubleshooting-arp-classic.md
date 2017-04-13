<properties
   pageTitle="Οδηγός αντιμετώπισης προβλημάτων ExpressRoute: γρήγορα ARP πίνακες | Microsoft Azure"
   description="Αυτή η σελίδα παρέχει οδηγίες για γρήγορα το ARP πίνακες για ένα κύκλωμα ExpressRoute."
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

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>Οδηγός αντιμετώπισης προβλημάτων ExpressRoute: γρήγορα ARP πίνακες στο μοντέλο κλασική ανάπτυξης

> [AZURE.SELECTOR]
[PowerShell - διαχείριση πόρων](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - κλασικό](expressroute-troubleshooting-arp-classic.md)

Σε αυτό το άρθρο σάς καθοδηγεί σε τα βήματα για τη λήψη των πινάκων ανάλυση πρωτόκολλο ARP (Address) για το κύκλωμα Azure ExpressRoute.

>[AZURE.IMPORTANT] Αυτό το έγγραφο προορίζεται για να σας βοηθήσει να διάγνωση και διόρθωση προβλημάτων απλό. Δεν προορίζεται για να αντικαθιστά υποστήριξης της Microsoft. Εάν δεν μπορείτε να επιλύσετε το πρόβλημα, χρησιμοποιώντας τις παρακάτω οδηγίες, ανοίξτε μια αίτηση υποστήριξης με το [Microsoft Azure Βοήθεια + υποστήριξης](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Διεύθυνση επίλυσης Protocol (ARP) και ARP πίνακες
ARP είναι ένα πρωτόκολλο επιπέδου 2 που έχει οριστεί στο [RFC 826](https://tools.ietf.org/html/rfc826). ARP χρησιμοποιείται για να αντιστοιχίσετε μια διεύθυνση Ethernet (διεύθυνση MAC) σε μια διεύθυνση IP.

Έναν πίνακα ARP παρέχει μια αντιστοίχιση της διεύθυνσης IPv4 και διεύθυνση MAC για ένα συγκεκριμένο διεισδύουν. Στον πίνακα ARP για μια διεισδύουν κυκλώματος ExpressRoute παρέχει τις ακόλουθες πληροφορίες για κάθε διασύνδεση (κύριας και δευτερεύουσας):

1. Αντιστοίχιση μιας διεύθυνσης IP εσωτερικής δρομολογητή περιβάλλοντος εργασίας σε μια διεύθυνση MAC
2. Αντιστοίχιση της διεύθυνσης IP ExpressRoute δρομολογητή περιβάλλοντος εργασίας σε μια διεύθυνση MAC
3. Η ηλικία της αντιστοίχισης

Πίνακες ARP μπορεί να σας βοηθήσει με την επικύρωση ρύθμισης παραμέτρων επιπέδου 2 και αντιμετώπιση προβλημάτων συνδεσιμότητας βασικές επιπέδου 2.

Ακολουθεί ένα παράδειγμα ενός πίνακα ARP:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Η παρακάτω ενότητα παρέχει πληροφορίες σχετικά με το πώς μπορείτε να προβάλετε τους πίνακες ARP που είναι ορατό από τους δρομολογητές άκρη ExpressRoute.

## <a name="prerequisites-for-using-arp-tables"></a>Προαπαιτούμενα στοιχεία για τη χρήση ARP πίνακες

Βεβαιωθείτε ότι έχετε τα ακόλουθα πριν να συνεχίσετε:

 - Ένα έγκυρο κύκλωμα ExpressRoute που έχει ρυθμιστεί με τουλάχιστον μία διεισδύουν. Το κύκλωμα πρέπει να ρυθμιστεί πλήρως από την υπηρεσία παροχής σύνδεσης. Εσείς (ή την υπηρεσία παροχής συνδεσιμότητας) πρέπει να ρυθμίσετε τις παραμέτρους τουλάχιστον ένα από τα peerings (Azure δημόσια ιδιωτικές, Azure ή Microsoft) σε αυτό το κύκλωμα.

 - Περιοχές διευθύνσεων IP που χρησιμοποιούνται για τη ρύθμιση των παραμέτρων του peerings (Azure δημόσια ιδιωτικές, Azure και Microsoft). Αναθεωρήστε τα παραδείγματα ανάθεσης διεύθυνση IP στη [σελίδα δρομολόγησης απαιτήσεις ExpressRoute](expressroute-routing.md) για να κατανοήσετε τον τρόπο διασυνδέσεων aise σας και, από την πλευρά ExpressRoute αντιστοίχισης διευθύνσεων IP. Μπορείτε να λάβετε πληροφορίες σχετικά με τη ρύθμιση παραμέτρων peering ανατρέχοντας στη [σελίδα Ρύθμιση παραμέτρων διεισδύουν ExpressRoute](expressroute-howto-routing-classic.md).

 - Πληροφορίες της υπηρεσίας παροχής κοινωνικής δικτύωσης ομάδας ή συνδεσιμότητας σχετικά με τις διευθύνσεις MAC, διασυνδέσεις που χρησιμοποιούνται με αυτές τις διευθύνσεις IP.

 - Η πιο πρόσφατη μονάδα Windows PowerShell για Azure (έκδοση 1,50 ή νεότερη έκδοση).

## <a name="arp-tables-for-your-expressroute-circuit"></a>Πίνακες ARP για το κύκλωμα ExpressRoute
Αυτή η ενότητα παρέχει οδηγίες σχετικά με το πώς μπορείτε να προβάλετε τους πίνακες ARP για κάθε τύπο διεισδύουν με χρήση του PowerShell. Πριν να συνεχίσετε, εσείς ή την υπηρεσία παροχής σύνδεσης πρέπει να ρυθμίσετε τις παραμέτρους του διεισδύουν. Κάθε κύκλωμα έχει δύο διαδρομές (κύριας και δευτερεύουσας). Μπορείτε να ελέγξετε τον πίνακα ARP για κάθε διαδρομή ανεξάρτητα.

### <a name="arp-tables-for-azure-private-peering"></a>Πίνακες ARP για Azure ιδιωτικό διεισδύουν
Το ακόλουθο cmdlet παρέχει το ARP πίνακες για Azure ιδιωτικό διεισδύουν:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Ακολουθεί ένα δείγμα εξόδου για μία από τις διαδρομές:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Πίνακες ARP για Azure δημόσια διεισδύουν:
Το ακόλουθο cmdlet παρέχει το ARP πίνακες για Azure δημόσια διεισδύουν:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Ακολουθεί ένα δείγμα εξόδου για μία από τις διαδρομές:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Ακολουθεί ένα δείγμα εξόδου για μία από τις διαδρομές:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Πίνακες ARP για διεισδύουν Microsoft
Το ακόλουθο cmdlet παρέχει το ARP πίνακες για διεισδύουν Microsoft:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Δείγμα εξόδου εμφανίζεται παρακάτω για μία από τις διαδρομές:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Πώς μπορείτε να χρησιμοποιήσετε αυτές τις πληροφορίες
Μια διεισδύουν ARP πίνακα μπορεί να χρησιμοποιηθεί για την επικύρωση ρύθμιση παραμέτρων επιπέδου 2 και συνδεσιμότητα. Αυτή η ενότητα παρέχει μια επισκόπηση της εμφάνισης των πινάκων ARP σε διαφορετικά σενάρια.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>Πίνακα ARP όταν ένα κύκλωμα βρίσκεται σε κατάσταση λειτουργίας (Αναμενόμενο)

 - Ο πίνακας ARP έχει μια εγγραφή για την πλευρά εσωτερικής εγκατάστασης με μια έγκυρη διεύθυνση IP και MAC και μια παρόμοια καταχώρηση για το στο πλάι της Microsoft.
 - Η τελευταία οκτάδα της διεύθυνσης IP εσωτερικής πάντα είναι περιττός αριθμός.
 - Η τελευταία οκτάδα της διεύθυνσης Microsoft IP πάντα είναι άρτιος αριθμός.
 - Εμφανίζεται η ίδια διεύθυνση MAC στην πλευρά της Microsoft για όλους τρεις peerings (πρωτεύον/δευτερεύον).


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>Πίνακα ARP όταν είναι εσωτερικής εγκατάστασης ή όταν στο πλάι συνδεσιμότητας παροχής έχει προβλήματα

 Εμφανίζεται μόνο μία εγγραφή στον πίνακα ARP. Εμφανίζει την αντιστοίχιση μεταξύ της διεύθυνσης MAC και τη διεύθυνση IP που χρησιμοποιείται στην πλευρά της Microsoft.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Εάν αντιμετωπίζετε πρόβλημα ως εξής, ανοίξτε μια αίτηση υποστήριξης με την υπηρεσία παροχής σύνδεσης για να το επιλύσετε.


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>Πίνακα ARP κατά την πλευρά του προγράμματος Microsoft έχει προβλήματα

 - Δεν θα βλέπετε ένα πίνακα ARP που εμφανίζεται για μια διεισδύουν εάν υπάρχουν ζητήματα στην πλευρά της Microsoft.
 -  Ανοίξτε μια αίτηση υποστήριξης με [Βοήθεια Microsoft Azure + υποστήριξης](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Καθορίστε ότι έχετε κάποιο πρόβλημα με τη σύνδεση επιπέδου 2.

## <a name="next-steps"></a>Επόμενα βήματα

 - Επικυρώστε τις ρυθμίσεις παραμέτρων επιπέδου 3 για το κύκλωμα ExpressRoute:
     - Λάβετε μια διαδρομή σύνοψης για να προσδιορίσετε την κατάσταση των περιόδων λειτουργίας το πρωτόκολλο BGP.
     - Λάβετε έναν πίνακα διαδρομών για να προσδιορίσετε ποια προθέματα έχουν κοινοποιηθεί σε ExpressRoute.
 - Επικυρώστε μεταφορά δεδομένων με την αναθεώρηση και σμίκρυνση byte που.
 - Ανοίξτε μια αίτηση υποστήριξης με [Βοήθεια Microsoft Azure + υποστήριξης](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , εάν εξακολουθείτε να αντιμετωπίζετε προβλήματα.
