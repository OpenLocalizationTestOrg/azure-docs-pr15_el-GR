<properties
    pageTitle="Βάση δεδομένων SQL πρόγραμμα εκμάθησης: γρήγορα αποτελέσματα με την ασφάλεια"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε λογαριασμούς χρήστη για να αποκτήσετε πρόσβαση και να διαχειριστείτε μια βάση δεδομένων."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="carlrab"/>

# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>Βάση δεδομένων SQL πρόγραμμα εκμάθησης: Δημιουργία βάσης δεδομένων SQL λογαριασμούς χρηστών για πρόσβαση και διαχείριση μιας βάσης δεδομένων


> [AZURE.SELECTOR]
- [Γρήγορα αποτελέσματα πρόγραμμα εκμάθησης](sql-database-get-started-security.md)
- [Εκχώρηση πρόσβασης](sql-database-manage-logins.md)

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε SQL Studio διαχείρισης διακομιστή (SSMS) για να:

- Συνδεθείτε βάση δεδομένων SQL χρησιμοποιώντας μια σύνδεση κεφαλαίου επιπέδου διακομιστή.
- Δημιουργία λογαριασμού χρήστη βάσης δεδομένων SQL.
- Εκχώρηση μια βάση δεδομένων SQL του χρήστη [db_owner δικαιώματα](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0).
- Σύνδεση με μια βάση δεδομένων SQL με ένα λογαριασμό χρήστη που δεν είναι αρχής επιπέδου διακομιστή.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που έχετε ολοκληρώσει αυτό το πρόγραμμα εκμάθησης βάσης δεδομένων SQL και δημιουργήσει ένα λογαριασμό χρήστη και εκχωρούνται στο χρήστη δικαιώματα dbo λογαριασμού, είστε έτοιμοι για να μάθετε περισσότερα σχετικά με την [ασφάλεια της βάσης δεδομένων SQL](sql-database-manage-logins.md).


