<properties 
   pageTitle="Γρήγορα αποτελέσματα με το χώρο αποθήκευσης λίμνης δεδομένων με χρήση REST API | Microsoft Azure" 
   description="Χρήση WebHDFS REST API για να εκτελέσετε λειτουργίες στο χώρο αποθήκευσης δεδομένων λίμνης" 
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

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Γρήγορα αποτελέσματα με χρήση APIs ΥΠΌΛΟΙΠΑ χώρου αποθήκευσης λίμνης δεδομένων Azure

> [AZURE.SELECTOR]
- [Πύλη](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε WebHDFS REST API και APIs ΥΠΌΛΟΙΠΑ Store λίμνης δεδομένων για να εκτελέσετε Διαχείριση λογαριασμού, καθώς και οι λειτουργίες του συστήματος αρχείων στο χώρο αποθήκευσης λίμνης Azure δεδομένων. Χώρος αποθήκευσης λίμνης δεδομένων Azure εκθέτει το δικό της REST API για λειτουργίες διαχείρισης λογαριασμού. Ωστόσο, επειδή το χώρο αποθήκευσης λίμνης δεδομένων είναι συμβατά με το περιβάλλον εμπορικής προσαρμογής HDFS και Hadoop, υποστηρίζει χρησιμοποιώντας WebHDFS ΥΠΌΛΟΙΠΟ APIs για λειτουργίες του συστήματος αρχείων.

>[AZURE.NOTE] Για λεπτομερείς πληροφορίες σχετικά με το REST API υποστήριξης για το χώρο αποθήκευσης λίμνης δεδομένων, ανατρέξτε στο θέμα [Azure δεδομένων λίμνης Store REST API αναφοράς](https://msdn.microsoft.com/library/mt693424.aspx).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

- **Δημιουργία μιας εφαρμογής υπηρεσίας καταλόγου Azure Active Directory**. Μπορείτε να χρησιμοποιήσετε την εφαρμογή Azure AD για τον έλεγχο ταυτότητας της εφαρμογής αποθήκευσης λίμνης δεδομένων με το Azure AD. Υπάρχουν διαφορετικές μεθόδους για τον έλεγχο ταυτότητας με Azure AD, που είναι **τελικού χρήστη ελέγχου ταυτότητας** ή **τον έλεγχο ταυτότητας υπηρεσίας εξυπηρέτησης**. Για οδηγίες και περισσότερες πληροφορίες σχετικά με τον τρόπο για τον έλεγχο ταυτότητας, ανατρέξτε στο θέμα [Έλεγχος ταυτότητας με το χώρο αποθήκευσης λίμνης δεδομένων χρησιμοποιώντας Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). Σε αυτό το άρθρο χρησιμοποιεί καμπύλη ώστε να δείχνουν πώς μπορείτε να κάνετε κλήσεις REST API σε σχέση με ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Πώς να τον έλεγχο ταυτότητας με χρήση Azure Active Directory;

Μπορείτε να χρησιμοποιήσετε δύο προσεγγίσεις για τον έλεγχο ταυτότητας με χρήση Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Έλεγχος ταυτότητας του τελικού χρήστη (αλληλεπιδραστικά)

Σε αυτό το σενάριο, η εφαρμογή ζητά από το χρήστη για να συνδεθείτε και όλες οι λειτουργίες που εκτελούνται στο περιβάλλον του χρήστη. Ακολουθήστε τα παρακάτω βήματα για αλληλεπιδραστικών έλεγχο ταυτότητας.

1. Από την εφαρμογή, ανακατευθύνετε το χρήστη στην ακόλουθη διεύθυνση URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT-URI > πρέπει να είναι κωδικοποιημένη για χρήση σε μια διεύθυνση URL. Επομένως, για https://localhost, χρησιμοποιήστε `https%3A%2F%2Flocalhost`)

    Για αυτό το πρόγραμμα εκμάθησης, μπορείτε να αντικαταστήστε τις τιμές κράτησης θέσης στη διεύθυνση URL παραπάνω και να επικολλήσετε στη γραμμή διευθύνσεων του προγράμματος περιήγησης. Θα ανακατευθυνθείτε για τον έλεγχο ταυτότητας με χρήση του Azure login. Μόλις συνδεθείτε με επιτυχία στον, η απόκριση εμφανίζεται στη γραμμή διευθύνσεων του προγράμματος περιήγησης. Η απόκριση θα είναι με την εξής μορφή:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Καταγράψτε τον κωδικό εξουσιοδότησης από την απάντηση. Για αυτό το πρόγραμμα εκμάθησης, μπορείτε να αντιγράψετε τον κώδικα εξουσιοδότησης από τη γραμμή διευθύνσεων του προγράμματος περιήγησης web και να μεταβιβάζουν το στην πρόσκληση σε ΚΑΤΑΧΏΡΗΣΗ για το τελικό σημείο διακριτικού, όπως φαίνεται παρακάτω:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] Σε αυτήν την περίπτωση, η \<REDIRECT-URI > δεν χρειάζεται να κωδικοποιηθεί.

3. Η απάντηση είναι ένα αντικείμενο JSON που περιέχει ένα διακριτικό πρόσβασης (π.χ., `"access_token": "<ACCESS_TOKEN>"`) και ενός διακριτικού ανανέωσης (π.χ., `"refresh_token": "<REFRESH_TOKEN>"`). Η εφαρμογή σας χρησιμοποιεί το διακριτικό πρόσβασης κατά την πρόσβαση στο χώρο αποθήκευσης λίμνης δεδομένων Azure και το διακριτικό Ανανέωση για να λάβετε έναν νέο κωδικό πρόσβασης όταν λήγει ένα διακριτικό πρόσβασης.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Όταν λήξει το διακριτικό πρόσβασης, μπορείτε να ζητήσετε έναν νέο κωδικό πρόσβασης, χρησιμοποιώντας το διακριτικό ανανέωσης, όπως φαίνεται παρακάτω:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Για περισσότερες πληροφορίες σχετικά με τον έλεγχο ταυτότητας χρήστη διαδραστικό, ανατρέξτε στο θέμα [Εκχώρηση ροής κωδικό εξουσιοδότησης](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Υπηρεσία εξυπηρέτησης ελέγχου ταυτότητας (μη αλληλεπιδραστική)

Σε αυτό το σενάριο, το δικό του διαπιστευτήρια για να εκτελέσετε τις λειτουργίες παρέχει την εφαρμογή. Για αυτό, πρέπει να εκδώσετε μια αίτηση POST όπως αυτό που φαίνεται παρακάτω. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Το αποτέλεσμα αυτής της αίτησης θα περιλαμβάνει ένα διακριτικό εξουσιοδότησης (δηλώνεται από `access-token` στο αποτέλεσμα του παρακάτω) που θα ανοίξει περάσετε με τις κλήσεις σας REST API. Αποθήκευση αυτό το διακριτικό ελέγχου ταυτότητας σε ένα αρχείο κειμένου. θα χρειαστείτε αργότερα σε αυτό το άρθρο.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Αυτό το άρθρο χρησιμοποιεί τη **μη αλληλεπιδραστική** προσέγγιση. Για περισσότερες πληροφορίες σχετικά με την μη αλληλεπιδραστική (υπηρεσία εξυπηρέτησης κλήσεις), ανατρέξτε στο θέμα [της υπηρεσίας εξυπηρέτησης κλήσεις χρησιμοποιώντας τα διαπιστευτήρια](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης δεδομένων λίμνης

Αυτή η λειτουργία είναι με βάση το REST API κλήσης που ορίζονται από [εδώ](https://msdn.microsoft.com/library/mt694078.aspx).

Χρησιμοποιήστε την ακόλουθη εντολή καμπύλη. Αντικατάσταση ** \<yourstorename >** με το όνομα του χώρου αποθήκευσης λίμνης δεδομένων σας.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Στην παραπάνω εντολή, αντικαταστήστε \< `REDACTED` \> με το διακριτικό εξουσιοδότησης που ανακτήσατε νωρίτερα. Το φορτίο αίτηση για αυτή την εντολή περιέχονται στο αρχείο **input.json** που παρέχεται για το `-d` παραμέτρου παραπάνω. Τα περιεχόμενα του αρχείου input.json μοιάζουν με τα εξής:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>Δημιουργία φακέλων σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης

Αυτή η λειτουργία είναι με βάση το REST API WebHDFS κλήσης που ορίζονται από [εδώ](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Χρησιμοποιήστε την ακόλουθη εντολή καμπύλη. Αντικατάσταση ** \<yourstorename >** με το όνομα του χώρου αποθήκευσης λίμνης δεδομένων σας.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

Στην παραπάνω εντολή, αντικαταστήστε \< `REDACTED` \> με το διακριτικό εξουσιοδότησης που ανακτήσατε νωρίτερα. Αυτή η εντολή δημιουργεί έναν κατάλογο που ονομάζεται **mytempdir** κάτω από το ριζικό φάκελο του λογαριασμού σας χώρου αποθήκευσης λίμνης δεδομένων.

Εάν η λειτουργία ολοκληρωθεί με επιτυχία, θα πρέπει να βλέπετε μια απάντηση ως εξής:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Λίστα φακέλων σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης

Αυτή η λειτουργία είναι με βάση το REST API WebHDFS κλήσης που ορίζονται από [εδώ](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Χρησιμοποιήστε την ακόλουθη εντολή καμπύλη. Αντικατάσταση ** \<yourstorename >** με το όνομα του χώρου αποθήκευσης λίμνης δεδομένων σας.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

Στην παραπάνω εντολή, αντικαταστήστε \< `REDACTED` \> με το διακριτικό εξουσιοδότησης που ανακτήσατε νωρίτερα.

Εάν η λειτουργία ολοκληρωθεί με επιτυχία, θα πρέπει να βλέπετε μια απάντηση ως εξής:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Αποστολή δεδομένων σε λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης

Αυτή η λειτουργία είναι με βάση το REST API WebHDFS κλήσης που ορίζονται από [εδώ](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Αποστολή δεδομένων χρησιμοποιώντας το REST API WebHDFS είναι μια διαδικασία δύο βημάτων, όπως περιγράφεται παρακάτω.

1. Υποβάλετε μια αίτηση HTTP ΤΟΠΟΘΈΤΗΣΗ χωρίς να το στείλετε τα δεδομένα του αρχείου να αποσταλεί. Στην παρακάτω εντολή, αντικαταστήστε ** \<yourstorename >** με το όνομα του χώρου αποθήκευσης λίμνης δεδομένων σας.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    Την έξοδο για αυτή την εντολή θα είναι περιέχει μια διεύθυνση URL redirect προσωρινά, όπως αυτή που εμφανίζεται παρακάτω.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. Τώρα πρέπει να υποβάλετε μια άλλη αίτηση HTTP ΤΟΠΟΘΕΤΉΣΕΤΕ σε σχέση με τη διεύθυνση URL που αναφέρονται για την ιδιότητα **θέση** στην απάντηση. Αντικατάσταση ** \<yourstorename >** με το όνομα του χώρου αποθήκευσης λίμνης δεδομένων σας.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    Το αποτέλεσμα θα είναι παρόμοιο με το εξής:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>Ανάγνωση δεδομένων από ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης

Αυτή η λειτουργία είναι με βάση το REST API WebHDFS κλήσης που ορίζονται από [εδώ](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Ανάγνωση δεδομένων από ένα χώρο αποθήκευσης δεδομένων λίμνης λογαριασμός είναι μια διαδικασία δύο βημάτων.

* Υποβολή πρώτα ένα αίτημα GET σε σχέση με το τελικό σημείο `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Αυτό θα επιστρέψει μια θέση για να υποβάλετε την επόμενη ΛΉΨΗ πρόσκληση σε σύσκεψη.
* Στη συνέχεια, υποβάλετε την αίτηση GET σε σχέση με το τελικό σημείο `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Αυτό θα εμφανίσει τα περιεχόμενα του αρχείου.

Ωστόσο, επειδή δεν υπάρχει διαφορά μεταξύ της πρώτης και το δεύτερο βήμα στις παραμέτρους εισόδου, μπορείτε να χρησιμοποιήσετε το `-L` παράμετρο, για να υποβάλετε αίτηση για την πρώτη. `-L`η επιλογή συνδυάζει δύο αιτήσεις σε μία ουσιαστικά και θα κάνει την καμπύλη επανάληψη της αίτησης στη νέα θέση. Τέλος, τα αποτελέσματα από όλες τις κλήσεις αίτηση εμφανίζονται, όπως φαίνεται παρακάτω. Αντικατάσταση ** \<yourstorename >** με το όνομα του χώρου αποθήκευσης λίμνης δεδομένων σας.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Θα πρέπει να δείτε το αποτέλεσμα παρόμοιο με το εξής:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Μετονομασία ενός αρχείου σε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης

Αυτή η λειτουργία είναι με βάση το REST API WebHDFS κλήσης που ορίζονται από [εδώ](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Χρησιμοποιήστε την ακόλουθη εντολή καμπύλη για να μετονομάσετε ένα αρχείο. Αντικατάσταση ** \<yourstorename >** με το όνομα του χώρου αποθήκευσης λίμνης δεδομένων σας.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Θα πρέπει να δείτε το αποτέλεσμα παρόμοιο με το εξής:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Διαγραφή ενός αρχείου από ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης

Αυτή η λειτουργία είναι με βάση το REST API WebHDFS κλήσης που ορίζονται από [εδώ](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Χρησιμοποιήστε την ακόλουθη εντολή καμπύλη για να διαγράψετε ένα αρχείο. Αντικατάσταση ** \<yourstorename >** με το όνομα του χώρου αποθήκευσης λίμνης δεδομένων σας.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Διαγραφή ενός λογαριασμού χώρου αποθήκευσης δεδομένων λίμνης

Αυτή η λειτουργία είναι με βάση το REST API κλήσης που ορίζονται από [εδώ](https://msdn.microsoft.com/library/mt694075.aspx).

Χρησιμοποιήστε την ακόλουθη εντολή καμπύλη για να διαγράψετε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης. Αντικατάσταση ** \<yourstorename >** με το όνομα του χώρου αποθήκευσης λίμνης δεδομένων σας.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Δείτε επίσης

- [Ανοίξετε τις εφαρμογές μεγάλο προέλευση δεδομένων συμβατή με το χώρο αποθήκευσης λίμνης δεδομένων Azure](data-lake-store-compatible-oss-other-applications.md)
 
