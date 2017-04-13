<properties
   pageTitle="Απεικόνιση δεδομένων αποθήκη δεδομένων του SQL με το Power BI Windows Azure"
   description="Απεικόνιση δεδομένων αποθήκη δεδομένων του SQL με το Power BI"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# <a name="visualize-data-with-power-bi"></a>Απεικόνιση δεδομένων με το Power BI

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure μηχανικής εκμάθησης](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε το Power BI για να συνδεθείτε σε αποθήκη δεδομένων του SQL και δημιουργία απεικονίσεων μερικές βασικές.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να περιηγηθείτε στις αυτό το πρόγραμμα εκμάθησης, πρέπει:

- Μια αποθήκη δεδομένων του SQL φορτώνονται εκ των προτέρων με τη βάση δεδομένων AdventureWorksDW. Για να προμηθεύσουν αυτό, ανατρέξτε στο θέμα [Δημιουργία μιας αποθήκη δεδομένων του SQL][] και επιλέξτε για να φορτώσετε το δείγμα δεδομένων. Εάν έχετε ήδη μια αποθήκη δεδομένων αλλά δεν έχετε το δείγμα δεδομένων, μπορείτε να [φορτώσετε το δείγμα δεδομένων με μη αυτόματο τρόπο][].


## <a name="1-connect-to-your-database"></a>1. συνδεθείτε με τη βάση δεδομένων

Για να ανοίξετε το Power BI και συνδεθείτε με τη βάση δεδομένων AdventureWorksDW:

1. Πραγματοποιήστε είσοδο στην [πύλη του Azure][].
2. Κάντε κλικ στην επιλογή **βάσεις δεδομένων SQL** και επιλέξτε τη βάση δεδομένων AdventureWorks αποθήκη δεδομένων SQL.

    ![Βρείτε τη βάση δεδομένων][1]

3. Κάντε κλικ στο κουμπί 'Άνοιγμα στο Power BI'.

    ![Κουμπί Power BI][2]

4. Τώρα θα πρέπει να βλέπετε αποθήκη δεδομένων του SQL σελίδα σύνδεσης εμφανίζει τη διεύθυνση web της βάσης δεδομένων. Κάντε κλικ στο κουμπί Επόμενο.

    ![Σύνδεση του Power BI][3]

6. Πληκτρολογήστε το όνομα χρήστη του Azure SQL server και τον κωδικό πρόσβασης και θα πλήρως συνδεθεί στη βάση δεδομένων σας αποθήκη δεδομένων του SQL.

    ![Power BI είσοδος][4]

7. Όταν έχετε πραγματοποιήσει είσοδο στο Power BI, κάντε κλικ στο κουμπί του συνόλου δεδομένων AdventureWorksDW σε το αριστερό blade. Αυτό θα ανοίξει η βάση δεδομένων.

    ![Άνοιγμα AdventureWorksDW του Power BI][5]



## <a name="2-create-a-report"></a>2. Δημιουργία αναφοράς

Τώρα είστε έτοιμοι να χρησιμοποιήσετε το Power BI για την ανάλυση δεδομένων AdventureWorksDW δείγμα. Για να εκτελέσετε την ανάλυση, AdventureWorksDW έχει μια προβολή που ονομάζεται AggregateSales. Αυτή η προβολή περιλαμβάνει μερικές από τις βασικές μετρήσεις για την ανάλυση τις πωλήσεις της εταιρείας.

1. Για να δημιουργήσετε ένα χάρτη του ποσού των πωλήσεων σύμφωνα με ταχυδρομικό κώδικα, στο παράθυρο πεδία στα δεξιά, κάντε κλικ στην προβολή AggregateSales για να το αναπτύξετε. Κάντε κλικ στις στήλες ΤαχυδρομικόςΚώδικας και Ποσό_πωλήσεων για να τις επιλέξετε.

    ![Power BI, επιλέξτε AggregateSales][6]

    Power BI αναγνωρίζει αυτόματα αυτή είναι γεωγραφικά δεδομένα και να το τοποθετήσετε σε ένα χάρτη για εσάς.

    ![Χάρτης του Power BI][7]

2. Αυτό το βήμα δημιουργεί ένα γράφημα ράβδων που εμφανίζει ποσό πωλήσεων ανά εισοδήματος πελατών. Για να δημιουργήσετε αυτό, μεταβείτε στην αναπτυγμένη προβολή AggregateSales. Κάντε κλικ στο πεδίο SalesAmount. Σύρετε το πεδίο εισοδήματος πελατών προς τα αριστερά και αποθέστε την σε άξονα.

    ![Επιλέξτε άξονα του Power BI][8]

    Θα σας να μετακινηθεί το γράφημα ράβδων επάνω από τα αριστερά.

    ![Power BI γραμμή][9]

3. Αυτό το βήμα δημιουργεί ένα γράφημα γραμμών που εμφανίζει ποσό πωλήσεων ανά ημερομηνία παραγγελίας. Για να δημιουργήσετε αυτό, μεταβείτε στην αναπτυγμένη προβολή AggregateSales. Κάντε κλικ στην επιλογή SalesAmount και OrderDate. Το απεικονίσεις στήλης, κάντε κλικ στο εικονίδιο γραφήματος γραμμών; Αυτό είναι το πρώτο εικονίδιο στη δεύτερη γραμμή στην περιοχή απεικονίσεων.

    ![Γράφημα γραμμών επιλέξτε Power BI][10]

    Τώρα έχετε μια έκθεση που εμφανίζει τρεις διαφορετικές απεικονίσεις των δεδομένων.

    ![Power BI γραμμής][11]

Μπορείτε να αποθηκεύσετε την πρόοδό σας οποιαδήποτε στιγμή, κάνοντας κλικ στην επιλογή **αρχείο** και επιλέγοντας **Αποθήκευση**.

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που θα σας έχετε δώσει λίγο χρόνο για να θερμή με το δείγμα δεδομένων, ανατρέξτε στο θέμα πώς μπορείτε να [αναπτύξετε][], [Φόρτωση][]ή [μετεγκατάσταση][]. Ή, Ρίξτε μια ματιά στην τοποθεσία του [Power BI τοποθεσίας Web][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[μετεγκατάσταση]: sql-data-warehouse-overview-migrate.md
[ανάπτυξη]: sql-data-warehouse-overview-develop.md
[φόρτωση]: sql-data-warehouse-overview-load.md
[φόρτωση του δείγματος δεδομένων με μη αυτόματο τρόπο]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Δημιουργήστε μια αποθήκη δεδομένων SQL]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Πύλη του Azure]: https://portal.azure.com/
[Τοποθεσία Web του Power BI]: http://www.powerbi.com/
