<properties
   pageTitle="Ανάπτυξη πολλούς NIC ΣΠΣ χρησιμοποιώντας το Azure CLI στο μοντέλο κλασική ανάπτυξης | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αναπτύξετε το ΣΠΣ NIC πολλούς χρησιμοποιώντας το Azure CLI στο μοντέλο κλασική ανάπτυξης"
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

#<a name="deploy-multi-nic-vms-classic-using-the-azure-cli"></a>Ανάπτυξη πολλούς NIC ΣΠΣ (κλασικό) χρησιμοποιώντας το CLI Azure

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Μπορείτε να δημιουργήσετε εικονικές μηχανές (ΣΠΣ) στο Azure και να επισυνάψετε πολλές διασυνδέσεις δικτύου (NIC) σε κάθε μία από τις ΣΠΣ. Πολλά NIC Ενεργοποίηση διαχωρισμό των τύπων κυκλοφορίας κατά μήκος NIC. Για παράδειγμα, ένα NIC μπορεί να επικοινωνήσει με στο Internet, ενώ μια άλλη επικοινωνεί μόνο με εσωτερικούς πόρους που δεν είναι συνδεδεμένοι στο Internet. Η δυνατότητα για να διαχωρίσετε κίνηση του δικτύου σε πολλά NIC απαιτείται για πολλές εικονικές συσκευές δικτύου, όπως η παράδοση εφαρμογών και λύσεις βελτιστοποίησης WAN.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας το μοντέλο από διαχειριστή πόρων](virtual-network-deploy-multinic-arm-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Προς το παρόν, δεν μπορείτε να έχετε ΣΠΣ με ένα μεμονωμένο NIC και ΣΠΣ με πολλά NIC στην ίδια υπηρεσία cloud. Επομένως, πρέπει να υλοποιήσετε τους διακομιστές παρασκηνίου σε μια υπηρεσία cloud διαφορετικό από και όλα τα άλλα στοιχεία στο σενάριο. Τα παρακάτω βήματα χρησιμοποιούν μια υπηρεσία στο cloud με το όνομα *IaaSStory* για το κύριο πόρων και *IaaSStory παρασκηνίου* για τους διακομιστές παρασκηνίου.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να αναπτύξετε τους διακομιστές παρασκηνίου, πρέπει να αναπτύξετε την υπηρεσία cloud κύριο με όλους τους πόρους που είναι απαραίτητο για αυτό το σενάριο. Τουλάχιστον, πρέπει να δημιουργήσετε ένα εικονικό δίκτυο με ένα υποδίκτυο για υπολογιστή στο παρασκήνιο. Επισκεφθείτε τη [Δημιουργία ενός εικονικού δικτύου, χρησιμοποιώντας το CLI Azure](virtual-networks-create-vnet-classic-cli.md) για να μάθετε πώς μπορείτε να αναπτύξετε ένα εικονικό δίκτυο.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Ανάπτυξη του παρασκηνίου ΣΠΣ

Το ΣΠΣ παρασκηνίου εξαρτώνται από τη δημιουργία των πόρων που παρατίθενται παρακάτω.

- **Λογαριασμό του χώρου αποθήκευσης για δίσκων δεδομένων**. Για καλύτερες επιδόσεις, των δίσκων δεδομένων στους διακομιστές βάσης δεδομένων θα χρησιμοποιήσει τεχνολογία συμπαγές κατάσταση μονάδα δίσκου (SSD), η οποία απαιτεί ένα λογαριασμό του χώρου αποθήκευσης premium. Βεβαιωθείτε ότι η θέση Azure αναπτύξετε για την υποστήριξη του χώρου αποθήκευσης premium.
- **NIC**. Κάθε Εικονική θα έχει δύο NIC, μία για πρόσβαση στη βάση δεδομένων, και μία για τη διαχείριση.
- **Ορισμός διαθεσιμότητας**. Θα προστεθούν όλους τους διακομιστές βάσης δεδομένων σε ένα μεμονωμένο διαθεσιμότητα Ορισμός, για να βεβαιωθείτε ότι τουλάχιστον ένα από τα ΣΠΣ είναι προς τα επάνω και να εκτελείται κατά τη συντήρηση.

### <a name="step-1---start-your-script"></a>Βήμα 1 - Έναρξη της δέσμης ενεργειών

Μπορείτε να κάνετε λήψη της δέσμης ενεργειών πλήρους πάρτι χρησιμοποιείται [εδώ](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Ακολουθήστε τα παρακάτω βήματα για να αλλάξετε τη δέσμη ενεργειών για να εργαστείτε στο περιβάλλον σας.

1. Αλλάξτε τις τιμές από τις παρακάτω μεταβλητές με βάση την υπάρχουσα ομάδα πόρων αναπτυχθεί παραπάνω στο [τις προϋποθέσεις](#Prerequisites).

        location="useast2"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"

2. Αλλάξτε τις τιμές από τις παρακάτω μεταβλητές με βάση τις τιμές που θέλετε να χρησιμοποιήσετε για την ανάπτυξη παρασκηνίου.

        backendCSName="IaaSStory-Backend"
        prmStorageAccountName="iaasstoryprmstorage"
        image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskPrefix="db"
        dataDiskName="datadisk"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Βήμα 2 - Δημιουργία απαραίτητων πόρων για το ΣΠΣ

1. Δημιουργήστε μια νέα υπηρεσία στο cloud για όλα τα ΣΠΣ παρασκηνίου. Παρατηρήστε τη χρήση του `$backendCSName` μεταβλητής για το όνομα της ομάδας πόρων, και `$location` για την περιοχή Azure.

        azure service create --serviceName $backendCSName \
            --location $location

2. Δημιουργία λογαριασμού premium χώρου αποθήκευσης για το λειτουργικό σύστημα και τα δεδομένα δίσκων θα χρησιμοποιηθεί από τη δική σας ΣΠΣ.

        azure storage account create $prmStorageAccountName \
            --location $location \
            --type PLRS

### <a name="step-3---create-vms-with-multiple-nics"></a>Βήμα 3 - Δημιουργία ΣΠΣ με πολλά NIC

1. Έναρξη ενός βρόχου για να δημιουργήσετε πολλές ΣΠΣ, με βάση το `numberOfVMs` μεταβλητές.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Για κάθε Εικονική, καθορίστε το όνομα και τη διεύθυνση IP του κάθε ένα από τα δύο NIC.

            nic1Name=$vmNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x

            nic2Name=$vmNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x

4. Δημιουργήστε την εικονική Μηχανή. Παρατηρήστε τη χρήση των το `--nic-config` παράμετρο, που περιέχει μια λίστα με όλα NIC με όνομα, υποδικτύου και τη διεύθυνση IP.

            azure vm create $backendCSName $image $username $password \
                --connect $backendCSName \
                --vm-name $vmNamePrefix$suffixNumber \
                --vm-size $vmSize \
                --availability-set $avSetName \
                --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
                --virtual-network-name $vnetName \
                --subnet-names $backendSubnetName \
                --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::

5. Για κάθε Εικονική, δημιουργήστε δύο δίσκων δεδομένων.

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
        done

### <a name="step-4---run-the-script"></a>Βήμα 4 - εκτελέστε τη δέσμη ενεργειών

Τώρα που έχετε κάνει λήψη και να αλλάξει τη δέσμη ενεργειών με βάση τις ανάγκες σας, εκτελέστε τη δέσμη ενεργειών για να δημιουργήσετε ΣΠΣ βάσης δεδομένων παρασκηνίου με πολλές NIC.

1. Αποθηκεύστε τη δέσμη ενεργειών και εκτελέστε το από τερματικού **πάρτι** σας. Θα δείτε το αρχικό εξόδου, όπως φαίνεται παρακάτω.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Μετά από μερικά λεπτά, θα τερματιστεί η εκτέλεση και θα δείτε το υπόλοιπο του αποτελέσματος όπως φαίνεται παρακάτω.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
