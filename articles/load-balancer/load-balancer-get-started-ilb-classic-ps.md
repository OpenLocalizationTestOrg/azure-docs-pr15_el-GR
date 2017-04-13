<properties
   pageTitle="Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου με χρήση του PowerShell στο μοντέλο κλασική ανάπτυξης | Microsoft Azure"
   description="Μάθετε πώς να δημιουργείτε μια εσωτερική εξισορρόπηση φόρτου με χρήση του PowerShell στο μοντέλο κλασική ανάπτυξης"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Να ξεκινήσετε τη δημιουργία μιας εσωτερικής μονάδα εξισορρόπησης φόρτου (κλασικό) με χρήση του PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας το μοντέλο από διαχειριστή πόρων](load-balancer-get-started-ilb-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου για εικονικές μηχανές

Για να δημιουργήσετε ένα σύνολο εξισορρόπησης εσωτερικό φόρτωση και τους διακομιστές που θα σας στέλνει την κυκλοφορία τους σε αυτό, πρέπει να κάνετε τα εξής:

1. Δημιουργήστε μια παρουσία του εσωτερικού εξισορρόπηση φόρτου που θα είναι το τελικό σημείο εισερχόμενης κυκλοφορίας για να γίνει με εξισορρόπηση σε τους διακομιστές ενός συνόλου εξισορρόπηση φόρτου.

1. Προσθήκη τελικά σημεία αντιστοιχεί τις εικονικές μηχανές που θα να λαμβάνουν τα εισερχόμενα δεδομένα.

1. Ρύθμιση παραμέτρων τους διακομιστές που θα στέλνετε η κίνηση για να γίνει με εξισορρόπηση για να στείλετε την κυκλοφορία τους για να την εικονική διεύθυνση IP (VIP) της παρουσίας του εσωτερικού εξισορρόπηση φόρτου.


### <a name="step-1-create-an-internal-load-balancing-instance"></a>Βήμα 1: Δημιουργήστε μια παρουσία εσωτερικού εξισορρόπηση φόρτου

Για μια υπάρχουσα υπηρεσία cloud ή μια υπηρεσία cloud αναπτυχθεί στην περιοχή τοπικές εικονικό δίκτυο, μπορείτε να δημιουργήσετε μια παρουσία εσωτερικού εξισορρόπηση φόρτου με τις παρακάτω εντολές του Windows PowerShell:

    $svc="<Cloud Service Name>"
    $ilb="<Name of your ILB instance>"
    $subnet="<Name of the subnet within your virtual network>"
    $IP="<The IPv4 address to use on the subnet-optional>"

    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


Σημειώστε ότι αυτή η χρήση του το [Πρόσθετο AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell cmdlet χρησιμοποιεί τη ρύθμιση παραμέτρων DefaultProbe. Για περισσότερες πληροφορίες σχετικά με τα σύνολα πρόσθετη παράμετρο, ανατρέξτε στο θέμα [Προσθήκη AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a>Βήμα 2: Προσθήκη τελικά σημεία για να την παρουσία του εσωτερικού εξισορρόπηση φόρτου

Ακολουθεί ένα παράδειγμα:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $ilb="ilbset"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a>Βήμα 3: Ρύθμιση των παραμέτρων τους διακομιστές σας για να στείλετε την κυκλοφορία τους στο νέο τελικό σημείο εσωτερικό εξισορρόπηση φόρτου

Πρέπει να ρυθμίσετε τις παραμέτρους των διακομιστών των οποίων η κυκλοφορία πρόκειται να γίνει με εξισορρόπηση για να χρησιμοποιήσετε τη νέα διεύθυνση IP (το VIP) της παρουσίας του εσωτερικού εξισορρόπηση φόρτου. Αυτή είναι η διεύθυνση στην οποία ακρόαση την παρουσία του εσωτερικού εξισορρόπηση φόρτου. Στις περισσότερες περιπτώσεις, πρέπει να προσθέσετε ή να τροποποιήσετε μια εγγραφή DNS για το VIP της παρουσίας του εσωτερικού εξισορρόπηση φόρτου μόνο.

Εάν έχετε καθορίσει τη διεύθυνση IP κατά τη δημιουργία της παρουσίας του εσωτερικού εξισορρόπηση φόρτου, έχετε ήδη το VIP. Διαφορετικά, μπορείτε να δείτε το VIP από τις παρακάτω εντολές:

    $svc="<Cloud Service Name>"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer



Για να χρησιμοποιήσετε αυτές τις εντολές, συμπληρώστε τις τιμές και να καταργήσετε το < και >. Ακολουθεί ένα παράδειγμα:

    $svc="mytestcloud"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer


Από την εμφάνιση της την εντολή Get-AzureInternalLoadBalancer, σημειώστε τη διεύθυνση IP και κάντε τις απαραίτητες αλλαγές για να σας διακομιστών ή των εγγραφών DNS για να βεβαιωθείτε ότι κίνηση αποστέλλονται στο το VIP.

>[AZURE.NOTE]Της πλατφόρμας Windows Azure χρησιμοποιεί μια στατική, δυνατότητα δρομολόγησης δημόσια διεύθυνση IPv4 για μια ποικιλία διαχείρισης σενάρια. Η διεύθυνση IP είναι 168.63.129.16. Αυτή η διεύθυνση IP δεν πρέπει να αποκλειστεί από οποιαδήποτε τείχη προστασίας, επειδή μπορεί να προκαλέσει μη αναμενόμενη συμπεριφορά.
>Σε σχέση με Azure εσωτερικό εξισορρόπηση φόρτου, χρησιμοποιείται αυτήν τη διεύθυνση IP, παρακολουθώντας καθετήρες από τη μονάδα εξισορρόπησης φόρτου για να προσδιορίσετε την κατάσταση εύρυθμης λειτουργίας για εικονικές μηχανές σε ένα σύνολο εξισορρόπησης φόρτου. Εάν μια ομάδα ασφαλείας δικτύου χρησιμοποιείται για να περιορίσετε την κίνηση Azure εικονικές μηχανές σε ένα σύνολο εσωτερικά εξισορρόπηση φόρτου ή έχει εφαρμοστεί σε εικονικό δίκτυο υποδίκτυο, βεβαιωθείτε ότι έχει προστεθεί ένας κανόνας ασφαλείας δικτύου για να επιτρέψετε την κυκλοφορία από 168.63.129.16.


## <a name="example-of-internal-load-balancing"></a>Παράδειγμα εξισορρόπηση φόρτου εσωτερικό

Για να σας καθοδηγήσει το τέλος στο τέλος διεργασίας της δημιουργίας ενός συνόλου εξισορρόπηση φόρτου για δύο παράδειγμα διαμορφώσεις, ανατρέξτε στις παρακάτω ενότητες.

### <a name="an-internet-facing-multi-tier-application"></a>Internet αντικριστές, εφαρμογή πολλαπλών επιπέδων

Θέλετε να δώσετε μια υπηρεσία εξισορρόπησης φόρτου βάσης δεδομένων για ένα σύνολο διακομιστών web μέσω Internet. Και τα δύο σύνολα των διακομιστών φιλοξενούνται σε μια υπηρεσία cloud μία Azure. Κίνηση διακομιστή Web στη θύρα TCP 1433 πρέπει να κατανέμεται μεταξύ των δύο εικονικές μηχανές στο επίπεδο βάσης δεδομένων. Το σχήμα 1 δείχνει τη ρύθμιση παραμέτρων.

![Εσωτερική εξισορρόπησης φόρτου που έχει οριστεί για το επίπεδο βάσης δεδομένων](./media/load-balancer-internal-getstarted/IC736321.png)


Η ρύθμιση παραμέτρων αποτελείται από τα εξής:

- Την υπάρχουσα υπηρεσία cloud που φιλοξενεί τις εικονικές μηχανές ονομάζεται mytestcloud.

- Δύο υπάρχοντες διακομιστές βάσης δεδομένων είναι καθορισμένοι Φθίνουσα 1, DB2.

- Διακομιστές Web στο επίπεδο web σύνδεσης με τους διακομιστές βάσης δεδομένων στο επίπεδο βάσης δεδομένων, χρησιμοποιώντας την ιδιωτική διεύθυνση IP. Μια άλλη επιλογή είναι να χρησιμοποιήσετε τη δική σας DNS για το εικονικό δίκτυο και καταχώρηση με μη αυτόματο τρόπο μια εγγραφή A για το σύνολο εξισορρόπησης εσωτερικό φόρτωση.

Τις παρακάτω εντολές ρυθμίσετε μια νέα παρουσία εσωτερικού εξισορρόπηση φόρτου με το όνομα **ILBset** και προσθέστε τα τελικά σημεία τις εικονικές μηχανές αντιστοιχεί με τους διακομιστές δύο βάσης δεδομένων:

    $svc="mytestcloud"
    $ilb="ilbset"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $vmname="DB1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="TCP-1433-1433-2"
    $vmname="DB2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


## <a name="remove-an-internal-load-balancing-configuration"></a>Καταργήστε μια ρύθμιση παραμέτρων του εσωτερικού εξισορρόπηση φόρτου

Για να καταργήσετε μια εικονική μηχανή ως τελικού σημείου από μια παρουσία εξισορρόπησης εσωτερικό φόρτωσης, χρησιμοποιήστε τις παρακάτω εντολές:

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Για να χρησιμοποιήσετε αυτές τις εντολές, συμπληρώστε τις τιμές, η κατάργηση του < και >.

Ακολουθεί ένα παράδειγμα:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Για να καταργήσετε μια παρουσία εξισορρόπησης εσωτερικό φόρτωσης από μια υπηρεσία cloud, χρησιμοποιήστε τις παρακάτω εντολές:

    $svc="<Cloud service name>"
    Remove-AzureInternalLoadBalancer -ServiceName $svc

Για να χρησιμοποιήσετε αυτές τις εντολές, συμπληρώστε την τιμή και να καταργήσετε το < και >.

Ακολουθεί ένα παράδειγμα:

    $svc="mytestcloud"
    Remove-AzureInternalLoadBalancer -ServiceName $svc



## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>Πρόσθετες πληροφορίες σχετικά με τα cmdlet εξισορρόπησης εσωτερικό φόρτωσης


Για να αποκτήσετε πρόσθετες πληροφορίες σχετικά με τα cmdlet εσωτερικό εξισορρόπηση φόρτου, εκτελέστε τις ακόλουθες εντολές σε μια γραμμή εντολών του Windows PowerShell:

- Get-Βοήθεια για νέα AzureInternalLoadBalancerConfig-πλήρης

- Λήψη-Βοήθειας Προσθήκη AzureInternalLoadBalancer-πλήρης

- Λήψη Βοήθειας Get-AzureInternalLoadbalancer-πλήρης

- Get-Βοήθεια για κατάργηση AzureInternalLoadBalancer-πλήρης

## <a name="next-steps"></a>Επόμενα βήματα

[Ρύθμιση παραμέτρων μια λειτουργία διανομής εξισορρόπησης φόρτου χρησιμοποιώντας συσχέτισης IP προέλευσης](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)