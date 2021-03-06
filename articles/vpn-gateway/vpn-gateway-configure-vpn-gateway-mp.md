<properties 
   pageTitle="Ρύθμιση παραμέτρων μιας πύλης VPN στην πύλη του Azure κλασική | Microsoft Azure"
   description="Σε αυτό το άρθρο σάς καθοδηγεί σε τη ρύθμιση των παραμέτρων σας εικονικού δικτύου πύλη VPN και την αλλαγή μιας πύλης VPN δρομολόγησης τύπου."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vpn-gateway-for-the-classic-deployment-model"></a>Ρύθμιση παραμέτρων μιας πύλης VPN για το μοντέλο κλασική ανάπτυξης


Εάν θέλετε να δημιουργήσετε μια σύνδεση ασφαλούς σταυρό εσωτερικής εγκατάστασης μεταξύ Azure και τη θέση του εσωτερικής εγκατάστασης, πρέπει να ρυθμίσετε μια σύνδεση VPN πύλης. Στο μοντέλο κλασική ανάπτυξης, μια πύλη μπορεί να είναι μία από δύο τύπους δρομολόγησης VPN: στατικό ή δυναμικό. Ο τύπος που επιλέγετε εξαρτάται από το πρόγραμμα σχεδίασης δικτύου, και η συσκευή VPN εσωτερικής εγκατάστασης που θέλετε να χρησιμοποιήσετε. 

Για παράδειγμα, ορισμένες επιλογές σύνδεσης, όπως μια σύνδεση σημείου σε τοποθεσία, απαιτείται μια δυναμική πύλη δρομολόγησης. Εάν θέλετε να ρυθμίσετε τις παραμέτρους της πύλης για την υποστήριξη συνδέσεων σημείου σε τοποθεσία (P2S) και μια σύνδεση τοποθεσίας σε τοποθεσία (S2S), πρέπει να ρυθμίσετε τις παραμέτρους μια δυναμική πύλη δρομολόγησης Παρόλο που--τοποθεσίας μπορούν να ρυθμιστούν με είτε πύλης VPN δρομολόγησης τύπου. 

Επιπλέον, πρέπει να βεβαιωθείτε ότι η συσκευή που θέλετε να χρησιμοποιήσετε για τη σύνδεσή σας υποστηρίζει τον τύπο δρομολόγησης VPN που θέλετε να δημιουργήσετε. Ανατρέξτε στο θέμα [σχετικά με τις συσκευές VPN](vpn-gateway-about-vpn-devices.md).


**Σχετικά με αυτό το άρθρο** 

Σε αυτό το άρθρο έχει συνταχθεί για το μοντέλο κλασική ανάπτυξης με την [πύλη κλασική](https://manage.windowsazure.com) (δεν Azure πύλη). 

**Σχετικά με τα μοντέλα Azure ανάπτυξης**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-overview"></a>Επισκόπηση της ρύθμισης παραμέτρων

Ακολουθήστε τα παρακάτω βήματα θα σας καθοδηγήσουν τη ρύθμιση των παραμέτρων της πύλης VPN στην πύλη του Azure κλασική. Αυτά τα βήματα ισχύουν για πύλες για εικονικού δίκτυα που έχουν δημιουργηθεί με τη χρήση του μοντέλου κλασική ανάπτυξης. Προς το παρόν, όχι για όλες τις ρυθμίσεις παραμέτρων για πύλες είναι διαθέσιμες στην πύλη του Azure. Όταν είναι, θα δημιουργήσουμε ένα νέο σύνολο οδηγιών που ισχύουν για την πύλη του Azure.


1. [Δημιουργία μιας πύλης VPN για το VNet](#create-a-vpn-gateway)

1. [Συλλογή πληροφοριών για τη ρύθμιση παραμέτρων συσκευή VPN](#gather-information-for-your-vpn-device-configuration)

1. [Ρύθμιση παραμέτρων συσκευή VPN](#configure-your-vpn-device)

1. [Επαλήθευση του τοπικού δικτύου περιοχές και η διεύθυνση IP πύλης VPN](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### <a name="before-you-begin"></a>Πριν ξεκινήσετε

Πριν να ρυθμίσετε τις παραμέτρους της πύλης, πρέπει πρώτα να δημιουργήσετε το εικονικό δίκτυο. Για τα βήματα για να δημιουργήσετε ένα εικονικό δίκτυο για συνδεσιμότητα σταυρό εσωτερικής εγκατάστασης, ανατρέξτε στο θέμα [Ρύθμιση εικονικού δικτύου με μια σύνδεση VPN--τοποθεσίας](vpn-gateway-site-to-site-create.md)ή [Ρύθμιση εικονικού δικτύου με μια σύνδεση VPN σημείου σε τοποθεσία](vpn-gateway-point-to-site-create.md). Στη συνέχεια, χρησιμοποιήστε τα ακόλουθα βήματα για να ρυθμίσετε τις παραμέτρους της πύλης VPN και συγκέντρωση των πληροφοριών που χρειάζεστε για να ρυθμίσετε τη συσκευή σας VPN. 

Εάν έχετε ήδη μια πύλη VPN και θέλετε να αλλάξετε τον τύπο δρομολόγησης VPN, δείτε [πώς μπορείτε να αλλάξετε τον τύπο δρομολόγησης VPN για την πύλη](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>Δημιουργία μιας πύλης VPN

1. Στην [πύλη του Azure κλασική](https://manage.windowsazure.com), στη σελίδα **δίκτυα** , βεβαιωθείτε ότι η στήλη κατάσταση για το εικονικό δίκτυο είναι **δημιουργήθηκε**.

1. Στη στήλη **όνομα** , κάντε κλικ στο όνομα του εικονικού δικτύου σας.

1. Στη σελίδα **πίνακα εργαλείων** , παρατηρήστε ότι αυτό VNet δεν διαθέτει μια πύλη έχει ρυθμιστεί ακόμα. Θα δείτε αυτήν την κατάσταση καθώς προχωράτε με τα βήματα για να ρυθμίσετε τις παραμέτρους της πύλης.

![Η πύλη δεν δημιουργήθηκε](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)


Στη συνέχεια, στο κάτω μέρος της σελίδας, κάντε κλικ στην επιλογή **Δημιουργία πύλης**. Μπορείτε να επιλέξετε είτε *Στατική δρομολόγηση* ή *Δυναμική δρομολόγηση*. Ο τύπος δρομολόγησης VPN που επιλέγετε εξαρτάται από μερικούς παράγοντες. Για παράδειγμα, τι υποστηρίζει τη συσκευή σας VPN και αν χρειάζεστε για την υποστήριξη συνδέσεων σημείου σε τοποθεσία. Ελέγξτε για να επαληθεύσετε τον τύπο δρομολόγησης VPN που χρειάζεστε [Σχετικά με τις συσκευές VPN για εικονικού η συνδεσιμότητα του δικτύου](vpn-gateway-about-vpn-devices.md) . Μετά τη δημιουργία της πύλης, δεν μπορείτε να αλλάξετε μεταξύ πύλης VPN δρομολόγησης τύπων χωρίς τη διαγραφή και εκ νέου τη δημιουργία της πύλης. Όταν το σύστημα σάς ζητά να επιβεβαιώσετε ότι θέλετε η πύλη που δημιουργήσατε, κάντε κλικ στο κουμπί **Ναι**.

![Τύπος δρομολόγησης VPN πύλης](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Όταν δημιουργείτε την πύλη, παρατηρήστε το γραφικό πύλης στη σελίδα αλλάζει σε κίτρινο και εμφανίζεται η ένδειξη *Τη δημιουργία πύλης*. Ενδέχεται να χρειαστούν έως και 45 λεπτά για την πύλη για να δημιουργήσετε. Περιμένετε έως ότου η πύλη έχει ολοκληρωθεί πριν να μετακινήσετε προς τα εμπρός με άλλες ρυθμίσεις παραμέτρων.

![Δημιουργία πύλης](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Όταν η πύλη μετατραπεί σε *σύνδεση*, μπορείτε να συγκεντρώσετε τις πληροφορίες που θα χρειαστείτε για τη συσκευή σας VPN.

![Σύνδεση πύλης](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="gather-information-for-your-vpn-device-configuration"></a>Συλλογή πληροφοριών για τη ρύθμιση παραμέτρων συσκευή VPN

Μετά τη δημιουργία της πύλης, συλλογή πληροφοριών για τη ρύθμιση παραμέτρων συσκευή VPN. Αυτές οι πληροφορίες βρίσκεται στη σελίδα **πίνακα εργαλείων** για το εικονικό δίκτυο:

1. **Διεύθυνση IP πύλης-** Στη σελίδα **πίνακα εργαλείων** , μπορείτε να βρείτε τη διεύθυνση IP. Δεν θα μπορείτε να δείτε το μέχρι αφού ολοκληρωθεί η δημιουργία της πύλης.

1. **Κοινή χρήση αριθμού-κλειδιού-** Κάντε κλικ στην επιλογή **Διαχείριση κλειδιού** στο κάτω μέρος της οθόνης. Κάντε κλικ στο εικονίδιο δίπλα στο στοιχείο το κλειδί για να αντιγράψετε στο Πρόχειρο, και, στη συνέχεια, επικολλήστε και αποθηκεύσετε το κλειδί. Αυτό το κουμπί λειτουργεί μόνο όταν υπάρχει μια μεμονωμένη διοχέτευσης S2S VPN. Εάν έχετε πολλές σήραγγες S2S VPN, χρησιμοποιήστε το *Γρήγορα εικονικού δικτύου κοινόχρηστο κλειδί πύλης* API ή τα cmdlet του PowerShell.

![Διαχείριση αριθμού-κλειδιού](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)


## <a name="configure-your-vpn-device"></a>Ρύθμιση παραμέτρων συσκευή VPN

Αφού ολοκληρώσετε τα προηγούμενα βήματα, εσείς ή ο διαχειριστής δικτύου θα πρέπει να ρυθμίσετε τις παραμέτρους της συσκευής VPN προκειμένου να δημιουργήσετε τη σύνδεση. Για περισσότερες πληροφορίες σχετικά με τις συσκευές VPN, ανατρέξτε στο θέμα [Σχετικά με τις συσκευές VPN για εικονικού η συνδεσιμότητα του δικτύου](vpn-gateway-about-vpn-devices.md) .

Αφού έχει ρυθμιστεί η συσκευή VPN, μπορείτε να προβάλετε τις πληροφορίες σας ενημερωμένο σύνδεσης στη σελίδα πίνακα εργαλείων για το VNet.

Μπορείτε επίσης να εκτελέσετε μια από τις παρακάτω εντολές για να ελέγξετε τη σύνδεσή σας:

|                      | Cisco ASA             | Cisco ISR/ASR         | Juniper SSG/ISG | Juniper SRX/J                            |
|----------------------|-----------------------|-----------------------|-----------------|------------------------------------------|
| **Έλεγχος συσχετισμών ασφαλείας κύριας κατάστασης λειτουργίας**  | Εμφάνιση crypto isakmp σα | Εμφάνιση crypto isakmp σα | λήψη ike cookie  | Εμφάνιση ασφαλείας συσχετισμού ασφαλείας ike   |
| **Έλεγχος συσχετισμών ασφαλείας γρήγορης κατάστασης λειτουργίας** | Εμφάνιση crypto ασφαλείας IP σα  | Εμφάνιση crypto ασφαλείας IP σα  | λήψη σα          | Εμφάνιση συσχετισμό ασφαλείας ασφαλείας IP ασφαλείας |


## <a name="verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Επαλήθευση του τοπικού δικτύου περιοχές και η διεύθυνση IP πύλης VPN

### <a name="verify-your-vpn-gateway-ip-address"></a>Επιβεβαιώστε τη διεύθυνση IP του VPN πύλης

Για την πύλη για να συνδεθεί σωστά, τη διεύθυνση IP για τη συσκευή σας VPN πρέπει να ρυθμιστεί σωστά για το τοπικό δίκτυο που έχετε καθορίσει για τη ρύθμιση παραμέτρων σταυρό εσωτερικής εγκατάστασης. Συνήθως, αυτό έχει ρυθμιστεί κατά τη διαδικασία ρύθμισης παραμέτρων τοποθεσίας σε τοποθεσία. Ωστόσο, εάν είχατε χρησιμοποιήσει προηγουμένως αυτού του τοπικού δικτύου με μια διαφορετική συσκευή ή τη διεύθυνση IP έχει αλλάξει για αυτό το τοπικό δίκτυο, επεξεργαστείτε τις ρυθμίσεις για να καθορίσετε τη σωστή διεύθυνση IP πύλης.

1. Για να επαληθεύσετε τη διεύθυνση IP σας πύλης, κάντε κλικ στην επιλογή **δίκτυα** στο αριστερό παράθυρο πύλης και, στη συνέχεια, επιλέξτε **Τοπικά δίκτυα** στο επάνω μέρος της σελίδας. Θα δείτε τη διεύθυνση της πύλης VPN για κάθε τοπικό δίκτυο που έχετε δημιουργήσει. Για να επεξεργαστείτε τη διεύθυνση IP, επιλέξτε το VNet και κάντε κλικ στην επιλογή " **Επεξεργασία** " στο κάτω μέρος της σελίδας.

1. Στη σελίδα **Καθορισμός τις λεπτομέρειες του τοπικού δικτύου σας** , επεξεργαστείτε τη διεύθυνση IP και, στη συνέχεια, κάντε κλικ στο επόμενο βέλος στο κάτω μέρος της σελίδας.

1. Στη σελίδα **Καθορισμός το χώρο διευθύνσεων** , επιλέξτε το σημάδι επιλογής σε στην κάτω δεξιά γωνία για να αποθηκεύσετε τις ρυθμίσεις σας.

### <a name="verify-the-address-ranges-for-your-local-networks"></a>Επαληθεύστε τις περιοχές διευθύνσεων για τοπική δίκτυά σας

Για τη σωστή ροή κυκλοφορίας από την πύλη στη θέση σας στην εσωτερική εγκατάσταση, πρέπει να βεβαιωθείτε ότι κάθε διεύθυνση IP έχει καθοριστεί. Κάθε περιοχή πρέπει να αναφέρεται στη ρύθμιση παραμέτρων σας Azure **Τοπικά δίκτυα** . Ανάλογα με τη ρύθμιση παραμέτρων δικτύου σας θέση εσωτερικής εγκατάστασης, αυτό μπορεί να είναι κάπως μεγάλο εργασίας. Κίνηση που είναι δεσμευμένο μια διεύθυνση IP που περιέχεται μέσα στις περιοχές που παρατίθενται θα αποστέλλονται από την πύλη VPN εικονικού δικτύου. Περιοχές που λίστα που δεν χρειάζεται να είναι ιδιωτικά περιοχές, παρόλο που θα θέλετε να επαληθεύσετε ότι η ρύθμιση των παραμέτρων σας στην εσωτερική εγκατάσταση να λαμβάνετε την εισερχόμενη κυκλοφορία.

Για να προσθέσετε ή να επεξεργάζονται τις περιοχές για ένα τοπικό δίκτυο, χρησιμοποιήστε τα παρακάτω βήματα.

1. Για να επεξεργαστείτε τις περιοχές διευθύνσεων IP για ένα τοπικό δίκτυο, κάντε κλικ στην επιλογή **δίκτυα** στο αριστερό παράθυρο πύλης και κατόπιν επιλέξτε **Τοπικά δίκτυα** στο επάνω μέρος της σελίδας. Στην πύλη του, ο ευκολότερος τρόπος για να προβάλετε τις περιοχές που έχετε συμπεριλάβει στη λίστα είναι στη σελίδα " **Επεξεργασία** ". Για να δείτε τις περιοχές, επιλέξτε το VNet και κάντε κλικ στην επιλογή " **Επεξεργασία** " στο κάτω μέρος της σελίδας.

1. Στη σελίδα **Καθορισμός τις λεπτομέρειες του τοπικού δικτύου σας** , να μην γίνουν αλλαγές. Κάντε κλικ στο επόμενο βέλος στο κάτω μέρος της σελίδας.

1. Στη σελίδα **Καθορισμός το χώρο διευθύνσεων** , κάντε τις αλλαγές σας δίκτυο διεύθυνση χώρο. Στη συνέχεια, επιλέξτε το σημάδι επιλογής για να αποθηκεύσετε τη ρύθμιση των παραμέτρων σας.

## <a name="how-to-view-gateway-traffic"></a>Πώς μπορείτε να προβάλετε την κυκλοφορία πύλης

Μπορείτε να προβάλετε την πύλη και κίνηση πύλης από τη σελίδα εικονικού δικτύου **πίνακα εργαλείων** σας.

Στη σελίδα **πίνακα εργαλείων** μπορείτε να προβάλετε τα εξής:

- Η ποσότητα των δεδομένων που ρέει μέσω της πύλης, δεδομένα και δεδομένων.

- Τα ονόματα των διακομιστών DNS που έχουν καθοριστεί για το εικονικό δίκτυο.

- Η σύνδεση μεταξύ της πύλης και συσκευή VPN.

- Το κοινόχρηστο κλειδί που χρησιμοποιείται για να ρυθμίσετε τη σύνδεση πύλης VPN στη συσκευή σας.


## <a name="how-to-change-the-vpn-routing-type-for-your-gateway"></a>Πώς μπορείτε να αλλάξετε τον τύπο δρομολόγησης VPN για την πύλη

Γιατί ορισμένες ρυθμίσεις παραμέτρων συνδεσιμότητας είναι διαθέσιμες μόνο για συγκεκριμένους τύπους δρομολόγησης πύλης, μπορεί να διαπιστώσετε ότι πρέπει να αλλάξετε την πύλη VPN δρομολόγησης τύπος μια υπάρχουσα πύλης VPN. Για παράδειγμα, μπορεί να θέλετε να προσθέσετε τη σύνδεση σημείου σε τοποθεσία σε μια ήδη υπάρχουσα σύνδεση τοποθεσίας σε τοποθεσία που περιλαμβάνει μια στατική πύλη. Οι συνδέσεις σημείου σε τοποθεσία απαιτούν μια δυναμική πύλη. Αυτό σημαίνει ότι για να ρυθμίσετε μια σύνδεση P2S, θα πρέπει να αλλάξετε την πύλη VPN δρομολόγησης τύπο από στατική σε δυναμικό.

Εάν θέλετε να αλλάξετε μια πύλη VPN δρομολόγησης τύπου, που θα διαγράψετε την υπάρχουσα πύλη και, στη συνέχεια, να δημιουργήσετε μια νέα πύλη με τον νέο τύπο δρομολόγησης. Δεν χρειάζεται να διαγράψετε ολόκληρο το εικονικό δίκτυο για να αλλάξετε τον τύπο δρομολόγησης πύλη.

Πριν να αλλάξετε την πύλη VPN δρομολόγησης τύπο, θα πρέπει να βεβαιωθείτε ότι η συσκευή σας VPN υποστηρίζει τον τύπο δρομολόγησης που θέλετε να χρησιμοποιήσετε. Για να κάνετε λήψη νέων δρομολόγησης δειγμάτων ρύθμισης παραμέτρων και έλεγχος απαιτήσεων συσκευή VPN, ανατρέξτε στο θέμα [Σχετικά με τις συσκευές VPN για εικονικού η συνδεσιμότητα του δικτύου](vpn-gateway-about-vpn-devices.md).

>[AZURE.IMPORTANT] Όταν διαγράφετε μια πύλη VPN εικονικού δικτύου, κυκλοφορήσει το VIP που έχουν εκχωρηθεί για την πύλη. Όταν δημιουργήσετε την πύλη, μια νέα VIP έχει εκχωρηθεί σε αυτόν.

1. **Διαγράψτε το υπάρχον πύλη VPN.**

    Στη σελίδα **πίνακα εργαλείων** για το εικονικό δίκτυο, μεταβείτε στο κάτω μέρος της σελίδας και κάντε κλικ στην επιλογή **Διαγραφή πύλης**. Περιμένετε για την ειδοποίηση ότι η πύλη έχει διαγραφεί. Όταν λάβετε την ειδοποίηση στην οθόνη ότι η πύλη έχει διαγραφεί, μπορείτε να δημιουργήσετε μια νέα πύλη.

1. **Δημιουργήστε μια νέα πύλη VPN.**

    Χρησιμοποιήστε τη διαδικασία στο επάνω μέρος της σελίδας για να δημιουργήσετε μια νέα πύλη: [Δημιουργία πύλης VPN](#create-a-vpn-gateway).


## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να προσθέσετε εικονικές μηχανές εικονικού δικτύου σας. Δείτε [πώς μπορείτε να δημιουργήσετε μια προσαρμοσμένη εικονική μηχανή](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Εάν θέλετε να ρυθμίσετε μια σύνδεση VPN σημείου σε τοποθεσία, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μια σύνδεση VPN σημείου σε τοποθεσία](vpn-gateway-point-to-site-create.md).

 
