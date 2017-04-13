<properties
   pageTitle="Στοιχεία γλώσσας SQL αποθήκη δεδομένων Transact-SQL | Microsoft Azure"
   description="Λίστα με συνδέσεις σε περιεχόμενο αναφοράς για τα στοιχεία γλώσσας Transact-SQL που χρησιμοποιούνται για την αποθήκη δεδομένων του SQL."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/08/2016"
   ms.author="barbkess;sonyama;kevin"/>

# <a name="language-elements"></a>Στοιχεία της γλώσσας

## <a name="core-elements"></a>Βασικά στοιχεία

- [σύνταξη συμβάσεις](https://msdn.microsoft.com/library/ms177563.aspx)
- [κανόνες ονοματοθεσίας αντικειμένου](https://msdn.microsoft.com/library/ms175874.aspx)
- [δεσμευμένες λέξεις-κλειδιά](https://msdn.microsoft.com/library/ms189822.aspx)
- [ταξινομήσεις](https://msdn.microsoft.com/library/ff848763.aspx)
- [σχόλια](https://msdn.microsoft.com/library/ms181627.aspx)
- [σταθερές](https://msdn.microsoft.com/library/ms179899.aspx)
- [τύποι δεδομένων](https://msdn.microsoft.com/library/ms187752.aspx)
- [ΕΚΤΈΛΕΣΗ](https://msdn.microsoft.com/library/ms188332.aspx)
- [παραστάσεις](https://msdn.microsoft.com/library/ms190286.aspx)
- [ΤΕΡΜΑΤΙΣΜΌΣ](https://msdn.microsoft.com/library/ms173730.aspx)
- [Λύση: η ιδιότητα ΤΑΥΤΌΤΗΤΑΣ](https://msdn.microsoft.com/library/ms186775.aspx)
- [ΕΚΤΎΠΩΣΗ](https://msdn.microsoft.com/library/ms176047.aspx)
- [ΧΡΉΣΗ](https://msdn.microsoft.com/library/ms188366.aspx)

## <a name="batches-control-of-flow-and-variables"></a>Δέσμες, το στοιχείο ελέγχου ροής και μεταβλητές

- [ΞΕΚΙΝΉΣΤΕ... ΤΈΛΟΣ](https://msdn.microsoft.com/library/ms190487.aspx)
- [ΑΛΛΑΓΉ](https://msdn.microsoft.com/library/ms181271.aspx)
- [ΔΉΛΩΣΗ@local_variable](https://msdn.microsoft.com/library/ms188927.aspx)
- [IF... ΆΛΛΟ](https://msdn.microsoft.com/library/ms182717.aspx)
- [RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx)
- [SET@local_variable](https://msdn.microsoft.com/library/ms189484.aspx)
- [ΕΜΦΑΝΊΣΟΥΝ](https://msdn.microsoft.com/library/ee677615.aspx)
- [ΔΟΚΙΜΆΣΤΕ ΝΑ... ΕΝΗΜΕΡΩΘΕΊΤΕ](https://msdn.microsoft.com/library/ms175976.aspx)
- [ΧΡΌΝΟΣ](https://msdn.microsoft.com/library/ms178642.aspx)

## <a name="operators"></a>Τελεστές
- [+ (Προσθήκη)](https://msdn.microsoft.com/library/ms178565.aspx)
- [+ (Συμβολοσειρά συνένωση)](https://msdn.microsoft.com/library/ms177561.aspx)
- [-(Αρνητικό)](https://msdn.microsoft.com/library/ms189480.aspx)
- [-(Αφαίρεση)](https://msdn.microsoft.com/library/ms189518.aspx)
- [* (Πολλαπλασιασμός)](https://msdn.microsoft.com/library/ms176019.aspx)
- [/ (Διαίρεση)](https://msdn.microsoft.com/library/ms175009.aspx)
- [Λειτουργική μονάδα](https://msdn.microsoft.com/library/ms190279.aspx)

## <a name="wildcard-characters-to-match"></a>Χαρακτήρες μπαλαντέρ, ώστε να ταιριάζει με
- [= (Ισούται με)](https://msdn.microsoft.com/library/ms175118.aspx)
- [> (Μεγαλύτερο από)](https://msdn.microsoft.com/library/ms178590.aspx)
- [< (Μικρότερο από)](https://msdn.microsoft.com/library/ms179873.aspx)
- [> = (εξαιρετικά από ή ίσο με)](https://msdn.microsoft.com/library/ms181567.aspx)
- [< = (μικρότερο από ή ίσο με)](https://msdn.microsoft.com/library/ms174978.aspx)
- [<> (Δεν ισούται με)](https://msdn.microsoft.com/library/ms176020.aspx)
- [! = (Δεν ισούται με)](https://msdn.microsoft.com/library/ms190296.aspx)
- [ΚΑΙ](https://msdn.microsoft.com/library/ms188372.aspx)
- [ΜΕΤΑΞΎ](https://msdn.microsoft.com/library/ms187922.aspx)
- [ΥΠΆΡΧΕΙ](https://msdn.microsoft.com/library/ms188336.aspx)
- [ΣΕ](https://msdn.microsoft.com/library/ms177682.aspx)
- [[ΔΕΝ] ΕΊΝΑΙ NULL](https://msdn.microsoft.com/library/ms188795.aspx)
- [ΌΠΩΣ](https://msdn.microsoft.com/library/ms179859.aspx)
- [ΔΕΝ](https://msdn.microsoft.com/library/ms189455.aspx)
- [OR](https://msdn.microsoft.com/library/ms188361.aspx)

### <a name="bitwise-operators"></a>Πράξης τελεστές

- [& (Πράξης AND)](https://msdn.microsoft.com/library/ms174965.aspx)
- [| (Πράξης OR)](https://msdn.microsoft.com/library/ms186714.aspx)
- [^ (Πράξης αποκλειστικό OR)](https://msdn.microsoft.com/library/ms190277.aspx)
- [~ (Δεν είναι πράξης)](https://msdn.microsoft.com/library/ms173468.aspx)
- [^ = (Πράξης αποκλειστική ή ισούται με)](https://msdn.microsoft.com/library/cc627413.aspx)
- [| = (Ισούται με το αποτέλεσμα πράξης OR)](https://msdn.microsoft.com/library/cc627409.aspx)
- [& = (ισούται με πράξης AND)](https://msdn.microsoft.com/library/cc627427.aspx)

## <a name="functions"></a>Συναρτήσεις

- [@@DATEFIRST](https://msdn.microsoft.com/library/ms187766.aspx)
- [@@ERROR](https://msdn.microsoft.com/library/ms188790.aspx)
- [@@LANGUAGE](https://msdn.microsoft.com/library/ms177557.aspx)
- [@@SPID](https://msdn.microsoft.com/library/ms189535.aspx)
- [@@TRANCOUNT](https://msdn.microsoft.com/library/ms187967.aspx)
- [@@VERSION](https://msdn.microsoft.com/library/ms177512.aspx)
- [ABS](https://msdn.microsoft.com/library/ms189800.aspx)
- [ACOS](https://msdn.microsoft.com/library/ms178627.aspx)
- [ASCII](https://msdn.microsoft.com/library/ms177545.aspx)
- [ASIN](https://msdn.microsoft.com/library/ms181581.aspx)
- [ATAN](https://msdn.microsoft.com/library/ms181746.aspx)
- [ATN2](https://msdn.microsoft.com/library/ms173854.aspx)
- [BINARY_CHECKSUM](https://msdn.microsoft.com/library/ms173784.aspx)
- [ΠΕΖΏΝ-ΚΕΦΑΛΑΊΩΝ](https://msdn.microsoft.com/library/ms181765.aspx)
- [CAST και ΜΕΤΑΤΡΟΠΉ](https://msdn.microsoft.com/library/ms187928.aspx)
- [ΑΝΏΤΑΤΟ ΌΡΙΟ](https://msdn.microsoft.com/library/ms189818.aspx)
- [ΚΑΤΆ ΈΝΑ ΧΑΡΑΚΤΉΡΑ](https://msdn.microsoft.com/library/ms187323.aspx)
- [CHARINDEX](https://msdn.microsoft.com/library/ms186323.aspx)
- [ΆΘΡΟΙΣΜΑ ΕΛΈΓΧΟΥ](https://msdn.microsoft.com/library/ms189788.aspx)
- [ΣΥΝΈΝΩΣΗ](https://msdn.microsoft.com/library/ms190349.aspx)
- [COL_NAME](https://msdn.microsoft.com/library/ms174974.aspx)
- [COLLATIONPROPERTY](https://msdn.microsoft.com/library/ms190305.aspx)
- [CONCAT](https://msdn.microsoft.com/library/hh231515.aspx)
- [COS](https://msdn.microsoft.com/library/ms188919.aspx)
- [COT](https://msdn.microsoft.com/library/ms188921.aspx)
- [ΚΑΤΑΜΈΤΡΗΣΗ](https://msdn.microsoft.com/library/ms175997.aspx)
- [COUNT_BIG](https://msdn.microsoft.com/library/ms190317.aspx)
- [CUME_DIST](https://msdn.microsoft.com/library/hh231078.aspx)
- [CURRENT_TIMESTAMP](https://msdn.microsoft.com/library/ms188751.aspx)
- [CURRENT_USER](https://msdn.microsoft.com/library/ms176050.aspx)
- [DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx)
- [DATALENGTH](https://msdn.microsoft.com/library/ms173486.aspx)
- [ΣΥΝΆΡΤΗΣΗ DATEADD](https://msdn.microsoft.com/library/ms186819.aspx)
- [ΣΥΝΆΡΤΗΣΗ DATEDIFF](https://msdn.microsoft.com/library/ms189794.aspx)
- [ΣΥΝΆΡΤΗΣΗ DATEFROMPARTS](https://msdn.microsoft.com/library/hh213228.aspx)
- [DATENAME](https://msdn.microsoft.com/library/ms174395.aspx)
- [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx)
- [DATETIME2FROMPARTS](https://msdn.microsoft.com/library/hh213312.aspx)
- [DATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213233.aspx)
- [DATETIMEOFFSETFROMPARTS](https://msdn.microsoft.com/library/hh231077.aspx)
- [ΗΜΈΡΑ](https://msdn.microsoft.com/library/ms176052.aspx)
- [DB_ID](https://msdn.microsoft.com/library/ms186274.aspx)
- [DB_NAME](https://msdn.microsoft.com/library/ms189753.aspx)
- [ΜΟΊΡΕΣ](https://msdn.microsoft.com/library/ms178566.aspx)
- [DENSE_RANK](https://msdn.microsoft.com/library/ms173825.aspx)
- [ΔΙΑΦΟΡΆ](https://msdn.microsoft.com/library/ms188753.aspx)
- [Η ΣΥΝΆΡΤΗΣΗ EOMONTH](https://msdn.microsoft.com/library/hh213020.aspx)
- [ERROR_MESSAGE](https://msdn.microsoft.com/library/ms190358.aspx)
- [ERROR_NUMBER](https://msdn.microsoft.com/library/ms175069.aspx)
- [ERROR_PROCEDURE](https://msdn.microsoft.com/library/ms188398.aspx)
- [ERROR_SEVERITY](https://msdn.microsoft.com/library/ms178567.aspx)
- [ERROR_STATE](https://msdn.microsoft.com/library/ms180031.aspx)
- [EXP](https://msdn.microsoft.com/library/ms179857.aspx)
- [FIRST_VALUE](https://msdn.microsoft.com/library/hh213018.aspx)
- [FLOOR](https://msdn.microsoft.com/library/ms178531.aspx)
- [GETDATE](https://msdn.microsoft.com/library/ms188383.aspx)
- [GETUTCDATE](https://msdn.microsoft.com/library/ms178635.aspx)
- [HAS_DBACCESS](https://msdn.microsoft.com/library/ms187718.aspx)
- [HASHBYTES](https://msdn.microsoft.com/library/ms174415.aspx)
- [INDEXPROPERTY](https://msdn.microsoft.com/library/ms187729.aspx)
- [ISDATE](https://msdn.microsoft.com/library/ms187347.aspx)
- [ISNULL](https://msdn.microsoft.com/library/ms184325.aspx)
- [ISNUMERIC](https://msdn.microsoft.com/library/ms186272.aspx)
- [ΑΠΌΚΡΙΣΗΣ](https://msdn.microsoft.com/library/hh231256.aspx)
- [LAST_VALUE](https://msdn.microsoft.com/library/hh231517.aspx)
- [ΥΠΟΨΉΦΙΟΣ ΠΕΛΆΤΗΣ](https://msdn.microsoft.com/library/hh213125.aspx)
- [ΑΡΙΣΤΕΡΆ](https://msdn.microsoft.com/library/ms177601.aspx)
- [Η ΣΥΝΆΡΤΗΣΗ LEN](https://msdn.microsoft.com/library/ms190329.aspx)
- [ΑΡΧΕΊΟ ΚΑΤΑΓΡΑΦΉΣ](https://msdn.microsoft.com/library/ms190319.aspx)
- [LOG10](https://msdn.microsoft.com/library/ms175121.aspx)
- [ΚΆΤΩ](https://msdn.microsoft.com/library/ms174400.aspx)
- [LTRIM](https://msdn.microsoft.com/library/ms177827.aspx)
- [MAX](https://msdn.microsoft.com/library/ms187751.aspx)
- [MIN](https://msdn.microsoft.com/library/ms179916.aspx)
- [ΜΉΝΑ](https://msdn.microsoft.com/library/ms187813.aspx)
- [NCHAR](https://msdn.microsoft.com/library/ms182673.aspx)
- [NTILE](https://msdn.microsoft.com/library/ms175126.aspx)
- [NULLIF](https://msdn.microsoft.com/library/ms177562.aspx)
- [OBJECT_ID](https://msdn.microsoft.com/library/ms190328.aspx)
- [ΌΝΟΜΑ ΑΝΤΙΚΕΙΜΈΝΟΥ](https://msdn.microsoft.com/library/ms186301.aspx)
- [OBJECTPROPERTY](https://msdn.microsoft.com/library/ms176105.aspx)
- [OIBJECTPROPERTYEX](https://msdn.microsoft.com/library/ms188390.aspx)
- [Συναρτήσεις ανυσμάτων ODBCS](https://msdn.microsoft.com/library/bb630290.aspx)
- [Επάνω από τον όρο FROM](https://msdn.microsoft.com/library/ms189461.aspx)
- [PARSENAME](https://msdn.microsoft.com/library/ms188006.aspx)
- [PATINDEX](https://msdn.microsoft.com/library/ms188395.aspx)
- [PERCENTILE_CONT](https://msdn.microsoft.com/library/hh231473.aspx)
- [PERCENTILE_DISC](https://msdn.microsoft.com/library/hh231327.aspx)
- [PERCENT_RANK](https://msdn.microsoft.com/library/hh213573.aspx)
- [PI](https://msdn.microsoft.com/library/ms189512.aspx)
- [POWER](https://msdn.microsoft.com/library/ms174276.aspx)
- [QUOTENAME](https://msdn.microsoft.com/library/ms176114.aspx)
- [ΑΚΤΊΝΙΑ](https://msdn.microsoft.com/library/ms189742.aspx)
- [ΣΥΝΆΡΤΗΣΗ RAND](https://msdn.microsoft.com/library/ms177610.aspx)
- [ΚΑΤΆΤΑΞΗ](https://msdn.microsoft.com/library/ms176102.aspx)
- [ΑΝΤΙΚΑΤΆΣΤΑΣΗ](https://msdn.microsoft.com/library/ms186862.aspx)
- [ΑΝΑΠΑΡΑΓΩΓΉ](https://msdn.microsoft.com/library/ms174383.aspx)
- [ΑΝΤΙΣΤΡΟΦΉ](https://msdn.microsoft.com/library/ms180040.aspx)
- [ΔΕΞΙΆ](https://msdn.microsoft.com/library/ms177532.aspx)
- [ΣΤΡΟΓΓΥΛΟΠΟΊΗΣΗ](https://msdn.microsoft.com/library/ms175003.aspx)
- [ΑΡΙΘΜΌΣ_ΓΡΑΜΜΉΣ](https://msdn.microsoft.com/library/ms186734.aspx)
- [RTRIM](https://msdn.microsoft.com/library/ms178660.aspx)
- [SCHEMA_ID](https://msdn.microsoft.com/library/ms188797.aspx)
- [SCHEMA_NAME](https://msdn.microsoft.com/library/ms175068.aspx)
- [SERVERPROPERTY](https://msdn.microsoft.com/library/ms174396.aspx)
- [SESSION_USER](https://msdn.microsoft.com/library/ms177587.aspx)
- [ΣΎΜΒΟΛΟ](https://msdn.microsoft.com/library/ms188420.aspx)
- [SIN](https://msdn.microsoft.com/library/ms188377.aspx)
- [SMALLDATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213396.aspx)
- [SOUNDEX](https://msdn.microsoft.com/library/ms187384.aspx)
- [ΚΕΝΌ ΔΙΆΣΤΗΜΑ](https://msdn.microsoft.com/library/ms187950.aspx)
- [SQL_VARIANT_PROPERTY](https://msdn.microsoft.com/library/ms178550.aspx)
- [Η ΣΥΝΆΡΤΗΣΗ SQRT](https://msdn.microsoft.com/library/ms176108.aspx)
- [ΤΕΤΡΆΓΩΝΟ](https://msdn.microsoft.com/library/ms173569.aspx)
- [STATS_DATE](https://msdn.microsoft.com/library/ms190330.aspx)
- [ΣΥΝΆΡΤΗΣΗ STDEV](https://msdn.microsoft.com/library/ms190474.aspx)
- [ΤΥΠΙΚΉ ΑΠΌΚΛΙΣΗ ΠΛΗΘΥΣΜΟΎ](https://msdn.microsoft.com/library/ms176080.aspx)
- [STR](https://msdn.microsoft.com/library/ms189527.aspx)
- [ΣΤΟΙΧΕΊΩΝ](https://msdn.microsoft.com/library/ms188043.aspx)
- [ΔΕΥΤΕΡΕΎΟΥΣΑ ΣΥΜΒΟΛΟΣΕΙΡΆ](https://msdn.microsoft.com/library/ms187748.aspx)
- [ΆΘΡΟΙΣΜΑ](https://msdn.microsoft.com/library/ms187810.aspx)
- [SUSER_SNAME](https://msdn.microsoft.com/library/ms174427.aspx)
- [SWITCHOFFSET](https://msdn.microsoft.com/library/bb677244.aspx)
- [SYSDATETIME](https://msdn.microsoft.com/library/bb630353.aspx)
- [SYSDATETIMEOFFSET](https://msdn.microsoft.com/library/bb677334.aspx)
- [SYSTEM_USER](https://msdn.microsoft.com/library/ms179930.aspx)
- [SYSUTCDATETIME](https://msdn.microsoft.com/library/bb630387.aspx)
- [TAN](https://msdn.microsoft.com/library/ms190338.aspx)
- [TERTIARY_WEIGHTS](https://msdn.microsoft.com/library/ms186881.aspx)
- [TIMEFROMPARTS](https://msdn.microsoft.com/library/hh213398.aspx)
- [TODATETIMEOFFSET](https://msdn.microsoft.com/library/bb630335.aspx)
- [TYPE_ID](https://msdn.microsoft.com/library/ms181628.aspx)
- [TYPE_NAME](https://msdn.microsoft.com/library/ms189750.aspx)
- [TYPEPROPERTY](https://msdn.microsoft.com/library/ms188419.aspx)
- [UNICODE](https://msdn.microsoft.com/library/ms180059.aspx)
- [ΕΠΆΝΩ](https://msdn.microsoft.com/library/ms180055.aspx)
- [ΧΡΉΣΤΗ](https://msdn.microsoft.com/library/ms186738.aspx)
- [ΌΝΟΜΑ_ΧΡΉΣΤΗ](https://msdn.microsoft.com/library/ms188014.aspx)
- [Η ΣΥΝΆΡΤΗΣΗ VAR](https://msdn.microsoft.com/library/ms186290.aspx)
- [ΣΥΝΆΡΤΗΣΗ VARP](https://msdn.microsoft.com/library/ms188735.aspx)
- [ΈΤΟΣ](https://msdn.microsoft.com/library/ms186313.aspx)
- [XACT_STATE](https://msdn.microsoft.com/library/ms189797.aspx)

## <a name="transactions"></a>Συναλλαγές

- [συναλλαγές](https://msdn.microsoft.com/library/mt204031.aspx)

## <a name="diagnostic-sessions"></a>Περίοδοι λειτουργίας διαγνωστικών

- [ΔΗΜΙΟΥΡΓΊΑ ΠΕΡΙΌΔΟΥ ΛΕΙΤΟΥΡΓΊΑΣ ΔΙΑΓΝΩΣΤΙΚΆ](https://msdn.microsoft.com/library/mt204029.aspx)

## <a name="procedures"></a>Διαδικασίες

- [sp_addrolemember](https://msdn.microsoft.com/library/ms187750.aspx)
- [sp_columns](https://msdn.microsoft.com/library/ms176077.aspx)
- [sp_configure](https://msdn.microsoft.com/library/ms188787.aspx)
- [sp_datatype_info_90](https://msdn.microsoft.com/library/mt204014.aspx)
- [sp_droprolemember](https://msdn.microsoft.com/library/ms188369.aspx)
- [sp_execute](https://msdn.microsoft.com/library/ff848746.aspx)
- [sp_executesql](https://msdn.microsoft.com/library/ms188001.aspx)
- [sp_fkeys](https://msdn.microsoft.com/library/ms175090.aspx)
- [sp_pdw_add_network_credentials](https://msdn.microsoft.com/library/mt204011.aspx)
- [sp_pdw_database_encryption](https://msdn.microsoft.com/library/mt219360.aspx)
- [sp_pdw_database_encryption_regenerate_system_keys](https://msdn.microsoft.com/library/mt204033.aspx)
- [sp_pdw_log_user_data_masking](https://msdn.microsoft.com/library/mt204023.aspx)
- [sp_pdw_remove_network_credentials](https://msdn.microsoft.com/library/mt204038.aspx)
- [sp_pkeys](https://msdn.microsoft.com/library/ms189813.aspx)
- [sp_prepare](https://msdn.microsoft.com/library/ff848808.aspx)
- [sp_spaceused](https://msdn.microsoft.com/library/ms188776.aspx)
- [sp_special_columns_100](https://msdn.microsoft.com/library/mt204025.aspx)
- [sp_sproc_columns](https://msdn.microsoft.com/library/ms182705.aspx)
- [sp_statistics](https://msdn.microsoft.com/library/ms173842.aspx)
- [sp_tables](https://msdn.microsoft.com/library/ms186250.aspx)
- [sp_unprepare](https://msdn.microsoft.com/library/ff848735.aspx)



## <a name="set-statements"></a>ΟΡΙΣΜΌΣ δηλώσεις

- [ΟΡΙΣΜΌΣ ANSI_DEFAULTS](https://msdn.microsoft.com/library/ms188340.aspx)
- [ΟΡΙΣΜΌΣ ANSI_NULL_DFLT_OFF](https://msdn.microsoft.com/library/ms187356.aspx)
- [ΟΡΙΣΜΌΣ ANSI_NULL_DFLT_ON](https://msdn.microsoft.com/library/ms187375.aspx)
- [ΟΡΙΣΜΌΣ ANSI_NULLS](https://msdn.microsoft.com/library/ms188048.aspx)
- [ΟΡΙΣΜΌΣ ANSI_PADDING](https://msdn.microsoft.com/library/ms187403.aspx)
- [ΟΡΙΣΜΌΣ ANSI_WARNINGS](https://msdn.microsoft.com/library/ms190368.aspx)
- [ΟΡΙΣΜΌΣ ARITHABORT](https://msdn.microsoft.com/library/ms190306.aspx)
- [ΟΡΙΣΜΌΣ ARITHIGNORE](https://msdn.microsoft.com/library/ms184341.aspx)
- [ΟΡΙΣΜΌΣ CONCAT_NULL_YIELDS_NULL](https://msdn.microsoft.com/library/ms176056.aspx)
- [ΟΡΙΣΜΌΣ DATEFIRST](https://msdn.microsoft.com/library/ms181598.aspx)
- [ΟΡΙΣΜΌΣ ΜΟΡΦΉ_ΗΜΕΡΟΜΗΝΊΑΣ](https://msdn.microsoft.com/library/ms189491.aspx)
- [ΟΡΙΣΜΌΣ FMTONLY](https://msdn.microsoft.com/library/ms173839.aspx)
- [ΟΡΙΣΜΌΣ IMPLICIT_TRANSACITONS](https://msdn.microsoft.com/library/ms187807.aspx)
- [ΟΡΙΣΜΌΣ LOCK_TIMEOUT](https://msdn.microsoft.com/library/ms189470.aspx)
- [ΟΡΙΣΜΌΣ NUMBERIC_ROUNDABORT](https://msdn.microsoft.com/library/ms188791.aspx)
- [ΟΡΙΣΜΌΣ QUOTED_IDENTIFIER](https://msdn.microsoft.com/library/ms174393.aspx)
- [ΟΡΙΣΜΌΣ ROWCOUNT](https://msdn.microsoft.com/library/ms188774.aspx)
- [ΟΡΙΣΜΌΣ ΜΈΓΕΘΟΣ ΚΕΙΜΈΝΟΥ](https://msdn.microsoft.com/library/ms186238.aspx)
- [ΟΡΙΣΜΌΣ ΕΠΙΠΈΔΟΥ ΑΠΟΜΌΝΩΣΗΣ ΣΥΝΑΛΛΑΓΉΣ](https://msdn.microsoft.com/library/ms173763.aspx)
- [ΟΡΙΣΜΌΣ XACT_ABORT](https://msdn.microsoft.com/library/ms188792.aspx)


## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες πληροφορίες αναφοράς, ανατρέξτε στο θέμα [Επισκόπηση αναφοράς αποθήκη δεδομένων του SQL][].

<!--Image references-->

<!--Article references-->
[Επισκόπηση αναφοράς αποθήκη δεδομένων του SQL]: sql-data-warehouse-overview-reference.md

<!--MSDN references-->
