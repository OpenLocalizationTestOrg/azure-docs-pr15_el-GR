<properties
   pageTitle="Προκαλέσουν χάος σε ύφασμα υπηρεσία συμπλεγμάτων | Microsoft Azure"
   description="Χρήση σφαλμάτων εισαγωγής και σύμπλεγμα ανάλυσης υπηρεσίας API του Yammer για τη Διαχείριση χάος στο σύμπλεγμα."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="rsinha"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="motanv"/>

# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Προκαλέσουν ελέγχεται χάος σε ύφασμα υπηρεσία συμπλεγμάτων
Μεγάλης κλίμακας κατανεμημένα συστήματα, όπως οι υποδομής cloud είναι εγγενώς αναξιόπιστες. Azure Service ύφασμα επιτρέπει στους προγραμματιστές να γράψετε αξιόπιστων υπηρεσιών επάνω σε μια μη αξιόπιστη υποδομή. Για να γράψετε έναν ισχυρό υπηρεσίες, οι προγραμματιστές πρέπει να μπορούν να προκαλέσουν σφάλματα έναντι αυτής της υποδομής αναξιόπιστες για να ελέγξετε τη σταθερότητα των υπηρεσιών τους.

Η εισαγωγή σφαλμάτων και η υπηρεσία ανάλυσης σύμπλεγμα (γνωστό και ως την υπηρεσία ανάλυσης σφάλματος) δίνει στους προγραμματιστές τη δυνατότητα να προκαλέσουν σφάλμα ενέργειες για να ελέγξετε τις υπηρεσίες. Ωστόσο, προορισμού προσομοιωμένη σφάλματα σας βοηθήσουν να μόνο μέχρι στιγμής. Για να κρατήσετε περαιτέρω τη δοκιμή, μπορείτε να χρησιμοποιήσετε χάος.

Χάος προσομοιώνει συνεχή, διεμπλεκόμενο σφάλματα (φυσιολογική και ungraceful) σε όλο το σύμπλεγμα πάνω από το εκτεταμένο χρονικά διαστήματα. Αφού ρυθμίσετε χάος με τη χρέωση και το είδος των σφαλμάτων, μπορείτε να ξεκινήσετε ή να διακόψετε μέσω είτε C# API ή του PowerShell για τη δημιουργία σφάλματα σε σύμπλεγμα και την υπηρεσία.

Ενώ εκτελείται το χάος, υπολογίζονται διαφορετικά συμβάντα που καταγραφή της κατάστασης της εκτέλεσης τη χρονική στιγμή. Για παράδειγμα, μια ExecutingFaultsEvent περιέχει όλες τις βλάβες που εκτελούνται στο συγκεκριμένο διαδοχικών προσεγγίσεων. Μια ValidationFailedEvent περιέχει τις λεπτομέρειες μιας αποτυχίας που βρέθηκε κατά την επικύρωση σύμπλεγμα. Μπορείτε να ενεργοποιήσετε το API GetChaosReportAsync για να λάβετε την αναφορά της χάος εκτελείται.

## <a name="faults-induced-in-chaos"></a>Σφάλματα που προκαλούνται από την στο χάος
Χάος δημιουργεί σφάλματα σε ολόκληρο το σύμπλεγμα ύφασμα υπηρεσίας και συμπιέζει σφαλμάτων που είναι ορατές σε μήνες ή έτη σε μερικές ώρες. Ο συνδυασμός διεμπλεκόμενο σφαλμάτων με τη χρέωση υψηλή σφαλμάτων εντοπίζει τη γωνία περιπτώσεις που είναι διαφορετικά παραβλέψει. Αυτήν την άσκηση χάος οδηγεί σε μια σημαντική βελτίωση της ποιότητας κώδικα της υπηρεσίας.

Χάος υποδείξει σφάλματα από τις εξής κατηγορίες:

 - Επανεκκινήστε κόμβου
 - Επανεκκινήστε το ένα πακέτο ανάπτυξη κώδικα
 - Κατάργηση ενός αντιγράφου
 - Επανεκκινήστε ρεπλίκα
 - Μετακινήσετε μια κύρια ρεπλίκα (με δυνατότητα ρύθμισης παραμέτρων)
 - Μετακίνηση μιας δευτερεύουσας ρεπλίκα (με δυνατότητα ρύθμισης παραμέτρων)

Χάος εκτελείται σε πολλές διαδοχικές προσεγγίσεις. Κάθε διαδοχικών προσεγγίσεων αποτελείται από σφαλμάτων και σύμπλεγμα επικύρωσης για την καθορισμένη περίοδο. Μπορείτε να ρυθμίσετε το χρόνο για το σύμπλεγμα για τη σταθεροποίηση και για επικύρωση για να ολοκληρωθεί με επιτυχία. Εάν βρεθεί αποτυχία επικύρωσης σύμπλεγμα, χάος δημιουργεί και εξακολουθεί να εμφανίζεται μια ValidationFailedEvent με τη χρονική σήμανση UTC και τις λεπτομέρειες της αποτυχίας.

Για παράδειγμα, εξετάστε το ενδεχόμενο μια παρουσία χαοτικό που έχει οριστεί για να εκτελέσετε για μια ώρα με έως και τρεις ταυτόχρονες σφάλματα. Χάος υποδείξει τρεις σφάλματα και, στη συνέχεια, επαληθεύει την εύρυθμη λειτουργία συμπλέγματος. Το επαναλαμβάνεται έως το προηγούμενο βήμα μέχρι να είναι ρητά σταμάτησε μέσω του API StopChaosAsync ή μία ώρα μεταβιβάζει. Εάν το σύμπλεγμα γίνεται κατεστραμμένες σε οποιαδήποτε διαδοχικών προσεγγίσεων (δηλαδή, αυτό δεν σταθεροποίηση μέσα σε καθορισμένο χρονικό διάστημα), χάος δημιουργεί μια ValidationFailedEvent. Αυτό το συμβάν υποδεικνύει ότι κάτι που έχει υπερβεί λάθος και ενδέχεται να χρειάζεστε περαιτέρω έρευνα.

Στην τρέχουσα μορφή, χάος υποδείξει μόνο ασφαλείς σφάλματα. Αυτό σημαίνει ότι, κατά την απουσία εξωτερικών σφάλματα, μια απαρτίας απώλεια ή απώλεια δεδομένων προκύψει ποτέ.

## <a name="important-configuration-options"></a>Επιλογές σημαντικό ρύθμισης παραμέτρων
 - **TimeToRun**: συνολικός χρόνος που εκτελείται χάος πριν να ολοκληρωθεί με επιτυχία. Μπορείτε να διακόψετε χάος πριν από την εκτέλεση του για την περίοδο TimeToRun μέσω του API StopChaos.
 - **MaxClusterStabilizationTimeout**: το μέγιστο χρονικό διάστημα για να περιμένετε για το σύμπλεγμα για να γίνετε άρτιο πριν να ελέγξετε το ξανά. Η αναμονή είναι να μειωθεί η φόρτωση στο σύμπλεγμα ενώ το ανακτήσετε. Οι έλεγχοι που εκτελείται είναι:
    - Εάν το σύμπλεγμα υγείας είναι OK
    - Εάν την εύρυθμη λειτουργία των υπηρεσιών είναι OK
    - Εάν στη ρεπλίκα προορισμού ορίστε μέγεθος είναι δυνατό για τα διαμερίσματα υπηρεσίας
    - Ότι υπάρχει καμία ρεπλίκα InBuild
 - **MaxConcurrentFaults**: Ο μέγιστος αριθμός ταυτόχρονες σφαλμάτων που είναι που προκαλούνται από κάθε διαδοχικών προσεγγίσεων. Είναι η υψηλότερη η τιμή, τόσο πιο αυστηρές χάος. Το αποτέλεσμα πιο περίπλοκα ανακατευθύνσεις και οι συνδυασμοί μετάβασης. Χάος εγγυάται ότι, κατά την απουσία εξωτερικών σφάλματα, δεν υπάρχει απαρτίας απώλεια ή απώλεια δεδομένων, ανεξάρτητα από τον τρόπο υψηλή τιμή έχει αυτήν τη ρύθμιση παραμέτρων.
 - **EnableMoveReplicaFaults**: Ενεργοποιεί ή απενεργοποιεί το σφαλμάτων που προκαλούν το πρωτεύον ή δευτερεύον αντίγραφα για να μετακινήσετε. Αυτά τα σφάλματα είναι απενεργοποιημένες από προεπιλογή.
 - **WaitTimeBetweenIterations**: το χρονικό διάστημα για να περιμένετε μεταξύ των διαδοχικών προσεγγίσεων, αυτό σημαίνει ότι, μετά από μια round των σφαλμάτων και αντίστοιχες επικύρωσης.
 - **WaitTimeBetweenFaults**: το χρονικό διάστημα αναμονής μεταξύ των δύο διαδοχικές σφάλματα σε μια διαδοχικών προσεγγίσεων.

## <a name="how-to-run-chaos"></a>Πώς μπορείτε να εκτελέσετε χάος
**C#:**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```
**PowerShell:**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
