<properties 
   pageTitle="Πώς μπορείτε να ορίσετε μια στατική ιδιωτικό IP σε κλασική λειτουργία χρησιμοποιώντας το PowerShell | Microsoft Azure"
   description="Κατανόηση των στατική ιδιωτικό διευθύνσεις IP (πτώσεις) και πώς μπορείτε να διαχειριστείτε τους σε κλασική λειτουργία και PowerShell"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-powershell"></a>Πώς μπορείτε να ορίσετε μια στατική διεύθυνση IP ιδιωτικό (κλασική) στο PowerShell

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο κλασική ανάπτυξης. Μπορείτε επίσης να [διαχειριστείτε μια στατική διεύθυνση IP ιδιωτικό στο μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-networks-static-private-ip-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Το δείγμα τις εντολές του PowerShell παρακάτω θεωρείτε ότι ένα απλό περιβάλλον που έχετε ήδη δημιουργήσει. Εάν θέλετε να εκτελέσετε τις εντολές που εμφανίζονται σε αυτό το έγγραφο, δημιουργήστε πρώτα το περιβάλλον δοκιμής που περιγράφονται στο θέμα [Δημιουργία μιας VNet](virtual-networks-create-vnet-classic-netcfg-ps.md).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Πώς μπορείτε να επαληθεύσετε εάν είναι διαθέσιμη μια συγκεκριμένη διεύθυνση IP
Για να επαληθεύσετε εάν η διεύθυνση IP *192.168.1.101* είναι διαθέσιμη σε μια VNet με το όνομα *TestVNet*, εκτελέστε την ακόλουθη εντολή PowerShell και να επαληθεύσετε την τιμή για το *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Αναμενόμενο αποτέλεσμα:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Πώς μπορείτε να καθορίσετε μια στατική διεύθυνση IP ιδιωτικά, κατά τη δημιουργία μια εικονική Μηχανή
Την παρακάτω δέσμη ενεργειών PowerShell δημιουργεί μια νέα υπηρεσία στο cloud με το όνομα *TestService*, στη συνέχεια, ανακτά μια εικόνα από το Azure, δημιουργεί μια Εικονική με το όνομα *DNS01* στην νέα υπηρεσία cloud χρησιμοποιώντας τα ανακτημένα εικόνα, ορίζει την εικονική Μηχανή να βρίσκονται σε ένα δευτερεύον που ονομάζεται *προσκήνιο*και συνόλων *192.168.1.7* ως ιδιωτικό στατική διεύθυνση IP για την εικονική Μηχανή:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Αναμενόμενο αποτέλεσμα:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Πώς μπορείτε να ανακτήσετε ιδιωτικό πληροφορίες στατικής διεύθυνσης IP για μια Εικονική
Για να προβάλετε τα ιδιωτικά πληροφορίες στατικής διεύθυνσης IP για την εικονική Μηχανή που δημιουργήθηκαν με τη δέσμη ενεργειών παραπάνω, εκτελέστε την ακόλουθη εντολή PowerShell και παρατηρήστε τις τιμές για τη *διεύθυνση IP*:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Αναμενόμενο αποτέλεσμα:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
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

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Πώς να καταργήσετε μια στατική διεύθυνση IP ιδιωτικό από μια εικονική Μηχανή
Για να καταργήσετε τη στατική διεύθυνση IP ιδιωτικό προστεθεί η Εικονική στη δέσμη ενεργειών παραπάνω, εκτελέστε την ακόλουθη εντολή PowerShell:
    
    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Αναμενόμενο αποτέλεσμα:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Πώς να προσθέσετε μια στατική διεύθυνση IP ιδιωτικό σε υπάρχουσα Εικονική μηχανή
Για να προσθέσετε μια στατική ιδιωτική διεύθυνση IP για την εικονική Μηχανή που δημιουργήθηκε με τη δέσμη ενεργειών παραπάνω, σφάλμα χρόνου εκτέλεσης ακόλουθο εντολή:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Αναμενόμενο αποτέλεσμα:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε σχετικά με τις διευθύνσεις [δεσμευμένη δημόσια διεύθυνση IP](virtual-networks-reserved-public-ip.md) .
- Μάθετε σχετικά με τις διευθύνσεις [επιπέδου παρουσίας δημόσια IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Συμβουλευθείτε τα [APIs ΥΠΌΛΟΙΠΑ δεσμευμένη διεύθυνση IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).
