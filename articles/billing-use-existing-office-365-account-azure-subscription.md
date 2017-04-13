<properties
    pageTitle="Κοινή χρήση ενός ενιαίου μισθωτή Azure AD στις συνδρομές του Office 365 και το Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να κάνετε κοινή χρήση Azure AD μισθωτή του Office 365 και τους χρήστες με τη συνδρομή σας Azure, ή αντίστροφα"
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
    ms.date="08/17/2016"
    ms.author="cjiang"/>

# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>Χρήση ενός υπάρχοντος λογαριασμού Office 365 με τη συνδρομή σας Azure ή αντίστροφα
Σενάριο: Που ήδη έχετε συνδρομή στο Office 365 και είστε έτοιμοι για μια συνδρομή στο Azure, αλλά θέλετε να χρησιμοποιήσετε τους υπάρχοντες λογαριασμούς χρήστη του Office 365 για τη συνδρομή σας Azure. Εναλλακτικά, είναι μια Azure συνδρομητή και θέλετε να λάβετε μια συνδρομή στο Office 365 για τους χρήστες σας υπάρχοντα Azure υπηρεσίας καταλόγου Active Directory. Σε αυτό το άρθρο σάς δείχνει πόσο εύκολο είναι να επιτύχετε και τα δύο.

> [AZURE.NOTE] Σε αυτό το άρθρο δεν ισχύει για τους πελάτες σύμβαση Enterprise (EA). Εάν χρειάζεστε περισσότερη βοήθεια σε οποιοδήποτε σημείο σε αυτό το άρθρο, [Επικοινωνήστε με την υποστήριξη](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) για να μεταβείτε γρήγορα επιλυθεί το ζήτημά σας.


## <a name="quick-guidance"></a>Σύντομη καθοδήγηση

- Εάν έχετε ήδη μια συνδρομή στο Office 365 και θέλετε να εγγραφείτε για το Azure, χρησιμοποιήστε την επιλογή **συνδεθείτε με τον εταιρικό λογαριασμό σας** . Στη συνέχεια, συνεχίστε τη διαδικασία εγγραφής Azure με το λογαριασμό σας στο Office 365. Δείτε [λεπτομερείς οδηγίες παρακάτω σε αυτό το άρθρο](#s1).

- Εάν έχετε ήδη μια συνδρομή του Azure και θέλετε να λάβετε μια συνδρομή στο Office 365, πραγματοποιήστε είσοδο στο Office 365 με το λογαριασμό σας Azure. Στη συνέχεια, συνεχίστε με τα βήματα εγγραφής. Αφού ολοκληρώσετε την εγγραφή, η συνδρομή στο Office 365 προστίθενται την ίδια παρουσία Azure Active Directory που ανήκει Azure τη συνδρομή σας. Για περισσότερες πληροφορίες, δείτε την ενότητα [λεπτομερείς οδηγίες παρακάτω σε αυτό το άρθρο](#s2).

>[AZURE.NOTE] Για να λάβετε μια συνδρομή στο Office 365, ο λογαριασμός χρησιμοποιείτε για εγγραφής πρέπει να είστε μέλος του ρόλου καταλόγου καθολικός διαχειριστής ή διαχειριστής χρεώσεων στο μισθωτή του Azure Active Directory. [Μάθετε πώς μπορείτε να προσδιορίσετε το ρόλο του Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

Για να κατανοήσετε τι συμβαίνει όταν προσθέτετε μια συνδρομή με ένα λογαριασμό, ανατρέξτε στις [πληροφορίες φόντου αργότερα στο άρθρο](#background-information).

## <a name="detailed-steps"></a>Λεπτομερείς οδηγίες
<a id="s1"></a>
### <a name="scenario-1-office-365-users-who-plan-to-buy-azure"></a>Σενάριο 1: Χρήστες του Office 365 που σκοπεύετε να αγοράσετε Azure
Σε αυτό το σενάριο, υποθέσουμε ότι ο τοίχος Kelley είναι ένα χρήστη που έχει μια συνδρομή στο Office 365 και προγραμματισμού για να εγγραφείτε για Azure. Υπάρχουν δύο πρόσθετες ενεργοί χρήστες, Μαρία και Ανδριάνα. Ο λογαριασμός του Kelley είναι admin@contoso.onmicrosoft.com.

![Κέντρο διαχείρισης του Office 365 χρήστη](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Για να εγγραφείτε για Azure, ακολουθήστε τα παρακάτω βήματα:

1. Εγγραφείτε στο Azure στο [Azure.com](https://azure.microsoft.com/). Κάντε κλικ στην επιλογή **δοκιμάστε δωρεάν**. Στην επόμενη σελίδα, κάντε κλικ στην επιλογή **Έναρξη τώρα**.

    ![Δοκιμάστε Azure δωρεάν.](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. Κάντε κλικ στην επιλογή **Είσοδος με τον εταιρικό λογαριασμό σας**.

    ![Πραγματοποιήστε είσοδο στο Azure.](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. Πραγματοποιήστε είσοδο με λογαριασμό του Office 365. Σε αυτήν την περίπτωση, είναι Kelley του λογαριασμού στο Office 365.

    ![Πραγματοποιήστε είσοδο με λογαριασμό του Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. Συμπληρώστε τις πληροφορίες και να ολοκληρώσετε τη διαδικασία εγγραφής.

    ![Συμπληρώστε τις πληροφορίες και να ολοκληρώσετε εγγραφής.](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

    ![Κάντε κλικ στο κουμπί Έναρξη, τη διαχείριση των υπηρεσιών μου.](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Τώρα είστε έτοιμοι. Στην πύλη του Azure, θα πρέπει να βλέπετε τους ίδιους χρήστες που εμφανίζεται. Για να επαληθεύσετε, ακολουθήστε τα παρακάτω βήματα:

1. Κάντε κλικ στην επιλογή **Ξεκινήστε τη διαχείριση της υπηρεσίας μου** στην οθόνη εμφανίζεται παραπάνω.
2. Κάντε κλικ στο κουμπί **Αναζήτηση**και, στη συνέχεια, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Κάντε κλικ στο κουμπί Αναζήτηση και, στη συνέχεια, κάντε κλικ στην επιλογή υπηρεσία καταλόγου Active Directory.](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Κάντε κλικ στην επιλογή **ΧΡΉΣΤΕΣ**.

    ![Στην καρτέλα χρήστες](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. Όλοι οι χρήστες, συμπεριλαμβανομένων των Kelley, εμφανίζονται όπως αναμένεται.

    ![Λίστα των χρηστών](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### <a name="scenario-2-azure-users-who-plan-to-buy-office-365"></a>Σενάριο 2: Azure χρήστες που σκοπεύετε να αγοράσετε το Office 365

Σε αυτό το σενάριο, ο τοίχος Kelley είναι ένας χρήστης που έχει μια συνδρομή με το λογαριασμό του Azure admin@contoso.onmicrosoft.com. Kelley θέλει να εγγραφείτε στο Office 365 και να χρησιμοποιήσετε το ίδιο κατάλογο που έχει ήδη με το Azure.

>[AZURE.NOTE] Για να λάβετε μια συνδρομή στο Office 365, το λογαριασμό που χρησιμοποιείτε για είσοδο πρέπει να είστε μέλος του ρόλου καταλόγου καθολικός διαχειριστής ή διαχειριστής χρεώσεων στο μισθωτή του Azure Active Directory. [Μάθετε πώς μπορείτε να γνωρίζετε ο ρόλος στο Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

![Ρυθμίσεις Azure πύλης εγγραφής](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Azure Active Directory τους χρήστες της πύλης](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Για να εγγραφείτε στο Office 365, ακολουθήστε τα παρακάτω βήματα:

1. Μεταβείτε στη [σελίδα του προϊόντος Office 365](https://products.office.com/business)και, στη συνέχεια, επιλέξτε ένα σχέδιο που είναι κατάλληλη για εσάς.
2. Αφού επιλέξετε το πρόγραμμα, εμφανίζεται στην επόμενη σελίδα. Δεν συμπληρώστε τη φόρμα. Στην επάνω δεξιά γωνία της σελίδας, κάντε κλικ στην επιλογή **Είσοδος** .

    ![Σελίδα δοκιμαστικής έκδοσης του Office 365](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. Πραγματοποιήστε είσοδο με τα διαπιστευτήρια λογαριασμού σας. Σε αυτό το παράδειγμα, είναι Kelley του λογαριασμού.

    ![Είσοδος στο Office 365](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. Κάντε κλικ στην επιλογή **δοκιμάστε τώρα**.

    ![Επιβεβαιώστε την παραγγελία σας για το Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. Στη σελίδα αποδεικτικό σειρά, κάντε κλικ στην επιλογή **Continue**.

    ![Παραλαβή Office 365](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Τώρα είστε έτοιμοι. Στο Κέντρο διαχείρισης του Office 365, θα πρέπει να βλέπετε τους χρήστες από τον κατάλογο Contoso εμφανίζονται ως ενεργοί χρήστες. Για να επαληθεύσετε, ακολουθήστε τα παρακάτω βήματα:

1. Ανοίξτε το Κέντρο διαχείρισης του Office 365.
2. Αναπτύξτε **τους ΧΡΉΣΤΕΣ**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ενεργοί χρήστες**.

    ![Χρήστες Κέντρο διαχείρισης του Office 365](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>Πώς μπορείτε να γνωρίζετε ο ρόλος σας στο σας Azure Active Directory

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com/).
2. Κάντε κλικ στο κουμπί **Αναζήτηση**και, στη συνέχεια, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

    ![Υπηρεσία καταλόγου Active Directory στην πύλη του Azure](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Κάντε κλικ στην επιλογή **ΧΡΉΣΤΕΣ**.

    ![Οι χρήστες Azure πύλης προεπιλεγμένη υπηρεσία καταλόγου Active Directory](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. Κάντε κλικ στο χρήστη. Σε αυτό το παράδειγμα, ο χρήστης είναι Kelley τοίχου.

    Παρατηρήστε ότι το πεδίο **ΕΤΑΙΡΙΚΌ**του ρόλου.

    ![Ταυτότητα χρήστη Azure πύλης](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Πληροφορίες σχετικά με τις συνδρομές Azure και το Office 365
Office 365 και του Azure χρησιμοποιήστε την υπηρεσία καταλόγου Active Directory του Azure για τη Διαχείριση χρηστών και συνδρομών. Εξετάστε το ενδεχόμενο ενός καταλόγου Azure ως κοντέινερ στο οποίο μπορείτε να ομαδοποιήσετε χρήστες και συνδρομών. Για να χρησιμοποιήσετε τον ίδιο λογαριασμό χρήστη για τις συνδρομές σας Azure και το Office 365, πρέπει να βεβαιωθείτε ότι δημιουργούνται τις συνδρομές στον ίδιο κατάλογο. Λάβετε υπόψη τα εξής σημεία:

- Μια συνδρομή λαμβάνει δημιουργηθεί κάτω από έναν κατάλογο, όχι το αντίστροφο.
- Οι χρήστες που ανήκουν σε καταλόγους, όχι το αντίστροφο.
- Μια συνδρομή καταλήξει στον κατάλογο του χρήστη που δημιουργεί τη συνδρομή. Ως αποτέλεσμα, συνδρομή στο Office 365 είναι συνδεδεμένη με τον ίδιο λογαριασμό ως Azure τη συνδρομή σας, όταν χρησιμοποιείτε αυτόν το λογαριασμό για να δημιουργήσετε τη συνδρομή στο Office 365.

![Πληροφορίες φόντου](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς Azure συνδρομές συσχετίζονται με Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

>[AZURE.NOTE] Οι μεμονωμένοι χρήστες στον κατάλογο ανήκουν Azure συνδρομές.

>[AZURE.NOTE] Συνδρομές του Office 365 ανήκουν τον ίδιο τον κατάλογο. Εάν οι χρήστες μέσα στον κατάλογο έχουν τα απαραίτητα δικαιώματα, μπορούν να λειτουργούν σε αυτές τις συνδρομές.

## <a name="next-steps"></a>Επόμενα βήματα
Εάν αποκτήσατε το Azure και το Office 365 συνδρομές ξεχωριστά στο παρελθόν και θέλετε να έχουν πρόσβαση στο μισθωτή του Office 365 από το Azure συνδρομή, ανατρέξτε στο θέμα [συσχετίσετε ένα μισθωτή του Office 365 με μια συνδρομή στο Azure](billing-add-office-365-tenant-to-azure-subscription.md).

> [AZURE.NOTE] Εάν εξακολουθείτε να έχετε ερωτήσεις, [Επικοινωνήστε με την υποστήριξη](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) για να μεταβείτε γρήγορα επιλυθεί το ζήτημά σας.
