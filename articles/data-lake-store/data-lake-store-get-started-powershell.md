<properties
   pageTitle="Γρήγορα αποτελέσματα με το χώρο αποθήκευσης δεδομένων λίμνης | Azure"
   description="Χρήση του Azure PowerShell για να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης και να εκτελέσετε βασικές λειτουργίες"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/04/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Γρήγορα αποτελέσματα με χρήση του Azure PowerShell χώρου αποθήκευσης λίμνης δεδομένων Azure

> [AZURE.SELECTOR]
- [Πύλη](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure PowerShell για να δημιουργήσετε ένα λογαριασμό χώρου αποθήκευσης λίμνης Azure δεδομένων και να εκτελέσετε βασικές λειτουργίες όπως όπως δημιουργία φακέλων, αποστολή και λήψη αρχείων δεδομένων, να διαγράψετε το λογαριασμό, κ.λπ. Για περισσότερες πληροφορίες σχετικά με το χώρο αποθήκευσης λίμνης δεδομένων, ανατρέξτε στο θέμα [Επισκόπηση της λίμνης χώρου αποθήκευσης δεδομένων](data-lake-store-overview.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

* **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

* **Azure PowerShell 1.0 ή μεγαλύτερη**. Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

## <a name="authentication"></a>Έλεγχος ταυτότητας

Σε αυτό το άρθρο χρησιμοποιεί μια πιο απλά προσέγγιση τον έλεγχο ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων όταν σας ζητηθεί να εισαγάγετε τα διαπιστευτήριά σας λογαριασμός Azure. Το επίπεδο πρόσβασης σε λογαριασμό και το σύστημα αρχείων, στη συνέχεια, διέπεται από το επίπεδο πρόσβασης του χρήστη για τον συνδεδεμένο στο χώρο αποθήκευσης λίμνης δεδομένων. Ωστόσο, υπάρχουν άλλες προσεγγίσεις καθώς και για τον έλεγχο ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων, που είναι **τελικού χρήστη ελέγχου ταυτότητας** ή **τον έλεγχο ταυτότητας υπηρεσίας εξυπηρέτησης**. Για οδηγίες και περισσότερες πληροφορίες σχετικά με τον τρόπο για τον έλεγχο ταυτότητας, ανατρέξτε στο θέμα [Έλεγχος ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων χρησιμοποιώντας Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων Azure

1. Από τον υπολογιστή σας, ανοίξτε ένα νέο παράθυρο του Windows PowerShell και πληκτρολογήστε το παρακάτω τμήμα κώδικα για να συνδεθείτε στο λογαριασμό σας στο Azure, να ορίσετε τη συνδρομή και καταχώρηση της υπηρεσίας παροχής χώρου αποθήκευσης λίμνης δεδομένων. Όταν σας ζητηθεί να συνδεθείτε στο, βεβαιωθείτε ότι συνδέεστε στο ως ένα από τα admininistrators/κάτοχος της συνδρομής:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Ένα λογαριασμό χώρου αποθήκευσης λίμνης Azure δεδομένων είναι συσχετισμένη με μια ομάδα πόρων του Azure. Ξεκινήστε με τη δημιουργία μιας ομάδας πόρων Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Δημιουργία ομάδας πόρων του Azure] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Δημιουργία ομάδας πόρων του Azure")

2. Δημιουργήστε ένα λογαριασμό χώρου αποθήκευσης λίμνης Azure δεδομένων. Το όνομα που καθορίζετε πρέπει να περιέχει μόνο πεζά γράμματα και αριθμούς.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Δημιουργία λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων Azure] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Δημιουργία λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων Azure")

3. Επαληθεύστε ότι ο λογαριασμός δημιουργήθηκε με επιτυχία.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Το αποτέλεσμα για αυτό πρέπει να είναι **True**.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Δημιουργία καταλόγου δομές στο χώρο αποθήκευσης λίμνης δεδομένων Azure

Μπορείτε να δημιουργήσετε σε καταλόγους στην περιοχή ο λογαριασμός σας Azure δεδομένων λίμνης αποθήκευσης για τη διαχείριση και αποθήκευση δεδομένων.

1. Καθορίστε έναν ριζικό κατάλογο.

        $myrootdir = "/"

2. Δημιουργήστε ένα νέο κατάλογο που ονομάζεται **mynewdirectory** κάτω από το καθορισμένο ριζικό κατάλογο.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Βεβαιωθείτε ότι το νέο κατάλογο δημιουργήθηκε με επιτυχία.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Θα πρέπει να εμφανίζεται το αποτέλεσμα όπως το εξής:

    ![Επαλήθευση του καταλόγου] (./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Επαλήθευση του καταλόγου")


## <a name="upload-data-to-your-azure-data-lake-store"></a>Αποστολή δεδομένων στο χώρο αποθήκευσης λίμνης δεδομένων Azure

Μπορείτε να αποστείλετε τα δεδομένα σας στο χώρο αποθήκευσης λίμνης δεδομένων απευθείας στο επίπεδο ριζικού ή σε έναν κατάλογο που δημιουργήσατε στο λογαριασμό. Τα παρακάτω τμήματα δείχνουν πώς μπορείτε να αποστείλετε μερικά δείγματα δεδομένων στον κατάλογο (**mynewdirectory**) που δημιουργήσατε στην προηγούμενη ενότητα.

Εάν αναζητάτε μερικά δείγματα δεδομένων για την αποστολή, μπορείτε να λάβετε το φάκελο **Ασθενοφόρων δεδομένων** από το [Azure δεδομένων λίμνης Git αποθετήριο](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Κάντε λήψη του αρχείου και αποθηκεύστε το σε έναν τοπικό κατάλογο στον υπολογιστή σας, όπως C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Μετονομασία, κάντε λήψη και διαγραφή δεδομένων από το χώρο αποθήκευσης δεδομένων λίμνης

Για να μετονομάσετε ένα αρχείο, χρησιμοποιήστε την ακόλουθη εντολή:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Για να κάνετε λήψη ενός αρχείου, χρησιμοποιήστε την ακόλουθη εντολή:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Για να διαγράψετε ένα αρχείο, χρησιμοποιήστε την ακόλουθη εντολή:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Όταν σας ζητηθεί, πληκτρολογήστε **Y** για να διαγράψετε το στοιχείο. Εάν έχετε περισσότερα από ένα αρχείο για να διαγράψετε, μπορείτε να παράσχετε όλες οι διαδρομές διαχωρισμένες με κόμμα.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Διαγραφή του λογαριασμού σας χώρου αποθήκευσης λίμνης δεδομένων Azure

Χρησιμοποιήστε την παρακάτω εντολή για να διαγράψετε το λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Όταν σας ζητηθεί, πληκτρολογήστε **Y** για να διαγράψετε το λογαριασμό.


## <a name="next-steps"></a>Επόμενα βήματα

- [Διασφάλιση των δεδομένων στο χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-secure-data.md)
- [Χρήση ανάλυσης λίμνης Azure δεδομένων με το χώρο αποθήκευσης δεδομένων λίμνης](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Χρήση Azure HDInsight με το χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-hdinsight-hadoop-use-portal.md)
