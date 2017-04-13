<properties 
   pageTitle="Αντιμετώπιση προβλημάτων με τις ομάδες ασφαλείας δικτύου - πύλη | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αντιμετωπίσετε ομάδες ασφαλείας δικτύου στο μοντέλο ανάπτυξης διαχείρισης πόρων Azure με την πύλη Azure."
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

# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Αντιμετώπιση προβλημάτων με τις ομάδες ασφαλείας δικτύου με την πύλη Azure

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

### <a name="view-effective-security-rules-for-a-virtual-machine"></a>Προβολή των κανόνων ισχύος ασφαλείας για μια εικονική μηχανή

Ολοκληρώστε τα ακόλουθα βήματα για την αντιμετώπιση προβλημάτων NSGs για μια Εικονική:

Μπορείτε να προβάλετε πλήρη λίστα των κανόνων ισχύος ασφαλείας στο NIC, από την εικονική Μηχανή ίδια. Μπορείτε επίσης να προσθέσετε, τροποποίηση και διαγραφή κανόνων NSG NIC και υποδικτύου από το blade αποτελεσματικές κανόνες, εάν έχετε δικαιώματα για την εκτέλεση αυτών των λειτουργιών.

1. Συνδεθείτε στην πύλη του Azure στη https://portal.azure.com.
2. Κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες**και, στη συνέχεια, κάντε κλικ στην επιλογή **εικονικές μηχανές** στη λίστα που εμφανίζεται.
3. Επιλέξτε μια Εικονική για την αντιμετώπιση προβλημάτων από τη λίστα που εμφανίζεται και θα εμφανιστεί μια Εικονική blade με τις επιλογές.
4. Κάντε κλικ στην επιλογή **διάγνωση & επίλυση προβλημάτων** και, στη συνέχεια, επιλέξτε ένα κοινό θέμα. Για αυτό το παράδειγμα, **δεν μπορώ να συνδεθώ μου Εικονική Windows** είναι επιλεγμένο. 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)

5. Τα βήματα εμφανίζονται κάτω από το πρόβλημα, όπως φαίνεται στην παρακάτω εικόνα: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)

    Κάντε κλικ στην επιλογή *κανόνες ομάδα αποτελεσματικές ασφαλείας* στη λίστα των προτεινόμενα βήματα.

6. Εμφανίζεται το blade **γρήγορα κανόνες αποτελεσματικές ασφαλείας** , όπως φαίνεται στην παρακάτω εικόνα:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)

    Παρατηρήστε τις ακόλουθες ενότητες της εικόνας:

    - **Εύρος:** Ορίστε σε *VM1*, η Εικονική που επιλέξατε στο βήμα 3.
    - **Διασύνδεση δικτύου:** *VM1 NIC1* είναι επιλεγμένο. Μια Εικονική μπορεί να έχει πολλές διασυνδέσεις δικτύου (NIC). Κάθε NIC μπορεί να έχει μοναδικό αποτελεσματικές ασφαλείας κανόνων. Κατά την αντιμετώπιση προβλημάτων, ίσως χρειαστεί να προβάλετε τους κανόνες ισχύος ασφαλείας για κάθε NIC.
    - **Που σχετίζονται NSGs:** NSGs μπορούν να εφαρμοστούν σε NIC και το υποδίκτυο NIC είναι συνδεδεμένο σε. Στην εικόνα, ένα NSG έχει συσχετιστεί με το NIC και το υποδίκτυο είναι συνδεδεμένος. Μπορείτε να κάνετε κλικ στα ονόματα NSG για την άμεση τροποποίηση κανόνων στο το NSGs.
    - **Καρτέλα VM1 nsg:** Η λίστα των κανόνες που εμφανίζονται στην εικόνα είναι για το NSG εφαρμοστεί NIC. Διάφορες προεπιλεγμένους κανόνες που δημιουργούνται από Azure κάθε φορά που δημιουργείται μια NSG. Δεν μπορείτε να καταργήσετε τους προεπιλεγμένους κανόνες, αλλά μπορείτε να τα αντικαταστήσετε με κανόνες για υψηλότερη προτεραιότητα. Για να μάθετε περισσότερα σχετικά με τους κανόνες προεπιλεγμένης, διαβάστε το άρθρο [Επισκόπηση NSG](virtual-networks-nsg.md#default-rules) .
    - **Στήλη ΠΡΟΟΡΙΣΜΟΎ:** Ορισμένα από τους κανόνες έχουν κείμενο στη στήλη, ενώ άλλοι χρήστες έχουν προθέματα διεύθυνση. Το κείμενο είναι το όνομα του προεπιλεγμένων tag εφαρμοστεί ο κανόνας ασφαλείας όταν δημιουργήθηκε. Οι ετικέτες είναι αναγνωριστικά που παρέχονται από σύστημα που αντιπροσωπεύουν πολλά προθέματα. Επιλέγοντας έναν κανόνα με μια ετικέτα, όπως *AllowInternetOutBound*, παραθέτει τα προθέματα στο το blade **προθέματα διεύθυνση** .
    - **Λήψη:** Η λίστα κανόνων μπορεί να είναι πολύ. Μπορείτε να κάνετε λήψη σε ένα αρχείο .csv από τους κανόνες για την ανάλυση για εργασία χωρίς σύνδεση, κάνοντας κλικ στην επιλογή **λήψη** και την αποθήκευση του αρχείου.
    - **AllowRDP** Κανόνας εισερχομένων: Αυτός ο κανόνας επιτρέπει RDP συνδέσεις με την εικονική Μηχανή.
7. Κάντε κλικ στην καρτέλα **Subnet1 NSG** για να προβάλετε την αποτελεσματική κανόνες από την NSG εφαρμόζονται στο υποδίκτυο, όπως φαίνεται στην παρακάτω εικόνα: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)

    Παρατηρήστε ότι ο κανόνας **Εισερχόμενα** *denyRDP* . Κανόνες εισερχομένων στο δευτερεύον αξιολογούνται πριν κανόνες που εφαρμόζονται στο περιβάλλον εργασίας του δικτύου. Εφόσον έχει εφαρμοστεί ο κανόνας απόρριψη στο δευτερεύον, η αίτηση για να συνδεθείτε με TCP 3389 αποτυγχάνει, επειδή ο κανόνας αποδοχή στο NIC αξιολογείται ποτέ. 

    Ο κανόνας *denyRDP* είναι το λόγο για τον γιατί η σύνδεση RDP αποτυγχάνει. Κατάργηση του πρέπει να επιλύσετε το πρόβλημα.

    >[AZURE.NOTE]Εάν η Εικονική που σχετίζεται με το NIC δεν είναι σε κατάσταση λειτουργίας ή NSGs δεν έχουν εφαρμοστεί στο NIC ή υποδίκτυο, εμφανίζονται κανένας κανόνας.

8. Για να επεξεργαστείτε τους κανόνες NSG, κάντε κλικ στην επιλογή *Subnet1 NSG* στην ενότητα **Που σχετίζονται NSGs** .
   Έτσι ανοίγει το blade **Subnet1 NSG** . Απευθείας, μπορείτε να επεξεργαστείτε τους κανόνες, κάνοντας κλικ σε **κανόνες εισερχομένων ασφαλείας**.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)

9. Μετά την κατάργηση του κανόνα εισερχομένων *denyRDP* από το **Subnet1 NSG** και την προσθήκη ενός κανόνα *allowRDP* , στη λίστα κανόνων αποτελεσματικές έχει την παρακάτω στην παρακάτω εικόνα:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)

    Επιβεβαιώστε ότι είναι ανοιχτό ανοίγοντας μιας σύνδεσης RDP για την εικονική Μηχανή ή χρησιμοποιώντας το εργαλείο PsPing αλλάξει. Μπορείτε να μάθετε περισσότερα σχετικά με το PsPing διαβάζοντας το [PsPing σελίδα λήψης](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="view-effective-security-rules-for-a-network-interface"></a>Προβολή των κανόνων ισχύος ασφαλείας για μια διασύνδεση δικτύου

Εάν επηρεάζεται σας Εικονική ροής κυκλοφορίας για ένα συγκεκριμένο NIC, μπορείτε να προβάλετε μια πλήρη λίστα των αποτελεσματικές κανόνων για NIC από το περιβάλλον διασυνδέσεων δικτύου, ολοκληρώνοντας τα παρακάτω βήματα:

1. Συνδεθείτε στην πύλη του Azure στη https://portal.azure.com.
2. Κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες**και, στη συνέχεια, κάντε κλικ στην επιλογή **διασυνδέσεις δικτύου** στη λίστα που εμφανίζεται.
3. Επιλέξτε ένα περιβάλλον εργασίας δικτύου. Στην παρακάτω εικόνα, με το όνομα *VM1 NIC1* NIC είναι επιλεγμένο.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)

    Παρατηρήστε ότι το **εύρος** έχει οριστεί στο περιβάλλον εργασίας του δικτύου που είναι επιλεγμένο. Για να μάθετε περισσότερα σχετικά με τις πρόσθετες πληροφορίες που παρουσιάζονται, διαβάστε βήμα 6 της ενότητας **NSGs αντιμετώπιση προβλημάτων για μια Εικονική** αυτού του άρθρου.

    >[AZURE.NOTE] Εάν μια NSG καταργείται από ένα περιβάλλον εργασίας δικτύου, το υποδίκτυο NSG είναι ακόμα αποτελεσματική στη δεδομένη σύνδεση NIC. Σε αυτήν την περίπτωση, το αποτέλεσμα θα Εμφάνιση μόνο των κανόνων από το υποδίκτυο NSG. Οι κανόνες εμφανίζονται μόνο εάν το NIC είναι συνδεδεμένος με μια Εικονική.

4. Μπορείτε να επεξεργαστείτε απευθείας κανόνες για NSGs που σχετίζεται με ένα NIC και ένα υποδίκτυο. Για να μάθετε πώς γίνεται αυτό, διαβάστε βήμα 8 της ενότητας **Προβολή κανόνων ισχύος ασφαλείας για μια εικονική μηχανή** αυτού του άρθρου.

## <a name="view-effective-security-rules-for-a-network-security-group-nsg"></a>Προβολή των κανόνων ισχύος ασφαλείας για μια ομάδα ασφαλείας δικτύου (NSG)

Όταν τροποποιείτε NSG κανόνες, που μπορεί να θέλετε να εξετάσετε την επίδραση των κανόνων προστεθείτε σε μια συγκεκριμένη Εικονική. Μπορείτε να προβάλετε μια πλήρη λίστα των κανόνων ισχύος ασφαλείας για όλα τα NIC που έχει εφαρμοστεί ένα δεδομένο NSG, χωρίς να χρειάζεται να κάνετε εναλλαγή περιβάλλον από τη δεδομένη blade NSG. Για την αντιμετώπιση προβλημάτων αποτελεσματικές κανόνες μέσα σε μια NSG, ολοκληρώστε τα ακόλουθα βήματα:

1. Συνδεθείτε στην πύλη του Azure στη https://portal.azure.com.
2. Κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες**και, στη συνέχεια, κάντε κλικ στην επιλογή **ομάδες ασφαλείας δικτύου** στη λίστα που εμφανίζεται.
3. Επιλέξτε μια NSG. Στην παρακάτω εικόνα, έχει επιλεγεί ένα NSG με το όνομα VM1 nsg.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)

    Παρατηρήστε τις παρακάτω ενότητες στην προηγούμενη εικόνα:

    - **Εύρος:** Ορίστε το NSG επιλεγμένο.
    - **Εικονική μηχανή:** Όταν μια NSG εφαρμόζεται με ένα δευτερεύον, εφαρμόζεται σε όλες τις διασυνδέσεις δικτύου που έχουν επισυναφθεί σε όλα ΣΠΣ συνδεδεμένοι στο υποδίκτυο. Αυτή η λίστα εμφανίζει όλα ΣΠΣ εφαρμόζεται αυτή NSG. Μπορείτε να επιλέξετε οποιοδήποτε Εικονική από τη λίστα.

    >[AZURE.NOTE] Εάν μια NSG εφαρμόζεται μόνο μια κενή υποδίκτυο, ΣΠΣ δεν θα εμφανίζεται. Εάν έχει εφαρμοστεί ένα NSG NIC που δεν έχει συσχετιστεί με μια Εικονική, αυτά τα NIC θα δεν εμφανίζονται επίσης. 
    - **Δικτύου του περιβάλλοντος εργασίας:** Μια Εικονική μπορεί να έχει πολλές διασυνδέσεις δικτύου. Μπορείτε να επιλέξετε ένα περιβάλλον εργασίας δικτύου που συνδέονται με το επιλεγμένο Εικονική.
    - **AssociatedNSGs:** Οποιαδήποτε στιγμή, NIC μπορεί να έχει έως και δύο αποτελεσματικές NSGs, εφαρμόζονται σε NIC και το άλλο στο υποδίκτυο. Παρόλο που το πεδίο εφαρμογής είναι επιλεγμένο ως VM1-nsg, εάν το NIC έχει μια αποτελεσματική υποδικτύου NSG, το αποτέλεσμα θα εμφανιστούν και οι δύο NSGs.
4. Μπορείτε να επεξεργαστείτε απευθείας κανόνες για NSGs που σχετίζεται με ένα NIC ή υποδικτύου. Για να μάθετε πώς γίνεται αυτό, διαβάστε βήμα 8 της ενότητας **Προβολή κανόνων ισχύος ασφαλείας για μια εικονική μηχανή** αυτού του άρθρου.

Για να μάθετε περισσότερα σχετικά με τις πρόσθετες πληροφορίες που παρουσιάζονται, διαβάστε βήμα 6 της ενότητας **Προβολή κανόνων ισχύος ασφαλείας για μια εικονική μηχανή** αυτού του άρθρου.

>[AZURE.NOTE] Αν και ένα υποδίκτυο και NIC μπορεί να έχετε μόνο μία εφαρμοστεί NSG, μια NSG μπορεί να είναι συσχετισμένη με πολλά NIC και πολλά δευτερεύοντα δίκτυα.

## <a name="considerations"></a>Ζητήματα

Κατά την αντιμετώπιση προβλημάτων συνδεσιμότητας, λάβετε υπόψη τα εξής σημεία:

- Προεπιλεγμένη NSG κανόνες θα αποκλεισμός εισερχόμενης πρόσβασης από το internet και μόνο επιτρέπουν VNet εισερχόμενη κυκλοφορία. Κανόνες πρέπει να προστεθεί ρητά για να επιτρέψετε την εισερχόμενη πρόσβαση από το Internet, όπως απαιτείται.
- Εάν δεν υπάρχουν κανόνες ασφαλείας NSG προκαλεί η συνδεσιμότητα του δικτύου μια Εικονική αποτυχία, το πρόβλημα μπορεί να προθεσμία για να:
    - Λογισμικό τείχος προστασίας που εκτελείται η Εικονική λειτουργικό σύστημα
    - Δρομολογεί έχει ρυθμιστεί για εικονικές συσκευές ή κίνηση εσωτερικής εγκατάστασης. Μπορεί να ανακατευθυνθεί κυκλοφορίας Internet στην εσωτερική εγκατάσταση μέσω υποχρεωτική διοχέτευση. Σύνδεση RDP/SSH από το Internet για να σας Εικονική ενδέχεται να μην λειτουργούν με αυτήν τη ρύθμιση, ανάλογα με τον τρόπο που το υλικό δικτύου εσωτερικής χειρίζεται αυτήν την κυκλοφορία. Διαβάστε το άρθρο [Αντιμετώπισης προβλημάτων διαδρομές](virtual-network-routes-troubleshoot-powershell.md) για να μάθετε πώς μπορείτε να διάγνωση προβλημάτων δρομολόγηση που μπορεί να παρεμπόδιση της ροής κυκλοφορίας και έξοδος από την εικονική Μηχανή. 
- Εάν που έχουν peered VNets, από προεπιλογή, η ετικέτα VIRTUAL_NETWORK θα επεκταθεί αυτόματα για να συμπεριλάβετε προθέματα peered VNets. Μπορείτε να προβάλετε αυτά τα προθέματα στη λίστα **ExpandedAddressPrefix** , για αντιμετώπιση προβλημάτων που σχετίζονται με τη σύνδεση διεισδύουν VNet. 
- Κανόνες αποτελεσματικές ασφαλείας εμφανίζονται μόνο εάν υπάρχει μια NSG που σχετίζεται με την εικονική Μηχανή NIC και ή υποδικτύου. 
- Εάν υπάρχουν χωρίς NSGs που σχετίζεται με το NIC ή υποδικτύου και έχετε μια δημόσια διεύθυνση IP που έχουν εκχωρηθεί σε σας Εικονική, όλες οι θύρες θα είναι ανοιχτό για πρόσβαση εισερχόμενων και εξερχόμενων. Εάν η Εικονική έχει μια δημόσια διεύθυνση IP, εφαρμόζοντας NSGs στο NIC ή υποδικτύου συνιστάται.