<properties
    pageTitle="Μετρικά κοινές την Azure οθόνη autoscaling. | Microsoft Azure"
    description="Μάθετε ποια μετρικά χρησιμοποιούνται ευρέως για autoscaling τις υπηρεσίες Cloud, εικονικές μηχανές και εφαρμογές Web."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/02/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure κοινές μετρικά autoscaling οθόνη

Azure autoscaling οθόνη σάς επιτρέπει να κλιμακωθεί τον αριθμό των παρουσίες που εκτελούνται προς τα επάνω ή προς τα κάτω, με βάση δεδομένα τηλεμετρίας (μετρικά). Αυτό το έγγραφο περιγράφει συνήθεις μετρικά που μπορεί να θέλετε να χρησιμοποιήσετε. Στην πύλη του Azure για τις υπηρεσίες Cloud και συστοιχίες διακομιστών, μπορείτε να επιλέξετε τη μέτρηση του πόρου για να κλιμακωθεί με. Ωστόσο, μπορείτε επίσης να επιλέξετε οποιοδήποτε μετρικό από έναν άλλο πόρο για να κλιμακωθεί με.

Εδώ θα βρείτε τις λεπτομέρειες σχετικά με τον τρόπο για να βρείτε και να παραθέσετε τα μετρικά που θέλετε να κλιμακωθεί με. Τα ακόλουθα ισχύουν για κλιμάκωση καθώς και τα σύνολα κλίμακα εικονική μηχανή.

## <a name="compute-metrics"></a>Τον υπολογισμό μετρικά
Από προεπιλογή, v2 Εικονική Azure διατίθεται με επέκταση Διαγνωστικά έχει ρυθμιστεί και έχουν τα ακόλουθα μετρικά ενεργοποιημένη.

- [Μετρήσεις επισκέπτη για τα Windows Εικονική v2](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [Μετρήσεις επισκέπτη για Εικονική Linux v2](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

Μπορείτε να χρησιμοποιήσετε το `Get MetricDefinitions` PoSH/API/CLI για να προβάλετε τα διαθέσιμα για τον πόρο VMSS μετρικά. 

Εάν χρησιμοποιείτε Εικονική κλίμακα σύνολα και δεν μπορείτε να δείτε ένα συγκεκριμένο μετρικό που παρατίθενται, στη συνέχεια, είναι πιθανό *απενεργοποιηθεί* στην επέκτασής σας Διαγνωστικά.

Εάν ένα συγκεκριμένο μετρικό δεν γίνεται δειγματοληψία ή μεταβιβάζονται με τη συχνότητα που θέλετε, μπορείτε να ενημερώσετε τις ρυθμίσεις παραμέτρων Διαγνωστικά.

Εάν ισχύει δύο παραπάνω περιπτώσεις, στη συνέχεια, εξετάστε [Χρήση του PowerShell για να ενεργοποιήσετε τα Διαγνωστικά σε έναν εικονικό υπολογιστή που εκτελεί Windows Azure](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) σχετικά με το PowerShell για να ρυθμίσετε τις παραμέτρους και να ενημερώσετε την επέκταση Διαγνωστικά Εικονική Azure για να ενεργοποιήσετε τη μέτρηση. Αυτό το άρθρο περιλαμβάνει επίσης ένα δείγμα αρχείου ρύθμισης παραμέτρων Διαγνωστικά.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Τον υπολογισμό μετρήσεις για Windows Εικονική v2 ως επισκέπτης λειτουργικό σύστημα

Όταν δημιουργείτε μια νέα Εικονική (v2) στο Azure, Διαγνωστικά είναι ενεργοποιημένη, χρησιμοποιώντας την επέκταση Διαγνωστικά.

Μπορείτε να δημιουργήσετε μια λίστα με τα μετρικά, χρησιμοποιώντας την ακόλουθη εντολή σε PowerShell.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Μπορείτε να δημιουργήσετε μια ειδοποίηση για τα ακόλουθα μετρικά.

|Μετρικό όνομα|   Μονάδα|
|---|---|
|\Processor(_Total)\% χρόνου επεξεργαστή    |Τοις εκατό|
|\Processor(_Total)\% δικαιώματα χρόνου   |Τοις εκατό|
|\Processor(_Total)\% χρόνου χρήστη |Τοις εκατό|
|Συχνότητα \Processor \Processor πληροφορίες (_Σύνολο) |Καταμέτρηση|
|\System\Processes| Καταμέτρηση|
|Πλήθος \Thread \Process (_Σύνολο)| Καταμέτρηση|
|Πλήθος \Handle \Process (_Σύνολο)  |Καταμέτρηση|
|\Memory\% δεσμευμένου byte σε χρήση   |Τοις εκατό|
|Byte που \Memory\Available|   Byte που|
|Byte που \Memory\Committed    |Byte που|
|Όριο \Memory\Commit|  Byte που|
|Byte που \Memory\Pool σελίδων|  Byte που|
|\Memory\Pool σελιδοποιημένης|   Byte που|
|\PhysicalDisk(_Total)\% χρόνου στο δίσκο| Τοις εκατό|
|\PhysicalDisk(_Total)\% χρόνος ανάγνωσης δίσκου|    Τοις εκατό|
|\PhysicalDisk(_Total)\% χρόνου εγγραφής στο δίσκο|   Τοις εκατό|
|\PhysicalDisk (_Σύνολο) \Disk μεταφορές/δευτερόλεπτο   |CountPerSecond|
|\PhysicalDisk (_Σύνολο) \Disk διαβάζει/sec   |CountPerSecond|
|\PhysicalDisk (_Σύνολο) \Disk εγγραφές/δευτερόλεπτο  |CountPerSecond|
|\PhysicalDisk (_Σύνολο) \Disk byte/δευτερόλεπτο   |BytesPerSecond|
|\PhysicalDisk (_Σύνολο) \Disk byte ανάγνωσης/δευτερόλεπτο| BytesPerSecond|
|\PhysicalDisk (_Σύνολο) \Disk byte εγγραφής/δευτερόλεπτο |BytesPerSecond|
|\Avg \PhysicalDisk (_Σύνολο). Μήκος ουράς δίσκου|  Καταμέτρηση|
|\Avg \PhysicalDisk (_Σύνολο). Μήκους ανάγνωσης ουράς δίσκου| Καταμέτρηση|
|\Avg \PhysicalDisk (_Σύνολο). Μήκους εγγραφής ουράς δίσκου |Καταμέτρηση|
|\LogicalDisk(_Total)\% ελεύθερο χώρο| Τοις εκατό|
|Megabyte που \Free \LogicalDisk (_Σύνολο)|   Καταμέτρηση|



### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Τον υπολογισμό μετρήσεις για Εικονική Linux v2 ως επισκέπτης λειτουργικό σύστημα

Όταν δημιουργείτε μια νέα Εικονική (v2) στο Azure, Διαγνωστικά είναι ενεργοποιημένη από προεπιλογή, χρησιμοποιώντας τα Διαγνωστικά επέκταση.

Μπορείτε να δημιουργήσετε μια λίστα με τα μετρικά, χρησιμοποιώντας την ακόλουθη εντολή σε PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Μπορείτε να δημιουργήσετε μια ειδοποίηση για τα ακόλουθα μετρικά.

|Μετρικό όνομα                            |Μονάδα|
|---|---|
|\Memory\AvailableMemory                |Byte που|
|\Memory\PercentAvailableMemory         |Τοις εκατό|
|\Memory\UsedMemory                     |Byte που|
|\Memory\PercentUsedMemory              |Τοις εκατό|
|\Memory\PercentUsedByCache             |Τοις εκατό|
|\Memory\PagesPerSec                    |CountPerSecond|
|\Memory\PagesReadPerSec                |CountPerSecond|
|\Memory\PagesWrittenPerSec             |CountPerSecond|
|\Memory\AvailableSwap                  |Byte που|
|\Memory\PercentAvailableSwap           |Τοις εκατό|
|\Memory\UsedSwap                       |Byte που|
|\Memory\PercentUsedSwap                |Τοις εκατό|
|\Processor\PercentIdleTime             |Τοις εκατό|
|\Processor\PercentUserTime             |Τοις εκατό|
|\Processor\PercentNiceTime             |Τοις εκατό|
|\Processor\PercentPrivilegedTime       |Τοις εκατό|
|\Processor\PercentInterruptTime        |Τοις εκατό|
|\Processor\PercentDPCTime              |Τοις εκατό|
|\Processor\PercentProcessorTime        |Τοις εκατό|
|\Processor\PercentIOWaitTime           |Τοις εκατό|
|\PhysicalDisk\BytesPerSecond           |BytesPerSecond|
|\PhysicalDisk\ReadBytesPerSecond       |BytesPerSecond|
|\PhysicalDisk\WriteBytesPerSecond      |BytesPerSecond|
|\PhysicalDisk\TransfersPerSecond       |CountPerSecond|
|\PhysicalDisk\ReadsPerSecond           |CountPerSecond|
|\PhysicalDisk\WritesPerSecond          |CountPerSecond|
|\PhysicalDisk\AverageReadTime          |Δευτερόλεπτα|
|\PhysicalDisk\AverageWriteTime         |Δευτερόλεπτα|
|\PhysicalDisk\AverageTransferTime      |Δευτερόλεπτα|
|\PhysicalDisk\AverageDiskQueueLength   |Καταμέτρηση|
|\NetworkInterface\BytesTransmitted     |Byte που|
|\NetworkInterface\BytesReceived        |Byte που|
|\NetworkInterface\PacketsTransmitted   |Καταμέτρηση|
|\NetworkInterface\PacketsReceived      |Καταμέτρηση|
|\NetworkInterface\BytesTotal           |Byte που|
|\NetworkInterface\TotalRxErrors        |Καταμέτρηση|
|\NetworkInterface\TotalTxErrors        |Καταμέτρηση|
|\NetworkInterface\TotalCollisions      |Καταμέτρηση|




## <a name="commonly-used-web-server-farm-metrics"></a>Μετρήσεις που χρησιμοποιούνται συχνά Web (σύμπλεγμα διακομιστών)

Μπορείτε επίσης να εκτελέσετε autoscale βάσει κοινών μετρικών διακομιστή web όπως το μήκος ουράς Http. Όνομα μετρικό του είναι **HttpQueueLength**.  Ενότητα που ακολουθεί παραθέτει διαθέσιμο διακομιστή μετρικά συστοιχία (Web Apps).

### <a name="web-apps-metrics"></a>Μετρικά εφαρμογών Web

Μπορείτε να δημιουργήσετε μια λίστα με τα μετρικά εφαρμογές Web, χρησιμοποιώντας την ακόλουθη εντολή σε PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Μπορείτε να λαμβάνω σχετικά ή την κλίμακα από αυτές τις μετρήσεις.

|Μετρικό όνομα        |Μονάδα|
|---                |---|
|CpuPercentage      |Τοις εκατό|
|MemoryPercentage   |Τοις εκατό|
|DiskQueueLength    |Καταμέτρηση|
|HttpQueueLength    |Καταμέτρηση|
|BytesReceived      |Byte που|
|BytesSent          |Byte που|


## <a name="commonly-used-storage-metrics"></a>Χρησιμοποιείται συχνά μετρικά αποθήκευσης
Μπορείτε να κλιμάκωση κατά μήκος ουράς χώρου αποθήκευσης, που είναι ο αριθμός των μηνυμάτων στην ουρά του χώρου αποθήκευσης. Μήκος ουράς χώρου αποθήκευσης είναι ένα ειδικό μετρικό και το όριο εφαρμόζεται θα είναι ο αριθμός των μηνυμάτων ανά παρουσία. Αυτό σημαίνει ότι αν υπάρχουν δύο παρουσίες και εάν το όριο έχει οριστεί στην τιμή 100, αυτό θα κλίμακα, όταν ο συνολικός αριθμός των μηνυμάτων στην ουρά είναι 200. Για παράδειγμα, 100 μηνύματα ανά παρουσία.

Μπορείτε να ρυθμίσετε αυτή είναι στην πύλη Azure με το blade **Ρυθμίσεις** . Για Εικονική κλίμακα σύνολα, μπορείτε να ενημερώσετε τη ρύθμιση Autoscale στο πρότυπο ARM για να χρησιμοποιήσετε *metricName* ως *ApproximateMessageCount* και μεταβιβάζουν το Αναγνωριστικό του χώρου αποθήκευσης ουρά ως *metricResourceUri*.

Για παράδειγμα, με ένα λογαριασμό του χώρου αποθήκευσης κλασική να περιλαμβάνει το metricTrigger ρύθμιση autoscale:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

Για ένα λογαριασμό χώρου αποθήκευσης (μη-κλασικό), το metricTrigger να περιλαμβάνει:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>Μετρήσεις που χρησιμοποιούνται συχνά Bus υπηρεσίας

Μπορείτε να κλιμάκωση κατά μήκος ουράς Bus υπηρεσίας, που είναι ο αριθμός των μηνυμάτων στην ουρά Bus υπηρεσίας. Μήκος Bus υπηρεσίας ουράς είναι μια ειδική μετρικό σύστημα και το όριο που καθορίζεται εφαρμοστεί θα τον αριθμό των μηνυμάτων ανά παρουσία. Αυτό σημαίνει ότι αν υπάρχουν δύο παρουσίες και εάν το όριο έχει οριστεί στην τιμή 100, αυτό θα κλίμακα, όταν ο συνολικός αριθμός των μηνυμάτων στην ουρά είναι 200. Για παράδειγμα, 100 μηνύματα ανά παρουσία.

Για Εικονική κλίμακα σύνολα, μπορείτε να ενημερώσετε τη ρύθμιση Autoscale στο πρότυπο ARM για να χρησιμοποιήσετε *metricName* ως *ApproximateMessageCount* και μεταβιβάζουν το Αναγνωριστικό του χώρου αποθήκευσης ουρά ως *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] Για Bus υπηρεσίας, την έννοια ομάδα πόρων δεν υπάρχει αλλά διαχείριση πόρων Azure δημιουργεί μια προεπιλεγμένη ομάδα πόρων ανά περιοχή. Η ομάδα πόρων είναι συνήθως στη μορφή "Προεπιλογή - ServiceBus-[περιοχή]". Για παράδειγμα, 'Προεπιλογή-ServiceBus-EastUS', 'Προεπιλεγμένη-ServiceBus-WestUS', 'Προεπιλεγμένη-ServiceBus-AustraliaEast' κ.λπ.
