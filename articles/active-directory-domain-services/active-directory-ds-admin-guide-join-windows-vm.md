<properties
    pageTitle="Azure υπηρεσίες τομέα Active Directory: Συμμετοχή σε μια Εικονική διακομιστή των Windows σε διαχειριζόμενες τομέα | Microsoft Azure"
    description="Συμμετοχή σε μια εικονική μηχανή Windows Server στις υπηρεσίες τομέα AD Azure"
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

# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Συμμετοχή σε μια εικονική μηχανή Windows Server σε διαχειριζόμενες τομέα

> [AZURE.SELECTOR]
- [Azure κλασική πύλης - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

Αυτό το άρθρο σας δείχνει πώς μπορείτε να συμμετάσχετε σε μια εικονική μηχανή με Windows Server 2012 R2 σε έναν τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure, με την πύλη Azure κλασική.


## <a name="step-1-create-the-windows-server-virtual-machine"></a>Βήμα 1: Δημιουργία του Windows Server εικονική μηχανή
Ακολουθήστε τις οδηγίες που περιγράφονται στην εκμάθηση [Δημιουργία μια εικονική μηχανή με Windows στην πύλη του Azure κλασική](../virtual-machines/virtual-machines-windows-classic-tutorial.md) . Είναι σημαντικό να διασφαλίσετε ότι αυτό που μόλις δημιουργήσατε εικονική μηχανή είναι συνδεδεμένος με το ίδιο εικονικό δίκτυο στο οποίο έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure. Η επιλογή 'Γρήγορη δημιουργία' δεν δίνει τη δυνατότητα να συμμετάσχετε η εικονική μηχανή σε εικονικό δίκτυο. Επομένως, πρέπει να χρησιμοποιήσετε την επιλογή 'Από τη συλλογή' για να δημιουργήσετε την εικονική μηχανή.

Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε μια εικονική μηχανή Windows συνδεθεί με το εικονικό δίκτυο στο οποίο έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure.

1. Στην κλασική πύλη Azure, στη γραμμή εντολών στο κάτω μέρος του παραθύρου, κάντε κλικ στην επιλογή **Δημιουργία**.

2. Στην περιοχή **τον υπολογισμό**, κάντε κλικ στην επιλογή **εικονική μηχανή**και, στη συνέχεια, κάντε κλικ στην επιλογή **Από τη συλλογή**.

3. Πρώτη οθόνη σάς επιτρέπει να **Επιλέξτε μια εικόνα** για την εικονική μηχανή σας από τη λίστα των διαθέσιμων εικόνων. Επιλέξτε την κατάλληλη εικόνα.

    ![Επιλέξτε Εικόνα](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. Η δεύτερη οθόνη σάς επιτρέπει να επιλέξτε το όνομα του υπολογιστή, το μέγεθος, και το όνομα χρήστη με δικαιώματα διαχειριστή και τον κωδικό πρόσβασης. Χρησιμοποιήστε τη σειρά και το μέγεθος που απαιτούνται για την εκτέλεση της εφαρμογής ή το φόρτο εργασίας σας. Το όνομα χρήστη που επιλέγετε εδώ είναι ένας χρήστης τοπικού διαχειριστή στον υπολογιστή. Δεν καταχωρείτε εδώ διαπιστευτηρίων του λογαριασμού χρήστη τομέα.

    ![Ρύθμιση παραμέτρων εικονική μηχανή](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)

5. Στην τρίτη οθόνη σάς επιτρέπει να ρυθμίσετε τις παραμέτρους πόρους για δικτύωση, αποθήκευση και διαθεσιμότητα. Βεβαιωθείτε ότι επιλέγετε το εικονικό δίκτυο στο οποίο έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure από την αναπτυσσόμενη λίστα της **Περιοχής/συνάφειας/εικονική ομάδα δικτύου** . Καθορίστε ένα **Όνομα DNS υπηρεσία Cloud** που είναι κατάλληλη για την εικονική μηχανή.

    ![Επιλέξτε εικονικού δικτύου για εικονική μηχανή](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [AZURE.WARNING]
    Βεβαιωθείτε ότι συμμετέχετε η εικονική μηχανή στο ίδιο εικονικό δίκτυο στο οποίο έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure. Ως αποτέλεσμα, η εικονική μηχανή να 'δείτε' του τομέα και να εκτελούν εργασίες όπως η συμμετοχή του τομέα. Εάν επιλέξετε για να δημιουργήσετε την εικονική μηχανή σε μια διαφορετική εικονικού δικτύου, σύνδεση που εικονικού δικτύου με το εικονικό δίκτυο στο οποίο έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure.

6. Η τέταρτη οθόνη σάς επιτρέπει να εγκαταστήσετε τον παράγοντα Εικονική και να ρυθμίσετε ορισμένες από τις διαθέσιμες επεκτάσεις.

    ![Εργασία](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)

7. Αφού δημιουργηθεί η εικονική μηχανή, την πύλη του κλασική παραθέτει τα νέα εικονική μηχανή κάτω από τον κόμβο **εικονικές μηχανές** . Τόσο η εικονική μηχανή και η υπηρεσία cloud ξεκινούν αυτόματα και εμφανίζεται ως **εκτελεί**την κατάστασή τους.

    ![Εικονική μηχανή βρίσκεται σε λειτουργία](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Βήμα 2: Σύνδεση με χρήση του λογαριασμού τοπικού διαχειριστή του Windows Server εικονική μηχανή
Τώρα, θα σας σύνδεση με το πρόσφατα δημιουργημένο Windows Server εικονικό υπολογιστή, για να το συνδέσετε με τον τομέα. Χρησιμοποιήστε τα διαπιστευτήρια τοπικού διαχειριστή που καθορίσατε κατά τη δημιουργία το εικονικό υπολογιστή, για να συνδεθείτε σε αυτήν.

Ακολουθήστε τα παρακάτω βήματα για να συνδεθείτε με την εικονική μηχανή.

1. Μεταβείτε σε **εικονικές μηχανές** κόμβου στην κλασική πύλη. Επιλέξτε την εικονική μηχανή που δημιουργήσατε στο βήμα 1 και κάντε κλικ στην επιλογή " **σύνδεση** " στη γραμμή εντολών στο κάτω μέρος του παραθύρου.

    ![Σύνδεση με Windows εικονική μηχανή](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Κλασική πύλη σάς ζητά να ανοίξετε ή να αποθηκεύσετε ένα αρχείο με επέκταση '.rdp', που χρησιμοποιείται για να συνδεθείτε με την εικονική μηχανή. Κάντε κλικ για να ανοίξετε το αρχείο όταν έχει ολοκληρωθεί τη λήψη.

3. Στη γραμμή εντολών σύνδεσης, εισαγάγετε τις **πιστοποιήσεις τοπικού διαχειριστή**, που έχει καθοριστεί κατά τη δημιουργία η εικονική μηχανή. Για παράδειγμα, χρησιμοποιήσαμε 'localhost\mahesh' σε αυτό το παράδειγμα.

Σε αυτό το σημείο, θα πρέπει να είστε συνδεδεμένοι την που έχουν δημιουργηθεί πρόσφατα Windows εικονική μηχανή χρησιμοποιώντας τα διαπιστευτήρια τοπικού διαχειριστή. Το επόμενο βήμα είναι να συνδέσετε την εικονική μηχανή με τον τομέα.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Βήμα 3: Συμμετοχή του Windows Server εικονική μηχανή στον τομέα διαχειριζόμενων AAD DS
Ακολουθήστε τα παρακάτω βήματα για να συμμετάσχετε σε η εικονική μηχανή Windows Server στον τομέα διαχειριζόμενων AAD DS.

1. Σύνδεση με το διακομιστή των Windows, όπως φαίνεται στο βήμα 2. Από την οθόνη έναρξης, ανοίξτε τη **Διαχείριση διακομιστή**.

2. Κάντε κλικ στην επιλογή **Τοπικού διακομιστή** στο αριστερό τμήμα του παραθύρου της διαχείρισης διακομιστή.

    ![Εκκίνηση Server Manager στην εικονική μηχανή](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. Στην ενότητα **ΙΔΙΌΤΗΤΕΣ** , κάντε κλικ στην επιλογή " **ομάδα ΕΡΓΑΣΊΑΣ** ". Στη σελίδα ιδιοτήτων **Ιδιότητες συστήματος** , κάντε κλικ στην επιλογή **Αλλαγή** για να συνδεθείτε με τον τομέα.

    ![Σελίδα ιδιοτήτων συστήματος](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

4. Καθορίστε το όνομα τομέα με τις υπηρεσίες τομέα AD Azure γίνεται τομέα στο πλαίσιο κειμένου του **τομέα σας** και κάντε κλικ στο κουμπί **OK**.

    ![Καθορίστε τον τομέα που θα ενοποιηθούν](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

5. Θα σας ζητηθεί να εισαγάγετε τα διαπιστευτήριά σας για να συνδεθείτε με τον τομέα. Βεβαιωθείτε ότι αυτό που **καθορίζετε τα διαπιστευτήρια για ένα χρήστη που ανήκει στην ομάδα διαχειριστών ελεγκτή Τομέα AAD** ομάδα. Μόνο τα μέλη αυτής της ομάδας έχουν δικαιώματα για να συμμετάσχετε σε μηχανές διαχειριζόμενων στον τομέα.

    ![Καθορίστε τα διαπιστευτήρια για συμμετοχή σε τομέα](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

6. Μπορείτε να καθορίσετε τα διαπιστευτήρια με έναν από τους εξής τρόπους:

    - Μορφή UPN: Καθορίστε το επίθημα UPN για το λογαριασμό χρήστη, όπως έχει ρυθμιστεί στο Azure AD. Σε αυτό το παράδειγμα, είναι το επίθημα UPN του χρήστη 'ο Μπάμπης' 'bob@domainservicespreview.onmicrosoft.com'.

    - Μορφή SAMAccountName: μπορείτε να καθορίσετε το όνομα του λογαριασμού με τη μορφή SAMAccountName. Σε αυτό το παράδειγμα, ο χρήστης 'ο Μπάμπης' θα πρέπει να πληκτρολογήσετε 'CONTOSO100\bob'.

        > [AZURE.NOTE] **Συνιστάται να χρησιμοποιείτε τη μορφή UPN για να καθορίσετε τα διαπιστευτήρια.** Τα χαρακτηριστικά SAMAccountName μπορεί να είναι αυτόματης δημιουργίας εάν πρόθεμα UPN ενός χρήστη είναι υπερβολικά μεγάλη (για παράδειγμα, 'joereallylongnameuser'). Εάν έχουν το ίδιο πρόθεμα UPN (για παράδειγμα, "ο Μπάμπης') στο μισθωτή του Azure AD πολλοί χρήστες, τους μορφή SAMAccountName ενδέχεται να δημιουργείται αυτόματα από την υπηρεσία. Σε αυτές τις περιπτώσεις, η μορφή UPN μπορεί να χρησιμοποιηθεί με αξιοπιστία για να συνδεθείτε με τον τομέα.

7. Μετά τη συμμετοχή σε τομέα είναι επιτυχής, μπορείτε να δείτε το ακόλουθο μήνυμα που σας καλωσορίζει στον τομέα. Επανεκκινήστε την εικονική μηχανή για να ολοκληρωθεί η λειτουργία συμμετοχή σε τομέα.

    ![Καλώς ορίσατε στον τομέα](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Αντιμετώπιση προβλημάτων τομέα συνδέσμου
### <a name="connectivity-issues"></a>Αντιμετωπίζετε προβλήματα συνδεσιμότητας
Εάν η εικονική μηχανή δεν είναι δυνατό να βρείτε τον τομέα, ανατρέξτε στα παρακάτω βήματα αντιμετώπισης προβλημάτων:

- Βεβαιωθείτε ότι η εικονική μηχανή συνδέεται στο ίδιο δίκτυο εικονικού ως που έχετε ενεργοποιήσει τις υπηρεσίες τομέα στο. Εάν όχι, η εικονική μηχανή δεν είναι δυνατή η σύνδεση με τον τομέα και, επομένως, δεν είναι δυνατό να συνδεθούν με τον τομέα.

- Εάν η εικονική μηχανή είναι συνδεδεμένη με μια άλλη εικονικού δικτύου, βεβαιωθείτε ότι αυτό εικονικού δικτύου είναι συνδεδεμένοι με το εικονικό δίκτυο στο οποίο έχετε ενεργοποιήσει τις υπηρεσίες τομέα.

- Προσπαθήστε να κάνετε ping στον τομέα που χρησιμοποιούν το όνομα τομέα του διαχειριζόμενων τομέα '(για παράδειγμα, ping contoso100.com). Εάν δεν μπορείτε να το κάνετε, προσπαθήστε να κάνετε ping στις διευθύνσεις IP για τον τομέα που εμφανίζεται στη σελίδα όπου έχετε ενεργοποιήσει τις υπηρεσίες τομέα AD Azure (για παράδειγμα, ' ping 10.0.0.4'). Εάν μπορείτε να κάνετε ping στη διεύθυνση IP, αλλά δεν τον τομέα, DNS ενδέχεται να έχει ρυθμιστεί σωστά. Ενδέχεται να μην έχετε ρυθμίσει τις διευθύνσεις IP του τομέα ως διακομιστές DNS για το εικονικό δίκτυο.

- Δοκιμάστε εκκαθάριση του cache επίλυσης DNS στον υπολογιστή εικονικές (' ipconfig/flushdns').

Εάν εμφανιστεί το παράθυρο διαλόγου που σας ρωτά για τα διαπιστευτήρια για να συνδεθείτε με τον τομέα, δεν έχετε προβλήματα συνδεσιμότητας.


### <a name="credentials-related-issues"></a>Ζητήματα που σχετίζονται με τα διαπιστευτήρια
Εάν αντιμετωπίζετε προβλήματα με τα διαπιστευτήρια και δεν μπορείτε να συνδεθείτε με τον τομέα, ανατρέξτε στα παρακάτω βήματα.

- Δοκιμάστε να χρησιμοποιήσετε τη μορφή UPN για να καθορίσετε τα διαπιστευτήρια. Τα χαρακτηριστικά SAMAccountName για το λογαριασμό σας μπορεί να είναι αυτόματης δημιουργίας εάν υπάρχουν πολλοί χρήστες με το ίδιο πρόθεμα UPN στο μισθωτή σας ή εάν το πρόθεμα UPN είναι υπερβολικά μεγάλη. Γι ' αυτό, η μορφή SAMAccountName για το λογαριασμό σας μπορεί να διαφέρουν από τι που αναμένετε ή τη χρήση του τομέα σας στην εσωτερική εγκατάσταση.

- Δοκιμάστε να χρησιμοποιήσετε τα διαπιστευτήρια από ένα λογαριασμό χρήστη που ανήκει στην ομάδα 'Διαχειριστές ελεγκτή Τομέα AAD' για να συμμετάσχετε σε μηχανές διαχειριζόμενων στον τομέα.

- Βεβαιωθείτε ότι έχετε [ενεργοποιήσει συγχρονισμό κωδικού πρόσβασης](active-directory-ds-getting-started-password-sync.md) σύμφωνα με τα βήματα που περιγράφονται στον οδηγό για γρήγορα αποτελέσματα.

- Βεβαιωθείτε ότι μπορείτε να χρησιμοποιήσετε το UPN του χρήστη όπως έχει ρυθμιστεί στο Azure AD (για παράδειγμα, 'bob@domainservicespreview.onmicrosoft.com') για να πραγματοποιήσετε είσοδο.

- Βεβαιωθείτε ότι έχετε σε αναμονή για αρκετά μεγάλο για συγχρονισμό κωδικού πρόσβασης για να ολοκληρώσετε όπως καθορίζεται στον οδηγό για γρήγορα αποτελέσματα.


## <a name="related-content"></a>Σχετικό περιεχόμενο

- [Υπηρεσίες Azure τομέα AD - Οδηγός γρήγορων αποτελεσμάτων](./active-directory-ds-getting-started.md)

- [Διαχείριση ενός τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](./active-directory-ds-admin-guide-administer-domain.md)
