<properties
   pageTitle="Τεχνική καθοδήγηση: αποκατάστασης από εσωτερικής εγκατάστασης για να Azure | Microsoft Azure"
   description="Άρθρο κατανόηση και σχεδίαση αποκατάστασης συστήματα από την υποδομή εσωτερικής εγκατάστασης για να Azure"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-on-premises-to-azure"></a>Τεχνική καθοδήγηση Azure ανοχή: αποκατάστασης από εσωτερικής εγκατάστασης για να Azure

Azure παρέχει ένα ολοκληρωμένο σύνολο των υπηρεσιών για την ενεργοποίηση την επέκταση του ένα κέντρο δεδομένων εσωτερικής εγκατάστασης για να Azure για υψηλή διαθεσιμότητα και την καταστροφή αποκατάσταση:

* __Δίκτυο__: με ένα εικονικό ιδιωτικό δίκτυο, μπορείτε με ασφάλεια να επεκτείνετε το δίκτυο εσωτερικής εγκατάστασης στο cloud.
* __Τον υπολογισμό__: οι πελάτες που χρησιμοποιούν το Hyper-V εσωτερικής εγκατάστασης να "ανύψωση και shift" υπάρχοντα εικονικές μηχανές (ΣΠΣ) για να Azure.
* __Χώρος αποθήκευσης__: StorSimple επεκτείνει το σύστημα αρχείων σας με το χώρο αποθήκευσης Azure. Η υπηρεσία Azure δημιουργίας αντιγράφων ασφαλείας παρέχει δημιουργίας αντιγράφων ασφαλείας για τα αρχεία και βάσεις δεδομένων SQL με το χώρο αποθήκευσης Azure.
* __Αναπαραγωγή βάσης δεδομένων__: με SQL Server 2014 (ή νεότερη έκδοση) διαθεσιμότητα ομάδες, μπορείτε να υλοποιήσετε υψηλή διαθεσιμότητα και την καταστροφή ανάκτησης για τα δεδομένα εσωτερικής εγκατάστασης.

##<a name="networking"></a>Δικτύωση

Μπορείτε να χρησιμοποιήσετε Azure εικονικού δικτύου για να δημιουργήσετε μια ενότητα λογικά απομόνωσης στο Azure και με ασφάλεια συνδέστε το με το κέντρο δεδομένων εσωτερικής εγκατάστασης ή ένα μεμονωμένο πρόγραμμα-πελάτης χρησιμοποιώντας μια σύνδεση ασφαλείας IP. Με το εικονικό δίκτυο, μπορείτε να επωφεληθείτε της υποδομής του μεταβλητού μεγέθους, σε ' απαίτηση στο Azure ενώ παρέχει τη δυνατότητα σύνδεσης με δεδομένα και τις εφαρμογές στην εσωτερική εγκατάσταση, συμπεριλαμβανομένων των συστήματα που εκτελούν Windows Server, τους κεντρικούς υπολογιστές και UNIX. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Azure δικτύωσης τεκμηρίωση](../virtual-network/virtual-networks-overview.md) .

##<a name="compute"></a>Υπολογισμός

Εάν χρησιμοποιείτε το Hyper-V εσωτερικής εγκατάστασης, μπορείτε να "ανύψωση και shift" υπάρχουσες εικονικές μηχανές σε Azure και υπηρεσίες παροχής με Windows Server 2012 (ή νεότερη έκδοση), χωρίς να κάνουν αλλαγές σε την εικονική Μηχανή ή τη μετατροπή Εικονική μορφές. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [σχετικά με δίσκων και VHD για Azure εικονικές μηχανές](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

##<a name="azure-site-recovery"></a>Επαναφορά τοποθεσίας Azure

Εάν θέλετε αποκατάσταση ως υπηρεσία (DRaaS), Azure παρέχει [Azure τοποθεσίας αποκατάστασης](https://azure.microsoft.com/services/site-recovery/). Azure Επαναφορά τοποθεσίας προσφέρει ολοκληρωμένη προστασία για τους διακομιστές Hyper-V και φυσική VMware. Με Azure Επαναφορά τοποθεσίας, μπορείτε να χρησιμοποιήσετε έναν άλλο διακομιστή εσωτερικής εγκατάστασης ή Azure ως η τοποθεσία σας αποκατάστασης. Για περισσότερες πληροφορίες σχετικά με Azure αποκατάστασης τοποθεσίας, ανατρέξτε στην [τεκμηρίωση Azure τοποθεσίας αποκατάστασης](https://azure.microsoft.com/documentation/services/site-recovery/).

##<a name="storage"></a>Χώρος αποθήκευσης

Υπάρχουν αρκετές επιλογές για τη χρήση Azure ως ένα αντίγραφο ασφαλείας της τοποθεσίας για τα δεδομένα εσωτερικής εγκατάστασης.

###<a name="storsimple"></a>StorSimple

StorSimple με ασφάλεια και διαφανή Ενοποιείται χώρο αποθήκευσης στο cloud για εφαρμογές εσωτερικής εγκατάστασης. Επίσης, προσφέρει μια μεμονωμένη συσκευή που προσφέρει ομότιμου τοπική υψηλών επιδόσεων και χώρο αποθήκευσης στο cloud, ζωντανή αρχειοθέτηση, προστασία των δεδομένων που βασίζεται στο cloud και αποκατάσταση. Για περισσότερες πληροφορίες, ανατρέξτε στη [σελίδα StorSimple προϊόντος](https://azure.microsoft.com/services/storsimple/).

###<a name="azure-backup"></a>Δημιουργία αντιγράφων ασφαλείας Azure

Azure δημιουργίας αντιγράφων ασφαλείας σας δίνει τη δυνατότητα δημιουργίας αντιγράφων ασφαλείας cloud, χρησιμοποιώντας την οικεία εργαλεία δημιουργίας αντιγράφων ασφαλείας στο Windows Server 2012 (ή νεότερη έκδοση), Windows Server 2012 Essentials (ή νεότερη έκδοση) και διαχείριση προστασίας δεδομένων 2012 κέντρο σύστημα (ή νεότερη έκδοση). Αυτά τα εργαλεία παρέχουν μια ροή εργασίας για τη Διαχείριση αντιγράφων ασφαλείας που είναι ανεξάρτητο από τη θέση αποθήκευσης των αντιγράφων ασφαλείας, ανεξάρτητα από το χώρο αποθήκευσης Azure ή μια στον τοπικό δίσκο. Αφού είναι αντίγραφα ασφαλείας δεδομένων στο cloud, εξουσιοδοτημένους χρήστες εύκολα να ανακτήσετε αντίγραφα ασφαλείας σε οποιονδήποτε διακομιστή.

Με αυξανόμενες αντίγραφα ασφαλείας, μόνο οι αλλαγές στα αρχεία μεταφέρονται στο cloud. Αυτό σας βοηθά να αποτελεσματικά χρησιμοποιούν το χώρο αποθήκευσης, μειώσετε την κατανάλωση εύρους ζώνης και υποστήριξης στη δεδομένη χρονική στιγμή αποκατάστασης από τις πολλαπλές εκδόσεις των δεδομένων. Μπορείτε επίσης να επιλέξετε να χρησιμοποιήσετε πρόσθετες δυνατότητες, όπως οι πολιτικές διατήρησης δεδομένων, συμπίεση δεδομένων και μεταφορά δεδομένων περιορισμού. Χρήση Azure ως τη θέση των αντιγράφων ασφαλείας έχει το εμφανή πλεονέκτημα ότι η δημιουργία αντιγράφων ασφαλείας είναι αυτόματα "εκτός τοποθεσίας". Αυτό καταργεί τις επιπλέον απαιτήσεις για την ασφάλεια και προστασία επιτόπου μέσα αντιγράφων ασφαλείας.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι το αντίγραφο ασφαλείας Azure;](../backup/backup-introduction-to-azure-backup.md) και [Ρύθμιση παραμέτρων Azure δημιουργίας αντιγράφων ασφαλείας για τα δεδομένα DPM](https://technet.microsoft.com/library/jj728752.aspx).

##<a name="database"></a>Βάση δεδομένων

Μπορείτε να έχετε μια λύση αποκατάστασης από καταστροφή για τις βάσεις δεδομένων SQL Server σε ένα περιβάλλον υβριδική-IT χρησιμοποιώντας ομάδες διαθεσιμότητας AlwaysOn, δημιουργία ειδώλου βάσης δεδομένων, Αποστολή αρχείου καταγραφής και δημιουργία αντιγράφων ασφαλείας και επαναφορά με το χώρο αποθήκευσης αντικειμένων Blob του Azure. Όλες αυτές οι λύσεις Χρησιμοποιήστε SQL Server που εκτελείται σε εικονικές μηχανές Windows Azure.

Ομάδες διαθεσιμότητας AlwaysOn μπορεί να χρησιμοποιηθεί σε ένα περιβάλλον υβριδική-IT όπου αντίγραφα βάσης δεδομένων υπάρχουν δύο εσωτερικής εγκατάστασης και στο cloud. Αυτό εμφανίζεται στο παρακάτω διάγραμμα.

![Ομάδες διαθεσιμότητα του SQL Server AlwaysOn σε υβριδική στο cloud αρχιτεκτονικής](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-3.png)

Δημιουργία ειδώλου βάσης δεδομένων μπορούν να εκτείνονται σε διακομιστές εσωτερικής εγκατάστασης και cloud σε ένα πρόγραμμα εγκατάστασης που βασίζεται σε πιστοποιητικό. Το παρακάτω διάγραμμα παρουσιάζει αυτής της έννοιας.

![SQL Server βάσης δεδομένων αποτελούν πιστή αναπαράσταση σε υβριδική αρχιτεκτονική cloud](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-4.png)

Αποστολή αρχείων καταγραφής μπορεί να χρησιμοποιηθεί για να συγχρονίσετε μια βάση δεδομένων εσωτερικής εγκατάστασης με μια βάση δεδομένων SQL Server σε μια εικονική μηχανή Azure.

![Αρχείο καταγραφής του SQL Server αποστολής σε μια υβριδική αρχιτεκτονική cloud](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-5.png)

Τέλος, μπορείτε να δημιουργήσετε αντίγραφα ασφαλείας για μια βάση δεδομένων εσωτερικής εγκατάστασης απευθείας στο χώρο αποθήκευσης αντικειμένων Blob του Azure.

![Δημιουργία αντιγράφου ασφαλείας SQL Server με τον χώρο αποθήκευσης αντικειμένων Blob του Azure σε αρχιτεκτονική cloud υβριδική](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-6.png)

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Υψηλή διαθεσιμότητα και την καταστροφή ανάκτησης για τον SQL Server σε εικονικές μηχανές Windows Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) και [Δημιουργία αντιγράφων ασφαλείας και επαναφοράς για τον SQL Server σε εικονικές μηχανές Windows Azure](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).

##<a name="checklists-for-on-premises-recovery-in-microsoft-azure"></a>Λίστες ελέγχου για την ανάκτηση της εσωτερικής εγκατάστασης στο Microsoft Azure

###<a name="networking"></a>Δικτύωση

  1. Εξετάστε την ενότητα δικτύωσης αυτού του εγγράφου.
  2. Χρησιμοποιήστε εικονικού δικτύου για την ασφαλή σύνδεση εσωτερικής εγκατάστασης στο cloud.

###<a name="compute"></a>Υπολογισμός

  1. Εξετάστε την ενότητα υπολογισμού αυτού του εγγράφου.
  2. Επανατοποθέτηση ΣΠΣ μεταξύ Hyper-V και Azure.

###<a name="storage"></a>Χώρος αποθήκευσης

  1. Εξετάστε την ενότητα χώρος αποθήκευσης αυτού του εγγράφου.
  2. Εκμεταλλευτείτε τις υπηρεσίες StorSimple για τη χρήση χώρο αποθήκευσης στο cloud.
  3. Χρησιμοποιήστε την υπηρεσία Azure δημιουργίας αντιγράφων ασφαλείας.

###<a name="database"></a>Βάση δεδομένων

  1. Εξετάστε την ενότητα βάσης δεδομένων αυτού του εγγράφου.
  2. Εξετάστε το ενδεχόμενο χρήσης SQL Server σε VM Azure ως το αντίγραφο ασφαλείας.
  3. Ορισμός ομάδων διαθεσιμότητας AlwaysOn.
  4. Ρύθμιση παραμέτρων αποτελούν πιστή αναπαράσταση των βάσεων δεδομένων που βασίζεται σε πιστοποιητικό.
  5. Χρησιμοποιήστε την αποστολή αρχείων καταγραφής.
  6. Δημιουργία αντιγράφου ασφαλείας βάσεις δεδομένων εσωτερικής εγκατάστασης με το χώρο αποθήκευσης αντικειμένων Blob του Azure.

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο αποτελεί μέρος μιας σειράς εστιασμένη σε [τεχνική καθοδήγηση Azure υποστηρίζεται](./resiliency-technical-guidance.md). Το επόμενο άρθρο σε αυτήν τη σειρά είναι [αποκατάστασης από καταστροφή δεδομένων ή διαγραφή κατά λάθος](./resiliency-technical-guidance-recovery-data-corruption.md).