<properties
   pageTitle="Υπηρεσία ύφασμα σύμπλεγμα από διαχειριστή πόρων - πολιτικές θέση | Microsoft Azure"
   description="Επισκόπηση των πολιτικών επιπλέον θέση και κανόνες για υπηρεσίες δομής υπηρεσίας"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="placement-policies-for-service-fabric-services"></a>Τοποθέτηση πολιτικών για υπηρεσίες δομής υπηρεσίας
Υπάρχουν πολλές διαφορετικές επιπλέον κανόνες που μπορεί να καταλήξετε επιμέλεια σχετικά με το εάν καλύπτει το σύμπλεγμά σας υπηρεσία ύφασμα κατά μήκος μιας γεωγραφικά αποστάσεις, πείτε πολλών κέντρα δεδομένων ή Azure περιοχές, ή εάν το περιβάλλον εκτείνεται σε πολλές περιοχές γεωπολιτική στοιχείου ελέγχου (ή κάποια άλλη περίπτωση όπου έχετε νομικές ή πολιτική όρια που σας ενδιαφέρουν ή τις αποστάσεις που εμπλέκονται έχουν επίδραση πραγματική επιδόσεων/λανθάνων χρόνος). Περισσότερες από αυτές τις ήταν δυνατό να ρυθμιστεί μέσω Ιδιότητες κόμβου και τοποθέτησή τους περιορισμούς, αλλά ορισμένα είναι πιο σύνθετη. Για να κάνετε πράγματα πιο απλά Παρέχουμε αυτές τις επιπλέον commmands. Όπως ακριβώς με άλλους περιορισμούς θέση, θέση πολιτικές μπορούν να ρυθμιστούν με βάση την παρουσία ανά με όνομα υπηρεσίας.

## <a name="specifying-invalid-domains"></a>Καθορισμός τομείς δεν είναι έγκυρη
Η πολιτική θέση InvalidDomain σας επιτρέπει να καθορίσετε ότι ένα συγκεκριμένο σφάλμα τομέα δεν είναι έγκυρο για αυτό φόρτο εργασίας. Αυτή η πολιτική εξασφαλίζει ότι μια συγκεκριμένη υπηρεσία εκτελείται ποτέ σε μια συγκεκριμένη περιοχή, για παράδειγμα για λόγους γεωπολιτική ή εταιρική πολιτική. Ενδέχεται να έχει καθοριστεί πολλούς τομείς δεν είναι έγκυρη μέσω ξεχωριστές πολιτικές.

![Παράδειγμα μη έγκυροι τομέα][Image1]

Κώδικας:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Καθορισμός τομέων απαιτούμενα
Η πολιτική θέση απαιτούμενο τομέα απαιτεί ότι όλων των παρουσιών χωρίς κατάσταση υπηρεσίας για την υπηρεσία ή με κατάσταση αντίγραφα να υπάρχουν στον καθορισμένο τομέα. Πολλοί τομείς απαιτείται μπορεί να καθοριστεί μέσω ξεχωριστές πολιτικές.

![Παράδειγμα απαιτούμενο τομέα][Image2]

Κώδικας:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Καθορισμός προτιμώμενης τομέα για τα κύρια αντίγραφα
Ο κύριος τομέας προτιμώμενη είναι ένα στοιχείο ελέγχου ενδιαφέρον, καθώς επιτρέπει την επιλογή των σφαλμάτων τομέα στην οποία πρέπει να τοποθετηθούν των κύριων, εάν είναι δυνατό να το κάνετε. Όταν όλα τα στοιχεία είναι σε καλή κατάσταση των κύριων θα καταλήξετε σε αυτόν τον τομέα. Πρέπει να τον τομέα ή την αποτυχία πρωτεύον αντίγραφο ή να τερματίσετε τη λειτουργία για κάποιο λόγο των κύριων θα μετεγκατασταθούν σε κάποια άλλη θέση. Εάν δεν αυτήν τη θέση του προτιμώμενη τομέα, στη συνέχεια, όταν είναι δυνατό η διαχείριση πόρων συμπλέγματος θα μετακινήσετε πίσω στον τομέα που προτιμάτε. Ομαλά αυτήν τη ρύθμιση μόνο έχει νόημα για τις υπηρεσίες με κατάσταση. Αυτή η πολιτική είναι πολύ χρήσιμη σε συμπλεγμάτων που είναι εκτείνονται σε Azure περιοχές ή πολλά κέντρα δεδομένων. Σε αυτές τις περιπτώσεις χρησιμοποιείτε όλες τις θέσεις για πλεονασμού, αλλά θα προτιμούσατε το πρωτεύον αντίγραφα να τοποθετηθούν σε μια συγκεκριμένη θέση προκειμένου να παρέχετε κάτω λανθάνων χρόνος για λειτουργίες που μεταβείτε στην κύρια (συντάσσει και επίσης, από προεπιλογή, όλα διαβάζει είναι εξυπηρετεί από την κύρια).

![Προτιμώμενη πρωτεύοντος τομείς και ανακατεύθυνσης][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Απαίτηση αντίγραφα να διανεμηθούν μεταξύ όλων των τομέων και απόρριψη συσκευασίας
Μια άλλη πολιτική, μπορείτε να καθορίσετε είναι να απαιτούν αντίγραφα πάντα να διανεμηθούν μεταξύ τους τομείς διαθέσιμη σφαλμάτων. Αυτό θα συμβεί από προεπιλογή, στις περισσότερες περιπτώσεις όπου το σύμπλεγμα είναι σε καλή κατάσταση, ωστόσο υπάρχουν degenerate περιπτώσεις όπου αντίγραφα για μια δεδομένη partition ενδέχεται να διακοπεί προσωρινά συσκευασμένο σε μία μόνο σφαλμάτων ή αναβάθμισης τομέα. Για παράδειγμα, ας υποθέσουμε ότι που Παρόλο που το σύμπλεγμα έχει 9 κόμβους σε 3 τομείς σφαλμάτων (0, 1 και 2), και την υπηρεσία έχει 3 αντίγραφα, μειώθηκε τους κόμβους που χρησιμοποιήθηκαν για αυτά τα αντίγραφα των σφαλμάτων τομείς 1 και 2 και λόγω ζητημάτων χωρητικότητα κανένα από άλλους κόμβους αυτούς τους τομείς ήταν έγκυρο. Εάν ύφασμα υπηρεσίας για να δημιουργήσετε αντικατάστασης ελέγχου για αυτά τα αντίγραφα, η διαχείριση πόρων συμπλέγματος θα πρέπει να τα τοποθετήσετε στον τομέα σφαλμάτων 0, αλλά που δημιουργεί μια κατάσταση όπου παραβιαστεί τον περιορισμό τομέα σφαλμάτων. Αυξάνει την πιθανότητα που το σύνολο ολόκληρη ρεπλίκα μπορεί να χαθούν (το όρισμα FD 0 σαν να χαθούν permananently). (Για περισσότερες πληροφορίες σχετικά με τους περιορισμούς και περιορισμού προτεραιότητες Γενικά, ανατρέξτε στο θέμα [αυτό το θέμα](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities) )

Εάν έχετε δει ποτέ μια προειδοποίηση εύρυθμης λειτουργίας, όπως "τη μονάδα εξισορρόπησης φόρτου έχει εντοπίσει μια παραβίαση περιορισμού για αυτό ρεπλίκα: ύφασμα: /<some service name> δευτερεύοντα διαμερίσματα <some partition ID> παραβιάζει τον περιορισμό: FaultDomain" έχετε πατήσετε αυτή η συνθήκη ή κάτι παρόμοιο με αυτό. Συνήθως αυτές τις περιπτώσεις είναι μεταβατικές (τους κόμβους δεν παραμένουν προς τα κάτω μεγάλη ή εάν ό, τι και πρέπει να δημιουργήσετε αντικατάστασης ελέγχου είναι τους άλλους κόμβους τους τομείς διόρθωση σφαλμάτων που ισχύουν), αλλά υπάρχουν ορισμένα φόρτους εργασίας που προτιμάτε να συναλλαγές διαθεσιμότητα για τον κίνδυνο να χάσετε όλα τα αντίγραφα τους. Μπορούμε να κάνουμε αυτό, καθορίζοντας την πολιτική "RequireDomainDistribution", η οποία θα εγγυάται ότι καμία δύο ρεπλίκα από την ίδια partition επιτρέπονται ποτέ στον ίδιο τομέα σφαλμάτων ή αναβάθμιση.

Ορισμένες φόρτους εργασίας προτιμούν να έχουν τον αριθμό προορισμού αντίγραφα (αντίγραφα κατάστασης) καθόλου ώρες (στοιχημάτων σε σχέση με αποτυχίες συνολικό τομέα και γνωρίζετε ότι αυτές συνήθως να ανακτήσετε τοπική κατάσταση), ότι θα άλλους προτιμάτε να λαμβάνει ο χρόνος εκτός λειτουργίας παλαιότερη από το ανησυχίες ακρίβειας και dataloss του κινδύνου. Αφού εκτελέσετε περισσότερους φόρτους εργασίας παραγωγής με περισσότερες από 3 αντίγραφα, η προεπιλογή είναι να μην απαιτούν διανομής τομέα και να αφήσετε εξισορρόπηση και ανακατεύθυνσης χειρισμού περιπτώσεις κανονικά, ακόμα και αν αυτό σημαίνει ότι προσωρινά έναν τομέα έχει πολλά αντίγραφα συσκευασμένο σε αυτήν.

Κώδικας:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Τώρα, θα ήταν δυνατό να χρησιμοποιήσετε αυτές τις ρυθμίσεις παραμέτρων για τις υπηρεσίες σε ένα σύμπλεγμα που δεν ήταν γεωγραφικά καλύπτει; Βεβαιωθείτε ότι μπορείτε να! Αλλά δεν υπάρχει ένα εξαιρετικό λόγο πολύ – ειδικά τις ρυθμίσεις παραμέτρων τομέων απαιτούμενα, δεν είναι έγκυρη και προτιμώμενη θα πρέπει να αποφεύγονται, εκτός εάν χρησιμοποιείτε στην πραγματικότητα ένα σύμπλεγμα γεωγραφικά διευρυμένο - δεν έχει νόημα να δοκιμάσετε για να επιβάλετε μια δεδομένη φόρτο εργασίας για να εκτελέσετε σε ένα μόνο rack ή για να προτιμάτε ορισμένες τμήματος του τοπικού συμπλέγματος πάνω από ένα άλλο, εκτός αν υπάρχει διαφορετικούς τύπους υλικού ή φόρτο εργασίας αγοράς, μεταβαίνοντας στην , και αυτές τις περιπτώσεις αντιμετώπισης μέσω κανονική θέση τους περιορισμούς.

## <a name="next-steps"></a>Επόμενα βήματα
- Για περισσότερες πληροφορίες σχετικά με τις άλλες διαθέσιμες επιλογές για τη ρύθμιση των παραμέτρων των υπηρεσιών ελέγχου εκτός του θέματος στην τις άλλες ρυθμίσεις παραμέτρων για τη διαχείριση πόρων συμπλέγματος διαθέσιμο, [Μάθετε περισσότερα σχετικά με τη ρύθμιση παραμέτρων υπηρεσιών](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
