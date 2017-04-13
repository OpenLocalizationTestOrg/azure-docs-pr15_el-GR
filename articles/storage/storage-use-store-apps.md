<properties
    pageTitle="Χρήση Azure χώρου αποθήκευσης σε εφαρμογές του Windows Store | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια εφαρμογή του Windows Store η οποία χρησιμοποιεί χώρο αποθήκευσης αντικειμένων Blob του Azure, ουρά, πίνακας ή αρχείο."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>
    
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης Azure στις εφαρμογές του Windows Store

## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός δείχνει πώς μπορείτε να ξεκινήσετε μια εφαρμογή του Windows Store η οποία χρησιμοποιεί χώρο αποθήκευσης Azure ανάπτυξη.

## <a name="download-required-tools"></a>Λήψη απαιτούμενα εργαλεία

- [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) διευκολύνει τη δημιουργία, εντοπισμός σφαλμάτων, μετάφραση, πακέτου, και ανάπτυξη εφαρμογών του Windows Store. Απαιτείται το Visual Studio 2012 ή νεότερη έκδοση.
- Η [Βιβλιοθήκη προγράμματος-πελάτη αποθήκευσης Azure](https://www.nuget.org/packages/WindowsAzure.Storage) παρέχει μια βιβλιοθήκη κλάσης χρόνου εκτέλεσης των Windows για την εργασία με το χώρο αποθήκευσης Azure.
- [WCF δεδομένων υπηρεσιών εργαλεία για το Windows Store εφαρμογές](http://www.microsoft.com/download/details.aspx?id=30714) επεκτείνει την εμπειρία Προσθήκη υπηρεσιών αναφοράς με την υποστήριξη OData πλευρά του προγράμματος-πελάτη για τις εφαρμογές του Windows Store στο Visual Studio.

## <a name="develop-apps"></a>Αναπτύξτε εφαρμογές

### <a name="getting-ready"></a>Ετοιμασία

Δημιουργήστε ένα νέο έργο εφαρμογή Windows Store στο Visual Studio 2012 ή νεότερη έκδοση:

![χώρος αποθήκευσης εφαρμογές-χώρου αποθήκευσης-σύγκριση-στο έργο][store-apps-storage-vs-project]

Στη συνέχεια, προσθέστε μια αναφορά στη βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης, κάνοντας δεξί κλικ σε **αναφορές**, κάνοντας κλικ στην επιλογή **Προσθήκη αναφοράς**και, στη συνέχεια, την περιήγηση με το χώρο αποθήκευσης στο πρόγραμμα-πελάτη βιβλιοθήκης για Windows χρόνο εκτέλεσης που έχετε λάβει:

![χώρος αποθήκευσης--χώρος αποθήκευσης-επιλέξτε-βιβλιοθήκη εφαρμογών][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>Χρήση της βιβλιοθήκης με τις υπηρεσίες Blob και ουρά

Σε αυτό το σημείο, την εφαρμογή σας είναι έτοιμη για να καλέσετε τις υπηρεσίες αντικειμένων Blob του Azure και ουρά. Προσθέστε τις παρακάτω προτάσεις **χρησιμοποιώντας** έτσι ώστε να τους τύπους αποθήκευσης Azure μπορούν να χρησιμοποιηθούν απευθείας:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

Στη συνέχεια, προσθέστε ένα κουμπί στη σελίδα σας. Προσθέστε τον ακόλουθο κώδικα για το συμβάν **κάντε κλικ στην επιλογή** και να τροποποιήσετε τη μέθοδο πρόγραμμα χειρισμού συμβάντων, χρησιμοποιώντας τη [λέξη-κλειδί ασύγχρονης](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

Αυτός ο κωδικός προϋποθέτει ότι έχετε συμβολοσειρά δύο μεταβλητές, το *όνομα λογαριασμού* και *accountKey*. Αντιπροσωπεύουν το όνομα του λογαριασμού χώρου αποθήκευσης και τον αριθμό-κλειδί λογαριασμού που σχετίζεται με τον συγκεκριμένο λογαριασμό.

Δημιουργία και εκτέλεση της εφαρμογής. Κάνοντας κλικ στο κουμπί, ελέγξτε εάν υπάρχει ένα κοντέινερ που ονομάζεται *container1* στο λογαριασμό σας και, στη συνέχεια, δημιουργήστε το εάν δεν.

### <a name="using-the-library-with-the-table-service"></a>Χρήση της βιβλιοθήκης με την υπηρεσία του πίνακα

Τύποι που χρησιμοποιούνται για την επικοινωνία με την υπηρεσία πίνακα Azure εξαρτώνται από WCF Data Services για τη βιβλιοθήκη του Windows Store app. Στη συνέχεια, προσθέστε μια αναφορά για τις απαιτούμενες βιβλιοθήκες WCF χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου:

![χώρος αποθήκευσης-εφαρμογές-χώρος αποθήκευσης--Διαχείριση πακέτου][store-apps-storage-package-manager]

Χρησιμοποιήστε την ακόλουθη εντολή για διαχείριση πακέτου σημείο στη θέση στον υπολογιστή σας:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Αυτή η εντολή θα προσθέσει αυτόματα όλες τις απαιτούμενες αναφορές στο έργο σας. Εάν δεν θέλετε να χρησιμοποιήσετε την Κονσόλα διαχείρισης πακέτου, μπορείτε να προσθέσετε το φάκελο NuGet υπηρεσίες WCF δεδομένων στον τοπικό υπολογιστή σας στη λίστα των προελεύσεων πακέτου και να, στη συνέχεια, προσθέστε την αναφορά μέσω του περιβάλλοντος εργασίας Χρήστη, όπως περιγράφεται στο [Τη Διαχείριση NuGet πακέτων χρησιμοποιώντας το παράθυρο διαλόγου](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Όταν έχετε αναφέρεται το πακέτο NuGet υπηρεσίες WCF δεδομένων, να αλλάξετε τον κωδικό στο συμβάν **κάντε κλικ στην επιλογή** του κουμπιού σας:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

Αυτός ο κωδικός ελέγχει εάν υπάρχει στο λογαριασμό σας έναν πίνακα με το όνομα *πίνακας1* και δημιουργεί, στη συνέχεια, εάν δεν.

Μπορείτε επίσης να προσθέσετε μια αναφορά σε Microsoft.WindowsAzure.Storage.Table.dll, που είναι διαθέσιμη στο ίδιο πακέτο που έχετε λάβει. Αυτή η βιβλιοθήκη περιέχει πρόσθετες λειτουργίες, όπως σειριοποίησης βάσει αντανάκλαση και γενικής χρήσης ερωτημάτων. Σημειώστε ότι αυτή η βιβλιοθήκη δεν υποστηρίζει JavaScript.



[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
