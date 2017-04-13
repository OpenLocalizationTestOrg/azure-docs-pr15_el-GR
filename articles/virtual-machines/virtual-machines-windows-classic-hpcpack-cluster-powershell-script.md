<properties
   pageTitle="Δέσμη ενεργειών του PowerShell για την ανάπτυξη σύμπλεγμα Windows HPC | Microsoft Azure"
   description="Εκτέλεση μιας δέσμης ενεργειών του PowerShell για να αναπτύξετε ένα σύμπλεγμα Windows HPC Pack σε εικονικές μηχανές Windows Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Δημιουργία Windows υπολογιστική υψηλών επιδόσεων (HPC) συμπλέγματος με τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS

Εκτελέστε την ανάπτυξη IaaS πακέτο HPC δέσμη ενεργειών του PowerShell για να αναπτύξετε ένα ολοκλήρωσης σύμπλεγμα HPC για φόρτους εργασίας των Windows σε εικονικές μηχανές Windows Azure. Το σύμπλεγμα αποτελείται από έναν συνδεθεί υπηρεσίας καταλόγου Active Directory κεφαλής κόμβο, που εκτελούν Windows Server και πακέτο HPC Microsoft και Windows επιπλέον πόρους που καθορίζετε τον υπολογισμό. Εάν θέλετε να αναπτύξετε ένα σύμπλεγμα HPC πακέτο στο Azure για Linux φόρτους εργασίας, ανατρέξτε στο θέμα [Δημιουργία ένα σύμπλεγμα Linux HPC με τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Μπορείτε επίσης να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure για να αναπτύξετε ένα σύμπλεγμα HPC πακέτο. Για παραδείγματα, ανατρέξτε στο θέμα [Δημιουργία ένα σύμπλεγμα HPC](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) και [Δημιουργία ένα σύμπλεγμα HPC με μια προσαρμοσμένη τον υπολογισμό εικόνα κόμβο](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Παράδειγμα ρύθμισης παραμέτρων αρχείων

Στα παρακάτω παραδείγματα, αντικαταστήστε τις δικές σας τιμές για τη συνδρομή σας αναγνωριστικό ή όνομα και τα ονόματα λογαριασμού και της υπηρεσίας.

### <a name="example-1"></a>Παράδειγμα 1

Το παρακάτω αρχείο ρύθμισης παραμέτρων αναπτύσσει ένα σύμπλεγμα HPC πακέτο το οποίο έχει μια κεφαλή κόμβο με τοπικές βάσεις δεδομένων και πέντε τον υπολογισμό κόμβους διαθέτουν το λειτουργικό σύστημα Windows Server 2012 R2. Όλες τις υπηρεσίες cloud δημιουργούνται απευθείας στη θέση Δυτική ΗΠΑ. Ο κόμβος κεφαλής λειτουργεί ως ελεγκτή τομέα του συμπλέγματος τομέα.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Παράδειγμα 2

Το παρακάτω αρχείο ρύθμισης παραμέτρων αναπτύσσει ένα σύμπλεγμα HPC πακέτο σε ένα υπάρχον σύμπλεγμα δομών τομέα. Το σύμπλεγμα έχει 1 κεφαλής κόμβο με τοπικές βάσεις δεδομένων και 12 τον υπολογισμό κόμβους με την επέκταση BGInfo Εικονική εφαρμοστεί.
Αυτόματη εγκατάσταση ενημερώσεων των Windows είναι απενεργοποιημένη για όλα τα ΣΠΣ στο σύμπλεγμα τομέα. Όλες τις υπηρεσίες cloud δημιουργούνται απευθείας στη θέση Ανατολικής Ασίας. Κόμβους υπολογιστικών δημιουργούνται σε τρεις υπηρεσίες cloud και τρεις λογαριασμούς χώρου αποθήκευσης: _MyHPCCN 0001_ να _MyHPCCN 0005_ στο _MyHPCCNService01_ και _mycnstorage01_; _MyHPCCN 0006_ να _MyHPCCN0010_ στο _MyHPCCNService02_ και _mycnstorage02_; και _MyHPCCN 0011_ σε _MyHPCCN 0012_ στο _MyHPCCNService03_ και _mycnstorage03_). Κόμβους υπολογιστικών δημιουργούνται από μια υπάρχουσα εικόνα ιδιωτικό καταγράφονται από έναν κόμβο υπολογισμού. Αυτόματη μεγέθυνση και σμίκρυνση υπηρεσία είναι ενεργοποιημένη με προεπιλεγμένο μεγέθυνση και σμίκρυνση χρονικά διαστήματα.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Παράδειγμα 3

Το παρακάτω αρχείο ρύθμισης παραμέτρων αναπτύσσει ένα σύμπλεγμα HPC πακέτο σε ένα υπάρχον σύμπλεγμα δομών τομέα. Το σύμπλεγμα περιέχει έναν κόμβο κεφαλής, μία βάση δεδομένων διακομιστή με ένα δίσκο δεδομένων 500 GB, 2 broker κόμβους διαθέτουν το λειτουργικό σύστημα Windows Server 2012 R2 και πέντε κόμβους υπολογιστικών διαθέτουν το λειτουργικό σύστημα Windows Server 2012 R2. Η υπηρεσία cloud MyHPCCNService δημιουργείται στην ομάδα συσχέτισης *MyIBAffinityGroup*και άλλες υπηρεσίες cloud δημιουργούνται στην ομάδα συσχέτισης *MyAffinityGroup*. Το HPC εργασία Scheduler REST API και πύλης web HPC είναι ενεργοποιημένες στον κόμβο κεφαλίδας.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Παράδειγμα 4

Το παρακάτω αρχείο ρύθμισης παραμέτρων αναπτύσσει ένα σύμπλεγμα HPC πακέτο σε ένα υπάρχον σύμπλεγμα δομών τομέα. Το σύμπλεγμα έχει δύο κεφαλών κόμβο με τοπικές βάσεις δεδομένων, δημιουργούνται δύο Azure κόμβο πρότυπα και τρεις κόμβους Azure Μεσαίο μέγεθος δημιουργούνται για πρότυπο Azure κόμβο _AzureTemplate1_. Ένα αρχείο δέσμης ενεργειών εκτελείται στον κόμβο κεφαλής μετά τη ρύθμιση παραμέτρων του κεφαλής κόμβου.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων


* **Σφάλμα "Δεν υπάρχει VNet"** - εάν εκτελείτε τη δέσμη ενεργειών για την ανάπτυξη πολλών συμπλεγμάτων στο Azure ταυτόχρονα κάτω από μία συνδρομές, μία ή περισσότερες αναπτύξεις ενδέχεται να αποτύχει με το σφάλμα "VNet *VNet\_όνομα* δεν υπάρχει".
Εάν αυτό το σφάλμα παρουσιάζεται, εκτελέστε τη δέσμη ενεργειών ξανά για την ανάπτυξη αποτυχίας.

* **Προβλημάτων την πρόσβαση στο Internet από το Azure εικονικού δικτύου** - εάν μπορείτε να δημιουργήσετε ένα σύμπλεγμα με έναν νέο ελεγκτή τομέα, χρησιμοποιώντας τη δέσμη ενεργειών ανάπτυξης ή μπορείτε να προβιβάσετε με μη αυτόματο τρόπο μια κεφαλή κόμβο Εικονική ελεγκτή τομέα, ενδέχεται να αντιμετωπίσετε προβλήματα με τη σύνδεση του ΣΠΣ στο Internet. Αυτό το πρόβλημα μπορεί να προκύψει εάν ένα διακομιστή DNS προώθησης ρυθμίζεται αυτόματα στον ελεγκτή τομέα και δεν αναγνωρίζεται σωστά αυτόν το διακομιστή DNS προώθησης.

    Για να επιλύσετε αυτό το πρόβλημα, συνδεθείτε στο ελεγκτή τομέα και να καταργήσετε τη ρύθμιση παραμέτρων προώθησης ή ρύθμιση παραμέτρων ενός διακομιστή DNS έγκυρη προώθησης. Για να ρυθμίσετε τις παραμέτρους αυτής της ρύθμισης, στη Διαχείριση διακομιστή, κάντε κλικ στην επιλογή **Εργαλεία** >
    **DNS** για να ανοίξετε τη Διαχείριση DNS και, στη συνέχεια, κάντε διπλό κλικ **προωθήσεων**.

* **Προβλημάτων πρόσβαση στο δίκτυο RDMA από ΣΠΣ υπολογισμού στενής** - Εάν προσθέσετε υπολογισμού Windows Server ή broker ΣΠΣ κόμβου χρησιμοποιώντας ένα με δυνατότητα RDMA μέγεθος όπως A8 ή A9, ενδέχεται να αντιμετωπίσετε προβλήματα με τη σύνδεση αυτά τα ΣΠΣ στο δίκτυο εφαρμογής RDMA. Ένας λόγος αυτό το πρόβλημα παρουσιάζεται είναι εάν την επέκταση HpcVmDrivers δεν έχει εγκατασταθεί σωστά όταν του ΣΠΣ προστίθενται στο σύμπλεγμα. Για παράδειγμα, την επέκταση μπορεί να έχει κολλήσει σε κατάσταση κατά την εγκατάσταση.

    Για να επιλύσετε αυτό το πρόβλημα, ελέγξτε την κατάσταση της επέκτασης στις του ΣΠΣ πρώτα. Εάν η επέκταση δεν έχει εγκατασταθεί σωστά, προσπαθήστε να καταργήσετε τους κόμβους από το σύμπλεγμα HPC και, στη συνέχεια, προσθέστε ξανά τους κόμβους. Για παράδειγμα, μπορείτε να προσθέσετε κόμβος ΣΠΣ με την εκτέλεση της δέσμης ενεργειών Προσθήκη HpcIaaSNode.ps1 στον κόμβο κεφαλίδας.
    
## <a name="next-steps"></a>Επόμενα βήματα

* Προσπαθήστε να εκτελέσετε ένα φόρτο εργασίας δοκιμής στο σύμπλεγμα. Για παράδειγμα, ανατρέξτε στο θέμα το πακέτο HPC [Οδηγό για γρήγορα αποτελέσματα](https://technet.microsoft.com/library/jj884144).

* Για ένα πρόγραμμα εκμάθησης να δέσμη ενεργειών ανάπτυξης σύμπλεγμα και να εκτελέσετε μια HPC φόρτο εργασίας, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με ένα σύμπλεγμα HPC πακέτο στο Azure για να εκτελέσετε φόρτους εργασίας του Excel και SOA](virtual-machines-windows-excel-cluster-hpcpack.md).

* Δοκιμάστε ενός πακέτου HPC εργαλεία για να ξεκινήσετε, να διακόψετε, να προσθέσετε και καταργήσετε κόμβους υπολογιστικών από ένα σύμπλεγμα που δημιουργείτε. Ανατρέξτε στο θέμα [Διαχείριση κόμβους σε ένα σύμπλεγμα HPC πακέτο στο Azure τον υπολογισμό](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md).

* Για να λάβετε ρυθμιστεί ώστε να υποβάλλουν εργασίες στο σύμπλεγμα από έναν τοπικό υπολογιστή, ανατρέξτε στο θέμα [Υποβολή HPC εργασίες από έναν υπολογιστή εσωτερικής εγκατάστασης σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).
