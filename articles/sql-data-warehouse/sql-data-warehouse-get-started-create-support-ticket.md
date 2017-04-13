<properties
   pageTitle="Πώς μπορείτε να δημιουργήσετε ένα δελτίο υποστήριξης για αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Πώς να δημιουργήσετε ένα δελτίο υποστήριξης στο αποθήκη δεδομένων του SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess"/>

# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Πώς μπορείτε να δημιουργήσετε ένα δελτίο υποστήριξης για αποθήκη δεδομένων του SQL
 
Εάν αντιμετωπίζετε προβλήματα με την αποθήκη δεδομένων του SQL, ανατρέξτε δημιουργείτε μια υποστήριξη δελτίων, έτσι ώστε να μας μηχανικής ομάδας μπορούν να σας βοηθήσουν να.

## <a name="create-a-support-ticket"></a>Δημιουργήστε ένα δελτίο υποστήριξης

1. Ανοίξτε την [πύλη του Azure][].

2. Στην αρχική οθόνη, κάντε κλικ στο πλακίδιο **Βοήθεια + υποστήριξης** .

    ![Βοήθεια + υποστήριξης](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)

3. Στη Βοήθεια + blade υποστήριξης, κάντε κλικ στην επιλογή **Δημιουργία αίτηση υποστήριξης**.

    ![Νέα αίτηση υποστήριξης](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
    
    <a name="request-quota-change"></a> 

4. Επιλέξτε την **πρόσκληση σε τύπο**.

    ![Τύπος αίτησης](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
    
    >[AZURE.NOTE]  Από προεπιλογή, κάθε SQL server (π.χ., myserver.database.windows.net) έχει **Όριο DTU** 45,000. Αυτό το όριο είναι απλώς ένα όριο ασφάλεια. Μπορείτε να αυξήσετε το όριο, δημιουργώντας μια δελτίων υποστήριξης και επιλογή *ορίου* ως τον τύπο αίτησης. Για να υπολογίσετε το DTU ανάγκες, πρέπει να πολλαπλασιάσετε το 7.5 από το σύνολο [DWU][] χρειάζεται. Για παράδειγμα, θα θέλατε να φιλοξενήσετε δύο DW6000s σε ένα διακομιστή SQL, στη συνέχεια, θα πρέπει να ζητήσετε ένα όριο DTU των 90,000.  Μπορείτε να προβάλετε την τρέχουσα κατανάλωση DTU από το blade SQL server στην πύλη. Καταμέτρηση τόσο παύσης και κατάργησης παύσης βάσεις δεδομένων προς το όριο DTU. 

5. Επιλέξτε τη **συνδρομή** που φιλοξενεί τη βάση δεδομένων με το πρόβλημα που αναφέρετε.

    ![Συνδρομή](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)

6. Επιλέξτε **αποθήκη δεδομένων SQL** του πόρου.

    ![Πόρων](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)

7. Επιλέξτε το [Azure υποστηρίζουν πρόγραμμα][].

    - Υποστήριξη **χρέωσης, Διαχείριση ορίων μεγέθους και συνδρομή** είναι διαθέσιμη σε όλα τα επίπεδα υποστήριξης.
    - **Υπηρεσίες** επιδιόρθωσης παρέχεται μέσω [προγραμματιστή][], [τυπικό][], [Απευθείας Professional][] ή [Premier][] support. Αλλαγή επιδιόρθωση ζητημάτων είναι προβλήματα που αντιμετωπίζουν οι πελάτες κατά τη χρήση του Azure όπου υπάρχει ένα λογικό προσδοκίες ότι Microsoft προκάλεσε το πρόβλημα.
    - **Εποπτεία προγραμματιστής** και **συμβουλευτικές υπηρεσίες** είναι διαθέσιμες στα επίπεδα υποστήριξης [Απευθείας Professional][] και [Premier][] . 
    
    Εάν έχετε μια υπηρεσία Premier υποστήριξη προγράμματος, μπορείτε επίσης να αναφέρετε αποθήκη δεδομένων του SQL που σχετίζονται με θέματα στην [ηλεκτρονική πύλη Microsoft Premier][].  Ανατρέξτε στο θέμα [Azure υποστηρίζουν προγράμματα][Azure υποστήριξη προγράμματος] για να μάθετε περισσότερα σχετικά με τα διάφορα προγράμματα υποστήριξης, όπως εύρος, χρόνους απόκρισης, Τιμολόγηση, κ.λπ.  Για συνήθεις ερωτήσεις σχετικά με το Azure υποστήριξης, ανατρέξτε στο θέμα [Azure υποστήριξης συνήθεις ερωτήσεις][].  

    ![Σχεδιασμός υποστήριξης](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)

8. Επιλέξτε τον **τύπο προβλήματος** και την **κατηγορία**.

    ![Κατηγορία πρόβλημα τύπου](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)

9. Περιγραφή του προβλήματος και επιλέξτε το επίπεδο επίδραση επιχειρήσεις.

    ![Περιγραφή προβλήματος](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)

10. Τις **πληροφορίες επικοινωνίας** για αυτό δελτίο υποστήριξης για θα προ-γέμισμα. Ενημέρωση αυτό, εάν είναι απαραίτητο.

    ![Τα στοιχεία επικοινωνίας](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)

11. Κάντε κλικ στην επιλογή **Δημιουργία** για να υποβάλετε την αίτηση υποστήριξης.


## <a name="monitor-a-support-ticket"></a>Παρακολούθηση μιας δελτίων υποστήριξης

Αφού έχετε υποβάλει την αίτηση υποστήριξης, την ομάδα υποστήριξης του Azure θα επικοινωνήσει μαζί σας. Για να ελέγξετε την πρόσκληση σε κατάσταση και λεπτομέρειες, κάντε κλικ στην επιλογή **Διαχείριση αιτήσεις υποστήριξης** στον πίνακα εργαλείων.

![Έλεγχος κατάστασης](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Άλλοι πόροι

Επιπλέον, μπορείτε να συνδεθείτε με την Κοινότητα αποθήκη δεδομένων του SQL σε [Υπερχείλιση στοίβας][] ή στο [φόρουμ MSDN αποθήκη δεδομένων SQL Azure][].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Πύλη του Azure]: https://portal.azure.com/
[Σχέδιο Azure υποστήριξης]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Για προγραμματιστές]: https://azure.microsoft.com/support/plans/developer/  
[Τυπική]: https://azure.microsoft.com/support/plans/standard/  
[Επαγγελματική άμεσους]: https://azure.microsoft.com/support/plans/prodirect/  
[Υπηρεσία Premier]: https://azure.microsoft.com/support/plans/premier/  
[Συνήθεις ερωτήσεις Azure υποστήριξης]: https://azure.microsoft.com/support/faq/
[Ηλεκτρονική πύλη Microsoft Premier]: https://premier.microsoft.com/
[Υπερχείλιση στοίβας]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Φόρουμ του MSDN αποθήκη δεδομένων SQL Azure]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

