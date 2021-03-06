<properties 
   pageTitle="Ρύθμιση παραμέτρων σύνδεσης VPN σημείου σε τοποθεσία πύλης εικονικού δικτύου, χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων και την πύλη του Azure | Microsoft Azure"
   description="Ασφαλή σύνδεση με το δίκτυο εικονικού Azure, δημιουργώντας σύνδεση VPN σημείου σε τοποθεσία πύλης χρήση της διαχείρισης πόρων και την πύλη του Azure."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Ρύθμιση παραμέτρων μια σύνδεση σημείου σε τοποθεσία με μια VNet με την πύλη Azure

> [AZURE.SELECTOR]
- [Διαχείριση πόρων - Azure πύλη](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Διαχείριση πόρων - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Κλασικό - Azure πύλη](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Μια ρύθμιση παραμέτρων σημείου σε τοποθεσία (P2S) σάς επιτρέπει να δημιουργήσετε μια ασφαλή σύνδεση από έναν υπολογιστή-πελάτη μεμονωμένα σε εικονικό δίκτυο. Μια σύνδεση P2S είναι χρήσιμη όταν θέλετε να συνδεθείτε με το VNet από μια απομακρυσμένη θέση, όπως από το σπίτι ή ένα Συνέδριο, ή όταν έχετε μόνο ορισμένα προγράμματα-πελάτες που πρέπει να συνδεθείτε με ένα εικονικό δίκτυο. 

Συνδέσεις σημείου σε τοποθεσία δεν απαιτούν μια συσκευή VPN ή μια διεύθυνση IP δημόσιας ώστε να λειτουργεί. Δημιουργείται μια σύνδεση VPN από την αρχική τη σύνδεση του υπολογιστή-πελάτη. Για περισσότερες πληροφορίες σχετικά με τις συνδέσεις σημείου σε τοποθεσία, ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις πύλης VPN](vpn-gateway-vpn-faq.md#point-to-site-connections) και [Προγραμματισμός και σχεδίαση](vpn-gateway-plan-design.md).

Σε αυτό το άρθρο σάς καθοδηγεί στη δημιουργία ενός VNet με μια σύνδεση σημείου σε τοποθεσία του μοντέλου ανάπτυξης για τη διαχείριση πόρων με την πύλη Azure.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Μοντέλα ανάπτυξης και οι μέθοδοι για τις συνδέσεις P2S

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Ο παρακάτω πίνακας εμφανίζει τα δύο μοντέλων ανάπτυξης και οι μέθοδοι διαθέσιμη ανάπτυξης για διαμορφώσεις P2S. Όταν ένα άρθρο με βήματα ρύθμισης παραμέτρων είναι διαθέσιμη, θα σας σύνδεση απευθείας σε αυτήν από αυτόν τον πίνακα.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Βασική ροής εργασίας 

![Σημείου-σε--διάγραμμα τοποθεσίας] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "σημείο σε τοποθεσία")

### <a name="example"></a>Παραδείγματα τιμών

- **Όνομα: VNet1**
- **Διεύθυνση χώρου: 192.168.0.0/16**<br>Για αυτό το παράδειγμα, χρησιμοποιούμε χώρο μόνο μία διεύθυνση. Μπορείτε να έχετε περισσότερα από ένα χώρο διευθύνσεων για το VNet.
- **Όνομα δευτερεύοντος δικτύου: FrontEnd**
- **Περιοχή υποδικτύου διευθύνσεων: 192.168.1.0/24**
- **Συνδρομή:** Εάν έχετε περισσότερες από μία συνδρομές, βεβαιωθείτε ότι χρησιμοποιείτε το σωστό αρχείο.
- **Ομάδα πόρων: TestRG**
- **Θέση: Ανατολικής η.π.α.**
- **GatewaySubnet: 192.168.200.0/24**
- **Όνομα πύλης εικονικού δικτύου: VNet1GW**
- **Τύπος πύλης: VPN**
- **Τύπος VPN: βάσει δρομολόγηση**
- **Δημόσια διεύθυνση IP: VNet1GWpip**
- **Τύπος σύνδεσης: σημείο σε τοποθεσία**
- **Σύνολο διευθύνσεων του προγράμματος-πελάτη: 172.16.201.0/24**<br>Προγράμματα-πελάτες VPN που συνδέονται με το VNet με αυτήν τη σύνδεση σημείου σε τοποθεσία λαμβάνουν μια διεύθυνση IP από το σύνολο διευθύνσεων του προγράμματος-πελάτη.

## <a name="before-beginning"></a>Πριν από την αρχή

- Βεβαιωθείτε ότι έχετε μια συνδρομή του Azure. Εάν δεν έχετε ήδη μια συνδρομή του Azure, μπορείτε να ενεργοποιήσετε το [MSDN συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ή το σύμβολο του για έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/).
    
## <a name="createvnet"></a>Μέρος 1 - Δημιουργήστε ένα εικονικό δίκτυο

Εάν θέλετε να δημιουργήσετε αυτήν τη ρύθμιση παραμέτρων ως μια άσκηση, μπορείτε να αναφερθείτε στις [τιμές παράδειγμα](#example).

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

### <a name="2-add-additional-address-space-and-subnets"></a>2. Προσθέστε διεύθυνση επιπλέον διάστημα και δευτερεύοντα δίκτυα

Μπορείτε να προσθέσετε χώρο επιπλέον διευθύνσεων και δευτερεύοντα δίκτυα να σας VNet αφού έχει δημιουργηθεί.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

### <a name="3-create-a-gateway-subnet"></a>3. Δημιουργήστε ένα υποδίκτυο πύλης

Πριν συνδεθείτε εικονικού το δίκτυό σας σε μια πύλη, πρέπει πρώτα να δημιουργήσετε το υποδίκτυο πύλης για το εικονικό δίκτυο στο οποίο θέλετε να συνδεθείτε. Εάν είναι δυνατό, είναι καλύτερα να δημιουργήσετε ένα δευτερεύον δίκτυο πύλης χρησιμοποιώντας ένα μπλοκ CIDR /28 ή /27 προκειμένου να παρέχετε αρκετές διευθύνσεις IP για να χωρέσει πρόσθετες ρυθμίσεις παραμέτρων μελλοντικές απαιτήσεις.

Τα στιγμιότυπα οθόνης σε αυτήν την ενότητα παρέχονται ως ένα παράδειγμα αναφοράς. Φροντίστε να χρησιμοποιήσετε την περιοχή διευθύνσεων GatewaySubnet που αντιστοιχεί με τις απαιτούμενες τιμές για τη ρύθμιση παραμέτρων.

#### <a name="to-create-a-gateway-subnet"></a>Για να δημιουργήσετε ένα υποδίκτυο πύλης

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="dns"></a>4. Καθορίστε έναν διακομιστή DNS (προαιρετικά)

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Μέρος 2 - Δημιουργία πύλης εικονικού δικτύου

Οι συνδέσεις σημείου σε τοποθεσία απαιτούν τις ακόλουθες ρυθμίσεις:

- Τύπος πύλης: VPN
- Τύπος VPN: βάσει δρομολόγηση

### <a name="to-create-a-virtual-network-gateway"></a>Για να δημιουργήσετε μια πύλη εικονικού δικτύου

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Μέρος 3 - Δημιουργία πιστοποιητικών

Τα πιστοποιητικά χρησιμοποιούνται από Azure για τον έλεγχο ταυτότητας προγραμμάτων-πελατών VPN για VPN σημείου σε τοποθεσία. Μπορείτε να εξαγάγετε δεδομένα πιστοποιητικό δημόσιου (μην το ιδιωτικό κλειδί) όπως μια βάσης 64 κωδικοποιημένο X.509 αρχείο .cer από ένα πιστοποιητικό ρίζας που δημιουργούνται από μια λύση πιστοποιητικό για μεγάλες επιχειρήσεις ή ένα πιστοποιητικό αυτόματης υπογραφής ρίζας. Στη συνέχεια, εισαγάγετε τα δεδομένα πιστοποιητικό δημόσιου από το πιστοποιητικό ρίζας σε Azure. Επιπλέον, πρέπει να δημιουργήσετε ένα πιστοποιητικό προγράμματος-πελάτη από το πιστοποιητικό ρίζας για προγράμματα-πελάτες. Κάθε υπολογιστή-πελάτη που θέλει να συνδεθείτε με το εικονικό δίκτυο με χρήση σύνδεσης P2S πρέπει να έχετε ένα πρόγραμμα-πελάτη εγκατεστημένο πιστοποιητικό που δημιουργήθηκε από το πιστοποιητικό ρίζας.

### <a name="getcer"></a>1. Κάντε λήψη του αρχείου .cer για το πιστοποιητικό ρίζας

Εάν χρησιμοποιείτε μια λύση για μεγάλες επιχειρήσεις, μπορείτε να χρησιμοποιήσετε υπάρχουσα αλυσίδα πιστοποιητικού. Εάν δεν χρησιμοποιείτε μια λύση CA για μεγάλες επιχειρήσεις, μπορείτε να δημιουργήσετε μια ρίζα αυτο-υπογεγραμμένου πιστοποιητικού. Μία μέθοδος για να δημιουργήσετε ένα αυτο-υπογεγραμμένο πιστοποιητικό είναι makecert.

- Εάν χρησιμοποιείτε ένα σύστημα πιστοποιητικό για μεγάλες επιχειρήσεις, αποκτήστε το αρχείο .cer του πιστοποιητικού ρίζας που θέλετε να χρησιμοποιήσετε. 

- Εάν δεν χρησιμοποιείτε μια λύση πιστοποιητικό για μεγάλες επιχειρήσεις, πρέπει να δημιουργήσετε ένα πιστοποιητικό αυτόματης υπογραφής ρίζας. Για τα Windows 10, μπορείτε να ανατρέξετε στην [εργασία με πιστοποιητικών ρίζας αυτο-υπογεγραμμένο για ρυθμίσεις παραμέτρων σημείου σε τοποθεσία](vpn-gateway-certificates-point-to-site.md).

1. Για να αποκτήσετε ένα αρχείο .cer από ένα πιστοποιητικό, ανοίξτε **certmgr.msc** και εντοπίστε το πιστοποιητικό ρίζας. Κάντε δεξί κλικ το αυτο-υπογεγραμμένο πιστοποιητικό, κάντε κλικ στην επιλογή **όλες τις εργασίες**και, στη συνέχεια, κάντε κλικ στην επιλογή **Εξαγωγή**. Έτσι ανοίγει το **Πιστοποιητικό του "Οδηγού εξαγωγής"**.

2. Στον οδηγό, κάντε κλικ στο κουμπί **Επόμενο**, επιλέξτε **Όχι, εξαγωγή το ιδιωτικό κλειδί**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

3. Στη σελίδα **Μορφή αρχείου εξαγωγής** , επιλέξτε **βάσης 64 κωδικοποιημένο X.509 (. Προαιρετικό).** Στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. 

4. Στην του **αρχείου προς εξαγωγή**, **μεταβείτε** στη θέση στην οποία θέλετε να εξαγάγετε το πιστοποιητικό. Για το **όνομα του αρχείου**, ονομάστε το αρχείο πιστοποιητικού. Στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

5. Κάντε κλικ στο κουμπί **Τέλος** για να εξαγάγετε το πιστοποιητικό.

### <a name="generateclientcert"></a>2. Δημιουργήστε ένα πιστοποιητικό προγράμματος-πελάτη

Μπορείτε να δημιουργήσετε είτε ένα μοναδικό πιστοποιητικό για κάθε πελάτη που θα συνδεθεί ή μπορείτε να χρησιμοποιήσετε το ίδιο πιστοποιητικό σε πολλά προγράμματα-πελάτες. Το πλεονέκτημα δημιουργό πιστοποιητικά μοναδικό προγράμματος-πελάτη είναι η δυνατότητα να ανακαλέσετε ενός πιστοποιητικού, εάν είναι απαραίτητο. Διαφορετικά, εάν όλοι χρησιμοποιεί το ίδιο πιστοποιητικό προγράμματος-πελάτη και μπορείτε να βρείτε ότι πρέπει να ανακαλέσετε το πιστοποιητικό για έναν υπολογιστή-πελάτη, θα πρέπει να δημιουργήσετε και να εγκαταστήσετε νέα πιστοποιητικά για όλα τα προγράμματα-πελάτες που χρησιμοποιούν αυτό το πιστοποιητικό για τον έλεγχο ταυτότητας.

- Εάν χρησιμοποιείτε μια λύση πιστοποιητικό για μεγάλες επιχειρήσεις, να δημιουργήσετε ένα πιστοποιητικό προγράμματος-πελάτη με την συνηθισμένη μορφή τιμή όνομα 'name@yourdomain.com', και όχι τη μορφή 'τομέα\όνομα_χρήστη'. 

- Εάν χρησιμοποιείτε ένα πιστοποιητικό αυτόματης υπογραφής, ανατρέξτε στο θέμα [εργασία με αυτο-υπογεγραμμένο ριζικών πιστοποιητικών για ρυθμίσεις παραμέτρων σημείου σε τοποθεσία](vpn-gateway-certificates-point-to-site.md) για να δημιουργήσετε ένα πιστοποιητικό προγράμματος-πελάτη.

### <a name="exportclientcert"></a>3. εξαγάγετε το πιστοποιητικό προγράμματος-πελάτη

Απαιτείται ένα πιστοποιητικό προγράμματος-πελάτη για έλεγχο ταυτότητας. Μετά από τη δημιουργία του πιστοποιητικού προγράμματος-πελάτη, εξαγάγετε. Το πιστοποιητικό προγράμματος-πελάτη εξαγωγή θα εγκατασταθεί αργότερα σε κάθε υπολογιστή-πελάτη.

1. Για να εξαγάγετε ένα πιστοποιητικό προγράμματος-πελάτη, μπορείτε να χρησιμοποιήσετε *certmgr.msc*. Κάντε δεξί κλικ στο πιστοποιητικό προγράμματος-πελάτη που θέλετε να εξαγάγετε, κάντε κλικ στην επιλογή **όλες τις εργασίες**και, στη συνέχεια, κάντε κλικ στην επιλογή **Εξαγωγή**.
2. Εξαγάγετε το πιστοποιητικό προγράμματος-πελάτη με το ιδιωτικό κλειδί. Αυτό είναι ένα αρχείο *.pfx* . Βεβαιωθείτε ότι η εγγραφή ή να θυμηθείτε τον κωδικό πρόσβασης (πλήκτρο) που έχετε ορίσει για αυτό το πιστοποιητικό.

## <a name="addresspool"></a>Μέρος 4 - Προσθήκη το χώρο συγκέντρωσης διευθύνσεων του προγράμματος-πελάτη

1. Μετά τη δημιουργία της πύλης εικονικού δικτύου, μεταβείτε στην ενότητα **Ρυθμίσεις** του blade πύλης εικονικού δικτύου. Στην ενότητα **Ρυθμίσεις** , κάντε κλικ στην επιλογή ρύθμιση παραμέτρων **σημείο σε τοποθεσία** για να ανοίξετε το blade **ρύθμισης παραμέτρων** .

    ![τοποθετήστε το δείκτη για να blade τοποθεσίας] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "τοποθετήστε το δείκτη για να blade τοποθεσίας")

2. **Χώρος συγκέντρωσης διεύθυνση** είναι το διευθύνσεων IP από την οποία οι υπολογιστές-πελάτες που συνδέονται θα λάβουν μια διεύθυνση IP. Προσθέστε το σύνολο των διευθύνσεων και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![σύνολο διευθύνσεων του προγράμματος-πελάτη] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "σύνολο διευθύνσεων του προγράμματος-πελάτη")

## <a name="uploadfile"></a>Τμήμα 5 - αποστολή το αρχείο .cer πιστοποιητικού ρίζας

Μετά τη δημιουργία της πύλης, μπορείτε να αποστείλετε το αρχείο .cer για ενός αξιόπιστου πιστοποιητικού ρίζας στο Azure. Μπορείτε να αποστείλετε αρχεία για έως και 20 πιστοποιητικών ρίζας. Μην αποστέλλετε ιδιωτικό κλειδί για το πιστοποιητικό ρίζας Azure. Εφόσον το αρχείο .cer αποσταλεί, το Azure χρησιμοποιεί για τον έλεγχο ταυτότητας προγράμματα-πελάτες που συνδέονται με το εικονικό δίκτυο.

1. Μεταβείτε στη ρύθμιση παραμέτρων **σημείο σε τοποθεσία** blade. Θα προσθέσετε τα αρχεία .cer στην ενότητα **πιστοποιητικού ρίζας** της αυτό blade.

    ![τοποθετήστε το δείκτη για να blade τοποθεσίας] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "τοποθετήστε το δείκτη για να blade τοποθεσίας")

2. Βεβαιωθείτε ότι έχετε εξαγάγει πιστοποιητικού ρίζας όπως μια βάσης 64 κωδικοποιημένο X.509 (.cer) αρχείο. Πρέπει να εξαγάγετε, σε αυτήν τη μορφή, ώστε να μπορείτε να ανοίξετε το πιστοποιητικό με το πρόγραμμα επεξεργασίας κειμένου.
3. Ανοίξτε το πιστοποιητικό με ένα πρόγραμμα επεξεργασίας κειμένου, όπως το Σημειωματάριο. Αντιγραφή μόνο την παρακάτω ενότητα:

    ![δεδομένα πιστοποιητικού] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "δεδομένα πιστοποιητικού")

4. Επικολλήστε τα δεδομένα πιστοποιητικού στην ενότητα **Δημόσιων δεδομένων πιστοποιητικό** της πύλης. Τοποθέτηση το όνομα του πιστοποιητικού στο χώρο **ονομάτων** και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**. Μπορείτε να προσθέσετε έως και 20 αξιόπιστων πιστοποιητικών ρίζας.

    ![Αποστολή πιστοποιητικού] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "Αποστολή πιστοποιητικού")

## <a name="clientconfig"></a>Τμήμα 6 - κάντε λήψη και εγκαταστήστε το πακέτο ρύθμισης παραμέτρων προγράμματος-πελάτη VPN

Προγράμματα-πελάτες τη σύνδεση με χρήση P2S Azure πρέπει να έχετε τόσο ένα πιστοποιητικό προγράμματος-πελάτη και ένα πακέτο ρύθμισης παραμέτρων προγράμματος-πελάτη VPN εγκατεστημένο. Πακέτα ρύθμισης παραμέτρων προγράμματος-πελάτη VPN είναι διαθέσιμα για προγράμματα-πελάτες των Windows. 

Το πακέτο του προγράμματος-πελάτη VPN περιέχει πληροφορίες για τη ρύθμιση παραμέτρων του λογισμικού προγράμματος-πελάτη VPN που είναι ενσωματωμένο στο Windows. Η ρύθμιση παραμέτρων είναι συγκεκριμένη για το VPN που θέλετε να συνδεθείτε. Το πακέτο δεν εγκαθιστά πρόσθετο λογισμικό. Ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις πύλης VPN](vpn-gateway-vpn-faq.md#point-to-site-connections) για περισσότερες πληροφορίες.

1. Η ρύθμιση των παραμέτρων του **σημείου σε τοποθεσία** blade, κάντε κλικ στην επιλογή **λήψη VPN προγράμματος-πελάτη** για να ανοίξετε το **πρόγραμμα-πελάτη VPN λήψη** blade.

    ![Κάντε λήψη του υπολογιστή-πελάτη VPN] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "Κάντε λήψη του υπολογιστή-πελάτη VPN")

2. Επιλέξτε το σωστό πακέτο για το πρόγραμμα-πελάτη και, στη συνέχεια, κάντε κλικ στην επιλογή **λήψη**. Για προγράμματα-πελάτες 64-bit, επιλέξτε **AMD64**. Για προγράμματα-πελάτες 32-bit, επιλέξτε **x86**.

3. Εγκαταστήστε το πακέτο του υπολογιστή-πελάτη. Εάν λάβετε ένα αναδυόμενο παράθυρο SmartScreen, κάντε κλικ στην επιλογή **περισσότερες πληροφορίες**, στη συνέχεια, **εκτελέστε οπωσδήποτε** για να εγκαταστήσετε το πακέτο.

4. Στον υπολογιστή-πελάτη, μεταβείτε στις **Ρυθμίσεις δικτύου** και κάντε κλικ στην επιλογή **VPN**. Θα δείτε τη σύνδεση που παρατίθενται. Θα εμφανίζεται το όνομα του εικονικού δικτύου που θα συνδεθεί και είναι παρόμοιο με αυτό το παράδειγμα: 

    ![Υπολογιστή-πελάτη VPN] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "Υπολογιστή-πελάτη VPN")

## <a name="installclientcert"></a>Τμήμα 7 - εγκατάσταση του πιστοποιητικού προγράμματος-πελάτη

Κάθε υπολογιστής-πελάτης πρέπει να έχετε ένα πιστοποιητικό προγράμματος-πελάτη για να τον έλεγχο ταυτότητας. Κατά την εγκατάσταση του πιστοποιητικού προγράμματος-πελάτη, θα χρειαστείτε τον κωδικό πρόσβασης που δημιουργήθηκε κατά την εξαγωγή του πιστοποιητικού προγράμματος-πελάτη.

1. Αντιγράψτε το αρχείο .pfx στον υπολογιστή-πελάτη.
2. Κάντε διπλό κλικ το αρχείο .pfx για να την εγκαταστήσετε. Μην τροποποιείτε τη θέση εγκατάστασης.

## <a name="connect"></a>Μέρος 8 - σύνδεση με Azure

1. Για να συνδεθείτε με το VNet, στον υπολογιστή-πελάτη, μεταβείτε στο συνδέσεις VPN και εντοπίστε τη σύνδεση VPN που δημιουργήσατε. Ονομάζεται το ίδιο όνομα με το εικονικό δίκτυο. Κάντε κλικ στην επιλογή **σύνδεση**. Ενδέχεται να εμφανιστεί ένα αναδυόμενο μήνυμα που αναφέρεται σε χρησιμοποιώντας το πιστοποιητικό. Αν συμβεί αυτό, κάντε κλικ στο κουμπί **συνέχεια** για να χρησιμοποιήσετε αναβαθμισμένα δικαιώματα. 

2. Στη σελίδα κατάστασης **σύνδεσης** , κάντε κλικ στην επιλογή **σύνδεση** για να ξεκινήσετε τη σύνδεση. Εάν εμφανιστεί μια οθόνη **Επιλογή πιστοποιητικού** , βεβαιωθείτε ότι το πιστοποιητικό προγράμματος-πελάτη που εμφανίζει είναι αυτό που θέλετε να χρησιμοποιήσετε για να συνδεθείτε. Εάν δεν είναι, χρησιμοποιήστε το αναπτυσσόμενο βέλος για να επιλέξετε το σωστό πιστοποιητικό και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

    ![Υπολογιστή-πελάτη VPN 2] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Σύνδεση του υπολογιστή-πελάτη VPN")

3. Σας θα πρέπει τώρα δυνατή η σύνδεση.

    ![Υπολογιστή-πελάτη VPN 3] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Σύνδεση του υπολογιστή-πελάτη VPN 2")

## <a name="verify"></a>Μέρος 9 - Ελέγξτε τη σύνδεσή σας

1. Για να επαληθεύσετε ότι τη σύνδεση VPN είναι ενεργό, ανοίξτε μια γραμμή εντολών με αναβαθμισμένα δικαιώματα και εκτελέστε *ipconfig/all*.

2. Προβολή των αποτελεσμάτων. Παρατηρήστε ότι η διεύθυνση IP που λάβατε είναι μία από τις διευθύνσεις μέσα σε χώρο συγκέντρωσης διεύθυνση προγράμματος-πελάτη VPN σημείου σε τοποθεσία που έχετε καθορίσει στη ρύθμιση παραμέτρων σας. Τα αποτελέσματα θα πρέπει να είναι κάτι παρόμοιο με αυτό:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="add"></a>Για να προσθέσετε ή να καταργήσετε αξιόπιστων πιστοποιητικών ρίζας

Μπορείτε να καταργήσετε αξιόπιστου πιστοποιητικού ρίζας από το Azure. Όταν καταργείτε ένα αξιόπιστο πιστοποιητικό, τα πιστοποιητικά προγράμματος-πελάτη που δημιουργήθηκαν από το πιστοποιητικό ρίζας δεν είναι πλέον θα μπορείτε να συνδεθείτε στο Azure μέσω του σημείου σε τοποθεσία. Εάν θέλετε οι υπολογιστές-πελάτες για να συνδεθείτε, πρέπει να εγκαταστήσετε ένα νέο πιστοποιητικό προγράμματος-πελάτη που δημιουργείται από ένα πιστοποιητικό που είναι αξιόπιστα στο Azure.

Μπορείτε να διαχειριστείτε τη λίστα των πιστοποιητικών έχει ανακληθεί προγράμματος-πελάτη στη ρύθμιση παραμέτρων του **σημείου σε τοποθεσία** blade. Αυτό είναι το blade που χρησιμοποιήσατε για την [Αποστολή ενός αξιόπιστου πιστοποιητικού ρίζας](#uploadfile).

## <a name="revokeclient"></a>Για να διαχειριστείτε τη λίστα ανάκλησης πιστοποιητικών προγράμματος-πελάτη

Μπορείτε να ανακαλέσετε πιστοποιητικά προγράμματος-πελάτη. Λίστα ανάκλησης πιστοποιητικών σάς επιτρέπει να αρνηθεί επιλεκτικής συνδεσιμότητας σημείου σε τοποθεσία που βασίζεται σε μεμονωμένα πιστοποιητικά πελατών. Αν καταργήσετε ένα .cer πιστοποιητικού ρίζας από το Azure, κάνει ανάκληση την πρόσβαση για όλα τα πιστοποιητικά του προγράμματος-πελάτη που δημιουργούνται/συνδεδεμένοι με το πιστοποιητικό έχει ανακληθεί ρίζας. Εάν θέλετε να ανακαλέσετε ένα πιστοποιητικό συγκεκριμένο πρόγραμμα-πελάτη, δεν τον ριζικό κατάλογο, μπορείτε να το κάνετε. Με αυτόν τον τρόπο που τα άλλα πιστοποιητικά που δημιουργήθηκαν από το πιστοποιητικό ρίζας θα εξακολουθεί να είναι έγκυρες. 

Η κοινή πρακτική είναι να χρησιμοποιήσετε το πιστοποιητικό ρίζας για να διαχειριστείτε την πρόσβαση σε επίπεδο ομάδας ή του οργανισμού, κατά τη χρήση του προγράμματος-πελάτη έχει ανακληθεί πιστοποιητικά για έλεγχο πρόσβασης σε λεπτομερή σε μεμονωμένους χρήστες.

Μπορείτε να διαχειριστείτε τη λίστα των πιστοποιητικών έχει ανακληθεί προγράμματος-πελάτη στη ρύθμιση παραμέτρων του **σημείου σε τοποθεσία** blade. Αυτό είναι το blade που χρησιμοποιήσατε για την [Αποστολή ενός αξιόπιστου πιστοποιητικού ρίζας](#uploadfile).


## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να προσθέσετε μια εικονική μηχανή στο δίκτυο του εικονικού σας. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .