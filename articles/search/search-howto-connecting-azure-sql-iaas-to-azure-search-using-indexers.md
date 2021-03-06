<properties 
    pageTitle="Ρύθμιση παραμέτρων σύνδεσης από το ευρετήριο αναζήτησης Azure SQL Server σε μια εικονική μηχανή Azure | Microsoft Azure | Οι δεικτοδότες" 
    description="Ενεργοποίηση κρυπτογραφημένες συνδέσεις και ρύθμιση παραμέτρων του τείχους προστασίας για να επιτρέπονται οι συνδέσεις με τον SQL Server σε μια εικονική μηχανή Azure (Εικονική) από ένα ευρετήριο σε Azure αναζήτησης." 
    services="search" 
    documentationCenter="" 
    authors="jack4it" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/26/2016" 
    ms.author="jackma"/>

# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Ρύθμιση παραμέτρων σύνδεσης από το ευρετήριο αναζήτησης Azure SQL Server σε μια Εικονική Azure

Όπως επισημαίνεται στο [Τη σύνδεση βάσης δεδομένων SQL Azure για αναζήτηση Azure χρησιμοποιώντας Οι δεικτοδότες](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions), δημιουργία Οι δεικτοδότες σε σχέση με **SQL Server σε VM Azure** (ή **SQL Azure ΣΠΣ** για συντομία) υποστηρίζεται από την αναζήτηση Azure, αλλά υπάρχουν μερικά προϋποθέσεις που σχετίζονται με την ασφάλεια για να αναλάβουν την πρώτη. 

**Διάρκειας εργασίας:** 30 λεπτά περίπου, υπό την προϋπόθεση ότι έχετε ήδη εγκατεστημένο ένα πιστοποιητικό στον η Εικονική.

## <a name="enable-encrypted-connections"></a>Ενεργοποίηση κρυπτογραφημένες συνδέσεις

Αναζήτηση Azure απαιτεί ένα κρυπτογραφημένο κανάλι για όλες τις αιτήσεις ευρετηρίου δημόσια σύνδεση στο internet. Αυτή η ενότητα παραθέτει τα βήματα για να κάνετε αυτή την εργασία.

1. Ελέγξτε τις ιδιότητες του πιστοποιητικού για να επιβεβαιώσετε το πλήρως προσδιορισμένο όνομα τομέα (FQDN) του η Εικονική Azure είναι το όνομα του θέματος. Μπορείτε να χρησιμοποιήσετε ένα εργαλείο όπως CertUtils ή του συμπληρωματικού προγράμματος πιστοποιητικών για να προβάλετε τις ιδιότητες. Μπορείτε να λάβετε το FQDN από την Εικονική blade υπηρεσίας βασικά στοιχεία για την ενότητα, στο πεδίο **διεύθυνση IP δημόσια/DNS όνομα ετικέτας** , στην [πύλη του Azure](https://portal.azure.com/).

    - Για ΣΠΣ που δημιουργούνται με χρήση του προτύπου νεότερη **Διαχείριση πόρων** , το FQDN μορφοποιείται ως `<your-VM-name>.<region>.cloudapp.azure.com`. 

    - Για παλαιότερες ΣΠΣ δημιουργείται ως μια **κλασική** Εικονική, το FQDN μορφοποιείται ως `<your-cloud-service-name.cloudapp.net>`. 

2. Ρύθμιση παραμέτρων του SQL Server για να χρησιμοποιήσετε το πιστοποιητικό χρησιμοποιώντας τον Επεξεργαστή μητρώου (regedit). 

    Παρόλο που το SQL Server Configuration Manager χρησιμοποιείται συχνά για αυτήν την εργασία, δεν μπορείτε να το χρησιμοποιήσετε για αυτό το σενάριο. Αυτό θα βρείτε το πιστοποιητικό που έχει εισαχθεί, επειδή το FQDN του η Εικονική στην Azure δεν συμφωνεί με το FQDN όπως καθορίζεται από την εικονική Μηχανή (προσδιορίζει τον τομέα ως στον τοπικό υπολογιστή ή τον τομέα του δικτύου στο οποίο είναι συνδεδεμένος). Όταν δεν συμφωνούν με τα ονόματα, χρησιμοποιήστε regedit για να καθορίσετε το πιστοποιητικό.

    - Στο regedit, μεταβείτε στο ακόλουθο κλειδί μητρώου: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
    Το `[MSSQL13.MSSQLSERVER]` τμήμα ποικίλλει, ανάλογα με την έκδοση και την παρουσία όνομα. 

    - Ορίστε την τιμή του αριθμού-κλειδιού **πιστοποιητικό** για την **αποτύπωση** του πιστοποιητικού SSL που εισαγάγατε την εικονική Μηχανή.

    Υπάρχουν πολλοί τρόποι για να λάβετε την αποτύπωση, κάποια καλύτερα από άλλους χρήστες. Εάν αντιγράφετε από το **πιστοποιητικά** Συγκράτηση στο MMC, θα σηκώστε πιθανώς ένα μη ορατά στην αρχή χαρακτήρα [που περιγράφεται σε αυτό το άρθρο υποστήριξης](https://support.microsoft.com/kb/2023869/), με αποτέλεσμα σφάλμα κατά την προσπάθεια σύνδεσης. Υπάρχουν διάφορες λύσεις για τη διόρθωση αυτό το πρόβλημα. Πιο εύκολο είναι να backspace πάνω από και, στη συνέχεια, πληκτρολογήστε ξανά τον πρώτο χαρακτήρα της την αποτύπωση για να καταργήσετε το αρχικό χαρακτήρα στο πεδίο τιμής κλειδιού regedit. Εναλλακτικά, μπορείτε να χρησιμοποιήσετε ένα διαφορετικό εργαλείο για να αντιγράψετε την αποτύπωση.

3. Εκχώρηση δικαιωμάτων για το λογαριασμό της υπηρεσίας. 

    Βεβαιωθείτε ότι ο λογαριασμός υπηρεσίας SQL Server παρέχεται κατάλληλα δικαιώματα στο ιδιωτικό κλειδί του πιστοποιητικού SSL. Εάν προσέξετε αυτό το βήμα, SQL Server δεν θα ξεκινήσει. Μπορείτε να χρησιμοποιήσετε το συμπληρωματικού **πιστοποιητικών** ή **CertUtils** για αυτήν την εργασία.

4. Επανεκκινήστε την υπηρεσία SQL Server.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>Ρύθμιση παραμέτρων σύνδεσης του SQL Server σε η Εικονική

Αφού ρυθμίσετε το κρυπτογραφημένη σύνδεση απαιτείται από το Azure αναζήτησης, υπάρχουν επιπλέον βήματα ρύθμισης παραμέτρων εγγενή με τον SQL Server σε VM Azure. Εάν δεν το έχετε κάνει ήδη, το επόμενο βήμα είναι να ολοκληρώσετε τη ρύθμιση παραμέτρων χρησιμοποιώντας οποιαδήποτε από αυτά τα άρθρα:

- Για ένα **Διαχειριστή πόρων** Εικονική, ανατρέξτε στο θέμα [σύνδεση σε ένα SQL Server εικονικό μηχάνημα σε Azure χρησιμοποιώντας τη διαχείριση πόρων](../virtual-machines/virtual-machines-windows-sql-connect.md). 

- Για μια **κλασική** Εικονική, ανατρέξτε στο θέμα [σύνδεση σε ένα SQL Server εικονικό μηχάνημα σε κλασική Azure](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

Συγκεκριμένα, εξετάστε την ενότητα σε κάθε άρθρο για "τη σύνδεση μέσω του internet".

## <a name="configure-the-network-security-group-nsg"></a>Ρύθμιση παραμέτρων της ομάδας ασφαλείας δικτύου (NSG)

Δεν είναι ασυνήθιστο για να ρυθμίσετε το NSG και αντίστοιχες Azure τελικού σημείου ή λίστα ελέγχου πρόσβασης (ACL) για να κάνετε το Azure Εικονική προσβάσιμα σε τρίτους. Το πιθανότερο είναι μπορείτε να το κάνει αυτό πριν να επιτρέψετε τη δική σας λογική εφαρμογής για να συνδεθείτε με το Εικονική Azure SQL. Είναι δεν διαφέρει για μια σύνδεση Azure αναζήτησης για να σας Εικονική Azure SQL. 

Οι παρακάτω συνδέσεις παρέχουν οδηγίες NSG ρύθμιση των παραμέτρων για αναπτύξεις Εικονική. Χρησιμοποιήστε αυτές τις οδηγίες για να ACL τελικού σημείου Azure αναζήτησης με βάση τη διεύθυνση IP.

> [AZURE.NOTE] Για το φόντο, ανατρέξτε στο θέμα [Τι είναι μια ομάδα ασφαλείας δικτύου;](../virtual-network/virtual-networks-nsg.md)

- Για ένα **Διαχειριστή πόρων** Εικονική, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε NSGs για αναπτύξεις ARM](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

- Για μια **κλασική** Εικονική, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε NSGs για αναπτύξεις κλασική](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

Εκχώρηση διευθύνσεων IP ενδέχεται να ενέχουν μερικά προκλήσεις που είναι εύκολα υπερβούν Εάν γνωρίζετε το ζήτημα και πιθανές λύσεις. Οι υπόλοιπες ενότητες παρέχουν συστάσεις για το χειρισμό θέματα που σχετίζονται με διευθύνσεις IP του ACL.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Περιορισμός πρόσβασης της διεύθυνσης IP της υπηρεσίας αναζήτησης

Συνιστάται να περιορίσετε την πρόσβαση στη διεύθυνση IP σας υπηρεσίας αναζήτησης σε ένα ACL αντί να κάνετε το ΣΠΣ Azure SQL ανοιχτό σε οποιαδήποτε αιτήσεις σύνδεσης. Μπορείτε εύκολα να μάθετε τη διεύθυνση IP κάνοντας ping το FQDN (για παράδειγμα, `<your-search-service-name>.search.windows.net`) από την υπηρεσία αναζήτησης.

#### <a name="managing-ip-address-fluctuations"></a>Διαχείριση των διακυμάνσεων διεύθυνση IP

Εάν η υπηρεσία αναζήτησης έχει μόνο μία μονάδα αναζήτησης (δηλαδή, μία ρεπλίκα και ένα διαμερίσματα), η διεύθυνση IP θα αλλάξει κατά τη διάρκεια επανεκκίνηση συντήρηση ρουτίνας, ακύρωση ενός υπάρχοντος ACL με τη διεύθυνση IP σας της υπηρεσίας αναζήτησης.

Ένας τρόπος για να αποφύγετε το σφάλμα οι επόμενες συνδεσιμότητας είναι να χρησιμοποιήσετε περισσότερες από μία ρεπλίκα και ένα διαμερίσματα στο πλαίσιο Αναζήτηση Azure. Αυτή η ενέργεια αυξάνει το κόστος, αλλά το επίσης επιλύει το πρόβλημα διεύθυνση IP. Στο πλαίσιο Αναζήτηση Azure, οι διευθύνσεις IP δεν αλλάζουν όταν έχετε περισσότερες από μία μονάδες αναζήτησης.

Μια δεύτερη προσέγγιση είναι να επιτρέπεται η σύνδεση να αποτύχει και, στη συνέχεια, ρυθμίστε ξανά τις παραμέτρους των ACL στο το NSG. Κατά μέσο όρο, μπορείτε να περιμένετε διευθύνσεις IP για να αλλάξετε κάθε λίγες εβδομάδες. Για πελάτες που κάνουν ελέγχεται η δημιουργία ευρετηρίου σε σπάνια βάση, αυτή η προσέγγιση μπορεί να είναι βιώσιμος.

Είναι μια τρίτη προσέγγιση βιώσιμων (αλλά όχι ιδιαίτερα να διασφαλίζουν) για να καθορίσετε τη διεύθυνση IP της περιοχής Azure πού παρέχεται υπηρεσία αναζήτησης. Τη λίστα με τις περιοχές IP από την οποία δημόσιες διευθύνσεις IP έχουν εκχωρηθεί σε Azure πόροι είναι Δημοσιευμένος στην [περιοχές διευθύνσεων IP του κέντρου δεδομένων του Azure](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Συμπεριλάβετε στις διευθύνσεις IP πύλης Azure αναζήτησης

Εάν χρησιμοποιείτε την πύλη του Azure για να δημιουργήσετε ένα ευρετήριο, αναζήτηση Azure πύλης λογικής πρέπει επίσης πρόσβαση σας Εικονική Azure SQL κατά τη διάρκεια της ώρας δημιουργίας. Διευθύνσεις IP πύλης Azure αναζήτησης μπορείτε να βρείτε κάνοντας ping `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Επόμενα βήματα

Με τη ρύθμιση παραμέτρων εκτός της περιοχής, μπορείτε τώρα να καθορίσετε SQL Server στην Εικονική Azure ως προέλευση δεδομένων για το ευρετήριο αναζήτησης Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Σύνδεση βάσης δεδομένων SQL Azure για αναζήτηση Azure χρησιμοποιώντας Οι δεικτοδότες](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) .
