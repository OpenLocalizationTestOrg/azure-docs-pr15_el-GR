<properties
   pageTitle="Δέσμη ενεργειών του PowerShell για την ανάπτυξη σύμπλεγμα Linux HPC | Microsoft Azure"
   description="Εκτέλεση μιας δέσμης ενεργειών του PowerShell για να αναπτύξετε ένα σύμπλεγμα Linux HPC πακέτο σε εικονικές μηχανές Windows Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Δημιουργήστε μια υψηλές επιδόσεις Linux υπολογιστική σύμπλεγμα (HPC) με τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS

Εκτελέστε την ανάπτυξη IaaS πακέτο HPC δέσμη ενεργειών του PowerShell για να αναπτύξετε ένα ολοκλήρωσης σύμπλεγμα HPC για Linux φόρτους εργασίας σε εικονικές μηχανές Windows Azure. Το σύμπλεγμα αποτελείται από μια κεφαλή κόμβου συνδεθεί υπηρεσίας καταλόγου Active Directory στην εκτέλεση Windows Server και πακέτο HPC Microsoft και κόμβους υπολογιστικών που εκτελούνται σε ένα από τα κατανομές Linux που υποστηρίζονται από το πακέτο HPC. Εάν θέλετε να αναπτύξετε ένα σύμπλεγμα HPC πακέτο στο φόρτους εργασίας του Azure για Windows, ανατρέξτε στο θέμα [Δημιουργία ένα σύμπλεγμα Windows HPC με τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). Μπορείτε επίσης να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure για να αναπτύξετε ένα σύμπλεγμα HPC πακέτο. Για παράδειγμα, ανατρέξτε στο θέμα [Δημιουργία ένα σύμπλεγμα HPC με Linux τον υπολογισμό κόμβους](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Παράδειγμα αρχείου ρύθμισης παραμέτρων

Το παρακάτω αρχείο ρύθμισης παραμέτρων δημιουργεί ένα νέο ελεγκτή τομέα και δάσος τομέα και αναπτύσσει ένα σύμπλεγμα HPC πακέτο που έχει 1 κεφαλής κόμβο με τοπικές βάσεις δεδομένων και 10 κόμβους υπολογιστικών Linux. Όλες τις υπηρεσίες cloud δημιουργούνται απευθείας στη θέση Ανατολικής Ασίας. Κόμβους υπολογιστικών Linux δημιουργούνται στο 2 υπηρεσιών cloud και 2 λογαριασμών χώρου αποθήκευσης (δηλαδή _MyLnxCN 0001_ να _MyLnxCN 0005_ στο _MyLnxCNService01_ και _mylnxstorage01_) και _MyLnxCN 0006_ να _MyLnxCN 0010_ στο _MyLnxCNService02_ και _mylnxstorage02_. Κόμβους υπολογιστικών δημιουργούνται από μια εικόνα OpenLogic CentOS έκδοση 7.0 Linux. 

Αντικαταστήστε τις δικές σας τιμές για το όνομα της συνδρομής σας και τα ονόματα λογαριασμού και υπηρεσίας.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

* **Σφάλμα "Δεν υπάρχει VNet"** - εάν εκτελείτε τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS για την ανάπτυξη πολλών συμπλεγμάτων στο Azure ταυτόχρονα κάτω από μία συνδρομές, μία ή περισσότερες αναπτύξεις ενδέχεται να αποτύχει με το σφάλμα "VNet *VNet\_όνομα* δεν υπάρχει".
Εάν αυτό το σφάλμα παρουσιάζεται, εκτελέστε ξανά τη δέσμη ενεργειών για την ανάπτυξη αποτυχίας.

* **Προβλημάτων την πρόσβαση στο Internet από το Azure εικονικού δικτύου** - εάν μπορείτε να δημιουργήσετε ένα σύμπλεγμα HPC πακέτο με έναν νέο ελεγκτή τομέα, χρησιμοποιώντας τη δέσμη ενεργειών ανάπτυξης ή μπορείτε να προβιβάσετε με μη αυτόματο τρόπο μια κεφαλή κόμβο Εικονική ελεγκτή τομέα, ενδέχεται να αντιμετωπίσετε προβλήματα με τη σύνδεση του ΣΠΣ στο Azure εικονικό δίκτυο στο Internet. Αυτό μπορεί να συμβεί εάν ένα διακομιστή DNS προώθησης ρυθμίζεται αυτόματα στον ελεγκτή τομέα και δεν αναγνωρίζεται σωστά αυτόν το διακομιστή DNS προώθησης.

    Για να επιλύσετε αυτό το πρόβλημα, συνδεθείτε στο ελεγκτή τομέα και να καταργήσετε τη ρύθμιση παραμέτρων προώθησης ή ρύθμιση παραμέτρων ενός διακομιστή DNS έγκυρη προώθησης. Για να το κάνετε αυτό, στο διακομιστή Manager, κάντε κλικ στην επιλογή **Εργαλεία** >
    **DNS** για να ανοίξετε τη Διαχείριση DNS και, στη συνέχεια, κάντε διπλό κλικ **προωθήσεων**.
    
## <a name="next-steps"></a>Επόμενα βήματα

* Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-linux-classic-hpcpack-cluster.md) για πληροφορίες σχετικά με τις υποστηριζόμενες Linux κατανομές, μετακίνηση δεδομένων και υποβολή εργασιών σε ένα σύμπλεγμα HPC πακέτο με Linux τον υπολογισμό κόμβους.
* Για προγράμματα εκμάθησης που χρησιμοποιούν τη δέσμη ενεργειών για να δημιουργήσετε ένα σύμπλεγμα και να εκτελέσετε ένα φόρτο εργασίας Linux HPC, ανατρέξτε στα θέματα:
    * [Εκτέλεση NAMD με το πακέτο του Microsoft HPC σε Linux υπολογισμού τους κόμβους Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [Εκτέλεση OpenFOAM με το πακέτο του Microsoft HPC σε Linux υπολογισμού τους κόμβους Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [Εκτέλεση ΑΣΤΈΡΙ-ΚΦΜ + με πακέτο HPC Microsoft στην Linux τον υπολογισμό κόμβους στο Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)
