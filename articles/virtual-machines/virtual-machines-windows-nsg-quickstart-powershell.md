<properties
   pageTitle="Ανοίξτε θύρες σε μια Εικονική χρήση του PowerShell | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να ανοίξετε μια θύρα / δημιουργία ένα τελικό σημείο για να σας Εικονική Windows χρησιμοποιώντας την κατάσταση λειτουργίας ανάπτυξης διαχείρισης Azure πόρων και Azure PowerShell"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Άνοιγμα θύρες και τελικών σημείων μια Εικονική στο Azure χρησιμοποιώντας το PowerShell
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Γρήγορες εντολές
Για να δημιουργήσετε μια ομάδα ασφαλείας δικτύου και ACL κανόνες χρειάζεστε [πιο πρόσφατη έκδοση του Azure PowerShell εγκατεστημένο](../powershell-install-configure.md). Μπορείτε επίσης να [εκτελέσετε αυτά τα βήματα με την πύλη Azure](virtual-machines-windows-nsg-quickstart-portal.md).

Συνδεθείτε στο λογαριασμό σας στο Azure:

```powershell
Login-AzureRmAccount
```

Στα παρακάτω παραδείγματα, αντικαταστήστε τα ονόματα παραμέτρων παράδειγμα με τις δικές σας τιμές. Τα ονόματα παραμέτρων παράδειγμα περιλαμβάνονται `myResourceGroup`, `myNetworkSecurityGroup`, και `myVnet`.

Δημιουργήστε έναν κανόνα. Το ακόλουθο παράδειγμα δημιουργεί έναν κανόνα που ονομάζεται `myNetworkSecurityGroupRule` για να επιτρέψετε την κυκλοφορία TCP στη θύρα 80:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Στη συνέχεια, δημιουργήστε την ομάδα ασφάλειας του δικτύου σας και αντιστοιχίστε τον κανόνα HTTP που μόλις δημιουργήσατε ως εξής. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα ασφαλείας δίκτυο με το όνομα `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Τώρα ας εκχώρηση σας ομάδα ασφαλείας δικτύου με ένα δευτερεύον. Το παρακάτω παράδειγμα αντιστοιχίζει ένα υπάρχον εικονικό δίκτυο με το όνομα `myVnet` στη μεταβλητή `$vnet`:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Συσχετισμός ομάδα ασφαλείας σας δίκτυο με το δικό σας υποδίκτυο. Το ακόλουθο παράδειγμα συσχετίζει δευτερεύον δίκτυο με το όνομα `mySubnet` με την ομάδα ασφαλείας σας δίκτυο:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Τέλος, ενημερώστε εικονικού το δίκτυό σας για να εφαρμοστούν οι αλλαγές σας:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Περισσότερες πληροφορίες σχετικά με τις ομάδες ασφαλείας δικτύου
Η γρήγορη εντολές εδώ σάς επιτρέπουν να εξασκηθείτε στη με κίνηση ρέει προς το Εικονική. Ομάδες ασφαλείας δικτύου παρέχει πολλά εξαιρετικά δυνατότητες και υποδιαίρεσης για τον έλεγχο πρόσβασης σε πόρους σας. Μπορείτε να διαβάσετε περισσότερα σχετικά με [τη δημιουργία μιας ομάδας ασφαλείας δικτύου και κανόνες ACL εδώ](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Μπορείτε να ορίσετε κανόνες ACL και τις ομάδες ασφαλείας δικτύου ως μέρος της διαχείρισης πόρων Azure πρότυπα. Διαβάστε περισσότερα σχετικά με [τη δημιουργία δικτύου ομάδες ασφαλείας με τα πρότυπα](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Εάν πρέπει να χρησιμοποιήσετε θύρα προώθησης για να αντιστοιχίσετε ένα μοναδικό εξωτερική θύρα σε μια εσωτερική θύρα Εικονική σας, χρησιμοποιήστε μια μονάδα εξισορρόπησης φόρτου και κανόνες μετάφρασης διευθύνσεων δικτύου (NAT). Για παράδειγμα, μπορεί να θέλετε να εκθέσετε TCP θύρα 8080 εξωτερικά και έχουν κίνηση κατευθύνεται στη θύρα TCP 80 σε μια Εικονική. Μπορείτε να μάθετε σχετικά με [τη δημιουργία μια μονάδα εξισορρόπησης φόρτου μέσω Internet](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το παράδειγμα, ίσως να δημιουργήσει έναν απλό κανόνα για να επιτρέψετε την κυκλοφορία HTTP. Μπορείτε να βρείτε πληροφορίες σχετικά με τη δημιουργία πιο λεπτομερή περιβάλλοντα στα ακόλουθα άρθρα:

- [Azure Επισκόπηση της διαχείρισης πόρων](../azure-resource-manager/resource-group-overview.md)
- [Τι είναι μια ομάδα ασφαλείας δικτύου (NSG);](../virtual-network/virtual-networks-nsg.md)
- [Azure Επισκόπηση διαχείρισης πόρων για Balancers φόρτωσης](../load-balancer/load-balancer-arm.md)