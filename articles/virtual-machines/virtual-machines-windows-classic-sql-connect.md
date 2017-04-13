<properties
    pageTitle="Σύνδεση με μια SQL Server εικονική μηχανή (κλασικό) | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να συνδεθείτε με τον SQL Server που εκτελείται σε μια εικονική μηχανή στο Azure. Αυτό το θέμα χρησιμοποιεί το μοντέλο κλασική ανάπτυξης. Τα σενάρια διαφέρουν ανάλογα με τη ρύθμιση παραμέτρων δικτύου και τη θέση του προγράμματος-πελάτη."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Σύνδεση με ένα SQL Server εικονικό μηχάνημα σε Azure (κλασική ανάπτυξη)

> [AZURE.SELECTOR]
- [Διαχείριση πόρων](virtual-machines-windows-sql-connect.md)
- [Κλασικό](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Επισκόπηση

Αυτό το θέμα περιγράφει τον τρόπο σύνδεσης με την παρουσία του SQL Server που εκτελείται σε μια εικονική μηχανή Azure. Αυτό περιγράφει ορισμένα [σενάρια γενικά συνδεσιμότητας](#connection-scenarios) και, στη συνέχεια, παρέχει [λεπτομερείς οδηγίες για τη ρύθμιση παραμέτρων σύνδεσης του SQL Server σε μια Εικονική Azure](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Εάν χρησιμοποιείτε ΣΠΣ διαχείριση πόρων, ανατρέξτε στο θέμα [σύνδεση σε ένα SQL Server εικονικό μηχάνημα σε Azure χρησιμοποιώντας τη διαχείριση πόρων](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Σενάρια σύνδεσης

Ο τρόπος ένας υπολογιστής-πελάτης συνδέεται με τον SQL Server που εκτελείται σε μια εικονική μηχανή διαφέρει ανάλογα με τη θέση του υπολογιστή-πελάτη και τη ρύθμιση παραμέτρων υπολογιστή/δικτύου. Αυτά τα σενάρια περιλαμβάνουν τα εξής:

- [Σύνδεση με τον SQL Server στην ίδια υπηρεσία cloud](#connect-to-sql-server-in-the-same-cloud-service)
- [Σύνδεση με τον SQL Server μέσω του internet](#connect-to-sql-server-over-the-internet)
- [Σύνδεση με τον SQL Server στο ίδιο δίκτυο εικονικού](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Πριν να συνδεθείτε με οποιαδήποτε από τις παρακάτω μεθόδους, πρέπει να ακολουθήσετε τα [βήματα σε αυτό το άρθρο για να ρυθμίσετε τη σύνδεση](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Σύνδεση με τον SQL Server στην ίδια υπηρεσία cloud

Πολλές εικονικές μηχανές μπορούν να δημιουργηθούν στην ίδια υπηρεσία cloud. Για να κατανοήσετε αυτό το σενάριο εικονικές μηχανές, δείτε [πώς μπορείτε να συνδεθείτε εικονικές μηχανές με μια υπηρεσία εικονικού δικτύου ή στο cloud](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service). Αυτό το σενάριο είναι όταν ένα πρόγραμμα-πελάτη σε έναν υπολογιστή εικονικές επιχειρεί να συνδεθεί με SQL Server που εκτελείται σε άλλον υπολογιστή εικονικές στην ίδια υπηρεσία cloud.

Σε αυτό το σενάριο, μπορείτε να συνδεθείτε χρησιμοποιώντας το Εικονική **όνομα** (εμφανίζεται επίσης ως **Το όνομα του υπολογιστή** ή **όνομα κεντρικού υπολογιστή** στην πύλη του). Αυτό είναι το όνομα που παρέχεται για την εικονική Μηχανή κατά τη δημιουργία. Για παράδειγμα, εάν ονομάζεται σας Εικονική SQL **mysqlvm**, ένα πρόγραμμα-πελάτη Εικονική στην ίδια υπηρεσία cloud μπορούν να χρησιμοποιήσουν την ακόλουθη συμβολοσειρά σύνδεσης για να συνδεθείτε:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Σύνδεση με τον SQL Server μέσω του Internet

Εάν θέλετε να συνδεθείτε με το του μηχανισμού βάσεων δεδομένων SQL Server από το Internet, πρέπει να δημιουργήσετε ένα τελικό σημείο εικονική μηχανή για εισερχόμενες TCP επικοινωνία. Αυτό το βήμα Azure ρύθμισης παραμέτρων, κατευθύνει εισερχόμενη κυκλοφορία θύρας TCP σε μια θύρα TCP που είναι προσβάσιμο με τον υπολογιστή εικονική.

Για να συνδεθείτε μέσω του internet, πρέπει να χρησιμοποιήσετε την εικονική Μηχανή όνομα DNS και τον αριθμό θύρας τελικού σημείου Εικονική (ρυθμιστεί αργότερα σε αυτό το άρθρο). Για να βρείτε το όνομα DNS, μεταβείτε στην πύλη του Azure και επιλέξτε **εικονικές μηχανές (κλασική)**. Στη συνέχεια, επιλέξτε την εικονική μηχανή σας. Το **όνομα DNS** εμφανίζεται στην ενότητα **Overview** .

Για παράδειγμα, εξετάστε το ενδεχόμενο κλασική εικονικό μηχάνημα που ονομάζεται **mysqlvm** με το όνομα DNS του **mysqlvm7777.cloudapp.net** και ένα τελικό σημείο Εικονική της **57500**. Υπό την προϋπόθεση ότι σωστά ρυθμισμένο συνδεσιμότητας, η ακόλουθη συμβολοσειρά σύνδεσης θα μπορούσαν να χρησιμοποιηθούν για να αποκτήσετε πρόσβαση στην εικονική μηχανή από οποιοδήποτε σημείο στο internet:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Παρόλο που αυτή η δυνατότητα επιτρέπει τη σύνδεση για προγράμματα-πελάτες μέσω του internet, αυτό δεν σημαίνει ότι οποιοσδήποτε μπορεί να συνδεθείτε με τον SQL Server. Έξω από προγράμματα-πελάτες πρέπει να το σωστό όνομα χρήστη και κωδικό πρόσβασης. Για πρόσθετη ασφάλεια, μην χρησιμοποιείτε τη θύρα γνωστές 1433 για το τελικό σημείο δημόσια εικονική μηχανή. Και εάν είναι δυνατόν, μπορείτε να προσθέσετε ένα ACL σε το τελικό σημείο για να περιορίσετε την κίνηση μόνο για τα προγράμματα-πελάτες στον οποίο το επιτρέπετε. Για οδηγίες σχετικά με τη χρήση ACL με τα τελικά σημεία, ανατρέξτε στο θέμα [Διαχείριση της ACL σε ένα τελικό σημείο](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

>[AZURE.NOTE] Είναι σημαντικό να λάβετε υπόψη ότι, όταν χρησιμοποιείτε αυτήν την τεχνική για να επικοινωνήσετε με τον SQL Server, όλα τα εξερχόμενα δεδομένα από το κέντρο δεδομένων Azure υπόκειται κανονική [τιμολόγησης σε μεταφορές εξερχομένων δεδομένων](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Σύνδεση με τον SQL Server στο ίδιο δίκτυο εικονικού

[Εικονικό δίκτυο](../virtual-network/virtual-networks-overview.md) επιτρέπει επιπλέον σενάρια. Μπορείτε να συνδεθείτε ΣΠΣ στο ίδιο δίκτυο εικονικού, ακόμα και αν αυτά τα ΣΠΣ υπάρχουν στο cloud διαφορετικές υπηρεσίες. Και με ένα [VPN-τοποθεσίας](../vpn-gateway/vpn-gateway-site-to-site-create.md), μπορείτε να δημιουργήσετε μια αρχιτεκτονική υβριδική που συνδέεται ΣΠΣ με δίκτυα εσωτερικής εγκατάστασης και μηχανές.

Εικονικό δίκτυα σάς επιτρέπει επίσης να συνδέσετε το ΣΠΣ Azure σε έναν τομέα. Αυτό είναι ο μόνος τρόπος για να χρησιμοποιήσετε έλεγχο ταυτότητας των Windows με τον SQL Server. Τα άλλα σενάρια σύνδεσης απαιτείται έλεγχος ταυτότητας του SQL με ονόματα χρηστών και κωδικούς πρόσβασης.

Εάν πρόκειται για να ρυθμίσετε ένα περιβάλλον τομέα και έλεγχος ταυτότητας των Windows, δεν χρειάζεται να ακολουθήσετε τα βήματα σε αυτό το άρθρο για να ρυθμίσετε το τελικό σημείο δημόσια ή τον έλεγχο ταυτότητας SQL και συνδέσεις. Σε αυτό το σενάριο, μπορείτε να συνδεθείτε για να την παρουσία του SQL Server, καθορίζοντας το όνομα του SQL Server Εικονική στη συμβολοσειρά σύνδεσης. Το παρακάτω παράδειγμα προϋποθέτει ότι έχει επίσης ρυθμίσεις ελέγχου ταυτότητας των Windows και ότι ο χρήστης έχει παραχωρηθεί πρόσβαση για να την παρουσία του SQL Server.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Βήματα για τη ρύθμιση παραμέτρων σύνδεσης του SQL Server σε μια Εικονική Azure

Ακολουθήστε τα παρακάτω βήματα δείχνουν τον τρόπο σύνδεσης με την παρουσία του SQL Server μέσω internet, χρησιμοποιώντας SQL Server Management Studio (SSMS). Ωστόσο, τα ίδια βήματα ισχύουν για βελτίωση της προσβασιμότητας για τις εφαρμογές σας, εκτελούνται δύο εσωτερικής εγκατάστασης και στο Azure εικονική μηχανή του SQL Server.

Για να συνδεθείτε στην παρουσία του SQL Server από μια άλλη Εικονική ή στο internet, πρέπει να ολοκληρώσετε τις παρακάτω εργασίες όπως περιγράφεται στις ενότητες που ακολουθούν:

- [Δημιουργήσετε ένα τελικό σημείο TCP για την εικονική μηχανή](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Ανοίξτε τις θύρες TCP στο τείχος προστασίας των Windows](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Ρύθμιση παραμέτρων του SQL Server για ακρόαση το πρωτόκολλο TCP](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Ρύθμιση παραμέτρων του SQL Server για μεικτά λειτουργίας ελέγχου ταυτότητας](#configure-sql-server-for-mixed-mode-authentication)
- [Δημιουργία συνδέσεων τον έλεγχο ταυτότητας του SQL Server](#create-sql-server-authentication-logins)
- [Προσδιορισμός του ονόματος DNS η εικονική μηχανή](#determine-the-dns-name-of-the-virtual-machine)
- [Σύνδεση με το μηχανισμό βάσεων δεδομένων από έναν άλλο υπολογιστή](#connect-to-the-database-engine-from-another-computer)

Η διαδρομή σύνδεσης σύνοψης από το παρακάτω διάγραμμα:

![Σύνδεση με μια εικονική μηχανή SQL Server](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Εάν σχεδιάζετε επίσης να χρησιμοποιήσετε ομάδες διαθεσιμότητας AlwaysOn για υψηλή διαθεσιμότητα και αποκατάσταση, πρέπει να λάβετε υπόψη εφαρμογής υπηρεσίας ακρόασης. Προγράμματα-πελάτες βάσης δεδομένων σύνδεση για το ακροατήριο και όχι απευθείας σε μία από τις παρουσίες του SQL Server. Το ακροατήριο δρομολογεί προγράμματα-πελάτες στην κύρια ρεπλίκα στην ομάδα διαθεσιμότητα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μιας υπηρεσίας ακρόασης ILB για ομάδες διαθεσιμότητας AlwaysOn στο Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Είναι σημαντικό για να δείτε όλες τις βέλτιστες πρακτικές ασφαλείας για τον SQL Server που εκτελείται σε μια εικονική μηχανή Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ζητήματα ασφαλείας για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-security.md).

[Εξερεύνηση της διαδρομής εκμάθησης](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) για τον SQL Server σε εικονικές μηχανές οι Azure. 

Για άλλα θέματα που σχετίζονται με εκτελεί τον SQL Server στο ΣΠΣ Azure, ανατρέξτε στο θέμα [SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-server-iaas-overview.md).
