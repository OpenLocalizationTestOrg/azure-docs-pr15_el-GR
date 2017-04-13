<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αρχείων από Java | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία Azure αρχείου για αποστολή, λήψη, λίστα, και διαγραφή αρχείων. Δείγματα γραμμένο σε Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-file-storage-from-java"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αρχείων από Java

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Επισκόπηση

Σε αυτόν τον οδηγό θα μάθετε πώς να εκτελείτε βασικές λειτουργίες στην υπηρεσία Microsoft Azure αρχείου χώρου αποθήκευσης. Μέσω δειγμάτων γραμμένο σε Java θα μάθετε πώς μπορείτε να δημιουργήσετε κοινόχρηστα στοιχεία και σε καταλόγους, αποστολή, λίστας και διαγραφή αρχείων. Εάν είστε εξοικειωμένοι με την υπηρεσία αποθήκευσης αρχείων στο Microsoft Azure, διέρχονται από τις έννοιες στις ενότητες που ακολουθούν θα είναι πολύ χρήσιμο στην κατανόηση των δειγμάτων.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Δημιουργία μιας εφαρμογής Java

Για να δημιουργήσετε τα δείγματα, θα χρειαστείτε τα Java Development Kit (JDK) και το [] [Azure SDK χώρου αποθήκευσης για Java]. Θα πρέπει επίσης έχετε δημιουργήσει ένα λογαριασμό Azure χώρου αποθήκευσης.

## <a name="setup-your-application-to-use-file-storage"></a>Ρύθμιση εφαρμογή σας να χρησιμοποιεί χώρο αποθήκευσης αρχείων

Για να χρησιμοποιήσετε το χώρο αποθήκευσης Azure APIs, προσθέστε την ακόλουθη πρόταση στο επάνω μέρος του αρχείου Java όπου σκοπεύετε να αποκτήσετε πρόσβαση στην υπηρεσία αποθήκευσης από.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.file.*;

## <a name="setup-an-azure-storage-connection-string"></a>Ρύθμιση μιας συμβολοσειράς σύνδεσης Azure χώρου αποθήκευσης

Για να χρησιμοποιήσετε το χώρο αποθήκευσης αρχείων, πρέπει να συνδεθείτε στο λογαριασμό σας Azure χώρου αποθήκευσης. Το πρώτο βήμα θα ήταν να ρυθμίσετε τις παραμέτρους μια συμβολοσειρά σύνδεσης, η οποία θα χρησιμοποιήσουμε για να συνδεθείτε στο λογαριασμό σας χώρου αποθήκευσης. Ας Καθορίστε μια στατική μεταβλητή για να το κάνετε.

    // Configure the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account_name;" +
        "AccountKey=your_storage_account_key";

> [AZURE.NOTE] Αντικατάσταση your_storage_account_name και your_storage_account_key με τις πραγματικές τιμές για το λογαριασμό χώρου αποθήκευσης.

## <a name="connecting-to-an-azure-storage-account"></a>Σύνδεση με ένα λογαριασμό Azure χώρου αποθήκευσης

Για να συνδεθείτε στο λογαριασμό σας χώρου αποθήκευσης, πρέπει να χρησιμοποιήσετε το αντικείμενο **CloudStorageAccount** , που περνά μέσα μια συμβολοσειρά σύνδεσης με τη μέθοδο **ανάλυση** .

    // Use the CloudStorageAccount object to connect to your storage account
    try {
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
    } catch (InvalidKeyException invalidKey) {
        // Handle the exception
    }

**CloudStorageAccount.parse** παρουσιάζει ένα InvalidKeyException, θα χρειαστεί να την τοποθετήσετε μέσα σε ένα μπλοκ δοκιμή/προϊόντων.

## <a name="how-to-create-a-share"></a>Πώς μπορείτε να: Δημιουργήστε ένα κοινόχρηστο στοιχείο

Όλα τα αρχεία και καταλόγους στο χώρο αποθήκευσης αρχείων που βρίσκονται σε ένα κοντέινερ που ονομάζεται ένα **κοινή χρήση**. Το λογαριασμό χώρου αποθήκευσης μπορούν να έχουν όσο μερίδια ως επιτρέπει τη χωρητικότητα λογαριασμού. Για να αποκτήσετε πρόσβαση σε ένα κοινόχρηστο στοιχείο και τα περιεχόμενά της, πρέπει να χρησιμοποιήσετε ένα πρόγραμμα-πελάτη χώρο αποθήκευσης αρχείων.

    // Create the file storage client.
    CloudFileClient fileClient = storageAccount.createCloudFileClient();

Χρησιμοποιώντας το πρόγραμμα-πελάτη χώρου αποθήκευσης αρχείων, μπορείτε να αποκτήσετε, στη συνέχεια, μια αναφορά σε ένα κοινόχρηστο στοιχείο.

    // Get a reference to the file share
    CloudFileShare share = fileClient.getShareReference("sampleshare");

Για να δημιουργήσετε στην πραγματικότητα κοινή χρήση, χρησιμοποιήστε τη μέθοδο **createIfNotExists** του αντικειμένου CloudFileShare.

    if (share.createIfNotExists()) {
        System.out.println("New share created");
    }

Σε αυτό το σημείο, η **κοινή χρήση** διατηρεί μια αναφορά σε ένα κοινόχρηστο στοιχείο με το όνομα **sampleshare**.

## <a name="how-to-upload-a-file"></a>Πώς μπορείτε να: Αποστολή αρχείου

Ένα κοινόχρηστο χώρο αποθήκευσης Azure αρχείο περιέχει τουλάχιστον, έναν ριζικό κατάλογο όπου μπορεί να βρίσκονται τα αρχεία. Σε αυτήν την ενότητα, θα μάθετε πώς μπορείτε να αποστείλετε ένα αρχείο από τον τοπικό χώρο αποθήκευσης στον ριζικό κατάλογο της ένα κοινόχρηστο στοιχείο.

Είναι το πρώτο βήμα για την αποστολή ενός αρχείου για να αποκτήσετε μια αναφορά στον κατάλογο όπου θα πρέπει να βρίσκονται. Μπορείτε να το κάνετε αυτό, καλώντας τη μέθοδο **getRootDirectoryReference** του αντικειμένου κοινή χρήση.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

Τώρα που έχετε μια αναφορά στον ριζικό κατάλογο του κοινόχρηστου στοιχείου, μπορείτε να αποστείλετε ένα αρχείο σε αυτήν χρησιμοποιώντας τον παρακάτω κώδικα.

    // Define the path to a local file.
    final String filePath = "C:\\temp\\Readme.txt";

    CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
    cloudFile.uploadFromFile(filePath);

## <a name="how-to-create-a-directory"></a>Πώς μπορείτε να: δημιουργία καταλόγου

Μπορείτε επίσης να οργανώσετε χώρου αποθήκευσης με την τοποθέτηση αρχείων μέσα δευτερευόντων καταλόγων αντί όλες τις στον ριζικό κατάλογο. Η υπηρεσία αποθήκευσης αρχείων Azure σας επιτρέπει να δημιουργήσετε όσο σε καταλόγους, καθώς επιτρέπει το λογαριασμό σας. Ο παρακάτω κώδικας θα δημιουργήσει μια δευτερεύουσα κατάλογο με όνομα **sampledir** κάτω από το ριζικό κατάλογο.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the sampledir directory
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    if (sampleDir.createIfNotExists()) {
        System.out.println("sampledir created");
    } else {
        System.out.println("sampledir already exists");
    }

## <a name="how-to-list-files-and-directories-in-a-share"></a>Πώς μπορείτε να: λίστα αρχείων και καταλόγων σε ένα κοινόχρηστο στοιχείο

Απόκτηση μια λίστα αρχείων και καταλόγων μέσα σε ένα κοινόχρηστο στοιχείο γίνεται εύκολα με κλήση **listFilesAndDirectories** σε μια αναφορά CloudFileDirectory. Η μέθοδος επιστρέφει μια λίστα με ListFileItem αντικείμενα τα οποία μπορείτε να επαναλάβετε σε. Ως παράδειγμα, ο ακόλουθος κώδικας θα εμφανιστούν τα αρχεία και καταλόγους μέσα στον ριζικό κατάλογο.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
        System.out.println(fileItem.getUri());
    }


## <a name="how-to-download-a-file"></a>Πώς μπορείτε να: λήψη ενός αρχείου

Μία από τις πιο συχνές λειτουργίες θα εκτελέσει σε σχέση με το χώρο αποθήκευσης αρχείων είναι να κάνετε λήψη αρχείων. Στο παρακάτω παράδειγμα, ο κώδικας λήψεις SampleFile.txt και εμφανίζει τα περιεχόμενά του.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the directory that contains the file
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    //Get a reference to the file you want to download
    CloudFile file = sampleDir.getFileReference("SampleFile.txt");

    //Write the contents of the file to the console.
    System.out.println(file.downloadText());

## <a name="how-to-delete-a-file"></a>Πώς μπορείτε να: διαγραφή αρχείου

Μια άλλη κοινές λειτουργία αποθήκευσης αρχείων είναι διαγραφή του αρχείου. Ο ακόλουθος κώδικας διαγράφει ένα αρχείο με το όνομα SampleFile.txt που είναι αποθηκευμένα μέσα σε έναν κατάλογο με όνομα **sampledir**.


    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory where the file to be deleted is in
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    String filename = "SampleFile.txt"
    CloudFile file;

    file = containerDir.getFileReference(filename)
    if ( file.deleteIfExists() ) {
        System.out.println(filename + " was deleted");
    }


## <a name="how-to-delete-a-directory"></a>Πώς μπορείτε να: διαγράψετε έναν κατάλογο

Η διαγραφή ενός καταλόγου είναι αρκετά απλό εργασίας, παρόλο που θα πρέπει να σημειωθεί ότι δεν μπορείτε να διαγράψετε έναν κατάλογο που εξακολουθεί να περιέχει αρχεία ή άλλους καταλόγους.

    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory you want to delete
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    // Delete the directory
    if ( containerDir.deleteIfExists() ) {
        System.out.println("Directory deleted");
    }


## <a name="how-to-delete-a-share"></a>Πώς μπορείτε να: διαγράψετε ένα κοινόχρηστο στοιχείο

Διαγραφή ενός κοινόχρηστου στοιχείου γίνεται καλώντας τη μέθοδο **deleteIfExists** σε ένα αντικείμενο CloudFileShare. Ακολουθεί δείγμα κώδικα που κάνει αυτό.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the file client.
       CloudFileClient fileClient = storageAccount.createCloudFileClient();

       // Get a reference to the file share
       CloudFileShare share = fileClient.getShareReference("sampleshare");

       if (share.deleteIfExists()) {
           System.out.println("sampleshare deleted");
       }
    } catch (Exception e) {
        e.printStackTrace();
    }

## <a name="next-steps"></a>Επόμενα βήματα

Εάν θέλετε να μάθετε περισσότερα σχετικά με άλλες APIs Azure χώρου αποθήκευσης, ακολουθήστε αυτές τις συνδέσεις.

- [Κέντρο για προγραμματιστές Java](http://azure.microsoft.com/develop/java/)
- [Azure αποθήκευσης SDK για Java](https://github.com/azure/azure-storage-java)
- [Azure αποθήκευσης SDK για Android](https://github.com/azure/azure-storage-android)
- [Αναφορά SDK Azure αποθήκευσης προγράμματος-πελάτη](http://dl.windowsazure.com/storage/javadoc/)
- [Υπηρεσίες Azure αποθήκευσης REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης](http://blogs.msdn.com/b/windowsazurestorage/)
- [Μεταφορά δεδομένων με το βοηθητικό πρόγραμμα γραμμής εντολών AzCopy](storage-use-azcopy.md)
