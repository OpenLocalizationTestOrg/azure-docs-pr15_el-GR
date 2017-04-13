<properties
   pageTitle="Πολλαπλές VIPs για μια υπηρεσία στο cloud"
   description="Επισκόπηση των multiVIP και πώς μπορείτε να ορίσετε πολλές VIPs σε μια υπηρεσία στο cloud"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-multiple-vips-for-a-cloud-service"></a>Ρύθμιση παραμέτρων πολλών VIPs για μια υπηρεσία στο cloud

Μπορείτε να αποκτήσετε πρόσβαση σε υπηρεσίες Azure cloud μέσω του Internet δημόσια, χρησιμοποιώντας μια διεύθυνση IP που παρέχεται από Azure. Αυτή η δημόσια διεύθυνση IP αναφέρεται ως ένα VIP (εικονικό IP) επειδή είναι συνδεδεμένη με το Azure εξισορρόπηση φόρτου και όχι η εικονική μηχανή (Εικονική) παρουσίες εντός την υπηρεσία cloud. Μπορείτε να αποκτήσετε πρόσβαση σε οποιαδήποτε παρουσία Εικονική μέσα σε μια υπηρεσία cloud, χρησιμοποιώντας μια μεμονωμένη VIP.

Ωστόσο, υπάρχουν σενάρια όπου μπορεί να χρειάζεστε περισσότερες από μία VIP ως καταχώρηση οδηγούν στην ίδια υπηρεσία cloud. Για παράδειγμα, την υπηρεσία cloud ενδέχεται να φιλοξενήσετε πολλές τοποθεσίες Web που απαιτούν σύνδεσης SSL μέσω η προεπιλεγμένη θύρα της 443, όπως κάθε τοποθεσία φιλοξενείται για διαφορετικό πελάτη ή μισθωτή. Σε αυτό το σενάριο, πρέπει να έχετε μια διαφορετική δημόσια αντικριστές διεύθυνση IP για κάθε τοποθεσία Web. Το παρακάτω διάγραμμα παρουσιάζει ένα τυπικό πολλών μισθωτών που φιλοξενούν το web με μια ανάγκη για πολλά πιστοποιητικά SSL στην ίδια θύρα δημόσια.

![Σενάριο πολλών VIP SSL](./media/load-balancer-multivip/Figure1.png)

Στο παραπάνω παράδειγμα, όλες VIPs, χρησιμοποιήστε την ίδια δημόσια θύρα (443) και κίνηση ανακατευθύνεται σε μία ή περισσότερες φόρτωσης εξισορρόπηση ΣΠΣ σε ένα μοναδικό ιδιωτικό θύρας για την εσωτερική διεύθυνση IP της υπηρεσίας cloud φιλοξενίας όλες τις τοποθεσίες Web.

>[AZURE.NOTE] Μια άλλη περίπτωση που απαιτεί τη χρήση του πολλών VIPs φιλοξενεί πολλές ακροατών ομάδας διαθεσιμότητας SQL AlwaysOn σε το ίδιο σύνολο εικονικές μηχανές.

VIPs είναι δυναμικά από προεπιλογή, γεγονός που σημαίνει ότι η πραγματική διεύθυνση IP που έχουν εκχωρηθεί στην υπηρεσία cloud ενδέχεται να αλλάξουν μέσα στο χρόνο. Για να αποτρέψετε αυτήν τη λειτουργία, μπορείτε να δεσμεύσετε ένα VIP της υπηρεσίας. Για να μάθετε περισσότερα σχετικά με τη δεσμευμένη VIPs, ανατρέξτε στο θέμα [Δεσμευμένη δημόσια διεύθυνση IP](../virtual-network/virtual-networks-reserved-public-ip.md).

>[AZURE.NOTE] Ανατρέξτε στο θέμα [τις τιμές διεύθυνση IP](https://azure.microsoft.com/pricing/details/ip-addresses/) για πληροφορίες σχετικά με τις τιμές σε VIPs και δεσμευμένες διευθύνσεις IP.

Μπορείτε να χρησιμοποιήσετε PowerShell για να επιβεβαιώσετε την VIPs που χρησιμοποιούνται από τις υπηρεσίες cloud, καθώς και προσθήκη και κατάργηση VIPs, συσχετίσετε μια VIP ένα τελικό σημείο, και ρυθμίσετε τις παραμέτρους σε ένα συγκεκριμένο VIP εξισορρόπησης φόρτου.

## <a name="limitations"></a>Περιορισμοί

Προς το παρόν, VIP πολλούς λειτουργικότητα περιορίζεται στα ακόλουθα σενάρια:

- **Μόνο IaaS**. Μπορείτε μόνο να ενεργοποιήσετε πολλούς VIP για τις υπηρεσίες cloud που περιέχουν ΣΠΣ. Δεν μπορείτε να χρησιμοποιήσετε πολλούς VIP σε σενάρια PaaS με ρόλο παρουσίες.
- **Μόνο του PowerShell**. Μπορείτε μόνο να διαχειριστείτε πολλούς VIP με χρήση του PowerShell.

Αυτές οι περιορισμοί είναι προσωρινά και ενδέχεται να αλλάξουν οποιαδήποτε στιγμή. Φροντίστε να επιστρέψουν αυτήν τη σελίδα για να επαληθεύσετε μελλοντικές αλλαγές.


## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Πώς μπορείτε να προσθέσετε μια VIP σε μια υπηρεσία cloud

Για να προσθέσετε μια VIP της υπηρεσίας, εκτελέστε την ακόλουθη εντολή PowerShell:

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

Αυτή η εντολή εμφανίζει ένα αποτέλεσμα που είναι παρόμοιο με το παρακάτω παράδειγμα:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Πώς να καταργήσετε μια VIP από μια υπηρεσία στο cloud

Για να καταργήσετε το VIP προσθέσει στην υπηρεσία σας στο παραπάνω παράδειγμα, εκτελέστε την ακόλουθη εντολή PowerShell:

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] Μπορείτε να καταργήσετε μια VIP μόνο εάν έχει τα τελικά σημεία δεν έχει συσχετιστεί με αυτό.

## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Πώς μπορείτε να ανακτήσετε πληροφορίες VIP από μια υπηρεσία στο Cloud

Για να ανακτήσετε τα VIPs που σχετίζεται με μια υπηρεσία cloud, εκτελέστε την ακόλουθη δέσμη ενεργειών PowerShell:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Η δέσμη ενεργειών εμφανίζει ένα αποτέλεσμα που είναι παρόμοιο με το παρακάτω παράδειγμα:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

Σε αυτό το παράδειγμα, η υπηρεσία cloud έχει 3 VIPs:

- **Vip1** είναι η προεπιλεγμένη VIP, που γνωρίζετε, επειδή η τιμή για το IsDnsProgrammedName έχει οριστεί στην τιμή true.
- Δεν χρησιμοποιούνται **Vip2** και **Vip3** που δεν έχουν τις διευθύνσεις IP. Αυτά θα χρησιμοποιηθούν μόνο εάν μπορείτε να συσχετίσετε ένα τελικό σημείο για να το VIP.

>[AZURE.NOTE] Τη συνδρομή σας θα χρεωθεί μόνο για επιπλέον VIPs, αφού γίνουν σχετίζεται με ένα τελικό σημείο. Για περισσότερες πληροφορίες σχετικά με τις τιμές, ανατρέξτε στο θέμα [τις τιμές διεύθυνση IP](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Πώς μπορείτε να συσχετίσετε ένα VIP ένα τελικό σημείο

Για να συσχετίσετε ένα VIP σε μια υπηρεσία στο cloud για ένα τελικό σημείο, εκτελέστε την ακόλουθη εντολή PowerShell:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

Η εντολή δημιουργεί ένα τελικό σημείο που συνδέονται με το VIP που ονομάζεται *Vip2* στη θύρα *80*και συνδέσεις για να την εικονική Μηχανή με το όνομα *myVM1* σε μια υπηρεσία στο cloud με το όνομα *myService* χρησιμοποιώντας *TCP* στη θύρα *8080*.

Για να επαληθεύσετε τις ρυθμίσεις παραμέτρων, εκτελέστε την ακόλουθη εντολή PowerShell:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Το αποτέλεσμα μοιάζει με το ακόλουθο παράδειγμα:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Πώς μπορείτε να ενεργοποιήσετε σε μια συγκεκριμένη VIP εξισορρόπησης φόρτου

Μπορείτε να συσχετίσετε ένα μεμονωμένο VIP με πολλές εικονικές μηχανές για σκοπούς εξισορρόπησης φόρτου. Για παράδειγμα, έχετε μια υπηρεσία στο cloud με το όνομα *myService*και δύο εικονικές μηχανές ονομάζεται *myVM1* και *myVM2*. Και την υπηρεσία cloud έχει πολλές VIPs, μία από αυτές με την ονομασία *Vip2*. Εάν θέλετε να βεβαιωθείτε ότι όλη την κυκλοφορία στο θύρα *81* σε *Vip2* διανέμεται μεταξύ *myVM1* και *myVM2* στη θύρα *8181*, εκτελέστε την ακόλουθη δέσμη ενεργειών PowerShell:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

Μπορείτε επίσης να ενημερώσετε την εξισορρόπηση φόρτου για να χρησιμοποιήσετε μια διαφορετική VIP. Για παράδειγμα, εάν εκτελείτε την παρακάτω εντολή PowerShell, θα μπορείτε να αλλάξετε το σύνολο για να χρησιμοποιήσετε μια VIP με το όνομα Vip1 εξισορρόπησης φόρτου:

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## <a name="next-steps"></a>Επόμενα βήματα

[Αρχείο καταγραφής ανάλυσης για εξισορρόπησης φόρτου Azure](load-balancer-monitor-log.md)

[Επισκόπηση εξισορρόπησης φόρτου αντικριστές Internet](load-balancer-internet-overview.md)

[Γρήγορα αποτελέσματα στο Internet αντικριστές εξισορρόπηση φόρτου](load-balancer-get-started-internet-arm-ps.md)

[Επισκόπηση εικονικού δικτύου](../virtual-network/virtual-networks-overview.md)

[APIs ΥΠΌΛΟΙΠΑ δεσμευμένη διεύθυνση IP](https://msdn.microsoft.com/library/azure/dn722420.aspx)