<properties
    pageTitle="Πρόγραμμα εκμάθησης: Web app με μια βάση δεδομένων πολλών μισθωτών χρησιμοποιώντας Framework οντότητα και την ασφάλεια επιπέδου γραμμής"
    description="Μάθετε πώς μπορείτε να αναπτύξετε μια εφαρμογή web ASP.NET MVC 5 με βάση δεδομένων SQL backent, με χρήση του πλαισίου οντότητα και ασφάλεια σε επίπεδο γραμμής πολλών μισθωτή."
  metaKeywords="azure asp.net mvc entity framework multi tenant row level security rls sql database"
    services="app-service\web"
    documentationCenter=".net"
    manager="jeffreyg"
  authors="tmullaney"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/25/2016"
    ms.author="thmullan"/>

# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Πρόγραμμα εκμάθησης: Web app με μια βάση δεδομένων πολλών μισθωτών χρησιμοποιώντας Framework οντότητα και την ασφάλεια επιπέδου γραμμής

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να δημιουργήσετε μια εφαρμογή web πολλών μισθωτή με "[κοινόχρηστη βάση δεδομένων, κοινόχρηστο σχήματος](https://msdn.microsoft.com/library/aa479086.aspx)" μίσθωση μοντέλου, με το πλαίσιο οντότητα και [Ασφάλεια σε επίπεδο γραμμής](https://msdn.microsoft.com/library/dn765131.aspx). Σε αυτό το μοντέλο, μία βάση δεδομένων περιέχει δεδομένα για πολλούς μισθωτές και κάθε γραμμή σε κάθε πίνακα είναι συσχετισμένη με μια "μισθωτή αναγνωριστικό". Γραμμή επίπεδο ασφάλειας (RLS), μια νέα δυνατότητα για βάση δεδομένων SQL Azure, χρησιμοποιείται για να αποτρέψετε την πρόσβαση στα δεδομένα του άλλου μισθωτές. Αυτό απαιτεί απλώς μια μεμονωμένη, μικρή αλλαγή στην εφαρμογή. Συγκεντρώνοντας της λογικής access μισθωτή μέσα σε ίδια τη βάση δεδομένων, RLS απλοποιεί τον κώδικα της εφαρμογής και μειώνει τον κίνδυνο διαρροής τυχαίες δεδομένων μεταξύ των μισθωτών.

Ας ξεκινήσουμε με την απλή εφαρμογή Contact Manager από [Δημιουργία εφαρμογής ASP.NET MVP με auth και SQL DB και ανάπτυξη Azure εφαρμογής υπηρεσίας](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Δεξιά τώρα, η εφαρμογή επιτρέπει σε όλους τους χρήστες (μισθωτές) για να δείτε όλες τις επαφές:

![Διαχείριση επαφών εφαρμογή πριν από την ενεργοποίηση RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Με λίγα μικρές αλλαγές, θα προσθέσουμε υποστήριξη για πολλαπλή μίσθωση, έτσι ώστε οι χρήστες μπορούν να βλέπουν μόνο τις επαφές που ανήκουν σε αυτές.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Βήμα 1: Προσθήκη μιας κλάσης Interceptor στην εφαρμογή για να ορίσετε το SESSION_CONTEXT

Υπάρχει μία αλλαγή εφαρμογής πρέπει να κάνετε. Επειδή όλοι οι χρήστες της εφαρμογής, συνδεθείτε με τη βάση δεδομένων χρησιμοποιώντας την ίδια συμβολοσειρά σύνδεσης (δηλαδή σύνδεση ίδια SQL), δεν υπάρχει αυτήν τη στιγμή τρόπος για μια πολιτική RLS να γνωρίζετε ποιος χρήστης θα πρέπει να εφαρμόσετε φίλτρο για. Αυτή η προσέγγιση είναι πολύ συνηθισμένο στις εφαρμογές web, επειδή δίνει τη δυνατότητα αποτελεσματική συγκέντρωση συνδέσεων, αλλά αυτό σημαίνει χρειαζόμαστε ένας άλλος τρόπος για να προσδιορίσετε τον τρέχοντα χρήστη εφαρμογή μέσα στη βάση δεδομένων. Η λύση είναι να εγκαταστήσει την εφαρμογή, ορίστε ένα ζεύγος κλειδιού-τιμής για το τρέχον αναγνωριστικό χρήστη στο το [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) αμέσως μετά το άνοιγμα μιας σύνδεσης, πριν να εκτελέσει οποιαδήποτε ερωτήματα. SESSION_CONTEXT είναι μια τιμή του αριθμού-κλειδιού την αποθήκευση εμβέλειας περιόδου λειτουργίας και την πολιτική μας RLS θα χρησιμοποιήσει το αναγνωριστικό χρήστη που είναι αποθηκευμένα σε αυτό για να προσδιορίσετε τον τρέχοντα χρήστη.

Θα προσθέσουμε μια [interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (ιδίως μια [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), μια νέα δυνατότητα στην οντότητα Framework (EF) 6, για να ρυθμίσει αυτόματα το τρέχον αναγνωριστικό χρήστη στο το SESSION_CONTEXT με την εκτέλεση μιας πρότασης T SQL κάθε φορά που EF ανοίγει μια σύνδεση.

1.  Ανοίξτε το έργο ContactManager στο Visual Studio.
2.  Κάντε δεξί κλικ στο φάκελο μοντέλα στην Εξερεύνηση λύσεων και επιλέξτε Προσθήκη > τάξης.
3.  Δώστε ένα όνομα τη νέα κλάση "SessionContextInterceptor.cs" και κάντε κλικ στην επιλογή Προσθήκη.
4.  Αντικαταστήστε τα περιεχόμενα του SessionContextInterceptor.cs με τον ακόλουθο κώδικα.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }
        
        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

Που είναι η αλλαγή μόνο εφαρμογής απαιτείται. Προχωρήστε και δημιουργία και δημοσίευση της εφαρμογής.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Βήμα 2: Προσθήκη στήλης αναγνωριστικό χρήστη του σχήματος βάσης δεδομένων

Στη συνέχεια, θα χρειαστεί να προσθέσετε μια στήλη αναγνωριστικό χρήστη για τον πίνακα επαφών για να συσχετίσετε κάθε γραμμή με ένα χρήστη (μισθωτής). Θα σας θα αλλάξει το σχήμα απευθείας στη βάση δεδομένων, ώστε να σας δεν χρειάζεται να συμπεριλάβετε το πεδίο στο μας EF μοντέλο δεδομένων.

Απευθείας σύνδεση με τη βάση δεδομένων, χρησιμοποιώντας το SQL Server Management Studio ή Visual Studio, και, στη συνέχεια, εκτελέστε την παρακάτω T-SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Αυτό προσθέτει μια στήλη αναγνωριστικό χρήστη του πίνακα επαφών. Χρησιμοποιούμε τον τύπο δεδομένων nvarchar(128) ώστε να ταιριάζει με το UserIds αποθηκεύονται στον πίνακα AspNetUsers και, δημιουργούμε έναν ΠΡΟΕΠΙΛΕΓΜΈΝΟ περιορισμό που θα ρυθμίσει αυτόματα το αναγνωριστικό χρήστη για νέες γραμμές να είναι το αναγνωριστικό χρήστη που είναι αποθηκευμένα στη SESSION_CONTEXT.

Τώρα ο πίνακας μοιάζει κάπως έτσι:

![Πίνακας επαφών SSMS](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Κατά τη δημιουργία νέων επαφών, θα αυτόματα τους ανατεθούν το σωστό αναγνωριστικό χρήστη. Για σκοπούς επίδειξης, ωστόσο, ας αντιστοιχίσετε μερικές από αυτές τις υπάρχουσες επαφές σας σε έναν υπάρχοντα χρήστη.

Εάν έχετε δημιουργήσει μερικούς χρήστες στην εφαρμογή ήδη (π.χ., με τοπική, Google ή Facebook λογαριασμοί), θα δείτε τις στον πίνακα AspNetUsers. Στο παρακάτω στιγμιότυπο, υπάρχει μόνο ένας χρήστης μέχρι στιγμής.

![SSMS AspNetUsers πίνακα](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Αντιγράψτε το αναγνωριστικό για user1@contoso.com, και επικολλήστε τον στο παρακάτω δήλωση T-SQL. Εκτέλεση αυτήν τη δήλωση για να συσχετίσετε τρεις από τις επαφές με το αναγνωριστικό χρήστη.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Βήμα 3: Δημιουργία πολιτικής ασφάλειας σε επίπεδο γραμμής βάσης δεδομένων

Το τελικό βήμα είναι να δημιουργήσετε μια πολιτική ασφαλείας που χρησιμοποιεί το αναγνωριστικό χρήστη στο SESSION_CONTEXT για αυτόματο φιλτράρισμα των αποτελεσμάτων που επιστρέφονται από ερωτήματα.

Ενώ εξακολουθείτε να είστε συνδεδεμένοι στη βάση δεδομένων, εκτελέστε την παρακάτω T-SQL:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

Αυτός ο κωδικός σημαίνει τρία στοιχεία. Πρώτα, δημιουργεί μια νέα διάταξη ως βέλτιστη πρακτική για τη συγκέντρωση και τον περιορισμό της πρόσβασης στα αντικείμενα RLS. Στη συνέχεια, δημιουργεί μια συνάρτηση κατηγορήματος που θα επιστρέψει "1", όταν το αναγνωριστικό χρήστη της γραμμής να ταιριάζει με το αναγνωριστικό χρήστη στο SESSION_CONTEXT. Τέλος, δημιουργεί μια πολιτική ασφαλείας που προσθέτει αυτή η συνάρτηση ως κατηγόρημα τόσο ενός φίλτρου και μπλοκ στον πίνακα "Επαφές". Το φίλτρο κατηγόρημα προκαλεί ερωτημάτων για να επιστρέψετε μόνο τις γραμμές που ανήκουν στον τρέχοντα χρήστη και το κατηγόρημα μπλοκ λειτουργεί ως μια στη διαφύλαξη για να αποτρέψετε την εφαρμογή από την εισαγωγή ποτέ κατά λάθος μια γραμμή για το πρόβλημα χρήστη.

Τώρα, εκτελέστε την εφαρμογή και συνδεθείτε ως user1@contoso.com. Αυτός ο χρήστης βλέπει τώρα μόνο τις επαφές σας που έχουν εκχωρηθεί σε αυτό το αναγνωριστικό χρήστη παλαιότερη έκδοση:

![Διαχείριση επαφών εφαρμογή πριν από την ενεργοποίηση RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Για να επικυρώσετε αυτό περαιτέρω, δοκιμάστε τη δήλωση έναν νέο χρήστη. Θα δουν χωρίς επαφές, επειδή κανένα έχουν εκχωρηθεί σε αυτά. Εάν δημιουργούν μια νέα επαφή, εκχωρούνται σε αυτά και μόνο θα μπορούν να τις δει.

## <a name="next-steps"></a>Επόμενα βήματα

Αυτό είναι! Απλή εφαρμογή web Contact Manager έχει μετατραπεί σε μια πολλαπλή μισθωτή μία όπου κάθε χρήστης έχει το δικό της λίστας επαφών. Χρησιμοποιώντας την ασφάλεια σε επίπεδο γραμμής, θα σας έχετε αποφεύγεται η πολυπλοκότητα επιβολής μισθωτή λογικής πρόσβαση στο μας κώδικα της εφαρμογής. Αυτή η διαφάνεια επιτρέπει στην εφαρμογή για να εστιάσετε σε το πρόβλημα πραγματική επιχειρηματική εκτελείται τη συγκεκριμένη στιγμή, και επίσης μειώνει τον κίνδυνο κατά λάθος διαρροή δεδομένων, όπως η εφαρμογή βάσης του κώδικα εξελίσσεται.

Αυτό το πρόγραμμα εκμάθησης περιλαμβάνει μόνο εάν συμβαίνει στην επιφάνεια της τι είναι δυνατή με RLS. Για παράδειγμα, είναι δυνατό να έχουν πιο περίπλοκη ή λογικής λεπτομερούς access και θα του δυνατή η αποθήκευση περισσότερο από το τρέχον αναγνωριστικό χρήστη στο το SESSION_CONTEXT. Αυτό είναι επίσης πιθανό να [ενοποιήσετε το RLS με τις βιβλιοθήκες ελαστικότητας βάσης δεδομένων εργαλεία προγράμματος-πελάτη](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) για την υποστήριξη πολλών μισθωτών shards σε μια σειρά δεδομένων κλιμάκωσης.

Πέρα από αυτές τις δυνατότητες, Προσπαθούμε επίσης να κάνετε ακόμη καλύτερα RLS. Εάν έχετε ερωτήσεις, ιδέες ή στοιχεία που θέλετε να δείτε, ενημερώστε μας στα σχόλια. Εκτιμούμε ιδιαίτερα τα σχόλιά σας!
