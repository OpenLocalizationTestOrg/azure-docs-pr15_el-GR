<properties
   pageTitle="Αρχιτεκτονική αξιόπιστη υπηρεσία | Microsoft Azure"
   description="Επισκόπηση της αρχιτεκτονικής αξιόπιστη υπηρεσίας για τις υπηρεσίες με κατάσταση και χωρίς κατάσταση"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/30/2016"
   ms.author="alanwar"/>

# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Αρχιτεκτονική για τις υπηρεσίες με κατάσταση και χωρίς κατάσταση αξιόπιστη

Μια υπηρεσία αξιόπιστη Azure Service ύφασμα μπορεί να κατάστασης ή χωρίς κατάσταση. Εκτελείται κάθε τύπο υπηρεσίας μέσα σε μια συγκεκριμένη αρχιτεκτονική. Αυτές οι αρχιτεκτονικές περιγράφονται σε αυτό το άρθρο.
Δείτε την [Επισκόπηση αξιόπιστων υπηρεσιών](service-fabric-reliable-services-introduction.md) για περισσότερες πληροφορίες σχετικά με τις διαφορές μεταξύ των υπηρεσιών με κατάσταση και χωρίς κατάσταση.

## <a name="stateful-reliable-services"></a>Κατάσταση αξιόπιστων υπηρεσιών

### <a name="architecture-of-a-stateful-service"></a>Αρχιτεκτονική της κατάστασης υπηρεσίας
![Αρχιτεκτονική διάγραμμα κατάστασης υπηρεσίας](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Υπηρεσία κατάστασης αξιόπιστη

Μια υπηρεσία κατάστασης αξιόπιστη μπορεί να προέρχεται από το StatefulService ή StatefulServiceBase κλάση. Και τα δύο από αυτές τις βασικές κλάσεις παρέχονται από ύφασμα υπηρεσίας. Προσφέρουν διάφορα επίπεδα υποστήριξης και αφαίρεσης για την υπηρεσία κατάστασης για συνεργασία με την υπηρεσία ύφασμα--και τη συμμετοχή ως υπηρεσία μέσα στο σύμπλεγμα ύφασμα υπηρεσίας.

StatefulService προέρχεται από StatefulServiceBase. StatefulServiceBase προσφέρει υπηρεσίες περισσότερη ευελιξία, αλλά απαιτεί περισσότερες Κατανόηση της τα εσωτερικά στοιχεία της υπηρεσίας ύφασμα.
Ανατρέξτε στο θέμα [Επισκόπηση αξιόπιστων υπηρεσιών](service-fabric-reliable-services-introduction.md) και [αξιόπιστη υπηρεσία advanced χρήση](service-fabric-reliable-services-advanced-usage.md) για περισσότερες πληροφορίες σχετικά με τις λεπτομέρειες γραφής υπηρεσίες, χρησιμοποιώντας τις κλάσεις StatefulService και StatefulServiceBase.

Και οι δύο βασικές κλάσεις διαχειριστείτε τη διάρκεια ζωής και ρόλων της εφαρμογής υπηρεσίας. Την υλοποίηση της υπηρεσίας ενδέχεται να αντικαταστήσουν εικονικού μεθόδους είτε βασικής κλάσης εάν την υλοποίηση της υπηρεσίας έχει εργασίας για να το κάνετε σε αυτά τα σημεία του κύκλου ζωής εφαρμογή υπηρεσίας--ή εάν θέλει να δημιουργήσετε ένα αντικείμενο ακρόασης επικοινωνίας. Σημειώστε ότι αν και μια εφαρμογή υπηρεσίας μπορεί να υλοποιήσετε το δικό του αντικειμένου ακρόασης επικοινωνίας εκθέσετε ICommunicationListener, στο διάγραμμα παραπάνω, την παρακολούθηση της επικοινωνίας πραγματοποιείται από ύφασμα υπηρεσίας--όπως την υλοποίηση της υπηρεσίας χρησιμοποιεί ένα πρόγραμμα ακρόασης επικοινωνίας που έχει υλοποιηθεί με ύφασμα υπηρεσίας.

Κατάσταση υπηρεσίας αξιόπιστη χρησιμοποιεί τη Διαχείριση αξιόπιστη κατάσταση για να επωφεληθείτε από αξιόπιστη συλλογών. Αξιόπιστη συλλογές είναι δομές τοπικών δεδομένων που είναι ιδιαίτερα διαθέσιμες για την υπηρεσία--δηλαδή, να είναι πάντα διαθέσιμη, ανεξάρτητα από την υπηρεσία ανακατευθύνσεις. Κάθε τύπο αξιόπιστη συλλογής έχει υλοποιηθεί από μια υπηρεσία παροχής αξιόπιστη κατάστασης.
Για περισσότερες πληροφορίες σχετικά με την αξιόπιστη συλλογές, ανατρέξτε στο θέμα η [Επισκόπηση αξιόπιστη συλλογές](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Διαχείριση αξιόπιστων κατάσταση και τις υπηρεσίες παροχής κατάσταση

Το αντικείμενο το οποίο διαχειρίζεται κατάσταση αξιόπιστων υπηρεσιών παροχής είναι ο διαχειριστής αξιόπιστη κατάσταση. Έχει τη λειτουργικότητα για τη δημιουργία, διαγραφή, απαρίθμηση και βεβαιωθείτε ότι οι υπηρεσίες παροχής αξιόπιστη κατάσταση μόνιμων και ιδιαίτερα διαθέσιμη. Μια παρουσία της υπηρεσίας παροχής αξιόπιστη νομό αντιπροσωπεύει μια παρουσία μιας δομής μόνιμων και ιδιαίτερα διαθέσιμες δεδομένων, όπως ένα λεξικό ή μια ουρά.

Κάθε υπηρεσία παροχής αξιόπιστη κατάσταση εκθέτει ένα περιβάλλον εργασίας που χρησιμοποιείται από μια υπηρεσία κατάστασης για να αλληλεπιδράσετε με την υπηρεσία παροχής αξιόπιστη κατάσταση. Για παράδειγμα, χρησιμοποιείται IReliableDictionary για αλληλεπίδραση με το λεξικό αξιόπιστη, ενώ IReliableQueue χρησιμοποιείται για αλληλεπίδραση με ουρά αξιόπιστη. Όλες τις υπηρεσίες παροχής αξιόπιστη νομό υλοποιεί τη διασύνδεση IReliableState.

Ο διευθυντής αξιόπιστη κατάσταση διαθέτει ένα περιβάλλον εργασίας με το όνομα IReliableStateManager, το οποίο επιτρέπει την πρόσβαση σε αυτήν από μια υπηρεσία κατάστασης. Διασυνδέσεις για την κατάσταση αξιόπιστων υπηρεσιών παροχής επιστρέφονται μέσω IReliableStateManager.

Η διαχείριση αξιόπιστη κατάσταση χρησιμοποιεί μια προσθήκης αρχιτεκτονική, έτσι ώστε οι νέοι τύποι αξιόπιστη συλλογές μπορεί να συνδεθεί δυναμικά.

Αξιόπιστη λεξικό και αξιόπιστη ουρά δημιουργούνται κατά την εκτέλεση του χώρου αποθήκευσης διακριτική υψηλών επιδόσεων, έκδοση.

### <a name="transactional-replicator"></a>Συναλλαγές προγράμματος αναπαραγωγής

Το στοιχείο συναλλαγών αναπαραγωγής είναι υπεύθυνη για την εξασφάλιση ότι η κατάσταση της υπηρεσίας (δηλαδή, η κατάσταση εντός της διαχείρισης αξιόπιστη κατάστασης και των συλλογών αξιόπιστη) είναι συνεπείς σε όλα τα αντίγραφα εκτελεί την υπηρεσία. Επίσης, εξασφαλίζει ότι η κατάσταση είναι διατηρηθεί στο αρχείο καταγραφής. Οι διασυνδέσεις manager αξιόπιστη κατάσταση με το πρόγραμμα αναπαραγωγής συναλλαγών μέσω μηχανισμό ιδιωτικό.

Το πρόγραμμα αναπαραγωγής συναλλαγών χρησιμοποιεί ένα πρωτόκολλο δικτύου για την επικοινωνία κατάσταση με άλλα αντίγραφα της παρουσίας υπηρεσίας, έτσι ώστε όλα τα αντίγραφα έχετε πληροφορίες κατάστασης ενημερωμένο.

Το πρόγραμμα αναπαραγωγής συναλλαγών χρησιμοποιεί ένα αρχείο καταγραφής για να διατηρηθούν οι πληροφορίες κατάστασης, ώστε οι πληροφορίες κατάστασης επιβιώνει διαδικασία ή να παρουσιάζει σφάλμα κόμβο. Το περιβάλλον εργασίας για το αρχείο καταγραφής είναι μέσω μηχανισμό ιδιωτικό.

### <a name="log"></a>Αρχείο καταγραφής

Το στοιχείο καταγραφής παρέχει χώρου αποθήκευσης μόνιμη υψηλών επιδόσεων που μπορεί να βελτιστοποιηθεί για γράψιμο στο περιστρεφόμενη ή οπτικοί δίσκων.  Η σχεδίαση του αρχείου καταγραφής είναι για τη μόνιμη αποθήκευση (π.χ., σκληρό δίσκων) να είναι τοπική τους κόμβους που λειτουργούν με την υπηρεσία κατάστασης. Αυτό επιτρέπει χαμηλής των αδρανειών και υψηλή απόδοση, σε σύγκριση με απομακρυσμένη μόνιμη χώρο αποθήκευσης, το οποίο δεν τοπικό στον κόμβο.

Το στοιχείο καταγραφής χρησιμοποιεί πολλά αρχεία καταγραφής. Υπάρχει κόμβο όλη κοινόχρηστο αρχείο καταγραφής που χρησιμοποιούν όλα τα αντίγραφα καθώς μπορεί να παρέχει το λανθάνων χρόνος χαμηλότερη και υψηλότερη απόδοση για την αποθήκευση δεδομένων κατάστασης. Από προεπιλογή, το κοινόχρηστο αρχείο καταγραφής τοποθετείται στον κατάλογο εργασίας κόμβο ύφασμα υπηρεσία αλλά μπορεί επίσης να ρυθμιστεί να τοποθετηθεί σε μια άλλη θέση, ιδανικά σε δίσκο προορίζεται για μόνο το κοινόχρηστο αρχείο καταγραφής. Κάθε ρεπλίκα για την υπηρεσία έχει επίσης ένα αρχείο καταγραφής αποκλειστικό και το αρχείο καταγραφής αποκλειστικό τοποθετείται εντός της υπηρεσίας καταλόγου εργασίας. Δεν υπάρχει μηχανισμός για να ρυθμίσετε τις παραμέτρους του αρχείου καταγραφής αποκλειστικό να τοποθετηθεί στο σε διαφορετική θέση.

Το κοινόχρηστο αρχείο καταγραφής είναι μια μεταβατικές περιοχή για τη ρεπλίκα οι πληροφορίες κατάστασης, ενώ το αρχείο καταγραφής αποκλειστικό είναι ο τελικός προορισμός όπου έχει διατηρηθεί. Σε αυτήν τη σχεδίαση, οι πληροφορίες κατάστασης είναι πρώτη εγγραφή στο κοινόχρηστο αρχείο καταγραφής και, στη συνέχεια, αμέριμνα μετακινηθεί στο αρχείο καταγραφής αποκλειστικό στο παρασκήνιο. Σε αυτόν τον τρόπο, η εγγραφή στο κοινόχρηστο αρχείο καταγραφής θα έχει το λανθάνων χρόνος χαμηλότερη και υψηλότερη απόδοση που επιτρέπει την υπηρεσία για να κάνετε πιο γρήγορα την πρόοδο.

Διαβάζει και εγγραφές στο κοινόχρηστο αρχείο καταγραφής γίνονται μέσω απευθείας είσοδος/ΈΞΟΔΟΣ preallocated χώρο στο δίσκο για το αρχείο καταγραφής κοινόχρηστο. Για να επιτρέψετε βέλτιστη χρήση χώρου στο δίσκο στη μονάδα δίσκου με αποκλειστικό αρχεία καταγραφής, το αρχείο καταγραφής αποκλειστικό δημιουργείται ως κατακερματισμένο αρχείο NTFS. Σημειώστε ότι αυτό θα σας επιτρέψει να overprovisioning χώρου στο δίσκο και το λειτουργικό σύστημα, θα εμφανιστούν τα αρχεία καταγραφής αποκλειστικό χρησιμοποιώντας πολύ περισσότερο χώρο στο δίσκο από αυτά που είναι στην πραγματικότητα χρησιμοποιούνται.

Εκτός από ένα περιβάλλον ελάχιστους λειτουργίας χρήστη για το αρχείο καταγραφής, το αρχείο καταγραφής είναι γραμμένο με ένα πρόγραμμα οδήγησης πυρήνα. Με εκτελείται ως πρόγραμμα οδήγησης πυρήνα, το αρχείο καταγραφής να παρέχετε την υψηλότερη απόδοση σε όλες τις υπηρεσίες που χρησιμοποιείτε.

Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων του αρχείου καταγραφής, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων κατάστασης αξιόπιστων υπηρεσιών](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Χωρίς κατάσταση υπηρεσίας αξιόπιστη

### <a name="architecture-of-a-stateless-service"></a>Αρχιτεκτονική χωρίς κατάσταση υπηρεσίας
![Διάγραμμα αρχιτεκτονική χωρίς κατάσταση υπηρεσίας](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Χωρίς κατάσταση υπηρεσίας αξιόπιστη

Χωρίς κατάσταση υπηρεσίας υλοποιήσεις προέρχεται από την κλάση StatelessService ή StatelessServiceBase. Η κλάση StatelessServiceBase επιτρέπει μεγαλύτερη ευελιξία από την κλάση StatelessService.
Και οι δύο βασικές κλάσεις διαχειριστείτε τη διάρκεια ζωής και ρόλων μιας υπηρεσίας.

Την υλοποίηση της υπηρεσίας ενδέχεται να αντικαταστήσουν εικονικού μεθόδους είτε βασικής κλάσης, εάν η υπηρεσία έχει εργασίας για να το κάνετε σε αυτά τα σημεία του κύκλου ζωής της υπηρεσίας--ή εάν θέλει να δημιουργήσετε ένα αντικείμενο ακρόασης επικοινωνίας. Σημειώστε ότι αν και η υπηρεσία ενδέχεται να εφαρμόζουν δικό του αντικειμένου ακρόασης επικοινωνίας εκθέσετε ICommunicationListener, στο διάγραμμα παραπάνω, την παρακολούθηση της επικοινωνίας πραγματοποιείται από ύφασμα υπηρεσίας, κατά την υλοποίηση της υπηρεσίας που χρησιμοποιεί ένα πρόγραμμα ακρόασης επικοινωνίας που έχει υλοποιηθεί με ύφασμα υπηρεσίας.

Ανατρέξτε στο θέμα [Επισκόπηση αξιόπιστων υπηρεσιών](service-fabric-reliable-services-introduction.md) και [αξιόπιστη υπηρεσία advanced χρήση](service-fabric-reliable-services-advanced-usage.md) για περισσότερες πληροφορίες σχετικά με τις λεπτομέρειες της γράφετε υπηρεσίες χρησιμοποιώντας τις κλάσεις StatelessService και StatelessServiceBase.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με την υπηρεσία ύφασμα, ανατρέξτε στα θέματα:

[Επισκόπηση της υπηρεσίας αξιόπιστη](service-fabric-reliable-services-introduction.md)

[Γρήγορη εκκίνηση](service-fabric-reliable-services-quick-start.md)

[Επισκόπηση αξιόπιστη συλλογές](service-fabric-reliable-services-reliable-collections.md)

[Αξιόπιστη υπηρεσία χρήση για προχωρημένους](service-fabric-reliable-services-advanced-usage.md)

[Ρύθμιση παραμέτρων της υπηρεσίας αξιόπιστη](service-fabric-reliable-services-configuration.md)  
