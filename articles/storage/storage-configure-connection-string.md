<properties 
    pageTitle="Ρύθμιση παραμέτρων συμβολοσειράς σύνδεσης με το χώρο αποθήκευσης Azure | Microsoft Azure"
    description="Ρυθμίστε τις παραμέτρους μιας συμβολοσειράς σύνδεσης σε ένα λογαριασμό Azure χώρου αποθήκευσης. Μια συμβολοσειρά σύνδεσης περιλαμβάνει τις πληροφορίες που απαιτούνται για τον έλεγχο ταυτότητας πρόσβασης σε λογαριασμό του χώρου αποθήκευσης από την εφαρμογή σας κατά το χρόνο εκτέλεσης."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="configure-azure-storage-connection-strings"></a>Ρύθμιση παραμέτρων συμβολοσειρές σύνδεσης Azure χώρου αποθήκευσης

## <a name="overview"></a>Επισκόπηση

Μια συμβολοσειρά σύνδεσης περιλαμβάνει τις πληροφορίες ελέγχου ταυτότητας που απαιτούνται για να αποκτήσετε πρόσβαση σε δεδομένα σε ένα λογαριασμό Azure χώρου αποθήκευσης από την εφαρμογή σας κατά το χρόνο εκτέλεσης. Μπορείτε να ρυθμίσετε μια συμβολοσειρά σύνδεσης για να:

- Σύνδεση με το προσομοίωσης Azure χώρου αποθήκευσης.
- Πρόσβαση σε έναν λογαριασμό χώρου αποθήκευσης στο Azure.
- Πρόσβαση σε καθορισμένο πόρους στο Azure μέσω υπογραφής κοινόχρηστη πρόσβαση (συσχετισμών Ασφαλείας).

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Αποθήκευση συμβολοσειρά σύνδεσης

Εφαρμογή σας θα πρέπει να αποκτήσετε πρόσβαση στη συμβολοσειρά σύνδεσης κατά το χρόνο εκτέλεσης προκειμένου να ελέγχουν την ταυτότητα αιτήσεις σε χώρο αποθήκευσης Azure. Έχετε ορισμένες διαφορετικές επιλογές για την αποθήκευση του συμβολοσειρά σύνδεσης:

- Για μια εφαρμογή εκτελείται στην επιφάνεια εργασίας ή σε μια συσκευή, μπορείτε να αποθηκεύσετε τη συμβολοσειρά σύνδεσης σε ένα `app.config `αρχείο ή μια `web.config` αρχείου. Προσθέστε τη συμβολοσειρά σύνδεσης με την ενότητα **AppSettings** .
- Για μια εφαρμογή που εκτελούνται σε μια υπηρεσία Azure cloud, μπορείτε να αποθηκεύσετε τη συμβολοσειρά σύνδεσης του [αρχείου σχήματος (.cscfg) ρύθμισης παραμέτρων Azure υπηρεσίας](https://msdn.microsoft.com/library/ee758710.aspx). Προσθέστε τη συμβολοσειρά σύνδεσης με την ενότητα **ConfigurationSettings** το αρχείο ρύθμισης παραμέτρων υπηρεσίας.
- Μπορείτε επίσης να χρησιμοποιήσετε τη συμβολοσειρά σύνδεσης σας απευθείας στον κώδικά σας. Για περισσότερα σενάρια, ωστόσο, συνιστάται να αποθηκεύσετε τη συμβολοσειρά ρύθμισης παραμέτρων σας σε ένα αρχείο ρύθμισης παραμέτρων.

Αποθήκευση συμβολοσειρά σύνδεσης μέσα σε ένα αρχείο ρύθμισης παραμέτρων διευκολύνει να ενημερώσετε τη συμβολοσειρά σύνδεσης για εναλλαγή μεταξύ του προσομοίωσης χώρου αποθήκευσης και ένα λογαριασμό Azure χώρου αποθήκευσης στο cloud. Μόνο πρέπει να επεξεργαστείτε τη συμβολοσειρά σύνδεσης, ώστε να οδηγούν στο περιβάλλον του προορισμού.

Μπορείτε να χρησιμοποιήσετε την κλάση [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) για να αποκτήσετε πρόσβαση σε συμβολοσειρά σύνδεσης στο περιβάλλον εκτέλεσης ανεξάρτητα από όπου εκτελείται η εφαρμογή σας.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Δημιουργία συμβολοσειράς σύνδεσης για το χώρο αποθήκευσης προσομοίωσης

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Για περισσότερες πληροφορίες σχετικά με το προσομοίωσης χώρου αποθήκευσης, ανατρέξτε στο θέμα [χρήση του Azure προσομοίωσης χώρου αποθήκευσης για ανάπτυξη και έλεγχος](storage-use-emulator.md) .

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Δημιουργία συμβολοσειράς σύνδεσης με ένα λογαριασμό Azure χώρου αποθήκευσης

Για να δημιουργήσετε μια συμβολοσειρά σύνδεσης στο λογαριασμό σας Azure χώρου αποθήκευσης, χρησιμοποιήστε την παρακάτω μορφή συμβολοσειρά σύνδεσης. Υποδεικνύει εάν θέλετε να συνδεθείτε με το λογαριασμό χώρου αποθήκευσης μέσω HTTPS (συνιστάται) ή HTTP, αντικαταστήστε `myAccountName` με το όνομα του λογαριασμού χώρου αποθήκευσης και αντικατάσταση `myAccountKey` με το κλειδί πρόσβασης του λογαριασμού σας:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

Για παράδειγμα, η συμβολοσειρά σύνδεσης θα είναι παρόμοιο με το παρακάτω δείγμα συμβολοσειρά σύνδεσης:

    DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Η αποθήκευση Azure υποστηρίζει HTTP και HTTPS σε μια συμβολοσειρά σύνδεσης. Ωστόσο, χρησιμοποιώντας το HTTPS συνιστούμε.

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Δημιουργία συμβολοσειράς σύνδεσης με μια κοινόχρηστη πρόσβαση υπογραφή

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Δημιουργία μιας συμβολοσειράς σύνδεσης σε ένα τελικό σημείο ρητή χώρου αποθήκευσης

Μπορείτε να καθορίσετε τα τελικά σημεία υπηρεσίας ρητά στη συμβολοσειρά σύνδεσης αντί να χρησιμοποιήσετε τα τελικά σημεία προεπιλογή. Για να δημιουργήσετε μια συμβολοσειρά σύνδεσης που καθορίζει ρητή τελικού σημείου, καθορίστε το τελικό σημείο ολοκλήρωσης υπηρεσίας για κάθε υπηρεσία, συμπεριλαμβανομένης της προδιαγραφής protocol (HTTPS (συνιστάται) ή HTTP), με την εξής μορφή:

    DefaultEndpointsProtocol=[http|https];
    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    AccountName=myAccountName;
    AccountKey=myAccountKey

Ένα σενάριο όπου μπορεί να θέλετε να καθορίσετε ρητή τελικού σημείου είναι εάν έχετε αντιστοιχίσει του ορίου χώρου αποθήκευσης αντικειμένων Blob σε προσαρμοσμένο τομέα. Σε αυτή την περίπτωση, μπορείτε να καθορίσετε το προσαρμοσμένο τελικό σημείο για το χώρο αποθήκευσης αντικειμένων Blob στη συμβολοσειρά σύνδεσης και προαιρετικά, καθορίστε τα τελικά σημεία προεπιλογή για την άλλη υπηρεσία εάν η εφαρμογή σας χρησιμοποιεί τους.

Ακολουθούν παραδείγματα συμβολοσειρών έγκυρη σύνδεση που καθορίζουν ρητή τελικού σημείου για την υπηρεσία Blob:

    # Blob endpoint only
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    AccountName=storagesample;
    AccountKey=account-key

    # All service endpoints
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    FileEndpoint=myaccount.file.core.windows.net;
    QueueEndpoint=myaccount.queue.core.windows.net;
    TableEndpoint=myaccount;
    AccountName=storagesample;
    AccountKey=account-key

Χρησιμοποιείται η τιμή τελικού σημείου που παρατίθεται στη συμβολοσειρά σύνδεσης για να δημιουργήσετε την αίτηση URIs στην υπηρεσία Blob και το Καθορίζει τη μορφή οποιαδήποτε URI που επιστρέφονται για τον κωδικό.

Λάβετε υπόψη ότι εάν επιλέξετε να παραλείψετε ένα τελικό σημείο υπηρεσίας από τη συμβολοσειρά σύνδεσης, στη συνέχεια, δεν θα μπορείτε να χρησιμοποιήσετε τη συμβολοσειρά σύνδεσης για να αποκτήσετε πρόσβαση σε δεδομένα σε αυτήν την υπηρεσία από τον κωδικό.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Δημιουργία συμβολοσειράς σύνδεσης με ένα τελικό σημείο επίθημα

Για να δημιουργήσετε μια συμβολοσειρά σύνδεσης για την υπηρεσία αποθήκευσης σε περιοχές ή παρουσίες με διαφορετικό τελικού σημείου προθέματα, όπως Κίνα Azure ή Azure διακυβέρνησης, χρησιμοποιήστε την εξής μορφή συμβολοσειρά σύνδεσης. Υποδεικνύει εάν θέλετε να συνδεθείτε με το λογαριασμό χώρου αποθήκευσης μέσω HTTP ή HTTPS, αντικαταστήστε `myAccountName` με το όνομα του λογαριασμού σας χώρου αποθήκευσης, αντικαταστήστε `myAccountKey` με το πλήκτρο πρόσβασης λογαριασμού και αντικατάσταση `mySuffix` με το επίθημα URI:


    DefaultEndpointsProtocol=[http|https];
    AccountName=myAccountName;
    AccountKey=myAccountKey;
    EndpointSuffix=mySuffix;


Για παράδειγμα, η συμβολοσειρά σύνδεσης πρέπει να μοιάζει με την ακόλουθη συμβολοσειρά σύνδεσης:

    DefaultEndpointsProtocol=https;
    AccountName=storagesample;
    AccountKey=<account-key>;
    EndpointSuffix=core.chinacloudapi.cn;

## <a name="parsing-a-connection-string"></a>Κατά την ανάλυση συμβολοσειράς σύνδεσης

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## <a name="next-steps"></a>Επόμενα βήματα

- [Χρησιμοποιήστε το προσομοίωσης Azure χώρου αποθήκευσης για ανάπτυξη και δοκιμές](storage-use-emulator.md)
- [Εξερεύνησης Azure χώρου αποθήκευσης](storage-explorers.md)
- [Χρήση υπογραφών κοινόχρηστη πρόσβαση (συσχετισμών Ασφαλείας)](storage-dotnet-shared-access-signature-part-1.md)
