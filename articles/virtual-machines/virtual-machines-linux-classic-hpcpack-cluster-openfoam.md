<properties
 pageTitle="Εκτελέστε OpenFOAM με HPC πακέτο στο ΣΠΣ Linux | Microsoft Azure"
 description="Ανάπτυξη ένα σύμπλεγμα πακέτο HPC Microsoft στην Azure και να εκτελέσετε μια εργασία OpenFOAM σε πολλούς κόμβους υπολογιστικών Linux σε ένα δίκτυο RDMA."
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
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Εκτέλεση OpenFoam με το Microsoft HPC πακέτο σε ένα σύμπλεγμα Linux RDMA στο Azure

Σε αυτό το άρθρο σάς δείχνει ένας τρόπος για να εκτελέσετε OpenFoam σε εικονικές μηχανές Windows Azure. Εδώ, μπορείτε να αναπτύξετε ένα σύμπλεγμα πακέτο HPC Microsoft με κόμβους υπολογιστικών Linux στην Azure και εκτέλεση ενός [OpenFoam](http://openfoam.com/) εργασίας με Intel MPI. Μπορείτε να χρησιμοποιήσετε με δυνατότητα RDMA ΣΠΣ Azure για τους κόμβους υπολογισμού, ώστε να κόμβους υπολογιστικών επικοινωνία μέσω του δικτύου Azure RDMA. Άλλες επιλογές για την εκτέλεση OpenFoam στο Azure περιλαμβάνουν πλήρως ρυθμισμένη εμπορική εικόνες που είναι διαθέσιμες στην αγορά, όπως του UberCloud [2.3 OpenFoam σε CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)και εκτελώντας στη [Δέσμη Azure](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (για το άνοιγμα λειτουργία πεδίο και χειρισμού) είναι ένα πακέτο λογισμικού υπολογιστική dynamics δυναμικού Άνοιγμα προέλευσης (CFD) που χρησιμοποιούνται ευρέως στο μηχανικής και φυσικής, στις εταιρείες τόσο εμπορικό και ακαδημαϊκό. Περιλαμβάνει εργαλεία για meshing, ιδίως snappyHexMesh, μια parallelized mesher για σύνθετη γεωμετρίες CAD και για προ- και επεξεργασίας εξόδου. Εκτελέστε σχεδόν όλες τις διεργασίες παράλληλα, επιτρέπει στους χρήστες να εκμεταλλευτείτε πλήρως το υλικό υπολογιστή στη διάθεσή τους.  

Πακέτο HPC Microsoft παρέχει δυνατότητες για την εκτέλεση ευρείας κλίμακας HPC και παράλληλες εφαρμογές, συμπεριλαμβανομένων των εφαρμογών MPI, σε συμπλεγμάτων εικονικές μηχανές Windows Azure. Πακέτο HPC υποστηρίζει επίσης εκτελείται Linux HPC εφαρμογών σε Linux τον υπολογισμό κόμβο ΣΠΣ αναπτυχθεί σε ένα σύμπλεγμα HPC πακέτο. Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-linux-classic-hpcpack-cluster.md) για μια εισαγωγή στη χρήση κόμβους υπολογιστικών Linux με HPC Pack.

>[AZURE.NOTE] Σε αυτό το άρθρο παρουσιάζει τον τρόπο για να εκτελέσετε ένα φόρτο εργασίας Linux MPI με HPC Pack. Αυτό προϋποθέτει ότι έχετε κάποια εξοικείωση με Linux σύστημα διαχείρισης και εκτέλεση MPI φόρτους εργασίας σε συμπλεγμάτων Linux. Εάν χρησιμοποιείτε εκδόσεις του MPI και OpenFOAM διαφορετική από αυτές που εμφανίζονται σε αυτό το άρθρο, ίσως χρειαστεί να τροποποιήσετε ορισμένα βήματα εγκατάστασης και ρύθμισης παραμέτρων. 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

*   **Σύμπλεγμα HPC πακέτο με με δυνατότητα RDMA Linux τον υπολογισμό κόμβοι** - ανάπτυξη ένα σύμπλεγμα HPC πακέτο με μέγεθος A8, A9, H16r ή H16rm Linux κόμβους υπολογιστικών με τη χρήση ενός [προτύπου για τη διαχείριση πόρων Azure](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) ή μια [δέσμη ενεργειών του Azure PowerShell](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-linux-classic-hpcpack-cluster.md) για το τις προϋποθέσεις και τα βήματα για κάθε επιλογή. Εάν επιλέξετε την επιλογή ανάπτυξης δέσμης ενεργειών του PowerShell, ανατρέξτε στο θέμα το δείγμα αρχείου ρύθμισης παραμέτρων σε τα δείγματα αρχείων στο τέλος αυτού του άρθρου. Χρησιμοποιήστε αυτήν τη ρύθμιση παραμέτρων για να αναπτύξετε ένα σύμπλεγμα βασίζονται στο Azure HPC πακέτο που αποτελείται από έναν κόμβο κεφαλής A8 Windows Server 2012 R2 μέγεθος και 2 κόμβους υπολογιστικών A8 SUSE Linux Enterprise Server 12 μέγεθος. Αντικαταστήστε τις κατάλληλες τιμές για τα ονόματα συνδρομής και υπηρεσίας. 

    **Επιπλέον στοιχεία που πρέπει να γνωρίζετε**

    *   Για Linux RDMA κοινωνικής δικτύωσης προϋποθέσεις στο Azure, ανατρέξτε στο θέμα [σχετικά με τη σειρά H και υπολογισμού στενής ΣΠΣ μια σειρά](virtual-machines-windows-a8-a9-a10-a11-specs.md).

    *   Εάν χρησιμοποιείτε την επιλογή Powershell δέσμη ενεργειών ανάπτυξης, ανάπτυξη όλους τους κόμβους υπολογισμού Linux μέσα στην υπηρεσία cloud μία για να χρησιμοποιήσετε τη σύνδεση δικτύου RDMA.

    *   Μετά την ανάπτυξη τους κόμβους Linux, σύνδεση με SSH για να εκτελέσετε τις περισσότερες εργασίες διαχείρισης. Βρείτε τις λεπτομέρειες σύνδεσης SSH για κάθε Εικονική Linux στην πύλη του Azure.  
        
*   **Intel MPI** - για να εκτελέσετε OpenFOAM σε SLES 12 HPC τον υπολογισμό τους κόμβους Azure, πρέπει να εγκαταστήσετε το περιβάλλον εκτέλεσης Intel MPI βιβλιοθήκη 5 από την [τοποθεσία Intel.com](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 είναι προεγκατεστημένη σε εικόνες με βάση CentOS HPC.)  Αργότερα, εάν είναι απαραίτητο, εγκατάσταση Intel MPI σε σας κόμβους υπολογιστικών Linux. Για να προετοιμαστείτε για αυτό το βήμα, μετά την καταχώρηση με τεχνολογία Intel, ακολουθήστε τη σύνδεση στο μήνυμα ηλεκτρονικού ταχυδρομείου επιβεβαίωσης στη σχετική σελίδα web. Στη συνέχεια, αντιγράψτε τη σύνδεση λήψης για το αρχείο .tgz για την κατάλληλη έκδοση του Intel MPI. Σε αυτό το άρθρο βασίζεται σε Intel MPI έκδοση 5.0.3.048.

*   **Πακέτο προέλευσης OpenFOAM** - λήψη του λογισμικού OpenFOAM προέλευσης πακέτου για Linux από την [τοποθεσία OpenFOAM Foundation](http://openfoam.org/download/2-3-1-source/). Σε αυτό το άρθρο βασίζεται σε προέλευση πακέτο έκδοση 2.3.1, διαθέσιμο για λήψη ως OpenFOAM 2.3.1.tgz. Ακολουθήστε τις οδηγίες παρακάτω σε αυτό το άρθρο για αποσυσκευασία και μεταγλώττιση OpenFOAM σε κόμβους υπολογιστικών Linux.

*   **EnSight** (προαιρετικά) - για να δείτε τα αποτελέσματα της προσομοίωσης σας OpenFOAM, κάντε λήψη και εγκατάσταση του προγράμματος [EnSight](https://www.ceisoftware.com/download/) απεικόνισης και ανάλυση. Άδειες χρήσης και λήψη πληροφοριών υπάρχουν στην τοποθεσία EnSight.


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Ρύθμιση αμοιβαία σχέση αξιοπιστίας μεταξύ κόμβους υπολογιστικών

Εκτέλεση μιας εργασίας σταυρό κόμβου σε πολλούς κόμβους Linux απαιτεί τους κόμβους ώστε να θεωρηθεί αξιόπιστος μεταξύ τους (από **rsh** ή **ssh**). Όταν δημιουργείτε το σύμπλεγμα HPC πακέτο με τη δέσμη ενεργειών ανάπτυξης του Microsoft HPC πακέτο IaaS, η δέσμη ενεργειών ρυθμίζει αυτόματα μόνιμο αμοιβαία αξιοπιστίας για το λογαριασμό διαχειριστή που καθορίζετε. Για τους χρήστες που δεν είναι διαχειριστής δημιουργείτε στον τομέα του συμπλέγματος, πρέπει να ρυθμίσετε προσωρινό αμοιβαία αξιοπιστίας μεταξύ τους κόμβους όταν μια εργασία που έχει εκχωρηθεί σε αυτά, και καταστρέψετε τη σχέση αφού η εργασία έχει ολοκληρωθεί. Για να δημιουργήσετε αξιοπιστίας για κάθε χρήστη, δώστε ένα ζεύγος κλειδιών RSA στο σύμπλεγμα που χρησιμοποιεί το πακέτο HPC για τη σχέση αξιοπιστίας.

### <a name="generate-an-rsa-key-pair"></a>Δημιουργία ενός ζεύγους κλειδιών RSA

Είναι εύκολο να δημιουργήσετε ένα ζεύγος κλειδιών RSA, η οποία περιέχει ένα δημόσιο κλειδί και ένα ιδιωτικό κλειδί, εκτελώντας την εντολή **ssh keygen** Linux.

1.  Συνδεθείτε με έναν υπολογιστή Linux.

2.  Εκτελέστε την ακόλουθη εντολή:

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Πατήστε το πλήκτρο **Enter** για να χρησιμοποιήσετε τις προεπιλεγμένες ρυθμίσεις μέχρι να ολοκληρωθεί η εντολή. Εισαγάγετε μια φράση πρόσβασης εδώ. Όταν σας ζητηθεί κωδικός πρόσβασης, απλώς πατήστε το πλήκτρο **Enter**.

    ![Δημιουργία ενός ζεύγους κλειδιών RSA][keygen]

3.  Αλλαγή καταλόγου στον κατάλογο ~/.ssh. Το ιδιωτικό κλειδί είναι αποθηκευμένο στο id_rsa και το δημόσιο κλειδί σε id_rsa.pub.

    ![Πλήκτρα ιδιωτικά και δημόσια][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Προσθέστε το ζεύγος κλειδιών στο σύμπλεγμα HPC πακέτο
1.  Δημιουργήστε μια σύνδεση απομακρυσμένης επιφάνειας εργασίας σας κεφαλής κόμβο με το λογαριασμό διαχειριστή του πακέτου HPC (ο λογαριασμός διαχειριστή που ορίζετε όταν εκτελέσατε τη δέσμη ενεργειών ανάπτυξης).

2. Χρησιμοποιήστε τις τυπικές διαδικασίες Windows Server για να δημιουργήσετε ένα λογαριασμό χρήστη τομέα στον τομέα της υπηρεσίας καταλόγου Active Directory του συμπλέγματος. Για παράδειγμα, χρησιμοποιήστε το εργαλείο Active Directory User και υπολογιστών στον κόμβο κεφαλίδας. Τα παραδείγματα σε αυτό το άρθρο προϋποθέτουν δημιουργήσετε έναν νέο χρήστη τομέα με το όνομα hpclab\hpcuser.

3.  Δημιουργήστε ένα αρχείο που ονομάζεται C:\cred.xml και αντιγράψτε τα δεδομένα κλειδιού RSA σε αυτό. Ένα δείγμα αρχείου cred.xml βρίσκεται στο τέλος αυτού του άρθρου.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  Ανοίξτε μια γραμμή εντολών και πληκτρολογήστε την παρακάτω εντολή για να ορίσετε τα δεδομένα διαπιστευτήρια για το λογαριασμό hpclab\hpcuser. Μπορείτε να χρησιμοποιήσετε την παράμετρο **extendeddata** για τη μεταβίβαση το όνομα του αρχείου C:\cred.xml που δημιουργήσατε για τα δεδομένα του κλειδιού.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Αυτή η εντολή ολοκληρωθεί με επιτυχία χωρίς αποτέλεσμα. Μετά τη ρύθμιση των διαπιστευτηρίων για τους λογαριασμούς χρηστών που πρέπει να εκτελέσετε εργασίες, αποθηκεύστε το αρχείο cred.xml σε ασφαλή θέση ή διαγράψτε το.

5.  Εάν δημιουργηθεί το ζεύγος κλειδιών RSA σε μία από τις κόμβους Linux, μην ξεχάσετε να διαγράψετε τα πλήκτρα μετά την ολοκλήρωση της με τη χρήση τους. Εάν πακέτο HPC εντοπίσει ένα υπάρχον αρχείο id_rsa ή ένα αρχείο id_rsa.pub, αυτό δεν έχετε ορίσει αμοιβαία αξιοπιστίας.

>[AZURE.IMPORTANT] Δεν συνιστάται να εκτελείτε μια εργασία Linux ως διαχειριστής συμπλέγματος σε ένα κοινόχρηστο σύμπλεγμα, επειδή μιας εργασίας που έχει υποβληθεί από ένα διαχειριστή εκτελείται στο λογαριασμό ρίζας σε τους κόμβους Linux. Ωστόσο, μια εργασία που υποβάλλεται από ένα χρήστη που δεν είναι διαχειριστής εκτελείται σε έναν τοπικό λογαριασμό χρήστη Linux με το ίδιο όνομα με το χρήστη του έργου. Σε αυτήν την περίπτωση, πακέτο HPC ρυθμίζει αμοιβαία αξιοπιστίας για αυτόν το χρήστη Linux κατά μήκος τους κόμβους που έχει εκχωρηθεί στο έργο. Μπορείτε να ρυθμίσετε το χρήστη Linux με μη αυτόματο τρόπο σε τους κόμβους Linux πριν από την εκτέλεση της εργασίας ή πακέτο HPC δημιουργεί ο χρήστης αυτόματα κατά την υποβολή της εργασίας. Εάν το πακέτο HPC δημιουργεί το χρήστη, HPC πακέτο τη διαγράφει αφού ολοκληρωθεί η εργασία. Για να μειώσετε κινδύνους ασφάλειας, πακέτο HPC καταργεί τα πλήκτρα μετά την ολοκλήρωση του έργου.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Ρυθμίστε ένα κοινόχρηστο αρχείο για τους κόμβους Linux

Ορισμός τυπική Ερωτήσεων κοινόχρηστο στοιχείο σε ένα φάκελο στον κόμβο κεφαλίδας. Για να επιτρέψετε τους κόμβους Linux για να αποκτήσετε πρόσβαση σε αρχεία εφαρμογών με μια κοινή διαδρομή, τοποθετήσετε στον κοινόχρηστο φάκελο σε τους κόμβους Linux. Εάν θέλετε, μπορείτε να χρησιμοποιήσετε μια άλλη επιλογή, όπως μια κοινή χρήση αρχείων Azure - συνιστάται για πολλά σενάρια - ή ένα κοινόχρηστο στοιχείο NFS κοινής χρήσης. Δείτε το αρχείο κοινή χρήση πληροφοριών και λεπτομερείς οδηγίες [γρήγορα](virtual-machines-linux-classic-hpcpack-cluster.md)αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure.

1.  Δημιουργήστε ένα φάκελο στον κόμβο κεφαλής και μοιραστείτε για όλους τους χρήστες, ορίζοντας δικαιώματα ανάγνωσης/εγγραφής. Για παράδειγμα, να κάνετε κοινή χρήση C:\OpenFOAM στον κόμβο κεφαλής ως \\ \\SUSE12RDMA HN\OpenFOAM. Εδώ, *SUSE12RDMA HN* είναι το όνομα κεντρικού υπολογιστή του κεφαλής κόμβου.

2.  Ανοίξτε ένα παράθυρο του Windows PowerShell και να εκτελέσετε τις παρακάτω εντολές:

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Η πρώτη εντολή δημιουργεί ένα φάκελο που ονομάζεται /openfoam σε όλους τους κόμβους στην ομάδα LinuxNodes. Η δεύτερη εντολή συνδέει το //SUSE12RDMA-HN/OpenFOAM κοινόχρηστου φακέλου σε τους κόμβους Linux με dir_mode και να οριστούν σε 777 file_mode bit. Το *όνομα χρήστη* και *τον κωδικό πρόσβασης* στην εντολή πρέπει να είναι τα διαπιστευτήρια του χρήστη στον κόμβο κεφαλίδας.

>[AZURE.NOTE]Το "\`" σύμβολο στην εντολή δεύτερο είναι ένα σύμβολο διαφυγής για το PowerShell. "\`," σημαίνει ότι το "," (κόμμα) είναι μέρος της εντολής.

## <a name="install-mpi-and-openfoam"></a>Εγκατάσταση MPI και OpenFOAM

Για να εκτελέσετε OpenFOAM ως μια εργασία MPI στο δίκτυο RDMA, πρέπει να μεταγλωττίσετε OpenFOAM με τις βιβλιοθήκες Intel MPI. 

Πρώτα, να εκτελέσετε διάφορες εντολές **clusrun** για να εγκαταστήσετε το Intel MPI βιβλιοθήκες (Εάν δεν είναι ήδη εγκατεστημένο) και OpenFOAM σας κόμβους Linux. Χρησιμοποιήστε την κοινή χρήση κεφαλής κόμβου έχει ρυθμιστεί προηγουμένως για την κοινή χρήση των αρχείων εγκατάστασης μεταξύ τους κόμβους Linux.

>[AZURE.IMPORTANT]Αυτές οι εγκατάστασης και μεταγλώττιση βήματα είναι παραδείγματα. Πρέπει να έχετε ορισμένα γνώσεις Linux της διαχείρισης του συστήματος για να βεβαιωθείτε ότι εξαρτώμενα προγράμματα μεταγλώττισης και βιβλιοθήκες έχουν εγκατασταθεί σωστά. Ίσως χρειαστεί να τροποποιήσετε ορισμένες μεταβλητές περιβάλλοντος ή άλλων ρυθμίσεων για τις εκδόσεις της Intel MPI και OpenFOAM. Για λεπτομέρειες, ανατρέξτε στο θέμα [Βιβλιοθήκη MPI Intel για τον Οδηγό εγκατάστασης Linux](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) και [OpenFOAM προέλευσης πακέτου εγκατάστασης](http://openfoam.org/download/2-3-1-source/) για το περιβάλλον σας.


### <a name="install-intel-mpi"></a>Εγκατάσταση Intel MPI

Αποθηκεύστε το πακέτο εγκατάστασης για Intel MPI (l_mpi_p_5.0.3.048.tgz σε αυτό το παράδειγμα) στο C:\OpenFoam στον κόμβο κεφαλής, έτσι ώστε οι κόμβοι Linux να αποκτήσετε πρόσβαση σε αυτό το αρχείο από /openfoam. Στη συνέχεια, εκτελέστε **clusrun** για να εγκαταστήσετε το Intel MPI βιβλιοθήκη σε όλους τους κόμβους Linux.

1.  Τις παρακάτω εντολές Αντιγραφή του πακέτου εγκατάστασης και εξαγάγετε /opt/intel σε κάθε κόμβο.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Για να εγκαταστήσετε σιωπηρά Intel MPI βιβλιοθήκη, χρησιμοποιήστε ένα αρχείο silent.cfg. Μπορείτε να βρείτε ένα παράδειγμα σε τα δείγματα αρχείων στο τέλος αυτού του άρθρου. Τοποθετήστε αυτό το αρχείο στο /openfoam του κοινόχρηστου φακέλου. Για λεπτομέρειες σχετικά με το αρχείο silent.cfg, ανατρέξτε στο θέμα [Βιβλιοθήκη MPI Intel για τον Οδηγό εγκατάστασης Linux - σιωπηρή εγκατάσταση](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).

    >[AZURE.TIP]Βεβαιωθείτε ότι αποθηκεύετε το αρχείο silent.cfg σας ως αρχείο κειμένου με Linux καταλήξεις των γραμμών (LF μόνο, δεν CR LF). Αυτό το βήμα εξασφαλίζει ότι εκτελείται σωστά σε τους κόμβους Linux.

3.  Εγκαταστήστε το Intel MPI βιβλιοθήκη σε λειτουργία χωρίς μηνύματα.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### <a name="configure-mpi"></a>Ρύθμιση παραμέτρων MPI

Για τη δοκιμή, θα πρέπει να μπορείτε να προσθέσετε τις ακόλουθες γραμμές για να το /etc/security/limits.conf σε κάθε έναν από τους κόμβους Linux:


    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Επανεκκινήστε τους κόμβους Linux αφού ενημερώσετε το αρχείο limits.conf. Για παράδειγμα, χρησιμοποιήστε την ακόλουθη εντολή **clusrun** :

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Μετά την επανεκκίνηση, βεβαιωθείτε ότι έχει ενεργοποιηθεί στον κοινόχρηστο φάκελο ως /openfoam.

### <a name="compile-and-install-openfoam"></a>Μεταγλώττιση και εγκατάσταση OpenFOAM

Αποθηκεύστε το πακέτο εγκατάστασης για το πακέτο προέλευσης OpenFOAM (2.3.1.tgz OpenFOAM σε αυτό το παράδειγμα) για να C:\OpenFoam στον κόμβο κεφαλής, έτσι ώστε οι κόμβοι Linux να αποκτήσετε πρόσβαση σε αυτό το αρχείο από /openfoam. Στη συνέχεια, εκτελέστε τις εντολές **clusrun** για να μεταγλωττίσετε OpenFOAM σε όλους τους κόμβους Linux.


1.  Δημιουργήστε ένα φάκελο /opt/OpenFOAM σε κάθε κόμβο Linux, αντιγράψτε το πακέτο προέλευσης σε αυτόν το φάκελο και εξαγάγετε εκεί.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
    
    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  Για να μεταγλωττίσετε OpenFOAM με τη βιβλιοθήκη MPI Intel, ρυθμίστε πρώτα ορισμένες μεταβλητές περιβάλλοντος για Intel MPI και OpenFOAM. Χρησιμοποιήστε μια δέσμη ενεργειών πάρτι που ονομάζεται settings.sh για να ορίσετε τις μεταβλητές. Μπορείτε να βρείτε ένα παράδειγμα σε τα δείγματα αρχείων στο τέλος αυτού του άρθρου. Τοποθετήστε αυτό το αρχείο (αποθηκεύονται με τέλους γραμμής Linux) στο /openfoam του κοινόχρηστου φακέλου. Αυτό το αρχείο περιέχει επίσης ρυθμίσεις για την MPI και OpenFOAM χρόνους εκτέλεσης που χρησιμοποιείτε αργότερα για να εκτελέσετε μια εργασία OpenFOAM.

3. Εγκαταστήστε εξαρτώμενα πακέτων που απαιτούνται για τη συγκέντρωση OpenFOAM. Ανάλογα με την κατανομή Linux, ενδέχεται να πρέπει πρώτα να προσθέσετε ένα αποθετήριο δεδομένων. Εκτέλεση εντολών **clusrun** παρόμοιο με το εξής:

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    Εάν είναι απαραίτητο, SSH σε κάθε κόμβο Linux για να εκτελέσετε τις εντολές για να επιβεβαιώσετε ότι εκτελούνται σωστά.

4.  Εκτελέστε την παρακάτω εντολή για να μεταγλωττίσετε OpenFOAM. Η διαδικασία μεταγλώττισης παίρνει λίγο χρόνο για να ολοκληρώσετε και δημιουργεί μια μεγάλη ποσότητα των πληροφοριών καταγραφής τυπική έξοδο, επομένως χρησιμοποιήστε την επιλογή **/ παρεμβαλλόμενα** για να εμφανίσετε το αποτέλεσμα με παρεμβολή.

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]Το "\`" σύμβολο στην εντολή είναι ένα σύμβολο διαφυγής για το PowerShell. "\`&" σημαίνει ότι το "&" είναι μέρος της εντολής.

## <a name="prepare-to-run-an-openfoam-job"></a>Προετοιμασία για να εκτελέσετε μια εργασία OpenFOAM

Τώρα να προετοιμαστείτε για να εκτελέσετε μια εργασία MPI που ονομάζεται sloshingTank3D, το οποίο είναι ένα από τα δείγματα OpenFoam, σε δύο κόμβους Linux. 

### <a name="set-up-the-runtime-environment"></a>Ρύθμιση του περιβάλλοντος χρόνου εκτέλεσης

Για να ρυθμίσετε το περιβάλλον εκτέλεσης περιβάλλοντα για MPI και OpenFOAM σε τους κόμβους Linux, εκτελέστε την ακόλουθη εντολή σε ένα παράθυρο του Windows PowerShell στον κόμβο κεφαλίδας. (Αυτή η εντολή είναι έγκυροι για την SUSE Linux μόνο.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Προετοιμασία δειγμάτων δεδομένων

Χρησιμοποιήστε την κοινή χρήση κεφαλής κόμβο έχετε ρυθμίσει προηγουμένως για την κοινή χρήση αρχείων μεταξύ τους κόμβους Linux (ενεργοποιηθεί ως /openfoam).

1.  SSH σε μία από τις Linux υπολογίσετε κόμβους.

2.  Εκτελέστε την παρακάτω εντολή για να ρυθμίσετε το περιβάλλον χρόνου εκτέλεσης OpenFOAM, εάν δεν το έχετε κάνει ήδη.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  Αντιγράψτε το δείγμα sloshingTank3D στον κοινόχρηστο φάκελο και μεταβείτε σε αυτό.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  Όταν χρησιμοποιείτε τις προεπιλεγμένες παραμέτρους αυτού του δείγματος, ενδέχεται να χρειαστούν δεκάδες λεπτά για να εκτελέσετε, ώστε να μπορεί να θέλετε να τροποποιήσετε ορισμένες παραμέτρους για να εκτελείται πιο γρήγορα. Μία επιλογή απλό είναι να τροποποιήσετε το χρόνο βήμα μεταβλητές deltaT και writeInterval στο αρχείο συστήματος/controlDict. Αυτό το αρχείο αποθηκεύει όλα τα δεδομένα εισόδου που σχετίζονται με το στοιχείο ελέγχου του χρόνου και ανάγνωση και εγγραφή δεδομένων λύση. Για παράδειγμα, μπορείτε να αλλάξετε την τιμή του deltaT από 0.05 0,5 και την τιμή του writeInterval από 0.05 0,5.

    ![Τροποποίηση μεταβλητών βήμα][step_variables]

5.  Καθορίστε την επιθυμητή τιμές για τις μεταβλητές στο αρχείο συστήματος/decomposeParDict. Αυτό το παράδειγμα χρησιμοποιεί δύο Linux τους κόμβους κάθε με 8 πυρήνων, επομένως, ορίστε numberOfSubdomains έως 16 και n του hierarchicalCoeffs να (1 1 16), που σημαίνει εκτέλεση OpenFOAM παράλληλα με 16 διεργασίες. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [OpenFOAM Οδηγός: 3.4 εφαρμογές εκτελείται παράλληλα](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).

    ![Ανάλυση διαδικασιών][decompose]

6.  Εκτελέστε τις ακόλουθες εντολές από τον κατάλογο sloshingTank3D για να προετοιμάσετε το δείγμα δεδομένων.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  Στον κόμβο κεφαλίδας, θα πρέπει να βλέπετε τα δείγματα αρχείων δεδομένων αντιγράφονται στο C:\OpenFoam\sloshingTank3D. (C:\OpenFoam είναι κοινόχρηστο φάκελο στον κόμβο κεφαλής.)

    ![Αρχεία δεδομένων στον κόμβο κεφαλής][data_files]

### <a name="host-file-for-mpirun"></a>Αρχείο κεντρικού υπολογιστή για mpirun

Σε αυτό το βήμα, δημιουργείτε ένα αρχείο κεντρικού υπολογιστή (μια λίστα των κόμβους υπολογιστικών) το οποίο χρησιμοποιεί την εντολή **mpirun** .

1.  Σε έναν από τους κόμβους Linux, δημιουργήστε ένα αρχείο με το όνομα hostfile στην περιοχή /openfoam, ώστε να μπορούν να σας καλούν αυτού του αρχείου /openfoam/hostfile σε όλους τους κόμβους Linux.

2.  Η εγγραφή σας ονόματα κόμβου Linux σε αυτό το αρχείο. Σε αυτό το παράδειγμα, το αρχείο περιέχει τα ονόματα των εξής:
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]Μπορείτε επίσης να δημιουργήσετε αυτό το αρχείο στο C:\OpenFoam\hostfile στον κόμβο κεφαλίδας. Εάν επιλέξετε αυτήν την επιλογή, αποθηκεύστε το ως αρχείο κειμένου με Linux καταλήξεις των γραμμών (LF μόνο, δεν CR LF). Αυτό εξασφαλίζει ότι εκτελείται σωστά σε τους κόμβους Linux.

    **Πάρτι περιτυλίγματος δέσμης ενεργειών**

    Εάν έχετε πολλούς κόμβους Linux και θέλετε την εργασία σας για να εκτελέσετε μόνο σε μερικές από αυτές, δεν είναι καλή ιδέα να χρησιμοποιήσετε ένα αρχείο σταθερό κεντρικού υπολογιστή, επειδή δεν γνωρίζετε ποια κόμβους θα κατανέμεται με το έργο σας. Σε αυτήν την περίπτωση, γράψτε ένα πάρτι περιτυλίγματος δέσμη ενεργειών για **mpirun** για να δημιουργήσετε το αρχείο host αυτόματα. Μπορείτε να βρείτε ένα παράδειγμα πάρτι δέσμης ενεργειών περιτυλίγματος, που ονομάζεται hpcimpirun.sh στο τέλος αυτού του άρθρου και αποθηκεύστε το ως /openfoam/hpcimpirun.sh. Σε αυτό το παράδειγμα δέσμης ενεργειών κάνει τα εξής:

    1.  Ρυθμίζει τις μεταβλητές περιβάλλοντος για **mpirun**και ορισμένες παράμετροι εντολή Προσθήκη για να εκτελέσετε την εργασία MPI μέσω του δικτύου RDMA. Σε αυτήν την περίπτωση, ορίζει τις παρακάτω μεταβλητές:

        *   I_MPI_FABRICS = shm:dapl
        *   I_MPI_DAPL_PROVIDER = ενός-v2-ib0
        *   I_MPI_DYNAMIC_CONNECTION = 0

    2.  Δημιουργεί ένα αρχείο host σύμφωνα με το περιβάλλον μεταβλητή $CCP_NODES_CORES, η οποία έχει οριστεί από τον κόμβο κεφαλής HPC όταν είναι ενεργοποιημένη η εργασία.

        Η μορφή $CCP_NODES_CORES ακολουθεί αυτό το μοτίβο:

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        όπου

        * `<Number of nodes>`-τον αριθμό των κόμβους που έχει εκχωρηθεί σε αυτήν την εργασία.  
        
        * `<Name of node_n_...>`-το όνομα του κάθε κόμβου έχει εκχωρηθεί σε αυτήν την εργασία.
        
        * `<Cores of node_n_...>`-τον αριθμό των πυρήνων στον κόμβο έχει εκχωρηθεί σε αυτήν την εργασία.

        Για παράδειγμα, εάν η εργασία χρειάζεται δύο κόμβους για να εκτελέσετε, $CCP_NODES_CORES είναι παρόμοια με
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  Καλεί την εντολή **mpirun** και τοποθετεί δύο παραμέτρους στη γραμμή εντολών.

        * `--hostfile <hostfilepath>: <hostfilepath>`-τη διαδρομή του αρχείου του host δημιουργεί τη δέσμη ενεργειών

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-μια μεταβλητή περιβάλλοντος που έχει οριστεί από τον κόμβο κεφαλής πακέτο HPC, η οποία αποθηκεύει τον αριθμό του συνόλου πυρήνων έχει εκχωρηθεί σε αυτήν την εργασία. Σε αυτήν την περίπτωση, καθορίζει τον αριθμό των διεργασιών για **mpirun**.


## <a name="submit-an-openfoam-job"></a>Υποβάλετε μια εργασία OpenFOAM

Τώρα μπορείτε να υποβάλετε μια εργασία στη Διαχείριση σύμπλεγμα HPC. Πρέπει να μεταφέρουν το hpcimpirun.sh δέσμης ενεργειών του τις γραμμές εντολών για ορισμένες από τις εργασίες.

1. Σύνδεση με τον κόμβο κεφαλής συμπλέγματος και εκκίνηση της διαχείρισης σύμπλεγμα HPC.

2. **Στη Διαχείριση πόρων**, βεβαιωθείτε ότι οι κόμβοι υπολογισμού Linux είναι σε κατάσταση **Online** . Εάν δεν είναι, επιλέξτε τα και κάντε κλικ στην επιλογή **Σύνδεση**.

3.  **Διαχείριση του έργου**, κάντε κλικ στην επιλογή **Νέα εργασία**.

4.  Πληκτρολογήστε ένα όνομα για την εργασία, όπως _sloshingTank3D_.

    ![Λεπτομέρειες εργασίας][job_details]

5.  Στους **πόρους εργασίας**, επιλέξτε τον τύπο του πόρου ως "Κόμβος" και ορίστε την ελάχιστη σε 2. Αυτή η ρύθμιση παραμέτρων εκτελεί την εργασία σε δύο κόμβους Linux, κάθε μία από τις οποίες έχει οκτώ πυρήνων σε αυτό το παράδειγμα.

    ![Πόροι εργασίας][job_resources]

6. Κάντε κλικ στην επιλογή **Επεξεργασία εργασίες** στο αριστερό παράθυρο περιήγησης και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη** για να προσθέσετε μια εργασία στο έργο. Προσθήκη τεσσάρων εργασιών στο έργο με την παρακάτω εντολή γραμμές και τις ρυθμίσεις.

    >[AZURE.NOTE]Εκτέλεση `source /openfoam/settings.sh` ρυθμίζει τα περιβάλλοντα χρόνου εκτέλεσης OpenFOAM και MPI, ώστε κάθε μία από τις ακόλουθες εργασίες κλήσεις πριν από την εντολή OpenFOAM.

    *   **Εργασία 1**. Εκτελέστε το **decomposePar** για τη δημιουργία αρχείων δεδομένων για την εκτέλεση **interDyMFoam** παράλληλα.
    
        *   Εκχώρηση έναν κόμβο με την εργασία

        *   **Γραμμή εντολών** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **Κατάλογος εργασίας** - / openfoam/sloshingTank3D
        
        Ανατρέξτε στην παρακάτω εικόνα. Ρυθμίζετε ομοίως τις υπόλοιπες εργασίες.

        ![Λεπτομέρειες εργασίας 1][task_details1]

    *   **Εργασία 2**. Εκτελέστε **interDyMFoam** παράλληλα για τον υπολογισμό του δείγματος.

        *   Εκχώρηση δύο κόμβους με την εργασία

        *   **Γραμμή εντολών** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **Κατάλογος εργασίας** - / openfoam/sloshingTank3D

    *   **Εργασία 3**. Εκτελέστε το **reconstructPar** για να συγχωνεύσετε τα σύνολα καταλόγων ώρας από κάθε κατάλογο processor_N_ σε ένα σύνολο.

        *   Εκχώρηση έναν κόμβο με την εργασία

        *   **Γραμμή εντολών** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **Κατάλογος εργασίας** - / openfoam/sloshingTank3D

    *   Η **εργασία 4**. Εκτελέστε **foamToEnsight** παράλληλα για να μετατρέψετε τα αρχεία OpenFOAM αποτέλεσμα σε μορφή EnSight και να τοποθετήσετε τα αρχεία που EnSight σε έναν κατάλογο με το όνομα Ensight στον κατάλογο πεζών-κεφαλαίων.

        *   Εκχώρηση δύο κόμβους με την εργασία

        *   **Γραμμή εντολών** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **Κατάλογος εργασίας** - / openfoam/sloshingTank3D

6.  Προσθήκη εξαρτήσεις σε αυτές τις εργασίες σε αύξουσα σειρά εργασιών.

    ![Εξαρτήσεις εργασιών][task_dependencies]

7.  Κάντε κλικ στην επιλογή **Υποβολή** για να εκτελέσετε αυτήν την εργασία.

    Από προεπιλογή, το πακέτο HPC υποβάλλει την εργασία ως τον τρέχοντα λογαριασμό συνδεδεμένου χρήστη. Αφού κάνετε κλικ στο κουμπί " **Υποβολή**", ενδέχεται να δείτε ένα παράθυρο διαλόγου που σας ρωτά για να εισαγάγετε το όνομα χρήστη και τον κωδικό πρόσβασης.

    ![Τα διαπιστευτήρια εργασία][creds]

    Υπό ορισμένες συνθήκες, το πακέτο HPC απομνημονεύει τις πληροφορίες χρήστη, εισαγάγετε πριν από και δεν εμφανίζει το παράθυρο διαλόγου. Για να εμφανίσετε ξανά πακέτο HPC, πληκτρολογήστε την παρακάτω εντολή στη γραμμή εντολών και, στη συνέχεια, να υποβάλετε την εργασία.

    ```
    hpccred delcreds
    ```

8.  Η εργασία σας μεταφέρει από δεκάδες λεπτά σε μερικές ώρες σύμφωνα με τις παραμέτρους που έχετε ορίσει για το δείγμα. Στο χάρτη θερμότητας, μπορείτε να δείτε την εργασία που εκτελείται σε τους κόμβους Linux. 

    ![Χάρτης θερμότητας][heat_map]

    Σε κάθε κόμβο, ξεκινούν οκτώ διεργασίες.

    ![Διεργασίες Linux][linux_processes]

9.  Όταν ολοκληρωθεί η εργασία, βρείτε τα αποτελέσματα του έργου σε φακέλους κάτω από C:\OpenFoam\sloshingTank3D και τα αρχεία καταγραφής στο C:\OpenFoam.


## <a name="view-results-in-ensight"></a>Προβολή αποτελεσμάτων σε EnSight

Προαιρετικά Χρησιμοποιήστε [EnSight](https://www.ceisoftware.com/) για την απεικόνιση και να αναλύσετε τα αποτελέσματα της εργασίας OpenFOAM. Για περισσότερες πληροφορίες σχετικά με την απεικόνιση και κίνησης σε EnSight, ανατρέξτε στο θέμα αυτός ο [Οδηγός βίντεο](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1.  Μετά την εγκατάσταση EnSight στον κόμβο κεφαλής, ξεκινήστε το.

2.  Άνοιγμα C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.

    Μπορείτε να δείτε μια δεξαμενή στο πρόγραμμα προβολής.

    ![Δεξαμενή στο EnSight][tank]

3.  Δημιουργία μιας **Isosurface** από **internalMesh**και, στη συνέχεια, επιλέξτε τη μεταβλητή **alpha_water**.

    ![Δημιουργήστε μια isosurface][isosurface]

4.  Ορίστε το χρώμα για **Isosurface_part** που δημιουργήσατε στο προηγούμενο βήμα. Για παράδειγμα, ρυθμίσετε να νερό μπλε.

    ![Επεξεργασία isosurface χρώματος][isosurface_color]

5.  Δημιουργήστε έναν **Iso όγκου** από **τοίχοι** , επιλέγοντας **τοίχοι** στον πίνακα " **τμήματα** " και κάντε κλικ στο κουμπί **Isosurfaces** στη γραμμή εργαλείων.

6.  Στο παράθυρο διαλόγου, επιλέξτε **τον τύπο** ως **Isovolume** και ορίστε το ελάχιστο της **περιοχής Isovolume** σε 0,5. Για να δημιουργήσετε το isovolume, κάντε κλικ στην επιλογή **Δημιουργία με επιλεγμένα τμήματα**.

7.  Ορίστε το χρώμα για **Iso_volume_part** που δημιουργήσατε στο προηγούμενο βήμα. Για παράδειγμα, ρυθμίσετε να νερό Βαθύ μπλε.

8.  Ορίστε το χρώμα για **τοίχων**. Για παράδειγμα, ρυθμίσετε να διαφανή λευκό.

9. Τώρα κάντε κλικ στην επιλογή **αναπαραγωγή** για να δείτε τα αποτελέσματα της προσομοίωση.

    ![Αποτέλεσμα δεξαμενή][tank_result]

## <a name="sample-files"></a>Δείγματα αρχείων

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Δείγμα αρχείο ρύθμισης παραμέτρων XML για ανάπτυξη σύμπλεγμα με δέσμη ενεργειών του PowerShell

 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
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
### <a name="sample-silentcfg-file-to-install-mpi"></a>Δείγμα αρχείου silent.cfg για να εγκαταστήσετε το MPI

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Δείγμα δέσμης ενεργειών settings.sh

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###<a name="sample-hpcimpirunsh-script"></a>Δείγμα δέσμης ενεργειών hpcimpirun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png
