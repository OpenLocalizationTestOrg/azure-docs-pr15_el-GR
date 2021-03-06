<properties
   pageTitle="Διαχείριση ενέργειας υπολογισμού σε αποθήκη δεδομένων SQL Azure (επισκόπηση) | Microsoft Azure"
   description="Κλίμακα επιδόσεων ανάληψη δυνατοτήτων στο αποθήκη δεδομένων του SQL Azure. Διαβάθμιση, προσαρμόζοντας DWUs ή παύση και συνέχιση υπολογισμού πόρους για να αποθηκεύσετε κόστους."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Διαχείριση ενέργειας υπολογισμού σε αποθήκη δεδομένων SQL Azure (επισκόπηση)

> [AZURE.SELECTOR]
- [Επισκόπηση](sql-data-warehouse-manage-compute-overview.md)
- [Πύλη](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [ΥΠΌΛΟΙΠΟ](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

Η αρχιτεκτονική της αποθήκη δεδομένων του SQL διαχωρίζει χώρου αποθήκευσης και υπολογισμού, επιτρέποντάς κάθε για να κλιμακωθεί ανεξάρτητα. Ως αποτέλεσμα, μπορείτε να κλιμάκωση απόδοσης κατά την αποθήκευση του κόστους πληρώνοντας μόνο για τις επιδόσεις όταν τα χρειάζεστε. 

Αυτή η επισκόπηση περιγράφει τις ακόλουθες δυνατότητες κλιμάκωσης επιδόσεων του αποθήκη δεδομένων του SQL και παρέχει προτάσεις σχετικά με τον τρόπο και πότε πρέπει να τις χρησιμοποιήσετε. 

- Κλίμακα υπολογιστική ισχύ, προσαρμόζοντας [δεδομένων αποθήκης μονάδες (DWUs)][]
- Παύση ή συνέχιση πόρους υπολογισμού

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Κλίμακα επιδόσεων

Στο αποθήκη δεδομένων του SQL, μπορείτε γρήγορα να κλιμάκωση επιδόσεων ανάληψη ή ξανά με αύξουσες ή φθίνουσες υπολογισμού πόρους CPU, μνήμης και το εύρος ζώνης εισόδου/εξόδου. Για να κλιμακωθεί επιδόσεων, μόνο που πρέπει να κάνετε είναι να προσαρμόσετε τον αριθμό των [δεδομένων αποθήκης μονάδες (DWUs)][] που εκχωρεί αποθήκη δεδομένων του SQL για τη βάση δεδομένων. Αποθήκη δεδομένων του SQL κάνει την αλλαγή γρήγορα και χειρισμού όλες οι υποκείμενα αλλαγές υλικού ή λογισμικού.

Υπερβεί είναι οι ημέρες όπου πρέπει να την έρευνα για τον τύπο των επεξεργαστών, πόση μνήμη ή τον τύπο του χώρου αποθήκευσης πρέπει να έχουν εξαιρετική απόδοση στην αποθήκη δεδομένων σας. Όταν εισάγετε σας αποθήκη δεδομένων στο cloud, δεν χρειάζεται πλέον να ασχοληθείτε με θέματα χαμηλού επιπέδου υλικού. Αντί για αυτό, αποθήκη δεδομένων του SQL σας ζητά αυτήν την ερώτηση: της ταχύτητας κίνησης θέλετε να αναλύσετε τα δεδομένα σας; 

### <a name="how-do-i-scale-performance"></a>Πώς να περιορίσω το μέγεθος επιδόσεων;

Για να elastically αύξηση ή μείωση του power υπολογισμού, απλώς αλλάξτε τη ρύθμιση [δεδομένων αποθήκης μονάδες (DWUs)][] για τη βάση δεδομένων. Επιδόσεις, θα αυξήσετε γραμμικά καθώς προσθέτετε περισσότερες DWU.  Στο ανώτερο DWU επίπεδα, πρέπει να προσθέσετε περισσότερους από 100 DWUs να παρατηρήσετε σημαντική βελτίωση των επιδόσεων. Για να σας βοηθήσει να επιλέξετε σημαντικά σημεία μεταπήδησης στο DWUs, προσφέρουμε τα επίπεδα DWU που θα σας δώσει τα καλύτερα αποτελέσματα.
 
Για να προσαρμόσετε DWUs, μπορείτε να χρησιμοποιήσετε οποιαδήποτε από τις παρακάτω μεθόδους μεμονωμένα.

- [Κλίμακα υπολογιστική ισχύ με την πύλη Azure][]
- [Κλίμακα υπολογιστική ισχύ με το PowerShell][]
- [Κλίμακα power υπολογισμού με REST API][]
- [Κλίμακα υπολογιστική ισχύ με TSQL][]

### <a name="how-many-dwus-should-i-use"></a>Πόσες DWUs πρέπει να χρησιμοποιήσω;
 
Απόδοση σε αποθήκη δεδομένων του SQL κλίμακες γραμμικά, και η αλλαγή από ένα υπολογισμού κλίμακα σε ένα άλλο (Καλώς από 100 DWUs να 2000 DWUs) θα συμβεί σε δευτερόλεπτα. Αυτό σας δίνει την ευελιξία να πειραματιστείτε με διάφορες ρυθμίσεις DWU μέχρι να εντοπίσετε το σενάριο που ταιριάζει καλύτερα.

Για να κατανοήσετε τι είναι το ιδανικό τιμή DWU, δοκιμάστε να κλίμακας προς τα επάνω ή προς τα κάτω και εκτέλεσης ερωτημάτων μερικά μετά τη φόρτωση των δεδομένων σας. Επειδή το "κλίμακα" είναι γρήγορη, μπορείτε να δοκιμάσετε διαφορετικά επίπεδα επιδόσεων σε μία ώρα ή λιγότερο έναν αριθμό. Έχετε υπόψη σας, που έχει σχεδιαστεί αποθήκη δεδομένων του SQL για την επεξεργασία μεγάλες ποσότητες δεδομένων και για να δείτε τις πραγματικές του δυνατότητες για κλιμάκωση, ιδίως σε μεγαλύτερο κλίμακες προσφέρουμε, θα θέλετε να χρησιμοποιήσετε ένα μεγάλο σύνολο δεδομένων, η οποία προσεγγίσεις ή υπερβαίνει 1 TB.

Συστάσεις για την εύρεση τη βέλτιστη DWU για το φόρτο εργασίας:

1. Για μια αποθήκη δεδομένων στην ανάπτυξη, ξεκινήστε επιλέγοντας ένα μικρό αριθμό των DWUs.  Ένα καλό σημείο εκκίνησης είναι DW400 ή DW200.
2. Παρακολούθηση των επιδόσεων του εφαρμογής, παρακολούθηση επιλεγμένο τον αριθμό των DWUs σε σύγκριση με τις επιδόσεις παρατηρείτε.
3. Προσδιορίστε τον όγκο πιο γρήγορο ή πιο αργές επιδόσεις πρέπει να για να φτάσετε στο επίπεδο βέλτιστη απόδοση για τις απαιτήσεις σας, υπό την προϋπόθεση ότι γραμμική κλίμακα.
4. Αυξήστε ή μειώστε τον αριθμό των DWUs ανάλογα με τον τρόπο πολύ πιο γρήγορα ή πιο αργά θέλετε το φόρτο εργασίας για να εκτελέσετε. Η υπηρεσία θα απαντήσετε γρήγορα και να προσαρμόσετε τους πόρους υπολογισμού για να πληροί τις απαιτήσεις νέα DWU.
5. Συνεχίστε προσαρμογές μέχρι να φτάσετε ένα επίπεδο βέλτιστη απόδοση για τις ανάγκες της επιχείρησής σας.

### <a name="when-should-i-scale-dwus"></a>Πότε πρέπει να περιορίσω το μέγεθος DWUs;

Όταν χρειάζεστε πιο γρήγορη αποτελέσματα, αύξηση σας DWUs και πληρώνετε για καλύτερη απόδοση.  Όταν χρειάζεται λιγότερο υπολογιστική ισχύ, μείωση του DWUs και πληρωμή μόνο για ό, τι χρειάζεστε. 

Συστάσεις για το χρόνο για να κλιμακωθεί DWUs:

1. Εάν η εφαρμογή σας έχει ένα κυμαίνεται φόρτο εργασίας, κλίμακα DWU επίπεδα προς τα επάνω ή προς τα κάτω για να χωρέσει κορυφών και χαμηλά σημεία. Για παράδειγμα, εάν το φόρτο εργασίας κορυφών συνήθως στο τέλος του μήνα, Σχεδιασμός για να προσθέσετε περισσότερες DWUs κατά τη διάρκεια αυτές τις ημέρες κορύφωσης και, στη συνέχεια, κλιμάκωση προς τα κάτω, όταν είναι η μέγιστη περίοδος πάνω από.
2. Πριν να εκτελέσετε μια λειτουργία έντονη δεδομένων κατά τη φόρτωση ή μετασχηματισμό, κλιμάκωσης DWUs έτσι ώστε τα δεδομένα σας είναι διαθέσιμο πιο γρήγορα.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Παύση υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Για να διακόψετε προσωρινά μια βάση δεδομένων, χρησιμοποιήστε οποιαδήποτε από τις παρακάτω μεθόδους μεμονωμένα.

- [Παύση υπολογισμού με την πύλη Azure][]
- [Παύση υπολογισμού με το PowerShell][]
- [Παύση υπολογισμού με REST API][]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Βιογραφικό σημείωμα υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Για να συνεχίσετε μια βάση δεδομένων, χρησιμοποιήστε οποιαδήποτε από τις παρακάτω μεθόδους μεμονωμένα.

- [Βιογραφικό σημείωμα υπολογισμού με την πύλη Azure][]
- [Βιογραφικό σημείωμα υπολογισμού με το PowerShell][]
- [Βιογραφικό σημείωμα υπολογισμού με REST API][]

## <a name="permissions"></a>Δικαιώματα

Κλιμάκωση της βάσης δεδομένων θα απαιτούν τα δικαιώματα που περιγράφονται σε [ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ][].  Προσωρινή διακοπή και συνέχιση απαιτεί το δικαίωμα [Συμβολής DB SQL][] , ειδικά Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Επόμενα βήματα
Ανατρέξτε στα ακόλουθα άρθρα που θα σας βοηθήσουν να κατανοήσετε τις έννοιες ορισμένες επιπλέον επιδόσεων κλειδιού:

- [Διαχείριση φόρτο εργασίας και ταυτόχρονης εκτέλεσης][]
- [Επισκόπηση Σχεδίαση πίνακα][]
- [Πίνακας διανομής][]
- [Δημιουργία ευρετηρίου πίνακα][]
- [Δημιουργία πίνακα διαμερισμάτων][]
- [Στατιστικά στοιχεία πίνακα][]
- [Βέλτιστες πρακτικές][]

<!--Image reference-->

<!--Article references-->
[μονάδες αποθήκη δεδομένων (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Κλίμακα υπολογιστική ισχύ με την πύλη Azure]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Κλίμακα υπολογιστική ισχύ με το PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Κλίμακα power υπολογισμού με REST API]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Κλίμακα υπολογιστική ισχύ με TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Παύση υπολογισμού με την πύλη Azure]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Παύση υπολογισμού με το PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Παύση υπολογισμού με REST API]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Βιογραφικό σημείωμα υπολογισμού με την πύλη Azure]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Βιογραφικό σημείωμα υπολογισμού με το PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Βιογραφικό σημείωμα υπολογισμού με REST API]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Διαχείριση φόρτο εργασίας και ταυτόχρονης εκτέλεσης]: ./sql-data-warehouse-develop-concurrency.md
[Επισκόπηση Σχεδίαση πίνακα]: ./sql-data-warehouse-tables-overview.md
[Πίνακας διανομής]: ./sql-data-warehouse-tables-distribute.md
[Δημιουργία ευρετηρίου πίνακα]: ./sql-data-warehouse-tables-index.md
[Δημιουργία πίνακα διαμερισμάτων]: ./sql-data-warehouse-tables-partition.md
[Στατιστικά στοιχεία πίνακα]: ./sql-data-warehouse-tables-statistics.md
[Βέλτιστες πρακτικές]: ./sql-data-warehouse-best-practices.md 
[development overview]: ./sql-data-warehouse-overview-develop.md

[Συνεργάτης DB SQL]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ΤΡΟΠΟΠΟΊΗΣΗ ΒΆΣΗΣ ΔΕΔΟΜΈΝΩΝ]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
