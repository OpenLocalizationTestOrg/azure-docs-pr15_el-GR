<properties 
   pageTitle="Πώς μπορείτε να μετακινήσετε μια παρουσία Εικονική ή ρόλο σε ένα διαφορετικό υποδίκτυο"
   description="Μάθετε πώς μπορείτε να μετακινήσετε ΣΠΣ και παρουσίες ρόλο σε διαφορετικό υποδίκτυο"
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
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Πώς μπορείτε να μετακινήσετε μια παρουσία Εικονική ή ρόλο σε ένα διαφορετικό υποδίκτυο

Μπορείτε να χρησιμοποιήσετε το PowerShell για να μετακινήσετε το ΣΠΣ από μία υποδικτύου σε ένα άλλο στο ίδιο δίκτυο εικονικού (VNet). Ρόλος παρουσίες μπορούν να μετακινηθούν από την επεξεργασία του CSCFG, αντί για χρήση του PowerShell.

>[AZURE.NOTE] Σε αυτό το άρθρο περιέχει πληροφορίες που είναι σχετική με Azure κλασική αναπτύξεις μόνο.

Γιατί μετακινήσω ΣΠΣ υποδίκτυο άλλο; Μετεγκατάσταση υποδικτύου είναι χρήσιμη όταν το παλαιότερο υποδίκτυο είναι πολύ μικρό και δεν είναι δυνατό να αναπτυχθούν λόγω υπάρχοντα ΣΠΣ εκτελείται σε αυτό το δευτερεύον δίκτυο. Σε αυτή την περίπτωση, μπορείτε να δημιουργήσετε ένα νέο, μεγαλύτερο υποδίκτυο και να μετεγκαταστήσετε του ΣΠΣ στο νέο υποδίκτυο, στη συνέχεια, μετά την ολοκλήρωση της μετεγκατάστασης, μπορείτε να διαγράψετε το παλιό υποδίκτυο κενό.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Πώς μπορείτε να μετακινήσετε μια Εικονική άλλο υποδίκτυο

Για να μετακινήσετε μια Εικονική, εκτελέστε το cmdlet Set-AzureSubnet PowerShell, χρησιμοποιώντας το παρακάτω παράδειγμα ως πρότυπο. Στο παρακάτω παράδειγμα, θα σας να μετακινήσετε TestVM από την παρουσίαση υποδίκτυο, υποδίκτυο-2. Φροντίστε να επεξεργαστείτε το παράδειγμα ώστε να αντικατοπτρίζει το περιβάλλον σας. Σημειώστε ότι κάθε φορά που εκτελείτε το cmdlet ενημέρωση AzureVM ως τμήμα μιας διαδικασίας, θα γίνει επανεκκίνηση του Εικονική ως μέρος της διαδικασίας ενημέρωσης.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

Αν έχετε καθορίσει μια στατική εσωτερική ιδιωτική IP για το Εικονική, έχετε για να καταργήσετε αυτήν τη ρύθμιση, πριν να μετακινήσετε την εικονική Μηχανή με ένα νέο δευτερεύον. Σε αυτή την περίπτωση, χρησιμοποιήστε τα εξής:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Για να μετακινήσετε μια παρουσία ρόλο σε μια άλλη υποδίκτυο

Για να μετακινήσετε μια παρουσία ρόλο, επεξεργαστείτε το αρχείο CSCFG. Στο παρακάτω παράδειγμα, θα σας να μετακινήσετε "Role0" στο εικονικού δικτύου *VNETName* από την παρουσίαση υποδίκτυο *υποδικτύου-2*. Επειδή η παρουσία ρόλο ήδη έχει αναπτυχθεί, θα μπορείτε να αλλάξετε μόνο το όνομα υποδικτύου = υποδικτύου-2. Φροντίστε να επεξεργαστείτε το παράδειγμα ώστε να αντικατοπτρίζει το περιβάλλον σας.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
