<properties
    pageTitle="Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob Azure μέσω AzCopy | Microsoft Azure"
    description="Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob Azure μέσω AzCopy"
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

# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Μετακίνηση δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob Azure μέσω AzCopy

AzCopy είναι ένα βοηθητικό πρόγραμμα γραμμής που έχει σχεδιαστεί για αποστολή, λήψη και αντιγραφή δεδομένων προς και από το Microsoft Azure blob αρχείου και χώρος αποθήκευσης πινάκων.

Για οδηγίες σχετικά με την εγκατάσταση του AzCopy και πρόσθετες πληροφορίες σχετικά με τη χρήση του με το Azure πλατφόρμα, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το βοηθητικό πρόγραμμα γραμμής εντολών AzCopy](../storage/storage-use-azcopy.md).

Οδηγίες για τις τεχνολογίες που χρησιμοποιούνται για τη μετακίνηση δεδομένων σε ή/και από το χώρο αποθήκευσης αντικειμένων Blob του Azure είναι συνδεδεμένες εδώ:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Εάν χρησιμοποιείτε Εικονική που έχει ρυθμιστεί με τις δέσμες ενεργειών που παρέχονται από [δεδομένα Science εικονικού μηχανές στο Azure](machine-learning-data-science-virtual-machines.md), στη συνέχεια, AzCopy είναι ήδη εγκατεστημένο στον η Εικονική.

> [AZURE.NOTE] Για μια πλήρη εισαγωγή με το χώρο αποθήκευσης αντικειμένων blob του Azure, ανατρέξτε για [Βασικά στοιχεία αντικειμένων Blob του Azure](../storage/storage-dotnet-how-to-use-blobs.md) και την [Υπηρεσία αντικειμένων Blob του Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το έγγραφο προϋποθέτει ότι έχετε μια συνδρομή του Azure, ένα λογαριασμό του χώρου αποθήκευσης και το αντίστοιχο κλειδί χώρου αποθήκευσης για τον συγκεκριμένο λογαριασμό. Πριν από την αποστολή/λήψη δεδομένων, πρέπει να γνωρίζετε Azure χώρου αποθήκευσης και το όνομα του λογαριασμού το κλειδί λογαριασμού σας.

- Για να ρυθμίσετε μια συνδρομή του Azure, ανατρέξτε στο θέμα [δωρεάν δοκιμαστική έκδοση ενός μήνα](https://azure.microsoft.com/pricing/free-trial/).

- Για οδηγίες σχετικά με τη δημιουργία ενός λογαριασμού χώρου αποθήκευσης και γρήγορα λογαριασμό και βασικές πληροφορίες, ανατρέξτε στο θέμα [σχετικά με το Azure λογαριασμοί χώρου αποθήκευσης](../storage/storage-create-storage-account.md).


## <a name="run-azcopy-commands"></a>Εκτέλεση εντολών AzCopy

Για να εκτελέσετε εντολές AzCopy, ανοίξτε ένα παράθυρο εντολών και μεταβείτε στον κατάλογο εγκατάστασης AzCopy στον υπολογιστή σας, όπου βρίσκεται το εκτελέσιμο AzCopy.exe. 

Η βασική σύνταξη για τις εντολές AzCopy είναι:

    AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] Μπορείτε να προσθέσετε τη θέση εγκατάστασης AzCopy διαδρομή του συστήματός σας και, στη συνέχεια, εκτελέστε τις εντολές από οποιονδήποτε κατάλογο. Από προεπιλογή, AzCopy είναι εγκατεστημένη *% ProgramFiles(x86) %\Microsoft SDKs\Azure\AzCopy* ή *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.

## <a name="upload-files-to-an-azure-blob"></a>Αποστολή αρχείων σε μια αντικειμένων blob του Azure

Για να αποστείλετε ένα αρχείο, χρησιμοποιήστε την ακόλουθη εντολή:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Λήψη αρχείων από μια αντικειμένων blob του Azure

Για να κάνετε λήψη ενός αρχείου από μια αντικειμένων blob του Azure, χρησιμοποιήστε την ακόλουθη εντολή:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Μεταφορά αντικειμένων blob μεταξύ Azure κοντέινερ

Για να μεταφέρετε αντικείμενα BLOB μεταξύ Azure κοντέινερ, χρησιμοποιήστε την ακόλουθη εντολή:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Συμβουλές για τη χρήση AzCopy

> [AZURE.TIP]   
> 1. Όταν **Αποστολή** αρχείων, */S* κάνει αποστολή των αρχείων σταδιακά. Χωρίς αυτήν την παράμετρο, αρχεία σε υποκατάλογοι δεν αποστέλλονται.  
> 2. Κατά **τη λήψη** αρχείου, */S* αναζητά το κοντέινερ σταδιακά μέχρι όλα τα αρχεία στον καθορισμένο κατάλογο και οι υποκατάλογοι ή όλα τα αρχεία που ταιριάζουν με το συγκεκριμένο μοτίβο στον δεδομένο κατάλογο και οι υποκατάλογοι, γίνεται λήψη.  
> 3.  Δεν μπορείτε να καθορίσετε ένα **αρχείο συγκεκριμένες blob** για να κάνετε λήψη χρησιμοποιώντας την παράμετρο */Source* . Για να κάνετε λήψη ενός συγκεκριμένου αρχείου, καθορίστε το όνομα του αρχείου blob για να κάνετε λήψη χρησιμοποιώντας την παράμετρο */Pattern* . **/S** παράμετρος μπορεί να χρησιμοποιηθεί για να έχετε AzCopy αναζητήστε ένα σταδιακά μοτίβο όνομα αρχείου. Χωρίς την παράμετρο μοτίβο, AzCopy λήψεις όλα τα αρχεία σε αυτόν τον κατάλογο.
