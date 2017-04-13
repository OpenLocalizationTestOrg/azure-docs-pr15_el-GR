<properties 
   pageTitle="Παράδειγμα επίπεδο δημόσια IP (ILPIP) | Microsoft Azure"
   description="Κατανόηση των ILPIP (PIP) και πώς μπορείτε να διαχειριστείτε τους"
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
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="instance-level-public-ip-overview"></a>Εμφάνιση επιπέδου δημόσια IP Επισκόπηση
Μια παρουσία επιπέδου δημόσια IP (ILPIP) είναι μια δημόσια διεύθυνση IP που μπορείτε να αντιστοιχίσετε απευθείας στην περίοδο λειτουργίας του ρόλο ή Εικονική, αντί για την υπηρεσία cloud που σας παρουσία Εικονική ή ρόλο που βρίσκονται σε. Αυτό δεν λαμβάνουν τόπου VIP (εικονικό IP) που έχει αντιστοιχιστεί σε υπηρεσία cloud. Αντίθετα, είναι μια πρόσθετη διεύθυνση IP που μπορείτε να χρησιμοποιήσετε για να συνδεθείτε απευθείας με την παρουσία σας Εικονική ή ρόλο.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας το μοντέλο από διαχειριστή πόρων](virtual-network-ip-addresses-overview-arm.md). 

Βεβαιωθείτε ότι έχετε κατανοήσει πώς λειτουργούν οι [διευθύνσεις IP](virtual-network-ip-addresses-overview-classic.md) στο Azure.

>[AZURE.NOTE] Στο παρελθόν, μια ILPIP ήταν αναφέρονται ως ένα PIP, που σημαίνει για δημόσια διεύθυνση IP. 

![Διαφορά μεταξύ ILPIP και VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Όπως φαίνεται στην εικόνα 1, την υπηρεσία cloud γίνεται με χρήση ενός VIP, ενώ τα μεμονωμένα ΣΠΣ κανονικά είναι προσβάσιμες χρησιμοποιώντας VIP:&lt;αριθμός θύρας&gt;. Με την εκχώρηση μιας ILPIP σε μια συγκεκριμένη Εικονική ότι Εικονική είναι δυνατή η πρόσβαση απευθείας χρησιμοποιώντας αυτήν τη διεύθυνση IP.

Όταν δημιουργείτε μια υπηρεσία cloud στο Azure, αντίστοιχες εγγραφές DNS A δημιουργούνται αυτόματα για να επιτρέψετε την πρόσβαση στην υπηρεσία μέσω ένα πλήρως προσδιορισμένο όνομα τομέα (FQDN) αντί να χρησιμοποιήσετε την πραγματική VIP. Συμβαίνει την ίδια διαδικασία για ILPIP, επιτρέπετε την πρόσβαση στην Εικονική ή ρόλο παρουσία κάνοντας FQDN αντί για το ILPIP. Για παράδειγμα, εάν δημιουργείτε μια υπηρεσία στο cloud με το όνομα *contosoadservice*και ρύθμιση παραμέτρων του ρόλου web με το όνομα *contosoweb* με δύο παρουσίες, Azure θα καταχωρήσετε τα ακόλουθα A εγγραφές για τις παρουσίες:

- contosoweb\_IN_0.contosoadservice.cloudapp.net
- contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] Μπορείτε να αντιστοιχίσετε μόνο ένα ILPIP για κάθε παρουσία Εικονική ή ρόλο. Μπορείτε να χρησιμοποιήσετε έως και 5 ILPIP του ανά συνδρομή. Προς το παρόν, ILPIP δεν υποστηρίζεται για πολλούς NIC ΣΠΣ.

## <a name="why-should-i-request-an-ilpip"></a>Γιατί θα πρέπει να ζητήσω μια ILPIP;
Εάν θέλετε να είναι σε θέση να συνδεθείτε με το ρόλο ή Εικονική παρουσία, μια διεύθυνση IP που έχουν εκχωρηθεί απευθείας σε αυτήν, αντί να χρησιμοποιήσετε το cloud υπηρεσίας VIP:&lt;αριθμός θύρας&gt;, ζητάτε ILPIP μια Εικονική σας ή την παρουσία ρόλο.
- **Παθητικό FTP** - από χρειάζεται μια ILPIP στην Εικονική σας, μπορείτε να λάβετε την κυκλοφορία σε οποιαδήποτε θύρα, δεν θα χρειαστεί για να ανοίξετε ένα τελικό σημείο για να λάβετε την κυκλοφορία. Αυτή η δυνατότητα επιτρέπει σενάρια όπως παθητικό FTP όπου τις θύρες επιλέγονται δυναμικά.
- **Εξερχομένων IP** - εξερχόμενη κυκλοφορία που προέρχονται από την εικονική Μηχανή σβήνει με το ILPIP ως προέλευση και αυτή η Εικονική εξωτερικών πρόσωπα που προσδιορίζει με μοναδικό τρόπο.

## <a name="how-to-request-an-ilpip-during-vm-creation"></a>Πώς μπορείτε να ζητήσετε μια ILPIP κατά τη δημιουργία Εικονική
Την παρακάτω δέσμη ενεργειών PowerShell δημιουργεί μια νέα υπηρεσία στο cloud με το όνομα *FTPService*, στη συνέχεια, ανακτά μια εικόνα από το Azure, και δημιουργεί μια Εικονική με το όνομα *FTPInstance* χρησιμοποιώντας τα ανακτημένα εικόνα, ορίζει την εικονική Μηχανή για να χρησιμοποιήσετε ένα ILPIP και προσθέτει την εικονική Μηχανή για τη νέα υπηρεσία:

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Πώς μπορείτε να ανακτήσετε πληροφορίες ILPIP για μια εικονική Μηχανή
Για να προβάλετε τις πληροφορίες ILPIP για την εικονική Μηχανή που δημιουργήθηκαν με τη δέσμη ενεργειών παραπάνω, εκτελέστε την ακόλουθη εντολή PowerShell και παρατηρήστε τις τιμές για *PublicIPAddress* και *PublicIPName*:

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## <a name="how-to-remove-an-ilpip-from-a-vm"></a>Πώς να καταργήσετε μια ILPIP από μια εικονική Μηχανή
Για να καταργήσετε το ILPIP προστεθεί η Εικονική στα παραπάνω τη δέσμη ενεργειών, εκτελέστε την ακόλουθη εντολή PowerShell:
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Πώς να προσθέσετε ένα ILPIP σε υπάρχουσα Εικονική μηχανή
Για να προσθέσετε μια ILPIP η Εικονική που δημιουργήθηκε με τη δέσμη ενεργειών παραπάνω, εκτελέστε την ακόλουθη εντολή:

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## <a name="how-to-associate-an-ilpip-to-a-vm-by-using-a-service-configuration-file"></a>Πώς μπορείτε να συσχετίσετε ένα ILPIP σε μια Εικονική, χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων υπηρεσίας
Μπορείτε επίσης να συσχετίσετε ένα ILPIP σε μια Εικονική χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων (CSCFG) υπηρεσίας. Το παρακάτω δείγμα xml δείχνει πώς μπορείτε να ρυθμίσετε μια υπηρεσία cloud για να χρησιμοποιήσετε μια ILPIP ονομάζεται *MyPublicIP* για μια παρουσία ρόλου: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Επόμενα βήματα

- Κατανόηση του τρόπου [διευθύνσεις IP](virtual-network-ip-addresses-overview-classic.md) λειτουργίας στο μοντέλο κλασική ανάπτυξης.

- Μάθετε περισσότερα σχετικά με [δεσμευμένες διευθύνσεις IP](virtual-networks-reserved-public-ip.md).
 