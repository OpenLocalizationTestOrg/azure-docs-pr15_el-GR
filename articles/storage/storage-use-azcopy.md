<properties
    pageTitle="Αντιγραφή ή μετακίνηση δεδομένων με το χώρο αποθήκευσης με AzCopy | Microsoft Azure"
    description="Χρησιμοποιήστε το βοηθητικό πρόγραμμα AzCopy για τη μετακίνηση ή αντιγραφή δεδομένων προς ή από blob πίνακα και το περιεχόμενο του αρχείου. Αντιγραφή δεδομένων με το χώρο αποθήκευσης Azure από τοπικά αρχεία ή αντιγραφή δεδομένων μέσα σε ή μεταξύ λογαριασμών χώρου αποθήκευσης. Μετεγκατάσταση εύκολα τα δεδομένα σας με το χώρο αποθήκευσης Azure."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="micurd"/>

# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>Μεταφορά δεδομένων με το βοηθητικό πρόγραμμα γραμμής εντολών AzCopy

## <a name="overview"></a>Επισκόπηση

AzCopy είναι ένα βοηθητικό πρόγραμμα γραμμής εντολών Windows έχει σχεδιαστεί για την αντιγραφή δεδομένων προς και από το χώρο αποθήκευσης αντικειμένων Blob Microsoft Azure, αρχείο ή πίνακα μέσω του απλό εντολές με βέλτιστη απόδοση. Μπορείτε να αντιγράψετε δεδομένα από ένα αντικείμενο σε ένα άλλο μέσα στο λογαριασμό σας χώρο αποθήκευσης ή μεταξύ λογαριασμών χώρου αποθήκευσης.

> [AZURE.NOTE] Αυτός ο οδηγός προϋποθέτει ότι είστε ήδη εξοικειωμένοι με το [Χώρο αποθήκευσης Azure](https://azure.microsoft.com/services/storage/). Εάν όχι, ανάγνωσης στην τεκμηρίωση [Εισαγωγή στο χώρο αποθήκευσης Azure](storage-introduction.md) θα είναι χρήσιμο. Σημαντικότερο, θα πρέπει να [δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης](storage-create-storage-account.md#create-a-storage-account) για να ξεκινήσετε να χρησιμοποιείτε το AzCopy.

## <a name="download-and-install-azcopy"></a>Λήψη και εγκατάσταση του AzCopy

### <a name="windows"></a>Windows

Λήψη της [πιο πρόσφατης έκδοσης του AzCopy](http://aka.ms/downloadazcopy).

### <a name="maclinux"></a>Mac/Linux

Δεν είναι διαθέσιμη για Mac/Linux OSs AzCopy. Ωστόσο, Azure CLI είναι ένα κατάλληλο εναλλακτική λύση για την αντιγραφή δεδομένων προς και από το χώρο αποθήκευσης Azure. Διαβάστε [χρησιμοποιώντας το Azure CLI με το χώρο αποθήκευσης Azure](storage-azure-cli.md) για να μάθετε περισσότερα.

## <a name="writing-your-first-azcopy-command"></a>Την πρώτη εντολή AzCopy γραφής

Η βασική σύνταξη για τις εντολές AzCopy είναι:

    AzCopy /Source:<source> /Dest:<destination> [Options]

Ανοίξτε ένα παράθυρο εντολών και μεταβείτε στον κατάλογο εγκατάστασης AzCopy στον υπολογιστή σας - όπου το `AzCopy.exe` εκτελέσιμο βρίσκεται. Εάν θέλετε, μπορείτε να προσθέσετε τη θέση εγκατάστασης AzCopy να διαδρομή του συστήματός σας. Από προεπιλογή, εγκαθίσταται AzCopy `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` ή `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

Τα παρακάτω παραδείγματα δείχνουν μια ποικιλία σενάρια για την αντιγραφή δεδομένων για να και από αντικείμενα BLOB Microsoft Azure, τα αρχεία και τους πίνακες. Ανατρέξτε στην ενότητα [AzCopy παραμέτρους](#azcopy-parameters) για μια αναλυτική εξήγηση των τις παραμέτρους που χρησιμοποιούνται σε κάθε δείγμα.

## <a name="blob-download"></a>BLOB: λήψη

### <a name="download-single-blob"></a>Λήψη μόνο blob

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"

Σημειώστε ότι αν ο φάκελος `C:\myfolder` δεν υπάρχει, AzCopy θα δημιουργήσετε και να κάνετε λήψη `abc.txt ` στον νέο φάκελο.

### <a name="download-single-blob-from-secondary-region"></a>Κάντε λήψη μόνο blob από δευτερεύουσα περιοχή

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Σημειώστε ότι πρέπει να έχετε δικαιώματα ανάγνωσης παν πλεονάζοντα αποθήκευσης με δυνατότητα.

### <a name="download-all-blobs"></a>Λήψη όλων των BLOB

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Ας υποθέσουμε ότι τα ακόλουθα αντικείμενα BLOB που βρίσκονται σε καθορισμένο κοντέινερ:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Μετά τη λειτουργία λήψης, τον κατάλογο `C:\myfolder` θα περιλαμβάνει τα ακόλουθα αρχεία:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Εάν δεν καθορίσετε την επιλογή `/S`, θα γίνει λήψη χωρίς αντικείμενα blob.

### <a name="download-blobs-with-specified-prefix"></a>Λήψη αντικείμενα blob με καθορισμένο πρόθεμα

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Ας υποθέσουμε ότι τα ακόλουθα αντικείμενα BLOB που βρίσκονται σε καθορισμένο κοντέινερ. Όλων των BLOB αρχίζουν με το πρόθεμα `a` θα γίνει λήψη:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Μετά τη λειτουργία λήψης, στο φάκελο `C:\myfolder` θα περιλαμβάνει τα ακόλουθα αρχεία:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Το πρόθεμα ισχύει για το εικονικό κατάλογο, που αποτελεί το πρώτο μέρος του ονόματος του blob. Στο παράδειγμα που φαίνεται παραπάνω, στον εικονικό κατάλογο δεν ταιριάζουν με το καθορισμένο πρόθεμα, ώστε να μην ληφθεί. Επιπλέον, εάν η επιλογή `\S` δεν έχει καθοριστεί, AzCopy δεν θα γίνει λήψη οποιαδήποτε αντικείμενα blob.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Ορισμός της ώρας τελευταίας τροποποίησης της αρχεία που έχουν εξαχθεί να είναι ίδια με την προέλευση αντικείμενα BLOB

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

Μπορείτε επίσης να εξαιρέσετε αντικείμενα BLOB από τη λειτουργία λήψης με βάση την ώρα τελευταίας τροποποίησης. Για παράδειγμα, εάν θέλετε να εξαιρέσετε αντικείμενα blob του οποίου τελευταίας τροποποίησης ώρας είναι η ίδια ή νεότερη έκδοση από το αρχείο προορισμού, προσθέστε το `/XN` επιλογή:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

Ή εάν θέλετε να εξαιρέσετε αντικείμενα blob του οποίου τελευταίας τροποποίησης ώρας είναι το ίδιο ή παλιότερα από το αρχείο προορισμού, προσθέστε το `/XO` επιλογή:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

## <a name="blob-upload"></a>BLOB: Αποστολή

### <a name="upload-single-file"></a>Αποστολή ενός αρχείου

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"

Εάν δεν υπάρχει το κοντέινερ καθορισμένο προορισμού, AzCopy θα δημιουργήσετε και να αποστείλετε το αρχείο σε αυτήν.

### <a name="upload-single-file-to-virtual-directory"></a>Αποστολή ενός αρχείου εικονικού καταλόγου

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Εάν δεν υπάρχει το καθορισμένο εικονικού καταλόγου, AzCopy θα αποστείλετε το αρχείο για να συμπεριλάβετε στον εικονικό κατάλογο στο όνομά της (*π.χ.*, `vd/abc.txt` στο παραπάνω παράδειγμα).

### <a name="upload-all-files"></a>Αποστολή όλων των αρχείων

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

Καθορισμός επιλογής `/S` στέλνει σταδιακά χώρο αποθήκευσης αντικειμένων Blob, γεγονός που σημαίνει ότι όλοι οι υποφάκελοι και τα αρχεία θα αποσταλούν καθώς και τα περιεχόμενα του καθορισμένου καταλόγου. Για παράδειγμα, ας υποθέσουμε ότι τα ακόλουθα αρχεία που βρίσκονται σε φάκελο `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Μετά τη λειτουργία αποστολής, το κοντέινερ θα περιλαμβάνει τα ακόλουθα αρχεία:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Εάν δεν καθορίσετε την επιλογή `/S`, AzCopy δεν θα αποσταλούν σταδιακά. Μετά τη λειτουργία αποστολής, το κοντέινερ θα περιλαμβάνει τα ακόλουθα αρχεία:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Αποστολή αρχείων που συμφωνούν με συγκεκριμένο μοτίβο

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Ας υποθέσουμε ότι τα ακόλουθα αρχεία που βρίσκονται σε φάκελο `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Μετά τη λειτουργία αποστολής, το κοντέινερ θα περιλαμβάνει τα ακόλουθα αρχεία:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Εάν δεν καθορίσετε την επιλογή `/S`, AzCopy θα αποσταλούν μόνο αντικείμενα BLOB που δεν βρίσκονται σε έναν εικονικό κατάλογο:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Καθορίστε τον τύπο περιεχομένου MIME από ένα blob προορισμού

Από προεπιλογή, AzCopy ορίζει τον τύπο περιεχομένου από ένα blob προορισμού για να `application/octet-stream`. Ξεκινώντας με έκδοση 3.1.0, να καθορίσετε τον τύπο περιεχομένου μέσω της επιλογής `/SetContentType:[content-type]`. Αυτήν τη σύνταξη ορίζει τον τύπο περιεχομένου για όλων των BLOB σε μια λειτουργία αποστολής.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Εάν καθορίσετε `/SetContentType` χωρίς τιμή, στη συνέχεια, AzCopy θα ορίσετε κάθε blob ή τύπο περιεχομένου του αρχείου σύμφωνα με την επέκταση αρχείου.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## <a name="blob-copy"></a>BLOB: αντιγραφή

### <a name="copy-single-blob-within-storage-account"></a>Αντιγραφή μόνο blob μέσα σε λογαριασμό χώρου αποθήκευσης

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt

Όταν αντιγράφετε ένα blob μέσα σε ένα λογαριασμό του χώρου αποθήκευσης, εκτελείται μια λειτουργία [αντίγραφο στο διακομιστή](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) .

### <a name="copy-single-blob-across-storage-accounts"></a>Αντιγραφή αντικειμένων blob μόνο σε λογαριασμούς χώρου αποθήκευσης

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Όταν αντιγράφετε ένα blob σε λογαριασμούς χώρου αποθήκευσης, εκτελείται μια λειτουργία [αντίγραφο στο διακομιστή](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) .

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Αντιγραφή μόνο blob από δευτερεύουσα περιοχή κύρια περιοχή

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Σημειώστε ότι πρέπει να έχετε δικαιώματα ανάγνωσης παν πλεονάζοντα αποθήκευσης με δυνατότητα.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Αντιγραφή μόνο blob και τα στιγμιότυπα σε λογαριασμούς χώρου αποθήκευσης

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Μετά τη λειτουργία αντιγραφής, το κοντέινερ προορισμού θα περιλαμβάνει το αντικείμενο blob και τα στιγμιότυπα. Υποθέτοντας ότι το αντικείμενο blob στο παραπάνω παράδειγμα έχει δύο στιγμιότυπα, το κοντέινερ θα περιλαμβάνει το παρακάτω blob και τα στιγμιότυπα:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Σύγχρονη αντιγραφή αντικείμενα BLOB σε λογαριασμούς χώρου αποθήκευσης

AzCopy από προεπιλογή αντιγράφει ασύγχρονα δεδομένων μεταξύ των δύο τελικά σημεία χώρου αποθήκευσης. Γι ' αυτό, η λειτουργία αντιγραφής θα εκτελείται παρασκήνιο χρησιμοποιώντας χωρητικότητα πλεονάζουσα εύρους ζώνης που έχει χωρίς SLA όσον αφορά τον τρόπο γρήγορα ένα blob θα αντιγραφούν και AzCopy περιοδικά θα ελέγξετε την κατάσταση αντίγραφο μέχρι την αντιγραφή ολοκληρωθεί ή απέτυχε.

Το `/SyncCopy` επιλογή εξασφαλίζει ότι θα σας βοηθήσουν συνεπή ταχύτητας λειτουργίας αντιγραφής. AzCopy εκτελεί το αντίγραφο σύγχρονη κάνοντας λήψη τα αντικείμενα BLOB για να αντιγράψετε από το καθορισμένο αρχείο προέλευσης τοπικής μνήμης και, στη συνέχεια, την αποστολή τους στον προορισμό χώρο αποθήκευσης αντικειμένων Blob.

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy`μπορεί να δημιουργήσει επιπλέον εξόδου κόστος σε σύγκριση με ασύγχρονης αντίγραφο, συνιστάται η προσέγγιση είναι να χρησιμοποιήσετε αυτήν την επιλογή σε μια Εικονική Azure που βρίσκεται στην ίδια περιοχή ως του λογαριασμού χώρου αποθήκευσης προέλευσης για να αποφύγετε κόστος εξόδου.

## <a name="file-download"></a>Αρχείο: λήψη

### <a name="download-single-file"></a>Κάντε λήψη ενός αρχείου

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Εάν το συγκεκριμένο αρχείο προέλευσης είναι ένα στοιχείο Azure κοινής χρήσης και, στη συνέχεια, πρέπει να καθορίσετε το ακριβές όνομα αρχείου, είτε (*π.χ.* `abc.txt`) για να κάνετε λήψη ενός μόνο αρχείου ή να καθορίσετε την επιλογή `/S` για να κάνετε λήψη όλων των αρχείων σε σταδιακά την κοινή χρήση. Για να καθορίσετε ένα μοτίβο αρχείου και την επιλογή `/S` μαζί θα έχει ως αποτέλεσμα σφάλμα.

### <a name="download-all-files"></a>Λήψη όλων των αρχείων

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S

Σημειώστε ότι δεν θα γίνεται λήψη των κενών φακέλων.

## <a name="file-upload"></a>Αρχείο: Αποστολή

### <a name="upload-single-file"></a>Αποστολή ενός αρχείου

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt

### <a name="upload-all-files"></a>Αποστολή όλων των αρχείων

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Σημειώστε ότι δεν θα γίνεται αποστολή των κενών φακέλων.

### <a name="upload-files-matching-specified-pattern"></a>Αποστολή αρχείων που συμφωνούν με συγκεκριμένο μοτίβο

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

## <a name="file-copy"></a>Αρχείο: αντιγραφή

### <a name="copy-across-file-shares"></a>Αντιγραφή σε κοινόχρηστα στοιχεία αρχείων

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

### <a name="copy-from-file-share-to-blob"></a>Αντιγραφή από κοινή χρήση αρχείου για να αντικειμένων blob

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Σημειώστε ότι ασύγχρονης αντιγραφή από χώρο αποθήκευσης αρχείων σε σελίδα Blob δεν υποστηρίζεται.

### <a name="copy-from-blob-to-file-share"></a>Αντιγραφή από blob σε κοινή χρήση αρχείου

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

### <a name="synchronously-copy-files"></a>Σύγχρονη αντιγραφή αρχείων

Μπορείτε να καθορίσετε το `/SyncCopy` την επιλογή για να αντιγράψετε δεδομένα από το χώρο αποθήκευσης αρχείων στο χώρο αποθήκευσης αρχείων, από το χώρο αποθήκευσης αρχείων με τον χώρο αποθήκευσης αντικειμένων Blob και από το χώρο αποθήκευσης αντικειμένων Blob στο χώρο αποθήκευσης αρχείων σύγχρονη, AzCopy το κάνει αυτό κατά τη λήψη του αρχείου δεδομένων προέλευσης τοπικής μνήμης και στείλτε το ξανά στον προορισμό.

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Κατά την αντιγραφή από το χώρο αποθήκευσης αρχείων στο χώρο αποθήκευσης αντικειμένων Blob, ο προεπιλεγμένος τύπος blob είναι blob μπλοκ, χρήστης μπορεί να καθορίσει την επιλογή `/BlobType:page` για να αλλάξετε τον τύπο blob προορισμού.

Λάβετε υπόψη ότι `/SyncCopy` μπορεί να δημιουργήσει επιπλέον εξόδου κόστους σύγκριση ασύγχρονης αντίγραφο, συνιστάται η προσέγγιση είναι να χρησιμοποιήσετε αυτήν την επιλογή σε η Εικονική Azure που βρίσκεται στην ίδια περιοχή ως του λογαριασμού χώρου αποθήκευσης προέλευσης για να αποφύγετε κόστος εξόδου.

## <a name="table-export"></a>Πίνακας: εξαγωγή

### <a name="export-table"></a>Εξαγωγή πίνακα

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy συντάσσει ένα αρχείο δηλώσεων σε καθορισμένο φάκελο προορισμού. Το αρχείο δήλωσης χρησιμοποιείται στη διαδικασία εισαγωγής για να εντοπίσετε τα απαραίτητα αρχεία δεδομένων και εκτελεί επικύρωση δεδομένων. Το αρχείο δήλωσης χρησιμοποιεί τους παρακάτω κανόνες ονοματοθεσίας από προεπιλογή:

    <account name>_<table name>_<timestamp>.manifest

Χρήστης επίσης, να καθορίσετε την επιλογή `/Manifest:<manifest file name>` για να ορίσετε το όνομα του αρχείου δήλωσης.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest

### <a name="split-export-into-multiple-files"></a>Διαίρεση εξαγωγή σε πολλαπλά αρχεία

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy χρησιμοποιεί ένα *ευρετήριο ένταση* στα ονόματα αρχείων δεδομένων διαιρεμένης να διακρίνετε τα πολλαπλά αρχεία. Το ευρετήριο ένταση αποτελείται από δύο μέρη, ένα *διαμερίσματα ευρετηρίου κλειδιού περιοχής* και *Διαίρεση ευρετήριο αρχείων*. Τα ευρετήρια και τα δύο έχουν ως βάση το μηδέν.

Ο δείκτης κλειδιού περιοχή διαμερίσματα θα είναι 0, εάν ο χρήστης δεν καθορίσει την επιλογή `/PKRS`.

Για παράδειγμα, ας υποθέσουμε ότι AzCopy δημιουργεί δύο αρχεία δεδομένων αφού ο χρήστης Καθορίζει την επιλογή `/SplitSize`. Τα ονόματα των αρχείων δεδομένων που προκύπτει μπορεί να είναι:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Σημειώστε ότι η την ελάχιστη δυνατή τιμή για την επιλογή `/SplitSize` είναι 32MB. Εάν η καθορισμένη προορισμός είναι χώρος αποθήκευσης αντικειμένων Blob, AzCopy θα διαίρεση του αρχείου δεδομένων όταν τα μεγέθη φτάσει τον περιορισμό μεγέθους αντικειμένων blob (200GB), ανεξάρτητα από εάν την επιλογή `/SplitSize` έχει οριστεί από το χρήστη.

### <a name="export-table-to-json-or-csv-data-file-format"></a>Εξαγωγή πίνακα σε μορφή αρχείου δεδομένων JSON ή CSV

AzCopy από προεπιλογή εξάγει πίνακες σε αρχεία δεδομένων JSON. Μπορείτε να καθορίσετε την επιλογή `/PayloadFormat:JSON|CSV` για να εξαγάγετε τους πίνακες ως JSON ή CSV.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Κατά τον καθορισμό της μορφής φορτίο CSV, AzCopy θα δημιουργήσει επίσης ένα αρχείο σχήματος με επέκταση αρχείου `.schema.csv` για κάθε αρχείο δεδομένων.

### <a name="export-table-entities-concurrently"></a>Εξαγωγή πίνακα οντοτήτων ταυτόχρονα

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

AzCopy θα ξεκινήσει ταυτόχρονες λειτουργίες για να εξαγάγετε οντοτήτων όταν ο χρήστης Καθορίζει την επιλογή `/PKRS`. Κάθε λειτουργίας εξάγει μία περιοχή κλειδιού διαμερίσματα.

Σημειώστε ότι ο αριθμός των λειτουργιών ταυτόχρονες ελέγχεται επίσης από την επιλογή `/NC`. AzCopy χρησιμοποιεί του αριθμού των επεξεργαστών πυρήνα ως την προεπιλεγμένη τιμή του `/NC` κατά την αντιγραφή πίνακα οντοτήτων, ακόμα και αν `/NC` δεν έχει καθοριστεί. Όταν ο χρήστης Καθορίζει την επιλογή `/PKRS`, AzCopy χρησιμοποιεί το μικρότερο από τις δύο τιμές - partition βασικές περιοχές έναντι σιωπηρά ή ρητά που καθορίζεται ταυτόχρονες λειτουργίες - να καθορίσετε τον αριθμό των ταυτόχρονες λειτουργίες για να ξεκινήσετε. Για περισσότερες λεπτομέρειες, πληκτρολογήστε `AzCopy /?:NC` στη γραμμή εντολών.

### <a name="export-table-to-blob"></a>Εξαγωγή πίνακα για να αντικειμένων blob

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy θα δημιουργήσει ένα αρχείο δεδομένων JSON στο κοντέινερ αντικειμένων blob με παρακολούθηση κανόνες ονοματοθεσίας:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Αρχείο δεδομένων που δημιουργήθηκε JSON ακολουθεί τη μορφή φορτίο για ελάχιστους μετα-δεδομένων. Για λεπτομέρειες σχετικά με αυτήν τη μορφή φορτίο, ανατρέξτε στο θέμα [Μορφοποίηση φορτίο για λειτουργίες υπηρεσίας πίνακα](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Σημειώστε ότι κατά την εξαγωγή πίνακες σε αντικείμενα blob, AzCopy θα κάνετε λήψη των οντοτήτων πίνακας στον τοπικό προσωρινά αρχεία δεδομένων και να στείλτε αυτές τις οντοτήτων το αντικείμενο blob. Αυτά τα προσωρινά αρχεία δεδομένων τοποθετούνται στο φάκελο αρχείων εγγραφών με την προεπιλεγμένη διαδρομή "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", μπορείτε να καθορίσετε την επιλογή/Z: [εγγραφών-αρχείο-φάκελος] για να αλλάξετε τις εγγραφές του αρχείου θέση φακέλου και, επομένως, να αλλάξετε τη θέση των αρχείων προσωρινά δεδομένα. Τα προσωρινά αρχεία δεδομένων μέγεθος καθορίζεται από τον πίνακα οντοτήτων και το μέγεθος που καθορίσατε με την επιλογή /SplitSize, παρόλο που το αρχείο προσωρινά δεδομένα σε στον τοπικό δίσκο θα διαγραφούν αμέσως μόλις έχει αποσταλεί η blob, βεβαιωθείτε ότι έχετε αρκετό χώρο στον τοπικό δίσκο για να αποθηκεύσετε αυτά τα προσωρινά αρχεία δεδομένων, προτού διαγραφούν.

## <a name="table-import"></a>Πίνακας: εισαγωγή

### <a name="import-table"></a>Εισαγωγή πίνακα

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace

Η επιλογή `/EntityOperation` υποδεικνύει πώς μπορείτε να εισαγάγετε οντοτήτων στον πίνακα. Πιθανές τιμές είναι:

- `InsertOrSkip`: Παραλείπει μια υπάρχουσα οντότητα ή εισάγει μια νέα οντότητα, αν δεν υπάρχει στον πίνακα.
- `InsertOrMerge`: Συγχωνεύει μια υπάρχουσα οντότητα ή εισάγει μια νέα οντότητα, αν δεν υπάρχει στον πίνακα.
- `InsertOrReplace`: Αντικαθιστά μια υπάρχουσα οντότητα ή εισάγει μια νέα οντότητα, αν δεν υπάρχει στον πίνακα.

Σημειώστε ότι δεν μπορείτε να καθορίσετε την επιλογή `/PKRS` στο σενάριο εισαγωγής. Σε αντίθεση με το σενάριο εξαγωγή, στην οποία πρέπει να καθορίσετε την επιλογή `/PKRS` για να ξεκινήσετε ταυτόχρονες λειτουργίες, AzCopy από προεπιλογή αρχίζει ταυτόχρονες λειτουργίες κατά την εισαγωγή ενός πίνακα. Ο προεπιλεγμένος αριθμός ταυτόχρονες λειτουργίες αποτελέσματα είναι ίση με του αριθμού των επεξεργαστών πυρήνα; Ωστόσο, μπορείτε να καθορίσετε διαφορετικό αριθμό ταυτόχρονες με την επιλογή `/NC`. Για περισσότερες λεπτομέρειες, πληκτρολογήστε `AzCopy /?:NC` στη γραμμή εντολών.

Σημειώστε ότι AzCopy υποστηρίζει μόνο εισαγωγή για JSON, δεν CSV. AzCopy δεν υποστηρίζουν εισαγωγές πίνακα από JSON που δημιουργήθηκαν από το χρήστη και δήλωσης αρχεία. Και τα δύο από αυτά τα αρχεία πρέπει να προέρχονται από μια εξαγωγή πίνακα AzCopy. Για να αποφύγετε σφάλματα, επικοινωνήστε μην τροποποιείτε το εξαγόμενο JSON ή αρχείο δηλώσεων.

### <a name="import-entities-to-table-using-blobs"></a>Εισαγωγή οντοτήτων στον πίνακα με αντικείμενα BLOB

Ας υποθέσουμε ότι ένα κοντέινερ αντικειμένων Blob περιλαμβάνει τις ακόλουθες: JSON ένα αρχείο που αντιπροσωπεύει έναν πίνακα του Azure και τις συνοδευτικές αρχείο δηλώσεων.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Μπορείτε να εκτελέσετε την παρακάτω εντολή για να εισαγάγετε οντοτήτων σε έναν πίνακα χρησιμοποιώντας το αρχείο δήλωσης σε αυτό το κοντέινερ αντικειμένων blob:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"

## <a name="other-azcopy-features"></a>Άλλες δυνατότητες AzCopy

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Αντιγραφή μόνο των δεδομένων που δεν υπάρχει στον προορισμό

Το `/XO` και `/XN` παράμετροι σάς επιτρέπουν να εξαιρέσετε πόρους παλαιότερων ή νεότερη έκδοση προέλευσης από την αντιγραφή, αντίστοιχα. Εάν θέλετε μόνο να αντιγράψετε πόρους προέλευσης που δεν υπάρχουν στον προορισμό, μπορείτε να καθορίσετε και τις δύο παραμέτρους στην εντολή AzCopy:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Σημείωση: Αυτή δεν υποστηρίζεται όταν το αρχείο προέλευσης ή προορισμού είναι ένας πίνακας.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Χρησιμοποιήστε ένα αρχείο ανταπόκρισης για να καθορίσετε παραμέτρους γραμμής εντολών

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

Μπορείτε να συμπεριλάβετε τις παραμέτρους γραμμής εντολών AzCopy σε ένα αρχείο αποκρίσεων. AzCopy επεξεργάζεται τις παραμέτρους του αρχείου σαν να είχατε οριστεί στη γραμμή εντολών, εκτελεί ένα άμεσο υποκατάστασης με τα περιεχόμενα του αρχείου.

Ας υποθέσουμε ότι ένα αρχείο αποκρίσεων με το όνομα `copyoperation.txt`, που περιέχει τις ακόλουθες γραμμές. Κάθε παράμετρο AzCopy μπορεί να καθοριστεί σε μία μόνο γραμμή

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

ή σε διαφορετικούς γραμμές:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy θα αποτύχει αν διαιρέσετε την παράμετρο σε δύο γραμμές, όπως φαίνεται εδώ για το `/sourcekey` παραμέτρου:

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Χρήση πολλών αρχείων απόκρισης για να καθορίσετε παραμέτρους γραμμής εντολών

Ας υποθέσουμε ότι ένα αρχείο αποκρίσεων με το όνομα `source.txt` που προσδιορίζει ένα κοντέινερ προέλευση:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

Και ένα αρχείο απάντηση με την ονομασία `dest.txt` που προσδιορίζει ένα φάκελο προορισμού στο σύστημα αρχείων:

    /Dest:C:\myfolder

Και ένα αρχείο απάντηση με την ονομασία `options.txt` που καθορίζει επιλογές για AzCopy:

    /S /Y

Για να καλέσετε AzCopy με αυτά τα αρχεία απόκρισης, οι οποίες βρίσκονται σε έναν κατάλογο `C:\responsefiles`, χρησιμοποιήστε αυτή την εντολή:

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy επεξεργάζονται αυτήν την εντολή, όπως ακριβώς αν έχετε συμπεριλάβει όλες τις μεμονωμένες παραμέτρους γραμμής εντολών:

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

### <a name="specify-a-shared-access-signature-sas"></a>Καθορίστε μια υπογραφή κοινόχρηστη πρόσβαση (συσχετισμών Ασφαλείας)

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

Μπορείτε επίσης να καθορίσετε μια συσχετισμών Ασφαλείας στο κοντέινερ URI:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

### <a name="journal-file-folder"></a>Φάκελο αρχείων εγγραφών

Κάθε φορά που δίνετε την εντολή να AzCopy, ελέγχει εάν υπάρχει ένα αρχείο εγγραφών στον προεπιλεγμένο φάκελο ή αν υπάρχει σε ένα φάκελο που έχετε καθορίσει μέσω αυτήν την επιλογή. Εάν δεν υπάρχει το αρχείο εγγραφών σε κάποιο σημείο, AzCopy χειρίζεται τη λειτουργία ως νέα και δημιουργεί ένα νέο αρχείο εγγραφών.

Εάν υπάρχει το αρχείο εγγραφών, AzCopy θα Ελέγξτε εάν η γραμμή εντολών που μπορείτε να εισαγάγετε συμφωνεί με τη γραμμή εντολών στο αρχείο εγγραφών. Εάν ταιριάζουν με τις δύο γραμμές εντολών, AzCopy ισχύει η λειτουργία δεν ολοκληρώθηκε. Εάν δεν συμφωνούν, θα σας ζητηθεί να αντικαταστήσετε είτε το αρχείο εγγραφών για να ξεκινήσετε μια νέα λειτουργία ή για να ακυρώσετε την τρέχουσα λειτουργία.

Εάν θέλετε να χρησιμοποιήσετε την προεπιλεγμένη θέση για το αρχείο εγγραφών:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Εάν παραλειφθεί το όρισμα επιλογή `/Z`, ή να καθορίσετε την επιλογή `/Z` χωρίς τη διαδρομή του φακέλου, όπως φαίνεται παραπάνω, AzCopy δημιουργεί το αρχείο εγγραφών στην προεπιλεγμένη θέση, η οποία είναι `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Εάν υπάρχει ήδη στο αρχείο εγγραφών, AzCopy συνεχίζει τη λειτουργία με βάση το αρχείο εγγραφών.

Εάν θέλετε να καθορίσετε μια προσαρμοσμένη θέση για το αρχείο εγγραφών:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

Αυτό το παράδειγμα δημιουργεί το αρχείο εγγραφών, εάν δεν υπάρχει ήδη. Εάν υπάρχει, AzCopy συνεχίζει τη λειτουργία με βάση το αρχείο εγγραφών.

Εάν θέλετε να συνεχίσετε μια λειτουργία AzCopy:

    AzCopy /Z:C:\journalfolder\

Αυτό το παράδειγμα συνεχίζει την τελευταία εργασία που μπορεί να απέτυχε να ολοκληρωθεί.

### <a name="generate-a-log-file"></a>Δημιουργία ενός αρχείου καταγραφής

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Εάν καθορίσετε την επιλογή `/V` χωρίς να σας παρέχει μια διαδρομή αρχείου για το λεπτομερές αρχείο καταγραφής, στη συνέχεια, AzCopy δημιουργεί το αρχείο καταγραφής στην προεπιλεγμένη θέση, η οποία είναι `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Διαφορετικά, μπορείτε να δημιουργήσετε ένα αρχείο καταγραφής σε μια προσαρμοσμένη θέση:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Λάβετε υπόψη ότι εάν καθορίσετε μια σχετική διαδρομή μετά από την επιλογή `/V`, όπως είναι οι `/V:test/azcopy1.log`, στη συνέχεια, η Λεπτομερής καταγραφή δημιουργείται στο τον τρέχοντα κατάλογο εργασίας μέσα σε έναν υποφάκελο με το όνομα `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Καθορίστε τον αριθμό των ταυτόχρονες λειτουργίες για να ξεκινήσετε

Επιλογή `/NC` καθορίζει τον αριθμό των λειτουργιών ταυτόχρονες αντιγραφής. Από προεπιλογή, AzCopy ξεκινά έναν ορισμένο αριθμό ταυτόχρονες λειτουργίες για να αυξήσετε την ταχύτητα μεταφοράς δεδομένων. Για λειτουργίες πίνακα, ο αριθμός των λειτουργιών ταυτόχρονες ισούται με τον αριθμό των επεξεργαστών που έχετε. Για λειτουργίες Blob και αρχείων, ο αριθμός των λειτουργιών ταυτόχρονες είναι ισούται με 8 ώρες του αριθμού των επεξεργαστών που έχετε. Εάν εκτελείτε AzCopy μέσω χαμηλού εύρους ζώνης δικτύου, μπορείτε να καθορίσετε ένα μικρότερο αριθμό για /NC για να αποφύγετε την αποτυχία που προκαλούνται από ανταγωνισμό πόρων.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Εκτέλεση AzCopy έναντι προσομοίωσης Azure χώρου αποθήκευσης

Μπορείτε να εκτελέσετε AzCopy έναντι του [Azure αποθήκευσης προσομοίωσης](storage-use-emulator.md) για αντικείμενα BLOB:

    AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

και πίνακες:

    AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table

## <a name="azcopy-parameters"></a>Παράμετροι AzCopy

Παράμετροι για AzCopy περιγράφονται παρακάτω. Μπορείτε επίσης να πληκτρολογήσετε μία από τις παρακάτω εντολές από τη γραμμή εντολών για βοήθεια στη χρήση των AzCopy:

- Για λεπτομερή βοήθεια της γραμμής εντολών για το AzCopy:`AzCopy /?`
- Για λεπτομερή βοήθεια σχετικά με κάποια παράμετρο AzCopy:`AzCopy /?:SourceKey`
- Για παραδείγματα γραμμής εντολών:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Προέλευσης: "Προέλευση"

Καθορίζει τα δεδομένα προέλευσης από την οποία θέλετε να αντιγράψετε. Η προέλευση μπορεί να είναι ένας κατάλογος συστήματος αρχείων, ένα κοντέινερ αντικειμένων blob, ένα εικονικό κατάλογο αντικειμένων blob, ένα κοινόχρηστο αρχείο χώρου αποθήκευσης, ένας κατάλογος χώρο αποθήκευσης αρχείων ή έναν πίνακα του Azure.

**Ισχύει για:** Αντικείμενα blob, αρχεία, πίνακες

### <a name="destdestination"></a>/ Προορισμού: "προορισμού"

Καθορίζει τον προορισμό για να αντιγράψετε. Ο προορισμός μπορεί να είναι ένας κατάλογος συστήματος αρχείων, ένα κοντέινερ αντικειμένων blob, ένα εικονικό κατάλογο αντικειμένων blob, ένα κοινόχρηστο αρχείο χώρου αποθήκευσης, ένας κατάλογος χώρο αποθήκευσης αρχείων ή έναν πίνακα του Azure.

**Ισχύει για:** Αντικείμενα blob, αρχεία, πίνακες

### <a name="patternfile-pattern"></a>/ Μοτίβο: "αρχείο-μοτίβο"

Καθορίζει ένα μοτίβο αρχείου που υποδεικνύει ποια αρχεία για να αντιγράψετε. Η συμπεριφορά της παραμέτρου /Pattern προσδιορίζεται από τη θέση των δεδομένων προέλευσης και την παρουσία της λειτουργίας επιλογής περιοδικότητας. Λειτουργία επαναλαμβανόμενες καθορίζεται μέσω επιλογή/s.

Εάν το συγκεκριμένο αρχείο προέλευσης είναι ένας κατάλογος στο σύστημα αρχείων, στη συνέχεια, τυπική χαρακτήρες μπαλαντέρ είναι στην πραγματικότητα και το μοτίβο αρχείου που παρέχονται είναι αντιστοιχισμένος σε σχέση με τα αρχεία στον κατάλογο του. Εάν η επιλογή έχει καθοριστεί, στη συνέχεια, AzCopy ταιριάζει επίσης το καθορισμένο μοτίβο σύμφωνα με όλα τα αρχεία των υποφακέλων κάτω από τον κατάλογο.

Εάν το συγκεκριμένο αρχείο προέλευσης είναι ένα κοντέινερ αντικειμένων blob ή εικονικού καταλόγου, στη συνέχεια, χαρακτήρες μπαλαντέρ δεν εφαρμόζονται. Εάν η επιλογή έχει καθοριστεί, στη συνέχεια, AzCopy ερμηνεύει το μοτίβο καθορισμένο αρχείο ως πρόθεμα blob. Εάν η επιλογή /S δεν έχει καθοριστεί, στη συνέχεια, AzCopy ταιριάζει το μοτίβο αρχείου σε σχέση με ονόματα αντικειμένων blob ακριβή.

Εάν το καθορισμένο αρχείο προέλευσης είναι ένα κοινόχρηστο αρχείο Azure, στη συνέχεια, θα πρέπει να καθορίσετε το ακριβές όνομα αρχείου, (π.χ., abc.txt) για να αντιγράψετε ένα αρχείο, ή να καθορίσετε την επιλογή /S για να αντιγράψετε όλα τα αρχεία σε σταδιακά την κοινή χρήση. Για να καθορίσετε ένα μοτίβο αρχείου και την επιλογή /S μαζί θα έχει ως αποτέλεσμα σφάλμα.

AzCopy χρησιμοποιεί που ταιριάζουν με διάκριση πεζών-κεφαλαίων όταν το /Source είναι ένα κοντέινερ αντικειμένων blob ή blob εικονικού καταλόγου και χρησιμοποιεί Ταίριασμα πεζών-κεφαλαίων σε όλες τις άλλες περιπτώσεις.

Προεπιλεγμένο αρχείο μοτίβου που χρησιμοποιείται όταν δεν υπάρχει μοτίβο αρχείου έχει καθοριστεί είναι *.* για μια θέση του συστήματος αρχείων ή μια κενή πρόθεμα για μια θέση αποθήκευσης Azure. Καθορισμός μοτίβα πολλών αρχείων δεν υποστηρίζεται.

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="destkeystorage-key"></a>/ DestKey: "κλειδί αποθήκευσης"

Καθορίζει το κλειδί λογαριασμού χώρου αποθήκευσης για τον πόρο προορισμού.

**Ισχύει για:** Αντικείμενα blob, αρχεία, πίνακες

### <a name="destsassas-token"></a>/ DestSAS: "συσχετισμών ασφαλείας-διακριτικό"

Καθορίζει μια κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας) με δικαιώματα ΑΝΆΓΝΩΣΗΣ και ΕΓΓΡΑΦΉΣ για τον προορισμό (εάν υπάρχει). Περιβάλλουν τις συσχετίσεις Ασφαλείας με διπλά εισαγωγικά, όπως μπορεί να περιέχει ειδικούς χαρακτήρες της γραμμής εντολών.

Εάν ο πόρος προορισμού είναι μια blob κοντέινερ, κοινή χρήση αρχείου ή έναν πίνακα, μπορείτε είτε να καθορίσετε αυτήν την επιλογή ακολουθούμενο από το διακριτικό συσχετισμών Ασφαλείας ή μπορείτε να καθορίσετε τις συσχετίσεις Ασφαλείας ως μέρος του κοντέινερ αντικειμένων blob προορισμού, κοινή χρήση αρχείου ή URI του πίνακα, χωρίς αυτήν την επιλογή.

Εάν προέλευσης και προορισμού είναι και τα δύο αντικείμενα blob, στη συνέχεια, το αντικείμενο blob προορισμού πρέπει να βρίσκονται στην τον ίδιο λογαριασμό χώρου αποθήκευσης ως το blob προέλευσης.

**Ισχύει για:** Αντικείμενα blob, αρχεία, πίνακες

### <a name="sourcekeystorage-key"></a>/ SourceKey: "κλειδί αποθήκευσης"

Καθορίζει το κλειδί λογαριασμού χώρου αποθήκευσης για τον πόρο προέλευσης.

**Ισχύει για:** Αντικείμενα blob, αρχεία, πίνακες

### <a name="sourcesassas-token"></a>/ SourceSAS: "συσχετισμών ασφαλείας-διακριτικό"

Καθορίζει μια υπογραφή πρόσβασης σε κοινή χρήση με δικαιώματα ΑΝΆΓΝΩΣΗΣ και ΛΊΣΤΑ για την προέλευση (εάν υπάρχει). Περιβάλλουν τις συσχετίσεις Ασφαλείας με διπλά εισαγωγικά, όπως μπορεί να περιέχει ειδικούς χαρακτήρες της γραμμής εντολών.

Εάν ο πόρος προέλευσης είναι ένα κοντέινερ αντικειμένων blob και παρέχεται μια συσχετισμών Ασφαλείας ούτε έναν αριθμό-κλειδί, στη συνέχεια, θα διαβάσει το κοντέινερ αντικειμένων blob μέσω ανώνυμης πρόσβασης.

Εάν το αρχείο προέλευσης είναι ένα κοινόχρηστο αρχείο ή πίνακα, πρέπει να δοθεί έναν αριθμό-κλειδί ή μια συσχετισμών Ασφαλείας.

**Ισχύει για:** Αντικείμενα blob, αρχεία, πίνακες

### <a name="s"></a>/S

Καθορίζει τη λειτουργία περιοδικότητας για λειτουργίες αντιγραφής. Στη λειτουργία περιοδικότητας, AzCopy θα αντιγράψετε όλα τα αντικείμενα BLOB ή τα αρχεία που ταιριάζουν με το καθορισμένο αρχείο μοτίβο, συμπεριλαμβανομένων των υποφακέλων.

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="blobtypeblock--page--append"></a>/ BlobType: "μπλοκ" | "σελίδα" | "προσαρτήσει"

Καθορίζει αν το αντικείμενο blob προορισμού είναι ένα μπλοκ blob, ένα blob σελίδας ή ένα blob προσάρτησης. Η επιλογή αυτή εφαρμόζεται μόνο όταν στέλνετε ένα blob. Διαφορετικά, παρουσιάζεται σφάλμα. Εάν ο προορισμός είναι ένα blob και αυτή η επιλογή δεν έχει καθοριστεί, από προεπιλογή, AzCopy δημιουργεί ένα μπλοκ blob.

**Ισχύει για:** Αντικείμενα BLOB

### <a name="checkmd5"></a>/ CheckMD5

Υπολογίζει μια κατακερματισμός MD5 για τα δεδομένα έχουν ληφθεί και επαληθεύει ότι ο κατακερματισμός MD5 είναι αποθηκευμένα στο αντικείμενο blob ή του αρχείου περιεχομένου-MD5 ιδιότητα αντιστοιχεί ο κατακερματισμός υπολογισμού. Ο έλεγχος MD5 είναι απενεργοποιημένη από προεπιλογή, επομένως πρέπει να ορίσετε αυτήν την επιλογή για την εκτέλεση του ελέγχου MD5 κατά τη λήψη δεδομένων.

Σημειώστε ότι αποθήκευσης Azure δεν εγγυάται ότι ο κατακερματισμός MD5 αποθηκεύονται για το blob ή το αρχείο είναι ενημερωμένα. Είναι ευθύνη του προγράμματος-πελάτη για να ενημερώσετε το MD5 κάθε φορά που το blob ή το αρχείο έχει τροποποιηθεί.

AzCopy ορίζει πάντα την ιδιότητα Content-MD5 για ένα αρχείο ή αντικειμένων blob του Azure μετά την αποστολή του στην υπηρεσία.  

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="snapshot"></a>/ Στιγμιότυπο

Υποδεικνύει εάν θέλετε να μεταφέρετε στιγμιότυπα. Αυτή η επιλογή ισχύει μόνο όταν το αρχείο προέλευσης είναι ένα blob.

Τα στιγμιότυπα μεταφερθέντων blob έχουν μετονομαστεί σε αυτήν τη μορφή: .extension ονομάτων αντικειμένων blob (στιγμιότυπο-ώρα)

Από προεπιλογή, δεν αντιγράφονται στιγμιότυπα.

**Ισχύει για:** Αντικείμενα BLOB

### <a name="vverbose-log-file"></a>/ V: [λεπτομερές--αρχείο καταγραφής]

Μηνύματα κατάστασης λεπτομερές εξόδους σε ένα αρχείο καταγραφής.

Από προεπιλογή, το αρχείο Λεπτομερής καταγραφή ονομάζεται AzCopyVerbose.log στο `%LocalAppData%\Microsoft\Azure\AzCopy`. Εάν καθορίσετε μια υπάρχουσα θέση αρχείου για αυτήν την επιλογή, η Λεπτομερής καταγραφή θα προσαρτηθεί σε αυτό το αρχείο.  

**Ισχύει για:** Αντικείμενα blob, αρχεία, πίνακες

### <a name="zjournal-file-folder"></a>/ Z: [εγγραφών-αρχείο-φάκελος]

Καθορίζει ένα φάκελο αρχείων εγγραφών για συνέχιση μιας λειτουργίας.

AzCopy υποστηρίζει πάντα συνέχιση εάν μια λειτουργία έχει διακοπεί.

Εάν αυτή η επιλογή δεν έχει καθοριστεί ή έχει καθοριστεί χωρίς μια διαδρομή φακέλου, στη συνέχεια, AzCopy θα δημιουργήσει το αρχείο εγγραφών στην προεπιλεγμένη θέση, η οποία είναι % LocalAppData%\Microsoft\Azure\AzCopy.

Κάθε φορά που δίνετε την εντολή να AzCopy, ελέγχει εάν υπάρχει ένα αρχείο εγγραφών στον προεπιλεγμένο φάκελο ή αν υπάρχει σε ένα φάκελο που έχετε καθορίσει μέσω αυτήν την επιλογή. Εάν δεν υπάρχει το αρχείο εγγραφών σε κάποιο σημείο, AzCopy χειρίζεται τη λειτουργία ως νέα και δημιουργεί ένα νέο αρχείο εγγραφών.

Εάν υπάρχει το αρχείο εγγραφών, AzCopy θα Ελέγξτε εάν η γραμμή εντολών που μπορείτε να εισαγάγετε συμφωνεί με τη γραμμή εντολών στο αρχείο εγγραφών. Εάν ταιριάζουν με τις δύο γραμμές εντολών, AzCopy ισχύει η λειτουργία δεν ολοκληρώθηκε. Εάν δεν συμφωνούν, θα σας ζητηθεί να αντικαταστήσετε είτε το αρχείο εγγραφών για να ξεκινήσετε μια νέα λειτουργία ή για να ακυρώσετε την τρέχουσα λειτουργία.

Το αρχείο εγγραφών διαγράφεται μετά την επιτυχή ολοκλήρωση της λειτουργίας.

Σημειώστε ότι συνέχιση μιας λειτουργίας από ένα αρχείο εγγραφών που δημιουργήθηκε από μια προηγούμενη έκδοση του AzCopy δεν υποστηρίζεται.

**Ισχύει για:** Αντικείμενα blob, αρχεία, πίνακες

### <a name="parameter-file"></a>/@:"parameter-file"

Καθορίζει ένα αρχείο που περιέχει τις παραμέτρους. AzCopy επεξεργάζεται τις παραμέτρους του αρχείου, όπως ακριβώς αν έχει οριστεί στη γραμμή εντολών.

Σε ένα αρχείο απόκρισης, μπορείτε να καθορίσετε πολλές παραμέτρους σε μία μόνο γραμμή ή να καθορίσετε κάθε παράμετρο σε ξεχωριστή γραμμή. Σημειώστε ότι μια επιμέρους παράμετρος δεν είναι δυνατό να εκτείνεται σε πολλές γραμμές.

Αρχεία ανταπόκρισης μπορεί να περιλαμβάνει γραμμές σχόλια που αρχίζουν με το σύμβολο #.

Μπορείτε να καθορίσετε πολλά αρχεία απόκρισης. Ωστόσο, σημειώστε ότι AzCopy δεν υποστηρίζει ένθετων αρχείων ανταπόκρισης.

**Ισχύει για:** Αντικείμενα blob, αρχεία, πίνακες

### <a name="y"></a>/Y

Αποκρύπτει όλα τα μηνύματα επιβεβαίωσης AzCopy.

**Ισχύει για:** Αντικείμενα blob, αρχεία, πίνακες

### <a name="l"></a>/ L

Καθορίζει μια λειτουργία καταχώρηση μόνο. αντιγράφεται χωρίς δεδομένα.

AzCopy θα ερμηνεύσετε τη χρήση αυτής της επιλογής ως ένα προσομοίωσης για την εκτέλεση της γραμμής εντολών χωρίς αυτή την επιλογή/l και μετρήσετε πόσες αντικείμενα θα αντιγραφούν, μπορείτε να ορίσετε την επιλογή /V την ίδια στιγμή για να ελέγξετε ποια αντικείμενα θα αντιγραφούν στο λεπτομερές αρχείο καταγραφής.

Η συμπεριφορά της αυτήν την επιλογή επίσης προσδιορίζεται από τη θέση των δεδομένων προέλευσης και την παρουσία της περιοδικότητας λειτουργία επιλογή /S και αρχείο μοτίβου επιλογής /Pattern.

AzCopy απαιτεί δικαιώματα ΑΝΆΓΝΩΣΗΣ και ΛΊΣΤΑ από αυτήν τη θέση προέλευσης με αυτήν την επιλογή.

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="mt"></a>/MT

Ορίζει ώρα τελευταίας τροποποίησης του αρχείου λήψης για να είναι ίδια με την προέλευση blob ή του αρχείου.

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="xn"></a>/XN

Αποκλείει τους νεότερη προέλευση πόρου. Ο πόρος δεν θα αντιγραφούν εάν ώρας της τελευταίας τροποποίησης της προέλευσης είναι ο ίδιος ή νεότερη έκδοση από προορισμού.

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="xo"></a>/XO

Αποκλείει τους σε έναν πόρο παλαιότερων προέλευσης. Ο πόρος δεν θα αντιγραφούν εάν ώρας της τελευταίας τροποποίησης της προέλευσης είναι ο ίδιος ή παλιότερα από προορισμού.

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="a"></a>/A

Κάνει αποστολή μόνο τα αρχεία που έχουν το χαρακτηριστικό φύλαξης.

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="iarashcnetoi"></a>/ Ι Α: [RASHCNETOI]

Κάνει αποστολή μόνο τα αρχεία που έχουν οποιαδήποτε από το σύνολο καθορισμένα χαρακτηριστικά.

Διαθέσιμα χαρακτηριστικά περιλαμβάνουν τα εξής:

- R = αρχεία μόνο για ανάγνωση
- A = αρχεία έτοιμα για αρχειοθέτηση
- S = αρχεία συστήματος
- H = κρυφά αρχεία
- C = συμπιεσμένα αρχεία
- N = κανονική αρχεία
- E = κρυπτογραφημένο αρχεία
- T = προσωρινών αρχείων
- O = αρχεία χωρίς σύνδεση
- Να = αρχεία μη ευρετηρίου

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Εξαίρεση των αρχείων που έχουν οποιαδήποτε από το σύνολο καθορισμένα χαρακτηριστικά.

Διαθέσιμα χαρακτηριστικά περιλαμβάνουν τα εξής:

- R = αρχεία μόνο για ανάγνωση
- A = αρχεία έτοιμα για αρχειοθέτηση
- S = αρχεία συστήματος
- H = κρυφά αρχεία
- C = συμπιεσμένα αρχεία
- N = κανονική αρχεία
- E = κρυπτογραφημένο αρχεία
- T = προσωρινών αρχείων
- O = αρχεία χωρίς σύνδεση
- Να = αρχεία μη ευρετηρίου

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="delimiterdelimiter"></a>/ Οριοθέτη: "διαχωριστικό"

Υποδεικνύει τον χαρακτήρα οριοθέτη που χρησιμοποιούνται για να διαχωρίζουν εικονικών καταλόγων σε ένα όνομα blob.

Από προεπιλογή, χρησιμοποιεί AzCopy / ως ο χαρακτήρας οριοθέτη. Ωστόσο, AzCopy υποστηρίζει χρησιμοποιώντας οποιαδήποτε κοινές χαρακτήρα (όπως είναι οι @, # ή %) ως οριοθέτη. Εάν θέλετε να συμπεριλάβετε ένα αυτών των ειδικών χαρακτήρων στη γραμμή εντολών, περικλείστε το όνομα του αρχείου με διπλά εισαγωγικά.

Αυτή η επιλογή ισχύει μόνο για τη λήψη αντικείμενα blob.

**Ισχύει για:** Αντικείμενα BLOB

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "αριθμός-του-ταυτόχρονης-λειτουργίες"

Καθορίζει τον αριθμό των ταυτόχρονες λειτουργίες.

AzCopy από προεπιλογή θα ξεκινήσει ένα συγκεκριμένο αριθμό ταυτόχρονες λειτουργίες για να αυξήσετε την ταχύτητα μεταφοράς δεδομένων. Σημειώστε ότι μεγάλου αριθμού ταυτόχρονες λειτουργίες σε περιβάλλον χαμηλού εύρους ζώνης μπορεί να κατακλύζω η σύνδεση δικτύου και να αποτρέψετε τις λειτουργίες από την ολοκλήρωση των πλήρως. Επιτάχυνσης ταυτόχρονες λειτουργίες που βασίζονται σε πραγματικό διαθέσιμο εύρος ζώνης δικτύου.

Το ανώτατο όριο για ταυτόχρονες λειτουργίες είναι 512.

**Ισχύει για:** Αντικείμενα blob, αρχεία, πίνακες

### <a name="sourcetypeblob--table"></a>/ SourceType: "Blob" | "Πίνακας"

Καθορίζει ότι το `source` πόρος είναι μια διαθέσιμη στο περιβάλλον τοπικού ανάπτυξης, εκτελείται στο προσομοίωσης το χώρο αποθήκευσης αντικειμένων blob.

**Ισχύει για:** Αντικείμενα blob, πίνακες

### <a name="desttypeblob--table"></a>/ DestType: "Blob" | "Πίνακας"

Καθορίζει ότι το `destination` πόρος είναι μια διαθέσιμη στο περιβάλλον τοπικού ανάπτυξης, εκτελείται στο προσομοίωσης το χώρο αποθήκευσης αντικειμένων blob.

**Ισχύει για:** Αντικείμενα blob, πίνακες

### <a name="pkrskey1key2key3"></a>/ PKRS: "κλειδί1 κλειδί2 # # κλειδί3 #..."

Διαιρεί την περιοχή κλειδιού διαμερίσματα για να ενεργοποιήσετε την εξαγωγή δεδομένων πίνακα παράλληλα, γεγονός που αυξάνει την ταχύτητα της λειτουργίας εξαγωγής.

Εάν αυτή η επιλογή δεν έχει καθοριστεί, στη συνέχεια, AzCopy χρησιμοποιεί ένα νήμα προς εξαγωγή πίνακα οντοτήτων. Για παράδειγμα, εάν ο χρήστης Καθορίζει /PKRS: "αα #bb", στη συνέχεια, AzCopy ξεκινά τρεις ταυτόχρονες λειτουργίες.

Κάθε λειτουργίας εξάγει μία από τρεις βασικές περιοχές διαμερίσματα, όπως φαίνεται παρακάτω:

  [πρώτη partition-πλήκτρων, ΑΑ)

  [αα, bb)

  [bb, τελευταία-partition-κλειδί]

**Ισχύει για:** Πίνακες

### <a name="splitsizefile-size"></a>/ SplitSize: "μέγεθος αρχείου"

Καθορίζει το εξαγόμενο αρχείο διαίρεση μέγεθος σε MB, την ελάχιστη επιτρεπόμενη τιμή είναι 32.

Εάν αυτή η επιλογή δεν έχει καθοριστεί, AzCopy θα εξάγετε δεδομένα πίνακα σε ενός αρχείου.

Εάν τα δεδομένα του πίνακα εξάγεται σε ένα αντικείμενο blob και το μέγεθος του εξαγόμενου αρχείου φτάσει το όριο 200 GB με μέγεθος αντικειμένων blob, στη συνέχεια, AzCopy θα διαίρεση αρχείου που έχει εξαχθεί, ακόμα και αν αυτή η επιλογή δεν έχει καθοριστεί.

**Ισχύει για:** Πίνακες

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"

Καθορίζει τη συμπεριφορά εισαγωγής δεδομένων πίνακα.

- InsertOrSkip - παραλείπει μια υπάρχουσα οντότητα ή εισάγει μια νέα οντότητα, αν δεν υπάρχει στον πίνακα.

- InsertOrMerge - συγχωνεύει μια υπάρχουσα οντότητα ή εισάγει μια νέα οντότητα, αν δεν υπάρχει στον πίνακα.

- InsertOrReplace - αντικαθιστά μια υπάρχουσα οντότητα ή εισάγει μια νέα οντότητα, αν δεν υπάρχει στον πίνακα.

**Ισχύει για:** Πίνακες

### <a name="manifestmanifest-file"></a>/ Δήλωσης: "δήλωση-αρχείο"

Καθορίζει το αρχείο δήλωσης για τον πίνακα, εξαγωγή και εισαγωγή λειτουργίας.

Αυτή η επιλογή είναι προαιρετική κατά τη διάρκεια της λειτουργίας εξαγωγής, AzCopy θα δημιουργήσει ένα αρχείο δηλώσεων με προκαθορισμένο όνομα Εάν αυτή η επιλογή δεν έχει καθοριστεί.

Αυτή η επιλογή απαιτείται κατά τη λειτουργία εισαγωγής για τον εντοπισμό τα αρχεία δεδομένων.

**Ισχύει για:** Πίνακες

### <a name="synccopy"></a>/ SyncCopy

Υποδεικνύει εάν θέλετε να αντιγράψετε τη σύγχρονη αντικείμενα BLOB ή αρχεία μεταξύ δύο τελικά σημεία αποθήκευσης Azure.

AzCopy από προεπιλογή χρησιμοποιεί ασύγχρονης αντίγραφο στο διακομιστή. Καθορίστε αυτήν την επιλογή για να εκτελέσετε μια συγχρονισμένη αντίγραφο, το οποίο κάνει λήψη αρχεία ή αντικείμενα blob στο τοπικής μνήμης και, στη συνέχεια, μεταφέρει τους με το χώρο αποθήκευσης Azure.

Μπορείτε να χρησιμοποιήσετε αυτήν την επιλογή κατά την αντιγραφή αρχείων σε χώρο αποθήκευσης αντικειμένων Blob, μέσα σε χώρο αποθήκευσης αρχείων ή από το χώρο αποθήκευσης αντικειμένων Blob στο χώρο αποθήκευσης αρχείων ή αντίστροφα.

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "Τύπος περιεχομένου"

Καθορίζει τον τύπο περιεχομένου MIME για αρχεία ή αντικείμενα BLOB προορισμού.

AzCopy ορίζει τον τύπο περιεχομένου για ένα αρχείο ή blob οκτάδα/εφαρμογή-ροή από προεπιλογή. Μπορείτε να ορίσετε τον τύπο περιεχομένου για όλα τα αντικείμενα BLOB ή αρχεία, καθορίζοντας ρητά μια τιμή για αυτήν την επιλογή.

Εάν ορίσετε αυτήν την επιλογή χωρίς τιμή, στη συνέχεια, AzCopy θα ορίσετε κάθε blob ή τύπο περιεχομένου του αρχείου σύμφωνα με την επέκταση αρχείου.

**Ισχύει για:** Αντικείμενα blob, αρχεία

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"

Καθορίζει τη μορφή του αρχείου δεδομένων που έχουν εξαχθεί πίνακα.

Εάν αυτή η επιλογή δεν έχει καθοριστεί, από προεπιλογή AzCopy εξάγει αρχείο δεδομένων του πίνακα σε μορφή JSON.

**Ισχύει για:** Πίνακες

## <a name="known-issues-and-best-practices"></a>Γνωστά θέματα και βέλτιστες πρακτικές

### <a name="limit-concurrent-writes-while-copying-data"></a>Όριο ταυτόχρονες εγγραφές κατά την αντιγραφή δεδομένων

Όταν αντιγράφετε αντικείμενα BLOB ή αρχεία με AzCopy, λάβετε υπόψη ότι μια άλλη εφαρμογή μπορεί να τροποποίηση των δεδομένων κατά την αντιγραφή του. Εάν είναι δυνατόν, βεβαιωθείτε ότι δεν είναι τροποποίηση των δεδομένων που αντιγράφετε κατά τη λειτουργία αντιγραφής. Για παράδειγμα, κατά την αντιγραφή ενός VHD που σχετίζεται με μια εικονική μηχανή Azure, βεβαιωθείτε ότι δεν υπάρχουν άλλες εφαρμογές συντάσσετε αυτήν τη στιγμή στο VHD. Ένας καλός τρόπος για να γίνει αυτό είναι με την εκχώρηση του πόρου να αντιγραφούν. Εναλλακτικά, μπορείτε πρώτα να δημιουργήσετε ένα στιγμιότυπο του VHD και, στη συνέχεια, αντιγράψτε το στιγμιότυπο.

Εάν δεν μπορείτε να αποτρέψετε άλλες εφαρμογές από την εγγραφή σε αρχεία ή αντικείμενα BLOB ενώ γίνεται αντιγραφή, στη συνέχεια, να θυμάστε ότι από τη στιγμή που η εργασία ολοκληρώνεται, τους πόρους που αντιγράψατε ίσως δεν χρειάζεται πλέον πλήρη ομοιομορφίας με τους πόρους προέλευσης.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Εκτελέστε μία παρουσία AzCopy σε έναν υπολογιστή.
AzCopy έχει σχεδιαστεί για να μεγιστοποιήσετε τη χρήση του πόρου υπολογιστή για να επιταχύνετε τη μεταφορά δεδομένων, συνιστούμε να εκτελέσετε μόνο μία παρουσία AzCopy σε έναν υπολογιστή και καθορίστε την επιλογή `/NC` Εάν χρειάζεστε περισσότερες ταυτόχρονες λειτουργίες. Για περισσότερες λεπτομέρειες, πληκτρολογήστε `AzCopy /?:NC` στη γραμμή εντολών.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>Ενεργοποίηση MD5 συμβατών με FIPS για AzCopy όταν έχετε "Χρήση FIPS συμβατή αλγόριθμοι για κρυπτογράφηση, ο κατακερματισμός και υπογραφή".
AzCopy από προεπιλογή χρησιμοποιεί υλοποίηση .NET MD5 για να υπολογίσετε το MD5 κατά την αντιγραφή αντικειμένων, αλλά υπάρχουν ορισμένες απαιτήσεις ασφαλείας που χρειάζεστε AzCopy για να ενεργοποιήσετε τη ρύθμιση FIPS συμβατή MD5.

Μπορείτε να δημιουργήσετε ένα αρχείο app.config `AzCopy.exe.config` με ιδιότητα `AzureStorageUseV1MD5` και να την τοποθετήσετε Ακύρωση με AzCopy.exe.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Για την ιδιότητα "AzureStorageUseV1MD5" • True - η προεπιλεγμένη τιμή, AzCopy θα χρησιμοποιήσει υλοποίησης .NET MD5.
• False – AzCopy θα χρησιμοποιήσει FIPS συμβατή αλγόριθμος MD5.

Σημειώστε ότι συμβατών με FIPS είναι απενεργοποιημένη από προεπιλογή στον υπολογιστή σας με Windows, μπορείτε να πληκτρολογήστε secpol.msc στο παράθυρο εκτέλεση του και να ελέγξετε -> αυτόν το διακόπτη στη ρύθμιση ασφαλείας τοπικής πολιτικής ασφαλείας -> Επιλογές -> Κρυπτογράφηση συστήματος: χρήση συμβατών με FIPS για κρυπτογράφηση, ο κατακερματισμός και υπογραφή.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με το χώρο αποθήκευσης Azure και AzCopy, ανατρέξτε στους ακόλουθους πόρους.

### <a name="azure-storage-documentation"></a>Azure αποθήκευσης τεκμηρίωση:

- [Εισαγωγή στις Azure χώρου αποθήκευσης](storage-introduction.md)
- [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από το .NET](storage-dotnet-how-to-use-blobs.md)
- [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αρχείων από το .NET](storage-dotnet-how-to-use-files.md)
- [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων του από το .NET](storage-dotnet-how-to-use-tables.md)
- [Πώς μπορείτε να δημιουργήσετε, να διαχειρίζεστε ή διαγραφή ενός λογαριασμού χώρου αποθήκευσης](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure καταχωρήσεων ιστολογίου χώρου αποθήκευσης:
- [Παρουσίαση του Azure χώρος αποθήκευσης δεδομένων κίνηση βιβλιοθήκη Preview](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Εισαγωγή στις αντιγραφή σύγχρονη και προσαρμοσμένο τύπο περιεχομένου](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Ανακοίνωση γενικής διαθεσιμότητας 3.0 AzCopy συν έκδοση preview του AzCopy 4.0 με την υποστήριξη πίνακα και αρχείων](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Βελτιστοποιημένη για σενάρια ευρείας κλίμακας αντιγράφου](http://go.microsoft.com/fwlink/?LinkId=507682)
- [AzCopy: Υποστήριξη για πρόσβαση για ανάγνωση παν πλεονάζοντα χώρου αποθήκευσης](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Μεταφορά δεδομένων με επιτρέψει εκ νέου την εκκίνηση λειτουργίας και διακριτικό συσχετισμών Ασφαλείας](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Χρήση αντικειμένων Blob αντίγραφο σταυρό λογαριασμού](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Αποστολή/λήψη αρχείων για αντικείμενα blob του Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
