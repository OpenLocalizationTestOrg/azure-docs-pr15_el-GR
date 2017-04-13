<properties
    pageTitle="Επιλέξτε τις γραμμές για να μετεγκαταστήσετε χρησιμοποιώντας μια συνάρτηση φίλτρου (βάση δεδομένων παραμόρφωση) | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να επιλέξετε γραμμές για τη μετεγκατάσταση χρησιμοποιώντας μια συνάρτηση φίλτρου."
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
    ms.date="06/28/2016"
    ms.author="douglasl"/>

# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Επιλέξτε τις γραμμές για να μετεγκαταστήσετε χρησιμοποιώντας μια συνάρτηση φίλτρου (παραμόρφωση βάση δεδομένων)

Εάν αποθηκεύετε ψυχρές δεδομένα σε έναν νέο πίνακα, μπορείτε να ρυθμίσετε παραμόρφωση βάσης δεδομένων για τη μετεγκατάσταση ολόκληρο τον πίνακα. Εάν ο πίνακας περιέχει δεδομένα τόσο κρύου και ζεστού, από την άλλη πλευρά, μπορείτε να καθορίσετε μια λειτουργία φίλτρου για να επιλέξετε τις γραμμές για τη μετεγκατάσταση. Το φίλτρο κατηγόρημα είναι ένας πίνακας ενσωματωμένη\-με τιμές συνάρτηση. Αυτό το θέμα περιγράφει πώς να συντάξετε έναν πίνακα ενσωματωμένο\-με τιμές συνάρτηση για να επιλέξετε γραμμές για τη μετεγκατάσταση.

>   [AZURE.NOTE] Εάν παρέχεται μια συνάρτηση φίλτρου που εκτελεί ακατάλληλα, μετεγκατάσταση δεδομένων επίσης εκτελεί ακατάλληλα. Επέκταση βάσης δεδομένων εφαρμόζει τη λειτουργία φίλτρου στον πίνακα, χρησιμοποιώντας τον τελεστή CROSS ΕΦΑΡΜΟΓΉ.

Εάν δεν καθορίσετε μια λειτουργία φίλτρου, μετεγκατάσταση ολόκληρο τον πίνακα.

Όταν εκτελείτε τη βάση δεδομένων ενεργοποίηση για τον Οδηγό παραμόρφωση, μπορείτε να κάνετε μετεγκατάσταση έναν ολόκληρο πίνακα ή μπορείτε να καθορίσετε μια συνάρτηση απλό φίλτρο στον οδηγό. Εάν θέλετε να χρησιμοποιήσετε διαφορετικό τύπο λειτουργία φίλτρου για να επιλέξετε γραμμές για τη μετεγκατάσταση, κάντε ένα από τα εξής στοιχεία.

-   Κλείστε τον οδηγό και εκτελέστε την πρόταση ALTER TABLE για να ενεργοποιήσετε παραμόρφωση για τον πίνακα και για να καθορίσετε μια συνάρτηση φίλτρου.

-   Εκτελέστε την πρόταση ALTER TABLE για να καθορίσετε μια λειτουργία φίλτρου μετά την έξοδο από τον οδηγό.

Η πρόταση ALTER TABLE σύνταξη για να προσθέσετε μια συνάρτηση περιγράφεται παρακάτω σε αυτό το θέμα.

## <a name="basic-requirements-for-the-filter-function"></a>Βασικές απαιτήσεις για τη λειτουργία φίλτρου
Ο πίνακας ενσωματωμένη\-τιμών λειτουργία που απαιτείται για μια βάση δεδομένων παραμόρφωση κατηγόρημα φίλτρο μοιάζει με το παρακάτω παράδειγμα.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE <predicate>
```
Τις παραμέτρους για τη συνάρτηση πρέπει να είναι αναγνωριστικά για τις στήλες από τον πίνακα.

Σύνδεση σχήματος είναι απαραίτητη για να αποτρέψετε την στήλες που χρησιμοποιούνται από τη λειτουργία φίλτρου καταργηθεί ή τροποποιηθεί.

### <a name="return-value"></a>Τιμή επιστροφής
Εάν η συνάρτηση επιστρέφει ένα μη\-αποτέλεσμα κενό, η γραμμή είναι επιλέξιμο για να μετεγκαταστήσετε. Διαφορετικά \- , εάν η συνάρτηση δεν επιστρέφει ένα αποτέλεσμα \- η γραμμή δεν είναι επιλέξιμο για να μετεγκαταστήσετε.

### <a name="conditions"></a>Συνθήκες
Το &lt; *κατηγόρημα* &gt; μπορούν να περιλαμβάνουν μία συνθήκη ή πολλές συνθήκες συνδεθεί με τον λογικό τελεστή.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Κάθε συνθήκη με τη σειρά μπορούν να περιλαμβάνουν μία συνθήκη στοιχειώδεις ή πολλές στοιχειώδεις συνθήκες συνδεθεί με τον λογικό τελεστή OR.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### <a name="primitive-conditions"></a>Στοιχειώδεις συνθήκες
Μια συνθήκη στοιχειώδεις να κάνετε μία από τις παρακάτω συγκρίσεις.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Συγκρίνετε μια παράμετρος συνάρτηση μια σταθερή έκφραση. Για παράδειγμα, `@column1 < 1000`.

    Ακολουθεί ένα παράδειγμα το οποίο ελέγχει εάν η τιμή της στήλης *ημερομηνίας* είναι &lt; 1/1/2016.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Εφαρμογή τον τελεστή IS NULL ή δεν ΕΊΝΑΙ NULL σε μια συνάρτηση παράμετρο.

-   Χρησιμοποιήστε τον τελεστή IN για να συγκρίνετε μια παράμετρος συνάρτηση μιας λίστας σταθερών τιμών.

    Ακολουθεί ένα παράδειγμα το οποίο ελέγχει εάν η τιμή του ένα *αποστολής\_κατάστασης* στήλη είναι `IN (N'Completed', N'Returned', N'Cancelled')`.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

### <a name="comparison-operators"></a>Τελεστές σύγκρισης
Υποστηρίζονται τους ακόλουθους τελεστές σύγκρισης.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### <a name="constant-expressions"></a>Οι σταθερές εκφράσεις
Οι σταθερές που χρησιμοποιείτε σε μια συνάρτηση φίλτρου μπορεί να είναι οποιαδήποτε ντετερμινιστική παράσταση που μπορεί να αξιολογηθεί όταν ορίζετε τη συνάρτηση. Οι σταθερές εκφράσεις μπορούν να περιέχουν τα εξής στοιχεία.

-   Λεκτικές σταθερές. Για παράδειγμα, `N’abc’, 123`.

-   Αλγεβρικό παραστάσεις. Για παράδειγμα, `123 + 456`.

-   Ντετερμινιστική συναρτήσεις. Για παράδειγμα, `SQRT(900)`.

-   Ντετερμινιστική τις μετατροπές που χρησιμοποιούν CAST ή ΜΕΤΑΤΡΟΠΉ. Για παράδειγμα, `CONVERT(datetime, '1/1/2016', 101)`.

### <a name="other-expressions"></a>Άλλες εκφράσεις
Μπορείτε να χρησιμοποιήσετε τις BETWEEN και δεν BETWEEN τους τελεστές, εάν η συνάρτηση που προκύπτει συμφωνεί με τους κανόνες που περιγράφονται εδώ αφού έχετε αντικαταστήσει το BETWEEN και δεν ΜΕΤΑΞΎ τελεστές με το ισοδύναμο και και ή παραστάσεις.

Δεν μπορείτε να χρησιμοποιήσετε δευτερεύοντα ερωτήματα ή μη\-ντετερμινιστική συναρτήσεις όπως RAND() ή GETDATE().

## <a name="add-a-filter-function-to-a-table"></a>Προσθέστε μια λειτουργία φίλτρου σε πίνακα
Προσθήκη μια λειτουργία φίλτρου σε έναν πίνακα, εκτελώντας την πρόταση **ALTER TABLE** και που καθορίζει έναν υπάρχοντα πίνακα ενσωματωμένη\-με τιμές συνάρτηση ως τιμή του το **ΦΊΛΤΡΟ\_ΚΑΤΗΓΟΡΉΜΑΤΟΣ** παραμέτρου. Για παράδειγμα:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
) )
```
Αφού συνδέσετε τη συνάρτηση στον πίνακα ως κατηγόρημα, ισχύουν τα εξής στοιχεία.

-   Η επόμενη μετεγκατάσταση δεδομένων χρόνου που προκύπτει, μόνο τις γραμμές για την οποία η συνάρτηση επιστρέφει ένα μη\-μετεγκαθίστανται κενή τιμή.

-   Οι στήλες που χρησιμοποιούνται από τη συνάρτηση είναι συνδεδεμένο σχήμα. Δεν μπορείτε να αλλάξετε αυτές τις στήλες με την προϋπόθεση ότι πίνακα χρησιμοποιεί τη συνάρτηση ως κατηγόρημα το φίλτρο.

Δεν μπορείτε να αποθέσετε τον πίνακα ενσωματωμένη\-με τιμές συνάρτηση εφόσον πίνακα χρησιμοποιεί τη συνάρτηση ως κατηγόρημα το φίλτρο.

>   [AZURE.NOTE] Για να βελτιώσετε την απόδοση της συνάρτησης φίλτρου, δημιουργήστε ένα ευρετήριο τις στήλες που χρησιμοποιούνται από τη συνάρτηση.

### <a name="passing-column-names-to-the-filter-function"></a>Ονόματα στηλών διέλευση στη συνάρτηση φίλτρου
Όταν εκχωρείτε μια λειτουργία φίλτρου σε πίνακα, καθορίστε τα ονόματα των στηλών που του μεταβιβάστηκε η συνάρτηση φίλτρου με τμήμα ένα όνομα. Εάν καθορίσετε ένα όνομα τριών τμημάτων όταν δίνετε της στήλης ονόματα, οι επόμενες ερωτήματα σε σχέση με την παραμόρφωση\-με δυνατότητα πίνακα θα αποτύχει.

Για παράδειγμα, εάν καθορίσετε ένα όνομα στήλης τριών τμημάτων, όπως φαίνεται στο παρακάτω παράδειγμα, η πρόταση θα εκτελεστεί με επιτυχία, αλλά οι επόμενες ερωτήματα από τον πίνακα θα αποτύχει.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Αντί για αυτό, καθορίστε τη συνάρτηση φίλτρου με το όνομα μιας στήλης ένα τμήμα, όπως φαίνεται στο παρακάτω παράδειγμα.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="addafterwiz"></a>Για να προσθέσετε μια συνάρτηση φίλτρου μετά την εκτέλεση του οδηγού  

Εάν θέλετε να χρησιμοποιήσετε μια συνάρτηση που δεν μπορείτε να δημιουργήσετε στον οδηγό **Ενεργοποίηση της βάσης δεδομένων για παραμόρφωση** , μπορείτε να εκτελέσετε την πρόταση ALTER TABLE για να καθορίσετε μια συνάρτηση μετά την έξοδο από τον οδηγό. Πριν να εφαρμόσετε μια συνάρτηση, ωστόσο, πρέπει να διακόψετε τη μετεγκατάσταση δεδομένων που βρίσκεται ήδη σε εξέλιξη και επαναφέρω μετεγκαταστάθηκαν δεδομένων. (Για περισσότερες πληροφορίες σχετικά με το γιατί αυτό είναι απαραίτητο, ανατρέξτε στο θέμα [αντικαταστήσετε μια υπάρχουσα συνάρτηση φίλτρου](#replacePredicate).  

1. Αντιστροφή της κατεύθυνσης μετεγκατάστασης και μεταφορά πίσω ήδη μετεγκατάσταση των δεδομένων. Δεν μπορείτε να ακυρώσετε αυτήν τη λειτουργία μετά την εκκίνηση. Μπορείτε επίσης να υπάρξουν κόστος στη Azure για μεταφορές εξερχομένων δεδομένων \(εξόδου\). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς Azure τιμολόγησης λειτουργεί](https://azure.microsoft.com/pricing/details/data-transfers/).  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Περιμένετε για μετεγκατάσταση για να ολοκληρώσετε. Μπορείτε να ελέγξετε την κατάσταση στην **Εποπτεία παραμόρφωση βάσης δεδομένων** από SQL Server Management Studio, ή μπορείτε να υποβάλετε ερώτημα στην προβολή **sys.dm_db_rda_migration_status** . Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [οθόνη και αντιμετώπιση προβλημάτων μετεγκατάστασης δεδομένων](sql-server-stretch-database-monitor.md) ή [sys.dm_db_rda_migration_status](https://msdn.microsoft.com/library/dn935017.aspx).  

3. Δημιουργήστε τη συνάρτηση φίλτρο που θέλετε να εφαρμόσετε στον πίνακα.  

4. Προσθέστε τη συνάρτηση στον πίνακα και κάντε επανεκκίνηση μετεγκατάστασης δεδομένων στο Azure.  

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## <a name="filter-rows-by-date"></a>Φιλτράρισμα γραμμών κατά ημερομηνία
Το παρακάτω παράδειγμα μετεγκαθιστά γραμμών όπου η στήλη **ημερομηνία** περιέχει μια τιμή νωρίτερα από 1 Ιανουαρίου 2016.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## <a name="filter-rows-by-the-value-in-a-status-column"></a>Φιλτράρισμα γραμμών κατά την τιμή σε μια στήλη κατάστασης
Το παρακάτω παράδειγμα μετεγκαθιστά γραμμών όπου στη στήλη **κατάσταση** περιέχει μία από τις καθορισμένες τιμές.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## <a name="filter-rows-by-using-a-sliding-window"></a>Γραμμές φίλτρου, χρησιμοποιώντας ένα κυλιόμενο παράθυρο
Για να φιλτράρετε γραμμών, χρησιμοποιώντας ένα κυλιόμενο παράθυρο, λάβετε υπόψη τις ακόλουθες απαιτήσεις για τη λειτουργία φίλτρου.

-   Η συνάρτηση πρέπει να είναι ντετερμινιστική. Επομένως, δεν μπορείτε να δημιουργήσετε μια συνάρτηση που αυτόματα επαναλαμβάνει τον υπολογισμό του κυλιόμενο παράθυρο ως αυτού του χρονικού διαστήματος.

-   Η συνάρτηση χρησιμοποιεί σύνδεση σχήματος. Επομένως, δεν μπορείτε απλώς να ενημερώσετε τη συνάρτηση "στη θέση" καθημερινά καλώντας την **ΤΡΟΠΟΠΟΊΗΣΗ ΣΥΝΆΡΤΗΣΗ** για να μετακινήσετε το κυλιόμενο παράθυρο.

Ξεκινήστε με μια συνάρτηση φίλτρου, όπως το παρακάτω παράδειγμα, η οποία μετεγκαθιστά γραμμών όπου η στήλη **systemEndTime** περιέχει μια τιμή νωρίτερα από 1 Ιανουαρίου 2016.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

Εφαρμόστε τη λειτουργία φίλτρου στον πίνακα.

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Όταν θέλετε να ενημερώσετε το κυλιόμενο παράθυρο, κάντε τα εξής στοιχεία.

1.  Δημιουργήστε μια νέα συνάρτηση που καθορίζει το νέο παράθυρο κυλιόμενο. Το παρακάτω παράδειγμα επιλέγει ημερομηνίες νωρίτερα από 2 Ιανουαρίου 2016, αντί για 1 Ιανουαρίου 2016.

2.  Αντικαταστήστε την προηγούμενη λειτουργία φίλτρου με τη νέα από κλήσης **Πρόταση ALTER TABLE**, όπως φαίνεται στο παρακάτω παράδειγμα.

3. Προαιρετικά, αποθέστε την προηγούμενη λειτουργία φίλτρου που χρησιμοποιείτε δεν είναι πλέον κατά την κλήση της **ΣΥΝΆΡΤΗΣΗΣ ΑΠΌΘΕΣΗ**. (Αυτό το βήμα δεν εμφανίζεται στο παράδειγμα.)

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as the filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## <a name="more-examples-of-valid-filter-functions"></a>Περισσότερα παραδείγματα συναρτήσεων έγκυρο φίλτρο

-   Το παρακάτω παράδειγμα συνδυάζει δύο συνθήκες στοιχειώδεις χρησιμοποιώντας τον τελεστή λογική.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Το παρακάτω παράδειγμα χρησιμοποιεί πολλές συνθήκες και ντετερμινιστική μετατροπή με ΜΕΤΑΤΡΟΠΉ.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   Το παρακάτω παράδειγμα χρησιμοποιεί μαθηματικούς τελεστές και συναρτήσεις.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   Το παρακάτω παράδειγμα χρησιμοποιεί το BETWEEN και δεν BETWEEN τελεστές. Αυτήν τη χρήση είναι έγκυρο, επειδή η συνάρτηση που προκύπτει συμφωνεί με τους κανόνες που περιγράφονται εδώ αφού έχετε αντικαταστήσει το BETWEEN και δεν ΜΕΤΑΞΎ τελεστές με το ισοδύναμο και και ή παραστάσεις.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 BETWEEN 0 AND 100
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    Η προηγούμενη συνάρτηση είναι ισοδύναμο με την ακόλουθη συνάρτηση αφού έχετε αντικαταστήσει τις BETWEEN και δεν BETWEEN τους τελεστές με το ισοδύναμο και και ή παραστάσεις.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## <a name="examples-of-filter-functions-that-arent-valid"></a>Παραδείγματα συναρτήσεων φίλτρου που δεν είναι έγκυρη

-   Η ακόλουθη συνάρτηση δεν είναι έγκυρο επειδή περιέχει ένα μη\-ντετερμινιστική μετατροπής.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   Η ακόλουθη συνάρτηση δεν είναι έγκυρο επειδή περιέχει ένα μη\-κλήσης συνάρτησης ντετερμινιστική.

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   Η ακόλουθη συνάρτηση δεν είναι έγκυρο, επειδή περιέχει ένα δευτερεύον ερώτημα.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   Οι παρακάτω συναρτήσεις δεν είναι έγκυρη επειδή παραστάσεων που χρησιμοποιούν τελεστές αλγεβρικό ή ενσωματωμένη\-σε συναρτήσεις πρέπει να είναι μια σταθερά όταν ορίζετε τη συνάρτηση. Δεν μπορείτε να συμπεριλάβετε αναφορές σε στήλες σε αλγεβρικό εκφράσεις ή συνάρτηση κλήσεις.

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE SQRT(@column1) = 30
    GO
    ```

-   Η ακόλουθη συνάρτηση δεν είναι έγκυρο, επειδή παραβιάζει τους κανόνες που περιγράφονται εδώ αφού έχετε αντικαταστήσει τον τελεστή BETWEEN στην ισοδύναμη παράσταση και.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    Η προηγούμενη συνάρτηση είναι ισοδύναμο με την ακόλουθη συνάρτηση μετά την αντικατάσταση τον τελεστή BETWEEN στην ισοδύναμη παράσταση και. Αυτή η συνάρτηση δεν είναι έγκυρο, επειδή στοιχειώδεις συνθήκες να χρησιμοποιήσετε μόνο τον λογικό τελεστή OR.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## <a name="how-stretch-database-applies-the-filter-function"></a>Πώς βάσης δεδομένων παραμόρφωση ισχύει η λειτουργία φίλτρου
Επέκταση βάσης δεδομένων εφαρμόζει τη λειτουργία φίλτρου στον πίνακα και καθορίζει κατάλληλη γραμμών, χρησιμοποιώντας τον τελεστή CROSS ΕΦΑΡΜΟΓΉ. Για παράδειγμα:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Εάν η συνάρτηση επιστρέφει ένα μη\-κενή αποτέλεσμα για τη γραμμή, η γραμμή είναι επιλέξιμο για να μετεγκαταστήσετε.

## <a name="replacePredicate"></a>Αντικατάσταση ενός υπάρχοντος λειτουργία φίλτρου
Μπορείτε να αντικαταστήσετε μια συνάρτηση προηγουμένως καθορισμένο φίλτρο εκτελείται η πρόταση **ALTER TABLE** και καθορίζοντας μια νέα τιμή για το **ΦΊΛΤΡΟ\_ΚΑΤΗΓΟΡΉΜΑΤΟΣ** παραμέτρου. Για παράδειγμα:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
```
Το νέο πίνακα ενσωματωμένη\-τιμών συνάρτηση περιλαμβάνει τις ακόλουθες απαιτήσεις.

-   Τη νέα συνάρτηση, πρέπει να είναι λιγότερο περιοριστικό από τη λειτουργία "Προηγούμενο".

-   Όλες οι τελεστές που υπήρχαν στη συνάρτηση παλιά πρέπει να υπάρχει στη νέα συνάρτηση.

-   Τη νέα συνάρτηση δεν είναι δυνατό να περιέχουν τελεστές που δεν υπάρχουν στο παλιό τη συνάρτηση.

-   Δεν είναι δυνατό να αλλάξετε τη σειρά των ορισμάτων τελεστή.

-   Μόνο οι σταθερές τιμές που αποτελούν μέρος μιας `<, <=, >, >=` σύγκρισης μπορεί να αλλάξει με τον τρόπο που το κάνει τη συνάρτηση λιγότερο περιοριστικό.

### <a name="example-of-a-valid-replacement"></a>Παράδειγμα μια έγκυρη αντικατάστασης
Ας υποθέσουμε ότι η ακόλουθη συνάρτηση είναι το τρέχον κατηγόρημα φίλτρο.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Η ακόλουθη συνάρτηση είναι έγκυρη αντικαθιστά επειδή η σταθερά νέα ημερομηνία (η οποία καθορίζει μια ημερομηνία αργότερα αποκοπής) κάνει τη συνάρτηση λιγότερο περιοριστικό.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
            AND (@column2 < -50 OR @column2 > 50)
GO
```

### <a name="examples-of-replacements-that-arent-valid"></a>Παραδείγματα αντικατάστασης ελέγχου που δεν είναι έγκυρη
Η ακόλουθη συνάρτηση δεν αντικαθιστά έγκυρη, επειδή η σταθερά νέα ημερομηνία (η οποία καθορίζει μια ημερομηνία νωρίτερα αποκοπής) δεν κάνει τη συνάρτηση λιγότερο περιοριστικό.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Η ακόλουθη συνάρτηση δεν είναι έγκυρη αντικαθιστά επειδή έναν από τους τελεστές σύγκρισης έχει καταργηθεί.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -50)
GO
```
Η ακόλουθη συνάρτηση δεν αντικαθιστά έγκυρη επειδή έχει προστεθεί μια νέα συνθήκη με τον λογικό τελεστή.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
            AND (@column2 <> 0)
GO
```

## <a name="remove-a-filter-function-from-a-table"></a>Κατάργηση μιας συνάρτησης φίλτρου από πίνακα
Για τη μετεγκατάσταση ολόκληρου του πίνακα αντί για επιλεγμένες γραμμές, καταργήστε την υπάρχουσα συνάρτηση με τη ρύθμιση **ΦΊΛΤΡΟ\_ΚΑΤΗΓΟΡΉΜΑΤΟΣ** σε null. Για παράδειγμα:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = NULL,
    MIGRATION_STATE = <desired_migration_state>
) )
```
Αφού καταργήσετε τη λειτουργία φίλτρου, όλες οι γραμμές του πίνακα είναι κατάλληλο για τη μετεγκατάσταση. Ως αποτέλεσμα, δεν μπορείτε να καθορίσετε μια λειτουργία φίλτρου για αργότερα στον ίδιο πίνακα, εκτός εάν μπορείτε να επιστρέψετε όλα τα απομακρυσμένα δεδομένα για τον πίνακα από το Azure πρώτα. Αυτός ο περιορισμός υπάρχει για να αποφύγετε την περίπτωση όπου γραμμές που δεν είναι διαθέσιμες για μετεγκατάσταση όταν δίνετε μια νέα λειτουργία φίλτρου ήδη έχουν μετεγκατασταθεί σε Azure.

## <a name="check-the-filter-function-applied-to-a-table"></a>Ελέγξτε τη συνάρτηση φίλτρο που έχουν εφαρμοστεί σε έναν πίνακα
Για να ελέγξετε τη λειτουργία φίλτρου εφαρμοστεί σε έναν πίνακα, ανοίξτε την προβολή καταλόγου **sys.remote\_δεδομένων\_αρχειοθέτησης\_πίνακες** και ελέγξτε την τιμή της το **φίλτρο\_κατηγόρημα** στήλης. Εάν η τιμή είναι μηδέν, ολόκληρο τον πίνακα είναι κατάλληλη για αρχειοθέτηση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="security-notes-for-filter-functions"></a>Σημειώσεις ασφαλείας για συναρτήσεις φίλτρου  
Ένα έχει παραβιαστεί λογαριασμό με δικαιώματα db_owner να κάνετε τα εξής στοιχεία.  

-   Δημιουργία και εφαρμογή μια συνάρτηση με πίνακα τιμών που καταναλώνει μεγάλες ποσότητες πόρων διακομιστή ή χρειάζεται να περιμένει για μεγάλο χρονικό που προκύπτει άρνηση της υπηρεσίας.  

-   Δημιουργία και εφαρμογή μια συνάρτηση με πίνακα τιμών που δίνει τη δυνατότητα να υπολογίσει το περιεχόμενο του πίνακα για τον οποίο ο χρήστης έχει έχουν ρητά δεν επιτρέπεται η πρόσβαση για ανάγνωση.  

## <a name="see-also"></a>Δείτε επίσης

[ΤΡΟΠΟΠΟΊΗΣΗ του ΠΊΝΑΚΑ (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
