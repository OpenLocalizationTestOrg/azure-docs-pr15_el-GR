<properties
   pageTitle="Αδιάκοπη παροχή υπηρεσιών στο cloud - επαναφορά διαγραμμένης βάσης δεδομένων - βάση δεδομένων SQL | Microsoft Azure"
   description="Μάθετε σχετικά με την επαναφορά σε δεδομένη χρονική στιγμή, που σας επιτρέπει να επαναφέρετε μια βάση δεδομένων SQL Azure σε ένα προηγούμενο σημείο στο χρονικό διάστημα (έως 35 ημέρες)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/01/2016"
   ms.author="sstein"/>

# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Ανάκτηση μιας βάσης δεδομένων Azure SQL με βάση δεδομένων της αυτόματης δημιουργίας αντιγράφων ασφαλείας

Βάση δεδομένων SQL παρέχει τρεις επιλογές για την ανάκτηση της βάσης δεδομένων με χρήση [της βάσης δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md). Μπορείτε να επαναφέρετε μια βάση δεδομένων από τα αντίγραφα ασφαλείας που ξεκινούν από την υπηρεσία στη διάρκεια της [περιόδου διατήρησης](sql-database-service-tiers.md) για να:

- Μια νέα βάση δεδομένων στον ίδιο διακομιστή λογική ανάκτησης σε ένα συγκεκριμένο σημείο σε ώρα εντός της περιόδου διατήρησης. 
- Μια βάση δεδομένων στον ίδιο διακομιστή λογική ανάκτησης στο χρόνο διαγραφής για μια διαγραμμένη βάση δεδομένων.
- Μια νέα βάση δεδομένων σε οποιονδήποτε λογική διακομιστή σε οποιαδήποτε περιοχή ανάκτησης για τα πιο πρόσφατα αντίγραφα ασφαλείας ημερήσια στο χώρο αποθήκευσης blob αναπαραχθούν παν (RA-Εξοπλισμό).

Μπορείτε επίσης να χρησιμοποιήσετε τη [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md) για να δημιουργήσετε μια [βάση δεδομένων αντιγραφή](sql-database-copy.md) σε οποιονδήποτε λογική διακομιστή σε κάθε περιοχή στην οποία έχει συμφωνεί με την τρέχουσα βάση δεδομένων SQL. Μπορείτε να χρησιμοποιήσετε αντίγραφο της βάσης δεδομένων και [Εξαγωγή σε ένα BACPAC](sql-database-export.md) για να αρχειοθετήσετε ένα ενημερώσετε συνεπή αντίγραφο της βάσης δεδομένων για μακροπρόθεσμη αποθήκευση πέρα από το περίοδος διατήρησης ή για να μεταφέρετε ένα αντίγραφο της βάσης δεδομένων σας σε μια εσωτερική ή Εικονική Azure παρουσία του SQL Server.

## <a name="recovery-time"></a>Χρόνος επαναφοράς

Τον χρόνο αποκατάστασης για να επαναφέρετε μια βάση δεδομένων με βάση δεδομένων της αυτόματης δημιουργίας αντιγράφων ασφαλείας επηρεάζεται από διάφορους παράγοντες: 
 - Το μέγεθος της βάσης δεδομένων
 - Το επίπεδο απόδοσης της βάσης δεδομένων
 - Τον αριθμό των αρχείων καταγραφής συναλλαγών που εμπλέκονται
 - Το ποσό των δραστηριοτήτων που πρέπει να γίνει αναπαραγωγή για να ανακτήσετε στο σημείο επαναφοράς
 - Το εύρος ζώνης δικτύου, εάν η επαναφορά είναι σε διαφορετική περιοχή 
 - Ο αριθμός των αιτήσεων ταυτόχρονες επαναφορά υποβάλλεται σε επεξεργασία στην περιοχή προορισμού. 
 
 Για μια βάση δεδομένων πολύ μεγάλο ή/και του ενεργού επαναφορά μπορεί να διαρκέσει μερικές ώρες. Εάν υπάρχει μακρά διακοπή ρεύματος σε μια περιοχή, είναι πιθανό ότι θα υπάρξει μεγάλου αριθμού των αιτήσεων παν-επαναφορά υποβάλλεται σε επεξεργασία από άλλες περιοχές. Εάν υπάρχουν πολλοί αιτήσεις αυτό μπορεί να αυξήσουν το χρόνο ανάκτησης για βάσεις δεδομένων σε αυτήν την περιοχή. Το μεγαλύτερο μέρος της βάσης δεδομένων επαναφέρει ολοκληρωθεί εντός 12 ώρες.

 Δεν υπάρχει καμία ενσωματωμένη λειτουργικότητα για τη μαζική επαναφορά. Το [βάση δεδομένων SQL Azure: πλήρη ανάκτηση διακομιστή](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) δέσμης ενεργειών είναι ένα παράδειγμα του ένας τρόπος για να κάνετε αυτήν την εργασία.

> [AZURE.IMPORTANT] Για να ανακτήσετε χρησιμοποιώντας την αυτόματη δημιουργία αντιγράφων ασφαλείας, πρέπει να είστε μέλος του ρόλου SQL Server συμβολής στην συνδρομή ή να είστε ο κάτοχος της συνδρομής. Μπορείτε να ανακτήσετε με την πύλη Azure, PowerShell ή το REST API. Δεν μπορείτε να χρησιμοποιήσετε Transact-SQL. 

## <a name="point-in-time-restore"></a>Επαναφορά σε δεδομένη χρονική στιγμή

Επαναφορά σε δεδομένη χρονική στιγμή σάς επιτρέπει να επαναφέρετε μια υπάρχουσα βάση δεδομένων ως μια νέα βάση δεδομένων σε μια προγενέστερη χρονική στιγμή στον ίδιο διακομιστή λογική με [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md). Δεν μπορείτε να αντικαταστήσετε την υπάρχουσα βάση δεδομένων. Μπορείτε να επαναφέρετε μια προγενέστερη χρονική στιγμή με χρήση του [Azure πύλη](sql-database-point-in-time-restore-portal.md), [PowerShell](sql-database-point-in-time-restore-powershell.md) ή το [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Επαναφορά σε δεδομένη χρονική στιγμή: Πύλη Azure](sql-database-point-in-time-restore-portal.md)
- [Επαναφορά σε δεδομένη χρονική στιγμή: PowerShell](sql-database-point-in-time-restore-powershell.md)

Η βάση δεδομένων μπορούν να αποκατασταθούν σε κάθε επίπεδο επιδόσεων ή ελαστικές χώρου συγκέντρωσης. Πρέπει να βεβαιωθείτε ότι έχετε αρκετό ποσόστωσης DTU στο διακομιστή λογική ή ελαστικά χώρου συγκέντρωσης. Λάβετε υπόψη ότι η επαναφορά δημιουργεί μια νέα βάση δεδομένων και ότι το επίπεδο επίπεδο και επιδόσεων υπηρεσίας της Επαναφορά βάσης δεδομένων μπορεί να είναι διαφορετική από την τρέχουσα κατάσταση της ζωντανή βάσης δεδομένων. Μόλις ολοκληρωθεί η λήψη, την Επαναφορά βάσης δεδομένων είναι μια κανονική πλήρως προσβάσιμα online βάση δεδομένων χρεωθεί κανονική επιτόκια με βάση το επίπεδο επίπεδο και επιδόσεων υπηρεσίας. Που δεν χρεωθείτε μέχρι να ολοκληρωθεί η Επαναφορά βάσης δεδομένων.

Συνήθως μπορείτε να επαναφέρετε μια βάση δεδομένων σε ένα σημείο earler για λόγους αποκατάστασης. Όταν αυτή την περίπτωση, μπορείτε να χειριστείτε την Επαναφορά βάσης δεδομένων ως αντικαθιστά την αρχική βάση δεδομένων ή να το χρησιμοποιήσετε για να ανακτήσετε δεδομένα από το και, στη συνέχεια, ενημερώστε την αρχική βάση δεδομένων. 

- ***Αντικατάστασης της βάσης δεδομένων:*** Εάν η βάση δεδομένων επαναφέρει προορίζεται ως αντικαθιστά την αρχική βάση δεδομένων, θα πρέπει να επαληθεύσετε το επίπεδο απόδοσης ή/και επίπεδο υπηρεσιών είναι κατάλληλα και κλιμάκωση της βάσης δεδομένων, εάν είναι απαραίτητο. Για να μετονομάσετε την αρχική βάση δεδομένων και, στη συνέχεια, δώστε την Επαναφορά βάσης δεδομένων το αρχικό όνομα, χρησιμοποιώντας την εντολή ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ στο T-SQL. 
- ***Ανάκτησης δεδομένων:*** Εάν σκοπεύετε να ανακτήσετε δεδομένα από το Επαναφορά βάσης δεδομένων για την ανάκτηση από ένα χρήστη ή εφαρμογή σφάλμα, ξεχωριστά θα πρέπει να εγγραφής και εκτέλεσης όποιο δέσμες ενεργειών ανάκτησης δεδομένων που χρειάζεστε για να εξαγάγετε δεδομένα από τη βάση δεδομένων επαναφέρει στην αρχική βάση δεδομένων. Παρόλο που η λειτουργία επαναφοράς ενδέχεται να χρειαστούν μεγάλο χρονικό διάστημα για να ολοκληρώσετε, θα είναι ορατός στη λίστα βάσεων δεδομένων σε όλο το Επαναφορά βάσης δεδομένων. Εάν διαγράψετε κατά την επαναφορά της βάσης δεδομένων, αυτό θα ακυρώσετε τη λειτουργία και δεν θα χρεωθείτε για τη βάση δεδομένων που δεν ολοκληρώθηκε η επαναφορά. 

Για λεπτομερείς πληροφορίες σχετικά με τη χρήση επαναφορά σε δεδομένη χρονική στιγμή για να ανακτήσετε από το χρήστη και εφαρμογή σφαλμάτων, ανατρέξτε στο θέμα επαναφορά [στη δεδομένη χρονική στιγμή](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="deleted-database-restore"></a>Επαναφορά διαγραμμένων βάσης δεδομένων

Επαναφορά διαγραμμένων βάσης δεδομένων σάς επιτρέπει να επαναφέρετε μια διαγραμμένη βάση δεδομένων για να την ώρα διαγραφής για μια διαγραμμένη βάση δεδομένων στον ίδιο διακομιστή λογική με [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md). 

> [AZURE.IMPORTANT] Εάν διαγράψετε μια παρουσία διακομιστή βάσης δεδομένων SQL Azure, όλες τις βάσεις δεδομένων, θα διαγραφούν και είναι δυνατή η ανάκτησή. Δεν υπάρχει υποστήριξη για την επαναφορά διαγραμμένων διακομιστή αυτήν τη στιγμή.

Μπορείτε να χρησιμοποιήσετε το ίδιο ή σε ένα νέο όνομα βάσης δεδομένων για την Επαναφορά βάσης δεδομένων. Μπορείτε να χρησιμοποιήσετε την [πύλη του Azure](sql-database-restore-deleted-database-portal.md), [PowerShell](sql-database-restore-deleted-database-powershell.md) ή το [ΥΠΌΛΟΙΠΑ (createMode = επαναφορά)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [AZURE.SELECTOR]
- [Διαγραφή Επαναφορά βάσης δεδομένων: πύλη του Azure](sql-database-restore-deleted-database-portal.md)
- [Διαγραφή Επαναφορά βάσης δεδομένων: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="geo-restore"></a>Επαναφορά παν

Επαναφορά παν σάς επιτρέπει να επαναφέρετε μια βάση δεδομένων SQL σε οποιονδήποτε διακομιστή σε οποιαδήποτε Azure περιοχή από την πιο πρόσφατη αναπαραχθούν παν [αυτοματοποιημένη Ημερήσια δημιουργία αντιγράφων ασφαλείας](sql-database-automated-backups.md). Επαναφορά παν χρησιμοποιεί παν πλεονάζοντα αντίγραφο ασφαλείας ως προέλευση και μπορούν να χρησιμοποιηθούν για να ανακτήσετε μια βάση δεδομένων, ακόμα και αν είναι δυνατή η πρόσβαση εξαιτίας ενός μη διαθεσιμότητα της βάσης δεδομένων ή ένα κέντρο δεδομένων. Μπορείτε να χρησιμοποιήσετε την [πύλη του Azure](sql-database-geo-restore-portal.md), [PowerShell](sql-database-geo-restore-powershell.md), ή το [ΥΠΌΛΟΙΠΑ (createMode = αποκατάστασης)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 

> [AZURE.SELECTOR]
- [Παν-επαναφορά: Πύλη Azure](sql-database-geo-restore-portal.md)
- [Επαναφορά παν: PowerShell](sql-database-geo-restore-powershell.md)

Επαναφορά παν είναι η προεπιλεγμένη επιλογή αποκατάστασης κατά τη βάση δεδομένων είναι διαθέσιμος, λόγω ενός περιστατικού στην περιοχή όπου φιλοξενείται τη βάση δεδομένων. Εάν ένα περιστατικό της μεγάλης κλίμακας σε μια περιοχή καταλήξει σε μη διαθεσιμότητα της εφαρμογής σας βάση δεδομένων, μπορείτε να χρησιμοποιήσετε παν-Επαναφορά για να επαναφέρετε μια βάση δεδομένων από το πιο πρόσφατο αντίγραφο ασφαλείας σε ένα διακομιστή σε οποιαδήποτε άλλη περιοχή. Όλα τα αντίγραφα ασφαλείας είναι αναπαραχθούν παν και μπορεί να έχει μια καθυστέρηση μεταξύ όταν το αντίγραφο ασφαλείας είναι λαμβάνονται και παν αναπαραχθούν για το Azure blob σε διαφορετική περιοχή. Αυτή η καθυστέρηση μπορεί να είναι έως και μία ώρα, ώστε να στην περίπτωση μιας καταστροφής μπορεί να υπάρχει προς τα επάνω σε 1 ώρα απώλεια δεδομένων, π.χ., RPO έως 1 ώρα. Η ακόλουθη εικόνα δείχνει επαναφορά της βάσης δεδομένων από την τελευταία Ημερήσια δημιουργία αντιγράφων ασφαλείας.

![Επαναφορά παν](./media/sql-database-geo-restore/geo-restore-2.png)

Για λεπτομερείς πληροφορίες σχετικά με τη χρήση παν-επαναφοράς προς ανάκτηση από μια μη διαθεσιμότητα, ανατρέξτε στο θέμα [ανάκτηση από μια μη διαθεσιμότητα](sql-database-disaster-recovery.md)

> [AZURE.IMPORTANT] Ενώ το παν-επαναφοράς είναι διαθέσιμο με όλα τα επίπεδα υπηρεσίας, είναι πιο βασικές των λύσεων αποκατάστασης από καταστροφή διαθέσιμο σε βάσεις δεδομένων SQL με την μεγαλύτερου RPO και την εκτίμηση αποκατάστασης χρόνου (εισαγωγή). Για βασικές βάσεις δεδομένων με το μέγιστο μέγεθος των 2 GB παν-επαναφορά παρέχει μια λογικό λύση DR με μια εισαγωγή 12 ώρες. Για μεγαλύτερη τυπική ή Premium για βάσεις δεδομένων, εάν σημαντικά μειώνει το χρόνο ανάκτησης είναι επιθυμητοί ή να μειώσετε την πιθανότητα απώλειας δεδομένων που θα πρέπει να μπορείτε να χρησιμοποιήσετε ενεργό παν-αναπαραγωγής. Ενεργό παν-αναπαραγωγής προσφέρει μια πολύ χαμηλότερη RPO και εισαγωγή καθώς απαιτεί μόνο προετοιμασία ανακατεύθυνσης για δευτερεύον συνεχώς από αναπαραγωγή. Για λεπτομέρειες, ανατρέξτε στο θέμα [Ενεργό παν-αναπαραγωγής](sql-database-geo-replication-overview.md).

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Μέσω προγραμματισμού εκτέλεση αποκατάστασης χρησιμοποιώντας την αυτόματη δημιουργία αντιγράφων ασφαλείας

Όπως περιγράφεται παραπάνω, στο addiition στην πύλη του Azure, ανάκτηση της βάσης δεδομένων μπορεί να πραγματοποιηθεί programmically με χρήση του Azure PowerShell και το REST API. Οι παρακάτω πίνακες περιγράφουν το σύνολο των διαθέσιμων εντολών.

### <a name="powershell"></a>PowerShell

|Cmdlet|Περιγραφή|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Λαμβάνει μία ή περισσότερες βάσεις δεδομένων.|
|[Get-AzureRMSqlDeletedDatabaseBackup](https://msdn.microsoft.com/en-us/library/azure/mt693387.aspx)|Λαμβάνει μια διαγραμμένη βάση δεδομένων που μπορείτε να τον επαναφέρετε.|
|[Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx)|Λαμβάνει παν πλεονάζοντα αντίγραφο ασφαλείας της βάσης δεδομένων.|
|[Επαναφορά AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx)|Επαναφέρει μια βάση δεδομένων SQL.|
||||

### <a name="rest-api"></a>REST API

|API|Περιγραφή|
|---|-----------|
|[ΥΠΌΛΟΙΠΟ (createMode = αποκατάστασης)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Επαναφέρει μια βάση δεδομένων|
|[Λήψη, δημιουργία ή ενημέρωση κατάσταση της βάσης δεδομένων](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Επιστρέφει την κατάσταση κατά τη διάρκεια της λειτουργίας επαναφοράς|
||||



## <a name="summary"></a>Σύνοψη

Αυτόματη δημιουργία αντιγράφων ασφαλείας προστασία τις βάσεις δεδομένων από το χρήστη και σφάλματα εφαρμογών, διαγραφή τυχαίες βάσης δεδομένων και μακρά διακοπές. Αυτή η λύση μηδέν κόστος μηδέν-διαχείρισης είναι διαθέσιμη με όλες τις βάσεις δεδομένων SQL. 

## <a name="next-steps"></a>Επόμενα βήματα

- Για μια επισκόπηση επιχειρηματικές συνέχειας και σενάρια, ανατρέξτε στο θέμα [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)
- Για να μάθετε σχετικά με τη βάση δεδομένων SQL Azure αυτόματης δημιουργίας αντιγράφων ασφαλείας, ανατρέξτε στο θέμα [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md)
- Για να μάθετε σχετικά με τις επιλογές αποκατάστασης του ταχύτερος, ανατρέξτε στο θέμα [Ενεργό-παν-αναπαραγωγής](sql-database-geo-replication-overview.md)  
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την αρχειοθέτηση, ανατρέξτε στο θέμα [Αντιγραφή βάσης δεδομένων](sql-database-copy.md)
