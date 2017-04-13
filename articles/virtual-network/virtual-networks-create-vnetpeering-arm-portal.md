<properties
   pageTitle="Δημιουργία VNet διεισδύουν με την πύλη Azure | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα εικονικό δίκτυο με την πύλη Azure στη Διαχείριση πόρων."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-a-virtual-network-peering-using-the-azure-portal"></a>Δημιουργία μιας διεισδύουν εικονικού δικτύου με την πύλη Azure

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Για να δημιουργήσετε ένα VNet διεισδύουν με βάση το σενάριο παραπάνω, χρησιμοποιώντας την πύλη του Azure, ακολουθήστε τα παρακάτω βήματα.

1. Από ένα πρόγραμμα περιήγησης, μεταβείτε στο http://portal.azure.com και, εάν είναι απαραίτητο, πραγματοποιήστε είσοδο με το λογαριασμό της Azure.
2. Για να δημιουργήσετε VNET διεισδύουν, πρέπει να δημιουργήσετε δύο συνδέσεις, έναν για κάθε κατεύθυνση, μεταξύ δύο VNets. Μπορείτε να δημιουργήσετε σύνδεση διεισδύουν VNET για VNET1 να VNET2 πρώτα. Στην πύλη, κάντε κλικ στην επιλογή **Αναζήτηση** > **Επιλέξτε εικονικών δικτύων**

    ![Δημιουργία VNet διεισδύουν στην πύλη του Azure](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. Στο blade εικονικών δικτύων, επιλέξτε VNET1, κάντε κλικ στην επιλογή Peerings, στη συνέχεια, κάντε κλικ στην επιλογή Προσθήκη

    ![Επιλέξτε διεισδύουν](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. Στο blade Προσθήκη διεισδύουν, δώστε ένα όνομα σύνδεσης peering LinkToVnet2, επιλέξτε τη συνδρομή και το ομότιμης σύνδεσης εικονικού VNET2 δικτύου, κάντε κλικ στο κουμπί OK.

    ![Σύνδεση με VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Όταν δημιουργηθεί αυτή τη σύνδεση peering VNET. Μπορείτε να δείτε την κατάσταση σύνδεσης ως εξής:

    ![Κατάσταση σύνδεσης](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Στη συνέχεια να δημιουργήσετε τη σύνδεση peering VNET για VNET2 να VNET1. Στο blade εικονικών δικτύων, επιλέξτε VNET2, κάντε κλικ στην επιλογή Peerings, στη συνέχεια, κάντε κλικ στην επιλογή Προσθήκη

    ![Ομότιμης σύνδεσης από άλλες VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. Στο blade Προσθήκη διεισδύουν, δώστε ένα όνομα σύνδεσης peering LinkToVnet1, επιλέξτε τη συνδρομή και το ομότιμης σύνδεσης εικονικού δικτύου, κάντε κλικ στο κουμπί OK.

    ![Δημιουργία πλακίδιο εικονικού δικτύου](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Όταν δημιουργηθεί αυτή τη σύνδεση peering VNET. Μπορείτε να δείτε την κατάσταση σύνδεσης ως εξής:

    ![Κατάστασης τελικό σύνδεσης](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. Έλεγχος της κατάστασης για LinkToVnet2 και τώρα μετατρέπεται σε σύνδεση καθώς και.  

    ![Κατάσταση σύνδεσης τελικό 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

    > [AZURE.NOTE] VNET διεισδύουν καθορίζεται μόνο αν είστε συνδεδεμένοι και οι δύο συνδέσεις.

Υπάρχουν μερικές ιδιότητες με δυνατότητα ρύθμισης παραμέτρων για κάθε σύνδεση:

|Επιλογή|Περιγραφή|Προεπιλεγμένη|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Εάν διευθύνσεων χώρο του VNet ομότιμης σύνδεσης που θα συμπεριληφθούν ως τμήμα της ετικέτας Virtual_network|Ναι|
|AllowForwardedTraffic|Εάν η κίνηση δεν που προέρχονται από μια peered VNet αποδοχή ή αποτεθεί|Όχι|
|AllowGatewayTransit|Επιτρέπει την ομότιμης σύνδεσης VNet για να χρησιμοποιήσετε την πύλη VNet|Όχι|
|UseRemoteGateways|Χρήση πύλης VNet σας ομότιμης σύνδεσης. Το ομότιμης σύνδεσης VNet πρέπει να έχετε μια πύλη έχει ρυθμιστεί και AllowGatewayTransit είναι επιλεγμένο. Δεν μπορείτε να χρησιμοποιήσετε αυτήν την επιλογή, εάν έχετε μια πύλη έχει ρυθμιστεί|Όχι|

Κάθε σύνδεση στο VNet διεισδύουν περιλαμβάνει ένα σύνολο επάνω από τις ιδιότητες. Από την πύλη, μπορείτε να κάντε κλικ στη σύνδεση διεισδύουν VNet και να αλλάξετε τις διαθέσιμες επιλογές, κάντε κλικ στο κουμπί Αποθήκευση για να κάνετε την αλλαγή εφέ.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. Από ένα πρόγραμμα περιήγησης, μεταβείτε στο http://portal.azure.com και, εάν είναι απαραίτητο, πραγματοποιήστε είσοδο με το λογαριασμό της Azure.
2. Σε αυτό το παράδειγμα θα χρησιμοποιήσουμε δύο συνδρομές A και B και δύο χρήστες ΧρήστηΑ και δικαιώματα με δικαιώματα στο τις συνδρομές αντίστοιχα
3. Στην πύλη, κάντε κλικ στο κουμπί Αναζήτηση, επιλέξτε εικονικού δίκτυα. Κάντε κλικ στην επιλογή το VNET και κάντε κλικ στην επιλογή Προσθήκη.

    ![Σενάριο 2 αναζήτηση](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. Στην blade access την προσθήκη, κάντε κλικ στην επιλογή ένα ρόλο και επιλέξτε συμβολής δικτύου, κάντε κλικ στην επιλογή Προσθήκη χρηστών, πληκτρολογήστε το σύμβολο δικαιώματα στο πλαίσιο όνομα και κάντε κλικ στο κουμπί OK.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    Δεν είναι η απαίτηση, διεισδύουν μπορεί να πραγματοποιηθεί ακόμα και εάν οι χρήστες μεμονωμένα αυξήσετε διεισδύουν αιτήσεις τους αντίστοιχους Vnets με την προϋπόθεση ότι ταιριάζουν με τις αιτήσεις. Προσθήκη χρήστη με δικαιώματα από τα άλλα VNet ως χρήστες στο το τοπικό VNet διευκολύνει ρύθμισης στην πύλη.

5. Στη συνέχεια, συνδεθείτε στο Azure πύλη με δικαιώματα που είναι ο χρήστης δικαίωμα για SubscriptionB. Ακολουθήστε την παραπάνω βήματα για να προσθέσετε ΧρήστηΑ ως συνεργάτης δικτύου.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    > [AZURE.NOTE] Μπορείτε να αποσυνδεθείτε και να συνδεθείτε σε δύο περιόδους λειτουργίας χρήστη στο πρόγραμμα περιήγησης για να βεβαιωθείτε ότι είναι ενεργοποιημένη με επιτυχία την άδεια.

6. Συνδεθείτε στην πύλη του ως ΧρήστηΑ, μεταβείτε στο το blade VNET3, κάντε κλικ στην επιλογή Peering, έλεγχος ' ξέρω το Αναγνωριστικό πόρου "το πλαίσιο ελέγχου και πληκτρολογήστε τον πόρο ΑΝΑΓΝΩΡΙΣΤΙΚΌ για VNET5 σε μορφή.

    / subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Network/VirtualNetwork/{VNETname}

    ![Αναγνωριστικό πόρου](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. Συνδεθείτε στην πύλη του ως δικαιώματα και παρακολούθηση παραπάνω βήμα για να δημιουργήσετε σύνδεση διεισδύουν από VNET5 VNet3.

    ![Αναγνωριστικό πόρου 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Διεισδύουν πρόκειται να εγκατασταθούν και οποιαδήποτε εικονική μηχανή στο VNet3 θα πρέπει να μπορούν να επικοινωνούν με οποιαδήποτε εικονική μηχανή στο VNet5

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Ως πρώτο βήμα, VNET διεισδύουν συνδέσεις από HubVnet σε VNET1. Σημειώστε ότι η επιλογή επιτρέψετε την κυκλοφορία προωθούνται δεν είναι επιλεγμένο για τη σύνδεση.

    ![Βασική διεισδύουν](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. Ως επόμενο βήμα, μπορούν να δημιουργηθούν διεισδύουν συνδέσεις από VNET1 σε HubVnet. Σημειώστε ότι είναι ενεργοποιημένη η επιλογή κίνηση δυνατότητα προωθούνται.

    ![Βασική διεισδύουν](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Αφού δημιουργηθεί διεισδύουν, μπορείτε να αναφέρονται σε αυτό το [άρθρο](virtual-network-create-udr-arm-ps.md) και να ορίσετε Route(UDR) που ορίζονται από το χρήστη για να ανακατευθύνετε την κίνηση VNet1 μέσω μια εικονική συσκευή για να χρησιμοποιήσετε τις δυνατότητές του. Όταν μπορείτε να καθορίσετε τη διεύθυνση επόμενου άλματος στο θέμα δρομολόγηση, μπορείτε να τη ρυθμίσετε με τη διεύθυνση ΙΡ εικονικού συσκευής στο ομότιμης σύνδεσης VNet HubVNet


[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]



1. Από ένα πρόγραμμα περιήγησης, μεταβείτε στο http://portal.azure.com και, εάν είναι απαραίτητο, πραγματοποιήστε είσοδο με το λογαριασμό της Azure.

2. Για να δημιουργήσετε VNET διεισδύουν σε αυτό το σενάριο, πρέπει να δημιουργήσετε μόνο μία σύνδεση, από το εικονικό δίκτυο στη Διαχείριση Azure πόρων με εκείνη στην κλασική. Αυτό σημαίνει ότι, από **VNET1** σε **VNET2**. Στην πύλη, κάντε κλικ στην επιλογή **Αναζήτηση** > Επιλέξτε **Εικονικών δικτύων**

3. Στο τα δίκτυα blade εικονικές, επιλέξτε **VNET1**. Κάντε κλικ στην επιλογή **Peerings**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

4. Στο blade Προσθήκη διεισδύουν, δώστε ένα όνομα της σύνδεσης. Δείτε εδώ ονομάζεται **LinkToVNet2**. Στην περιοχή Λεπτομέρειες ομότιμης σύνδεσης, επιλέξτε **κλασική**.

5. Στη συνέχεια, επιλέξτε τη συνδρομή και το ομότιμης σύνδεσης εικονικού δικτύου **VNET2**. Στη συνέχεια, κάντε κλικ στο κουμπί OK.

    ![Σύνδεση Vnet1 Vnet 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)

6. Αφού δημιουργηθεί αυτή τη σύνδεση peering VNet, των δύο εικονικού δικτύων είναι peered και θα μπορείτε να δείτε τα εξής:

    ![Έλεγχος διεισδύουν σύνδεσης](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)


## <a name="remove-vnet-peering"></a>Κατάργηση διεισδύουν VNet

1.  Από ένα πρόγραμμα περιήγησης, μεταβείτε στο http://portal.azure.com και, εάν είναι απαραίτητο, πραγματοποιήστε είσοδο με το λογαριασμό της Azure.
2.  Μεταβείτε στην blade εικονικού δικτύου, κάντε κλικ στην επιλογή Peerings, κάντε κλικ στη σύνδεση που θέλετε να καταργήσετε, κάντε κλικ στο κουμπί Διαγραφή.

    ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Αφού καταργήσετε μια σύνδεση στο διεισδύουν VNET, την κατάσταση σύνδεσης ομότιμης σύνδεσης θα μεταβείτε αποσύνδεσης.

    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. Σε αυτήν την κατάσταση, δεν μπορείτε να δημιουργήσετε εκ νέου τη σύνδεση μέχρι να αλλάξει η κατάσταση σύνδεσης ομότιμης σύνδεσης σε ξεκίνησε. Συνιστάται να καταργήσετε τις δύο συνδέσεις πριν να δημιουργήσετε ξανά το διεισδύουν VNET.
