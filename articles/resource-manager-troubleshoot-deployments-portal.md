<properties
   pageTitle="Προβολή λειτουργίες ανάπτυξης με την πύλη | Microsoft Azure"
   description="Περιγράφει τον τρόπο για να χρησιμοποιήσουν την πύλη του Azure για τον εντοπισμό σφαλμάτων από διαχειριστή πόρων ανάπτυξης."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-portal"></a>Λειτουργίες ανάπτυξης προβολή με την πύλη Azure

> [AZURE.SELECTOR]
- [Πύλη](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API](resource-manager-troubleshoot-deployments-rest.md)

Μπορείτε να προβάλετε τις εργασίες για μια ανάπτυξη μέσω της πύλης Azure. Ενδέχεται να πιο χρήσιμο να προβάλετε τις λειτουργίες όταν έχετε λάβει ένα μήνυμα σφάλματος κατά την ανάπτυξη, ώστε να σε αυτό το άρθρο εστιάζει στην προβολή λειτουργίες που έχουν αποτύχει. Η πύλη παρέχει ένα περιβάλλον εργασίας που σας επιτρέπει να βρείτε τα σφάλματα και να προσδιορίσετε πιθανά επιδιορθώσεις εύκολα.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="use-deployment-operations-to-troubleshoot"></a>Χρήση λειτουργιών ανάπτυξης για την αντιμετώπιση προβλημάτων

Για να δείτε τις λειτουργίες ανάπτυξης, χρησιμοποιήστε τα ακόλουθα βήματα:

1. Για την ομάδα των πόρων που περιλαμβάνονται στην ανάπτυξη, παρατηρήστε την κατάσταση της τελευταίας ανάπτυξης. Μπορείτε να επιλέξετε αυτήν την κατάσταση για να δείτε περισσότερες λεπτομέρειες.

    ![κατάσταση ανάπτυξης](./media/resource-manager-troubleshoot-deployments-portal/deployment-status.png)

2. Θα δείτε το πρόσφατα ιστορικό ανάπτυξης. Επιλέξτε την ανάπτυξη που απέτυχε.

    ![κατάσταση ανάπτυξης](./media/resource-manager-troubleshoot-deployments-portal/select-deployment.png)

3. Απέτυχε η επιλογή **. Κάντε κλικ εδώ για λεπτομέρειες** για να δείτε μια περιγραφή των γιατί απέτυχε η ανάπτυξη. Στην παρακάτω εικόνα, η εγγραφή DNS δεν είναι μοναδικό.  

    ![Προβολή αποτυχίας ανάπτυξης](./media/resource-manager-troubleshoot-deployments-portal/view-error.png)

    Αυτό το μήνυμα σφάλματος πρέπει να είναι αρκετά για να ξεκινήσετε την αντιμετώπιση προβλημάτων. Ωστόσο, εάν θέλετε περισσότερες λεπτομέρειες σχετικά με το ποιες εργασίες έχουν ολοκληρωθεί, μπορείτε να προβάλετε τις εργασίες, όπως φαίνεται στα παρακάτω βήματα.

4. Μπορείτε να προβάλετε όλες τις λειτουργίες ανάπτυξης στο το blade **ανάπτυξης** . Επιλέξτε οποιαδήποτε εργασία για να δείτε περισσότερες λεπτομέρειες.

    ![λειτουργίες προβολής](./media/resource-manager-troubleshoot-deployments-portal/view-operations.png)

    Σε αυτήν την περίπτωση, μπορείτε να δείτε ότι το λογαριασμό χώρου αποθήκευσης, εικονικού δικτύου και διαθεσιμότητα σύνολο δημιουργήθηκαν με επιτυχία. Απέτυχε η δημόσια διεύθυνση IP και άλλοι πόροι δεν έχουν Επιχειρήθηκε.

5. Μπορείτε να προβάλετε συμβάντα για την ανάπτυξη, επιλέγοντας **συμβάντα**.

    ![Προβολή συμβάντων](./media/resource-manager-troubleshoot-deployments-portal/view-events.png)

6. Δείτε όλα τα συμβάντα για την ανάπτυξη και επιλέξτε ένα για περισσότερες λεπτομέρειες.

    ![ανατρέξτε στο θέμα συμβάντα](./media/resource-manager-troubleshoot-deployments-portal/see-all-events.png)

## <a name="use-audit-logs-to-troubleshoot"></a>Χρήση των αρχείων καταγραφής ελέγχου για την αντιμετώπιση προβλημάτων

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Για να δείτε τα σφάλματα για μια ανάπτυξη, χρησιμοποιήστε τα παρακάτω βήματα:

1. Προβολή των αρχείων καταγραφής ελέγχου για μια ομάδα πόρων, επιλέγοντας το στοιχείο **Ελέγχου αρχείων καταγραφής**.

    ![Επιλέξτε αρχεία καταγραφής ελέγχου](./media/resource-manager-troubleshoot-deployments-portal/select-audit-logs.png)

2. Στο blade τα **Αρχεία καταγραφής ελέγχου** , θα δείτε μια σύνοψη των πρόσφατων λειτουργίες για όλες τις ομάδες πόρων στη συνδρομή σας. Περιλαμβάνει μια γραφική αναπαράσταση του χρόνου και της κατάστασης των εργασιών, καθώς και μια λίστα με τις λειτουργίες.

    ![Εμφάνιση ενεργειών](./media/resource-manager-troubleshoot-deployments-portal/audit-summary.png)

3. Μπορείτε να φιλτράρετε την προβολή των αρχείων καταγραφής ελέγχου στην εστίαση σε συγκεκριμένες συνθήκες. Επιλέξτε **φίλτρο** στο επάνω μέρος του blade **αρχείων καταγραφής ελέγχου** .

    ![αρχεία καταγραφής φίλτρου](./media/resource-manager-troubleshoot-deployments-portal/filter-logs.png)

4. Από το **φίλτρο** blade, επιλέξτε συνθήκες για να περιορίσετε την προβολή των αρχείων καταγραφής ελέγχου σε μόνο αυτές τις εργασίες που θέλετε να δείτε. Για παράδειγμα, μπορείτε να φιλτράρετε λειτουργιών, για να εμφανίζονται μόνο οι σφαλμάτων για την ομάδα πόρων.

    ![Ορισμός επιλογών φίλτρου](./media/resource-manager-troubleshoot-deployments-portal/set-filter.png)

5. Μπορείτε να φιλτράρετε περαιτέρω λειτουργίες, ορίζοντας ένα χρονικό διάστημα. Η παρακάτω εικόνα φιλτράρει την προβολή σε ένα συγκεκριμένο χρονικό διάστημα 20 λεπτών.

    ![ρύθμιση ώρας](./media/resource-manager-troubleshoot-deployments-portal/select-time.png)

6. Μπορείτε να επιλέξετε μία από τις εργασίες στη λίστα. Επιλέξτε τη λειτουργία που περιέχει το σφάλμα που θέλετε να ερευνήσετε.

    ![Κάντε κλικ στην επιλογή](./media/resource-manager-troubleshoot-deployments-portal/select-operation.png)
  
7. Θα δείτε όλα τα συμβάντα για αυτήν τη λειτουργία. Παρατηρήστε τα **Αναγνωριστικά συσχέτισης** στη σύνοψη. Αυτό το Αναγνωριστικό χρησιμοποιείται για την παρακολούθηση συμβάντων για τις σχετικές. Μπορεί να είναι χρήσιμες κατά την εργασία με την τεχνική υποστήριξη για την αντιμετώπιση προβλημάτων του ζητήματος. Μπορείτε να επιλέξετε οποιοδήποτε συμβάν για να δείτε λεπτομέρειες σχετικά με το συμβάν.

    ![Επιλέξτε το συμβάν](./media/resource-manager-troubleshoot-deployments-portal/select-event.png)

8. Θα δείτε τις λεπτομέρειες σχετικά με το συμβάν. Συγκεκριμένα, προσέξτε στις **Ιδιότητες** για πληροφορίες σχετικά με το σφάλμα.

    ![Εμφάνιση λεπτομερειών αρχείου καταγραφής ελέγχου](./media/resource-manager-troubleshoot-deployments-portal/audit-details.png)

Το φίλτρο που εφαρμόσατε στο αρχείο καταγραφής ελέγχου διατηρείται την επόμενη φορά που προβάλλετε, επομένως ίσως χρειαστεί να αλλάξετε αυτές τις τιμές για να διευρύνετε την προβολή των εργασιών.

## <a name="next-steps"></a>Επόμενα βήματα

- Για βοήθεια σχετικά με την επίλυση σφαλμάτων συγκεκριμένη ανάπτυξη, ανατρέξτε στο θέμα [Επίλυση συνηθισμένων σφαλμάτων κατά την ανάπτυξη πόρων για Azure με τη διαχείριση πόρων Azure](resource-manager-common-deployment-errors.md).
- Για να μάθετε σχετικά με τη χρήση των αρχείων καταγραφής ελέγχου για την παρακολούθηση της άλλους τύπους ενεργειών, ανατρέξτε στο θέμα [ελέγχου λειτουργιών με τη διαχείριση πόρων](resource-group-audit.md).
- Για να επικυρώσετε την ανάπτυξη πριν από την εκτέλεση του, ανατρέξτε στο θέμα [ανάπτυξη μιας ομάδας πόρων με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy.md).
