<properties
    pageTitle="Αντιγράψτε μια βάση δεδομένων Azure SQL με την πύλη Azure | Microsoft Azure"
    description="Δημιουργήστε ένα αντίγραφο της βάση δεδομένων Azure SQL"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database-using-the-azure-portal"></a>Αντιγράψτε μια βάση δεδομένων SQL Azure με την πύλη Azure

> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-copy.md)
- [Πύλη του Azure](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Ακολουθήστε τα παρακάτω βήματα που δείχνουν τον τρόπο για να αντιγράψετε μια βάση δεδομένων SQL με την [πύλη του Azure](https://portal.azure.com) στον ίδιο διακομιστή ή σε διαφορετικό διακομιστή.

Για να αντιγράψετε μια βάση δεδομένων SQL, χρειάζεστε τα ακόλουθα στοιχεία:

- Μια συνδρομή του Azure. Εάν χρειάζεστε μια συνδρομή του Azure απλώς κάντε κλικ στην επιλογή **ΔΩΡΕΆΝ δοκιμαστική ΈΚΔΟΣΗ** στο επάνω μέρος αυτής της σελίδας και, στη συνέχεια, επιστρέψτε στο τέλος αυτού του άρθρου.
- Μια βάση δεδομένων SQL για να αντιγράψετε. Εάν δεν έχετε μια βάση δεδομένων SQL, να δημιουργήσετε μία ακολουθώντας τα βήματα σε αυτό το άρθρο: [Δημιουργία του πρώτου βάση δεδομένων SQL Azure](sql-database-get-started.md).


## <a name="copy-your-sql-database"></a>Αντιγράψτε τη βάση δεδομένων SQL

Ανοίξτε τη σελίδα βάσης δεδομένων SQL για τη βάση δεδομένων που θέλετε να αντιγράψετε:

1.  Μεταβείτε στην [πύλη του Azure](https://portal.azure.com).
2.  Κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες** > **βάσεις δεδομένων SQL**, και, στη συνέχεια, κάντε κλικ στην επιλογή βάσης δεδομένων που θέλετε.
3.  Στη σελίδα βάσης δεδομένων SQL, κάντε κλικ στην επιλογή **Αντιγραφή**:

    ![Βάση δεδομένων SQL](./media/sql-database-copy-portal/sql-database-copy.png)

1.  Στη σελίδα **Αντιγραφή** , παρέχεται ένα προεπιλεγμένο όνομα βάσης δεδομένων. Εάν θέλετε, πληκτρολογήστε ένα διαφορετικό όνομα (όλες τις βάσεις δεδομένων σε ένα διακομιστή πρέπει να έχουν μοναδικά ονόματα).
2.  Επιλέξτε ένα **διακομιστή προορισμού**. Ο διακομιστής προορισμού είναι όπου έχει δημιουργηθεί το αντίγραφο της βάσης δεδομένων. Μπορείτε να αντιγράψετε τη βάση δεδομένων στον ίδιο διακομιστή ή σε διαφορετικό διακομιστή. Μπορείτε να δημιουργήσετε ένα διακομιστή ή να επιλέξετε έναν υπάρχοντα διακομιστή από τη λίστα. 
3.  Αφού επιλέξετε το **διακομιστή προορισμού**, **χώρος συγκέντρωσης ελαστικότητας βάσης δεδομένων**και **Τιμολόγηση επίπεδο** θα είναι ενεργοποιημένες επιλογές. Εάν ο διακομιστής έχει ένα χώρο συγκέντρωσης, μπορείτε να αντιγράψετε τη βάση δεδομένων σε αυτήν.
3.  Κάντε κλικ στο **κουμπί OK** για να ξεκινήσει η διαδικασία αντιγραφής.

    ![Βάση δεδομένων SQL](./media/sql-database-copy-portal/copy-page.png)


## <a name="monitor-the-progress-of-the-copy-operation"></a>Παρακολούθηση της προόδου της λειτουργίας αντιγραφής

- Μετά την εκκίνηση στο αντίγραφο, κάντε κλικ στην πύλη ειδοποίηση για λεπτομέρειες.

    ![ειδοποίηση][3]
 
    ![οθόνη][4]


## <a name="verify-the-database-is-live-on-the-server"></a>Επαληθεύστε τη βάση δεδομένων είναι ενεργή στο διακομιστή

- Κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες** > **βάσεις δεδομένων SQL** και να επαληθεύσετε τη νέα βάση δεδομένων είναι σε **λειτουργία**.


## <a name="resolve-logins"></a>Επίλυση συνδέσεις

Για να επιλύσετε συνδέσεις μετά την ολοκλήρωση της λειτουργίας αντιγραφής, ανατρέξτε στο θέμα [επίλυση συνδέσεις](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="next-steps"></a>Επόμενα βήματα

- Για μια επισκόπηση του αντιγράφοντας μια βάση δεδομένων SQL Azure, ανατρέξτε στο θέμα [Αντιγραφή μιας βάσης δεδομένων Azure SQL](sql-database-copy.md) .
- Ανατρέξτε στο θέμα [Αντιγραφή μια βάση δεδομένων Azure SQL με χρήση του PowerShell](sql-database-copy-powershell.md) για να αντιγράψετε μια βάση δεδομένων με χρήση του PowerShell.
- Ανατρέξτε στο θέμα [Αντιγραφή μια βάση δεδομένων Azure SQL χρησιμοποιώντας T-SQL](sql-database-copy-transact-sql.md) για να αντιγράψετε μια βάση δεδομένων με χρήση Transact-SQL.
- Δείτε [πώς μπορείτε να διαχειριστείτε ασφαλείας βάσης δεδομένων Azure SQL μετά την αποκατάσταση](sql-database-geo-replication-security-config.md) για να μάθετε περισσότερα σχετικά με τη Διαχείριση χρηστών και συνδέσεις κατά την αντιγραφή μιας βάσης δεδομένων σε διαφορετικό διακομιστή λογική.



## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Διαχείριση συνδέσεων](sql-database-manage-logins.md)
- [Σύνδεση με βάση δεδομένων SQL με SQL Server Management Studio και να εκτελέσετε ένα ερώτημα T SQL δείγματος](sql-database-connect-query-ssms.md)
- [Εξαγωγή της βάσης δεδομένων σε ένα BACPAC](sql-database-export.md)
- [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)
- [Τεκμηρίωση βάσης δεδομένων SQL](https://azure.microsoft.com/documentation/services/sql-database/)




<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png

