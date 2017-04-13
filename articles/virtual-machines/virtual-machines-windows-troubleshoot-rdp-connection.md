<properties
    pageTitle="Δεν είναι δυνατό να RDP σε μια Εικονική Azure | Microsoft Azure"
    description="Αντιμετώπιση προβλημάτων όταν δεν μπορείτε να συνδεθείτε στον υπολογιστή σας εικονικές Windows στο Azure χρησιμοποιώντας απομακρυσμένης επιφάνειας εργασίας"
    keywords="Σφάλμα απομακρυσμένης επιφάνειας εργασίας, το σφάλμα σύνδεση απομακρυσμένης επιφάνειας εργασίας, να συνδεθείτε με το Εικονική αντιμετώπιση προβλημάτων απομακρυσμένης επιφάνειας εργασίας"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/26/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Αντιμετώπιση προβλημάτων συνδέσεις απομακρυσμένης επιφάνειας εργασίας για μια εικονική μηχανή Azure

Η σύνδεση απομακρυσμένης επιφάνειας εργασίας Protocol (RDP) σας βασίζεται σε Windows Azure εικονική μηχανή (Εικονική) μπορεί να αποτύχει για διάφορους λόγους, διατηρώντας που δεν είναι δυνατή η πρόσβαση του Εικονική. Το πρόβλημα μπορεί να είναι με την υπηρεσία απομακρυσμένης επιφάνειας εργασίας σε η Εικονική, η σύνδεση δικτύου ή το πρόγραμμα-πελάτη απομακρυσμένης επιφάνειας εργασίας στον κεντρικό υπολογιστή. Σε αυτό το άρθρο σάς καθοδηγεί ορισμένες από τις πιο συνηθισμένες μεθόδους για την επίλυση ζητημάτων σύνδεσης RDP. 

Εάν χρειάζεστε περισσότερη βοήθεια σε οποιοδήποτε σημείο σε αυτό το άρθρο, μπορείτε να επικοινωνήσετε με το Azure τους ειδικούς στο [MSDN Azure και φόρουμ υπερχείλισης στοίβας](https://azure.microsoft.com/support/forums/). Εναλλακτικά, μπορείτε να υποβάλετε ένα περιστατικό Azure υποστήριξης. Μεταβείτε στην [τοποθεσία υποστήριξης Azure](https://azure.microsoft.com/support/options/) και επιλέξτε **Λήψη υποστήριξης**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Γρήγορα βήματα αντιμετώπισης προβλημάτων
Μετά από κάθε βήμα της αντιμετώπισης προβλημάτων, δοκιμάστε ξανά για να την εικονική Μηχανή:

1. Επαναφορά ρύθμισης παραμέτρων απομακρυσμένης επιφάνειας εργασίας.
2. Επιλέξτε ομάδα ασφαλείας δικτύου κανόνες / Cloud Services τελικά σημεία.
3. Επισκόπηση των αρχείων καταγραφής κονσόλας Εικονική.
4. Ελέγξτε την εύρυθμη λειτουργία Εικονική πόρων.
5. Επαναφέρετε τον κωδικό πρόσβασής σας Εικονική.
6. Επανεκκινήστε το Εικονική.
7. Αναπτύξτε ξανά το Εικονική.

Συνέχιση ανάγνωσης Εάν χρειάζεστε πιο λεπτομερείς οδηγίες και επεξηγήσεις.

> [AZURE.TIP] Εάν το κουμπί **σύνδεσης** για την Εικονική είναι απενεργοποιημένη στην πύλη και δεν είστε συνδεδεμένοι σε Azure μέσω μιας σύνδεσης [VPN-τοποθεσίας](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) ή [Δρομολόγηση Express](../expressroute/expressroute-introduction.md) , πρέπει να δημιουργήσετε και να εκχωρήσετε μια δημόσια διεύθυνση IP σας Εικονική να μπορέσετε να χρησιμοποιήσετε RDP. Μπορείτε να διαβάσετε περισσότερα σχετικά με τις [δημόσιες διευθύνσεις IP στο Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-to-troubleshoot-rdp-issues"></a>Τρόποι για την αντιμετώπιση προβλημάτων RDP
Μπορείτε να αντιμετωπίσετε ΣΠΣ που δημιουργούνται με χρήση του μοντέλου ανάπτυξης για τη διαχείριση πόρων, χρησιμοποιώντας μία από τις παρακάτω μεθόδους:

- [Azure πύλη](#using-the-azure-portal) - εξαιρετική εάν πρέπει να τον επαναφέρετε γρήγορα τα διαπιστευτήρια RDP χρηστών ή ρύθμισης παραμέτρων και δεν έχετε το Azure εγκατεστημένα τα εργαλεία.
- [Azure PowerShell](#using-azure-powershell) - Εάν είστε εξοικειωμένοι με μια γραμμή εντολών του PowerShell, γρήγορα επαναφορά του RDP ρύθμισης παραμέτρων χρήστη διαπιστευτήρια χρησιμοποιώντας τα cmdlet του Azure PowerShell.

Μπορείτε επίσης να βρείτε τα βήματα αντιμετώπισης προβλημάτων ΣΠΣ που δημιουργήθηκε με το [μοντέλο ανάπτυξης κλασική](#troubleshoot-vms-created-using-the-classic-deployment-model).


<a id="fix-common-remote-desktop-errors"></a>
## <a name="troubleshoot-using-the-azure-portal"></a>Αντιμετώπιση προβλημάτων με την πύλη Azure
Μετά από κάθε βήμα της αντιμετώπισης προβλημάτων, δοκιμάστε να συνδεθείτε ξανά με το Εικονική. Εάν εξακολουθείτε να μπορείτε να συνδεθείτε, δοκιμάστε το επόμενο βήμα.

1. **Επαναφέρετε τη σύνδεση RDP**. Αυτό το βήμα αντιμετώπισης προβλημάτων επαναφέρει τη ρύθμιση παραμέτρων RDP όταν είναι απενεργοποιημένες απομακρυσμένων συνδέσεων ή κανόνες τείχους προστασίας των Windows εμποδίζουν RDP, για παράδειγμα.

    Επιλέξτε το Εικονική στην πύλη του Azure. Κάντε κύλιση προς τα κάτω παράθυρο ρυθμίσεις στην ενότητα **υποστήριξης + αντιμετώπιση προβλημάτων** κοντά στο κάτω μέρος της λίστας. Κάντε κλικ στο κουμπί **Επαναφορά κωδικού πρόσβασης** . Ορίστε τη **λειτουργία** για να **επαναφέρετε μόνο ρύθμιση παραμέτρων** και, στη συνέχεια, κάντε κλικ στο κουμπί **Ενημέρωση** :

    ![Επαναφέρει τη ρύθμιση παραμέτρων RDP στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-rdp.png)

2. **Κανόνες επαλήθευση ομάδα ασφαλείας δικτύου**. Αυτό το βήμα αντιμετώπισης προβλημάτων επαληθεύει ότι έχετε έναν κανόνα στο σας ομάδα ασφαλείας δικτύου για να επιτρέψετε την κυκλοφορία RDP. Η προεπιλεγμένη θύρα για RDP έχει αλλάξει. Ένας κανόνας για να επιτρέψετε την κυκλοφορία RDP ενδέχεται να μην δημιουργηθούν αυτόματα κατά τη δημιουργία του Εικονική.

    Επιλέξτε το Εικονική στην πύλη του Azure. Κάντε κλικ στην επιλογή οι **διασυνδέσεις δικτύου** από το παράθυρο ρυθμίσεων.

    ![Προβολή διασυνδέσεις δικτύου για μια Εικονική στα Azure πύλη](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-network-interfaces.png)

    Επιλέξτε το περιβάλλον εργασίας του δικτύου από τη λίστα (υπάρχει συνήθως μόνο):

    ![Επιλέξτε διασύνδεση δικτύου στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-interface.png)

    Επιλέξτε την **ομάδα ασφαλείας δικτύου** για να δείτε την ομάδα ασφαλείας δικτύου που σχετίζονται με το περιβάλλον εργασίας του δικτύου:

    ![Επιλέξτε ομάδα ασφαλείας δικτύου στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-nsg.png)

    Βεβαιωθείτε ότι υπάρχει σε έναν κανόνα εισερχομένων που επιτρέπει την κυκλοφορία RDP στη αλλάξει. Το παρακάτω παράδειγμα εμφανίζει έναν κανόνα έγκυρη ασφάλειας που επιτρέπει την κυκλοφορία RDP. Μπορείτε να δείτε `Service` και `Action` έχουν ρυθμιστεί σωστά:

    ![Επαλήθευση του κανόνα RDP NSG στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/verify-nsg-rules.png)

    Εάν δεν έχετε έναν κανόνα που επιτρέπει την κυκλοφορία RDP, [Δημιουργήστε έναν κανόνα ομάδας ασφαλείας δικτύου](virtual-machines-windows-nsg-quickstart-portal.md). Να επιτρέπεται αλλάξει.

3. **Διαγνωστικά εκκίνησης Εικονική αναθεώρηση**. Αυτό το βήμα αντιμετώπισης προβλημάτων ελέγχει τα αρχεία καταγραφής κονσόλας Εικονική για να προσδιορίσετε εάν η Εικονική είναι η αναφορά κάποιο πρόβλημα. Δεν έχουν όλοι ΣΠΣ έχουν Διαγνωστικά εκκίνησης ενεργοποιημένο, ώστε αυτό το βήμα αντιμετώπισης προβλημάτων μπορεί να είναι προαιρετικό.
    
    Συγκεκριμένα βήματα αντιμετώπισης προβλημάτων είναι πέρα από το πεδίο αυτού του άρθρου, αλλά μπορεί να υποδεικνύουν ένα ευρύτερο πρόβλημα που επηρεάζει την RDP συνδεσιμότητας. Για περισσότερες πληροφορίες σχετικά με την αναθεώρηση των τα αρχεία καταγραφής από την κονσόλα και Εικονική στιγμιότυπο οθόνης, ανατρέξτε στο θέμα [Διαγνωστικά εκκίνησης για ΣΠΣ](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **Ελέγξτε την εύρυθμη λειτουργία Εικονική πόρων**. Αυτό το βήμα αντιμετώπισης προβλημάτων επαληθεύει υπάρχουν γνωστά θέματα με την πλατφόρμα Azure που ίσως να επηρεάσουν τη δυνατότητα σύνδεσης με την εικονική Μηχανή.

    Επιλέξτε το Εικονική στην πύλη του Azure. Κάντε κύλιση προς τα κάτω παράθυρο ρυθμίσεις στην ενότητα **υποστήριξης + αντιμετώπιση προβλημάτων** κοντά στο κάτω μέρος της λίστας. Κάντε κλικ στο κουμπί **εύρυθμη λειτουργία των πόρων** . Μια καλή Εικονική αναφορές είναι **διαθέσιμες**:

    ![Έλεγχος εύρυθμης λειτουργίας πόρων Εικονική στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/check-resource-health.png)

5. **Επαναφορά διαπιστευτήρια χρήστη**. Αυτό το βήμα αντιμετώπισης προβλημάτων επαναφέρει τον κωδικό πρόσβασης σε λογαριασμό τοπικού διαχειριστή όταν δεν είστε βέβαιοι για ή έχετε ξεχάσει τα διαπιστευτήρια.

    Επιλέξτε το Εικονική στην πύλη του Azure. Κάντε κύλιση προς τα κάτω παράθυρο ρυθμίσεις στην ενότητα **υποστήριξης + αντιμετώπιση προβλημάτων** κοντά στο κάτω μέρος της λίστας. Κάντε κλικ στο κουμπί **Επαναφορά κωδικού πρόσβασης** . Βεβαιωθείτε ότι η **λειτουργία** έχει οριστεί να **επαναφέρει τον κωδικό πρόσβασης** και, στη συνέχεια, πληκτρολογήστε το όνομα χρήστη και έναν νέο κωδικό πρόσβασης. Τέλος, κάντε κλικ στο κουμπί **Ενημέρωση** :

    ![Επαναφέρετε τα διαπιστευτήρια του χρήστη στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-password.png)

6. **Επανεκκινήστε το Εικονική**. Αυτό το βήμα αντιμετώπισης προβλημάτων μπορεί να διορθώσει υποκείμενο προβλήματα αντιμετωπίζει τα Εικονική ίδια.

    Επιλέξτε το Εικονική στην πύλη του Azure και κάντε κλικ στην καρτέλα **Επισκόπηση** . Κάντε κλικ στο κουμπί **επανεκκίνηση** :

    ![Επανεκκινήστε την εικονική Μηχανή στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/restart-vm.png)

7. **Αναπτύξτε ξανά το Εικονική**. Αυτό το βήμα αντιμετώπισης προβλημάτων redeploys σας Εικονική σε άλλη υπηρεσία παροχής φιλοξενίας εντός Azure για να διορθώσετε τυχόν υποκείμενα πλατφόρμα ή ζητήματα δικτύωσης.

    Επιλέξτε το Εικονική στην πύλη του Azure. Κάντε κύλιση προς τα κάτω παράθυρο ρυθμίσεις στην ενότητα **υποστήριξης + αντιμετώπιση προβλημάτων** κοντά στο κάτω μέρος της λίστας. Κάντε κλικ στο κουμπί **αναπτύξτε ξανά** και, στη συνέχεια, κάντε κλικ στην επιλογή **αναπτύξτε ξανά**:

    ![Αναπτύξτε ξανά την εικονική Μηχανή στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/redeploy-vm.png)

    Αφού ολοκληρωθεί αυτή η λειτουργία, χάνονται δεδομένα προσωρινές δίσκου και ενημερώνονται δυναμικές διευθύνσεις IP που συσχετίζονται με την εικονική Μηχανή.

Εάν εξακολουθείτε να αντιμετωπίζετε προβλήματα RDP, μπορείτε να [ανοίξετε μια αίτηση υποστήριξης](https://azure.microsoft.com/support/options/) ή να διαβάσετε [πιο λεπτομερείς RDP αντιμετώπισης προβλημάτων έννοιες και τα βήματα](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-using-azure-powershell"></a>Αντιμετώπιση προβλημάτων με χρήση του PowerShell Azure
Εάν δεν το έχετε κάνει ήδη, [Εγκαταστήστε και ρυθμίστε την πιο πρόσφατη Azure PowerShell](../powershell-install-configure.md).

Τα παρακάτω παραδείγματα χρησιμοποιούν μεταβλητές όπως `myResourceGroup`, `myVM`, και `myVMAccessExtension`. Αντικαταστήστε αυτά τα ονόματα των μεταβλητών και θέσεις με τις δικές σας τιμές.

> [AZURE.NOTE] Μπορείτε να επαναφέρετε τα διαπιστευτήρια του χρήστη και τη ρύθμιση παραμέτρων RDP χρησιμοποιώντας το cmdlet [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) PowerShell. Στα παρακάτω παραδείγματα, `myVMAccessExtension` είναι το όνομα που ορίσατε ως μέρος της διαδικασίας. Εάν έχετε χρησιμοποιήσει προηγουμένως με το VMAccessAgent, μπορείτε να λάβετε το όνομα της υπάρχουσας επέκτασης με τη χρήση `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` για να ελέγξετε τις ιδιότητες του Εικονική. Για να προβάλετε το όνομα, κοιτάξτε κάτω από την ενότητα 'Επεκτάσεις' του αποτελέσματος.

Μετά από κάθε βήμα της αντιμετώπισης προβλημάτων, δοκιμάστε να συνδεθείτε ξανά με το Εικονική. Εάν εξακολουθείτε να μπορείτε να συνδεθείτε, δοκιμάστε το επόμενο βήμα.

1. **Επαναφέρετε τη σύνδεση RDP**. Αυτό το βήμα αντιμετώπισης προβλημάτων επαναφέρει τη ρύθμιση παραμέτρων RDP όταν είναι απενεργοποιημένες απομακρυσμένων συνδέσεων ή κανόνες τείχους προστασίας των Windows εμποδίζουν RDP, για παράδειγμα.

    Το παράδειγμα παρακολούθηση επαναφέρει τη σύνδεση RDP μια Εικονική με το όνομα `myVM` στο το `WestUS` θέση και στην ομάδα πόρων με το όνομα `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```

2. **Κανόνες επαλήθευση ομάδα ασφαλείας δικτύου**. Αυτό το βήμα αντιμετώπισης προβλημάτων επαληθεύει ότι έχετε έναν κανόνα στο σας ομάδα ασφαλείας δικτύου για να επιτρέψετε την κυκλοφορία RDP. Η προεπιλεγμένη θύρα για RDP έχει αλλάξει. Ένας κανόνας για να επιτρέψετε την κυκλοφορία RDP ενδέχεται να μην δημιουργηθούν αυτόματα κατά τη δημιουργία του Εικονική.

    Πρώτα, να αντιστοιχίσετε όλα τα δεδομένα ρύθμισης παραμέτρων για την ομάδα ασφαλείας δικτύου σας για να το `$rules` μεταβλητή. Το παρακάτω παράδειγμα λαμβάνει πληροφορίες σχετικά με την ομάδα ασφαλείας δίκτυο με το όνομα `myNetworkSecurityGroup` στην ομάδα πόρων με το όνομα `myResourceGroup`:

    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```

    Τώρα, μπορείτε να προβάλετε τους κανόνες που έχουν ρυθμιστεί για αυτήν την ομάδα ασφαλείας δικτύου. Βεβαιωθείτε ότι υπάρχει ένας κανόνας για να επιτρέψετε τη θύρα TCP 3389 για εισερχόμενες συνδέσεις ως εξής:

    ```powershell
    $rules.SecurityRules
    ```

    Το παρακάτω παράδειγμα εμφανίζει έναν κανόνα έγκυρη ασφάλειας που επιτρέπει την κυκλοφορία RDP. Μπορείτε να δείτε `Protocol`, `DestinationPortRange`, `Access`, και `Direction` έχουν ρυθμιστεί σωστά:

    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```

    Εάν δεν έχετε έναν κανόνα που επιτρέπει την κυκλοφορία RDP, [Δημιουργήστε έναν κανόνα ομάδας ασφαλείας δικτύου](virtual-machines-windows-nsg-quickstart-powershell.md). Να επιτρέπεται αλλάξει.

3. **Επαναφορά διαπιστευτήρια χρήστη**. Αυτό το βήμα αντιμετώπισης προβλημάτων επαναφέρει τον κωδικό πρόσβασης του λογαριασμού τοπικού διαχειριστή που καθορίζετε όταν δεν είστε βέβαιοι για ή έχετε ξεχάσει, τα διαπιστευτήρια.

    Πρώτα, καθορίστε το όνομα χρήστη και έναν νέο κωδικό πρόσβασης με την εκχώρηση διαπιστευτήρια για να το `$cred` μεταβλητή ως εξής:

    ```powershell
    $cred=Get-Credential
    ```

    Τώρα, ενημερώστε τα διαπιστευτήρια στην Εικονική σας. Το παρακάτω παράδειγμα ενημερώνει τα διαπιστευτήρια σε μια Εικονική με το όνομα `myVM` στο το `WestUS` θέση και στην ομάδα πόρων με το όνομα `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```

4. **Επανεκκινήστε το Εικονική**. Αυτό το βήμα αντιμετώπισης προβλημάτων μπορεί να διορθώσει υποκείμενο προβλήματα αντιμετωπίζει τα Εικονική ίδια.

    Το παρακάτω παράδειγμα επανεκκίνηση του την εικονική Μηχανή με το όνομα `myVM` στην ομάδα πόρων με το όνομα `myResourceGroup`:

    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```

5. **Αναπτύξτε ξανά το Εικονική**. Αυτό το βήμα αντιμετώπισης προβλημάτων redeploys σας Εικονική σε άλλη υπηρεσία παροχής φιλοξενίας εντός Azure για να διορθώσετε τυχόν υποκείμενα πλατφόρμα ή ζητήματα δικτύωσης.

    Το παρακάτω παράδειγμα redeploys η Εικονική με το όνομα `myVM` στο το `WestUS` θέση και στην ομάδα πόρων με το όνομα `myResourceGroup`:

    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Εάν εξακολουθείτε να αντιμετωπίζετε προβλήματα RDP, μπορείτε να [ανοίξετε μια αίτηση υποστήριξης](https://azure.microsoft.com/support/options/) ή να διαβάσετε [πιο λεπτομερείς RDP αντιμετώπισης προβλημάτων έννοιες και τα βήματα](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Αντιμετώπιση προβλημάτων ΣΠΣ που δημιουργούνται με χρήση του μοντέλου κλασική ανάπτυξης

Μετά από κάθε βήμα της αντιμετώπισης προβλημάτων, δοκιμάστε ξανά για να την εικονική Μηχανή.

1. **Επαναφέρετε τη σύνδεση RDP**. Αυτό το βήμα αντιμετώπισης προβλημάτων επαναφέρει τη ρύθμιση παραμέτρων RDP όταν είναι απενεργοποιημένες απομακρυσμένων συνδέσεων ή κανόνες τείχους προστασίας των Windows εμποδίζουν RDP, για παράδειγμα.

    Επιλέξτε το Εικονική στην πύλη του Azure. Κάντε κλικ στην επιλογή το **... Περισσότερες** κουμπί και, στη συνέχεια, κάντε κλικ στην επιλογή **Επαναφορά της απομακρυσμένης πρόσβασης**:

    ![Επαναφέρει τη ρύθμιση παραμέτρων RDP στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-rdp.png)

2.  **Επαληθεύστε τις υπηρεσίες Cloud τελικά σημεία**. Αυτό το βήμα αντιμετώπισης προβλημάτων επαληθεύει ότι έχετε τα τελικά σημεία στο τις υπηρεσίες Cloud για να επιτρέψετε την κυκλοφορία RDP. Η προεπιλεγμένη θύρα για RDP έχει αλλάξει. Ένας κανόνας για να επιτρέψετε την κυκλοφορία RDP ενδέχεται να μην δημιουργηθούν αυτόματα κατά τη δημιουργία του Εικονική.

    Επιλέξτε το Εικονική στην πύλη του Azure. Κάντε κλικ στο κουμπί **τελικά σημεία** για να προβάλετε τα τελικά σημεία που έχουν ρυθμιστεί για την Εικονική. Βεβαιωθείτε ότι τα τελικά σημεία υπάρχουν που επιτρέπουν την κυκλοφορία RDP στην αλλάξει.
    
    Το παρακάτω παράδειγμα εμφανίζει έγκυρη τελικά σημεία που επιτρέπει την κυκλοφορία RDP:

    ![Επαληθεύστε τις υπηρεσίες Cloud τελικά σημεία στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)

    Εάν δεν έχετε ένα τελικό σημείο που επιτρέπει την κυκλοφορία RDP, [Δημιουργήστε ένα τελικό σημείο υπηρεσίες Cloud](virtual-machines-windows-classic-setup-endpoints.md). Να επιτρέπεται TCP σε ιδιωτική θύρα 3389.

3. **Διαγνωστικά εκκίνησης Εικονική αναθεώρηση**. Αυτό το βήμα αντιμετώπισης προβλημάτων ελέγχει τα αρχεία καταγραφής κονσόλας Εικονική για να προσδιορίσετε εάν η Εικονική είναι η αναφορά κάποιο πρόβλημα. Δεν έχουν όλοι ΣΠΣ έχουν Διαγνωστικά εκκίνησης ενεργοποιημένο, ώστε αυτό το βήμα αντιμετώπισης προβλημάτων μπορεί να είναι προαιρετικό.
    
    Συγκεκριμένα βήματα αντιμετώπισης προβλημάτων είναι πέρα από το πεδίο αυτού του άρθρου, αλλά μπορεί να υποδεικνύουν ένα ευρύτερο πρόβλημα που επηρεάζει την RDP συνδεσιμότητας. Για περισσότερες πληροφορίες σχετικά με την αναθεώρηση των τα αρχεία καταγραφής από την κονσόλα και Εικονική στιγμιότυπο οθόνης, ανατρέξτε στο θέμα [Διαγνωστικά εκκίνησης για ΣΠΣ](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **Ελέγξτε την εύρυθμη λειτουργία Εικονική πόρων**. Αυτό το βήμα αντιμετώπισης προβλημάτων επαληθεύει υπάρχουν γνωστά θέματα με την πλατφόρμα Azure που ίσως να επηρεάσουν τη δυνατότητα σύνδεσης με την εικονική Μηχανή.

    Επιλέξτε το Εικονική στην πύλη του Azure. Κάντε κύλιση προς τα κάτω παράθυρο ρυθμίσεις στην ενότητα **υποστήριξης + αντιμετώπιση προβλημάτων** κοντά στο κάτω μέρος της λίστας. Κάντε κλικ στο κουμπί **Εύρυθμη λειτουργία των πόρων** . Μια καλή Εικονική αναφορές είναι **διαθέσιμες**:

    ![Έλεγχος εύρυθμης λειτουργίας πόρων Εικονική στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-check-resource-health.png)

5. **Επαναφορά διαπιστευτήρια χρήστη**. Αυτό το βήμα αντιμετώπισης προβλημάτων επαναφέρει τον κωδικό πρόσβασης του λογαριασμού τοπικού διαχειριστή που καθορίζετε όταν δεν είστε βέβαιοι για ή έχετε ξεχάσει τα διαπιστευτήρια.

    Επιλέξτε το Εικονική στην πύλη του Azure. Κάντε κύλιση προς τα κάτω παράθυρο ρυθμίσεις στην ενότητα **υποστήριξης + αντιμετώπιση προβλημάτων** κοντά στο κάτω μέρος της λίστας. Κάντε κλικ στο κουμπί **Επαναφορά κωδικού πρόσβασης** . Πληκτρολογήστε το όνομα χρήστη και έναν νέο κωδικό πρόσβασης. Τέλος, κάντε κλικ στο κουμπί **Αποθήκευση** :

    ![Επαναφέρετε τα διαπιστευτήρια του χρήστη στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-password.png)

6. **Επανεκκινήστε το Εικονική**. Αυτό το βήμα αντιμετώπισης προβλημάτων μπορεί να διορθώσει υποκείμενο προβλήματα αντιμετωπίζει τα Εικονική ίδια.

    Επιλέξτε το Εικονική στην πύλη του Azure και κάντε κλικ στην καρτέλα **Επισκόπηση** . Κάντε κλικ στο κουμπί **επανεκκίνηση** :

    ![Επανεκκινήστε την εικονική Μηχανή στην πύλη του Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-restart-vm.png)
    
Εάν εξακολουθείτε να αντιμετωπίζετε προβλήματα RDP, μπορείτε να [ανοίξετε μια αίτηση υποστήριξης](https://azure.microsoft.com/support/options/) ή να διαβάσετε [πιο λεπτομερείς RDP αντιμετώπισης προβλημάτων έννοιες και τα βήματα](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-specific-rdp-errors"></a>Αντιμετώπιση προβλημάτων συγκεκριμένα σφάλματα RDP
Που μπορεί να αντιμετωπίσετε ένα συγκεκριμένο μήνυμα σφάλματος όταν προσπαθείτε να συνδεθείτε με το Εικονική μέσω RDP. Οι παρακάτω είναι τα πιο συνηθισμένα μηνύματα σφάλματος:

- [Η απομακρυσμένη περίοδος λειτουργίας διακόπηκε επειδή υπάρχουν χωρίς απομακρυσμένης επιφάνειας εργασίας διαθέσιμοι διακομιστές άδειας χρήσης για την παροχή μια άδεια χρήσης](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdplicense).
- [Σύνδεση απομακρυσμένης επιφάνειας εργασίας δεν μπορείτε να βρείτε τον υπολογιστή "όνομα"](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpname).
- Παρουσιάστηκε σφάλμα [έναν έλεγχο ταυτότητας. Δεν είναι δυνατή η επικοινωνία με την τοπική αρχή ασφαλείας](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpauth).
- [Σφάλμα ασφαλείας των Windows: τα διαπιστευτήρια δεν λειτούργησε](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#wincred).
- [Αυτός ο υπολογιστής δεν μπορεί να συνδεθεί στον απομακρυσμένο υπολογιστή](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpconnect).


## <a name="additional-resources"></a>Πρόσθετοι πόροι
Εάν κανένα από αυτά τα σφάλματα Παρουσιάστηκε και συνεχίζετε δεν είναι δυνατό να συνδεθείτε με το Εικονική μέσω απομακρυσμένης επιφάνειας εργασίας, διαβάστε τη λεπτομερή [Αντιμετώπιση προβλημάτων Οδηγός για σύνδεση απομακρυσμένης επιφάνειας εργασίας](virtual-machines-windows-detailed-troubleshoot-rdp.md).

- [Azure πακέτο διαγνωστικών IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Για βήματα αντιμετώπισης προβλημάτων στις πρόσβαση σε εφαρμογές που εκτελούνται σε μια Εικονική, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων πρόσβασης σε μια εφαρμογή που εκτελείται σε μια Εικονική Azure](virtual-machines-linux-troubleshoot-app-connection.md).
- Εάν αντιμετωπίζετε προβλήματα όταν χρησιμοποιείτε ασφαλούς κελύφους (SSH) για να συνδεθείτε με μια Εικονική Linux στα Azure, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων SSH συνδέσεις μια Εικονική Linux στα Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).