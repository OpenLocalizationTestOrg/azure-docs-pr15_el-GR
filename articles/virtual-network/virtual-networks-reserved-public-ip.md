<properties
   pageTitle="Δεσμευμένες IP | Microsoft Azure"
   description="Κατανόηση των δεσμευμένες διευθύνσεις IP και πώς μπορείτε να διαχειριστείτε τους"
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

# <a name="reserved-ip-overview"></a>Επισκόπηση δεσμευμένη διεύθυνση IP
Διευθύνσεις IP στο Azure εμπίπτουν σε δύο κατηγορίες: δυναμικής και το δεσμευμένο. Δημόσιες διευθύνσεις IP που ελέγχονται από Azure είναι δυναμικά από προεπιλογή. Αυτό σημαίνει ότι η διεύθυνση IP χρησιμοποιείται για μια υπηρεσία cloud που δίνεται (VIP) ή για να αποκτήσετε πρόσβαση σε μια Εικονική ή ρόλο παρουσία απευθείας (ILPIP), να αλλάξετε κατά καιρούς, όταν οι πόροι είναι τερματισμού ή κατάργηση εκχώρησης.

Για να αποτρέψετε την αλλαγή διευθύνσεις IP, μπορείτε να δεσμεύσετε μια διεύθυνση IP. Δεσμευμένη διευθύνσεις IP μπορεί να χρησιμοποιηθεί μόνο ως ένα VIP, εξασφαλίζοντας ότι η διεύθυνση IP για την υπηρεσία cloud θα είναι τα ίδια ακόμα και όταν οι πόροι είναι τερματισμού ή κατάργηση εκχώρησης. Επιπλέον, μπορείτε να μετατρέψετε υπάρχουσες δυναμικές διευθύνσεις IP χρησιμοποιείται ως μια VIP σε μια δεσμευμένη διεύθυνση IP.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να δεσμεύσετε μια στατική διεύθυνση IP δημόσια χρησιμοποιώντας το [μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-network-ip-addresses-overview-arm.md).

Βεβαιωθείτε ότι έχετε κατανοήσει πώς λειτουργούν οι [διευθύνσεις IP](virtual-network-ip-addresses-overview-classic.md) στο Azure.

## <a name="when-do-i-need-a-reserved-ip"></a>Πότε χρειάζομαι ένα δεσμευμένο IP;
- **Θέλετε να διασφαλίσετε ότι το IP είναι δεσμευμένο στο τη συνδρομή σας**. Εάν θέλετε να δεσμεύσετε μια διεύθυνση IP που δεν θα κυκλοφορήσει από τη συνδρομή σας σε καμία περίπτωση, πρέπει να χρησιμοποιήσετε μια δεσμευμένη δημόσια διεύθυνση IP.  
- **Θέλετε την IP σας για να παραμείνετε με την υπηρεσία cloud ακόμη και σε κατάσταση διακοπεί ή κατάργηση εκχώρησης (ΣΠΣ)**. Εάν θέλετε να έχει πρόσβαση, χρησιμοποιώντας μια διεύθυνση IP που δεν θα αλλάξει την υπηρεσία, ακόμη και όταν ΣΠΣ στην υπηρεσία cloud είναι διακοπή ή κατάργηση εκχώρησης.
- **Θέλετε να διασφαλίσετε ότι η εξερχόμενη κυκλοφορία από το Azure χρησιμοποιεί μια προβλέψιμα διεύθυνση IP**. Ενδέχεται να έχετε το τείχος προστασίας στην εσωτερική εγκατάσταση σας έχει ρυθμιστεί ώστε να επιτρέψετε την κυκλοφορία μόνο από συγκεκριμένες διευθύνσεις IP. Με δέσμευση μια IP, θα γνωρίζετε τη διεύθυνση IP προέλευσης και δεν θα πρέπει να ενημερώσετε το κανόνες τείχους προστασίας λόγω μια αλλαγή διευθύνσεων IP.

## <a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
1. Μπορώ να χρησιμοποιήσω μια δεσμευμένη διεύθυνση IP για όλες τις υπηρεσίες του Azure;  
  - Δεσμευμένη διευθύνσεις IP μπορεί να χρησιμοποιηθεί μόνο για ΣΠΣ και εκτίθεται μέσω ενός VIP τους ρόλους παρουσία υπηρεσία cloud.
1. Πόσες δεσμευμένες διευθύνσεις IP μπορώ να έχω;  
  - Προς το παρόν, όλες τις συνδρομές του Azure είναι εξουσιοδοτημένοι να χρησιμοποιήσετε 20 δεσμευμένες διευθύνσεις IP. Ωστόσο, μπορείτε να ζητήσετε πρόσθετες δεσμευμένες διευθύνσεις IP. Ανατρέξτε στη σελίδα [συνδρομή και περιορισμοί υπηρεσίας](../azure-subscription-service-limits.md) για περισσότερες πληροφορίες.
1. Υπάρχει μια χρέωση για διευθύνσεις IP του δεσμευμένο;
  - Ανατρέξτε στο θέμα [Δεσμευμένη IP Address τις τιμές λεπτομέρειες](http://go.microsoft.com/fwlink/?LinkID=398482) για τις πληροφορίες τιμολόγησης.
1. Πώς να δεσμεύσετε μια διεύθυνση IP;
  - Μπορείτε να χρησιμοποιήσετε PowerShell ή το [Azure διαχείρισης REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx) για να δεσμεύσετε μια διεύθυνση IP σε μια συγκεκριμένη περιοχή. Αυτήν τη δεσμευμένη διεύθυνση IP έχει συσχετιστεί με τη συνδρομή σας. Δεν μπορείτε να δεσμεύσετε μια διεύθυνση IP, χρησιμοποιώντας την πύλη διαχείρισης.
1. Μπορώ να χρησιμοποιήσω αυτό με ομάδα συσχέτισης βάσει VNets;
  - Διευθύνσεις IP δεσμευμένες υποστηρίζονται μόνο σε τοπικές VNets. Δεν υποστηρίζεται για VNets που συσχετίζονται με τις ομάδες συνάφεια. Για περισσότερες πληροφορίες σχετικά με τη συσχέτιση μιας VNet με μια περιοχή ή μια ομάδα συσχέτισης, ανατρέξτε στο θέμα [σχετικά με τις τοπικές VNets και ομάδες συνάφεια](virtual-networks-migrate-to-regional-vnet.md).

## <a name="how-to-manage-reserved-vips"></a>Πώς μπορείτε να διαχειριστείτε δεσμευμένη VIPs

Μπορείτε να χρησιμοποιήσετε δεσμευμένες διευθύνσεις IP, που πρέπει να το προσθέσετε στη συνδρομή σας. Για να δημιουργήσετε ένα δεσμευμένο IP από το χώρο συγκέντρωσης δημόσιων διευθύνσεων IP στη θέση *Κεντρικές ΗΠΑ* , εκτελέστε την ακόλουθη εντολή PowerShell:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

Παρατηρήστε, ωστόσο, ότι δεν μπορείτε να καθορίσετε τι γίνεται IP δεσμευμένες. Για να δείτε ποιες διευθύνσεις IP είναι δεσμευμένες στη συνδρομή σας, εκτελέστε την ακόλουθη εντολή PowerShell και παρατηρήστε τις τιμές για *ReservedIPName* και *διεύθυνση*:

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

Όταν δεσμευτεί ένα IP, παραμένει συσχετιστεί με τη συνδρομή σας μέχρι να το διαγράψετε. Για να διαγράψετε τη δεσμευμένη διεύθυνση IP που εμφανίζεται παραπάνω, εκτελέστε την ακόλουθη εντολή PowerShell:

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## <a name="how-to-reserve-the-ip-address-of-an-existing-cloud-service"></a>Πώς μπορείτε να δεσμεύσετε τη διεύθυνση IP του μια υπάρχουσα υπηρεσία cloud

Μπορείτε να δεσμεύσετε τη διεύθυνση IP του μια υπάρχουσα υπηρεσία cloud, προσθέτοντας την παράμετρο *- Όνομα_υπηρεσίας* . Για να δεσμεύσετε τη διεύθυνση IP του μια υπηρεσία cloud *TestService* στη θέση *Κεντρικές ΗΠΑ* , εκτελέστε την ακόλουθη εντολή PowerShell:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService


## <a name="how-to-associate-a-reserved-ip-to-a-new-cloud-service"></a>Πώς μπορείτε να συσχετίσετε μια δεσμευμένη διεύθυνση IP σε μια νέα υπηρεσία cloud
Η παρακάτω δέσμη ενεργειών δημιουργεί μια νέα δεσμευμένη διεύθυνση IP και, στη συνέχεια, αντιστοιχεί σε μια νέα υπηρεσία cloud με το όνομα *TestService*.

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] Όταν δημιουργείτε μια δεσμευμένη διεύθυνση IP για χρήση με μια υπηρεσία cloud, θα εξακολουθείτε να χρειάζεστε για να ανατρέξετε στις την εικονική Μηχανή χρησιμοποιώντας *VIP:&lt;αριθμός θύρας >* για την εισερχόμενη επικοινωνία. Δέσμευση μια IP δεν σημαίνει μπορείτε να συνδεθείτε με την εικονική Μηχανή απευθείας. Τη δεσμευμένη διεύθυνση IP έχει ανατεθεί στην υπηρεσία cloud που έχει αναπτυχθεί το Εικονική. Εάν θέλετε να συνδεθείτε με μια Εικονική από IP απευθείας, πρέπει να ρυθμίσετε τις παραμέτρους ενός επιπέδου παρουσίας δημόσια IP. Μια δημόσια IP επιπέδου παρουσίας είναι ένας τύπος δημόσια IP (ονομάζεται μια ILPIP) που έχει αντιστοιχιστεί απευθείας σε Εικονική σας. Δεν είναι δυνατό να δεσμευτεί. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επιπέδου παρουσίας δημόσια IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .

## <a name="how-to-remove-a-reserved-ip-from-a-running-deployment"></a>Πώς να καταργήσετε μια δεσμευμένη διεύθυνση IP από μια ενσωματωμένη ανάπτυξη
Για να καταργήσετε τη δεσμευμένη διεύθυνση IP που προσθέσατε για τη νέα υπηρεσία που έχουν δημιουργηθεί με τη δέσμη ενεργειών παραπάνω, εκτελέστε την ακόλουθη εντολή PowerShell:

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] Κατάργηση ενός δεσμευμένη διεύθυνση IP από μια ενσωματωμένη ανάπτυξη δεν καταργεί τη δέσμευση από τη συνδρομή σας. Το ελευθερώνει απλώς το IP που θα χρησιμοποιηθεί από άλλον πόρο στη συνδρομή σας.

## <a name="how-to-associate-a-reserved-ip-to-a-running-deployment"></a>Πώς μπορείτε να συσχετίσετε μια δεσμευμένη διεύθυνση IP για να εκτελείται μια ανάπτυξη
Το παρακάτω δέσμη ενεργειών δημιουργεί μια νέα υπηρεσία στο cloud με το όνομα *TestService2* με μια νέα Εικονική με το όνομα *TestVM2*, και, στη συνέχεια, συσχετίζει την υπάρχουσα δεσμευμένη διεύθυνση IP με το όνομα *MyReservedIP* για την υπηρεσία cloud.

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## <a name="how-to-associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Πώς μπορείτε να συσχετίσετε μια δεσμευμένη διεύθυνση IP σε μια υπηρεσία cloud, χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων υπηρεσίας
Μπορείτε επίσης να συσχετίσετε μια δεσμευμένη διεύθυνση IP σε μια υπηρεσία cloud, χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων (CSCFG) υπηρεσίας. Το παρακάτω δείγμα xml δείχνει πώς μπορείτε να ρυθμίσετε μια υπηρεσία cloud για να χρησιμοποιήσετε ένα δεσμευμένο VIP με το όνομα *MyReservedIP*:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Επόμενα βήματα

- Κατανόηση του τρόπου [διευθύνσεις IP](virtual-network-ip-addresses-overview-classic.md) λειτουργίας στο μοντέλο κλασική ανάπτυξης.

- Μάθετε περισσότερα σχετικά με [δεσμευμένες ιδιωτικών διευθύνσεων IP](virtual-networks-reserved-private-ip.md).

- Μάθετε σχετικά με τις [διευθύνσεις IP δημόσια παρουσία επίπεδο (ILPIP)](virtual-networks-instance-level-public-ip.md).
