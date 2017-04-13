<properties
   pageTitle="Σενάρια προσαρμοσμένου δοκιμών | Microsoft Azure"
   description="Μάθετε πώς να σταθεροποίηση έναντι φυσιολογική και ungraceful αποτυχίες των υπηρεσιών σας."
   services="service-fabric"
   documentationCenter=".net"
   authors="anmolah"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="anmola"/>

# <a name="simulate-failures-during-service-workloads"></a>Προσομοίωση αποτυχίες κατά τη διάρκεια της υπηρεσίας φόρτους εργασίας

Τα σενάρια με δυνατότητα δοκιμής σε ύφασμα υπηρεσίας Azure επιτρέπει στους προγραμματιστές να μην ανησυχείτε σχετικά με την αντιμετώπιση σφαλμάτων μεμονωμένα. Υπάρχουν σενάρια, ωστόσο, όπου μια ρητή παρεμβολή φόρτο εργασίας προγράμματος-πελάτη και αποτυχίες ενδέχεται να είναι απαραίτητο. Η παρεμβολή φόρτο εργασίας προγράμματος-πελάτη και σφάλματα εξασφαλίζει ότι η υπηρεσία στην πραγματικότητα εκτελεί κάποια ενέργεια όταν συμβαίνει αποτυχία. Δεδομένο το επίπεδο του στοιχείου ελέγχου που παρέχει με δυνατότητα δοκιμής, μπορεί να είναι σε ακριβή σημεία της εκτέλεσης φόρτο εργασίας. Αυτό πρόκληση σφάλματα σε διαφορετικές καταστάσεις στην εφαρμογή μπορεί να βρείτε σφαλμάτων και τη βελτίωση της ποιότητας.

## <a name="sample-custom-scenario"></a>Δείγμα προσαρμοσμένο σεναρίου
Αυτή η δοκιμή δείχνει ένα σενάριο που interleaves το φόρτο εργασίας επιχειρήσεις με [φυσιολογική και ungraceful αποτυχίες](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions). Θα πρέπει να είναι που προκαλούνται από τα σφάλματα στη μέση λειτουργίες υπηρεσίας ή υπολογισμού για καλύτερα αποτελέσματα.

Ας δούμε ένα παράδειγμα μιας υπηρεσίας που εκθέτει τέσσερις φόρτους εργασίας: A, B, C και D. κάθε αντιστοιχεί σε ένα σύνολο των ροών εργασιών και θα μπορούσε να είναι υπολογισμού, το χώρο αποθήκευσης ή συνδυασμό. Για λόγους απλούστευσης, θα σας θα συνοπτική το φόρτο εργασίας στο παράδειγμά μας. Τις διαφορετικές βλάβες εκτελεστεί σε αυτό το παράδειγμα είναι οι εξής:
  + RestartNode: Ungraceful σφαλμάτων για να προσομοιώσετε μια επανεκκίνηση του υπολογιστή.
  + RestartDeployedCodePackage: Παρουσιάζει σφάλμα Ungraceful σφαλμάτων για να προσομοιώσετε υπηρεσίας διεργασία κεντρικού υπολογιστή.
  + RemoveReplica: Φυσιολογική σφαλμάτων για να προσομοιώσετε κατάργησης αντιγράφου.
  + MovePrimary: Φυσιολογική σφαλμάτων για να προσομοιώσετε ρεπλίκα μετακινεί που ενεργοποιούνται από από τη μονάδα εξισορρόπησης φόρτου ύφασμα υπηρεσίας.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        // Replace these strings with the actual version for your cluster and application.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time to wait for a service to stabilize.
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want to execute.
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start the workload.
            var workloadTask = RunWorkloadAsync(workload);

            // While the task is running, induce faults into the service. They can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary.
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector, which will select a primary replica from the given service to test.
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run the selected random fault.
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate the health and stability of the service.
            await fabricClient.ServiceManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for the workload to finish successfully.
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.ClusterManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.ApplicationManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.ServiceManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.ServiceManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload.
        // Note that the faults induced while your service workload is running will
        // fault the primary service. Hence, you will need to reconnect to complete or check
        // the status of the workload.
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return workload;
    }
}
```
