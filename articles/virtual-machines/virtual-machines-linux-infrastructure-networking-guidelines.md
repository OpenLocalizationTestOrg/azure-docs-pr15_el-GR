<properties
    pageTitle="Οδηγίες για την υποδομή δικτύου | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με το πλήκτρο σχεδίαση και υλοποίηση οδηγίες για την ανάπτυξη εικονικού δικτύου στις υπηρεσίες υποδομής Azure."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="networking-infrastructure-guidelines"></a>Οδηγίες για την υποδομή δικτύου

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Σε αυτό το άρθρο εστιάζει στην κατανόηση τα απαραίτητα βήματα προγραμματισμού για το εικονικό δίκτυο μέσα σε Azure και συνδεσιμότητας ανάμεσα σε υπάρχοντα περιβάλλοντα εσωτερικής εγκατάστασης.


## <a name="implementation-guidelines-for-virtual-networks"></a>Υλοποίηση οδηγίες για εικονικών δικτύων

Αποφάσεις:

- Τι τύπο εικονικού δικτύου πρέπει να φιλοξενήσετε το φόρτο εργασίας IT ή υποδομή (μόνο στο cloud ή σταυρό εσωτερικής εγκατάστασης);
- Για τα δίκτυα εικονικού σταυρό εσωτερικής εγκατάστασης, πόσο χώρο διευθύνσεων χρειάζεται να φιλοξενήσετε το δευτερεύοντα δίκτυα και ΣΠΣ τώρα και για το λογικό επέκτασης στο μέλλον;
- Πρόκειται να δημιουργήσετε κεντρική εικονικών δικτύων ή να δημιουργήσετε μεμονωμένα εικονικών δικτύων για κάθε ομάδα πόρων;

Εργασίες:

- Ορισμός το χώρο διευθύνσεων για το εικονικό δίκτυα που θα δημιουργηθεί.
- Ορίσετε το σύνολο των δευτερευόντων δικτύων και το χώρο διευθύνσεων για κάθε μία.
- Για διασταυρούμενο εσωτερικής εγκατάστασης εικονικού δίκτυα, ορίσετε το σύνολο των κενών διαστημάτων διεύθυνση τοπικού δικτύου για τις θέσεις εσωτερικής εγκατάστασης που του ΣΠΣ στο δίκτυο εικονικού πρέπει να επικοινωνήσετε.
- Εργασία με εσωτερικής εγκατάστασης έχει ρυθμιστεί κοινωνικής δικτύωσης ομάδας για να βεβαιωθείτε ότι το κατάλληλο δρομολόγηση κατά τη δημιουργία σταυρό εσωτερικής εγκατάστασης εικονικών δικτύων.
- Δημιουργία εικονικών δίκτυο με τη χρήση του κανόνες ονοματοθεσίας.


## <a name="virtual-networks"></a>Εικονικό δίκτυα

Εικονικό δίκτυα είναι απαραίτητες για την υποστήριξη των επικοινωνιών μεταξύ εικονικές μηχανές (ΣΠΣ). Μπορείτε να ορίσετε δευτερεύοντα δίκτυα, προσαρμοσμένη διεύθυνση IP, ρυθμίσεις DNS, φίλτρα ασφαλείας και εξισορρόπηση φόρτου, όπως με φυσικά δίκτυα. Με τη χρήση μιας [Πύλης VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) ή [κυκλώματος δρομολόγηση Express](../expressroute/expressroute-introduction.md), μπορείτε να συνδεθείτε Azure εικονικού δίκτυα στα δίκτυά σας εσωτερικής εγκατάστασης. Μπορείτε να διαβάσετε περισσότερα σχετικά με το [εικονικό δίκτυα και των στοιχείων τους](../virtual-network/virtual-networks-overview.md).

Χρησιμοποιώντας τις ομάδες πόρων, έχετε ευελιξία κατά τη σχεδίαση της εικονικού στοιχεία του δικτύου σας. ΣΠΣ να συνδεθείτε με εικονικού δίκτυα εκτός τις δικές τους ομάδα πόρων. Θα ήταν κοινή προσέγγιση σχεδίασης για να δημιουργήσετε ομάδες κεντρικό πόρο που περιέχουν την υποδομή δικτύου πυρήνα που μπορεί να γίνεται από ένα κοινό την ομάδα. ΣΠΣ και τις εφαρμογές αναπτυχθεί σε ομάδες ξεχωριστή πόρων. Αυτή η προσέγγιση επιτρέπει την πρόσβαση κάτοχοι εφαρμογής στην ομάδα πόρων που περιέχει τους ΣΠΣ χωρίς να ανοίξω πρόσβαση για τη ρύθμιση παραμέτρων των μεγαλύτερου εικονικού κοινωνικής δικτύωσης πόρων.

## <a name="site-connectivity"></a>Σύνδεση τοποθεσίας

### <a name="cloud-only-virtual-networks"></a>Μόνο στο cloud εικονικών δικτύων
Εάν δεν απαιτούν χρήστες εσωτερικής εγκατάστασης και υπολογιστές βρίσκεται σε εξέλιξη συνδεσιμότητας με ΣΠΣ σε μια Azure εικονικού δικτύου, σχεδίαση εικονικού δικτύου σας είναι απευθείας προς τα εμπρός:

![Βασικό μόνο στο cloud εικονικού διάγραμμα δικτύου](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Αυτή η προσέγγιση είναι συνήθως φόρτους εργασίας μέσω Internet, όπως ένα διακομιστή web που βασίζονται στο Internet. Μπορείτε να διαχειριστείτε αυτά τα ΣΠΣ χρησιμοποιώντας SSH ή συνδέσεις VPN σημείου σε τοποθεσία.

Επειδή δεν συνδέονται με το δίκτυό σας στην εσωτερική εγκατάσταση, μόνο Azure εικονικού δίκτυα να χρησιμοποιήσετε οποιοδήποτε τμήμα του ιδιωτικού χώρου διευθύνσεων IP. Ο χώρος διευθύνσεων μπορεί να είναι ίδια ιδιωτικό χώρο που είναι σε χρήση εσωτερικής εγκατάστασης.


### <a name="cross-premises-virtual-networks"></a>Εικονικό δίκτυα σταυρό εσωτερικής εγκατάστασης
Εάν οι χρήστες εσωτερικής εγκατάστασης και υπολογιστές απαιτούν βρίσκεται σε εξέλιξη συνδεσιμότητας με ΣΠΣ σε μια Azure εικονικού δικτύου, δημιουργήστε ένα εικονικό δίκτυο σταυρό εσωτερικής εγκατάστασης. Συνδέστε το εικονικό δίκτυο το δίκτυο εσωτερικής εγκατάστασης με μια σύνδεση VPN τοποθεσίας σε τοποθεσία ή ExpressRoute.

![Διάγραμμα δικτύου εικονικού σταυρό εσωτερικής εγκατάστασης](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

Σε αυτήν τη ρύθμιση, το Azure εικονικού δικτύου είναι ουσιαστικά βασίζεται στο cloud επέκταση του δικτύου σας εσωτερικής εγκατάστασης.

Επειδή μπορούν να συνδεθούν στο δίκτυό σας στην εσωτερική εγκατάσταση, σταυρό εσωτερικής εγκατάστασης εικονικών δικτύων πρέπει να χρησιμοποιήσετε ένα τμήμα του το χώρο διευθύνσεων που χρησιμοποιείται από τον οργανισμό σας που είναι μοναδικό. Με τον ίδιο τρόπο που διαφορετικές θέσεις του εταιρικού σας έχουν εκχωρηθεί σε ένα συγκεκριμένο υποδίκτυο IP, Azure γίνεται μια άλλη θέση, όπως μπορείτε να επεκτείνετε το δίκτυό σας.

Για να επιτρέψετε πακέτα να μεταβαίνουν από το δίκτυο εικονικού σταυρό εσωτερικής εγκατάστασης για το δίκτυο εσωτερικής εγκατάστασης, πρέπει να ρυθμίσετε το σύνολο των σχετικών εσωτερικής διεύθυνση προθέματα ως τμήμα του ορισμού της τοπικού δικτύου για το εικονικό δίκτυο. Ανάλογα με το χώρο διευθύνσεων του εικονικού δικτύου και το σύνολο των σχετικών εσωτερικής θέσεις, μπορεί να υπάρχουν πολλά προθέματα διεύθυνση στο τοπικό δίκτυο.

Μπορείτε να μετατρέψετε ένα μόνο στο cloud εικονικού δικτύου σε δίκτυο εικονικού σταυρό εσωτερικής εγκατάστασης, αλλά πιθανότατα απαιτεί να επανάληψη IP σας χώρο διευθύνσεων εικονικού δικτύου και Azure τους πόρους. Επομένως, εξετάστε προσεκτικά εάν ένα εικονικό δίκτυο πρέπει να είστε συνδεδεμένοι με το δίκτυο εσωτερικής εγκατάστασης, όταν εκχωρείτε μια υποδικτύου διευθύνσεων IP.

## <a name="subnets"></a>Δευτερεύοντα δίκτυα
Δευτερεύοντα δίκτυα σάς επιτρέπουν να οργανώσετε τους πόρους που έχουν σχέση, είτε λογικά (για παράδειγμα, ένα υποδίκτυο για ΣΠΣ που σχετίζεται με την ίδια εφαρμογή), ή φυσικά (για παράδειγμα, ένα υποδίκτυο ανά ομάδα πόρων). Μπορείτε επίσης να χρησιμοποιήσετε υποδικτύου απομόνωσης τεχνικές για πρόσθετη ασφάλεια.

Για τα δίκτυα εικονικού σταυρό εσωτερικής εγκατάστασης, θα πρέπει να σχεδιάζετε δευτερεύοντα δίκτυα με τις ίδιες συμβάσεις που χρησιμοποιείτε για τους πόρους εσωτερικής εγκατάστασης. **Azure πάντα χρησιμοποιεί τις τρεις πρώτες διευθύνσεις IP του χώρου διευθύνσεων για κάθε υποδίκτυο**. Για να καθορίσετε τον αριθμό των διευθύνσεων που χρειάζονται για το υποδίκτυο, ξεκινήστε μετρώντας τον αριθμό των ΣΠΣ που χρειάζεστε πλέον. Εκτίμηση για μελλοντικής ανάπτυξης και, στη συνέχεια, χρησιμοποιήστε τον παρακάτω πίνακα για να προσδιορίσετε το μέγεθος του υποδικτύου.

Αριθμός των ΣΠΣ χρειάζεται | Αριθμός bit κεντρικού υπολογιστή είναι απαραίτητο | Μέγεθος του υποδικτύου
--- | --- | ---
1-3 | 3 | / 29
4-11     | 4 | / 28
12-27 | 5 | / 27
28 – 59 | 6 | / 26
60 – 123 | 7 | / 25

> [AZURE.NOTE] Για δευτερεύοντα δίκτυα κανονική εσωτερικής εγκατάστασης, ο μέγιστος αριθμός διευθύνσεις κεντρικού υπολογιστή για κάποιο δευτερεύον δίκτυο με το bit n κεντρικού υπολογιστή είναι 2<sup>n</sup> -2. Για μια Azure υποδίκτυο, ο μέγιστος αριθμός διευθύνσεις κεντρικού υπολογιστή για κάποιο δευτερεύον δίκτυο με το bit n κεντρικού υπολογιστή είναι 2<sup>n</sup> -5 (2 συν 3 για τις διευθύνσεις που χρησιμοποιεί το Azure σε κάθε υποδίκτυο).

Εάν επιλέξετε το μέγεθος του δευτερεύοντος δικτύου που είναι πολύ μικρό, πρέπει να επανάληψη IP και αναπτύξτε ξανά τα ΣΠΣ στο δευτερεύον.


## <a name="network-security-groups"></a>Ομάδες ασφαλείας δικτύου
Μπορείτε να εφαρμόσετε κανόνες φιλτραρίσματος για την κίνηση που ρέει μέσω εικονικών δίκτυά σας, χρησιμοποιώντας τις ομάδες ασφαλείας δικτύου. Μπορείτε να δημιουργήσετε λεπτομερούς φιλτραρίσματος κανόνες για την ασφάλιση το εικονικό περιβάλλον κοινωνικής δικτύωσης, έλεγχος εισερχόμενων και εξερχόμενων κίνηση, προέλευσης και προορισμού περιοχές διευθύνσεων IP, επιτρέπονται θύρες, κ.λπ. Δίκτυο ομάδες ασφαλείας μπορούν να εφαρμοστούν σε δευτερεύοντα δίκτυα μέσα σε ένα εικονικό δίκτυο ή απευθείας σε ένα περιβάλλον εργασίας που δίνεται εικονικού δικτύου. Συνιστάται να έχετε ένα επίπεδο ομάδας ασφαλείας δικτύου φιλτράρουν κυκλοφορία στον εικονικό δίκτυά σας. Μπορείτε να διαβάσετε περισσότερα σχετικά με τις [Ομάδες ασφαλείας δικτύου](../virtual-network/virtual-networks-nsg.md).


## <a name="additional-network-components"></a>Επιπλέον στοιχεία δικτύου
Όπως με μια εσωτερική φυσικής υποδομή δικτύου, Azure εικονικού δικτύου μπορεί να περιέχει περισσότερα από απλή δευτερεύοντα δίκτυα και εκχώρηση διευθύνσεων IP. Κατά τη σχεδίαση του υποδομής εφαρμογών, ίσως θέλετε να ενσωματώσετε ορισμένα από αυτά τα πρόσθετα στοιχεία:

- [Πύλες VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) - σύνδεση Azure εικονικών δικτύων σε άλλα δίκτυα Azure εικονικού, ή να συνδεθείτε με δίκτυα εσωτερικής εγκατάστασης μέσα από μια σύνδεση VPN-τοποθεσίας. Υλοποίηση δρομολόγηση Express συνδέσεις για αποκλειστική, ασφαλείς συνδέσεις. Μπορείτε επίσης να παρέχετε στους χρήστες άμεση πρόσβαση με συνδέσεις VPN σημείου σε τοποθεσία.
- [Μονάδα εξισορρόπησης φόρτου](../load-balancer/load-balancer-overview.md) - παρέχει εξισορρόπηση φόρτου κίνησης για την κίνηση τόσο εξωτερικών και εσωτερικών όπως θέλετε.
- [Πύλη εφαρμογής](../application-gateway/application-gateway-introduction.md) - HTTP εξισορρόπησης φόρτου στο επίπεδο εφαρμογής, παρέχοντας ορισμένα πρόσθετα πλεονεκτήματα για τις εφαρμογές web αντί για την ανάπτυξη του Azure μονάδα εξισορρόπησης φόρτου.
- [Διαχείριση κίνηση](../traffic-manager/traffic-manager-overview.md) - DNS με βάση την κυκλοφορία διανομής για να κατευθύνετε τους τελικούς χρήστες στο πλησιέστερο τελικό σημείο διαθέσιμη εφαρμογή, επιτρέποντάς σας να φιλοξενήσετε την εφαρμογή από το Azure κέντρα δεδομένων σε διαφορετικές περιοχές.


## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 