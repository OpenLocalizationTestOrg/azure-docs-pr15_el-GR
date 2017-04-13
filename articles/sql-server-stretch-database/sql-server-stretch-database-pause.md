<properties
    pageTitle="Παύση και συνέχιση μετεγκατάσταση δεδομένων (βάση δεδομένων παραμόρφωση) | Microsoft Azure"
    description="Μάθετε πώς να παύση ή συνέχιση της μετεγκατάστασης δεδομένων στο Azure."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="pause-and-resume-data-migration-stretch-database"></a>Παύση και συνέχιση μετεγκατάσταση δεδομένων (βάση δεδομένων παραμόρφωση)

Παύση ή συνέχιση μετεγκατάσταση δεδομένων στο Azure, επιλέξτε **παραμόρφωση** για έναν πίνακα στο SQL Server Management Studio και, στη συνέχεια, επιλέξτε **Παύση** για μετεγκατάσταση δεδομένων παύση ή **Συνέχιση** για να συνεχίσετε τη μετεγκατάσταση δεδομένων. Μπορείτε επίσης να χρησιμοποιήσετε Transact\-SQL για παύση ή συνέχιση μετεγκατάσταση δεδομένων.

Παύση μετεγκατάσταση δεδομένων σε μεμονωμένους πίνακες όταν θέλετε να αντιμετωπίσετε προβλήματα στον τοπικό διακομιστή ή για να μεγιστοποιήσετε το διαθέσιμο εύρος ζώνης δικτύου.

## <a name="pause-data-migration"></a>Παύση δεδομένων μετεγκατάστασης

### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Χρήση του SQL Server Management Studio για παύση μετεγκατάσταση δεδομένων

1.  Στο SQL Server Management Studio, στην Εξερεύνηση των αντικειμένων, επιλέξτε την παραμόρφωση\-με δυνατότητα πίνακα για την οποία θέλετε να διακόψετε τη μετεγκατάσταση δεδομένων.

2.  Δεξιά\-κάντε κλικ στην επιλογή και επιλέξτε **παραμόρφωση**και, στη συνέχεια, επιλέξτε **Παύση**.

### <a name="use-transact-sql-to-pause-data-migration"></a>Χρήση Transact\-SQL για παύση μετεγκατάσταση δεδομένων
Εκτελέστε την ακόλουθη εντολή.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Μετεγκατάσταση δεδομένων βιογραφικού σημειώματος

### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>Χρήση του SQL Server Management Studio για να συνεχίσετε τη μετεγκατάσταση δεδομένων

1.  Στο SQL Server Management Studio, στην Εξερεύνηση των αντικειμένων, επιλέξτε την παραμόρφωση\-με δυνατότητα πίνακα για την οποία θέλετε να συνεχίσετε τη μετεγκατάσταση δεδομένων.

2.  Δεξιά\-κάντε κλικ στην επιλογή και επιλέξτε **παραμόρφωση**και, στη συνέχεια, επιλέξτε **βιογραφικό σημείωμα**.

### <a name="use-transact-sql-to-resume-data-migration"></a>Χρήση Transact\-SQL για να συνεχίσετε τη μετεγκατάσταση δεδομένων
Εκτελέστε την ακόλουθη εντολή.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Ελέγξτε εάν μετεγκατάστασης είναι ενεργό ή βρίσκεται σε παύση

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>Χρήση του SQL Server Management Studio για να ελέγξετε εάν μετεγκατάστασης είναι ενεργό ή βρίσκεται σε παύση
Στο SQL Server Management Studio, ανοίξτε **Παραμόρφωση εποπτεία βάσης δεδομένων** και ελέγξτε την τιμή της στήλης **State μετεγκατάστασης** . Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [οθόνη και αντιμετώπιση προβλημάτων μετεγκατάστασης δεδομένων](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Χρησιμοποιήστε Transact-SQL για να ελέγξετε εάν μετεγκατάστασης είναι ενεργό ή βρίσκεται σε παύση
Προβολή του καταλόγου **sys.remote_data_archive_tables** ερωτήματος και ελέγξτε την τιμή της στήλης **is_migration_paused** . Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="see-also"></a>Δείτε επίσης

[Πρόταση ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
[οθόνη και αντιμετώπιση προβλημάτων μετεγκατάστασης δεδομένων](sql-server-stretch-database-monitor.md)
