<properties
    pageTitle="Τι πρέπει να κάνετε σε περίπτωση μια Azure υπηρεσίας διαταραχή που επηρεάζει τις υπηρεσίες Cloud Azure | Microsoft Azure"
    description="Μάθετε τι μπορείτε να κάνετε σε περίπτωση μη Διακοπή υπηρεσίας Azure που επηρεάζει τις υπηρεσίες Cloud Azure."
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Τι πρέπει να κάνετε σε περίπτωση μια Azure υπηρεσίας διαταραχή που επηρεάζει τις υπηρεσίες Cloud Azure

Στη Microsoft, εργαζόμαστε οριστική για να βεβαιωθείτε ότι οι υπηρεσίες μας είναι πάντα διαθέσιμη σε εσάς όταν τις χρειάζεστε. Δυνάμεις πέρα από το στοιχείο ελέγχου μερικές φορές επηρεάσει μας με τρόπους που προκαλούν διακοπές μη προγραμματισμένη υπηρεσίας.

Η Microsoft παρέχει μια υπηρεσία σύμβαση (SLA) για τις υπηρεσίες ως δέσμευση για συνεχούς και συνδεσιμότητας. Μπορείτε να βρείτε το SLA για μεμονωμένες Azure υπηρεσίες στο [Τις συμβάσεις επιπέδου υπηρεσιών Azure](https://azure.microsoft.com/support/legal/sla/).

Azure έχει ήδη πολλές δυνατότητες ενσωματωμένη πλατφόρμα που υποστηρίζουν ιδιαίτερα διαθέσιμες εφαρμογές. Για περισσότερες πληροφορίες σχετικά με αυτές τις υπηρεσίες, διαβάστε [Αποκατάσταση και υψηλή διαθεσιμότητα για τις εφαρμογές του Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Σε αυτό το άρθρο καλύπτει ένα σενάριο αποκατάστασης true από καταστροφή, όταν μια ολόκληρη περιοχή έχει ένα μη διαθεσιμότητα λόγω κύρια φυσική καταστροφή ή διακοπή ευρεία υπηρεσίας. Αυτές είναι σπάνιων εμφανίσεις, αλλά πρέπει να προετοιμάσετε για την πιθανότητα ότι υπάρχει ένα μη διαθεσιμότητα από μια ολόκληρη την περιοχή. Εάν μια ολόκληρη την περιοχή αντιμετωπίσει διαταραχή υπηρεσίας, τα πλεονάζοντα τοπικά αντίγραφα των δεδομένων σας θα προσωρινά να μην είναι διαθέσιμες. Εάν έχετε ενεργοποιήσει το παν αναπαραγωγής, τρεις πρόσθετα αντίγραφα των Azure χώρο αποθήκευσης αντικειμένων blob και πίνακες αποθηκεύονται σε διαφορετική περιοχή. Σε περίπτωση μια ολοκληρωμένη τοπικές μη διαθεσιμότητα ή μια καταστροφή στην οποία η κύρια περιοχή δεν είναι ανακτήσιμα, Azure αντιστοιχίζει ξανά τις όλες τις εγγραφές DNS στην περιοχή αναπαραχθούν παν.

>[AZURE.NOTE]Πρέπει να γνωρίζετε ότι δεν έχετε κανένα στοιχείο ελέγχου πάνω από αυτήν τη διαδικασία και θα συμβεί μόνο για διακοπές υπηρεσίας όλο το κέντρο δεδομένων. Αυτόν το λόγο, πρέπει να βασίζονται σε άλλες στρατηγικές δημιουργίας αντιγράφων ασφαλείας συγκεκριμένη εφαρμογή για να επιτύχετε το υψηλότερο επίπεδο διαθεσιμότητας. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα σχετικά με τα [δεδομένα στρατηγικές για την αποκατάσταση](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR). Εάν θέλετε να μπορούν να επηρεάσουν τη δική σας ανακατεύθυνσης, που μπορεί να θέλετε να λάβετε υπόψη τη χρήση των [πλεοναζόντων παν αποθήκευσης πρόσβαση για ανάγνωση (RA-Εξοπλισμό)](../storage/storage-redundancy.md#read-access-geo-redundant-storage), που δημιουργεί ένα αντίγραφο μόνο για ανάγνωση των δεδομένων σας σε μια άλλη περιοχή.

Για να σας βοηθήσει να χειριστείτε αυτών των εμφανίσεων σπάνιων, παρέχουμε τις παρακάτω οδηγίες για Azure εικονικές μηχανές (ΣΠΣ) στην περίπτωση διαταραχή υπηρεσίας από ολόκληρη την περιοχή όπου έχει αναπτυχθεί την εφαρμογή του Azure Εικονική.

##<a name="option-1-wait-for-recovery"></a>Επιλογή 1: Περιμένετε ανάκτησης
Σε αυτήν την περίπτωση, απαιτείται καμία ενέργεια εκ μέρους σας. Γνωρίζετε ότι ευλαβικά ενημερωμένη λειτουργούν Azure τις ομάδες για να επαναφέρετε διαθεσιμότητα υπηρεσίας. Μπορείτε να δείτε την τρέχουσα κατάσταση υπηρεσίας στην μας [Πίνακας εύρυθμης λειτουργίας υπηρεσιών Azure](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Αυτή είναι η καλύτερη επιλογή, εάν ένας πελάτης δεν έχει ρυθμίσει Επαναφορά τοποθεσίας Azure ή έχει μια δευτερεύουσα ανάπτυξη σε διαφορετική περιοχή.

Για τους πελάτες που θέλετε άμεση πρόσβαση στις υπηρεσίες τους ανεπτυγμένος cloud, τις παρακάτω επιλογές είναι διαθέσιμες.

>[AZURE.NOTE]Έχετε υπόψη ότι αυτές οι επιλογές έχουν τη δυνατότητα απώλεια ορισμένων δεδομένων.     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>Επιλογή 2: Ανάπτυξη εκ νέου στη ρύθμιση παραμέτρων της υπηρεσίας cloud σε μια νέα περιοχή

Εάν έχετε τον αρχικό κωδικό, μπορείτε να απλώς απλώς αναπτύξτε ξανά την εφαρμογή συσχετισμένη ρύθμισης παραμέτρων και τους συναφείς πόρους σε μια νέα υπηρεσία cloud σε μια νέα περιοχή.  

Για περισσότερες λεπτομέρειες σχετικά με τον τρόπο δημιουργίας και ανάπτυξης μιας εφαρμογής υπηρεσίας cloud, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε και να αναπτύξετε μια υπηρεσία στο cloud](./cloud-services-how-to-create-deploy-portal.md).

Ανάλογα με τις προελεύσεις δεδομένων εφαρμογής, ίσως χρειαστεί να ελέγξετε τις διαδικασίες ανάκτησης για την προέλευση δεδομένων της εφαρμογής.
  * Για το χώρο αποθήκευσης Azure προελεύσεις δεδομένων, ανατρέξτε στο θέμα [αποθήκευσης Azure αναπαραγωγής](../storage/storage-redundancy.md#read-access-geo-redundant-storage) για να ελέγξετε για τις επιλογές που είναι διαθέσιμες με βάση το μοντέλο αναπαραγωγής επιλέξατε για την εφαρμογή σας.
  * Για προελεύσεις βάσεων δεδομένων SQL, διαβάστε [Επισκόπηση: Cloud επιχειρήσεις συνέχειας και βάσεων δεδομένων αποκατάσταση με βάση δεδομένων SQL](../sql-database/sql-database-business-continuity.md) για να ελέγξετε για τις επιλογές που είναι διαθέσιμες με βάση το επιλεγμένο αναπαραγωγής μοντέλο για την εφαρμογή σας.

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Επιλογή 3: Χρησιμοποιήστε μια ανάπτυξη του αντιγράφου ασφαλείας μέσω της διαχείρισης κίνηση Azure
Η επιλογή αυτή προϋποθέτει ότι έχετε σχεδιάσει ήδη η λύση εφαρμογής με τοπικές αποκατάσταση υπόψη. Μπορείτε να χρησιμοποιήσετε αυτήν την επιλογή εάν έχετε ήδη μια ανάπτυξη εφαρμογών υπηρεσιών δευτερεύοντα cloud που εκτελείται σε διαφορετική περιοχή και συνδεδεμένοι μέσω ενός καναλιού manager κίνηση. Σε αυτήν την περίπτωση, ελέγξτε την εύρυθμη λειτουργία της δευτερεύοντα ανάπτυξης. Εάν είναι σε καλή κατάσταση, μπορείτε να ανακατευθύνετε την κίνηση σε αυτό μέσω του Azure κίνηση Manager. Με αυτήν τη στρατηγική, μπορείτε να εκμεταλλευτείτε τη μέθοδο δρομολόγησης κίνηση και τις ρυθμίσεις παραμέτρων σειρά ανακατεύθυνσης στη Διαχείριση Azure κίνηση. Για περισσότερες πληροφορίες, δείτε [πώς μπορείτε να ρυθμίσετε τις παραμέτρους διαχείρισης κίνηση](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings).

![Εξισορρόπηση Azure Cloud Services σε περιοχές με τη Διαχείριση κίνηση Azure](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με το πώς μπορείτε να υλοποιήσετε ένα αποκατάσταση και υψηλή διαθεσιμότητα στρατηγική, ανατρέξτε στο θέμα [Αποκατάσταση και υψηλή διαθεσιμότητα για τις εφαρμογές του Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Για να αναπτύξετε μια λεπτομερή τεχνική Κατανόηση δυνατοτήτων του cloud μια πλατφόρμα, ανατρέξτε στο θέμα [τεχνική καθοδήγηση Azure υποστηρίζεται](../resiliency/resiliency-technical-guidance.md).

Εάν οι οδηγίες θα καταργήσετε, ή εάν θέλετε Microsoft για να κάνετε τις ενέργειες για λογαριασμό σας, επικοινωνήστε με [Την υποστήριξη πελατών](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
