<properties
    pageTitle="Υποστηριζόμενες πλατφόρμες μετεγκατάστασης IaaS πόρων από κλασική διαχείριση πόρων για να Azure | Microsoft Azure"
    description="Σε αυτό το άρθρο παρουσιάζει τη μετεγκατάσταση που υποστηρίζονται πλατφόρμα πόρων από κλασική διαχείριση πόρων για να Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kasing"/>

# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Υποστηριζόμενες πλατφόρμες μετεγκατάστασης IaaS πόρων από κλασική διαχείριση πόρων για να Azure

Σε αυτό το άρθρο θα σας περιγράφουν πώς θα σας Ενεργοποίηση μετεγκατάστασης της υποδομής ως πόροι υπηρεσίας (IaaS) από το κλασικό σε μοντέλα ανάπτυξης διαχείρισης πόρων. Μπορείτε να διαβάσετε περισσότερα σχετικά με τις [δυνατότητες της διαχείρισης πόρων Azure και τα πλεονεκτήματα](../azure-resource-manager/resource-group-overview.md). Θα σας με λεπτομέρειες τον τρόπο σύνδεσης πόρους από τα μοντέλα δύο ανάπτυξης που συνυπάρχουν στη συνδρομή σας, χρησιμοποιώντας πύλες τοποθεσίας σε τοποθεσία εικονικού δικτύου. 

## <a name="goal-for-migration"></a>Στόχος για μετεγκατάσταση

Διαχείριση πόρων επιτρέπει την ανάπτυξη εφαρμογών σύνθετα πρότυπα, ρυθμίζει τις παραμέτρους εικονικές μηχανές χρησιμοποιώντας τις επεκτάσεις Εικονική και ενσωματώνει πρόσβασης διαχείρισης και εφαρμογή ετικετών. Azure διαχείριση πόρων περιλαμβάνει μεταβλητού μεγέθους, παράλληλα για ανάπτυξη εικονικές μηχανές σε σύνολα διαθεσιμότητα. Το νέο μοντέλο ανάπτυξης παρέχει επίσης διαχείρισης κύκλου ζωής του υπολογισμού, δικτύου και αποθήκευσης ανεξάρτητα. Τέλος, υπάρχει μια εστίαση σχετικά με την ενεργοποίηση ασφαλείας από προεπιλογή με την επιβολή της εικονικές μηχανές σε ένα εικονικό δίκτυο.

Σχεδόν όλες τις δυνατότητες του από το μοντέλο κλασική ανάπτυξης υποστηρίζονται για υπολογισμού, το δίκτυο και χώρου αποθήκευσης στην περιοχή Διαχείριση Azure πόρων. Για να επωφεληθείτε από τις νέες δυνατότητες στη Διαχείριση Azure πόρων, μπορείτε να κάνετε μετεγκατάσταση υπάρχοντος αναπτύξεις από το μοντέλο ανάπτυξης κλασική.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Αλλαγές στην αυτοματοποίηση και tooling μετά τη μετεγκατάσταση

Ως μέρος της μετεγκατάσταση τους πόρους σας από το μοντέλο ανάπτυξης κλασική στο μοντέλο ανάπτυξης για τη διαχείριση πόρων, πρέπει να ενημερώσετε το υπάρχον αυτοματισμού ή tooling για να βεβαιωθείτε ότι το εξακολουθούν να λειτουργούν μετά τη μετεγκατάσταση.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Έννοια της μετεγκατάστασης IaaS πόρων από κλασική για τη διαχείριση πόρων

Πριν από την μας Διερεύνηση σε τις λεπτομέρειες, ας δούμε τη διαφορά μεταξύ των λειτουργιών δεδομένων-επιπέδου και διαχείριση επιπέδου σχετικά με τους πόρους IaaS.

- *Διαχείριση επιπέδου* περιγράφει τις κλήσεις που έρχονται στα το επίπεδο διαχείρισης ή το API για την τροποποίηση πόρους. Για παράδειγμα, λειτουργίες όπως τη δημιουργία μια Εικονική και επανεκκίνηση μια Εικονική ενημέρωση ένα εικονικό δίκτυο με ένα νέο υποδίκτυο Διαχείριση τους πόρους που εκτελείται. Απευθείας δεν επηρεάζουν τη σύνδεση με τις παρουσίες.
- *Επίπεδο δεδομένων* (εφαρμογή) περιγράφει το περιβάλλον εκτέλεσης της την ίδια την εφαρμογή και περιλαμβάνει την αλληλεπίδραση με τις εμφανίσεις που δεν περνούν από το API Azure. Πρόσβαση σε τοποθεσία Web σας ή έλξη δεδομένων από μια ενσωματωμένη παρουσία του SQL Server ή σε ένα διακομιστή MongoDB θα θεωρείται δεδομένων επίπεδο ή εφαρμογή επικοινωνίας. Αντιγραφή ένα blob από ένα λογαριασμό του χώρου αποθήκευσης και πρόσβαση σε μια δημόσια διεύθυνση IP RDP ή SSH σε η εικονική μηχανή είναι επίσης επίπεδο δεδομένων. Αυτές οι λειτουργίες Διατηρήστε την εφαρμογή που εκτελείται σε υπολογισμού, τη δικτύωση και χώρου αποθήκευσης.

>[AZURE.NOTE] Σε ορισμένα σενάρια μετεγκατάστασης, την πλατφόρμα Azure σταματά, καταργεί και επανεκκίνηση του σας εικονικές μηχανές. Αυτό επιφέρει ένα σύντομο χρόνο εκτός λειτουργίας επίπεδο δεδομένων.

## <a name="supported-scopes-of-migration"></a>Υποστηριζόμενες εύρους της μετεγκατάστασης

Υπάρχουν τρεις εύρους μετεγκατάστασης που στοχεύουν κυρίως υπολογισμού, δικτύου και χώρου αποθήκευσης. 

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Μετεγκατάσταση των εικονικές μηχανές (όχι σε ένα εικονικό δίκτυο)

Στο μοντέλο ανάπτυξης για τη διαχείριση πόρων, ασφαλείας τίθεται σε ισχύ για τις εφαρμογές σας από προεπιλογή. Όλα ΣΠΣ πρέπει να είστε σε δίκτυο εικονικού στο μοντέλο διαχείρισης πόρων. Την επανεκκίνηση του Azure πλατφόρμα (`Stop`, `Deallocate`, και `Start`) του ΣΠΣ ως μέρος της μετεγκατάστασης. Έχετε δύο επιλογές για το εικονικό δίκτυα:

- Μπορείτε να ζητήσετε την πλατφόρμα για να δημιουργήσετε ένα νέο εικονικό δίκτυο και να μετεγκαταστήσετε την εικονική μηχανή στο νέο εικονικό δίκτυο.
- Μπορείτε να μετεγκαταστήσετε την εικονική μηχανή σε ένα υπάρχον δίκτυο εικονικού στη Διαχείριση πόρων.

>[AZURE.NOTE] Σε αυτή την εμβέλεια μετεγκατάστασης, οι λειτουργίες διαχείρισης επιπέδου και τις λειτουργίες δεδομένων επίπεδο δεν επιτρέπεται για ένα χρονικό διάστημα κατά τη διάρκεια της μετεγκατάστασης.

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Μετεγκατάσταση των εικονικές μηχανές (σε ένα εικονικό δίκτυο)

Για περισσότερες ρυθμίσεις παραμέτρων Εικονική, μόνο τα μετα-δεδομένα μετεγκατάσταση μεταξύ των μοντέλων ανάπτυξης κλασική και διαχείριση πόρων. Το υποκείμενο ΣΠΣ εκτελούνται στον ίδιο υλικό, στο ίδιο δίκτυο και με τον ίδιο χώρο αποθήκευσης. Οι λειτουργίες διαχείρισης επίπεδο δεν επιτρέπεται για ένα συγκεκριμένο χρονικό διάστημα κατά τη διάρκεια της μετεγκατάστασης. Ωστόσο, το επίπεδο δεδομένων εξακολουθούν να λειτουργούν. Δηλαδή, τις εφαρμογές που εκτελούνται επάνω σε ΣΠΣ (κλασική) δεν γίνονται χρόνου εκτός λειτουργίας κατά τη διάρκεια της μετεγκατάστασης.

Τις ακόλουθες ρυθμίσεις παραμέτρων δεν υποστηρίζονται αυτήν τη στιγμή. Εάν υποστήριξη προστίθεται στο μέλλον, ορισμένες ΣΠΣ σε αυτήν τη ρύθμιση παραμέτρων ίσως να προκύψουν χρόνου εκτός λειτουργίας (Μετάβαση μέσω διακοπή, εκχώρηση και κάντε επανεκκίνηση του λειτουργίες Εικονική).

-   Έχετε περισσότερες από μία διαθεσιμότητα Ορισμός σε μια υπηρεσία cloud μία.
-   Έχετε μία ή περισσότερες διαθεσιμότητα σύνολα και ΣΠΣ που δεν βρίσκονται σε μια ρύθμιση σε μια υπηρεσία cloud μόνο διαθεσιμότητα.

>[AZURE.NOTE] Σε αυτή την εμβέλεια μετεγκατάστασης, το επίπεδο διαχείρισης δεν επιτρέπεται για ένα χρονικό διάστημα κατά τη διάρκεια της μετεγκατάστασης. Για ορισμένες ρυθμίσεις παραμέτρων όπως περιγράφεται παραπάνω, παρουσιάζεται δεδομένων επίπεδο χρόνου εκτός λειτουργίας.

### <a name="storage-accounts-migration"></a>Μετεγκατάσταση λογαριασμών χώρου αποθήκευσης

Για να επιτρέψετε την απρόσκοπτη μετεγκατάστασης, μπορείτε να αναπτύξετε ΣΠΣ διαχείριση πόρων σε ένα λογαριασμό κλασική χώρου αποθήκευσης. Με αυτήν τη δυνατότητα, υπολογισμού και πόρους δικτύου μπορεί να και πρέπει να μετεγκαταστήσετε ανεξάρτητα από λογαριασμούς χώρου αποθήκευσης. Αφού μετεγκατάσταση επάνω από τις εικονικές μηχανές και εικονικού δικτύου, θα πρέπει να μετεγκαταστήσετε πάνω από τους λογαριασμούς σας χώρου αποθήκευσης για την ολοκλήρωση της διαδικασίας μετεγκατάστασης. 

>[AZURE.NOTE] Το μοντέλο ανάπτυξης διαχείρισης πόρων δεν διαθέτει την έννοια της κλασική εικόνες και δίσκων. Όταν το λογαριασμό χώρου αποθήκευσης είναι μετεγκαταστάθηκαν, κλασική εικόνες και δίσκων δεν είναι ορατές σε στοίβα από διαχειριστή πόρων αλλά της δημιουργίας αντιγράφων VHD παραμένουν στο λογαριασμό χώρου αποθήκευσης. 

## <a name="unsupported-features-and-configurations"></a>Μη υποστηριζόμενες δυνατότητες και τις ρυθμίσεις παραμέτρων

Δεν αυτήν τη στιγμή υποστηρίζουμε ορισμένες δυνατότητες και ρυθμίσεις παραμέτρων. Οι παρακάτω ενότητες περιγράφουν μας συστάσεις σχετικά με τους.

### <a name="unsupported-features"></a>Μη υποστηριζόμενες δυνατότητες

Οι ακόλουθες δυνατότητες δεν υποστηρίζονται αυτήν τη στιγμή. Μπορείτε να καταργήσετε προαιρετικά αυτές τις ρυθμίσεις, μετεγκατάσταση του ΣΠΣ και, στη συνέχεια, να ενεργοποιήσετε ξανά τις ρυθμίσεις στο μοντέλο ανάπτυξης διαχείρισης πόρων.

Υπηρεσία παροχής πόρων | Η δυνατότητα
---------- | ------------
Υπολογισμός | Μη συσχετισμένη εικονική μηχανή δίσκων.
Υπολογισμός | Εικονική μηχανή εικόνες.
Δικτύου | ACL τελικού σημείου.
Δικτύου | Πύλες εικονικού δικτύου (τοποθεσίας, Azure ExpressRoute, πύλη εφαρμογής, τοποθετήστε το δείκτη στην τοποθεσία).
Δικτύου | Χρήση του VNet διεισδύουν εικονικού δίκτυα. (Μετεγκατάσταση VNet στο ARM, στη συνέχεια, ομότιμη) Μάθετε περισσότερα σχετικά με [VNet διεισδύουν] (... /Virtual-Network/Virtual-Network-peering-overview.MD).
Δικτύου | Τα προφίλ της διαχείρισης κίνηση.

### <a name="unsupported-configurations"></a>Ρυθμίσεις δεν υποστηρίζονται

Τις ακόλουθες ρυθμίσεις παραμέτρων δεν υποστηρίζονται αυτήν τη στιγμή.

Υπηρεσία | Ρύθμιση παραμέτρων | Σύσταση
---------- | ------------ | ------------
Διαχείριση πόρων | Ρόλος βάσει πρόσβασης ελέγχου (RBAC) για τους πόρους κλασική | Επειδή το URI από τους πόρους τροποποιείται μετά τη μετεγκατάσταση, συνιστάται να σχεδιάζετε με τις ενημερώσεις πολιτικής RBAC που πρέπει να συμβεί μετά τη μετεγκατάσταση.
Υπολογισμός | Πολλά δευτερεύοντα δίκτυα που σχετίζονται με μια εικονική Μηχανή | Ενημερώστε τις ρυθμίσεις παραμέτρων υποδικτύου για αναφορά μόνο δευτερεύοντα δίκτυα.
Υπολογισμός | Εικονικές μηχανές που ανήκουν σε ένα εικονικό δίκτυο, αλλά δεν έχετε μια ρητή υποδικτύου στους οποίους έχουν ανατεθεί | Προαιρετικά, μπορείτε να διαγράψετε την εικονική Μηχανή.
Υπολογισμός | Εικονικές μηχανές που έχουν ειδοποιήσεις, Autoscale πολιτικές | Η μετεγκατάσταση, έχετε και χάνονται αυτές τις ρυθμίσεις. Συνιστάται ιδιαίτερα αξιολογήσετε το περιβάλλον σας πριν το κάνετε τη μετεγκατάσταση. Εναλλακτικά, μπορείτε να ρυθμίσετε ξανά τις ρυθμίσεις ειδοποίησης μετά την ολοκλήρωση της μετεγκατάστασης.
Υπολογισμός | Επεκτάσεις Εικονική XML (BGInfo 1.*, πρόγραμμα εντοπισμού σφαλμάτων Visual Studio, ανάπτυξη Web και ο απομακρυσμένος εντοπισμός σφαλμάτων) | Αυτό δεν υποστηρίζεται. Συνιστάται να που μπορείτε να καταργήσετε αυτές τις επεκτάσεις από την εικονική μηχανή για να συνεχίσετε τη μετεγκατάσταση ή αυτά θα καταργηθεί αυτόματα κατά τη διάρκεια της διαδικασίας μετεγκατάστασης.
Υπολογισμός | Διαγνωστικά εκκίνησης με Premium αποθήκευσης | Απενεργοποίηση δυνατότητας Διαγνωστικά εκκίνησης για του ΣΠΣ πριν να συνεχίσετε με μετεγκατάστασης. Μπορείτε να ενεργοποιήσετε ξανά τα Διαγνωστικά εκκίνησης στη στοίβα από διαχειριστή πόρων μετά την ολοκλήρωση της μετεγκατάστασης. Επιπλέον, θα πρέπει να διαγραφεί αντικείμενα BLOB που χρησιμοποιούνται για το στιγμιότυπο οθόνης και σειριακή αρχεία καταγραφής, ώστε να που δεν είναι πλέον θα χρεωθείτε για αυτά τα αντικείμενα blob.
Υπολογισμός | Υπηρεσίες cloud που περιέχουν τους ρόλους web/εργαζόμενου | Προς το παρόν δεν υποστηρίζεται.
Δικτύου | Εικονικό δίκτυα που περιέχουν εικονικές μηχανές και τους ρόλους web/εργαζόμενου |  Προς το παρόν δεν υποστηρίζεται.
Azure εφαρμογής υπηρεσίας | Εικονικό δίκτυα που περιέχουν περιβάλλοντα εφαρμογής υπηρεσίας | Προς το παρόν δεν υποστηρίζεται.
Azure HDInsight | Εικονικό δίκτυα που περιέχουν τις υπηρεσίες HDInsight | Προς το παρόν δεν υποστηρίζεται.
Υπηρεσίες Microsoft Dynamics κύκλου ζωής | Εικονικό δίκτυα που περιέχουν εικονικές μηχανές που πραγματοποιείται από τις υπηρεσίες Dynamics κύκλου ζωής | Προς το παρόν δεν υποστηρίζεται.
Υπολογισμός | Κέντρο ασφάλειας Azure επεκτάσεις με ένα VNET που έχει μια πύλη VPN ή ER πύλης με το διακομιστή DNS εσωτερικής εγκατάστασης | Κέντρο ασφάλειας Azure εγκαθιστά αυτόματα επεκτάσεις σε σας εικονικές μηχανές να παρακολουθούν τους ασφαλείας και να βελτιώσετε τις ειδοποιήσεις. Αυτές οι επεκτάσεις συνήθως να εγκατασταθεί αυτόματα εάν είναι ενεργοποιημένη η πολιτική Azure Κέντρο ασφάλειας της συνδρομής. Καθώς η μετεγκατάσταση πύλης δεν υποστηρίζεται αυτήν τη στιγμή και της πύλης πρέπει να διαγραφούν πριν να συνεχίσετε με την ολοκλήρωση της μετεγκατάστασης, την πρόσβαση στο internet με το λογαριασμό χώρου αποθήκευσης Εικονική χάνονται όταν έχει διαγραφεί από την πύλη. Η μετεγκατάσταση δεν θα συνεχιστεί όταν συμβαίνει αυτό, καθώς δεν είναι δυνατό να συμπληρωθεί το blob κατάσταση παράγοντας επισκέπτη. Συνιστάται να απενεργοποιήσετε την πολιτική Azure Κέντρο ασφάλειας της συνδρομής 3 ώρες πριν να συνεχίσετε με μετεγκατάστασης.

## <a name="the-migration-experience"></a>Η εμπειρία μετεγκατάστασης

Πριν να ξεκινήσετε τη μετεγκατάσταση εμπειρία, συνιστάται τα εξής:

- Βεβαιωθείτε ότι δεν χρησιμοποιούν τους πόρους που θέλετε να μετεγκαταστήσετε τις δυνατότητες που δεν υποστηρίζονται ή ρυθμίσεις παραμέτρων. Συνήθως της πλατφόρμας εντοπίσει αυτά τα θέματα και δημιουργεί ένα μήνυμα σφάλματος.
- Εάν έχετε ΣΠΣ που δεν βρίσκονται σε ένα εικονικό δίκτυο, αυτά θα διακοπεί και κατάργηση εκχώρησης ως μέρος της λειτουργίας προετοιμασία. Εάν δεν θέλετε να χάσετε τη δυνατότητα στη δημόσια διεύθυνση IP, Ρίξτε μια ματιά σε δέσμευση της διεύθυνσης IP πριν από την ενεργοποίηση της λειτουργίας προετοιμασία. Ωστόσο, εάν το ΣΠΣ είναι σε ένα εικονικό δίκτυο, είναι δεν διακοπεί και κατάργηση εκχώρησης.
- Σχεδιασμός μετεγκατάστασή σας κατά τη διάρκεια μη-εργάσιμων ωρών για να χωρέσει για τυχόν μη αναμενόμενο αποτυχίες που μπορεί να συμβεί κατά τη διάρκεια της μετεγκατάστασης.
- Κάντε λήψη του η τρέχουσα ρύθμιση παραμέτρων του ΣΠΣ σας με τη χρήση του PowerShell, οι εντολές περιβάλλον γραμμής εντολών (CLI), ή API ΥΠΌΛΟΙΠΟ ώστε να είναι πιο εύκολη για επικύρωση μετά την ολοκλήρωση του βήματος προετοιμασία.
- Ενημερώστε τις δέσμες ενεργειών αυτοματισμού/operationalization να χειρίζεται το μοντέλο ανάπτυξης για τη διαχείριση πόρων, πριν να ξεκινήσετε τη μετεγκατάσταση. Προαιρετικά, μπορείτε να κάνετε ΛΉΨΗ λειτουργίες όταν οι πόροι είναι σε κατάσταση είστε έτοιμοι.
- Αξιολόγηση των πολιτικών RBAC που έχουν ρυθμιστεί σε κλασική IaaS πόρων και σχεδιασμός για μετά την ολοκλήρωση της μετεγκατάστασης.

Η ροή εργασίας μετεγκατάστασης είναι ως εξής

![Στιγμιότυπο οθόνης που εμφανίζει τη μετεγκατάσταση ροής εργασίας](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] Όλες οι εργασίες που περιγράφονται στις ενότητες που ακολουθούν είναι idempotent. Εάν έχετε κάποιο πρόβλημα εκτός μιας μη υποστηριζόμενης δυνατότητας ή ένα σφάλμα παραμέτρων, συνιστάται να επαναλάβετε την προετοιμασία, ματαίωση, ή ολοκλήρωση της λειτουργίας. Η πλατφόρμα Azure προσπαθεί ξανά την ενέργεια.

### <a name="validate"></a>Επικύρωση

Η λειτουργία επικύρωση είναι το πρώτο βήμα της διαδικασίας μετεγκατάστασης. Ο στόχος αυτού του βήματος είναι να αναλύετε δεδομένα στο παρασκήνιο για τους πόρους στην περιοχή μετεγκατάστασης και να επιστρέψετε επιτυχίας/αποτυχίας εάν οι πόροι είναι σε θέση να μετεγκατάστασης.

Επιλέγετε το εικονικό δίκτυο ή η υπηρεσία (Εάν δεν είναι ένα εικονικό δίκτυο) που θέλετε να επικυρώσετε για μετεγκατάσταση.

* Εάν ο πόρος δεν έχει τη δυνατότητα της μετεγκατάστασης, την πλατφόρμα Azure παραθέτει όλους τους λόγους για ποιο λόγο δεν υποστηρίζεται για μετεγκατάσταση.

### <a name="prepare"></a>Προετοιμασία

Η λειτουργία προετοιμασία είναι το δεύτερο βήμα της διαδικασίας μετεγκατάστασης. Ο στόχος αυτού του βήματος είναι να προσομοιώσετε τον μετασχηματισμό από τους πόρους IaaS από κλασική διαχείριση πόρων πόρους και την παρουσίαση με παράθεση για να απεικονίσετε.

Επιλέγετε το εικονικό δίκτυο ή η υπηρεσία (Εάν δεν είναι ένα εικονικό δίκτυο) που θέλετε να προετοιμαστείτε για μετεγκατάσταση.

* Εάν ο πόρος δεν έχει δυνατότητα μετεγκατάστασης, της πλατφόρμας Azure διακόπτει τη διαδικασία μετεγκατάστασης και εμφανίζει την αιτία γιατί απέτυχε η λειτουργία προετοιμασίας.
* Εάν ο πόρος έχει δυνατότητα μετεγκατάστασης, το πρώτο κλειδωμάτων Azure πλατφόρμα κάτω τις λειτουργίες διαχείρισης επιπέδου για τους πόρους στην περιοχή μετεγκατάστασης. Για παράδειγμα, δεν είναι δυνατή η για να προσθέσετε ένα δίσκο δεδομένων σε μια Εικονική στην περιοχή μετεγκατάστασης.

Azure την πλατφόρμα, στη συνέχεια, ξεκινά η μετεγκατάσταση μετα-δεδομένων από κλασική για τη διαχείριση πόρων για τους πόρους που η μετεγκατάσταση.

Όταν ολοκληρωθεί η λειτουργία προετοιμασία, έχετε την επιλογή των πόρων σε δύο κλασική οπτικοποίηση και διαχείριση πόρων. Για κάθε υπηρεσία cloud στο μοντέλο κλασική ανάπτυξης, την πλατφόρμα Azure δημιουργεί ένα όνομα ομάδας πόρου που έχει το μοτίβο `cloud-service-name>-migrated`.

>[AZURE.NOTE] Εικονικές μηχανές που δεν βρίσκονται σε μια κλασική εικονικού δικτύου τερματίζονται deallocated σε αυτήν τη φάση της μετεγκατάστασης.

### <a name="check-manual-or-scripted"></a>Έλεγχος (μη αυτόματη ή δέσμες ενεργειών)

Στο βήμα ελέγχου, μπορείτε να χρησιμοποιήσετε τις ρυθμίσεις παραμέτρων που λάβατε νωρίτερα προαιρετικά για την επικύρωση ότι η μετεγκατάσταση φαίνονται σωστά. Εναλλακτικά, μπορείτε να εισέλθετε στην πύλη και σημείο ελέγχου τις ιδιότητες και πόρους για να επικυρώσετε ότι σας αρέσει μετεγκατάστασης μετα-δεδομένων.

Εάν κάνετε μετεγκατάσταση ένα εικονικό δίκτυο, οι περισσότερες ρύθμιση των παραμέτρων του εικονικές μηχανές δεν γίνεται επανεκκίνηση. Για εφαρμογές σε αυτές τις VM, μπορείτε να επικυρώσετε ότι η εφαρμογή είναι ακόμη εγκατάσταση και λειτουργία.

Μπορείτε να ελέγξετε την παρακολούθηση αυτοματισμού και λειτουργικές δέσμες ενεργειών για να δείτε εάν το ΣΠΣ λειτουργούν όπως αναμένεται και τις δέσμες ενεργειών σας ενημερωμένο λειτουργεί σωστά. Υποστηρίζονται μόνο λειτουργίες GET όταν οι πόροι είναι σε κατάσταση είστε έτοιμοι.

Δεν υπάρχει καμία ρύθμιση χρονικού διαστήματος πριν από την οποία πρέπει να ολοκληρωθεί η μετεγκατάσταση. Μπορείτε να έχετε περισσότερο χρόνο θέλετε σε αυτήν την κατάσταση. Ωστόσο, το επίπεδο διαχείρισης είναι κλειδωμένο για αυτούς τους πόρους, μέχρι να ματαίωση ή ολοκλήρωση.

Εάν βλέπετε προβλήματα, μπορείτε να ματαιώσετε της μετεγκατάστασης και να επιστρέψετε στο μοντέλο κλασική ανάπτυξης πάντα. Αφού επιστρέψετε, την πλατφόρμα Azure θα ανοίξει τις λειτουργίες διαχείρισης επίπεδο σχετικά με τους πόρους ώστε να μπορείτε να συνεχίσετε το συνήθεις εργασίες σε αυτές τις VM στο μοντέλο κλασική ανάπτυξης.

### <a name="abort"></a>Ματαίωση

Ματαίωση το βήμα είναι προαιρετικό που μπορείτε να χρησιμοποιήσετε για να επαναφέρετε τις αλλαγές σας στο μοντέλο κλασική ανάπτυξης και να διακόψετε τη μετεγκατάσταση.

>[AZURE.NOTE] Αυτή η λειτουργία δεν είναι δυνατό να εκτελεστεί αφού έχετε ενεργοποίησε τη λειτουργία ολοκλήρωσης.  

### <a name="commit"></a>Υποβολή

Αφού ολοκληρώσετε τη διαδικασία επικύρωσης, μπορείτε να ολοκληρώσετε τη μετεγκατάσταση. Πόροι δεν εμφανίζονται πλέον στην κλασική και είναι διαθέσιμες μόνο στο μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε να διαχειριστείτε τους πόρους που μετεγκαταστάθηκαν μόνο στην νέα πύλη.

>[AZURE.NOTE] Αυτή είναι μια λειτουργία idempotent. Εάν αποτύχει, συνιστάται να επαναλάβετε τη λειτουργία. Αν εξακολουθεί να αποτύχει, δημιουργήστε ένα δελτίο υποστήριξης ή δημιουργήστε μια καταχώρηση φόρουμ με ετικέτα ClassicIaaSMigration στο μας [Εικονική φόρουμ](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

## <a name="frequently-asked-questions"></a>Συνήθεις ερωτήσεις

**Αυτό το πρόγραμμα μετεγκατάστασης επηρεάζει οποιαδήποτε από τις υπάρχουσες υπηρεσίες ή εφαρμογές που εκτελούνται σε Azure εικονικές μηχανές μου;**

Όχι. Το ΣΠΣ (κλασική) είναι υπηρεσίες υποστηρίζεται πλήρως στο γενικής διαθεσιμότητας. Μπορείτε να συνεχίσετε να χρησιμοποιείτε αυτούς τους πόρους για να αναπτύξετε το αποτύπωμα σε Windows Azure.

**Τι συμβαίνει με μου ΣΠΣ εάν που δεν σκοπεύετε να κάνετε μετεγκατάσταση στο άμεσο μέλλον;**

Θα σας δεν deprecating τον υπάρχουσες κλασική API και το μοντέλο πόρων. Θέλουμε να σας διευκολύνουν μετεγκατάστασης, εξετάζοντας τις σύνθετες δυνατότητες που είναι διαθέσιμες στο μοντέλο ανάπτυξης διαχείρισης πόρων. Προτείνουμε να αναθεωρείτε [ορισμένα από τα εξελίξεις](virtual-machines-windows-compare-deployment-models.md) που αποτελούν μέρος της IaaS στην περιοχή Διαχείριση πόρων.

**Τι σημαίνει αυτό το πρόγραμμα μετεγκατάστασης για το υπάρχον tooling;**

Ενημέρωση του tooling στο μοντέλο ανάπτυξης για τη διαχείριση πόρων είναι μία από τις πιο σημαντικές αλλαγές που έχετε με λογαριασμού για στο τα σχέδιά σας μετεγκατάστασης.

**Πόσος χρόνος θα ο χρόνος εκτός λειτουργίας επιπέδου διαχείρισης είναι;**

Εξαρτάται από τον αριθμό των πόρων που θα μετεγκατασταθούν. Για μικρότερες αναπτύξεις (μερικά δεκάδες ΣΠΣ), ολόκληρη η μετεγκατάσταση πρέπει να εκτελεί τουλάχιστον μία ώρα. Για αναπτύξεις ευρείας κλίμακας (εκατοντάδες ΣΠΣ), η μετεγκατάσταση μπορεί να διαρκέσει μερικές ώρες.

**Να συγκεντρώσετε μετά τη μετεγκατάσταση πόρων μου είναι δεσμευμένου στη Διαχείριση πόρων;**

Μπορείτε να ματαιώσετε τη μετεγκατάσταση με την προϋπόθεση ότι οι πόροι είναι σε κατάσταση είστε έτοιμοι. Επαναφορά δεν υποστηρίζεται, αφού τους πόρους που έχουν μετεγκατασταθεί με επιτυχία μέσω της λειτουργίας ολοκλήρωσης.

**Μπορώ να επαναφέρετε μου μετεγκατάστασης εάν αποτύχει η διαδικασία δέσμευσης;**

Δεν μπορείτε να ματαιώσετε μετεγκατάστασης, εάν αποτύχει η διαδικασία δέσμευσης. Όλες οι εργασίες μετεγκατάστασης, συμπεριλαμβανομένης της λειτουργίας ολοκλήρωσης, είναι idempotent. Επομένως, συνιστάται να ότι μπορείτε να επαναλάβετε τη λειτουργία μετά από μια σύντομη ώρα. Εάν εξακολουθείτε να κάνει ένα μήνυμα σφάλματος, δημιουργήστε ένα δελτίο υποστήριξης για ή δημιουργήστε μια δημοσίευση φόρουμ με την ετικέτα ClassicIaaSMigration στο μας [Εικονική φόρουμ](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

**Πρέπει να αγοράσετε μια άλλη κυκλώματος express δρομολόγηση, εάν είναι απαραίτητο να χρησιμοποιήσω IaaS στην περιοχή Διαχείριση πόρων;**

Όχι. Πρόσφατα ενεργοποιημένη [Μετακίνηση κυκλώματα ExpressRoute από το κλασικό στο μοντέλο ανάπτυξης διαχείρισης πόρων](../expressroute/expressroute-move.md). Δεν χρειάζεται να αγοράσετε ένα νέο κύκλωμα ExpressRoute εάν έχετε ήδη ένα.

**Τι γίνεται εάν που είχατε ρυθμίσει τις παραμέτρους πολιτικών έλεγχος πρόσβασης βάσει ρόλων για τους πόρους μου κλασική IaaS;**

Κατά τη διάρκεια της μετεγκατάστασης, οι πόροι μετασχηματισμός από κλασική για τη διαχείριση πόρων. Επομένως, συνιστάται να σχεδιάζετε με τις ενημερώσεις πολιτικής RBAC που πρέπει να συμβεί μετά τη μετεγκατάσταση.

**Τι γίνεται εάν χρησιμοποιώ το Azure τοποθεσίας ανάκτησης ή δημιουργία αντιγράφων ασφαλείας Azure σήμερα;**

Για να μετεγκαταστήσετε το εικονικό μηχάνημα που έχουν ενεργοποιηθεί για το αντίγραφο ασφαλείας, ανατρέξτε στο θέμα [που έχετε δημιουργήσει αντίγραφα ασφαλείας μου κλασική ΣΠΣ στο θάλαμο δημιουργίας αντιγράφων ασφαλείας. Τώρα θέλω να μετεγκαταστήσετε μου ΣΠΣ από Κλασική λειτουργία λειτουργία διαχείρισης πόρων. Πώς μπορώ να δημιουργία αντιγράφων ασφαλείας τους στο θάλαμο υπηρεσίες ανάκτησης;](../backup/backup-azure-backup-ibiza-faq.md#i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode-how-can-i-backup-them-in-recovery-services-vault)

**Να επικυρώσετε τη συνδρομή ή πόρους για να δείτε εάν είναι σε θέση να μετεγκατάστασης μου;**

Ναι. Στην επιλογή μετεγκατάσταση πλατφόρμα που υποστηρίζονται, το πρώτο βήμα κατά την προετοιμασία για μετεγκατάσταση είναι να επικυρώσετε ότι οι πόροι είναι σε θέση να μετεγκατάστασης. Σε περίπτωση που η λειτουργία επικύρωση αποτύχει, μπορείτε να λαμβάνετε μηνύματα για όλους τους λόγους που δεν μπορεί να ολοκληρωθεί η μετεγκατάσταση.

**Τι συμβαίνει εάν να αντιμετωπίσετε ένα σφάλμα ορίου κατά την προετοιμασία τους πόρους IaaS για μετεγκατάσταση;**

Συνιστάται να που ματαίωση μετεγκατάστασή σας και, στη συνέχεια, συνδεθείτε μια αίτηση υποστήριξης για να αυξήσετε το ορίων στην περιοχή όπου κάνετε μετεγκατάσταση του ΣΠΣ. Μετά την έγκριση της αίτησης ορίου, μπορείτε να ξεκινήσετε ξανά την εκτέλεση των βημάτων μετεγκατάστασης του.

**Πώς μπορώ να αναφέρω κάποιο πρόβλημα;**

Δημοσίευση σας θέματα και ερωτήσεις σχετικά με τη μετεγκατάσταση μας [Εικονική φόρουμ](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows), με τη λέξη-κλειδί ClassicIaaSMigration. Συνιστάται να καταχώρησης όλες τις ερωτήσεις σας στο φόρουμ αυτό. Εάν έχετε ένα συμβόλαιο υποστήριξης, είστε υποδοχής για να συνδεθείτε δελτίου υποστήριξης.

**Τι γίνεται εάν δεν μου αρέσει τα ονόματα των πόρων που επιλέξατε την πλατφόρμα κατά τη διάρκεια της μετεγκατάστασης;**

Όλοι οι πόροι που παρέχετε ρητά ονόματα στο μοντέλο κλασική ανάπτυξης διατηρούνται κατά τη διάρκεια της μετεγκατάστασης. Σε ορισμένες περιπτώσεις, δημιουργούνται νέων πόρων. Για παράδειγμα: δημιουργείται ένα περιβάλλον εργασίας δικτύου για κάθε Εικονική. Προς το παρόν δεν υποστηρίζουμε τη δυνατότητα να ελέγχετε τα ονόματα των αυτές τις νέες πόρων που δημιουργήσατε κατά τη διάρκεια της μετεγκατάστασης. Συνδεθείτε σας ψήφων για αυτήν τη δυνατότητα στο [φόρουμ Azure σχολίων](http://feedback.azure.com).

* *έλαβα ένα μήνυμα που αναφέρει *"είναι η αναφορά της κατάστασης παράγοντας συνολική Εικονική ώστε να μην είστε έτοιμοι. Ως εκ τούτου, δεν είναι δυνατό να μετεγκαταστήσετε το Εικονική. Βεβαιωθείτε ότι είναι η αναφορά συνολική κατάσταση παράγοντα ως ετοιμότητα για τον παράγοντα Εικονική"* ή *"Εικονική περιέχει επέκταση των οποίων η κατάσταση είναι όχι αυτό που αναφέρεται από την εικονική Μηχανή. Ως εκ τούτου, αυτή η Εικονική δεν είναι δυνατό να μετεγκατασταθούν."***

Αυτό το μήνυμα παραλήφθηκε όταν η Εικονική δεν έχει εξερχομένων τη σύνδεση στο Internet. Ο παράγοντας Εικονική χρησιμοποιεί εξερχομένων συνδεσιμότητας για την επίτευξη ο λογαριασμός Azure χώρου αποθήκευσης για την ενημέρωση της κατάστασης παράγοντας κάθε πέντε λεπτά.


## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που γνωρίζετε τη μετεγκατάσταση κλασική IaaS πόρων για τη διαχείριση πόρων, μπορείτε να ξεκινήσετε τη μετεγκατάσταση των πόρων.

- [Τεχνική βαθύ κατάρρευση στη μετεγκατάσταση πλατφόρμα που υποστηρίζονται από κλασική διαχείριση πόρων για να Azure](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Χρήση του PowerShell για μετεγκατάσταση IaaS πόρους από κλασική διαχείριση πόρων για να Azure](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [Χρήση CLI για τη μετεγκατάσταση IaaS πόρους από κλασική διαχείριση πόρων για να Azure](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Δημιουργία διπλότυπου μια εικονική μηχανή κλασική διαχείριση πόρων για να Azure χρησιμοποιώντας δέσμες ενεργειών PowerShell Κοινότητας](virtual-machines-windows-migration-scripts.md)