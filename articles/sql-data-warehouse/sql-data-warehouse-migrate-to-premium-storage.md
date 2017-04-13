<properties
   pageTitle="Μετεγκατάσταση του υπάρχοντος αποθήκη δεδομένων του SQL Azure με το χώρο αποθήκευσης premium | Microsoft Azure"
   description="Οδηγίες για τη μετεγκατάσταση ενός υπάρχοντος αποθήκη δεδομένων του SQL με το χώρο αποθήκευσης premium"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="nicw;barbkess;sonyama"/>

# <a name="migration-to-premium-storage-details"></a>Μετεγκατάσταση σε λεπτομέρειες Premium χώρου αποθήκευσης
Αποθήκη δεδομένων του SQL εισαχθεί πρόσφατα [Premium χώρου αποθήκευσης για μεγαλύτερη προβλεψιμότητα επιδόσεων][].  Συνεχίζουμε να τώρα έτοιμο για τη μετεγκατάσταση υπάρχοντος χώρους αποθήκευσης δεδομένων αυτήν τη στιγμή στην τυπική χώρου αποθήκευσης στο χώρο αποθήκευσης Premium.  Διαβάστε παρακάτω για περισσότερες λεπτομέρειες σχετικά με το πώς και πότε αυτόματων μετεγκαταστάσεις προκύψουν και πώς μπορείτε να αυτο-μετεγκαταστήσετε Εάν προτιμάτε να ελέγχετε πότε παρουσιάζεται ο χρόνος εκτός λειτουργίας.

Εάν έχετε περισσότερες από μία αποθήκη δεδομένων, χρησιμοποιήστε το [Χρονοδιάγραμμα αυτόματων μετεγκατάστασης][] παρακάτω για να καθορίσετε πότε θα επίσης να μετεγκατασταθούν.

## <a name="determine-storage-type"></a>Προσδιορισμός του χώρου αποθήκευσης
Εάν έχετε δημιουργήσει μια DW πριν από τις παρακάτω ημερομηνίες, αυτήν τη στιγμή χρησιμοποιείτε Τυπική χώρου αποθήκευσης.  Κάθε αποθήκη δεδομένων σε τυπική χώρου αποθήκευσης που υπόκεινται αυτόματη μετεγκατάσταση περιλαμβάνει μια ειδοποίηση στο επάνω μέρος του blade αποθήκη δεδομένων στην [Πύλη του Azure][] που αναφέρει ότι "μια προσεχή*αναβάθμιση με το χώρο αποθήκευσης premium θα απαιτούν ένα μη διαθεσιμότητα.  Μάθετε περισσότερα ->*. "

| **Περιοχή**          | **DW που έχουν δημιουργηθεί πριν από αυτήν την ημερομηνία**   |
| :------------------ | :-------------------------------- |
| Ανατολική Αυστραλία      | Premium χώρου αποθήκευσης δεν είναι ακόμη διαθέσιμη |
| Αυστραλία νοτιοανατολικής | 5 Αυγούστου 2016                    |
| Νότια Βραζιλίας        | 5 Αυγούστου 2016                    |
| Κεντρική ώρα Καναδά      | 25 Μαΐου 2016                      |
| Καναδάς Ανατολή         | 26 Μαΐου 2016                      |
| Κεντρική η.π.α.          | 26 Μαΐου 2016                      |
| Κίνα Ανατολή          | Premium χώρου αποθήκευσης δεν είναι ακόμη διαθέσιμη |
| Βόρεια Κίνα         | Premium χώρου αποθήκευσης δεν είναι ακόμη διαθέσιμη |
| Ανατολικής Ασίας           | 25 Μαΐου 2016                      |
| Ανατολικής η.π.α.             | 26 Μαΐου 2016                      |
| Ανατολικής US2            | 27 Μαΐου 2016                      |
| Κεντρική Ινδίας       | 27 Μαΐου 2016                      |
| Νότια Ινδίας         | 26 Μαΐου 2016                      |
| Δυτική Ινδίας          | Premium χώρου αποθήκευσης δεν είναι ακόμη διαθέσιμη |
| Ιαπωνία Ανατολή          | 5 Αυγούστου 2016                    |
| Δυτική Ιαπωνία          | Premium χώρου αποθήκευσης δεν είναι ακόμη διαθέσιμη |
| Βόρεια κεντρική η.π.α.    | Premium χώρου αποθήκευσης δεν είναι ακόμη διαθέσιμη |
| Βόρεια Ευρώπη        | 5 Αυγούστου 2016                    |
| Νότια κεντρικής η.π.α.    | 27 Μαΐου 2016                      |
| Νοτιοανατολικής Ασίας      | 24 Μαΐου 2016                      |
| Δυτική Ευρώπη         | 25 Μαΐου 2016                      |
| Δυτική κεντρικής η.π.α.     | 26 Αυγούστου 2016                   |
| Δυτική η.π.α.             | 26 Μαΐου 2016                      |
| Δυτική US2            | 26 Αυγούστου 2016                   |

## <a name="automatic-migration-details"></a>Λεπτομέρειες αυτόματων μετεγκατάστασης
Από προεπιλογή, θα σας θα μετεγκατάσταση βάσης δεδομένων σας για εσάς κατά τη διάρκεια 6 μμ και 6 ΠΜ στην τοπική ώρα της περιοχής σας κατά το [Χρονοδιάγραμμα αυτόματων μετεγκατάστασης][] κάτω από.  Το υπάρχον αποθήκη δεδομένων θα χωρίς δυνατότητα χρήσης κατά τη διάρκεια της μετεγκατάστασης.  Θα σας εκτίμηση ότι η μετεγκατάσταση θα χρειαστεί περίπου μία ώρα ανά TB χώρου αποθήκευσης ανά αποθήκη δεδομένων.  Θα σας θα εξασφαλίσει επίσης ότι δεν θα χρεωθείτε σε οποιοδήποτε τμήμα της αυτόματης μετεγκατάστασης.

> [AZURE.NOTE] Δεν θα μπορείτε να χρησιμοποιήσετε την υπάρχουσα αποθήκη δεδομένων κατά τη διάρκεια της μετεγκατάστασης.  Μόλις ολοκληρωθεί η μετεγκατάσταση, σας αποθήκη δεδομένων θα συνδεθείτε ξανά.

Οι παρακάτω λεπτομέρειες είναι τα βήματα που Microsoft διαρκεί για λογαριασμό σας για την ολοκλήρωση της μετεγκατάστασης και δεν απαιτεί οποιαδήποτε συμμετοχή από μέρους σας.  Σε αυτό το παράδειγμα, φανταστείτε ότι το υπάρχον DW σχετικά με την τυπική αποθήκευσης αυτήν τη στιγμή ονομάζεται "MyDW."

1.  Microsoft μετονομάζει "MyDW" σε "MyDW_DO_NOT_USE_ [χρονικής σήμανσης]"
2.  Microsoft διακόπτει προσωρινά "MyDW_DO_NOT_USE_ [χρονικής σήμανσης]."  Σε αυτό το διάστημα, λαμβάνεται ένα αντίγραφο ασφαλείας.  Μπορείτε να δείτε πολλά παύση/βιογραφικά σημειώματα εάν θα σας αντιμετωπίσετε προβλήματα κατά τη διάρκεια αυτής της διαδικασίας.
3.  Η Microsoft δημιουργεί μια νέα DW με το όνομα "MyDW" στο χώρο αποθήκευσης Premium από το αντίγραφο ασφαλείας στο βήμα 2.  "MyDW" δεν θα εμφανίζεται μέχρι μετά την ολοκλήρωση της επαναφοράς.
4.  Μόλις ολοκληρωθεί η επαναφορά, "MyDW" επιστρέφει το ίδιο DWUs και έχει διακοπεί ή ενεργή κατάσταση που ήταν πριν από τη μετεγκατάσταση.
5.  Μόλις ολοκληρωθεί η μετεγκατάσταση, Microsoft διαγράφει "MyDW_DO_NOT_USE_ [χρονικής σήμανσης]"
    
> [AZURE.NOTE] Αυτές οι ρυθμίσεις δεν μεταφέρετε ως μέρος της μετεγκατάστασης:
> 
>   -  Έλεγχος στο επίπεδο της βάσης δεδομένων πρέπει να ενεργοποιηθεί ξανά
>   -  Κανόνες τείχους προστασίας στο επίπεδο της **βάσης δεδομένων** πρέπει να είναι readded.  Κανόνες τείχους προστασίας στο επίπεδο του **διακομιστή** δεν επηρεάζονται.

### <a name="automatic-migration-schedule"></a>Προγραμματισμός αυτόματης μετεγκατάστασης
Αυτόματη μετεγκαταστάσεις προκύπτουν από 6 μμ – 6 ΠΜ (τοπική ώρα ανά περιοχή) κατά τη διάρκεια το παρακάτω χρονοδιάγραμμα μη διαθεσιμότητα.

| **Περιοχή**          | **Εκτιμώμενη έναρξη ημερομηνίας**     | **Εκτιμώμενη ημερομηνία λήξης**       |
| :------------------ | :--------------------------- | :--------------------------- |
| Ανατολική Αυστραλία      | Δεν έχει καθοριστεί ακόμη           | Δεν έχει καθοριστεί ακόμη           |
| Αυστραλία νοτιοανατολικής | 10 Αυγούστου 2016              | 24 Αυγούστου 2016              |
| Νότια Βραζιλίας        | 10 Αυγούστου 2016              | 24 Αυγούστου 2016              |
| Κεντρική ώρα Καναδά      | 23 Ιουνίου 2016                | 1 Ιουλίου 2016                 |
| Καναδάς Ανατολή         | 23 Ιουνίου 2016                | 1 Ιουλίου 2016                 |
| Κεντρική η.π.α.          | 23 Ιουνίου 2016                | 4 Ιουλίου 2016                 |
| Κίνα Ανατολή          | Δεν έχει καθοριστεί ακόμη           | Δεν έχει καθοριστεί ακόμη           |
| Βόρεια Κίνα         | Δεν έχει καθοριστεί ακόμη           | Δεν έχει καθοριστεί ακόμη           |
| Ανατολικής Ασίας           | 23 Ιουνίου 2016                | 1 Ιουλίου 2016                 |
| Ανατολικής η.π.α.             | 23 Ιουνίου 2016                | 11 Ιουλίου 2016                |
| Ανατολικής US2            | 23 Ιουνίου 2016                | 8 Ιουλίου 2016                 |
| Κεντρική Ινδίας       | 23 Ιουνίου 2016                | 1 Ιουλίου 2016                 |
| Νότια Ινδίας         | 23 Ιουνίου 2016                | 1 Ιουλίου 2016                 |
| Δυτική Ινδίας          | Δεν έχει καθοριστεί ακόμη           | Δεν έχει καθοριστεί ακόμη           |
| Ιαπωνία Ανατολή          | 10 Αυγούστου 2016              | 24 Αυγούστου 2016              |
| Δυτική Ιαπωνία          | Δεν έχει καθοριστεί ακόμη           | Δεν έχει καθοριστεί ακόμη           |
| Βόρεια κεντρική η.π.α.    | Δεν έχει καθοριστεί ακόμη           | Δεν έχει καθοριστεί ακόμη           |
| Βόρεια Ευρώπη        | 10 Αυγούστου 2016              | 31 Αυγούστου 2016              |
| Νότια κεντρικής η.π.α.    | 23 Ιουνίου 2016                | 2 Ιουλίου 2016                 |
| Νοτιοανατολικής Ασίας      | 23 Ιουνίου 2016                | 1 Ιουλίου 2016                 |
| Δυτική Ευρώπη         | 23 Ιουνίου 2016                | 8 Ιουλίου 2016                 |
| Δυτική κεντρικής η.π.α.     | 14 Αυγούστου 2016              | 31 Αυγούστου 2016              |
| Δυτική η.π.α.             | 23 Ιουνίου 2016                | 7 Ιουλίου 2016                 |
| Δυτική US2            | 14 Αυγούστου 2016              | 31 Αυγούστου 2016              |

## <a name="self-migration-to-premium-storage"></a>Αυτόματη μετεγκατάσταση στο χώρο αποθήκευσης Premium
Εάν θέλετε να ελέγχετε πότε θα παρουσιαστεί του χρόνου εκτός λειτουργίας, μπορείτε να χρησιμοποιήσετε τα παρακάτω βήματα για τη μετεγκατάσταση ενός υπάρχοντος αποθήκη δεδομένων σε τυπική χώρου αποθήκευσης στο χώρο αποθήκευσης Premium.  Εάν επιλέξετε να μετεγκαταστήσετε αυτόνομα, πρέπει να ολοκληρώσετε τη μετεγκατάσταση μισθωτής πριν να ξεκινήσει η αυτόματη μετεγκατάσταση σε αυτήν την περιοχή για να αποφύγετε τυχόν κίνδυνο της αυτόματης μετεγκατάστασης που δημιουργεί διένεξη (ανατρέξτε στο [Χρονοδιάγραμμα αυτόματων μετεγκατάστασης][]).

### <a name="self-migration-instructions"></a>Οδηγίες μισθωτής μετεγκατάστασης
Εάν θέλετε να ελέγχετε το χρόνο εκτός λειτουργίας, μπορείτε να αυτο-μετεγκαταστήσετε σας αποθήκη δεδομένων με χρήση της δημιουργίας αντιγράφων ασφαλείας/επαναφοράς.  Στο τμήμα επαναφοράς της μετεγκατάστασης αναμένεται να χρειαστεί περίπου μία ώρα ανά χώρο αποθήκευσης ανά DW TB.  Εάν θέλετε να διατηρήσετε το ίδιο όνομα, όταν ολοκληρωθεί η μετεγκατάσταση, ακολουθήστε τα βήματα για [τα βήματα για να μετονομάσετε κατά τη διάρκεια της μετεγκατάστασης][]. 

1.  [Παύση][] σας DW που λαμβάνει μια αυτόματη δημιουργία αντιγράφων ασφαλείας
2.  [Επαναφορά][] από τις πιο πρόσφατες στιγμιότυπο
3.  Διαγράψτε το υπάρχον DW στην τυπική χώρου αποθήκευσης. **Εάν μπορέσετε να εκτελέσετε αυτό το βήμα, θα χρεωθεί για δύο DWs.**

> [AZURE.NOTE] Αυτές οι ρυθμίσεις δεν μεταφέρετε ως μέρος της μετεγκατάστασης:
> 
>   -  Έλεγχος στο επίπεδο της βάσης δεδομένων πρέπει να ενεργοποιηθεί ξανά
>   -  Κανόνες τείχους προστασίας στο επίπεδο της **βάσης δεδομένων** πρέπει να είναι readded.  Κανόνες τείχους προστασίας στο επίπεδο του **διακομιστή** δεν επηρεάζονται.

#### <a name="optional-steps-to-rename-during-migration"></a>Προαιρετικό: βήματα για τη μετονομασία κατά τη διάρκεια της μετεγκατάστασης 
Δύο βάσεις δεδομένων στον ίδιο διακομιστή λογική δεν μπορούν να έχουν το ίδιο όνομα. Αποθήκη δεδομένων του SQL υποστηρίζει πλέον τη δυνατότητα να μετονομάσετε μια DW.

Σε αυτό το παράδειγμα, φανταστείτε ότι το υπάρχον DW σχετικά με την τυπική αποθήκευσης αυτήν τη στιγμή ονομάζεται "MyDW."

1.  Μετονομασία "MyDW" με την εντολή ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ που ακολουθεί για κάτι, όπως "MyDW_BeforeMigration."  Η εντολή τερματίζει όλες τις υπάρχουσες συναλλαγές και πρέπει να γίνουν με την κύρια βάση δεδομένων για να ολοκληρωθεί με επιτυχία.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.  [Παύση][] "MyDW_BeforeMigration", το οποίο λαμβάνει μια αυτόματη δημιουργία αντιγράφων ασφαλείας
3.  [Επαναφορά][] από τις πιο πρόσφατες στιγμιότυπο μια νέα βάση δεδομένων με το όνομα που χρησιμοποιήσατε για να έχετε (ex: "MyDW")
4.  Διαγραφή "MyDW_BeforeMigration".  **Εάν μπορέσετε να εκτελέσετε αυτό το βήμα, θα χρεωθεί για δύο DWs.**

> [AZURE.NOTE] Αυτές οι ρυθμίσεις δεν μεταφέρετε ως μέρος της μετεγκατάστασης:
> 
>   -  Έλεγχος στο επίπεδο της βάσης δεδομένων πρέπει να ενεργοποιηθεί ξανά
>   -  Κανόνες τείχους προστασίας στο επίπεδο της **βάσης δεδομένων** πρέπει να είναι readded.  Κανόνες τείχους προστασίας στο επίπεδο του **διακομιστή** δεν επηρεάζονται.

## <a name="next-steps"></a>Επόμενα βήματα
Με την αλλαγή χώρου αποθήκευσης Premium, θα σας έχουν αυξηθεί επίσης τον αριθμό των αρχείων blob βάσης δεδομένων στην υποκείμενη αρχιτεκτονική της σας αποθήκη δεδομένων.  Για να μεγιστοποιήσετε τα οφέλη επιδόσεων αυτής της αλλαγής, συνιστάται να κάνετε αναδόμηση των ομαδοποιημένων Columnstore δεικτών σας χρησιμοποιώντας την ακόλουθη δέσμη ενεργειών.  Το παρακάτω δέσμη ενεργειών λειτουργεί, να επιβάλετε ορισμένα από τα υπάρχοντα δεδομένα για τα πρόσθετα αντικείμενα blob.  Εάν κάνετε καμία ενέργεια, τα δεδομένα θα αναδιανομή ομαλά μέσα στο χρόνο κατά τη φόρτωση περισσότερων δεδομένων σε πίνακες σας αποθήκη δεδομένων.

**Προαπαιτούμενα:**

1.  Αποθήκη δεδομένων θα πρέπει να εκτελούνται με 1.000 DWUs ή νεότερη έκδοση (ανατρέξτε στο θέμα [power υπολογισμού κλίμακα][])
2.  Εκτέλεση της δέσμης ενεργειών χρήστη πρέπει να είναι στο [ρόλο mediumrc][] ή υψηλότερη
    1.  Για να προσθέσετε ένα χρήστη με αυτόν το ρόλο, εκτελέστε τα εξής: 
        1.  ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create Table to control Index Rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select 
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from 
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go
 
--------------------------------------------------------------------------------
-- Step 2: Execute Index Rebuilds.  If script fails, the below can be rerun to restart where last left off
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Cleanup Table Created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Εάν αντιμετωπίσετε προβλήματα με την αποθήκη δεδομένων, να [δημιουργήσετε ένα δελτίο υποστήριξης για][] και αναφορά "Μετεγκατάσταση για να Premium χώρου αποθήκευσης" ως την πιθανή αιτία.

<!--Image references-->

<!--Article references-->
[Προγραμματισμός αυτόματης μετεγκατάστασης]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[Δημιουργήστε ένα δελτίο υποστήριξης]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Παύση]: sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Επαναφορά]: sql-data-warehouse-restore-database-portal.md
[βήματα για τη μετονομασία κατά τη διάρκεια της μετεγκατάστασης]: #optional-steps-to-rename-during-migration
[κλίμακα power υπολογισμού]: sql-data-warehouse-manage-compute-portal/#scale-compute-power
[ρόλος mediumrc]: sql-data-warehouse-develop-concurrency/#workload-management

<!--MSDN references-->


<!--Other Web references-->
[Χώρος αποθήκευσης Premium για μεγαλύτερη προβλεψιμότητα επιδόσεων]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Πύλη του Azure]: https://portal.azure.com