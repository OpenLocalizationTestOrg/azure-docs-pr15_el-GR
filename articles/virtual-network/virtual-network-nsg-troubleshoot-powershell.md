<properties 
   pageTitle="Αντιμετώπιση προβλημάτων με τις ομάδες ασφαλείας δικτύου - PowerShell | Microsoft Azure"
   description="Μάθετε τον τρόπο αντιμετώπισης προβλημάτων δικτύου ομάδες ασφαλείας στο μοντέλο ανάπτυξης διαχείρισης πόρων Azure χρησιμοποιώντας το Azure PowerShell."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Αντιμετώπιση προβλημάτων με χρήση του Azure PowerShell τις ομάδες ασφαλείας δικτύου

> [AZURE.SELECTOR]
- [Πύλη του Azure](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Εάν έχετε ρυθμίσει τις παραμέτρους δικτύου ομάδες ασφαλείας (NSGs) στον υπολογιστή σας εικονικές (Εικονική) και αντιμετωπίζετε προβλήματα συνδεσιμότητας Εικονική, αυτό το άρθρο παρέχει μια επισκόπηση των δυνατοτήτων Διαγνωστικά για NSGs για να σας βοηθήσει να αντιμετωπίσετε περαιτέρω.

NSGs σάς επιτρέπουν να ελέγχετε τους τύπους κίνησης που ροής και έξοδος από το εικονικές μηχανές (ΣΠΣ). NSGs μπορούν να εφαρμοστούν σε δευτερεύοντα δίκτυα σε ένα δίκτυο εικονικού Azure (VNet), διασυνδέσεις δικτύου (NIC) ή και τα δύο. Οι αποτελεσματικές κανόνες που εφαρμόζονται στο NIC είναι μια συγκέντρωση των κανόνων που υπάρχουν στο το NSGs εφαρμοστεί NIC και το υποδίκτυο είναι συνδεδεμένος. Κανόνες σε αυτές τις NSGs μερικές φορές μπορεί να έρχονται σε διένεξη μεταξύ τους και να επηρεάσουν μια Εικονική η συνδεσιμότητα του δικτύου.  

Μπορείτε να προβάλετε όλους τους κανόνες ισχύος ασφαλείας από το NSGs, όπως εφαρμόζεται στο NIC σας Εικονική. Αυτό το άρθρο παρουσιάζει τον τρόπο αντιμετώπισης προβλημάτων συνδεσιμότητας Εικονική με τη χρήση αυτών των κανόνων στο μοντέλο ανάπτυξης Azure διαχείριση πόρων. Εάν δεν είστε εξοικειωμένοι με VNet και NSG έννοιες, διαβάστε τα άρθρα Επισκόπηση [εικονικές δικτύου](virtual-networks-overview.md) και [τις ομάδες ασφαλείας δικτύου](virtual-networks-nsg.md) .

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Χρήση κανόνων αποτελεσματικές ασφαλείας για την αντιμετώπιση προβλημάτων ροής κυκλοφορίας Εικονική

Το σενάριο που ακολουθεί είναι ένα παράδειγμα του ένα κοινό θέμα σύνδεσης:

Μια Εικονική με το όνομα *VM1* αποτελεί τμήμα ενός δευτερεύοντος δικτύου με το όνομα *Subnet1* μέσα σε μια VNet με το όνομα *WestUS VNet1*. Η προσπάθεια για να συνδεθείτε με τη χρήση RDP μέσω αλλάξει Εικονική αποτυγχάνει. NSGs εφαρμόζονται στο το NIC *VM1 NIC1* και το υποδίκτυο *Subnet1*. Κίνηση να αλλάξει επιτρέπεται σε το NSG που σχετίζονται με το περιβάλλον εργασίας δικτύου *VM1 NIC1*, ωστόσο TCP ping να αποτύχει θύρα 3389 του VM1.

Ενώ αυτό το παράδειγμα χρησιμοποιεί αλλάξει, ακολουθήστε τα παρακάτω βήματα μπορεί να χρησιμοποιηθεί για τον καθορισμό αποτυχίες σύνδεσης εισερχόμενων και εξερχόμενων πάνω από οποιαδήποτε θύρα.

## <a name="detailed-troubleshooting-steps"></a>Λεπτομερή βήματα αντιμετώπισης προβλημάτων
Ολοκληρώστε τα ακόλουθα βήματα για την αντιμετώπιση προβλημάτων NSGs για μια Εικονική:

1. Ξεκινήστε μια περίοδο λειτουργίας Azure PowerShell και να συνδεθείτε στη Azure. Εάν δεν είστε εξοικειωμένοι με τη χρήση του PowerShell Azure, διαβάστε το άρθρο [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .

2. Πληκτρολογήστε την παρακάτω εντολή για να επιστρέψει όλες NSG κανόνες που εφαρμόζονται στο NIC με το όνομα *VM1 NIC1* στην ομάδα πόρων *RG1*:

        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Εάν δεν γνωρίζετε το όνομα του NIC, πληκτρολογήστε την παρακάτω εντολή για να ανακτήσετε τα ονόματα των όλα NIC σε μια ομάδα πόρων: 
    
    >`Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`

    Το ακόλουθο κείμενο είναι ένα δείγμα των αποτελεσμάτων της αποτελεσματικές κανόνες επιστρέφεται για *VM1 NIC1* NIC:

        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
        
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]

    Λάβετε υπόψη τις ακόλουθες πληροφορίες στο αποτέλεσμα:

    - Υπάρχουν δύο ενότητες **NetworkSecurityGroup** : μία συσχετίζεται με ένα υποδίκτυο (*Subnet1*) και ένα είναι συσχετισμένη με μια NIC (*VM1 NIC1*). Σε αυτό το παράδειγμα, μια NSG έχουν εφαρμοστεί σε κάθε μία.
    - **Συσχέτιση** εμφανίζει τον πόρο (υποδίκτυο ή NIC) μιας δεδομένης NSG συσχετίζεται με. Εάν ο πόρος NSG έχει καταργηθεί η μετακινηθεί/συσχέτιση αμέσως πριν από την εκτέλεση αυτής της εντολής, ίσως χρειαστεί να περιμένετε μερικά δευτερόλεπτα για την αλλαγή ώστε να αντικατοπτρίζει στην έξοδο της εντολής. 
    - Τα ονόματα των κανόνων που είναι πριν από αυτό, *defaultSecurityRules*: δημιουργείται όταν μια NSG, διάφορες προεπιλεγμένους κανόνες ασφαλείας δημιουργούνται μέσα σε αυτό. Δεν είναι δυνατό να καταργηθούν προεπιλεγμένους κανόνες, αλλά μπορούν να παρακαμφθούν με υψηλότερη προτεραιότητα κανόνων.
     Διαβάστε το άρθρο [Επισκόπηση NSG](virtual-networks-nsg.md#default-rules) για να μάθετε περισσότερα σχετικά με τους κανόνες ασφαλείας προεπιλεγμένη NSG.
    - **ExpandedAddressPrefix** επεκτείνει τα προθέματα διεύθυνση για NSG προεπιλεγμένες ετικέτες. Ετικέτες αντιπροσωπεύουν πολλά προθέματα διεύθυνση. Επέκτασης από τις ετικέτες μπορεί να είναι χρήσιμες κατά την αντιμετώπιση προβλημάτων συνδεσιμότητας Εικονική προς/από προθέματα συγκεκριμένη διεύθυνση. Για παράδειγμα, με VNET διεισδύουν, ετικέτα VIRTUAL_NETWORK επεκτείνεται για να εμφανίσετε peered VNet προθέματα στο προηγούμενο αποτέλεσμα.

        >[AZURE.NOTE] Η εντολή μόνο εμφανίζει αποτελεσματικών κανόνες εάν μια NSG σχετίζεται με ένα υποδίκτυο, NIC, ή και τα δύο. Μια Εικονική μπορεί να έχει πολλές NIC με διαφορετικό NSGs εφαρμοστεί. Κατά την αντιμετώπιση προβλημάτων, εκτελέστε την εντολή για κάθε NIC.
        
3. Για να διευκολύνετε φιλτράρισμα μέσω μεγαλύτερο του αριθμού των κανόνων NSG, εισαγάγετε τις παρακάτω εντολές για την αντιμετώπιση προβλημάτων περαιτέρω: 

        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView

    Έχει εφαρμοστεί ένα φίλτρο για την κίνηση RDP (TCP θύρα 3389), στην προβολή γραμμών πλέγματος, όπως φαίνεται στην παρακάτω εικόνα:

    ![Λίστα κανόνων](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
    
4. Όπως μπορείτε να δείτε στην προβολή γραμμών πλέγματος, υπάρχουν και τα δύο επιτρέπουν και απόρριψη κανόνες για RDP. Το αποτέλεσμα από το βήμα 2 εμφανίζει ότι ο κανόνας *DenyRDP* βρίσκεται σε το NSG εφαρμόζονται στο υποδίκτυο. Για κανόνες εισερχόμενων, NSGs εφαρμοστεί στο υποδίκτυο υποβάλλονται σε επεξεργασία πρώτα. Εάν βρεθεί αντιστοίχιση, δεν γίνεται η NSG εφαρμόζονται στο περιβάλλον εργασίας του δικτύου. Σε αυτήν την περίπτωση, ο κανόνας *DenyRDP* από το υποδίκτυο αποκλείει RDP για την εικονική Μηχανή (**VM1**).

    >[AZURE.NOTE] Μια Εικονική μπορεί να έχει πολλές NIC συνημμένη. Κάθε ενδέχεται να είστε συνδεδεμένοι σε ένα διαφορετικό υποδίκτυο. Επειδή οι εντολές στα προηγούμενα βήματα που εκτελούνται σε σχέση με μια NIC, είναι σημαντικό για να βεβαιωθείτε ότι μπορείτε να καθορίσετε το NIC αντιμετωπίζετε την έλλειψη συνδεσιμότητας. Εάν δεν είστε βέβαιοι, μπορείτε να εκτελέσετε τις εντολές πάντα σε σχέση με κάθε NIC που συνδέονται με την εικονική Μηχανή.

5. Για να RDP σε VM1, αλλάξτε τον κανόνα *"Απόρριψη" RDP (3389)* για να *Επιτρέψετε RDP(3389)* στο **Subnet1 NSG** NSG. Επιβεβαιώστε ότι είναι ανοιχτό ανοίγοντας μιας σύνδεσης RDP για την εικονική Μηχανή ή χρησιμοποιώντας το εργαλείο PsPing αλλάξει. Μπορείτε να μάθετε περισσότερα σχετικά με το PsPing διαβάζοντας το [PsPing λήψη σελίδας](https://technet.microsoft.com/sysinternals/psping.aspx)

    Μπορείτε να ή κατάργηση κανόνων από ένα NSG, χρησιμοποιώντας τις πληροφορίες στο αποτέλεσμα από την ακόλουθη εντολή:

        Get-Help *-AzureRmNetworkSecurityRuleConfig
        

## <a name="considerations"></a>Ζητήματα

Κατά την αντιμετώπιση προβλημάτων συνδεσιμότητας, λάβετε υπόψη τα εξής σημεία:

- Προεπιλεγμένη NSG κανόνες θα αποκλεισμός εισερχόμενης πρόσβασης από το internet και μόνο επιτρέπουν VNet εισερχόμενη κυκλοφορία. Κανόνες πρέπει να προστεθεί ρητά για να επιτρέψετε την εισερχόμενη πρόσβαση από το Internet, όπως απαιτείται.
- Εάν δεν υπάρχουν κανόνες ασφαλείας NSG προκαλεί η συνδεσιμότητα του δικτύου μια Εικονική αποτυχία, το πρόβλημα μπορεί να προθεσμία για να:
    - Λογισμικό τείχος προστασίας που εκτελείται η Εικονική λειτουργικό σύστημα
    - Δρομολογεί έχει ρυθμιστεί για συσκευές εικονικού ή κίνηση εσωτερικής εγκατάστασης. Μπορεί να ανακατευθυνθεί κυκλοφορίας Internet στην εσωτερική εγκατάσταση μέσω υποχρεωτική διοχέτευση. Σύνδεση RDP/SSH από το Internet για να σας Εικονική ενδέχεται να μην λειτουργούν με αυτήν τη ρύθμιση, ανάλογα με τον τρόπο που το υλικό δικτύου εσωτερικής χειρίζεται αυτήν την κυκλοφορία. Διαβάστε το άρθρο [Αντιμετώπισης προβλημάτων διαδρομές](virtual-network-routes-troubleshoot-powershell.md) για να μάθετε πώς μπορείτε να διάγνωση προβλημάτων δρομολόγηση που μπορεί να παρεμπόδιση της ροής κυκλοφορίας και έξοδος από την εικονική Μηχανή. 
- Εάν που έχουν peered VNets, από προεπιλογή, η ετικέτα VIRTUAL_NETWORK θα επεκταθεί αυτόματα για να συμπεριλάβετε προθέματα για peered VNets. Μπορείτε να προβάλετε αυτά τα προθέματα στη λίστα **ExpandedAddressPrefix** , για αντιμετώπιση προβλημάτων που σχετίζονται με τη σύνδεση διεισδύουν VNet. 
- Κανόνες αποτελεσματικές ασφαλείας εμφανίζονται μόνο εάν υπάρχει μια NSG που σχετίζεται με την εικονική Μηχανή NIC και ή υποδικτύου. 
- Εάν υπάρχουν χωρίς NSGs που σχετίζεται με το NIC ή υποδικτύου και έχετε μια δημόσια διεύθυνση IP που έχουν εκχωρηθεί σε σας Εικονική, όλες οι θύρες θα είναι ανοιχτό για πρόσβαση εισερχόμενων και εξερχόμενων. Εάν η Εικονική έχει μια δημόσια διεύθυνση IP, εφαρμόζοντας NSGs στο NIC ή υποδικτύου συνιστάται.  
