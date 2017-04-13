<properties
   pageTitle="Πώς μπορείτε να διαχειριστείτε λίστες ελέγχου πρόσβασης (ACL) για τα τελικά σημεία με χρήση του PowerShell"
   description="Μάθετε πώς να διαχειρίζεστε ACL με το PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-manage-access-control-lists-acls-for-endpoints-by-using-powershell"></a>Πώς μπορείτε να διαχειριστείτε λίστες ελέγχου πρόσβασης (ACL) για τα τελικά σημεία με χρήση του PowerShell

Μπορείτε να δημιουργήσετε και να διαχειριστείτε το δίκτυό λίστες ελέγχου πρόσβασης (ACL) για τα τελικά σημεία με χρήση του Azure PowerShell ή στην πύλη διαχείρισης. Σε αυτό το θέμα, θα βρείτε διαδικασίες για ACL συνήθεις εργασίες που μπορείτε να ολοκληρώσετε τη χρήση του PowerShell. Για τη λίστα των cmdlet του Azure PowerShell ανατρέξτε στο άρθρο [Cmdlet διαχείρισης Azure](http://go.microsoft.com/fwlink/?LinkId=317721). Για περισσότερες πληροφορίες σχετικά με τις ACL, ανατρέξτε στο θέμα [Τι είναι μια λίστα ελέγχου πρόσβασης (ACL) δικτύου;](virtual-networks-acl.md). Εάν θέλετε να διαχειριστείτε ACL σας χρησιμοποιώντας την πύλη διαχείρισης, δείτε [πώς μπορείτε να ορίσετε του τελικά σημεία σε μια εικονική μηχανή](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Διαχείριση ACL δικτύου με χρήση του Azure PowerShell

Μπορείτε να χρησιμοποιήσετε το cmdlet του Azure PowerShell για δημιουργία, κατάργηση και ρύθμιση παραμέτρων (έχει οριστεί) δικτύου λίστες ελέγχου πρόσβασης (ACL). Θα σας έχετε συμπεριλάβει μερικά παραδείγματα ορισμένων από τους τρόπους που μπορείτε να ρυθμίσετε μια ACL με χρήση του PowerShell.

Για να ανακτήσετε μια πλήρη λίστα των τα cmdlet του ACL PowerShell, μπορείτε να χρησιμοποιήσετε ένα από τα εξής:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Δημιουργία μιας ACL δικτύου με κανόνες που επιτρέπουν την πρόσβαση από ένα απομακρυσμένο υποδίκτυο

Το παρακάτω παράδειγμα ένας τρόπος για να δημιουργήσετε μια νέα ACL που περιέχει κανόνες. Σε αυτό το ACL, στη συνέχεια, εφαρμόζεται σε ένα τελικό σημείο εικονική μηχανή. Οι κανόνες ACL στο παρακάτω παράδειγμα θα επιτρέπεται η πρόσβαση από ένα απομακρυσμένο υποδίκτυο. Για να δημιουργήσετε μια νέα ACL δικτύου με άδεια κανόνες για ένα απομακρυσμένο υποδίκτυο, ανοίξτε μια PowerShell ISE Azure. Αντιγράψτε και επικολλήστε τη δέσμη ενεργειών παρακάτω, τη ρύθμιση των παραμέτρων της δέσμης ενεργειών με το δικό σας τιμές και, στη συνέχεια, εκτελέστε τη δέσμη ενεργειών.

1. Δημιουργία νέου αντικειμένου ACL δικτύου.

        $acl1 = New-AzureAclConfig

1. Ορίστε έναν κανόνα που επιτρέπει την πρόσβαση από ένα απομακρυσμένο υποδίκτυο. Στο παρακάτω παράδειγμα, μπορείτε να ορίσετε τον κανόνα *100* (το οποίο έχει προτεραιότητα στην κανόνα 200 και νεότερη έκδοση) για να επιτρέψετε υποδικτύου απομακρυσμένη πρόσβαση *10.0.0.0/8* το τελικό σημείο εικονική μηχανή. Αντικαταστήστε τις τιμές με τις δικές σας απαιτήσεις ρύθμισης παραμέτρων. Το όνομα "Ρύθμισης παραμέτρων του SharePoint ACL" θα πρέπει να αντικατασταθεί με το φιλικό όνομα που θέλετε να καλέσετε αυτόν τον κανόνα.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. Για επιπλέον κανόνες, επαναλάβετε το cmdlet, αντικαθιστώντας τις τιμές με τις δικές σας απαιτήσεις ρύθμισης παραμέτρων. Φροντίστε να αλλάξετε τον κανόνα για να απεικονίσει τη σειρά με την οποία θέλετε να τους κανόνες για να εφαρμοστεί ο αριθμός. Το μικρότερο αριθμό κανόνα προηγείται το μεγαλύτερο αριθμό.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. Στη συνέχεια, μπορείτε να δημιουργήσετε ένα νέο τελικό σημείο (Προσθήκη) ή να ορίσετε το ACL για ένα υπάρχον τελικό σημείο (έχει οριστεί). Σε αυτό το παράδειγμα, θα προσθέσετε ένα νέο τελικό σημείο εικονικό μηχάνημα που ονομάζεται "web" και ενημερώστε το τελικό σημείο εικονική μηχανή με τις ρυθμίσεις ACL.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. Στη συνέχεια, Συνδυάστε τα cmdlet και εκτελέστε τη δέσμη ενεργειών. Για αυτό το παράδειγμα, τα συνδυασμένα cmdlet θα μοιάζει κάπως έτσι:

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Κατάργηση ενός κανόνα ACL δικτύου που επιτρέπει την πρόσβαση από ένα απομακρυσμένο υποδίκτυο

Το παρακάτω παράδειγμα ένας τρόπος για να καταργήσετε έναν κανόνα ACL δικτύου.  Για να καταργήσετε έναν κανόνα ACL δικτύου με άδεια κανόνες για ένα απομακρυσμένο υποδίκτυο, ανοίξτε μια PowerShell ISE Azure. Αντιγράψτε και επικολλήστε τη δέσμη ενεργειών παρακάτω, τη ρύθμιση των παραμέτρων της δέσμης ενεργειών με το δικό σας τιμές και, στη συνέχεια, εκτελέστε τη δέσμη ενεργειών.

1. Πρώτο βήμα είναι να λάβετε το αντικείμενο ACL δικτύου για το τελικό σημείο εικονική μηχανή. Μπορείτε, στη συνέχεια, θα καταργήσετε τον κανόνα ACL. Σε αυτήν την περίπτωση, θα σας κατάργηση του κατά αναγνωριστικό κανόνα. Αυτό θα καταργήσει μόνο ο κανόνας ID 0 από την ACL. Αυτό δεν καταργεί το αντικείμενο ACL από το τελικό σημείο εικονική μηχανή.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. Στη συνέχεια, πρέπει να εφαρμόσετε το αντικείμενο ACL δικτύου για το τελικό σημείο εικονική μηχανή και ενημερώστε την εικονική μηχανή.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Κατάργηση μιας ACL δικτύου από ένα τελικό σημείο εικονική μηχανή

Σε ορισμένα σενάρια, ενδέχεται να θέλετε να καταργήσετε ένα αντικείμενο ACL δικτύου από ένα τελικό σημείο εικονική μηχανή. Για να το κάνετε αυτό, ανοίξτε μια PowerShell ISE Azure. Αντιγράψτε και επικολλήστε τη δέσμη ενεργειών παρακάτω, τη ρύθμιση των παραμέτρων της δέσμης ενεργειών με το δικό σας τιμές και, στη συνέχεια, εκτελέστε τη δέσμη ενεργειών.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Επόμενα βήματα

[Τι είναι μια λίστα ελέγχου πρόσβασης (ACL) δικτύου;](virtual-networks-acl.md)
