<properties 
    pageTitle="Παρακολούθηση προόδου εργασίας χρησιμοποιώντας .NET" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε κωδικό πρόγραμμα χειρισμού συμβάντων για την παρακολούθηση της προόδου εργασίας και στείλτε ενημερώσεις κατάστασης. Το δείγμα κώδικα είναι γραμμένο σε C# και χρησιμοποιεί το Media Services SDK για .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016"   
    ms.author="juliako"/>

# <a name="monitor-job-progress-using-net"></a>Παρακολούθηση προόδου εργασίας χρησιμοποιώντας .NET

> [AZURE.SELECTOR]
- [Πύλη](media-services-portal-check-job-progress.md)
- [.NET](media-services-check-job-progress.md)
- [ΥΠΌΛΟΙΠΟ](media-services-rest-check-job-progress.md)

Όταν εκτελείτε εργασίες, συχνά απαιτούν ένας τρόπος για να παρακολουθείτε την πρόοδο του έργου. Μπορείτε να ελέγξετε την πρόοδο καθορίζοντας ένα πρόγραμμα χειρισμού συμβάντων StateChanged (όπως περιγράφεται σε αυτό το θέμα) ή χρησιμοποιώντας ουρά Azure χώρου αποθήκευσης για την παρακολούθηση ειδοποιήσεων σχετικά με εργασίες Media Services (όπως περιγράφεται σε [αυτό](media-services-dotnet-check-job-progress-with-queues.md) το θέμα).

## <a name="define-statechanged-event-handler-to-monitor-job-progress"></a>Ορισμός πρόγραμμα χειρισμού συμβάντων StateChanged για την παρακολούθηση της προόδου εργασίας

Το ακόλουθο παράδειγμα κώδικα ορίζει το πρόγραμμα χειρισμού συμβάντων StateChanged. Σε αυτό το πρόγραμμα χειρισμού συμβάντων παρακολουθεί την πρόοδο εργασίας και σας παρέχει ενημερωμένο κατάσταση, ανάλογα με την κατάσταση. Ο κώδικας που ορίζει επίσης τη μέθοδο LogJobStop. Αυτή η μέθοδος Βοήθειας καταγράφει τις λεπτομέρειες του σφάλματος.

    private static void StateChanged(object sender, JobStateChangedEventArgs e)
    {
        Console.WriteLine("Job state changed event:");
        Console.WriteLine("  Previous state: " + e.PreviousState);
        Console.WriteLine("  Current state: " + e.CurrentState);
    
        switch (e.CurrentState)
        {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("********************");
                Console.WriteLine("Job is finished.");
                Console.WriteLine("Please wait while local tasks or downloads complete...");
                Console.WriteLine("********************");
                Console.WriteLine();
                Console.WriteLine();
                break;
            case JobState.Canceling:
            case JobState.Queued:
            case JobState.Scheduled:
            case JobState.Processing:
                Console.WriteLine("Please wait...\n");
                break;
            case JobState.Canceled:
            case JobState.Error:
                // Cast sender as a job.
                IJob job = (IJob)sender;
                // Display or log error details as needed.
                LogJobStop(job.Id);
                break;
            default:
                break;
        }
    }
    
    private static void LogJobStop(string jobId)
    {
        StringBuilder builder = new StringBuilder();
        IJob job = GetJob(jobId);
    
        builder.AppendLine("\nThe job stopped due to cancellation or an error.");
        builder.AppendLine("***************************");
        builder.AppendLine("Job ID: " + job.Id);
        builder.AppendLine("Job Name: " + job.Name);
        builder.AppendLine("Job State: " + job.State.ToString());
        builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
        builder.AppendLine("Media Services account name: " + _accountName);
        builder.AppendLine("Media Services account location: " + _accountLocation);
        // Log job errors if they exist.  
        if (job.State == JobState.Error)
        {
            builder.Append("Error Details: \n");
            foreach (ITask task in job.Tasks)
            {
                foreach (ErrorDetail detail in task.ErrorDetails)
                {
                    builder.AppendLine("  Task Id: " + task.Id);
                    builder.AppendLine("    Error Code: " + detail.Code);
                    builder.AppendLine("    Error Message: " + detail.Message + "\n");
                }
            }
        }
        builder.AppendLine("***************************\n");
        // Write the output to a local file and to the console. The template 
        // for an error output file is:  JobStop-{JobId}.txt
        string outputFile = _outputFilesFolder + @"\JobStop-" + JobIdAsFileName(job.Id) + ".txt";
        WriteToFile(outputFile, builder.ToString());
        Console.Write(builder.ToString());
    }
    
    private static string JobIdAsFileName(string jobID)
    {
        return jobID.Replace(":", "_");
    }



##<a name="next-step"></a>Επόμενο βήμα

Αναθεώρηση Media Services διαδικασίες εκμάθησης.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
