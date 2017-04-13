<properties
   pageTitle="Κύκλος ζωής εφαρμογής στην υπηρεσία ύφασμα | Microsoft Azure"
   description="Περιγράφει ανάπτυξη, την ανάπτυξη, δοκιμή, την αναβάθμιση, διατηρώντας και κατάργηση εφαρμογών υπηρεσίας ύφασμα."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="service-fabric-application-lifecycle"></a>Κύκλος ζωής εφαρμογής υπηρεσίας ύφασμα
Όπως με άλλες πλατφόρμες, μια εφαρμογή σε ύφασμα υπηρεσίας Azure συνήθως, έχετε τις εξής φάσεις: Σχεδίαση, ανάπτυξη, δοκιμές, ανάπτυξη, την αναβάθμιση, συντήρηση και κατάργηση. Υπηρεσία ύφασμα παρέχει κορυφαία υποστήριξη για την πλήρη εφαρμογή του κύκλου ζωής των εφαρμογών του cloud, από την ανάπτυξη μέσω ανάπτυξης, ημερήσια διαχείρισης και συντήρησης για ενδεχόμενη διακόψετε τη χρήση. Το μοντέλο υπηρεσιών επιτρέπει πολλούς διαφορετικούς ρόλους για να συμμετάσχετε ανεξάρτητη στο του κύκλου ζωής της εφαρμογής. Σε αυτό το άρθρο παρέχει μια επισκόπηση των τα API και τον τρόπο που χρησιμοποιούνται από τους διαφορετικούς ρόλους σε όλο το φάσεις του κύκλου ζωής της εφαρμογής υπηρεσίας ύφασμα.

## <a name="service-model-roles"></a>Ρόλοι μοντέλο υπηρεσίας
Οι ρόλοι μοντέλο υπηρεσίας είναι:

- **Υπηρεσία για προγραμματιστές**: αναπτύσσει λειτουργική και γενικό υπηρεσίες που μπορούν να εκ νέου purposed και να χρησιμοποιηθεί σε πολλές εφαρμογές του ίδιου τύπου ή διαφορετικούς τύπους. Για παράδειγμα, μια υπηρεσία ουράς μπορεί να χρησιμοποιηθεί για τη δημιουργία μιας εφαρμογής εισιτηρίων (γραφείο βοήθειας) ή μια εφαρμογή ηλεκτρονικού εμπορίου (καλάθι αγορών).

- **Προγραμματιστή της εφαρμογής**: δημιουργεί εφαρμογές ενσωματώνοντας μια συλλογή υπηρεσιών για να ικανοποιήσετε ορισμένες συγκεκριμένες απαιτήσεις ή σενάρια. Για παράδειγμα, μια τοποθεσία Web ηλεκτρονικού εμπορίου μπορεί να ενοποιήσετε το "JSON χωρίς κατάσταση περιβάλλοντος χρήστη υπηρεσίας," "Προσφορών βάσει της κατάστασης υπηρεσίας" και "Ουρά βάσει της κατάστασης υπηρεσίας" για να δημιουργήσετε μια λύση auctioning.

- **Διαχειριστής της εφαρμογής**: καθιστά αποφάσεις για την εφαρμογή ρύθμισης παραμέτρων (συμπληρώνοντας τη ρύθμιση παραμέτρων του προτύπου), ανάπτυξης (αντιστοίχιση διαθέσιμους πόρους) και την ποιότητα της υπηρεσίας. Για παράδειγμα, ο διαχειριστής της εφαρμογής καθορίζεται τοπικές ρυθμίσεις γλώσσας (Αγγλικά για τις Ηνωμένες Πολιτείες) ή τα ιαπωνικά για Ιαπωνία, για παράδειγμα της εφαρμογής. Μια διαφορετική εφαρμογή ανεπτυγμένος μπορούν να έχουν διαφορετικές ρυθμίσεις.

- **Τελεστής**: ανάπτυξη εφαρμογές με βάση τις ρυθμίσεις της εφαρμογής και τις απαιτήσεις που καθορίζεται από το διαχειριστή της εφαρμογής. Για παράδειγμα, έναν τελεστή διατάξεις και ανάπτυξη της εφαρμογής και εξασφαλίζει ότι εκτελείται στο Azure. Τελεστές παρακολούθηση της εύρυθμης λειτουργίας και επιδόσεων πληροφορίες της εφαρμογής και να διατηρήσετε την υποδομή φυσικής σύμφωνα με τις ανάγκες.


## <a name="develop"></a>Ανάπτυξη
1. *Υπηρεσία προγραμματιστής* αναπτύσσει διαφορετικούς τύπους υπηρεσιών χρησιμοποιώντας το μοντέλο προγραμματισμού [Αξιόπιστη ηθοποιών](service-fabric-reliable-actors-introduction.md) ή [Αξιόπιστων υπηρεσιών](service-fabric-reliable-services-introduction.md) .
2. *Υπηρεσία προγραμματιστής* δηλωτικά περιγράφει τους τύπους αναπτυχθεί υπηρεσίας σε ένα αρχείο δηλώσεων υπηρεσίας που αποτελείται από ένα ή περισσότερα πακέτα κώδικα, ρύθμιση παραμέτρων και δεδομένων.
3. Μια *προγραμματιστή της εφαρμογής* , στη συνέχεια, δημιουργεί μια εφαρμογή χρησιμοποιώντας τύπους διαφορετική υπηρεσία.
4. Μια *προγραμματιστή της εφαρμογής* περιγράφονται δηλωτικά ο τύπος της εφαρμογής στη δήλωση εφαρμογής με την αναφορά του δηλώσεων υπηρεσίας του στοιχείου υπηρεσιών και σωστά παράκαμψη και ρύθμιση διαφορετικές ρυθμίσεις παραμέτρων και την ανάπτυξη των υπηρεσιών στοιχείου.

Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το αξιόπιστη ηθοποιών](service-fabric-reliable-actors-get-started.md) και [Γρήγορα αποτελέσματα με τις υπηρεσίες του αξιόπιστη](service-fabric-reliable-services-quick-start.md) για να δείτε παραδείγματα.

## <a name="deploy"></a>Ανάπτυξη
1. Ο *διαχειριστής της εφαρμογής* προσαρμόζει τον τύπο εφαρμογής σε μια συγκεκριμένη εφαρμογή για να αναπτυχθεί σε ένα σύμπλεγμα ύφασμα υπηρεσίας, ορίζοντας τις κατάλληλες παραμέτρους του στοιχείου **ApplicationType** στο τη δήλωση της εφαρμογής.

2. *Τελεστής* αποστέλλει το πακέτο εφαρμογών στο χώρο αποθήκευσης ειδώλου σύμπλεγμα, χρησιμοποιώντας τη [μέθοδο **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) ή το [cmdlet **ServiceFabricApplicationPackage αντίγραφο** ](https://msdn.microsoft.com/library/azure/mt125905.aspx). Το πακέτο εφαρμογών περιέχει τη δήλωση της εφαρμογής και τη συλλογή των πακέτων υπηρεσίας. Υπηρεσία ύφασμα ανάπτυξη εφαρμογών από το πακέτο εφαρμογών που είναι αποθηκευμένα στο χώρο αποθήκευσης εικόνα, η οποία μπορεί να είναι ένας χώρος αποθήκευσης αντικειμένων blob του Azure ή η υπηρεσία συστήματος ύφασμα υπηρεσίας.

3. Τον *τελεστή* διατάξεις, στη συνέχεια, ο τύπος της εφαρμογής στο σύμπλεγμα προορισμού από το πακέτο εφαρμογών που έχουν αποσταλεί με τη [μέθοδο **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), το [cmdlet **Register-ServiceFabricApplicationType** ](https://msdn.microsoft.com/library/azure/mt125958.aspx)ή η [λειτουργία ΥΠΌΛΟΙΠΟ **παροχή μιας εφαρμογής** ](https://msdn.microsoft.com/library/azure/dn707672.aspx).

4. Μετά την προμήθεια της εφαρμογής, έναν *τελεστή* ξεκινά την εφαρμογή με τις παραμέτρους που παρέχονται από το *διαχειριστή της εφαρμογής* , χρησιμοποιώντας τη [μέθοδο **CreateApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), το [cmdlet **New-ServiceFabricApplication** ](https://msdn.microsoft.com/library/azure/mt125913.aspx)ή η [λειτουργία ΥΠΌΛΟΙΠΟ **Δημιουργία εφαρμογής** ](https://msdn.microsoft.com/library/azure/dn707676.aspx).

5. Μετά την ανάπτυξη της εφαρμογής, έναν *τελεστή* χρησιμοποιεί τη [μέθοδο **CreateServiceAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), το [cmdlet **New-ServiceFabricService** ](https://msdn.microsoft.com/library/azure/mt125874.aspx)ή η [λειτουργία ΥΠΌΛΟΙΠΟ **Δημιουργία υπηρεσίας** ](https://msdn.microsoft.com/library/azure/dn707657.aspx) για να δημιουργήσετε νέες παρουσίες υπηρεσίας για την εφαρμογή που βασίζεται σε τύπους διαθέσιμη υπηρεσία.

6. Η εφαρμογή εκτελείται τώρα στο σύμπλεγμα ύφασμα υπηρεσίας.

Ανατρέξτε στο θέμα [ανάπτυξη μιας εφαρμογής](service-fabric-deploy-remove-applications.md) για να δείτε παραδείγματα.

## <a name="test"></a>Έλεγχος
1. Μετά την ανάπτυξη του συμπλέγματος τοπικής ανάπτυξης ή ένα σύμπλεγμα δοκιμής, *υπηρεσία προγραμματιστής* εκτελείται το σενάριο δοκιμής ενσωματωμένη ανακατεύθυνσης χρησιμοποιώντας τις κλάσεις [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) και [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) ή το [cmdlet **Ενεργοποίηση ServiceFabricFailoverTestScenario** ](https://msdn.microsoft.com/library/azure/mt644783.aspx). Το σενάριο δοκιμής ανακατεύθυνσης εκτελείται μιας συγκεκριμένης υπηρεσίας μέσω σημαντικές μεταβάσεων και ανακατευθύνσεις για να βεβαιωθείτε ότι είναι ακόμη διαθέσιμες και λειτουργεί.

2. Ο *Προγραμματιστής υπηρεσία* εκτελείται, στη συνέχεια, το σενάριο δοκιμής ενσωματωμένη χάος με τη χρήση των κλάσεων [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) και [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) ή το [cmdlet **Ενεργοποίηση ServiceFabricChaosTestScenario** ](https://msdn.microsoft.com/library/azure/mt644774.aspx). Το σενάριο δοκιμής χάος υποδείξει τυχαία πολλών κόμβο πακέτου κώδικα και σφάλματα αντιγράφου στο σύμπλεγμα.

3. *Υπηρεσία προγραμματιστής* [δοκιμές υπηρεσία εξυπηρέτησης επικοινωνίας](service-fabric-testability-scenarios-service-communication.md) με τη σύνταξη σεναρίων δοκιμών που κινούνται κύρια αντίγραφα γύρω από το σύμπλεγμα.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Εισαγωγή στην υπηρεσία ανάλυσης σφαλμάτων](service-fabric-testability-overview.md) .

## <a name="upgrade"></a>Αναβάθμιση
1. *Υπηρεσία προγραμματιστής* ενημερώνει απαρτίζουν υπηρεσιών από τον οποίο δημιουργήθηκε παρουσία της εφαρμογής ή/και επιδιορθώνει σφάλματα και παρέχει μια νέα έκδοση της δήλωσης υπηρεσίας.

2. Μια *προγραμματιστή της εφαρμογής* παρακάμπτει και parameterizes τις ρυθμίσεις παραμέτρων και την ανάπτυξη των υπηρεσιών συνεπή και παρέχει μια νέα έκδοση της εφαρμογής δηλωτικό. Ο προγραμματιστής εφαρμογών, στη συνέχεια, ενσωματώνει τις νέες εκδόσεις με την υπηρεσία δηλώσεων σε μια εφαρμογή και παρέχει μια νέα έκδοση του ο τύπος της εφαρμογής σε ένα πακέτο εφαρμογών ενημερωμένες.

3. Ο *διαχειριστής της εφαρμογής* ενσωματώνει τη νέα έκδοση της ο τύπος της εφαρμογής στην εφαρμογή προορισμού, ενημερώνοντας τις κατάλληλες παραμέτρους.

5. *Τελεστής* αποστέλλει το πακέτο εφαρμογών ενημερωμένες στο χώρο αποθήκευσης ειδώλου σύμπλεγμα χρησιμοποιώντας τη [μέθοδο **CopyApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) ή το [cmdlet **ServiceFabricApplicationPackage αντίγραφο** ](https://msdn.microsoft.com/library/azure/mt125905.aspx). Το πακέτο εφαρμογών περιέχει τη δήλωση της εφαρμογής και τη συλλογή των πακέτων υπηρεσίας.

6. *Τελεστής* διατάξεις τη νέα έκδοση της εφαρμογής στο σύμπλεγμα προορισμού, χρησιμοποιώντας τη [μέθοδο **ProvisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), το [cmdlet **Register-ServiceFabricApplicationType** ](https://msdn.microsoft.com/library/azure/mt125958.aspx)ή η [λειτουργία ΥΠΌΛΟΙΠΟ **παροχή μιας εφαρμογής** ](https://msdn.microsoft.com/library/azure/dn707672.aspx).

7. *Τελεστής* αναβαθμίζει την εφαρμογή προορισμού στη νέα έκδοση χρησιμοποιώντας τη [μέθοδο **UpgradeApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), το [cmdlet **ServiceFabricApplicationUpgrade Έναρξη** ](https://msdn.microsoft.com/library/azure/mt125975.aspx)ή η [λειτουργία ΥΠΌΛΟΙΠΟ **αναβάθμιση μιας εφαρμογής** ](https://msdn.microsoft.com/library/azure/dn707633.aspx).

8. *Τελεστής* ελέγχει την πρόοδο της αναβάθμισης χρησιμοποιώντας τη [μέθοδο **GetApplicationUpgradeProgressAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), το [cmdlet **Get-ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt125988.aspx)ή η [λειτουργία ΥΠΌΛΟΙΠΟ **Γρήγορα την πρόοδο εφαρμογής αναβάθμιση** ](https://msdn.microsoft.com/library/azure/dn707631.aspx).

9. Εάν είναι απαραίτητο, τον *τελεστή* τροποποιεί και εφαρμόζει ξανά τις παραμέτρους της τρέχουσας αναβάθμισης εφαρμογής χρησιμοποιώντας τη [μέθοδο **UpdateApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), το [cmdlet **Ενημέρωση ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt126030.aspx)ή τη [λειτουργία **Αναβάθμισης εφαρμογής ενημέρωση** ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt628489.aspx).

10. Εάν είναι απαραίτητο, τον *τελεστή* επαναφέρει η αναβάθμιση εφαρμογών χρησιμοποιώντας τη [μέθοδο **RollbackApplicationUpgradeAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), το [cmdlet **ServiceFabricApplicationRollback Έναρξη** ](https://msdn.microsoft.com/library/azure/mt125833.aspx)ή η [λειτουργία ΥΠΌΛΟΙΠΟ **Αναβάθμισης εφαρμογής επαναφοράς** ](https://msdn.microsoft.com/library/azure/mt628494.aspx).

11. Υπηρεσία ύφασμα αναβαθμίζει την εφαρμογή-στόχο εκτελείται στο σύμπλεγμα χωρίς να χάσετε τη διαθεσιμότητα των οποιαδήποτε από τις υπηρεσίες του στοιχείου.

Ανατρέξτε στο θέμα [εφαρμογή αναβάθμιση του προγράμματος εκμάθησης](service-fabric-application-upgrade-tutorial.md) για να δείτε παραδείγματα.

## <a name="maintain"></a>Διατήρηση
1. Για το λειτουργικό σύστημα αναβαθμίσεις και ενημερωμένες εκδόσεις κώδικα, υπηρεσία ύφασμα περιβάλλοντα εργασίας με το Azure υποδομή για να εξασφαλίσετε Διαθεσιμότητα από όλες τις εφαρμογές που εκτελούνται στο σύμπλεγμα.

2. Για αναβαθμίσεις και ενημερωμένες εκδόσεις κώδικα για την πλατφόρμα ύφασμα υπηρεσίας, υπηρεσία ύφασμα αναβαθμίζει ίδια χωρίς να χάσετε διαθεσιμότητα κάποια από τις εφαρμογές που εκτελούνται στο σύμπλεγμα.

3. Ο *διαχειριστής της εφαρμογής* εγκρίνει της προσθήκης ή κατάργησης του κόμβους από ένα σύμπλεγμα μετά την ανάλυση δεδομένων χρήσης ιστορικού χωρητικότητα και πρόβλεψη μελλοντικών ζήτηση.

4. *Τελεστής* προσθέτει και καταργεί κόμβους που καθορίζεται από το *διαχειριστή της εφαρμογής*.

5. Όταν προστίθεται νέο κόμβους ή υπάρχουσα κόμβους καταργούνται από το σύμπλεγμα, υπηρεσία ύφασμα αυτόματα φόρτωση-υπολοίπων την εκτέλεση εφαρμογών σε όλους τους κόμβους του συμπλέγματος για να επιτύχετε βέλτιστη απόδοση.

## <a name="remove"></a>Κατάργηση
1. *Τελεστής* να διαγράψετε μια συγκεκριμένη παρουσία της υπηρεσίας που εκτελείται στο σύμπλεγμα χωρίς να καταργήσετε ολόκληρη την εφαρμογή χρησιμοποιώντας τη [μέθοδο **DeleteServiceAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), το [cmdlet **Κατάργηση ServiceFabricService** ](https://msdn.microsoft.com/library/azure/mt126033.aspx)ή η [λειτουργία ΥΠΌΛΟΙΠΟ **Διαγραφή υπηρεσίας** ](https://msdn.microsoft.com/library/azure/dn707687.aspx).  

2. *Τελεστής* επίσης να διαγράψετε μια παρουσία της εφαρμογής και όλες τις υπηρεσίες χρησιμοποιώντας τη [μέθοδο **DeleteApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), το [cmdlet **Κατάργηση ServiceFabricApplication** ](https://msdn.microsoft.com/library/azure/mt125914.aspx)ή η [λειτουργία ΥΠΌΛΟΙΠΟ **Διαγραφή εφαρμογής** ](https://msdn.microsoft.com/library/azure/dn707651.aspx).

3. Μετά την εφαρμογή και υπηρεσίες έχουν διακοπεί, τον *τελεστή* να Κατάργηση προμήθειας ο τύπος της εφαρμογής χρησιμοποιώντας τη [μέθοδο **UnprovisionApplicationAsync** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), το [cmdlet **Unregister-ServiceFabricApplicationType** ](https://msdn.microsoft.com/library/azure/mt125885.aspx)ή η [λειτουργία ΥΠΌΛΟΙΠΟ **Αναίρεση παροχής μια εφαρμογή του** ](https://msdn.microsoft.com/library/azure/dn707671.aspx). Απομάκρυνση ο τύπος της εφαρμογής δεν καταργεί το πακέτο εφαρμογών από το ImageStore. Πρέπει να καταργήσετε το πακέτο εφαρμογών με μη αυτόματο τρόπο.

4. *Τελεστής* καταργεί το πακέτο εφαρμογών από το ImageStore χρησιμοποιώντας τη [μέθοδο **RemoveApplicationPackage** ](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) ή το [cmdlet **Κατάργηση ServiceFabricApplicationPackage** ](https://msdn.microsoft.com/library/azure/mt163532.aspx).

Ανατρέξτε στο θέμα [ανάπτυξη μιας εφαρμογής](service-fabric-deploy-remove-applications.md) για να δείτε παραδείγματα.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με την ανάπτυξη, δοκιμή και διαχείριση εφαρμογών υπηρεσίας ύφασμα και υπηρεσίες, ανατρέξτε στο θέμα:

- [Αξιόπιστη ηθοποιών](service-fabric-reliable-actors-introduction.md)
- [Αξιόπιστη υπηρεσιών](service-fabric-reliable-services-introduction.md)
- [Ανάπτυξη μιας εφαρμογής](service-fabric-deploy-remove-applications.md)
- [Αναβάθμιση εφαρμογής](service-fabric-application-upgrade.md)
- [Επισκόπηση με δυνατότητα δοκιμής](service-fabric-testability-overview.md)
- [Δείγμα εφαρμογής που βασίζεται στα ΥΠΌΛΟΙΠΑ κύκλου ζωής](service-fabric-rest-based-application-lifecycle-sample.md)
