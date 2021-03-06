<properties
   pageTitle="Αδιάκοπη παροχή υπηρεσιών στο cloud - βάσης δεδομένων αποκατάστασης - βάση δεδομένων SQL | Microsoft Azure"
   description="Μάθετε πώς η βάση δεδομένων SQL Azure υποστηρίζει αδιάκοπη παροχή υπηρεσιών cloud και ανάκτηση της βάσης δεδομένων και διατήρηση της κρίσιμης σημασίας cloud εφαρμογές που εκτελούνται."
   keywords="συνεχής λειτουργία, αδιάκοπη παροχή υπηρεσιών cloud, αποκατάσταση βάσης δεδομένων, ανάκτηση της βάσης δεδομένων"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Επισκόπηση των αδιάκοπη με βάση δεδομένων SQL Azure

Αυτή η επισκόπηση περιγράφει τις δυνατότητες που παρέχει βάση δεδομένων SQL Azure για αδιάκοπη και αποκατάσταση. Παρέχει επιλογές, συστάσεις και προγράμματα εκμάθησης για την ανάκτηση από ενοχλητικούς συμβάντα που μπορεί να προκαλέσει απώλεια δεδομένων ή ως αποτέλεσμα της βάσης δεδομένων και την εφαρμογή για να μην είναι πλέον διαθέσιμο. Συζήτησης περιλαμβάνει τι πρέπει να κάνετε όταν ένας χρήστης ή σφάλμα εφαρμογής επηρεάζει ακεραιότητας δεδομένων, μια περιοχή Azure περιλαμβάνει ένα μη διαθεσιμότητα ή την εφαρμογή σας απαιτεί συντήρηση. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Δυνατότητες βάσης δεδομένων SQL που μπορείτε να χρησιμοποιήσετε για την παροχή αδιάκοπη παροχή υπηρεσιών

Βάση δεδομένων SQL παρέχει διάφορες δυνατότητες συνέχειας επιχειρήσεις, συμπεριλαμβανομένων των αυτόματη δημιουργία αντιγράφων ασφαλείας και αναπαραγωγή προαιρετικό βάσης δεδομένων. Κάθε μία με διαφορετικά χαρακτηριστικά για εκτιμώμενη αποκατάστασης χρόνου (εισαγωγή) και ενδεχόμενη απώλεια δεδομένων για τις πρόσφατες συναλλαγές. Μόλις κατανοήσετε αυτές τις επιλογές, μπορείτε να επιλέξετε μεταξύ τους - και, στις περισσότερες περιπτώσεις, χρησιμοποιούνται μαζί για διαφορετικά σενάρια. Κατά την ανάπτυξη του σχεδίου συνέχειας επιχειρήσεις, πρέπει να κατανοήσετε τον μέγιστο χρόνο αποδεκτή πριν από την εφαρμογή ανακτά πλήρως μετά το συμβάν ενοχλητικούς - αυτός είναι ο στόχος σας αποκατάστασης χρόνου (RTO). Πρέπει επίσης να κατανοήσετε τη μέγιστη ποσότητα πρόσφατες ενημερώσεις δεδομένων (χρονικό διάστημα) η εφαρμογή μπορεί να αποδεχτεί τις χάσετε όταν γίνεται ανάκτηση μετά από το συμβάν ενοχλητικούς - στόχος σημείου αποκατάστασης (RPO). 

Ο παρακάτω πίνακας συγκρίνει την εισαγωγή και RPO για τα τρία πιο συνηθισμένα σενάρια.

| Η δυνατότητα |  Βασικού επιπέδου | Τυπική σειρά  | Επίπεδο Premium |
|---|---|---|---|
| Τοποθετήστε το δείκτη στο χρόνο επαναφορά από αντίγραφα ασφαλείας | Οποιοδήποτε σημείο επαναφοράς από 7 ημέρες   | Οποιοδήποτε σημείο επαναφοράς 35 μέρες  | Οποιοδήποτε σημείο επαναφοράς 35 μέρες |
Παν-επαναφορά από αναπαραχθούν παν αντίγραφα ασφαλείας | Εισαγωγή < 12 ώρες, RPO < 1h   | Εισαγωγή < 12 ώρες, RPO < 1h   | Εισαγωγή < 12 ώρες, RPO < 1h |
|Ενεργό παν-αναπαραγωγής | Εισαγωγή < 30s, RPO < 5s   | Εισαγωγή < 30s, RPO < 5s | Εισαγωγή < 30s, RPO < 5s |


### <a name="use-database-backups-to-recover-a-database"></a>Χρήση βάσης δεδομένων αντιγράφων ασφαλείας για να ανακτήσετε μια βάση δεδομένων

Βάση δεδομένων SQL πραγματοποιεί αυτόματα ένα συνδυασμό πλήρη αντίγραφα ασφαλείας βάσης δεδομένων κάθε εβδομάδα, διακριτική της βάσης δεδομένων αντιγράφων ασφαλείας ανά ώρα και δημιουργία αντιγράφων ασφαλείας αρχείο καταγραφής συναλλαγών κάθε πέντε λεπτά για να προστατεύσετε την επιχείρησή σας από απώλεια δεδομένων. Αυτά τα αντίγραφα ασφαλείας αποθηκεύονται στο χώρο αποθήκευσης τοπικά πλεονάζοντα για 35 ημέρες για τις βάσεις δεδομένων σε τα επίπεδα υπηρεσίας τυπική και Premium και επτά ημέρες για τις βάσεις δεδομένων στο επίπεδο βασικές υπηρεσίας - δείτε [επίπεδα υπηρεσίας](sql-database-service-tiers.md) για περισσότερες λεπτομέρειες σχετικά με επίπεδα υπηρεσίας. Αν η περίοδος διατήρησης για το επίπεδο υπηρεσιών που δεν πληροί τις απαιτήσεις επιχειρήσεις σας, μπορείτε να αυξήσετε την περίοδο διατήρησης, [αλλάζοντας το επίπεδο υπηρεσιών](sql-database-scale-up.md). Πλήρης και διακριτική βάση δεδομένων αντιγράφων ασφαλείας αναπαράγονται επίσης σε ένα [Κέντρο δεδομένων ζεύγη](../best-practices-availability-paired-regions.md) για την προστασία από ένα μη διαθεσιμότητα κέντρο δεδομένων - δείτε [βάση δεδομένων της αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md) για περισσότερες λεπτομέρειες.

Μπορείτε να χρησιμοποιήσετε αυτές τις αυτόματες βάσης δεδομένων αντιγράφων ασφαλείας για να ανακτήσετε μια βάση δεδομένων από διάφορες ενοχλητικούς συμβάντα, τόσο μέσα σε κέντρο δεδομένων σας και σε ένα άλλο κέντρου δεδομένων. Χρήση βάσης δεδομένων αυτόματης δημιουργίας αντιγράφων ασφαλείας, την εκτιμώμενη ώρα της ανάκτησης εξαρτάται από διάφορους παράγοντες όπως ο συνολικός αριθμός των βάσεων δεδομένων ανακτήσετε στην ίδια περιοχή κατά την ίδια στιγμή, το μέγεθος της βάσης δεδομένων, το μέγεθος του αρχείου καταγραφής συναλλαγών και εύρος ζώνης δικτύου. Στις περισσότερες περιπτώσεις, η ώρα αποκατάστασης είναι μικρότερη από 12 ώρες. Όταν γίνεται ανάκτηση σε άλλη περιοχή δεδομένων, η ενδεχόμενη απώλεια δεδομένων περιορίζεται σε 1 ώρα από τα πλεονάζοντα παν αποθήκευσης ωριαία διακριτική βάσης δεδομένων αντιγράφων ασφαλείας. 

> [AZURE.IMPORTANT] Για να ανακτήσετε χρησιμοποιώντας την αυτόματη δημιουργία αντιγράφων ασφαλείας, πρέπει να είστε μέλος του ρόλου συμβολής SQL Server ή ο κάτοχος της συνδρομής - δείτε [RBAC: ενσωματωμένη ρόλους](../active-directory/role-based-access-built-in-roles.md). Μπορείτε να ανακτήσετε χρησιμοποιώντας την πύλη του Azure, PowerShell ή το REST API. Δεν μπορείτε να χρησιμοποιήσετε Transact-SQL.

Χρησιμοποιήστε αυτόματη δημιουργία αντιγράφων ασφαλείας ως το μηχανισμό συνέχειας και αποκατάστασης επιχειρήσεις εάν η εφαρμογή σας:

- Δεν θεωρείται αποστολής κρίσιμη.
- Δεν διαθέτει μια σύνδεση SLA, επομένως, ο χρόνος εκτός λειτουργίας από 24 ώρες ή περισσότερο θα έχει ως αποτέλεσμα οικονομική ευθύνη.
- Έχει χαμηλή ταχύτητα αλλαγή δεδομένων (χαμηλή συναλλαγές ανά ώρα) και να χάσετε προς τα επάνω σε ώρα της αλλαγής είναι μια απώλεια δεδομένων αποδεκτή. 
- Είναι το κόστος διάκριση πεζών-κεφαλαίων. 

Εάν χρειάζεστε πιο γρήγορη ανάκτησης, χρησιμοποιήστε [Ενεργό παν-αναπαραγωγής](sql-database-geo-replication-overview.md) , (όπως εξηγήθηκε Επόμενο). Εάν πρέπει να έχετε τη δυνατότητα να ανακτήσετε δεδομένα από μια περίοδο παλιότερα από 35 ημέρες, εξετάστε το ενδεχόμενο να αρχειοθέτηση της βάσης δεδομένων τακτικά σε ένα αρχείο BACPAC (ένα συμπιεσμένο αρχείο που περιέχει το σχήμα της βάσης δεδομένων και σχετικών δεδομένων) αποθηκεύονται στο χώρο αποθήκευσης αντικειμένων blob του Azure ή σε άλλη θέση της επιλογής σας. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να δημιουργήσετε μια αρχειοθήκη ενημερώσετε συνεπή βάσης δεδομένων, ανατρέξτε στο θέμα [Δημιουργία ενός αντιγράφου της βάσης δεδομένων](sql-database-copy.md) και [Εξαγωγή το αντίγραφο της βάσης δεδομένων](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Χρήση ενεργό παν-αναπαραγωγής για μείωση του χρόνου ανάκτησης και τον περιορισμό που σχετίζεται με μια αποκατάσταση απώλεια δεδομένων

Εκτός από πρόγραμμα δημιουργίας αντιγράφων ασφαλείας βάσης δεδομένων για την ανάκτηση της βάσης δεδομένων σε περίπτωση διαταραχή επιχειρήσεις, μπορείτε να χρησιμοποιήσετε [Ενεργό παν-αναπαραγωγής](sql-database-geo-replication-overview.md) για να ρυθμίσετε τις παραμέτρους μιας βάσης δεδομένων για να έχετε έως και τέσσερα αναγνώσιμο δευτερεύοντα βάσεις δεδομένων στις περιοχές της επιλογής σας. Αυτές οι δευτερεύουσες βάσεις δεδομένων διατηρούνται συγχρονισμένοι με την κύρια βάση δεδομένων, χρησιμοποιώντας ένα μηχανισμό ασύγχρονης αναπαραγωγής. Αυτή η δυνατότητα χρησιμοποιείται για την προστασία από επιχειρήσεις διαταραχή σε περίπτωση μια μη διαθεσιμότητα του κέντρου δεδομένων ή κατά την αναβάθμιση μιας εφαρμογής. Ενεργό παν-αναπαραγωγής μπορεί επίσης να χρησιμοποιηθεί για την παροχή καλύτερες επιδόσεις ερωτημάτων για ερωτήματα μόνο για ανάγνωση σε γεωγραφικά διεσπαρμένη χρήστες.

Εάν η κύρια βάση δεδομένων χωρίς σύνδεση μεταβαίνει απροσδόκητα ή πρέπει να την μεταφέρω εκτός σύνδεσης για δραστηριότητες συντήρησης, μπορείτε γρήγορα να προβιβάσετε δευτερεύον για να γίνει κύρια (ονομάζεται επίσης ανακατεύθυνσης) και ρύθμιση παραμέτρων των εφαρμογών για να συνδεθείτε με το πρωτεύον που μόλις Προβιβασμένες. Με μια προγραμματισμένη ανακατεύθυνση, υπάρχει απώλεια δεδομένων. Με μια μη προγραμματισμένη ανακατεύθυνση, μπορεί να υπάρχουν ορισμένες ελάχιστο απώλεια δεδομένων για πολύ πρόσφατες συναλλαγές λόγω της φύσης της ασύγχρονης αναπαραγωγής. Μετά την ανακατεύθυνσης, μπορείτε να νεότερες αποκατάσταση μετά από - σύμφωνα με ένα πρόγραμμα ή όταν συνδεθείτε ξανά το κέντρο δεδομένων. Σε κάθε περίπτωση, οι χρήστες αντιμετωπίσετε ένα μικρό τμήμα χρόνου εκτός λειτουργίας και πρέπει να συνδεθείτε ξανά. 

> [AZURE.IMPORTANT] Για να χρησιμοποιήσετε ενεργό παν-αναπαραγωγής, πρέπει να είστε ο κάτοχος της συνδρομής ή να έχετε δικαιώματα διαχειριστή στον SQL Server. Μπορείτε να ρυθμίσετε τις παραμέτρους και ανακατεύθυνσης χρησιμοποιώντας την πύλη του Azure, PowerShell ή το REST API χρησιμοποιώντας τα δικαιώματα της συνδρομής ή τη χρήση Transact-SQL χρησιμοποιώντας τα δικαιώματα μέσα σε SQL Server.

Χρησιμοποιήστε ενεργό παν-αναπαραγωγής, εάν η εφαρμογή σας ικανοποιεί κάποια από αυτά τα κριτήρια:

- Είναι σημαντικό αποστολής.
- Έχει ένα επίπεδο σύμβαση παροχής υπηρεσιών (SLA) που δεν επιτρέπει την 24 ώρες ή περισσότερο από τη διακοπή λειτουργίας.
- Χρόνος εκτός λειτουργίας θα έχει ως αποτέλεσμα οικονομική ευθύνη.
- Έχει υψηλή ταχύτητα δεδομένων είναι υψηλή αλλαγή και να χάσετε ώρας δεδομένων δεν είναι αποδεκτή.
- Το πρόσθετο κόστος του ενεργού παν-αναπαραγωγής είναι χαμηλότερη από την πιθανή οικονομική ευθύνη και σχετικές απώλεια της επιχείρησής.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Ανάκτηση μιας βάσης δεδομένων μετά από ένα χρήστη ή εφαρμογή σφάλμα

* Κανείς είναι ιδανική! Ένας χρήστης μπορεί να διαγράψετε κατά λάθος ορισμένα δεδομένα, αποθέστε κατά λάθος ένα σημαντικό πίνακα ή ακόμα και απόρριψη μιας ολόκληρης βάσης δεδομένων. Ή, μια εφαρμογή μπορεί να αντικαταστήσετε κατά λάθος καλή δεδομένων με εσφαλμένες δεδομένα οφείλεται σε μια εφαρμογή βλάβη. 

Σε αυτό το σενάριο, πρόκειται για τις επιλογές αποκατάστασης.

### <a name="perform-a-point-in-time-restore"></a>Εκτελέστε μια επαναφορά σε δεδομένη χρονική στιγμή

Μπορείτε να χρησιμοποιήσετε την αυτόματη δημιουργία αντιγράφων ασφαλείας για να ανακτήσετε ένα αντίγραφο της βάσης δεδομένων σας σε ένα γνωστό καλό σημείο στο χρόνου, υπό την προϋπόθεση ότι ώρα εντός της περιόδου διατήρησης βάσης δεδομένων. Μετά την επαναφορά της βάσης δεδομένων, μπορείτε να αντικαταστήσετε την αρχική βάση δεδομένων με την Επαναφορά βάσης δεδομένων ή να αντιγράψετε τα δεδομένα απαιτείται από το επαναφέρει δεδομένα στη βάση δεδομένων του αρχικού. Εάν η βάση δεδομένων χρησιμοποιεί Active παν-αναπαραγωγής, συνιστάται να αντιγράψετε τα απαιτούμενα δεδομένα από το αντίγραφο επαναφέρει στην αρχική βάση δεδομένων. Εάν δεν μπορείτε να αντικαταστήσετε την αρχική βάση δεδομένων με την Επαναφορά βάσης δεδομένων, θα πρέπει να ρυθμίσει ξανά τις παραμέτρους και να συγχρονίσετε ξανά ενεργό αναπαραγωγής παν (το οποίο μπορεί να διαρκέσει αρκετά κάποιος χρόνος για μια μεγάλη βάση δεδομένων). 

Για περισσότερες πληροφορίες και για λεπτομερή βήματα για την επαναφορά μιας βάσης δεδομένων σε ένα σημείο στο χρόνο με την πύλη Azure ή χρησιμοποιώντας το PowerShell, ανατρέξτε στο θέμα [Επαναφορά σε δεδομένη χρονική στιγμή](sql-database-recovery-using-backups.md#point-in-time-restore). Δεν μπορείτε να ανακτήσετε χρησιμοποιώντας την Transact-SQL.

### <a name="restore-a-deleted-database"></a>Επαναφορά διαγραμμένων βάσης δεδομένων

Εάν η βάση δεδομένων έχει διαγραφεί αλλά λογική διακομιστή δεν έχει διαγραφεί, μπορείτε να επαναφέρετε τη διαγραμμένη βάση δεδομένων στο σημείο στο οποίο έχει διαγραφεί. Αυτό επαναφέρει ένα αντίγραφο ασφαλείας της βάσης δεδομένων με τον ίδιο λογική SQL server από την οποία έχει διαγραφεί. Μπορείτε να τον επαναφέρετε χρησιμοποιώντας το αρχικό όνομα ή να παρέχουν ένα νέο όνομα ή την Επαναφορά βάσης δεδομένων.

Για περισσότερες πληροφορίες και για λεπτομερή βήματα για την επαναφορά μιας διαγραμμένης βάσης δεδομένων με την πύλη Azure ή χρησιμοποιώντας το PowerShell, ανατρέξτε στο θέμα [Επαναφορά διαγραμμένων βάσης δεδομένων](sql-database-recovery-using-backups.md#deleted-database-restore). Δεν μπορείτε να επαναφέρετε χρησιμοποιώντας την Transact-SQL.

> [AZURE.IMPORTANT] Εάν ο διακομιστής λογική διαγραφεί, δεν μπορείτε να ανακτήσετε μια διαγραμμένη βάση δεδομένων. 

### <a name="import-from-a-database-archive"></a>Εισαγωγή από μια αρχειοθήκη βάσης δεδομένων

Εάν η απώλεια δεδομένων παρουσιάστηκε έξω από την τρέχουσα περίοδο διατήρησης για αυτόματη δημιουργία αντιγράφων ασφαλείας και έχουν αρχειοθέτηση της βάσης δεδομένων, μπορείτε να κάνετε [Εισαγωγή ενός αρχείου BACPAC αρχειοθετημένα](sql-database-import.md) σε μια νέα βάση δεδομένων. Σε αυτό το σημείο, μπορείτε να αντικαταστήσετε την αρχική βάση δεδομένων με τη βάση δεδομένων που έχουν εισαχθεί ή να αντιγράψετε τα δεδομένα που απαιτείται από τα εισαγόμενα δεδομένα στη βάση δεδομένων του αρχικού. 

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Ανάκτηση μιας βάσης δεδομένων σε άλλη περιοχή από ένα μη διαθεσιμότητα κέντρο Azure τοπικά δεδομένα

<!-- Explain this scenario -->

Παρόλο που σπάνια, ένα κέντρο δεδομένων Azure μπορεί να έχει ένα μη διαθεσιμότητα. Όταν προκύπτει μια μη διαθεσιμότητα, προκαλεί διαταραχή επιχειρήσεις που μπορεί να τελευταίος μόνο μερικά λεπτά ή μπορεί να διαρκέσει ώρες. 

- Μία επιλογή είναι να περιμένετε για τη βάση δεδομένων για να συνδεθείτε και πάλι όταν ολοκληρωθεί η μη διαθεσιμότητα του κέντρου δεδομένων. Αυτό λειτουργεί για τις εφαρμογές που μπορούν να πρέπει να έχετε τη βάση δεδομένων χωρίς σύνδεση. Για παράδειγμα, ένα έργο ανάπτυξης ή μια δωρεάν δοκιμαστική έκδοση δεν χρειάζεται να εργαστούν στο συνεχώς. Όταν ένα κέντρο δεδομένων περιέχει ένα μη διαθεσιμότητα, δεν θα γνωρίζετε πώς τα μη διαθεσιμότητα θα διαρκέσει, ώστε η επιλογή αυτή λειτουργεί μόνο εάν δεν χρειάζεστε τη βάση δεδομένων για κάποιο χρονικό διάστημα.
- Μια άλλη επιλογή είναι να είτε ανακατεύθυνσης σε άλλη περιοχή δεδομένων εάν θα χρησιμοποιείτε ενεργό παν-αναπαραγωγή ή την ανάκτηση χρησιμοποιώντας παν πλεονάζοντα βάσης δεδομένων αντιγράφων ασφαλείας (παν-επαναφορά). Ανακατεύθυνση διαρκεί μόνο μερικά δευτερόλεπτα, ενώ αποκατάστασης από αντίγραφα ασφαλείας μεταφέρει ώρες.

Όταν εκτελείτε την ενέργεια, το χρόνο που χρειάζεται να ανακτήσετε και πόσο απώλεια δεδομένων που λαμβάνετε σε περίπτωση μια μη διαθεσιμότητα του κέντρου δεδομένων εξαρτάται από το πώς να αποφασίσετε να χρησιμοποιήσετε τις δυνατότητες συνέχειας επιχειρήσεις που περιγράφονται παραπάνω στην εφαρμογή σας. Στην πραγματικότητα, μπορείτε να επιλέξετε να χρησιμοποιήσετε ένα συνδυασμό των αντιγράφων ασφαλείας βάσης δεδομένων και η ενεργή παν-αναπαραγωγή, ανάλογα με τις απαιτήσεις της εφαρμογής. Για πληροφορίες σχετικά με θέματα σχεδίασης εφαρμογής για μεμονωμένο βάσεων δεδομένων και για ελαστικά χώρους συγκέντρωσης χρήσης αυτών των δυνατοτήτων συνέχειας επιχειρήσεις, ανατρέξτε στο θέμα [Σχεδίαση μια εφαρμογή για το cloud αποκατάσταση](sql-database-designing-cloud-solutions-for-disaster-recovery.md) και [στρατηγικές αποκατάστασης από καταστροφή ελαστικότητας χώρου συγκέντρωσης](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Οι παρακάτω ενότητες παρέχουν μια επισκόπηση των βημάτων για να ανακτήσετε χρησιμοποιώντας αντίγραφα ασφαλείας βάσης δεδομένων ή ενεργό παν-αναπαραγωγής. Για λεπτομερείς οδηγίες, συμπεριλαμβανομένων σχεδιασμού απαιτήσεις, βήματα ανάκτησης δημοσίευση και πληροφορίες σχετικά με τον τρόπο για να προσομοιώσετε μια μη διαθεσιμότητα για να εκτελέσετε μια διερεύνησης αποκατάστασης από καταστροφή, ανατρέξτε στο θέμα [Ανάκτηση μια βάση δεδομένων SQL από ένα μη διαθεσιμότητα](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Προετοιμασία για μια μη διαθεσιμότητα

Ανεξάρτητα από τη δυνατότητα συνέχειας επιχειρήσεις που χρησιμοποιείτε, θα πρέπει:

- Προσδιορίστε και προετοιμασία του διακομιστή προορισμού, συμπεριλαμβανομένων των κανόνες τείχους προστασίας επιπέδου διακομιστή, συνδέσεις και τα δικαιώματα επιπέδου κύρια βάση δεδομένων.
- Καθορίσετε τον τρόπο για να ανακατευθύνετε προγράμματα-πελάτες και σε εφαρμογές προγράμματος-πελάτη στο νέο διακομιστή
- Έγγραφο άλλες εξαρτήσεις, όπως τον έλεγχο ρυθμίσεων και ειδοποιήσεων 
 
Εάν δεν Προγραμματισμός και προετοιμασία σωστά, μεταφέρετε τις εφαρμογές σας online αφού ανακατεύθυνσης ή αποκατάστασης διαρκεί επιπλέον και ελκυστικό απαιτούν επίσης αντιμετώπιση προβλημάτων ταυτόχρονα από φόρτο - έναν εσφαλμένο συνδυασμό.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Ανακατεύθυνση να αναπαραχθούν παν δευτερεύοντα βάσης δεδομένων 

Εάν χρησιμοποιείτε το ενεργό παν-αναπαραγωγής ως το μηχανισμό ανάκτησης, [επιβάλετε ανακατεύθυνσης για να αναπαραχθούν παν δευτερεύον](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). Μέσα σε δευτερόλεπτα, τη δευτερεύουσα προωθείται να γίνει η νέα κύρια και είστε έτοιμοι να εγγράψετε νέες συναλλαγές και να απαντήσετε σε τυχόν ερωτήματα - με μερικά μόνο δευτερόλεπτα απώλειας δεδομένων για τα δεδομένα που δεν είχαν ακόμη αναπαραγωγή. Για πληροφορίες σχετικά με την αυτοματοποίηση της διαδικασίας ανακατεύθυνση, ανατρέξτε στο θέμα [Σχεδίαση μια εφαρμογή για το cloud αποκατάσταση](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Όταν το κέντρο δεδομένων αποκατασταθεί στο Internet, μπορείτε να κάνετε αποκατάσταση μετά από για να το αρχικό πρωτεύον (ή όχι).

### <a name="perform-a-geo-restore"></a>Εκτελέσετε παν-επαναφορά 

Εάν χρησιμοποιείτε αυτόματη δημιουργία αντιγράφων ασφαλείας με την αναπαραγωγή παν πλεονάζοντα χώρο αποθήκευσης ως το μηχανισμό ανάκτησης, [Προετοιμασία αποκατάστασης βάσης δεδομένων με χρήση παν-επαναφοράς](sql-database-disaster-recovery.md#recover-using-geo-restore). Ανάκτηση συνήθως τίθεται σε ισχύ εντός 12 ώρες - με απώλεια δεδομένων έως και μία ώρα καθορίζεται από πότε το τελευταίο ωριαία διακριτική δημιουργία αντιγράφων ασφαλείας με λαμβάνονται και από αναπαραγωγή. Μέχρι να ολοκληρωθεί η ανάκτηση, η βάση δεδομένων είναι δυνατό να απαντήσετε σε τυχόν ερωτήματα ή εγγραφή οποιαδήποτε συναλλαγές. 

> [AZURE.NOTE] Εάν το κέντρο δεδομένων αποκατασταθεί στο Internet πριν να κάνετε εναλλαγή πάνω από την εφαρμογή σας στη βάση δεδομένων που έχει ανακτηθεί, μπορείτε απλώς να ακυρώσετε την ανάκτηση.  

### <a name="perform-post-failover--recovery-tasks"></a>Εκτέλεση δημοσίευση ανακατεύθυνσης / εργασιών ανάκτησης 

Μετά την αποκατάσταση από κάποιο μηχανισμό ανάκτησης, πρέπει να εκτελέσετε τις ακόλουθες πρόσθετες εργασίες πριν από τους χρήστες σας και οι εφαρμογές είναι ξανά εγκατάσταση και λειτουργία:

- Redirect προγράμματα-πελάτες και σε εφαρμογές προγράμματος-πελάτη με το νέο διακομιστή και επαναφορά βάσης δεδομένων
- Βεβαιωθείτε ότι είναι κανόνες τείχους προστασίας κατάλληλο επίπεδο διακομιστή σε σημείο για τους χρήστες να συνδέσετε (ή χρησιμοποιήστε [τα τείχη προστασίας επιπέδου βάσης δεδομένων](sql-database-firewall-configure.md#creating-database-level-firewall-rules))
- Βεβαιωθείτε ότι κατάλληλες συνδέσεις και δικαιώματα επιπέδου κύρια βάση δεδομένων είναι σε θέση (ή χρησιμοποιήστε [που περιέχονται χρήστες](https://msdn.microsoft.com/library/ff929188.aspx))
- Ρύθμιση παραμέτρων ελέγχου, ανάλογα με την περίπτωση
- Ρύθμιση παραμέτρων ειδοποιήσεων, ανάλογα με την περίπτωση

## <a name="upgrade-an-application-with-minimal-downtime"></a>Αναβάθμιση μιας εφαρμογής με το ελάχιστο χρόνο εκτός λειτουργίας

Μερικές φορές μια εφαρμογή πρέπει να ληφθούν χωρίς σύνδεση λόγω προγραμματισμένης συντήρησης, όπως η αναβάθμιση εφαρμογών. [Διαχείριση εφαρμογών αναβαθμίσεις](sql-database-manage-application-rolling-upgrade.md) περιγράφει τον τρόπο χρήσης ενεργό παν-αναπαραγωγής για να ενεργοποιήσετε την συνάθροισης αναβαθμίσεις της εφαρμογής σας cloud για να ελαχιστοποιήσετε χρόνου εκτός λειτουργίας κατά τη διάρκεια αναβαθμίσεις και δώστε τη διαδρομή ανάκτησης σε περίπτωση που κάτι δεν πάει καλά. Σε αυτό το άρθρο εξετάζει δύο διαφορετικές μεθόδους orchestrating τη διαδικασία αναβάθμισης και περιγράφει τα πλεονεκτήματα και τα ανταλλάγματα κάθε επιλογής.

## <a name="next-steps"></a>Επόμενα βήματα

Για πληροφορίες σχετικά με θέματα σχεδίασης εφαρμογής για μεμονωμένο βάσεις δεδομένων και για χώρους συγκέντρωσης ελαστικά, ανατρέξτε στο θέμα [Σχεδίαση μια εφαρμογή για το cloud αποκατάσταση](sql-database-designing-cloud-solutions-for-disaster-recovery.md) και [στρατηγικές αποκατάστασης από καταστροφή ελαστικότητας χώρου συγκέντρωσης](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).






