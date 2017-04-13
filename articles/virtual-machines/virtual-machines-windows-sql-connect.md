<properties
    pageTitle="Σύνδεση με μια SQL Server εικονική μηχανή (Διαχείριση πόρων) | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να συνδεθείτε με τον SQL Server που εκτελείται σε μια εικονική μηχανή στο Azure. Αυτό το θέμα χρησιμοποιεί το μοντέλο κλασική ανάπτυξης. Τα σενάρια διαφέρουν ανάλογα με τη ρύθμιση παραμέτρων δικτύου και τη θέση του προγράμματος-πελάτη."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Σύνδεση με ένα SQL Server εικονικό μηχάνημα σε Azure (Διαχείριση πόρων)

> [AZURE.SELECTOR]
- [Διαχείριση πόρων](virtual-machines-windows-sql-connect.md)
- [Κλασικό](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Επισκόπηση

Αυτό το θέμα περιγράφει τον τρόπο σύνδεσης με την παρουσία του SQL Server που εκτελείται σε μια εικονική μηχανή Azure. Αυτό περιγράφει ορισμένα [σενάρια γενικά συνδεσιμότητας](#connection-scenarios) και, στη συνέχεια, παρέχει [λεπτομερείς οδηγίες για τη ρύθμιση παραμέτρων σύνδεσης του SQL Server σε μια Εικονική Azure](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης. Για να προβάλετε την κλασική έκδοση αυτού του άρθρου, ανατρέξτε στο θέμα [σύνδεση σε ένα SQL Server εικονικό μηχάνημα σε κλασική Azure](virtual-machines-windows-classic-sql-connect.md).

Εάν προτιμάτε να έχετε μια πλήρη Γνωρίστε προμήθεια και συνδεσιμότητα, ανατρέξτε στο θέμα [προμήθεια του SQL Server εικονικό μηχάνημα σε Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Σενάρια σύνδεσης

Ο τρόπος ένας υπολογιστής-πελάτης συνδέεται με τον SQL Server που εκτελείται σε μια εικονική μηχανή διαφέρει ανάλογα με τη θέση του υπολογιστή-πελάτη και τη ρύθμιση παραμέτρων υπολογιστή/δικτύου. Αυτά τα σενάρια περιλαμβάνουν τα εξής:

- [Σύνδεση με τον SQL Server μέσω του internet](#connect-to-sql-server-over-the-internet)
- [Σύνδεση με τον SQL Server στο ίδιο δίκτυο εικονικού](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>Σύνδεση με τον SQL Server μέσω του Internet

Εάν θέλετε να συνδεθείτε με το του μηχανισμού βάσεων δεδομένων SQL Server από το Internet, υπάρχουν αρκετά βήματα που απαιτούνται, όπως τη ρύθμιση των παραμέτρων του τείχους προστασίας, ενεργοποίηση του ελέγχου ταυτότητας SQL και τη ρύθμιση των παραμέτρων σας ομάδα ασφαλείας δικτύου που πρέπει να έχετε έναν κανόνα ομάδας ασφαλείας δικτύου για να επιτρέψετε την κυκλοφορία TCP στη θύρα 1433.

Εάν χρησιμοποιείτε την πύλη για την παροχή μια εικόνα εικονική μηχανή SQL Server με τη διαχείριση πόρων, αυτά τα βήματα γίνονται για εσάς όταν επιλέγετε **δημόσια** για την επιλογή συνδεσιμότητας SQL:

![Δημόσια επιλογή σύνδεσης SQL κατά την προμήθεια του](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Εάν αυτό δεν ήταν κατά την προμήθεια, στη συνέχεια, μπορείτε να με μη αυτόματο τρόπο ρυθμίσετε SQL Server και σας εικονικές μηχανές, ακολουθώντας τα [βήματα σε αυτό το άρθρο για να ρυθμίσετε τη σύνδεση του με μη αυτόματο τρόπο](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

>[AZURE.NOTE] Η εικόνα εικονική μηχανή για SQL Server Express edition δεν ενεργοποιεί αυτόματα το πρωτόκολλο TCP/IP. Για Express edition, πρέπει να χρησιμοποιήσετε SQL Server Configuration Manager για να [ενεργοποιήσετε με μη αυτόματο τρόπο το πρωτόκολλο TCP/IP](#configure-sql-server-to-listen-on-the-tcp-protocol) μετά τη δημιουργία του Εικονική.

Αφού το κάνετε αυτό, οποιοδήποτε πρόγραμμα-πελάτη με πρόσβαση στο internet να συνδεθείτε με την παρουσία του SQL Server, καθορίζοντας είτε στη δημόσια διεύθυνση IP του υπολογιστή εικονική ή την ετικέτα DNS που έχουν εκχωρηθεί σε αυτήν τη διεύθυνση IP. Εάν η θύρα SQL Server είναι 1433, δεν χρειάζεται να το καθορίσετε στη συμβολοσειρά σύνδεσης.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Παρόλο που αυτή η δυνατότητα επιτρέπει τη σύνδεση για προγράμματα-πελάτες μέσω του internet, αυτό δεν σημαίνει ότι οποιοσδήποτε μπορεί να συνδεθείτε με τον SQL Server. Έξω από προγράμματα-πελάτες πρέπει να το σωστό όνομα χρήστη και κωδικό πρόσβασης. Για πρόσθετη ασφάλεια, μπορείτε να αποφύγετε τη θύρα γνωστές 1433. Για παράδειγμα, εάν έχετε ρυθμίσει τις παραμέτρους SQL Server για να ακούσετε στη θύρα 1500 και έχει δημιουργηθεί proper τείχος προστασίας και των κανόνων ομάδα ασφαλείας δικτύου, μπορείτε να συνδεθείτε προσαρτώντας τον αριθμό θύρας στο όνομα του διακομιστή, όπως στο ακόλουθο παράδειγμα:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

>[AZURE.NOTE] Είναι σημαντικό να λάβετε υπόψη ότι, όταν χρησιμοποιείτε αυτήν την τεχνική για να επικοινωνήσετε με τον SQL Server, όλα τα εξερχόμενα δεδομένα από το κέντρο δεδομένων Azure υπόκειται κανονική [τιμολόγησης σε μεταφορές εξερχομένων δεδομένων](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Σύνδεση με τον SQL Server στο ίδιο δίκτυο εικονικού

[Εικονικό δίκτυο](../virtual-network/virtual-networks-overview.md) επιτρέπει επιπλέον σενάρια. Μπορείτε να συνδεθείτε ΣΠΣ στο ίδιο δίκτυο εικονικού, ακόμα και αν αυτά τα ΣΠΣ που υπάρχουν σε διαφορετικές ομάδες πόρων. Και με ένα [VPN-τοποθεσίας](../vpn-gateway/vpn-gateway-site-to-site-create.md), μπορείτε να δημιουργήσετε μια αρχιτεκτονική υβριδική που συνδέεται ΣΠΣ με δίκτυα εσωτερικής εγκατάστασης και μηχανές.

Εικονικό δίκτυα σάς επιτρέπει επίσης να συνδέσετε το ΣΠΣ Azure σε έναν τομέα. Αυτό είναι ο μόνος τρόπος για να χρησιμοποιήσετε έλεγχο ταυτότητας των Windows με τον SQL Server. Τα άλλα σενάρια σύνδεσης απαιτείται έλεγχος ταυτότητας του SQL με ονόματα χρηστών και κωδικούς πρόσβασης.

Εάν χρησιμοποιείτε την πύλη για την παροχή μια εικόνα εικονική μηχανή SQL Server με τη διαχείριση πόρων, οι κανόνες proper τείχους προστασίας για την επικοινωνία του εικονικού δικτύου είναι ρύθμισης όταν επιλέγετε **ιδιωτικό** για την επιλογή συνδεσιμότητας SQL. Εάν αυτό δεν ήταν κατά την προμήθεια, στη συνέχεια, μπορείτε να με μη αυτόματο τρόπο ρυθμίσετε SQL Server και σας εικονικές μηχανές, ακολουθώντας τα [βήματα σε αυτό το άρθρο για να ρυθμίσετε τη σύνδεση του με μη αυτόματο τρόπο](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm). Αλλά εάν σχεδιάζετε να ρυθμίσετε τις παραμέτρους σε ένα περιβάλλον τομέα και έλεγχος ταυτότητας των Windows, δεν χρειάζεται να ακολουθήσετε τα βήματα σε αυτό το άρθρο για να ρυθμίσετε τον έλεγχο ταυτότητας SQL και συνδέσεις. Επίσης δεν χρειάζεται να ρυθμίσετε τις παραμέτρους δικτύου ομάδα ασφαλείας κανόνες για την πρόσβαση μέσω του internet.

>[AZURE.NOTE] Η εικόνα εικονική μηχανή για SQL Server Express edition δεν ενεργοποιεί αυτόματα το πρωτόκολλο TCP/IP. Για Express edition, πρέπει να χρησιμοποιήσετε SQL Server Configuration Manager για να [ενεργοποιήσετε με μη αυτόματο τρόπο το πρωτόκολλο TCP/IP](#configure-sql-server-to-listen-on-the-tcp-protocol) μετά τη δημιουργία του Εικονική.

Εάν υποθέσουμε ότι έχετε ρυθμίσει DNS στο δίκτυό σας εικονικού, μπορείτε να συνδεθείτε για να την παρουσία του SQL Server, καθορίζοντας το όνομα του υπολογιστή SQL Server Εικονική στη συμβολοσειρά σύνδεσης. Το παρακάτω παράδειγμα επίσης προϋποθέτει ότι έχει επίσης ρυθμίσεις ελέγχου ταυτότητας των Windows και ότι ο χρήστης έχει παραχωρηθεί πρόσβαση για να την παρουσία του SQL Server.

    "Server=mysqlvm;Integrated Security=true"

Σημειώστε ότι σε αυτό το σενάριο, που μπορεί επίσης να καθορίσετε τη διεύθυνση IP του η Εικονική.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Βήματα για τη μη αυτόματη ρύθμιση παραμέτρων συνδεσιμότητας SQL Server σε μια Εικονική Azure

Ακολουθήστε τα παρακάτω βήματα δείχνουν πώς μπορείτε να ρυθμίσετε με μη αυτόματο τρόπο τη δυνατότητα σύνδεσης με την παρουσία του SQL Server και, στη συνέχεια, συνδεθείτε προαιρετικά μέσω του internet με χρήση του SQL Server Management Studio (SSMS). Σημειώστε ότι πολλά από τα παρακάτω βήματα θα γίνει για εσάς όταν επιλέγετε τις κατάλληλες επιλογές συνδεσιμότητας του SQL Server στην πύλη.

Για να συνδεθείτε στην παρουσία του SQL Server από μια άλλη Εικονική ή στο internet, πρέπει να ολοκληρώσετε τις παρακάτω εργασίες όπως περιγράφεται στις ενότητες που ακολουθούν:

- [Ανοίξτε τις θύρες TCP στο τείχος προστασίας των Windows](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Ρύθμιση παραμέτρων του SQL Server για ακρόαση το πρωτόκολλο TCP](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Ρύθμιση παραμέτρων του SQL Server για μεικτά λειτουργίας ελέγχου ταυτότητας](#configure-sql-server-for-mixed-mode-authentication)
- [Δημιουργία συνδέσεων τον έλεγχο ταυτότητας του SQL Server](#create-sql-server-authentication-logins)
- [Ρύθμιση παραμέτρων ενός κανόνα εισερχομένων δικτύου ομάδας ασφαλείας](#configure-a-network-security-group-inbound-rule-for-the-vm)
- [Ρύθμιση παραμέτρων DNS ετικέτας για τη δημόσια διεύθυνση IP](#configure-a-dns-label-for-the-public-ip-address)
- [Σύνδεση με το μηχανισμό βάσεων δεδομένων από έναν άλλο υπολογιστή](#connect-to-the-database-engine-from-another-computer)

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Για να δείτε την προμήθεια οδηγίες μαζί με αυτά τα βήματα συνδεσιμότητας, ανατρέξτε στο θέμα [προμήθεια του SQL Server εικονικό μηχάνημα σε Azure](virtual-machines-windows-portal-sql-server-provision.md).

[Εξερεύνηση της διαδρομής εκμάθησης](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) για τον SQL Server σε εικονικές μηχανές οι Azure.

Για άλλα θέματα που σχετίζονται με εκτελεί τον SQL Server στο ΣΠΣ Azure, ανατρέξτε στο θέμα [SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-server-iaas-overview.md).
