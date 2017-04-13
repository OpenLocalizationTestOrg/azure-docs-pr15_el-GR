<properties
   pageTitle="Χρήση μετρητών επιδόσεων στο Azure Διαγνωστικά | Microsoft Azure"
   description="Χρησιμοποιήστε μετρητές επιδόσεων σε υπηρεσίες Azure cloud ή εικονική μηχανή για να βρείτε συνωστισμών και με ακρίβεια τις επιδόσεις."
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Δημιουργία και χρήση μετρητών επιδόσεων σε μια εφαρμογή του Azure

Σε αυτό το άρθρο περιγράφει τα πλεονεκτήματα της και πώς μπορείτε να θέσετε μετρητές επιδόσεων Azure εφαρμογή σας. Μπορείτε να τα χρησιμοποιήσετε για συλλογή δεδομένων και Εύρεση συνωστισμών ακρίβεια τις επιδόσεις του συστήματος και των εφαρμογών.

Μετρητές επιδόσεων που είναι διαθέσιμοι για Windows Server, των υπηρεσιών IIS και ASP.NET μπορεί να είναι επίσης που συλλέγονται και χρησιμοποιείται για τον προσδιορισμό της εύρυθμης λειτουργίας των ρόλων Azure web, εργαζόμενου ρόλους και εικονικές μηχανές. Μπορείτε επίσης να δημιουργήσετε και να χρησιμοποιήστε μετρητές προσαρμοσμένο επιδόσεων.  

Μπορείτε να εξετάσετε τα δεδομένα του μετρητή επιδόσεων
1. Απευθείας στον κεντρικό υπολογιστή εφαρμογή με το εργαλείο Εποπτεία επιδόσεων πρόσβαση χρησιμοποιώντας απομακρυσμένης επιφάνειας εργασίας
2. Με το σύστημα Center Operations Manager με χρήση του πακέτου διαχείρισης Azure
3. Με άλλα εργαλεία παρακολούθησης που έχει πρόσβαση στα δεδομένα διαγνωστικών μεταφερθεί Azure χώρου αποθήκευσης. Ανατρέξτε στο θέμα [Αποθήκευση και προβολή διαγνωστικών δεδομένων στο χώρο αποθήκευσης Azure](https://msdn.microsoft.com/library/azure/hh411534.aspx) για περισσότερες πληροφορίες.  

Για περισσότερες πληροφορίες σχετικά με την παρακολούθηση των επιδόσεων της εφαρμογής σας στην [πύλη του Azure κλασική](http://manage.azure.com/), δείτε [πώς μπορείτε να τις υπηρεσίες Cloud της οθόνης](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Για πρόσθετες λεπτομερείς οδηγίες σχετικά με τη δημιουργία μια καταγραφή και παρακολούθηση στρατηγικής και χρήση εργαλεία διαγνωστικών και άλλες τεχνικές για την αντιμετώπιση προβλημάτων και βελτιστοποίηση Azure εφαρμογές, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων βέλτιστες πρακτικές για την ανάπτυξη εφαρμογών Azure](https://msdn.microsoft.com/library/azure/hh771389.aspx).


## <a name="enable-performance-counter-monitoring"></a>Ενεργοποίηση παρακολούθησης μετρητή επιδόσεων

Μετρητές επιδόσεων δεν είναι ενεργοποιημένες από προεπιλογή. Η εφαρμογή σας ή σε μια εργασία εκκίνησης πρέπει να τροποποιήσετε την προεπιλεγμένη ρύθμιση παραμέτρων παράγοντα Διαγνωστικά για να συμπεριλάβετε το συγκεκριμένους μετρητές επιδόσεων που θέλετε να παρακολουθείτε για κάθε παρουσία ρόλο.

### <a name="performance-counters-available-for-microsoft-azure"></a>Μετρητές επιδόσεων που είναι διαθέσιμοι για το Microsoft Azure

Azure παρέχει ένα υποσύνολο των μετρητών επιδόσεων διαθέσιμη για Windows Server, των υπηρεσιών IIS και στη στοίβα ASP.NET. Ο παρακάτω πίνακας παραθέτει ορισμένα από τα μετρητές επιδόσεων συγκεκριμένο ενδιαφέροντος για τις εφαρμογές του Azure.

|Μετρητής κατηγορία: Αντικείμενο (παρουσία)|Όνομα μετρητή      |Αναφορά|
|---|---|---|
|.NET CLR εξαιρέσεις (_καθολική_)|# Exceps δημιουργήθηκε / δευτ.   |Εξαίρεση μετρητές επιδόσεων|
|.NET CLR μνήμης (_καθολική_)    |% Χρόνου στο καθολικός Κατάλογος            |Μετρητές επιδόσεων μνήμης|
|ASP.NET                      |Επανεκκίνηση εφαρμογής    |Μετρητές επιδόσεων για το ASP.NET|
|ASP.NET                      |Χρόνος εκτέλεσης αίτησης  |Μετρητές επιδόσεων για το ASP.NET|
|ASP.NET                      |Αιτήσεις αποσύνδεση   |Μετρητές επιδόσεων για το ASP.NET|
|ASP.NET                      |Επανεκκίνηση του διεργασίας εργασίας |Μετρητές επιδόσεων για το ASP.NET|
|Εφαρμογές ASP.NET (__συνολικό__)|Σύνολο αιτήσεων        |Μετρητές επιδόσεων για το ASP.NET|
|Εφαρμογές ASP.NET (__συνολικό__)|Αιτήσεις/δευτερόλεπτο          |Μετρητές επιδόσεων για το ASP.NET|
|ASP.NET v4.0.30319           |Χρόνος εκτέλεσης αίτησης  |Μετρητές επιδόσεων για το ASP.NET|
|ASP.NET v4.0.30319           |Χρόνος αναμονής αίτησης       |Μετρητές επιδόσεων για το ASP.NET|
|ASP.NET v4.0.30319           |Τρέχουσα αιτήσεις        |Μετρητές επιδόσεων για το ASP.NET|
|ASP.NET v4.0.30319           |Αιτήσεις σε ουρά         |Μετρητές επιδόσεων για το ASP.NET|
|ASP.NET v4.0.30319           |Αιτήσεις απορρίφθηκε       |Μετρητές επιδόσεων για το ASP.NET|
|Μνήμη                       |Διαθέσιμα MB        |Μετρητές επιδόσεων μνήμης|
|Μνήμη                       |Δεσμευμένα byte         |Μετρητές επιδόσεων μνήμης|
|Επεξεργαστής(_Σύνολο)            |% Χρόνος επεξεργασίας        |Μετρητές επιδόσεων για το ASP.NET|
|TCPv4                        |Αποτυχίες σύνδεσης     |Αντικείμενο TCP|
|TCPv4                        |Συνδέσεις σε εξέλιξη |Αντικείμενο TCP|
|TCPv4                        |Επαναφορά συνδέσεων       |Αντικείμενο TCP|
|TCPv4                        |Τμήματα αποστολής/δευτερόλεπτο       |Αντικείμενο TCP|
|Interface(*) δικτύου         |Byte λήψης/δευτερόλεπτο      |Αντικείμενο περιβάλλοντος εργασίας δικτύου|
|Interface(*) δικτύου         |Byte αποστολής/δευτερόλεπτο          |Αντικείμενο περιβάλλοντος εργασίας δικτύου|
|Δικτύου του περιβάλλοντος εργασίας (_2 προσαρμογέα δικτύου του Microsoft εικονική μηχανή Bus)|Byte λήψης/δευτερόλεπτο|Αντικείμενο περιβάλλοντος εργασίας δικτύου|
|Δικτύου του περιβάλλοντος εργασίας (_2 προσαρμογέα δικτύου του Microsoft εικονική μηχανή Bus)|Byte αποστολής/δευτερόλεπτο|Αντικείμενο περιβάλλοντος εργασίας δικτύου|
|Δικτύου του περιβάλλοντος εργασίας (_2 προσαρμογέα δικτύου του Microsoft εικονική μηχανή Bus)|Σύνολο byte/δευτερόλεπτο|Αντικείμενο περιβάλλοντος εργασίας δικτύου|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Δημιουργία και προσθήκη προσαρμοσμένου μετρητών στην εφαρμογή σας

Azure έχει υποστήριξη για να δημιουργήσετε και να τροποποιήσετε μετρητές προσαρμοσμένο επιδόσεων για ρόλους web και εργασίας. Οι μετρητές μπορεί να χρησιμοποιηθεί για την παρακολούθηση και την παρακολούθηση της συμπεριφοράς συγκεκριμένη εφαρμογή. Μπορείτε να δημιουργήσετε και να Διαγραφή κατηγοριών μετρητών προσαρμοσμένο επιδόσεων και προσδιοριστικά από μια εργασία εκκίνησης, ρόλου web ή ρόλο εργαζόμενου με αναβαθμισμένα δικαιώματα.

>[AZURE.NOTE] Κώδικα που να κάνετε αλλαγές σε προσαρμοσμένο μετρητών πρέπει να έχετε αναβαθμισμένα δικαιώματα για να εκτελέσετε. Εάν ο κώδικας είναι σε ένα ρόλο web ή ρόλο εργαζόμενου, ο ρόλος πρέπει να περιλαμβάνει την ετικέτα <Runtime executionContext="elevated" /> στο αρχείο ServiceDefinition.csdef για το ρόλο να προετοιμαστεί σωστά.

Μπορείτε να στείλετε δεδομένα του μετρητή επιδόσεων προσαρμοσμένη με το χώρο αποθήκευσης Azure χρησιμοποιώντας τον παράγοντα Διαγνωστικά.

Τα δεδομένα του μετρητή επιδόσεων τυπική δημιουργείται από το Azure διεργασίες. Δεδομένα του μετρητή επιδόσεων προσαρμοσμένο πρέπει να δημιουργηθεί από την εφαρμογή web ρόλο ή εργαζόμενου του ρόλου. Για πληροφορίες σχετικά με τους τύπους δεδομένων που μπορούν να αποθηκευτούν στη λίστα μετρητές προσαρμοσμένο επιδόσεων, ανατρέξτε στο θέμα [Τύποι μετρητή επιδόσεων](https://msdn.microsoft.com/library/z573042h.aspx) . Ανατρέξτε στο θέμα [PerformanceCounters δείγμα](http://code.msdn.microsoft.com/azure/) για ένα παράδειγμα το οποίο δημιουργεί και σύνολα δεδομένων μετρητή επιδόσεων προσαρμοσμένο σε ένα ρόλο web.

## <a name="store-and-view-performance-counter-data"></a>Αποθήκευση και προβολή δεδομένων μετρητή επιδόσεων

Azure αποθηκεύει προσωρινά δεδομένα του μετρητή επιδόσεων με άλλες πληροφορίες διαγνωστικών. Αυτά τα δεδομένα είναι διαθέσιμη για απομακρυσμένη παρακολούθηση ενώ εκτελείται η παρουσία ρόλων με πρόσβαση απομακρυσμένης επιφάνειας εργασίας για να δείτε εργαλεία όπως Εποπτεία επιδόσεων. Για να διατηρούνται τα δεδομένα εκτός της παρουσίας του ρόλου, τον παράγοντα Διαγνωστικά πρέπει να μεταφέρετε τα δεδομένα του Azure χώρου αποθήκευσης. Το όριο μεγέθους των δεδομένων στο cache επιδόσεων μετρητή μπορούν να ρυθμιστούν στον παράγοντα Διαγνωστικά ή μπορεί να ρυθμιστεί να αποτελεί μέρος ενός κοινόχρηστου όριο για όλα τα διαγνωστικά δεδομένα. Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση του μεγέθους του buffer, ανατρέξτε στο θέμα [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) και [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Ανατρέξτε στο θέμα [Αποθήκευση και προβολή διαγνωστικών δεδομένων στο χώρο αποθήκευσης Azure](https://msdn.microsoft.com/library/azure/hh411534.aspx) για μια επισκόπηση της ρύθμιση τον παράγοντα Διαγνωστικά για να μεταφέρετε δεδομένα σε ένα λογαριασμό του χώρου αποθήκευσης.

Κάθε παρουσίας του μετρητή ρυθμισμένο επιδόσεων έχει καταγραφεί στο καθορισμένο δειγματοληψία χρέωσης και τα δεδομένα του δείγματος μεταφέρεται με το λογαριασμό χώρου αποθήκευσης με μια αίτηση μεταφοράς προγραμματισμένη ή μια αίτηση μεταφοράς στη ζήτηση. Αυτόματη μεταφορές μπορούν να προγραμματιστούν όσο συχνά μία φορά το λεπτό. Μεταφορά από τον παράγοντα διαγνωστικά δεδομένα του μετρητή επιδόσεων είναι αποθηκευμένο σε έναν πίνακα, WADPerformanceCountersTable, στο λογαριασμό χώρου αποθήκευσης. Αυτός ο πίνακας μπορεί να είναι προσβάσιμη και ερώτημα με τυπική Azure αποθήκευσης API μεθόδους. Ανατρέξτε στο θέμα [Microsoft Azure PerformanceCounters δείγμα](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) για παράδειγμα υποβολή ερωτημάτων και εμφανίζει δεδομένα του μετρητή επιδόσεων από τον πίνακα WADPerformanceCountersTable.

>[AZURE.NOTE] Ανάλογα με τη συχνότητα μεταφοράς παράγοντας Διαγνωστικά και λανθάνων χρόνος ουρά, τα πιο πρόσφατα δεδομένα μετρητή επιδόσεων στο λογαριασμό χώρου αποθήκευσης ενδέχεται να είναι αρκετά λεπτά λήξει.

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Ενεργοποίηση μετρητών επιδόσεων χρησιμοποιώντας τα Διαγνωστικά αρχείο ρύθμισης παραμέτρων

Χρησιμοποιήστε την ακόλουθη διαδικασία για να ενεργοποιήσετε την μετρητές επιδόσεων στην εφαρμογή σας Azure.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτή η ενότητα προϋποθέτει ότι έχετε εισάγονται στην οθόνη Διαγνωστικά στην εφαρμογή σας και προσθέσατε το αρχείο ρύθμισης παραμέτρων Διαγνωστικά τη λύση σας Visual Studio (diagnostics.wadcfg στο SDK 2.4 και παρακάτω ή diagnostics.wadcfgx στο SDK 2,5 και παραπάνω). Δείτε τα βήματα 1 και 2 σε [Ενεργοποίηση Διαγνωστικά στις υπηρεσίες Cloud Azure και εικονικές μηχανές](./cloud-services-dotnet-diagnostics.md)) για περισσότερες πληροφορίες.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Βήμα 1: Συλλογή και αποθήκευση δεδομένων από μετρητές επιδόσεων

Αφού προσθέσετε το αρχείο Διαγνωστικά τη λύση Visual Studio σας μπορείτε να ρυθμίσετε τη συλλογή και αποθήκευσης των δεδομένων μετρητή επιδόσεων σε μια εφαρμογή του Azure. Αυτό γίνεται με την προσθήκη μετρητών επιδόσεων για το αρχείο Διαγνωστικά. Διαγνωστικά δεδομένα, συμπεριλαμβανομένων των μετρητών επιδόσεων, πρώτα συλλέγονται στην παρουσία. Τα δεδομένα είναι μόνιμα, στη συνέχεια, στον πίνακα WADPerformanceCountersTable στην υπηρεσία πίνακα Azure, έτσι θα πρέπει επίσης να καθορίσετε το λογαριασμό χώρου αποθήκευσης στην εφαρμογή σας. Εάν δοκιμές εφαρμογή σας τοπικά στο το προσομοίωσης τον υπολογισμό, μπορείτε επίσης να αποθηκεύσετε δεδομένα Διαγνωστικά τοπικά στο προσομοίωσης του χώρου αποθήκευσης. Πριν από την αποθήκευση δεδομένων Διαγνωστικά μπορείτε πρώτα πρέπει να μεταβείτε στην [πύλη του Azure κλασική](http://manage.windowsazure.com/) και δημιουργία λογαριασμού χώρου αποθήκευσης. Βέλτιστη πρακτική είναι να εντοπίσετε το λογαριασμό χώρου αποθήκευσης στην ίδια παν-θέση με την εφαρμογή Azure σας προκειμένου να αποφύγετε πληρωμής εξωτερικών εύρους ζώνης κόστους και για να μειώσετε λανθάνων χρόνος.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Προσθήκη μετρητών επιδόσεων για το αρχείο Διαγνωστικά

Υπάρχουν πολλοί μετρητές που μπορείτε να χρησιμοποιήσετε. Το παρακάτω παράδειγμα εμφανίζει πολλές μετρητές επιδόσεων που προτείνονται για το web και παρακολούθηση ρόλο εργασίας.

Ανοίξτε το αρχείο Διαγνωστικά (diagnostics.wadcfg στο SDK 2.4 και παρακάτω ή diagnostics.wadcfgx στο SDK 2,5 και παραπάνω) και προσθέστε το στοιχείο DiagnosticMonitorConfiguration τα εξής:

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

Το χαρακτηριστικό bufferQuotaInMB, η οποία καθορίζει το μέγιστο μέγεθος του χώρου αποθήκευσης αρχείων συστήματος που είναι διαθέσιμη για τον τύπο συλλογής δεδομένων (Azure αρχεία καταγραφής, αρχεία καταγραφής των υπηρεσιών IIS, κ.λπ.). Η προεπιλογή είναι 0. Όταν το όριο χρονικού, τα παλαιότερα δεδομένα διαγράφονται καθώς προστίθενται νέα δεδομένα. Το άθροισμα όλων των ιδιοτήτων bufferQuotaInMB πρέπει να είναι μεγαλύτερη από την τιμή του χαρακτηριστικού OverallQuotaInMB. Για πιο λεπτομερείς πληροφορίες σχετικά με τον καθορισμό της ποσότητας χώρου αποθήκευσης, θα σας ζητηθεί για τη συλλογή δεδομένων Διαγνωστικά, ανατρέξτε στην ενότητα WAD ρύθμισης της [Αντιμετώπισης προβλημάτων βέλτιστες πρακτικές για την ανάπτυξη εφαρμογών Azure](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

Χαρακτηριστικό scheduledTransferPeriod, η οποία καθορίζει το διάστημα μεταξύ των προγραμματισμένων μεταφορές δεδομένων, στρογγυλοποιείται προς τα επάνω για να το πλησιέστερο λεπτό. Στα παρακάτω παραδείγματα έχει ρυθμιστεί να PT30M (30 λεπτά). Ρύθμιση της περιόδου μεταφοράς για μια μικρή τιμή, όπως ένα λεπτό, θα επηρεάσουν αρνητικά την απόδοση της εφαρμογής σας σε παραγωγής αλλά μπορεί να είναι χρήσιμες για εμφάνιση Διαγνωστικά εργασία γρήγορα όταν κάνετε δοκιμές. Η περίοδος προγραμματισμένη μεταφοράς πρέπει να είναι αρκετά μικρό, για να βεβαιωθείτε ότι δεν αντικαθίσταται διαγνωστικών δεδομένων στην παρουσία, αλλά είναι αρκετά μεγάλος, ότι δεν θα επηρεάσει την απόδοση της εφαρμογής σας.

Το χαρακτηριστικό counterSpecifier Καθορίζει το μετρητή επιδόσεων για τη συλλογή. Το χαρακτηριστικό sampleRate Καθορίζει την ταχύτητα με την οποία ο μετρητής επιδόσεων θα πρέπει να δείγματα, σε αυτήν την περίπτωση 30 δευτερόλεπτα.

Αφού προσθέσετε μετρητών επιδόσεων που θέλετε να συλλέξετε, αποθηκεύστε τις αλλαγές στο αρχείο Διαγνωστικά. Στη συνέχεια, πρέπει να καθορίσετε το λογαριασμό χώρου αποθήκευσης που θα είναι σταθερές τα διαγνωστικά δεδομένα για να.

### <a name="specify-the-storage-account"></a>Καθορίστε το λογαριασμό χώρου αποθήκευσης

Για να διατηρηθούν σας Διαγνωστικά πληροφορίες στο λογαριασμό σας χώρο αποθήκευσης Azure, πρέπει να καθορίσετε μια συμβολοσειρά σύνδεσης στο αρχείο παραμέτρων (ServiceConfiguration.cscfg) υπηρεσίας.

Για το 2,5 SDK Azure μπορεί να καθοριστεί το λογαριασμό χώρου αποθήκευσης του αρχείου diagnostics.wadcfgx.

>[AZURE.NOTE] Αυτές τις οδηγίες ισχύουν μόνο για Azure SDK 2.4 και κάτω από το στοιχείο. Για το 2,5 SDK Azure μπορεί να καθοριστεί το λογαριασμό χώρου αποθήκευσης του αρχείου diagnostics.wadcfgx.

Για να ορίσετε τις συμβολοσειρές σύνδεσης:

1. Ανοίξτε το αρχείο ServiceConfiguration.Cloud.cscfg χρησιμοποιώντας το πρόγραμμα επεξεργασίας κειμένου Αγαπημένα και ορίστε τη συμβολοσειρά σύνδεσης για το χώρο αποθήκευσης. Το *όνομα λογαριασμού* και *AccountKey* τιμών βρεθούν στην πύλη του Azure κλασική στον πίνακα εργαλείων λογαριασμού χώρου αποθήκευσης, στην περιοχή Διαχείριση κλειδιών.

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Αποθηκεύστε το αρχείο ServiceConfiguration.Cloud.cscfg.

3. Ανοίξτε το αρχείο ServiceConfiguration.Local.cscfg και βεβαιωθείτε ότι UseDevelopmentStorage έχει οριστεί στην τιμή true.

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
Τώρα που έχουν ρυθμιστεί οι συμβολοσειρές σύνδεσης, η εφαρμογή σας θα παραμένει Διαγνωστικά δεδομένων στο λογαριασμό σας χώρου αποθήκευσης όταν έχει αναπτυχθεί από την εφαρμογή σας.
4. Αποθήκευση και δημιουργία έργου σας και, στη συνέχεια, αναπτύξτε την εφαρμογή σας.

## <a name="step-2-optional-create-custom-performance-counters"></a>Βήμα 2: Δημιουργία (προαιρετικά) προσαρμοσμένο μετρητών

Εκτός από τους μετρητές προκαθορισμένες επιδόσεων, μπορείτε να προσθέσετε τη δική σας προσαρμοσμένη επιδόσεων μετρητές για την παρακολούθηση της web ή εργαζόμενου ρόλους. Μετρητές επιδόσεων προσαρμοσμένο μπορεί να χρησιμοποιηθεί για παρακολούθηση και παρακολούθηση της συμπεριφοράς συγκεκριμένη εφαρμογή και μπορεί να δημιουργηθεί ή διαγραφεί σε μια εργασία εκκίνησης, ρόλου web ή ρόλο εργαζόμενου με αναβαθμισμένα δικαιώματα.

Ο παράγοντας Azure Διαγνωστικά ανανεώνει τα ρυθμίσεων παραμέτρων μετρητή επιδόσεων από το αρχείο .wadcfg ένα λεπτό μετά την εκκίνηση.  Εάν δημιουργείτε προσαρμοσμένες μετρητών στη μέθοδο OnStart και διαρκούν περισσότερο από ένα λεπτό για την εκτέλεση των εργασιών σας εκκίνησης, το προσαρμοσμένο μετρητών θα δεν έχουν δημιουργηθεί όταν ο παράγοντας Διαγνωστικά Azure προσπαθεί να φορτώσει τους.  Σε αυτό το σενάριο θα δείτε ότι Διαγνωστικά Azure καταγράφει σωστά όλα τα δεδομένα Διαγνωστικά εκτός από το προσαρμοσμένο μετρητών.  Για να επιλύσετε αυτό το ζήτημα, δημιουργήστε μετρητών επιδόσεων σε μια εργασία εκκίνησης ή μετακινήστε ορισμένα της εργασίας σας εκκίνησης εργασία για τη μέθοδο OnStart μετά τη δημιουργία των μετρητών επιδόσεων.

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε ένα απλό προσαρμοσμένο επιδόσεων αντίθεση με το όνομα "\MyCustomCounterCategory\MyButton1Counter":

1. Ανοίξτε το αρχείο ορισμού υπηρεσίας (CSDEF) για την εφαρμογή σας.
2. Προσθέστε το στοιχείο χρόνου εκτέλεσης για την WebRole ή WorkerRole στοιχείο για να επιτρέψετε την εκτέλεση με αναβαθμισμένα δικαιώματα:

    ```
    <runtime executioncontext="elevated"/>
    ```
3. Αποθηκεύστε το αρχείο.
4. Ανοίξτε το αρχείο Διαγνωστικά (diagnostics.wadcfg στο SDK 2.4 και παρακάτω ή diagnostics.wadcfgx στο SDK 2,5 και παραπάνω) και προσθέστε τα εξής για να το DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Αποθηκεύστε το αρχείο.
6. Δημιουργία της κατηγορίας μετρητή επιδόσεων προσαρμοσμένο στη μέθοδο OnStart ο ρόλος σας, πριν από την κλήση βάσης. OnStart. Το παρακάτω παράδειγμα C# δημιουργεί μια προσαρμοσμένη κατηγορία, εάν δεν υπάρχει ήδη:

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. Ενημερώστε τους μετρητές μέσα από την εφαρμογή. Το παρακάτω παράδειγμα ενημερώνει μετρητή επιδόσεων προσαρμοσμένα συμβάντα Button1_Click:

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. Αποθηκεύστε το αρχείο.  

Δεδομένα του μετρητή επιδόσεων προσαρμοσμένο τώρα θα συλλεχθεί από την εποπτεία Azure Διαγνωστικά.

## <a name="step-3-query-performance-counter-data"></a>Βήμα 3: Δεδομένα του μετρητή επιδόσεων ερωτήματος

Μόλις η εφαρμογή σας έχει αναπτυχθεί και εκτελεί την οθόνη Διαγνωστικά θα ξεκινήσει τη συλλογή μετρητών επιδόσεων και συνεχίζεται αυτά τα δεδομένα με το Azure χώρο αποθήκευσης. Μπορείτε να χρησιμοποιήσετε εργαλεία όπως το διακομιστή Explorer στο Visual Studio, την [Εξερεύνηση χώρου αποθήκευσης Azure](http://azurestorageexplorer.codeplex.com/)ή [Azure Διαγνωστικά Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) με Cerebrata για να προβάλετε τα δεδομένα μετρητών επιδόσεων στον πίνακα WADPerformanceCountersTable. Μπορείτε επίσης ορθογώνιο να υποβάλετε ερώτημα στην υπηρεσία πίνακα χρησιμοποιώντας [C#](../storage/storage-dotnet-how-to-use-tables.d), [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [φωνητικής γραφής](../storage/storage-ruby-how-to-use-table-storage.md)ή [PHP](../storage/storage-php-how-to-use-table-storage.md).

Το παρακάτω παράδειγμα C# εμφανίζει ένα απλό ερώτημα σε σχέση με τον πίνακα WADPerformanceCountersTable και αποθηκεύει τα διαγνωστικά δεδομένα σε ένα αρχείο CSV. Μόλις μετρητών επιδόσεων αποθηκεύονται σε ένα αρχείο CSV μπορείτε να χρησιμοποιήσετε τις δυνατότητες γραφική στο Microsoft Excel ή ορισμένες άλλες εργαλείο για την απεικόνιση των δεδομένων. Φροντίστε να προσθέσετε μια αναφορά σε Microsoft.WindowsAzure.Storage.dll, το οποίο αποτελεί περιλαμβάνονται στο SDK Azure για το .NET Οκτώβριος 2012 και νεότερη έκδοση. Συγκρότησης είναι εγκατεστημένη στον κατάλογο προγράμματος Azure.NET SDKs\Microsoft Files%\Microsoft SDK\version-num\ref\ %.

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

Αντιστοίχιση οντοτήτων για να C# αντικείμενα χρησιμοποιώντας μια προσαρμοσμένη κλάση που προέρχονται από **TableEntity**. Ο ακόλουθος κώδικας καθορίζει μια κλάση οντότητα που αντιπροσωπεύει ένα μετρητή επιδόσεων στον πίνακα **WADPerformanceCountersTable** .


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>Επόμενα βήματα
[Πρόσθετα άρθρα για προβολή σε διαγνωστικά Azure] (.. / azure diagnostics.md)
