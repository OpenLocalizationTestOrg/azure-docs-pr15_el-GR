<properties
    pageTitle="Ρύθμιση παραμέτρων Oracle GoldenGate ΣΠΣ | Microsoft Azure"
    description="Ακολουθήστε τα βήματα ένα πρόγραμμα εκμάθησης για τη ρύθμιση του και την εφαρμογή Oracle GoldenGate σε VM διακομιστή Windows Azure για υψηλή διαθεσιμότητα και την καταστροφή αποκατάστασης."
    services="virtual-machines-windows"
    authors="rickstercdn"
    manager="timlt"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/06/2016"
    ms.author="rclaus" />


#<a name="configuring-oracle-goldengate-for-azure"></a>Ρύθμιση παραμέτρων Oracle GoldenGate για Azure


Αυτό το πρόγραμμα εκμάθησης παρουσιάζει τον τρόπο ρύθμισης Oracle GoldenGate για το περιβάλλον εικονικές μηχανές Windows Azure για υψηλή διαθεσιμότητα και αποκατάσταση. Το πρόγραμμα εκμάθησης εστιάζει σε [διπλής κατεύθυνσης αναπαραγωγής](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm) για βάσεις δεδομένων της Oracle μη RAC και απαιτεί δύο τοποθεσίες είναι ενεργό.

Oracle GoldenGate υποστηρίζει διανομής δεδομένων και ενσωμάτωση δεδομένων. Σας επιτρέπει να ορίσετε μια κατανομή δεδομένων και λύση συγχρονισμού δεδομένων μέσα από τη ρύθμιση παραμέτρων αναπαραγωγής Oracle Oracle και παρέχει μια λύση ευέλικτη υψηλής διαθεσιμότητας. Oracle GoldenGate συμπληρώνει Επιφυλακή δεδομένων Oracle με τις δυνατότητές της αναπαραγωγής για να ενεργοποιήσετε την κατανομή και μηδέν-χρόνου εκτός λειτουργίας αναβαθμίσεις εταιρικές πληροφορίες και μετεγκαταστάσεις. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Χρήση Oracle GoldenGate με Επιφυλακή δεδομένων Oracle](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm).

Oracle GoldenGate περιλαμβάνει τα παρακάτω κύρια στοιχεία: εξαγάγετε, ίχνη αντλία, Replicat, δεδομένων ή να εξαγάγετε αρχεία, τα σημεία ελέγχου, διαχείριση και συλλογής. Για να έχετε διπλής κατεύθυνσης αλληλεπίδραση μεταξύ των δύο τοποθεσίες, πρέπει να δημιουργήσετε και να ξεκινήσετε όλων των στοιχείων στις δύο τοποθεσίες. Για λεπτομερείς πληροφορίες για την αρχιτεκτονική Oracle GoldenGate, ανατρέξτε στο θέμα [Οδηγός GoldenGate Oracle](http://docs.oracle.com/goldengate/1212/gg-winux/index.html).

Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη θεωρητική και πρακτική γνώσεων σε υψηλή διαθεσιμότητα βάση δεδομένων Oracle και αποκατάσταση έννοιες, καθώς και [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html). Για περισσότερες πληροφορίες, ανατρέξτε στην [τοποθεσία web Oracle](http://www.oracle.com/technetwork/database/features/availability/index.html).

Επιπλέον, το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη εγκαταστήσει τις ακόλουθες προϋποθέσεις:

- Μπορείτε να αναθεωρήσετε ήδη ενότητα υψηλή διαθεσιμότητα και τα θέματα αποκατάστασης από καταστροφή στο θέμα [Oracle εικονική μηχανή εικόνες - διάφορα θέματα](virtual-machines-windows-classic-oracle-considerations.md) . Σημειώστε ότι Azure υποστηρίζει μεμονωμένη βάση δεδομένων της Oracle παρουσίες, αλλά δεν Oracle πραγματικό εφαρμογή συμπλεγμάτων (Oracle RAC) προς το παρόν.

- Λήψη του λογισμικού Oracle GoldenGate από την τοποθεσία web [Στοιχεία λήψης Oracle](http://www.oracle.com/us/downloads/index.html) . Έχετε επιλέξει το προϊόν πακέτο Oracle σύντηξης ενδιάμεσο-ενσωμάτωση δεδομένων. Στη συνέχεια, έχετε επιλέξει Oracle GoldenGate στην Oracle v11.2.1 πακέτο πολυμέσων για τα Microsoft Windows x64 (64 bit) για μια βάση δεδομένων Oracle 11 g. Στη συνέχεια, κάντε λήψη του V11.2.1.0.3 GoldenGate Oracle για Oracle 11g 64 bit σε Windows 2008 (64 bit).

- Έχετε δημιουργήσει δύο εικονικές μηχανές (ΣΠΣ) στο Azure χρησιμοποιώντας Oracle Enterprise Edition σε Windows Server. Βεβαιωθείτε ότι το εικονικές μηχανές είναι στην [ίδια υπηρεσία στο cloud](virtual-machines-linux-load-balance.md) και στο ίδιο [Εικονικού δικτύου](https://azure.microsoft.com/documentation/services/virtual-network/) για να βεβαιωθείτε ότι μπορούν να έχουν πρόσβαση σε άλλη επάνω από τη μόνιμη ιδιωτική διεύθυνση IP.

- Που έχετε ορίσει τα ονόματα εικονική μηχανή ως "MachineGG1" για μια τοποθεσία και "MachineGG2" για την τοποθεσία Β στην πύλη του Azure κλασική.

- Δοκιμή βάσεις δεδομένων έχετε δημιουργήσει "TestGG1" σε μια τοποθεσία και "TestGG2" στην τοποθεσία B.

- Συνδέεστε με το διακομιστή Windows ως μέλος της ομάδας διαχειριστών ή μέλος της ομάδας **ORA_DBA** .

Σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει:

1. Διαμόρφωση βάσης δεδομένων στην τοποθεσία A και B τοποθεσιών  

    1. Εκτέλεση φόρτωσης αρχικών δεδομένων

2. Προετοιμασία τοποθεσία A και B τοποθεσίας για αναπαραγωγή βάσης δεδομένων

3. Δημιουργία όλα τα αντικείμενα είναι απαραίτητο για την υποστήριξη DDL αναπαραγωγής

4. Ρύθμιση παραμέτρων διαχείρισης GoldenGate στην τοποθεσία A και B τοποθεσίας

5. Δημιουργία ομάδας εξάγουν και αντλία δεδομένων διεργασίες στην τοποθεσία A και B τοποθεσίας

    1. Διεργασιών για την εξαγωγή και αντλία δεδομένων σε μια τοποθεσία

    2. Δημιουργία πίνακα σημείο ελέγχου GoldenGate στην τοποθεσία B

    3. Προσθήκη REPLICAT στην τοποθεσία B

    4. Διεργασιών για την εξαγωγή και αντλία δεδομένων στην τοποθεσία B

    5. Δημιουργία πίνακα GoldenGate σημείο ελέγχου σε μια τοποθεσία

    6. Προσθήκη REPLICAT σε μια τοποθεσία

    7. Προσθήκη trandata στην τοποθεσία A και B τοποθεσίας

    8. Έναρξη διεργασίες εξαγωγή και αντλία δεδομένων σε μια τοποθεσία

    9. Έναρξη διεργασίες εξαγωγή και αντλία δεδομένων στην τοποθεσία B

    10. Έναρξη διεργασίας REPLICAT σε μια τοποθεσία

    11. Έναρξη διεργασίας REPLICAT στην τοποθεσία B

6. Επαληθεύστε τη διαδικασία αναπαραγωγής διπλής κατεύθυνσης

>[AZURE.IMPORTANT] Αυτό το πρόγραμμα εκμάθησης έχει διαμόρφωση και ελέγχονται σύμφωνα με τις ακόλουθες ρυθμίσεις παραμέτρων λογισμικό:
>
>|                        | **Τοποθεσία μια βάση δεδομένων**              | **Τοποθεσία B βάσης δεδομένων**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Έκδοση Oracle**     | Oracle11g έκδοση 2 – (11.2.0.1) | Oracle11g έκδοση 2 – (11.2.0.1) |
>| **Το όνομα του υπολογιστή**       | MachineGG1                       | MachineGG2                       |
>| **Λειτουργικό σύστημα**   | Windows 2008 R2                  | Windows 2008 R2                  |
>| **Oracle αναγνωριστικό ΑΣΦΑΛΕΊΑΣ**         | TESTGG1                          | TESTGG2                          |
>| **Σχήμα αναπαραγωγής** | SCOTT                            | SCOTT                            |

Οι επόμενες εκδόσεις της βάσης δεδομένων Oracle και Oracle GoldenGate, ενδέχεται να υπάρχουν ορισμένες πρόσθετες αλλαγές που χρειάζεστε για την υλοποίηση. Για την πιο ενημερωμένη έκδοση συγκεκριμένες πληροφορίες, ανατρέξτε στην τεκμηρίωση [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) και [Βάση δεδομένων της Oracle](http://www.oracle.com/us/corporate/features/database-12c/index.html) στην τοποθεσία web της Oracle. Για παράδειγμα, για μια βάση δεδομένων προέλευσης κυκλοφορίας 11.2.0.4 και νεότερες εκδόσεις, την καταγραφή της DDL πραγματοποιείται από το διακομιστή logmining ασύγχρονα και απαιτεί ειδική εναύσματα, πίνακες ή άλλα αντικείμενα της βάσης δεδομένων να έχει εγκατασταθεί. Μπορούν να εκτελεστούν Oracle GoldenGate αναβαθμίσεις χωρίς διακοπή εφαρμογές χρήστη. Χρησιμοποιείτε ένα έναυσμα DDL και αντικείμενα υποστήριξης είναι απαραίτητη όταν εξαγάγετε είναι σε ενσωματωμένη λειτουργία με μια βάση δεδομένων Oracle 11g προέλευσης που είναι παλαιότερες από την έκδοση 11.2.0.4. Για λεπτομερείς οδηγίες, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων GoldenGate Oracle για βάση δεδομένων Oracle](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf).

##<a name="1-setup-database-on-site-a-and-site-b"></a>1. διαμόρφωση βάσης δεδομένων στην τοποθεσία A και B τοποθεσιών
Η ενότητα αυτή εξηγεί πώς να εκτελείτε τις προϋποθέσεις βάσης δεδομένων στην τοποθεσία Α και β τοποθεσίας. Πρέπει να εκτελέσετε όλα τα βήματα αυτής της ενότητας στις δύο τοποθεσίες: τοποθεσία Α και β τοποθεσίας.

Πρώτη απομακρυσμένη επιφάνεια εργασίας, τοποθεσία A και B τοποθεσίας μέσω Azure κλασική πύλη. Ανοίξτε μια γραμμή εντολών των Windows και δημιουργήστε έναν κεντρικό κατάλογο για αρχεία της Oracle GoldenGate εγκατάστασης:

    mkdir C:\OracleGG

Στη συνέχεια, αποσυμπίεση και εγκατάσταση του λογισμικού Oracle GoldenGate σε αυτόν το φάκελο. Μετά από αυτό το βήμα, μπορείτε να ξεκινήσετε το πρόγραμμα μεταγλώττισης εντολή λογισμικού GoldenGate (GGSCI), εκτελώντας την ακόλουθη εντολή:

    C:\OracleGG\.\ggsci

Μπορείτε να χρησιμοποιήσετε [GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm) για να εκτελέσετε διάφορες εντολές που ρύθμιση παραμέτρων, έλεγχο και εποπτεία Oracle GoldenGate.

Στη συνέχεια, εκτελέστε την ακόλουθη εντολή για τη δημιουργία όλων των υποφακέλων από του πακέτου εγκατάστασης:

    GGSCI (Hostname) 1> CREATE SUBDIRS

Εκτελέστε την παρακάτω εντολή για να εξέλθετε από τη γραμμή εντολών GGSCI:

    GGSCI (Hostname) 1> EXIT

Στη συνέχεια, πρέπει να δημιουργήσετε έναν νέο χρήστη βάσης δεδομένων, που θα χρησιμοποιηθεί από τις διεργασίες διευθυντή GoldenGate Oracle, εξαγωγή και αναπαραγωγής. Σημειώστε ότι μπορείτε να δημιουργήσετε μεμονωμένους χρήστες για κάθε διαδικασία ή να ρυθμίσετε τις παραμέτρους μόνο μία κοινή χρήστη. Σε αυτό το πρόγραμμα εκμάθησης, μπορούμε να δημιουργήσουμε ένα χρήστη, το οποίο καλείται ως ggate. Στη συνέχεια, σας παρέχουμε αυτόν το χρήστη τα απαραίτητα δικαιώματα. Σημειώστε ότι πρέπει να εκτελέσετε τις ακόλουθες ενέργειες στην τοποθεσία Α και β τοποθεσίας.

Άνοιγμα του SQL\*συν παράθυρο εντολών στην τοποθεσία A και B τοποθεσία με δικαιώματα διαχειριστή βάσης δεδομένων χρησιμοποιώντας το **SYSDBA**, όπως:

    Enter username: / as sysdba

Και να εκτελέσετε:

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m;
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

Στη συνέχεια, εντοπίστε την Προετοιμασία\<DatabaseSID\>. ORA αρχείων στο % ORACLE\_ΚΕΝΤΡΙΚΉ %\\φάκελο στην τοποθεσία A και B τοποθεσίας βάσης δεδομένων και να προσαρτήσετε τις ακόλουθες παραμέτρους βάσης δεδομένων σε INITTEST.ora:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

Για μια πλήρη λίστα όλων των εντολών Oracle GoldenGate GGSCI, ανατρέξτε στο θέμα [αναφορά για GoldenGate Oracle για Windows](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm).

### <a name="perform-initial-data-load"></a>Εκτέλεση φόρτωσης αρχικών δεδομένων

Μπορείτε να εκτελέσετε το φορτίο αρχικών δεδομένων στη βάση δεδομένων, ακολουθώντας τις διάφορες μεθόδους. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε το [Oracle GoldenGate απευθείας φόρτωσης](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm) ή κανονική εισαγωγή και εξαγωγή βοηθητικά προγράμματα να εξάγετε δεδομένα πίνακα από την τοποθεσία Α σε τοποθεσία B.

Για μια επίδειξη της διαδικασίας αναπαραγωγής Oracle GoldenGate, αυτό το πρόγραμμα εκμάθησης δείχνει τη δημιουργία ενός πίνακα σε τοποθεσία A και B τοποθεσίας, χρησιμοποιώντας τις παρακάτω εντολές.

Πρώτα, ανοίξτε του SQL\*Plus εντολή παράθυρο και εκτελέστε την ακόλουθη εντολή για να δημιουργήσετε έναν πίνακα αποθέματος στην τοποθεσία Α και Β τοποθεσίας βάσεις δεδομένων:

    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

Στη συνέχεια, προσθέστε έναν περιορισμό στον πίνακα που μόλις δημιουργήθηκε στην τοποθεσία A και B τοποθεσίας βάσεις δεδομένων:

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

Στη συνέχεια, να εκχωρήσετε όλα τα δικαιώματα του νέου πίνακα απόθεμα για να το ggate χρήστη στην τοποθεσία Α και β τοποθεσίας:

    grant all on scott.inventory to ggate;

Στη συνέχεια, δημιουργία και ενεργοποίηση μιας βάσης δεδομένων έναυσμα, INVENTORY_CDR_TRG, στον πίνακα που μόλις δημιουργήθηκε για να βεβαιωθείτε ότι όλες οι συναλλαγές με τον νέο πίνακα καταγράφονται εάν ο χρήστης δεν είναι ggate. Εκτέλεση της λειτουργίας σε τοποθεσία Α και β τοποθεσίας.

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    /


##<a name="2-prepare-site-a-and-site-b-for-database-replication"></a>2. προετοιμασία τοποθεσία A και B τοποθεσίας για αναπαραγωγή βάσης δεδομένων
Η ενότητα αυτή εξηγεί τον τρόπο προετοιμασίας τοποθεσία A και B τοποθεσίας για αναπαραγωγή βάσης δεδομένων. Πρέπει να εκτελέσετε όλα τα βήματα αυτής της ενότητας στις δύο τοποθεσίες: τοποθεσία Α και β τοποθεσίας.

Πρώτη απομακρυσμένη επιφάνεια εργασίας, τοποθεσία A και B τοποθεσίας μέσω Azure κλασική πύλη. Μετάβαση στη βάση δεδομένων σε κατάσταση λειτουργίας archivelog χρησιμοποιώντας το SQL * συν παράθυρο γραμμής εντολών:

    sql>shutdown immediate
    sql>startup mount
    sql>alter database archivelog;
    sql>alter database open;


Στη συνέχεια, ενεργοποιήστε ελάχιστη [συμπληρωματικοί καταγραφής](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm) ως εξής:

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

Στη συνέχεια, προετοιμασία της βάσης δεδομένων για την υποστήριξη αναπαραγωγής DDL (γλώσσα ορισμού δεδομένων):

    SQL> alter system set recyclebin=off scope=spfile;

Στη συνέχεια, Τερματισμός και επανεκκίνηση της βάσης δεδομένων:

    sql>shutdown immediate
    sql>startup


##<a name="3-create-all-necessary-objects-to-support-ddl-replication"></a>3. Δημιουργία όλα τα αντικείμενα είναι απαραίτητο για την υποστήριξη DDL αναπαραγωγής
Αυτή η ενότητα παραθέτει τις δέσμες ενεργειών που πρέπει να χρησιμοποιήσετε για να δημιουργήσετε όλα τα αντικείμενα είναι απαραίτητο για την υποστήριξη DDL αναπαραγωγής. Πρέπει να εκτελέσετε τις δέσμες ενεργειών που καθορίζονται σε αυτήν την ενότητα στην τοποθεσία Α και β τοποθεσίας.

Ανοίξτε μια γραμμή εντολών των Windows προς τα επάνω και μεταβείτε στο φάκελο Oracle GoldenGate, όπως C:\\OracleGG. Έναρξη SQL\*συν εντολών με δικαιώματα διαχειριστή βάσης δεδομένων, όπως χρησιμοποιώντας **SYSDBA** στην τοποθεσία Α και β τοποθεσίας.

Στη συνέχεια, εκτελέστε τα ακόλουθα δέσμης ενεργειών:

    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Εργαλείο GoldenGate Oracle απαιτεί μια σύνδεση επιπέδου πίνακα για την υποστήριξη DDL (γλώσσα ορισμού δεδομένων). Που είναι η αιτία, ενεργοποίηση συμπληρωματικοί καταγραφής σε επίπεδο πίνακα, χρησιμοποιώντας την εντολή Προσθήκη TRANDATA. Άνοιγμα του παράθυρο μεταφραστή εντολών GoldenGate Oracle, συνδεθείτε στη βάση δεδομένων, και, στη συνέχεια, εκτελέστε την εντολή Προσθήκη TRANDATA:

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##<a name="4-configure-goldengate-manager-on-site-a-and-site-b"></a>4. ρύθμισης παραμέτρων GoldenGate Manager στην τοποθεσία A και B τοποθεσίας
Ο διευθυντής GoldenGate Oracle εκτελεί ένας αριθμός συναρτήσεις, όπως τα άλλα GoldenGate διαδικασιών έναρξης, Διαχείριση αρχείου καταγραφής ίχνους και δημιουργία αναφορών.

Πρέπει να ρυθμίσετε τις παραμέτρους της διαδικασίας Oracle GoldenGate Manager τόσο στην τοποθεσία A και B. τοποθεσίας Για να το κάνετε αυτό, ακολουθήστε τα παρακάτω βήματα στην τοποθεσία Α και β τοποθεσίας.

Ανοιχτά παράθυρα εντολή παράθυρο και ξεκινήσετε το πρόγραμμα μεταγλώττισης εντολών Oracle GoldenGate:

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


Καταγράφει την περίοδο λειτουργίας GGSCI σε μια βάση δεδομένων, ώστε να μπορείτε να εκτελέσετε εντολές που επηρεάζει τη βάση δεδομένων:

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Εμφανίζει την κατάσταση και χρονικής υστέρησης (ενδεχομένως) για όλες τις διεργασίες διευθυντή, εξαγωγή και Replicat σε σύστημα:

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Ανοίξτε το αρχείο παραμέτρων χρησιμοποιώντας την εντολή ΕΠΕΞΕΡΓΑΣΊΑ ΠΑΡΑΜΈΤΡΩΝ και, στη συνέχεια, να προσαρτήσετε τις ακόλουθες πληροφορίες:

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

Εμφανίζει την κατάσταση και χρονικής υστέρησης (ενδεχομένως) για όλες τις διεργασίες διευθυντή, εξαγωγή και Replicat σε σύστημα:

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Καταγράφει την περίοδο λειτουργίας GGSCI σε μια βάση δεδομένων, ώστε να μπορείτε να εκτελέσετε εντολές που επηρεάζει τη βάση δεδομένων:

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

Ξεκινήστε τη διαδικασία διαχείρισης:

    GGSCI (HostName) 48> start manager
    Manager started.

##<a name="5-create-extract-group-and-data-pump-processes-on-site-a-and-site-b"></a>5. Δημιουργία αποσπάσματος διεργασίες ομάδας και αντλία δεδομένων στην τοποθεσία A και B τοποθεσίας

###<a name="create-extract-and-data-pump-processes-on-site-a"></a>Διεργασιών για την εξαγωγή και αντλία δεδομένων σε μια τοποθεσία

Πρέπει να δημιουργήσετε τις διαδικασίες εξαγωγή και αντλία δεδομένων στην τοποθεσία A και τοποθεσία B. απομακρυσμένης επιφάνειας εργασίας σε τοποθεσία A και B τοποθεσίας μέσω Azure κλασική πύλη. Άνοιγμα παραθύρου διερμηνείας εντολή GGSCI. Εκτελέστε τις ακόλουθες εντολές στην τοποθεσία α:

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

Ανοίξτε το αρχείο παραμέτρων χρησιμοποιώντας την εντολή ΕΠΕΞΕΡΓΑΣΊΑ ΠΑΡΑΜΈΤΡΩΝ και, στη συνέχεια, να προσαρτήσετε τις ακόλουθες πληροφορίες: GGSCI (MachineGG1) 18 > Επεξεργασία παραμέτρων ext1 ΕΞΑΓΆΓΕΤΕ ext1 αναγνωριστικό ΧΡΉΣΤΗ ggate, τον κωδικό ΠΡΌΣΒΑΣΗΣ ggate scott.inventory ΠΊΝΑΚΑ ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER, GETBEFORECOLS (KEYINCLUDING ΕΝΗΜΈΡΩΣΗ ON (prod_category, qty_in_stock, last_dml), Ενεργοποίηση των KEYINCLUDING που ΔΙΑΓΡΑΦΉ (prod_category, qty_in_stock, last_dml))

Ανοίξτε το αρχείο παραμέτρων χρησιμοποιώντας την εντολή ΕΠΕΞΕΡΓΑΣΊΑ ΠΑΡΑΜΈΤΡΩΝ και, στη συνέχεια, να προσαρτήσετε τις ακόλουθες πληροφορίες:

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-b"></a>Δημιουργία πίνακα σημείο ελέγχου GoldenGate στην τοποθεσία B

Στη συνέχεια, θα πρέπει να προσθέσετε έναν πίνακα σημείο ελέγχου στην τοποθεσία B. Για να το κάνετε αυτό, πρέπει να ανοίξει ένα παράθυρο διερμηνείας εντολή GoldenGate και να εκτελέσετε:

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Και, στη συνέχεια, προσθέστε τον πίνακα σημείο ελέγχου στη βάση δεδομένων, όπου ggate είναι ο κάτοχος του:

    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

Προσθέστε το όνομα του πίνακα σημείο ελέγχου στο αρχείο καθολικών ΜΕΤΑΒΛΗΤΏΝ στο διακομιστή προορισμού, που είναι τοποθεσία B σε αυτό το βήμα. Επεξεργαστείτε το αρχείο καθολικών ΜΕΤΑΒΛΗΤΏΝ στην τοποθεσία β:

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

Προσάρτηση, στη συνέχεια, την παράμετρο CHECKPOINTTABLE στο υπάρχον αρχείο καθολικών ΜΕΤΑΒΛΗΤΏΝ:

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Ως τελικό βήμα, αποθηκεύστε και κλείστε το αρχείο παραμέτρων καθολικών ΜΕΤΑΒΛΗΤΏΝ.


###<a name="add-replicat-on-site-b"></a>Προσθήκη REPLICAT στην τοποθεσία B
Αυτή η ενότητα περιγράφει πώς μπορείτε να προσθέσετε μια διαδικασία REPLICAT "REP2" στην τοποθεσία B.

Χρησιμοποιήστε εντολή Προσθήκη REPLICAT για να δημιουργήσετε μια ομάδα Replicat στην τοποθεσία β:

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

Ανοίξτε το αρχείο παραμέτρων χρησιμοποιώντας την εντολή ΕΠΕΞΕΡΓΑΣΊΑ ΠΑΡΑΜΈΤΡΩΝ και, στη συνέχεια, να προσαρτήσετε τις ακόλουθες πληροφορίες:

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###<a name="create-extract-and-data-pump-processes-on-site-b"></a>Διεργασιών για την εξαγωγή και αντλία δεδομένων στην τοποθεσία B

Αυτή η ενότητα περιγράφει πώς μπορείτε να δημιουργήσετε μια νέα διαδικασία εξαγωγής "EXT2" και μια νέα διαδικασία αντλία δεδομένων "DPUMP2" στην τοποθεσία β:

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

Ανοίξτε το αρχείο παραμέτρων χρησιμοποιώντας την εντολή ΕΠΕΞΕΡΓΑΣΊΑ ΠΑΡΑΜΈΤΡΩΝ και, στη συνέχεια, να προσαρτήσετε τις ακόλουθες πληροφορίες:

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

Ανοίξτε το αρχείο παραμέτρων χρησιμοποιώντας την εντολή ΕΠΕΞΕΡΓΑΣΊΑ ΠΑΡΑΜΈΤΡΩΝ και, στη συνέχεια, να προσαρτήσετε τις ακόλουθες πληροφορίες:

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-a"></a>Δημιουργία πίνακα GoldenGate σημείο ελέγχου σε μια τοποθεσία

Ανοίξτε Oracle GoldenGate εντολή διερμηνείας παράθυρο και δημιουργήστε έναν πίνακα σημείο ελέγχου:

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

Πρέπει επίσης να προσθέσετε το όνομα του πίνακα ελέγχου σημείο στο αρχείο καθολικών ΜΕΤΑΒΛΗΤΏΝ στην τοποθεσία α.

Ανοίξετε παράθυρο διερμηνείας εντολή Oracle GoldenGate και να επεξεργαστείτε το αρχείο καθολικών ΜΕΤΑΒΛΗΤΏΝ στην τοποθεσία α:

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Αποθηκεύστε και κλείστε το αρχείο παραμέτρων καθολικών ΜΕΤΑΒΛΗΤΏΝ.

###<a name="add-replicat-on-site-a"></a>Προσθήκη REPLICAT σε μια τοποθεσία

Αυτή η ενότητα περιγράφει πώς μπορείτε να προσθέσετε μια διαδικασία REPLICAT "REP1" στην τοποθεσία α.

Την παρακάτω εντολή δημιουργεί μια Replicat rep1 ομάδα με το όνομα του ίχνη και το σχετικό checkpointtable.

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

Ανοίξτε το αρχείο παραμέτρων χρησιμοποιώντας την εντολή ΕΠΕΞΕΡΓΑΣΊΑ ΠΑΡΑΜΈΤΡΩΝ και, στη συνέχεια, να προσαρτήσετε τις ακόλουθες πληροφορίες:

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### <a name="add-trandata-on-site-a-and-site-b"></a>Προσθήκη trandata στην τοποθεσία A και B τοποθεσίας

Ενεργοποίηση της καταγραφής συμπληρωματικοί σε επίπεδο πίνακα, χρησιμοποιώντας την εντολή Προσθήκη TRANDATA. Άνοιγμα του παράθυρο μεταφραστή εντολών GoldenGate Oracle, συνδεθείτε στη βάση δεδομένων, και, στη συνέχεια, εκτελέστε την εντολή Προσθήκη TRANDATA.

Σύνδεση απομακρυσμένης επιφάνειας εργασίας για να MachineGG1, ανοίξτε του Oracle GoldenGate μεταφραστή εντολών και εκτελέστε:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Σύνδεση απομακρυσμένης επιφάνειας εργασίας για να MachineGG2, ανοίξτε του Oracle GoldenGate μεταφραστή εντολών και εκτελέστε:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

Εμφάνιση πληροφοριών σχετικά με την κατάσταση του επιπέδου πίνακα συμπληρωματικοί καταγραφή:

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="add-trandata-on-site-a-and-site-b"></a>Προσθήκη trandata στην τοποθεσία A και B τοποθεσίας

Ενεργοποίηση της καταγραφής συμπληρωματικοί σε επίπεδο πίνακα, χρησιμοποιώντας την εντολή Προσθήκη TRANDATA. Άνοιγμα του παράθυρο μεταφραστή εντολών GoldenGate Oracle, συνδεθείτε στη βάση δεδομένων, και, στη συνέχεια, εκτελέστε την εντολή Προσθήκη TRANDATA.

Σύνδεση απομακρυσμένης επιφάνειας εργασίας για να MachineGG1, ανοίξτε του Oracle GoldenGate μεταφραστή εντολών και εκτελέστε:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Σύνδεση απομακρυσμένης επιφάνειας εργασίας για να MachineGG2, ανοίξτε του Oracle GoldenGate μεταφραστή εντολών και εκτελέστε:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="start-extract-and-data-pump-processes-on-site-a"></a>Έναρξη διεργασίες εξαγωγή και αντλία δεδομένων σε μια τοποθεσία

Έναρξη του ext1 διαδικασία εξαγωγής σε τοποθεσία α:

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

Έναρξη του dpump1 διαδικασία αντλία δεδομένων στην τοποθεσία α:

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
Εμφανίζονται πληροφορίες σχετικά με την εξαγωγή ext1 ομάδα: GGSCI (MachineGG1) 32 > πληροφορίες εξαγάγετε ext1 ΕΞΑΓΆΓΕΤΕ EXT1 τελευταία αποτελέσματα 2013-11-25 08:03 απόκρισης σημείο ελέγχου ΕΚΤΕΛΕΊΤΑΙ κατάστασης 00:00:00 (ενημερωμένη 00:00:02 πριν) επανάληψη των αρχείων καταγραφής 2013-11-25 08 του αρχείου καταγραφής ανάγνωση σημείο ελέγχου Oracle: 03:18 Seqno 6, ΣΆΡΩΣΗ 3230720 RBA 0.1074371 (1074371)

Εμφανίζει την κατάσταση και χρονικής υστέρησης (ενδεχομένως) για όλες τις διεργασίες διευθυντή, εξαγωγή και Replicat σε σύστημα:

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###<a name="start-extract-and-data-pump-processes-on-site-b"></a>Έναρξη διεργασίες εξαγωγή και αντλία δεδομένων στην τοποθεσία B

Ξεκινήστε το ext2 διαδικασία εξαγωγής στην τοποθεσία β:

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

Έναρξη του dpump2 διαδικασία αντλία δεδομένων στην τοποθεσία β:

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

Εμφανίζει την κατάσταση και χρονικής υστέρησης (ενδεχομένως) για όλες τις διεργασίες διευθυντή, εξαγωγή και Replicat σε σύστημα:

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### <a name="start-replicat-process-on-site-a"></a>Έναρξη διεργασίας REPLICAT σε μια τοποθεσία

Αυτή η ενότητα περιγράφει πώς μπορείτε να ξεκινήσετε τη διαδικασία REPLICAT "REP1" στην τοποθεσία α.

Ξεκινήστε τη διαδικασία Replicat στην τοποθεσία α:

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

Εμφανίζει την κατάσταση μιας ομάδας Replicat:

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###<a name="start-replicat-process-on-site-b"></a>Έναρξη διεργασίας REPLICAT στην τοποθεσία B

Αυτή η ενότητα περιγράφει τον τρόπο για να ξεκινήσετε τη διαδικασία REPLICAT "REP2" στην τοποθεσία B.

Ξεκινήστε τη διαδικασία Replicat στην τοποθεσία β:

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

Εμφανίζει την κατάσταση μιας ομάδας Replicat:

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##<a name="6-verify-the-bi-directional-replication-process"></a>6. Βεβαιωθείτε ότι η διαδικασία αναπαραγωγής διπλής κατεύθυνσης

Για να επαληθεύσετε τις παραμέτρους Oracle GoldenGate, εισαγάγετε μια γραμμή στη βάση δεδομένων στην τοποθεσία A. απομακρυσμένης επιφάνειας εργασίας σε τοποθεσία A. ανοικτό του SQL * συν παράθυρο γραμμής εντολών και εκτέλεση: SQL > Επιλέξτε όνομα από v$ βάσης δεδομένων.

    NAME
    ———
    TESTGG

    SQL> insert into inventory values  (100,’TV’,100,sysdate);

    1 row created.

    SQL> commit;

    Commit complete.

Στη συνέχεια, ελέγξτε εάν αυτήν τη γραμμή είναι αναπαραχθούν στην τοποθεσία B. Για να το κάνετε αυτό, απομακρυσμένης επιφάνειας εργασίας σε τοποθεσία B. άνοιγμα του παραθύρου SQL συν και να εκτελέσετε:

    SQL> select name from v$database;

    NAME
    ———
    TESTGG

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

Εισαγάγετε μια νέα εγγραφή στην τοποθεσία β:

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.

    SQL> commit;

    Commit complete.

Σύνδεση απομακρυσμένης επιφάνειας εργασίας σε τοποθεσία A και ελέγξτε εάν η αναπαραγωγή έχει πραγματοποιηθεί:

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##<a name="additional-resources"></a>Πρόσθετοι πόροι
[Εικόνες εικονική μηχανή Oracle για Azure](virtual-machines-linux-classic-oracle-images.md)
