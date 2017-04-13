<properties
   pageTitle="Χρήση δεδομένων λίμνης Store Java SDK για να αναπτύξετε εφαρμογές | Microsoft Azure"
   description="Χρήση του Azure δεδομένων λίμνης Store Java SDK για να αναπτύξετε εφαρμογές"
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
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-java"></a>Γρήγορα αποτελέσματα με χρήση Java χώρου αποθήκευσης λίμνης δεδομένων Azure

> [AZURE.SELECTOR]
- [Πύλη](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Μάθετε πώς μπορείτε να χρησιμοποιήσετε στο SDK Java Azure δεδομένων λίμνης χώρου αποθήκευσης για να εκτελέσετε βασικές λειτουργίες όπως όπως δημιουργία φακέλων, αποστολή και λήψη αρχείων δεδομένων, κ.λπ. Για περισσότερες πληροφορίες σχετικά με την λίμνης δεδομένων, ανατρέξτε στο θέμα [Χώρου αποθήκευσης λίμνης Azure δεδομένων](data-lake-store-overview.md).

Μπορείτε να αποκτήσετε πρόσβαση σε έγγραφα Java SDK API για Azure χώρου αποθήκευσης λίμνης δεδομένων σε [έγγραφα του Azure δεδομένων λίμνης Store Java API](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Java Development Kit (JDK 7 ή νεότερη έκδοση, χρησιμοποιώντας Java έκδοση 1,7 ή νεότερη έκδοση)
* Λογαριασμός Azure δεδομένων λίμνης Store. Ακολουθήστε τις οδηγίες στο θέμα [Γρήγορα αποτελέσματα με με την πύλη Azure χώρου αποθήκευσης λίμνης δεδομένων Azure](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί Maven για εξαρτήσεις Δόμηση και το project. Αν και είναι δυνατή η δημιουργία χωρίς τη χρήση ενός συστήματος Δόμηση όπως Maven ή Gradle, αυτά τα συστήματα δημιουργίας είναι πολύ πιο εύκολο να διαχειριστείτε εξαρτήσεις.
* (Προαιρετικό) Και IDE όπως [IntelliJ ΙΔΈΑ](https://www.jetbrains.com/idea/download/) ή [Έκλειψη](https://www.eclipse.org/downloads/) ή παρόμοια.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Πώς να τον έλεγχο ταυτότητας με χρήση Azure Active Directory;

Σε αυτό το πρόγραμμα εκμάθησης χρησιμοποιούμε ένα μυστικό Azure AD εφαρμογή προγράμματος-πελάτη για την ανάκτηση ενός διακριτικού Azure Active Directory (έλεγχος ταυτότητας υπηρεσίας εξυπηρέτησης). Χρησιμοποιούμε αυτό το διακριτικό για να δημιουργήσετε το αντικείμενο χώρου αποθήκευσης λίμνης δεδομένων προγράμματος-πελάτη για την εκτέλεση λειτουργιών λειτουργίες αρχείων και καταλόγων. Για οδηγίες σχετικά με τον τρόπο για τον έλεγχο ταυτότητας με Azure Store λίμνης δεδομένων χρησιμοποιώντας το μυστικό προγράμματος-πελάτη, θα σας να εκτελέσετε τα παρακάτω βήματα υψηλού επιπέδου:

1. Δημιουργία μιας εφαρμογής web Azure AD
2. Ανακτήστε το Αναγνωριστικό υπολογιστή-πελάτη, μυστικό προγράμματος-πελάτη και διακριτικού τελικό σημείο για την εφαρμογή web Azure AD.
3. Ρύθμιση παραμέτρων πρόσβασης για την εφαρμογή web Azure AD στο χώρο αποθήκευσης δεδομένων λίμνης/φάκελο αρχείων που θέλετε να έχετε πρόσβαση από την εφαρμογή Java που δημιουργείτε.

Για οδηγίες σχετικά με τον τρόπο για να εκτελέσετε αυτά τα βήματα, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής υπηρεσίας καταλόγου Active Directory](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

Azure Active Directory παρέχει άλλες επιλογές, καθώς και για την ανάκτηση ενός διακριτικού. Μπορείτε να επιλέξετε από διάφορες μηχανισμούς διαφορετικό ελέγχου ταυτότητας να ταιριάζει με τη δική σας περίπτωση, για παράδειγμα, μια εφαρμογή που εκτελείται στο πρόγραμμα περιήγησης, μια εφαρμογή διανέμεται ως μια εφαρμογή υπολογιστή ή μια εφαρμογή διακομιστή εκτελείται εσωτερικής εγκατάστασης ή σε μια εικονική μηχανή Azure. Μπορείτε επίσης να επιλέξετε από διαφορετικούς τύπους διαπιστευτήρια όπως κωδικούς πρόσβασης, πιστοποιητικά, έλεγχος ταυτότητας παραγόντων 2, κ.λπ. Επιπλέον, Azure Active Directory σάς επιτρέπει να συγχρονίσετε τους χρήστες σας υπηρεσία καταλόγου Active Directory εσωτερικής εγκατάστασης με το cloud. Για λεπτομέρειες, ανατρέξτε στο θέμα [Σενάρια ελέγχου ταυτότητας για την υπηρεσία καταλόγου Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Δημιουργία μιας εφαρμογής Java

Ο κωδικός δείγματος διαθέσιμη [σε GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) σας καθοδηγεί σας καθοδηγεί στη διαδικασία δημιουργίας αρχεία στο χώρο αποθήκευσης, συνένωση αρχείων, τη λήψη ενός αρχείου και διαγραφή ορισμένα αρχεία στο χώρο αποθήκευσης. Αυτή η ενότητα αυτού του άρθρου καθοδηγήσουμε την κύρια μέρη του κώδικα.

1. Δημιουργήστε ένα έργο Maven χρησιμοποιώντας [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) από τη γραμμή εντολών ή IDE. Για οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα έργο Java χρησιμοποιώντας IntelliJ, δείτε [εδώ](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Για οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα έργο χρησιμοποιώντας το Έκλειψη, δείτε [εδώ](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 

2. Προσθέστε τις ακόλουθες εξαρτήσεις με το αρχείο **pom.xml** Maven. Προσθέστε το παρακάτω τμήμα κώδικα κειμένου μεταξύ του ** \</version >** ετικέτα και το ** \</του project >** ετικέτα:

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.0.4-SNAPSHOT</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    Η πρώτη εξάρτηση είναι να χρησιμοποιήσετε το SDK Store λίμνης δεδομένων (`azure-datalake-store`) από το χώρο αποθήκευσης maven. Η δεύτερη εξάρτηση (`slf4j-nop`) είναι για να καθορίσετε ποια framework καταγραφής για να χρησιμοποιήσετε για αυτήν την εφαρμογή. Το SDK Store λίμνης δεδομένων χρησιμοποιεί πρόσοψη [slf4j](http://www.slf4j.org/) καταγραφής, η οποία σας επιτρέπει να επιλέξετε από διάφορες πλαίσια δημοφιλείς καταγραφής, όπως log4j, Java καταγραφή, logback, κ.λπ., ή χωρίς σύνδεση. Για αυτό το παράδειγμα, θα απενεργοποιήσουμε καταγραφή, ως εκ τούτου, χρησιμοποιούμε τη σύνδεση **slf4j nop** . Για να χρησιμοποιήσετε άλλες επιλογές καταγραφή από την εφαρμογή σας, ανατρέξτε στο θέμα [εδώ](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>Προσθήκη του κώδικα εφαρμογής

Υπάρχουν τρία κύρια τμήματα στον κώδικα.

1. Αποκτήστε το διακριτικό Azure Active Directory

2. Χρησιμοποιήστε το διακριτικό για να δημιουργήσετε ένα πρόγραμμα-πελάτη του χώρου αποθήκευσης δεδομένων λίμνης.

3. Χρησιμοποιήστε το πρόγραμμα-πελάτη του χώρου αποθήκευσης λίμνης δεδομένων για την εκτέλεση λειτουργιών.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Βήμα 1: Λήψη ενός διακριτικού Azure Active Directory.

Το SDK Store λίμνης δεδομένων παρέχει εύκολη μεθόδους που σας επιτρέπουν να αποκτήσετε τα διακριτικά ασφαλείας που απαιτείται για να μιλήσετε μαζί με το λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων. Ωστόσο, το SDK δεν υποχρεώνει ώστε να χρησιμοποιηθεί μόνο τις παρακάτω μεθόδους. Μπορείτε να χρησιμοποιήσετε οποιοδήποτε άλλο μέσο παραγωγής διακριτικό καθώς και, όπως με τη χρήση του [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java)ή το δικό σας προσαρμοσμένο κώδικα.

Για να χρησιμοποιήσετε το SDK δεδομένων λίμνης Store για να αποκτήσετε το διακριτικό για την εφαρμογή Active Directory Web που δημιουργήσατε νωρίτερα, χρησιμοποιήστε τις μεθόδους στατική στο `AzureADAuthenticator` τάξης. Αντικαταστήστε **ΣΥΜΠΛΉΡΩΣΗΣ-ΕΔΏ** με τις πραγματικές τιμές για την εφαρμογή Azure Active Directory Web.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Βήμα 2: Δημιουργία ενός αντικειμένου χώρου αποθήκευσης λίμνης Azure δεδομένων προγράμματος-πελάτη (ADLStoreClient)

Δημιουργία ενός αντικειμένου [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) απαιτεί να καθορίσετε το όνομα λογαριασμού του χώρου αποθήκευσης δεδομένων λίμνης και το διακριτικό Azure Active Directory που δημιουργήσατε στο τελευταίο βήμα. Σημειώστε ότι το όνομα λογαριασμού του χώρου αποθήκευσης δεδομένων λίμνης πρέπει να είναι ένα πλήρως προσδιορισμένο όνομα τομέα. Για παράδειγμα, αντικαταστήστε **ΣΥΜΠΛΉΡΩΣΗΣ-ΕΔΏ** με κάπως **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Βήμα 3: Χρησιμοποιήστε το ADLStoreClient για να εκτελέσετε λειτουργίες αρχείων και καταλόγων

Ο παρακάτω κώδικας περιέχει παράδειγμα τμήματα κώδικα από ορισμένες κοινές λειτουργίες. Μπορείτε να ανατρέξετε σε πλήρη [έγγραφα δεδομένων λίμνης Store Java SDK API](https://azure.github.io/azure-data-lake-store-java/javadoc/) του αντικειμένου **ADLStoreClient** για να δείτε τις άλλες λειτουργίες.
 
Σημειώστε ότι τα αρχεία είναι ανάγνωση από και γραμμένο σε χρήση ροών τυπική Java. Αυτό σημαίνει ότι μπορείτε να δημιουργήσετε επίπεδα οποιαδήποτε από τις ροές Java επάνω μέρος των ροών χώρου αποθήκευσης λίμνης δεδομένων για να επωφεληθείτε από τυπική Java λειτουργίες (π.χ., ροές εκτύπωσης για μορφοποιημένη Έξοδος, ή σε οποιαδήποτε από τις ροές συμπίεσης ή κρυπτογράφησης για πρόσθετη λειτουργικότητα στην αρχή, κ.λπ.).

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Βήμα 4: Δημιουργία και εκτέλεση της εφαρμογής

1. Για να εκτελέσετε από μέσα σε IDE, εντοπίστε και πατήστε το κουμπί **Εκτέλεση** . Για να εκτελέσετε από Maven, χρησιμοποιήστε [εκτέλεσης: εκτέλεση](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).

2. Για να δημιουργήσετε μια μεμονωμένη βάζο που μπορείτε να εκτελέσετε από γραμμής εντολών δημιουργία το βάζο με όλες τις εξαρτήσεις περιλαμβάνονται, με χρήση της [προσθήκης συγκρότησης Maven](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). Το pom.xml στο [παράδειγμα πηγαίου κώδικα στην github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) έχει ένα παράδειγμα του τρόπου να το κάνετε αυτό.


## <a name="next-steps"></a>Επόμενα βήματα

- [Διασφάλιση των δεδομένων στο χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-secure-data.md)
- [Χρήση ανάλυσης λίμνης Azure δεδομένων με το χώρο αποθήκευσης δεδομένων λίμνης](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Χρήση Azure HDInsight με το χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-hdinsight-hadoop-use-portal.md)
