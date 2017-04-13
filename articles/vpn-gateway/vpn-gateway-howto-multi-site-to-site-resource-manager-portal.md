<properties
   pageTitle="Πώς μπορείτε να προσθέσετε πολλές συνδέσεις τοποθεσίας σε τοποθεσία πύλης VPN σε δίκτυο εικονικού για το μοντέλο ανάπτυξης για τη διαχείριση πόρων με την πύλη Azure | Microsoft Azure"
   description="Προσθήκη συνδέσεων S2S πολλών τοποθεσίας σε μια πύλη VPN που έχει μια υπάρχουσα σύνδεση"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>



# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Προσθήκη σύνδεσης τοποθεσίας σε τοποθεσία για μια VNet με μια υπάρχουσα σύνδεση VPN πύλης

> [AZURE.SELECTOR]
- [Διαχείριση πόρων - πύλη](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Κλασικό - PowerShell](vpn-gateway-multi-site.md)

Σε αυτό το άρθρο σάς καθοδηγεί σε με την πύλη Azure για να προσθέσετε συνδέσεις-τοποθεσίας (S2S) σε μια πύλη VPN που έχει μια υπάρχουσα σύνδεση. Αυτός ο τύπος σύνδεσης αναφέρεται συχνά ως "πολλών τοποθεσία" Ρύθμιση παραμέτρων. 

Μπορείτε να χρησιμοποιήσετε αυτό το άρθρο για να προσθέσετε μια σύνδεση S2S σε ένα VNet που διαθέτει ήδη μια σύνδεση S2S, σύνδεση σημείου σε τοποθεσία ή VNet-VNet σύνδεσης. Υπάρχουν ορισμένοι περιορισμοί κατά την προσθήκη συνδέσεων. Ελέγξτε την ενότητα [πριν να ξεκινήσετε](#before) σε αυτό το άρθρο για να επαληθεύσετε πριν να ξεκινήσετε τη ρύθμιση των παραμέτρων σας. 

Σε αυτό το άρθρο ισχύει για το VNets που δημιουργούνται με χρήση του μοντέλου ανάπτυξης διαχείρισης πόρων που έχουν μια πύλη RouteBased VPN. Αυτά τα βήματα δεν ισχύουν για ExpressRoute /--τοποθεσίας συνυπάρχουσες ρυθμίσεις παραμέτρων σύνδεσης. Για πληροφορίες σχετικά με τις συνδέσεις συνυπάρχουσες, ανατρέξτε στο θέμα [ExpressRoute/S2S συνυπάρχουσες συνδέσεις](../expressroute/expressroute-howto-coexist-resource-manager.md) .

### <a name="deployment-models-and-methods"></a>Μοντέλα ανάπτυξης και οι μέθοδοι

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Ενημερώνουμε αυτόν τον πίνακα ως νέα άρθρα και πρόσθετα εργαλεία γίνονται διαθέσιμοι για αυτήν τη ρύθμιση παραμέτρων. Όταν ένα άρθρο είναι διαθέσιμη, θα σας σύνδεση απευθείας σε αυτήν από αυτόν τον πίνακα.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 


## <a name="before"></a>Πριν ξεκινήσετε

Επαληθεύστε τα ακόλουθα στοιχεία:

- Μην δημιουργείτε μια σύνδεση συνυπάρχουσες ExpressRoute/S2S.
- Έχετε ένα εικονικό δίκτυο που δημιουργήθηκε με χρήση του μοντέλου ανάπτυξης διαχείρισης πόρων με μια υπάρχουσα σύνδεση.
- Η πύλη εικονικού δικτύου για το VNet είναι RouteBased. Εάν έχετε μια πύλη PolicyBased VPN, πρέπει να διαγράψετε την πύλη εικονικού δικτύου και να δημιουργήσετε μια νέα πύλη VPN ως RoutBased.
- Καμία από τις περιοχές διευθύνσεων επικαλύπτονται για οποιαδήποτε από τα VNets που VNet αυτό γίνεται η σύνδεση.
- Έχετε συμβατή συσκευή VPN και κάποιο άτομο που είναι σε θέση να ρυθμίσετε τις παραμέτρους της. Ανατρέξτε στο θέμα [σχετικά με τις συσκευές VPN](vpn-gateway-about-vpn-devices.md). Εάν δεν είστε εξοικειωμένοι με τη ρύθμιση των παραμέτρων σας συσκευή VPN, ή να είστε εξοικειωμένοι με τη διεύθυνση IP περιοχές που βρίσκεται στο ρυθμίσεις παραμέτρων δικτύου σας εσωτερικής εγκατάστασης, θα πρέπει να επικοινωνήσετε με κάποιον που μπορούν να παρέχουν αυτές τις λεπτομέρειες για εσάς.
- Έχετε μια εξωτερική αντικριστές δημόσια διεύθυνση IP για τη συσκευή σας VPN. Αυτή η διεύθυνση IP δεν μπορεί να βρίσκεται πίσω από μια συσκευή NAT.


## <a name="part1"></a>Μέρος 1 - ρύθμιση παραμέτρων σύνδεσης

1. Από ένα πρόγραμμα περιήγησης, μεταβείτε στην [πύλη του Azure](http://portal.azure.com) και, εάν είναι απαραίτητο, πραγματοποιήστε είσοδο με το λογαριασμό της Azure.
2. Κάντε κλικ στην επιλογή **όλους τους πόρους** και εντοπίστε την **πύλη εικονικού δικτύου** σας από τη λίστα των πόρων και κάντε κλικ σε αυτό.
3. Στην το blade **εικονικές πύλη δικτύου** , κάντε κλικ στην επιλογή **συνδέσεις**.

    ![Συνδέσεις blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>

4. Στην το blade **συνδέσεις** , κάντε κλικ στο κουμπί **+ Προσθήκη**.

    ![Προσθήκη κουμπιού σύνδεσης](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>

5. Στην την **Προσθήκη σύνδεσης** blade, συμπληρώστε τα παρακάτω πεδία:
    - **Όνομα:** Το όνομα που θέλετε να δώσετε στην τοποθεσία που θέλετε να δημιουργήσετε τη σύνδεση με.
    - **Τύπος σύνδεσης:** Επιλογή **τοποθεσίας σε τοποθεσία (ασφαλείας IP)**.

    ![Προσθήκη σύνδεσης blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="part2"></a>Μέρος 2 - Προσθήκη μιας πύλης τοπικού δικτύου

1. Κάντε κλικ στην επιλογή **τοπικό δίκτυο πύλης** ***Επιλέξτε μια πύλη τοπικού δικτύου***. Αυτό θα ανοίξει το blade **επιλογή τοπικό δίκτυο πύλης** .

    ![Επιλέξτε τοπικό δίκτυο πύλη](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Κάντε κλικ στην επιλογή **Δημιουργία νέου** για να ανοίξετε το blade **Δημιουργία πύλης τοπικού δικτύου** .

    ![Δημιουργία τοπικού δικτύου blade πύλης](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>

3. Στην blade τη **Δημιουργία πύλης τοπικό δίκτυο** , συμπληρώστε τα παρακάτω πεδία:
    - **Όνομα:** Το όνομα που θέλετε να δώσετε στον πόρο πύλης τοπικού δικτύου.
    - **Διεύθυνση IP:** Στη δημόσια διεύθυνση IP της συσκευής VPN στην τοποθεσία που θέλετε να συνδεθείτε.
    - **Διευθύνσεων χώρο:** Το χώρο διευθύνσεων που θέλετε να δρομολογούνται στη νέα τοποθεσία τοπικού δικτύου.
4. Κάντε κλικ στο κουμπί **OK** στην το blade **Δημιουργία πύλης τοπικό δίκτυο** για να αποθηκεύσετε τις αλλαγές.

## <a name="part3"></a>Μέρος 3 - Προσθήκη το κοινόχρηστο κλειδί και δημιουργία της σύνδεσης

1. Στη blade η **Προσθήκη σύνδεσης** , προσθέστε το κοινόχρηστο κλειδί που θέλετε να χρησιμοποιήσετε για να δημιουργήσετε τη σύνδεση. Μπορείτε να λάβετε το κοινόχρηστο κλειδί από τη συσκευή σας VPN, ή κάντε μία του εδώ και, στη συνέχεια, να ρυθμίσετε τη συσκευή σας VPN για να χρησιμοποιήσετε το ίδιο κοινόχρηστο κλειδί. Σημαντικό είναι ότι τα πλήκτρα είναι ακριβώς ίδια.

    ![Κοινόχρηστο κλειδί](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. Στο κάτω μέρος του blade, κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε τη σύνδεση.

## <a name="part4"></a>Μέρος 4 - Επαληθεύστε τη σύνδεση VPN

Μπορείτε να επαληθεύσετε τη σύνδεση VPN στην πύλη είτε με χρήση του PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## <a name="next-steps"></a>Επόμενα βήματα

- Μόλις ολοκληρωθεί η σύνδεσή σας, μπορείτε να προσθέσετε εικονικές μηχανές στα δίκτυά σας εικονικού. Ανατρέξτε στο θέμα το εικονικές μηχανές [διαδρομή εκμάθησης](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) για περισσότερες πληροφορίες.