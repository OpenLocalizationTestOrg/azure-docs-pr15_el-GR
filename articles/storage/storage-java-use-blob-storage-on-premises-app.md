<properties
    pageTitle="Εσωτερικής εγκατάστασης εφαρμογών με το χώρο αποθήκευσης αντικειμένων blob (Java) | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια εφαρμογή κονσόλας που στέλνει μια εικόνα Azure και, στη συνέχεια, η εικόνα εμφανίζεται στο πρόγραμμα περιήγησης. Δείγματα κώδικα σε Java."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="on-premises-application-with-blob-storage"></a>Εφαρμογή εσωτερικής εγκατάστασης με το χώρο αποθήκευσης αντικειμένων blob

## <a name="overview"></a>Επισκόπηση

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure χώρου αποθήκευσης για την αποθήκευση εικόνων στο Azure. Ο κωδικός σε αυτό το άρθρο είναι για μια εφαρμογή κονσόλας που στέλνει μια εικόνα Azure και, στη συνέχεια, δημιουργεί ένα αρχείο HTML που εμφανίζει την εικόνα στο πρόγραμμα περιήγησης.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- Μια Java προγραμματιστής Kit (JDK), έκδοση 1,6 ή νεότερη έκδοση, είναι εγκατεστημένη.
- Είναι εγκατεστημένο το SDK Azure.
- Το ΒΆΖΟ για τις βιβλιοθήκες Azure για Java και οποιαδήποτε πλατύστομα ισχύει εξάρτηση, εγκαθίστανται και βρίσκονται στη διαδρομή Δόμηση χρησιμοποιούνται από το πρόγραμμα μεταγλώττισης Java. Για πληροφορίες σχετικά με την εγκατάσταση των βιβλιοθηκών Azure για Java, ανατρέξτε στο θέμα [λήψη του Azure SDK για Java](java-download-azure-sdk.md).
- Ρύθμιση λογαριασμού Azure χώρου αποθήκευσης. Το όνομα λογαριασμού και το κλειδί λογαριασμού για το λογαριασμό χώρου αποθήκευσης θα χρησιμοποιηθεί από τον κωδικό σε αυτό το άρθρο. Δείτε [πώς μπορείτε να Δημιουργία λογαριασμού χώρου αποθήκευσης](storage-create-storage-account.md#create-a-storage-account) για πληροφορίες σχετικά με τη δημιουργία ενός λογαριασμού χώρου αποθήκευσης και [Προβολή και αντιγραφή πλήκτρα πρόσβασης χώρου αποθήκευσης](storage-create-storage-account.md#view-and-copy-storage-access-keys) για πληροφορίες σχετικά με την ανάκτηση το κλειδί λογαριασμού.

- Έχετε δημιουργήσει ένα αρχείο τοπική εικόνα με το όνομα είναι αποθηκευμένα στη γραμμή εντολών διαδρομή c:\\myimages\\image1.jpg. Εναλλακτικά, μπορείτε να τροποποιήσετε την κατασκευή   **FileInputStream** στο παράδειγμα για να χρησιμοποιήσετε μια διαφορετική εικόνα διαδρομή και το όνομα αρχείου.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Για να χρησιμοποιήσετε το χώρο αποθήκευσης αντικειμένων blob του Azure για την αποστολή ενός αρχείου

Μια διαδικασία βήμα προς βήμα παρουσιάζεται εδώ. Εάν θέλετε να μεταβείτε στο μέλλον, ολόκληρο τον κωδικό παρουσιάζεται παρακάτω σε αυτό το άρθρο.

Ξεκινήστε τον κώδικα, συμπεριλαμβανομένων των εισαγωγών για τις κατηγορίες αποθήκευσης Azure πυρήνα, οι κλάσεις αντικειμένων blob του Azure προγράμματος-πελάτη, οι κλάσεις Java εισόδου/ΕΞΌΔΟΥ και την κλάση **URISyntaxException** .

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

Δήλωση μιας κλάσης με το όνομα **StorageSample**και περιλαμβάνει την ανοιχτή αγκύλη, **{**.

    public class StorageSample {

Στα πλαίσια του **StorageSample** κατηγορίας, δηλώνουν σε μεταβλητή συμβολοσειράς που θα περιέχει το προεπιλεγμένο τελικό σημείο πρωτόκολλο, το όνομα του λογαριασμού χώρου αποθήκευσης και κλειδιού πρόσβασης χώρου αποθήκευσης, όπως καθορίζονται στο λογαριασμό σας στο Azure χώρου αποθήκευσης. Αντικαταστήστε τις τιμές του πλαισίου κράτησης θέσης **σας\_λογαριασμού\_όνομα** και **σας\_λογαριασμού\_κλειδί** με το δικό σας λογαριασμό κλειδί όνομα και το λογαριασμό, αντίστοιχα.

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

Προσθήκη στη δήλωση σας για το **κύριο**, συμπεριλάβετε ένα μπλοκ **δοκιμάστε** και συμπεριλάβετε τις απαραίτητες Άνοιγμα αγκύλες, **{**.

    public static void main(String[] args)
    {
        try
        {

Δήλωση μεταβλητών του παρακάτω τύπου (είναι τις περιγραφές για τον τρόπο που χρησιμοποιούνται σε αυτό το παράδειγμα):

-   **CloudStorageAccount**: χρησιμοποιούνται για την προετοιμασία του αντικειμένου λογαριασμού με το όνομα του λογαριασμού Azure χώρου αποθήκευσης και αριθμού-κλειδιού, καθώς και για να δημιουργήσετε το αντικείμενο blob προγράμματος-πελάτη.
-   **CloudBlobClient**: χρησιμοποιείται για την πρόσβαση στην υπηρεσία blob.
-   **CloudBlobContainer**: χρησιμοποιείται για να δημιουργήσετε ένα κοντέινερ αντικειμένων blob, λίστα τα αντικείμενα blob στο κοντέινερ και τη διαγραφή του κοντέινερ.
-   **CloudBlockBlob**: χρησιμοποιείται για την αποστολή ενός αρχείου τοπικής εικόνας στο κοντέινερ.

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

Αντιστοίχιση της τιμής στη μεταβλητή **λογαριασμού** .

    account = CloudStorageAccount.parse(storageConnectionString);

Αντιστοίχιση της τιμής στη μεταβλητή **serviceClient** .

    serviceClient = account.createCloudBlobClient();

Αντιστοίχιση της τιμής στη μεταβλητή **κοντέινερ** . Θα σας θα λάβετε μια αναφορά σε ένα κοντέινερ που ονομάζεται **gettingstarted**.

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

Δημιουργία του κοντέινερ. Αυτή η μέθοδος θα δημιουργήσει το κοντέινερ, εάν δεν υπάρχει (και επιστροφή **true**). Εάν υπάρχει το κοντέινερ, θα επιστρέψει **τιμή false**. Εναλλακτική λύση για τη **createIfNotExists** είναι η μέθοδος **Δημιουργία** (το οποίο θα επιστρέψουν σφάλμα εάν υπάρχει ήδη στο κοντέινερ).

    container.createIfNotExists();

Ρύθμιση ανώνυμης πρόσβασης για το κοντέινερ.

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

Λάβετε μια αναφορά για το μπλοκ blob, που θα αντιπροσωπεύει το αντικείμενο blob στο χώρο αποθήκευσης Azure.

    blob = container.getBlockBlobReference("image1.jpg");

Χρησιμοποιήστε την κατασκευή **αρχείου** για να λάβετε μια αναφορά για το αρχείο που δημιουργήσατε τοπικά που θα μπορείτε να αποστείλετε. Βεβαιωθείτε ότι έχετε δημιουργήσει αυτού του αρχείου πριν από την εκτέλεση του κώδικα.

    File fileReference = new File ("c:\\myimages\\image1.jpg");

Αποστείλετε το τοπικό αρχείο μέσω κλήσης για τη μέθοδο **CloudBlockBlob.upload** . Η πρώτη παράμετρος στη μέθοδο **CloudBlockBlob.upload** είναι ένα αντικείμενο **FileInputStream** που αντιπροσωπεύει το τοπικό αρχείο που θα αποσταλεί Azure χώρου αποθήκευσης. Η δεύτερη παράμετρος είναι το μέγεθος, σε byte, του αρχείου.

    blob.upload(new FileInputStream(fileReference), fileReference.length());

Κλήση μιας συνάρτησης Βοήθειας με το όνομα **MakeHTMLPage**, για να κάνετε μια βασική σελίδα HTML που περιέχει ένα ** &lt;εικόνα&gt; ** στοιχείο με την προέλευση οριστεί για το αντικείμενο blob που βρίσκεται τώρα στο λογαριασμό σας Azure χώρου αποθήκευσης. Τον κωδικό για **MakeHTMLPage** εξετάζονται αργότερα σε αυτό το άρθρο.

    MakeHTMLPage(container);

Εκτυπώστε ένα μήνυμα κατάστασης και πληροφορίες σχετικά με τη σελίδα HTML που έχουν δημιουργηθεί.

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

Κλείστε το μπλοκ **δοκιμάστε** , εισάγοντας μια αγκύλη κλεισίματος: **}**

Χειρίζεται τις εξής εξαιρέσεις:

-   **FileNotFoundException**: μπορεί να είναι δημιουργήθηκε από το **FileInputStream** ή **FileOutputStream** κατασκευές.
-   **StorageException**: μπορεί να είναι δημιουργήθηκε από τη βιβλιοθήκη του χώρου αποθήκευσης Azure προγράμματος-πελάτη.
-   **URISyntaxException**: μπορεί να είναι δημιουργήθηκε με τη μέθοδο **ListBlobItem.getUri** .
-   **Εξαίρεση**: χειρισμού εξαιρέσεων γενικής χρήσης.

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Κλείστε το **κύριο** , εισάγοντας μια αγκύλη κλεισίματος: **}**

Δημιουργήστε μια μέθοδο με το όνομα **MakeHTMLPage** για τη δημιουργία μιας βασικής σελίδας HTML. Αυτή η μέθοδος έχει μια παράμετρο του τύπου **CloudBlobContainer**, που θα χρησιμοποιηθεί για να επαναλάβετε τη λίστα των αντικειμένων blob αποσταλεί. Αυτή η μέθοδος θα εμφανίσουν εξαιρέσεις του τύπου **FileNotFoundException**, που μπορεί να είναι δημιουργήθηκε από την κατασκευή **FileOutputStream** , και **URISyntaxException**, που μπορεί να είναι δημιουργήθηκε με τη μέθοδο **ListBlobItem.getUri** . Συμπεριλάβετε την αριστερή παρένθεση, **{**.

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

Δημιουργήστε ένα τοπικό αρχείο που ονομάζεται **index.html**.

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

Εγγραφή στο τοπικό αρχείο, προσθήκη στην το ** &lt;html&gt;**, ** &lt;κεφαλίδα&gt;**, και ** &lt;σώμα&gt; ** στοιχεία.

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

Διαδοχικές προσεγγίσεις μέσα στη λίστα των αποσταλεί αντικείμενα blob. Για κάθε blob, στη σελίδα HTML Δημιουργήστε μια ** &lt;img&gt; ** στοιχείο που έχει το χαρακτηριστικό **src** αποστέλλονται το URI της το αντικείμενο blob να παραμείνουν στο λογαριασμό σας στο Azure αποθήκευσης ως.
Παρόλο που έχετε προσθέσει μόνο μία εικόνα σε αυτό το δείγμα, αν έχετε προσθέσει περισσότερες, αυτός ο κώδικας θα διαδοχικές προσεγγίσεις όλες τις.

Για λόγους ευκολίας, αυτό το παράδειγμα προϋποθέτει κάθε αποσταλεί blob είναι μια εικόνα. Εάν έχετε ενημερώσει αντικείμενα BLOB εκτός από εικόνες ή αντικείμενα BLOB σελίδα αντί για το μπλοκ αντικείμενα blob, ρυθμίστε τον κώδικα, όπως απαιτείται.

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

Κλείσιμο του ** &lt;σώμα&gt; ** στοιχείο και τη ** &lt;html&gt; ** στοιχείο.

    stream.println("</body>");
    stream.println("</html>");

Κλείστε το τοπικό αρχείο.

    stream.close();

Κλείσιμο **MakeHTMLPage** , εισάγοντας μια αγκύλη κλεισίματος: **}**

Κλείσιμο **StorageSample** , εισάγοντας μια αγκύλη κλεισίματος: **}**

Ακολουθεί τον πλήρη κώδικα για αυτό το παράδειγμα. Μην ξεχάσετε να τροποποιήσετε τις τιμές του πλαισίου κράτησης θέσης **σας\_λογαριασμού\_όνομα** και **σας\_λογαριασμού\_κλειδί** για να χρησιμοποιήσετε το όνομα του λογαριασμού και το κλειδί λογαριασμού, αντίστοιχα.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Εκτός από την αποστολή αρχείου τοπικής εικόνας με το χώρο αποθήκευσης Azure, ο κώδικας του παραδείγματος δημιουργεί ένα τοπικό αρχείο namedindex.html, το οποίο μπορείτε να ανοίξετε στο πρόγραμμα περιήγησης για να δείτε την εικόνα σας έχουν αποσταλεί.

Επειδή ο κωδικός περιέχει το όνομα λογαριασμού και το κλειδί λογαριασμού, βεβαιωθείτε ότι το πηγαίος κώδικας είναι ασφαλής.

## <a name="to-delete-a-container"></a>Για να διαγράψετε ένα κοντέινερ

Επειδή που θα χρεωθείτε για το χώρο αποθήκευσης, μπορεί να θέλετε να διαγράψετε το κοντέινερ **gettingstarted** αφού ολοκληρώσετε δοκιμές με αυτό το παράδειγμα. Για να διαγράψετε ένα κοντέινερ, χρησιμοποιήστε τη μέθοδο **CloudBlobContainer.delete** .

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

Για να καλέσετε τη μέθοδο **CloudBlobContainer.delete** , η διαδικασία για την προετοιμασία **CloudStorageAccount**, **ClodBlobClient**και **CloudBlobContainer** αντικείμενα είναι ίδια όπως φαίνεται για τη μέθοδο **createIfNotExist** . Ακολουθεί ένα παράδειγμα ολοκλήρωσης που διαγράφει το κοντέινερ με όνομα **gettingstarted**.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

Για μια επισκόπηση των άλλων κλάσεις αποθήκευσης αντικειμένων blob και οι μέθοδοι, ανατρέξτε στο θέμα [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από Java](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Επόμενα βήματα

Ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα σχετικά με τις πιο σύνθετες εργασίες χώρου αποθήκευσης.

- [Azure αποθήκευσης SDK για Java](https://github.com/azure/azure-storage-java)
- [Αναφορά SDK Azure αποθήκευσης προγράμματος-πελάτη](http://dl.windowsazure.com/storage/javadoc/)
- [Υπηρεσίες Azure αποθήκευσης REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης](http://blogs.msdn.com/b/windowsazurestorage/)
