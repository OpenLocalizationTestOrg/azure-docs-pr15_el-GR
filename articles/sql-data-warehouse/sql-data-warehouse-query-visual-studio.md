<properties
   pageTitle="Ερώτημα Azure SQL αποθήκη δεδομένων (Visual Studio) | Microsoft Azure"
   description="Ερώτημα αποθήκη δεδομένων του SQL με το Visual Studio."
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
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Αποθήκη δεδομένων του ερωτήματος Azure SQL (Visual Studio)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure μηχανικής εκμάθησης](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Χρησιμοποιήστε το Visual Studio ερώτημα αποθήκη δεδομένων του SQL Azure σε λίγα λεπτά. Αυτή η μέθοδος χρησιμοποιεί την επέκταση Εργαλεία δεδομένων SQL Server (SSDT) στο Visual Studio. 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να χρησιμοποιήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει:

+ Μια υπάρχουσα αποθήκη δεδομένων του SQL. Για να δημιουργήσετε μια λίστα, ανατρέξτε στο θέμα [Δημιουργία μιας αποθήκη δεδομένων του SQL][].
+ SSDT για το Visual Studio. Εάν έχετε εγκαταστήσει το Visual Studio, που πιθανότατα έχετε ήδη αυτό. Για οδηγίες εγκατάστασης και επιλογές, ανατρέξτε στο θέμα [την εγκατάσταση του Visual Studio και SSDT][].
+ Το πλήρως προσδιορισμένο όνομα διακομιστή SQL. Για να βρείτε αυτό, ανατρέξτε στο θέμα [σύνδεση σε αποθήκη δεδομένων του SQL][].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. σύνδεση με την αποθήκη δεδομένων του SQL

1. Ανοίξτε το Visual Studio 2013 ή 2015.
2. Ανοίξτε την Εξερεύνηση αντικειμένου SQL Server. Για να το κάνετε αυτό, επιλέξτε **Προβολή** > **Εξερεύνηση αντικειμένου SQL Server**.

    ![Εξερεύνηση αντικειμένου SQL Server][1]

3. Κάντε κλικ στο εικονίδιο **Προσθήκη SQL Server** .

    ![Προσθήκη SQL Server][2]

4. Συμπληρώστε τα πεδία στην σύνδεση στα παράθυρο διακομιστή.

    ![Σύνδεση με διακομιστή][3]

    - **Το όνομα του διακομιστή**. Πληκτρολογήστε το **όνομα του διακομιστή** προηγουμένως προσδιόρισαν.
    - **Έλεγχος ταυτότητας**. Επιλέξτε **τον έλεγχο ταυτότητας του SQL Server** ή την **υπηρεσία καταλόγου Active Directory ενσωματωμένο έλεγχο ταυτότητας**.
    - **Όνομα χρήστη** και **τον κωδικό πρόσβασης**. Εισαγάγετε όνομα χρήστη και τον κωδικό πρόσβασης, εάν έχει επιλεγεί η έλεγχος ταυτότητας του SQL Server παραπάνω.
    - Κάντε κλικ στην επιλογή **σύνδεση**.

5. Για να εξερευνήσετε, αναπτύξτε το Azure SQL server. Μπορείτε να προβάλετε τις βάσεις δεδομένων που σχετίζονται με το διακομιστή. Ανάπτυξη AdventureWorksDW για να δείτε τους πίνακες στο το δείγμα βάσης δεδομένων.

    ![Εξερευνήστε AdventureWorksDW][4]

## <a name="2-run-a-sample-query"></a>2. εκτέλεση ενός ερωτήματος δείγματος

Τώρα που έχει δημιουργηθεί μια σύνδεση με τη βάση δεδομένων, ας συντάξετε ένα ερώτημα.

1. Κάντε δεξί κλικ τη βάση δεδομένων στην Εξερεύνηση αντικειμένου SQL Server.

2. Επιλέξτε **νέο ερώτημα**. Ανοίγει ένα νέο παράθυρο του ερωτήματος.

    ![Δημιουργία ερωτήματος][5]

3. Αντιγράψτε αυτό το ερώτημα TSQL παράθυρο ερωτήματος:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Εκτελέστε το ερώτημα. Για να κάνετε αυτό, κάντε κλικ στο πράσινο βέλος ή χρησιμοποιήστε την παρακάτω συντόμευση: `CTRL` + `SHIFT` + `E`.

    ![Εκτέλεση ερωτήματος][6]

5. Εξετάστε τα αποτελέσματα του ερωτήματος. Σε αυτό το παράδειγμα, ο πίνακας FactInternetSales έχει 60398 γραμμών.

    ![Αποτελέσματα ερωτήματος][7]

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μπορείτε να συνδεθείτε και να υποβάλετε ερώτημα, δοκιμάστε να [απεικόνιση των δεδομένων με PowerBI][].

Για να ρυθμίσετε το περιβάλλον σας για τον έλεγχο ταυτότητας Azure Active Directory, ανατρέξτε στο θέμα [Έλεγχος ταυτότητας για την αποθήκη δεδομένων του SQL][].

<!--Arcticles-->
[Σύνδεση με αποθήκη δεδομένων του SQL]: sql-data-warehouse-connect-overview.md
[Δημιουργήστε μια αποθήκη δεδομένων SQL]: sql-data-warehouse-get-started-provision.md
[Κατά την εγκατάσταση του Visual Studio και SSDT]: sql-data-warehouse-install-visual-studio.md
[Ο έλεγχος ταυτότητας αποθήκη δεδομένων του SQL]: sql-data-warehouse-authentication.md
[απεικόνιση των δεδομένων με PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
