<properties
    pageTitle="Εξαρτήσεις σε Azure δέσμη εργασιών | Microsoft Azure"
    description="Δημιουργία εργασιών που εξαρτώνται από την επιτυχή ολοκλήρωση άλλες εργασίες για την επεξεργασία MapReduce στυλ και παρόμοια δεδομένα μεγάλο φόρτους εργασίας σε δέσμη Azure."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>Εξαρτήσεις εργασιών σε δέσμη Azure

Η δυνατότητα εξαρτήσεων εργασιών της δέσμης Azure είναι κατάλληλες, εάν θέλετε να επεξεργαστείτε:

- Στυλ MapReduce φόρτους εργασίας του στο cloud.
- Εργασίες των οποίων οι εργασίες επεξεργασίας δεδομένων μπορεί να είναι εκφρασμένη σε ένα γράφημα άμεση μη κυκλικό (DAG).
- Οποιαδήποτε άλλη εργασία στην οποία ροής εργασιών εξαρτώνται από το αποτέλεσμα της νέα εργασίες.

Εξαρτήσεις εργασιών δέσμη σάς επιτρέπουν να δημιουργήσετε εργασίες που έχουν προγραμματιστεί για εκτέλεση σε κόμβους υπολογιστικών μόνο μετά την επιτυχή ολοκλήρωση μία ή περισσότερες εργασίες. Για παράδειγμα, μπορείτε να δημιουργήσετε μια εργασία που αποδίδει κάθε πλαίσιο του 3D ταινίας με ξεχωριστές, παράλληλες εργασίες. Η τελική εργασιών--"Συγχώνευση εργασίας"--συγχωνεύει τα πλαίσια που αποδίδονται στην πλήρη ταινία μόνο αφού απόδοσης με επιτυχία όλα τα πλαίσια.

Μπορείτε να δημιουργήσετε εργασίες που εξαρτώνται από άλλες εργασίες στη σχέση ένα-προς-ένα ή ένα-προς-πολλά. Μπορείτε ακόμα να δημιουργήσετε μια εξάρτηση περιοχής όπου εργασίας εξαρτάται από την επιτυχή ολοκλήρωση μιας ομάδας εργασιών μέσα σε μια συγκεκριμένη περιοχή των αναγνωριστικών εργασίας. Μπορείτε να συνδυάσετε αυτά τα τρία βασικά σενάρια για να δημιουργήσετε σχέσεις πολλά-προς-πολλά.

## <a name="task-dependencies-with-batch-net"></a>Εξαρτήσεις εργασιών με δέσμη .NET

Σε αυτό το άρθρο, αναλύονται τον τρόπο ρύθμισης παραμέτρων εξαρτήσεις εργασιών χρησιμοποιώντας το [.NET δέσμη] [ net_msdn] βιβλιοθήκη. Θα πρώτα δείξουμε πώς μπορείτε να [ενεργοποιήσετε την εξάρτηση εργασίας](#enable-task-dependencies) σε των εργασιών σας, και, στη συνέχεια, δείχνουν πώς μπορείτε να [ρυθμίσετε τις παραμέτρους μιας εργασίας με τις εξαρτήσεις](#create-dependent-tasks). Τέλος, αναλύονται τα [εξάρτηση σενάρια](#dependency-scenarios) που υποστηρίζει δέσμη.

## <a name="enable-task-dependencies"></a>Ενεργοποίηση εξαρτήσεις εργασιών

Για να χρησιμοποιήσετε εξαρτήσεις εργασίας στην εφαρμογή σας δέσμη, πρέπει πρώτα να ενημερώσετε την υπηρεσία δέσμη ότι η εργασία χρησιμοποιεί εξαρτήσεις εργασιών. Στο .NET δέσμη, ενεργοποιήσετε σε σας [CloudJob] [ net_cloudjob] , ορίζοντας την [UsesTaskDependencies] [ net_usestaskdependencies] ιδιότητα που θα `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Στο τμήμα κώδικα "Προηγούμενο", "batchClient" είναι μια παρουσία του [BatchClient] [ net_batchclient] τάξης.

## <a name="create-dependent-tasks"></a>Δημιουργία εξαρτώμενων εργασιών

Για να δημιουργήσετε μια εργασία που εξαρτάται από την επιτυχή ολοκλήρωση μία ή περισσότερες άλλες εργασίες, υποδεικνύετε δέσμη ότι η εργασία "εξαρτάται από το" τις άλλες εργασίες. Στο .NET δέσμη, ρυθμίστε τις παραμέτρους του [CloudTask][net_cloudtask]. [DependsOn] [net_dependson] ιδιότητα με μια παρουσία του [TaskDependencies] [ net_taskdependencies] κλάσης:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Σε αυτό το τμήμα κώδικα δημιουργεί μια εργασία με το Αναγνωριστικό "Λουλουδιών" που θα προγραμματιστεί να εκτελεστεί σε έναν κόμβο υπολογισμού μόνο αφού τις εργασίες με αναγνωριστικά "Βροχή" και "Κυρ" έχει ολοκληρωθεί με επιτυχία.

 > [AZURE.NOTE] Μια εργασία θεωρείται να ολοκληρωθεί όταν είναι σε κατάσταση **ολοκληρωθεί** και το **κωδικός εξόδου** είναι `0`. Στο .NET δέσμη, αυτό σημαίνει ότι ένα [CloudTask][net_cloudtask]. [Κατάσταση] [net_taskstate] τιμή της ιδιότητας `Completed` και το CloudTask [TaskExecutionInformation][net_taskexecutioninformation]. [ExitCode] [net_exitcode] είναι η τιμή της ιδιότητας `0`.

## <a name="dependency-scenarios"></a>Εξάρτηση σενάρια

Υπάρχουν τρία σενάρια εξάρτηση βασικής εργασίας που μπορείτε να χρησιμοποιήσετε σε δέσμη Azure: ένα προς ένα, ένα-προς-πολλά και Αναγνωριστικό εργασίας περιοχής εξάρτηση. Μπορούν να συνδυαστούν να δώσετε ένα τέταρτο σενάριο, πολλά-προς-πολλά.

 Σενάριο&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Παράδειγμα | |
 :-------------------: | ------------------- | -------------------
 [Ένα προς ένα](#one-to-one) | *taskB* εξαρτάται από το *taskA* <p/> *taskB* δεν μπορούν να προγραμματιστούν για εκτέλεση, μέχρι να ολοκληρωθεί με επιτυχία το *taskA* | ![Διάγραμμα: εξάρτηση εργασίας-προς-ένα][1]
 [Ένα-προς-πολλά](#one-to-many) | *taskC* εξαρτάται από το *taskA* και *taskB* <p/> *taskC* δεν μπορούν να προγραμματιστούν για εκτέλεση μέχρι να *taskA* και *taskB* έχει ολοκληρωθεί με επιτυχία | ![Διάγραμμα: εξάρτησης εργασίας ένα-προς-πολλά][2]
 [Περιοχή Αναγνωριστικό εργασίας](#task-id-range) | *taskD* εξαρτάται από μια περιοχή εργασιών <p/> *taskD* δεν μπορούν να προγραμματιστούν για εκτέλεση, έως ότου ολοκληρωθεί με επιτυχία τις εργασίες με αναγνωριστικά *1* έως *10* | ![Διάγραμμα: Εξάρτηση περιοχής αναγνωριστικό εργασίας][3]

>[AZURE.TIP] Μπορείτε να δημιουργήσετε σχέσεις **πολλά-προς-πολλά** , όπως όπου εργασίες Γ, Δ, Ε και F κάθε εξαρτώνται από εργασίες Α και β. Αυτό είναι χρήσιμο, για παράδειγμα, σε parallelized προεπεξεργασίας σενάρια όπου ροής εργασιών σας εξαρτώνται από την έξοδο από πολλά νέα εργασίες.

### <a name="one-to-one"></a>Ένα προς ένα

Για να δημιουργήσετε μια εργασία που διαθέτει μια εξάρτηση από την επιτυχή ολοκλήρωση μία άλλη εργασία, παρέχετε ένα Αναγνωριστικό μίας εργασίας για να το [TaskDependencies][net_taskdependencies]. [OnId] [net_onid] στατική μέθοδο κατά τη συμπλήρωση του [DependsOn] [ net_dependson] ιδιότητα του [CloudTask][net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Ένα-προς-πολλά

Για να δημιουργήσετε μια εργασία που διαθέτει μια εξάρτηση από την επιτυχή ολοκλήρωση πολλαπλών εργασιών, παρέχετε μια συλλογή των αναγνωριστικών για το [TaskDependencies]εργασιών[net_taskdependencies]. [OnIds] [net_onids] στατική μέθοδο κατά τη συμπλήρωση του [DependsOn] [ net_dependson] ιδιότητα του [CloudTask][net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Περιοχή Αναγνωριστικό εργασίας

Για να δημιουργήσετε μια εργασία που διαθέτει μια εξάρτηση από την επιτυχή ολοκλήρωση μιας ομάδας εργασιών των οποίων οι ταυτότητες βρίσκεται μέσα σε μια περιοχή, παρέχετε την πρώτη και τελευταία εργασίας αναγνωριστικά στην περιοχή για να το [TaskDependencies][net_taskdependencies]. [OnIdRange] [net_onidrange] στατική μέθοδο κατά τη συμπλήρωση του [DependsOn] [ net_dependson] ιδιότητα του [CloudTask][net_cloudtask].

>[AZURE.IMPORTANT] Όταν χρησιμοποιείτε περιοχές Αναγνωριστικών εργασίας για τις εξαρτήσεις, των αναγνωριστικών εργασιών στην περιοχή *πρέπει να* είναι συμβολοσειρά παραστάσεις ακέραιες τιμές. Επιπλέον, κάθε εργασίας στην περιοχή πρέπει να ολοκληρωθεί με επιτυχία για της εξαρτώμενης εργασίας για προγραμματισμό εκτέλεσης.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Δείγμα κώδικα

Το [TaskDependencies] [ github_taskdependencies] δείγμα έργου είναι ένα από τα [δείγματα κώδικα δέσμης Azure] [ github_samples] σε GitHub. Αυτή η λύση Visual Studio 2015 παρουσιάζει πώς μπορείτε να ενεργοποιήσετε την εξάρτηση εργασίας σε μια εργασία, δημιουργία εργασιών που εξαρτώνται από άλλες εργασίες και εκτέλεση αυτές τις εργασίες σε ένα χώρο συγκέντρωσης κόμβους υπολογιστικών.

## <a name="next-steps"></a>Επόμενα βήματα

### <a name="application-deployment"></a>Ανάπτυξη εφαρμογών

Η δυνατότητα [πακέτων εφαρμογών](batch-application-packages.md) της δέσμης παρέχει έναν εύκολο τρόπο για να αναπτύξετε και τις εφαρμογές που εκτελεί τις εργασίες σας στη τον υπολογισμό κόμβους έκδοση.

### <a name="installing-applications-and-staging-data"></a>Την εγκατάσταση εφαρμογών και ενδιάμεσου δεδομένων

Ανατρέξτε στο θέμα η [εγκατάσταση των εφαρμογών και ενδιάμεσου σταδίου δεδομένων στη δέσμη τον υπολογισμό κόμβοι] [ forum_post] δημοσίευση στο φόρουμ Azure δέσμη για μια επισκόπηση της τις διάφορες μεθόδους για να προετοιμάσετε το κόμβους για να εκτελέσετε εργασίες. Αυτήν τη δημοσίευση συνταχθεί από ένα από τα μέλη της ομάδας δέσμη Azure, είναι ένα καλό κύριο βήμα σχετικά με τους διαφορετικούς τρόπους για να μεταφέρετε αρχεία (συμπεριλαμβανομένων των εφαρμογών και δεδομένων εισαγωγής εργασιών) σε σας κόμβους υπολογιστικών.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Διάγραμμα: εξάρτηση-προς-ένα"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Διάγραμμα: ένα-προς-πολλά εξάρτησης"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Διαγράμματος: εξάρτηση περιοχής αναγνωριστικό εργασίας"
