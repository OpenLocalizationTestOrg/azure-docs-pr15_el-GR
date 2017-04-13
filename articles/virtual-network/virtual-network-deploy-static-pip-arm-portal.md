<properties 
   pageTitle="Ανάπτυξη μια Εικονική με στατική δημόσια IP με την πύλη Azure στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αναπτύξετε το ΣΠΣ με στατική δημόσια IP με την πύλη zure στη Διαχείριση πόρων"
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
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-the-azure-portal"></a>Ανάπτυξη μια Εικονική με στατική δημόσια IP με την πύλη Azure

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Δημιουργήστε μια Εικονική με στατική δημόσια IP 

Για να δημιουργήσετε μια Εικονική με μια στατική δημόσια διεύθυνση IP στην πύλη του Azure, ακολουθήστε τα παρακάτω βήματα.

1. Από ένα πρόγραμμα περιήγησης, μεταβείτε στην [πύλη του Azure](https://portal.azure.com) και, εάν είναι απαραίτητο, πραγματοποιήστε είσοδο με το λογαριασμό της Azure.
2. Στην η επάνω αριστερή γωνία της πύλης, επιλέξτε **Δημιουργία**>>**τον υπολογισμό**>**Windows Server 2012 R2 κέντρου δεδομένων**.
3. Στη λίστα **Επιλέξτε ένα μοντέλο ανάπτυξης** , επιλέξτε **Διαχείριση πόρων** και κάντε κλικ στην επιλογή **Δημιουργία**.
4. Στο blade τα **βασικά στοιχεία** , εισαγάγετε τις πληροφορίες Εικονική, όπως φαίνεται παρακάτω και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

    ![Azure πύλης - βασικά στοιχεία](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)

5. Στο το blade **Επιλέξτε ένα μέγεθος** , κάντε κλικ στην επιλογή **Τυπική A1** όπως φαίνεται παρακάτω και, στη συνέχεια, κάντε κλικ στην **επιλογή**.

    ![Azure πύλης - επιλέξτε ένα μέγεθος](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)

6. Στο το blade **Ρυθμίσεις** , κάντε κλικ στην επιλογή **δημόσια διεύθυνση IP**και, στη συνέχεια, στο blade τη **Δημιουργία δημόσια διεύθυνση IP** , στην περιοχή **ανάθεσης**, κάντε κλικ στην επιλογή **στατικής** όπως φαίνεται παρακάτω. Και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

    ![Azure πύλης - δημιουργία δημόσια διεύθυνση IP](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)

7. Στο το blade **Ρυθμίσεις** , κάντε κλικ στο κουμπί **OK**.
8. Εξετάστε τη **Σύνοψη** blade, όπως φαίνεται παρακάτω και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

    ![Azure πύλης - δημιουργία δημόσια διεύθυνση IP](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)

9. Παρατηρήστε το νέο πλακίδιο στον πίνακα εργαλείων σας.

    ![Azure πύλης - δημιουργία δημόσια διεύθυνση IP](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)

10. Όταν δημιουργηθεί η Εικονική, θα εμφανιστεί το blade **Ρυθμίσεις** όπως φαίνεται παρακάτω

    ![Azure πύλης - δημιουργία δημόσια διεύθυνση IP](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)