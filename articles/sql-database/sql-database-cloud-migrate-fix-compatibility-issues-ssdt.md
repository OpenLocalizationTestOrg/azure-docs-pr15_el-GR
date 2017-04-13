<properties
   pageTitle="Επιδιόρθωση ζητημάτων συμβατότητας βάσης δεδομένων SQL Server πριν από την μετεγκατάσταση σε βάση δεδομένων SQL | Microsoft Azure"
   description="Azure βάση δεδομένων Microsoft SQL, μετεγκατάσταση της βάσης δεδομένων, συμβατότητα, Οδηγός μετεγκατάστασης Azure SQL, SSDT"
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
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="migrate-a-sql-server-database-to-azure-sql-database-using-sql-server-data-tools-for-visual-studio"></a>Μετεγκατάσταση μιας βάσης δεδομένων SQL Server σε βάση δεδομένων SQL Azure χρησιμοποιώντας εργαλεία δεδομένων του SQL Server για το Visual Studio 

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Σύμβουλος αναβάθμισης](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

Σε αυτό το άρθρο, θα μάθετε για να εντοπίσετε και να διορθώσετε προβλήματα συμβατότητας βάσης δεδομένων SQL Server με χρήση του SQL Server Data Tools για το Visual Studio, πριν από την μετεγκατάσταση σε βάση δεδομένων SQL Azure.

## <a name="using-sql-server-data-tools-for-visual-studio"></a>Χρήση του SQL Server Data Tools για το Visual Studio

Χρήση του SQL Server Data Tools για το Visual Studio ("SSDT") για να εισαγάγετε το σχήμα της βάσης δεδομένων σε ένα έργο Visual Studio βάσης δεδομένων για ανάλυση. Για να αναλύσετε, καθορίστε την πλατφόρμα προορισμού για το έργο ως V12 βάσης δεδομένων SQL και, στη συνέχεια, δημιουργήστε το έργο. Εάν η Δόμηση είναι επιτυχής, η βάση δεδομένων είναι συμβατή. Εάν η δημιουργία αποτυγχάνει, μπορείτε να επιλύσετε τα σφάλματα στο SSDT (ή σε ένα από τα άλλα εργαλεία που περιγράφονται σε αυτό το θέμα). Όταν το project δημιουργεί με επιτυχία, μπορείτε να το δημοσιεύσετε ξανά ως αντίγραφο της βάσης δεδομένων προέλευσης. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε τη δυνατότητα σύγκριση δεδομένων στο SSDT για να αντιγράψετε τα δεδομένα από τη βάση δεδομένων προέλευσης για τη βάση δεδομένων συμβατή Azure SQL V12. Στη συνέχεια, μπορείτε να μετεγκαταστήσετε αυτής της ενημερωμένης βάσης δεδομένων. Για να χρησιμοποιήσετε αυτήν την επιλογή, κάντε λήψη του την [πιο πρόσφατη έκδοση του SSDT](https://msdn.microsoft.com/library/mt204009.aspx).

  ![Διάγραμμα VSSSDT μετεγκατάστασης](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] Εάν απαιτείται μόνο για σχήμα μετεγκατάστασης, το σχήμα είναι δυνατό να δημοσιευτεί απευθείας από το Visual Studio απευθείας σε βάση δεδομένων SQL Azure. Χρησιμοποιήστε αυτήν τη μέθοδο όταν το σχήμα της βάσης δεδομένων απαιτεί περισσότερες αλλαγές από αντιμετώπισης από τον Οδηγό μετεγκατάστασης από μόνο του.

## <a name="detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Εντοπισμός ζητήματα συμβατότητας με εργαλεία δεδομένων του SQL Server για το Visual Studio
   
1.  Ανοίξτε την **Εξερεύνηση αντικειμένου SQL Server** στο Visual Studio. Χρησιμοποιήστε **Προσθήκη SQL Server** για να συνδεθείτε με την παρουσία του SQL Server που περιέχει τη βάση δεδομένων θα μετεγκατασταθούν. Εντοπίστε τη βάση δεδομένων στην Εξερεύνηση των αντικειμένων, κάντε δεξί κλικ στη βάση δεδομένων και επιλέξτε **Δημιουργία νέου έργου...**     
    
    ![Νέο έργο](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)    
   
2.  Ρυθμίστε τις παραμέτρους των ρυθμίσεων εισαγωγής για να **εισαγάγετε εμβέλειας εφαρμογής αντικείμενα μόνο**. Καταργήστε την επιλογή από τις επιλογές για να εισαγάγετε τα εξής: στα οποία γίνεται αναφορά συνδέσεις, δικαιώματα και ρυθμίσεις της βάσης δεδομένων.    

    ![εναλλακτικό κείμενο](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)    

3.  Κάντε κλικ στην επιλογή **Έναρξη** για να εισαγάγετε τη βάση δεδομένων και να δημιουργήσετε το έργο που περιέχει ένα αρχείο δέσμης ενεργειών T-SQL για κάθε αντικείμενο στη βάση δεδομένων. Τα αρχεία δέσμης ενεργειών είναι ένθετες σε φακέλους μέσα στο έργο.    

    ![εναλλακτικό κείμενο](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)    

4.  Στο του Visual Studio Εξερεύνηση λύσεων, κάντε δεξί κλικ στο έργο βάσης δεδομένων και επιλέξτε Ιδιότητες. Στη σελίδα **Ρυθμίσεις του Project** , ρυθμίστε τις παραμέτρους της πλατφόρμας προορισμού για να V12 βάση δεδομένων του Microsoft Azure SQL.    
    
    ![εναλλακτικό κείμενο](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)    
    
5.  Κάντε δεξί κλικ στο έργο και επιλέξτε **Δημιουργία** για να δημιουργήσετε το έργο.    
    
    ![εναλλακτικό κείμενο](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)    
    
6.  Η **Λίστα σφαλμάτων** εμφανίζει κάθε ασυμβατότητα. Σε αυτήν την περίπτωση, το όνομα χρήστη NT AUTHORITY\NETWORK SERVICE είναι συμβατή. Επειδή δεν είναι συμβατή, μπορείτε να σχόλιο την προβολή ή να καταργήσετε (και διεύθυνση τις επιπτώσεις της κατάργησης αυτό σύνδεσης και το ρόλο από τη λύση βάσης δεδομένων).     
    
    ![εναλλακτικό κείμενο](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)    
    
## <a name="fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Διόρθωση ζητημάτων συμβατότητας με εργαλεία δεδομένων του SQL Server για το Visual Studio

1.  Κάντε διπλό κλικ την πρώτη δέσμη ενεργειών για να ανοίξετε τη δέσμη ενεργειών σε ένα παράθυρο ερωτήματος και σχολιάζετε τη δέσμη ενεργειών και, στη συνέχεια, εκτελέστε τη δέσμη ενεργειών.     
    ![εναλλακτικό κείμενο](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

2.  Επαναλάβετε αυτήν τη διαδικασία για κάθε δέσμη ενεργειών που περιέχει ασυμβατότητα μέχρι να παραμείνουν κανένα σφάλμα.    
    ![εναλλακτικό κείμενο](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
    
3.  Όταν η βάση δεδομένων δεν περιέχει σφάλματα, κάντε δεξί κλικ στο έργο και επιλέξτε **Δημοσίευση**. Ένα αντίγραφο της βάσης δεδομένων προέλευσης είναι ενσωματωμένη και να δημοσιευτεί (συνιστάται ιδιαίτερα να χρησιμοποιήσετε ένα αντίγραφο, τουλάχιστον αρχικά).     
 - Πριν να δημοσιεύσετε, ανάλογα με την προέλευση SQL Server έκδοση (παλαιότερη από το SQL Server 2014), ίσως χρειαστεί να τον επαναφέρετε πλατφόρμα προορισμού του έργου για να ενεργοποιήσετε την ανάπτυξη.     
 - Εάν κάνετε μετεγκατάσταση μια παλαιότερη βάση δεδομένων SQL Server, δεν προκαλέσει όλες τις δυνατότητες στο έργο που δεν υποστηρίζονται στο αρχείο προέλευσης SQL Server μέχρι την μετεγκατάσταση βάσης δεδομένων σε νεότερη έκδοση του SQL Server.     

        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
    
        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
        
4.  Στην Εξερεύνηση αντικειμένου SQL Server, κάντε δεξί κλικ τη βάση δεδομένων προέλευσης και κάντε κλικ στην επιλογή **Σύγκριση δεδομένων**. Σύγκριση του έργου στην αρχική βάση δεδομένων σας βοηθά να κατανοήσετε τις αλλαγές που έχουν γίνει από τον οδηγό. Επιλέξτε την έκδοση Azure SQL V12 της βάσης δεδομένων και, στη συνέχεια, κάντε κλικ στο κουμπί **Τέλος**.    
    
    ![εναλλακτικό κείμενο](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)    
    
    ![εναλλακτικό κείμενο](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)    

5.  Εξετάστε τις διαφορές που εντοπίζονται και, στη συνέχεια, κάντε κλικ στην επιλογή **Ενημέρωση προορισμού** για τη μετεγκατάσταση δεδομένων από τη βάση δεδομένων προέλευσης στη βάση δεδομένων Azure SQL V12.     
    
    ![εναλλακτικό κείμενο](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)    
    
6.  Επιλέξτε μια μέθοδο ανάπτυξης. Ανατρέξτε στο θέμα [μετεγκατάσταση συμβατά βάσης δεδομένων SQL Server με βάση δεδομένων SQL.](sql-database-cloud-migrate.md)  

## <a name="next-steps"></a>Επόμενα βήματα

- [Νεότερη έκδοση του SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Νεότερη έκδοση του SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [V12 βάση δεδομένων SQL](sql-database-v12-whats-new.md)
- [Transact-SQL μερικώς ή δεν υποστηρίζεται συναρτήσεις](sql-database-transact-sql-information.md)
- [Μετεγκατάσταση βάσεων δεδομένων SQL Server με χρήση Βοηθό μετεγκατάστασης του SQL Server](http://blogs.msdn.com/b/ssma/)
