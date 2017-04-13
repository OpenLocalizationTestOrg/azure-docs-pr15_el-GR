<properties
   pageTitle="Ανάπτυξη πολλούς NIC ΣΠΣ χρησιμοποιώντας το Azure CLI στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αναπτύξετε το ΣΠΣ NIC πολλούς χρησιμοποιώντας το Azure CLI στη Διαχείριση πόρων"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-using-the-azure-cli"></a>Ανάπτυξη πολλούς NIC ΣΠΣ χρησιμοποιώντας το CLI Azure

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][μοντέλο κλασική ανάπτυξης](virtual-network-deploy-multinic-classic-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Προς το παρόν, δεν μπορείτε να έχετε ΣΠΣ με ένα μεμονωμένο NIC και ΣΠΣ με πολλά NIC στην ίδια ομάδα πόρων. Επομένως, πρέπει να υλοποιήσετε τους διακομιστές παρασκηνίου σε μια ομάδα πόρων διαφορετικό από όλα τα άλλα στοιχεία. Τα παρακάτω βήματα Χρησιμοποιήστε μια ομάδα πόρων με το όνομα *IaaSStory* για την ομάδα κύριο πόρων και *IaaSStory παρασκηνίου* για τους διακομιστές παρασκηνίου.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να αναπτύξετε τους διακομιστές παρασκηνίου, πρέπει να αναπτύξετε την ομάδα των πόρων κύριο με όλους τους πόρους που είναι απαραίτητο για αυτό το σενάριο. Για να αναπτύξετε αυτούς τους πόρους, ακολουθήστε τα παρακάτω βήματα.

1. Μεταβείτε [στη](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)σελίδα προτύπου.
2. Στη σελίδα του προτύπου, στα δεξιά της **γονικής ομάδας πόρων**, κάντε κλικ στην επιλογή **Ανάπτυξη να Azure**.
3. Εάν είναι απαραίτητο, αλλάξτε τις τιμές παραμέτρων και, στη συνέχεια, ακολουθήστε τα βήματα στην πύλη του Azure προεπισκόπηση για να αναπτύξετε την ομάδα των πόρων.

> [AZURE.IMPORTANT] Βεβαιωθείτε ότι τα ονόματα λογαριασμού χώρου αποθήκευσης είναι μοναδικό. Δεν μπορείτε να έχετε ονόματα λογαριασμών διπλότυπων χώρου αποθήκευσης στο Azure.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Ανάπτυξη του παρασκηνίου ΣΠΣ

Το ΣΠΣ παρασκηνίου εξαρτώνται από τη δημιουργία των πόρων που παρατίθενται παρακάτω.

- **Λογαριασμό του χώρου αποθήκευσης για δίσκων δεδομένων**. Για καλύτερες επιδόσεις, των δίσκων δεδομένων στους διακομιστές βάσης δεδομένων θα χρησιμοποιήσει τεχνολογία συμπαγές κατάσταση μονάδα δίσκου (SSD), η οποία απαιτεί ένα λογαριασμό του χώρου αποθήκευσης premium. Βεβαιωθείτε ότι η θέση Azure αναπτύξετε για την υποστήριξη του χώρου αποθήκευσης premium.
- **NIC**. Κάθε Εικονική θα έχει δύο NIC, μία για πρόσβαση στη βάση δεδομένων, και μία για τη διαχείριση.
- **Ορισμός διαθεσιμότητας**. Θα προστεθούν όλους τους διακομιστές βάσης δεδομένων σε ένα μεμονωμένο διαθεσιμότητα Ορισμός, για να βεβαιωθείτε ότι τουλάχιστον ένα από τα ΣΠΣ είναι προς τα επάνω και να εκτελείται κατά τη συντήρηση.

### <a name="step-1---start-your-script"></a>Βήμα 1 - Έναρξη της δέσμης ενεργειών

Μπορείτε να κάνετε λήψη της δέσμης ενεργειών πλήρους πάρτι χρησιμοποιείται [εδώ](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh). Ακολουθήστε τα παρακάτω βήματα για να αλλάξετε τη δέσμη ενεργειών για να εργαστείτε στο περιβάλλον σας.

1. Αλλάξτε τις τιμές από τις παρακάτω μεταβλητές με βάση την υπάρχουσα ομάδα πόρων αναπτυχθεί παραπάνω στο [τις προϋποθέσεις](#Prerequisites).

        existingRGName="IaaSStory"
        location="westus"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"
        remoteAccessNSGName="NSG-RemoteAccess"

2. Αλλάξτε τις τιμές από τις παρακάτω μεταβλητές με βάση τις τιμές που θέλετε να χρησιμοποιήσετε για την ανάπτυξη παρασκηνίου.

        backendRGName="IaaSStory-Backend"
        prmStorageAccountName="wtestvnetstorageprm"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        publisher="Canonical"
        offer="UbuntuServer"
        sku="14.04.2-LTS"
        version="latest"
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskName="datadisk"
        nicNamePrefix="NICDB"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

3. Ανάκτηση το Αναγνωριστικό για το `BackEnd` υποδικτύου όπου θα δημιουργηθεί του ΣΠΣ. Πρέπει να το κάνετε αυτό, επειδή το NIC που θα συσχετιστεί με αυτό το υποδίκτυο βρίσκονται σε μια ομάδα διαφορετικό πόρο.

        subnetId="$(azure network vnet subnet show --resource-group $existingRGName \
                        --vnet-name $vnetName \
                        --name $backendSubnetName|grep Id)"
        subnetId=${subnetId#*/}

    >[AZURE.TIP] Η πρώτη εντολή παραπάνω χρησιμοποιεί [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) και [χειρισμού συμβολοσειράς](http://tldp.org/LDP/abs/html/string-manipulation.html) (πιο συγκεκριμένα, η κατάργηση δευτερεύουσας).

4. Ανάκτηση το Αναγνωριστικό για το `NSG-RemoteAccess` NSG. Πρέπει να το κάνετε αυτό, επειδή το NIC που θα συσχετιστεί με αυτό NSG βρίσκονται σε μια ομάδα διαφορετικό πόρο.

        nsgId="$(azure network nsg show --resource-group $existingRGName \
                        --name $remoteAccessNSGName|grep Id)"
        nsgId=${nsgId#*/}

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Βήμα 2 - Δημιουργία απαραίτητων πόρων για το ΣΠΣ

1. Δημιουργία νέας ομάδας πόρων για όλους τους πόρους υποστήριξης. Παρατηρήστε τη χρήση του `$backendRGName` μεταβλητής για το όνομα της ομάδας πόρων, και `$location` για την περιοχή Azure.

        azure group create $backendRGName $location

2. Δημιουργία λογαριασμού premium χώρου αποθήκευσης για το λειτουργικό σύστημα και τα δεδομένα δίσκων θα χρησιμοποιηθεί από τη δική σας ΣΠΣ.

        azure storage account create $prmStorageAccountName \
            --resource-group $backendRGName \
            --location $location \
            --type PLRS

3. Δημιουργήστε μια ρύθμιση για το ΣΠΣ διαθεσιμότητα.

        azure availset create --resource-group $backendRGName \
            --location $location \
            --name $avSetName

### <a name="step-3---create-the-nics-and-backend-vms"></a>Βήμα 3 - Δημιουργία το NIC και παρασκηνίου ΣΠΣ

1. Έναρξη ενός βρόχου για να δημιουργήσετε πολλές ΣΠΣ, με βάση το `numberOfVMs` μεταβλητές.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Για κάθε Εικονική, δημιουργήστε ένα NIC για πρόσβαση στη βάση δεδομένων.

            nic1Name=$nicNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x
            azure network nic create --name $nic1Name \
                --resource-group $backendRGName \
                --location $location \
                --private-ip-address $ipAddress1 \
                --subnet-id $subnetId

3. Για κάθε Εικονική, δημιουργήστε ένα NIC για απομακρυσμένη πρόσβαση. Ειδοποίηση του `--network-security-group` παράμετρος, που χρησιμοποιείται για να συσχετίσετε το NIC για να μια NSG.

            nic2Name=$nicNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x
            azure network nic create --name $nic2Name \
                --resource-group $backendRGName \
                --location $location \
                --private-ip-address $ipAddress2 \
                --subnet-id $subnetId $vnetName \
                --network-security-group-id $nsgId

4. Δημιουργήστε την εικονική Μηχανή.

            azure vm create --resource-group $backendRGName \
                --name $vmNamePrefix$suffixNumber \
                --location $location \
                --vm-size $vmSize \
                --subnet-id $subnetId \
                --availset-name $avSetName \
                --nic-names $nic1Name,$nic2Name \
                --os-type linux \
                --image-urn $publisher:$offer:$sku:$version \
                --storage-account-name $prmStorageAccountName \
                --storage-account-container-name vhds \
                --os-disk-vhd $osDiskName$suffixNumber.vhd \
                --admin-username $username \
                --admin-password $password

5. Για κάθε Εικονική δημιουργία δύο δίσκων δεδομένων και τελειώνει το βρόχο με το `done` εντολή.

            azure vm disk attach-new --resource-group $backendRGName \
                --vm-name $vmNamePrefix$suffixNumber \        
                --storage-account-name $prmStorageAccountName \
                --storage-account-container-name vhds \
                --vhd-name $dataDiskName$suffixNumber-1.vhd \
                --size-in-gb $diskSize \
                --lun 0

            azure vm disk attach-new --resource-group $backendRGName \
                --vm-name $vmNamePrefix$suffixNumber \        
                --storage-account-name $prmStorageAccountName \
                --storage-account-container-name vhds \
                --vhd-name $dataDiskName$suffixNumber-2.vhd \
                --size-in-gb $diskSize \
                --lun 1
        done


### <a name="step-4---run-the-script"></a>Βήμα 4 - εκτελέστε τη δέσμη ενεργειών

Τώρα που έχετε κάνει λήψη και να αλλάξει τη δέσμη ενεργειών με βάση τις ανάγκες σας, εκτελέστε τη δέσμη ενεργειών για να δημιουργήσετε ΣΠΣ βάσης δεδομένων παρασκηνίου με πολλές NIC.

1. Αποθηκεύστε τη δέσμη ενεργειών και εκτελέστε το από τερματικού **πάρτι** σας. Θα δείτε το αρχικό εξόδου, όπως φαίνεται παρακάτω.

        info:    Executing command group create
        info:    Getting resource group IaaSStory-Backend
        info:    Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command availset create
        info:    Looking up the availability set "ASDB"
        info:    Creating availability set "ASDB"
        info:    availset create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB1-DA"
        info:    Creating network interface "NICDB1-DA"
        info:    Looking up the network interface "NICDB1-DA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-DA
        data:    Name                            : NICDB1-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB1-RA"
        info:    Creating network interface "NICDB1-RA"
        info:    Looking up the network interface "NICDB1-RA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-RA
        data:    Name                            : NICDB1-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.54
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up the VM "DB1"
        info:    Using the VM Size "Standard_DS3"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account wtestvnetstorageprm
        info:    Looking up the availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up the NIC "NICDB1-DA"
        info:    Looking up the NIC "NICDB1-RA"
        info:    Creating VM "DB1"

2. Μετά από μερικά λεπτά, θα τερματιστεί η εκτέλεση και θα δείτε το υπόλοιπο του αποτελέσματος όπως φαίνεται παρακάτω.

        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB1"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-1.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB1"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-2.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB2-DA"
        info:    Creating network interface "NICDB2-DA"
        info:    Looking up the network interface "NICDB2-DA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-DA
        data:    Name                            : NICDB2-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.5
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB2-RA"
        info:    Creating network interface "NICDB2-RA"
        info:    Looking up the network interface "NICDB2-RA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-RA
        data:    Name                            : NICDB2-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.55
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up the VM "DB2"
        info:    Using the VM Size "Standard_DS3"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account wtestvnetstorageprm
        info:    Looking up the availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up the NIC "NICDB2-DA"
        info:    Looking up the NIC "NICDB2-RA"
        info:    Creating VM "DB2"
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB2"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-1.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB2"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-2.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK
