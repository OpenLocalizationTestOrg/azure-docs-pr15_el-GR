Ακολουθήστε τα παρακάτω βήματα για να εγκαταστήσετε και να εκτελέσετε MongoDB σε εικονικό μηχάνημα με CentOS Linux.

> [AZURE.WARNING] MongoDB δυνατότητες ασφαλείας, όπως τον έλεγχο ταυτότητας και τη σύνδεση διευθύνσεων IP, δεν είναι ενεργοποιημένες από προεπιλογή. Δυνατότητες ασφαλείας θα πρέπει να ενεργοποιηθεί πριν από την ανάπτυξη MongoDB για ένα περιβάλλον παραγωγής.  Για περισσότερες πληροφορίες, ανατρέξτε [ασφαλείας και τον έλεγχο ταυτότητας](http://www.mongodb.org/display/DOCS/Security+and+Authentication) .

1. Ρυθμίστε τις παραμέτρους του συστήματος διαχείρισης πακέτου (YUM), ώστε να μπορείτε να εγκαταστήσετε το MongoDB. Δημιουργήστε ένα αρχείο */etc/yum.repos.d/10gen.repo* για τη διατήρηση πληροφοριών σχετικά με το αποθετήριο και προσθέστε τα εξής:

        [10gen]
        name=10gen Repository
        baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64
        gpgcheck=0
        enabled=1

2. Αποθηκεύστε το αρχείο repo και, στη συνέχεια, εκτελέστε την ακόλουθη εντολή για να ενημερώσετε το τοπικό πακέτο βάσης δεδομένων:

        $ sudo yum update

3. Για την εγκατάσταση του πακέτου, εκτελέστε την ακόλουθη εντολή για να εγκαταστήσετε την πιο πρόσφατη έκδοση σταθερό MongoDB και το σχετικό εργαλεία:

        $ sudo yum install mongo-10gen mongo-10gen-server

    Περιμένετε ενώ MongoDB πραγματοποιεί λήψη και εγκαθιστά.

4. Δημιουργήστε έναν κατάλογο δεδομένων. Από προεπιλογή MongoDB αποθηκεύει τα δεδομένα στον κατάλογο */data/db* , αλλά θα πρέπει να δημιουργήσετε αυτόν τον κατάλογο. Για να δημιουργήσετε, εκτελέστε:

        $ sudo mkdir -p /srv/datadrive/data
        $ sudo chown `id -u` /srv/datadrive/data

    Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση του MongoDB σε Linux, ανατρέξτε στο θέμα [Γρήγορη έναρξη Unix][QuickstartUnix].

5. Για να ξεκινήσετε τη βάση δεδομένων, εκτελέστε:

        $ mongod --dbpath /srv/datadrive/data --logpath /srv/datadrive/data/mongod.log

    Όλα τα μηνύματα καταγραφής θα κατευθύνεται προς το αρχείο */srv/datadrive/data/mongod.log* όπως διακομιστή MongoDB ξεκινά και προεκχωρεί αρχεία εγγραφών. Ενδέχεται να χρειαστούν αρκετά λεπτά για MongoDB να εκ των προτέρων εκχώρηση τα αρχεία εγγραφών και ξεκινήστε ακρόαση για τις συνδέσεις.

6. Για να ξεκινήσετε το κέλυφος διαχείρισης MongoDB, ανοίξτε ένα νέο παράθυρο SSH ή PuTTY και να εκτελέσετε:

        $ mongo
        > db.foo.save ( { a:1 } )
        > db.foo.find()
        { _id : ..., a : 1 }
        > show dbs  
        ...
        > show collections  
        ...  
        > help  

    Η βάση δεδομένων δημιουργείται από την εισαγωγή.

7. Μόλις εγκαταστήσετε MongoDB πρέπει να ρυθμίσετε ένα τελικό σημείο, έτσι ώστε να MongoDB δυνατότητα απομακρυσμένης πρόσβασης. Στην πύλη διαχείρισης, κάντε κλικ στην επιλογή **εικονικές μηχανές**, στη συνέχεια, κάντε κλικ στο όνομα του νέου εικονική μηχανή και, στη συνέχεια, κάντε κλικ στην επιλογή **τελικά σημεία**.
    
    ![Τα τελικά σημεία][Image7]

8. Κάντε κλικ στην επιλογή **Προσθήκη τελικό σημείο** στο κάτω μέρος της σελίδας.
    
    ![Τα τελικά σημεία][Image8]

9. Προσθέστε ένα τελικό σημείο με τις ακόλουθες ρυθμίσεις:

 - **Όνομα**: Mongo
 - **Πρωτόκολλο**: TCP
 - **Δημόσια θύρας**: 27017
 - **Ιδιωτική θύρας**: 27017
 
 Αυτό θα σας επιτρέψει MongoDB να απομακρυσμένης πρόσβασης.



[QuickStartUnix]: http://www.mongodb.org/display/DOCS/Quickstart+Unix


[Image7]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint.png
[Image8]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint2.png
