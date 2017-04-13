<properties 
   pageTitle="Πώς μπορείτε να ορίσετε μια στατική εσωτερική ιδιωτική IP"
   description="Κατανόηση των στατική εσωτερικές διευθύνσεις IP (πτώσεις) και πώς μπορείτε να διαχειριστείτε τους"
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

# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Πώς μπορείτε να ορίσετε μια στατική εσωτερική ιδιωτική διεύθυνση IP χρησιμοποιώντας το PowerShell (κλασικό)
Στις περισσότερες περιπτώσεις, δεν θα χρειαστεί να καθορίσετε μια εσωτερική στατική διεύθυνση IP για την εικονική μηχανή σας. ΣΠΣ σε δίκτυο εικονικού θα λάβετε αυτόματα μια εσωτερική διεύθυνση IP από μια περιοχή που καθορίζετε. Αλλά, σε ορισμένες περιπτώσεις, καθορίζοντας μια στατική διεύθυνση IP για μια συγκεκριμένη Εικονική έχει νόημα. Για παράδειγμα, εάν σας Εικονική εκτελέσουμε DNS ή θα ελεγκτή τομέα. Μια στατική διεύθυνση IP εσωτερικού παραμένει η Εικονική ακόμα και μέσω ενός μέλους διακοπή/διαχειριστείτε. 

> [AZURE.IMPORTANT] Azure έχει δύο διαφορετικές ανάπτυξης μοντέλα για τη δημιουργία και εργασία με πόρους: [Διαχείριση πόρων και κλασική](../resource-manager-deployment-model.md). Σε αυτό το άρθρο καλύπτει χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης. Η Microsoft συνιστά περισσότερες νέες αναπτύξεις χρησιμοποιείτε το [μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-networks-static-private-ip-arm-ps.md).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Πώς μπορείτε να επαληθεύσετε εάν είναι διαθέσιμη μια συγκεκριμένη διεύθυνση IP
Για να επαληθεύσετε εάν η διεύθυνση IP *10.0.0.7* είναι διαθέσιμη σε μια vnet με το όνομα *TestVnet*, εκτελέστε την ακόλουθη εντολή PowerShell και να επαληθεύσετε την τιμή για το *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

>[AZURE.NOTE] Εάν θέλετε να δοκιμάσετε την εντολή επάνω σε ένα περιβάλλον ασφαλή, ακολουθήστε τις οδηγίες στο θέμα [Δημιουργία ένα εικονικό δίκτυο](virtual-networks-create-vnet-classic-portal.md) για να δημιουργήσετε ένα vnet *TestVnet* με το όνομα και βεβαιωθείτε ότι χρησιμοποιεί το χώρο διευθύνσεων *10.0.0.0/8* .

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Πώς μπορείτε να καθορίσετε μια στατική εσωτερική IP κατά τη δημιουργία μια εικονική Μηχανή
Την παρακάτω δέσμη ενεργειών PowerShell δημιουργεί μια νέα υπηρεσία στο cloud με το όνομα *TestService*, στη συνέχεια, ανακτά μια εικόνα από το Azure, στη συνέχεια, δημιουργεί μια Εικονική με το όνομα *TestVM* στην νέα υπηρεσία cloud χρησιμοποιώντας τα ανακτημένα εικόνα, ορίζει την εικονική Μηχανή να είναι σε ένα δευτερεύον δίκτυο με το όνομα *υποδικτύου-1*και συνόλων *10.0.0.7* ως στατική εσωτερική IP για την εικονική Μηχανή:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzureSubnet –SubnetNames Subnet-1 `
  	| Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
  	| New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Πώς μπορείτε να ανακτήσετε στατική εσωτερικές πληροφορίες διευθύνσεων IP για μια Εικονική
Για να προβάλετε το εσωτερικό πληροφορίες στατικής IP για την εικονική Μηχανή που δημιουργήθηκαν με τη δέσμη ενεργειών παραπάνω, εκτελέστε την ακόλουθη εντολή PowerShell και παρατηρήστε τις τιμές για τη *διεύθυνση IP*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Πώς να καταργήσετε μια στατική εσωτερική IP από μια εικονική Μηχανή
Για να καταργήσετε τη στατική εσωτερική IP προστεθεί η Εικονική στα παραπάνω τη δέσμη ενεργειών, εκτελέστε την ακόλουθη εντολή PowerShell:
    
    Get-AzureVM -ServiceName TestService -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Πώς να προσθέσετε μια στατική εσωτερική IP σε υπάρχουσα Εικονική μηχανή
Για να προσθέσετε μια εσωτερική στατική IP για την εικονική Μηχανή δημιουργήθηκε με τη δέσμη ενεργειών παραπάνω, σφάλμα χρόνου εκτέλεσης ακόλουθο εντολή:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
  	| Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
  	| Update-AzureVM

## <a name="next-steps"></a>Επόμενα βήματα

[Δεσμευμένη διεύθυνση IP](virtual-networks-reserved-public-ip.md)

[Εμφάνιση επιπέδου δημόσια IP (ILPIP)](virtual-networks-instance-level-public-ip.md)

[APIs ΥΠΌΛΟΙΠΑ δεσμευμένη διεύθυνση IP](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 
