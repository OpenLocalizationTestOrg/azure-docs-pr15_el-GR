<properties
 pageTitle="Σχετικά με το ΣΠΣ υπολογισμού στενής με Linux | Microsoft Azure"
 description="Λήψη πληροφοριών φόντου και ζητήματα σχετικά με τα μεγέθη υπολογισμού στενής H σειρά και A8, A9, A10 και A11 για ΣΠΣ Linux"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Σχετικά με τη σειρά H και υπολογισμού στενής ΣΠΣ μια σειρά 

Παρακάτω θα δείτε πληροφορίες φόντου και ορισμένα ζητήματα σχετικά με τη χρήση τη νεότερη Azure H σειρά και προηγούμενες μεγέθη A8, A9, A10 και A11, γνωστές και ως *υπολογισμού στενής* παρουσίες. Σε αυτό το άρθρο εστιάζει σχετικά με αυτά τα μεγέθη για ΣΠΣ Linux. Σε αυτό το άρθρο είναι επίσης διαθέσιμες για [Τα Windows ΣΠΣ](virtual-machines-windows-a8-a9-a10-a11-specs.md).




[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Πρόσβαση στο δίκτυο RDMA

Μπορείτε να δημιουργήσετε συμπλεγμάτων με δυνατότητα RDMA Linux ΣΠΣ που εκτελέστε μία από τις ακόλουθες υποστήριξη κατανομές Linux HPC και μια υποστηριζόμενη υλοποίηση MPI για να επωφεληθείτε από το δίκτυο Azure RDMA. Ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα Linux RDMA για να εκτελέσετε εφαρμογές MPI](virtual-machines-linux-classic-rdma-cluster.md) για επιλογές ανάπτυξης και της ρύθμισης παραμέτρων δείγματος.

* **Κατανομή μεταξύ** - πρέπει να αναπτύξετε ΣΠΣ από εικόνες Linux της με δυνατότητα RDMA SUSE διακομιστή για μεγάλες επιχειρήσεις-ευρώ (SLES) ή βάσει OpenLogic CentOS HPC από το Azure Marketplace. Μόνο οι παρακάτω εικόνες Marketplace υποστηρίζουν τα απαραίτητα προγράμματα οδήγησης Linux RDMA:

    * SLES 12 SP1 για HPC, SLES 12 SP1 για HPC (Premium)
    
    * SLES 12 για HPC, SLES 12 για HPC (Premium)
    
    * Βάσει centOS 7.1 HPC
    
    * Βάσει centOS 6.5 HPC
    
    >[AZURE.NOTE]Για ΣΠΣ H σειρά, συνιστάται να είτε ένα SP1 12 SLES HPC εικόνας ή εικόνων που CentOS HPC 7.1.
    >
    >Στην τις εικόνες με βάση CentOS HPC, ενημερώσεις πυρήνα έχουν απενεργοποιηθεί στο αρχείο παραμέτρων **yum** . Αυτό συμβαίνει επειδή τα προγράμματα οδήγησης Linux RDMA διανέμονται ως πακέτο RPM και ενημερωμένες εκδόσεις προγραμμάτων οδήγησης ενδέχεται να μην λειτουργούν εάν το πυρήνα είναι ενημερωθεί.

* **MPI** - Intel MPI βιβλιοθήκη 5.x

    Ανάλογα με την εικόνα Marketplace επιλέξετε, ξεχωριστή άδεια, εγκατάσταση, ή ρύθμιση των παραμέτρων του Intel MPI ίσως φανούν χρήσιμες, ως εξής: 
    
    * **SLES 12 SP1 για εικόνα HPC** - εγκατάσταση τα πακέτα Intel MPI κατανεμημένο σε η Εικονική, εκτελώντας την ακόλουθη εντολή:
    
            sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

    * **12 SLES για εικόνα HPC** - ξεχωριστά πρέπει να εγγραφείτε για να κάνετε λήψη και εγκατάσταση Intel MPI. Ανατρέξτε στον [Οδηγό εγκατάστασης Intel MPI βιβλιοθήκης](https://software.intel.com/sites/default/files/managed/7c/2c/intelmpi-2017-installguide-linux.pdf).
    
    * **Εικόνες με βάση centOS HPC** - Intel MPI 5.1 είναι ήδη εγκατεστημένο.  

    Για να εκτελέσετε εργασίες MPI σε ομαδοποιημένων VM απαιτείται πρόσθετο σύστημα ρύθμιση. Για παράδειγμα, σε ένα σύμπλεγμα ΣΠΣ, πρέπει να δημιουργήσουν σχέση αξιοπιστίας μεταξύ τους κόμβους υπολογισμού. Για τυπικές ρυθμίσεις, ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα Linux RDMA για να εκτελέσετε MPI εφαρμογές](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="considerations-for-hpc-pack-and-linux"></a>Θέματα HPC πακέτο και Linux

[Πακέτο HPC](https://technet.microsoft.com/library/jj899572.aspx), δωρεάν σύμπλεγμα HPC της Microsoft και λύσης διαχείρισης έργων, παρέχει μια επιλογή για να χρησιμοποιήσετε τις παρουσίες υπολογισμού στενής με Linux. Τις πιο πρόσφατες εκδόσεις υποστήριξης HPC πακέτο 2012 R2 κατανομή μεταξύ πολλών Linux να εκτελέσετε στο υπολογίσετε κόμβους αναπτυχθεί στο ΣΠΣ Azure, ελέγχονται από έναν κόμβο κεφαλής Windows Server. Με κόμβους υπολογιστικών με δυνατότητα RDMA Linux εκτελείται Intel MPI, πακέτο HPC να προγραμματίσετε και να εκτελέσετε Linux MPI εφαρμογές που έχουν πρόσβαση στο δίκτυο RDMA. Για να ξεκινήσετε, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="network-topology-considerations"></a>Ζητήματα τοπολογίας δικτύου

* Σε με δυνατότητα RDMA VM Linux στο Azure Eth1 δεσμεύεται για την κίνηση δικτύου RDMA. Μην αλλάξετε τις ρυθμίσεις Eth1 ή τις πληροφορίες στο αρχείο ρύθμισης παραμέτρων που αναφέρονται σε αυτό το δίκτυο. Eth0 δεσμεύεται για κανονική Azure κίνηση του δικτύου.

* Στο Azure, IP μέσω InfiniBand (i β) δεν υποστηρίζεται. Υποστηρίζεται μόνο RDMA μέσω i β.

## <a name="rdma-driver-updates-for-sles-12"></a>RDMA ενημερωμένες εκδόσεις προγραμμάτων οδήγησης για SLES 12

Αφού δημιουργήσετε μια Εικονική που βασίζεται σε μια εικόνα SLES 12 HPC, ίσως χρειαστεί να ενημερώσετε τα προγράμματα οδήγησης RDMA του ΣΠΣ για RDMA η συνδεσιμότητα του δικτύου. 

>[AZURE.IMPORTANT]Αυτό το βήμα είναι **απαιτείται** για το SLES 12 για αναπτύξεις Εικονική HPC σε όλες τις περιοχές Azure. 
>Αυτό το βήμα δεν χρειάζεται Εάν αναπτύξετε μια SP1 12 SLES για HPC, βάσει CentOS 7.1 HPC ή βάσει CentOS 6.5 HPC Εικονική. 

Πριν να ενημερώσετε τα προγράμματα οδήγησης, διακόψτε όλες τις διεργασίες **zypper** ή τις διαδικασίες που κλείδωμα των βάσεων δεδομένων repo SUSE σε η Εικονική. Διαφορετικά τα προγράμματα οδήγησης ενδέχεται να μην ενημέρωσης σωστά.  

Για να ενημερώσετε τα προγράμματα οδήγησης Linux RDMA σε κάθε Εικονική, εκτελέστε ένα από τα ακόλουθα σύνολα εντολών Azure CLI από υπολογιστή-πελάτη σας.

**12 SLES για Εικονική HPC παρασχεθεί στο μοντέλο κλασική ανάπτυξης**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**12 SLES για Εικονική HPC παρασχεθεί στο μοντέλο ανάπτυξης για τη διαχείριση πόρων**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]Ενδέχεται να χρειαστεί κάποιος χρόνος για να εγκαταστήσετε τα προγράμματα οδήγησης και επιστρέφει την εντολή χωρίς αποτέλεσμα. Μετά την ενημέρωση, σας Εικονική θα γίνει επανεκκίνηση και πρέπει να είστε έτοιμοι για χρήση σε μερικά λεπτά.

### <a name="sample-script-for-driver-updates"></a>Δείγμα δέσμης ενεργειών για ενημερωμένες εκδόσεις προγραμμάτων οδήγησης

Εάν έχετε ένα σύμπλεγμα 12 SLES για HPC ΣΠΣ, μπορείτε να δέσμη ενεργειών την ενημέρωση του προγράμματος οδήγησης σε όλους τους κόμβους στο σύμπλεγμά σας. Για παράδειγμα, η ακόλουθη δέσμη ενεργειών ενημερώσεις των προγραμμάτων οδήγησης σε ένα σύμπλεγμα 8-κόμβο.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Επόμενα βήματα

* Για λεπτομέρειες σχετικά με τη διαθεσιμότητα και τις τιμές από τα μεγέθη στενής υπολογισμού, ανατρέξτε στο θέμα [εικονικές μηχανές τις πληροφορίες τιμολόγησης](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).

* Για χωρητικότητα αποθήκευσης και δίσκου λεπτομέρειες, ανατρέξτε στο θέμα [μεγέθη για εικονικές μηχανές](virtual-machines-linux-sizes.md).

* Για να ξεκινήσετε την ανάπτυξη και τη χρήση υπολογισμού στενής μεγέθη με RDMA στην Linux, ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα Linux RDMA για να εκτελέσετε MPI εφαρμογές](virtual-machines-linux-classic-rdma-cluster.md).


