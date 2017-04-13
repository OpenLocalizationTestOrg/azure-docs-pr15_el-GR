<properties
    pageTitle="Εργασία προετοιμασίας και εκκαθάριση στη δέσμη | Microsoft Azure"
    description="Χρησιμοποιήστε τις εργασίες προετοιμασίας επιπέδου εργασίας για να ελαχιστοποιήσετε μεταφορά δεδομένων στη δέσμη Azure κόμβους υπολογιστικών και αφήστε εργασίες για εκκαθάριση κόμβου στο ολοκλήρωση του έργου."
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>Εκτέλεση εργασία προετοιμασίας και ολοκλήρωση εργασιών σε κόμβους υπολογιστικών δέσμη Azure

 Μια μαζική Azure εργασία απαιτεί συχνά κάποια μορφή της εγκατάστασης πριν από τις εργασίες εκτελούνται και μετά εργασίας συντήρησης όταν τις εργασίες ολοκληρωθούν. Ίσως χρειαστεί να λήψη δεδομένων εισόδου κοινών εργασιών για να σας κόμβους υπολογιστικών ή αποστολή δεδομένων εξόδου εργασιών στο χώρο αποθήκευσης Azure μετά την ολοκλήρωση της εργασίας. Μπορείτε να χρησιμοποιήσετε **εργασία προετοιμασίας** και **αφήστε εργασία** εργασίες για την εκτέλεση αυτών των λειτουργιών.

## <a name="what-are-job-preparation-and-release-tasks"></a>Τι είναι οι εργασία προετοιμασίας και αφήστε εργασίες;

Πριν εκτελέσετε εργασίες μιας εργασίας, η εργασία προετοιμασίας εκτελείται σε όλους τους κόμβους υπολογισμού που έχει προγραμματιστεί να εκτελεστεί τουλάχιστον μία εργασία. Μόλις ολοκληρωθεί η εργασία, την εργασία κυκλοφορίας εκτελείται σε κάθε κόμβο στο χώρο συγκέντρωσης που εκτελεστεί τουλάχιστον μία εργασία. Όπως με κανονική δέσμη εργασίες, μπορείτε να καθορίσετε μια γραμμή εντολών για να ενεργοποιηθεί όταν εκτελείται μια εργασία προετοιμασίας ή την έκδοση του έργου.

Εργασίες προετοιμασίας και την έκδοση προσφέρουν οικείες δέσμη εργασιών δυνατότητες όπως η λήψη του αρχείου ([αρχεία πόρων][net_job_prep_resourcefiles]), αυξημένα εκτέλεσης, προσαρμοσμένο περιβάλλον μεταβλητές, μέγιστο εκτέλεσης διάρκειας, των επαναλήψεων και ώρα διατήρησης αρχείου.

Στις ακόλουθες ενότητες, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε το [JobPreparationTask] [ net_job_prep] και [JobReleaseTask] [ net_job_release] κλάσεις βρίσκονται στη [Δέσμη .NET] [ api_net] βιβλιοθήκη.

> [AZURE.TIP] Εργασίες προετοιμασίας και την έκδοση είναι ιδιαίτερα χρήσιμα στα περιβάλλοντα "κοινή χρήση του χώρου συγκέντρωσης", στο οποίο χώρου συγκέντρωσης κόμβους υπολογιστικών εξακολουθεί να υπάρχει ανάμεσα σε εργασία εκτελείται και χρησιμοποιείται από πολλές εργασίες.

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Πότε να χρησιμοποιήσετε εργασία προετοιμασίας και αφήστε εργασίες

Εργασία προετοιμασίας και τις εργασίες από την έκδοση είναι κατάλληλες για τις ακόλουθες περιπτώσεις:

**Λήψη δεδομένων κοινών εργασιών**

Μαζικές εργασίες απαιτούν συχνά ένα κοινό σύνολο δεδομένων ως είσοδο για τις εργασίες του έργου. Για παράδειγμα, σε ημερήσια υπολογισμών ανάλυσης κινδύνου, αγορά δεδομένα είναι συγκεκριμένη εργασία, ακόμη κοινά σε όλες τις εργασίες της εργασίας. Αυτά τα δεδομένα αγοράς, συχνά πολλών gigabyte μέγεθος, θα πρέπει να λαμβάνονται στο κάθε κόμβος μόνο μία φορά, έτσι ώστε να το χρησιμοποιήσετε οποιαδήποτε εργασία που εκτελείται στον κόμβο. Χρησιμοποιήστε μια **εργασία προετοιμασίας** για να κάνετε λήψη αυτών των δεδομένων σε κάθε κόμβο πριν από την εκτέλεση της εργασίας της άλλες εργασίες.

**Διαγραφή εξόδου έργων και εργασιών**

Σε ένα περιβάλλον "κοινή χρήση του χώρου συγκέντρωσης", όπου κόμβους υπολογιστικών χώρου συγκέντρωσης δεν είναι παροπλισμού μεταξύ των εργασιών, ίσως χρειαστεί να διαγράψετε δεδομένα εργασία μεταξύ εκτελείται. Ίσως χρειαστεί να εξοικονομήσετε χώρο στο δίσκο σε τους κόμβους ή ικανοποιήσετε πολιτικών ασφάλειας της εταιρείας σας. Χρησιμοποιήστε μια **έκδοση εργασία** για να διαγράψετε δεδομένα που έχει ληφθεί από μια εργασία προετοιμασίας ή που δημιουργούνται κατά την εκτέλεση εργασιών.

**Διατήρηση καταγραφής**

Ενδέχεται να θέλετε να διατηρήσετε ένα αντίγραφο των αρχείων καταγραφής που δημιουργούν τις εργασίες σας ή ίσως παρουσιάσει σφάλμα αρχεία ένδειξης που μπορεί να δημιουργηθεί από αποτυχίας εφαρμογές. Χρησιμοποιήστε μια **έκδοση εργασία** σε αυτές τις περιπτώσεις να συμπιέσετε και αποστολή αυτών των δεδομένων στο χώρο [Αποθήκευσης Azure] [ azure_storage] λογαριασμού.

>[AZURE.TIP] Ένας άλλος τρόπος για να διατηρηθούν αρχεία καταγραφής και άλλες εργασίας και εργασίας εξόδου δεδομένων είναι να χρησιμοποιήσετε τη βιβλιοθήκη [Συμβάσεις αρχείων δέσμης Azure](batch-task-output.md) .

## <a name="job-preparation-task"></a>Εργασία προετοιμασίας

Πριν από την εκτέλεση των εργασιών του έργου, δέσμη εκτελεί την εργασία προετοιμασίας σε κάθε κόμβο υπολογισμού που έχει προγραμματιστεί για να εκτελέσετε μια εργασία. Από προεπιλογή, η υπηρεσία δέσμη χρειάζεται να περιμένει για την εργασία προετοιμασίας για να ολοκληρωθούν πριν από την εκτέλεση των εργασιών που έχει προγραμματιστεί να εκτελεστεί στον κόμβο. Ωστόσο, μπορείτε να ρυθμίσετε την υπηρεσία δεν χρειάζεται να περιμένετε. Εάν ο κόμβος επανεκκίνηση του, εκτελεί ξανά την εργασία προετοιμασίας, αλλά μπορείτε επίσης να απενεργοποιήσετε αυτήν τη συμπεριφορά.

Η εργασία προετοιμασίας εκτελείται μόνο σε κόμβους που έχουν προγραμματιστεί για να εκτελέσετε μια εργασία. Αυτό αποτρέπει την άσκοπη εκτέλεση μια εργασία προετοιμασίας στην περίπτωση που ένας κόμβος δεν έχει ανατεθεί μια εργασία. Αυτό μπορεί να συμβεί όταν ο αριθμός των εργασιών για μια εργασία είναι μικρότερος από τον αριθμό των κόμβους σε ένα χώρο συγκέντρωσης. Εφαρμόζεται επίσης όταν [εκτέλεσης ταυτόχρονες εργασίας](batch-parallel-node-tasks.md) είναι ενεργοποιημένη, έτσι ορισμένοι κόμβοι αδράνειας εάν η μέτρηση εργασίας είναι χαμηλότερη από το συνολικό πιθανές ταυτόχρονες εργασίες. Εκτελώντας δεν την εργασία προετοιμασίας στην αδράνειας κόμβοι, μπορείτε να ξοδέψετε λιγότερο χρήματα για χρεώσεις μεταφορά δεδομένων.

> [AZURE.NOTE] [JobPreparationTask] [ net_job_prep_cloudjob] διαφέρει από [CloudPool.StartTask] [ pool_starttask] σε που εκτελεί την JobPreparationTask στην αρχή κάθε εργασίας, ενώ το StartTask εκτελείται μόνο όταν ένα κόμβος πρώτα ενώνει χώρου συγκέντρωσης ή επανεκκίνηση του.

## <a name="job-release-task"></a>Αποδέσμευση εργασίας έργου

Όταν μια εργασία έχει σημανθεί ως ολοκληρωμένο, την τελική έκδοση εργασία εκτελείται σε κάθε κόμβο στο χώρο συγκέντρωσης που εκτελεστεί τουλάχιστον μία εργασία. Μπορείτε να επισημάνετε μια εργασία ως ολοκληρωμένη με την έκδοση μια αίτηση τερματισμού. Η υπηρεσία δέσμη, στη συνέχεια, ορίζει την κατάσταση εργασίας για να *τον τερματισμό*καταγγελία οποιαδήποτε ενεργό ή δεν εκτελείται εργασίες που σχετίζονται με την εργασία και εκτελεί την εργασία κυκλοφορίας. Η εργασία μετακινείται στη συνέχεια, στην κατάσταση *ολοκληρώθηκε* .

> [AZURE.NOTE] Διαγραφή εργασίας εκτελεί επίσης την έκδοση εργασία. Ωστόσο, εάν μια εργασία έχει ήδη τερματιστεί, η αποδέσμευση εργασίας δεν εκτελείται δεύτερη φορά εάν η εργασία έχει διαγραφεί αργότερα.

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Προετοιμασία έργου και αφήστε τις εργασίες με δέσμη .NET

Για να χρησιμοποιήσετε μια εργασία προετοιμασίας, αντιστοίχιση ενός [JobPreparationTask] [ net_job_prep] αντικειμένου του έργου σας [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] την ιδιότητα. Ομοίως, προετοιμασία ενός [JobReleaseTask] [ net_job_release] και να την εκχωρήσετε σε του έργου σας [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] ιδιοτήτων για να ορίσετε την εργασία Αποδέσμευση εργασίας.

Σε αυτό το τμήμα κώδικα, `myBatchClient` είναι μια παρουσία του [BatchClient][net_batch_client], και `myPool` είναι έναν υπάρχοντα χώρο συγκέντρωσης μέσα στο λογαριασμό δέσμης.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Όπως προαναφέρθηκε, την αποδέσμευση εργασίας εκτελείται όταν μια εργασία έχει διακοπεί ή διαγραφεί. Τερματισμός μιας εργασίας με [JobOperations.TerminateJobAsync][net_job_terminate]. Διαγραφή μιας εργασίας με [JobOperations.DeleteJobAsync][net_job_delete]. Συνήθως τερματισμός ή διαγραφή μιας εργασίας όταν τις εργασίες ολοκληρωθούν ή όταν έχει φτάσει ένα χρονικό όριο που έχετε ορίσει.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Δείγμα κώδικα στην GitHub

Για να δείτε εργασία προετοιμασίας και αφήστε εργασίες στην πράξη, ανατρέξτε στο θέμα το [JobPrepRelease] [ job_prep_release_sample] δείγμα έργου σε GitHub. Αυτή η εφαρμογή κονσόλας κάνει τα εξής:

1. Δημιουργεί ένα χώρο συγκέντρωσης με δύο κόμβους "μικρές".
2. Δημιουργεί μια εργασία με εργασία προετοιμασίας, την έκδοση και τυπικές εργασίες.
3. Εκτελεί την εργασία προετοιμασίας εργασία, που γράφει πρώτα το Αναγνωριστικό κόμβου σε ένα αρχείο κειμένου σε έναν κόμβο "κοινόχρηστο" κατάλογο.
4. Εκτελεί μια εργασία σε κάθε κόμβο που γράφει το Αναγνωριστικό εργασίας στο ίδιο αρχείο κειμένου.
5. Μόλις ολοκληρωθούν όλες οι εργασίες (ή το χρονικό όριο έχει φτάσει), εκτυπώνει τα περιεχόμενα του αρχείου κειμένου κάθε κόμβο στην κονσόλα.
6. Όταν ολοκληρωθεί η εργασία, εκτελεί την τελική έκδοση εργασία για να διαγράψετε το αρχείο από τον κόμβο.
6. Εκτυπώνει τους κωδικούς Έξοδος από την προετοιμασία και την έκδοση εργασιών για κάθε κόμβο στον οποίο εκτελείται.
7. Εκτέλεση παύσεις για να επιτρέψετε επιβεβαίωση της εργασίας ή/και το χώρο συγκέντρωσης διαγραφής.

Έξοδος από το δείγμα εφαρμογής είναι παρόμοιο με το εξής:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] Λόγω το μεταβλητής δημιουργίας και ώρα έναρξης της κόμβοι σε ένα νέο χώρο συγκέντρωσης (ορισμένοι κόμβοι είναι έτοιμα για εργασίες πριν από άλλους χρήστες), μπορείτε να δείτε διαφορετικά εξόδου. Συγκεκριμένα, επειδή οι εργασίες ολοκλήρωσης γρήγορα, μία από το χώρο συγκέντρωσης κόμβοι είναι δυνατό να εκτελεστεί όλες οι εργασίες του έργου. Αν συμβεί αυτό, θα παρατηρήσετε ότι η εργασία προετοιμασία και εργασίες κυκλοφορίας δεν υπάρχουν για τον κόμβο που εκτελείται χωρίς εργασίες.

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Έλεγχος εργασία προετοιμασίας και την έκδοση εργασίες στην πύλη του Azure

Όταν εκτελείτε το δείγμα εφαρμογής, μπορείτε να χρησιμοποιήσετε την [πύλη του Azure] [ portal] για να προβάλετε τις ιδιότητες της εργασίας και των εργασιών ή ακόμα και κάντε λήψη του αρχείου κοινόχρηστο κειμένου που τροποποιείται από τις εργασίες του έργου.

Το παρακάτω στιγμιότυπο οθόνης εμφανίζει το **blade εργασίες προετοιμασίας** στην πύλη του Azure μετά από έναν κύκλο από το δείγμα εφαρμογής. Μεταβείτε στις ιδιότητες *JobPrepReleaseSampleJob* αφού έχετε ολοκληρώσει τις εργασίες σας (αλλά πριν από τη διαγραφή του έργου και το χώρο συγκέντρωσης) και κάντε κλικ στην επιλογή **εργασίες προετοιμασίας** ή **την έκδοση εργασίες** για να προβάλετε τις ιδιότητές τους.

![Εργασία προετοιμασίας ιδιότητες στην πύλη του Azure][1]

## <a name="next-steps"></a>Επόμενα βήματα

### <a name="application-packages"></a>Πακέτα εφαρμογών

Εκτός από την εργασία προετοιμασίας, μπορείτε επίσης να χρησιμοποιήσετε τη δυνατότητα [πακέτων εφαρμογών](batch-application-packages.md) της δέσμης για να προετοιμάσετε κόμβους υπολογιστικών για εκτέλεση εργασιών. Αυτή η δυνατότητα είναι ιδιαίτερα χρήσιμη για την ανάπτυξη εφαρμογών που δεν απαιτούν την εκτέλεση ενός προγράμματος εγκατάστασης, εφαρμογές που περιέχουν πολλά (100 +) αρχεία ή τις εφαρμογές που απαιτούν έλεγχο βασική έκδοση.

### <a name="installing-applications-and-staging-data"></a>Την εγκατάσταση εφαρμογών και ενδιάμεσου δεδομένων

Αυτή η καταχώρηση φόρουμ MSDN παρέχει μια επισκόπηση των διάφορες μεθόδους προετοιμασίας κόμβους σας για την εκτέλεση εργασιών:

[Την εγκατάσταση εφαρμογών και ενδιάμεσου δεδομένων στη δέσμη τον υπολογισμό κόμβοι][forum_post]

Συνταχθεί από ένα από τα μέλη της ομάδας δέσμη Azure, ασχολείται με διάφορες τεχνικές που μπορείτε να χρησιμοποιήσετε για την ανάπτυξη εφαρμογών και δεδομένων για τον υπολογισμό κόμβοι.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
