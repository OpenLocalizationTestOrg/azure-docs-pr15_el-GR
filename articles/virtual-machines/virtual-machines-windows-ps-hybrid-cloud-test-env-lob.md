<properties 
    pageTitle="Το περιβάλλον δοκιμής εφαρμογής LOB | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια γραμμή που βασίζεται στο web, επιχειρηματικών εφαρμογών σε ένα υβριδικό περιβάλλον cloud για IT pro ή ανάπτυξη δοκιμές." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Ρύθμιση μιας εφαρμογής LOB βασίζεται στο web σε μια υβριδική στο cloud για σκοπούς δοκιμής

Αυτό το θέμα σας καθοδηγεί στη δημιουργία προσομοιωμένη υβριδικού περιβάλλοντος cloud για σκοπούς δοκιμής μια γραμμή εφαρμογής εταιρικών (LOB) που φιλοξενούνται στο Microsoft Azure βασίζεται στο web. Ακολουθεί η ρύθμιση παραμέτρων που προκύπτει.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Αυτή η ρύθμιση παραμέτρων αποτελείται από:

- Ένα δίκτυο προσομοιωμένη εσωτερικής εγκατάστασης που φιλοξενούνται στο Azure (το TestLab VNet).
- Ένα δίκτυο εσωτερικής εγκατάστασης σταυρό εικονικού φιλοξενείται στο Azure (TestVNET).
- Μια σύνδεση VPN VNet-VNet.
- Ένα διακομιστή LOB που βασίζεται στο web, SQL server και ελεγκτή δευτερεύοντα τομέα στο δίκτυο εικονικού TestVNET.

Αυτή η ρύθμιση παραμέτρων παρέχει μια βάση και κοινές σημείο εκκίνησης από την οποία μπορείτε να κάνετε:

- Ανάπτυξη και δοκιμή LOB εφαρμογές που φιλοξενείται στο Internet Information Services (IIS) με έναν υπολογιστή στο παρασκήνιο βάσης δεδομένων του SQL Server 2014 στο Azure.
- Εκτέλεση δοκιμών του φόρτου εργασίας IT βασίζεται στο cloud από αυτό προσομοιωμένη υβριδική.

Υπάρχουν τρεις κύριες φάσεις για τη ρύθμιση των αυτό το υβριδικό περιβάλλον δοκιμής cloud:

1.  Ρυθμίστε το προσομοιωμένη υβριδικό περιβάλλον cloud.
2.  Ρυθμίστε τις παραμέτρους του υπολογιστή SQL server (SQL1).
3.  Ρύθμιση παραμέτρων του διακομιστή LOB (LOB1).

Σε αυτό το φόρτο εργασίας απαιτεί μια συνδρομή του Azure. Εάν έχετε μια συνδρομή MSDN ή Visual Studio, ανατρέξτε στο θέμα [πιστωτικής μηνιαία Azure για τους συνδρομητές του Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Για ένα παράδειγμα μιας εφαρμογής LOB που φιλοξενούνται στο Azure παραγωγής, ανατρέξτε στο θέμα το σχέδιο αρχιτεκτονική **επιχειρησιακές εφαρμογές** στο [διαγράμματα αρχιτεκτονικής λογισμικού της Microsoft και σχέδια](http://msdn.microsoft.com/dn630664).

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Φάση 1: Ρύθμιση του περιβάλλοντος του cloud προσομοιωμένη υβριδική

Δημιουργήστε την [προσομοιωμένη υβριδικό περιβάλλον δοκιμής cloud](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md). Επειδή αυτό το περιβάλλον δοκιμής δεν απαιτεί την παρουσία του διακομιστή APP1 στο υποδίκτυο Corpnet, μπορείτε να την τερματίσετε προς το παρόν.

Αυτή είναι η τρέχουσα ρύθμιση παραμέτρων.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)
 
## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Φάση 2: Ρύθμιση παραμέτρων του υπολογιστή SQL server (SQL1)

Από την πύλη Azure, ξεκινήστε τον υπολογιστή ελεγκτή τομέα DC2, εάν είναι απαραίτητο.

Στη συνέχεια, δημιουργήστε μια εικονική μηχανή για SQL1 με αυτές τις εντολές σε μια γραμμή εντολών του PowerShell Azure στον τοπικό σας υπολογιστή. Πριν από την εκτέλεση αυτών των εντολών, συμπληρώστε τις τιμές μεταβλητών και να καταργήσετε το < και > χαρακτήρες.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Χρησιμοποιήστε την πύλη του Azure για να συνδεθείτε με SQL1 χρήση του λογαριασμού τοπικού διαχειριστή του SQL1.

Στη συνέχεια, ρυθμίστε τις παραμέτρους κανόνες τείχους προστασίας των Windows για να επιτρέψετε δοκιμές βασική σύνδεση και κίνηση του SQL Server. Από μια γραμμή εντολών του Windows PowerShell επιπέδου διαχειριστή στην SQL1, εκτελέστε αυτές οι εντολές.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Η εντολή ping θα πρέπει να έχει ως αποτέλεσμα τέσσερις επιτυχής απαντήσεις από τη διεύθυνση IP 192.168.0.4.

Στη συνέχεια, προσθέστε το δίσκο επιπλέον δεδομένα στη SQL1 ως μια νέα ένταση ήχου με το γράμμα στ:.

1.  Στο αριστερό τμήμα του παραθύρου της διαχείρισης διακομιστών, κάντε κλικ στην επιλογή **αρχείο και τις υπηρεσίες αποθήκευσης**και, στη συνέχεια, κάντε κλικ στην επιλογή **δίσκων**.
2.  Στο παράθυρο περιεχόμενο, στην ομάδα **δίσκων** , κάντε κλικ στην επιλογή **δίσκο 2** (με τα **διαμερίσματα** οριστεί σε **Άγνωστη**).
3.  Κάντε κλικ στην επιλογή **εργασίες**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέα ένταση**.
4.  Στην πριν ξεκινήσετε σελίδα του οδηγού ένταση, κάντε κλικ στο κουμπί **Επόμενο**.
5.  Στη σελίδα επιλέξτε τον διακομιστή και του δίσκου, κάντε κλικ στην επιλογή **2 δίσκου**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. Όταν σας ζητηθεί, κάντε κλικ στο **κουμπί OK**.
6.  Στη Καθορισμός το μέγεθος της σελίδας ένταση, κάντε κλικ στο κουμπί **Επόμενο**.
7.  Στη εκχώρηση σε μια σελίδα γράμμα ή το φάκελο μονάδα δίσκου, κάντε κλικ στο κουμπί **Επόμενο**.
8.  Στη σελίδα ρυθμίσεις Επιλέξτε αρχείο συστήματος, κάντε κλικ στο κουμπί **Επόμενο**.
9.  Στη σελίδα Επιλογές επιβεβαίωση, κάντε κλικ στην επιλογή **Δημιουργία**.
10. Όταν τελειώσετε, κάντε κλικ στο κουμπί **Κλείσιμο**.

Εκτέλεση αυτών των εντολών στη γραμμή εντολών του Windows PowerShell σε SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

Στη συνέχεια, συμμετοχή SQL1 τομέα CORP Windows Server υπηρεσίας καταλόγου Active Directory με αυτές τις εντολές στη γραμμή εντολών του Windows PowerShell σε SQL1.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Χρησιμοποιήστε το λογαριασμό CORP\User1 όταν σας ζητηθεί να παρέχετε διαπιστευτήρια λογαριασμού τομέα για την εντολή **Προσθήκη υπολογιστή** .

Μετά την επανεκκίνηση, χρησιμοποιήστε την πύλη του Azure για να συνδεθείτε με SQL1 *με το λογαριασμό τοπικού διαχειριστή του SQL1*.

Στη συνέχεια, ρύθμιση παραμέτρων του SQL Server 2014, να χρησιμοποιήσετε τη μονάδα δίσκου στ: για νέες βάσεις δεδομένων και για δικαιώματα λογαριασμού χρήστη.

1.  Από την οθόνη έναρξης, πληκτρολογήστε **SQL Server Management**και, στη συνέχεια, κάντε κλικ στην επιλογή **SQL Server 2014 Management Studio**.
2.  Στην περιοχή **σύνδεση με το διακομιστή**, κάντε κλικ στην επιλογή **σύνδεση**.
3.  Στο παράθυρο Εξερεύνηση αντικείμενο δέντρου, κάντε δεξί κλικ **SQL1**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ιδιότητες**.
4.  Στο παράθυρο **Ιδιότητες διακομιστή** , κάντε κλικ στην επιλογή **Ρυθμίσεις της βάσης δεδομένων**.
5.  Εντοπίστε τη **βάση δεδομένων προεπιλεγμένες θέσεις** και να ορίσετε αυτές τις τιμές: 
    - Για τα **δεδομένα**, πληκτρολογήστε τη διαδρομή **f:\Data**.
    - **Αρχείο καταγραφής**, πληκτρολογήστε τη διαδρομή **f:\Log**.
    - **Δημιουργία αντιγράφων ασφαλείας**, πληκτρολογήστε τη διαδρομή **f:\Backup**.
    - Σημείωση: Μόνο νέες βάσεις δεδομένων, χρησιμοποιήστε αυτές τις θέσεις.
6.  Κάντε κλικ στο **κουμπί OK** για να κλείσετε το παράθυρο.
7.  Στο παράθυρο **Εξερεύνηση αντικείμενο** δέντρου, ανοίξτε **ασφαλείας**.
8.  Κάντε δεξί κλικ σε **συνδέσεις** και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέα σύνδεση**.
9.  Στο πλαίσιο **όνομα σύνδεσης**, πληκτρολογήστε **CORP\User1**.
10. Στη σελίδα **Τους ρόλους διακομιστή** , κάντε κλικ στην επιλογή **sysadmin**και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.
11. Κλείστε το Microsoft SQL Server Management Studio.

Αυτή είναι η τρέχουσα ρύθμιση παραμέτρων.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)
 
## <a name="phase-3-configure-the-lob-server-lob1"></a>Φάση 3: Ρύθμιση παραμέτρων του διακομιστή LOB (LOB1)

Πρώτα, δημιουργήστε μια εικονική μηχανή για LOB1 με αυτές τις εντολές στη γραμμή εντολών του PowerShell Azure στον τοπικό σας υπολογιστή.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Στη συνέχεια, χρησιμοποιήστε την πύλη του Azure για να συνδεθείτε με LOB1 με τα διαπιστευτήρια του λογαριασμού τοπικού διαχειριστή του LOB1.

Στη συνέχεια, ρυθμίστε τις παραμέτρους ενός κανόνα τείχους προστασίας των Windows για να επιτρέψετε την κυκλοφορία για σκοπούς δοκιμής βασική σύνδεση. Από μια γραμμή εντολών του Windows PowerShell επιπέδου διαχειριστή στην LOB1, εκτελέστε αυτές οι εντολές.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Η εντολή ping θα πρέπει να έχει ως αποτέλεσμα τέσσερις επιτυχής απαντήσεις από τη διεύθυνση IP 192.168.0.4.

Στη συνέχεια, συμμετοχή LOB1 στον τομέα της υπηρεσίας καταλόγου Active Directory CORP με αυτές τις εντολές στη γραμμή εντολών του Windows PowerShell.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Χρησιμοποιήστε το λογαριασμό CORP\User1 όταν σας ζητηθεί να παρέχετε διαπιστευτήρια λογαριασμού τομέα για την εντολή **Προσθήκη υπολογιστή** .

Μετά την επανεκκίνηση, χρησιμοποιήστε την πύλη του Azure για να συνδεθείτε στο LOB1 με το λογαριασμό CORP\User1 και τον κωδικό πρόσβασης.

Στη συνέχεια, ρύθμιση παραμέτρων LOB1 για τις υπηρεσίες IIS και δοκιμάστε πρόσβαση από CLIENT1.

1.  Από τη Διαχείριση διακομιστή, κάντε κλικ στην επιλογή **Προσθήκη ρόλων και δυνατότητες**.
2.  Στη σελίδα **πριν να ξεκινήσετε** , κάντε κλικ στο κουμπί **Επόμενο**.
3.  Στη σελίδα **Επιλογή τύπου εγκατάστασης** , κάντε κλικ στο κουμπί **Επόμενο**.
4.  Στη σελίδα **Επιλέξτε διακομιστή προορισμού** , κάντε κλικ στο κουμπί **Επόμενο**.
5.  Στη σελίδα **τους ρόλους διακομιστή** , κάντε κλικ στην επιλογή **Web Server (IIS)** στη λίστα με **τους ρόλους**.
6.  Όταν σας ζητηθεί, κάντε κλικ στην επιλογή **Προσθήκη δυνατοτήτων**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
7.  Στη σελίδα **Επιλέξτε δυνατότητες** , κάντε κλικ στο κουμπί **Επόμενο**.
8.  Στη σελίδα **Web Server (IIS)** , κάντε κλικ στο κουμπί **Επόμενο**.
9.  Στη σελίδα **επιλογή ρόλου για τις υπηρεσίες** , επιλέξτε ή καταργήστε την επιλογή από τα πλαίσια ελέγχου για τις υπηρεσίες που χρειάζεστε για τη δοκιμή της εφαρμογής σας LOB και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
10. Στη σελίδα **Επιβεβαιώστε τις επιλογές εγκατάστασης** , κάντε κλικ στην επιλογή **εγκατάσταση**.
11. Περιμένετε μέχρι να ολοκληρωθεί η εγκατάσταση των στοιχείων και, στη συνέχεια, κάντε κλικ στο κουμπί **Κλείσιμο**.
12. Από την πύλη Azure, συνδεθείτε με τον υπολογιστή CLIENT1 με τα διαπιστευτήρια λογαριασμού CORP\User1 και, στη συνέχεια, ξεκινήστε τον Internet Explorer.
13. Στη γραμμή διευθύνσεων, πληκτρολογήστε **http://lob1/** και, στη συνέχεια, πατήστε το πλήκτρο ENTER. Θα πρέπει να βλέπετε την προεπιλεγμένη σελίδα web των υπηρεσιών IIS 8.

Αυτή είναι η τρέχουσα ρύθμιση παραμέτρων.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)
 
Αυτό το περιβάλλον είναι τώρα έτοιμο για να αναπτύξετε την εφαρμογή που βασίζεται στο web στο LOB1 και να δοκιμάσετε τη λειτουργικότητα από CLIENT1 στο υποδίκτυο Corpnet.

## <a name="next-step"></a>Επόμενο βήμα

- Προσθέστε μια νέα εικονική μηχανή με την [πύλη του Azure](virtual-machines-windows-hero-tutorial.md).
