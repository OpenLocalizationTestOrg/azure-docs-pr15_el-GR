<properties
    pageTitle="Azure υπηρεσίες τομέα Active Directory: Διαχείριση διαχειριζόμενων τομέα | Microsoft Azure"
    description="Διαχειριστείτε υπηρεσίες τομέα Active Directory Azure διαχειριζόμενων τομείς"
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

# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Διαχειριστείτε έναν τομέα διαχειριζόμενων υπηρεσίες τομέα Active Directory Azure
Αυτό το άρθρο σας δείχνει πώς να διαχειριστείτε έναν τομέα διαχειριζόμενων υπηρεσίες τομέα Azure Active Directory (AD).


## <a name="before-you-begin"></a>Πριν ξεκινήσετε
Για να εκτελέσετε τις εργασίες που αναφέρονται σε αυτό το άρθρο, χρειάζεστε:

1. Μια έγκυρη **Azure συνδρομής**.

2. Μια **καταλόγου Azure AD** - είτε συγχρονίζονται με μια εσωτερική ή κατάλογο μόνο στο cloud.

3. **Υπηρεσίες τομέα AD Azure** πρέπει να είναι ενεργοποιημένη για τον κατάλογο Azure AD. Εάν δεν το έχετε κάνει, ακολουθήστε όλες τις εργασίες που περιγράφονται σε του [Οδηγού για γρήγορα αποτελέσματα](./active-directory-ds-getting-started.md).

4. Μια **εικονική μηχανή τομέα** από την οποία μπορείτε να διαχειριστείτε τις υπηρεσίες τομέα AD Azure διαχείρισης τομέα. Εάν δεν έχετε όπως μια εικονική μηχανή, ακολουθήστε όλες τις εργασίες που περιγράφεται στο άρθρο με τίτλο [συμμετοχή σε μια εικονική μηχανή των Windows σε διαχειριζόμενες τομέα](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Χρειάζεστε τα διαπιστευτήρια ενός **λογαριασμού χρήστη που ανήκει στην ομάδα ' διαχειριστές ελεγκτή Τομέα AAD'** στον κατάλογό σας, για να διαχειριστείτε διαχειριζόμενων τον τομέα σας.

<br>


## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Εργασίες διαχείρισης που μπορείτε να εκτελέσετε σε διαχειριζόμενες τομέα
Τα μέλη της ομάδας 'Διαχειριστές ελεγκτή Τομέα AAD' έχουν εκχωρηθεί δικαιώματα διαχειριζόμενων τον τομέα που σας επιτρέπουν να εκτελούν εργασίες όπως:

- Συμμετοχή σε μηχανές διαχειριζόμενων στον τομέα.

- Ρύθμιση παραμέτρων το ενσωματωμένο αντικείμενο πολιτικής ΟΜΆΔΑΣ για το κοντέινερ 'AADDC υπολογιστές' και 'AADDC χρήστες' στον διαχειριζόμενων τομέα.

- Διαχείριση του DNS για τον τομέα διαχειριζόμενων.

- Δημιουργήστε και διαχειριστείτε προσαρμοσμένο εταιρικό μονάδες (OU) διαχειριζόμενων τον τομέα.

- Απόκτηση πρόσβασης διαχείρισης σε υπολογιστές συνδεδεμένα διαχειριζόμενων στον τομέα.


## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Δεν έχετε σε διαχειριζόμενες τομέα δικαιώματα διαχειριστή
Ο τομέας γίνεται από τη Microsoft, συμπεριλαμβανομένων των δραστηριοτήτων όπως ενημέρωση, την παρακολούθηση και, εκτέλεση δημιουργίας αντιγράφων ασφαλείας. Επομένως, ο τομέας είναι κλειδωμένο και δεν έχετε δικαιώματα για να εκτελέσετε ορισμένες εργασίες διαχείρισης για τον τομέα. Ορισμένα παραδείγματα εργασιών που δεν μπορείτε να εκτελέσετε είναι κάτω από το στοιχείο.

- Που δεν έχουν εκχωρηθεί δικαιώματα διαχειριστή τομέα ή το διαχειριστή της εταιρείας για τον τομέα διαχειριζόμενων.

- Δεν μπορείτε να επεκτείνετε το σχήμα του διαχειριζόμενων τομέα.

- Δεν μπορείτε να συνδεθείτε σε ελεγκτές τομέα για τον τομέα διαχειριζόμενων χρήση απομακρυσμένης επιφάνειας εργασίας.

- Δεν μπορείτε να προσθέσετε ελεγκτές τομέα στον τομέα διαχειριζόμενων.


## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Εργασία 1 - παροχή τομέα Windows Server εικονικό μηχάνημα για την απομακρυσμένη διαχείριση διαχειριζόμενων τομέα
Azure τομείς διαχειριζόμενων υπηρεσίες τομέα AD μπορείτε να διαχειριστείτε χρησιμοποιώντας οικεία εργαλεία διαχείρισης υπηρεσίας καταλόγου Active Directory όπως η διαχείρισης κέντρο Active Directory (ADAC) ή το AD PowerShell. Διαχειριστές μισθωτών δεν έχετε δικαιώματα για να συνδεθείτε με ελεγκτές τομέα για τον τομέα διαχειριζόμενων μέσω απομακρυσμένης επιφάνειας εργασίας. Επομένως, τα μέλη της ομάδας 'Διαχειριστές ελεγκτή Τομέα AAD' μπορεί να διαχειρίζεται διαχειριζόμενων τομείς από μακριά χρησιμοποιώντας εργαλεία διαχείρισης AD από έναν υπολογιστή Windows Server/πελάτη που συνδέεται με τη διαχειριζόμενη τομέα. Εργαλεία διαχείρισης AD μπορεί να εγκατασταθεί ως μέρος της δυνατότητας προαιρετικά εργαλεία διαχείρισης απομακρυσμένου διακομιστή (RSAT) σε Windows Server και υπολογιστές-πελάτες που συμμετέχουν στον τομέα διαχειριζόμενων.

Το πρώτο βήμα είναι να ρυθμίσετε μια εικονική μηχανή Windows Server που είναι συνδεδεμένος στον τομέα διαχειριζόμενων. Για οδηγίες, ανατρέξτε στο άρθρο με τίτλο [συμμετοχή σε μια εικονική μηχανή Windows Server σε έναν τομέα υπηρεσίες τομέα AD Azure διαχειριζόμενων](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Απομακρυσμένη διαχείριση διαχειριζόμενων τομέα από έναν υπολογιστή-πελάτη (για παράδειγμα, τα Windows 10)
Τις οδηγίες σε αυτό το άρθρο, χρησιμοποιήστε μια εικονική μηχανή Windows Server για να διαχειριστείτε την DS AAD διαχείρισης τομέα. Ωστόσο, μπορείτε επίσης να επιλέξετε να χρησιμοποιήσετε μια εικονική μηχανή προγράμματος-πελάτη (για παράδειγμα, τα Windows 10) των Windows για να το κάνετε.

Μπορείτε να [εγκαταστήσετε τα εργαλεία διαχείρισης απομακρυσμένου διακομιστή (RSAT)](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) σε υπολογιστή Windows προγράμματος-πελάτη εικονικές, ακολουθώντας τις οδηγίες στο TechNet.


## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Εργασία 2 - Εργαλεία διαχείρισης, εγκατάσταση υπηρεσίας καταλόγου Active Directory στον η εικονική μηχανή
Ακολουθήστε τα παρακάτω βήματα για να εγκαταστήσετε τα εργαλεία διαχείρισης Active Directory στον υπολογιστή εικονικές τομέα συμμετέχει. Για περισσότερες [πληροφορίες σχετικά με την εγκατάσταση και χρήση εργαλείων απομακρυσμένης διαχείρισης διακομιστή](https://technet.microsoft.com/library/hh831501.aspx), ανατρέξτε στο Technet.

1. Μεταβείτε σε **εικονικές μηχανές** κόμβου στην πύλη του Azure κλασική. Επιλέξτε την εικονική μηχανή που δημιουργήσατε στην εργασία 1 και κάντε κλικ στην επιλογή " **σύνδεση** " στη γραμμή εντολών στο κάτω μέρος του παραθύρου.

    ![Σύνδεση με Windows εικονική μηχανή](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Κλασική πύλη σάς ζητά να ανοίξετε ή να αποθηκεύσετε ένα αρχείο με επέκταση '.rdp', που χρησιμοποιείται για να συνδεθείτε με την εικονική μηχανή. Κάντε κλικ για να ανοίξετε το αρχείο όταν έχει ολοκληρωθεί τη λήψη.

3. Στη γραμμή εντολών login, χρησιμοποιήστε τα διαπιστευτήρια του χρήστη που ανήκει στην ομάδα 'Διαχειριστές ελεγκτή Τομέα AAD'. Για παράδειγμα, χρησιμοποιούμε 'bob@domainservicespreview.onmicrosoft.com' στην περίπτωσή μας.

4. Από την οθόνη έναρξης, ανοίξτε τη **Διαχείριση διακομιστή**. Κάντε κλικ στην επιλογή **Προσθήκη ρόλων και δυνατότητες** στο κεντρικό τμήμα του παραθύρου Server Manager.

    ![Εκκίνηση Server Manager στην εικονική μηχανή](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. Στη σελίδα **Πριν να ξεκινήσετε** από την **Προσθήκη τους ρόλους και Οδηγό δυνατοτήτων**, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Πριν ξεκινήσετε τη σελίδα](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Στη σελίδα **Τύπο εγκατάστασης** , αφήστε την επιλογή **εγκατάστασης βάσει ρόλων ή με βάση τη δυνατότητα** ελέγχου και κάντε κλικ στο κουμπί **Επόμενο**.

    ![Σελίδα τύπου εγκατάστασης](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. Στη σελίδα **Επιλογή διακομιστή** , επιλέξτε την τρέχουσα εικονική μηχανή από το χώρο συγκέντρωσης διακομιστή και κάντε κλικ στο κουμπί **Επόμενο**.

    ![Σελίδα επιλογής διακομιστή](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Στη σελίδα **Τους ρόλους διακομιστή** , κάντε κλικ στο κουμπί **Επόμενο**. Θα σας Παράλειψη αυτής της σελίδας, επειδή θα σας δεν την εγκατάσταση όλους τους ρόλους στο διακομιστή.

9. Στη σελίδα " **δυνατότητες** ", κάντε κλικ για να αναπτύξετε τον κόμβο **Εργαλεία απομακρυσμένης διαχείρισης διακομιστή** και, στη συνέχεια, κάντε κλικ στην επιλογή για να αναπτύξετε τον κόμβο **Εργαλεία διαχείρισης ρόλο** . Επιλέξτε δυνατότητα **AD DS και εργαλεία AD LDS** από τη λίστα των εργαλείων διαχείρισης ρόλο.

    ![Σελίδα "δυνατότητες"](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)

10. Στη σελίδα **επιβεβαίωσης** , κάντε κλικ στην επιλογή **εγκατάσταση** για εγκατάσταση του AD και δυνατότητα εργαλεία AD LDS στον υπολογιστή εικονική. Κατά την εγκατάσταση της δυνατότητας ολοκληρωθεί με επιτυχία, κάντε κλικ στο κουμπί **Κλείσιμο** για να κλείσετε τον οδηγό **Προσθήκη ρόλων και δυνατότητες** .

    ![Σελίδα επιβεβαίωσης](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)


## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Εργασία 3 - σύνδεση με και Εξερεύνηση διαχειριζόμενων τομέα
Τώρα που είναι εγκατεστημένα τα εργαλεία διαχείρισης AD στον τομέα συνδεθεί εικονική μηχανή, μπορούμε να χρησιμοποιήσουμε αυτά τα εργαλεία για να εξερευνήσετε και να διαχειριστείτε τα διαχειριζόμενα τομέα.

> [AZURE.NOTE] Πρέπει να είστε μέλος της ομάδας 'Διαχειριστές ελεγκτή Τομέα AAD', για να διαχειριστείτε τη διαχειριζόμενη τομέα.

1. Από την οθόνη έναρξης, κάντε κλικ στην επιλογή **Εργαλεία διαχείρισης**. Θα πρέπει να δείτε τα εργαλεία διαχείρισης AD εγκατεστημένο στον υπολογιστή εικονική.

    ![Εργαλεία διαχείρισης που είναι εγκατεστημένα στο διακομιστή](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Κάντε κλικ στο **Κέντρο διαχείρισης υπηρεσίας καταλόγου Active Directory**.

    ![Κέντρο διαχείρισης υπηρεσίας καταλόγου Active Directory](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Για να εξερευνήσετε τον τομέα, κάντε κλικ στο όνομα τομέα στο αριστερό τμήμα του παραθύρου (για παράδειγμα, ' contoso100.com'). Παρατηρήστε δύο κοντέινερ που ονομάζεται 'AADDC υπολογιστές' και 'AADDC χρήστες' αντίστοιχα.

    ![ADAC - προβολή τομέα](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)

4. Κάντε κλικ στο κοντέινερ που ονομάζεται **AADDC χρήστες** για να δείτε όλους τους χρήστες και ομάδες που ανήκουν σε διαχειριζόμενες τομέα. Θα πρέπει να δείτε τους λογαριασμούς χρηστών και ομάδων από το Azure AD μισθωτή εμφάνιση προς τα επάνω σε αυτό το κοντέινερ. Ειδοποίηση σε αυτό το παράδειγμα, ένα λογαριασμό χρήστη για το χρήστη που ονομάζεται "χαράλαμπος" και μια ομάδα που ονομάζεται 'Διαχειριστές ελεγκτή Τομέα AAD' είναι διαθέσιμες σε αυτό το κοντέινερ.

    ![ADAC - χρήστες τομέα](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)

5. Κάντε κλικ στο κοντέινερ που ονομάζεται **AADDC υπολογιστές** για να δείτε το υπολογιστές που είναι συνδεδεμένοι σε αυτόν τον τομέα διαχειριζόμενων. Θα πρέπει να βλέπετε μια καταχώρηση για το τρέχον εικονικό υπολογιστή, που είναι συνδεδεμένος στον τομέα. Σε αυτό το κοντέινερ 'AADDC υπολογιστές' αποθηκεύονται οι λογαριασμοί υπολογιστή για όλους τους υπολογιστές που είναι συνδεδεμένοι σε υπηρεσίες τομέα AD Azure διαχειριζόμενων τομέα.

    ![ADAC - τομέα συνδεθεί υπολογιστές](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Σχετικό περιεχόμενο

- [Υπηρεσίες Azure τομέα AD - Οδηγός γρήγορων αποτελεσμάτων](./active-directory-ds-getting-started.md)

- [Συμμετοχή σε μια εικονική μηχανή Windows Server σε έναν τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](active-directory-ds-admin-guide-join-windows-vm.md)

- [Ανάπτυξη των εργαλείων απομακρυσμένης διαχείρισης διακομιστή](https://technet.microsoft.com/library/hh831501.aspx)