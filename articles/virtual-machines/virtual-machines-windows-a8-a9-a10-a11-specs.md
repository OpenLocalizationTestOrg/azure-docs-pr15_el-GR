<properties
 pageTitle="Σχετικά με το ΣΠΣ υπολογισμού στενής με Windows | Microsoft Azure"
 description="Λήψη πληροφοριών φόντο και θέματα για τη χρήση του Azure μεγέθη υπολογισμού στενής H σειρά και A8, A9, A10 και A11 για τις υπηρεσίες Windows ΣΠΣ και cloud"
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Σχετικά με τη σειρά H και υπολογισμού στενής ΣΠΣ μια σειρά

Παρακάτω θα δείτε πληροφορίες φόντου και ορισμένα ζητήματα σχετικά με τη χρήση τη νεότερη Azure H σειρά και τις προηγούμενες A8, A9, A10 και A11 παρουσίες, γνωστές και ως *υπολογισμού στενής* παρουσίες. Σε αυτό το άρθρο εστιάζει σχετικά με αυτές τις περιπτώσεις για Windows ΣΠΣ. Σε αυτό το άρθρο είναι επίσης διαθέσιμο για [ΣΠΣ Linux](virtual-machines-linux-a8-a9-a10-a11-specs.md).


[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Πρόσβαση στο δίκτυο RDMA

Μπορείτε να δημιουργήσετε συμπλεγμάτων παρουσιών με δυνατότητα RDMA Windows Server και να αναπτύξετε ένα από τα υποστηριζόμενα υλοποιήσεις MPI για να επωφεληθείτε από το δίκτυο Azure RDMA. Αυτό το δίκτυο χαμηλής αδράνειας, υψηλής ταχύτητας μετάδοσης δεσμεύεται για την κίνηση MPI μόνο.

* **Λειτουργικό σύστημα**
    * **Εικονικές μηχανές** - Windows Server 2012 R2, Windows Server 2012
    * **Υπηρεσίες cloud** - Windows Server 2012 R2, Windows Server 2012, συγγενείς Windows Server 2008 R2 επισκέπτη λειτουργικό σύστημα

* **MPI** - MPI της Microsoft (MS-MPI) 2012 R2 ή νεότερη έκδοση, Intel MPI βιβλιοθήκη 5.x

Υποστηριζόμενες εφαρμογές MPI χρησιμοποιούν το περιβάλλον εργασίας του Microsoft δικτύου απευθείας για την επικοινωνία μεταξύ παρουσιών. Ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα RDMA των Windows με το πακέτο HPC για να εκτελέσετε εφαρμογές MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) και [χρήση πολλών παρουσιών εργασίες για να εκτελέσετε εφαρμογές μηνύματος που περνά μέσα περιβάλλοντος εργασίας (MPI) σε Azure δέσμη](../batch/batch-mpi.md) για επιλογές ανάπτυξης και της ρύθμισης παραμέτρων δείγματος.


>[AZURE.NOTE]Σε με δυνατότητα RDMA υπολογισμού στενής VM, την επέκταση HpcVmDrivers πρέπει να προστεθεί στις του ΣΠΣ για να εγκαταστήσετε το Windows δικτύου προγράμματα οδήγησης των συσκευών που είναι απαραίτητα για τη σύνδεση RDMA. Περισσότερες αναπτύξεις, την επέκταση HpcVmDrivers προστίθεται αυτόματα. Εάν θέλετε να προσθέσετε την επέκταση τον εαυτό σας, ανατρέξτε στο θέμα [Διαχείριση Εικονική επεκτάσεις](virtual-machines-windows-classic-manage-extensions.md).

## <a name="considerations-for-hpc-pack-and-windows"></a>Θέματα HPC πακέτο και Windows

[Πακέτο HPC Microsoft](https://technet.microsoft.com/library/jj899572.aspx), δωρεάν σύμπλεγμα HPC της Microsoft και λύσης διαχείρισης έργων, δεν είναι απαραίτητο για να χρησιμοποιήσετε τις παρουσίες υπολογισμού στενής με τον Windows Server. Ωστόσο, είναι μία επιλογή για να δημιουργήσετε ένα σύμπλεγμα υπολογισμού στο Azure για να εκτελέσετε εφαρμογές MPI που βασίζεται σε Windows και άλλες HPC φόρτους εργασίας. HPC πακέτο 2012 R2 και νεότερες εκδόσεις περιλαμβάνουν ένα περιβάλλον χρόνου εκτέλεσης για MS-MPI που μπορούν να χρησιμοποιούν το δίκτυο Azure RDMA όταν αναπτυχθεί σε με δυνατότητα RDMA VM.

Για περισσότερες πληροφορίες και λίστες ελέγχου για να χρησιμοποιήσετε τις παρουσίες υπολογισμού στενής με πακέτο HPC σε Windows Server, ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα Windows RDMA με το πακέτο HPC για να εκτελέσετε MPI εφαρμογές](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).




## <a name="next-steps"></a>Επόμενα βήματα

* Για λεπτομέρειες σχετικά με τη διαθεσιμότητα και τις τιμές από τα μεγέθη στενής υπολογισμού, ανατρέξτε στο θέμα [εικονικές μηχανές τιμές](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) και [τις τιμές τις υπηρεσίες Cloud](https://azure.microsoft.com/pricing/details/cloud-services/).

* Για χωρητικότητα αποθήκευσης και δίσκου λεπτομέρειες, ανατρέξτε στο θέμα [μεγέθη για εικονικές μηχανές](virtual-machines-linux-sizes.md).

* Για να ξεκινήσετε την ανάπτυξη και τη χρήση υπολογισμού στενής παρουσίες με πακέτο HPC στα Windows, ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα Windows RDMA με το πακέτο HPC για να εκτελέσετε MPI εφαρμογές](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).

* Για πληροφορίες σχετικά με τη χρήση A8 και A9 παρουσίες για να εκτελέσετε εφαρμογές MPI με τη μαζική Azure, ανατρέξτε στο θέμα [χρήση πολλών παρουσιών εργασίες για να εκτελέσετε εφαρμογές μηνύματος που περνά μέσα περιβάλλοντος εργασίας (MPI) σε δέσμη Azure](../batch/batch-mpi.md).
