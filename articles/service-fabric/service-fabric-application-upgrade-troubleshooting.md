<properties
   pageTitle="Αντιμετώπιση προβλημάτων εφαρμογής αναβαθμίσεις | Microsoft Azure"
   description="Σε αυτό το άρθρο περιγράφει ορισμένα κοινά θέματα γύρω από την αναβάθμιση μιας εφαρμογής υπηρεσίας ύφασμα και τον τρόπο για να τα επιλύσετε."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="troubleshoot-application-upgrades"></a>Αντιμετώπιση προβλημάτων εφαρμογής αναβαθμίσεις

Σε αυτό το άρθρο περιγράφει ορισμένα από τα κοινά θέματα γύρω από την αναβάθμιση μιας εφαρμογής Azure Service ύφασμα και τον τρόπο για να τα επιλύσετε.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Αντιμετώπιση προβλημάτων μιας αναβάθμισης εφαρμογής που έχει αποτύχει

Όταν αποτύχει η αναβάθμιση, το αποτέλεσμα της εντολής **Get-ServiceFabricApplicationUpgrade** περιέχει πρόσθετες πληροφορίες για τον εντοπισμό σφαλμάτων την αποτυχία.  Η παρακάτω λίστα καθορίζει πώς μπορούν να χρησιμοποιηθούν οι πρόσθετες πληροφορίες:

1. Προσδιορισμός του τύπου αποτυχία.
2. Προσδιορίστε την αιτία της αποτυχίας.
3. Απομόνωση ένα ή περισσότερα στοιχεία παρουσιάζει σφάλμα για περαιτέρω διερεύνηση.

Αυτές οι πληροφορίες είναι διαθέσιμες όταν ύφασμα υπηρεσίας εντοπίσει την αποτυχία ανεξάρτητα από το εάν είναι η **FailureAction** για να επαναφέρετε ή να αναστείλει την αναβάθμιση.

### <a name="identify-the-failure-type"></a>Προσδιορίστε τον τύπο αποτυχία

Στο αποτέλεσμα της **Get-ServiceFabricApplicationUpgrade**, **FailureTimestampUtc** προσδιορίζει τη χρονική σήμανση (στο UTC) με τον οποίο μια αποτυχία αναβάθμισης εντοπίστηκε από ύφασμα υπηρεσίας και **FailureAction** ενεργοποιήθηκε. **FailureReason** προσδιορίζει μία από τρεις πιθανές αιτίες υψηλού επιπέδου της αποτυχίας:

1. UpgradeDomainTimeout - υποδεικνύει ότι ένα συγκεκριμένο τομέα αναβάθμισης εκτελέσατε πολύ χρόνο για να ολοκληρώσετε και **UpgradeDomainTimeout** έχει λήξει.
2. OverallUpgradeTimeout - υποδεικνύει ότι η συνολική αναβάθμιση εκτελέσατε πολύ χρόνο για να ολοκληρώσετε και **UpgradeTimeout** έχει λήξει.
3. Πρόγραμμα HealthCheck - υποδεικνύει ότι μετά την αναβάθμιση ενός τομέα ενημέρωση, την εφαρμογή παρέμεινε κατεστραμμένες σύμφωνα με τις πολιτικές καθορισμένο εύρυθμης λειτουργίας και **HealthCheckRetryTimeout** έχει λήξει.

Αυτές τις καταχωρήσεις μόνο εμφανίζεται στο αποτέλεσμα κατά την αναβάθμιση αποτυγχάνει και ξεκινά επαναφορά. Ανάλογα με τον τύπο της αποτυχίας εμφανίζονται περαιτέρω πληροφορίες.

### <a name="investigate-upgrade-timeouts"></a>Διερεύνηση αναβάθμισης χρονικών ορίων

Αναβάθμιση χρονικού ορίου αποτυχίες πιο συχνά προκαλούνται από ζητήματα διαθεσιμότητα υπηρεσίας. Το αποτέλεσμα μετά από αυτήν την παράγραφο είναι τυπικές αναβαθμίσεων όπου υπηρεσίας αντίγραφα ή παρουσίες μπορούν να ξεκινήσουν στη νέα έκδοση κώδικα. Το πεδίο **UpgradeDomainProgressAtFailure** καταγράφει ένα στιγμιότυπο του οποιαδήποτε εργασία σε εκκρεμότητα αναβάθμισης τη στιγμή της αποτυχίας.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

Σε αυτό το παράδειγμα, η αναβάθμιση απέτυχε κατά την αναβάθμιση τομέα *MYUD1* και έχουν κολλήσει δύο διαμερίσματα (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* και *4b43f4d8-b26b-424e-9307-7a7a62e79750*). Τα διαμερίσματα έχουν κολλήσει επειδή το περιβάλλον εκτέλεσης δεν ήταν δυνατό να τοποθετήσετε κύρια αντίγραφα (*WaitForPrimaryPlacement*) σε κόμβους προορισμού *Node1* και *Node4*.

Η εντολή **Get-ServiceFabricNode** μπορεί να χρησιμοποιηθεί για να επαληθεύσετε ότι αυτά τα δύο κόμβους αναβάθμισης τομέα *MYUD1*. Το *UpgradePhase* αναφέρει *PostUpgradeSafetyCheck*, γεγονός που σημαίνει ότι αυτές οι έλεγχοι ασφαλείας παρουσιάζονται μετά την ολοκλήρωση της όλους τους κόμβους στον τομέα αναβάθμισης έχετε την αναβάθμιση. Οδηγεί όλες αυτές τις πληροφορίες σε ένα πιθανό ζήτημα με τη νέα έκδοση της τον κώδικα της εφαρμογής. Τα πιο συνηθισμένα ζητήματα είναι σφάλματα της υπηρεσίας στο το άνοιγμα ή την προώθηση σε διαδρομές πρωτεύοντος κώδικα.

Μια *UpgradePhase* του *PreUpgradeSafetyCheck* σημαίνει ότι υπήρχαν θέματα Προετοιμασία αναβάθμισης τομέα πριν από την πραγματοποιήθηκε. Τα πιο συνηθισμένα ζητήματα σε αυτήν την περίπτωση είναι σφάλματα της υπηρεσίας στο κλείσιμο ή υποβιβασμός από τις διαδρομές πρωτεύοντος κώδικα.

Η τρέχουσα **UpgradeState** είναι *RollingBackCompleted*, ώστε η αρχική αναβάθμιση πρέπει να έχουν εκτελεστεί με μια επαναφορά **FailureAction**, που επανέρχεται αυτόματα η αναβάθμιση περίπτωση αποτυχίας. Εάν η αρχική αναβάθμιση έχει πραγματοποιηθεί με μια μη αυτόματη **FailureAction**, στη συνέχεια, η αναβάθμιση θα αντί για αυτό σε κατάσταση αναστολής για να επιτρέψετε σε ζωντανή αντιμετώπιση σφαλμάτων της εφαρμογής.

### <a name="investigate-health-check-failures"></a>Διερεύνηση αποτυχιών του ελέγχου εύρυθμης λειτουργίας

Αποτυχίες ελέγχου εύρυθμης λειτουργίας μπορεί να ενεργοποιηθεί από διάφορα θέματα που μπορεί να συμβεί μετά όλους τους κόμβους σε έναν τομέα της αναβάθμισης λήξης την αναβάθμιση και που περνά μέσα σε όλους τους ελέγχους ασφαλείας. Το αποτέλεσμα μετά από αυτήν την παράγραφο είναι τυπικές από μια αποτυχία αναβάθμισης λόγω έλεγχοι εύρυθμης λειτουργίας απέτυχε. Το πεδίο **UnhealthyEvaluations** καταγράφει ένα στιγμιότυπο του έλεγχοι εύρυθμης λειτουργίας που απέτυχε κατά την αναβάθμιση του σύμφωνα με την καθορισμένη [πολιτική εύρυθμης λειτουργίας](service-fabric-health-introduction.md).

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

Πρώτα Διερεύνηση αποτυχιών του ελέγχου εύρυθμης λειτουργίας απαιτεί την κατανόηση του μοντέλου εύρυθμης λειτουργίας υπηρεσίας ύφασμα. Αλλά ακόμα και χωρίς μια τέτοια αναλυτικά διευθέτηση, μπορούμε να δούμε ότι δύο υπηρεσίες είναι κατεστραμμένη: *ύφασμα: / DemoApp/Svc3* και *ύφασμα: / DemoApp/Svc2*, μαζί με τις αναφορές εύρυθμης λειτουργίας σφάλματος ("InjectedFault" σε αυτήν την περίπτωση). Σε αυτό το παράδειγμα, οι υπηρεσίες δύο από τις τέσσερις είναι κατεστραμμένη, που είναι κάτω από την προεπιλεγμένη προορισμού 0% κατεστραμμένες (*MaxPercentUnhealthyServices*).

Η αναβάθμιση ήταν σε αναστολή κατά την αποτυχία, καθορίζοντας μια **FailureAction** των μη αυτόματα κατά την εκκίνηση της αναβάθμισης. Αυτή η λειτουργία επιτρέπει μας για να εξερευνήσετε το πραγματικό σύστημα σε κατάσταση αποτυχίας πριν κάνετε περαιτέρω την ενέργεια.

### <a name="recover-from-a-suspended-upgrade"></a>Ανάκτηση από μια αιωρούμενα αναβάθμιση

Με την επαναφορά **FailureAction**, δεν υπάρχει καμία αποκατάστασης είναι απαραίτητο, επειδή η αναβάθμιση αυτόματα επαναφέρει κατά την αποτυχία. Με μια μη αυτόματη **FailureAction**, υπάρχουν αρκετές επιλογές αποκατάστασης:

1. Έναυσμα με μη αυτόματο τρόπο μια επαναφορά
2. Συνεχίστε με το υπόλοιπο της αναβάθμισης με μη αυτόματο τρόπο
3. Συνέχιση της εποπτευόμενη αναβάθμισης

Η εντολή **Έναρξη ServiceFabricApplicationRollback** μπορεί να χρησιμοποιηθεί οποιαδήποτε στιγμή για να ξεκινήσετε την κατάργηση της εγκατάστασης της εφαρμογής. Όταν η εντολή επιστρέφει με επιτυχία, η αίτηση επαναφοράς έχει καταχωρηθεί στο σύστημα και ξεκινά αμέσως μετά.

Η εντολή **Βιογραφικού σημειώματος ServiceFabricApplicationUpgrade** μπορεί να χρησιμοποιηθεί για με μη αυτόματο τρόπο, συνεχίστε με το υπόλοιπο της αναβάθμισης έναν τομέα αναβάθμισης κάθε φορά. Σε αυτήν τη λειτουργία μόνο ασφάλεια ελέγχους που εκτελούνται από το σύστημα. Δεν υπάρχουν άλλες έλεγχοι εύρυθμης λειτουργίας που εκτελούνται. Αυτή η εντολή μπορεί να χρησιμοποιηθεί μόνο όταν το *UpgradeState* εμφανίζει *RollingForwardPending*, γεγονός που σημαίνει ότι τον τρέχοντα τομέα αναβάθμισης ολοκλήρωσε την αναβάθμιση, αλλά επόμενης δεν έχει ξεκινήσει (σε εκκρεμότητα).

Η εντολή **Ενημέρωση ServiceFabricApplicationUpgrade** μπορεί να χρησιμοποιηθεί για να συνεχίσετε την εποπτευόμενη αναβάθμιση και οι δύο ασφάλεια και εύρυθμης λειτουργίας ελέγχει την εκτέλεση.

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

Συνεχίζεται η αναβάθμιση από την αναβάθμιση τομέα όπου αναβλήθηκε τελευταία και χρησιμοποιήστε το ίδιο αναβάθμιση παραμέτρους και τις πολιτικές εύρυθμης λειτουργίας ως πριν από. Εάν είναι απαραίτητο, οποιαδήποτε από τις παραμέτρους αναβάθμισης και πολιτικές εύρυθμης λειτουργίας εμφανίζεται στο προηγούμενο αποτέλεσμα μπορεί να αλλάξει με την ίδια εντολή μετά την επαναφορά της αναβάθμισης. Σε αυτό το παράδειγμα, η αναβάθμιση ήταν επαναληφθεί στη λειτουργία εγγεγραμμένων, με τις παραμέτρους και τις πολιτικές εύρυθμης λειτουργίας που δεν έχουν αλλάξει.

## <a name="further-troubleshooting"></a>Περισσότερες πληροφορίες αντιμετώπισης προβλημάτων

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Υπηρεσία ύφασμα δεν παρακολουθεί τις πολιτικές καθορισμένο εύρυθμης λειτουργίας

Πιθανή αιτία 1:

Υπηρεσία ύφασμα μεταφράζει όλα ποσοστά σε πραγματική αριθμούς οντοτήτων (για παράδειγμα, αντίγραφα, τα διαμερίσματα και υπηρεσίες) για την αξιολόγηση της υγείας και πάντα Στρογγυλοποιεί προς τα επάνω σε ολόκληρο οντοτήτων. Για παράδειγμα, εάν το μέγιστο _MaxPercentUnhealthyReplicasPerPartition_ είναι 21% και υπάρχουν πέντε αντίγραφα, στη συνέχεια, υπηρεσία ύφασμα επιτρέπει έως και δύο κατεστραμμένες αντίγραφα (που is,'Math.Ceiling (5\*0.21)). Έτσι, οι πολιτικές εύρυθμης λειτουργίας πρέπει να οριστεί αντίστοιχα.

Πιθανή αιτία 2:

Πολιτικές εύρυθμης λειτουργίας καθορίζονται όσον αφορά ποσοστά σύνολο υπηρεσιών και όχι συγκεκριμένα υπηρεσίας παρουσίες. Για παράδειγμα, πριν από την αναβάθμιση, εάν μια εφαρμογή έχει τέσσερις υπηρεσίας παρουσίες A, B, C και D, όπου υπηρεσίας D είναι κατεστραμμένη, αλλά με μικρή επίδραση στην εφαρμογή. Θέλουμε να παραβλέψετε την υπηρεσία γνωστά κατεστραμμένες Δ κατά τη διάρκεια της αναβάθμισης και να ορίσετε την παράμετρο *MaxPercentUnhealthyServices* να είναι 25%, υπό την προϋπόθεση ότι μόνο A, B και C πρέπει να είναι σε καλή κατάσταση.

Ωστόσο, κατά την αναβάθμιση, D μπορεί να μετατραπεί σε καλή κατάσταση ενώ γίνεται κατεστραμμένες C. Η αναβάθμιση θα εξακολουθούν να επιτύχουν επειδή είναι κατεστραμμένη μόνο το 25% των υπηρεσιών. Ωστόσο, αυτό μπορεί να έχει ως αποτέλεσμα απροσδόκητες σφάλματα λόγω C που απροσδόκητα κατεστραμμένες αντί για D. Σε αυτήν την περίπτωση, πρέπει να διαμορφωθεί D ως έναν τύπο διαφορετικό υπηρεσίας από το A, B και C. Επειδή το πολιτικές εύρυθμης λειτουργίας καθορίζονται ανά τύπο υπηρεσίας, όρια διαφορετικό ποσοστό κατεστραμμένες μπορούν να εφαρμοστούν σε διαφορετικές υπηρεσίες. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Να δεν καθορίσατε μια πολιτική εύρυθμης λειτουργίας για αναβάθμιση σε εφαρμογή, αλλά η αναβάθμιση εξακολουθεί να αποτυγχάνει για ορισμένες λήξη περιόδων που ποτέ καθοριστεί

Όταν πολιτικές εύρυθμης λειτουργίας δεν παρέχεται για την αίτηση αναβάθμισης, λαμβάνονται από το *ApplicationManifest.xml* από την τρέχουσα έκδοση της εφαρμογής. Για παράδειγμα, εάν πραγματοποιείτε αναβάθμιση X εφαρμογή από την έκδοση 1.0 στην έκδοση 2.0, πολιτικές εύρυθμη λειτουργία των εφαρμογών που έχουν καθοριστεί για στην έκδοση 1.0 χρησιμοποιούνται. Εάν μια πολιτική διαφορετικό εύρυθμης λειτουργίας πρέπει να χρησιμοποιούνται για την αναβάθμιση, στη συνέχεια, την πολιτική πρέπει να έχει καθοριστεί ως τμήμα της κλήσης API αναβάθμισης εφαρμογής. Τις πολιτικές που έχουν καθοριστεί ως μέρους της κλήσης API ισχύουν μόνο στη διάρκεια της αναβάθμισης. Μόλις ολοκληρωθεί η αναβάθμιση, χρησιμοποιούνται οι πολιτικές που καθορίζεται στο το *ApplicationManifest.xml* .

### <a name="incorrect-time-outs-are-specified"></a>Έχουν καθοριστεί εσφαλμένες λήξη περιόδων

Που μπορεί να έχετε αναρωτηθεί σχετικά με το τι συμβαίνει κατά τη λήξη περιόδων έχουν οριστεί με ασυνέπεια. Για παράδειγμα, μπορεί να έχετε ένα *UpgradeTimeout* που είναι μικρότερη από την *UpgradeDomainTimeout*. Η απάντηση είναι ότι, επιστρέφεται σφάλμα. Εάν το *UpgradeDomainTimeout* είναι μικρότερο του αθροίσματος των *HealthCheckWaitDuration* και *HealthCheckRetryTimeout*, ή εάν *UpgradeDomainTimeout* είναι μικρότερος από το άθροισμα των *HealthCheckWaitDuration* και *HealthCheckStableDuration*επιστρέφονται σφάλματα.

### <a name="my-upgrades-are-taking-too-long"></a>Μου αναβαθμίσεις διαρκεί πολύ χρόνο

Την ώρα για αναβάθμιση για να ολοκληρώσετε εξαρτάται από το έλεγχοι εύρυθμης λειτουργίας και λήξη περιόδων που καθορίζεται. Έλεγχοι εύρυθμης λειτουργίας και λήξη περιόδων εξαρτώνται από πόσος χρόνος χρειάζεται για να αντιγράψετε, να αναπτύξουν και τη σταθεροποίηση της εφαρμογής. Που πολύ αυστηρές με λήξη περιόδων μπορεί να σημαίνει πιο αποτυχίας αναβαθμίσεις, ώστε να σας συνιστούμε να συντηρητικά ξεκινώντας με μεγαλύτερες λήξη περιόδων.

Ακολουθεί μια γρήγορη υπενθύμιση σε πώς η λήξη περιόδων αλληλεπίδραση με τις ώρες την αναβάθμιση:

Αναβαθμίζει για έναν τομέα της αναβάθμισης δεν μπορεί να ολοκληρωθεί ταχύτερα από *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Αποτυχία αναβάθμισης δεν μπορεί να προκύψουν ταχύτερα από *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

Την ώρα αναβάθμισης για την αναβάθμιση τομέα περιορίζεται από *UpgradeDomainTimeout*.  Εάν *HealthCheckRetryTimeout* και *HealthCheckStableDuration* είναι μηδέν και τα δύο και διατηρεί Μετάβαση εμπρός και πίσω την εύρυθμη λειτουργία της εφαρμογής, στη συνέχεια, την αναβάθμιση τελικά λήγει το χρονικό όριο στην *UpgradeDomainTimeout*. *UpgradeDomainTimeout* ξεκινά μετρώντας προς τα κάτω, όταν αρχίσει η αναβάθμιση για τον τρέχοντα τομέα αναβάθμισης.

## <a name="next-steps"></a>Επόμενα βήματα

[Αναβάθμιση σας εφαρμογή χρησιμοποιώντας Visual Studio](service-fabric-application-upgrade-tutorial.md) σάς καθοδηγεί σε μια εφαρμογή αναβάθμιση χρήση του Visual Studio.

[Αναβάθμιση του Powershell χρήση εφαρμογής](service-fabric-application-upgrade-tutorial-powershell.md) σάς καθοδηγεί σε μια αναβάθμιση εφαρμογών με χρήση του PowerShell.

Να ελέγχετε τον τρόπο αναβαθμίζει την εφαρμογή σας, χρησιμοποιώντας [Παραμέτρους αναβάθμιση](service-fabric-application-upgrade-parameters.md).

Κάνει σας αναβαθμίσεις εφαρμογή συμβατά μαθαίνοντας πώς μπορείτε να χρησιμοποιήσετε [Δεδομένα σειριοποίησης](service-fabric-application-upgrade-data-serialization.md).

Μάθετε πώς μπορείτε να χρησιμοποιήσετε λειτουργίες για προχωρημένους κατά την αναβάθμιση της εφαρμογής σας με αναφορά σε [Θέματα για προχωρημένους](service-fabric-application-upgrade-advanced.md).

Διορθώστε συνήθη προβλήματα στην εφαρμογή αναβαθμίσεις με αναφορά σε τα βήματα στο θέμα [Αντιμετώπιση προβλημάτων εφαρμογής αναβαθμίσεις του στοιχείου](service-fabric-application-upgrade-troubleshooting.md).
 
