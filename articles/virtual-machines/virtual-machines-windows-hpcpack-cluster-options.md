<properties
 pageTitle="Επιλογές σύμπλεγμα Windows HPC πακέτο στο cloud | Microsoft Azure"
 description="Μάθετε σχετικά με τις επιλογές με το Microsoft HPC πακέτου για να δημιουργήσετε και να διαχειριστείτε ένα Windows υψηλές επιδόσεις υπολογιστική σύμπλεγμα (HPC) στο Azure cloud"
 services="virtual-machines-windows,cloud-services,batch"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Επιλογές με HPC πακέτου για να δημιουργήσετε και να διαχειριστείτε ένα σύμπλεγμα Windows HPC στο Azure

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Σε αυτό το άρθρο εστιάζει στην επιλογές για τη δημιουργία πακέτου HPC συμπλεγμάτων για να εκτελέσετε φόρτους εργασίας των Windows. Υπάρχουν επίσης επιλογές για τη δημιουργία συμπλεγμάτων για να εκτελέσετε [Linux HPC φόρτους εργασίας με HPC Pack](virtual-machines-linux-hpcpack-cluster-options.md).


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Εκτέλεση ένα σύμπλεγμα HPC πακέτο στο ΣΠΣ Azure

### <a name="azure-templates"></a>Πρότυπα του Azure

* (Marketplace) [Σύμπλεγμα HPC πακέτο για φόρτους εργασίας των Windows](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)

* (Marketplace) [Σύμπλεγμα HPC πακέτο για φόρτους εργασίας του Excel](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)

* Γρήγορη (Έναρξη) [Δημιουργία ένα σύμπλεγμα HPC](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)

* Γρήγορη (Έναρξη) [Δημιουργία ένα σύμπλεγμα HPC με εικόνα κόμβο προσαρμοσμένου υπολογισμού](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure Εικονική εικόνες

* [Πακέτο HPC σε Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [Πακέτο HPC κόμβος σε Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [Πακέτο HPC τον υπολογισμό κόμβο με το Excel σε Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)



### <a name="powershell-deployment-script"></a>Δέσμη ενεργειών του PowerShell ανάπτυξης

* [Δημιουργήστε ένα σύμπλεγμα HPC με τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Προγράμματα εκμάθησης

* [Πρόγραμμα εκμάθησης: Γρήγορα αποτελέσματα με ένα σύμπλεγμα HPC πακέτο στο Azure για να εκτελέσετε φόρτους εργασίας του Excel και SOA](virtual-machines-windows-excel-cluster-hpcpack.md)



### <a name="manual-deployment-with-the-azure-portal"></a>Μη αυτόματη ανάπτυξη με την πύλη του Azure

* [Ρυθμίστε τον κόμβο κεφαλής από ένα σύμπλεγμα HPC πακέτο σε μια Εικονική Azure](virtual-machines-windows-hpcpack-cluster-headnode.md)

### <a name="cluster-management"></a>Διαχείριση συμπλέγματος

* [Διαχείριση κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)

* [Μεγέθυνση και σμίκρυνση Azure υπολογισμού πόρων σε ένα σύμπλεγμα HPC πακέτο](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)

* [Υποβολή εργασιών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Διαχείριση του έργου στο πακέτο HPC](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Προσθήκη εργασίας ρόλο κόμβους σε ένα σύμπλεγμα HPC πακέτο


* [Καταιγισμού παρουσίες Azure εργαζόμενου με HPC πακέτο](https://technet.microsoft.com/library/gg481749.aspx)

* [Πρόγραμμα εκμάθησης: Ρύθμιση ένα σύμπλεγμα υβριδική με Pack HPC στο Azure](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [Προσθήκη κόμβους Azure "καταιγισμού" σε ένα πακέτο HPC κεφαλής κόμβο Azure](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md)


## <a name="integrate-with-azure-batch"></a>Ενοποίηση με το Azure δέσμης 

* [Καταιγισμού Azure δέσμη με HPC πακέτο](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Δημιουργία συμπλεγμάτων RDMA για MPI φόρτους εργασίας

* [Ρύθμιση του ένα σύμπλεγμα Windows RDMA με το πακέτο HPC για να εκτελέσετε εφαρμογές MPI](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)
