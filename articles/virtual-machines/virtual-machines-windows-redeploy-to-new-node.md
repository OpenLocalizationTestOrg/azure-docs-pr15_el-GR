<properties 
    pageTitle="Αναπτύξτε ξανά εικονικές μηχανές Windows | Microsoft Azure" 
    description="Περιγράφει τον τρόπο για να αναπτύξετε εκ νέου εικονικές μηχανές Windows να συμβάλει στην αντιμετώπιση προβλημάτων σύνδεσης RDP." 
    services="virtual-machines-windows" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-windows" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>


# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Αναπτύξτε ξανά εικονική μηχανή νέο κόμβο Azure

Εάν έχετε αντιμετωπίζουν δυσκολίες αντιμετώπιση προβλημάτων απομακρυσμένης επιφάνειας εργασίας (RDP) σύνδεσης ή εφαρμογή πρόσβαση βασίζεται σε Windows Azure εικονική μηχανή (Εικονική), η επανάληψη ανάπτυξης η Εικονική μπορεί να σας βοηθήσουν. Όταν αναπτύξετε εκ νέου μια Εικονική, μετακινείται η Εικονική σε έναν νέο κόμβο εντός της υποδομής Azure και, στη συνέχεια, να το προσφέρει επιστρέψτε στην, διατηρώντας όλες τις επιλογές ρύθμισης παραμέτρων και τους συναφείς πόρους. Αυτό το άρθρο σας δείχνει πώς μπορείτε να αναπτύξετε εκ νέου μια Εικονική με τη χρήση του PowerShell Azure ή την πύλη του Azure.

> [AZURE.NOTE] Αφού αναπτύξετε εκ νέου μια Εικονική, χάνεται το προσωρινό δίσκο και ενημερώνονται δυναμικές διευθύνσεις IP που σχετίζεται με το περιβάλλον εργασίας εικονικού δικτύου. 

## <a name="using-azure-powershell"></a>Χρήση του Azure PowerShell

Βεβαιωθείτε ότι έχετε την πιο πρόσφατη Azure PowerShell 1.x εγκατεστημένο στον υπολογιστή σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

Χρησιμοποιήστε αυτή την εντολή Azure PowerShell για να αναπτύξετε εκ νέου την εικονική μηχανή σας:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Επόμενα βήματα
Αν αντιμετωπίζετε θέματα σύνδεσης με Εικονική σας, μπορείτε να βρείτε συγκεκριμένες βοήθεια στην [Αντιμετώπιση προβλημάτων συνδέσεις RDP](virtual-machines-windows-troubleshoot-rdp-connection.md) ή [λεπτομερείς RDP βήματα αντιμετώπισης προβλημάτων](virtual-machines-windows-detailed-troubleshoot-rdp.md). Εάν δεν μπορείτε να αποκτήσετε πρόσβαση σε μια εφαρμογή που εκτελείται σε Εικονική σας, μπορείτε επίσης να διαβάσετε [Αντιμετώπιση προβλημάτων εφαρμογής](virtual-machines-windows-troubleshoot-app-connection.md).