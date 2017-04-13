<properties
    pageTitle="Κατανόηση των χρεώσεων Azure εξωτερική υπηρεσία | Microsoft Azure"
    description="Μάθετε σχετικά με τις χρεώσεις από εξωτερικές υπηρεσίες, παλαιότερα γνωστό ως Marketplace, χρεώσεις στο Azure."
    services=""
    documentationCenter=""
    authors="adpick"
    manager="felixwu"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="adpick"/>

# <a name="understand-your-azure-external-service-charges"></a>Κατανόηση των Azure εξωτερική υπηρεσία χρεώσεων

Σε αυτό το άρθρο εξηγεί τη χρέωση των εξωτερικών υπηρεσιών στο Azure. Εξωτερικές υπηρεσίες χρησιμοποιείται για να ονομάζεται παραγγελίες αγοράς. Εξωτερικές υπηρεσίες παρέχονται από προμηθευτές ανεξάρτητη υπηρεσία, αλλά να ενσωματωθούν πλήρως εντός του Azure περιβάλλον εμπορικής προσαρμογής. Μάθετε πώς μπορείτε να:

- Προσδιορισμός εξωτερικές υπηρεσίες
- Κατανόηση πώς η χρέωση διαφέρει από άλλους πόρους Azure
- Προβάλετε και να παρακολουθήσετε οποιαδήποτε που προσαύξηση κόστους από τη χρήση εξωτερικών υπηρεσιών
- Διαχείριση εξωτερική υπηρεσία παραγγελίες και πώς να πληρώσετε για τους

## <a name="what-are-azure-external-services"></a>Τι είναι οι εξωτερικές υπηρεσίες Azure;

Εξωτερικές υπηρεσίες χρησιμοποιείται για να ονομάζεται Azure Marketplace. Γενικά, να είναι υπηρεσίες που δημοσιεύονται από τρίτους κατασκευαστές διαθέσιμη για Azure. Για παράδειγμα, ClearDB και SendGrid είναι εξωτερικές υπηρεσίες που μπορείτε να αγοράσετε στο Azure, αλλά δεν δημοσιεύονται από τη Microsoft.

### <a name="identify-external-services"></a>Προσδιορισμός εξωτερικές υπηρεσίες

Κατά την προμήθεια μια νέα εξωτερική υπηρεσία ή έναν πόρο, εμφανίζεται μια προειδοποίηση:

![Προειδοποίηση αγοράς Marketplace](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

>[AZURE.NOTE] Εξωτερικές υπηρεσίες δημοσιεύονται από εταιρείες που δεν είναι της Microsoft, αλλά μερικές φορές προϊόντα της Microsoft επίσης κατηγοριοποιούνται ως εξωτερικές υπηρεσίες.

### <a name="external-services-are-billed-separately"></a>Εξωτερικές υπηρεσίες έχουν τιμολογηθεί ξεχωριστά

Εξωτερικές υπηρεσίες, υπολογίζονται ως μεμονωμένες παραγγελίες εντός Azure τη συνδρομή σας. Την περίοδο χρέωσης για κάθε υπηρεσία έχει οριστεί όταν αγοράζετε της υπηρεσίας. Δεν πρέπει να συγχέονται με την περίοδο χρέωσης της συνδρομής που έχετε αγοράσει το. Μπορείτε επίσης να λάβετε ξεχωριστή λογαριασμοί και η πιστωτική κάρτα σας θα χρεωθεί ξεχωριστά.

### <a name="each-external-service-has-a-different-billing-model"></a>Κάθε εξωτερική υπηρεσία έχει ένα διαφορετικό μοντέλο χρεώσεων

Ορισμένες υπηρεσίες έχουν τιμολογηθεί με διανεμητική τρόπο ενώ άλλες χρησιμοποιούν ένα μηνιαίο μοντέλο με πληρωμή. Χρειάζεστε μια πιστωτική κάρτα για Azure εξωτερικές υπηρεσίες, δεν μπορείτε να αγοράσετε εξωτερικές υπηρεσίες με πληρωμή με τιμολόγιο.

### <a name="you-cant-use-monthly-free-credits-for-external-services"></a>Δεν μπορείτε να χρησιμοποιήσετε μηνιαία δωρεάν πιστώσεων για εξωτερικές υπηρεσίες

Εάν χρησιμοποιείτε μια συνδρομή Azure που περιλαμβάνει [δωρεάν πιστώσεων](https://azure.microsoft.com/pricing/spending-limits/), δεν μπορεί να εφαρμοστεί σε εξωτερική υπηρεσία λογαριασμών. Χρησιμοποιήστε μια πιστωτική κάρτα για να αγοράσετε εξωτερικές υπηρεσίες.

## <a name="view-external-service-spending-and-history"></a>Προβολή εξωτερική υπηρεσία δαπανών και ιστορικό

Μπορείτε να προβάλετε μια λίστα με τις εξωτερικές υπηρεσίες που βρίσκονται σε κάθε συνδρομή εντός του [Azure πύλη](https://portal.azure.com/): 

1. Πραγματοποιήστε είσοδο στο το [Azure πύλη](https://portal.azure.com/) και [μεταβείτε στις επιλογές του blade **χρέωση** ](https://portal.azure.com/?flight=1#blade/Microsoft_Azure_Billing/BillingBlade).

    ![Επιλέξτε χρέωση στο μενού διανομέα](./media/billing-understand-your-azure-marketplace-charges/billing-button.png) 
  
2. Στην ενότητα **κόστους συνδρομής** , επιλέξτε τη συνδρομή στην οποία θέλετε να προβάλετε. 
   
    ![Επιλέξτε μια συνδρομή στο το blade χρεώσεων](./media/billing-understand-your-azure-marketplace-charges/select-sub.png)

3. Κάντε κλικ στην επιλογή **εξωτερικές υπηρεσίες**.

    ![Κάντε κλικ στην επιλογή εξωτερικές υπηρεσίες στο το blade συνδρομή](./media/billing-understand-your-azure-marketplace-charges/external-service-blade.png)

4. Θα πρέπει να βλέπετε κάθε μία από τις παραγγελίες σας εξωτερική υπηρεσία, το publisher ονόματος, που αγοράσατε από το επίπεδο υπηρεσιών, που σας έδωσε ο πόρος και την τρέχουσα κατάσταση σειρά. Επιλέξτε μια εξωτερική υπηρεσία για να δείτε προηγούμενες λογαριασμοί.

    ![Επιλέξτε μια εξωτερική υπηρεσία](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)

5. Από εδώ, μπορείτε να προβάλετε πέρα από τις ποσών χρέωσης με την ανάλυση φόρου.

    ![Προβολή εξωτερικές υπηρεσίες ιστορικό χρεώσεων](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="manage-payment-methods-for-external-service-orders"></a>Διαχειριστείτε τις μεθόδους πληρωμής για τις παραγγελίες εξωτερική υπηρεσία

Ενημερώστε τις μεθόδους πληρωμής για τις παραγγελίες εξωτερική υπηρεσία από το [Κέντρο του λογαριασμού](https://account.windowsazure.com/).

> [AZURE.NOTE] Εάν αγοράσατε τη συνδρομή σας με ένα λογαριασμό εργασίας ή σχολείου θα πρέπει να [επικοινωνήσετε με την υποστήριξη](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) για να κάνετε αλλαγές στη μέθοδο πληρωμής σας.

1. Πραγματοποιήστε είσοδο στο [Λογαριασμό κέντρο](https://account.windowsazure.com/) και [μεταβείτε στην καρτέλα **marketplace** ](https://account.windowsazure.com/Store)

    ![Επιλέξτε marketplace στο κέντρο του λογαριασμού](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)

2. Επιλέξτε την εξωτερική υπηρεσία που θέλετε να διαχειριστείτε

    ![Επιλέξτε την εξωτερική υπηρεσία που θέλετε να διαχειριστείτε](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)

3. Στη δεξιά πλευρά της σελίδας, κάντε κλικ στην επιλογή **Αλλαγή μεθόδου πληρωμής** . Αυτήν τη σύνδεση που εμφανίζει μια διαφορετική πύλη για τη διαχείριση της μεθόδου πληρωμής.
    
    ![Σύνοψη παραγγελίας](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)

4. Κάντε κλικ στην επιλογή **Επεξεργασία πληροφοριών** και ακολουθήστε τις οδηγίες για να ενημερώσετε τις πληροφορίες πληρωμής.

    ![Επιλέξτε Επεξεργασία πληροφοριών](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)
    
## <a name="cancel-an-external-service-order"></a>Ακύρωση παραγγελίας εξωτερική υπηρεσία

Εάν θέλετε να ακυρώσετε την παραγγελία σας εξωτερική υπηρεσία, πρέπει να διαγράψετε τον πόρο στην [πύλη του Azure](https://portal.azure.com).

![Διαγραφή πόρου](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a>Χρειάζεστε βοήθεια; Επικοινωνήστε με την υποστήριξη.

Εάν εξακολουθείτε να έχετε περισσότερες ερωτήσεις, λάβετε [Επικοινωνήστε με την υποστήριξη](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) για να λάβετε το πρόβλημα επιλυθεί γρήγορα.
