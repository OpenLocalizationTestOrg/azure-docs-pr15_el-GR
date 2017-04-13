<properties
    pageTitle="Azure υπηρεσίες τομέα AD: Γενικές οδηγίες δικτύωση | Microsoft Azure"
    description="Ζητήματα δικτύωσης για υπηρεσίες τομέα Active Directory Azure"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="maheshu"/>

# <a name="networking-considerations-for-azure-ad-domain-services"></a>Ζητήματα δικτύωσης για τις υπηρεσίες τομέα AD Azure

## <a name="how-to-select-an-azure-virtual-network"></a>Πώς μπορείτε να επιλέξετε μια Azure εικονικού δικτύου
Οι παρακάτω οδηγίες θα σας βοηθήσει να επιλέξετε ένα εικονικό δίκτυο για να χρησιμοποιήσετε με υπηρεσίες τομέα AD Azure.

### <a name="type-of-azure-virtual-network"></a>Τύπος Azure εικονικού δικτύου

- Μπορείτε να ενεργοποιήσετε τις υπηρεσίες τομέα AD Azure σε μια κλασική Azure εικονικού δικτύου.

- Azure υπηρεσίες τομέα AD **δεν είναι δυνατό να ενεργοποιηθεί σε εικονικό δίκτυα δημιουργήθηκε με τη διαχείριση πόρων Azure**.

- Μπορείτε να συνδέσετε ένα εικονικό δίκτυο με βάση τη διαχείριση πόρων σε κλασική εικονικό δίκτυο στο οποίο είναι ενεργοποιημένη Azure υπηρεσίες τομέα AD. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε τις υπηρεσίες τομέα AD Azure στο δίκτυο εικονικού βάσει διαχείριση πόρων. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [η συνδεσιμότητα του δικτύου](active-directory-ds-networking.md#network-connectivity) .

- **Τοπικές εικονικών δικτύων**: Εάν σκοπεύετε να χρησιμοποιήσετε μια υπάρχουσα εικονικού δικτύου, βεβαιωθείτε ότι είναι μια τοπικές εικονικού δικτύου.

    - Εικονικό δίκτυα που χρησιμοποιούν το μηχανισμό παλαιού τύπου συσχέτισης ομάδες δεν μπορούν να χρησιμοποιηθούν με τις υπηρεσίες τομέα AD Azure.

    - Για να χρησιμοποιήσετε τις υπηρεσίες τομέα AD Azure, [μετεγκατάσταση παλαιού τύπου εικονικού δίκτυα σε τοπικές εικονικού δίκτυα](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).


### <a name="azure-region-for-the-virtual-network"></a>Περιοχή Azure για το εικονικό δίκτυο

- Τις υπηρεσίες τομέα AD Azure διαχειριζόμενων τομέα έχει αναπτυχθεί στην ίδια περιοχή Azure ως το εικονικό δίκτυο επιλέγετε για να ενεργοποιήσετε την υπηρεσία στο.

- Επιλέξτε ένα εικονικό δίκτυο σε μια περιοχή Azure που υποστηρίζονται από τις υπηρεσίες τομέα AD Azure.

- Δείτε τη σελίδα [υπηρεσίες Azure ανά περιοχή](https://azure.microsoft.com/regions/#services/) να γνωρίζετε τις Azure περιοχές στις οποίες Azure υπηρεσίες τομέα AD είναι διαθέσιμη.


### <a name="requirements-for-the-virtual-network"></a>Απαιτήσεις για το εικονικό δίκτυο

- **Απόσταση από το Azure φόρτους εργασίας**: Επιλέξτε το εικονικό δίκτυο που αυτήν τη στιγμή φιλοξενεί/θα φιλοξενήσει εικονικές μηχανές που πρέπει να έχετε πρόσβαση στις υπηρεσίες τομέα AD Azure.

- **Οι διακομιστές DNS προσαρμοσμένη/μεταφορά-σας-κάτοχος**: Βεβαιωθείτε ότι δεν υπάρχουν προσαρμοσμένες διακομιστές DNS έχουν ρυθμιστεί για το εικονικό δίκτυο.

- **Υπάρχον τομέων με το ίδιο όνομα τομέα**: Βεβαιωθείτε ότι δεν έχετε έναν υπάρχοντα τομέα με το ίδιο όνομα τομέα που είναι διαθέσιμες σε αυτό το εικονικό δίκτυο. Για παράδειγμα, ας υποθέσουμε ότι έχετε έναν τομέα που ονομάζεται 'contoso.com' ήδη διαθέσιμα του επιλεγμένου εικονικού δικτύου. Αργότερα, προσπαθείτε να ενεργοποιήσετε μια υπηρεσίες τομέα AD Azure διαχειριζόμενων τομέα με το ίδιο όνομα τομέα (δηλαδή, 'contoso.com') σε αυτό το εικονικό δίκτυο. Παρουσιαστεί σφάλμα όταν προσπαθείτε να ενεργοποιήσετε τις υπηρεσίες τομέα AD Azure. Αυτό το σφάλμα οφείλεται διενέξεων ονομάτων για το όνομα τομέα που εικονικού δικτύου. Σε αυτήν την περίπτωση, πρέπει να χρησιμοποιήσετε ένα διαφορετικό όνομα για να ρυθμίσετε τον τομέα σας διαχειριζόμενων υπηρεσίες τομέα AD Azure. Εναλλακτικά, μπορείτε να καταργήστε την προμήθεια τον υπάρχοντα τομέα και, στη συνέχεια, προχωρήστε για να ενεργοποιήσετε τις υπηρεσίες τομέα AD Azure.

> [AZURE.WARNING] Δεν μπορείτε να μετακινήσετε τις υπηρεσίες τομέα σε διαφορετικό δίκτυο εικονικού μετά την ενεργοποίηση της υπηρεσίας.


## <a name="network-security-groups-and-subnet-design"></a>Ομάδες ασφαλείας δικτύου και υποδικτύου σχεδίασης
Μια [Ομάδα ασφαλείας δικτύου (NSG)](../virtual-network/virtual-networks-nsg.md) περιέχει μια λίστα των κανόνων λίστας ελέγχου πρόσβασης (ACL) που επιτρέψετε ή να απορρίψετε την κίνηση του δικτύου σας παρουσίες Εικονική σε ένα εικονικό δίκτυο. NSGs μπορούν να συσχετιστούν με δευτερεύοντα δίκτυα ή ξεχωριστές παρουσίες Εικονική μέσα σε αυτό το δευτερεύον δίκτυο. Όταν μια NSG σχετίζεται με ένα υποδίκτυο, εφαρμόζονται οι κανόνες ACL για όλες τις παρουσίες Εικονική σε αυτό το δευτερεύον δίκτυο. Επιπλέον, μπορεί να είναι περιορισμένο κίνηση σε μεμονωμένα Εικονική μηχανή περαιτέρω, συσχετίζοντας ένα NSG απευθείας στο συγκεκριμένο Εικονική.

![Προτεινόμενη υποδικτύου σχεδίασης](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)


### <a name="best-practices-for-choosing-a-subnet"></a>Βέλτιστες πρακτικές για την επιλογή ενός δευτερεύοντος δικτύου
- Ανάπτυξη υπηρεσίες τομέα AD Azure σε ένα **ξεχωριστό αποκλειστικό υποδίκτυο** εντός του Azure εικονικού δικτύου.

- Δεν εφαρμόζονται NSGs στο αποκλειστικό υποδίκτυο για τον τομέα σας διαχειριζόμενων. Εάν πρέπει να εφαρμόσετε NSGs στο αποκλειστικό υποδίκτυο, βεβαιωθείτε ότι έχετε **αποκλείει τις θύρες που απαιτούνται για να εξυπηρέτησης και διαχείριση του τομέα σας**.

- Χωρίς περιορισμό υπερβολικά τον αριθμό των διευθύνσεων IP που είναι διαθέσιμο στο δευτερεύον αποκλειστικός για τον τομέα σας διαχειριζόμενων. Αυτός ο περιορισμός εμποδίζει διάθεση δύο ελεγκτές τομέα για τον τομέα σας διαχειριζόμενων της υπηρεσίας.

- **Δεν δίνει τη δυνατότητα υπηρεσίες τομέα AD Azure στο δευτερεύον πύλης** του εικονικού δικτύου σας.


> [AZURE.WARNING] Όταν συσχετίζετε ένα NSG με ένα δευτερεύον δίκτυο στο οποίο υπηρεσίες τομέα AD Azure είναι ενεργοποιημένη, που ενδέχεται να διακόψουν τη δυνατότητα της Microsoft για υπηρεσίας και τη διαχείριση του τομέα. Επιπλέον, διακοπεί συγχρονισμό μεταξύ του μισθωτή Azure AD και διαχειριζόμενων τον τομέα σας. **Το SLA δεν ισχύει για αναπτύξεις όπου μια NSG έχει εφαρμοστεί που αποκλείει υπηρεσίες τομέα AD Azure από ενημέρωση και τη διαχείριση του τομέα σας.**


### <a name="ports-required-for-azure-ad-domain-services"></a>Θύρες που απαιτείται για υπηρεσίες τομέα AD Azure
Οι ακόλουθες θύρες απαιτείται για υπηρεσίες τομέα AD Azure στην υπηρεσία και να διατηρήσετε διαχειριζόμενων τον τομέα σας. Βεβαιωθείτε ότι αυτές οι θύρες δεν αποκλείονται για το υποδίκτυο στο οποίο έχετε ενεργοποιήσει διαχειριζόμενων τον τομέα σας.

| Αριθμός θύρας | Σκοπός |
|---|---|
| 443 | Συγχρονισμός με το μισθωτή του Azure AD |
| 3389 | Διαχείριση του τομέα σας |
| 5986 | Διαχείριση του τομέα σας |
| 636 | Ασφαλή πρόσβαση LDAP (LDAPS) για τον τομέα σας διαχειριζόμενων |



## <a name="network-connectivity"></a>Η συνδεσιμότητα του δικτύου
Μια διαχειριζόμενη τομέα υπηρεσίες τομέα AD Azure μπορούν να ενεργοποιηθούν μόνο μέσα σε ένα μεμονωμένο κλασική εικονικού δικτύου στο Azure. Εικονικό δίκτυα που δημιουργήθηκε με τη διαχείριση πόρων Azure δεν υποστηρίζονται.


### <a name="scenarios-for-connecting-azure-networks"></a>Σενάρια για σύνδεση Azure δικτύων
Σύνδεση Azure εικονικού δίκτυα να χρησιμοποιείτε τον τομέα διαχειριζόμενων οποιοδήποτε από τα παρακάτω σενάρια ανάπτυξης:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Χρήση διαχειριζόμενων τομέα με περισσότερες από μία Azure κλασική εικονικού δικτύου
Μπορείτε να συνδεθείτε άλλα Azure κλασική εικονικού δίκτυα το Azure κλασική εικονικό δίκτυο στο οποίο έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure. Αυτή η σύνδεση VPN σάς επιτρέπει να χρησιμοποιείτε τον διαχειριζόμενων τομέα με το φόρτο εργασίας αναπτυχθεί σε άλλα δίκτυα εικονικού.

![Η συνδεσιμότητα του κλασική εικονικού δικτύου](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Χρησιμοποιείτε τον τομέα διαχειριζόμενων σε ένα δίκτυο εικονικού βάσει διαχείριση πόρων
Μπορείτε να συνδεθείτε από διαχειριστή πόρων βάσει εικονικό δίκτυο με το Azure κλασική εικονικό δίκτυο στο οποίο έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure. Αυτήν τη σύνδεση σάς επιτρέπει να χρησιμοποιείτε τον διαχειριζόμενων τομέα με το φόρτο εργασίας αναπτυχθεί στο διαχειριστή πόρων βάσει εικονικού δικτύου.

![Διαχείριση πόρων για να συνδεσιμότητας κλασική εικονικού δικτύου](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### <a name="network-connection-options"></a>Επιλογές σύνδεσης δικτύου

- **VNet-VNet συνδέσεις που χρησιμοποιούν συνδέσεις VPN--τοποθεσίας**: σύνδεση ένα εικονικό δίκτυο με κάποιο άλλο εικονικό δίκτυο (VNet-VNet) είναι παρόμοια με τη σύνδεση σε δίκτυο εικονικού σε μια θέση τοποθεσία εσωτερικής εγκατάστασης. Και οι δύο τύποι συνδεσιμότητας Χρησιμοποιήστε μια πύλη VPN για να παρέχουν μια ασφαλής διοχέτευση χρησιμοποιώντας ασφαλείας IP/IKE.

    ![Χρήση πύλης VPN συνδεσιμότητας εικονικού δικτύου](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Περισσότερες πληροφορίες - σύνδεση εικονικού δίκτυα χρησιμοποιώντας πύλης VPN](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **VNet-VNet συνδέσεων με διεισδύουν εικονικού δικτύου**: διεισδύουν εικονικού δικτύου είναι ένας μηχανισμός που συνδέει δύο δίκτυα εικονικού στην ίδια περιοχή μέσα από το Azure κεντρικό δίκτυο. Αφού peered, των δύο δικτύων εικονικού εμφανίζονται ως ένα για όλες τις χρήσεις συνδεσιμότητας. Εξακολουθείτε να πραγματοποιείται ως ξεχωριστή πόρους, αλλά εικονικές μηχανές σε αυτά τα δίκτυα εικονικού μπορούν να επικοινωνούν μεταξύ τους απευθείας με τη χρήση ιδιωτικών διευθύνσεων IP.

    ![Χρήση διεισδύουν συνδεσιμότητας εικονικού δικτύου](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Περισσότερες πληροφορίες - εικονικού δικτύου διεισδύουν](../virtual-network/virtual-network-peering-overview.md)



<br>

## <a name="related-content"></a>Σχετικό περιεχόμενο

- [Διεισδύουν Azure εικονικού δικτύου](../virtual-network/virtual-network-peering-overview.md)

- [Ρύθμιση παραμέτρων σύνδεσης VNet-VNet για το μοντέλο κλασική ανάπτυξης](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Ομάδες ασφαλείας Azure δικτύου](../virtual-network/virtual-networks-nsg.md)