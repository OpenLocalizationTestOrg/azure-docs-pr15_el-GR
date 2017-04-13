<properties
    pageTitle="Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob Azure μέσω Python | Microsoft Azure"
    description="Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob Azure μέσω Python"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob Azure μέσω Python

Αυτό το θέμα περιγράφει τον τρόπο λίστας, αποστολή και λήψη αντικείμενα blob με χρήση του API Python. Με το API Python που παρέχονται στο Azure SDK, μπορείτε να:

- Δημιουργία κοντέινερ
- Αποστείλετε ένα blob σε ένα κοντέινερ
- Λήψη αντικείμενα BLOB
- Λίστα με τα αντικείμενα BLOB σε ένα κοντέινερ
- Διαγράψτε ένα blob

Για περισσότερες πληροφορίες σχετικά με τη χρήση του API Python, δείτε [πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία αποθήκευσης αντικειμένων Blob του Python](../storage/storage-python-how-to-use-blob-storage.md).

Οδηγίες για τις τεχνολογίες που χρησιμοποιούνται για τη μετακίνηση δεδομένων σε ή/και από το χώρο αποθήκευσης αντικειμένων Blob του Azure είναι συνδεδεμένες εδώ:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Εάν χρησιμοποιείτε Εικονική που έχει ρυθμιστεί με τις δέσμες ενεργειών που παρέχονται από [δεδομένα Science εικονικού μηχανές στο Azure](machine-learning-data-science-virtual-machines.md), στη συνέχεια, AzCopy είναι ήδη εγκατεστημένο στον η Εικονική.

> [AZURE.NOTE] Για μια πλήρη εισαγωγή με το χώρο αποθήκευσης αντικειμένων blob του Azure, ανατρέξτε για [Βασικά στοιχεία αντικειμένων Blob του Azure](../storage/storage-dotnet-how-to-use-blobs.md) και την [Υπηρεσία αντικειμένων Blob του Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το έγγραφο προϋποθέτει ότι έχετε μια συνδρομή του Azure, ένα λογαριασμό του χώρου αποθήκευσης και το αντίστοιχο κλειδί χώρου αποθήκευσης για τον συγκεκριμένο λογαριασμό. Πριν από την αποστολή/λήψη δεδομένων, πρέπει να γνωρίζετε Azure χώρου αποθήκευσης και το όνομα του λογαριασμού το κλειδί λογαριασμού σας.

- Για να ρυθμίσετε μια συνδρομή του Azure, ανατρέξτε στο θέμα [δωρεάν δοκιμαστική έκδοση ενός μήνα](https://azure.microsoft.com/pricing/free-trial/).

- Για οδηγίες σχετικά με τη δημιουργία ενός λογαριασμού χώρου αποθήκευσης και γρήγορα λογαριασμό και βασικές πληροφορίες, ανατρέξτε στο θέμα [σχετικά με το Azure λογαριασμοί χώρου αποθήκευσης](../storage/storage-create-storage-account.md).


## <a name="upload-data-to-blob"></a>Αποστολή δεδομένων στο Blob

Προσθέστε το παρακάτω τμήμα κώδικα κοντά στο επάνω μέρος κάθε Python κώδικα στην οποία θέλετε να πρόσβαση μέσω προγραμματισμού αποθήκευσης Azure:

    from azure.storage.blob import BlobService

Το αντικείμενο **BlobService** σάς επιτρέπει να εργάζεστε με το κοντέινερ και αντικείμενα blob. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο BlobService χρησιμοποιώντας το κλειδί αποθήκευσης λογαριασμού όνομα και το λογαριασμό. Αντικαταστήστε το όνομα του λογαριασμού και το κλειδί λογαριασμού με το πραγματικό λογαριασμού και αριθμού-κλειδιού.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Χρησιμοποιήστε τις παρακάτω μεθόδους για την αποστολή δεδομένων σε ένα αντικείμενο blob:

1. τοποθέτηση\_μπλοκ\_blob\_από\_διαδρομή (αποστέλλει τα περιεχόμενα του αρχείου από την καθορισμένη διαδρομή)
2. τοποθέτηση\_block_blob\_από\_αρχείου (κάνει αποστολή των περιεχομένων από ένα ήδη ανοικτό αρχείο/ροή)
3. τοποθέτηση\_μπλοκ\_blob\_από\_byte που (αποστολές ένα πίνακα byte)
4. τοποθέτηση\_μπλοκ\_blob\_από\_κειμένου (στέλνει την τιμή καθορισμένο κείμενο χρησιμοποιώντας την καθορισμένη κωδικοποίηση)

Το παρακάτω δείγμα κώδικα αποστέλλει ένα τοπικό αρχείο σε ένα κοντέινερ:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Το παρακάτω δείγμα κώδικα μεταφέρει όλα τα αρχεία (εκτός από καταλόγους) σε έναν τοπικό κατάλογο με το χώρο αποθήκευσης αντικειμένων blob:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Λήψη δεδομένων από το Blob

Χρησιμοποιήστε τις παρακάτω μεθόδους για να κάνετε λήψη δεδομένων από ένα blob:
1. Λήψη\_blob\_να\_διαδρομή
2. Λήψη\_blob\_να\_αρχείου
3. Λήψη\_blob\_να\_byte που
4. Λήψη\_blob\_να\_κειμένου

Αυτές οι μέθοδοι που εκτελούν τα απαραίτητα chunking όταν το μέγεθος των δεδομένων που υπερβαίνει τα 64 MB.

Το παρακάτω δείγμα κώδικα λήψεις των περιεχομένων μιας blob σε ένα κοντέινερ σε ένα τοπικό αρχείο:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Το παρακάτω δείγμα κώδικα κάνει λήψη όλων των BLOB από ένα κοντέινερ. Χρησιμοποιεί λίστα\_αντικείμενα BLOB για να λάβετε τη λίστα διαθέσιμα αντικείμενα blob στο κοντέινερ και τις λήψεις σε έναν τοπικό κατάλογο.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
