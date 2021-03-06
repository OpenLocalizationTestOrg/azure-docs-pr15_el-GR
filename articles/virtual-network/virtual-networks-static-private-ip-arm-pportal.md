<properties 
   pageTitle="Πώς μπορείτε να ορίσετε μια στατική ιδιωτικό IP στη λειτουργία ARM με την πύλη Azure | Microsoft Azure"
   description="Κατανόηση των ιδιωτικών διευθύνσεις IP (πτώσεις) και πώς μπορείτε να διαχειριστείτε τους στη λειτουργία ARM με την πύλη Azure"
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

# <a name="how-to-set-a-static-private-ip-address-in-the-azure-portal"></a>Πώς μπορείτε να ορίσετε μια στατική διεύθυνση IP ιδιωτικό στην πύλη του Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε επίσης να [διαχειριστείτε στατική διεύθυνση IP ιδιωτικό στο μοντέλο κλασική ανάπτυξης](virtual-networks-static-private-ip-classic-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Τα παρακάτω βήματα δείγμα θεωρείτε ότι ένα απλό περιβάλλον που έχετε ήδη δημιουργήσει. Εάν θέλετε να εκτελέσετε τα βήματα που εμφανίζονται σε αυτό το έγγραφο, δημιουργήστε πρώτα το περιβάλλον δοκιμής περιγράφεται στη [Δημιουργία ενός vnet](virtual-networks-create-vnet-arm-pportal.md).

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Πώς μπορείτε να δημιουργήσετε μια Εικονική για σκοπούς δοκιμής στατική ιδιωτικών διευθύνσεων IP

Δεν μπορείτε να ορίσετε μια στατική διεύθυνση IP ιδιωτικό κατά τη δημιουργία της μια Εικονική στα στη λειτουργία ανάπτυξης για τη διαχείριση πόρων, χρησιμοποιώντας την πύλη του Azure. Πρέπει πρώτα να δημιουργήσετε την εικονική Μηχανή, μετά ορίστε τα ιδιωτικά IP να είναι στατική.

Για να δημιουργήσετε μια Εικονική με το όνομα *DNS01* στο δευτερεύον *FrontEnd* από ένα VNet με το όνομα *TestVNet*, ακολουθήστε τα παρακάτω βήματα.

1. Από ένα πρόγραμμα περιήγησης, μεταβείτε στο http://portal.azure.com και, εάν είναι απαραίτητο, πραγματοποιήστε είσοδο με το λογαριασμό της Azure.
2. Κάντε κλικ στην επιλογή **ΔΗΜΙΟΥΡΓΊΑ** > **τον υπολογισμό** > **Windows Server 2012 R2 κέντρο δεδομένων**, παρατηρήστε ότι η λίστα **Επιλέξτε ένα μοντέλο ανάπτυξης** ήδη εμφανίζει **Διαχείριση πόρων**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**, όπως φαίνεται στην παρακάτω εικόνα.

    ![Δημιουργία Εικονική στην πύλη του Azure](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3. Στο blade τα **βασικά στοιχεία** , πληκτρολογήστε το όνομα του η Εικονική σε δημιουργηθούν (*DNS01* σε σενάριο μας), το λογαριασμό του τοπικού διαχειριστή και τον κωδικό πρόσβασης, όπως φαίνεται στην παρακάτω εικόνα.

    ![Βασικά στοιχεία blade](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. Βεβαιωθείτε ότι η **θέση** που επιλέξατε είναι *Κεντρικές ΗΠΑ*, στη συνέχεια, κάντε κλικ στην επιλογή **επιλογή υπάρχον** στην περιοχή **ομάδα πόρων**, στη συνέχεια, κάντε ξανά κλικ **ομάδα πόρων** , στη συνέχεια, κάντε κλικ στην επιλογή *TestRG*, και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

    ![Βασικά στοιχεία blade](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5. Στο το blade **Επιλέξτε ένα μέγεθος** , επιλέξτε **Τυπική A1**και, στη συνέχεια, κάντε κλικ στην **επιλογή**.

    ![Επιλέξτε ένα μέγεθος blade](./media/virtual-networks-static-ip-arm-pportal/figure04.png) 

6. Στο το blade **Ρυθμίσεις** , βεβαιωθείτε ότι έχουν οριστεί από τις ακόλουθες ιδιότητες έχουν οριστεί με τις παρακάτω τιμές και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

    -**Το λογαριασμό χώρου αποθήκευσης**: *vnetstorage*
    - **Δικτύου**: *TestVNet*
    - **Υποδίκτυο**: *FrontEnd*

    ![Επιλέξτε ένα μέγεθος blade](./media/virtual-networks-static-ip-arm-pportal/figure05.png)  

7. Στο blade τη **Σύνοψη** , κάντε κλικ στο κουμπί **OK**. Παρατηρήστε το παρακάτω πλακίδιο εμφανίζεται στον πίνακα εργαλείων σας.

    ![Δημιουργία Εικονική στην πύλη του Azure](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Πώς μπορείτε να ανακτήσετε ιδιωτικό πληροφορίες στατικής διεύθυνσης IP για μια Εικονική

Για να προβάλετε τα ιδιωτικά πληροφορίες στατικής διεύθυνσης IP για την εικονική Μηχανή που δημιουργήθηκαν με τα παραπάνω βήματα, εκτελέστε τα παρακάτω βήματα.

1. Από την πύλη Azure Azure, κάντε κλικ στην επιλογή **ΑΝΑΖΉΤΗΣΗ ΌΛΩΝ** > **εικονικές μηχανές** > **DNS01** > **όλες τις ρυθμίσεις** > **διασυνδέσεις δικτύου** και κατόπιν κάντε κλικ στο περιβάλλον εργασίας μόνο δικτύου που παρατίθενται.

    ![Εικονική πλακίδιο για την ανάπτυξη](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2. Στο το blade **διασύνδεση δικτύου** , κάντε κλικ στην επιλογή **όλες οι ρυθμίσεις** > **διευθύνσεις IP** και παρατηρήστε τις τιμές **ανάθεσης** και **τη διεύθυνση IP** .

    ![Εικονική πλακίδιο για την ανάπτυξη](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Πώς να προσθέσετε μια στατική διεύθυνση IP ιδιωτικό σε υπάρχουσα Εικονική μηχανή
Για να προσθέσετε μια στατική διεύθυνση IP ιδιωτικό η Εικονική δημιουργήθηκε χρησιμοποιώντας τα παραπάνω βήματα, ακολουθήστε τα παρακάτω βήματα:

1. Από το blade **διευθύνσεις IP** που εμφανίζεται παραπάνω, κάντε κλικ στην επιλογή **στατικής** στην περιοχή **ανάθεσης**.
2. Πληκτρολογήστε *192.168.1.101* για **τη διεύθυνση IP**και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![Δημιουργία Εικονική στην πύλη του Azure](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] Εάν μετά κάνοντας κλικ στην επιλογή **Αποθήκευση** παρατηρήσετε ότι ανάθεσης εξακολουθεί να έχει οριστεί σε **δυναμικό**, αυτό σημαίνει ότι η διεύθυνση IP που πληκτρολογήσατε χρησιμοποιείται ήδη. Δοκιμάστε μια διαφορετική διεύθυνση IP.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Πώς να καταργήσετε μια στατική διεύθυνση IP ιδιωτικό από μια εικονική Μηχανή
Για να καταργήσετε τη στατική διεύθυνση IP ιδιωτικό η Εικονική δημιουργήθηκε παραπάνω, ακολουθήστε τα παρακάτω βήματα.
    
1. Από το φαίνεται παραπάνω blade **διευθύνσεις IP** , **δυναμική** κάντε κλικ στην επιλογή **ανάθεση**και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε σχετικά με τις διευθύνσεις [δεσμευμένη δημόσια διεύθυνση IP](virtual-networks-reserved-public-ip.md) .
- Μάθετε σχετικά με τις διευθύνσεις [επιπέδου παρουσίας δημόσια IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Συμβουλευθείτε τα [APIs ΥΠΌΛΟΙΠΑ δεσμευμένη διεύθυνση IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).