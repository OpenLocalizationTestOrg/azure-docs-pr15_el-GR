<properties
   pageTitle="Δημιουργήστε και στείλτε μια εικόνα Εικονική FreeBSD | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε και να στείλετε ένα εικονικό σκληρό δίσκο (VHD) που περιέχει το λειτουργικό σύστημα FreeBSD για να δημιουργήσετε μια εικονική μηχανή Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/29/2016"
   ms.author="kyliel"/>

# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Δημιουργία και αποστολή ενός VHD FreeBSD Azure

Σε αυτό το άρθρο σάς δείχνει πώς μπορείτε να δημιουργήσετε και να στείλετε ένα εικονικό σκληρό δίσκο (VHD) που περιέχει το λειτουργικό σύστημα FreeBSD. Αφού κάνετε αποστολή, μπορείτε να το χρησιμοποιήσετε ως τη δική σας εικόνα για να δημιουργήσετε μια εικονική μηχανή (Εικονική) στο Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Σε αυτό το άρθρο προϋποθέτει ότι έχετε τα ακόλουθα στοιχεία:

- **Συνδρομή του Azure**--Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε μία σε λίγα λεπτά. Εάν έχετε μια συνδρομή στο MSDN, ανατρέξτε στο θέμα [πιστωτικής μηνιαία Azure για τους συνδρομητές του Visual Studio.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Διαφορετικά, μάθετε πώς μπορείτε να [δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).  

- **Εργαλεία azure PowerShell**--το PowerShell Azure λειτουργική μονάδα πρέπει να εγκατασταθεί και ρυθμιστεί ώστε να χρησιμοποιεί Azure τη συνδρομή σας. Για να κάνετε λήψη της λειτουργικής μονάδας, ανατρέξτε στο θέμα [λήψη του Azure](https://azure.microsoft.com/downloads/). Ένα πρόγραμμα εκμάθησης που περιγράφει τον τρόπο εγκατάστασης και ρύθμισης παραμέτρων της λειτουργικής μονάδας είναι διαθέσιμη εδώ. Χρησιμοποιήστε το cmdlet [Azure στοιχεία λήψης](https://azure.microsoft.com/downloads/) για την αποστολή του VHD.

- Για έναν εικονικό σκληρό δίσκο, πρέπει να έχει εγκατασταθεί **FreeBSD λειτουργικό σύστημα είναι εγκατεστημένο σε ένα αρχείο .vhd**--ένα που υποστηρίζονται FreeBSD λειτουργικό σύστημα. Υπάρχουν πολλά εργαλεία για να δημιουργήσετε αρχεία .vhd. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε μια λύση virtualization όπως το Hyper-V για να δημιουργήσετε το αρχείο .vhd και να εγκαταστήσετε το λειτουργικό σύστημα. Για οδηγίες σχετικά με το πώς μπορείτε να εγκαταστήσετε και να χρησιμοποιήσετε το Hyper-V, ανατρέξτε στο θέμα [εγκατάσταση Hyper-V και να δημιουργήσετε μια εικονική μηχανή](http://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Στη νεότερη μορφή VHDX δεν υποστηρίζεται στο Azure. Μπορείτε να μετατρέψετε το δίσκο VHD μορφή χρησιμοποιώντας το Hyper-V Manager ή τα cmdlet [vhd μετατροπή](https://technet.microsoft.com/library/hh848454.aspx). Επιπλέον, υπάρχει ένα [πρόγραμμα εκμάθησης στο MSDN σχετικά με τη χρήση FreeBSD με το Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).

Αυτή η εργασία περιλαμβάνει τα παρακάτω βήματα πέντε.

## <a name="step-1-prepare-the-image-for-upload"></a>Βήμα 1: Προετοιμασία εικόνας για αποστολή

Στον υπολογιστή εικονικές όπου έχετε εγκαταστήσει το λειτουργικό σύστημα FreeBSD, ολοκληρώστε τις παρακάτω διαδικασίες:

1. Ενεργοποίηση DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart

2. Ενεργοποίηση SSH.

    SSH είναι ενεργοποιημένη από προεπιλογή μετά την εγκατάσταση από δίσκο. Εάν δεν έχει ενεργοποιηθεί για κάποιο λόγο, ή εάν χρησιμοποιείτε FreeBSD VHD απευθείας, πληκτρολογήστε τα εξής:

        # echo 'sshd_enable="YES"' >> /etc/rc.conf
        # ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
        # ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
        # service sshd restart

3. Ρυθμίστε μια σειρά κονσόλα.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. Εγκαταστήστε το sudo.

    Ο λογαριασμός ρίζας είναι απενεργοποιημένη στο Azure. Αυτό σημαίνει ότι πρέπει να χρησιμοποιούν sudo από έναν χωρίς δικαιώματα χρήστη για την εκτέλεση εντολών με αναβαθμισμένα δικαιώματα.

        # pkg install sudo
;
5. Προαπαιτούμενα για Azure παράγοντα.

        # pkg install python27  
        # pkg install Py27-setuptools27   
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git

6. Εγκαταστήστε το Azure παράγοντα.

    Την πιο πρόσφατη έκδοση του Azure παράγοντα πάντα μπορεί να βρεθεί στην [github](https://github.com/Azure/WALinuxAgent/releases). Το 2.0.10 + επίσημα υποστηρίζει FreeBSD 10 & 10.1, και την έκδοση 2.1.4 υποστηρίζει επίσημα FreeBSD 10.2 και νεότερες εκδόσεις.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    Για 2.0, ας χρησιμοποιήσουμε 2.0.16 ως παράδειγμα:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    Για 2.1, ας χρησιμοποιήσουμε 2.1.4 ως παράδειγμα:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

    >[AZURE.IMPORTANT] Μετά την εγκατάσταση του Azure παράγοντας, είναι καλή ιδέα να βεβαιωθείτε ότι εκτελείται:

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # service –e | grep waagent
        /etc/rc.d/waagent
        # cat /var/log/waagent.log

7. Deprovision του συστήματος.

    Deprovision το σύστημα για να καθαρίσετε και να είναι κατάλληλη για την προμήθεια του εκ νέου. Η ακόλουθη εντολή διαγράφει επίσης το τελευταίο λογαριασμό χρήστη προμήθεια του φακέλου και τα συσχετισμένα δεδομένα:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Τώρα μπορείτε να τερματίσετε την Εικονική.

## <a name="step-2-create-a-storage-account-in-azure"></a>Βήμα 2: Δημιουργία λογαριασμού χώρου αποθήκευσης στο Azure ##

Χρειάζεστε ένα λογαριασμό χώρου αποθήκευσης στο Azure για να αποστείλετε ένα αρχείο .vhd, ώστε να μπορεί να χρησιμοποιηθεί για να δημιουργήσετε μια εικονική μηχανή. Μπορείτε να χρησιμοποιήσετε το Azure κλασική πύλη για να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης.

1. Πραγματοποιήστε είσοδο στο [Azure κλασική πύλη](https://manage.windowsazure.com).

2. Στη γραμμή εντολών, επιλέξτε **Δημιουργία**.

3. Επιλέξτε **υπηρεσίες δεδομένων** > **χώρου αποθήκευσης** > **γρήγορης δημιουργίας**.

    ![Γρήγορη δημιουργία λογαριασμού χώρου αποθήκευσης](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-quick-create.png)

4. Συμπληρώστε τα πεδία ως εξής:

    - Στο πεδίο **διεύθυνση URL** , πληκτρολογήστε ένα όνομα δευτερεύοντος τομέα για να χρησιμοποιήσετε τη διεύθυνση URL του λογαριασμού χώρου αποθήκευσης. Η εγγραφή μπορεί να περιέχει από 3-24 αριθμούς και πεζών γραμμάτων. Αυτό το όνομα είναι το όνομα κεντρικού υπολογιστή εντός της διεύθυνσης URL που χρησιμοποιείται για την αντιμετώπιση χώρος αποθήκευσης αντικειμένων Blob του Azure, αποθήκευση ουράς Azure ή πίνακα Azure πόρων αποθήκευσης για τη συνδρομή.

    - Στο μενού αναπτυσσόμενη **Ομάδα θέση/συνάφειας** , επιλέξτε τη **θέση ή ομάδα συσχέτισης** για το λογαριασμό χώρου αποθήκευσης. Μια ομάδα συσχέτισης σάς επιτρέπει να τοποθετήσετε τις υπηρεσίες cloud και χώρου αποθήκευσης στο ίδιο κέντρο δεδομένων.

    - Στο πεδίο **αναπαραγωγής** , αποφασίσετε εάν θα χρησιμοποιήσετε **Παν πλεονάζοντα** αναπαραγωγής για το λογαριασμό χώρου αποθήκευσης. Αναπαραγωγή παν είναι ενεργοποιημένη από προεπιλογή. Αυτή η επιλογή αναπαράγει τα δεδομένα σας σε μια δευτερεύουσα θέση, χωρίς κόστος, έτσι ώστε το χώρο αποθήκευσης αποτυγχάνει πάνω από σε αυτήν τη θέση, σε περίπτωση κύρια διακοπής στην κύρια θέση. Η δευτερεύουσα θέση αντιστοιχίζεται αυτόματα και δεν μπορεί να αλλάξει. Εάν χρειάζεστε περισσότερο έλεγχο στη θέση του του χώρου αποθήκευσης που βασίζεται στο cloud, λόγω νομικές απαιτήσεις ή την εταιρική πολιτική, μπορείτε να απενεργοποιήσετε παν αναπαραγωγής. Ωστόσο, έχετε υπόψη ότι εάν αργότερα ενεργοποιήσετε παν αναπαραγωγής, που θα χρεωθεί χρέωση μεταφοράς εφάπαξ δεδομένων για να αναπαραγάγετε τα υπάρχοντα δεδομένα στη δευτερεύουσα θέση. Υπηρεσίες αποθήκευσης χωρίς αλληλεπίδραση παν παρέχεται με έκπτωση. Περισσότερες λεπτομέρειες σχετικά με τη Διαχείριση παν αναπαραγωγής λογαριασμών χώρου αποθήκευσης θα βρείτε εδώ: [Δημιουργία, διαχείριση, ή διαγραφή ενός λογαριασμού χώρου αποθήκευσης](../storage-create-storage-account/#replication-options).

    ![Πληκτρολογήστε τα στοιχεία λογαριασμού χώρου αποθήκευσης](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-create-account.png)


5. Επιλέξτε **Δημιουργία λογαριασμού χώρου αποθήκευσης**. Ο λογαριασμός εμφανίζεται τώρα στην περιοχή **χώρου αποθήκευσης**.

    ![Επιτυχής Δημιουργία λογαριασμού χώρου αποθήκευσης](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storagenewaccount.png)

6. Στη συνέχεια, δημιουργήστε ένα κοντέινερ για τα αρχεία σας έχουν αποσταλεί .vhd. Επιλέξτε το όνομα του λογαριασμού χώρου αποθήκευσης και, στη συνέχεια, επιλέξτε **κοντέινερ**.

    ![Λεπτομέρειες λογαριασμού χώρου αποθήκευσης](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_detail.png)

7. Επιλέξτε **Δημιουργία κοντέινερ**.

    ![Λεπτομέρειες λογαριασμού χώρου αποθήκευσης](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_container.png)

8. Στο πεδίο **όνομα** , πληκτρολογήστε ένα όνομα για το κοντέινερ. Στη συνέχεια, στο αναπτυσσόμενο μενού **πρόσβασης** , επιλέξτε τον τύπο της πολιτικής πρόσβασης που θέλετε.

    ![Το όνομα του κοντέινερ](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] Από προεπιλογή, το κοντέινερ είναι ιδιωτική και είναι δυνατή η πρόσβαση μόνο από τον κάτοχο του λογαριασμού. Για να επιτρέψετε δημόσια πρόσβαση για ανάγνωση σε τα αντικείμενα blob στο κοντέινερ, αλλά όχι τις ιδιότητες κοντέινερ και μετα-δεδομένων, χρησιμοποιήστε την επιλογή **Δημόσιο Blob** . Για να επιτρέψετε πλήρη δημόσια πρόσβαση για ανάγνωση για το κοντέινερ και αντικείμενα blob, χρησιμοποιήστε την επιλογή **Δημόσιο κοντέινερ** .

## <a name="step-3-prepare-the-connection-to-azure"></a>Βήμα 3: Προετοιμασία της σύνδεσης με Azure

Πριν να αποστείλετε ένα αρχείο .vhd, πρέπει να δημιουργήσει μια ασφαλή σύνδεση μεταξύ του υπολογιστή και Azure τη συνδρομή σας. Για να το κάνετε αυτό, μπορείτε να χρησιμοποιήσετε τη μέθοδο του Azure Active Directory (Azure AD) ή τη μέθοδο του πιστοποιητικού.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Χρησιμοποιήστε τη μέθοδο Azure AD για να αποστείλετε ένα αρχείο .vhd

1. Ανοίξτε την κονσόλα Azure PowerShell.

2. Πληκτρολογήστε την ακόλουθη εντολή:  
    `Add-AzureAccount`

    Αυτή η εντολή ανοίγει ένα παράθυρο εισόδου όπου μπορείτε να συνδεθείτε με το λογαριασμό εργασίας ή του σχολείου.

    ![Παράθυρο του PowerShell](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/add_azureaccount.png)

3. Azure πραγματοποιεί έλεγχο ταυτότητας και αποθηκεύει τις πληροφορίες διαπιστευτηρίων. Στη συνέχεια, κλείνει το παράθυρο.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Χρησιμοποιήστε τη μέθοδο πιστοποιητικό για να αποστείλετε ένα αρχείο .vhd

1. Ανοίξτε την κονσόλα Azure PowerShell.

2. Τύπος:  `Get-AzurePublishSettingsFile`.

3. Ένα παράθυρο προγράμματος περιήγησης ανοίγει και σας ζητά να κάνετε λήψη ενός αρχείου .publishsettings. Αυτό το αρχείο περιέχει πληροφορίες και ένα πιστοποιητικό για τη συνδρομή σας στο Azure.

    ![Σελίδα λήψης του προγράμματος περιήγησης](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. Αποθηκεύστε το αρχείο .publishsettings.

4. Τύπος:  `Import-AzurePublishSettingsFile <PathToFile>`, όπου `<PathToFile>` είναι η πλήρης διαδρομή προς το αρχείο .publishsettings.

   Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το cmdlet του Azure](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση και ρύθμιση παραμέτρων του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

## <a name="step-4-upload-the-vhd-file"></a>Βήμα 4: Αποστολή του αρχείου .vhd

Όταν κάνετε αποστολή του αρχείου .vhd, μπορείτε να το τοποθετήσετε σε οποιοδήποτε σημείο μέσα αποθήκευσης αντικειμένων Blob του. Ακολουθούν ορισμένες όρους που θα χρησιμοποιήσετε κατά την αποστολή του αρχείου:
-  **BlobStorageURL** είναι η διεύθυνση URL για το λογαριασμό χώρου αποθήκευσης που δημιουργήσατε στο βήμα 2.
-  **YourImagesFolder** είναι το κοντέινερ μέσα αποθήκευσης αντικειμένων Blob όπου θέλετε να αποθηκεύσετε τις εικόνες σας.
- **VHDName** είναι η ετικέτα που εμφανίζεται στην πύλη του Azure κλασική για τον προσδιορισμό του εικονικού σκληρού δίσκου.
- **PathToVHDFile** είναι η πλήρης διαδρομή και το όνομα του αρχείου .vhd.


Από το παράθυρο Azure PowerShell που χρησιμοποιήσατε στο προηγούμενο βήμα, πληκτρολογήστε:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Βήμα 5: Δημιουργήστε μια Εικονική με το αρχείο αποσταλεί .vhd
Μετά την αποστολή του αρχείου .vhd, μπορείτε να την προσθέσετε ως εικόνα στη λίστα των προσαρμοσμένων εικόνων που σχετίζονται με τη συνδρομή σας και να δημιουργήσετε μια εικονική μηχανή με αυτήν την προσαρμοσμένη εικόνα.

1. Από το παράθυρο Azure PowerShell που χρησιμοποιήσατε στο προηγούμενο βήμα, πληκτρολογήστε:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    > [AZURE.NOTE]Χρησιμοποιήστε Linux ως τον τύπο του λειτουργικού Συστήματος. Η τρέχουσα έκδοση του PowerShell Azure δέχεται μόνο "Linux" ή "Των Windows" ως παράμετρο.

2. Αφού ολοκληρώσετε τα προηγούμενα βήματα, η νέα εικόνα εμφανίζεται όταν επιλέγετε την καρτέλα **εικόνες** στην πύλη του Azure κλασική.  

    ![Επιλέξτε μια εικόνα](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Δημιουργήστε μια εικονική μηχανή από τη συλλογή. Αυτή η νέα εικόνα είναι πλέον διαθέσιμη στην περιοχή **Εικόνες μου**.
4. Επιλέξτε τη νέα εικόνα. Στη συνέχεια, μεταβείτε μέσω τις οδηγίες για να ρυθμίσετε ένα όνομα κεντρικού υπολογιστή, τον κωδικό πρόσβασης, SSH κλειδί και ούτω καθεξής.

    ![Προσαρμοσμένη εικόνα](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Αφού ολοκληρώσετε την προμήθεια του, θα δείτε την Εικονική FreeBSD εκτελείται στο Azure.

    ![Εικόνα FreeBSD στο azure](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/freebsdimageinazure.png)
