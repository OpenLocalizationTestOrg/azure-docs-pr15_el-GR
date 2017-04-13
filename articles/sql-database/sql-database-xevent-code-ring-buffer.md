<properties 
    pageTitle="Κωδικός Buffer δακτυλίου XEvent για βάση δεδομένων SQL | Microsoft Azure" 
    description="Παρέχει ένα δείγμα κώδικα Transact-SQL που έχει γίνει εύκολη και γρήγορη με τη χρήση του στόχου Buffer δακτυλίου, σε βάση δεδομένων SQL Azure." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Κλήση Buffer προορισμού κώδικα για εκτεταμένο συμβάντα σε βάση δεδομένων SQL

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Θέλετε ένα δείγμα ολοκλήρωσης κώδικα για ο ευκολότερος τρόπος γρήγορη καταγραφή και αναφορά πληροφορίες για ένα εκτεταμένο συμβάν κατά τη διάρκεια μιας δοκιμής. Ευκολότερος προορισμού για τα δεδομένα εκτεταμένο συμβάν είναι [Buffer δακτυλίου προορισμού](http://msdn.microsoft.com/library/ff878182.aspx).


Αυτό το θέμα παρουσιάζει ένα δείγμα κώδικα Transact-SQL που:


1. Δημιουργεί έναν πίνακα με τα δεδομένα για μια επίδειξη με.

2. Δημιουργεί μια περίοδο λειτουργίας για ένα υπάρχον συμβάν εκτεταμένο, δηλαδή **sqlserver.sql_statement_starting**.
    - Το συμβάν περιορίζεται σε προτάσεις SQL που περιέχει μια συγκεκριμένη συμβολοσειρά ενημέρωσης: **δήλωση ΌΠΩΣ "% ΕΝΗΜΈΡΩΣΗ tabEmployee %"**.
    - Επιλέγει για να στείλετε το αποτέλεσμα του συμβάντος σε έναν προορισμό του τύπου Buffer δακτυλίου, δηλαδή **package0.ring_buffer**.

3. Ξεκινά η περίοδος λειτουργίας συμβάντων.

4. Θέματα ορισμένες απλές προτάσεις SQL UPDATE.

5. Θέματα μια ΕΠΙΛΟΓΉ SQL για την ανάκτηση συμβάν εξόδου από το Buffer δακτυλίου.
    - συνδεδεμένα **sys.dm_xe_database_session_targets** και άλλες προβολές για τη δυναμική διαχείριση (DMVs).

6. Τερματίζει την περίοδο λειτουργίας του συμβάντος.

7. Προσθέτει τον στόχο Buffer δακτυλίου, για να αφήσετε τους πόρους.

8. Προσθέτει την περίοδο λειτουργίας συμβάντων και του πίνακα επίδειξη.


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία


- Μια λογαριασμός Azure και τη συνδρομή. Μπορείτε να εγγραφείτε για μια [δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/).


- Μπορείτε να δημιουργήσετε έναν πίνακα σε οποιαδήποτε βάση δεδομένων.
 - Προαιρετικά μπορείτε να [δημιουργήσετε μια βάση δεδομένων της επίδειξης **AdventureWorksLT** ](sql-database-get-started.md) σε λεπτά.


- SQL Server Management Studio (ssms.exe), ιδανικά την πιο πρόσφατη μηνιαία ενημέρωση έκδοση. Μπορείτε να λάβετε την πιο πρόσφατη ssms.exe από:
 - Θέμα με τίτλο [Κάντε λήψη του SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Μια απευθείας σύνδεση για τη λήψη.](http://go.microsoft.com/fwlink/?linkid=616025)


## <a name="code-sample"></a>Δείγμα κώδικα


Με πολύ μικρή τροποποίηση, το παρακάτω δείγμα κώδικα Buffer δακτυλίου μπορούν να εκτελεστούν σε βάση δεδομένων SQL Azure ή Microsoft SQL Server. Η διαφορά είναι η παρουσία του κόμβου '_database' στο όνομα του ορισμένες προβολές δυναμική διαχείριση (DMVs), χρησιμοποιείται από τον όρο FROM στο βήμα 5. Για παράδειγμα:

- sys.dm_xe**_database**_session_targets
- sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## <a name="ring-buffer-contents"></a>Περιεχόμενα Buffer δακτυλίου


Χρησιμοποιήσαμε ssms.exe για να εκτελέσετε το δείγμα κώδικα.


Για να προβάλετε τα αποτελέσματα, θα σας κάνει κλικ στο κελί κάτω από την κεφαλίδα της στήλης **target_data_XML**.

Στη συνέχεια, στο παράθυρο αποτελεσμάτων θα σας κάνει κλικ στο κελί κάτω από την κεφαλίδα της στήλης **target_data_XML**. Σε αυτό, κάντε κλικ στην επιλογή δημιουργήσατε μια άλλη καρτέλα αρχείο στο ssms.exe στο οποίο ήταν εμφανίζεται το περιεχόμενο του κελιού στο αποτέλεσμα, με τη μορφή XML.


Το αποτέλεσμα εμφανίζεται στο παρακάτω μπλοκ. Φαίνεται ότι η πλήρης, αλλά είναι απλώς δύο **<event>** στοιχεία.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Έκδοση πόρων που βρίσκονται στην κατοχή σας Buffer δακτυλίου


Όταν ολοκληρώσετε με το Buffer δακτυλίου, μπορείτε να καταργήσετε και να αφήσετε τους πόρους έκδοσης ενός **ΤΡΟΠΟΠΟΙΉΣΟΥΝ** όπως το εξής:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Τον ορισμό της περιόδου λειτουργίας των συμβάντων είναι ενημερωθεί, αλλά δεν έχει καταργηθεί. Αργότερα, μπορείτε να προσθέσετε μια άλλη παρουσία του Buffer κλήση σε περίοδο λειτουργίας σας στο συμβάν:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Περισσότερες πληροφορίες


Το κύριο θέμα για εκτεταμένο συμβάντα σε βάση δεδομένων SQL Azure είναι:


- [Ζητήματα συμβάν εκτεταμένη σε βάση δεδομένων SQL](sql-database-xevent-db-diff-from-svr.md), που παρουσιάζει ορισμένα από τα χαρακτηριστικά του εκτεταμένου συμβάντα που διαφέρουν μεταξύ της βάσης δεδομένων SQL Azure έναντι της Microsoft SQL Server.


Άλλα θέματα δείγματα κώδικα για εκτεταμένο συμβάντα διατίθενται οι ακόλουθες συνδέσεις. Ωστόσο, πρέπει να αναλάβετε τακτικά κάθε δείγμα για να δείτε εάν το δείγμα ως προορισμό Microsoft SQL Server έναντι της βάσης δεδομένων SQL Azure. Στη συνέχεια, μπορείτε να αποφασίσετε εάν απαιτούνται αλλαγές μικρής σημασίας για να εκτελέσετε το δείγμα.


- Δείγμα κώδικα για βάση δεδομένων SQL Azure: [συμβάν αρχείου προορισμού κώδικα για εκτεταμένο συμβάντα σε βάση δεδομένων SQL](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
