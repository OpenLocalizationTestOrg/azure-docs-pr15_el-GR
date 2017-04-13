<properties
    pageTitle="Διαχείριση της βάσης δεδομένων SQL Azure με την πύλη Azure | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιήσουν την πύλη του Azure για τη Διαχείριση σχεσιακή βάση δεδομένων στο cloud με την πύλη Azure."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.date="09/19/2016"
    ms.author="sstein"/>


# <a name="managing-azure-sql-databases-using-the-azure-portal"></a>Διαχείριση βάσεων δεδομένων SQL Azure με την πύλη Azure


> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

Στην [πύλη του Azure](https://portal.azure.com/) σάς επιτρέπει να δημιουργήσετε, παρακολούθηση και διαχείριση βάσεων δεδομένων Azure SQL και των διακομιστών. Σε αυτό το άρθρο παρέχει μια γρήγορη περιγραφή και συνδέσεις για τις λεπτομέρειες από τις πιο συνηθισμένες εργασίες.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Προβάλετε τις βάσεις δεδομένων Azure SQL, διακομιστές και σύνολα

Για να προβάλετε τις διαθέσιμες υπηρεσίες βάσης δεδομένων SQL, κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες**και πληκτρολογήστε **SQL** στο πλαίσιο αναζήτησης:

![Βάση δεδομένων SQL](./media/sql-database-manage-portal/sql-services.png)


## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Πώς δημιουργώ ή προβολή βάσεις δεδομένων Azure SQL;

Για να ανοίξετε το blade **βάσεις δεδομένων SQL** , κάντε κλικ στην επιλογή **βάσεις δεδομένων SQL**, και, στη συνέχεια, κάντε κλικ στην επιλογή της βάσης δεδομένων που θέλετε να εργαστείτε με ή κάντε κλικ στο κουμπί **+ Προσθήκη** για να δημιουργήσετε μια βάση δεδομένων SQL. Για λεπτομέρειες, ανατρέξτε στο θέμα [Δημιουργία μιας βάσης δεδομένων SQL σε λεπτά, χρησιμοποιώντας την πύλη του Azure](sql-database-get-started.md).


![Βάσεις δεδομένων SQL](./media/sql-database-manage-portal/sql-databases.png)


## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Πώς δημιουργώ ή προβολή Azure SQL διακομιστές;

Για να ανοίξετε το blade **διακομιστών SQL Server** , κάντε κλικ στην επιλογή **διακομιστές SQL**, και, στη συνέχεια, κάντε κλικ στην επιλογή που θέλετε να εργαστείτε με το διακομιστή ή κάντε κλικ στο κουμπί **+ Προσθήκη** για να δημιουργήσετε SQL server. Για λεπτομέρειες, ανατρέξτε στο θέμα [Δημιουργία μιας βάσης δεδομένων SQL σε λεπτά, χρησιμοποιώντας την πύλη του Azure](sql-database-get-started.md).

![Οι διακομιστές SQL](./media/sql-database-manage-portal/sql-servers.png)


## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Πώς δημιουργώ ή προβολή SQL ελαστικότητας χώρους συγκέντρωσης;

Για να ανοίξετε το blade **ελαστικότητας χώρους συγκέντρωσης SQL** , κάντε κλικ στην επιλογή **SQL ελαστικότητας χώρους συγκέντρωσης**, και, στη συνέχεια, κάντε κλικ στην επιλογή το χώρο συγκέντρωσης που θέλετε να εργαστείτε με ή κάντε κλικ στο κουμπί **+ Προσθήκη** για να δημιουργήσετε ένα χώρο συγκέντρωσης. Για λεπτομέρειες, ανατρέξτε στο θέμα [Δημιουργία ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων με την πύλη του Azure](sql-database-elastic-pool-create-portal.md).

![Ελαστικά χώρους συγκέντρωσης SQL](./media/sql-database-manage-portal/elastic-pools.png)



## <a name="how-do-i-update-or-view-sql-database-settings"></a>Πώς ενημέρωση ή προβολή ρυθμίσεων βάσης δεδομένων SQL;

Για να προβάλετε ή να ενημερώσετε τις ρυθμίσεις της βάσης δεδομένων, κάντε κλικ στην επιθυμητή ρύθμιση σε το blade βάσης δεδομένων SQL:


![Ρυθμίσεις της βάσης δεδομένων SQL](./media/sql-database-manage-portal/settings.png)


## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Πώς μπορώ να βρω ένα όνομα διακομιστή πλήρως προσδιορισμένη βάσεις δεδομένων SQL;

Για να προβάλετε το όνομα του διακομιστή βάσεων δεδομένων, κάντε κλικ στην εντολή **Επισκόπηση** blade τη **βάση δεδομένων SQL** και σημειώστε το όνομα του διακομιστή:


![Ρυθμίσεις της βάσης δεδομένων SQL](./media/sql-database-manage-portal/server-name.png)


## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Πώς μπορώ να διαχειριστώ κανόνες τείχους προστασίας για τον έλεγχο της πρόσβασης σε SQL server και της βάσης δεδομένων μου;

Για να προβάλετε, να δημιουργήσετε ή να ενημερώσετε κανόνες τείχους προστασίας, κάντε κλικ στην επιλογή **Ορισμός τείχους προστασίας διακομιστή** blade τη **βάση δεδομένων SQL** . Για λεπτομέρειες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ενός κανόνα τείχους προστασίας του επιπέδου διακομιστή βάσης δεδομένων SQL Azure με την πύλη Azure](sql-database-configure-firewall-settings.md).


![κανόνες τείχους προστασίας](./media/sql-database-manage-portal/sql-database-firewall.png)


## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Πώς μπορώ να αλλάξω μου SQL βάσης δεδομένων υπηρεσιών επιπέδων ή επιδόσεων επιπέδου;


Για να ενημερώσετε το επίπεδο σειρά ή επιδόσεων υπηρεσίας από μια βάση δεδομένων SQL, κάντε κλικ στην εντολή **σειρά Τιμολόγηση (κλίμακα DTUs)** blade τη **βάση δεδομένων SQL** . Για λεπτομέρειες, ανατρέξτε στο θέμα [Αλλαγή υπηρεσίας επίπεδο και επιδόσεων επιπέδου (επίπεδο τις τιμές) μια βάση δεδομένων SQL](sql-database-scale-up.md).


![τις τιμές σειρών](./media/sql-database-manage-portal/pricing-tier.png)


## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Πώς πρέπει να ρυθμίσω τον έλεγχο και την απειλή ανίχνευσης για μια βάση δεδομένων SQL;

Για να ρυθμίσετε τον έλεγχο και την απειλή ανίχνευσης για μια βάση δεδομένων SQL, κάντε κλικ στην επιλογή **Έλεγχος και απειλή ανίχνευση** σε blade τη **βάση δεδομένων SQL** . Για λεπτομέρειες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με τον έλεγχο βάσης δεδομένων SQL](sql-database-auditing-get-started.md)και [Γρήγορα αποτελέσματα με τον εντοπισμό απειλή βάσης δεδομένων SQL](sql-database-threat-detection-get-started.md).


## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Πώς να η ρύθμιση παραμέτρων δυναμικά δεδομένα masking για μια βάση δεδομένων SQL;

Για να ρυθμίσετε τις παραμέτρους δυναμικά δεδομένα masking για μια βάση δεδομένων SQL, κάντε κλικ στην επιλογή **δυναμικά δεδομένα masking** στην blade τη **βάση δεδομένων SQL** . Για λεπτομέρειες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το SQL βάσης δεδομένων δυναμικής δεδομένων Masking](sql-database-dynamic-data-masking-get-started.md).


## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Πώς πρέπει να ρυθμίσω κρυπτογράφηση διαφανή δεδομένων (TDE) για μια βάση δεδομένων SQL;

Για να ρυθμίσετε τις παραμέτρους κρυπτογράφηση διαφανή δεδομένων για μια βάση δεδομένων SQL, κάντε κλικ στην επιλογή **κρυπτογράφηση διαφανή δεδομένων** σε blade τη **βάση δεδομένων SQL** . Για λεπτομέρειες, ανατρέξτε στο θέμα [Ενεργοποίηση TDE σε μια βάση δεδομένων με την πύλη](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Πώς να να προβάλετε ή να αλλάξετε το μέγιστο μέγεθος μιας βάσης δεδομένων SQL;

Για να προβάλετε ή να αλλάξετε το μέγεθος μιας βάσης δεδομένων SQL, κάντε κλικ στο **μέγεθος της βάσης δεδομένων** στην blade τη **βάση δεδομένων SQL** . Ενημερώστε το μέγιστο μέγεθος μιας βάσης δεδομένων, αλλάζοντας το επίπεδο σειρά ή επιδόσεων υπηρεσίας. Για λεπτομέρειες, ανατρέξτε στο θέμα [Αλλαγή υπηρεσίας επίπεδο και επιδόσεων επιπέδου (επίπεδο τις τιμές) μια βάση δεδομένων SQL](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Πώς να παρακολουθείτε και να βελτιώσετε την απόδοση της μια βάση δεδομένων SQL;

Για την παρακολούθηση και τη βελτίωση απόδοσης χαρακτηριστικά από μια βάση δεδομένων SQL, κάντε κλικ στην επιλογή **Επισκόπηση επιδόσεων** σε blade τη **βάση δεδομένων SQL** . Για λεπτομέρειες, ανατρέξτε στο θέμα [Πληροφορίες για επιδόσεων βάσης δεδομένων SQL](sql-database-performance.md).


## <a name="how-do-i-configure-geo-replication"></a>Πώς να η ρύθμιση παραμέτρων παν αναπαραγωγής;

Για να ρυθμίσετε παν αναπαραγωγής για μια βάση δεδομένων SQL, κάντε κλικ στην επιλογή **Παν αναπαραγωγής** στην blade τη **βάση δεδομένων SQL** . Για λεπτομέρειες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων παν-αναπαραγωγή για βάση δεδομένων SQL Azure με την πύλη του Azure](sql-database-geo-replication-portal.md).


## <a name="how-do-i-failover-to-a-geo-replicated-sql-database"></a>Πώς μπορώ να ανακατεύθυνση σε μια βάση δεδομένων SQL αναπαραχθούν παν;

Η δυνατότητα ανακατεύθυνσης σε δευτερεύον αναπαραχθούν παν, κάντε κλικ στην επιλογή **Παν αναπαραγωγής** στην blade τη **βάση δεδομένων SQL** και, στη συνέχεια, κάντε κλικ στην επιλογή **ανακατεύθυνσης**. Για λεπτομέρειες, ανατρέξτε στο θέμα [Προετοιμασία προγραμματισμένες ή μη προγραμματισμένες ανακατεύθυνσης για βάση δεδομένων SQL Azure με την πύλη του Azure](sql-database-geo-replication-failover-portal.md).


## <a name="how-do-i-copy-a-sql-database"></a>Πώς μπορώ να αντιγράψω μια βάση δεδομένων SQL;

Για να αντιγράψετε μια βάση δεδομένων SQL, κάντε κλικ στην επιλογή **Αντιγραφή** σε blade τη **βάση δεδομένων SQL** . Για λεπτομέρειες, ανατρέξτε στο θέμα [Αντιγραφή μιας βάσης δεδομένων Azure SQL με την πύλη Azure](sql-database-copy-portal.md).


![Ρυθμίσεις της βάσης δεδομένων SQL](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Πώς μπορώ να αρχειοθετήσω μια βάση δεδομένων Azure SQL σε ένα αρχείο BACPAC;

Για να δημιουργήσετε μια BACPAC από μια βάση δεδομένων SQL, κάντε κλικ στην επιλογή **Εξαγωγή** σε blade τη **βάση δεδομένων SQL** . Για λεπτομέρειες, ανατρέξτε στο θέμα [αρχειοθέτησης μια βάση δεδομένων Azure SQL σε ένα αρχείο BACPAC με την πύλη Azure](sql-database-export.md).


![Εξαγωγή της βάσης δεδομένων SQL](./media/sql-database-manage-portal/sql-database-export.png)



## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Πώς μπορώ να επαναφέρω μια βάση δεδομένων SQL σε ένα προηγούμενο σημείο στιγμή;

Για να επαναφέρετε μια βάση δεδομένων SQL, κάντε κλικ στην εντολή **Επαναφορά** blade τη **βάση δεδομένων SQL** . Για λεπτομέρειες, ανατρέξτε στο θέμα [Επαναφορά μιας βάσης δεδομένων SQL Azure σε μια προηγούμενη χρονική στιγμή με την πύλη του Azure](sql-database-point-in-time-restore-portal.md).


![Ρυθμίσεις της βάσης δεδομένων SQL](./media/sql-database-manage-portal/sql-database-restore.png)


## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Πώς μπορώ να δημιουργήσω μια βάση δεδομένων Azure SQL από ένα αρχείο BACPAC;

Για να δημιουργήσετε μια βάση δεδομένων SQL από ένα αρχείο BACPAC, κάντε κλικ στην εντολή **Εισαγωγή βάσης δεδομένων** του **SQL server** blade. Για λεπτομέρειες, ανατρέξτε στο θέμα [Εισαγωγή ενός αρχείου BACPAC για να δημιουργήσετε μια βάση δεδομένων Azure SQL](sql-database-import.md).


![SQL server](./media/sql-database-manage-portal/server-commands.png)


## <a name="how-do-i-restore-a-deleted-sql-database"></a>Πώς μπορώ να επαναφέρω ένα διαγραμμένο βάση δεδομένων SQL;

Για να επαναφέρετε μια διαγραμμένη βάση δεδομένων SQL, κάντε κλικ στην επιλογή **Διαγραφή βάσεων δεδομένων** του **SQL server** blade (τον SQL server που περιέχεται στη βάση δεδομένων που έχει διαγραφεί). Για λεπτομέρειες, ανατρέξτε στο θέμα [Επαναφορά διαγραμμένων βάσης δεδομένων Azure SQL με την πύλη Azure](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Πώς μπορώ να διαγράψω μια βάση δεδομένων SQL;

Για να διαγράψετε μια βάση δεδομένων SQL, κάντε κλικ στην εντολή **Διαγραφή** blade τη **βάση δεδομένων SQL** . 

![Ρυθμίσεις της βάσης δεδομένων SQL](./media/sql-database-manage-portal/sql-database-delete.png)



## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Βάση δεδομένων SQL](sql-database-technical-overview.md)
- [Παρακολούθηση και διαχείριση χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων με την πύλη του Azure](sql-database-elastic-pool-manage-portal.md)
