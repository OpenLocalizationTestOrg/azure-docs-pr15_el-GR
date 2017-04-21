
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## <a name="create-a-new-azure-sql-database"></a>Δημιουργήστε μια νέα βάση δεδομένων Azure SQL

Χρησιμοποιήστε τα ακόλουθα βήματα στην πύλη του Azure για να δημιουργήσετε μια νέα βάση δεδομένων Azure SQL σε μια νέα ή υπάρχουσα βάση δεδομένων SQL Azure λογική διακομιστή.

1. Εάν τη δεδομένη στιγμή δεν είστε συνδεδεμένοι, συνδεθείτε στην [πύλη του Azure](http://portal.azure.com).
2. Κάντε κλικ στην επιλογή **Δημιουργία**, πληκτρολογήστε **Βάση δεδομένων SQL**και, στη συνέχεια, κάντε κλικ στην επιλογή **Βάση δεδομένων SQL (νέα βάση δεδομένων)**.

     ![Δημιουργία βάσης δεδομένων](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)

3. Κάντε κλικ στην επιλογή **Βάση δεδομένων SQL (νέα βάση δεδομένων)**.

     ![Δημιουργία βάσης δεδομένων](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)

4. Κάντε κλικ στην επιλογή **Δημιουργία** για να δημιουργήσετε μια νέα βάση δεδομένων στην υπηρεσία βάσης δεδομένων SQL.

     ![Δημιουργία βάσης δεδομένων](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)

5. Παρέχουν τις τιμές για τις ακόλουθες ιδιότητες διακομιστή:

 - Όνομα βάσης δεδομένων
 - Συνδρομή: Αυτό ισχύει μόνο εάν έχετε πολλές συνδρομές.
 - Ομάδα πόρων: Εάν μόλις ξεκινάτε, χρησιμοποιήστε την ομάδα των πόρων του διακομιστή λογική.
 - Επιλέξτε προέλευση: μπορείτε να επιλέξετε μια κενή βάση δεδομένων, το δείγμα δεδομένων ή ένα αντίγραφο ασφαλείας Azure βάσης δεδομένων. Για να μετεγκαταστήσετε μια βάση δεδομένων SQL Server εσωτερικής εγκατάστασης ή φόρτωση δεδομένων χρησιμοποιώντας το εργαλείο γραμμής εντολών BCP, ανατρέξτε στις συνδέσεις στο τέλος αυτού του άρθρου.
 - Διακομιστής: Ένα νέο ή υπάρχον λογική διακομιστή.
 - Διακομιστής διαχείρισης σύνδεσης
 - Κωδικός πρόσβασης
 - Πληροφορίες για την τιμολόγηση επίπεδο: Εάν μόλις ξεκινάτε, χρησιμοποιήστε την προεπιλεγμένη τιμή S0.
 - Συρραφή: Αυτό ισχύει μόνο εάν έχει επιλεγεί μια κενή βάση δεδομένων.

        ![New database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)

6.  Κάντε κλικ στην επιλογή **Δημιουργία**. Στην περιοχή ειδοποιήσεων, μπορείτε να δείτε ότι έχει ξεκινήσει ανάπτυξης.

     ![Δημιουργία βάσης δεδομένων](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)

7. Περιμένετε έως ότου ανάπτυξης για να ολοκληρώσετε πριν να συνεχίσετε με το επόμενο βήμα.

     ![Δημιουργία βάσης δεδομένων](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)
