<properties
 pageTitle="NAMD με το Microsoft HPC πακέτο σε VM Linux | Microsoft Azure"
 description="Ανάπτυξη ένα σύμπλεγμα πακέτο HPC Microsoft στην Azure και να εκτελέσετε μια προσομοίωση NAMD με charmrun σε πολλούς κόμβους υπολογιστικών Linux"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="10/13/2016"
 ms.author="danlep"/>

# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>Εκτέλεση NAMD με το πακέτο του Microsoft HPC σε Linux υπολογισμού τους κόμβους Azure

Σε αυτό το άρθρο σάς δείχνει ένας τρόπος για να εκτελέσετε ένα φόρτο εργασίας υπολογιστική υψηλών επιδόσεων (HPC) Linux σε Azure εικονικές μηχανές. Εδώ, να ρυθμίσετε ένα σύμπλεγμα [Πακέτο HPC Microsoft](https://technet.microsoft.com/library/cc514029) στην Azure με κόμβους υπολογιστικών Linux και να εκτελέσετε μια προσομοίωση [NAMD](http://www.ks.uiuc.edu/Research/namd/) για τον υπολογισμό και την απεικόνιση τη δομή ενός συστήματος μεγάλη βιομοριακών.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (για το Dynamics μοριακή Nanoscale πρόγραμμα) είναι ένα πακέτο παράλληλες μοριακή dynamics έχει σχεδιαστεί για προσομοίωση υψηλών επιδόσεων μεγάλη βιομοριακών συστήματα που περιέχει έως εκατομμύρια άτομα. Αυτά τα συστήματα παραδείγματα ιούς, δομές κελιού και μεγάλο πρωτεϊνών. NAMD κλίμακες εκατοντάδες πυρήνων για τυπικές προσομοιώσεις και περισσότερους από 500.000 πυρήνων για μεγαλύτερη προσομοιώσεις.

* **Πακέτο HPC Microsoft** παρέχει δυνατότητες για να εκτελέσετε ευρείας κλίμακας HPC και παράλληλες εφαρμογές σε συμπλεγμάτων υπολογιστών εσωτερικής εγκατάστασης ή Azure εικονικές μηχανές. Υποστηρίζει πλέον την εκτέλεση Linux HPC εφαρμογών σε Linux που αναπτύχθηκαν αρχικά ως λύση για το Windows HPC φόρτους εργασίας, HPC πακέτο υπολογίσετε ΣΠΣ κόμβο αναπτυχθεί σε ένα σύμπλεγμα HPC πακέτο. Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-linux-classic-hpcpack-cluster.md) για μια εισαγωγή.

Για άλλες επιλογές για να εκτελέσετε Linux HPC φόρτους εργασίας σε Azure, ανατρέξτε στο θέμα [υπολογιστική τεχνική πόρους για μαζική και υψηλών επιδόσεων](../batch/batch-hpc-solutions.md).




## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* **Σύμπλεγμα HPC πακέτο με Linux τον υπολογισμό κόμβοι** - ανάπτυξη ένα σύμπλεγμα HPC πακέτο με κόμβους υπολογιστικών Linux στην Azure χρησιμοποιώντας ένα [πρότυπο από διαχειριστή πόρων Azure](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) ή μια [δέσμη ενεργειών του Azure PowerShell](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-linux-classic-hpcpack-cluster.md) για το τις προϋποθέσεις και τα βήματα για κάθε επιλογή. Εάν επιλέξετε την επιλογή ανάπτυξης δέσμης ενεργειών του PowerShell, ανατρέξτε στο θέμα το δείγμα αρχείου ρύθμισης παραμέτρων σε τα δείγματα αρχείων στο τέλος αυτού του άρθρου. Αυτό το αρχείο ρυθμίζει ένα σύμπλεγμα βασίζονται στο Azure HPC πακέτο που αποτελείται από έναν κόμβο κεφαλής Windows Server 2012 R2 και τέσσερις κόμβους υπολογιστικών μεγάλο 6.6 CentOS μέγεθος. Προσαρμόστε αυτό το αρχείο, όπως απαιτείται για το περιβάλλον σας.


* **Αρχεία NAMD λογισμικό και το πρόγραμμα εκμάθησης** - λήψη NAMD λογισμικού για Linux από την τοποθεσία [NAMD](http://www.ks.uiuc.edu/Research/namd/) (απαιτείται εγγραφή). Σε αυτό το άρθρο βασίζεται σε NAMD έκδοση 2.10 και χρησιμοποιεί την αρχειοθέτηση [Linux-x86_64 (64 bit Intel/AMD με Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) . Λήψη επίσης τα [αρχεία του προγράμματος εκμάθησης NAMD](http://www.ks.uiuc.edu/Training/Tutorials/#namd). Τα στοιχεία λήψης είναι .tar αρχεία και χρειάζεστε ένα εργαλείο των Windows για να εξαγάγετε τα αρχεία στον κόμβο κεφαλής συμπλέγματος. Για να εξαγάγετε τα αρχεία, ακολουθήστε τις οδηγίες παρακάτω σε αυτό το άρθρο. 

* **VMD** (προαιρετικά) - για να δείτε τα αποτελέσματα της εργασίας σας NAMD, λήψη και εγκατάσταση του προγράμματος μοριακή απεικόνιση [VMD](http://www.ks.uiuc.edu/Research/vmd/) σε έναν υπολογιστή της επιλογής σας. Η τρέχουσα έκδοση είναι 1.9.2. Ανατρέξτε στο θέμα το VMD λήψη τοποθεσία για να ξεκινήσετε.  


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Ρύθμιση αμοιβαία σχέση αξιοπιστίας μεταξύ κόμβους υπολογιστικών
Εκτέλεση μιας εργασίας σταυρό κόμβου σε πολλούς κόμβους Linux απαιτεί τους κόμβους ώστε να θεωρηθεί αξιόπιστος μεταξύ τους (από **rsh** ή **ssh**). Όταν δημιουργείτε το σύμπλεγμα HPC πακέτο με τη δέσμη ενεργειών ανάπτυξης του Microsoft HPC πακέτο IaaS, η δέσμη ενεργειών ρυθμίζει αυτόματα μόνιμο αμοιβαία αξιοπιστίας για το λογαριασμό διαχειριστή που καθορίζετε. Για τους χρήστες που δεν είναι διαχειριστής δημιουργείτε στον τομέα του συμπλέγματος, πρέπει να ρυθμίσετε προσωρινό αμοιβαία αξιοπιστίας μεταξύ τους κόμβους όταν μια εργασία που έχει εκχωρηθεί σε αυτά. Στη συνέχεια, καταστρέψετε τη σχέση αφού ολοκληρωθεί η εργασία. Για να το κάνετε αυτό για κάθε χρήστη, δώστε ένα ζεύγος κλειδιών RSA στο σύμπλεγμα που χρησιμοποιεί το πακέτο HPC για να δημιουργήσετε τη σχέση αξιοπιστίας. Ακολουθούν οδηγίες.

### <a name="generate-an-rsa-key-pair"></a>Δημιουργία ενός ζεύγους κλειδιών RSA
Είναι εύκολο να δημιουργήσετε ένα ζεύγος κλειδιών RSA, η οποία περιέχει ένα δημόσιο κλειδί και ένα ιδιωτικό κλειδί, εκτελώντας την εντολή **ssh keygen** Linux.

1.  Συνδεθείτε με έναν υπολογιστή Linux.

2.  Εκτελέστε την ακόλουθη εντολή:

    ```bash
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Πατήστε το πλήκτρο **Enter** για να χρησιμοποιήσετε τις προεπιλεγμένες ρυθμίσεις μέχρι να ολοκληρωθεί η εντολή. Εισαγάγετε μια φράση πρόσβασης εδώ. Όταν σας ζητηθεί κωδικός πρόσβασης, απλώς πατήστε το πλήκτρο **Enter**.

    ![Δημιουργία ενός ζεύγους κλειδιών RSA][keygen]

3.  Αλλαγή καταλόγου στον κατάλογο ~/.ssh. Το ιδιωτικό κλειδί είναι αποθηκευμένο στο id_rsa και το δημόσιο κλειδί σε id_rsa.pub.

    ![Πλήκτρα ιδιωτικά και δημόσια][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Προσθέστε το ζεύγος κλειδιών στο σύμπλεγμα HPC πακέτο
1.  [Σύνδεση με απομακρυσμένη επιφάνεια εργασίας](virtual-machines-windows-connect-logon.md) στον κόμβο κεφαλής Εικονική χρησιμοποιούν τον τομέα διαπιστευτηρίων που παρέχονται όταν αναπτυχθεί το σύμπλεγμα (για παράδειγμα, hpc\clusteradmin). Μπορείτε να διαχειριστείτε το σύμπλεγμα από τον κόμβο κεφαλίδας.

2. Χρησιμοποιήστε τις τυπικές διαδικασίες Windows Server για να δημιουργήσετε ένα λογαριασμό χρήστη τομέα στον τομέα της υπηρεσίας καταλόγου Active Directory του συμπλέγματος. Για παράδειγμα, χρησιμοποιήστε το εργαλείο Active Directory User και υπολογιστών στον κόμβο κεφαλίδας. Τα παραδείγματα σε αυτό το άρθρο προϋποθέτουν δημιουργήσετε έναν νέο χρήστη τομέα με το όνομα hpcuser στον τομέα hpclab (hpclab\hpcuser).

3. Προσθήκη του τομέα χρήστη στο σύμπλεγμα HPC πακέτο ως χρήστης σύμπλεγμα. Για οδηγίες, ανατρέξτε στο θέμα [Προσθήκη ή κατάργηση συμπλέγματος χρήστες](https://technet.microsoft.com/library/ff919330.aspx).

2.  Δημιουργήστε ένα αρχείο που ονομάζεται C:\cred.xml και αντιγράψτε τα δεδομένα κλειδιού RSA σε αυτό. Μπορείτε να βρείτε ένα παράδειγμα σε τα δείγματα αρχείων στο τέλος αυτού του άρθρου.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Ανοίξτε μια γραμμή εντολών και πληκτρολογήστε την παρακάτω εντολή για να ορίσετε τα δεδομένα διαπιστευτήρια για το λογαριασμό hpclab\hpcuser. Μπορείτε να χρησιμοποιήσετε την παράμετρο **extendeddata** για τη μεταβίβαση το όνομα του αρχείου C:\cred.xml που δημιουργήσατε για τα δεδομένα του κλειδιού.

    ```command
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Αυτή η εντολή ολοκληρωθεί με επιτυχία χωρίς αποτέλεσμα. Μετά τη ρύθμιση των διαπιστευτηρίων για τους λογαριασμούς χρηστών που πρέπει να εκτελέσετε εργασίες, αποθηκεύστε το αρχείο cred.xml σε ασφαλή θέση ή διαγράψτε το.

5.  Εάν δημιουργηθεί το ζεύγος κλειδιών RSA σε μία από τις κόμβους Linux, μην ξεχάσετε να διαγράψετε τα πλήκτρα μετά την ολοκλήρωση της με τη χρήση τους. Πακέτο HPC δεν έχετε ορίσει αμοιβαία αξιοπιστίας εάν εντοπίσει ένα υπάρχον αρχείο id_rsa ή ένα αρχείο id_rsa.pub.

>[AZURE.IMPORTANT] Δεν συνιστάται να εκτελείτε μια εργασία Linux ως διαχειριστής συμπλέγματος σε ένα κοινόχρηστο σύμπλεγμα, επειδή μιας εργασίας που έχει υποβληθεί από ένα διαχειριστή εκτελείται στο λογαριασμό ρίζας σε τους κόμβους Linux. Μια εργασία που υποβάλλεται από ένα χρήστη που δεν είναι διαχειριστής εκτελείται σε έναν τοπικό λογαριασμό χρήστη Linux με το ίδιο όνομα με το χρήστη του έργου. Σε αυτήν την περίπτωση, πακέτο HPC ρυθμίζει αμοιβαία αξιοπιστίας για αυτόν το χρήστη Linux σε όλους τους κόμβους που έχει εκχωρηθεί στο έργο. Μπορείτε να ρυθμίσετε το χρήστη Linux με μη αυτόματο τρόπο σε τους κόμβους Linux πριν από την εκτέλεση της εργασίας ή πακέτο HPC δημιουργεί ο χρήστης αυτόματα κατά την υποβολή της εργασίας. Εάν το πακέτο HPC δημιουργεί το χρήστη, HPC πακέτο τη διαγράφει αφού ολοκληρωθεί η εργασία. Για να μειώσετε απειλή για την ασφάλεια, τα πλήκτρα καταργούνται μετά την ολοκλήρωση της εργασίας σε τους κόμβους.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Ρυθμίστε ένα κοινόχρηστο αρχείο για τους κόμβους Linux

Τώρα, ρυθμίστε ένα κοινόχρηστο αρχείο Ερωτήσεων και ενεργοποίησης του κοινόχρηστου φακέλου σε όλους τους κόμβους Linux για να επιτρέψετε τους κόμβους Linux για πρόσβαση σε αρχεία NAMD με μια κοινή διαδρομή. Ακολουθούν τα βήματα για ενεργοποίησης έναν κοινόχρηστο φάκελο στον κόμβο κεφαλίδας. Ένα κοινόχρηστο στοιχείο προτείνεται για διανομή όπως CentOS 6.6 που δεν υποστηρίζουν αυτήν τη στιγμή η υπηρεσία Azure αρχείων. Εάν σας κόμβους Linux υποστηρίζει ένα κοινόχρηστο αρχείο Azure, ανατρέξτε στο θέμα [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αρχείων Azure με Linux](../storage/storage-how-to-use-files-linux.md). Για πρόσθετες επιλογές κοινής χρήσης αρχείου με το πακέτο HPC, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Δημιουργήστε ένα φάκελο στον κόμβο κεφαλής και μοιραστείτε για όλους τους χρήστες, ορίζοντας δικαιώματα ανάγνωσης/εγγραφής. Σε αυτό το παράδειγμα, \\ \\CentOS66HN\Namd είναι το όνομα του φακέλου, όπου CentOS66HN είναι το όνομα κεντρικού υπολογιστή του κεφαλής κόμβου.

2. Δημιουργήστε έναν υποφάκελο με το όνομα namd2 στον κοινόχρηστο φάκελο. Στο namd2, δημιουργήστε μια άλλη υποφάκελο με το όνομα namdsample.

3. Εξαγωγή αρχείων NAMD στο φάκελο, χρησιμοποιώντας μια έκδοση Windows **πίσσα** ή κάποιο άλλο βοηθητικό πρόγραμμα των Windows που λειτουργεί σε αρχειοθήκες .tar. 
    * Εξαγωγή στην αρχειοθήκη πίσσα NAMD να \\ \\CentOS66HN\Namd\namd2.
    
    * Εξαγάγετε τα αρχεία του προγράμματος εκμάθησης στην περιοχή \\ \\CentOS66HN\Namd\namd2\namdsample.

4. Ανοίξτε ένα παράθυρο του Windows PowerShell και εκτελέστε τις ακόλουθες εντολές για να τοποθετήσετε τον κοινόχρηστο φάκελο σε τους κόμβους Linux.

    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Η πρώτη εντολή δημιουργεί ένα φάκελο που ονομάζεται /namd2 σε όλους τους κόμβους στην ομάδα LinuxNodes. Η δεύτερη εντολή συνδέει το //CentOS66HN/Namd/namd2 κοινόχρηστου φακέλου σε φάκελο με dir_mode και να οριστούν σε 777 file_mode bit. Το *όνομα χρήστη* και *τον κωδικό πρόσβασης* στην εντολή πρέπει να είναι τα διαπιστευτήρια του χρήστη στον κόμβο κεφαλίδας.

>[AZURE.NOTE]Το "\`" σύμβολο στην εντολή δεύτερο είναι ένα σύμβολο διαφυγής για το PowerShell. "\`," σημαίνει ότι το "," (κόμμα) είναι μέρος της εντολής.


## <a name="create-a-bash-script-to-run-a-namd-job"></a>Δημιουργία δέσμης ενεργειών πάρτι για να εκτελέσετε μια εργασία NAMD

Εργαστείτε NAMD ανάγκες αρχείου *λίστας κόμβων* **charmrun** για να καθορίσετε τον αριθμό των κόμβους για χρήση κατά την εκκίνηση NAMD διεργασίες. Μπορείτε να χρησιμοποιήσετε μια δέσμη ενεργειών πάρτι που δημιουργεί το αρχείο λίστας κόμβων και εκτελεί **charmrun** με αυτό το αρχείο λίστας κόμβων. Στη συνέχεια, μπορείτε να υποβάλετε μια εργασία NAMD στο HPC σύμπλεγμα Manager που καλεί αυτήν τη δέσμη ενεργειών.

Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας κειμένου της επιλογής σας, δημιουργήστε μια δέσμη ενεργειών πάρτι στο φάκελο /namd2 που περιέχει τα αρχεία προγράμματος NAMD και ονομάστε το hpccharmrun.sh. Για ένα γρήγορο δοκίμιο της έννοια, αντιγράψτε τη δέσμη ενεργειών hpccharmrun.sh παράδειγμα που παρέχονται στο τέλος αυτού του άρθρου και μεταβείτε στην [Υποβολή μιας εργασίας NAMD](#submit-a-namd-job).

>[AZURE.TIP] Αποθήκευση δέσμη ενεργειών σας ως αρχείο κειμένου με Linux καταλήξεις των γραμμών (LF μόνο, δεν CR LF). Αυτό εξασφαλίζει ότι εκτελείται σωστά σε τους κόμβους Linux.

Παρακάτω θα βρείτε λεπτομέρειες σχετικά με το τι κάνει αυτή η δέσμη ενεργειών πάρτι. 

1.  Ορισμός ορισμένες μεταβλητές.

    ```bash
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Λήψη πληροφοριών κόμβο από τις μεταβλητές περιβάλλοντος. $NODESCORES αποθηκεύει μια λίστα με λέξεις διαίρεσης από $CCP_NODES_CORES. $COUNT είναι το μέγεθος του $NODESCORES.
    ```
    # Get node information from the environment variables
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```    
    
    Η μορφή για τη μεταβλητή CCP_NODES_CORES $ είναι ως εξής:

    ```
    <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
    ```

    Αυτή η μεταβλητή παραθέτει τον συνολικό αριθμό κόμβους, ονόματα κόμβου και αριθμό πυρήνων σε κάθε κόμβο που έχουν εκχωρηθεί στο έργο. Για παράδειγμα, εάν η εργασία χρειάζεται 10 πυρήνων για την εκτέλεση, την τιμή $CCP_NODES_CORES είναι παρόμοια με:

    ```
    3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
    ```
        
3.  Εάν δεν έχει οριστεί η μεταβλητή CCP_NODES_CORES $, ξεκινήστε **charmrun** απευθείας. (Αυτό θα πρέπει να μόνο συμβεί όταν εκτελείτε αυτήν τη δέσμη ενεργειών απευθείας σε σας κόμβους Linux.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Ή να δημιουργήσετε ένα αρχείο λίστας κόμβων για **charmrun**.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  Εκτέλεση **charmrun** με το αρχείο λίστας κόμβων, να λάβετε την κατάστασή του αποστολέα και καταργήστε το αρχείο λίστας κόμβων στο τέλος.

    ${CCP_NUMCPUS} είναι μια άλλη μεταβλητή περιβάλλοντος οριστεί από τον κόμβο κεφαλής HPC πακέτο. Αποθηκεύει τον αριθμό συνολικό πυρήνων έχει εκχωρηθεί σε αυτήν την εργασία. Χρησιμοποιούμε για να καθορίσετε τον αριθμό των διεργασιών για charmrun.

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  Κλείστε το με την κατάσταση επιστροφής **charmrun** .

    ```
    exit ${RTNSTS}
    ```



Ακολουθεί τις πληροφορίες στο αρχείο λίστας κόμβων, το οποίο δημιουργεί τη δέσμη ενεργειών:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Για παράδειγμα:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Υποβάλετε μια εργασία NAMD

Τώρα είστε έτοιμοι να υποβάλετε μια εργασία NAMD στη Διαχείριση σύμπλεγμα HPC.

1.  Σύνδεση με τον κόμβο κεφαλής συμπλέγματος και εκκίνηση της διαχείρισης σύμπλεγμα HPC.

2.  **Διαχείριση πόρων**, βεβαιωθείτε ότι κόμβους υπολογιστικών Linux είναι σε κατάσταση **Online** . Εάν δεν είναι, επιλέξτε τα και κάντε κλικ στην επιλογή **Σύνδεση**.

2.  **Διαχείριση του έργου**, κάντε κλικ στην επιλογή **Νέα εργασία**.

3.  Πληκτρολογήστε ένα όνομα για την εργασία όπως *hpccharmrun*.

    ![Νέα εργασία HPC][namd_job]

4.  Στη σελίδα **Λεπτομέρειες εργασίας** , στην περιοχή **Πόρους εργασίας**, επιλέξτε τον τύπο του πόρου ως **κόμβος** και ορίστε την **Ελάχιστη** σε 3. , μπορούμε να εκτελέσουμε της εργασίας σε τρεις κόμβους Linux και κάθε κόμβο έχει τεσσάρων πυρήνων.

    ![Πόροι εργασίας][job_resources]

5. Κάντε κλικ στην επιλογή **Επεξεργασία εργασίες** στο αριστερό παράθυρο περιήγησης και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη** για να προσθέσετε μια εργασία στο έργο.    


6. Στη σελίδα **λεπτομερειών για την εργασία και ανακατεύθυνση εισόδου/εξόδου** , ορίστε τις παρακάτω τιμές:

    * **Γραμμή εντολών** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

        >[AZURE.TIP] Η προηγούμενη γραμμή εντολών είναι μια απλή εντολή χωρίς αλλαγές γραμμών. Να αναδιπλώνεται να εμφανίζεται σε πολλές γραμμές κάτω από τη **γραμμή εντολών**.

    * **Κατάλογος εργασίας** - /namd2

    * **Ελάχιστο** - 3

    ![Λεπτομέρειες εργασίας][task_details]

    >[AZURE.NOTE] Μπορείτε να ορίσετε τον κατάλογο εργασίας εδώ επειδή **charmrun** προσπαθεί να μεταβείτε στον ίδιο κατάλογο εργασίας σε κάθε κόμβο. Εάν ο κατάλογος εργασίας δεν οριστεί, πακέτο HPC ξεκινά η εντολή σε ένα φάκελο με τυχαία ονομασία που δημιουργήθηκε σε έναν από τους κόμβους Linux. Αυτό έχει ως αποτέλεσμα το ακόλουθο σφάλμα στους άλλους κόμβους: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` για να αποφύγετε αυτό το πρόβλημα, καθορίστε μια διαδρομή φακέλου που είναι δυνατή από όλους τους κόμβους ως ο κατάλογος εργασίας.

5.  Κάντε κλικ στο κουμπί **OK** και, στη συνέχεια, κάντε κλικ στην επιλογή **Υποβολή** για να εκτελέσετε αυτήν την εργασία.

    Από προεπιλογή, το πακέτο HPC υποβάλλει την εργασία ως τον τρέχοντα λογαριασμό συνδεδεμένου χρήστη. Ένα παράθυρο διαλόγου μπορεί να σας ζητηθεί να εισαγάγετε το όνομα χρήστη και τον κωδικό πρόσβασης, αφού κάνετε κλικ στην επιλογή **Υποβολή**.

    ![Τα διαπιστευτήρια εργασία][creds]

    Υπό ορισμένες συνθήκες, το πακέτο HPC απομνημονεύει τις πληροφορίες χρήστη, εισαγάγετε πριν από και δεν εμφανίζει το παράθυρο διαλόγου. Για να εμφανίσετε ξανά πακέτο HPC, πληκτρολογήστε την παρακάτω εντολή στη γραμμή εντολών και, στη συνέχεια, να υποβάλετε την εργασία.

    ```command
    hpccred delcreds
    ```

6.  Η εργασία χρειάζονται μερικά λεπτά για να ολοκληρώσετε.

7.  Βρείτε το αρχείο καταγραφής έργου στο \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log και τα αρχεία εξόδου \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.

8.  Προαιρετικά, ξεκινήστε VMD για να προβάλετε τα αποτελέσματα της εργασίας. Τα βήματα για την απεικόνιση του NAMD εξόδου αρχεία (στη συγκεκριμένη περίπτωση, μια ubiquitin πρωτεΐνης μόριο στο νερό σφαίρα) είναι πέρα από το πεδίο αυτού του άρθρου. Για λεπτομέρειες, ανατρέξτε στο θέμα [Πρόγραμμα εκμάθησης NAMD](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) .

    ![Αποτελέσματα εργασία][vmd_view]

## <a name="sample-files"></a>Δείγματα αρχείων

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Δείγμα αρχείο ρύθμισης παραμέτρων XML για ανάπτυξη σύμπλεγμα με δέσμη ενεργειών του PowerShell

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>    
```

### <a name="sample-credxml-file"></a>Δείγμα αρχείου cred.xml

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```

### <a name="sample-hpccharmrunsh-script"></a>Δείγμα δέσμης ενεργειών hpccharmrun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/creds.png
[task_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/vmd_view.png
