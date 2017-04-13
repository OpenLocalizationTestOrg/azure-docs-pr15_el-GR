<properties
    pageTitle="Χρησιμοποιήστε ένα μισθωτή του Office 365 με μια συνδρομή στο Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να προσθέσετε έναν κατάλογο του Office 365 (μισθωτής) σε μια συνδρομή του Azure για να κάνετε τη συσχέτιση."
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="cjiang"/>

# <a name="associate-an-office-365-tenant-with-an-azure-subscription"></a>Συσχετισμός ένα μισθωτή του Office 365 με μια συνδρομή του Azure
Εάν αποκτήσατε Azure και το Office 365 συνδρομές ξεχωριστά στο παρελθόν και τώρα θέλετε να μπορεί να αποκτήσει πρόσβαση μισθωτή του Office 365 από το Azure συνδρομή, είναι εύκολο να το κάνετε. Σε αυτό το άρθρο σάς δείχνει πώς.

> [AZURE.NOTE] Σε αυτό το άρθρο δεν ισχύει για τους πελάτες σύμβαση Enterprise (EA).

## <a name="quick-guidance"></a>Σύντομη καθοδήγηση
Για να συσχετίσετε το μισθωτή του Office 365 με τη συνδρομή σας Azure, χρησιμοποιήστε το Azure λογαριασμό για να προσθέσετε το μισθωτή του Office 365 και, στη συνέχεια, να συσχετίσετε Azure τη συνδρομή σας με το μισθωτή του Office 365.

## <a name="detailed-steps"></a>Λεπτομερείς οδηγίες
Σε αυτό το σενάριο, ο τοίχος Kelley είναι ένας χρήστης που έχει μια συνδρομή με το λογαριασμό του Azure kelley.wall@outlook.com. Kelley περιλαμβάνει επίσης μια συνδρομή στο Office 365 στην περιοχή ο λογαριασμός kelley.wall@contoso.onmicrosoft.com. Τώρα Kelley θέλει να πρόσβαση στο μισθωτή του Office 365 με τη συνδρομή Azure.

![Στιγμιότυπο οθόνης του Azure Active Directory κατάστασης](./media/billing-add-office-365-tenant-to-azure-subscription/s31_msa-aad-status.png)

![Στιγμιότυπο οθόνης του Office 365 admin center ενεργοί χρήστες](./media/billing-add-office-365-tenant-to-azure-subscription/s32_office-365-user.png)

### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Για τη συσχέτιση για να λειτουργήσει σωστά, απαιτούνται οι ακόλουθες προϋποθέσεις:

- Χρειάζεστε τα διαπιστευτήρια του διαχειριστή της υπηρεσίας της Azure συνδρομής. Συνδιαχειριστών δεν μπορεί να εκτελέσει ένα υποσύνολο των βημάτων.
- Χρειάζεστε τα διαπιστευτήρια του ο Καθολικός διαχειριστής του μισθωτή του Office 365.
- Τη διεύθυνση ηλεκτρονικού ταχυδρομείου από το διαχειριστή της υπηρεσίας δεν πρέπει να περιλαμβάνονται στο μισθωτή του Office 365.
- Τη διεύθυνση ηλεκτρονικού ταχυδρομείου από το διαχειριστή της υπηρεσίας δεν πρέπει να αντιστοιχεί από οποιαδήποτε καθολικού διαχειριστή του μισθωτή του Office 365.
- Εάν χρησιμοποιείτε τη συγκεκριμένη στιγμή μια διεύθυνση ηλεκτρονικού ταχυδρομείου που είναι ένα λογαριασμό Microsoft και έναν εταιρικό λογαριασμό, να αλλάξετε προσωρινά το διαχειριστή της υπηρεσίας της συνδρομής σας στο Azure για να χρησιμοποιήσετε έναν άλλο λογαριασμό Microsoft. Μπορείτε να δημιουργήσετε ένα νέο λογαριασμό Microsoft στη [σελίδα εγγραφής λογαριασμού Microsoft](https://signup.live.com/).


Για να αλλάξετε το διαχειριστή της υπηρεσίας, ακολουθήστε τα παρακάτω βήματα:

1. Είσοδος στην [πύλη Διαχείριση λογαριασμού](https://account.windowsazure.com/subscriptions).
2. Επιλέξτε τη συνδρομή που θέλετε να αλλάξετε.
3. Επιλέξτε **Επεξεργασία λεπτομερειών συνδρομής**.

    ![Στιγμιότυπο οθόνης του Azure πληροφορίες συνδρομής, με "Επεξεργασία λεπτομέρειες συνδρομής" Επισήμανση](./media/billing-add-office-365-tenant-to-azure-subscription/s33_azure-edit-subscription-details.png)

4. Στο πλαίσιο **ΔΙΑΧΕΙΡΙΣΤΉ της ΥΠΗΡΕΣΊΑΣ** , πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου του νέου διαχειριστή υπηρεσίας.

    ![Στιγμιότυπο οθόνης από το παράθυρο διαλόγου "Επεξεργασία τη συνδρομή σας"](./media/billing-add-office-365-tenant-to-azure-subscription/s34_change-subscription-service-admin.png)

### <a name="associate-the-office-365-tenant-with-the-azure-subscription"></a>Συσχετισμός μισθωτή του Office 365 με τη συνδρομή του Azure
Για να συσχετίσετε το μισθωτή του Office 365 με τη συνδρομή Azure, ακολουθήστε τα παρακάτω βήματα:

1.  Είσοδος στην [πύλη διαχείρισης λογαριασμό](https://account.windowsazure.com/subscriptions) με τα διαπιστευτήρια διαχειριστή υπηρεσίας.
2.  Στο αριστερό παράθυρο, επιλέξτε την **Υπηρεσία καταλόγου ACTIVE DIRECTORY**.

    ![Στιγμιότυπο οθόνης της υπηρεσίας καταλόγου Active Directory καταχώρησης](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

    > [AZURE.NOTE] Δεν θα πρέπει να βλέπετε το μισθωτή του Office 365. Εάν βλέπετε το, μεταβείτε στο επόμενο βήμα.

    ![Στιγμιότυπο οθόνης από τον προεπιλεγμένο κατάλογο του Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s36-aad-tenant-default.png)

3. Προσθέστε μισθωτή του Office 365 Azure τη συνδρομή σας.

    μια. Επιλέξτε **ΔΗΜΙΟΥΡΓΊΑ** > **ΚΑΤΑΛΌΓΟΥ** > **ΔΗΜΙΟΥΡΓΊΑ ΠΡΟΣΑΡΜΟΣΜΈΝΟΥ**.

    ![Δημιουργία προσαρμοσμένης στιγμιότυπο οθόνης του Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)

    β. Στη σελίδα **Προσθήκη καταλόγου** , στην περιοχή **ΚΑΤΑΛΌΓΟΥ**, επιλέξτε **Χρήση υπάρχουσας καταλόγου**. Στη συνέχεια, επιλέξτε **είμαι έτοιμος να πραγματοποιήσει έξοδο τώρα**και επιλέξτε **ολοκλήρωσης** ![ολοκλήρωση εικονίδιο](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Στιγμιότυπο οθόνης της "Χρήση υπάρχουσας καταλόγου"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)

    c. Αφού έχετε πραγματοποιήσει έξοδο, πραγματοποιήστε είσοδο με τα διαπιστευτήρια του καθολικού διαχειριστή του μισθωτή του Office 365.

    ![Στιγμιότυπο οθόνης του Office 365 καθολικός διαχειριστής εισόδου](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)

    d. Επιλέξτε **συνέχεια**.

    ![Στιγμιότυπο οθόνης της επαλήθευσης](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)

    ε. Επιλέξτε **Έξοδος τώρα**.

    ![Στιγμιότυπο οθόνης της sign-out](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)

    f. Είσοδος στην [πύλη διαχείρισης λογαριασμό](https://account.windowsazure.com/subscriptions) με τα διαπιστευτήρια διαχειριστή υπηρεσίας.

    ![Στιγμιότυπο οθόνης του Azure εισόδου](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

    γ. Θα πρέπει να βλέπετε μισθωτή του Office 365 στον πίνακα εργαλείων.

    ![Στιγμιότυπο οθόνης του πίνακα εργαλείων](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

4. Αλλάξτε τον κατάλογο που σχετίζεται με τη συνδρομή του Azure.

    μια. Επιλέξτε **Ρυθμίσεις**.

    ![Στιγμιότυπο οθόνης του Azure εικονίδιο κλασική ρυθμίσεις πύλης](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)

    β. Επιλέξτε τη συνδρομή σας στο Azure και, στη συνέχεια, επιλέξτε **ΕΠΕΞΕΡΓΑΣΊΑ ΚΑΤΑΛΌΓΟΥ**.
    ![Στιγμιότυπο οθόνης του Azure καταλόγου επεξεργασία εγγραφής](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)

    c. Επιλέξτε **Επόμενο** ![Επόμενο εικονίδιο](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).

    ![Στιγμιότυπο οθόνης της "Αλλαγή ο κατάλογος σχετικό"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)

    > [AZURE.WARNING] Θα λάβετε μια προειδοποίηση ότι θα καταργηθούν όλες οι διαχειριστές από κοινού.

    ![Azure-Επιβεβαιώστε--αντιστοίχιση καταλόγου](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)

    >[AZURE.WARNING] Επιπλέον, όλοι οι χρήστες [έλεγχο πρόσβασης βάσει ρόλων (RBAC)](./active-directory/role-based-access-control-configure.md) με την access εκχωρημένες στις υπάρχουσες ομάδες πόρων επίσης θα καταργηθούν. Ωστόσο, η προειδοποίηση που λαμβάνετε αναφορές μόνο την κατάργηση των διαχειριστών από κοινού.

    ![στους οποίους έχουν ανατεθεί-χρήστες-καταργηθεί--ομάδες πόρων](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)

    d. Επιλέξτε **Ολοκλήρωση** ![ολοκλήρωση εικονίδιο](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

5. Τώρα μπορείτε να προσθέσετε το εταιρικοί λογαριασμοί Office 365 ως συνδιαχειριστών στο μισθωτή του Azure Active Directory.

    μια. Επιλέξτε την καρτέλα **ΔΙΑΧΕΙΡΙΣΤΈΣ** και, στη συνέχεια, επιλέξτε **ΠΡΟΣΘΉΚΗ**.

    ![Στιγμιότυπο οθόνης του Azure καρτέλα διαχειριστές κλασική ρυθμίσεις πύλης](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)

    β. Εισαγάγετε έναν εταιρικό λογαριασμό του μισθωτή του Office 365, επιλέξτε τη συνδρομή Azure και, στη συνέχεια, επιλέξτε **ολοκλήρωσης** ![ολοκλήρωση εικονίδιο](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Στιγμιότυπο οθόνης του Azure Προσθήκη διαλόγου Διαχειριστής από κοινού](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)

    c. Επιστρέψτε στην καρτέλα **ΔΙΑΧΕΙΡΙΣΤΈΣ** . Θα πρέπει να βλέπετε το εταιρικό λογαριασμό σας εμφανίζονται ως διαχειριστής από κοινού.

    ![Στιγμιότυπο οθόνης της καρτέλας "διαχειριστές"](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)

6. Στη συνέχεια, μπορείτε να ελέγξετε πρόσβασης με το διαχειριστή του από κοινού.

    μια. Έξοδος από την πύλη Διαχείριση λογαριασμού.

    β. Ανοίξτε την [πύλη διαχείρισης λογαριασμό](https://account.windowsazure.com/subscriptions) ή την [πύλη του Azure](https://portal.azure.com/).

    c. Εάν το Azure στη σελίδα εισόδου έχει μια σύνδεση **Πραγματοποιήστε είσοδο με τον εταιρικό λογαριασμό σας**, επιλέξτε τη σύνδεση. Διαφορετικά, μεταβείτε σε αυτό το βήμα.

    ![Στιγμιότυπο οθόνης του Azure στη σελίδα εισόδου](./media/billing-add-office-365-tenant-to-azure-subscription/3-sign-in-to-azure.png)

    d. Πληκτρολογήστε τα διαπιστευτήρια του διαχειριστή από κοινού και, στη συνέχεια, επιλέξτε **Είσοδος**.

    ![Στιγμιότυπο οθόνης του Azure στη σελίδα εισόδου](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="next-steps"></a>Επόμενα βήματα
Σχετικές σενάρια περιλαμβάνουν τα εξής:

- Έχετε ήδη μια συνδρομή στο Office 365 και είστε έτοιμοι για μια συνδρομή στο Azure, αλλά θέλετε να χρησιμοποιήσετε τους υπάρχοντες λογαριασμούς χρήστη του Office 365 για τη συνδρομή σας Azure.
- Είστε ένα Azure συνδρομητή και θέλετε να λάβετε μια συνδρομή στο Office 365 για τους χρήστες στην υπάρχουσα παρουσία Azure Active Directory.

Για να μάθετε πώς μπορείτε να εκτελέσετε αυτές τις εργασίες, ανατρέξτε στο θέμα [Χρήση λογαριασμού του υπάρχοντος Office 365 με τη συνδρομή σας Azure, ή το αντίστροφο](billing-use-existing-office-365-account-azure-subscription.md).
