<properties 
   pageTitle="Πώς μπορείτε να ορίσετε μια στατική ιδιωτικό IP σε κλασική λειτουργία με την πύλη Azure | Microsoft Azure"
   description="Κατανόηση των στατική ιδιωτικό διευθύνσεις IP και πώς μπορείτε να διαχειριστείτε τους σε κλασική λειτουργία με την πύλη Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-the-azure-portal"></a>Πώς μπορείτε να ορίσετε μια στατική διεύθυνση IP ιδιωτικό (κλασική) στην πύλη του Azure

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο κλασική ανάπτυξης. Μπορείτε επίσης να [διαχειριστείτε μια στατική διεύθυνση IP ιδιωτικό στο μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-networks-static-private-ip-arm-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Τα παρακάτω βήματα δείγμα θεωρείτε ότι ένα απλό περιβάλλον που έχετε ήδη δημιουργήσει. Εάν θέλετε να εκτελέσετε τα βήματα που εμφανίζονται σε αυτό το έγγραφο, δημιουργήστε πρώτα το περιβάλλον δοκιμής περιγράφεται στη [Δημιουργία ενός vnet](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Πώς μπορείτε να καθορίσετε μια στατική διεύθυνση IP ιδιωτικά, κατά τη δημιουργία μια εικονική Μηχανή
Για να δημιουργήσετε μια Εικονική με το όνομα *DNS01* στο δευτερεύον *FrontEnd* από ένα VNet με το όνομα *TestVNet* με στατική ιδιωτικό IP του *192.168.1.101*, ακολουθήστε τα παρακάτω βήματα:

1. Από ένα πρόγραμμα περιήγησης, μεταβείτε στο http://portal.azure.com και, εάν είναι απαραίτητο, πραγματοποιήστε είσοδο με το λογαριασμό της Azure.
2. Κάντε κλικ στην επιλογή **ΔΗΜΙΟΥΡΓΊΑ** > **τον υπολογισμό** > **Windows Server 2012 R2 κέντρο δεδομένων**, παρατηρήστε ότι η λίστα **Επιλέξτε ένα μοντέλο ανάπτυξης** ήδη εμφανίζει **κλασική**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

    ![Δημιουργία Εικονική στην πύλη του Azure](./media/virtual-networks-static-ip-classic-pportal/figure01.png)

3. Στο η **Δημιουργία Εικονική** blade, πληκτρολογήστε το όνομα του η Εικονική σε δημιουργηθούν (*DNS01* σε σενάριο μας), το λογαριασμό του τοπικού διαχειριστή και τον κωδικό πρόσβασης.

    ![Δημιουργία Εικονική στην πύλη του Azure](./media/virtual-networks-static-ip-classic-pportal/figure02.png)

4. Κάντε κλικ στην επιλογή **Προαιρετικό ρύθμισης παραμέτρων** > **δικτύου** > **Εικονικού δικτύου**, και, στη συνέχεια, κάντε κλικ στην επιλογή **TestVNet**. Εάν **TestVNet** δεν είναι διαθέσιμη, βεβαιωθείτε ότι χρησιμοποιείτε τη θέση *Κεντρικές ΗΠΑ* και έχετε δημιουργήσει το περιβάλλον δοκιμής περιγράφεται στην αρχή αυτού του άρθρου.

    ![Δημιουργία Εικονική στην πύλη του Azure](./media/virtual-networks-static-ip-classic-pportal/figure03.png)

5. Στο το blade **δικτύου** , βεβαιωθείτε ότι το επιλεγμένο τη συγκεκριμένη στιγμή είναι *FrontEnd*, στη συνέχεια, κάντε κλικ στην επιλογή **διευθύνσεις IP**, στην περιοχή **εκχώρηση διευθύνσεων IP** , κάντε κλικ στην επιλογή **στατικής**και, στη συνέχεια, εισαγάγετε *192.168.1.101* διεύθυνση **IP** , όπως φαίνεται παρακάτω.

    ![Δημιουργία Εικονική στην πύλη του Azure](./media/virtual-networks-static-ip-classic-pportal/figure04.png)   

6. Κάντε κλικ στο κουμπί **OK** στο το blade **διευθύνσεις IP** , στη συνέχεια, κάντε κλικ στο κουμπί **OK** στο το blade **δικτύου** , και επιλέξτε **OK** στο το blade **προαιρετικό config** .
7. Στο η **Δημιουργία Εικονική** blade, κάντε κλικ στην επιλογή **Δημιουργία**. Παρατηρήστε το παρακάτω πλακίδιο εμφανίζεται στον πίνακα εργαλείων σας.

    ![Δημιουργία Εικονική στην πύλη του Azure](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Πώς μπορείτε να ανακτήσετε ιδιωτικό πληροφορίες στατικής διεύθυνσης IP για μια Εικονική

Για να προβάλετε τα ιδιωτικά πληροφορίες στατικής διεύθυνσης IP για την εικονική Μηχανή που δημιουργήθηκαν με τα παραπάνω βήματα, εκτελέστε τα παρακάτω βήματα.

1. Από την πύλη Azure Azure, κάντε κλικ στην επιλογή **ΑΝΑΖΉΤΗΣΗ ΌΛΩΝ** > **εικονικές μηχανές (κλασική)** > **DNS01** > **όλες τις ρυθμίσεις** > **διευθύνσεις IP** και ειδοποίηση την εκχώρηση διευθύνσεων IP και διευθύνσεων IP όπως φαίνεται παρακάτω.

    ![Δημιουργία Εικονική στην πύλη του Azure](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Πώς να καταργήσετε μια στατική διεύθυνση IP ιδιωτικό από μια εικονική Μηχανή
Για να καταργήσετε τη στατική διεύθυνση IP ιδιωτικό από την εικονική Μηχανή δημιουργήθηκε παραπάνω, ακολουθήστε τα παρακάτω βήματα.
    
1. Από το blade **διευθύνσεις IP** που εμφανίζεται παραπάνω, κάντε κλικ στην επιλογή **δυναμικής** στα δεξιά της **ανάθεσης διεύθυνση IP**, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι**.

    ![Δημιουργία Εικονική στην πύλη του Azure](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Πώς να προσθέσετε μια στατική διεύθυνση IP ιδιωτικό σε υπάρχουσα Εικονική μηχανή
Για να προσθέσετε μια στατική διεύθυνση IP ιδιωτικό η Εικονική δημιουργήθηκε χρησιμοποιώντας τα παραπάνω βήματα, ακολουθήστε τα παρακάτω βήματα:

1. Από το blade **διευθύνσεις IP** που εμφανίζεται παραπάνω, κάντε κλικ στην επιλογή **στατικής** στα δεξιά της **εκχώρηση διευθύνσεων IP**.
2. Πληκτρολογήστε *192.168.1.101* για τη **διεύθυνση IP**, στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**και, στη συνέχεια, κάντε κλικ στο κουμπί **Ναι**.

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε σχετικά με τις διευθύνσεις [δεσμευμένη δημόσια διεύθυνση IP](virtual-networks-reserved-public-ip.md) .
- Μάθετε σχετικά με τις διευθύνσεις [επιπέδου παρουσίας δημόσια IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Συμβουλευθείτε τα [APIs ΥΠΌΛΟΙΠΑ δεσμευμένη διεύθυνση IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).