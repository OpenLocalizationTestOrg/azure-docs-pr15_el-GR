<properties
   pageTitle="Δημιουργία ακρόασης για την ομάδα availabilty AlwaysOn για SQL Server σε εικονικές μηχανές Windows Azure"
   description="Οδηγίες βήμα προς βήμα για τη δημιουργία μιας υπηρεσίας ακρόασης για μια ομάδα availabilty AlwaysOn για SQL Server σε εικονικές μηχανές Windows Azure"
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-an-internal-load-balancer-for-an-alwayson-availability-group-in-azure"></a>Ρύθμιση παραμέτρων μιας εσωτερικής εξισορρόπηση φόρτου για μια ομάδα διαθεσιμότητας AlwaysOn στο Azure

Αυτό το θέμα εξηγεί τον τρόπο για να δημιουργήσετε μια εσωτερική εξισορρόπηση φόρτου για μια ομάδα διαθεσιμότητας SQL Server AlwaysOn στο Azure εικονικές μηχανές εκτελούνται στο μοντέλο διαχείρισης πόρων. Μια ομάδα διαθεσιμότητας AlwaysOn απαιτεί μια μονάδα εξισορρόπησης φόρτου όταν υπάρχουν παρουσίες του SQL Server σε εικονικές μηχανές οι Azure. Τη μονάδα εξισορρόπησης φόρτου αποθηκεύει τη διεύθυνση IP για το ακροατήριο ομάδας διαθεσιμότητας. Εάν μια ομάδα διαθεσιμότητα εκτείνεται σε περιοχές πολλαπλών, κάθε περιοχής χρειάζεται μια μονάδα εξισορρόπησης φόρτου.

Για να ολοκληρώσετε αυτήν την εργασία, πρέπει να έχετε μια ομάδα διαθεσιμότητα SQL Server AlwaysOn αναπτυχθεί σε Azure εικονικές μηχανές στο μοντέλο διαχείρισης πόρων. Και οι δύο εικονικές μηχανές του SQL Server πρέπει να ανήκουν σε το ίδιο σύνολο διαθεσιμότητα. Μπορείτε να χρησιμοποιήσετε το [πρότυπο της Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) για να δημιουργήσετε αυτόματα το ομάδας διαθεσιμότητας AlwaysOn στη Διαχείριση Azure πόρων. Αυτό το πρότυπο δημιουργεί αυτόματα το εσωτερικό εξισορρόπηση φόρτου για εσάς. 

Εάν προτιμάτε, μπορείτε να [ρυθμίσετε με μη αυτόματο τρόπο μια ομάδας διαθεσιμότητας AlwaysOn](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Αυτό το θέμα απαιτεί ότι έχουν ήδη ρυθμιστεί τις ομάδες σας διαθεσιμότητας.  

Σχετικά θέματα περιλαμβάνουν τα εξής:

 - [Ρύθμιση παραμέτρων ομάδες διαθεσιμότητας AlwaysOn σε Azure Εικονική (Γραφικών)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
 
 - [Ρύθμιση παραμέτρων σύνδεσης VNet-VNet, χρησιμοποιώντας τη διαχείριση πόρων Azure και PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="steps"></a>Τα βήματα

Κατά την παρουσίαση μερικών μέσω αυτού του εγγράφου θα δημιουργήσετε και να ρυθμίσετε μια μονάδα εξισορρόπησης φόρτου στην πύλη του Azure. Αφού που έχει ολοκληρωθεί, θα μπορείτε να ρυθμίσετε τις παραμέτρους του συμπλέγματος για να χρησιμοποιήσετε τη διεύθυνση IP από τη μονάδα εξισορρόπησης φόρτου για το ακροατήριο AlwaysOn διαθεσιμότητα ομάδας.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Δημιουργία και ρύθμιση παραμέτρων τη μονάδα εξισορρόπησης φόρτου στην πύλη του Azure

Σε αυτό το τμήμα της εργασίας που θα κάνει τα ακόλουθα βήματα στην πύλη του Azure:

1. Δημιουργήστε την εξισορρόπηση φόρτου και ρυθμίσετε τις παραμέτρους της διεύθυνσης IP

1. Ρύθμιση παραμέτρων του χώρου συγκέντρωσης παρασκηνίου

1. Δημιουργήστε τη διερεύνηση 

1. Ορίστε το κανόνες εξισορρόπησης φόρτου

>[AZURE.NOTE] Εάν οι διακομιστές SQL είναι σε διαφορετικές ομάδες πόρων και περιοχές, θα το κάνετε όλα αυτά τα βήματα δύο φορές, μία φορά σε κάθε ομάδα πόρων.

## <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. Δημιουργία την εξισορρόπηση φόρτου και να ρυθμίσετε τις παραμέτρους της διεύθυνσης IP

Το πρώτο βήμα είναι να δημιουργήσετε τη μονάδα εξισορρόπησης φόρτου. Στην πύλη του Azure, ανοίξτε την ομάδα πόρων που περιέχει τις εικονικές μηχανές του SQL Server. Στην ομάδα πόρων, κάντε κλικ στην επιλογή **Προσθήκη**.

- Αναζήτηση για **μονάδα εξισορρόπησης φόρτου**. Από τα αποτελέσματα αναζήτησης, επιλέξτε **Εξισορρόπηση φόρτου**, οποίο δημοσιεύεται από τη **Microsoft**.

- Στην blade την **Εξισορρόπηση φόρτου** , κάντε κλικ στην επιλογή **Δημιουργία**.

- Στη **Δημιουργία μονάδα εξισορρόπησης φόρτου**, ρυθμίστε τις παραμέτρους του τη μονάδα εξισορρόπησης φόρτου ως εξής:

| Ρύθμιση | Τιμή |
| ----- | ----- |
| **Όνομα** | Ένα όνομα κειμένου που αντιπροσωπεύει τη μονάδα εξισορρόπησης φόρτου. Για παράδειγμα, **sqlLB**. |
| **Σχήμα** | **Εσωτερική** |
| **Εικονικό δίκτυο** | Επιλέξτε το εικονικό δίκτυο που τους διακομιστές SQL που βρίσκονται στο.   |
| **Υποδίκτυο**  | Επιλέξτε το υποδίκτυο που τους διακομιστές SQL που βρίσκονται στο. |
| **Συνδρομή** | Εάν έχετε πολλές συνδρομές, μπορεί να εμφανιστεί αυτό το πεδίο. Επιλέξτε τη συνδρομή στην οποία θέλετε να συσχετίσετε με αυτόν τον πόρο. Συνήθως είναι η ίδια συνδρομή ως όλους τους πόρους για την ομάδα διαθεσιμότητα.  |
| **Ομάδα πόρων** | Επιλέξτε την ομάδα πόρων που τους διακομιστές SQL που βρίσκονται στο. | 
| **Θέση** | Επιλέξτε τη θέση Azure που τους διακομιστές SQL που βρίσκονται στο. |

- Κάντε κλικ στην επιλογή **Δημιουργία**. 

Azure δημιουργεί τη μονάδα εξισορρόπησης φόρτου που ρυθμίσατε παραπάνω. Η μονάδα εξισορρόπησης φόρτου ανήκει σε ένα συγκεκριμένο δίκτυο, υποδίκτυο, ομάδα πόρων και θέση. Αφού ολοκληρωθεί η Azure, επαληθεύστε τις ρυθμίσεις εξισορρόπησης φόρτου στο Azure. 

Τώρα, ρυθμίστε τις παραμέτρους της διεύθυνσης IP εξισορρόπησης φόρτου.  

- Στην το blade **Ρυθμίσεις** εξισορρόπησης φόρτου, κάντε κλικ στην επιλογή **διεύθυνση IP**. Η **διεύθυνση IP** blade εμφανίζει ότι αυτό είναι ένα ιδιωτικό εξισορρόπηση φόρτου στο ίδιο δίκτυο εικονικού ως διακομιστές SQL. 

- Ορίστε τις ακόλουθες ρυθμίσεις: 

| Ρύθμιση | Τιμή |
| ----- | ----- |
| **Υποδίκτυο** | Επιλέξτε το υποδίκτυο που τους διακομιστές SQL που βρίσκονται στο. |
| **Ανάθεση** | **Στατική** |
| **Διεύθυνση IP** | Πληκτρολογήστε μια που δεν χρησιμοποιούνται εικονική διεύθυνση IP από το υποδίκτυο.  |

- Αποθήκευση των ρυθμίσεων.

Τώρα τη μονάδα εξισορρόπησης φόρτου έχει μια διεύθυνση IP. Εγγραφή αυτήν τη διεύθυνση IP. Θα χρησιμοποιήσετε αυτήν τη διεύθυνση IP κατά τη δημιουργία μιας υπηρεσίας ακρόασης στο σύμπλεγμα. Σε μια δέσμη ενεργειών PowerShell αργότερα σε αυτό το άρθρο, χρησιμοποιήστε αυτήν τη διεύθυνση για το `$ILBIP` μεταβλητή.



## <a name="2-configure-the-backend-pool"></a>2. ρύθμιση παραμέτρων του χώρου συγκέντρωσης παρασκηνίου

Το επόμενο βήμα είναι να δημιουργήσετε ένα χώρο συγκέντρωσης διεύθυνση παρασκηνίου. Azure καλεί το χώρο συγκέντρωσης διευθύνσεων παρασκηνίου *χώρου συγκέντρωσης παρασκηνίου*. Σε αυτήν την περίπτωση, το χώρο συγκέντρωσης υποστήριξης είναι οι διευθύνσεις στους δύο διακομιστές SQL της ομάδας σας διαθεσιμότητα. 

- Στην ομάδα σας πόρων, κάντε κλικ στη τη μονάδα εξισορρόπησης φόρτου που δημιουργήσατε. 

- Στις **Ρυθμίσεις**, κάντε κλικ στην επιλογή **σύνολα παρασκηνίου**.

- Σε **χώρους συγκέντρωσης διεύθυνση παρασκηνίου**, κάντε κλικ στην επιλογή **Προσθήκη** για να δημιουργήσετε ένα χώρο συγκέντρωσης διεύθυνση παρασκηνίου. 

- Στο **χώρο συγκέντρωσης παρασκηνίου Προσθήκη** στην περιοχή **όνομα**, πληκτρολογήστε ένα όνομα για το χώρο συγκέντρωσης παρασκηνίου.

- Στην περιοχή **εικονικές μηχανές** , κάντε κλικ στο κουμπί **+ Προσθήκη μια εικονική μηχανή**. 

- Στην περιοχή **επιλογή εικονικές μηχανές** κάντε κλικ στην επιλογή **Επιλέξτε ένα σύνολο διαθεσιμότητα** και καθορίστε τους διαθέσιμους ρύθμιση που ανήκει εικονικές μηχανές του SQL Server.

- Αφού έχετε επιλέξει το σύνολο διαθεσιμότητας, κάντε κλικ στην επιλογή **Επιλέξτε τις εικονικές μηχανές**. Επιλέξτε τις δύο εικονικές μηχανές που φιλοξενεί τις παρουσίες του SQL Server στην ομάδα διαθεσιμότητα. Κάντε κλικ στην **επιλογή**. 

- Κάντε κλικ στο **κουμπί OK** για να κλείσετε τις λεπίδες για **επιλογή εικονικές μηχανές**και **Προσθήκη χώρου συγκέντρωσης παρασκηνίου**. 

Azure ενημερώνει τις ρυθμίσεις για το χώρο συγκέντρωσης διευθύνσεων παρασκηνίου. Το σύνολο διαθεσιμότητα έχει τώρα ένα χώρο συγκέντρωσης των δύο διακομιστών SQL.

## <a name="3-create-a-probe"></a>3. Δημιουργήστε μια δοκιμή του

Το επόμενο βήμα είναι να δημιουργήσετε μια δοκιμή του. Η δοκιμή του καθορίζει τον τρόπο Azure θα επαληθεύσετε ποιος από τους διακομιστές SQL ανήκει αυτήν τη στιγμή το ακροατήριο ομάδας διαθεσιμότητας. Azure θα Διερεύνηση την υπηρεσία που βασίζεται σε IP address στη θύρα που ορίζετε όταν δημιουργείτε τη διερεύνηση.

- Στην το blade **Ρυθμίσεις** εξισορρόπησης φόρτου, κάντε κλικ στην επιλογή **Probes**. 

- Στην **Probes** blade, κάντε κλικ στην επιλογή **Προσθήκη**.

- Ρύθμιση παραμέτρων τη δοκιμή του στο τη **Δοκιμή του Προσθήκη** blade. Χρησιμοποιήστε τις παρακάτω τιμές για να ρυθμίσετε τη δοκιμή του:

| Ρύθμιση | Τιμή |
| ----- | ----- |
| **Όνομα** | Ένα όνομα κειμένου που αντιπροσωπεύει τη διερεύνηση. Για παράδειγμα, **SQLAlwaysOnEndPointProbe**. |
| **Πρωτόκολλο** | **TCP** |
| **Θύρα** | Μπορείτε να χρησιμοποιήσετε οποιαδήποτε διαθέσιμη θύρας. Για παράδειγμα, *59999*.    |
| **Χρονικό διάστημα**  | *5* | 
| **Κατεστραμμένες όριο**  | *2* | 

- Κάντε κλικ στο **κουμπί OK**. 

>[AZURE.NOTE] Βεβαιωθείτε ότι είναι ανοιχτό το τείχος προστασίας των δύο διακομιστών SQL τη θύρα που καθορίζετε. Οι διακομιστές και οι δύο απαιτούν έναν κανόνα εισερχομένων για τη θύρα TCP που χρησιμοποιείτε. Ανατρέξτε στο θέμα [Προσθήκη ή επεξεργασία κανόνα τείχους προστασίας](http://technet.microsoft.com/library/cc753558.aspx) για περισσότερες πληροφορίες. 

Azure δημιουργεί τη διερεύνηση. Azure θα χρησιμοποιήσει τη δοκιμή του για να ελέγξετε ποια SQL Server περιλαμβάνει την υπηρεσία ακρόασης για την ομάδα διαθεσιμότητα.

## <a name="4-set-the-load-balancing-rules"></a>4. Ορίστε το κανόνες εξισορρόπησης φόρτου

Ορίστε το κανόνες εξισορρόπησης φόρτου. Οι κανόνες εξισορρόπησης φόρτου ρύθμιση παραμέτρων πώς τη μονάδα εξισορρόπησης φόρτου δρομολογεί την κίνηση προς τους διακομιστές SQL. Για αυτό εξισορρόπηση φόρτου θα ενεργοποιείτε απευθείας διακομιστή επιστροφής επειδή μόνο ένας από τους δύο διακομιστές SQL ποτέ θα ανήκει ο πόρος ακρόασης ομάδα διαθεσιμότητα κάθε φορά.

- Στην το blade **Ρυθμίσεις** εξισορρόπησης φόρτου, κάντε κλικ στην επιλογή **Φόρτωση εξισορρόπησης κανόνων**. 

- Στην blade **εξισορρόπηση φόρτου κανόνες** , κάντε κλικ στην επιλογή **Προσθήκη**.

- Χρησιμοποιήστε το blade **Προσθήκη φόρτωση εξισορρόπησης κανόνες** για να ρυθμίσετε το κανόνα εξισορρόπησης φόρτου. Χρησιμοποιήστε τις παρακάτω ρυθμίσεις: 

| Ρύθμιση | Τιμή |
| ----- | ----- |
| **Όνομα** | Ένα όνομα κειμένου που αντιπροσωπεύει το κανόνες εξισορρόπησης φόρτου. Για παράδειγμα, **SQLAlwaysOnEndPointListener**. |
| **Πρωτόκολλο** | **TCP** |
| **Θύρα** | *1433*   |
| **Θύρα παρασκηνίου** | *1433*. Σημειώστε ότι αυτό θα απενεργοποιηθεί επειδή αυτός ο κανόνας χρησιμοποιεί **Αιωρούμενα IP (επιστροφής απευθείας server)**.   |
| **Δοκιμή του** | Χρησιμοποιήστε το όνομα του τη δοκιμή του που δημιουργήσατε για αυτό εξισορρόπηση φόρτου. |
| **Persistance περιόδου λειτουργίας**  | **Κανένας** | 
| **Το χρονικό όριο αδράνειας (λεπτά)**  | *4* | 
| **Αιωρούμενα IP (επιστροφής απευθείας server)**  | **Με δυνατότητα** | 

 >[AZURE.NOTE] Ίσως χρειαστεί να κάνετε κύλιση προς τα κάτω στην το blade για να δείτε όλες τις ρυθμίσεις.

- Κάντε κλικ στο **κουμπί OK**. 

- Azure ρυθμίζει το κανόνα εξισορρόπησης φόρτου. Τώρα τη μονάδα εξισορρόπησης φόρτου έχει ρυθμιστεί για την κυκλοφορία δρομολόγηση με τον SQL Server που φιλοξενεί το ακροατήριο για την ομάδα διαθεσιμότητα. 

Σε αυτό το σημείο την ομάδα των πόρων έχει μια μονάδα εξισορρόπησης φόρτου, τη σύνδεση σε δύο υπολογιστές SQL Server. Τη μονάδα εξισορρόπησης φόρτου περιέχει επίσης μια διεύθυνση IP για το ακροατήριο ομάδας διαθεσιμότητας SQL Server AlwaysOn, έτσι ώστε κάθε υπολογιστή μπορούν να απαντούν αιτήσεις για τις ομάδες διαθεσιμότητας.

>[AZURE.NOTE] Εάν οι διακομιστές SQL είναι σε δύο ξεχωριστές περιοχές, επαναλάβετε τα βήματα στην περιοχή άλλες. Κάθε περιοχή απαιτεί μια μονάδα εξισορρόπησης φόρτου. 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Ρύθμιση παραμέτρων του συμπλέγματος για να χρησιμοποιήσετε τη διεύθυνση IP εξισορρόπησης φόρτου 

Το επόμενο βήμα είναι να ρυθμίσετε τις παραμέτρους της ακρόασης στο σύμπλεγμα και μεταφέρετε το ακροατήριο online. Για να γίνει αυτό, κάντε τα εξής: 

1. Δημιουργήστε το ακροατήριο ομάδας διαθεσιμότητας στο σύμπλεγμα ανακατεύθυνσης 

1. Μεταφέρετε το ακροατήριο online

## <a name="1-create-the-availablity-group-listener-on-the-failover-cluster"></a>1. Δημιουργήστε το ακροατήριο ομάδας διαθεσιμότητας στο σύμπλεγμα ανακατεύθυνσης

Σε αυτό το βήμα, δημιουργείτε με μη αυτόματο τρόπο το ακροατήριο ομάδα διαθεσιμότητα στη Διαχείριση ανακατεύθυνσης συμπλεγμάτων και SQL Server Management Studio (SSMS).

- Χρησιμοποιήστε RDP για να συνδεθείτε με το Azure εικονική μηχανή που φιλοξενεί την κύρια ρεπλίκα. 

- Ανοίξτε τη Διαχείριση συμπλέγματος ανακατεύθυνσης.

- Επιλέξτε τον κόμβο **δίκτυα** και σημειώστε το όνομα του δικτύου συμπλέγματος. Αυτό το όνομα θα χρησιμοποιηθεί με το `$ClusterNetworkName` μεταβλητής στη δέσμη ενεργειών PowerShell.

- Αναπτύξτε το όνομα του συμπλέγματος και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρόλοι**.

- Στο παράθυρο **ρόλων** , κάντε δεξί κλικ στο όνομα της ομάδας διαθεσιμότητας και, στη συνέχεια, επιλέξτε **Προσθήκη πόρων** > **Σημείο πρόσβασης υπολογιστή-πελάτη**.

- Στο πλαίσιο **όνομα** , δημιουργήστε ένα όνομα για το νέο πρόγραμμα ακρόασης, στη συνέχεια, κάντε κλικ δύο φορές στο κουμπί **Επόμενο** και, στη συνέχεια, κάντε κλικ στο κουμπί **Τέλος**. Μην επαναφέρετε το ακροατήριο ή ο πόρος online σε αυτό το σημείο.

 >[AZURE.NOTE] Το όνομα για το νέο πρόγραμμα ακρόασης είναι το όνομα του δικτύου που θα χρησιμοποιούν οι εφαρμογές για να συνδεθείτε με βάσεις δεδομένων στην ομάδα διαθεσιμότητα SQL Server.

- Κάντε κλικ στην καρτέλα **πόροι** και, στη συνέχεια, αναπτύξτε το σημείο πρόσβασης υπολογιστή-πελάτη που μόλις δημιουργήσατε. Κάντε δεξί κλικ στον πόρο IP και κάντε κλικ στην επιλογή Ιδιότητες. Σημειώστε το όνομα της διεύθυνσης IP. Θα χρησιμοποιήσετε αυτό το όνομα στο το `$IPResourceName` μεταβλητής στη δέσμη ενεργειών PowerShell.

- Στην περιοχή **Διεύθυνση IP** , κάντε κλικ στην επιλογή **Στατική διεύθυνση IP** και ορίστε τη στατική διεύθυνση IP στην ίδια διεύθυνση που χρησιμοποιήσατε κατά τη ρύθμιση της διεύθυνσης IP εξισορρόπησης φόρτου στην πύλη του Azure. Ενεργοποίηση του NetBIOS για αυτήν τη διεύθυνση και κάντε κλικ στο **κουμπί OK**. Επαναλάβετε αυτό το βήμα για κάθε πόρο IP, εάν η λύση εκτείνεται σε πολλές VNets Azure. 

- Στον κόμβο συμπλέγματος που φιλοξενεί αυτήν τη στιγμή η κύρια ρεπλίκα, ανοίξτε μια αναβαθμισμένη PowerShell ISE και επικολλήστε τις παρακάτω εντολές σε μια νέα δέσμη ενεργειών.

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Ενημερώστε τις μεταβλητές και εκτελέστε τη δέσμη ενεργειών του PowerShell για να ρυθμίσετε τις παραμέτρους της διεύθυνσης IP και θύρες για το νέο πρόγραμμα ακρόασης.

 >[AZURE.NOTE] Εάν οι διακομιστές SQL σας είναι σε ξεχωριστές περιοχές, πρέπει να εκτελέσετε τη δέσμη ενεργειών PowerShell δύο φορές. Την πρώτη φορά που χρησιμοποιούν το όνομα δικτύου σύμπλεγμα, όνομα πόρου συμπλέγματος IP, και φορτώστε διεύθυνση IP εξισορρόπησης από την πρώτη ομάδα πόρων. Τη δεύτερη φορά που χρησιμοποιούν το όνομα δικτύου σύμπλεγμα, όνομα πόρου συμπλέγματος IP, και φορτώστε διεύθυνση IP εξισορρόπησης από τη δεύτερη ομάδα πόρων.

Τώρα το σύμπλεγμα διαθέτει έναν πόρο διαθεσιμότητα ομάδα ακρόασης.

## <a name="2-bring-the-listener-online"></a>2. μεταφέρετε το ακροατήριο online

Με τη διαθεσιμότητα ομάδα ακρόασης πόρο έχει ρυθμιστεί, μπορείτε να μεταφέρετε το ακροατήριο online ώστε να μπορούν να συνδεθούν εφαρμογές σε βάσεις δεδομένων με την ομάδα διαθεσιμότητας με την παρακολούθηση.

- Μεταβείτε ξανά για Διαχείριση ανακατεύθυνσης συμπλεγμάτων. Αναπτύξτε **τους ρόλους** και, στη συνέχεια, επισημάνετε τη διαθεσιμότητα ομάδα. Στην καρτέλα **πόροι** , κάντε δεξί κλικ στο όνομα ακρόασης και κάντε κλικ στην επιλογή **Ιδιότητες**.

- Κάντε κλικ στην καρτέλα **εξαρτήσεις** . Εάν υπάρχουν πολλά πόρων που εμφανίζονται, επιβεβαιώστε ότι οι διευθύνσεις IP έχουν ή όχι AND, εξαρτήσεις. Κάντε κλικ στο **κουμπί OK**.

- Κάντε δεξί κλικ στο όνομα ακρόασης και κάντε κλικ στην επιλογή **Σύνδεση**.


- Όταν το ακροατήριο βρίσκεται στο Internet, στην καρτέλα **πόροι** , κάντε δεξί κλικ στην ομάδα διαθεσιμότητα και κάντε κλικ στην επιλογή **Ιδιότητες**.

- Δημιουργία εξάρτησης στον πόρο όνομα ακρόασης (όχι η IP διεύθυνση πόρους όνομα). Κάντε κλικ στο **κουμπί OK**.


- Εκκίνηση του SQL Server Management Studio και να συνδεθείτε με την κύρια ρεπλίκα.


- Μεταβείτε στη **διαθεσιμότητα AlwaysOn υψηλής** | **ομάδες διαθεσιμότητας** | **ακροατών ομάδας διαθεσιμότητας**. 


- Τώρα θα πρέπει να βλέπετε το όνομα ακρόασης που δημιουργήσατε στη Διαχείριση ανακατεύθυνσης συμπλεγμάτων. Κάντε δεξί κλικ στο όνομα ακρόασης και κάντε κλικ στην επιλογή **Ιδιότητες**.


- Στο πλαίσιο **θύρα** , καθορίστε τον αριθμό θύρας για την παρακολούθηση ομάδα διαθεσιμότητα, χρησιμοποιώντας το $EndpointPort που χρησιμοποιήσατε νωρίτερα (1433 ήταν η προεπιλογή), στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

Τώρα έχετε μια ομάδα διαθεσιμότητας SQL Server AlwaysOn στο Azure εικονικές μηχανές εκτέλεση σε κατάσταση λειτουργίας της διαχείρισης πόρων. 

## <a name="test-the-connection-to-the-listener"></a>Ελέγξτε τη σύνδεση για το ακροατήριο

Για να ελέγξετε τη σύνδεση:

1. RDP για SQL Server που είναι στο ίδιο δίκτυο εικονικού, αλλά δεν ανήκει στη ρεπλίκα. Αυτό μπορεί να τα άλλα SQL Server στο σύμπλεγμα.

1. Χρησιμοποιήστε πρόγραμμα **sqlcmd** για να ελέγξετε τη σύνδεση. Για παράδειγμα, η ακόλουθη δέσμη ενεργειών δημιουργεί μια σύνδεση **sqlcmd** στην κύρια ρεπλίκα έως το ακροατήριο με έλεγχο ταυτότητας των Windows:

        sqlcmd -S <listenerName> -E

Η σύνδεση SQLCMD συνδέεται αυτόματα στο πρωτεύον αντίγραφο φιλοξενεί όποια παρουσία του SQL Server. 

## <a name="guidelines-and-limitations"></a>Γενικές οδηγίες και περιορισμοί

Λάβετε υπόψη τις ακόλουθες οδηγίες σε ακρόασης ομάδας διαθεσιμότητας στο Azure χρησιμοποιώντας εσωτερική μονάδα εξισορρόπησης φόρτου:

- Μόνο μία ακρόασης ομάδα εσωτερικό διαθεσιμότητας υποστηρίζεται ανά υπηρεσία cloud, επειδή το ακροατήριο έχει ρυθμιστεί ώστε να την εξισορρόπηση φόρτου και δεν υπάρχει μόνο ένα εσωτερικό εξισορρόπηση φόρτου. Ωστόσο είναι δυνατή η δημιουργία διπλότυπη εξωτερικών ακροατών. 

- Με μια εσωτερική έχετε πρόσβαση μόνο το ακροατήριο από μέσα στο ίδιο δίκτυο εικονικού μονάδα εξισορρόπησης φόρτου.
 