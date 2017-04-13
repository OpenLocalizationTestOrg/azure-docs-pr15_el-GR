<properties
   pageTitle="Καταγραφή διαγνωστικών Azure δέσμη | Microsoft Azure"
   description="Εγγραφή και να αναλύσετε συμβάντα αρχείο καταγραφής διαγνωστικών για μαζική Azure λογαριασμό πόρους, όπως σύνολα και εργασίες."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Καταγραφή διαγνωστικών Azure δέσμης

Όπως με πολλές υπηρεσίες Azure, την υπηρεσία δέσμη εκπέμπει αρχείο καταγραφής συμβάντων για ορισμένους πόρους κατά τη διάρκεια ζωής του πόρου. Μπορείτε να ενεργοποιήσετε δέσμη Azure αρχεία καταγραφής διαγνωστικών για να εγγράψετε συμβάντα για πόρους, όπως σύνολα και εργασίες και, στη συνέχεια, χρησιμοποιήστε τα αρχεία καταγραφής διαγνωστικών αξιολόγησης και παρακολούθησης. Δημιουργία συμβάντα όπως χώρου συγκέντρωσης, Διαγραφή χώρου συγκέντρωσης, έναρξη της εργασίας, η εργασία ολοκληρώθηκε και άλλους περιλαμβάνονται στα αρχεία καταγραφής διαγνωστικών δέσμης.

>[AZURE.NOTE] Σε αυτό το άρθρο περιγράφεται η καταγραφή συμβάντων για τους πόρους λογαριασμό δέσμη τον εαυτό τους, δεν έργων και εργασιών δεδομένα εξόδου. Για λεπτομέρειες σχετικά με την αποθήκευση των δεδομένων εξόδου τις εργασίες και τις εργασίες, ανατρέξτε στο θέμα [παραμένει δέσμη Azure εξόδου έργων και εργασιών](batch-task-output.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* [Λογαριασμός Azure δέσμης](batch-account-create-portal.md)

* [Λογαριασμός Azure χώρου αποθήκευσης](../storage/storage-create-storage-account.md#create-a-storage-account)

  Για να διατηρηθούν αρχεία καταγραφής διαγνωστικών δέσμη, πρέπει να δημιουργήσετε ένα λογαριασμό αποθήκευσης Azure όπου Azure θα αποθηκεύσει τα αρχεία καταγραφής. Μπορείτε να καθορίσετε αυτόν το λογαριασμό χώρου αποθήκευσης όταν [ενεργοποιήσετε την καταγραφή διαγνωστικών](#enable-diagnostic-logging) για το λογαριασμό σας δέσμης. Ο λογαριασμός χώρου αποθήκευσης που καθορίζετε κατά την ενεργοποίηση καταγραφής συλλογής δεν είναι ίδια με ένα λογαριασμό συνδεδεμένων χώρου αποθήκευσης που αναφέρεται στα άρθρα [πακέτων εφαρμογών](batch-application-packages.md) και [Διατήρηση εξόδου εργασίας](batch-task-output.md) .

  >[AZURE.WARNING] Θα **χρεωθείτε** για τα δεδομένα που είναι αποθηκευμένα στο λογαριασμό σας στο χώρο αποθήκευσης Azure. Αυτό περιλαμβάνει τα αρχεία καταγραφής διαγνωστικών που αναφέρονται σε αυτό το άρθρο. Λάβετε αυτό υπόψη κατά τη σχεδίαση την [πολιτική διατήρησης αρχείου καταγραφής](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).

## <a name="enable-diagnostic-logging"></a>Ενεργοποιήσετε την καταγραφή διαγνωστικών

Καταγραφή διαγνωστικών δεν είναι ενεργοποιημένη από προεπιλογή για το λογαριασμό σας δέσμης. Πρέπει να ενεργοποιήσετε ρητά καταγραφή διαγνωστικών για κάθε λογαριασμό δέσμη που θέλετε να παρακολουθήσετε:

[Πώς μπορείτε να ενεργοποιήσετε τη συλλογή των αρχείων καταγραφής διαγνωστικών](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

Συνιστάται να διαβάσετε το πλήρες άρθρο [Επισκόπηση του Azure αρχεία καταγραφής διαγνωστικών](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) για να κατανοήσετε όχι μόνο τρόπος για να ενεργοποιήσετε την καταγραφή, αλλά το αρχείο καταγραφής κατηγορίες που υποστηρίζονται από τις διάφορες υπηρεσίες Azure. Για παράδειγμα, δέσμη Azure υποστηρίζει επί του παρόντος μία κατηγορία καταγραφής: **Αρχεία καταγραφής από την υπηρεσία**.

## <a name="service-logs"></a>Αρχεία καταγραφής από την υπηρεσία

Αρχεία καταγραφής υπηρεσιών Azure δέσμη περιέχουν συμβάντα που εκπέμπει από την υπηρεσία δέσμη Azure κατά τη διάρκεια ζωής ενός πόρου δέσμης, όπως μια εργασία ή το χώρο συγκέντρωσης. Κάθε συμβάντος εκπομπής δέσμη αποθηκεύονται σε ένα καθορισμένο λογαριασμό χώρου αποθήκευσης σε μορφή JSON. Για παράδειγμα, αυτό είναι το σώμα του ένα δείγμα **χώρου συγκέντρωσης δημιουργία συμβάντος**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Κάθε σώμα συμβάντος βρίσκεται σε ένα αρχείο .json στο συγκεκριμένο λογαριασμό αποθήκευσης Azure. Εάν θέλετε να έχουν άμεση πρόσβαση στα αρχεία καταγραφής, ίσως θελήσετε να αναθεωρήσετε το [σχήμα των αρχείων καταγραφής διαγνωστικών στο λογαριασμό χώρου αποθήκευσης](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

## <a name="service-log-events"></a>Υπηρεσία καταγραφής συμβάντων

Η υπηρεσία δέσμη εκπέμπει αυτήν τη στιγμή τα ακόλουθα αρχείο καταγραφής της υπηρεσίας. Αυτή η λίστα ενδέχεται να μην είναι εκτεταμένη, επειδή το πρόσθετα συμβάντα ενδέχεται να έχουν προστεθεί επειδή έγινε η τελευταία ενημέρωση σε αυτό το άρθρο.

| **Υπηρεσία καταγραφής συμβάντων** |
| ------------------ |
| [Δημιουργία χώρου συγκέντρωσης][pool_create] |
| [Έναρξη Διαγραφή χώρου συγκέντρωσης][pool_delete_start] |
| [Διαγραφή χώρου συγκέντρωσης ολοκλήρωσης][pool_delete_complete] |
| [Έναρξη αλλαγής μεγέθους χώρου συγκέντρωσης][pool_resize_start] |
| [Αλλαγή μεγέθους του χώρου συγκέντρωσης ολοκλήρωσης][pool_resize_complete] |
| [Έναρξη της εργασίας][task_start] |
| [Η εργασία ολοκληρώθηκε][task_complete] |
| [ΑΠΟΤΥΧΙΑ εργασίας][task_fail] |

## <a name="next-steps"></a>Επόμενα βήματα

Εκτός από την αποθήκευση συμβάντα του αρχείου καταγραφής διαγνωστικών σε ένα λογαριασμό αποθήκευσης Azure, μπορείτε επίσης ροή συμβάντα του αρχείου καταγραφής υπηρεσίας δέσμη με ένα [Διανομέα συμβάν Azure](../event-hubs/event-hubs-what-is-event-hubs.md), και στείλτε τους για [Ανάλυση καταγραφής Azure](../log-analytics/log-analytics-overview.md).

* [Ροή Azure αρχεία καταγραφής διαγνωστικών με διανομείς συμβάντος](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  Κατευθύνετε δέσμη διαγνωστικών συμβάντα με την υπηρεσία εισόδου ιδιαίτερα με δεδομένα, διανομείς συμβάν. Συμβάν διανομείς να ingest εκατομμύρια συμβάντα ανά δευτερόλεπτο, το οποίο, στη συνέχεια, μπορείτε να μετασχηματισμός και να αποθηκεύσετε οποιαδήποτε υπηρεσία παροχής σε πραγματικό χρόνο αναλυτικών στοιχείων χρήσης.

* [Ανάλυση Azure αρχεία καταγραφής διαγνωστικών καταγραφής αναλυτικών στοιχείων χρήσης](../log-analytics/log-analytics-azure-storage-json.md)

  Αποστολή του αρχεία καταγραφής διαγνωστικών καταγραφής ανάλυσης όπου μπορείτε να αναλύσετε τους στην πύλη του λειτουργίες διαχείρισης οικογένεια προγραμμάτων (OMS) ή να τις εξαγάγετε για ανάλυση στο Power BI ή το Excel.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx