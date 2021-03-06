<properties
   pageTitle="Επισκόπηση της ρύθμισης παραμέτρων του Azure Service ύφασμα αξιόπιστη ReliableDictionaryActorStateProvider ηθοποιών | Microsoft Azure"
   description="Μάθετε περισσότερα σχετικά με τη ρύθμιση των παραμέτρων Azure Service υφάσματος με κατάσταση ηθοποιών του τύπου ReliableDictionaryActorStateProvider."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/18/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Ρύθμιση παραμέτρων αξιόπιστων ηθοποιών--ReliableDictionaryActorStateProvider
Μπορείτε να τροποποιήσετε την προεπιλεγμένη ρύθμιση παραμέτρων του ReliableDictionaryActorStateProvider, αλλάζοντας το αρχείο settings.xml που δημιουργήθηκε στον ριζικό κατάλογο του πακέτου Visual Studio κάτω από το φάκελο ρύθμισης παραμέτρων για το καθορισμένο παράγοντα.

Το περιβάλλον εκτέλεσης Azure Service ύφασμα αναζητά ονόματα προκαθορισμένες ενοτήτων στο αρχείο settings.xml και χρησιμοποιεί τις τιμές παραμέτρων κατά τη δημιουργία τα υποκείμενα στοιχεία χρόνου εκτέλεσης.

>[AZURE.NOTE] Να **μην** διαγράψετε ή να τροποποιήσετε τα ονόματα ενότητας από τις ακόλουθες ρυθμίσεις παραμέτρων στο αρχείο settings.xml που δημιουργείται στη λύση Visual Studio.

Υπάρχουν επίσης καθολικές ρυθμίσεις που επηρεάζουν τις παραμέτρους της ReliableDictionaryActorStateProvider.

## <a name="global-configuration"></a>Καθολικές ρυθμίσεις

Τις καθολικές ρυθμίσεις έχει καθοριστεί στο σύμπλεγμα δηλωτικό για το σύμπλεγμα κάτω από την ενότητα KtlLogger. Το επιτρέπει τη ρύθμιση παραμέτρων της τη θέση του κοινόχρηστου καταγραφής και μέγεθος μαζί με τα όρια καθολική μνήμη που χρησιμοποιείται από το πρόγραμμα καταγραφής. Σημειώστε ότι οι αλλαγές στο σύμπλεγμα δηλωτικό επηρεάζουν όλες τις υπηρεσίες που χρησιμοποιούν ReliableDictionaryActorStateProvider και αξιόπιστη κατάστασης υπηρεσίες.

Το δηλωτικό σύμπλεγμα είναι ένα αρχείο XML που περιέχει τις ρυθμίσεις και ρυθμίσεις παραμέτρων που ισχύουν για όλους τους κόμβους και υπηρεσίες στο σύμπλεγμα. Το αρχείο συνήθως ονομάζεται ClusterManifest.xml. Μπορείτε να δείτε το σύμπλεγμα δήλωσης για το σύμπλεγμά σας χρησιμοποιώντας την εντολή Get-ServiceFabricClusterManifest powershell.

### <a name="configuration-names"></a>Ονόματα παραμέτρων

|Όνομα|Μονάδα|Προεπιλεγμένη τιμή|Παρατηρήσεις|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|ΚΒ|8388608|Ελάχιστος αριθμός KB θα εκχωρήσετε σε λειτουργία πυρήνα για το χώρο συγκέντρωσης μνήμης buffer καταγραφής εγγραφής. Αυτός ο χώρος συγκέντρωσης μνήμης χρησιμοποιείται για προσωρινή αποθήκευση πληροφοριών κατάστασης πριν από την εγγραφή σε δίσκο.|
|WriteBufferMemoryPoolMaximumInKB|ΚΒ|Χωρίς όριο|Μέγιστο μέγεθος στο οποίο το πρόγραμμα καταγραφής γράψετε χώρου συγκέντρωσης buffer μνήμης μπορούν να αναπτυχθούν.|
|SharedLogId|GUID|""|Καθορίζει ένα μοναδικό GUID για τον εντοπισμό το προεπιλεγμένο κοινόχρηστο αρχείο καταγραφής χρησιμοποιείται από όλες τις αξιόπιστες υπηρεσίες σε όλους τους κόμβους του συμπλέγματος που δεν καθορίσετε το SharedLogId στο τους συγκεκριμένη ρύθμιση παραμέτρων της υπηρεσίας. Εάν έχει καθοριστεί SharedLogId, στη συνέχεια, SharedLogPath πρέπει επίσης να καθοριστεί.|
|SharedLogPath|Πλήρες όνομα διαδρομής|""|Καθορίζει την πλήρη διαδρομή όπου το αρχείο καταγραφής κοινόχρηστο χρησιμοποιούνται από όλες τις αξιόπιστες υπηρεσίες σε όλους τους κόμβους του συμπλέγματος που δεν καθορίσετε το SharedLogPath στο τους συγκεκριμένη ρύθμιση παραμέτρων της υπηρεσίας. Ωστόσο, εάν έχει καθοριστεί SharedLogPath, στη συνέχεια, SharedLogId πρέπει επίσης να καθοριστεί.|
|SharedLogSizeInMB|Megabyte που|8192|Καθορίζει τον αριθμό των MB χώρου στο δίσκο για να δεσμεύσετε στατικά για το κοινόχρηστο αρχείο καταγραφής. Η τιμή πρέπει να είναι 2048 ή μεγαλύτερη.|

### <a name="sample-cluster-manifest-section"></a>Ενότητα δηλώσεων σύμπλεγμα δείγμα
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Παρατηρήσεις
Στο πρόγραμμα καταγραφής έχει ενός καθολικού χώρου συγκέντρωσης μνήμης που έχει εκχωρηθεί από μνήμη μη σελιδοποιημένος πυρήνα που είναι διαθέσιμες σε όλες τις υπηρεσίες αξιόπιστη σε έναν κόμβο για προσωρινή αποθήκευση δεδομένων κατάστασης πριν από την εγγραφή στο αρχείο καταγραφής αποκλειστικό που σχετίζεται με στη ρεπλίκα αξιόπιστη υπηρεσία. Το μέγεθος του χώρου συγκέντρωσης ελέγχεται από τις ρυθμίσεις WriteBufferMemoryPoolMinimumInKB και WriteBufferMemoryPoolMaximumInKB. WriteBufferMemoryPoolMinimumInKB Καθορίζει το αρχικό μέγεθος αυτής της περιοχής μνήμης και το μικρότερο μέγεθος στις οποίες ενδέχεται να σμικρύνετε το χώρο συγκέντρωσης μνήμης. WriteBufferMemoryPoolMaximumInKB είναι το μεγαλύτερο μέγεθος στο οποίο μπορεί να αναπτυχθεί το χώρο συγκέντρωσης μνήμης. Κάθε ρεπλίκα αξιόπιστη υπηρεσία που ανοίγει μπορεί να αύξηση του μεγέθους του χώρου συγκέντρωσης μνήμης κατά ένα ποσό συστήματος καθορίζεται έως WriteBufferMemoryPoolMaximumInKB. Εάν υπάρχουν περισσότερες ζήτηση για μνήμης από το χώρο συγκέντρωσης μνήμη από όση είναι διαθέσιμη, αιτήσεις για μνήμη θα να καθυστερήσει μέχρι να υπάρχει διαθέσιμη μνήμη. Επομένως, εάν το χώρο συγκέντρωσης εγγραφής buffer μνήμης είναι πολύ μικρό για μια συγκεκριμένη ρύθμιση παραμέτρων, στη συνέχεια, επιδόσεων μπορεί να υποστεί.

Οι ρυθμίσεις SharedLogId και SharedLogPath χρησιμοποιούνται πάντα μαζί, για να ορίσετε το GUID και τη θέση για το κοινόχρηστο αρχείο καταγραφής προεπιλογή για όλους τους κόμβους του συμπλέγματος. Το προεπιλεγμένο κοινόχρηστο αρχείο καταγραφής χρησιμοποιείται για όλες τις αξιόπιστες υπηρεσίες που δεν καθορίσετε τις ρυθμίσεις στο το settings.xml για τη συγκεκριμένη υπηρεσία. Για καλύτερες επιδόσεις, πρέπει να τοποθετηθούν κοινόχρηστα αρχεία καταγραφής δίσκων που χρησιμοποιούνται μόνο για το αρχείο καταγραφής κοινόχρηστο για να μειώσετε διένεξης.

SharedLogSizeInMB Καθορίζει την ποσότητα χώρου στο δίσκο σε εκ των προτέρων εκχώρηση για το προεπιλεγμένο κοινόχρηστο αρχείο καταγραφής σε όλους τους κόμβους.  SharedLogId και SharedLogPath δεν χρειάζεται να έχει καθοριστεί για SharedLogSizeInMB να έχει καθοριστεί.

## <a name="replicator-security-configuration"></a>Ρύθμιση παραμέτρων ασφαλείας προγράμματος αναπαραγωγής
Ρύθμιση παραμέτρων ασφαλείας του προγράμματος αναπαραγωγής που χρησιμοποιούνται για την ασφάλιση το κανάλι επικοινωνίας που χρησιμοποιείται κατά την αναπαραγωγή. Αυτό σημαίνει ότι οι υπηρεσίες δεν είναι δυνατό να βλέπουν του άλλου κίνησης αναπαραγωγής, ταυτόχρονα εξασφαλίζετε ότι τα δεδομένα που είναι ιδιαίτερα επίσης είναι ασφαλή.
Από προεπιλογή, μια ενότητα ρύθμισης παραμέτρων κενό ασφαλείας αποτρέπει την ασφάλεια αναπαραγωγής.

### <a name="section-name"></a>Όνομα της ενότητας
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Ρύθμιση παραμέτρων του προγράμματος αναπαραγωγής
Ρυθμίσεις παραμέτρων προγράμματος αναπαραγωγής που χρησιμοποιούνται για να ρυθμίσετε το πρόγραμμα αναπαραγωγής που είναι υπεύθυνος για την πραγματοποίηση της κατάστασης υπηρεσίας παροχής κατάσταση παράγοντα ιδιαίτερα αξιόπιστη, αναπαραγωγή και συνεχίζεται η κατάσταση τοπικά.
Η προεπιλεγμένη ρύθμιση παραμέτρων δημιουργείται από το πρότυπο Visual Studio και θα πρέπει να αρκεί. Αυτή η ενότητα ακρόασης σχετικά με πρόσθετες ρυθμίσεις που είναι διαθέσιμες για να ρυθμίσετε το πρόγραμμα αναπαραγωγής.

### <a name="section-name"></a>Όνομα της ενότητας
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Ονόματα παραμέτρων

|Όνομα|Μονάδα|Προεπιλεγμένη τιμή|Παρατηρήσεις|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Δευτερόλεπτα|0.015|Για τον οποίο το πρόγραμμα αναπαραγωγής κατά την αναμονή δευτερεύοντα μετά τη λήψη μιας λειτουργίας πριν από την αποστολή επιστροφή μια βεβαίωση λήψης των κύριων χρονική περίοδο. Τυχόν άλλες επιβεβαίωση να αποστέλλονται για λειτουργίες υποβάλλονται σε επεξεργασία μέσα σε αυτό το χρονικό διάστημα αποστέλλονται ως μία απάντηση.||
|ReplicatorEndpoint|Δ/Υ|Δεν υπάρχει προεπιλεγμένη--απαιτούμενη παράμετρος|Ορίστε τη διεύθυνση IP και τη θύρα που θα χρησιμοποιήσετε το πρόγραμμα αναπαραγωγής πρωτεύον/δευτερεύον για να επικοινωνήσετε με άλλες αναπαραγωγείς στη ρεπλίκα. Αυτό θα πρέπει να αναφέρονται σε ένα τελικό σημείο πόρων TCP στο δηλωτικό υπηρεσίας. Αναφορά σε [υπηρεσία δήλωσης πόρους](service-fabric-service-manifest-resources.md) για περισσότερες πληροφορίες σχετικά με τον ορισμό πόρους τελικού σημείου στο δηλωτικό υπηρεσίας. |
|MaxReplicationMessageSize|Byte που|50 MB|Μέγιστο μέγεθος των δεδομένων αναπαραγωγής που μπορεί να μεταφερθεί σε ένα μήνυμα.|
|MaxPrimaryReplicationQueueSize|Αριθμός των λειτουργιών|8192|Μέγιστος αριθμός των λειτουργιών στην κύρια ουρά. Μια λειτουργία ελευθερωθεί μετά το πρωτεύον πρόγραμμα αναπαραγωγής λαμβάνει μια βεβαίωση λήψης από όλα τα δευτερεύοντα αναπαραγωγείς. Αυτή η τιμή πρέπει να είναι μεγαλύτερη από 64 και σε δύναμη του 2.|
|MaxSecondaryReplicationQueueSize|Αριθμός των λειτουργιών|16384|Μέγιστος αριθμός των λειτουργιών στην ουρά δευτερεύοντα. Μια λειτουργία ελευθερωθεί μετά την πραγματοποίηση ιδιαίτερα διαθέσιμες μέσω διατήρησης στην κατάσταση. Αυτή η τιμή πρέπει να είναι μεγαλύτερη από 64 και σε δύναμη του 2.|
|CheckpointThresholdInMB|MB|200|Μέγεθος του χώρου αρχείου καταγραφής μετά από την οποία η κατάσταση είναι checkpointed.|
|MaxRecordSizeInKB|KB|1024|Μεγαλύτερο μέγεθος εγγραφής που μπορεί να γράψετε το πρόγραμμα αναπαραγωγής στο αρχείο καταγραφής. Αυτή η τιμή πρέπει να είναι πολλαπλάσιο του 4 και μεγαλύτερο από 16.|
|OptimizeLogForLowerDiskUsage|Δυαδική τιμή|TRUE|Όταν είναι true, το αρχείο καταγραφής έχει ρυθμιστεί ώστε να δημιουργείται στη ρεπλίκα αποκλειστικό αρχείο καταγραφής χρησιμοποιώντας ένα αρχείο κατακερματισμένο NTFS. Αυτό μειώνει το χρήση χώρου στο δίσκο πραγματική για το αρχείο. Όταν είναι false, το αρχείο δημιουργείται με σταθερό εκχωρήσεις, που παρέχουν τις καλύτερες γράψετε επιδόσεων.|
|SharedLogId|GUID|""|Καθορίζει ένα μοναδικό guid για τον εντοπισμό το αρχείο καταγραφής κοινόχρηστο που χρησιμοποιείται με αυτήν τη ρεπλίκα. Συνήθως, υπηρεσίες δεν πρέπει να χρησιμοποιήσετε αυτήν τη ρύθμιση. Ωστόσο, εάν έχει καθοριστεί SharedLogId, στη συνέχεια, SharedLogPath πρέπει επίσης να καθοριστεί.|
|SharedLogPath|Πλήρες όνομα διαδρομής|""|Καθορίζει την πλήρη διαδρομή όπου θα δημιουργηθεί το αρχείο καταγραφής κοινόχρηστο για αυτήν τη ρεπλίκα. Συνήθως, υπηρεσίες δεν πρέπει να χρησιμοποιήσετε αυτήν τη ρύθμιση. Ωστόσο, εάν έχει καθοριστεί SharedLogPath, στη συνέχεια, SharedLogId πρέπει επίσης να καθοριστεί.|


## <a name="sample-configuration-file"></a>Δείγμα αρχείου ρύθμισης παραμέτρων

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```

## <a name="remarks"></a>Παρατηρήσεις
Η παράμετρος BatchAcknowledgementInterval ελέγχει λανθάνοντος χρόνου αναπαραγωγής. Η τιμή "0" ως αποτέλεσμα την χαμηλότερη των πιθανών λανθάνων χρόνος, εις βάρος της μετάδοσης (όπως περισσότερα μηνύματα αναγνώρισης πρέπει να αποστέλλονται και να υποβάλλονται σε επεξεργασία, λιγότερες επιβεβαίωση που περιέχουν).
Τόσο μεγαλύτερη η τιμή BatchAcknowledgementInterval, υψηλή τη συνολική αναπαραγωγής μεταγωγή, εις βάρος της υψηλότερα λανθάνων χρόνος λειτουργίας. Απευθείας μεταφράζεται σε η αδράνεια των συναλλαγής.

Η παράμετρος CheckpointThresholdInMB ελέγχει την ποσότητα του χώρου που να χρησιμοποιήσετε το πρόγραμμα αναπαραγωγής για να αποθηκεύσετε πληροφορίες κατάστασης στο αρχείο καταγραφής αποκλειστικό στη ρεπλίκα. Αύξηση αυτό σε υψηλότερη τιμή από την προεπιλεγμένη θα μπορούσε να έχει ως αποτέλεσμα ταχύτερους χρόνους αλλαγή των ρυθμίσεων παραμέτρων όταν προστίθεται μια νέα ρεπλίκα στο σύνολο. Αυτό οφείλεται τη μεταφορά μερική κατάσταση που τίθεται σε ισχύ λόγω τη διαθεσιμότητα των περισσότερων το ιστορικό των λειτουργιών στο αρχείο καταγραφής. Αυτό ενδεχομένως να αυξήσετε τον χρόνο αποκατάστασης από μια ρεπλίκα μετά από ένα σφάλμα.

Εάν ορίσετε OptimizeForLowerDiskUsage την τιμή true, ο χώρος αρχείου καταγραφής θα υπερ-προμήθεια του φακέλου ώστε να ενεργό αντίγραφα μπορούν να αποθηκεύουν περισσότερες πληροφορίες κατάστασης στα αρχεία καταγραφής τους, ενώ ανενεργός αντίγραφα θα χρησιμοποιήσει λιγότερο χώρο στο δίσκο. Αυτό δίνει τη δυνατότητα να φιλοξενήσετε περισσότερες αντίγραφα από έναν κόμβο. Εάν ορίσετε OptimizeForLowerDiskUsage FALSE (ψευδής), οι πληροφορίες κατάστασης είναι γραμμένο στα αρχεία καταγραφής πιο γρήγορα.

Η ρύθμιση MaxRecordSizeInKB Καθορίζει το μέγιστο μέγεθος μιας εγγραφής που μπορούν να εγγραφούν από το πρόγραμμα αναπαραγωγής στο αρχείο καταγραφής. Στις περισσότερες περιπτώσεις, το προεπιλεγμένο μέγεθος 1024-KB εγγραφή είναι η βέλτιστη. Ωστόσο, εάν η υπηρεσία προκαλεί μεγαλύτερο στοιχεία δεδομένων για να είναι μέρος του οι πληροφορίες κατάστασης, στη συνέχεια, αυτή η τιμή ίσως χρειαστεί να αυξάνεται. Υπάρχει λίγο όφελος στην MaxRecordSizeInKB μικρότερο από 1024, καθώς μικρότερο εγγραφές χρησιμοποιεί μόνο τον απαιτούμενο χώρο για την εγγραφή μικρότερο. Αναμένεται ότι αυτή η τιμή θα πρέπει να αλλάξουν μόνο στην περίπτωση αυτή.

Οι ρυθμίσεις SharedLogId και SharedLogPath χρησιμοποιούνται πάντα μαζί, για να κάνετε μια υπηρεσία, χρησιμοποιήστε ένα ξεχωριστό αρχείο καταγραφής κοινόχρηστο από το κοινόχρηστο αρχείο καταγραφής προεπιλογή για τον κόμβο. Για βέλτιστη απόδοση, όσο το δυνατόν περισσότερες υπηρεσίες όσο το δυνατόν θα πρέπει να καθορίσετε το ίδιο κοινόχρηστο αρχείο καταγραφής. Κοινόχρηστα αρχεία καταγραφής θα πρέπει να τεθούν σε δίσκων που χρησιμοποιούνται μόνο για το αρχείο καταγραφής κοινόχρηστο, για να μειώσετε ασυμφωνίας κεφαλής κίνηση. Αναμένεται ότι αυτές οι τιμές θα πρέπει να αλλάξουν μόνο στην περίπτωση αυτή.
