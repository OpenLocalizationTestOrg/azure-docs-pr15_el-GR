<properties
   pageTitle="Ανάλυση αρχεία καταγραφής τοποθεσία Web χρησιμοποιώντας ανάλυση λίμνης δεδομένων Azure | Azure"
   description="Μάθετε πώς μπορείτε να αναλύσετε αρχεία καταγραφής τοποθεσίας Web με χρήση δεδομένων λίμνης ανάλυσης. "
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

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>Πρόγραμμα εκμάθησης: Ανάλυση αρχεία καταγραφής τοποθεσία Web χρησιμοποιώντας ανάλυση λίμνης δεδομένων Azure

Μάθετε πώς μπορείτε να αναλύσετε αρχεία καταγραφής τοποθεσίας Web με χρήση λίμνης ανάλυση δεδομένων, ειδικά σε μάθετε ποια αναφέροντες εκτελέσατε σε σφάλματα κατά την προσπάθεια επισκεφθείτε την τοποθεσία Web.

>[AZURE.NOTE] Εάν θέλετε απλώς να δείτε την εφαρμογή εργασία, αποθηκεύει χρόνο για να μεταβείτε μέσω [αλληλεπιδραστικών προγράμματα εκμάθησης για χρήση Azure δεδομένων λίμνης ανάλυσης](data-lake-analytics-use-interactive-tutorials.md). Αυτό το πρόγραμμα εκμάθησης είναι βάσει του ίδιου σεναρίου και τον ίδιο κωδικό. Το σκοπό αυτού του προγράμματος εκμάθησης είναι να δίνουν στους προγραμματιστές την εμπειρία με τη δημιουργία και την εκτέλεση μιας εφαρμογής ανάλυση λίμνης δεδομένων από λήξη σε λήξη.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία:

- **Visual Studio 2015, το Visual Studio 2013 ενημέρωση 4, ή Visual Studio 2012, με τη Visual C++ εγκατεστημένο**.
- **Microsoft Azure SDK για .NET έκδοση 2.5 ή νεότερη έκδοση**.  Εγκατάσταση χρησιμοποιώντας το [πρόγραμμα εγκατάστασης πλατφόρμας Web](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Εργαλεία λίμνης δεδομένων για το Visual Studio](http://aka.ms/adltoolsvs)**.

    Μόλις εγκαταστήσετε εργαλεία λίμνης δεδομένων για το Visual Studio, θα δείτε ένα μενού **Λίμνης δεδομένων** στο Visual Studio:

    ![Μενού Visual Studio U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Βασικές γνώσεις ανάλυση λίμνης δεδομένων και τα εργαλεία λίμνης δεδομένων για το Visual Studio**. Για να ξεκινήσετε, ανατρέξτε στα θέματα:

    - [Γρήγορα αποτελέσματα με το Azure δεδομένων λίμνης ανάλυση με Azure πύλη](data-lake-analytics-get-started-portal.md).
    - [Ανάπτυξη U-SQL δέσμη ενεργειών, χρησιμοποιώντας εργαλεία λίμνης δεδομένων για το Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

- **Ένας λογαριασμός ανάλυση λίμνης δεδομένων.**  Ανατρέξτε στο θέμα [Δημιουργία λογαριασμού Azure δεδομένων λίμνης ανάλυσης](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).

    Τα εργαλεία λίμνης δεδομένων δεν υποστηρίζει τη δημιουργία λογαριασμών ανάλυση λίμνης δεδομένων.  Επομένως, πρέπει να δημιουργήσετε χρησιμοποιώντας την πύλη Azure, Azure PowerShell, .NET SDK ή Azure CLI.
- **Αποστείλετε το δείγμα δεδομένων στο λογαριασμό ανάλυση λίμνης δεδομένων.** Ανατρέξτε στο θέμα [Αποστολή SearchLog.tsv στον προεπιλεγμένο λογαριασμό χώρος αποθήκευσης δεδομένων λίμνης](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Για να εκτελέσετε μια ανάλυση δεδομένων λίμνης εργασία, θα χρειαστεί ορισμένα δεδομένα. Παρόλο που τα εργαλεία λίμνης δεδομένων υποστηρίζει αποστολής δεδομένων, θα χρησιμοποιήσετε την πύλη για να αποστείλετε το δείγμα δεδομένων για να διευκολύνετε την εκμάθηση για να παρακολουθήσετε.

## <a name="connect-to-azure"></a>Σύνδεση με Azure

Πριν να δημιουργήσετε και να ελέγξετε τις δέσμες ενεργειών U-SQL, πρέπει πρώτα να συνδεθείτε σε Azure.

**Για να συνδεθείτε με δεδομένα λίμνης ανάλυσης**

1. Ανοίξτε το Visual Studio.
2. Από το μενού **Λίμνης δεδομένων** , κάντε κλικ στην επιλογή **ρυθμίσεις και επιλογές**.
4. Κάντε κλικ στην επιλογή **Sign In**ή **Αλλαγή χρήστη** εάν κάποιος έχει πραγματοποιήσει είσοδο και ακολουθήστε τις οδηγίες.
5. Κάντε κλικ στο **κουμπί OK** για να κλείσετε το παράθυρο διαλόγου ρυθμίσεις και επιλογές.

**Για να αναζητήσετε τους λογαριασμούς σας ανάλυσης δεδομένων λίμνης**

1. Από το Visual Studio, ανοίξτε την **Εξερεύνηση διακομιστή** , πατήστε το **Συνδυασμό πλήκτρων CTRL + ALT + S**.
2. Από την **Εξερεύνηση Server**, αναπτύξτε **Azure**και, στη συνέχεια, αναπτύξτε το στοιχείο **Ανάλυση λίμνης δεδομένων**. Μέλη μπορείτε να δείτε μια λίστα με τους λογαριασμούς σας ανάλυση λίμνης δεδομένων, αν υπάρχουν. Δεν μπορείτε να δημιουργήσετε λογαριασμούς ανάλυση λίμνης δεδομένων από το studio. Για να δημιουργήσετε ένα λογαριασμό, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure ανάλυση λίμνης δεδομένων με την πύλη Azure](data-lake-analytics-get-started-portal.md) ή [Γρήγορα αποτελέσματα με το Azure ανάλυση λίμνης δεδομένων με χρήση του PowerShell Azure](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>Ανάπτυξη εφαρμογής U-SQL

Μια εφαρμογή U-SQL είναι κυρίως μια δέσμη ενεργειών U-SQL. Για να μάθετε περισσότερα σχετικά με το U-SQL, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το U-SQL](data-lake-analytics-u-sql-get-started.md).

Μπορείτε να προσθέσετε επιπλέον τελεστές που ορίζονται από το χρήστη στην εφαρμογή.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [τελεστές για ανάλυση δεδομένων λίμνης εργασίες ορίζονται από το χρήστη να αναπτύξω U-SQL](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Για να δημιουργήσετε και να υποβάλετε μια εργασία ανάλυσης δεδομένων λίμνης**

1. Από το μενού **αρχείο** , κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **έργο**.
2. Επιλέξτε τον τύπο του έργου U-SQL.

    ![νέο έργο Visual Studio U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Κάντε κλικ στο **κουμπί OK**. Visual studio δημιουργεί μια λύση με ένα αρχείο Script.usql.
4. Εισαγάγετε την ακόλουθη δέσμη ενεργειών στο αρχείο Script.usql:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    Για να κατανοήσετε το U-SQL, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το γλώσσας δεδομένων ανάλυσης λίμνης U-SQL](data-lake-analytics-u-sql-get-started.md).    

5. Προσθέστε μια νέα δέσμη ενεργειών U-SQL στο έργο σας και πληκτρολογήστε τα εξής:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. Εναλλαγή Επιστροφή στην πρώτη δέσμη ενεργειών U SQL και δίπλα στο κουμπί **Υποβολή** , καθορίστε το λογαριασμό σας ανάλυσης.
7. Από την **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στην επιλογή **Script.usql**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία δέσμης ενεργειών**. Επαληθεύστε τα αποτελέσματα στο παράθυρο εξόδου.
8. Από την **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στην επιλογή **Script.usql**και, στη συνέχεια, κάντε κλικ στην επιλογή **Υποβολή δέσμης ενεργειών**.
9. Επαληθεύστε την **Ανάλυση λογαριασμού** είναι αυτό όπου θέλετε να εκτελέσετε την εργασία και, στη συνέχεια, κάντε κλικ στην επιλογή **Υποβολή**. Αποτελέσματα υποβολής και σύνδεση εργασίας είναι διαθέσιμες στα εργαλεία λίμνης δεδομένων για το παράθυρο του Visual Studio προκαλεί όταν ολοκληρωθεί η υποβολή.
10. Περιμένετε έως ότου η εργασία ολοκληρώθηκε με επιτυχία.  Εάν η εργασία απέτυχε, πιθανώς λείπουν το αρχείο προέλευσης.  Ανατρέξτε στην ενότητα προαπαιτούμενες αυτού του προγράμματος εκμάθησης. Για πρόσθετες πληροφορίες αντιμετώπισης προβλημάτων, ανατρέξτε στο θέμα [οθόνη και αντιμετώπιση προβλημάτων του Azure δεδομένων λίμνης ανάλυση εργασίες](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Όταν ολοκληρωθεί η εργασία, θα βλέπετε η παρακάτω οθόνη:

    ![ανάλυση δεδομένων λίμνης ανάλυση ιστολόγια αρχεία καταγραφής από την τοποθεσία Web](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. Τώρα μπορείτε να επαναλάβετε τα βήματα 7 - 10 για **Script1.usql**.

>[AZURE.NOTE]Δεν μπορείτε να διαβάζετε από ή να γράψετε σε έναν πίνακα U-SQL που έχει δημιουργηθεί ή τροποποιηθεί στην ίδια δέσμη ενεργειών.  Λόγο χρήση δύο δεσμών ενεργειών για αυτό το παράδειγμα.

**Για να δείτε το αποτέλεσμα του έργου**

1. Από την **Εξερεύνηση Server**, αναπτύξτε **Azure**, ανάπτυξη **Ανάλυσης δεδομένων λίμνης**, αναπτύξτε το λογαριασμό σας ανάλυσης δεδομένων λίμνης, αναπτύξτε **Τους λογαριασμούς χώρου αποθήκευσης**, κάντε δεξί κλικ στον προεπιλεγμένο λογαριασμό χώρος αποθήκευσης δεδομένων λίμνης και, στη συνέχεια, κάντε κλικ στην επιλογή **Εξερεύνηση**.
2.  Κάντε διπλό κλικ **δείγματα** για να ανοίξετε το φάκελο και, στη συνέχεια, κάντε διπλό κλικ **εξόδους**.
3.  Κάντε διπλό κλικ **UnsuccessfulResponsees.log**.
4.  Μπορείτε επίσης να κάνετε διπλό κλικ το αρχείο εξόδου μέσα σε προβολή γραφήματος της εργασίας για να μεταβείτε απευθείας το αποτέλεσμα του.

## <a name="see-also"></a>Δείτε επίσης

Για να ξεκινήσετε με ανάλυση λίμνης δεδομένων με διάφορα εργαλεία, ανατρέξτε στα θέματα:

- [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με την πύλη Azure](data-lake-analytics-get-started-portal.md)
- [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με χρήση του PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με .NET SDK](data-lake-analytics-get-started-net-sdk.md)

Για να δείτε περισσότερα θέματα ανάπτυξης:

- [Ανάπτυξη δεσμών ενεργειών U-SQL χρησιμοποιώντας εργαλεία λίμνης δεδομένων για το Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Γρήγορα αποτελέσματα με το γλώσσας δεδομένων λίμνης ανάλυση U-SQL Azure](data-lake-analytics-u-sql-get-started.md)
- [Ανάπτυξη U-SQL που ορίζονται από το χρήστη τελεστές για ανάλυση δεδομένων λίμνης εργασίες](data-lake-analytics-u-sql-develop-user-defined-operators.md)
