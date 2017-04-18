<properties 
   pageTitle="Ρύθμιση παραμέτρων σύνδεσης VPN σημείου σε τοποθεσία πύλης εικονικού δικτύου, χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων | Microsoft Azure"
   description="Ασφαλή σύνδεση με το δίκτυο εικονικού Azure, δημιουργώντας μια σύνδεση VPN σημείου σε τοποθεσία πύλης."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Ρύθμιση παραμέτρων μια σύνδεση σημείου σε τοποθεσία με μια VNet χρήση του PowerShell

> [AZURE.SELECTOR]
- [Διαχείριση πόρων - Azure πύλη](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Διαχείριση πόρων - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Κλασικό - Azure πύλη](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Μια ρύθμιση παραμέτρων σημείου σε τοποθεσία (P2S) σάς επιτρέπει να δημιουργήσετε μια ασφαλή σύνδεση από έναν υπολογιστή-πελάτη μεμονωμένα σε εικονικό δίκτυο. Μια σύνδεση P2S είναι χρήσιμη όταν θέλετε να συνδεθείτε με το VNet από μια απομακρυσμένη θέση, όπως από το σπίτι ή ένα Συνέδριο, ή όταν έχετε μόνο ορισμένα προγράμματα-πελάτες που πρέπει να συνδεθείτε με ένα εικονικό δίκτυο. 

Συνδέσεις σημείου σε τοποθεσία δεν απαιτούν μια συσκευή VPN ή μια διεύθυνση IP δημόσιας ώστε να λειτουργεί. Δημιουργείται μια σύνδεση VPN από την αρχική τη σύνδεση του υπολογιστή-πελάτη. Για περισσότερες πληροφορίες σχετικά με τις συνδέσεις σημείου σε τοποθεσία, ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις πύλης VPN](vpn-gateway-vpn-faq.md#point-to-site-connections) και [Προγραμματισμός και σχεδίαση](vpn-gateway-plan-design.md). 

Σε αυτό το άρθρο σάς καθοδηγεί στη δημιουργία ενός VNet με μια σύνδεση σημείου σε τοποθεσία στο μοντέλο ανάπτυξης διαχείρισης πόρων με χρήση του PowerShell.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Μοντέλα ανάπτυξης και οι μέθοδοι για τις συνδέσεις P2S

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Ο παρακάτω πίνακας εμφανίζει τα δύο μοντέλων ανάπτυξης και οι μέθοδοι διαθέσιμη ανάπτυξης για διαμορφώσεις P2S. Όταν ένα άρθρο με βήματα ρύθμισης παραμέτρων είναι διαθέσιμη, θα σας σύνδεση απευθείας σε αυτήν από αυτόν τον πίνακα.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Βασική ροής εργασίας 

![Σημείου-σε--διάγραμμα τοποθεσίας] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "σημείο σε τοποθεσία")

Σε αυτό το σενάριο, θα δημιουργήσετε ένα εικονικό δίκτυο με μια σύνδεση σημείου σε τοποθεσία. Τις οδηγίες που εμφανίζονται επίσης θα σας βοηθήσει να δημιουργήσετε τα πιστοποιητικά, που απαιτούνται για αυτήν τη ρύθμιση παραμέτρων. Μια σύνδεση P2S αποτελείται από τα ακόλουθα στοιχεία: A VNet με μια πύλη VPN, ένα αρχείο .cer πιστοποιητικού ρίζας (δημόσιο κλειδί), ένα πιστοποιητικό προγράμματος-πελάτη και τη ρύθμιση παραμέτρων VPN του υπολογιστή-πελάτη. 

Χρησιμοποιούμε τις παρακάτω τιμές για αυτήν τη ρύθμιση παραμέτρων. Μπορούμε να εγκαταστήσουμε τις μεταβλητές στο τμήμα [1](#declare) του άρθρου. Μπορείτε να χρησιμοποιήστε τα βήματα ως ένα Γνωρίστε και να χρησιμοποιήσετε τις τιμές χωρίς να αλλάξετε τους ή να αλλάξετε τους ώστε να αντικατοπτρίζει το περιβάλλον. 

### <a name="example"></a>Παραδείγματα τιμών

- **Όνομα: VNet1**
- **Διευθύνσεων χώρο: 192.168.0.0/16** και **10.254.0.0/16**<br>Για αυτό το παράδειγμα, χρησιμοποιούμε περισσότερα από ένα χώρο διευθύνσεων για την απεικόνιση ότι αυτή η ρύθμιση παραμέτρων θα λειτουργεί με πολλαπλά διαστήματα διεύθυνση. Ωστόσο, πολλαπλά διαστήματα διεύθυνση δεν είναι απαραίτητα για αυτήν τη ρύθμιση παραμέτρων.
- **Όνομα δευτερεύοντος δικτύου: FrontEnd**
    - **Περιοχή υποδικτύου διευθύνσεων: 192.168.1.0/24**
- **Όνομα δευτερεύοντος δικτύου: παρασκηνίου**
    - **Περιοχή υποδικτύου διευθύνσεων: 10.254.1.0/24**
- **Όνομα δευτερεύοντος δικτύου: GatewaySubnet**<br>Το όνομα του υποδικτύου *GatewaySubnet* είναι υποχρεωτική για την πύλη VPN για να εργαστείτε.
    - **Περιοχή υποδικτύου διευθύνσεων: 192.168.200.0/24** 
- **Σύνολο διευθύνσεων του προγράμματος-πελάτη VPN: 172.16.201.0/24**<br>Προγράμματα-πελάτες VPN που συνδέονται με το VNet με αυτήν τη σύνδεση σημείου σε τοποθεσία λαμβάνουν μια διεύθυνση IP από το σύνολο διευθύνσεων του προγράμματος-πελάτη VPN.
- **Συνδρομή:** Εάν έχετε περισσότερες από μία συνδρομές, βεβαιωθείτε ότι χρησιμοποιείτε το σωστό αρχείο.
- **Ομάδα πόρων: TestRG**
- **Θέση: Ανατολικής η.π.α.**
- **Διακομιστή DNS: διεύθυνση IP** από το διακομιστή DNS που θέλετε να χρησιμοποιήσετε για την επίλυση ονομάτων.
- **Όνομα GW: Vnet1GW**
- **Δημόσιο όνομα IP: VNet1GWPIP**
- **VpnType: RouteBased**

## <a name="before-beginning"></a>Πριν από την αρχή

- Βεβαιωθείτε ότι έχετε μια συνδρομή του Azure. Εάν δεν έχετε ήδη μια συνδρομή του Azure, μπορείτε να ενεργοποιήσετε το [MSDN συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ή το σύμβολο του για έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/).
    
- Εγκαταστήστε την πιο πρόσφατη έκδοση του τα cmdlet του PowerShell για τη διαχείριση πόρων Azure. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) . Όταν εργάζεστε με το PowerShell για αυτήν τη ρύθμιση παραμέτρων, βεβαιωθείτε ότι εκτελείτε ως διαχειριστής. 

## <a name="declare"></a>Τμήμα 1 - καταγραφής στο και σύνολο μεταβλητών

Σε αυτήν την ενότητα, μπορείτε να συνδεθείτε και δηλώνουν τις τιμές που χρησιμοποιούνται για αυτήν τη ρύθμιση παραμέτρων. Η δήλωση τιμές χρησιμοποιούνται σε τα δείγματα δεσμών ενεργειών. Αλλάξτε τις τιμές ώστε να αντικατοπτρίζει το δικό σας περιβάλλον. Εναλλακτικά, μπορείτε να χρησιμοποιήσετε τη δήλωση τιμές και να ακολουθήσετε τα βήματα ως άσκηση.

1. Στην κονσόλα του PowerShell, συνδεθείτε στο λογαριασμό σας στο Azure. Αυτό το cmdlet ζητά για τα διαπιστευτήρια σύνδεσης στο λογαριασμό σας στο Azure. Μετά την καταγραφή στο, να κάνει λήψη ρυθμίσεις του λογαριασμού σας, έτσι ώστε να είναι διαθέσιμες για Azure PowerShell.

        Login-AzureRmAccount 

2. Λάβετε μια λίστα με όλες τις συνδρομές σας Azure.

        Get-AzureRmSubscription

3. Καθορίστε τη συνδρομή στην οποία θέλετε να χρησιμοποιήσετε. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Για να δηλώσετε τις μεταβλητές που θέλετε να χρησιμοποιήσετε. Χρησιμοποιήστε το παρακάτω παράδειγμα, αντικαθιστώντας τις τιμές για το δικό όταν είναι απαραίτητο.

        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"
        
## <a name="ConfigureVNet"></a>Μέρος 2 - ρύθμιση παραμέτρων ενός VNet 

1. Δημιουργία μιας ομάδας πόρων.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Δημιουργήστε τις ρυθμίσεις παραμέτρων υποδικτύου για το εικονικό δίκτυο, ονομασία τους *FrontEnd*, *παρασκηνίου*και *GatewaySubnet*. Αυτά τα προθέματα πρέπει να είναι μέρος του χώρου διευθύνσεων VNet που που έχουν δηλωθεί.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Δημιουργήστε το εικονικό δίκτυο. Ο διακομιστής DNS που καθορίζεται πρέπει να είναι ένα διακομιστή DNS που μπορεί να επιλύσει τα ονόματα για τους πόρους που πρόκειται να συνδεθείτε με. Για αυτό το παράδειγμα, χρησιμοποιήσαμε μια δημόσια διεύθυνση IP. Φροντίστε να χρησιμοποιήσετε τις δικές σας τιμές.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Καθορίστε τις μεταβλητές για το εικονικό δίκτυο που δημιουργήσατε.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Αίτηση για μια δυναμική αντιστοίχιση δημόσια διεύθυνση IP. Αυτή η διεύθυνση IP είναι απαραίτητο για την πύλη για να λειτουργήσει σωστά. Αργότερα, μπορείτε να συνδέσετε την πύλη για τη ρύθμιση παραμέτρων IP πύλης.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="Certificates"></a>Τμήμα 3 - πιστοποιητικών

Τα πιστοποιητικά χρησιμοποιούνται από Azure για τον έλεγχο ταυτότητας προγραμμάτων-πελατών VPN για VPN σημείου σε τοποθεσία. Μπορείτε να εξαγάγετε δεδομένα πιστοποιητικό δημόσιου (μην το ιδιωτικό κλειδί) όπως μια βάσης 64 κωδικοποιημένο X.509 αρχείο .cer από ένα πιστοποιητικό ρίζας που δημιουργούνται από μια λύση πιστοποιητικό για μεγάλες επιχειρήσεις ή ένα πιστοποιητικό αυτόματης υπογραφής ρίζας. Στη συνέχεια, εισαγάγετε τα δεδομένα πιστοποιητικό δημόσιου από το ριζικό κατάλογο πιστοποιητικό για να Azure. Επιπλέον, πρέπει να δημιουργήσετε ένα πιστοποιητικό προγράμματος-πελάτη από το πιστοποιητικό ρίζας για προγράμματα-πελάτες. Κάθε υπολογιστή-πελάτη που θέλει να συνδεθείτε με το εικονικό δίκτυο με χρήση σύνδεσης P2S πρέπει να έχετε ένα πρόγραμμα-πελάτη εγκατεστημένο πιστοποιητικό που δημιουργήθηκε από το πιστοποιητικό ρίζας.

### <a name="cer"></a>1. Κάντε λήψη του αρχείου .cer για το πιστοποιητικό ρίζας

Θα πρέπει να λαμβάνουν τα δεδομένα πιστοποιητικό δημόσιου του πιστοποιητικού ρίζας που θέλετε να χρησιμοποιήσετε.

- Εάν χρησιμοποιείτε ένα σύστημα πιστοποιητικό για μεγάλες επιχειρήσεις, αποκτήστε το αρχείο .cer του πιστοποιητικού ρίζας που θέλετε να χρησιμοποιήσετε. 

- Εάν δεν χρησιμοποιείτε μια λύση πιστοποιητικό για μεγάλες επιχειρήσεις, πρέπει να δημιουργήσετε ένα πιστοποιητικό αυτόματης υπογραφής ρίζας. Για τα Windows 10, μπορείτε να ανατρέξετε στην [εργασία με πιστοποιητικών ρίζας αυτο-υπογεγραμμένο για ρυθμίσεις παραμέτρων σημείου σε τοποθεσία](vpn-gateway-certificates-point-to-site.md).


1. Για να αποκτήσετε ένα αρχείο .cer από ένα πιστοποιητικό, ανοίξτε **certmgr.msc** και εντοπίστε το πιστοποιητικό ρίζας. Κάντε δεξί κλικ το αυτο-υπογεγραμμένο πιστοποιητικό, κάντε κλικ στην επιλογή **όλες τις εργασίες**και, στη συνέχεια, κάντε κλικ στην επιλογή **Εξαγωγή**. Έτσι ανοίγει το **Πιστοποιητικό του "Οδηγού εξαγωγής"**.

2. Στον οδηγό, κάντε κλικ στο κουμπί **Επόμενο**, επιλέξτε **Όχι, εξαγωγή το ιδιωτικό κλειδί**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

3. Στη σελίδα **Μορφή αρχείου εξαγωγής** , επιλέξτε **βάσης 64 κωδικοποιημένο X.509 (. Προαιρετικό).** Στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. 

4. Στην του **αρχείου προς εξαγωγή**, **μεταβείτε** στη θέση στην οποία θέλετε να εξαγάγετε το πιστοποιητικό. Για το **όνομα του αρχείου**, ονομάστε το αρχείο πιστοποιητικού. Στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

5. Κάντε κλικ στο κουμπί **Τέλος** για να εξαγάγετε το πιστοποιητικό.

### <a name="generate"></a>2. Δημιουργία του πιστοποιητικού προγράμματος-πελάτη

Στη συνέχεια, να δημιουργήσετε τα πιστοποιητικά προγράμματος-πελάτη. Μπορείτε να δημιουργήσετε είτε ένα μοναδικό πιστοποιητικό για κάθε πελάτη που θα συνδεθεί ή μπορείτε να χρησιμοποιήσετε το ίδιο πιστοποιητικό σε πολλά προγράμματα-πελάτες. Το πλεονέκτημα δημιουργό πιστοποιητικά μοναδικό προγράμματος-πελάτη είναι η δυνατότητα να ανακαλέσετε ενός πιστοποιητικού, εάν είναι απαραίτητο. Διαφορετικά, εάν όλοι χρησιμοποιεί το ίδιο πιστοποιητικό προγράμματος-πελάτη και μπορείτε να βρείτε ότι πρέπει να ανακαλέσετε το πιστοποιητικό για έναν υπολογιστή-πελάτη, θα πρέπει να δημιουργήσετε και να εγκαταστήσετε νέα πιστοποιητικά για όλα τα προγράμματα-πελάτες που χρησιμοποιούν το πιστοποιητικό για τον έλεγχο ταυτότητας. Τα πιστοποιητικά προγράμματος-πελάτη είναι εγκατεστημένα σε κάθε υπολογιστή-πελάτη αργότερα σε αυτήν την άσκηση.

- Εάν χρησιμοποιείτε μια λύση πιστοποιητικό για μεγάλες επιχειρήσεις, να δημιουργήσετε ένα πιστοποιητικό προγράμματος-πελάτη με την συνηθισμένη μορφή τιμή όνομα 'name@yourdomain.com', και όχι το NetBIOS 'Τομέας\όνομα χρήστη' μορφοποίηση. 

- Εάν χρησιμοποιείτε ένα πιστοποιητικό αυτόματης υπογραφής, ανατρέξτε στο θέμα [εργασία με πιστοποιητικών ρίζας αυτο-υπογεγραμμένο για ρυθμίσεις παραμέτρων σημείου σε τοποθεσία](vpn-gateway-certificates-point-to-site.md) για να δημιουργήσετε ένα πιστοποιητικό προγράμματος-πελάτη.

### <a name="exportclientcert"></a>3. εξαγάγετε το πιστοποιητικό προγράμματος-πελάτη

Απαιτείται ένα πιστοποιητικό προγράμματος-πελάτη για έλεγχο ταυτότητας. Μετά από τη δημιουργία του πιστοποιητικού προγράμματος-πελάτη, εξαγάγετε. Το πιστοποιητικό προγράμματος-πελάτη εξαγωγή θα εγκατασταθεί αργότερα σε κάθε υπολογιστή-πελάτη.

1. Για να εξαγάγετε ένα πιστοποιητικό προγράμματος-πελάτη, μπορείτε να χρησιμοποιήσετε *certmgr.msc*. Κάντε δεξί κλικ στο πιστοποιητικό προγράμματος-πελάτη που θέλετε να εξαγάγετε, κάντε κλικ στην επιλογή **όλες τις εργασίες**και, στη συνέχεια, κάντε κλικ στην επιλογή **Εξαγωγή**.
2. Εξαγάγετε το πιστοποιητικό προγράμματος-πελάτη με το ιδιωτικό κλειδί. Αυτό είναι ένα αρχείο *.pfx* . Βεβαιωθείτε ότι η εγγραφή ή να θυμηθείτε τον κωδικό πρόσβασης (πλήκτρο) που έχετε ορίσει για αυτό το πιστοποιητικό.

### <a name="upload"></a>4. αποστείλετε το αρχείο .cer πιστοποιητικού ρίζας

Δηλώστε την μεταβλητή για το όνομα πιστοποιητικού, αντικαθιστώντας την τιμή με το δικό σας:

        $P2SRootCertName = "Mycertificatename.cer"

Προσθέστε τα δεδομένα δημόσια πιστοποιητικό για το πιστοποιητικό ρίζας Azure. Μπορείτε να αποστείλετε αρχεία για έως και 20 πιστοποιητικών ρίζας. Μην αποστέλλετε ιδιωτικό κλειδί για το πιστοποιητικό ρίζας Azure. Εφόσον το αρχείο .cer αποσταλεί, το Azure χρησιμοποιεί για τον έλεγχο ταυτότητας προγράμματα-πελάτες που συνδέονται με το εικονικό δίκτυο. 

Αντικαταστήστε τη διαδρομή του αρχείου με το δικό σας και, στη συνέχεια, εκτελέστε το cmdlet.
    
        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Μέρος 4 - δημιουργία της πύλης VPN

Ρύθμιση παραμέτρων και Δημιουργία πύλης εικονικού δικτύου για το VNet. Το *-GatewayType* πρέπει να είναι **Vpn** και το *-VpnType* πρέπει να είναι **RouteBased**. Αυτό μπορεί να διαρκέσει έως και 45 λεπτά για να ολοκληρωθεί.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Μέρος 5 - λήψη του πακέτου ρύθμισης παραμέτρων προγράμματος-πελάτη VPN

Προγράμματα-πελάτες τη σύνδεση με χρήση P2S Azure πρέπει να έχετε ένα πιστοποιητικό προγράμματος-πελάτη και ένα πακέτο ρύθμισης παραμέτρων προγράμματος-πελάτη VPN εγκατεστημένο. Πακέτα ρύθμισης παραμέτρων προγράμματος-πελάτη VPN είναι διαθέσιμα για προγράμματα-πελάτες των Windows. Το πακέτο του προγράμματος-πελάτη VPN περιέχει πληροφορίες για τη ρύθμιση παραμέτρων του λογισμικού προγράμματος-πελάτη VPN που είναι ενσωματωμένο στο Windows και ειδικά για το VPN που θέλετε να συνδεθείτε. Το πακέτο δεν εγκαθιστά πρόσθετο λογισμικό. Ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις πύλης VPN](vpn-gateway-vpn-faq.md#point-to-site-connections) για περισσότερες πληροφορίες.

1. Μετά τη δημιουργία της πύλης, μπορείτε να κάνετε λήψη του πακέτου ρύθμισης παραμέτρων προγράμματος-πελάτη. Αυτό το παράδειγμα κάνει λήψη του πακέτου για προγράμματα-πελάτες 64-bit. Εάν θέλετε να κάνετε λήψη του προγράμματος-πελάτη 32 bit, αντικαταστήστε 'Amd64' με 'x86'. Μπορείτε επίσης να κάνετε λήψη του υπολογιστή-πελάτη VPN, χρησιμοποιώντας την πύλη του Azure.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. Το cmdlet του PowerShell επιστρέφει μια σύνδεση URL. Αυτό είναι ένα παράδειγμα του τι το επιστρεφόμενο είναι διεύθυνση URL όπως:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Αντιγράψτε και επικολλήστε τη σύνδεση που επιστρέφονται από ένα πρόγραμμα περιήγησης web για να κάνετε λήψη του πακέτου. Στη συνέχεια, εγκαταστήστε το πακέτο του υπολογιστή-πελάτη. Εάν λάβετε ένα αναδυόμενο παράθυρο SmartScreen, κάντε κλικ στην επιλογή **περισσότερες πληροφορίες**, στη συνέχεια, **εκτελέστε οπωσδήποτε** για να εγκαταστήσετε το πακέτο.

4. Στον υπολογιστή-πελάτη, μεταβείτε στις **Ρυθμίσεις δικτύου** και κάντε κλικ στην επιλογή **VPN**. Θα δείτε τη σύνδεση που παρατίθενται. Θα εμφανίζεται το όνομα του εικονικού δικτύου που θα συνδεθεί και είναι παρόμοιο με αυτό το παράδειγμα: 

    ![Υπολογιστή-πελάτη VPN] (./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "Υπολογιστή-πελάτη VPN")


## <a name="clientcertificate"></a>Τμήμα 6 - εγκατάσταση του πιστοποιητικού προγράμματος-πελάτη

Κάθε υπολογιστής-πελάτης πρέπει να έχετε ένα πιστοποιητικό προγράμματος-πελάτη για να τον έλεγχο ταυτότητας. Κατά την εγκατάσταση του πιστοποιητικού προγράμματος-πελάτη, θα χρειαστείτε τον κωδικό πρόσβασης που δημιουργήθηκε κατά την εξαγωγή του πιστοποιητικού προγράμματος-πελάτη.

1. Αντιγράψτε το αρχείο .pfx στον υπολογιστή-πελάτη.
2. Κάντε διπλό κλικ το αρχείο .pfx για να την εγκαταστήσετε. Μην τροποποιείτε τη θέση εγκατάστασης.

## <a name="connect"></a>Μέρος 7 - σύνδεση με Azure

1. Για να συνδεθείτε με το VNet, στον υπολογιστή-πελάτη, μεταβείτε στις συνδέσεις VPN και εντοπίστε τη σύνδεση VPN που δημιουργήσατε. Ονομάζεται το ίδιο όνομα με το εικονικό δίκτυο. Κάντε κλικ στην επιλογή **σύνδεση**. Ενδέχεται να εμφανιστεί ένα αναδυόμενο μήνυμα που αναφέρεται σε χρησιμοποιώντας το πιστοποιητικό. Αν συμβεί αυτό, κάντε κλικ στο κουμπί **συνέχεια** για να χρησιμοποιήσετε αναβαθμισμένα δικαιώματα. 

2. Στη σελίδα κατάστασης **σύνδεσης** , κάντε κλικ στην επιλογή **σύνδεση** για να ξεκινήσετε τη σύνδεση. Εάν εμφανιστεί μια οθόνη **Επιλογή πιστοποιητικού** , βεβαιωθείτε ότι το πιστοποιητικό προγράμματος-πελάτη που εμφανίζει είναι αυτό που θέλετε να χρησιμοποιήσετε για να συνδεθείτε. Εάν δεν είναι, χρησιμοποιήστε το αναπτυσσόμενο βέλος για να επιλέξετε το σωστό πιστοποιητικό και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

    ![Σύνδεση του υπολογιστή-πελάτη VPN] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Σύνδεση του υπολογιστή-πελάτη VPN")

3. Σας θα πρέπει τώρα δυνατή η σύνδεση.

    ![Δημιουργία σύνδεσης] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Δημιουργία σύνδεσης")

## <a name="verify"></a>Μέρος 8 - Ελέγξτε τη σύνδεσή σας

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

## <a name="addremovecert"></a>Για να προσθέσετε ή να καταργήσετε ενός αξιόπιστου πιστοποιητικού ρίζας

Τα πιστοποιητικά που χρησιμοποιούνται για τον έλεγχο ταυτότητας προγραμμάτων-πελατών VPN για VPN σημείου σε τοποθεσία. Τα παρακάτω βήματα θα σας καθοδηγεί στην προσθήκη και Κατάργηση πιστοποιητικών ρίζας. Όταν προσθέτετε ένα αρχείο κωδικοποίηση Base64 X.509 (.cer) για να Azure, δίνετε Azure αξιόπιστο το πιστοποιητικό ρίζας που αντιπροσωπεύει το αρχείο. 

Μπορείτε να προσθέσετε ή να καταργήσετε αξιόπιστων πιστοποιητικών ρίζας με χρήση του PowerShell ή στην πύλη του Azure. Εάν θέλετε να το κάνετε αυτό με την πύλη Azure, μεταβείτε στο σας **πύλης εικονικού δικτύου > Ρυθμίσεις > Ρύθμιση παραμέτρων σημείου σε τοποθεσία > πιστοποιητικών ρίζας**. Ακολουθήστε τα παρακάτω βήματα θα σας καθοδηγήσουν αυτές τις εργασίες με χρήση του PowerShell. 

### <a name="add-a-trusted-root-certificate"></a>Προσθήκη ενός αξιόπιστου πιστοποιητικού ρίζας

Μπορείτε να προσθέσετε έως και 20 αρχεία .cer πιστοποιητικών αξιόπιστης ρίζας σε Azure. Ακολουθήστε τα παρακάτω βήματα για να προσθέσετε ένα πιστοποιητικό ρίζας.

1. Δημιουργία και προετοιμασία του νέου πιστοποιητικού ρίζας που θα προσθέσετε στο Azure. Εξαγάγετε το δημόσιο κλειδί, όπως μια βάσης 64 κωδικοποιημένο X.509 (. Προαιρετικό) και να το ανοίξετε με ένα πρόγραμμα επεξεργασίας κειμένου. Στη συνέχεια, αντιγράψτε μόνο την ενότητα που φαίνεται παρακάτω. 
 
    Αντιγράψτε τις τιμές, όπως φαίνεται στο ακόλουθο παράδειγμα:

    ![πιστοποιητικό] (./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "πιστοποιητικό")
    
2. Καθορίστε το όνομα πιστοποιητικό και βασικές πληροφορίες ως μεταβλητή. Αντικαταστήστε τις πληροφορίες με δική σας, όπως φαίνεται στο το ακόλουθο παράδειγμα:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Προσθήκη νέου πιστοποιητικού ρίζας. Μπορείτε να προσθέσετε μόνο ένα πιστοποιητικό κάθε φορά.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Μπορείτε να επαληθεύσετε ότι το νέο πιστοποιητικό προστέθηκαν σωστά, χρησιμοποιώντας το ακόλουθο cmdlet.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Για να καταργήσετε ενός αξιόπιστου πιστοποιητικού ρίζας

Μπορείτε να καταργήσετε αξιόπιστου πιστοποιητικού ρίζας από το Azure. Όταν καταργείτε ένα αξιόπιστο πιστοποιητικό, τα πιστοποιητικά προγράμματος-πελάτη που δημιουργήθηκαν από το πιστοποιητικό ρίζας δεν είναι πλέον θα μπορείτε να συνδεθείτε στο Azure μέσω του σημείου σε τοποθεσία. Εάν θέλετε οι υπολογιστές-πελάτες για να συνδεθείτε, πρέπει να εγκαταστήσετε ένα νέο πιστοποιητικό προγράμματος-πελάτη που δημιουργείται από ένα πιστοποιητικό που είναι αξιόπιστα στο Azure.

1. Για να καταργήσετε ενός αξιόπιστου πιστοποιητικού ρίζας, τροποποιήστε το παρακάτω παράδειγμα:

    Για να δηλώσετε τις μεταβλητές.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Κατάργηση του πιστοποιητικού.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Χρησιμοποιήστε το ακόλουθο cmdlet για να βεβαιωθείτε ότι το πιστοποιητικό έχει καταργηθεί με επιτυχία. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>Για να διαχειριστείτε τη λίστα ανάκλησης πιστοποιητικών προγράμματος-πελάτη

Μπορείτε να ανακαλέσετε πιστοποιητικά προγράμματος-πελάτη. Λίστα ανάκλησης πιστοποιητικών σάς επιτρέπει να αρνηθεί επιλεκτικής συνδεσιμότητας σημείου σε τοποθεσία που βασίζεται σε μεμονωμένα πιστοποιητικά πελατών. Αν καταργήσετε ένα .cer πιστοποιητικού ρίζας από το Azure, κάνει ανάκληση την πρόσβαση για όλα τα πιστοποιητικά του προγράμματος-πελάτη που δημιουργούνται/συνδεδεμένοι με το πιστοποιητικό έχει ανακληθεί ρίζας. Εάν θέλετε να ανακαλέσετε ένα πιστοποιητικό συγκεκριμένο πρόγραμμα-πελάτη, δεν τον ριζικό κατάλογο, μπορείτε να το κάνετε. Με αυτόν τον τρόπο που τα άλλα πιστοποιητικά που δημιουργήθηκαν από το πιστοποιητικό ρίζας θα εξακολουθεί να είναι έγκυρες.

Η κοινή πρακτική είναι να χρησιμοποιήσετε το πιστοποιητικό ρίζας για να διαχειριστείτε την πρόσβαση σε επίπεδο ομάδας ή του οργανισμού, κατά τη χρήση του προγράμματος-πελάτη έχει ανακληθεί πιστοποιητικά για έλεγχο πρόσβασης σε λεπτομερή σε μεμονωμένους χρήστες.

### <a name="revoke-a-client-certificate"></a>Ανάκληση πιστοποιητικού προγράμματος-πελάτη

1. Λάβετε την αποτύπωση του πιστοποιητικού προγράμματος-πελάτη για να ανακαλέσετε.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Προσθέστε την αποτύπωση στη λίστα των έχει ανακληθεί αποτύπωση.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Βεβαιωθείτε ότι η αποτύπωση προστέθηκε στη λίστα ανάκλησης πιστοποιητικών. Πρέπει να προσθέσετε μία αποτύπωση κάθε φορά.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Επαναφέρει ένα πιστοποιητικό προγράμματος-πελάτη

Μπορείτε να επαναφέρετε ένα πιστοποιητικό προγράμματος-πελάτη, καταργώντας την αποτύπωση από τη λίστα των πιστοποιητικών έχει ανακληθεί προγράμματος-πελάτη.

1.  Καταργήστε την αποτύπωση από τη λίστα των αποτύπωση πιστοποιητικό έχει ανακληθεί προγράμματος-πελάτη.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Ελέγξτε εάν την αποτύπωση έχει καταργηθεί από τη λίστα έχει ανακληθεί.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να προσθέσετε μια εικονική μηχανή στο δίκτυο του εικονικού σας. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .