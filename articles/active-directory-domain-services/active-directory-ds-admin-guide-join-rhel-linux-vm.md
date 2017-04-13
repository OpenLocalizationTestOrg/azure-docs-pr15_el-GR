<properties
    pageTitle="Azure υπηρεσίες τομέα Active Directory: Συμμετοχή σε μια Εικονική RHEL σε διαχειριζόμενες τομέα | Microsoft Azure"
    description="Συμμετοχή σε μια εικονική μηχανή κόκκινο καπέλο Enterprise Linux στις υπηρεσίες τομέα AD Azure"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Συμμετοχή σε μια εικονική μηχανή κόκκινο καπέλο Enterprise Linux 7 σε διαχειριζόμενες τομέα
Αυτό το άρθρο σας δείχνει πώς μπορείτε να συμμετάσχετε σε μια εικονική μηχανή κόκκινο καπέλο Enterprise Linux (RHEL) 7 σε έναν τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Προμήθεια μια εικονική μηχανή Linux Enterprise κόκκινο καπέλο
Εκτελέστε τα ακόλουθα βήματα για την παροχή μια εικονική μηχανή RHEL 7 με την πύλη Azure.

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com).

    ![Azure πύλης πίνακα εργαλείων](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. Κάντε κλικ στην επιλογή " **Δημιουργία** " στο αριστερό τμήμα του παραθύρου και πληκτρολογήστε **Καπέλο κόκκινο χρώμα** στη γραμμή αναζήτησης, όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης. Καταχωρήσεις για κόκκινο καπέλο Enterprise Linux εμφανίζονται στα αποτελέσματα αναζήτησης. Κάντε κλικ στο **κόκκινο καπέλο Enterprise Linux 7.2**.

    ![Επιλέξτε RHEL στα αποτελέσματα](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. Στα αποτελέσματα αναζήτησης στο παράθυρο **όλα τα στοιχεία** θα πρέπει να περιλαμβάνονται στην εικόνα με κόκκινο καπέλο Enterprise Linux 7.2. Κάντε κλικ στο **κόκκινο καπέλο Enterprise Linux 7.2** για να δείτε περισσότερες πληροφορίες σχετικά με την εικόνα εικονική μηχανή.

    ![Επιλέξτε RHEL στα αποτελέσματα](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. Στο παράθυρο **κόκκινο καπέλο Enterprise Linux 7.2** , θα πρέπει να βλέπετε περισσότερες πληροφορίες σχετικά με την εικόνα εικονική μηχανή. Στην αναπτυσσόμενη λίστα **Επιλέξτε ένα μοντέλο ανάπτυξης** , επιλέξτε **κλασική**. Στη συνέχεια, κάντε κλικ στο κουμπί **Δημιουργία** .

    ![Προβολή λεπτομερειών εικόνας](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. Στο παράθυρο **Δημιουργία Εικονική** , πληκτρολογήστε το **Όνομα κεντρικού υπολογιστή** για τη νέα εικονική μηχανή. Επίσης Καθορίστε ένα όνομα χρήστη τοπικού διαχειριστή στο πεδίο **όνομα χρήστη** και έναν **κωδικό πρόσβασης**. Μπορείτε επίσης να χρησιμοποιήσετε ένα πλήκτρο SSH για τον έλεγχο ταυτότητας χρήστη τοπικού διαχειριστή. Επίσης, επιλέξτε μια **Σειρά τις τιμές** για την εικονική μηχανή.

    ![Δημιουργία Εικονική - βασικές λεπτομέρειες](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. Κάντε κλικ στην επιλογή **προαιρετικών παραμέτρων**. Στο παράθυρο **προαιρετικό ρύθμισης παραμέτρων** , κάντε κλικ στην επιλογή **δικτύου**.

    ![Δημιουργία Εικονική - ρύθμιση παραμέτρων εικονικού δικτύου](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. Αυτόν τον τρόπο εμφανίζεται ένα παράθυρο με τίτλο **δικτύου**. Στο παράθυρο **δικτύου** , κάντε κλικ στην επιλογή **Εικονικού δικτύου** για να επιλέξετε το εικονικό δίκτυο στο οποίο πρέπει να αναπτυχθεί το Εικονική Linux. Έτσι ανοίγει το παράθυρο **Εικονικού δικτύου** . Στο παράθυρο **Εικονικού δικτύου** , κάντε την επιλογή **χρήση ενός υπάρχοντος εικονικού δικτύου** . Στη συνέχεια, επιλέξτε το εικονικό δίκτυο στο οποίο Azure υπηρεσίες τομέα AD είναι διαθέσιμη. Σε αυτό το παράδειγμα, επιλέγουμε το εικονικό δίκτυο 'MyPreviewVNet'.

    ![Δημιουργία Εικονική - επιλέξτε εικονικού δικτύου](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. Στο παράθυρο **προαιρετικό ρύθμισης παραμέτρων** , κάντε κλικ στο κουμπί **OK** .

    ![Δημιουργία Εικονική - εικονικού δικτύου επιλεγμένο](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. Τώρα είστε έτοιμοι να δημιουργήσετε την εικονική μηχανή. Στο παράθυρο **Δημιουργία Εικονική** , κάντε κλικ στο κουμπί **Δημιουργία** .

    ![Δημιουργία Εικονική - έτοιμοι](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. Ανάπτυξη του η νέα εικονική μηχανή με βάση την εικόνα RHEL 7.2 πρέπει να ξεκινά.

  ![Δημιουργία Εικονική - ανάπτυξης αποτελέσματα](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. Μετά από μερικά λεπτά, η εικονική μηχανή θα πρέπει να αναπτυχθούν με επιτυχία και έτοιμη για χρήση.

  ![Δημιουργία Εικονική - αναπτυχθεί](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Σύνδεση απομακρυσμένης με την προμήθεια του φακέλου που μόλις Linux εικονική μηχανή
Η εικονική μηχανή RHEL 7.2 έχει αποδοθεί στο Azure. Η επόμενη εργασία είναι να συνδεθούν απομακρυσμένα σε η εικονική μηχανή.

**Σύνδεση με την RHEL 7.2 εικονική μηχανή** Ακολουθήστε τις οδηγίες στο άρθρο [πώς μπορείτε να συνδεθείτε με μια εικονική μηχανή εκτελείται Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

Τα υπόλοιπα βήματα θεωρείται ότι μπορείτε να χρησιμοποιήσετε το PuTTY SSH πρόγραμμα-πελάτη για να συνδεθείτε με την εικονική μηχανή RHEL. Για περισσότερες πληροφορίες, ανατρέξτε στη [σελίδα λήψης PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Ανοίξτε το πρόγραμμα PuTTY.

2. Πληκτρολογήστε το **Όνομα κεντρικού υπολογιστή** για την που έχουν δημιουργηθεί πρόσφατα εικονική μηχανή RHEL. Σε αυτό το παράδειγμα, μας εικονική μηχανή περιλαμβάνει το όνομα κεντρικού υπολογιστή 'contoso-rhel.cloudapp .net'. Εάν δεν είστε βέβαιοι για το όνομα κεντρικού υπολογιστή του Εικονική σας, ανατρέξτε στον πίνακα εργαλείων Εικονική στην πύλη του Azure.

    ![Σύνδεση puTTY](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. Συνδεθείτε με την εικονική μηχανή χρησιμοποιώντας τα διαπιστευτήρια του τοπικού διαχειριστή που καθορίσατε κατά την οποία δημιουργήθηκε η εικονική μηχανή. Σε αυτό το παράδειγμα, χρησιμοποιήσαμε λογαριασμού του τοπικού διαχειριστή "mahesh".

    ![PuTTY σύνδεσης](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Εγκατάσταση απαραίτητα πακέτα στον υπολογιστή εικονικές Linux
Μετά τη σύνδεση με την εικονική μηχανή, την επόμενη εργασία είναι να εγκαταστήσετε πακέτα που απαιτούνται για συμμετοχή σε τομέα στον υπολογιστή εικονική. Ακολουθήστε τα παρακάτω βήματα:

1. **Εγκατάσταση realmd:** Το πακέτο realmd χρησιμοποιείται για συμμετοχή σε τομέα. Στο σας PuTTY terminal, πληκτρολογήστε την ακόλουθη εντολή:

    realmd εγκατάσταση yum sudo

    ![Εγκατάσταση realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Μετά από μερικά λεπτά, θα πρέπει να λάβετε εγκατασταθεί το πακέτο realmd στον υπολογιστή εικονική.

    ![realmd εγκατεστημένο](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **Εγκατάσταση sssd:** Το πακέτο realmd εξαρτάται από sssd για την εκτέλεση λειτουργιών συμμετοχή σε τομέα. Στο σας PuTTY terminal, πληκτρολογήστε την ακόλουθη εντολή:

    sssd εγκατάσταση yum sudo

    ![Εγκατάσταση του sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Μετά από μερικά λεπτά, θα πρέπει να λάβετε εγκατασταθεί το πακέτο sssd στον υπολογιστή εικονική.

    ![realmd εγκατεστημένο](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **Εγκατάσταση του kerberos:** Στο σας PuTTY terminal, πληκτρολογήστε την ακόλουθη εντολή:

    sudo krb5-σταθμούς εργασίας εγκατάσταση yum krb5-βιβλιοθηκών

    ![Εγκατάσταση του kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Μετά από μερικά λεπτά, θα πρέπει να λάβετε εγκατασταθεί το πακέτο realmd στον υπολογιστή εικονική.

    ![Kerberos εγκατεστημένο](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Συμμετοχή σε η εικονική μηχανή Linux διαχειριζόμενων τομέα
Τώρα που τα απαιτούμενα πακέτα εγκαθίστανται στον υπολογιστή εικονικές Linux, την επόμενη εργασία είναι για να συμμετάσχετε σε η εικονική μηχανή διαχειριζόμενων στον τομέα.

1. Ανακαλύψτε τα διαχειριζόμενα AAD τομέα υπηρεσιών τομέα. Στο σας PuTTY terminal, πληκτρολογήστε την ακόλουθη εντολή:

    Ανακαλύψτε το CONTOSO100.COM sudo τομέα

    ![Ανακαλύψτε το τομέα](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Εάν **ανακαλύψετε τομέα** είναι δεν είναι δυνατή η εύρεση διαχειριζόμενων τον τομέα σας, βεβαιωθείτε ότι ο τομέας είναι προσβάσιμος από την εικονική μηχανή (δοκιμάστε ping). Επίσης, βεβαιωθείτε ότι η εικονική μηχανή έχει αναπτυχθεί στην πραγματικότητα στο ίδιο δίκτυο εικονικού στην οποία διαχειριζόμενων τομέα είναι διαθέσιμο.

2. Προετοιμασία kerberos. Στο σας PuTTY terminal, πληκτρολογήστε την ακόλουθη εντολή. Βεβαιωθείτε ότι καθορίζετε ένα χρήστη που ανήκει στην ομάδα 'Διαχειριστές ελεγκτή Τομέα AAD'. Μόνο οι χρήστες μπορούν να συμμετάσχουν σε υπολογιστές διαχειριζόμενων στον τομέα.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Βεβαιωθείτε ότι μπορείτε να καθορίσετε το όνομα του τομέα με κεφαλαία γράμματα, άλλο kinit αποτυγχάνει.

3. Συμμετοχή σε υπολογιστή με τον τομέα. Στο σας PuTTY terminal, πληκτρολογήστε την ακόλουθη εντολή. Καθορίστε τον ίδιο χρήστη που καθορίσατε στο προηγούμενο βήμα ('kinit').

    συμμετοχή σε τομέα sudo--λεπτομερής CONTOSO100.COM -U'bob@CONTOSO100.COM'

    ![Συμμετοχή σε τομέα](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Θα πρέπει να λάβετε ένα μήνυμα ("με επιτυχία καταχωρήθηκε υπολογιστή στην περιοχή") όταν υπολογιστή είναι συνδεδεμένος με επιτυχία στον τομέα διαχειριζόμενων.


## <a name="verify-domain-join"></a>Επαλήθευση του τομέα συνδέσμου
Μπορείτε γρήγορα να επαληθεύσετε εάν ο υπολογιστής συνδέθηκε με επιτυχία στον τομέα διαχειριζόμενων. Σύνδεση με το νέο τομέα συνδεθεί Εικονική RHEL χρησιμοποιώντας SSH και ένα λογαριασμό χρήστη τομέα και στη συνέχεια, επιλέξτε για να δείτε αν ο λογαριασμός χρήστη έχει επιλυθεί σωστά.

1. Στο σας PuTTY terminal, πληκτρολογήστε την παρακάτω εντολή για να συνδεθείτε με το νέο τομέα συνδεθεί RHEL εικονική μηχανή χρησιμοποιώντας SSH. Χρησιμοποιήστε ένα λογαριασμό τομέα που ανήκει στον τομέα διαχειριζόμενων (για παράδειγμα, 'bob@CONTOSO100.COM' σε αυτήν την περίπτωση.)

    SSH -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net

2. Στο σας PuTTY terminal, πληκτρολογήστε την παρακάτω εντολή για να δείτε εάν η προετοιμασία του κεντρικού καταλόγου ολοκληρώθηκε σωστά.

    PWD

3. Στο σας PuTTY terminal, πληκτρολογήστε την παρακάτω εντολή για να δείτε εάν επιλύονται σωστά των μελών της ομάδας.

    αναγνωριστικό

Ακολουθεί ένα δείγμα εξόδου από αυτές τις εντολές:

![Επαλήθευση του τομέα συνδέσμου](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## <a name="troubleshooting-domain-join"></a>Αντιμετώπιση προβλημάτων τομέα συνδέσμου
Ανατρέξτε στο άρθρο [Αντιμετώπιση προβλημάτων τομέα συμμετοχή](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) .


## <a name="related-content"></a>Σχετικό περιεχόμενο
- [Υπηρεσίες Azure τομέα AD - Οδηγός γρήγορων αποτελεσμάτων](./active-directory-ds-getting-started.md)

- [Συμμετοχή σε μια εικονική μηχανή Windows Server σε έναν τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](active-directory-ds-admin-guide-join-windows-vm.md)

- [Πώς μπορείτε να συνδεθείτε με μια εικονική μηχανή εκτελείται Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

- [Κατά την εγκατάσταση του Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [Για μεγάλες επιχειρήσεις Linux κόκκινο καπέλο 7 - Οδηγός ενοποίηση των Windows](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
