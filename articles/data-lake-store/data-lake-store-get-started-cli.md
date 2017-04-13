<properties
   pageTitle="Γρήγορα αποτελέσματα με το χώρο αποθήκευσης λίμνης δεδομένων χρησιμοποιώντας περιβάλλον γραμμής εντολών πλατφόρμες | Microsoft Azure"
   description="Χρησιμοποιήστε Azure γραμμή εντολών πλατφόρμες για να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης και να εκτελέσετε βασικές λειτουργίες"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Γρήγορα αποτελέσματα με χρήση γραμμής εντολών Azure χώρου αποθήκευσης λίμνης δεδομένων Azure

> [AZURE.SELECTOR]
- [Πύλη](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure περιβάλλον γραμμής εντολών για να δημιουργήσετε ένα λογαριασμό χώρου αποθήκευσης λίμνης Azure δεδομένων και να εκτελέσετε βασικές λειτουργίες όπως όπως δημιουργία φακέλων, αποστολή και λήψη αρχείων δεδομένων, να διαγράψετε το λογαριασμό, κ.λπ. Για περισσότερες πληροφορίες σχετικά με το χώρο αποθήκευσης λίμνης δεδομένων, ανατρέξτε στο θέμα [Επισκόπηση της λίμνης χώρου αποθήκευσης δεδομένων](data-lake-store-overview.md).

Το Azure CLI έχει υλοποιηθεί σε Node.js. Μπορεί να χρησιμοποιηθεί σε οποιαδήποτε πλατφόρμα που υποστηρίζει Node.js, συμπεριλαμβανομένων των Windows, Mac και Linux. Το Azure CLI είναι ανοιχτό αρχείο προέλευσης. Ο κωδικός προέλευσης γίνεται στο GitHub στο <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. Σε αυτό το άρθρο καλύπτει χρησιμοποιώντας μόνο το Azure CLI με το χώρο αποθήκευσης λίμνης δεδομένων. Για γενικές οδηγίες σχετικά με τη χρήση Azure CLI, δείτε [πώς μπορείτε να χρησιμοποιήσετε το Azure CLI] [azure-command-line-tools].


##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI** - δείτε [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md) για πληροφορίες εγκατάστασης και ρύθμισης παραμέτρων. Βεβαιωθείτε ότι η επανεκκίνηση του υπολογιστή σας μετά την εγκατάσταση του CLI.

## <a name="authentication"></a>Έλεγχος ταυτότητας

Σε αυτό το άρθρο χρησιμοποιεί μια πιο απλά προσέγγιση τον έλεγχο ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων όπου θα συνδεθείτε ως χρήστης τελικού χρήστη. Το επίπεδο πρόσβασης σε λογαριασμό και το σύστημα αρχείων, στη συνέχεια, διέπεται από το επίπεδο πρόσβασης του χρήστη για τον συνδεδεμένο στο χώρο αποθήκευσης λίμνης δεδομένων. Ωστόσο, υπάρχουν άλλες προσεγγίσεις καθώς και για τον έλεγχο ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων, που είναι **τελικού χρήστη ελέγχου ταυτότητας** ή **τον έλεγχο ταυτότητας υπηρεσίας εξυπηρέτησης**. Για οδηγίες και περισσότερες πληροφορίες σχετικά με τον τρόπο για τον έλεγχο ταυτότητας, ανατρέξτε στο θέμα [Έλεγχος ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων χρησιμοποιώντας Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

##<a name="login-to-your-azure-subscription"></a>Συνδεθείτε στη συνδρομή σας στο Azure

1. Ακολουθήστε τα βήματα που τεκμηριώνονται σε [σύνδεση με μια συνδρομή του Azure από το περιβάλλον γραμμής εντολών Azure (Azure CLI)](../xplat-cli-connect.md) και να συνδεθείτε με σας χρησιμοποιώντας τη συνδρομή του `azure login` μέθοδο.

2. Λίστα με τις συνδρομές που συσχετίζονται με το λογαριασμό χρησιμοποιώντας το `azure account list` εντολή.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    Από την έξοδο του παραπάνω, **Azure-sub-1** είναι ενεργοποιημένες και την άλλη συνδρομή είναι **Azure-sub-2**. 

3. Επιλέξτε τη συνδρομή στην οποία θέλετε να εργαστείτε στην περιοχή. Εάν θέλετε να εργαστείτε στην περιοχή τη συνδρομή Azure-sub-2, χρησιμοποιήστε το `azure account set` εντολή.

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων Azure

Ανοίξτε μια γραμμή εντολών, εξωτερικό περίβλημα ή μιας περιόδου λειτουργίας τερματικού και εκτελέστε τις ακόλουθες εντολές.

2. Μεταβείτε σε λειτουργία διαχείρισης πόρων Azure χρησιμοποιώντας την ακόλουθη εντολή:

        azure config mode arm


5. Δημιουργία νέας ομάδας πόρων. Στην παρακάτω εντολή, δώστε τις τιμές παραμέτρων που θέλετε να χρησιμοποιήσετε.

        azure group create <resourceGroup> <location>

    Αν το όνομα της θέσης περιέχει κενά διαστήματα, τοποθετήστε το σε εισαγωγικά. Για παράδειγμα "Ανατολικής US 2".

5. Δημιουργήστε το λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Δημιουργία φακέλων στο χώρο αποθήκευσης των δεδομένων λίμνης

Μπορείτε να δημιουργήσετε φακέλους στην περιοχή ο λογαριασμός σας Azure δεδομένων λίμνης αποθήκευσης για τη διαχείριση και αποθήκευση δεδομένων. Χρησιμοποιήστε την παρακάτω εντολή για να δημιουργήσετε ένα φάκελο που ονομάζεται "mynewfolder" στη ρίζα της λίμνης χώρου αποθήκευσης δεδομένων.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Για παράδειγμα:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Αποστολή δεδομένων στο χώρο αποθήκευσης των δεδομένων λίμνης

Μπορείτε να αποστείλετε τα δεδομένα σας στο χώρο αποθήκευσης λίμνης δεδομένων απευθείας στο επίπεδο ριζικού ή σε ένα φάκελο που δημιουργήσατε στο λογαριασμό. Τα παρακάτω τμήματα δείχνουν πώς μπορείτε να αποστείλετε μερικά δείγματα δεδομένων στο φάκελο (**mynewfolder**) που δημιουργήσατε στην προηγούμενη ενότητα.

Εάν αναζητάτε μερικά δείγματα δεδομένων για την αποστολή, μπορείτε να λάβετε το φάκελο **Ασθενοφόρων δεδομένων** από το [Azure δεδομένων λίμνης Git αποθετήριο](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Κάντε λήψη του αρχείου και αποθηκεύστε το σε έναν τοπικό κατάλογο στον υπολογιστή σας, όπως C:\sampledata\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Για παράδειγμα:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Λίστα αρχείων στο χώρο αποθήκευσης δεδομένων λίμνης

Χρησιμοποιήστε την παρακάτω εντολή για να παραθέσετε τα αρχεία σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Για παράδειγμα:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Το αποτέλεσμα της παρούσας πρέπει να είναι παρόμοια με τα εξής:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Μετονομασία, κάντε λήψη και διαγραφή δεδομένων από το χώρο αποθήκευσης δεδομένων λίμνης

* **Για να μετονομάσετε ένα αρχείο**, χρησιμοποιήστε την ακόλουθη εντολή:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Για παράδειγμα:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Για να κάνετε λήψη ενός αρχείου**, χρησιμοποιήστε την ακόλουθη εντολή. Βεβαιωθείτε ότι η διαδρομή προορισμού που καθορίζετε ήδη υπάρχει.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Για παράδειγμα:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Για να διαγράψετε ένα αρχείο**, χρησιμοποιήστε την ακόλουθη εντολή:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Για παράδειγμα:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Όταν σας ζητηθεί, πληκτρολογήστε **Y** για να διαγράψετε το στοιχείο.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>Προβάλετε τη λίστα ελέγχου πρόσβασης για ένα φάκελο στο χώρο αποθήκευσης δεδομένων λίμνης

Χρησιμοποιήστε την παρακάτω εντολή για να προβάλετε το ACL σε ένα φάκελο του χώρου αποθήκευσης δεδομένων λίμνης. Στην τρέχουσα έκδοση, ACL μπορεί να οριστεί μόνο από το ριζικό κατάλογο του χώρου αποθήκευσης λίμνης δεδομένων. Επομένως, η παράμετρος διαδρομή παρακάτω θα είναι πάντα ρίζας (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Για παράδειγμα:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Διαγραφή του λογαριασμού σας χώρου αποθήκευσης δεδομένων λίμνης

Χρησιμοποιήστε την παρακάτω εντολή για να διαγράψετε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.

    azure datalake store account delete <dataLakeStoreAccountName>

Για παράδειγμα:

    azure datalake store account delete mynewdatalakestore

Όταν σας ζητηθεί, πληκτρολογήστε **Y** για να διαγράψετε το λογαριασμό.


## <a name="next-steps"></a>Επόμενα βήματα

- [Διασφάλιση των δεδομένων στο χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-secure-data.md)
- [Χρήση ανάλυσης λίμνης Azure δεδομένων με το χώρο αποθήκευσης δεδομένων λίμνης](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Χρήση Azure HDInsight με το χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md
