<properties
   pageTitle="Ανάπτυξη πολλούς NIC ΣΠΣ χρήση του PowerShell στο μοντέλο κλασική ανάπτυξης | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αναπτύξετε το ΣΠΣ NIC πολλούς χρήση του PowerShell στο μοντέλο κλασική ανάπτυξης"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
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

#<a name="deploy-multi-nic-vms-classic-using-powershell"></a>Ανάπτυξη πολλούς ΣΠΣ NIC (κλασική) με χρήση του PowerShell

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Μπορείτε να δημιουργήσετε εικονικές μηχανές (ΣΠΣ) στο Azure και να επισυνάψετε πολλές διασυνδέσεις δικτύου (NIC) σε κάθε μία από τις ΣΠΣ. Πολλά NIC Ενεργοποίηση διαχωρισμό των τύπων κυκλοφορίας κατά μήκος NIC. Για παράδειγμα, ένα NIC μπορεί να επικοινωνήσει με στο Internet, ενώ μια άλλη επικοινωνεί μόνο με εσωτερικούς πόρους που δεν είναι συνδεδεμένοι στο Internet. Η δυνατότητα για να διαχωρίσετε κίνηση του δικτύου σε πολλά NIC απαιτείται για πολλές εικονικές συσκευές δικτύου, όπως η παράδοση εφαρμογών και λύσεις βελτιστοποίησης WAN.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας το μοντέλο από διαχειριστή πόρων](virtual-network-deploy-multinic-arm-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Προς το παρόν, δεν μπορείτε να έχετε ΣΠΣ με ένα μεμονωμένο NIC και ΣΠΣ με πολλά NIC στην ίδια υπηρεσία cloud. Επομένως, πρέπει να υλοποιήσετε τους διακομιστές παρασκηνίου σε μια υπηρεσία cloud διαφορετικό από και όλα τα άλλα στοιχεία στο σενάριο. Τα παρακάτω βήματα χρησιμοποιούν μια υπηρεσία στο cloud με το όνομα *IaaSStory* για το κύριο πόρων και *IaaSStory παρασκηνίου* για τους διακομιστές παρασκηνίου.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να αναπτύξετε τους διακομιστές παρασκηνίου, πρέπει να αναπτύξετε την υπηρεσία cloud κύριο με όλους τους πόρους που είναι απαραίτητο για αυτό το σενάριο. Τουλάχιστον, πρέπει να δημιουργήσετε ένα εικονικό δίκτυο με ένα υποδίκτυο για υπολογιστή στο παρασκήνιο. Επισκεφθείτε [Δημιουργία ένα εικονικό δίκτυο με χρήση του PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md) για να μάθετε πώς μπορείτε να αναπτύξετε ένα εικονικό δίκτυο.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Ανάπτυξη του παρασκηνίου ΣΠΣ

Το ΣΠΣ παρασκηνίου εξαρτώνται από τη δημιουργία των πόρων που παρατίθενται παρακάτω.

- **Υποδίκτυο παρασκηνίου**. Οι διακομιστές βάσης δεδομένων θα είναι μέρος του ξεχωριστό υποδίκτυο, να segregate κίνηση. Η παρακάτω δέσμη ενεργειών αναμένει αυτό υποδίκτυο, να υπάρχουν σε μια vnet με το όνομα *WTestVnet*.
- **Λογαριασμό του χώρου αποθήκευσης για δίσκων δεδομένων**. Για καλύτερες επιδόσεις, των δίσκων δεδομένων στους διακομιστές βάσης δεδομένων θα χρησιμοποιήσει τεχνολογία συμπαγές κατάσταση μονάδα δίσκου (SSD), η οποία απαιτεί ένα λογαριασμό του χώρου αποθήκευσης premium. Βεβαιωθείτε ότι η θέση Azure αναπτύξετε για την υποστήριξη του χώρου αποθήκευσης premium.
- **Ορισμός διαθεσιμότητας**. Θα προστεθούν όλους τους διακομιστές βάσης δεδομένων σε ένα μεμονωμένο διαθεσιμότητα Ορισμός, για να βεβαιωθείτε ότι τουλάχιστον ένα από τα ΣΠΣ είναι προς τα επάνω και να εκτελείται κατά τη συντήρηση.

### <a name="step-1---start-your-script"></a>Βήμα 1 - Έναρξη της δέσμης ενεργειών

Μπορείτε να κάνετε λήψη του πλήρους δέσμη ενεργειών του PowerShell χρησιμοποιείται [εδώ](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Ακολουθήστε τα παρακάτω βήματα για να αλλάξετε τη δέσμη ενεργειών για να εργαστείτε στο περιβάλλον σας.

1. Αλλάξτε τις τιμές από τις παρακάτω μεταβλητές με βάση την υπάρχουσα ομάδα πόρων αναπτυχθεί παραπάνω στο [τις προϋποθέσεις](#Prerequisites).

        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"

2. Αλλάξτε τις τιμές από τις παρακάτω μεταβλητές με βάση τις τιμές που θέλετε να χρησιμοποιήσετε για την ανάπτυξη παρασκηνίου.

        $backendCSName         = "IaaSStory-Backend"
        $prmStorageAccountName = "iaasstoryprmstorage"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $diskSize              = 127
        $vmNamePrefix          = "DB"
        $dataDiskSuffix        = "datadisk"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Βήμα 2 - Δημιουργία απαραίτητων πόρων για το ΣΠΣ

Πρέπει να δημιουργήσετε μια νέα υπηρεσία cloud, και ένα λογαριασμό χώρου αποθήκευσης για το δίσκων δεδομένων για όλες ΣΠΣ. Πρέπει επίσης να καθορίσετε μια εικόνα και ένα λογαριασμό του τοπικού διαχειριστή για το ΣΠΣ. Για να δημιουργήσετε αυτούς τους πόρους, εκτελέστε τα ακόλουθα βήματα.

1. Δημιουργήστε μια νέα υπηρεσία στο cloud.

        New-AzureService -ServiceName $backendCSName -Location $location

2. Δημιουργία νέου λογαριασμού χώρου αποθήκευσης premium.

        New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
            -Location $location `
            -Type Premium_LRS

3. Ορίστε το λογαριασμό χώρου αποθήκευσης που δημιουργήθηκε παραπάνω ως τον τρέχοντα λογαριασμό χώρου αποθήκευσης για τη συνδρομή σας.

        $subscription = Get-AzureSubscription `
            | where {$_.IsCurrent -eq $true}  
        Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
            -CurrentStorageAccountName $prmStorageAccountName

4. Επιλέξτε μια εικόνα για την εικονική Μηχανή.

        $image = Get-AzureVMImage `
            | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
            | sort PublishedDate -Descending `
            | select -ExpandProperty ImageName -First 1

5. Ορίστε τις διαπιστευτήρια λογαριασμού του τοπικού διαχειριστή.

        $cred = Get-Credential -Message "Enter username and password for local admin account"

### <a name="step-3---create-vms"></a>Βήμα 3 - Δημιουργία ΣΠΣ

Πρέπει να χρησιμοποιήσετε ένα βρόχο για να δημιουργήσετε όσες ΣΠΣ θέλετε, και να δημιουργήσετε τις απαραίτητες NIC και ΣΠΣ μέσα στο βρόχο. Για να δημιουργήσετε το NIC και ΣΠΣ, εκτελέστε τα ακόλουθα βήματα.

1. Έναρξη μιας `for` επανάληψη για να επαναλάβετε τις εντολές για να δημιουργήσετε μια Εικονική και δύο NIC όσες φορές είναι απαραίτητο, με βάση την τιμή από το `$numberOfVMs` μεταβλητή.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Δημιουργία μιας `VMConfig` αντικείμενο που καθορίζει την εικόνα, το μέγεθος και διαθεσιμότητα ρύθμιση για την εικονική Μηχανή.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureVMConfig -Name $vmName `
                            -ImageName $image `
                            -InstanceSize $vmSize `
                            -AvailabilitySetName $avSetName  

3. Παροχή η Εικονική ως Windows Εικονική.

            Add-AzureProvisioningConfig -VM $vmConfig -Windows `
                -AdminUsername $cred.UserName `
                -Password $cred.Password

4. Ορισμός της προεπιλεγμένης NIC και να την εκχωρήσετε μια στατική διεύθυνση IP.

            Set-AzureSubnet -SubnetNames $backendSubnetName -VM $vmConfig
            Set-AzureStaticVNetIP -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig

5. Προσθέστε μια δεύτερη NIC για κάθε Εικονική.

            Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
                -SubnetName $backendSubnetName `
                -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
                -VM $vmConfig

6. Δημιουργία για να δίσκων δεδομένων για κάθε Εικονική.

            $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk1Name `
                -LUN 0       

            $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk2Name `
                -LUN 1

7. Δημιουργήστε κάθε Εικονική και τελειώνει το βρόχο.

            New-AzureVM -VM $vmConfig `
                -ServiceName $backendCSName `
                -Location $location `
                -VNetName $vnetName
        }

### <a name="step-4---run-the-script"></a>Βήμα 4 - εκτελέστε τη δέσμη ενεργειών

Τώρα που έχετε κάνει λήψη και να αλλάξει τη δέσμη ενεργειών με βάση τις ανάγκες σας, σφάλμα χρόνου εκτέλεσης αυτός δέσμη ενεργειών για να δημιουργήσετε ΣΠΣ βάσης δεδομένων παρασκηνίου με πολλές NIC.

1. Αποθηκεύστε τη δέσμη ενεργειών και εκτελέστε το από τη γραμμή εντολών **του PowerShell** ή το **PowerShell ISE**. Θα δείτε το αρχικό εξόδου, όπως φαίνεται παρακάτω.

        OperationDescription    OperationId                          OperationStatus
        --------------------    -----------                          ---------------
        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      

        WARNING: No deployment found in service: 'IaaSStory-Backend'.

2. Συμπληρώστε τις πληροφορίες που απαιτούνται στην προτροπή διαπιστευτηρίων και κάντε κλικ στο κουμπί **OK**. Θα εμφανιστεί το παρακάτω αποτέλεσμα.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
