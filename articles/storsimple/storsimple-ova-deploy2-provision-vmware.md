<properties
   pageTitle="Ανάπτυξη πίνακα εικονικού StorSimple - διάταξη στο VMware"
   description="Αυτό το πρόγραμμα εκμάθησης δεύτερο πίνακα εικονικού StorSimple ανάπτυξης σειράς περιλαμβάνει την προμήθεια εικονική συσκευή στο VMware."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/12/2016"
   ms.author="alkohli"/>


# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-vmware"></a>Ανάπτυξη πίνακα εικονικού StorSimple - προμήθεια μια εικονική πίνακας VMware

![](./media/storsimple-ova-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Επισκόπηση 
Αυτό το πρόγραμμα εκμάθησης παροχής ισχύει έκδοση γενικής διαθεσιμότητας (GA) εκτελείται 2016 Μαρτίου StorSimple εικονικού πίνακες (γνωστό και ως εικονικές συσκευές εσωτερικής StorSimple ή StorSimple εικονικές συσκευές). Αυτό το πρόγραμμα εκμάθησης περιγράφει πώς να προμηθεύσουν και να συνδεθείτε με έναν πίνακα εικονικού StorSimple σε ένα σύστημα κεντρικού υπολογιστή που εκτελεί VMware ESXi 5,5 και επάνω από. Σε αυτό το άρθρο ισχύει για την ανάπτυξη των πινάκων εικονικού StorSimple στην πύλη του Azure κλασική, καθώς και Microsoft Azure στο Cloud για δημόσιους.

Θα χρειαστείτε δικαιώματα διαχειριστή για την παροχή των και σύνδεση με μια εικονική συσκευή. Το παροχής και την αρχική ρύθμιση μπορεί να διαρκέσει περίπου 10 λεπτά για να ολοκληρωθεί.


## <a name="provisioning-prerequisites"></a>Προμήθεια τις προϋποθέσεις

Εδώ θα βρείτε τις προϋποθέσεις για την παροχή μια εικονική συσκευή σε ένα σύστημα κεντρικού υπολογιστή που εκτελεί VMware ESXi 5,5 και παραπάνω.

### <a name="for-the-storsimple-manager-service"></a>Για την υπηρεσία διαχείρισης StorSimple

Πριν ξεκινήσετε, βεβαιωθείτε ότι:

-   Ολοκληρώσατε όλα τα βήματα στο θέμα [Προετοιμασία της πύλης για StorSimple εικονικό πίνακα](storsimple-ova-deploy1-portal-prep.md).

-   Έχετε λάβει την εικόνα εικονικού συσκευής για VMware από την πύλη του Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [βήμα 3: λήψη της εικόνας εικονική συσκευή](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

### <a name="for-the-storsimple-virtual-device"></a>Για τη συσκευή εικονικού StorSimple 

Πριν αναπτύξετε μια εικονική συσκευή, βεβαιωθείτε ότι:

-   Έχετε πρόσβαση σε ένα σύστημα κεντρικού υπολογιστή που εκτελεί το Hyper-V (2008 R2 ή νεότερη έκδοση) που μπορεί να χρησιμοποιούνται για την παροχή μια συσκευή.

-   Το σύστημα υποδοχής είναι σε θέση να χρησιμοποιηθεί στους παρακάτω πόρους για την προμήθεια εικονική συσκευή σας:

    -   Τουλάχιστον τεσσάρων πυρήνων.

    -   Τουλάχιστον 8 GB RAM.

    -   Περιβάλλον εργασίας ένα δίκτυο.

    -   Μια 500 GB εικονικού δίσκου για δεδομένα του συστήματος.

### <a name="for-the-network-in-datacenter"></a>Για το δίκτυο στο κέντρο δεδομένων 

Πριν ξεκινήσετε, βεβαιωθείτε ότι:

-   Να αναθεωρήσετε τις απαιτήσεις κοινωνικής δικτύωσης για την ανάπτυξη StorSimple εικονική συσκευή και να ρυθμίσετε το δίκτυο του κέντρου δεδομένων σύμφωνα με τις απαιτήσεις. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [απαιτήσεις συστήματος για το εικονικό πίνακα StorSimple](storsimple-ova-system-requirements.md).

## <a name="step-by-step-provisioning"></a>Προμήθεια βήμα προς βήμα 

Για να προμηθεύσουν και να συνδεθείτε με μια εικονική συσκευή, θα πρέπει να εκτελέσετε τα παρακάτω βήματα:

1.  Βεβαιωθείτε ότι το σύστημα κεντρικού υπολογιστή έχει επαρκείς πόρους για να πληροί τις απαιτήσεις ελάχιστη εικονική συσκευή.

2.  Προμήθεια εικονική συσκευή στο σας υπερεπόπτη.

3.  Ξεκινήστε την εικονική συσκευή και λάβετε τη διεύθυνση IP.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Βήμα 1: Βεβαιωθείτε ότι κεντρικού συστήματος πληροί απαιτήσεις ελάχιστη εικονική συσκευή

Για να δημιουργήσετε μια εικονική συσκευή, θα πρέπει:

-   Πρόσβαση σε ένα σύστημα κεντρικού υπολογιστή που εκτελεί VMware ESXi διακομιστή 5,5 και παραπάνω.

-   VMware vSphere προγράμματος-πελάτη του συστήματός σας για να διαχειριστείτε τον κεντρικό υπολογιστή του ESXi.

    -   Τουλάχιστον τεσσάρων πυρήνων.

    -   Τουλάχιστον 8 GB RAM.

    -   Ένα περιβάλλον εργασίας δικτύου συνδεδεμένοι στο δίκτυο με δυνατότητα δρομολόγησης κίνησης στο Internet. Το ελάχιστο εύρος ζώνης Internet πρέπει να είναι 5 Mbps για να επιτρέψετε για βέλτιστη λειτουργία της συσκευής.

    -   Μια 500 GB εικονικού δίσκου για τα δεδομένα.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Βήμα 2: Προμήθεια εικονική συσκευή στο υπερεπόπτη

Εκτελέστε τα ακόλουθα βήματα για την παροχή μια εικονική συσκευή στο σας υπερεπόπτη.

1.  Αντιγράψτε την εικόνα εικονικού συσκευής στο σύστημά σας. Αυτή είναι η εικόνα που έχετε λάβει μέσω της Azure κλασική πύλης. 
    1.  Βεβαιωθείτε ότι αυτό είναι το πιο πρόσφατο αρχείο εικόνας που έχετε λάβει. Εάν έχετε κάνει λήψη της εικόνας νωρίτερα, κάντε λήψη του ξανά για να βεβαιωθείτε ότι έχετε την πιο πρόσφατη εικόνα. Η πιο πρόσφατη εικόνα έχει δύο αρχεία (αντί για ένα).
    2.  Σημειώστε τη θέση όπου αντιγράψατε την εικόνα που θα χρησιμοποιήσετε αυτό αργότερα στη διαδικασία.

2.  Συνδεθείτε στο διακομιστή ESXi χρησιμοποιώντας το πρόγραμμα-πελάτη vSphere. Θα πρέπει να έχετε δικαιώματα διαχειριστή για να δημιουργήσετε μια εικονική μηχανή.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image1.png)

1.  Στο πρόγραμμα-πελάτη vSphere, στην ενότητα απόθεμα στο αριστερό τμήμα του παραθύρου, επιλέξτε το διακομιστή ESXi.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image2.png)

1.  Που θα αποστείλετε πρώτα το VMDK στο διακομιστή ESXi. Μεταβείτε στην καρτέλα **παράμετροι** στο δεξιό παράθυρο. Στην περιοχή **υλικό**, επιλέξτε **χώρου αποθήκευσης**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image3.png)

1.  Στο δεξιό παράθυρο, στην περιοχή **Datastores**, επιλέξτε το αποθήκευσης δεδομένων στον οποίο θέλετε να αποστείλετε το VMDK. Το αποθήκευσης δεδομένων πρέπει να έχετε αρκετό ελεύθερο χώρο για το λειτουργικό σύστημα και δεδομένων δίσκο.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image4.png)

1.  Κάντε δεξί κλικ και επιλέξτε **Αναζήτηση αποθήκευσης δεδομένων**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image5.png)

1.  Θα εμφανιστεί ένα παράθυρο **Προγράμματος περιήγησης αποθήκευσης δεδομένων** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image6.png)

1.  Στη γραμμή εργαλείων, κάντε κλικ στην επιλογή ![](./media/storsimple-ova-deploy2-provision-vmware/image7.png) εικονίδιο για να δημιουργήσετε ένα νέο φάκελο. Καθορίστε το όνομα του φακέλου και σημειώστε την. Θα χρησιμοποιήσετε αυτό το όνομα φακέλου αργότερα κατά τη δημιουργία μια εικονική μηχανή (συνιστάται βέλτιστη πρακτική). Κάντε κλικ στο **κουμπί OK**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image8.png)

1.  Ο νέος φάκελος θα εμφανίζεται στο αριστερό τμήμα του παραθύρου του προγράμματος **Περιήγησης αποθήκευσης δεδομένων**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image9.png)

1.  Κάντε κλικ στο εικονίδιο αποστολής ![](./media/storsimple-ova-deploy2-provision-vmware/image10.png) και επιλέξτε **Αποστολή αρχείου**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image11.png)

1.  Τώρα θα πρέπει να μεταβείτε και παραπέμπουν στα αρχεία VMDK που έχετε λάβει. Θα υπάρχουν δύο αρχεία. Επιλέξτε ένα αρχείο για να αποστείλετε.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image12m.png)

1.  Κάντε κλικ στην επιλογή **Άνοιγμα**. Τώρα, αυτό θα ξεκινήσει η αποστολή του αρχείου VMDK για την καθορισμένη αποθήκευσης δεδομένων. Ενδέχεται να χρειαστούν αρκετά λεπτά για το αρχείο για να αποστείλετε.


1.  Όταν ολοκληρωθεί η αποστολή, θα δείτε το αρχείο σε το αποθήκευσης δεδομένων στο φάκελο που δημιουργήσατε. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image14.png)

    Τώρα θα πρέπει να αποστείλετε το δεύτερο αρχείο VMDK για την ίδια αποθήκευσης δεδομένων.

1.  Επιστρέψτε στο παράθυρο του προγράμματος-πελάτη vSphere. Με επιλεγμένο το διακομιστή ESXi, κάντε δεξί κλικ και επιλέξτε **Νέα εικονική μηχανή**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image15.png)

1.  Θα εμφανιστεί ένα παράθυρο **Δημιουργία νέου εικονική μηχανή** . Στη σελίδα **Ρύθμιση παραμέτρων** , ενεργοποιήστε την επιλογή **Προσαρμογή** . Κάντε κλικ στο κουμπί **Επόμενο**.
    ![](./media/storsimple-ova-deploy2-provision-vmware/image16.png)

2.  Στη σελίδα **όνομα και τη θέση** , καθορίστε το όνομα του υπολογιστή σας εικονική. Αυτό το όνομα πρέπει να συμφωνεί με το όνομα του φακέλου (συνιστάται βέλτιστη πρακτική) που ορίσατε προηγουμένως στο βήμα 8.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image17.png)

1.  Στη σελίδα **χώρος αποθήκευσης** , επιλέξτε μια αποθήκευσης δεδομένων που θέλετε να χρησιμοποιήσετε για την προμήθεια του Εικονική.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image18.png)

1.  Στη σελίδα **Εικονική μηχανή έκδοση** , επιλέξτε **εικονική μηχανή έκδοση: 8**. Σημειώστε ότι εκδόσεις 8 έως 11 υποστηρίζονται.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image19.png)

1.  Στη σελίδα **φιλοξενούμενο λειτουργικό σύστημα** , επιλέξτε το **λειτουργικό σύστημα επισκέπτη** ως **των Windows**. Για την **έκδοση**, από την αναπτυσσόμενη λίστα, επιλέξτε **Microsoft Windows Server 2012 (64 bit)**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image20.png)

1.  Στη σελίδα **υποστήριξη CPU** , προσαρμόστε τον **αριθμό των εικονικού sockets** και **Αριθμός πυρήνων ανά εικονικό socket** έτσι ώστε να είναι ο **Συνολικός αριθμός πυρήνων** 4 (ή περισσότερα). Κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image21.png)

1.  Στη σελίδα **μνήμης** , καθορίστε 8 GB (ή περισσότερα) μνήμη RAM. Κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image22.png)

1.  Στη σελίδα **δικτύου** , καθορίστε τον αριθμό των διασυνδέσεων δικτύου. Η ελάχιστη απαίτηση είναι ένα περιβάλλον εργασίας δικτύου.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image23.png)

1.  Στη σελίδα **Ελεγκτή SCSI** , αποδεχτείτε την προεπιλεγμένη **συσχετισμών Ασφαλείας λογικής LSI ελεγκτή**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image24.png)

1.  Στη σελίδα **Επιλέξτε ένα δίσκο** , επιλέξτε **χρήση ενός υπάρχοντος εικονικού δίσκου**. Κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image25.png)

1.  Στη σελίδα **Επιλέξτε υπάρχον δίσκου** , στην περιοχή **Δίσκου διαδρομή του αρχείου**, κάντε κλικ στην επιλογή **Αναζήτηση**. Έτσι ανοίγει ένα παράθυρο διαλόγου **Αναζήτηση Datastores** . Μεταβείτε στη θέση όπου έχετε αποστείλει το VMDK. Τώρα βλέπετε μόνο ένα αρχείο σε το αποθήκευσης δεδομένων κατά τη συγχώνευση δύο αρχεία που έχετε αποστείλει αρχικά. Επιλέξτε το αρχείο και κάντε κλικ στο **κουμπί OK**. Κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image26.png)

1.  Στη σελίδα **Επιλογές για προχωρημένους** , αποδεχτείτε την προεπιλογή και κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image27.png)

1.  Στη σελίδα **έτοιμο ολοκληρώθηκε** , εξετάστε όλες τις ρυθμίσεις που σχετίζονται με τη νέα εικονική μηχανή. Επιλέξτε **Επεξεργασία των ρυθμίσεων εικονική μηχανή πριν από την ολοκλήρωση**. Κάντε κλικ στο κουμπί **συνέχεια**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image28.png)

1.  Στη σελίδα **Ιδιότητες εικονικές μηχανές** , στην καρτέλα **υλικό** , εντοπίστε το υλικό της συσκευής. Επιλέξτε **νέο σκληρό δίσκο**. Κάντε κλικ στην επιλογή **Προσθήκη**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image29.png)

1.  Αυτόν τον τρόπο εμφανίζεται το παράθυρο " **Προσθήκη υλικού** ". Στη σελίδα **Τύπος συσκευής** , στην περιοχή **Επιλέξτε τον τύπο της συσκευής που θέλετε να προσθέσετε**, επιλέξτε **σκληρό δίσκο** και κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image30.png)

1.  Στη σελίδα **Επιλέξτε ένα δίσκο** , επιλέξτε **Δημιουργία ενός νέου εικονικού δίσκου**. Κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image31.png)

1.  Στη σελίδα **Δημιουργία ένα δίσκο** , αλλάξτε το **Μέγεθος του δίσκου** 500 GB (ή περισσότερα). Στην περιοχή **Προμήθεια του δίσκου**, επιλέξτε **Λεπτό διάταξη**. Κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image32.png)

1.  Στη σελίδα **Επιλογές για προχωρημένους** , αποδεχτείτε την προεπιλεγμένη.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image33.png)

1.  Στη σελίδα **έτοιμο ολοκληρώθηκε** , εξετάστε τις επιλογές δίσκου. Κάντε κλικ στο κουμπί **Τέλος**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image34.png)

1.  Τώρα θα επιστρέψετε στη σελίδα Ιδιότητες εικονική μηχανή. Προστίθεται ένα νέο σκληρό δίσκο σας εικονική μηχανή. Κάντε κλικ στο κουμπί **Τέλος**.
  
    ![](./media/storsimple-ova-deploy2-provision-vmware/image35.png)

2.  Με την εικονική μηχανή επιλεγμένο στο δεξιό παράθυρο, μεταβείτε στην καρτέλα **Σύνοψη** . Εξετάστε τις ρυθμίσεις για την εικονική μηχανή σας.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image36.png)

Το εικονικό μηχάνημα παρέχεται τώρα. Το επόμενο βήμα είναι να λάβετε τη διεύθυνση IP και power σε αυτόν τον υπολογιστή.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Βήμα 3: Ξεκινήστε τη συσκευή εικονικού και λάβετε το IP

Ακολουθήστε τα παρακάτω βήματα για να ξεκινήσετε εικονικού τη συσκευή σας και να συνδεθείτε.

#### <a name="to-start-the-virtual-device"></a>Για να ξεκινήσετε την εικονική συσκευή

1.  Ξεκινήστε την εικονική συσκευή. Στο το vSphere Configuration Manager, στο αριστερό παράθυρο, επιλέξτε τη συσκευή σας και κάντε δεξί κλικ για να εμφανίσετε το μενού περιβάλλοντος. Επιλέξτε **Power** και, στη συνέχεια, επιλέξτε **Power στην**. Αυτό θα πρέπει να power στον υπολογιστή σας εικονική. Μπορείτε να προβάλετε την κατάσταση στο παράθυρο **Πρόσφατες εργασίες** κάτω από το πρόγραμμα-πελάτη vSphere.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image37.png)

1.  Οι εργασίες εγκατάστασης θα χρειαστούν μερικά λεπτά για να ολοκληρωθεί. Όταν η συσκευή διαθέτει, μεταβείτε στην καρτέλα **κονσόλας** . Στείλτε το συνδυασμό πλήκτρων Ctrl + Alt + Delete για να συνδεθείτε στη συσκευή. Εναλλακτικά, μπορείτε να τοποθετήσετε το δρομέα στο παράθυρο της κονσόλας και πατήστε το συνδυασμό πλήκτρων Ctrl + Alt + Insert. Ο προεπιλεγμένος χρήστης είναι *StorSimpleAdmin* και τον κωδικό πρόσβασης του προεπιλεγμένου *κωδικού πρόσβασης1*.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image38.png)

1.  Για λόγους ασφαλείας, τον κωδικό πρόσβασης διαχειριστή συσκευής λήγει το πρώτο αρχείο καταγραφής στην. Θα σας ζητηθεί να αλλάξετε τον κωδικό πρόσβασης.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image39.png)

1.  Πληκτρολογήστε έναν κωδικό πρόσβασης που περιέχει τουλάχιστον 8 χαρακτήρες. Ο κωδικός πρόσβασης πρέπει να περιέχει 3 εκτός 4 από αυτές τις απαιτήσεις: κεφαλαία, πεζά, αριθμητικά και ειδικούς χαρακτήρες. Πληκτρολογήστε ξανά τον κωδικό πρόσβασης για να τον επιβεβαιώσετε. Που θα ειδοποιηθούν ότι έχει αλλάξει τον κωδικό πρόσβασης.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image40.png)

1.  Μετά την επιτυχημένη έχει αλλάξει τον κωδικό πρόσβασης, ενδέχεται να επανεκκινήστε τη συσκευή εικονικού. Περιμένετε για την επανεκκίνηση του υπολογιστή για να ολοκληρωθεί. Το Windows PowerShell κονσόλας της συσκευής ενδέχεται να εμφανίζονται μαζί με μια γραμμή προόδου.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image41.png)

1.  Τα βήματα 6-8 ισχύουν μόνο κατά την εκκίνηση προς τα επάνω σε ένα περιβάλλον μη DHCP. Εάν είστε σε ένα περιβάλλον DHCP, στη συνέχεια, παραλείψτε αυτά τα βήματα και μεταβείτε στο βήμα 9. Εάν εκτελέσατε εκκίνηση του στο περιβάλλον μη DHCP τη συσκευή σας, θα δείτε την παρακάτω οθόνη. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image42m.png)

    Τώρα θα πρέπει να ρυθμίσετε τις παραμέτρους του δικτύου.

1.  Χρησιμοποιήστε το `Get-HcsIpAddress` εντολή για να παραθέσετε τις διασυνδέσεις δικτύου με δυνατότητα στη συσκευή σας εικονικού. Εάν η συσκευή σας έχει ένα περιβάλλον εργασίας ενιαίου δικτύου με δυνατότητα, το προεπιλεγμένο όνομα που έχουν εκχωρηθεί σε αυτήν τη διασύνδεση είναι `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image43m.png)

1.  Χρησιμοποιήστε το `Set-HcsIpAddress` cmdlet για να ρυθμίσετε τις παραμέτρους του δικτύου. Παράδειγμα φαίνεται παρακάτω:


    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-vmware/image44.png)

1.  Αφού ολοκληρωθεί η αρχική ρύθμιση και τη συσκευή έχει εκκίνηση προς τα επάνω, θα δείτε το κείμενο διαφημιστικού πλαισίου συσκευή. Σημειώστε τη διεύθυνση IP και τη διεύθυνση URL που εμφανίζεται στο πλαίσιο κειμένου για να διαχειριστείτε τη συσκευή. Θα χρησιμοποιήσετε αυτήν τη διεύθυνση IP για να συνδεθείτε με το περιβάλλον εργασίας Χρήστη της συσκευής σας εικονικού web και να ολοκληρώσετε την τοπική ρύθμιση και την καταγραφή.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image45.png)


1. (Προαιρετικό) Εκτέλεση αυτού του βήματος μόνο εάν αναπτύσσετε τη συσκευή σας στο Cloud για δημόσιους οργανισμούς. Τώρα θα ενεργοποιήσετε τη λειτουργία Ηνωμένες Πολιτείες Federal πληροφορίες επεξεργασίας τυπική (FIPS) στη συσκευή σας. Το πρότυπο FIPS 140 ορίζει αλγορίθμους κρυπτογράφησης εγκριθεί για χρήση από τα συστήματα υπολογιστών για δημόσιους οργανισμούς Federal ΜΑΣ για την προστασία των ευαίσθητων δεδομένων.
    1. Για να ενεργοποιήσετε τη λειτουργία FIPS, εκτελέστε το ακόλουθο cmdlet:
        
        `Enter-HcsFIPSMode`

    2. Επανεκκινήστε τη συσκευή σας μετά την ενεργοποίηση της λειτουργίας FIPS, έτσι ώστε το κρυπτογράφησης επικυρώσεων τεθούν σε ισχύ.

        > [AZURE.NOTE] Μπορείτε να ενεργοποιήσετε ή να απενεργοποιήσετε τη λειτουργία FIPS στη συσκευή σας. Εναλλαγή της συσκευής μεταξύ της λειτουργίας FIPS και μη FIPS δεν υποστηρίζεται.


Εάν η συσκευή σας δεν πληροί τις απαιτήσεις ελάχιστες ρύθμισης παραμέτρων, θα εμφανιστεί ένα σφάλμα στο κείμενο διαφημιστικού πλαισίου (απεικονίζεται παρακάτω). Θα πρέπει να τροποποιήσετε τις παραμέτρους της συσκευής, έτσι ώστε να έχει επαρκείς πόρους για να πληροί τις ελάχιστες απαιτήσεις. Στη συνέχεια, για να επανεκκινήσετε και συνδεθείτε στη συσκευή. Αναφορά ρύθμισης παραμέτρων ελάχιστες απαιτήσεις στο [βήμα 1: Βεβαιωθείτε ότι το κεντρικό σύστημα πληροί απαιτήσεις ελάχιστες εικονική συσκευή](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-vmware/image46.png)

Εάν αντιμετωπίσετε οποιοδήποτε άλλο σφάλμα κατά την αρχική ρύθμιση παραμέτρων χρησιμοποιώντας το τοπικό web περιβάλλοντος εργασίας Χρήστη, ανατρέξτε στο τις παρακάτω ροές εργασίας [Διαχείριση σας StorSimple εικονικού πίνακα](storsimple-ova-web-ui-admin.md)χρησιμοποιώντας το τοπικό web περιβάλλοντος εργασίας Χρήστη.

-   Εκτέλεση δοκιμών διαγνωστικών για την [Αντιμετώπιση προβλημάτων ρύθμισης περιβάλλοντος εργασίας Χρήστη web](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).

-   [Δημιουργία πακέτου καταγραφής και προβολή των αρχείων καταγραφής](storsimple-ova-web-ui-admin.md#generate-a-log-package)...

## <a name="next-steps"></a>Επόμενα βήματα

-   [Ρύθμιση του πίνακα εικονικού StorSimple ως διακομιστή αρχείων](storsimple-ova-deploy3-fs-setup.md)

-   [Ρύθμιση του πίνακα εικονικού StorSimple ως διακομιστής iSCSI](storsimple-ova-deploy3-iscsi-setup.md)
