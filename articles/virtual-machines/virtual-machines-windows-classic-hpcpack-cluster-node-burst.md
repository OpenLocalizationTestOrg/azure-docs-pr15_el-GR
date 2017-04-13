<properties
 pageTitle="Προσθήκη καταιγισμού κόμβους σε ένα σύμπλεγμα HPC πακέτο | Microsoft Azure"
 description="Μάθετε πώς μπορείτε να αναπτύξετε ένα σύμπλεγμα HPC πακέτο Azure στη ζήτηση, προσθέτοντας εργαζόμενου ρόλο παρουσιών που εκτελούνται σε μια υπηρεσία στο cloud"
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
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Προσθήκη κόμβους on demand "καταιγισμού" σε ένα σύμπλεγμα HPC πακέτο στο Azure



Εάν ρυθμίσετε ένα σύμπλεγμα [Πακέτο HPC Microsoft](https://technet.microsoft.com/library/cc514029) στο Azure, ίσως θέλετε ένας τρόπος για να γρήγορα κλιμακωθεί η δυναμικότητα σύμπλεγμα προς τα επάνω ή προς τα κάτω, χωρίς να διατηρηθεί ένα σύνολο προδιαμορφωμένο κόμβος ΣΠΣ. Σε αυτό το άρθρο θα μάθετε πώς να προσθέσετε κόμβους on demand "καταιγισμού" (παρουσίες ρόλο εργασίας που εκτελούνται σε μια υπηρεσία cloud) ως πόρους υπολογισμού σε μια κεφαλή κόμβο στο Azure. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Οι κόμβοι καταιγισμού][burst]

Τα βήματα σε αυτό το άρθρο θα σας βοηθήσει να προσθέσετε Azure κόμβους γρήγορα σε έναν βασίζεται στο cloud πακέτο HPC κεφαλής κόμβο Εικονική για έλεγχο ή απόδειξη λειτουργίας ανάπτυξης. Τα βήματα υψηλού επιπέδου είναι το ίδιο με τα βήματα για να "καταιγισμού να Azure" για να προσθέσετε cloud υπολογιστική χωρητικότητα σε ένα σύμπλεγμα HPC πακέτο εσωτερικής εγκατάστασης. Για ένα πρόγραμμα εκμάθησης, ανατρέξτε στο θέμα [Ρύθμιση υβριδική τον υπολογισμό σύμπλεγμα με πακέτο HPC Microsoft](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Για λεπτομερείς οδηγίες και ζητήματα για παραγωγής αναπτύξεις, ανατρέξτε στο θέμα [καταιγισμού να Azure με πακέτο HPC Microsoft](https://technet.microsoft.com/library/gg481749.aspx).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Το **πακέτο HPC κεφαλής κόμβο αναπτυχθεί σε μια Εικονική Azure** - μπορείτε να χρησιμοποιήσετε έναν μεμονωμένο κόμβο κεφαλής Εικονική ή ένα που αποτελεί τμήμα ενός μεγαλύτερου συμπλέγματος. Για να δημιουργήσετε έναν μεμονωμένο κόμβο κεφαλής, ανατρέξτε στο θέμα [Ανάπτυξη έναν κόμβο HPC πακέτο κεφαλή σε μια Εικονική Azure](virtual-machines-windows-hpcpack-cluster-headnode.md). Για αυτοματοποιημένη επιλογές ανάπτυξης σύμπλεγμα HPC πακέτο, ανατρέξτε στο θέμα [Επιλογές για να δημιουργήσετε και να διαχειριστείτε ένα σύμπλεγμα Windows HPC στο Azure με πακέτο HPC Microsoft](virtual-machines-windows-hpcpack-cluster-options.md).

    >[AZURE.TIP] Εάν χρησιμοποιείτε τη [δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) για να δημιουργήσετε το σύμπλεγμα στο Azure, μπορείτε να συμπεριλάβετε τους κόμβους Azure καταιγισμού στην ανάπτυξή σας αυτοματοποιημένη. Δείτε τα παραδείγματα σε αυτό το άρθρο.

* **Azure συνδρομή** - για να προσθέσετε Azure κόμβους, μπορείτε να επιλέξετε την ίδια συνδρομή που χρησιμοποιείται για την ανάπτυξη στον κόμβο κεφαλής Εικονική ή μια διαφορετική συνδρομή του (ή συνδρομών).

* **Όριο πυρήνων** - ίσως χρειαστεί να αυξήσετε το όριο πυρήνων, ειδικά εάν επιλέξετε να αναπτύξετε πολλές Azure κόμβους με πολλών πυρήνων μεγέθη. Για να αυξήσετε ένα όριο, [Ανοίξτε μια αίτηση υποστήριξης πελατών online](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) χωρίς χρέωση.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Βήμα 1: Δημιουργήστε μια υπηρεσία στο cloud και ένα λογαριασμό χώρου αποθήκευσης για τους κόμβους Azure

Χρησιμοποιήστε τις Azure κλασική πύλη ή ισοδύναμη εργαλεία για τη ρύθμιση παραμέτρων στους παρακάτω πόρους που απαιτούνται για την ανάπτυξη του Azure κόμβους:

* Μια νέα υπηρεσία Azure cloud
* Νέο λογαριασμό του Azure χώρου αποθήκευσης

>[AZURE.NOTE] Μην χρησιμοποιήσετε ξανά μια υπάρχουσα υπηρεσία cloud στη συνδρομή σας. 

**Ζητήματα**

* Ρύθμιση παραμέτρων σε μια υπηρεσία cloud ξεχωριστά για κάθε πρότυπο Azure κόμβου που σκοπεύετε να δημιουργήσετε. Ωστόσο, μπορείτε να χρησιμοποιήσετε τον ίδιο λογαριασμό χώρου αποθήκευσης για πολλά πρότυπα κόμβο.

* Συνιστάται να εντοπίσετε την υπηρεσία cloud και το λογαριασμό χώρου αποθήκευσης για την ανάπτυξη στην ίδια περιοχή Azure.




## <a name="step-2-configure-an-azure-management-certificate"></a>Βήμα 2: Ρύθμιση παραμέτρων ενός πιστοποιητικού Azure διαχείρισης

Για να προσθέσετε Azure κόμβους ως πόρους υπολογισμού, χρειάζεστε ένα πιστοποιητικό διαχείρισης στην κεφαλή κόμβο και αποστολή ένα αντίστοιχο πιστοποιητικού με τη συνδρομή Azure που χρησιμοποιείται για την ανάπτυξη.

Για αυτό το σενάριο, μπορείτε να επιλέξετε το **Προεπιλεγμένο HPC Azure πιστοποιητικού διαχείρισης** που HPC πακέτο εγκαθιστά και ρυθμίζει αυτόματα στον κόμβο κεφαλίδας. Αυτό το πιστοποιητικό είναι χρήσιμη για τον έλεγχο σκοπούς και αναπτύξεις απόδειξη λειτουργίας. Για να χρησιμοποιήσετε αυτό το πιστοποιητικό, αποστείλετε το αρχείο 2012\Bin\hpccert.cer C:\Program Files\Microsoft HPC πακέτο από τον κόμβο κεφαλής Εικονική με τη συνδρομή. Για να αποστείλετε το πιστοποιητικό στην [πύλη του Azure κλασική](https://manage.windowsazure.com), κάντε κλικ στην επιλογή **Ρυθμίσεις** > **Διαχείρισης πιστοποιητικά**.

Για πρόσθετες επιλογές για να ρυθμίσετε τις παραμέτρους του πιστοποιητικού διαχείρισης, ανατρέξτε στο θέμα [σενάρια για να ρυθμίσετε τις παραμέτρους του πιστοποιητικού διαχείρισης Azure για αναπτύξεις καταιγισμού Azure](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Βήμα 3: Ανάπτυξη Azure κόμβοι στο σύμπλεγμα



Τα βήματα για να προσθέσετε και να ξεκινήσετε Azure κόμβους σε αυτό το σενάριο γενικά είναι το ίδιο με τα βήματα με έναν κόμβο κεφαλής εσωτερικής εγκατάστασης. Για περισσότερες πληροφορίες, ανατρέξτε στις παρακάτω ενότητες σε [βήματα για την ανάπτυξη κόμβους Azure με πακέτο HPC Microsoft](https://technet.microsoft.com/library/gg481758.aspx):

* Δημιουργία προτύπου Azure κόμβου

* Προσθήκη Azure κόμβοι στο σύμπλεγμα HPC των Windows

* Έναρξη κόμβους (προετοιμασία) το Azure

Αφού προσθέσετε και ξεκινήστε τους κόμβους, είναι έτοιμος για να χρησιμοποιήσετε για να εκτελέσετε εργασίες σύμπλεγμα.

Εάν αντιμετωπίσετε προβλήματα κατά την ανάπτυξη Azure κόμβους, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων αναπτύξεων του Azure κόμβοι με πακέτο HPC Microsoft](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Επόμενα βήματα

* Για να χρησιμοποιήσετε ένα μέγεθος υπολογισμού στενής παρουσία για τους κόμβους καταιγισμού, ανατρέξτε στο θέμα τα ζητήματα στο [σχετικά με τη σειρά H και υπολογισμού στενής ΣΠΣ μια σειρά](virtual-machines-windows-a8-a9-a10-a11-specs.md).

* Εάν θέλετε για την αυτόματη μεγέθυνση ή σμίκρυνση του Azure υπολογιστικών πόρων σύμφωνα με το φόρτο εργασίας σύμπλεγμα, ανατρέξτε στο θέμα [Αυτόματη μεγέθυνση και σμίκρυνση Azure υπολογισμού πόρων σε ένα σύμπλεγμα HPC πακέτο](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png