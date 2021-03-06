<properties
   pageTitle="Παρουσιάστε στον κατάλογο δεδομένων λίμνης ανάλυση U-SQL Azure | Azure"
   description="Παρουσιάστε στον κατάλογο δεδομένων λίμνης ανάλυση U-SQL Azure"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="use-u-sql-catalog"></a>Χρήση καταλόγου U-SQL

Τον κατάλογο U-SQL χρησιμοποιείται για τη Δόμηση δεδομένων και κώδικα, ώστε να είναι κοινόχρηστη με δέσμες ενεργειών U-SQL. Τον κατάλογο επιτρέπει την υψηλότερη απόδοση πιθανές με δεδομένα σε Azure λίμνης δεδομένων.

Κάθε λογαριασμός Azure δεδομένων λίμνης ανάλυση έχει ακριβώς ένα κατάλογο U-SQL που σχετίζεται με το. Δεν μπορείτε να διαγράψετε τον κατάλογο U-SQL. Προς το παρόν δεν μπορεί να είναι κοινόχρηστη κατάλογοι U-SQL μεταξύ λογαριασμών χώρου αποθήκευσης λίμνης δεδομένων.

Κάθε κατάλογος U-SQL περιέχει μια βάση δεδομένων που ονομάζεται **υπόδειγμα**. Η κύρια βάση δεδομένων δεν είναι δυνατό να διαγραφεί.  Κάθε καταλόγου U-SQL μπορεί να περιέχει περισσότερες πρόσθετες βάσεις δεδομένων.

Βάση δεδομένων U SQL περιέχει:

- Συγκροτήσεις – κοινή χρήση κώδικα .NET μεταξύ των δεσμών ενεργειών U-SQL.
- Συναρτήσεις τιμές πίνακα – κοινή χρήση κώδικα U SQL μεταξύ των δεσμών ενεργειών U-SQL.
- Πίνακες-κοινή χρήση δεδομένων μεταξύ των δεσμών ενεργειών U-SQL.
- Οι διατάξεις - κοινή χρήση πίνακα σχημάτων μεταξύ των δεσμών ενεργειών U-SQL.

## <a name="manage-catalogs"></a>Διαχείριση κατάλογοι
Κάθε λογαριασμός Azure δεδομένων λίμνης ανάλυση έχει έναν προεπιλεγμένο λογαριασμό χώρου αποθήκευσης λίμνης Azure δεδομένων που σχετίζεται με το. Αυτόν το λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων στα οποία αναφέρονται ως του προεπιλεγμένου λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων. Κατάλογος U-SQL αποθηκεύονται σε τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων κάτω από το φάκελο του. Μην διαγράψετε τα αρχεία που βρίσκονται στο φάκελο του.

### <a name="use-azure-portal"></a>Χρήση του Azure πύλη

Ανατρέξτε στο θέμα [Διαχείριση ανάλυσης λίμνης δεδομένων με την πύλη](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog)


### <a name="use-data-lake-tools-for-visual-studio"></a>Χρησιμοποιήστε τα εργαλεία λίμνης δεδομένων για το Visual Studio.

Μπορείτε να χρησιμοποιήσετε εργαλεία λίμνης δεδομένων για το Visual Studio για τη διαχείριση του καταλόγου.  Για περισσότερες πληροφορίες σχετικά με τα εργαλεία, ανατρέξτε στο θέμα [Χρήση εργαλεία λίμνης δεδομένων για το Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

**Για να διαχειριστείτε τον κατάλογο**

1. Ανοίξτε το Visual Studio και συνδεθείτε με azure. Για τις οδηγίες, ανατρέξτε στο θέμα [σύνδεση με το Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure).
1. Ανοίξτε την **Εξερεύνηση διακομιστή** , πατήστε το **Συνδυασμό πλήκτρων CTRL + ALT + S**.
2. Από την **Εξερεύνηση Server**, ανάπτυξη **Azure**, αναπτύξτε **Ανάλυση λίμνης δεδομένα**, αναπτύξτε το λογαριασμό σας ανάλυσης δεδομένων λίμνης, ανάπτυξη **βάσεις δεδομένων**και, στη συνέχεια, αναπτύξτε το στοιχείο **κύριας**.



    - Για να προσθέσετε μια νέα βάση δεδομένων, κάντε δεξί κλικ σε **βάση δεδομένων**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία βάσης δεδομένων**.
    - Για να προσθέσετε μια νέα συγκρότησης, κάντε δεξί κλικ **συγκροτήσεις**και, στη συνέχεια, κάντε κλικ στην επιλογή **Καταχώρηση συγκρότησης**.
    - Για να προσθέσετε μια νέα διάταξη, κάντε δεξί κλικ σε **σχήματα**και, στη συνέχεια, κάντε κλικ στην επιλογή "Δημιουργία σχήματος **.
    - Για να προσθέσετε έναν νέο πίνακα, κάντε δεξί κλικ σε **πίνακες**και, στη συνέχεια, κάντε κλικ στην επιλογή "" Δημιουργία πίνακα **.
    - Για να προσθέσετε μια νέα συνάρτηση πίνακα με τιμών, ανατρέξτε στο θέμα [τελεστές για ανάλυση δεδομένων λίμνης εργασίες ορίζονται από το χρήστη να αναπτύξω U-SQL](data-lake-analytics-u-sql-develop-user-defined-operators.md).


![Αναζήτηση κατάλογοι Visual Studio U-SQL](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## <a name="see-also"></a>Δείτε επίσης

- Γρήγορα αποτελέσματα
    - [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με την πύλη του Azure](data-lake-analytics-get-started-portal.md)
    - [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με χρήση του PowerShell Azure](data-lake-analytics-get-started-powershell.md)
    - [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με Azure .NET SDK](data-lake-analytics-get-started-net-sdk.md)
    - [Ανάπτυξη δεσμών ενεργειών U-SQL χρησιμοποιώντας εργαλεία λίμνης δεδομένων για το Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
    - [Γρήγορα αποτελέσματα με το γλώσσας δεδομένων λίμνης ανάλυση U-SQL Azure](data-lake-analytics-u-sql-get-started.md)

- U-SQL & ανάπτυξης
    - [Γρήγορα αποτελέσματα με το γλώσσας δεδομένων λίμνης ανάλυση U-SQL Azure](data-lake-analytics-u-sql-get-started.md)
    - [Χρησιμοποιήστε συναρτήσεις παραθύρου U-SQL για τα έργα Azure δεδομένων λίμνης ανάλυσης](data-lake-analytics-use-window-functions.md)
    - [Ανάπτυξη U-SQL που ορίζονται από το χρήστη τελεστές για ανάλυση δεδομένων λίμνης εργασίες](data-lake-analytics-u-sql-develop-user-defined-operators.md)

- Διαχείριση
    - [Διαχείριση Azure ανάλυση λίμνης δεδομένων με την πύλη του Azure](data-lake-analytics-manage-use-portal.md)
    - [Διαχείριση Azure ανάλυση λίμνης δεδομένων με χρήση του PowerShell Azure](data-lake-analytics-manage-use-powershell.md)
    - [Παρακολούθηση και αντιμετώπιση προβλημάτων του Azure δεδομένων λίμνης ανάλυσης εργασιών με Azure πύλη](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- Πρόγραμμα εκμάθησης για να ολοκληρωμένες
    - [Χρήση αλληλεπιδραστικών προγράμματα εκμάθησης για ανάλυση λίμνης δεδομένων Azure](data-lake-analytics-use-interactive-tutorials.md)
    - [Ανάλυση αρχεία καταγραφής τοποθεσία Web χρησιμοποιώντας ανάλυση λίμνης δεδομένων Azure](data-lake-analytics-analyze-weblogs.md)
