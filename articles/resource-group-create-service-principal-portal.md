<properties
   pageTitle="Δημιουργία υπηρεσίας κεφαλαίου στην πύλη | Microsoft Azure"
   description="Περιγράφει τον τρόπο δημιουργίας μιας νέας εφαρμογής υπηρεσίας καταλόγου Active Directory και την υπηρεσία κεφάλαιο που μπορούν να χρησιμοποιηθούν με το στοιχείο ελέγχου πρόσβασης βάσει ρόλων στη Διαχείριση Azure πόρων για να διαχειριστείτε την πρόσβαση σε πόρους."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# <a name="use-portal-to-create-active-directory-application-and-service-principal-that-can-access-resources"></a>Χρήση πύλης για τη δημιουργία εφαρμογής υπηρεσίας καταλόγου Active Directory και κεφάλαιο υπηρεσιών που έχουν πρόσβαση σε πόρους

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Πύλη](resource-group-create-service-principal-portal.md)


Όταν έχετε μια εφαρμογή που πρέπει να έχετε πρόσβαση ή να τροποποιήσετε πόρους, πρέπει να ρυθμίσετε μια εφαρμογή του Active Directory (AD) και να εκχωρήσετε τα απαιτούμενα δικαιώματα σε αυτήν. Αυτό το θέμα δείχνει πώς μπορείτε να εκτελέσετε αυτά τα βήματα μέσω της πύλης. Προς το παρόν, πρέπει να χρησιμοποιήσετε την κλασική πύλη για να δημιουργήσετε μια νέα εφαρμογή υπηρεσίας καταλόγου Active Directory και, στη συνέχεια, μεταβείτε στην πύλη του Azure για να αντιστοιχίσετε ένα ρόλο στην εφαρμογή. 

> [AZURE.NOTE] Τα βήματα σε αυτό το θέμα ισχύουν μόνο κατά τη χρήση της **κλασικής πύλη** για τη δημιουργία της εφαρμογής AD. **Εάν χρησιμοποιείτε την πύλη του Azure για τη δημιουργία της εφαρμογής AD, τα παρακάτω βήματα θα αποτύχει.** 
>
> Που μπορεί να είναι ευκολότερο για να ρυθμίσετε την εφαρμογή AD και την υπηρεσία κεφαλαίου μέσω του [PowerShell](resource-group-authenticate-service-principal.md) ή [Azure CLI](resource-group-authenticate-service-principal-cli.md), ειδικά εάν θέλετε να χρησιμοποιήσετε ένα πιστοποιητικό για έλεγχο ταυτότητας. Αυτό το θέμα δεν δείχνει πώς μπορείτε να χρησιμοποιήσετε ένα πιστοποιητικό.

Για επεξήγηση της υπηρεσίας καταλόγου Active Directory έννοιες, ανατρέξτε στο θέμα [εφαρμογή και των αντικειμένων κεφάλαιο υπηρεσίας](./active-directory/active-directory-application-objects.md). Για περισσότερες πληροφορίες σχετικά με τον έλεγχο ταυτότητας υπηρεσίας καταλόγου Active Directory, ανατρέξτε στο θέμα [Σενάρια ελέγχου ταυτότητας για Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Για λεπτομερή βήματα στην ενσωμάτωση μιας εφαρμογής σε Azure για τη διαχείριση των πόρων, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές να εξουσιοδότησης με το API διαχείρισης πόρων Azure](resource-manager-api-authentication.md).

## <a name="create-an-active-directory-application"></a>Δημιουργία μιας εφαρμογής υπηρεσίας καταλόγου Active Directory

1. Συνδεθείτε στο λογαριασμό σας στο Azure μέσω της [κλασικής πύλη](https://manage.windowsazure.com/). 

2. Βεβαιωθείτε ότι γνωρίζετε την προεπιλεγμένη υπηρεσία καταλόγου Active Directory για τη συνδρομή σας. Μπορείτε μόνο να εκχωρήσετε πρόσβαση για τις εφαρμογές στο ίδιο κατάλογο με τη συνδρομή σας. Επιλέξτε **Ρυθμίσεις** και αναζητήστε το όνομα του καταλόγου που σχετίζεται με τη συνδρομή σας.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς Azure συνδρομές συσχετίζονται με Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).
   
     ![Εύρεση του προεπιλεγμένου καταλόγου](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. Επιλέξτε την **Υπηρεσία καταλόγου Active Directory** από το αριστερό παράθυρο.

     ![Επιλέξτε την υπηρεσία καταλόγου Active Directory](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Επιλέξτε την υπηρεσία καταλόγου Active Directory που θέλετε να χρησιμοποιήσετε για τη δημιουργία της εφαρμογής. Εάν έχετε περισσότερες από μία υπηρεσίας καταλόγου Active Directory, δημιουργία της εφαρμογής στον κατάλογο προεπιλογή για τη συνδρομή σας.   

     ![Επιλέξτε καταλόγου](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. Για να προβάλετε τις εφαρμογές στον κατάλογό σας, επιλέξτε **εφαρμογές**.

     ![Προβολή εφαρμογών](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Εάν δεν έχετε δημιουργήσει μια εφαρμογή του σε αυτόν τον κατάλογο πριν, θα πρέπει να βλέπετε κάτι παρόμοιο στην παρακάτω εικόνα. Επιλέξτε **ΠΡΟΣΘΉΚΗ ΕΦΑΡΜΟΓΉΣ**

     ![Προσθήκη εφαρμογής](./media/resource-group-create-service-principal-portal/create-application.png)

     Εναλλακτικά, κάντε κλικ στην επιλογή **Προσθήκη** στο κάτω τμήμα του παραθύρου.

     ![Προσθήκη](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Επιλέξτε τον τύπο της εφαρμογής που θέλετε να δημιουργήσετε. Για αυτό το πρόγραμμα εκμάθησης, επιλέξτε **Προσθήκη εφαρμογής ανάπτυξη την εταιρεία μου**. 

     ![νέα εφαρμογή](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Δώστε ένα όνομα για την εφαρμογή και επιλέξτε τον τύπο της εφαρμογής που θέλετε να δημιουργήσετε. Για αυτό το πρόγραμμα εκμάθησης, δημιουργήστε μια **ΕΦΑΡΜΟΓΉ WEB ή/και το API WEB** και κάντε κλικ στο κουμπί Επόμενο. Εάν επιλέξετε **ΕΓΓΕΝΉ ΕΦΑΡΜΟΓΉ προγράμματος-ΠΕΛΆΤΗ**, τα υπόλοιπα βήματα αυτού του άρθρου θα συμφωνεί με την εμπειρία σας.

     ![όνομα εφαρμογής](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Συμπληρώστε τις ιδιότητες για την εφαρμογή σας. Για **Διεύθυνση URL ΕΙΣΌΔΟΥ Ενεργο**, δώστε το URI σε μια τοποθεσία web που περιγράφει την εφαρμογή σας. Η ύπαρξη της τοποθεσίας web δεν είναι επικύρωση. Για **URI Αναγνωριστικό Εφαρμογής**, δώστε το URI που προσδιορίζει την εφαρμογή σας.

     ![Ιδιότητες εφαρμογής](./media/resource-group-create-service-principal-portal/app-properties.png)

Έχετε δημιουργήσει την εφαρμογή σας.

## <a name="get-client-id-and-authentication-key"></a>Λήψη προγράμματος-πελάτη κλειδιού id και τον έλεγχο ταυτότητας

Κατά τη σύνδεση μέσω προγραμματισμού στο, χρειάζεστε το αναγνωριστικό για την εφαρμογή σας. Εάν η εφαρμογή θα εκτελείται στο δικό του διαπιστευτήρια, χρειάζεστε επίσης ένα κλειδί ελέγχου ταυτότητας.

1. Επιλέξτε την καρτέλα **Ρύθμιση παραμέτρων** για τη ρύθμιση παραμέτρων της εφαρμογής σας κωδικό πρόσβασης.

     ![ρύθμιση παραμέτρων εφαρμογής](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Αντιγράψτε το **Αναγνωριστικό υπολογιστή-ΠΕΛΆΤΗ**.
  
     ![Αναγνωριστικό υπολογιστή-πελάτη](./media/resource-group-create-service-principal-portal/client-id.png)

3. Εάν η εφαρμογή θα εκτελείται στο δικό του διαπιστευτήρια, κάντε κύλιση προς τα κάτω στην ενότητα **πλήκτρα** και επιλέξτε το χρονικό διάστημα που θέλετε τον κωδικό πρόσβασής σας για να είναι έγκυρες.

     ![πλήκτρα](./media/resource-group-create-service-principal-portal/create-key.png)

4. Επιλέξτε **Αποθήκευση** για να δημιουργήσετε τον αριθμό-κλειδί.

     ![Αποθήκευση](./media/resource-group-create-service-principal-portal/save-icon.png)

     Εμφανίζεται το κλειδί που αποθηκεύσατε και μπορείτε να το αντιγράψετε. Δεν είναι δυνατή η ανάκτηση του κλειδιού αργότερα επομένως, αντιγράψτε την τώρα.

     ![Αποθήκευση αριθμού-κλειδιού](./media/resource-group-create-service-principal-portal/save-key.png)

## <a name="get-tenant-id"></a>Λήψη αναγνωριστικό μισθωτή

Κατά τη σύνδεση μέσω προγραμματισμού στο, πρέπει να περάσει το αναγνωριστικό μισθωτή με την αίτηση ελέγχου ταυτότητας. Για εφαρμογές Web και εφαρμογές API Web, μπορείτε να ανακτήσετε το αναγνωριστικό μισθωτή, επιλέγοντας **τα τελικά σημεία προβολής** στο κάτω μέρος της οθόνης και την ανάκτηση το αναγνωριστικό όπως φαίνεται στην παρακάτω εικόνα.  

   ![αναγνωριστικό μισθωτή](./media/resource-group-create-service-principal-portal/save-tenant.png)

Μπορείτε επίσης να ανακτήσετε το αναγνωριστικό μισθωτή μέσω του PowerShell:

    Get-AzureRmSubscription

Εναλλακτικά, Azure CLI:

    azure account show --json

## <a name="set-delegated-permissions"></a>Ορισμός με ανάθεση δικαιωμάτων

Εάν η εφαρμογή σας αποκτά πρόσβαση σε πόρους εκ μέρους συνδεδεμένοι στο χρήστη, πρέπει να εκχωρήσετε την εφαρμογή σας το δικαίωμα "Ανάθεση" για να αποκτήσετε πρόσβαση σε άλλες εφαρμογές. Εκχωρείτε πρόσβαση σε αυτό στην ενότητα **δικαιώματα σε άλλες εφαρμογές** στην καρτέλα " **Ρύθμιση παραμέτρων** ". Από προεπιλογή, μια ανάθεση δικαιωμάτων έχει ήδη ενεργοποιηθεί για το Azure Active Directory. Αφήστε αυτό το δικαίωμα ανάθεση που δεν έχουν αλλάξει.

1. Επιλέξτε **Προσθήκη εφαρμογής**.

2. Από τη λίστα, επιλέξτε το **API διαχείρισης της υπηρεσίας Windows Azure**. Στη συνέχεια, επιλέξτε το εικονίδιο ολοκλήρωσης.

      ![Επιλέξτε εφαρμογή](./media/resource-group-create-service-principal-portal/select-app.png)

3. Στην αναπτυσσόμενη λίστα για ανάθεση δικαιώματα, επιλέξτε **Διαχείριση της υπηρεσίας πρόσβασης Azure ως εταιρεία**.

      ![Επιλέξτε δικαίωμα](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. Αποθηκεύστε την αλλαγή.

## <a name="assign-application-to-role"></a>Αντιστοίχιση εφαρμογών σε ρόλο

Εάν εκτελείται η εφαρμογή σας στην περιοχή τα διαπιστευτήριά δικό του, πρέπει να αντιστοιχίσετε την εφαρμογή σε ένα ρόλο. Αποφασίστε ποια ρόλος αντιπροσωπεύει τα κατάλληλα δικαιώματα για την εφαρμογή. Για να μάθετε σχετικά με τους ρόλους διαθέσιμη, ανατρέξτε στο θέμα [RBAC: ενσωματωμένη ρόλους](./active-directory/role-based-access-built-in-roles.md). 

Για να αντιστοιχίσετε ένα ρόλο σε μια εφαρμογή, πρέπει να έχετε τα σωστά δικαιώματα. Συγκεκριμένα, πρέπει να έχετε `Microsoft.Authorization/*/Write` πρόσβασης που παρέχεται μέσω από το ρόλο [κατόχου](./active-directory/role-based-access-built-in-roles.md#owner) ή το ρόλο [Διαχειριστή πρόσβασης χρήστη](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) . Ο ρόλος συμβολής δεν έχει τη σωστή πρόσβαση.

Μπορείτε να ορίσετε το εύρος στο επίπεδο τη συνδρομή, ομάδα πόρων ή πόρου. Δικαιώματα μεταβιβάζονται χαμηλότερα επίπεδα εμβέλειας. Για παράδειγμα, προσθέτοντας μια εφαρμογή για το ρόλο του προγράμματος ανάγνωσης για μια ομάδα πόρων σημαίνει ότι το να διαβάσετε την ομάδα των πόρων και πόροι που περιέχει.

1. Για να αντιστοιχίσετε την εφαρμογή σε ένα ρόλο, εναλλαγή από την πύλη κλασική [Azure πύλη](https://portal.azure.com).

1. Ελέγξει τα δικαιώματά σας για να βεβαιωθείτε ότι μπορείτε να εκχωρήσετε το κεφάλαιο υπηρεσίας σε ένα ρόλο. Επιλέξτε τα **δικαιώματά μου** για το λογαριασμό σας.

    ![Επιλέξτε τα δικαιώματά μου](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. Προβάλετε τα δικαιώματα που έχουν εκχωρηθεί για το λογαριασμό σας. Όπως σημειώθηκε προηγουμένως, πρέπει να ανήκουν οι ρόλοι κάτοχος ή ο διαχειριστής πρόσβασης χρήστη, ή να έχετε ένα προσαρμοσμένο ρόλο που εκχωρεί πρόσβαση εγγραφής για Microsoft.Authorization. Η παρακάτω εικόνα δείχνει ένα λογαριασμό που έχει αντιστοιχιστεί το ρόλο του συνεργάτη για τη συνδρομή, το οποίο δεν είναι κατάλληλα δικαιώματα για να αντιστοιχίσετε μια εφαρμογή σε ένα ρόλο.

    ![Εμφάνιση δικαιώματά μου](./media/resource-group-create-service-principal-portal/show-permissions.png)

     Εάν δεν έχετε τα σωστά δικαιώματα για να εκχωρήσετε πρόσβαση σε μια εφαρμογή, πρέπει να κάνετε οποιαδήποτε αίτηση ότι ο διαχειριστής τη συνδρομή σας προσθέτει στη το ρόλο διαχειριστή πρόσβασης χρήστη ή αίτηση που ένας διαχειριστής εκχωρεί πρόσβαση στην εφαρμογή.

1. Μεταβείτε στο επίπεδο του πεδίου που θέλετε να αντιστοιχίσετε την εφαρμογή. Για να αντιστοιχίσετε ένα ρόλο στο εύρος συνδρομής, επιλέξτε **συνδρομές**.

     ![Επιλέξτε τη συνδρομή](./media/resource-group-create-service-principal-portal/select-subscription.png)

     Επιλέξτε τη συγκεκριμένη συνδρομή για να αντιστοιχίσετε την εφαρμογή.

     ![Επιλέξτε τη συνδρομή για ανάθεση](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

     Επιλέξτε το εικονίδιο **πρόσβασης** στην επάνω δεξιά γωνία.

     ![Επιλέξτε πρόσβαση](./media/resource-group-create-service-principal-portal/select-access.png)
     
     Εναλλακτικά, για να αντιστοιχίσετε ένα ρόλο στο εύρος ομάδα πόρων, μεταβείτε σε μια ομάδα πόρων. Από την ομάδα blade πόρου, επιλέξτε **Έλεγχος πρόσβασης**.

     ![Επιλέξτε τους χρήστες](./media/resource-group-create-service-principal-portal/select-users.png)

     Τα ακόλουθα βήματα είναι η ίδια για κάθε πεδίο.

2. Επιλέξτε **Προσθήκη**.

     ![Επιλέξτε "Προσθήκη"](./media/resource-group-create-service-principal-portal/select-add.png)

3. Επιλέξτε το ρόλο του **προγράμματος ανάγνωσης** (ή όποιο ρόλο που θέλετε να αντιστοιχίσετε την εφαρμογή για να).

     ![Επιλέξτε το ρόλο](./media/resource-group-create-service-principal-portal/select-role.png)

4. Όταν εμφανιστεί η λίστα των χρηστών που μπορείτε να προσθέσετε το ρόλο πρώτα, δεν θα βλέπετε τις εφαρμογές. Θα δείτε μόνο χρήστες και ομάδας.

     ![Δείξτε στους χρήστες](./media/resource-group-create-service-principal-portal/show-users.png)

5. Για να βρείτε την εφαρμογή σας, θα πρέπει να το αναζητήσετε. Αρχίστε να πληκτρολογείτε το όνομα της εφαρμογής σας και θα αλλάξει τη λίστα των διαθέσιμων επιλογών. Επιλέξτε την εφαρμογή σας, όταν την βλέπετε στη λίστα.

     ![Εκχώρηση ρόλων](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Επιλέξτε **εντάξει** για να ολοκληρώσετε την εκχώρηση του ρόλου. Τώρα θα πρέπει να βλέπετε την εφαρμογή στη λίστα των χρήσεων στους οποίους έχουν ανατεθεί σε ένα ρόλο για την ομάδα πόρων.


Για περισσότερες πληροφορίες σχετικά με την εκχώρηση χρηστών και εφαρμογές σε ρόλους μέσω της πύλης, ανατρέξτε στο θέμα [Χρήση εκχωρήσεις ρόλων για τη διαχείριση της πρόσβασης σε πόρους Azure τη συνδρομή σας](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal).

## <a name="sample-applications"></a>Δείγματα εφαρμογών

Τα ακόλουθα δείγματα εφαρμογών δείχνουν πώς μπορείτε να συνδεθείτε ως το κεφάλαιο υπηρεσίας.

**.NET**

- [Ανάπτυξη μιας SSH με δυνατότητα Εικονική με ένα πρότυπο με το .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Διαχείριση Azure τους πόρους και τις ομάδες πόρων με .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Γρήγορα αποτελέσματα με τους πόρους - ανάπτυξη χρήση προτύπου για τη διαχείριση πόρων Azure - στο Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Γρήγορα αποτελέσματα με τους πόρους - Διαχείριση ομάδας πόρων - στο Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Ανάπτυξη μιας SSH με δυνατότητα Εικονική με ένα πρότυπο στο Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Διαχείριση Azure πόρου και πόρου ομάδες με Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Ανάπτυξη μιας SSH με δυνατότητα Εικονική με ένα πρότυπο στο Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Διαχείριση Azure τους πόρους και τις ομάδες πόρων με Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Κείμενο φωνητικής γραφής**

- [Ανάπτυξη μιας SSH με δυνατότητα Εικονική με ένα πρότυπο στο κείμενο φωνητικής γραφής](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Διαχείριση Azure πόρου και πόρου ομάδες με κείμενο φωνητικής γραφής](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε σχετικά με τον καθορισμό πολιτικές ασφαλείας, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης βάσει ρόλων Azure](./active-directory/role-based-access-control-configure.md).  
- Για μια επίδειξη με βίντεο σχετικά με αυτά τα βήματα, ανατρέξτε στο θέμα [Ενεργοποίηση μέσω προγραμματισμού διαχείρισης ενός πόρου Azure με Azure Active Directory](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory).

