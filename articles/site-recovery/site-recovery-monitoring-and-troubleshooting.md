<properties
    pageTitle="Παρακολούθηση και αντιμετώπιση προβλημάτων προστασίας για εικονικές μηχανές και φυσική διακομιστές | Microsoft Auzre" 
    description="Azure Επαναφορά τοποθεσίας συντεταγμένες την αναπαραγωγή, ανακατεύθυνσης και αξιοποίηση των εικονικές μηχανές βρίσκεται σε διακομιστές εσωτερικής Azure ή μια δευτερεύουσα κέντρου δεδομένων. Χρησιμοποιήστε αυτό το άρθρο για την παρακολούθηση και αντιμετώπιση προβλημάτων προστασίας VMM ή τοποθεσία Hyper-V." 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/13/2016"    
    ms.author="rajanaki"/>
    
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Παρακολούθηση και αντιμετώπιση προβλημάτων προστασίας για εικονικές μηχανές και φυσική διακομιστών

Αυτό παρακολούθησης και αντιμετώπιση προβλημάτων οδηγός σάς επιτρέπει να μάθετε παρακολουθεί την εύρυθμη λειτουργία αναπαραγωγής και τεχνικές αντιμετώπισης προβλημάτων για την ανάκτηση της τοποθεσίας Azure.

## <a name="understanding-the-components"></a>Κατανόηση των στοιχείων

### <a name="vmwarephysical-site-deployment-for-replication-between-on-premises-and-azure"></a>Ανάπτυξη της τοποθεσίας VMware/φυσικά για αναπαραγωγή μεταξύ εσωτερικής εγκατάστασης και του Azure.
Για να τη ρύθμιση DR μεταξύ του υπολογιστή VMware/φυσικά εσωτερικής εγκατάστασης; Ρύθμιση παραμέτρων διακομιστή, υπόδειγμα προορισμού και διαδικασία διακομιστή πρέπει να ρυθμίσει τις παραμέτρους. Ενώ η ενεργοποίηση προστασίας για το διακομιστή προέλευσης Azure Επαναφορά τοποθεσίας θα εγκαταστήσετε φορητότητα υπηρεσία. Μετά τη διακοπή ρεύματος εσωτερικής αφού στο διακομιστή προέλευσης αποτυγχάνει Azure, οι πελάτες πρέπει να εγκατάστασης διαδικασία διακομιστή στο Azure και ο στόχος υπόδειγμα διακομιστή εσωτερικής εγκατάστασης για να προστατεύσετε το διακομιστή προέλευσης Επιστροφή στην αναδημιουργία εσωτερικής εγκατάστασης. 

![Ανάπτυξη της τοποθεσίας VMware/φυσικά για αναπαραγωγή μεταξύ της εσωτερικής & Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises-site"></a>Ανάπτυξη της τοποθεσίας VMM για αναπαραγωγή μεταξύ τοποθεσιών εσωτερικής εγκατάστασης.

Ως μέρος της ρύθμισης των DR μεταξύ των δύο εσωτερικής θέση. Azure υπηρεσίας παροχής αποκατάστασης τοποθεσίας πρέπει να λήψη και εγκατάσταση του διακομιστή VMM. Υπηρεσία παροχής ανάγκες σύνδεσης στο internet για να βεβαιωθείτε ότι όλες οι λειτουργίες που ενεργοποιούνται από την πύλη Azure μεταφράζεται σε εσωτερικής εργασίες όπως η ενεργοποίηση προστασίας, τερματισμού πλευρά πρωτεύοντος εικονικές μηχανές ως μέρος του ανακατευθύνσεις κ.λπ.

![Ανάπτυξη της τοποθεσίας VMM για αναπαραγωγή μεταξύ τοποθεσιών εσωτερικής εγκατάστασης](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises--azure"></a>Ανάπτυξη της τοποθεσίας VMM για αναπαραγωγή μεταξύ της εσωτερικής & Azure.

Ως μέρος της ρύθμισης των DR μεταξύ της εσωτερικής & Azure; Azure υπηρεσίας παροχής αποκατάστασης τοποθεσίας πρέπει να λήψη και εγκατάσταση του διακομιστή VMM μαζί με παράγοντα υπηρεσίες ανάκτησης Azure που πρέπει να έχει εγκατασταθεί σε κάθε κεντρικό υπολογιστή Hyper-V. Ανατρέξτε στην [Τοποθεσία Κατανόηση Azure προστασίας](./site-recovery-understanding-site-to-azure-protection.md) για περισσότερες πληροφορίες.

![Ανάπτυξη της τοποθεσίας VMM για αναπαραγωγή μεταξύ της εσωτερικής & Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises--azure"></a>Ανάπτυξη της τοποθεσίας Hyper-V για αναπαραγωγή μεταξύ της εσωτερικής & Azure

Αυτό είναι το ίδιο με αυτό της ανάπτυξης VMM-μόνο διαφορά την υπηρεσία παροχής & παράγοντας λαμβάνει εγκατεστημένη στον κεντρικό υπολογιστή Hyper-V ίδια. Ανατρέξτε στην [Τοποθεσία Κατανόηση Azure προστασίας](./site-recovery-understanding-site-to-azure-protection.md) για περισσότερες πληροφορίες.

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Παρακολούθηση της ρύθμισης παραμέτρων, προστασίας και αποκατάστασης λειτουργίες

Κάθε λειτουργία σε ASR λαμβάνει ελέγχονται και παρακολουθείται κάτω από την καρτέλα "ΕΡΓΑΣΊΕΣ". Σε περίπτωση οποιαδήποτε ρύθμιση παραμέτρων, προστασία ή Σφάλμα ανάκτησης μεταβείτε στη σελίδα "ΕΡΓΑΣΊΕΣ" και δείτε εάν υπάρχουν τυχόν αποτυχίες.

![Παρακολούθηση της ρύθμισης παραμέτρων, προστασίας και αποκατάστασης λειτουργίες](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Μόλις βρείτε αποτυχίες κάτω από την προβολή ΕΡΓΑΣΙΏΝ, επιλέξτε την ΕΡΓΑΣΊΑ και κάντε κλικ στην επιλογή ΛΕΠΤΟΜΈΡΕΙΕΣ του ΣΦΆΛΜΑΤΟΣ για τη συγκεκριμένη εργασία.

![Παρακολούθηση της ρύθμισης παραμέτρων, προστασίας και αποκατάστασης λειτουργίες](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Τις λεπτομέρειες του σφάλματος θα σας βοηθήσει να αναγνωρίζετε πιθανή αιτία και προτάσεων για το συγκεκριμένο θέμα.

![Παρακολούθηση της ρύθμισης παραμέτρων, προστασίας και αποκατάστασης λειτουργίες](media/site-recovery-monitoring-and-troubleshooting/image5.png)

Στην περίπτωση που παραπάνω φαίνεται να κάποια άλλη λειτουργία που βρίσκεται σε εξέλιξη λόγω που αποτυγχάνει ρύθμιση παραμέτρων προστασία. Βεβαιωθείτε ότι μπορείτε να επιλύσετε το ζήτημα σύμφωνα με τις προτάσεις – εκεί μετά κάντε κλικ στην επιλογή RESART για να ξεκινήσετε ξανά τη λειτουργία.

![Παρακολούθηση της ρύθμισης παραμέτρων, προστασίας και αποκατάστασης λειτουργίες](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Επιλογή ΕΠΑΝΕΚΚΊΝΗΣΗ δεν είναι διαθέσιμη για όλες τις εργασίες – για εκείνα που δεν διαθέτει την επιλογή ΕΠΑΝΕΚΚΊΝΗΣΗ επιστροφής σε του αντικειμένου και επανάληψη ξανά τη λειτουργία. Κάθε ΕΡΓΑΣΊΑ να ακυρωθεί σε οποιοδήποτε σημείο του χρόνου κατά τη χρήση του κουμπιού "Άκυρο" σε εξέλιξη.

![Παρακολούθηση της ρύθμισης παραμέτρων, προστασίας και αποκατάστασης λειτουργίες](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machine"></a>Εποπτεία εύρυθμης λειτουργίας αναπαραγωγής για εικονική μηχανή

Υπηρεσία παροχής ASR κεντρική & απομακρυσμένη παρακολούθηση μέσω της πύλης Azure για κάθε ένα από τα προστατευμένα οντοτήτων. Μεταβείτε στα στοιχεία ΠΡΟΣΤΑΤΕΥΜΈΝΗ εκεί μετά επιλέξτε ΣΎΝΝΕΦΩΝ VMM ή ΟΜΆΔΕΣ ΠΡΟΣΤΑΣΊΑΣ. Καρτέλα ΣΎΝΝΕΦΩΝ VMM είναι μόνο για αναπτύξεις VMM βάσει και όλα τα άλλα σενάρια έχετε τα προστατευμένα οντοτήτων στην καρτέλα ΠΡΟΣΤΑΣΊΑ ΟΜΆΔΕΣ.

![Εποπτεία εύρυθμης λειτουργίας αναπαραγωγής για εικονική μηχανή](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Αφού εκεί, επιλέξτε την προστατευμένη οντότητα κάτω από το αντίστοιχο cloud ή την ομάδα προστασίας. Αφού επιλέξετε την προστατευμένη οντότητα όλα επιτρέπονται λειτουργίες εμφανίζονται στο κάτω τμήμα του παραθύρου.

![Εποπτεία εύρυθμης λειτουργίας αναπαραγωγής για εικονική μηχανή](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Όπως φαίνεται παραπάνω στην υπόθεση η εικονική μηχανή εύρυθμης ΛΕΙΤΟΥΡΓΊΑΣ είναι κρίσιμης – μπορείτε να κάνετε κλικ στο κουμπί ΛΕΠΤΟΜΈΡΕΙΕΣ του ΣΦΆΛΜΑΤΟΣ στην κάτω για να δείτε το σφάλμα. Με βάση τις "πιθανές αιτίες" και "Σύσταση" που αναφέρονται επιλύσει το πρόβλημα.

![Εποπτεία εύρυθμης λειτουργίας αναπαραγωγής για εικονική μηχανή](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Εποπτεία εύρυθμης λειτουργίας αναπαραγωγής για εικονική μηχανή](media/site-recovery-monitoring-and-troubleshooting/image11.png)

Σημείωση: Εάν υπάρχουν οποιαδήποτε ενεργό λειτουργίες που είναι σε εξέλιξη ή αποτυχίας, στη συνέχεια, μεταβείτε στην προβολή ΕΡΓΑΣΊΕΣ όπως αναφέρθηκε προηγουμένως για να προβάλετε το συγκεκριμένο σφάλμα ΕΡΓΑΣΊΑ.

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Αντιμετώπιση προβλημάτων Hyper-V εσωτερικής εγκατάστασης

Σύνδεση με την Κονσόλα διαχείρισης Hyper-V εσωτερικής εγκατάστασης, επιλέξτε την εικονική μηχανή και δείτε την εύρυθμη λειτουργία αναπαραγωγής.

![Αντιμετώπιση προβλημάτων Hyper-V εσωτερικής εγκατάστασης](media/site-recovery-monitoring-and-troubleshooting/image12.png)

Σε αυτήν την περίπτωση *Εύρυθμη λειτουργία αναπαραγωγής* που υποδεικνύεται ως κρίσιμη – *Εύρυθμη λειτουργία αναπαραγωγής προβολής* για να δείτε τις λεπτομέρειες.

![Αντιμετώπιση προβλημάτων Hyper-V εσωτερικής εγκατάστασης](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Για τις περιπτώσεις όπου αναπαραγωγής βρίσκεται σε παύση για την εικονική μηχανή, κάντε δεξί κλικ επιλογή *αναπαραγωγής*->*αναπαραγωγής βιογραφικού σημειώματος*
![αντιμετώπιση προβλημάτων στην εσωτερική εγκατάσταση θέματα Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Στην περίπτωση που η εικονική μηχανή μετεγκαθιστά ενός νέου Hyper-V κεντρικού υπολογιστή (εντός του συμπλέγματος ή μια μεμονωμένη υπολογιστή), που έχει ρυθμιστεί μέσω ASR, μπορούσατε να αντιμετωπίσουν προβλήματα αναπαραγωγής για την εικονική μηχανή. Βεβαιωθείτε ότι το νέο host Hyper-V ικανοποιεί όλα τα ανά-απαιτήσεις και ρυθμίζονται χρησιμοποιώντας τον ASR.

### <a name="event-log"></a>Αρχείο καταγραφής συμβάντων

| Προελεύσεις συμβάντων                | Λεπτομέρειες                                                                                                                                                                                           |
|-------------------------  |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| **Εφαρμογές και αρχεία καταγραφής/Microsoft/VirtualMachineManager/διακομιστή/διαχειριστής υπηρεσίας** (VMM Server)   |  Αυτή η δυνατότητα παρέχει χρήσιμες καταγραφής για την αντιμετώπιση προβλημάτων πολλά διαφορετικά θέματα VMM. |
| **Εφαρμογές και αρχεία καταγραφής/MicrosoftAzureRecoveryServices/αναπαραγωγή της υπηρεσίας** (Κεντρικός υπολογιστής hyper-V)   | Αυτή η δυνατότητα παρέχει χρήσιμες καταγραφής για την αντιμετώπιση προβλημάτων πολλά ζητήματα παράγοντα υπηρεσίες Microsoft Azure αποκατάστασης. <br/> ![Προέλευση συμβάντος για τον κεντρικό υπολογιστή Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Εφαρμογές και υπηρεσία αρχεία καταγραφής/Microsoft Azure τοποθεσίας αποκατάστασης/παροχής/λειτουργικές** (Κεντρικός υπολογιστής hyper-V)   | Αυτή η δυνατότητα παρέχει χρήσιμες καταγραφής για την αντιμετώπιση προβλημάτων πολλά ζητήματα υπηρεσίας ανάκτησης του Microsoft Azure τοποθεσίας. <br/> ![Προέλευση συμβάντος για τον κεντρικό υπολογιστή Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Εφαρμογές και αρχεία καταγραφής/Microsoft/Windows/Hyper-V-VMMS/διαχειριστής υπηρεσίας** (Κεντρικός υπολογιστής hyper-V) | Αυτή η δυνατότητα παρέχει χρήσιμες καταγραφής για την αντιμετώπιση προβλημάτων πολλά ζητήματα διαχείρισης εικονική μηχανή Hyper-V. <br/> ![Προέλευση συμβάντος για τον κεντρικό υπολογιστή Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |


### <a name="hyper-v-replication-logging-options"></a>Επιλογές αναπαραγωγής Hyper-V της καταγραφής

Όλα τα συμβάντα που αφορούν το Hyper-V ρεπλίκα καταγράφονται στο Hyper-V-VMMS\\καταγραφής διαχείρισης που βρίσκεται στην περιοχή **υπηρεσίες αρχεία καταγραφής εφαρμογών και\\Microsoft\\Windows**. Επιπλέον, ένα αναλυτικό αρχείο καταγραφής μπορεί να ενεργοποιηθεί για το Hyper-V-VMMS. Για να ενεργοποιήσετε αυτό το αρχείο καταγραφής, βεβαιωθείτε πρώτα τα αρχεία καταγραφής αναλυτικά και εντοπισμού σφαλμάτων μπορεί να προβληθεί στο πρόγραμμα προβολής συμβάντων. Ανοίξτε το πρόγραμμα προβολής συμβάντων, στη συνέχεια, στο **μενού Προβολή**, κάντε κλικ στην επιλογή **Εμφάνιση αναλυτικών και αρχεία καταγραφής εντοπισμού σφαλμάτων**.

![Αντιμετώπιση προβλημάτων Hyper-V εσωτερικής εγκατάστασης](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Ένα αναλυτικό αρχείο καταγραφής είναι ορατό στην περιοχή Hyper-V-VMMS

![Αντιμετώπιση προβλημάτων Hyper-V εσωτερικής εγκατάστασης](media/site-recovery-monitoring-and-troubleshooting/image15.png)

Στο παράθυρο " **Ενέργειες** ", κάντε κλικ στην **Ενεργοποίηση καταγραφής**. Μόλις ενεργοποιηθεί, εμφανίζεται στην **Εποπτεία επιδόσεων** όπως μιας περιόδου λειτουργίας παρακολούθησης συμβάντων που βρίσκεται στην περιοχή **σύνολα Συλλογής δεδομένων.**

![Αντιμετώπιση προβλημάτων Hyper-V εσωτερικής εγκατάστασης](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Για να προβάλετε τις πληροφορίες που συλλέγονται, πρώτα διακόψετε την περίοδο λειτουργίας ανίχνευσης απενεργοποιώντας το αρχείο καταγραφής, και, στη συνέχεια, αποθηκεύστε το αρχείο καταγραφής και ανοίξετε ξανά στο πρόγραμμα προβολής συμβάντων ή χρήση άλλων εργαλείων για να τον μετατρέψετε σύμφωνα με τις επιθυμίες.



## <a name="reaching-out-for-microsoft-support"></a>Απλώνονται για υποστήριξη της Microsoft

### <a name="log-collection"></a>Συλλογή αρχείων καταγραφής

Για την προστασία VMM τοποθεσίας, ανατρέξτε στην [συλλογή αρχείων καταγραφής ASR χρησιμοποιώντας το εργαλείο πλατφόρμα Διαγνωστικά υποστήριξης (SDP)](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) για τη συλλογή των απαιτούμενων αρχείων καταγραφής.

Για την προστασία Hyper-V τοποθεσίας, κάντε λήψη του [εργαλείου](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) & το εκτελέσει στον κεντρικό υπολογιστή Hyper-V για να συγκεντρώσετε τα αρχεία καταγραφής.

Για σενάρια VMware/φυσικά, ανατρέξτε [Συλλογής τοποθεσιών καταγραφής αποκατάστασης Azure για VMware και φυσικά προστασία τοποθεσίας](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) για τη συλλογή των απαιτούμενων αρχείων καταγραφής.

Εργαλείο συλλέγει τα αρχεία καταγραφής τοπικά στην περιοχή τυχαία ονομασία υποφάκελος στην περιοχή **%LocalAppData%\ElevatedDiagnostics**

![Δείγμα τα βήματα που εμφανίζονται από το Hyper-V τοποθεσίας προστασίας.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="opening-a-support-ticket"></a>Ανοίγει ένα δελτίο υποστήριξης

Για να βελτιώσετε δελτίων υποστήριξης για το ASR, επικοινωνήστε με Azure υποστηρίζουν χρησιμοποιώντας τη διεύθυνση URL στο <http://aka.ms/getazuresupport>

## <a name="kb-articles"></a>Άρθρα Γνωσιακής Βάσης

-   [Πώς μπορείτε να διατηρήσετε το γράμμα προστατευμένο εικονικές μηχανές που απέτυχε επάνω από ή μετεγκατασταθεί στο Azure](http://support.microsoft.com/kb/3031135)
-   [Πώς μπορείτε να διαχειριστείτε εσωτερικής εγκατάστασης με χρήση του εύρους ζώνης δικτύου Azure προστασίας](https://support.microsoft.com/kb/3056159)
-   [ASR: "δεν ήταν δυνατή η εύρεση του πόρου συμπλέγματος" το σφάλμα όταν προσπαθείτε να ενεργοποιήσετε την προστασία για μια εικονική μηχανή](http://support.microsoft.com/kb/3010979)
-   [Κατανόηση και αντιμετώπιση προβλημάτων με τον Οδηγό ρεπλίκα Hyper-V](http://www.microsoft.com/en-in/download/details.aspx?id=29016) 

## <a name="common-asr-errors-and-their-resolutions"></a>Συνήθη σφάλματα ASR και τις λύσεις

Παρακάτω υπάρχουν τα συνηθισμένα σφάλματα που μπορεί να πατήσετε και τις λύσεις. Κάθε μία από το σφάλμα που τεκμηριώνονται σε ξεχωριστή σελίδα WIKI.

### <a name="general"></a>Γενικά
-   <span style="color:green;">ΔΗΜΙΟΥΡΓΊΑ</span> Εργασίες [αποτυγχάνει με το σφάλμα "μια λειτουργία βρίσκεται σε εξέλιξη". Σφάλμα 505, 514, 532](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
-   <span style="color:green;">ΔΗΜΙΟΥΡΓΊΑ</span> Οι εργασίες [αποτυγχάνει με το σφάλμα "Διακομιστής δεν είναι συνδεδεμένος στο Internet". Σφάλμα 25018](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Το πρόγραμμα εγκατάστασης
-   [Δεν είναι δυνατή η καταχώρηση στο διακομιστή VMM λόγω εσωτερικό σφάλμα. Ανατρέξτε στην προβολή εργασίες στην πύλη αποκατάστασης τοποθεσίας για περισσότερες λεπτομέρειες σχετικά με το σφάλμα. Εκτέλεση του προγράμματος Εγκατάστασης ξανά για να καταχωρήσετε το διακομιστή.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
-   [Δεν είναι δυνατή η σύνδεση για τη διαχείριση αποκατάστασης Hyper-V θάλαμο. Επαληθεύστε τις ρυθμίσεις διακομιστή μεσολάβησης ή δοκιμάστε ξανά αργότερα.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Ρύθμιση παραμέτρων
-   [Δεν είναι δυνατή η δημιουργία προστασίας ομάδας: Παρουσιάστηκε σφάλμα κατά την ανάκτηση της λίστας των διακομιστών.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
-   [Το Hyper-V host σύμπλεγμα περιέχει τουλάχιστον έναν προσαρμογέα στατική δικτύου ή χωρίς συνδεδεμένο προσαρμογέων έχουν ρυθμιστεί ώστε να χρησιμοποιούν DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
-   [VMM δεν έχει δικαιώματα για την ολοκλήρωση μιας ενέργειας](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
-   [Δεν μπορείτε να επιλέξετε το λογαριασμό χώρου αποθήκευσης του συνδρομής κατά τη ρύθμιση παραμέτρων προστασίας](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Προστασία
- <span style="color:green;">ΔΗΜΙΟΥΡΓΊΑ</span> Αποτυχία [Ενεργοποίηση προστασίας με σφάλμα "προστασία δεν ήταν δυνατό να ρυθμιστεί για την εικονική μηχανή". Σφάλμα 60007, 40003](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
- <span style="color:green;">ΔΗΜΙΟΥΡΓΊΑ</span> Αποτυχία [Ενεργοποίηση προστασίας με σφάλματος "προστασίας δεν ήταν δυνατό να ενεργοποιηθεί για την εικονική μηχανή." Σφάλμα 70094](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
- <span style="color:green;">ΔΗΜΙΟΥΡΓΊΑ</span> Σφάλμα ζωντανή μετεγκατάστασης [23848 - η εικονική μηχανή πρόκειται να μετακινηθούν με τη χρήση τύπου Live. Αυτό θα μπορούσε να διακόψει την κατάσταση προστασίας αποκατάστασης η εικονική μηχανή.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx) 
- [Ενεργοποίηση προστασίας απέτυχε επειδή το παράγοντας δεν εγκατεστημένος στον κεντρικό υπολογιστή](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
- [Δεν είναι δυνατό να βρεθεί μια κατάλληλη υπηρεσία παροχής φιλοξενίας για την εικονική μηχανή ρεπλίκα - λόγω χαμηλής πρέπει να υπολογίσετε τους πόρους](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
- [Μια κατάλληλη υπηρεσία παροχής φιλοξενίας για την εικονική μηχανή ρεπλίκα δεν μπορεί να βρεθεί - λόγω χωρίς λογική δικτύου συνημμένα](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
- [Δεν μπορεί να συνδεθεί για τον κεντρικό υπολογιστή ρεπλίκα - δεν ήταν δυνατή η δημιουργία σύνδεσης](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)


### <a name="recovery"></a>Ανάκτηση
- VMM δεν είναι δυνατό να ολοκληρωθεί η λειτουργία host-
    -   [Ανακατεύθυνση στο σημείο επιλεγμένο ανάκτησης για εικονική μηχανή: γενικές σφάλμα απαγόρευσης πρόσβασης.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
    -   [Το Hyper-V απέτυχε να ανακατευθύνει στο σημείο επιλεγμένο ανάκτησης για εικονική μηχανή: λειτουργία ματαιώθηκε δοκιμάστε μια πιο πρόσφατη σημείο αποκατάστασης. (0x80004004)](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
    -   Μια σύνδεση με το διακομιστή δεν ήταν δυνατό να υπάρχοντα (0x00002EFD)
        -   [Απέτυχε η Hyper-V για να ενεργοποιήσετε την αντίστροφη αναπαραγωγής για εικονική μηχανή](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
        -   [Απέτυχε η Hyper-V για να ενεργοποιήσετε την αναπαραγωγή για εικονική μηχανή εικονική μηχανή](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
    -   [Δεν ήταν δυνατή η ολοκλήρωση ανακατεύθυνσης για εικονική μηχανή](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
-   [Το πρόγραμμα αποκατάστασης περιέχει εικονικές μηχανές που δεν είναι έτοιμη για προγραμματισμένη ανακατεύθυνσης](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
-   [Η εικονική μηχανή δεν είναι έτοιμη για προγραμματισμένη ανακατεύθυνσης](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   [Εικονική μηχανή δεν εκτελείται και δεν είναι απενεργοποιημένος](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
-   [Εκτός λειτουργίας ζώνης συνέβη σε μια εικονική μηχανή και υποβολή ανακατεύθυνσης απέτυχε](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   Δοκιμή ανακατεύθυνσης
    -   [Δεν ήταν δυνατή η έναρξη ανακατεύθυνσης εφόσον ανακατεύθυνσης δοκιμής βρίσκεται σε εξέλιξη](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
-   <span style="color:green;">ΔΗΜΙΟΥΡΓΊΑ</span>  Ανακατεύθυνση λήγει το χρονικό όριο με 'PreFailoverWorkflow task WaitForScriptExecutionTaskTimeout' λόγω τις ρυθμίσεις παραμέτρων στην ομάδα ασφαλείας δικτύου που σχετίζονται με την εικονική μηχανή ή το υποδίκτυο στην οποία ανήκει σε υπολογιστή. Ανατρέξτε στο ['PreFailoverWorkflow task WaitForScriptExecutionTaskTimeout'](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) για λεπτομέρειες.


### <a name="configuration-server-process-server-master-target"></a>Ρύθμιση παραμέτρων διακομιστή, διαδικασία διακομιστή, υπόδειγμα προορισμού
Ρύθμιση παραμέτρων διακομιστή (CS), διαδικασία διακομιστής (PS), υπόδειγμα Targer (MT)
-   [Ο κεντρικός υπολογιστής ESXi στην οποία φιλοξενείται η PS/στρατηγικής συνεργασίας ανά ΧΏΡΑ ως μια Εικονική θα αποτύχει με μια μοβ οθόνη.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Σύνδεση απομακρυσμένης επιφάνειας εργασίας Αντιμετώπιση προβλημάτων μετά την ανακατεύθυνση
-   Πολλοί πελάτες αντιμετωπίζουν θέματα που πρέπει να συνδεθείτε με το αποτυχίας μέσω Εικονική στα Azure. [Χρησιμοποιήστε το έγγραφο αντιμετώπισης προβλημάτων για να RDP σε την εικονική Μηχανή](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Προσθήκη μιας δημόσιας IP σε εικονικό μηχάνημα διαχείρισης πόρων
Εάν είναι απενεργοποιημένο το κουμπί **σύνδεσης** στην πύλη και δεν είστε συνδεδεμένοι σε Azure μέσω ενός Express δρομολόγηση ή σύνδεση VPN-τοποθεσίας, πρέπει να δημιουργήσετε και να εκχωρήσετε μια δημόσια διεύθυνση IP σας Εικονική να μπορέσετε να χρησιμοποιήσετε RDP/SSH. Ακολουθήστε τα παρακάτω βήματα για να προσθέσετε μια δημόσια IP στο περιβάλλον εργασίας δικτύου του υπολογιστή εικονική.  

![Απέτυχε η προσθήκη μιας δημόσιας IP στη διασύνδεση δικτύου του μέσω εικονική μηχανή](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)