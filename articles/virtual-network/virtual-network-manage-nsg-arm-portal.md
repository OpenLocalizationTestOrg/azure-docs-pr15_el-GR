<properties 
   pageTitle="Διαχείριση NSGs με την πύλη προεπισκόπηση στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς να διαχειρίζεστε exising NSGs με την πύλη προεπισκόπηση στη Διαχείριση πόρων"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-the-preview-portal"></a>Διαχείριση NSGs με την πύλη preview

> [AZURE.SELECTOR]
- [Πύλη](virtual-network-manage-nsg-arm-portal.md)
- [PowerShell](virtual-network-manage-nsg-arm-ps.md)
- [Azure CLI](virtual-network-manage-nsg-arm-cli.md)

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Ανάκτηση πληροφοριών

Μπορείτε να προβάλετε τις υπάρχουσες NSGs, να ανακτήσετε κανόνες για μια υπάρχουσα NSG και να μάθετε ποια πόρους μια NSG είναι συσχετισμένη με.

### <a name="view-existing-nsgs"></a>NSGs υπάρχουσα προβολή
Για να προβάλετε όλες τις υπάρχουσες NSGs μια συνδρομή, ακολουθήστε τα παρακάτω βήματα.

1. Από ένα πρόγραμμα περιήγησης, μεταβείτε στο http://portal.azure.com και, εάν είναι απαραίτητο, πραγματοποιήστε είσοδο με το λογαριασμό της Azure.
2. Κάντε κλικ στην επιλογή **Αναζήτηση >** > **ομάδες ασφαλείας δικτύου**.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Ελέγξτε τη λίστα των NSGs στο το blade **ομάδες ασφαλείας δικτύου** .

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

Για να προβάλετε τη λίστα των NSGs στην ομάδα πόρων **RG NSG** , ακολουθήστε τα παρακάτω βήματα. 

1. Κάντε κλικ στην επιλογή **ομάδες πόρων >** > **RG NSG** > **...**.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. Στη λίστα των πόρων, αναζητήστε τα στοιχεία που εμφανίζει το εικονίδιο NSG, όπως φαίνεται στην το παρακάτω **πόρους** blade.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)
         
### <a name="list-all-rules-for-an-nsg"></a>Λίστα όλοι οι κανόνες για μια NSG

Για να προβάλετε τους κανόνες της μια NSG ονομάζεται **Προσκήνιο NSG**, ακολουθήστε τα παρακάτω βήματα. 

1. Από το blade **ομάδες ασφαλείας δικτύου** ή το blade **πόρους** που εμφανίζεται παραπάνω, κάντε κλικ στην επιλογή **NSG FrontEnd**.
2. Στην καρτέλα " **Ρυθμίσεις** ", κάντε κλικ στην επιλογή **κανόνες εισερχομένων ασφαλείας**.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. Εμφανίζεται το blade **ασφαλείας κανόνες εισερχομένων** , όπως φαίνεται παρακάτω.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. Στην καρτέλα " **Ρυθμίσεις** ", κάντε κλικ στην επιλογή **κανόνες εξερχομένων ασφαλείας** για να δείτε τους κανόνες εξερχομένων.

>[AZURE.NOTE] Για να προβάλετε προεπιλεγμένους κανόνες, κάντε κλικ στο εικονίδιο **προεπιλεγμένους κανόνες** στο επάνω μέρος του blade που εμφανίζει τους κανόνες.

### <a name="view-nsgs-associations"></a>Προβολή NSGs συσχετίσεων

Για να δείτε τι είναι το NSG **NSG FrontEnd** πόρους συσχετίσετε με, ακολουθήστε τα παρακάτω βήματα.

1. Από το blade **ομάδες ασφαλείας δικτύου** ή το blade **πόρους** που εμφανίζεται παραπάνω, κάντε κλικ στην επιλογή **NSG FrontEnd**.
2. Στην καρτέλα " **Ρυθμίσεις** ", κάντε κλικ στην επιλογή για να δείτε ποια δευτερεύοντα δίκτυα είναι συσχετισμένη με το NSG **δευτερεύοντα δίκτυα** .

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. Στην καρτέλα " **Ρυθμίσεις** ", κάντε κλικ στην επιλογή **διασυνδέσεις δικτύου** για να δείτε τι NIC είναι συσχετισμένη με το NSG.

## <a name="manage-rules"></a>Διαχείριση κανόνων

Μπορείτε να προσθέσετε κανόνες για μια υπάρχουσα NSG, να επεξεργαστείτε υπάρχοντα τους κανόνες και να καταργήσετε κανόνες.

### <a name="add-a-rule"></a>Προσθήκη κανόνα

Για να προσθέσετε έναν κανόνα που υπάρχει δυνατότητα **εισερχόμενη** κυκλοφορία θύρα **443** από οποιονδήποτε υπολογιστή για να το **NSG FrontEnd** NSG, ακολουθήστε τα παρακάτω βήματα.

1. Από το blade **ομάδες ασφαλείας δικτύου** ή το blade **πόρους** που εμφανίζεται παραπάνω, κάντε κλικ στην επιλογή **NSG FrontEnd**.
2. Στην καρτέλα " **Ρυθμίσεις** ", κάντε κλικ στην επιλογή **κανόνες εισερχομένων ασφαλείας**.
3. Στο blade **ασφαλείας κανόνες εισερχομένων** , κάντε κλικ στην επιλογή **Προσθήκη**. Στη συνέχεια, στο blade την **Προσθήκη κανόνα εισερχομένων ασφαλείας** , συμπληρώσετε τις τιμές, όπως φαίνεται παρακάτω και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

4. Μετά από μερικά δευτερόλεπτα, σημειώστε τον νέο κανόνα στο το blade **κανόνες εισερχομένων ασφαλείας** .

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Αλλαγή ενός κανόνα

Για να αλλάξετε τον κανόνα που δημιουργήσατε παραπάνω για να επιτρέψετε την εισερχόμενη κυκλοφορία από το **Internet** μόνο, ακολουθήστε τα παρακάτω βήματα.

1. Από το blade **ομάδες ασφαλείας δικτύου** ή το blade **πόρους** που εμφανίζεται παραπάνω, κάντε κλικ στην επιλογή **NSG FrontEnd**.
2. Στην καρτέλα " **Ρυθμίσεις** ", επιλέξτε τον κανόνα που δημιουργήσατε παραπάνω.
3. Στο το blade **επιτρέπουν https** , αλλάξτε την ιδιότητα **προέλευση** , όπως φαίνεται παρακάτω και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Διαγραφή κανόνα

Για να διαγράψετε τον κανόνα που δημιουργήσατε παραπάνω, ακολουθήστε τα παρακάτω βήματα.

1. Από το blade **ομάδες ασφαλείας δικτύου** ή το blade **πόρους** που εμφανίζεται παραπάνω, κάντε κλικ στην επιλογή **NSG FrontEnd**.
2. Στην καρτέλα " **Ρυθμίσεις** ", επιλέξτε τον κανόνα που δημιουργήσατε παραπάνω.
3. Στο το blade **επιτρέπουν https** , κάντε κλικ στην επιλογή **Διαγραφή**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι**.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Διαχείριση συσχετίσεων

Μπορείτε να συσχετίσετε ένα NSG σε δευτερεύοντα δίκτυα και NIC. Μπορείτε επίσης να διαχωρίσετε μια NSG από οποιονδήποτε πόρο που συσχετίζεται με.

### <a name="associate-an-nsg-to-a-nic"></a>Συσχετισμός ενός NSG στο NIC

Για να συσχετίσετε το **NSG FrontEnd** NSG να **TestNICWeb1** NIC, ακολουθήστε τα παρακάτω βήματα.

1. Από το blade **ομάδες ασφαλείας δικτύου** ή το blade **πόρους** που εμφανίζεται παραπάνω, κάντε κλικ στην επιλογή **NSG FrontEnd**.
2. Στην καρτέλα " **Ρυθμίσεις** ", κάντε κλικ στην επιλογή **διασυνδέσεις δικτύου** > **Συσχέτιση** > **TestNICWeb1**.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Καταργούν ένα NSG από NIC

Για να διαχωρίσετε το **NSG FrontEnd** NSG από το **TestNICWeb1** NIC, ακολουθήστε τα παρακάτω βήματα.

1. Από την πύλη Azure, κάντε κλικ στην επιλογή **ομάδες πόρων >** > **RG NSG** > **...**  >  **TestNICWeb1**.
2. Στο το blade **TestNICWeb1** , κάντε κλικ στην επιλογή **Αλλαγή ασφαλείας...**  > **None**.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

>[AZURE.NOTE] Μπορείτε επίσης να χρησιμοποιήσετε αυτό blade να συσχετίσετε το NIC σε οποιαδήποτε υπάρχουσα NSG.

### <a name="dissociate-an-nsg-from-a-subnet"></a>Καταργούν ένα NSG από ένα δευτερεύον δίκτυο

Για να διαχωρίσετε το **NSG FrontEnd** NSG από το υποδίκτυο **FrontEnd** , ακολουθήστε τα παρακάτω βήματα.

1. Από την πύλη Azure, κάντε κλικ στην επιλογή **ομάδες πόρων >** > **RG NSG** > **...**  >  **TestVNet**.
2. Στο το blade **Ρυθμίσεις** , κάντε κλικ στην επιλογή **δευτερεύοντα δίκτυα** > **FrontEnd** > **ομάδα ασφαλείας δικτύου** > **κανένα**.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. Στο το blade **FrontEnd** , κάντε κλικ στην επιλογή **Αποθήκευση**.

![Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Συσχετισμός ενός NSG με ένα δευτερεύον

Για να συσχετίσετε το **NSG FrontEnd** NSG στο υποδίκτυο **FronEnd** ξανά, ακολουθήστε τα παρακάτω βήματα.

1. Από την πύλη Azure, κάντε κλικ στην επιλογή **ομάδες πόρων >** > **RG NSG** > **...**  >  **TestVNet**.
2. Στο το blade **Ρυθμίσεις** , κάντε κλικ στην επιλογή **δευτερεύοντα δίκτυα** > **FrontEnd** > **ομάδα ασφαλείας δικτύου** > **NSG FrontEnd**.
3. Στο το blade **FrontEnd** , κάντε κλικ στην επιλογή **Αποθήκευση**.

>[AZURE.NOTE] Μπορείτε επίσης να συσχετίσετε ένα NSG με ένα δευτερεύον από blade **Ρυθμίσεις** του thh NSG.

## <a name="delete-an-nsg"></a>Διαγραφή ενός NSG

Μπορείτε να διαγράψετε μια NSG μόνο εάν δεν έχει συνδεθεί σε κάθε πόρο. Για να διαγράψετε μια NSG, ακολουθήστε τα παρακάτω βήματα.

1. Από την πύλη Azure, κάντε κλικ στην επιλογή **ομάδες πόρων >** > **RG NSG** > **...**  >  **NSG FrontEnd**.
2. Στο το blade **Ρυθμίσεις** , κάντε κλικ στην επιλογή **διασυνδέσεις δικτύου**.
3. Εάν υπάρχουν οποιαδήποτε NIC που εμφανίζεται, κάντε κλικ στην επιλογή NIC και ακολουθήστε το βήμα 2 στο [Dissociate ένα NSG από NIC](#Dissociate-an-NSG-from-a-NIC).
4. Επαναλάβετε το βήμα 3 για κάθε NIC.
5. Στο το blade **Ρυθμίσεις** , κάντε κλικ στην επιλογή **δευτερεύοντα δίκτυα**.
6. Εάν υπάρχουν οποιαδήποτε δευτερεύοντα δίκτυα που παρατίθενται, κάντε κλικ στο υποδίκτυο και ακολουθήστε τα βήματα 2 και 3 στο [Dissociate ένα NSG από ένα δευτερεύον δίκτυο](#Dissociate-an-NSG-from-a-subnet).
7. Κύλιση προς τα αριστερά για να το blade **NSG FrontEnd** , στη συνέχεια, κάντε κλικ στην επιλογή **Διαγραφή** > **Ναι**.

[Azure πύλης - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Επόμενα βήματα

- [Ενεργοποίηση καταγραφής](virtual-network-nsg-manage-log.md) για NSGs.
