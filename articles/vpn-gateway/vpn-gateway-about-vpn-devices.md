<properties 
   pageTitle="Σχετικά με τις συσκευές VPN για συνδέσεις τοποθεσίας σε τοποθεσία πύλης VPN για δίκτυα εικονικού Azure | Microsoft Azure"
   description="Σε αυτό το άρθρο περιγράφει συσκευών VPN και παράμετροι ασφαλείας IP για συνδέσεις πύλης VPN S2S και περιέχει συνδέσεις με οδηγίες ρύθμισης παραμέτρων και δείγματα."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="yushwang;cherylmc" />

# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>Σχετικά με τις συσκευές VPN για συνδέσεις τοποθεσίας σε τοποθεσία πύλης VPN

Μια συσκευή VPN είναι απαραίτητη για να ρυθμίσετε μια σύνδεση VPN-τοποθεσίας (S2S). Συνδέσεις τοποθεσίας σε τοποθεσία μπορούν να χρησιμοποιηθούν για να δημιουργήσετε μια λύση υβριδική ή οποιαδήποτε στιγμή θέλετε ασφαλή σύνδεση μεταξύ του εικονικού δικτύου και το δίκτυο εσωτερικής εγκατάστασης. Σε αυτό το άρθρο περιγράφει συμβατές συσκευές VPN και ρύθμιση παραμέτρων. 

>[AZURE.NOTE] Όταν ρυθμίζετε τις παραμέτρους μιας σύνδεσης τοποθεσίας σε τοποθεσία, μια διεύθυνση IPv4 IP δημόσιας απαιτείται για τη συσκευή σας VPN.                                                                                                                                                                               

Εάν η συσκευή σας δεν εμφανίζεται στον πίνακα [επικυρωθεί VPN συσκευές](#devicetable) , ανατρέξτε στην ενότητα [συσκευών επικυρωθεί μη VPN](#additionaldevices) αυτού του άρθρου. Είναι πιθανό ότι τη συσκευή σας μπορεί να εξακολουθούν να λειτουργούν με το Azure. Για υποστήριξη συσκευών VPN, επικοινωνήστε με τον κατασκευαστή της συσκευής.

**Στοιχεία για να λάβετε υπόψη κατά την προβολή τους πίνακες:**

- Παρουσιάστηκε μια αλλαγή ορολογία για τη δρομολόγηση στατικές και δυναμικές. Πιθανώς θα αντιμετωπίσετε και τα δύο όρους. Δεν υπάρχει καμία αλλαγή λειτουργίες, πρόκειται να αλλάξετε μόνο τα ονόματα.
    - Στατική δρομολόγηση = PolicyBased
    - Δυναμική δρομολόγηση = RouteBased
- Προδιαγραφές για πύλης VPN υψηλές επιδόσεις και την πύλη RouteBased VPN είναι ίδιες εκτός και αν αναφέρεται διαφορετικά. Για παράδειγμα, το επικυρωμένο συσκευές VPN που είναι συμβατό με τις πύλες RouteBased VPN είναι επίσης συμβατή με την πύλη VPN υψηλές επιδόσεις Azure. 


## <a name="devicetable"></a>Επικυρωμένο συσκευών VPN 

Θα σας έχουν επικύρωση ένα σύνολο τυπικές συσκευές VPN σε συνεργασία με συσκευή προμηθευτές. Όλες τις συσκευές στις οικογένειες συσκευή που περιέχονται στη λίστα που ακολουθεί, θα πρέπει να εργαστείτε με Azure VPN πύλες. Ανατρέξτε στο θέμα [Σχετικά με την πύλη VPN](vpn-gateway-about-vpngateways.md) για να επαληθεύσετε τον τύπο της πύλης που χρειάζεστε για να δημιουργήσετε για τη λύση που θέλετε να ρυθμίσετε τις παραμέτρους. 

Για να ρυθμίσετε τη συσκευή σας VPN, ανατρέξτε στις συνδέσεις που αντιστοιχούν σε οικογένεια κατάλληλη συσκευή. Για υποστήριξη συσκευών VPN, επικοινωνήστε με τον κατασκευαστή της συσκευής.



| **Προμηθευτή**                      | **Συσκευή οικογένεια**                                        | **Ελάχιστη έκδοση του λειτουργικού Συστήματος**                             | **PolicyBased**                                                                                                                                                                                                             | **RouteBased**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Συναφών Telesis                  | Εισ δρομολογητές VPN σειράς                                    | 2.9.2                                              | Έρχομαι σύντομα                                                                                                                                                                                                                                          | Μη συμβατή                                                                                                                                                                                               |
| Barracuda δίκτυα, Inc.        | Τείχος προστασίας NextGen barracuda F-σειράς             | PolicyBased: 5.4.3, RouteBased: 6.2.0  | [Οδηγίες ρύθμισης παραμέτρων](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Οδηγίες ρύθμισης παραμέτρων](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda δίκτυα, Inc.        |  Barracuda NextGen τείχος προστασίας-σειρά X                 | Barracuda 6.5 τείχους προστασίας | [Barracuda τείχος προστασίας](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Μη συμβατή                                                                                                                                                                                               |
| Μπροκάρ                         | VRouter Vyatta 5400                                      | Εικονικό δρομολογητή 6.6R3 GA                            | [Οδηγίες ρύθμισης παραμέτρων](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Μη συμβατή                                                                                                                                                                                               |
| Σημείο ελέγχου                     | Πύλη ασφαλείας                                         | R75.40, R75.40VS                                     | [Οδηγίες ρύθμισης παραμέτρων](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Οδηγίες ρύθμισης παραμέτρων](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Δείγματα Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Μη συμβατή                                                                                                                                                                                               |
| Cisco                           | ASR                                                      | IOS 15.1 (PolicyBased), IOS 15.2 (RouteBased)                | [Δείγματα Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Δείγματα Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15,0 (PolicyBased), IOS 15.1 (RouteBased *)               | [Δείγματα Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Δείγματα Cisco *](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | MPX NetScaler, SDX, VPX      |10.1 και παραπάνω                                           | [Ενοποίηση οδηγίες](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Μη συμβατή                                                                                                                                                                                               |
| Dell SonicWALL                  | Σειρά ΖΩ, σειρά ΕΑΑ, SuperMassive σειρά, E-κλάσης ΕΑΑ σειράς | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Οδηγίες - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Οδηγίες - SonicOS αριθμό 5,9 προς](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Οδηγίες - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Οδηγίες - SonicOS αριθμό 5,9 προς](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | Σειρά BIG-IP                                 |           12.0                                            | [Οδηγίες ρύθμισης παραμέτρων](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | [Οδηγίες ρύθμισης παραμέτρων](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling)                                                                                                                                                                                         |
| Fortinet                        | FortiGate                                                | FortiOS 5.2.7                                      | [Οδηγίες ρύθμισης παραμέτρων](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Οδηγίες ρύθμισης παραμέτρων](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Ιαπωνία πρωτοβουλία Internet (IIJ) | SEIL σειρά                                              | SEIL / X 4.60, 4.60 SEIL/B1, SEIL/x86 3.20            | [Οδηγίες ρύθμισης παραμέτρων](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Μη συμβατή                                                                                                                                                                                               |
| Juniper                         | SRX                                                      | JunOS 10.2 (PolicyBased), JunOS 11,4 (RouteBased)            | [Δείγματα juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Δείγματα juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Juniper                         | Σειρά J                                                 | JunOS 10.4r9 (PolicyBased), JunOS 11,4 (RouteBased)          | [Δείγματα juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Δείγματα juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Juniper                         | ISG                                                      | ScreenOS 6.3 (PolicyBased και RouteBased)                  | [Δείγματα juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Δείγματα juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Juniper                         | SSG                                                      | ScreenOS 6.2 (PolicyBased και RouteBased)                  | [Δείγματα juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Δείγματα juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Δρομολόγηση και απομακρυσμένη πρόσβαση υπηρεσίας                        | Windows Server 2012                                | Μη συμβατή                                                                                                                                                                                                                                       | [Δείγματα της Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Άνοιγμα AG συστήματα | Πύλη αποστολής ελέγχου ασφαλείας | Δ/Υ | [Οδηγός εγκατάστασης](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Οδηγός εγκατάστασης](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (Σύντομα διαθέσιμο)                                                                                                                                                                                                                                        | Μη συμβατή                                                                                                                                                                                               |
| Palo Alto δίκτυα              | Λειτουργικό σύστημα ΜΕΤΑΤΌΠΙΣΗ όλες τις συσκευές     | ΜΕΤΑΤΌΠΙΣΗ-OS 6.1.5 ή νεότερη έκδοση (PolicyBased), ΜΕΤΑΤΌΠΙΣΗ-OS 7.0.5 ή νεότερη έκδοση (RouteBased)       | [Οδηγίες ρύθμισης παραμέτρων]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Οδηγίες ρύθμισης παραμέτρων](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| WatchGuard                      | Όλων                                                      | V11.x Fireware XTM                                 | [Οδηγίες ρύθμισης παραμέτρων](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Μη συμβατή                                                                                                                                                                                               |

(*) Της σειράς 7200 ISR υποστηρίζει μόνο PolicyBased VPN.

## <a name="additionaldevices"></a>Επικύρωση μη συσκευών VPN

Εάν δεν βλέπετε τη συσκευή σας που παρατίθενται στον πίνακα συσκευές επικυρωθεί VPN, εξακολουθεί να μπορεί να λειτουργεί με μια σύνδεση τοποθεσίας σε τοποθεσία. Επιβεβαιώστε ότι η συσκευή σας VPN πληροί τις ελάχιστες απαιτήσεις που περιγράφονται στην ενότητα απαιτήσεις πύλης αυτού του άρθρου [Σχετικά με τις πύλες VPN](vpn-gateway-about-vpngateways.md#gateway-requirements) . Συσκευές που πληροί τις ελάχιστες απαιτήσεις επίσης πρέπει να λειτουργούν καλύτερα με VPN πυλών. Επικοινωνήστε με τον κατασκευαστή της συσκευής για πρόσθετες οδηγίες υποστήριξης και ρύθμισης παραμέτρων.


## <a name="editing-device-configuration-samples"></a>Επεξεργασία δείγματα ρύθμισης παραμέτρων συσκευής

Μετά τη λήψη του δείγματος ρύθμισης παραμέτρων του παρεχόμενη VPN τη συσκευή, θα χρειαστεί να αντικαταστήσετε ορισμένες από τις τιμές ώστε να αντικατοπτρίζει τις ρυθμίσεις για το περιβάλλον σας. 

**Για να επεξεργαστείτε ένα δείγμα:**

1. Ανοίξτε το δείγμα χρησιμοποιώντας το Σημειωματάριο. 
1. Αναζήτηση και αντικαταστήστε όλες τις συμβολοσειρές <*κειμένου*> με τις τιμές που σχετίζονται με το περιβάλλον σας. Φροντίστε να συμπεριλάβετε < και >. Όταν καθορίζεται ένα όνομα, το όνομα που επιλέγετε πρέπει να είναι μοναδικό. Εάν μια εντολή δεν λειτουργεί, συμβουλευτείτε την τεκμηρίωση του κατασκευαστή της συσκευής σας.

| **Δείγμα κειμένου**                | **Αλλαγή σε**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | Το επιλεγμένο όνομα για αυτό το αντικείμενο. Παράδειγμα: myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | Το επιλεγμένο όνομα για αυτό το αντικείμενο. Παράδειγμα: myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | Το επιλεγμένο όνομα για αυτό το αντικείμενο. Παράδειγμα: myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | Το επιλεγμένο όνομα για αυτό το αντικείμενο. Παράδειγμα: myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | Το επιλεγμένο όνομα για αυτό το αντικείμενο. Παράδειγμα: myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Καθορίστε την περιοχή. Παράδειγμα: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Καθορίστε τη μάσκα υποδικτύου. Παράδειγμα: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Καθορίστε περιοχή εσωτερικής εγκατάστασης. Παράδειγμα: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Καθορίστε τη μάσκα υποδικτύου εσωτερικής εγκατάστασης. Παράδειγμα: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Αυτές οι πληροφορίες ειδικά για το εικονικό δίκτυο και βρίσκεται στην πύλη διαχείρισης ως **διεύθυνση IP πύλης**. |
| &lt;SP_PresharedKey&gt;                | Αυτές οι πληροφορίες ειδικά για το εικονικό δίκτυο και βρίσκεται στην πύλη διαχείρισης ως Διαχείριση κλειδιού.          |



## <a name="ipsec-parameters"></a>Παράμετροι ασφαλείας IP

>[AZURE.NOTE] Παρόλο που οι τιμές που παρατίθενται στον παρακάτω πίνακα που υποστηρίζονται από την πύλη VPN Azure, προς το παρόν δεν υπάρχει τρόπος για να καθορίσετε ή επιλέξτε έναν συγκεκριμένο συνδυασμό από την πύλη VPN Azure. Πρέπει να καθορίσετε τους περιορισμούς από τη συσκευή VPN εσωτερικής εγκατάστασης. Μπορείτε, επίσης, πρέπει να κολάρο MSS στο 1350.

### <a name="ike-phase-1-setup"></a>Το πρόγραμμα εγκατάστασης IKE φάση 1

| **Ιδιότητα**                                       | **PolicyBased** | **RouteBased και τυπική ή υψηλή VPN επιδόσεων πύλης** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| Έκδοση IKE                                        | IKEv1                          | IKEv2                                                            |
| Ομάδα Diffie-Hellman                               | Ομάδα 2 (1024 bit)             | Ομάδα 2 (1024 bit)                                               |
| Μέθοδος ελέγχου ταυτότητας                              | Κλειδί κοινής χρήσης                 | Κλειδί κοινής χρήσης                                                   |
| Αλγόριθμοι κρυπτογράφησης                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Ο κατακερματισμός αλγόριθμου                                  | SHA1(SHA128)                   | SHA1(SHA128), SHA2(SHA256)                                                     |
| Φάση 1 ασφαλείας συσχετισμός διάρκεια ζωής (χρόνος) | 28,800 δευτερόλεπτα                 | 10,800 δευτερόλεπτα                                                   |


### <a name="ike-phase-2-setup"></a>Ρύθμιση IKE φάση 2

| **Ιδιότητα**                                                             | **PolicyBased**                 | **RouteBased και τυπική ή υψηλή VPN επιδόσεων πύλης**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| Έκδοση IKE                                                              | IKEv1                                          | IKEv2                                                              |
| Ο κατακερματισμός αλγόριθμου                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Φάση 2 ασφαλείας συσχετισμός διάρκεια ζωής (χρόνος)                        | 3.600 δευτερόλεπτα                                  | 3.600 δευτερόλεπτα                                                                  |
| Φάση 2 ασφαλείας συσχετισμός διάρκεια ζωής (μετάδοσης)                  | 102,400,000 KB                                 | -                                                                  |
| Κρυπτογράφηση σα ασφαλείας IP & προσφέρει ελέγχου ταυτότητας (με τη σειρά των προτιμήσεων) | 1. ESP-AES256 2. ESP-AES128 3. ESP 3DES 4. Δ/Υ | Ανατρέξτε στο θέμα *προσφέρει πύλης RouteBased ασφαλείας IP ασφαλείας συσχετισμός* (παρακάτω) |
| Ιδανική εμπιστευτικότητας (PFS)                                            | Όχι                                             | Δεν υπάρχει (*)|
| Νεκρά εντοπισμού ομότιμης σύνδεσης                                                      | Δεν υποστηρίζεται                                  | Υποστηρίζονται                                                          |

(*) Azure πύλης ως ανταπόκριση IKE μπορεί να αποδεχτεί PFS DH ομάδα 1, 2, 5, 14, 24.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>Η πύλη RouteBased ασφαλείας IP ασφαλείας συσχετισμός προσφέρει

Ο παρακάτω πίνακας παραθέτει κρυπτογράφησης σα ασφαλείας IP και παρέχει έλεγχο ταυτότητας. Προσφορές παρατίθενται τη σειρά των προτιμήσεων που την προσφορά είναι που παρουσιάζονται ή αποδεκτές.

| **Κρυπτογράφηση σα ασφαλείας IP και προσφορές ελέγχου ταυτότητας** | **Azure πύλης ως δημιουργού**                               | **Azure πύλης ως ανταπόκριση**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | SHA1 AH με ESP AES_128 με τιμή null HMAC                      |
| 5                                                 | SHA1 AH με ESP AES_256 με τιμή null HMAC                      | SHA1 AH με ESP 3_DES με τιμή null HMAC                        |
| 6                                                 | SHA1 AH με ESP AES_128 με τιμή null HMAC                      | AH MD5 με ESP 3_DES με τιμή null HMAC, δεν υπάρχει η διάρκεια ζωής προτάθηκε |
| 7                                                 | SHA1 AH με ESP 3_DES με τιμή null HMAC                        | SHA1 AH με SHA1 3_DES ESP, δεν υπάρχει διάρκεια ζωής                    |
| 8                                                 | AH MD5 με ESP 3_DES με τιμή null HMAC, δεν υπάρχει η διάρκεια ζωής προτάθηκε | AH MD5 με MD5 3_DES ESP, δεν υπάρχει διάρκεια ζωής                     |
| 9                                                 | SHA1 AH με SHA1 3_DES ESP, δεν υπάρχει διάρκεια ζωής                    | ESP DES MD5                                                  |
| 10                                                | AH MD5 με MD5 3_DES ESP, δεν υπάρχει διάρκεια ζωής                     | SHA1 DES ESP, δεν υπάρχει διάρκεια ζωής                                   |
| 11                                                | ESP DES MD5                                                  | AH SHA1 με τιμή null HMAC ESP DES, δεν υπάρχει διάρκεια ζωής προτάθηκε        |
| 12                                                | SHA1 DES ESP, δεν υπάρχει διάρκεια ζωής                                   | AH MD5 με τιμή null HMAC ESP DES, δεν υπάρχει διάρκεια ζωής προτάθηκε        |
| 13                                                | AH SHA1 με τιμή null HMAC ESP DES, δεν υπάρχει διάρκεια ζωής προτάθηκε        | SHA1 AH με SHA1 DES ESP, δεν υπάρχει διάρκεια ζωής                      |
| 14                                                | AH MD5 με τιμή null HMAC ESP DES, δεν υπάρχει διάρκεια ζωής προτάθηκε        | AH MD5 με MD5 DES ESP, δεν υπάρχει διάρκεια ζωής                       |
| 15                                                | SHA1 AH με SHA1 DES ESP, δεν υπάρχει διάρκεια ζωής                      | SHA ESP, δεν υπάρχει διάρκεια ζωής                                        |
| 16                                                | AH MD5 με MD5 DES ESP, δεν υπάρχει διάρκεια ζωής                       | ESP MD5, δεν υπάρχει διάρκεια ζωής                                        |
| 17                                                | -                                                            | AH SHA, δεν υπάρχει διάρκεια ζωής                                         |
| 18                                                | -                                                            | AH MD5, δεν υπάρχει διάρκεια ζωής                                        |


- Μπορείτε να καθορίσετε ασφαλείας IP ESP NULL κρυπτογράφησης με RouteBased και υψηλές επιδόσεις VPN πυλών. Null που βασίζονται σε κρυπτογράφηση δεν παρέχει προστασία με τα δεδομένα κατά τη μεταφορά και πρέπει να χρησιμοποιείται μόνο όταν μέγιστο μετάδοσης και ελάχιστη λανθάνων χρόνος είναι απαραίτητη.  Προγράμματα-πελάτες μπορεί να επιλέξετε να χρησιμοποιήστε αυτήν την επιλογή σε σενάρια επικοινωνίας VNet-VNet ή όταν κρυπτογράφησης είναι που έχουν εφαρμοστεί σε κάποιο άλλο σημείο στη λύση.

- Για συνδεσιμότητα σταυρό εσωτερικής εγκατάστασης μέσω του Internet, χρησιμοποιήστε τις προεπιλεγμένες ρυθμίσεις πύλης Azure VPN με κρυπτογράφηση και κλειδώματος αλγόριθμους που αναφέρονται στους πίνακες παραπάνω για να βεβαιωθείτε ότι ασφαλείας σας κρίσιμες επικοινωνίας.





