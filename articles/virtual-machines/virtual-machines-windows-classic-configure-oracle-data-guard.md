<properties
    pageTitle="Ρύθμιση παραμέτρων προστασίας δεδομένων Oracle ΣΠΣ | Microsoft Azure"
    description="Ακολουθήστε τα βήματα ένα πρόγραμμα εκμάθησης για τη ρύθμιση του και την εφαρμογή προστασίας δεδομένων Oracle σε Azure εικονικές μηχανές για υψηλή διαθεσιμότητα και την καταστροφή αποκατάστασης."
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

#<a name="configuring-oracle-data-guard-for-azure"></a>Ρύθμιση παραμέτρων προστασίας δεδομένων Oracle για Azure


Αυτό το πρόγραμμα εκμάθησης παρουσιάζει πώς μπορείτε να ρυθμίσετε και να υλοποιήσετε Επιφυλακή δεδομένων Oracle σε περιβάλλον εικονικές μηχανές Windows Azure για υψηλή διαθεσιμότητα και αποκατάσταση. Το πρόγραμμα εκμάθησης εστιάζει σε μονόδρομη αναπαραγωγής για βάσεις δεδομένων RAC Oracle.

Προστασία δεδομένων Oracle υποστηρίζει προστασία δεδομένων και την αποκατάσταση για βάση δεδομένων Oracle. Είναι μια απλή, υψηλών επιδόσεων, δοχεία λύσης για αποκατάσταση, προστασία δεδομένων και υψηλή διαθεσιμότητα για ολόκληρη τη βάση δεδομένων Oracle.

Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη θεωρητική και πρακτική γνώσεων σε υψηλή διαθεσιμότητα βάση δεδομένων Oracle και αποκατάσταση έννοιες. Για πληροφορίες, ανατρέξτε στην [τοποθεσία web Oracle](http://www.oracle.com/technetwork/database/features/availability/index.html) και επίσης την [έννοια Επιφυλακή δεδομένων Oracle και τον Οδηγό διαχείρισης](https://docs.oracle.com/cd/E11882_01/server.112/e41134/toc.htm).

Επιπλέον, το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε ήδη εγκαταστήσει τις ακόλουθες προϋποθέσεις:

- Μπορείτε να αναθεωρήσετε ήδη ενότητα υψηλή διαθεσιμότητα και τα θέματα αποκατάστασης από καταστροφή στο θέμα [Oracle εικονική μηχανή εικόνες - διάφορα θέματα](virtual-machines-windows-classic-oracle-considerations.md) . Azure υποστηρίζει μεμονωμένη βάση δεδομένων της Oracle παρουσίες αλλά δεν Oracle πραγματικό εφαρμογή συμπλεγμάτων (Oracle RAC) προς το παρόν.


- Έχετε δημιουργήσει δύο εικονικές μηχανές (ΣΠΣ) στο Azure χρησιμοποιώντας την ίδια πλατφόρμα που παρέχονται εικόνα Oracle Enterprise Edition. Βεβαιωθείτε ότι το εικονικές μηχανές είναι στην [ίδια υπηρεσία στο cloud](virtual-machines-windows-load-balance.md) και στο ίδιο δίκτυο εικονικού για να βεβαιωθείτε ότι μπορούν να έχουν πρόσβαση σε άλλη επάνω από τη μόνιμη ιδιωτική διεύθυνση IP. Επιπλέον, καλό είναι να τοποθετήσετε το ΣΠΣ στο ίδιο [Ορισμός διαθεσιμότητας](virtual-machines-windows-manage-availability.md) για να επιτρέψετε Azure για να τοποθετήσετε σε ξεχωριστή σφαλμάτων τομείς και αναβάθμιση τομείς. Προστασία δεδομένων Oracle είναι διαθέσιμη μόνο με βάση δεδομένων Oracle Enterprise Edition. Κάθε υπολογιστή πρέπει να έχετε τουλάχιστον 2 GB μνήμης και 5 GB χώρου στο δίσκο. Για τις πιο πρόσφατες πληροφορίες στην πλατφόρμα που παρέχονται μεγέθη Εικονική, ανατρέξτε στο θέμα [Μεγέθη εικονική μηχανή για Azure](virtual-machines-windows-sizes.md). Εάν χρειάζεστε επιπλέον σκληρό δίσκο για το ΣΠΣ, μπορείτε να επισυνάψετε επιπλέον δίσκων. Για πληροφορίες, ανατρέξτε στο θέμα [Πώς να επισυνάψετε ένα δίσκο δεδομένων σε μια εικονική μηχανή](virtual-machines-windows-classic-attach-disk.md).



- Που έχετε ορίσει τα ονόματα εικονική μηχανή ως "Μηχάνημα_1" για την κύρια Εικονική και "Μηχάνημα_2" για την αναμονής εικονική Μηχανή στην πύλη του Azure κλασική.

- Έχετε ορίσει τη μεταβλητή περιβάλλοντος **ORACLE_HOME** ώστε να οδηγούν στην ίδια διαδρομή εγκατάστασης του ριζικού oracle στην κύρια και αναμονής εικονικές μηχανές, όπως είναι οι `C:\OracleDatabase\product\11.2.0\dbhome_1\database`.

- Συνδέεστε με το διακομιστή Windows ως μέλος της ομάδας **διαχειριστών** ή μέλος της ομάδας **ORA_DBA** .

Σε αυτό το πρόγραμμα εκμάθησης, θα πρέπει:

Υλοποίηση του περιβάλλοντος φυσικής αναμονής βάσης δεδομένων

1. Δημιουργήστε μια κύρια βάση δεδομένων

2. Προετοιμασία την κύρια βάση δεδομένων για δημιουργία αναμονής βάσης δεδομένων

    1. Ενεργοποίηση της καταγραφής εξαναγκασμένη

    2. Δημιουργία αρχείου κωδικού πρόσβασης

    3. Ρύθμιση παραμέτρων ενός αρχείου καταγραφής αναμονής Ακύρωση αναίρεσης

    4. Ενεργοποίηση αρχειοθέτησης

    5. Ορισμός παράμετροι προετοιμασίας κύρια βάση δεδομένων

Δημιουργία βάσης δεδομένων φυσικής αναμονής

1. Προετοιμασία αρχείου παραμέτρου προετοιμασίας για αναμονής βάσης δεδομένων

2. Ρύθμιση παραμέτρων του ακρόασης και tnsnames για την υποστήριξη της βάσης δεδομένων σε υπολογιστές πρωτεύοντα και αναμονής

    1. Ρύθμιση παραμέτρων listener.ora και στους δύο διακομιστές για τη διατήρηση καταχωρήσεις για δύο βάσεις δεδομένων

    2. Για να κρατήσετε καταχωρήσεις για βάσεις δεδομένων κύριες και αναμονής, ρυθμίστε τις παραμέτρους tnsnames.ora σε εικονικές μηχανές πρωτεύοντα και αναμονής. 

    3. Ξεκινήστε το ακροατήριο και ελέγξτε tnsping σε δύο εικονικές μηχανές με τις υπηρεσίες και τις δύο.

3. Εκκίνηση του την παρουσία αναμονής σε κατάσταση nomount

4. Χρήση RMAN για να αντιγράψετε τη βάση δεδομένων και για να δημιουργήσετε μια βάση δεδομένων αναμονής

5. Εκκίνηση της φυσικής αναμονής βάσης δεδομένων σε κατάσταση λειτουργίας διαχειριζόμενων αποκατάστασης

6. Επαλήθευση της φυσικής αναμονής βάσης δεδομένων

> [AZURE.IMPORTANT] Αυτό το πρόγραμμα εκμάθησης έχει ρύθμιση και δοκιμή σύμφωνα με τις ακόλουθες ρυθμίσεις παραμέτρων υλικού και λογισμικού:
>
>|                      | **Κύρια βάση δεδομένων**                      | **Αναμονής βάσης δεδομένων**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Έκδοση Oracle**   | Την έκδοση Enterprise Oracle11g (11.2.0.4.0) | Την έκδοση Enterprise Oracle11g (11.2.0.4.0) |
>| **Το όνομα του υπολογιστή**     | Μηχάνημα_1                                  | Μηχάνημα_2                                  |
>| **Λειτουργικό σύστημα** | Windows 2008 R2                           | Windows 2008 R2                           |
>| **Oracle αναγνωριστικό ΑΣΦΑΛΕΊΑΣ**       | ΈΛΕΓΧΟΣ                                      | ΔΟΚΙΜΉ\_STBY                                |
>| **Μνήμη**           | Min 2 GB                                  | Min 2 GB                                  |
>| **Χώρος στο δίσκο**       | Min 5 GB                                  | Min 5 GB                                  |

Οι επόμενες εκδόσεις της βάσης δεδομένων Oracle και Επιφυλακή δεδομένων Oracle, ενδέχεται να υπάρχουν ορισμένες πρόσθετες αλλαγές που χρειάζεστε για την υλοποίηση. Για τις πιο πρόσφατες πληροφορίες συγκεκριμένη έκδοση, ανατρέξτε στην τεκμηρίωση [Επιφυλακή δεδομένων](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) και τη [Βάση δεδομένων Oracle](http://www.oracle.com/us/corporate/features/database-12c/index.html) στην τοποθεσία web της Oracle.

##<a name="implement-the-physical-standby-database-environment"></a>Υλοποίηση του περιβάλλοντος φυσικής αναμονής βάσης δεδομένων
### <a name="1-create-a-primary-database"></a>1. Δημιουργήστε μια κύρια βάση δεδομένων

- Δημιουργήστε μια κύρια βάση δεδομένων "ΔΟΚΙΜΉ" στο την κύρια εικονική μηχανή. Για πληροφορίες, ανατρέξτε στο θέμα Δημιουργία και ρύθμιση παραμέτρων σε μια βάση δεδομένων Oracle.
- Για να δείτε το όνομα της βάσης δεδομένων σας, συνδεθείτε με τη βάση δεδομένων ως ο χρήστης Συστήματος με ρόλο SYSDBA στο SQL * συν εντολών και εκτελέστε την ακόλουθη πρόταση:

        SQL> select name from v$database;

        The result will display like the following:

        NAME
        ---------
        TEST
- Στη συνέχεια, ερωτήματος τα ονόματα των αρχείων της βάσης δεδομένων από την προβολή συστήματος dba_data_files:

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### <a name="2-prepare-the-primary-database-for-standby-database-creation"></a>2. προετοιμασία την κύρια βάση δεδομένων για δημιουργία αναμονής βάσης δεδομένων

Πριν να δημιουργήσετε μια βάση δεδομένων αναμονής, συνιστάται να διασφαλίσετε την κύρια βάση δεδομένων έχει ρυθμιστεί σωστά. Ακολουθεί μια λίστα με τα βήματα που πρέπει να εκτελέσετε:

1. Ενεργοποίηση της καταγραφής εξαναγκασμένη
2. Δημιουργία αρχείου κωδικού πρόσβασης
3. Ρύθμιση παραμέτρων ενός αρχείου καταγραφής αναμονής Ακύρωση αναίρεσης
4. Ενεργοποίηση αρχειοθέτησης
5. Ορισμός παράμετροι προετοιμασίας κύρια βάση δεδομένων

#### <a name="enable-forced-logging"></a>Ενεργοποίηση της καταγραφής εξαναγκασμένη

Για να υλοποιήσετε μια βάση δεδομένων σε αναμονή, πρέπει να ενεργοποιήσετε την 'Καταγραφή υποχρεωτική' στην κύρια βάση δεδομένων. Αυτή η επιλογή εξασφαλίζει ότι ακόμα και αν ολοκληρωθεί μια λειτουργία 'nologging', ισχύ καταγραφής έχει προτεραιότητα και όλες οι λειτουργίες έχετε συνδεθεί στο τα αρχεία καταγραφής Ακύρωση αναίρεσης. Γι ' αυτό, θα σας βεβαιωθείτε ότι όλα τα στοιχεία στην κύρια βάση δεδομένων είναι συνδεδεμένος και να την κατάσταση αναμονής αναπαραγωγή περιλαμβάνει όλες τις εργασίες στην κύρια βάση δεδομένων. Για να ενεργοποιήσετε την καταγραφή ισχύ, εκτελέστε την πρόταση ALTER βάσης δεδομένων:

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### <a name="create-a-password-file"></a>Δημιουργία αρχείου κωδικού πρόσβασης

Για να μπορέσετε να αποστείλετε και να εφαρμόσετε αρχειοθετημένα αρχεία καταγραφής από το κύριο διακομιστή στο διακομιστή σε αναμονή, πρέπει να είναι πανομοιότυπες στους διακομιστές πρωτεύοντα και αναμονής τον κωδικό πρόσβασης του συστήματος. Λόγο που δημιουργείτε ένα αρχείο τον κωδικό πρόσβασης στο την κύρια βάση δεδομένων και αντιγραφή του στο διακομιστή σε αναμονή.

>[AZURE.IMPORTANT] Όταν χρησιμοποιείτε τη βάση δεδομένων της Oracle 12c, υπάρχει ένα νέο χρήστη, **SYSDG**, που μπορείτε να χρησιμοποιήσετε για να διαχειριστείτε Επιφυλακή δεδομένων Oracle. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αλλαγές στη βάση δεδομένων Oracle 12 c κυκλοφορίας](http://docs.oracle.com/database/121/UNXAR/release_changes.htm#UNXAR404).

Επιπλέον, βεβαιωθείτε ότι το ORACLE\_ΚΕΝΤΡΙΚΉ περιβάλλον έχει ήδη οριστεί σε Μηχάνημα_1. Εάν δεν είναι, ορίστε το ως μια μεταβλητή περιβάλλοντος χρησιμοποιώντας το πλαίσιο διαλόγου μεταβλητές περιβάλλοντος. Για να αποκτήσετε πρόσβαση σε αυτό το πλαίσιο διαλόγου, ξεκινήστε το βοηθητικό πρόγραμμα **συστήματος** , κάνοντας διπλό κλικ στο εικονίδιο συστήματος στον **Πίνακα ελέγχου**; στη συνέχεια, κάντε κλικ στην καρτέλα **για προχωρημένους** και επιλέξτε **Μεταβλητές περιβάλλοντος**. Για να ορίσετε τις μεταβλητές περιβάλλοντος, κάντε κλικ στο κουμπί **Δημιουργία** στην περιοχή **Μεταβλητές συστήματος**. Μετά τη ρύθμιση των μεταβλητών περιβάλλοντος, κλείστε την υπάρχουσα γραμμή εντολών των Windows και ανοίξτε ένα νέο.

Εκτελέστε την ακόλουθη πρόταση για να μεταβείτε σε της Oracle\_κεντρική καταλόγου, όπως C:\\OracleDatabase\\προϊόντος\\11.2.0\\dbhome\_1\\βάσης δεδομένων.

    cd %ORACLE_HOME%\database

Στη συνέχεια, δημιουργήστε ένα αρχείο κωδικού πρόσβασης χρήση του κωδικού πρόσβασης αρχείο δημιουργίας βοηθητικού προγράμματος, [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm). Στο την ίδια γραμμή εντολών των Windows σε Μηχάνημα_1, εκτελέστε την ακόλουθη εντολή, ορίζοντας την τιμή του κωδικού πρόσβασης με τον κωδικό πρόσβασης του **Συστήματος**:

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

Αυτή η εντολή δημιουργεί ένα αρχείο τον κωδικό πρόσβασης, που ονομάζεται ως PWDTEST.ora, στο της ORACLE\_για οικιακή ΧΡΉΣΗ\\κατάλογο βάσης δεδομένων. Θα πρέπει να αντιγράψετε αυτό το αρχείο σε % ORACLE\_ΚΕΝΤΡΙΚΉ %\\βάσης δεδομένων καταλόγου στο Μηχάνημα_2 με μη αυτόματο τρόπο.

#### <a name="configure-a-standby-redo-log"></a>Ρύθμιση παραμέτρων ενός αρχείου καταγραφής αναμονής Ακύρωση αναίρεσης

Στη συνέχεια, πρέπει να ρυθμίσετε μια αναμονής καταγραφής επανάληψη, έτσι ώστε η κύρια μπορεί να λάβει την ακύρωση αναίρεσης σωστά όταν γίνεται μια κατάσταση αναμονής. Πριν από τη δημιουργία τους εδώ σας επιτρέπει επίσης τα αρχεία καταγραφής Ακύρωση αναίρεσης αναμονής, θα δημιουργηθεί αυτόματα κατά την αναμονή. Είναι σημαντικό να ρυθμίσετε τις παραμέτρους για το πρωτόκολλο αναμονής επανάληψη των αρχείων καταγραφής (SRL) με το ίδιο μέγεθος, όπως την ηλεκτρονική επανάληψη των αρχείων καταγραφής. Το μέγεθος των αρχείων καταγραφής αναμονής Ακύρωση αναίρεσης τρέχουσα πρέπει να αντιστοιχεί ακριβώς το μέγεθος του τα τρέχοντα αρχεία καταγραφής online Ακύρωση αναίρεσης κύρια βάση δεδομένων.

Εκτελέστε την ακόλουθη πρόταση στο SQL\*συν εντολών στο Μηχάνημα_1. Το αρχείο καταγραφής $ v είναι μια προβολή συστήματος που περιέχει πληροφορίες σχετικά με τα αρχεία καταγραφής Ακύρωση αναίρεσης.

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800


Σημειώστε ότι 52428800 είναι 50 megabyte.

Στη συνέχεια, στο SQL\*συν το παράθυρο, εκτελέστε τις ακόλουθες προτάσεις για να προσθέσετε μια νέα ομάδα αρχείο καταγραφής αναμονής Ακύρωση αναίρεσης αναμονής βάσης δεδομένων και να καθορίσετε έναν αριθμό που προσδιορίζει την ομάδα χρησιμοποιώντας τον όρο ΟΜΆΔΑΣ. Χρήση αριθμών ομάδα να κάνετε τη διαχείριση ομάδων αρχείων καταγραφής αναμονής Ακύρωση αναίρεσης πιο εύκολη:

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

Στη συνέχεια, εκτελέστε την ακόλουθη προβολή συστήματος για μια λίστα πληροφοριών σχετικά με την ακύρωση αναίρεσης αρχεία καταγραφής. Αυτή η λειτουργία επαληθεύει επίσης ότι οι ομάδες αρχείο καταγραφής αναμονής Ακύρωση αναίρεσης δημιουργήθηκαν:

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### <a name="enable-archiving"></a>Ενεργοποίηση αρχειοθέτησης

Στη συνέχεια, ενεργοποιήστε αρχειοθέτηση, εκτελώντας τις παρακάτω προτάσεις για να τοποθετήσετε την κύρια βάση δεδομένων σε λειτουργία ARCHIVELOG και να ενεργοποιήσετε την αυτόματη αρχειοθέτηση. Μπορείτε να ενεργοποιήσετε αρχειοθέτησης λειτουργία καταγραφής προσαρμογής της βάσης δεδομένων και, στη συνέχεια, εκτέλεση της εντολής archivelog.

Πρώτα, συνδεθείτε ως sysdba. Στη γραμμή εντολών των Windows, εκτελέστε:

    sqlplus /nolog

    connect / as sysdba

Στη συνέχεια, τον τερματισμό της βάσης δεδομένων στο SQL\*συν εντολών:

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

Στη συνέχεια, εκτελέστε την εντολή ενεργοποίησης εκκίνησης για να ενεργοποίησης της βάσης δεδομένων. Αυτό εξασφαλίζει ότι Oracle συσχετίζει την παρουσία με την καθορισμένη βάση δεδομένων.

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

Στη συνέχεια, εκτελέστε:

    SQL> alter database archivelog;
    Database altered.

Στη συνέχεια, εκτελέστε την πρόταση Alter δήλωση βάσης δεδομένων με το άνοιγμα όρο WHERE για να κάνετε διαθέσιμη για κανονική χρήση της βάσης δεδομένων:

    SQL> alter database open;

    Database altered.

#### <a name="set-primary-database-initialization-parameters"></a>Ορισμός παράμετροι προετοιμασίας κύρια βάση δεδομένων

Για να ρυθμίσετε τις παραμέτρους του Επιφυλακή δεδομένων, πρέπει να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους αναμονής σε μια κανονική pfile (αρχείο παραμέτρων προετοιμασίας κειμένου) πρώτη. Όταν το pfile είναι έτοιμη, πρέπει να τον μετατρέψετε σε ένα αρχείο παραμέτρων διακομιστή (SPFILE).

Μπορείτε να ελέγχετε το περιβάλλον Επιφυλακή δεδομένων χρησιμοποιώντας τις παραμέτρους του την Προετοιμασία. Αρχείο ORA. Όταν ακολουθείτε αυτό το πρόγραμμα εκμάθησης, πρέπει να ενημερώσετε την κύρια βάση δεδομένων Προετοιμασίας. ORA έτσι ώστε να μπορεί να περιέχει δύο ρόλους: κύρια ή αναμονής.

    SQL> create pfile from spfile;
    File created.

Στη συνέχεια, πρέπει να επεξεργαστείτε το pfile για να προσθέσετε τις παραμέτρους αναμονής. Για να κάνετε αυτό, ανοίξτε το INITTEST. ORA αρχείου στη θέση % ORACLE\_ΚΕΝΤΡΙΚΉ %\\βάσης δεδομένων. Στη συνέχεια, να προσαρτήσετε τις παρακάτω προτάσεις στο αρχείο INITTEST.ora. Τους κανόνες ονοματοθεσίας για την Προετοιμασία σας. Αρχείο ORA είναι Προετοιμασία\<YourDatabaseName\>. ORA.

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


Το προηγούμενο μπλοκ δήλωση περιλαμβάνει τρία ρύθμισης σημαντικά στοιχεία:
-   **LOG_ARCHIVE_CONFIG...:** Μπορείτε να καθορίσετε τα αναγνωριστικά μοναδικό βάσης δεδομένων χρησιμοποιώντας αυτήν τη δήλωση.
-   **LOG_ARCHIVE_DEST_1...:** Μπορείτε να καθορίσετε τη θέση του φακέλου αρχειοθέτησης τοπικό χρησιμοποιώντας αυτήν τη δήλωση. Συνιστάται να δημιουργήσετε έναν νέο κατάλογο για τις ανάγκες αρχειοθέτησης της βάσης δεδομένων σας και καθορίστε τη θέση του τοπικού αρχειοθέτησης χρησιμοποιώντας αυτήν τη δήλωση ρητά και όχι με χρήση της Oracle προεπιλεγμένο φάκελο % ORACLE_HOME%\database\archive.
-   **LOG_ARCHIVE_DEST_2... ΑΣΎΓΧΡΟΝΗ LGWR...:** μπορείτε να καθορίσετε μια διαδικασία writer ασύγχρονης log (LGWR) για να συλλέξετε δεδομένα Ακύρωση αναίρεσης συναλλαγής και το μετάδοσης σε αναμονής προορισμούς. Εδώ, η DB_UNIQUE_NAME καθορίζει ένα μοναδικό όνομα για τη βάση δεδομένων στο διακομιστή αναμονής προορισμού.

Όταν είστε έτοιμοι το νέο αρχείο παραμέτρων, πρέπει να δημιουργήσετε το spfile από αυτό.

Αρχικά, τερματισμός της βάσης δεδομένων:

    SQL> shutdown immediate;

    Database closed.

    Database dismounted.

    ORACLE instance shut down.

Στη συνέχεια, εκτελέστε την εντολή nomount εκκίνησης ως εξής:

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

Τώρα, δημιουργήστε μια spfile:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

Στη συνέχεια, τερματισμός της βάσης δεδομένων:

    SQL> shutdown immediate;

    ORA-01507: database not mounted

Στη συνέχεια, χρησιμοποιήστε την εντολή εκκίνησης για να ξεκινήσετε μια περίοδο λειτουργίας:

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##<a name="create-a-physical-standby-database"></a>Δημιουργία βάσης δεδομένων φυσικής αναμονής
Αυτή η ενότητα εστιάζει στα βήματα που πρέπει να εκτελέσετε στο Μηχάνημα_2 για να προετοιμάσετε το φυσικό αναμονής βάσης δεδομένων.

Αρχικά, πρέπει να απομακρυσμένης επιφάνειας εργασίας για να Μηχάνημα_2 μέσω Azure κλασική πύλη.

Στη συνέχεια, στο αναμονής διακομιστή (Μηχάνημα_2), δημιουργήστε όλους τους φακέλους που είναι απαραίτητο για τη βάση δεδομένων αναμονής, όπως C:\\\<YourLocalFolder\>\\ΔΟΚΙΜΉΣ. Ενώ μετά από αυτό το πρόγραμμα εκμάθησης, βεβαιωθείτε ότι η δομή του φακέλου συμφωνεί με τη δομή των φακέλων σε Μηχάνημα_1 για να διατηρήσετε όλα τα απαραίτητα αρχεία, όπως αρχεία controlfile, datafiles, redologfiles, udump, bdump και cdump. Επιπλέον, ορισμός της ORACLE\_για οικιακή ΧΡΉΣΗ και ORACLE\_μεταβλητές ΒΆΣΗΣ περιβάλλοντος σε Μηχάνημα_2. Εάν δεν είναι, καθορίστε τους ως μια μεταβλητή περιβάλλοντος χρησιμοποιώντας το πλαίσιο διαλόγου μεταβλητές περιβάλλοντος. Για να αποκτήσετε πρόσβαση σε αυτό το πλαίσιο διαλόγου, ξεκινήστε το βοηθητικό πρόγραμμα **συστήματος** , κάνοντας διπλό κλικ στο εικονίδιο συστήματος στον **Πίνακα ελέγχου**; στη συνέχεια, κάντε κλικ στην καρτέλα **για προχωρημένους** και επιλέξτε **Μεταβλητές περιβάλλοντος**. Για να ορίσετε τις μεταβλητές περιβάλλοντος, κάντε κλικ στο κουμπί **Δημιουργία** στην περιοχή τις **Μεταβλητές συστήματος**. Μετά τη ρύθμιση των μεταβλητών περιβάλλοντος, πρέπει να κλείσετε την υπάρχουσα γραμμή εντολών των Windows και ανοίξτε ένα νέο για να δείτε τις αλλαγές.

Στη συνέχεια, ακολουθήστε τα παρακάτω βήματα:

1. Προετοιμασία αρχείου παραμέτρου προετοιμασίας για αναμονής βάσης δεδομένων

2. Ρύθμιση παραμέτρων του ακρόασης και tnsnames για την υποστήριξη της βάσης δεδομένων σε υπολογιστές πρωτεύοντα και αναμονής

    1. Ρύθμιση παραμέτρων listener.ora και στους δύο διακομιστές για τη διατήρηση καταχωρήσεις για δύο βάσεις δεδομένων

    2. Ρύθμιση παραμέτρων tnsnames.ora στο τις εικονικές μηχανές πρωτεύοντα και αναμονής για τη διατήρηση εγγραφές για τις κύριες και αναμονής βάσεις δεδομένων

    3. Ξεκινήστε το ακροατήριο και ελέγξτε tnsping σε δύο εικονικές μηχανές με τις υπηρεσίες και τις δύο.

3. Εκκίνηση του την παρουσία αναμονής σε κατάσταση nomount

4. Χρήση RMAN για να αντιγράψετε τη βάση δεδομένων και για να δημιουργήσετε μια βάση δεδομένων αναμονής

5. Εκκίνηση της φυσικής αναμονής βάσης δεδομένων σε κατάσταση λειτουργίας διαχειριζόμενων αποκατάστασης

6. Επαλήθευση της φυσικής αναμονής βάσης δεδομένων

### <a name="1-prepare-an-initialization-parameter-file-for-standby-database"></a>1. προετοιμάστε ένα αρχείο παραμέτρων προετοιμασίας για αναμονής βάση δεδομένων

Αυτή η ενότητα παρουσιάζει τον τρόπο για να προετοιμάσετε ένα αρχείο παραμέτρων προετοιμασίας για τη βάση δεδομένων αναμονής. Για να το κάνετε αυτό, πρέπει πρώτα να αντιγράψετε τα INITTEST. ORA αρχείων από τον υπολογιστή 1 για να Μηχάνημα_2 με μη αυτόματο τρόπο. Θα πρέπει να μπορείτε να δείτε το INITTEST. ORA αρχείων στο % ORACLE\_ΚΕΝΤΡΙΚΉ %\\φάκελος βάσης δεδομένων σε δύο υπολογιστές. Στη συνέχεια, να τροποποιήσετε το αρχείο INITTEST.ora στο Μηχάνημα_2 για να ρυθμίσετε για χρήση με το ρόλο αναμονής όπως καθορίζεται παρακάτω:

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'


    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


Του προηγούμενου μπλοκ δήλωση περιλαμβάνει δύο ρύθμισης σημαντικά στοιχεία:

-   **\*. LOG_ARCHIVE_DEST_1:** πρέπει να δημιουργήσετε το φάκελο c:\OracleDatabase\TEST_STBY\archives σε Μηχάνημα_2 με μη αυτόματο τρόπο.
-   **\*. LOG_ARCHIVE_DEST_2:** αυτό το βήμα είναι προαιρετικό. Θα ορίσετε ως ίσως φανούν χρήσιμες όταν το κύριο υπολογιστή βρίσκεται σε συντήρηση και αναμονής υπολογιστή γίνει μια κύρια βάση δεδομένων.

Στη συνέχεια, θα πρέπει να ξεκινήσετε την παρουσία αναμονής. Στο διακομιστή βάσης δεδομένων αναμονής, πληκτρολογήστε την παρακάτω εντολή στη γραμμή εντολών των Windows για να δημιουργήσετε μια παρουσία Oracle, δημιουργώντας μια υπηρεσία των Windows:

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

Η εντολή **Oradim** δημιουργεί μια παρουσία Oracle αλλά δεν ξεκινά. Μπορείτε να το βρείτε στη μονάδα δίσκου C:\\OracleDatabase\\προϊόντος\\11.2.0\\dbhome\_1\\κατάλογος ΚΆΔΟΥ.

##<a name="configure-the-listener-and-tnsnames-to-support-the-database-on-primary-and-standby-machines"></a>Ρύθμιση παραμέτρων του ακρόασης και tnsnames για την υποστήριξη της βάσης δεδομένων σε υπολογιστές πρωτεύοντα και αναμονής
Πριν να δημιουργήσετε μια βάση δεδομένων αναμονής, πρέπει να βεβαιωθείτε ότι η κύρια και αναμονής βάσεις δεδομένων στη ρύθμιση παραμέτρων σας μπορούν να επικοινωνούν μεταξύ τους. Για να κάνετε αυτό, πρέπει να ρυθμίσετε το ακροατήριο και TNSNames είτε με μη αυτόματο τρόπο είτε χρησιμοποιώντας το βοηθητικό πρόγραμμα ρύθμισης παραμέτρων δικτύου NETCA. Αυτή είναι μια υποχρεωτική εργασία όταν χρησιμοποιείτε το βοηθητικό πρόγραμμα Διαχείριση αποκατάστασης (RMAN).

### <a name="configure-listenerora-on-both-servers-to-hold-entries-for-both-databases"></a>Ρύθμιση παραμέτρων listener.ora και στους δύο διακομιστές για τη διατήρηση καταχωρήσεις για δύο βάσεις δεδομένων

Σύνδεση απομακρυσμένης επιφάνειας εργασίας για να Μηχάνημα_1 και επεξεργαστείτε το αρχείο listener.ora ως που καθορίζονται παρακάτω. Πάντα όταν επεξεργάζεστε το αρχείο listener.ora, βεβαιωθείτε ότι το άνοιγμα και τη δεξιά παρένθεση ευθυγραμμίσετε στην ίδια στήλη. Μπορείτε να βρείτε το αρχείο listener.ora στον παρακάτω φάκελο: c:\\OracleDatabase\\προϊόντος\\11.2.0\\dbhome\_1\\ΔΙΚΤΎΟΥ\\ΔΙΑΧΕΊΡΙΣΗΣ\\.

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

Επόμενη, απομακρυσμένης επιφάνειας εργασίας για να Μηχάνημα_2 και επεξεργαστείτε το listener.ora αρχείου ως εξής: # listener.ora αρχείο ρύθμισης παραμέτρων δικτύου: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### <a name="configure-tnsnamesora-on-the-primary-and-standby-virtual-machines-to-hold-entries-for-both-primary-and-standby-databases"></a>Ρύθμιση παραμέτρων tnsnames.ora στο τις εικονικές μηχανές πρωτεύοντα και αναμονής για τη διατήρηση εγγραφές για τις κύριες και αναμονής βάσεις δεδομένων

Σύνδεση απομακρυσμένης επιφάνειας εργασίας για να Μηχάνημα_1 και επεξεργασία του αρχείουTnsnames.ora ως που καθορίζονται παρακάτω. Μπορείτε να βρείτε του αρχείουTnsnames.ora στον παρακάτω φάκελο: c:\\OracleDatabase\\προϊόντος\\11.2.0\\dbhome\_1\\ΔΙΚΤΎΟΥ\\ΔΙΑΧΕΊΡΙΣΗΣ\\.

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Σύνδεση απομακρυσμένης επιφάνειας εργασίας για να Μηχάνημα_2 και επεξεργαστείτε το tnsnames.ora αρχείου ως εξής:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### <a name="start-the-listener-and-check-tnsping-on-both-virtual-machines-to-both-services"></a>Ξεκινήστε το ακροατήριο και ελέγξτε tnsping σε δύο εικονικές μηχανές με τις υπηρεσίες και τις δύο.

Ανοίξτε μια νέα γραμμή εντολών των Windows σε κύριες και αναμονής εικονικές μηχανές Windows και να εκτελέσετε τις παρακάτω προτάσεις:

    C:\Users\DBAdmin>tnsping test

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)


    C:\Users\DBAdmin>tnsping test_stby

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##<a name="start-up-the-standby-instance-in-nomount-state"></a>Εκκίνηση του την παρουσία αναμονής σε κατάσταση nomount
Για να ρυθμίσετε το περιβάλλον για την υποστήριξη της αναμονής βάσης δεδομένων στον υπολογιστή αναμονής εικονικές (Μηχάνημα_2).

Πρώτα, αντιγράψτε το αρχείο κωδικού πρόσβασης από τον πρωτεύοντα υπολογιστή (Μηχάνημα_1) στον υπολογιστή αναμονής (Μηχάνημα_2) με μη αυτόματο τρόπο. Αυτό είναι απαραίτητο, όπως τον κωδικό πρόσβασης του **συστήματος** πρέπει να είναι ίδια και στους δύο υπολογιστές.

Στη συνέχεια, ανοίξτε τη γραμμή εντολών των Windows σε Μηχάνημα_2 και εγκατάστασης τις μεταβλητές περιβάλλοντος για να τοποθετήστε το δείκτη στη βάση δεδομένων του αναμονής ως εξής:

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

Στη συνέχεια, ξεκινήστε τη βάση δεδομένων σε αναμονή σε κατάσταση nomount και, στη συνέχεια, να δημιουργήσετε μια spfile.

Εκκίνηση της βάσης δεδομένων:

    SQL>shutdown immediate;

    SQL>startup nomount
    ORACLE instance started.

    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##<a name="use-rman-to-clone-the-database-and-to-create-a-standby-database"></a>Χρήση RMAN για να αντιγράψετε τη βάση δεδομένων και για να δημιουργήσετε μια βάση δεδομένων αναμονής
Μπορείτε να χρησιμοποιήσετε το βοηθητικό πρόγραμμα Διαχείριση αποκατάστασης (RMAN) για να οποιαδήποτε αντίγραφο ασφαλείας της την κύρια βάση δεδομένων για να δημιουργήσετε τη βάση δεδομένων φυσικής αναμονής.

Σύνδεση απομακρυσμένης επιφάνειας εργασίας για να το αναμονής εικονική μηχανή (Μηχάνημα_2) και εκτελέστε το βοηθητικό πρόγραμμα RMAN, καθορίζοντας μια πλήρη σύνδεση συμβολοσειράς για δύο ΠΡΟΟΡΙΣΜΟΎ (κύρια βάση δεδομένων, Μηχάνημα_1) και βοηθητικό ΣΤΟΙΧΕΊΟ (αναμονής βάση δεδομένων, Μηχάνημα_2) παρουσίες.

>[AZURE.IMPORTANT] Μην χρησιμοποιείτε τον έλεγχο ταυτότητας λειτουργικό σύστημα καθώς δεν υπάρχει καμία βάση δεδομένων στον υπολογιστή του διακομιστή αναμονής ακόμη.

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY

    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##<a name="start-the-physical-standby-database-in-managed-recovery-mode"></a>Εκκίνηση της φυσικής αναμονής βάσης δεδομένων σε κατάσταση λειτουργίας διαχειριζόμενων αποκατάστασης
Αυτό το πρόγραμμα εκμάθησης παρουσιάζει πώς μπορείτε να δημιουργήσετε μια βάση δεδομένων φυσικής αναμονής. Για πληροφορίες σχετικά με τη δημιουργία μιας βάσης δεδομένων λογική αναμονής, ανατρέξτε στην τεκμηρίωση Oracle.

Άνοιγμα του SQL\*Plus εντολών και να ενεργοποιήσετε την προστασία δεδομένων στον αναμονής εικονική μηχανή ή το διακομιστή (Μηχάνημα_2) ως εξής:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Όταν ανοίγετε τη βάση δεδομένων αναμονής στη λειτουργία **ΕΝΕΡΓΟΠΟΊΗΣΗΣ** , το αρχείο καταγραφής αρχειοθέτησης συνεχίζει αποστολής και η διαδικασία διαχειριζόμενων αποκατάστασης συνεχίζει καταγραφής εφαρμογή αναμονής βάση δεδομένων. Αυτό εξασφαλίζει ότι η βάση δεδομένων αναμονής παραμένει ενημερωμένο με την κύρια βάση δεδομένων. Σημειώστε ότι η βάση δεδομένων αναμονής δεν μπορεί να είναι προσβάσιμα για σκοπούς αναφοράς σε αυτό το διάστημα.

Όταν ανοίγετε τη βάση δεδομένων αναμονής στη λειτουργία **ΑΝΆΓΝΩΣΗΣ ΜΌΝΟ** , την αρχειοθέτηση συνδεθείτε αποστολής συνεχίζει. Αλλά σταματά η διαδικασία διαχειριζόμενων αποκατάστασης. Αυτό προκαλεί τη βάση δεδομένων αναμονής για να γίνετε αυξανόμενη λήξει μέχρι να διαχειριζόμενων αποκατάστασης επαναληφθεί. Μπορείτε να αποκτήσετε πρόσβαση στη βάση δεδομένων αναμονής για σκοπούς δημιουργίας αναφορών σε αυτό το διάστημα, αλλά δεδομένων μπορεί να μην παρουσιάζει τις πιο πρόσφατες αλλαγές.

Σε γενικές γραμμές, συνιστάται να διατηρείτε τη βάση δεδομένων αναμονής στη λειτουργία **ΕΝΕΡΓΟΠΟΊΗΣΗΣ** για να διατηρήσετε τα δεδομένα στη βάση δεδομένων αναμονής ενημερωμένο εάν υπάρχει μια αποτυχία από την κύρια βάση δεδομένων. Ωστόσο, μπορείτε να διατηρήσετε την αναμονής βάση δεδομένων σε λειτουργία **ΑΝΆΓΝΩΣΗΣ ΜΌΝΟ** για σκοπούς αναφοράς ανάλογα με τις απαιτήσεις της εφαρμογής σας. Τα παρακάτω βήματα δείχνουν πώς μπορείτε να ενεργοποιήσετε την προστασία δεδομένων σε λειτουργία μόνο για ανάγνωση με χρήση SQL\*συν:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##<a name="verify-the-physical-standby-database"></a>Επαλήθευση της φυσικής αναμονής βάσης δεδομένων
Αυτή η ενότητα παρουσιάζει πώς μπορείτε να επαληθεύσετε τις παραμέτρους υψηλή διαθεσιμότητα ως διαχειριστής.

Άνοιγμα του SQL\*Plus παράθυρο γραμμής εντολών και επιλέξτε αρχειοθετηθούν επανάληψη καταγραφής σε αναμονή εικονικές υπολογιστή (Μηχάνημα_2):

    SQL> show parameters db_unique_name;

    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY

    SQL> SELECT NAME FROM V$DATABASE

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

Άνοιγμα του SQL * συν logfiles παράθυρο και ο διακόπτης γραμμή εντολών στον πρωτεύοντα υπολογιστή (Μηχάνημα_1):

    SQL> alter system switch logfile;
    System altered.

    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

Αρχείο καταγραφής Επανάληψη ελέγχου αρχειοθετηθούν σε αναμονή εικονικές υπολογιστή (Μηχάνημα_2):

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES

    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

Έλεγχος για οποιοδήποτε κενό σε αναμονή εικονικές υπολογιστή (Μηχάνημα_2):

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

Μια άλλη μέθοδος επαλήθευσης θα μπορούσε να είναι να ανακατεύθυνσης στη βάση δεδομένων του αναμονής και, στη συνέχεια, ελέγξτε εάν είναι δυνατή η αποκατάσταση μετά από την κύρια βάση δεδομένων. Για να ενεργοποιήσετε το αναμονής βάσης δεδομένων ως κύρια βάση δεδομένων, χρησιμοποιήστε τις παρακάτω προτάσεις:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Εάν δεν έχετε ενεργοποιήσει flashback αρχική κύρια βάση δεδομένων, συνιστάται να που αποθέστε την αρχική κύρια βάση δεδομένων και να δημιουργήσετε ξανά με μια βάση δεδομένων της αναμονής.

Συνιστάται η ενεργοποίηση της βάσης δεδομένων flashback στην κύρια και των βάσεων δεδομένων αναμονής. Όταν συμβαίνει ανακατεύθυνσης, την κύρια βάση δεδομένων μπορεί να είναι αναβοσβήνει ξανά για να την ώρα πριν από την ανακατεύθυνση και γρήγορα μετατρέπονται σε μια βάση δεδομένων αναμονής.

##<a name="additional-resources"></a>Πρόσθετοι πόροι
[Εικόνες εικονική μηχανή Oracle για Azure](virtual-machines-windows-classic-oracle-images.md)