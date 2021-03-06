<properties
 pageTitle="Ρύθμιση του ένα σύμπλεγμα RDMA των Windows για να εκτελέσετε εφαρμογές MPI | Microsoft Azure"
 description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα σύμπλεγμα Windows HPC Pack με μέγεθος H16r, H16mr, A8 ή ΣΠΣ A9 για χρήση του δικτύου Azure RDMA για να εκτελέσετε MPI εφαρμογές."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/20/2016"
 ms.author="danlep"/>

# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Ρύθμιση του ένα σύμπλεγμα Windows RDMA με το πακέτο HPC για να εκτελέσετε εφαρμογές MPI

Ρυθμίστε ένα σύμπλεγμα Windows RDMA στο Azure με [Πακέτο HPC Microsoft](https://technet.microsoft.com/library/cc514029) και [H σειρά ή υπολογισμού στενής παρουσίες μια σειρά](virtual-machines-windows-a8-a9-a10-a11-specs.md) για να εκτελέσετε παράλληλη εφαρμογές μηνύματος που περνά μέσα περιβάλλοντος εργασίας (MPI). Όταν ρυθμίζετε κόμβους με δυνατότητα RDMA, με βάση το διακομιστή των Windows σε ένα σύμπλεγμα HPC πακέτο, MPI εφαρμογές επικοινωνίας αποτελεσματικά πάνω από ένα μικρό λανθάνοντα, υψηλής απόδοσης δικτύου στο Azure που βασίζεται σε απομακρυσμένο απευθείας μνήμης τεχνολογία πρόσβασης (RDMA).

Εάν θέλετε να εκτελέσετε MPI φόρτους εργασίας σε ΣΠΣ Linux που χρησιμοποιούν το δίκτυο Azure RDMA, ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα Linux RDMA για να εκτελέσετε MPI εφαρμογές](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="hpc-pack-cluster-deployment-options"></a>Επιλογές ανάπτυξης σύμπλεγμα HPC πακέτο
Πακέτο HPC Microsoft είναι ένα εργαλείο που παρέχονται χωρίς πρόσθετο κόστος για να δημιουργήσετε HPC συμπλεγμάτων εσωτερικής εγκατάστασης ή στο Azure για την εκτέλεση των Windows ή Linux HPC εφαρμογές. Πακέτο HPC περιλαμβάνει ένα περιβάλλον χρόνου εκτέλεσης για την εφαρμογή Microsoft της μηνύματος που περνά μέσα περιβάλλοντος εργασίας των Windows (MS-MPI). Όταν χρησιμοποιείται με με δυνατότητα RDMA παρουσιών που εκτελούνται ένα υποστηριζόμενο λειτουργικό σύστημα Windows Server, HPC Pack παρέχει μια αποτελεσματική επιλογή για να εκτελέσετε Windows MPI εφαρμογές που έχουν πρόσβαση στο δίκτυο Azure RDMA. 

Σε αυτό το άρθρο παρουσιάζει δύο σενάρια και συνδέσεις για λεπτομερείς οδηγίες για να ρυθμίσετε ένα σύμπλεγμα Winodws RDMA με το πακέτο HPC Microsoft. 

* Σενάριο 1. Ανάπτυξη των παρουσιών ρόλο υπολογισμού στενής εργαζόμενου (PaaS)

* Σενάριο 2. Αναπτύξτε τους κόμβους υπολογισμού υπολογισμού στενής ΣΠΣ (IaaS)

Για γενικές τις προϋποθέσεις για να χρησιμοποιήσετε υπολογισμού στενής παρουσίες με Windows, ανατρέξτε στο θέμα [σχετικά με τη σειρά H και υπολογισμού στενής ΣΠΣ μια σειρά](virtual-machines-windows-a8-a9-a10-a11-specs.md) .



## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Σενάριο 1. Ανάπτυξη των παρουσιών ρόλο υπολογισμού στενής εργαζόμενου (PaaS)

Από ένα υπάρχον σύμπλεγμα HPC πακέτο, προσθέστε επιπλέον υπολογισμού πόρων σε παρουσίες ρόλο Azure εργαζόμενου (Azure κόμβοι) που εκτελούνται σε μια υπηρεσία cloud (PaaS). Αυτήν τη δυνατότητα, που ονομάζεται επίσης "καταιγισμού να Azure" από το πακέτο HPC, υποστηρίζει μια ποικιλία μεγεθών για τις παρουσίες ρόλο εργασίας. Κατά την προσθήκη τους κόμβους Azure, απλώς καθορίστε μία από τις διαστάσεις του με δυνατότητα RDMA.

Ακολουθούν ζητήματα και βήματα που πρέπει να καταιγισμού σε με δυνατότητα RDMA Azure παρουσιών από ένα υπάρχον (συνήθως εσωτερικής εγκατάστασης) σύμπλεγμα. Χρησιμοποιήστε παρόμοια διαδικασίες για να προσθέσετε παρουσίες ρόλο εργαζόμενου σε έναν κόμβο κεφαλής HPC πακέτο που έχει αναπτυχθεί σε μια Εικονική Azure.

>[AZURE.NOTE] Για ένα πρόγραμμα εκμάθησης για να καταιγισμού να Azure με HPC Pack, ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα υβριδική με HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Σημειώστε τα θέματα στα παρακάτω βήματα που αφορούν συγκεκριμένα με δυνατότητα RDMA Azure κόμβους.

![Καταιγισμού να Azure][burst]

### <a name="steps"></a>Τα βήματα

4. **Ανάπτυξη και ρύθμιση παραμέτρων έναν κόμβο κεφαλής HPC πακέτο 2012 R2**

    Κάντε λήψη του πακέτου εγκατάστασης την πιο πρόσφατη HPC πακέτο από το [Κέντρο λήψης της Microsoft](https://www.microsoft.com/download/details.aspx?id=49922). Για απαιτήσεις και οδηγίες για να προετοιμαστείτε για μια ανάπτυξη του Azure καταιγισμού, ανατρέξτε στο θέμα [HPC πακέτου Οδηγού για γρήγορα αποτελέσματα](https://technet.microsoft.com/library/jj884144.aspx) και [καταιγισμού παρουσίες εργαζόμενου Azure με πακέτο HPC Microsoft](https://technet.microsoft.com/library/gg481749.aspx).

5. **Ρύθμιση παραμέτρων ενός πιστοποιητικού διαχείρισης στην συνδρομή του Azure**

    Ρύθμιση παραμέτρων ένα πιστοποιητικό για την ασφάλιση της σύνδεσης μεταξύ του κεφαλής κόμβο και Azure. Για επιλογές και διαδικασίες, ανατρέξτε στο θέμα [σενάρια για να ρυθμίσετε τις παραμέτρους του πιστοποιητικού διαχείρισης Azure για το πακέτο HPC](http://technet.microsoft.com/library/gg481759.aspx). Για αναπτύξεις δοκιμής, HPC Pack εγκαθιστά μια προεπιλεγμένη Microsoft HPC Azure πιστοποιητικού διαχείρισης μπορείτε γρήγορα να στείλετε στο Azure τη συνδρομή σας.

6. **Δημιουργήστε μια νέα υπηρεσία στο cloud και ένα λογαριασμό του χώρου αποθήκευσης**

    Χρησιμοποιήστε Azure κλασική πύλη για να δημιουργήσετε μια υπηρεσία στο cloud και ένα λογαριασμό χώρου αποθήκευσης για την ανάπτυξη σε μια περιοχή όπου οι εμφανίσεις με δυνατότητα RDMA είναι διαθέσιμες.

7. **Δημιουργία προτύπου Azure κόμβου**

    Χρησιμοποιήστε τη δημιουργία Οδηγός προτύπου κόμβου στη Διαχείριση σύμπλεγμα HPC. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία προτύπου Azure κόμβου](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) στο "Βήματα για να αναπτύξετε Azure κόμβους με HPC πακέτο Microsoft".

    Για το αρχικό δοκιμές, προτείνουμε τη ρύθμιση των παραμέτρων μιας πολιτικής διαθεσιμότητα μη αυτόματης στο πρότυπο.

8. **Προσθήκη κόμβοι στο σύμπλεγμα**

    Χρήση του οδηγού κόμβο προσθήκης στη Διαχείριση σύμπλεγμα HPC. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Προσθήκη κόμβους Azure στο σύμπλεγμα HPC των Windows](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).

    Κατά τον καθορισμό του μεγέθους τους κόμβους, επιλέξτε μία από τις διαστάσεις του με δυνατότητα RDMA παρουσία.
    
    >[AZURE.NOTE]Σε κάθε καταιγισμού σε ανάπτυξη Azure με τις παρουσίες υπολογισμού στενής, πακέτο HPC αναπτύσσει αυτόματα τουλάχιστον 2 με δυνατότητα RDMA παρουσίες (όπως A8) ως κόμβοι διακομιστή μεσολάβησης, εκτός από τις παρουσίες ρόλο Azure εργασίας που καθορίζετε. Οι κόμβοι διακομιστή μεσολάβησης Χρησιμοποιήστε πυρήνων που έχουν εκχωρηθεί σε τη συνδρομή και χρεωθείτε μαζί με τις παρουσίες ρόλο Azure εργασίας.

9. **Έναρξη (προετοιμασία) τους κόμβους και μεταφέρετέ τα online για να εκτελέσετε εργασίες**

    Επιλέξτε τους κόμβους και να χρησιμοποιήσετε την ενέργεια **Εκκίνηση** στη Διαχείριση σύμπλεγμα HPC. Όταν ολοκληρωθεί η προμήθεια, επιλέξτε τους κόμβους και χρησιμοποιήστε την ενέργεια **Σύνδεση** στη Διαχείριση σύμπλεγμα HPC. Οι κόμβοι είναι έτοιμη για να εκτελέσετε εργασίες.

10. **Υποβολή εργασιών στο σύμπλεγμα**

    Χρησιμοποιήστε πακέτο HPC εργασία υποβολής εργαλεία για να εκτελέσετε εργασίες σύμπλεγμα. Ανατρέξτε στο θέμα [πακέτο Microsoft HPC: Διαχείριση έργων](http://technet.microsoft.com/library/jj899585.aspx).

11. **Διακοπή (διαχειριστείτε) τους κόμβους**

    Όταν ολοκληρώσετε την εκτέλεση εργασιών, μεταφέρετε τους κόμβους εκτός σύνδεσης και να χρησιμοποιήσετε την ενέργεια **Διακοπή** στη Διαχείριση σύμπλεγμα HPC.





## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Σενάριο 2. Αναπτύξτε τους κόμβους υπολογισμού υπολογισμού στενής ΣΠΣ (IaaS)

Σε αυτό το σενάριο, αναπτύξτε τον κόμβο κεφαλής HPC πακέτο και σύμπλεγμα τον υπολογισμό κόμβους σε VM συνδεθεί σε έναν τομέα της υπηρεσίας καταλόγου Active Directory σε μια Azure εικονικού δικτύου. Πακέτο HPC παρέχει έναν αριθμό [επιλογών ανάπτυξης σε ΣΠΣ Azure](virtual-machines-linux-hpcpack-cluster-options.md), συμπεριλαμβανομένων των δεσμών ενεργειών αυτοματοποιημένης ανάπτυξης και πρότυπα Azure γρήγορη έναρξη. Ως παράδειγμα, τα ζητήματα και βήματα κάτω από το στοιχείο Οδηγός μπορείτε να χρησιμοποιήσετε τη [δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) για την αυτοματοποίηση περισσότερες από αυτήν τη διαδικασία.

![Σύμπλεγμα στο ΣΠΣ Azure][iaas]



### <a name="steps"></a>Τα βήματα

1. **Δημιουργήστε έναν κόμβο κεφαλής συμπλέγματος και τον υπολογισμό κόμβο ΣΠΣ, εκτελώντας το δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS σε έναν υπολογιστή-πελάτη**

    Λήψη του πακέτου δέσμη ενεργειών ανάπτυξης IaaS HPC πακέτο από το [Κέντρο λήψης της Microsoft](https://www.microsoft.com/download/details.aspx?id=49922).

    Για την προετοιμασία του υπολογιστή-πελάτη, δημιουργήστε το αρχείο ρύθμισης παραμέτρων δέσμης ενεργειών, και εκτελέστε τη δέσμη ενεργειών, ανατρέξτε στο θέμα [Δημιουργία ένα σύμπλεγμα HPC με τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). 
    
    Για να αναπτύξετε με δυνατότητα RDMA κόμβους υπολογιστικών, σημειώστε τα ακόλουθα θέματα επιπλέον:
    
    * **Εικονική δικτύου** - Καθορίστε ένα νέο εικονικό δίκτυο σε μια περιοχή στην οποία είναι διαθέσιμο το με δυνατότητα RDMA παρουσία μέγεθος που θέλετε να χρησιμοποιήσετε.

    * **Λειτουργικό σύστημα Windows Server** - για την υποστήριξη συνδεσιμότητας RDMA, καθορίστε ένα λειτουργικό σύστημα Windows Server 2012 R2 ή Windows Server 2012 για τον κόμβο υπολογισμού ΣΠΣ.

    * **Υπηρεσίες cloud** - συνιστάται να την ανάπτυξη κεφαλής κόμβο σας στην υπηρεσία cloud μία και σας κόμβους υπολογιστικών σε μια υπηρεσία cloud διαφορετικά.

    * **Μέγεθος κόμβο κεφαλή** - για αυτό το σενάριο, εξετάστε το ενδεχόμενο να μέγεθος τουλάχιστον A4 (επιπλέον μεγάλα) για τον κόμβο κεφαλίδας.

    * **Επέκταση HpcVmDrivers** - η δέσμη ενεργειών ανάπτυξης εγκαθιστά τον παράγοντα Εικονική Azure και την επέκταση HpcVmDrivers αυτόματα κατά την ανάπτυξη μέγεθος A8 ή A9 τον υπολογισμό κόμβους με λειτουργικό σύστημα Windows Server. HpcVmDrivers εγκαθιστά προγράμματα οδήγησης στον κόμβο υπολογισμού ΣΠΣ, ώστε να μπορούν να συνδεθούν στο δίκτυο RDMA.

    * **Ρύθμιση παραμέτρων του συμπλέγματος δικτύου** - η δέσμη ενεργειών ανάπτυξης ρυθμίζει αυτόματα το σύμπλεγμα HPC πακέτο τοπολογία 5 (όλους τους κόμβους του εταιρικού δικτύου). Αυτή η τοπολογία είναι απαραίτητη για όλες τις αναπτύξεις σύμπλεγμα HPC πακέτο στο ΣΠΣ. Μην αλλάξετε την τοπολογία του δικτύου συμπλέγματος αργότερα.

2. **Μεταφορά κόμβους υπολογιστικών online για να εκτελέσετε εργασίες**

    Επιλέξτε τους κόμβους και να χρησιμοποιήσετε την ενέργεια **Σύνδεση** στη Διαχείριση σύμπλεγμα HPC. Οι κόμβοι είναι έτοιμη για να εκτελέσετε εργασίες.

3. **Υποβολή εργασιών στο σύμπλεγμα**

    Σύνδεση με τον κόμβο κεφαλής να υποβάλλουν εργασίες, ή να ρυθμίσετε έναν υπολογιστή εσωτερικής εγκατάστασης για να το κάνετε αυτό. Για πληροφορίες, ανατρέξτε στο θέμα [Υποβολή εργασιών σε ένα σύμπλεγμα HPC στο Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

4. **Μεταφέρετε τους κόμβους εκτός σύνδεσης "και" Διακοπή (Κατάργηση εκχώρησης) τους**

    Όταν ολοκληρώσετε την εκτέλεση εργασιών, διαρκέσει τους κόμβους για εργασία χωρίς σύνδεση στη Διαχείριση σύμπλεγμα HPC. Στη συνέχεια, χρησιμοποιήστε τα εργαλεία διαχείρισης Azure για να τερματίσετε τους.



## <a name="run-mpi-applications-on-the-cluster"></a>Εκτέλεση εφαρμογών MPI στο σύμπλεγμα

### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Παράδειγμα: Εκτέλεση mpipingpong σε ένα σύμπλεγμα HPC πακέτο

Για να επαληθεύσετε μια ανάπτυξη του πακέτου HPC των παρουσιών με δυνατότητα RDMA, εκτελέστε την εντολή **mpipingpong** HPC πακέτο στο σύμπλεγμα. **mpipingpong** αποστέλλει πακέτα δεδομένων μεταξύ ζεύγη κόμβους TAB, για να υπολογίσετε λανθάνων χρόνος και μετρήσεις μετάδοσης και στατιστικά στοιχεία για το δίκτυο με δυνατότητα RDMA εφαρμογής. Αυτό το παράδειγμα δείχνει ένα τυπικό μοτίβο για την εκτέλεση μιας εργασίας MPI (σε αυτήν την περίπτωση, **mpipingpong**), χρησιμοποιώντας την εντολή **mpiexec** σύμπλεγμα.

Αυτό το παράδειγμα προϋποθέτει ότι έχετε προσθέσει Azure κόμβους σε μια ρύθμιση παραμέτρων "καταιγισμού να Azure" ([Σενάριο 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS) in this article). Εάν αναπτύξει το πακέτο HPC σε ένα σύμπλεγμα του Azure ΣΠΣ, θα χρειαστεί να τροποποιήσετε τη σύνταξη της εντολής για να καθορίσετε μια ομάδα διαφορετικό κόμβο και να ορίσετε πρόσθετες μεταβλητές περιβάλλοντος για να κατευθύνουν στο δίκτυο RDMA κίνηση του δικτύου.


Για να εκτελέσετε mpipingpong στο σύμπλεγμα:


1. Στον κόμβο κεφαλής ή σε σωστά ρυθμισμένο προγράμματος-πελάτη υπολογιστή, ανοίξτε μια γραμμή εντολών.

2. Για να εκτιμήσετε λανθάνων χρόνος ανάμεσα σε ζεύγη κόμβους σε μια ανάπτυξη του Azure καταιγισμού από 4 κόμβους, πληκτρολογήστε την παρακάτω εντολή για να υποβάλετε μια εργασία για να εκτελέσετε mpipingpong με μέγεθος μικρές πακέτου και μεγάλου αριθμού διαδοχικών προσεγγίσεων:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    Η εντολή επιστρέφει το Αναγνωριστικό της εργασίας που έχει υποβληθεί.

    Εάν αναπτυχθεί το σύμπλεγμα HPC πακέτο αναπτυχθεί σε ΣΠΣ Azure, καθορίστε μια ομάδα κόμβο που περιέχει τον υπολογισμό κόμβο ΣΠΣ αναπτυχθεί σε μια υπηρεσία cloud μία και τροποποιήστε την εντολή **mpiexec** ως εξής:

    ```
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```

3. Όταν ολοκληρωθεί η εργασία, για να προβάλετε το αποτέλεσμα (σε αυτήν την περίπτωση, το αποτέλεσμα της εργασίας 1 της εργασίας), πληκτρολογήστε τα εξής

    ```
    task view <JobID>.1
    ```

    όπου &lt; *JobID* &gt; είναι το Αναγνωριστικό της εργασίας που έχει υποβληθεί.

    Το αποτέλεσμα θα περιλαμβάνει αποτελέσματα λανθάνων χρόνος παρόμοια με τα εξής.

    ![Λανθάνων χρόνος pong ping][pingpong1]

4. Για να εκτιμήσετε μετάδοσης ανάμεσα σε ζεύγη Azure καταιγισμού κόμβους, πληκτρολογήστε την παρακάτω εντολή για να υποβάλετε μια εργασία για να εκτελέσετε **mpipingpong** με ένα πακέτο μεγάλο μέγεθος και ένα μικρό αριθμό των διαδοχικών προσεγγίσεων:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    Η εντολή επιστρέφει το Αναγνωριστικό της εργασίας που έχει υποβληθεί.

    Σε ένα σύμπλεγμα HPC πακέτο αναπτυχθεί σε ΣΠΣ Azure, τροποποιήστε την εντολή που σημειώσατε στο βήμα 2.

5. Όταν ολοκληρωθεί η εργασία, για να προβάλετε το αποτέλεσμα (σε αυτήν την περίπτωση, το αποτέλεσμα της εργασίας 1 της εργασίας), πληκτρολογήστε τα εξής:

    ```
    task view <JobID>.1
    ```

  Το αποτέλεσμα θα περιλαμβάνει αποτελέσματα μετάδοσης παρόμοια με τα εξής.

  ![Εντολή ping pong μετάδοσης][pingpong2]


### <a name="mpi-application-considerations"></a>Θέματα εφαρμογής MPI


Ακολουθούν ζητήματα για την εκτέλεση MPI εφαρμογών με το πακέτο HPC στο Azure. Ορισμένες ισχύουν μόνο για υλοποιήσεις του Azure κόμβους (εργαζόμενου παρουσίες ρόλο προστεθεί σε μια ρύθμιση παραμέτρων "καταιγισμού να Azure").

* Παρουσίες ρόλο εργαζόμενου σε μια υπηρεσία cloud είναι περιοδικά reprovisioned χωρίς ειδοποίηση με Azure (για παράδειγμα, για συντήρησης συστήματος ή σε περίπτωση που μια παρουσία αποτυγχάνει). Εάν μια παρουσία είναι reprovisioned ενώ εκτελείται μια εργασία MPI, την παρουσία χάσει τα δεδομένα και επιστρέφει την κατάσταση όταν πρώτα αναπτύχθηκε, που μπορούν να προκαλέσουν την αποτυχία της εργασίας MPI. Οι περισσότερες κόμβοι που χρησιμοποιείτε για μία μόνο εργασία MPI και περισσότερο την εργασία που εκτελείται, τόσο πιο πιθανό ότι μία από τις παρουσίες θα είναι reprovisioned ενώ μια εργασία εκτελείται. Επίσης το ενδεχόμενο αυτό εάν καθορίσετε έναν μεμονωμένο κόμβο στην ανάπτυξη ως διακομιστής αρχείων.


* Για να εκτελέσετε εργασίες MPI στο Azure, δεν χρειάζεται να χρησιμοποιήσετε τις παρουσίες με δυνατότητα RDMA. Μπορείτε να χρησιμοποιήσετε οποιοδήποτε μέγεθος παρουσία που υποστηρίζεται από το πακέτο HPC. Ωστόσο, οι εμφανίσεις με δυνατότητα RDMA συνιστώνται για εκτέλεση σχετικά ευρείας κλίμακας εργασιών MPI που διάκριση πεζών-κεφαλαίων το λανθάνοντος χρόνου και το εύρος ζώνης του δικτύου που συνδέει τους κόμβους. Εάν χρησιμοποιείτε άλλες μεγέθη για να εκτελέσετε εργασίες MPI και το εύρος ζώνης-διάκριση αδράνειας, συνιστάται να εκτελείτε μικρές εργασίες, στην οποία εκτελείται σε μερικά κόμβους μόνο μία εργασία.

* Εφαρμογές που έχουν αναπτυχθεί σε παρουσίες Azure είναι διέπεται από τους όρους άδειας χρήσης που σχετίζεται με την εφαρμογή. Επικοινωνήστε με τον προμηθευτή του εμπορική αίτηση για άδεια ή άλλους περιορισμούς για την εκτέλεση του στο cloud. Δεν όλους τους προμηθευτές προσφέρουν διανεμητική παραχώρησης αδειών χρήσης.


* Azure παρουσίες χρειάζεται περαιτέρω ρύθμιση σε access εσωτερικής εγκατάστασης κόμβους, κοινόχρηστα στοιχεία και διακομιστές αδειών χρήσης. Για παράδειγμα, για να ενεργοποιήσετε το Azure κόμβους για να αποκτήσετε πρόσβαση σε διακομιστή στην εσωτερική εγκατάσταση άδειας χρήσης, μπορείτε να ρυθμίσετε ένα εικονικό δίκτυο Azure--τοποθεσίας.


* Για να εκτελέσετε εφαρμογές MPI σε Azure παρουσίες, καταχωρήστε κάθε εφαρμογή MPI τείχος προστασίας των Windows σε τις παρουσίες, εκτελέστε την εντολή **hpcfwutil** . Αυτό σας επιτρέπει επικοινωνίες MPI να πραγματοποιείται σε μια θύρα που έχει εκχωρηθεί δυναμικά από το τείχος προστασίας.

    >[AZURE.NOTE] Για καταιγισμού να Azure αναπτύξεις, μπορείτε επίσης να ρυθμίσετε μια εντολή εξαίρεση τείχος προστασίας ώστε να εκτελείται αυτόματα σε όλους νέα Azure τους κόμβους που προστίθενται σε το σύμπλεγμά σας. Μετά την εκτέλεση της εντολής **hpcfwutil** και βεβαιωθείτε ότι λειτουργεί η εφαρμογή σας, προσθέστε την εντολή σε μια δέσμη ενεργειών εκκίνησης για το Azure κόμβους. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [χρήση μιας δέσμης ενεργειών εκκίνησης για τους κόμβους Azure](https://technet.microsoft.com/library/jj899632.aspx).



* Πακέτο HPC χρησιμοποιεί τη μεταβλητή περιβάλλοντος σύμπλεγμα CCP_MPI_NETMASK για να καθορίσετε μια περιοχή αποδεκτή διευθύνσεων για την επικοινωνία MPI. Εκκίνηση στο πακέτο HPC 2012 R2, η μεταβλητή περιβάλλοντος σύμπλεγμα CCP_MPI_NETMASK επηρεάζει μόνο MPI επικοινωνία μεταξύ τους κόμβους τομέα σύμπλεγμα υπολογισμού (είτε στην εσωτερική εγκατάσταση είτε σε ΣΠΣ Azure). Η μεταβλητή παραβλέπεται από κόμβους προστεθεί στη διάρκεια ενός καταιγισμού Azure ρύθμισης παραμέτρων.


* Εργασίες MPI δεν είναι δυνατό να δημιουργήσετε όλων των Azure παρουσιών που έχουν αναπτυχθεί σε διαφορετική cloud services (για παράδειγμα, σε καταιγισμού να Azure αναπτύξεις με διαφορετικό κόμβο πρότυπα ή κόμβους υπολογιστικών Εικονική Azure αναπτυχθεί σε πολλές υπηρεσίες cloud). Εάν έχετε πολλές αναπτύξεις Azure κόμβου που ξεκινούν με διαφορετικό κόμβο πρότυπα, η εργασία MPI πρέπει να εκτελέσετε μόνο ένα σύνολο Azure κόμβους.


* Όταν προσθέτετε κόμβους Azure για το σύμπλεγμα και μεταφέρετε σε σύνδεση, η υπηρεσία του χρονοδιαγράμματος έργου HPC αμέσως προσπαθεί να ξεκινήσει εργασίες σε τους κόμβους. Εάν μόνο ένα τμήμα του φόρτου εργασίας μπορούν να εκτελεστούν σε Azure, βεβαιωθείτε ότι μπορείτε να ενημερώσετε ή να δημιουργήσετε πρότυπα έργων για να καθορίσετε τι μπορούν να εκτελεστούν τύπους εργασιών στην Azure. Για παράδειγμα, για να εξασφαλίσετε ότι οι εργασίες που υποβάλλεται με ένα πρότυπο έργου εκτελείται μόνο σε Azure κόμβους, προσθέστε την ιδιότητα ομάδες κόμβο με το πρότυπο έργων και επιλέξτε AzureNodes ως η τιμή απαιτείται. Για να δημιουργήσετε προσαρμοσμένες ομάδες για το Azure κόμβους, χρησιμοποιήστε το cmdlet του PowerShell HPC Προσθήκη HpcGroup.


## <a name="next-steps"></a>Επόμενα βήματα

* Ως εναλλακτική λύση για τη χρήση του πακέτου HPC, αναπτύξτε με την υπηρεσία δέσμη Azure να εκτελούν εφαρμογές MPI σε διαχειριζόμενες χώρους συγκέντρωσης του κόμβους υπολογιστικών στο Azure. Ανατρέξτε στο θέμα [χρήση πολλών παρουσιών εργασίες να εκτελείτε εφαρμογές μήνυμα διέρχεται περιβάλλοντος εργασίας (MPI) σε δέσμη Azure](../batch/batch-mpi.md).

* Εάν θέλετε να εκτελέσετε Linux MPI εφαρμογές που έχουν πρόσβαση στο δίκτυο Azure RDMA, ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα Linux RDMA για να εκτελέσετε MPI εφαρμογές](virtual-machines-linux-classic-rdma-cluster.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/burst.png
[iaas]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/iaas.png
[pingpong1]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong2.png