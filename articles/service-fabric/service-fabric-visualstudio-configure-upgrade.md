<properties
   pageTitle="Ρύθμιση παραμέτρων την αναβάθμιση μιας εφαρμογής υπηρεσίας ύφασμα | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους των ρυθμίσεων για την αναβάθμιση μιας εφαρμογής υπηρεσίας υφάσματος με χρήση του Microsoft Visual Studio."
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/29/2016"
   ms.author="cawa" />

# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Ρύθμιση παραμέτρων την αναβάθμιση μιας εφαρμογής υπηρεσίας ύφασμα στο Visual Studio

Εργαλεία του Visual Studio για Azure Service ύφασμα παρέχουν υποστήριξη αναβάθμισης για τη δημοσίευση τοπική ή απομακρυσμένη συμπλεγμάτων. Υπάρχουν δύο πλεονεκτήματα για την αναβάθμιση σε νεότερη έκδοση αντί για αντικατάστασης της εφαρμογής κατά τη δοκιμή και εντοπισμός σφαλμάτων την εφαρμογή σας:

- Τα δεδομένα των εφαρμογών δεν θα χαθούν κατά την αναβάθμιση.
- Διαθεσιμότητα παραμένει υψηλή ώστε δεν υπάρχει Διακοπή υπηρεσίας κατά την αναβάθμιση, εάν υπάρχουν αρκετές παρουσιών της υπηρεσίας που εκτείνεται αναβάθμισης τομείς.

Δοκιμές μπορεί να εκτελεστεί σε σχέση με μια εφαρμογή, ενώ είναι γίνεται αναβάθμιση.

## <a name="parameters-needed-to-upgrade"></a>Παράμετροι που απαιτείται για την αναβάθμιση

Μπορείτε να επιλέξετε από δύο τύπους ανάπτυξης: κανονική ή την αναβάθμιση. Μια κανονική ανάπτυξη διαγράφει τις προηγούμενες πληροφορίες ανάπτυξης και δεδομένων στο σύμπλεγμα, ενώ μια ανάπτυξη του αναβάθμισης διατηρεί το. Κατά την αναβάθμιση μιας εφαρμογής υπηρεσίας ύφασμα στο Visual Studio, πρέπει να παρέχουν οι παράμετροι αναβάθμισης της εφαρμογής και την εύρυθμη λειτουργία ελέγχου πολιτικές. Οι παράμετροι αναβάθμισης της εφαρμογής σας βοηθήσει να ελέγχετε την αναβάθμιση, ενώ πολιτικές ελέγχου εύρυθμης λειτουργίας καθορίζουν εάν η αναβάθμιση ολοκληρώθηκε με επιτυχία. Ανατρέξτε στο θέμα [αναβάθμισης εφαρμογής υπηρεσίας ύφασμα: αναβάθμιση παράμετροι](service-fabric-application-upgrade-parameters.md) για περισσότερες λεπτομέρειες.

Υπάρχουν τρεις λειτουργίες αναβάθμισης: *εγγεγραμμένων*, *UnmonitoredAuto*και *UnmonitoredManual*.

  - Μια αναβάθμιση εγγεγραμμένων αυτοματοποιεί την αναβάθμιση και έλεγχο εύρυθμης λειτουργίας της εφαρμογής.

  - Αναβάθμιση UnmonitoredAuto αυτοματοποιεί την αναβάθμιση, αλλά παραλείπει τον έλεγχο εύρυθμης λειτουργίας της εφαρμογής.

  - Όταν κάνετε αναβάθμιση UnmonitoredManual, πρέπει να αναβαθμίσετε με μη αυτόματο τρόπο κάθε αναβάθμισης τομέα.

Κάθε λειτουργία αναβάθμισης απαιτεί διαφορετικά σύνολα των παραμέτρων. Ανατρέξτε στο θέμα [εφαρμογή αναβάθμισης παραμέτρους](service-fabric-application-upgrade-parameters.md) για να μάθετε περισσότερα σχετικά με τις διαθέσιμες επιλογές αναβάθμισης.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Αναβάθμιση μιας εφαρμογής υπηρεσίας ύφασμα στο Visual Studio

Εάν χρησιμοποιείτε το Visual Studio υπηρεσίας ύφασμα εργαλεία για την αναβάθμιση μιας εφαρμογής υπηρεσίας ύφασμα, μπορείτε να καθορίσετε μια διαδικασία δημοσίευσης για να αναβάθμισης και όχι μια κανονική ανάπτυξη, επιλέγοντας το πλαίσιο ελέγχου να **αναβαθμίσετε την εφαρμογή** .

### <a name="to-configure-the-upgrade-parameters"></a>Για να ρυθμίσετε τις παραμέτρους αναβάθμισης

1. Κάντε κλικ στο κουμπί **Ρυθμίσεις** δίπλα στο πλαίσιο ελέγχου. Εμφανίζεται το παράθυρο διαλόγου **Επεξεργασία αναβάθμιση παράμετροι** . Παράθυρο διαλόγου **Επεξεργασία αναβάθμιση παράμετροι** υποστηρίζει τις λειτουργίες αναβάθμισης εγγεγραμμένων, UnmonitoredAuto και UnmonitoredManual.

2. Επιλέξτε τη λειτουργία αναβάθμισης που θέλετε να χρησιμοποιήσετε και, στη συνέχεια, συμπληρώστε πλέγμα παραμέτρου.

    Κάθε παράμετρο έχει προεπιλεγμένες τιμές. Η προαιρετική παράμετρος *DefaultServiceTypeHealthPolicy* χρειάζονται μια κατακερματισμός εισαγωγής πίνακα. Ακολουθεί ένα παράδειγμα της κατακερματισμός εισαγωγής μορφοποίησης πίνακα για *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* είναι μια άλλη προαιρετική παράμετρος που τίθεται σε μια κατακερματισμός εισαγωγής πίνακα με την εξής μορφή:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Ακολουθεί ένα παράδειγμα πραγματικής διάρκειας ζωής:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. Εάν επιλέξετε τη λειτουργία αναβάθμισης UnmonitoredManual, πρέπει να ξεκινήσετε με μη αυτόματο τρόπο μια κονσόλα του PowerShell για να συνεχίσετε και να ολοκληρώσετε τη διαδικασία αναβάθμισης. Αναφορά σε [αναβάθμισης εφαρμογής υπηρεσίας ύφασμα: θέματα για προχωρημένους](service-fabric-application-upgrade-advanced.md) για να μάθετε τον τρόπο μη αυτόματης αναβάθμιση λειτουργεί.

## <a name="upgrade-an-application-by-using-powershell"></a>Αναβάθμιση μιας εφαρμογής με χρήση του PowerShell

Μπορείτε να χρησιμοποιήσετε το cmdlet του PowerShell για να αναβαθμίσετε μια εφαρμογή υπηρεσίας ύφασμα. Ανατρέξτε στο θέμα [εφαρμογή υπηρεσίας ύφασμα αναβάθμιση του προγράμματος εκμάθησης](service-fabric-application-upgrade-tutorial.md) και [Έναρξη-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) για λεπτομερείς πληροφορίες.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Καθορίστε μια πολιτική ελέγχου εύρυθμης λειτουργίας στο αρχείο δήλωσης εφαρμογής

Κάθε υπηρεσία σε μια εφαρμογή υπηρεσίας ύφασμα μπορεί να έχει το δικό της παραμέτρους πολιτικής εύρυθμης λειτουργίας που αντικαθιστούν τις προεπιλεγμένες τιμές. Μπορείτε να παράσχετε αυτές τις τιμές παραμέτρων στο αρχείο δήλωσης της εφαρμογής.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να εφαρμόσετε μια πολιτική ελέγχου μοναδικές εύρυθμης λειτουργίας για κάθε υπηρεσία στο δηλωτικό εφαρμογής.

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες πληροφορίες σχετικά με την ανάπτυξη μιας εφαρμογής, ανατρέξτε στο θέμα [Ανάπτυξη μια υπάρχουσα εφαρμογή σε ύφασμα υπηρεσίας Azure](service-fabric-deploy-existing-app.md).
