<properties
   pageTitle="Δημιουργήστε ένα εικονικό δίκτυο με μια σύνδεση VPN-τοποθεσίας χρήση της διαχείρισης πόρων Azure και την πύλη Azure | Microsoft Azure"
   description="Πώς μπορείτε να δημιουργήσετε VNet χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων και συνδέστε τη με το τοπικό εσωτερικής εγκατάστασης δικτύου, χρησιμοποιώντας μια σύνδεση S2S VPN πύλης."
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
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Δημιουργήστε μια VNet με μια σύνδεση τοποθεσίας σε τοποθεσία χρησιμοποιώντας την πύλη του Azure

> [AZURE.SELECTOR]
- [Διαχείριση πόρων - Azure πύλη](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Διαχείριση πόρων - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Κλασικό - κλασική πύλη](vpn-gateway-site-to-site-create.md)


Σε αυτό το άρθρο σάς καθοδηγεί σε δημιουργώντας ένα εικονικό δίκτυο και σύνδεση VPN-τοποθεσίας πύλης στο δίκτυό σας εσωτερικής εγκατάστασης χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων Azure και την πύλη του Azure. Συνδέσεις τοποθεσίας σε τοποθεσία μπορούν να χρησιμοποιηθούν για διασταυρούμενο εσωτερικής εγκατάστασης και υβριδικό ρυθμίσεις παραμέτρων.

![Διάγραμμα](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Μοντέλα ανάπτυξης και οι μέθοδοι για συνδέσεις τοποθεσίας σε τοποθεσία

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Ο παρακάτω πίνακας εμφανίζει τα υποδείγματα αυτήν τη στιγμή διαθέσιμα ανάπτυξης και τις μεθόδους για ρυθμίσεις παραμέτρων τοποθεσίας σε τοποθεσία. Όταν ένα άρθρο με βήματα ρύθμισης παραμέτρων είναι διαθέσιμη, θα σας σύνδεση απευθείας σε αυτήν από αυτόν τον πίνακα.

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Πρόσθετες ρυθμίσεις 

Εάν θέλετε να συνδεθείτε μαζί VNets, αλλά δεν θα δημιουργήσετε μια σύνδεση προς μια θέση εσωτερικής εγκατάστασης, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων σύνδεσης VNet-VNet](vpn-gateway-vnet-vnet-rm-ps.md). Εάν θέλετε να προσθέσετε μια σύνδεση-τοποθεσίας σε μια VNet που διαθέτει ήδη μια σύνδεση, ανατρέξτε στο θέμα [Προσθήκη σύνδεσης S2S σε μια VNet με μια υπάρχουσα σύνδεση VPN πύλης](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Βεβαιωθείτε ότι έχετε τα ακόλουθα στοιχεία πριν να ξεκινήσετε τη ρύθμιση των παραμέτρων σας:

- Μια συμβατή συσκευή VPN και κάποιο άτομο που είναι σε θέση να ρυθμίσετε τις παραμέτρους της. Ανατρέξτε στο θέμα [σχετικά με τις συσκευές VPN](vpn-gateway-about-vpn-devices.md). Εάν δεν είστε εξοικειωμένοι με τη ρύθμιση των παραμέτρων σας συσκευή VPN, ή να είστε εξοικειωμένοι με τη διεύθυνση IP περιοχές που βρίσκεται στο ρυθμίσεις παραμέτρων δικτύου σας εσωτερικής εγκατάστασης, θα πρέπει να επικοινωνήσετε με κάποιον που μπορούν να παρέχουν αυτές τις λεπτομέρειες για εσάς.

- Μια εξωτερική αντικριστές δημόσια διεύθυνση IP για τη συσκευή σας VPN. Αυτή η διεύθυνση IP δεν μπορεί να βρίσκεται πίσω από μια συσκευή NAT.
    
- Μια συνδρομή του Azure. Εάν δεν έχετε ήδη μια συνδρομή του Azure, μπορείτε να ενεργοποιήσετε το [MSDN συνδρομητών πλεονεκτήματα](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ή το σύμβολο του για έναν [δωρεάν λογαριασμό](http://azure.microsoft.com/pricing/free-trial/).

### <a name="values"></a>Δείγμα ρύθμισης παραμέτρων τιμές για αυτήν την άσκηση


Όταν χρησιμοποιείτε αυτά τα βήματα ως άσκηση, μπορείτε να χρησιμοποιήσετε τις τιμές παραμέτρων δείγμα:

- **Όνομα VNet:** TestVNet1
- **Διευθύνσεων χώρο:** 10.11.0.0/16 και 10.12.0.0/16
- **Δευτερεύοντα δίκτυα:**
    - FrontEnd: 10.11.0.0/24
    - Παρασκηνίου: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- **Ομάδα πόρων:** TestRG1
- **Θέση:** Ανατολικής η.π.α.
- **Διακομιστή DNS:** 8.8.8.8
- **Όνομα πύλης:** VNet1GW
- **Δημόσια IP:** VNet1GWIP
- **Τύπος VPN:** Δρομολόγηση βάσει
- **Τύπος σύνδεσης:** Τοποθεσία σε τοποθεσία (ασφαλείας IP)
- **Τύπο πύλη:** VPN
- **Όνομα πύλης τοπικού δικτύου:** Site2
- **Όνομα σύνδεσης:** VNet1toSite2


## <a name="CreatVNet"></a>1. Δημιουργία εικονικού δικτύου 

Εάν έχετε ήδη ένα VNet, βεβαιωθείτε ότι οι ρυθμίσεις είναι συμβατές με το σχεδιασμό πύλης VPN. Δώστε ιδιαίτερη προσοχή οποιαδήποτε δευτερεύοντα δίκτυα που μπορεί να επικαλύπτονται με άλλα δίκτυα. Εάν έχετε επικαλυπτόμενες δευτερεύοντα δίκτυα, τη σύνδεσή σας δεν θα λειτουργούν σωστά. Εάν το VNet έχει ρυθμιστεί με τις σωστές ρυθμίσεις, μπορείτε να ξεκινήσετε τα βήματα στην ενότητα [Καθορισμός ενός διακομιστή DNS](#dns) .

### <a name="to-create-a-virtual-network"></a>Για να δημιουργήσετε ένα εικονικό δίκτυο

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. Προσθέστε διεύθυνση επιπλέον διάστημα και δευτερεύοντα δίκτυα

Μπορείτε να προσθέσετε χώρο επιπλέον διευθύνσεων και δευτερεύοντα δίκτυα να σας VNet αφού έχει δημιουργηθεί.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. Καθορίστε έναν διακομιστή DNS

### <a name="to-specify-a-dns-server"></a>Για να καθορίσετε ένα διακομιστή DNS

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>4. Δημιουργήστε ένα υποδίκτυο πύλης

Πριν συνδεθείτε εικονικού το δίκτυό σας σε μια πύλη, πρέπει πρώτα να δημιουργήσετε το υποδίκτυο πύλης για το εικονικό δίκτυο στο οποίο θέλετε να συνδεθείτε. Εάν είναι δυνατό, είναι καλύτερα να δημιουργήσετε ένα δευτερεύον δίκτυο πύλης χρησιμοποιώντας ένα μπλοκ CIDR /28 ή /27 προκειμένου να παρέχετε αρκετές διευθύνσεις IP για να χωρέσει πρόσθετες ρυθμίσεις παραμέτρων μελλοντικές απαιτήσεις.

Εάν δημιουργείτε αυτήν τη ρύθμιση παραμέτρων ως μια άσκηση, ανατρέξτε σε αυτές τις [τιμές](#values) κατά τη δημιουργία σας υποδίκτυο πύλης.

### <a name="to-create-a-gateway-subnet"></a>Για να δημιουργήσετε ένα υποδίκτυο πύλης


[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Δημιουργία πύλης εικονικού δικτύου

Εάν θέλετε να δημιουργήσετε αυτήν τη ρύθμιση παραμέτρων ως μια άσκηση, μπορείτε να αναφερθείτε στις [τιμές παραμέτρων δείγματος](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Για να δημιουργήσετε μια πύλη εικονικού δικτύου

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>6. Δημιουργία πύλης τοπικού δικτύου

Η πύλη' τοπικό δίκτυο' αναφέρεται στη θέση σας εσωτερικής εγκατάστασης. Δώστε την πύλη τοπικό δίκτυο ένα όνομα με την οποία Azure μπορεί να αναφέρεται σε αυτήν. 

Εάν θέλετε να δημιουργήσετε αυτήν τη ρύθμιση παραμέτρων ως μια άσκηση, μπορείτε να αναφερθείτε στις [τιμές παραμέτρων δείγματος](#values).

### <a name="to-create-a-local-network-gateway"></a>Για να δημιουργήσετε μια πύλη τοπικού δικτύου

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="VPNDevice"></a>7. ρυθμίσετε τη συσκευή σας VPN

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. Δημιουργήστε μια σύνδεση VPN τοποθεσίας σε τοποθεσία

Δημιουργία της σύνδεσης τοποθεσίας σε τοποθεσία VPN μεταξύ της πύλης εικονικού δικτύου και συσκευή VPN. Φροντίστε να αντικαταστήσετε τις τιμές με το δικό σας. Το κοινόχρηστο κλειδί πρέπει να συμφωνεί με την τιμή που χρησιμοποιήσατε για τη ρύθμιση παραμέτρων συσκευή VPN. 

Πριν ξεκινήσετε αυτήν την ενότητα, βεβαιωθείτε ότι το πύλης εικονικού δικτύου και των πυλών τοπικό δίκτυο έχετε ολοκληρώσει τη δημιουργία. Εάν θέλετε να δημιουργήσετε αυτήν τη ρύθμιση παραμέτρων ως μια άσκηση, αναφέρονται σε αυτές τις [τιμές](#values) κατά τη δημιουργία τη σύνδεσή σας.

### <a name="to-create-the-vpn-connection"></a>Για να δημιουργήσετε τη σύνδεση VPN


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="VerifyConnection"></a>9. Βεβαιωθείτε ότι η σύνδεση VPN

Μπορείτε να επαληθεύσετε τη σύνδεση VPN στην πύλη είτε με χρήση του PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Επόμενα βήματα

- Μόλις ολοκληρωθεί η σύνδεσή σας, μπορείτε να προσθέσετε εικονικές μηχανές στα δίκτυά σας εικονικού. Ανατρέξτε στο θέμα το εικονικές μηχανές [διαδρομή εκμάθησης](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) για περισσότερες πληροφορίες.

- Για πληροφορίες σχετικά με το πρωτόκολλο BGP, ανατρέξτε στο θέμα το [Πρωτόκολλο BGP Επισκόπηση](vpn-gateway-bgp-overview.md) και [πώς μπορείτε να ρυθμίσετε τις παραμέτρους πρωτόκολλο BGP](vpn-gateway-bgp-resource-manager-ps.md).
