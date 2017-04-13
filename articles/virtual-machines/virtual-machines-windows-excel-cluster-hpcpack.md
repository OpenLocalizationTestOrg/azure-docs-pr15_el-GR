<properties
 pageTitle="Σύμπλεγμα HPC πακέτου για το Excel και SOA | Microsoft Azure"
 description="Γρήγορα αποτελέσματα εκτέλεση ευρείας κλίμακας Excel και SOA φόρτους εργασίας σε ένα σύμπλεγμα HPC πακέτο στο Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/25/2016"
 ms.author="danlep"/>

# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Γρήγορα αποτελέσματα εκτέλεση φόρτους εργασίας του Excel και SOA σε ένα σύμπλεγμα HPC πακέτο στο Azure

Αυτό το άρθρο σας δείχνει πώς μπορείτε να αναπτύξετε ένα σύμπλεγμα πακέτο HPC Microsoft στην Azure εικονικές μηχανές, χρησιμοποιώντας ένα πρότυπο Azure γρήγορη έναρξη ή προαιρετικά μια δέσμη ενεργειών ανάπτυξης Azure PowerShell. Το σύμπλεγμα χρησιμοποιεί Azure Marketplace Εικονική εικόνες που έχει σχεδιαστεί για να εκτελέσετε το Microsoft Excel ή φόρτους εργασίας προσανατολισμένος σε υπηρεσία αρχιτεκτονική (SOA) με το πακέτο HPC. Μπορείτε να χρησιμοποιήσετε το σύμπλεγμα για να εκτελέσετε απλές HPC Excel και των υπηρεσιών SOA από έναν υπολογιστή-πελάτη εσωτερικής εγκατάστασης. Οι υπηρεσίες Excel HPC περιλαμβάνουν μείωση φόρτου βιβλίο εργασίας του Excel και συναρτήσεων που ορίζονται από το χρήστη του Excel ή UDF.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Σε υψηλό επίπεδο, το παρακάτω διάγραμμα παρουσιάζει το σύμπλεγμα HPC πακέτο που δημιουργείτε.

![Σύμπλεγμα HPC με κόμβους εκτελείται φόρτους εργασίας του Excel][scenario]

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

*   Ο **υπολογιστής-πελάτης** - χρειάζεστε έναν υπολογιστή-πελάτη που βασίζεται σε Windows για να υποβάλετε εργασίες Excel και SOA δείγμα στο σύμπλεγμα. Χρειάζεστε επίσης έναν υπολογιστή Windows για να εκτελέσετε τη δέσμη ενεργειών Azure PowerShell σύμπλεγμα ανάπτυξης (Εάν επιλέξετε αυτήν τη μέθοδο ανάπτυξης) και

*   **Azure συνδρομή** - Εάν δεν έχετε μια συνδρομή του Azure, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/) σε λίγα λεπτά.

*   **Όριο πυρήνων** - ίσως χρειαστεί να αυξήσετε το όριο πυρήνων, ειδικά εάν αναπτύξετε πολλές κόμβοι συμπλέγματος με πολλών πυρήνων μεγέθη Εικονική. Εάν χρησιμοποιείτε ένα πρότυπο Azure γρήγορη έναρξη, του ορίου πυρήνων στη Διαχείριση πόρων είναι ανά περιοχή Azure. Σε αυτή την περίπτωση, ίσως χρειαστεί να αυξήσετε το όριο σε μια συγκεκριμένη περιοχή. Ανατρέξτε στο θέμα [όρια Azure συνδρομή, όρια, και τους περιορισμούς](../azure-subscription-service-limits.md). Για να αυξήσετε ένα όριο, [Ανοίξτε μια αίτηση υποστήριξης πελατών online](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) χωρίς χρέωση.

*   **Άδεια χρήσης του Microsoft Office** - Εάν αναπτύξετε κόμβους υπολογιστικών χρησιμοποιώντας μια Εικονική πακέτο HPC Marketplace εικόνα με το Microsoft Excel, μια έκδοση 30 ημερών αξιολόγησης του Microsoft Excel Professional Plus 2013 είναι εγκατεστημένη. Μετά την τελεία αξιολόγησης, πρέπει να δώσετε μια έγκυρη άδεια χρήσης του Microsoft Office για να ενεργοποιήσετε το Excel για να συνεχίσετε να εκτελέσετε φόρτους εργασίας. Ανατρέξτε στο θέμα [Ενεργοποίηση του Excel](#excel-activation) αργότερα σε αυτό το άρθρο. 


## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Βήμα 1. Ρυθμίστε ένα σύμπλεγμα HPC πακέτο στο Azure

Θα σας δείξουμε δύο επιλογές για να ρυθμίσετε το σύμπλεγμα: πρώτα, χρησιμοποιώντας ένα πρότυπο Azure γρήγορης έναρξης και την πύλη του Azure; και τα δευτερόλεπτα, χρησιμοποιώντας μια δέσμη ενεργειών ανάπτυξης Azure PowerShell.


### <a name="option-1-use-a-quickstart-template"></a>Επιλογή 1. Χρησιμοποιήστε ένα πρότυπο γρήγορη έναρξη
Χρήση προτύπου Azure γρήγορης έναρξης για ένα σύμπλεγμα HPC πακέτο στην πύλη του Azure αναπτύσσουν γρήγορα και εύκολα. Όταν ανοίγετε το πρότυπο στην πύλη, λαμβάνετε μια απλή UI όπου μπορείτε να εισαγάγετε τις ρυθμίσεις για το σύμπλεγμά σας. Ακολουθούν τα βήματα. 

>[AZURE.TIP]Εάν θέλετε, χρησιμοποιήστε ένα [πρότυπο Azure Marketplace](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) που δημιουργεί ένα σύμπλεγμα παρόμοια ειδικά για φόρτους εργασίας του Excel. Τα βήματα διαφέρουν λίγο από τα εξής.

1.  Επισκεφθείτε τη [σελίδα Δημιουργία σύμπλεγμα HPC πρότυπο GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Εάν θέλετε, διαβάστε πληροφορίες σχετικά με το πρότυπο και τον κωδικό προέλευσης.

2.  Κάντε κλικ στην επιλογή **Ανάπτυξη να Azure** για να ξεκινήσετε μια ανάπτυξη με το πρότυπο στην πύλη του Azure.

    ![Ανάπτυξη προτύπου Azure][github]

3.  Στην πύλη, ακολουθήστε τα παρακάτω βήματα για να εισαγάγετε τις παραμέτρους για το πρότυπο σύμπλεγμα HPC.

    μια. Στη σελίδα " **παράμετροι** ", πληκτρολογήστε ή τροποποιήστε τις τιμές για τις παραμέτρους του προτύπου. (Κάντε κλικ στο εικονίδιο δίπλα σε κάθε ρύθμιση πληροφορίες Βοήθειας). Δείγμα τιμές εμφανίζονται στην παρακάτω οθόνη. Αυτό το παράδειγμα δημιουργεί ένα σύμπλεγμα με το όνομα *hpc01* στον τομέα *hpc.local* που αποτελείται από έναν κόμβο κεφαλής και 2, τον υπολογισμό κόμβους. Κόμβους υπολογιστικών δημιουργούνται από μια εικόνα Εικονική πακέτο HPC που περιλαμβάνει το Microsoft Excel.

    ![Εισαγάγετε τις παραμέτρους][parameters]

    >[AZURE.NOTE]Ο κόμβος κεφαλής Εικονική δημιουργείται αυτόματα από την [πιο πρόσφατη εικόνα Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) του HPC πακέτο 2012 R2 σε Windows Server 2012 R2. Προς το παρόν την εικόνα βασίζεται σε HPC Pack 2012 R2 ενημερωμένη έκδοση 3.
    >
    >Πρέπει να υπολογίσετε ΣΠΣ κόμβο δημιουργούνται από την πιο πρόσφατη εικόνα της οικογένειας κόμβο επιλεγμένου υπολογισμού. Ενεργοποιήστε την επιλογή **ComputeNodeWithExcel** για την πιο πρόσφατη εικόνα κόμβο υπολογισμού HPC πακέτο που περιλαμβάνει μια δοκιμαστική έκδοση του Microsoft Excel Professional Plus 2013. Για να αναπτύξετε ένα σύμπλεγμα γενικά περιόδους λειτουργίας SOA ή για μείωση φόρτου Excel UDF, ενεργοποιήστε την επιλογή **ComputeNode** (χωρίς εγκατεστημένο το Excel).

    β. Επιλέξτε τη συνδρομή.

    c. Δημιουργήστε μια ομάδα πόρων για το σύμπλεγμα, όπως *hpc01RG*.

    d. Επιλέξτε μια θέση για την ομάδα πόρων, όπως κεντρικές ΗΠΑ.

    ε. Στη σελίδα **νομική όρους** , διαβάστε τους όρους. Εάν συμφωνείτε, κάντε κλικ στην επιλογή **αγορά**. Στη συνέχεια, όταν ολοκληρώσετε τη ρύθμιση των τιμών για το πρότυπο, κάντε κλικ στην επιλογή **Δημιουργία**.

4.  Όταν ολοκληρωθεί η ανάπτυξη (συνήθως διαρκεί περίπου 30 λεπτά), εξαγωγή του αρχείου πιστοποιητικού σύμπλεγμα από τον κόμβο κεφαλής συμπλέγματος. Αργότερα, μπορείτε να εισαγάγετε αυτό το πιστοποιητικό δημόσια στον υπολογιστή-πελάτη για την παροχή ο έλεγχος ταυτότητας διακομιστή για ασφαλούς σύνδεσης HTTP.

    μια. Σύνδεση με τον κεφαλής κόμβο με σύνδεση απομακρυσμένης επιφάνειας εργασίας από την πύλη του Azure.

     ![Σύνδεση με τον κόμβο κεφαλής][connect]

    β. Χρησιμοποιήστε τυπικές διαδικασίες στη Διαχείριση πιστοποιητικών για να εξαγάγετε το πιστοποιητικό κεφαλής κόμβου (που βρίσκεται στην περιοχή πιστοποιητικού: \LocalMachine\My) χωρίς το ιδιωτικό κλειδί. Σε αυτό το παράδειγμα, εξαγωγή *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Εξαγάγετε το πιστοποιητικό][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Επιλογή 2. Χρησιμοποιήστε τη δέσμη ενεργειών ανάπτυξης IaaS πακέτο HPC

Η δέσμη ενεργειών ανάπτυξης IaaS πακέτο HPC παρέχει ένας άλλος τρόπος ευέλικτη για να αναπτύξετε ένα σύμπλεγμα HPC πακέτο. Δημιουργεί ένα σύμπλεγμα στο μοντέλο κλασική ανάπτυξης, ότι το μοντέλο ανάπτυξης διαχείρισης πόρων Azure χρησιμοποιεί το πρότυπο. Επίσης, η δέσμη ενεργειών είναι συμβατή με μια συνδρομή στην υπηρεσία του Azure καθολικά ή Azure Κίνα.

**Επιπλέον προαπαιτούμενα στοιχεία**

* **Azure PowerShell** - [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../powershell-install-configure.md) (έκδοση 0.8.10 ή νεότερη έκδοση) στον υπολογιστή-πελάτη.

* **Δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS** - λήψη και αποσυμπίεση την πιο πρόσφατη έκδοση της δέσμης ενεργειών από το [Κέντρο λήψης της Microsoft](https://www.microsoft.com/download/details.aspx?id=44949). Ελέγξτε την έκδοση της δέσμης ενεργειών, εκτελώντας `New-HPCIaaSCluster.ps1 –Version`. Σε αυτό το άρθρο βασίζεται σε έκδοση 4.5.0 ή νεότερη έκδοση της δέσμης ενεργειών.

**Δημιουργήστε το αρχείο ρύθμισης παραμέτρων**

 Η δέσμη ενεργειών ανάπτυξης IaaS πακέτο HPC χρησιμοποιεί ένα αρχείο ρύθμισης παραμέτρων XML ως είσοδο που περιγράφει την υποδομή του συμπλέγματος HPC. Για να αναπτύξετε ένα σύμπλεγμα που αποτελείται από έναν κόμβο κεφαλής και 18 κόμβους υπολογισμού που δημιουργούνται από την εικόνα κόμβο υπολογισμού που περιλαμβάνει το Microsoft Excel, αντικαταστήστε τιμές για το περιβάλλον στο παρακάτω παράδειγμα αρχείου ρύθμισης παραμέτρων. Για περισσότερες πληροφορίες σχετικά με το αρχείο ρύθμισης παραμέτρων, ανατρέξτε στο αρχείο Manual.rtf στο φάκελο δέσμης ενεργειών και [Δημιουργία ένα σύμπλεγμα HPC με τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Σημειώσεις σχετικά με το αρχείο ρύθμισης παραμέτρων**

* Το **VMName** του κεφαλής κόμβου **πρέπει να** είναι ίδια με την **όνομα_υπηρεσίας**ή εργασίες SOA αποτύχουν για να εκτελέσετε.

* Φροντίστε να καθορίσετε **EnableWebPortal** , έτσι ώστε το πιστοποιητικό κεφαλής κόμβου είναι που δημιουργούνται και εξαγωγή.

* Το αρχείο Καθορίζει μια δέσμη ενεργειών PowerShell μετά τη ρύθμιση των παραμέτρων PostConfig.ps1 που εκτελείται στον κόμβο κεφαλίδας. Το παρακάτω δείγμα δέσμης ενεργειών ρυθμίζει τις παραμέτρους της συμβολοσειράς σύνδεσης Azure χώρου αποθήκευσης, καταργεί το ρόλο κόμβο υπολογισμού από τον κόμβο κεφαλής και εμφανίζει όλους τους κόμβους online όταν αναπτύσσονται. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Εκτελέστε τη δέσμη ενεργειών**

1.  Ανοίξτε την κονσόλα του PowerShell του υπολογιστή-πελάτη ως διαχειριστής.

2.  Αλλαγή καταλόγου στο φάκελο δέσμης ενεργειών (E:\IaaSClusterScript σε αυτό το παράδειγμα).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  Για να αναπτύξετε το σύμπλεγμα HPC πακέτου, εκτελέστε την ακόλουθη εντολή. Αυτό το παράδειγμα προϋποθέτει ότι το αρχείο ρύθμισης παραμέτρων βρίσκεται στο E:\HPCDemoConfig.xml.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

Η δέσμη ενεργειών ανάπτυξης HPC πακέτο εκτελείται για κάποιο χρονικό διάστημα. Ένα σημείο κάνει τη δέσμη ενεργειών είναι να εξαγάγετε και να κάνετε λήψη του πιστοποιητικού σύμπλεγμα και αποθηκεύστε το στο φάκελο έγγραφα του τρέχοντος χρήστη του υπολογιστή-πελάτη. Η δέσμη ενεργειών δημιουργεί ένα παρόμοιο με το ακόλουθο μήνυμα. Σε ένα βήμα παρακάτω, μπορείτε να εισαγάγετε το πιστοποιητικό στο χώρο αποθήκευσης κατάλληλο πιστοποιητικό.    
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Βήμα 2. Μείωση φόρτου βιβλία εργασίας του Excel και εκτελέστε το UDF από ένα πρόγραμμα-πελάτη στην εσωτερική εγκατάσταση

### <a name="excel-activation"></a>Ενεργοποίηση του Excel

Όταν χρησιμοποιείτε την Εικονική ComputeNodeWithExcel εικόνα για παραγωγής φόρτους εργασίας, πρέπει να δώσετε ένα έγκυρο κλειδί άδειας χρήσης του Microsoft Office για να ενεργοποιήσετε το Excel σε κόμβους υπολογιστικών. Διαφορετικά, την έκδοση αξιολόγησης του Excel λήξη μετά από 30 ημέρες και εκτελεί βιβλία εργασίας του Excel θα αποτύχει με το COMException (0x800AC472). 

Μπορείτε να κάνετε επανενεργοποίηση Excel για μια άλλη 30 ημέρες του χρόνου υπολογισμού: σύνδεση στο η κεφαλή κόμβο και clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` σε όλο το Excel υπολογίζει κόμβους μέσω HPC σύμπλεγμα Manager. Μπορείτε να κάνετε επανενεργοποίηση έως και δύο φορές. Μετά από αυτό, πρέπει να δώσετε ένα έγκυρο κλειδί άδειας χρήσης του Office.

Το Office Professional Plus 2013 εγκατεστημένη στην εικόνα της Εικονική είναι μια έκδοση πολλαπλών αδειών χρήσης με ένα γενικό κλειδί άδειας χρήσης ένταση (GVLK). Μπορείτε να το ενεργοποιήσετε μέσω υπηρεσίας διαχείρισης κλειδιών (KMS) / Active Directory-Based ενεργοποίησης (AD-BA) ή αριθμού-κλειδιού πολλαπλής ενεργοποίησης (MAK). 

    * Για να χρησιμοποιήσετε KMS/AD-BA, χρησιμοποιήστε έναν υπάρχοντα διακομιστή KMS ή να ρυθμίσετε ένα νέο, χρησιμοποιώντας το πακέτο άδεια χρήσης του Microsoft Office 2013 ένταση. (Εάν θέλετε να, ρυθμίστε το διακομιστή στον κόμβο κεφαλής.) Στη συνέχεια, ενεργοποιήστε τον αριθμό-κλειδί κεντρικό υπολογιστή KMS μέσω του Internet ή μέσω τηλεφώνου. Στη συνέχεια, clusrun `ospp.vbs` για να ορίσετε το KMS διακομιστή και τη θύρα και ενεργοποίηση του Office σε ολόκληρο το Excel υπολογίζει κόμβους. 
    
    * Για να χρησιμοποιήσετε MAK, πρώτη clusrun `ospp.vbs` να εισαγάγετε τον αριθμό-κλειδί και, στη συνέχεια, ενεργοποιήστε όλα το Excel υπολογίζει τους κόμβους μέσω του Internet ή μέσω τηλεφώνου. 

>[AZURE.NOTE]Αριθμούς-κλειδιά προϊόντος λιανικής πώλησης για το Office Professional Plus 2013 δεν μπορούν να χρησιμοποιηθούν με αυτήν την εικόνα Εικονική. Εάν έχετε έγκυρη κλειδιά και μέσων εγκατάστασης για το Office ή το Excel εκδόσεις εκτός από αυτό ομαδική έκδοση του Office Professional Plus 2013, μπορείτε να τις χρησιμοποιήσετε αντί για αυτό. Πρώτα, καταργήστε αυτό έκδοση πολλαπλών αδειών χρήσης και να εγκαταστήσετε την έκδοση που διαθέτετε. Το εγκαταστάθηκε ξανά κόμβος Excel μπορούν να καταγραφούν ως προσαρμοσμένο Εικονική εικόνα για να χρησιμοποιήσετε σε μια ανάπτυξη του με κλίμακα.

### <a name="offload-excel-workbooks"></a>Μείωση φόρτου βιβλία εργασίας του Excel

Ακολουθήστε τα παρακάτω βήματα για τη μείωση φόρτου σε ένα βιβλίο εργασίας του Excel, ώστε να εκτελείται στο σύμπλεγμα HPC πακέτο στο Azure. Για να το κάνετε αυτό, πρέπει να έχετε ήδη εγκατεστημένο στον υπολογιστή-πελάτη Excel 2010 ή 2013.

1. Χρησιμοποιήστε μία από τις επιλογές στο βήμα 1 για να αναπτύξετε ένα σύμπλεγμα HPC πακέτο με το Excel υπολογίζει εικόνα κόμβο. Αποκτήστε το αρχείο πιστοποιητικού σύμπλεγμα (.cer) και σύμπλεγμα όνομα χρήστη και κωδικό πρόσβασης.

2. Στον υπολογιστή-πελάτη, εισαγάγετε το πιστοποιητικό σύμπλεγμα στην περιοχή πιστοποιητικού: \CurrentUser\Root.

3. Βεβαιωθείτε ότι έχει εγκατασταθεί το Excel. Δημιουργήστε ένα αρχείο Excel.exe.config με τα ακόλουθα περιεχόμενα στον ίδιο φάκελο με Excel.exe στον υπολογιστή-πελάτη. Αυτό το βήμα εξασφαλίζει ότι το πρόσθετο του HPC πακέτο 2012 R2 Excel COM φορτώνει με επιτυχία.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.  Ρυθμίστε το πρόγραμμα-πελάτη για να υποβάλλουν εργασίες στο σύμπλεγμα HPC πακέτο. Μία επιλογή είναι να κάνετε λήψη της πλήρους [HPC Pack 2012 R2 Update 3 εγκατάστασης](http://www.microsoft.com/download/details.aspx?id=49922) και εγκατάσταση του προγράμματος-πελάτη HPC πακέτο. Εναλλακτικά, λήψη και εγκατάσταση του [βοηθητικά προγράμματα υπολογιστή-πελάτη HPC Pack 2012 R2 ενημέρωση 3](https://www.microsoft.com/download/details.aspx?id=49923) και το κατάλληλο Visual C++ 2010 με δυνατότητα ανακατανομής για τον υπολογιστή σας ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  Σε αυτό το παράδειγμα, χρησιμοποιούμε ένα δείγμα βιβλίου εργασίας Excel με το όνομα ConvertiblePricing_Complete.xlsb. Μπορείτε να κάνετε λήψη του [εδώ](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  Αντιγράψτε το βιβλίο εργασίας του Excel σε ένα φάκελο εργασίας όπως D:\Excel\Run.

7.  Ανοίξτε το βιβλίο εργασίας του Excel. **Ανάπτυξη** της κορδέλας, κάντε κλικ στην επιλογή **Πρόσθετα COM** και επιβεβαιώστε ότι το πρόσθετο COM Excel πακέτο HPC σε έχει φορτωθεί με επιτυχία.

    ![Το Excel για το πακέτο HPC][addin]

8.  Επεξεργασία της μακροεντολής VBA HPCControlMacros στο Excel, αλλάζοντας το γραμμές σχολίων, όπως φαίνεται στην ακόλουθη δέσμη ενεργειών. Αντικαταστήστε τις κατάλληλες τιμές για το περιβάλλον σας.

    ![Για το πακέτο HPC μακροεντολών του Excel][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.  Αντιγράψτε το βιβλίο εργασίας του Excel σε έναν κατάλογο αποστολής όπως D:\Excel\Upload. Αυτός ο κατάλογος έχει καθοριστεί στη σταθερά HPC_DependsFiles στη μακροεντολή VBA.

10. Για να εκτελέσετε το βιβλίο εργασίας στο σύμπλεγμα στο Azure, κάντε κλικ στο κουμπί **σύμπλεγμα** στο φύλλο εργασίας.

### <a name="run-excel-udfs"></a>Εκτέλεση UDF Excel

Για να εκτελέσετε Excel UDF, ακολουθήστε τα παραπάνω βήματα 1-3 για να ορίσετε τον υπολογιστή-πελάτη. Για το Excel UDF, δεν χρειάζεται να έχετε την εφαρμογή Excel εγκατεστημένο στον κόμβους υπολογιστικών. Επομένως, κατά τη δημιουργία συμπλέγματος τον υπολογισμό κόμβους, θα μπορούσατε να επιλέξετε κανονική τον υπολογισμό κόμβο εικόνα αντί για την εικόνα κόμβο υπολογισμού με το Excel.

>[AZURE.NOTE] Υπάρχει ένα όριο χαρακτήρων 34 στο το Excel 2010 και 2013 σύμπλεγμα σύνδεσης διαλόγου. Μπορείτε να χρησιμοποιήσετε αυτό το πλαίσιο διαλόγου για να καθορίσετε το σύμπλεγμα που εκτελεί το UDF. Εάν το όνομα πλήρες σύμπλεγμα είναι μεγαλύτερο (για παράδειγμα, hpcexcelhn01.southeastasia.cloudapp.azure.com), δεν είναι κατάλληλη για στο παράθυρο διαλόγου. Η λύση είναι να ορίσετε μια μεταβλητή συνολικές όπως *CCP_IAASHN* με την τιμή του ονόματος μεγάλο σύμπλεγμα. Στη συνέχεια, πληκτρολογήστε *% CCP_IAASHN %* στο παράθυρο διαλόγου ως το όνομα κεφαλής κόμβο συμπλέγματος. 

Μετά την ανάπτυξη του συμπλέγματος είναι επιτυχής, συνεχίστε με τα παρακάτω βήματα για να εκτελέσετε μια ενσωματωμένη δείγμα Excel UDF. Για το προσαρμοσμένο UDF Excel, ανατρέξτε στο θέμα αυτοί οι [πόροι](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) για να δημιουργήστε το XLL και αναπτύξτε τους στο σύμπλεγμα IaaS.

1.  Ανοίξτε ένα νέο βιβλίο εργασίας του Excel. **Ανάπτυξη** της κορδέλας, κάντε κλικ στην επιλογή **Πρόσθετα**. Στη συνέχεια, στο παράθυρο διαλόγου, κάντε κλικ στο κουμπί **Αναζήτηση**, μεταβείτε στο φάκελο %CCP_HOME%Bin\XLL32 και επιλέξτε το δείγμα ClusterUDF32.xll. Εάν το ClusterUDF32 δεν υπάρχει στον υπολογιστή-πελάτη, αντιγράψτε το από το φάκελο %CCP_HOME%Bin\XLL32 στον κόμβο κεφαλίδας.

    ![Επιλέξτε το UDF][udf]

2.  Κάντε κλικ στο **αρχείο** > **Επιλογές** > **για προχωρημένους**. Στην περιοχή **τύπων**, επιλέξτε **να επιτρέπεται XLL συναρτήσεις χρήστη για να εκτελέσετε ένα σύμπλεγμα υπολογισμού**. Στη συνέχεια, κάντε κλικ στις **Επιλογές** και πληκτρολογήστε το όνομα πλήρες σύμπλεγμα στο **όνομα κεφαλής κόμβο συμπλέγματος**. (Όπως σημειώνεται προηγουμένως αυτό το πλαίσιο εισαγωγής περιορίζεται στους 34 χαρακτήρες, για ένα σύμπλεγμα μεγάλο όνομα δεν μπορεί να ταιριάζουν. Μπορείτε να χρησιμοποιήσετε μια μεταβλητή συνολικές εδώ για ένα όνομα μεγάλο σύμπλεγμα.)

    ![Ρύθμιση παραμέτρων του UDF][options]

3.  Για να εκτελέσετε τον υπολογισμό UDF στο σύμπλεγμα, κάντε κλικ στο κελί με τιμή =XllGetComputerNameC() και πατήστε το πλήκτρο Enter. Η συνάρτηση ανακτά απλώς το όνομα του κόμβου υπολογισμού στην οποία εκτελείται το UDF. Για την πρώτη εκτέλεση, ένα παράθυρο διαλόγου διαπιστευτήρια σας ζητά το όνομα χρήστη και τον κωδικό πρόσβασης για να συνδεθείτε με το σύμπλεγμα IaaS.

    ![Εκτέλεση UDF][run]

    Όταν υπάρχουν πολλά κελιά για τον υπολογισμό, πιέστε το συνδυασμό πλήκτρων Alt-Shift-Ctrl + F9 για να εκτελέσετε τον υπολογισμό σε όλα τα κελιά.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Βήμα 3. Εκτέλεση ένα φόρτο εργασίας SOA από ένα πρόγραμμα-πελάτη στην εσωτερική εγκατάσταση

Για να εκτελέσετε γενικές εφαρμογές SOA στο σύμπλεγμα HPC πακέτο IaaS, πρώτα χρησιμοποιήστε μία από τις μεθόδους στο βήμα 1 για να αναπτύξετε το σύμπλεγμα. Καθορίστε ένα γενικό τον υπολογισμό εικόνα κόμβο σε αυτήν την περίπτωση, επειδή δεν χρειάζεται Excel σε κόμβους υπολογιστικών. Στη συνέχεια, ακολουθήστε τα παρακάτω βήματα.

1. Μετά από την ανάκτηση του πιστοποιητικού σύμπλεγμα, εισαγάγετέ το στον υπολογιστή-πελάτη στην περιοχή πιστοποιητικού: \CurrentUser\Root.

2. Εγκαταστήστε το [HPC πακέτο 2012 R2 ενημέρωση 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) και [HPC Pack 2012 R2 ενημέρωση 3 βοηθητικά προγράμματα υπολογιστή-πελάτη](https://www.microsoft.com/download/details.aspx?id=49923). Αυτά τα εργαλεία σάς επιτρέπουν να αναπτύξετε και να εκτελέσετε SOA εφαρμογές προγράμματος-πελάτη.

3. Κάντε λήψη του HelloWorldR2 [δείγμα κώδικα](https://www.microsoft.com/download/details.aspx?id=41633). Ανοίξτε το HelloWorldR2.sln στο Visual Studio 2010 ή 2012.

4. Δημιουργήστε πρώτα το έργο EchoService. Στη συνέχεια, αναπτύξτε την υπηρεσία στο σύμπλεγμα IaaS με τον ίδιο τρόπο που αναπτύσσετε σε ένα σύμπλεγμα εσωτερικής εγκατάστασης. Για λεπτομερείς οδηγίες, ανατρέξτε στο θέμα το Readme.doc στο HelloWordR2. Τροποποίηση και δημιουργήστε την HellWorldR2 και άλλα έργα όπως περιγράφεται στην επόμενη ενότητα για να δημιουργήσετε τις εφαρμογές προγράμματος-πελάτη SOA που εκτελούνται σε ένα σύμπλεγμα Azure IaaS.

### <a name="use-http-binding-with-azure-storage-queue"></a>Χρήση σύνδεσης Http με ουρά Azure χώρου αποθήκευσης

Για να χρησιμοποιήσετε Http σύνδεση με μια ουρά Azure χώρου αποθήκευσης, κάντε αλλαγές μερικά το δείγμα κώδικα.

* Ενημερώστε το όνομα του συμπλέγματος.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Προαιρετικά, χρησιμοποιήστε την προεπιλεγμένη TransportScheme στο SessionStartInfo ή ρητά ρυθμίστε την σε Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Χρήση βιβλιοδεσίας προεπιλογή για το BrokerClient.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Ή ρητά χρησιμοποιώντας το basicHttpBinding.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* Προαιρετικά, μπορείτε να ορίσετε τη σημαία UseAzureQueue στην τιμή true στο SessionStartInfo. Εάν δεν έχει οριστεί, αυτό θα οριστεί σε true από προεπιλογή, όταν το όνομα του συμπλέγματος έχει προθέματα Azure τομέα και το TransportScheme είναι Http.

    ```
    info.UseAzureQueue = true;
```

###<a name="use-http-binding-without-azure-storage-queue"></a>Χρήση βιβλιοδεσίας Http χωρίς ουρά Azure χώρου αποθήκευσης

Για να χρησιμοποιήσετε Http σύνδεσης χωρίς μια ουρά Azure χώρου αποθήκευσης, ορίστε ρητά τη σημαία UseAzureQueue σε false σε το SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Χρήση NetTcp βιβλιοδεσίας

Για να χρησιμοποιήσετε τη σύνδεση NetTcp, τη ρύθμιση παραμέτρων είναι παρόμοια με τη σύνδεση σε ένα σύμπλεγμα εσωτερικής εγκατάστασης. Πρέπει να το ανοίξετε ορισμένα τελικά σημεία στον κόμβο κεφαλής Εικονική. Εάν χρησιμοποιήσατε τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS για να δημιουργήσετε το σύμπλεγμα, για παράδειγμα, ορίστε τα τελικά σημεία στην πύλη του Azure κλασική ως εξής.


1. Διακόψτε την εικονική Μηχανή.

2. Προσθέστε τις θύρες TCP 9090, 9087, 9091, 9094 για την περίοδο λειτουργίας, Broker, Broker εργαζόμενου και υπηρεσίες δεδομένων, αντίστοιχα

    ![Ρύθμιση παραμέτρων τελικά σημεία][endpoint]

3. Ξεκινήστε την εικονική Μηχανή.

Η εφαρμογή προγράμματος-πελάτη SOA απαιτεί καμία αλλαγή εκτός από τροποποίηση στο κεφαλής όνομα για να το πλήρες όνομα σύμπλεγμα IaaS.

## <a name="next-steps"></a>Επόμενα βήματα

* Ανατρέξτε [στους παρακάτω πόρους](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) για περισσότερες πληροφορίες σχετικά με την εκτέλεση φόρτους εργασίας του Excel με το πακέτο HPC.

* Για περισσότερες πληροφορίες σχετικά με την ανάπτυξη και τη διαχείριση των υπηρεσιών SOA με πακέτο HPC, ανατρέξτε στο θέμα [Διαχείριση υπηρεσιών SOA στο πακέτο HPC Microsoft](https://technet.microsoft.com/library/ff919412.aspx) .

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png
