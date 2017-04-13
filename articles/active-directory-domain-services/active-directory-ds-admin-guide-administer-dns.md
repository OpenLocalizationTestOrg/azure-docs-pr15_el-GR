<properties
    pageTitle="Azure υπηρεσίες τομέα Active Directory: Διαχείριση του DNS σε διαχειριζόμενες τομείς | Microsoft Azure"
    description="Διαχείριση του DNS σε υπηρεσίες τομέα Active Directory Azure διαχειριζόμενων τομείς"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Διαχείριση του DNS σε μια διαχειριζόμενη τομέα υπηρεσίες τομέα AD Azure
Azure υπηρεσίες τομέα Active Directory περιλαμβάνει ένα διακομιστή DNS (ανάλυση ονόματος τομέα) που παρέχει επίλυσης DNS για τον τομέα διαχειριζόμενων. Περιστασιακά, ίσως χρειαστεί να ρυθμίσετε τις παραμέτρους DNS για τον τομέα διαχειριζόμενων. Ίσως χρειαστεί να δημιουργήσετε τις εγγραφές DNS για υπολογιστές που δεν ανήκουν στον τομέα, ρύθμιση παραμέτρων διευθύνσεις IP για φόρτωση balancers ή ρύθμιση εξωτερικών προωθήσεις DNS. Για αυτόν το λόγο, οι χρήστες που ανήκουν στην ομάδα 'Διαχειριστές ελεγκτή Τομέα AAD' έχουν εκχωρηθεί δικαιώματα διαχείρισης DNS για τον τομέα διαχειριζόμενων.


## <a name="before-you-begin"></a>Πριν ξεκινήσετε
Για να εκτελέσετε τις εργασίες που αναφέρονται σε αυτό το άρθρο, χρειάζεστε:

1. Μια έγκυρη **Azure συνδρομής**.

2. Μια **καταλόγου Azure AD** - είτε συγχρονίζονται με μια εσωτερική ή κατάλογο μόνο στο cloud.

3. **Υπηρεσίες τομέα AD Azure** πρέπει να είναι ενεργοποιημένη για τον κατάλογο Azure AD. Εάν δεν το έχετε κάνει, ακολουθήστε όλες τις εργασίες που περιγράφονται σε του [Οδηγού για γρήγορα αποτελέσματα](./active-directory-ds-getting-started.md).

4. Μια **εικονική μηχανή τομέα** από την οποία μπορείτε να διαχειριστείτε τις υπηρεσίες τομέα AD Azure διαχείρισης τομέα. Εάν δεν έχετε όπως μια εικονική μηχανή, ακολουθήστε όλες τις εργασίες που περιγράφεται στο άρθρο με τίτλο [συμμετοχή σε μια εικονική μηχανή των Windows σε διαχειριζόμενες τομέα](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Χρειάζεστε τα διαπιστευτήρια ενός **λογαριασμού χρήστη που ανήκει στην ομάδα ' διαχειριστές ελεγκτή Τομέα AAD'** στον κατάλογό σας, για να διαχειριστείτε το DNS για τον τομέα σας διαχειριζόμενων.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Εργασία 1 - παροχή μια εικονική μηχανή τομέα για την απομακρυσμένη διαχείριση DNS για τον τομέα διαχειριζόμενων
Azure τομείς διαχειριζόμενων υπηρεσίες τομέα AD μπορείτε να διαχειριστείτε απομακρυσμένα χρησιμοποιώντας οικεία εργαλεία διαχείρισης υπηρεσίας καταλόγου Active Directory όπως η διαχείρισης κέντρο Active Directory (ADAC) ή το AD PowerShell. Ομοίως, DNS για τον τομέα διαχειριζόμενων μπορεί να διαχειριστεί από μακριά χρησιμοποιώντας τα εργαλεία διαχείρισης διακομιστή DNS.

Οι διαχειριστές στον κατάλογό σας Azure AD δεν έχετε δικαιώματα για να συνδεθείτε με ελεγκτές τομέα για τον τομέα διαχειριζόμενων μέσω απομακρυσμένης επιφάνειας εργασίας. Τα μέλη της ομάδας 'Διαχειριστές ελεγκτή Τομέα AAD' μπορεί να διαχειρίζεται το DNS για τους τομείς διαχειριζόμενων απομακρυσμένα χρησιμοποιώντας εργαλεία διακομιστή DNS από έναν υπολογιστή Windows Server/πελάτη που συνδέεται με τη διαχειριζόμενη τομέα. Εργαλεία διακομιστή DNS μπορεί να εγκατασταθεί ως μέρος της δυνατότητας προαιρετικά εργαλεία διαχείρισης απομακρυσμένου διακομιστή (RSAT) σε Windows Server και υπολογιστές-πελάτες που συμμετέχουν στον τομέα διαχειριζόμενων.

Η πρώτη εργασία είναι η παροχή μια εικονική μηχανή Windows Server που συνδέεται με τη διαχειριζόμενη τομέα. Για οδηγίες, ανατρέξτε στο άρθρο με τίτλο [συμμετοχή σε μια εικονική μηχανή Windows Server σε έναν τομέα υπηρεσίες τομέα AD Azure διαχειριζόμενων](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Εργασία 2 - εργαλεία εγκατάσταση διακομιστή DNS στον υπολογιστή εικονικές
Ακολουθήστε τα παρακάτω βήματα για να εγκαταστήσετε τα εργαλεία διαχείρισης DNS στον τομέα συμμετέχει εικονικές υπολογιστή. Για περισσότερες πληροφορίες σχετικά με [την εγκατάσταση και χρήση εργαλείων απομακρυσμένης διαχείρισης διακομιστή](https://technet.microsoft.com/library/hh831501.aspx), ανατρέξτε στο Technet.

1. Μεταβείτε σε **εικονικές μηχανές** κόμβου στην πύλη του Azure κλασική. Επιλέξτε την εικονική μηχανή που δημιουργήσατε στην εργασία 1 και κάντε κλικ στην επιλογή " **σύνδεση** " στη γραμμή εντολών στο κάτω μέρος του παραθύρου.

    ![Σύνδεση με Windows εικονική μηχανή](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Κλασική πύλη σάς ζητά να ανοίξετε ή να αποθηκεύσετε ένα αρχείο με επέκταση '.rdp', που χρησιμοποιείται για να συνδεθείτε με την εικονική μηχανή. Όταν ολοκληρωθεί τη λήψη, κάντε κλικ στο αρχείο.

3. Στη γραμμή εντολών login, χρησιμοποιήστε τα διαπιστευτήρια του χρήστη που ανήκει στην ομάδα 'Διαχειριστές ελεγκτή Τομέα AAD'. Για παράδειγμα, χρησιμοποιούμε 'bob@domainservicespreview.onmicrosoft.com' στην περίπτωσή μας.

4. Από την οθόνη έναρξης, ανοίξτε τη **Διαχείριση διακομιστή**. Κάντε κλικ στην επιλογή **Προσθήκη ρόλων και δυνατότητες** στο κεντρικό τμήμα του παραθύρου Server Manager.

    ![Εκκίνηση Διαχείριση διακομιστών στην εικονική μηχανή](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. Στη σελίδα **Πριν να ξεκινήσετε** από την **Προσθήκη τους ρόλους και Οδηγό δυνατοτήτων**, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Πριν ξεκινήσετε τη σελίδα](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Στη σελίδα **Τύπο εγκατάστασης** , αφήστε την επιλογή **εγκατάστασης βάσει ρόλων ή με βάση τη δυνατότητα** ελέγχου και κάντε κλικ στο κουμπί **Επόμενο**.

    ![Σελίδα τύπου εγκατάστασης](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. Στη σελίδα **Επιλογή διακομιστή** , επιλέξτε την τρέχουσα εικονική μηχανή από το χώρο συγκέντρωσης διακομιστή και κάντε κλικ στο κουμπί **Επόμενο**.

    ![Σελίδα επιλογής διακομιστή](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Στη σελίδα **Τους ρόλους διακομιστή** , κάντε κλικ στο κουμπί **Επόμενο**. Θα σας Παράλειψη αυτής της σελίδας, επειδή θα σας δεν την εγκατάσταση όλους τους ρόλους στο διακομιστή.

9. Στη σελίδα " **δυνατότητες** ", κάντε κλικ για να αναπτύξετε τον κόμβο **Εργαλεία απομακρυσμένης διαχείρισης διακομιστή** και, στη συνέχεια, κάντε κλικ στην επιλογή για να αναπτύξετε τον κόμβο **Εργαλεία διαχείρισης ρόλο** . Επιλέξτε τη δυνατότητα **Εργαλεία διακομιστή DNS** από τη λίστα με τα εργαλεία διαχείρισης ρόλο.

    ![Σελίδα "δυνατότητες"](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. Στη σελίδα **επιβεβαίωσης** , κάντε κλικ στην επιλογή **εγκατάσταση** για να εγκαταστήσετε τη δυνατότητα εργαλεία διακομιστή DNS στον υπολογιστή εικονική. Κατά την εγκατάσταση της δυνατότητας ολοκληρωθεί με επιτυχία, κάντε κλικ στο κουμπί **Κλείσιμο** για να κλείσετε τον οδηγό **Προσθήκη ρόλων και δυνατότητες** .

    ![Σελίδα επιβεβαίωσης](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Εργασία 3 - εκκίνηση στην κονσόλα διαχείρισης DNS για τη Διαχείριση DNS
Τώρα που η δυνατότητα εργαλεία διακομιστή DNS είναι εγκατεστημένη στον τομέα συνδεθεί εικονική μηχανή, χρησιμοποιούμε τα εργαλεία DNS για τη Διαχείριση DNS για τον τομέα διαχειριζόμενων.

> [AZURE.NOTE] Πρέπει να είστε μέλος της ομάδας 'Διαχειριστές ελεγκτή Τομέα AAD', για να διαχειριστείτε το DNS για τον τομέα διαχειριζόμενων.

1. Από την οθόνη έναρξης, κάντε κλικ στην επιλογή **Εργαλεία διαχείρισης**. Θα πρέπει να βλέπετε στην κονσόλα **DNS** εγκατεστημένο στον υπολογιστή εικονική.

    ![Εργαλεία διαχείρισης - κονσόλα DNS](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Κάντε κλικ στην επιλογή **DNS** για να ξεκινήσετε την Κονσόλα διαχείρισης DNS.

3. Στο παράθυρο διαλόγου **σύνδεση με το διακομιστή DNS** , κάντε κλικ στην επιλογή με τίτλο **του παρακάτω υπολογιστή**και πληκτρολογήστε το όνομα τομέα DNS του τομέα διαχειριζόμενων (για παράδειγμα, ' contoso100.com').

    ![Κονσόλα DNS - σύνδεση με τον τομέα](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. Κονσόλα DNS συνδέεται με τη διαχειριζόμενη τομέα.

    ![Κονσόλα DNS - διαχείριση του τομέα σας](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. Τώρα, μπορείτε να χρησιμοποιήσετε την κονσόλα DNS για να προσθέσετε εγγραφές DNS για υπολογιστές στο εσωτερικό του εικονικού δικτύου στο οποίο έχετε ενεργοποιήσει τις υπηρεσίες τομέα της AAD.

> [AZURE.WARNING] Να είστε προσεκτικοί κατά τη Διαχείριση DNS για τον τομέα διαχειριζόμενων χρησιμοποιώντας τα εργαλεία διαχείρισης DNS. Βεβαιωθείτε ότι αυτό που **δεν διαγράψετε ή να τροποποιήσετε τις ενσωματωμένες εγγραφές DNS που χρησιμοποιούνται από τις υπηρεσίες τομέα του τομέα**. Ενσωματωμένη εγγραφές DNS περιλαμβάνονται εγγραφές DNS του τομέα, εγγραφών του διακομιστή ονομάτων και άλλες εγγραφές που χρησιμοποιείται για το Κέντρο Δεδομένων του θέση. Εάν τροποποιείτε αυτών των εγγραφών, υπηρεσίες τομέα έχουν διακοπεί του εικονικού δικτύου.


Ανατρέξτε στο [άρθρο εργαλεία DNS στο Technet](https://technet.microsoft.com/library/cc753579.aspx) για περισσότερες πληροφορίες σχετικά με τη Διαχείριση DNS.


## <a name="related-content"></a>Σχετικό περιεχόμενο

- [Υπηρεσίες Azure τομέα AD - Οδηγός γρήγορων αποτελεσμάτων](./active-directory-ds-getting-started.md)

- [Συμμετοχή σε μια εικονική μηχανή Windows Server σε έναν τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](active-directory-ds-admin-guide-join-windows-vm.md)

- [Διαχείριση ενός τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](active-directory-ds-admin-guide-administer-domain.md)

- [Εργαλεία διαχείρισης DNS](https://technet.microsoft.com/library/cc753579.aspx)