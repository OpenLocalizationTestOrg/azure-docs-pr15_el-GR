<properties
    pageTitle="Ρύθμιση του PostgreSQL σε μια Εικονική Linux | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους PostgreSQL σε μια εικονική μηχανή Linux στο Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


# <a name="install-and-configure-postgresql-on-azure"></a>Εγκατάσταση και ρύθμιση παραμέτρων PostgreSQL στο Azure

PostgreSQL είναι για προχωρημένους ανοιχτού κώδικα βάση παρόμοια με Oracle και DB2. Περιλαμβάνει ετοιμότητα για μεγάλες επιχειρήσεις δυνατότητες όπως τα πλήρη ΟΞΈΟΣ συμμόρφωσης, αξιόπιστη συναλλαγών επεξεργασίας και ταυτόχρονης εκτέλεσης πολλών έκδοσης στοιχείου ελέγχου. Επίσης, υποστηρίζει τα πρότυπα όπως ANSI SQL και SQL/MED (συμπεριλαμβανομένων των προγράμματα εξομοίωσης εξωτερικά δεδομένα για Oracle, MySQL, MongoDB και πολλά άλλα). Είναι ιδιαίτερα επεκτάσιμη με υποστήριξη για γλώσσες πάνω από 12 σχετικά με τη διαδικασία, GIN και GiST ευρετήρια, υποστήριξη του χώρου δεδομένων και NoSQL πολλές δυνατότητες για JSON ή βάσει αριθμού-κλειδιού-τιμής εφαρμογές.

Σε αυτό το άρθρο, θα μάθετε πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους PostgreSQL σε μια Azure εικονική μηχανή εκτελείται Linux.


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="install-postgresql"></a>Εγκατάσταση του PostgreSQL

> [AZURE.NOTE] Πρέπει να έχετε ήδη ένα Azure εικονική μηχανή εκτελείτε Linux για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης. Για να δημιουργήσετε και να ρυθμίσετε μια Εικονική Linux πριν να συνεχίσετε, ανατρέξτε στο θέμα το [πρόγραμμα εκμάθησης Εικονική Linux Azure](virtual-machines-linux-quick-create-cli.md).

Σε αυτήν την περίπτωση, χρησιμοποιήστε θύρα 1999 ως τη θύρα PostgreSQL.  

Σύνδεση με το Linux Εικονική που δημιουργήσατε μέσω PuTTY. Εάν αυτή είναι η πρώτη φορά που χρησιμοποιείτε μια Εικονική Linux Azure, δείτε [πώς μπορείτε να χρησιμοποιήσετε SSH με Linux στην Azure](virtual-machines-linux-mac-create-ssh-keys.md) για να μάθετε πώς μπορείτε να χρησιμοποιήσετε PuTTY για να συνδεθείτε με μια Εικονική Linux.

1. Εκτελέστε την παρακάτω εντολή για να μεταβείτε στη ρίζα (διαχειριστές):

        # sudo su -

2. Ορισμένες κατανομές έχουν εξαρτήσεις που πρέπει να εγκαταστήσετε πριν από την εγκατάσταση του PostgreSQL. Έλεγχος για το distro σε αυτήν τη λίστα και να εκτελέσετε την κατάλληλη εντολή:

    - Κόκκινο καπέλο βάσης Linux:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Debian Linux βάσης:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - SUSE Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. Κάντε λήψη του PostgreSQL στον ριζικό κατάλογο και, στη συνέχεια, αποσυμπιέστε το πακέτο:

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    Τα παραπάνω είναι ένα παράδειγμα. Μπορείτε να βρείτε τη διεύθυνση λήψη πιο λεπτομερείς στο [ευρετήριο /pub/προέλευσης /](https://ftp.postgresql.org/pub/source/).

4. Για να ξεκινήσετε τη δημιουργία, εκτέλεση αυτών των εντολών:

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. Εάν θέλετε να δημιουργήσετε όλα τα στοιχεία που μπορεί να δημιουργηθεί, συμπεριλαμβανομένης της τεκμηρίωσης (σελίδες HTML και Άντρας) και πρόσθετες λειτουργικές μονάδες (ΗΜΕΡΟΜΙΣΘΙΟΥ ΠΡΟΣΩΠΙΚΟΥ), εκτελέστε την ακόλουθη εντολή:

        # gmake install-world

    Θα πρέπει να λαμβάνετε το ακόλουθο μήνυμα επιβεβαίωσης:

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>Ρύθμιση παραμέτρων του PostgreSQL

1. (Προαιρετικό) Δημιουργία συμβολικής σύνδεσης για να μειώσετε την αναφορά PostgreSQL ώστε να μην περιλαμβάνουν τον αριθμό έκδοσης:

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. Δημιουργία καταλόγου για τη βάση δεδομένων:

        # mkdir -p /opt/pgsql_data

3. Δημιουργία μη ριζικό χρήστη και να τροποποιήσετε το προφίλ του χρήστη. Στη συνέχεια, μεταβείτε σε αυτόν το χρήστη (που ονομάζεται *postgres* στο παράδειγμά μας):

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] Για λόγους ασφαλείας, PostgreSQL χρησιμοποιεί μη ριζικό χρήστη για την προετοιμασία, Έναρξη ή τερματισμός της βάσης δεδομένων.


4. Επεξεργαστείτε το αρχείο *bash_profile* , εισάγοντας τις παρακάτω εντολές. Αυτές οι γραμμές θα προστεθούν στο τέλος του αρχείου *bash_profile* :

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. Εκτελέστε το αρχείο *bash_profile* :

        $ source .bash_profile

6. Επικυρώστε την εγκατάσταση, χρησιμοποιώντας την ακόλουθη εντολή:

        $ which psql

    Εάν η εγκατάσταση είναι επιτυχής, θα δείτε την ακόλουθη απόκριση:

        /opt/pgsql/bin/psql

7. Μπορείτε επίσης να ελέγξετε την έκδοση PostgreSQL:

        $ psql -V

8. Προετοιμασία της βάσης δεδομένων:

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    Πρέπει να λάβετε το εξής αποτέλεσμα:

![εικόνα](./media/virtual-machines-linux-postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>Ρύθμιση του PostgreSQL

<!--    [postgres@ test ~]$ exit -->

Εκτελέστε τις εξής εντολές:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Τροποποίηση δύο μεταβλητές στο αρχείο /etc/init.d/postgresql. Το πρόθεμα έχει οριστεί στη διαδρομή εγκατάστασης του PostgreSQL: **/opt/pgsql**. PGDATA έχει οριστεί στη διαδρομή χώρου αποθήκευσης δεδομένων του PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![εικόνα](./media/virtual-machines-linux-postgresql-install/no2.png)

Αλλαγή του αρχείου ώστε να είναι εκτελέσιμο:

    # chmod +x /etc/init.d/postgresql

Ξεκινήστε PostgreSQL:

    # /etc/init.d/postgresql start

Έλεγχος αν το τελικό σημείο του PostgreSQL είναι σε:

    # netstat -tunlp|grep 1999

Θα πρέπει να βλέπετε το εξής αποτέλεσμα:

![εικόνα](./media/virtual-machines-linux-postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Συνδεθείτε στη βάση δεδομένων Postgres

Μεταβείτε στο χρήστη postgres ξανά:

    # su - postgres

Δημιουργία βάσης δεδομένων Postgres:

    $ createdb events

Συνδεθείτε στη βάση δεδομένων συμβάντα που μόλις δημιουργήσατε:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Δημιουργία και διαγραφή πίνακα Postgres

Τώρα που έχετε συνδέσει με τη βάση δεδομένων, μπορείτε να δημιουργήσετε πίνακες σε αυτό.

Για παράδειγμα, μπορείτε να δημιουργήσετε έναν νέο πίνακα Postgres παράδειγμα, χρησιμοποιώντας την ακόλουθη εντολή:

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

Τώρα έχετε ρυθμίσει έναν πίνακα τεσσάρων στηλών με την εξής ονόματα στηλών και περιορισμούς:

1. Η στήλη "όνομα" έχει έχουν περιορισμένη από την εντολή VARCHAR να είναι στην περιοχή 20 χαρακτήρες.
2. Η στήλη "τρόφιμα" υποδηλώνει το στοιχείο τροφίμου που προκαλεί την εμφάνιση κάθε άτομο. VARCHAR όρια αυτό το κείμενο να είναι στην περιοχή 30 χαρακτήρες.
3. Η στήλη "επιβεβαιώθηκε" εγγραφών εάν το άτομο έχει RSVP'd για να το φαγοπότι. Οι αποδεκτές τιμές είναι "Ν" και "N".
4. Η "ημερομηνία" στήλη εμφανίζει όταν τους έχει εγγραφεί για το συμβάν. Postgres απαιτεί ότι ημερομηνίες να γράφονται με εεεε-μμ-ηη.

Θα πρέπει να βλέπετε τα εξής, εάν έχει δημιουργηθεί με επιτυχία τον πίνακά σας:

![εικόνα](./media/virtual-machines-linux-postgresql-install/no4.png)

Μπορείτε επίσης να ελέγξετε τη δομή του πίνακα, χρησιμοποιώντας την ακόλουθη εντολή:

![εικόνα](./media/virtual-machines-linux-postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Προσθήκη δεδομένων σε έναν πίνακα

Πρώτα, εισαγάγετε πληροφορίες σε μια γραμμή:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Θα πρέπει να βλέπετε αυτό το αποτέλεσμα:

![εικόνα](./media/virtual-machines-linux-postgresql-install/no6.png)

Μπορείτε να προσθέσετε μερικά περισσότερα άτομα, καθώς και στον πίνακα. Εδώ θα βρείτε ορισμένες επιλογές ή μπορείτε να δημιουργήσετε το δικό σας:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Εμφάνιση πινάκων

Χρησιμοποιήστε την παρακάτω εντολή για να εμφανίσετε έναν πίνακα:

    select * from potluck;

Το αποτέλεσμα είναι:

![εικόνα](./media/virtual-machines-linux-postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Διαγραφή δεδομένων σε έναν πίνακα

Χρησιμοποιήστε την παρακάτω εντολή για να διαγράψετε δεδομένα σε έναν πίνακα:

    delete from potluck where name=’John’;

Αυτή η ενέργεια διαγράφει όλες τις πληροφορίες στη γραμμή "John". Το αποτέλεσμα είναι:

![εικόνα](./media/virtual-machines-linux-postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Ενημέρωση δεδομένων σε έναν πίνακα

Χρησιμοποιήστε την παρακάτω εντολή για να ενημερώσετε δεδομένα σε έναν πίνακα. Για αυτό ένα Sandy έχει επιβεβαιώσει ότι she συμμετεχόντων, έτσι θα αλλάξουμε το RSVP από το "N" σε "Ν":

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##<a name="get-more-information-about-postgresql"></a>Λήψη περισσότερων πληροφοριών σχετικά με το PostgreSQL
Τώρα που έχετε ολοκληρώσει την εγκατάσταση του PostgreSQL σε μια Εικονική Linux Azure, μπορείτε να απολαύσετε χρησιμοποιείτε στο Azure. Για να μάθετε περισσότερα σχετικά με το PostgreSQL, επισκεφθείτε την [τοποθεσία Web του PostgreSQL](http://www.postgresql.org/).
