<properties
   pageTitle="Πώς μπορείτε να καλέσετε απώλεια δεδομένων στην υπηρεσία υπηρεσίες δομής | Microsoft Azure"
   description="Περιγράφει τον τρόπο χρήσης της απώλειας δεδομένων api"
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# <a name="how-to-invoke-data-loss-on-services"></a>Πώς μπορείτε να καλέσετε απώλεια δεδομένων στις υπηρεσίες

>[AZURE.WARNING] Αυτό το έγγραφο περιγράφουν πώς να προκαλέσει απώλεια δεδομένων στις υπηρεσίες σας και θα πρέπει να χρησιμοποιούνται με φειδώ.

## <a name="introduction"></a>Εισαγωγή
Μπορείτε να ανακαλέσετε απώλεια δεδομένων σε ένα διαμερίσματα της υπηρεσίας σας ύφασμα υπηρεσίας από StartPartitionDataLossAsync() κλήσης.  Αυτό το api χρησιμοποιεί το σφάλμα εισαγωγής και υπηρεσιών ανάλυσης για την εκτέλεση της εργασίας για να προκαλέσει συνθήκες απώλεια δεδομένων.

## <a name="using-the-fault-injection-and-analysis-service"></a>Χρήση των σφαλμάτων εισαγωγής και υπηρεσιών ανάλυσης

Η υπηρεσία ανάλυσης και σφαλμάτων εισαγωγής υποστηρίζει επί του παρόντος τα παρακάτω API του γραφήματος παρακάτω.  Στη δεξιά πλευρά του γραφήματος εμφανίζεται το αντίστοιχο cmdlet του PowerShell.  Ανατρέξτε στην τεκμηρίωση του msdn σε κάθε API για περισσότερες πληροφορίες σχετικά με κάθε μία.

|           C# API                    |         Cmdlet του PowerShell                      |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync] [dl]   |[Έναρξη-ServiceFabricPartitionDataLoss] [psdl]   |
|[StartPartitionQuorumLossAsync] [ql] |[Έναρξη-ServiceFabricPartitionQuorumLoss] [psql] |
|[StartPartitionRestartAsync] [rp]    |[Έναρξη-ServiceFabricPartitionRestart] [psrp]    |

## <a name="conceptual-overview-of-running-a-command"></a>Επισκόπηση της εκτέλεσης μιας εντολής

Η υπηρεσία ανάλυσης και σφαλμάτων εισαγωγής χρησιμοποιεί μια ασύγχρονης μοντέλο όπου μπορείτε ξεκινήσετε την εντολή με ένα API, αναφέρεται ως το API "Έναρξη" σε αυτό το έγγραφο και στη συνέχεια ελέγχει την πρόοδο αυτής της εντολής χρησιμοποιώντας μια "GetProgress" API μέχρι να φτάσει κατάσταση τερματικού ή μέχρι να την ακυρώσετε.
Για να ξεκινήσετε μια εντολή, καλέστε το API "Έναρξη" για το αντίστοιχο API.  Αυτό το API επιστρέφει όταν η υπηρεσία ανάλυσης και σφαλμάτων εισαγωγής έχει αποδεχτεί την πρόσκληση.  Ωστόσο, δεν δηλώνει πόσο έχει εκτελέσετε μια εντολή, ή ακόμα και εάν έχει αρχίσει ακόμα.  Για να ελέγξετε την πρόοδο μιας εντολής, καλέστε το API που αντιστοιχεί στο API "Έναρξη" προηγουμένως ονομάζονταν "GetProgress".  Το API "GetProgress" θα επιστρέψει ένα αντικείμενο που υποδεικνύει την τρέχουσα κατάσταση της εντολής μέσα την ιδιότητα μέλους.  Εκτελεί μια εντολή απεριόριστο χρονικό διάστημα μέχρι:

1.  Ολοκληρωθεί με επιτυχία.  Εάν καλέσετε "GetProgress" σε αυτό σε αυτήν την περίπτωση, θα ολοκληρωθεί κατάστασης του αντικειμένου προόδου.
2.  Συναντά μια λάθος.  Αν καλέσετε "GetProgress" σε αυτό σε αυτήν την περίπτωση, κατάσταση του αντικειμένου προόδου θα παρουσιάσει σφάλμα
3.  Μπορείτε να ακυρώσετε την έως το [CancelTestCommandAsync]  [ cancel] API ή [Διακοπή ServiceFabricTestCommand]  [ cancelps] cmdlet του PowerShell.  Αν καλέσετε "GetProgress" σε αυτό σε αυτήν την περίπτωση, κατάσταση του αντικειμένου προόδου θα ακυρώθηκε ή ForceCancelled, ανάλογα με το όρισμα που API.  Ανατρέξτε στην τεκμηρίωση για [CancelTestCommandAsync]  [ cancel] για περισσότερες λεπτομέρειες.


## <a name="details-of-running-a-command"></a>Λεπτομέρειες της εκτέλεσης μιας εντολής

Για να ξεκινήσετε μια εντολή, καλέστε το API Έναρξη με τα αναμενόμενα ορίσματα.  API Έναρξη όλα έχουν ένα όρισμα Guid με το όνομα operationId.  Που θα πρέπει να παρακολουθείτε από το όρισμα operationId, επειδή χρησιμοποιούνται για την παρακολούθηση της προόδου αυτής της εντολής.  Αυτό πρέπει να περάσουν στο "GetProgress" API για να παρακολουθήσετε την πρόοδο της εντολής.  Το operationId πρέπει να είναι μοναδικό.

Μετά την επιτυχή κλήση του API Έναρξη, το API GetProgress πρέπει να καλείται σε Επανάληψη μέχρι να ολοκληρωθεί η ιδιότητα State του αντικειμένου επιστρέφεται προόδου.  Όλα [του FabricTransientException]  [ fte] και του OperationCanceledException πρέπει να επαναληφθεί.
Όταν η εντολή έχει φτάσει κατάσταση τερματικού (ολοκληρώθηκε, Faulted ή ακυρώθηκε), την πρόοδο επιστρέφεται ιδιότητα του αντικειμένου αποτέλεσμα θα έχει πρόσθετες πληροφορίες.  Εάν η κατάσταση έχει ολοκληρωθεί, Result.SelectedPartition.PartitionId θα περιέχει το αναγνωριστικό διαμερισμάτων που επιλέξατε.  Result.Exception θα είναι null.  Εάν η κατάσταση είναι σφάλμα, Result.Exception θα έχει το λόγο για τον την διοχέτευση σφαλμάτων και υπηρεσιών ανάλυσης παρουσιάσει σφάλμα την εντολή.  Result.SelectedPartition.PartitionId θα έχει το αναγνωριστικό διαμερισμάτων που επιλέξατε.  Σε ορισμένες περιπτώσεις, την εντολή ενδέχεται να μην έχετε συνεχίζεται επαρκής για να επιλέξετε ένα διαμερίσματα.  Σε αυτή την περίπτωση, το ΑναγνωριστικόΔιαμερίσματος θα είναι 0.  Εάν η κατάσταση είναι ακυρώθηκε, Result.Exception θα είναι null.  Όπως συμβαίνει Faulted, Result.SelectedPartition.PartitionId θα έχει το αναγνωριστικό διαμερισμάτων που έχει επιλεγεί, αλλά εάν η εντολή δεν έχει συνεχίζεται επαρκής για να γίνει αυτό, θα είναι 0.  Ανατρέξτε επίσης να ανατρέξετε το παρακάτω δείγμα.

Το παρακάτω δείγμα κώδικα δείχνει πώς μπορείτε να ξεκινήσετε και, στη συνέχεια, ελέγξτε την πρόοδο σε μια εντολή για να προκαλέσει απώλεια δεδομένων σε ένα συγκεκριμένο διαμερίσματα.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε το PartitionSelector για να επιλέξετε μια τυχαία διαμερίσματα της μιας συγκεκριμένης υπηρεσίας:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Ιστορικό και περικοπή

Μετά μια εντολή έχει φτάσει κράτος terminal, τα μετα-δεδομένα θα παραμείνει στο σφάλμα εισαγωγής και υπηρεσιών ανάλυσης για ένα συγκεκριμένο χρονικό διάστημα, θα είναι δυνατή η κατάργηση του για να εξοικονομήσετε χώρο.  Εάν ονομάζεται "GetProgress" με το operationId μιας εντολής αφού έχει καταργηθεί, θα επιστρέψει μια FabricException με ένα κωδικός σφάλματος KeyNotFound.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
