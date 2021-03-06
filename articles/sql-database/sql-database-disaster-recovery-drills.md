<properties 
   pageTitle="Drills αποκατάστασης από καταστροφή βάσης δεδομένων SQL | Microsoft Azure" 
   description="Μάθετε οδηγίες και βέλτιστες πρακτικές για τη χρήση βάσης δεδομένων SQL Azure για την εκτέλεση drills αποκατάστασης από καταστροφή που θα σας βοηθήσει διατήρηση σας αποστολής κρίσιμες επιχειρηματικές εφαρμογές είναι ανθεκτικά να αποτυχίες και διακοπές." 
   services="sql-database" 
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/31/2016"
   ms.author="sstein; sashan"/>

#<a name="performing-disaster-recovery-drill"></a>Εκτέλεση διερεύνησης αποκατάστασης από καταστροφή

Συνιστάται να ότι η επικύρωση της εφαρμογής ετοιμότητας για τη ροή εργασίας αποκατάστασης πραγματοποιείται περιοδικά. Επαλήθευση η συμπεριφορά της εφαρμογής και συνέπειες του απώλεια δεδομένων ή/και τη διακοπή της περιλαμβάνει την ανακατεύθυνση που είναι μια καλή πρακτική μηχανικής. Επίσης, είναι απαίτηση από τα περισσότερα βιομηχανικά πρότυπα ως μέρος της πιστοποίησης συνέχειας επιχειρήσεις.

Εκτέλεση αποκατάστασης εμφάνιση λεπτομερειών από καταστροφή αποτελείται από:

- Προσομοιώνουν μη διαθεσιμότητα σειρά δεδομένων
- Ανάκτηση 
- Επικυρώστε αποκατάσταση δημοσίευση της ακεραιότητας εφαρμογών

Ανάλογα με τον τρόπο που [έχει σχεδιαστεί της εφαρμογής για αδιάκοπη παροχή υπηρεσιών](sql-database-business-continuity.md), η ροή εργασίας να εκτελέσει η άσκηση μπορεί να διαφέρει. Παρακάτω περιγράφουν τις βέλτιστες πρακτικές τη διεξαγωγή εμφάνιση λεπτομερειών αποκατάστασης από καταστροφή στο περιβάλλον της βάσης δεδομένων SQL Azure. 

##<a name="geo-restore"></a>Επαναφορά παν

Για να αποτρέψετε την ενδεχόμενη απώλεια δεδομένων κατά την εκτέλεση αποκατάστασης εμφάνιση λεπτομερειών από καταστροφή, συνιστάται να εκτελεί η άσκηση χρησιμοποιώντας ένα περιβάλλον δοκιμής, δημιουργώντας ένα αντίγραφο του στο περιβάλλον παραγωγής και χρησιμοποιώντας το για να επαληθεύσετε ροής εργασίας της εφαρμογής ανακατεύθυνσης.
 
####<a name="outage-simulation"></a>Μη διαθεσιμότητα προσομοίωσης

Για να προσομοιώσετε τα μη διαθεσιμότητα να διαγράψετε ή να μετονομάσετε τη βάση δεδομένων προέλευσης. Αυτό θα προκαλέσει την αποτυχία συνδεσιμότητας εφαρμογής. 

####<a name="recovery"></a>Ανάκτηση

- Εκτέλεση παν-επαναφορά της βάσης δεδομένων σε διαφορετικό διακομιστή, όπως περιγράφεται [εδώ](sql-database-disaster-recovery.md). 
- Αλλάξτε τις ρυθμίσεις της εφαρμογής για να συνδεθείτε με τα ανακτημένα βάσεις δεδομένων και ακολουθήστε τον οδηγό [Ρύθμιση παραμέτρων μιας βάσης δεδομένων μετά την ανάκτηση](sql-database-disaster-recovery.md) για να ολοκληρώσετε την ανάκτηση.

####<a name="validation"></a>Επικύρωση

- Ολοκλήρωση η άσκηση με επαλήθευση την ανάκτηση εφαρμογής της ακεραιότητας δημοσίευση (δηλαδή συμβολοσειρές σύνδεσης, συνδέσεις, βασικές λειτουργίες δοκιμές ή άλλο τμήμα επικυρώσεων βασική εφαρμογή signoffs διαδικασίες).

##<a name="geo-replication"></a>Παν αναπαραγωγής

Για μια βάση δεδομένων που προστατεύεται με χρήση της αναπαραγωγής παν η άσκηση διερεύνησης συνεπάγεται προγραμματισμένη ανακατεύθυνσης στη δευτερεύουσα βάση δεδομένων. Η προγραμματισμένη ανακατεύθυνσης εξασφαλίζει ότι δεν κύρια και δευτερεύουσα βάσεων δεδομένων παραμένει σε συγχρονισμό όταν οι ρόλοι έχουν αλλάξει. Σε αντίθεση με την μη προγραμματισμένη ανακατεύθυνση, αυτή η λειτουργία θα δεν έχει ως αποτέλεσμα απώλεια δεδομένων, ώστε να είναι δυνατή η άσκηση στο περιβάλλον παραγωγής. 

####<a name="outage-simulation"></a>Μη διαθεσιμότητα προσομοίωσης

Για να προσομοιώσετε τα μη διαθεσιμότητα μπορείτε να απενεργοποιήσετε την εφαρμογή web ή εικονική μηχανή συνδεδεμένοι στη βάση δεδομένων. Αυτό θα έχει ως αποτέλεσμα το αποτυχίες συνδεσιμότητας για τα προγράμματα-πελάτες web.

####<a name="recovery"></a>Ανάκτηση

- Βεβαιωθείτε ότι η ρύθμιση παραμέτρων της περιοχής DR οδηγεί στο δευτερεύον πρώην η οποία θα γίνει πλήρως προσβάσιμα νέα κύρια. 
- Εκτελέστε την [προγραμματισμένη ανακατεύθυνσης](sql-database-geo-replication-powershell.md#initiate-a-planned-failover) για να κάνετε μια νέα κύρια δευτερεύοντα βάσης δεδομένων
- Ακολουθήστε τον οδηγό [Ρύθμιση παραμέτρων μιας βάσης δεδομένων μετά την ανάκτηση](sql-database-disaster-recovery.md) για να ολοκληρώσετε την ανάκτηση.

####<a name="validation"></a>Επικύρωση

- Ολοκλήρωση η άσκηση με επαλήθευση την ανάκτηση εφαρμογής της ακεραιότητας δημοσίευση (δηλαδή συμβολοσειρές σύνδεσης, συνδέσεις, βασικές λειτουργίες δοκιμές ή άλλο τμήμα επικυρώσεων βασική εφαρμογή signoffs διαδικασίες).


## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε περισσότερα σχετικά με συνέχειας επιχειρηματικά σενάρια, ανατρέξτε στο θέμα [σενάρια συνέχειας](sql-database-business-continuity.md)
- Για να μάθετε σχετικά με τη βάση δεδομένων SQL Azure αυτόματης δημιουργίας αντιγράφων ασφαλείας, ανατρέξτε στο θέμα [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md)
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την ανάκτηση, ανατρέξτε στο θέμα [Επαναφορά βάσης δεδομένων από τα αντίγραφα ασφαλείας που ξεκινούν από την υπηρεσία](sql-database-recovery-using-backups.md)
- Για να μάθετε σχετικά με τις επιλογές αποκατάστασης του ταχύτερος, ανατρέξτε στο θέμα [Ενεργό-παν-αναπαραγωγής](sql-database-geo-replication-overview.md)  
