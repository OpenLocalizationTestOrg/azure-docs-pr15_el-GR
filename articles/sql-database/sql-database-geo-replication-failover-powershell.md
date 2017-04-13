<properties 
    pageTitle="Προετοιμασία προγραμματισμένες ή μη προγραμματισμένες ανακατεύθυνσης για βάση δεδομένων SQL Azure με PowerShell | Microsoft Azure" 
    description="Προετοιμασία προγραμματισμένες ή μη προγραμματισμένες ανακατεύθυνσης για βάση δεδομένων SQL Azure με χρήση του PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-powershell"></a>Προετοιμασία προγραμματισμένες ή μη προγραμματισμένες ανακατεύθυνσης για βάση δεδομένων SQL Azure με το PowerShell



> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


Σε αυτό το άρθρο σάς δείχνει πώς να ξεκινήσετε μια προγραμματισμένη ή μη προγραμματισμένη ανακατεύθυνσης για βάση δεδομένων SQL με το PowerShell. Για να ρυθμίσετε τις παραμέτρους παν αναπαραγωγής, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων παν-αναπαραγωγή για βάση δεδομένων SQL Azure](sql-database-geo-replication-powershell.md).



## <a name="initiate-a-planned-failover"></a>Προετοιμασία προγραμματισμένη ανακατεύθυνσης

Χρησιμοποιήστε το cmdlet **Set-AzureRmSqlDatabaseSecondary** με την παράμετρο **- Ανακατεύθυνσης** για να προβιβάσετε μια δευτερεύουσα βάση δεδομένων για να γίνετε τη νέα κύρια βάση δεδομένων, το υπάρχον πρωτεύον για να γίνετε δευτερεύον υποβιβασμός. Αυτή η λειτουργία έχει σχεδιαστεί για μια προγραμματισμένη ανακατεύθυνση, όπως κατά τη διάρκεια drills αποκατάστασης από καταστροφή, και απαιτεί να είναι διαθέσιμη η κύρια βάση δεδομένων.

Η εντολή εκτελεί τη ροή εργασίας παρακάτω:

1. Προσωρινή μετάβαση αναπαραγωγής σύγχρονη λειτουργία. Αυτό θα προκαλέσει όλες τις εκκρεμείς συναλλαγές να καταγραφούν στο δευτερεύον.

2. Μεταβείτε τους ρόλους των δύο βάσεων δεδομένων με τη συνεργασία παν αναπαραγωγής.  

Αυτή η ακολουθία εγγυάται ότι οι δύο βάσεις δεδομένων είναι συγχρονισμένα πριν από την αλλαγή τους ρόλους και, επομένως, θα παρουσιαστεί χωρίς απώλεια δεδομένων. Υπάρχει μια σύντομη περίοδο κατά την οποία δύο βάσεις δεδομένων δεν είναι διαθέσιμες (στη διάταξη 0 έως 25 δευτερόλεπτα) ενώ οι ρόλοι έχουν αλλάξει. Ολόκληρη η λειτουργία πρέπει να εκτελεί λιγότερο από ένα λεπτό για να ολοκληρωθεί σε κανονικές συνθήκες. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ορισμός AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx).




Αυτό το cmdlet θα επιστρέψει όταν ολοκληρωθεί η διαδικασία της αλλαγής της βάσης δεδομένων δευτερεύοντα πρωτεύοντος.

Την ακόλουθη εντολή: αλλάζει τους ρόλους της βάσης δεδομένων με το όνομα "περίπτωση" στο διακομιστή "srv2" κάτω από την ομάδα πόρων "rg2" για να πρωτεύοντος. Η αρχική κύρια στο οποίο "db2" έχει συνδεθεί με θα μεταβεί σε δευτερεύοντα μετά τις δύο βάσεις δεδομένων είναι συγχρονισμένα.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] Στην περίπτωση αυτή είναι πιθανό ότι η λειτουργία δεν μπορεί να ολοκληρωθεί και μπορεί να φαίνεται ότι δεν ανταποκρίνεται. Σε αυτήν την περίπτωση ο χρήστης μπορεί να καλέσετε την εντολή ανακατεύθυνσης ισχύ (μη προγραμματισμένη ανακατεύθυνσης) και αποδοχή απώλεια δεδομένων.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Ξεκινήσετε μια μη προγραμματισμένη ανακατεύθυνσης από την κύρια βάση δεδομένων στη βάση δεδομένων του δευτερεύοντος


Μπορείτε να χρησιμοποιήσετε το cmdlet **Set-AzureRmSqlDatabaseSecondary** με παραμέτρους **Ανακατεύθυνσης-** και **- AllowDataLoss** για να προβιβάσετε μια δευτερεύουσα βάση δεδομένων για να γίνετε τη νέα κύρια βάση δεδομένων με μια μη προγραμματισμένη τον τρόπο, να επιβάλετε ο υποβιβασμός του το υπάρχον πρωτεύον για να γίνετε δευτερεύον κάθε φορά κατά την κύρια βάση δεδομένων δεν είναι πλέον διαθέσιμη.

Αυτή η λειτουργία έχει σχεδιαστεί για αποκατάσταση όταν επαναφορά διαθεσιμότητα της βάσης δεδομένων είναι κρίσιμης και ορισμένες απώλεια δεδομένων είναι αποδεκτή. Όταν ενεργοποιείται εξαναγκασμένη ανακατεύθυνσης, η καθορισμένη βάση δεδομένων δευτερεύοντα γίνεται την κύρια βάση δεδομένων αμέσως και αρχίζει αποδοχή συναλλαγές εγγραφής. Μόλις η αρχική κύρια βάση δεδομένων είναι σε θέση να το συνδέσετε με αυτήν τη νέα κύρια βάση μετά τη λειτουργία εξαναγκασμένη ανακατεύθυνσης, μια προσαύξησης λαμβάνεται αρχική κύρια βάση δεδομένων και την παλιά κύρια βάση δεδομένων είναι που έχουν γίνει σε μια δευτερεύουσα βάση δεδομένων για τη νέα κύρια βάση δεδομένων. στη συνέχεια, είναι απλώς μια ρεπλίκα της νέας κύριας.

Αλλά επειδή το σημείο στο χρόνο επαναφορά δεν υποστηρίζεται στη δευτερεύουσα βάσεις δεδομένων, εάν θέλετε να ανάκτησης δεδομένων δεσμευμένου στην παλιά κύρια βάση δεδομένων που είχαν δεν έχει αντιγραφεί στη νέα κύρια βάση δεδομένων, θα πρέπει να απευθυνθείτε CSS για να επαναφέρετε μια βάση δεδομένων για το αντίγραφο ασφαλείας γνωστά καταγραφής.

> [AZURE.NOTE] Εάν η εντολή εκδίδεται όταν κύριων και δευτερευουσών που online θα γίνει η παλιά κύρια τη νέα δευτερεύουσα αμέσως χωρίς συγχρονισμό δεδομένων. Εάν είναι η κύρια μπορεί να προκύψουν ολοκλήρωση συναλλαγές όταν η εντολή εκδίδεται απώλεια ορισμένων δεδομένων.


Εάν η κύρια βάση δεδομένων έχει πολλούς secondaries την εντολή θα πραγματοποιηθεί μερικώς με επιτυχία. Θα μετατραπεί σε κύρια τη δευτερεύουσα στον οποίο εκτελέστηκε η εντολή. Η παλιά κύρια ωστόσο θα παραμείνει κύρια, δηλαδή τα δύο βασικά χρώματα θα καταλήγουν στο ασυνέπεια και είστε συνδεδεμένοι με μια σύνδεση σε αναστολή αναπαραγωγής. Ο χρήστης θα χρειαστεί να επιδιορθώσετε με μη αυτόματο τρόπο χρησιμοποιώντας ένα API "Κατάργηση δευτερεύοντα" σε οποιαδήποτε από αυτές τις βάσεις δεδομένων πρωτεύοντος αυτήν τη ρύθμιση παραμέτρων.


Την ακόλουθη εντολή: αλλάζει τους ρόλους της βάσης δεδομένων με το όνομα "περίπτωση" για να πρωτεύοντος όταν η κύρια δεν είναι διαθέσιμη. Η αρχική κύρια στο οποίο "περίπτωση" έχει συνδεθεί με θα μεταβεί σε δευτερεύον αφού είναι πάλι σε σύνδεση. Σε αυτό το σημείο, ο συγχρονισμός μπορεί να οδηγήσουν σε απώλεια δεδομένων.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## <a name="next-steps"></a>Επόμενα βήματα   

- Μετά την ανακατεύθυνση, βεβαιωθείτε ότι έχουν ρυθμιστεί οι απαιτήσεις ελέγχου ταυτότητας για το διακομιστή και τη βάση δεδομένων στην νέα κύρια. Για λεπτομέρειες, ανατρέξτε στο θέμα [ασφαλείας βάσης δεδομένων SQL μετά την αποκατάσταση](sql-database-geo-replication-security-config.md).
- Για να μάθετε ανάκτηση μετά από μια καταστροφή χρησιμοποιώντας ενεργό παν-αναπαραγωγή, συμπεριλαμβανομένων των pre και δημοσιεύσετε αποκατάστασης βήματα και να εκτελέσετε εμφάνιση λεπτομερειών αποκατάστασης από καταστροφή, ανατρέξτε στο θέμα [Drills αποκατάστασης από καταστροφή](sql-database-disaster-recovery.md)
- Για Sasha Nosov δημοσίευση ιστολογίου σχετικά με το ενεργό παν-αναπαραγωγής, ανατρέξτε στο θέμα [Ανάδειξη σε νέες δυνατότητες παν αναπαραγωγής](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Για πληροφορίες σχετικά με τη σχεδίαση εφαρμογών cloud για να χρησιμοποιήσετε ενεργό παν-αναπαραγωγής, ανατρέξτε στο θέμα [Σχεδιασμός cloud εφαρμογών για αδιάκοπη χρησιμοποιώντας παν αναπαραγωγής](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Για πληροφορίες σχετικά με τη χρήση ενεργό παν-αλληλεπίδραση με τους χώρους συγκέντρωσης ελαστικότητας βάσης δεδομένων, ανατρέξτε στο θέμα [στρατηγικές αποκατάστασης από καταστροφή ελαστικότητας χώρου συγκέντρωσης](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Για μια επισκόπηση του continurity επιχειρήσεις, ανατρέξτε στο θέμα [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)