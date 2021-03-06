<properties
    pageTitle="Δημιουργήστε μια Εικονική εκτελείται MySQL | Microsoft Azure"
    description="Δημιουργήστε ένα Azure εικονικές υπολογιστή που εκτελεί Windows Server 2012 R2 και τη βάση δεδομένων MySQL χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="cynthn"/>


# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2012-r2"></a>Εγκατάσταση MySQL σε μια εικονική μηχανή που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης που εκτελεί Windows Server 2012 R2

[MySQL](http://www.mysql.com) είναι μια δημοφιλή Άνοιγμα αρχείου προέλευσης, βάση δεδομένων SQL. Αυτό το πρόγραμμα εκμάθησης θα μάθετε πώς να εγκαταστήσετε και να εκτελέσετε την έκδοση της Κοινότητας του MySQL 5.6.23 ως διακομιστής MySQL σε εικονικές υπολογιστή με Windows Server 2012 R2. Για οδηγίες σχετικά με την εγκατάσταση του MySQL σε Linux, ανατρέξτε στην: [Πώς να εγκαταστήσετε το MySQL σε Azure](virtual-machines-linux-mysql-install.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="create-a-virtual-machine-running-windows-server-2012-r2"></a>Δημιουργήστε μια εικονική μηχανή με Windows Server 2012 R2

Εάν δεν έχετε ήδη μια Εικονική με Windows Server 2012 R2, μπορείτε να χρησιμοποιήσετε αυτό [το πρόγραμμα εκμάθησης](virtual-machines-windows-classic-tutorial.md) για να δημιουργήσετε την εικονική μηχανή. 

## <a name="attach-a-data-disk"></a>Επισυνάψτε ένα δίσκο δεδομένων

Αφού δημιουργηθεί η εικονική μηχανή, μπορείτε προαιρετικά να επισυνάψετε ένα δίσκο πρόσθετα δεδομένα. Αυτό συνιστάται για παραγωγής φόρτους εργασίας και να αποφύγετε την έλλειψη χώρου στο δίσκο λειτουργικό σύστημα (C:), που περιλαμβάνει το λειτουργικό σύστημα.

Δείτε [πώς μπορείτε να επισυνάψετε ένα δίσκο δεδομένων σε μια εικονική μηχανή των Windows](virtual-machines-windows-classic-attach-disk.md) και ακολουθήστε τις οδηγίες για την επισύναψη ενός κενού δίσκου. Ορίστε τη ρύθμιση cache host σε **κανένα** ή **μόνο για ανάγνωση**.

## <a name="log-on-to-the-virtual-machine"></a>Συνδεθείτε με την εικονική μηχανή

Στη συνέχεια, θα [συνδεθείτε με την εικονική μηχανή](virtual-machines-windows-classic-connect-logon.md) , ώστε να μπορείτε να εγκαταστήσετε το MySQL.

##<a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Εγκατάσταση και εκτέλεση διακομιστής Κοινότητας MySQL στον υπολογιστή εικονικές

Ακολουθήστε τα παρακάτω βήματα για την εγκατάσταση, ρύθμιση παραμέτρων και εκτέλεση της έκδοσης της Κοινότητας του διακομιστή MySQL:

> [AZURE.NOTE] Αυτά τα βήματα που αφορούν το 5.6.23.0 Κοινότητας έκδοση του MySQL και Windows Server 2012 R2. Την εμπειρία σας μπορεί να διαφέρει για διαφορετικές εκδόσεις του MySQL ή Windows Server.

1.  Αφού έχετε συνδέσει η εικονική μηχανή χρησιμοποιώντας απομακρυσμένης επιφάνειας εργασίας, κάντε κλικ στην επιλογή **Internet Explorer** , από την οθόνη έναρξης.
2.  Επιλέξτε το κουμπί " **Εργαλεία** " στην επάνω δεξιά γωνία (το εικονίδιο cogged τροχού) και, στη συνέχεια, κάντε κλικ στην εντολή **Επιλογές Internet**. Κάντε κλικ στην καρτέλα **ασφάλεια** , κάντε κλικ στο εικονίδιο **Αξιόπιστες τοποθεσίες** και, στη συνέχεια, κάντε κλικ στο κουμπί **τοποθεσίες** . Προσθήκη http://*. mysql.com στη λίστα των αξιόπιστων τοποθεσιών. Κάντε κλικ στην επιλογή * *Κλείσιμο**, και, στη συνέχεια, κάντε κλικ στην επιλογή * *OK**.
3.  Στη διεύθυνση γραμμή του Internet Explorer, πληκτρολογήστε http://dev.mysql.com/downloads/mysql/.
4.  Χρησιμοποιήστε την τοποθεσία MySQL για να εντοπίσετε και να κάνετε λήψη την πιο πρόσφατη έκδοση του προγράμματος εγκατάστασης MySQL για Windows. Όταν επιλέγετε το πρόγραμμα εγκατάστασης του MySQL, κάντε λήψη της έκδοσης που έχει μια αρχείων σύνολο (για παράδειγμα, το mysql-installer-Κοινότητας-5.6.23.0.msi με μέγεθος αρχείου του 282.4 MB) και αποθηκεύστε το πρόγραμμα εγκατάστασης.
5.  Όταν ολοκληρωθεί η λήψη του προγράμματος εγκατάστασης, κάντε κλικ στο κουμπί **Εκτέλεση** για να ξεκινήσετε το πρόγραμμα εγκατάστασης.
6.  Στη σελίδα **Άδεια χρήσης** , αποδεχτείτε την άδεια χρήσης και κάντε κλικ στο κουμπί **Επόμενο**.
7.  Στη σελίδα **Επιλογή τύπου εγκατάστασης** , κάντε κλικ στον τύπο εγκατάστασης που θέλετε και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. Ακολουθήστε τα παρακάτω βήματα λαμβάνεται ως δεδομένο της επιλογής τον τύπο εγκατάστασης **μόνο για διακομιστή** .
8.  Στη σελίδα **εγκατάστασης** , κάντε κλικ στην επιλογή **Εκτέλεση**. Μόλις ολοκληρωθεί η εγκατάσταση, κάντε κλικ στο κουμπί **Επόμενο**.
9.  Στη σελίδα **Ρύθμιση παραμέτρων του προϊόντος** , κάντε κλικ στο κουμπί **Επόμενο**.
10. Στη σελίδα **δικτύωση και τύπων** , καθορίστε την επιθυμητή ρύθμιση παραμέτρων τύπο και συνδεσιμότητας επιλογές, συμπεριλαμβανομένου του τη θύρα TCP εάν είναι απαραίτητο. Επιλέξτε **Εμφάνιση επιλογών για προχωρημένους**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)

11. Στη σελίδα **Λογαριασμοί και τους ρόλους** , καθορίστε έναν ισχυρό κωδικό πρόσβασης ρίζας MySQL. Προσθήκη επιπλέον λογαριασμών χρήστη MySQL, όπως απαιτείται και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)

12. Στη σελίδα **Υπηρεσία των Windows** , καθορίστε τις αλλαγές στις προεπιλεγμένες ρυθμίσεις για την εκτέλεση ο διακομιστής MySQL ως μια υπηρεσία των Windows, όπως απαιτείται και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)

13. Στη σελίδα **Επιλογές για προχωρημένους** , καθορίστε αλλαγές στις επιλογές καταγραφής όπως απαιτείται και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)

14. Στη σελίδα **Εφαρμογή ρύθμισης παραμέτρων του διακομιστή** , κάντε κλικ στην επιλογή **Εκτέλεση**. Όταν ολοκληρώσετε τα βήματα ρύθμισης παραμέτρων, κάντε κλικ στο κουμπί **Τέλος**.
15. Στη σελίδα **Ρύθμιση παραμέτρων του προϊόντος** , κάντε κλικ στο κουμπί **Επόμενο**.
16. Στη σελίδα **Ολοκλήρωση εγκατάστασης** , κάντε κλικ στην επιλογή **Log Αντιγραφή στο Πρόχειρο** , εάν θέλετε να εξετάσετε το αργότερα και, στη συνέχεια, κάντε κλικ στο κουμπί **Τέλος**.
17. Από την οθόνη έναρξης, πληκτρολογήστε **mysql**και, στη συνέχεια, κάντε κλικ στην επιλογή **MySQL 5.6 γραμμής εντολών προγράμματος-πελάτη**.
18. Πληκτρολογήστε τον κωδικό πρόσβασης ρίζας που καθορίσατε στο βήμα 11 και θα εμφανιστεί με ένα μήνυμα όπου μπορείτε να εκδώσετε εντολές για να ρυθμίσετε τις παραμέτρους MySQL. Για τις λεπτομέρειες των εντολών και σύνταξη, ανατρέξτε [Εγχειρίδια αναφοράς MySQL](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)

19. Μπορείτε επίσης να ρυθμίσετε το διακομιστή ρύθμισης παραμέτρων προεπιλεγμένες ρυθμίσεις, όπως η βάση και δεδομένα σε καταλόγους και μονάδες, με εγγραφές στο αρχείο 5.6\my-default.ini C:\Program Files (x86) \MySQL\MySQL διακομιστή. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [5.1.2 διακομιστή ρύθμισης παραμέτρων προεπιλογών](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Ρύθμιση παραμέτρων τελικά σημεία

Εάν θέλετε η υπηρεσία διακομιστής MySQL να είναι διαθέσιμη σε υπολογιστές-πελάτες MySQL στο Internet, πρέπει να ρυθμίσετε τις παραμέτρους ενός ορίου για τη θύρα TCP στην οποία ακρόαση της υπηρεσίας διακομιστή MySQL και δημιουργήσετε πρόσθετο κανόνα τείχους προστασίας των Windows. Πρόκειται για τη θύρα TCP 3306 εκτός και εάν έχετε καθορίσει μια διαφορετική θύρα στη σελίδα **Τύπος και δικτύωση** (βήμα 10 της προηγούμενης διαδικασίας).


> [AZURE.NOTE] Θα πρέπει να αναλογιστείτε τα ζητήματα ασφαλείας του αυτή η ενέργεια, επειδή έτσι θα είναι η υπηρεσία διακομιστής MySQL διαθέσιμη σε όλους τους υπολογιστές στο Internet. Μπορείτε να ορίσετε το σύνολο των διευθύνσεων IP προέλευσης που επιτρέπονται για να χρησιμοποιήσετε το τελικό σημείο με μια λίστα ελέγχου πρόσβασης (ACL). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να ορίσετε του τελικά σημεία σε μια εικονική μηχανή](virtual-machines-windows-classic-setup-endpoints.md).


Για να ρυθμίσετε ένα τελικό σημείο για την υπηρεσία MySQL διακομιστή:

1.  Στην κλασική Azure πύλη, κάντε κλικ στην επιλογή **εικονικές μηχανές**, κάντε κλικ στο όνομα του υπολογιστή εικονικές MySQL και, στη συνέχεια, κάντε κλικ στην επιλογή **τελικά σημεία**.
2.  Στη γραμμή εντολών, κάντε κλικ στην επιλογή **Προσθήκη**.
3.  Στη σελίδα **Προσθήκη ένα τελικό σημείο σε ένα εικονικό μηχάνημα** , κάντε κλικ στο δεξιό βέλος.
4.  Εάν χρησιμοποιείτε την προεπιλεγμένη θύρα MySQL TCP της 3306, κάντε κλικ στην επιλογή **MySQL** στο πλαίσιο **όνομα**και, στη συνέχεια, κάντε κλικ στο σημάδι ελέγχου.
5.  Εάν χρησιμοποιείτε μια διαφορετική θύρα TCP, πληκτρολογήστε ένα μοναδικό όνομα στο πεδίο **όνομα**. Επιλέξτε **TCP** στο πρωτόκολλο, πληκτρολογήστε τον αριθμό θύρας σε **Δημόσια θύρας** και **Ιδιωτική θύρα**, και, στη συνέχεια, κάντε κλικ στο σημάδι ελέγχου.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>Προσθήκη κανόνα τείχους προστασίας των Windows για να επιτρέψετε την κυκλοφορία MySQL

Για να προσθέσετε έναν κανόνα τείχους προστασίας των Windows που επιτρέπει την κυκλοφορία MySQL από το Internet, εκτελέστε την ακόλουθη εντολή σε μια αναβαθμισμένη γραμμή εντολών του Windows PowerShell στον υπολογιστή εικονικές του διακομιστή MySQL.

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


    
## <a name="test-your-remote-connection"></a>Έλεγχος της απομακρυσμένης σύνδεσης


Για να ελέγξετε τον απομακρυσμένο σύνδεση με την υπηρεσία διακομιστής MySQL εκτελούνται στον υπολογιστή του Azure εικονικές, πρέπει πρώτα να προσδιορίσετε το όνομα DNS που αντιστοιχεί στην υπηρεσία cloud που περιέχει την εικονική μηχανή εκτελείται διακομιστής MySQL.

1.  Στην κλασική Azure πύλη, κάντε κλικ στην επιλογή **εικονικές μηχανές**, κάντε κλικ στο όνομα του υπολογιστή εικονικές διακομιστής MySQL και, στη συνέχεια, κάντε κλικ στην επιλογή **πίνακα εργαλείων**.
2.  Από τον πίνακα εργαλείων εικονική μηχανή, σημειώστε την τιμή **Όνομα DNS** , στην ενότητα **Γρήγορη Glance** . Ακολουθεί ένα παράδειγμα:

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)

3.  Από έναν τοπικό υπολογιστή που εκτελεί MySQL ή το πρόγραμμα-πελάτη MySQL, εκτελέστε την ακόλουθη εντολή για να συνδεθείτε ως χρήστης MySQL.

        mysql -u <yourMysqlUsername> -p -h <yourDNSname>

    Για παράδειγμα, για το dbadmin3 όνομα χρήστη MySQL και το όνομα DNS testmysql.cloudapp.net για την εικονική μηχανή, χρησιμοποιήστε την ακόλουθη εντολή.

        mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με την εκτέλεση MySQL, ανατρέξτε στην [Τεκμηρίωση MySQL](http://dev.mysql.com/doc/).
