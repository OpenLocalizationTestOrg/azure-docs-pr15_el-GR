<properties
    pageTitle="Πρόγραμμα εκμάθησης - γρήγορα αποτελέσματα με το πρόγραμμα-πελάτη Python δέσμη Azure | Microsoft Azure"
    description="Μάθετε τις βασικές έννοιες των δέσμη Azure και πώς μπορείτε να αναπτύξετε την υπηρεσία δέσμη με ένα απλό σενάριο"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/27/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-python-client"></a>Γρήγορα αποτελέσματα με το πρόγραμμα-πελάτη Python δέσμη Azure

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Μάθετε τα βασικά στοιχεία της [Δέσμης Azure] [ azure_batch] και τη [Δέσμη Python] [ py_azure_sdk] προγράμματος-πελάτη όπως αναλύονται μια μικρή εφαρμογή δέσμης γραμμένο σε Python. Εξετάσουμε πώς δύο δειγμάτων δεσμών ενεργειών χρήση της υπηρεσίας μαζική επεξεργασία μια παράλληλη φόρτο εργασίας σε εικονικές μηχανές Linux στο cloud και πώς αλληλεπιδρούν με [Αποθήκευσης Azure](./../storage/storage-introduction.md) για το αρχείο ενδιάμεσου σταδίου και ανάκτησης. Θα μάθετε κοινές ροής εργασίας εφαρμογή δέσμης και αποκτήσετε μια βασική κατανόηση τα κύρια στοιχεία της δέσμης όπως εργασίες, εργασίες, χώρους συγκέντρωσης, και τον υπολογισμό κόμβους.

![Μαζική λύση ροής εργασίας (basic)][11]<br/>

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Σε αυτό το άρθρο προϋποθέτει ότι έχετε κάποια εμπειρία Python και εξοικείωση με Linux. Επίσης, προϋποθέτει ότι είστε μπορούν να πληρούν τις απαιτήσεις δημιουργίας λογαριασμού που έχουν καθοριστεί κάτω από το Azure και τη δέσμη και τις υπηρεσίες αποθήκευσης.

### <a name="accounts"></a>Λογαριασμοί

- **Λογαριασμός Azure**: Εάν δεν έχετε ήδη μια συνδρομή Azure, [Δημιουργήστε έναν δωρεάν λογαριασμό Azure][azure_free_account].
- **Μαζική λογαριασμό**: όταν έχετε μια συνδρομή Azure, [Δημιουργήστε ένα λογαριασμό δέσμη Azure](batch-account-create-portal.md).
- **Το λογαριασμό χώρου αποθήκευσης**: ανατρέξτε στο θέμα [Δημιουργία λογαριασμού χώρου αποθήκευσης](../storage/storage-create-storage-account.md#create-a-storage-account) στο [Λογαριασμοί Azure σχετικά με το χώρο αποθήκευσης](../storage/storage-create-storage-account.md).

### <a name="code-sample"></a>Δείγμα κώδικα

Εκμάθηση Python [δείγμα κώδικα] [ github_article_samples] ένα από τα πολλά δείγματα κώδικα δέσμης βρίσκεται στο [azure-δέσμη-δείγματα] [ github_samples] αποθετήριο δεδομένων σε GitHub. Μπορείτε να κάνετε λήψη όλων των δειγμάτων κάνοντας κλικ στην επιλογή **κλωνοποίηση ή λήψη > λήψη ZIP** στην αρχική σελίδα του αποθετηρίου ή κάνοντας κλικ στην επιλογή το [azure-δέσμη-δείγματα-master.zip] [ github_samples_zip] Άμεση λήψη σύνδεσης. Αφού έχετε εξαγάγατε τα περιεχόμενα του αρχείου ZIP, των δύο δεσμών ενεργειών για αυτό το πρόγραμμα εκμάθησης βρίσκονται σε το `article_samples` καταλόγου:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Περιβάλλον Python

Για να εκτελέσετε το δείγμα δέσμης ενεργειών *python_tutorial_client.py* στην τον τοπικό σταθμούς εργασίας, χρειάζεστε ένα **μεταφραστή Python** συμβατό με την έκδοση **2.7** ή **3,3 +**. Η δέσμη ενεργειών έχει ελεγχθεί Linux και Windows.

### <a name="cryptography-dependencies"></a>εξαρτήσεις κρυπτογράφησης

Πρέπει να εγκαταστήσετε τις εξαρτήσεις για την [κρυπτογράφηση] [ crypto] βιβλιοθήκη, απαιτείται από το `azure-batch` και `azure-storage` Python πακέτων. Εκτελέστε μία από τις ακόλουθες λειτουργίες που είναι κατάλληλη για την πλατφόρμα σας ή ανατρέξτε στην [εγκατάσταση κρυπτογράφησης] [ crypto_install] λεπτομέρειες για περισσότερες πληροφορίες:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* Windows

    `pip install cryptography`

>[AZURE.NOTE] Εάν εγκαταστήσετε για Python 3.3 + σε Linux, χρησιμοποιήστε τα ισοδύναμα python3 για τις εξαρτήσεις Python. Για παράδειγμα, στο Ubuntu:`apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### <a name="azure-packages"></a>Πακέτα Azure

Στη συνέχεια, εγκαταστήστε τα πακέτα **Δέσμη Azure** και **Αποθήκευσης Azure** Python. Μπορείτε να το κάνετε αυτό με **pip** και *requirements.txt* εδώ:

`/azure-batch-samples/Python/Batch/requirements.txt`

Θέμα ακολουθώντας **pip** εντολή για να εγκαταστήσετε τα πακέτα δέσμης και αποθήκευσης:

`pip install -r requirements.txt`

Ή, μπορείτε να εγκαταστήσετε τη [δέσμη azure] [ pypi_batch] και [αποθήκευσης azure] [ pypi_storage] Python πακέτων με μη αυτόματο τρόπο:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [AZURE.TIP] Ίσως χρειαστεί να τις εντολές με πρόθεμα `sudo` Εάν χρησιμοποιείτε λογαριασμό χωρίς δικαιώματα. Για παράδειγμα, `sudo pip install -r requirements.txt`. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση πακέτων Python, ανατρέξτε στο θέμα [Εγκατάσταση πακέτων] [ pypi_install] σε readthedocs.io.

## <a name="batch-python-tutorial-code-sample"></a>Δείγμα προγραμμάτων εκμάθησης κώδικα Python δέσμης

Το δείγμα προγραμμάτων εκμάθησης κώδικα δέσμης Python αποτελείται από δύο Python δεσμών ενεργειών και μερικά αρχεία δεδομένων.

- **python_tutorial_client.PY**: αλληλεπιδρά με τη δέσμη και τις υπηρεσίες αποθήκευσης για να εκτελέσετε μια παράλληλη φόρτο εργασίας σε κόμβους υπολογιστικών (εικονικές μηχανές). Η δέσμη ενεργειών *python_tutorial_client.py* εκτελείται σε τον τοπικό σταθμούς εργασίας.

- **python_tutorial_task.PY**: Η δέσμη ενεργειών που εκτελείται στον τον υπολογισμό τους κόμβους Azure για να εκτελέσετε την πραγματική εργασία. Στο δείγμα, *python_tutorial_task.py* αναλύει το κείμενο σε ένα αρχείο που έχουν ληφθεί από χώρο αποθήκευσης Azure (το αρχείο εισαγωγής). Στη συνέχεια, δημιουργεί ένα αρχείο κειμένου (το αρχείο εξόδου) που περιέχει μια λίστα με τις καλύτερες τρεις λέξεις που εμφανίζονται στο αρχείο εισαγωγής. Αφού που δημιουργεί το αρχείο εξόδου, *python_tutorial_task.py* αποστέλλει το αρχείο με το χώρο αποθήκευσης Azure. Αυτό καθιστά διαθέσιμο για λήψη στη δέσμη ενεργειών προγράμματος-πελάτη που εκτελείται σε σας σταθμούς εργασίας. Η δέσμη ενεργειών *python_tutorial_task.py* εκτελείται παράλληλα σε πολλούς κόμβους υπολογιστικών στην υπηρεσία δέσμης.

- **./data/taskdata\*.txt**: αυτά τα αρχεία τρεις κειμένου παρέχουν την είσοδο για τις εργασίες που εκτελούνται σε κόμβους υπολογιστικών.

Το παρακάτω διάγραμμα παρουσιάζει τα κύρια λειτουργίες που εκτελούνται μέσω των δεσμών ενεργειών του υπολογιστή-πελάτη και εργασιών. Αυτή η ροή εργασίας βασικές είναι τυπικό πολλές λύσεις υπολογισμού που δημιουργούνται με τη μαζική. Ενώ δεν υποδεικνύει κάθε δυνατότητα που είναι διαθέσιμη στην υπηρεσία δέσμη, σχεδόν κάθε σενάριο δέσμη περιλαμβάνει τμήματα αυτής της ροής εργασίας.

![Μαζική παραδείγματος ροής εργασίας][8]<br/>

[**Βήμα 1.**](#step-1-create-storage-containers) Δημιουργία **κοντέινερ** στο χώρο αποθήκευσης Blob του Azure.<br/>
[**Βήμα 2.**](#step-2-upload-task-script-and-data-files) Αποστολή αρχείων δέσμης ενεργειών και εισαγωγή εργασιών στο κοντέινερ.<br/>
[**Βήμα 3.**](#step-3-create-batch-pool) Δημιουργήστε μια δέσμη **χώρου συγκέντρωσης**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3α.** Το χώρο συγκέντρωσης **StartTask** στοιχεία λήψης της δέσμης ενεργειών εργασίας (python_tutorial_task.py) σε κόμβους όπως συνδέονται το χώρο συγκέντρωσης.<br/>
[**Βήμα 4.**](#step-4-create-batch-job) Δημιουργήστε μια μαζική **εργασία**.<br/>
[**Βήμα 5.**](#step-5-add-tasks-to-job) Προσθήκη **εργασιών** στο έργο.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5α.** Οι εργασίες προγραμματίζονται να εκτελέσει σε κόμβους.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Κάθε εργασία λήψεις των εισαγωγής δεδομένων από το χώρο αποθήκευσης Azure και, στη συνέχεια, αρχίζει εκτέλεσης.<br/>
[**Βήμα 6.**](#step-6-monitor-tasks) Παρακολούθηση εργασιών.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Κατά την ολοκλήρωση των εργασιών, αποστείλετε τα δεδομένα εξόδου τους με το χώρο αποθήκευσης Azure.<br/>
[**Βήμα 7.**](#step-7-download-task-output) Κάντε λήψη του εξόδου εργασίας από το χώρο αποθήκευσης.

Όπως αναφέρθηκε προηγουμένως, δεν κάθε λύση δέσμη εκτελεί αυτά τα ακριβή βήματα και ενδέχεται να περιλαμβάνουν πολλές περισσότερες, αλλά αυτό δείγμα παρουσιάζει κοινών διαδικασιών που βρίσκονται σε μια λύση δέσμης.

## <a name="prepare-client-script"></a>Προετοιμασία δέσμης ενεργειών του προγράμματος-πελάτη

Πριν να εκτελέσετε το δείγμα, προσθέστε τα διαπιστευτήριά σας δέσμης και αποθήκευσης λογαριασμού *python_tutorial_client.py*. Εάν δεν το έχετε κάνει ήδη, ανοίξτε το αρχείο στο πρόγραμμα επεξεργασίας Αγαπημένα και ενημερώστε τις ακόλουθες γραμμές με τα διαπιστευτήριά σας.

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

Μπορείτε να βρείτε τη δέσμη και αποθήκευσης λογαριασμού τα διαπιστευτήριά σας εντός του λογαριασμού blade κάθε υπηρεσίας στην [πύλη του Azure][azure_portal]:

![Μαζική διαπιστευτήρια στην πύλη του][9]
![χώρο αποθήκευσης διαπιστευτηρίων στην πύλη][10]<br/>

Στις ενότητες που ακολουθούν, θα σας να αναλύσετε τα βήματα που χρησιμοποιούνται από τις δέσμες ενεργειών για ένα φόρτο εργασίας στην υπηρεσία μαζική επεξεργασία. Συνιστάται να αναφέρεται τακτικά τις δέσμες ενεργειών στο πρόγραμμα επεξεργασίας κατά την εργασία σας τρόπο στις υπόλοιπες αυτού του άρθρου.

Περιήγηση με την ακόλουθη γραμμή στο **python_tutorial_client.py** για να ξεκινήσετε με το βήμα 1:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Βήμα 1: Δημιουργία χώρους αποθήκευσης

![Δημιουργία κοντέινερ στο χώρο αποθήκευσης Azure][1]
<br/>

Μαζική περιλαμβάνει ενσωματωμένη υποστήριξη για αλληλεπίδραση με το χώρο αποθήκευσης Azure. Θα σας δώσει τα αρχεία που απαιτούνται από τις εργασίες που εκτελούνται στο λογαριασμό σας δέσμη κοντέινερ στο λογαριασμό σας στο χώρο αποθήκευσης. Το κοντέινερ παρέχουν επίσης μια θέση για την αποθήκευση των δεδομένων εξόδου που παράγουν τις εργασίες. Το πρώτο πράγμα που κάνει τη δέσμη ενεργειών *python_tutorial_client.py* είναι δημιουργία κοντέινερ τρεις στο [Χώρο αποθήκευσης Blob του Azure](../storage/storage-introduction.md#blob-storage):

- **εφαρμογή**: αυτό το κοντέινερ θα αποθηκεύσει την Python δέσμη ενεργειών που εκτελούνται από τις εργασίες, *python_tutorial_task.py*.
- **εισαγωγής**: εργασίες θα κάνετε λήψη των αρχείων δεδομένων για να επαναλάβετε τη διαδικασία από το κοντέινερ *εισαγωγής* .
- **αποτέλεσμα**: όταν εργασίες ολοκλήρωσης Επεξεργασία αρχείου εισαγωγής, αυτά θα αποσταλούν τα αποτελέσματα στο κοντέινερ *εξόδου* .

Για να αλληλεπιδράσετε με ένα λογαριασμό του χώρου αποθήκευσης και δημιουργία κοντέινερ, χρησιμοποιούμε τη [αποθήκευσης azure] [ pypi_storage] πακέτου για να δημιουργήσετε ένα [BlockBlobService] [ py_blockblobservice] αντικείμενο--το "blob πρόγραμμα-πελάτη." Στη συνέχεια, μπορούμε να δημιουργήσουμε τρεις κοντέινερ στο χώρο αποθήκευσης λογαριασμού με χρήση του προγράμματος-πελάτη blob.

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

Αφού δημιουργηθεί το κοντέινερ, η εφαρμογή τώρα να αποστείλετε τα αρχεία που θα χρησιμοποιηθεί από τις εργασίες.

> [AZURE.TIP] [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob του Azure από Python](../storage/storage-python-how-to-use-blob-storage.md) παρέχει μια επαρκής Επισκόπηση της εργασίας με χώρους αποθήκευσης Azure και αντικείμενα blob. Καθώς αρχίζετε να εργάζεστε με τη μαζική, θα πρέπει να κοντά στην κορυφή της λίστας σας ανάγνωσης.

## <a name="step-2-upload-task-script-and-data-files"></a>Βήμα 2: Αποστολή αρχείων εργασίας δέσμης ενεργειών και δεδομένων

![Αποστολή εφαρμογή εργασιών και αρχείων εισαγωγής (δεδομένα) για κοντέινερ][2]
<br/>

Στη λειτουργία αποστολής αρχείου, *python_tutorial_client.py* ορίζει πρώτα συλλογές της **εφαρμογής** και **εισαγωγής** διαδρομές αρχείων που υπάρχουν στον τοπικό υπολογιστή. Στη συνέχεια, αποστέλλει αυτά τα αρχεία για να τα κοντέινερ που δημιουργήσατε στο προηγούμενο βήμα.

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

Χρησιμοποιώντας την κατανόηση λίστα, η `upload_file_to_container` συνάρτηση ονομάζεται για κάθε αρχείο των συλλογών και δύο [ResourceFile] [ py_resource_file] συμπληρώνονται συλλογών. Το `upload_file_to_container` συνάρτηση εμφανίζεται κάτω από:

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles

Μια [ResourceFile] [ py_resource_file] παρέχει εργασίες στο δέσμη με τη διεύθυνση URL για ένα αρχείο στο χώρο αποθήκευσης Azure που έχουν ληφθεί σε έναν κόμβο υπολογισμού πριν εκτελέσετε αυτήν την εργασία. Το [ResourceFile][py_resource_file]. η ιδιότητα **blob_source** Καθορίζει την πλήρη διεύθυνση URL του αρχείου που υπάρχει στο χώρο αποθήκευσης Azure. Η διεύθυνση URL ενδέχεται να περιλαμβάνουν μια υπογραφή κοινόχρηστη πρόσβαση (συσχετισμών Ασφαλείας) που παρέχει ασφαλή πρόσβαση στο αρχείο. Οι περισσότεροι τύποι εργασιών σε δέσμη περιλαμβάνουν μια ιδιότητα *ResourceFiles* , όπως:

- [CloudTask][py_task]
- [StartTask][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

Αυτό το δείγμα δεν χρησιμοποιεί τους τύπους εργασίας JobPreparationTask ή JobReleaseTask, αλλά μπορείτε να διαβάσετε περισσότερα σχετικά με τους στο [εργασία προετοιμασίας και ολοκλήρωση εργασιών στη δέσμη Azure κόμβους τον υπολογισμό](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Κοινόχρηστη πρόσβαση υπογραφή (συσχετισμών Ασφαλείας)

Κοινόχρηστη πρόσβαση υπογραφών είναι συμβολοσειρές που παρέχουν ασφαλή πρόσβαση σε κοντέινερ και αντικείμενα blob στο χώρο αποθήκευσης Azure. Η δέσμη ενεργειών *python_tutorial_client.py* χρησιμοποιεί δύο blob και κοντέινερ υπογραφές access για κοινή χρήση και παρουσιάζει πώς μπορείτε να λάβετε αυτές τις συμβολοσειρές υπογραφή κοινόχρηστη πρόσβαση από την υπηρεσία αποθήκευσης.

- **BLOB θέσει σε κοινή χρήση υπογραφών πρόσβασης**: χρησιμοποιεί το χώρο συγκέντρωσης StartTask αντικειμένων blob υπογραφές κοινόχρηστη πρόσβαση κατά τη λήψη των αρχείων δεδομένων εργασίας δέσμης ενεργειών και εισαγωγή δεδομένων από το χώρο αποθήκευσης (ανατρέξτε στο [βήμα #3](#step-3-create-batch-pool) παρακάτω). Το `upload_file_to_container` συνάρτηση σε *python_tutorial_client.py* περιέχει τον κωδικό που λαμβάνει κάθε blob κοινόχρηστη πρόσβαση υπογραφής. Αυτό γίνεται με κλήση [BlockBlobService.make_blob_url] [ py_make_blob_url] στη λειτουργική μονάδα χώρου αποθήκευσης.

- **Υπογραφή κοινόχρηστη πρόσβαση κοντέινερ**: όπως κάθε εργασία ολοκληρώσει την εργασία στον κόμβο του υπολογισμού, αποστέλλει το αρχείο εξόδου στο κοντέινερ *εξόδου* στο χώρο αποθήκευσης Azure. Για να το κάνετε, *python_tutorial_task.py* χρησιμοποιεί μια υπογραφή κοινόχρηστη πρόσβαση κοντέινερ που παρέχει πρόσβαση εγγραφής στο κοντέινερ. Το `get_container_sas_token` συνάρτηση σε *python_tutorial_client.py* λαμβάνει το κοντέινερ κοινόχρηστη πρόσβαση υπογραφής, η οποία διέρχεται ως ένα όρισμα της γραμμής εντολών για τις εργασίες. Βήμα #5, [Προσθήκη εργασιών σε ένα έργο](#step-5-add-tasks-to-job), ασχολείται με τη χρήση του κοντέινερ συσχετισμών Ασφαλείας.

> [AZURE.TIP] Ελέγξτε τη σειρά δύο τμημάτων στην κοινόχρηστη πρόσβαση υπογραφές, [μέρος 1: Κατανόηση του μοντέλου συσχετισμών Ασφαλείας](../storage/storage-dotnet-shared-access-signature-part-1.md) και [μέρος 2: δημιουργία και χρήση ενός συσχετισμών Ασφαλείας με την υπηρεσία Blob](../storage/storage-dotnet-shared-access-signature-part-2.md), για να μάθετε περισσότερα σχετικά με την παροχή ασφαλή πρόσβαση σε δεδομένα στο λογαριασμό σας στο χώρο αποθήκευσης.

## <a name="step-3-create-batch-pool"></a>Βήμα 3: Δημιουργία δέσμης συγκέντρωσης

![Δημιουργία χώρου συγκέντρωσης δέσμης][3]
<br/>

Μια δέσμη **χώρου συγκέντρωσης** είναι μια συλλογή από κόμβους υπολογιστικών (εικονικές μηχανές) στην οποία δέσμη εκτελεί τις εργασίες ενός έργου.

Αφού το μεταφέρει τα αρχεία δέσμης ενεργειών και τα δεδομένα του εργασιών με το λογαριασμό χώρου αποθήκευσης, *python_tutorial_client.py* ξεκινά την επικοινωνία με την υπηρεσία δέσμη με τη χρήση της λειτουργικής μονάδας Python δέσμης. Για να το κάνετε αυτό, μια [BatchServiceClient] [ py_batchserviceclient] δημιουργείται:

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

Στη συνέχεια, δημιουργείται ένα χώρο συγκέντρωσης κόμβους υπολογιστικών στο λογαριασμό δέσμη με μια κλήση για να `create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Όταν δημιουργείτε ένα χώρο συγκέντρωσης, ορίζετε μια [PoolAddParameter] [ py_pooladdparam] που καθορίζει πολλές ιδιότητες για το χώρο συγκέντρωσης:

- **Αναγνωριστικό** του χώρου συγκέντρωσης (*αναγνωριστικό* - απαιτείται)<p/>Όπως συμβαίνει με τα περισσότερα οντοτήτων στη δέσμη, το νέο σύνολο πρέπει να έχει ένα μοναδικό Αναγνωριστικό μέσα στο λογαριασμό σας δέσμης. Τον κωδικό που αναφέρεται σε αυτό το σύνολο χρησιμοποιώντας το Αναγνωριστικό και είναι πώς μπορείτε να αναγνωρίσετε το χώρο συγκέντρωσης στην [πύλη του]Azure[azure_portal].

- **Αριθμός κόμβους υπολογιστικών** (*target_dedicated* - απαιτείται)<p/>Αυτή η ιδιότητα καθορίζει πόσες ΣΠΣ θα πρέπει να αναπτυχθούν στο χώρο συγκέντρωσης. Είναι σημαντικό να γνωρίζετε ότι όλοι οι λογαριασμοί δέσμη έχει ένα προεπιλεγμένο **όριο** που περιορίζει τον αριθμό των **πυρήνων** (και συνεπώς, κόμβους υπολογιστικών) σε ένα λογαριασμό δέσμης. Μπορείτε να βρείτε η προεπιλεγμένη ορίων και οδηγίες σχετικά με τον τρόπο για να [αυξήσετε ένα όριο](batch-quota-limit.md#increase-a-quota) (όπως ο μέγιστος αριθμός πυρήνων στο λογαριασμό σας δέσμη) σε [όρια και περιορισμοί για την υπηρεσία δέσμη Azure](batch-quota-limit.md). Εάν δεν μπορείτε να βρείτε τον εαυτό σας ζητά "Γιατί δεν μου χώρος συγκέντρωσης επίτευξη περισσότερες από X κόμβους;" Αυτό το όριο πυρήνα μπορεί να είναι η αιτία.

- **Λειτουργικό σύστημα** για τους κόμβους (*virtual_machine_configuration* **ή** *cloud_service_configuration* - απαιτείται)<p/>Στο *python_tutorial_client.py*, μπορούμε να δημιουργήσουμε χώρου συγκέντρωσης κόμβους Linux χρησιμοποιώντας μια [VirtualMachineConfiguration][py_vm_config]. Το `select_latest_verified_vm_image_with_node_agent_sku` λειτουργούν σε `common.helpers` απλοποιεί την εργασία με [Azure Marketplace εικονικές μηχανές] [ vm_marketplace] εικόνες. Για περισσότερες πληροφορίες σχετικά με τη χρήση εικόνων Marketplace, ανατρέξτε στο θέμα [Linux παροχή τον υπολογισμό τους κόμβους δέσμη Azure χώρους συγκέντρωσης](batch-linux-nodes.md) .

- **Μέγεθος του κόμβους υπολογιστικών** (*vm_size* - απαιτείται)<p/>Επειδή αυτό καθορίζουμε Linux κόμβους για μας [VirtualMachineConfiguration][py_vm_config], θα σας Καθορίστε το μέγεθος Εικονική (`STANDARD_A1` σε αυτό το δείγμα) από [μεγέθη για εικονικές μηχανές στο Azure](../virtual-machines/virtual-machines-linux-sizes.md). Ξανά, ανατρέξτε στο θέμα [Linux παροχή τον υπολογισμό τους κόμβους χώρους συγκέντρωσης Azure δέσμη](batch-linux-nodes.md) για περισσότερες πληροφορίες.

- **Έναρξη εργασίας** (*start_task* - δεν απαιτείται)<p/>Μαζί με το επάνω από το φυσικό κόμβο ιδιότητες, μπορείτε επίσης να καθορίσετε μια [StartTask] [ py_starttask] για το χώρο συγκέντρωσης (δεν είναι απαραίτητο). Το StartTask εκτελείται σε κάθε κόμβο όπως αυτόν τον κόμβο συνδέει το χώρο συγκέντρωσης και κάθε φορά που γίνεται επανεκκίνηση έναν κόμβο. Το StartTask είναι ιδιαίτερα χρήσιμη για την προετοιμασία κόμβους υπολογιστικών για την εκτέλεση των εργασιών, όπως την εγκατάσταση των εφαρμογών που εκτελούνται τις εργασίες σας.<p/>Σε αυτήν την εφαρμογή δείγματος, το StartTask αντιγράφει τα αρχεία που να κάνει λήψη από το χώρο αποθήκευσης (που καθορίζονται, χρησιμοποιώντας το StartTask **resource_files** ιδιότητα) από το StartTask *κατάλογος εργασίας* στον *κοινόχρηστο* κατάλογο που να αποκτήσετε πρόσβαση σε όλες τις εργασίες που εκτελούνται στον κόμβο. Ουσιαστικά, αυτή η ενέργεια αντιγράφει `python_tutorial_task.py` στον κοινόχρηστο κατάλογο σε κάθε κόμβο όπως ο κόμβος συνδέει το χώρο συγκέντρωσης, έτσι ώστε οι εργασίες που εκτελούνται στον κόμβο μπορούν να έχουν πρόσβαση.

Ενδέχεται να παρατηρήσετε την κλήση του `wrap_commands_in_shell` συνάρτηση Βοήθειας. Αυτή η συνάρτηση λαμβάνει μια συλλογή ξεχωριστή εντολών και δημιουργεί μια απλή γραμμή εντολών κατάλληλο για την ιδιότητα γραμμή εντολών μιας εργασίας.

Δυνατότητα σημείωσης στο επάνω τμήμα κώδικα είναι επίσης τη χρήση των δύο μεταβλητές περιβάλλοντος στην ιδιότητα **command_line** το StartTask: `AZ_BATCH_TASK_WORKING_DIR` και `AZ_BATCH_NODE_SHARED_DIR`. Κάθε κόμβος μέσα σε ένα χώρο συγκέντρωσης δέσμη ρυθμίζεται αυτόματα με πολλές μεταβλητές περιβάλλοντος που είναι ειδικές για μαζική. Κάθε διεργασίας που εκτελείται από μια εργασία έχει πρόσβαση σε αυτές τις μεταβλητές περιβάλλοντος.

> [AZURE.TIP] Για να μάθετε περισσότερα σχετικά με τις μεταβλητές περιβάλλοντος που είναι διαθέσιμες σε κόμβους υπολογισμού σε ένα χώρο συγκέντρωσης δέσμη, καθώς και πληροφορίες σχετικά με την εργασία σε καταλόγους εργασίας, ανατρέξτε στο θέμα **ρυθμίσεις περιβάλλοντος για τις εργασίες** και **τα αρχεία και τους καταλόγους** στην [Επισκόπηση των δυνατοτήτων δέσμης Azure](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Βήμα 4: Δημιουργία μαζική εργασία

![Δημιουργία μαζική εργασία][4]<br/>

Μια μαζική **εργασία** είναι μια συλλογή των εργασιών και συσχετίζεται με ένα χώρο συγκέντρωσης κόμβους υπολογιστικών. Εκτέλεση των εργασιών σε ένα έργο σε κόμβους υπολογιστικών το σχετικό χώρο συγκέντρωσης.

Μπορείτε να χρησιμοποιήσετε μια εργασία, όχι μόνο για την οργάνωση και την παρακολούθηση των εργασιών στο σχετικό φόρτους εργασίας, αλλά και για την επιβολή ορισμένους περιορισμούς--όπως το μέγιστο χρόνο εκτέλεσης του έργου (και από την επέκταση, τις εργασίες) και εργασίας προτεραιότητα σε σχέση με άλλες εργασίες στο λογαριασμό δέσμης. Σε αυτό το παράδειγμα, ωστόσο, η εργασία σχετίζεται μόνο με το χώρο συγκέντρωσης που δημιουργήθηκε στο βήμα #3. Πρόσθετες ιδιότητες δεν έχουν ρυθμιστεί.

Όλες οι εργασίες δέσμη σχετίζονται με ένα συγκεκριμένο σύνολο. Αυτή η συσχέτιση υποδεικνύει ποια κόμβους εκτελεί εργασίες του έργου σε. Μπορείτε να καθορίσετε το χώρο συγκέντρωσης χρησιμοποιώντας το [PoolInformation] [ py_poolinfo] ιδιότητα, όπως φαίνεται στην παρακάτω τμήμα κώδικα.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Τώρα που έχει δημιουργηθεί μια εργασία, εργασίες προστίθενται για να εκτελέσετε την εργασία.

## <a name="step-5-add-tasks-to-job"></a>Βήμα 5: Προσθήκη εργασιών έργου

![Προσθήκη εργασιών στο έργο][5]<br/>
*(1) εργασίες προστίθενται στην εργασία, (2) οι εργασίες είναι προγραμματισμένες για να εκτελέσετε σε κόμβους και (3) τις εργασίες, κάντε λήψη των αρχείων δεδομένων για την επεξεργασία*

Μαζική **εργασίες** είναι οι επιμέρους μονάδες της εργασίας που εκτελούνται σε κόμβους υπολογιστικών. Μια εργασία έχει μια γραμμή εντολών και εκτελεί τις δέσμες ενεργειών ή εκτελέσιμα αρχεία που καθορίζετε σε αυτήν τη γραμμή εντολών.

Για να εκτελούν στην πραγματικότητα μια εργασία, πρέπει να προστεθεί εργασίες σε μια εργασία. Κάθε [CloudTask] [ py_task] έχει ρυθμιστεί με μια ιδιότητα της γραμμής εντολών και [ResourceFiles] [ py_resource_file] (όπως με το χώρο συγκέντρωσης StartTask) που η εργασία λήψεις στον κόμβο πριν από τη γραμμή εντολών εκτελείται αυτόματα. Στο δείγμα, κάθε εργασία επεξεργάζεται μόνο ένα αρχείο. Έτσι, η συλλογή ResourceFiles περιέχει ένα στοιχείο.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] Όταν πρόσβαση μεταβλητές περιβάλλοντος όπως `$AZ_BATCH_NODE_SHARED_DIR` ή η εκτέλεση μιας εφαρμογής που δεν βρίσκονται σε στον κόμβο `PATH`, γραμμές εντολή εργασίας πρέπει να καλέσετε το κέλυφος ρητά, όπως με `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Αυτή η απαίτηση δεν είναι απαραίτητη εάν τις εργασίες σας εκτέλεση μιας εφαρμογής από τον κόμβο `PATH` και να μην γίνεται αναφορά μεταβλητές περιβάλλοντος.

Εντός του `for` βρόχο στο τμήμα κώδικα παραπάνω, μπορείτε να δείτε ότι έχει συνταχθεί τη γραμμή εντολών για την εργασία με πέντε ορίσματα γραμμής εντολής που περνούν *python_tutorial_task.py*:

1. **FilePath**: Αυτή είναι η τοπική διαδρομή προς το αρχείο που υπάρχει στον κόμβο. Όταν το ResourceFile αντικείμενο στο `upload_file_to_container` που δημιουργήθηκε στο βήμα 2 παραπάνω, το όνομα του αρχείου που χρησιμοποιήθηκε για αυτήν την ιδιότητα (το `file_path` παραμέτρου στην κατασκευή ResourceFile). Αυτό υποδεικνύει ότι το αρχείο μπορεί να βρεθεί στον ίδιο κατάλογο στον κόμβο ως *python_tutorial_task.py*.

2. **NUMWORDS**: οι καλύτερες λέξεις *N* πρέπει να γράφονται με το αρχείο εξόδου.

3. **storageaccount**: το όνομα του λογαριασμού χώρου αποθήκευσης που κατέχει το κοντέινερ στην οποία θα πρέπει να αποσταλεί το αποτέλεσμα της εργασίας.

4. **storagecontainer**: το όνομα του κοντέινερ χώρου αποθήκευσης στην οποία θα πρέπει να αποσταλεί τα αρχεία εξόδου.

5. **sastoken**: Η υπογραφή κοινόχρηστη πρόσβαση (συσχετισμών Ασφαλείας) που παρέχει πρόσβαση εγγραφής στο κοντέινερ **εξόδου** στο χώρο αποθήκευσης Azure. Η δέσμη ενεργειών *python_tutorial_task.py* χρησιμοποιεί αυτήν την υπογραφή κοινόχρηστη πρόσβαση όταν δημιουργεί την αναφορά BlockBlobService. Αυτό παρέχει πρόσβαση εγγραφής στο κοντέινερ χωρίς να απαιτείται ένα πλήκτρο πρόσβασης για το λογαριασμό χώρου αποθήκευσης.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Βήμα 6: Παρακολούθηση εργασιών

![Εργασίες παρακολούθησης][6]<br/>
*Η δέσμη ενεργειών (1) παρακολουθεί τις εργασίες για την κατάσταση ολοκλήρωσης και (2) οι εργασίες αποστολή δεδομένων αποτέλεσμα στο χώρο αποθήκευσης Azure*

Όταν εργασίες προστίθενται σε μια εργασία, είναι αυτόματα στην ουρά και έχει προγραμματιστεί για εκτέλεση σε κόμβους υπολογιστικών εντός του χώρου συγκέντρωσης που σχετίζονται με την εργασία. Με βάση τις ρυθμίσεις που καθορίζετε, δέσμη χειρίζεται όλα ουράς εργασιών, τον προγραμματισμό, επανάληψη και άλλα καθήκοντα διαχείρισης εργασιών για εσάς.

Υπάρχουν πολλά προσεγγίσεις για την παρακολούθηση εργασιών εκτέλεσης. Το `wait_for_tasks_to_complete` συνάρτηση σε *python_tutorial_client.py* παρέχει ένα απλό παράδειγμα της παρακολούθησης εργασίες για ένα συγκεκριμένο μέλος, σε αυτήν την περίπτωση, η [Ολοκλήρωση] [ py_taskstate] κατάσταση.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Βήμα 7: Λήψη εξόδου εργασίας

![Λήψη εξόδου εργασίας από το χώρο αποθήκευσης][7]<br/>

Τώρα που η εργασία έχει ολοκληρωθεί, το αποτέλεσμα από τις εργασίες μπορούν να ληφθούν από χώρο αποθήκευσης Azure. Αυτό γίνεται με μια κλήση σε `download_blobs_from_container` στο *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] Η κλήση `download_blobs_from_container` σε *python_tutorial_client.py* Καθορίζει ότι θα πρέπει να ληφθούν τα αρχεία στο κεντρικό σας κατάλογο. Είστε ευπρόσδεκτοι να τροποποιήσετε αυτήν τη θέση εξόδου.

## <a name="step-8-delete-containers"></a>Βήμα 8: Διαγραφή κοντέινερ

Επειδή που θα χρεωθείτε για τα δεδομένα που βρίσκονται στο χώρο αποθήκευσης Azure, είναι πάντα καλή ιδέα να καταργήσετε οποιαδήποτε αντικείμενα BLOB που δεν χρειάζονται πλέον για τις μαζικές εργασίες. Στο *python_tutorial_client.py*, αυτό γίνεται με τρεις κλήσεων στο [BlockBlobService.delete_container][py_delete_container]:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Βήμα 9: Διαγραφή της εργασίας και το χώρο συγκέντρωσης

Στο τελευταίο βήμα, θα σας ζητηθεί να διαγράψετε την εργασία και το χώρο συγκέντρωσης που έχουν δημιουργηθεί με τη δέσμη ενεργειών *python_tutorial_client.py* . Παρόλο που δεν θα χρεωθείτε για εργασίες και τις εργασίες τους, *είναι* χρεωθείτε για κόμβους υπολογιστικών. Επομένως, συνιστάται να που θα εκχωρηθεί κόμβους μόνο σύμφωνα με τις ανάγκες. Διαγραφή που δεν χρησιμοποιούνται χώρους συγκέντρωσης μπορεί να είναι μέρος της διαδικασίας συντήρησης.

Το BatchServiceClient [JobOperations] [ py_job] και [PoolOperations] [ py_pool] και τα δύο έχουν αντίστοιχες μέθοδοι διαγραφής, που ονομάζονται εάν μπορείτε να επιβεβαιώσετε τη διαγραφή:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] Έχετε υπόψη ότι που θα χρεωθείτε για τους πόρους υπολογισμού--διαγραφή σύνολα που δεν χρησιμοποιείται θα ελαχιστοποιήσει κόστος. Επίσης, πρέπει να γνωρίζετε ότι η διαγραφή ενός χώρου συγκέντρωσης διαγράφει όλους τους κόμβους υπολογισμού μέσα σε αυτόν το χώρο συγκέντρωσης και ότι όλα τα δεδομένα τους κόμβους θα μπορούν να ανακτηθούν μετά από τη διαγραφή του χώρου συγκέντρωσης.

## <a name="run-the-sample-script"></a>Εκτελέστε το δείγμα δέσμης ενεργειών

Κατά την εκτέλεση της δέσμης ενεργειών *python_tutorial_client.py* από το πρόγραμμα εκμάθησης [δείγμα κώδικα][github_article_samples], το αποτέλεσμα κονσόλας είναι παρόμοια με τα εξής. Υπάρχει μια παύση στο `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` ενώ το χώρο συγκέντρωσης κόμβους υπολογιστικών δημιουργούνται, αποτελέσματα, και εκτελούνται οι εντολές στο χώρο συγκέντρωσης Έναρξη εργασίας. Χρήση του [Azure πύλη] [ azure_portal] για την παρακολούθηση του χώρου συγκέντρωσης, τον υπολογισμό κόμβους, έργων και εργασιών κατά τη διάρκεια και μετά την εκτέλεση. Χρήση του [Azure πύλη] [ azure_portal] ή το [Microsoft Azure αποθήκευσης Explorer] [ storage_explorer] για να προβάλετε τους πόρους αποθήκευσης (κοντέινερ και αντικείμενα BLOB) που δημιουργούνται από την εφαρμογή.

>[AZURE.TIP] Εκτελέστε τη δέσμη ενεργειών *python_tutorial_client.py* μέσα από το `azure-batch-samples/Python/Batch/article_samples` καταλόγου. Χρησιμοποιεί μια σχετική διαδρομή για το `common.helpers` Εισαγωγή λειτουργικής μονάδας, ώστε να μπορεί να δείτε `ImportError: No module named 'common'` Εάν δεν μπορείτε να εκτελέσετε το της δέσμης ενεργειών από μέσα σε αυτόν τον κατάλογο.

Τυπικές χρόνος εκτέλεσης είναι **περίπου 5-7 λεπτά** όταν εκτελείτε το δείγμα με την προεπιλεγμένη ρύθμιση παραμέτρων.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>Επόμενα βήματα

Είστε ευπρόσδεκτοι να κάνετε αλλαγές σε *python_tutorial_client.py* και *python_tutorial_task.py* για να πειραματιστείτε με διαφορετικές υπολογισμού σενάρια. Για παράδειγμα, δοκιμάστε να προσθέσετε μια καθυστέρηση εκτέλεσης να *python_tutorial_task.py* να προσομοιώσετε μεγάλη διάρκεια εκτέλεσης εργασίες και να παρακολουθήσετε τους στην πύλη. Δοκιμάστε να προσθέσετε περισσότερες εργασίες ή να προσαρμόσετε τον αριθμό των κόμβους υπολογιστικών. Προσθήκη λογικής ελέγχου και να επιτρέψετε τη χρήση ενός υπάρχοντος χώρου συγκέντρωσης χρόνο εκτέλεσης ταχύτητας.

Τώρα που είστε εξοικειωμένοι με τη βασική ροή εργασίας μιας λύσης δέσμης, είναι ώρα για να ψάξετε στις πρόσθετες δυνατότητες της υπηρεσίας δέσμης.

- Διαβάστε το άρθρο [Επισκόπηση του Azure δέσμη δυνατότητες](batch-api-basics.md) , το οποίο συνιστάται εάν είστε εξοικειωμένοι με την υπηρεσία.
- Έναρξη στην τα άλλα άρθρα ανάπτυξης δέσμη στην περιοχή **ανάπτυξης αναλυτικά** κατά τη [διαδικασία εκμάθησης δέσμη][batch_learning_path].
- Δείτε μια διαφορετική υλοποίηση της επεξεργασίας το φόρτο εργασίας "επάνω Ν λέξεις" με τη μαζική στο το [TopNWords] [ github_topnwords] δείγμα.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Δημιουργία κοντέινερ στο χώρο αποθήκευσης Azure"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Αποστολή εφαρμογή εργασιών και αρχείων εισαγωγής (δεδομένα) για κοντέινερ"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Δημιουργία χώρου συγκέντρωσης δέσμης"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Δημιουργία μαζική εργασία"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Προσθήκη εργασιών στο έργο"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Εργασίες παρακολούθησης"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Λήψη εξόδου εργασίας από το χώρο αποθήκευσης"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Μαζική λύση ροής εργασίας (πλήρες διάγραμμα)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Μαζική διαπιστευτήρια στην πύλη"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Χώρος αποθήκευσης διαπιστευτηρίων στην πύλη"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Μαζική λύση ροής εργασίας (ελάχιστους διάγραμμα)"
