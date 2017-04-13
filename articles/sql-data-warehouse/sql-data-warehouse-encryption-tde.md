<properties
   pageTitle="Κρυπτογράφηση διαφανή δεδομένων σε αποθήκη δεδομένων του SQL (πύλη) | Microsoft Azure"
   description="Κρυπτογράφηση διαφανή δεδομένων (TDE) σε αποθήκη δεδομένων του SQL"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Γρήγορα αποτελέσματα με διαφανή δεδομένων κρυπτογράφησης (TDE) στο αποθήκη δεδομένων του SQL

> [AZURE.SELECTOR]
- [Επισκόπηση ασφαλείας](sql-data-warehouse-overview-manage-security.md)
- [Έλεγχος ταυτότητας](sql-data-warehouse-authentication.md)
- [Κρυπτογράφηση (πύλη)](sql-data-warehouse-encryption-tde.md)
- [Κρυπτογράφηση (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Απαιτούμενα δικαιώματα

Για να ενεργοποιήσετε διαφανή κρυπτογράφησης δεδομένων (TDE), πρέπει να είστε διαχειριστής ή μέλος του ρόλου dbmanager.

## <a name="enabling-encryption"></a>Ενεργοποίηση της κρυπτογράφησης

Για να ενεργοποιήσετε την TDE για μια αποθήκη δεδομένων του SQL, ακολουθήστε τα παρακάτω βήματα:

1. Ανοίξτε τη βάση δεδομένων στην [πύλη του Azure](https://portal.azure.com)
2. Στο blade τη βάση δεδομένων, κάντε κλικ στο κουμπί **Ρυθμίσεις**
3. Ενεργοποιήστε την επιλογή **κρυπτογράφηση διαφανή δεδομένων**![][1]
4. Επιλέξτε τη ρύθμιση **σε**![][2]
5. Επιλέξτε **Αποθήκευση**
![][3]  

## <a name="disabling-encryption"></a>Απενεργοποίηση της κρυπτογράφησης

Για να απενεργοποιήσετε TDE για μια αποθήκη δεδομένων του SQL, ακολουθήστε τα παρακάτω βήματα:

1. Ανοίξτε τη βάση δεδομένων στην [πύλη του Azure](https://portal.azure.com)
2. Στο blade τη βάση δεδομένων, κάντε κλικ στο κουμπί **Ρυθμίσεις**
3. Ενεργοποιήστε την επιλογή **κρυπτογράφηση διαφανή δεδομένων**![][1]
4. Επιλέξτε τη ρύθμιση **απενεργοποιημένη**![][4]
5. Επιλέξτε **Αποθήκευση**
![][5]  

## <a name="encryption-dmvs"></a>Κρυπτογράφηση DMVs

Κρυπτογράφηση μπορεί να είναι επιβεβαιωθεί με την παρακάτω DMVs:

- [sys.Databases]
- [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
