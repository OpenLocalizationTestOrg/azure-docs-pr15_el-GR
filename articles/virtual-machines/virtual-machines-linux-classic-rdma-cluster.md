<properties
 pageTitle="Σύμπλεγμα Linux RDMA για να εκτελέσετε εφαρμογές MPI | Microsoft Azure"
 description="Δημιουργήστε ένα σύμπλεγμα Linux του μεγέθους H16r, H16mr, A8 ή ΣΠΣ A9 για χρήση του δικτύου Azure RDMA για να εκτελέσετε εφαρμογές MPI"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Ρύθμιση του ένα σύμπλεγμα Linux RDMA για να εκτελέσετε εφαρμογές MPI


Μάθετε πώς μπορείτε να ρυθμίσετε ένα σύμπλεγμα Linux RDMA στο Azure με [H σειρά ή υπολογισμού στενής ΣΠΣ μια σειρά](virtual-machines-linux-a8-a9-a10-a11-specs.md) για να εκτελέσετε παράλληλη εφαρμογές μηνύματος που περνά μέσα περιβάλλοντος εργασίας (MPI). Σε αυτό το άρθρο παρέχει τα βήματα για να προετοιμάσετε ένα Linux HPC εικόνας για να εκτελέσετε Intel MPI σε ένα σύμπλεγμα. Στη συνέχεια, μπορείτε να αναπτύξετε ένα σύμπλεγμα ΣΠΣ χρησιμοποιώντας αυτήν την εικόνα και ένα από τα μεγέθη με δυνατότητα RDMA Εικονική Azure (προς το παρόν H16r, H16mr, A8 ή A9). Χρησιμοποιήστε το σύμπλεγμα για να εκτελέσετε εφαρμογές MPI που αποτελεσματική επικοινωνία μέσω ένα μικρό λανθάνοντα, υψηλής απόδοσης δικτύου που βασίζεται σε απομακρυσμένο μνήμης απευθείας πρόσβαση τεχνολογία (RDMA).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="cluster-deployment-options"></a>Επιλογές ανάπτυξης συμπλέγματος

Ακολουθούν μεθόδους που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε ένα σύμπλεγμα Linux RDMA με ή χωρίς ένα χρονοδιάγραμμα έργου.


* **Δέσμες ενεργειών azure CLI** - όπως φαίνεται παρακάτω σε αυτό το άρθρο, χρησιμοποιήστε το [περιβάλλον γραμμής εντολών Azure](../xplat-cli-install.md) (CLI) με δέσμη ενεργειών ανάπτυξης από ένα σύμπλεγμα με δυνατότητα RDMA ΣΠΣ. Το CLI στη λειτουργία της υπηρεσίας διαχείρισης δημιουργεί σειριακά κόμβους συμπλέγματος στο μοντέλο κλασική ανάπτυξης, ώστε να την ανάπτυξη πολλά κόμβους υπολογιστικών ενδέχεται να χρειαστούν αρκετά λεπτά. Για να ενεργοποιήσετε τη σύνδεση δικτύου RDMA κατά τη χρήση του μοντέλου κλασική ανάπτυξης, ανάπτυξη του ΣΠΣ στην ίδια υπηρεσία cloud.

* **Διαχείριση πόρων azure πρότυπα** - μπορείτε επίσης να χρησιμοποιήσετε το μοντέλο ανάπτυξης για τη διαχείριση πόρων για να αναπτύξετε ένα σύμπλεγμα με δυνατότητα RDMA ΣΠΣ που συνδέεται με το δίκτυο RDMA. Μπορείτε να [δημιουργήσετε το δικό σας πρότυπο](../resource-group-authoring-templates.md), ή να ελέγξετε τα [πρότυπα Azure γρήγορης έναρξης](https://azure.microsoft.com/documentation/templates/) για πρότυπα συνεισφέρει από τη Microsoft ή την Κοινότητα για την ανάπτυξη της λύσης που θέλετε. Διαχείριση πόρων πρότυπα μπορούν να παρέχουν μια γρήγορη και αξιόπιστη τρόπος για να αναπτύξετε ένα σύμπλεγμα Linux. Για να ενεργοποιήσετε τη σύνδεση δικτύου RDMA όταν χρησιμοποιείτε το μοντέλο από διαχειριστή πόρων ανάπτυξης, ανάπτυξη του ΣΠΣ στο ίδιο σύνολο διαθεσιμότητα.

* **Πακέτο HPC** - Δημιουργήστε ένα σύμπλεγμα πακέτο HPC Microsoft στο Azure και προσθέστε με δυνατότητα RDMA κόμβους υπολογιστικών που εκτελούν μια υποστηριζόμενη κατανομή Linux για να αποκτήσετε πρόσβαση στο δίκτυο RDMA. Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-classic-model"></a>Δείγμα ανάπτυξης των βημάτων σε κλασική μοντέλο

Τα παρακάτω βήματα δείχνουν πώς μπορείτε να χρησιμοποιήσετε το CLI Azure για να αναπτύξετε μια Εικονική SUSE Linux Enterprise Server (SLES) 12 SP1 HPC από το Azure Marketplace, προσαρμόστε το και τη δημιουργία ενός προσαρμοσμένου ειδώλου Εικονική. Στη συνέχεια, χρησιμοποιήστε την εικόνα με δέσμη ενεργειών ανάπτυξης από ένα σύμπλεγμα με δυνατότητα RDMA ΣΠΣ. 

>[AZURE.TIP]Χρησιμοποιήστε παρόμοια βήματα για να αναπτύξετε ένα σύμπλεγμα με δυνατότητα RDMA ΣΠΣ που βασίζονται σε άλλες υποστηριζόμενες HPC εικόνες από το Azure Marketplace. Ορισμένα βήματα μπορεί να διαφέρει λίγο, όπως σημειώνεται. Για παράδειγμα, Intel MPI είναι περιλαμβάνονται και έχει ρυθμιστεί σε μόνο ορισμένες από αυτές τις εικόνες. Και αν αναπτύξετε μια Εικονική SLES 12 HPC αντί για μια Εικονική HPC SLES 12 SP1, πρέπει να ενημερώσετε τα προγράμματα οδήγησης RDMA. Για λεπτομέρειες, ανατρέξτε στο θέμα [σχετικά με A8, A9, A10, και A11 παρουσίες στενής υπολογισμού](virtual-machines-linux-a8-a9-a10-a11-specs.md#rdma-driver-updates-for-sles-12).


### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

*   Ο **υπολογιστής-πελάτης** - χρειάζεστε ένα Mac, Linux ή προγράμματος-πελάτη που βασίζεται σε Windows υπολογιστή για να επικοινωνήσετε με το Azure. Αυτά τα βήματα θεωρείται ότι χρησιμοποιείτε ένα πρόγραμμα-πελάτη Linux.

*   **Azure συνδρομή** - Εάν δεν έχετε μια συνδρομή, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/free/) σε λίγα λεπτά. Για μεγαλύτερη συμπλεγμάτων, εξετάστε το ενδεχόμενο διανεμητική συνδρομή ή άλλες επιλογές αγορών. 

*   **Διαθεσιμότητα μέγεθος Εικονική** - αυτήν τη στιγμή τα παρακάτω μεγέθη παρουσία είναι RDMA με δυνατότητα: H16r, H16mr, A8 και A9. Δείτε [τα προϊόντα που διατίθενται ανά περιοχή](https://azure.microsoft.com/regions/services/) για διαθεσιμότητας στο Azure περιοχές. 

*   **Όριο πυρήνων** - ίσως χρειαστεί να αυξήσετε το όριο πυρήνων για να αναπτύξετε ένα σύμπλεγμα υπολογισμού στενής ΣΠΣ. Για παράδειγμα, χρειάζεστε τουλάχιστον 128 πυρήνων εάν θέλετε να αναπτύξετε 8 A9 ΣΠΣ, όπως φαίνεται σε αυτό το άρθρο. Τη συνδρομή σας ενδέχεται να περιορίζει επίσης τον αριθμό των πυρήνων μπορείτε να αναπτύξετε σε ορισμένες Εικονική μέγεθος των οικογενειών, συμπεριλαμβανομένης της σειράς H. Για να ζητήσετε από ένα όριο αύξηση, [Ανοίξτε μια αίτηση υποστήριξης πελατών online](../azure-supportability/how-to-create-azure-support-request.md) χωρίς χρέωση. 

*   **Azure CLI** - [εγκαταστήσετε](../xplat-cli-install.md) το CLI Azure και [σύνδεση με τη συνδρομή σας στο Azure](../xplat-cli-connect.md) από τον υπολογιστή-πελάτη.


### <a name="step-1-provision-a-sles-12-sp1-hpc-vm"></a>Βήμα 1. Προμήθεια μια Εικονική HPC 12 SP1 SLES

Μετά τη σύνδεση στο Azure με το Azure CLI, εκτελέστε `azure config list` για να επιβεβαιώσετε ότι το αποτέλεσμα εμφανίζει λειτουργία διαχείριση της υπηρεσίας. Εάν δεν είναι, ορίστε την κατάσταση λειτουργίας με την εκτέλεση αυτής της εντολής:


    azure config mode asm


Πληκτρολογήστε τα εξής για να εμφανίσετε όλες τις συνδρομές που είναι εξουσιοδοτημένοι να χρησιμοποιήσετε:


    azure account list

Η τρέχουσα ενεργή συνδρομή προσδιορίζεται με `Current` οριστεί σε `true`. Εάν αυτή η συνδρομή δεν είναι αυτό που θέλετε να χρησιμοποιήσετε για να δημιουργήσετε το σύμπλεγμα, ορίστε την κατάλληλη συνδρομή αναγνωριστικό ως η ενεργή συνδρομή:

    azure account set <subscription-Id>

Για να δείτε τη διαθέσιμη στο κοινό SLES 12 SP1 HPC εικόνες στο Azure, εκτελέστε μια παρόμοια με την ακόλουθη εντολή, υπό την προϋπόθεση ότι σας υποστηρίζει περιβάλλον κελύφους **grep**:


    azure vm image list | grep "suse.*hpc"

Τώρα προμήθεια Εικονική μηχανή RDMA με δυνατότητα σύνδεσης με μια εικόνα SLES 12 SP1 HPC, εκτελώντας μια εντολή παρόμοια με τα εξής:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

όπου

* Το μέγεθος (A9 σε αυτό το παράδειγμα) είναι μία από τις διαστάσεις του με δυνατότητα RDMA Εικονική.

* Ο αριθμός θύρας εξωτερικών SSH (22 σε αυτό το παράδειγμα, το οποίο είναι η προεπιλεγμένη SSH) είναι οποιονδήποτε έγκυρο αριθμό θύρας. Τον εσωτερικό αριθμό θύρας SSH έχει οριστεί σε 22.

* Δημιουργείται μια νέα υπηρεσία στο cloud στην περιοχή Azure που καθορίζεται από την τοποθεσία. Καθορίστε μια θέση στην οποία είναι διαθέσιμο το μέγεθος Εικονική που επιλέγετε.

* Το όνομα εικόνας SLES 12 SP1 αυτήν τη στιγμή μπορεί να είναι `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824` ή `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824` για την υποστήριξη προτεραιότητα SUSE (ισχύουν πρόσθετες χρεώσεις).

### <a name="step-2-customize-the-vm"></a>Βήμα 2. Προσαρμόστε την εικονική Μηχανή

Αφού ολοκληρωθεί η Εικονική προμήθεια, SSH για την εικονική Μηχανή χρησιμοποιώντας την εικονική Μηχανή εξωτερική διεύθυνση IP (ή το όνομα DNS) και εξωτερική θύρα αριθμός που έχει ρυθμιστεί και να την προσαρμόσετε. Για λεπτομέρειες σύνδεσης, δείτε [πώς μπορείτε να συνδεθείτε στο μια εικονική μηχανή εκτελείται Linux](virtual-machines-linux-mac-create-ssh-keys.md). Εκτέλεση εντολών ως ο χρήστης που ρυθμίσατε στο η Εικονική, εκτός εάν είναι απαραίτητη η πρόσβαση στο ριζικό κατάλογο για την ολοκλήρωση του βήματος.

>[AZURE.IMPORTANT]Microsoft Azure δεν παρέχει πρόσβαση στο ριζικό σε ΣΠΣ Linux. Για να αποκτήσετε δικαιώματα πρόσβασης διαχειριστή όταν συνδεθεί ως χρήστης με δικαιώματα για την εικονική Μηχανή, εκτελούν εντολές του χρησιμοποιώντας `sudo`.

* **Ενημερώσεις** - εγκατάσταση ενημερώσεων χρησιμοποιώντας **zypper**. Μπορεί επίσης να θέλετε να εγκαταστήσετε το NFS βοηθητικών προγραμμάτων. 

    >[AZURE.IMPORTANT]Σε μια Εικονική HPC SLES 12 SP1, συνιστάται να που δεν μπορείτε να εφαρμόσετε πυρήνα ενημερώσεις, οι οποίες μπορούν να προκαλέσουν προβλήματα με την RDMA Linux προγράμματα οδήγησης.

* **Intel MPI** - ολοκλήρωση της εγκατάστασης του Intel MPI στην η Εικονική HPC SLES 12 SP1, εκτελώντας την ακόλουθη εντολή:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

* **Κλείδωμα μνήμης** - κωδικούς για MPI για να κλειδώσετε τη διαθέσιμη μνήμη για RDMA, να προσθέσετε ή να αλλάξετε τις παρακάτω ρυθμίσεις στο αρχείο /etc/security/limits.conf. (Χρειάζεστε πρόσβαση στο ριζικό για να επεξεργαστείτε αυτό το αρχείο.) 

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

    >[AZURE.NOTE]Για σκοπούς δοκιμής, μπορείτε επίσης να ορίσετε memlock για απεριόριστο. Για παράδειγμα: `<User or group name>    hard    memlock unlimited`. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γνωστά Βέλτιστες μέθοδοι για ρύθμιση κλειδωμένο μέγεθος μνήμης](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).

* **Πλήκτρα SSH για ΣΠΣ SLES** - δημιουργία SSH πλήκτρα για να δημιουργήσετε αξιοπιστίας για το λογαριασμό χρήστη σας μεταξύ των κόμβους υπολογιστικών στο σύμπλεγμα SLES κατά την εκτέλεση εργασιών MPI. (Εάν αναπτύξει μια Εικονική HPC βάσει CentOS, μην ακολουθήσετε αυτό το βήμα. Ανατρέξτε στο θέμα οδηγίες αργότερα στο άρθρο για να ρυθμίσετε passwordless SSH αξιοπιστίας μεταξύ τους κόμβους συμπλέγματος Αφού καταγράψετε την εικόνα και ανάπτυξη του συμπλέγματος.) 

    Εκτελέστε την παρακάτω εντολή για να δημιουργήσετε κλειδιά SSH. Όταν σας ζητηθεί για εισαγωγή δεδομένων, πατήστε το πλήκτρο Enter για να δημιουργήσετε τα πλήκτρα στην προεπιλεγμένη θέση χωρίς να ορίσω μια φράση πρόσβασης.

        ssh-keygen

    Προσαρτήστε το δημόσιο κλειδί στο αρχείο authorized_keys για γνωστά δημόσια κλειδιά.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    Στον κατάλογο ~/.ssh, επεξεργασία ή δημιουργία του αρχείου "config". Δώστε τη διεύθυνση IP του ιδιωτικού δικτύου που σκοπεύετε να χρησιμοποιήσετε στο Azure (10.32.0.0/16 σε αυτό το παράδειγμα):

        host 10.32.0.*
        StrictHostKeyChecking no

    Εναλλακτικά, λίστα της διεύθυνσης IP του ιδιωτικού δικτύου του κάθε Εικονική στα το σύμπλεγμά σας ως εξής:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

    >[AZURE.NOTE]Ρύθμιση παραμέτρων `StrictHostKeyChecking no` να δημιουργήσετε μια πιθανή κίνδυνος για την ασφάλεια, όταν δεν έχει καθοριστεί σε συγκεκριμένη διεύθυνση IP ή περιοχή.

* **Εφαρμογές** - εγκαταστήστε τις εφαρμογές που χρειάζεστε σε αυτή η Εικονική ή να εκτελέσετε άλλες προσαρμογές πριν να καταγράψετε την εικόνα.

### <a name="step-3-capture-the-image"></a>Βήμα 3. Καταγράψετε την εικόνα

Για να καταγράψετε την εικόνα, πρώτα εκτελέστε την ακόλουθη εντολή σε η Εικονική Linux. Αυτή η εντολή deprovisions η Εικονική αλλά διατηρεί τους λογαριασμούς χρηστών και τα πλήκτρα SSH που ορίζετε.

```
sudo waagent -deprovision
```

Στη συνέχεια, από τον υπολογιστή-πελάτη, εκτελέστε τις ακόλουθες εντολές Azure CLI για να καταγράψετε την εικόνα. Για λεπτομέρειες, ανατρέξτε στο θέμα [πώς μπορείτε να καταγράψετε μια κλασική Linux εικονική μηχανή ως εικόνα](virtual-machines-linux-classic-capture-image.md) .  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Μετά την εκτέλεση αυτών των εντολών, η εικόνα Εικονική καταγράφεται για δική σας χρήση και διαγράφεται η Εικονική. Τώρα έχετε προσαρμοσμένο την εικόνα σας έτοιμοι να αναπτύξετε ένα σύμπλεγμα.

### <a name="step-4-deploy-a-cluster-with-the-image"></a>Βήμα 4. Ανάπτυξη ένα σύμπλεγμα με την εικόνα

Τροποποιήστε την ακόλουθη δέσμη ενεργειών πάρτι με τις κατάλληλες τιμές για το περιβάλλον και την εκτέλεση από τον υπολογιστή-πελάτη. Επειδή το Azure αναπτύσσει του ΣΠΣ σειριακά στο μοντέλο κλασική ανάπτυξης, χρειάζονται μερικά λεπτά για την ανάπτυξη του 8 ΣΠΣ A9 προτείνονται σε αυτήν τη δέσμη ενεργειών.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Ζητήματα για ένα σύμπλεγμα CentOS HPC

Εάν θέλετε να ρυθμίσετε ένα σύμπλεγμα που βασίζεται σε μία από τις εικόνες με βάση CentOS HPC από το Azure Marketplace αντί για SLES 12 για HPC, ακολουθήστε τα γενικά βήματα στην προηγούμενη ενότητα. Σημειώστε τις ακόλουθες διαφορές κατά την προμήθεια και ρυθμίσετε τις παραμέτρους του Εικονική:

1. Intel MPI έχει ήδη εγκατασταθεί σε μια Εικονική παρασχεθεί από μια εικόνα που βασίζεται σε CentOS HPC. 

2. Ρυθμίσεις μνήμης κλείδωμα έχουν ήδη προστεθεί στο αρχείο /etc/security/limits.conf η Εικονική.

2. Δεν παράγουν SSH πλήκτρα σε η Εικονική που προμήθεια για καταγραφή. Αντί για αυτό, σας συνιστούμε να τη ρύθμιση του ελέγχου ταυτότητας που βασίζεται σε χρήστη μετά την ανάπτυξη του cluser. Ανατρέξτε στην επόμενη ενότητα.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Ρύθμιση του passwordless SSH αξιοπιστίας στο σύμπλεγμα

Σε ένα σύμπλεγμα HPC βάσει CentOS, υπάρχουν δύο μέθοδοι για τον προσδιορισμό σχέση αξιοπιστίας μεταξύ κόμβους υπολογιστικών: έλεγχος ταυτότητας που βασίζεται σε κεντρικού υπολογιστή και έλεγχος ταυτότητας που βασίζεται σε χρήστη. Έλεγχος ταυτότητας που βασίζεται σε κεντρικού υπολογιστή είναι εκτός της εμβέλειας αυτού του άρθρου και γενικά πρέπει να γίνουν μέσω μιας δέσμης ενεργειών επέκταση κατά την ανάπτυξη. Έλεγχος ταυτότητας που βασίζεται σε χρήστη είναι πρακτικό αξιοπιστία μετά την ανάπτυξη και απαιτεί τη δημιουργία και την κοινή χρήση των πλήκτρων SSH μεταξύ των κόμβους υπολογιστικών στο σύμπλεγμα. Αυτή η μέθοδος είναι ευρέως γνωστή ως passwordless SSH login και απαιτείται κατά την εκτέλεση εργασιών MPI. 

Ένα δείγμα δέσμης ενεργειών συνεισφέρει από την Κοινότητα είναι διαθέσιμη στο [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) για να ενεργοποιήσετε τον έλεγχο ταυτότητας χρήστη εύκολα σε ένα σύμπλεγμα HPC βάσει CentOS. Κάντε λήψη και να χρησιμοποιήσετε αυτήν τη δέσμη ενεργειών με τα παρακάτω βήματα. Μπορείτε επίσης να τροποποιήσετε αυτήν τη δέσμη ενεργειών ή να χρησιμοποιήσετε κάποια άλλη μέθοδο για να καθορίσετε τον έλεγχο ταυτότητας passwordless SSH μεταξύ τους κόμβους σύμπλεγμα υπολογισμού.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh
    
Για να εκτελέσετε τη δέσμη ενεργειών, πρέπει να γνωρίζετε το πρόθεμα για τις διευθύνσεις IP σας υποδίκτυο. Λάβετε το πρόθεμα, εκτελέστε την παρακάτω εντολή σε έναν από τους κόμβους συμπλέγματος. Το αποτέλεσμα θα πρέπει να μοιάζει με 10.1.3.5 και το πρόθεμα είναι το 10.1.3 τμήμα.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Τώρα, εκτελέστε τη δέσμη ενεργειών χρησιμοποιώντας τρεις παραμέτρους: το κοινό όνομα χρήστη σε κόμβους υπολογιστικών, το κοινό κωδικό πρόσβασης για τον συγκεκριμένο χρήστη σε κόμβους υπολογιστικών και το πρόθεμα υποδικτύου που επιστράφηκε από την προηγούμενη εντολή.


    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Αυτή η δέσμη ενεργειών κάνει τα εξής:

* Δημιουργεί έναν κατάλογο στον κόμβο του κεντρικού υπολογιστή με το όνομα .ssh, που είναι απαραίτητη για το passwordless login. 
* Δημιουργεί ένα αρχείο ρύθμισης παραμέτρων στον κατάλογο .ssh που καθοδηγεί passwordless σύνδεσης για να επιτρέψετε login από οποιονδήποτε κόμβο του συμπλέγματος. 
* Δημιουργεί αρχεία που περιέχει τα ονόματα κόμβου και οι διευθύνσεις IP κόμβου για όλους τους κόμβους του συμπλέγματος. Αυτά τα αρχεία παραμένουν μετά την εκτέλεση της δέσμης ενεργειών για μελλοντική αναφορά. 
* Δημιουργεί ένα ζεύγος ιδιωτικών και δημόσιων κλειδιών για κάθε κόμβο συμπλέγματος, συμπεριλαμβανομένου του κόμβου κεντρικού υπολογιστή και δημιουργεί εγγραφές στο αρχείο authorized_keys.

>[AZURE.WARNING]Εκτελεί αυτήν τη δέσμη ενεργειών να δημιουργήσετε ένα πιθανό κίνδυνο ασφαλείας. Βεβαιωθείτε ότι η δημόσια βασικές πληροφορίες στις ~/.ssh δεν διανέμεται.


## <a name="configure-intel-mpi"></a>Ρύθμιση παραμέτρων Intel MPI

Για να εκτελέσετε εφαρμογές MPI σε Azure Linux RDMA, πρέπει να ρυθμίσετε τις παραμέτρους ορισμένων μεταβλητές περιβάλλοντος αφορούν Intel MPI. Ακολουθεί ένα δείγμα δέσμης ενεργειών πάρτι για να ρυθμίσετε τις μεταβλητές που χρειάζονται για την εκτέλεση μιας εφαρμογής. Αλλάξτε τη διαδρομή σε mpivars.sh, όπως απαιτείται για την εγκατάσταση της Intel MPI.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Η μορφή του αρχείου κεντρικού υπολογιστή είναι ως εξής. Προσθέστε μία γραμμή για κάθε κόμβο στο σύμπλεγμά σας. Καθορίστε ιδιωτικών διευθύνσεων IP από το VNet που ορίζονται από προηγούμενες εκδόσεις, δεν ονομάτων DNS. Για παράδειγμα, για δύο κεντρικών υπολογιστών με διευθύνσεις IP 10.32.0.1 και 10.32.0.2 το αρχείο περιέχει τα εξής:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Εκτέλεση MPI σε ένα σύμπλεγμα βασικές δύο κόμβου

Εάν έχετε κάνει ήδη, ρυθμίστε πρώτα το περιβάλλον για Intel MPI. 

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-a-simple-mpi-command"></a>Εκτελέστε την εντολή απλό MPI

Εκτελέστε μια απλή εντολή MPI σε έναν από τους κόμβους υπολογισμού για να εμφανίσετε ότι MPI έχει εγκατασταθεί σωστά και να μπορούν να επικοινωνούν μεταξύ τουλάχιστον δύο τον υπολογισμό κόμβους. Η παρακάτω εντολή **mpirun** εκτελεί την εντολή **hostname** δύο κόμβους.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Το αποτέλεσμα θα πρέπει να περιλαμβάνει τα ονόματα των όλους τους κόμβους που που πέρασε σαν είσοδος για `-hosts`. Για παράδειγμα, μια εντολή **mpirun** με δύο κόμβους επιστρέφει εξόδου παρόμοια με τα εξής:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Εκτέλεση μιας αναφοράς MPI

Η ακόλουθη εντολή Intel MPI εκτελεί μια συγκριτικών pingpong για να επιβεβαιώσετε τη ρύθμιση παραμέτρων του συμπλέγματος και σύνδεση με το δίκτυο RDMA.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Σε ένα σύμπλεγμα εργάζεστε με δύο κόμβους, θα πρέπει να βλέπετε παρόμοιο με το εξής αποτέλεσμα. Στο δίκτυο Azure RDMA, περιμένετε έως 512 byte μεγέθη λανθάνων χρόνος ή κάτω από 3 μικροδευτερόλεπτα για το μήνυμα.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Επόμενα βήματα

* Δοκιμάστε την ανάπτυξη και εκτέλεση του MPI Linux εφαρμογών σε το σύμπλεγμά σας Linux.

* Ανατρέξτε στην [τεκμηρίωση βιβλιοθήκη MPI Intel](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) για οδηγίες σχετικά Intel MPI.

* Δοκιμάστε ένα [πρότυπο γρήγορης έναρξης](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) για να δημιουργήσετε ένα σύμπλεγμα Intel Lustre χρησιμοποιώντας μια εικόνα βάσει CentOS HPC. Για λεπτομέρειες, ανατρέξτε σε αυτήν [την καταχώρηση ιστολογίου](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
