<properties
    pageTitle="Αποκατάσταση για τον SQL Server και υψηλή διαθεσιμότητα | Microsoft Azure"
    description="Μια συζήτηση σχετικά με τους διάφορους τύπους στρατηγικές HADR για SQL Server που εκτελείται σε εικονικές μηχανές Windows Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Υψηλή διαθεσιμότητα και την καταστροφή ανάκτησης για τον SQL Server σε εικονικές μηχανές Windows Azure

## <a name="overview"></a>Επισκόπηση

Microsoft εικονικές μηχανές Windows Azure (ΣΠΣ) με τον SQL Server μπορεί να σας βοηθήσει να μειώσουν το κόστος μιας υψηλή διαθεσιμότητα και την καταστροφή λύσης αποκατάστασης (HADR) βάση δεδομένων. Οι περισσότερες λύσεις SQL Server HADR υποστηρίζονται στο Azure εικονικές μηχανές, ως μόνο για Azure και ως υβριδική λύσεις. Σε μια λύση μόνο Azure, ολόκληρο το σύστημα HADR εκτελείται στο Azure. Σε μια ρύθμιση παραμέτρων υβριδικού, τμήμα της λύσης εκτελείται στο Azure και το άλλο τμήμα εκτελείται εσωτερικής εγκατάστασης στην εταιρεία σας. Την ευελιξία του Azure περιβάλλοντος σάς επιτρέπει να μετακινηθείτε μερικώς ή πλήρως στην Azure για να πληρούν τις απαιτήσεις HADR σας συστήματα βάσεων δεδομένων SQL Server και προϋπολογισμού.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="understanding-the-need-for-an-hadr-solution"></a>Κατανόηση των την ανάγκη για μια λύση HADR

Είναι ώστε να μπορείτε να διασφαλίσετε ότι το σύστημα βάσης δεδομένων διαθέτει τις δυνατότητες HADR που απαιτεί η συμφωνία σε επίπεδο εξυπηρέτησης (SLA). Το γεγονός ότι Azure παρέχει τους μηχανισμούς υψηλής διαθεσιμότητας, όπως η υπηρεσία διόρθωσης για τις υπηρεσίες cloud και εντοπισμός αποκατάστασης αποτυχίας για τις εικονικές μηχανές, ίδια δεν εγγυάται θα μπορούν να επιτύχετε την επιθυμητή SLA. Αυτοί οι μηχανισμοί προστασία η υψηλή διαθεσιμότητα του ΣΠΣ, αλλά όχι τη υψηλή διαθεσιμότητα του SQL Server που εκτελείται στο εσωτερικό του ΣΠΣ. Είναι δυνατή για την παρουσία του SQL Server αποτυχία ενώ η Εικονική είναι συνδεδεμένος και σε καλή κατάσταση. Επιπλέον, ακόμα και την υψηλή διαθεσιμότητα μηχανισμοί που παρέχεται από Azure επιτρέπουν χρόνου εκτός λειτουργίας της του ΣΠΣ λόγω συμβάντα όπως αποκατάστασης από λογισμικό ή αποτυχίες υλικού και αναβαθμίσεων του λειτουργικού συστήματος.

Επιπλέον, παν πλεονάζοντα χώρο αποθήκευσης Εξοπλισμό στο Azure, η οποία έχει υλοποιηθεί με μια δυνατότητα που ονομάζεται παν αναπαραγωγής, ενδέχεται να μην είναι μια λύση αποκατάστασης από καταστροφή επαρκής για τις βάσεις δεδομένων. Επειδή παν αναπαραγωγής στέλνει δεδομένα ασύγχρονα, ενδέχεται να χαθούν σε περίπτωση καταστροφής πρόσφατες ενημερώσεις. Περισσότερες πληροφορίες σχετικά με τους περιορισμούς παν αναπαραγωγής καλύπτονται στην ενότητα [παν αναπαραγωγή δεν υποστηρίζεται για αρχεία δεδομένων και καταγραφής σε ξεχωριστές δίσκων](#geo-replication-support) .

## <a name="hadr-deployment-architectures"></a>HADR αρχιτεκτονικές ανάπτυξης

Τεχνολογίες SQL Server HADR που υποστηρίζονται στο Azure περιλαμβάνουν τα εξής:

- [Πάντα σε ομάδες διαθεσιμότητας](https://technet.microsoft.com/library/hh510230.aspx)
- [Δημιουργία ειδώλου βάσης δεδομένων](https://technet.microsoft.com/library/ms189852.aspx)
- [Αποστολή αρχείων καταγραφής](https://technet.microsoft.com/library/ms187103.aspx)
- [Δημιουργία αντιγράφων ασφαλείας και επαναφορά με υπηρεσία αποθήκευσης αντικειμένων Blob του Azure](https://msdn.microsoft.com/library/jj919148.aspx)
- [Πάντα σε παρουσίες συμπλέγματος ανακατεύθυνσης](https://technet.microsoft.com/library/ms189134.aspx)

Είναι δυνατό να συνδυάσετε τις τεχνολογίες για να υλοποιήσετε μια λύση SQL Server που έχει υψηλή διαθεσιμότητα και δυνατότητες αποκατάστασης από καταστροφή. Ανάλογα με την τεχνολογία που χρησιμοποιείτε, μια υβριδική ανάπτυξη μπορεί να απαιτεί μια διοχέτευσης VPN με το Azure εικονικού δικτύου. Οι παρακάτω ενότητες σας δείξουν ορισμένα από τα αρχιτεκτονικές ανάπτυξης παράδειγμα.

## <a name="azure-only-high-availability-solutions"></a>Μόνο Azure: λύσεις υψηλής διαθεσιμότητας

Μπορείτε να έχετε μια λύση υψηλής διαθεσιμότητας για τις βάσεις δεδομένων SQL Server στο Azure χρησιμοποιώντας πάντα σε ομάδες διαθεσιμότητα ή να αποτελούν πιστή αναπαράσταση της βάσης δεδομένων.

| Τεχνολογία                               | Παράδειγμα αρχιτεκτονικές                    |
| ---------------------------------------- | ---------------------------------------- |
| **Πάντα σε ομάδες διαθεσιμότητας**        | Όλα τα αντίγραφα διαθεσιμότητα εκτελείται στο ΣΠΣ Azure για υψηλή διαθεσιμότητα μέσα στην ίδια περιοχή. Πρέπει να ρυθμίσετε τις παραμέτρους ενός ελεγκτή τομέα Εικονική, επειδή έναν τομέα της υπηρεσίας καταλόγου Active Directory απαιτεί Windows Server ανακατεύθυνσης συμπλέγματος (WSFC).<br/> ![Πάντα σε ομάδες διαθεσιμότητας](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων πάντα σε ομάδες διαθεσιμότητας στο Azure (Γραφικών)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Πάντα σε παρουσίες συμπλέγματος ανακατεύθυνσης** | Παρουσίες συμπλέγματος ανακατεύθυνσης (FCI), οι που απαιτούν κοινόχρηστο χώρο αποθήκευσης, μπορούν να δημιουργηθούν με 2 διαφορετικούς τρόπους.<br/><br/>1. FCI σε μια WSFC δύο κόμβου εκτελείται στο ΣΠΣ Azure με το χώρο αποθήκευσης που υποστηρίζονται από μια λύση συμπλέγματος τρίτων κατασκευαστών. Για ένα συγκεκριμένο παράδειγμα που χρησιμοποιεί το γράμμα SIOS DataKeeper, ανατρέξτε στο θέμα [υψηλής διαθεσιμότητας για έναν κοινόχρηστο πόρο αρχείων με χρήση του WSFC και λογισμικό τρίτου κατασκευαστή γράμμα SIOS Datakeeper](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>2. ένα FCI σε μια WSFC δύο κόμβου λειτουργεί σε ΣΠΣ Azure με απομακρυσμένη iSCSI προορισμού κοινόχρηστο χώρο αποθήκευσης μπλοκ μέσω ExpressRoute. Για παράδειγμα, NetApp ιδιωτικού χώρου αποθήκευσης (NPS) εκθέτει έναν προορισμό iSCSI μέσω ExpressRoute με Equinix σε ΣΠΣ Azure.<br/><br/>Για κοινόχρηστο χώρο αποθήκευσης τρίτων κατασκευαστών και λύσεις αναπαραγωγή δεδομένων, πρέπει να επικοινωνήσετε με τον προμηθευτή για ζητήματα που σχετίζονται με την πρόσβαση σε δεδομένα σε ανακατεύθυνσης.<br/><br/>Σημειώστε ότι χρησιμοποιώντας FCI επάνω από το [χώρο αποθήκευσης αρχείων Azure](https://azure.microsoft.com/services/storage/files/) δεν υποστηρίζεται ακόμα, επειδή αυτή η λύση δεν χρησιμοποιεί χώρο αποθήκευσης Premium. Εργαζόμαστε για την υποστήριξη αυτό σύντομα. |

## <a name="azure-only-disaster-recovery-solutions"></a>Μόνο Azure: λύσεων αποκατάστασης από καταστροφή

Μπορείτε να έχετε μια λύση αποκατάστασης από καταστροφή για τις βάσεις δεδομένων SQL Server στο Azure χρησιμοποιώντας πάντα σε ομάδες διαθεσιμότητας, κατοπτρισμού της βάσης δεδομένων ή δημιουργία αντιγράφων ασφαλείας και επαναφορά με το χώρο αποθήκευσης αντικειμένων blob.

| Τεχνολογία                               | Παράδειγμα αρχιτεκτονικές                    |
| ---------------------------------------- | ---------------------------------------- |
| **Πάντα σε ομάδες διαθεσιμότητας**        | Διαθεσιμότητα αντίγραφα εκτελείται σε πολλά κέντρα δεδομένων στο ΣΠΣ Azure για αποκατάσταση. Αυτήν τη λύση σταυρό περιοχή προστατεύει από μη διαθεσιμότητα πλήρης τοποθεσία. <br/> ![Πάντα σε ομάδες διαθεσιμότητας](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Μέσα σε μια περιοχή, όλα τα αντίγραφα πρέπει να είναι μέσα σε την ίδια υπηρεσία cloud και το ίδιο VNet. Επειδή κάθε περιοχή θα έχει μια ξεχωριστή VNet, αυτές οι λύσεις απαιτούν VNet να VNet συνδεσιμότητας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μιας τοποθεσίας σε τοποθεσία VPN στην πύλη του Azure κλασική](../vpn-gateway/vpn-gateway-site-to-site-create.md). |
| **Δημιουργία ειδώλου βάσης δεδομένων**                   | Κεφάλαιο και αντικριστά και τους διακομιστές να εκτελούν σε διαφορετικά κέντρα δεδομένων για αποκατάσταση. Πρέπει να αναπτύξετε χρησιμοποιώντας πιστοποιητικά διακομιστή επειδή έναν τομέα της υπηρεσίας καταλόγου active directory δεν είναι δυνατό να εκτείνεται σε πολλές κέντρα δεδομένων.<br/>![Δημιουργία ειδώλου βάσης δεδομένων](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Δημιουργία αντιγράφων ασφαλείας και επαναφορά με υπηρεσία αποθήκευσης αντικειμένων Blob του Azure** | Βάσεις δεδομένων παραγωγής αντίγραφα ασφαλείας απευθείας στο χώρο αποθήκευσης αντικειμένων blob σε μια διαφορετική κέντρα δεδομένων για αποκατάσταση.<br/>![Δημιουργία αντιγράφων ασφαλείας και επαναφοράς](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας και επαναφοράς για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Υβριδική IT: Λύσεων αποκατάστασης από καταστροφή

Μπορείτε να έχετε μια λύση αποκατάστασης από καταστροφή για τις βάσεις δεδομένων SQL Server σε ένα περιβάλλον υβριδική-IT χρησιμοποιώντας πάντα σε ομάδες διαθεσιμότητας, δημιουργία ειδώλου βάσης δεδομένων, Αποστολή αρχείου καταγραφής και δημιουργία αντιγράφων ασφαλείας και επαναφορά με αποθήκευσης Azure ιστολογίου.

| Τεχνολογία                               | Παράδειγμα αρχιτεκτονικές                    |
| ---------------------------------------- | ---------------------------------------- |
| **Πάντα σε ομάδες διαθεσιμότητας**        | Ορισμένες αντίγραφα διαθεσιμότητα εκτελείται στο ΣΠΣ Azure και άλλα αντίγραφα εκτελείται εσωτερικής εγκατάστασης για διατοποθεσιακή αποκατάσταση. Στην τοποθεσία παραγωγής μπορεί να είναι είτε στην εσωτερική εγκατάσταση ή σε ένα κέντρο δεδομένων του Azure.<br/>![Πάντα σε ομάδες διαθεσιμότητας](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Επειδή όλα τα αντίγραφα διαθεσιμότητα πρέπει να είναι στο ίδιο σύμπλεγμα WSFC, το σύμπλεγμα WSFC πρέπει να εκτείνεται σε δύο δίκτυα (ένα σύμπλεγμα WSFC πολλών υποδικτύου). Αυτή η ρύθμιση παραμέτρων απαιτεί σύνδεση VPN μεταξύ Azure και το δίκτυο εσωτερικής εγκατάστασης.<br/><br/>Για επιτυχημένη αποκατάσταση από τις βάσεις δεδομένων, μπορείτε, επίσης, θα πρέπει να εγκαταστήσετε μια ρεπλίκα ελεγκτή τομέα στην τοποθεσία αποκατάστασης από καταστροφή.<br/><br/>Είναι δυνατό να χρησιμοποιήσετε τον Οδηγό ρεπλίκα Προσθήκη στο SSMS για να προσθέσετε μια Azure αντιγράφου μιας υπάρχουσας πάντα στη διαθεσιμότητα ομάδας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα πρόγραμμα εκμάθησης: επέκταση πάντα στη διαθεσιμότητα την ομάδα σας να Azure. |
| **Δημιουργία ειδώλου βάσης δεδομένων**                   | Ένα συνεργάτη που εκτελούνται σε μια Εικονική Azure και την εκτέλεση άλλων εσωτερικής εγκατάστασης για διατοποθεσιακή αποκατάσταση χρησιμοποιώντας πιστοποιητικά διακομιστή. Συνεργάτες, δεν χρειάζεται να είναι στον ίδιο τομέα υπηρεσίας καταλόγου Active Directory και χωρίς σύνδεση VPN απαιτείται.<br/>![Δημιουργία ειδώλου βάσης δεδομένων](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Μια άλλη βάση δεδομένων που αποτελούν πιστή αναπαράσταση σενάριο περιλαμβάνει ένα συνεργάτη που εκτελούνται σε μια Εικονική Azure και τα άλλα εκτελείται εσωτερικής εγκατάστασης στον ίδιο τομέα υπηρεσίας καταλόγου Active Directory για διατοποθεσιακή αποκατάσταση. Απαιτείται μια [σύνδεση VPN μεταξύ του Azure εικονικού δικτύου και το δίκτυο εσωτερικής εγκατάστασης](../vpn-gateway/vpn-gateway-site-to-site-create.md) .<br/><br/>Για επιτυχημένη αποκατάσταση από τις βάσεις δεδομένων, μπορείτε, επίσης, θα πρέπει να εγκαταστήσετε μια ρεπλίκα ελεγκτή τομέα στην τοποθεσία αποκατάστασης από καταστροφή. |
| **Αποστολή αρχείων καταγραφής**                         | Ένα διακομιστή εκτελείται σε μια Εικονική Azure και την εκτέλεση άλλων εσωτερικής εγκατάστασης για διατοποθεσιακή αποκατάσταση. Αποστολή αρχείων καταγραφής εξαρτάται από κοινή χρήση αρχείων των Windows, ώστε να απαιτείται σύνδεση VPN μεταξύ του Azure εικονικού δικτύου και το δίκτυο εσωτερικής εγκατάστασης.<br/>![Αποστολή αρχείων καταγραφής](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Για επιτυχημένη αποκατάσταση από τις βάσεις δεδομένων, μπορείτε, επίσης, θα πρέπει να εγκαταστήσετε μια ρεπλίκα ελεγκτή τομέα στην τοποθεσία αποκατάστασης από καταστροφή. |
| **Δημιουργία αντιγράφων ασφαλείας και επαναφορά με υπηρεσία αποθήκευσης αντικειμένων Blob του Azure** | Εσωτερικής βάσεις δεδομένων παραγωγής αντίγραφα ασφαλείας απευθείας στο χώρο αποθήκευσης αντικειμένων blob του Azure για αποκατάσταση.<br/>![Δημιουργία αντιγράφων ασφαλείας και επαναφοράς](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας και επαναφοράς για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Σημαντικά θέματα για SQL Server HADR στο Azure

Azure ΣΠΣ, χώρος αποθήκευσης και δίκτυα έχουν διαφορετικά χαρακτηριστικά λειτουργικές από μια εσωτερική, χωρίς εικονική διαμόρφωση την υποδομή των τμημάτων. Μια επιτυχή υλοποίηση μιας λύσης HADR SQL Server στο Azure απαιτεί που κατανοήσετε αυτές τις διαφορές και να σχεδιάσετε τη λύση σας ώστε να περιλαμβάνει τους.

### <a name="high-availability-nodes-in-an-availability-set"></a>Υψηλή διαθεσιμότητα κόμβους σε ένα σύνολο διαθεσιμότητα

Διαθεσιμότητα σύνολα στο Azure σάς επιτρέπουν να τοποθετήσετε τους κόμβους υψηλή διαθεσιμότητα σε ξεχωριστή σφαλμάτων τομείς (FDs) και ενημέρωση τομείς (UDs). Για το Azure ΣΠΣ να τοποθετηθεί στο ίδιο σύνολο διαθεσιμότητα, πρέπει να αναπτύξετε τους στην ίδια υπηρεσία cloud. Μόνο οι κόμβοι την ίδια υπηρεσία cloud μπορούν να συμμετάσχουν στο ίδιο σύνολο διαθεσιμότητα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση τη διαθεσιμότητα των εικονικές μηχανές](virtual-machines-windows-manage-availability.md).

### <a name="wsfc-cluster-behavior-in-azure-networking"></a>Συμπεριφορά σύμπλεγμα WSFC στο δίκτυο Azure

Η υπηρεσία DHCP μη συμβατό RFC στο Azure μπορεί να προκαλέσει τη δημιουργία ορισμένες ρυθμίσεις παραμέτρων συμπλέγματος WSFC αποτυχία, λόγω το όνομα του δικτύου συμπλέγματος ανατίθεται μια διπλότυπη διεύθυνση IP, όπως την ίδια διεύθυνση IP με έναν από τους κόμβους συμπλέγματος. Αυτό είναι ένα ζήτημα όταν εφαρμόζετε πάντα στη διαθεσιμότητα ομάδες, οι οποίες εξαρτάται από τη δυνατότητα WSFC.

Εξετάστε το σενάριο, όταν ένα σύμπλεγμα δύο κόμβου έχει δημιουργηθεί και να τεθούν σε σύνδεση:

1. Το σύμπλεγμα παρέχεται online και, στη συνέχεια, NODE1 τις αιτήσεις μια δυναμική αντιστοίχιση διεύθυνση IP για το όνομα του δικτύου συμπλέγματος.

2. Δεν υπάρχει διεύθυνση IP εκτός από τη διεύθυνση IP του NODE1 δίνεται από την υπηρεσία DHCP, επειδή η υπηρεσία DHCP αναγνωρίζει ότι η αίτηση προέρχεται από NODE1 ίδια.

3. Windows εντοπίζουν που έχει αντιστοιχιστεί μια διπλότυπη διεύθυνση τόσο για NODE1 και για το όνομα του δικτύου συμπλέγματος και, στην προεπιλεγμένη ομάδα συμπλέγματος δεν μπορεί να συνδεθεί.

4. Προεπιλεγμένη ομάδα συμπλέγματος μετακινεί NODE2, το οποίο χειρίζεται διεύθυνση IP του NODE1 ως διεύθυνση IP του συμπλέγματος και συνδυάζει την προεπιλεγμένη ομάδα συμπλέγματος online.

5. Όταν NODE2 επιχειρεί να δημιουργήσετε τη σύνδεση με NODE1, πακέτα που απευθύνονται σε NODE1 αφήστε ποτέ NODE2, επειδή το επιλύει διεύθυνση IP του NODE1 στον εαυτό. NODE2 δεν είναι δυνατό να δημιουργήσετε τη σύνδεση με NODE1, στη συνέχεια, χάνονται απαρτίας και τερματίζει το σύμπλεγμα.

6. Στο μεταξύ, NODE1 να στείλουν πακέτα NODE2, αλλά δεν είναι δυνατό να απαντήσετε NODE2. NODE1 χάνει απαρτίας και τερματίζει το σύμπλεγμα.

Αυτό το σενάριο μπορεί να αποφεύγονται αναθέτοντας που δεν χρησιμοποιείται στατική διεύθυνση IP, όπως μια διεύθυνση IP τοπικής σύνδεσης όπως 169.254.1.1, το όνομα του δικτύου συμπλέγματος για να εμφανιστεί το όνομα δικτύου του συμπλέγματος online. Για να απλοποιήσετε αυτήν τη διαδικασία, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων συμπλέγματος ανακατεύθυνσης των Windows στο Azure για πάντα σε ομάδες διαθεσιμότητας](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων πάντα σε ομάδες διαθεσιμότητας στο Azure (Γραφικών)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Διαθεσιμότητα ομάδας ακρόασης υποστήριξης

Διαθεσιμότητα ομάδας ακροατών υποστηρίζονται σε ΣΠΣ Azure που εκτελεί Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 και Windows Server 2016. Αυτή η υποστήριξη πραγματοποιείται πιθανές με τη χρήση των εξισορρόπηση φόρτου τα τελικά σημεία ενεργοποιημένη σε του ΣΠΣ Azure που είναι κόμβοι ομάδας διαθεσιμότητας. Πρέπει να ακολουθήσετε τα βήματα ειδική ρύθμιση παραμέτρων για το ακροατών ώστε να εργάζεται για δύο εφαρμογές προγράμματος-πελάτη που εκτελούνται στο Azure, καθώς και εκείνους που εκτελούν εσωτερικής εγκατάστασης.

Υπάρχουν δύο κύριες επιλογές για τη ρύθμιση του ακρόασης: εξωτερικές (δημόσιας) ή εσωτερικό. Η εξωτερική ακρόαση (δημόσιας) χρησιμοποιεί internet αντικριστές εξισορρόπηση φόρτου και είναι συσχετισμένη με μια δημόσια εικονικό IP (VIP) που είναι προσβάσιμα μέσω του internet. Μια εσωτερική ακρόασης χρησιμοποιεί μια εσωτερική εξισορρόπηση φόρτου και υποστηρίζουν μόνο τα προγράμματα-πελάτες μέσα στο ίδιο δίκτυο εικονικού. Είτε η φόρτωση του τύπου εξισορρόπησης, πρέπει να ενεργοποιήσετε το άμεσο επιστροφής διακομιστή. 

Εάν η ομάδα διαθεσιμότητα εκτείνεται σε πολλά δευτερεύοντα δίκτυα Azure (όπως μια ανάπτυξη που τέμνει Azure περιοχές), πρέπει να περιλαμβάνει τη συμβολοσειρά σύνδεσης του προγράμματος-πελάτη "**MultisubnetFailover = True**". Το αποτέλεσμα προσπάθειες παράλληλες σύνδεσης για να τα αντίγραφα στο τα διαφορετικά δευτερεύοντα δίκτυα. Για οδηγίες σχετικά με τη ρύθμιση του ένα ακροατήριο, ανατρέξτε στο θέμα

- [Ρύθμιση παραμέτρων ενός ακρόασης ILB για πάντα στη διαθεσιμότητα ομάδες στο Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
- [Ρύθμιση παραμέτρων ενός εξωτερικού ακρόασης για πάντα στη διαθεσιμότητα ομάδες στο Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

Μπορείτε να συνδέσετε σε κάθε ρεπλίκα διαθεσιμότητα ξεχωριστά από τη σύνδεση απευθείας με την παρουσία της υπηρεσίας. Επίσης, εφόσον πάντα σε ομάδες διαθεσιμότητα είναι συμβατές με βάση δεδομένων που αποτελούν πιστή αναπαράσταση των υπολογιστών-πελατών, μπορείτε να συνδεθείτε με τη διαθεσιμότητα αντίγραφα όπως κατοπτρισμού συνεργάτες με την προϋπόθεση ότι τα αντίγραφα έχουν ρυθμιστεί οι παράμετροι παρόμοια με τη δημιουργία ειδώλου βάσης δεδομένων της βάσης δεδομένων:

- Μία ρεπλίκα κύρια και μια δευτερεύουσα ρεπλίκα

- Η δευτερεύουσα ρεπλίκα έχει ρυθμιστεί ως μη αναγνώσιμο (**Αναγνώσιμα δευτερεύοντα** επιλογή οριστεί σε **όχι**)

Ένα παράδειγμα συμβολοσειράς σύνδεσης προγράμματος-πελάτη που αντιστοιχεί σε αυτήν τη ρύθμιση αποτελούν πιστή αναπαράσταση μοιάζει με βάση δεδομένων χρησιμοποιώντας το ADO.NET ή SQL Server Native Client είναι κάτω από το στοιχείο:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Για περισσότερες πληροφορίες σχετικά με συνδεσιμότητα υπολογιστή-πελάτη, ανατρέξτε στο θέμα:

- [Χρήση λέξεων-κλειδιών συμβολοσειράς σύνδεσης με το SQL Server Native Client](https://msdn.microsoft.com/library/ms130822.aspx)
- [Σύνδεση των υπολογιστών-πελατών με μια βάση δεδομένων που αποτελούν πιστή αναπαράσταση περιόδου λειτουργίας (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
- [Σύνδεση με ακρόασης διαθεσιμότητα ομάδας σε υβριδικό IT](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
- [Ακροατών ομάδας διαθεσιμότητας, συνδεσιμότητα υπολογιστή-πελάτη και εφαρμογή ανακατεύθυνσης (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
- [Χρήση συμβολοσειρές σύνδεσης δημιουργία ειδώλου βάσης δεδομένων με ομάδες διαθεσιμότητας](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Λανθάνων χρόνος δικτύου σε υβριδική IT

Θα πρέπει να αναπτύξετε τη λύση σας HADR με την προϋπόθεση ότι ενδέχεται να υπάρχουν χρονικά διαστήματα με υψηλή δικτύου λανθάνοντος χρόνου μεταξύ σας δίκτυο εσωτερικής εγκατάστασης και του Azure. Κατά την ανάπτυξη αντίγραφα σε Azure, θα πρέπει να χρησιμοποιείτε ασύγχρονης υποβολή αντί για Σύγχρονη ολοκλήρωση κατάσταση συγχρονισμού. Κατά την ανάπτυξη της βάσης δεδομένων αποτελούν πιστή αναπαράσταση των διακομιστών τόσο εσωτερικής εγκατάστασης και στο Azure, χρησιμοποιήστε τη λειτουργία υψηλές επιδόσεις αντί για τη λειτουργία υψηλή ασφάλεια.

### <a name="geo-replication-support"></a>Υποστήριξη παν αναπαραγωγής

Παν-αναπαραγωγή σε Azure δίσκων δεν υποστηρίζει το αρχείο δεδομένων και το αρχείο καταγραφής από την ίδια βάση δεδομένων να είναι αποθηκευμένο σε ξεχωριστή δίσκων. Εξοπλισμό αναπαράγει τις αλλαγές σε κάθε δίσκο ανεξάρτητα και ασύγχρονη. Αυτός ο μηχανισμός εγγυάται τη σειρά εγγραφής μέσα σε ένα μεμονωμένο δίσκο στο αντίγραφο αναπαραχθούν παν, αλλά όχι σε αναπαραχθούν παν αντίγραφα των πολλών δίσκων. Εάν ρυθμίζετε τις παραμέτρους μιας βάσης δεδομένων για την αποθήκευση του αρχείου δεδομένων και το αρχείο καταγραφής σε ξεχωριστή δίσκων, τα ανακτημένα δίσκων μετά από μια καταστροφή μπορεί να περιέχει ένα πιο ενημερωμένο αντίγραφο του αρχείου δεδομένων από το αρχείο καταγραφής, η οποία διακόπτει την εγγραφή-ahead στο αρχείο καταγραφής SQL Server και τις ιδιότητες ACID συναλλαγών. Εάν δεν έχετε την επιλογή για να απενεργοποιήσετε την παν-αναπαραγωγή στο λογαριασμό του χώρου αποθήκευσης, θα πρέπει να διατηρήσετε όλα αρχεία δεδομένων και καταγραφής για μια δεδομένη βάση δεδομένων στον ίδιο δίσκο. Εάν χρησιμοποιείτε περισσότερες από μία δίσκου λόγω του μεγέθους της βάσης δεδομένων, πρέπει να αναπτύξετε μία από τις λύσεις αποκατάστασης από καταστροφή που αναφέρονται παραπάνω για να βεβαιωθείτε ότι πλεονάζοντα δεδομένα.

## <a name="next-steps"></a>Επόμενα βήματα

Εάν χρειάζεστε για να δημιουργήσετε μια εικονική μηχανή Azure με τον SQL Server, ανατρέξτε στο θέμα [προμήθεια του SQL Server εικονικό μηχάνημα σε Azure](virtual-machines-windows-portal-sql-server-provision.md).

Για να λάβετε τις καλύτερες επιδόσεις από SQL Server που εκτελείται σε μια Εικονική Azure, δείτε τις οδηγίες στην [Απόδοση βέλτιστες πρακτικές για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-performance.md).

Για άλλα θέματα που σχετίζονται με εκτελεί τον SQL Server στο ΣΠΣ Azure, ανατρέξτε στο θέμα [SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Άλλοι πόροι

- [Εγκαταστήστε ένα νέο δάσος υπηρεσίας καταλόγου Active Directory στο Azure](../active-directory/active-directory-new-forest-virtual-machine.md)
- [Δημιουργία συμπλέγματος WSFC για πάντα σε ομάδες διαθεσιμότητα σε Εικονική Azure](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)
