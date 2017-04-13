<properties
    pageTitle="Επισκόπηση του SQL Server σε εικονικές μηχανές οι Azure | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με τον τρόπο εκτέλεσης πλήρους εκδόσεις του SQL Server σε εικονικές Azure μηχανές. Λάβετε συνδέσεις απευθείας σε όλες τις εικόνες Εικονική SQL Server και σχετικό περιεχόμενο."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/19/2016"
    ms.author="jroth"/>

# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Επισκόπηση του SQL Server σε εικονικές μηχανές οι Azure

Αυτό το θέμα περιγράφει τις επιλογές για εκτελεί τον SQL Server στο Azure εικονικές μηχανές (ΣΠΣ), μαζί με [συνδέσεις προς πύλης εικόνες](#option-1-create-a-sql-vm-with-per-minute-licensing) και μια επισκόπηση των [κοινών εργασιών](#manage-your-sql-vm).

>[AZURE.NOTE] Εάν είστε ήδη εξοικειωμένοι με τον SQL Server και απλώς θέλετε να δείτε πώς μπορείτε να αναπτύξετε μια Εικονική SQL Server, ανατρέξτε στο θέμα [παροχή μια εικονική μηχανή SQL Server στην πύλη του Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Επισκόπηση
Εάν είστε διαχειριστής μιας βάσης δεδομένων ή προγραμματιστής, ΣΠΣ Azure παρέχει κάποιο τρόπο για να μετακινήσετε τα φόρτους εργασίας του SQL Server εσωτερικής εγκατάστασης και τις εφαρμογές στο cloud. Το βίντεο που ακολουθεί παρέχει μια τεχνική επισκόπηση των ΣΠΣ Azure SQL Server.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

Το βίντεο καλύπτει τα ακόλουθα θέματα:

|Ώρα|Περιοχή|
|---|---|
| 00:21 | Τι είναι οι ΣΠΣ Azure; |
| 01:45 | Ασφάλεια |
| 02:50 | Συνδεσιμότητα |
| 03:30 | Χώρος αποθήκευσης αξιοπιστίας και επιδόσεων |
| 05:20 | Εικονική μεγέθη |
| 05:54 | SLA και υψηλή διαθεσιμότητα |
| 07:30 | Ρύθμιση παραμέτρων υποστήριξης |
| 08:00 | Παρακολούθηση |
| 08:32 | Επίδειξη: Δημιουργία μια Εικονική SQL Server 2016 |

>[AZURE.NOTE] Το βίντεο εστιάζει σε SQL Server 2016, αλλά Azure παρέχει Εικονική εικόνες για πολλές εκδόσεις του SQL Server, συμπεριλαμβανομένων των 2008, 2012, 2014 και 2016. 

## <a name="scenarios"></a>Σενάρια
Υπάρχουν πολλοί λόγοι που ενδέχεται να μπορείτε να επιλέξετε για τη φιλοξενία των δεδομένων σας στο Azure. Εάν η εφαρμογή σας Μετακίνηση στο Azure, το βελτιώνει τις επιδόσεις για να μετακινήσετε επίσης τα δεδομένα. Αλλά υπάρχουν και άλλα πλεονεκτήματα. Έχετε αυτόματα πρόσβαση σε πολλά κέντρα δεδομένων για καθολική παρουσίας και καταστροφή αποκατάστασης. Τα δεδομένα είναι επίσης ιδιαίτερα ασφαλή και διαρκή.

SQL Server που εκτελείται σε VM Azure είναι μία επιλογή για την αποθήκευση των σχεσιακών δεδομένων σε Azure. Είναι καλή επιλογή για διάφορα σενάρια. Για παράδειγμα, μπορεί να θέλετε να ρυθμίσετε τις παραμέτρους του Azure Εικονική ομοίως όσο το δυνατόν σε υπολογιστή SQL Server εσωτερικής εγκατάστασης. Ή μπορεί να θέλετε να εκτελέσετε πρόσθετες εφαρμογές και υπηρεσίες στον ίδιο διακομιστή βάσης δεδομένων. Υπάρχουν δύο κύρια πόροι που μπορεί να σας βοηθήσει να πιστεύετε ότι μέσω ακόμα περισσότερα σενάρια και τα θέματα:

 - [SQL Server σε εικονικές μηχανές οι Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/) παρέχει μια επισκόπηση βέλτιστη σενάρια για τη χρήση του SQL Server σε VM Azure. 
 - [Επιλέξτε μια επιλογή SQL Server στο cloud: βάση δεδομένων SQL Azure (PaaS) ή του SQL Server σε Azure VM (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) παρέχει μια αναλυτική σύγκριση μεταξύ της βάσης δεδομένων SQL και του SQL Server που εκτελείται σε μια Εικονική.

## <a name="create-a-new-sql-vm"></a>Δημιουργήστε μια νέα Εικονική SQL
Οι παρακάτω ενότητες παρέχουν απευθείας συνδέσεις στην πύλη του Azure για τις εικόνες συλλογή εικονική μηχανή SQL Server. Ανάλογα με την εικόνα που επιλέγετε, μπορείτε να κάνετε είτε πληρωμή για τον SQL Server παραχώρησης πολλαπλών αδειών χρήσης κόστους σε βάση ανά λεπτό ή μπορείτε να μεταφέρετε το δικό σας άδεια χρήσης (BYOL).

Βρείτε οδηγίες βήμα προς βήμα για αυτήν τη διαδικασία σε το πρόγραμμα εκμάθησης, [παροχή μια εικονική μηχανή SQL Server στην πύλη του Azure](virtual-machines-windows-portal-sql-server-provision.md). Επίσης, αναθεωρήστε τις [επιδόσεις βέλτιστες πρακτικές για SQL Server ΣΠΣ](virtual-machines-windows-sql-performance.md), που εξηγεί τον τρόπο για να επιλέξετε το μέγεθος κατάλληλο υπολογιστή και άλλες δυνατότητες που είναι διαθέσιμες κατά την προμήθεια του.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Επιλογή 1: Δημιουργήστε μια Εικονική SQL με άδεια χρήσης ανά λεπτό
Ο παρακάτω πίνακας παρέχει μια μήτρα διαθέσιμες εικόνες SQL Server στη συλλογή εικονική μηχανή. Κάντε κλικ σε οποιαδήποτε σύνδεση για να ξεκινήσετε τη δημιουργία ενός νέου Εικονική SQL με την καθορισμένη έκδοση edition και το λειτουργικό σύστημα.

|Έκδοση|Λειτουργικό σύστημα|Edition|
|---|---|---|
|**SQL 2016**|Windows Server 2012 R2|[Για μεγάλες επιχειρήσεις](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [τυπικό](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [αποκλίσεις](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL 2014 SP1**|Windows Server 2012 R2|[Για μεγάλες επιχειρήσεις](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [τυπικό](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014**|Windows Server 2012 R2|[Για μεγάλες επιχειρήσεις](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [τυπικό](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Για μεγάλες επιχειρήσεις](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [τυπικό](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Για μεγάλες επιχειρήσεις](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [τυπικό](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Για μεγάλες επιχειρήσεις](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [τυπικό](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Για μεγάλες επιχειρήσεις](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [τυπικό](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>Επιλογή 2: Δημιουργήστε μια Εικονική SQL με μια άδεια χρήσης του υπάρχοντος
Μπορείτε επίσης να μεταφέρετε το δικό σας άδεια χρήσης (BYOL). Σε αυτό το σενάριο, πληρώνετε μόνο για την εικονική Μηχανή χωρίς τυχόν πρόσθετες χρεώσεις για την άδεια χρήσης του SQL Server. Για να χρησιμοποιήσετε τη δική σας άδεια χρήσης, χρησιμοποιήστε τον πίνακα εκδόσεις SQL Server, εκδόσεις και λειτουργικά συστήματα παρακάτω. Στην πύλη, αυτά τα ονόματα εικόνων είναι με το πρόθεμα **{BYOL}**.

|Έκδοση|Λειτουργικό σύστημα|Edition|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Για μεγάλες επιχειρήσεις BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Τυπική BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Για μεγάλες επιχειρήσεις BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Τυπική BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Για μεγάλες επιχειρήσεις BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Τυπική BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] Για να χρησιμοποιήσετε εικόνες Εικονική BYOL, πρέπει να έχετε και σύμβαση Enterprise με [Άδεια χρήσης φορητότητα μέσω εξασφάλισης αναβάθμισης λογισμικού σε Azure](https://azure.microsoft.com/pricing/license-mobility/). Επίσης, χρειάζεστε μια έγκυρη άδεια χρήσης για την έκδοση/έκδοση του SQL Server που θέλετε να χρησιμοποιήσετε. Πρέπει να [παρέχετε τις απαραίτητες πληροφορίες BYOL στη Microsoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) εντός **10** ημερών από την προμήθεια του Εικονική.

## <a name="manage-your-sql-vm"></a>Διαχείριση σας Εικονική SQL
Μετά την προμήθεια του Εικονική SQL Server, υπάρχουν αρκετές προαιρετικό Διαχείριση εργασιών. Σε πολλούς τομείς, μπορείτε να ρυθμίσετε τις παραμέτρους και να διαχειριστείτε SQL Server ακριβώς όπως θα διαχειρίζεστε μια παρουσία του SQL Server εσωτερικής εγκατάστασης. Ωστόσο, ορισμένες εργασίες αφορούν ειδικά Azure. Οι παρακάτω ενότητες επισημάνετε ορισμένες από αυτές τις περιοχές με συνδέσεις για περισσότερες πληροφορίες.

### <a name="connect-to-the-vm"></a>Σύνδεση με την εικονική Μηχανή
Ένα από τα πιο βασικά βήματα διαχείρισης είναι για να συνδεθείτε με το Εικονική SQL Server μέσω εργαλείων, όπως SQL Server Management Studio (SSMS). Για οδηγίες σχετικά με τον τρόπο για να συνδεθείτε με το νέο Εικονική SQL Server, ανατρέξτε στο θέμα [σύνδεση σε ένα SQL Server εικονικό μηχάνημα σε Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Μετεγκατάσταση των δεδομένων σας
Εάν έχετε μια υπάρχουσα βάση δεδομένων, που θα θέλετε να μετακινήσετε που για την προμήθεια του φακέλου που μόλις Εικονική SQL. Για μια λίστα με επιλογές μετεγκατάστασης και οδηγίες, ανατρέξτε στο θέμα [Μετεγκατάσταση βάσης δεδομένων με τον SQL Server σε μια Εικονική Azure](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Ρύθμιση παραμέτρων υψηλής διαθεσιμότητας
Εάν χρειάζεστε υψηλής διαθεσιμότητας, εξετάστε το ενδεχόμενο τη ρύθμιση των παραμέτρων ομάδες διαθεσιμότητα του SQL Server. Αυτό περιλαμβάνει πολλές ΣΠΣ Azure σε ένα εικονικό δίκτυο. Πύλη του Azure έχει ένα πρότυπο που ρυθμίζει αυτή η ρύθμιση παραμέτρων για εσάς. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μιας ομάδας διαθεσιμότητας AlwaysOn σε εικονικές μηχανές Windows Azure για τη διαχείριση πόρων](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Εάν θέλετε να ρυθμίσετε με μη αυτόματο τρόπο το ομάδας διαθεσιμότητας και σχετικές ακρόασης, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ομάδες διαθεσιμότητας AlwaysOn σε Εικονική Azure](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Για άλλα θέματα υψηλής διαθεσιμότητας, ανατρέξτε στο θέμα [Υψηλή διαθεσιμότητα και την αποκατάσταση για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Δημιουργία αντιγράφου ασφαλείας των δεδομένων σας
Azure ΣΠΣ να επωφεληθείτε από [Αυτόματης δημιουργίας αντιγράφων ασφαλείας](virtual-machines-windows-sql-automated-backup.md), που δημιουργεί τακτικά αντίγραφα ασφαλείας της βάσης δεδομένων σας με το χώρο αποθήκευσης αντικειμένων blob. Επίσης με μη αυτόματο τρόπο, μπορείτε να χρησιμοποιήσετε αυτήν την τεχνική. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση Azure χώρου αποθήκευσης για δημιουργία αντιγράφων ασφαλείας του SQL Server και επαναφορά](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Για μια επισκόπηση του όλες τις επιλογές δημιουργίας αντιγράφων ασφαλείας και επαναφορά, ανατρέξτε στο θέμα [Δημιουργία αντιγράφων ασφαλείας και επαναφοράς για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Αυτοματοποίηση ενημερώσεις
Azure ΣΠΣ να χρησιμοποιήσετε [Αυτόματη Διόρθωση](virtual-machines-windows-sql-automated-patching.md) για να προγραμματίσετε ένα παράθυρο συντήρησης για την εγκατάσταση του windows σημαντικές και SQL Server ενημερώνεται αυτόματα.

### <a name="customer-experience-improvement-program-ceip"></a>Πρόγραμμα βελτίωσης εμπειρίας πελατών (CEIP)
Το πρόγραμμα βελτίωσης εμπειρίας πελατών (CEIP) είναι ενεργοποιημένη από προεπιλογή. Αυτό περιοδικά στέλνει αναφορές στη Microsoft για να βοηθήσει στη βελτίωση του SQL Server. Δεν υπάρχει εργασία διαχείρισης απαιτείται με CEIP, εκτός αν θέλετε να την απενεργοποιήσετε μετά την προμήθεια. Μπορείτε να προσαρμόσετε ή να απενεργοποιήσετε το CEIP κατά τη σύνδεση με την εικονική Μηχανή με σύνδεση απομακρυσμένης επιφάνειας εργασίας. Στη συνέχεια, εκτελέστε το βοηθητικό πρόγραμμα **λαθών SQL Server και δημιουργία αναφορών χρήσης** . Ακολουθήστε τις οδηγίες για να απενεργοποιήσετε την αναφορά. 

Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα CEIP του θέματος [Αποδεχτείτε τους όρους άδειας χρήσης](https://msdn.microsoft.com/library/ms143343.aspx) . 

## <a name="next-steps"></a>Επόμενα βήματα
[Εξερεύνηση της διαδρομής εκμάθησης](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) για τον SQL Server σε εικονικές μηχανές οι Azure.

Περισσότερες ερώτηση; Πρώτα, ανατρέξτε στο θέμα το [SQL Server σε συνήθεις Ερωτήσεις εικονικές μηχανές Azure](virtual-machines-windows-sql-server-iaas-faq.md). Αλλά επίσης να προσθέσετε τις ερωτήσεις ή σχόλια στο κάτω μέρος τυχόν θέματα Εικονική SQL για να αλληλεπιδράσετε με το Microsoft και την Κοινότητα.
