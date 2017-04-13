<properties 
   pageTitle="Πώς μπορείτε να δημιουργήσετε NSGs στη λειτουργία ARM με την πύλη Azure | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε και να αναπτύξετε NSGs στο ARM με την πύλη Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-manage-nsgs-using-the-azure-portal"></a>Πώς μπορείτε να διαχειριστείτε NSGs με την πύλη Azure

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε επίσης να [δημιουργήσετε NSGs στο μοντέλο κλασική ανάπτυξης](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Το δείγμα PowerShell παρακάτω εντολές περιμένετε ένα απλό περιβάλλον που έχετε ήδη δημιουργήσει με βάση το παραπάνω σενάριο. Εάν θέλετε να εκτελέσετε τις εντολές που εμφανίζονται σε αυτό το έγγραφο, πρώτα δημιουργία δοκιμαστικό περιβάλλον με [αυτό το πρότυπο](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd)για την ανάπτυξη, κάντε κλικ στην επιλογή **Ανάπτυξη να Azure**, αντικαταστήστε τις προεπιλεγμένες τιμές παραμέτρων Εάν είναι απαραίτητο, και ακολουθήστε τις οδηγίες στην πύλη. Τα παρακάτω βήματα χρησιμοποιούν **RG NSG** ως το όνομα της ομάδας πόρων το πρότυπο έχει αναπτυχθεί σε.

## <a name="create-the-nsg-frontend-nsg"></a>Δημιουργία του NSG NSG FrontEnd

Για να δημιουργήσετε το **NSG FrontEnd** NSG, όπως φαίνεται στο παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα.

1. Από ένα πρόγραμμα περιήγησης, μεταβείτε στο http://portal.azure.com και, εάν είναι απαραίτητο, πραγματοποιήστε είσοδο με το λογαριασμό της Azure.
2. Κάντε κλικ στην επιλογή **Αναζήτηση >** > **ομάδες ασφαλείας δικτύου**.

    ![Azure πύλης - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. Στο το blade **ομάδες ασφαλείας δικτύου** , κάντε κλικ στην επιλογή **Προσθήκη**.
  
    ![Azure πύλης - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. Στο το blade **Δημιουργία ομάδας ασφαλείας δικτύου** , δημιουργήστε ένα NSG με το όνομα *NSG FrontEnd* στην ομάδα πόρων *RG NSG* και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Azure πύλης - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Δημιουργία κανόνων σε μια υπάρχουσα NSG

Για να δημιουργήσετε κανόνες σε μια υπάρχουσα NSG από την πύλη του Azure, ακολουθήστε τα παρακάτω βήματα.

2. Κάντε κλικ στην επιλογή **Αναζήτηση >** > **ομάδες ασφαλείας δικτύου**.

3. Στη λίστα των NSGs, κάντε κλικ στην επιλογή **NSG FrontEnd** > **κανόνες εισερχομένων ασφαλείας**

    ![Azure πύλης - NSG FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. Στη λίστα των **κανόνων εισερχομένων ασφαλείας**, κάντε κλικ στην επιλογή **Προσθήκη**.

    ![Azure πύλης - Προσθήκη κανόνα](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. Στο το blade **Προσθήκη κανόνα εισερχομένων ασφαλείας** , δημιουργήστε έναν κανόνα που ονομάζεται *web κανόνα* με προτεραιότητα *200* υπάρχει δυνατότητα πρόσβασης μέσω *TCP* θύρα *80* σε οποιαδήποτε Εικονική από οποιαδήποτε προέλευση και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**. Παρατηρήστε ότι οι περισσότερες από αυτές τις ρυθμίσεις είναι ήδη προεπιλεγμένες τιμές.

    ![Azure πύλης - ρυθμίσεων κανόνα](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. Μετά από μερικά δευτερόλεπτα θα δείτε τον νέο κανόνα στο το NSG.

    ![Azure πύλης - Δημιουργία κανόνα](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Επαναλάβετε τα βήματα 6 για να δημιουργήσετε έναν κανόνα εισερχομένων με το όνομα *rdp κανόνα* με προτεραιότητα *250* υπάρχει δυνατότητα πρόσβασης μέσω *TCP* θύρα *3389* σε οποιαδήποτε Εικονική από οποιαδήποτε προέλευση.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>Συσχέτιση του NSG στο υποδίκτυο FrontEnd

1. Κάντε κλικ στην επιλογή **Αναζήτηση >** > **ομάδες πόρων** > **RG NSG**.
2. Στο το blade **RG NSG** , κάντε κλικ στην επιλογή **...**  >  **TestVNet**.

    ![Azure πύλης - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. Στο το blade **Ρυθμίσεις** , κάντε κλικ στην επιλογή **δευτερεύοντα δίκτυα** > **FrontEnd** > **ομάδα ασφαλείας δικτύου** > **NSG FrontEnd**.

    ![Azure πύλης - ρυθμίσεις υποδίκτυο](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. Στο το blade **FrontEnd** , κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Azure πύλης - ρυθμίσεις υποδίκτυο](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>Δημιουργία του NSG NSG παρασκηνίου

Για να δημιουργήσετε **NSG παρασκηνίου** NSG και να συσχετίσετε στο υποδίκτυο **παρασκηνίου** , ακολουθήστε τα παρακάτω βήματα.

1. Επαναλάβετε τα βήματα στο θέμα [Δημιουργία το NSG NSG FrontEnd](#Create-the-NSG-FrontEnd-NSG) για να δημιουργήσετε μια NSG με το όνομα *NSG παρασκηνίου*
2. Επαναλάβετε τα βήματα στο θέμα [Δημιουργία κανόνων σε μια υπάρχουσα NSG](#Create-rules-in-an-existing-NSG) για τη δημιουργία των κανόνων **εισερχομένων** στον παρακάτω πίνακα.

  	|Κανόνας εισερχομένων|Κανόνας εξερχομένων|
  	|---|---|
  	|![Azure πύλης - κανόνα εισερχομένων](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Azure πύλης - εξερχομένων κανόνα](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Επαναλάβετε τα βήματα στο θέμα [συσχετίσετε το NSG στο υποδίκτυο FrontEnd](#Associate-the-NSG-to-the-FrontEnd-subnet) για να συσχετίσετε **NSG παρασκηνίου** NSG στο υποδίκτυο **παρασκηνίου** .

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε πώς μπορείτε να [διαχειριστείτε τις υπάρχουσες NSGs](virtual-network-manage-nsg-arm-portal.md)
- [Ενεργοποίηση καταγραφής](virtual-network-nsg-manage-log.md) για NSGs.