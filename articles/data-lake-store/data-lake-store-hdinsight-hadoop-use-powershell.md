<properties
   pageTitle="Δημιουργία συμπλεγμάτων HDInsight με χρήση του PowerShell χώρου αποθήκευσης λίμνης δεδομένων Azure | Azure"
   description="Χρήση του Azure PowerShell για δημιουργία και χρήση συμπλεγμάτων HDInsight με το Azure δεδομένων λίμνης"
   services="data-lake-store,hdinsight" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="nitinme"/>

# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-powershell"></a>Δημιουργήστε ένα σύμπλεγμα HDInsight με το χώρο αποθήκευσης λίμνης δεδομένων με χρήση του PowerShell Azure

> [AZURE.SELECTOR]
- [Με πύλη](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Χρήση του PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)
- [Χρήση της διαχείρισης πόρων](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure PowerShell για να ρυθμίσετε ένα σύμπλεγμα HDInsight (Hadoop, HBase ή καταιγίδας) με την πρόσβαση στο χώρο αποθήκευσης λίμνης Azure δεδομένων. Ορισμένα σημαντικά θέματα για αυτήν την έκδοση:

* **Για τους συμπλεγμάτων (Linux), και Hadoop/καταιγίδας συμπλεγμάτων (Windows και Linux)**, ο χώρος αποθήκευσης δεδομένων λίμνης μπορεί να χρησιμοποιηθεί μόνο ως ένα λογαριασμό επιπλέον χώρο αποθήκευσης. Τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης για το εν λόγω συμπλεγμάτων θα εξακολουθούν να Azure χώρο αποθήκευσης αντικειμένων blob (WASB).

* **Για HBase συμπλεγμάτων (Windows και Linux)**, ο χώρος αποθήκευσης δεδομένων λίμνης μπορεί να χρησιμοποιηθεί ως προεπιλεγμένο αποθήκευσης ή επιπλέον χώρο αποθήκευσης.

> [AZURE.NOTE] Ορισμένα σημαντικά σημεία σε σημείωση. 
> 
> * Επιλογή για να δημιουργήσετε συμπλεγμάτων HDInsight με την πρόσβαση στο χώρο αποθήκευσης λίμνης δεδομένων είναι διαθέσιμη μόνο για τις εκδόσεις HDInsight 3.2 και 3.4 (για Hadoop, HBase και καταιγίδας συμπλεγμάτων σε Windows, καθώς και Linux). Για τους συμπλεγμάτων σε Linux, αυτή η επιλογή είναι διαθέσιμη μόνο σε συμπλεγμάτων HDInsight 3.4.
>
> * Όπως προαναφέρθηκε, χώρου αποθήκευσης λίμνης δεδομένων είναι διαθέσιμα ως προεπιλεγμένο αποθήκευσης για ορισμένους τύπους συμπλέγματος (HBase) και επιπλέον χώρου αποθήκευσης για άλλους τύπους συμπλέγματος (Hadoop, τους, καταιγίδας). Χρήση χώρου αποθήκευσης δεδομένων λίμνης ως ένα λογαριασμό επιπλέον χώρο αποθήκευσης δεν επηρεάζει τις επιδόσεις ή τη δυνατότητα ανάγνωσης/εγγραφής στο χώρο αποθήκευσης από το σύμπλεγμα. Σε ένα σενάριο όπου χρησιμοποιείται ο χώρος αποθήκευσης δεδομένων λίμνης ως επιπλέον χώρο αποθήκευσης, τα αρχεία που σχετίζονται με το σύμπλεγμα (όπως αρχεία καταγραφής, κ.λπ.) εγγράφεται το προεπιλεγμένο αποθήκευσης (αντικείμενα BLOB Azure), ενώ τα δεδομένα που θέλετε να επεξεργαστείτε μπορούν να αποθηκευτούν σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.


Σε αυτό το άρθρο θα σας προμήθεια ένα σύμπλεγμα Hadoop με το χώρο αποθήκευσης δεδομένων λίμνης ως επιπλέον χώρο αποθήκευσης.

Ρύθμιση παραμέτρων HDInsight για να εργαστείτε με το χώρο αποθήκευσης λίμνης δεδομένων με χρήση του PowerShell περιλαμβάνει τα ακόλουθα βήματα:

* Δημιουργία ενός χώρου αποθήκευσης λίμνης δεδομένων Azure
* Ρύθμιση του ελέγχου ταυτότητας για πρόσβασης βάσει ρόλων στο χώρο αποθήκευσης δεδομένων λίμνης
* Δημιουργία συμπλέγματος HDInsight με έλεγχο ταυτότητας στο χώρο αποθήκευσης δεδομένων λίμνης
* Εκτελέστε μια εργασία δοκιμής στο σύμπλεγμα

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

- **Azure PowerShell 1.0 ή μεγαλύτερη**. Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

- **Windows SDK**. Μπορείτε να το εγκαταστήσετε από [εδώ](https://dev.windows.com/en-us/downloads). Αυτό μπορείτε να το χρησιμοποιήσετε για να δημιουργήσετε ένα πιστοποιητικό ασφαλείας.

- **Azure Active Directory υπηρεσίας κεφάλαιο**. Βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης παρέχουν οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα κεφάλαιο υπηρεσίας στο Azure AD. Ωστόσο, πρέπει να είστε διαχειριστής Azure AD για να μπορέσετε να δημιουργήσετε ένα κεφάλαιο υπηρεσίας. Εάν είστε διαχειριστής Azure AD, μπορείτε να παραλείψετε αυτήν την προϋπόθεση και να συνεχίσετε με το πρόγραμμα εκμάθησης.
    
    **Εάν είστε δεν διαχειριστής Azure AD**, δεν θα μπορείτε να εκτελέσετε τα βήματα που απαιτούνται για να δημιουργήσετε ένα κεφάλαιο υπηρεσίας. Σε αυτήν την περίπτωση, ο διαχειριστής Azure AD πρέπει πρώτα να δημιουργήσετε αρχής υπηρεσίας πριν να δημιουργήσετε ένα σύμπλεγμα HDInsight με το χώρο αποθήκευσης λίμνης δεδομένων. Επίσης, το κεφάλαιο υπηρεσίας πρέπει να δημιουργηθούν με χρήση ενός πιστοποιητικού, όπως περιγράφεται στην υποστήριξη [δημιουργίας μιας υπηρεσίας κεφαλαίου με το πιστοποιητικό](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate). 


## <a name="create-an-azure-data-lake-store"></a>Δημιουργία ενός χώρου αποθήκευσης λίμνης δεδομένων Azure

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε ένα χώρο αποθήκευσης λίμνης δεδομένων.

1. Από την επιφάνεια εργασίας, ανοίξτε ένα νέο παράθυρο Azure PowerShell και πληκτρολογήστε το παρακάτω τμήμα κώδικα. Όταν σας ζητηθεί να συνδεθείτε στο, βεβαιωθείτε ότι συνδέεστε στο ως ένα από τα admininistrators/κάτοχος της συνδρομής:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    >[AZURE.NOTE] Εάν λάβετε ένα σφάλμα παρόμοιο με `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` κατά την εγγραφή για την υπηρεσία παροχής του χώρου αποθήκευσης δεδομένων λίμνης πόρων, είναι πιθανό ότι το subsrcription δεν είναι whitelisted για το χώρο αποθήκευσης λίμνης Azure δεδομένων. Βεβαιωθείτε ότι μπορείτε να ενεργοποιήσετε τη συνδρομή σας Azure για το χώρο αποθήκευσης δεδομένων λίμνης δημόσια preview, ακολουθώντας τις παρακάτω [οδηγίες](data-lake-store-get-started-portal.md#signup).

3. Ένα λογαριασμό χώρου αποθήκευσης λίμνης Azure δεδομένων είναι συσχετισμένη με μια ομάδα πόρων του Azure. Ξεκινήστε με τη δημιουργία μιας ομάδας πόρων Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Δημιουργία ομάδας πόρων του Azure] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Δημιουργία ομάδας πόρων του Azure")

2. Δημιουργήστε ένα λογαριασμό χώρου αποθήκευσης λίμνης Azure δεδομένων. Το όνομα του λογαριασμού που καθορίζετε πρέπει να περιέχει μόνο πεζά γράμματα και αριθμούς.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Δημιουργία λογαριασμού λίμνης δεδομένων Azure] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Δημιουργία λογαριασμού λίμνης δεδομένων Azure")

3. Επαληθεύστε ότι ο λογαριασμός δημιουργήθηκε με επιτυχία.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Το αποτέλεσμα για αυτό πρέπει να είναι **True**.

4. Αποστολή μερικά δείγματα δεδομένων Azure λίμνης δεδομένων. Θα χρησιμοποιήσουμε αυτό αργότερα σε αυτό το άρθρο για να επαληθεύσετε ότι τα δεδομένα είναι προσβάσιμα από ένα σύμπλεγμα HDInsight. Εάν αναζητάτε μερικά δείγματα δεδομένων για την αποστολή, μπορείτε να λάβετε το φάκελο **Ασθενοφόρων δεδομένων** από το [Azure δεδομένων λίμνης Git αποθετήριο](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).


        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Ρύθμιση του ελέγχου ταυτότητας για πρόσβασης βάσει ρόλων στο χώρο αποθήκευσης δεδομένων λίμνης

Κάθε Azure συνδρομή είναι συσχετισμένη με μια Azure Active Directory. Οι χρήστες και τις υπηρεσίες που έχει πρόσβαση σε πόρους από τη συνδρομή χρησιμοποιώντας την πύλη κλασική Azure ή το API διαχείρισης πόρων Azure πρέπει να πρώτα τον έλεγχο ταυτότητας του με συγκεκριμένο Azure Active Directory. Η πρόσβαση παρέχεται σε συνδρομές του Azure και υπηρεσίες αναθέτοντας σε αυτά τον κατάλληλο ρόλο σε έναν πόρο Azure.  Για τις υπηρεσίες, αρχής υπηρεσίας προσδιορίζει την υπηρεσία στο Azure Active Directory (AAD). Αυτή η ενότητα παρουσιάζει τον τρόπο για την εκχώρηση μιας εφαρμογής υπηρεσίας, όπως το HDInsight, πρόσβαση σε έναν πόρο Azure (το λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων Azure που δημιουργήσατε νωρίτερα), δημιουργώντας ένα κεφάλαιο υπηρεσία για την εφαρμογή και εκχώρηση ρόλων που μέσω του Azure PowerShell.

Για να ρυθμίσετε τον έλεγχο ταυτότητας υπηρεσίας καταλόγου Active Directory για λίμνης δεδομένων Azure, πρέπει να εκτελέσετε τις ακόλουθες εργασίες.

* Δημιουργία πιστοποιητικού αυτόματης υπογραφής
* Δημιουργία μιας εφαρμογής στο Azure Active Directory και αρχής υπηρεσίας

### <a name="create-a-self-signed-certificate"></a>Δημιουργία πιστοποιητικού αυτόματης υπογραφής

Βεβαιωθείτε ότι έχετε εγκαταστήσει πριν να συνεχίσετε με τα βήματα σε αυτήν την ενότητα [SDK των Windows](https://dev.windows.com/en-us/downloads) . Πρέπει να έχετε επίσης δημιουργήσει έναν κατάλογο, όπως **C:\mycertdir**, όπου θα δημιουργηθεί το πιστοποιητικό.

1. Από το παράθυρο του PowerShell, μεταβείτε στη θέση όπου έχετε εγκαταστήσει το SDK των Windows (συνήθως, `C:\Program Files (x86)\Windows Kits\10\bin\x86` και χρησιμοποιήστε το [MakeCert] [ makecert] βοηθητικού προγράμματος για να δημιουργήσετε ένα αυτο-υπογεγραμμένο πιστοποιητικό και ένα ιδιωτικό κλειδί. Χρησιμοποιήστε τις παρακάτω εντολές.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048

    Θα σας ζητηθεί να εισαγάγετε τον κωδικό πρόσβασης ιδιωτικό κλειδί. Μετά την εκτέλεση της εντολής με επιτυχία, θα πρέπει να βλέπετε ένα **CertFile.cer** και **mykey.pvk** στον κατάλογο πιστοποιητικού που καθορίσατε.

4. Χρησιμοποιήστε το [Pvk2Pfx] [ pvk2pfx] βοηθητικού προγράμματος για να μετατρέψετε τα αρχεία .pvk και .cer που δημιουργήσατε MakeCert σε ένα αρχείο .pfx. Εκτελέστε την ακόλουθη εντολή.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Όταν σας ζητηθεί κωδικός πρόσβασης του ιδιωτικού κλειδιού που καθορίσατε προηγουμένως. Η τιμή που ορίζετε για την παράμετρο **-Ταχυδρομική** είναι ο κωδικός πρόσβασης που σχετίζεται με το αρχείο .pfx. Μετά την εντολή ολοκληρωθεί με επιτυχία, θα πρέπει επίσης να δείτε μια CertFile.pfx στον κατάλογο πιστοποιητικού που καθορίσατε.

###  <a name="create-an-azure-active-directory-and-a-service-principal"></a>Δημιουργήστε ένα Azure Active Directory και αρχής υπηρεσίας

Σε αυτήν την ενότητα, μπορείτε να εκτελέσετε τα βήματα για να δημιουργήσετε μια υπηρεσία κεφαλαίου για μια εφαρμογή του Azure Active Directory, εκχώρηση ρόλου με την υπηρεσία του κεφαλαίου και τον έλεγχο ταυτότητας ως το κεφάλαιο υπηρεσίας, παρέχοντας ένα πιστοποιητικό. Εκτελέστε τις ακόλουθες εντολές για να δημιουργήσετε μια εφαρμογή στο Azure Active Directory.

1. Επικολλήστε τα ακόλουθα cmdlet στο παράθυρο της κονσόλας του PowerShell. Βεβαιωθείτε ότι η τιμή που ορίζετε για την ιδιότητα **- DisplayName** είναι μοναδικό. Επίσης, οι τιμές για **Την αρχική σελίδα -** και **- IdentiferUris** τιμές κράτησης θέσης και δεν έχουν επαληθευτεί.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate

        $applicationId = $application.ApplicationId

2. Δημιουργία αρχής υπηρεσία χρησιμοποιώντας το αναγνωριστικό εφαρμογής.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id

3. Παραχωρήστε πρόσβαση κεφαλαίου υπηρεσίας για το χώρο αποθήκευσης δεδομένων λίμνης αρχείο/φάκελος που θα έχετε πρόσβαση από το σύμπλεγμα HDInsight. Το παρακάτω τμήμα κώδικα παρέχει πρόσβαση στον ριζικό κατάλογο του χώρου αποθήκευσης δεδομένων λίμνης λογαριασμού.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All

    Στη γραμμή εντολών, πληκτρολογήστε **Y** για να επιβεβαιώσετε.

## <a name="create-an-hdinsight-cluster-with-authentication-to-data-lake-store"></a>Δημιουργήστε ένα σύμπλεγμα HDInsight με έλεγχο ταυτότητας στο χώρο αποθήκευσης δεδομένων λίμνης

Σε αυτήν την ενότητα, μπορούμε να δημιουργήσουμε ένα σύμπλεγμα HDInsight Hadoop. Για αυτήν την έκδοση, το σύμπλεγμα HDInsight και το χώρο αποθήκευσης λίμνης δεδομένων πρέπει να είναι στην ίδια θέση (Ανατολικής η.π.α. 2).

1. Ξεκινήστε με την ανάκτηση το αναγνωριστικό του μισθωτή συνδρομής. Θα χρειαστείτε που αργότερα.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Για αυτήν την έκδοση, για ένα σύμπλεγμα Hadoop, χώρος αποθήκευσης δεδομένων λίμνης μπορεί να χρησιμοποιηθεί μόνο ως μια επιπλέον χώρο αποθήκευσης για το σύμπλεγμα. Το προεπιλεγμένο αποθήκευσης θα εξακολουθούν να είναι το Azure χώρο αποθήκευσης αντικειμένων blob (WASB). Επομένως, θα σας θα πρώτα να δημιουργήσετε το λογαριασμό χώρου αποθήκευσης και χώρους αποθήκευσης που απαιτούνται για το σύμπλεγμα.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext

3. Δημιουργία του συμπλέγματος HDInsight. Χρησιμοποιήστε τα ακόλουθα cmdlet.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Αφού ολοκληρωθεί με επιτυχία το cmdlet, θα πρέπει να βλέπετε το αποτέλεσμα ως εξής:

        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Εκτέλεση δοκιμής εργασίες στο σύμπλεγμα HDInsight για να χρησιμοποιήσετε το χώρο αποθήκευσης δεδομένων λίμνης

Αφού έχετε ρυθμίσει ένα σύμπλεγμα HDInsight, μπορείτε να εκτελέσετε εργασίες δοκιμής στο σύμπλεγμα για να ελέγξετε ότι το σύμπλεγμα HDInsight πρόσβαση χώρου αποθήκευσης λίμνης δεδομένων. Για να το κάνετε αυτό, θα σας θα εκτελέσετε μια εργασία Hive δείγμα που δημιουργεί έναν πίνακα χρησιμοποιώντας το δείγμα δεδομένων που έχετε αποστείλει προηγουμένως στο χώρο αποθήκευσης λίμνης των δεδομένων.

### <a name="for-a-linux-cluster"></a>Για ένα σύμπλεγμα Linux

Σε αυτήν την ενότητα θα SSH στον σύμπλεγμα και την εκτέλεση του ερωτήματος Hive δείγμα. Τα Windows δεν παρέχει ένα ενσωματωμένο πρόγραμμα-πελάτη SSH. Συνιστάται να χρησιμοποιείτε **PuTTY**, που μπορούν να ληφθούν από [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Για περισσότερες πληροφορίες σχετικά με τη χρήση PuTTY, ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από τα Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Μετά τη σύνδεση, ξεκινήστε το CLI Hive, χρησιμοποιώντας την ακόλουθη εντολή:

        hive

2. Χρησιμοποιώντας το CLI, εισαγάγετε τις παρακάτω προτάσεις για να δημιουργήσετε έναν νέο πίνακα με το όνομα **οχήματα** , χρησιμοποιώντας το δείγμα δεδομένων στο χώρο αποθήκευσης λίμνης δεδομένων:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Θα πρέπει να δείτε το αποτέλεσμα παρόμοιο με το εξής:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


### <a name="for-a-windows-cluster"></a>Για ένα σύμπλεγμα των Windows

Χρησιμοποιήστε τα ακόλουθα cmdlet για να εκτελέσετε το ερώτημα ομάδας. Σε αυτό το ερώτημα Δημιουργία πίνακα από τα δεδομένα στο χώρο αποθήκευσης λίμνης δεδομένων και, στη συνέχεια, να εκτελέσετε ένα ερώτημα επιλογής στον πίνακα που δημιουργήσατε.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

Αυτό θα έχει το εξής αποτέλεσμα. **ExitValue** 0 στο αποτέλεσμα προτείνει ότι η εργασία ολοκληρώθηκε με επιτυχία.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

Ανάκτηση την έξοδο από το έργο χρησιμοποιώντας το ακόλουθο cmdlet:

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

Το αποτέλεσμα του έργου παρόμοιο με το εξής:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## <a name="access-data-lake-store-using-hdfs-commands"></a>Χρήση εντολών HDFS χώρου αποθήκευσης λίμνης δεδομένων Access

Αφού έχετε ρυθμίσει τις παραμέτρους του συμπλέγματος HDInsight για να χρησιμοποιήσετε το χώρο αποθήκευσης λίμνης δεδομένων, μπορείτε να χρησιμοποιήσετε τις εντολές κελύφους HDFS για πρόσβαση στο χώρο αποθήκευσης.

### <a name="for-a-linux-cluster"></a>Για ένα σύμπλεγμα Linux

Σε αυτό ενότητα που θα SSH στο σύμπλεγμα και να εκτελέσετε τις εντολές HDFS. Τα Windows δεν παρέχει ένα ενσωματωμένο πρόγραμμα-πελάτη SSH. Συνιστάται να χρησιμοποιείτε **PuTTY**, που μπορούν να ληφθούν από [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Για περισσότερες πληροφορίες σχετικά με τη χρήση PuTTY, ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από τα Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Μόλις συνδεθεί, χρησιμοποιήστε την ακόλουθη εντολή HDFS στο σύστημα αρχείων για μια λίστα των αρχείων στο χώρο αποθήκευσης λίμνης δεδομένων.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Αυτό θα πρέπει να αναφέρετε το αρχείο που έχετε αποστείλει προηγουμένως στο χώρο αποθήκευσης λίμνης δεδομένων.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Μπορείτε επίσης να χρησιμοποιήσετε το `hdfs dfs -put` εντολή για την αποστολή ορισμένα αρχεία στο χώρο αποθήκευσης λίμνης δεδομένων και, στη συνέχεια, χρησιμοποιήστε `hdfs dfs -ls` για να επαληθεύσετε εάν τα αρχεία έχουν αποσταλεί με επιτυχία.


### <a name="for-a-windows-cluster"></a>Για ένα σύμπλεγμα των Windows

1. Πραγματοποιήστε είσοδο νέα [Πύλη Azure](https://portal.azure.com).

2. Κάντε κλικ στο κουμπί **Αναζήτηση**, κάντε κλικ στην επιλογή **HDInsight συμπλεγμάτων**και, στη συνέχεια, κάντε κλικ στην επιλογή το σύμπλεγμα HDInsight που δημιουργήσατε.

3. Στο το blade σύμπλεγμα, κάντε κλικ στην επιλογή **Σύνδεση απομακρυσμένης επιφάνειας εργασίας**και, στη συνέχεια, στο το blade **Απομακρυσμένης επιφάνειας εργασίας** , κάντε κλικ στην επιλογή **σύνδεση**.

    ![Απομακρυσμένη σε HDI συμπλέγματος] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Δημιουργία ομάδας πόρων του Azure")

    Όταν σας ζητηθεί, πληκτρολογήστε τα διαπιστευτήρια που παρέχονται για τον απομακρυσμένο υπολογιστή χρήστη.

4. Στην απομακρυσμένη περίοδο λειτουργίας, ξεκινήστε του Windows PowerShell και χρησιμοποιήστε τις εντολές του συστήματος αρχείων HDFS για μια λίστα των αρχείων στο χώρο αποθήκευσης λίμνης Azure δεδομένων.

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    Αυτό θα πρέπει να αναφέρετε το αρχείο που έχετε αποστείλει προηγουμένως στο χώρο αποθήκευσης λίμνης δεδομένων.

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv

    Μπορείτε επίσης να χρησιμοποιήσετε το `hdfs dfs -put` εντολή για την αποστολή ορισμένα αρχεία στο χώρο αποθήκευσης λίμνης δεδομένων και, στη συνέχεια, χρησιμοποιήστε `hdfs dfs -ls` για να επαληθεύσετε εάν τα αρχεία έχουν αποσταλεί με επιτυχία.

## <a name="see-also"></a>Δείτε επίσης

* [Πύλη: Δημιουργήστε ένα σύμπλεγμα HDInsight για να χρησιμοποιήσετε το χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
