<properties
   pageTitle="Αναφορά και έλεγχος εύρυθμης λειτουργίας με Azure Service ύφασμα | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να στείλετε αναφορές εύρυθμης λειτουργίας από την υπηρεσία κώδικα και πώς μπορείτε να ελέγξετε την εύρυθμη λειτουργία της υπηρεσίας σας, χρησιμοποιώντας τα εργαλεία παρακολούθησης εύρυθμης λειτουργίας που παρέχει το Azure Service ύφασμα."
   services="service-fabric"
   documentationCenter=".net"
   authors="toddabel"
   manager="mfussell"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/06/2016"
   ms.author="toddabel"/>

# <a name="report-and-check-service-health"></a>Αναφορά και έλεγχος εύρυθμης λειτουργίας υπηρεσιών
Όταν τις υπηρεσίες αντιμετωπίσετε προβλήματα, τη δυνατότητα να απαντούν σε και διόρθωση συμβάντων και ΡΕΥΜΑΤΟΣ εξαρτάται από τη δυνατότητα για να εντοπίσετε γρήγορα τα προβλήματα. Εάν αναφέρετε προβλήματα και αποτυχίες στη Διαχείριση εύρυθμης λειτουργίας του Azure Service ύφασμα από την υπηρεσία κώδικα, μπορείτε να χρησιμοποιήσετε τυπική εύρυθμης λειτουργίας εργαλεία που παρέχει ύφασμα υπηρεσίας για να ελέγξετε την κατάσταση εύρυθμης λειτουργίας παρακολούθησης.

Υπάρχουν δύο τρόποι που μπορείτε να αναφέρετε εύρυθμης λειτουργίας από την υπηρεσία:

- Χρήση αντικειμένων [διαμερίσματα](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.aspx) ή [CodePackageActivationContext](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.aspx) .  
Μπορείτε να χρησιμοποιήσετε το `Partition` και `CodePackageActivationContext` αντικειμένων για να αναφέρετε την εύρυθμη λειτουργία των στοιχείων που αποτελούν μέρος το τρέχον περιβάλλον. Για παράδειγμα, ένας κώδικας που εκτελείται ως μέρος ενός αντιγράφου να αναφέρετε εύρυθμης λειτουργίας μόνο σε συγκεκριμένη ρεπλίκα, τα διαμερίσματα που ανήκει σε και την εφαρμογή που είναι μέρος του.

- Χρήση `FabricClient`.   
Μπορείτε να χρησιμοποιήσετε `FabricClient` για την αναφορά υγεία από τον κώδικα υπηρεσίας εάν το σύμπλεγμα δεν είναι [ασφαλής](service-fabric-cluster-security.md) ή εάν εκτελείται η υπηρεσία με δικαιώματα διαχειριστή. Αυτό δεν θα είναι αληθής στις περισσότερες περιπτώσεις ρεαλιστικό. Με `FabricClient`, μπορείτε να το αναφέρετε εύρυθμης λειτουργίας σε οποιαδήποτε οντότητα που αποτελεί μέρος του συμπλέγματος. Ιδανικά, ωστόσο, κωδικός υπηρεσίας θα πρέπει να στείλετε μόνο αναφορές που σχετίζονται με το δικό της εύρυθμης λειτουργίας.

Σε αυτό το άρθρο σάς καθοδηγεί σε ένα παράδειγμα ότι οι αναφορές εύρυθμης λειτουργίας από την υπηρεσία κώδικα. Το παράδειγμα εμφανίζει επίσης πώς τα εργαλεία που παρέχει ύφασμα υπηρεσίας μπορεί να χρησιμοποιηθεί για να ελέγξετε την κατάσταση εύρυθμης λειτουργίας. Σε αυτό το άρθρο προορίζονται για να είναι μια σύντομη εισαγωγή της εύρυθμης λειτουργίας παρακολούθησης δυνατοτήτων της υπηρεσίας ύφασμα. Για πιο λεπτομερείς πληροφορίες, μπορείτε να διαβάσετε τη σειρά αναλυτικά άρθρα σχετικά με την εύρυθμη λειτουργία που ξεκινούν με τη σύνδεση στο τέλος αυτού του άρθρου.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Πρέπει να έχετε εγκαταστήσει τα εξής:

   * Visual Studio 2015
   * Υπηρεσία ύφασμα SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Για να δημιουργήσετε ένα τοπικό ασφαλούς αποκλίσεις σύμπλεγμα
- Ανοίξτε το PowerShell με δικαιώματα διαχειριστή και εκτελέστε τις ακόλουθες εντολές.

![Εντολές που σας δείχνουν πώς μπορείτε να δημιουργήσετε ένα σύμπλεγμα ασφαλούς ανάπτυξης](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Για να αναπτύξετε μια εφαρμογή και να ελέγξετε την εύρυθμη λειτουργία

1. Ανοίξτε το Visual Studio ως διαχειριστής.

2. Δημιουργήστε ένα έργο χρησιμοποιώντας το πρότυπο **Βάσει της κατάστασης υπηρεσίας** .

    ![Δημιουργία μιας εφαρμογής υπηρεσίας υφάσματος με την υπηρεσία βάσει της κατάστασης](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)

3. Πατήστε το πλήκτρο **F5** για να εκτελέσετε την εφαρμογή σε κατάσταση εντοπισμού σφαλμάτων. Η εφαρμογή πρόκειται να αναπτυχθούν στο τοπικό σύμπλεγμα.

4. Μετά την εκτέλεση της εφαρμογής, δεξί κλικ στο εικονίδιο Διαχείριση συμπλεγμάτων τοπική στην περιοχή ειδοποιήσεων και επιλέξτε **Διαχείριση τοπικό σύμπλεγμα** από το μενού συντόμευσης για να ανοίξετε την Εξερεύνηση ύφασμα υπηρεσίας.

    ![Ανοίξτε την Εξερεύνηση ύφασμα υπηρεσίας από την περιοχή ειδοποιήσεων](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)

5. Την εύρυθμη λειτουργία της εφαρμογής θα πρέπει να εμφανίζονται όπως αυτή η εικόνα. Προς το παρόν, η εφαρμογή πρέπει να είναι σε καλή κατάσταση χωρίς σφάλματα.

    ![Καλή κατάσταση εφαρμογής στην Εξερεύνηση ύφασμα υπηρεσίας](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)

6. Μπορείτε επίσης να ελέγξετε την εύρυθμη λειτουργία με χρήση του PowerShell. Μπορείτε να χρησιμοποιήσετε ```Get-ServiceFabricApplicationHealth``` για να ελέγξετε την εύρυθμη λειτουργία της εφαρμογής και μπορείτε να χρησιμοποιήσετε το ```Get-ServiceFabricServiceHealth``` για να ελέγξετε την εύρυθμη λειτουργία της υπηρεσίας. Η αναφορά εύρυθμης λειτουργίας για την ίδια εφαρμογή σε PowerShell είναι σε αυτήν την εικόνα.

    ![Καλή εφαρμογή σε PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Για να προσθέσετε προσαρμοσμένα εύρυθμης λειτουργίας συμβάντα σας κωδικός υπηρεσίας
Τα πρότυπα του έργου ύφασμα υπηρεσίας στο Visual Studio περιέχουν δείγματα κώδικα. Τα ακόλουθα βήματα δείχνουν πώς μπορείτε να αναφέρετε εύρυθμης λειτουργίας προσαρμοσμένα συμβάντα από την υπηρεσία κώδικα. Αυτές οι αναφορές θα εμφανιστεί αυτόματα στα τυπική εργαλεία για την παρακολούθηση της εύρυθμης λειτουργίας που παρέχει ύφασμα υπηρεσία, όπως Explorer ύφασμα υπηρεσίας, η προβολή Azure πύλης εύρυθμης λειτουργίας και PowerShell.

1. Ανοίξτε ξανά την εφαρμογή που δημιουργήσατε προηγουμένως στο Visual Studio, ή να δημιουργήσετε μια νέα εφαρμογή χρησιμοποιώντας το πρότυπο Visual Studio **Βάσει της κατάστασης υπηρεσίας** .

2. Ανοίξτε το αρχείο Stateful1.cs και να βρείτε το `myDictionary.TryGetValueAsync` καλέσετε το `RunAsync` μέθοδο. Μπορείτε να δείτε ότι αυτή η μέθοδος επιστρέφει μια `result` που περιέχει την τρέχουσα αξία του μετρητή, επειδή είναι η λογική κλειδιών σε αυτήν την εφαρμογή για να διατηρήσετε μια καταμέτρηση εκτελείται. Εάν έχει μια πραγματική εφαρμογή και η έλλειψη αποτέλεσμα αντιπροσωπεύει μια αποτυχία, που θα θέλετε να επισημάνετε το συμβάν.

3. Για να αναφέρετε ένα συμβάν εύρυθμης λειτουργίας, όταν η έλλειψη αποτέλεσμα αντιπροσωπεύει μια αποτυχία, προσθέστε τα παρακάτω βήματα.

    μια. Προσθήκη του `System.Fabric.Health` χώρο ονομάτων με το αρχείο Stateful1.cs.

    ```csharp
    using System.Fabric.Health;
    ```

    β. Προσθέστε τον ακόλουθο κώδικα μετά το `myDictionary.TryGetValueAsync` κλήσεων

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Αναφέρουμε ρεπλίκα εύρυθμης λειτουργίας, επειδή είναι που αναφέρθηκε από μια υπηρεσία κατάστασης. Το `HealthInformation` παραμέτρου αποθηκεύει πληροφορίες σχετικά με το ζήτημα εύρυθμης λειτουργίας που είναι αυτό που αναφέρεται.

    Αν είχατε δημιουργήσει χωρίς κατάσταση υπηρεσίας, χρησιμοποιήστε τον ακόλουθο κώδικα

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```

4. Εάν η υπηρεσία σας εκτελείται με δικαιώματα διαχειριστή ή εάν το σύμπλεγμα δεν είναι [ασφαλής](service-fabric-cluster-security.md), μπορείτε επίσης να χρησιμοποιήσετε `FabricClient` για αναφορά εύρυθμης λειτουργίας, όπως φαίνεται στα παρακάτω βήματα.  

    μια. Δημιουργία του `FabricClient` παρουσίας μετά το `var myDictionary` δήλωσης.

    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```

    β. Προσθέστε τον ακόλουθο κώδικα μετά το `myDictionary.TryGetValueAsync` κλήσεων.

    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.ServiceInitializationParameters.PartitionId,
            this.ServiceInitializationParameters.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```

5. Ας προσομοιώσετε αυτό το σφάλμα και δείτε εμφανίζονται στα εργαλεία εύρυθμης λειτουργίας παρακολούθησης. Για να προσομοιώσετε την αποτυχία, σχολιάζετε στην πρώτη γραμμή στον κώδικα αναφορών εύρυθμης λειτουργίας που προσθέσατε προηγουμένως. Αφού σχολιάσετε στην πρώτη γραμμή, ο κώδικας θα μοιάζει το παρακάτω παράδειγμα.

    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
 Αυτός ο κώδικας θα εφαρμόζεται τώρα αυτήν την αναφορά εύρυθμης λειτουργίας κάθε φορά που `RunAsync` εκτελεί την. Αφού κάνετε την αλλαγή, πατήστε το πλήκτρο **F5** για να εκτελέσετε την εφαρμογή.

6. Μετά την εκτέλεση της εφαρμογής, ανοίξτε την Εξερεύνηση ύφασμα υπηρεσίας για να ελέγξετε την εύρυθμη λειτουργία της εφαρμογής. Αυτήν τη στιγμή, υπηρεσία ύφασμα Explorer θα σας δείξουν ότι η εφαρμογή είναι κατεστραμμένη. Αυτό είναι λόγω του σφάλματος που αναφέρθηκε από τον κώδικα που έχουμε προσθέσει προηγουμένως.

    ![Κατεστραμμένη εφαρμογή στην Εξερεύνηση ύφασμα υπηρεσίας](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)

7. Εάν επιλέξετε την κύρια ρεπλίκα στην προβολή δέντρου του Explorer ύφασμα υπηρεσιών, θα δείτε ότι η **Κατάσταση εύρυθμης λειτουργίας** δηλώνει σφάλμα, πολύ. Εξερεύνηση ύφασμα υπηρεσίας εμφανίζει επίσης τις λεπτομέρειες αναφορά εύρυθμης λειτουργίας που έχουν προστεθεί στο το `HealthInformation` παραμέτρου στον κώδικα. Μπορείτε να δείτε τις ίδιες αναφορές εύρυθμης λειτουργίας στο PowerShell και την πύλη του Azure.

    ![Εύρυθμη λειτουργία ρεπλίκα στην Εξερεύνηση ύφασμα υπηρεσίας](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Αυτή η αναφορά θα παραμείνει στη Διαχείριση εύρυθμης λειτουργίας μέχρι αντικαθίσταται από μια άλλη αναφορά ή μέχρι να διαγραφεί αυτή η ρεπλίκα. Επειδή δεν θα σας έχει ορίσει `TimeToLive` για αυτήν την αναφορά εύρυθμης λειτουργίας στο το `HealthInformation` αντικείμενο, η αναφορά δεν λήγει ποτέ.

Συνιστάται να ότι εύρυθμης λειτουργίας θα πρέπει να αναφέρεται σε το πιο λεπτομερές επίπεδο, το οποίο σε αυτήν την περίπτωση είναι η ρεπλίκα. Μπορείτε επίσης να αναφέρετε εύρυθμης λειτουργίας στην `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Για αναφορά εύρυθμης λειτουργίας σε `Application`, `DeployedApplication`, και `DeployedServicePackage`, χρησιμοποιήστε `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Επόμενα βήματα
[Βαθύ κατάρρευση στην υπηρεσία ύφασμα εύρυθμης λειτουργίας](service-fabric-health-introduction.md)
