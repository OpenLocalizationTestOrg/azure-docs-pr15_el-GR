<properties
   pageTitle="Επισκόπηση Azure εικονικού δικτύου (VNet)"
   description="Μάθετε σχετικά με τα δίκτυα εικονικού (VNets) στο Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="virtual-network-overview"></a>Επισκόπηση εικονικού δικτύου

Azure εικονικού δικτύου (VNet) είναι μια αναπαράσταση του δικτύου τη δική σας στο cloud.  Είναι μια λογική απομόνωσης του Azure αποκλειστικό στη συνδρομή σας στο cloud. Μπορείτε να ελέγχετε πλήρως το μπλοκ διευθύνσεων IP, DNS ρυθμίσεις, πολιτικές ασφάλειας και πίνακες διαδρομών μέσα σε αυτό το δίκτυο. Μπορείτε να επίσης περαιτέρω τμήμα VNet σας σε δευτερεύοντα δίκτυα και εκκίνηση Azure IaaS εικονικές μηχανές (ΣΠΣ) ή/και [τις υπηρεσίες Cloud (PaaS ρόλο παρουσίες)](../cloud-services/cloud-services-choose-me.md). Επιπλέον, μπορείτε να συνδεθείτε στο δίκτυο εικονικού στο δίκτυό σας εσωτερικής εγκατάστασης χρησιμοποιώντας μία από τις [Επιλογές συνδεσιμότητας](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) διαθέσιμη στο Azure. Στην πραγματικότητα, μπορείτε να επεκτείνετε το δίκτυό σας σε Azure, με πλήρη έλεγχο σε μπλοκ διευθύνσεων IP με το πλεονέκτημα της κλίμακας για μεγάλες επιχειρήσεις παρέχει Azure.

Για να κατανοήσετε καλύτερα VNets, Ρίξτε μια ματιά στην παρακάτω εικόνα που δείχνει ένα δίκτυο απλοποιημένη εσωτερικής εγκατάστασης.

![Δίκτυο εσωτερικής εγκατάστασης](./media/virtual-networks-overview/figure01.png)

Στην παραπάνω εικόνα εμφανίζεται ένα δίκτυο εσωτερικής συνδεδεμένοι στο Internet δημόσια μέσω δρομολογητή. Μπορείτε επίσης να δείτε ένα τείχος προστασίας μεταξύ το δρομολογητή και μια DMZ φιλοξενίας ένα διακομιστή DNS και ένα σύμπλεγμα διακομιστών web. Σύμπλεγμα διακομιστών web είναι εξισορρόπηση χρησιμοποιώντας μια μονάδα εξισορρόπησης φόρτου υλικού που εκτίθεται στο Internet και καταναλώνει πόρους από το εσωτερικό υποδίκτυο. Το εσωτερικό υποδίκτυο διαχωρίζεται από το DMZ με ένα άλλο τείχος προστασίας, και διακομιστές ελεγκτή τομέα Active Directory κεντρικούς υπολογιστές, διακομιστές βάσεων δεδομένων και διακομιστές εφαρμογών.

Στο ίδιο δίκτυο μπορούν να φιλοξενηθούν σε Azure, όπως φαίνεται στην παρακάτω εικόνα.

![Azure εικονικού δικτύου](./media/virtual-networks-overview/figure02.png)

Παρατηρήστε πώς λαμβάνει την υποδομή του Azure σχετικά με το ρόλο του δρομολογητή, υπάρχει δυνατότητα πρόσβασης από το VNet στο δημόσιο Internet χωρίς να χρειάζεστε οποιαδήποτε ρύθμιση παραμέτρων. Τείχη προστασίας μπορούν να αντικαταστήσουν με ομάδες ασφαλείας δικτύου (NSGs) που εφαρμόζεται σε κάθε μεμονωμένο υποδίκτυο. Και φυσική φόρτωσης balancers είναι αντικαθίστανται από internet αντικριστές και εσωτερικών φόρτωσης balancers στο Azure.

>[AZURE.NOTE] Υπάρχουν δύο λειτουργίες ανάπτυξης στο Azure: κλασική (γνωστό και ως υπηρεσία διαχείρισης) και διαχείριση πόρων Azure (ARM). Κλασική VNets μπορεί να προστεθεί σε μια ομάδα συσχέτισης ή δημιουργείται ως μια VNet τοπικές ρυθμίσεις. Εάν έχετε ένα VNet σε μια ομάδα συσχέτισης, συνιστάται να [μετεγκαταστήσετε να μια τοπικές VNet](virtual-networks-migrate-to-regional-vnet.md).

## <a name="virtual-network-benefits"></a>Πλεονεκτήματα εικονικού δικτύου

- **Απομόνωσης**. VNets είναι πλήρως απομόνωσης από το ένα το άλλο. Σας επιτρέπει να δημιουργήσετε ασυνεχή δικτύων για ανάπτυξη, δοκιμή και παραγωγής που χρησιμοποιούν το ίδιο μπλοκ διεύθυνσης CIDR.

- **Πρόσβαση σε δημόσια στο Internet**. Όλα τα ΣΠΣ IaaS και PaaS παρουσιών ρόλο σε μια VNet να αποκτήσετε πρόσβαση σε δημόσια στο Internet από προεπιλογή. Μπορείτε να ελέγχετε την πρόσβαση, χρησιμοποιώντας τις ομάδες ασφαλείας δικτύου (NSGs).

- **Πρόσβαση σε ΣΠΣ εντός του VNet**. Παρουσίες ρόλο PaaS και IaaS ΣΠΣ μπορεί να εκκινηθεί στο ίδιο δίκτυο εικονικού και μπορούν να συνδεθούν μεταξύ τους με χρήση ιδιωτικών διευθύνσεων IP, ακόμα και αν βρίσκονται σε διαφορετικά δευτερεύοντα δίκτυα χωρίς να χρειάζεται να ρυθμίσετε τις παραμέτρους μιας πύλης ή χρήση δημόσιων διευθύνσεων IP.

- **Επίλυση ονόματος**. Azure παρέχει όνομα εσωτερικού επίλυση για ΣΠΣ IaaS και PaaS ρόλο παρουσίες αναπτυχθεί στο VNet σας. Επίσης, μπορείτε να αναπτύξετε τις δικές σας διακομιστές DNS και να ρυθμίσετε τις παραμέτρους του VNet για να τις χρησιμοποιήσετε.

- **Ασφάλεια**. Εισαγωγή και έξοδος από το εικονικές μηχανές και PaaS ρόλο παρουσιών σε μια VNet την κυκλοφορία μπορεί να ελέγχεται χρησιμοποιώντας ομάδες ασφαλείας δικτύου.

- **Συνδεσιμότητα**. VNets μπορούν να συνδεθούν μεταξύ τους με τη χρήση πύλες δικτύου ή διεισδύουν VNet. VNets μπορεί να συνδεθεί με κέντρα δεδομένων εσωτερικής εγκατάστασης μέσω δίκτυα VPN--τοποθεσίας ή Azure ExpressRoute. Για να μάθετε περισσότερα σχετικά με τη σύνδεση VPN τοποθεσίας σε τοποθεσία, επισκεφθείτε [VPN σχετικά με τις πύλες](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site). Για να μάθετε περισσότερα σχετικά με το ExpressRoute, επισκεφθείτε [ExpressRoute Τεχνική επισκόπηση](../expressroute/expressroute-introduction.md). Για να μάθετε περισσότερα σχετικά με VNet διεισδύουν, επισκεφθείτε [διεισδύουν VNet](virtual-network-peering-overview.md).

    >[AZURE.NOTE] Βεβαιωθείτε ότι έχετε δημιουργήσει μια VNet πριν από την ανάπτυξη οποιαδήποτε ΣΠΣ IaaS ή PaaS ρόλο παρουσίες στο περιβάλλον του Azure. ARM βάσει ΣΠΣ απαιτείται μια VNet και, εάν δεν καθορίσετε μια υπάρχουσα VNet, Azure δημιουργεί έναν προεπιλεγμένο VNet που ενδέχεται να έχετε μια διένεξη μπλοκ διεύθυνσης CIDR με το δίκτυο εσωτερικής εγκατάστασης. Καθιστώντας αδύνατη για να συνδεθείτε VNet σας για το δίκτυο εσωτερικής εγκατάστασης.

## <a name="subnets"></a>Δευτερεύοντα δίκτυα

Είναι μια περιοχή διευθύνσεων IP στο το VNet, μπορείτε να διαιρέσετε μια VNet σε πολλά δευτερεύοντα δίκτυα για την εταιρεία και την ασφάλεια. ΣΠΣ και PaaS ρόλο παρουσίες αναπτυχθεί σε δευτερεύοντα δίκτυα (ίδια ή διαφορετικό) μέσα σε μια VNet μπορούν να επικοινωνούν μεταξύ τους χωρίς οποιαδήποτε επιπλέον ρύθμιση παραμέτρων. Μπορείτε επίσης να ρυθμίσετε τους πίνακες δρομολόγηση και NSGs με ένα δευτερεύον.

## <a name="ip-addresses"></a>Διευθύνσεις IP


Υπάρχουν δύο τύποι διευθύνσεις IP που έχουν εκχωρηθεί σε πόρους στο Azure: *δημόσια* και *ιδιωτικά*. Δημόσιες διευθύνσεις IP επιτρέπουν Azure πόρων για την επικοινωνία με το Internet και άλλες Azure δημόσιες υπηρεσίες όπως το [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure συμβάν διανομείς](https://azure.microsoft.com/documentation/services/event-hubs/). Διευθύνσεις IP ιδιωτική επιτρέπει την επικοινωνία μεταξύ των πόρων σε ένα εικονικό δίκτυο, μαζί με αυτά συνδεδεμένοι μέσω ενός VPN, χωρίς να χρησιμοποιήσετε μια δυνατότητα δρομολόγησης Internet διευθύνσεις.

Για να μάθετε περισσότερα σχετικά με τις διευθύνσεις IP στο Azure, επισκεφθείτε [διευθύνσεις IP του εικονικού δικτύου](virtual-network-ip-addresses-overview-arm.md)

## <a name="azure-load-balancers"></a>Balancers Azure φόρτωσης

Εικονικές μηχανές και τις υπηρεσίες cloud σε ένα δίκτυο εικονικές μπορεί να είναι εκτεθειμένη Internet, χρησιμοποιώντας balancers Azure φόρτωση. Επιχειρηματικές εφαρμογές που είναι άμεσα προσβάσιμη από εσωτερικές μόνο μπορεί να είναι εξισορρόπηση χρησιμοποιώντας εσωτερική εξισορρόπηση φόρτου.

- **Εξωτερική εξισορρόπηση φόρτου**. Μπορείτε να χρησιμοποιήσετε μια εξωτερική εξισορρόπηση φόρτου, για την παροχή υψηλή διαθεσιμότητα ΣΠΣ IaaS και PaaS παρουσίες ρόλο προσβάσιμη από το Internet δημόσια.

- Η **μονάδα εξισορρόπησης φόρτου εσωτερικά**. Μπορείτε να χρησιμοποιήσετε μια εσωτερική εξισορρόπηση φόρτου για την παροχή υψηλή διαθεσιμότητα ΣΠΣ IaaS και PaaS ρόλο παρουσίες προσβάσιμες από άλλες υπηρεσίες του VNet σας.

Για να μάθετε περισσότερα σχετικά με τη φόρτωση εξισορρόπηση στο Azure, επισκεφθείτε τη [Φόρτωση εξισορρόπησης Επισκόπηση](../load-balancer/load-balancer-overview.md).

## <a name="network-security-group-nsg"></a>Ομάδα ασφαλείας δικτύου (NSG)

Μπορείτε να δημιουργήσετε NSGs για να ελέγχετε την πρόσβαση εισερχόμενων και εξερχόμενων σε διασυνδέσεις δικτύου (NIC), ΣΠΣ και δευτερεύοντα δίκτυα. Κάθε NSG περιέχει μία ή περισσότερες κανόνες, καθορίζοντας ή όχι εγκριθεί ή δεν επιτρέπεται η κίνηση που βασίζονται σε διεύθυνση IP προέλευσης, θύρα προέλευσης, τη διεύθυνση IP προορισμού και θύρα προορισμού. Για να μάθετε περισσότερα σχετικά με το NSGs, επισκεφθείτε [Τι είναι μια ομάδα ασφαλείας δικτύου](virtual-networks-nsg.md).

## <a name="virtual-appliances"></a>Εικονικό συσκευές

Μια εικονική συσκευή είναι απλώς μια άλλη Εικονική στα σας VNet που εκτελεί μια συνάρτηση συσκευής βάσει λογισμικού, όπως τείχος προστασίας, WAN βελτιστοποίηση ή ανίχνευση παραβίασης. Μπορείτε να δημιουργήσετε μια διαδρομή στο Azure για να δρομολογήσετε την κυκλοφορία σας VNet μέσω μια εικονική συσκευή για να χρησιμοποιήσετε τις δυνατότητές του.

Για παράδειγμα, NSGs μπορεί να χρησιμοποιηθεί για την παροχή ασφάλειας στην VNet σας. Ωστόσο, NSGs παρέχουν επίπεδο 4 λίστας ελέγχου πρόσβασης (ACL) για εισερχόμενων και εξερχόμενων πακέτων. Εάν θέλετε να χρησιμοποιήσετε ένα μοντέλο 7 ασφάλεια επιπέδου, πρέπει να χρησιμοποιήσετε μια συσκευή τείχους προστασίας.

Εικονικό συσκευές εξαρτώνται από [ορίζονται από το χρήστη δρομολογεί και προώθηση IP](virtual-networks-udr-overview.md).

## <a name="limits"></a>Όρια
Υπάρχουν όρια στον αριθμό των εικονικών δικτύων επιτρέπεται σε μια συνδρομή, ανατρέξτε [δικτύωσης Azure όρια](../azure-subscription-service-limits.md#networking-limits) για περισσότερες πληροφορίες.

## <a name="pricing"></a>Τις τιμές
Είναι δεν επιπλέον κόστους για τη χρήση εικονικού δίκτυα στο Azure. Ξεκινά το εντός του Vnet τις παρουσίες του υπολογισμού θα χρεωθεί τις τυπικές χρεώσεις όπως περιγράφεται στο [Azure Εικονική τιμολόγησης](https://azure.microsoft.com/pricing/details/virtual-machines/). Η [Πύλες VPN](https://azure.microsoft.com/pricing/details/vpn-gateway/) και [δημόσιες διευθύνσεις IP] (https://azure.microsoft.com/pricing/details/ip-addresses/) χρησιμοποιείται σε το VNet θα αναλάβει τη τυπικές χρεώσεις.

## <a name="next-steps"></a>Επόμενα βήματα

- [Δημιουργία ενός VNet](virtual-networks-create-vnet-arm-pportal.md) και δευτερεύοντα δίκτυα.
- [Δημιουργήστε μια Εικονική στα μια VNet](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Μάθετε πώς να [NSGs](virtual-networks-nsg.md).
- Μάθετε πώς να [ορίζονται από το χρήστη δρομολογεί και προώθηση IP](virtual-networks-udr-overview.md).