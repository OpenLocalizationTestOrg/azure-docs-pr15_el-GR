<properties
    pageTitle="Εκτέλεση MPI εφαρμογών σε δέσμη Azure με εργασίες πολλών παρουσιών | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να εκτελέσει εφαρμογές μηνύματος που περνά μέσα περιβάλλοντος εργασίας (MPI) χρησιμοποιώντας τον τύπο εργασίας πολλών παρουσιών σε δέσμη Azure."
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
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>Χρήση πολλών παρουσιών εργασιών για να εκτελέσετε εφαρμογές του μηνύματος που περνά μέσα περιβάλλοντος εργασίας (MPI) σε δέσμη Azure

Εργασίες πολλών παρουσιών σάς επιτρέπουν να εκτελείτε μια εργασία του Azure δέσμης σε πολλούς κόμβους υπολογιστικών ταυτόχρονα. Αυτές οι εργασίες Ενεργοποίηση υψηλών επιδόσεων υπολογιστική σενάρια όπως εφαρμογές μηνύματος που περνά μέσα περιβάλλοντος εργασίας (MPI) της δέσμης. Σε αυτό το άρθρο, θα μάθετε πώς να εκτελεί εργασίες πολλών παρουσιών χρησιμοποιώντας τη [Μαζική .NET] [ api_net] βιβλιοθήκη.

>[AZURE.NOTE] Ενώ τα παραδείγματα σε αυτό το άρθρο εστίαση σε δέσμη .NET, MS-MPI, και των Windows, τον υπολογισμό κόμβους, τις έννοιες εργασίας πολλών παρουσιών που αναφέρονται εδώ ισχύουν για άλλες πλατφόρμες και τεχνολογίες (Python και Intel MPI σε κόμβους Linux, για παράδειγμα).

## <a name="multi-instance-task-overview"></a>Επισκόπηση εργασίας πολλών παρουσιών

Στη δέσμη, κάθε εργασία είναι συνήθως εκτελείται σε ένα μεμονωμένο κόμβος--υποβολή πολλές εργασίες σε μια εργασία και την υπηρεσία δέσμη προγραμματίζει κάθε εργασία για εκτέλεση σε έναν κόμβο. Ωστόσο, κατά τη ρύθμιση των παραμέτρων μιας εργασίας **πολλών παρουσιών ρυθμίσεων**, μπορείτε να καταλάβετε δέσμη για να δημιουργήσετε αντί για αυτό ένα πρωτεύον εργασίας και πολλές δευτερεύουσες εργασίες που εκτελούνται, στη συνέχεια, σε πολλά κόμβους.

![Επισκόπηση εργασίας πολλών παρουσιών][1]

Κατά την υποβολή μιας εργασίας με τις ρυθμίσεις πολλών παρουσιών για μια εργασία, δέσμη πραγματοποιεί αρκετά βήματα μοναδικές πολλών παρουσιών εργασίες:

1. Η υπηρεσία δέσμη δημιουργεί μία **κύρια** και αρκετές **δευτερεύουσες εργασίες** με βάση τις ρυθμίσεις πολλών παρουσιών. Ο συνολικός αριθμός των εργασιών (πρωτεύων συν όλες τις δευτερεύουσες εργασίες) συμφωνεί με τον αριθμό των **παρουσιών** (κόμβους υπολογιστικών) που καθορίζετε στις ρυθμίσεις πολλών παρουσιών.
1. Μαζική ορίζει έναν από τους κόμβους υπολογισμού ως **κύρια**και προγραμματίζει την κύρια εργασία να εκτελέσει στο υπόδειγμα. Προγραμματίζει τις δευτερεύουσες εργασίες για την εκτέλεση στα υπόλοιπα κόμβους υπολογιστικών έχει εκχωρηθεί στην εργασία πολλών παρουσιών, μία δευτερεύουσα εργασία ανά κόμβο.
1. Η κύρια και όλες τις δευτερεύουσες εργασίες λήψη τα **κοινά αρχεία πόρων** που καθορίζετε στις ρυθμίσεις πολλών παρουσιών.
1. Μετά τον πόρο κοινά αρχεία έχουν ληφθεί, των κύριων και δευτερευουσών εργασιών εκτελέσει την **εντολή συντονισμό** που καθορίζετε στις ρυθμίσεις πολλών παρουσιών. Η εντολή συντονισμό χρησιμοποιείται συνήθως για να προετοιμάσετε τους κόμβους για την εκτέλεση της εργασίας. Αυτό μπορεί να περιλαμβάνει ξεκινώντας υπηρεσίες στο παρασκήνιο (όπως το [Microsoft MPI][msmpi_msdn]του `smpd.exe`) και την επαλήθευση ότι οι κόμβοι είναι έτοιμες για την επεξεργασία μηνυμάτων μεταξύ κόμβο.
1. Η κύρια εργασία εκτελεί την **εντολή εφαρμογής** στον το κύριο κόμβο *μετά* την εντολή συντονισμό έχει ολοκληρωθεί με επιτυχία, των κύριων και όλες τις δευτερεύουσες εργασίες. Η εντολή εφαρμογή είναι η γραμμή εντολών της εργασίας πολλών παρουσιών ίδια και εκτελείται μόνο από την κύρια εργασία. Σε μια [MS-MPI][msmpi_msdn]-βάσει λύση, αυτό είναι όπου μπορείτε να εκτελέσετε την εφαρμογή με δυνατότητα MPI χρησιμοποιώντας `mpiexec.exe`.

> [AZURE.NOTE] Αν και είναι λειτουργικά distinct, η εργασία"πολλών παρουσιών" δεν είναι ένας τύπος μοναδικών εργασιών, όπως το [StartTask] [ net_starttask] ή [JobPreparationTask][net_jobprep]. Η εργασία πολλών παρουσιών είναι απλώς μια τυπική εργασία δέσμης ([CloudTask] [ net_task] στο .NET δέσμη) των οποίων τις ρυθμίσεις πολλών παρουσιών που έχουν ρυθμιστεί. Σε αυτό το άρθρο, αναφέρουμε αυτό ως την **παρουσία πολλών εργασιών**.

## <a name="requirements-for-multi-instance-tasks"></a>Απαιτήσεις για τις εργασίες πολλών παρουσιών

Εργασίες πολλών παρουσιών απαιτούν ένα χώρο συγκέντρωσης με **επικοινωνία μεταξύ κόμβο με δυνατότητα**και με **ταυτόχρονη εργασία εκτέλεσης απενεργοποιηθεί**. Εάν προσπαθείτε να εκτελέσετε μια εργασία πολλών παρουσιών σε ένα χώρο συγκέντρωσης με μεσογονατίου επικοινωνίας απενεργοποιηθεί ή με *maxTasksPerNode* τιμή μεγαλύτερη από 1, η εργασία είναι προγραμματισμένη ποτέ--παραμένει απεριόριστο χρονικό διάστημα στην κατάσταση "Ενεργή". Το τμήμα κώδικα δείχνει τη δημιουργία όπως ένα χώρο συγκέντρωσης χρησιμοποιώντας τη βιβλιοθήκη .NET δέσμης.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Επιπλέον, εργασίες πολλών παρουσιών μπορεί να εκτελεστεί *μόνο* σε κόμβους **χώρους συγκέντρωσης που έχουν δημιουργηθεί μετά την 14 Δεκεμβρίου 2015**.

### <a name="use-a-starttask-to-install-mpi"></a>Χρησιμοποιήστε ένα StartTask για την εγκατάσταση MPI

Για να εκτελέσετε εφαρμογές MPI με μια εργασία πολλών παρουσιών, πρέπει πρώτα να εγκαταστήσετε μια εφαρμογή MPI (MS-MPI ή MPI Intel, για παράδειγμα) σε κόμβους υπολογιστικών στο χώρο συγκέντρωσης. Αυτή είναι μια καλή φορά για να χρησιμοποιήσετε μια [StartTask][net_starttask], που εκτελεί κάθε φορά έναν κόμβο ενώνει ένα χώρο συγκέντρωσης, ή να γίνει επανεκκίνηση του. Σε αυτό το τμήμα κώδικα δημιουργεί μια StartTask που καθορίζει το πακέτο εγκατάστασης MS-MPI ως [αρχείο πόρου][net_resourcefile]. Γραμμή εντολών Έναρξη εργασίας εκτελείται μετά το αρχείο πόρου λήψη στον κόμβο. Σε αυτήν την περίπτωση, η γραμμή εντολών εκτελεί μια εγκατάσταση χωρίς παρακολούθηση του MS-MPI.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Πρόσβαση σε απομακρυσμένο απευθείας μνήμη (RDMA)

Όταν επιλέγετε ένα [μέγεθος με δυνατότητα RDMA](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) όπως A9 για κόμβους υπολογιστικών στο σας συγκέντρωσης δέσμη, η εφαρμογή MPI σας να επωφεληθείτε από του Azure υψηλών επιδόσεων, χαμηλής αδράνειας απομακρυσμένης μνήμης απευθείας πρόσβαση (RDMA) δικτύου.

Αναζητήστε τα μεγέθη που έχουν καθοριστεί ως "Με δυνατότητα RDMA" στα ακόλουθα άρθρα:

* Σύνολα **CloudServiceConfiguration**

  * [Μεγέθη για τις υπηρεσίες Cloud](../cloud-services/cloud-services-sizes-specs.md) (Μόνο στα Windows)

* Σύνολα **VirtualMachineConfiguration**

  * [Μεγέθη για εικονικές μηχανές στο Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux)

  * [Μεγέθη για εικονικές μηχανές στο Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows)

>[AZURE.NOTE] Για να επωφεληθείτε από RDMA στην [Linux τον υπολογισμό κόμβους](batch-linux-nodes.md), πρέπει να χρησιμοποιήσετε **Intel MPI** στους κόμβους. Για περισσότερες πληροφορίες σχετικά με τα σύνολα CloudServiceConfiguration και VirtualMachineConfiguration, ανατρέξτε στην ενότητα [χώρος συγκέντρωσης](batch-api-basics.md#Pool) από την επισκόπηση δυνατοτήτων δέσμης.

## <a name="create-a-multi-instance-task-with-batch-net"></a>Δημιουργία εργασίας πολλών παρουσιών με δέσμη .NET

Τώρα που θα σας έχετε καλύπτεται το χώρο συγκέντρωσης απαιτήσεις και εγκατάσταση του πακέτου MPI, ας δημιουργήσουμε της εργασίας πολλών παρουσιών. Σε αυτό το τμήμα κώδικα, μπορούμε να δημιουργήσουμε μια τυπική [CloudTask][net_task], στη συνέχεια, ρυθμίστε τις παραμέτρους του [MultiInstanceSettings] [ net_multiinstance_prop] την ιδιότητα. Όπως προαναφέρθηκε, η εργασία πολλών παρουσιών δεν είναι έναν τύπο διακριτού εργασίας, αλλά μια τυπική εργασία δέσμης έχει ρυθμιστεί με τις ρυθμίσεις πολλών παρουσιών.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Κύρια εργασία και δευτερεύουσες εργασίες

Όταν δημιουργείτε τις ρυθμίσεις πολλών παρουσιών για μια εργασία, μπορείτε να καθορίσετε τον αριθμό των κόμβους υπολογιστικών που είναι να εκτελέσει την εργασία. Κατά την υποβολή της εργασίας σε ένα έργο, η υπηρεσία δέσμη δημιουργεί μία **κύρια** εργασία και αρκετές **δευτερεύουσες εργασίες** που μαζί με τον αριθμό των κόμβους που καθορίσατε.

Αυτές οι εργασίες έχουν ανατεθεί σε αναγνωριστικό ακέραιος στην περιοχή από 0 σε *numberOfInstances* - 1. Η εργασία με το αναγνωριστικό 0 είναι η κύρια εργασία και όλα τα άλλα αναγνωριστικά είναι δευτερεύουσες εργασίες. Για παράδειγμα, εάν δημιουργείτε τις ακόλουθες ρυθμίσεις πολλών παρουσιών για μια εργασία, την κύρια εργασία να έχουν αναγνωριστικό 0 και τις δευτερεύουσες εργασίες να έχουν αναγνωριστικά 1 έως 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Κύρια κόμβου

Κατά την υποβολή μιας εργασίας πολλών παρουσιών, η υπηρεσία δέσμη ορίζει έναν από τους κόμβους υπολογισμού ως "κύρια" κόμβος και προγραμματίζει την κύρια εργασία να εκτελέσει στον κύριο κόμβο. Το χρονοδιάγραμμα των δευτερευουσών εργασιών για την εκτέλεση στα υπόλοιπα τους κόμβους έχει εκχωρηθεί στην εργασία πολλών παρουσιών.

## <a name="coordination-command"></a>Εντολή συντονισμό

Η **εντολή συντονισμό** εκτελείται με δύο των κύριων και δευτερευουσών εργασιών.

Αποκλείει την κλήση της εντολής συντονισμό--δέσμη δεν εκτελεί την εντολή εφαρμογή μέχρι την εντολή συντονισμό επέστρεψε με επιτυχία για όλες τις δευτερεύουσες εργασίες. Η εντολή συντονισμό πρέπει, επομένως, Έναρξη οποιεσδήποτε υπηρεσίες απαιτείται φόντου βεβαιωθείτε ότι είναι έτοιμες για χρήση και, στη συνέχεια, κλείστε. Για παράδειγμα, αυτή η εντολή συντονισμό για μια λύση, χρησιμοποιώντας την έκδοση MS-MPI 7 ξεκινά η υπηρεσία SMPD στον κόμβο, στη συνέχεια, Έξοδος από:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Σημειώστε τη χρήση της `start` σε αυτήν την εντολή συντονισμό. Αυτό είναι απαραίτητο, επειδή το `smpd.exe` εφαρμογή δεν επιστρέφει αμέσως μετά την εκτέλεση. Χωρίς τη χρήση του [Έναρξη] [ cmd_start] εντολή, αυτή η εντολή συντονισμό δεν θα επιστρέψει και, επομένως, να αποκλείσετε την εφαρμογή εντολή Εκτέλεση.

## <a name="application-command"></a>Εντολή εφαρμογής

Όταν η κύρια εργασία και όλες τις δευτερεύουσες εργασίες έχετε ολοκληρώσει την εκτέλεση της εντολής συντονισμό, γραμμή εντολών της εργασίας πολλών παρουσιών εκτελείται κατά το πρωτεύον εργασίας *μόνο*. Ονομάζουμε αυτή την **εντολή εφαρμογής** για να διακρίνετε από την εντολή συντονισμό.

Για τις εφαρμογές του MS-MPI, χρησιμοποιήστε την εντολή εφαρμογή για να εκτελέσετε την εφαρμογή σας με δυνατότητα MPI με `mpiexec.exe`. Για παράδειγμα, ακολουθεί μια εντολή εφαρμογής για μια λύση χρησιμοποιώντας MS-MPI έκδοση 7:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Επειδή το MS MPI `mpiexec.exe` χρησιμοποιεί το `CCP_NODES` μεταβλητή με προεπιλεγμένη (ανατρέξτε στο θέμα [μεταβλητές περιβάλλοντος](#environment-variables)) παράδειγμα γραμμής εντολών εφαρμογής παραπάνω δεν περιλαμβάνει το.

## <a name="environment-variables"></a>Μεταβλητές περιβάλλοντος

Μαζική δημιουργεί πολλές [μεταβλητές περιβάλλοντος] [ msdn_env_var] συγκεκριμένες εργασίες πολλών παρουσιών στη κόμβους υπολογιστικών εκχωρηθεί σε μια εργασία πολλών παρουσιών. Τις γραμμές εντολών συντονισμό και εφαρμογή μπορεί να παραπέμπει αυτές τις μεταβλητές περιβάλλοντος, όπως τις δέσμες ενεργειών και τα προγράμματα που εκτελούν.

Οι παρακάτω μεταβλητές περιβάλλοντος δημιουργούνται από την υπηρεσία δέσμη για χρήση από πολλών παρουσιών εργασίες:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Για πλήρεις λεπτομέρειες σχετικά με αυτές και τις άλλες δέσμη υπολογισμού κόμβο μεταβλητές περιβάλλοντος, περιλαμβανομένων των περιεχομένων τους και αναγνωσιμότητα, ανατρέξτε στο θέμα [τον υπολογισμό μεταβλητές περιβάλλοντος κόμβο][msdn_env_var].

>[AZURE.TIP] Το δείγμα κώδικα δέσμης Linux MPI περιέχει ένα παράδειγμα του τρόπου πολλές από αυτές τις μεταβλητές περιβάλλοντος μπορεί να χρησιμοποιηθεί. Το [συντονισμό cmd] [ coord_cmd_example] δέσμης ενεργειών πάρτι λήψεις συνηθισμένη εφαρμογή και εισαγωγής αρχείων από χώρο αποθήκευσης Azure, ενεργοποιεί ένα κοινόχρηστο στοιχείο του συστήματος αρχείων δικτύου (NFS) στον κόμβο κύρια και ρυθμίζει τις παραμέτρους άλλους κόμβους έχει εκχωρηθεί στην εργασία πολλών παρουσιών ως υπολογιστές-πελάτες NFS.

## <a name="resource-files"></a>Αρχεία πόρων

Υπάρχουν δύο σύνολα αρχείων πόρων για να λάβετε υπόψη σας για τις εργασίες πολλών παρουσιών: **κοινά αρχεία πόρων** που κάνετε λήψη *όλων* των εργασιών (και τα δύο κύρια και δευτερεύουσες εργασίες), καθώς και τα **αρχεία πόρων** που έχουν καθοριστεί για την παρουσία πολλών εργασιών μόνη της, ποια εργασίας *μόνο η κύρια* στοιχεία λήψης.

Μπορείτε να καθορίσετε ένα ή περισσότερα **κοινά αρχεία πόρων** στις ρυθμίσεις πολλών παρουσιών για μια εργασία. Αυτές οι συνήθεις αρχείων πόρων λαμβάνονται από [Χώρο αποθήκευσης Azure](./../storage/storage-introduction.md) σε κάθε κόμβο **καταλόγου κοινόχρηστων εργασιών** από την κύρια και όλες τις δευτερεύουσες εργασίες. Μπορείτε να αποκτήσετε πρόσβαση στον κατάλογο κοινόχρηστων εργασιών από εφαρμογή και συντονισμός γραμμές εντολών, χρησιμοποιώντας το `AZ_BATCH_TASK_SHARED_DIR` μεταβλητή περιβάλλοντος. Το `AZ_BATCH_TASK_SHARED_DIR` διαδρομή είναι ίδιες σε κάθε κόμβο έχει εκχωρηθεί στην εργασία πολλών παρουσιών, επομένως μπορείτε να μοιραστείτε μια εντολή μία συντονισμό μεταξύ των κύριων σελίδων και όλες τις δευτερεύουσες εργασίες. Μαζική δεν "κοινή χρήση" στον κατάλογο σε μια ιδέα για απομακρυσμένη πρόσβαση, αλλά μπορείτε να χρησιμοποιήσετε ως ένα ενεργοποίησης ή κοινή χρήση σημείου όπως αναφέρθηκε προηγουμένως σε τη συμβουλή στην μεταβλητές περιβάλλοντος.

Λήψη αρχείων πόρων που καθορίζετε για την εργασία πολλών παρουσιών ίδιο κατάλογο εργασίας της εργασίας, `AZ_BATCH_TASK_WORKING_DIR`, από προεπιλογή. Όπως αναφέρθηκε, σε αντίθεση με κοινά αρχεία πόρων, μόνο την κύρια εργασία λήψεις αρχείων πόρων που έχουν καθοριστεί για την εργασία πολλών παρουσιών ίδια.

> [AZURE.IMPORTANT] Χρησιμοποιείτε πάντα τις μεταβλητές περιβάλλοντος `AZ_BATCH_TASK_SHARED_DIR` και `AZ_BATCH_TASK_WORKING_DIR` για να ανατρέξετε σε αυτούς τους καταλόγους σας εντολή γραμμές. Μην επιχειρήσετε να δημιουργήσετε τις διαδρομές με μη αυτόματο τρόπο.

## <a name="task-lifetime"></a>Διάρκεια ζωής εργασίας

Η διάρκεια ζωής των στοιχείων ελέγχου πρωτεύοντος εργασίας της διάρκειας ζωής της εργασίας ολόκληρο πολλών παρουσιών. Όταν βγει από την κύρια, τερματίζονται όλες τις δευτερεύουσες εργασίες. Τον κωδικό εξόδου της κύριας είναι ο κωδικός εξόδου της εργασίας και, επομένως, χρησιμοποιείται για να προσδιορίσετε την επιτυχία ή την αποτυχία της εργασίας για σκοπούς "Επανάληψη".

Εάν αποτύχει οποιαδήποτε από τις δευτερεύουσες εργασίες, έξοδος με κώδικα επιστροφής δεν είναι μηδέν, για παράδειγμα, η εργασία ολόκληρο πολλών παρουσιών αποτυγχάνει. Η εργασία πολλών παρουσιών είναι, στη συνέχεια, να τερματιστεί και να επαναληφθεί, μέχρι το όριο "Επανάληψη".

Όταν διαγράφετε μια εργασία πολλών παρουσιών, κύρια και όλες τις δευτερεύουσες εργασίες διαγράφονται επίσης από την υπηρεσία δέσμης. Όλα δευτερεύουσας εργασίας σε καταλόγους και τα αρχεία διαγράφονται από τους κόμβους υπολογισμού, ακριβώς όπως για μια τυπική εργασία.

[TaskConstraints] [ net_taskconstraints] για μια εργασία πολλών παρουσιών, όπως το [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], και [RetentionTime] [ net_taskconstraint_retention] είναι πραγματοποίηση ιδιότητες, όπως για μια τυπική εργασία και την εφαρμογή των κύριων σελίδων και όλες τις δευτερεύουσες εργασίες. Ωστόσο, εάν αλλάξετε τη [RetentionTime] [ net_taskconstraint_retention] ιδιότητα μετά την προσθήκη της εργασίας πολλών παρουσιών στην εργασία, αυτή η αλλαγή εφαρμόζεται μόνο σε κύρια εργασία. Όλες τις δευτερεύουσες εργασίες συνεχίσετε να χρησιμοποιείτε το αρχικό [RetentionTime][net_taskconstraint_retention].

Λίστα εργασιών πρόσφατη ενός κόμβου υπολογισμού απεικονίζει το αναγνωριστικό του μιας δευτερεύουσας εργασίας εάν η εργασία πρόσφατες ήταν μέρος μιας εργασίας πολλών παρουσιών.

## <a name="obtain-information-about-subtasks"></a>Λήψη πληροφοριών σχετικά με τις δευτερεύουσες εργασίες

Για να λάβετε πληροφορίες σχετικά με τις δευτερεύουσες εργασίες χρησιμοποιώντας τη βιβλιοθήκη .NET δέσμη, καλέστε την [CloudTask.ListSubtasks] [ net_task_listsubtasks] μέθοδο. Αυτή η μέθοδος επιστρέφει πληροφορίες σχετικά με όλες τις δευτερεύουσες εργασίες και πληροφορίες σχετικά με τον κόμβο υπολογισμού που εκτελούνται οι εργασίες. Από αυτές τις πληροφορίες, μπορείτε να προσδιορίσετε το ριζικό κατάλογο κάθε δευτερεύουσα εργασία, το αναγνωριστικό του χώρου συγκέντρωσης, τρέχουσα κατάστασή, κωδικό εξόδου και άλλα. Μπορείτε να χρησιμοποιήσετε αυτές τις πληροφορίες σε συνδυασμό με το [PoolOperations.GetNodeFile] [ poolops_getnodefile] μέθοδο για τη λήψη αρχείων των δευτερευουσών εργασιών. Σημειώστε ότι αυτή η μέθοδος δεν επιστρέφει πληροφορίες για την κύρια εργασία (αναγνωριστικό 0).

> [AZURE.NOTE] Εκτός αν αναφέρεται διαφορετικά, δέσμη .NET μεθόδους που λειτουργούν τα πολλών παρουσιών [CloudTask] [ net_task] ίδια ισχύουν *μόνο* για την κύρια εργασία. Για παράδειγμα, όταν καλείτε το [CloudTask.ListNodeFiles] [ net_task_listnodefiles] μέθοδο σε μια εργασία πολλών παρουσιών, μόνο τα αρχεία της εργασίας πρωτεύοντος επιστρέφονται.

Το παρακάτω τμήμα κώδικα δείχνει πώς μπορείτε να λάβετε πληροφορίες δευτερεύουσα εργασία, καθώς και να ζητήσετε περιεχόμενα του αρχείου από τους κόμβους στην οποία εκτελείται.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Δείγμα κώδικα

Το [MultiInstanceTasks] [ github_mpi] δείγμα κώδικα σε GitHub παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε μια εργασία πολλών παρουσιών για να εκτελέσετε μια [MS-MPI] [ msmpi_msdn] εφαρμογή στη δέσμη, τον υπολογισμό κόμβοι. Ακολουθήστε τα βήματα στο θέμα [Προετοιμασία](#preparation) και [Εκτέλεση](#execution) για να εκτελέσετε το δείγμα.

### <a name="preparation"></a>Προετοιμασία

1. Ακολουθήστε τα πρώτα δύο βήματα στο θέμα [Πώς να μεταγλωττίσετε και να εκτελέσετε ένα απλό πρόγραμμα MS-MPI][msmpi_howto]. Αυτό ικανοποιεί το prerequesites για το παρακάτω βήμα.
1. Δημιουργήστε *μια έκδοση του το [MPIHelloWorld] * [ helloworld_proj] δείγμα MPI προγράμματος. Αυτό είναι το πρόγραμμα που θα εκτελείται κόμβους υπολογιστικών από την εργασία πολλών παρουσιών.
1. Δημιουργήστε ένα συμπιεσμένο αρχείο που περιέχει `MPIHelloWorld.exe` (όπου θα δημιουργηθεί βήμα 2) και `MSMpiSetup.exe` (που έχετε κάνει λήψη του βήματος 1). Θα αποστείλετε το αρχείο zip ως ένα πακέτο εφαρμογών στο επόμενο βήμα.
1. Χρήση του [Azure πύλη] [ portal] για να δημιουργήσετε μια δέσμη [εφαρμογή](batch-application-packages.md) ονομάζεται "MPIHelloWorld" και καθορίστε το αρχείο zip που δημιουργήσατε στο προηγούμενο βήμα ως έκδοση "1.0" από το πακέτο εφαρμογών. Ανατρέξτε στο θέμα [Αποστολή και διαχείριση εφαρμογών](batch-application-packages.md#upload-and-manage-applications) για περισσότερες πληροφορίες.

>[AZURE.TIP] Δημιουργήστε *μια έκδοση του* `MPIHelloWorld.exe` έτσι ώστε να μην χρειάζεται για να συμπεριλάβετε τυχόν πρόσθετες εξαρτήσεις (για παράδειγμα, `msvcp140d.dll` ή `vcruntime140d.dll`) στο πακέτο εφαρμογών σας.

### <a name="execution"></a>Εκτέλεση

1. Κάντε λήψη του [azure-δέσμη-δείγματα] [ github_samples_zip] από GitHub.
1. Ανοίξτε την MultiInstanceTasks **λύση** στο Visual Studio 2015. Το `MultiInstanceTasks.sln` λύση αρχείο βρίσκεται σε:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. Πληκτρολογήστε το διαπιστευτήρια λογαριασμού δέσμης και χώρου αποθήκευσης στο `AccountSettings.settings` του **Microsoft.Azure.Batch.Samples.Common** έργου.
1. **Δημιουργία και εκτέλεση** της λύσης MultiInstanceTasks να εκτελέσει το MPI δείγμα εφαρμογής στην υπολογίσετε κόμβους σε ένα χώρο συγκέντρωσης δέσμης.
1. *Προαιρετικό*: Χρησιμοποιήστε την [πύλη του Azure] [ portal] ή η [Εξερεύνηση δέσμη] [ batch_explorer] να εξετάσετε το δείγμα χώρου συγκέντρωσης, εργασία και εργασιών ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") πριν από τη διαγραφή τους πόρους.

>[AZURE.TIP] Μπορείτε να κάνετε λήψη του [Visual Studio Κοινότητας] [ visual_studio] δωρεάν Εάν δεν έχετε Visual Studio.

Έξοδος από το `MultiInstanceTasks.exe` είναι παρόμοια με τα εξής:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Επόμενα βήματα

- Το ιστολόγιο του Microsoft HPC & ομάδας δέσμη Azure αναλύει [MPI υποστήριξης για το Linux στη δέσμη Azure][blog_mpi_linux], και περιλαμβάνει πληροφορίες σχετικά με τη χρήση [OpenFOAM] [ openfoam] με τη μαζική. Μπορείτε να βρείτε Python δείγματα κώδικα, για [παράδειγμα OpenFOAM σε GitHub][github_mpi].

- Μάθετε πώς μπορείτε να [δημιουργήσετε χώρους συγκέντρωσης του κόμβους υπολογιστικών Linux](batch-linux-nodes.md) για χρήση σε σας MPI δέσμη Azure λύσεις.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Επισκόπηση πολλών παρουσιών"
