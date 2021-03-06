<properties
   pageTitle="Ρύθμιση παραμέτρων σύνδεσης VPN σημείου σε τοποθεσία πύλης σε ένα δίκτυο εικονικού Azure με την πύλη κλασική | Microsoft Azure"
   description="Ασφαλή σύνδεση με το δίκτυο εικονικού Azure, δημιουργώντας μια σύνδεση VPN σημείου σε τοποθεσία πύλης."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Ρύθμιση παραμέτρων μια σύνδεση σημείου σε τοποθεσία με μια VNet με την πύλη κλασική

> [AZURE.SELECTOR]
- [Διαχείριση πόρων - Azure πύλη](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Διαχείριση πόρων - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Κλασικό - Azure πύλη](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Κλασικό - κλασική πύλη](vpn-gateway-point-to-site-create.md)

Μια ρύθμιση παραμέτρων σημείου σε τοποθεσία (P2S) σάς επιτρέπει να δημιουργήσετε μια ασφαλή σύνδεση από έναν υπολογιστή-πελάτη μεμονωμένα σε εικονικό δίκτυο. Μια σύνδεση P2S είναι χρήσιμη όταν θέλετε να συνδεθείτε με το VNet από μια απομακρυσμένη θέση, όπως από το σπίτι ή ένα Συνέδριο, ή όταν έχετε μόνο ορισμένα προγράμματα-πελάτες που πρέπει να συνδεθείτε με ένα εικονικό δίκτυο.

Σε αυτό το άρθρο σάς καθοδηγεί στη δημιουργία ενός VNet με μια σύνδεση σημείου σε τοποθεσία στο **μοντέλο κλασική ανάπτυξης** με την **πύλη κλασική**.

Συνδέσεις σημείου σε τοποθεσία δεν απαιτούν μια συσκευή VPN ή μια διεύθυνση IP δημόσιας ώστε να λειτουργεί. Δημιουργείται μια σύνδεση VPN από την αρχική τη σύνδεση του υπολογιστή-πελάτη. Για περισσότερες πληροφορίες σχετικά με τις συνδέσεις σημείου σε τοποθεσία, ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις πύλης VPN](vpn-gateway-vpn-faq.md#point-to-site-connections) και [Προγραμματισμός και σχεδίαση](vpn-gateway-plan-design.md).


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Ανάπτυξη μοντέλων και μέθοδοι για P2S συνδέσεις

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Ο παρακάτω πίνακας εμφανίζει τα δύο μοντέλων ανάπτυξης και οι μέθοδοι διαθέσιμη ανάπτυξης για διαμορφώσεις P2S. Όταν ένα άρθρο με βήματα ρύθμισης παραμέτρων είναι διαθέσιμη, θα σας σύνδεση απευθείας σε αυτήν από αυτόν τον πίνακα.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 



## <a name="basic-workflow"></a>Βασική ροής εργασίας

![Σημείου-σε--διάγραμμα τοποθεσίας] (./media/vpn-gateway-point-to-site-create/p2sclassic.png "σημείο σε τοποθεσία")
 
Ακολουθήστε τα παρακάτω βήματα θα σας καθοδηγήσουν τα βήματα για να δημιουργήσετε μια ασφαλή σύνδεση σημείου σε τοποθεσία με ένα εικονικό δίκτυο. 

Η ρύθμιση παραμέτρων για μια σύνδεση σημείου σε τοποθεσία αναλύεται σε τέσσερις ενότητες. Η σειρά με την οποία ρυθμίζετε τις παραμέτρους κάθε μία από αυτές τις ενότητες είναι σημαντική. Μην παραλείψετε τα βήματα ή μετάβαση προς τα εμπρός.

- **Ενότητα 1** Δημιουργία εικονικού δικτύου και πύλη VPN.
- **Ενότητα 2** Δημιουργήστε τα πιστοποιητικά που χρησιμοποιούνται για τον έλεγχο ταυτότητας και στείλτε τους.
- **Ενότητα 3** Εξαγάγετε και να εγκαταστήσετε τα πιστοποιητικά προγράμματος-πελάτη σας.
- **Τμήμα 4** Ρυθμίστε τις παραμέτρους του υπολογιστή-πελάτη VPN.

## <a name="vnetvpn"></a>Τμήμα 1 - Δημιουργήστε ένα εικονικό δίκτυο και μιας πύλης VPN

### <a name="part-1-create-a-virtual-network"></a>Μέρος 1: Δημιουργήστε ένα εικονικό δίκτυο

1. Συνδεθείτε στο [Azure κλασική πύλη](https://manage.windowsazure.com/). Αυτά τα βήματα χρησιμοποιούν την πύλη κλασική, όχι την πύλη του Azure. Προς το παρόν, δεν μπορείτε να δημιουργήσετε μια σύνδεση P2S με την πύλη Azure.

2. Στην κάτω αριστερή γωνία της οθόνης, κάντε κλικ στην επιλογή **Δημιουργία**. Στο παράθυρο περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσίες δικτύου**και, στη συνέχεια, κάντε κλικ στην επιλογή **Εικονικού δικτύου**. Κάντε κλικ στην επιλογή **Δημιουργία προσαρμοσμένης** για να ξεκινήσετε τον Οδηγό ρύθμισης παραμέτρων.

3. Στη σελίδα **Λεπτομέρειες εικονικού δικτύου** , εισαγάγετε τις παρακάτω πληροφορίες και, στη συνέχεια, κάντε κλικ στο επόμενο βέλος στην κάτω δεξιά γωνία.
    - **Όνομα**: όνομα εικονικού δικτύου σας. Για παράδειγμα, 'VNet1'. Αυτό είναι το όνομα που θα αναφέρονται σε κατά την ανάπτυξη ΣΠΣ στη αυτό VNet.
    - **Θέση**: Η θέση συνδέεται απευθείας με τη φυσική θέση (περιοχή) όπου θέλετε να τους πόρους σας (ΣΠΣ) να βρίσκεται. Για παράδειγμα, εάν θέλετε το ΣΠΣ που αναπτύσσετε σε αυτό το δίκτυο εικονικού φυσικά βρίσκονται στην Ανατολικής η.π.α., επιλέξτε αυτήν τη θέση. Δεν μπορείτε να αλλάξετε την περιοχή που σχετίζεται με το δίκτυό σας εικονικού μετά τη δημιουργία του.

4. Στη σελίδα **οι διακομιστές DNS και τη σύνδεση VPN** , εισαγάγετε τις παρακάτω πληροφορίες και, στη συνέχεια, κάντε κλικ στο επόμενο βέλος στην κάτω δεξιά γωνία.
    - **Οι διακομιστές DNS**: Πληκτρολογήστε το όνομα του διακομιστή DNS και τη διεύθυνση IP ή επιλέξτε ένα διακομιστή DNS που έχει ήδη καταχωρηθεί από το μενού συντόμευσης. Αυτή η ρύθμιση δεν δημιουργεί ένα διακομιστή DNS. Σας επιτρέπει να καθορίσετε τους διακομιστές DNS που θέλετε να χρησιμοποιήσετε για την επίλυση ονομάτων για αυτό το εικονικό δίκτυο. Εάν θέλετε να χρησιμοποιήσετε την υπηρεσία Azure προεπιλεγμένο όνομα ανάλυση, αφήστε κενό αυτήν την ενότητα.
    - **Ρύθμιση παραμέτρων σημείου σε τοποθεσία VPN**: Επιλέξτε το πλαίσιο ελέγχου.

5. Στη σελίδα **Σύνδεση σημείου σε τοποθεσία** , καθορίστε την περιοχή διευθύνσεων IP από το οποίο τους πελάτες σας VPN θα λάβουν μια διεύθυνση IP κατά τη σύνδεση. Υπάρχουν μερικά κανόνες σχετικά με τις περιοχές διευθύνσεων που μπορείτε να καθορίσετε. Είναι σημαντικό για να επιβεβαιώσετε ότι η περιοχή που έχετε καθορίσει δεν επικαλύπτει με οποιαδήποτε από τις περιοχές που βρίσκεται στο δίκτυό σας εσωτερικής εγκατάστασης.

6. Εισαγάγετε τις παρακάτω πληροφορίες και, στη συνέχεια, κάντε κλικ στο βέλος Επόμενο.
 - **Χώρο διευθύνσεων**: συμπεριλάβετε το αρχικό IP και CIDR (διεύθυνση μέτρηση).
 - **Προσθήκη χώρου διευθύνσεων**: Προσθήκη χώρου διευθύνσεων μόνο εάν είναι απαραίτητη για το σχεδιασμό του δικτύου σας.

7. Στη σελίδα **Εικονικού κενά διαστήματα διεύθυνση δικτύου** , καθορίστε την περιοχή διευθύνσεων που θέλετε να χρησιμοποιήσετε για το εικονικό δίκτυο. Αυτά είναι τα δυναμικές διευθύνσεις IP (ΠΤΏΣΕΙΣ) που εκχωρούνται σε του ΣΠΣ και άλλες εμφανίσεις ρόλο που αναπτύσσετε σε αυτό το εικονικό δίκτυο.<br><br>Είναι ιδιαίτερα σημαντικό για να επιλέξετε μια περιοχή που να μην επικαλύπτει με οποιαδήποτε από τις περιοχές που χρησιμοποιούνται για το δίκτυο εσωτερικής εγκατάστασης. Πρέπει να συντονισμό με το διαχειριστή του δικτύου σας, που ίσως χρειαστεί να carve ανάληψη μια περιοχή διευθύνσεων IP από το χώρο σας εσωτερικής εγκατάστασης δικτύου διεύθυνση για να χρησιμοποιήσετε για το εικονικό δίκτυο.

8. Εισαγάγετε τις παρακάτω πληροφορίες και, στη συνέχεια, κάντε κλικ στην επιλογή το σημάδι επιλογής για να ξεκινήσετε τη δημιουργία εικονικών το δίκτυό σας.
 - **Χώρο διευθύνσεων**: Προσθέστε το εσωτερικό διεύθυνση IP που θέλετε να χρησιμοποιήσετε για αυτό το εικονικό δίκτυο, συμπεριλαμβανομένων ξεκινώντας IP και πλήθος. Είναι σημαντικό για να επιλέξετε μια περιοχή που να μην επικαλύπτει με οποιαδήποτε από τις περιοχές που χρησιμοποιούνται για το δίκτυο εσωτερικής εγκατάστασης. 
 - **Προσθήκη υποδικτύου**: επιπλέον δευτερεύοντα δίκτυα δεν είναι απαραίτητο, αλλά μπορεί να θέλετε να δημιουργήσετε ένα ξεχωριστό υποδίκτυο για ΣΠΣ που θα έχει στατική ΠΤΏΣΕΙΣ. Ή μπορείτε να έχετε ΣΠΣ σας σε ένα υποδίκτυο που είναι ξεχωριστό από τις άλλες εμφανίσεις ρόλο.
 - **Προσθήκη πύλης υποδικτύου**: το υποδίκτυο πύλης απαιτείται για ένα VPN σημείου σε τοποθεσία. Κάντε κλικ για να προσθέσετε το υποδίκτυο πύλης. Το υποδίκτυο πύλης χρησιμοποιείται μόνο για την πύλη εικονικού δικτύου.

9. Μετά τη δημιουργία εικονικών το δίκτυό σας, θα δείτε **δημιουργήθηκε** που αναφέρονται στην περιοχή **κατάστασης** στη σελίδα δίκτυα στην πύλη του Azure κλασική. Μόλις δημιουργηθεί το εικονικό δίκτυο, μπορείτε να δημιουργήσετε την δυναμική πύλη δρομολόγησης.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Μέρος 2: Δημιουργήστε μια δυναμική πύλη δρομολόγησης

Ο τύπος πύλης πρέπει να ρυθμιστεί ως δυναμικό. Στατική δρομολόγηση πυλών δεν λειτουργούν με αυτήν τη δυνατότητα.

1. Στην κλασική πύλη Azure, στη σελίδα **δίκτυα** , κάντε κλικ στην επιλογή το εικονικό δίκτυο που δημιουργήσατε και μεταβείτε στη σελίδα **πίνακα εργαλείων** .

2. Κάντε κλικ στην επιλογή **Δημιουργία πύλης**, που βρίσκεται στο κάτω μέρος της σελίδας **πίνακα εργαλείων** . Θα εμφανιστεί ένα μήνυμα που σας ρωτά **θέλετε να δημιουργήσετε μια πύλη για εικονικού δικτύου "VNet1"**. Κάντε κλικ στο κουμπί **Ναι** για να ξεκινήσετε τη δημιουργία της πύλης. Μπορεί να χρειαστούν περίπου 15 λεπτά για την πύλη για να δημιουργήσετε.

## <a name="generate"></a>Ενότητα 2 - Δημιουργία και αποστολή πιστοποιητικών

Τα πιστοποιητικά που χρησιμοποιούνται για τον έλεγχο ταυτότητας προγραμμάτων-πελατών VPN για VPN σημείου σε τοποθεσία. Μπορείτε να χρησιμοποιήσετε ένα πιστοποιητικό ρίζας που δημιουργούνται από μια λύση πιστοποιητικό για μεγάλες επιχειρήσεις ή μπορείτε να χρησιμοποιήσετε ένα αυτο-υπογεγραμμένο πιστοποιητικό. Μπορείτε να αποστείλετε έως 20 πιστοποιητικών ρίζας στο Azure. Εφόσον το αρχείο .cer αποσταλεί, Azure να χρησιμοποιήσετε τις πληροφορίες που περιέχονται σε αυτό για τον έλεγχο ταυτότητας προγράμματα-πελάτες που έχουν εγκαταστήσει ένα πιστοποιητικό προγράμματος-πελάτη. Το πιστοποιητικό προγράμματος-πελάτη πρέπει να έχει δημιουργηθεί από το ίδιο πιστοποιητικό που αντιπροσωπεύει το αρχείο .cer.

Σε αυτήν την ενότητα που θα κάντε τα εξής:

- Αποκτήστε το αρχείο .cer πιστοποιητικού ρίζας. Αυτό μπορεί να είναι είτε ένα αυτο-υπογεγραμμένο πιστοποιητικό ή μπορείτε να χρησιμοποιήσετε το σύστημα πιστοποιητικό για μεγάλες επιχειρήσεις.
- Αποστείλετε το αρχείο .cer Azure.
- Δημιουργία πιστοποιητικών προγράμματος-πελάτη.

### <a name="root"></a>Μέρος 1: Λήψη το αρχείο .cer για το πιστοποιητικό ρίζας

Εάν χρησιμοποιείτε ένα σύστημα πιστοποιητικό για μεγάλες επιχειρήσεις, αποκτήστε το αρχείο .cer του πιστοποιητικού ρίζας που θέλετε να χρησιμοποιήσετε. Στο [τμήμα 3](#createclientcert), μπορείτε να δημιουργήσετε τα πιστοποιητικά προγράμματος-πελάτη από το πιστοποιητικό ρίζας.

Εάν δεν χρησιμοποιείτε μια λύση πιστοποιητικό για μεγάλες επιχειρήσεις, θα πρέπει να δημιουργήσετε ένα πιστοποιητικό αυτόματης υπογραφής ρίζας. Για τα Windows 10, μπορείτε να ανατρέξετε στην [εργασία με πιστοποιητικών ρίζας αυτο-υπογεγραμμένο για ρυθμίσεις παραμέτρων σημείου σε τοποθεσία](vpn-gateway-certificates-point-to-site.md). Το άρθρο σάς καθοδηγεί σε χρησιμοποιώντας makecert για να δημιουργήσετε ένα αυτο-υπογεγραμμένο πιστοποιητικό και, στη συνέχεια, εξαγωγή το αρχείο .cer.

### <a name="upload"></a>Μέρος 2: Αποστολή το αρχείο .cer πιστοποιητικού ρίζας στην πύλη του Azure κλασική

Προσθέστε ένα αξιόπιστο πιστοποιητικό Azure. Όταν προσθέτετε ένα αρχείο κωδικοποίηση Base64 X.509 (.cer) για να Azure, δίνετε Azure αξιόπιστο το πιστοποιητικό ρίζας που αντιπροσωπεύει το αρχείο.

1. Στην κλασική πύλη Azure, στη σελίδα **πιστοποιητικά** για το δίκτυό σας εικονικού, κάντε κλικ στην επιλογή **Αποστολή ενός πιστοποιητικού ρίζας**.

2. Στη σελίδα **Αποστολή πιστοποιητικού** , αναζητήστε το πιστοποιητικό ρίζας .cer και, στη συνέχεια, κάντε κλικ στην επιλογή το σημάδι ελέγχου.

### <a name="createclientcert"></a>Μέρος 3: Δημιουργία ενός πιστοποιητικού προγράμματος-πελάτη

Στη συνέχεια, να δημιουργήσετε τα πιστοποιητικά προγράμματος-πελάτη. Μπορείτε να δημιουργήσετε είτε ένα μοναδικό πιστοποιητικό για κάθε πελάτη που θα συνδεθεί ή μπορείτε να χρησιμοποιήσετε το ίδιο πιστοποιητικό σε πολλά προγράμματα-πελάτες. Το πλεονέκτημα δημιουργό πιστοποιητικά μοναδικό προγράμματος-πελάτη είναι η δυνατότητα να ανακαλέσετε ενός πιστοποιητικού, εάν είναι απαραίτητο. Διαφορετικά, εάν όλοι χρησιμοποιεί το ίδιο πιστοποιητικό προγράμματος-πελάτη και μπορείτε να βρείτε ότι πρέπει να ανακαλέσετε το πιστοποιητικό για έναν υπολογιστή-πελάτη, θα πρέπει να δημιουργήσετε και να εγκαταστήσετε νέα πιστοποιητικά για όλα τα προγράμματα-πελάτες που χρησιμοποιούν το πιστοποιητικό για τον έλεγχο ταυτότητας.

- Εάν χρησιμοποιείτε μια λύση πιστοποιητικό για μεγάλες επιχειρήσεις, να δημιουργήσετε ένα πιστοποιητικό προγράμματος-πελάτη με την συνηθισμένη μορφή τιμή όνομα 'name@yourdomain.com', και όχι το NetBIOS 'Τομέας\όνομα χρήστη' μορφοποίηση. 

- Εάν χρησιμοποιείτε ένα πιστοποιητικό αυτόματης υπογραφής, ανατρέξτε στο θέμα [εργασία με πιστοποιητικών ρίζας αυτο-υπογεγραμμένο για ρυθμίσεις παραμέτρων σημείου σε τοποθεσία](vpn-gateway-certificates-point-to-site.md) για να δημιουργήσετε ένα πιστοποιητικό προγράμματος-πελάτη.

## <a name="installclientcert"></a>Ενότητα 3 - εξαγωγή και εγκαταστήστε το πιστοποιητικό προγράμματος-πελάτη

Εγκατάσταση πιστοποιητικού προγράμματος-πελάτη σε κάθε υπολογιστή στον οποίο θέλετε να συνδεθείτε με το εικονικό δίκτυο. Απαιτείται ένα πιστοποιητικό προγράμματος-πελάτη για έλεγχο ταυτότητας. Μπορείτε να αυτοματοποιήσετε κατά την εγκατάσταση του προγράμματος-πελάτη πιστοποιητικού ή μπορείτε να εγκαταστήσετε με μη αυτόματο τρόπο. Τα παρακάτω βήματα θα σας καθοδηγήσει εξαγωγή και την εγκατάσταση του πιστοποιητικού προγράμματος-πελάτη με μη αυτόματο τρόπο.

1. Για να εξαγάγετε ένα πιστοποιητικό προγράμματος-πελάτη, μπορείτε να χρησιμοποιήσετε *certmgr.msc*. Κάντε δεξί κλικ στο πιστοποιητικό προγράμματος-πελάτη που θέλετε να εξαγάγετε, κάντε κλικ στην επιλογή **όλες τις εργασίες**και, στη συνέχεια, κάντε κλικ στην επιλογή **Εξαγωγή**.
2. Εξαγάγετε το πιστοποιητικό προγράμματος-πελάτη με το ιδιωτικό κλειδί. Αυτό είναι ένα αρχείο *.pfx* . Βεβαιωθείτε ότι η εγγραφή ή να θυμηθείτε τον κωδικό πρόσβασης (πλήκτρο) που έχετε ορίσει για αυτό το πιστοποιητικό.
3. Αντιγράψτε το αρχείο *.pfx* στον υπολογιστή-πελάτη. Στον υπολογιστή-πελάτη, κάντε διπλό κλικ το αρχείο *.pfx* να το εγκαταστήσετε. Πληκτρολογήστε τον κωδικό πρόσβασης όταν σας ζητηθεί. Μην τροποποιείτε τη θέση εγκατάστασης.

## <a name="vpnclientconfig"></a>Ενότητα 4 - ρύθμιση παραμέτρων του υπολογιστή-πελάτη VPN

Για να συνδεθείτε στο δίκτυο εικονικού, πρέπει επίσης να ρυθμίσετε τις παραμέτρους ενός υπολογιστή-πελάτη VPN. Το πρόγραμμα-πελάτη απαιτεί ένα πιστοποιητικό προγράμματος-πελάτη και τη σωστή ρύθμιση παραμέτρων προγράμματος-πελάτη VPN προκειμένου να συνδέσετε. Για να ρυθμίσετε τις παραμέτρους ενός υπολογιστή-πελάτη VPN, εκτελέστε τα παρακάτω βήματα, με τη σειρά.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Μέρος 1: Δημιουργία του πακέτου ρύθμισης παραμέτρων προγράμματος-πελάτη VPN

1. Στην κλασική πύλη Azure, στη σελίδα **πίνακα εργαλείων** για το δίκτυό σας εικονικού, μεταβείτε στο μενού γρήγορη ματιά στη δεξιά γωνία. Για τη λίστα του προγράμματος-πελάτη λειτουργικά συστήματα που υποστηρίζονται, ανατρέξτε στο θέμα τις συνδέσεις [σημείο σε τοποθεσία](vpn-gateway-vpn-faq.md#point-to-site-connections) ενότητα Συνήθεις Ερωτήσεις του VPN πύλης. Το πακέτο του προγράμματος-πελάτη VPN περιέχει πληροφορίες ρύθμισης παραμέτρων για τη ρύθμιση παραμέτρων του λογισμικού προγράμματος-πελάτη VPN ενσωματωμένη στα Windows. Το πακέτο δεν εγκαθιστά πρόσθετο λογισμικό. Τις ρυθμίσεις αφορούν ειδικά το εικονικό δίκτυο που θέλετε να συνδεθείτε.<br><br>Επιλέξτε τη λήψη του πακέτου που αντιστοιχεί στο λειτουργικό σύστημα υπολογιστή-πελάτη στον οποίο πρόκειται να εγκατασταθεί:
 - Για προγράμματα-πελάτες 32-bit, επιλέξτε **Άμεση λήψη του πακέτου 32 bit προγράμματος-πελάτη VPN**.
 - Για προγράμματα-πελάτες 64-bit, επιλέξτε **Άμεση λήψη του πακέτου 64-bit προγράμματος-πελάτη VPN**.

2. Χρειάζονται μερικά λεπτά για να δημιουργήσετε το πακέτο του προγράμματος-πελάτη σας. Όταν το πακέτο έχει ολοκληρωθεί, μπορείτε να κάνετε λήψη του αρχείου. Το αρχείο *.exe* που λαμβάνετε μπορούν να αποθηκευτούν με ασφάλεια στον τοπικό σας υπολογιστή.

3. Μετά τη δημιουργία και κάντε λήψη του πακέτου προγράμματος-πελάτη VPN από την πύλη του Azure κλασική, μπορείτε να εγκαταστήσετε το πακέτο του προγράμματος-πελάτη του υπολογιστή-πελάτη από την οποία θέλετε να συνδεθείτε στο δίκτυο του εικονικού σας. Εάν σκοπεύετε να εγκαταστήσετε το πακέτο του προγράμματος-πελάτη VPN σε πολλούς υπολογιστές-πελάτες, βεβαιωθείτε ότι κάθε έχουν εγκατεστημένο πιστοποιητικό προγράμματος-πελάτη.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Μέρος 2: Εγκατάσταση του πακέτου VPN ρύθμισης παραμέτρων του υπολογιστή-πελάτη

1. Αντιγράψτε το αρχείο ρύθμισης παραμέτρων τοπικά στον υπολογιστή που θέλετε να συνδεθείτε με το δίκτυό σας εικονικού και κάντε διπλό κλικ στο αρχείο .exe. 

2. Όταν το πακέτο έχει εγκατασταθεί, μπορείτε να ξεκινήσετε τη σύνδεση VPN. Το πακέτο ρύθμισης παραμέτρων δεν είναι υπογεγραμμένο από τη Microsoft. Ενδέχεται να θέλετε να υπογράψετε το πακέτο χρησιμοποιώντας την υπηρεσία υπογραφής της εταιρείας σας ή συνδεθείτε στον εαυτό σας με χρήση [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Είναι OK για να χρησιμοποιήσετε το πακέτο χωρίς να πραγματοποιήσουν είσοδο. Ωστόσο, εάν το πακέτο δεν είναι συνδεδεμένος, εμφανίζεται μια προειδοποίηση όταν εγκαθιστάτε το πακέτο.

3. Στον υπολογιστή-πελάτη, μεταβείτε στις **Ρυθμίσεις δικτύου** και κάντε κλικ στην επιλογή **VPN**. Θα δείτε τη σύνδεση που παρατίθενται. Θα εμφανίζεται το όνομα του εικονικού δικτύου που θα συνδεθείτε και θα είναι παρόμοιο με αυτό: 

    ![Υπολογιστή-πελάτη VPN] (./media/vpn-gateway-point-to-site-create/vpn.png "Υπολογιστή-πελάτη VPN")


### <a name="part-3-connect-to-azure"></a>Μέρος 3: Σύνδεση με Azure

1. Για να συνδεθείτε με το VNet, στον υπολογιστή-πελάτη, μεταβείτε στο συνδέσεις VPN και εντοπίστε τη σύνδεση VPN που δημιουργήσατε. Ονομάζεται το ίδιο όνομα με το εικονικό δίκτυο. Κάντε κλικ στην επιλογή **σύνδεση**. Ενδέχεται να εμφανιστεί ένα αναδυόμενο μήνυμα που αναφέρεται σε χρησιμοποιώντας το πιστοποιητικό. Αν συμβεί αυτό, κάντε κλικ στο κουμπί **συνέχεια** για να χρησιμοποιήσετε αναβαθμισμένα δικαιώματα. 

2. Στη σελίδα κατάστασης **σύνδεσης** , κάντε κλικ στην επιλογή **σύνδεση** για να ξεκινήσετε τη σύνδεση. Εάν εμφανιστεί μια οθόνη **Επιλογή πιστοποιητικού** , βεβαιωθείτε ότι το πιστοποιητικό προγράμματος-πελάτη που εμφανίζει είναι αυτό που θέλετε να χρησιμοποιήσετε για να συνδεθείτε. Εάν δεν είναι, χρησιμοποιήστε το αναπτυσσόμενο βέλος για να επιλέξετε το σωστό πιστοποιητικό και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

    ![Υπολογιστή-πελάτη VPN 2] (./media/vpn-gateway-point-to-site-create/clientconnect.png "Σύνδεση του υπολογιστή-πελάτη VPN")

3. Σας θα πρέπει τώρα δυνατή η σύνδεση.

    ![Υπολογιστή-πελάτη VPN 3] (./media/vpn-gateway-point-to-site-create/connected.png "Σύνδεση του υπολογιστή-πελάτη VPN 2")

### <a name="part-4-verify-the-vpn-connection"></a>Μέρος 4: Επαληθεύστε τη σύνδεση VPN

1. Για να επαληθεύσετε ότι τη σύνδεση VPN είναι ενεργό, ανοίξτε μια γραμμή εντολών με αναβαθμισμένα δικαιώματα και εκτελέστε *ipconfig/all*.
2. Προβολή των αποτελεσμάτων. Παρατηρήστε ότι η διεύθυνση IP που λάβατε είναι μία από τις διευθύνσεις μέσα στην περιοχή διεύθυνση συνδεσιμότητας σημείου σε τοποθεσία που καθορίσατε κατά τη δημιουργία του VNet. Τα αποτελέσματα θα πρέπει να είναι κάτι παρόμοιο με αυτό:

Παράδειγμα:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να προσθέσετε εικονικές μηχανές εικονικού δικτύου σας. Δείτε [πώς μπορείτε να δημιουργήσετε μια προσαρμοσμένη εικονική μηχανή](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Εάν θέλετε περισσότερες πληροφορίες σχετικά με τα δίκτυα εικονικού, ανατρέξτε στη σελίδα [Εικονικού τεκμηρίωση του δικτύου](https://azure.microsoft.com/documentation/services/virtual-network/) .
