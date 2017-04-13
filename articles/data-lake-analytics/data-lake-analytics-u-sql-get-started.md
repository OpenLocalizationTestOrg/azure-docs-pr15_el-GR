<properties
   pageTitle="Ανάπτυξη δεσμών ενεργειών U-SQL χρησιμοποιώντας εργαλεία λίμνης δεδομένων για το Visual Studio | Azure"
   description="Μάθετε πώς μπορείτε να εγκαταστήσετε εργαλεία λίμνης δεδομένων για το Visual Studio, τον τρόπο ανάπτυξης και δεσμών ενεργειών δοκιμής U-SQL. "
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

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Πρόγραμμα εκμάθησης: Γρήγορα αποτελέσματα με το γλώσσας δεδομένων λίμνης ανάλυση U-SQL Azure

U-SQL είναι μια γλώσσα που ενοποιεί τα πλεονεκτήματα της SQL με τη εκφραστικές δύναμη του δικού σας κώδικα για την επεξεργασία όλων των δεδομένων σε οποιαδήποτε κλίμακα. Η δυνατότητα με κατανεμημένο ερώτημα U SQL σάς επιτρέπει να αποτελεσματική ανάλυση δεδομένων στο χώρο αποθήκευσης και σε σχεσιακή αποθηκεύει όπως η βάση δεδομένων SQL Azure.  Σας επιτρέπει να διαδικασία μη δομημένα δεδομένα, εφαρμόζοντας σχήματος στην ανάγνωση, εισαγάγετε προσαρμοσμένη λογική και του UDF και περιλαμβάνει επεκτασιμότητα του για να ενεργοποιήσετε την ρυθμίσετε κοκκώδης έλεγχο τον τρόπο εκτέλεσης σε κλίμακα. Για να μάθετε περισσότερα σχετικά με τη σχεδίαση φιλοσοφίας πίσω από το Α-SQL, ανατρέξτε σε αυτό [ιστολογίου του Visual Studio](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Υπάρχουν ορισμένες διαφορές από ANSI SQL ή T-SQL. Για παράδειγμα, τις λέξεις-κλειδιά όπως SELECT πρέπει να βρίσκονται σε ΚΕΦΑΛΑΙΑ.

Το του τύπου σύστημα και παράσταση γλώσσας μέσα όροι select, πού βρίσκονται οι κατηγορήματα κ.λπ στο C#.
Αυτό σημαίνει ότι οι τύποι δεδομένων που είναι οι τύποι C# και C# NULL σημασιολογία Χρησιμοποιήστε τους τύπους δεδομένων και τις λειτουργίες σύγκρισης μέσα σε ένα κατηγόρημα ακολουθήστε C# σύνταξη (π.χ., μια == "foo").  Αυτό σημαίνει επίσης, ότι οι τιμές είναι πλήρης .NET αντικείμενα, επιτρέποντάς σας να χρησιμοποιούν εύκολα οποιαδήποτε μέθοδο για τη λειτουργία του αντικειμένου (π.χ. "f o o o". Split(' ')).

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αναφορά U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).

###<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Πρέπει να ολοκληρώσετε [πρόγραμμα εκμάθησης: ανάπτυξη δεσμών ενεργειών U-SQL χρησιμοποιώντας εργαλεία λίμνης δεδομένων για το Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

Στο πρόγραμμα εκμάθησης, εκτελέσατε μια εργασία ανάλυσης λίμνης δεδομένων με την ακόλουθη δέσμη ενεργειών U-SQL:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

Αυτή η δέσμη ενεργειών δεν διαθέτει τα βήματα μετασχηματισμό. Το διαβάζει από το αρχείο προέλευσης που ονομάζεται **SearchLog.tsv**, το schematizes και εξάγει το σύνολο γραμμών σε ένα αρχείο που ονομάζεται **SearchLog-πρώτης-u-sql.csv**.

Παρατηρήστε το αγγλικό ερωτηματικό δίπλα στον τύπο δεδομένων του πεδίου "διάρκεια". Αυτό σημαίνει ότι το πεδίο "διάρκεια" μπορεί να είναι null.

Ορισμένες έννοιες και λέξεις-κλειδιά βρίσκονται στη δέσμη ενεργειών:

- **Μεταβλητές σύνολο γραμμών**: κάθε παράσταση ερωτήματος μέσω του οποίου υπολογίζεται ένα σύνολο γραμμών μπορεί να αντιστοιχιστεί σε μια μεταβλητή. U-SQL ακολουθεί το T-SQL μεταβλητής μοτίβο ονομασίας, για παράδειγμα, **@searchlog** στη δέσμη ενεργειών.
    Σημείωση ανάθεσης χωρίς αναγκαστική εκτέλεση. Απλώς ονόματα παράστασης και σας δίνει τη δυνατότητα να συσσώρευση πιο σύνθετες παραστάσεις.
- **ΕΞΑΓΩΓΉ** σας δίνει τη δυνατότητα να ορίσετε ένα σχήμα στην ανάγνωση. Το σχήμα έχει καθοριστεί από ένα όνομα στήλης και C# τύπος ζεύγος ονόματος ανά στήλη. Χρησιμοποιεί ένα λεγόμενο **πρόγραμμα εξαγωγής**, για παράδειγμα, **Extractors.Tsv()** για να εξαγάγετε tsv αρχεία. Μπορείτε να αναπτύξετε προσαρμοσμένο αντιστοίχισης.
- **ΑΠΟΤΈΛΕΣΜΑ** λαμβάνει ένα σύνολο γραμμών και τοποθετεί σειριακά αυτό. Το Outputters.Csv() εξόδου ένα αρχείο διαχωρισμένο με κόμματα στην καθορισμένη θέση. Μπορείτε επίσης να αναπτύξετε προσαρμοσμένο Outputters.
- Παρατηρήστε τις δύο διαδρομές είναι σχετική διαδρομές. Μπορείτε επίσης να χρησιμοποιήσετε απόλυτες διαδρομές.  Για παράδειγμα

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Πρέπει να χρησιμοποιήσετε απόλυτη διαδρομή για να αποκτήσετε πρόσβαση στα αρχεία σε συνδεδεμένους λογαριασμούς χώρου αποθήκευσης.  Η σύνταξη για τα αρχεία που είναι αποθηκευμένα σε συνδεδεμένο λογαριασμό Azure χώρου αποθήκευσης είναι:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob κοντέινερ με αντικείμενα BLOB δημόσια ή τα δικαιώματα πρόσβασης δημόσια κοντέινερ δεν υποστηρίζονται αυτήν τη στιγμή.

## <a name="use-scalar-variables"></a>Χρήση μεταβλητών ανυσμάτων

Μπορείτε να χρησιμοποιήσετε ανυσματική μεταβλητές, καθώς και για να διευκολύνετε τη συντήρηση της δέσμης ενεργειών. Η προηγούμενη δέσμη ενεργειών U-SQL μπορεί επίσης να γράφονται με τα εξής:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Μετασχηματισμός συνόλων γραμμών

Χρησιμοποιήστε την **ΕΠΙΛΟΓΉ** για τη μετατροπή των συνόλων γραμμών:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

Ο όρος WHERE χρησιμοποιεί [C# δυαδική παράσταση](https://msdn.microsoft.com/library/6a71f45d.aspx). Μπορείτε να χρησιμοποιήσετε τη γλώσσα παράστασης C# για να εκτελέσετε τη δική σας παραστάσεις και τις συναρτήσεις. Μπορείτε ακόμα και να εκτελέσετε πιο σύνθετο φίλτρο συνδυάζοντάς τα με λογική συμπλεκτικών συνδέσμων (και) και disjunctions (ORs).

Η ακόλουθη δέσμη ενεργειών χρησιμοποιεί τη μέθοδο DateTime.Parse() και ένα συνδυασμό.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

Παρατηρήστε ότι το δεύτερο ερώτημα λειτουργεί το αποτέλεσμα του πρώτου συνόλου γραμμών και, επομένως, το αποτέλεσμα είναι μια σύνθεση των δύο φίλτρων. Μπορείτε επίσης να χρησιμοποιήσετε ξανά ένα όνομα μεταβλητής και τα ονόματα περιορίζονται lexically.

## <a name="aggregate-rowsets"></a>Συγκέντρωση συνόλων γραμμών

U-SQL σάς παρέχει το οικείο **ORDER BY**, **ΟΜΑΔΟΠΟΊΗΣΗ κατά** και συναθροίσεων.

Το παρακάτω ερώτημα βρίσκει τη συνολική διάρκεια ανά περιοχή και στη συνέχεια εξάγει επάνω 5 διάρκειες με τη σειρά.

U-SQL συνόλων γραμμών δεν διατηρούν τη σειρά τους για το επόμενο ερώτημα. Επομένως, για να ταξινομήσετε το αποτέλεσμα, πρέπει να προσθέσετε ORDER BY στη δήλωση ΕΞΌΔΟΥ, όπως φαίνεται παρακάτω:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

U-SQL όρο ORDER BY έχει να συνδυάζονται με τον όρο ΛΉΨΗ σε μια ΕΠΙΛΟΓΉ παράσταση.

Όρος U SQL ΑΝΤΙΜΕΤΩΠΊΖΕΤΕ μπορεί να χρησιμοποιηθεί για να περιορίσετε τα αποτελέσματα σε ομάδες που ικανοποιούν τη συνθήκη HAVING:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Συμμετοχή σε δεδομένα

U-SQL παρέχει κοινές συμμετοχή τελεστές, όπως ΕΣΩΤΕΡΙΚΌΣ ΣΎΝΔΕΣΜΟΣ, ΔΕΞΙΆ/ΑΡΙΣΤΕΡΆ/ΠΛΉΡΟΥΣ ΕΞΩΤΕΡΙΚΟΎ ΣΥΝΔΈΣΜΟΥ, συμμετοχή σε ΗΜΙΔΙΑΦΑΝΈΣ, για να συμμετάσχετε σε όχι μόνο πίνακες, αλλά οποιαδήποτε σύνολα γραμμών (ακόμα και εκείνα που παράγονται από αρχεία).

Η ακόλουθη δέσμη ενεργειών ενώνει τα searchlog με ένα αρχείο καταγραφής εντύπωση διαφήμιση και παρέχει μας τις διαφημίσεις για τη συμβολοσειρά ερωτήματος για μια δεδομένη ημερομηνία.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


U-SQL υποστηρίζει μόνο τη σύνταξη συμβατή με συμμετοχή ANSI: Rowset1 ΣΥΝΔΈΣΜΟΥ Rowset2 ON κατηγόρημα. Η παλιά σύνταξη από Rowset1, όπου Rowset2 κατηγόρημα δεν υποστηρίζεται.
Το κατηγόρημα σε ένα ΣΎΝΔΕΣΜΟ πρέπει να είναι ένας σύνδεσμος ισότητας και χωρίς παράσταση. Εάν θέλετε να χρησιμοποιήσετε μια παράσταση, προσθέσετε σε ένα προηγούμενο σύνολο γραμμών όρο SELECT. Εάν θέλετε να κάνετε διαφορετική σύγκριση, μπορείτε να το μετακινήσετε σε μια πρόταση WHERE.


## <a name="create-databases-table-valued-functions-views-and-tables"></a>Δημιουργήστε βάσεις δεδομένων, συναρτήσεις με πίνακα τιμών, προβολών και πίνακες

U-SQL σάς επιτρέπει να χρησιμοποιήσετε δεδομένα στο περιβάλλον από μια βάση δεδομένων και το σχήμα. Επομένως, δεν χρειάζεται να πάντα ανάγνωση από ή εγγραφή σε αρχεία.

Κάθε δέσμη ενεργειών U-SQL εκτελείται με προεπιλεγμένη βάση δεδομένων (κύρια) και προεπιλεγμένη διάταξη (DBO) ως το προεπιλεγμένο περιβάλλον. Μπορείτε να δημιουργήσετε τη δική σας βάση δεδομένων ή/και του σχήματος. Για να αλλάξετε το περιβάλλον, χρησιμοποιήστε την πρόταση **ΧΡΗΣΙΜΟΠΟΙΉΣΕΤΕ** για να αλλάξετε το περιβάλλον.


### <a name="create-a-table-valued-function-tvf"></a>Δημιουργήστε μια συνάρτηση με πίνακα τιμών (TVF)

Στην προηγούμενη δέσμη ενεργειών U-SQL, που επαναλαμβάνεται, χρησιμοποιώντας ΕΞΑΓΆΓΕΤΕ ανάγνωσης από το ίδιο αρχείο προέλευσης. Συνάρτηση με πίνακα τιμών U-SQL σάς επιτρέπει να συμπύκνωση τα δεδομένα για μελλοντική χρήση.   

Η ακόλουθη δέσμη ενεργειών δημιουργεί μια TVF που ονομάζεται *Searchlog()* στην προεπιλεγμένη βάση δεδομένων και σχήμα:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

Η ακόλουθη δέσμη ενεργειών δείχνει πώς μπορείτε να χρησιμοποιήσετε το TVF που ορίζονται από το προηγούμενο δέσμης ενεργειών:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Δημιουργία προβολών

Εάν έχετε μόνο μία παράσταση ερωτήματος που θέλετε να συνοπτική και δεν θέλετε να ρυθμίσετε τις παραμέτρους του, μπορείτε να δημιουργήσετε μια προβολή αντί για μια συνάρτηση με πίνακα τιμών.

Η ακόλουθη δέσμη ενεργειών δημιουργεί μια προβολή που ονομάζεται *SearchlogView* στην προεπιλεγμένη βάση δεδομένων και σχήμα:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Η ακόλουθη δέσμη ενεργειών επιδεικνύει χρησιμοποιώντας το καθορισμένο προβολή:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Δημιουργία πινάκων

Παρόμοια με πίνακα σχεσιακή βάση δεδομένων, U-SQL σάς επιτρέπει να δημιουργήσετε έναν πίνακα με μια προκαθορισμένη διάταξη ή να δημιουργήσετε έναν πίνακα και να υπολογίσει το σχήμα από το ερώτημα που συμπληρώνει τον πίνακα (γνωστό και ως ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ ΩΣ ΕΠΙΛΟΓΉ ή CTAS).

Η ακόλουθη δέσμη ενεργειών δημιουργήσετε μια βάση δεδομένων και τους δύο πίνακες:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Πίνακες ερωτημάτων

Μπορείτε να υποβάλετε ερώτημα τους πίνακες (που δημιουργήθηκε σε προηγούμενη δέσμης ενεργειών) με τον ίδιο τρόπο ως ερώτημα που επάνω από τα αρχεία δεδομένων. Αντί να δημιουργήσετε ένα σύνολο γραμμών με ΕΞΑΓΩΓΉ, μπορείτε τώρα απλώς να ανατρέξετε στο όνομα του πίνακα.

Η δέσμη ενεργειών μετασχηματισμού που έχετε χρησιμοποιήσει ήδη τροποποιείται για να διαβάσετε από τους πίνακες:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Σημειώστε ότι τη συγκεκριμένη στιγμή δεν μπορείτε να εκτελέσετε μια ΕΠΙΛΟΓΉ σε έναν πίνακα στο ίδιο δέσμης ενεργειών ως δέσμης ενεργειών όπου δημιουργείτε αυτόν τον πίνακα.


##<a name="conclusion"></a>Ολοκλήρωση

Τι είναι που καλύπτονται από το πρόγραμμα εκμάθησης είναι μόνο ένα μικρό τμήμα U-SQL. Λόγω της εμβέλειας αυτό το πρόγραμμα εκμάθησης, το δεν είναι δυνατό να καλύψετε όλα τα στοιχεία, όπως:

- Χρήση CROSS ΕΦΑΡΜΟΓΉ αποσυσκευασία τμήματα του συμβολοσειρές, πίνακες και οι χάρτες σε γραμμές.
- Λειτουργία διαμερίσματα σύνολα δεδομένων (αρχείο συνόλων και διαμερίσματα πίνακες).
- Αναπτύξτε τελεστές ορίζονται από το χρήστη όπως προγράμματα εξαγωγής, outputters, επεξεργαστές, συλλέκτες που ορίζονται από το χρήστη στο C#.
- Χρησιμοποιήστε συναρτήσεις Άνοιγμα παραθύρου U-SQL.
- Διαχείριση κώδικα U SQL με προβολές, συναρτήσεις με πίνακα τιμών και αποθηκευμένες διαδικασίες.
- Εκτέλεση αυθαίρετου προσαρμοσμένου κώδικα στην σας κόμβους επεξεργασίας.
- Σύνδεση με βάσεις δεδομένων SQL Azure και federate ερωτήματα σε αυτά και τα δεδομένα σας U-SQL και Azure λίμνης δεδομένων.

## <a name="see-also"></a>Δείτε επίσης

- [Επισκόπηση της ανάλυσης λίμνης δεδομένων Microsoft Azure](data-lake-analytics-overview.md)
- [Ανάπτυξη δεσμών ενεργειών U-SQL χρησιμοποιώντας εργαλεία λίμνης δεδομένων για το Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Χρήση συναρτήσεων παραθύρου U-SQL για τα έργα Azure δεδομένων λίμνης ανάλυσης](data-lake-analytics-use-window-functions.md)
- [Παρακολούθηση και αντιμετώπιση προβλημάτων του Azure δεδομένων λίμνης ανάλυσης εργασιών με πύλη Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Επιτρέψτε μας γνωρίζετε τη γνώμη σας

- [Υποβάλετε μια αίτηση δυνατότητα](http://aka.ms/adlafeedback)
- [Λήψη Βοήθειας στα φόρουμ του](http://aka.ms/adlaforums)
- [Παροχή σχολίων σε U-SQL](http://aka.ms/usqldiscuss)
