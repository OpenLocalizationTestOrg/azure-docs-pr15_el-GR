<properties
 pageTitle="Επιλογές σύμπλεγμα Linux HPC πακέτο στο cloud | Microsoft Azure"
 description="Μάθετε σχετικά με τις επιλογές με το Microsoft HPC πακέτου για να δημιουργήσετε και να διαχειριστείτε ένα υψηλές επιδόσεις Linux υπολογιστική σύμπλεγμα (HPC) στο Azure cloud"
 services="virtual-machines-linux,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Επιλογές με HPC πακέτου για να δημιουργήσετε και να διαχειριστείτε ένα σύμπλεγμα HPC στο Azure για Linux φόρτους εργασίας

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Σε αυτό το άρθρο εστιάζει σε επιλογές για να χρησιμοποιήσετε HPC πακέτου για να εκτελέσετε Linux φόρτους εργασίας. Υπάρχουν επίσης επιλογές για την εκτέλεση [Windows HPC φόρτους εργασίας με HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Εκτέλεση ένα σύμπλεγμα HPC πακέτο στο ΣΠΣ Azure

### <a name="azure-templates"></a>Πρότυπα του Azure


* (Marketplace) [Σύμπλεγμα HPC πακέτο για Linux φόρτους εργασίας](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)

* Γρήγορη (Έναρξη) [Δημιουργία ένα σύμπλεγμα HPC με Linux τον υπολογισμό κόμβοι](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)


### <a name="powershell-deployment-script"></a>Δέσμη ενεργειών του PowerShell ανάπτυξης

* [Δημιουργήστε ένα σύμπλεγμα Linux HPC με τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Προγράμματα εκμάθησης

* [Πρόγραμμα εκμάθησης: Γρήγορα αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-linux-classic-hpcpack-cluster.md)

* [Πρόγραμμα εκμάθησης: Εκτέλεση NAMD με το πακέτο HPC Microsoft στη Linux τον υπολογισμό κόμβους στο Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Πρόγραμμα εκμάθησης: Εκτέλεση OpenFOAM με το Microsoft HPC πακέτο σε ένα σύμπλεγμα Linux RDMA στο Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Πρόγραμμα εκμάθησης: Εκτέλεση ΑΣΤΈΡΙ-ΚΦΜ + με το Microsoft HPC πακέτο σε μια RDMA Linux συμπλέγματος στο Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

### <a name="cluster-management"></a>Διαχείριση συμπλέγματος

* [Υποβολή εργασιών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Διαχείριση του έργου στο πακέτο HPC](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="create-rdma-clusters-for-mpi-workloads"></a>Δημιουργία συμπλεγμάτων RDMA για MPI φόρτους εργασίας

* [Πρόγραμμα εκμάθησης: Εκτέλεση OpenFOAM με το Microsoft HPC πακέτο σε ένα σύμπλεγμα Linux RDMA στο Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Ρύθμιση του ένα σύμπλεγμα Linux RDMA για να εκτελέσετε εφαρμογές MPI](virtual-machines-linux-classic-rdma-cluster.md)

